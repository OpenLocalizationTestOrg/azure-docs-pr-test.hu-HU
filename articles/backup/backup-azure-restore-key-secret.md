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
# <a name="restore-key-vault-key-and-secret-for-encrypted-vms-using-azure-backup"></a><span data-ttu-id="25d54-103">Állítsa vissza a Key Vault kulcs és a titkos kulcs titkosított virtuális gépek Azure Backup segítségével</span><span class="sxs-lookup"><span data-stu-id="25d54-103">Restore Key Vault key and secret for encrypted VMs using Azure Backup</span></span>
<span data-ttu-id="25d54-104">Ez a cikk beszél Azure virtuális gép biztonsági mentése tooperform visszaállítással titkosított Azure virtuális gépek, ha a kulcs és a titkos kulcs nem létezik a kulcstároló hello.</span><span class="sxs-lookup"><span data-stu-id="25d54-104">This article talks about using Azure VM Backup tooperform restore of encrypted Azure VMs, if your key and secret do not exist in hello key vault.</span></span> <span data-ttu-id="25d54-105">Ezeket a lépéseket is használható, ha azt szeretné, hogy egy külön példányát (kulcs titkosítási kulcsot) toomaintain és titkos kulcsát (a BitLocker titkosítási kulcs) hello visszaállítani a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="25d54-105">These steps can also be used if you want toomaintain a separate copy of key (Key Encryption Key) and secret (BitLocker Encryption Key) for hello restored VM.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="25d54-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="25d54-106">Prerequisites</span></span>
* <span data-ttu-id="25d54-107">**Biztonsági másolat a virtuális gépek titkosítva** - titkosított Azure virtuális gépek mentett Azure Backup segítségével.</span><span class="sxs-lookup"><span data-stu-id="25d54-107">**Backup encrypted VMs** - Encrypted Azure VMs have been backed up using Azure Backup.</span></span> <span data-ttu-id="25d54-108">Tekintse meg a hello cikk [biztonsági mentéséhez és visszaállításához a PowerShell használata Azure virtuális gépek kezelése](backup-azure-vms-automation.md) hogyan toobackup titkosított Azure virtuális gépek vonatkozó további információért.</span><span class="sxs-lookup"><span data-stu-id="25d54-108">Refer hello article [Manage backup and restore of Azure VMs using PowerShell](backup-azure-vms-automation.md) for details about how toobackup encrypted Azure VMs.</span></span>
* <span data-ttu-id="25d54-109">**Konfigurálja az Azure Key Vault** – győződjön meg arról, hogy kulcstároló toowhich kulcsok és titkos kulcsok kell toobe visszaállítása már létezik.</span><span class="sxs-lookup"><span data-stu-id="25d54-109">**Configure Azure Key Vault** – Ensure that key vault toowhich keys and secrets need toobe restored is already present.</span></span> <span data-ttu-id="25d54-110">Tekintse meg a hello cikk [Ismerkedés az Azure Key Vault](../key-vault/key-vault-get-started.md) kulcstároló-kezeléssel kapcsolatos részletekért.</span><span class="sxs-lookup"><span data-stu-id="25d54-110">Refer hello article [Get Started with Azure Key Vault](../key-vault/key-vault-get-started.md) for details about key vault management.</span></span>
* <span data-ttu-id="25d54-111">**Állítsa vissza a lemez** -győződjön meg arról, hogy Ön léptetnie visszaállítási feladat lemezeket a használatával titkosított virtuális gép visszaállítására [PowerShell lépéseket](backup-azure-vms-automation.md#restore-an-azure-vm).</span><span class="sxs-lookup"><span data-stu-id="25d54-111">**Restore disk** - Ensure that you have triggered restore job for restoring disks for encrypted VM using [PowerShell steps](backup-azure-vms-automation.md#restore-an-azure-vm).</span></span> <span data-ttu-id="25d54-112">Ennek oka az, a feladat a kulcsok és titkos Titkosított hello VM toobe visszaállítani a tartalmazó tárfiókot hoz létre egy JSON-fájl.</span><span class="sxs-lookup"><span data-stu-id="25d54-112">This is because this job generates a JSON file in your storage account containing keys and secrets for hello encrypted VM toobe restored.</span></span>

## <a name="get-key-and-secret-from-azure-backup"></a><span data-ttu-id="25d54-113">Az Azure Backup kulcs és a titkos kulcs beszerzése</span><span class="sxs-lookup"><span data-stu-id="25d54-113">Get key and secret from Azure Backup</span></span>

> [!NOTE]
> <span data-ttu-id="25d54-114">Ha a lemez vissza lett állítva a hello titkosított virtuális gép, ügyeljen arra, hogy:</span><span class="sxs-lookup"><span data-stu-id="25d54-114">Once disk has been restored for hello encrypted VM, ensure that:</span></span>
> 1. <span data-ttu-id="25d54-115">$details feltöltődik a visszaállítási lemez feladat részleteit, ahogyan az [PowerShell visszaállítási hello lemezek szakasz lépései](backup-azure-vms-automation.md#restore-an-azure-vm)</span><span class="sxs-lookup"><span data-stu-id="25d54-115">$details is populated with restore disk job details, as mentioned in [PowerShell steps in Restore hello Disks section](backup-azure-vms-automation.md#restore-an-azure-vm)</span></span>
> 2. <span data-ttu-id="25d54-116">Virtuális gép kell létrehozni, csak a visszaállított lemezekből **után és a titkos kulcs visszaállított tookey tároló**.</span><span class="sxs-lookup"><span data-stu-id="25d54-116">VM should be created from restored disks only **after key and secret is restored tookey vault**.</span></span>
>
>

<span data-ttu-id="25d54-117">Lekérdezés hello lemez a részleteket a Tulajdonságok hello feladat visszaállítva.</span><span class="sxs-lookup"><span data-stu-id="25d54-117">Query hello restored disk properties for hello job details.</span></span>

```
PS C:\> $properties = $details.properties
PS C:\> $storageAccountName = $properties["Target Storage Account Name"]
PS C:\> $containerName = $properties["Config Blob Container Name"]
PS C:\> $encryptedBlobName = $properties["Encryption Info Blob Name"]
```

<span data-ttu-id="25d54-118">Az Azure storage-környezet hello beállítása, és állítsa vissza a kulcsot és titkos adatait tartalmazó titkosított virtuális gép JSON-konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="25d54-118">Set hello Azure storage context and restore JSON configuration file containing key and secret details for encrypted VM.</span></span>

```
PS C:\> Set-AzureRmCurrentStorageAccount -Name $storageaccountname -ResourceGroupName '<rg-name>'
PS C:\> $destination_path = 'C:\vmencryption_config.json'
PS C:\> Get-AzureStorageBlobContent -Blob $encryptedBlobName -Container $containerName -Destination $destination_path
PS C:\> $encryptionObject = Get-Content -Path $destination_path  | ConvertFrom-Json
```

## <a name="restore-key"></a><span data-ttu-id="25d54-119">Kulcs helyreállítása</span><span class="sxs-lookup"><span data-stu-id="25d54-119">Restore key</span></span>
<span data-ttu-id="25d54-120">A fent említett hello célútvonalat hello JSON-fájl jön létre, ha hello JSON hozza létre kulcs fájlját, és újra a hello kulcstároló toorestore kulcs parancsmag tooput hello kulcs-(KEK) az adatcsatorna.</span><span class="sxs-lookup"><span data-stu-id="25d54-120">Once hello JSON file is generated in hello destination path mentioned above, generate key blob file from hello JSON and feed it toorestore key cmdlet tooput hello key (KEK) back in hello key vault.</span></span>

```
PS C:\> $keyDestination = 'C:\keyDetails.blob'
PS C:\> [io.file]::WriteAllBytes($keyDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyBackupData))
PS C:\> Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile $keyDestination
```

## <a name="restore-secret"></a><span data-ttu-id="25d54-121">Titkos kulcs visszaállítása</span><span class="sxs-lookup"><span data-stu-id="25d54-121">Restore secret</span></span>
<span data-ttu-id="25d54-122">Használata hello JSON-fájl generált fent tooget titkos nevét és értékét, és a hírcsatorna azt újra a hello kulcstároló tooset titkos parancsmag tooput hello titkos (BEK).</span><span class="sxs-lookup"><span data-stu-id="25d54-122">Use hello JSON file generated above tooget secret name and value and feed it tooset secret cmdlet tooput hello secret (BEK) back in hello key vault.</span></span> <span data-ttu-id="25d54-123">**Ezeket a parancsmagokat használja, ha a virtuális gép titkosítása BEK és KEK.**</span><span class="sxs-lookup"><span data-stu-id="25d54-123">**Use these cmdlets if your VM is encrypted using BEK and KEK.**</span></span>

```
PS C:\> $secretdata = $encryptionObject.OsDiskKeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
PS C:\> $secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = $encryptionObject.OsDiskKeyAndSecretDetails.KeyUrl;'MachineName' = 'vm-name'}
PS C:\> Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $Secret -ContentType  'Wrapped BEK' -Tags $Tags
```

<span data-ttu-id="25d54-124">Ha a virtuális gép **BEK csak használatával titkosított**, titkos blob-fájl generálása hello JSON és a hírcsatorna azt újra a hello kulcstároló toorestore titkos parancsmag tooput hello titkos (BEK).</span><span class="sxs-lookup"><span data-stu-id="25d54-124">If your VM is **encrypted using BEK only**, generate secret blob file from hello JSON and feed it toorestore secret cmdlet tooput hello secret (BEK) back in hello key vault.</span></span>

```
PS C:\> $secretDestination = 'C:\secret.blob'
PS C:\> [io.file]::WriteAllBytes($secretDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyVaultSecretBackupData))
PS C:\> Restore-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -InputFile $secretDestination -Verbose
```

> [!NOTE]
> 1. <span data-ttu-id="25d54-125">$Secretname értéke kérhetők le utaló $encryptionObject.OsDiskKeyAndSecretDetails.SecretUrl toohello kimenete és titkos kulcsok után pedig SMS használatával / pl. kimeneti titkos URL-cím https://keyvaultname.vault.azure.net/secrets/ B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 és titkos neve B3284AAA-DAAA-4AAA-B393-60CAA848AAAA:</span><span class="sxs-lookup"><span data-stu-id="25d54-125">Value for $secretname can be obtained by referring toohello output of $encryptionObject.OsDiskKeyAndSecretDetails.SecretUrl and using text after secrets/ e.g. output secret URL is https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 and secret name is B3284AAA-DAAA-4AAA-B393-60CAA848AAAA</span></span>
> 2. <span data-ttu-id="25d54-126">Hello címke DiskEncryptionKeyFileName értéke ugyanaz, mint a titkos nevét.</span><span class="sxs-lookup"><span data-stu-id="25d54-126">Value of hello tag DiskEncryptionKeyFileName is same as secret name.</span></span>
>
>

## <a name="create-virtual-machine-from-restored-disk"></a><span data-ttu-id="25d54-127">Virtuális gép létrehozása a visszaállított lemezről</span><span class="sxs-lookup"><span data-stu-id="25d54-127">Create virtual machine from restored disk</span></span>
<span data-ttu-id="25d54-128">Ha készített biztonsági másolatot titkosított virtuális gép Azure virtuális gép biztonsági mentéssel, hello PowerShell-parancsmagok fent említett súgó állítsa vissza a kulcsot és titkos hátsó toohello kulcstároló.</span><span class="sxs-lookup"><span data-stu-id="25d54-128">If you have backed up encrypted VM using Azure VM Backup, hello PowerShell cmdlets mentioned above help you restore key and secret back toohello key vault.</span></span> <span data-ttu-id="25d54-129">Után a visszaállítás, tekintse meg a hello cikk [biztonsági mentéséhez és visszaállításához a PowerShell használata Azure virtuális gépek kezelése](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) toocreate titkosított virtuális gépek a visszaállított lemez, a kulcs és a titkos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="25d54-129">After restoring them, refer hello article [Manage backup and restore of Azure VMs using PowerShell](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) toocreate encrypted VMs from restored disk, key, and secret.</span></span>

## <a name="legacy-approach"></a><span data-ttu-id="25d54-130">A hagyományos megközelítés</span><span class="sxs-lookup"><span data-stu-id="25d54-130">Legacy approach</span></span>
<span data-ttu-id="25d54-131">a fent említett hello megközelítés minden hello helyreállítási pontot csatlakoztatás működik.</span><span class="sxs-lookup"><span data-stu-id="25d54-131">hello approach mentioned above would work for all hello recovery points.</span></span> <span data-ttu-id="25d54-132">Azonban hello régebbi módszert is, amely kulcsot és titkos információk lekérése a helyreállítási pont lesz érvényes a 2017. július 11. BEK és KEK használatával titkosított virtuális gépek régebbi helyreállítási pontok.</span><span class="sxs-lookup"><span data-stu-id="25d54-132">However, hello older approach of getting key and secret information from recovery point, would be valid for recovery points older than July 11, 2017 for VMs encrypted using BEK and KEK.</span></span> <span data-ttu-id="25d54-133">A visszaállítási lemez feladatot titkosított virtuális gép használatának befejezése után [PowerShell lépéseket](backup-azure-vms-automation.md#restore-an-azure-vm), győződjön meg arról, hogy $rp érvényes értéket a telepítéskor.</span><span class="sxs-lookup"><span data-stu-id="25d54-133">Once restore disk job is complete for encrypted VM using [PowerShell steps](backup-azure-vms-automation.md#restore-an-azure-vm), ensure that $rp is populated with a valid value.</span></span>

### <a name="restore-key"></a><span data-ttu-id="25d54-134">Kulcs helyreállítása</span><span class="sxs-lookup"><span data-stu-id="25d54-134">Restore key</span></span>
<span data-ttu-id="25d54-135">Használja a következő parancsmagok tooget kulcs-(KEK) információ helyreállítási pontból hello és hírcsatorna azt toorestore kulcs parancsmag tooput hello a key vaultban vissza.</span><span class="sxs-lookup"><span data-stu-id="25d54-135">Use hello following cmdlets tooget key (KEK) information from recovery point and feed it toorestore key cmdlet tooput it back in hello key vault.</span></span>

```
PS C:\> $rp1 = Get-AzureRmRecoveryServicesBackupRecoveryPoint -RecoveryPointId $rp[0].RecoveryPointId -Item $backupItem -KeyFileDownloadLocation 'C:\Users\downloads'
PS C:\> Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile 'C:\Users\downloads'
```

### <a name="restore-secret"></a><span data-ttu-id="25d54-136">Titkos kulcs visszaállítása</span><span class="sxs-lookup"><span data-stu-id="25d54-136">Restore secret</span></span>
<span data-ttu-id="25d54-137">Használja a következő parancsmagok tooget titkos (BEK) információ helyreállítási pontból hello és hírcsatorna azt tooset titkos parancsmag tooput hello a key vaultban vissza.</span><span class="sxs-lookup"><span data-stu-id="25d54-137">Use hello following cmdlets tooget secret (BEK) information from recovery point and feed it tooset secret cmdlet tooput it back in hello key vault.</span></span>

```
PS C:\> $secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
PS C:\> $secretdata = $rp1.KeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = 'https://mykeyvault.vault.azure.net:443/keys/KeyName/84daaac999949999030bf99aaa5a9f9';'MachineName' = 'vm-name'}
PS C:\> Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $secret -Tags $Tags -SecretValue $Secret -ContentType  'Wrapped BEK'
```

> [!NOTE]
> 1. <span data-ttu-id="25d54-138">$Secretname értéke $rp1 toohello kimenete hivatkozással érhető el. KeyAndSecretDetails.SecretUrl és titkos kulcsok után pedig SMS használatával / pl. kimeneti titkos URL-cím https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 és titkos neve B3284AAA-DAAA-4AAA-B393-60CAA848AAAA</span><span class="sxs-lookup"><span data-stu-id="25d54-138">Value for $secretname can be obtained by referring toohello output of $rp1.KeyAndSecretDetails.SecretUrl and using text after secrets/ e.g. output secret URL is https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 and secret name is B3284AAA-DAAA-4AAA-B393-60CAA848AAAA</span></span>
> 2. <span data-ttu-id="25d54-139">Hello címke DiskEncryptionKeyFileName értéke ugyanaz, mint a titkos nevét.</span><span class="sxs-lookup"><span data-stu-id="25d54-139">Value of hello tag DiskEncryptionKeyFileName is same as secret name.</span></span>
> 3. <span data-ttu-id="25d54-140">Kulcstároló DiskEncryptionKeyEncryptionKeyURL értéke beszerezhető hello kulcsok vissza visszaállítása és használata után [Get-AzureKeyVaultKey](https://msdn.microsoft.com/library/dn868053.aspx) parancsmag</span><span class="sxs-lookup"><span data-stu-id="25d54-140">Value for DiskEncryptionKeyEncryptionKeyURL can be obtained from key vault after restoring hello keys back and using [Get-AzureKeyVaultKey](https://msdn.microsoft.com/library/dn868053.aspx) cmdlet</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="25d54-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="25d54-141">Next steps</span></span>
<span data-ttu-id="25d54-142">Kulcs és a titkos hátsó tookey tároló visszaállítása, után tekintse meg a hello cikk [biztonsági mentéséhez és visszaállításához a PowerShell használata Azure virtuális gépek kezelése](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) toocreate titkosított virtuális gépek a visszaállított lemez, a kulcs és a titkos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="25d54-142">After restoring key and secret back tookey vault, refer hello article [Manage backup and restore of Azure VMs using PowerShell](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) toocreate encrypted VMs from restored disk, key and secret.</span></span>
