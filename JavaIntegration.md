# Introduction #

Reports can be generated directly from your application, typically with only a single line of Java code. First we need a report definition, which defines a report's data sources, queries and display format.

A report definition can exist in the form of a POJO, an XML String, or as an XML File. Most methods for generating reports are overloaded an allow the developer to provide a report definition in any of these formats.

Below you will find a list of methods to generate reports in HTML and Excel format. These methods a simple facades to simplify the report generation for the developer. We attempt to cover most use cases, but you can always interact with the report engine at a lower level if these don't meet your needs.

# HTML Reports #

The `HTMLReportBuilder.build` method can be executed to generate a report in HTML format. You can generate a full HTML page, or an HTML fragment which is basically a `<div>` snippet. A Fragment is useful if you want to embed the report's html inside a page, similar to a portlet.

Option 1: provide the XML report definition file, a hash map of report parameters, and the file where the HTML report will be written to the file system.
```
static void build(File reportDefinitionXML, Map params, File reportResultsFile);
```

Option 2: provide the XML report definition file, a hash map of report parameters, and a boolean value indicating if the report should be rendered as a fragment. The method will generate the report and return it as a String.
```
static String build(File reportDefinitonXML, Map params, boolean renderAsDiv);
```

Option 3: provide the report definition object as an XML string, a hash map of report parameters, and a boolean value indicating if the report should be rendered as a fragment. The method will generate the report and return it as a String.
```
static String build(String reportDefinitionXML, Map params, boolean renderAsDiv)
```

Option 4: provide the report definition object (pojo), a hash map of report parameters, and a boolean value indicating if the report should be rendered as a fragment. The method will generate the report and return it as a String.
```
static String build(ReportDefinition reportDefinition, Map params, boolean renderAsDiv)
```

These options all serve different use cases. For example, option #1 is great for application that wants to quickly generate reports to the file system, while option #3 is great for systems that would like to store report definitions in the database, get the results as a string, and display the contents in a servlet (great for GAE/J)

# Excel Reports #

The `ExcelReportBuilder.build` method can be executed to generate a report in Excel format. Please note that a report definition must explicitly contain instructions (design) for building a report in Excel format.

Option 1: provide the XML report definition file, a hash map of report parameters, and the file where the Excel report will be written to the file system.
```
static void build(File reportDefinitionXML, Map params, File outputFile);
```

Option 2: provide the report definition object (pojo), a hash map of report parameters, and the file where the Excel report will be written to the file system.
```
static void build(ReportDefinition reportDefiniton, Map params, File outputFile);
```

Option 3: provide the report definition object as an XML string, a hash map of report parameters, and the output stream to which the Excel report should be written. It is then up to the developer to do something with the output stream, such as save it to the file system, etc.  This is great for use with Servlets, allowing the report to be written to the servlet response's output stream.
```
static void build(File reportDefinitonXML, Map params, OutputStream stream);
```

Option 4: provide the report definition object (pojo), a hash map of report parameters, and the output stream to which the Excel report should be written. The method will generate the report and return it as a String.
```
static void build(ReportDefinition reportDefiniton, Map params, OutputStream stream);
```

# Examples #

## Command Line Example ##
Here is an example of how to execute a report from a java applications `main` method to generate an HTML report. A few assumptions: 1) the report definition xml file path is provided as the first argument 2) no input parameters are being provided, hence the empty map 3) the output file path is provided as the second argument.

```
public static void main( String[] args )
{
  HTMLReportBuilder.build(args[0], new HashMap(), args[1]);
}
```

## Servlet Example ##
Here is an example of how to execute a report from inside an `HttpServet`, and write the HTML contents to its `ServletOutputStream`. A few assumptions: 1) the report file path is provided via a URL parameter called "reportPath" 2) no input parameters are being provided 3) we are rendering this as a full HTML page, not as a fragment.
```
@Override
protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
  
  /* report path provided via url param (ex. c:/HelloWorld.rxml) */
  String report = request.getParameter("reportPath");

  /* generate the report as an html string */
  String html = HTMLReportBuilder.build(new File(report), new HashMap(), false);

  /* write the report to the servlet response stream */
  response.getOutputStream.print(html);

  /* set as html content */
  response.setContentType("text/html;charset=UTF-8");
}
```