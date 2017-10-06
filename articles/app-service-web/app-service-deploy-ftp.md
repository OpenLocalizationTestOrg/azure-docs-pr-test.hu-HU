---
title: "aaaDeploy az alkalmazás tooAzure App Service FTP/S használatával |} Microsoft Docs"
description: "Megtudhatja, hogyan toodeploy az alkalmazás tooAzure App Service segítségével FTP- vagy FTPS."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: ae78b410-1bc0-4d72-8fc4-ac69801247ae
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2016
ms.author: cephalin;dariac
ms.openlocfilehash: 318ae79d4fae269f853ea5c3ce28353b0864131e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-app-tooazure-app-service-using-ftps"></a>Az alkalmazás tooAzure használatával az FTP/S App Service telepítése

Ez a cikk bemutatja, hogyan toouse FTP vagy FTPS toodeploy a webalkalmazást, mobil-háttéralkalmazás vagy API-alkalmazás túl[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).

hello FTP/S végpont az alkalmazás már aktív. Szükséges tooenable FTP/mp telepítése nem igényel konfigurálást.

> [!IMPORTANT]
> Folyamatosan jegyében lépéseket tooimprove Microsoft Azure Platform biztonsági. A folyamatban lévő elérhető részeként webalkalmazások frissítés tervezett Németország központi és Németország szerepel. Ennek során Web Apps letiltása telepítések hello egyszerű szöveges FTP protokoll használatát fog lehet. A javaslat tooour ügyfeleink tooswitch tooFTPS telepítésekhez. Ez a frissítés, amely a 9/5 tervezett során nem minden megszűnésének tooyour szolgáltatás várhatóan. Nagyra értékeljük ebből a törekvésből támogatja.

<a name="step1"></a>
## <a name="step-1-set-deployment-credentials"></a>1. lépés: Az üzembe helyezési hitelesítő adatok beállítása

tooaccess hello FTP-kiszolgáló beállítása az alkalmazáshoz, először üzembe helyezési hitelesítő adatokat. 

tooset vagy visszaállítása az üzembe helyezési hitelesítő adatokat, lásd: [Azure App Service üzembe helyezési hitelesítő adatok](app-service-deployment-credentials.md). Ez az oktatóanyag bemutatja, hello felhasználói szintű hitelesítő adatokat használjanak.

## <a name="step-2-get-ftp-connection-information"></a>2. lépés: FTP-kiszolgáló kapcsolati adatainak lekérése

1. A hello [Azure-portálon](https://portal.azure.com), nyissa meg az alkalmazás [erőforráspanelen](../azure-resource-manager/resource-group-portal.md#manage-resources).
2. Válassza ki **áttekintése** hello bal oldali menüben, majd jegyezze fel hello értékeinek **FTP vagy üzembe helyező felhasználó**, **FTP-állomás neve**, és **állomásnév FTPS**. 

    ![FTP-kapcsolat információi](./media/web-sites-deploy/FTP-Connection-Info.PNG)

    > [!NOTE]
    > Hello **FTP vagy üzembe helyező felhasználó** felhasználói érték által megjelenített hello hello alkalmazás neve például rendelés tooprovide megfelelő környezetben hello FTP-kiszolgáló Azure-portálon.
    > Található hello ugyanazokat az információkat, amikor kiválaszt **tulajdonságok** hello bal oldali menüben. 
    >
    > Emellett hello telepítési jelszó soha nem jelenik meg. Ha a központi telepítés jelszavát, lépjen vissza túl[1. lépés](#step1) és a központi telepítés jelszó.
    >
    >

## <a name="step-3-deploy-files-tooazure"></a>3. lépés: A fájlok tooAzure telepítése

1. Az FTP-ügyfél ([Visual Studio](https://www.visualstudio.com/vs/community/), [FileZilla](https://filezilla-project.org/download.php?type=client)stb), tooconnect tooyour app összegyűjtött információkat felhasználva hello kapcsolat.
3. Másolja a fájlokat és a megfelelő könyvtár struktúra toohello [ **/hely/wwwroot** directory](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) az Azure-ban (vagy hello **/hely/wwwroot/App_Data/feladatok/** a könyvtár Webjobs-feladatok).
4. Tallózás tooyour alkalmazás URL-cím tooverify hello alkalmazás megfelelően fut. 

> [!NOTE] 
> Eltérően [Git-alapú telepítések](app-service-deploy-local-git.md), FTP-telepítés nem támogatja a központi telepítés automatizálása a következő hello: 
>
> - a függőségi visszaállítási (például a NuGet, NPM, PIP és szerkesztő automatizálása)
> - a .NET bináris fájl fordítás
> - a Web.config fájl. generációs (itt egy [Node.js példa](https://github.com/projectkudu/kudu/wiki/Using-a-custom-web.config-for-Node-apps))
> 
> Ön kell visszaállítani, felépítéséhez, és hozza létre ezeket manuálisan a helyi számítógépen a szükséges fájlokat és együtt az alkalmazás központi telepítésére.
>
>

## <a name="next-steps"></a>Következő lépések

Összetettebb központi telepítési forgatókönyve esetén próbálja [központi telepítése a Git tooAzure](app-service-deploy-local-git.md). Git-alapú üzembe helyezési tooAzure lehetővé teszi a verziókövetés, a csomag visszaállítás, az MSBuild és több.

## <a name="more-resources"></a>További források

* [PHP-MySQL-webalkalmazás létrehozása és központi telepítése FTP használatával](web-sites-php-mysql-deploy-use-ftp.md).
* [Az Azure App Service üzembe helyezési hitelesítő adatok](app-service-deploy-ftp.md)
