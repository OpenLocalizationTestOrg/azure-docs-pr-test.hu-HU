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
# <a name="oms-architecture"></a><span data-ttu-id="f3d65-104">OMS-architektúra</span><span class="sxs-lookup"><span data-stu-id="f3d65-104">OMS architecture</span></span>
<span data-ttu-id="f3d65-105">Az [Operations Management Suite (OMS)](https://azure.microsoft.com/documentation/services/operations-management-suite/) felhőalapú szolgáltatások gyűjteménye a helyszíni és a felhőalapú környezet kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="f3d65-105">[Operations Management Suite (OMS)](https://azure.microsoft.com/documentation/services/operations-management-suite/) is a collection of cloud-based services for managing your on-premises and cloud environments.</span></span>  <span data-ttu-id="f3d65-106">Ez a cikk ismerteti a hello különböző helyszíni és felhőalapú összetevőinek OMS és magas szintű felhőalapú számítási architektúráját.</span><span class="sxs-lookup"><span data-stu-id="f3d65-106">This article describes hello different on-premises and cloud components of OMS and their high level cloud computing architecture.</span></span>  <span data-ttu-id="f3d65-107">Minden egyes szolgáltatás további részletekért olvassa el a toohello dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="f3d65-107">You can refer toohello documentation for each service for further details.</span></span>

## <a name="log-analytics"></a><span data-ttu-id="f3d65-108">Log Analytics</span><span class="sxs-lookup"><span data-stu-id="f3d65-108">Log Analytics</span></span>
<span data-ttu-id="f3d65-109">Gyűjtött összes adat [Naplóelemzési](https://azure.microsoft.com/documentation/services/log-analytics/) hello OMS-tárházban, amely az Azure-ban üzemeltetett tárolja.</span><span class="sxs-lookup"><span data-stu-id="f3d65-109">All data collected by [Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) is stored in hello OMS repository which is hosted in Azure.</span></span>  <span data-ttu-id="f3d65-110">Csatlakoztatott adatforrások hozhat létre a történő hello OMS tárházba összegyűjtött adatokat.</span><span class="sxs-lookup"><span data-stu-id="f3d65-110">Connected Sources generate data collected into hello OMS repository.</span></span>  <span data-ttu-id="f3d65-111">Jelenleg háromféle csatlakoztatott forrás támogatott.</span><span class="sxs-lookup"><span data-stu-id="f3d65-111">There are currently three types of connected sources supported.</span></span>

* <span data-ttu-id="f3d65-112">Egy telepített ügynök egy [Windows](../log-analytics/log-analytics-windows-agents.md) vagy [Linux](../log-analytics/log-analytics-linux-agents.md) tooOMS közvetlenül csatlakoztatott számítógép.</span><span class="sxs-lookup"><span data-stu-id="f3d65-112">An agent installed on a [Windows](../log-analytics/log-analytics-windows-agents.md) or [Linux](../log-analytics/log-analytics-linux-agents.md) computer connected directly tooOMS.</span></span>
* <span data-ttu-id="f3d65-113">A System Center Operations Manager (SCOM) felügyeleti csoport [tooLog Analytics csatlakoztatott](../log-analytics/log-analytics-om-agents.md) .</span><span class="sxs-lookup"><span data-stu-id="f3d65-113">A System Center Operations Manager (SCOM) management group [connected tooLog Analytics](../log-analytics/log-analytics-om-agents.md) .</span></span>  <span data-ttu-id="f3d65-114">SCOM-ügynököt az események és a teljesítmény adatok tooLog Analytics felügyeleti kiszolgálókat toocommunicate továbbra is.</span><span class="sxs-lookup"><span data-stu-id="f3d65-114">SCOM agents continue toocommunicate with management servers which forward events and performance data tooLog Analytics.</span></span>
* <span data-ttu-id="f3d65-115">[Azure-tárfiók,](../log-analytics/log-analytics-azure-storage.md) amely [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md)-adatokat gyűjt feldolgozói szerepkörből, webes szerepkörből vagy virtuális gépről az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="f3d65-115">An [Azure storage account](../log-analytics/log-analytics-azure-storage.md) that collects [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) data from a worker role, web role, or virtual machine in Azure.</span></span>

<span data-ttu-id="f3d65-116">Adatforrások Naplóelemzési gyűjti össze az csatlakoztatott móddal, többek között a eseménynaplóit és a teljesítményszámlálók hello adatok megadása.</span><span class="sxs-lookup"><span data-stu-id="f3d65-116">Data sources define hello data that Log Analytics collects from connected sources including event logs and performance counters.</span></span>  <span data-ttu-id="f3d65-117">Megoldások funkció tooOMS hozzáadása, és könnyen felveheti tooyour munkaterület a hello [OMS megoldások gyűjtemény](../log-analytics/log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="f3d65-117">Solutions add functionality tooOMS and can easily be added tooyour workspace from hello [OMS Solutions Gallery](../log-analytics/log-analytics-add-solutions.md).</span></span>  <span data-ttu-id="f3d65-118">Néhány megoldás egy közvetlen kapcsolat tooLog Analytics az SCOM-ügynökök lehet szükség, míg más esetekben előfordulhat, hogy egy további ügynök toobe telepítve.</span><span class="sxs-lookup"><span data-stu-id="f3d65-118">Some solutions may require a direct connection tooLog Analytics from SCOM agents while others may require an additional agent toobe installed.</span></span>

<span data-ttu-id="f3d65-119">A Naplóelemzési rendelkezik egy webes portál, hogy toomanage OMS-erőforrások használata, hozzáadása és konfigurálása az OMS-megoldások, és tekintheti meg és elemezhetik a hello OMS-tárházban.</span><span class="sxs-lookup"><span data-stu-id="f3d65-119">Log Analytics has a web-based portal that you can use toomanage OMS resources, add and configure OMS solutions, and view and analyze data in hello OMS repository.</span></span>

![A Log Analytics magas szintű architektúrája](media/operations-management-suite-architecture/log-analytics.png)

## <a name="azure-automation"></a><span data-ttu-id="f3d65-121">Azure Automation</span><span class="sxs-lookup"><span data-stu-id="f3d65-121">Azure Automation</span></span>
<span data-ttu-id="f3d65-122">[Azure Automation runbookjai](http://azure.microsoft.com/documentation/services/automation) hello Azure felhőben való végrehajtásakor, és érhetik el erőforrásokat, amelyek az Azure, a felhőszolgáltatásokra vagy érhető el a nyilvános interneten hello.</span><span class="sxs-lookup"><span data-stu-id="f3d65-122">[Azure Automation runbooks](http://azure.microsoft.com/documentation/services/automation) are executed in hello Azure cloud and can access resources that are in Azure, in other cloud services, or accessible from hello public Internet.</span></span>  <span data-ttu-id="f3d65-123">A helyi adatközpont helyszíni gépeit is kijelölheti [hibrid runbook-feldolgozó](../automation/automation-hybrid-runbook-worker.md) használatával, hogy a runbookok elérhessenek helyi erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="f3d65-123">You can also designate on-premises machines in your local data center using [Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md) so that runbooks can access local resources.</span></span>

<span data-ttu-id="f3d65-124">[A DSC-konfigurációk](../automation/automation-dsc-overview.md) közvetlenül alkalmazott tooAzure virtuális gépek az Azure Automationben tárolt lehet.</span><span class="sxs-lookup"><span data-stu-id="f3d65-124">[DSC configurations](../automation/automation-dsc-overview.md) stored in Azure Automation can be directly applied tooAzure virtual machines.</span></span>  <span data-ttu-id="f3d65-125">Egyéb fizikai és virtuális gépek használói kérhetnek konfigurációk hello Azure Automation DSC lekérési kiszolgálójával.</span><span class="sxs-lookup"><span data-stu-id="f3d65-125">Other physical and virtual machines can request configurations from hello Azure Automation DSC pull server.</span></span>

<span data-ttu-id="f3d65-126">Azure Automation szolgáltatásbeli rendelkezik egy OMS-megoldás, amely statisztikáit jeleníti meg, és hivatkozásokat tartalmaz toolaunch hello Azure-portálon a műveleteket.</span><span class="sxs-lookup"><span data-stu-id="f3d65-126">Azure Automation has an OMS solution that displays statistics and links toolaunch hello Azure portal for any operations.</span></span>

![Az Azure Automation magas szintű architektúrája](media/operations-management-suite-architecture/automation.png)

## <a name="azure-backup"></a><span data-ttu-id="f3d65-128">Azure Backup</span><span class="sxs-lookup"><span data-stu-id="f3d65-128">Azure Backup</span></span>
<span data-ttu-id="f3d65-129">Az [Azure Backup](http://azure.microsoft.com/documentation/services/backup) védett adatainak tárolása egy meghatározott földrajzi régióban elhelyezkedő biztonságimásolat-tárolóban történik.</span><span class="sxs-lookup"><span data-stu-id="f3d65-129">Protected data in [Azure Backup](http://azure.microsoft.com/documentation/services/backup) is stored in a backup vault located in a particular geographic region.</span></span>  <span data-ttu-id="f3d65-130">hello adatait replikálja a rendszer belül hello ugyanabban a régióban, és, tároló hello típusától függően is lehet további redundancia replikált tooanother régióját.</span><span class="sxs-lookup"><span data-stu-id="f3d65-130">hello data is replicated within hello same region and, depending on hello type of vault, may also be replicated tooanother region for further redundancy.</span></span>

<span data-ttu-id="f3d65-131">Az Azure Backup három alapvető alkalmazási helyzetben használható.</span><span class="sxs-lookup"><span data-stu-id="f3d65-131">Azure Backup has three fundamental scenarios.</span></span>

* <span data-ttu-id="f3d65-132">Windows rendszerű gép Azure Backup-ügynökkel.</span><span class="sxs-lookup"><span data-stu-id="f3d65-132">Windows machine with Azure Backup agent.</span></span>  <span data-ttu-id="f3d65-133">Ez lehetővé teszi, hogy toobackup fájlokat és mappákat a Windows server vagy az ügyfél közvetlenül tooyour Azure mentési tárolóval.</span><span class="sxs-lookup"><span data-stu-id="f3d65-133">This allows you toobackup files and folders from any Windows server or client directly tooyour Azure backup vault.</span></span>  
* <span data-ttu-id="f3d65-134">System Center Data Protection Manager (DPM) vagy Microsoft Azure Backup Server.</span><span class="sxs-lookup"><span data-stu-id="f3d65-134">System Center Data Protection Manager (DPM) or Microsoft Azure Backup Server.</span></span> <span data-ttu-id="f3d65-135">Ez lehetővé teszi, hogy Ön tooleverage DPM vagy a Microsoft Azure Backup Server toobackup fájlok és mappák továbbá tooapplication munkaterhelések, például az SQL és a SharePoint toolocal tárolási és majd replikálása tooyour Azure mentési tárolóval.</span><span class="sxs-lookup"><span data-stu-id="f3d65-135">This allows you tooleverage DPM or Microsoft Azure Backup Server toobackup files and folders in addition tooapplication workloads such as SQL and SharePoint toolocal storage and then replicate tooyour Azure backup vault.</span></span>
* <span data-ttu-id="f3d65-136">Azure Virtual Machine Extensions.</span><span class="sxs-lookup"><span data-stu-id="f3d65-136">Azure Virtual Machine Extensions.</span></span>  <span data-ttu-id="f3d65-137">Ez lehetővé teszi toobackup Azure virtuális gépek tooyour Azure mentési tárolóval.</span><span class="sxs-lookup"><span data-stu-id="f3d65-137">This allows you toobackup Azure virtual machines tooyour Azure backup vault.</span></span>

<span data-ttu-id="f3d65-138">Azure biztonsági mentés egy OMS-megoldás, amely statisztikáit jeleníti meg, és hivatkozásokat tartalmaz toolaunch hello Azure-portál a műveleteket sem rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="f3d65-138">Azure Backup has an OMS solution that displays statistics and links toolaunch hello Azure portal for any operations.</span></span>

![Az Azure Backup magas szintű architektúrája](media/operations-management-suite-architecture/backup.png)

## <a name="azure-site-recovery"></a><span data-ttu-id="f3d65-140">Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="f3d65-140">Azure Site Recovery</span></span>
<span data-ttu-id="f3d65-141">Az [Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) koordinálja a virtuális gépek és a fizikai kiszolgálók replikálását, feladatátvételét és feladat-visszavételét.</span><span class="sxs-lookup"><span data-stu-id="f3d65-141">[Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) orchestrates replication, failover, and failback of virtual machines and physical servers.</span></span> <span data-ttu-id="f3d65-142">A replikációs adatcsere Hyper-V-gazdagépek, VMware hipervizorok és az elsődleges és másodlagos adatközpontjaiban fizikai kiszolgálók között, vagy hello adatközpont és az Azure storage között.</span><span class="sxs-lookup"><span data-stu-id="f3d65-142">Replication data is exchanged between Hyper-V hosts, VMware hypervisors, and physical servers in primary and secondary datacenters, or between hello datacenter and Azure storage.</span></span>  <span data-ttu-id="f3d65-143">A Site Recovery a metaadatokat meghatározott földrajzi Azure-régióban elhelyezkedő tárolókban tárolja.</span><span class="sxs-lookup"><span data-stu-id="f3d65-143">Site Recovery stores metadata in vaults located in a particular geographic Azure region.</span></span> <span data-ttu-id="f3d65-144">Nem replikált adatok hello Site Recovery szolgáltatásban tárolja.</span><span class="sxs-lookup"><span data-stu-id="f3d65-144">No replicated data is stored by hello Site Recovery service.</span></span>

<span data-ttu-id="f3d65-145">Az Azure Site Recovery három alapvető replikációs helyzetben használható.</span><span class="sxs-lookup"><span data-stu-id="f3d65-145">Azure Site Recovery has three fundamental replication scenarios.</span></span>

<span data-ttu-id="f3d65-146">**Hyper-V virtuális gépek replikálása**</span><span class="sxs-lookup"><span data-stu-id="f3d65-146">**Replication of Hyper-V virtual machines**</span></span>

* <span data-ttu-id="f3d65-147">Ha a Hyper-V virtuális gépek VMM-felhőkben felügyelt, tooa másodlagos center vagy tooAzure adattárolás replikálhatja.</span><span class="sxs-lookup"><span data-stu-id="f3d65-147">If Hyper-V virtual machines are managed in VMM clouds, you can replicate tooa secondary data center or tooAzure storage.</span></span>  <span data-ttu-id="f3d65-148">Replikációs tooAzure van egy biztonságos internetkapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="f3d65-148">Replication tooAzure is over a secure internet connection.</span></span>  <span data-ttu-id="f3d65-149">Replikációs tooa másodlagos adatközpontba hello LAN felett van.</span><span class="sxs-lookup"><span data-stu-id="f3d65-149">Replication tooa secondary datacenter is over hello LAN.</span></span>
* <span data-ttu-id="f3d65-150">Hyper-V virtuális gépek nem a VMM felügyelete alatt, ha csak a tárolási tooAzure replikálhatja.</span><span class="sxs-lookup"><span data-stu-id="f3d65-150">If Hyper-V virtual machines aren’t managed by VMM, you can replicate tooAzure storage only.</span></span>  <span data-ttu-id="f3d65-151">Replikációs tooAzure van egy biztonságos internetkapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="f3d65-151">Replication tooAzure is over a secure internet connection.</span></span>

<span data-ttu-id="f3d65-152">**VMWare virtuális gépek replikálása**</span><span class="sxs-lookup"><span data-stu-id="f3d65-152">**Replication of VMWare virtual machines**</span></span>

* <span data-ttu-id="f3d65-153">VMware virtuális gépek tooa másodlagos adatközpontba VMware vagy tooAzure tárolási futtató replikálhatja.</span><span class="sxs-lookup"><span data-stu-id="f3d65-153">You can replicate VMware virtual machines tooa secondary datacenter running VMware or tooAzure storage.</span></span>  <span data-ttu-id="f3d65-154">Replikációs tooAzure akkor fordulhat elő, a pont-pont VPN vagy Azure ExpressRoute vagy biztonságos internetkapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="f3d65-154">Replication tooAzure can occur over a site-to-site VPN or Azure ExpressRoute or over a secure Internet connection.</span></span> <span data-ttu-id="f3d65-155">Replikációs tooa másodlagos adatközpontba hello InMage Scout adatcsatornán keresztül történik.</span><span class="sxs-lookup"><span data-stu-id="f3d65-155">Replication tooa secondary datacenter occurs over hello InMage Scout data channel.</span></span>

<span data-ttu-id="f3d65-156">**A fizikai Windows- és Linux-kiszolgálók replikálása**</span><span class="sxs-lookup"><span data-stu-id="f3d65-156">**Replication of physical Windows and Linux servers**</span></span> 

* <span data-ttu-id="f3d65-157">Fizikai kiszolgálók tooa datacenter vagy tooAzure háttértár replikálhatja.</span><span class="sxs-lookup"><span data-stu-id="f3d65-157">You can replicate physical servers tooa secondary datacenter or tooAzure storage.</span></span> <span data-ttu-id="f3d65-158">Replikációs tooAzure akkor fordulhat elő, a pont-pont VPN vagy Azure ExpressRoute vagy biztonságos internetkapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="f3d65-158">Replication tooAzure can occur over a site-to-site VPN or Azure ExpressRoute or over a secure Internet connection.</span></span> <span data-ttu-id="f3d65-159">Replikációs tooa másodlagos adatközpontba hello InMage Scout adatcsatornán keresztül történik.</span><span class="sxs-lookup"><span data-stu-id="f3d65-159">Replication tooa secondary datacenter occurs over hello InMage Scout data channel.</span></span>  <span data-ttu-id="f3d65-160">Az Azure Site Recovery egy OMS megoldást, amely megjeleníti a statisztikai adatokat tartalmaz, de a műveleteket hello Azure-portálon kell használnia.</span><span class="sxs-lookup"><span data-stu-id="f3d65-160">Azure Site Recovery has an OMS solution that displays some statistics, but you must use hello Azure portal for any operations.</span></span>

![Az Azure Site Recovery magas szintű architektúrája](media/operations-management-suite-architecture/site-recovery.png)

## <a name="next-steps"></a><span data-ttu-id="f3d65-162">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f3d65-162">Next steps</span></span>
* <span data-ttu-id="f3d65-163">További tudnivalók a [Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) szolgáltatásról.</span><span class="sxs-lookup"><span data-stu-id="f3d65-163">Learn about [Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics).</span></span>
* <span data-ttu-id="f3d65-164">További tudnivalók az [Azure Automation](https://azure.microsoft.com/documentation/services/automation) szolgáltatásról.</span><span class="sxs-lookup"><span data-stu-id="f3d65-164">Learn about [Azure Automation](https://azure.microsoft.com/documentation/services/automation).</span></span>
* <span data-ttu-id="f3d65-165">További tudnivalók az [Azure Backup](http://azure.microsoft.com/documentation/services/backup) szolgáltatásról.</span><span class="sxs-lookup"><span data-stu-id="f3d65-165">Learn about [Azure Backup](http://azure.microsoft.com/documentation/services/backup).</span></span>
* <span data-ttu-id="f3d65-166">További tudnivalók az [Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) szolgáltatásról.</span><span class="sxs-lookup"><span data-stu-id="f3d65-166">Learn about [Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery).</span></span>
