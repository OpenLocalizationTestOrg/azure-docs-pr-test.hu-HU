---
title: "Linux virtuális gépekkel kapcsolatos problémákat, az Azure-ban telepítése aaaTroubleshoot |} Microsoft Docs"
description: "Problémamegoldás telepítését Linux virtuális gép Azurethe Resource Manager üzembe helyezési modellben."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
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
ms.openlocfilehash: d1092ca3d9d51af7510b19c8c9a79e6dda588697
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deploying-linux-virtual-machine-issues-in-azure"></a><span data-ttu-id="ee84c-103">Problémamegoldás telepítését Linux virtuális gép az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="ee84c-103">Troubleshoot deploying Linux virtual machine issues in Azure</span></span>

<span data-ttu-id="ee84c-104">tootroubleshoot virtuális gép (VM) telepítési problémákat Azure, tekintse át a hello [leggyakoribb problémák](#top-issues) a gyakori hibák és megoldására.</span><span class="sxs-lookup"><span data-stu-id="ee84c-104">tootroubleshoot virtual machine (VM) deployment issues in Azure, review hello [top issues](#top-issues) for common failures and resolutions.</span></span>

<span data-ttu-id="ee84c-105">Ha ez a cikk bármely pontján további segítségre van szüksége, forduljon az Azure-szakértők hello [MSDN Azure és a Stack Overflow fórumok hello](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="ee84c-105">If you need more help at any point in this article, you can contact hello Azure experts on [hello MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="ee84c-106">Másik lehetőségként is fájl az Azure támogatási incidens.</span><span class="sxs-lookup"><span data-stu-id="ee84c-106">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="ee84c-107">Nyissa meg toohello [az Azure támogatási webhelyén](https://azure.microsoft.com/support/options/) válassza ki **támogatja az beszerzése**.</span><span class="sxs-lookup"><span data-stu-id="ee84c-107">Go toohello [Azure support site](https://azure.microsoft.com/support/options/) and select **Get Support**.</span></span>

## <a name="top-issues"></a><span data-ttu-id="ee84c-108">Problémák</span><span class="sxs-lookup"><span data-stu-id="ee84c-108">Top issues</span></span>
[!INCLUDE [virtual-machines-linux-troubleshoot-deploy-vm-top](../../../includes/virtual-machines-linux-troubleshoot-deploy-vm-top.md)]

## <a name="hello-cluster-cannot-support-hello-requested-vm-size"></a><span data-ttu-id="ee84c-109">hello fürt nem támogatja a kért Virtuálisgép-méretet hello</span><span class="sxs-lookup"><span data-stu-id="ee84c-109">hello cluster cannot support hello requested VM size</span></span>
<properties
supportTopicIds="123456789"
resourceTags="windows"
productPesIds="1234, 5678"
/>
- <span data-ttu-id="ee84c-110">Próbálja megismételni a kérelmet hello kisebb Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="ee84c-110">Retry hello request using a smaller VM size.</span></span>
- <span data-ttu-id="ee84c-111">Ha hello a hello mérete nem lehet módosítani a virtuális gép kért:</span><span class="sxs-lookup"><span data-stu-id="ee84c-111">If hello size of hello requested VM cannot be changed:</span></span>
    - <span data-ttu-id="ee84c-112">Állítsa le a rendelkezésre állási csoport hello hello virtuális gépeinek.</span><span class="sxs-lookup"><span data-stu-id="ee84c-112">Stop all hello VMs in hello availability set.</span></span> <span data-ttu-id="ee84c-113">Kattintson a **erőforráscsoportok** > az erőforráscsoport > **erőforrások** > a rendelkezésre állási csoport > **virtuális gépek** > a virtuális gép > **leállítása**.</span><span class="sxs-lookup"><span data-stu-id="ee84c-113">Click **Resource groups** > your resource group > **Resources** > your availability set > **Virtual Machines** > your virtual machine > **Stop**.</span></span>
    - <span data-ttu-id="ee84c-114">Amikor az összes virtuális gépek leállítási hello, és hello VM szükséges hello mérete.</span><span class="sxs-lookup"><span data-stu-id="ee84c-114">After all hello VMs stop, create hello VM in hello desired size.</span></span>
    - <span data-ttu-id="ee84c-115">Indítsa el először hello új virtuális Gépet, és jelölje hello virtuális gépek leállítása, majd válassza az Indítás parancsot.</span><span class="sxs-lookup"><span data-stu-id="ee84c-115">Start hello new VM first, and then select each of hello stopped VMs and click Start.</span></span>


## <a name="hello-cluster-does-not-have-free-resources"></a><span data-ttu-id="ee84c-116">hello fürtnek nincs szabad erőforrást</span><span class="sxs-lookup"><span data-stu-id="ee84c-116">hello cluster does not have free resources</span></span>
<properties
supportTopicIds="123456789"
resourceTags="windows"
productPesIds="1234, 5678"
/>
- <span data-ttu-id="ee84c-117">Próbálkozzon újra később hello kéréssel.</span><span class="sxs-lookup"><span data-stu-id="ee84c-117">Retry hello request later.</span></span>
- <span data-ttu-id="ee84c-118">Ha hello új virtuális gép is része egy másik rendelkezésre állási beállítani</span><span class="sxs-lookup"><span data-stu-id="ee84c-118">If hello new VM can be part of a different availability set</span></span>
    - <span data-ttu-id="ee84c-119">Hozzon létre egy virtuális Gépet egy másik rendelkezésre állási készlet (a hello ugyanabban a régióban).</span><span class="sxs-lookup"><span data-stu-id="ee84c-119">Create a VM in a different availability set (in hello same region).</span></span>
    - <span data-ttu-id="ee84c-120">Adja hozzá a hello új virtuális gép toohello ugyanazt a virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="ee84c-120">Add hello new VM toohello same virtual network.</span></span>

## <a name="how-do-i-activate-my-monthly-credit-for-visual-studio-enterprise-bizspark"></a><span data-ttu-id="ee84c-121">Hogyan aktiválhatja a Visual Studio Enterprise (BizSpark) a havi keretet</span><span class="sxs-lookup"><span data-stu-id="ee84c-121">How do I activate my monthly credit for Visual studio Enterprise (BizSpark)</span></span>

<span data-ttu-id="ee84c-122">tooactivate a havi jóváírása, ez [cikk](https://azure.microsoft.com/offers/ms-azr-0064p/).</span><span class="sxs-lookup"><span data-stu-id="ee84c-122">tooactivate your monthly  credit, see this [article](https://azure.microsoft.com/offers/ms-azr-0064p/).</span></span>

## <a name="why-can-i-not-install-hello-gpu-driver-for-an-ubuntu-nv-vm"></a><span data-ttu-id="ee84c-123">Miért nem telepíthetők hello GPU illesztőprogramjának portok HV Ubuntu virtuális gép?</span><span class="sxs-lookup"><span data-stu-id="ee84c-123">Why can I not install hello GPU driver for an Ubuntu NV VM?</span></span>

<span data-ttu-id="ee84c-124">Linux GPU támogatja jelenleg csak Azure virtuális gépeken NC Ubuntu Server 16.04 LTS futtató érhető el.</span><span class="sxs-lookup"><span data-stu-id="ee84c-124">Currently, Linux GPU support is only available on Azure NC VMs running Ubuntu Server 16.04 LTS.</span></span> <span data-ttu-id="ee84c-125">További információkért lásd: [N-sorozat linuxos virtuális gépek GPU illesztőprogramok beállítása](n-series-driver-setup.md).</span><span class="sxs-lookup"><span data-stu-id="ee84c-125">For more information, see [Set up GPU drivers for N-series VMs running Linux](n-series-driver-setup.md).</span></span>

## <a name="my-drivers-are-missing-for-my-linux-n-series-vm"></a><span data-ttu-id="ee84c-126">Az illesztőprogramok hiányoznak a Linux N sorozatú virtuális Gépemhez</span><span class="sxs-lookup"><span data-stu-id="ee84c-126">My drivers are missing for my Linux N-Series VM</span></span>

<span data-ttu-id="ee84c-127">Linux-alapú virtuális gépek illesztőprogramok találhatók [Itt](n-series-driver-setup.md).</span><span class="sxs-lookup"><span data-stu-id="ee84c-127">Drivers for Linux-based VMs are located [here](n-series-driver-setup.md).</span></span> 

## <a name="i-cant-find-a-gpu-instance-within-my-n-series-vm"></a><span data-ttu-id="ee84c-128">A GPU példánya nem található a N sorozatú virtuális Gépen belül</span><span class="sxs-lookup"><span data-stu-id="ee84c-128">I can’t find a GPU instance within my N-Series VM</span></span>

<span data-ttu-id="ee84c-129">tootake előny az Azure N sorozatú virtuális gépeket futtathatnak Windows Server 2016 hello GPU lehetőségeit vagy Windows Server 2012 R2, telepítenie kell NVIDIA video-illesztőprogramok a virtuális gépek telepítést követően.</span><span class="sxs-lookup"><span data-stu-id="ee84c-129">tootake advantage of hello GPU capabilities of Azure N-series VMs running Windows Server 2016 or Windows Server 2012 R2, you must install NVIDIA graphics drivers on each VM after deployment.</span></span> <span data-ttu-id="ee84c-130">Illesztőprogram-telepítő információ [Windows virtuális gépek](../windows/n-series-driver-setup.md) és [Linux virtuális gépek](n-series-driver-setup.md).</span><span class="sxs-lookup"><span data-stu-id="ee84c-130">Driver setup information is available for [Windows VMs](../windows/n-series-driver-setup.md) and [Linux VMs](n-series-driver-setup.md).</span></span>

## <a name="is-n-series-vms-available-in-my-region"></a><span data-ttu-id="ee84c-131">Érhető el N sorozatú virtuális gépek régiómban?</span><span class="sxs-lookup"><span data-stu-id="ee84c-131">Is N-Series VMs available in my region?</span></span>

<span data-ttu-id="ee84c-132">Ellenőrizheti a hello rendelkezésre biztosít hello [régió tábla által elérhető termékek](https://azure.microsoft.com/regions/services), és az árképzés terén [Itt](https://azure.microsoft.com/pricing/details/virtual-machines/series/#n-series).</span><span class="sxs-lookup"><span data-stu-id="ee84c-132">You can check hello availability from hello [Products available by region table](https://azure.microsoft.com/regions/services), and pricing [here](https://azure.microsoft.com/pricing/details/virtual-machines/series/#n-series).</span></span>

## <a name="i-am-not-able-toosee-vm-size-family-that-i-want-when-resizing-my-vm"></a><span data-ttu-id="ee84c-133">A számítógépen nem tudja toosee Virtuálisgép-méretet termékcsalád, amelyek szeretnék, ha a virtuális gép átméretezésével.</span><span class="sxs-lookup"><span data-stu-id="ee84c-133">I am not able toosee VM Size family that I want when resizing my VM.</span></span>

<span data-ttu-id="ee84c-134">Amikor egy virtuális gép fut, nem telepített tooa fizikai kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="ee84c-134">When a VM is running, it is deployed tooa physical server.</span></span> <span data-ttu-id="ee84c-135">hello fizikai kiszolgálók Azure-régiók közös fizikai hardver fürtök vannak csoportosítva.</span><span class="sxs-lookup"><span data-stu-id="ee84c-135">hello physical servers in Azure regions are grouped in clusters of common physical hardware.</span></span> <span data-ttu-id="ee84c-136">A hello VM áthelyezése toobe toodifferent hardver fürtök igénylő virtuális gépek átméretezésével eltér attól függően, hogy melyik üzembe helyezési modellel használt toodeploy hello VM volt.</span><span class="sxs-lookup"><span data-stu-id="ee84c-136">Resizing a VM that requires hello VM toobe moved toodifferent hardware clusters is different depending on which deployment model was used toodeploy hello VM.</span></span>

- <span data-ttu-id="ee84c-137">Klasszikus üzembe helyezési modellel, hello felhő szolgáltatástelepítés telepített virtuális gépek el kell távolítani, és toochange hello virtuális gépek tooa mérete egy másik mérete termékcsalád újratelepíteni.</span><span class="sxs-lookup"><span data-stu-id="ee84c-137">VMs deployed in Classic deployment model, hello cloud service deployment must be removed and redeployed toochange hello VMs tooa size in another size family.</span></span>

- <span data-ttu-id="ee84c-138">Erőforrás-kezelő üzembe helyezési modellben telepített virtuális gépek, le kell állítania minden virtuális gép hello rendelkezésre állási csoportban, a virtuális gép hello rendelkezésre állási készlet hello méretének módosítása előtt.</span><span class="sxs-lookup"><span data-stu-id="ee84c-138">VMs deployed in Resource Manager deployment model, you must stop all VMs in hello availability set before changing hello size of any VM in hello availability set.</span></span>

## <a name="hello-listed-vm-size-is-not-supported-while-deploying-in-availability-set"></a><span data-ttu-id="ee84c-139">hello felsorolt Virtuálisgép-méret nem támogatott a rendelkezésre állási csoport központi telepítése során.</span><span class="sxs-lookup"><span data-stu-id="ee84c-139">hello listed VM size is not supported while deploying in Availability Set.</span></span>

<span data-ttu-id="ee84c-140">Hello rendelkezésre állási csoport fürtön támogatott méret kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="ee84c-140">Choose a size that is supported on hello availability set's cluster.</span></span> <span data-ttu-id="ee84c-141">Ajánlott létrehozásakor rendelkezésre állási készlet toochoose hello legnagyobb Virtuálisgép-méretet úgy gondolja, hogy kell, és rendelkezik, amelyek az első központi telepítési toohello rendelkezésre állási csoportban.</span><span class="sxs-lookup"><span data-stu-id="ee84c-141">It is recommended when creating an availability set toochoose hello largest VM size you think you need, and have that be your first deployment toohello Availability set.</span></span>

## <a name="what-linux-distributionsversions-are-supported-on-azure"></a><span data-ttu-id="ee84c-142">Milyen Linux terjesztéseket/verziók támogatottak az Azure-on?</span><span class="sxs-lookup"><span data-stu-id="ee84c-142">What Linux distributions/versions are supported on Azure?</span></span>

<span data-ttu-id="ee84c-143">Hello listáján, a Linux található [Azure-Endorsed Terjesztéseket](endorsed-distros.md).</span><span class="sxs-lookup"><span data-stu-id="ee84c-143">You can find hello list at Linux on [Azure-Endorsed Distributions](endorsed-distros.md).</span></span>

## <a name="can-i-add-an-existing-classic-vm-tooan-availability-set"></a><span data-ttu-id="ee84c-144">Adhat hozzá egy meglévő klasszikus virtuális gép tooan rendelkezésre állási csoportot?</span><span class="sxs-lookup"><span data-stu-id="ee84c-144">Can I add an existing Classic VM tooan availability set?</span></span>

<span data-ttu-id="ee84c-145">Igen.</span><span class="sxs-lookup"><span data-stu-id="ee84c-145">Yes.</span></span> <span data-ttu-id="ee84c-146">Hozzáadhat egy meglévő klasszikus virtuális gép tooa új vagy meglévő rendelkezésre állási csoportban.</span><span class="sxs-lookup"><span data-stu-id="ee84c-146">You can add an existing classic VM tooa new or existing Availability Set.</span></span> <span data-ttu-id="ee84c-147">További információ: [adja hozzá a virtuális gép tooan rendelkezésre állási készlet](../windows/classic/configure-availability.md#addmachine).</span><span class="sxs-lookup"><span data-stu-id="ee84c-147">For more information see [Add an existing virtual machine tooan availability set](../windows/classic/configure-availability.md#addmachine).</span></span>


## <a name="next-steps"></a><span data-ttu-id="ee84c-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ee84c-148">Next steps</span></span>
<span data-ttu-id="ee84c-149">Ha ez a cikk bármely pontján további segítségre van szüksége, forduljon az Azure-szakértők hello [MSDN Azure és a Stack Overflow fórumok hello](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="ee84c-149">If you need more help at any point in this article, you can contact hello Azure experts on [hello MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/support/forums/).</span></span>

<span data-ttu-id="ee84c-150">Másik lehetőségként is fájl az Azure támogatási incidens.</span><span class="sxs-lookup"><span data-stu-id="ee84c-150">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="ee84c-151">Nyissa meg toohello [az Azure támogatási webhelyén](https://azure.microsoft.com/support/options/) válassza ki **támogatja az beszerzése**.</span><span class="sxs-lookup"><span data-stu-id="ee84c-151">Go toohello [Azure support site](https://azure.microsoft.com/support/options/) and select **Get Support**.</span></span>
