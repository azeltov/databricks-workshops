
This section covers provisioning of Azure Databricks and configuration required.

# 5. Azure Databricks
From the portal navigate to the resource group you created - "Telemetry-Processor".

## 5.0.1.  Provision a storage account for Databricks 
Databricks leverages Azure object storage as its distributed file system - Databricks File System (DBFS).<br>
In this section, we will create a storage account with containers for use with Databricks.<br>
Each container will be used to model the data flow across storage/file system paritions as part of information management - "raw" for data exactly as it comes in, "curated" for cleansed, transformed, joined etc data and "consumption" for reports and such.  Each of these layers, typically, has a retention period and access control.

### 5.0.1.1.  Create storage account
Create a general purpose storage account (version 1) in the resource group - Telemetry-Processor.<br>
![SA-1](../images/CreateStorageAcct-1.png)
<br>
<br>
![SA-2](../images/CreateStorageAcct-2.png)
<br>
<br>
![SA-3](../images/CreateStorageAcct-3.png)
<br>
<br>
![SA-4](../images/CreateStorageAcct-4.png)
<br><br>

### 5.0.1.2.  Create storage account containers
Within this storage account, provision 3 containers with private (no anonymous access) configuration<br><br>
![SA-7](../images/CreateStorageAcct-7.png)
<br><br>1.  Create a container called raw<br>
![SA-8](../images/CreateStorageAcct-8.png)
<br><br>2.  Create a container called curated<br>
![SA-9](../images/CreateStorageAcct-9.png)
<br><br>3.  Create a container called consumption<br>
![SA-10](../images/CreateStorageAcct-10.png)
<br><br>
![SA-11](../images/CreateStorageAcct-11.png)

### 5.0.1.3. Capture the storage account credentials
Storage account credentials is needed for accessing the storage account from the Databricks cluster.
![SA-5](../images/CreateStorageAcct-5.png)
<br><br>
![SA-6](../images/CreateStorageAcct-6.png)
<br><br>

## 5.0.2. Provision an Azure Databricks workspace
Provision an Azure Databricks workspace in the resource group - Telemetry-Processor<br>
[Documentation](https://docs.microsoft.com/en-us/azure/azure-databricks/quickstart-create-databricks-workspace-portal)<br>

![CDB-1](../images/CreateDatabricks-1.png)
<br><br><br>
![CDB-2](../images/CreateDatabricks-2.png)
<br><br><br>
Enter your workspace name and other details; Be sure to pick the same region as your device telemetry simulator.<br>
![CDB-3](../images/CreateDatabricks-3.png)
<br><br><br>
You should see the resource in your resource group. <br>
![CDB-5](../images/CreateDatabricks-5.png)
<br><br>

## 5.0.3. Provision an Azure Databricks cluster in the workspace
Provision an Azure Databricks cluster with 3 workers with default SKU, with ability to autoscale to 5 workers.<br>
[Documentation](https://docs.microsoft.com/en-us/azure/azure-databricks/quickstart-create-databricks-workspace-portal#create-a-spark-cluster-in-databricks)
![CDB-9](../images/CreateDatabricks-9.png)
<br><br><br>
![CDB-10](../images/CreateDatabricks-10.png)
<br><br><br>
![CDB-11](../images/CreateDatabricks-11.png)
<br><br><br>
![CDB-12](../images/CreateDatabricks-12.png)
<br><br><br>

## 5.0.4. Set up Vnet peering between Databricks and virtual network in the resource group (for Kafka)
Set up peering from the Databricks vnet to the Kafka vnet and vice-versa.<br>
[Documentation on Vnet peering](https://docs.azuredatabricks.net/administration-guide/cloud-configurations/azure/vnet-peering.html#vnet-peering)
![CDB-6](../images/CreateDatabricks-6.png)
<br><br><br>
![CDB-7](../images/CreateDatabricks-7.png)
<br><br><br>
![CDB-8](../images/CreateDatabricks-8.png)
<br><br><br>
![CDB-8a](../images/CreateDatabricks-8a.png)
<br><br><br>
![CDB-8b](../images/CreateDatabricks-8b.png)
<br><br><br>
![CDB-8c](../images/CreateDatabricks-8c.png)
<br><br><br>
![CDB-8d](../images/CreateDatabricks-8d.png)
<br><br><br>
![CDB-8e](../images/CreateDatabricks-8e.png)
<br><br><br>

## 5.0.5. Attach the Databricks notebooks (DBC) to the cluster
The URL for the DBC is https://github.com/anagha-microsoft/databricks-workshops/blob/master/iot/code/scala/dbc/iot-workshop.dbc.<br>
The following are steps for importing the DBC into the cluster.<br>
![DBC-1](../images/dbc-1.png)
<br><br><br>
![DBC-2](../images/dbc-2.png)
<br><br><br>
You should the workshop directory.<br>
![DBC-3](../images/dbc-3.png)
<br><br><br>

## 5.0.5. Add the Spark - Kafka dependencies to the cluster
Add the Spark Kafka library to the cluster.  Find the compatible version on Maven central.  For HDInsight 3.6, with Kafka 1.1/1.0/0.10.1, and Databricks Runtime 4.3, Spark 2.3.1, Scala 2.11, the author used-<br>
org.apache.spark:spark-sql-kafka-0-10_2.11:2.3.1<br><br>
The following are the steps to add Kafka depenency to the cluster.<br>
![LIB-1](../images/lib-1.png)
<br><br><br>
![LIB-2](../images/lib-2.png)
<br><br><br>
![LIB-3](../images/lib-3.png)
<br><br><br>
![LIB-4](../images/lib-4.png)
<br><br><br>

## 5.0.6. Add the Spark - Azure Cosmos DB dependencies to the cluster
Download the Azure Cosmos DB jar for SQL API.  At the time of authoring, the jar was available at:<br>
https://search.maven.org/search?q=a:azure-cosmosdb-spark_2.3.0_2.11

![CDB-LIB-1](../images/cdb-lib-1.png)
<br><br><br>
Attach to the cluster:<BR>
![CDB-LIB-2](../images/cdb-lib-2.png)
<br><br><br>
![CDB-LIB-3](../images/cdb-lib-3.png)
<br><br><br>
![CDB-LIB-4](../images/cdb-lib-4.png)
<br><br><br>
![CDB-LIB-5](../images/cdb-lib-5.png)
<br><br><br>
  
Validate if the cluster has the two dependencies attached:<br>  
![CDB-LIB-6](../images/cdb-lib-6.png)
<br><br><br>
![CDB-LIB-7](../images/cdb-lib-7.png)
<br><br><br>
![CDB-LIB-8](../images/cdb-lib-8.png)
<br><br><br>


## 5.0.7. Add the storage account credentials and Cosmos DB credentials to the cluster configuration
Navigate to the cluser configuration UI as shown below.<br>

### 5.0.7.1. Add the storage account credentials
Add the storage account credentials as key value pair separated by a space as show.<br>
```spark.hadoop.fs.azure.account.key.<YOUR_STORAGE_ACCOUNT_NAME>.blob.core.windows.net <YOUR_STORAGE_ACCOUNT_KEY>```

### 5.0.7.2. Add the Cosmos DB credentials
Add the Cosmos DB credentials - URI and key as space separate key-value pairs.<br>
```cdbEndpoint https://<YOUR_COSMOSDB_ACCOUNT>.documents.azure.com:443/```
```cdbAccessKey <YOUR_COSMOSDB_ACCOUNT_KEY>```

### 5.0.7.3. Ensure Databricks Delta is enabled
Add the following to a new line if it does not exist:<br>
```spark.databricks.delta.preview.enabled true```

<BR>
Step by step instructions:<br>

![DBD-8](../images/db-8.png)
<br><br><br>
![DBD-9](../images/db-9.png)
<br><br><br>
![DBD-10](../images/db-10.png)
<br><br><br>

Now restart the cluster.<br>
This concludes the Databricks setup for the workshop.<BR>
In the next module we will provision and configure Kafka.



