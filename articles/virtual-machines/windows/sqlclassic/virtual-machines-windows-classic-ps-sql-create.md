---
title: "egy SQL Server virtuális gép (klasszikus) Azure PowerShell aaaCreate |} Microsoft Docs"
description: "Lépéseket és a PowerShell-parancsfájlok biztosít az Azure virtuális gép létrehozása az SQL Server virtuális gép a gyűjtemény lemezképei. Ez a témakör hello klasszikus üzembe helyezési módot használ."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
tags: azure-service-management
ms.assetid: b73be387-9323-4e08-be53-6e5928e3786e
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/07/2017
ms.author: jroth
ms.openlocfilehash: b14d5d9bc192fa0a21126395ee20ffd06b3bf47d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-classic"></a>Egy SQL Server rendszerű virtuális gép (klasszikus) Azure PowerShell használatával

Ez a cikk a hogyan toocreate SQL Server virtuális gépen az Azure használatával hello PowerShell-parancsmagok lépéseit ismerteti.

> [!IMPORTANT] 
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../azure-resource-manager/resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.

Ez a témakör hello erőforrás-kezelő verziója: [egy SQL Server rendszerű virtuális gép Azure PowerShell Resource Manager használatával](../sql/virtual-machines-windows-ps-sql-create.md).

### <a name="install-and-configure-powershell"></a>PowerShell telepítése és konfigurálása:
1. Ha nem rendelkezik Azure-fiókkal, az [Azure ingyenes próbát](https://azure.microsoft.com/pricing/free-trial/) biztosít.
2. [Töltse le és telepítse a legújabb Azure PowerShell-parancsok hello](/powershell/azure/overview).
3. Indítsa el a Windows Powershellt, valamint elérheti az Azure-előfizetés tooyour hello **Add-AzureAccount** parancsot.

   ```powershell
   Add-AzureAccount
   ```

## <a name="determine-your-target-azure-region"></a>Határozza meg a cél Azure-régió

Az SQL Server virtuális gépen egy felhőalapú szolgáltatás, amely egy adott Azure-régióban található fog üzemelni. hello következő lépések segítségével meg toodetermine a régió, a tárfiók és a felhőszolgáltatás, amely jelzi a hello rest hello oktatóanyag.

1. Határozza meg, amelyet toouse toohost az SQL Server virtuális gép hello adatközpont. a következő PowerShell-paranccsal hello rendelkezésre álló terület neveinek listáját jeleníti meg.

   ```powershell
   (Get-AzureLocation).Name
   ```

2. Miután az elsődleges hely állapította meg, állítsa be a nevű változó **$dcLocation** toothat régióban. Például a készlet túl hello régió parancs a következő hello "USA keleti régiója":

   ```powershell
   $dcLocation = "East US"
   ```

## <a name="set-your-subscription-and-storage-account"></a>Az előfizetés és a tárolási fiók beállítása

1. Határozza meg, hogy hello Azure-előfizetés hello új virtuális gép használja.

   ```powershell
   (Get-AzureSubscription).SubscriptionName
   ```

2. Rendelje hozzá a cél Azure-előfizetés toohello **$subscr** változó. Ezután állítsa be a jelenlegi Azure-előfizetése.

   ```powershell
   $subscr="<subscription name>"
   Select-AzureSubscription -SubscriptionName $subscr –Current
   ```

3. Ezután ellenőrizze a meglévő tárfiókok. hello következő parancsfájlja felsorolja az összes tárfiók, amely szerepel a kiválasztott régióban:

   ```powershell
   (Get-AzureStorageAccount | where { $_.GeoPrimaryLocation -eq $dcLocation }).StorageAccountName
   ```

   > [!NOTE]
   > Új tárfiók szükséges, ha először létre kell hoznia egy kisbetű minden tárfiók neve a New-AzureStorageAccount hello paranccsal beállítani, mint például a következő hello:`New-AzureStorageAccount -StorageAccountName "<storage account name>" -Location $dcLocation`

4. Rendelje hozzá a hello cél tárolási fiók neve toohello **$staccount**. Ezután **Set-AzureSubscription** tooset hello előfizetés és az aktuális tárfiók.

   ```powershell
   $staccount="<storage account name>"
   Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount
   ```

## <a name="select-a-sql-server-virtual-machine-image"></a>Egy SQL Server virtuálisgép-lemezkép kiválasztása

1. Ismerje meg az elérhető SQL Server virtuális gépek rendszerkép hello gyűjteményből hello listája. Ezeket a lemezképeket mindegyik rendelkezik egy **ImageFamily** "SQL" kezdetű tulajdonság. hello következő jeleníti meg hello kép termékcsalád elérhető tooyou előre telepített SQL Server rendelkező lekérdezés.

   ```powershell
   Get-AzureVMImage | where { $_.ImageFamily -like "SQL*" } | select ImageFamily -Unique | Sort-Object -Property ImageFamily
   ```

2. Ha megtalálta hello virtuálisgép-lemezkép termékcsalád, lehet több közzétett lemezképet a családban. Használjon hello alábbi parancsfájl toofind hello legújabb közzétett virtuális géphez tartozó kép neve a kiválasztott kép családba (például **SQL Server 2016 Windows Server 2012 R2 RTM Enterprise**):

   ```powershell
   $family="<ImageFamily value>"
   $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

   echo "Selected SQL Server image name:"
   echo "   $image"
   ```

## <a name="create-hello-virtual-machine"></a>Hello virtuális gép létrehozása

Végezetül hozza létre a hello virtuális gépet, a PowerShell használatával:

1. Hozzon létre egy felhőalapú szolgáltatás toohost hello új virtuális Gépet. Vegye figyelembe, hogy az is lehetséges toouse egy meglévő felhőszolgáltatáshoz helyette. Hozzon létre egy új változót **$svcname** hello rövid névvel hello felhőalapú szolgáltatás.

   ```powershell
   $svcname = "<cloud service name>"
   New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation
   ```

2. Adja meg a hello virtuális gép nevét és méretét. További információ a virtuálisgép-méretek: [Azure virtuálisgép-méretek](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

   ```powershell
   $vmname="<machine name>"
   $vmsize="<Specify one: Large, ExtraLarge, A5, A6, A7, A8, A9, or see hello link toohello other VM sizes>"
   $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image
   ```

3. Adja meg a hello helyi rendszergazda fiókot és jelszót.

   ```powershell
   $cred=Get-Credential -Message "Type hello name and password of hello local administrator account."
   $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password
   ```

4. Futtassa a következő parancsfájl toocreate hello virtuális gép hello.

   ```powershell
   New-AzureVM –ServiceName $svcname -VMs $vm1
   ```

> [!NOTE]
> További magyarázat és konfigurációs beállítások: hello **összeállítása a parancskészlethez** szakasz [használata Azure PowerShell toocreate és előre konfigurálása a Windows-alapú virtuális gépek](../classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="example-powershell-script"></a>Példa PowerShell-parancsfájl

hello következő parancsfájl egy példát a teljes parancsfájl, amely létrehoz egy **SQL Server 2016 Windows Server 2012 R2 RTM Enterprise** virtuális gépet. Ha ezt a parancsfájlt használja, akkor testre kell szabnia hello kezdeti változók hello előző témakörben leírt lépések alapján.

```powershell
# Customize these variables based on your settings and requirements:
$dcLocation = "East US"
$subscr="mysubscription"
$staccount="mystorageaccount"
$family="SQL Server 2016 RTM Enterprise on Windows Server 2012 R2"
$svcname = "mycloudservice"
$vmname="myvirtualmachine"
$vmsize="A5"

# Set hello current subscription and storage account
# Comment out hello New-AzureStorageAccount line if hello account already exists
Select-AzureSubscription -SubscriptionName $subscr –Current
New-AzureStorageAccount -StorageAccountName $staccount -Location $dcLocation
Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

# Select hello most recent VM image in this image family:
$image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

# Create hello new cloud service; comment out this line if cloud service exists already:
New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation

# Create hello VM config:
$vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

# Set administrator credentials:
$cred=Get-Credential -Message "Type hello name and password of hello local administrator account."
$vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

# Create hello SQL Server VM:
New-AzureVM –ServiceName $svcname -VMs $vm1
```

## <a name="connect-with-remote-desktop"></a>Csatlakozzon a távoli asztal

1. Hozzon létre hello RDP-fájlok hello aktuális felhasználó Dokumentumok mappa toolaunch ezen virtuális gépek toocomplete beállítása:

   ```powershell
   $documentspath = [environment]::getfolderpath("mydocuments")
   Get-AzureRemoteDesktopFile -ServiceName $svcname -Name $vmname -LocalPath "$documentspath\vm1.rdp"
   ```

2. Hello Dokumentumok mappájába indítsa el a hello RDP-fájlt. Csatlakozás hello rendszergazda felhasználónevet és jelszót adott meg a korábban (például ha a felhasználónév VMAdmin, adja meg a "\VMAdmin" hello felhasználói és hello jelszót).

   ```powershell
   cd $documentspath
   .\vm1.rdp
   ```

## <a name="complete-hello-configuration-of-hello-sql-server-machine-for-remote-access"></a>SQL Server számítógép a távoli hozzáféréshez hello teljes hello konfigurálása

A bejelentkezés után a toohello számítógép a távoli asztal konfigurálása hello utasításait SQL Serveren [egy Azure virtuális gép az SQL Server-kapcsolat beállításának lépései](virtual-machines-windows-classic-sql-connect.md#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

## <a name="next-steps"></a>Következő lépések

További utasítások hello a PowerShell használatával a virtuális gépek rendszerbe állításához található [virtual machines – dokumentáció](../classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Sok esetben hello következő lépésre toomigrate az adatbázisok toothis új SQL Server virtuális gép. Adatbázis-áttelepítési útmutatóért lásd: [egy adatbázis tooSQL kiszolgálót egy Azure virtuális gép áttelepítése](../sql/virtual-machines-windows-migrate-sql.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).

Ha is szeretné használni az Azure portál toocreate SQL virtuális gépek hello, lásd: [Azure SQL Server virtuális gépek kiépítése](../sql/virtual-machines-windows-portal-sql-server-provision.md). Vegye figyelembe, hogy, hogy ajánlott Resource Manager modellt, nem pedig hello klasszikus modellt használja a PowerShell témakör bemutatja, hogyan hello portálon keresztül akkor hoz létre a virtuális gépek hello hello oktatóanyag.

Ezenkívül toothese erőforrások, javasoljuk, hogy tekintse át [más témakörök kapcsolódó SQL Server Azure virtuális gépek toorunning](../sql/virtual-machines-windows-sql-server-iaas-overview.md).
