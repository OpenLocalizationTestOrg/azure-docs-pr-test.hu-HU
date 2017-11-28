---
title: "aaaDeploy hello Site Recovery mobilitási szolgáltatás az Azure Automation DSC Szolgáltatásban |} Microsoft Docs"
description: "Ismerteti, hogyan toouse Azure Automation DSC tooautomatically üzembe hello Azure Site Recovery mobilitási szolgáltatás és az Azure-ügynököt a VMware virtuális gép és a fizikai kiszolgáló replikációs tooAzure"
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
ms.openlocfilehash: 52cdd13ceb61718a21137180c55db86919af5929
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-hello-mobility-service-with-azure-automation-dsc-for-replication-of-vm"></a><span data-ttu-id="15e1c-103">Hello mobilitási szolgáltatás az Azure Automation DSC Szolgáltatásban a replikáció a virtuális gép üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="15e1c-103">Deploy hello Mobility service with Azure Automation DSC for replication of VM</span></span>
<span data-ttu-id="15e1c-104">Az Operations Management Suite azt biztosít egy átfogó biztonsági mentési és vész-helyreállítási megoldást, amely az üzletmenet folytonosságát biztosító terve részeként is használhatja.</span><span class="sxs-lookup"><span data-stu-id="15e1c-104">In Operations Management Suite, we provide you with a comprehensive backup and disaster recovery solution that you can use as part of your business continuity plan.</span></span>

<span data-ttu-id="15e1c-105">A Hyper-V együtt út indította Hyper-V-replika használatával.</span><span class="sxs-lookup"><span data-stu-id="15e1c-105">We started this journey together with Hyper-V by using Hyper-V Replica.</span></span> <span data-ttu-id="15e1c-106">De tudunk bővített toosupport heterogén telepítő mert azok felhők ügyfelek több hipervizorok és platformok.</span><span class="sxs-lookup"><span data-stu-id="15e1c-106">But we have expanded toosupport a heterogeneous setup because customers have multiple hypervisors and platforms in their clouds.</span></span>

<span data-ttu-id="15e1c-107">Ha futtat VMware munkaterhelések és/vagy fizikai kiszolgálók ma, egy felügyeleti kiszolgálót futtatja összes hello Azure Site Recovery-összetevők a környezet toohandle hello kommunikáció és replikálás az Azure-a cél Azure esetén.</span><span class="sxs-lookup"><span data-stu-id="15e1c-107">If you are running VMware workloads and/or physical servers today, a management server runs all of hello Azure Site Recovery components in your environment toohandle hello communication and data replication with Azure, when Azure is your destination.</span></span>

## <a name="deploy-hello-site-recovery-mobility-service-by-using-automation-dsc"></a><span data-ttu-id="15e1c-108">Hello Site Recovery mobilitási szolgáltatás telepítését az Automation DSC</span><span class="sxs-lookup"><span data-stu-id="15e1c-108">Deploy hello Site Recovery Mobility service by using Automation DSC</span></span>
<span data-ttu-id="15e1c-109">Kezdjük gyors részletes információkat a felügyeleti kiszolgáló funkciója végrehajtásával.</span><span class="sxs-lookup"><span data-stu-id="15e1c-109">Let's start by doing a quick breakdown of what this management server does.</span></span>

<span data-ttu-id="15e1c-110">hello felügyeleti kiszolgáló számos kiszolgálói szerepkört futtatja.</span><span class="sxs-lookup"><span data-stu-id="15e1c-110">hello management server runs several server roles.</span></span> <span data-ttu-id="15e1c-111">Ezek a szerepkörök egyik *konfigurációs*, amely kommunikáció koordinálását, és kezeli az adatokat replikálás és helyreállítási folyamatok.</span><span class="sxs-lookup"><span data-stu-id="15e1c-111">One of these roles is *configuration*, which coordinates communication and manages data replication and recovery processes.</span></span>

<span data-ttu-id="15e1c-112">Ezenkívül hello *folyamat* szerepkör replikációs átjáróként.</span><span class="sxs-lookup"><span data-stu-id="15e1c-112">In addition, hello *process* role acts as a replication gateway.</span></span> <span data-ttu-id="15e1c-113">Ezt a szerepkört kap replikációs adatokat védett forrásgépek, gyorsítótárazás, tömörítés és titkosítás segítségével optimalizálja őket, és visszaküldi az tooan Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="15e1c-113">This role receives replication data from protected source machines, optimizes it with caching, compression, and encryption, and then sends it tooan Azure storage account.</span></span> <span data-ttu-id="15e1c-114">Hello funkciók hello folyamat szerepkör egyik is hello mobilitási szolgáltatás tooprotected gépek toopush telepítését, és a VMware virtuális gépek automatikus észlelését.</span><span class="sxs-lookup"><span data-stu-id="15e1c-114">One of hello functions for hello process role is also toopush installation of hello Mobility service tooprotected machines and perform automatic discovery of VMware VMs.</span></span>

<span data-ttu-id="15e1c-115">Ha a feladat-visszavétel az Azure-ból, hello *fő* szerepkör hello replikációs adatok kezelésére, ez a művelet részeként.</span><span class="sxs-lookup"><span data-stu-id="15e1c-115">If there's a failback from Azure, hello *master target* role will handle hello replication data as part of this operation.</span></span>

<span data-ttu-id="15e1c-116">Hello védett gépek, a Microsoft hello támaszkodjon *mobilitásiszolgáltatás*.</span><span class="sxs-lookup"><span data-stu-id="15e1c-116">For hello protected machines, we rely on hello *Mobility service*.</span></span> <span data-ttu-id="15e1c-117">Ez az összetevő telepített tooevery machine (VMware virtuális gép vagy fizikai kiszolgálón), amelyet az tooreplicate tooAzure áll.</span><span class="sxs-lookup"><span data-stu-id="15e1c-117">This component is deployed tooevery machine (VMware VM or physical server) that you want tooreplicate tooAzure.</span></span> <span data-ttu-id="15e1c-118">Hello gépen végbemenő adatírásokat, és továbbítja őket toohello felügyeleti kiszolgáló (folyamat szerepkör).</span><span class="sxs-lookup"><span data-stu-id="15e1c-118">It captures data writes on hello machine and forwards them toohello management server (process role).</span></span>

<span data-ttu-id="15e1c-119">Üzleti folytonosság kezelése, esetén fontos toounderstand a munkaterhelések, az infrastruktúra és a hello összetevők szerepet játszanak.</span><span class="sxs-lookup"><span data-stu-id="15e1c-119">When you're dealing with business continuity, it's important toounderstand your workloads, your infrastructure, and hello components involved.</span></span> <span data-ttu-id="15e1c-120">A helyreállítási idő célkitűzése (RTO) és a helyreállítási időkorlát (RPO) hello követelményei majd megfelelhetnek.</span><span class="sxs-lookup"><span data-stu-id="15e1c-120">You can then meet hello requirements for your recovery time objective (RTO) and recovery point objective (RPO).</span></span> <span data-ttu-id="15e1c-121">Ebben a környezetben hello mobilitásiszolgáltatás a kulcs tooensuring a munkaterhelések védett módon teheti meg.</span><span class="sxs-lookup"><span data-stu-id="15e1c-121">In this context, hello Mobility service is key tooensuring that your workloads are protected as you would expect.</span></span>

<span data-ttu-id="15e1c-122">Igen, hogyan, egy optimalizált módon biztosíthatja, hogy rendelkezik-e súgó néhány Operations Management Suite összetevői egy megbízható védett telepítése?</span><span class="sxs-lookup"><span data-stu-id="15e1c-122">So how can you, in an optimized way, ensure that you have a reliable protected setup with help from some Operations Management Suite components?</span></span>

<span data-ttu-id="15e1c-123">Ez a cikk bemutatja, hogyan használhatók az Azure Automation szükséges konfiguráló (DSC), és a hely helyreállítását követően tooensure együttes, amelyek:</span><span class="sxs-lookup"><span data-stu-id="15e1c-123">This article provides an example of how you can use Azure Automation Desired State Configuration (DSC), together with Site Recovery, tooensure that:</span></span>

* <span data-ttu-id="15e1c-124">hello mobilitási szolgáltatás és Azure Virtuálisgép-ügynök a telepített toohello windowsos gépekre, amelyet az tooprotect.</span><span class="sxs-lookup"><span data-stu-id="15e1c-124">hello Mobility service and Azure VM agent are deployed toohello Windows machines that you want tooprotect.</span></span>
* <span data-ttu-id="15e1c-125">hello mobilitási szolgáltatás és az Azure Virtuálisgép-ügynök mindig futnak, amikor Azure hello replikációs cél.</span><span class="sxs-lookup"><span data-stu-id="15e1c-125">hello Mobility service and Azure VM agent are always running when Azure is hello replication target.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="15e1c-126">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="15e1c-126">Prerequisites</span></span>
* <span data-ttu-id="15e1c-127">A tárház toostore szükséges hello beállítása</span><span class="sxs-lookup"><span data-stu-id="15e1c-127">A repository toostore hello required setup</span></span>
* <span data-ttu-id="15e1c-128">A tárház toostore hello szükséges jelszót tooregister hello felügyeleti kiszolgálóval</span><span class="sxs-lookup"><span data-stu-id="15e1c-128">A repository toostore hello required passphrase tooregister with hello management server</span></span>

  > [!NOTE]
  > <span data-ttu-id="15e1c-129">Minden felügyeleti kiszolgáló létrejön egy egyedi jelszót.</span><span class="sxs-lookup"><span data-stu-id="15e1c-129">A unique passphrase is generated for each management server.</span></span> <span data-ttu-id="15e1c-130">Ha több felügyeleti kiszolgálót fog toodeploy, hogy tooensure, hogy helyes-e jelszót hello passphrase.txt fájl tárolja hello.</span><span class="sxs-lookup"><span data-stu-id="15e1c-130">If you are going toodeploy multiple management servers, you have tooensure that hello correct passphrase is stored in hello passphrase.txt file.</span></span>
  >
  >
* <span data-ttu-id="15e1c-131">A Windows Management Framework (WMF) telepítve, amelyet az tooenable (előfeltétele annak, hogy Automation DSC) védelemre hello gépen 5.0</span><span class="sxs-lookup"><span data-stu-id="15e1c-131">Windows Management Framework (WMF) 5.0 installed on hello machines that you want tooenable for protection (a requirement for Automation DSC)</span></span>

  > [!NOTE]
  > <span data-ttu-id="15e1c-132">Ha azt szeretné, hogy toouse DSC for Windows gépeken, amelyek WMF 4.0-s verziójával, című hello [DSC használata a kapcsolat nélküli környezetben](## Use DSC in disconnected environments).</span><span class="sxs-lookup"><span data-stu-id="15e1c-132">If you want toouse DSC for Windows machines that have WMF 4.0 installed, see hello section [Use DSC in disconnected environments](## Use DSC in disconnected environments).</span></span>
  

<span data-ttu-id="15e1c-133">hello mobilitásiszolgáltatás hello parancssor használatával is telepíthető, és több argumentumot fogad el.</span><span class="sxs-lookup"><span data-stu-id="15e1c-133">hello Mobility service can be installed through hello command line and accepts several arguments.</span></span> <span data-ttu-id="15e1c-134">Ezért kell toohave hello bináris fájlokat (ezt követően ezeket a telepítésből), és tárolja őket egy helyen, ahol helyreállíthatók a DSC-konfiguráció használatával.</span><span class="sxs-lookup"><span data-stu-id="15e1c-134">That’s why you need toohave hello binaries (after extracting them from your setup) and store them in a place where you can retrieve them by using a DSC configuration.</span></span>

## <a name="step-1-extract-binaries"></a><span data-ttu-id="15e1c-135">1. lépés: Kivonat bináris fájlok</span><span class="sxs-lookup"><span data-stu-id="15e1c-135">Step 1: Extract binaries</span></span>
1. <span data-ttu-id="15e1c-136">a telepítéshez szükséges tooextract hello fájlokat keresse meg a toohello címtár a felügyeleti kiszolgálón a következő:</span><span class="sxs-lookup"><span data-stu-id="15e1c-136">tooextract hello files that you need for this setup, browse toohello following directory on your management server:</span></span>

    <span data-ttu-id="15e1c-137">**\Microsoft azure hely Recovery\home\svsystems\pushinstallsvc\repository**</span><span class="sxs-lookup"><span data-stu-id="15e1c-137">**\Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**</span></span>

    <span data-ttu-id="15e1c-138">Ebben a mappában kell: nevű MSI-fájl</span><span class="sxs-lookup"><span data-stu-id="15e1c-138">In this folder, you should see an MSI file named:</span></span>

    <span data-ttu-id="15e1c-139">**Microsoft-ASR_UA_version_Windows_GA_date_Release.exe**</span><span class="sxs-lookup"><span data-stu-id="15e1c-139">**Microsoft-ASR_UA_version_Windows_GA_date_Release.exe**</span></span>

    <span data-ttu-id="15e1c-140">A következő parancs tooextract hello telepítő hello használata:</span><span class="sxs-lookup"><span data-stu-id="15e1c-140">Use hello following command tooextract hello installer:</span></span>

    <span data-ttu-id="15e1c-141">**.\Microsoft-ASR_UA_9.1.0.0_Windows_GA_02May2016_release.exe /q /x:C:\Users\Administrator\Desktop\Mobility_Service\Extract**</span><span class="sxs-lookup"><span data-stu-id="15e1c-141">**.\Microsoft-ASR_UA_9.1.0.0_Windows_GA_02May2016_release.exe /q /x:C:\Users\Administrator\Desktop\Mobility_Service\Extract**</span></span>
2. <span data-ttu-id="15e1c-142">Válassza ki az összes fájlt, és küldje el tooa tömörített mappa.</span><span class="sxs-lookup"><span data-stu-id="15e1c-142">Select all files and send them tooa compressed (zipped) folder.</span></span>

<span data-ttu-id="15e1c-143">Most már rendelkezik hello bináris fájlokat, hogy kell-e a mobilitási szolgáltatás hello tooautomate hello telepítése Automation DSC használatával.</span><span class="sxs-lookup"><span data-stu-id="15e1c-143">You now have hello binaries that you need tooautomate hello setup of hello Mobility service by using Automation DSC.</span></span>

### <a name="passphrase"></a><span data-ttu-id="15e1c-144">Hozzáférési kód</span><span class="sxs-lookup"><span data-stu-id="15e1c-144">Passphrase</span></span>
<span data-ttu-id="15e1c-145">A következő lépésben toodetermine kívánt tooplace a tömörített mappából.</span><span class="sxs-lookup"><span data-stu-id="15e1c-145">Next, you need toodetermine where you want tooplace this zipped folder.</span></span> <span data-ttu-id="15e1c-146">Egy Azure storage-fiók látható későbbi, toostore hello jelszót, amelyekre szüksége van az hello telepítő is használhatja.</span><span class="sxs-lookup"><span data-stu-id="15e1c-146">You can use an Azure storage account, as shown later, toostore hello passphrase that you need for hello setup.</span></span> <span data-ttu-id="15e1c-147">hello ügynök ezután regisztrálja az hello felügyeleti kiszolgáló hello folyamat részeként.</span><span class="sxs-lookup"><span data-stu-id="15e1c-147">hello agent will then register with hello management server as part of hello process.</span></span>

<span data-ttu-id="15e1c-148">hello jelszót hello felügyeleti kiszolgáló telepítésekor portáltól menthető tooa szövegfájl passphrase.txt.</span><span class="sxs-lookup"><span data-stu-id="15e1c-148">hello passphrase that you got when you deployed hello management server can be saved tooa text file as passphrase.txt.</span></span>

<span data-ttu-id="15e1c-149">Mind a hello tömörített mappából, és a hello jelszót tegyen egy dedikált tárolójára hello Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="15e1c-149">Place both hello zipped folder and hello passphrase in a dedicated container in hello Azure storage account.</span></span>

![Mappa helye](./media/site-recovery-automate-mobilitysevice-install/folder-and-passphrase-location.png)

<span data-ttu-id="15e1c-151">Ha jobban szeret tookeep ezeket a fájlokat egy megosztást a hálózaton, megteheti.</span><span class="sxs-lookup"><span data-stu-id="15e1c-151">If you prefer tookeep these files on a share on your network, you can do so.</span></span> <span data-ttu-id="15e1c-152">Egyszerűen tooensure, hogy az Ön által használt később hello DSC-erőforrás hozzáfér, és hello beállítása és a jelszót.</span><span class="sxs-lookup"><span data-stu-id="15e1c-152">You just need tooensure that hello DSC resource that you will be using later has access and can get hello setup and passphrase.</span></span>

## <a name="step-2-create-hello-dsc-configuration"></a><span data-ttu-id="15e1c-153">2. lépés: Hello DSC-konfiguráció létrehozása</span><span class="sxs-lookup"><span data-stu-id="15e1c-153">Step 2: Create hello DSC configuration</span></span>
<span data-ttu-id="15e1c-154">hello beállítása WMF 5.0 függ.</span><span class="sxs-lookup"><span data-stu-id="15e1c-154">hello setup depends on WMF 5.0.</span></span> <span data-ttu-id="15e1c-155">Hello gép toosuccessfully alkalmazni hello konfigurációs keresztül Automation DSC, WMF 5.0 toobe jelen van szüksége.</span><span class="sxs-lookup"><span data-stu-id="15e1c-155">For hello machine toosuccessfully apply hello configuration through Automation DSC, WMF 5.0 needs toobe present.</span></span>

<span data-ttu-id="15e1c-156">hello környezetben használja a következő példa DSC-konfiguráció hello:</span><span class="sxs-lookup"><span data-stu-id="15e1c-156">hello environment uses hello following example DSC configuration:</span></span>

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
<span data-ttu-id="15e1c-157">hello konfigurációs hajt végre a következő hello:</span><span class="sxs-lookup"><span data-stu-id="15e1c-157">hello configuration will do hello following:</span></span>

* <span data-ttu-id="15e1c-158">hello változók hello konfigurációs megtudhatja, hol tooget hello-e binárist hello mobilitási szolgáltatás és a hello Azure Virtuálisgép-ügynök, ha tooget hello jelszót, és ha toostore hello kimeneti.</span><span class="sxs-lookup"><span data-stu-id="15e1c-158">hello variables will tell hello configuration where tooget hello binaries for hello Mobility service and hello Azure VM agent, where tooget hello passphrase, and where toostore hello output.</span></span>
* <span data-ttu-id="15e1c-159">hello konfigurációs importálja a hello xPSDesiredStateConfiguration DSC erőforrás, így használhatja `xRemoteFile` toodownload hello fájlok adattárból hello.</span><span class="sxs-lookup"><span data-stu-id="15e1c-159">hello configuration will import hello xPSDesiredStateConfiguration DSC resource, so that you can use `xRemoteFile` toodownload hello files from hello repository.</span></span>
* <span data-ttu-id="15e1c-160">hello konfigurációs létrehoz egy könyvtárat, ahová toostore hello bináris fájljait.</span><span class="sxs-lookup"><span data-stu-id="15e1c-160">hello configuration will create a directory where you want toostore hello binaries.</span></span>
* <span data-ttu-id="15e1c-161">hello archív erőforrás hello tömörített mappából hello fájlt csomagolja ki.</span><span class="sxs-lookup"><span data-stu-id="15e1c-161">hello archive resource will extract hello files from hello zipped folder.</span></span>
* <span data-ttu-id="15e1c-162">hello csomag telepítési erőforrás hello mobilitásiszolgáltatás telepíthessenek hello UNIFIEDAGENT. A következő EXE telepítő hello megadott argumentumokkal.</span><span class="sxs-lookup"><span data-stu-id="15e1c-162">hello package Install resource will install hello Mobility service from hello UNIFIEDAGENT.EXE installer with hello specific arguments.</span></span> <span data-ttu-id="15e1c-163">(hello argumentumok összeállításához hello változók kell megváltozott toobe tooreflect meg a környezetben.)</span><span class="sxs-lookup"><span data-stu-id="15e1c-163">(hello variables that construct hello arguments need toobe changed tooreflect your environment.)</span></span>
* <span data-ttu-id="15e1c-164">hello csomag AzureAgent erőforrás hello Azure Virtuálisgép-ügynök, ez az ajánlott minden Azure-ban futó virtuális gépre telepíti.</span><span class="sxs-lookup"><span data-stu-id="15e1c-164">hello package AzureAgent resource will install hello Azure VM agent, which is recommended on every VM that runs in Azure.</span></span> <span data-ttu-id="15e1c-165">hello Azure Virtuálisgép-ügynök is teszi lehetséges tooadd bővítmények toohello virtuális gép a feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="15e1c-165">hello Azure VM agent also makes it possible tooadd extensions toohello VM after failover.</span></span>
* <span data-ttu-id="15e1c-166">hello szolgáltatást vagy erőforrást biztosítja, hogy hello kapcsolatos mobilitási szolgáltatásokat és hello Azure-szolgáltatások mindig futnak.</span><span class="sxs-lookup"><span data-stu-id="15e1c-166">hello service resource or resources will ensure that hello related Mobility services and hello Azure services are always running.</span></span>

<span data-ttu-id="15e1c-167">Mentés hello konfigurációt **ASRMobilityService**.</span><span class="sxs-lookup"><span data-stu-id="15e1c-167">Save hello configuration as **ASRMobilityService**.</span></span>

> [!NOTE]
> <span data-ttu-id="15e1c-168">Ne felejtse el tooreplace hello CSIP a konfigurációs tooreflect hello tényleges felügyeleti kiszolgálóján, így hello ügynök megfelelően csatlakoztatva és hello helyes jelszót fogja használni.</span><span class="sxs-lookup"><span data-stu-id="15e1c-168">Remember tooreplace hello CSIP in your configuration tooreflect hello actual management server, so that hello agent will be connected correctly and will use hello correct passphrase.</span></span>
>
>

## <a name="step-3-upload-tooautomation-dsc"></a><span data-ttu-id="15e1c-169">3. lépés: Töltse fel a tooAutomation DSC</span><span class="sxs-lookup"><span data-stu-id="15e1c-169">Step 3: Upload tooAutomation DSC</span></span>
<span data-ttu-id="15e1c-170">Mivel végzett hello DSC-konfiguráció importálni fogja a szükséges DSC erőforrásmodul (xPSDesiredStateConfiguration), meg kell tooimport az automatizálás modult hello DSC-konfiguráció feltöltés előtt.</span><span class="sxs-lookup"><span data-stu-id="15e1c-170">Because hello DSC configuration that you made will import a required DSC resource module (xPSDesiredStateConfiguration), you need tooimport that module in Automation before you upload hello DSC configuration.</span></span>

<span data-ttu-id="15e1c-171">Tooyour Automation-fiók bejelentkezési Tallózás túl**eszközök** > **modulok**, és kattintson a **Tallózás gyűjtemény**.</span><span class="sxs-lookup"><span data-stu-id="15e1c-171">Sign in tooyour Automation account, browse too**Assets** > **Modules**, and click **Browse Gallery**.</span></span>

<span data-ttu-id="15e1c-172">Itt kereshet hello modul és importálja a tooyour fiók.</span><span class="sxs-lookup"><span data-stu-id="15e1c-172">Here you can search for hello module and import it tooyour account.</span></span>

![Modul importálása](./media/site-recovery-automate-mobilitysevice-install/search-and-import-module.png)

<span data-ttu-id="15e1c-174">Ha ezzel végzett, nyissa meg tooyour gép hello Azure Resource Manager-modulok telepítése esetében, és folytatható tooimport az újonnan létrehozott hello DSC-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="15e1c-174">When you finish this, go tooyour machine where you have hello Azure Resource Manager modules installed and proceed tooimport hello newly created DSC configuration.</span></span>

### <a name="import-cmdlets"></a><span data-ttu-id="15e1c-175">Importálási parancsmagokat</span><span class="sxs-lookup"><span data-stu-id="15e1c-175">Import cmdlets</span></span>
<span data-ttu-id="15e1c-176">A PowerShell a bejelentkezés tooyour Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="15e1c-176">In PowerShell, sign in tooyour Azure subscription.</span></span> <span data-ttu-id="15e1c-177">Hello parancsmagok tooreflect módosítása a környezetben, és rögzítheti az automatizálási fiók adatait egy változóban:</span><span class="sxs-lookup"><span data-stu-id="15e1c-177">Modify hello cmdlets tooreflect your environment and capture your Automation account information in a variable:</span></span>

```powershell
$AAAccount = Get-AzureRmAutomationAccount -ResourceGroupName 'KNOMS' -Name 'KNOMSAA'
```

<span data-ttu-id="15e1c-178">Töltse fel a hello konfigurációs tooAutomation DSC hello a következő parancsmag használatával:</span><span class="sxs-lookup"><span data-stu-id="15e1c-178">Upload hello configuration tooAutomation DSC by using hello following cmdlet:</span></span>

```powershell
$ImportArgs = @{
    SourcePath = 'C:\ASR\ASRMobilityService.ps1'
    Published = $true
    Description = 'DSC Config for Mobility Service'
}
$AAAccount | Import-AzureRmAutomationDscConfiguration @ImportArgs
```

### <a name="compile-hello-configuration-in-automation-dsc"></a><span data-ttu-id="15e1c-179">Hello konfiguráció az Automation DSC-fordítási</span><span class="sxs-lookup"><span data-stu-id="15e1c-179">Compile hello configuration in Automation DSC</span></span>
<span data-ttu-id="15e1c-180">A következő lépésben toocompile hello konfiguráció Automation DSC, így tooregister csomópontok tooit megkezdése.</span><span class="sxs-lookup"><span data-stu-id="15e1c-180">Next, you need toocompile hello configuration in Automation DSC, so that you can start tooregister nodes tooit.</span></span> <span data-ttu-id="15e1c-181">Érhetők el, amely hello a következő parancsmag futtatásával:</span><span class="sxs-lookup"><span data-stu-id="15e1c-181">You achieve that by running hello following cmdlet:</span></span>

```powershell
$AAAccount | Start-AzureRmAutomationDscCompilationJob -ConfigurationName ASRMobilityService
```

<span data-ttu-id="15e1c-182">Ez is igénybe vehet néhány percet, mert alapvetően telepít hello konfigurációs üzemeltetett toohello DSC lekérési szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="15e1c-182">This can take a few minutes, because you're basically deploying hello configuration toohello hosted DSC pull service.</span></span>

<span data-ttu-id="15e1c-183">Után hello konfigurációs lefordításához hello feladatinformációkat powershellel (Get-AzureRmAutomationDscCompilationJob) vagy hello segítségével kérheti le [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="15e1c-183">After you compile hello configuration, you can retrieve hello job information by using PowerShell (Get-AzureRmAutomationDscCompilationJob) or by using hello [Azure portal](https://portal.azure.com/).</span></span>

![Feladat beolvasása](./media/site-recovery-automate-mobilitysevice-install/retrieve-job.png)

<span data-ttu-id="15e1c-185">Most már sikeresen közzé, és a DSC-konfiguráció tooAutomation DSC feltöltve.</span><span class="sxs-lookup"><span data-stu-id="15e1c-185">You have now successfully published and uploaded your DSC configuration tooAutomation DSC.</span></span>

## <a name="step-4-onboard-machines-tooautomation-dsc"></a><span data-ttu-id="15e1c-186">4. lépés: A bevezetni gépek tooAutomation DSC</span><span class="sxs-lookup"><span data-stu-id="15e1c-186">Step 4: Onboard machines tooAutomation DSC</span></span>
> [!NOTE]
> <span data-ttu-id="15e1c-187">Ez a forgatókönyv befejezése hello előfeltételei egyike, hogy a Windows-alapú gépek hello WMF legújabb verziójára frissít.</span><span class="sxs-lookup"><span data-stu-id="15e1c-187">One of hello prerequisites for completing this scenario is that your Windows machines are updated with hello latest version of WMF.</span></span> <span data-ttu-id="15e1c-188">Töltse le, és telepítse a hello megfelelő verzióját a platformhoz való hello [letöltőközpontból](https://www.microsoft.com/download/details.aspx?id=50395).</span><span class="sxs-lookup"><span data-stu-id="15e1c-188">You can download and install hello correct version for your platform from hello [Download Center](https://www.microsoft.com/download/details.aspx?id=50395).</span></span>
>
>

<span data-ttu-id="15e1c-189">Most, hogy érvényesek-e tooyour csomópontok DSC létrehozandó egy metaconfig.</span><span class="sxs-lookup"><span data-stu-id="15e1c-189">You will now create a metaconfig for DSC that you will apply tooyour nodes.</span></span> <span data-ttu-id="15e1c-190">Ennek toosucceed, kell tooretrieve hello URL-cím és hello elsődleges végpontkulcs a kijelölt Automation-fiókhoz az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="15e1c-190">toosucceed with this, you need tooretrieve hello endpoint URL and hello primary key for your selected Automation account in Azure.</span></span> <span data-ttu-id="15e1c-191">Ezek az értékek alapján is megtalálhatja **kulcsok** a hello **összes beállítás** hello Automation-fiók paneljén.</span><span class="sxs-lookup"><span data-stu-id="15e1c-191">You can find these values under **Keys** on hello **All settings** blade for hello Automation account.</span></span>

![Értékek](./media/site-recovery-automate-mobilitysevice-install/key-values.png)

<span data-ttu-id="15e1c-193">Ebben a példában egy megjeleníteni kívánt tooprotect Site Recovery segítségével fizikai Windows Server 2012 R2-kiszolgálóval rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="15e1c-193">In this example, you have a Windows Server 2012 R2 physical server that you want tooprotect by using Site Recovery.</span></span>

### <a name="check-for-any-pending-file-rename-operations-in-hello-registry"></a><span data-ttu-id="15e1c-194">Minden függőben lévő fájlátnevezési művelet hello beállításjegyzék ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="15e1c-194">Check for any pending file rename operations in hello registry</span></span>
<span data-ttu-id="15e1c-195">Mielőtt tooassociate hello server hello Automation DSC-végponthoz, azt javasoljuk, hogy ellenőrizze az összes függőben lévő fájlműveletek átnevezése hello beállításjegyzékben.</span><span class="sxs-lookup"><span data-stu-id="15e1c-195">Before you start tooassociate hello server with hello Automation DSC endpoint, we recommend that you check for any pending file rename operations in hello registry.</span></span> <span data-ttu-id="15e1c-196">A befejezési miatt előfordulhat, hogy tiltják hello beállítása tooa függőben lévő újraindítás.</span><span class="sxs-lookup"><span data-stu-id="15e1c-196">They might prohibit hello setup from finishing due tooa pending reboot.</span></span>

<span data-ttu-id="15e1c-197">Futtassa a következő parancsmag tooverify, hogy nincs-e hello kiszolgálón nincs függőben lévő újraindítás hello:</span><span class="sxs-lookup"><span data-stu-id="15e1c-197">Run hello following cmdlet tooverify that there’s no pending reboot on hello server:</span></span>

```powershell
Get-ItemProperty 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\' | Select-Object -Property PendingFileRenameOperations
```
<span data-ttu-id="15e1c-198">Ez azt mutatja, üres, ha OK tooproceed áll.</span><span class="sxs-lookup"><span data-stu-id="15e1c-198">If this shows empty, you are OK tooproceed.</span></span> <span data-ttu-id="15e1c-199">Ha nem, eleget kell tennie a karbantartási időszak alatt hello server újraindításával.</span><span class="sxs-lookup"><span data-stu-id="15e1c-199">If not, you should address this by rebooting hello server during a maintenance window.</span></span>

<span data-ttu-id="15e1c-200">tooapply hello konfigurációs hello kiszolgálón indítsa el a hello PowerShell integrált parancsfájlkezelési környezet (ISE), és futtassa a következő parancsfájl hello.</span><span class="sxs-lookup"><span data-stu-id="15e1c-200">tooapply hello configuration on hello server, start hello PowerShell Integrated Scripting Environment (ISE) and run hello following script.</span></span> <span data-ttu-id="15e1c-201">Ez a beállítás lényegében egy DSC helyi hello helyi Configuration Manager motor tooregister az Automation DSC szolgáltatás hello utasítani és hello konfigurációs (ASRMobilityService.localhost) beolvasása.</span><span class="sxs-lookup"><span data-stu-id="15e1c-201">This is essentially a DSC local configuration that will instruct hello Local Configuration Manager engine tooregister with hello Automation DSC service and retrieve hello specific configuration (ASRMobilityService.localhost).</span></span>

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

<span data-ttu-id="15e1c-202">Ez a konfiguráció miatt hello helyi Configuration Manager motor tooregister magát az Automation DSC Szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="15e1c-202">This configuration will cause hello Local Configuration Manager engine tooregister itself with Automation DSC.</span></span> <span data-ttu-id="15e1c-203">Azt is meghatározza hello motor hogyan működjön, mit kell tenni, ha a konfigurációs eltéréseket (ApplyAndAutoCorrect), és hogyan, kell folytatni a hello konfigurációs Ha újraindításra szükség.</span><span class="sxs-lookup"><span data-stu-id="15e1c-203">It will also determine how hello engine should operate, what it should do if there's a configuration drift (ApplyAndAutoCorrect), and how it should proceed with hello configuration if a reboot is required.</span></span>

<span data-ttu-id="15e1c-204">Ez a parancsfájl futtatása után hello csomópont tooregister kell kezdenie az Automation DSC Szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="15e1c-204">After you run this script, hello node should start tooregister with Automation DSC.</span></span>

![Folyamatban van a csomópont regisztrációs](./media/site-recovery-automate-mobilitysevice-install/register-node.png)

<span data-ttu-id="15e1c-206">Azure-portálon toohello vissza, ha hello újonnan regisztrált csomóponton most már megjelent a portál hello látható.</span><span class="sxs-lookup"><span data-stu-id="15e1c-206">If you go back toohello Azure portal, you can see that hello newly registered node has now appeared in hello portal.</span></span>

![Hello portálon regisztrált csomópont](./media/site-recovery-automate-mobilitysevice-install/registered-node.png)

<span data-ttu-id="15e1c-208">Hello kiszolgálón futtatható hello PowerShell parancsmag tooverify, hogy a csomópont hello megfelelően regisztrálva van a következő:</span><span class="sxs-lookup"><span data-stu-id="15e1c-208">On hello server, you can run hello following PowerShell cmdlet tooverify that hello node has been registered correctly:</span></span>

```powershell
Get-DscLocalConfigurationManager
```

<span data-ttu-id="15e1c-209">Hello konfigurációs lekért és alkalmazott toohello server után ezt hello a következő parancsmag futtatásával ellenőrizheti:</span><span class="sxs-lookup"><span data-stu-id="15e1c-209">After hello configuration has been pulled and applied toohello server, you can verify this by running hello following cmdlet:</span></span>

```powershell
Get-DscConfigurationStatus
```

<span data-ttu-id="15e1c-210">hello az alábbiakat mutatja be, hello kiszolgálón sikeresen hívja elő a konfigurációban:</span><span class="sxs-lookup"><span data-stu-id="15e1c-210">hello output shows that hello server has successfully pulled its configuration:</span></span>

![Kimenet](./media/site-recovery-automate-mobilitysevice-install/successful-config.png)

<span data-ttu-id="15e1c-212">Ezenkívül hello mobilitási szolgáltatás telepítő rendelkezik-e a saját naplóban, amely a következő *SystemDrive*\ProgramData\ASRSetupLogs.</span><span class="sxs-lookup"><span data-stu-id="15e1c-212">In addition, hello Mobility service setup has its own log that can be found at *SystemDrive*\ProgramData\ASRSetupLogs.</span></span>

<span data-ttu-id="15e1c-213">Ennyi az egész.</span><span class="sxs-lookup"><span data-stu-id="15e1c-213">That’s it.</span></span> <span data-ttu-id="15e1c-214">Most már sikeresen telepített és regisztrált hello mobilitási szolgáltatás, amelyet az tooprotect Site Recovery segítségével hello gépen.</span><span class="sxs-lookup"><span data-stu-id="15e1c-214">You have now successfully deployed and registered hello Mobility service on hello machine that you want tooprotect by using Site Recovery.</span></span> <span data-ttu-id="15e1c-215">A DSC-ből fog győződjön meg arról, hogy hello szükséges szolgáltatások mindig futnak.</span><span class="sxs-lookup"><span data-stu-id="15e1c-215">DSC will make sure that hello required services are always running.</span></span>

![Sikeres telepítés](./media/site-recovery-automate-mobilitysevice-install/successful-install.png)

<span data-ttu-id="15e1c-217">Miután hello felügyeleti kiszolgáló észleli-hello sikeres telepítés, konfigurálja a védelmet, és replikáció engedélyezése gépen hello Site Recovery segítségével.</span><span class="sxs-lookup"><span data-stu-id="15e1c-217">After hello management server detects hello successful deployment, you can configure protection and enable replication on hello machine by using Site Recovery.</span></span>

## <a name="use-dsc-in-disconnected-environments"></a><span data-ttu-id="15e1c-218">A DSC használata a kapcsolat nélküli környezetekben</span><span class="sxs-lookup"><span data-stu-id="15e1c-218">Use DSC in disconnected environments</span></span>
<span data-ttu-id="15e1c-219">Ha a gépek nem csatlakoztatott toohello Internet, továbbra is DSC toodeploy támaszkodnak és hello mobilitási szolgáltatás konfigurálása, hogy szeretné-e tooprotect hello munkaterhelések.</span><span class="sxs-lookup"><span data-stu-id="15e1c-219">If your machines aren’t connected toohello Internet, you can still rely on DSC toodeploy and configure hello Mobility service on hello workloads that you want tooprotect.</span></span>

<span data-ttu-id="15e1c-220">A környezetben a saját DSC lekérési kiszolgálójával példányosítható tooessentially biztosítanak hello Automation DSC hogy azonos képességekkel.</span><span class="sxs-lookup"><span data-stu-id="15e1c-220">You can instantiate your own DSC pull server in your environment tooessentially provide hello same capabilities that you get from Automation DSC.</span></span> <span data-ttu-id="15e1c-221">Ez azt jelenti, hogy hello ügyfelek fogja lekérni a konfigurációs hello (regisztráció) után toohello DSC végpont.</span><span class="sxs-lookup"><span data-stu-id="15e1c-221">That is, hello clients will pull hello configuration (after it's registered) toohello DSC endpoint.</span></span> <span data-ttu-id="15e1c-222">Egy másik nincs azonban toomanually leküldéses hello DSC konfigurációs tooyour gépek, helyileg vagy távolról.</span><span class="sxs-lookup"><span data-stu-id="15e1c-222">However, another option is toomanually push hello DSC configuration tooyour machines, either locally or remotely.</span></span>

<span data-ttu-id="15e1c-223">Vegye figyelembe, hogy ebben a példában egy hozzáadott paraméter hello számítógép neveként.</span><span class="sxs-lookup"><span data-stu-id="15e1c-223">Note that in this example, there's an added parameter for hello computer name.</span></span> <span data-ttu-id="15e1c-224">hello távoli most találhatók egy távoli megosztáson elérhetőnek kell lennie, amelyet az tooprotect hello gépek.</span><span class="sxs-lookup"><span data-stu-id="15e1c-224">hello remote files are now located on a remote share that should be accessible by hello machines that you want tooprotect.</span></span> <span data-ttu-id="15e1c-225">hello parancsfájl hello vége hello konfigurációs ír elő, és ezután elindítja az tooapply hello DSC konfigurációs toohello célszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="15e1c-225">hello end of hello script enacts hello configuration and then starts tooapply hello DSC configuration toohello target computer.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="15e1c-226">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="15e1c-226">Prerequisites</span></span>
<span data-ttu-id="15e1c-227">Győződjön meg arról, hogy hello xPSDesiredStateConfiguration PowerShell modul telepítve van-e.</span><span class="sxs-lookup"><span data-stu-id="15e1c-227">Make sure that hello xPSDesiredStateConfiguration PowerShell module is installed.</span></span> <span data-ttu-id="15e1c-228">Windows-alapú gépek WMF 5.0 futtató telepítheti hello xPSDesiredStateConfiguration modul hello parancsmag hello célszámítógépen a következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="15e1c-228">For Windows machines where WMF 5.0 is installed, you can install hello xPSDesiredStateConfiguration module by running hello following cmdlet on hello target machines:</span></span>

```powershell
Find-Module -Name xPSDesiredStateConfiguration | Install-Module
```

<span data-ttu-id="15e1c-229">Is töltse le és mentse hello modul, abban az esetben toodistribute kell azt tooWindows gépeken, amelyek WMF 4.0-s verzióját.</span><span class="sxs-lookup"><span data-stu-id="15e1c-229">You can also download and save hello module in case you need toodistribute it tooWindows machines that have WMF 4.0.</span></span> <span data-ttu-id="15e1c-230">Futtassa ezt a parancsmagot egy számítógépen, ha jelen-e PowerShellGet (WMF 5.0):</span><span class="sxs-lookup"><span data-stu-id="15e1c-230">Run this cmdlet on a machine where PowerShellGet (WMF 5.0) is present:</span></span>

```powershell
Save-Module -Name xPSDesiredStateConfiguration -Path <location>
```

<span data-ttu-id="15e1c-231">Is WMF 4.0, győződjön meg arról, hogy hello [Windows 8.1 update KB2883200](https://www.microsoft.com/download/details.aspx?id=40749) a hello készülékre van telepítve.</span><span class="sxs-lookup"><span data-stu-id="15e1c-231">Also for WMF 4.0, ensure that hello [Windows 8.1 update KB2883200](https://www.microsoft.com/download/details.aspx?id=40749) is installed on hello machines.</span></span>

<span data-ttu-id="15e1c-232">hello következő konfigurációs továbbíthatja tooWindows gépeken, amelyek WMF 5.0 és WMF 4.0:</span><span class="sxs-lookup"><span data-stu-id="15e1c-232">hello following configuration can be pushed tooWindows machines that have WMF 5.0 and WMF 4.0:</span></span>

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

<span data-ttu-id="15e1c-233">Ha azt szeretné tooinstantiate saját DSC lekérési kiszolgálójával be a vállalati hálózat toomimic hello képességeket, amelyek Automation DSC letölthető, lásd: [DSC lekérési webkiszolgáló beállítása](https://msdn.microsoft.com/powershell/dsc/pullserver?f=255&MSPPError=-2147217396).</span><span class="sxs-lookup"><span data-stu-id="15e1c-233">If you want tooinstantiate your own DSC pull server on your corporate network toomimic hello capabilities that you can get from Automation DSC, see [Setting up a DSC web pull server](https://msdn.microsoft.com/powershell/dsc/pullserver?f=255&MSPPError=-2147217396).</span></span>

## <a name="optional-deploy-a-dsc-configuration-by-using-an-azure-resource-manager-template"></a><span data-ttu-id="15e1c-234">Választható lehetőség: Telepítését a DSC-konfiguráció Azure Resource Manager-sablonnal</span><span class="sxs-lookup"><span data-stu-id="15e1c-234">Optional: Deploy a DSC configuration by using an Azure Resource Manager template</span></span>
<span data-ttu-id="15e1c-235">Ez a cikk fordította hogyan hozhatja létre saját DSC-konfiguráció tooautomatically hello mobilitási szolgáltatás és az Azure Virtuálisgép-ügynök – hello telepítéséhez, és győződjön meg arról, hogy futnak a hello gépeken, amelyet az tooprotect.</span><span class="sxs-lookup"><span data-stu-id="15e1c-235">This article has focused on how you can create your own DSC configuration tooautomatically deploy hello Mobility service and hello Azure VM Agent--and ensure that they are running on hello machines that you want tooprotect.</span></span> <span data-ttu-id="15e1c-236">Azt is, hogy az Azure Resource Manager sablon, amely telepíti a DSC konfigurációs tooa új vagy meglévő Azure Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="15e1c-236">We also have an Azure Resource Manager template that will deploy this DSC configuration tooa new or existing Azure Automation account.</span></span> <span data-ttu-id="15e1c-237">hello sablon bemeneti paraméterek toocreate Automation eszközök, amely a környezetnek hello változók fogja tartalmazni fogja használni.</span><span class="sxs-lookup"><span data-stu-id="15e1c-237">hello template will use input parameters toocreate Automation assets that will contain hello variables for your environment.</span></span>

<span data-ttu-id="15e1c-238">Hello sablon telepítése után olvassa el a egyszerűen a Ez az útmutató tooonboard 4 toostep a gépek.</span><span class="sxs-lookup"><span data-stu-id="15e1c-238">After you deploy hello template, you can simply refer toostep 4 in this guide tooonboard your machines.</span></span>

<span data-ttu-id="15e1c-239">hello sablon hajt végre a következő hello:</span><span class="sxs-lookup"><span data-stu-id="15e1c-239">hello template will do hello following:</span></span>

1. <span data-ttu-id="15e1c-240">Használjon meglévő Automation-fiókot, vagy hozzon létre egy újat</span><span class="sxs-lookup"><span data-stu-id="15e1c-240">Use an existing Automation account or create a new one</span></span>
2. <span data-ttu-id="15e1c-241">A bemeneti paraméterek igénybe, hogy:</span><span class="sxs-lookup"><span data-stu-id="15e1c-241">Take input parameters for:</span></span>
   * <span data-ttu-id="15e1c-242">ASRRemoteFile – hello mobilitási szolgáltatás beállítása tároló hello helye</span><span class="sxs-lookup"><span data-stu-id="15e1c-242">ASRRemoteFile--hello location where you have stored hello Mobility service setup</span></span>
   * <span data-ttu-id="15e1c-243">ASRPassphrase – hello passphrase.txt fájl tároló hello helye</span><span class="sxs-lookup"><span data-stu-id="15e1c-243">ASRPassphrase--hello location where you have stored hello passphrase.txt file</span></span>
   * <span data-ttu-id="15e1c-244">ASRCSEndpoint – hello IP-címet a felügyeleti kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="15e1c-244">ASRCSEndpoint--hello IP address of your management server</span></span>
3. <span data-ttu-id="15e1c-245">Hello xPSDesiredStateConfiguration PowerShell modul importálása</span><span class="sxs-lookup"><span data-stu-id="15e1c-245">Import hello xPSDesiredStateConfiguration PowerShell module</span></span>
4. <span data-ttu-id="15e1c-246">Hozzon létre és hello DSC-konfiguráció-fordítási</span><span class="sxs-lookup"><span data-stu-id="15e1c-246">Create and compile hello DSC configuration</span></span>

<span data-ttu-id="15e1c-247">A fenti lépéseket minden hello hello megfelelő sorrendben, történik meg, hogy elindíthatja bevezetése az ügyfélgépek.</span><span class="sxs-lookup"><span data-stu-id="15e1c-247">All hello preceding steps will happen in hello right order, so that you can start onboarding your machines for protection.</span></span>

<span data-ttu-id="15e1c-248">hello sablon üzembe helyezés esetén utasításokkal található [GitHub](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/DSC).</span><span class="sxs-lookup"><span data-stu-id="15e1c-248">hello template, with instructions for deployment, is located on [GitHub](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/DSC).</span></span>

<span data-ttu-id="15e1c-249">Hello sablon üzembe helyezése a PowerShell használatával:</span><span class="sxs-lookup"><span data-stu-id="15e1c-249">Deploy hello template by using PowerShell:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="15e1c-250">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="15e1c-250">Next steps</span></span>
<span data-ttu-id="15e1c-251">Miután hello mobilitási szolgáltatás ügynökök telepítésére, [engedélyezze a replikálást](site-recovery-vmware-to-azure.md) hello virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="15e1c-251">After you deploy hello Mobility service agents, you can [enable replication](site-recovery-vmware-to-azure.md) for hello virtual machines.</span></span>
