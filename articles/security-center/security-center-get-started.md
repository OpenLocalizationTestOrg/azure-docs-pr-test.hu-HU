---
title: "a Security Center gyors aaaAzure útmutató |} Microsoft Docs"
description: "Ez a cikk segít használatának gyors megkezdése az Azure Security Center által szabályzatkezelési hello biztonsági figyelést és szabályzatkezelési összetevőit, valamint felsorolja toonext lépéseket."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: terrylan
ms.openlocfilehash: 23b2444ba1ba30d0a1bd1a1afbc4fd0abfd0827c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-quick-start-guide"></a>Bevezetés az Azure Security Center használatába
Ez a cikk segít gyorsan megismerkedhet az Azure Security Center hello biztonsági figyelést és szabályzatkezelési összetevőit a Security Center létrehozásában.

> [!NOTE]
> Korai. június 2017 verziótól kezdve a Security Center használnak hello Microsoft Monitoring Agent toocollect, illetve adatainak tárolásához. Lásd: [Azure Security Center Platform áttelepítési](security-center-platform-migration.md) további toolearn. a cikkben szereplő információkat hello Security Center funkció átmenet toohello Microsoft Monitoring Agent után jelöli.
>
>

## <a name="prerequisites"></a>Előfeltételek
a Security Center használatába tooget, rendelkeznie kell egy előfizetési tooMicrosoft Azure. Ha nem rendelkezik előfizetéssel, regisztrálhat egy [ingyenes fiókkal](https://azure.microsoft.com/pricing/free-trial/).

Ingyenes szint hello a Security Center automatikusan engedélyezve van az előfizetés, és betekintést hello az Azure-erőforrások biztonsági állapotát. Biztosítja az alapszintű biztonságiszabályzat-kezelést, a biztonsági javaslatokat, valamint az Azure-partnerek biztonsági szolgáltatásainak és termékeinek az integrálását.

A Security Center elérje hello [Azure-portálon](https://azure.microsoft.com/features/azure-portal/). toolearn hello Azure-portálon kapcsolatos további információkért lásd: hello [portál dokumentációjában](https://azure.microsoft.com/documentation/services/azure-portal/).

## <a name="permissions"></a>Engedélyek
A biztonsági központban, csak akkor jelenik meg kapcsolatos adatokat tooan Azure-erőforrás hello előfizetés vagy az erőforrás csoport, amely egy erőforrás tartozik tulajdonos, közreműködő vagy olvasó hello szerepkör hozzárendelése esetén. Lásd: [engedélyek az Azure Security Centerben](security-center-permissions.md) toolearn további szerepkörök és a Security Center engedélyezett műveletek.

## <a name="data-collection"></a>Adatgyűjtés
A Security Center adatokat gyűjti össze a virtuális gépek (VM) tooassess biztonsági állapotukra, adja meg a biztonsági javaslatok és riasztást küldjön, toothreats. A Security Center első használatakor az adatgyűjtés engedélyezve van az előfizetéséhez tartozó összes virtuális gépen. A Security Center rendelkezések hello Microsoft Monitoring Agent összes meglévő támogatott Azure virtuális gépek és bármely új gazdarendszerhez jönnek létre. Lásd: [az adatgyűjtést](security-center-enable-data-collection.md) adatgyűjtés működésével kapcsolatos további toolearn.

Adatgyűjtés ajánlott. Ha hello ingyenes szint a Security Center használ, letilthatja a virtuális gépek adatok gyűjtése hello biztonsági házirend adatgyűjtés kikapcsolásával. Adatgyűjtés a Security Center szabványos szintjének hello előfizetések szükség. Lásd: [Security Center árképzési](security-center-pricing.md) toolearn arról hello szabad és a Standard tarifacsomag szükséges.

hello lépések ismertetik, hogyan tooaccess és -felhasználási hello Security Center összetevői. E lépések azt mutatja be tooturn adatok gyűjtését, ha úgy dönt, hogy a kimenő tooopt ki.

> [!NOTE]
> Ez a cikk a központi telepítésre példát a hello szolgáltatás be. A cikk nem tartalmaz lépésenkénti útmutatót.
>
>

## <a name="access-security-center"></a>A Security Center elérése
Hello portálon kövesse ezeket a lépéseket tooaccess Security Center:

1. A hello **Microsoft Azure** menü **Security Center**.

   ![Az Azure menü][1]
2. Ha igénybe veszi a Security Center hello az első alkalommal, hello **üdvözlő** panel nyílik meg. Válassza ki **indítsa el a Security Center** tooopen hello **Security Center** panel és tooenable adatgyűjtés.
   ![Üdvözlőképernyő][10]
3. Indítsa el a Security Center hello üdvözlő paneljén, vagy hello Microsoft Azure menüből válassza ki a Security Center után hello **Security Center** panel nyílik meg. Az egyszerű a hozzáférés toohello **Security Center** hello jövőbeli, jelölje be hello paneljén **PIN-kód panel toodashboard** (jobb felső) lehetőséget.
   ![PIN-kód panel toodashboard beállítás][2]

## <a name="use-security-center"></a>A Security Center használata
Biztonsági szabályzatokat állíthat be Azure-előfizetéseihez és -erőforráscsoportjaihoz. Állítsunk be egy biztonsági szabályzatot az előfizetéséhez:

1. A hello **Security Center** panelen, jelölje be hello **házirend** csempére.
2. A hello **biztonsági házirend - előfizetés szabályzat definiálása** panelen válasszon ki egy előfizetést.
3. A hello **biztonsági házirend** panelen **adatgyűjtés** engedélyezett tooautomatically gyűjtése naplók van. figyelési bővítmény hello hello előfizetés összes jelenlegi és új virtuális lett kiépítve. (A Security Center ingyenes szint hello, dönthet úgy is adatgyűjtés kívül úgy, hogy **adatgyűjtés** túl**ki**. Beállítás **adatgyűjtés** túl**ki** megakadályozza, hogy a Security Center biztonsági riasztások és javaslatok felkínálva.)
4. A hello **biztonsági házirend** panelen válassza **megakadályozási szabályzat**. Ekkor megnyílik a hello **megakadályozási szabályzat** panelen.
5. A hello **megakadályozási szabályzat** panelen kapcsolja be, amelyet a biztonsági szabályzat részeként szeretne toosee hello javaslatokat. Példák:

   * Beállítás **rendszerfrissítések** túl**a** vizsgálatok az összes támogatott virtuális gépek hiányzik az operációs rendszer frissítése érdekében.
   * Beállítás **az operációs rendszer biztonsági rések** túl**a** vizsgálatok összes támogatott virtuális gépek tooidentify olyan operációsrendszer-beállítások, amelyek miatt előfordulhat, hogy a virtuális gép hello sebezhetőbbé tooattack.

### <a name="view-recommendations"></a>Javaslatok megtekintése
1. Térjen vissza a toohello **Security Center** panel megnyitásához, és jelölje be hello **javaslatok** csempe. A Security Center rendszeresen elemzi az Azure-erőforrások biztonsági állapotának hello. Amikor a Security Center a potenciális biztonsági hiányosságokat azonosít, azt mutatja be javaslatok hello **javaslatok** panelen.
   ![Javaslatok az Azure Security Centerben][5]
2. Válassza ki a hello ajánlást **javaslatok** panel tooview további információt és/vagy tootake művelet tooresolve hello ki.

### <a name="view-hello-security-state-of-your-resources"></a>Hello az erőforrások biztonsági állapotának megtekintése
1. Térjen vissza a toohello **Security Center** panelen. Hello **megelőzési** hello irányítópult szakasza tartalmazza a hello biztonsági állapotát utaló virtuális gépekhez, hálózati, adatokhoz és alkalmazásokhoz.
2. Válassza ki **számítási** tooview további információt. Hello **számítási** panel megnyitása megjelenítő három lappal:

  - **Áttekintés** -tartalmaz figyelési és a virtuális gépekre vonatkozó javaslatok.
  - **Virtuális gépek** -felsorolja az összes virtuális gépek és a jelenlegi biztonsági állapotok.
  - **A felhőalapú szolgáltatások** -Security Center által figyelt webes és feldolgozói szerepkörök listája.

    ![hello erőforrás biztonsági állapota csempe az Azure Security Centerben][6]

3. A hello **áttekintése** lapra, válassza ki az ajánlás **virtuális gépek javaslatok** tooview további információt és/vagy hajtsa végre a megfelelő művelet tooconfigure szükséges vezérlők.
4. A hello **virtuális gépek** lapra, válassza ki a virtuális gép tooview további részleteket.

### <a name="view-security-alerts"></a>Biztonsági riasztások megtekintése
1. Térjen vissza a toohello **Security Center** panel megnyitásához, és jelölje be hello **biztonsági riasztások** csempére. Hello **biztonsági riasztások** panel nyílik meg, és a riasztások listáját jeleníti meg. hello elemzése a Security Center biztonsági naplókra és hálózati tevékenységet ezeket a riasztásokat állít elő. Továbbá az integrált partnermegoldásoktól érkező riasztásokat is tartalmazza.
   ![Biztonsági riasztások az Azure Security Centerben][7]

   > [!NOTE]
   > Biztonsági riasztások csak érhetők el, ha a Standard szint hello Security Center engedélyezve van. 60 nap ingyenes hello szabványos réteg érhető el. Lásd: [további lépések](#next-steps) olvashat, hogyan tooget hello Standard réteg.
   >
   >
2. Válassza ki a riasztási tooview további információ. Ebben a példában válassza a **Módosított bináris rendszerfájl észlelve**. Ekkor megnyílik a paneleken hello riasztással kapcsolatos további részleteit.
   ![Biztonsági riasztások részletei az Azure Security Centerben][8]

### <a name="view-hello-health-of-your-partner-solutions"></a>Partneri megoldások hello állapotának megtekintése
1. Térjen vissza a toohello **Security Center** panelen. Hello **partneri megoldások** csempe segítségével figyelheti, egy pillanat alatt hello és az Azure-előfizetésében integrált partnermegoldások biztonsági állapotát.
2. Jelölje be hello **partneri megoldások** csempére. Ekkor megnyílik egy panel, és megjeleníti a partneri megoldások listáját tooSecurity Center csatlakoztatva.
   ![Partnermegoldások][9]
3. Válasszon ki egy partneri megoldást. Ebben a példában hello kiválasztása most **QualysVa1** megoldás.  Megnyílik egy panel, és bemutatja, hello partneri megoldás és hello megoldás hello állapotának kapcsolódó erőforrások. Válassza ki **megoldáskonzol** tooopen hello partner felület ebben a megoldásban.

## <a name="next-steps"></a>Következő lépések
Ez a cikk bevezetett toohello biztonsági figyelést és szabályzatkezelési összetevőit a Security Center. Most, hogy ismeri a Security Center, próbálja meg a lépéseket követve hello:

* Állítson be egy biztonsági szabályzatot az Azure-előfizetéséhez. több, lásd: toolearn [biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md).
* Hello javaslatokat használja a Security Center toohelp az Azure-erőforrások védelme. több, lásd: toolearn [biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md).
* Tekintse át és ellenőrizze a jelenlegi biztonsági riasztásokat. több, lásd: toolearn [az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md).
- [Az Azure Security Center adatainak biztonsági](security-center-data-security.md) -megtudhatja, hogyan adatok felügyelt és a Security Center védelmét.
* További tudnivalók hello [advanced threat szolgáltatásai](security-center-detection-capabilities.md) hello rendelkeznek, amelyek [Standard csomagra](security-center-pricing.md) a Security Center. hello Standard csomagra tartományregisztráció ingyenesen hello az első hatvan.
* Ha kérdése merül fel a Security Center használatáról, tekintse meg a hello [Azure Security Center: GYIK](security-center-faq.md).

<!--Image references-->
[1]: ./media/security-center-get-started/azure-menu.png
[2]: ./media/security-center-get-started/security-center-pin.png
[3]: ./media/security-center-get-started/security-policy.png
[4]: ./media/security-center-get-started/prevention-policy.png
[5]: ./media/security-center-get-started/recommendations.png
[6]: ./media/security-center-get-started/resources-health.png
[7]: ./media/security-center-get-started/security-alert.png
[8]: ./media/security-center-get-started/security-alert-detail.png
[9]: ./media/security-center-get-started/partner-solutions.png
[10]: ./media/security-center-get-started/welcome.png
