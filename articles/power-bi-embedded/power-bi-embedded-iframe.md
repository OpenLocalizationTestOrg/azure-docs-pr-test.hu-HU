---
title: "a Power BI Embedded többi aaaHow toouse |} Microsoft Docs"
description: "Ismerje meg, hogy a Power BI Embedded többi toouse "
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 8bcef780-cca0-4f30-9a9b-9daa1a7ae865
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 02/06/2017
ms.author: asaxton
ms.openlocfilehash: 98057724e60ba868f9c93de8c50383569eb8852d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-power-bi-embedded-with-rest"></a>Hogyan Power BI Embedded többi toouse

## <a name="power-bi-embedded-what-it-is-and-what-its-for"></a>A Power BI Embedded: Mi az, és mit

A Power BI Embedded áttekintését hello hivatalos ismertetett [Power BI Embedded hely](https://azure.microsoft.com/services/power-bi-embedded/), de Ismerkedjen meg az gyors, mielőtt azt bejutni hello adatait használja azt a többi.

A használata rendkívül egyszerű, valóban. érdemes lehet toouse hello dinamikus adatok vizuális [Power BI](https://powerbi.microsoft.com) a saját alkalmazásban.

A legtöbb egyéni alkalmazások számára, a saját, nem feltétlenül a saját szervezetében lévő felhasználók toodeliver hello adatokra van szükségük. Például ha a vállalat és a B vállalat néhány szolgáltatás rendelkezik, A vállalatnál lévő felhasználóknál kell csak adatait jeleníti meg a saját vállalati azonosítójához. Ez azt jelenti, hogy több-bérlős hello hello kézbesítési van szükség.

hello egyéni alkalmazás is lehet, hogy ajánlat saját hitelesítési módszerek, például űrlapalapú hitelesítés, alapszintű hitelesítés, stb. Ezt követően megoldás beágyazás hello kell vállalatokkal működnek együtt a meglévő hitelesítési módszerek biztonságosan. A felhasználók toobe képes toouse szükséges nélkül ISV alkalmazásokon hello extra beszerzési vagy a Power BI előfizetés licencelésének.

 **A Power BI Embedded** pontosan ilyen típusú forgatókönyvek tervezték. Igen most, hogy a gyors bevezetés hello útból, folytassuk az egyes részletei

Használhatja a hello .NET \(C#) vagy a Node.js SDK, tooeasily építenie az alkalmazást a Power BI Embedded. Azonban ez a cikk azt ismertetjük, HTTP-folyamat kapcsolatos \(AuthN együtt) a Power BI SDK-k nélkül. Ez a folyamat ismertetése, az alkalmazás hozhat létre **bármely programozási nyelv**, és értelmezni lehet mélyen Power BI Embedded hello lényege.

## <a name="create-power-bi-workspace-collection-and-get-access-key-provisioning"></a>Munkaterület-csoport létrehozása a Power bi-ban, és a get elérési kulcsot \(kiépítés)

A Power BI Embedded egyike hello Azure-szolgáltatásokhoz. Csak hello Azure Portal használó ISV van szó, a használati díjak \(óránkénti felhasználói munkamenetenként), és a nézetek hello jelentés nem megterhelni vagy akár hello felhasználó Azure-előfizetés szükséges.
Az alkalmazásfejlesztés megkezdése előtt létre kell hoznia azt hello **Power BI-munkaterületcsoport** Azure portál használatával.

A Power BI Embedded egyes munkaterületeken hello munkaterület minden ügyfél (bérlői), és sok munkaterületek azt adhat hozzá minden egyes munkaterület-csoportot. hello azonos elérési kulcsot minden munkaterület-csoport szerepel. A hatás, hello munkaterület-csoport a Power BI Embedded hello biztonsági határokat.

![](media/power-bi-embedded-iframe/create-workspace.png)

Hello munkaterület-csoport létrehozása befejeződik, ha hello hozzáférési kulcs másolása az Azure portálról.

![](media/power-bi-embedded-iframe/copy-access-key.png)

> [!NOTE]
> Azt is kiépítése hello munkaterület-csoportot, és a REST API-n keresztül hozzáférést kulcs beszerzése. több, lásd: toolearn [Power BI erőforrás szolgáltató API-k](https://msdn.microsoft.com/library/azure/mt712306.aspx).

## <a name="create-pbix-file-with-power-bi-desktop"></a>A Power BI Desktop .pbix-fájlok létrehozása

Ezután azt kell létrehoznia hello adatkapcsolat és a beágyazott jelentések toobe.
Ez a feladat nincs programozási vagy kódot. A Microsoft Power BI Desktop használja.
Ez a cikk azt nem kerül a hello részletes tájékoztatást a Power BI Desktop toouse. Néhány segítséget itt, lásd: [első lépések a Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/). A példa kedvéért most használjuk hello [kiskereskedelmi elemzési minta](https://powerbi.microsoft.com/documentation/powerbi-sample-datasets/).

![](media/power-bi-embedded-iframe/power-bi-desktop-1.png)

## <a name="create-a-power-bi-workspace"></a>A Power BI-munkaterület létrehozása

Most, hogy hello kiépítés összes történik, lássunk neki egy ügyfél-munkaterület létrehozása a munkaterület-csoportok hello REST API-kon keresztül. hello alábbi HTTP POST kérelem (REST) van hello új munkaterület létrehozása a meglévő munkaterület gyűjteményben. Ez a hello [POST munkaterület API](https://msdn.microsoft.com/library/azure/mt711503.aspx). A példa kedvéért hello munkaterület gyűjtemény neve: **mypbiapp**. Azt az imént beállított hello elérési kulcsot, amely azt korábban másolta, mint a **AppKey**. Nagyon egyszerű hitelesítést is!

**HTTP-kérelem**

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces
Authorization: AppKey MpaUgrTv5e...
```

**HTTP-válasz**

```
HTTP/1.1 201 Created
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces
RequestId: 4220d385-2fb3-406b-8901-4ebe11a5f6da

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/$metadata#workspaces/$entity",
  "workspaceId": "32960a09-6366-4208-a8bb-9e0678cdbb9d",
  "workspaceCollectionName": "mypbiapp"
}
```

visszaadott hello **workspaceId** hello a következő további API-hívásokban használt. Az alkalmazás őrizze meg ezt az értéket.

## <a name="import-pbix-file-into-hello-workspace"></a>A .pbix fájlok importálása hello munkaterület

Az egyes jelentések a munkaterületen felel meg egyetlen Power BI Desktop fájl tooa dataset \(datasource beállításokat is beleértve). A .pbix fájl toohello munkaterület, ahogy az alábbi kód hello importálhatja azt. Ahogy látja, azt feltöltheti hello bináris .pbix fájl MIME multipart http használatával.

hello uri-töredéket **32960a09-6366-4208-a8bb-9e0678cdbb9d** hello workspaceId, és a lekérdezési paraméter **datasetDisplayName** hello dataset neve toocreate van. hello létrehozott adatkészlet alkalmazáshoz kapcsolódó összes adatot tartalmazza az importált adatok, például a .pbix fájlok összetevők hello mutató toohello adatforrás stb...

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports?datasetDisplayName=mydataset01
Authorization: AppKey MpaUgrTv5e...
Content-Type: multipart/form-data; boundary="A300testx"

--A300testx
Content-Disposition: form-data

{hello content (binary) of .pbix file}
--A300testx--
```

A importálási feladat lehet, hogy futtatni egy ideig. Amikor végzett, az alkalmazás importálása azonosítójával hello feladatállapot teheti fel. A jelen példában hello importálási azonosító: **4eec64dd-533b-47c3-a72c-6508ad854659**.

```
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659?tenantId=myorg
RequestId: 658bd6b4-b68d-4ec3-8818-2a94266dc220

{"id":"4eec64dd-533b-47c3-a72c-6508ad854659"}
```

hello következő kért állapota az importálás azonosítójával:

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659
Authorization: AppKey MpaUgrTv5e...
```

Ha hello feladat nem teljes, HTTP-válasz hello ehhez hasonló lehet:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: 614a13a5-4de7-43e8-83c9-9cd225535136

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Publishing",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "name": "mydataset01"
}
```

Hello feladat befejeződött, ha HTTP-válasz hello további ehhez hasonló lehet:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: eb2c5a85-4d7d-4cc2-b0aa-0bafee4b1606

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Succeeded",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "reports": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://app.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c"
    }
  ],
  "datasets": [
    {
      "id": "458e0451-7215-4029-80b3-9627bf3417b0",
      "name": "mydataset01",
      "tables": [
      ],
      "webUrl": "https://app.powerbi.com/datasets/458e0451-7215-4029-80b3-9627bf3417b0"
    }
  ],
  "name": "mydataset01"
}
```

## <a name="data-source-connectivity-and-multi-tenancy-of-data"></a>Az adatforrás-kapcsolat \(és az adatok több-bérlős)

A munkaterületre importált, szinte teljes egészében a .pbix fájlok hello összetevők hello adatforrások hitelesítő adatait nem is. Ennek eredményeképpen használatakor **DirectQuery módban**, hello beágyazott jelentés nem jeleníthető meg helyesen. De használata esetén **importálás módra**, azt megtekintheti hello jelentést hello meglévő importált adatok használatával. Ebben az esetben a Microsoft hello hitelesítő adatot használja az alábbi lépések segítségével REST-hívások hello kell beállítania.

Először azt kell szereznie hello gateway datasource. Tudjuk, hello dataset **azonosító** hello korábban visszaadott azonosítója.

**HTTP-kérelem**

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.GetBoundGatewayDatasources
Authorization: AppKey MpaUgrTv5e...
```

**HTTP-válasz**

```
GET HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: 574b0b18-a6fa-46a6-826c-e65840cf6e15

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#gatewayDatasources",
  "value": [
    {
      "id": "5f7ee2e7-4851-44a1-8b75-3eb01309d0ea",
      "gatewayId": "ca17e77f-1b51-429b-b059-6b3e3e9685d1",
      "datasourceType": "Sql",
      "connectionDetails": "{\"server\":\"testserver.database.windows.net\",\"database\":\"testdb01\"}"
    }
  ]
}
```

Átjáró és adatforrás-azonosító segítségével hello visszaadott \(tekintse meg az előző hello **gatewayId** és **azonosító** eredményt adott vissza a hello), az alábbiak szerint módosíthatja azt hello Ez az adatforrás hitelesítő adatai:

**HTTP-kérelem**

```
PATCH https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/gateways/ca17e77f-1b51-429b-b059-6b3e3e9685d1/datasources/5f7ee2e7-4851-44a1-8b75-3eb01309d0ea
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "credentialType": "Basic",
  "basicCredentials": {
    "username": "demouser",
    "password": "P@ssw0rd"
  }
}
```

**HTTP-válasz**

```
HTTP/1.1 200 OK
Content-Type: application/octet-stream
RequestId: 0e533c13-266a-4a9d-8718-fdad90391099
```

Éles azt is beállíthatja az egyes munkaterületeken REST API használatával hello különböző kapcsolati karakterláncot. \(Egytényezős, azt is külön hello adatbázis egyes ügyfelek esetén.)

hello következő hello kapcsolati karakterlánca datasource REST-en keresztül módosítja.

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.SetAllConnections
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "connectionString": "data source=testserver02.database.windows.net;initial catalog=testdb02;persist security info=True;encrypt=True;trustservercertificate=False"
}
```

Vagy a Power BI Embedded is használhatók. a sorszintű biztonság, és azt is külön hello minden felhasználója egy jelentést. As a Result, azt hozhat létre minden ügyfél azonos .pbix jelentés \(felhasználói felületén, stb.) és a különböző adatforrásokból.

> [!NOTE]
> Használata **importálás módra** helyett **DirectQuery módban**, nincs módja toorefresh modellek API-n keresztül. És a helyszíni adatforrások keresztül Power BI-átjáró a Power BI Embedded még nem támogatott. Azonban valóban érdemes tookeep követheti a hello [Power BI blog](https://powerbi.microsoft.com/blog/) újdonságok, és mi várható későbbi kiadásai.

## <a name="authentication-and-hosting-embedding-reports-in-our-web-page"></a>Hitelesítés és a weblap (beágyazási) jelentéseket tartalmazó

A hello előző REST API-t, használhatjuk hello hozzáférési kulcs **AppKey** hello engedélyezési fejléc saját magát. Az adott hívások kezelhető hello háttér kiszolgáló oldalán, mert az biztonságos.

De azt hello jelentés beágyazása a weblapot, ha az ilyen biztonsági információk szeretné kezelni JavaScript használatával \(előtér). Majd hello engedélyezési fejléc értéke védetté kell tennie. A hozzáférési kulcsot a rosszindulatú felhasználók vagy a kártevő kód észlel, ha azokat a műveleteket, ezt a kulcsot használva tudják hívni.

Ha azt hello jelentés beágyazása a weblap, azt kell használnia hello számított token helyett a hozzáférési kulcsot **AppKey**. Az alkalmazás létre kell hoznia az OAuth Json Web Token hello \(JWT) amely: hello jogcímek és hello számított digitális aláírás. Alább bemutatott, az OAuth JWT kódolású karakterlánc ponttal elválasztott jogkivonatokat.

![](media/power-bi-embedded-iframe/oauth-jwt.png)

Először azt kell előkészítenie hello bemeneti érték, amely később alá van írva. Ez az érték hello base64 kódolású url (rfc4648) karakterlánc a következő json hello, és ezek határolja hello pont \(.) karaktert. Később azt ismertetjük, hogyan tooget hello azonosítója.

> [!NOTE]
> Ha azt szeretné, hogy toouse sor sorszintű biztonságot (RLS) Power BI Embedded, azt is meg kell adnia **felhasználónév** és **szerepkörök** hello jogcímekben.

```
{
  "typ":"JWT",
  "alg":"HS256"
}
```

```
{
  "wid":"{workspace id}",
  "rid":"{report id}",
  "wcn":"{workspace collection name}",
  "iss":"PowerBISDK",
  "ver":"0.2.0",
  "aud":"https://analysis.windows.net/powerbi/api",
  "nbf":{start time of token expiration},
  "exp":{end time of token expiration}
}
```

Ezután azt kell létrehoznia hello base64 kódolású karakterlánc HMAC \(hello aláírás) SHA-256 algoritmussal. Az aláírt bemeneti értéke hello előző karakterlánc.

Utolsó, igazolnia kell alakítania hello bemeneti érték és időszak aláírás karakterláncot \(.) karaktert. befejeződött hello karakterlánca hello app hello jelentés beágyazása lexikális eleme. Akkor is, ha egy rosszindulatú felhasználó hello alkalmazási jogkivonatának észlel, akkor nem olvasható be a hello eredeti elérési kulcsot. Az alkalmazás jogkivonatában gyorsan lejár.

Íme egy PHP-példa az alábbi lépéseket:

```
<?php
// 1. power bi access key
$accesskey = "MpaUgrTv5e...";

// 2. construct input value
$token1 = "{" .
  "\"typ\":\"JWT\"," .
  "\"alg\":\"HS256\"" .
  "}";
$token2 = "{" .
  "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
  "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
  "\"wcn\":\"mypbiapp\"," . // workspace collection name
  "\"iss\":\"PowerBISDK\"," .
  "\"ver\":\"0.2.0\"," .
  "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
  "\"nbf\":" . date("U") . "," .
  "\"exp\":" . date("U" , strtotime("+1 hour")) .
  "}";
$inputval = rfc4648_base64_encode($token1) .
  "." .
  rfc4648_base64_encode($token2);

// 3. get encoded signature
$hash = hash_hmac("sha256",
    $inputval,
    $accesskey,
    true);
$sig = rfc4648_base64_encode($hash);

// 4. show result (which is hello apptoken)
$apptoken = $inputval . "." . $sig;
echo($apptoken);

// helper functions
function rfc4648_base64_encode($arg) {
  $res = $arg;
  $res = base64_encode($res);
  $res = str_replace("/", "_", $res);
  $res = str_replace("+", "-", $res);
  $res = rtrim($res, "=");
  return $res;
}
?>
```

## <a name="finally-embed-hello-report-into-hello-web-page"></a>Végezetül hello jelentés beágyazása hello weblap

A jelentés beágyazása, igazolnia kell kapnak hello URL-cím és a jelentés beágyazása **azonosító** hello következő REST API használatával.

**HTTP-kérelem**

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/reports
Authorization: AppKey MpaUgrTv5e...
```

**HTTP-válasz**

```
HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: d4099022-405b-49d3-b3b7-3c60cf675958

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#reports",
  "value": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c",
      "isFromPbix": false
    }
  ]
}
```

Azt is hello jelentés beágyazása a webalkalmazást a hello előző app token használatával.
Ha úgy tekintünk hello következő mintakód, hello korábbi részében van ugyanaz, mint a korábbi példa hello hello. Hello utóbbi részében, az alábbi példa hello **embedUrl** \(hello előző eredményt) hello iframe, és van könyvelési hello alkalmazási jogkivonatának hello iframe be.

> [!NOTE]
> Toochange hello jelentés azonosító érték tooone saját lesz szüksége. Emellett a Tartalomkezelés rendszerben tooa hiba miatt hello iframe-címke hello kódminta olvasható szó. Ha másolja és illessze be a mintakód hello címke eltávolítása tárfiókonként hello szöveg.

```
    <?php
    // 1. power bi access key
    $accesskey = "MpaUgrTv5e...";

    // 2. construct input value
    $token1 = "{" .
      "\"typ\":\"JWT\"," .
      "\"alg\":\"HS256\"" .
      "}";
    $token2 = "{" .
      "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
      "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
      "\"wcn\":\"mypbiapp\"," . // workspace collection name
      "\"iss\":\"PowerBISDK\"," .
      "\"ver\":\"0.2.0\"," .
      "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
      "\"nbf\":" . date("U") . "," .
      "\"exp\":" . date("U" , strtotime("+1 hour")) .
      "}";
    $inputval = rfc4648_base64_encode($token1) .
      "." .
      rfc4648_base64_encode($token2);

    // 3. get encoded signature value
    $hash = hash_hmac("sha256",
        $inputval,
        $accesskey,
        true);
    $sig = rfc4648_base64_encode($hash);

    // 4. get apptoken
    $apptoken = $inputval . "." . $sig;

    // helper functions
    function rfc4648_base64_encode($arg) {
      $res = $arg;
      $res = base64_encode($res);
      $res = str_replace("/", "_", $res);
      $res = str_replace("+", "-", $res);
      $res = rtrim($res, "=");
      return $res;
    }
    ?>
    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8" />
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <title>Test page</title>
      <meta name="viewport" content="width=device-width, initial-scale=1">
    </head>
    <body>
      <button id="btnView">View Report !</button>
      <div id="divView">
        <**REMOVE THIS CAPPED TEXT IF COPIED** iframe id="ifrTile" width="100%" height="400"></iframe>
      </div>
      <script>
        (function () {
          document.getElementById('btnView').onclick = function() {
            var iframe = document.getElementById('ifrTile');
            iframe.src = 'https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c';
            iframe.onload = function() {
              var msgJson = {
                action: "loadReport",
                accessToken: "<?=$apptoken?>",
                height: 500,
                width: 722
              };
              var msgTxt = JSON.stringify(msgJson);
              iframe.contentWindow.postMessage(msgTxt, "*");
            };
          };
        }());
      </script>
    </body>
```

És az eredmény itt található:

![](media/power-bi-embedded-iframe/view-report.png)

Jelenleg a Power BI Embedded csak mutatja be hello hello iframe. De, nyomon követheti a hello [Power BI Blog](https://powerbi.microsoft.com/blog/). Jövőbeli fejlesztések használhatja új ügyféloldali API-t fog ossza meg velünk hello iframe adatokat küldeni, valamint információ. Izgalmas dolgai!

## <a name="see-also"></a>Lásd még:
* [Hitelesítés és engedélyezés a Power BI Embedded használatával](power-bi-embedded-app-token-flow.md)

További kérdései vannak? [Próbálja meg a Power BI-Közösség hello](http://community.powerbi.com/)

