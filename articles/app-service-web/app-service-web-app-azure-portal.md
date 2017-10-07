---
title: "aaaReference navigáláshoz hello Azure-portálon"
description: "App Service Web hello különböző felhasználói élmények közötti hello Szolgáltatáskezelési portál és hello Azure Portal megismerése"
services: app-service
documentationcenter: 
author: jaime-espinosa
manager: erikre
editor: jimbe
ms.assetid: 0cc6a3cc-bd89-4a96-9177-d25f6fb737bb
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2016
ms.author: jaime-espinosa
ms.openlocfilehash: dcf7c1fc17f9a0c31005ad0f2fd53723d2966058
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reference-for-navigating-hello-azure-portal"></a>Útmutató a Navigálás hello Azure-portálon
Azure-webhelyek most nevezzük [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714). Frissítjük a dokumentáció tooreflect mindegyikét hello Azure-portál a neve a változás- és tooprovide utasításokat. Amíg a folyamat befejeződik, a webalkalmazásokkal az Azure-portálon hello használatához a dokumentum használhatja útmutatóként.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="hello-future-of-hello-azure-classic-portal"></a>hello jövőjét a klasszikus Azure portál hello
Amíg a módosítások a klasszikus Azure portál hello branding hello láthatja, a portál a felváltja hello Azure Portal hello folyamatban van. Hello klasszikus portál van kivezetése, mivel az új fejlesztési hello fókusz toohello Azure Portal van időeltolású. A Web Apps minden jövőbeli új funkcióit a hello Azure Portal határozza meg. Hello Azure Portal tootake előnyeinek legújabb és legjobb, Web Apps rendelkezik-e toooffer hello használatának megkezdése.

## <a name="layout-differences-between-hello-azure-classic-portal-and-azure-portal"></a>Hello klasszikus Azure portál és az Azure portál elrendezés különbségei
Klasszikus portál hello összes hello Azure services hello bal oldalán találhatók. A klasszikus portálon hello navigációs követi a faszerkezetben, ahol hello szolgáltatás-tól kezdődnek, és keresse meg az egyes elemek. Ez a struktúra jól működik, ha független összetevők kezeléséhez. Azonban, melyek Azure épülő alkalmazások összekapcsolt szolgáltatások, és a faszerkezet nem célszerű a szolgáltatások gyűjtemények használata. 

hello Azure portál segítségével könnyen toobuild alkalmazások-végpont több szolgáltatás-összetevőivel. hello portal formájában rendszerezett *utak*. A *út* sorozata *paneleken*, mely hello tárolói különböző összetevőket. Például beállítása automatikus skálázást a webes alkalmazás egy *út* amely viszi Több panelen látható módon a következő példa hello: hello **webhelyen** (hogy panel címe még nem frissített toouse panel új terminológia hello), hello **beállítások** panelen, és hello **horizontális felskálázás** panelen. Hello példában automatikus skálázás beállítása folyamatban van a CPU-használat toodepend úgy is egy **processzor** panelen. hello összetevőit hello *paneleken* nevezzük *részek*, amely csempék tűnik. 

![](./media/app-service-web-app-azure-portal/AutoScaling.png)

## <a name="navigation-example-create-a-web-app"></a>Navigációs példa: egy webalkalmazás létrehozása
Új webalkalmazások létrehozása a rendszer továbbra is egyszerű módon 1-2-3. a következő kép bemutatja hello klasszikus portál és hello portál egymás melletti toodemonstrate nem sokkal módosított hello több lépésből áll hello tooget szükséges, egy webalkalmazás be és futtatása. 

![](./media/app-service-web-app-azure-portal/CreateWebApp.png)

Hello portálon választhat hello leggyakoribb azokból a webalkalmazásokból, például egy WordPress népszerű gyűjtemény alkalmazáson. A választható alkalmazások teljes listájának megtekintéséhez keresse fel a hello [Azure piactér].

A webes alkalmazás létrehozásakor megadott URL-címet, az alkalmazásszolgáltatási csomag és a hely csak, mint a klasszikus portálon hello hello portálon. 

![](./media/app-service-web-app-azure-portal/CreateWebAppSettings.png)

Ezenkívül hello portál lehetővé teszi további közös beállításainak megadása. Például [erőforráscsoportok](../azure-resource-manager/resource-group-overview.md) egyszerű toosee tétele és a kapcsolódó Azure-erőforrások kezeléséhez. 

## <a name="navigation-example-settings-and-features"></a>Navigációs példa: beállítások és szolgáltatások
Összes hello beállítások és szolgáltatások logikailag csoportba kerülnek egy egyetlen panelen, amelyből léphet.

![](./media/app-service-web-app-azure-portal/WebAppSettings.png)

Például, létrehozhat egyéni tartományok kattintva **egyéni tartományok és SSL** a hello **beállítások** panelen.

![](./media/app-service-web-app-azure-portal/ConfigureWebApp.png)

a figyelési riasztások mentése tooset kattintson **kérések és hibák követése** , majd **riasztás hozzáadása**.

![](./media/app-service-web-app-azure-portal/Monitoring.png)

tooenable diagnosztika, kattintson a **diagnosztikai naplók** a hello **beállítások** panelen.

![](./media/app-service-web-app-azure-portal/Diagnostics.png)

tooconfigure alkalmazás beállításait, kattintson a **Alkalmazásbeállítások** a hello **beállítások** panelen. 

![](./media/app-service-web-app-azure-portal/AppSettingsPreview.png)

Eltérő hello márka felhasználónevet, néhány dolog hello portálon átnevezték vagy csoportosított másképp toomake azt könnyebb toofind őket. Az alábbiakban például van egy képernyőfelvétel a hello megfelelő lap az alkalmazásbeállítások (**konfigurálása**) hello a klasszikus portálon.

![](./media/app-service-web-app-azure-portal/AppSettings.png)

## <a name="more-resources"></a>További források
[Azure Portal]: https://portal.azure.com
[Azure piactér]: /marketplace/

> [!NOTE]
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

## <a name="whats-changed"></a>A változások
* Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)

