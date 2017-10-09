---
title: "aaaAzure App Service Web App Linux – gyakori kérdések |} Microsoft Docs"
description: "Az Azure App Service webalkalmazásba Linux – gyakori kérdések."
keywords: "az Azure app service, webalkalmazás, gyakran ismételt kérdések, linux, oss"
services: app-service
documentationCenter: 
author: ahmedelnably
manager: erikre
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: aelnably;wesmc
ms.openlocfilehash: c7798d9144d936eecdc0e191fc870b0ee0b220c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-web-app-on-linux-faq"></a>Az Azure App Service webalkalmazásba Linux – gyakori kérdések

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


Webalkalmazás Linux hello kiadással fejlesztjük szolgáltatások hozzáadására és fejlesztések tooour platform elvégzése. Íme néhány gyakori kérdésekkel (GYIK) ügyfeleink kérésére velünk keresztül hello utolsó hónapban.
Ha egy kérdést, Megjegyzés hello cikken, és azt fogja fogadja a hívást a lehető leghamarabb.

## <a name="built-in-images"></a>Beépített lemezképek

**K:** kívánt toofork hello beépített Docker tárolókat hello platformot biztosít. Hol találok azokat a fájlokat?

**V:** minden Docker-fájl található [GitHub](https://github.com/azure-app-service). Az összes Docker-tároló található [Docker Hub](https://hub.docker.com/u/appsvc/).

**K:** hello várt értékek hello indítási fájl a szakaszban a amikor beállítani a hello futásidejű verem Mik?

**V:** a Node.Js megadott hello PM2 konfigurációs fájl vagy a parancsfájl. A .NET Core írja be a lefordított dll-fájl nevét. Ruby, a Ruby parancsfájl hello adhat meg, amelyet tooinitialize az alkalmazás.

## <a name="management"></a>Kezelés

**K:** mi történik, amikor hello Újraindítás gombra a hello Azure-portálon?

**V:** ezzel egyenértékű Docker-újraindításának van hello.

**K:** használható Secure Shell (SSH) tooconnect toohello app tároló virtuális gép (VM)?

**V:** Igen, akkor teheti meg, hogy hello SCM webhelyen, hello következő ellenőrzése a következő cikket: további információk [webalkalmazás Linux SSH-támogatás](./app-service-linux-ssh-support.md)

**K:** toocreate Linux App Service ponton keresztül SDK vagy egy ARM-sablont szeretnék, hogyan lehet elérése Ez?

**V:** tooset hello kell `reserved` hello alkalmazás mező túl szolgáltatás`true`.

## <a name="continuous-integrationdeployment"></a>A folyamatos integrációs vagy üzembe helyező

**K:** a webes alkalmazás továbbra is használja egy Docker-tároló régi lemezképet, miután hello kép Docker központ frissítése már megtörtént. Egyéni tárolók folyamatos integráció/telepítésének támogatása?

**V:** folyamatos integráció/üzembe által a következő cikket ellenőrzés hello Azure tároló beállításjegyzék vagy DockerHub lemezképet fel tooset [folyamatos üzembe helyezés az Azure Web Apps Linux](./app-service-linux-ci-cd.md). A titkos nyilvántartó hello tároló frissítéséhez a webalkalmazás majd indítása és leállítása. Vagy módosíthatja vagy tooforce beállítása a tároló frissítését üres alkalmazás felvétele.

**K:** támogatott az átmeneti környezetben?

**V:** Igen.

**K:** használható **a web deploy** toodeploy a webes alkalmazást?

**V:** , kell tooset nevű beállítása alkalmazás `WEBSITE_WEBDEPLOY_USE_SCM` túl`false`.

## <a name="language-support"></a>Nyelvi támogatás

**K:** támogatják nem fordított .NET Core alkalmazások?

**V:** Igen.

**K:** támogatják a szerkesztő függőségi vezető PHP-alkalmazásokhoz?

**V:** Igen. A Git telepítés során a Kudu telepíti a PHP-alkalmazások (Köszönjük toohello jelenlétét a composer.json fájlok), és akkor indul el, a szerkesztő telepítése meg kell észleli.

## <a name="custom-containers"></a>Egyéni tárolók

**K:** használom a saját egyéni tároló. Saját alkalmazás hello található `\home\` könyvtár, de nem található a fájlok I hello tartalmak böngészése hello segítségével [SCM hely](https://github.com/projectkudu/kudu) vagy az FTP-ügyfél. Hol találhatók a fájlok?

**V:** azt csatlakoztatása egy SMB megosztási toohello `\home\` könyvtár. Ez a művelet felülírja a telepített tartalmak van-e.

**K:** használom a saját egyéni tároló. Most nem kívánok hello platform toomount egy SMB megosztási toohello `\home\`.

**V:** elvégezhető, amely a hello beállítása `WEBSITES_ENABLE_APP_SERVICE_STORAGE` app beállítás túl`false`.

**K:** saját egyéni tároló egy hosszú ideig toostart, valamint a hello platform újraindítás hello tároló példánytól, mielőtt befejezi indítása.

**V:** beállíthatja a tároló újraindítása előtt várjon hello platform hello időt. Ezt úgy teheti hello beállítása `WEBSITES_CONTAINER_START_TIME_LIMIT` app beállítás toohello a keresett másodpercben. hello alapértelmezett érték 230 másodperc, és maximális hello 600 másodperc.

**K:** titkos beállításjegyzék-kiszolgáló URL-címének formátuma hello újdonságai?

**V:** tooprovide hello teljes beállításjegyzék URL-címe például szüksége `http://` vagy `https://`.

**K:** Újdonságok hello formátum hello kép nevét a titkos beállításjegyzék-beállítást?

**V:** szüksége tooadd hello teljes lemezkép neve például hello titkos beállításjegyzék URL-cím (pl.. myacr.azurecr.IO/DotNet:Latest)

**K:** kívánt tooexpose több port saját egyéni tároló lemezképen. Van lehetőség, amely?

**V:** jelenleg nem támogatott.

**K:** hozzáadhatom saját storage?

**V:** jelenleg nem támogatott.

**K:** nem érhető saját egyéni tároló fájl rendszer futó folyamatok hello SCM helyről. Az oka?

**V:** hello SCM hely külön futtatja. Nem lehet ellenőrizni a hello fájlrendszer vagy a futó folyamatok hello app tároló.

**K:** saját egyéni tároló tooa port a 80-as port nem figyel. Hogyan konfigurálható a saját alkalmazás tooroute hello kérelmek toothat port?

**V:** automatikus port észlelési van, egy alkalmazás nevű beállítása meg is **WEBSITES_PORT**, és adjon neki hello értéket várt hello portszámot. Hello platform használta korábban `PORT` app, azt tervezi, toodeprecate hello használata az alkalmazás, beállítás és toousing áthelyezése `WEBSITES_PORT` kizárólag.

**K:** van tooimplement HTTPS saját egyéni tárolóban.

**V:** nem, hello platform kezeli-e a megosztott hello frontends HTTPS-záráshoz.

## <a name="pricing-and-sla"></a>Díjszabás és SLA

**K:** mi van hello árképzési hello nyilvános előzetes használata során?

**V:** fele hello óraszámon belül, az alkalmazás, amelynek hello normál Azure App Service szolgáltatás díjszabása van szó. Ez azt jelenti, hogy a normál Azure App Service szolgáltatás díjszabása 50 % kedvezménnyel kap.

## <a name="other"></a>Egyéb

**K:** Mik azok a beállítások az alkalmazásnevekben hello támogatott karakter?

**V:** csak használja A-Z, a – z, 0-9, és az alkalmazásbeállítások aláhúzás hello.

**K:** ahol is kérjen új szolgáltatások?

**V:** elküldheti a képet, hello [webalkalmazások visszajelzési fórumon](https://aka.ms/webapps-uservoice). Adja hozzá a ötlet toohello címe "[Linux]".

## <a name="next-steps"></a>Következő lépések
* [Mi az Azure Web Apps Linux?](app-service-linux-intro.md)
* [Azure Web Apps Linux SSH támogatása](./app-service-linux-ssh-support.md)
* [Átmeneti környezet az Azure App Service beállítása](./web-sites-staged-publishing.md)
* [Folyamatos üzembe helyezés az Azure-webalkalmazásban Linux rendszeren](./app-service-linux-ci-cd.md)
