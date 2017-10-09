---
title: "aaaPowerShell parancsfájl toodeploy Windows HPC-fürt |} Microsoft Docs"
description: "Az Azure virtuális gépeken a Windows HPC Pack 2012 R2-fürt futtatására a egy PowerShell-parancsfájl toodeploy"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,hpc-pack
ms.assetid: 286b2be8-2533-40df-b02a-26156b1f1133
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 12/29/2016
ms.author: danlep
ms.openlocfilehash: 10ce1e9bc4e98954b955549bd72aaaf6106c69fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-high-performance-computing-hpc-cluster-with-hello-hpc-pack-iaas-deployment-script"></a>Hozzon létre egy Windows nagy teljesítményű számítástechnikai rendszerek (HPC) fürtöt hello HPC Pack IaaS telepítési parancsfájl
Egy teljes HPC Pack 2012 R2-fürt Windows munkaterhelések az hello HPC Pack IaaS telepítési PowerShell parancsfájl toodeploy futtatása az Azure virtuális gépeken. hello fürt tartalmaz egy Active Directory-tartományhoz átjárócsomópont Windows Server és a Microsoft HPC Pack fut, és további Windows számítási erőforrásokat, akkor adja meg. Ha az Azure HPC Pack fürtöt toodeploy Linux munkaterhelésekhez, lásd: [hozzon létre egy Linux HPC-fürtöt hello HPC Pack IaaS telepítési parancsfájl](../../linux/classic/hpcpack-cluster-powershell-script.md). Az Azure Resource Manager sablon toodeploy egy HPC Pack fürthöz is használható. Tekintse meg a [HPC-fürt létrehozása](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/) és [HPC-fürt létrehozása egyéni számítási csomópont képének](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-custom-image/).

> [!IMPORTANT] 
> hello ebben a cikkben ismertetett PowerShell-parancsfájl a klasszikus üzembe helyezési modellel hello Azure-ban létrehoz egy Microsoft HPC Pack 2012 R2 fürtöt. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.
> Ebben a cikkben ismertetett hello parancsfájl emellett nem támogatja a HPC Pack 2016.

[!INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-files"></a>Példa konfigurációs fájlok
Hello az alábbi példák a saját értékeit az előfizetés-azonosítóval vagy és hello fiók és a szolgáltatás nevének helyettesítse be.

### <a name="example-1"></a>1. példa
hello következő konfigurációs fájl telepíti egy HPC Pack fürt, amelyen egy helyi adatbázisok átjárócsomópont és öt számítási csomópontok hello Windows Server 2012 R2 operációs rendszert futtat. Összes hello felhőszolgáltatás közvetlenül hello USA nyugati régiója hely jönnek létre. hello átjárócsomópont hello tartomány erdő tartományvezérlőjévé funkcionál.

```Xml
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionId>08701940-C02E-452F-B0B1-39D50119F267</SubscriptionId>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%1000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>5</NodeCount>
    <OSVersion>WindowsServer2012R2</OSVersion>
  </ComputeNodes>
</IaaSClusterConfig>
```

### <a name="example-2"></a>2. példa
a következő konfigurációs fájl hello telepíti egy meglévő tartomány erdőben HPC Pack fürt. hello fürt 1 átjárócsomópont helyi adatbázisokkal rendelkezik, és 12 számítási csomópontokat a hello alkalmazott BGInfo Virtuálisgép-bővítmény.
Windows-frissítések automatikus telepítése a virtuális gépek hello tartomány erdő összes hello le van tiltva. Összes hello felhőszolgáltatás közvetlenül a Kelet-Ázsia hely jönnek létre. hello számítási csomópontok létrehozott három felhőszolgáltatások és három storage-fiókok: *MyHPCCN-0001* túl*MyHPCCN-0005* a *MyHPCCNService01* és  *mycnstorage01*; *MyHPCCN-0006* túl*MyHPCCN0010* a *MyHPCCNService02* és *mycnstorage02*; és  *MyHPCCN-0011* túl*MyHPCCN-0012* a *MyHPCCNService03* és *mycnstorage03*). hello számítási csomópontokat, a számítási csomópont rögzítése meglévő titkos lemezképpel jönnek létre. hello automatikus növelhető vagy csökkenthető az engedélyezve van az alapértelmezett növelhető vagy csökkenthető a időközönként.

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
     <NoWindowsAutoUpdate />
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
  </Certificates>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0001%</VMNamePattern>
<ServiceNamePattern>MyHPCCNService%01%</ServiceNamePattern>
<MaxNodeCountPerService>5</MaxNodeCountPerService>
<StorageAccountNamePattern>mycnstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>12</NodeCount>
    <ImageName HPCPackInstalled=”true”>MyHPCComputeNodeImage</ImageName>
    <VMExtensions>
       <VMExtension>
          <ExtensionName>BGInfo</ExtensionName>
          <Publisher>Microsoft.Compute</Publisher>
          <Version>1.*</Version>
       </VMExtension>
    </VMExtensions>
  </ComputeNodes>
  <AutoGrowShrink>
    <CertificateId>1</CertificateId>
  </AutoGrowShrink>
</IaaSClusterConfig>

```

### <a name="example-3"></a>3. példa
a következő konfigurációs fájl hello telepíti egy meglévő tartomány erdőben HPC Pack fürt. hello fürt tartalmaz egy átjárócsomópont, egy adatbázis-kiszolgálón, 500 GB adatlemezt, hello Windows Server 2012 R2 operációs rendszer, és öt számítási csomópontok hello Windows Server 2012 R2 operációs rendszert futtató két csomópontok. hello MyHPCCNService hello affinitáscsoportban jön létre a felhőalapú szolgáltatás *MyIBAffinityGroup*, és hello felhőszolgáltatásokra jönnek létre hello affinitáscsoportban *MyAffinityGroup*. hello HPC feladat ütemezési REST API-t és a HPC-webportál hello átjárócsomópont engedélyezve.

```Xml
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>    
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>NewRemoteDB</DBOption>
    <DBVersion>SQLServer2014_Enterprise</DBVersion>
    <DBServer>
      <VMName>MyDBServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>ExtraLarge</VMSize>
      <DataDiskSizeInGB>500</DataDiskSizeInGB>
    </DBServer>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>A8</VMSize>
<NodeCount>5</NodeCount>
<AffinityGroup>MyIBAffinityGroup</AffinityGroup>
  </ComputeNodes>
  <BrokerNodes>
    <VMNamePattern>MyHPCBN-%0000%</VMNamePattern>
    <ServiceName>MyHPCBNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>2</NodeCount>
  </BrokerNodes>
</IaaSClusterConfig>
```



### <a name="example-4"></a>4. példa
a következő konfigurációs fájl hello telepíti egy meglévő tartomány erdőben HPC Pack fürt. helyi adatbázisok két központi csomóponttal rendelkező hello fürt, két Azure csomópont sablonok létrehozása és három mérete közepes Azure csomópontot hoz létre Azure csomópontsablonhoz *AzureTemplate1*. A parancsfájl futó hello átjárócsomópont hello átjárócsomópont konfigurálása után.

```Xml
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
<VMSize>ExtraLarge</VMSize>
    <PostConfigScript>c:\MyHNPostActions.ps1</PostConfigScript>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
    <Certificate>
      <Id>2</Id>
      <PfxFile>d:\mytestcert2.pfx</PfxFile>
    </Certificate>    
  </Certificates>
  <AzureBurst>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate1</TemplateName>
      <SubscriptionId>bb9252ba-831f-4c9d-ae14-9a38e6da8ee4</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc1</ServiceName>
      <StorageAccount>myteststorage1</StorageAccount>
      <NodeCount>3</NodeCount>
      <RoleSize>Medium</RoleSize>
    </AzureNodeTemplate>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate2</TemplateName>
      <SubscriptionId>ad4b9f9f-05f2-4c74-a83f-f2eb73000e0b</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc2</ServiceName>
      <StorageAccount>myteststorage2</StorageAccount>
      <Proxy>
        <UsesStaticProxyCount>false</UsesStaticProxyCount>     
        <ProxyRatio>100</ProxyRatio>
        <ProxyRatioBase>400</ProxyRatioBase>
      </Proxy>
      <OSVersion>WindowsServer2012</OSVersion>
    </AzureNodeTemplate>
  </AzureBurst>
</IaaSClusterConfig>
```

## <a name="troubleshooting"></a>Hibaelhárítás
* **"A virtuális hálózat nem létezik" hiba** -futtatásakor hello parancsfájl toodeploy több fürt az Azure-ban egyszerre egy előfizetéshez tartozó, egy vagy több üzemelő példány hello hiba miatt sikertelen lehet "VNet *VNet\_neve* nem a létező".
  Ez a hiba akkor fordul elő, ha parancsprogrammal hello újra a sikertelen hello telepítéshez.
* **Az Azure-beli virtuális hálózat hello Internet probléma elérése hello** – Ha hoz létre egy fürtöt az új tartományvezérlő hello telepítési parancsfájlt, vagy manuálisan főkiszolgálóvá előléptetni egy átjárócsomóponttal VM toodomain vezérlő, problémák hello virtuális gépek toohello internethez kapcsolódik. Ez a probléma akkor fordulhat elő, ha továbbító DNS-kiszolgáló automatikusan hello tartományvezérlőn van konfigurálva, és a továbbító DNS-kiszolgáló nem oldja meg megfelelően.
  
    a probléma megoldásához toowork jelentkezzen be a toohello tartományvezérlő és vagy eltávolítása hello továbbító konfigurációs beállítás, vagy egy érvényes továbbító DNS-kiszolgáló konfigurálása. Ezt a beállítást, a Kiszolgálókezelőben kattintson tooconfigure **eszközök** >
    **DNS** tooopen DNS-kezelőben, majd kattintson duplán **továbbítók**.
* **RDMA hálózati elérésekor számítási igényű virtuális gépek** –, ha ad hozzá a Windows Server számítási csomópont használja az RDMA-kompatibilisek-e virtuális gépek méretezés például A8 és A9, vagy e virtuális gépek toohello RDMA alkalmazás hálózati csatlakozás problémák. Ez a probléma akkor fordul elő, akkor ha hello HpcVmDrivers kiterjesztése nincs megfelelően telepítve hello virtuális gépek hozzáadásakor toohello fürt. Például a bővítmény előfordulhat, hogy ragadt; ebben az állapotban telepítése hello.
  
    a probléma megoldásához, első hello állapotának hello virtuális gépek hello bővítmény toowork. Hello bővítmény nem megfelelően van telepítve, ha próbálja meg eltávolítani a hello csomópontok hello HPC-fürtből, és majd újból vegye fel hello csomópontok. Számítási csomópont virtuális gépek például hello átjárócsomópont hello Add-HpcIaaSNode.ps1 parancsfájl futtatásával is hozzáadhat.

## <a name="next-steps"></a>Következő lépések
* Futtassa a test-munkaterhelések hello fürtön. Egy vonatkozó példáért lásd: hello HPC Pack [– első lépések útmutató](https://technet.microsoft.com/library/jj884144).
* Egy oktatóanyag tooscript hello fürttelepítést, és futtassa a HPC-munkaterhelés, a következő témakörben: [Ismerkedés az Azure toorun Excel és a SOA munkaterhelések HPC Pack fürtöt a](../../virtual-machines-windows-excel-cluster-hpcpack.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* HPC Pack eszközök toostart próbálja, állítsa le, vegye fel és számítási csomópontok eltávolítása egy fürtről hoz létre. Lásd: [kezelése számítási csomópontok HPC csomagban fürtön, az Azure-ban](hpcpack-cluster-node-manage.md).
* tooget toosubmit feladatok toohello fürt beállításához a helyi számítógépről, tekintse meg [egy helyi számítógép tooan HPC Pack-feladatok HPC küldje el a fürt az Azure-ban](../../virtual-machines-windows-hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

