# TSG:: Spark jars using Pyspark

## Content
[Problem](#problem)

[Background](#background)

[Troubleshooting](#troubleshooting)

[Solution](#solution)

[Reference](#reference)


## Problem <a name="problem"></a>
Customer use 3rd party jars for scala spark workflows but they also use them for pyspark.  In this case customer is using databricks spark-xml jar from Maven repository.

Maven repro: https://mvnrepository.com/artifact/com.databricks/spark-xml_2.12/0.16.0

When customer's use pyspark with a jar like above even after loading it in the spark environment library for the workspace.

<img src="https://cssexamplesallee.blob.core.windows.net/tsgs/Fabric/Fabric_notebook_images/uploadLibrary.png" whidth="1000" height="500"/>

Customers run into this issue for pyspark using the jars:

```
xml_df = spark.read\
    .format("com.databricks.spark.xml")\
    .option("rootTag", "catalog")\
    .option("rowTag", "book")\
    .load("Files/books.xml")
```
Error message:
> Py4JJavaError: An error occurred while calling o4768.load.
: java.lang.ClassNotFoundException: 
Failed to find data source: com.databricks.spark.xml. Please find packages at
https://spark.apache.org/third-party-projects.html

### Background <a name="background"></a>
Customers can use these jars fine in Databricks or Spark on Synpase.  The issue here is that there needs to be a wrapper to help pyspark understand and use jars.  Since jars is a Scala and Java concept and uses the JVM, python does not understand this or have a way to use the JVM out of the box.

### Troubleshooting <a name="troubleshooting"></a>

The best way to troubleshoot this issue is to gather the output message below the notebook and/or gather the spark STDERR logs.

***Pyspark output in the Notebook:***

<img src="https://cssexamplesallee.blob.core.windows.net/tsgs/Fabric/Fabric_notebook_images/pysparkErrorOutput.png" width="1000" hight="100">

***Get STDOUT from Spark UI***

<img src="https://cssexamplesallee.blob.core.windows.net/tsgs/Fabric/Fabric_notebook_images/getSparkSTDERR.png" width="1000" hight="100">

***Then get the STDOUT logs***

<img src="https://cssexamplesallee.blob.core.windows.net/tsgs/Fabric/Fabric_notebook_images/getSTDOUTlogs.png" width="1000" hight="100">

From the above information you can gather the error.

### Solution <a name="solution"></a>

The first workaround here is to simply switch from pyspark in the cell to Scala.
<img src="https://cssexamplesallee.blob.core.windows.net/tsgs/Fabric/Fabric_notebook_images/useScala.png" width="1000" hight="100">

The second solution is to use magic configure with pyspark.
<img src="https://cssexamplesallee.blob.core.windows.net/tsgs/Fabric/Fabric_notebook_images/magicConfigure.png" width="1000" hight="100">


### Reference <a name="reference"></a>

WorkItem: https://msdata.visualstudio.com/A365/_workitems/edit/2500517