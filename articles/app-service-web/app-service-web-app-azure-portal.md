---
title: "Útmutató az Azure-portálon lépjen a"
description: "Ismerje meg a különböző felhasználói élmények az App Service Web között a Szolgáltatáskezelési portál és az Azure portálon"
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
ms.openlocfilehash: d1ef6e87d82df0840e49412154df40cc937b320c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="reference-for-navigating-the-azure-portal"></a>Útmutató az Azure-portálon lépjen a
Azure-webhelyek most nevezzük [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714). A név változásnak, és utasításokkal láthatja el az Azure portál dokumentációban mindegyikét frissítjük. Amíg a folyamat befejeződik, használhatja a dokumentum útmutatóként dolgozik a webalkalmazásokkal az Azure-portálon.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="the-future-of-the-azure-classic-portal"></a>A jövőben is a klasszikus Azure-portálon
A márkajelzési változásai a klasszikus Azure portálon megfigyelheti, a portál napjainkban folyamatban van a felváltja az Azure portálon. A klasszikus portálon van kivezetése, mivel a fókusz az új fejlesztési van időeltolású az Azure-portálra. A Web Apps minden jövőbeli új funkcióit határozza meg az Azure portálon. Indítsa el az Azure portál használatával, hogy webalkalmazásokat fel kell-e a legújabb és legjobb előnyeinek kihasználása érdekében.

## <a name="layout-differences-between-the-azure-classic-portal-and-azure-portal"></a>A klasszikus Azure portál és az Azure portál elrendezés különbségei
A klasszikus portálon minden Azure-szolgáltatások bal oldali listája látható. A klasszikus portálon navigációs követi a faszerkezetben, amelyen a szolgáltatás indítása, és keresse meg az egyes elemek. Ez a struktúra jól működik, ha független összetevők kezeléséhez. Azonban, melyek Azure épülő alkalmazások összekapcsolt szolgáltatások, és a faszerkezet nem célszerű a szolgáltatások gyűjtemények használata. 

Az Azure-portálon megkönnyíti az alkalmazások-végpont több szolgáltatás-összetevőivel felépítéséhez. A portál formájában rendszerezett *utak*. A *út* sorozata *paneleken*, amely a különböző összetevők tárolói. Például beállítása automatikus skálázást a webes alkalmazás egy *út* amely viszi Több paneleket az alábbi példában látható módon: a **webhelyen** (hogy panel címe nem még megújult az új panel terminológiát), a **beállítások** panelt, és a **horizontális felskálázás** panelen. A példában automatikus skálázás beállítása folyamatban van a CPU-használat függ, így nincs is egy **processzor** panelen. Az összetevők belül a *paneleken* nevezzük *részek*, amely csempék tűnik. 

![](./media/app-service-web-app-azure-portal/AutoScaling.png)

## <a name="navigation-example-create-a-web-app"></a>Navigációs példa: egy webalkalmazás létrehozása
Új webalkalmazások létrehozása a rendszer továbbra is egyszerű módon 1-2-3. A következő kép bemutatja a klasszikus portál és a portál-párhuzamos annak bemutatásához, hogy nem sokkal kevesebb lépésben a webes alkalmazás eléréséhez szükséges, és fut a megváltozott. 

![](./media/app-service-web-app-azure-portal/CreateWebApp.png)

A portál a leggyakoribb webes alkalmazásokat, akár népszerű gyűjtemény alkalmazások, például WordPress közül választhat. A választható alkalmazások teljes listájának megtekintéséhez keresse fel a [Azure piactér].

A webes alkalmazás létrehozásakor megadott URL-címet, az alkalmazásszolgáltatási csomag és a hely csak, mint a klasszikus portál a portálon. 

![](./media/app-service-web-app-azure-portal/CreateWebAppSettings.png)

Emellett a portál segítségével más általános beállításainak megadása. Például [erőforráscsoportok](../azure-resource-manager/resource-group-overview.md) révén egyszerűen áttekinthetők és felügyelhetők a kapcsolódó Azure-erőforrások. 

## <a name="navigation-example-settings-and-features"></a>Navigációs példa: beállítások és szolgáltatások
A beállítások és szolgáltatások logikailag csoportba kerülnek egy egyetlen panelen, amelyből léphet.

![](./media/app-service-web-app-azure-portal/WebAppSettings.png)

Például, létrehozhat egyéni tartományok kattintva **egyéni tartományok és SSL** a a **beállítások** panelen.

![](./media/app-service-web-app-azure-portal/ConfigureWebApp.png)

A figyelési riasztások beállításához kattintson **kérések és hibák követése** , majd **hozzáadása riasztás**.

![](./media/app-service-web-app-azure-portal/Monitoring.png)

Diagnosztika engedélyezéséhez kattintson **diagnosztikai naplók** a a **beállítások** panelen.

![](./media/app-service-web-app-azure-portal/Diagnostics.png)

Az alkalmazás beállításainak megadásához kattintson **Alkalmazásbeállítások** a a **beállítások** panelen. 

![](./media/app-service-web-app-azure-portal/AppSettingsPreview.png)

Eltérő márka felhasználónevet néhány dolgot a portálon átnevezték vagy azok megkereséséről könnyebb másképp csoportosítva. Az alábbiakban például van egy Képernyőkép a beállítások a megfelelő lap (**konfigurálása**) a klasszikus portálon.

![](./media/app-service-web-app-azure-portal/AppSettings.png)

## <a name="more-resources"></a>További források
[Azure Portal]: https://portal.azure.com
[Azure piactér]: /marketplace/

> [!NOTE]
> Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

## <a name="whats-changed"></a>A változások
* Információk a Websites szolgáltatásról az App Service-re való váltásról: [Az Azure App Service és a hatása a meglévő Azure-szolgáltatásokra](http://go.microsoft.com/fwlink/?LinkId=529714)

