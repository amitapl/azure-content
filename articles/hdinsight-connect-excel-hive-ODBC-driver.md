<properties urlDisplayName="Connect Excel to HDInsight" pageTitle="Connect Excel to Hadoop with the Hive ODBC Driver | Azure" metaKeywords="" description="Learn how to set up and use the Microsoft Hive ODBC driver for Excel to query data in an HDInsight cluster." metaCanonical="" services="hdinsight" documentationCenter="" title="Connect Excel to Hadoop with the Microsoft Hive ODBC Driver" authors="bradsev" solutions="" manager="paulettm" editor="cgronlun" />

<tags ms.service="hdinsight" ms.workload="big-data" ms.tgt_pltfrm="na" ms.devlang="na" ms.topic="article" ms.date="11/10/2014" ms.author="bradsev" />



#Connect Excel to Hadoop by using the Microsoft Hive ODBC driver


The Microsoft big-data solution integrates Microsoft business intelligence (BI) components with Apache Hadoop clusters that have been deployed by Azure HDInsight. An example of this integration is the ability to connect Excel to the Hive data warehouse of a Hadoop cluster in HDInsight by using the Microsoft Hive ODBC Driver. 

It is also possible to connect the data associated with an HDInsight cluster and other data sources, including other (non-HDInsight) Hadoop clusters, from Excel by using the Microsoft Power Query add-in for Excel. For information on installing and using Power Query, see [Connect Excel to HDInsight with Power Query][hdinsight-power-query].

**Prerequisites**:

Before you begin this article, you must have the following:

- An HDInsight cluster. To configure one, see [Get started with Azure HDInsight][hdinsight-get-started].
- A computer that is running Windows 8, Windows 7, Windows Server 2012, or Windows Server 2008 R2.
- Office 2013 Professional Plus, Office 365 ProPlus, Excel 2013 Standalone, or Office 2010 Professional Plus.


##<a id="InstallHiveODBCDriver"></a>Install the Microsoft Hive ODBC Driver

Download and install the Microsoft Hive ODBC Driver from the [Download Center][hive-odbc-driver-download]. 

This driver can be installed on 32-bit or 64-bit versions of Windows 8, Windows 7, Windows Server 2012, and Windows Server 2008 R2 and will allow connection to Azure HDInsight (version 1.6 and later) and the HDInsight Emulator (v.1.0.0.0 and later). You should install the version that matches the version of the application where you will be using the ODBC driver. For this tutorial, the driver will be used from Excel. 

##<a id="CreateHiveODBCDataSource"></a>Create a Hive ODBC data source

The following steps show you how to create a Hive ODBC data source.

1. From Windows 8, press the Windows logo key to open the Start screen, and then type **data sources**.
2. Click **Set up ODBC Data sources (32-bit)** or **Set up ODBC Data Sources (64-bit)** depending on your Office version. If you are using Windows 7, choose **ODBC Data Sources (32 bit)** or **ODBC Data Sources (64 bit)** from **Administrative Tools**. This will launch the **ODBC Data Source Administrator** dialog. 
 
	![OBDC data source administrator][img-hdi-simbahiveodbc-datasource-admin]

3. From **User DSN**, click **Add** to open the Create New Data Source wizard. 
4. Select **Microsoft Hive ODBC Driver**, and then click **Finish**. This will launch the **Microsoft Hive ODBC Driver DSN Setup** dialog. 

5. Type or select the following values:

	<table border="1">
	<tr><td><strong>Property</strong></td><td><strong>Description</strong></td></tr>
	<tr><td>Data Source Name</td><td>Give a name to your data source.</td></tr>
	<tr><td>Host</td><td>Enter <strong><HDInsightClusterName>.azurehdinsight.net</strong>. For example, <strong>myHDICluster.azurehdinsight.net</strong>.</td></tr>
	<tr><td>Port</td><td>Use <strong>443</strong>. (This port has been changed from 563 to 443.)</td></tr>
	<tr><td>Database</td><td>Use <strong>Default</strong>.</td></tr>
	<tr><td>Hive Server Type</td><td>Select <strong>Hive Server 2</strong>.</td></tr>
	<tr><td>Mechanism</td><td>Select <strong>Azure HDInsight Service</strong>.</td></tr>
	<tr><td>HTTP Path</td><td>Leave it blank.</td></tr>
	<tr><td>User Name</td><td>Enter the HDInsight cluster user name. This is the user name created during the cluster provisioning process. If you used the Quick Create option, the default user name is <strong>admin</strong>.</td></tr>
	<tr><td>Password</td><td>Enter the HDInsight cluster user password.</td></tr>
	</table>

	There are some important parameters to be aware of when you click **Advanced Options**:

	<table border="1">
	<tr><td>Use Native Query</td><td>When it is selected, the ODBC driver will NOT try to convert Transact-SQL into HiveQL. Use it only if you are 100% sure you are submitting pure HiveQL statements. When connecting to SQL Server or Azure SQL Database, you should leave it unchecked.</td></tr>
	<tr><td>Rows fetched per block</td><td>When fetching a large amount of records, tuning this parameter may be required to ensure optimal performance.</td></tr>
	<tr><td>Default string column length, <br/>
			Binary column length,  <br/>
			Decimal column scale</td><td>The data type lengths and precisions may affect how data is returned. They will cause incorrect information to be returned due to loss of precision and/or truncation.</td></tr>
	</table>

	![Advanced options][img-HiveOdbc-DataSource-AdvancedOptions]

6. Click **Test** to test the data source. When the data source is configured correctly, it shows **TESTS COMPLETED SUCCESSFULLY!**.
7. Click **OK** to close the **Test** dialog. The new data source should now be listed in the **ODBC Data Source Administrator** dialog. 
8. Click **OK** to exit the wizard.
	
##<a id="ImportData"></a>Import data into Excel from an HDInsight cluster

The steps below describe the way to import data from a Hive table into an Excel workbook by using the ODBC data source that you created in the steps above.

1. Open a new or existing workbook in Excel.
2. From the **Data** tab, click the **Get External Data** tile, click **From Other Sources**, and then click **From Data Connection Wizard** to launch the Data Connection Wizard.

	![Open data connection wizard][img-hdi-simbahiveodbc.excel.dataconnection]

3. Select **ODBC DSN** as the data source, and then click **Next**.
4. From **ODBC data sources**, select the data source name that you created in the previous step, and then click **Next**.
5. Re-enter the password for the cluster in the wizard, and then click **Test** to verify the configuration.
6. Click **OK** to close the **Test** dialog.
7. Click **OK**. Wait for the **Select Database and Table** dialog to open. This can take a few seconds.
8. Select the table that you want to import, and then click **Next**. The sample Hive table called *hivesampletable* comes with HDInsight clusters. You can choose it if you haven't created one. For more information on running Hive queries and creating Hive tables, see [Use Hive with HDInsight][hdinsight-use-hive].
8. Click **Finish**.
9. In the **Import Data** dialog, you can change or specify the query. To do so, click **Properties**. This can take a few seconds.
10. Click on the **Definition** tab, and then append **LIMIT 200** to the Hive select statement in the **Command text** text box. The modification will limit the returned record set to 200.

	![Connection Properties][img-hdi-simbahiveodbc-excel-connectionproperties]

11. Click **OK** to close the **Connection Properties** dialog.
12. Click **OK** to close the **Import Data** dialog.  
13. Re-enter the password, and then click **OK**. It takes a few seconds before data gets imported to Excel.

##<a id="nextsteps"></a>Next steps

In this article, you learned how to use the Microsoft Hive ODBC Driver to retrieve data from the HDInsight service into Excel. Similarly, you can retrieve data from the HDInsight service into SQL Database. It is also possible to upload data into the HDInsight service. To learn more, see:

- [Analyze flight delay data using HDInsight][hdinsight-analyze-flight-data]
- [Upload data to HDInsight][hdinsight-upload-data]
- [Use Sqoop with HDInsight][hdinsight-use-sqoop]


[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-get-started]: hdinsight-get-started.md

[hive-odbc-driver-download]: http://go.microsoft.com/fwlink/?LinkID=286698

[img-hdi-simbahiveodbc-datasource-admin]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png 
[img-HiveOdbc-DataSource-AdvancedOptions]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png 
[img-hdi-simbahiveodbc-excel-connectionproperties]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveODBC.Excel.ConnectionProperties1.png 
[img-hdi-simbahiveodbc.excel.dataconnection]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png 
