---
title: "aaaMonitor Azure DC/OS-fürtről - Dynatrace |} Microsoft Docs"
description: "Egy Azure tároló szolgáltatás DC/OS fürtben Dynatrace a figyelheti. Hello Dynatrace OneAgent hello DC/OS Irányítópult segítségével telepítheti."
services: container-service
documentationcenter: 
author: MartinGoodwell
manager: 
editor: 
tags: acs, azure-container-service
keywords: "Tárolók, DC/OS, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/13/2016
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: 9e2e2d1c8b454422d1db30dac7c385d31d336853
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-dynatrace-saasmanaged"></a>Egy Azure tároló szolgáltatás DC/OS fürtben a Dynatrace SaaS/felügyelt figyelése
Ez a cikk azt mutatja be toodeploy hello [Dynatrace](https://www.dynatrace.com/) OneAgent toomonitor összes hello ügynök az Azure Tárolószolgáltatás-fürt csomópontja. A Dynatrace SaaS/felügyelt fiók ehhez a konfigurációhoz szükség van. 

## <a name="dynatrace-saasmanaged"></a>Dynatrace SaaS/felügyelt
Dynatrace egy olyan felhőalapú natív figyelési megoldás magas dinamikus tároló és a fürt környezetekhez. Lehetővé teszi toobetter optimalizálhatja a üzemelő tárolópéldányokat és memória-kiosztásokat valós idejű használati adatok használatával. Is képes automatikusan felügyelő alkalmazás és az infrastruktúra-problémák automatikus viszonyítási probléma korrelációs és kiváltó okának észlelési megadásával.

hello következő ábra azt mutatja be hello Dynatrace felhasználói felület:

![Dynatrace felhasználói felület](./media/container-service-monitoring-dynatrace/dynatrace.png)

## <a name="prerequisites"></a>Előfeltételek 
[Telepítése](container-service-deployment.md) és [csatlakozás](./../container-service-connect.md) állította be az Azure Tárolószolgáltatás tooa fürt. Fedezze fel hello [Marathon felhasználói felület](container-service-mesos-marathon-ui.md). Nyissa meg túl[https://www.dynatrace.com/trial/](https://www.dynatrace.com/trial/) tooset Dynatrace Szolgáltatottszoftver-fiók létrehozásához.  

## <a name="configure-a-dynatrace-deployment-with-marathon"></a>A marathon segítségével Dynatrace telepítés konfigurálása
Ezeket a lépéseket mutatja be tooconfigure és Dynatrace alkalmazások tooyour fürtben a marathon segítségével telepíthet.

1. A DC/OS felhasználói felületén keresztül elérni [http://localhost:80 /](http://localhost:80/). Egyszer hello DC/OS felhasználói felületének, lépjen a toohello **Universe** fülre, majd keresse meg a **Dynatrace**.

    ![A DC/OS Universe Dynatrace](./media/container-service-monitoring-dynatrace/dynatrace-universe.png)

2. toocomplete hello konfigurálása van szüksége egy Dynatrace Szolgáltatottszoftver-fiók vagy egy ingyenes próbafiókot. Után jelentkezzen be a hello Dynatrace irányítópultot, válassza a **telepítése Dynatrace**.

    ![A PaaS integrációs Dynatrace beállítása](./media/container-service-monitoring-dynatrace/setup-paas.png)

3. Hello lapon jelölje be **PaaS-integráció beállítása**. 

    ![Dynatrace API jogkivonat](./media/container-service-monitoring-dynatrace/api-token.png) 

4. Adjon meg az API-token hello Dynatrace OneAgent konfigurációs hello DC/OS Universe belül. 

    ![A DC/OS Universe hello Dynatrace OneAgent konfiguráció](./media/container-service-monitoring-dynatrace/dynatrace-config.png)

5. Beállítása hello példányok toohello csomópontok toorun szeretné. Nagyobb értékre is működik, de a DC/OS tovább próbálkozik toofind új példányok eléréséig, hogy a kívánt ténylegesen. Ha szeretné, például 1000000 tooa érték is beállíthat. Ebben az esetben amikor egy új csomópont hozzáadása toohello fürt Dynatrace automatikusan telepíti a az ügynök toothat új csomópont, a DC/OS folyamatosan próbált toodeploy további példányokat hello áron.

    ![A DC/OS Universe-példányokat hello Dynatrace konfiguráció](./media/container-service-monitoring-dynatrace/dynatrace-config2.png)

## <a name="next-steps"></a>Következő lépések

Hello csomag telepítése után nyissa meg a visszafelé toohello Dynatrace irányítópult. Ismerje meg a különböző a szoftverhasználati mérési adatok hello az hello tárolókat a fürtön belül. 
