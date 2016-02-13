# Introduction #

Basic JDO query support has been added to allow G2 Report Engine integration in the Google App Engine environment. After some basic tests both locally and deployed everything seems to be working, including Excel Export capability.

## JDO and POJO ##

First thing is first, we need to define our Entities that are to be stored in the App Engine. For this example we create a simple Person entity with a last and first name.

```
@PersistenceCapable(identityType = IdentityType.APPLICATION)
public class Person {
    @PrimaryKey
    @Persistent(valueStrategy = IdGeneratorStrategy.IDENTITY)
    private Long id; 

    @Persistent
    private String firstName;

    @Persistent
    private String lastName;
...
```

## Creating the Report ##

We need to add an `AppEngineConnection` and `AppEngineQuery` to the report. These are the App Engine equivalent to the `JdbcConnection` and `JdbcQuery` from the previous pages.

You will notice that, for now, we have to pass in the instance of the `PersistenceManagerFactory`. _Note: Fix is applied in Trunk, you now pass the `PersistenceManagerFactory` to `Settings.setPersistenceManagerFactory`, which holds all global variables. `PersistenceManagerFactory` therefore only needs to be set once._

```
AppEngineConnection conn = new AppEngineConnection();
conn.setPersistenceManagerFactory(PMF.get());
report.getDataConnections().add(conn);
```

When creating the query, we link it to the Connection. The query is provided a JDO query string. Note: the query string must contain attribute names in the SELECT clause, which will force JDO to return a `List` of `Object[]`, as opposed to a list of `Person` objects.

```
AppEngineQuery query = new AppEngineQuery();
query.setConnection(conn);
query.setName("Person query");
query.setSqlQuery("select lastName, firstName from brad.rydzewski.Person");
report.getDataQueries().add(query);
```

Last step is to define the data columns and add them to the query definition:

```
DataColumn col1 = new DataColumn("LastName", 0, DataType.STRING);
DataColumn col2 = new DataColumn("FirstName", 1, DataType.STRING);
query.getColumns().add(col1);
query.getColumns().add(col2);
```

## Displaying Data ##

Now you can start to build the visual components to your report (tables, charts, etc). Refer to the _Design Data Table_ section from the [QueryingData](QueryingData.md) tutorial, which demonstrates adding a data-driven HTML table to a report.

All data driven components (tables, charts, etc) require you to provide it with a `DataQuery` for data binding. Both `AppEngineQuery` and `JdbcQuery` extend the `DataQuery` class.

## Limitations ##

Due to classes excluded from App Engine's white-list, the following functionality will not work under Google App Engine:
  1. Cannot save the report to a file on the File System
  1. Cannot read a `ReportDefinition` from an XML file on the File System
  1. Cannot use XStream to convert an xml file to a `ReportDefinition` object 
  1. Cannot use JDBC queries (obviously)

 I'm currently evaluating alternatives to XStream. I've been told Apache Betwixt will work, but I haven't had time to do adequate analysis.

## Missing Features ##

I have yet to implement and test the following features:
  1. Use of Parameters in the JDO queries. _Update: Feature added in Trunk_