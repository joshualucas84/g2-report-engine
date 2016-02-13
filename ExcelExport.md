# Introduction #

Excel Export functionality is optional for each report, and must be explicitly configured when creating a `ReportDefinition`. This page will discuss our approach to Excel extracts and how to include them in your reports.

# Approach #

Many report engines provide Excel export capabilities. However, in many cases they produce an Excel export that looks **exactly** like the HTML version of the report... including page breaks, formatting, charts and more. This can be problematic when a user wants to slice and dice, filter, sort or pivot the data.

Therefore we provide very basic Excel export functionality. No formatting, no line breaks, no grouping, etc. Our Excel exports can only contain on or more sheets of tabular data. Each sheet can only display a single data table.

# Instructions #

Each report has a special format defined for Excel exports. So we start by getting the Excel document property from the `ReportDefinition`.

  * First, we add a `Worksheet` to the Excel document. The method takes
    * the worksheets's name
    * the worksheet's left to right order
    * Note: we can create 1 or many worksheets per Excel document
  * Second we add a Table to the worksheet
    * `addTable` method asks for the data query
    * this allows us to bind the table a query result set
  * Third we add our header rows to the table
    * We use the `addHeaderRowCell` method
    * We provide the cell's text and left-to-right order
  * Last we add the body rows to the table
    * We use the `addBodyRowCell` method
    * we provide the `DataColumn` to which the cell is bound
    * we provide the cell's left-to-right order
**```
report.addExcelDocument()
                /* add worksheet */
                .addWorksheet("page1", 0)
                /* add table to worksheet */
                .addTable(query1)
                /* add header cells (labels) to table */
                .addHeaderRowCell(new Cell("Name", "", 0))
                .addHeaderRowCell(new Cell("Description", "", 1))
                .addHeaderRowCell(new Cell("Price", "", 2))
                /* add body cells (data bound) to table */
                .addBodyRowCell(new Cell(col1, "", 0))
                .addBodyRowCell(new Cell(col2, "", 1))
                .addBodyRowCell(new Cell(col2, "", 2));
```**

Notice that it is pretty easy to add Excel export capability. The API provides a number of helper method that make it easy to chain everything together. Hopefully we can achieve this same level of simplicity for construction of HTML reports...