---
title: "Referenciaadatok aaaManage Azure idő adatsorozat Insights környezet C# használatával |} Microsoft Docs"
description: "Ez az oktatóanyag ismerteti, hogyan toomanage referenciaadatok Azure idő adatsorozat Insights környezet C# használatával"
keywords: 
services: time-series-insights
documentationcenter: 
author: venkatgct
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: venkatja
ms.openlocfilehash: 77b85aa7f9a5dc46c132afa56c82df48f41577fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-reference-data-for-an-azure-time-series-insights-environment-by-using-c"></a><span data-ttu-id="3b9e2-103">C# használatával referenciaadatok Azure idő adatsorozat Insights környezet kezelése</span><span class="sxs-lookup"><span data-stu-id="3b9e2-103">Manage reference data for an Azure Time Series Insights environment by using C#</span></span>

<span data-ttu-id="3b9e2-104">Az alábbi C# példa bemutatja, hogyan toomanage Azure idő adatsorozat Insights környezet adatokra hivatkoztak.</span><span class="sxs-lookup"><span data-stu-id="3b9e2-104">This C# sample demonstrates how toomanage reference data for an Azure Time Series Insights environment.</span></span>
<span data-ttu-id="3b9e2-105">Futó hello minta előtt győződjön meg arról, hello lépések elvégzése.</span><span class="sxs-lookup"><span data-stu-id="3b9e2-105">Before running hello sample, ensure hello following steps are completed.</span></span>
1. <span data-ttu-id="3b9e2-106">A referencia-adatkészlet használatával lett létrehozva [Ez a cikk](time-series-insights-add-reference-data-set.md).</span><span class="sxs-lookup"><span data-stu-id="3b9e2-106">A reference data set has been created using [this article](time-series-insights-add-reference-data-set.md).</span></span>
2. <span data-ttu-id="3b9e2-107">hello hozzáférési jogkivonat hello alkalmazást futtató rendszer beszerzett hello Azure Active Directory API használatos.</span><span class="sxs-lookup"><span data-stu-id="3b9e2-107">hello access token used when running hello application is acquired through hello Azure Active Directory API.</span></span> <span data-ttu-id="3b9e2-108">Ez a token kell átadni hello `Authorization` minden lekérdezés API-kérelem fejlécében.</span><span class="sxs-lookup"><span data-stu-id="3b9e2-108">This token should be passed in hello `Authorization` header of every Query API request.</span></span> <span data-ttu-id="3b9e2-109">Nem interaktív alkalmazások beállítása, lásd: hello [hitelesítési és engedélyezési](time-series-insights-authentication-and-authorization.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="3b9e2-109">For setting up non-interactive applications, see hello [Authentication and authorization](time-series-insights-authentication-and-authorization.md) article.</span></span>
3. <span data-ttu-id="3b9e2-110">Minden hello konstans definiálva hello minta hello elején megfelelően van beállítva.</span><span class="sxs-lookup"><span data-stu-id="3b9e2-110">All hello constants defined at hello beginning of hello sample are correctly set.</span></span>

## <a name="c-sample"></a><span data-ttu-id="3b9e2-111">C#-minta</span><span class="sxs-lookup"><span data-stu-id="3b9e2-111">C# sample</span></span>

```csharp
// Copyright (c) Microsoft Corporation.  All rights reserved.

using System;
using System.IO;
using System.Net;
using System.Threading.Tasks;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

namespace TimeSeriesInsightsReferenceDataSampleApp
{
    public static class Program
    {
        // SET hello environment fqdn.
        private static string EnvironmentFqdn = "#DUMMY#.env.timeseries.azure.com";

        // SET hello environment reference data set name used when creating it.
        private static string EnvironmentReferenceDataSetName = "#DUMMY#";

        // For automated execution under application identity,
        // use application created in Active Directory.
        // toocreate hello application in AAD, follow hello steps provided here:
        // https://docs.microsoft.com/en-us/azure/time-series-insights/time-series-insights-authentication-and-authorization

        // SET hello application ID of application registered in your Azure Active Directory
        private static string ApplicationClientId = "#DUMMY#";

        // SET hello application key of hello application registered in your Azure Active Directory
        private static string ApplicationClientSecret = "#DUMMY#";

        // SET hello Azure Active Directory tenant.
        private static string Tenant = "#DUMMY#.onmicrosoft.com";

        private static async Task DemoReferenceDataAsync()
        {
            // 1. Acquire an access token.
            string accessToken = await AcquireAccessTokenAsync();

            // 2. Put a new reference data item.
            {
                HttpWebRequest request = CreateWebRequest(accessToken);
                string input = @"
{
    ""put"": [{
        ""DeviceId"": ""Fan1"",
        ""Type"": ""Fan"",
        ""BladeCount"": ""3""
    }]
}";
                await SendRequestAsync(request, input);
                string output = await GetResponseAsync(request);
                PrintInputOutput(action: "Put", input: input, output: output);
            }

            // 3. Get reference data item.
            {
                HttpWebRequest request = CreateWebRequest(accessToken);
                string input = @"
{
    ""get"": [{
        ""DeviceId"": ""Fan1""
    }]
}";
                await SendRequestAsync(request, input);
                string output = await GetResponseAsync(request);
                PrintInputOutput(action: "Get", input: input, output: output);
            }

            // 4. Patch an existing reference data item.
            // Update BladeCount and add Colour.
            {
                HttpWebRequest request = CreateWebRequest(accessToken);
                string input = @"
{
    ""patch"": [{
        ""DeviceId"": ""Fan1"",
        ""BladeCount"": ""4"",
        ""Colour"": ""Brown""
    }]
}";
                await SendRequestAsync(request, input);
                string output = await GetResponseAsync(request);
                PrintInputOutput(action: "Patch", input: input, output: output);
            }

            // 5. Delete a property from an existing reference data item.
            {
                HttpWebRequest request = CreateWebRequest(accessToken);
                string input = @"
{
    ""deleteproperties"": [{
        ""key"": {
            ""DeviceId"": ""Fan1""
        },
        ""properties"": [""BladeCount""]
    }]
}";
                await SendRequestAsync(request, input);
                string output = await GetResponseAsync(request);
                PrintInputOutput(action: "DeleteProperties", input: input, output: output);
            }

            // 6. Delete an existing reference data item.
            {
                HttpWebRequest request = CreateWebRequest(accessToken);
                string input = @"
{
    ""delete"": [{
        ""DeviceId"": ""Fan1""
    }]
}";
                await SendRequestAsync(request, input);
                string output = await GetResponseAsync(request);
                PrintInputOutput(action: "Delete", input: input, output: output);
            }
        }

        private static async Task<string> AcquireAccessTokenAsync()
        {
            if (ApplicationClientId == "#DUMMY#" || ApplicationClientSecret == "#DUMMY#" || Tenant.StartsWith("#DUMMY#"))
            {
                throw new Exception(
                    $"Use hello link {"https://docs.microsoft.com/en-us/azure/time-series-insights/time-series-insights-authentication-and-authorization"} tooupdate hello values of 'ApplicationClientId', 'ApplicationClientSecret' and 'Tenant'.");
            }

            var authenticationContext = new AuthenticationContext(
                $"https://login.windows.net/{Tenant}",
                TokenCache.DefaultShared);

            AuthenticationResult token = await authenticationContext.AcquireTokenAsync(
                resource: "https://api.timeseries.azure.com/",
                clientCredential: new ClientCredential(
                    clientId: ApplicationClientId,
                    clientSecret: ApplicationClientSecret));

            // Show interactive logon dialog tooacquire token on behalf of hello user.
            // Suitable for native apps, and not on server-side of a web application.
            //AuthenticationResult token = await authenticationContext.AcquireTokenAsync(
            //    resource: "https://api.timeseries.azure.com/",
            //    // Set well-known client ID for Azure PowerShell
            //    clientId: "1950a258-227b-4e31-a9cf-717495945fc2",
            //    // Set redirect URI for Azure PowerShell
            //    redirectUri: new Uri("urn:ietf:wg:oauth:2.0:oob"),
            //    parameters: new PlatformParameters(PromptBehavior.Auto));

            return token.AccessToken;
        }

        private static HttpWebRequest CreateWebRequest(string accessToken)
        {
            Uri uri = new UriBuilder("https", EnvironmentFqdn)
            {
                Path = $"referencedatasets/{EnvironmentReferenceDataSetName}/$batch",
                Query = "api-version=2016-12-12"
            }.Uri;
            HttpWebRequest request = WebRequest.CreateHttp(uri);
            request.Method = "POST";
            request.Headers.Add("x-ms-client-application-name", "TimeSeriesInsightsReferenceDataSample");
            request.Headers.Add("Authorization", "Bearer " + accessToken);
            return request;
        }

        private static async Task SendRequestAsync(HttpWebRequest request, string json)
        {
            using (var stream = await request.GetRequestStreamAsync())
            using (var streamWriter = new StreamWriter(stream))
            {
                await streamWriter.WriteAsync(json);
                await streamWriter.FlushAsync();
                streamWriter.Close();
            }
        }

        private static async Task<string> GetResponseAsync(HttpWebRequest request)
        {
            using (WebResponse webResponse = await request.GetResponseAsync())
            using (var sr = new StreamReader(webResponse.GetResponseStream()))
            {
                string result = await sr.ReadToEndAsync();
                return result;
            }
        }

        private static void PrintInputOutput(string action, string input, string output)
        {
            Console.WriteLine("=== {0} request ===", action);
            Console.WriteLine("Input: {0}", JsonConvert.SerializeObject(JsonConvert.DeserializeObject<JObject>(input), Formatting.Indented));
            Console.WriteLine("Output: {0}", JsonConvert.SerializeObject(JsonConvert.DeserializeObject<JObject>(output), Formatting.Indented));
            Console.WriteLine();
        }

        static void Main(string[] args)
        {
            Task.Run(async () => await DemoReferenceDataAsync()).Wait();
        }
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="3b9e2-112">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3b9e2-112">Next steps</span></span>

<span data-ttu-id="3b9e2-113">Hello teljes API referenciáért lásd: [referencia az API](/rest/api/time-series-insights/time-series-insights-reference-reference-data-api) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="3b9e2-113">For hello complete API reference, see [Reference Data API](/rest/api/time-series-insights/time-series-insights-reference-reference-data-api) document.</span></span>
