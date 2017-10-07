---
ms.assetid: 
title: "Key Vault Tárfiókkulcsok aaaAzure"
description: "Tárfiókkulcsok adjon meg egy seemless integráció az Azure Key Vault és alapuló hozzáférés a kulcshoz tooAzure Tárfiók között."
ms.topic: article
services: key-vault
ms.service: key-vault
author: BrucePerlerMS
ms.author: bruceper
manager: mbaldwin
ms.date: 07/25/2017
ms.openlocfilehash: becdf97798a08164c48d3a7a14aea6ca54085c9a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-storage-account-keys"></a>Az Azure Key Vault Tárfiókkulcsok

Azure Key Vault Tárfiókok kulcsait, mielőtt a fejlesztők kellett toomanage a saját Azure Storage-fiók (ASA) kulcsokat, és elforgatása őket manuálisan vagy egy külső automation. Most, a Key Vault Tárfiókok kulcsait, megvalósítva [Key Vault titkok](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates#BKMK_WorkingWithSecrets) hitelesítéséhez az Azure Storage-fiók. 

hello ASA alapfunkciója kezeli az Ön titkos elforgatás, és megszüntetéséhez hello szükség a közvetlen kapcsolattól ASA kulccsal rendelkező közös hozzáférésű aláírás (SAS) egy módszert kínál. 

Az Azure Storage-fiókokról további általános információkért lásd: [tudnivalók az Azure storage-fiókok](https://docs.microsoft.com/azure/storage/storage-create-storage-account).

## <a name="supporting-interfaces"></a>Kapcsolatok támogatása

hello Azure Storage-fiók kulcsok funkció érhető el eredetileg keresztül hello REST, .NET / C# és PowerShell-felületek. További információkért lásd: [Key Vault hivatkozás](https://docs.microsoft.com/azure/key-vault/).


## <a name="storage-account-keys-behavior"></a>Tárolási fiók kulcsok viselkedése

### <a name="what-key-vault-manages"></a>Key Vault kezeli

Key Vault több belső felügyeleti funkciókat az Ön nevében hajtja végre a Tárfiókkulcsok használatakor.

1. Az Azure Key Vault kezeli a kulcsokat egy Azure Storage figyelembe (ASA). 
    - Belsőleg az Azure Key Vault készíthetünk egy Azure Storage-fiók (sync) kulcsokat.  
    - Az Azure Key Vault újragenerálja (forog) hello kulcsok rendszeres időközönként. 
    - Válasz toocaller soha nem adott vissza a kulcs értékeket. 
    - Az Azure Key Vault kezeli a Storage-fiókok és a klasszikus Tárfiókokat kulcsokat. 
2. Az Azure Key Vault lehetővé teszi, hogy Ön, hello tároló/objektum tulajdonosa, toocreate (fiók vagy szolgáltatás SAS) SAS-definíciók. 
    - hello SAS érték létrehozni a SAS-definíció, használatával adja vissza a rendszer a titkos kulcs keresztül hello REST URI elérési útja. További információkért lásd: [Azure Key Vault tárolási fiók műveletek](https://docs.microsoft.com/rest/api/keyvault/storage-account-key-operations).

### <a name="naming-guidance"></a>Elnevezési irányelvei

A tárfiókok neve 3–24 karakter hosszúságú lehet, és csak számokból és kisbetűkből állhat.  
 
A SAS-definíció nevét 1-102 karakter hosszúságú-csak 0-9, a – z, A-Z kell lennie.

## <a name="developer-experience"></a>A fejlesztői változat 

### <a name="before-azure-key-vault-storage-keys"></a>Az Azure Key Vault tárolási kulcsok előtt 

A fejlesztők a következő eljárásokat a tárolási fiók kulcs tooget hozzáférés tooAzure tároló tooneed toodo hello használt. 
 
 ```
//create storage account using connection string containing account name 
// and hello storage key 

var storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));
var blobClient = storageAccount.CreateCloudBlobClient();
 ```
 
### <a name="after-azure-key-vault-storage-keys"></a>Az Azure Key Vault tárolási kulcsok után 

```
//Please make sure tooset storage permissions appropriately on your key vault
Set-AzureRmKeyVaultAccessPolicy -VaultName 'yourVault' -ObjectId yourObjectId -PermissionsToStorage all

//Use PowerShell command tooget Secret URI 

Set-AzureKeyVaultManagedStorageSasDefinition -Service Blob -ResourceType Container,Service -VaultName yourKV  
-AccountName msak01 -Name blobsas1 -Protocol HttpsOnly -ValidityPeriod ([System.Timespan]::FromDays(1)) -Permission Read,List

//Get SAS token from Key Vault

var secret = await kv.GetSecretAsync("SecretUri");

// Create new storage credentials using hello SAS token. 

var accountSasCredential = new StorageCredentials(secret.Value); 

// Use credentials and hello Blob storage endpoint toocreate a new Blob service client. 

var accountWithSas = new CloudStorageAccount(accountSasCredential, new Uri ("https://myaccount.blob.core.windows.net/"), null, null, null); 

var blobClientWithSas = accountWithSas.CreateCloudBlobClient(); 
 
// If SAS token is about tooexpire then Get sasToken again from Key Vault and update it.

accountSasCredential.UpdateSASToken(sasToken);

  ```
 
 ### <a name="developer-best-practices"></a>Fejlesztői gyakorlati tanácsok 

- Az ASA kulcsok csak engedélyezze a Key Vault toomanage. Ne kísérelje meg toomanage őket, hogy ne zavarja Key Vault folyamatok. 
- Több Key Vault-objektum által kezelt ASA kulcsok toobe nem teszik lehetővé. 
- Ha szeretne toomanually a ASA kulcsok újragenerálása, javasoljuk, hogy keresztül Key Vault generálni. 

## <a name="getting-started"></a>Bevezetés

### <a name="setup-for-role-based-access-control-rbac-permissions"></a>A szerepköralapú hozzáférés-vezérlést (RBAC) engedélyek beállítása

Key Vault szükséges engedélyek túl*lista* és *újragenerálja* tárfiókok esetén a kulcsok. Állítsa be ezeket az engedélyeket a lépéseket követve hello használata:

- A kulcstároló ObjectId beolvasása: 

    `Get-AzureRmADServicePrincipal -SearchString "AzureKeyVault"`

- Rendelje hozzá a tároló kulcsa operátor szerepkör tooAzure Key Vault identitás: 

    `New-AzureRmRoleAssignment -ObjectId <objectId of AzureKeyVault from previous command> -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope '<azure resource id of storage account>'`

    >[!NOTE]
    > A klasszikus fióktípus hello szerepkör paraméter értéke túl*"Klasszikus tárolási fiók kulcs operátori szerepkör-szolgáltatás"*.

### <a name="storage-account-onboarding"></a>Tárolási fiók bevezetése 

Példa:, A Key Vault objektum tulajdonosa hozzá a tárolási fiók objektum tooyour Azure Key Vault tooonboard egy tárfiókot.

Bevezetési, során Key Vault kell, hogy hello identitás hello bevezetési fiók jogosult túl tooverify*lista* és túl*újragenerálja* tárolási kulcsok. A rendezés tooverify ezeket az engedélyeket, Key Vault lekérése egy OBO (a nevében a) token hello hitelesítési szolgáltatás, a célközönség tooAzure erőforrás-kezelő beállítása, és lehetővé teszi egy *lista* hívás toohello Azure Storage szolgáltatás kulcsát. Ha hello *lista* hívás sikertelen lesz, az objektum-létrehozás sikertelen, és a HTTP-állapotkód: a Key Vault hello *tiltott*. ilyen módon hello kulcsok gyorsítótárba kerüljenek-e a kulcstartót entitás tárolóval. 

Key Vault ellenőriznie kell, hogy rendelkezik-e hello identitás *újragenerálja* engedélyek előtt tulajdonjogát, a kulcsok újragenerálása is igénybe vehet. hello identitás OBO token keresztül, valamint a Key Vault első fél identitás hello tooverify rendelkezik ezekkel a jogosultságokkal:

- Key Vault hello tárolási fiók erőforrás RBAC engedélyeket sorolja fel.
- Key Vault hello válasz keresztül reguláris kifejezések egyeztetésének műveletek és a nem műveletek ellenőrzi. 

A támogató példákat található [Key Vault - kezelt tárolási fiók kulcsok minták](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/KeyVault/dataPlane/Microsoft.Azure.KeyVault.Samples/samples/HelloKeyVault/Program.cs#L167).

Ha hello identitása nem rendelkezik *újragenerálja* engedélyeket, vagy ha a Key Vault első fél identitása nem rendelkezik *lista* vagy *újragenerálja* engedélyt, majd hello bevezetése kérelem sikertelen lesz, egy megfelelő hibakód és az üzenet vissza. 

hello OBO jogkivonat-k, a PowerShell vagy a CLI natív ügyfélalkalmazások használatakor csak működik.

## <a name="other-applications"></a>Más alkalmazások

- SAS-tokenje használatával a Key Vault tárfiókok kulcsait, még több ellenőrzött hozzáférés tooan Azure storage-fiókot adjon meg. További információkért lásd: [használata közös hozzáférésű jogosultságkód](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1).

## <a name="see-also"></a>Lásd még:

- [A kulcsok, titkos kódok és tanúsítványok ismertetése](https://docs.microsoft.com/rest/api/keyvault/)
- [Kulcstároló csapat blogja](https://blogs.technet.microsoft.com/kv/)
