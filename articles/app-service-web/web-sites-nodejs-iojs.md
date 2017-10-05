---
title: "Az io.js használata az Azure App Service Web Apps szolgáltatással"
description: "Ismerje meg, hogyan használható egy webalkalmazást az Azure App Service io.js."
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
ms.openlocfilehash: 4800504e1939a46842d15e8c9d4279a4b9cae787
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-iojs-with-azure-app-service-web-apps"></a>Az io.js használata az Azure App Service Web Apps szolgáltatással
A népszerű csomópont elágazás [io.js] Joyent a Node.js projekthez, beleértve a modell több megnyitása cégirányítási, gyorsabb kiadás ciklust és gyorsabb elfogadását új és kísérleti JavaScript-szolgáltatások különböző különbségek szolgáltatásokat.

Amíg [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webalkalmazások sok Node.js verzió előre telepítve van, lehetővé teszi egy felhasználó által megadott Node.js bináris fájljának. Ez a cikk ismerteti, amelyek két módszer io.js az App Service Web Apps használatának engedélyezése: egy kiterjesztett telepítési parancsfájlt, amely automatikusan konfigurálja az Azure használata a legújabb elérhető io.js, valamint egy bináris io.js manuális feltöltésének használatát. 

<a id="deploymentscript"></a>

## <a name="using-a-deployment-script"></a>A telepítési parancsfájl használatával
A Node.js-alkalmazás központi telepítését, akkor App Service Web Apps kis parancsok futtatásával győződjön meg arról, hogy megfelelően van-e konfigurálva a környezetben több futtatja. A telepítési parancsfájl használatával, ez a folyamat testre szabható letöltését és io.js konfigurációját tartalmazza.

A [io.js telepítési parancsfájl](https://github.com/felixrieseberg/iojs-azure) a Githubon érhető el. Ahhoz, hogy a webalkalmazás az io.js, egyszerűen másolhatja **.deployment**, **Deploy.cmd fájl** és **iisnode.yml fájlt** a gyökérben, az alkalmazás mappájában, és a webalkalmazások telepítését.  

Az első ilyen fájlt, **.deployment**, arra utasítja a webes alkalmazások futtatásához **Deploy.cmd fájl** központi telepítés esetén. Ezt a parancsfájlt a szokásos lépéseket egy Node.js-alkalmazás fut, de is letölti az io.js legújabb verzióját. Végezetül **iisnode.yml fájlt** csak a letöltött io.js használata bináris helyett egy előre telepített Node.js bináris webalkalmazások konfigurálása.

> [!NOTE]
> A parancsfájl és a használt io.js bináris, egyszerűen telepítse újra az alkalmazás - letölti egy új verziója io.js minden telepítik az alkalmazást egy alkalommal.
> 
> 

<a id="manualinstallation"></a>

## <a name="using-manual-installation"></a>Manuális telepítés
A manuális telepítés olyan egyéni io.js verzió csak két folyamatát foglalja magában. Első lépésként töltse le a **win-x64** bináris közvetlenül a [io.js terjesztési]. Szükség van a két fájlt - **iojs.exe** és **iojs.lib**. Mentse mindkét fájlt, hogy egy mappát a web app alkalmazásban például **bin/iojs**.

Webalkalmazások használandó konfigurálása **iojs.exe** helyett egy előre telepített csomópont, hozzon létre egy **iisnode.yml fájlt** fájlt az alkalmazás gyökérkönyvtárában, és adja hozzá a következő sort.

    nodeProcessCommandLine: "D:\home\site\wwwroot\bin\iojs\iojs.exe"

<a id="nextsteps"></a>

## <a name="next-steps"></a>Következő lépések
Ebben a cikkben megtanulta, hogyan használhatja az App Service Web Apps, mind io.js, valamint a manuális telepítés megadott üzembe helyezési parancsfájlok. 

> [!NOTE]
> IO.js nehéz fejlesztési, és Node.js gyakoribb. A Node.js modulok száma nem működik együtt io.js - adjon tájékoztatást [a Githubon io.js] hibaelhárításhoz.
> 
> 

## <a name="whats-changed"></a>A változások
* Információk a Websites szolgáltatásról az App Service-re való váltásról: [Az Azure App Service és a hatása a meglévő Azure-szolgáltatásokra](http://go.microsoft.com/fwlink/?LinkId=529714)

> [!NOTE]
> Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

[io.js]: https://iojs.org
[io.js terjesztési]: https://iojs.org/dist/
[a Githubon io.js]: https://github.com/iojs/io.js
[io.js Deployment Script]: https://github.com/felixrieseberg/iojs-azure
