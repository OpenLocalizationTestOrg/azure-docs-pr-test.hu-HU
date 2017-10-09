---
title: "aaaEnable adatgyűjtést az Azure Security Centerben |} Microsoft Docs"
description: " Megtudhatja, hogyan tooenable adatgyűjtést az Azure Security Centerben. "
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
ms.openlocfilehash: 78bbf9a3d852095e2a1387c1606ff4bbb778a0dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-data-collection-in-azure-security-center"></a>Az adatgyűjtést az Azure Security Centerben

> [!NOTE]
> Korai. június 2017 verziótól kezdve a Security Center használnak hello Microsoft Monitoring Agent toocollect, illetve adatainak tárolásához. több, lásd: toolearn [Azure Security Center Platform áttelepítési](security-center-platform-migration.md). a cikkben szereplő információkat hello Security Center funkció átmenet toohello Microsoft Monitoring Agent után jelöli.
>
>

A Security Center adatokat gyűjti össze a virtuális gépek (VM) tooassess biztonsági állapotukra, adja meg a biztonsági javaslatok és riasztást küldjön, toothreats. A Security Center első megnyitásakor hello beállítás tooenable adatok gyűjtése az összes virtuális gép van az előfizetésben. Adatok gyűjtése nem engedélyezett, ha a Security Center javasolja, hogy kapcsolja be az adatgyűjtést az adott előfizetéshez tartozó hello biztonsági házirend.

Adatgyűjtés engedélyezve van, a Security Center rendelkezések hello Microsoft Monitoring Agent összes meglévő támogatott az Azure virtuális gépek és a létrehozott újakat. a Microsoft Monitoring Agent hello ellenőrzi a különböző biztonsági konfigurációkat. Emellett a hello operációs rendszer Eseménynapló eseményeinek riasztást. A gyűjtött adatok például a következők: az operációs rendszer típusa és verziója, az operációs rendszer naplói (Windows-eseménynaplók), a futó folyamatok, a gép neve, az IP-címek, a bejelentkezett felhasználó és a bérlő azonosítója. hello Microsoft Monitoring Agent olvassa be az eseménynapló-bejegyzések és konfigurációkat, és másolja a hello adatok tooyour munkaterület elemzés céljából. Microsoft Monitoring Agent hello átmásolja összeomlási kiírt fájlok tooyour munkaterületen.

Ha hello ingyenes szint a Security Center használ, letilthatja a virtuális gépek adatok gyűjtése kikapcsolásával adatgyűjtés hello biztonsági házirendben. Adatgyűjtés letiltása korlátozza a biztonsági vizsgálatok során a virtuális géphez. több, lásd: toolearn [adatok gyűjtésének letiltása](#disabling-data-collection). Virtuálisgép-lemez pillanatfelvételek és összetevő gyűjtemény engedélyezve van, akkor is, ha az adatok gyűjtése le van tiltva. Adatgyűjtés a Security Center szabványos szintjének hello előfizetések szükség.

> [!NOTE]
> További tudnivalók a Security Center szabad és Standard [tarifacsomagok](security-center-pricing.md).
>
>

## <a name="implement-hello-recommendation"></a>Hello javaslat megvalósítása

> [!NOTE]
> Ez a dokumentum hello szolgáltatás telepítését bemutató példát használatával vezet be. Ez a dokumentum nem tartalmaz lépésenkénti útmutatót.
>
>

1. A hello **javaslatok** panelen válassza **az adatgyűjtést az előfizetések**.  Ekkor megnyílik a hello **bekapcsolni az adatgyűjtést** panelen.
   ![Javaslatok panel][2]
2. A hello **bekapcsolni az adatgyűjtést** panelen jelölje ki az előfizetését. Hello **biztonsági házirend** adott előfizetéshez tartozó panel nyílik meg.
3. A hello **biztonsági házirend** panelen válassza **a** alatt **adatgyűjtés** tooautomatically naplógyűjtéshez. Hello előfizetés összes jelenlegi és az új adatok gyűjtemény rendelkezések hello bővítmény figyelését bekapcsolásával támogatja a virtuális gépek.
4. Kattintson a **Mentés** gombra.
5. Kattintson az **OK** gombra.

## <a name="disabling-data-collection"></a>Adatgyűjtés letiltása
Ha hello ingyenes szint a Security Center használ, letilthatja a adatgyűjtés a virtuális gépekről bármikor kikapcsolásával adatgyűjtés hello biztonsági házirendben. Adatgyűjtés a Security Center szabványos szintjének hello előfizetések szükség.

1. Térjen vissza a toohello **Security Center** panel megnyitásához, és jelölje be hello **házirend** csempére. Ekkor megnyílik a hello **biztonsági szabályzat – szabályzat definiálása előfizetésenként** panelen.
   ![Hello házirend csempe kiválasztása][5]
2. A hello **biztonsági szabályzat – szabályzat definiálása előfizetésenként** panelen, jelölje be hello előfizetés, hogy kívánja-e toodisable adatgyűjtés.
3. Hello **biztonsági házirend** adott előfizetéshez tartozó panel nyílik meg.  Válassza ki **ki** adatgyűjtés alapján.
4. Válassza ki **mentése** hello felső szalagon.

## <a name="next-steps"></a>Következő lépések
Ez a cikk bemutatta, hogyan tooimplement hello Security Center ajánlás "Enable adatgyűjtés." További információ a Security Center toolearn hello következő lásd:

* [Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) – megtudhatja, hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.
* [Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) – megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.
* [Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md)– megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.
* [Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md)– megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.
* [Partnermegoldások figyelése az Azure Security Center](security-center-partner-solutions.md) – megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.
- [Az Azure Security Center adatainak biztonsági](security-center-data-security.md) -megtudhatja, hogyan adatok felügyelt és a Security Center védelmét.
* [Azure Security Center: GYIK](security-center-faq.md)– gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.
* [Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/)– hello legújabb Azure biztonsági hírek és információ lekérése.

<!--Image references-->
[2]: ./media/security-center-enable-data-collection/recommendations.png
[3]: ./media/security-center-enable-data-collection/data-collection.png
[4]: ./media/security-center-enable-data-collection/storage-account.png
[5]: ./media/security-center-enable-data-collection/policy.png
[6]: ./media/security-center-enable-data-collection/disable-data-collection.png
