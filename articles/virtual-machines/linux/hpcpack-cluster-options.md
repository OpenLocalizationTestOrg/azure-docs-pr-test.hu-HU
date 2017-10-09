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
# <a name="options-with-hpc-pack-toocreate-and-manage-an-hpc-cluster-in-azure-for-linux-workloads"></a>A HPC Pack toocreate beállítások és a Linux-munkaterhelések az Azure-ban HPC-fürt kezelése
[!INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

Ez a cikk beállítások toouse HPC Pack toorun Linux munkaterhelések összpontosít. Módon is futtatásához [Windows HPC-munkaterhelések HPC Pack](../windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a>Az HPC Pack-fürt futtatására az Azure virtuális gépeken
### <a name="azure-templates"></a>Azure-sablonok
* (GitHub) [HPC Pack 2016 fürt sablonok](https://github.com/MsHpcPack/HPCPack2016)
* (Marketplace) [HPC Pack fürt Linux munkaterhelések](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/)
* (Gyors üzembe helyezés) [Linux számítási csomópontok HPC-fürt létrehozása](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-linux-cn)

### <a name="powershell-deployment-script"></a>PowerShell telepítési parancsfájlt
* [Hozzon létre egy Linux HPC-fürtöt hello HPC Pack IaaS telepítési parancsfájl](../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="tutorials"></a>oktatóanyagokat
* [Oktatóanyag: Megismerkedés a Linux számítási csomópontok HPC Pack fürtben az Azure-ban](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Oktatóanyag: Futtassa a Microsoft HPC Pack NAMD Linux számítási csomópontok az Azure-ban](classic/hpcpack-cluster-namd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Oktatóanyag: Futtatás OpenFOAM Microsoft HPC Pack Azure Linux RDMA fürtön](classic/hpcpack-cluster-openfoam.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Oktatóanyag: Futtatás csillag-CCM + Microsoft HPC Pack egy Linux RDMA a fürt az Azure-ban](classic/hpcpack-cluster-starccm.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="cluster-management"></a>Fürtkezelés
* [Küldje el a feladatok tooan HPC Pack fürt az Azure-ban](../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [HPC Pack feladatok kezelése](https://technet.microsoft.com/library/jj899585.aspx)

## <a name="create-rdma-clusters-for-mpi-workloads"></a>MPI munkaterhelések RDMA-fürtök létrehozása
* [Oktatóanyag: Futtatás OpenFOAM Microsoft HPC Pack Azure Linux RDMA fürtön](classic/hpcpack-cluster-openfoam.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [A Linux RDMA fürt toorun MPI alkalmazások beállítása](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

