---
title: az Azure Stream Analytics aaaManagement .NET SDK v1.x |} Microsoft Docs
description: "Első lépések a Stream Analytics felügyeleti .NET SDK-val. Megtudhatja, hogyan tooset össze, és az analytics-feladatok futtatása. Hozzon létre egy projekt, bemenetek, kimenetek és átalakításához."
keywords: .NET SDK-val analytics API
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 5e93de87-0c6f-4f4b-be98-08d63f832897
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/06/2017
ms.author: jeffstok
ms.openlocfilehash: d205c388880e3d9c2ca5df218f4b68abac8c9780
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="management-net-sdk-v1x-set-up-and-run-analytics-jobs-using-hello-azure-stream-analytics-api-for-net"></a>Felügyeleti .NET SDK v1.x: állítson be és futtatási analytics-feladatok használatával hello Azure Stream Analytics API a .NET-hez
Ismerje meg, hogyan tooset fel és futtatási analytics-feladatok hello Stream Analytics API használatával a .NET használatával hello felügyeleti .NET SDK-val. Projekt beállítása, hozzon létre a bemeneti és kimeneti adatforrások, átalakítások, és úgy indítsa és feladatok. Az analytics-feladatok adatok Blob-tároló vagy az eseményközpont folyamatos átviteléhez.

Lásd: hello [felügyeleti referenciadokumentációt hello Stream Analytics API a .NET-keretrendszerhez készült](https://msdn.microsoft.com/library/azure/dn889315.aspx).

Az Azure Stream Analytics egy teljes körűen felügyelt szolgáltatás biztosítása kis késleltetésű, magas rendelkezésre állású, méretezhető, összetett esemény feldolgozása keresztül adatfolyam hello felhőben. A Stream Analytics lehetővé teszi, hogy az ügyfelek tooset folyamatos átviteli feladatok tooanalyze adatfolyamot fel, és közel valós idejű elemzési toodrive lehetővé teszi.  

> [!NOTE]
> Ebben a cikkben a példakód továbbra is az Azure Stream Analytics Management .NET SDK örökölt (1.x) verzióját használja. A mintakód hello naprakész SDK verzióját használja, ellenőrizze [a Stream Analytics Management .NET SDK használata hello](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-dotnet-management-sdk).

## <a name="prerequisites"></a>Előfeltételek
Ez a cikk elkezdéséhez hello következő kell rendelkeznie:

* Telepítse a Visual Studio 2017 vagy 2015.
* Töltse le és telepítse [Azure .NET SDK](https://azure.microsoft.com/downloads/).
* Azure-erőforráscsoport létrehozása az előfizetésben. hello az alábbiakban látható egy minta Azure PowerShell-parancsfájlt. Azure PowerShell információkért lásd: [telepítse és konfigurálja az Azure Powershellt](/powershell/azure/overview);  

        # Log in tooyour Azure account
        Add-AzureAccount

        # Select hello Azure subscription you want toouse toocreate hello resource group
        Select-AzureSubscription -SubscriptionName <subscription name>

            # If Stream Analytics has not been registered toohello subscription, remove hello remark symbol (#) toorun hello Register-AzureRMProvider cmdlet tooregister hello provider namespace
            #Register-AzureRMProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>


* Állítson be egy bemeneti forrás és cél toouse kimeneti. A további tudnivalókat [bemenet hozzáadása](stream-analytics-add-inputs.md) be egy minta bemeneti tooset és [hozzáadása kimenetek](stream-analytics-add-outputs.md) tooset be egy minta kimenet.

## <a name="set-up-a-project"></a>Projekt beállítása
toocreate analytics-feladat hello Stream Analytics API használata a .NET, először állítsa be a projekthez.

1. Hozzon létre egy Visual Studio C# .NET konzolalkalmazást.
2. Hello Csomagkezelő konzol, a következő futtatási hello parancsokat tooinstall hello NuGet-csomagok. hello először egyik hello Azure Stream Analytics felügyeleti .NET SDK-t. hello másikat a hello Azure Active Directory ügyfél-hitelesítéshez használandó.
   
        Install-Package Microsoft.Azure.Management.StreamAnalytics -Version 1.8.3
        Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.28.4
3. Adja hozzá a következő hello **appSettings** szakasz toohello App.config fájlban:
   
        <appSettings>
          <!--CSM Prod related values-->
          <add key="ActiveDirectoryEndpoint" value="https://login.microsoftonline.com/" />
          <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
          <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
          <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
          <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
          <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION" />
          <add key="ActiveDirectoryTenantId" value="YOU TENANT ID" />
        </appSettings>

    Cserélje le az értékeket **SubscriptionId** és **ActiveDirectoryTenantId** a Azure-előfizetés és a bérlői azonosítók. Ezek az értékek kaphat hello Azure PowerShell-parancsmag a következő futtatásával:

        Get-AzureAccount

4. Adja hozzá a következő hivatkozás a .csproj fájlban hello:

        <Reference Include="System.Configuration" />

1. Adja hozzá a következő hello **használatával** utasítások toohello forrásfájl (Program.cs) hello projektben:
   
        using System;
        using System.Configuration;
        using System.Threading.Tasks;
        
        using Microsoft.Azure;
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
2. Adja hozzá a segítő hitelesítési módszert:

   ```   
   private static async Task<string> GetAuthorizationHeader()
   {
       var context = new AuthenticationContext(
           ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] +
           ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

        AuthenticationResult result = await context.AcquireTokenASync(
           resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
           clientId: ConfigurationManager.AppSettings["AsaClientId"],
           redirectUri: new Uri(ConfigurationManager.AppSettings["RedirectUri"]),
           promptBehavior: PromptBehavior.Always);

        if (result != null)
            return result.AccessToken;

       throw new InvalidOperationException("Failed tooacquire token");
   }
   ```  

## <a name="create-a-stream-analytics-management-client"></a>Hozzon létre egy Stream Analytics management ügyfél
A **StreamAnalyticsManagementClient** objektum lehetővé teszi a toomanage hello feladat és hello feladat összetevők, például bemeneti, kimeneti és átalakítás.

Adja hozzá a következő kód toohello elejére hello hello **fő** módszert:

    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";
    string streamAnalyticsInputName = "<YOUR JOB INPUT NAME>";
    string streamAnalyticsOutputName = "<YOUR JOB OUTPUT NAME>";
    string streamAnalyticsTransformationName = "<YOUR JOB TRANSFORMATION NAME>";

    // Get authentication token
    TokenCloudCredentials aadTokenCredentials = new TokenCloudCredentials(
        ConfigurationManager.AppSettings["SubscriptionId"],
        GetAuthorizationHeader().Result);

    // Create Stream Analytics management client
    StreamAnalyticsManagementClient client = new StreamAnalyticsManagementClient(aadTokenCredentials);

Hello **resourceGroupName** változó értékét a hello erőforrás hello neve azonos gazdagépcsoport létrehozott vagy hello előfeltételként szükséges lépések kivételezett hello kell lennie.

tooautomate hello credential bemutató szempontja, hogy a feladat létrehozása, tekintse meg a túl[hitelesítéséhez az Azure Resource Manager szolgáltatásnevet](../azure-resource-manager/resource-group-authenticate-service-principal.md).

hello Ez a cikk fennmaradó részében feltételezik, hogy ez a kód hello hello elején **fő** metódust.

## <a name="create-a-stream-analytics-job"></a>Stream Analytics-feladat létrehozása
hello alábbi kód létrehoz egy Stream Analytics-feladat a megadott erőforráscsoport hello. Egy bemeneti, kimeneti és átalakítása toohello feladat később fogja hozzáadni.

    // Create a Stream Analytics job
    JobCreateOrUpdateParameters jobCreateParameters = new JobCreateOrUpdateParameters()
    {
        Job = new Job()
        {
            Name = streamAnalyticsJobName,
            Location = "<LOCATION>",
            Properties = new JobProperties()
            {
                EventsOutOfOrderPolicy = EventsOutOfOrderPolicy.Adjust,
                Sku = new Sku()
                {
                    Name = "Standard"
                }
            }
        }
    };

    JobCreateOrUpdateResponse jobCreateResponse = client.StreamingJobs.CreateOrUpdate(resourceGroupName, jobCreateParameters);


## <a name="create-a-stream-analytics-input-source"></a>Hozzon létre egy Stream Analytics bemeneti forrása
hello alábbira hoz létre a Stream Analytics bemeneti forrás hello blob bemeneti forrása típusa és a fürt megosztott kötetei szolgáltatás szerializálást. az event hubs ezen bemeneti forrása, toocreate használja **EventHubStreamInputDataSource** helyett **BlobStreamInputDataSource**. Hasonlóképpen testre szabhatja a hello szerializálási típushoz hello bemeneti forrás.

    // Create a Stream Analytics input source
    InputCreateOrUpdateParameters jobInputCreateParameters = new InputCreateOrUpdateParameters()
    {
        Input = new Input()
        {
            Name = streamAnalyticsInputName,
            Properties = new StreamInputProperties()
            {
                Serialization = new CsvSerialization
                {
                    Properties = new CsvSerializationProperties
                    {
                        Encoding = "UTF8",
                        FieldDelimiter = ","
                    }
                },
                DataSource = new BlobStreamInputDataSource
                {
                    Properties = new BlobStreamInputDataSourceProperties
                    {
                        StorageAccounts = new StorageAccount[]
                        {
                            new StorageAccount()
                            {
                                AccountName = "<YOUR STORAGE ACCOUNT NAME>",
                                AccountKey = "<YOUR STORAGE ACCOUNT KEY>"
                            }
                        },
                        Container = "samples",
                        PathPattern = ""
                    }
                }
            }
        }
    };

    InputCreateOrUpdateResponse inputCreateResponse =
        client.Inputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobInputCreateParameters);

Beviteli móddal, akár a Blob-tároló vagy egy eseményközpontot, kapcsolt tooa adott feladat. toouse hello ugyanazon bemeneti forrás a különböző feladatokhoz, kell hello metódust hívja meg ismét, és adjon meg egy másik feladatnévvel.

## <a name="test-a-stream-analytics-input-source"></a>A Stream Analytics bemeneti forrás tesztelése
Hello **TestConnection** e hello Stream Analytics-feladat-e képes tooconnect toohello bemeneti forrás, valamint egyéb szempontok adott toohello metódus tesztek bemeneti forrás típusa. Például hello blob bemeneti forrása egy korábbi lépésben létrehozott hello metódus ellenőrzi, hogy hello tárfióknév kulcspár is használt tooconnect toohello tárfiókot kell, valamint ellenőrizze, hogy hello megadott tároló létezik-e.

    // Test input source connection
    DataSourceTestConnectionResponse inputTestResponse =
        client.Inputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsInputName);

## <a name="create-a-stream-analytics-output-target"></a>A Stream Analytics kimeneti cél létrehozásához
Nagyon hasonló toocreating a Stream Analytics bemeneti forrás egy kimeneti cél létrehozása történik. Beviteli móddal, például a kimeneti célpontjai kapcsolt tooa adott feladat. toouse hello azonos kimeneti célja a különböző feladatokhoz, kell hello metódust hívja meg ismét, és adjon meg egy másik feladatnévvel.

a következő kód hello hoz létre egy kimeneti célhoz (Azure SQL-adatbázis). Testre szabhatja hello kimeneti cél adatok típusát és/vagy szerializálási típushoz.

    // Create a Stream Analytics output target
    OutputCreateOrUpdateParameters jobOutputCreateParameters = new OutputCreateOrUpdateParameters()
    {
        Output = new Output()
        {
            Name = streamAnalyticsOutputName,
            Properties = new OutputProperties()
            {
                DataSource = new SqlAzureOutputDataSource()
                {
                    Properties = new SqlAzureOutputDataSourceProperties()
                    {
                        Server = "<YOUR DATABASE SERVER NAME>",
                        Database = "<YOUR DATABASE NAME>",
                        User = "<YOUR DATABASE LOGIN>",
                        Password = "<YOUR DATABASE LOGIN PASSWORD>",
                        Table = "<YOUR DATABASE TABLE NAME>"
                    }
                }
            }
        }
    };

    OutputCreateOrUpdateResponse outputCreateResponse =
        client.Outputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobOutputCreateParameters);

## <a name="test-a-stream-analytics-output-target"></a>A Stream Analytics kimeneti cél tesztelése
A Stream Analytics kimeneti cél is tartozik hello **TestConnection** kapcsolatok tesztelése a módszert.

    // Test output target connection
    DataSourceTestConnectionResponse outputTestResponse =
        client.Outputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsOutputName);

## <a name="create-a-stream-analytics-transformation"></a>Hozzon létre egy Stream Analytics átalakítása
hello alábbira hoz létre a Stream Analytics átalakítás hello lekérdezés "Válasszon * bemeneti" és egy streamelési egységet tooallocate hello Stream Analytics-feladat határozza meg. A streamelési egységek beállítása további információkért lásd: [Scale Azure Stream Analytics-feladatok](stream-analytics-scale-jobs.md).

    // Create a Stream Analytics transformation
    TransformationCreateOrUpdateParameters transformationCreateParameters = new TransformationCreateOrUpdateParameters()
    {
        Transformation = new Transformation()
        {
            Name = streamAnalyticsTransformationName,
            Properties = new TransformationProperties()
            {
                StreamingUnits = 1,
                Query = "select * from Input"
            }
        }
    };

    var transformationCreateResp =
        client.Transformations.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, transformationCreateParameters);

Bemeneti és kimeneti, például átalakítás egyben a feltételekhez toohello adott Stream Analytics-feladat csoportban hozták létre.

## <a name="start-a-stream-analytics-job"></a>A Stream Analytics-feladat indítása
Miután létrehozta a Stream Analytics-feladat, és a input(s), kimenete és átalakítása, elindíthatja hello feladat hívó hello **Start** metódust.

a következő példakód hello Stream Analytics-feladat kezdődik-e egy egyéni kimeneti kezdési idő beállítása tooDecember 12, 2012, 12:12:12 UTC:

    // Start a Stream Analytics job
    JobStartParameters jobStartParameters = new JobStartParameters
    {
        OutputStartMode = OutputStartMode.CustomTime,
        OutputStartTime = new DateTime(2012, 12, 12, 0, 0, 0, DateTimeKind.Utc)
    };

    LongRunningOperationResponse jobStartResponse = client.StreamingJobs.Start(resourceGroupName, streamAnalyticsJobName, jobStartParameters);

## <a name="stop-a-stream-analytics-job"></a>A Stream Analytics-feladat leállítása
Egy futó Stream Analytics-feladat leállításához hívó hello **leállítása** metódust.

    // Stop a Stream Analytics job
    LongRunningOperationResponse jobStopResponse = client.StreamingJobs.Stop(resourceGroupName, streamAnalyticsJobName);

## <a name="delete-a-stream-analytics-job"></a>A Stream Analytics-feladat törlése
Hello **törlése** metódus hello feladat, valamint az alapul szolgáló alárendelt erőforrások, például az input(s), kimenete és átalakítása hello feladat hello törli.

    // Delete a Stream Analytics job
    LongRunningOperationResponse jobDeleteResponse = client.StreamingJobs.Delete(resourceGroupName, streamAnalyticsJobName);

## <a name="get-support"></a>Támogatás kérése
Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Következő lépések
Hogy megismerte a .NET SDK toocreate használatával hello alapjait és analytics-feladatok futtatásához. toolearn több, tekintse meg a hello következőt:

* [A Stream Analytics bemutatása tooAzure](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)
* [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)
* [Az Azure Stream Analytics felügyeleti .NET SDK](https://msdn.microsoft.com/library/azure/dn889315.aspx).
* [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)
* [Az Azure Stream Analytics felügyeleti REST API referenciája](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[azure.blob.storage]: http://azure.microsoft.com/documentation/services/storage/
[azure.blob.storage.use]: http://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/

[azure.event.hubs]: http://azure.microsoft.com/services/event-hubs/
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.forum]: http://go.microsoft.com/fwlink/?LinkId=512151

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.developer.guide]: stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
