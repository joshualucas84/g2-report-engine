This is a very quick introduction to the g2 Report Engine. Skim and read to get an idea of how to build and execute a simple report.

_Note: the g2 Report Engine does not currently include a GUI Report Builder, so everything must be built via Java code..._

#### Create A Report ####

The first step to building the report is to create the `ReportDefinition` and give your report a name, description, etc:
```
ReportDefinition report = new ReportDefinition();
report.setName("my report");
report.setDescription("created for tutorial");
```

#### Declare Report Connections ####

Now we need to add a data source to our report. A project can have one or more data sources at its disposal. Notice that we create a JDBC data source (connection), set all required properties, and then finally add it to the report.
```
JdbcConnection conn1 = new JdbcConnection();
conn1.setDatabaseUser("sa");
conn1.setDatabasePassword("");
conn1.setDriverClass("org.h2.Driver");
conn1.setDatabaseUrl("jdbc:h2:mem:test-db;DB_CLOSE_DELAY=-1");

report.getDataConnections().add(conn1);
```

#### Define Data Queries ####

Now we need to create a data query (obviously, because with no data there is no report). A report can have one or more queries. So first, we will create a `JdbcQuery` object and provide it with the previously created connection and a sql statement.
```
JdbcQuery query1 = new JdbcQuery();
query1.setDataConnection(conn1);
query1.setSqlQuery("select * from item");
```

Next, we need to declare each column that the dataset will return and add to the query's list of columns. Once complete, add the query to the report's list of queries.
```
DataColumn col1 = new DataColumn("Name", 0, DataType.STRING);
DataColumn col2 = new DataColumn("Description", 1, DataType.STRING);
DataColumn col3 = new DataColumn("Price", 2, DataType.DOUBLE);

query1.getColumns().add(col1);
query1.getColumns().add(col2);
query1.getColumns().add(col3);

report.getDataQueries().add(query1);
```

#### Design HTML Output Format ####

Once we have declared the query and connection we are ready to design the report. First, we can start by adding a data table. This is basically an HTML table that will display data in a tabular format. We create a data table object and set the query to which it should be bound:
```
DataTable table = new DataTable();
table.setDataQuery(query1);
```

Now, we need to add header rows to the table. Create a `GridRow` object and add the header cells (1 for each column returned by the query). Once the header row is built, add it to the table's list of header rows.
```
GridRow tableHeader = new GridRow();
tableHeader.addCell(GridCell(new RawHTML("Name")));
tableHeader.addCell(GridCell(new RawHTML("Description")));
tableHeader.addCell(GridCell(new RawHTML("Price")));
table.getHeaderRows().add(tableHeader);
```

Next we add rows to the body of the table. These rows will be bound to data elements from our query. So first we create the `GridRow` and add its cells. Each cell contains a `DataElement`, which allows us to bind the cell to a column in the query. Note that the `DataElement` expects a `DataColumn` in the constructor. We provide the same data columns we created in the earlier steps:
```
GridRow tableBody = new GridRow();
tableBody.addCell(GridCell(new DataElement(col1, 0)));
tableBody.addCell(GridCell(new DataElement(col2, 1)));
tableBody.addCell(GridCell(new DataElement(col3, 2)));
table.getBodyRows().add(tableBody);
```

Now that we have built the data table, we can add it to the report:
```
report.getWebPage().addChildElement(table);
```

#### Build and Run ####

Finally, we can "serialize" the report to XML and save it to the File System:
```
ReportSerializationUtil.toXMLFile(new File("myReport.rxml"),report);
```

We can now process the report definition and create an HTML formatted report:
```
HTMLReportBuilder.build(new File("myReport.rxml"),new HashMap(),new File("myReport.html"));
```