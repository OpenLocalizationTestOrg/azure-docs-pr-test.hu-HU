---
title: "Webes alkalmazás Klónozás Azure portál használatával"
description: "Megtudhatja, hogyan klónozhatja a webalkalmazások Azure portál segítségével új webes alkalmazásokhoz."
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
ms.openlocfilehash: 9ebfa91c7972ab3c264032ead8376c23c1197a4b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-app-cloning-using-azure-portal"></a>Azure App Service alkalmazás Klónozás Azure-portál használatával
A Klónozási szolgáltatásának [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) megadható, hogy könnyen klónozását meglévő webalkalmazások egy újonnan létrehozott alkalmazást, egy másik régióban vagy ugyanabban a régióban. Ez lehetővé teszi az ügyfelek központi telepítése a alkalmazások számos különböző régiókban teljes gyorsan és egyszerűen.

Alkalmazás Klónozás jelenleg csak a premium szint app service-csomagokról támogatott. Az új szolgáltatás ugyanazokkal a korlátozásokkal használja, mint a Web Apps biztonsági másolat szolgáltatás című [készítsen biztonsági másolatot egy webalkalmazást az Azure App Service](web-sites-backup.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="cloning-an-existing-app"></a>A Klónozás egy meglévő alkalmazáshoz
A webalkalmazás futniuk kell a **prémium szintű** ahhoz, hogy a webalkalmazás klónozhatja mód.

1. Az a [Azure Portal](https://portal.azure.com/), nyissa meg a webalkalmazás panelen.
2. Kattintson a **eszközök**. Ezt követően a a **eszközök** panelen kattintson a **Klónozott alkalmazás**.
   
    ![][1]
   
   > [!NOTE]
   > Ha a webalkalmazás már nem része a **prémium** mód, kapni fog egy üzenetet, a támogatott módok app Klónozás. Ezen a ponton rendelkezik választhatja **frissítése**.
   > 
   > 
3. Az a **Klónozott alkalmazás** panelen adja meg egy nevet az új webalkalmazásba, a erőforráscsoport és az App Service-csomag. A felhasználó fog is, hogy a klón forrás a webalkalmazás-beállítások több e vagy sem.
   
    ![][2]
4. Miután rákattintott **létrehozása** a platform kezdő a forrás-webalkalmazás a klón létrehozásához.

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Egy meglévő alkalmazást az App Service-környezetek való klónozása
Az a **Klónozott alkalmazás** panel az ügyfél van egy meglévő App Service Environment-környezet egy alkalmazáskészlet kiválasztásához.

## <a name="current-restrictions"></a>Aktuális korlátozások
Ez a funkció jelenleg előzetes verzióban érhetők, új képességeket adhat a időbeli dolgozunk, a következő lista rendszer app klónozást Azure-portálon az aktuális támogatási ismert korlátozásai:

* Az Azure Traffic Manager-beállításai nem klónozható vannak
* Automatikus skálázási beállításokat a rendszer nem klónozható
* Biztonsági mentés ütemezése beállítások vannak nem klónozható
* Vannak nem klónozható a VNET beállításait
* App Insights nem automatikusan be vannak állítva a célként megadott webalkalmazásban
* Egyszerű hitelesítési beállítások a rendszer nem klónozható
* A kudu bővítmény nem klónozható vannak
* Tipp szabályok nem klónozható vannak
* Adatbázis-tartalom nem klónozható vannak

### <a name="references"></a>Referencia
* [Webes alkalmazás Klónozás PowerShell használatával](app-service-web-app-cloning.md)
* [Készítsen biztonsági másolatot egy webalkalmazást az Azure App Service-ben](web-sites-backup.md)
* [App Service Environment létrehozása](app-service-web-how-to-create-an-app-service-environment.md)
* [Webalkalmazás létrehozása App Service Environment-környezetben](app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [Az App Service Environment bemutatása](app-service-app-service-environment-intro.md)

<!--Image references-->
[1]: ./media/app-service-web-app-cloning-portal/CloningBlade.png
[2]: ./media/app-service-web-app-cloning-portal/CloneSettings.png
