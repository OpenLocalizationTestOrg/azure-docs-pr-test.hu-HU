---
title: "Az adatgyűjtést az Azure Security Centerben |} Microsoft Docs"
description: " Megtudhatja, hogyan szeretné az adatgyűjtést az Azure Security Centerben. "
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: terrylan
ms.openlocfilehash: 7e9ad8cd8c77c57c37dc208b86b3727a4e1dc7b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="enable-data-collection-in-azure-security-center"></a>Az adatgyűjtést az Azure Security Centerben

> [!NOTE]
> 2017 júniusának elejétől kezdve a Security Center a Microsoft Monitoring Agent használatával gyűjti össze és tárolja az adatokat. További tudnivalókért lásd: [Azure Security Center Platform áttelepítési](security-center-platform-migration.md). A jelen cikkben található információk a Security Center a Microsoft Monitoring Agentre való váltás után elérhető funkcióit ismertetik.
>
>

A Security Center adatokat gyűjt a virtuális gépekről a biztonsági állapotuk értékeléséhez, a biztonsági javaslatok létrehozásához és a fenyegetésekkel kapcsolatos riasztásokhoz. Amikor először nyitja meg a Security Center, lehetősége van az előfizetés virtuális gépeinek gyűjtésének engedélyezése. Adatok gyűjtése nem engedélyezett, ha a Security Center javasolja, hogy kapcsolja be az adatgyűjtést az adott előfizetéshez tartozó biztonsági házirend.

Adatgyűjtés engedélyezve van, a Security Center kiosztja a Microsoft Monitoring Agent összes meglévő támogatott az Azure virtuális gépek és a létrehozott újakat. A Microsoft Monitoring Agent ellenőrzi a különböző biztonsági konfigurációkat. Az operációs rendszer ezenkívül Eseménynapló eseményeinek riasztást. A gyűjtött adatok például a következők: az operációs rendszer típusa és verziója, az operációs rendszer naplói (Windows-eseménynaplók), a futó folyamatok, a gép neve, az IP-címek, a bejelentkezett felhasználó és a bérlő azonosítója. A Microsoft Monitoring Agent beolvassa az eseménynapló-bejegyzések és konfigurációkat, és másolja az adatokat a munkaterületre elemzés céljából. A Microsoft Monitoring Agent összeomlási memóriaképek is másolja a munkaterületre.

Ha a Security Center az ingyenes csomagot használ, letilthatja a virtuális gépek adatok gyűjtése által a biztonsági szabályzatban adatok gyűjtésének kikapcsolása. Adatgyűjtés letiltása korlátozza a biztonsági vizsgálatok során a virtuális géphez. További tudnivalókért lásd: [adatok gyűjtésének letiltása](#disabling-data-collection). Virtuálisgép-lemez pillanatfelvételek és összetevő gyűjtemény engedélyezve van, akkor is, ha az adatok gyűjtése le van tiltva. Adatgyűjtés a Standard szint a Security Center előfizetések szükség.

> [!NOTE]
> További tudnivalók a Security Center szabad és Standard [tarifacsomagok](security-center-pricing.md).
>
>

## <a name="implement-the-recommendation"></a>A javaslat megvalósítása

> [!NOTE]
> Ez a dokumentum egy üzembe helyezést szemléltető példa segítségével mutatja be a szolgáltatást. Ez a dokumentum nem tartalmaz lépésenkénti útmutatót.
>
>

1. Az a **javaslatok** panelen válassza **az adatgyűjtést az előfizetések**.  Ekkor megnyílik a **bekapcsolni az adatgyűjtést** panelen.
   ![Javaslatok panel][2]
2. Az a **bekapcsolni az adatgyűjtést** panelen jelölje ki az előfizetését. A **biztonsági házirend** adott előfizetéshez tartozó panel nyílik meg.
3. Az a **biztonsági házirend** panelen válassza **a** alatt **adatgyűjtés** az automatikus naplógyűjtéshez. Az előfizetés bekapcsolásával adatok gyűjtemény kiosztja a monitorozási bővítményt összes jelenlegi és új támogatott virtuális gépek.
4. Kattintson a **Mentés** gombra.
5. Kattintson az **OK** gombra.

## <a name="disabling-data-collection"></a>Adatgyűjtés letiltása
Ha a Security Center az ingyenes csomagot használ, letilthatja a adatgyűjtést bármikor a virtuális gépek által a biztonsági szabályzatban adatok gyűjtésének kikapcsolása. Adatgyűjtés a Standard szint a Security Center előfizetések szükség.

1. Lépjen vissza a **Security Center** panelhez, és válassza a **házirend** csempére. Ekkor megnyílik a **biztonsági szabályzat – szabályzat definiálása előfizetésenként** panelen.
   ![Válassza ki a házirend csempe][5]
2. Az a **biztonsági szabályzat – szabályzat definiálása előfizetésenként** panelen válassza ki az előfizetést, amelyet meg kíván kapcsolni az adatgyűjtést.
3. A **biztonsági házirend** adott előfizetéshez tartozó panel nyílik meg.  Válassza ki **ki** adatgyűjtés alapján.
4. Válassza ki **mentése** a felső szalagon.

## <a name="next-steps"></a>Következő lépések
Ez a cikk bemutatta megvalósításához a Security Center ajánlás "Adatgyűjtés engedélyezése." A Security Centerrel kapcsolatos további információkért olvassa el a következőket:

* [Biztonsági szabályzatok beállítása az Azure Security Centerben](security-center-policies.md) – Ez a cikk bemutatja, hogyan konfigurálhat biztonsági házirendeket Azure-előfizetései és -erőforráscsoportjai számára.
* [Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) – megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.
* [Biztonsági állapotmonitorozás az Azure Security Centerben](security-center-monitoring.md) – Útmutató az Azure-erőforrások állapotának monitorozásához.
* [Biztonsági riasztások kezelése és reagálás a riasztásokra az Azure Security Centerben](security-center-managing-and-responding-alerts.md) – A biztonsági riasztások kezelése és az azokra való reagálás.
* [Partneri megoldások monitorozása az Azure Security Centerrel](security-center-partner-solutions.md) – Útmutató a partneri megoldások biztonsági állapotának monitorozásához.
- [Az Azure Security Center adatainak biztonsági](security-center-data-security.md) -megtudhatja, hogyan adatok felügyelt és a Security Center védelmét.
* [Azure Security Center – gyakran ismételt kérdések](security-center-faq.md) – Gyakran ismételt kérdések a szolgáltatás használatával kapcsolatban.
* [Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – Tájékozódás az Azure biztonságával kapcsolatos legfrissebb hírekről és információkról.

<!--Image references-->
[2]: ./media/security-center-enable-data-collection/recommendations.png
[3]: ./media/security-center-enable-data-collection/data-collection.png
[4]: ./media/security-center-enable-data-collection/storage-account.png
[5]: ./media/security-center-enable-data-collection/policy.png
[6]: ./media/security-center-enable-data-collection/disable-data-collection.png
