---
title: "aaaGet Node.js az Azure Search használatába |} Microsoft Docs"
description: "Keresőalkalmazás felépítési útmutatója az Azure egy üzemeltetett felhőalapú keresőszolgáltatásához a Node.js programozási nyelv használatával."
services: search
documentationcenter: 
author: EvanBoyle
manager: pablocas
editor: v-lincan
ms.assetid: 0625dc1b-9db6-40d5-ba9a-4738b75cbe19
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.date: 04/26/2017
ms.author: evboyle
ms.openlocfilehash: e9c7d756c2ea191ee2a285485c90439b96aa73b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-search-in-nodejs"></a>Bevezetés az Azure Search használatába Node.js-ben
> [!div class="op_single_selector"]
> * [Portal](search-get-started-portal.md)
> * [.NET](search-howto-dotnet-sdk.md)
> 
> 

Ismerje meg, egy egyéni Node.js toobuild a keresési alkalmazás használ az Azure Search szolgáltatást a keresésekhez. Ez az oktatóanyag használja hello [Azure Search szolgáltatás REST API](https://msdn.microsoft.com/library/dn798935.aspx) tooconstruct hello objektumok és műveletek ebben a gyakorlatban.

Használtuk [Node.js](https://Nodejs.org) és NPM, [Sublime Text 3](http://www.sublimetext.com/3), és a Windows PowerShell, a Windows 8.1 toodevelop és tesztelheti ezt a kódot.

toorun Ez a minta rendelkeznie kell egy Azure Search szolgáltatással, amely jelentkezhet be a hello [Azure-portálon](https://portal.azure.com). Lásd: [Azure Search szolgáltatás létrehozása a portál hello](search-create-service-portal.md) részletes útmutatásait.

## <a name="about-hello-data"></a>Hello adatokról
A mintaalkalmazás hello adatait használja [az Amerikai Egyesült Államok geológiai szolgáltatások (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), szűrt a hello Rhode Island államra tooreduce hello dataset mérete. Az adatok toobuild, amely jellegzetes épületeket, például kórházakat és iskolákat, valamint geológiai jellegzetességeket, például folyókat, tavakat és hegycsúcsokat ad vissza egy olyan keresőalkalmazás fogjuk használni.

Ebben az alkalmazásban hello **DataIndexer** program létrehozza és betölti az indexet hello egy [indexelő](https://msdn.microsoft.com/library/azure/dn798918.aspx) szerkezet hello szűrt USGS-adatkészletet a nyilvános Azure SQL-adatbázis. Hitelesítő adatok és csatlakozási adatokat toohello online adatforrás hello programkód valósul meg. Nincs szükség további konfigurációra.

> [!NOTE]
> Olyan szűrőt alkalmaztunk a dataset toostay hello 10 000 dokumentumos korlátja az ingyenes tarifacsomag hello alatt. Hello standard csomagot használja, ha a nem vonatkozik ez a korlátozás. Az egyes tarifacsomagok kapacitásával kapcsolatos részletes információkat lásd: [A Search szolgáltatásra vonatkozó korlátozások](search-limits-quotas-capacity.md).
> 
> 

<a id="sub-2"></a>

## <a name="find-hello-service-name-and-api-key-of-your-azure-search-service"></a>Hello szolgáltatás- és api-kulcsot az Azure Search szolgáltatás keresése
Miután létrehozta a hello szolgáltatást, térjen vissza toohello portál tooget hello URL-cím vagy `api-key`. Kapcsolatok tooyour keresési szolgáltatás szükséges, hogy rendelkezik-e mindkét hello URL-címet és egy `api-key` tooauthenticate hello hívás.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello Ugrás sávon kattintson **keresési szolgáltatás** minden Azure Search szolgáltatás megjelenjen az előfizetéséhez kapcsolódó toolist.
3. Válassza ki a kívánt toouse hello szolgáltatást.
4. Hello szolgáltatás irányítópultján meg kell jelennie az alapvető információkat, például a hello adminisztrációs kulcsok eléréséhez szükséges kulcs ikon hello csempék.
5. Másolja a hello szolgáltatás URL-cím, egy adminisztrációs kulcsot és egy lekérdezési kulcsot. Három később szüksége amikor toohello config.js fájl hozzáadja őket.

## <a name="download-hello-sample-files"></a>Hello mintafájlok letöltése
A következő módszerekkel toodownload hello minta hello bármelyikét használhatja.

1. Nyissa meg túl[AzureSearchNodeJSIndexerDemo](https://github.com/AzureSearch/AzureSearchNodejsIndexerDemo).
2. Kattintson a **töltse le a ZIP-**, mentse hello .zip fájlt, és bontsa ki a benne található összes hello fájlt.

Minden további fájlmódosítás és utasításfuttatás az ebben a mappában lévő fájlokra vonatkozóan történik.

## <a name="update-hello-configjs-with-your-search-service-url-and-api-key"></a>Frissítse a hello config.js. a keresőszolgáltatása URL-címével és API-kulcsával
URL-CÍMÉT és api-kulcsot, korábban kimásolt, használatával hello hello URL-Címének megadása, adminisztrációs-kulcsot, és a lekérdezési kulcsot a konfigurációs fájlban.

Az adminisztrációs kulcsok teljes körű vezérlést biztosítanak a szolgáltatási műveletek felett, beleértve az index létrehozását vagy törlését, illetve a dokumentumok betöltését. Ezzel szemben lekérdezés kulcsai csak olvasható műveletekhez, jellemzően tooAzure keresési szolgáltatáshoz kapcsolódó ügyfélalkalmazásokhoz által használt.

Ez a példa hello lekérdezési kulcs toohelp megerősítése hello ajánlott eljárás használatával hello lekérdezési kulcs ügyfélalkalmazásokban magában foglalja.

a következő képernyőfelvételen látható hello **config.js** hello egy szövegszerkesztőben megnyitott megfelelő bejegyzések meg vannak jelölve, hogy láthatja, ahol tooupdate hello fájl hello értékek, amelyek érvényesek a keresési szolgáltatáshoz.

![][5]

## <a name="host-a-runtime-environment-for-hello-sample"></a>Hello minta futtatókörnyezetének üzemeltetése
hello minta HTTP-kiszolgáló, amely npm segítségével globálisan telepítése szükséges.

A következő parancsok hello használni egy PowerShell-ablakot.

1. Keresse meg a hello tartalmazó mappa toohello **package.json** fájlt.
2. Gépelje be: `npm install`.
3. Gépelje be: `npm install -g http-server`.

## <a name="build-hello-index-and-run-hello-application"></a>Hello index létrehozása és hello alkalmazás futtatása
1. Gépelje be: `npm run indexDocuments`.
2. Gépelje be: `npm run build`.
3. Gépelje be: `npm run start_server`.
4. Irányítsa a böngészőt a következő helyre: `http://localhost:8080/index.html`

## <a name="search-on-usgs-data"></a>USGS-adatok keresése
hello USGS-adatkészlet a Rhode Island államra vonatkozó toohello rekordokat tartalmaz. Ha **keresési** egy üres keresőmező kap hello 50 legfontosabb bejegyzés; hello alapértelmezett.

A keresett kifejezés beírása ad hello keresőmotor valamit a toogo. Próbáljon meg a helyhez kötődő nevet beírni. "Roger Williams" volt Rhode Island első kormányzója hello. Számos parkot, épületet és iskolát neveztek el róla.

![][9]

Megpróbálhatja beírni az alábbi kifejezések bármelyikét is:

* Pawtucket
* Pembroke
* goose+cape

## <a name="next-steps"></a>Következő lépések
Ez az hello Azure Search első oktatóanyaga a Node.js és hello USGS-adatkészlet alapján. Idővel majd tovább bővítjük ezen oktatóanyag toodemonstrate kiegészítő keresési funkciókat, amelyeket esetleg szívesen toouse egyéni megoldásaiban.

Ha már rendelkezik bizonyos tapasztalattal az Azure Search használatában, ezt a mintát akár ugródeszkaként is használhatja a javaslattevők (előre begépelt vagy automatikusan kitöltött lekérdezések), szűrők és a jellemzőalapú navigáció kipróbálásához. Akkor is tovább fejlesztheti hello keresési eredmények oldalát által számok és kötegelt dokumentumok, így a felhasználók lapozhassanak az eredmények hello hozzáadásával.

Új tooAzure keresési? Azt javasoljuk, próbáljon más oktatóanyagok toodevelop megismerhesse, mit hozhat létre. Látogasson el a [dokumentációs oldal](https://azure.microsoft.com/documentation/services/search/) toofind több erőforrást. Hello hivatkozások is megtekintheti a [videók és oktatóanyagok listáját](search-video-demo-tutorial-list.md) tooaccess további információt.

<!--Image references-->
[1]: ./media/search-get-started-Nodejs/create-search-portal-1.PNG
[2]: ./media/search-get-started-Nodejs/create-search-portal-2.PNG
[3]: ./media/search-get-started-Nodejs/create-search-portal-3.PNG
[5]: ./media/search-get-started-Nodejs/AzSearch-Nodejs-configjs.png
[9]: ./media/search-get-started-Nodejs/rogerwilliamsschool.png
