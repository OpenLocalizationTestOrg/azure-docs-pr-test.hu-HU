---
title: "aaaLOB alkalmazás tesztkörnyezetben |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate a web-alapú, üzletági alkalmazás egy hibrid felhőalapú környezetet informatikai pro vagy fejlesztői teszteléshez."
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
ms.openlocfilehash: e9f825bbb1cbeb841ee3c974ebf6721dc236c311
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-web-based-lob-application-in-a-hybrid-cloud-for-testing"></a>Webes üzletági alkalmazás telepítése hibrid felhőben tesztelés céljára
Ez a témakör végigvezeti egy üzletági (LOB) alkalmazások a Microsoft Azure-ban üzemeltetett webalapú üzletági tesztelési szimulált hibrid felhőkörnyezetekben létrehozása. Itt található hello létrejövő konfigurációt.

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

Ez a konfiguráció a következőkből áll:

* A szimulált a helyi hálózaton (hello TestLab VNet) Azure-ban üzemeltetett.
* A létesítmények közötti virtuális hálózat (TestVNET) Azure-ban üzemeltetett.
* Egy VNet – VNet VPN-kapcsolatot.
* A web-alapú LOB server, a SQL server és a másodlagos tartományvezérlő hello TestVNET virtuális hálózatban.

Ez a konfiguráció alapján és közös kiindulási pont, amelyről is biztosít:

* Alkalmazások fejlesztéséhez és teszteléséhez LOB futó Internet Information Services (IIS) egy SQL Server 2014 adatbázis-fájlok az Azure-ban.
* A szimulált hibrid felhőalapú informatikai munkaterhelés tesztek végrehajtása.

A hibrid felhő tesztkörnyezet három fő fázisokat toosetting van:

1. Hello szimulált hibrid felhő környezet beállítása.
2. Hello az SQL server számítógép (SQL1) konfigurálása.
3. Hello LOB-kiszolgáló (LOB1) konfigurálása.

A munkaterhelés Azure-előfizetés szükséges. Ha MSDN vagy a Visual Studio-előfizetéssel rendelkezik, tekintse meg [havi Azure-kredit a Visual Studio-előfizetők](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

Az Azure-ban tárolt LOB-alkalmazás termelési példáért lásd: hello **üzletági alkalmazások** architektúra tervezetének: [Microsoft szoftverek architektúra diagramok és tervrajzokat](http://msdn.microsoft.com/dn630664).

## <a name="phase-1-set-up-hello-simulated-hybrid-cloud-environment"></a>1. fázis: Hello szimulált hibrid felhő környezet beállítása
Hozzon létre hello [szimulált hibrid felhő tesztkörnyezet](ps-hybrid-cloud-test-env-sim.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Mivel ebben a tesztkörnyezetben nem szükséges hello az APP1 kiszolgáló hello Corpnet alhálózat hello jelenlétét, leállíthat azt a lépést.

Ez az az aktuális konfigurációt.

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph1.png)

## <a name="phase-2-configure-hello-sql-server-computer-sql1"></a>2. fázis: Hello az SQL server számítógép (SQL1) konfigurálása
Hello Azure-portálon, a start hello DC2 számítógépen, szükség esetén.

Ezután hozzon létre egy virtuális gépet az sql1 számítógép a következő parancsokkal Azure PowerShell parancsot egy parancssorba a helyi számítógépen. Előzetes toorunning ezek a parancsok adja meg hello változók értékeinek és eltávolítása hello < és > karakter.

    $rgName="<your resource group name>"
    $locName="<hello Azure location of your resource group>"
    $saName="<your storage account name>"

    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName SQL1 -VMSize Standard_A4
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-SQLDataDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name "Data" -DiskSizeInGB 100 -VhdUri $vhdURI  -CreateOption empty

    $cred=Get-Credential -Message "Type hello name and password of hello local administrator account for hello SQL Server computer." 
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName SQL1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftSQLServer -Offer SQL2014-WS2012R2 -Skus Standard -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Használja az Azure portál tooconnect tooSQL1 hello hello helyi rendszergazdai fiókkal az SQL1.

Ezután konfigurálja a Windows tűzfal szabályai tooallow hálózati kapcsolat tesztelése és az SQL Server-forgalmat. Egy rendszergazda szintű Windows PowerShell parancssorból az sql1 számítógépen futtassa az alábbi parancsokat.

    New-NetFirewallRule -DisplayName "SQL Server" -Direction Inbound -Protocol TCP -LocalPort 1433,1434,5022 -Action allow 
    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

hello ping parancs négy sikeres választ 192.168.0.4 IP-címről eredményez.

Ezután adja hozzá a hello további adatlemezt az sql1 számítógépen hello meghajtóbetűjellel F: új kötetként.

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

A parancsok hello Windows PowerShell parancssorába az sql1 számítógépen:

    md f:\Data
    md f:\Log
    md f:\Backup

A következő csatlakoztassa az sql1 számítógép toohello CORP Windows Server Active Directory-tartomány az sql1 számítógépen a Windows PowerShell-parancssorba hello parancsokkal.

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

Hello corp\felhasználó1 fiókkal, amikor a rendszer kéri a hello toosupply tartományi fiók hitelesítő adatainak **Add-Computer** parancsot.

Az újraindítás után használja az Azure portál tooconnect tooSQL1 hello *hello helyi rendszergazdai fiókkal az SQL1*.

Ezután konfigurálja az SQL Server 2014 toouse hello F: meghajtó az új adatbázisokat és a felhasználói fiók engedélyeit.

1. Hello kezdőképernyőről írja be a **az SQL Server felügyeleti**, és kattintson a **SQL Server 2014 Management Studio**.
2. A **tooServer csatlakozás**, kattintson a **Connect**.
3. Hello Object Explorer navigációs ablaktáblájában kattintson a jobb gombbal **SQL1**, és kattintson a **tulajdonságok**.
4. A hello **kiszolgáló tulajdonságai** ablak, kattintson a **adatbázis beállításainak**.
5. Keresse meg a hello **alapértelmezett helyek adatbázis** és állítsa be ezeket az értékeket: 
   * A **adatok**, a típus hello elérési útjának **f:\Data**.
   * A **napló**, a típus hello elérési útjának **f:\Log**.
   * A **biztonsági mentés**, a típus hello elérési útjának **f:\Backup**.
   * Megjegyzés: Csak az új adatbázisok használja ezeket a helyeket.
6. Kattintson a hello **OK** tooclose hello ablak.
7. A hello **Object Explorer** fát megjelenítő ablaktáblán, nyissa meg **biztonsági**.
8. Kattintson a jobb gombbal **bejelentkezések** majd **új bejelentkezés**.
9. A **bejelentkezési név**, típus **corp\felhasználó1**.
10. A hello **kiszolgálói szerepkörök** kattintson **sysadmin**, és kattintson a **OK**.
11. Zárja be a Microsoft SQL Server Management Studio eszközt.

Ez az az aktuális konfigurációt.

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph2.png)

## <a name="phase-3-configure-hello-lob-server-lob1"></a>3. fázis: Hello LOB-kiszolgáló (LOB1) konfigurálása
Először hozzon létre egy virtuális gépet a LOB1 a következő parancsokkal hello Azure PowerShell-parancssorba a helyi számítógépen.

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<your storage account name>"

    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName LOB1 -VMSize Standard_A2
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $cred=Get-Credential -Message "Type hello name and password of hello local administrator account for LOB1."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName LOB1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/LOB1-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name LOB1-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Ezt követően használható hello Azure portál tooconnect tooLOB1 hello LOB1 hello helyi rendszergazdai fiókjának hitelesítő adatait.

Ezután konfigurálja a hálózati kapcsolat tesztelése a Windows tűzfal szabály tooallow forgalom. Egy rendszergazda szintű Windows PowerShell parancssorból a LOB1 futtassa az alábbi parancsokat.

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

hello ping parancs négy sikeres választ 192.168.0.4 IP-címről eredményez.

A következő tartományhoz LOB1 toohello CORP Active Directory Windows PowerShell-parancssorba hello parancsokkal.

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

Hello corp\felhasználó1 fiókkal, amikor a rendszer kéri a hello toosupply tartományi fiók hitelesítő adatainak **Add-Computer** parancsot.

Az újraindítás után használni hello Azure portál tooconnect tooLOB1 hello corp\felhasználó1 fiókot és jelszót.

A következő LOB1 konfigurálása az IIS és az ÜGYFÉL1-hozzáférés tesztelése.

1. A Kiszolgálókezelőben kattintson **szerepkörök és szolgáltatások hozzáadása**.
2. A hello **megkezdése előtt** kattintson **következő**.
3. A hello **telepítés típusának kiválasztása** kattintson **következő**.
4. A hello **jelölje be a célkiszolgáló** kattintson **következő**.
5. A hello **kiszolgálói szerepkörök** kattintson **webkiszolgáló (IIS)** hello listájában **szerepkörök**.
6. Amikor a rendszer kéri, kattintson a **szolgáltatások hozzáadása**, és kattintson a **következő**.
7. A hello **szolgáltatások kiválasztása** kattintson **következő**.
8. A hello **webkiszolgáló (IIS)** kattintson **következő**.
9. A hello **szerepkör-szolgáltatások kiválasztása** lapon válassza ki vagy törölje a LOB-alkalmazás teszteléséhez kell hello szolgáltatások hello jelölőnégyzeteit, és kattintson a **következő**.
10. A hello **erősítse meg a telepítendő** kattintson **telepítése**.
11. Várjon, amíg az összetevők hello telepítése befejeződött, és kattintson a **Bezárás**.
12. Hello Azure-portálon, a csatlakozás toohello ÜGYFÉL1 számítógép hello corp\felhasználó1 fiók hitelesítő adatait, és indítsa el az Internet Explorer.
13. Hello címsorába írja be a **http://lob1/** , és nyomja le az ENTER BILLENTYŰT. Hello alapértelmezett IIS 8 weblap kell megjelennie.

Ez az az aktuális konfigurációt.

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

Ez a környezet már készen áll a akkor toodeploy a webes alkalmazás LOB1 és a vizsgálati funkcióival ÜGYFÉL1 hello Corpnet alhálózat.

## <a name="next-step"></a>Következő lépés
* Adja hozzá egy új virtuális gép hello [Azure-portálon](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

