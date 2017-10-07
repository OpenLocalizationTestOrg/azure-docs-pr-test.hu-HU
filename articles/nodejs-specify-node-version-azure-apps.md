---
title: "a Node.js verzió aaaSpecifying"
description: "Ismerje meg, hogyan toospecify hello Node.js Azure-webhelyek és Felhőszolgáltatások által használt verziója"
services: 
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: d0e15278-2ab4-4ec8-8256-913839c6d5ef
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: tarcher
ms.openlocfilehash: 09c27bfc43c132b6d66f9a2943179e06ee75bedc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="specifying-a-nodejs-version-in-an-azure-application"></a>A Node.js verzió megadása az Azure alkalmazásban
Amikor egy Node.js-alkalmazást üzemeltet, érdemes lehet az alkalmazás által egy adott verziójához Node.js tooensure. Nincsenek számos módon tooaccomplish Ez az Azure-platformon futó alkalmazások.

## <a name="default-versions"></a>Alapértelmezett verziók
Azure által biztosított hello Node.js verzió folyamatosan frissülnek. Hacsak másként nincs megadva, alapértelmezett verzió hello megadott hello `WEBSITE_NODE_DEFAULT_VERSION` környezeti változó fogja használni. toooverride Ez az alapértelmezett érték, ez a cikk a következő szakaszokban kövesse hello lépéseket

> [!NOTE]
> Ha az alkalmazást egy Azure-Felhőszolgáltatásban (webes vagy feldolgozói szerepkör) üzemeltet, és hello először hello alkalmazást telepített, Azure kísérli meg toouse hello Node.js ugyanazon verzióját telepítette a fejlesztési környezet Ha, akkor megfelelő hello alapértelmezett verziójú, amely az Azure-on.
>
>

## <a name="versioning-with-packagejson"></a>A package.json Versioning
Adja hozzá a következő tooyour hello használt Node.js toobe hello verzióját is megadhat **package.json** fájlt:

    "engines":{"node":version}

Ha *verzió* hello adott verziójához számú toouse van. Például megadhatja verziójához, összetettebb feltételek:

    "engines":{"node": "0.6.22 || 0.8.x"}

Mivel 0.6.22 nem érhető el a gazdakörnyezet hello hello-verziók valamelyikét, hello hello 0,8 adatsorozat elérhető lesz a legújabb verziót használja - 0.8.4.

## <a name="versioning-websites-with-app-settings"></a>Az alkalmazásbeállítások Versioning webhelyek
Ha egy webhelyen lévő hello alkalmazást üzemeltet, beállíthatja azt a hello környezeti változó **WEBSITE_NODE_DEFAULT_VERSION** toohello kívánt verziójával.

## <a name="versioning-cloud-services-with-powershell"></a>A PowerShell-lel Versioning Cloud Services csomag
Ha egy felhőalapú szolgáltatás hello alkalmazást üzemeltető, és az Azure PowerShell hello alkalmazást telepít, felülbírálhatja hello segítségével hello alapértelmezett Node.js verziót **Set-AzureServiceProjectRole** PowerShell-parancsmagot. Példa:

    Set-AzureServiceProjectRole WebRole1 Node 0.8.4

Megjegyzés: hello fent utasítás hello paraméterei kis-és nagybetűket.  Ellenőrizheti a Node.js hello verziójának kijelölt hello ellenőrzésével **motorok** tulajdonság a szerepkör **package.json**.

Is használhatja a hello **Get-AzureServiceProjectRoleRuntime** tooretrieve az Node.js verzió érhető el a felhőalapú szolgáltatásként üzemeltetett alkalmazások listáját.  Mindig győződjön meg arról a projekt függ Node.js hello verzióját ezen a listán.

## <a name="using-a-custom-version-with-azure-websites"></a>Az Azure Websitesra egyéni verziójával
Amíg az Azure biztosít a Node.js több alapértelmezett verzióját, érdemes lehet toouse olyan verzióra, amely alapértelmezés szerint nincs megadva. Ha az alkalmazás üzemel, egy Azure-webhelyre, ez elvégezhető hello segítségével **iisnode.yml fájlt** fájlt. hello következő lépéseit ismerteti hello folyamatot, amely az Azure-webhely Node.Js egyéni verzióját használja:

1. Hozzon létre egy új könyvtárat, és hozzon létre egy **server.js** hello könyvtárban lévő fájlt. Hello **server.js** fájl hello következőket kell tartalmaznia:

        var http = require('http');
        http.createServer(function(req,res) {
          res.writeHead(200, {'Content-Type': 'text/html'});
          res.end('Hello from Azure running node version: ' + process.version + '</br>');
        }).listen(process.env.PORT || 3000);

    Ez megjeleníti hello Node.js verziót használják, ha a felhasználó hello webhelyet.
2. Hozzon létre egy új webhelyet és megjegyzés hello hello hely nevét. Például a következő hello használ hello [Azure parancssori eszközök] toocreate nevű új Azure webhely **segítségével**, és engedélyez egy Git-tárház hello webhelyhez.

        azure site create mywebsite --git
3. Hozzon létre egy új könyvtárat nevű **bin** hello tartalmazó hello könyvtár gyermekeként **server.js** fájlt.
4. Töltse le az adott verziójához hello **node.exe** (hello Windows-verzió) toouse kíván az alkalmazással. Például a következő felhasználási hello **curl** toodownload verzió 0.8.1:

        curl -O http://nodejs.org/dist/v0.8.1/node.exe

    Mentse a hello **node.exe** hello fájlból **bin** korábban létrehozott mappába.
5. Hozzon létre egy **iisnode.yml fájlt** hello fájlt azonos könyvtárhoz, mint a hello **server.js** fájlt, és adja hozzá a következő tartalom toohello hello **iisnode.yml fájlt** fájlt:

        nodeProcessCommandLine: "D:\home\site\wwwroot\bin\node.exe"

    Ez az elérési út, ahol hello **node.exe** fájlt a projektben található után az alkalmazás toohello Azure webhelyen közzétett.
6. Az alkalmazás közzétételére. Például egy új webhely korábbi hello--git paraméterrel létrehozott, mert hello következő parancsok fog hello alkalmazás fájlok toomy helyi Git-tárház hozzáadása, és majd küldje le őket toohello webhely tárház:

        git add .
        git commit -m "testing node v0.8.1"
        git push azure master

    Miután közzétette hello alkalmazás, hello webhely megnyitása a böngészőben. Megjelenik egy üzenet "Hello Azure futó csomópont verziójáról: v0.8.1".

## <a name="next-steps"></a>Következő lépések
Most, hogy megértse, hogyan Node.js toospecify hello verzióját használja-e az alkalmazás, megtudhatja, hogyan túl[a modulok], [és üzembe egy Node.js webhely](app-service-web/app-service-web-get-started-nodejs.md), és [hogyan toouse hello Azure A Mac és Linux parancssori eszközök].

További információkért lásd: hello [Node.js fejlesztői központ](https://azure.microsoft.com/develop/nodejs/).

[hogyan toouse hello Azure A Mac és Linux parancssori eszközök]:cli-install-nodejs.md
[Azure parancssori eszközök]:cli-install-nodejs.md
[a modulok]: nodejs-use-node-modules-azure-apps.md
[build and deploy a Node.js Web Site]: app-service-web/app-service-web-get-started-nodejs.md
