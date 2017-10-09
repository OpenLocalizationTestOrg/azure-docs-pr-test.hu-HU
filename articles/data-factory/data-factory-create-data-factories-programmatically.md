---
title: "aaaCreate adatok folyamatok Azure .NET SDK használatával |} Microsoft Docs"
description: "Ismerje meg, hogyan tooprogrammatically létrehozásához, figyeléséhez és felügyeletéhez Azure adat-előállítók Data Factory SDK használatával."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: b0a357be-3040-4789-831e-0d0a32a0bda5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 190b5f99edbb3c27e1e8efb8990b9e601b22458f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-monitor-and-manage-azure-data-factories-using-azure-data-factory-net-sdk"></a>Hozzon létre, felügyelete és kezelése az Azure Data Factory .NET SDK használatával Azure adat-előállítók
## <a name="overview"></a>Áttekintés
Hozzon létre, figyelése és kezelése az Azure adat-előállítók programozott módon a Data Factory .NET SDK használatával. A cikkben egy forgatókönyv, amelyeket követve toocreate minta .NET-Konzolalkalmazás, amely létrehoz és egy adat-előállító figyeli. 

> [!NOTE]
> Ez a cikk nem foglalkozik az összes hello Data Factory .NET API. Lásd: [Data Factory .NET API-referencia](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1) .NET API-t a Data Factory átfogó dokumentációját. 

## <a name="prerequisites"></a>Előfeltételek
* Visual Studio 2012, 2013 vagy 2015
* Töltse le és telepítse [Azure .NET SDK](http://azure.microsoft.com/downloads/).
* Azure PowerShell. Kövesse az utasításokat [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) tooinstall Azure PowerShell, a számítógépen a következő cikket. Használhatja az Azure PowerShell toocreate egy Azure Active Directory-alkalmazást.

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
    $azureAdApplication = New-AzureRmADApplication -DisplayName "ADFDotNetWalkthroughApp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.adfdotnetwalkthroughapp.org/example" -Password "Pass@word1"
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
Hello forgatókönyv akkor hozzon létre egy adat-előállító egy folyamatot, amely tartalmazza a másolási tevékenység. hello másolási tevékenység adatainak másolása az Azure blob storage tooanother mappában a hello mappából azonos blob-tároló. 

hello másolási tevékenység hello adatmozgás Azure Data Factory hajt végre. hello tevékenység egy globálisan elérhető szolgáltatás közötti biztonságos, megbízható és skálázható módon különböző adattárolókhoz másolhatja van-e kapcsolva. Lásd: [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) szóló cikkben olvashat hello másolási tevékenység.

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
4. Cserélje le a hello tartalmát **App.config** hello a projekt hello tartalom a következő fájlt: 
    
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
5. Hello App.Config fájlban frissítse az értékeket  **&lt;Alkalmazásazonosító&gt;**,  **&lt;jelszó&gt;**,  **&lt; Előfizetés-azonosító&gt;**, és  **&lt;bérlői azonosító&gt;**  saját értékekkel.
6. Adja hozzá a következő hello **használatával** utasítások toohello **Program.cs** fájl hello projektben.

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

    //IMPORTANT: specify hello name of Azure resource group here
    string resourceGroupName = "ADFTutorialResourceGroup";

    //IMPORTANT: hello name of hello data factory must be globally unique.
    // Therefore, update this value. For example:APITutorialFactory05122017
    string dataFactoryName = "APITutorialFactory";

    TokenCloudCredentials aadTokenCredentials = new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader().Result);

    Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);
    ```

   > [!IMPORTANT]
   > Cserélje le a hello értékének **resourceGroupName** hello nevet, az Azure-erőforráscsoportot. Létrehozhat egy olyan erőforráscsoport, hello segítségével [New-használják](/powershell/module/azurerm.resources/new-azurermresourcegroup) parancsmag.
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
9. Adja hozzá a kódot, amely létrehozza a következő hello **bemeneti és kimeneti adatkészletek** toohello **fő** metódust.

    Hello **FolderPath** hello bemeneti blob túl van-e állítva a**adftutorial /** ahol **adftutorial** hello név hello tároló a blob Storage tárolóban. Ha ez a tároló nem létezik az Azure blob Storage tárolóban, a tároló létrehozása ezen a néven: **adftutorial** , és töltse fel a szöveges fájl toohello tárolót.

    hello FolderPath hello a kimeneti blob értékre van állítva: **adftutorial/apifactoryoutput / {szelet}** ahol **szelet** dinamikusan számított alapján hello érték **SliceStart**(Indítás ideje minden szelet.)

    ```csharp
    // create input and output datasets
    Console.WriteLine("Creating input and output datasets");
    string Dataset_Source = "DatasetBlobSource";
    string Dataset_Destination = "DatasetBlobDestination";
    
    client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
    new DatasetCreateOrUpdateParameters()
    {
        Dataset = new Dataset()
        {
            Name = Dataset_Source,
            Properties = new DatasetProperties()
            {
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
    
    client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
    new DatasetCreateOrUpdateParameters()
    {
        Dataset = new Dataset()
        {
            Name = Dataset_Destination,
            Properties = new DatasetProperties()
            {
    
                LinkedServiceName = "AzureStorageLinkedService",
                TypeProperties = new AzureBlobDataset()
                {
                    FolderPath = "adftutorial/apifactoryoutput/{Slice}",
                    PartitionedBy = new Collection<Partition>()
                    {
                        new Partition()
                        {
                            Name = "Slice",
                            Value = new DateTimePartitionValue()
                            {
                                Date = "SliceStart",
                                Format = "yyyyMMdd-HH"
                            }
                        }
                    }
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
10. Adja hozzá a következő hello-kód **hoz létre, és aktiválja az adatcsatorna** toohello **fő** metódust. Az adatcsatorna **CopyActivity** paraméterrel meghatározott másolási tevékenysége a **BlobSource** értékét használja forrásként, és a **BlobSink** értékét fogadóként.

    hello másolási tevékenység hello adatmozgás Azure Data Factory hajt végre. hello tevékenység egy globálisan elérhető szolgáltatás közötti biztonságos, megbízható és skálázható módon különböző adattárolókhoz másolhatja van-e kapcsolva. Lásd: [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) szóló cikkben olvashat hello másolási tevékenység.

    ```csharp
    // create a pipeline
    Console.WriteLine("Creating a pipeline");
    DateTime PipelineActivePeriodStartTime = new DateTime(2014, 8, 9, 0, 0, 0, 0, DateTimeKind.Utc);
    DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
    string PipelineName = "PipelineBlobSample";
    
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
                        Name = "BlobToBlob",
                        Inputs = new List<ActivityInput>()
                        {
                            new ActivityInput()
                {
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
    
                },
            }
        }
    });
    ```
12. Adja hozzá a következő kód toohello hello **fő** metódus tooget hello állapotát egy adatszelet hello a kimeneti adatkészlet. Ez a minta a várt egyetlen szelet van.

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
13. **(választható)**  Hozzáadás hello következő kódot a adatok szelet toohello futtatása tooget részleteit **fő** metódust.

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
        });
    
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
14. Adja hozzá a következő hello segítő módszerének hello **fő** metódus toohello **Program** osztály. Ez a módszer egy párbeszédpanelt, amely, amely lehetővé teszi, hogy biztosítható a POP **felhasználónév** és **jelszó** használt toolog tooAzure portálon.

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

15. A Solution Explorer hello, bontsa ki a hello projekt: **DataFactoryAPITestApp**, kattintson a jobb gombbal **hivatkozások**, és kattintson a **hivatkozás hozzáadása**. Jelölje be a jelölőnégyzetet `System.Configuration` szerelvény, és kattintson **OK**.
15. Hello Konzolalkalmazás létrehozása. Kattintson a **Build** hello menüre, majd a **megoldás fordítása**.
16. Győződjön meg arról, hogy van legalább egy fájlt hello adftutorial tárolóban az Azure blob storage. Ha nem, hozzon létre Emp.txt fájlt a Jegyzettömbben a következő hello tartalom, és töltse fel az toohello adftutorial tároló.

    ```
    John, Doe
    Jane, Doe
    ```
17. Futtassa a hello minta kattintva **Debug** -> **Start Debugging** hello menüben. Amikor megjelenik a hello **első futtatása egy adatszelet részleteit**, várjon néhány percet, és nyomja le az **ENTER**.
18. Használja az Azure portál tooverify hello adott hello adat-előállító **APITutorialFactory** jön létre a következő összetevők hello:
    * Társított szolgáltatás: **AzureStorageLinkedService**
    * Adatkészlet: **DatasetBlobSource** és **DatasetBlobDestination**.
    * Adatcsatorna: **PipelineBlobSample**
19. Győződjön meg arról, hogy a kimeneti fájl létrejött-e hello **apifactoryoutput** hello mappájában **adftutorial** tároló.

## <a name="get-a-list-of-failed-data-slices"></a>Hibás adatszeletek listájának lekérdezése 

```csharp
// Parse hello resource path
var ResourceGroupName = "ADFTutorialResourceGroup";
var DataFactoryName = "DataFactoryAPITestApp";

var parameters = new ActivityWindowsByDataFactoryListParameters(ResourceGroupName, DataFactoryName);
parameters.WindowState = "Failed";
var response = dataFactoryManagementClient.ActivityWindows.List(parameters);
do
{
    foreach (var activityWindow in response.ActivityWindowListResponseValue.ActivityWindows)
    {
        var row = string.Join(
            "\t",
            activityWindow.WindowStart.ToString(),
            activityWindow.WindowEnd.ToString(),
            activityWindow.RunStart.ToString(),
            activityWindow.RunEnd.ToString(),
            activityWindow.DataFactoryName,
            activityWindow.PipelineName,
            activityWindow.ActivityName,
            string.Join(",", activityWindow.OutputDatasets));
        Console.WriteLine(row);
    }

    if (response.NextLink != null)
    {
        response = dataFactoryManagementClient.ActivityWindows.ListNext(response.NextLink, parameters);
    }
    else
    {
        response = null;
    }
}
while (response != null);
```

## <a name="next-steps"></a>Következő lépések
Tekintse meg a következő példa egy folyamatot, amely másol adatokat az Azure blob storage tooan Azure SQL-adatbázis .NET SDK használatával létrehozásához hello: 

- [A folyamat toocopy Blob Storage tooSQL adatbázis létrehozása](data-factory-copy-activity-tutorial-using-dotnet-api.md)
