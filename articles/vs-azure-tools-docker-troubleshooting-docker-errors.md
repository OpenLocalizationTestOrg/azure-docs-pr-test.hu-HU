---
title: "aaaTroubleshooting Docker ügyfél hibákat a Windows Visual Studio használatával |} Microsoft Docs"
description: "Visual Studio toocreate használata során felmerülő problémák megoldásához, és központi telepítése a web apps tooDocker Windows rendszeren Visual Studio használatával."
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: 
ms.assetid: 346f70b9-7b52-4688-a8e8-8f53869618d3
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: 7421ae8e044d58fc412d748fb870da4c9b2fdb3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-visual-studio-docker-development"></a>A Visual Studio Docker fejlesztési hibaelhárítása

Ha a Visual Studio eszközök a Docker Preview dolgozik, néhány problémákat tapasztalhat a hello preview hello jellege miatt.
Az alábbiakban néhány gyakori problémák és megoldások.  

## <a name="visual-studio-2017-rc"></a>A Visual Studio 2017 RC

### <a name="linux-containers"></a>**Linux-tárolók**

####  <a name="build-errors-occur-when-debugging-a-net-core-web-or-console-application"></a>Build hiba történik, amikor a .NET Core web vagy konzol alkalmazásban  

Ennek oka lehet a Docker a Windows hello projektet tartalmazó hello meghajtó megosztása kapcsolódó toonot.  Hasonló hello hiba jelenhet meg:

```
hello "PrepareForLaunch" task failed unexpectedly.
Microsoft.DotNet.Docker.CommandLineClientException: Creating network "webapplication13628050196_default" with hello default driver
Building webapplication1
Creating webapplication13628050196_webapplication1_1
ERROR: for webapplication1  Cannot create container for service webapplication1: C: drive is not shared. Please share it in Docker for Windows Settings
```
tooresolve probléma:

1. Kattintson a jobb gombbal **Docker for Windows** a hello értesítési területén, majd válassza ki **beállítások**.  
2. Válassza ki **megosztott meghajtók** és hello meghajtó, ahol hello projekt található.

### <a name="windows-containers"></a>**Windows-tárolók**

hello következő problémák adott toodebugging .NET-keretrendszer webes és a konzol Windows-tárolókban lévő alkalmazások.

#### <a name="prerequisites"></a>Előfeltételek

1. A Visual Studio 2017 RC (vagy újabb) a .NET Core hello és Docker Preview munkaterhelést kell telepíteni.
2. Windows 10 évforduló frissítése a legújabb Windows frissítéshez javításokkal. Kifejezetten [KB3194798](https://support.microsoft.com/en-us/help/3194798/cumulative-update-for-windows-10-version-1607-and-windows-server-2016-october-11,-2016) kell telepíteni. 
3. [A Windows docker](https://docs.docker.com/docker-for-windows/) (build 1.13.0 vagy újabb) kell telepíteni.
4. **Váltás tooWindows tárolók** ki kell jelölni. Hello értesítési területen kattintson a **Docker for Windows**, majd válassza ki **tooWindows tárolók kapcsoló**. Hello számítógép újraindítását követően győződjön meg arról, hogy ez a beállítás megőrzi.

#### <a name="console-output-does-not-appear-in-visual-studios-output-window-while-debugging-a-console-application"></a>Konzol kimeneti nem jelenik meg a Visual Studio kimeneti ablakában a Konzolalkalmazás-hibakeresés során

Ez az egy ismert probléma az hello Visual Studio hibakereső (msvsmon.exe), amely jelenleg nem az ebben a forgatókönyvben. Ez a forgatókönyv támogatása egy későbbi kiadásban szereplő. toosee kimenetét hello konzolalkalmazást a Visual Studio, használja a **Docker: Start projekt**, egyenértékű túl**indítása nélkül hibakeresés**.

#### <a name="debugging-web-applications-with-hello-release-configuration-fails-with-403-forbidden-error"></a>Hello hibakeresési alkalmazások kiadási (403-as) tiltja hiba konfigurációs meghiúsul

toowork a probléma megoldásához nyissa meg a web.release.config hello megoldásban és megjegyzéssé, vagy törölje az alábbi hello:

```
<compilation xdt:Transform="RemoveAttributes(debug)" />
```

## <a name="visual-studio-2015"></a>Visual Studio 2015

### <a name="linux-containers"></a>**Linux-tárolók**

#### <a name="unable-toovalidate-volume-mapping"></a>Nem lehet toovalidate kötet leképezése
Kötet lesz szükség tooshare hello forráskódját és a hello tároló hello app mappa az alkalmazás a bináris fájljait.  Adott kötet hozzárendelések docker-compose.dev.debug.yml és a docker-compose.dev.release.yml belül találhatók. Fájlok módosulnak, a gazdagépen, mert a hello tárolók hasonló gyökérmappa-szerkezetében lévő változtatásoknak megfelelően.

tooenable kötet hozzárendelése:

1. Kattintson a **Moby** hello értesítési területről, és válassza ki a **beállítások**.
2. Válassza ki **megosztott meghajtók**.
3. Válassza ki a projekt és hello meghajtó, ahol a % USERPROFILE % található hello meghajtón.
4. Kattintson az **Alkalmaz** gombra.

tootest kötet leképezési működik, ha építse újra, és válassza ki az F5 billentyűt a Visual Studio, egy vagy több meghajtó van megosztva, vagy után futtassa a következő kód egy parancs parancssori futtatásával hello.

> [!NOTE]
> Ez a példa feltételezi, hogy a felhasználók mappában található a C meghajtó, és hogy azt meg lett osztva.
> Szükség esetén, ha egy másik meghajtót megosztott módosításához.

```
docker run -it -v /c/Users/Public:/wormhole busybox
```

Futtassa a következő kód hello Linux tárolóban hello.

```
/ # ls
```

Meg kell jelennie egy könyvtárlistát hello felhasználók vagy nyilvános mappából. Ha nem jelenik meg, és a /c/Users/Public mappa nem üres, kötet leképezési nincs megfelelően konfigurálva.

```
bin       etc       proc      sys       usr       wormhole
dev       home      root      tmp       var
```

Módosítsa toohello féreglyuk directory toosee hello hello `/c/Users/Public` könyvtár:

```
/ # cd wormhole/
/wormhole # ls
AccountPictures  Downloads        Music            Videos
Desktop          Host             NuGet.Config     desktop.ini
Documents        Libraries        Pictures
/wormhole #
```

> [!NOTE]
> Linux virtuális gépek dolgozik, hello tároló fájlrendszer esetén kis-és nagybetűket.

## <a name="build-prepareforbuild-task-failed-unexpectedly"></a>Build: "PrepareForBuild" feladat váratlanul leállt

Microsoft.DotNet.Docker.CommandLine.ClientException: Hiba történt, miközben a rendszer tooconnect.

Ellenőrizze, hogy adott hello alapértelmezett Docker gazdagépen fut-e. Nyisson meg egy parancssort, és hajtsa végre:

```
docker info
```

Ha ezt a hibát ad vissza, majd próbálja meg toostart hello **Docker for Windows** egy asztali alkalmazás. Ha hello egy asztali alkalmazás fut, majd **Moby** hello értesítési területen látható kell lennie. Kattintson a jobb gombbal **Moby** , és nyissa meg **beállítások**. Kattintson a **alaphelyzetbe**, majd indítsa újra a Docker.

## <a name="an-error-dialog-occurs-when-attempting-tooadd-docker-support-or-debug-f5-an-aspnet-core-application-in-a-container"></a>Hiba-párbeszédpanel tooadd Docker támogatási tett kísérlet során következik be, vagy a hibakeresési (F5), az ASP.NET Core alkalmazás tároló

Eltávolítása és bővítmények telepítése után a Visual Studio felügyelt bővítési keretrendszer (MEF) gyorsítótár hello is megsérülhet. Ha ez történik, különböző hibaüzenetek okozhat, ha Ön most Docker-támogatás hozzáadása és/vagy toorun kísérlet, illetve az ASP.NET Core alkalmazás (F5) hibakeresését. Ideiglenes megoldásként használja a következő lépéseket toodelete és újragenerálása hello MEF gyorsítótár hello.

1. Zárja be a Visual Studio.
1. Nyissa meg a % USERPROFILE%\AppData\Local\Microsoft\VisualStudio\14.0\.
1. A következő mappák hello törlése:
     ```
       ComponentModelCache
       Extensions
       MEFCacheBackup
    ```
1. Nyissa meg a Visual Studiót.
1. Próbálja megismételni az hello forgatókönyv.
