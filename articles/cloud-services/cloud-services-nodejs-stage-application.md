---
title: "egy felhőalapú szolgáltatás telepítését (Node.js) aaaStage |} Microsoft Docs"
description: "Megtudhatja, hogyan toodeploy az Azure-alkalmazásokban tooa átmeneti környezet, majd telepítse a virtuális IP-cím (VIP) swap használatával tooa éles környezetben."
services: cloud-services
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: d65d26a6-b424-49cd-a88c-7ef46bb112a8
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2017
ms.author: tarcher
ms.openlocfilehash: 0656c84ac444a10f152f441265f85141347bf1fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="staging-an-application-in-azure"></a>Átmeneti egy alkalmazás az Azure-ban
A burkolt alkalmazás lehet átmeneti Azure toobe tesztelt mozgatása előtt toohello éles környezetben telepített toohello mely hello alkalmazás elérhető-e hello internetes környezetben. Az átmeneti hasonlít pontosan hello éles környezetben, azzal a különbséggel, hogy csak a hozzáférési hello előkészített rejtjelezett URL-címének Azure által létrehozott alkalmazás. Miután ellenőrizte, hogy az alkalmazás megfelelően működik-e, éles környezetben telepített toohello lehet egy virtuális IP-cím (VIP) felcserélés elvégzésével.

> [!NOTE]
> hello cikkben leírt lépéseket csak érvényes Azure felhőalapú szolgáltatásként üzemeltetett toonode alkalmazások.
> 
> 

## <a name="step-1-stage-an-application"></a>1. lépés: Az alkalmazás tesztelése
Ez a feladat ismerteti, hogyan toostage egy alkalmazás használatával hello **Microsoft Azure PowerShell**.

1. Egy szolgáltatás közzétételekor egyszerűen adja át hello **-tárolóhely** hello paramétert **Publish-AzureServiceProject** parancsmag.
   
   ```powershell
   Publish-AzureServiceProject -Slot staging
   ```
2. Jelentkezzen be toohello [a klasszikus Azure portálon] válassza ki **Felhőszolgáltatások**. Létrehozása után hello felhőalapú szolgáltatás, és hello **átmeneti** oszlop állapota túl lett módosítva**futtató**, kattintson a hello szolgáltatás neve.
   
   ![a portál egy futó szolgáltatással megjelenítése][cloud-service]
3. Jelölje be hello **irányítópult**, majd válassza ki **átmeneti**.
   
   ![cloud service irányítópult][cloud-service-dashboard]
4. Jegyezze fel a hello hello értékét **webhely URL-címe** bejegyzés toohello jobbra. hello DNS-neve: egy rejtjelezett belső azonosítója, ami Azure jön létre.
   
    ![webhely URL-címe][cloud-service-staging-url]

Most már ellenőrizheti, hogy hello alkalmazás az átmeneti környezet hello átmeneti hely URL-cím használatával hello megfelelően működjön.

## <a name="step-2-upgrade-an-application-in-production-by-swapping-vips"></a>2. lépés: A tárhellyel való felcserélése VIP által éles alkalmazás frissítése
Miután ellenőrizte, hogy egy alkalmazás, a tesztelési környezetben hello frissített verzióját, akkor gyorsan elérhetővé teheti azt éles környezetben által áttelepíteni a forráskörnyezetból hello virtuális IP-címek (VIP) hello átmeneti és üzemi környezetben.

> [!NOTE]
> Ez a lépés feltételezi, hogy már telepített egy alkalmazás tooproduction és előkészített hello az alkalmazásnak a frissített verziója.
> 
> 

1. Jelentkezzen be a hello [a klasszikus Azure portálon], kattintson a **Felhőszolgáltatások** , és válassza a hello szolgáltatás neve.
2. A hello **irányítópult**, jelölje be **átmeneti**, és kattintson a **felcserélése** hello lap hello alján. Ekkor megnyílik a virtuális IP-Címcsere párbeszédpanel hello.
   
   ![VIP-csere párbeszédpanel][vip-swap-dialog]
3. Tekintse át a hello információkat, majd **OK**. hello két központi telepítések kezdhető meg, mint a központi telepítési kapcsolók tooproduction és hello éles telepítési kapcsolók toostaging átmeneti hello frissítése.

Sikeresen előkészítette a központi telepítés, és éles környezet frissíteni a virtuális IP-címek csere a tesztelési hello telepítés.

## <a name="additional-resources"></a>További források
* [Hogyan tooDeploy a szolgáltatás frissítése tooProduction által áttelepíteni a Forráskörnyezetból virtuális IP-címek az Azure-ban]

[a klasszikus Azure portálon]: http://manage.windowsazure.com
[cloud-service]: ./media/cloud-services-nodejs-stage-application/staging-cloud-service-running.png
[cloud-service-dashboard]: ./media/cloud-services-nodejs-stage-application/cloud-service-dashboard-staging.png
[cloud-service-staging-url]: ./media/cloud-services-nodejs-stage-application/cloud-service-staging-url.png
[vip-swap-dialog]: ./media/cloud-services-nodejs-stage-application/vip-swap-dialog.png
[Hogyan tooDeploy a szolgáltatás frissítése tooProduction által áttelepíteni a Forráskörnyezetból virtuális IP-címek az Azure-ban]: cloud-services-how-to-manage.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production
