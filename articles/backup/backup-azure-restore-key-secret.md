---
title: "Állítsa vissza a Key Vault kulcs és a titkos kulcs titkosított virtuális gépek Azure Backup használatával |} Microsoft Docs"
description: "Ismerje meg, hogyan lehet visszaállítani a Key Vault kulcsot és titkos kulcsot az Azure Backup szolgáltatáshoz a PowerShell használatával"
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
ms.openlocfilehash: f2db3449187d655248b13198b268841052570626
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="restore-key-vault-key-and-secret-for-encrypted-vms-using-azure-backup"></a><span data-ttu-id="69931-103">Állítsa vissza a Key Vault kulcs és a titkos kulcs titkosított virtuális gépek Azure Backup segítségével</span><span class="sxs-lookup"><span data-stu-id="69931-103">Restore Key Vault key and secret for encrypted VMs using Azure Backup</span></span>
<span data-ttu-id="69931-104">Ez a cikk beszél biztonsági mentéssel Azure VM visszaállításhoz titkosított Azure virtuális gépeken, ha a kulcs és a titkos kulcs nem létezik a kulcstároló kulcsain.</span><span class="sxs-lookup"><span data-stu-id="69931-104">This article talks about using Azure VM Backup to perform restore of encrypted Azure VMs, if your key and secret do not exist in the key vault.</span></span> <span data-ttu-id="69931-105">Ezeket a lépéseket is használható, ha meg szeretné tartani a (Key titkosítási kulcsot) és a titkos kulcs (a BitLocker titkosítási kulcs) egy külön példányát a visszaállított virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="69931-105">These steps can also be used if you want to maintain a separate copy of key (Key Encryption Key) and secret (BitLocker Encryption Key) for the restored VM.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="69931-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="69931-106">Prerequisites</span></span>
* <span data-ttu-id="69931-107">**Biztonsági másolat a virtuális gépek titkosítva** - titkosított Azure virtuális gépek mentett Azure Backup segítségével.</span><span class="sxs-lookup"><span data-stu-id="69931-107">**Backup encrypted VMs** - Encrypted Azure VMs have been backed up using Azure Backup.</span></span> <span data-ttu-id="69931-108">Tekintse meg a cikk [biztonsági mentéséhez és visszaállításához a PowerShell használata Azure virtuális gépek kezelése](backup-azure-vms-automation.md) titkosított Azure virtuális gépek biztonsági mentés részletei.</span><span class="sxs-lookup"><span data-stu-id="69931-108">Refer the article [Manage backup and restore of Azure VMs using PowerShell](backup-azure-vms-automation.md) for details about how to backup encrypted Azure VMs.</span></span>
* <span data-ttu-id="69931-109">**Konfigurálja az Azure Key Vault** – győződjön meg arról, hogy a kulcstároló, amelyhez kulcsok és titkos kell állítani már létezik.</span><span class="sxs-lookup"><span data-stu-id="69931-109">**Configure Azure Key Vault** – Ensure that key vault to which keys and secrets need to be restored is already present.</span></span> <span data-ttu-id="69931-110">Tekintse meg a cikk [Ismerkedés az Azure Key Vault](../key-vault/key-vault-get-started.md) kulcstároló-kezeléssel kapcsolatos részletekért.</span><span class="sxs-lookup"><span data-stu-id="69931-110">Refer the article [Get Started with Azure Key Vault](../key-vault/key-vault-get-started.md) for details about key vault management.</span></span>
* <span data-ttu-id="69931-111">**Állítsa vissza a lemez** -győződjön meg arról, hogy Ön léptetnie visszaállítási feladat lemezeket a használatával titkosított virtuális gép visszaállítására [PowerShell lépéseket](backup-azure-vms-automation.md#restore-an-azure-vm).</span><span class="sxs-lookup"><span data-stu-id="69931-111">**Restore disk** - Ensure that you have triggered restore job for restoring disks for encrypted VM using [PowerShell steps](backup-azure-vms-automation.md#restore-an-azure-vm).</span></span> <span data-ttu-id="69931-112">Ennek oka az, a feladatot hoz létre egy JSON-fájlt tartalmazó kulcsok és titkos állítani a titkosított virtuális gép tárfiókját.</span><span class="sxs-lookup"><span data-stu-id="69931-112">This is because this job generates a JSON file in your storage account containing keys and secrets for the encrypted VM to be restored.</span></span>

## <a name="get-key-and-secret-from-azure-backup"></a><span data-ttu-id="69931-113">Az Azure Backup kulcs és a titkos kulcs beszerzése</span><span class="sxs-lookup"><span data-stu-id="69931-113">Get key and secret from Azure Backup</span></span>

> [!NOTE]
> <span data-ttu-id="69931-114">Miután lemez helyre lett állítva a titkosított virtuális gép számára, győződjön meg arról, hogy:</span><span class="sxs-lookup"><span data-stu-id="69931-114">Once disk has been restored for the encrypted VM, ensure that:</span></span>
> 1. <span data-ttu-id="69931-115">$details feltöltődik a visszaállítási lemez feladat részleteit, ahogyan az [visszaállítási lépések PowerShell a lemez szakasz](backup-azure-vms-automation.md#restore-an-azure-vm)</span><span class="sxs-lookup"><span data-stu-id="69931-115">$details is populated with restore disk job details, as mentioned in [PowerShell steps in Restore the Disks section](backup-azure-vms-automation.md#restore-an-azure-vm)</span></span>
> 2. <span data-ttu-id="69931-116">Virtuális gép kell létrehozni, csak a visszaállított lemezekből **kulcs és a titkos kulcs tárolóba történt visszaállítása után**.</span><span class="sxs-lookup"><span data-stu-id="69931-116">VM should be created from restored disks only **after key and secret is restored to key vault**.</span></span>
>
>

<span data-ttu-id="69931-117">A feladat részleteit a visszaállított lemez tulajdonságainak lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="69931-117">Query the restored disk properties for the job details.</span></span>

```
PS C:\> $properties = $details.properties
PS C:\> $storageAccountName = $properties["Target Storage Account Name"]
PS C:\> $containerName = $properties["Config Blob Container Name"]
PS C:\> $encryptedBlobName = $properties["Encryption Info Blob Name"]
```

<span data-ttu-id="69931-118">Állítsa be az Azure storage-környezetben, és állítsa vissza a kulcsot és titkos adatait tartalmazó titkosított virtuális gép JSON-konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="69931-118">Set the Azure storage context and restore JSON configuration file containing key and secret details for encrypted VM.</span></span>

```
PS C:\> Set-AzureRmCurrentStorageAccount -Name $storageaccountname -ResourceGroupName '<rg-name>'
PS C:\> $destination_path = 'C:\vmencryption_config.json'
PS C:\> Get-AzureStorageBlobContent -Blob $encryptedBlobName -Container $containerName -Destination $destination_path
PS C:\> $encryptionObject = Get-Content -Path $destination_path  | ConvertFrom-Json
```

## <a name="restore-key"></a><span data-ttu-id="69931-119">Kulcs helyreállítása</span><span class="sxs-lookup"><span data-stu-id="69931-119">Restore key</span></span>
<span data-ttu-id="69931-120">Miután a JSON-fájl jön létre a fent említett elérési utat, hozza létre a JSON kulcs fájlját, és úgy, hogy a kulcs parancsmag használatával helyezze vissza a kulcsot (KEK) a key vault visszaállítása hírcsatorna.</span><span class="sxs-lookup"><span data-stu-id="69931-120">Once the JSON file is generated in the destination path mentioned above, generate key blob file from the JSON and feed it to restore key cmdlet to put the key (KEK) back in the key vault.</span></span>

```
PS C:\> $keyDestination = 'C:\keyDetails.blob'
PS C:\> [io.file]::WriteAllBytes($keyDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyBackupData))
PS C:\> Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile $keyDestination
```

## <a name="restore-secret"></a><span data-ttu-id="69931-121">Titkos kulcs visszaállítása</span><span class="sxs-lookup"><span data-stu-id="69931-121">Restore secret</span></span>
<span data-ttu-id="69931-122">A létrehozott fenti titkos nevét és értékét, és az adatcsatorna JSON-fájl használatával állítsa vissza a titkos (BEK) kerüljenek bele a key vault titkos parancsmag.</span><span class="sxs-lookup"><span data-stu-id="69931-122">Use the JSON file generated above to get secret name and value and feed it to set secret cmdlet to put the secret (BEK) back in the key vault.</span></span> <span data-ttu-id="69931-123">**Ezeket a parancsmagokat használja, ha a virtuális gép titkosítása BEK és KEK.**</span><span class="sxs-lookup"><span data-stu-id="69931-123">**Use these cmdlets if your VM is encrypted using BEK and KEK.**</span></span>

```
PS C:\> $secretdata = $encryptionObject.OsDiskKeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
PS C:\> $secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = $encryptionObject.OsDiskKeyAndSecretDetails.KeyUrl;'MachineName' = 'vm-name'}
PS C:\> Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $Secret -ContentType  'Wrapped BEK' -Tags $Tags
```

<span data-ttu-id="69931-124">Ha a virtuális gép **BEK csak használatával titkosított**, hozza létre a JSON titkos blob fájlját, és úgy, hogy a titkos parancsmagot, hogy a titkos kulcsot (BEK) helyezze vissza a key vault visszaállítása hírcsatorna.</span><span class="sxs-lookup"><span data-stu-id="69931-124">If your VM is **encrypted using BEK only**, generate secret blob file from the JSON and feed it to restore secret cmdlet to put the secret (BEK) back in the key vault.</span></span>

```
PS C:\> $secretDestination = 'C:\secret.blob'
PS C:\> [io.file]::WriteAllBytes($secretDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyVaultSecretBackupData))
PS C:\> Restore-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -InputFile $secretDestination -Verbose
```

> [!NOTE]
> 1. <span data-ttu-id="69931-125">$Secretname értéke kérhetők le $encryptionObject.OsDiskKeyAndSecretDetails.SecretUrl kimenete hivatkozó és titkos kulcsok után pedig SMS használatával / pl. kimeneti titkos URL-cím https://keyvaultname.vault.azure.net/secrets/ B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 és titkos neve B3284AAA-DAAA-4AAA-B393-60CAA848AAAA:</span><span class="sxs-lookup"><span data-stu-id="69931-125">Value for $secretname can be obtained by referring to the output of $encryptionObject.OsDiskKeyAndSecretDetails.SecretUrl and using text after secrets/ e.g. output secret URL is https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 and secret name is B3284AAA-DAAA-4AAA-B393-60CAA848AAAA</span></span>
> 2. <span data-ttu-id="69931-126">A címke DiskEncryptionKeyFileName értéke ugyanaz, mint a titkos nevét.</span><span class="sxs-lookup"><span data-stu-id="69931-126">Value of the tag DiskEncryptionKeyFileName is same as secret name.</span></span>
>
>

## <a name="create-virtual-machine-from-restored-disk"></a><span data-ttu-id="69931-127">Virtuális gép létrehozása a visszaállított lemezről</span><span class="sxs-lookup"><span data-stu-id="69931-127">Create virtual machine from restored disk</span></span>
<span data-ttu-id="69931-128">Ha a biztonsági másolatban titkosított virtuális gép Azure virtuális gép biztonsági mentéssel, a PowerShell-parancsmagok fent említett súgó állítja vissza a kulcsot és titkos vissza a key vault.</span><span class="sxs-lookup"><span data-stu-id="69931-128">If you have backed up encrypted VM using Azure VM Backup, the PowerShell cmdlets mentioned above help you restore key and secret back to the key vault.</span></span> <span data-ttu-id="69931-129">Után a visszaállítás, tekintse meg a cikk [biztonsági mentéséhez és visszaállításához a PowerShell használata Azure virtuális gépek kezelése](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) a visszaállított lemez, a kulcs és a titkos kulcs titkosított virtuális gépek létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="69931-129">After restoring them, refer the article [Manage backup and restore of Azure VMs using PowerShell](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) to create encrypted VMs from restored disk, key, and secret.</span></span>

## <a name="legacy-approach"></a><span data-ttu-id="69931-130">A hagyományos megközelítés</span><span class="sxs-lookup"><span data-stu-id="69931-130">Legacy approach</span></span>
<span data-ttu-id="69931-131">A fent említett módszer a helyreállítási pontok csatlakoztatás működik.</span><span class="sxs-lookup"><span data-stu-id="69931-131">The approach mentioned above would work for all the recovery points.</span></span> <span data-ttu-id="69931-132">Azonban a régebbi módszert is, amely kulcsot és titkos információk lekérése a helyreállítási pont lesz érvényes a 2017. július 11. BEK és KEK használatával titkosított virtuális gépek régebbi helyreállítási pontok.</span><span class="sxs-lookup"><span data-stu-id="69931-132">However, the older approach of getting key and secret information from recovery point, would be valid for recovery points older than July 11, 2017 for VMs encrypted using BEK and KEK.</span></span> <span data-ttu-id="69931-133">A visszaállítási lemez feladatot titkosított virtuális gép használatának befejezése után [PowerShell lépéseket](backup-azure-vms-automation.md#restore-an-azure-vm), győződjön meg arról, hogy $rp érvényes értéket a telepítéskor.</span><span class="sxs-lookup"><span data-stu-id="69931-133">Once restore disk job is complete for encrypted VM using [PowerShell steps](backup-azure-vms-automation.md#restore-an-azure-vm), ensure that $rp is populated with a valid value.</span></span>

### <a name="restore-key"></a><span data-ttu-id="69931-134">Kulcs helyreállítása</span><span class="sxs-lookup"><span data-stu-id="69931-134">Restore key</span></span>
<span data-ttu-id="69931-135">A következő parancsmagok használatával (KEK) kapcsolatos információkat lekérni a helyreállítási pont és az adatcsatorna úgy, hogy a kulcs parancsmag használatával helyezze vissza a key vault visszaállítása.</span><span class="sxs-lookup"><span data-stu-id="69931-135">Use the following cmdlets to get key (KEK) information from recovery point and feed it to restore key cmdlet to put it back in the key vault.</span></span>

```
PS C:\> $rp1 = Get-AzureRmRecoveryServicesBackupRecoveryPoint -RecoveryPointId $rp[0].RecoveryPointId -Item $backupItem -KeyFileDownloadLocation 'C:\Users\downloads'
PS C:\> Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile 'C:\Users\downloads'
```

### <a name="restore-secret"></a><span data-ttu-id="69931-136">Titkos kulcs visszaállítása</span><span class="sxs-lookup"><span data-stu-id="69931-136">Restore secret</span></span>
<span data-ttu-id="69931-137">A következő parancsmagok használatával beolvasni a titkos (BEK) adatait a helyreállítási ponttal, és úgy, hogy titkos parancsmag használatával helyezze vissza a key vault hírcsatorna.</span><span class="sxs-lookup"><span data-stu-id="69931-137">Use the following cmdlets to get secret (BEK) information from recovery point and feed it to set secret cmdlet to put it back in the key vault.</span></span>

```
PS C:\> $secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
PS C:\> $secretdata = $rp1.KeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = 'https://mykeyvault.vault.azure.net:443/keys/KeyName/84daaac999949999030bf99aaa5a9f9';'MachineName' = 'vm-name'}
PS C:\> Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $secret -Tags $Tags -SecretValue $Secret -ContentType  'Wrapped BEK'
```

> [!NOTE]
> 1. <span data-ttu-id="69931-138">$Secretname értéke $rp1 kimenetével hivatkozással érhető el. KeyAndSecretDetails.SecretUrl és titkos kulcsok után pedig SMS használatával / pl. kimeneti titkos URL-cím https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 és titkos neve B3284AAA-DAAA-4AAA-B393-60CAA848AAAA</span><span class="sxs-lookup"><span data-stu-id="69931-138">Value for $secretname can be obtained by referring to the output of $rp1.KeyAndSecretDetails.SecretUrl and using text after secrets/ e.g. output secret URL is https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 and secret name is B3284AAA-DAAA-4AAA-B393-60CAA848AAAA</span></span>
> 2. <span data-ttu-id="69931-139">A címke DiskEncryptionKeyFileName értéke ugyanaz, mint a titkos nevét.</span><span class="sxs-lookup"><span data-stu-id="69931-139">Value of the tag DiskEncryptionKeyFileName is same as secret name.</span></span>
> 3. <span data-ttu-id="69931-140">Kulcstároló DiskEncryptionKeyEncryptionKeyURL értéke beszerezhető visszaállítása vissza a kulcsokat, és használata után [Get-AzureKeyVaultKey](https://msdn.microsoft.com/library/dn868053.aspx) parancsmag</span><span class="sxs-lookup"><span data-stu-id="69931-140">Value for DiskEncryptionKeyEncryptionKeyURL can be obtained from key vault after restoring the keys back and using [Get-AzureKeyVaultKey](https://msdn.microsoft.com/library/dn868053.aspx) cmdlet</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="69931-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="69931-141">Next steps</span></span>
<span data-ttu-id="69931-142">Kulcstároló kulcsot és titkos vissza visszaállítása, után tekintse meg a cikk [biztonsági mentéséhez és visszaállításához a PowerShell használata Azure virtuális gépek kezelése](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) a visszaállított lemez, a kulcs és a titkos kulcs titkosított virtuális gépek létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="69931-142">After restoring key and secret back to key vault, refer the article [Manage backup and restore of Azure VMs using PowerShell](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) to create encrypted VMs from restored disk, key and secret.</span></span>
