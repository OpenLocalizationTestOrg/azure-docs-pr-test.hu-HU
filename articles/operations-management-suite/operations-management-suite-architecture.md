---
title: "Felügyeleti csomag (OMS) architektúra aaaOperations |} Microsoft Docs"
description: "A Microsoft Operations Management Suite (OMS) a Microsoft felhőalapú informatikai felügyeleti megoldása, amely segít a helyszíni és a felhőalapú infrastruktúra kezelésében és védelmében.  Ez a cikk hello különböző szolgáltatásait az OMS azonosítja, és hivatkozások tootheir részletes tartalmat."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 40e41686-7e35-4d85-bbe8-edbcb295a534
ms.service: operations-management-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/11/2017
ms.author: bwren
ms.openlocfilehash: fa3227aa9c19219009ebe363b7fd2d6565cec59c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="oms-architecture"></a>OMS-architektúra
Az [Operations Management Suite (OMS)](https://azure.microsoft.com/documentation/services/operations-management-suite/) felhőalapú szolgáltatások gyűjteménye a helyszíni és a felhőalapú környezet kezeléséhez.  Ez a cikk ismerteti a hello különböző helyszíni és felhőalapú összetevőinek OMS és magas szintű felhőalapú számítási architektúráját.  Minden egyes szolgáltatás további részletekért olvassa el a toohello dokumentációját.

## <a name="log-analytics"></a>Log Analytics
Gyűjtött összes adat [Naplóelemzési](https://azure.microsoft.com/documentation/services/log-analytics/) hello OMS-tárházban, amely az Azure-ban üzemeltetett tárolja.  Csatlakoztatott adatforrások hozhat létre a történő hello OMS tárházba összegyűjtött adatokat.  Jelenleg háromféle csatlakoztatott forrás támogatott.

* Egy telepített ügynök egy [Windows](../log-analytics/log-analytics-windows-agents.md) vagy [Linux](../log-analytics/log-analytics-linux-agents.md) tooOMS közvetlenül csatlakoztatott számítógép.
* A System Center Operations Manager (SCOM) felügyeleti csoport [tooLog Analytics csatlakoztatott](../log-analytics/log-analytics-om-agents.md) .  SCOM-ügynököt az események és a teljesítmény adatok tooLog Analytics felügyeleti kiszolgálókat toocommunicate továbbra is.
* [Azure-tárfiók,](../log-analytics/log-analytics-azure-storage.md) amely [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md)-adatokat gyűjt feldolgozói szerepkörből, webes szerepkörből vagy virtuális gépről az Azure-ban.

Adatforrások Naplóelemzési gyűjti össze az csatlakoztatott móddal, többek között a eseménynaplóit és a teljesítményszámlálók hello adatok megadása.  Megoldások funkció tooOMS hozzáadása, és könnyen felveheti tooyour munkaterület a hello [OMS megoldások gyűjtemény](../log-analytics/log-analytics-add-solutions.md).  Néhány megoldás egy közvetlen kapcsolat tooLog Analytics az SCOM-ügynökök lehet szükség, míg más esetekben előfordulhat, hogy egy további ügynök toobe telepítve.

A Naplóelemzési rendelkezik egy webes portál, hogy toomanage OMS-erőforrások használata, hozzáadása és konfigurálása az OMS-megoldások, és tekintheti meg és elemezhetik a hello OMS-tárházban.

![A Log Analytics magas szintű architektúrája](media/operations-management-suite-architecture/log-analytics.png)

## <a name="azure-automation"></a>Azure Automation
[Azure Automation runbookjai](http://azure.microsoft.com/documentation/services/automation) hello Azure felhőben való végrehajtásakor, és érhetik el erőforrásokat, amelyek az Azure, a felhőszolgáltatásokra vagy érhető el a nyilvános interneten hello.  A helyi adatközpont helyszíni gépeit is kijelölheti [hibrid runbook-feldolgozó](../automation/automation-hybrid-runbook-worker.md) használatával, hogy a runbookok elérhessenek helyi erőforrásokat.

[A DSC-konfigurációk](../automation/automation-dsc-overview.md) közvetlenül alkalmazott tooAzure virtuális gépek az Azure Automationben tárolt lehet.  Egyéb fizikai és virtuális gépek használói kérhetnek konfigurációk hello Azure Automation DSC lekérési kiszolgálójával.

Azure Automation szolgáltatásbeli rendelkezik egy OMS-megoldás, amely statisztikáit jeleníti meg, és hivatkozásokat tartalmaz toolaunch hello Azure-portálon a műveleteket.

![Az Azure Automation magas szintű architektúrája](media/operations-management-suite-architecture/automation.png)

## <a name="azure-backup"></a>Azure Backup
Az [Azure Backup](http://azure.microsoft.com/documentation/services/backup) védett adatainak tárolása egy meghatározott földrajzi régióban elhelyezkedő biztonságimásolat-tárolóban történik.  hello adatait replikálja a rendszer belül hello ugyanabban a régióban, és, tároló hello típusától függően is lehet további redundancia replikált tooanother régióját.

Az Azure Backup három alapvető alkalmazási helyzetben használható.

* Windows rendszerű gép Azure Backup-ügynökkel.  Ez lehetővé teszi, hogy toobackup fájlokat és mappákat a Windows server vagy az ügyfél közvetlenül tooyour Azure mentési tárolóval.  
* System Center Data Protection Manager (DPM) vagy Microsoft Azure Backup Server. Ez lehetővé teszi, hogy Ön tooleverage DPM vagy a Microsoft Azure Backup Server toobackup fájlok és mappák továbbá tooapplication munkaterhelések, például az SQL és a SharePoint toolocal tárolási és majd replikálása tooyour Azure mentési tárolóval.
* Azure Virtual Machine Extensions.  Ez lehetővé teszi toobackup Azure virtuális gépek tooyour Azure mentési tárolóval.

Azure biztonsági mentés egy OMS-megoldás, amely statisztikáit jeleníti meg, és hivatkozásokat tartalmaz toolaunch hello Azure-portál a műveleteket sem rendelkezik.

![Az Azure Backup magas szintű architektúrája](media/operations-management-suite-architecture/backup.png)

## <a name="azure-site-recovery"></a>Azure Site Recovery
Az [Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) koordinálja a virtuális gépek és a fizikai kiszolgálók replikálását, feladatátvételét és feladat-visszavételét. A replikációs adatcsere Hyper-V-gazdagépek, VMware hipervizorok és az elsődleges és másodlagos adatközpontjaiban fizikai kiszolgálók között, vagy hello adatközpont és az Azure storage között.  A Site Recovery a metaadatokat meghatározott földrajzi Azure-régióban elhelyezkedő tárolókban tárolja. Nem replikált adatok hello Site Recovery szolgáltatásban tárolja.

Az Azure Site Recovery három alapvető replikációs helyzetben használható.

**Hyper-V virtuális gépek replikálása**

* Ha a Hyper-V virtuális gépek VMM-felhőkben felügyelt, tooa másodlagos center vagy tooAzure adattárolás replikálhatja.  Replikációs tooAzure van egy biztonságos internetkapcsolaton keresztül.  Replikációs tooa másodlagos adatközpontba hello LAN felett van.
* Hyper-V virtuális gépek nem a VMM felügyelete alatt, ha csak a tárolási tooAzure replikálhatja.  Replikációs tooAzure van egy biztonságos internetkapcsolaton keresztül.

**VMWare virtuális gépek replikálása**

* VMware virtuális gépek tooa másodlagos adatközpontba VMware vagy tooAzure tárolási futtató replikálhatja.  Replikációs tooAzure akkor fordulhat elő, a pont-pont VPN vagy Azure ExpressRoute vagy biztonságos internetkapcsolaton keresztül. Replikációs tooa másodlagos adatközpontba hello InMage Scout adatcsatornán keresztül történik.

**A fizikai Windows- és Linux-kiszolgálók replikálása** 

* Fizikai kiszolgálók tooa datacenter vagy tooAzure háttértár replikálhatja. Replikációs tooAzure akkor fordulhat elő, a pont-pont VPN vagy Azure ExpressRoute vagy biztonságos internetkapcsolaton keresztül. Replikációs tooa másodlagos adatközpontba hello InMage Scout adatcsatornán keresztül történik.  Az Azure Site Recovery egy OMS megoldást, amely megjeleníti a statisztikai adatokat tartalmaz, de a műveleteket hello Azure-portálon kell használnia.

![Az Azure Site Recovery magas szintű architektúrája](media/operations-management-suite-architecture/site-recovery.png)

## <a name="next-steps"></a>Következő lépések
* További tudnivalók a [Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) szolgáltatásról.
* További tudnivalók az [Azure Automation](https://azure.microsoft.com/documentation/services/automation) szolgáltatásról.
* További tudnivalók az [Azure Backup](http://azure.microsoft.com/documentation/services/backup) szolgáltatásról.
* További tudnivalók az [Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) szolgáltatásról.

