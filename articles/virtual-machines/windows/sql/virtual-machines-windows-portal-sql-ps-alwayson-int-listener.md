---
title: "aaaConfigure mindig a rendelkezésre állási csoport figyelője – a Microsoft Azure |} Microsoft Docs"
description: "Konfigurálja a rendelkezésre állási csoport figyelője a hello Azure Resource Manager modellt, a belső terheléselosztók használatával egy vagy több IP-címmel."
services: virtual-machines
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: monicar
ms.assetid: 14b39cde-311c-4ddf-98f3-8694e01a7d3b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/22/2017
ms.author: mikeray
ms.openlocfilehash: 81edfe2c2ea536d8dcec466f36fccf8bc0e02c2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-one-or-more-always-on-availability-group-listeners---resource-manager"></a>Egy vagy több Always On rendelkezésre állási csoport figyelői - erőforrás-kezelő konfigurálása
Ez a témakör bemutatja, hogyan:

* Hozzon létre egy belső elosztott terhelésű az SQL Server rendelkezésre állási csoportok PowerShell-parancsmagok használatával.
* Adjon hozzá további IP címek tooa terheléselosztó egynél több rendelkezésre állási csoporthoz. 

Egy rendelkezésre állási csoport figyelőjének egy virtuális hálózat neve, hogy az ügyfelek kapcsolódnak toofor adatbázis eléréséhez. Az Azure virtuális gépeken a terheléselosztó hello figyelő hello IP-címet tartalmazza. hello terhelés terheléselosztó útvonalak forgalom toohello az SQL Server példányát, amely hello mintavételi portot figyel. Általában a rendelkezésre állási csoport belső terheléselosztót használja. Az Azure belső terheléselosztót rendelkezhet egy vagy több IP-címeket. Minden IP-cím egy adott mintavételi portot használja. Ez a dokumentum bemutatja, hogyan toouse PowerShell toocreate olyan terheléselosztóhoz, vagy adja hozzá az IP-címek tooan meglévő terheléselosztó SQL Server rendelkezésre állási csoportok. 

hello képességét tooassign több IP címek tooan belső elosztott terhelésű új tooAzure, és csak Resource Manager modellt érhető el. toocomplete ebben a feladatban van szüksége a Resource Manager modellt az Azure virtuális gépeken telepített egy SQL Server rendelkezésre állási csoport toohave. Mindkét SQL Server virtuális gépek kell tartozniuk toohello azonos rendelkezésre állási csoportot. Használhatja a hello [Microsoft sablon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) tooautomatically hello rendelkezésre állási csoport létrehozása az Azure Resource Manager. Ez a sablon hello rendelkezésre állási csoportból, beleértve a belső terheléselosztó hello automatikusan létrehozza. Ha kívánja, akkor [kézzel konfigurálásához az Always On rendelkezésre állási csoport](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Ez a témakör megköveteli, hogy a rendelkezésre állási csoportok már be van állítva.  

Kapcsolódó témakörök az alábbiak:

* [AlwaysOn rendelkezésre állási csoportok konfigurálása Azure virtuális gépen (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   
* [Egy VNet – VNet-kapcsolat beállítása az Azure Resource Manager és a PowerShell használatával](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

[!INCLUDE [Start your PowerShell session](../../../../includes/sql-vm-powershell.md)]

## <a name="configure-hello-windows-firewall"></a>A Windows tűzfal hello konfigurálása
Hello Windows tűzfal tooallow SQL Server hozzáférés konfigurálása. hello tűzfalszabályok engedélyezése hello figyelő mintavétel, valamint hello SQL Server-példány TCP-kapcsolatok toohello portok használatát. Részletes útmutatásért lásd: [beállítani a Windows tűzfalat a hozzáféréshez](http://msdn.microsoft.com/library/ms175043.aspx#Anchor_1). SQL Server port hello és hello mintavételi portot a bejövő szabályt létrehozni.

## <a name="example-script-create-an-internal-load-balancer-with-powershell"></a>Mintaparancsfájl: A belső terheléselosztók létrehozása a PowerShell használatával
> [!NOTE]
> Ha a rendelkezésre állási csoport hello létre [Microsoft sablon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md), hello belső terheléselosztóhoz már létre lett hozva. 
> 
> 

hello következő PowerShell-parancsfájlt hoz létre a belső terheléselosztók hello terheléselosztási szabályok konfigurálása és IP-cím beállítása hello terheléselosztóhoz. toorun hello parancsfájl, nyissa meg a Windows PowerShell ISE, és illessze be a hello parancsfájlt hello parancsfájl ablaktáblán. Használjon `Login-AzureRMAccount` a tooPowerShell toolog. Ha több Azure-előfizetéssel rendelkezik, `Select-AzureRmSubscription ` tooset hello előfizetés. 

```powershell
# Login-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<Resource Group Name>" # Resource group name
$VNetName = "<Virtual Network Name>"         # Virtual network name
$SubnetName = "<Subnet Name>"                # Subnet name
$ILBName = "<Load Balancer Name>"            # ILB name
$Location = "<Azure Region>"                 # Azure location
$VMNames = "<VM1>","<VM2>"                   # Virtual machine names

$ILBIP = "<n.n.n.n>"                         # IP address
[int]$ListenerPort = "<nnnn>"                # AG listener port
[int]$ProbePort = "<nnnn>"                   # Probe port

$LBProbeName ="ILBPROBE_$ListenerPort"       # hello Load balancer Probe Object Name              
$LBConfigRuleName = "ILBCR_$ListenerPort"    # hello Load Balancer Rule Object Name

$FrontEndConfigurationName = "FE_SQLAGILB_1" # Object name for hello front-end configuration 
$BackEndConfigurationName ="BE_SQLAGILB_1"   # Object name for hello back-end configuration

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 

$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName 

$FEConfig = New-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.id

$BEConfig = New-AzureRMLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName 

$SQLHealthProbe = New-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -Protocol tcp -Port $ProbePort -IntervalInSeconds 15 -ProbeCount 2

$ILBRule = New-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP 

$ILB= New-AzureRmLoadBalancer -Location $Location -Name $ILBName -ResourceGroupName $ResourceGroupName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -LoadBalancingRule $ILBRule -Probe $SQLHealthProbe 

$bepool = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB 

foreach($VMName in $VMNames)
    {
        $VM = Get-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VMName 
        $NICName = ($vm.NetworkProfile.NetworkInterfaces.Id.split('/') | select -last 1)
        $NIC = Get-AzureRmNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools = $BEPool
        Set-AzureRmNetworkInterface -NetworkInterface $NIC
        start-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VM.Name 
    }
```

## <a name="Add-IP"></a>Mintaparancsfájl: az IP cím tooan meglévő terheléselosztó felvétele a PowerShell használatával
toouse több mint egy rendelkezésre állási csoportból, vegyen fel egy további IP cím toohello terheléselosztót. Minden IP-cím a saját terheléselosztási szabály, a mintavételi portot és az első port szükséges.

hello előtér-port hello port, hogy alkalmazások tooconnect toohello SQL Server-példányt használ. Másik rendelkezésre állási csoportok használható IP-címek hello előtér-portra.

> [!NOTE]
> SQL Server rendelkezésre állási csoportok minden IP-cím szükséges egy adott mintavételi portot. Például ha egy IP-címet a terheléselosztóhoz 59999 mintavételi portot használ, nincs más IP-címeit, hogy a terheléselosztó mintavételi portot 59999 is használhat.

* A load balancer korlátok kapcsolatos információkért lásd: **terheléselosztó magánhálózati front-end IP-cím** alatt [hálózatkezelés korlátok - Azure Resource Manager](../../../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits).
* További információ a rendelkezésre állási csoport korlátok: [korlátozások (rendelkezésre állási csoportok)](http://msdn.microsoft.com/library/ff878487.aspx#RestrictionsAG).

hello következő parancsfájl hozzáad egy új IP cím tooan meglévő terheléselosztó. hello ILB hello figyelő portot hello terheléselosztási előtér-portot használja. Ezt a portot, hogy az SQL Server nem hello port lehet. Az SQL Server alapértelmezett példánya hello port az 1433-as. hello terheléselosztási szabály a rendelkezésre állási csoportban van szükség, egy fix IP-cím (közvetlen kiszolgálói válasz), hello háttér-portot kell hello azonos hello előtér-portjaként működik. A környezet hello változók frissítése. 

```powershell
# Login-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<ResourceGroup>"          # Resource group name
$VNetName = "<VirtualNetwork>"                  # Virtual network name
$SubnetName = "<Subnet>"                        # Subnet name
$ILBName = "<ILBName>"                          # ILB name                      

$ILBIP = "<n.n.n.n>"                            # IP address
[int]$ListenerPort = "<nnnn>"                   # AG listener port
[int]$ProbePort = "<nnnnn>"                     # Probe port 

$ILB = Get-AzureRmLoadBalancer -Name $ILBName -ResourceGroupName $ResourceGroupName 

$count = $ILB.FrontendIpConfigurations.Count+1
$FrontEndConfigurationName ="FE_SQLAGILB_$count"  

$LBProbeName = "ILBPROBE_$count"
$LBConfigrulename = "ILBCR_$count"

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 
$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName

$ILB | Add-AzureRmLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.Id 

$ILB | Add-AzureRmLoadBalancerProbeConfig -Name $LBProbeName  -Protocol Tcp -Port $Probeport -ProbeCount 2 -IntervalInSeconds 15  | Set-AzureRmLoadBalancer 

$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName

$FEConfig = get-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB

$SQLHealthProbe  = Get-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

$BEConfig = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $ILB.BackendAddressPools[0].Name -LoadBalancer $ILB 

$ILB | Add-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort  $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP | Set-AzureRmLoadBalancer   
```

## <a name="configure-hello-listener"></a>Hello figyelő konfigurálása

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

## <a name="set-hello-listener-port-in-sql-server-management-studio"></a>Az SQL Server Management Studio hello figyelő port beállítása

1. Indítsa el az SQL Server Management Studio eszközt, és csatlakozzon a toohello elsődleges másodpéldány.

1. Keresse meg a túl**AlwaysOn magas rendelkezésre állású** | **rendelkezésre állási csoportok** | **rendelkezésre állási csoport figyelői**. 

1. Most látnia kell a Feladatátvevőfürt-kezelő létrehozta hello figyelő nevét. Kattintson a jobb gombbal a hello figyelőjének nevével, és kattintson a **tulajdonságok**.

1. A hello **Port** hello rendelkezésre állási csoport figyelőjének hello portszáma hello segítségével adja meg a korábban használt $EndpointPort (1433 volt hello alapértelmezett), majd kattintson a **OK**.

## <a name="test-hello-connection-toohello-listener"></a>Teszt hello kapcsolat toohello figyelő

tootest hello kapcsolat:

1. RDP tooa hello az SQL Server azonos virtuális hálózat, de nem saját hello replika. Is ez hello hello fürt más SQL-kiszolgáló.

1. Használjon **sqlcmd** segédprogram tootest hello kapcsolat. Például a következő parancsfájl hello hoz létre egy **sqlcmd** kapcsolat toohello elsődleges replika hello figyelő Windows-hitelesítés használatával:
   
    ```
    sqlmd -S <listenerName> -E
    ```
   
    Ha hello figyelő eltérő portot használ hello alapértelmezett portot (1433), adja meg a hello portot hello kapcsolati karakterláncban. Például hello következő sqlcmd paranccsal összekapcsolja az tooa figyelési port 1435: 
   
    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

hello SQLCMD kapcsolat automatikusan csatlakozik a toowhichever példány SQL Server-gazdagépek hello elsődleges replika. 

> [!NOTE]
> Győződjön meg arról, hogy a megadott hello port a hello tűzfalat mindkét SQL Server nyitva-e. Mindkét kiszolgáló egy bejövő forgalomra vonatkozó szabály hello használt TCP-port szükséges. Lásd: [hozzáadása vagy szerkesztése tűzfalszabály](http://technet.microsoft.com/library/cc753558.aspx) további információt. 
> 
> 

## <a name="guidelines-and-limitations"></a>Irányelvek és korlátozások
Vegye figyelembe a következő irányelveket a rendelkezésre állási csoport figyelőjének az Azure-ban a belső terheléselosztó hello:

* A belső terheléselosztót, csak lépjen hello figyelő a hello belül azonos virtuális hálózatban.


## <a name="for-more-information"></a>További információ
További információkért lásd: [beállítása Always On rendelkezésre állási csoport az Azure virtuális gép manuálisan](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

## <a name="powershell-cmdlets"></a>PowerShell-parancsmagok
A következő PowerShell-parancsmagok toocreate az Azure virtuális gépek belső terheléselosztót hello használata.

* [Új AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt619450.aspx) új terheléselosztó létrehozása. 
* [Új AzureRMLoadBalancerFrontendIpConfig](http://msdn.microsoft.com/library/mt603510.aspx) hoz létre egy terheléselosztó előtér-IP-konfigurációja. 
* [Új AzureRmLoadBalancerRuleConfig](http://msdn.microsoft.com/library/mt619391.aspx) hoz létre egy terheléselosztó szabály konfigurációját. 
* [Új AzureRmLoadBalancerBackendAddressPoolConfig](http://msdn.microsoft.com/library/mt603791.aspx) hoz létre egy háttér címkészlet beállítása a terheléselosztóhoz. 
* [Új AzureRmLoadBalancerProbeConfig](http://msdn.microsoft.com/library/mt603847.aspx) hoz létre egy terhelés-kiegyenlítő mintavételi konfigurációját.
* [Remove-AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt603862.aspx) terheléselosztó eltávolítása egy Azure-erőforráscsoportot.
