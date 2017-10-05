---
title: "Bevezetés az Azure-webalkalmazásban Linux |} Microsoft Docs"
description: "További információk a Linux Azure-webalkalmazásban."
keywords: az Azure app service, linux, oss
services: app-service
documentationcenter: 
author: naziml
manager: erikre
editor: 
ms.assetid: bc85eff6-bbdf-410a-93dc-0f1222796676
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2017
ms.author: naziml;wesmc
ms.openlocfilehash: 0870f811845ec7c705da13f08abdfa762d25b209
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="introduction-to-azure-web-app-on-linux"></a>Bevezetés az Azure-webalkalmazásban Linux rendszeren

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

## <a name="overview"></a>Áttekintés
Felhasználók használhatják a webalkalmazás Linux gazdagép webalkalmazások Linux rendszeren natív módon a támogatott alkalmazás verem. A következő szakasz az alkalmazás verem jelenleg támogatott. 

## <a name="features"></a>Szolgáltatások
Webes alkalmazás Linux rendszeren jelenleg a következő alkalmazás verem támogatja:

* Node.js
    * 4.4
    * 4.5
    * 6.2
    * 6.6
    * 6.9
    * 6.10
    * 6.11
    * 8.0
    * 8.1
* PHP
    * 5.6
    * 7.0
* A .NET core
    * 1.0
    * 1.1
* Ruby
    * 2.3

Központi telepítését az alkalmazások használatával:

* FTP
* Helyi Git
* GitHub
* Bitbucket

Az alkalmazás méretezés:

* Az ügyfelek is méretezhető webalkalmazások felfelé és lefelé az App Service-csomag szintjének módosítása
* Ügyfelek horizontális felskálázás alkalmazások és azok metódust belül több alkalmazás példányának futtatása

A Kudu, néhány alapvető funkcióit:

* Környezetekben
* Központi telepítés
* Alapszintű konzol
* SSH

A devops:

* Átmeneti környezetek
* ACR és DockerHub CI/CD

## <a name="limitations"></a>Korlátozások
Az Azure-portálon megjelenítése csak olyan funkciókat, amelyek a webalkalmazás Linux rendszeren jelenleg működik, és a többi elrejtése. Engedélyezzük a szolgáltatásokat, azok lesznek láthatók a portálon.

Egyes szolgáltatások, például a virtuális hálózati integráció, az Azure Active Directory vagy harmadik fél hitelesítést és a Kudu helyhez kiterjesztések, még nem érhető el. Ezek a funkciók érhetők el, ha a dokumentáció és a változásokról blog frissítjük.

A nyilvános előzetes verzió érhető el jelenleg csak a következő régióban:

* USA nyugati régiója
* USA keleti régiója
* Nyugat-Európa
* Észak-Európa
* USA déli középső régiója
* USA északi középső régiója
* Délkelet-Ázsia
* Kelet-Ázsia
* Kelet-Ausztrália
* Kelet-Japán
* Dél-Brazília
* Dél-India

Web Apps Linux csak akkor támogatott a kijelölt app service-csomagok, és nem rendelkezik egy ingyenes vagy közös réteg. App Service-csomagokról a normál és a Linux-webalkalmazások is kölcsönösen kizárja egymást, így a nem Linux app service-csomag nem hozható létre egy Linux-webalkalmazást.

Web Apps Linux egy erőforráscsoport, amely nem tartalmazza a nem Linux webalkalmazások ugyanabban a régióban kell létrehozni.

## <a name="troubleshooting"></a>Hibaelhárítás ##

Az alkalmazás nem indul el, vagy a naplózás a alkalmazás ellenőrizni kívánja, hogy a Docker naplózza a naplófájlok könyvtárban. Ez a könyvtár az SCM-hely vagy FTP-n keresztül érheti el.
A napló a `stdout` és `stderr` a tárolóból, engedélyezni kell a **Docker-tároló naplózási** alatt **diagnosztikai naplók**.

![Naplózás engedélyezése][2]

![A Kudu segítségével Docker naplók megtekintése][1]

Érheti el az SCM helyet **speciális eszközök** a a **Fejlesztőeszközök** menü.

## <a name="next-steps"></a>Következő lépések
Tekintse meg az alábbi hivatkozásokra kattintva az App Service Linux rendszeren. Kérdések és problémákat is közzétesz a [fórumban](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).

* [Egyéni Docker-lemezkép használata Linux Azure webalkalmazás számára](app-service-linux-using-custom-docker-image.md)
* [PM2 Configuration for Node.js Linux Azure Web App használatával](app-service-linux-using-nodejs-pm2.md)
* [.NET Core használatát az Azure App Service webalkalmazásba Linux rendszeren](app-service-linux-using-dotnetcore.md)
* [Ruby használatát az Azure App Service webalkalmazásba Linux rendszeren](app-service-linux-ruby-get-started.md)
* [Az Azure App Service webalkalmazásba Linux – gyakori kérdések](app-service-linux-faq.md)
* [Azure Web Apps Linux SSH támogatása](./app-service-linux-ssh-support.md)
* [Átmeneti környezet az Azure App Service beállítása](./web-sites-staged-publishing.md)
* [Docker Hub folyamatos üzembe helyezés Linux Azure-webalkalmazásban](./app-service-linux-ci-cd.md)

<!--Image references-->
[1]: ./media/app-service-linux-intro/kudu-docker-logs.png
[2]: ./media/app-service-linux-intro/logging.png