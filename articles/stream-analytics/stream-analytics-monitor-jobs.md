---
title: "a Stream Analytics aaaProgrammatically figyelő feladatok |} Microsoft Docs"
description: "Ismerje meg, hogyan figyelheti az tooprogrammatically a REST API-k, az Azure SDK-t vagy a PowerShell segítségével létrehozott Stream Analytics-feladatok."
keywords: ".NET-figyelő, feladat figyelője alkalmazás figyelése"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 2ec02cc9-4ca5-4a25-ae60-c44be9ad4835
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeffstok
ms.openlocfilehash: 44a9c29c2161ee81ea76ece4646a8691bf5d5b48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="programmatically-create-a-stream-analytics-job-monitor"></a>A Stream Analytics-feladat figyelő létrehozása programmal.

Ez a cikk bemutatja, hogyan tooenable figyelési Stream Analytics-feladat. REST API-k, az Azure SDK-t vagy a PowerShell létrehozott Stream Analytics-feladatok nincs figyelés alapértelmezés szerint engedélyezett. Manuálisan engedélyezheti azt az Azure-portálon hello toohello feladat figyelő lap megnyitásával és gombra kattintva hello engedélyezése vagy hello cikkben leírt lépéseket követve automatizálhatja is a folyamatot. figyelési adatok hello hello Azure-portál a Stream Analytics-feladat a hello metrikák területén fog megjelenni.

## <a name="prerequisites"></a>Előfeltételek

Ez a folyamat megkezdése előtt hello következő kell rendelkeznie:

* A Visual Studio 2017 vagy 2015
* [Az Azure .NET SDK](https://azure.microsoft.com/downloads/) letöltése és telepítése
* Egy meglévő Stream Analytics-feladat, amelyet a toohave figyelő engedélyezve

## <a name="create-a-project"></a>A projekt létrehozása

1. Hozzon létre egy Visual Studio C# .NET konzolalkalmazást.
2. Hello Csomagkezelő konzol, a következő futtatási hello parancsokat tooinstall hello NuGet-csomagok. hello először egyik hello Azure Stream Analytics felügyeleti .NET SDK-t. hello második hello Azure figyelő SDK használandó tooenable figyelését. hello utolsó egyik hello Azure Active Directory-ügyfél-hitelesítéshez használandó.
   
   ```
   Install-Package Microsoft.Azure.Management.StreamAnalytics
   Install-Package Microsoft.Azure.Insights -Pre
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   ```
3. Adja hozzá a következő appSettings szakaszt toohello App.config fájl hello.
   
   ```
   <appSettings>
     <!--CSM Prod related values-->
     <add key="ResourceGroupName" value="RESOURCE GROUP NAME" />
     <add key="JobName" value="YOUR JOB NAME" />
     <add key="StorageAccountName" value="YOUR STORAGE ACCOUNT"/>
     <add key="ActiveDirectoryEndpoint" value="https://login.microsoftonline.com/" />
     <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
     <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
     <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
     <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
     <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION ID" />
     <add key="ActiveDirectoryTenantId" value="YOUR TENANT ID" />
   </appSettings>
   ```
   Cserélje le az értékeket *SubscriptionId* és *ActiveDirectoryTenantId* a Azure-előfizetés és a bérlői azonosítók. Hello a következő PowerShell-parancsmag futtatásával kaphatunk ezekkel az értékekkel:
   
   ```
   Get-AzureAccount
   ```
4. Adja hozzá a következő hello segítségével utasítások toohello forrásfájl (Program.cs) hello projektben.
   
   ```
     using System;
     using System.Configuration;
     using System.Threading;
     using Microsoft.Azure;
     using Microsoft.Azure.Management.Insights;
     using Microsoft.Azure.Management.Insights.Models;
     using Microsoft.Azure.Management.StreamAnalytics;
     using Microsoft.Azure.Management.StreamAnalytics.Models;
     using Microsoft.IdentityModel.Clients.ActiveDirectory;
   ```
5. Adja hozzá a segítő hitelesítési módszert.
   
     nyilvános, statikus karakterlánc GetAuthorizationHeader()
   
         {
             AuthenticationResult result = null;
             var thread = new Thread(() =>
             {
                 try
                 {
                     var context = new AuthenticationContext(
                         ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] +
                         ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);
   
                     result = context.AcquireToken(
                         resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
                         clientId: ConfigurationManager.AppSettings["AsaClientId"],
                         redirectUri: new Uri(ConfigurationManager.AppSettings["RedirectUri"]),
                         promptBehavior: PromptBehavior.Always);
                 }
                 catch (Exception threadEx)
                 {
                     Console.WriteLine(threadEx.Message);
                 }
             });
   
             thread.SetApartmentState(ApartmentState.STA);
             thread.Name = "AcquireTokenThread";
             thread.Start();
             thread.Join();
   
             if (result != null)
             {
                 return result.AccessToken;
             }
   
             throw new InvalidOperationException("Failed tooacquire token");
     }

## <a name="create-management-clients"></a>Hozzon létre felügyeleti ügyfelek

hello alábbira beállításához hello szükséges változók és a felügyeleti ügyfeleket.

    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";

    // Get authentication token
    TokenCloudCredentials aadTokenCredentials =
        new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader());

    Uri resourceManagerUri = new
    Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    // Create Stream Analytics and Insights management client
    StreamAnalyticsManagementClient streamAnalyticsClient = new
    StreamAnalyticsManagementClient(aadTokenCredentials, resourceManagerUri);
    InsightsManagementClient insightsClient = new
    InsightsManagementClient(aadTokenCredentials, resourceManagerUri);

## <a name="enable-monitoring-for-an-existing-stream-analytics-job"></a>Egy meglévő Stream Analytics-feladat figyelésének engedélyezése

hello következő kódot a figyelő lehetővé teszi, hogy egy **meglévő** Stream Analytics-feladat. hello hello kód első része egy GET kérelmet hello Stream Analytics szolgáltatás tooretrieve információ hello adott Stream Analytics-feladatot hajt végre. Hello használ *azonosító* hello Put metódust a hello paramétereként (beolvasva a hello GET kérés) tulajdonság második felében hello kód, amely elküld egy PUT-kérelmet toohello Insights szolgáltatás tooenable a Stream Analytics hello megfigyelése a feladat.

>[!WARNING]
>Ha már engedélyezte különböző Stream Analytics-feladat, hello Azure-portálon vagy programozottan keresztül az alábbi kód, hello figyelési **azt javasoljuk, hogy megadja a hello használt ugyanazon tárfiók neve, korábban már engedélyezte a figyelést.**
> 
> hello tárfiók csatolt toohello régióban létrehozott a Stream Analytics-feladat, nem kifejezetten maga toohello feladat kerül.
> 
> Az összes Stream Analytics feladatok (és minden más Azure-erőforrások) ugyanabban a régióban ugyanazt a tárolási fiók toostore figyelési adatok. Egy másik tárolási fiókot ad meg, ha nem kívánt hatásai megakadályozhatja a hello a más Stream Analytics-feladatok vagy más Azure-erőforrások figyelése.
> 
> hello tárfióknév tooreplace használt `<YOUR STORAGE ACCOUNT NAME>` hello a következő kódot kell egy tárfiókot, amely hello hello Stream Analytics-feladat, amely engedélyezi a figyelést tárolóként ugyanazt az előfizetést.
> 
> 

    // Get an existing Stream Analytics job
    JobGetParameters jobGetParameters = new JobGetParameters()
    {
        PropertiesToExpand = "inputs,transformation,outputs"
    };
    JobGetResponse jobGetResponse = streamAnalyticsClient.StreamingJobs.Get(resourceGroupName, streamAnalyticsJobName, jobGetParameters);

    // Enable monitoring
    ServiceDiagnosticSettingsPutParameters insightPutParameters = new ServiceDiagnosticSettingsPutParameters()
    {
            Properties = new ServiceDiagnosticSettings()
            {
                StorageAccountName = "<YOUR STORAGE ACCOUNT NAME>"
            }
    };
    insightsClient.ServiceDiagnosticSettingsOperations.Put(jobGetResponse.Job.Id, insightPutParameters);



## <a name="get-support"></a>Támogatás kérése

Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Következő lépések

* [A Stream Analytics bemutatása tooAzure](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)
* [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)
* [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)
* [Az Azure Stream Analytics felügyeleti REST API referenciája](https://msdn.microsoft.com/library/azure/dn835031.aspx)

