---
title: "Végpontok kezelése az Azure Traffic Managerben | Microsoft Docs"
description: "Ez a cikk a végpontok Azure Traffic Managerben végzett felvételében, eltávolításában, engedélyezésében és letiltásában segít."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: ade2bbc2-35a7-43c5-8001-4698f7254526
ms.service: traffic-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/08/2017
ms.author: kumud
ms.openlocfilehash: 765d12bc283d991783fb3190ce7917b573f9fc78
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="add-disable-enable-or-delete-endpoints"></a>Végpontok felvétele, letiltása, engedélyezése és törlése

Az Azure App Service Web Apps szolgáltatása is biztosít feladatátvételi és ciklikus időszeletelési forgalom-útválasztási funkciót az adatközponton belüli webhelyekhez, a webhely működési módjától függetlenül. Az Azure Traffic Manager azonban lehetővé teszi a feladatátvételi és a ciklikus időszeletelési forgalom-útválasztás beállítását a különböző adatközpontokban elhelyezett webhelyek és felhőszolgáltatások esetén is. A funkció üzembe állításának első lépése a felhőszolgáltatás vagy a webhely típusú végpont felvétele a Traffic Manager szolgáltatásba.

A Traffic Manager-profil részét képező egyedi végpontok is letilthatók. A végpont letiltása után a végpont a profil része marad, de a profil úgy viselkedik, mintha a végpont nem szerepelne benne. Ez a művelet a karbantartási módban lévő vagy újratelepítés alatt álló végpont átmeneti eltávolítására való. Amint a végpont újra működik, engedélyezhető.

> [!NOTE]
> A végpont letiltása nem érinti a végpont Azure rendszerbeli üzembe helyezési állapotát. Egy megfelelően működő végpont a Traffic Managerben történő letiltás után is aktív marad és képes az adatforgalom fogadására. Továbbá a végpont egyik profilban végzett letiltása nem befolyásolja a többi profilban érvényes állapotát.

## <a name="to-add-a-cloud-service-or-an-app-service-endpoint-to-a-traffic-manager-profile"></a>Felhőszolgáltatás- vagy App Service-végpont hozzáadása Traffic Manager-profilhoz

1. Egy böngészőben jelentkezzen be az [Azure Portalra](http://portal.azure.com).
2. A portál keresősávjában keressen rá a módosítani kívánt **Traffic Manager-profil** nevére, majd kattintson a Traffic Manager-profilra a megjelenített eredmények között.
3. A **Traffic Manager-profil** panel **Beállítások** szakaszában kattintson a **Végpontok** elemre.
4. A megjelenő **Végpontok** panelen kattintson a **Hozzáadás** gombra.
5. A **Végpont hozzáadása** panelt töltse ki az alábbiak szerint:
    1. A **Típusnál** kattintson az **Azure-végpont** lehetőségre.
    2. Adjon meg egy **Nevet**, amelyről felismeri majd a végpontot.
    3. A **Célerőforrás típusánál** válassza ki a megfelelő erőforrástípust a legördülő listából.
    4. A **Célerőforrásnál** válassza ki a megfelelő célerőforrást a legördülő listából az előfizetéshez tartozó, az **Erőforrások panelen** listázott erőforrások megjelenítéséhez. A megjelenő **Erőforrás** panelen válassza ki az első végpontként hozzáadni kívánt szolgáltatást.
    5. A **Prioritásnál** válassza az **1-es** értéket. Ennek eredményeképpen a teljes forgalom erre a végpontra irányul, ha ez nem befolyásolja a rendszer megfelelő működését.
    6. A **Beállítás letiltottként** jelölőnégyzetet ne jelölje ki.
    7. Kattintson az **OK** gombra
6.  A 4–5. lépés megismétlésével adja hozzá a következő Azure-végpontot. Ennek a **Prioritása** mindenképpen **2-es** legyen.
7.  Miután mindkét végpontot hozzáadta, azok megjelennek a **Traffic Manager-profil** panelen, **Online** figyelési állapottal.

> [!NOTE]
> Miután a *Feladatátvitel* forgalom-útválasztási módszer segítségével hozzáad vagy eltávolít egy végpontot a profilból, előfordulhat, hogy a feladatátvitel prioritási listát nem rendezheti át úgy, ahogy szeretné. A feladatátvétel prioritási lista sorrendjét a konfigurációs lapon adhatja meg. További információkért tekintse meg a [Feladatátvételi forgalom-útválasztás beállítása](traffic-manager-configure-failover-routing-method.md) szakaszt.

## <a name="to-disable-an-endpoint"></a>A végpontok letiltása

1. Egy böngészőben jelentkezzen be az [Azure Portalra](http://portal.azure.com).
2. A portál keresősávjában keressen rá a módosítani kívánt **Traffic Manager-profil** nevére, majd kattintson a Traffic Manager-profilra a megjelenített eredmények között.
3. A **Traffic Manager-profil** panel **Beállítások** szakaszában kattintson a **Végpontok** elemre. 
4. Kattintson a letiltani kívánt végpontra, majd a megjelenő **Végpont** panelen a **Szerkesztés** gombra.
5. A **Végpont** panelen állítsa a végpontot **Letiltva** állapotba, majd kattintson a **Mentés** gombra.
6. Az ügyfelek az élettartam (TTL) végéig továbbítják az adatforgalmat a végpont felé. Az élettartamot a Traffic Manager profil konfigurációs panelén módosíthatja.

## <a name="to-enable-an-endpoint"></a>A végpontok engedélyezése

1. Egy böngészőben jelentkezzen be az [Azure Portalra](http://portal.azure.com).
2. A portál keresősávjában keressen rá a módosítani kívánt **Traffic Manager-profil** nevére, majd kattintson a Traffic Manager-profilra a megjelenített eredmények között.
3. A **Traffic Manager-profil** panel **Beállítások** szakaszában kattintson a **Végpontok** elemre. 
4. Kattintson a letiltani kívánt végpontra, majd a megjelenő **Végpont** panelen a **Szerkesztés** gombra.
5. A **Végpont** panelen állítsa a végpontot **Engedélyezve** állapotba, majd kattintson a **Mentés** gombra.
6. Az ügyfelek az élettartam (TTL) végéig továbbítják az adatforgalmat a végpont felé. Az élettartamot a Traffic Manager profil konfigurációs panelén módosíthatja.

## <a name="to-delete-an-endpoint"></a>Végpont törlése

1. Egy böngészőben jelentkezzen be az [Azure Portalra](http://portal.azure.com).
2. A portál keresősávjában keressen rá a módosítani kívánt **Traffic Manager-profil** nevére, majd kattintson a Traffic Manager-profilra a megjelenített eredmények között.
3. A **Traffic Manager-profil** panel **Beállítások** szakaszában kattintson a **Végpontok** elemre. 
4. Kattintson a letiltani kívánt végpontra, majd a megjelenő **Végpont** panelen a **Szerkesztés** gombra.
5. A **Végpont** panelen állítsa a végpontot **Engedélyezve** állapotba, majd kattintson a **Mentés** gombra.


## <a name="next-steps"></a>Következő lépések

* [Traffic Manager-profilok kezelése](traffic-manager-manage-profiles.md)
* [Útválasztási módszerek konfigurálása](traffic-manager-configure-routing-method.md)
* [A Traffic Manager csökkentett teljesítményének elhárítása](traffic-manager-troubleshooting-degraded.md)
* [A Traffic Manager teljesítményével kapcsolatos megfontolások](traffic-manager-performance-considerations.md)
* [A Traffic Manager műveletei (REST API-referencia)](http://go.microsoft.com/fwlink/p/?LinkID=313584)

