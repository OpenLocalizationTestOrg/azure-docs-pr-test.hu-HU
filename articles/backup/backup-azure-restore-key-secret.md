---
title: "aaaRestore Key Vault kulcs és a titkos kulcs titkosított virtuális gépek Azure Backup használatával |} Microsoft Docs"
description: "Megtudhatja, hogyan toorestore kulcsot tároló kulcsot és titkos kulcsot az Azure Backup szolgáltatáshoz a PowerShell használatával"
services: backup
documentationcenter: 
author: JPallavi
manager: vijayts
editor: 
ms.assetid: 45214083-d5fc-4eb3-a367-0239dc59e0f6
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: pajosh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 659b2f647493ffcc9494caee111c8bd584ef9c7f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="restore-key-vault-key-and-secret-for-encrypted-vms-using-azure-backup"></a>Állítsa vissza a Key Vault kulcs és a titkos kulcs titkosított virtuális gépek Azure Backup segítségével
Ez a cikk beszél Azure virtuális gép biztonsági mentése tooperform visszaállítással titkosított Azure virtuális gépek, ha a kulcs és a titkos kulcs nem létezik a kulcstároló hello. Ezeket a lépéseket is használható, ha azt szeretné, hogy egy külön példányát (kulcs titkosítási kulcsot) toomaintain és titkos kulcsát (a BitLocker titkosítási kulcs) hello visszaállítani a virtuális gép.

## <a name="prerequisites"></a>Előfeltételek
* **Biztonsági másolat a virtuális gépek titkosítva** - titkosított Azure virtuális gépek mentett Azure Backup segítségével. Tekintse meg a hello cikk [biztonsági mentéséhez és visszaállításához a PowerShell használata Azure virtuális gépek kezelése](backup-azure-vms-automation.md) hogyan toobackup titkosított Azure virtuális gépek vonatkozó további információért.
* **Konfigurálja az Azure Key Vault** – győződjön meg arról, hogy kulcstároló toowhich kulcsok és titkos kulcsok kell toobe visszaállítása már létezik. Tekintse meg a hello cikk [Ismerkedés az Azure Key Vault](../key-vault/key-vault-get-started.md) kulcstároló-kezeléssel kapcsolatos részletekért.
* **Állítsa vissza a lemez** -győződjön meg arról, hogy Ön léptetnie visszaállítási feladat lemezeket a használatával titkosított virtuális gép visszaállítására [PowerShell lépéseket](backup-azure-vms-automation.md#restore-an-azure-vm). Ennek oka az, a feladat a kulcsok és titkos Titkosított hello VM toobe visszaállítani a tartalmazó tárfiókot hoz létre egy JSON-fájl.

## <a name="get-key-and-secret-from-azure-backup"></a>Az Azure Backup kulcs és a titkos kulcs beszerzése

> [!NOTE]
> Ha a lemez vissza lett állítva a hello titkosított virtuális gép, ügyeljen arra, hogy:
> 1. $details feltöltődik a visszaállítási lemez feladat részleteit, ahogyan az [PowerShell visszaállítási hello lemezek szakasz lépései](backup-azure-vms-automation.md#restore-an-azure-vm)
> 2. Virtuális gép kell létrehozni, csak a visszaállított lemezekből **után és a titkos kulcs visszaállított tookey tároló**.
>
>

Lekérdezés hello lemez a részleteket a Tulajdonságok hello feladat visszaállítva.

```
PS C:\> $properties = $details.properties
PS C:\> $storageAccountName = $properties["Target Storage Account Name"]
PS C:\> $containerName = $properties["Config Blob Container Name"]
PS C:\> $encryptedBlobName = $properties["Encryption Info Blob Name"]
```

Az Azure storage-környezet hello beállítása, és állítsa vissza a kulcsot és titkos adatait tartalmazó titkosított virtuális gép JSON-konfigurációs fájlt.

```
PS C:\> Set-AzureRmCurrentStorageAccount -Name $storageaccountname -ResourceGroupName '<rg-name>'
PS C:\> $destination_path = 'C:\vmencryption_config.json'
PS C:\> Get-AzureStorageBlobContent -Blob $encryptedBlobName -Container $containerName -Destination $destination_path
PS C:\> $encryptionObject = Get-Content -Path $destination_path  | ConvertFrom-Json
```

## <a name="restore-key"></a>Kulcs helyreállítása
A fent említett hello célútvonalat hello JSON-fájl jön létre, ha hello JSON hozza létre kulcs fájlját, és újra a hello kulcstároló toorestore kulcs parancsmag tooput hello kulcs-(KEK) az adatcsatorna.

```
PS C:\> $keyDestination = 'C:\keyDetails.blob'
PS C:\> [io.file]::WriteAllBytes($keyDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyBackupData))
PS C:\> Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile $keyDestination
```

## <a name="restore-secret"></a>Titkos kulcs visszaállítása
Használata hello JSON-fájl generált fent tooget titkos nevét és értékét, és a hírcsatorna azt újra a hello kulcstároló tooset titkos parancsmag tooput hello titkos (BEK). **Ezeket a parancsmagokat használja, ha a virtuális gép titkosítása BEK és KEK.**

```
PS C:\> $secretdata = $encryptionObject.OsDiskKeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
PS C:\> $secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = $encryptionObject.OsDiskKeyAndSecretDetails.KeyUrl;'MachineName' = 'vm-name'}
PS C:\> Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $Secret -ContentType  'Wrapped BEK' -Tags $Tags
```

Ha a virtuális gép **BEK csak használatával titkosított**, titkos blob-fájl generálása hello JSON és a hírcsatorna azt újra a hello kulcstároló toorestore titkos parancsmag tooput hello titkos (BEK).

```
PS C:\> $secretDestination = 'C:\secret.blob'
PS C:\> [io.file]::WriteAllBytes($secretDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyVaultSecretBackupData))
PS C:\> Restore-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -InputFile $secretDestination -Verbose
```

> [!NOTE]
> 1. $Secretname értéke kérhetők le utaló $encryptionObject.OsDiskKeyAndSecretDetails.SecretUrl toohello kimenete és titkos kulcsok után pedig SMS használatával / pl. kimeneti titkos URL-cím https://keyvaultname.vault.azure.net/secrets/ B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 és titkos neve B3284AAA-DAAA-4AAA-B393-60CAA848AAAA:
> 2. Hello címke DiskEncryptionKeyFileName értéke ugyanaz, mint a titkos nevét.
>
>

## <a name="create-virtual-machine-from-restored-disk"></a>Virtuális gép létrehozása a visszaállított lemezről
Ha készített biztonsági másolatot titkosított virtuális gép Azure virtuális gép biztonsági mentéssel, hello PowerShell-parancsmagok fent említett súgó állítsa vissza a kulcsot és titkos hátsó toohello kulcstároló. Után a visszaállítás, tekintse meg a hello cikk [biztonsági mentéséhez és visszaállításához a PowerShell használata Azure virtuális gépek kezelése](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) toocreate titkosított virtuális gépek a visszaállított lemez, a kulcs és a titkos kulcsot.

## <a name="legacy-approach"></a>A hagyományos megközelítés
a fent említett hello megközelítés minden hello helyreállítási pontot csatlakoztatás működik. Azonban hello régebbi módszert is, amely kulcsot és titkos információk lekérése a helyreállítási pont lesz érvényes a 2017. július 11. BEK és KEK használatával titkosított virtuális gépek régebbi helyreállítási pontok. A visszaállítási lemez feladatot titkosított virtuális gép használatának befejezése után [PowerShell lépéseket](backup-azure-vms-automation.md#restore-an-azure-vm), győződjön meg arról, hogy $rp érvényes értéket a telepítéskor.

### <a name="restore-key"></a>Kulcs helyreállítása
Használja a következő parancsmagok tooget kulcs-(KEK) információ helyreállítási pontból hello és hírcsatorna azt toorestore kulcs parancsmag tooput hello a key vaultban vissza.

```
PS C:\> $rp1 = Get-AzureRmRecoveryServicesBackupRecoveryPoint -RecoveryPointId $rp[0].RecoveryPointId -Item $backupItem -KeyFileDownloadLocation 'C:\Users\downloads'
PS C:\> Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile 'C:\Users\downloads'
```

### <a name="restore-secret"></a>Titkos kulcs visszaállítása
Használja a következő parancsmagok tooget titkos (BEK) információ helyreállítási pontból hello és hírcsatorna azt tooset titkos parancsmag tooput hello a key vaultban vissza.

```
PS C:\> $secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
PS C:\> $secretdata = $rp1.KeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = 'https://mykeyvault.vault.azure.net:443/keys/KeyName/84daaac999949999030bf99aaa5a9f9';'MachineName' = 'vm-name'}
PS C:\> Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $secret -Tags $Tags -SecretValue $Secret -ContentType  'Wrapped BEK'
```

> [!NOTE]
> 1. $Secretname értéke $rp1 toohello kimenete hivatkozással érhető el. KeyAndSecretDetails.SecretUrl és titkos kulcsok után pedig SMS használatával / pl. kimeneti titkos URL-cím https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 és titkos neve B3284AAA-DAAA-4AAA-B393-60CAA848AAAA
> 2. Hello címke DiskEncryptionKeyFileName értéke ugyanaz, mint a titkos nevét.
> 3. Kulcstároló DiskEncryptionKeyEncryptionKeyURL értéke beszerezhető hello kulcsok vissza visszaállítása és használata után [Get-AzureKeyVaultKey](https://msdn.microsoft.com/library/dn868053.aspx) parancsmag
>
>

## <a name="next-steps"></a>Következő lépések
Kulcs és a titkos hátsó tookey tároló visszaállítása, után tekintse meg a hello cikk [biztonsági mentéséhez és visszaállításához a PowerShell használata Azure virtuális gépek kezelése](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) toocreate titkosított virtuális gépek a visszaállított lemez, a kulcs és a titkos kulcsot.
