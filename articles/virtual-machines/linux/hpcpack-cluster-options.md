---
title: "Linux HPC Pack fürt beállításai a felhőben |} Microsoft Docs"
description: "További tudnivalók a Microsoft HPC Pack létrehozása és kezelése a Linux nagy teljesítményű számítástechnikai (HPC) fürt Azure felhőben beállítások"
services: virtual-machines-linux,cloud-services
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management,hpc-pack
ms.assetid: ac60624e-aefa-40c3-8bc1-ef6d5c0ef1a2
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 02/06/2017
ms.author: danlep
ms.openlocfilehash: 616006d29149f7f47b03bd127cf3c83ad63a5c39
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="options-with-hpc-pack-to-create-and-manage-an-hpc-cluster-in-azure-for-linux-workloads"></a><span data-ttu-id="b5b8c-103">HPC Pack létrehozása és kezelése a Linux-munkaterhelések az Azure-ban HPC-fürt beállítások</span><span class="sxs-lookup"><span data-stu-id="b5b8c-103">Options with HPC Pack to create and manage an HPC cluster in Azure for Linux workloads</span></span>
[!INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

<span data-ttu-id="b5b8c-104">Ez a cikk foglalkozik Linux alkalmazásokat és szolgáltatásokat futtathatnak HPC Pack használatával a beállítások.</span><span class="sxs-lookup"><span data-stu-id="b5b8c-104">This article focuses on options to use HPC Pack to run Linux workloads.</span></span> <span data-ttu-id="b5b8c-105">Módon is futtatásához [Windows HPC-munkaterhelések HPC Pack](../windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b5b8c-105">There are also options for running [Windows HPC workloads with HPC Pack](../windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a><span data-ttu-id="b5b8c-106">Az HPC Pack-fürt futtatására az Azure virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="b5b8c-106">Run an HPC Pack cluster in Azure VMs</span></span>
### <a name="azure-templates"></a><span data-ttu-id="b5b8c-107">Azure-sablonok</span><span class="sxs-lookup"><span data-stu-id="b5b8c-107">Azure templates</span></span>
* <span data-ttu-id="b5b8c-108">(GitHub) [HPC Pack 2016 fürt sablonok](https://github.com/MsHpcPack/HPCPack2016)</span><span class="sxs-lookup"><span data-stu-id="b5b8c-108">(GitHub) [HPC Pack 2016 cluster templates](https://github.com/MsHpcPack/HPCPack2016)</span></span>
* <span data-ttu-id="b5b8c-109">(Marketplace) [HPC Pack fürt Linux munkaterhelések](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/)</span><span class="sxs-lookup"><span data-stu-id="b5b8c-109">(Marketplace) [HPC Pack cluster for Linux workloads](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/)</span></span>
* <span data-ttu-id="b5b8c-110">(Gyors üzembe helyezés) [Linux számítási csomópontok HPC-fürt létrehozása](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-linux-cn)</span><span class="sxs-lookup"><span data-stu-id="b5b8c-110">(Quickstart) [Create an HPC cluster with Linux compute nodes](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-linux-cn)</span></span>

### <a name="powershell-deployment-script"></a><span data-ttu-id="b5b8c-111">PowerShell telepítési parancsfájlt</span><span class="sxs-lookup"><span data-stu-id="b5b8c-111">PowerShell deployment script</span></span>
* [<span data-ttu-id="b5b8c-112">Hozzon létre egy Linux HPC-fürtöt a HPC Pack IaaS telepítési parancsfájl</span><span class="sxs-lookup"><span data-stu-id="b5b8c-112">Create a Linux HPC cluster with the HPC Pack IaaS deployment script</span></span>](../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="tutorials"></a><span data-ttu-id="b5b8c-113">oktatóanyagokat</span><span class="sxs-lookup"><span data-stu-id="b5b8c-113">Tutorials</span></span>
* [<span data-ttu-id="b5b8c-114">Oktatóanyag: Megismerkedés a Linux számítási csomópontok HPC Pack fürtben az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="b5b8c-114">Tutorial: Get started with Linux compute nodes in an HPC Pack cluster in Azure</span></span>](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [<span data-ttu-id="b5b8c-115">Oktatóanyag: Futtassa a Microsoft HPC Pack NAMD Linux számítási csomópontok az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="b5b8c-115">Tutorial: Run NAMD with Microsoft HPC Pack on Linux compute nodes in Azure</span></span>](classic/hpcpack-cluster-namd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [<span data-ttu-id="b5b8c-116">Oktatóanyag: Futtatás OpenFOAM Microsoft HPC Pack Azure Linux RDMA fürtön</span><span class="sxs-lookup"><span data-stu-id="b5b8c-116">Tutorial: Run OpenFOAM with Microsoft HPC Pack on a Linux RDMA cluster in Azure</span></span>](classic/hpcpack-cluster-openfoam.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [<span data-ttu-id="b5b8c-117">Oktatóanyag: Futtatás csillag-CCM + Microsoft HPC Pack egy Linux RDMA a fürt az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="b5b8c-117">Tutorial: Run STAR-CCM+ with Microsoft HPC Pack on a Linux RDMA cluster in Azure</span></span>](classic/hpcpack-cluster-starccm.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="cluster-management"></a><span data-ttu-id="b5b8c-118">Fürtkezelés</span><span class="sxs-lookup"><span data-stu-id="b5b8c-118">Cluster management</span></span>
* [<span data-ttu-id="b5b8c-119">Az Azure-ban HPC Pack fürthöz feladatok elküldéséhez</span><span class="sxs-lookup"><span data-stu-id="b5b8c-119">Submit jobs to an HPC Pack cluster in Azure</span></span>](../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="b5b8c-120">HPC Pack feladatok kezelése</span><span class="sxs-lookup"><span data-stu-id="b5b8c-120">Job management in HPC Pack</span></span>](https://technet.microsoft.com/library/jj899585.aspx)

## <a name="create-rdma-clusters-for-mpi-workloads"></a><span data-ttu-id="b5b8c-121">MPI munkaterhelések RDMA-fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="b5b8c-121">Create RDMA clusters for MPI workloads</span></span>
* [<span data-ttu-id="b5b8c-122">Oktatóanyag: Futtatás OpenFOAM Microsoft HPC Pack Azure Linux RDMA fürtön</span><span class="sxs-lookup"><span data-stu-id="b5b8c-122">Tutorial: Run OpenFOAM with Microsoft HPC Pack on a Linux RDMA cluster in Azure</span></span>](classic/hpcpack-cluster-openfoam.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [<span data-ttu-id="b5b8c-123">MPI-alkalmazások futtatására Linux RDMA fürt beállítása</span><span class="sxs-lookup"><span data-stu-id="b5b8c-123">Set up a Linux RDMA cluster to run MPI applications</span></span>](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

