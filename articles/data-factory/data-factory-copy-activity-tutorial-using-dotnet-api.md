---
title: "Oktatóanyag: Másolási tevékenységgel ellátott adatcsatorna létrehozása a .NET API használatával | Microsoft Docs"
description: "Ez az oktatóanyag bemutatja, hogyan hozhat létre másolási tevékenységgel rendelkező Azure Data Factory-adatcsatornákat a .NET API használatával."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 58fc4007-b46d-4c8e-a279-cb9e479b3e2b
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 52f72da54cdd80691e09d7453bf6730454c4089e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-net-api"></a>Oktatóanyag: Másolási tevékenységgel ellátott adatcsatorna létrehozása a .NET API használatával
> [!div class="op_single_selector"]
> * [Áttekintés és előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Másolás varázsló](data-factory-copy-data-wizard-tutorial.md)
> * [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Azure Resource Manager-sablon](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)

Ebből a cikkből megismerheti, hogyan toouse [.NET API](https://portal.azure.com) toocreate egy adat-előállítót, és egy folyamatot, amely másol adatokat az Azure blob storage tooan Azure SQL-adatbázis. Ha új tooAzure adat-előállítót, olvassa végig hello [Data Factory bemutatása tooAzure](data-factory-introduction.md) cikk Ez az oktatóanyag elvégzése előtt.   

Az oktatóanyag segítségével egyetlen tevékenységgel (másolási tevékenységgel) rendelkező folyamatot hozhat létre. hello másolási tevékenység során a támogatott adatokat tároló tooa támogatott fogadó adatokat tároló másol adatokat. A forrásként és fogadóként támogatott adattárak listájáért lásd: [támogatott adattárak](data-factory-data-movement-activities.md#supported-data-stores-and-formats). hello tevékenység egy globálisan elérhető szolgáltatás közötti biztonságos, megbízható és skálázható módon különböző adattárolókhoz másolhatja van-e kapcsolva. További információ a másolási tevékenység hello: [adatok mozgása tevékenységek](data-factory-data-movement-activities.md).

Egy folyamathoz több tevékenység is tartozhat. És láncolt telepítését (egymás után futtatni egy tevékenységet) két tevékenység által hello bemeneti hello az adatkészlet többi tevékenység hello kimeneti adatkészlet egy tevékenység beállítását. További információért lásd: [egy folyamaton belüli több tevékenység](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline). 

> [!NOTE] 
> A Data Factory .NET API teljes dokumentációját a [Data Factory szolgáltatással kapcsolatos .NET API-referencia](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1) tartalmazza.
> 
> hello adatok feldolgozási sor az oktatóanyag a forrás adatokat tároló tooa cél adatokat tároló másol adatokat. Hogyan oktatóanyagért tootransform adatok Azure Data Factory használatával, lásd: [oktatóanyag: a folyamat tootransform adatokat Hadoop-fürt létrehozása](data-factory-build-your-first-pipeline.md).

## <a name="prerequisites"></a>Előfeltételek
* Lépkedjen végig [oktatóanyag áttekintése és a szükséges előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) tooget hello oktatóanyag és teljes hello áttekintését **előfeltétel** lépéseket.
* Visual Studio 2012, 2013 vagy 2015
* Az [Azure .NET SDK](http://azure.microsoft.com/downloads/) letöltése és telepítése.
* Azure PowerShell. Kövesse az utasításokat [hogyan tooinstall és konfigurálja az Azure Powershellt](../powershell-install-configure.md) tooinstall Azure PowerShell, a számítógépen a következő cikket. Használhatja az Azure PowerShell toocreate egy Azure Active Directory-alkalmazást.

### <a name="create-an-application-in-azure-active-directory"></a>Alkalmazás létrehozása az Azure Active Directoryban
Hozzon létre egy Azure Active Directory-alkalmazás, hello alkalmazás szolgáltatásnevet létrehozni, és rendelje hozzá toohello **Data Factory közreműködői** szerepkör.

1. Indítsa el a **PowerShellt**.
2. Futtassa a következő parancs hello, és írja be a hello felhasználónevet és jelszót toosign használhatja a toohello Azure-portálon.

    ```PowerShell
    Login-AzureRmAccount
    ```
3. Futtassa a következő parancs tooview hello minden hello előfizetés ehhez a fiókhoz.

    ```PowerShell
    Get-AzureRmSubscription
    ```
4. Futtassa a következő parancs tooselect hello előfizetést, amelyet a toowork hello. Cserélje le  **&lt;NameOfAzureSubscription** &gt; hello nevet, az Azure-előfizetéshez.

    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```

   > [!IMPORTANT]
   > Jegyezze fel **SubscriptionId** és **TenantId** a parancs hello kimenetből.

5. Hozzon létre egy Azure erőforráscsoport nevű **ADFTutorialResourceGroup** a következő parancsot a hello PowerShell hello futtatásával.

    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```

    Ha hello erőforráscsoportban már létezik, akkor adja meg, hogy tooupdate (Y), vagy legyen (n).

    Ha egy másik erőforráscsoportban található használja, ebben az oktatóanyagban kell toouse hello ADFTutorialResourceGroup helyett az erőforráscsoport nevét.
6. Hozzon létre egy Azure Active Directory-alkalmazást.

    ```PowerShell
    $azureAdApplication = New-AzureRmADApplication -DisplayName "ADFCopyTutotiralApp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.adfcopytutorialapp.org/example" -Password "Pass@word1"
    ```

    Ha hiba történt a következő hello, adjon meg egy másik URL-címet, majd futtassa újra hello parancsot.
    
    ```PowerShell
    Another object with hello same value for property identifierUris already exists.
    ```
7. Hello AD szolgáltatásnevet létrehozni.

    ```PowerShell
    New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId
    ```
8. Adja hozzá a szolgáltatás egyszerű toohello **Data Factory közreműködői** szerepkör.

    ```PowerShell
    New-AzureRmRoleAssignment -RoleDefinitionName "Data Factory Contributor" -ServicePrincipalName $azureAdApplication.ApplicationId.Guid
    ```
9. Első hello azonosítót.

    ```PowerShell
    $azureAdApplication 
    ```
    Jegyezze fel hello Alkalmazásazonosítót (applicationID) hello kimenetből.

A fenti lépések elvégzésével beszereztük az alábbi négy értéket:

* Bérlőazonosító
* Előfizetés azonosítója
* Alkalmazásazonosító
* Jelszó (hello első parancsban megadott)

## <a name="walkthrough"></a>Útmutatás
1. Hozzon létre egy a C# nyelvet használó .NET-konzolalkalmazást a Visual Studio 2012/2013/2015 segítségével.
   1. Indítsa el a **Visual Studio** 2012/2013/2015-at.
   2. Kattintson a **fájl**, pont túl**új**, és kattintson a **projekt**.
   3. Bontsa ki a **Sablonok** lehetőséget, és válassza a **Visual C#** lehetőséget. Ebben az útmutatóban a C#-t fogjuk használni, de bármelyik más .NET-nyelv is alkalmazható.
   4. Válassza ki **Konzolalkalmazás** jobb hello projekttípusok hello listája.
   5. Adja meg **DataFactoryAPITestApp** a hello nevét.
   6. Válassza ki **C:\ADFGetStarted** hello hely számára.
   7. Kattintson a **OK** toocreate hello projekt.
2. Kattintson a **eszközök**, pont túl**NuGet-Csomagkezelő**, és kattintson a **Csomagkezelő konzol**.
3. A hello **Csomagkezelő konzol**, hello a következő lépéseket:
   1. Futtassa a következő parancs tooinstall adat-előállító csomag hello:`Install-Package Microsoft.Azure.Management.DataFactories`
   2. Futtassa a következő parancs tooinstall Azure Active Directory csomagot (Active Directory API-t használja a hello kódban) hello:`Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`
4. Adja hozzá a következő hello **appSetttings** szakasz toohello **App.config** fájlt. Ezek a beállítások használhatók hello segítő módszerrel: **GetAuthorizationHeader**.

    Az **&lt;Application ID&gt;**, **&lt;Password&gt;**, **&lt;Subscription ID&gt;** és **&lt;tenant ID&gt;** értékek helyére írja be saját értékeit.

    ```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <appSettings>
            <add key="ActiveDirectoryEndpoint" value="https://login.microsoftonline.com/" />
            <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
            <add key="WindowsManagementUri" value="https://management.core.windows.net/" />

            <add key="ApplicationId" value="your application ID" />
            <add key="Password" value="Password you used while creating hello AAD application" />
            <add key="SubscriptionId" value= "Subscription ID" />
            <add key="ActiveDirectoryTenantId" value="Tenant ID" />
        </appSettings>
    </configuration>
    ```

5. Adja hozzá a következő hello **használatával** utasítások toohello forrásfájl (Program.cs) hello projektben.

    ```csharp
    using System.Configuration;
    using System.Collections.ObjectModel;
    using System.Threading;
    using System.Threading.Tasks;

    using Microsoft.Azure;
    using Microsoft.Azure.Management.DataFactories;
    using Microsoft.Azure.Management.DataFactories.Models;
    using Microsoft.Azure.Management.DataFactories.Common.Models;

    using Microsoft.IdentityModel.Clients.ActiveDirectory;

    ```

6. Adja hozzá a következő kódot, amely létrehoz egy új hello **DataPipelineManagementClient** osztály toohello **fő** metódust. Egy adat-előállító, csatolt szolgáltatásként, bemeneti és kimeneti adatkészletek és egy folyamatot ezen objektum toocreate használhatja. Az objektum toomonitor szeletek a DataSet adatkészlet futtatás közben is használ.

    ```csharp
    // create data factory management client
    string resourceGroupName = "ADFTutorialResourceGroup";
    string dataFactoryName = "APITutorialFactory";

    TokenCloudCredentials aadTokenCredentials = new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader().Result);

    Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);
    ```

   > [!IMPORTANT]
   > Cserélje le a hello értékének **resourceGroupName** hello nevet, az Azure-erőforráscsoportot.
   >
   > Frissítés hello data factory (dataFactoryName) toobe egyedi neve. Hello adat-előállító nevét globálisan egyedinek kell lennie. A Data Factory-összetevők elnevezési szabályait a [Data Factory - Naming Rules](data-factory-naming-rules.md) (Data Factory – Elnevezési szabályok) című témakörben találhatja.

7. Adja hozzá a kódot, amely létrehozza a következő hello egy **adat-előállító** toohello **fő** metódust.

    ```csharp
    // create a data factory
    Console.WriteLine("Creating a data factory");
    client.DataFactories.CreateOrUpdate(resourceGroupName,
        new DataFactoryCreateOrUpdateParameters()
        {
            DataFactory = new DataFactory()
            {
                Name = dataFactoryName,
                Location = "westus",
                Properties = new DataFactoryProperties()
            }
        }
    );
    ```

    A data factory egy vagy több folyamattal rendelkezhet. A folyamaton belül egy vagy több tevékenység lehet. Adja meg például a másolási tevékenység toocopy forrás tooa cél adattárat és adatait egy HDInsight Hive tevékenység toorun a Hive parancsfájl tootransform adatok tooproduct kimeneti adatokat. Kezdjük, az ebben a lépésben hello adat-előállító létrehozása.
8. Adja hozzá a kódot, amely létrehozza a következő hello egy **Azure Storage társított szolgáltatásnak** toohello **fő** metódust.

   > [!IMPORTANT]
   > A **storageaccountname** és az **accountkey** értékek helyére írja be Azure Storage-tárfiókja nevét, illetve kulcsát.

    ```csharp
    // create a linked service for input data store: Azure Storage
    Console.WriteLine("Creating Azure Storage linked service");
    client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
        new LinkedServiceCreateOrUpdateParameters()
        {
            LinkedService = new LinkedService()
            {
                Name = "AzureStorageLinkedService",
                Properties = new LinkedServiceProperties
                (
                    new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<storageaccountname>;AccountKey=<accountkey>")
                )
            }
        }
    );
    ```

    A data factory toolink adatait tárolja, és számítási szolgáltatások toohello adat-előállító létrehozása társított szolgáltatások. Ebben az oktatóanyagban nem használunk számítási szolgáltatásokat (például Azure HDInsight vagy Azure Data Lake Analytics). Csak kétféle típusú adattárat használunk: Azure Storage (forrás) és Azure SQL Database (cél). 

    Ezért két társított szolgáltatást fog létrehozni AzureStorageLinkedService és AzureSqlLinkedService néven (típus: AzureStorage és AzureSqlDatabase).  

    hello AzureStorageLinkedService művelet ugyan összeköti a az Azure storage-fiók toohello adat-előállítóban. Ez a tárfiók egy hello akkor létrejött a tároló és hello adatok részeként feltöltött [Előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
9. Adja hozzá a kódot, amely létrehozza a következő hello egy **Azure SQL társított szolgáltatásnak** toohello **fő** metódust.

   > [!IMPORTANT]
   > A **servername**, **databasename**, **username** és **password** paraméterek értékét cserélje le az Azure SQL-kiszolgáló, az adatbázis és a felhasználói fiók nevére, valamint a felhasználói fiók jelszavára.

    ```csharp
    // create a linked service for output data store: Azure SQL Database
    Console.WriteLine("Creating Azure SQL Database linked service");
    client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
        new LinkedServiceCreateOrUpdateParameters()
        {
            LinkedService = new LinkedService()
            {
                Name = "AzureSqlLinkedService",
                Properties = new LinkedServiceProperties
                (
                    new AzureSqlDatabaseLinkedService("Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30")
                )
            }
        }
    );
    ```

    AzureSqlLinkedService az Azure SQL adatbázis toohello adat-előállító hivatkozásokat tartalmaz. hello blobtárolóból másolt hello adatok az adatbázis tárolja. Hello üres tábla az adatbázisban részeként létrehozott [Előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
10. Adja hozzá a kódot, amely létrehozza a következő hello **bemeneti és kimeneti adatkészletek** toohello **fő** metódust.

    ```csharp
    // create input and output datasets
    Console.WriteLine("Creating input and output datasets");
    string Dataset_Source = "InputDataset";
    string Dataset_Destination = "OutputDataset";

    Console.WriteLine("Creating input dataset of type: Azure Blob");
    client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,

    new DatasetCreateOrUpdateParameters()
    {
        Dataset = new Dataset()
        {
            Name = Dataset_Source,
            Properties = new DatasetProperties()
            {
                Structure = new List<DataElement>()
                {
                    new DataElement() { Name = "FirstName", Type = "String" },
                    new DataElement() { Name = "LastName", Type = "String" }
                },
                LinkedServiceName = "AzureStorageLinkedService",
                TypeProperties = new AzureBlobDataset()
                {
                    FolderPath = "adftutorial/",
                    FileName = "emp.txt"
                },
                External = true,
                Availability = new Availability()
                {
                    Frequency = SchedulePeriod.Hour,
                    Interval = 1,
                },

                Policy = new Policy()
                {
                    Validation = new ValidationPolicy()
                    {
                        MinimumRows = 1
                    }
                }
            }
        }
    });

    Console.WriteLine("Creating output dataset of type: Azure SQL");
    client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
        new DatasetCreateOrUpdateParameters()
        {
            Dataset = new Dataset()
            {
                Name = Dataset_Destination,
                Properties = new DatasetProperties()
                {
                    Structure = new List<DataElement>()
                    {
                        new DataElement() { Name = "FirstName", Type = "String" },
                        new DataElement() { Name = "LastName", Type = "String" }
                    },
                    LinkedServiceName = "AzureSqlLinkedService",
                    TypeProperties = new AzureSqlTableDataset()
                    {
                        TableName = "emp"
                    },
                    Availability = new Availability()
                    {
                        Frequency = SchedulePeriod.Hour,
                        Interval = 1,
                    },
                }
            }
        });
    ```
    
    Hello előző lépésben létrehozott összekapcsolt szolgáltatások toolink az Azure Storage-fiók és az Azure SQL adatbázis tooyour adat-előállítóban. Ebben a lépésben InputDataset és OutputDataset, amelyek megfelelnek a bemeneti és kimeneti adatok AzureStorageLinkedService és AzureSqlLinkedService által hivatkozott hello adattárolókhoz tárolt nevű két adatkészletet határozza meg.

    hello Azure tárolás társított szolgáltatásának határozza meg a Data Factory szolgáltatásnak a futási idő tooconnect tooyour Azure storage-fiók használó hello kapcsolati karakterláncot. És hello bemeneti blob-adathalmazra (InputDataset) hello tároló és hello bemeneti adatokat tartalmazó hello mappát adja meg.  

    Hello csatolt Azure SQL Database szolgáltatáshoz hasonlóan hello kapcsolati karakterláncot használó Data Factory szolgáltatásnak a futási idő tooconnect tooyour Azure SQL adatbázis határozza meg. És hello kimeneti SQL táblázat dataset (OututDataset) hello adatbázis toowhich hello hello blobtárolóból az adatok másolásakor hello tábla határozza meg.

    Ebben a lépésben hoz létre, mely tooa fájlját (emp.txt) InputDataset nevű adatkészlete hello gyökérmappájában lévő mappának a blob-tároló (adftutorial) az Azure Storage hello AzureStorageLinkedService kapcsolódó szolgáltatás által képviselt hello. Ha nem hello fájlnév értéket (vagy hagyja ki), a hello bemeneti mappában található összes BLOB adatait is másolt toohello cél. Ebben az oktatóanyagban hello fájlnév értéket kell megadni.    

    Ebben a lépésben egy kimeneti adatkészletet hoz létre **OutputDataset** néven. Ez az adatkészlet mutat tooa SQL táblázat hello Azure SQL Database által képviselt **AzureSqlLinkedService**.
11. Adja hozzá a következő hello-kód **hoz létre, és aktiválja az adatcsatorna** toohello **fő** metódust. Ebben a lépésben létrehoz egy **másolási tevékenységgel** rendelkező folyamatot, amely bemenetként az **InputDataset**, kimenetként pedig az **OutputDataset** adatkészletet használja.

    ```csharp
    // create a pipeline
    Console.WriteLine("Creating a pipeline");
    DateTime PipelineActivePeriodStartTime = new DateTime(2017, 5, 11, 0, 0, 0, 0, DateTimeKind.Utc);
    DateTime PipelineActivePeriodEndTime = new DateTime(2017, 5, 12, 0, 0, 0, 0, DateTimeKind.Utc);
    string PipelineName = "ADFTutorialPipeline";

    client.Pipelines.CreateOrUpdate(resourceGroupName, dataFactoryName,
        new PipelineCreateOrUpdateParameters()
        {
            Pipeline = new Pipeline()
            {
                Name = PipelineName,
                Properties = new PipelineProperties()
                {
                    Description = "Demo Pipeline for data transfer between blobs",

                    // Initial value for pipeline's active period. With this, you won't need tooset slice status
                    Start = PipelineActivePeriodStartTime,
                    End = PipelineActivePeriodEndTime,

                    Activities = new List<Activity>()
                    {
                        new Activity()
                        {
                            Name = "BlobToAzureSql",
                            Inputs = new List<ActivityInput>()
                            {
                                new ActivityInput() {
                                    Name = Dataset_Source
                                }
                            },
                            Outputs = new List<ActivityOutput>()
                            {
                                new ActivityOutput()
                                {
                                    Name = Dataset_Destination
                                }
                            },
                            TypeProperties = new CopyActivity()
                            {
                                Source = new BlobSource(),
                                Sink = new BlobSink()
                                {
                                    WriteBatchSize = 10000,
                                    WriteBatchTimeout = TimeSpan.FromMinutes(10)
                                }
                            }
                        }
                    }
                }
            }
        });
    ```

    Vegye figyelembe a következő pontok hello:
   
    - Hello tevékenységek szakaszban csak egy tevékenység nincs amelynek **típus** értéke túl**másolási**. Hello másolási tevékenység kapcsolatos további információkért lásd: [adatok mozgása tevékenységek](data-factory-data-movement-activities.md). A Data Factory megoldásaiban használhatja az [adatátalakítási tevékenységeket](data-factory-data-transformation-activities.md) is.
    - Adjon meg a hello tevékenység értéke túl**InputDataset** és hello tevékenység túl van-e állítva a kimeneti**OutputDataset**. 
    - A hello **typeProperties** szakaszban **BlobSource** hello forrás típusaként van megadva, és **SqlSink** hello a fogadó típusa van megadva. Források és mosdók hello másolási tevékenység által támogatott adattárolókhoz teljes listáját lásd: [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Hogyan toouse a támogatott adatokat tárolót, mint a forrás/fogadó toolearn hivatkozásra hello hello táblában.  
   
    Kimeneti adatkészlet jelenleg milyen meghajtók hello ütemezést. Ebben az oktatóanyagban a kimeneti adatkészlet óránként konfigurált tooproduce szelet. hello folyamat rendelkezik egy kezdési és befejezési időpontja, amelyek egy nap telhet el, amely 24 óra. Ezért a kimeneti adatkészlet 24 szeletek előállított hello folyamat.
12. Adja hozzá a következő kód toohello hello **fő** metódus tooget hello állapotát egy adatszelet hello a kimeneti adatkészlet. Ebben a példában csupán egy szeletet fogunk kapni.

    ```csharp
    // Pulling status within a timeout threshold
    DateTime start = DateTime.Now;
    bool done = false;

    while (DateTime.Now - start < TimeSpan.FromMinutes(5) && !done)
    {
        Console.WriteLine("Pulling hello slice status");        
        // wait before hello next status check
        Thread.Sleep(1000 * 12);

        var datalistResponse = client.DataSlices.List(resourceGroupName, dataFactoryName, Dataset_Destination,
            new DataSliceListParameters()
            {
                DataSliceRangeStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString(),
                DataSliceRangeEndTime = PipelineActivePeriodEndTime.ConvertToISO8601DateTimeString()
            });

        foreach (DataSlice slice in datalistResponse.DataSlices)
        {
            if (slice.State == DataSliceState.Failed || slice.State == DataSliceState.Ready)
            {
                Console.WriteLine("Slice execution is done with status: {0}", slice.State);
                done = true;
                break;
            }
            else
            {
                Console.WriteLine("Slice status is: {0}", slice.State);
            }
        }
    }
    ```

13. Adja hozzá a következő kód futtatása tooget adatok számára egy adatok szelet toohello hello **fő** metódust.

    ```csharp
    Console.WriteLine("Getting run details of a data slice");

    // give it a few minutes for hello output slice toobe ready
    Console.WriteLine("\nGive it a few minutes for hello output slice toobe ready and press any key.");
    Console.ReadKey();

    var datasliceRunListResponse = client.DataSliceRuns.List(
            resourceGroupName,
            dataFactoryName,
            Dataset_Destination,
            new DataSliceRunListParameters()
            {
                DataSliceStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString()
            }
        );

    foreach (DataSliceRun run in datasliceRunListResponse.DataSliceRuns)
    {
        Console.WriteLine("Status: \t\t{0}", run.Status);
        Console.WriteLine("DataSliceStart: \t{0}", run.DataSliceStart);
        Console.WriteLine("DataSliceEnd: \t\t{0}", run.DataSliceEnd);
        Console.WriteLine("ActivityId: \t\t{0}", run.ActivityName);
        Console.WriteLine("ProcessingStartTime: \t{0}", run.ProcessingStartTime);
        Console.WriteLine("ProcessingEndTime: \t{0}", run.ProcessingEndTime);
        Console.WriteLine("ErrorMessage: \t{0}", run.ErrorMessage);
    }

    Console.WriteLine("\nPress any key tooexit.");
    Console.ReadKey();
    ```

14. Adja hozzá a következő hello segítő módszerének hello **fő** metódus toohello **Program** osztály.

    > [!NOTE] 
    > Másolja és illessze be a kódját a következő hello, győződjön meg arról, hogy hello másolt kód jelenleg hello azonos szinten, hello fő metódus.

    ```csharp
    public static async Task<string> GetAuthorizationHeader()
    {
        AuthenticationContext context = new AuthenticationContext(ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] + ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);
        ClientCredential credential = new ClientCredential(
            ConfigurationManager.AppSettings["ApplicationId"],
            ConfigurationManager.AppSettings["Password"]);
        AuthenticationResult result = await context.AcquireTokenAsync(
            resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
            clientCredential: credential);

        if (result != null)
            return result.AccessToken;

        throw new InvalidOperationException("Failed tooacquire token");
    }
    ```

15. A Megoldáskezelőben hello, bontsa ki a hello project (DataFactoryAPITestApp), kattintson a jobb gombbal **hivatkozások**, és kattintson a **hivatkozás hozzáadása**. Jelölje be a **System.Configuration** szerelvényhez tartozó jelölőnégyzetet. Kattintson **OK** gombra.
16. Hello Konzolalkalmazás létrehozása. Kattintson a **Build** hello menüre, majd a **megoldás fordítása**.
17. Győződjön meg arról, hogy nincs-e legalább egy fájl hello **adftutorial** az Azure blob Storage tárolóban. Ha nem, hozzon létre **Emp.txt** hello tartalom a következő fájlt a Jegyzettömbben, és töltse fel toohello adftutorial tároló.

    ```
    John, Doe
    Jane, Doe
    ```
18. Futtassa a hello minta kattintva **Debug** -> **Start Debugging** hello menüben. Amikor megjelenik a hello **első futtatása egy adatszelet részleteit**, várjon néhány percet, és nyomja le az **ENTER**.
19. Használja az Azure portál tooverify hello adott hello adat-előállító **APITutorialFactory** jön létre a következő összetevők hello:
   * Társított szolgáltatás: **LinkedService_AzureStorage**
   * Adatkészlet: **InputDataset** és **OutputDataset**.
   * Adatcsatorna: **PipelineBlobSample**
20. Győződjön meg arról, hogy a két hello alkalmazott rekordok hello létrejönnek **üres** hello táblázat megadott Azure SQL-adatbázis.

## <a name="next-steps"></a>Következő lépések
A Data Factory .NET API teljes dokumentációját a [Data Factory szolgáltatással kapcsolatos .NET API-referencia](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1) tartalmazza.

Ez az oktatóanyag egy olyan másolási műveletet mutatott be, amelynek a forrásadattára egy Azure Blob Storage-tár, a céladattára pedig egy Azure SQL-adatbázis volt. hello következő táblázat felsorolja az adatforrások és a célhelyek hello másolási tevékenység által támogatott adattárolókhoz: 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

Hogyan toocopy adatok belőle egy adatok tárolására, kapcsolatos toolearn hivatkozásra hello hello adattároló hello tábla.

