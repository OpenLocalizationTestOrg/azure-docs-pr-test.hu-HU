---
title: "aaaMonitor egy Azure Tárolószolgáltatás-fürtöt Sysdig |} Microsoft Docs"
description: "Azure tárolószolgáltatás-fürt megfigyelése a Sysdig segítségével."
services: container-service
documentationcenter: 
author: sauryadas
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Tárolók, DC/OS, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 72f2d3d6f6885f9876fa158b88aae58b84a4610f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-sysdig"></a>Azure tárolószolgáltatás-fürt megfigyelése a Sysdig segítségével
Ebben a cikkben fogjuk üzembe helyezni Sysdig ügynökök tooall hello ügynök a Azure Tárolószolgáltatási fürt csomópontja. Ehhez a konfiguráláshoz Sysdig-fiókra van szüksége. 

## <a name="prerequisites"></a>Előfeltételek
[Helyezzen üzembe](container-service-deployment.md) és [csatlakoztasson](../container-service-connect.md) egy, az Azure Container Service által konfigurált fürtöt. Fedezze fel hello [Marathon felhasználói felület](container-service-mesos-marathon-ui.md). Nyissa meg túl[http://app.sysdigcloud.com](http://app.sysdigcloud.com) tooset Sysdig felhő fiók létrehozása. 

## <a name="sysdig"></a>Sysdig
Sysdig egy figyelési szolgáltatása lehetővé teszi toomonitor a tárolók a fürtön belül. Sysdig ismert toohelp kaptak, de a alapvető figyelési metrikákat, a CPU, a hálózat, a memória és az i/o is rendelkezik. Sysdig teszi, hogy mely tárolók dolgozik könnyen toosee hello segítségével hardest vagy lényegében hello legtöbb memóriát és CPU. Ez a nézet hello "Overview" szakaszban, amely jelenleg bétaverziójú van. 

![Sysdig felhasználói felület](./media/container-service-monitoring-sysdig/sysdig6.png) 

## <a name="configure-a-sysdig-deployment-with-marathon"></a>Üzemelő Sysdig-példány konfigurálása a Marathonnal
Ezeket a lépéseket bemutatja, hogyan tooconfigure és Sysdig alkalmazások tooyour fürtben a marathon segítségével telepíthet. 

A DC/OS felhasználói felületén keresztül elérni [http://localhost:80 /](http://localhost:80/) egyszer hello DC/OS felhasználói felületének lépjen a toohello "Universe", amelyek a hello bal alsó, és keressen a "Sysdig."

![Sysdig a DC/OS Universe rendszerben](./media/container-service-monitoring-sysdig/sysdig1.png)

Most toocomplete hello konfigurációs egy Sysdig van szüksége a fiók vagy egy ingyenes próbafiók felhő. Miután toohello Sysdig felhő webhelyen jelentkezett, kattintson a felhasználónevére, és hello oldalon kell megjelennie a "hozzáférési kulcsot." 

![Sysdig API-kulcs](./media/container-service-monitoring-sysdig/sysdig2.png) 

A hozzáférési kulcsot következő kezdenek hello Sysdig konfigurációs hello DC/OS Universe belül. 

![A DC/OS Universe hello Sysdig konfiguráció](./media/container-service-monitoring-sysdig/sysdig3.png)

Most már készen hello példányok too10000000, amikor egy új csomópontot hozzáadják toohello fürt Sysdig automatikusan telepíteni fogja az ügynök toothat új csomópont. Ez az egy átmeneti megoldás toomake meg arról, hogy Sysdig telepíti az új ügynökök tooall hello fürtön belül. 

![A DC/OS Universe-példányokat hello Sysdig konfiguráció](./media/container-service-monitoring-sysdig/sysdig4.png)

Hello csomag telepítése után nyissa meg a visszafelé toohello Sysdig felhasználói felület, és be fog tudni tooexplore hello különböző a szoftverhasználati mérési adatok az hello tárolókat a fürtön belül. 

Telepíthet kimondottan a Mesoshoz és a Marathonhoz készült irányítópultokat is az [új irányítópult varázsló](https://app.sysdigcloud.com/#/dashboards/new) segítségével.
