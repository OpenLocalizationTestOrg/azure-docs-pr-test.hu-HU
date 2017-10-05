---
title: "Egyéni Docker-lemezkép használata Linux Azure webalkalmazás számára |} Microsoft Docs"
description: "Hogyan használható az egyéni Docker-lemezkép Linux Azure webalkalmazás számára."
keywords: "az Azure app service, a webes alkalmazás, a linux, a docker, a tároló"
services: app-service
documentationcenter: 
author: naziml
manager: erikre
editor: 
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2017
ms.author: naziml;wesmc
ms.openlocfilehash: 1458217a31c4781b28877c030a665f5b22819e13
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="using-a-custom-docker-image-for-azure-web-app-on-linux"></a>Az Azure Web Apps Linux rendszeren egyéni Docker-lemezkép használatával #

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


App Service biztosít előre definiált alkalmazás verem Linux-támogatással rendelkező különböző verziókat, például a PHP 7.0 és a Node.js 4.5. App Service Linux Docker-tároló ezeket az előre elkészített alkalmazás-készleteket használ. Egyéni Docker-lemezkép használatával a webalkalmazás telepítése az egy alkalmazás verem nem definiált már az Azure-ban. Egyéni Docker lemezképek alábbiakon tárolható vagy egy nyilvános vagy privát Docker-tárházat.


## <a name="how-to-set-a-custom-docker-image-for-a-web-app"></a>Útmutató: a webalkalmazáshoz tartozó egyéni Docker-lemezkép beállítása
Az egyéni Docker lemezképet mindkét meglévő és új webhelyek alkalmazások állíthatja be. Ha a Linux-webalkalmazás létrehozása a [Azure-portálon](https://portal.azure.com/#create/Microsoft.AppSvcLinux), kattintson a **konfigurálása tároló** beállítása egy egyéni Docker-lemezképet:

![Az új webalkalmazáshoz Linux rendszeren egyéni Docker kép][1]


## <a name="how-to-use-a-custom-docker-image-from-docker-hub"></a>Útmutató: a Docker Hub egyéni Docker kép használata ##
A Docker Hub egyéni Docker-lemezkép használata:

1. Az a [Azure-portálon](https://portal.azure.com), majd keresse meg a webalkalmazás Linux, a **beállítások** kattintson **Docker-tároló**.

2.  Válassza ki **Docker Hub** , a **lemezképforrás**, ezután kattintson **nyilvános** vagy **titkos** , és írja be a **lemezkép és nem kötelező címke neve**, például a `node:4.5`. A **indítási parancs** beállítása automatikusan alapul a Docker bináris fájlban meghatározott, de a saját parancsok állíthatja be.  

    ![Kép: a nyilvános tárház Docker központ konfigurálása][2]

    A lemezkép egy titkos tárházból esetén is kell a Docker Hub hitelesítő adatokat, mint (**bejelentkezési felhasználónevének** és **jelszó**) a saját Docker Hub tárház.

    ![Kép: Docker Hub személyes adattár konfigurálása][3]

3. Miután konfigurálta a tárolót, kattintson a **mentése**.

## <a name="how-to-use-a-docker-image-from-a-private-image-registry"></a>A Docker-lemezkép használata a titkos kép beállításjegyzékből ##
Egyéni Docker-lemezkép használata a titkos kép beállításjegyzékből:

1. Az a [Azure-portálon](https://portal.azure.com), majd keresse meg a webalkalmazás Linux, a **beállítások** kattintson **Docker-tároló**.

2.  Kattintson a **titkos beállításjegyzék** , a **lemezképforrás**. Adja meg a **kép és a választható címkenév**, **URL-címe** titkos beállításjegyzék, a hitelesítő adatokkal együtt (**bejelentkezési felhasználónevének** és **jelszó**). Kattintson a **Save** (Mentés) gombra.

    ![Docker kép titkos beállításjegyzékből konfigurálása][4]


## <a name="how-to-set-the-port-used-by-your-docker-image"></a>Útmutató: a Docker-lemezkép által használt port beállítása ##

Ha egy egyéni Docker lemezképet használ a webalkalmazás, használhatja a `WEBSITES_PORT` környezeti változót a Dockerfile, amely lekérdezi a generált tárolóhoz adni. Vegye figyelembe a következő példa egy docker-fájl egy Ruby alkalmazáshoz:

    FROM ruby:2.2.0
    RUN mkdir /app
    WORKDIR /app
    ADD . /app
    RUN bundle install
    CMD bundle exec puma config.ru -p WEBSITES_PORT -e production

A parancs utolsó sora megtekintheti, hogy a WEBSITES_PORT környezeti változó átadása futásidőben. Ne feledje, hogy a kis-és nagybetűk parancsokban számít.

A platform használta korábban `PORT` app beállításnál azt tervezi, hogy érvényteleníthető használatát az alkalmazás, beállítás és áthelyezése használatával `WEBSITES_PORT` kizárólag.

Egy meglévő, beépített valaki más Docker kép használatakor esetleg adja meg az alkalmazás 80-as portra. Az a port megadásához nevű beállításával alkalmazás hozzáadása `WEBSITES_PORT` a értékű alább látható módon:

![Egyéni Docker-lemezképet alkalmazás portbeállítást konfigurálása][6]

## <a name="how-to-set-the-startup-time-for-your-docker-image"></a>Útmutató: a Docker-lemezkép indítási idejének beállítása ##

Alapértelmezés szerint a tároló nem indul el, mielőtt 230 másodperc, ha a platform újraindítja a tárolót. Ha az egyéni Docker-lemezkép 230 másodpercnél indul, használhatja a `WEBSITES_CONTAINER_START_TIME_LIMIT` másodpercben alkalmazás beállítása, a beállításnak az értéke, ez lehetővé teszi a platform tartsa a tárolóban fut, akkor az újraindítás előtt. Az alapértelmezett érték 230 másodperc, és a maximális megengedett értéke 600 másodperc.

## <a name="how-to-unmount-the-platform-provided-storage"></a>Útmutató: a megadott platform tárolási leválasztása ##

Alapértelmezés szerint a platform egy állandó tároló megosztás fogja csatlakoztatni a `\home\` könyvtár. Ha a tároló-lemezkép nem kell egy állandó megosztást, tárolási csatlakoztatása beállításával letilthatja a `WEBSITES_ENABLE_APP_SERVICE_STORAGE` Alkalmazásbeállítás `false`. Ön továbbra is hozzáférhetnek a tárolási az scm-helyről, és minden Docker-naplók (Ha engedélyezve van), a platform által létrehozott naplófájlok lesz írva.

## <a name="how-to-switch-back-to-using-a-built-in-image"></a>Hogyan: Váltás beépített lemezkép használatával ##

Váltás a beépített lemezképpel egyéni lemezkép használatával:

1. Az a [Azure-portálon](https://portal.azure.com), majd keresse meg a webalkalmazás Linux, a **beállítások** kattintson **App Service**.

2. Válassza ki a **futásidejű verem** kíván használni a beépített lemezképet, majd kattintson a **mentése**. 

![Kép: a beépített Docker konfigurálása][5]


## <a name="troubleshooting"></a>Hibaelhárítás ##

Ha az alkalmazás nem az egyéni Docker lemezképpel indul el, ellenőrizze a Docker naplózza a naplófájlok könyvtárban. Ez a könyvtár az SCM-hely vagy FTP-n keresztül érheti el.
A napló a `stdout` és `stderr` a tárolóból, engedélyezni kell a **Docker-tároló naplózási** alatt **diagnosztikai naplók**.

![Naplózás engedélyezése][8]

![A Kudu segítségével Docker naplók megtekintése][7]

Érheti el az SCM helyet **speciális eszközök** a a **Fejlesztőeszközök** menü.

## <a name="next-steps"></a>Következő lépések ##

Az alábbi hivatkozásokból Ismerkedés a Linux webalkalmazás.   

* [Bevezetés az Azure-webalkalmazásban Linux rendszeren](./app-service-linux-intro.md)
* [PM2 Configuration for Node.js Linux Azure Web App használatával](./app-service-linux-using-nodejs-pm2.md)
* [Az Azure App Service webalkalmazásba Linux – gyakori kérdések](app-service-linux-faq.md)

Kérdések és aggályokat írjon [fórumban](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).


<!--Image references-->
[1]: ./media/app-service-linux-using-custom-docker-image/new-configure-container.png
[2]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-dockerhub-public.png
[3]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-dockerhub-private.png
[4]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-privateregistry.png
[5]: ./media/app-service-linux-using-custom-docker-image/existingapp-configure-builtin.png
[6]: ./media/app-service-linux-using-custom-docker-image/setting-port.png
[7]: ./media/app-service-linux-using-custom-docker-image/kudu-docker-logs.png
[8]: ./media/app-service-linux-using-custom-docker-image/logging.png
