---
title: "hibrid felhő tesztkörnyezet aaaSimulated |} Microsoft Docs"
description: "Hozzon létre egy szimulált hibrid felhőalapú környezetet informatikai pro vagy fejlesztői tesztelni, két Azure virtuális hálózat és VNet – VNet-kapcsolatot."
services: virtual-machines-windows
documentationcenter: 
author: JoeDavies-MSFT
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: ca5bf294-8172-44a9-8fed-d6f38d345364
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/30/2016
ms.author: josephd
ms.openlocfilehash: 6063022e373c2b3564e040c4fbee122e2438974b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-simulated-hybrid-cloud-environment-for-testing"></a>Szimulált hibridfelhő-környezet kialakítása teszteléshez
Ez a cikk végigvezeti egy szimulált hibrid felhőkörnyezet létrehozása a Microsoft Azure-ban két Azure virtuális hálózatot használ. Itt található hello létrejövő konfigurációt.

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)

Hibrid felhő éles környezetben szimulálja, és a következő tartalmazza:

* A szimulált és egyszerűsített helyszíni hálózatot tárolva az Azure virtuális hálózat (hello TestLab virtuális hálózat).
* (TestVNET) Azure-ban üzemeltetett szimulált létesítmények közötti virtuális hálózat.
* VNet – VNet hello két virtuális hálózatok közötti kapcsolatot.
* A másodlagos tartományvezérlő hello TestVNET virtuális hálózatban.

Ez biztosít, amelyben pont alapja és közös indítása:

* Alkalmazások fejlesztéséhez és teszteléséhez a szimulált hibrid felhőkörnyezetekben.
* Vizsgálati beállítások számítógépek, néhány hello TestLab virtuális hálózaton belül, és néhány toosimulate hibrid felhőalapú informatikai munkaterhelések hello TestVNET virtuális hálózaton belül létrehozása.

A hibrid felhő tesztkörnyezet négy fő fázisokat toosetting van:

1. Hello TestLab virtuális hálózat konfigurálása.
2. Hello létesítmények közötti virtuális hálózat létrehozása.
3. Hello VNet – VNet VPN-kapcsolat létrehozása.
4. A DC2 konfigurálása. 

Ez a konfiguráció Azure-előfizetés szükséges. Ha MSDN vagy a Visual Studio-előfizetéssel rendelkezik, tekintse meg [havi Azure-kredit a Visual Studio-előfizetők](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

> [!NOTE]
> Virtuális gépek és virtuális hálózati átjárók az Azure-ban fel Önnek egy folyamatban lévő a költség, amikor futnak. Az ehhez kapcsolódó költséget szemben az MSDN Számlázva vagy -előfizetéssel. Az Azure VPN gateway, két Azure virtuális gépek halmaza lett megvalósítva. toominimize hello költségek, hello tesztelési környezet létrehozása, és végezze el a szükséges tesztelés és bemutató lehető leggyorsabban tegye.
> 
> 

## <a name="phase-1-configure-hello-testlab-virtual-network"></a>1. fázis: Hello TestLab virtuális hálózat konfigurálása
Hello utasítások használata hello [alapkonfiguráció tesztkörnyezetben](https://technet.microsoft.com/library/mt771177.aspx) témakör tooconfigure hello DC1, APP1 és CLIENT1 számítógépek hello nevű TestLab Azure virtuális hálózatot. 

Ezután indítsa el az Azure PowerShell-promptot.

> [!NOTE]
> hello a következő parancs úgy állítja be az Azure PowerShell 1.0-ás vagy újabb verzióját használja.
> 
> 

Jelentkezzen be tooyour fiókjával.

    Login-AzureRMAccount

Az előfizetés nevét, a következő parancs hello segítségével beolvasása.

    Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName

Állítsa be az Azure-előfizetéshez. Használja az azonos hello toobuild 1. szakasza az alapkonfiguráció használt előfizetés. Cserélje le mindent, ami hello idézőjelek között, beleértve a hello < és > karakter, helyes hello nevét.

    $subscr="<subscription name>"
    Get-AzureRmSubscription –SubscriptionName $subscr | Select-AzureRmSubscription

Ezután adja hozzá egy átjáró alhálózati toohello TestLab virtuális hálózat a alapkonfiguráció lépéseit, amely használt toohost hello Azure átjáró lesz.

    $rgName="<name of your resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed hello TestLab virtual network, such as West US>"
    $vnet=Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name TestLab
    Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 10.255.255.248/29 -VirtualNetwork $vnet
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

A következő kérelmet egy nyilvános IP-cím tooassign toohello átjáró hello TestLab virtuális hálózat.

    $gwpip=New-AzureRmPublicIpAddress -Name TestLab_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic

Ezután hozzon létre az átjárót.

    $vnet=Get-AzureRmVirtualNetwork -Name TestLab -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name TestLab_GWConfig -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 
    New-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

Ne feledje, hogy új átjárók is igénybe vehet 20 perc vagy több toocreate.

Hello Azure-portál a helyi számítógépen, a csatlakozás tooDC1 hello corp\felhasználó1 hitelesítő adatait. tooconfigure hello CORP tartományhoz, hogy a felhasználók és számítógépek használja a hitelesítéshez, a helyi tartományvezérlő futtassa ezeket a parancsokat egy rendszergazda szintű Windows PowerShell parancssorból a DC1-en.

    New-ADReplicationSite -Name "TestLab" 
    New-ADReplicationSite -Name "TestVNET"
    New-ADReplicationSubnet -Name "10.0.0.0/8" -Site "TestLab"
    New-ADReplicationSubnet -Name "192.168.0.0/16" -Site "TestVNET"

Ez az az aktuális konfigurációt.

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph1.png)

## <a name="phase-2-create-hello-testvnet-virtual-network"></a>2. fázis: Hello TestVNET virtuális hálózat létrehozása
Először hello TestVNET virtuális hálózat létrehozása, és a hálózati biztonsági csoport védelmét.

    $rgName="<name of hello resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed hello TestLab virtual network, such as West US>"
    $locShortName="<Azure location name from $locName in all lowercase letters with spaces removed. Example:  westus>"
    $testSubnet=New-AzureRMVirtualNetworkSubnetConfig -Name "TestSubnet" -AddressPrefix 192.168.0.0/24
    $gatewaySubnet=New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 192.168.255.248/29
    New-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName -Location $locName -AddressPrefix 192.168.0.0/16 -Subnet $testSubnet,$gatewaySubnet –DNSServer 10.0.0.4
    $rule1=New-AzureRMNetworkSecurityRuleConfig -Name "RDPTraffic" -Description "Allow RDP tooall VMs on hello subnet" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 3389
    New-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName -Location $locShortName -SecurityRules $rule1
    $vnet=Get-AzureRMVirtualNetwork -ResourceGroupName $rgName -Name TestVNET
    $nsg=Get-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName
    Set-AzureRMVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet" -AddressPrefix 192.168.0.0/24 -NetworkSecurityGroup $nsg

A következő kérelmet egy nyilvános IP-cím toobe toohello átjáró hello TestVNET virtuális hálózat számára lefoglalt, és létrehozhatja az átjárót.

    $gwpip=New-AzureRmPublicIpAddress -Name TestVNET_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $vnet=Get-AzureRmVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name "TestVNET_GWConfig" -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
    New-AzureRmVirtualNetworkGateway -Name "TestVNET_GW" -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

Ez az az aktuális konfigurációt.

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph2.png)

## <a name="phase-3-create-hello-vnet-to-vnet-connection"></a>3. fázis: Hello VNet – VNet-kapcsolat létrehozása
Először szerezze be a véletlenszerű, erős titkosítással, 32 karakterből álló előmegosztott kulccsal, a hálózati vagy biztonsági rendszergazdától. Alternatív megoldásként használjon hello információt [hozzon létre egy véletlenszerű karakterlánc egy IPsec előmegosztott kulcs](http://social.technet.microsoft.com/wiki/contents/articles/32330.create-a-random-string-for-an-ipsec-preshared-key.aspx) tooobtain előmegosztott kulcs.

A következő ezen parancsok toocreate hello VNet – VNet VPN-kapcsolat használata, ami eltarthat némi időt toocomplete.

    $sharedKey="<pre-shared key value>"
    $gwTestLab=Get-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName
    $gwTestVNET=Get-AzureRmVirtualNetworkGateway -Name TestVNET_GW -ResourceGroupName $rgName
    New-AzureRmVirtualNetworkGatewayConnection -Name TestLab_to_TestVNET -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestLab -VirtualNetworkGateway2 $gwTestVNET -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey
    New-AzureRmVirtualNetworkGatewayConnection -Name TestVNET_to_TestLab -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestVNET -VirtualNetworkGateway2 $gwTestLab -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey

Néhány perc elteltével hello kapcsolatot kell létrehozni.

Ez az az aktuális konfigurációt.

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph3.png)

## <a name="phase-4-configure-dc2"></a>4. fázis: A DC2 konfigurálása
Először hozzon létre egy virtuális gép dc2 tartományvezérlő számára. A parancsok hello Azure PowerShell parancssorába a helyi számítógépen.

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<hello storage account name for hello base configuration>"
    $vnet=Get-AzureRMVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $pip=New-AzureRMPublicIpAddress -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -PrivateIpAddress 192.168.0.4
    $vm=New-AzureRMVMConfig -VMName DC2 -VMSize Standard_A1
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestVNET-ADDSDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name ADDS-Data -DiskSizeInGB 20 -VhdUri $vhdURI  -CreateOption empty
    $cred=Get-Credential -Message "Type hello name and password of hello local administrator account for DC2."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName DC2 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name DC2-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Ezután csatlakoztassa toohello új DC2 virtuális gép hello Azure-portálon.

Ezután konfigurálja a hálózati kapcsolat tesztelése a Windows tűzfal szabály tooallow forgalom. Egy rendszergazda szintű Windows PowerShell parancssorból a dc2 számítógépen futtassa az alábbi parancsokat.

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc1.corp.contoso.com

hello ping parancs négy sikeres választ a következő IP-10.0.0.4 eredményez. Ez egy tesztművelet forgalom hello VNet – VNet-kapcsolaton keresztül.

Ezután adja hozzá a hello további adatlemezt a dc2 számítógépen hello meghajtóbetűjellel F: új kötetként.

1. Hello bal oldali ablaktáblán Kiszolgálókezelő kattintson **fájl- és tárolási szolgáltatások**, és kattintson a **lemezek**.
2. Hello Tartalom ablaktáblán, a hello **lemezek** csoportjában kattintson a **2 lemez** (a hello **partíció** túl beállítása**ismeretlen**).
3. Kattintson a **feladatok**, és kattintson a **új kötet**.
4. A hello hello új kötet varázsló oldalán elkezdéséhez kattintson **következő**.
5. Hello válassza hello kiszolgáló és lemez oldal, kattintson a **Disk 2**, és kattintson a **következő**. Amikor a rendszer kéri, kattintson a **OK**.
6. A hello hello méret megadása hello kötet lap, kattintson **következő**.
7. Oldalon hello hozzárendelése tooa meghajtó betűjele vagy a mappát, kattintson a **következő**.
8. Hello fájl kijelölése rendszer beállítások lapon kattintson **következő**.
9. Hello megerősítése összetevők megerősítése oldalon kattintson **létrehozása**.
10. Amikor végzett, kattintson **Bezárás**.

Ezután konfigurálja a DC2 hello corp.contoso.com tartományhoz tartozó replika tartományvezérlőként működik. A parancsok Windows PowerShell-parancssorból hello a dc2 számítógépen.

    Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
    Install-ADDSDomainController -Credential (Get-Credential CORP\User1) -DomainName "corp.contoso.com" -InstallDns:$true -DatabasePath "F:\NTDS" -LogPath "F:\Logs" -SysvolPath "F:\SYSVOL"

Vegye figyelembe, hogy-e rákérdezéses toosupply mindkét corp\felhasználó1 jelszó és a Directory címtárszolgáltatások helyreállító módjában (DSRM) jelszavát, és a toorestart DC2 hello.

Most, hogy hello TestVNET virtuális hálózatnak a saját DNS-kiszolgáló (DC2), a DNS-kiszolgáló hello TestVNET virtuális hálózati toouse kell konfigurálnia.

1. Hello hello Azure-portálon a bal oldali ablaktáblán, kattintson hello virtuális hálózatok ikonra, majd **TestVNET**.
2. A hello **beállítások** lapra, majd **DNS-kiszolgálók**.
3. A **elsődleges DNS-kiszolgáló**, típus **192.168.0.4** tooreplace 10.0.0.4.
4. Kattintson a **Save** (Mentés) gombra.

Ez az az aktuális konfigurációt. 

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)

A szimulált hibrid felhőkörnyezet most már készen áll a tesztelésre.

## <a name="next-step"></a>Következő lépés
* Állítson be egy [web-alapú, üzletági alkalmazás](ps-hybrid-cloud-test-env-lob.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ebben a környezetben.

