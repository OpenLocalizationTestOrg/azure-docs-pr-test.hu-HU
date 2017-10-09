# <a name="common-errors-during-classic-tooazure-resource-manager-migration"></a>Klasszikus tooAzure erőforrás-kezelő áttelepítése során előforduló hibákat
Ez a cikk katalógusok leggyakrabban előforduló hibákat, és azok mérséklési hello Azure klasszikus telepítési modell toohello Azure Resource Manager veremből IaaS-erőforrásokra hello áttelepítése során.

## <a name="list-of-errors"></a>Hibalista
| Hibakarakterlánc | Kezelés |
| --- | --- |
| Belső kiszolgálóhiba |Bizonyos esetekben ez egy átmeneti hiba, ami újbóli próbálkozással megszűnik. Ha folytatja toopersist, [kérje az Azure támogatási](../articles/azure-supportability/how-to-create-azure-support-request.md) platform naplók vizsgálat igényeinek megfelelően. <br><br> **Megjegyzés:** hello incidens hello támogatási csoport követi nyomon, ha nem kísérelni bármely önálló megoldás, előfordulhat, hogy ez a környezet nem várt következményekkel. |
| A migrálás az {üzemeltetett szolgáltatás} üzemeltetett szolgáltatás {üzemelő példány neve} üzemelő példánya esetében nem támogatott, mivel ez egy PaaS üzemelő példány (webes/feldolgozó). |Ez akkor fordul elő, ha az üzemelő példány egy webes/feldolgozói szerepkört tartalmaz. Áttelepítés csak virtuális gépek esetén támogatott, mert távolítsa el a hello telepítési hello webes vagy feldolgozói szerepkör, és próbálja megismételni az áttelepítést. |
| A {sablonnév} sablon üzembe helyezése meghiúsult. CorrelationId={guid} |Hello háttér áttelepítési szolgáltatás Azure Resource Manager sablonok toocreate erőforrások használjuk hello Azure Resource Manager-készletben. Mivel a sablonok az idempotent, általában biztonságos megpróbálkozhat a hello áttelepítési művelet tooget túli ezt a hibát. Ha ez a hiba továbbra is fennáll, toopersist, [kérje az Azure támogatási](../articles/azure-supportability/how-to-create-azure-support-request.md) és hello CorrelationId számukra. <br><br> **Megjegyzés:** hello incidens hello támogatási csoport követi nyomon, ha nem kísérelni bármely önálló megoldás, előfordulhat, hogy ez a környezet nem várt következményekkel. |
| {hello virtuális-hálózati-name} virtuális hálózat nem létezik. |Ez akkor fordulhat elő, ha a virtuális hálózati hello hello új Azure-portálon létrehozott. hello tényleges Virtuálishálózat-név hello mintát követi "csoport * <VNET name>" |
| Az {üzemeltetett szolgáltatás neve} üzemeltetett szolgáltatás {virtuális gép neve} virtuális gépe tartalmazza a {bővítmény neve} bővítményt, amely az Azure Resource Managerben nem támogatott. Ajánlott toouninstall hello virtuális gép áttelepítés folytatása előtt le. |Az XML bővítmények, például a BGInfo 1.*, nem támogatottak az Azure Resource Managerben. Ezért az ilyen bővítmények nem migrálhatók. Ha ezek a bővítmények bal telepíthető hello virtuális gépet, akkor lesznek automatikusan eltávolítva hello az áttelepítés befejezése előtt. |
| Az {üzemeltetett szolgáltatás neve} üzemeltetett szolgáltatás {virtuális gép neve} virtuális gépe tartalmazza a VMSnapshot/VMSnapshotLinux bővítményt, amely jelenleg a migrálási szolgáltatásban nem támogatott. Távolítsa el azt a virtuális gép hello, majd vegye fel azt követően hello áttelepítési Complete vissza Azure Resource Manager használatával |Ez a hello forgatókönyv, ahol a virtuális gép hello van-e beállítva az Azure biztonsági mentés. Mivel jelenleg egy nem támogatott forgatókönyvet, kérjük, kövesse a https://aka.ms/vmbackupmigration hello megoldás |
| VM {vm-neve} {üzemeltetett-szolgáltatásnév} üzemeltetett bővítményt {kiterjesztésnév} állapotú nem ügynökökről származó hello VM tartalmazza. Ezért ez a virtuális gép nem migrálható. Győződjön meg arról, hogy hello bővítmény állapotának ügynökökről vagy hello VM hello bővítmény eltávolítása, majd próbálja megismételni a áttelepítési. <br><br> Az {üzemeltetett szolgáltatás neve} üzemeltetett szolgáltatás {virtuális gép neve} virtuális gépe tartalmazza a {bővítmény neve} bővítményt, amely a {kezelői állapot} kezelői állapotot jelenti. Emiatt hello virtuális gép nem telepíthető át. Győződjön meg arról, hogy hello bővítmény kezelő állapotot jelentett {kezelő-status}, vagy távolítsa el azt a virtuális gép hello, majd próbálja megismételni a áttelepítési. <br><br> Virtuálisgép-ügynök üzemeltetett {üzemeltetett-szolgáltatásnév} {vm – name} virtuális géphez van-e jelentés hello általános ügynök állapota nem üzemkész. Ezért hello VM előfordulhat, hogy nem telepíthetők át, ha áttelepíthető-e célállomásra bővítmény van. Győződjön meg arról, hogy hello Virtuálisgép-ügynök az jelentéskészítési készként összesített ügynök állapotát. Tekintse meg a toohttps://aka.ms/classiciaasmigrationfaqs. |Azure vendégügynök & Virtuálisgép-bővítmények kimenő internet access toohello VM tárolási fiók toopopulate kell az állapotukat. Az állapothibák gyakori okai a következők lehetnek <li> a hálózati biztonsági csoport, amely megakadályozza a kimenő hozzáférést toohello internet <li> Ha a virtuális hálózat hello rendelkezik a helyszínen DNS-kiszolgálók és DNS-kapcsolat elvész <br><br> Egy nem támogatott állapot toosee továbbra is, ha hello bővítmények tooskip eltávolítani ezt az ellenőrzést, és előre áttelepítési együtt mozognak. |
| A migrálás az {üzemeltetett szolgáltatás neve} üzemeltetett szolgáltatás {üzemelő példány neve} üzemelő példánya esetében nem támogatott, mivel ez több rendelkezésre állási csoporttal rendelkezik. |Jelenleg csak az 1 vagy kevesebb rendelkezésre állási csoporttal rendelkező üzemeltetett szolgáltatások migrálhatók. a probléma megoldásához toowork helyezze hello további, rendelkezésre állási készletek és a virtuális gépek az adott rendelkezésre állási készletek tooa különböző üzemeltetett szolgáltatásban. |
| Áttelepítés nem támogatott telepítési {telepítési neve} üzemeltetett {üzemeltetett-szolgáltatásnév mert a virtuális gépek, amelyek nem részei rendelkezésre állási csoport hello annak ellenére, hogy egy vagy több üzemeltetett hello tartalmaz. |hello megoldás ehhez a forgatókönyvhöz, hello egy egyetlen rendelkezésre állási az összes virtuális gép beállítása, vagy távolítsa el az összes virtuális gép rendelkezésre állási készlethez megadott hello üzemeltetett szolgáltatás hello tooeither áthelyezés. |
| Fiók/üzemeltetett/virtuális tárolási {virtuális-hálózati-name} hálózati áttelepítendő hello folyamatát, és ezért nem módosítható |Ez a hiba történik, ha hello "Felkészülés" áttelepítési művelet befejezése után hello erőforráson és módosítás toohello erőforrás tenné művelet elindul. Hello zárolást hello felügyeleti vezérlősík "Felkészülés" művelet, mert a módosítások toohello erőforrás le vannak tiltva. toounlock hello felügyeleti vezérlősík hello "Véglegesítése" áttelepítési művelet toocomplete áttelepítési futtathatja, vagy hello "Megszakítás" áttelepítési művelet tooroll vissza hello "Felkészülés" műveletet. |
| Az {üzemeltetett szolgáltatás neve} üzemeltetett szolgáltatás migrálása nem engedélyezett, mivel a benne lévő {virtuális gép neve} virtuális gép állapota RoleStateUnknown. Áttelepítési engedélyezett, csak ha a virtuális gép van a következő hello hello állapota - fut, leállítva, felszabadítása leállt. |hello VM elképzelhető, hogy végzi egy állapotváltás, amely általában akkor fordul elő, például újraindítás üzemeltetett hello a frissítési művelet során, a bővítmény telepítése stb. Ajánlott hello frissítési művelet toocomplete hello üzemeltetett az áttelepítés előtt. |
| Telepítési {telepítési neve} {üzemeltetett-szolgáltatásnév} üzemeltetett adatlemez {adatok-lemez-neve} amelynek fizikai blob mérete {size-of-the-vhd-blob-backing-the-data-disk} bájt nem egyezik meg a hello VM adatlemez logikai méret {{vm – name} virtuális gép tartalmazza size-of-the-Data-Disk-specified-in-the-VM-API} bájt. Áttelepítés egy hello használható adatlemez a(z) hello Azure Resource Manager virtuális gép méretének megadása nélkül folytatódik. | Ez a hiba történik, ha már átméretezte hello Virtuálismerevlemez-blobot hello mérete hello VM API modell frissítése nélkül. A hibakezelés részletes lépéseit az [alábbi szakasz](#vm-with-data-disk-whose-physical-blob-size-bytes-does-not-match-the-vm-data-disk-logical-size-bytes) ismerteti.|
| Tárolási hiba lépett fel a {felhőszolgáltatás neve} felhőszolgáltatásban lévő {virtuális gép neve} virtuális gépre mutató {adatlemez URI azonosítója} adathordozó-hivatkozással rendelkező {adatlemez neve} adatlemez érvényesítése során. Győződjön meg arról, hogy hello VHD Médiahivatkozás érhető el a virtuális gép számára | Ez a hiba akkor fordulhat elő, ha hello hello lemezek virtuális gép törölve lett, vagy nem érhető el többé. Győződjön meg arról, hogy egy virtuális gép létezik hello hello lemez.|
| A {felhőszolgáltatás neve} üzemeltetett szolgáltatás {virtuális gép neve} virtuális gépe tartalmazza a(z) {virtuális merevlemez uri-azonosítója} adathordozó-hivatkozással rendelkező lemezt, amely a {virtuális merevlemez blob neve} nevű blobot tartalmazza, amely az Azure Resource Managerben nem támogatott. | Ez a hiba akkor fordul elő, ha hello blob hello neve a "/" azt is, amely nem támogatott az erőforrás-szolgáltató számítási jelenleg. |
| Nem migrálható üzemeltetett {felhőszolgáltatás--neve} {telepítési neve} telepítéshez, mert nincs hello regionális hatókörbe. Tekintse meg a központi telepítés tooregional hatókör áthelyezésére toohttp://aka.ms/regionalscope. | 2014 Azure bejelentette, hogy hálózati erőforrások áthelyezi egy fürt szintű hatókör tooregional hatókörből. További részletek: [http://aka.ms/regionalscope] (http://aka.ms/regionalscope). Ez a hiba történik, ha az áttelepítendő hello telepítési még nem volt a frissítési művelet, amely automatikusan áthelyezi azt tooa regionális hatókörbe. Legjobb megoldás, tooeither hozzáadása egy végpont tooa virtuális gép vagy egy adatok toohello VM lemezre, majd próbálkozzon újra az áttelepítési. <br> Lásd: [hogyan tooset be a klasszikus Azure-ban Windows rendszerű virtuális gép végpontja](../articles/virtual-machines/windows/classic/setup-endpoints.md#create-an-endpoint) vagy [adatok lemez tooa Windows létrehozott virtuális gépek hello klasszikus üzembe helyezési modellel csatolása](../articles/virtual-machines/windows/classic/attach-disk.md)|


## <a name="detailed-mitigations"></a>Részletes hibakezelés

### <a name="vm-with-data-disk-whose-physical-blob-size-bytes-does-not-match-hello-vm-data-disk-logical-size-bytes"></a>Adatlemez, amelynek fizikai blob mérete bájtban rendelkező virtuális gépet nem egyezik meg a hello VM adatlemez logikai mérete bájtban.

Ez akkor fordul elő, amikor hello adattároló lemezeinek logikai mérete kaphat nincs szinkronban a tényleges VHD blobméretével hello. Ezzel könnyen ellenőrizheti hello a következő parancsok használatával:

#### <a name="verifying-hello-issue"></a>Hello probléma ellenőrzése

```PowerShell
# Store hello VM details in hello VM object
$vm = Get-AzureVM -ServiceName $servicename -Name $vmname

# Display hello data disk properties
# NOTE hello data disk LogicalDiskSizeInGB below which is 11GB. Also note hello MediaLink Uri of hello VHD blob as we'll use this in hello next step
$vm.VM.DataVirtualHardDisks


HostCaching         : None
DiskLabel           : 
DiskName            : coreosvm-coreosvm-0-201611230636240687
Lun                 : 0
LogicalDiskSizeInGB : 11
MediaLink           : https://contosostorage.blob.core.windows.net/vhds/coreosvm-dd1.vhd
SourceMediaLink     : 
IOType              : Standard
ExtensionData       : 

# Now get hello properties of hello blob backing hello data disk above
# NOTE hello size of hello blob is about 15 GB which is different from LogicalDiskSizeInGB above
$blob = Get-AzureStorageblob -Blob "coreosvm-dd1.vhd" -Container vhds 

$blob

ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudPageBlob
BlobType          : PageBlob
Length            : 16106127872
ContentType       : application/octet-stream
LastModified      : 11/23/2016 7:16:22 AM +00:00
SnapshotTime      : 
ContinuationToken : 
Context           : Microsoft.WindowsAzure.Commands.Common.Storage.AzureStorageContext
Name              : coreosvm-dd1.vhd
```

#### <a name="mitigating-hello-issue"></a>Hello probléma orvoslása

```PowerShell
# Convert hello blob size in bytes tooGB into a variable which we'll use later
$newSize = [int]($blob.Length / 1GB)

# See hello calculated size in GB
$newSize

15

# Store hello disk name of hello data disk as we'll use this tooidentify hello disk toobe updated
$diskName = $vm.VM.DataVirtualHardDisks[0].DiskName

# Identify hello LUN of hello data disk tooremove
$lunToRemove = $vm.VM.DataVirtualHardDisks[0].Lun

# Now remove hello data disk from hello VM so that hello disk isn't leased by hello VM and it's size can be updated
Remove-AzureDataDisk -LUN $lunToRemove -VM $vm | Update-AzureVm -Name $vmname -ServiceName $servicename

OperationDescription OperationId                          OperationStatus
-------------------- -----------                          ---------------
Update-AzureVM       213xx1-b44b-1v6n-23gg-591f2a13cd16   Succeeded  

# Verify we have hello right disk that's going toobe updated
Get-AzureDisk -DiskName $diskName

AffinityGroup        : 
AttachedTo           : 
IsCorrupted          : False
Label                : 
Location             : East US
DiskSizeInGB         : 11
MediaLink            : https://contosostorage.blob.core.windows.net/vhds/coreosvm-dd1.vhd
DiskName             : coreosvm-coreosvm-0-201611230636240687
SourceImageName      : 
OS                   : 
IOType               : Standard
OperationDescription : Get-AzureDisk
OperationId          : 0c56a2b7-a325-123b-7043-74c27d5a61fd
OperationStatus      : Succeeded

# Now update hello disk toohello new size
Update-AzureDisk -DiskName $diskName -ResizedSizeInGB $newSize -Label $diskName

OperationDescription OperationId                          OperationStatus
-------------------- -----------                          ---------------
Update-AzureDisk     cv134b65-1b6n-8908-abuo-ce9e395ac3e7 Succeeded 

# Now verify that hello "DiskSizeInGB" property of hello disk matches hello size of hello blob 
Get-AzureDisk -DiskName $diskName


AffinityGroup        : 
AttachedTo           : 
IsCorrupted          : False
Label                : coreosvm-coreosvm-0-201611230636240687
Location             : East US
DiskSizeInGB         : 15
MediaLink            : https://contosostorage.blob.core.windows.net/vhds/coreosvm-dd1.vhd
DiskName             : coreosvm-coreosvm-0-201611230636240687
SourceImageName      : 
OS                   : 
IOType               : Standard
OperationDescription : Get-AzureDisk
OperationId          : 1v53bde5-cv56-5621-9078-16b9c8a0bad2
OperationStatus      : Succeeded

# Now we'll add hello disk back toohello VM as a data disk. First we need tooget an updated VM object
$vm = Get-AzureVM -ServiceName $servicename -Name $vmname

Add-AzureDataDisk -Import -DiskName $diskName -LUN 0 -VM $vm -HostCaching ReadWrite | Update-AzureVm -Name $vmname -ServiceName $servicename

OperationDescription OperationId                          OperationStatus
-------------------- -----------                          ---------------
Update-AzureVM       b0ad3d4c-4v68-45vb-xxc1-134fd010d0f8 Succeeded      
```

### <a name="moving-a-vm-tooa-different-subscription-after-completing-migration"></a>Helyezze át a virtuális gép tooa másik előfizetést az áttelepítés befejezése után

Hello áttelepítési folyamat befejezése után érdemes lehet toomove hello VM tooanother előfizetés. Azonban ha egy titkos kulcsot/tanúsítványa van virtuális Gépet, amely hivatkozik a Key Vault erőforrás hello áthelyezése hello jelenleg nem támogatott. alább utasításokat hello tooworkaround hello probléma lehetővé teszi. 

#### <a name="powershell"></a>PowerShell
```powershell
$vm = Get-AzureRmVM -ResourceGroupName "MyRG" -Name "MyVM"
Remove-AzureRmVMSecret -VM $vm
Update-AzureRmVM -ResourceGroupName "MyRG" -VM $vm
```
#### <a name="azure-cli-20"></a>Azure CLI 2.0

```bash
az vm update -g "myrg" -n "myvm" --set osProfile.Secrets=[]
```
