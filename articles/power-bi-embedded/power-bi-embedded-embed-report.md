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
# <a name="embed-a-report-in-power-bi-embedded"></a>A jelentés beágyazása a Power BI Embedded

Megtudhatja, hogyan tooembed egy jelentést, amely a Power BI Embedded az alkalmazásba.

Követően áttekintjük hogyan tooactually jelentés beágyazása az alkalmazásba. Ennek feltétele, hogy már rendelkezik egy jelentést, amely létezik egy munkaterület belül a munkaterület-csoportot. Ha a lépés még nem végzett, lásd: [Ismerkedés a Power BI Embedded](power-bi-embedded-get-started.md).

Hello .NET (C#) vagy a Node.js SDK, valamint JavaScript, tooeasily építenie az alkalmazást a Power BI Embedded. 

## <a name="using-hello-access-keys-toouse-rest-apis"></a>Hello hozzáférési kulcsok toouse REST API-k használatával

A sorrend toocall hello REST API-t átadhatók hello hozzáférési kulcsot, amely letölthető hello Azure-portál a megadott munkaterület-csoport. További információkért lásd: [Ismerkedés a Power BI Embedded](power-bi-embedded-get-started.md).

## <a name="get-a-report-id"></a>A jelentés azonosító beszerzése

Minden hozzáférési jogkivonat jelentés alapul. Szüksége lesz a megadott azonosítója, amelyet az tooembed hello jelentés tooget hello. Ezt megteheti a hívások toohello [jelentések lekérése](https://msdn.microsoft.com/library/azure/mt711510.aspx) REST API-t. Ezzel visszatér hello jelentés azonosítója és hello beágyazása URL-címét. Ezt megteheti hello Power BI .NET SDK használatával, vagy hívja közvetlenül a REST API hello.

### <a name="using-hello-power-bi-net-sdk"></a>Hello Power BI .NET SDK használatával

Hello .NET SDK használatával, meg kell toocreate hello elérési kulcsot kap hello Azure-portálon alapuló token hitelesítő adatokat. Ehhez szükséges, hogy telepítse hello [Power BI API NuGet-csomag](https://www.nuget.org/profiles/powerbi).

**NuGet-csomag telepítése**

```
Install-Package Microsoft.PowerBI.Api
```

**C#-kódban**

```
using Microsoft.PowerBI.Api.V1;
using Microsoft.Rest;

var credentials = new TokenCredentials("{access key}", "AppKey");
var client = new PowerBIClient(credentials);
client.BaseUri = new Uri(https://api.powerbi.com);

var reports = (IList<Report>)client.Reports.GetReports(workspaceCollectionName, workspaceId).Value;

// Select hello report that you want toowork with from hello collection of reports.
```

### <a name="calling-hello-rest-api-directly"></a>REST API hívása-hello közvetlenül

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

## <a name="create-an-access-token"></a>Hozzon létre egy hozzáférési jogkivonatot:

A Power BI Embedded használ beágyazási jogkivonatot, amely HMAC aláírt JSON webes jogkivonatainak. hello jogkivonatok hello hozzáférési kulcsot a Azure Power BI Embedded munkaterület-csoportok vannak aláírva. Beágyazása a jogkivonatokat, alapértelmezés szerint, van olvasási használt tooprovide csak akkor férhessenek hozzá tooa jelentés tooembed egy alkalmazásba. Beágyazása jogkivonatok egy adott jelentés ki, és lehet társítva egy beágyazási URL-CÍMÉT.

Hozzáférési jogkivonatok hello elérési kulcsok vannak használt toosign/titkosítása hello jogkivonatok hello kiszolgálón kell létrehozni. Információ toocreate hozzáférési tokent, lásd: [Authenticating és engedélyezése a Power BI Embedded](power-bi-embedded-app-token-flow.md). Emellett áttekintheti hello [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) metódust. Íme egy példa Mi ez néz hello .NET SDK használatával a Power BI.

Hello azonosítója, amely korábban kapott fogja használni. Miután hello beágyazása token jön létre, akkor a, majd hello hozzáférési kulcs toogenerate hello token hello javascript szempontjából is használhatja. Hello *PowerBIToken osztály* kell telepíteni hello [Power BI Core NuGut csomag](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).

**NuGet-csomag telepítése**

```
Install-Package Microsoft.PowerBI.Core
```

**C#-kódban**

```
using Microsoft.PowerBI.Security;

// rlsUsername, roles and scopes are optional.
embedToken = PowerBIToken.CreateReportEmbedToken(workspaceCollectionName, workspaceId, reportId, rlsUsername, roles, scopes);

var token = embedToken.Generate("{access key}");
```

### <a name="adding-permission-scopes-tooembed-tokens"></a>Engedély hatókörök tooembed tokenek hozzáadása

Beágyazási jogkivonatok használata esetén érdemes lehet toorestrict használatát hello erőforrásokhoz való hozzáférést. Emiatt a hatókörbe tartozó engedélyek jogkivonatot is létrehozhat. További információkért lásd: [hatókörök](power-bi-embedded-app-token-flow.md#scopes)

## <a name="embed-using-javascript"></a>Beágyazása a JavaScript használatával

Miután hello hozzáférési jogkivonat és hello azonosítója, azt beágyazása hello jelentést JavaScript használatával. Ehhez szükséges, hogy telepítse hello nuget [Power BI JavaScript csomag](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/). hello embedUrl https://embedded.powerbi.com/appTokenReportEmbed fog majd.

> [!NOTE]
> Használhatja a hello [JavaScript-jelentés beágyazása minta](https://microsoft.github.io/PowerBI-JavaScript/demo/) tootest funkciót. Emellett a segítségével kódpéldák hello különböző műveletek érhetők el.

**NuGet-csomag telepítése**

```
Install-Package Microsoft.PowerBI.JavaScript
```

**JavaScript-kód**

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

### <a name="set-hello-size-of-embedded-elements"></a>A beágyazott elemek hello mérete

hello jelentés automatikusan beágyazni kívánt tárolója hello mérete alapján. toooverride hello alapértelmezett mérete hello beágyazza egyszerűen adja hozzá a CSS osztály attribútuma vagy beágyazott stílusok szélessége és magassága számára.

## <a name="see-also"></a>Lásd még:

[Bevezetés a minta használatába](power-bi-embedded-get-started-sample.md)  
[Hitelesítés és engedélyezés a Power BI Embedded használatával](power-bi-embedded-app-token-flow.md)  
[CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_)  
[JavaScript beágyazási minta](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[A Power BI JavaScript-csomag](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/)  
[A Power BI API NuGet-csomag](https://www.nuget.org/profiles/powerbi)
[Power BI Core NuGut csomag](https://www.nuget.org/packages/Microsoft.PowerBI.Core/)  
[A csharp nyelvű Power bi Git-tárház](https://github.com/Microsoft/PowerBI-CSharp)  
[Power bi-csomópont Git-tárház](https://github.com/Microsoft/PowerBI-Node)  
További kérdései vannak? [Próbálja meg a Power BI-Közösség hello](http://community.powerbi.com/)
