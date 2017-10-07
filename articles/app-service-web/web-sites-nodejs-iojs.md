---
title: az Azure App Service Web Apps aaaHow toouse io.js
description: "Megtudhatja, hogyan toouse egy webalkalmazást az io.js az Azure App Service-ben."
services: app-service\web
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: d6320725-ffcb-4ad7-ba63-fc72fa2f2808
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: tarcher
ms.openlocfilehash: 5dfdac37546b36bc91ab43d9e0a39c2126b4fa9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-iojs-with-azure-app-service-web-apps"></a>Hogyan toouse io.js az Azure App Service Web Apps
hello népszerű csomópont elágazás [io.js] különböző különbségek tooJoyent Node.js projekt, beleértve a modell több megnyitása cégirányítási, gyorsabb kiadás ciklust és gyorsabb elfogadását új és kísérleti JavaScript-szolgáltatások szolgáltatásait.

Amíg [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webalkalmazások sok Node.js verzió előre telepítve van, lehetővé teszi egy felhasználó által megadott Node.js bináris fájljának. Ez a cikk ismerteti, amelyek két módszer az App Service Web Apps io.js hello használatának engedélyezése: hello egy kiterjesztett telepítési parancsfájlt, amely automatikusan konfigurálja az Azure toouse hello legújabb elérhető io.js verzió használatát, valamint az io.js bináris hello manuális feltöltése. 

<a id="deploymentscript"></a>

## <a name="using-a-deployment-script"></a>A telepítési parancsfájl használatával
A Node.js-alkalmazás központi telepítését, akkor App Service Web Apps megfelelően van-e konfigurálva, hogy a környezet hello tooensure kis parancsok számos futtatja. A telepítési parancsfájl használatával, ez a folyamat csak testreszabott tooinclude hello letöltési és io.js konfigurációját.

Hello [io.js telepítési parancsfájl](https://github.com/felixrieseberg/iojs-azure) a Githubon érhető el. a webalkalmazásban tooenable io.js egyszerűen másolhatja **.deployment**, **Deploy.cmd fájl** és **iisnode.yml fájlt** toohello gyökérmappájában az alkalmazás mappájában és tooWeb alkalmazások telepítése.  

hello első fájl, **.deployment**, arra utasítja a webes alkalmazások toorun **Deploy.cmd fájl** központi telepítés esetén. Ez a parancsfájl minden hello szokásos lépései a Node.js-alkalmazás fut, de is letölti az io.js hello legújabb verzióját. Végezetül **iisnode.yml fájlt** webalkalmazások toouse letöltött csak hello io.js bináris helyett egy előre telepített Node.js bináris konfigurálja.

> [!NOTE]
> tooupdate hello használt io.js bináris, egyszerűen telepítse újra az alkalmazás - hello parancsfájl letölti azokat a io.js minden egy alkalommal hello alkalmazás központi telepítése egy új verziója.
> 
> 

<a id="manualinstallation"></a>

## <a name="using-manual-installation"></a>Manuális telepítés
manuális telepítés hello olyan egyéni io.js verzió csak két folyamatát foglalja magában. Első lépésként töltse le a hello **win-x64** bináris közvetlenül a hello [io.js terjesztési]. Szükség van a két fájlt - **iojs.exe** és **iojs.lib**. Egyaránt mentés-fájlok tooa mappa a web app alkalmazásban például **bin/iojs**.

tooconfigure webalkalmazások toouse **iojs.exe** helyett egy előre telepített csomópont, hozzon létre egy **iisnode.yml fájlt** hello gyökerében az alkalmazás fájlt, és adja hozzá a következő sor hello.

    nodeProcessCommandLine: "D:\home\site\wwwroot\bin\iojs\iojs.exe"

<a id="nextsteps"></a>

## <a name="next-steps"></a>Következő lépések
Ebben a cikkben megtanulta, hogyan toouse io.js az App Service Web Apps, mind a megadott üzembe helyezési parancsfájlok, valamint a manuális telepítés. 

> [!NOTE]
> IO.js nehéz fejlesztési, és Node.js gyakoribb. A Node.js modulok száma nem működik együtt io.js - adjon tájékoztatást [a Githubon io.js] hibaelhárításhoz.
> 
> 

## <a name="whats-changed"></a>A változások
* Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)

> [!NOTE]
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

[io.js]: https://iojs.org
[io.js terjesztési]: https://iojs.org/dist/
[a Githubon io.js]: https://github.com/iojs/io.js
[io.js Deployment Script]: https://github.com/felixrieseberg/iojs-azure
