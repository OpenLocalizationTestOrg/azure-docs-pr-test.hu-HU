## <a name="overview"></a>Áttekintés
Amikor hoz létre egy új virtuális gép (VM) erőforráscsoportban lemezképek [Azure piactér](https://azure.microsoft.com/marketplace/), hello alapértelmezett operációsrendszer-meghajtón 127 GB. Annak ellenére, hogy lehetséges tooadd adatok lemezek toohello (hello SKU számától függően a választott) virtuális gép, és továbbá ajánlott tooinstall alkalmazások és a CPU-igényes munkaterhelések ezeket a kiegészítő lemezeken, gyakran kell tooexpand hello az operációs rendszer meghajtó toosupport bizonyos forgatókönyvekben, például a következőket:

1. Az operációs rendszer meghajtójára összetevőket telepítő, régebbi alkalmazások támogatása.
2. Nagyobb operációsrendszer-meghajtóval rendelkező fizikai számítógép vagy virtuális gép migrálása a helyszínről.

> [!IMPORTANT]
> Az Azure két különböző üzemi modellel rendelkezik az erőforrások létrehozásához és használatához: Resource Manager-alapú és klasszikus. Ez a cikk ismerteti a hello Resource Manager modellt használja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.
> 
> 

## <a name="resize-hello-os-drive"></a>Az operációs rendszer hello meghajtó átméretezése
Ebben a cikkben azt fogja a feladatnak hello a resource manager modulja segítségével az operációs rendszer hello meghajtó átméretezése [Azure Powershell](/powershell/azureps-cmdlets-docs). Felügyeleti üzemmódban megnyitott a Powershell ISE vagy a Powershell-ablakot, és kövesse az alábbi hello lépéseket:

1. Bejelentkezési tooyour Microsoft Azure erőforrás-kezelő módban a fiókot, és jelölje ki az előfizetését a következőképpen:
   
   ```Powershell
   Login-AzureRmAccount
   Select-AzureRmSubscription –SubscriptionName 'my-subscription-name'
   ```
2. Állítsa az erőforráscsoport és a virtuális gép nevét a következőre:
   
   ```Powershell
   $rgName = 'my-resource-group-name'
   $vmName = 'my-vm-name'
   ```
3. Szerezzen be egy hivatkozási tooyour VM az alábbiak szerint:
   
   ```Powershell
   $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```
4. Hello VM leállítása előtt hello lemez átméretezése az alábbiak szerint:
   
    ```Powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    ```
5. Tesztelhet, és itt azt vár a hello jelenleg! Állítsa be a hello az operációs rendszer szükséges toohello érték hello méretét, és hello VM frissítése az alábbiak szerint:
   
   ```Powershell
   $vm.StorageProfile.OSDisk.DiskSizeGB = 1023
   Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
   ```
   
   > [!WARNING]
   > hello új méret hello meglévő lemez méretének nagyobbnak kell lennie. hello maximális engedélyezett 1023 GB-os.
   > 
   > 
6. Virtuális gép frissítése hello néhány másodpercet vehet igénybe. Hello parancs futtatásának befejeztével végrehajtása után indítsa újra a virtuális gép hello az alábbiak szerint:
   
   ```Powershell
   Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```

Készen is van. Most a virtuális gép, hello RDP nyissa meg a számítógép-kezelés (vagy a Lemezkezelés eszköz), és bontsa ki az újonnan lefoglalt terület hello segítségével hello meghajtó.

## <a name="summary"></a>Összefoglalás
Ebben a cikkben Powershell tooexpand hello az operációs rendszer meghajtóját a IaaS virtuális gépként az Azure Resource Manager modulok használtuk. Hello referenciaként a teljes parancsfájl másolható alatt van:

```Powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName 'my-subscription-name'
$rgName = 'my-resource-group-name'
$vmName = 'my-vm-name'
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
$vm.StorageProfile.OSDisk.DiskSizeGB = 1023
Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
```

## <a name="next-steps"></a>Következő lépések
Bár ebben a cikkben azt összpontosított elsősorban a virtuális gép hello hello OS lemez bővítése, hello fejlett parancsfájl is felhasználhatók a bővített hello adatok lemezek csatolt toohello VM egyetlen kódsort módosításával. Például tooexpand hello első adatok lemezre csatlakoztatott toohello VM, cserélje le a hello ```OSDisk``` objektum ```StorageProfile``` rendelkező ```DataDisks``` tömb, és egy numerikus index tooobtain hivatkozás toofirst csatolt adatlemezt, használja az alább látható módon:

```Powershell
$vm.StorageProfile.DataDisks[0].DiskSizeGB = 1023
```
Hasonló módon lehet, hogy más adatok lemezek csatolt toohello index használatával, ahogy fent látható, a virtuális gép hivatkozik, vagy hello ```Name``` tulajdonság hello lemez, az alábbi ábra szemlélteti:

```Powershell
($vm.StorageProfile.DataDisks | Where {$_.Name -eq 'my-second-data-disk'})[0].DiskSizeGB = 1023
```

Ha azt szeretné, hogy ki hogyan tooattach lemezek tooan Azure Resource Manager virtuális gép toofind, ellenőrizze a [cikk](../articles/virtual-machines/windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

