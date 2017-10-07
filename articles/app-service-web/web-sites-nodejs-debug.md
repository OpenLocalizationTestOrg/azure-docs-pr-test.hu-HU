---
title: "aaaHow toodebug Node.js-webalkalmazás az Azure App Service-ben"
description: "Ismerje meg, hogyan toodebug egy Node.js webalkalmazás az Azure App Service-ben."
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
ms.openlocfilehash: 888ec5c3f92cfc3aeea4ea86005b9b6a0d1306ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodebug-a-nodejs-web-app-in-azure-app-service"></a>Hogyan toodebug egy Node.js webalkalmazás az Azure App Service-ben
Azure biztosít a hibakeresés üzemeltetett Node.js-alkalmazások beépített diagnosztika tooassist [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webalkalmazások. Ebből a cikkből megtudhatja, hogyan stdout és az stderr-en, a hello böngészőben megjelenített hibaadatok és a hogyan toodownload és nézet naplófájlok tooenable naplózása.

Az Azure-platformon futó Node.js alkalmazások diagnosztika által biztosított [IISNode]. Ez a cikk ismerteti, amelyek a hello leggyakrabban használt beállítások diagnosztikai adatok gyűjtésének, amíg nem biztosít teljes IISNode való munkához. Az IISNode munkavégzés további információkért lásd: hello [IISNode információs] a Githubon.

<a id="enablelogging"></a>

## <a name="enable-logging"></a>Naplózás engedélyezése
Alapértelmezés szerint egy App Service webalkalmazásba csak telepítések, például a Git használatával webes alkalmazás telepítésekor diagnosztikai adatait rögzíti. Ez az információ akkor hasznos, ha problémák merülnek fel a hivatkozott modul telepítésekor hiba például a telepítés során **package.json**, vagy ha egy egyéni telepítési parancsfájlt használ.

tooenable hello naplózás az stdout és az stderr adatfolyamok, létre kell hoznia egy **iisnode.yml fájlt** a(z) hello a Node.js-alkalmazás gyökérkönyvtárában, és adja hozzá a következő hello:

    loggingEnabled: true

Ez lehetővé teszi a stderr-en és a Node.js-alkalmazás az stdout hello naplózása.

Hello **iisnode.yml fájlt** fájl is lehet használt toocontrol e egyéni hibaüzenetek vagy fejlesztői hibák visszakerülnek toohello böngésző, ha hiba történik. tooenable fejlesztői hibák, adja hozzá a következő sor toohello hello **iisnode.yml fájlt** fájlt:

    devErrorsEnabled: true

Ha engedélyezi ezt a beállítást, az IISNode utolsó 64K továbbított toostderr helyett egy rövid hiba, például a "egy belső kiszolgálóhiba történt" hello ad vissza.

> [!NOTE]
> DevErrorsEnabled akkor hasznos, ha problémák diagnosztizálása a fejlesztés során, amíg a termelési környezetben engedélyezné azt az elküldött tooend felhasználók fejlesztési hibákat eredményezhet.
> 
> 

Ha hello **iisnode.yml fájlt** fájl már nem létezett az alkalmazáson belül, újra kell indítania a webalkalmazás frissítése hello alkalmazás közzététele után. Egyszerűen módosítani egy meglévő beállítások **iisnode.yml fájlt** korábban közzétett fájl, újraindításra nincs szükség.

> [!NOTE]
> Ha a webalkalmazás létrejött hello Azure parancssori eszközök, vagy az Azure PowerShell-parancsmagok, egy alapértelmezett **iisnode.yml fájlt** fájl automatikusan létrejön.
> 
> 

toorestart hello web app alkalmazásban válassza hello webalkalmazás hello [Azure Portal](https://portal.azure.com), és kattintson a **indítsa újra a** gombra:

![Indítsa újra a gomb][restart-button]

Ha hello Azure parancssori eszköz a fejlesztési környezetben van telepítve, a következő parancs toorestart hello webalkalmazás hello is használhatja:

    azure site restart [sitename]

> [!NOTE]
> Míg loggingEnabled és devErrorsEnabled leggyakrabban használt hello iisnode.yml fájlt konfigurációs beállítások diagnosztikai adatok rögzítéséhez, iisnode.yml fájlt lehet használt tooconfigure számos lehetőséget a üzemeltetési környezet. Hello konfigurációs beállítások teljes listáját lásd: hello [iisnode_schema.xml](https://github.com/tjanczuk/iisnode/blob/master/src/config/iisnode_schema.xml) fájlt.
> 
> 

<a id="viewlogs"></a>

## <a name="accessing-logs"></a>Naplók elérése
Diagnosztikai naplók elérhető háromféleképpen; Hello File Transfer Protocol (FTP), egy Zip-archívum letöltése használatával, vagy mint egy élő adatfolyam hello napló (más néven utóhívás) frissítése. A Zip-archívum hello hello naplófájlok letöltése vagy megtekinti a hello élő adatfolyam szükséges hello Azure parancssori eszközök. Ezek a hello a következő parancs használatával telepíthető:

    npm install azure-cli -g

A telepítést követően hello eszközök segítségével férhetők el hello "azure" parancsot. hello parancssori eszközöket kell konfigurált toouse az Azure-előfizetéshez. Hogyan tooaccomplish ennek a feladatnak a információkért lásd: hello **hogyan toodownload-importálási közzétételi beállítások** hello szakasza [hogyan tooUse hello Azure parancssori eszközök](../xplat-cli-connect.md) cikk.

### <a name="ftp"></a>FTP
tooaccess hello diagnosztikai adatokat keresztül FTP, látogasson el a hello [Azure Portal](https://portal.azure.com), a webes alkalmazást, majd válassza ki és hello **IRÁNYÍTÓPULT**. A hello **Gyorshivatkozások** című szakaszba hello **FTP diagnosztikai naplók** és **diagnosztikai naplók FTPS** hivatkozásokra kattintva belépési toohello naplók hello FTP protokoll használatával.

> [!NOTE]
> Ha nem korábban konfigurált felhasználónevet és jelszót FTP- vagy a központi telepítés, ehhez a hello **gyors üzembe helyezés** kezelése lap választásával **üzembe helyezési hitelesítő adatok beállítása**.
> 
> 

hello hello irányítópulton visszaadott FTP URL van hello **naplófájlok** könyvtár, amely tartalmazza a következő alkönyvtár hello:

* [Az üzembe helyezési módszer](web-sites-deploy.md) -használhatja egy központi telepítési módszer, például a Git, ha egy könyvtárat hello ugyanazzal a névvel jön létre, és információt tartalmaz a kapcsolódó toodeployments.
* nodejs - Stdout és az stderr információk rögzítése az alkalmazás összes példányát (teljesülésekor loggingEnabled.)

### <a name="zip-archive"></a>A ZIP-archívum
Zip-archívumból hello diagnosztikai naplók, a következő parancsot a hello Azure parancssori eszközök használata hello toodownload:

    azure site log download [sitename]

Ez letölti egy **diagnostics.zip** hello aktuális könyvtárban található. Ebből az archívumból tartalmazza a következő könyvtárstruktúrát hello:

* központi telepítések - információ az alkalmazás központi naplózása
* Naplófájlok
  
  * [Az üzembe helyezési módszer](web-sites-deploy.md) -használhatja egy központi telepítési módszer, például a Git, ha egy könyvtárat hello ugyanazzal a névvel jön létre, és információt tartalmaz a kapcsolódó toodeployments.
  * nodejs - Stdout és az stderr információk rögzítése az alkalmazás összes példányát (teljesülésekor loggingEnabled.)

### <a name="live-stream-tail"></a>Élő stream (utóhívás)
diagnosztikai szereplő információkat, a következő parancsot a hello Azure parancssori eszközök használata hello élő adatfolyam tooview:

    azure site log tail [sitename]

Ezzel visszatér a naplóeseményeket hello kiszolgálón előforduló frissített adatfolyam. Ez az adatfolyam visszaadható központi telepítési információk, valamint az stdout és az stderr adatokat (ha loggingEnabled értéke igaz.)

<a id="nextsteps"></a>

## <a name="next-steps"></a>Következő lépések
Az e cikkben megtanulta, hogyan meg tooenable és hozzáférés az Azure diagnosztikai adatokat. Amíg ez az információ a ismertetése előforduló problémákat az alkalmazással, akkor lehetséges, hogy pont tooa probléma használ modul vagy a Node.js App Service Web Apps által használt hello verzió eltér hello egyet a központi telepítésben használja környezet.

Információ a modulok használata Azure: [Node.js modulok használata Azure alkalmazásokkal](../nodejs-use-node-modules-azure-apps.md).

Információ az alkalmazás a Node.js verzió megadása: [a Node.js verzió megadása az Azure alkalmazásban].

További információkért lásd még: hello [Node.js fejlesztői központ](/develop/nodejs/).

## <a name="whats-changed"></a>A változások
* Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)

> [!NOTE]
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

[IISNode]: https://github.com/tjanczuk/iisnode
[IISNode információs]: https://github.com/tjanczuk/iisnode#readme
[How tooUse hello Azure Command-Line Interface]:../cli-install-nodejs.md
[Using Node.js Modules with Azure Applications]: ../nodejs-use-node-modules-azure-apps.md
[a Node.js verzió megadása az Azure alkalmazásban]: ../nodejs-specify-node-version-azure-apps.md

[restart-button]: ./media/web-sites-nodejs-debug/restartbutton.png

