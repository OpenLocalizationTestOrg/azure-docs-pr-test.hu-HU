---
title: "az Azure Web Apps Linux rendszeren egyéni Docker kép aaaHow toouse |} Microsoft Docs"
description: "Hogyan toouse egyéni Docker kép Linux Azure webalkalmazás számára."
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
ms.openlocfilehash: 8853095d0e1067cfea4297bbd23b622fe4a0d4db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-a-custom-docker-image-for-azure-web-app-on-linux"></a>Az Azure Web Apps Linux rendszeren egyéni Docker-lemezkép használatával #

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


App Service biztosít előre definiált alkalmazás verem Linux-támogatással rendelkező különböző verziókat, például a PHP 7.0 és a Node.js 4.5. App Service Linux az Docker-tároló toohost ezek előzetesen elkészített alkalmazás verem. A Docker egyéni rendszerkép toodeploy a webes alkalmazás tooan alkalmazáscsoportokat, amely már nincs definiálva az Azure-ban is használja. Egyéni Docker lemezképek alábbiakon tárolható vagy egy nyilvános vagy privát Docker-tárházat.


## <a name="how-to-set-a-custom-docker-image-for-a-web-app"></a>Útmutató: a webalkalmazáshoz tartozó egyéni Docker-lemezkép beállítása
Hello egyéni Docker lemezképet mindkét új és meglévő Web apps állíthatja be. Amikor hoz létre egy webalkalmazást Linux rendszeren a hello [Azure-portálon](https://portal.azure.com/#create/Microsoft.AppSvcLinux), kattintson a **konfigurálása tároló** tooset egyéni Docker-lemezkép:

![Az új webalkalmazáshoz Linux rendszeren egyéni Docker kép][1]


## <a name="how-to-use-a-custom-docker-image-from-docker-hub"></a>Útmutató: a Docker Hub egyéni Docker kép használata ##
a Docker Hub egyéni Docker-lemezkép toouse:

1. A hello [Azure-portálon](https://portal.azure.com), majd keresse meg a webalkalmazás Linux, a **beállítások** kattintson **Docker-tároló**.

2.  Válassza ki **Docker Hub** hello, **lemezképforrás**, ezután kattintson **nyilvános** vagy **titkos** és típus hello **lemezkép és nem kötelező címke neve**, például a `node:4.5`. Hello **indítási parancs** beállítása automatikusan alapuló hello Docker képfájl meghatározott, de a saját parancsok állíthatja be.  

    ![Kép: a nyilvános tárház Docker központ konfigurálása][2]

    A lemezkép egy titkos tárházból esetén szükség tooenter hello Docker Hub hitelesítő adatokat (**bejelentkezési felhasználónevének** és **jelszó**) hello titkos Docker Hub tárház.

    ![Kép: Docker Hub személyes adattár konfigurálása][3]

3. Hello tároló beállítását követően kattintson **mentése**.

## <a name="how-toouse-a-docker-image-from-a-private-image-registry"></a>Hogyan toouse egy Docker titkos kép beállításjegyzékbeli kép ##
a titkos kép beállításjegyzékből egyéni Docker-lemezkép toouse:

1. A hello [Azure-portálon](https://portal.azure.com), majd keresse meg a webalkalmazás Linux, a **beállítások** kattintson **Docker-tároló**.

2.  Kattintson a **titkos beállításjegyzék** , hello **lemezképforrás**. Adja meg a hello **kép és a nem kötelező címke neve**, **URL-címe** hello titkos rendszerleíró hello hitelesítő adatokkal együtt (**bejelentkezési felhasználónevének** és **jelszó** ). Kattintson a **Save** (Mentés) gombra.

    ![Docker kép titkos beállításjegyzékből konfigurálása][4]


## <a name="how-to-set-hello-port-used-by-your-docker-image"></a>Útmutató: a Docker-lemezkép által használt hello port beállítása ##

A webalkalmazás egyéni Docker-lemezkép használatakor hello használhatja `WEBSITES_PORT` a Dockerfile, amely lekérdezi hozzá toohello létrehozott tároló környezeti változót. Vegye figyelembe a következő példa egy docker-fájl egy Ruby alkalmazáshoz hello:

    FROM ruby:2.2.0
    RUN mkdir /app
    WORKDIR /app
    ADD . /app
    RUN bundle install
    CMD bundle exec puma config.ru -p WEBSITES_PORT -e production

Utolsó sora hello parancs megtekintheti hello WEBSITES_PORT környezeti változó átadása futásidőben. Ne feledje, hogy a kis-és nagybetűk parancsokban számít.

Hello platform használta korábban `PORT` app, azt tervezi, toodeprecate hello használata az alkalmazás, beállítás és toousing áthelyezése `WEBSITES_PORT` kizárólag.

Ha valaki más beépített meglévő Docker lemezképet használja, szükség lehet hello alkalmazás toospecify 80-as portra. tooconfigure hello port, nevű beállításával alkalmazás hozzáadása `WEBSITES_PORT` hello értékkel alább látható módon:

![Egyéni Docker-lemezképet alkalmazás portbeállítást konfigurálása][6]

## <a name="how-to-set-hello-startup-time-for-your-docker-image"></a>Útmutató: a Docker kép hello indítási idő beállítása ##

Alapértelmezés szerint a tároló nem indul 230 másodperc, mielőtt hello platform újraindítja a tárolót. A Docker egyéni rendszerkép 230 másodpercnél indítja el, ha használható hello `WEBSITES_CONTAINER_START_TIME_LIMIT` másodpercben app beállítása, a beállításnak hello értéke, ez lehetővé teszi a hello platform tartsa a tárolóban fut, akkor az újraindítás előtt. hello alapértelmezett értéke 230 másodperc, és hello maximálisan megengedett értéke 600 másodperc.

## <a name="how-to-unmount-hello-platform-provided-storage"></a>Hogyan: hello megadott platform tárolási leválasztása ##

Alapértelmezés szerint hello platform fogja csatlakoztatni egy állandó tároló megosztás toohello `\home\` könyvtár. Ha a tároló-lemezkép nem kell egy állandó megosztást, letilthatja, által hello beállítása, hogy a tároló csatlakoztatása `WEBSITES_ENABLE_APP_SERVICE_STORAGE` app beállítás túl`false`. Továbbra is kapnak toothat tároló elérése hello scm helyről, valamint minden Docker-naplók (Ha engedélyezve van), a rendszer úgy tünteti toohello hello platform által generált naplófájlokat.

## <a name="how-to-switch-back-toousing-a-built-in-image"></a>Útmutató: biztonsági toousing beépített kép kapcsoló ##

egy egyéni lemezkép toousing beépített lemezkép használatával tooswitch:

1. A hello [Azure-portálon](https://portal.azure.com), majd keresse meg a webalkalmazás Linux, a **beállítások** kattintson **App Service**.

2. Válassza ki a **futásidejű verem** toouse hello beépített lemezképhez, majd kattintson a **mentése**. 

![Kép: a beépített Docker konfigurálása][5]


## <a name="troubleshooting"></a>Hibaelhárítás ##

Ha az alkalmazás nem sikerül toostart az egyéni Docker-lemezképre, ellenőrizze a Docker bejelentkezik hello naplófájlok directory hello. Ez a könyvtár az SCM-hely vagy FTP-n keresztül érheti el.
toolog hello `stdout` és `stderr` a tárolóból tooenable kell **Docker-tároló naplózási** alatt **diagnosztikai naplók**.

![Naplózás engedélyezése][8]

![A Kudu tooview Docker-naplók segítségével][7]

Van-e hozzáférési hello SCM helyet **speciális eszközök** a hello **Fejlesztőeszközök** menü.

## <a name="next-steps"></a>Következő lépések ##

Hajtsa végre a következő hivatkozások tooget Linux webalkalmazás használatába hello.   

* [Bevezetés tooAzure webalkalmazás Linux rendszeren](./app-service-linux-intro.md)
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
