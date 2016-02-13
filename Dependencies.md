| **Library** | **Required (Y/N)** | **Description** |
|:------------|:-------------------|:----------------|
| xstream-1.3.jar | Yes                | Used to serialize and de-serialze report definitions to xml format |
| xpp3\_min-1.1.4c.jar |  Yes               | Used by XStream to improve XML parsing |
| jxl-2.6.jar |  Yes               | Used to generate a report in Excel format |
| servlet-api-2.5.jar |  No                | Used by the embedded servlet, providing web-based reporting.<br />Only required when using embedded servlet |
| velocity-1.5.jar |  No                | Used by the embedded servlet, providing web-based reporting.<br />Only required when using embedded servlet |
| velocity-tools-generic-1.4.jar |  No                | Used by to format dates in the Velocity templates |
| commons-collections-3.1.jar | No                 | Only required if using Velocity / embedded servlet |
| commons-lang-2.1.jar | No                 | Only required if using Velocity / embedded servlet |
| oro-2.0.8.jar | No                 | Only required if using Velocity / embedded servlet |

The report engine also requires on **Java 1.6**. This is primarily because we use the new `ScriptManager` to allow for custom scripting inside your report. Using Java 1.6 and `ScriptManager` provide native scripting with javascript as well as the ability to integrate other scripting languages such as groovy, jython, and more.

For your convenience the dependencies have been zipped and provided for download on the [Downloads page](http://code.google.com/p/g2-report-engine/downloads/list)