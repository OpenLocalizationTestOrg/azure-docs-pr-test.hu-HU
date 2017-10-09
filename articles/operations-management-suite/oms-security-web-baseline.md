---
title: "Felügyeleti csomag biztonsági és naplózási megoldás webes alapvető aaaOperations |} Microsoft Docs"
description: "Ez a dokumentum azt ismerteti, hogyan toouse OMS biztonsági és hitelesítési megoldás tooperform megfelelőségi és biztonsági célra minden figyelt webkiszolgálók webes alapkonfiguráció értékelését."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ROBOTS: NOINDEX
redirect_url: https://www.microsoft.com/cloud-platform/security-and-compliance
ms.assetid: 17837c8b-3e79-47c0-9b83-a51c6ca44ca6
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2017
ms.author: yurid
ms.openlocfilehash: 8aa87fa404ff97ab549dda3f9bebb75766055963
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="web-baseline-assessment-in-operations-management-suite-security-and-audit-solution"></a>A webes alapkonfiguráció értékelése az Operations Management Suite biztonsági és auditálási megoldásában
Ez a dokumentum segít toouse [Operations Management Suite (OMS) biztonsági és naplózási megoldás](operations-management-suite-overview.md) webalkalmazás-alapkonfiguráció értékelése képességek tooaccess hello a figyelt erőforrások biztonsági állapotát.

## <a name="what-is-web-baseline-assessment"></a>Mi az a webes alapkonfiguráció-értékelés?
Jelenleg az OMS biztonsági megoldása biztosít alapkonfiguráció-értékelést az operációs rendszerekhez. Hello operációsrendszer-beállításokat a kiszolgálók 24 óránként keres, és sebezhető beállítások nézetét jeleníti meg. Ezzel kapcsolatban további információkat olvashat [az alapkonfiguráció az Operations Management Suite biztonsági és auditálási megoldásában történő értékelését](oms-security-baseline.md) ismertető cikkben.

hello hello webes alapkonfiguráció értékelése célja toofind sebezhető webkiszolgálói beállítások. három elsődleges források hello a hello webes alapterv konfigurációk a következők: .NET, az ASP.NET és az IIS konfigurációját.  Ugyanúgy, mint az operációs rendszer alapkonfiguráció értékelése hello, OMS biztonsági érintetlen tooscan a webkiszolgálón minden 24hrs és azok biztonsági állapotának áttekintése.  Az Internet Information Service (IIS), konfigurációk olyan nagy mértékben testre szabható, amely lehetővé teszi, hogy a különböző helyek és alkalmazások szintek toobe felülbírálható. hello képolvasó hello beállítások hozzáadása toohello alapértelmezett gyökérszinten minden egyes alkalmazás-vagy helyvédelmi szinten ellenőrzi. Ez segít tooidentify lehetséges biztonsági beállítások helyeket, és gyorsan kijavítani.


## <a name="web-security-baseline-assessment"></a>A webes biztonsági alapkonfiguráció értékelése
Ez a szolgáltatás lesz az előzetes verzió toobe hello OMS keresési lehetőség használatával érhető el. Kövesse az alábbi tooperform sajátíthatja hello lekérdezés hello lépéseket:

1. A hello **a Microsoft Operations Management Suite** fő irányítópultján kattintson **biztonsági és naplózási** csempére.
2. A hello **biztonsági és naplózási** irányítópultján kattintson **naplófájl-keresési** gombra.
3. hello első lekérdezés használható hello **webes alapterv felmérésének összegzése**. A hello **Begin Keresés itt** mezőbe írja be a lekérdezést: típus*SecurityBaselineSummary BaselineType = webes =*. a következő képernyő hello egy kimeneti minta rendelkezik:

![Webes alapkonfiguráció-értékelés összegzése](./media/oms-security-web-baseline/oms-security-web-baseline-fig1-new.png)

> [!NOTE]
> Ebben a lekérdezésben minden rekord egy kiszolgáló értékelési összegzését jelöli.

Miután belépett hello **naplófájl-keresési**, beírhatja különböző lekérdezéseket tooobtain hello webes alapkonfiguráció értékelése további információt. Továbbá toohello előző lekérdezést, használhatja a következő azokat, ebben az előzetes verzióban hello.

**Webes alapkonfigurációsszabály-értékelés**: Minden rekord egy kiszolgáló webes alapkonfigurációsszabály-értékelését jelöli. Ez magában foglalja a hello szabály, hely, hello várt és tényleges értékét hello összes adatát.

**Lekérdezés**: Type*=SecurityBaseline BaselineType=web*

![Webes alapkonfigurációsszabály-értékelés](./media/oms-security-web-baseline/oms-security-web-baseline-fig2.png)

**Egy adott kiszolgálóhoz tartozó összes találat megjelenítése**: Ez a lekérdezés bemutatja, hogyan toosee eredménye a megadott kiszolgáló.

**Lekérdezés**: *Type=SecurityBaseline BaselineType=web Computer=BaselineTestVM1*

![Minden eredmény](./media/oms-security-web-baseline/oms-security-web-baseline-fig3.png)

Ezen rekordok/lekérdezések toocreate használhatja saját irányítópultokat, jelentéseket és riasztásokat is. az alábbi üdvözlő képernyőt rendelkezik egy minta felhasználói felületének vezérlői tooyour irányítópult adhat hozzá. Azt is megtudhatja, hogyan toovisualize OMS Nézettervező használata esetén az adatok [Itt](https://blogs.technet.microsoft.com/msoms/2016/06/30/oms-view-designer-visualize-your-data-your-way/). az alábbi üdvözlő képernyőt például hogyan hello csempe fog megjelenni a testreszabás után.

![Mintául szolgáló felhasználói felület](./media/oms-security-web-baseline/oms-security-web-baseline-fig4.png)

> [!NOTE]
> Ha azt szeretné tooknow hello beállításokat ellenőrzi hello alapkonfiguráció értékelése, letöltheti [az Excel-táblázat](https://gallery.technet.microsoft.com/OMS-Web-Baseline-1e811690) , amely tartalmazza ezeket a beállításokat.

## <a name="see-also"></a>Lásd még:
Ebben a dokumentumban az OMS biztonsági és auditálási megoldásának webes alapkonfiguráció-értékeléséről olvashatott. További információ az OMS biztonsági toolearn tekintse meg a következő cikkek hello:

* [Az Operations Management Suite (OMS) áttekintése](operations-management-suite-overview.md)
* [Figyelés és a válaszoló tooSecurity riasztásait az Operations Management Suite biztonsági és naplózási megoldás](oms-security-responding-alerts.md)
* [Az erőforrások figyelése az Operations Management Suite biztonsági és auditálási megoldásban](oms-security-monitoring-resources.md)

