# Create A Report #

The first step to building the report is to create the `ReportDefinition` and give your report a name, description, etc:
```
ReportDefinition report = new ReportDefinition();
report.setName("My First Report");
```

Now we will create our first component. There are many components that can be displayed on the page, including images, charts, maps and data tables. In this case we just want to create some static text, so we will use the Label component. We provide the constructor with the text that we want the label to display.
```
Label helloLabel = new Label ("Hello World!");
```

Now we will add the component to report. We will add it to the `WebPage` object, which is used to define all components, styles and scripts for rendering a report in HTML format.
```
report.getWebPage().addChildElement(helloLabel);
```

Finally, we can generate the report by calling the `HTMLReportBuilder.build` method. We provide the build method with 1) the `ReportDefinition` object 2) an empty map since there are no input parameters defined 3) the `File` the output location where the report should be generated on the file system.
```
HTMLReportBuilder.build(report, new HashMap, new File("HelloWorld.html"));
```

You can save this  to XML for later use. Please see the ["Save to XML"](XMLSerialization.md) section of this tutorial for complete instructions.