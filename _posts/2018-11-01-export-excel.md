---
layout: article
title: C#导出Excel
tags:
  - C#
lang: zh-Hans
---

<!--more-->

使用第三方库NPIO；  
NPOI，开源、免费的强大Excel组件。

```csharp
/// <summary>
/// DataTable导出Excel
/// </summary>
/// <param name="cell"></param>
/// <returns></returns>
public void ExportExcel(DataTable dt)
{
    try
    {
        //创建一个工作簿
        IWorkbook workbook = new HSSFWorkbook();

        //创建一个 sheet 表
        ISheet sheet = workbook.CreateSheet(dt.TableName);

        //创建一行
        IRow rowH = sheet.CreateRow(0);

        //创建一个单元格
        ICell cell = null;

        //创建单元格样式
        ICellStyle cellStyle = workbook.CreateCellStyle();

        //创建格式
        IDataFormat dataFormat = workbook.CreateDataFormat();

        //设置为文本格式，也可以为 text，即 dataFormat.GetFormat("text");
        cellStyle.DataFormat = dataFormat.GetFormat("@");

        //设置列名
        foreach (DataColumn col in dt.Columns)
        {
            //创建单元格并设置单元格内容
            rowH.CreateCell(col.Ordinal).SetCellValue(col.Caption);

            //设置单元格格式
            rowH.Cells[col.Ordinal].CellStyle = cellStyle;
        }

        //写入数据
        for (int i = 0; i < dt.Rows.Count; i++)
        {
            //跳过第一行，第一行为列名
            IRow row = sheet.CreateRow(i + 1);

            for (int j = 0; j < dt.Columns.Count; j++)
            {
                cell = row.CreateCell(j);
                cell.SetCellValue(dt.Rows[i][j].ToString());
                cell.CellStyle = cellStyle;
            }
        }

        //设置导出文件路径
        string path = HttpContext.Current.Server.MapPath("Export/");

        //设置新建文件路径及名称
        string savePath = path + DateTime.Now.ToString("yyyy-MM-dd-HH-mm-ss") + ".xls";

        //创建文件
        FileStream file = new FileStream(savePath, FileMode.CreateNew,FileAccess.Write);

        //创建一个 IO 流
        MemoryStream ms = new MemoryStream();

        //写入到流
        workbook.Write(ms);

        //转换为字节数组
        byte[] bytes = ms.ToArray();

        file.Write(bytes, 0, bytes.Length);
        file.Flush();

        //还可以调用下面的方法，把流输出到浏览器下载
        OutputClient(bytes);

        //释放资源
        bytes = null;

        ms.Close();
        ms.Dispose();

        file.Close();
        file.Dispose();

        workbook.Close();
        sheet = null;
        workbook = null;
    }
    catch(Exception ex)
    {

    }
}
```

```csharp
/// <summary>
/// 输出文件到用户浏览器
/// </summary>
/// <param name="cell"></param>
/// <returns></returns>
public void OutputClient(byte[] bytes)
{
    HttpResponse response = HttpContext.Current.Response;
    response.Buffer = true;
    response.Clear();
    response.ClearHeaders();
    response.ClearContent();
    response.ContentType = "application/vnd.ms-excel";
    response.AddHeader("Content-Disposition",
        string.Format("attachment; filename={0}.xls", DateTime.Now.ToString("yyyy-MM-dd-HH-mm-ss")));
    response.Charset = "GB2312";
    response.ContentEncoding = Encoding.GetEncoding("GB2312");
    response.BinaryWrite(bytes);
    response.Flush();
    response.Close();
}
```

附加代码：

```csharp
/// <summary>
/// 获取单元格类型
/// </summary>
/// <param name="cell"></param>
/// <returns></returns>
private static object GetValueType(ICell cell)
{
    if (cell == null)
        return null;
    switch (cell.CellType)
    {
        case CellType.Blank: //BLANK:  
            return null;
        case CellType.Boolean: //BOOLEAN:  
            return cell.BooleanCellValue;
        case CellType.Numeric: //NUMERIC:  
            return cell.NumericCellValue;
        case CellType.String: //STRING:  
            return cell.StringCellValue;
        case CellType.Error: //ERROR:  
            return cell.ErrorCellValue;
        case CellType.Formula: //FORMULA:  
        default:
            return "=" + cell.CellFormula;
    }
}
```

