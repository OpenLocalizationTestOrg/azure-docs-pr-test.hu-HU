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
# <a name="options-with-hpc-pack-to-create-and-manage-an-hpc-cluster-in-azure-for-linux-workloads"></a>HPC Pack létrehozása és kezelése a Linux-munkaterhelések az Azure-ban HPC-fürt beállítások
[!INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

Ez a cikk foglalkozik Linux alkalmazásokat és szolgáltatásokat futtathatnak HPC Pack használatával a beállítások. Módon is futtatásához [Windows HPC-munkaterhelések HPC Pack](../windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a>Az HPC Pack-fürt futtatására az Azure virtuális gépeken
### <a name="azure-templates"></a>Azure-sablonok
* (GitHub) [HPC Pack 2016 fürt sablonok](https://github.com/MsHpcPack/HPCPack2016)
* (Marketplace) [HPC Pack fürt Linux munkaterhelések](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/)
* (Gyors üzembe helyezés) [Linux számítási csomópontok HPC-fürt létrehozása](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-linux-cn)

### <a name="powershell-deployment-script"></a>PowerShell telepítési parancsfájlt
* [Hozzon létre egy Linux HPC-fürtöt a HPC Pack IaaS telepítési parancsfájl](../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="tutorials"></a>oktatóanyagokat
* [Oktatóanyag: Megismerkedés a Linux számítási csomópontok HPC Pack fürtben az Azure-ban](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Oktatóanyag: Futtassa a Microsoft HPC Pack NAMD Linux számítási csomópontok az Azure-ban](classic/hpcpack-cluster-namd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Oktatóanyag: Futtatás OpenFOAM Microsoft HPC Pack Azure Linux RDMA fürtön](classic/hpcpack-cluster-openfoam.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Oktatóanyag: Futtatás csillag-CCM + Microsoft HPC Pack egy Linux RDMA a fürt az Azure-ban](classic/hpcpack-cluster-starccm.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="cluster-management"></a>Fürtkezelés
* [Az Azure-ban HPC Pack fürthöz feladatok elküldéséhez](../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [HPC Pack feladatok kezelése](https://technet.microsoft.com/library/jj899585.aspx)

## <a name="create-rdma-clusters-for-mpi-workloads"></a>MPI munkaterhelések RDMA-fürtök létrehozása
* [Oktatóanyag: Futtatás OpenFOAM Microsoft HPC Pack Azure Linux RDMA fürtön](classic/hpcpack-cluster-openfoam.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [MPI-alkalmazások futtatására Linux RDMA fürt beállítása](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

