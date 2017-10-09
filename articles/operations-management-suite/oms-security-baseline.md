---
title: "Felügyeleti csomag biztonsági és naplózási megoldás alapvető aaaOperations |} Microsoft Docs"
description: "Ez a dokumentum azt ismerteti, hogyan toouse OMS biztonsági és hitelesítési megoldás tooperform egy alapkonfiguráció értékelése az összes figyelt számítógépek megfelelőségi és biztonsági célra."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 17837c8b-3e79-47c0-9b83-a51c6ca44ca6
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: yurid
ms.openlocfilehash: ea52408cb9d2598728fe3826a946067e1c99318f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="baseline-assessment-in-operations-management-suite-security-and-audit-solution"></a>Az alapkonfiguráció értékelése az Operations Management Suite biztonsági és auditálási megoldásában
Ez a dokumentum segít toouse [Operations Management Suite (OMS) biztonsági és naplózási megoldás](operations-management-suite-overview.md) alapkonfiguráció értékelése képességek tooaccess hello a figyelt erőforrások biztonsági állapotát.

## <a name="what-is-baseline-assessment"></a>Mi az az alapkonfiguráció-értékelés?
A Microsoft a világszerte elhelyezkedő iparági és kormányzati szervezetekkel közösen olyan Windows-konfigurációt határoz meg, amely nagy biztonságú kiszolgálók üzembe helyezését teszi lehetővé. Ez a konfiguráció beállításkulcsokból, auditálási szabályzatok beállításaiból és biztonsági szabályzatok beállításaiból áll, valamint a Microsoft ajánlott értékeit tartalmazza ezekhez a beállításokhoz. Ez a szabályzathalmaz biztonsági alapkonfigurációként ismert. Az OMS biztonság és auditálás alapkonfiguráció-értékelési képességével megfelelőség szempontjából zökkenőmentesen átvizsgálható az összes számítógépe. 

Három szabálytípus áll rendelkezésre:

* **Beállításjegyzék-szabályok**: a beállításkulcsok megfelelő beállításának ellenőrzéséhez.
* **Auditálási házirend-szabályok**: az auditálási házirendre vonatkozó szabályok.
* **Biztonsági szabályzat előírásainak**: szabályok hello gépen vonatkozó hello felhasználó engedélyeit.

> [!NOTE]
> Olvasási [használata OMS biztonsági tooassess hello biztonsági Alapkonfigurációjához](https://blogs.technet.microsoft.com/msoms/2016/08/12/use-oms-security-to-assess-the-security-configuration-baseline/) Ez a szolgáltatás rövid áttekintést.
> 
> 

## <a name="security-baseline-assessment"></a>A biztonsági alapkonfiguráció értékelése
Áttekintheti az aktuális biztonsági alapkonfiguráció assessment az OMS biztonsági és hello irányítópulttal naplózása által figyelt összes számítógépen.  A következő lépéseket tooaccess hello biztonsági alapkonfiguráció értékelése irányítópult hello hajtható végre:

1. A hello **a Microsoft Operations Management Suite** fő irányítópult kattintson **biztonsági és naplózási** csempére.
2. A hello **biztonsági és naplózási** irányítópultján kattintson **alapkonfiguráció értékelése** alatt **biztonsági tartományok**. Hello **biztonsági alapkonfiguráció értékelése** irányítópult jelenik meg, ahogy az a következő kép hello:
   
    ![Az OMS biztonsági és auditálási alapkonfiguráció értékelése](./media/oms-security-baseline/oms-security-baseline-fig1.png)

Ez az irányítópult három fő területet oszlik:

* **Számítógépek képest toobaseline**: Ez a szakasz hello számítógépek száma, amelyek elért és számítógépei hello assessment továbbított hello összegzi. Emellett a segítségével hello első 10 számítógépéről és hello százalékos eredménye a hello ellenőrzéséhez.
* **Szükséges szabályok állapot**: Ez a szakasz hello leképezési toobring tájékoztatási sikertelen hello szabályok rendelkezik súlyosság szerint, és nem sikerült a szabályok típus szerint. Gyorsan azonosíthatja a legtöbb hello sikertelen szabályok toohello első diagram megkeresésével kritikusak, vagy nem. Hello felső 10 nem teljesített szabályok és a súlyosság listáját is biztosít. hello második grafikon azt ábrázolja, hello típusú hello értékelés során sikertelen szabályt. 
* **Hiányzik az alapkonfiguráció értékelése számítógépek**: Ez a szakasz felsorolása hello számítógépek toooperating rendszer kompatibilitási vagy hibák miatt nem használt. 

### <a name="accessing-computers-compared-toobaseline"></a>Számítógépek elérése toobaseline képest
Ideális esetben a számítógépek minden meg kell felelniük hello biztonsági alapkonfiguráció értékelése. Azonban várható, hogy bizonyos körülmények között ez nem valósul meg. Tekintse át a sikertelen toopass biztonsági assessment tesztjein hello számítógépek fontos tooinclude hello biztonsági felügyeleti folyamat részeként. A gyors toovisualize, amely hello lehetőség kiválasztásával **elérhető számítógépek** hello található **számítógépek képest toobaseline** szakasz. Hello napló keresése eredményt megjelenítő hello számítógépek listáját a következő képernyő hello látható módon kell megjelennie:

![Az elért számítógépeket tartalmazó eredmények](./media/oms-security-baseline/oms-security-baseline-fig2.png)

hello keresési eredmény jelenik meg egy tábla formátumban, ahol hello első oszlop hello számítógépnév pedig hello második szín hello száma szabályokat, amelyek nem sikerült. tooretrieve hello információhoz hello típusú szabály, amely nem sikerült, kattintson a nem teljesített szabályok mellett hello számítógépnév hello száma. Egy kép a következő hello látható eredmény hasonló toohello kell megjelennie:

![Az elért számítógépeket tartalmazó eredmények részletei](./media/oms-security-baseline/oms-security-baseline-fig3.png)

A keresési eredmény rendelkezik hello összesen elért szabályok, hello sikertelen kritikus szabályok száma, hello figyelmeztetés szabályokat, és nem sikerült információt szabályok hello.

### <a name="accessing-required-rules-status"></a>A kötelező szabályok állapotának elérése
Miután beszerezte a hello százalékos számítógépek száma, amelyek hello assessment átadott hello információhoz, érdemes lehet tooobtain mely szabályokkal kapcsolatos további információk függően toohello kritikusság nem működnek. Ez a képi megjelenítés nyújt segítséget tooprioritize, mely számítógépek kell venni az első tooensure fogják hello következő assessment megfelelőnek. Hello legkritikusabb feladata hello található hello gráf rámutat **szabályok sikertelen súlyosság szerint** csempére a **szükséges szabályok állapot** , és kattintson rá. A következő képernyő eredmény hasonló toohello kell megjelennie:

![A Nem teljesített szabályok súlyosság szerint részletei](./media/oms-security-baseline/oms-security-baseline-fig4.png) 

A napló eredményt látja hello típusú eredeti szabályt, a sikertelen, a szabály és a hello Common Configuration Enumeration (CCE) azonosítója a biztonsági szabály hello leírása. Ezek az attribútumok legyen elég tooperform a javítási művelet toofix hello célszámítógépen a probléma.

> [!NOTE]
> További információ a CCE hozzáférési hello [National Vulnerability adatbázis](https://nvd.nist.gov/cce/index.cfm).
> 
> 

### <a name="accessing-computers-missing-baseline-assessment"></a>Az alapkonfiguráció értékeléséből kimaradt számítógépek elérése
OMS hello tartomány tagja, és a tartományvezérlő alapterv profil Windows Server 2008 R2 rendszerben legfeljebb támogat tooWindows Server 2012 R2. A Windows Server 2016 alapkonfigurációja még nem végleges, és közzététele után azonnal megtörténik a hozzáadása. Minden más operációs rendszerek OMS biztonsági és naplózási alapterv értékeléssel beolvasott jelenik meg az hello **hiányzik az alapkonfiguráció értékelése számítógépek** szakasz.

## <a name="see-also"></a>Lásd még:
Ebben a dokumentumban az OMS biztonság és audit alapkonfigurációs értékeléséről olvashatott. További információ az OMS biztonsági toolearn tekintse meg a következő cikkek hello:

* [Az Operations Management Suite (OMS) áttekintése](operations-management-suite-overview.md)
* [Figyelés és a válaszoló tooSecurity riasztásait az Operations Management Suite biztonsági és naplózási megoldás](oms-security-responding-alerts.md)
* [Az erőforrások figyelése az Operations Management Suite biztonsági és auditálási megoldásban](oms-security-monitoring-resources.md)

