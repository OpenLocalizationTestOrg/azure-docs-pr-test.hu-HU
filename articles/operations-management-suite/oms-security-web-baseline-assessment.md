---
title: "az Operations Management Suite biztonsági és naplózási megoldás alapvető alapkonfiguráció értékelése aaaWeb |} Microsoft Docs"
description: "Ez a dokumentum azt ismerteti, hogyan toouse webes alapkonfiguráció értékelése az OMS biztonsági és hitelesítési megoldás tooperform egy alapkonfiguráció megfelelőségi és biztonsági célra minden figyelt webkiszolgálók értékelése."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 17837c8b-3e79-47c0-9b83-a51c6ca44ca6
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: yurid
ms.openlocfilehash: dafa9d3d93fae31748306b60ee40b285dd59c802
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="web-baseline-assessment-in-operations-management-suite-security-and-audit-solution"></a>A webes alapkonfiguráció értékelése az Operations Management Suite biztonsági és auditálási megoldásában
Ez a dokumentum segít az OMS biztonsági és naplózási webes alapkonfiguráció értékelése képességek tooaccess hello a figyelt erőforrások biztonsági állapotát.

## <a name="what-is-web-baseline-assessment"></a>Mi az a webes alapkonfiguráció-értékelés?
Jelenleg az OMS biztonsági megoldása biztosít alapkonfiguráció-értékelést az operációs rendszerekhez. Hello az operációs rendszer beállításait a kiszolgálók 24 óránként keres, és sebezhető beállítások nézetét jeleníti meg. Ezzel kapcsolatban további információkat olvashat [az alapkonfiguráció az Operations Management Suite biztonsági és auditálási megoldásában történő értékelését](https://docs.microsoft.com/azure/operations-management-suite/oms-security-baseline) ismertető cikkben.

hello hello webes alapkonfiguráció értékelése célja toofind sebezhető webkiszolgálói beállítások. három elsődleges források hello a hello webes alapterv konfigurációk a következők: .NET, az ASP.NET és az IIS konfigurációját.  Ugyanúgy, mint az operációs rendszer alapkonfiguráció értékelése hello, OMS biztonsági érintetlen tooscan a webkiszolgálón minden 24hrs és azok biztonsági állapotának áttekintése.  Az Internet Information Service (IIS), konfigurációk olyan nagy mértékben testre szabható, amely lehetővé teszi, hogy a különböző helyek és alkalmazások szintek toobe felülbírálható. hello képolvasó hello beállítások hozzáadása toohello alapértelmezett gyökérszinten minden egyes alkalmazás-vagy helyvédelmi szinten ellenőrzi. Ez segít tooidentify sebezhető beállításait, és gyorsan javítása, valamint a javaslatok ezeket a beállításokat.

>[!NOTE] 
>Letöltheti a hello közös konfigurációs azonosítókat és az OMS biztonsági által használt alapkonfigurációs szabályok [lap](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335?redir=0).


## <a name="web-security-baseline-assessment"></a>A webes biztonsági alapkonfiguráció értékelése

Az előzetes verzió hello szolgáltatás hello OMS keresési lehetőség, és az OMS biztonsági hello és a naplózási irányítópult keresztül elérhető. Kövesse az alábbi tooperform sajátíthatja hello lekérdezés hello lépéseket:

1. A hello **a Microsoft Operations Management Suite** fő irányítópult kattintson **biztonsági és naplózási** csempére.
2. A hello **biztonsági és naplózási** az irányítópult hello webes alapterv perspektíva következő toohello OS alapterv perspektíva látható.
   
    ![A webes biztonsági alapkonfiguráció értékelése az OMS biztonsági és auditálási irányítópultján](./media/oms-security-web-baseline/oms-security-web-baseline-fig5.png)

3. hello bal oldali ablaktáblán hello számának képest webkiszolgálók toohello alapterv, szabályok, amelyek kiértékelése hello minden olyan kiszolgálón átadott hello hányada és hello volt értékelni kiszolgálók listája látható.
4. jobb oldali ablaktábla listázza a hello egyedi hello szabályok által sikertelenül *súlyossági*, és *RuleType*. Bármely hello jobb oldali szabályok kattint, megjelenik a szabály hello részleteit. Egy példa az alábbi kép hello. hello szabály, amely ki lesz értékelve megtalálható-e *szabály beállításai*. Hello *AzId* mező, amelyben az egyes szabályokhoz nyomon követése hello alapkonfigurációs szabályok a Microsoft által használt egyedi azonosítója. Ezenkívül toothat értesülhet hello *várt eredmény* (a Microsoft ajánlott érték), és egyéb részletek vonatkozó hello biztonsági következményei hello szabály.
    
    ![Lekérdezés](./media/oms-security-web-baseline/oms-security-web-baseline-fig6.png)

5. A saját lekérdezések tooreview hello eredményeket hozhat létre. 

hello első lekérdezés használható hello **webes alapterv felmérésének összegzése**. A hello **Begin Keresés itt** mezőbe írja be a lekérdezést: *típus = SecurityBaselineSummary BaselineType webes =*. hello az alábbiakban egy kimeneti példa látható:

![Lekérdezés eredménye](./media/oms-security-web-baseline/oms-security-web-baseline-fig7.png)

>[!NOTE] 
>Ebben a lekérdezésben minden rekord egy kiszolgáló értékelési összegzését jelöli.

Miután belépett hello **naplófájl-keresési**, beírhatja különböző lekérdezéseket tooobtain hello webes alapkonfiguráció értékelése további információt. Továbbá toohello előző lekérdezést, használhatja a következő azokat, ebben az előzetes verzióban hello:

**Webes alapkonfigurációsszabály-értékelés**: Minden rekord egy kiszolgáló webes alapkonfigurációsszabály-értékelését jelöli. Tartalmazza a sikertelen szabályt, minden adatát hello *SitePath* mely hello a szabály értékelése, hello *várt eredmény*, és hello *tényleges eredmény*.

Lekérdezés: *Type=SecurityBaseline BaselineType=Web AnalyzeResult=Failed*

![2. lekérdezés eredménye](./media/oms-security-web-baseline/oms-security-web-baseline-fig8.png)

**Egy adott kiszolgálóhoz tartozó összes találat megjelenítése**: Ez a lekérdezés bemutatja, hogyan toosee eredménye egy adott kiszolgáló: lekérdezés: *típus = SecurityBaseline BaselineType webes számítógép = = BaselineTestVM1*

![3. lekérdezés eredménye](./media/oms-security-web-baseline/oms-security-web-baseline-fig3.png)

A rekordok/lekérdezések toocreate használhatja a saját irányítópultok, jelentések és értesítések. Íme egy minta felhasználói felületének vezérlői tooyour irányítópult adhat hozzá. Azt is megtudhatja, hogyan toovisualize OMS Nézettervező használata esetén az adatok [Itt](https://blogs.technet.microsoft.com/msoms/2016/06/30/oms-view-designer-visualize-your-data-your-way/). az alábbi üdvözlő képernyőt például hogyan hello csempe fog megjelenni a testreszabás után.

![irányítópult](./media/oms-security-web-baseline/oms-security-web-baseline-fig4.png)

## <a name="see-also"></a>Lásd még:
Ebben a dokumentumban az OMS biztonsági és auditálási megoldásának webes alapkonfiguráció-értékeléséről olvashatott. További információ az OMS biztonsági toolearn tekintse meg a következő cikkek hello:

* [Az Operations Management Suite (OMS) áttekintése](operations-management-suite-overview.md)
* [Figyelés és a válaszoló tooSecurity riasztásait az Operations Management Suite biztonsági és naplózási megoldás](oms-security-responding-alerts.md)
* [Az erőforrások figyelése az Operations Management Suite biztonsági és auditálási megoldásban](oms-security-monitoring-resources.md)

