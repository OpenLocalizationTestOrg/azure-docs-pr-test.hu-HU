---
title: "Azure Kubernetes-fürtöt aaaManage webes felhasználói felületen |} Microsoft Docs"
description: "Hello segítségével Kubernetes webes felhasználói felülete, az Azure Tárolószolgáltatásban"
services: container-service
documentationcenter: 
author: bburns
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/21/2017
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: e24ea0b82c94d2fd4610e4442699ef756590e6bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-kubernetes-web-ui-with-azure-container-service"></a>Hello segítségével Kubernetes webes felhasználói felülete, az Azure Tárolószolgáltatásban

## <a name="prerequisites"></a>Előfeltételek
Ez az útmutató feltételezi, hogy rendelkezik [a Kubernetes Azure Tárolószolgáltatási fürt létrehozott](container-service-kubernetes-walkthrough.md).


Azt is feltételezi, hogy rendelkezik-e hello Azure CLI 2.0 és `kubectl` eszközök vannak telepítve.

Ha hello tesztelheti `az` eszköz futtatásával telepítve:

```console
$ az --version
```

Ha még nem rendelkezik hello `az` eszköz telepítve, az e-mail utasításokat is [Itt](https://github.com/azure/azure-cli#installation).

Ha hello tesztelheti `kubectl` eszköz futtatásával telepítve:

```console
$ kubectl version
```

Ha nem rendelkezik `kubectl` telepítve, futtatható:

```console
$ az acs kubernetes install-cli
```

## <a name="overview"></a>Áttekintés

### <a name="connect-toohello-web-ui"></a>Csatlakozás toohello webes felhasználói felület
Hello Kubernetes webes felhasználói felület futtatásával indíthatja el:

```console
$ az acs kubernetes browse -g [Resource Group] -n [Container service instance name]
```

Ez meg kell nyitnia a böngésző konfigurált tootalk tooa biztonságos webproxy csatlakozás a helyi számítógép toohello Kubernetes webes felhasználói felület.

### <a name="create-and-expose-a-service"></a>Hozzon létre, és tegye elérhetővé a szolgáltatás
1. Hello Kubernetes webes felhasználói felület, kattintson **létrehozása** a hello felső jobb oldali gomb.

    ![Kubernetes felhasználói Felületüket létrehozni.](./media/container-service-kubernetes-ui/create.png)

    Egy adott elindíthatja az alkalmazás létrehozása párbeszédpanel megnyílik.

2. Adjon neki nevet hello `hello-nginx`. Használjon hello [ `nginx` tárolót a Docker](https://hub.docker.com/_/nginx/) és három replikák webszolgáltatás telepítése.

    ![Kubernetes Pod létrehozása párbeszédpanel](./media/container-service-kubernetes-ui/nginx.png)

3. A **szolgáltatás**, jelölje be **külső** , és írja be a 80-as porton.

    Ez a beállítás kiegyenlíti forgalom toohello három replikákat.

    ![Kubernetes Service létrehozás párbeszédpanel](./media/container-service-kubernetes-ui/service.png)

4. Kattintson a **telepítés** toodeploy ezeket tárolók és a szolgáltatásokat.

    ![Kubernetes központi telepítése](./media/container-service-kubernetes-ui/deploy.png)

### <a name="view-your-containers"></a>A tárolók megtekintése
Miután rákattintott **telepítés**, felhasználói felület hello jeleníti meg, a szolgáltatás módon telepíti:

![Kubernetes állapota](./media/container-service-kubernetes-ui/status.png)

Látható hello állapota hello kör Kubernetes objektumok hello bal oldalán lévő felhasználói felület, a **három munkaállomás-csoporttal**. Ha egy részben teljes kör, hello objektum még telepítés. Az objektum teljes mértékben telepítve, egy zöld pipa jeleníti meg:

![Telepített Kubernetes](./media/container-service-kubernetes-ui/deployed.png)

Ha minden fut, kattintson a webszolgáltatást futtató hello három munkaállomás-csoporttal toosee részleteit.

![Kubernetes három munkaállomás-csoporttal](./media/container-service-kubernetes-ui/pods.png)

A hello **három munkaállomás-csoporttal** nézetben hello tárolók hello pod, valamint a hello CPU és memória-erőforrások összes blobhoz által használt információ látható:

![Kubernetes erőforrások](./media/container-service-kubernetes-ui/resources.png)

Ha hello erőforrások nem látható, szükség lehet toowait néhány percig, amíg a figyelési adatok toopropagate hello.

Kattintson a tároló toosee hello naplók **naplók megtekintése**.

![Kubernetes naplók](./media/container-service-kubernetes-ui/logs.png)

### <a name="viewing-your-service"></a>A szolgáltatás megtekintése
Továbbá toorunning a tárolók hello Kubernetes felhasználói felület létrehozott külső `Service` amely látja el a terhelés terheléselosztó toobring forgalom toohello tárolók a fürtön.

Hello bal oldali navigációs ablaktábláján kattintson **szolgáltatások** tooview összes szolgáltatást (kell csak egyet).

![Kubernetes szolgáltatások](./media/container-service-kubernetes-ui/service-deployed.png)

A fenti nézetben megtekintheti az tooyour szolgáltatás felosztott külső végpont (IP-cím).
Ha az IP-címet gombra kattint, megtekintheti a terheléselosztó mögött futó Nginx tároló.

![nginx megtekintése](./media/container-service-kubernetes-ui/nginx-page.png)

### <a name="resizing-your-service"></a>A szolgáltatás átméretezése
A hozzáadása tooviewing az objektumok hello felhasználói felület, szerkesztheti és hello Kubernetes API objektumokat frissíteni.

Első lépésként kattintson **központi telepítések** a bal oldali navigációs ablaktábla toosee hello szolgáltatás központi telepítését a hello.

Miután a fenti nézetben, kattintson a hello replikakészlethez, majd **szerkesztése** hello felső navigációs sáv:

![Kubernetes szerkesztése](./media/container-service-kubernetes-ui/edit.png)

Hello szerkesztése `spec.replicas` mező toobe `2`, és kattintson a **frissítés**.

Ennek hatására a replikák toodrop tootwo hello száma egy a három munkaállomás-csoporttal.

 

