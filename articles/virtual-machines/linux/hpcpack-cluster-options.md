---
title: "aaaLinux HPC Pack fürt beállításai hello felhőben |} Microsoft Docs"
description: "További tudnivalók a Microsoft HPC Pack toocreate beállítások és a Linux nagy teljesítményű számítástechnikai hello Azure-felhőbe (HPC) fürt kezelése"
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
ms.openlocfilehash: 60d093b466f44a0391815842c421c328e8c7a0d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="options-with-hpc-pack-toocreate-and-manage-an-hpc-cluster-in-azure-for-linux-workloads"></a><span data-ttu-id="3f76a-103">A HPC Pack toocreate beállítások és a Linux-munkaterhelések az Azure-ban HPC-fürt kezelése</span><span class="sxs-lookup"><span data-stu-id="3f76a-103">Options with HPC Pack toocreate and manage an HPC cluster in Azure for Linux workloads</span></span>
[!INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

<span data-ttu-id="3f76a-104">Ez a cikk beállítások toouse HPC Pack toorun Linux munkaterhelések összpontosít.</span><span class="sxs-lookup"><span data-stu-id="3f76a-104">This article focuses on options toouse HPC Pack toorun Linux workloads.</span></span> <span data-ttu-id="3f76a-105">Módon is futtatásához [Windows HPC-munkaterhelések HPC Pack](../windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3f76a-105">There are also options for running [Windows HPC workloads with HPC Pack](../windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a><span data-ttu-id="3f76a-106">Az HPC Pack-fürt futtatására az Azure virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="3f76a-106">Run an HPC Pack cluster in Azure VMs</span></span>
### <a name="azure-templates"></a><span data-ttu-id="3f76a-107">Azure-sablonok</span><span class="sxs-lookup"><span data-stu-id="3f76a-107">Azure templates</span></span>
* <span data-ttu-id="3f76a-108">(GitHub) [HPC Pack 2016 fürt sablonok](https://github.com/MsHpcPack/HPCPack2016)</span><span class="sxs-lookup"><span data-stu-id="3f76a-108">(GitHub) [HPC Pack 2016 cluster templates](https://github.com/MsHpcPack/HPCPack2016)</span></span>
* <span data-ttu-id="3f76a-109">(Marketplace) [HPC Pack fürt Linux munkaterhelések](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/)</span><span class="sxs-lookup"><span data-stu-id="3f76a-109">(Marketplace) [HPC Pack cluster for Linux workloads](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/)</span></span>
* <span data-ttu-id="3f76a-110">(Gyors üzembe helyezés) [Linux számítási csomópontok HPC-fürt létrehozása](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-linux-cn)</span><span class="sxs-lookup"><span data-stu-id="3f76a-110">(Quickstart) [Create an HPC cluster with Linux compute nodes](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-linux-cn)</span></span>

### <a name="powershell-deployment-script"></a><span data-ttu-id="3f76a-111">PowerShell telepítési parancsfájlt</span><span class="sxs-lookup"><span data-stu-id="3f76a-111">PowerShell deployment script</span></span>
* [<span data-ttu-id="3f76a-112">Hozzon létre egy Linux HPC-fürtöt hello HPC Pack IaaS telepítési parancsfájl</span><span class="sxs-lookup"><span data-stu-id="3f76a-112">Create a Linux HPC cluster with hello HPC Pack IaaS deployment script</span></span>](../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="tutorials"></a><span data-ttu-id="3f76a-113">oktatóanyagokat</span><span class="sxs-lookup"><span data-stu-id="3f76a-113">Tutorials</span></span>
* [<span data-ttu-id="3f76a-114">Oktatóanyag: Megismerkedés a Linux számítási csomópontok HPC Pack fürtben az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="3f76a-114">Tutorial: Get started with Linux compute nodes in an HPC Pack cluster in Azure</span></span>](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [<span data-ttu-id="3f76a-115">Oktatóanyag: Futtassa a Microsoft HPC Pack NAMD Linux számítási csomópontok az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="3f76a-115">Tutorial: Run NAMD with Microsoft HPC Pack on Linux compute nodes in Azure</span></span>](classic/hpcpack-cluster-namd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [<span data-ttu-id="3f76a-116">Oktatóanyag: Futtatás OpenFOAM Microsoft HPC Pack Azure Linux RDMA fürtön</span><span class="sxs-lookup"><span data-stu-id="3f76a-116">Tutorial: Run OpenFOAM with Microsoft HPC Pack on a Linux RDMA cluster in Azure</span></span>](classic/hpcpack-cluster-openfoam.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [<span data-ttu-id="3f76a-117">Oktatóanyag: Futtatás csillag-CCM + Microsoft HPC Pack egy Linux RDMA a fürt az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="3f76a-117">Tutorial: Run STAR-CCM+ with Microsoft HPC Pack on a Linux RDMA cluster in Azure</span></span>](classic/hpcpack-cluster-starccm.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="cluster-management"></a><span data-ttu-id="3f76a-118">Fürtkezelés</span><span class="sxs-lookup"><span data-stu-id="3f76a-118">Cluster management</span></span>
* [<span data-ttu-id="3f76a-119">Küldje el a feladatok tooan HPC Pack fürt az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="3f76a-119">Submit jobs tooan HPC Pack cluster in Azure</span></span>](../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="3f76a-120">HPC Pack feladatok kezelése</span><span class="sxs-lookup"><span data-stu-id="3f76a-120">Job management in HPC Pack</span></span>](https://technet.microsoft.com/library/jj899585.aspx)

## <a name="create-rdma-clusters-for-mpi-workloads"></a><span data-ttu-id="3f76a-121">MPI munkaterhelések RDMA-fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="3f76a-121">Create RDMA clusters for MPI workloads</span></span>
* [<span data-ttu-id="3f76a-122">Oktatóanyag: Futtatás OpenFOAM Microsoft HPC Pack Azure Linux RDMA fürtön</span><span class="sxs-lookup"><span data-stu-id="3f76a-122">Tutorial: Run OpenFOAM with Microsoft HPC Pack on a Linux RDMA cluster in Azure</span></span>](classic/hpcpack-cluster-openfoam.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [<span data-ttu-id="3f76a-123">A Linux RDMA fürt toorun MPI alkalmazások beállítása</span><span class="sxs-lookup"><span data-stu-id="3f76a-123">Set up a Linux RDMA cluster toorun MPI applications</span></span>](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

