---
title: "aaaTroubleshoot Windows virtuális gép aktiválási problémák az Azure-ban |} Microsoft Docs"
description: "Hello biztosít hibáinak elhárítása Windows virtuális gép aktiválási problémák elhárítása az Azure-ban lépései"
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: genlin
manager: willchen
editor: 
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: genli
ms.openlocfilehash: 4ebdac66166e80dcd68cd9e2931b30a29ac01eef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-windows-virtual-machine-activation-problems"></a><span data-ttu-id="f039b-103">Azure Windows virtuális gép aktiválással kapcsolatos problémák elhárítása</span><span class="sxs-lookup"><span data-stu-id="f039b-103">Troubleshoot Azure Windows virtual machine activation problems</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="f039b-104">Ha gondja támad, amikor aktiválja a Azure Windows virtuális gép (VM), amely egy egyéni lemezképből jön létre, a dokumentum tootroubleshoot hello probléma hello információk is használhatja.</span><span class="sxs-lookup"><span data-stu-id="f039b-104">If you have trouble when activating Azure Windows virtual machine (VM) that is created from a custom image, you can use hello information provided in this document tootroubleshoot hello issue.</span></span> 

## <a name="symptom"></a><span data-ttu-id="f039b-105">Jelenség</span><span class="sxs-lookup"><span data-stu-id="f039b-105">Symptom</span></span>

<span data-ttu-id="f039b-106">Amikor egy Windows Azure VM tooactivate, hibaüzenetet kap üzenet a következő minta hello hasonlít:</span><span class="sxs-lookup"><span data-stu-id="f039b-106">When you try tooactivate an Azure Windows VM, you receive an error message resembles hello following sample:</span></span>

<span data-ttu-id="f039b-107">**Hiba: a szoftver LicensingService 0xC004F074 hello jelentett hello számítógép aktiválása sikertelen volt. Érhető el. nem kulcs ManagementService (KMS). Tekintse meg az alkalmazások eseménynaplójában hello további információt.**</span><span class="sxs-lookup"><span data-stu-id="f039b-107">**Error: 0xC004F074 hello Software LicensingService reported that hello computer could not be activated. No Key ManagementService (KMS) could be contacted. Please see hello Application Event Log for additional information.**</span></span>

## <a name="cause"></a><span data-ttu-id="f039b-108">Ok</span><span class="sxs-lookup"><span data-stu-id="f039b-108">Cause</span></span>
<span data-ttu-id="f039b-109">Azure virtuális gép aktiválási problémák általában fordulhat elő, hello Windows virtuális gép nem konfigurálható megfelelő KMS-ügyfél telepítési kulcsának használatával hello, vagy hello Windows virtuális gép rendelkezik a kapcsolati probléma toohello Azure KMS szolgáltatást (kms.core.windows.net, port 1668).</span><span class="sxs-lookup"><span data-stu-id="f039b-109">Generally, Azure VM activation issues occur if hello Windows VM is not configured by using hello appropriate KMS client setup key, or hello Windows VM has a connectivity problem toohello Azure KMS service (kms.core.windows.net, port 1668).</span></span> 

## <a name="solution"></a><span data-ttu-id="f039b-110">Megoldás</span><span class="sxs-lookup"><span data-stu-id="f039b-110">Solution</span></span>

>[!NOTE]
><span data-ttu-id="f039b-111">Ha a telephelyek közötti VPN használ, és a kényszerített bújtatás, lásd: [használata Azure egyéni útvonalak tooenable KMS-aktiválás kényszerített bújtatással](http://blogs.msdn.com/b/mast/archive/2015/05/20/use-azure-custom-routes-to-enable-kms-activation-with-forced-tunneling.aspx).</span><span class="sxs-lookup"><span data-stu-id="f039b-111">If you are using a site-to-site VPN and forced tunneling, see [Use Azure custom routes tooenable KMS activation with forced tunneling](http://blogs.msdn.com/b/mast/archive/2015/05/20/use-azure-custom-routes-to-enable-kms-activation-with-forced-tunneling.aspx).</span></span> 
>
><span data-ttu-id="f039b-112">Ha ExpressRoute használ, és rendelkezik az alapértelmezett útvonal közzétett című [Azure virtuális gép tooactivate ExpressRoute előfordulhat, hogy feladatátvételt](http://blogs.msdn.com/b/mast/archive/2015/12/01/azure-vm-may-fail-to-activate-over-expressroute.aspx).</span><span class="sxs-lookup"><span data-stu-id="f039b-112">If you are using ExpressRoute and you have a default route published, see [Azure VM may fail tooactivate over ExpressRoute](http://blogs.msdn.com/b/mast/archive/2015/12/01/azure-vm-may-fail-to-activate-over-expressroute.aspx).</span></span>

### <a name="step-1-configure-hello-appropriate-kms-client-setup-key-for-windows-server-2016-and-windows-server-2012-r2"></a><span data-ttu-id="f039b-113">1. lépés konfigurálása hello megfelelő KMS-ügyfél telepítő kulcsát (a Windows Server 2016 és a Windows Server 2012 R2)</span><span class="sxs-lookup"><span data-stu-id="f039b-113">Step 1 Configure hello appropriate KMS client setup key (for Windows Server 2016 and Windows Server 2012 R2)</span></span>

<span data-ttu-id="f039b-114">A virtuális Gépet, amely a Windows Server 2016 vagy a Windows Server 2012 R2 egyéni lemezkép készült hello hello megfelelő KMS-ügyfél telepítő kulcsát a virtuális gép hello kell konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="f039b-114">For hello VM that is created from a custom image of Windows Server 2016 or Windows Server 2012 R2, you must configure hello appropriate KMS client setup key for hello VM.</span></span>

<span data-ttu-id="f039b-115">Ez a lépés nem érvényes tooWindows 2012 vagy Windows 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="f039b-115">This step does not apply tooWindows 2012 or Windows 2008 R2.</span></span> <span data-ttu-id="f039b-116">Hello Automation virtuális gép aktiválása (AVMA) szolgáltatás, amely csak a Windows Server 2012 R2 és Windows Server 2016 által támogatott használ.</span><span class="sxs-lookup"><span data-stu-id="f039b-116">It uses hello Automation Virtual Machine Activation (AVMA) feature, which is supported only by Windows Server 2016 and Windows Server 2012 R2.</span></span>

1. <span data-ttu-id="f039b-117">Futtatás **slmgr.vbs/dlv** egy rendszergazda jogú parancssorba.</span><span class="sxs-lookup"><span data-stu-id="f039b-117">Run **slmgr.vbs /dlv** at an elevated command prompt.</span></span> <span data-ttu-id="f039b-118">Ellenőrizze a hello leírás hello kimeneti értéket, és ellenőrizze, hogy létrehozták, kereskedelmi (KISKERESKEDELMI csatorna) vagy (VOLUME_KMSCLIENT) mennyiségi licencelési adathordozóról:</span><span class="sxs-lookup"><span data-stu-id="f039b-118">Check hello Description value in hello output, and then determine whether it was created from retail (RETAIL channel) or volume (VOLUME_KMSCLIENT) license media:</span></span>
  
    ```
    cscript c:\windows\system32\slmgr.vbs /dlv
    ```

2. <span data-ttu-id="f039b-119">Ha **slmgr.vbs/dlv** jeleníti meg a KISKERESKEDELMI csatorna, futtassa a következő parancsok tooset hello hello [KMS-ügyfél telepítési kulcsának](https://technet.microsoft.com/library/jj612867%28v=ws.11%29.aspx?f=255&MSPPError=-2147217396) Windows Server verziójának hello használ, és kényszeríti a tooretry aktiválási:</span><span class="sxs-lookup"><span data-stu-id="f039b-119">If **slmgr.vbs /dlv** shows RETAIL channel, run hello following commands tooset hello [KMS client setup key](https://technet.microsoft.com/library/jj612867%28v=ws.11%29.aspx?f=255&MSPPError=-2147217396) for hello version of Windows Server being used, and force it tooretry activation:</span></span> 

    ```
    cscript c:\windows\system32\slmgr.vbs /ipk <KMS client setup key>

    cscript c:\windows\system32\slmgr.vbs /ato
     ```

    <span data-ttu-id="f039b-120">Például Windows Server 2016 Datacenter, akkor futnak a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="f039b-120">For example, for Windows Server 2016 Datacenter, you would run hello following command:</span></span>

    ```
    cscript c:\windows\system32\slmgr.vbs /ipk CB7KF-BWN84-R7R2Y-793K2-8XDDG
    ```

### <a name="step-2-verify-hello-connectivity-between-hello-vm-and-azure-kms-service"></a><span data-ttu-id="f039b-121">2. lépés ellenőrizze, hogy hello kapcsolódik hello VM és Azure KMS szolgáltatás között</span><span class="sxs-lookup"><span data-stu-id="f039b-121">Step 2 Verify hello connectivity between hello VM and Azure KMS service</span></span>

1. <span data-ttu-id="f039b-122">Letöltéséhez és kibontásához hello [Psping](http:/technet.microsoft.com/en-us/sysinternals/jj729731.aspx) eszköz tooa helyi mappa a virtuális Gépet, amely nem aktiválható hello.</span><span class="sxs-lookup"><span data-stu-id="f039b-122">Download and extract hello [Psping](http:/technet.microsoft.com/en-us/sysinternals/jj729731.aspx) tool tooa local folder in hello VM that does not activate.</span></span> 

2. <span data-ttu-id="f039b-123">Nyissa meg tooStart, keresse meg a Windows PowerShell, kattintson a jobb gombbal a Windows PowerShell és válassza a Futtatás rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="f039b-123">Go tooStart, search on Windows PowerShell, right-click Windows PowerShell, and then select Run as administrator.</span></span>

3. <span data-ttu-id="f039b-124">Győződjön meg arról, hogy a virtuális gép hello konfigurált toouse hello megfelelő Azure KMS-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="f039b-124">Make sure that hello VM is configured toouse hello correct Azure KMS server.</span></span> <span data-ttu-id="f039b-125">toodo Ez a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="f039b-125">toodo this, run the following command:</span></span>
  
    ```
    iex “$env:windir\system32\cscript.exe $env:windir\system32\slmgr.vbs /skms
    kms.core.windows.net:1688
    ```
    <span data-ttu-id="f039b-126">Hello parancsot kell visszaadnia: kulcskezelő szolgáltatás számítógépnév tookms.core.windows.net:1688 beállítása sikerült.</span><span class="sxs-lookup"><span data-stu-id="f039b-126">hello command should return: Key Management Service machine name set tookms.core.windows.net:1688 successfully.</span></span>

4. <span data-ttu-id="f039b-127">Győződjön meg arról, hogy rendelkezik-e kapcsolat toohello KMS-kiszolgáló Psping használatával.</span><span class="sxs-lookup"><span data-stu-id="f039b-127">Verify by using Psping that you have connectivity toohello KMS server.</span></span> <span data-ttu-id="f039b-128">Váltás toohello mappát, amelyikbe kibontotta hello Pstools.zip letöltése, és futtassa a következő hello:</span><span class="sxs-lookup"><span data-stu-id="f039b-128">Switch toohello folder where you extracted hello Pstools.zip download, and then run hello following:</span></span>
  
    ```
    \psping.exe kms.core.windows.net:1688
    ```
  
  <span data-ttu-id="f039b-129">Utolsó előtti második sorában hello hello kimeneti, győződjön meg arról, hogy látni: küldött = 4, fogadott = 4, elveszett = 0 (0 % veszteség).</span><span class="sxs-lookup"><span data-stu-id="f039b-129">In hello second-to-last line of hello output, make sure that you see: Sent = 4, Received = 4, Lost = 0 (0% loss).</span></span>

  <span data-ttu-id="f039b-130">Ha elveszett nagyobb, mint 0 (nulla), hello virtuális gép nincs kapcsolat toohello KMS-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="f039b-130">If Lost is greater than 0 (zero), hello VM does not have connectivity toohello KMS server.</span></span> <span data-ttu-id="f039b-131">Ebben a helyzetben hello virtuális gép virtuális hálózatban, és van egy egyéni DNS-kiszolgáló megadva, meg kell győződnie arról, hogy a DNS-kiszolgáló esetén képes tooresolve kms.core.windows.net.</span><span class="sxs-lookup"><span data-stu-id="f039b-131">In this situation, if hello VM is in a virtual network and has a custom DNS server specified, you must make sure that DNS server is able tooresolve kms.core.windows.net.</span></span> <span data-ttu-id="f039b-132">Vagy hello DNS kiszolgáló tooone kms.core.windows.net megoldásához módosítsa.</span><span class="sxs-lookup"><span data-stu-id="f039b-132">Or, change hello DNS server tooone that does resolve kms.core.windows.net.</span></span>

  <span data-ttu-id="f039b-133">Figyelje meg, hogy ha eltávolítja az összes DNS-kiszolgáló egy virtuális hálózatot, virtuális gépek Azure belső DNS-szolgáltatás használatára.</span><span class="sxs-lookup"><span data-stu-id="f039b-133">Notice that if you remove all DNS servers from a virtual network, VMs use Azure’s internal DNS service.</span></span> <span data-ttu-id="f039b-134">Ez a szolgáltatás kms.core.windows.net tudja oldani.</span><span class="sxs-lookup"><span data-stu-id="f039b-134">This service can resolve kms.core.windows.net.</span></span>
  
<span data-ttu-id="f039b-135">Azt is ellenőrizze, hogy a hello Vendég tűzfal nincs konfigurálva, hogy megakadályozza a aktiválási kísérletek.</span><span class="sxs-lookup"><span data-stu-id="f039b-135">Also verify that hello guest firewall has not been configured in a manner that would block activation attempts.</span></span>

5. <span data-ttu-id="f039b-136">Sikeres kapcsolat tookms.core.windows.net ellenőrzése után futtassa a következő parancsot, hogy emelt szintű Windows PowerShell-parancssorba hello.</span><span class="sxs-lookup"><span data-stu-id="f039b-136">After you verify successful connectivity tookms.core.windows.net, run hello following command at that elevated Windows PowerShell prompt.</span></span> <span data-ttu-id="f039b-137">Ez a parancs megpróbál aktiválási több alkalommal.</span><span class="sxs-lookup"><span data-stu-id="f039b-137">This command tries activation multiple times.</span></span>

    ```
    1..12 | % { iex “$env:windir\system32\cscript.exe $env:windir\system32\slmgr.vbs /ato” ; start-sleep 5 }
    ```

<span data-ttu-id="f039b-138">A sikeres aktiválási hello alábbihoz információt adhatja vissza:</span><span class="sxs-lookup"><span data-stu-id="f039b-138">A successful activation returns information that resembles hello following:</span></span>

<span data-ttu-id="f039b-139">**Windows(R), ServerDatacenter edition (12345678-1234-1234-1234-12345678) aktiválása... A termék aktiválása sikeres.**</span><span class="sxs-lookup"><span data-stu-id="f039b-139">**Activating Windows(R), ServerDatacenter edition (12345678-1234-1234-1234-12345678) … Product activated successfully.**</span></span>

## <a name="faq"></a><span data-ttu-id="f039b-140">GYIK</span><span class="sxs-lookup"><span data-stu-id="f039b-140">FAQ</span></span> 

### <a name="i-created-hello-windows-server-2016-from-azure-marketplace-do-i-need-tooconfigure-kms-key-for-activating-hello-windows-server-2016"></a><span data-ttu-id="f039b-141">Windows Server 2016 hello létrehozott Azure piactérről.</span><span class="sxs-lookup"><span data-stu-id="f039b-141">I created hello Windows Server 2016 from Azure Marketplace.</span></span> <span data-ttu-id="f039b-142">Szükséges tooconfigure KMS-kulcs aktiválásához hello Windows Server 2016?</span><span class="sxs-lookup"><span data-stu-id="f039b-142">Do I need tooconfigure KMS key for activating hello Windows Server 2016?</span></span> 
 
<span data-ttu-id="f039b-143">Nem.</span><span class="sxs-lookup"><span data-stu-id="f039b-143">No.</span></span> <span data-ttu-id="f039b-144">hello lemezképként az Azure piactér hello megfelelő KMS ügyfél telepítési kulcsának már konfigurálva van.</span><span class="sxs-lookup"><span data-stu-id="f039b-144">hello image in Azure Marketplace has hello appropriate KMS client setup key already configured.</span></span> 

### <a name="does-windows-activation-work-hello-same-way-regardless-if-hello-vm-is-using-azure-hybrid-use-benefit-hub-or-not"></a><span data-ttu-id="f039b-145">Nem a Windows folyamataktivációs munkahelyi hello azonos módon függetlenül attól, hogy ha hello virtuális gép által használt Azure hibrid használata juttatás (HUB), vagy nem?</span><span class="sxs-lookup"><span data-stu-id="f039b-145">Does Windows activation work hello same way regardless if hello VM is using Azure Hybrid Use Benefit (HUB) or not?</span></span> 
 
<span data-ttu-id="f039b-146">Igen.</span><span class="sxs-lookup"><span data-stu-id="f039b-146">Yes.</span></span> 
 
### <a name="what-happens-if-windows-activation-period-expires"></a><span data-ttu-id="f039b-147">Mi történik, ha a Windows aktiválási időszak lejár?</span><span class="sxs-lookup"><span data-stu-id="f039b-147">What happens if Windows activation period expires?</span></span> 
 
<span data-ttu-id="f039b-148">Ha még nincs aktiválva a Windows hello türelmi időszak lejárt, a Windows Server 2008 R2 és a Windows újabb verziói jelennek meg aktiválásával kapcsolatos további értesítések.</span><span class="sxs-lookup"><span data-stu-id="f039b-148">When hello grace period has expired and Windows is still not activated, Windows Server 2008 R2 and later versions of Windows will show additional notifications about activating.</span></span> <span data-ttu-id="f039b-149">hello tapéta fekete marad, és a Windows Update biztonsági és csak a kritikus frissítéseket, de nem választható frissítéseket telepíti.</span><span class="sxs-lookup"><span data-stu-id="f039b-149">hello desktop wallpaper remains black, and Windows Update will install security and critical updates only, but not optional updates.</span></span> <span data-ttu-id="f039b-150">Hello értesítést hello hello alján szakasz [licencelési feltételek](http://technet.microsoft.com/en-us/library/ff793403.aspx) lap.</span><span class="sxs-lookup"><span data-stu-id="f039b-150">See  hello Notifications section at hello bottom of hello [Licensing Conditions](http://technet.microsoft.com/en-us/library/ff793403.aspx) page.</span></span>   

## <a name="need-help-contact-support"></a><span data-ttu-id="f039b-151">Segítség</span><span class="sxs-lookup"><span data-stu-id="f039b-151">Need help?</span></span> <span data-ttu-id="f039b-152">Forduljon a támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="f039b-152">Contact support.</span></span>
<span data-ttu-id="f039b-153">Ha további segítségre van, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma megoldódik gyorsan tooget.</span><span class="sxs-lookup"><span data-stu-id="f039b-153">If you still need help, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget your issue resolved quickly.</span></span>
