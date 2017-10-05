---
title: "A LOB-alkalmazás tesztkörnyezetben |} Microsoft Docs"
description: "Útmutató a web-alapú, üzletági alkalmazás létrehozása a hibrid felhő környezetben informatikai pro vagy fejlesztői teszteléshez."
services: virtual-machines-windows
documentationcenter: 
author: JoeDavies-MSFT
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 92d2d8ce-60ed-4512-95e5-a7fe3b0ca00b
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/30/2016
ms.author: josephd
ms.openlocfilehash: 8e9a380458bfede80ed0fc1ee7e62b0caec09501
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-a-web-based-lob-application-in-a-hybrid-cloud-for-testing"></a>Webes üzletági alkalmazás telepítése hibrid felhőben tesztelés céljára
Ez a témakör végigvezeti egy üzletági (LOB) alkalmazások a Microsoft Azure-ban üzemeltetett webalapú üzletági tesztelési szimulált hibrid felhőkörnyezetekben létrehozása. Ez a létrejövő konfigurációt.

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

Ez a konfiguráció a következőkből áll:

* (A TestLab VNet) Azure-ban üzemeltetett szimulált a helyszíni hálózat.
* A létesítmények közötti virtuális hálózat (TestVNET) Azure-ban üzemeltetett.
* Egy VNet – VNet VPN-kapcsolatot.
* Egy webes LOB-kiszolgáló, a SQL server és a másodlagos tartományvezérlő TestVNET virtuális hálózatban.

Ez a konfiguráció alapján és közös kiindulási pont, amelyről is biztosít:

* Alkalmazások fejlesztéséhez és teszteléséhez LOB futó Internet Information Services (IIS) egy SQL Server 2014 adatbázis-fájlok az Azure-ban.
* A szimulált hibrid felhőalapú informatikai munkaterhelés tesztek végrehajtása.

A hibrid felhő tesztkörnyezet beállítása három fő fázisból áll:

1. Állítsa be a szimulált hibrid felhőalapú környezetben.
2. Konfigurálja az SQL-kiszolgálószámítógépet (SQL1).
3. A LOB-kiszolgáló (LOB1) konfigurálása.

A munkaterhelés Azure-előfizetés szükséges. Ha MSDN vagy a Visual Studio-előfizetéssel rendelkezik, tekintse meg [havi Azure-kredit a Visual Studio-előfizetők](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

Például egy termelési LOB-alkalmazás az Azure-ban, a **üzletági alkalmazások** architektúra tervezetének: [Microsoft szoftverek architektúra diagramok és tervrajzokat](http://msdn.microsoft.com/dn630664).

## <a name="phase-1-set-up-the-simulated-hybrid-cloud-environment"></a>1. fázis: A szimulált hibrid felhő környezet beállítása
Hozzon létre a [szimulált hibrid felhő tesztkörnyezet](ps-hybrid-cloud-test-env-sim.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Mivel ebben a tesztkörnyezetben nem igényel az APP1 kiszolgálót a Corpnet alhálózathoz jelenlétét, leállíthat azt a lépést.

Ez az az aktuális konfigurációt.

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph1.png)

## <a name="phase-2-configure-the-sql-server-computer-sql1"></a>2. fázis: Az SQL-kiszolgálószámítógépet (SQL1) konfigurálása
Azure-portálról indítsa el a DC2 számítógépen, szükség esetén.

Ezután hozzon létre egy virtuális gépet az sql1 számítógép a következő parancsokkal Azure PowerShell parancsot egy parancssorba a helyi számítógépen. Ezek a parancsok futtatása előtt adja meg a változó értéke, és távolítsa el a < és > karakter.

    $rgName="<your resource group name>"
    $locName="<the Azure location of your resource group>"
    $saName="<your storage account name>"

    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName SQL1 -VMSize Standard_A4
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-SQLDataDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name "Data" -DiskSizeInGB 100 -VhdUri $vhdURI  -CreateOption empty

    $cred=Get-Credential -Message "Type the name and password of the local administrator account for the SQL Server computer." 
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName SQL1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftSQLServer -Offer SQL2014-WS2012R2 -Skus Standard -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Az Azure portál segítségével csatlakoztassa az sql1 számítógépen az sql1 számítógépen a helyi rendszergazda fiók használatával.

Ezután konfigurálja a hálózati kapcsolat tesztelése és az SQL Server-forgalom engedélyezése a Windows tűzfal-szabályokat. Egy rendszergazda szintű Windows PowerShell parancssorból az sql1 számítógépen futtassa az alábbi parancsokat.

    New-NetFirewallRule -DisplayName "SQL Server" -Direction Inbound -Protocol TCP -LocalPort 1433,1434,5022 -Action allow 
    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

A ping parancs négy sikeres választ 192.168.0.4 IP-címről eredményez.

Ezután a lemez hozzáadása a további adatokat az sql1 számítógépen a meghajtó betűjelével F: új kötetként.

1. Kattintson a bal oldali ablaktáblában a Server Manager **fájl- és tárolási szolgáltatások**, és kattintson a **lemezek**.
2. A Tartalom ablaktáblán a a **lemezek** csoportjában kattintson a **2 lemez** (az a **partíció** beállítása **ismeretlen**).
3. Kattintson a **feladatok**, és kattintson a **új kötet**.
4. Az alapismeretek lapon az új kötet varázsló, kattintson a **következő**.
5. A Select a kiszolgáló és lemez lap, kattintson a **Disk 2**, és kattintson a **következő**. Amikor a rendszer kéri, kattintson a **OK**.
6. Kattintson a adja meg a kötet oldal méretét **következő**.
7. A hozzárendelés meghajtóbetűjel vagy mappa lapra, kattintson **következő**.
8. A fájl kijelölése rendszer beállítások lapján kattintson a **következő**.
9. Beállítások oldalon, kattintson a **létrehozása**.
10. Amikor végzett, kattintson **Bezárás**.

A parancsok a Windows PowerShell parancssorába az sql1 számítógépen:

    md f:\Data
    md f:\Log
    md f:\Backup

A következő sql1 számítógép csatlakoztatása a CORP Windows Server Active Directory-tartomány a következő parancsokkal az sql1 számítógépen a Windows PowerShell parancssorába.

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

Tartományi fiók hitelesítő adatait használjuk a corp\felhasználó1 fiókkal, amikor a rendszer kéri a **Add-Computer** parancsot.

Az újraindítás után az Azure-portálon való csatlakozáskor használandó SQL1 *az sql1 számítógépen a helyi rendszergazdai fiókkal*.

Ezután konfigurálja az SQL Server 2014 F: meghajtó használatára, az új adatbázisokat és a felhasználói fiók engedélyeit.

1. Írja be a kezdőképernyőről **az SQL Server felügyeleti**, és kattintson a **SQL Server 2014 Management Studio**.
2. A **kapcsolódás a kiszolgálóhoz**, kattintson a **Connect**.
3. Az Object Explorer fát megjelenítő ablaktáblán kattintson a jobb gombbal **SQL1**, és kattintson a **tulajdonságok**.
4. Az a **kiszolgáló tulajdonságai** ablak, kattintson a **adatbázis beállításainak**.
5. Keresse meg a **alapértelmezett helyek adatbázis** és állítsa be ezeket az értékeket: 
   * A **adatok**, írja be a elérési utat **f:\Data**.
   * A **napló**, írja be a elérési utat **f:\Log**.
   * A **biztonsági mentés**, írja be a elérési utat **f:\Backup**.
   * Megjegyzés: Csak az új adatbázisok használja ezeket a helyeket.
6. Kattintson a **OK** az ablak bezárásához.
7. Az a **Object Explorer** fát megjelenítő ablaktáblán, nyissa meg **biztonsági**.
8. Kattintson a jobb gombbal **bejelentkezések** majd **új bejelentkezés**.
9. A **bejelentkezési név**, típus **corp\felhasználó1**.
10. A a **kiszolgálói szerepkörök** kattintson **sysadmin**, és kattintson a **OK**.
11. Zárja be a Microsoft SQL Server Management Studio eszközt.

Ez az az aktuális konfigurációt.

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph2.png)

## <a name="phase-3-configure-the-lob-server-lob1"></a>3. fázis: A LOB-kiszolgáló (LOB1) konfigurálása
Először hozzon létre egy virtuális gépet a LOB1 a következő parancsokkal a helyi számítógépen Azure PowerShell parancssorába.

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<your storage account name>"

    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName LOB1 -VMSize Standard_A2
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for LOB1."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName LOB1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/LOB1-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name LOB1-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Ezután használhatja az Azure-portál csatlakozni LOB1 LOB1 a helyi rendszergazdai fiók hitelesítő adatait.

Ezután konfigurálja a Windows tűzfal szabály forgalom engedélyezésére hálózati kapcsolat tesztelése. Egy rendszergazda szintű Windows PowerShell parancssorból a LOB1 futtassa az alábbi parancsokat.

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

A ping parancs négy sikeres választ 192.168.0.4 IP-címről eredményez.

A következő LOB1 csatlakoztatása a CORP Active Directory-tartomány, a következő parancsokkal a Windows PowerShell parancssorába.

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

Tartományi fiók hitelesítő adatait használjuk a corp\felhasználó1 fiókkal, amikor a rendszer kéri a **Add-Computer** parancsot.

Az újraindítás után csatlakozhat az Azure-portálon LOB1 a corp\felhasználó1 fiókkal és jelszóval.

A következő LOB1 konfigurálása az IIS és az ÜGYFÉL1-hozzáférés tesztelése.

1. A Kiszolgálókezelőben kattintson **szerepkörök és szolgáltatások hozzáadása**.
2. Az a **megkezdése előtt** kattintson **következő**.
3. Az a **telepítés típusának kiválasztása** kattintson **következő**.
4. Az a **jelölje be a célkiszolgáló** kattintson **következő**.
5. Az a **kiszolgálói szerepkörök** kattintson **webkiszolgáló (IIS)** listájában **szerepkörök**.
6. Amikor a rendszer kéri, kattintson a **szolgáltatások hozzáadása**, és kattintson a **következő**.
7. Az a **szolgáltatások kiválasztása** kattintson **következő**.
8. Az a **webkiszolgáló (IIS)** kattintson **következő**.
9. Az a **szerepkör-szolgáltatások kiválasztása** lapon jelölje be vagy törölje a jelet a jelölőnégyzetből, a szolgáltatások, a LOB-alkalmazás teszteléséhez kell, és kattintson a **következő**.
10. Az a **erősítse meg a telepítendő** kattintson **telepítése**.
11. Várjon, amíg az összetevők telepítése befejeződött, és kattintson a **Bezárás**.
12. Azure-portálról csatlakozzon a CLIENT1 számítógépre a corp\felhasználó1 hitelesítő adatai, és indítsa el az Internet Explorer.
13. Írja be a címsorba **http://lob1/** , és nyomja le az ENTER BILLENTYŰT. Az alapértelmezett IIS 8 weblap kell megjelennie.

Ez az az aktuális konfigurációt.

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

Ebben a környezetben, hogy a webes alkalmazás LOB1 telepítse és tesztelje a Corpnet alhálózathoz az ÜGYFÉL1 funkciói készen áll.

## <a name="next-step"></a>Következő lépés
* Adja hozzá egy új virtuális gép használata a [Azure-portálon](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

