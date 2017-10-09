Az Azure Data Factory támogatja a következő átalakító, amely lehet hozzáadott toopipelines vagy külön-külön vagy visszavezethetők rendelkező tevékenységek egy másik tevékenység hello.

| Adatátalakítási tevékenység | Számítási környezet |
|:--- |:--- |
| [Hive](../articles/data-factory/data-factory-hive-activity.md) |HDInsight [Hadoop] |
| [Pig](../articles/data-factory/data-factory-pig-activity.md) |HDInsight [Hadoop] |
| [MapReduce](../articles/data-factory/data-factory-map-reduce.md) |HDInsight [Hadoop] |
| [Hadoop Streaming](../articles/data-factory/data-factory-hadoop-streaming-activity.md) |HDInsight [Hadoop] |
| [Spark](../articles/data-factory/data-factory-spark.md) | HDInsight [Hadoop] |
| [Machine Learning-tevékenységek: kötegelt végrehajtás és erőforrás frissítése](../articles/data-factory/data-factory-azure-ml-batch-execution-activity.md) |Azure VM |
| [Tárolt eljárás](../articles/data-factory/data-factory-stored-proc-activity.md) |Azure SQL, Azure SQL Data Warehouse vagy SQL Server |
| [Data Lake Analytics U-SQL](../articles/data-factory/data-factory-usql-activity.md) |Azure Data Lake Analytics |
| [DotNet](../articles/data-factory/data-factory-use-custom-activities.md) |HDInsight [Hadoop] vagy Azure Batch |

> [!NOTE]
> A HDInsight Spark-fürt a MapReduce tevékenység toorun Spark programokat is használhat. A részletekért lásd: [Invoke Spark programs from Azure Data Factory](../articles/data-factory/data-factory-spark.md) (Spark-programok meghívása az Azure Data Factory-ból).
> Létrehozhat egyéni tevékenység toorun R parancsfájlok a HDInsight-fürt az R telepítve. Lásd: [Run R Script using Azure Data Factory](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample) (R-parancsfájl futtatása az Azure Data Factory használatával).
> 
> 

