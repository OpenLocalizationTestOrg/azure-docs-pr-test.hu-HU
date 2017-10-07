---
title: "a jelentés Azure Power BI Embedded aaaEmbed |} Microsoft Docs"
description: "Megtudhatja, hogyan tooembed egy jelentést, amely a Power BI Embedded az alkalmazásba."
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 03/11/2017
ms.author: asaxton
ms.openlocfilehash: f25344bbd0b9c092ef19da04d0b455d453b426a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="embed-a-report-in-power-bi-embedded"></a><span data-ttu-id="e6e89-103">A jelentés beágyazása a Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="e6e89-103">Embed a report in Power BI Embedded</span></span>

<span data-ttu-id="e6e89-104">Megtudhatja, hogyan tooembed egy jelentést, amely a Power BI Embedded az alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="e6e89-104">Learn how tooembed a report that is in Power BI Embedded into your application.</span></span>

<span data-ttu-id="e6e89-105">Követően áttekintjük hogyan tooactually jelentés beágyazása az alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="e6e89-105">We will look at how tooactually embed a report into your application.</span></span> <span data-ttu-id="e6e89-106">Ennek feltétele, hogy már rendelkezik egy jelentést, amely létezik egy munkaterület belül a munkaterület-csoportot.</span><span class="sxs-lookup"><span data-stu-id="e6e89-106">This is assuming you already have a report that exists within a workspace in your workspace collection.</span></span> <span data-ttu-id="e6e89-107">Ha a lépés még nem végzett, lásd: [Ismerkedés a Power BI Embedded](power-bi-embedded-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="e6e89-107">If you haven't done that step yet, see [Get started with Power BI Embedded](power-bi-embedded-get-started.md).</span></span>

<span data-ttu-id="e6e89-108">Hello .NET (C#) vagy a Node.js SDK, valamint JavaScript, tooeasily építenie az alkalmazást a Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="e6e89-108">You can use hello .NET (C#) or Node.js SDK, along with JavaScript, tooeasily build your application with Power BI Embedded.</span></span> 

## <a name="using-hello-access-keys-toouse-rest-apis"></a><span data-ttu-id="e6e89-109">Hello hozzáférési kulcsok toouse REST API-k használatával</span><span class="sxs-lookup"><span data-stu-id="e6e89-109">Using hello access keys toouse REST APIs</span></span>

<span data-ttu-id="e6e89-110">A sorrend toocall hello REST API-t átadhatók hello hozzáférési kulcsot, amely letölthető hello Azure-portál a megadott munkaterület-csoport.</span><span class="sxs-lookup"><span data-stu-id="e6e89-110">In order toocall hello REST API, you can pass hello access key which you can get from hello Azure portal for a given workspace collection.</span></span> <span data-ttu-id="e6e89-111">További információkért lásd: [Ismerkedés a Power BI Embedded](power-bi-embedded-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="e6e89-111">For more information, see [Get started with Power BI Embedded](power-bi-embedded-get-started.md).</span></span>

## <a name="get-a-report-id"></a><span data-ttu-id="e6e89-112">A jelentés azonosító beszerzése</span><span class="sxs-lookup"><span data-stu-id="e6e89-112">Get a report id</span></span>

<span data-ttu-id="e6e89-113">Minden hozzáférési jogkivonat jelentés alapul.</span><span class="sxs-lookup"><span data-stu-id="e6e89-113">Every access token is based on a report.</span></span> <span data-ttu-id="e6e89-114">Szüksége lesz a megadott azonosítója, amelyet az tooembed hello jelentés tooget hello.</span><span class="sxs-lookup"><span data-stu-id="e6e89-114">You will need tooget hello given report id for hello report that you want tooembed.</span></span> <span data-ttu-id="e6e89-115">Ezt megteheti a hívások toohello [jelentések lekérése](https://msdn.microsoft.com/library/azure/mt711510.aspx) REST API-t.</span><span class="sxs-lookup"><span data-stu-id="e6e89-115">This can be done based on calls toohello [Get Reports](https://msdn.microsoft.com/library/azure/mt711510.aspx) REST API.</span></span> <span data-ttu-id="e6e89-116">Ezzel visszatér hello jelentés azonosítója és hello beágyazása URL-címét.</span><span class="sxs-lookup"><span data-stu-id="e6e89-116">This will return hello report id and hello embed url.</span></span> <span data-ttu-id="e6e89-117">Ezt megteheti hello Power BI .NET SDK használatával, vagy hívja közvetlenül a REST API hello.</span><span class="sxs-lookup"><span data-stu-id="e6e89-117">This can be done using hello Power BI .NET SDK or calling hello REST API directly.</span></span>

### <a name="using-hello-power-bi-net-sdk"></a><span data-ttu-id="e6e89-118">Hello Power BI .NET SDK használatával</span><span class="sxs-lookup"><span data-stu-id="e6e89-118">Using hello Power BI .NET SDK</span></span>

<span data-ttu-id="e6e89-119">Hello .NET SDK használatával, meg kell toocreate hello elérési kulcsot kap hello Azure-portálon alapuló token hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="e6e89-119">When using hello .NET SDK, you will need toocreate a token credential which is based on hello access key you get from hello Azure portal.</span></span> <span data-ttu-id="e6e89-120">Ehhez szükséges, hogy telepítse hello [Power BI API NuGet-csomag](https://www.nuget.org/profiles/powerbi).</span><span class="sxs-lookup"><span data-stu-id="e6e89-120">This requires that you install hello [Power BI API NuGet package](https://www.nuget.org/profiles/powerbi).</span></span>

<span data-ttu-id="e6e89-121">**NuGet-csomag telepítése**</span><span class="sxs-lookup"><span data-stu-id="e6e89-121">**NuGet package install**</span></span>

```
Install-Package Microsoft.PowerBI.Api
```

<span data-ttu-id="e6e89-122">**C#-kódban**</span><span class="sxs-lookup"><span data-stu-id="e6e89-122">**C# code**</span></span>

```
using Microsoft.PowerBI.Api.V1;
using Microsoft.Rest;

var credentials = new TokenCredentials("{access key}", "AppKey");
var client = new PowerBIClient(credentials);
client.BaseUri = new Uri(https://api.powerbi.com);

var reports = (IList<Report>)client.Reports.GetReports(workspaceCollectionName, workspaceId).Value;

// Select hello report that you want toowork with from hello collection of reports.
```

### <a name="calling-hello-rest-api-directly"></a><span data-ttu-id="e6e89-123">REST API hívása-hello közvetlenül</span><span class="sxs-lookup"><span data-stu-id="e6e89-123">Calling hello REST API directly</span></span>

```
System.Net.WebRequest request = System.Net.WebRequest.Create("https://api.powerbi.com/v1.0/collections/{collectionName}/workspaces/{workspaceId}/Reports") as System.Net.HttpWebRequest;

request.Method = "GET";
request.ContentLength = 0;
request.Headers.Add("Authorization", String.Format("AppKey {0}", accessToken.Value));

using (var response = request.GetResponse() as System.Net.HttpWebResponse)
{
    using (var reader = new System.IO.StreamReader(response.GetResponseStream()))
    {
        // Work with JSON response tooget hello report you want toowork with.
    }

}
```

## <a name="create-an-access-token"></a><span data-ttu-id="e6e89-124">Hozzon létre egy hozzáférési jogkivonatot:</span><span class="sxs-lookup"><span data-stu-id="e6e89-124">Create an access token</span></span>

<span data-ttu-id="e6e89-125">A Power BI Embedded használ beágyazási jogkivonatot, amely HMAC aláírt JSON webes jogkivonatainak.</span><span class="sxs-lookup"><span data-stu-id="e6e89-125">Power BI Embedded uses embed token, which are HMAC signed JSON Web Tokens.</span></span> <span data-ttu-id="e6e89-126">hello jogkivonatok hello hozzáférési kulcsot a Azure Power BI Embedded munkaterület-csoportok vannak aláírva.</span><span class="sxs-lookup"><span data-stu-id="e6e89-126">hello tokens are signed with hello access key from your Azure Power BI Embedded workspace collection.</span></span> <span data-ttu-id="e6e89-127">Beágyazása a jogkivonatokat, alapértelmezés szerint, van olvasási használt tooprovide csak akkor férhessenek hozzá tooa jelentés tooembed egy alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="e6e89-127">Embed tokens, by default, are used tooprovide read only access tooa report tooembed into an application.</span></span> <span data-ttu-id="e6e89-128">Beágyazása jogkivonatok egy adott jelentés ki, és lehet társítva egy beágyazási URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="e6e89-128">Embed tokens are issued for a specific report and should be associated with an embed URL.</span></span>

<span data-ttu-id="e6e89-129">Hozzáférési jogkivonatok hello elérési kulcsok vannak használt toosign/titkosítása hello jogkivonatok hello kiszolgálón kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="e6e89-129">Access tokens should be created on hello server as hello access keys are used toosign/encrypt hello tokens.</span></span> <span data-ttu-id="e6e89-130">Információ toocreate hozzáférési tokent, lásd: [Authenticating és engedélyezése a Power BI Embedded](power-bi-embedded-app-token-flow.md).</span><span class="sxs-lookup"><span data-stu-id="e6e89-130">For information on how toocreate an access token, see [Authenticating and authorizing with Power BI Embedded](power-bi-embedded-app-token-flow.md).</span></span> <span data-ttu-id="e6e89-131">Emellett áttekintheti hello [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) metódust.</span><span class="sxs-lookup"><span data-stu-id="e6e89-131">You can also review hello [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) method.</span></span> <span data-ttu-id="e6e89-132">Íme egy példa Mi ez néz hello .NET SDK használatával a Power BI.</span><span class="sxs-lookup"><span data-stu-id="e6e89-132">Here is an example of what this would look like using hello .NET SDK for Power BI.</span></span>

<span data-ttu-id="e6e89-133">Hello azonosítója, amely korábban kapott fogja használni.</span><span class="sxs-lookup"><span data-stu-id="e6e89-133">You will use hello report id that you retrieved earlier.</span></span> <span data-ttu-id="e6e89-134">Miután hello beágyazása token jön létre, akkor a, majd hello hozzáférési kulcs toogenerate hello token hello javascript szempontjából is használhatja.</span><span class="sxs-lookup"><span data-stu-id="e6e89-134">Once hello embed token is created, you will then use hello access key toogenerate hello token which you can use from hello javascript perspective.</span></span> <span data-ttu-id="e6e89-135">Hello *PowerBIToken osztály* kell telepíteni hello [Power BI Core NuGut csomag](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).</span><span class="sxs-lookup"><span data-stu-id="e6e89-135">hello *PowerBIToken class* requires that you install hello [Power BI Core NuGut Package](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).</span></span>

<span data-ttu-id="e6e89-136">**NuGet-csomag telepítése**</span><span class="sxs-lookup"><span data-stu-id="e6e89-136">**NuGet package install**</span></span>

```
Install-Package Microsoft.PowerBI.Core
```

<span data-ttu-id="e6e89-137">**C#-kódban**</span><span class="sxs-lookup"><span data-stu-id="e6e89-137">**C# code**</span></span>

```
using Microsoft.PowerBI.Security;

// rlsUsername, roles and scopes are optional.
embedToken = PowerBIToken.CreateReportEmbedToken(workspaceCollectionName, workspaceId, reportId, rlsUsername, roles, scopes);

var token = embedToken.Generate("{access key}");
```

### <a name="adding-permission-scopes-tooembed-tokens"></a><span data-ttu-id="e6e89-138">Engedély hatókörök tooembed tokenek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e6e89-138">Adding permission scopes tooembed tokens</span></span>

<span data-ttu-id="e6e89-139">Beágyazási jogkivonatok használata esetén érdemes lehet toorestrict használatát hello erőforrásokhoz való hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="e6e89-139">When using Embed tokens, you may want toorestrict usage of hello resources you give access to.</span></span> <span data-ttu-id="e6e89-140">Emiatt a hatókörbe tartozó engedélyek jogkivonatot is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="e6e89-140">For this reason, you can generate a token with scoped permissions.</span></span> <span data-ttu-id="e6e89-141">További információkért lásd: [hatókörök](power-bi-embedded-app-token-flow.md#scopes)</span><span class="sxs-lookup"><span data-stu-id="e6e89-141">For more information, see [Scopes](power-bi-embedded-app-token-flow.md#scopes)</span></span>

## <a name="embed-using-javascript"></a><span data-ttu-id="e6e89-142">Beágyazása a JavaScript használatával</span><span class="sxs-lookup"><span data-stu-id="e6e89-142">Embed using JavaScript</span></span>

<span data-ttu-id="e6e89-143">Miután hello hozzáférési jogkivonat és hello azonosítója, azt beágyazása hello jelentést JavaScript használatával.</span><span class="sxs-lookup"><span data-stu-id="e6e89-143">After you have hello access token and hello report id, we can embed hello report using JavaScript.</span></span> <span data-ttu-id="e6e89-144">Ehhez szükséges, hogy telepítse hello nuget [Power BI JavaScript csomag](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/).</span><span class="sxs-lookup"><span data-stu-id="e6e89-144">This requires that you install hello nuget [Power BI JavaScript package](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/).</span></span> <span data-ttu-id="e6e89-145">hello embedUrl https://embedded.powerbi.com/appTokenReportEmbed fog majd.</span><span class="sxs-lookup"><span data-stu-id="e6e89-145">hello embedUrl will just be https://embedded.powerbi.com/appTokenReportEmbed.</span></span>

> [!NOTE]
> <span data-ttu-id="e6e89-146">Használhatja a hello [JavaScript-jelentés beágyazása minta](https://microsoft.github.io/PowerBI-JavaScript/demo/) tootest funkciót.</span><span class="sxs-lookup"><span data-stu-id="e6e89-146">You can use hello [JavaScript Report Embed Sample](https://microsoft.github.io/PowerBI-JavaScript/demo/) tootest functionality.</span></span> <span data-ttu-id="e6e89-147">Emellett a segítségével kódpéldák hello különböző műveletek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="e6e89-147">It also gives code examples for hello different operations that are available.</span></span>

<span data-ttu-id="e6e89-148">**NuGet-csomag telepítése**</span><span class="sxs-lookup"><span data-stu-id="e6e89-148">**NuGet package install**</span></span>

```
Install-Package Microsoft.PowerBI.JavaScript
```

<span data-ttu-id="e6e89-149">**JavaScript-kód**</span><span class="sxs-lookup"><span data-stu-id="e6e89-149">**JavaScript code**</span></span>

```
<script src="/scripts/powerbi.js"></script>
<div id="reportContainer"></div>

var embedConfiguration = {
    type: 'report',
    accessToken: 'eyJ0eXAiO...Qron7qYpY9MI',
    id: '5dac7a4a-4452-46b3-99f6-a25915e0fe55',
    embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed'
};

var $reportContainer = $('#reportContainer');
var report = powerbi.embed($reportContainer.get(0), embedConfiguration);
```

### <a name="set-hello-size-of-embedded-elements"></a><span data-ttu-id="e6e89-150">A beágyazott elemek hello mérete</span><span class="sxs-lookup"><span data-stu-id="e6e89-150">Set hello size of embedded elements</span></span>

<span data-ttu-id="e6e89-151">hello jelentés automatikusan beágyazni kívánt tárolója hello mérete alapján.</span><span class="sxs-lookup"><span data-stu-id="e6e89-151">hello report will automatically be embedded based on hello size of its container.</span></span> <span data-ttu-id="e6e89-152">toooverride hello alapértelmezett mérete hello beágyazza egyszerűen adja hozzá a CSS osztály attribútuma vagy beágyazott stílusok szélessége és magassága számára.</span><span class="sxs-lookup"><span data-stu-id="e6e89-152">toooverride hello default size of hello embeds simply add a CSS class attribute or inline styles for width & height.</span></span>

## <a name="see-also"></a><span data-ttu-id="e6e89-153">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="e6e89-153">See also</span></span>

[<span data-ttu-id="e6e89-154">Bevezetés a minta használatába</span><span class="sxs-lookup"><span data-stu-id="e6e89-154">Get started with sample</span></span>](power-bi-embedded-get-started-sample.md)  
[<span data-ttu-id="e6e89-155">Hitelesítés és engedélyezés a Power BI Embedded használatával</span><span class="sxs-lookup"><span data-stu-id="e6e89-155">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="e6e89-156">CreateReportEmbedToken</span><span class="sxs-lookup"><span data-stu-id="e6e89-156">CreateReportEmbedToken</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_)  
[<span data-ttu-id="e6e89-157">JavaScript beágyazási minta</span><span class="sxs-lookup"><span data-stu-id="e6e89-157">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[<span data-ttu-id="e6e89-158">A Power BI JavaScript-csomag</span><span class="sxs-lookup"><span data-stu-id="e6e89-158">Power BI JavaScript package</span></span>](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/)  
<span data-ttu-id="e6e89-159">[A Power BI API NuGet-csomag](https://www.nuget.org/profiles/powerbi)
[Power BI Core NuGut csomag](https://www.nuget.org/packages/Microsoft.PowerBI.Core/)</span><span class="sxs-lookup"><span data-stu-id="e6e89-159">[Power BI API NuGet package](https://www.nuget.org/profiles/powerbi)
[Power BI Core NuGut Package](https://www.nuget.org/packages/Microsoft.PowerBI.Core/)</span></span>  
[<span data-ttu-id="e6e89-160">A csharp nyelvű Power bi Git-tárház</span><span class="sxs-lookup"><span data-stu-id="e6e89-160">PowerBI-CSharp Git Repo</span></span>](https://github.com/Microsoft/PowerBI-CSharp)  
[<span data-ttu-id="e6e89-161">Power bi-csomópont Git-tárház</span><span class="sxs-lookup"><span data-stu-id="e6e89-161">PowerBI-Node Git Repo</span></span>](https://github.com/Microsoft/PowerBI-Node)  
<span data-ttu-id="e6e89-162">További kérdései vannak?</span><span class="sxs-lookup"><span data-stu-id="e6e89-162">More questions?</span></span> [<span data-ttu-id="e6e89-163">Próbálja meg a Power BI-Közösség hello</span><span class="sxs-lookup"><span data-stu-id="e6e89-163">Try hello Power BI Community</span></span>](http://community.powerbi.com/)
