---
title: "aaaAzure Active Directory hibrid identitáskezelési elrendezésével kapcsolatos szempontok - adatvédelmi követelményeinek meghatározása |} Microsoft Docs"
description: "Azonosítsa a hibrid identitáskezelési megoldás tervezése hello vállalatának adatvédelmi követelményeinek, és melyik lehetőségek állnak rendelkezésre toobest ezen követelmények teljesítése."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: 40dc4baa-fe82-4ab6-a3e4-f36fa9dcd0df
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 189abf9affbc2894c322f362d84222d4e33d472e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="plan-for-enhancing-data-security-through-strong-identity-solution"></a>Adatok biztonsági erős identitáskezelési megoldással továbbfejlesztésének tervezése
hello első lépés tooprotect hello adatok azonosíthatja, ki férhet hozzá az adatok, és ez a folyamat részeként egy identitás-megoldás, amely is integrálható, a rendszer tooprovide hitelesítési és engedélyezési képességek toohave szüksége. Hitelesítési és engedélyezési rendszer gyakran összetéveszthető egymáshoz, és azok a szerepkörök böngésző. A valóságban ezek eltérnek a Igen, az alábbi hello ábrán látható módon:

![](./media/hybrid-id-design-considerations/mobile-devicemgt-lifecycle.png)

**Mobileszköz-kezelés életciklusának szakaszait**

A hibrid identitáskezelési megoldás tervezése során ismernie kell hello adatvédelmi követelményeinek az üzleti és melyik lehetőségek állnak rendelkezésre toobest teljesítik ezeket a követelményeket.

> [!NOTE]
> Miután befejezte az adatok biztonsági tervezése, tekintse át a [határozza meg a multi-factor authentication követelményeinek](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md) tooensure, hogy a multi-factor authentication követelményeinek vonatkozó beállításokat nem érintette hello döntések meg ebben a szakaszban arról.
> 
> 

## <a name="determine-data-protection-requirements"></a>Adatvédelmi követelményeinek meghatározása
A mobilitási hello korát, a legtöbb vállalat rendelkezik egy közös cél: a felhasználók hatékonyabb a mobileszközökön a helyszínen vagy távolról, a rendelés tooincrease termelékenység bárhol toobe engedélyezése. Ennek oka lehet egy közös cél, amíg vállalatok számára, hogy az ilyen követelménynek is aggodalmát, szolgáló kell mérsékelni a rendelés tookeep vállalati adatok biztonságos, és a felhasználó adatvédelmének hello mennyisége. Minden vállalat igényei lehetnek az különböző ebben a tekintetben; különböző megfelelőségi szabályokat, amelyek a konfigurációtól függően toowhich iparági hello vállalati jár el lesz toodifferent tervezési döntésekhez vezethet. 

Van azonban néhány biztonsági szempontok felfedezte és érvényesítve, függetlenül hello iparági hello a következő szakasz ismerteti.

## <a name="data-protection-paths"></a>Data protection elérési utak
![](./media/hybrid-id-design-considerations/data-protection-paths.png)

**Data protection elérési utak**

A fenti ábrán hello a hello identitás összetevő lesz hello először egy toobe ellenőrzése előtt az adatokhoz. Azonban ezek az adatok különböző állapota lehet lett elért hello idő alatt. Ez az ábra egyes szám egy elérési utat, ahol lehet az adatokat bizonyos ponton található időben jelzi. Ezeket a számokat ismertetése az alábbiakban olvasható:

1. Az adatvédelem hello eszköz szinten.
2. Az adatvédelem az átvitel során.
3. A többi helyszíni adatvédelem.
4. Az adatvédelem aktívan hello felhő.

Bár a hello technikai vezérlők, amely lehetővé teszi az egyes ezeket a fázisokat informatikai tooprotect hello adatokat mozgatná nem közvetlenül hello hibrid identitáskezelési megoldás által felajánlott, továbbra is szükséges, hogy képes a helyszínen, ami-e hello hibrid identitáskezelési megoldás és a felhő-identity management erőforrások tooidentify hello felhasználói grant hozzáférés toohello adatok előtt. Ha a hibrid identitáskezelési megoldás tervezésének részeként győződjön meg arról, hogy a következő hello a kérdések és válaszok tooyour szervezet igényeinek megfelelően:

## <a name="data-protection-at-rest"></a>Az adatvédelem aktívan
Függetlenül attól, hol hello adatokat aktívan (eszköz, felhőalapú vagy helyszíni) fontos tooperform assessment toounderstand hello szervezetként kell ebben a tekintetben. Ez a terület győződjön meg arról, a rendszer kéri, hogy a következő kérdések hello:

* A vállalatának meg kell inaktív adatok tooprotect?
  * Ha igen, akkor hello hibrid identitáskezelési megoldás képes toointegrate a jelenlegi helyszíni infrastruktúrával?
  * Ha igen, van hello hibrid identitáskezelési megoldás képes toointegrate a hello felhőben lévő munkaterhelések?
* Az hello cloud identity management képes tooprotect hello felhasználói hitelesítő adatokat és egyéb hello felhőben tárolt adatok?

## <a name="data-protection-in-transit"></a>Adatok védelmére átvitel közben
Hello eszköz és hello datacenter vagy hello eszköz és hello felhő között átvitt adatokat kell védeni. Azonban az átvitel alatt nem feltétlenül jelenti a kommunikációs folyamat egy összetevő kívül a felhőalapú szolgáltatás; átvitel során, belső is, mint például két virtuális hálózatot. Ez a terület győződjön meg arról, a rendszer kéri, hogy a következő kérdések hello:

* A vállalatának meg kell tooprotect adatokat átvitel közben?
  * Ha igen, például az SSL/TLS biztonságos vezérlők hello hibrid identitáskezelési megoldás képes toointegrate van?
* Hello felhő Identitáskezelés megtartja hello forgalom tooand hello directory-tárolóhoz (belül és adatközpontok között) aláírt belül?

## <a name="compliance"></a>Megfelelőség
Szabályzat, a törvényi és az előírásoknak való megfelelés követelmények konfigurációtól függően toohello iparági, amely a vállalat tartozik. Magas szabályozott iparágakban vállalatok Identitáskezelés maillel kapcsolódó toocompliance problémákat kell oldania. Például a Sarbanes-Oxley (SOX), hello egészségügyi biztosítás hordozhatóságáról és elszámolási kötelezettségéről szóló törvény (HIPAA) szabályzat hello Gramm-Leach-Bliley Act (GLBA) és hello fizetési kártya adatvédelmi szabvány (PCI DSS) nagyon szigorú identitások és hozzáférések kapcsolatban. hello hibrid identitáskezelési megoldás a vállalat által alkalmazott lesz legalább egy szabályzatnak hello követelményeinek teljesítéséhez hello alapképességek kell rendelkeznie. Ez a terület győződjön meg arról, a rendszer kéri, hogy a következő kérdések hello:

* Megfelel hello hibrid identitáskezelési megoldás hello a saját üzleti szabályozási követelmények?
* Biztosítja hello hibrid identitáskezelési megoldás rendelkezik beépített képességei, amely lehetővé teszi a vállalat toobe megfelelő szabályozási követelmények? 

> [!NOTE]
> Győződjön meg arról, hogy tootake megjegyzések minden válaszról és hello válasz hello logikája ismertetése. [Data Protection stratégia meghatározása](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) hello lehetőségeit és az egyes lehetőségek előnyeit és hátrányait ismerteti.  Ezen kérdések mely leginkább megfelelő lehetőséget az üzleti megválaszolása szükséges.
> 
> 

## <a name="next-steps"></a>Következő lépések
 [A Tartalomkezelés követelmények meghatározása](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)

## <a name="see-also"></a>Lásd még:
[Kialakítási szempontok áttekintése](active-directory-hybrid-identity-design-considerations-overview.md)

