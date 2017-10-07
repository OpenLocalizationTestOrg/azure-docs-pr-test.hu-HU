---
title: "az Azure-ban fürt aaaSubmit feladatok tooan HPC Pack |} Microsoft Docs"
description: "Ismerje meg, hogyan tooset be egy helyi számítógép toosubmit feladatok tooan HPC Pack fürt az Azure-ban"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management,hpc-pack
ms.assetid: 78f6833c-4aa6-4b3e-be71-97201abb4721
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 10/14/2016
ms.author: danlep
ms.openlocfilehash: 2918cf633917d8730487152e6a5ddb863eb8bb5e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="submit-hpc-jobs-from-an-on-premises-computer-tooan-hpc-pack-cluster-deployed-in-azure"></a><span data-ttu-id="2567a-103">A helyi számítógép tooan HPC Pack fürtök Azure szolgáltatásba telepített HPC feladatok elküldéséhez</span><span class="sxs-lookup"><span data-stu-id="2567a-103">Submit HPC jobs from an on-premises computer tooan HPC Pack cluster deployed in Azure</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="2567a-104">Konfigurálja a helyszíni ügyfél számítógép toosubmit feladatok tooa [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) fürt az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="2567a-104">Configure an on-premises client computer toosubmit jobs tooa [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) cluster in Azure.</span></span> <span data-ttu-id="2567a-105">Ez a cikk bemutatja, hogyan tooset ügyféllel a helyi számítógép eszközök keresztül HTTPS típusú fürt toohello az Azure-ban toosubmit feladat.</span><span class="sxs-lookup"><span data-stu-id="2567a-105">This article shows you how tooset up a local computer with client tools toosubmit job over HTTPS toohello cluster in Azure.</span></span> <span data-ttu-id="2567a-106">Ezzel a módszerrel több fürt felhasználók elküldheti a feladatok tooa felhőalapú HPC Pack fürt, de Csatlakozás közvetlenül toohello átjárócsomópont VM vagy Azure-előfizetéssel való hozzáférés nélkül.</span><span class="sxs-lookup"><span data-stu-id="2567a-106">In this way, several cluster users can submit jobs tooa cloud-based HPC Pack cluster, but without connecting directly toohello head node VM or accessing an Azure subscription.</span></span>

![Küldje el a feladat tooa fürt az Azure-ban][jobsubmit]

## <a name="prerequisites"></a><span data-ttu-id="2567a-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2567a-108">Prerequisites</span></span>
* <span data-ttu-id="2567a-109">**Egy Azure virtuális Gépen telepített HPC Pack átjárócsomópont** -azt javasoljuk, hogy az automatikus eszközeit használja, mint egy [Azure gyors üzembe helyezés sablon](https://azure.microsoft.com/documentation/templates/) vagy egy [Azure PowerShell-parancsfájl](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) toodeploy hello átjárócsomópont és fürt.</span><span class="sxs-lookup"><span data-stu-id="2567a-109">**HPC Pack head node deployed in an Azure VM** - We recommend that you use automated tools such as an [Azure quickstart template](https://azure.microsoft.com/documentation/templates/) or an [Azure PowerShell script](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) toodeploy hello head node and cluster.</span></span> <span data-ttu-id="2567a-110">Hello átjárócsomópont hello DNS-nevét és hello hitelesítő adatait. Ez a cikk hello végrehajtásához a fürt rendszergazdája van szüksége.</span><span class="sxs-lookup"><span data-stu-id="2567a-110">You need hello DNS name of hello head node and hello credentials of a cluster administrator to complete hello steps in this article.</span></span>
* <span data-ttu-id="2567a-111">**Ügyfélszámítógép** -HPC Pack ügyfél segédprogramok futtatható Windows vagy Windows Server ügyfél számítógépre van szüksége (lásd: [rendszerkövetelmények](https://technet.microsoft.com/library/dn535781.aspx)).</span><span class="sxs-lookup"><span data-stu-id="2567a-111">**Client computer** - You need a Windows or Windows Server client computer that can run HPC Pack client utilities (see [system requirements](https://technet.microsoft.com/library/dn535781.aspx)).</span></span> <span data-ttu-id="2567a-112">Ha csak szeretné toouse hello HPC Pack webes portál vagy a REST API toosubmit feladatokat, az Ön által választott bármely ügyfélszámítógép is használhatja.</span><span class="sxs-lookup"><span data-stu-id="2567a-112">If you only want toouse hello HPC Pack web portal or REST API toosubmit jobs, you can use any client computer of your choice.</span></span>
* <span data-ttu-id="2567a-113">**HPC Pack telepítési adathordozó** -tooinstall hello HPC Pack ügyfél segédprogramok hello szabad telepítési csomag, a HPC Pack (HPC Pack 2012 R2) legújabb verziója érhető el a [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024).</span><span class="sxs-lookup"><span data-stu-id="2567a-113">**HPC Pack installation media** - tooinstall hello HPC Pack client utilities, hello free installation package for the latest version of HPC Pack (HPC Pack 2012 R2) is available from the [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024).</span></span> <span data-ttu-id="2567a-114">Győződjön meg arról, hogy töltse le a hello hello átjárócsomópont VM telepített HPC Pack ugyanazt a verzióját.</span><span class="sxs-lookup"><span data-stu-id="2567a-114">Make sure that you download hello same version of HPC Pack that is installed on hello head node VM.</span></span>

## <a name="step-1-install-and-configure-hello-web-components-on-hello-head-node"></a><span data-ttu-id="2567a-115">1. lépés: A telepítésével és beállításával hello webösszetevők hello átjárócsomópont</span><span class="sxs-lookup"><span data-stu-id="2567a-115">Step 1: Install and configure hello web components on hello head node</span></span>
<span data-ttu-id="2567a-116">tooenable REST felület toosubmit feladatok toohello fürt a HTTPS PROTOKOLLOKON keresztül győződjön meg arról, hogy hello HPC Pack webösszetevők hello HPC Pack átjárócsomópont van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="2567a-116">tooenable a REST interface toosubmit jobs toohello cluster over HTTPS, ensure that hello HPC Pack web components are configured on hello HPC Pack head node.</span></span> <span data-ttu-id="2567a-117">Ha még nincsenek telepítve, először telepítenie hello webösszetevők hello HpcWebComponents.msi telepítési fájl futtatásával.</span><span class="sxs-lookup"><span data-stu-id="2567a-117">If they aren't already installed, first install hello web components by running hello HpcWebComponents.msi installation file.</span></span> <span data-ttu-id="2567a-118">Ezt követően konfigurálja a hello összetevők hello HPC PowerShell parancsfájl futtatásával **Set-HPCWebComponents.ps1**.</span><span class="sxs-lookup"><span data-stu-id="2567a-118">Then, configure hello components by running hello HPC PowerShell script **Set-HPCWebComponents.ps1**.</span></span>

<span data-ttu-id="2567a-119">Szükséges részletes eljárásokért lásd: [hello Microsoft HPC Pack webes összetevők telepítése](http://technet.microsoft.com/library/hh314627.aspx).</span><span class="sxs-lookup"><span data-stu-id="2567a-119">For detailed procedures, see [Install hello Microsoft HPC Pack Web Components](http://technet.microsoft.com/library/hh314627.aspx).</span></span>

> [!TIP]
> <span data-ttu-id="2567a-120">HPC Pack bizonyos Azure gyors üzembe helyezési sablonokat telepítse, és hello webösszetevők automatikusan konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="2567a-120">Certain Azure quickstart templates for HPC Pack install and configure hello web components automatically.</span></span> <span data-ttu-id="2567a-121">Ha hello [HPC Pack IaaS telepítési parancsfájl](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) toocreate hello fürt, lehetősége van telepítése és konfigurálása hello webösszetevők hello központi telepítésének részeként.</span><span class="sxs-lookup"><span data-stu-id="2567a-121">If you use hello [HPC Pack IaaS deployment script](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) toocreate hello cluster, you can optionally install and configure hello web components as part of hello deployment.</span></span>
> 
> 

<span data-ttu-id="2567a-122">**tooinstall hello webösszetevők**</span><span class="sxs-lookup"><span data-stu-id="2567a-122">**tooinstall hello web components**</span></span>

1. <span data-ttu-id="2567a-123">Csatlakozás toohello átjárócsomópont VM hello hitelesítő adatok a fürt rendszergazdai fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="2567a-123">Connect toohello head node VM by using hello credentials of a cluster administrator.</span></span>
2. <span data-ttu-id="2567a-124">Hello HPC Pack telepítési mappából futtassa a HpcWebComponents.msi hello központi csomóponton.</span><span class="sxs-lookup"><span data-stu-id="2567a-124">From hello HPC Pack Setup folder, run HpcWebComponents.msi on hello head node.</span></span>
3. <span data-ttu-id="2567a-125">Hello kövesse hello varázsló tooinstall hello webösszetevők</span><span class="sxs-lookup"><span data-stu-id="2567a-125">Follow hello steps in hello wizard tooinstall hello web components</span></span>

<span data-ttu-id="2567a-126">**tooconfigure hello webösszetevők**</span><span class="sxs-lookup"><span data-stu-id="2567a-126">**tooconfigure hello web components**</span></span>

1. <span data-ttu-id="2567a-127">Hello központi csomóponton indítsa el a HPC PowerShell rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="2567a-127">On hello head node, start HPC PowerShell as an administrator.</span></span>
2. <span data-ttu-id="2567a-128">toochange directory toohello helye hello konfigurációs parancsfájl típus hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="2567a-128">toochange directory toohello location of hello configuration script, type hello following command:</span></span>
   
    ```powershell
    cd $env:CCP_HOME\bin
    ```
3. <span data-ttu-id="2567a-129">tooconfigure hello REST-felületen, és indítsa el a hello HPC webes szolgáltatás, a következő parancs típusa hello:</span><span class="sxs-lookup"><span data-stu-id="2567a-129">tooconfigure hello REST interface and start hello HPC Web Service, type hello following command:</span></span>
   
    ```powershell
    .\Set-HPCWebComponents.ps1 –Service REST –enable
    ```
4. <span data-ttu-id="2567a-130">Amikor felszólító tooselect tanúsítvány, válasszon hello tanúsítványt, amely megfelel a hello átjárócsomópont toohello nyilvános DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="2567a-130">When prompted tooselect a certificate, choose hello certificate that corresponds toohello public DNS name of hello head node.</span></span> <span data-ttu-id="2567a-131">Például, ha Ön átjárócsomópont hello hello klasszikus üzembe helyezési modellel, tanúsítvány neve hello használó virtuális gépek néz CN =&lt;*HeadNodeDnsName*&gt;. cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="2567a-131">For example, if you deploy hello head node VM using hello classic deployment model, hello certificate name looks like CN=&lt;*HeadNodeDnsName*&gt;.cloudapp.net.</span></span> <span data-ttu-id="2567a-132">Hello Resource Manager üzembe helyezési modellben használatakor hello tanúsítvány neve néz CN =&lt;*HeadNodeDnsName*&gt;.&lt; *régió*&gt;. cloudapp.azure.com.</span><span class="sxs-lookup"><span data-stu-id="2567a-132">If you use hello Resource Manager deployment model, hello certificate name looks like CN=&lt;*HeadNodeDnsName*&gt;.&lt;*region*&gt;.cloudapp.azure.com.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="2567a-133">Újabb beállítást választja ezt a tanúsítványt a helyi számítógépről feladatok toohello átjárócsomópont elküldésekor.</span><span class="sxs-lookup"><span data-stu-id="2567a-133">You select this certificate later when you submit jobs toohello head node from an on-premises computer.</span></span> <span data-ttu-id="2567a-134">Ne válasszon, vagy adja meg egy tanúsítványt, amely megfelel a toohello számítógépneve hello átjárócsomópont hello Active Directory-tartományban (például: CN =*MyHPCHeadNode.HpcAzure.local*).</span><span class="sxs-lookup"><span data-stu-id="2567a-134">Don't select or configure a certificate that corresponds toohello computer name of hello head node in hello Active Directory domain (for example, CN=*MyHPCHeadNode.HpcAzure.local*).</span></span>
   > 
   > 
5. <span data-ttu-id="2567a-135">tooconfigure hello webes portál a feladat elküldése, típus hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="2567a-135">tooconfigure hello web portal for job submission, type hello following command:</span></span>
   
    ```powershell
    .\Set-HPCWebComponents.ps1 –Service Portal -enable
    ```
6. <span data-ttu-id="2567a-136">Hello parancsfájl befejezése után állítsa le és indítsa újra a HPC Job Feladatütemező szolgáltatás hello hello a következő parancsok beírásával:</span><span class="sxs-lookup"><span data-stu-id="2567a-136">After hello script completes, stop and restart hello HPC Job Scheduler Service by typing hello following commands:</span></span>
   
    ```powershell
    net stop hpcscheduler
    net start hpcscheduler
    ```

## <a name="step-2-install-hello-hpc-pack-client-utilities-on-an-on-premises-computer"></a><span data-ttu-id="2567a-137">2. lépés: Hello HPC Pack ügyfél segédeszközök telepítése a helyi számítógépen</span><span class="sxs-lookup"><span data-stu-id="2567a-137">Step 2: Install hello HPC Pack client utilities on an on-premises computer</span></span>
<span data-ttu-id="2567a-138">Ha azt szeretné, tooinstall hello HPC Pack ügyfél segédprogramok a számítógépen, le a HPC Pack telepítési fájlok (teljes telepítés) hello [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024).</span><span class="sxs-lookup"><span data-stu-id="2567a-138">If you want tooinstall hello HPC Pack client utilities on your computer, download the HPC Pack setup files (full installation) from hello [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024).</span></span> <span data-ttu-id="2567a-139">Ha hello a telepítés megkezdéséhez válassza hello telepítési beállítással a hello **HPC Pack ügyfél segédprogramok**.</span><span class="sxs-lookup"><span data-stu-id="2567a-139">When you begin hello installation, choose hello setup option for hello **HPC Pack client utilities**.</span></span>

<span data-ttu-id="2567a-140">toouse hello HPC Pack ügyféleszközök toosubmit feladatok toohello. átjárócsomópontjához VM, akkor is tooexport hello átjárócsomópont tanúsítványt kell, és telepítse azt hello ügyfélszámítógép.</span><span class="sxs-lookup"><span data-stu-id="2567a-140">toouse hello HPC Pack client tools toosubmit jobs toohello head node VM, you also need tooexport a certificate from hello head node and install it on hello client computer.</span></span> <span data-ttu-id="2567a-141">hello tanúsítványt kell lennie. CER formátumú.</span><span class="sxs-lookup"><span data-stu-id="2567a-141">hello certificate must be in .CER format.</span></span>

<span data-ttu-id="2567a-142">**hello átjárócsomópont tooexport hello tanúsítványt**</span><span class="sxs-lookup"><span data-stu-id="2567a-142">**tooexport hello certificate from hello head node**</span></span>

1. <span data-ttu-id="2567a-143">Hello átjárócsomópont vegye fel hello tanúsítványok beépülő modul tooa a Microsoft Management Console hello helyi számítógépfiók számára.</span><span class="sxs-lookup"><span data-stu-id="2567a-143">On hello head node, add hello Certificates snap-in tooa Microsoft Management Console for hello Local Computer account.</span></span> <span data-ttu-id="2567a-144">Tooadd hello beépülő modul lépéseiért lásd: [adja hozzá a tanúsítványok beépülő modul tooan hello MMC](https://technet.microsoft.com/library/cc754431.aspx).</span><span class="sxs-lookup"><span data-stu-id="2567a-144">For steps tooadd hello snap-in, see [Add hello Certificates Snap-in tooan MMC](https://technet.microsoft.com/library/cc754431.aspx).</span></span>
2. <span data-ttu-id="2567a-145">A hello a konzolfán bontsa ki a **tanúsítványok – helyi számítógép** > **személyes**, és kattintson a **tanúsítványok**.</span><span class="sxs-lookup"><span data-stu-id="2567a-145">In hello console tree, expand **Certificates – Local Computer** > **Personal**, and then click **Certificates**.</span></span>
3. <span data-ttu-id="2567a-146">Keresse meg a hello tanúsítvány hello HPC Pack webösszetevők a beállított [1. lépés: Telepítse és a webes összetevők hello konfigurálása a hello átjárócsomópont](#step-1:-install-and-configure-the-web-components-on-the-head-node) (például CN =&lt;*HeadNodeDnsName* &gt;. cloudapp.net).</span><span class="sxs-lookup"><span data-stu-id="2567a-146">Locate hello certificate that you configured for hello HPC Pack web components in [Step 1: Install and configure hello web components on hello head node](#step-1:-install-and-configure-the-web-components-on-the-head-node) (for example, CN=&lt;*HeadNodeDnsName*&gt;.cloudapp.net).</span></span>
4. <span data-ttu-id="2567a-147">Kattintson a jobb gombbal a hello tanúsítványt, és kattintson a **feladataival** > **exportálása**.</span><span class="sxs-lookup"><span data-stu-id="2567a-147">Right-click hello certificate, and click **All Tasks** > **Export**.</span></span>
5. <span data-ttu-id="2567a-148">Hello Tanúsítványexportáló varázslóban, kattintson **következő**, és győződjön meg arról, hogy **nem, nem exportálja a titkos kulcs hello** van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="2567a-148">In hello Certificate Export Wizard, click **Next**, and ensure that **No, do not export hello private key** is selected.</span></span>
6. <span data-ttu-id="2567a-149">Hajtsa végre hello hátralévő lépéseket hello varázsló tooexport hello tanúsítvány DER kódolású bináris X.509 (. CER) formátumban.</span><span class="sxs-lookup"><span data-stu-id="2567a-149">Follow hello remaining steps of hello wizard tooexport hello certificate in DER encoded binary X.509 (.CER) format.</span></span>

<span data-ttu-id="2567a-150">**tooimport hello tanúsítvány hello ügyfélszámítógépen**</span><span class="sxs-lookup"><span data-stu-id="2567a-150">**tooimport hello certificate on hello client computer**</span></span>

1. <span data-ttu-id="2567a-151">Másolja a hello mappából hello átjárócsomópont tooa hello ügyfélszámítógépen exportált tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="2567a-151">Copy hello certificate that you exported from hello head node tooa folder on hello client computer.</span></span>
2. <span data-ttu-id="2567a-152">Hello ügyfélszámítógépen certmgr.msc futtatásához.</span><span class="sxs-lookup"><span data-stu-id="2567a-152">On hello client computer, run certmgr.msc.</span></span>
3. <span data-ttu-id="2567a-153">A tanúsítvány-kezelőben bontsa ki a **tanúsítványok – aktuális felhasználó** > **megbízható legfelső szintű hitelesítésszolgáltatók**, kattintson a jobb gombbal **tanúsítványok**, és kattintson a **feladataival** > **importálási**.</span><span class="sxs-lookup"><span data-stu-id="2567a-153">In Certificate Manager, expand **Certificates – Current user** > **Trusted Root Certification Authorities**, right-click **Certificates**, and then click **All Tasks** > **Import**.</span></span>
4. <span data-ttu-id="2567a-154">Hello Tanúsítványimportáló varázslóban, kattintson **következő** és kövesse hello lépéseket tooimport hello exportált tanúsítványt a megbízható legfelső szintű hitelesítésszolgáltatók tárolására hello átjárócsomópont toohello.</span><span class="sxs-lookup"><span data-stu-id="2567a-154">In hello Certificate Import Wizard, click **Next** and follow hello steps tooimport hello certificate that you exported from hello head node toohello Trusted Root Certification Authorities store.</span></span>

> [!TIP]
> <span data-ttu-id="2567a-155">Láthatja a biztonsági figyelmeztetést, mert hello hitelesítésszolgáltatójától hello átjárócsomópont nem ismerik fel a hello ügyfélszámítógép.</span><span class="sxs-lookup"><span data-stu-id="2567a-155">You might see a security warning, because hello certification authority on hello head node isn't recognized by hello client computer.</span></span> <span data-ttu-id="2567a-156">Tesztelési célokra, figyelmen kívül hagyhatja a figyelmeztetési és a teljes hello tanúsítvány importálása.</span><span class="sxs-lookup"><span data-stu-id="2567a-156">For testing purposes, you can ignore this warning and complete hello certificate import.</span></span>
> 
> 

## <a name="step-3-run-test-jobs-on-hello-cluster"></a><span data-ttu-id="2567a-157">3. lépés: Futtatás Tesztfeladatok hello fürtön</span><span class="sxs-lookup"><span data-stu-id="2567a-157">Step 3: Run test jobs on hello cluster</span></span>
<span data-ttu-id="2567a-158">az Azure-ban a hello a fürt a konfigurációt, próbálkozzon egy futó feladat hello tooverify a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="2567a-158">tooverify your configuration, try running jobs on hello cluster in Azure from hello on-premises computer.</span></span> <span data-ttu-id="2567a-159">Használhatja például a HPC Pack grafikus eszközöket vagy a parancssori parancsokat toosubmit feladatok toohello fürt.</span><span class="sxs-lookup"><span data-stu-id="2567a-159">For example, you can use HPC Pack GUI tools or command-line commands toosubmit jobs toohello cluster.</span></span> <span data-ttu-id="2567a-160">Egy webes portál toosubmit feladatok is használható.</span><span class="sxs-lookup"><span data-stu-id="2567a-160">You can also use a web-based portal toosubmit jobs.</span></span>

<span data-ttu-id="2567a-161">**toorun feladat elküldése parancsok hello ügyfélszámítógépen**</span><span class="sxs-lookup"><span data-stu-id="2567a-161">**toorun job submission commands on hello client computer**</span></span>

1. <span data-ttu-id="2567a-162">Egy ügyfélszámítógépen, amelyen telepítve vannak-e az hello HPC Pack ügyfél segédprogramok egy parancssor elindításához.</span><span class="sxs-lookup"><span data-stu-id="2567a-162">On a client computer where hello HPC Pack client utilities are installed, start a Command Prompt.</span></span>
2. <span data-ttu-id="2567a-163">Írja be egy minta parancsot.</span><span class="sxs-lookup"><span data-stu-id="2567a-163">Type a sample command.</span></span> <span data-ttu-id="2567a-164">Toolist hello fürtön futó összes feladatot írja be például a parancs a következő, attól függően, hogy a teljes DNS-nevét hello hello átjárócsomópont hello hasonló tooone:</span><span class="sxs-lookup"><span data-stu-id="2567a-164">For example, toolist all jobs on hello cluster, type a command similar tooone of hello following, depending on hello full DNS name of hello head node:</span></span>
   
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.cloudapp.net /all
    ```
   
    <span data-ttu-id="2567a-165">vagy</span><span class="sxs-lookup"><span data-stu-id="2567a-165">or</span></span>
   
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.<region>.cloudapp.azure.com /all
    ```
   
   > [!TIP]
   > <span data-ttu-id="2567a-166">Teljes DNS-nevét hello hello átjárócsomópont, ne hello IP-címet, hello Feladatütemező URL-címet használja.</span><span class="sxs-lookup"><span data-stu-id="2567a-166">Use hello full DNS name of hello head node, not hello IP address, in hello scheduler URL.</span></span> <span data-ttu-id="2567a-167">Ha hello IP-címet ad meg, egy hibaüzenet jelenik meg, hasonló túl "hello tanúsítvány szükséges a tooeither egy érvényes lánc megbízhatósági vagy hello megbízható főtanúsítványainak tárolójába helyezi toobe rendelkezik."</span><span class="sxs-lookup"><span data-stu-id="2567a-167">If you specify hello IP address, an error appears similar too"hello server certificate needs tooeither have a valid chain of trust or toobe placed in hello trusted root store."</span></span>
   > 
   > 
3. <span data-ttu-id="2567a-168">Amikor a rendszer kéri, írja be a hello felhasználónevet (hello formában &lt;tartománynév&gt;\\&lt;felhasználónév&gt;) és a jelszó hello HPC-fürt rendszergazdája vagy egy másik fürthöz felhasználó konfigurált.</span><span class="sxs-lookup"><span data-stu-id="2567a-168">When prompted, type hello user name (in hello form &lt;DomainName&gt;\\&lt;UserName&gt;) and password of hello HPC cluster administrator or another cluster user that you configured.</span></span> <span data-ttu-id="2567a-169">Választhat toostore hello hitelesítő adatok helyben több feladat műveletet.</span><span class="sxs-lookup"><span data-stu-id="2567a-169">You can choose toostore hello credentials locally for more job operations.</span></span>
   
    <span data-ttu-id="2567a-170">Feladatok listája jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="2567a-170">A list of jobs appears.</span></span>

<span data-ttu-id="2567a-171">**a HPC Job Manager toouse hello ügyfélszámítógépen**</span><span class="sxs-lookup"><span data-stu-id="2567a-171">**toouse HPC Job Manager on hello client computer**</span></span>

1. <span data-ttu-id="2567a-172">Ha korábban egy fürt felhasználó tartományi hitelesítő adatok nem tárolja, amikor elküld egy feladatot, a hitelesítőadat-kezelőben hello hitelesítő adatokat adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="2567a-172">If you didn't previously store domain credentials for a cluster user when submitting a job, you can add hello credentials in Credential Manager.</span></span>
   
    <span data-ttu-id="2567a-173">a.</span><span class="sxs-lookup"><span data-stu-id="2567a-173">a.</span></span> <span data-ttu-id="2567a-174">A Vezérlőpult hello ügyfélszámítógépen indítsa el a hitelesítőadat-kezelő.</span><span class="sxs-lookup"><span data-stu-id="2567a-174">In Control Panel on hello client computer, start Credential Manager.</span></span>
   
    <span data-ttu-id="2567a-175">b.</span><span class="sxs-lookup"><span data-stu-id="2567a-175">b.</span></span> <span data-ttu-id="2567a-176">Kattintson a **Windows hitelesítő adatok** > **egy általános hitelesítő adatok hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="2567a-176">Click **Windows Credentials** > **Add a generic credential**.</span></span>
   
    <span data-ttu-id="2567a-177">c.</span><span class="sxs-lookup"><span data-stu-id="2567a-177">c.</span></span> <span data-ttu-id="2567a-178">Adja meg a hello internetcím (például https://&lt;HeadNodeDnsName&gt;.cloudapp.net/HpcScheduler vagy a https://&lt;HeadNodeDnsName&gt;.&lt; régió&gt;.cloudapp.azure.com/HpcScheduler), és hello felhasználónevét (&lt;tartománynév&gt;\\&lt;felhasználónév&gt;) és a jelszó hello fürt rendszergazdája vagy egy másik fürt felhasználói konfigurált.</span><span class="sxs-lookup"><span data-stu-id="2567a-178">Specify hello Internet address (for example, https://&lt;HeadNodeDnsName&gt;.cloudapp.net/HpcScheduler or https://&lt;HeadNodeDnsName&gt;.&lt;region&gt;.cloudapp.azure.com/HpcScheduler), and hello user name (&lt;DomainName&gt;\\&lt;UserName&gt;) and password of hello cluster administrator or another cluster user that you configured.</span></span>
2. <span data-ttu-id="2567a-179">Hello ügyfélszámítógépen indítsa el a HPC Job Manager használatát.</span><span class="sxs-lookup"><span data-stu-id="2567a-179">On hello client computer, start HPC Job Manager.</span></span>
3. <span data-ttu-id="2567a-180">A hello **Head csomópont kiválasztása** párbeszédpanelen típus hello URL-cím toohello átjárócsomópont az Azure-ban (például https://&lt;HeadNodeDnsName&gt;. cloudapp.net vagy a https://&lt;HeadNodeDnsName&gt;. &lt;régió&gt;. cloudapp.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2567a-180">In hello **Select Head Node** dialog box, type hello URL toohello head node in Azure (for example, https://&lt;HeadNodeDnsName&gt;.cloudapp.net or https://&lt;HeadNodeDnsName&gt;.&lt;region&gt;.cloudapp.azure.com).</span></span>
   
    <span data-ttu-id="2567a-181">A HPC Job Manager megnyílik, és hello központi csomóponton lévő feladatok listáját jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="2567a-181">HPC Job Manager opens and shows a list of jobs on hello head node.</span></span>

<span data-ttu-id="2567a-182">**hello központi csomóponton futó toouse hello webportál**</span><span class="sxs-lookup"><span data-stu-id="2567a-182">**toouse hello web portal running on hello head node**</span></span>

1. <span data-ttu-id="2567a-183">Indítsa el a webböngésző hello ügyfélszámítógépen, és adja meg a következő címeket, attól függően, hogy a teljes DNS-nevét hello hello átjárócsomópont hello egyikét:</span><span class="sxs-lookup"><span data-stu-id="2567a-183">Start a web browser on hello client computer, and enter one of hello following addresses, depending on hello full DNS name of hello head node:</span></span>
   
    ```
    https://<HeadNodeDnsName>.cloudapp.net/HpcPortal
    ```
   
    <span data-ttu-id="2567a-184">vagy</span><span class="sxs-lookup"><span data-stu-id="2567a-184">or</span></span>
   
    ```
    https://<HeadNodeDnsName>.<region>.cloudapp.azure.com/HpcPortal
    ```
2. <span data-ttu-id="2567a-185">Hello biztonsági megjelenő párbeszédpanelen írja be a HPC-fürt rendszergazdája hello hello tartományi hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="2567a-185">In hello security dialog box that appears, type hello domain credentials of hello HPC cluster administrator.</span></span> <span data-ttu-id="2567a-186">(Is hozzáadhat más fürt felhasználók különböző szerepkörök.</span><span class="sxs-lookup"><span data-stu-id="2567a-186">(You can also add other cluster users in different roles.</span></span> <span data-ttu-id="2567a-187">Lásd: [fürt felhasználók kezelése](https://technet.microsoft.com/library/ff919335.aspx).)</span><span class="sxs-lookup"><span data-stu-id="2567a-187">See [Managing Cluster Users](https://technet.microsoft.com/library/ff919335.aspx).)</span></span>
   
    <span data-ttu-id="2567a-188">hello webportál toohello feladat lista nézetének megnyitása.</span><span class="sxs-lookup"><span data-stu-id="2567a-188">hello web portal opens toohello job list view.</span></span>
3. <span data-ttu-id="2567a-189">egy minta feladatot, amely karakterláncot ad vissza, hello "Hello World" hello fürtből toosubmit kattintson **új feladat** hello bal oldali navigációs sáv.</span><span class="sxs-lookup"><span data-stu-id="2567a-189">toosubmit a sample job that returns hello string “Hello World” from hello cluster, click **New job** in hello left-hand navigation.</span></span>
4. <span data-ttu-id="2567a-190">A hello **új feladat** lap **küldésének lapjáról**, kattintson a **HelloWorld**.</span><span class="sxs-lookup"><span data-stu-id="2567a-190">On hello **New Job** page, under **From submission pages**, click **HelloWorld**.</span></span> <span data-ttu-id="2567a-191">hello feladat elküldése lap jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="2567a-191">hello job submission page appears.</span></span>
5. <span data-ttu-id="2567a-192">Kattintson a **nyújt**.</span><span class="sxs-lookup"><span data-stu-id="2567a-192">Click **Submit**.</span></span> <span data-ttu-id="2567a-193">Ha a rendszer kéri, adja meg a HPC-fürt rendszergazdája hello hello tartományi hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="2567a-193">If prompted, provide hello domain credentials of hello HPC cluster administrator.</span></span> <span data-ttu-id="2567a-194">hello feladatot küld, és hello Feladatazonosító jelenik meg hello **saját feladatok** lap.</span><span class="sxs-lookup"><span data-stu-id="2567a-194">hello job is submitted, and hello job ID appears on hello **My Jobs** page.</span></span>
6. <span data-ttu-id="2567a-195">tooview hello eredmények hello feladat küldő, kattintson hello Feladatazonosító, majd **nézet feladatai** tooview hello parancs kimenetében (alatt **kimeneti**).</span><span class="sxs-lookup"><span data-stu-id="2567a-195">tooview hello results of hello job that you submitted, click hello job ID, and then click **View Tasks** tooview hello command output (under **Output**).</span></span>

## <a name="next-steps"></a><span data-ttu-id="2567a-196">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2567a-196">Next steps</span></span>
* <span data-ttu-id="2567a-197">Is elküldheti a feladatok toohello hello Azure fürt [HPC Pack REST API](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx).</span><span class="sxs-lookup"><span data-stu-id="2567a-197">You can also submit jobs toohello Azure cluster with hello [HPC Pack REST API](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx).</span></span>
* <span data-ttu-id="2567a-198">Ha azt szeretné, hogy a Linux-ügyfél toosubmit fürt feladatait, tekintse meg a hello Python hello a minta [HPC Pack 2012 R2 SDK és mintakód](https://www.microsoft.com/download/details.aspx?id=41633).</span><span class="sxs-lookup"><span data-stu-id="2567a-198">If you want toosubmit cluster jobs from a Linux client, see hello Python sample in hello [HPC Pack 2012 R2 SDK and Sample Code](https://www.microsoft.com/download/details.aspx?id=41633).</span></span>

<!--Image references-->
[jobsubmit]: ./media/virtual-machines-windows-hpcpack-cluster-submit-jobs/jobsubmit.png
