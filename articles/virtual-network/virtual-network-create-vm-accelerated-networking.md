---
title: "egy Azure virtuális gép a hálózat elérését gyorsítja fel aaaCreate |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate egy virtuális gép a hálózat elérését gyorsítja fel."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/10/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 241362891bfe083ab32c2f558be54f63f87c0693
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-with-accelerated-networking"></a>A hálózat elérését gyorsítja fel a virtuális gép létrehozása

Ebben az oktatóanyagban elsajátíthatja, hogyan toocreate egy Azure virtuális gép (VM) a hálózat elérését gyorsítja fel. Gyorsított hálózatkezelés a Windows és a nyilvános előzetes verziójában az adott Linux terjesztésekről GA. Gyorsított hálózatkezelés lehetővé teszi, hogy az egygyökerű i/o-virtualizálás (SR-IOV) tooa VM, a hálózati teljesítmény nagy mértékben javítva. A nagy teljesítményű elérési út nincs hatással a hello gazdagép hello datapath csökkenti a késést, a jitter és a CPU-felhasználás legnagyobb igénybevételt jelentő hálózati munkaterhelését hello támogatott Virtuálisgép-típusokon való használatra. hello rendelkező és anélküli gyorsított hálózatkezelés a képen látható kommunikációs következő két virtuális gép (VM) között:

![Összehasonlítása](./media/virtual-network-create-vm-accelerated-networking/image1.png)

Gyorsított hálózatkezelés nélkül, az összes hálózati forgalom mindkét virtuális gép hello be kell járnia hello gazdagépről, illetve hello virtuális kapcsolót. hello virtuális kapcsoló betartatja az összes, például hálózati biztonsági csoportok, hozzáférhet szabálygyűjteményeket, elkülönítési és egyéb hálózati szempontból virtualizált toonetwork adatforgalmának. További információk a virtuális kapcsolók, olvassa el a hello toolearn [Hyper-V hálózatvirtualizálás és a virtuális kapcsoló](https://technet.microsoft.com/library/jj945275.aspx) cikk.

Gyorsított hálózattal, a hálózati forgalom érkezik hello VM hálózati illesztőt (NIC), és ezután kerül toohello virtuális gép. Minden hálózati házirendek, amelyek a virtuális kapcsoló hello nélkül gyorsított hálózatkezelés kiszervezve, és a hardver alkalmazza a vonatkozik. A hardver lehetővé teszi, hogy hello NIC tooforward házirend alkalmazása hálózati forgalom közvetlenül toohello VM kihagyásával hello gazdagépről, illetve hello virtuális kapcsolót, minden hello házirenddel azt hello fogadó megőrzésével.

hello gyorsított hálózatkezelés előnyei csak akkor jutnak érvényre toohello, hogy engedélyezve van a virtuális gép. A hello legjobb eredmény érdekében ideális tooenable legalább két virtuális gépeken Ez a szolgáltatás kapcsolódó toohello azonos Azure Virtual Network (VNet). Ha kommunikál a Vnetek vagy az összekötő a helyszíni, ez a szolgáltatás rendelkezik gyakorolt minimális hatás mellett toooverall késés.

> [!WARNING]
> A nyilvános előzetes verzió nem lehet azonos szintű rendelkezésre állást és megbízhatóságot, szolgáltatásokat, amelyek hello Linux általában elérhető kiadásai. hello szolgáltatás nem támogatott, van, korlátozott képességek, és előfordulhat, hogy nem érhető el az összes Azure helyét. A legfrissebb értesítések hello rendelkezésre állás és a szolgáltatás állapotának tekintse meg hello Azure Virtual Network frissítések lap.

## <a name="benefits"></a>Előnyök
* **Késés csökkentése / magasabb csomag / másodperc (pps):** eltávolításával hello virtuális kapcsoló a hello datapath hello idejének csomagok töltött eltávolítja hello gazdagép a házirend-feldolgozás, és növeli hello belül hello VM feldolgozható csomagok száma.
* **Alacsonyabb jitter:** virtuális kapcsoló feldolgozása, amelyet a toobe alkalmazott házirend hello mennyiségét és hello hello CPU azon munkaterhelés, amely hello feldolgozási függ. Hello házirend kényszerítési toohello hardver kiszervezésével megszüntetéséhez, hogy a variancia csomagok továbbítása közvetlen toohello VM hello állomás tooVM kommunikáció és az összes szoftver megszakítások és a környezet eltávolítása vált.
* **Csökkent a CPU-felhasználás:** Bypassing hello virtuális kapcsoló hello fogadó vezet a hálózati forgalom feldolgozása tooless CPU-használatot.

## <a name="Limitations"></a>Korlátozások
a következő korlátozások hello létezik ez a funkció használata esetén:

* **A hálózati illesztő létrehozása:** gyorsított hálózat csak akkor engedélyezhető, az új hálózati Nem engedélyezhető az egy meglévő hálózati adaptert.
* **Virtuális gép létrehozása:** A hálózati adapter engedélyezve gyorsított hálózattal csak a virtuális gép kerül létrehozásra csatolt tooa méretű lehet, ha hello lehet. hello hálózati adapter nem lehet meglévő virtuális gép csatlakoztatott tooan.
* **Régiók:** Windows-alapú virtuális gépek gyorsított hálózattal érhető el az Azure-régióban. Linux virtuális gépek gyorsított hálózattal több régióba érhető el. Ez a funkció érhető el a hello régiók növekszik. Ellenőrizze a hello Azure virtuális hálózat frissítések blog alatt hello legújabb információkat.   
* **Támogatott operációs rendszerekről:** Windows: Microsoft Windows Server 2012 R2 Datacenter és a Windows Server 2016. Linux: Ubuntu Server 16.04 kernel 4.4.0-77 vagy annál újabb LTS, SLES 12 SP2, RHEL 7.3 és CentOS 7.3 (közzétett "Engedélyezetlen Wave szoftver").
* **Virtuálisgép-méret:** általános célú és nyolc vagy több maggal rendelkező számítási optimalizált példány mérete. További információkért lásd: hello [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) és [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuális gép méretének cikkeket. hello támogatott Virtuálisgép-példány méretek készlete bont ki a jövőbeli hello.
* **Telepítés csak Azure Resource Managerrel (ARM) keresztül:** az elérését gyorsítja fel hálózati nincs elérhető a telepítéshez a címterület-kezelés/RDFE keresztül.

Módosítások toothese korlátozások hello keresztül történik bejelentés [Azure virtuális hálózat frissítése](https://azure.microsoft.com/updates/accelerated-networking-in-preview) lap.

## <a name="create-a-windows-vm"></a>Windows rendszerű virtuális gép létrehozása
Hello Azure-portálon vagy az Azure [PowerShell](#windows-powershell) toocreate hello virtuális gép.

### <a name="windows-portal"></a>Portál

1. Egy webböngészőben nyissa meg a hello Azure [portal](https://portal.azure.com) és jelentkezzen be a Azure [fiók](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Ha már nincs fiókja, regisztrálhat az egy [ingyenes próbaverzió](https://azure.microsoft.com/offers/ms-azr-0044p).
2. Hello portálon kattintson **+ új** > **számítási** > **Windows Server 2016 Datacenter**. 
3. A hello **Windows Server 2016 Datacenter** panel, amely akkor jelenik meg, hagyja *erőforrás-kezelő* a kijelölt **telepítési modell kiválasztása**, és kattintson a **létrehozása** .
4. A hello **alapjai** panel, amelyen megjelenik, írja be a következő értékek hello, hagyja az alapértelmezett beállítások fennmaradó hello vagy válassza ki vagy adja meg a saját értékeit, és kattintson hello **OK** gombra:

    |Beállítás|Érték|
    |---|---|
    |Név|MyVm|
    |Erőforráscsoport|Hagyja **hozzon létre új** kiválasztva, és írja be *MyResourceGroup*|
    |Hely|USA nyugati régiója, 2.|

    Ha új tooAzure, további információ [erőforráscsoportok](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [előfizetések](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), és [helyek](https://azure.microsoft.com/regions) (amely is hivatkozunk tooas régiók).
5. A hello **méret kiválasztása** panel, amelyen megjelenik, írja be *8* a hello **minimális magszámra** mezőbe, majd kattintson az **összes**.
6. Kattintson a **DS4_V2 szabványos**, vagy bármely támogatott virtuális gép, majd kattintson a hello **válasszon** gombra.
7. Hello a **beállítások** panelen megjelenő összes beállításokat hagyja-van, kivéve, kattintson **engedélyezve** alatt **elérését gyorsítja fel a hálózat**, majd kattintson a hello **OK** gombra. **Megjegyzés:** , ha előző lépésben kiválasztott hello nem szereplő értékek a virtuális gép mérete, az operációs rendszer vagy a hely [korlátozások](#Limitations) című szakaszát, **gyorsított hálózatkezelés**nem látható.
8. A hello **összegzés** panel, amelyen megjelenik, kattintson a hello **OK** gombra. Hello virtuális gép létrehozása Azure elindul. Virtuális gép létrehozásának folyamata eltart néhány percig.
9. tooinstall hello a Windows hálózati illesztőprogram gyorsított, teljes hello szükséges lépések hello [konfigurálása Windows](#configure-windows) című szakaszát.

### <a name="windows-powershell"></a>PowerShell
1. Hello hello Azure PowerShell legújabb verziójának telepítéséhez [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modul. Ha új tooAzure PowerShell, olvassa el a hello [Azure PowerShell áttekintése](/powershell/azure/get-started-azureps?toc=%2fazure%2fvirtual-network%2ftoc.json) cikk.
2. Indítsa el a Windows hello gombra kattintva indítsa el a egy PowerShell-munkamenetben írja be **powershell**, majd kattintson a **PowerShell** hello a keresési eredmények.
3. A PowerShell-ablakban írja be a hello `login-azurermaccount` parancs toosign be a Azure [fiók](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Ha már nincs fiókja, regisztrálhat az egy [ingyenes próbaverzió](https://azure.microsoft.com/offers/ms-azr-0044p).
4. A böngészőben másolja a következő parancsfájl hello:
    ```powershell
    $RgName="MyResourceGroup"
    $Location="westus2"
    
    # Create a resource group
    New-AzureRmResourceGroup `
      -Name $RgName `
      -Location $Location
    
    # Create a subnet
    $Subnet = New-AzureRmVirtualNetworkSubnetConfig `
      -Name MySubnet `
      -AddressPrefix 10.0.0.0/24
    
    # Create a virtual network
    $Vnet=New-AzureRmVirtualNetwork `
      -ResourceGroupName $RgName `
      -Location $Location `
      -Name MyVnet `
      -AddressPrefix 10.0.0.0/16 `
      -Subnet $Subnet

    # Create a public IP address
    $Pip = New-AzureRmPublicIpAddress `
      -Name MyPublicIp `
      -ResourceGroupName $RgName `
      -Location $Location `
      -AllocationMethod Static

    # Create a virtual network interface and associate hello public IP address tooit
    $Nic = New-AzureRmNetworkInterface `
      -Name MyNic `
      -ResourceGroupName $RgName `
      -Location $Location `
      -SubnetId $Vnet.Subnets[0].Id `
      -PublicIpAddressId $Pip.Id `
      -EnableAcceleratedNetworking
     
    # Define a credential object for hello VM. PowerShell prompts you for a username and password.
    $Cred = Get-Credential

    # Create a virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
      -VMName MyVM -VMSize Standard_DS4_v2 | `
      Set-AzureRmVMOperatingSystem `
      -Windows `
      -ComputerName myVM `
      -Credential $Cred | `
    Set-AzureRmVMSourceImage `
      -PublisherName MicrosoftWindowsServer `
      -Offer WindowsServer `
      -Skus 2016-Datacenter `
      -Version latest | `
    Add-AzureRmVMNetworkInterface -Id $Nic.Id 

    # Create hello virtual machine.    
    New-AzureRmVM `
      -ResourceGroupName $RgName `
      -Location $Location `
      -VM $VmConfig
    #
    ```
5. A PowerShell-ablakban kattintson a jobb gombbal a toopaste hello parancsfájlt, és futtatnia kell elindítani. Felhasználónév és jelszó megadását kéri. Használja ezeket a hitelesítő adatok toolog toohello VM, hello következő lépésben tooit kapcsolódáskor. Ha hello parancsfájl futása sikertelen, és azt végrehajtása előtt módosította hello parancsfájlban értékét, ellenőrizze a virtuális gép méretét, operációs rendszer használt hello értékeket, és helyet hello szereplő [korlátozások](#Limitations) című szakaszát.
6. tooinstall hello a Windows hálózati illesztőprogram gyorsított, teljes hello szükséges lépések hello [konfigurálása Windows](#configure-windows) című szakaszát.

### <a name="configure-windows"></a>A Windows beállítása
Miután létrehozta a virtuális gép hello az Azure-ban, telepítenie kell a Windows hello gyorsított hálózati illesztőprogram. Hello lépések elvégzése előtt kell korábban létrehozott egy Windows virtuális gép vagy hello segítségével gyorsított hálózatkezelés [portal](#windows-portal) vagy [PowerShell](#windows-powershell) ebben a cikkben ismertetett visszaállítási lépésekkel. 

1. Egy webböngészőben nyissa meg a hello Azure [portal](https://portal.azure.com) és jelentkezzen be a Azure [fiók](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Ha már nincs fiókja, regisztrálhat az egy [ingyenes próbaverzió](https://azure.microsoft.com/offers/ms-azr-0044p).
2. Hello mezőben hello szöveget tartalmazó *keresési erőforrások* tetején hello hello Azure-portálon, írja be a *MyVm*. Ha **MyVm** jelenik meg a hello találatok, kattintson rá.
3. A hello **MyVm** panel, amelyen megjelenik, kattintson a hello **Connect** hello bal felső sarkában hello panel gombjára. **Megjegyzés:** Ha **létrehozása** hello alatt látható **Connect** gomb, Azure még nem fejeződött be hello virtuális gép létrehozása. Kattintson a **Connect** csak után többé nem láthatja **létrehozása** alatt hello **Connect** gombra.
4. A böngésző toodownload hello engedélyezése **MyVm.rdp** fájlt.  Ha le, kattintson a hello fájl tooopen azt. 
5. Kattintson a hello **Connect** hello gombjára **távoli asztali kapcsolat** panelen, amely tájékoztat arról, hogy hello hello távoli kapcsolat közzétevője nem azonosítható.
6. A hello **Windows biztonsági** meg, kattintson a **több lehetőséget**, majd kattintson a **használjon más fiókot**. Írja be a hello felhasználónév és a 4. lépésben megadott jelszót, majd kattintson a hello **OK** gombra.
7. Kattintson a hello **Igen** gomb hello távoli asztali kapcsolat-be, amely tájékoztat arról, hogy hello hello távoli számítógép nem azonosítható.
8. Kattintson a jobb gombbal a hello Windows Start gombra, és kattintson a **Eszközkezelő**. Bontsa ki a hello **hálózati adapterek** csomópont. Győződjön meg arról, hogy hello **Mellanox ConnectX-3 virtuális függvény Ethernet-Adapter** jelenik meg, ahogy az alábbi képen hello:
   
    ![Eszközkezelő](./media/virtual-network-create-vm-accelerated-networking/image2.png)

9. Gyorsított hálózatkezelés engedélyezve van a virtuális gép számára.

## <a name="create-a-linux-vm"></a>Linux rendszerű virtuális gép létrehozása
Hello Azure-portálon vagy az Azure [PowerShell](#linux-powershell) toocreate egy Ubuntu vagy SLES virtuális gép. Az RHEL és CentOS virtuális gépeket egy másik munkafolyamat van.  Tekintse meg az alábbi hello utasításokat.

### <a name="linux-portal"></a>Portál
1. Hello gyorsított közötti hálózati Linux Preview 1-5. lépéseket a hello befejezése a register [hozzon létre egy Linux virtuális Gépet - PowerShell](#linux-powershell) című szakaszát.  Hello Preview hello portálon nem regisztrálható.
2. Teljes lépések 1-8 a hello [hozzon létre egy Windows virtuális Gépet - portál](#windows-portal) című szakaszát. A 2. lépésben, kattintson a **Ubuntu Server 16.04 LTS** helyett **Windows Server 2016 Datacenter**. Ebben az oktatóanyagban válassza a toouse jelszót, nem pedig egy SSH-kulccsal, abban az esetben, ha az üzemi környezetek is használhatja. Ha **hálózat az elérését gyorsítja fel** nem jelenik meg, amikor befejezte a hello 7. lépés [hozzon létre egy Windows virtuális Gépet - portál](#windows-portal) szakasz ezen cikk valószínű hello a következő okok valamelyike miatt:
    - Még nem regisztrált hello Preview. Győződjön meg arról, hogy a regisztrációs állapotát **regisztrált**, hello 4. lépésben leírtak szerint [hozzon létre egy Linux virtuális Gépet - Powershell](#linux-powershell) című szakaszát. **Megjegyzés:** Ha Ön részt vett a hello gyorsított hálózatkezelés a Windows virtuális gépek preview (fennállt nem hosszabb szükséges tooregister toouse az elérését gyorsítja fel Windows virtuális gépek hálózati), akkor nem automatikusan regisztrálva vannak hello gyorsított a hálózati Linux virtuális gépek megtekintése. Regisztrálnia kell a hello gyorsított hálózati a Linux virtuális gépek előzetes tooparticipate azt.
    - Nincs kiválasztva a virtuális gép méretét, a operációs rendszer vagy a hely hello felsorolt [korlátozások](#limitations) című szakaszát.
3. tooinstall hello az elérését gyorsítja fel hálózati illesztőprogram Linux, teljes hello szükséges lépések hello [konfigurálása Linux](#configure-linux) című szakaszát.

### <a name="linux-powershell"></a>PowerShell

>[!WARNING]
>Ha a Linux virtuális gépek létrehozása egy előfizetésben gyorsított hálózattal, majd próbálja meg toocreate hello gyorsított hálózatkezelés a Windows virtuális gép ugyanahhoz az előfizetéshez, hello Windows virtuális gép létrehozása sikertelen lehet. Ez az előzetes ajánlott, hogy tesztelje a Linux és a Windows virtuális gépek külön előfizetésekhez gyorsított hálózattal.
>

1. Hello hello Azure PowerShell legújabb verziójának telepítéséhez [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modul. Ha új tooAzure PowerShell, olvassa el a hello [Azure PowerShell áttekintése](/powershell/azure/get-started-azureps?toc=%2fazure%2fvirtual-network%2ftoc.json) cikk.
2. Indítsa el a Windows hello gombra kattintva indítsa el a egy PowerShell-munkamenetben írja be **powershell**, majd kattintson a **PowerShell** hello a keresési eredmények.
3. A PowerShell-ablakban írja be a hello `login-azurermaccount` parancs toosign be a Azure [fiók](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Ha már nincs fiókja, regisztrálhat az egy [ingyenes próbaverzió](https://azure.microsoft.com/offers/ms-azr-0044p).
4. Regisztrálás az Azure-hálózat elérését gyorsítja fel hello preview hello lépések végrehajtásával:
    - Az e-mailek küldése túl[ axnpreview@microsoft.com ](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) az Azure-előfizetése Azonosítóját és a tervezett használattól. Várjon, amíg egy e-mailes megerősítés tudnivalók az előfizetés engedélyezése a Microsoft.
    - Adja meg a következő parancs tooconfirm hello Preview regisztrálta hello:
    
        ```powershell
        Get-AzureRmProviderFeature -FeatureName AllowAcceleratedNetworkingForLinux -ProviderNamespace Microsoft.Network
        ```

        Ne folytassa az 5. lépés-ig **regisztrált** hello kimeneti hello előző parancs megadása után jelenik meg. A kimeneti hello hasonló kimenetet a folytatás előtt kell kinéznie:
    
        ```powershell
        FeatureName                        ProviderName      RegistrationState
        -----------                        ------------      -----------------
        AllowAcceleratedNetworkingForLinux Microsoft.Network Registered
        ```
        
      >[!NOTE]
      >Ha Ön részt vett a hello gyorsított hálózatkezelés a Windows virtuális gépek preview (fennállt nem hosszabb szükséges tooregister toouse az elérését gyorsítja fel Windows virtuális gépek hálózati), akkor nem automatikusan regisztrálva vannak hello gyorsított Linux virtuális gépek Preview hálózat. Regisztrálnia kell a hello gyorsított hálózati a Linux virtuális gépek előzetes tooparticipate azt.
      >
5. A böngészőben a következő parancsfájl és Ubuntu vagy SLES tetszés szerint hello másolja.  Ebben az esetben Redhat és CentOS van egy másik munkafolyamat, az alábbiakban leírt:

    ```powershell
    $RgName="MyResourceGroup"
    $Location="westus2"
    
    # Create a resource group
    New-AzureRmResourceGroup `
      -Name $RgName `
      -Location $Location
    
    # Create a subnet
    $Subnet = New-AzureRmVirtualNetworkSubnetConfig `
      -Name MySubnet `
      -AddressPrefix 10.0.0.0/24
    
    # Create a virtual network
    $Vnet=New-AzureRmVirtualNetwork `
      -ResourceGroupName $RgName `
      -Location $Location `
      -Name MyVnet `
      -AddressPrefix 10.0.0.0/16 `
      -Subnet $Subnet

    # Create a public IP address
    $Pip = New-AzureRmPublicIpAddress `
      -Name MyPublicIp `
      -ResourceGroupName $RgName `
      -Location $Location `
      -AllocationMethod Static

    # Create a virtual network interface and associate hello public IP address tooit
    $Nic = New-AzureRmNetworkInterface `
      -Name MyNic `
      -ResourceGroupName $RgName `
      -Location $Location `
      -SubnetId $Vnet.Subnets[0].Id `
      -PublicIpAddressId $Pip.Id `
      -EnableAcceleratedNetworking
     
    # Create a new Storage account and define hello new VM’s OSDisk name and its URI
    # Must end with ".vhd" extension
    $OSDiskName = "MyOsDiskName.vhd"
    # Storage account name must be between 3 and 24 characters in length and use numbers and lower-case letters only.
    $OSDiskSAName = "thestorageaccountname"  
    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $RgName -Name $OSDiskSAName -Type "Standard_GRS" -Location $Location
    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName
 
    # Define a credential object for hello VM. PowerShell prompts you for a username and password.
    $Cred = Get-Credential

    # Create a virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
      -VMName MyVM -VMSize Standard_DS4_v2 | `
      Set-AzureRmVMOperatingSystem `
      -Linux `
      -ComputerName myVM `
      -Credential $Cred | `
    Set-AzureRmVMSourceImage `
      -PublisherName Canonical `
      -Offer UbuntuServer `
      -Skus 16.04-LTS `
      -Version latest | `
    Add-AzureRmVMNetworkInterface -Id $Nic.Id | `
    Set-AzureRmVMOSDisk -Name $OSDiskName `
      -VhdUri $OSDiskUri `
      -CreateOption FromImage 

    # Create hello virtual machine.    
    New-AzureRmVM `
      -ResourceGroupName $RgName `
      -Location $Location `
      -VM $VmConfig
    ```
    
6. A PowerShell-ablakban kattintson a jobb gombbal a toopaste hello parancsfájlt, és futtatnia kell elindítani. Felhasználónév és jelszó megadását kéri. Használja ezeket a hitelesítő adatok toolog toohello VM, hello következő lépésben tooit kapcsolódáskor. Ha hello parancsfájl futása sikertelen, ellenőrizze, hogy:
    - Regisztrált hello Preview, a 4. lépésben leírtak szerint
    - Ha módosította virtuális gép méretét, operációsrendszer-típusra vagy hello parancsfájlban értékei előtt futtatnia kell, hogy hello értékek szerepelnek hello [korlátozások](#Limitations) című szakaszát.
7. tooinstall hello az elérését gyorsítja fel hálózati illesztőprogram Linux, teljes hello szükséges lépések hello [konfigurálása Linux](#configure-linux) című szakaszát.

### <a name="configure-linux"></a>Linux konfigurálása

Miután létrehozta a virtuális gép hello az Azure-ban, Linux hello gyorsított hálózati illesztőprogram kell telepítenie. Hello lépések elvégzése előtt kell korábban létrehozott egy Linux virtuális gép vagy hello segítségével gyorsított hálózatkezelés [portal](#linux-portal) vagy [PowerShell](#linux-powershell) ebben a cikkben ismertetett visszaállítási lépésekkel. 

1. Egy webböngészőben nyissa meg a hello Azure [portal](https://portal.azure.com) és jelentkezzen be a Azure [fiók](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Ha már nincs fiókja, regisztrálhat az egy [ingyenes próbaverzió](https://azure.microsoft.com/offers/ms-azr-0044p).
2. Hello portálon toohello jobb oldalán hello hello tetején *keresési erőforrások* kattintson a hello **> _** ikon toostart a felhő rendszerhéjakba (előzetes verzió). hello Bash felhő rendszerhéj ablaktáblán megjelenik hello alsó hello portál, és néhány másodpercen belül megadja egy  **username@Azure:~ $** kérdés. Abban az esetben, ha a számítógép, hanem hello felhő rendszerhéj a virtuális gép SSH toohello is, ebben az oktatóanyagban hello utasítások feltételezik, hogy hello felhő rendszerhéj használata.
3. Hello portál hello tetején hello mezőben tartalmazó hello szöveg *keresési erőforrások*, típus *MyVm*. Ha **MyVm** jelenik meg a hello találatok, kattintson rá.
4. A hello **MyVm** panel, amelyen megjelenik, kattintson a hello **Connect** hello bal felső sarkában hello panel gombjára. **Megjegyzés:** Ha **létrehozása** hello alatt látható **Connect** gomb, Azure még nem fejeződött be hello virtuális gép létrehozása. Kattintson a **Connect** csak után többé nem láthatja **létrehozása** alatt hello **Connect** gombra.
5. Azure megnyitása felszólító tooenter hello `ssh adminuser@<ipaddress>`. Adja meg a parancs hello felhő rendszerhéj (vagy másolási megjelent hello listából, 4. lépés:, és illessze be azt a toohello felhő rendszerhéj), majd az ENTER billentyűt.
6. Adjon meg **Igen** kérni, ha azt szeretné, hogy a toocontinue csatlakozni, majd nyomja le az Enter toohello kérdést.
7. Adja meg a hello virtuális gép létrehozásakor megadott hello jelszó. Egyszer a bejelentkezés sikerült toohello VM, megjelenik egy adminuser@MyVm:~ $ kérdés. Most jelentkezett toohello VM hello felhő rendszerhéj kapcsolaton keresztül. **Megjegyzés:** felhő rendszerhéj munkamenetek időkorlátja lejárt 10 perc inaktivitás után.

Ezen a ponton hello utasításokat hello telepítési módjától függően változhat. 

#### <a name="ubuntusles"></a>Ubuntu/SLES

1. Hello kérdés esetén `uname -r` , majd erősítse meg a hello verzió:

    * Ubuntu "4.4.0-77-generic," vagy nagyobb
    * SLES "4.4.59-92.20-default" vagy nagyobb

2. Hozzon létre egy szabványos hálózati vNIC hello és gyorsított hello hálózati vNIC közötti kötés futó hello parancsoknál, amelyek. Hálózati forgalom hello magasabb gyorsított hálózati vNIC, hajt végre, amíg hello kötés biztosítja, hogy hálózati adatforgalom nem szakad meg bizonyos konfigurációs változások során használja.
          
     ```bash
     wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/configure_hv_sriov.sh
     chmod +x ./configure_hv_sriov.sh
     sudo ./configure_hv_sriov.sh
     ```
3. Hello a parancsfájl futtatását, miután a hello VM 60 másodperc felfüggesztése után újraindul.
4. Virtuális gép újraindul, hello egyszer tooit újracsatlakozás újra 5-7 lépések elvégzésével.
5. Futtassa a hello `ifconfig` parancsot, és győződjön meg róla, hogy bond0 merült fel, és a rendszer hello felület azt jelzi, fel. 
 
 >[!NOTE]
      >Gyorsított hálózatkezelés használó alkalmazások kell kommunikálhassanak hello *bond0* nem felület *eth0*.  hello kapcsolat neve előtt gyorsított hálózatkezelés eléri az általánosan rendelkezésre álló módosíthatja.

#### <a name="rhelcentos"></a>RHEL vagy CentOS

A Red Hat Enterprise Linux vagy a CentOS 7.3 VM létrehozásához meg néhány további lépések tooload hello legújabb illesztőprogramokkal SR-iov-t és a hello virtuális funkciójú (VF) illesztőprogram hello hálózati kártya számára szükséges. hello hello utasításokat első fázisa előkészít egy olyanra, amely lehet használt toomake egy vagy több virtuális gépeken, amelyek előre betöltött hello illesztőprogramok.

##### <a name="phase-one-prepare-a-red-hat-enterprise-linux-or-centos-73-base-image"></a>1. fázis: készítse elő a Red Hat Enterprise Linux vagy a CentOS 7.3 alapjául szolgáló lemezképhez. 

1.  Egy nem - PORTPROFIL CentOS 7.3 virtuális Gépet az Azure telepítéséhez

2.  Telepíteni azokat 4.2.2:
    
    ```bash
    wget http://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.2-2.tar.gz
    tar -xvf lis-rpms-4.2.2-2.tar.gz
    cd LISISO && sudo ./install.sh
    ```

3.  Konfigurációs fájljainak letöltése
    
    ```bash
    cd /etc/udev/rules.d/  
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/60-hyperv-vf-name.rules 
    cd /usr/sbin/
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/hv_vf_name 
    sudo chmod +x hv_vf_name
    cd /etc/sysconfig/network-scripts/
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/ifcfg-vf1   
    ```

4.  Ez a virtuális gép kiosztásának megszüntetése

    ```bash
    sudo waagent -deprovision+user 
    ```

5.  Azure-portálon állítsa le a virtuális gép; Nyissa meg a tooVM "lemez", hello OSDisk URI Azonosítójú virtuális merevlemez rögzítése és. Ezt az URI hello alapjául szolgáló lemezképhez VHD-t és a tárfiók tartalmazza. 
 
##### <a name="phase-two-provision-new-vms-on-azure"></a>2. fázis: Azure-alapú virtuális gépek új kiépítése

1.  A New-AzureRMVMConfig alapuló új virtuális gépek kiépítése alapjául szolgáló lemezképhez AcceleratedNetworking hello virtuális engedélyezve van az első lépése, rögzített virtuális merevlemez használatával hello:

    ```powershell
    $RgName="MyResourceGroup"
    $Location="westus2"
    
    # Create a resource group
    New-AzureRmResourceGroup `
     -Name $RgName `
     -Location $Location

    # Create a subnet
    $Subnet = New-AzureRmVirtualNetworkSubnetConfig `
     -Name MySubnet `
     -AddressPrefix 10.0.0.0/24

    # Create a virtual network
    $Vnet=New-AzureRmVirtualNetwork `
     -ResourceGroupName $RgName `
     -Location $Location `
     -Name MyVnet `
     -AddressPrefix 10.0.0.0/16 `
     -Subnet $Subnet
    
    # Create a public IP address
    $Pip = New-AzureRmPublicIpAddress `
     -Name MyPublicIp `
     -ResourceGroupName $RgName `
     -Location $Location `
     -AllocationMethod Static
    
    # Create a virtual network interface and associate hello public IP address tooit
    $Nic = New-AzureRmNetworkInterface `
     -Name MyNic `
     -ResourceGroupName $RgName `
     -Location $Location `
     -SubnetId $Vnet.Subnets[0].Id `
     -PublicIpAddressId $Pip.Id `
     -EnableAcceleratedNetworking
    
    # Specify hello base image's VHD URI (from phase one step 5). 
    # Note: hello storage account of this base image vhd should have "Storage service encryption" disabled
    # See more from here: https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption
    # This is just an example URI, you will need tooreplace this when running this script
    $sourceUri="https://myexamplesa.blob.core.windows.net/vhds/CentOS73-Base-Test120170629111341.vhd" 

    # Specify a URI for hello location from which hello new image binary large object (BLOB) is copied toostart hello virtual machine. 
    # Must end with ".vhd" extension
    $OsDiskName = "MyOsDiskName.vhd" 
    $destOsDiskUri = "https://myexamplesa.blob.core.windows.net/vhds/" + $OsDiskName
    
    # Define a credential object for hello VM. PowerShell prompts you for a username and password.
    $Cred = Get-Credential
    
    # Create a custom virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
     -VMName MyVM -VMSize Standard_DS4_v2 | `
    Set-AzureRmVMOperatingSystem `
     -Linux `
     -ComputerName myVM `
     -Credential $Cred | `
    Add-AzureRmVMNetworkInterface -Id $Nic.Id | `
    Set-AzureRmVMOSDisk `
     -Name $OsDiskName `
     -SourceImageUri $sourceUri `
     -VhdUri $destOsDiskUri `
     -CreateOption FromImage `
     -Linux
    
    # Create hello virtual machine.    
    New-AzureRmVM `
     -ResourceGroupName $RgName `
     -Location $Location `
     -VM $VmConfig
    ```

2.  Után indítsa el a virtuális gépek, "lspci" hello VF eszköz ellenőrzi, és ellenőrizze hello Mellanox bejegyzést. Például azt kell látnia hello lspci kimeneti ezt az elemet:
    
    ```
    0001:00:02.0 Ethernet controller: Mellanox Technologies MT27500/MT27520 Family [ConnectX-3/ConnectX-3 Pro Virtual Function]
    ```
    
3.  Hello ragasztás parancsfájl által futtatása:

    ```bash
    sudo bondvf.sh
    ```

4.  Indítsa újra új virtuális gépek hello:

    ```bash
    sudo reboot
    ```

virtuális gép hello bond0 konfigurálva és hello az elérését gyorsítja fel hálózati elérési út engedélyezve kell kezdődnie.  Futtatás `ifconfig` tooconfirm.
