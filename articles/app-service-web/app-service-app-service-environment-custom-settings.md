---
title: "App Service Environment-környezetek aaaCustom beállításai"
description: "App Service Environment-környezetek egyéni konfigurációs beállításai"
services: app-service
documentationcenter: 
author: stefsch
manager: nirma
editor: 
ms.assetid: 1d1d85f3-6cc6-4d57-ae1a-5b37c642d812
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2016
ms.author: stefsch
ms.openlocfilehash: 3d140688c88b389e71bfdd465c418339cccab3a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="custom-configuration-settings-for-app-service-environments"></a>App Service Environment-környezetek egyéni konfigurációs beállításai
## <a name="overview"></a>Áttekintés
Mivel az App Service Environment-környezetek elkülönített tooa egyetlen felhasználói, vannak bizonyos használható konfigurációs beállítások alkalmazása kizárólag tooApp környezetek. Ez a cikk dokumentumok hello App Service Environment-környezetek számára elérhető különböző testreszabásokat.

Ha még nem rendelkezik az App Service-környezetek, lásd: [hogyan tooCreate az App Service-környezetek](app-service-web-how-to-create-an-app-service-environment.md).

App Service Environment-környezet testreszabások tárolhatja használatával tömb hello új **clusterSettings** attribútum. Ez az attribútum található hello "Tulajdonságok" szótára hello *hostingEnvironments* Azure Resource Manager entitás.

hello rövidített Resource Manager sablon alábbi kódrészletben láthatja hello **clusterSettings** attribútum:

    "resources": [
    {
       "apiVersion": "2015-08-01",
       "type": "Microsoft.Web/hostingEnvironments",
       "name": ...,
       "location": ...,
       "properties": {
          "clusterSettings": [
             {
                 "name": "nameOfCustomSetting",
                 "value": "valueOfCustomSetting"
             }
          ],
          "workerPools": [ ...],
          etc...
       }
    }

Hello **clusterSettings** attribútum tartalmazhat egy Resource Manager sablon tooupdate hello App Service Environment-környezet.

## <a name="use-azure-resource-explorer-tooupdate-an-app-service-environment"></a>Használja az Azure erőforrás-kezelő tooupdate egy App Service Environment-környezet
Másik megoldásként frissítheti hello App Service Environment-környezet használatával [Azure erőforrás-kezelő](https://resources.azure.com).  

1. Az erőforrás-kezelőben, nyissa meg az App Service Environment-környezet hello toohello csomópontot (**előfizetések** > **resourceGroups** > **szolgáltatók**  >  **Microsoft.Web** > **hostingEnvironments**). Kattintson az adott App Service-környezet, amelyet az tooupdate hello.
2. Hello jobb oldali ablaktáblában kattintson **olvasási/írási** a hello felső eszköztár tooallow interaktív szerkesztése az erőforrás-kezelőben.  
3. Kattintson a kék hello **szerkesztése** gomb toomake hello Resource Manager sablon szerkeszthető.
4. Toohello hello jobb oldali ablaktábla alján görgessen. Hello **clusterSettings** attribútum jelenleg hello legalsó, ahol adja meg, vagy frissítse az értéket.
5. Hello kívánt konfigurációs értékeket típusa (vagy a másolás és beillesztés) hello tömbje **clusterSettings** attribútum.  
6. Kattintson a zöld hello **PUT** gombra kattint, amely rendelkezik hello jobb oldali toocommit hello módosítása toohello App Service Environment-környezet hello tetején található.

Hello módosítás elküldését, de hello App Service Environment-környezet tootake hatás hello módosítása az előtér-webkiszolgálóinak hello számát szorozva körülbelül 30 percet vesz igénybe.
Például ha az App Service-környezetek négy előtér-webkiszolgálóinak, körülbelül két órát hello konfigurációs frissítés toofinish tart. Hello konfigurációváltozás megkezdődött van, míg más skálázási műveletek vagy konfiguráció-módosítási műveletek nem történhet a hello App Service Environment-környezet.

## <a name="disable-tls-10"></a>A TLS 1.0 letiltása
Ismétlődő kérdés az ügyfelektől, különösen az ügyfelek, akik a PCI-megfelelőséget foglalkoznak eseményeket, a hogyan tooexplicitly letiltja a TLS 1.0 alkalmazásaikat.

A TLS 1.0 hello következő keresztül letiltható **clusterSettings** bejegyzést:

        "clusterSettings": [
            {
                "name": "DisableTls1.0",
                "value": "1"
            }
        ],

## <a name="change-tls-cipher-suite-order"></a>Módosítsa a TLS titkosító csomag sorrendje
-Ügyfél egy másik kérdést akkor, ha azok módosíthatja, hogy a kiszolgáló által egyeztetett titkosítási hello listáját, és ez elérhető hello módosításával **clusterSettings** alább látható módon. hello elérhető titkosító csomagok listája olvasható be a [MSDN-cikkben](https://msdn.microsoft.com/library/windows/desktop/aa374757\(v=vs.85\).aspx).

        "clusterSettings": [
            {
                "name": "FrontEndSSLCipherSuiteOrder",
                "value": "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384_P256,TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256"
            }
        ],

> [!WARNING]
> Érvénytelen értékek vannak beállítva, a SChannel nem érti hello titkosítócsomag, ha minden TLS kommunikációs tooyour server előfordulhat, hogy működni. Ebben az esetben szüksége lesz a tooremove hello *FrontEndSSLCipherSuiteOrder* bejegyzést **clusterSettings** , és küldje el a hello Resource Manager sablon toorevert hátsó toohello alapértelmezett titkosítás frissítése csomag beállításait.  Körültekintően használja ezt a funkciót.
> 
> 

## <a name="get-started"></a>Bevezetés
hello Azure gyors üzembe helyezés Resource Manager sablon webhely alap definíciója hello rendelkező sablont tartalmaz [egy App Service Environment-környezet létrehozása](https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/).

<!-- LINKS -->

<!-- IMAGES -->
