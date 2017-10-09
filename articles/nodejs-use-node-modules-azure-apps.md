---
title: a Node.js modulok aaaWorking
description: "Ismerje meg, hogy az Azure App Service és a Felhőszolgáltatások használata a Node.js modulok toowork."
services: 
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: c0e6cd3d-932d-433e-b72d-e513e23b4eb6
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: tarcher
ms.openlocfilehash: 926358b7eb80a659dbc1015686b06a30d8c9b8f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-nodejs-modules-with-azure-applications"></a>A Node.js modulok használata az Azure alkalmazásokkal
Ez a dokumentum útmutatást nyújt a Node.js modulok használata az Azure-platformon futó alkalmazások. beleértve annak biztosítását, hogy az alkalmazás a modul egy adott verzióját használja, továbbá a natív modulok az Azure-ral való használatát.

Ha már ismeri a Node.js modulok használata **package.json** és **npm-shrinkwrap.json** fájlok, a következő információk hello Ez a cikk ismertet gyors összegzését tartalmazza:

* Az Azure App Service megértette **package.json** és **npm-shrinkwrap.json** fájlok, illetve az ezen fájlokban található bejegyzések alapuló modult telepítheti.

* Azure Cloud Services várt hello fejlesztőkörnyezetet és hello telepített összes modul toobe **csomópont\_modulok** directory toobe hello központi telepítési csomag részét képezi. Már lehetséges tooenable modulokkal telepítésének támogatása **package.json** vagy **npm-shrinkwrap.json** fájlokat a Felhőszolgáltatások; azonban ez a konfiguráció szükséges hello alapértelmezett testreszabása Cloud Service projektek által használt parancsfájlok. A példa bemutatja, hogyan tooconfigure ebben a környezetben, lásd: [Azure indítási tevékenységhez toorun npm telepítése tooavoid csomópont modulok telepítése](https://github.com/woloski/nodeonazure-blog/blob/master/articles/startup-task-to-run-npm-in-azure.markdown)

> [!NOTE]
> Az Azure virtuális gépek nem esik szó ebben a cikkben mivel hello telepítési élményt nyújt a virtuális gép hello operációs rendszer hello virtuális gép által üzemeltetett függ.
> 
> 

## <a name="nodejs-modules"></a>A node.js modulok
A modulokra betölthető JavaScript-csomagok, amelyek bizonyos funkciók biztosítanak az alkalmazáshoz. Modulok általában segítségével telepíthetők hello **npm** parancssori eszközzel, azonban néhány modulokat (például a http-modulja hello) hello core Node.js csomag részeként biztosított.

Amikor modulok telepítve vannak, hello tárolásuk **csomópont\_modulok** könyvtár: az alkalmazás könyvtárstruktúrát hello gyökérmappájában. Minden modul belül hello **csomópont\_modulok** directory kezeli a saját **csomópont\_modulok** modulokat, attól függ, és ez a viselkedés ismétlődik tartalmazó könyvtárat minden modul összes hello módon hello függőségi lánc le. Ebben a környezetben lehetővé teszi, hogy minden telepített modulokban toohave saját verzióra vonatkozó követelmények hello modulok függ, azonban igen nagy könyvtárstruktúrát eredményezheti.

Központi telepítés hello **csomópont\_modulok** könyvtárába, ha az alkalmazás részét hello telepítési képest hello mérete növekszik toousing egy **package.json** vagy  **npm-shrinkwrap.json** azonban azt garantálja, hogy éles környezetben használt hello modulok hello verziói vannak hello ugyanaz a fejlesztési használt hello modulok; fájlt.

### <a name="native-modules"></a>A natív modulok
A legtöbb modulokra egyszerűen csak egyszerű szöveges JavaScript-fájlt, miközben néhány modulokra platformspecifikus bináris képek. Ezek a modulok összeállítása telepítésekor, általában a Python és a csomópont-gyp. Mivel Azure Felhőszolgáltatások támaszkodnak hello **csomópont\_modulok** mappa hello alkalmazásokban, valamint hello telepített modulok részét felhőszolgáltatásban kell működnie, mindaddig, amíg volt bármilyen natív modul telepítése telepített és összeállítása a Windows fejlesztői rendszernek.

Az Azure App Service nem támogatja a natív modulok, és meghiúsulhat, ha a konkrét Előfeltételek modulok fordítása. Néhány népszerű modulokat, például a MongoDB választható natív függőségekkel rendelkeznek, és ezeket nem működnek, két lehetséges megoldások napjainkban szinte minden elérhető natív modul sikeres bizonyult:

* Futtatás **npm telepítése** egy Windows-gépen, amelyen minden hello natív modul előfeltétel telepítve van. Ezután telepítse a létrehozott hello **csomópont\_modulok** mappa hello alkalmazás tooAzure App Service részeként.

  * Előtt fordítása, ellenőrizze, hogy a helyi Node.js telepítési rendelkezik a megfelelő architektúra és hello verzió minél az Azure-ban használt egyik lehetséges toohello (hello aktuális értékek ellenőrizhetők tulajdonságokból Runtime **process.arch**és **process.version**).

* Az Azure App Service lehet konfigurált tooexecute egyéni bash vagy rendszerhéj-parancsfájlok felkínálva a telepítés során hello lehetőség tooexecute egyéni parancsokat, és pontosan a hello konfigurálása módon **npm telepítése** futtatták. A videó ábrázoló hogyan tooconfigure környezet, lásd: [egyéni webhely üzembe helyezési parancsfájlok a Kudu].

### <a name="using-a-packagejson-file"></a>A package.json fájl használata
Hello **package.json** fájl egy módon toospecify hello legfelső szintű függőségek az alkalmazás által igényelt hello üzemeltetési platformtól telepíthetik hello függőségek, ahelyett, hogy nincs szükség tooinclude hello **csomópont \_csomagok** mappa hello központi telepítésének részeként. Hello alkalmazás telepítése után hello **npm telepítése** parancs használt tooparse hello: **package.json** fájlt, és felsorolt összes hello függőségek telepítése.

Fejlesztési, használhatja a hello **--mentése**, **--mentés-fejlesztői**, vagy **--mentés választható** paraméterek modulok tooadd hello modul tooyour bejegyzésttelepítésekor**package.json** automatikusan fájlt. További információkért lásd: [npm-telepítés](https://docs.npmjs.com/cli/install).

Egy potenciális problémát, amelynek hello **package.json** fájl csak meghatározza a legfelső szintű függőségekhez hello verziója. Minden telepített modulokban előfordulhat, hogy, vagy attól függ, hello modulok hello verziója nem adható meg, és így, akkor előfordulhat, hogy akkor fordulhatnak elő egy másik függőségi lánc egyik fejlesztési használt hello mint.

> [!NOTE]
> App Service-ben tooAzure telepítésekor, ha a <b>package.json</b> fájl hivatkozik egy natív moduljára, megjelenhet egy hasonló toohello hiba, ha a Git használatával hello alkalmazás közzététele a következő példa:
> 
> npm hiba! module-name@0.6.0telepítés: "csomópont-gyp konfigurálása build"
> 
> npm hiba! "cmd"/ c""csomópont-gyp konfigurálása build"" sikertelen 1
> 
> 

### <a name="using-a-npm-shrinkwrapjson-file"></a>Npm-shrinkwrap.json fájl használatával
Hello **npm-shrinkwrap.json** fájl egy kísérlet tooaddress hello modul versioning korlátozásai hello **package.json** fájlt. Hello közben **package.json** fájl csak verziójával együtt hello legfelső szintű modulok, hello **npm-shrinkwrap.json** fájl hello verzió követelményeit hello teljes modul függőségi lánc tartalmazza.

Amikor az alkalmazás készen áll a termelési, verzióra vonatkozó követelmények zárolását, és hozzon létre egy **npm-shrinkwrap.json** hello segítségével fájlt **npm shrinkwrap** parancsot. Ez a parancs fogja használni a hello jelenleg telepített hello verziók **csomópont\_modulok** mappa, és jegyezze fel ezeket verziók toohello **npm-shrinkwrap.json** fájlt. Hello alkalmazást üzemeltető környezetben telepített toohello után hello **npm telepítése** parancs használt tooparse hello: **npm-shrinkwrap.json** fájlt, és felsorolt összes hello függőségek telepítése. További információkért lásd: [npm-shrinkwrap](https://docs.npmjs.com/cli/shrinkwrap).

> [!NOTE]
> App Service-ben tooAzure telepítésekor, ha a <b>npm-shrinkwrap.json</b> fájl hivatkozik egy natív moduljára, megjelenhet egy hasonló toohello hiba, ha a Git használatával hello alkalmazás közzététele a következő példa:
> 
> npm hiba! module-name@0.6.0telepítés: "csomópont-gyp konfigurálása build"
> 
> npm hiba! "cmd"/ c""csomópont-gyp konfigurálása build"" sikertelen 1
> 
> 

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerkedett hogyan toouse Node.js modulok az Azure-ral, megtudhatja, hogyan túl[hello Node.js verzió megadása], [és üzembe egy Node.js-webalkalmazás](app-service-web/app-service-web-get-started-nodejs.md), és [hogyan toouse hello Azure parancssori Felület Mac és Linux].

További információkért lásd: hello [Node.js fejlesztői központ](/nodejs/azure/).

[hello Node.js verzió megadása]: nodejs-specify-node-version-azure-apps.md
[hogyan toouse hello Azure parancssori Felület Mac és Linux]:cli-install-nodejs.md
[egyéni webhely üzembe helyezési parancsfájlok a Kudu]: https://channel9.msdn.com/Shows/Azure-Friday/Custom-Web-Site-Deployment-Scripts-with-Kudu-with-David-Ebbo
