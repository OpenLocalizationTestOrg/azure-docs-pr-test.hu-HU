---
title: "az Operations Management Suite (OMS) dokumentációja – oktatóanyagok aaaAzure |} Microsoft Docs"
description: "A Microsoft Operations Management Suite (OMS) a Microsoft felhőalapú informatikai felügyeleti megoldása, amely segít a helyszíni és a felhőalapú infrastruktúra kezelésében és védelmében. Ez a cikk hello különböző szolgáltatásait az OMS azonosítja, és hivatkozások tootheir részletes tartalmat."
services: operations-management-suite
author: carolz
manager: carolz
layout: LandingPage
ms.assetid: 
ms.service: operations-management-suite
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: landing-page
ms.date: 01/23/2017
ms.author: carolz
ms.openlocfilehash: 11a8f5ecb3d84aed90554510fc1bb43320fdebb2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-operations-management-suite-oms"></a>Mi az az Operations Management Suite (OMS)?
A Microsoft Operations Management Suite (OMS) a Microsoft felhőalapú informatikai felügyeleti megoldása, amely segít a helyszíni és a felhőalapú infrastruktúra kezelésében és védelmében.  Mivel az OMS felhőalapú szolgáltatás, gyorsan és az infrastruktúra-szolgáltatásokra fordított minimális mértékű befektetéssel üzembe helyezhető.  Az új funkciók bevezetése automatikus, így csökkenthetők a folyamatos karbantartás és frissítés költségei.

Ezenkívül tooproviding értékes szolgáltatások a saját, OMS-ben integrálható a System Center-összetevők, például a System Center Operations Manager tooextend a meglévő felügyeleti beruházások hello felhőbe.  A System Center és OMS hogyan tudnak együttműködni egy teljes hibrid felület tooprovide.

a következő szakaszok hello hello eltérő érték területek OMS és hello szolgáltatás, amely végrehajtja a magas szintű leírását adhatja meg.  Olvassa el a tooOMS architektúra hello különböző OMS-összetevők áttekintését előtt hello megtekintésével részletes minden dokumentációját.

## <a name="insight-and-analyticsmediaoperations-management-suite-overviewicon-insight-analyticspng-insight-and-analytics"></a>![Betekintések és elemzés](media/operations-management-suite-overview/icon-insight-analytics.png) Betekintések és elemzés
A [Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) segít az operációs rendszerek és alkalmazások által generált napló- és teljesítményadatokat összegyűjteni, keresni, a köztük lévő összefüggéseket felderíteni, illetve az adatok alapján cselekedni. Biztosít a valós idejű operational insights szolgáltatással integrált keresés, és egyéni irányítópultok tooreadily elemzése több millió rekordot összes a számítási feladatok és a kiszolgálók fizikai helytől függetlenül.

Is egyszerűen hozzáadhatja: megoldásokkal tooLog, amely meghatározza az összegyűjtött adatok toobe, és az elemzés logikát hello elemzés.  Megoldások további funkciókat, amelyek automatikusan a rendszer a minimális tooagents vagy a konfiguráció nem tartalmazhat.  Ezenkívül toousing elemzésére szolgáló eszközöket által biztosított egyes megoldások között hello teljes adatkészlet rendelés toocorrelate adatok között rendszerek és alkalmazások egyéni keresések végezheti el.  

## <a name="automation--controlmediaoperations-management-suite-overviewicon-automation-controlpng-automation--control"></a>![Automatizálás és vezérlés](media/operations-management-suite-overview/icon-automation-control.png) Automatizálás és vezérlés
Azure Automation szolgáltatásbeli automatizálja a felügyeleti folyamatok [runbookok](../automation/automation-runbook-types.md) , amely PowerShell alapulnak, és hello Azure felhőben futtatni.  A runbookok hozzáférhetnek minden olyan termékhez vagy szolgáltatáshoz, amely PowerShell használatával felügyelhető, így többek között a más felhőkben – például az Amazon Web Servicesben (AWS) – található erőforrásokhoz is.  A Runbookok is az adott kiszolgálón a helyi adatközponti toomanage helyi erőforrásokat az hajtható végre.

Az Azure Automation a [PowerShell DSC](../automation/automation-dsc-overview.md) használatával biztosít konfigurációkezelést.  Hozhat létre és Azure-ban üzemeltetett DSC-erőforrások kezelése és alkalmazza őket toocloud és a helyszíni rendszer toodefine és automatikusan kényszeríthetnek a konfigurációt.

## <a name="protection-and-recoverymediaoperations-management-suite-overviewicon-protection-recoverypng-protection-and-disaster-recovery"></a>![Védelem és helyreállítás](media/operations-management-suite-overview/icon-protection-recovery.png) Védelem és vészhelyreállítás
Az [Azure Backup](http://azure.microsoft.com/documentation/services/backup) védelmet biztosít az alkalmazásadatok számára, valamint évekig megőrzi őket minden tőkebefektetés nélkül és minimális működési költségek mellett.  Azt a fizikai és virtuális Windows kiszolgálók hozzáadása tooapplication munkaterhelések, például az SQL Server és a SharePoint adatainak biztonsági mentése.  Azt is használhatják a System Center Data Protection Manager (DPM) tooreplicate védett adatok tooAzure a redundancia és a hosszú távú tároláshoz.

[Az Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) hozzájárul tooyour üzleti folytonossági és vészhelyreállítási (BCDR) helyreállítási stratégia megvalósításában a replikáció, feladatátvétel és helyreállítás helyszíni Hyper-V virtuális gépek, a VMware virtuális gépek és a fizikai Windows / Linux-kiszolgálókon. Gépek tooa másodlagos adatközpontba replikálja, vagy az Adatközpont kiterjesztésére tooAzure végez replikációt. A Site Recovery is egyszerű feladatátvételt és helyreállítási lehetőségeket biztosít a számítási feladatok számára. Integrálható az olyan vészhelyreállítási mechanizmusokkal, mint például az SQL Server AlwaysOn, valamint helyreállítási terveket kínál a több számítógépen rétegzett számítási feladatok egyszerű feladatátvételéhez.

## <a name="oms-security-and-compliancemediaoperations-management-suite-overviewicon-security-compliancepng-security-and-compliance"></a>![OMS Biztonság és megfelelőség](media/operations-management-suite-overview/icon-security-compliance.png) Biztonság és megfelelőség
Biztonsági és megfelelőségi segít azonosítani, felmérési és biztonsági kockázatok tooyour infrastrukturális mérséklése.  Ezeket a szolgáltatásokat OMS több megoldásokat, amely a naplózási adatokat és ügynök rendszerek tooassist-konfiguráció elemzése Naplóelemzési keresztül valósulnak meg hello folyamatos biztonsági környezet biztosításában.

* Hello [biztonsági, naplózási megoldást](oms-security-getting-started.md) gyűjti és elemzi a biztonsági események felügyelt rendszerekről tooidentify gyanús tevékenységgel.
* Hello [kártevőirtó megoldás](../log-analytics/log-analytics-malware.md) szembeni védelem a felügyelt rendszerekről hello állapotának kapcsolatos jelentések.  
* hello rendszerfrissítések megoldás hello biztonsági frissítések elemzést végez, és más a frissítések a kezelt számítógépeken, hogy könnyen rendszerek azonosításához igénylő javítását.

## <a name="next-steps"></a>Következő lépések
* További tudnivalók a [Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) szolgáltatásról.
* További tudnivalók az [Azure Automation](../automation/automation-intro.md) szolgáltatásról.
* További tudnivalók az [Azure Backup](http://azure.microsoft.com/documentation/services/backup) szolgáltatásról.
* További tudnivalók az [Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) szolgáltatásról.

