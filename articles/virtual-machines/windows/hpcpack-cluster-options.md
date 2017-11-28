---
title: "HPC Pack aaaWindows fürt hello felhőben beállítások |} Microsoft Docs"
description: "További tudnivalók a Microsoft HPC Pack toocreate beállítások és a Windows nagy teljesítményű számítástechnikai hello Azure-felhőbe (HPC) fürt kezelése"
services: virtual-machines-windows,cloud-services,batch
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management,hpc-pack
ms.assetid: 02c5566d-2129-483c-9ecf-0d61030442d7
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 02/06/2017
ms.author: danlep
ms.openlocfilehash: 6158d9dd0cecda38b14f85a2b2080163f18e8cf8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="options-with-hpc-pack-toocreate-and-manage-a-windows-hpc-cluster-in-azure"></a><span data-ttu-id="bea68-103">A HPC Pack toocreate lehetőségek és az Azure-ban a Windows HPC-fürt kezeléséhez</span><span class="sxs-lookup"><span data-stu-id="bea68-103">Options with HPC Pack toocreate and manage a Windows HPC cluster in Azure</span></span>
[!INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

<span data-ttu-id="bea68-104">Ez a cikk foglalkozik a beállítások toocreate HPC Pack fürtök toorun Windows munkaterhelések.</span><span class="sxs-lookup"><span data-stu-id="bea68-104">This article focuses on options toocreate HPC Pack clusters toorun Windows workloads.</span></span> <span data-ttu-id="bea68-105">Módon is a létrehozásakor HPC Pack fürtök toorun [Linux HPC munkaterhelések](../linux/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="bea68-105">There are also options for creating HPC Pack clusters toorun [Linux HPC workloads](../linux/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>


## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a><span data-ttu-id="bea68-106">Az HPC Pack-fürt futtatására az Azure virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="bea68-106">Run an HPC Pack cluster in Azure VMs</span></span>
### <a name="azure-templates"></a><span data-ttu-id="bea68-107">Azure-sablonok</span><span class="sxs-lookup"><span data-stu-id="bea68-107">Azure templates</span></span>
* <span data-ttu-id="bea68-108">(GitHub) [HPC Pack 2016 fürt sablonok](https://github.com/MsHpcPack/HPCPack2016)</span><span class="sxs-lookup"><span data-stu-id="bea68-108">(GitHub) [HPC Pack 2016 cluster templates](https://github.com/MsHpcPack/HPCPack2016)</span></span>
* <span data-ttu-id="bea68-109">(Marketplace) [Munkaterhelések Windows HPC Pack fürt](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/)</span><span class="sxs-lookup"><span data-stu-id="bea68-109">(Marketplace) [HPC Pack cluster for Windows workloads](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/)</span></span>
* <span data-ttu-id="bea68-110">(Marketplace) [HPC Pack fürt Excel munkaterhelések](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterexcelcn/)</span><span class="sxs-lookup"><span data-stu-id="bea68-110">(Marketplace) [HPC Pack cluster for Excel workloads](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterexcelcn/)</span></span>
* <span data-ttu-id="bea68-111">(Gyors üzembe helyezés) [HPC-fürt létrehozása](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster)</span><span class="sxs-lookup"><span data-stu-id="bea68-111">(Quickstart) [Create an HPC cluster](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster)</span></span>
* <span data-ttu-id="bea68-112">(Gyors üzembe helyezés) [Egyéni számítási csomópont lemezképpel HPC-fürt létrehozása](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-custom-image)</span><span class="sxs-lookup"><span data-stu-id="bea68-112">(Quickstart) [Create an HPC cluster with custom compute node image](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-custom-image)</span></span>

### <a name="azure-vm-images"></a><span data-ttu-id="bea68-113">Az Azure Virtuálisgép-rendszerképek</span><span class="sxs-lookup"><span data-stu-id="bea68-113">Azure VM images</span></span>
* [<span data-ttu-id="bea68-114">Windows Server 2016 HPC Pack 2016 átjárócsomópontjához</span><span class="sxs-lookup"><span data-stu-id="bea68-114">HPC Pack 2016 head node on Windows Server 2016</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.HPCPack2016HeadNodeonWindowsServer2016?tab=Overview)
* [<span data-ttu-id="bea68-115">A Windows Server 2016 HPC Pack 2016 számítási csomópont</span><span class="sxs-lookup"><span data-stu-id="bea68-115">HPC Pack 2016 compute node on Windows Server 2016</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.HPCPack2016ComputeNodeonWindowsServer2016?tab=Overview)
* [<span data-ttu-id="bea68-116">HPC Pack 2016 átjárócsomópont Windows Server 2012 R2 rendszeren</span><span class="sxs-lookup"><span data-stu-id="bea68-116">HPC Pack 2016 head node on Windows Server 2012 R2</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.HPCPack2016HeadNodeonWindowsServer2012R2?tab=Overview)
* [<span data-ttu-id="bea68-117">HPC Pack 2016 számítási csomóponton a Windows Server 2012 R2 rendszeren</span><span class="sxs-lookup"><span data-stu-id="bea68-117">HPC Pack 2016 compute node on Windows Server 2012 R2</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.HPCPack2016ComputeNodeonWindowsServer2012R2?tab=Overview)
* [<span data-ttu-id="bea68-118">HPC Pack 2012 R2 átjárócsomópont Windows Server 2012 R2 rendszeren</span><span class="sxs-lookup"><span data-stu-id="bea68-118">HPC Pack 2012 R2 head node on Windows Server 2012 R2</span></span>](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/)
* [<span data-ttu-id="bea68-119">HPC Pack 2012 R2 számítási csomóponton a Windows Server 2012 R2 rendszeren</span><span class="sxs-lookup"><span data-stu-id="bea68-119">HPC Pack 2012 R2 compute node on Windows Server 2012 R2</span></span>](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodeonwindowsserver2012r2/)
* [<span data-ttu-id="bea68-120">HPC Pack számítási csomópont az Excel alkalmazással Windows Server 2012 R2 rendszeren</span><span class="sxs-lookup"><span data-stu-id="bea68-120">HPC Pack compute node with Excel on Windows Server 2012 R2</span></span>](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodewithexcelonwindowsserver2012r2/)

### <a name="powershell-deployment-script"></a><span data-ttu-id="bea68-121">PowerShell telepítési parancsfájlt</span><span class="sxs-lookup"><span data-stu-id="bea68-121">PowerShell deployment script</span></span>
* [<span data-ttu-id="bea68-122">HPC Pack IaaS telepítési parancsfájl hello HPC-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="bea68-122">Create an HPC cluster with hello HPC Pack IaaS deployment script</span></span>](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

### <a name="tutorials"></a><span data-ttu-id="bea68-123">oktatóanyagokat</span><span class="sxs-lookup"><span data-stu-id="bea68-123">Tutorials</span></span>
* [<span data-ttu-id="bea68-124">Oktatóanyag: HPC Pack 2016-fürt üzembe helyezése az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="bea68-124">Tutorial: Deploy an HPC Pack 2016 cluster in Azure</span></span>](hpcpack-2016-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="bea68-125">Oktatóanyag: Ismerkedés az Azure toorun Excel HPC Pack fürtöt és SOA munkaterhelések</span><span class="sxs-lookup"><span data-stu-id="bea68-125">Tutorial: Get started with an HPC Pack cluster in Azure toorun Excel and SOA workloads</span></span>](excel-cluster-hpcpack.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

### <a name="manual-deployment-with-hello-azure-portal"></a><span data-ttu-id="bea68-126">Az Azure-portálon hello manuális telepítése</span><span class="sxs-lookup"><span data-stu-id="bea68-126">Manual deployment with hello Azure portal</span></span>
* [<span data-ttu-id="bea68-127">Átjárócsomópont hello egy Azure virtuális gép HPC Pack fürt beállítása</span><span class="sxs-lookup"><span data-stu-id="bea68-127">Set up hello head node of an HPC Pack cluster in an Azure VM</span></span>](hpcpack-cluster-headnode.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

### <a name="cluster-management"></a><span data-ttu-id="bea68-128">Fürtkezelés</span><span class="sxs-lookup"><span data-stu-id="bea68-128">Cluster management</span></span>
* [<span data-ttu-id="bea68-129">Az Azure-ban HPC Pack-fürtben lévő számítási csomópontok kezelése</span><span class="sxs-lookup"><span data-stu-id="bea68-129">Manage compute nodes in an HPC Pack cluster in Azure</span></span>](classic/hpcpack-cluster-node-manage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [<span data-ttu-id="bea68-130">Növelhető vagy csökkenthető a HPC Pack-fürtben lévő Azure számítási erőforrásokat</span><span class="sxs-lookup"><span data-stu-id="bea68-130">Grow and shrink Azure compute resources in an HPC Pack cluster</span></span>](classic/hpcpack-cluster-node-autogrowshrink.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [<span data-ttu-id="bea68-131">Küldje el a feladatok tooan HPC Pack fürt az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="bea68-131">Submit jobs tooan HPC Pack cluster in Azure</span></span>](hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="bea68-132">HPC Pack feladatok kezelése</span><span class="sxs-lookup"><span data-stu-id="bea68-132">Job management in HPC Pack</span></span>](https://technet.microsoft.com/library/jj899585.aspx)
* [<span data-ttu-id="bea68-133">Az Azure szolgáltatásban az Azure Active Directory HPC Pack fürtöt kezelése</span><span class="sxs-lookup"><span data-stu-id="bea68-133">Manage an HPC Pack cluster in Azure with Azure Active Directory</span></span>](hpcpack-cluster-active-directory.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="add-worker-role-nodes-tooan-hpc-pack-cluster"></a><span data-ttu-id="bea68-134">Adja hozzá a feldolgozói szerepkör csomópontok tooan HPC Pack fürt</span><span class="sxs-lookup"><span data-stu-id="bea68-134">Add worker role nodes tooan HPC Pack cluster</span></span>
* [<span data-ttu-id="bea68-135">Kapacitásnövelés a HPC Pack tooAzure worker-példányok</span><span class="sxs-lookup"><span data-stu-id="bea68-135">Burst tooAzure worker instances with HPC Pack</span></span>](https://technet.microsoft.com/library/gg481749.aspx)
* [<span data-ttu-id="bea68-136">Oktatóanyag: Az Azure-ban HPC Pack hibrid fürt beállítása</span><span class="sxs-lookup"><span data-stu-id="bea68-136">Tutorial: Set up a hybrid cluster with HPC Pack in Azure</span></span>](../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)
* [<span data-ttu-id="bea68-137">Az Azure "kapacitásnövelés" csomópontok tooan HPC Pack központi csomópont hozzáadása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="bea68-137">Add Azure "burst" nodes tooan HPC Pack head node in Azure</span></span>](classic/hpcpack-cluster-node-burst.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="integrate-with-azure-batch"></a><span data-ttu-id="bea68-138">Az Azure Batch integrálása</span><span class="sxs-lookup"><span data-stu-id="bea68-138">Integrate with Azure Batch</span></span>
* [<span data-ttu-id="bea68-139">Kapacitásnövelés a HPC Pack kötegelt tooAzure</span><span class="sxs-lookup"><span data-stu-id="bea68-139">Burst tooAzure Batch with HPC Pack</span></span>](https://technet.microsoft.com/library/mt612877.aspx)

## <a name="create-rdma-clusters-for-mpi-workloads"></a><span data-ttu-id="bea68-140">MPI munkaterhelések RDMA-fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="bea68-140">Create RDMA clusters for MPI workloads</span></span>
* [<span data-ttu-id="bea68-141">HPC Pack toorun MPI alkalmazásokkal Windows RDMA fürt beállítása</span><span class="sxs-lookup"><span data-stu-id="bea68-141">Set up a Windows RDMA cluster with HPC Pack toorun MPI applications</span></span>](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

