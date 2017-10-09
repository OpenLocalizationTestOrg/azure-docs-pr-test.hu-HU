---
title: "aaaWeb App Klónozás Azure portál használatával"
description: "Megtudhatja, hogyan tooclone a Web Apps toonew webalkalmazások Azure portál használatával."
services: app-service\web
documentationcenter: 
author: ahmedelnably
manager: stefsch
editor: 
ms.assetid: 20b0ae4e-67e8-4bae-9d74-8a24dc445cce
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2016
ms.author: aelnably
ms.openlocfilehash: 605c4879f34d568e9981c34109f9496731c9ed18
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-app-cloning-using-azure-portal"></a>Azure App Service alkalmazás Klónozás Azure-portál használatával
a szolgáltatás a Klónozás hello [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) megadható, hogy könnyen klónozza a meglévő alkalmazások tooa az újonnan létrehozott webalkalmazás egy másik régióban található, vagy a hello ugyanabban a régióban. Ezzel a lépéssel engedélyezi az ügyfelek toodeploy alkalmazások számos különböző régiókban között gyors és egyszerű.

Alkalmazás Klónozás jelenleg csak a premium szint app service-csomagokról támogatott. új hello szolgáltatás által használt azonos hello korlátozások Web Apps biztonsági másolat szolgáltatás, lásd: [készítsen biztonsági másolatot egy webalkalmazást az Azure App Service](web-sites-backup.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="cloning-an-existing-app"></a>A Klónozás egy meglévő alkalmazáshoz
hello webalkalmazás futnia kell a hello **prémium** ahhoz, hogy a klónozott hello webalkalmazás toocreate mód.

1. A hello [Azure Portal](https://portal.azure.com/), nyissa meg a webalkalmazás panelen.
2. Kattintson a **eszközök**. Ezt követően a hello **eszközök** panelen kattintson a **Klónozott alkalmazás**.
   
    ![][1]
   
   > [!NOTE]
   > Ha hello webes alkalmazás nem szerepel a hello **prémium** mód, kapni fog egy üzenetet támogatott hello módjainak app Klónozás. Ezen a ponton rendelkezik hello beállítás tooselect **frissítése**.
   > 
   > 
3. A hello **Klónozott alkalmazás** panelen adja meg a hello új webalkalmazást, az erőforráscsoport és az App Service-csomag nevét. Is hello felhasználó fog tudni toochoose e forrást a webalkalmazás-beállítások több tooclone vagy sem.
   
    ![][2]
4. Miután rákattintott **létrehozása** hello platform kezdő hello forrás webalkalmazás a klón létrehozásához.

## <a name="cloning-an-existing-app-tooan-app-service-environment"></a>A Klónozás egy meglévő App tooan App Service Environment-környezet
A hello **Klónozott alkalmazás** panel hello ügyfél fog rendelkezni a hello beállítás toochoose egy alkalmazáskészlet a meglévő App Service-környezet.

## <a name="current-restrictions"></a>Aktuális korlátozások
Ez a funkció jelenleg előzetes verzióban érhetők, jelenleg is dolgozunk tooadd új képességeket adott idő alatt, a következő lista hello vannak hello alkalmazás Azure-portálon Klónozás hello aktuális támogatási ismert korlátozásai:

* Az Azure Traffic Manager-beállításai nem klónozható vannak
* Automatikus skálázási beállításokat a rendszer nem klónozható
* Biztonsági mentés ütemezése beállítások vannak nem klónozható
* Vannak nem klónozható a VNET beállításait
* App Insights nem automatikusan be vannak állítva hello cél webalkalmazásban
* Egyszerű hitelesítési beállítások a rendszer nem klónozható
* A kudu bővítmény nem klónozható vannak
* Tipp szabályok nem klónozható vannak
* Adatbázis-tartalom nem klónozható vannak

### <a name="references"></a>Referencia
* [Webes alkalmazás Klónozás PowerShell használatával](app-service-web-app-cloning.md)
* [Készítsen biztonsági másolatot egy webalkalmazást az Azure App Service-ben](web-sites-backup.md)
* [Hogyan tooCreate egy App Service Environment-környezet](app-service-web-how-to-create-an-app-service-environment.md)
* [Webalkalmazás létrehozása App Service Environment-környezetben](app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [Bevezetés tooApp Service-környezet](app-service-app-service-environment-intro.md)

<!--Image references-->
[1]: ./media/app-service-web-app-cloning-portal/CloningBlade.png
[2]: ./media/app-service-web-app-cloning-portal/CloneSettings.png
