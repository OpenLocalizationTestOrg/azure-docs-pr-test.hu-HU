---
title: "Felügyeleti csomag biztonsági és naplózási megoldás adatbiztonság aaaOperations |} Microsoft Docs"
description: "Ez a dokumentum azt ismerteti, hogyan zajlik az adatok kezelése az Operations Management Suite biztonsági és auditálási megoldásban."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 9cdf7deb-2a30-4672-b89f-71179ee8326a
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: yurid
ms.openlocfilehash: 9c4181b3b491e4f7f0c57d7252eca78a819722d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="operations-management-suite-security-and-audit-solution-data-security"></a>Adatbiztonság az Operations Management Suite biztonsági és auditálási megoldásban
toohelp felhasználók megakadályozása, észlelésében és kezelésében toothreats, [Operations Management Suite (OMS) biztonsági és naplózási megoldás](operations-management-suite-overview.md) gyűjt, és feldolgozza az erőforrások adatait, amely tartalmazza:

* Biztonsági eseménynapló
* A Windows esemény-nyomkövetés (ETW) eseményei
* AppLocker naplózási események
* A Windows tűzfal naplója
* Advanced Threat Analytics-események
* A biztonsági alapkonfiguráció értékelése
* Kártevőirtó-felmérés eredménye
* Frissítések/javítások felmérésének eredménye
* Rendszerbejegyzések adatfolyamok kifejezetten engedélyezett hello ügynökön

Erős kötelezettségvállalások tooprotect hello adatvédelmi és biztonsági ezeket az adatokat biztosítjuk. A Microsoft megfelelő toostrict megfelelőségi és biztonsági irányelvek – toooperating egy szolgáltatás-ból.
Ez a cikk bemutatja, hogyan kezeli az OMS biztonsági és auditálási megoldás az adatokat, illetve hogyan gondoskodik a védelmükről.

## <a name="data-sources"></a>Adatforrások
OMS biztonsági és naplózási megoldás elemzése a hello OMS-ügynököt futtató virtuális gépek és fizikai számítógépek adatokból. Az OMS biztonsági és auditálási megoldás konfigurációs információkat gyűjthet a biztonsági eseményekről, például a Windows-eseményekről, az auditnaplókról, az IIS-naplókról és a syslog-üzenetekről. A gyűjtött adatok például a következők: az operációs rendszer típusa és verziója, a futó folyamatok, a gép neve, az IP-címek, a bejelentkezett felhasználó és a bérlő azonosítója.  

## <a name="data-protection"></a>Adatvédelem
**Az adatok elkülönítése**: adatok az egyes összetevők teljes hello szolgáltatás logikailag külön maradnak. Az összes adat szervezet szerint van megcímkézve. A címkézés továbbra is fennáll, hello adatok életciklus során, és van kényszerítve hello szolgáltatást minden egyes rétegben. 

**Adat-hozzáférési**: tooprovide biztonsági javaslatok és vizsgálja meg az esetleges biztonsági fenyegetéseket jelezhetnek, a Microsoft személyzete elérhessék az összegyűjtött vagy a szolgáltatások által elemzett információkkal. Toohello mérvadónak [Microsoft Online Services használati](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) és [adatvédelmi nyilatkozat](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), amely adja meg, hogy Microsoft nem használja a felhasználói adatok, vagy származnia hirdetését vagy hasonló származó információk kereskedelmi célra. biztonsági javaslatok tooprovide és vizsgálja meg az esetleges biztonsági fenyegetéseket jelezhetnek, a Microsoft személyzete elérhessék az összegyűjtött vagy a szolgáltatások által elemzett információkkal. Csak vesszük ügyféladatok szükséges tooprovide, az Azure szolgáltatások, beleértve azokat a szolgáltatásokat nyújtó kompatibilis céljából. Amely a saját adatok összes jogok tooyour megőrzése mellett.

**Adatok felhasználásával**: Microsoft minták használ, illetve több körében tapasztalható fenyegetésfelderítési adataival bérlők tooenhance a megelőzése és észlelési képességek, azt megteheti hello adatvédelmi kötelezettségvállalások leírtak szerint a [adatvédelmi Utasítás](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).

> [!NOTE]
> Adatok helye szintjén állítható be hello OMS munkaterület, hello kezdeti OMS biztonsági és a naplózási konfiguráció folyamat része hello munkaterület létrehozása során.
> 
> 

## <a name="see-also"></a>Lásd még:
Ebből a dokumentumból megtudta, hogyan kezeli az OMS az adatokat, és hogyan gondoskodik a védelmükről. További információ az OMS biztonsági és naplózási megoldás toolearn lásd:

* [Az Operations Management Suite (OMS) áttekintése](operations-management-suite-overview.md)
* [Figyelés és a válaszoló tooSecurity riasztásait az Operations Management Suite biztonsági és naplózási megoldás](oms-security-responding-alerts.md)
* [Az erőforrások figyelése az Operations Management Suite biztonsági és auditálási megoldásban](oms-security-monitoring-resources.md)

