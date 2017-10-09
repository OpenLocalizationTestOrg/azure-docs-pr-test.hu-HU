---
title: "egy SAP multi-SID-konfiguráció Azure aaaCreate |} Microsoft Docs"
description: "Az útmutató toohigh rendelkezésre állási SAP NetWeaver multi-SID konfigurációban Windows virtuális gépeken"
services: virtual-machines-windows, virtual-network, storage
documentationcenter: saponazure
author: goraco
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 0b89b4f8-6d6c-45d7-8d20-fe93430217ca
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e2a4b12928231743c59af86dab72595ad38d364b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-sap-netweaver-multi-sid-configuration"></a>Egy SAP NetWeaver multi-SID-konfiguráció létrehozása

[load-balancer-multivip-overview]:../../../load-balancer/load-balancer-multivip-overview.md
[sap-ha-guide]:sap-high-availability-guide.md 
[sap-ha-guide-figure-6001]:./media/virtual-machines-shared-sap-high-availability-guide/6001-sap-multi-sid-ascs-scs-sid1.png
[sap-ha-guide-figure-6002]:./media/virtual-machines-shared-sap-high-availability-guide/6002-sap-multi-sid-ascs-scs.png
[sap-ha-guide-figure-6003]:./media/virtual-machines-shared-sap-high-availability-guide/6003-sap-multi-sid-full-landscape.png
[sap-ha-guide-figure-6004]:./media/virtual-machines-shared-sap-high-availability-guide/6004-sap-multi-sid-dns.png
[sap-ha-guide-figure-6005]:./media/virtual-machines-shared-sap-high-availability-guide/6005-sap-multi-sid-azure-portal.png
[sap-ha-guide-figure-6006]:./media/virtual-machines-shared-sap-high-availability-guide/6006-sap-multi-sid-sios-replication.png
[networking-limits-azure-resource-manager]:../../../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits
[sap-ha-guide-9.1.1]:sap-high-availability-guide.md#a97ad604-9094-44fe-a364-f89cb39bf097 
[sap-ha-guide-8.8]:sap-high-availability-guide.md#f19bd997-154d-4583-a46e-7f5a69d0153c
[sap-ha-guide-8.12.3.3]:sap-high-availability-guide.md#d9c1fc8e-8710-4dff-bec2-1f535db7b006 
[sap-ha-guide-9]:sap-high-availability-guide.md#a06f0b49-8a7a-42bf-8b0d-c12026c5746b
[sap-ha-guide-9.1]:sap-high-availability-guide.md#31c6bd4f-51df-4057-9fdf-3fcbc619c170 
[sap-ha-guide-9.1.2]:sap-high-availability-guide.md#eb5af918-b42f-4803-bb50-eff41f84b0b0 
[sap-ha-guide-9.1.3]:sap-high-availability-guide.md#e4caaab2-e90f-4f2c-bc84-2cd2e12a9556 
[sap-ha-guide-9.1.4]:sap-high-availability-guide.md#10822f4f-32e7-4871-b63a-9b86c76ce761 
[sap-ha-guide-9.4]:sap-high-availability-guide.md#094bc895-31d4-4471-91cc-1513b64e406a 
[sap-ha-guide-9.5]:sap-high-availability-guide.md#2477e58f-c5a7-4a5d-9ae3-7b91022cafb5 
[sap-ha-guide-9.6]:sap-high-availability-guide.md#0ba4a6c1-cc37-4bcf-a8dc-025de4263772 
[sap-ha-guide-10]:sap-high-availability-guide.md#18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9

2016 szeptemberétől a Microsoft, amely egy szolgáltatás, ahol több virtuális IP-cím kezelheti az Azure belső terheléselosztó használatával. Ez a funkció már létezik a hello Azure külső terheléselosztó.

Ha egy SAP üzemelő példányt, használhatja egy belső terheléselosztó toocreate Windows fürtkonfiguráció az SAP ASC/SCS, dokumentált módon hello [útmutató a magas rendelkezésre állású SAP NetWeaver Windows virtuális gépeken] [ sap-ha-guide].

Hogyan toomove egyetlen ASC/SCS telepítési tooan SAP multi-SID-konfigurációról további SAP ASC/SCS telepítésével fürtözött be egy meglévő Windows Server feladatátvételi fürtszolgáltatási (WSFC) fürt példányok cikkben összpontosít. Ez a folyamat befejezése után fog konfigurálta az SAP multi-SID-fürtöt.


## <a name="prerequisites"></a>Előfeltételek
Már konfigurálta a WSFC-fürt, amely egy SAP ASC/SCS példány szolgál a hello leírtaknak megfelelően [útmutató a magas rendelkezésre állású SAP NetWeaver Windows virtuális gépeken] [ sap-ha-guide] és ezen a diagramon látható módon.

![Magas rendelkezésre állású SAP ASC/SCS példány][sap-ha-guide-figure-6001]

## <a name="target-architecture"></a>Tároló-architektúra

hello célja tooinstall több SAP ABAP ASC vagy SAP Java SCS fürtözött példánya hello azonos WSFC-fürtre, az alábbiak szerint:

![Több SAP ASC/SCS fürtözött példánya, az Azure-ban][sap-ha-guide-figure-6002]

> [!NOTE]
>Nincs titkos előtér-IP-címek minden Azure belső terheléselosztóhoz számának korlátozása toohello.
>
>egy WSFC-fürtön SAP ASC/SCS példányok maximális száma hello titkos előtér-IP-címek minden Azure belső terheléselosztóhoz maximális száma egyenlő toohello.
>

Terheléselosztó korlátok kapcsolatos további információkért lásd: "magánhálózati front-end IP-cím terheléselosztó" a [hálózatkezelés korlátok: Azure Resource Manager][networking-limits-azure-resource-manager].

hello teljes fekvő két magas rendelkezésre állású SAP rendszerrel néz ki:

![SAP magas rendelkezésre állású multi-SID telepítése két SAP rendszer SID-k][sap-ha-guide-figure-6003]

> [!IMPORTANT]
> hello beállítása hello a következő feltételeknek kell megfelelniük:
> - hello SAP ASC/SCS példányai közös hello azonos WSFC-fürtre.
> - Minden adatbázis-kezelő SID rendelkeznie kell a saját dedikált WSFC-fürtre.
> - SAP alkalmazáskiszolgálók tooone SAP rendszer SID tartozó rendelkeznie kell a saját dedikált virtuális gépek.


## <a name="prepare-hello-infrastructure"></a>Hello infrastruktúra előkészítése
tooprepare az infrastruktúra telepítése egy SAP ASC/SCS másodpéldányt a következő paraméterek hello:

| Paraméter neve | Érték |
| --- | --- |
| SAP ASC/SCS SID |PR1-lb-ASC |
| SAP DBMS belső terheléselosztó | PR5 |
| SAP virtuális állomás neve | pr5-sap-cl |
| SAP ASC/SCS virtuális gazdagép IP-címe (további Azure load balancer IP-címe) | 10.0.0.50 |
| SAP ASC/SCS példányszámának | 50 |
| ILB mintavételi portot további SAP ASC/SCS-példány | 62350 |

> [!NOTE]
> Az SAP ASC/SCS példányokat a minden IP-cím csak egy egyedi mintavételi portot. Például egy IP-cím, egy Azure belső terheléselosztón 62300 mintavételi portot használ, ha nincs más IP-címet a terheléselosztóhoz mintavételi portot 62300 is használhat.
>
>A célokra mintavételi portot 62300 már foglalt, mert használjuk 62350 mintavételi portot.

SAP ASC/SCS további példányokat is telepíthet hello létező WSFC-fürtön két csomóponttal rendelkező:

| Virtuálisgép-szerepkör | Virtuális gép állomásneve | Statikus IP-cím |
| --- | --- | --- |
| 1. fürtcsomópont ASC/SCS-példány |PR1-ASC-0 |10.0.0.10 |
| 2. fürtcsomópont ASC/SCS-példány |PR1-ASC-1 |10.0.0.9 |

### <a name="create-a-virtual-host-name-for-hello-clustered-sap-ascsscs-instance-on-hello-dns-server"></a>Hozzon létre egy virtuális nevet fürtözött hello SAP ASC/SCS példány hello DNS-kiszolgálón

Egy DNS-bejegyzést hello ASC/SCS példány hello virtuális állomás nevét a következő paraméterek hello segítségével hozhat létre:

| Új SAP ASC/SCS virtuális állomás neve | Társított IP-cím |
| --- | --- | --- |
|pr5-sap-cl |10.0.0.50 |

hello új állomás nevének és IP-cím hello DNS-kezelőben, ahogy az alábbi képernyőfelvétel a hello láthatók:

![DNS-kezelő lista hello kiemelés definiált hello új SAP ASC/SCS fürt virtuális nevét és a TCP/IP-cím DNS-bejegyzés][sap-ha-guide-figure-6004]

hello való létrehozásakor a DNS-bejegyzés is eljárása fő hello részletes [útmutató a magas rendelkezésre állású SAP NetWeaver Windows virtuális gépeken][sap-ha-guide-9.1.1].

> [!NOTE]
> hello új IP-cím hozzárendelése hello további ASC/SCS példány toohello virtuális állomás nevét kell lennie hello ugyanaz, mint a hello toohello SAP Azure terheléselosztóhoz rendelt új IP-címet.
>
>A mi esetünkben hello IP-cím 10.0.0.50.

### <a name="add-an-ip-address-tooan-existing-azure-internal-load-balancer-by-using-powershell"></a>IP cím tooan meglévő Azure belső terheléselosztó hozzáadása a PowerShell használatával

egy SAP ASC/SCS példánya a nagyobb toocreate hello azonos WSFC fürt esetén használjon PowerShell tooadd egy meglévő Azure belső terheléselosztó IP-cím tooan. Minden IP-címet igényel a saját terheléselosztási szabályok, a mintavételi portot, az előtér-IP-készlet és a háttér-készlet.

hello következő parancsfájl hozzáad egy új IP cím tooan meglévő terheléselosztó. Hello PowerShell változók a környezet frissítése. hello parancsfájlt hoz létre minden SAP ASC/SCS porthoz valamennyi szükséges terheléselosztási szabályok.

```powershell

# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>
Clear-Host
$ResourceGroupName = "SAP-MULTI-SID-Landscape"      # Existing resource group name
$VNetName = "pr2-vnet"                        # Existing virtual network name
$SubnetName = "Subnet"                        # Existing subnet name
$ILBName = "pr2-lb-ascs"                      # Existing ILB name                      
$ILBIP = "10.0.0.50"                          # New IP address
$VMNames = "pr2-ascs-0","pr2-ascs-1"          # Existing cluster virtual machine names
$SAPInstanceNumber = 50                       # SAP ASCS/SCS instance number: must be a unique value for each cluster
[int]$ProbePort = "623$SAPInstanceNumber"     # Probe port: must be a unique value for each IP and load balancer

$ILB = Get-AzureRmLoadBalancer -Name $ILBName -ResourceGroupName $ResourceGroupName

$count = $ILB.FrontendIpConfigurations.Count + 1
$FrontEndConfigurationName ="lbFrontendASCS$count"
$LBProbeName = "lbProbeASCS$count"

# Get hello Azure VNet and subnet
$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName
$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName

# Add second front-end and probe configuration
Write-Host "Adding new front end IP Pool '$FrontEndConfigurationName' ..." -ForegroundColor Green
$ILB | Add-AzureRmLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.Id
$ILB | Add-AzureRmLoadBalancerProbeConfig -Name $LBProbeName  -Protocol Tcp -Port $Probeport -ProbeCount 2 -IntervalInSeconds 10  | Set-AzureRmLoadBalancer

# Get new updated configuration
$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName
# Get new updated LP FrontendIP COnfig
$FEConfig = Get-AzureRmLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB
$HealthProbe  = Get-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

# Add new back-end configuration into existing ILB
$BackEndConfigurationName  = "backendPoolASCS$count"
Write-Host "Adding new backend Pool '$BackEndConfigurationName' ..." -ForegroundColor Green
$BEConfig = Add-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB | Set-AzureRmLoadBalancer

# Get new updated config
$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName

# Assign VM NICs toobackend pool
$BEPool = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB
foreach($VMName in $VMNames){
        $VM = Get-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VMName
        $NICName = ($VM.NetworkInterfaceIDs[0].Split('/') | select -last 1)        
        $NIC = Get-AzureRmNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName                
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools += $BEPool
        Write-Host "Assigning network card '$NICName' of hello '$VMName' VM toohello backend pool '$BackEndConfigurationName' ..." -ForegroundColor Green
        Set-AzureRmNetworkInterface -NetworkInterface $NIC
        #start-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VM.Name
}


# Create load-balancing rules
$Ports = "445","32$SAPInstanceNumber","33$SAPInstanceNumber","36$SAPInstanceNumber","39$SAPInstanceNumber","5985","81$SAPInstanceNumber","5$SAPInstanceNumber`13","5$SAPInstanceNumber`14","5$SAPInstanceNumber`16"
$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName
$FEConfig = get-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB
$BEConfig = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB
$HealthProbe  = Get-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

Write-Host "Creating load balancing rules for hello ports: '$Ports' ... " -ForegroundColor Green

foreach ($Port in $Ports) {

        $LBConfigrulename = "lbrule$Port" + "_$count"
        Write-Host "Creating load balancing rule '$LBConfigrulename' for hello port '$Port' ..." -ForegroundColor Green

        $ILB | Add-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $HealthProbe -Protocol tcp -FrontendPort  $Port -BackendPort $Port -IdleTimeoutInMinutes 30 -LoadDistribution Default -EnableFloatingIP   
}

$ILB | Set-AzureRmLoadBalancer

Write-Host "Succesfully added new IP '$ILBIP' toohello internal load balancer '$ILBName'!" -ForegroundColor Green

```
Miután hello parancsfájl lefutott, jelennek meg hello hello Azure-portálon, ahogy az alábbi képernyőfelvétel a hello:

![Hello Azure-portálon található új előtér-IP-készlet][sap-ha-guide-figure-6005]

### <a name="add-disks-toocluster-machines-and-configure-hello-sios-cluster-share-disk"></a>Lemezek toocluster hozzáadni, és konfigurálja a hello SIOS fürtlemez megosztás

Hozzá kell adnia egy új megosztás fürtlemez minden további SAP ASC/SCS-példányhoz. A Windows Server 2012 R2 hello WSFC megosztás fürtlemez jelenleg használatban, hello SIOS DataKeeper szoftveres megoldás.

A következő hello:
1. Adja hozzá a további lemezként vagy lemezekként, hello azonos méretűnek (amely kell toostripe) tooeach hello a fürtcsomópontok és formázni őket.
2. Storage replikáció konfigurálását a SIOS DataKeeper.

Az eljárás azt feltételezi, hogy még telepítette SIOS DataKeeper hello WSFC-fürt gépeken. Ha már telepítette azt, hello gépek közötti replikáció most konfigurálnia kell. a fő hello részletes leírása a hello folyamat [útmutató a magas rendelkezésre állású SAP NetWeaver Windows virtuális gépeken][sap-ha-guide-8.12.3.3].  

![A szinkron tükrözés hello új SAP ASC/SCS megosztás lemez DataKeeper][sap-ha-guide-figure-6006]

### <a name="deploy-vms-for-sap-application-servers-and-dbms-cluster"></a>Virtuális gépek SAP alkalmazáskiszolgálókra és az adatbázis-kezelő fürt üzembe helyezése

toocomplete hello infrastruktúra előkészítése hello második SAP rendszer hello a következő:

1. Az SAP alkalmazáskiszolgálók dedikált virtuális gépek telepítése, és azokat a saját dedikált rendelkezésre állási csoportban.
2. Adatbázis-kezelő fürt dedikált virtuális gépek telepítése, és azokat a saját dedikált rendelkezésre állási csoportban.


## <a name="install-hello-second-sap-sid2-netweaver-system"></a>Hello második SAP SID2 NetWeaver rendszer telepítése

hello teljes egy második SAP SID2 rendszer telepítését eljárást a fő hello [útmutató a magas rendelkezésre állású SAP NetWeaver Windows virtuális gépeken][sap-ha-guide-9].

hello magas szintű eljárás a következőképpen történik:

1. [Hello SAP első fürtcsomópontra telepíteni][sap-ha-guide-9.1.2].  
 Ebben a lépésben telepíti SAP, ha a magas rendelkezésre állású ASC/SCS példánya hello **létező WSFC-fürt csomópont 1**.

2. [Hello SAP profil hello ASC/SCS példány módosítása][sap-ha-guide-9.1.3].

3. [Konfigurálja a mintavételi portot][sap-ha-guide-9.1.4].  
 Ebben a lépésben a PowerShell használatával konfigurál egy SAP-erőforrás SAP-SID2-IP mintavételi portot. Hajtsa végre ezt a konfigurációt hello SAP ASC/SCS fürtcsomópontok egyike.

4. [Hello adatbázispéldány telepítés] [sap-magas rendelkezésre állású-útmutató-9.2].  
 Ebben a lépésben kívánja telepíteni az adatbázis-kezelő egy dedikált WSFC-fürtre.

5. [Hello második fürtcsomópont telepítés] [sap-magas rendelkezésre állású-útmutató-9.3].  
 Ebben a lépésben telepíti, ha a magas rendelkezésre állású ASC/SCS példánya hello létező WSFC fürtcsomóponton 2 SAP.

6. Nyissa meg a Windows tűzfal portjainak hello SAP ASC/SCS példányhoz, és ProbePort.  
 Mindkét fürtcsomóponton SAP ASC/SCS-példányok használt az összes Windows tűzfal portjainak SAP ASC/SCS használt nyitja meg. Ezeket a portokat hello szereplő [útmutató a magas rendelkezésre állású SAP NetWeaver Windows virtuális gépeken][sap-ha-guide-8.8].  
 Hello Azure belső terheléselosztási terheléselosztó mintavételi portot, amely a mi esetünkben 62350 is megnyithatja.

7. [Hello indítási típust hello SAP SSZON Windows szolgáltatáspéldány módosítása][sap-ha-guide-9.4].

8. [Hello SAP elsődleges alkalmazáskiszolgáló telepítése] [ sap-ha-guide-9.5] új hello a dedikált virtuális gép.

9. [Hello SAP további alkalmazáskiszolgáló telepítése] [ sap-ha-guide-9.6] új hello a dedikált virtuális gép.

10. [Tesztelje a hello SAP ASC/SCS példány feladatátvétel és SIOS replikációs][sap-ha-guide-10].

## <a name="next-steps"></a>Következő lépések

- [Hálózatkezelés korlátok: az Azure Resource Manager][networking-limits-azure-resource-manager]
- [Több virtuális IP-címek az Azure terheléselosztó][load-balancer-multivip-overview]
- [Útmutató a magas rendelkezésre állású SAP NetWeaver Windows virtuális gépeken][sap-ha-guide]
