---
title: "egy helyi Docker-tároló aaaDebugging alkalmazások |} Microsoft Docs"
description: "Megtudhatja, hogyan toomodify egy alkalmazást, amely futtatja a helyi Docker-tároló hello tároló szerkesztési és frissítési keresztül frissítse, és állítsa a hibakeresés töréspontok"
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: 
ms.assetid: 480e3062-aae7-48ef-9701-e4f9ea041382
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 07/22/2016
ms.author: mlearned
ms.openlocfilehash: ff64e62fbb93901a29b5496bd5e17d2c4ea5ca99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="debugging-apps-in-a-local-docker-container"></a>Alkalmazások hibakeresése a helyi Docker-tárolóban
## <a name="overview"></a>Áttekintés
Visual Studio eszközök hello Docker egy egységes módon toodevelop a biztosít, és az alkalmazást helyileg egy Linux Docker-tároló ellenőrzése.
Toorestart hello tároló nincs minden egyes módosítani egy kódot.
Ez a cikk bemutatja, hogyan toouse hello "Szerkesztése és a frissítés" funkció toostart az ASP.NET Core webalkalmazás egy helyi Docker-tároló, a szükséges módosításokat, és ezt követően frissítse hello böngésző toosee ezeket a módosításokat.
Ez a cikk azt is bemutatja, hogyan tooset töréspontokat a hibakereséshez.

> [!NOTE]
> Windows-tároló támogatása hamarosan egy későbbi kiadásban
>
>

## <a name="prerequisites"></a>Előfeltételek
a következő eszközök hello telepítve kell lennie.

* [Visual Studio legújabb verziójának](https://www.visualstudio.com/downloads/)
* [A Microsoft ASP.NET Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)

Docker toorun tárolók helyileg, szüksége lesz egy helyi docker-ügyfél.
Hello használhatja [Docker eszközkészlet](https://www.docker.com/products/docker-toolbox), ami megköveteli, hogy a Hyper-V toobe le van tiltva, vagy használhatja [Docker a Windows](https://www.docker.com/get-docker), amely Hyper-V használ, és a Windows 10 szükséges.

Ha használja a Docker eszközkészlet, szüksége lesz túl[hello Docker-ügyfél konfigurálása](vs-azure-tools-docker-setup.md)

## <a name="1-create-a-web-app"></a>1. Webalkalmazás létrehozása
[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. Adja hozzá a Docker-támogatás
[!INCLUDE [Add docker support](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-edit-your-code-and-refresh"></a>3. A kód és a frissítési szerkesztése
tooquickly többször módosításokat, indítsa el az alkalmazás olyan tárolóban, és továbbra is toomake módosításokat, mint az IIS Express a Megtekintés.

1. Hello megoldás konfigurációs beállítása túl`Debug` nyomja le az ENTER  **&lt;CTRL + F5 >** toobuild a docker rendszerképet, futtassa helyileg.

    Miután hello tároló kép készült, és futtatja a Docker-tároló, a Visual Studio elindít hello webalkalmazást az alapértelmezett böngészőben.
    Ha hello Microsoft Edge böngészőt használ, vagy ellenkező esetben a hibák rendelkezik, tekintse meg a [hibaelhárítás](vs-azure-tools-docker-troubleshooting-docker-errors.md) szakasz.
2. Nyissa meg toohello oldalról, amely ahol programot fogjuk toomake szükséges módosításokat.
3. Térjen vissza a tooVisual Studio, és nyissa meg a `Views\Home\About.cshtml`.
4. Adja hozzá a következő hello fájl végéhez HTML tartalom toohello hello és hello módosítások mentéséhez.

    ```
    <h1>Hello from a Docker Container!</h1>
    ```
5. Hello kimeneti ablakában jelenik meg, amikor hello .NET build befejeződött, és ezek a sorok látja, váltson vissza tooyour böngészőt, és frissítse a hello oldalról.

   ```
   Now listening on: http://*:80
   Application started. Press Ctrl+C tooshut down
   ```
6. A módosítások léptek érvénybe!

## <a name="4-debug-with-breakpoints"></a>4. Töréspontokat a hibakereséshez
Gyakran módosításokat kell további ellenőrzési funkciók a Visual Studio hibakeresési hello kihasználva.

1. Térjen vissza a tooVisual Studio, és nyissa meg a`Controllers\HomeController.cs`
2. Cserélje le a hello About() metódus hello tartalmát hello alábbira:

   ```
   string message = "Your application description page from within a Container";
   ViewData["Message"] = message;
   ````
3. A töréspont toohello hello a bal oldali set `string message`... sor.
4. Találati  **&lt;F5 >** toostart hibakeresést.
5. Keresse meg a lap toohit kapcsolatos toohello a töréspont megjelenését.
6. TooVisual Studio tooview hello töréspont váltson, és vizsgálja meg az üzenet hello értékét.

   ![][2]

## <a name="summary"></a>Összefoglalás
A [Docker Visual Studio 2015 eszközök](https://aka.ms/DockerToolsForVS), helyben, hello éles létrehozásáról a belül egy Docker-tároló fejlődő hello hatékonyságára kaphat.

## <a name="troubleshooting"></a>Hibaelhárítás
[Visual Studio Docker fejlesztői hibaelhárítása](vs-azure-tools-docker-troubleshooting-docker-errors.md)

## <a name="more-about-docker-with-visual-studio-windows-and-azure"></a>További információ a Visual Studio, a Windows és az Azure Docker
* [Docker Tools for Visual Studio](http://aka.ms/dockertoolsforvs) -tároló a .NET Core kódot fejlesztése
* [Docker Tools for Visual Studio Team Services](http://aka.ms/dockertoolsforvsts) - Build és a docker-tároló üzembe helyezése
* [Docker Tools for Visual Studio Code](http://aka.ms/dockertoolsforvscode) -nyelv szolgáltatások docker-fájlok, szerkesztéséhez hamarosan további e2e esetén
* [Információ a Windows tároló](http://aka.ms/containers)-Windows Server és a Nano Server információk
* [Azure Tárolószolgáltatás](https://azure.microsoft.com/services/container-service/) - [Azure tároló szolgáltatás tartalom](http://aka.ms/AzureContainerService)
* További Docker használata című részben talál példákat [Docker használata](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) a hello [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Connect [bemutató](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Tekintse meg a hello HealthClinic.biz bemutató további quickstarts [Azure fejlesztői eszközök Quickstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

## <a name="various-docker-tools"></a>Különböző Docker-eszközök
[Néhány nagy docker eszközök (Steve Lasker blog)](https://blogs.msdn.microsoft.com/stevelasker/2016/03/25/some-great-docker-tools/)

## <a name="good-articles"></a>Jó cikkek
[A NGINX bemutatása tooMicroservices](https://www.nginx.com/blog/introduction-to-microservices/)

## <a name="presentations"></a>Bemutatók
* [Steve Lasker: A VS élő moszkvai 2016 - Docker e2e](https://github.com/SteveLasker/Presentations/blob/master/VSLive2016/Vegas/)
* [Bevezetés tooASP.NET Core @ build 2016 – Ha Ön a bemutató](https://channel9.msdn.com/Events/Build/2016/B810)
* [.NET-alkalmazásfejlesztés tárolókban, a Channel 9](https://blogs.msdn.microsoft.com/stevelasker/2016/02/19/developing-asp-net-apps-in-docker-containers/)

[2]: ./media/vs-azure-tools-docker-edit-and-refresh/breakpoint.png
