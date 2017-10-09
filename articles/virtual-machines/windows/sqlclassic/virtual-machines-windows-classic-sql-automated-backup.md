---
title: "biztonsági mentés SQL Server virtuális gépek (klasszikus) aaaAutomated |} Microsoft Docs"
description: "Ismerteti a hello automatikus biztonsági mentés szolgáltatás az SQL Server fut, az Azure virtuális gépek erőforrás-kezelő használatával. "
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 3333e830-8a60-42f5-9f44-8e02e9868d7b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/05/2017
ms.author: jroth
ms.openlocfilehash: 5d8f0412578c2d86edc6e54093a5da4891d3847e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automated-backup-for-sql-server-in-azure-virtual-machines-classic"></a>Automatikus biztonsági mentés az SQL Server Azure virtuális gépekben (klasszikus)
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-sql-automated-backup.md)
> * [Klasszikus](../classic/sql-automated-backup.md)
> 
> 

Automatikus biztonsági mentés automatikusan konfigurálja a [való felügyelt biztonsági mentésének tooMicrosoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) az összes meglévő és új adatbázis egy SQL Server 2014 Standard vagy Enterprise rendszert futtató Azure virtuális gépen. Ez lehetővé teszi tooconfigure rendszeres biztonsági tartós Azure blob-tárolók. Automatikus biztonsági mentés függ hello [SQL Server infrastruktúra-szolgáltatási ügynök bővítmény](../classic/sql-server-agent-extension.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

> [!IMPORTANT] 
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../azure-resource-manager/resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. tooview hello erőforrás-kezelő változata ebből a cikkből: [automatikus biztonsági mentés az SQL Server az Azure virtuális gépek erőforrás-kezelő](../sql/virtual-machines-windows-sql-automated-backup.md).

## <a name="prerequisites"></a>Előfeltételek
toouse automatikus biztonsági mentés, vegye figyelembe a következő előfeltételek hello:

**Operációs rendszer**:

* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

**SQL Server verziójához/kiadásához**:

* Az SQL Server 2014 Standard
* SQL Server 2014 Enterprise

> [!NOTE]
> SQL Server 2016 jelenleg nem támogatott az automatikus biztonsági mentés.
> 
> 

**Adatbázis-konfiguráció**:

* Cél adatbázisok hello teljes helyreállítási modell kell megadni.

**Az Azure PowerShell**:

* [Telepítse a legújabb Azure PowerShell-parancsok hello](/powershell/azure/overview).

**SQL Server IaaS bővítmény**:

* [Telepítse az SQL Server IaaS bővítmény hello](../classic/sql-server-agent-extension.md).

## <a name="settings"></a>Beállítások
hello következő táblázat ismerteti, amelyek az automatikus biztonsági mentés hello-beállítások. Klasszikus virtuális gépekhez PowerShell tooconfigure ezeket a beállításokat kell használnia.

| Beállítás | Tartomány (alapértelmezett) | Leírás |
| --- | --- | --- |
| **Automatikus biztonsági mentés** |Engedélyezi/letiltja (letiltva) |Engedélyezi vagy letiltja az automatikus biztonsági mentés az SQL Server 2014 Standard vagy Enterprise rendszert futtató Azure virtuális gép esetében. |
| **Megőrzési időtartam** |1-30 nap (30 nap) |nap tooretain biztonsági hello száma. |
| **Tárfiók** |Azure storage-fiók (hello létrehozott tárfiókban hello a megadott virtuális gép) |Egy Azure storage fiók toouse automatikus biztonsági mentés fájlok tárolásához a blob Storage tárolóban. Egy tároló összes biztonságimásolat-fájl jön létre a hely toostore. hello biztonságimásolat-fájl elnevezési konvenciója hello dátum, idő és a gép neve tartalmazza. |
| **Titkosítás** |Engedélyezi/letiltja (letiltva) |Engedélyezi vagy letiltja a titkosítást. Ha engedélyezve van a titkosítás, hello tanúsítványok használt toorestore hello biztonsági mentés található hello megadott tárfiók hello a azonos automaticbackup tárolót használó hello azonos elnevezési konvenciót. Hello jelszó is módosul, ha új tanúsítványt hoz létre, hogy a jelszó, de a régi tanúsítvány hello marad toorestore korábbi biztonsági másolatok. |
| **Jelszó** |Jelszó szöveg (nincs) |A titkosítási kulcsok jelszava. Erre csak akkor van szükség, ha engedélyezve van-e a titkosítás. A sorrend toorestore egy biztonsági másolat titkosításának engedélyezése, rendelkeznie kell hello helyes jelszót és a kapcsolódó hello a biztonsági mentés készült hello során használt tanúsítvány. | **Biztonsági mentési rendszer adatbázisok** | Engedélyezi/letiltja (letiltva) | A Master, Model és MSDB teljes biztonsági másolatok készítése |
| **Konfigurálja a biztonsági mentés ütemezése** | Manuális vagy automatikus (automatikus) | Válassza ki **automatikus** tooautomatically teljes igénybe, valamint naplófájl-biztonsági mentések napló növekedési alapján. Válassza ki **manuális** teljes toospecify hello ütemezését, valamint naplófájl-biztonsági mentések. |

## <a name="configuration-with-powershell"></a>PowerShell-konfiguráció
A következő PowerShell-példa hello, az automatikus biztonsági mentés meglévő SQL Server 2014 virtuális van konfigurálva. Hello **New-AzureVMSqlServerAutoBackupConfig** parancs hello automatikus biztonsági mentés beállításait toostore biztonsági mentések konfigurálása hello $storageaccount változó által megadott hello Azure storage-fiók. Ezek a biztonsági másolatok 10 napig lesznek megőrizve. Hello **Set-AzureVMSqlServerExtension** frissítések hello Azure virtuális Géphez megadott ezekkel a beállításokkal.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

Nem sikerült eltarthat néhány percig tooinstall és hello SQL Server IaaS-ügynök konfigurálása.

tooenable titkosítási hello CertificatePassword paraméter hello előző parancsfájl toopass hello EnableEncryption paraméter és egy jelszó (biztonságos karakterlánc) módosítása. hello következő parancsfájl lehetővé teszi, hogy az előző példában hello hello automatikus biztonsági mentés beállításait, és hozzáadja titkosítás.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $password = "P@ssw0rd"
    $encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10 -EnableEncryption -CertificatePassword $encryptionpassword

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

toodisable automatikus biztonsági mentés, azonos hello nélkül parancsfájl futtatási hello **-engedélyezése** paraméter toohello **New-AzureVMSqlServerAutoBackupConfig**. Csakúgy, mint a telepítés több percet toodisable automatikus biztonsági mentés is igénybe vehet.

> [!NOTE]
> Letiltásával és hello SQL Server IaaS-ügynök eltávolítása nem távolítja el korábban konfigurált hello való felügyelt biztonsági mentésének beállításait. Automatikus biztonsági mentés letiltása, vagy hello SQL Server IaaS-ügynök eltávolítása előtt tiltsa le.
> 
> 

## <a name="next-steps"></a>Következő lépések
Automatikus biztonsági mentés való felügyelt biztonsági mentésének konfigurálása Azure virtuális gépeken. Ezért fontos túl[tekintse meg a felügyelt biztonsági mentéshez hello dokumentációt](https://msdn.microsoft.com/library/dn449496.aspx) toounderstand hello viselkedést és megvalósítását.

További biztonsági mentés található, és állítsa vissza a következő témakör hello útmutatást az SQL Server Azure virtuális gépeken: [biztonsági mentése és visszaállítása az SQL Server Azure virtuális gépek](../sql/virtual-machines-windows-sql-backup-recovery.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).

Más elérhető automation feladatokkal kapcsolatos további információkért lásd: [SQL Server infrastruktúra-szolgáltatási ügynök bővítmény](../classic/sql-server-agent-extension.md).

További információ az Azure virtuális gépeken futó SQL Server: [SQL Server Azure virtuális gépek – áttekintés](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

