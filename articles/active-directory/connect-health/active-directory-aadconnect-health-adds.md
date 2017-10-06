---
title: "az Azure AD Connect Health és az AD DS aaaUsing |} Microsoft Docs"
description: "Ez a hello Azure AD Connect Health oldal bemutatja, hogyan lehet hogyan toomonitor Active Directory tartományi Szolgáltatásokban."
services: active-directory
documentationcenter: 
author: arluca
manager: femila
editor: curtand
ms.assetid: 19e3cf15-f150-46a3-a10c-2990702cd700
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: e2fb6be65407d02c214dcab385b85d6cb54f48de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-ad-connect-health-with-ad-ds"></a>Az Azure AD Connect Health használata az AD DS szolgáltatással
a következő dokumentáció hello adott toomonitoring Active Directory tartományi szolgáltatások az Azure AD Connect Health. Active Directory tartományi szolgáltatások hello támogatott verziói: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 és Windows Server 2016.

Az AD FS az Azure AD Connect Health használatával történő megfigyelésére vonatkozó további információkat lásd: [Az Azure AD Connect Health használata az AD FS szolgáltatással](active-directory-aadconnect-health-adfs.md). Az Azure AD Connect (szinkronizálási szolgáltatás) az Azure AD Connect Health használatával történő megfigyelésével kapcsolatos információkat [Az Azure AD Connect Health szinkronizálási szolgáltatás használata](active-directory-aadconnect-health-sync.md) című témakörben tekintheti meg.

![Azure AD Connect Health for AD DS](./media/active-directory-aadconnect-health/aadconnect-health-adds-entry.png)

## <a name="alerts-for-azure-ad-connect-health-for-ad-ds"></a>Az Azure AD Connect Health for AD DS riasztásai
Riasztások szakaszban az Azure AD Connect Health belül hello Active Directory tartományi szolgáltatások, az aktív és megoldott riasztások, a kapcsolódó tooyour tartományvezérlők listáját jeleníti meg. Egy aktív vagy megoldott riasztás kiválasztása megoldási lépések, valamint további információt egy új panel nyílik meg, és kapcsolatok toosupporting dokumentációját. Minden riasztási típus egy vagy több példánya, amely adott riasztás által érintett hello tartományvezérlők tooeach is lehet. Kis hatótávolságú hello alsó hello riasztási panelről kattintson duplán az érintett tartomány a tartományvezérlő tooopen egy további riasztási példánynak kapcsolatos további részletek panelről.

Ezen a panelen belül engedélyezheti a riasztások értesítő e-mailek és hello időtartomány megtekintési módosítása. Hello időtartomány kibővítése lehetővé teszi toosee előzetes megoldott riasztások.

![Azure AD Connect szinkronizálási szolgáltatás – hiba](./media/active-directory-aadconnect-health/aadconnect-health-adds-alerts.png)

## <a name="domain-controllers-dashboard"></a>A Tartományvezérlők irányítópult
Ez az irányítópult a környezet topológiai áttekintését teszi lehetővé, a főbb üzemeltetési mérőszámokkal és minden megfigyelt tartományvezérlő állapotadataival. hello jelenik meg a metrikák tooquickly azonosítására, a bármely olyan tartományvezérlőn, amely lehet szükség további vizsgálatra. Alapértelmezés szerint csak hello oszlopok csoportja jelenik meg. Azonban található hello teljes készletét megtisztítja az elérhető oszlopok hello oszlopok parancs duplán kattintva. Leggyakrabban az Ön számára legfontosabb, viszont ez az irányítópult és egyszerű, egyetlen helyezze el az AD DS-környezet állapotának tooview hello hello oszlopok kiválasztása.

![Tartományvezérlők](./media/active-directory-aadconnect-health/aadconnect-health-adds-domainsandsites-dashboard.png)

A megfelelő tartomány vagy a helyhez, ami segít annak megértésében hello környezet topológia tartományvezérlők csoportosíthatók. Végül hello panel fejléc duplán kattint, a hello irányítópult maximalizálja tooutilize hello elérhető képernyő-ingatlan. Ez a nagyobb nézet több oszlop megjelenítése esetén hasznos.

## <a name="replication-status-dashboard"></a>A Replikációs állapot irányítópult
Ez az irányítópult hello állapotát és a replikációs topológiát a figyelt tartományvezérlők nézetét biztosítja. hello hello legutóbbi replikáció kísérlet állapotot, és a hasznos dokumentáció az esetleges hibákat talált. Hiba történt, tooopen információt egy új panel egy olyan tartományvezérlőre, mint duplán kattintva: hello hibával, ajánlott megoldási lépések, és a tootroubleshooting dokumentációját.

![Replikáció állapota](./media/active-directory-aadconnect-health/aadconnect-health-adds-replication.png)

## <a name="monitoring"></a>Figyelés
Ez a funkció különböző teljesítményszámlálók, amelyet a rendszer folyamatosan gyűjti az egyes hello figyelt tartományvezérlők grafikus trendek biztosítja. Egy tartományvezérlő teljesítménye könnyen összehasonlítható más megfigyelt tartományvezérlőkkel az erdőben. Ezenfelül különböző teljesítményszámlálókat láthat egymás mellett, amely a környezetében történő hibaelhárítás során lehet hasznos.

![Figyelés](./media/active-directory-aadconnect-health/aadconnect-health-adds-monitoring.png)

Alapértelmezés szerint azt négy teljesítményszámlálók; van megadva azonban megadhat mások által hello szűrő parancsra kattint, és jelölje be vagy törölje a kívánt teljesítményszámlálókat. Továbbá kattintson duplán a teljesítmény számláló graph tooopen egy új panel, amelyen az egyes hello figyelt tartományvezérlők adatpontok tartalmazza.

## <a name="related-links"></a>Kapcsolódó hivatkozások
* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Az Azure AD Connect Health-ügynök telepítése](active-directory-aadconnect-health-agent-install.md)
* [Az Azure AD Connect Health műveletei](active-directory-aadconnect-health-operations.md)
* [Az Azure AD Connect Health használata az AD FS szolgáltatással](active-directory-aadconnect-health-adfs.md)
* [Az Azure AD Connect Health szinkronizálási szolgáltatás használata](active-directory-aadconnect-health-sync.md)
* [Azure AD Connect Health – gyakori kérdések](active-directory-aadconnect-health-faq.md)
* [Az Azure AD Connect Health verzióelőzményei](active-directory-aadconnect-health-version-history.md)

