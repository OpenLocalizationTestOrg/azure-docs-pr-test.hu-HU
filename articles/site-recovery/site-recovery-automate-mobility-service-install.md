---
title: "Az Azure Automation DSC Szolgáltatásban a Site Recovery mobilitási szolgáltatás üzembe helyezése |} Microsoft Docs"
description: "Azure Automation DSC használata az Azure Site Recovery mobilitási szolgáltatás és az Azure agent automatikus telepítése a VMware virtuális gép és a fizikai kiszolgáló replikálás az Azure-bA"
services: site-recovery
documentationcenter: 
author: krnese
manager: lorenr
editor: 
ms.assetid: 1f8cd3ac-0522-48eb-a5f0-679ee9192ddb
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: krnese
ms.openlocfilehash: bcc5f11afbecac8fe63935f3401dd3e2d767e8aa
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-the-mobility-service-with-azure-automation-dsc-for-replication-of-vm"></a><span data-ttu-id="5f6de-103">Telepíteni a mobilitási szolgáltatást a replikáció a virtuális gépet az Azure Automation DSC Szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="5f6de-103">Deploy the Mobility service with Azure Automation DSC for replication of VM</span></span>
<span data-ttu-id="5f6de-104">Az Operations Management Suite azt biztosít egy átfogó biztonsági mentési és vész-helyreállítási megoldást, amely az üzletmenet folytonosságát biztosító terve részeként is használhatja.</span><span class="sxs-lookup"><span data-stu-id="5f6de-104">In Operations Management Suite, we provide you with a comprehensive backup and disaster recovery solution that you can use as part of your business continuity plan.</span></span>

<span data-ttu-id="5f6de-105">A Hyper-V együtt út indította Hyper-V-replika használatával.</span><span class="sxs-lookup"><span data-stu-id="5f6de-105">We started this journey together with Hyper-V by using Hyper-V Replica.</span></span> <span data-ttu-id="5f6de-106">De azt kibővítette a heterogén telepítő támogatásához, mert a felhő ügyfelek több hipervizorok és platformok.</span><span class="sxs-lookup"><span data-stu-id="5f6de-106">But we have expanded to support a heterogeneous setup because customers have multiple hypervisors and platforms in their clouds.</span></span>

<span data-ttu-id="5f6de-107">Futtat VMware munkaterhelések és/vagy fizikai kiszolgálók ma, ha a felügyeleti kiszolgáló futtatja az Azure Site Recovery-összetevőket a kommunikáció és replikálás az Azure-ral, kezeléséhez, a cél Azure esetén a környezetben.</span><span class="sxs-lookup"><span data-stu-id="5f6de-107">If you are running VMware workloads and/or physical servers today, a management server runs all of the Azure Site Recovery components in your environment to handle the communication and data replication with Azure, when Azure is your destination.</span></span>

## <a name="deploy-the-site-recovery-mobility-service-by-using-automation-dsc"></a><span data-ttu-id="5f6de-108">A Site Recovery mobilitási szolgáltatás telepítését Automation DSC</span><span class="sxs-lookup"><span data-stu-id="5f6de-108">Deploy the Site Recovery Mobility service by using Automation DSC</span></span>
<span data-ttu-id="5f6de-109">Kezdjük gyors részletes információkat a felügyeleti kiszolgáló funkciója végrehajtásával.</span><span class="sxs-lookup"><span data-stu-id="5f6de-109">Let's start by doing a quick breakdown of what this management server does.</span></span>

<span data-ttu-id="5f6de-110">A felügyeleti kiszolgáló számos kiszolgálói szerepkört futtatja.</span><span class="sxs-lookup"><span data-stu-id="5f6de-110">The management server runs several server roles.</span></span> <span data-ttu-id="5f6de-111">Ezek a szerepkörök egyik *konfigurációs*, amely kommunikáció koordinálását, és kezeli az adatokat replikálás és helyreállítási folyamatok.</span><span class="sxs-lookup"><span data-stu-id="5f6de-111">One of these roles is *configuration*, which coordinates communication and manages data replication and recovery processes.</span></span>

<span data-ttu-id="5f6de-112">Emellett a *folyamat* szerepkör replikációs átjáróként.</span><span class="sxs-lookup"><span data-stu-id="5f6de-112">In addition, the *process* role acts as a replication gateway.</span></span> <span data-ttu-id="5f6de-113">Ezt a szerepkört kap replikációs adatokat védett forrásgépek, gyorsítótárazás, tömörítés és titkosítás segítségével optimalizálja őket, és küld egy Azure storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="5f6de-113">This role receives replication data from protected source machines, optimizes it with caching, compression, and encryption, and then sends it to an Azure storage account.</span></span> <span data-ttu-id="5f6de-114">A funkciók a folyamat szerepkör egyik is védett gépek leküldéssel telepíteni a mobilitási szolgáltatást, és végezze el a VMware virtuális gépek automatikus észlelése.</span><span class="sxs-lookup"><span data-stu-id="5f6de-114">One of the functions for the process role is also to push installation of the Mobility service to protected machines and perform automatic discovery of VMware VMs.</span></span>

<span data-ttu-id="5f6de-115">Ha a feladat-visszavétel, az Azure-ból a *fő* szerepkör Ez a művelet részeként fogja kezelni a replikációs adatokat.</span><span class="sxs-lookup"><span data-stu-id="5f6de-115">If there's a failback from Azure, the *master target* role will handle the replication data as part of this operation.</span></span>

<span data-ttu-id="5f6de-116">A védett gépek azt támaszkodhat a *mobilitásiszolgáltatás*.</span><span class="sxs-lookup"><span data-stu-id="5f6de-116">For the protected machines, we rely on the *Mobility service*.</span></span> <span data-ttu-id="5f6de-117">Ez az összetevő van telepítve minden gépen (VMware virtuális gép vagy fizikai kiszolgálón), amely az Azure-bA replikálni kívánt.</span><span class="sxs-lookup"><span data-stu-id="5f6de-117">This component is deployed to every machine (VMware VM or physical server) that you want to replicate to Azure.</span></span> <span data-ttu-id="5f6de-118">A gépen végbemenő adatírásokat, és továbbítja őket a felügyeleti kiszolgáló (folyamat szerepkör).</span><span class="sxs-lookup"><span data-stu-id="5f6de-118">It captures data writes on the machine and forwards them to the management server (process role).</span></span>

<span data-ttu-id="5f6de-119">Üzleti folytonosság kezelése, fontos a munkaterhelések, az infrastruktúra és az érintett összetevők megismerése.</span><span class="sxs-lookup"><span data-stu-id="5f6de-119">When you're dealing with business continuity, it's important to understand your workloads, your infrastructure, and the components involved.</span></span> <span data-ttu-id="5f6de-120">Majd is megfelel a követelményeknek a helyreállítási idő célkitűzése (RTO) és a helyreállítási időkorlát (RPO).</span><span class="sxs-lookup"><span data-stu-id="5f6de-120">You can then meet the requirements for your recovery time objective (RTO) and recovery point objective (RPO).</span></span> <span data-ttu-id="5f6de-121">Ebben a környezetben a mobilitási szolgáltatás a kulcs annak biztosításához, hogy a munkaterhelések védelmének módon teheti meg.</span><span class="sxs-lookup"><span data-stu-id="5f6de-121">In this context, the Mobility service is key to ensuring that your workloads are protected as you would expect.</span></span>

<span data-ttu-id="5f6de-122">Igen, hogyan, egy optimalizált módon biztosíthatja, hogy rendelkezik-e súgó néhány Operations Management Suite összetevői egy megbízható védett telepítése?</span><span class="sxs-lookup"><span data-stu-id="5f6de-122">So how can you, in an optimized way, ensure that you have a reliable protected setup with help from some Operations Management Suite components?</span></span>

<span data-ttu-id="5f6de-123">Ez a cikk bemutatja, hogyan használhatók az Azure Automation szükséges konfiguráló (DSC), és a hely helyreállítását követően annak érdekében, hogy:</span><span class="sxs-lookup"><span data-stu-id="5f6de-123">This article provides an example of how you can use Azure Automation Desired State Configuration (DSC), together with Site Recovery, to ensure that:</span></span>

* <span data-ttu-id="5f6de-124">A mobilitási szolgáltatás és az Azure Virtuálisgép-ügynök a védeni kívánt Windows gépek vannak telepítve.</span><span class="sxs-lookup"><span data-stu-id="5f6de-124">The Mobility service and Azure VM agent are deployed to the Windows machines that you want to protect.</span></span>
* <span data-ttu-id="5f6de-125">A mobilitási szolgáltatás és az Azure Virtuálisgép-ügynök mindig futnak, amikor Azure a replikációs cél.</span><span class="sxs-lookup"><span data-stu-id="5f6de-125">The Mobility service and Azure VM agent are always running when Azure is the replication target.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5f6de-126">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5f6de-126">Prerequisites</span></span>
* <span data-ttu-id="5f6de-127">A tárház tárolja a szükséges telepítés</span><span class="sxs-lookup"><span data-stu-id="5f6de-127">A repository to store the required setup</span></span>
* <span data-ttu-id="5f6de-128">A tárház tárolja a szükséges jelszót regisztrálása a management Server kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="5f6de-128">A repository to store the required passphrase to register with the management server</span></span>

  > [!NOTE]
  > <span data-ttu-id="5f6de-129">Minden felügyeleti kiszolgáló létrejön egy egyedi jelszót.</span><span class="sxs-lookup"><span data-stu-id="5f6de-129">A unique passphrase is generated for each management server.</span></span> <span data-ttu-id="5f6de-130">Ha több felügyeleti kiszolgálót telepíteni kívánja, akkor győződjön meg arról, hogy a helyes jelszót a passphrase.txt fájl tárolja.</span><span class="sxs-lookup"><span data-stu-id="5f6de-130">If you are going to deploy multiple management servers, you have to ensure that the correct passphrase is stored in the passphrase.txt file.</span></span>
  >
  >
* <span data-ttu-id="5f6de-131">A Windows Management Framework (WMF) 5.0-védelmi (előfeltétele annak, hogy Automation DSC) engedélyezni kívánt számítógépeken telepítve van</span><span class="sxs-lookup"><span data-stu-id="5f6de-131">Windows Management Framework (WMF) 5.0 installed on the machines that you want to enable for protection (a requirement for Automation DSC)</span></span>

  > [!NOTE]
  > <span data-ttu-id="5f6de-132">A DSC-ből a Windows gépeken, amelyek WMF 4.0-s verziójával használni kívánt, szakaszában olvashat [DSC használata a kapcsolat nélküli környezetben](## Use DSC in disconnected environments).</span><span class="sxs-lookup"><span data-stu-id="5f6de-132">If you want to use DSC for Windows machines that have WMF 4.0 installed, see the section [Use DSC in disconnected environments](## Use DSC in disconnected environments).</span></span>
  

<span data-ttu-id="5f6de-133">A mobilitási szolgáltatást a parancssor használatával is telepíthető, és több argumentumot fogad el.</span><span class="sxs-lookup"><span data-stu-id="5f6de-133">The Mobility service can be installed through the command line and accepts several arguments.</span></span> <span data-ttu-id="5f6de-134">Ezért a bináris fájlok rendelkezik (után azokat a telepítő kibontása), és tárolja őket egy helyen, ahol helyreállíthatók a DSC-konfiguráció használatával.</span><span class="sxs-lookup"><span data-stu-id="5f6de-134">That’s why you need to have the binaries (after extracting them from your setup) and store them in a place where you can retrieve them by using a DSC configuration.</span></span>

## <a name="step-1-extract-binaries"></a><span data-ttu-id="5f6de-135">1. lépés: Kivonat bináris fájlok</span><span class="sxs-lookup"><span data-stu-id="5f6de-135">Step 1: Extract binaries</span></span>
1. <span data-ttu-id="5f6de-136">Bontsa ki az ehhez a telepítéshez szükséges, hogy keresse meg a felügyeleti kiszolgálón a következő könyvtárra:</span><span class="sxs-lookup"><span data-stu-id="5f6de-136">To extract the files that you need for this setup, browse to the following directory on your management server:</span></span>

    <span data-ttu-id="5f6de-137">**\Microsoft azure hely Recovery\home\svsystems\pushinstallsvc\repository**</span><span class="sxs-lookup"><span data-stu-id="5f6de-137">**\Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**</span></span>

    <span data-ttu-id="5f6de-138">Ebben a mappában kell: nevű MSI-fájl</span><span class="sxs-lookup"><span data-stu-id="5f6de-138">In this folder, you should see an MSI file named:</span></span>

    <span data-ttu-id="5f6de-139">**Microsoft-ASR_UA_version_Windows_GA_date_Release.exe**</span><span class="sxs-lookup"><span data-stu-id="5f6de-139">**Microsoft-ASR_UA_version_Windows_GA_date_Release.exe**</span></span>

    <span data-ttu-id="5f6de-140">Az alábbi parancs segítségével bontsa ki a telepítő:</span><span class="sxs-lookup"><span data-stu-id="5f6de-140">Use the following command to extract the installer:</span></span>

    <span data-ttu-id="5f6de-141">**.\Microsoft-ASR_UA_9.1.0.0_Windows_GA_02May2016_release.exe /q /x:C:\Users\Administrator\Desktop\Mobility_Service\Extract**</span><span class="sxs-lookup"><span data-stu-id="5f6de-141">**.\Microsoft-ASR_UA_9.1.0.0_Windows_GA_02May2016_release.exe /q /x:C:\Users\Administrator\Desktop\Mobility_Service\Extract**</span></span>
2. <span data-ttu-id="5f6de-142">Válassza ki az összes fájlt, és küldje el a tömörített mappa.</span><span class="sxs-lookup"><span data-stu-id="5f6de-142">Select all files and send them to a compressed (zipped) folder.</span></span>

<span data-ttu-id="5f6de-143">Most már rendelkezik a bináris fájlokat, amelyekre szüksége van a mobilitási szolgáltatást a telepítés automatizálása Automation DSC használatával.</span><span class="sxs-lookup"><span data-stu-id="5f6de-143">You now have the binaries that you need to automate the setup of the Mobility service by using Automation DSC.</span></span>

### <a name="passphrase"></a><span data-ttu-id="5f6de-144">Hozzáférési kód</span><span class="sxs-lookup"><span data-stu-id="5f6de-144">Passphrase</span></span>
<span data-ttu-id="5f6de-145">Ezután meg kell határoznia, ahová a tömörített mappából.</span><span class="sxs-lookup"><span data-stu-id="5f6de-145">Next, you need to determine where you want to place this zipped folder.</span></span> <span data-ttu-id="5f6de-146">Egy Azure storage-fiók is használhatja, ahogy újabb, a jelszót, amelyet a telepítő kell tárolni.</span><span class="sxs-lookup"><span data-stu-id="5f6de-146">You can use an Azure storage account, as shown later, to store the passphrase that you need for the setup.</span></span> <span data-ttu-id="5f6de-147">Az ügynök ezután regisztrálja az a felügyeleti kiszolgáló, a folyamat részeként.</span><span class="sxs-lookup"><span data-stu-id="5f6de-147">The agent will then register with the management server as part of the process.</span></span>

<span data-ttu-id="5f6de-148">A jelszót, amelyet a felügyeleti kiszolgáló telepítésekor portáltól passphrase.txt szövegfájlba menthető.</span><span class="sxs-lookup"><span data-stu-id="5f6de-148">The passphrase that you got when you deployed the management server can be saved to a text file as passphrase.txt.</span></span>

<span data-ttu-id="5f6de-149">Az Azure storage-fiók egy dedikált tárolójára tegyen mind a tömörített mappából, és a jelszót.</span><span class="sxs-lookup"><span data-stu-id="5f6de-149">Place both the zipped folder and the passphrase in a dedicated container in the Azure storage account.</span></span>

![Mappa helye](./media/site-recovery-automate-mobilitysevice-install/folder-and-passphrase-location.png)

<span data-ttu-id="5f6de-151">Ha szeretné megtartani ezeket a fájlokat egy megosztást a hálózaton, megteheti.</span><span class="sxs-lookup"><span data-stu-id="5f6de-151">If you prefer to keep these files on a share on your network, you can do so.</span></span> <span data-ttu-id="5f6de-152">Csak akkor kell győződjön meg arról, hogy az Ön által használt később DSC erőforrás hozzáfér, és a telepítés és a jelszót.</span><span class="sxs-lookup"><span data-stu-id="5f6de-152">You just need to ensure that the DSC resource that you will be using later has access and can get the setup and passphrase.</span></span>

## <a name="step-2-create-the-dsc-configuration"></a><span data-ttu-id="5f6de-153">2. lépés: A DSC-konfiguráció létrehozása</span><span class="sxs-lookup"><span data-stu-id="5f6de-153">Step 2: Create the DSC configuration</span></span>
<span data-ttu-id="5f6de-154">A telepítő WMF 5.0 függ.</span><span class="sxs-lookup"><span data-stu-id="5f6de-154">The setup depends on WMF 5.0.</span></span> <span data-ttu-id="5f6de-155">A gép sikeresen alkalmazta a konfiguráció keresztül Automation DSC WMF 5.0 kell elhelyezkednie.</span><span class="sxs-lookup"><span data-stu-id="5f6de-155">For the machine to successfully apply the configuration through Automation DSC, WMF 5.0 needs to be present.</span></span>

<span data-ttu-id="5f6de-156">A környezetben használja a következő példa DSC-konfiguráció:</span><span class="sxs-lookup"><span data-stu-id="5f6de-156">The environment uses the following example DSC configuration:</span></span>

```powershell
configuration ASRMobilityService {

    $RemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    $RemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    $RemoteAzureAgent = 'http://go.microsoft.com/fwlink/p/?LinkId=394789'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node localhost {

        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
```
<span data-ttu-id="5f6de-157">A konfigurációs rendszer tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="5f6de-157">The configuration will do the following:</span></span>

* <span data-ttu-id="5f6de-158">A változók jelzi a konfigurációs Honnan szerezhetők be a bináris fájlok a mobilitási szolgáltatás és az Azure Virtuálisgép-ügynök, honnan szerezhetők be a jelszót, és a kimeneti tárolási helyét.</span><span class="sxs-lookup"><span data-stu-id="5f6de-158">The variables will tell the configuration where to get the binaries for the Mobility service and the Azure VM agent, where to get the passphrase, and where to store the output.</span></span>
* <span data-ttu-id="5f6de-159">A konfigurációs xPSDesiredStateConfiguration DSC erőforrás importálja, hogy használható `xRemoteFile` töltse le a fájlokat a tárházból.</span><span class="sxs-lookup"><span data-stu-id="5f6de-159">The configuration will import the xPSDesiredStateConfiguration DSC resource, so that you can use `xRemoteFile` to download the files from the repository.</span></span>
* <span data-ttu-id="5f6de-160">A konfigurációs hoz létre egy könyvtárat, ahová a bináris fájlok tárolására.</span><span class="sxs-lookup"><span data-stu-id="5f6de-160">The configuration will create a directory where you want to store the binaries.</span></span>
* <span data-ttu-id="5f6de-161">Az archív erőforrás lesz csomagolja ki a fájlokat a tömörített mappából.</span><span class="sxs-lookup"><span data-stu-id="5f6de-161">The archive resource will extract the files from the zipped folder.</span></span>
* <span data-ttu-id="5f6de-162">A csomag telepítési erőforrás a UNIFIEDAGENT a telepíti a mobilitási szolgáltatást. A megadott argumentumokkal telepítő exe-fájl.</span><span class="sxs-lookup"><span data-stu-id="5f6de-162">The package Install resource will install the Mobility service from the UNIFIEDAGENT.EXE installer with the specific arguments.</span></span> <span data-ttu-id="5f6de-163">(A változókat, amelyek az argumentumok összeállítani a környezetnek megfelelően módosítani kell.)</span><span class="sxs-lookup"><span data-stu-id="5f6de-163">(The variables that construct the arguments need to be changed to reflect your environment.)</span></span>
* <span data-ttu-id="5f6de-164">A csomag AzureAgent erőforrás telepíti az Azure Virtuálisgép-ügynök, ez az ajánlott minden Azure-ban futó virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="5f6de-164">The package AzureAgent resource will install the Azure VM agent, which is recommended on every VM that runs in Azure.</span></span> <span data-ttu-id="5f6de-165">Az Azure Virtuálisgép-ügynök is lehetővé teszi bővítmények hozzáadása a virtuális gép a feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="5f6de-165">The Azure VM agent also makes it possible to add extensions to the VM after failover.</span></span>
* <span data-ttu-id="5f6de-166">A szolgáltatás vagy erőforrást biztosítja, hogy a kapcsolódó mobilitási szolgáltatás és az Azure-szolgáltatások mindig futnak.</span><span class="sxs-lookup"><span data-stu-id="5f6de-166">The service resource or resources will ensure that the related Mobility services and the Azure services are always running.</span></span>

<span data-ttu-id="5f6de-167">Mentse a konfigurációt **ASRMobilityService**.</span><span class="sxs-lookup"><span data-stu-id="5f6de-167">Save the configuration as **ASRMobilityService**.</span></span>

> [!NOTE]
> <span data-ttu-id="5f6de-168">Ne felejtse el cserélje le a CSIP a konfiguráció megfelelően a tényleges felügyeleti kiszolgálót, hogy az ügynök megfelelően csatlakoztatva, és a helyes jelszót fogja használni.</span><span class="sxs-lookup"><span data-stu-id="5f6de-168">Remember to replace the CSIP in your configuration to reflect the actual management server, so that the agent will be connected correctly and will use the correct passphrase.</span></span>
>
>

## <a name="step-3-upload-to-automation-dsc"></a><span data-ttu-id="5f6de-169">3. lépés: Töltse fel az Automation DSC</span><span class="sxs-lookup"><span data-stu-id="5f6de-169">Step 3: Upload to Automation DSC</span></span>
<span data-ttu-id="5f6de-170">A DSC-konfiguráció végzett importálni fogja a szükséges DSC erőforrásmodul (xPSDesiredStateConfiguration), mert kell importálni az automatizálás modult, mielőtt feltölti a DSC-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="5f6de-170">Because the DSC configuration that you made will import a required DSC resource module (xPSDesiredStateConfiguration), you need to import that module in Automation before you upload the DSC configuration.</span></span>

<span data-ttu-id="5f6de-171">Jelentkezzen be az Automation-fiók, keresse meg a **eszközök** > **modulok**, és kattintson a **Tallózás gyűjtemény**.</span><span class="sxs-lookup"><span data-stu-id="5f6de-171">Sign in to your Automation account, browse to **Assets** > **Modules**, and click **Browse Gallery**.</span></span>

<span data-ttu-id="5f6de-172">Itt kereshet a modult, és importálja a fiókjába.</span><span class="sxs-lookup"><span data-stu-id="5f6de-172">Here you can search for the module and import it to your account.</span></span>

![Modul importálása](./media/site-recovery-automate-mobilitysevice-install/search-and-import-module.png)

<span data-ttu-id="5f6de-174">Ha ezzel végzett, a számítógépen nyissa meg az Azure Resource Manager-modulok telepítése esetében, és folytassa az újonnan létrehozott DSC-konfiguráció importálásakor.</span><span class="sxs-lookup"><span data-stu-id="5f6de-174">When you finish this, go to your machine where you have the Azure Resource Manager modules installed and proceed to import the newly created DSC configuration.</span></span>

### <a name="import-cmdlets"></a><span data-ttu-id="5f6de-175">Importálási parancsmagokat</span><span class="sxs-lookup"><span data-stu-id="5f6de-175">Import cmdlets</span></span>
<span data-ttu-id="5f6de-176">A PowerShellben jelentkezzen be az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="5f6de-176">In PowerShell, sign in to your Azure subscription.</span></span> <span data-ttu-id="5f6de-177">Módosítsa a parancsmagok segítségével a környezetnek megfelelően, és rögzítheti az automatizálási fiók adatait egy változóban:</span><span class="sxs-lookup"><span data-stu-id="5f6de-177">Modify the cmdlets to reflect your environment and capture your Automation account information in a variable:</span></span>

```powershell
$AAAccount = Get-AzureRmAutomationAccount -ResourceGroupName 'KNOMS' -Name 'KNOMSAA'
```

<span data-ttu-id="5f6de-178">Automation DSC konfigurációját feltölteni a következő parancsmag használatával:</span><span class="sxs-lookup"><span data-stu-id="5f6de-178">Upload the configuration to Automation DSC by using the following cmdlet:</span></span>

```powershell
$ImportArgs = @{
    SourcePath = 'C:\ASR\ASRMobilityService.ps1'
    Published = $true
    Description = 'DSC Config for Mobility Service'
}
$AAAccount | Import-AzureRmAutomationDscConfiguration @ImportArgs
```

### <a name="compile-the-configuration-in-automation-dsc"></a><span data-ttu-id="5f6de-179">A konfiguráció az Automation DSC-fordítási</span><span class="sxs-lookup"><span data-stu-id="5f6de-179">Compile the configuration in Automation DSC</span></span>
<span data-ttu-id="5f6de-180">Ezt követően kell a konfiguráció az Automation DSC-fordítási, hogy regisztrálja azt csomópontok megkezdése.</span><span class="sxs-lookup"><span data-stu-id="5f6de-180">Next, you need to compile the configuration in Automation DSC, so that you can start to register nodes to it.</span></span> <span data-ttu-id="5f6de-181">Érhetők el, amely a következő parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="5f6de-181">You achieve that by running the following cmdlet:</span></span>

```powershell
$AAAccount | Start-AzureRmAutomationDscCompilationJob -ConfigurationName ASRMobilityService
```

<span data-ttu-id="5f6de-182">Ez is igénybe vehet néhány percet, mert a konfiguráció az üzemeltett szolgáltatásban, DSC lekérési alapvetően telepít.</span><span class="sxs-lookup"><span data-stu-id="5f6de-182">This can take a few minutes, because you're basically deploying the configuration to the hosted DSC pull service.</span></span>

<span data-ttu-id="5f6de-183">Miután a konfiguráció fordítása le az adatok PowerShell (Get-AzureRmAutomationDscCompilationJob) használatával, vagy a a [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="5f6de-183">After you compile the configuration, you can retrieve the job information by using PowerShell (Get-AzureRmAutomationDscCompilationJob) or by using the [Azure portal](https://portal.azure.com/).</span></span>

![Feladat beolvasása](./media/site-recovery-automate-mobilitysevice-install/retrieve-job.png)

<span data-ttu-id="5f6de-185">Most már sikeresen közzé, és feltölti a DSC-konfiguráció Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="5f6de-185">You have now successfully published and uploaded your DSC configuration to Automation DSC.</span></span>

## <a name="step-4-onboard-machines-to-automation-dsc"></a><span data-ttu-id="5f6de-186">4. lépés: A bevezetni gépek Automation DSC</span><span class="sxs-lookup"><span data-stu-id="5f6de-186">Step 4: Onboard machines to Automation DSC</span></span>
> [!NOTE]
> <span data-ttu-id="5f6de-187">Ez a forgatókönyv befejezése előfeltételeit egyike, hogy a Windows-alapú gépek WMF legújabb verziójára frissít.</span><span class="sxs-lookup"><span data-stu-id="5f6de-187">One of the prerequisites for completing this scenario is that your Windows machines are updated with the latest version of WMF.</span></span> <span data-ttu-id="5f6de-188">Töltse le, és a rendszernek a megfelelő verziót telepítse a [letöltőközpontból](https://www.microsoft.com/download/details.aspx?id=50395).</span><span class="sxs-lookup"><span data-stu-id="5f6de-188">You can download and install the correct version for your platform from the [Download Center](https://www.microsoft.com/download/details.aspx?id=50395).</span></span>
>
>

<span data-ttu-id="5f6de-189">Egy metaconfig most létrehozza a DSC, akkor a csomópontra érvényes lesz.</span><span class="sxs-lookup"><span data-stu-id="5f6de-189">You will now create a metaconfig for DSC that you will apply to your nodes.</span></span> <span data-ttu-id="5f6de-190">A sikeres, szeretné beolvasni a végponti URL-cím és az elsődleges kulcsát a kijelölt Automation-fiók az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="5f6de-190">To succeed with this, you need to retrieve the endpoint URL and the primary key for your selected Automation account in Azure.</span></span> <span data-ttu-id="5f6de-191">Ezek az értékek alapján is megtalálhatja **kulcsok** a a **összes beállítás** az Automation-fiók paneljén.</span><span class="sxs-lookup"><span data-stu-id="5f6de-191">You can find these values under **Keys** on the **All settings** blade for the Automation account.</span></span>

![Értékek](./media/site-recovery-automate-mobilitysevice-install/key-values.png)

<span data-ttu-id="5f6de-193">Ebben a példában a Site Recovery segítségével védeni kívánt fizikai Windows Server 2012 R2-kiszolgálóval rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="5f6de-193">In this example, you have a Windows Server 2012 R2 physical server that you want to protect by using Site Recovery.</span></span>

### <a name="check-for-any-pending-file-rename-operations-in-the-registry"></a><span data-ttu-id="5f6de-194">Ellenőrizze az összes függőben lévő fájlműveletek nevezze át a beállításjegyzékben</span><span class="sxs-lookup"><span data-stu-id="5f6de-194">Check for any pending file rename operations in the registry</span></span>
<span data-ttu-id="5f6de-195">A kiszolgáló társítja a Automation DSC végponthoz megkezdése előtt javasoljuk, hogy ellenőrizze az összes függőben lévő fájlműveletek nevezze át a beállításjegyzékben.</span><span class="sxs-lookup"><span data-stu-id="5f6de-195">Before you start to associate the server with the Automation DSC endpoint, we recommend that you check for any pending file rename operations in the registry.</span></span> <span data-ttu-id="5f6de-196">A telepítő előfordulhat, hogy tiltják a befejezési miatt függőben lévő újraindítás.</span><span class="sxs-lookup"><span data-stu-id="5f6de-196">They might prohibit the setup from finishing due to a pending reboot.</span></span>

<span data-ttu-id="5f6de-197">Futtassa az alábbi parancsmagot, ellenőrizze, hogy a kiszolgálón nincs függőben lévő újraindítás:</span><span class="sxs-lookup"><span data-stu-id="5f6de-197">Run the following cmdlet to verify that there’s no pending reboot on the server:</span></span>

```powershell
Get-ItemProperty 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\' | Select-Object -Property PendingFileRenameOperations
```
<span data-ttu-id="5f6de-198">Ez azt mutatja, üres, ha az OK gombra a folytatáshoz áll.</span><span class="sxs-lookup"><span data-stu-id="5f6de-198">If this shows empty, you are OK to proceed.</span></span> <span data-ttu-id="5f6de-199">Ha nem, eleget kell tennie a karbantartási időszak alatt a kiszolgáló újraindításával.</span><span class="sxs-lookup"><span data-stu-id="5f6de-199">If not, you should address this by rebooting the server during a maintenance window.</span></span>

<span data-ttu-id="5f6de-200">A konfiguráció alkalmazása a kiszolgálón, indítsa el a PowerShell integrált parancsfájlkezelési környezet (ISE), és futtassa a következő parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="5f6de-200">To apply the configuration on the server, start the PowerShell Integrated Scripting Environment (ISE) and run the following script.</span></span> <span data-ttu-id="5f6de-201">Ez a lényegében egy DSC helyi konfiguráció, amely arra utasítja a helyi Configuration Manager motor regisztrálása az Automation DSC szolgáltatásban, és a megadott konfigurációs (ASRMobilityService.localhost) beolvasása.</span><span class="sxs-lookup"><span data-stu-id="5f6de-201">This is essentially a DSC local configuration that will instruct the Local Configuration Manager engine to register with the Automation DSC service and retrieve the specific configuration (ASRMobilityService.localhost).</span></span>

```powershell
[DSCLocalConfigurationManager()]
configuration metaconfig {
    param (
        $URL,
        $Key
    )
    node localhost {
        Settings {
            RefreshFrequencyMins = '30'
            RebootNodeIfNeeded = $true
            RefreshMode = 'PULL'
            ActionAfterReboot = 'ContinueConfiguration'
            ConfigurationMode = 'ApplyAndMonitor'
            AllowModuleOverwrite = $true
        }

        ResourceRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }

        ConfigurationRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
            ConfigurationNames = 'ASRMobilityService.localhost'
        }

        ReportServerWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }
    }
}
metaconfig -URL 'https://we-agentservice-prod-1.azure-automation.net/accounts/<YOURAAAccountID>' -Key '<YOURAAAccountKey>'

Set-DscLocalConfigurationManager .\metaconfig -Force -Verbose
```

<span data-ttu-id="5f6de-202">Ezt a konfigurációt, akkor a helyi Configuration Manager motor az Automation DSC regisztrálja magát.</span><span class="sxs-lookup"><span data-stu-id="5f6de-202">This configuration will cause the Local Configuration Manager engine to register itself with Automation DSC.</span></span> <span data-ttu-id="5f6de-203">Azt is meghatározza a motor hogyan működjön, mit kell tenni, ha a konfigurációs eltéréseket (ApplyAndAutoCorrect), és hogyan azt kell a konfigurálás folytatásához Ha újraindításra szükség.</span><span class="sxs-lookup"><span data-stu-id="5f6de-203">It will also determine how the engine should operate, what it should do if there's a configuration drift (ApplyAndAutoCorrect), and how it should proceed with the configuration if a reboot is required.</span></span>

<span data-ttu-id="5f6de-204">Ez a parancsfájl futtatása után a csomópont az Automation DSC Szolgáltatásban regisztrálni kell kezdődnie.</span><span class="sxs-lookup"><span data-stu-id="5f6de-204">After you run this script, the node should start to register with Automation DSC.</span></span>

![Folyamatban van a csomópont regisztrációs](./media/site-recovery-automate-mobilitysevice-install/register-node.png)

<span data-ttu-id="5f6de-206">Ha vissza az Azure-portálon, láthatja, hogy az újonnan regisztrált csomópont már megjelent a portál.</span><span class="sxs-lookup"><span data-stu-id="5f6de-206">If you go back to the Azure portal, you can see that the newly registered node has now appeared in the portal.</span></span>

![A portál regisztrált csomópontja](./media/site-recovery-automate-mobilitysevice-install/registered-node.png)

<span data-ttu-id="5f6de-208">A kiszolgálón futtathatja a következő PowerShell-parancsmag segítségével győződjön meg arról, hogy a csomópont megfelelően van regisztrálva:</span><span class="sxs-lookup"><span data-stu-id="5f6de-208">On the server, you can run the following PowerShell cmdlet to verify that the node has been registered correctly:</span></span>

```powershell
Get-DscLocalConfigurationManager
```

<span data-ttu-id="5f6de-209">Miután a konfigurációs lekért, és a a kiszolgálóra alkalmazza, ezt a következő parancsmag futtatásával ellenőrizheti:</span><span class="sxs-lookup"><span data-stu-id="5f6de-209">After the configuration has been pulled and applied to the server, you can verify this by running the following cmdlet:</span></span>

```powershell
Get-DscConfigurationStatus
```

<span data-ttu-id="5f6de-210">A kimeneti jeleníti meg, hogy a kiszolgáló sikeresen hívja elő a konfigurációban:</span><span class="sxs-lookup"><span data-stu-id="5f6de-210">The output shows that the server has successfully pulled its configuration:</span></span>

![Kimenet](./media/site-recovery-automate-mobilitysevice-install/successful-config.png)

<span data-ttu-id="5f6de-212">Emellett a mobilitási szolgáltatás telepítő rendelkezik-e a saját naplóban, amely a következő *SystemDrive*\ProgramData\ASRSetupLogs.</span><span class="sxs-lookup"><span data-stu-id="5f6de-212">In addition, the Mobility service setup has its own log that can be found at *SystemDrive*\ProgramData\ASRSetupLogs.</span></span>

<span data-ttu-id="5f6de-213">Ennyi az egész.</span><span class="sxs-lookup"><span data-stu-id="5f6de-213">That’s it.</span></span> <span data-ttu-id="5f6de-214">Most már sikeresen telepített és regisztrált a mobilitási szolgáltatást a Site Recovery segítségével védeni kívánt számítógépen.</span><span class="sxs-lookup"><span data-stu-id="5f6de-214">You have now successfully deployed and registered the Mobility service on the machine that you want to protect by using Site Recovery.</span></span> <span data-ttu-id="5f6de-215">A DSC-ből fog győződjön meg arról, hogy a szükséges szolgáltatások mindig futnak.</span><span class="sxs-lookup"><span data-stu-id="5f6de-215">DSC will make sure that the required services are always running.</span></span>

![Sikeres telepítés](./media/site-recovery-automate-mobilitysevice-install/successful-install.png)

<span data-ttu-id="5f6de-217">Miután a felügyeleti kiszolgáló észleli a sikeres telepítés, konfigurálja a védelmet, és a gép replikációs engedélyezése a Site Recovery segítségével.</span><span class="sxs-lookup"><span data-stu-id="5f6de-217">After the management server detects the successful deployment, you can configure protection and enable replication on the machine by using Site Recovery.</span></span>

## <a name="use-dsc-in-disconnected-environments"></a><span data-ttu-id="5f6de-218">A DSC használata a kapcsolat nélküli környezetekben</span><span class="sxs-lookup"><span data-stu-id="5f6de-218">Use DSC in disconnected environments</span></span>
<span data-ttu-id="5f6de-219">Ha a gépek nem kapcsolódik az internethez, továbbra is támaszkodhat a DSC telepítheti és konfigurálhatja a mobilitási szolgáltatás a védeni kívánt munkaterhelések.</span><span class="sxs-lookup"><span data-stu-id="5f6de-219">If your machines aren’t connected to the Internet, you can still rely on DSC to deploy and configure the Mobility service on the workloads that you want to protect.</span></span>

<span data-ttu-id="5f6de-220">A saját DSC lekérési kiszolgálójával példányosítható lényegében arra, hogy Automation DSC ugyanazokat a képességeket a környezetben.</span><span class="sxs-lookup"><span data-stu-id="5f6de-220">You can instantiate your own DSC pull server in your environment to essentially provide the same capabilities that you get from Automation DSC.</span></span> <span data-ttu-id="5f6de-221">Ez azt jelenti, hogy az ügyfelek fogja lekérni a konfigurációs (regisztráció) után a DSC-végponthoz.</span><span class="sxs-lookup"><span data-stu-id="5f6de-221">That is, the clients will pull the configuration (after it's registered) to the DSC endpoint.</span></span> <span data-ttu-id="5f6de-222">Azonban egy másik lehetőség egy manuális leküldése a DSC-konfiguráció a gépek helyileg vagy távolról.</span><span class="sxs-lookup"><span data-stu-id="5f6de-222">However, another option is to manually push the DSC configuration to your machines, either locally or remotely.</span></span>

<span data-ttu-id="5f6de-223">Vegye figyelembe, hogy ebben a példában egy hozzáadott paraméter, a számítógép neveként.</span><span class="sxs-lookup"><span data-stu-id="5f6de-223">Note that in this example, there's an added parameter for the computer name.</span></span> <span data-ttu-id="5f6de-224">A távoli fájlok most már található távoli megosztásra mutat, amely érhető el, a védeni kívánt gépek.</span><span class="sxs-lookup"><span data-stu-id="5f6de-224">The remote files are now located on a remote share that should be accessible by the machines that you want to protect.</span></span> <span data-ttu-id="5f6de-225">A parancsfájl végén ír elő a konfigurációs, és ezután elindítja a DSC-konfiguráció alkalmazása a célszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="5f6de-225">The end of the script enacts the configuration and then starts to apply the DSC configuration to the target computer.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="5f6de-226">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5f6de-226">Prerequisites</span></span>
<span data-ttu-id="5f6de-227">Győződjön meg arról, hogy a xPSDesiredStateConfiguration PowerShell-modul telepítve van-e.</span><span class="sxs-lookup"><span data-stu-id="5f6de-227">Make sure that the xPSDesiredStateConfiguration PowerShell module is installed.</span></span> <span data-ttu-id="5f6de-228">Windows-alapú gépek WMF 5.0 futtató a célszámítógépen a következő parancsmag futtatásával telepítheti a xPSDesiredStateConfiguration modul:</span><span class="sxs-lookup"><span data-stu-id="5f6de-228">For Windows machines where WMF 5.0 is installed, you can install the xPSDesiredStateConfiguration module by running the following cmdlet on the target machines:</span></span>

```powershell
Find-Module -Name xPSDesiredStateConfiguration | Install-Module
```

<span data-ttu-id="5f6de-229">Letöltheti, és mentse a modul, abban az esetben kell terjesztenie windowsos gépeken, amelyek WMF 4.0-s verzióját.</span><span class="sxs-lookup"><span data-stu-id="5f6de-229">You can also download and save the module in case you need to distribute it to Windows machines that have WMF 4.0.</span></span> <span data-ttu-id="5f6de-230">Futtassa ezt a parancsmagot egy számítógépen, ha jelen-e PowerShellGet (WMF 5.0):</span><span class="sxs-lookup"><span data-stu-id="5f6de-230">Run this cmdlet on a machine where PowerShellGet (WMF 5.0) is present:</span></span>

```powershell
Save-Module -Name xPSDesiredStateConfiguration -Path <location>
```

<span data-ttu-id="5f6de-231">Is WMF 4.0-s, győződjön meg arról, hogy a [Windows 8.1 update KB2883200](https://www.microsoft.com/download/details.aspx?id=40749) számítógépeken telepítve van.</span><span class="sxs-lookup"><span data-stu-id="5f6de-231">Also for WMF 4.0, ensure that the [Windows 8.1 update KB2883200](https://www.microsoft.com/download/details.aspx?id=40749) is installed on the machines.</span></span>

<span data-ttu-id="5f6de-232">A következő konfigurációs windowsos gépeken, amelyek WMF 5.0 és WMF 4.0 lehet leküldeni:</span><span class="sxs-lookup"><span data-stu-id="5f6de-232">The following configuration can be pushed to Windows machines that have WMF 5.0 and WMF 4.0:</span></span>

```powershell
configuration ASRMobilityService {
    param (
        [Parameter(Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [System.String] $ComputerName
    )

    $RemoteFile = '\\myfileserver\share\asr.zip'
    $RemotePassphrase = '\\myfileserver\share\passphrase.txt'
    $RemoteAzureAgent = '\\myfileserver\share\AzureVmAgent.msi'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node $ComputerName {      
        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
ASRMobilityService -ComputerName 'MyTargetComputerName'

Start-DscConfiguration .\ASRMobilityService -Wait -Force -Verbose
```

<span data-ttu-id="5f6de-233">Ha szeretné példányosítani saját DSC lekérési kiszolgálójával, hogy utánozzák funkciója, amelyek Automation DSC lekérheti a vállalati hálózaton, lásd: [DSC lekérési webkiszolgáló beállítása](https://msdn.microsoft.com/powershell/dsc/pullserver?f=255&MSPPError=-2147217396).</span><span class="sxs-lookup"><span data-stu-id="5f6de-233">If you want to instantiate your own DSC pull server on your corporate network to mimic the capabilities that you can get from Automation DSC, see [Setting up a DSC web pull server](https://msdn.microsoft.com/powershell/dsc/pullserver?f=255&MSPPError=-2147217396).</span></span>

## <a name="optional-deploy-a-dsc-configuration-by-using-an-azure-resource-manager-template"></a><span data-ttu-id="5f6de-234">Választható lehetőség: Telepítését a DSC-konfiguráció Azure Resource Manager-sablonnal</span><span class="sxs-lookup"><span data-stu-id="5f6de-234">Optional: Deploy a DSC configuration by using an Azure Resource Manager template</span></span>
<span data-ttu-id="5f6de-235">Ez a cikk fordította hogyan hozhatja létre saját DSC-konfiguráció automatikusan telepíteni a mobilitási szolgáltatás és az Azure Virtuálisgép-ügynök –, és győződjön meg arról, hogy a védeni kívánt gépeken futnak.</span><span class="sxs-lookup"><span data-stu-id="5f6de-235">This article has focused on how you can create your own DSC configuration to automatically deploy the Mobility service and the Azure VM Agent--and ensure that they are running on the machines that you want to protect.</span></span> <span data-ttu-id="5f6de-236">Azt is, amely egy új vagy meglévő Azure Automation-fiók telepíti ezen DSC-konfiguráció Azure Resource Manager-sablonokban.</span><span class="sxs-lookup"><span data-stu-id="5f6de-236">We also have an Azure Resource Manager template that will deploy this DSC configuration to a new or existing Azure Automation account.</span></span> <span data-ttu-id="5f6de-237">A sablon bemeneti paraméterek segítségével Automation eszközöket, mely tartalmazni fogja a környezetnek a változókat létrehozni.</span><span class="sxs-lookup"><span data-stu-id="5f6de-237">The template will use input parameters to create Automation assets that will contain the variables for your environment.</span></span>

<span data-ttu-id="5f6de-238">A sablon telepítése után egyszerűen hivatkozhat előkészítésére jelen útmutató 4. lépés a gépek.</span><span class="sxs-lookup"><span data-stu-id="5f6de-238">After you deploy the template, you can simply refer to step 4 in this guide to onboard your machines.</span></span>

<span data-ttu-id="5f6de-239">A sablon fog tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="5f6de-239">The template will do the following:</span></span>

1. <span data-ttu-id="5f6de-240">Használjon meglévő Automation-fiókot, vagy hozzon létre egy újat</span><span class="sxs-lookup"><span data-stu-id="5f6de-240">Use an existing Automation account or create a new one</span></span>
2. <span data-ttu-id="5f6de-241">A bemeneti paraméterek igénybe, hogy:</span><span class="sxs-lookup"><span data-stu-id="5f6de-241">Take input parameters for:</span></span>
   * <span data-ttu-id="5f6de-242">ASRRemoteFile--a mobilitási szolgáltatás beállítása tároló helyét</span><span class="sxs-lookup"><span data-stu-id="5f6de-242">ASRRemoteFile--the location where you have stored the Mobility service setup</span></span>
   * <span data-ttu-id="5f6de-243">ASRPassphrase--a passphrase.txt fájl tároló helyét</span><span class="sxs-lookup"><span data-stu-id="5f6de-243">ASRPassphrase--the location where you have stored the passphrase.txt file</span></span>
   * <span data-ttu-id="5f6de-244">ASRCSEndpoint – a felügyeleti kiszolgáló IP-címe</span><span class="sxs-lookup"><span data-stu-id="5f6de-244">ASRCSEndpoint--the IP address of your management server</span></span>
3. <span data-ttu-id="5f6de-245">A xPSDesiredStateConfiguration PowerShell modul importálása</span><span class="sxs-lookup"><span data-stu-id="5f6de-245">Import the xPSDesiredStateConfiguration PowerShell module</span></span>
4. <span data-ttu-id="5f6de-246">Hozzon létre, és a DSC-konfiguráció-fordítási</span><span class="sxs-lookup"><span data-stu-id="5f6de-246">Create and compile the DSC configuration</span></span>

<span data-ttu-id="5f6de-247">Az előző lépések a megfelelő sorrendben történik meg, hogy a bevezetése az ügyfélgépek is elindítható.</span><span class="sxs-lookup"><span data-stu-id="5f6de-247">All the preceding steps will happen in the right order, so that you can start onboarding your machines for protection.</span></span>

<span data-ttu-id="5f6de-248">A sablon üzembe helyezés esetén utasításokkal található [GitHub](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/DSC).</span><span class="sxs-lookup"><span data-stu-id="5f6de-248">The template, with instructions for deployment, is located on [GitHub](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/DSC).</span></span>

<span data-ttu-id="5f6de-249">A sablon üzembe helyezése a PowerShell használatával:</span><span class="sxs-lookup"><span data-stu-id="5f6de-249">Deploy the template by using PowerShell:</span></span>

```powershell
$RGDeployArgs = @{
    Name = 'DSC3'
    ResourceGroupName = 'KNOMS'
    TemplateFile = 'https://raw.githubusercontent.com/krnese/AzureDeploy/master/OMS/MSOMS/DSC/azuredeploy.json'
    OMSAutomationAccountName = 'KNOMSAA'
    ASRRemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    ASRRemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    ASRCSEndpoint = '10.0.0.115'
    DSCJobGuid = [System.Guid]::NewGuid().ToString()
}
New-AzureRmResourceGroupDeployment @RGDeployArgs -Verbose
```

## <a name="next-steps"></a><span data-ttu-id="5f6de-250">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5f6de-250">Next steps</span></span>
<span data-ttu-id="5f6de-251">A mobilitási szolgáltatás ügynökök telepítése után is [engedélyezze a replikálást](site-recovery-vmware-to-azure.md) a virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="5f6de-251">After you deploy the Mobility service agents, you can [enable replication](site-recovery-vmware-to-azure.md) for the virtual machines.</span></span>
