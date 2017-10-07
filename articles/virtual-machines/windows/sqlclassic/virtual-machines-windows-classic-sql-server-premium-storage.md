---
title: "prémium szintű Azure Storage az SQL Server aaaUse |} Microsoft Docs"
description: "Ez a cikk hello klasszikus telepítési modellel létrehozott erőforrást használ, és a prémium szintű Azure Storage használata az Azure virtuális gépeken futó SQL Server útmutatást biztosít."
services: virtual-machines-windows
documentationcenter: 
author: danielsollondon
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: 7ccf99d7-7cce-4e3d-bbab-21b751ab0e88
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/01/2017
ms.author: jroth
ms.openlocfilehash: 393ea2020b39ea686302ae632e1049935c24af00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-premium-storage-with-sql-server-on-virtual-machines"></a>Az Azure Premium Storage és az SQL Server együttes használata virtuális gépeken
## <a name="overview"></a>Áttekintés
[Prémium szintű Storage](../../../storage/common/storage-premium-storage.md) van hello tárhelyet biztosít alacsony késéssel és magas teljesítmény IO következő generációja. A kulcs IO igényes munkaterhelések, például az SQL Server IaaS a legjobban [virtuális gépek](https://azure.microsoft.com/services/virtual-machines/).

> [!IMPORTANT]
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../azure-resource-manager/resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.

Ez a cikk ismerteti a tervezési és az SQL Server toouse prémium szintű Storage futtató virtuális gép áttelepítését. Ez magában foglalja az Azure-infrastruktúra (hálózati, tárolási) és a vendég Windows virtuális gép lépéseket. hello hello példát [függelék](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) bemutatja, hogyan toomove nagyobb virtuális gépek tootake előnyeit továbbfejlesztett teljes átfogó end tooend áttelepítését helyi SSD-tárolóba, a PowerShell használatával.

Fontos toounderstand hello végpont folyamata terjesztése prémium szintű Azure Storage az infrastruktúra-szolgáltatási virtuális gépeken futó SQL Server is. Ehhez a következőket:

* Hello Előfeltételek toouse prémium szintű Storage azonosítása.
* SQL Server telepítése az infrastruktúra-szolgáltatási tooPremium tárhely az új központi telepítéseknél példát.
* Önálló kiszolgálók és a üzembe helyezése SQL Always On rendelkezésre állási csoportok áttelepítése meglévő telepítés példát.
* Lehetséges áttelepítési módszer.
* Teljes-végpont például megjelenítő hello áttelepítés végrehajtásának egy meglévő Always On Azure, a Windows és az SQL Server lépései.

Tekintse meg az SQL Server Azure virtuális gépek további háttérinformációkat [SQL Server Azure virtuális gépek](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

**Szerző:** Daniel Sol **műszaki véleményezők:** Luis Carlos Vargas hering, Sanjay Mishra, Pravin Mital, Juergen Thomas, Gonzalo Ruiz.

## <a name="prerequisites-for-premium-storage"></a>Prémium szintű Storage előfeltételei
Prémium szintű Storage használatával több előfeltételei van.

### <a name="machine-size"></a>Mérete
Prémium szintű Storage használatához szüksége lesz a toouse DS adatsorozat virtuális gépek (VM). Ha DS adatsorozat gépek nem használta a felhőszolgáltatásban előtt, törölje a meglévő virtuális gép hello, hello csatlakoztatott lemezek tároljuk, és majd új felhőalapú szolgáltatás létrehozása előtt újra létrehozni a virtuális gép méreteként DS * szerepkör hello. További információ a virtuálisgép-méretek: [virtuális gépek és Felhőszolgáltatások mérete az Azure-](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="cloud-services"></a>Felhőszolgáltatások
Csak használata virtuális gépek DS * prémium szintű Storage az új felhőalapú szolgáltatás létrehozásakor. Ha az SQL Server Always On használ az Azure-ban, mindig figyelő hello toohello Azure belső vagy a külső terheléselosztási terheléselosztó IP-cím egy felhőalapú szolgáltatás társított vonatkozik. Ez a cikk foglalkozik, hogyan ebben a forgatókönyvben rendelkezésre állásának toomigrate.

> [!NOTE]
> Első virtuális gép, amely telepített toohello DS * több kell hello új felhőalapú szolgáltatás.
>
>

### <a name="regional-vnets"></a>Regionális VNETEK
A DS * virtuális gépek hello virtuális hálózatot (VNET) a regionális virtuális gépek toobe üzemeltető kell konfigurálni. A "szélesebb formája" hello VNET tooallow hello nagyobb virtuális gépek toobe más fürtök kiépítése és azok közötti kommunikáció lehetővé tételéhez. A következő képernyőkép hello hello kiemelt helyen mutatja regionális Vnetek, mivel hello első eredmény azt mutatja, a "keskeny" VNET.

![RegionalVNET][1]

A Microsoft támogatási jegy toomigrate tooa is növelheti regionális virtuális hálózat, a Microsoft fog olyan módosítást, majd toocomplete hello áttelepítési tooregional Vnetek, módosítsa a hello tulajdonság AffinityGroup hello a hálózati konfigurációban. Először exportálnia hello PowerShell a hálózati konfigurációt, és lecseréli a hello **AffinityGroup** hello tulajdonság **VirtualNetworkSite** elem egy **hely** tulajdonság. Adja meg `Location = XXXX` ahol `XXXX` egy Azure-régióban van. Importálja az új konfiguráció hello.

Például figyelembe véve a VNET konfigurációját a következő hello:

    <VirtualNetworkSite name="danAzureSQLnet" AffinityGroup="AzureSQLNetwork">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

toomove a tooa Nyugat-Európa, a regionális virtuális hálózat módosítása hello konfigurációs toohello a következő:

    <VirtualNetworkSite name="danAzureSQLnet" Location="West Europe">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

### <a name="storage-accounts"></a>Tárfiókok
Szüksége lesz egy új tárfiókot, amelyet a prémium szintű Storage beállított toocreate. Figyelje meg, hogy hello storage-fiók nem egyedi virtuális merevlemezek, a prémium szintű Storage hello használata van beállítva azonban a DS * adatsorozat virtuális gépek használatakor csatolhat a VHD-k a prémium és standard szintű tárolást fiókokhoz. Ha nem szeretné, hogy tooplace hello az operációs rendszer virtuális Merevlemezt a prémium szintű Storage-fiók toohello ez foglalkozhat.

hello következő **New-AzureStorageAccountPowerShell** hello "Premium_LRS" parancsot **típus** hoz létre a prémium szintű Storage-fiókok:

    $newstorageaccountname = "danpremstor"
    New-AzureStorageAccount -StorageAccountName $newstorageaccountname -Location "West Europe" -Type "Premium_LRS"   

### <a name="vhds-cache-settings"></a>Virtuális merevlemezek gyorsítótár beállításai
hello fő különbség a prémium szintű Storage-fiókok részét képező lemezek létrehozása, hello lemezgyorsítótár-beállítás. Használata javasolt az SQL Server adatmennyiség lemezek azt "**olvasási gyorsítótárazás**". A tranzakció naplózási kötetek, hello lemezgyorsítótár-beállítás beállításaként túl "**nincs**". Ez eltér a szabványos tárfiókok hello javaslatok.

Hello VHD-k csatolást követően hello lemezgyorsítótár-beállítás nem módosítható. Ehhez szükséges toodetach, és csatlakoztassa újra a virtuális merevlemez hello frissített gyorsítótár-beállítással.

### <a name="windows-storage-spaces"></a>Windows tárolóhelyek
Használhat [Windows tárolóhelyek](https://technet.microsoft.com/library/hh831739.aspx) úgy, ahogy az előző standard szintű Storage, ez lehetővé teszi egy virtuális Gépet, amely már van okhoz tárolóhelyek toomigrate. hello példát [függelék](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) (9-es és az utána lépés) azt mutatja be, hello Powershell kód tooextract, és importálja a virtuális gép több csatlakoztatott virtuális merevlemezek és.

Tárolókészletek szabványos Azure tárolási fiók tooenhance átviteli alkalmazott, és a késés csökkentésére. Bizonyára hasznosnak találja érték a prémium szintű Storage Tárolókészletek tesztelése az új központi telepítéseknél, de nagyobb fokú összetettségével jár tárolási telepítés adnak hozzá.

#### <a name="how-toofind-which-azure-virtual-disks-map-toostorage-pools"></a>Hogyan toofind mely Azure virtuális lemezek toostorage készletek leképezése
Mivel másik gyorsítótármappa beállítás javaslatok csatolt VHD-k, dönthet úgy, hogy toocopy hello VHD-k tooa prémium szintű Storage-fiók. Azonban amikor Ön áttelepítést csatlakoztassa őket újra toohello új DS adatsorozat VM, szükség lehet a tooalter hello gyorsítótár beállításait is. Prémium szintű Storage ajánlott gyorsítótár beállításai, ha külön virtuális merevlemezek hello SQL adatok fájlok napló fájlok (helyett és egy virtuális Merevlemezt, amely egyaránt tartalmaz) egyszerűbb tooapply hello.

> [!NOTE]
> Ha SQL Server adatainak és naplókönyvtárainak fájlok hello ugyanazon a köteten, gyorsítótárazási beállítását választja hello hello IO hozzáférési minták az adatbázis-terhelések függ. Csak tesztelési is bemutatják, milyen gyorsítótárazás esetén ajánlott ehhez a forgatókönyvhöz.
>
>

Azonban használata Windows tárolóhelyek, amelyek össze több VHD-k toolook, szüksége lesz az eredeti parancsfájlok tooidentify, amelyek csatlakoztatott virtuális merevlemezek milyen adott készletben, majd beállíthatja hello gyorsítótár beállításainak ennek megfelelően az egyes lemezek.

Ha nem rendelkezik eredeti parancsfájl elérhető tooshow, amely a VHD-k leképezése toohello tárolókészlethez, használhatja a következő lépéseket toodetermine hello lemezegységet/készlet leképezési hello.

Az egyes lemezek lépések hello használata:

1. Lemezek listájának beszerzése a hello tooVM csatolt **Get-AzureVM** parancs:

    Get-AzureVM - ServiceName <servicename> -név <vmname> |} Get-AzureDataDisk
2. Megjegyzés: hello Diskname és a logikai Egységet.

    ![DisknameAndLUN][2]
3. Távoli asztali kapcsolatot hello virtuális gép. Keresse meg a túl**számítógép-kezelés** | **Eszközkezelő** | **lemezmeghajtók**. Nézze meg az egyes hello "Microsoft virtuális lemezek" hello tulajdonságait

    ![VirtualDiskProperties][3]
4. Itt hello LUN számot a hivatkozási toohello LUN számot adja meg a hello VHD toohello virtuális gép csatlakoztatása.
5. A "Microsoft virtuális lemez" hello go toohello **részletek** fülre, majd a hello **tulajdonság** listában, nyissa meg túl**illesztőprogram kulcs**. A hello **érték**, Megjegyzés hello **eltolás**, vagyis a következő képernyőkép hello 0002. hello 0002 tárolási készlet hivatkozások hello hello Fizikailemez2 jelöli.

    ![VirtualDiskPropertyDetails][4]
6. Minden egyes tárolókészlethez kimenő hello memóriakép kapcsolódó lemezek:

    Get-StoragePool - FriendlyName AMS1pooldata |} Get-PhysicalDisk

    ![GetStoragePool][5]

Most már használhat ezen információk tooassociate csatolt VHD-k tooPhysical lemezek tárolókészletek.

Miután leképezte a VHD-k tooPhysical lemezek tárolókészletek válassza le, majd másolja őket keresztül tooa prémium szintű Storage-fiókot, majd csatolja a hello beállítása helyes gyorsítótár. Tekintse meg a hello hello példát [függelék](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage), 8 – 12 lépést. Ezeket a lépéseket hogyan tooextract virtuális merevlemez virtuális gép csatlakoztatott lemez konfigurációs tooa CSV-fájl, másolja a VHD-k hello, hello lemez konfigurációs gyorsítótár beállításainak módosításához, és végül telepítse újra hello VM, az összes hello DS több virtuális gép csatlakoztatott lemezekkel megjelenítése.

### <a name="vm-storage-bandwidth-and-vhd-storage-throughput"></a>Virtuális gép tárolási sávszélesség és a virtuális merevlemez tárolási teljesítmény
hello tárolási teljesítményének mértékét hello DS * Virtuálisgép-méretet megadott és hello virtuális merevlemez méretét. hello virtuális gépek különböző támogatás hello számát, amely lehet csatolni, és maximális sávszélesség (MB/s) fog támogatják hello VHD-k számára rendelkezik. Hello meghatározott sávszélesség számok, lásd: [virtuális gépek és Felhőszolgáltatások mérete az Azure-](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Nagyobb IOPS mérete nagyobb a érhetők el. Ez akkor érdemes megfontolni, amikor az áttelepítési útvonalának. További információkért [hello táblázatban találja az iops-érték és lemeztípusok](../../../storage/common/storage-premium-storage.md#scalability-and-performance-targets).

Mindemellett érdemes lehet megfontolnia, virtuális gépek támogatják az összes csatolt lemezek különböző maximális lemez sávszélesség rendelkezik-e. Magas terhelés alatt hello maximális sávszélesség érhető el a Virtuálisgép-szerepkör méretéhez sikerült telítsük. Például egy Standard_DS14 támogatja mentése too512MB/s. három P30 lemezzel ezért hello lemez sávszélesség hello VM telítsük sikerült. De ebben a példában hello átviteli korlátja sikerült túllépve, attól függően, hogy olvasási és írási IOs hello kombinációját.

## <a name="new-deployments"></a>Új központi telepítéséhez
hello következő két szakaszok bemutatják, hogyan telepítheti az SQL Server VMs tooPremium tároló. Ahogy korábban említettük, nem feltétlenül kell tooplace hello operációsrendszer-lemez prémium szintű storage-kiszolgálóra. Előfordulhat, hogy toodo ezt választja, ha meg vannak szándékos volt tooplace bármely intenzív IO-munkaterhelések az operációs rendszer virtuális merevlemez hello.

hello első példa bemutatja, meglévő Azure-gyűjtemény lemezképei használatával. hello második példa bemutatja, hogyan toouse egyéni VM lemezkép, amelyet a meglévő standard szintű tárfiók van.

> [!NOTE]
> Ezek a példák feltételezik, hogy már létrehozta a regionális virtuális Hálózatot.
>
>

### <a name="create-a-new-vm-with-premium-storage-with-gallery-image"></a>Prémium szintű Storage gyűjtemény lemezképpel új virtuális gép létrehozása
hello az alábbi példa bemutatja, hogyan tooplace az operációs rendszer virtuális merevlemez hello alakzatot prémium szintű storage-e, és csatolja a prémium szintű Storage VHD-k. Azonban is hello operációsrendszer-lemezzel helyez egy standard szintű tárfiókot, és majd csatolja a VHD-k, amelyek tárolása a prémium szintű Storage-fiók. Mindkét forgatókönyvet egy.

    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Set up subscription
    Set-AzureSubscription -SubscriptionName $mysubscription
    Select-AzureSubscription -SubscriptionName $mysubscription -Current  

#### <a name="step-1-create-a-premium-storage-account"></a>1. lépés: A prémium szintű Storage-fiók létrehozása
    #Create Premium Storage account, note Type
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  


#### <a name="step-2-create-a-new-cloud-service"></a>2. lépés: Új felhőalapú szolgáltatás létrehozása
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-reserve-a-cloud-service-vip-optional"></a>3. lépés: Lefoglalni egy felhőalapú szolgáltatás VIP (nem kötelező)
    #check exisitng reserved VIP
    Get-AzureReservedIP

    $reservedVIPName = “sqlcloudVIP”
    New-AzureReservedIP –ReservedIPName $reservedVIPName –Label $reservedVIPName –Location $location

#### <a name="step-4-create-a-vm-container"></a>4. lépés:, Hozzon létre egy Virtuálisgép-tároló
    #Generate storage keys for later
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    ##Generate storage acc contexts
    $xioContext = New-AzureStorageContext –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary   

    #Create container
    $containerName = 'vhds'
    New-AzureStorageContainer -Name $containerName -Context $xioContext

#### <a name="step-5-placing-os-vhd-on-standard-or-premium-storage"></a>5. lépés: az operációs rendszer virtuális Merevlemezt a Standard vagy prémium szintű Storage helyezi el.
    #NOTE: Set up subscription and default storage account which will be used tooplace hello OS VHD in

    #If you want tooplace hello OS VHD Premium Storage Account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $newxiostorageaccountname  

    #If you wanted tooplace hello OS VHD Standard Storage Account but attach Premium Storage VHDs then you would run this instead:
    $standardstorageaccountname = "danstdams"

    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $standardstorageaccountname

#### <a name="step-6-create-vm"></a>6. lépés: Virtuális gép létrehozása
    #Get list of available SQL Server Images from hello Azure Image Gallery.
    $galleryImage = Get-AzureVMImage | where-object {$_.ImageName -like "*SQL*2014*Enterprise*"}
    $image = $galleryImage.ImageName

    #Set up Machine Specific Information
    $vmName = "dsDan1"
    $vnet = "dansvnetwesteur"
    $subnet = "SQL"
    $ipaddr = "192.168.0.8"

    #Remember toochange tooDS series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "mycomplexpwd4*"

    #Create VM Config
    $vmConfigsl = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $image  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Add Data and Log Disks tooVM Config
    #Note hello size specified ‘-DiskSizeInGB 1023’, this will attach 2 x P30 Premium Storage Disk Type
    #Utilising hello Premium Storage enabled Storage account

    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-data1.vhd"
    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "logDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-log1.vhd"

    #Create VM
    $vmConfigsl  | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)  

    #Add RDP Endpoint
    $EndpointNameRDPInt = "3389"
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Add-AzureEndpoint -Name "EndpointNameRDP" -Protocol "TCP" -PublicPort "53385" -LocalPort $EndpointNameRDPInt  | Update-AzureVM

    #Check VHD storage account, these should be in $newxiostorageaccountname
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Get-AzureDataDisk
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName |Get-AzureOSDisk


### <a name="create-a-new-vm-toouse-premium-storage-with-a-custom-image"></a>Hozzon létre egy új virtuális gép toouse prémium szintű Storage egyéni kép
Ebben a forgatókönyvben azt mutatja be, melyekben egy standard szintű tárfiókot található meglévő testre szabott lemezképet. Ahogy azt korábban említettük, ha azt szeretné, tooplace hello az operációs rendszer virtuális Merevlemezt a prémium szintű Storage toocopy kell hello lemezképet, Standard szintű tárfiók hello szerepel, és helyezze tooa prémium szintű Storage használat előtt. Ha rendelkezik helyszíni kép, érdemes a metódus toocopy is használhatja, amely közvetlenül toohello prémium szintű Storage-fiók.

#### <a name="step-1-create-storage-account"></a>1. lépés: Tárfiók létrehozása
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Standard Storage account
    $origstorageaccountname = "danstdams"

#### <a name="step-2-create-cloud-service"></a>2. lépés a felhőalapú szolgáltatás létrehozása
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-use-existing-image"></a>3. lépés: A meglévő kép használata
Egy meglévő lemezképet is használhatja. Is [igénybe vehet egy meglévő számítógép lemezképét](../classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Megjegyzés: hello géphez rendszerképet készítene nincs toobe DS * gép. Miután hello kép, hello módját a következő lépéseket megjelenítése toocopy, prémium szintű Storage-fiókkal és hello toohello **Start-AzureStorageBlobCopy** PowerShell-parancsmag segítségével.

    #Get storage account keys:
    #Standard Storage account
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    #Premium Storage account
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Set up contexts for hello storage accounts:
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $destContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

#### <a name="step-4-copy-blob-between-storage-accounts"></a>4. lépés: Másolat Blob Storage-fiókok között
    #Get Image VHD
    $myImageVHD = "dansoldonorsql2k14-os-2015-04-15.vhd"
    $containerName = 'vhds'

    #Copy Blob between accounts
    $blob = Start-AzureStorageBlobCopy -SrcBlob $myImageVHD -SrcContainer $containerName `
    -DestContainer vhds -Destblob "prem-$myImageVHD" `
    -Context $origContext -DestContext $destContext  

#### <a name="step-5-regularly-check-copy-status"></a>5. lépés: Rendszeresen ellenőrizze a példány állapotát:
    $blob | Get-AzureStorageBlobCopyState

#### <a name="step-6-add-image-disk-tooazure-disk-repository-in-subscription"></a>6. lépés: Kép tooAzure lemezről lemezre tárház hozzáadása az előfizetéshez
    $imageMediaLocation = $destContext.BlobEndPoint+"/"+$myImageVHD
    $newimageName = "prem"+"dansoldonorsql2k14"

    Add-AzureVMImage -ImageName $newimageName -MediaLocation $imageMediaLocation

> [!NOTE]
> Előfordulhat, hogy annak ellenére, hogy hello állapotáról szóló jelentések sikeres, sikertelen továbbra is megjelenik bérleti lemezhiba. Ebben az esetben várjon körülbelül 10 percet.
>
>

#### <a name="step-7--build-hello-vm"></a>7. lépés: Hello virtuális gép létrehozása
Itt készítésekor hello VM a lemezkép és a VHD-k két Premium Storage:

    $newimageName = "prem"+"dansoldonorsql2k14"
    #Set up Machine Specific Information
    $vmName = "dansolchild"
    $vnet = "westeur"
    $subnet = "Clients"
    $ipaddr = "192.168.0.41"

    #This will need toobe a new cloud service
    $destcloudsvc = "danregsvcamsxio2"

    #Use tooDS Series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS3"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "theM)stC0mplexP@ssw0rd!”


    #Create VM Config
    $vmConfigsl2 = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $newimageName  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-Datadisk-1.vhd"
    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "LogDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-logdisk-1.vhd"



    $vmConfigsl2 | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet

## <a name="existing-deployments-that-do-not-use-always-on-availability-groups"></a>Always On rendelkezésre állási csoportok nem használó meglévő központi telepítések
> [!NOTE]
> A meglévő telepítések esetében először lásd: hello [Előfeltételek](#prerequisites-for-premium-storage) című szakaszát.
>
>

Nincsenek az Always On rendelkezésre állási csoportok és azok, amelyek nem használó SQL Server-telepítések kapcsolatos szempontokat. Ha nem használják a mindig bekapcsolva, és rendelkezik egy meglévő önálló SQL Server, frissítheti tooPremium tárolási új felhőalapú szolgáltatás és a tárolási fiók használatával. Vegye figyelembe az alábbi beállítások hello:

* **Hozzon létre egy új SQL Server virtuális gép**. Új SQL Server virtuális gép egy prémium szintű Storage-fiókot használó hozhat létre, az új központi megfelelően. Majd készítsen biztonsági másolatot, és az SQL Server-konfigurációs és felhasználói adatbázisok visszaállítása. hello alkalmazásnak szüksége lesz frissítve toobe tooreference hello új SQL-kiszolgálót, ha azt kívül és belül is hozzáférnek. Kellene toocopy minden "kívül db" objektumot, ha korábban végzett (SxS) SQL Server párhuzamos áttelepítés. Ez magában foglalja az objektumok, például a bejelentkezési adatok, a tanúsítványokat és a csatolt kiszolgálók.
* **Telepítse át a meglévő SQL Server virtuális**. Ehhez szükséges hello SQL Server virtuális gép offline állapotba helyezése, majd áthelyezte azt tooa új felhőalapú szolgáltatás, beleértve a csatlakoztatott virtuális merevlemezek toohello prémium szintű Storage-fiók összes másolása. Virtuális gép hello online állapotba kerül, ha hello alkalmazás hello állomásnevét, mielőtt hivatkozik. Vegye figyelembe, hogy a meglévő lemez hello hello mérete hatással lesz a hello teljesítményt nyújt. Például egy 400 GB lemezterület tooa P20 kerekíti lekérdezi. Ha tudja, hogy nincs szüksége a lemez teljesítménye akkor lehetett hello VM DS adatsorozat virtuális gépként hozza létre, és csatolja a prémium szintű Storage VHD-k hello mérete/teljesítmény specifikáció van szüksége. Ezután sikerült leválasztani a, és csatlakoztassa újra hello SQL-adatbázis a fájlokat.

> [!NOTE]
> Érdemes figyelembe hello méretű hello méretétől függően hello VHD lemezek másolásának azt jelenti, hogy prémium szintű tároló lemez típusát esnek, ez határozza meg a lemez teljesítménye megadását. Lemez legközelebbi toohello mentése round Azure lesz mérete, így ha egy 400 GB lemezterület, ez kerekíti tooa P20. Attól függően, hogy a meglévő IO-követelményeket a hello az operációs rendszer virtuális merevlemez előfordulhat, nem kell toomigrate a tooa prémium szintű Storage-fiók.
>
>

Az SQL Server külsőleg érhető el, ha hello cloud service VIP változik. Ki is tooupdate végpontok, a hozzáférés-vezérlési listák és a DNS-beállításait.

## <a name="existing-deployments-that-use-always-on-availability-groups"></a>Always On rendelkezésre állási csoportok használó meglévő központi telepítések
> [!NOTE]
> A meglévő telepítések esetében először lásd: hello [Előfeltételek](#prerequisites-for-premium-storage) című szakaszát.
>
>

Kezdetben ebben a szakaszban követően áttekintjük hogyan Always On Azure hálózatkezelési működjön. Azt fogja majd lebontva áttelepítések tootwo forgatókönyvekben: áttelepítéseket, ahol is megengedett némi állásidővel, és áttelepítéseket, ahol minimális állásidővel kell elérni.

A helyszíni SQL Server Always On rendelkezésre állási csoportok használatára a figyelő a helyi, amely egy virtuális DNS-nevet egy IP-cím, egy vagy több SQL Server-kiszolgálók között megosztott együtt regisztrálja. Ha az ügyfelek hello figyelő IP toohello elsődleges SQL-kiszolgálón keresztül halad. Ez az adott időpontban mindig az IP-erőforrás hello birtokló hello kiszolgáló.

![A DeploymentsUseAlways][6]

A Microsoft Azure-ban akkor is csak egy IP cím tooa hálózati adapter a hello VM, tehát a rendelés tooachieve hello azonos, a helyszíni absztrakciós réteget, Azure hello IP-címet, amely hozzá van rendelve toohello belső/külső terheléselosztó (ILB/ELB) használja. hello kiszolgálók között megosztott hello IP-erőforrás értéke toohello azonos IP mint hello ILB-/ ELB. Ez a hello DNS közzé van téve, és ügyfélforgalmat továbbítja a hello ILB-/ ELB toohello elsődleges SQL-kiszolgáló replika. hello ILB-/ ELB tudja, melyik SQL Server elsődleges óta mintavételt tooprobe hello mindig az IP-erőforrást használ. Hello előző példában azt vizsgálat hello ELB/ILB által hivatkozott végpont minden csomópont, a amelyik reagál hello elsődleges SQL Server.

> [!NOTE]
> hello ILB és üzembe helyezett ELB mindkét rendelt tooa adott Azure-felhőszolgáltatásban, ezért bármely áttelepülés a felhőbe az Azure-ban lesz valószínűleg jelenti azt, hogy hello Load Balancer IP változik.
>
>

### <a name="migrating-always-on-deployments-that-can-allow-some-downtime"></a>Áttelepítése mindig a központi telepítések engedélyezhetik bizonyos időre leállítást
Számos két stratégiák toomigrate mindig a központi telepítések lehetővé teszik a bizonyos időre leállítást.

1. **Több másodlagos replika tooan meglévő mindig a fürt hozzáadása**
2. **Telepítse át a tooa új mindig a fürt**

#### <a name="1-add-more-secondary-replicas-tooan-existing-always-on-cluster"></a>1. Több másodlagos replika tooan hozzáadása meglévő mindig a fürt
Egy stratégia tooadd van több másodlagos adatbázist toohello Always On rendelkezésre állási csoportnak. Új felhőalapú szolgáltatás be ezeket tooadd kell, és frissítse az hello figyelő hello új load balancer IP.

##### <a name="points-of-downtime"></a>Állásidő pontok:
* A fürt ellenőrzése.
* Új másodlagos adatbázis-tesztelési mindig a feladatátvételt.

Ha használ Tárolókészletek Windows hello VM belül IO nagyobb átviteli teljesítményt, akkor a rendszer offline állapotra állítja a teljes fürt ellenőrzése során. hello teszttel ellenőrizheti, ha a csomópontok toohello fürt hozzáadása. hello időt toorun hello teszt eltérőek lehetnek, így kell tesztelje a reprezentatív tesztelési környezetben tooget, hogy mennyi ideig Ez eltarthat egy megközelítőleges időpont, amikor.

Ha kézi feladatátvételre végezheti el, és chaos tesztelés hello az újonnan hozzáadott csomópontok tooensure magas rendelkezésre állású mindig a Funkciók, a várt kell kiépíteni.

![DeploymentUseAlways On2][7]

> [!NOTE]
> Ha hello Tárolókészletek hello érvényesítés futtatása előtt használt SQL Server összes példányát le kell állítani.
>
> ##### <a name="high-level-steps"></a>Magas szintű lépései
>

1. Hozzon létre új felhőszolgáltatás csatlakoztatott prémium szintű Storage két új SQL-kiszolgáló.
2. Másolja át a teljes biztonsági mentést, és állítsa vissza a **NORECOVERY**.
3. Másolja át a "kívül felhasználói DB" függő objektumok, például a bejelentkezéseket stb.
4. Létrehozhat új egy új belső Load Balancer (ILB), illetve egy külső Load Balancer (ELB) használja, majd állítsa be a terhelés eloszlik végpontok mindkét új csomópontjának.

   > [!NOTE]
   > Ellenőrizze minden csomópont hello megfelelő végpont-konfiguráció van, a folytatás előtt
   >
   >
5. Állítsa le a felhasználó vagy alkalmazás-hozzáférés toohello SQL Server (ha Tárolókészletek használata).
6. SQL Server adatbázismotor-szolgáltatások leállítása az összes olyan csomóponton, (ha Tárolókészletek használata).
7. Adja hozzá az új csomópontok toocluster, és futtassa teljes ellenőrzést.
8. Miután az ellenőrzés nem jelez hibát, indítsa el az összes SQL Server szolgáltatás.
9. Tranzakciós naplók biztonsági mentése és visszaállítása felhasználói adatbázisokat.
10. Vegyen fel új csomópontok hello Always On rendelkezésre állási csoportnak, és helyezze el a replikációs **szinkron**.
11. Hello IP-cím hozzáadása hello cím erőforrása új felhőalapú szolgáltatás ILB-/ ELB a PowerShell segítségével az Always On alapján hello többhelyes példát hello [függelék](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage). A Windows fürtszolgáltatás, állítsa be a hello **lehetséges tulajdonosok** a hello **IP-cím** erőforrás toohello új csomópontok régi. Című rész hello "IP-cím erőforrás hozzáadása ugyanazon az alhálózaton" hello [függelék](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).
12. Feladatátvételi tooone hello új csomópontok.
13. Ellenőrizze a hello új csomópontok automatikus feladatátvételi partnerként és feladatátvételi tesztek.
14. Távolítsa el az eredeti csomópont a rendelkezésre állási csoport.

##### <a name="advantages"></a>Előnyei
* Új SQL-kiszolgálók lehet tesztelni (SQL Server alkalmazás) tooAlways a Hozzáadás előtt.
* Hello VM Oldalméret módosítása oly módon, és testre szabhatja hello tárolási tooyour pontos követelményeit. Azonban hasznos tookeep lenne hello SQL fájl görbékhez hello azonos.
* Hello DB biztonsági mentések toohello másodlagos replikák hello átruházása elkezdésének szabályozhatja. Ez eltér az Azure használatával **Start-AzureStorageBlobCopy** parancsmag toocopy VHD-k, mert ez egy aszinkron másolatot.

##### <a name="disadvantages"></a>Hátrányok
* Windows Tárolókészletek használata esetén nincs fürt állásidő hello teljes fürtérvényesítési hello új további csomópontok során.
* Attól függően, hogy az SQL Server verziója hello és hello meglévő másodlagos replikák száma, akkor előfordulhat, hogy nem tud tooadd több másodlagos replika meglévő másodlagos adatbázis eltávolítása nélkül lehet.
* Előfordulhat, hogy hosszú SQL adatátviteli idő hello másodlagos beállítása közben.
* Nincs további költség nélkül az áttelepítés során előfordulhat, hogy a párhuzamosan futó új gépek.

#### <a name="2-migrate-tooa-new-always-on-cluster"></a>2. Telepítse át a tooa új mindig a fürt
Egy másik olyan stratégia toocreate egy teljesen új mindig a fürt új csomóponttal rendelkező új felhőalapú szolgáltatás és az átirányítási hello ügyfelek toouse azt.

##### <a name="points-of-downtime"></a>Állásidő pontok
Alkalmazások és a felhasználók toohello új mindig a figyelő-átvitel során, nincs leállás. hello állásidő függ:

* hello idő toorestore végső tranzakciós napló biztonsági mentések toodatabases új kiszolgálókon.
* hello igénybe vett idő tooupdate ügyfél alkalmazások toouse új mindig a figyelő.

##### <a name="advantages"></a>Előnyei
* Hello tényleges éles környezetben, SQL Server, tesztelheti, és operációs rendszer a módosításokat.
* Hello beállítás toocustomize hello tárhellyel rendelkező és toopotentially csökkentheti a virtuális gép méretét. Emiatt költségek csökkentéséhez.
* A SQL Server build vagy a verziójával frissítheti a folyamat során. Operációs rendszer hello is lehet frissíteni.
* hello előző mindig a fürt működhet teli visszaállítás céljaként.

##### <a name="disadvantages"></a>Hátrányok
* Ha azt szeretné, hogy mindkét fut egyszerre mindig a fürtök szüksége hello figyelő toochange hello DNS-nevét. Adminisztrációs terhet hello az áttelepítés során ez biztosítja a, az ügyfél alkalmazás karakterláncok tükröznie kell hello új figyelő nevét.
* Meg kell valósítani a szinkronizálási mechanizmus hello azokat a lehetséges toominimize hello végleges szinkronizálást követelmény áttelepítés előtt zárja be két környezetek tookeep között.
* Hiba kerül költség az áttelepítés során közben fut hello új környezettel rendelkezik.

### <a name="migrating-always-on-deployments-for-minimal-downtime"></a>Áttelepítése mindig a központi telepítések a minimális állásidő érdekében
Nincsenek áttelepítése mindig a központi telepítés két stratégiák a minimális állásidő érdekében:

1. **Egy létező másodlagos használják: egy helyen**
2. **Meglévő másodlagos másodpéldányt használják: többhelyes**

#### <a name="1-utilize-an-existing-secondary-single-site"></a>1. Egy létező másodlagos használják: egy helyen
A minimális állásidő érdekében egy stratégia tootake egy létező másodlagos felhőben, és távolítsa el az aktuális felhőszolgáltatás hello. Ezután másolja a VHD-k toohello új prémium szintű Storage-fiók hello, és hozzon létre hello VM hello új felhőalapú szolgáltatás. Módosítsa a fürtszolgáltatás és a feladatátvételi hello figyelő.

##### <a name="points-of-downtime"></a>Állásidő pontok
* Amikor a végső csomópontja hello hello az elosztott terhelésű végpont, nincs leállás.
* Az ügyfél újracsatlakozás ügyfél és a DNS-konfigurációtól függően előfordulhat, hogy később.
* Ha úgy dönt, hogy tootake hello mindig fürt csoport offline tooswap kimenő hello IP-címek, nincs további állásidő. Egy vagy függőség használatával elkerülheti ezt, és lehetséges tulajdonosainak hello hozzáadott IP-cím erőforrás. Című rész hello "IP-cím erőforrás hozzáadása ugyanazon az alhálózaton" hello [függelék](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).

> [!NOTE]
> Ha azt szeretné, hogy hello hozzáadott csomópont toopartake a partnerként mindig a feladatátvétel, tooadd a terhelés eloszlik beállítása hivatkozás toohello Azure végpont kell. Hello futtatásakor **Add-AzureEndpoint** toodo nyissa meg a, aktuális kapcsolatok tooremain parancsot, de új kapcsolatok toohello figyelő nem lesz képes toobe mindaddig, amíg hello terheléselosztó frissült. A tesztelés látott toolast 90-120seconds volt, ez kell vizsgálni.
>
>

##### <a name="advantages"></a>Előnyei
* Nincsenek további az áttelepítés során felmerülő költség.
* -Az-egyhez áttelepítés.
* Csökkentett összetettségét.
* Lehetővé teszi, hogy a megnövekedett IOPS a prémium szintű Storage SKU. Amikor hello lemezek hello VM leválasztani és toohello új felhőalapú szolgáltatás, másolja a 3. fél eszköz lehet magasabb teljesítmények biztosít használt tooincrease hello virtuális merevlemez méretét. Virtuális merevlemez mérete növekszik, lásd: a [fórum vitafórum](https://social.msdn.microsoft.com/Forums/azure/4a9bcc9e-e5bf-4125-9994-7c154c9b0d52/resizing-azure-data-disk?forum=WAVirtualMachinesforWindows).

##### <a name="disadvantages"></a>Hátrányok
* Nincs magas rendelkezésre ÁLLÁSÚ és vész-Helyreállítási átmenetileg megszakad az áttelepítés során.
* Mivel ez egy 1:1 áttelepítési, egy minimális Virtuálisgép-méretet, amely támogatja a virtuális merevlemezeket, számát, nem feltétlenül tudja toodownsize a virtuális gépek toouse fog rendelkezni.
* Ebben a forgatókönyvben használna hello Azure **Start-AzureStorageBlobCopy** parancsmag, amely aszinkron. Nincs nincs SLA-t a Másolás befejezése. hello példányok hello idő változó, amíg ez függ a várakozási sorban is függ adatok tootransfer hello mennyisége. hello ideje növekszik, ha hello átviteli tooanother Azure-adatközponthoz, amely támogatja a prémium szintű Storage egy másik régióban. Ha csak 2 csomópontok, inkább egy lehetséges megoldás, ha hello másolási tovább tart, mint a tesztelés. Ez magában foglalhatja a következő ötletek hello.
  * Ideiglenes 3. az SQL Server csomópont hozzáadása a magas rendelkezésre ÁLLÁSÚ egyeztetett állásidővel hello áttelepítés előtt.
  * Hello áttelepítéshez Azure ütemezett karbantartás kívül.
  * Győződjön meg arról, a fürt kvóruma megfelelően konfigurálta-e.  

##### <a name="high-level-steps"></a>Magas szintű lépései
Ez a dokumentum nem bemutatása teljes end tooend példában azonban hello [függelék](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) ismerteti, amelyek alkalmazhatók tooperform lehetnek ez.

![MinimalDowntime][8]

* Összefog lemez konfigurációját, és távolítsa el hello csomópont (ne törölje a csatolt VHD-k).
* Prémium szintű Storage-fiók létrehozása és virtuális merevlemezek másolása hello standard szintű tárfiók
* Új felhőalapú szolgáltatás létrehozása, és telepítse újra a hello SQL2 virtuális gép található, amely a felhőszolgáltatás. Hozzon létre virtuális gép hello hello használatával másolja az eredeti operációs rendszer virtuális merevlemez és a kapcsolódó hello másolja a VHD-k.
* Konfigurálja a ILB / ELB és végpont-hozzáadáshoz.
* Frissítés figyelő egyike:
  * Véve hello mindig csoport offline állapotú, és a frissítési hello mindig a figyelő az új ILB és üzembe helyezett ELB IP-cím.
  * Vagy hello IP-cím erőforrás az új felhőalapú szolgáltatás ILB-/ ELB Powershellen keresztül történő Windows-fürtszolgáltatás hozzáadására. Majd set hello lehetséges tulajdonosainak hello IP-cím erőforrás toohello csomópont, SQL2, át, és állítsa a hello hálózatnév vagy függőségként. Című rész hello "IP-cím erőforrás hozzáadása ugyanazon az alhálózaton" hello [függelék](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).
* Ellenőrizze a DNS-konfiguráció/propagálási toohello ügyfelek.
* Telepítse át az sql1 számítógép virtuális gép, és nyissa meg a 2 – 4. lépésben.
* Ha lépéseket 5ii használ, majd adja hozzá az sql1 számítógép ennek hello lehetséges tulajdonosa hozzá IP-cím erőforrás
* Feladatátvétel tesztelése.

#### <a name="2-utilize-existing-secondary-replicas-multi-site"></a>2. Meglévő másodlagos másodpéldányt használják: többhelyes
Ha több Azure-adatközpontban (DC) vannak olyan csomópontok, vagy ha hibrid környezettel rendelkezik, majd egy mindig a konfigurációt használhatja a környezet toominimize állásidőt.

hello megoldás toochange hello mindig a szinkronizálási tooSynchronous hello a helyszíni vagy Azure a másodlagos Tartományvezérlőt, majd feladatátvétel, SQL Server toothat keresztül. Hello VHD-k tooa prémium szintű Storage-fiók másolja, és az új felhőalapú szolgáltatás hello gép újbóli üzembe helyezése. Hello figyelő frissítése, és ezután a feladat-visszavételt.

##### <a name="points-of-downtime"></a>Állásidő pontok
hello állásidő hello idő toofailover toohello alternatív DC és biztonsági áll. Is függ, az ügyfél és a DNS-konfiguráció, és később, az ügyfél lehet újracsatlakozni.
Vegye figyelembe a következő példa egy hibrid mindig a konfiguráció hello:

![MultiSite1][9]

##### <a name="advantages"></a>Előnyei
* Úgy használhatja a meglévő infrastruktúrát.
* Hello beállítás toopre frissítési hello az Azure storage először a vész-Helyreállítási Azure DC hello rendelkezik.
* DR Azure DC tárolási hello újra kell konfigurálni.
* Nincs legalább két feladatátvételi teszteket kivéve az áttelepítés során.
* Nem kell toomove SQL Server-adatok biztonsági mentése és visszaállítása.

##### <a name="disadvantages"></a>Hátrányok
* Attól függően, hogy az ügyfél-hozzáférési tooSQL kiszolgáló előfordulhat nagyobb késéseket SQL Server egy másik tartományvezérlő toohello alkalmazás futtatásakor.
* virtuális merevlemezek tooPremium tárolási hello ideje hosszú lehet. Ez befolyásolhatja a döntést e tookeep hello hello rendelkezésre állási csoport csomópontja. Megfontolandó szempontok a amikor napló intenzív feladata terhelések hello az áttelepítés során futnak, mivel hello elsődleges csomópontot a tranzakciónaplóban árvává tranzakciók hello tookeep kell. Ezért ez sikerült jelentősen megnő.
* Ebben a forgatókönyvben használna hello Azure **Start-AzureStorageBlobCopy** parancsmag, amely aszinkron. Nincs nincs SLA befejezését követően. hello hello példányok idő változik, ez függ a várakozási sorban, miközben az adatok tootransfer hello mennyisége is függ. Ezért csak egy csomópont van a 2. adatközpontban, abban az esetben hello másolási tovább tart, mint a tesztelés megoldás lépéseket kell tennie. Ez magában foglalhatja a következő ötletek hello.
  * Adja hozzá a 2. az SQL ideiglenes csomópontot a magas rendelkezésre ÁLLÁSÚ egyeztetett állásidővel hello áttelepítés előtt.
  * Hello áttelepítéshez Azure ütemezett karbantartás kívül.
  * Győződjön meg arról, a fürt kvóruma megfelelően konfigurálta-e.

Ebben a forgatókönyvben feltételezi, hogy rendelkezik saját telepítési dokumentált, és tudja, hogyan le van képezve hello tárolási a rendelés toomake optimális gyorsítótár beállítások módosításait.

##### <a name="high-level-steps"></a>Magas szintű lépései
![Multisite2][10]

* Győződjön meg a helyszíni hello / Azure DC hello SQL Server elsődleges alternatív és könnyebben hello más automatikus feladatátvételi Partner (AFP).
* Lemez konfigurációs adatokat gyűjt a SQL2, és távolítsa el hello csomópont (ne törölje a csatolt VHD-k).
* Prémium szintű Storage-fiók létrehozása, és másolja a VHD-k a hello standard szintű tárfiókot.
* Hozzon létre egy új felhőalapú szolgáltatás, és hozzon létre a kapcsolódó díjakat tárolólemezek hello SQL2 virtuális gép.
* Konfigurálja a ILB / ELB és végpont-hozzáadáshoz.
* Frissítés hello mindig a figyelő az új ILB / ELB IP cím és a teszt feladatátvételt.
* Hello DNS konfigurációjának ellenőrzése.
* Hello AFP tooSQL2, módosítsa, majd telepítse át az sql1 számítógép és végrehajtania 2 – 5. lépéseket.
* Feladatátvétel tesztelése.
* Váltson vissza tooSQL1 hello AFP és SQL2

## <a name="appendix-migrating-a-multisite-always-on-cluster-toopremium-storage"></a>A függelék: Egy mindig a fürtözött tooPremium tároló áttelepítése
Ez a témakör további része hello példát egy részletes egy többhelyes mindig a tooPremium fürttároló alakítása. Egy külső terheléselosztó (ELB) tooan belső terheléselosztón (ILB) használatával a hello figyelő is alakítja.

### <a name="environment"></a>Környezet
* 2 KB-os Windows 12 / 2 KB-os SQL 12
* SP 1 DB fájlok
* Tárolókészletek csomópontonként x 2

![Appendix1][11]

### <a name="vm"></a>VIRTUÁLIS GÉP:
Ebben a példában fogjuk tér át egy üzembe helyezett ELB tooILB toodemonstrate. Üzembe helyezett ELB volt elérhető ILB, mielőtt, így ez azt jelenti, hogy hogyan tooswitch toothis során hello áttelepítési.

![Appendix2][12]

### <a name="pre-steps-connect-toosubscription"></a>Előkészítő lépések: Csatlakozás tooSubscription
    Add-AzureAccount

    #Set up subscription
    Get-AzureSubscription

#### <a name="step-1-create-new-storage-account-and-cloud-service"></a>1. lépés: Új Tárfiók létrehozása, és a felhőalapú szolgáltatás
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Storage accounts
    #current storage account where hello vm toomigrate resides
    $origstorageaccountname = "danstdams"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Generate storage keys for later
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Generate storage acc contexts
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $xioContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $origstorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #CREATE NEW CLOUD SVC
    $vnet = "dansvnetwesteur"

    ##Existing cloud service
    $sourceSvc="dansolSrcAms"

    ##Create new cloud service
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location

#### <a name="step-2-increase-hello-permitted-failures-on-resources-optional"></a>2. lépés: Növelje hello hibák engedélyezett erőforrások<Optional>
Bizonyos erőforrások tooyour mindig a rendelkezésre állási csoporthoz tartozó nincsenek korlátozások a hány sikertelen végrehajtása esetén, amelyek adott időszakban, ahol a hello Fürtszolgáltatás megkísérli toorestart hello erőforráscsoport fordul elő. Ajánlott növeli a miközben érdekében keresztül ez az eljárás, mert ellenkező esetben kézzel eseményindító és feladatátvételi feladatátvételek által gépek leállítása Bezárás toothis korlát kérheti le.

Volna kell körültekintő toodouble hello hiba támogatás, toodo Ez a Feladatátvevőfürt-kezelőben, nyissa meg hello mindig az erőforráscsoport toohello tulajdonságainak:

![Appendix3][13]

Hello maximális hibák too6 módosítása.

#### <a name="step-3-addition-ip-address-resource-for-cluster-group-optional"></a>3. lépés: IP-cím hozzáadása erőforrás fürt csoport<Optional>
Ha hello fürtcsoport csak egy IP-címe és igazított toohello felhő alhálózati, ügyeljen arra, ha véletlenül kapcsolat nélküli hello felhőalapú a fürt összes csomópontján, hogy hálózati majd hello fürt IP- és hálózati fürtnév nem lesz képes toocome online. A hello Ez megakadályozza az esemény tooother fürterőforrások frissíti.

#### <a name="step-4-dns-configuration"></a>4. lépés: DNS-konfiguráció
tooimplement zökkenőmentes átmenet, attól függ, hogyan DNS folyamatban van szükség, és frissíti.
Mindig telepítve van, ha létrehoz egy Windows fürterőforrás-csoporthoz, ha a Feladatátvevőfürt-kezelő megnyitásához láthatja, hogy legalább három erőforrások lesz, a dokumentum hello két hello tooare hivatkozik:

* Virtuális hálózat neve (VNN) – Ez az hello DNS-nevet, hogy az ügyfél toowhen tooconnect tooSQL mindig a kiszolgálókra, aki a csatlakozáshoz.
* IP-cím erőforrás – Ez az IP-címe, amelyhez társítva hello VNN hello, több is van, és többhelyes konfigurációban hely/alhálózatot / IP-cím lesz.

Amikor csatlakozó tooSQL Server, SQL Server ügyfél-illesztőprogram hello lekéri hello figyelő társított hello DNS-rekordokat, és próbálja tooconnect tooeach mindig a hozzárendelt IP-cím, alább néhány befolyásoló tényezők is ez tárgyaljuk.

hello hello figyelőjének nevével társított párhuzamos DNS-rekordok száma attól függ, nem csak hello társított IP-címek száma, de hello "RegisterAllIpProviders'setting a Feladatátvételi fürtszolgáltatás hello Always ON VNN erőforrás számára.

Always On Azure telepítésekor más lépéseket toocreate hello figyelő és IP-címek, hello "RegisterAllIpProviders" too1 konfigurálása toomanually rendelkezik, ez különböző tooan helyi mindig a központi már állítottak too1.

Ha a "RegisterAllIpProviders" értéke 0, majd csak akkor jelenik meg egy DNS-rekordot a DNS-ben társított hello figyelő:

![Appendix4][14]

Ha 'RegisterAllIpProviders' 1:

![Appendix5][15]

hello kódot fog kimenő hello dump VNN beállításait, és állítsa be az Ön, adjon megjegyzés, hello tootake hello VNN offline állapotba, és kapcsolja hálózatra újra kell tootake effektus módosítása, ez véve hello figyelő offline okozó ügyfél kapcsolat megszűnésének.

    ##Always On Listener Name
    $ListenerName = "Mylistener"
    ##Get AlwaysOn Network Name Settings
    Get-ClusterResource $ListenerName| Get-ClusterParameter
    ##Set RegisterAllProvidersIP
    Get-ClusterResource $ListenerName| Set-ClusterParameter RegisterAllProvidersIP  1

Egy későbbi lépésben áttelepítési tooupdate hello mindig a figyelő frissített IP-címet, amely a használatával hivatkozik a terheléselosztó kell, ez magában foglalja az IP-cím erőforrás eltávolítása és hozzáadását. Hello IP frissítés után kell tooensure hello új IP-címet a DNS-zóna frissítették, és hogy hello ügyfelek frissítése a helyi gyorsítótárban.

Ha az ügyfelek egy másik hálózati szegmensben találhatók, és egy másik DNS-kiszolgáló hivatkozik, mi történik, DNS-zóna átviteli kapcsolatos hello az áttelepítés során, hello alkalmazás újracsatlakozás idő fog korlátozott tooconsider kell által legalább hello zóna átviteli idő bármely új IP-címek hello figyelője. Ha itt időkorlát alatt, érdemes tárgyalja, és a Windows-csoportok egy növekményes zónaletöltés kényszerítése, és helyezheti is hello DNS állomás rekord tooa alacsonyabb idő tooLive (TTL), így hello ügyfelek frissítése. További információkért lásd: [növekményes zónaátvitelek](https://technet.microsoft.com/library/cc958973.aspx) és [Start-DnsServerZoneTransfer](https://technet.microsoft.com/library/jj649917.aspx).

Alapértelmezett hello TTL-t, a DNS-rekord társított hello mindig az Azure-ban a figyelő az 1200-as másodperc. Kezdésként érdemes lehet tooreduce ez Ha frissítésekor az áttelepítési tooensure hello ügyfelek hello figyelő DNS-frissített hello IP-címmel vannak időkorlát alatt. Megtekintheti és hello konfigurációjának módosítása okai kimenő hello VNN hello konfigurálása:

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"
    #Look at HostRecordTTL
    Get-ClusterResource $ListenerName| Get-ClusterParameter

    #Set HostRecordTTL Examples
    Get-ClusterResource $ListenerName| Set-ClusterParameter -Name "HostRecordTTL" 120

Vegye figyelembe, hello alacsonyabb hello "HostRecordTTL", a DNS-forgalom nagyobb mennyiségű történik.

##### <a name="client-application-settings"></a>Ügyfél-Alkalmazásbeállítások
Ha az SQL client alkalmazás támogatja a .net 4.5 hello SQLClient, akkor használhatja "MULTISUBNETFAILOVER = TRUE" kulcsszót, az ajánlott toobe szerint lehetővé teszi a gyorsabb kapcsolat tooSQL Always On rendelkezésre állási csoportnak a feladatátvétel során. Hello mindig a figyelő párhuzamosan társított összes IP-címek keresztül enumerálása, és átadta szigorúbb TCP-kapcsolat újrapróbálkozási sebességét.

A fenti hello-beállítások kapcsolatos további információkért lásd: [MultiSubnetFailover kulcsszó és a kapcsolódó szolgáltatások](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover). Lásd még: [SqlClient támogatja a magas rendelkezésre állási, vészhelyreállítási](https://msdn.microsoft.com/library/hh205662\(v=vs.110\).aspx).

#### <a name="step-5-cluster-quorum-settings"></a>5. lépés: Fürt kvórumbeállításainak megadása
Mivel legalább egy SQL Server le egyszerre véve toobe fog, módosítsa hello fürt kvórumbeállításokat, ha 2 csomópontok fájl megosztási tanúsító (FSW) használ, meg kell hello kvórum tooallow csomóponttöbbség beállítása és felhasználását dinamikus szavazás, és ezek a tooallow egy egycsomópontos tooremain állandó.

    Set-ClusterQuorum -NodeMajority  

További információ a kezelése és hello a fürtkvórum konfigurálása, lásd: [konfigurálása és kezelése a Windows Server 2012 feladatátvevő fürt kvórum hello](https://technet.microsoft.com/library/jj612870.aspx).

#### <a name="step-6-extract-existing-endpoints-and-acls"></a>6. lépés: Bontsa ki a meglévő végpontok és hozzáférés-vezérlési listák
    #GET Endpoint info
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureEndpoint
    #GET ACL Rules for Each EP, this example is for hello Always On Endpoint
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureAclConfig -EndpointName "myAOEndPoint-LB"  

Ezek tooa szöveges fájlt mentse.

#### <a name="step-7-change-failover-partners-and-replication-modes"></a>7. lépés: Feladatátvételi partnerként és replikációs mód módosítása
Ha 2-nél több SQL-kiszolgálóval rendelkezik, meg kell változtatni a másik tartományvezérlő egy másik másodlagos hello feladatátvételi vagy helyszíni too'Synchronous', és teszi az automatikus feladatátvételi Partner (AFP), ez a helyzet magas rendelkezésre ÁLLÁSÚ karbantartása, míg a módosításokat. Ezt megteheti a TSQL használatával is módosíthatja, ha a szolgáltatáshoz az SSMS:

![Appendix6][16]

#### <a name="step-8-remove-secondary-vm-from-cloud-service"></a>8. lépés: Másodlagos virtuális gép eltávolítása a felhőalapú szolgáltatás
Kell egy felhőalapú másodlagos csomópont toomigrate tervezési először, ha jelenleg az elsődleges, akkor kell kezdeményezni kézi feladatátvételre.

    $vmNameToMigrate="dansqlams2"

    #Check machine status
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Shutdown secondary VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM


    #Extract disk configuration

    ##Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk config, make sure below returns hello disks associated with hello VM
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machibe is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild toonew cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-9-change-disk-caching-settings-in-csv-file-and-save"></a>9. lépés: Lemez gyorsítótárazási beállítások a CSV-fájlban, és mentse
A adatkötetek ezek tooREADONLY kell állítani.

TLOG kötetek ezek tooNONE kell állítani.

![Appendix7][17]

#### <a name="step-10-copy-vhds"></a>10. lépés: Másolat VHD-k
    #Ensure you have created hello container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContext

    ####DISK COPYING####
    #Get disks from csv, get settings for each VHDs and copy tooPremium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname.blob.core.windows.net/vhds/$vhdname" `
    -SrcContext $origContext `
    -DestContainer $containerName `
    -DestBlob $vhdname `
    -DestContext $xioContext
       }



Hello példány állapotának hello VHD-k toohello prémium szintű Storage-fiók ellenőrizheti:

    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContext
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix8][18]

Várjon, amíg a fenti sikeres fájl rögzíti.

Az egyes blobok információt:

    Get-AzureStorageBlobCopyState -Blob "blobname.vhd" -Container $containerName -Context $xioContext

#### <a name="step-11-register-os-disk"></a>11. lépés: az operációs rendszer Register lemez
    #Change storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

#### <a name="step-12-import-secondary-into-new-cloud-service"></a>12. lépés: Importálása másodlagos új felhőszolgáltatás
hello kódot is használ hello hozzá alább beállítást itt meg lehet hello gép importálni és használni hello retainable VIP.

    #Build VM Config
    $ipaddr = "192.168.0.5"
    #Remember toochange tooXIO
    $newInstanceSize = "Standard_DS13"
    $subnet = "SQL"

    #Create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###Attaching disks tooa VM during a deploy tooa new cloud service and new storage account is different from just attaching VHDs toojust a redeploy in a new cloud service
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)

#### <a name="step-13-create-ilb-on-new-cloud-svc-add-load-balanced-endpoints-and-acls"></a>13. lépés: Hozható létre ILB új felhő Svc, vegye fel a terhelés eloszlik a végpontok és hozzáférés-vezérlési listák
    #Check for existing ILB
    GET-AzureInternalLoadBalancer -ServiceName $destcloudsvc

    $ilb="sqlIntIlbDest"
    $subnet = "SQL"
    $IP="192.168.0.25"
    Add-AzureInternalLoadBalancer -ServiceName $destcloudsvc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP

    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM

    #SET Azure ACLs or Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

    ####WAIT FOR FULL AlwaysOn RESYNCRONISATION!!!!!!!!!#####

#### <a name="step-14-update-always-on"></a>14. lépés: Mindig a frissítése.
    #Code toobe executed on a Cluster Node
    $ClusterNetworkNameAmsterdam = "Cluster Network 2" # hello azure cluster subnet network name
    $newCloudServiceIPAmsterdam = "192.168.0.25" # IP address of your cloud service

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"


    Add-ClusterResource "IP Address $newCloudServiceIPAmsterdam" -ResourceType "IP Address" -Group $AGName -ErrorAction Stop |  Set-ClusterParameter -Multiple @{"Address"="$newCloudServiceIPAmsterdam";"ProbePort"="59999";SubnetMask="255.255.255.255";"Network"=$ClusterNetworkNameAmsterdam;"OverrideAddressMatch"=1;"EnableDhcp"=0} -ErrorAction Stop

    #set dependancy and NETBIOS, then remove old IP address

    #set NETBIOS, then remove old IP address
    Get-ClusterGroup $AGName | Get-ClusterResource -Name "IP Address $newCloudServiceIPAmsterdam" | Set-ClusterParameter -Name EnableNetBIOS -Value 0

    #set dependency tooListener (OR Dependency) and delete previous IP Address resource that references:

    #Make sure no static records in DNS

![Appendix9][19]

Hello régi felhőalapú szolgáltatás IP-cím eltávolítása.

![Appendix10][20]

#### <a name="step-15-dns-update-check"></a>15. lépés: A DNS-frissítési ellenőrzése
Most DNS-kiszolgálók ellenőrizze az SQL Server ügyfél hálózatokon és győződjön meg arról, hogy a fürtszolgáltatás hozzáadta hello hello extra állomásrekordot hozzáadott IP-címet. Ha ezeket a DNS-kiszolgálók nem frissítették, fontolja meg a DNS zónaletöltés kényszerítése, és győződjön meg arról, hogy hello ott alhálózaton lévő ügyfelek képesek tooresolve tooboth mindig az IP-címek, ez így nem kell toowait a automatikus DNS-replikáció az.

#### <a name="step-16-reconfigure-always-on"></a>16. lépés: Mindig a újrakonfigurálása
Ezen a ponton, de a áttelepített toofully csomóponton újraszinkronizálása hello helyi csomóponthoz, és toosynchronous replikációs csomópont váltson, és könnyebben hello AFP másodlagos hello várja.  

#### <a name="step-17-migrate-second-node"></a>17. lépés: A második csomópont áttelepítése
    $vmNameToMigrate="dansqlams1"

    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Get endpoint information
    $endpoint = Get-AzureVM -ServiceName $sourceSvc  -Name $vmNameToMigrate | Get-AzureEndpoint

    #Shutdown VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM

    #Get disk config

    #Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk configuration
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machine is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild toonew cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-18-change-disk-caching-settings-in-csv-file-and-save"></a>18. lépés: Lemez gyorsítótárazási beállítások a CSV-fájlban, és mentse
A adatkötetek ezek tooREADONLY kell állítani.

TLOG kötetek ezek tooNONE kell állítani.

![Appendix11][21]

#### <a name="step-19-create-new-independent-storage-account-for-secondary-node"></a>19. lépés: A másodlagos csomópont új független Storage-fiók létrehozása
    $newxiostorageaccountnamenode2 = "danspremsams2"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountnamenode2 -Location $location -Type "Premium_LRS"  

    #Reset hello storage account src if node 1 in a different storage account
    $origstorageaccountname2nd = "danstdams2"

    #Generate storage keys for later
    $xiostoragenode2 = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountnamenode2

    #Generate storage acc contexts
    $xioContextnode2 = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountnamenode2 -StorageAccountKey $xiostoragenode2.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

#### <a name="step-20-copy-vhds"></a>20. lépés: Másolat VHD-k
    #Ensure you have created hello container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContextnode2  

    ####DISK COPYING####
    ##get disks from csv, get settings for each VHDs and copy tooPremium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname2nd.blob.core.windows.net/vhds/$vhdname" `
        -SrcContext $origContext `
        -DestContainer $containerName `
        -DestBlob $vhdname `
        -DestContext $xioContextnode2
       }

    #Check for copy progress

    #check induvidual blob status
    Get-AzureStorageBlobCopyState -Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd" -Container $containerName -Context $xioContext


Hello VHD-másolat állapota ellenőrizze, hogy minden virtuális merevlemez: ForEach (a $diskobjects $disk) {$lun = $disk. LUN $vhdname $disk.vhdname $cacheoption = = $disk. HostCaching $disklabel = $disk. Lemezcímke $diskName = $disk. DiskName

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContextnode2
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix12][22]

Várjon, amíg a fenti sikeres fájl rögzíti.

Az egyes blobok információt:

    #Check induvidual blob status
    Get-AzureStorageBlobCopyState -Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd" -Container $containerName -Context $xioContextnode2

#### <a name="step-21-register-os-disk"></a>21. lépés: az operációs rendszer Register lemez
    #change storage account toohello new XIO storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

    #Build VM Config
    $ipaddr = "192.168.0.4"
    $newInstanceSize = "Standard_DS13"

    #Join tooexisting Avaiability Set

    #Build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###This is different toojust a straight cloud service change
    #note if you do not have a disk label hello command below will fail, populate as required.
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet -Verbose

#### <a name="step-22-add-load-balanced-endpoints-and-acls"></a>22. lépés: Adja hozzá a terhelés eloszlik a végpontok és hozzáférés-vezérlési listák
    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM


    #STOP!!! CHECK in hello Azure portal or Machine Endpoints through PowerShell that these Endpoints are created!

    #SET ACLs or Azure Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

#### <a name="step-23-test-failover"></a>23. lépés: Feladatátvételi teszt
Ha hagyja hello áttelepített csomópont szinkronizálja a helyszíni hello mindig a csomóponttal, helyezze toosynchronous replikációs mód, és várjon, amíg azt szinkronizálása. Ezután feladatátvétel helyszíni toohello első csomópontot az áttelepíti, amely hello AFP. Amennyiben rendelkezik működőképes, módosítás hello legutóbbi áttelepítésük óta csomópont toohello AFP.

Feladatátvételi tesztek összes csomópontok között kell, és bár tooensure feladatátvételek működhet chaos tesztek várt, egy időben manor futnak.

#### <a name="step-24-put-back-cluster-quorum-settings--dns-ttl--failover-pntrs--sync-settings"></a>24. lépés: Helyezze vissza fürt kvórumbeállításainak megadása / DNS-élettartam / feladatátvételi Pntrs / szinkronizálási beállítások
##### <a name="adding-ip-address-resource-on-same-subnet"></a>Azonos alhálózatban lévő IP-cím erőforrás hozzáadása
Ha csak 2 SQL-kiszolgálókhoz, és toomigrate szeretné őket tooa új felhőalapú szolgáltatás, de nem szeretnének tookeep azokat a hello ugyanazon az alhálózaton, elkerülheti a mindig offline toodelete hello eredeti hello figyelő véve az IP-címet, és adja hozzá az új IP-cím hello. Ha az áttelepítés hello virtuális gépek tooanother alhálózati nincs szükség van egy toodo ez nem lesznek egy fürt további hálózati alhálózaton fog hivatkozni.

Miután léptetik hello másodlagos át, és előtt feladatátvételi hello meglévő elsődleges hozzáadott új IP-cím erőforrás hello hello új felhőalapú szolgáltatás, ezeket a lépéseket a fürt Feladatátvevőfürt-kezelő hello belül kell végrehajtani:

az IP-cím tooadd lásd: hello [függelék](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage), 14 lépésben.

1. Az aktuális IP-cím erőforrás hello, módosítsa a hello lehetséges tulajdonos too'Existing elsődleges SQL Server ", az alábbi"dansqlams4"hello példa:

    ![Appendix13][23]
2. Hello új IP-cím erőforráson, módosítsa a hello lehetséges tulajdonos too'Migrated másodlagos SQL-kiszolgáló ", az alábbi"dansqlams5"hello példa:

    ![Appendix14][24]
3. Miután ezt állítja be a következőket teheti feladatátvevő, és hello utolsó csomópontja áttelepítésekor hello lehetséges tulajdonosok módosítani kell úgy, hogy a csomópont lehetséges tulajdonosa meg van adva:

    ![Appendix15][25]

## <a name="additional-resources"></a>További források
* [Prémium szintű Azure Storage](../../../storage/common/storage-premium-storage.md)
* [Virtuális gépek](https://azure.microsoft.com/services/virtual-machines/)
* [SQL Server Azure virtuális gépeken](../sql/virtual-machines-windows-sql-server-iaas-overview.md)

<!-- IMAGES -->
[1]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/1_VNET_Portal.png
[2]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/2_Diskname_Lun.png
[3]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/3_Virtual_Disk_Properties.png
[4]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/4_Virtual_Disk_Properties_Details.png
[5]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/5_Get_Storage_Pool.png
[6]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/6_Deployments_Always_On.png
[7]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/7_Add_More_Secondaries.png
[8]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/8_Use_Existing_Secondary_Single_Site.png
[9]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site.png
[10]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site_b.png
[11]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_01.png
[12]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_02.png
[13]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_03.png
[14]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_04.png
[15]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_05.png
[16]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_06.png
[17]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_07.png
[18]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_08.png
[19]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_09.png
[20]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_10.png
[21]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_11.png
[22]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_12.png
[23]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_13.png
[24]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_14.png
[25]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_15.png
