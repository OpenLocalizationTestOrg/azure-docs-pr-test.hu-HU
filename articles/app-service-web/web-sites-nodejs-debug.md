---
title: "A Node.js webalkalmazás hibakeresése az Azure App Service-ben"
description: "További tudnivalók az Azure App Service a Node.js webalkalmazás hibakeresése."
tags: azure-portal
services: app-service\web
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: a48f906c-1a3e-43bc-ae84-7d2dde175b15
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: tarcher
ms.openlocfilehash: 5e302a4c58a171d40e43a22c34c724e868019ec8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-debug-a-nodejs-web-app-in-azure-app-service"></a>A Node.js webalkalmazás hibakeresése az Azure App Service-ben
Azure biztosít, elősegítve ezzel a hibakeresés üzemeltetett Node.js-alkalmazások beépített diagnosztika [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webalkalmazások. Ebből a cikkből megtudhatja, hogy a stdout és az stderr naplózását, a hiba adatait a böngészőben megjelenő, és töltse le és naplófájlban talál.

Az Azure-platformon futó Node.js alkalmazások diagnosztika által biztosított [IISNode]. Ez a cikk ismerteti a leggyakoribb beállítások diagnosztikai adatok gyűjtésének, amíg nem biztosít teljes IISNode való munkához. Az IISNode munkavégzés további információkért lásd: a [IISNode információs] a Githubon.

<a id="enablelogging"></a>

## <a name="enable-logging"></a>Naplózás engedélyezése
Alapértelmezés szerint egy App Service webalkalmazásba csak telepítések, például a Git használatával webes alkalmazás telepítésekor diagnosztikai adatait rögzíti. Ez az információ akkor hasznos, ha problémák merülnek fel a hivatkozott modul telepítésekor hiba például a telepítés során **package.json**, vagy ha egy egyéni telepítési parancsfájlt használ.

Az stdout és az stderr adatfolyamok a naplózás engedélyezéséhez létre kell hoznia egy **iisnode.yml fájlt** fájlt a Node.js-alkalmazás gyökérkönyvtárában, és adja hozzá a következő:

    loggingEnabled: true

Ez lehetővé teszi az stderr-en és a Node.js-alkalmazás az stdout naplózását.

A **iisnode.yml fájlt** fájl is is kell annak vezérlésére szolgál, hogy egyéni hibaüzenetek és a fejlesztői hibát a rendszer visszairányítja a böngésző Ha hiba történik. Fejlesztői hibák engedélyezéséhez vegye fel a következő sort a **iisnode.yml fájlt** fájlt:

    devErrorsEnabled: true

Ha engedélyezi ezt a beállítást, az IISNode, mint például "egy belső kiszolgálóhiba történt" helyett a felhasználóbarát hibaüzenet stderr elküldött adatok utolsó 64K ad vissza.

> [!NOTE]
> Amíg devErrorsEnabled akkor hasznos, ha problémák diagnosztizálása a fejlesztés során, a termelési környezetben engedélyezné azt az küldi el a végfelhasználók fejlesztési hibákat eredményezhet.
> 
> 

Ha a **iisnode.yml fájlt** fájl már nem létezett az alkalmazáson belül, a frissített alkalmazás közzététele után újra kell indítania a webes alkalmazást. Egyszerűen módosítani egy meglévő beállítások **iisnode.yml fájlt** korábban közzétett fájl, újraindításra nincs szükség.

> [!NOTE]
> Ha a webalkalmazás létrehozása az Azure parancssori eszközök vagy az Azure PowerShell-parancsmagok, egy alapértelmezett **iisnode.yml fájlt** fájl automatikusan létrejön.
> 
> 

A webalkalmazás újraindítása, válassza ki a webalkalmazás a [Azure Portal](https://portal.azure.com), és kattintson a **indítsa újra a** gombra:

![Indítsa újra a gomb][restart-button]

Ha a fejlesztési környezetet az Azure parancssori eszközök vannak telepítve, a következő paranccsal indítsa újra a webes alkalmazást:

    azure site restart [sitename]

> [!NOTE]
> Míg loggingEnabled és devErrorsEnabled a leggyakrabban használt iisnode.yml fájlt konfigurációs beállítások diagnosztikai adatok rögzítéséhez, iisnode.yml fájlt számos lehetőséget a üzemeltetési környezet konfigurálásához használható. A konfigurációs beállítások teljes listáját lásd: a [iisnode_schema.xml](https://github.com/tjanczuk/iisnode/blob/master/src/config/iisnode_schema.xml) fájlt.
> 
> 

<a id="viewlogs"></a>

## <a name="accessing-logs"></a>Naplók elérése
Diagnosztikai naplók elérhető háromféleképpen; A File Transfer Protocol (FTP), egy Zip-archívum letöltése használatával, vagy mint egy élő adatfolyam a napló (más néven utóhívás) frissítése. A Zip-archívumot, a naplófájlok letöltése, illetve az élő adatfolyam megtekintése megkövetelése az Azure parancssori eszközök. Ezek a következő paranccsal telepíthető:

    npm install azure-cli -g

A telepítést követően az eszközök segítségével érhető el az "azure" paranccsal. A parancssori eszközök először konfigurálni kell az Azure-előfizetés használatára. Ennek a feladatnak kapcsolatos információkért lásd: a **letöltése és importálása közzétételi beállítások** szakasza a [hogyan használható az Azure parancssori eszközt](../xplat-cli-connect.md) cikk.

### <a name="ftp"></a>FTP
A diagnosztikai információk eléréséhez FTP keresztül, látogasson el a [Azure Portal](https://portal.azure.com), a webes alkalmazást, majd válassza ki és a **IRÁNYÍTÓPULT**. Az a **Gyorshivatkozások** szakaszban, a **FTP diagnosztikai naplók** és **diagnosztikai naplók FTPS** hivatkozásokat biztosít hozzáférést a naplókat az FTP protokoll használatával.

> [!NOTE]
> Ha nem korábban konfigurált felhasználónevet és jelszót FTP- vagy a központi telepítés, ehhez a a **gyors üzembe helyezés** kezelése lap választásával **üzembe helyezési hitelesítő adatok beállítása**.
> 
> 

Az FTP URL-cím, az irányítópult visszaadott a **naplófájlok** könyvtár, amely tartalmazza a következő alkönyvtárat:

* [Az üzembe helyezési módszer](web-sites-deploy.md) -használhatja egy központi telepítési módszer, például a Git, ha egy azonos nevű könyvtárat jön létre, és a központi telepítések kapcsolatos adatokat tartalmazza.
* nodejs - Stdout és az stderr információk rögzítése az alkalmazás összes példányát (teljesülésekor loggingEnabled.)

### <a name="zip-archive"></a>A ZIP-archívum
Egy Zip-archívum a diagnosztikai naplók letöltéséhez az Azure parancssori eszközök a következő parancs használható:

    azure site log download [sitename]

Ez letölti egy **diagnostics.zip** az aktuális könyvtárban található. Ebből az archívumból tartalmazza a következő könyvtárstruktúrát:

* központi telepítések - információ az alkalmazás központi naplózása
* Naplófájlok
  
  * [Az üzembe helyezési módszer](web-sites-deploy.md) -használhatja egy központi telepítési módszer, például a Git, ha egy azonos nevű könyvtárat jön létre, és a központi telepítések kapcsolatos adatokat tartalmazza.
  * nodejs - Stdout és az stderr információk rögzítése az alkalmazás összes példányát (teljesülésekor loggingEnabled.)

### <a name="live-stream-tail"></a>Élő stream (utóhívás)
Egy élő adatfolyam diagnosztikai naplófájl-információk megtekintéséhez az Azure parancssori eszközök a következő parancs használható:

    azure site log tail [sitename]

Ezzel visszatér a naplóeseményeket a kiszolgálón előforduló frissített adatfolyam. Ez az adatfolyam visszaadható központi telepítési információk, valamint az stdout és az stderr adatokat (ha loggingEnabled értéke igaz.)

<a id="nextsteps"></a>

## <a name="next-steps"></a>Következő lépések
Ebben a cikkben megtanulta, engedélyezése, és hozzáférés az Azure diagnosztikai adatokat. Míg ezek az információk ismertetése problémákat, amelyek a az alkalmazás hasznos, akkor használ, vagy Node.js App Service Web Apps által használt verziója nem egyezik a központi telepítési környezetben használt modul probléma mutat.

Információ a modulok használata Azure: [Node.js modulok használata Azure alkalmazásokkal](../nodejs-use-node-modules-azure-apps.md).

Információ az alkalmazás a Node.js verzió megadása: [a Node.js verzió megadása az Azure alkalmazásban].

További információ: a [Node.js fejlesztői központ](/develop/nodejs/).

## <a name="whats-changed"></a>A változások
* Információk a Websites szolgáltatásról az App Service-re való váltásról: [Az Azure App Service és a hatása a meglévő Azure-szolgáltatásokra](http://go.microsoft.com/fwlink/?LinkId=529714)

> [!NOTE]
> Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

[IISNode]: https://github.com/tjanczuk/iisnode
[IISNode információs]: https://github.com/tjanczuk/iisnode#readme
[How to Use The Azure Command-Line Interface]:../cli-install-nodejs.md
[Using Node.js Modules with Azure Applications]: ../nodejs-use-node-modules-azure-apps.md
[a Node.js verzió megadása az Azure alkalmazásban]: ../nodejs-specify-node-version-azure-apps.md

[restart-button]: ./media/web-sites-nodejs-debug/restartbutton.png

