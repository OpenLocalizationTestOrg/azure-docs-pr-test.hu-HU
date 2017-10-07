---
title: "aaaPowerShell parancsfájl toodeploy Linux HPC-fürt |} Microsoft Docs"
description: "Az Azure virtuális gépeken Linux HPC Pack 2012 R2-fürt futtatni egy PowerShell-parancsfájl toodeploy"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,hpc-pack
ms.assetid: 73041960-58d3-4ecf-9540-d7e1a612c467
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 12/29/2016
ms.author: danlep
ms.openlocfilehash: 885b03fa2fd604827dc388803fc21debab730979
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-linux-high-performance-computing-hpc-cluster-with-hello-hpc-pack-iaas-deployment-script"></a>Hozzon létre egy Linux nagy teljesítményű számítástechnikai rendszerek (HPC) fürtöt hello HPC Pack IaaS telepítési parancsfájl
Egy teljes HPC Pack 2012 R2-fürt Linux munkaterhelések az hello HPC Pack IaaS telepítési PowerShell parancsfájl toodeploy futtassa az Azure virtuális gépeken. hello fürt egy Active Directory-tartományhoz átjárócsomópont Windows Server és a Microsoft HPC Pack fut, és a számítási csomópontok valamelyikét futtató hello HPC Pack által támogatott Linux terjesztésekről áll. Ha azt szeretné, hogy az a Windows Azure munkaterhelések HPC Pack fürtöt toodeploy, [hozzon létre egy Windows HPC-fürtöt hello HPC Pack IaaS telepítési parancsfájl](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Az Azure Resource Manager sablon toodeploy egy HPC Pack fürthöz is használható. Egy vonatkozó példáért lásd: [HPC-fürt létrehozása Linux számítási csomópontok](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/).

> [!IMPORTANT] 
> hello ebben a cikkben ismertetett PowerShell-parancsfájl a klasszikus üzembe helyezési modellel hello Azure-ban létrehoz egy Microsoft HPC Pack 2012 R2 fürtöt. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.
> Ebben a cikkben ismertetett hello parancsfájl emellett nem támogatja a HPC Pack 2016.

[!INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-file"></a>Példa konfigurációs fájlt
hello következő konfigurációs fájl egy tartományvezérlő és a tartomány erdő hoz létre és telepít egy HPC Pack fürt, amelynek helyi adatbázisok egy átjárócsomópont és 10 Linux számítási csomópontok. Összes hello felhőszolgáltatás közvetlenül hello Kelet-Ázsia hely jönnek létre. hello Linux számítási csomópontok jönnek létre két felhőalapú szolgáltatások és a két storage-fiókok (Ez azt jelenti, hogy *MyLnxCN-0001* való *MyLnxCN-0005* a *MyLnxCNService01* és *mylnxstorage01*, és *MyLnxCN-0006* való *MyLnxCN-0010* a *MyLnxCNService02* és *mylnxstorage02* ). OpenLogic CentOS 7.0 verzió Linux lemezképből hello számítási csomópontok jönnek létre. 

Helyettesítse a saját értékeit az előfizetés nevét és az hello fiók és a szolgáltatás nevét.

```Xml
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>MyDCServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>Large</VMSize>
    </DomainController>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>MyLnxCN-%0001%</VMNamePattern>
    <ServiceNamePattern>MyLnxCNService%01%</ServiceNamePattern>
    <MaxNodeCountPerService>5</MaxNodeCountPerService>
    <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>10</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20150325 </ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```
## <a name="troubleshooting"></a>Hibaelhárítás
* **"A virtuális hálózat nem létezik" hiba**. Ha hello HPC Pack IaaS telepítési parancsfájl toodeploy több fürt az Azure-ban egyszerre egy előfizetéshez tartozó, egy vagy több üzemelő példány hello hiba miatt sikertelen lehet "VNet *VNet\_neve* nem létezik".
  Ha ez a hiba akkor fordul elő, futtassa újra a hello parancsfájl nem tudta hello központi telepítéshez.
* **Az Azure-beli virtuális hálózat hello Internet probléma elérése hello**. HPC Pack-fürtöt hoz létre az új tartományvezérlő hello telepítési parancsfájl használatával, vagy manuálisan főkiszolgálóvá előléptetni egy átjárócsomóponttal VM toodomain vezérlő, ha problémába ütközik a virtuális gépek hello hello Azure-beli virtuális hálózat toohello Internet tapasztalhatja. Ez akkor fordulhat elő, ha egy továbbító DNS-kiszolgáló automatikusan hello tartományvezérlőn van konfigurálva, és a továbbító DNS-kiszolgáló nem oldja meg megfelelően.
  
    a probléma megoldásához toowork jelentkezzen be a toohello tartományvezérlő és vagy eltávolítása hello továbbító konfigurációs beállítás, vagy egy érvényes továbbító DNS-kiszolgáló konfigurálása. Kattintson a Kiszolgálókezelő toodo **eszközök** > **DNS** tooopen DNS-kezelőben, majd kattintson duplán **továbbítók**.

## <a name="next-steps"></a>Következő lépések
* Lásd: [Ismerkedés az Azure-ban HPC Pack-fürtben lévő Linux számítási csomópontok](hpcpack-cluster.md) támogatott Linux disztribúciókról kapcsolatos információkért megköveteli az adatok, és a feladatok tooan HPC Pack fürt Linux elküldése számítási csomópontjain.
* Oktatóprogramot kínál, amelyek hello parancsfájl toocreate egy fürt használja, és futtassa a Linux HPC munkaterhelés lásd:
  * [A Microsoft HPC Pack NAMD futó Linux számítási csomópontok az Azure-ban](hpcpack-cluster-namd.md)
  * [A Microsoft HPC Pack OpenFOAM futó Linux számítási csomópontok az Azure-ban](hpcpack-cluster-openfoam.md)
  * [Futtassa a csillag-CCM + Microsoft HPC Pack Linux számítási csomópontok az Azure-ban](hpcpack-cluster-starccm.md)

