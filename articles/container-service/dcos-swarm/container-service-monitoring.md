---
title: "aaaMonitor Azure DC/OS-fürtről - Datadog |} Microsoft Docs"
description: "Az Azure Tárolószolgáltatás-fürt Datadog a figyelheti. Hello DC/OS webes felhasználói felület toodeploy hello Datadog ügynökök tooyour-fürt használatára."
services: container-service
documentationcenter: 
author: sauryadas
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Tárolók, a DC/OS, a Docker Swarm Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure
ms.date: 07/28/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 10268c04b5c2ef393429e706ed4a467fff80f718
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-datadog"></a>Egy Azure tároló szolgáltatás DC/OS fürt Datadog figyelése
Ebben a cikkben fogjuk üzembe helyezni Datadog ügynökök tooall hello ügynök a Azure Tárolószolgáltatási fürt csomópontja. Ehhez a konfigurációhoz szüksége lesz egy fiókra Datadog. 

## <a name="prerequisites"></a>Előfeltételek
[Helyezzen üzembe](container-service-deployment.md) és [csatlakoztasson](../container-service-connect.md) egy, az Azure Container Service által konfigurált fürtöt. Fedezze fel hello [Marathon felhasználói felület](container-service-mesos-marathon-ui.md). Nyissa meg túl[http://datadoghq.com](http://datadoghq.com) tooset Datadog fiók létrehozásához. 

## <a name="datadog"></a>Datadog
Datadog egy figyelési szolgáltatás, amely a figyelési adatokat gyűjt a tárolók skálázása az Azure Tárolószolgáltatás-fürt belül. Datadog rendelkezik egy Docker integrációs irányítópultot, ahol láthatja adott mérőszámok belül a tárolók. A tárolók gyűjtött metrikák Processzor, memória, hálózati és i/o szerint vannak rendszerezve. Datadog metrikák felosztja a tárolók és a képeket. Milyen hello például felhasználói felület tűnik processzor kihasználtsága nem éri el.

![Datadog felhasználói felület](./media/container-service-monitoring/datadog4.png)

## <a name="configure-a-datadog-deployment-with-marathon"></a>A marathon segítségével Datadog telepítés konfigurálása
Ezeket a lépéseket bemutatja, hogyan tooconfigure és Datadog alkalmazások tooyour fürtben a marathon segítségével telepíthet. 

A DC/OS felhasználói felületén keresztül elérni [http://localhost:80 /](http://localhost:80/). Egyszer hello DC/OS felhasználói felületének keresse meg a "Universe", amelyek a hello toohello bal alsó, majd keresse meg a "Datadog", majd kattintson "Telepítés".

![A DC/OS Universe hello belül Datadog csomag](./media/container-service-monitoring/datadog1.png)

Most toocomplete hello konfigurációs szüksége lesz egy Datadog fiók vagy egy ingyenes próbafiókot. Miután jelentkezett toohello Datadog webhelyen keresse meg a toohello balra, és nyissa meg tooIntegrations majd -> [API-k](https://app.datadoghq.com/account/settings#api). 

![Datadog API-kulcs](./media/container-service-monitoring/datadog2.png)

Ezután adjon meg az API-kulcs hello Datadog konfigurációs hello DC/OS Universe belül. 

![A DC/OS Universe hello Datadog konfiguráció](./media/container-service-monitoring/datadog3.png) 

A fenti konfigurációs hello példányok úgy van beállítva, too10000000 amikor egy új csomópontot hozzáadják toohello fürt Datadog automatikusan telepíteni fogja az ügynök toothat csomóponthoz. Ez az egy átmeneti megoldás. Hello csomag telepítése után, nyissa meg a visszafelé toohello Datadog webhelyet, és található "[irányítópultok](https://app.datadoghq.com/dash/list)." Ott látni fogja a egyéni és integrációs irányítópultok. Hello [Docker irányítópult](https://app.datadoghq.com/screen/integration/docker) lesz szüksége figyelését a fürt összes hello tároló metrikát. 

