---
ms.assetid: 
title: "Az Azure Key Vault Tárfiókkulcsok"
description: "Tárfiókkulcsok az Azure Storage-fiókot adjon meg egy Azure Key Vault és a kulcs alapú hozzáférés seemless integrációjával."
ms.topic: article
services: key-vault
ms.service: key-vault
author: BrucePerlerMS
ms.author: bruceper
manager: mbaldwin
ms.date: 07/25/2017
ms.openlocfilehash: 3148088c88236c64e089fd25c98eb8ac7cdcbfea
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-key-vault-storage-account-keys"></a>Az Azure Key Vault Tárfiókkulcsok

Azure Key Vault Tárfiókok kulcsait, mielőtt a fejlesztők kellett a saját Azure Storage-fiók (ASA) kulcsok kezelése és elforgatása őket manuálisan vagy egy külső automation. Most, a Key Vault Tárfiókok kulcsait, megvalósítva [Key Vault titkok](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates#BKMK_WorkingWithSecrets) hitelesítéséhez az Azure Storage-fiók. 

Az ASA alapfunkciója kezeli az Ön titkos elforgatás, és többé nem kell a közvetlen forduljon ASA kulccsal rendelkező megosztott hozzáférési aláírásokkal (SAS) módszerként felajánlásával. 

Az Azure Storage-fiókokról további általános információkért lásd: [tudnivalók az Azure storage-fiókok](https://docs.microsoft.com/azure/storage/storage-create-storage-account).

## <a name="supporting-interfaces"></a>Kapcsolatok támogatása

Az Azure Storage-fiók kulcsok funkció kezdetben érhető el a többi, .NET útján / C# és PowerShell-felületek. További információkért lásd: [Key Vault hivatkozás](https://docs.microsoft.com/azure/key-vault/).


## <a name="storage-account-keys-behavior"></a>Tárolási fiók kulcsok viselkedése

### <a name="what-key-vault-manages"></a>Key Vault kezeli

Key Vault több belső felügyeleti funkciókat az Ön nevében hajtja végre a Tárfiókkulcsok használatakor.

1. Az Azure Key Vault kezeli a kulcsokat egy Azure Storage figyelembe (ASA). 
    - Belsőleg az Azure Key Vault készíthetünk egy Azure Storage-fiók (sync) kulcsokat.  
    - Az Azure Key Vault újragenerálja (forog) a kulcsok rendszeres időközönként. 
    - Értékek soha nem visszakerülnek a hívó adott válaszként. 
    - Az Azure Key Vault kezeli a Storage-fiókok és a klasszikus Tárfiókokat kulcsokat. 
2. Az Azure Key Vault lehetővé teszi, a tároló/objektum tulajdonosa, SAS (fiók vagy szolgáltatás SAS)-meghatározások létrehozása. 
    - A SAS értéket, SAS-definícióban használatával létrehozott egy titkos kulcsként adja vissza a többi URI elérési útján. További információkért lásd: [Azure Key Vault tárolási fiók műveletek](https://docs.microsoft.com/rest/api/keyvault/storage-account-key-operations).

### <a name="naming-guidance"></a>Elnevezési irányelvei

A tárfiókok neve 3–24 karakter hosszúságú lehet, és csak számokból és kisbetűkből állhat.  
 
A SAS-definíció nevét 1-102 karakter hosszúságú-csak 0-9, a – z, A-Z kell lennie.

## <a name="developer-experience"></a>A fejlesztői változat 

### <a name="before-azure-key-vault-storage-keys"></a>Az Azure Key Vault tárolási kulcsok előtt 

A fejlesztők kell tennie az alábbi eljárásokkal a tárfiók kulcsa a használatával is elérheti az Azure storage. 
 
 ```
//create storage account using connection string containing account name 
// and the storage key 

var storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));
var blobClient = storageAccount.CreateCloudBlobClient();
 ```
 
### <a name="after-azure-key-vault-storage-keys"></a>Az Azure Key Vault tárolási kulcsok után 

```
//Please make sure to set storage permissions appropriately on your key vault
Set-AzureRmKeyVaultAccessPolicy -VaultName 'yourVault' -ObjectId yourObjectId -PermissionsToStorage all

//Use PowerShell command to get Secret URI 

Set-AzureKeyVaultManagedStorageSasDefinition -Service Blob -ResourceType Container,Service -VaultName yourKV  
-AccountName msak01 -Name blobsas1 -Protocol HttpsOnly -ValidityPeriod ([System.Timespan]::FromDays(1)) -Permission Read,List

//Get SAS token from Key Vault

var secret = await kv.GetSecretAsync("SecretUri");

// Create new storage credentials using the SAS token. 

var accountSasCredential = new StorageCredentials(secret.Value); 

// Use credentials and the Blob storage endpoint to create a new Blob service client. 

var accountWithSas = new CloudStorageAccount(accountSasCredential, new Uri ("https://myaccount.blob.core.windows.net/"), null, null, null); 

var blobClientWithSas = accountWithSas.CreateCloudBlobClient(); 
 
// If SAS token is about to expire then Get sasToken again from Key Vault and update it.

accountSasCredential.UpdateSASToken(sasToken);

  ```
 
 ### <a name="developer-best-practices"></a>Fejlesztői gyakorlati tanácsok 

- Engedélyezése csak a Key Vault a ASA kulcsok kezeléséhez. Ne kísérelje meg magának kezelhetők, a Key Vault folyamatok zavarja meg. 
- Több Key Vault objektum által kezelt ASA kulcsok nem engedélyezett. 
- Manuálisan újragenerálja a ASA kulcsok van szüksége, azt javasoljuk keresztül Key Vault generálni. 

## <a name="getting-started"></a>Bevezetés

### <a name="setup-for-role-based-access-control-rbac-permissions"></a>A szerepköralapú hozzáférés-vezérlést (RBAC) engedélyek beállítása

Key Vault engedélyeket kell *lista* és *újragenerálja* tárfiókok esetén a kulcsok. Állítsa be ezeket az engedélyeket az alábbi lépéseket követve:

- A kulcstároló ObjectId beolvasása: 

    `Get-AzureRmADServicePrincipal -SearchString "AzureKeyVault"`

- Tárolási kulcs kezelői szerepkör hozzárendelése az Azure Key Vault Identity: 

    `New-AzureRmRoleAssignment -ObjectId <objectId of AzureKeyVault from previous command> -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope '<azure resource id of storage account>'`

    >[!NOTE]
    > A klasszikus fióktípus be a szerepkör-paraméter *"Klasszikus tárolási fiók kulcs operátori szerepkör-szolgáltatás"*.

### <a name="storage-account-onboarding"></a>Tárolási fiók bevezetése 

Példa: tulajdonosaként Key Vault objektumot ad hozzá egy tárolóobjektumot fiók az Azure Key Vault szolgáltatás bevezetésében egy tárfiókot.

Bevezetési, során Key Vault kell győződjön meg arról, hogy a bevezetési fiók identitását jogosult legyen *lista* , és *újragenerálja* tárolási kulcsok. Ahhoz, hogy ezek az engedélyek ellenőrzéséhez a Key Vault lekérése egy OBO (a nevében a) token a hitelesítési szolgáltatás, állítsa be az Azure Resource Manager célközönség, és lehetővé teszi egy *lista* kulcsát az Azure Storage szolgáltatás hívása. Ha a *lista* hívás sikertelen lesz, az objektum-létrehozás sikertelen, és a HTTP-állapotkód: a Key Vault *tiltott*. Ilyen módon szereplő kulcsok gyorsítótárba kerüljenek-e a kulcstartót entitás tárolóval. 

Key Vault ellenőriznie kell, hogy rendelkezik-e az identity *újragenerálja* engedélyek előtt tulajdonjogát, a kulcsok újragenerálása is igénybe vehet. Győződjön meg arról, hogy rendelkezik-e ezeket az engedélyeket az identitást, keresztül OBO jogkivonatot, valamint a Key Vault első fél identitás: a

- Key Vault a tárolási fiók erőforrás RBAC engedélyeinek sorolja fel.
- Key Vault érvényesíti a válaszból reguláris kifejezések egyeztetésének műveletek és a nem műveletek keresztül. 

A támogató példákat található [Key Vault - kezelt tárolási fiók kulcsok minták](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/KeyVault/dataPlane/Microsoft.Azure.KeyVault.Samples/samples/HelloKeyVault/Program.cs#L167).

Ha az identitás nem rendelkezik *újragenerálja* engedélyeket, vagy ha a Key Vault első fél identitása nem rendelkezik *lista* vagy *újragenerálja* engedélyt, majd a Bevezetés kérelem sikertelen lesz, egy megfelelő hibakód és az üzenet vissza. 

A OBO jogkivonat-k, a PowerShell vagy a CLI natív ügyfélalkalmazások használatakor csak működik.

## <a name="other-applications"></a>Más alkalmazások

- SAS-tokenje használatával a Key Vault tárfiókok kulcsait, az Azure-tárfiók még több ellenőrzött hozzáférést biztosítanak. További információkért lásd: [használata közös hozzáférésű jogosultságkód](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1).

## <a name="see-also"></a>Lásd még:

- [A kulcsok, titkos kódok és tanúsítványok ismertetése](https://docs.microsoft.com/rest/api/keyvault/)
- [Kulcstároló csapat blogja](https://blogs.technet.microsoft.com/kv/)
