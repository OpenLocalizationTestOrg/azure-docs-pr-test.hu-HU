---
title: "az Azure Functions fejlesztése aaaGuidance |} Microsoft Docs"
description: "Ismerje meg, hello Azure Functions elvekről és technikákról, hogy kell-e toodevelop funkciók Azure, az összes programozási nyelveket és kötéseket."
services: functions
documentationcenter: na
author: christopheranderson
manager: erikre
editor: 
tags: 
keywords: "Fejlesztői útmutató, azure funkciók, Funkciók, Eseményfeldolgozási, webhookokkal, a dinamikus számítási, a kiszolgáló nélküli architektúrája"
ms.assetid: d8efe41a-bef8-4167-ba97-f3e016fcd39e
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/30/2017
ms.author: chrande
ms.openlocfilehash: 86d37dae5333f615faafc966e9da6e08e0a6354e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-developers-guide"></a>Az Azure Functions fejlesztői útmutatója
Az Azure Functions adott funkciókhoz ossza meg néhány alapvető technikai kulcsfogalmak és összetevők függetlenül hello nyelvi vagy használata kötelező. Ahhoz, hogy belevágjon tanulási részletek adott tooa megadott nyelv vagy a kötés, lehet, hogy tooread keresztül ez az áttekintés, amely egyiket tooall vonatkozik.

Ez a cikk feltételezi, hogy Ön már elolvasta hello [Azure Functions áttekintése](functions-overview.md) és ismeri a [WebJobs SDK fogalmak hello JobHost futásidejű, eseményindítók és kötések például](../app-service-web/websites-dotnet-webjobs-sdk.md). Az Azure Functions hello WebJobs SDK alapul. 

## <a name="function-code"></a>Funkciókódot
A *függvény* hello Azure Functions elsődleges fogalom. Írhat kódot a funkció az Ön által választott nyelven, és mentse hello kódot, és a konfigurációs fájlok hello ugyanabban a mappában. hello konfigurációs nevű `function.json`, amely tartalmazza a JSON-konfigurációs adatok. Különböző nyelveket támogatja, és mindegyiknek egy optimalizált némileg eltérő élmény toowork ajánlott az adott nyelven. 

hello function.json fájl határozza meg, hello függvénykötés és egyéb konfigurációs beállításokkal. hello futásidejű használ, a fájl toodetermine hello események toomonitor és hogyan toopass adatok és a visszatérési adatainak függvény végrehajtása. hello az alábbiakban látható egy példa function.json fájlt.

```json
{
    "disabled":false,
    "bindings":[
        // ... bindings here
        {
            "type": "bindingType",
            "direction": "in",
            "name": "myParamName",
            // ... more depending on binding
        }
    ]
}
```

Set hello `disabled` tulajdonság túl`true` tooprevent hello függvény végrehajtását.

Hello `bindings` tulajdonság értéke, ahol konfigurálhatja az eseményindítók és kötések is. Minden kötésnek osztja meg néhány általános beállítások és az egyes beállítások, amelyek adott tooa bizonyos típusú kötés. Minden kötésnek hello-beállítások a következő szükséges:

| Tulajdonság | Értékek/típusok | Megjegyzések |
| --- | --- | --- |
| `type` |Karakterlánc |Kötés típusa. Például: `queueTrigger`. |
| `direction` |"in" "out" |Azt jelzi, hogy hello kötés hello függvénynek fogadó adatok vagy a küldő adat hello függvényből. |
| `name` |Karakterlánc |hello neve hello használt adatok kötött hello függvényben. C# ez pedig egy argumentum neve; a JavaScript a következőre hello kulcsot a kulcs/érték listáját. |

## <a name="function-app"></a>Függvény alkalmazás
Egy vagy több egyéni függvények felügyelete együtt, amelyet az Azure App Service egy függvény alkalmazást magában foglalja. Az összes olyan függvény app megosztáson hello funkciók hello azonos árképzési terv, a folyamatos üzembe helyezés és a futásidejű verzióját. Több nyelven is írt hello osztoznak funkciók olyan funkciókat alkalmazást. Gondolja át, hogy egy függvény alkalmazást egy módon tooorganize, és a funkciók együttesen kezelése. 

## <a name="runtime-script-host-and-web-host"></a>Futásidejű (script host és webkiszolgáló)
hello futásidejű vagy parancsfájlfuttató, nem hello mögöttes WebJobs SDK állomás eseményeket figyeli, összegyűjti és adatokat küld, és végső soron futtatja a kódot. 

toofacilitate HTTP eseményindítók is van olyan webes gazdagépet, amely tervezett toosit hello parancsfájlfuttató éles forgatókönyvekben elé. Két állomás rendelkező segít tooisolate hello script host alrendszerhez hello előtér forgalom hello webállomását kezeli.

## <a name="folder-structure"></a>Mappaszerkezet
[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Amikor létrehozása egy projektjét, amely telepíti az funkciók tooa függvény alkalmazást az Azure App Service-ben, a helykódot, a gyökérmappa-szerkezetében is kezelheti. Meglévő eszközök, például a folyamatos integrációt és telepítést, vagy egyéni telepítési parancsfájl ennek transpilation kód vagy deploy idő csomag telepítése.

> [!NOTE]
> Győződjön meg arról, hogy toodeploy a `host.json` fájl-és a függvény közvetlenül toohello `wwwroot` mappa. Nem tartalmaznak hello `wwwroot` mappába a központi telepítés. Ellenkező esetben, végül `wwwroot\wwwroot` mappák. 
> 
> 

## <a id="fileupdate"></a>Hogyan tooupdate működni az alkalmazás fájljai
hello függvény szerkesztő épített hello Azure-portál lehetővé teszi, hogy frissíteni hello *function.json* és hello kód fájl függvény esetében. tooupload vagy más frissítési fájlok, például a *package.json* vagy *project.json* vagy függőségek toouse más központi telepítési módszerekkel van.

Függvény a épülnek App Service-ben, az összes hello [központi telepítési beállítások a webalkalmazások rendelkezésre toostandard](../app-service-web/web-sites-deploy.md) függvény alkalmazások esetében is elérhetők. Az alábbiakban néhány módszerek tooupload használhatja, vagy függvény app fájlok frissítése. 

#### <a name="toouse-app-service-editor"></a>App Service-szerkesztő toouse
1. Hello Azure Functions portálon kattintson **Alkalmazásbeállítások működéséhez**.
2. A hello **speciális beállítások** kattintson **Ugrás tooApp szolgáltatás beállításaira**.
3. Kattintson a **App Service-szerkesztő** alkalmazás menü NAV alatt **FEJLESZTŐESZKÖZÖK**.
4. Kattintson a **Ugrás**.
   
   App Service-szerkesztő betöltése után látni fogja a hello *host.json* fájl- és függvény mappáit *wwwroot*. 
5. A megnyitott fájlok tooedit őket, vagy áthúzása a fejlesztési számítógép tooupload fájlokból.

#### <a name="toouse-hello-function-apps-scm-kudu-endpoint"></a>toouse hello függvény app SCM (Kudu) végpont
1. Nyissa meg: `https://<function_app_name>.scm.azurewebsites.net`.
2. Kattintson a **konzol Debug > CMD**.
3. Keresse meg a túl`D:\home\site\wwwroot\` tooupdate *host.json* vagy `D:\home\site\wwwroot\<function_name>` tooupdate egy függvény fájlokat.
4. Fogd és vidd hello fájl rácsban hello megfelelő mappába tooupload kívánt fájl. Nincsenek a két területen hello fájl rácsban, ha egy fájl elvetné. A *.zip* fájlok, megjelenik egy hello címkével "húzzon ide tooupload és csomagolja ki." Minden olyan fájltípus esetében hello fájl rácsban dobja el, de kívülről hello "csomagolja ki" mezőbe.

<!--NOTE: I've removed documentation on FTP, because it does not sync triggers on hello consumption plan --DonnaM -->

#### <a name="toouse-continuous-deployment"></a>toouse folyamatos üzembe helyezés
Hello hello témakörben található utasítások [folyamatos üzembe helyezés az Azure Functions](functions-continuous-deployment.md).

## <a name="parallel-execution"></a>Párhuzamos végrehajtás
Több eseményindító események bekövetkezésekor gyorsabb, mint az egyszálas függvény futtatókörnyezettel is dolgozza fel őket, hello futásidejű hivatkozhat hello függvény párhuzamosan több alkalommal.  Ha egy függvény alkalmazás hello [üzemeltetési terv fogyasztás](functions-scale.md#how-the-consumption-plan-works), hello függvény alkalmazás automatikusan sikerült kiterjesztése.  Minden példánya hello függvény alkalmazást, és hogy hello alkalmazás fut. hello üzemeltetési terv vagy egy rendszeres felhasználási [az alkalmazásszolgáltatási csomag üzemeltetési](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), előfordulhat, hogy feldolgozni egyidejű függvény meghívásához párhuzamosan több szálat használ.  hello maximális száma párhuzamos függvény meghívásához minden függvény alkalmazáspéldány attól függően változik, valamint egyéb funkciók hello függvény alkalmazásban használt hello erőforrások használt indítófeltétel típusát hello.

## <a name="functions-runtime-versioning"></a>Funkciók futásidejű versioning

Hello verziója hello Functions futtatókörnyezete hello segítségével konfigurálhatja `FUNCTIONS_EXTENSION_VERSION` Alkalmazásbeállítás. Hello "~ 1" érték például azt jelzi, hogy a függvény App 1 fog használni a fő verziószáma. Függvény alkalmazások nem frissített tooeach új alverzió, mivel azok kiadásakor. A függvény App pontos verziójának hello megtekintheti a hello **beállítások** hello Azure Portal lapján.

## <a name="repositories"></a>Adattárak
az Azure Functions hello kódját nyílt forráskódú, és GitHub-adattárak tárolva:

* [Az Azure Functions futtatókörnyezettel](https://github.com/Azure/azure-webjobs-sdk-script/)
* [Az Azure Functions portálra](https://github.com/projectkudu/AzureFunctionsPortal)
* [Az Azure Functions sablonokkal](https://github.com/Azure/azure-webjobs-sdk-templates/)
* [Az Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/)
* [Azure WebJobs SDK-bővítmények](https://github.com/Azure/azure-webjobs-sdk-extensions/)

## <a name="bindings"></a>Kötések
Ez az összes támogatott kötések tábla.

[!INCLUDE [dynamic compute](../../includes/functions-bindings.md)]

## <a name="reporting-issues"></a>Jelentéskészítési problémák
[!INCLUDE [Reporting Issues](../../includes/functions-reporting-issues.md)]

## <a name="next-steps"></a>Következő lépések
További információkért tekintse meg a következő erőforrások hello:

* [Azure Functions – ajánlott eljárások](functions-best-practices.md)
* [Az Azure Functions C# fejlesztői leírás](functions-reference-csharp.md)
* [Az Azure Functions F # fejlesztői leírás](functions-reference-fsharp.md)
* [Az Azure Functions NodeJS fejlesztői leírás](functions-reference-node.md)
* [Az Azure Functions eseményindítók és kötések](functions-triggers-bindings.md)
* [Az Azure Functions: hello út](https://blogs.msdn.microsoft.com/appserviceteam/2016/04/27/azure-functions-the-journey/) hello Azure App Service csapatának blogjában. Hogyan jött létre az Azure Functions előzményeit.

