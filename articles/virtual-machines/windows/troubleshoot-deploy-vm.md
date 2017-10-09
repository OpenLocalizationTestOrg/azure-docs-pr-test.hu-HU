---
title: "Windows virtuális gépekkel kapcsolatos problémákat, az Azure-ban telepítése aaaTroubleshoot |} Microsoft Docs"
description: "Problémamegoldás telepítését Windows virtuális gép Azurethe Resource Manager üzembe helyezési modellben."
services: virtual-machines-windows
documentationcenter: 
author: genlin
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4e383427-4aff-4bf3-a0f4-dbff5c6f0c81
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: genli
ms.openlocfilehash: 30de25827c050cc266761cfc14548bcc64237dac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deploying-windows-virtual-machine-issues-in-azure"></a><span data-ttu-id="fe706-103">Problémamegoldás telepítését Windows virtuális gép az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="fe706-103">Troubleshoot deploying Windows virtual machine issues in Azure</span></span>

<span data-ttu-id="fe706-104">tootroubleshoot virtuális gép (VM) telepítési problémákat Azure, tekintse át a hello [leggyakoribb problémák](#top-issues) a gyakori hibák és megoldására.</span><span class="sxs-lookup"><span data-stu-id="fe706-104">tootroubleshoot virtual machine (VM) deployment issues in Azure, review hello [top issues](#top-issues) for common failures and resolutions.</span></span>

<span data-ttu-id="fe706-105">Ha ez a cikk bármely pontján további segítségre van szüksége, forduljon az Azure-szakértők hello [MSDN Azure és a Stack Overflow fórumok hello](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="fe706-105">If you need more help at any point in this article, you can contact hello Azure experts on [hello MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="fe706-106">Másik lehetőségként is fájl az Azure támogatási incidens.</span><span class="sxs-lookup"><span data-stu-id="fe706-106">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="fe706-107">Nyissa meg toohello [az Azure támogatási webhelyén](https://azure.microsoft.com/support/options/) válassza ki **támogatja az beszerzése**.</span><span class="sxs-lookup"><span data-stu-id="fe706-107">Go toohello [Azure support site](https://azure.microsoft.com/support/options/) and select **Get Support**.</span></span>

## <a name="top-issues"></a><span data-ttu-id="fe706-108">Problémák</span><span class="sxs-lookup"><span data-stu-id="fe706-108">Top issues</span></span>
[!INCLUDE [virtual-machines-windows-troubleshoot-deploy-vm-top](../../../includes/virtual-machines-windows-troubleshoot-deploy-vm-top.md)]

## <a name="hello-cluster-cannot-support-hello-requested-vm-size"></a><span data-ttu-id="fe706-109">hello fürt nem támogatja a kért Virtuálisgép-méretet hello</span><span class="sxs-lookup"><span data-stu-id="fe706-109">hello cluster cannot support hello requested VM size</span></span>
<properties
supportTopicIds="123456789"
resourceTags="windows"
productPesIds="1234, 5678"
/>
- <span data-ttu-id="fe706-110">Próbálja megismételni a kérelmet hello kisebb Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="fe706-110">Retry hello request using a smaller VM size.</span></span>
- <span data-ttu-id="fe706-111">Ha hello a hello mérete nem lehet módosítani a virtuális gép kért:</span><span class="sxs-lookup"><span data-stu-id="fe706-111">If hello size of hello requested VM cannot be changed:</span></span>
    - <span data-ttu-id="fe706-112">Állítsa le a rendelkezésre állási csoport hello hello virtuális gépeinek.</span><span class="sxs-lookup"><span data-stu-id="fe706-112">Stop all hello VMs in hello availability set.</span></span> <span data-ttu-id="fe706-113">Kattintson a **erőforráscsoportok** > az erőforráscsoport > **erőforrások** > a rendelkezésre állási csoport > **virtuális gépek** > a virtuális gép > **leállítása**.</span><span class="sxs-lookup"><span data-stu-id="fe706-113">Click **Resource groups** > your resource group > **Resources** > your availability set > **Virtual Machines** > your virtual machine > **Stop**.</span></span>
    - <span data-ttu-id="fe706-114">Amikor az összes virtuális gépek leállítási hello, és hello VM szükséges hello mérete.</span><span class="sxs-lookup"><span data-stu-id="fe706-114">After all hello VMs stop, create hello VM in hello desired size.</span></span>
    - <span data-ttu-id="fe706-115">Indítsa el először hello új virtuális Gépet, és jelölje hello virtuális gépek leállítása, majd válassza az Indítás parancsot.</span><span class="sxs-lookup"><span data-stu-id="fe706-115">Start hello new VM first, and then select each of hello stopped VMs and click Start.</span></span>


## <a name="hello-cluster-does-not-have-free-resources"></a><span data-ttu-id="fe706-116">hello fürtnek nincs szabad erőforrást</span><span class="sxs-lookup"><span data-stu-id="fe706-116">hello cluster does not have free resources</span></span>
<properties
supportTopicIds="123456789"
resourceTags="windows"
productPesIds="1234, 5678"
/>
- <span data-ttu-id="fe706-117">Próbálkozzon újra később hello kéréssel.</span><span class="sxs-lookup"><span data-stu-id="fe706-117">Retry hello request later.</span></span>
- <span data-ttu-id="fe706-118">Ha hello új virtuális gép is része egy másik rendelkezésre állási beállítani</span><span class="sxs-lookup"><span data-stu-id="fe706-118">If hello new VM can be part of a different availability set</span></span>
    - <span data-ttu-id="fe706-119">Hozzon létre egy virtuális Gépet egy másik rendelkezésre állási készlet (a hello ugyanabban a régióban).</span><span class="sxs-lookup"><span data-stu-id="fe706-119">Create a VM in a different availability set (in hello same region).</span></span>
    - <span data-ttu-id="fe706-120">Adja hozzá a hello új virtuális gép toohello ugyanazt a virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="fe706-120">Add hello new VM toohello same virtual network.</span></span>

## <a name="how-can-i-use-and-deploy-a-windows-client-image-into-azure"></a><span data-ttu-id="fe706-121">Hogyan használja és az Azure windows ügyfél lemezkép telepítéséhez?</span><span class="sxs-lookup"><span data-stu-id="fe706-121">How can I use and deploy a windows client image into Azure?</span></span>

<span data-ttu-id="fe706-122">Használhatja a Windows 7, Windows 8 vagy Windows 10 az Azure-ban fejlesztési/Tesztelési forgatókönyvek ha megfelelő (korábbi nevén MSDN) Visual Studio-előfizetéssel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="fe706-122">You can use Windows 7, Windows 8, or Windows 10 in Azure for dev/test scenarios if you have an appropriate Visual Studio (formerly MSDN) subscription.</span></span> <span data-ttu-id="fe706-123">Ez [cikk](client-images.md) vázol fel hello Windows-ügyféllel rendelkező Azure-ban és felhasználási területei hello Azure gyűjtemény lemezképei jogosultsági követelményei.</span><span class="sxs-lookup"><span data-stu-id="fe706-123">This [article](client-images.md) outlines hello eligibility requirements for running Windows client in Azure and uses of hello Azure Gallery images.</span></span>

## <a name="how-can-i-deploy-a-virtual-machine-using-hello-hybrid-use-benefit-hub"></a><span data-ttu-id="fe706-124">Hogyan telepítheti a virtuális gépek hello hibrid használata juttatás (HUB)?</span><span class="sxs-lookup"><span data-stu-id="fe706-124">How can I deploy a virtual machine using hello Hybrid Use Benefit (HUB)?</span></span>

<span data-ttu-id="fe706-125">Többféle különböző módokon toodeploy Windows virtuális gépek hello Azure hibrid használata juttatásra.</span><span class="sxs-lookup"><span data-stu-id="fe706-125">There are a couple of different ways toodeploy Windows virtual machines with hello Azure Hybrid Use Benefit.</span></span>

<span data-ttu-id="fe706-126">Egy nagyvállalati szerződés előfizetéshez:</span><span class="sxs-lookup"><span data-stu-id="fe706-126">For an Enterprise Agreement subscription:</span></span>

<span data-ttu-id="fe706-127">• Az adott piactéren elérhető rendszerkép, amelyek az Azure hibrid használata juttatás előre konfigurált virtuális gépek telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="fe706-127">•   Deploy VMs from specific Marketplace images that are pre-configured with Azure Hybrid Use Benefit.</span></span>

<span data-ttu-id="fe706-128">A nagyvállalati szerződés:</span><span class="sxs-lookup"><span data-stu-id="fe706-128">For Enterprise agreement:</span></span>

<span data-ttu-id="fe706-129">• Feltöltése egy egyéni virtuális Gépet, és telepítése a Resource Manager-sablon vagy Azure PowerShell segítségével.</span><span class="sxs-lookup"><span data-stu-id="fe706-129">•   Upload a custom VM and deploy using a Resource Manager template or Azure PowerShell.</span></span>

<span data-ttu-id="fe706-130">További információkért tekintse meg a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="fe706-130">For more information, see hello following resources:</span></span>

 - [<span data-ttu-id="fe706-131">Az Azure hibrid használata juttatás áttekintése</span><span class="sxs-lookup"><span data-stu-id="fe706-131">Azure Hybrid Use Benefit overview </span></span>](https://azure.microsoft.com/pricing/hybrid-use-benefit/)

 - [<span data-ttu-id="fe706-132">Letölthető – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="fe706-132">Downloadable FAQ</span></span>](http://download.microsoft.com/download/4/2/1/4211AC94-D607-4A45-B472-4B30EDF437DE/Windows_Server_Azure_Hybrid_Use_FAQ_EN_US.pdf)

 - <span data-ttu-id="fe706-133">[Windows Server és a Windows ügyfél Azure hibrid használata juttatás](hybrid-use-benefit-licensing.md).</span><span class="sxs-lookup"><span data-stu-id="fe706-133">[Azure Hybrid Use Benefit for Windows Server and Windows Client](hybrid-use-benefit-licensing.md).</span></span>

 - [<span data-ttu-id="fe706-134">Hogyan használhatom hello hibrid használja juttatás az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="fe706-134">How can I use hello Hybrid Use Benefit in Azure</span></span>](https://blogs.msdn.microsoft.com/azureedu/2016/04/13/how-can-i-use-the-hybrid-use-benefit-in-azure)

## <a name="how-do-i-activate-my-monthly-credit-for-visual-studio-enterprise-bizspark"></a><span data-ttu-id="fe706-135">Hogyan aktiválhatja a Visual Studio Enterprise (BizSpark) a havi keretet</span><span class="sxs-lookup"><span data-stu-id="fe706-135">How do I activate my monthly credit for Visual studio Enterprise (BizSpark)</span></span>

<span data-ttu-id="fe706-136">tooactivate a havi jóváírása, ez [cikk](https://azure.microsoft.com/offers/ms-azr-0064p/).</span><span class="sxs-lookup"><span data-stu-id="fe706-136">tooactivate your monthly  credit, see this [article](https://azure.microsoft.com/offers/ms-azr-0064p/).</span></span>

## <a name="how-tooadd-enterprise-devtest-toomy-enterprise-agreement-ea-tooget-access-toowindow-client-images"></a><span data-ttu-id="fe706-137">Tooadd vállalati fejlesztési és tesztelési célú toomy nagyvállalati szerződés (EA) tooget miként férhetnek hozzá az tooWindow ügyfél képek?</span><span class="sxs-lookup"><span data-stu-id="fe706-137">How tooadd Enterprise Dev/Test toomy Enterprise Agreement (EA) tooget access tooWindow client images?</span></span>

<span data-ttu-id="fe706-138">hello képességét toocreate előfizetések hello vállalati fejlesztési és tesztelési célú alapján kínálnak engedélye toodo kezelhet korlátozott tooAccount tulajdonosai a vállalati rendszergazda igen.</span><span class="sxs-lookup"><span data-stu-id="fe706-138">hello ability toocreate subscriptions based on hello Enterprise Dev/Test offer is restricted tooAccount Owners who have been given permission toodo so by an Enterprise Administrator.</span></span> <span data-ttu-id="fe706-139">hello fiók tulajdonosának hello Azure fiók portálon keresztül előfizetések hoz létre, és kell adja aktív Visual Studio-előfizetők társadminisztrátorként.</span><span class="sxs-lookup"><span data-stu-id="fe706-139">hello Account Owner creates subscriptions via hello Azure Account Portal, and then should add active Visual Studio subscribers as co-administrators.</span></span> <span data-ttu-id="fe706-140">Így azok kezelése, és használja a fejlesztéshez és teszteléshez szükséges hello erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="fe706-140">So that they can manage and use hello resources needed for development and testing.</span></span> <span data-ttu-id="fe706-141">További információkért lásd: [vállalati fejlesztési és tesztelési célú](https://azure.microsoft.com/offers/ms-azr-0148p/).</span><span class="sxs-lookup"><span data-stu-id="fe706-141">For more information, see [Enterprise Dev/Test](https://azure.microsoft.com/offers/ms-azr-0148p/).</span></span>

## <a name="my-drivers-are-missing-for-my-windows-n-series-vm"></a><span data-ttu-id="fe706-142">Az illesztőprogramok hiányoznak a Windows-N sorozatú virtuális Gépemhez</span><span class="sxs-lookup"><span data-stu-id="fe706-142">My drivers are missing for my Windows N-Series VM</span></span>

<span data-ttu-id="fe706-143">Illesztőprogramok Windows-alapú virtuális gépek találhatók [Itt](n-series-driver-setup.md).</span><span class="sxs-lookup"><span data-stu-id="fe706-143">Drivers for Windows-based VMs are located [here](n-series-driver-setup.md).</span></span>

## <a name="i-cant-find-a-gpu-instance-within-my-n-series-vm"></a><span data-ttu-id="fe706-144">A GPU példánya nem található a N sorozatú virtuális Gépen belül</span><span class="sxs-lookup"><span data-stu-id="fe706-144">I can’t find a GPU instance within my N-Series VM</span></span>

<span data-ttu-id="fe706-145">tootake előny az Azure N sorozatú virtuális gépeket futtathatnak Windows Server 2016 hello GPU lehetőségeit vagy Windows Server 2012 R2, telepítenie kell NVIDIA video-illesztőprogramok a virtuális gépek telepítést követően.</span><span class="sxs-lookup"><span data-stu-id="fe706-145">tootake advantage of hello GPU capabilities of Azure N-series VMs running Windows Server 2016 or Windows Server 2012 R2, you must install NVIDIA graphics drivers on each VM after deployment.</span></span> <span data-ttu-id="fe706-146">Illesztőprogram-telepítő információ [Windows virtuális gépek](n-series-driver-setup.md) és [Linux virtuális gépek](../linux/n-series-driver-setup.md).</span><span class="sxs-lookup"><span data-stu-id="fe706-146">Driver setup information is available for [Windows VMs](n-series-driver-setup.md) and [Linux VMs](../linux/n-series-driver-setup.md).</span></span>

## <a name="are-client-images-supported-for-n-series"></a><span data-ttu-id="fe706-147">Ügyfél képek N-adatsorozathoz támogatottak?</span><span class="sxs-lookup"><span data-stu-id="fe706-147">Are client images supported for N-Series?</span></span>

<span data-ttu-id="fe706-148">Jelenleg Azure csak támogatja az N-sorozat a Windows Server és Linux operációs rendszert futtató virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="fe706-148">Currently, Azure only supports N-Series on VMs running Windows Server and Linux operating systems.</span></span>

## <a name="is-n-series-vms-available-in-my-region"></a><span data-ttu-id="fe706-149">Érhető el N sorozatú virtuális gépek régiómban?</span><span class="sxs-lookup"><span data-stu-id="fe706-149">Is N-Series VMs available in my region?</span></span>

<span data-ttu-id="fe706-150">Ellenőrizheti a hello rendelkezésre biztosít hello [régió tábla által elérhető termékek](https://azure.microsoft.com/regions/services), és az árképzés terén [Itt](https://azure.microsoft.com/pricing/details/virtual-machines/series/#n-series).</span><span class="sxs-lookup"><span data-stu-id="fe706-150">You can check hello availability from hello [Products available by region table](https://azure.microsoft.com/regions/services), and pricing [here](https://azure.microsoft.com/pricing/details/virtual-machines/series/#n-series).</span></span>

## <a name="what-client-images-can-i-use-and-deploy-in-azure-and-how-tooi-get-them"></a><span data-ttu-id="fe706-151">Milyen ügyfél képek is I használata és telepítése az Azure-ban, és hogyan tooI be őket?</span><span class="sxs-lookup"><span data-stu-id="fe706-151">What client images can I use and deploy in Azure, and how tooI get them?</span></span>

<span data-ttu-id="fe706-152">Használhatja a Windows 7, Windows 8 vagy Windows 10 fejlesztési és tesztelési célú forgatókönyvek az Azure-ban biztosított megfelelő (korábbi nevén MSDN) Visual Studio-előfizetéssel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="fe706-152">You can use Windows 7, Windows 8, or Windows 10 in Azure for dev/test scenarios provided you have an appropriate Visual Studio (formerly MSDN) subscription.</span></span> 

- <span data-ttu-id="fe706-153">Windows 10-lemezképek elérhetők belül hello Azure gyűjteményből [jogosult fejlesztési és tesztelési célú kínál](client-images.md#eligible-offers).</span><span class="sxs-lookup"><span data-stu-id="fe706-153">Windows 10 images are available from hello Azure Gallery within [eligible dev/test offers](client-images.md#eligible-offers).</span></span> 
- <span data-ttu-id="fe706-154">A Visual Studio-előfizetők ajánlat bármilyen típusú belül is [megfelelően készítse elő és hozzon létre](prepare-for-upload-vhd-image.md) egy 64 bites Windows 7, Windows 8 vagy Windows 10-lemezképet, majd [tooAzure feltöltése](upload-generalized-managed.md).</span><span class="sxs-lookup"><span data-stu-id="fe706-154">Visual Studio subscribers within any type of offer can also [adequately prepare and create](prepare-for-upload-vhd-image.md) a 64-bit Windows 7, Windows 8, or Windows 10 image and then [upload tooAzure](upload-generalized-managed.md).</span></span> <span data-ttu-id="fe706-155">hello használata korlátozott toodev/test aktív Visual Studio-előfizetők által marad.</span><span class="sxs-lookup"><span data-stu-id="fe706-155">hello use remains limited toodev/test by active Visual Studio subscribers.</span></span>

<span data-ttu-id="fe706-156">Ez [cikk](client-images.md) vázol fel hello jogosultsági követelményei Windows-ügyféllel rendelkező Azure-ban és hello használata Azure-katalógus képek.</span><span class="sxs-lookup"><span data-stu-id="fe706-156">This [article](client-images.md) outlines hello eligibility requirements for running Windows client in Azure and use of hello Azure Gallery images.</span></span>

## <a name="i-am-not-able-toosee-vm-size-family-that-i-want-when-resizing-my-vm"></a><span data-ttu-id="fe706-157">A számítógépen nem tudja toosee Virtuálisgép-méretet termékcsalád, amelyek szeretnék, ha a virtuális gép átméretezésével.</span><span class="sxs-lookup"><span data-stu-id="fe706-157">I am not able toosee VM Size family that I want when resizing my VM.</span></span>

<span data-ttu-id="fe706-158">Amikor egy virtuális gép fut, nem telepített tooa fizikai kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="fe706-158">When a VM is running, it is deployed tooa physical server.</span></span> <span data-ttu-id="fe706-159">hello fizikai kiszolgálók Azure-régiók közös fizikai hardver fürtök vannak csoportosítva.</span><span class="sxs-lookup"><span data-stu-id="fe706-159">hello physical servers in Azure regions are grouped in clusters of common physical hardware.</span></span> <span data-ttu-id="fe706-160">A hello VM áthelyezése toobe toodifferent hardver fürtök igénylő virtuális gépek átméretezésével eltér attól függően, hogy melyik üzembe helyezési modellel használt toodeploy hello VM volt.</span><span class="sxs-lookup"><span data-stu-id="fe706-160">Resizing a VM that requires hello VM toobe moved toodifferent hardware clusters is different depending on which deployment model was used toodeploy hello VM.</span></span>

- <span data-ttu-id="fe706-161">Klasszikus üzembe helyezési modellel, hello felhő szolgáltatástelepítés telepített virtuális gépek el kell távolítani, és toochange hello virtuális gépek tooa mérete egy másik mérete termékcsalád újratelepíteni.</span><span class="sxs-lookup"><span data-stu-id="fe706-161">VMs deployed in Classic deployment model, hello cloud service deployment must be removed and redeployed toochange hello VMs tooa size in another size family.</span></span>

- <span data-ttu-id="fe706-162">Erőforrás-kezelő üzembe helyezési modellben telepített virtuális gépek, le kell állítania minden virtuális gép hello rendelkezésre állási csoportban, a virtuális gép hello rendelkezésre állási készlet hello méretének módosítása előtt.</span><span class="sxs-lookup"><span data-stu-id="fe706-162">VMs deployed in Resource Manager deployment model, you must stop all VMs in hello availability set before changing hello size of any VM in hello availability set.</span></span>

## <a name="hello-listed-vm-size-is-not-supported-while-deploying-in-availability-set"></a><span data-ttu-id="fe706-163">hello felsorolt Virtuálisgép-méret nem támogatott a rendelkezésre állási csoport központi telepítése során.</span><span class="sxs-lookup"><span data-stu-id="fe706-163">hello listed VM size is not supported while deploying in Availability Set.</span></span>

<span data-ttu-id="fe706-164">Hello rendelkezésre állási csoport fürtön támogatott méret kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="fe706-164">Choose a size that is supported on hello availability set's cluster.</span></span> <span data-ttu-id="fe706-165">Ajánlott létrehozásakor rendelkezésre állási készlet toochoose hello legnagyobb Virtuálisgép-méretet úgy gondolja, hogy kell, és rendelkezik, amelyek az első központi telepítési toohello rendelkezésre állási csoportban.</span><span class="sxs-lookup"><span data-stu-id="fe706-165">It is recommended when creating an availability set toochoose hello largest VM size you think you need, and have that be your first deployment toohello Availability set.</span></span>

## <a name="can-i-add-an-existing-classic-vm-tooan-availability-set"></a><span data-ttu-id="fe706-166">Adhat hozzá egy meglévő klasszikus virtuális gép tooan rendelkezésre állási csoportot?</span><span class="sxs-lookup"><span data-stu-id="fe706-166">Can I add an existing Classic VM tooan availability set?</span></span>

<span data-ttu-id="fe706-167">Igen.</span><span class="sxs-lookup"><span data-stu-id="fe706-167">Yes.</span></span> <span data-ttu-id="fe706-168">Hozzáadhat egy meglévő klasszikus virtuális gép tooa új vagy meglévő rendelkezésre állási csoportban.</span><span class="sxs-lookup"><span data-stu-id="fe706-168">You can add an existing classic VM tooa new or existing Availability Set.</span></span> <span data-ttu-id="fe706-169">További információ: [adja hozzá a virtuális gép tooan rendelkezésre állási készlet](classic/configure-availability.md#addmachine).</span><span class="sxs-lookup"><span data-stu-id="fe706-169">For more information see [Add an existing virtual machine tooan availability set](classic/configure-availability.md#addmachine).</span></span>


## <a name="next-steps"></a><span data-ttu-id="fe706-170">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fe706-170">Next steps</span></span>
<span data-ttu-id="fe706-171">Ha ez a cikk bármely pontján további segítségre van szüksége, forduljon az Azure-szakértők hello [MSDN Azure és a Stack Overflow fórumok hello](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="fe706-171">If you need more help at any point in this article, you can contact hello Azure experts on [hello MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/support/forums/).</span></span>

<span data-ttu-id="fe706-172">Másik lehetőségként is fájl az Azure támogatási incidens.</span><span class="sxs-lookup"><span data-stu-id="fe706-172">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="fe706-173">Nyissa meg toohello [az Azure támogatási webhelyén](https://azure.microsoft.com/support/options/) válassza ki **támogatja az beszerzése**.</span><span class="sxs-lookup"><span data-stu-id="fe706-173">Go toohello [Azure support site](https://azure.microsoft.com/support/options/) and select **Get Support**.</span></span>
