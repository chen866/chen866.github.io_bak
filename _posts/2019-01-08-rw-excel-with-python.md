---
layout: article
title: Python读写Excel
tags:
  - python
---

<!--more-->

Excel还是平时经常用得到的数据存储格式，这是平时使用python读取写入Excel文件的代码：

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

# 读写2007 excel
import openpyxl
# 读写2003 excel
import xlrd
import xlwt

"""
excel文件和单个二维数组之间的相互转换
"""


def read03Excel(path, sheet_index=0):
    workbook = xlrd.open_workbook(path)
    sheets = workbook.sheet_names()
    worksheet = workbook.sheet_by_name(sheets[sheet_index])
    rt_list = []
    for i in range(worksheet.nrows):
        rt_list.append([])
        for j in range(worksheet.ncols):
            rt_list[-1].append(worksheet.cell_value(i, j))
    return rt_list


def write03Excel(path, out_value):
    wb = xlwt.Workbook()
    sheet = wb.add_sheet("Sheet")
    for i, r in enumerate(out_value):
        for j, c in enumerate(r):
            if c:
                sheet.write(i, j, c)
    wb.save(path)
    print("%s -> %s" % (path, len(out_value)))


def base_read_cell(cell):
    """默认读取单元格方法 单元格的样式、超链接、标注等等都在cell里面有存储"""
    return cell.value


def read07Excel(path, sheet_index=0, cell_handle=None):
    # 默认使用默认读取方法
    if cell_handle is None:
        cell_handle = base_read_cell
    rt_list = []
    wb = openpyxl.load_workbook(path)
    sheet = wb.worksheets[int(sheet_index)]
    for row in sheet.rows:
        rt_list.append([])
        for cell in row:
            rt_list[-1].append(cell_handle(cell))
    return rt_list


def write07Excel(path, out_value, sheet_name='Sheet1'):
    wb = openpyxl.Workbook()
    sheet = wb.active
    sheet.title = sheet_name
    for i, r in enumerate(out_value):
        for j, c in enumerate(r):
            if c:
                sheet.cell(row=i + 1, column=j + 1).value = c
    wb.save(path)
    print("%s -> %s" % (path, len(out_value)))

```