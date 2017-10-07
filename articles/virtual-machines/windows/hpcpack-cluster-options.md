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
# <a name="options-with-hpc-pack-toocreate-and-manage-a-windows-hpc-cluster-in-azure"></a>A HPC Pack toocreate lehetőségek és az Azure-ban a Windows HPC-fürt kezeléséhez
[!INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

Ez a cikk foglalkozik a beállítások toocreate HPC Pack fürtök toorun Windows munkaterhelések. Módon is a létrehozásakor HPC Pack fürtök toorun [Linux HPC munkaterhelések](../linux/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a>Az HPC Pack-fürt futtatására az Azure virtuális gépeken
### <a name="azure-templates"></a>Azure-sablonok
* (GitHub) [HPC Pack 2016 fürt sablonok](https://github.com/MsHpcPack/HPCPack2016)
* (Marketplace) [Munkaterhelések Windows HPC Pack fürt](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/)
* (Marketplace) [HPC Pack fürt Excel munkaterhelések](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterexcelcn/)
* (Gyors üzembe helyezés) [HPC-fürt létrehozása](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster)
* (Gyors üzembe helyezés) [Egyéni számítási csomópont lemezképpel HPC-fürt létrehozása](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-custom-image)

### <a name="azure-vm-images"></a>Az Azure Virtuálisgép-rendszerképek
* [Windows Server 2016 HPC Pack 2016 átjárócsomópontjához](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.HPCPack2016HeadNodeonWindowsServer2016?tab=Overview)
* [A Windows Server 2016 HPC Pack 2016 számítási csomópont](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.HPCPack2016ComputeNodeonWindowsServer2016?tab=Overview)
* [HPC Pack 2016 átjárócsomópont Windows Server 2012 R2 rendszeren](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.HPCPack2016HeadNodeonWindowsServer2012R2?tab=Overview)
* [HPC Pack 2016 számítási csomóponton a Windows Server 2012 R2 rendszeren](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.HPCPack2016ComputeNodeonWindowsServer2012R2?tab=Overview)
* [HPC Pack 2012 R2 átjárócsomópont Windows Server 2012 R2 rendszeren](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/)
* [HPC Pack 2012 R2 számítási csomóponton a Windows Server 2012 R2 rendszeren](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodeonwindowsserver2012r2/)
* [HPC Pack számítási csomópont az Excel alkalmazással Windows Server 2012 R2 rendszeren](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodewithexcelonwindowsserver2012r2/)

### <a name="powershell-deployment-script"></a>PowerShell telepítési parancsfájlt
* [HPC Pack IaaS telepítési parancsfájl hello HPC-fürt létrehozása](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

### <a name="tutorials"></a>oktatóanyagokat
* [Oktatóanyag: HPC Pack 2016-fürt üzembe helyezése az Azure-ban](hpcpack-2016-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Oktatóanyag: Ismerkedés az Azure toorun Excel HPC Pack fürtöt és SOA munkaterhelések](excel-cluster-hpcpack.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

### <a name="manual-deployment-with-hello-azure-portal"></a>Az Azure-portálon hello manuális telepítése
* [Átjárócsomópont hello egy Azure virtuális gép HPC Pack fürt beállítása](hpcpack-cluster-headnode.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

### <a name="cluster-management"></a>Fürtkezelés
* [Az Azure-ban HPC Pack-fürtben lévő számítási csomópontok kezelése](classic/hpcpack-cluster-node-manage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Növelhető vagy csökkenthető a HPC Pack-fürtben lévő Azure számítási erőforrásokat](classic/hpcpack-cluster-node-autogrowshrink.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Küldje el a feladatok tooan HPC Pack fürt az Azure-ban](hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [HPC Pack feladatok kezelése](https://technet.microsoft.com/library/jj899585.aspx)
* [Az Azure szolgáltatásban az Azure Active Directory HPC Pack fürtöt kezelése](hpcpack-cluster-active-directory.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="add-worker-role-nodes-tooan-hpc-pack-cluster"></a>Adja hozzá a feldolgozói szerepkör csomópontok tooan HPC Pack fürt
* [Kapacitásnövelés a HPC Pack tooAzure worker-példányok](https://technet.microsoft.com/library/gg481749.aspx)
* [Oktatóanyag: Az Azure-ban HPC Pack hibrid fürt beállítása](../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)
* [Az Azure "kapacitásnövelés" csomópontok tooan HPC Pack központi csomópont hozzáadása az Azure-ban](classic/hpcpack-cluster-node-burst.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="integrate-with-azure-batch"></a>Az Azure Batch integrálása
* [Kapacitásnövelés a HPC Pack kötegelt tooAzure](https://technet.microsoft.com/library/mt612877.aspx)

## <a name="create-rdma-clusters-for-mpi-workloads"></a>MPI munkaterhelések RDMA-fürtök létrehozása
* [HPC Pack toorun MPI alkalmazásokkal Windows RDMA fürt beállítása](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

