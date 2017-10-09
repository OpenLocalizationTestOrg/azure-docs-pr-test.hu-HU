---
title: "Webalkalmazás Linux aaaIntroduction tooAzure |} Microsoft Docs"
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
ms.openlocfilehash: 43b9865ade251909a77429eb3e18fe0bcaac3bde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-web-app-on-linux"></a>Bevezetés tooAzure webalkalmazás Linux rendszeren

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

## <a name="overview"></a>Áttekintés
Az ügyfelek használhatják a webalkalmazás Linux toohost webalkalmazásokban Linux rendszeren natív módon a támogatott alkalmazás verem. hello következő szakasz hello alkalmazás verem jelenleg támogatott. 

## <a name="features"></a>Szolgáltatások
Webes alkalmazás Linux rendszeren jelenleg a következő alkalmazás verem hello támogatja:

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

* Az ügyfelek is méretezhető webalkalmazások felfelé és lefelé az App Service-csomag hello szintjének módosítása
* Ügyfelek horizontális felskálázás alkalmazások és azok metódust hello határain belül több alkalmazás példányának futtatása

A Kudu, néhány alapvető hello-funkciókat:

* Környezetekben
* Központi telepítés
* Alapszintű konzol
* SSH

A devops:

* Átmeneti környezetek
* ACR és DockerHub CI/CD

## <a name="limitations"></a>Korlátozások
hello Azure-portálon megjelenítése csak olyan funkciókat, amelyek jelenleg webalkalmazás Linux rendszeren működik és elrejtése hello rest. Engedélyezzük a szolgáltatásokat, mivel ezek láthatók lesznek a hello portál.

Egyes szolgáltatások, például a virtuális hálózati integráció, az Azure Active Directory vagy harmadik fél hitelesítést és a Kudu helyhez kiterjesztések, még nem érhető el. Ezek a funkciók érhetők el, ha a dokumentáció és hello változásaira vonatkozó blog frissítjük.

A nyilvános előzetes verzió jelenleg csak a következő régiókban hello érhető el:

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

Web Apps Linux csak akkor támogatott a hello dedikált app service-csomagok, és nem rendelkezik egy ingyenes vagy közös réteg. App Service-csomagokról a normál és a Linux-webalkalmazások is kölcsönösen kizárja egymást, így a nem Linux app service-csomag nem hozható létre egy Linux-webalkalmazást.

Web Apps Linux létre kell hozni egy erőforráscsoport, amely nem tartalmazza a hello nem Linux webalkalmazások ugyanabban a régióban.

## <a name="troubleshooting"></a>Hibaelhárítás ##

Az alkalmazás toostart meghibásodik, vagy az alkalmazás kívánt toocheck hello naplózási, ellenőrizze a Docker bejelentkezik hello naplófájlok directory hello. Ez a könyvtár az SCM-hely vagy FTP-n keresztül érheti el.
toolog hello `stdout` és `stderr` a tárolóból tooenable kell **Docker-tároló naplózási** alatt **diagnosztikai naplók**.

![Naplózás engedélyezése][2]

![A Kudu tooview Docker-naplók segítségével][1]

Van-e hozzáférési hello SCM helyet **speciális eszközök** a hello **Fejlesztőeszközök** menü.

## <a name="next-steps"></a>Következő lépések
Tekintse meg a következő hivatkozások tooget az App Service Linux hello. Kérdések és problémákat is közzétesz a [fórumban](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).

* [Hogyan toouse egyéni Docker kép Linux Azure webalkalmazás számára](app-service-linux-using-custom-docker-image.md)
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