---
title: "SQL Server virtuális gépen az Azure PowerShell (erőforrás-kezelő) aaaCreate |} Microsoft Docs"
description: "Lépéseket és a PowerShell-parancsfájlok biztosít az Azure virtuális gép létrehozása az SQL Server virtuális gép a gyűjtemény lemezképei."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: 98d50dd8-48ad-444f-9031-5378d8270d7b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/17/2017
ms.author: jroth
ms.openlocfilehash: 2b8cb8f69ff9894a95eab617816a60c8674eeefa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-resource-manager"></a>Egy SQL Server rendszerű virtuális gép az Azure PowerShell (Resource Manager)
> [!div class="op_single_selector"]
> * [Portál](virtual-machines-windows-portal-sql-server-provision.md)
> * [PowerShell](virtual-machines-windows-ps-sql-create.md)
>
>

## <a name="overview"></a>Áttekintés
Az oktatóanyag bemutatja, hogyan using Azure virtuális gép egyetlen toocreate hello **Azure Resource Manager** telepítési modell Azure PowerShell-parancsmagok használatával. Ebben az oktatóanyagban létre fogunk hozni egy egyetlen meghajtó lemezkép használatát hello SQL gyűjtemény egyetlen virtuális gépre. Hello tárolási, hálózati és számítási erőforrásokat hello virtuális gép által használt új szolgáltatók létre fogunk hozni. Ha meglévő szolgáltatók bármely ezeket az erőforrásokat, helyette használhatja azokat a szolgáltatókat.

Ha ez a témakör a klasszikus verzióra kell hello, lásd: [egy SQL Server rendszerű virtuális gép használata a klasszikus Azure-PowerShell](../classic/ps-sql-create.md).

## <a name="prerequisites"></a>Előfeltételek
Ebben az oktatóanyagban lesz szüksége:

* Egy Azure-fiókot és az előfizetés megkezdése előtt. Ha még nincs fiókja, regisztráljon egy [ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/).
* [Az Azure PowerShell)](/powershell/azure/overview), minimális verziója 1.4.0 vagy újabb (Ez az oktatóanyag verziójával 1.5.0 írása).
  * tooretrieve a verziója, a típus **Azure Get-Module - ListAvailable**.

## <a name="configure-your-subscription"></a>Az előfizetés konfigurálása
Nyissa meg a Windows Powershellt, és hozzáférést tooyour Azure-fiók létrehozásához hello a következő parancsmag futtatásával. Választhat egy bejelentkezési képernyő tooenter a hitelesítő adatait. Használja az azonos hello e-mailek és a jelszó toosign használhatja a toohello Azure-portálon.

    Add-AzureRmAccount

Sikeres bejelentkezés után néhány információt a képernyőn hello; előfizetés-azonosító, amelyhez a bejelentkezett megjelenik. Ez a hello előfizetés, amely ebben az oktatóanyagban hello erőforrások létrehozza kivéve tooa különböző előfizetés módosítása. Ha több SubscriptionIds, futtassa a következő parancsmag tooreturn hello összes a SubscriptionIds listáját:

    Get-AzureRmSubscription

Futtassa a következő parancsmagot a kívánt előfizetés-azonosítójú hello toochange tooanother, előfizetés-azonosító.

    Select-AzureRmSubscription -SubscriptionId xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

## <a name="define-image-variables"></a>Adja meg a lemezkép változókat
toosimplify használhatósága és ismeretekkel rendelkezik az oktatóanyag befejezése hello parancsfájlt, először lesz számos változó meghatározásával. Hello paraméterértékek igényei, de elnevezési korlátozások kapcsolódó tooname hosszak és speciális karakterek ügyeljen arra, ha módosítja a megadott hello értékek módosítása

### <a name="location-and-resource-group"></a>Hely és az erőforráscsoport
Két változók toodefine hello adatok régió és hello erőforráscsoport amelybe létrehozhat hello egyéb erőforrások hello virtuális gép használja.

Módosítsa a kívánt módon működjenek, és majd hajtható végre a következő parancsmagok tooinitialize hello változókhoz.

    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"

### <a name="storage-properties"></a>Tárolási tulajdonságai
A következő változók toodefine hello tárolási fiók és hello típusú hello virtuális gép által használt tárolási toobe hello használata.

Módosítsa a kívánt módon működjenek, és majd hajtható végre a következő parancsmag tooinitialize hello változókhoz. Vegye figyelembe, hogy a jelen példában használjuk [prémium szintű Storage](../../../storage/common/storage-premium-storage.md), amely a termelési számítási feladatokhoz ajánlott. Ez az útmutató és más javaslatokról a részletekért lásd: [teljesítmény ajánlott eljárások az SQL Server Azure virtuális gépek](virtual-machines-windows-sql-performance.md).

    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

### <a name="network-properties"></a>Hálózat tulajdonságai
A következő változók toodefine hello hálózati illesztő, hello TCP/IP-kiosztási módszerrel, hello virtuálishálózat-név, hello virtuális alhálózat neve, az IP-címek hello hello virtuális hálózat, hello az IP-címek hello alhálózati és hello hello használata nyilvános tartomány neve címke toobe hello hálózat hello virtuális gép által használt.

Módosítsa a kívánt módon működjenek, és majd hajtható végre a következő parancsmag tooinitialize hello változókhoz.

    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $TCPIPAllocationMethod = "Dynamic"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $DomainName = "sqlvm1"

### <a name="virtual-machine-properties"></a>Virtuális gép tulajdonságai
A következő változók toodefine hello virtuálisgép-nevet, hello számítógépnév, hello virtuálisgép-méret és operációs rendszer Lemeznév hello hello virtuális gép hello használata.

Módosítsa a kívánt módon működjenek, és majd hajtható végre a következő parancsmag tooinitialize hello változókhoz.

    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

### <a name="image-properties"></a>Lemezkép tulajdonságai
A következő változók toodefine hello kép toouse hello virtuális gép hello használata. Ebben a példában hello SQL Server 2016 Enterprise lemezképet használja.

Módosítsa a kívánt módon működjenek, és majd hajtható végre a következő parancsmag tooinitialize hello változókhoz.

    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

Vegye figyelembe, hogy kaphat a teljes listáját az SQL Server kép ajánlatokat a Get-AzureRmVMImageOffer hello paranccsal:

    Get-AzureRmVMImageOffer -Location 'East US' -Publisher 'MicrosoftSQLServer'

És egy ajánlatot a Get-AzureRmVMImageSku hello paranccsal elérhető termékváltozatok hello látható. hello alábbi a parancs megjeleníti a termékváltozatokat hello elérhető **SQL2014SP1-WS2012R2** kínálnak.

    Get-AzureRmVMImageSku -Location 'East US' -Publisher 'MicrosoftSQLServer' -Offer 'SQL2014SP1-WS2012R2' | Select Skus

## <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot
Hello Resource Manager üzembe helyezési modelljével hello első-objektumnak hello erőforráscsoportot. Hello használjuk [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) parancsmag toocreate egy Azure-erőforráscsoportot és az erőforrások hello erőforrás csoport nevét és helyét, amelyek korábban inicializálták hello változók határozzák meg.

Az új erőforráscsoport hajtható végre a következő parancsmag toocreate hello.

    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

## <a name="create-a-storage-account"></a>Create a storage account
hello virtuális gép igényel a tárolási erőforrások hello operációsrendszer-lemez és az SQL Server-adatok hello és naplófájlok. Az egyszerűség kedvéért létre fogunk hozni egy lemezt is. Később a hello segítségével további lemezek csatolhat [Add-Azure lemez](/powershell/module/azure/add-azuredisk) parancsmag tooplace rendezés az SQL Server-adatok és naplófájlok dedikált lemezeken. Hello használjuk [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) parancsmag toocreate egy standard tárolási fiókra az új erőforráscsoport és hello tárfiók neve, a tárolási Sku név és a hely definiált hello változók használata korábban inicializálták.

Hajtsa végre a következő parancsmag toocreate hello új tárfiókját.

    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

## <a name="create-network-resources"></a>Hálózati erőforrások létrehozása
hello virtuális gép hálózati erőforrások több hálózati kapcsolatot igényel.

* Minden virtuális gép virtuális hálózat szükséges.
* A virtuális hálózati rendelkeznie kell legalább egy meghatározott alhálózatot.
* Egy adott hálózati csatoló nyilvános vagy magán IP-címet kell megadni.

### <a name="create-a-virtual-network-subnet-configuration"></a>Hozzon létre egy virtuális hálózati alhálózat beállítása
Először hozzon létre a virtuális hálózati alhálózat konfigurációját fogja. A jelen oktatóanyagban létrehozunk egy alapértelmezett alhálózatot hello [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) parancsmag. Létre fogunk hozni a virtuális hálózati alhálózat beállítása hello alhálózat nevét és címét előtaggal rendelkező megadott hello változókat, amelyek korábban inicializálták.

> [!NOTE]
> Hello virtuális hálózati alhálózat konfiguráció Ez a parancsmag segítségével további tulajdonságok adhatók, de ez az oktatóanyag hello terjed.
>
>

A következő parancsmag toocreate hello hajtható végre a virtuális alhálózati konfigurációt.

    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix

### <a name="create-a-virtual-network"></a>Virtuális hálózat létrehozása
Ezután létrehozunk hello segítségével a virtuális hálózati [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) parancsmag. Létre fogunk hozni a virtuális hálózat az új erőforráscsoportban, hello nevét, helyét és címelőtag definiálva, amely korábban inicializálták hello változók használata, és hello előző lépésben megadott hello alhálózati konfigurációt használ.

A következő parancsmag toocreate hello hajtható végre a virtuális hálózat.

    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig

### <a name="create-hello-public-ip-address"></a>Hello nyilvános IP-cím létrehozása
Most, hogy a megadott virtuális hálózat, tooconfigure IP-címet kell kapcsolatot toohello virtuális géphez. Ebben az oktatóanyagban létre fogunk hozni egy nyilvános IP-címet, dinamikus IP-címzés toosupport internetkapcsolat használatával. Hello használjuk [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) parancsmag toocreate hello nyilvános IP-cím hello erőforrás létrehozott csoport prevously és hello nevét, helyét, elosztási módszert és DNS tartománynév-címke definiálva hello használata az változókat, amelyek korábban inicializálták.

> [!NOTE]
> Hello azt az nyilvános IP-cím, ez a parancsmag segítségével további tulajdonságok adhatók, de ez az első oktatóanyag hello terjed. Is létrehozhatja saját cím vagy a cím statikus címmel rendelkező, de, amely még nem ez az oktatóanyag hello terjed.
>
>

Hajtsa végre a következő parancsmag toocreate hello a nyilvános IP-cím.

    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName

### <a name="create-hello-network-interface"></a>Hello hálózati illesztő létrehozása
Dolgozunk most már készen áll a toocreate hello hálózati illesztő a virtuális gép által használt. Hello használjuk [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) parancsmag toocreate a hálózati illesztő a korábban létrehozott erőforráscsoportot hello és hello nevű helyen, alhálózat és nyilvános IP-címet korábban már definiálva.

Hajtsa végre a következő parancsmag toocreate hello a hálózati illesztő.

    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

## <a name="configure-a-vm-object"></a>A Virtuálisgép-objektum konfigurálása
Most, hogy meghatározott tárolási és hálózati erőforrásokat, vagy folyamatban hello virtuális gép készen áll a toodefine számítási erőforrásokat. A jelen oktatóanyagban azt fogja hello virtuálisgép-méret és a különböző operációs rendszer tulajdonságait adja meg, adja meg azt a korábban létrehozott hello hálózati illesztő, blob-tároló megadása, és adja hello operációsrendszer-lemez.

### <a name="create-hello-vm-object"></a>Hello Virtuálisgép-objektum létrehozása
A Microsoft hello virtuálisgép-méret megadásával indul el. Ebben az oktatóanyagban egy DS13 meg azt. Hello használjuk [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) parancsmag toocreate egy konfigurálható virtuális gép objektum hello neve és mérete megadott hello változókat, amelyek korábban inicializálták.

A következő parancsmag toocreate hello virtuálisgép-objektumot hello hajtható végre.

    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize

### <a name="create-a-credential-object-toohold-hello-name-and-password-for-hello-local-administrator-credentials"></a>A hitelesítő objektum toohold hello nevet és jelszót hello helyi rendszergazdai hitelesítő adatok létrehozása
Azt is beállíthatja a hello operációs rendszer tulajdonságokat hello virtuális géphez, igazolnia kell toosupply hello hitelesítő adatok hello helyi rendszergazdai fiók, egy biztonságos karakterláncot kell megadnia. tooaccomplish, használjuk hello [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) parancsmag.

A következő parancsmag hello hajtható végre, és hello Windows PowerShell hitelesítő adatok kérelem ablakban írja be a hello helyi rendszergazdai fiók hello Windows virtuális gép nevét és jelszavát toouse hello.

    $Credential = Get-Credential -Message "Type hello name and password of hello local administrator account."

### <a name="set-hello-operating-system-properties-for-hello-virtual-machine"></a>Hello hello virtuális gép operációs rendszer tulajdonságainak beállítása
Most már készen áll a tooset hello virtuális gép operációs rendszer tulajdonságainak folyamatban. Hello használjuk [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) parancsmag tooset hello típusú operációs rendszerrel, a Windows hello szükséges [virtuális gép ügynökének](../classic/agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) toobe telepítve, adja meg, hogy hello parancsmag lehetővé teszi, hogy az automatikus frissítés és hello virtuális gép neve, hello számítógép neve és hello hitelesítő adatok használatával hello változókat, amelyek korábban inicializálták.

Hajtsa végre a következő parancsmag tooset hello operációs rendszer tulajdonságai a virtuális gép hello.

    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate

### <a name="add-hello-network-interface-toohello-virtual-machine"></a>Hello hálózati illesztő toohello virtuális gép hozzáadása
A következő adunk hozzá hello hálózati illesztő létrehozott korábban toohello virtuális gépet. Hello használjuk [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) parancsmag tooadd hello hálózati illesztő hello hálózati illesztő változóval, amelyet korábban megadott.

A következő parancsmag tooset hello hálózati illesztő a virtuális gép hello hajtható végre.

    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id

### <a name="set-hello-blob-storage-location-for-hello-disk-toobe-used-by-hello-virtual-machine"></a>Hello blob storage helyének hello lemez toobe hello virtuális gép által használt beállítása
A következő azt állítja be, amelyet korábban megadott hello változók használata hello virtuális gép által használt hello lemez toobe hello blob tárolási helyét.

A következő parancsmag tooset hello blob tároló helye hello hajtható végre.

    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"

### <a name="set-hello-operating-system-disk-properties-for-hello-virtual-machine"></a>Hello operációs rendszer virtuális gép hello lemez tulajdonságainak beállítása
A következő helyezünk hello operációs rendszer lemez tulajdonságainak hello virtuális géphez. Hello használjuk [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk) parancsmag toospecify hello hello virtuális gép operációs rendszerét, kép, gyorsítótárazás csak tooread tooset határozza meg (mivel az SQL-kiszolgáló van telepítés alatt a hello ugyanazt a lemezt), és határozza meg hello virtuális gép nevét és a korábban meghatározott hello változók segítségével meghatározott hello operációsrendszer-lemez.

Hajtsa végre a következő parancsmag tooset hello operációs rendszer lemez tulajdonságai a virtuális gép hello.

    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

### <a name="specify-hello-platform-image-for-hello-virtual-machine"></a>Adja meg a hello platformlemezképet hello virtuális géphez
Az utolsó konfigurációs lépés toospecify hello platformlemezképet a virtuális gép. A jelen oktatóanyagban hello legújabb SQL Server 2016 CTP kép használunk. Hello használjuk [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) parancsmag toouse a kép határozzák meg, amelyet korábban megadott hello változók.

A következő parancsmag toospecify hello platformlemezképet a virtuális gép hello hajtható végre.

    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

## <a name="create-hello-sql-vm"></a>Hello SQL virtuális gép létrehozása
Most, hogy befejezte a hello konfigurációs lépésekkel, készen áll a toocreate hello virtuális gép áll. Hello használjuk [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) parancsmag toocreate hello virtuális gépet, hogy definiáltuk hello változók használatával.

A következő parancsmag toocreate hello hajtható végre a virtuális gép.

    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

hello a virtuális gép létrehozása. Figyelje meg, hogy egy standard szintű tárfiókot létre vonatkozó rendszerindítási diagnosztika, mert hello megadott hello virtuális gép lemezét tárfiók egy prémium szintű tárfiók.

Most már megtekintheti az ezen a számítógépen a hello Azure Portal toosee [nyilvános IP-címének és a teljes tartománynév](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="example-script"></a>A példaként megadott parancsfájlt
hello következő parancsfájl hello teljes PowerShell-parancsfájlt tartalmazó ehhez az oktatóanyaghoz. Azt feltételezi, hogy már telepítő hello Azure-előfizetés toouse a hello **Add-AzureRmAccount** és **Select-AzureRmSubscription** parancsok.

    # Variables
    ## Global
    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"
    ## Storage
    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

    ## Network
    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $TCPIPAllocationMethod = "Dynamic"
    $DomainName = "sqlvm1"

    ##Compute
    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

    ##Image
    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

    # Resource Group
    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

    # Storage
    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

    # Network
    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

    # Compute
    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
    $Credential = Get-Credential -Message "Type hello name and password of hello local administrator account."
    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate #-TimeZone = $TimeZone
    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

    # Image
    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

    ## Create hello VM in Azure
    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

## <a name="next-steps"></a>Következő lépések
Hello virtuális gép létrehozása után készen áll a tooconnect toohello virtuális gép RDP és a telepítő kapcsolatot áll. További információkért lásd: [csatlakozzon az SQL Server virtuális gépet az Azure (erőforrás-kezelő) tooa](virtual-machines-windows-sql-connect.md).

