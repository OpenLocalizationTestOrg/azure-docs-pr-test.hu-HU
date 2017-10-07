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
# <a name="azure-key-vault-storage-account-keys"></a><span data-ttu-id="c9e13-103">Az Azure Key Vault Tárfiókkulcsok</span><span class="sxs-lookup"><span data-stu-id="c9e13-103">Azure Key Vault Storage Account Keys</span></span>

<span data-ttu-id="c9e13-104">Azure Key Vault Tárfiókok kulcsait, mielőtt a fejlesztők kellett toomanage a saját Azure Storage-fiók (ASA) kulcsokat, és elforgatása őket manuálisan vagy egy külső automation.</span><span class="sxs-lookup"><span data-stu-id="c9e13-104">Before Azure Key Vault Storage Account Keys, developers had toomanage their own Azure Storage Account (ASA) keys and rotate them manually or through an external automation.</span></span> <span data-ttu-id="c9e13-105">Most, a Key Vault Tárfiókok kulcsait, megvalósítva [Key Vault titkok](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates#BKMK_WorkingWithSecrets) hitelesítéséhez az Azure Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="c9e13-105">Now, Key Vault Storage Account Keys are implemented as [Key Vault secrets](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates#BKMK_WorkingWithSecrets) for authenticating with an Azure Storage Account.</span></span> 

<span data-ttu-id="c9e13-106">hello ASA alapfunkciója kezeli az Ön titkos elforgatás, és megszüntetéséhez hello szükség a közvetlen kapcsolattól ASA kulccsal rendelkező közös hozzáférésű aláírás (SAS) egy módszert kínál.</span><span class="sxs-lookup"><span data-stu-id="c9e13-106">hello ASA key feature manages secret rotation for you and removes hello need for your direct contact with an ASA key by offering Shared Access Signatures (SAS) as a method.</span></span> 

<span data-ttu-id="c9e13-107">Az Azure Storage-fiókokról további általános információkért lásd: [tudnivalók az Azure storage-fiókok](https://docs.microsoft.com/azure/storage/storage-create-storage-account).</span><span class="sxs-lookup"><span data-stu-id="c9e13-107">For more general information on Azure Storage Accounts, see [About Azure storage accounts](https://docs.microsoft.com/azure/storage/storage-create-storage-account).</span></span>

## <a name="supporting-interfaces"></a><span data-ttu-id="c9e13-108">Kapcsolatok támogatása</span><span class="sxs-lookup"><span data-stu-id="c9e13-108">Supporting interfaces</span></span>

<span data-ttu-id="c9e13-109">hello Azure Storage-fiók kulcsok funkció érhető el eredetileg keresztül hello REST, .NET / C# és PowerShell-felületek.</span><span class="sxs-lookup"><span data-stu-id="c9e13-109">hello Azure Storage Account keys feature is initially available through hello REST, .NET/C# and PowerShell interfaces.</span></span> <span data-ttu-id="c9e13-110">További információkért lásd: [Key Vault hivatkozás](https://docs.microsoft.com/azure/key-vault/).</span><span class="sxs-lookup"><span data-stu-id="c9e13-110">For more information, see [Key Vault Reference](https://docs.microsoft.com/azure/key-vault/).</span></span>


## <a name="storage-account-keys-behavior"></a><span data-ttu-id="c9e13-111">Tárolási fiók kulcsok viselkedése</span><span class="sxs-lookup"><span data-stu-id="c9e13-111">Storage account keys behavior</span></span>

### <a name="what-key-vault-manages"></a><span data-ttu-id="c9e13-112">Key Vault kezeli</span><span class="sxs-lookup"><span data-stu-id="c9e13-112">What Key Vault manages</span></span>

<span data-ttu-id="c9e13-113">Key Vault több belső felügyeleti funkciókat az Ön nevében hajtja végre a Tárfiókkulcsok használatakor.</span><span class="sxs-lookup"><span data-stu-id="c9e13-113">Key Vault performs several internal management functions on your behalf when you use Storage Account Keys.</span></span>

1. <span data-ttu-id="c9e13-114">Az Azure Key Vault kezeli a kulcsokat egy Azure Storage figyelembe (ASA).</span><span class="sxs-lookup"><span data-stu-id="c9e13-114">Azure Key Vault manages keys of an Azure Storage Account (ASA).</span></span> 
    - <span data-ttu-id="c9e13-115">Belsőleg az Azure Key Vault készíthetünk egy Azure Storage-fiók (sync) kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="c9e13-115">Internally, Azure Key Vault can list (sync) keys with an Azure Storage Account.</span></span>  
    - <span data-ttu-id="c9e13-116">Az Azure Key Vault újragenerálja (forog) hello kulcsok rendszeres időközönként.</span><span class="sxs-lookup"><span data-stu-id="c9e13-116">Azure Key Vault regenerates (rotates) hello keys periodically.</span></span> 
    - <span data-ttu-id="c9e13-117">Válasz toocaller soha nem adott vissza a kulcs értékeket.</span><span class="sxs-lookup"><span data-stu-id="c9e13-117">Key values are never returned in response toocaller.</span></span> 
    - <span data-ttu-id="c9e13-118">Az Azure Key Vault kezeli a Storage-fiókok és a klasszikus Tárfiókokat kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="c9e13-118">Azure Key Vault manages keys of both Storage Accounts and Classic Storage Accounts.</span></span> 
2. <span data-ttu-id="c9e13-119">Az Azure Key Vault lehetővé teszi, hogy Ön, hello tároló/objektum tulajdonosa, toocreate (fiók vagy szolgáltatás SAS) SAS-definíciók.</span><span class="sxs-lookup"><span data-stu-id="c9e13-119">Azure Key Vault allows you, hello vault/object owner, toocreate SAS (account or service SAS) definitions.</span></span> 
    - <span data-ttu-id="c9e13-120">hello SAS érték létrehozni a SAS-definíció, használatával adja vissza a rendszer a titkos kulcs keresztül hello REST URI elérési útja.</span><span class="sxs-lookup"><span data-stu-id="c9e13-120">hello SAS value, created using SAS definition, is returned as a secret via hello REST URI path.</span></span> <span data-ttu-id="c9e13-121">További információkért lásd: [Azure Key Vault tárolási fiók műveletek](https://docs.microsoft.com/rest/api/keyvault/storage-account-key-operations).</span><span class="sxs-lookup"><span data-stu-id="c9e13-121">For more information, see [Azure Key Vault storage account operations](https://docs.microsoft.com/rest/api/keyvault/storage-account-key-operations).</span></span>

### <a name="naming-guidance"></a><span data-ttu-id="c9e13-122">Elnevezési irányelvei</span><span class="sxs-lookup"><span data-stu-id="c9e13-122">Naming guidance</span></span>

<span data-ttu-id="c9e13-123">A tárfiókok neve 3–24 karakter hosszúságú lehet, és csak számokból és kisbetűkből állhat.</span><span class="sxs-lookup"><span data-stu-id="c9e13-123">Storage account names must be between 3 and 24 characters in length and may contain numbers and lowercase letters only.</span></span>  
 
<span data-ttu-id="c9e13-124">A SAS-definíció nevét 1-102 karakter hosszúságú-csak 0-9, a – z, A-Z kell lennie.</span><span class="sxs-lookup"><span data-stu-id="c9e13-124">A SAS definition name must be 1-102 characters in length containing only 0-9, a-z, A-Z.</span></span>

## <a name="developer-experience"></a><span data-ttu-id="c9e13-125">A fejlesztői változat</span><span class="sxs-lookup"><span data-stu-id="c9e13-125">Developer experience</span></span> 

### <a name="before-azure-key-vault-storage-keys"></a><span data-ttu-id="c9e13-126">Az Azure Key Vault tárolási kulcsok előtt</span><span class="sxs-lookup"><span data-stu-id="c9e13-126">Before Azure Key Vault Storage Keys</span></span> 

<span data-ttu-id="c9e13-127">A fejlesztők a következő eljárásokat a tárolási fiók kulcs tooget hozzáférés tooAzure tároló tooneed toodo hello használt.</span><span class="sxs-lookup"><span data-stu-id="c9e13-127">Developers used tooneed toodo hello following practices with a storage account key tooget access tooAzure storage.</span></span> 
 
 ```
//create storage account using connection string containing account name 
// and hello storage key 

var storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));
var blobClient = storageAccount.CreateCloudBlobClient();
 ```
 
### <a name="after-azure-key-vault-storage-keys"></a><span data-ttu-id="c9e13-128">Az Azure Key Vault tárolási kulcsok után</span><span class="sxs-lookup"><span data-stu-id="c9e13-128">After Azure Key Vault Storage Keys</span></span> 

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
 
 ### <a name="developer-best-practices"></a><span data-ttu-id="c9e13-129">Fejlesztői gyakorlati tanácsok</span><span class="sxs-lookup"><span data-stu-id="c9e13-129">Developer best practices</span></span> 

- <span data-ttu-id="c9e13-130">Az ASA kulcsok csak engedélyezze a Key Vault toomanage.</span><span class="sxs-lookup"><span data-stu-id="c9e13-130">Only allow Key Vault toomanage your ASA keys.</span></span> <span data-ttu-id="c9e13-131">Ne kísérelje meg toomanage őket, hogy ne zavarja Key Vault folyamatok.</span><span class="sxs-lookup"><span data-stu-id="c9e13-131">Do not attempt toomanage them yourself, you will interfere with Key Vault's processes.</span></span> 
- <span data-ttu-id="c9e13-132">Több Key Vault-objektum által kezelt ASA kulcsok toobe nem teszik lehetővé.</span><span class="sxs-lookup"><span data-stu-id="c9e13-132">Do not allow ASA keys toobe managed by more than one Key Vault object.</span></span> 
- <span data-ttu-id="c9e13-133">Ha szeretne toomanually a ASA kulcsok újragenerálása, javasoljuk, hogy keresztül Key Vault generálni.</span><span class="sxs-lookup"><span data-stu-id="c9e13-133">If you need toomanually regenerate your ASA keys, we recommend that you regenerate them via Key Vault.</span></span> 

## <a name="getting-started"></a><span data-ttu-id="c9e13-134">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="c9e13-134">Getting started</span></span>

### <a name="setup-for-role-based-access-control-rbac-permissions"></a><span data-ttu-id="c9e13-135">A szerepköralapú hozzáférés-vezérlést (RBAC) engedélyek beállítása</span><span class="sxs-lookup"><span data-stu-id="c9e13-135">Setup for role-based access control (RBAC) permissions</span></span>

<span data-ttu-id="c9e13-136">Key Vault szükséges engedélyek túl*lista* és *újragenerálja* tárfiókok esetén a kulcsok.</span><span class="sxs-lookup"><span data-stu-id="c9e13-136">Key Vault needs permissions too*list* and *regenerate* keys for a storage account.</span></span> <span data-ttu-id="c9e13-137">Állítsa be ezeket az engedélyeket a lépéseket követve hello használata:</span><span class="sxs-lookup"><span data-stu-id="c9e13-137">Set up these permissions using hello following steps:</span></span>

- <span data-ttu-id="c9e13-138">A kulcstároló ObjectId beolvasása:</span><span class="sxs-lookup"><span data-stu-id="c9e13-138">Get ObjectId of Key Vault:</span></span> 

    `Get-AzureRmADServicePrincipal -SearchString "AzureKeyVault"`

- <span data-ttu-id="c9e13-139">Rendelje hozzá a tároló kulcsa operátor szerepkör tooAzure Key Vault identitás:</span><span class="sxs-lookup"><span data-stu-id="c9e13-139">Assign Storage Key Operator role tooAzure Key Vault Identity:</span></span> 

    `New-AzureRmRoleAssignment -ObjectId <objectId of AzureKeyVault from previous command> -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope '<azure resource id of storage account>'`

    >[!NOTE]
    > <span data-ttu-id="c9e13-140">A klasszikus fióktípus hello szerepkör paraméter értéke túl*"Klasszikus tárolási fiók kulcs operátori szerepkör-szolgáltatás"*.</span><span class="sxs-lookup"><span data-stu-id="c9e13-140">For a classic account type, set hello role parameter too*"Classic Storage Account Key Operator Service Role"*.</span></span>

### <a name="storage-account-onboarding"></a><span data-ttu-id="c9e13-141">Tárolási fiók bevezetése</span><span class="sxs-lookup"><span data-stu-id="c9e13-141">Storage account onboarding</span></span> 

<span data-ttu-id="c9e13-142">Példa:, A Key Vault objektum tulajdonosa hozzá a tárolási fiók objektum tooyour Azure Key Vault tooonboard egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="c9e13-142">Example: As a Key Vault object owner you add a storage account object tooyour Azure Key Vault tooonboard a storage account.</span></span>

<span data-ttu-id="c9e13-143">Bevezetési, során Key Vault kell, hogy hello identitás hello bevezetési fiók jogosult túl tooverify*lista* és túl*újragenerálja* tárolási kulcsok.</span><span class="sxs-lookup"><span data-stu-id="c9e13-143">During onboarding, Key Vault needs tooverify that hello identity of hello onboarding account has permissions too*list* and too*regenerate* storage keys.</span></span> <span data-ttu-id="c9e13-144">A rendezés tooverify ezeket az engedélyeket, Key Vault lekérése egy OBO (a nevében a) token hello hitelesítési szolgáltatás, a célközönség tooAzure erőforrás-kezelő beállítása, és lehetővé teszi egy *lista* hívás toohello Azure Storage szolgáltatás kulcsát.</span><span class="sxs-lookup"><span data-stu-id="c9e13-144">In order tooverify these permissions, Key Vault gets an OBO (On Behalf Of) token from hello authentication service, audience set tooAzure Resource Manager, and makes a *list* key call toohello Azure Storage service.</span></span> <span data-ttu-id="c9e13-145">Ha hello *lista* hívás sikertelen lesz, az objektum-létrehozás sikertelen, és a HTTP-állapotkód: a Key Vault hello *tiltott*.</span><span class="sxs-lookup"><span data-stu-id="c9e13-145">If hello *list* call fails, hello Key Vault object creation fails with a HTTP status code of *Forbidden*.</span></span> <span data-ttu-id="c9e13-146">ilyen módon hello kulcsok gyorsítótárba kerüljenek-e a kulcstartót entitás tárolóval.</span><span class="sxs-lookup"><span data-stu-id="c9e13-146">hello keys listed in this fashion are cached with your key vault entity storage.</span></span> 

<span data-ttu-id="c9e13-147">Key Vault ellenőriznie kell, hogy rendelkezik-e hello identitás *újragenerálja* engedélyek előtt tulajdonjogát, a kulcsok újragenerálása is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="c9e13-147">Key Vault must verify that hello identity has *regenerate* permissions before it can take ownership of regenerating your keys.</span></span> <span data-ttu-id="c9e13-148">hello identitás OBO token keresztül, valamint a Key Vault első fél identitás hello tooverify rendelkezik ezekkel a jogosultságokkal:</span><span class="sxs-lookup"><span data-stu-id="c9e13-148">tooverify that hello identity, via OBO token, as well as hello Key Vault first party identity has these permissions:</span></span>

- <span data-ttu-id="c9e13-149">Key Vault hello tárolási fiók erőforrás RBAC engedélyeket sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="c9e13-149">Key Vault lists RBAC permissions on hello storage account resource.</span></span>
- <span data-ttu-id="c9e13-150">Key Vault hello válasz keresztül reguláris kifejezések egyeztetésének műveletek és a nem műveletek ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="c9e13-150">Key Vault validates hello response via regular expression matching of actions and non-actions.</span></span> 

<span data-ttu-id="c9e13-151">A támogató példákat található [Key Vault - kezelt tárolási fiók kulcsok minták](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/KeyVault/dataPlane/Microsoft.Azure.KeyVault.Samples/samples/HelloKeyVault/Program.cs#L167).</span><span class="sxs-lookup"><span data-stu-id="c9e13-151">Find some supporting examples at [Key Vault - Managed Storage Account Keys Samples](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/KeyVault/dataPlane/Microsoft.Azure.KeyVault.Samples/samples/HelloKeyVault/Program.cs#L167).</span></span>

<span data-ttu-id="c9e13-152">Ha hello identitása nem rendelkezik *újragenerálja* engedélyeket, vagy ha a Key Vault első fél identitása nem rendelkezik *lista* vagy *újragenerálja* engedélyt, majd hello bevezetése kérelem sikertelen lesz, egy megfelelő hibakód és az üzenet vissza.</span><span class="sxs-lookup"><span data-stu-id="c9e13-152">If hello identity does not have *regenerate* permissions or if Key Vault's first party identity doesn’t have *list* or *regenerate* permission, then hello onboarding request fails returning an appropriate error code and message.</span></span> 

<span data-ttu-id="c9e13-153">hello OBO jogkivonat-k, a PowerShell vagy a CLI natív ügyfélalkalmazások használatakor csak működik.</span><span class="sxs-lookup"><span data-stu-id="c9e13-153">hello OBO token will only work when you use first-party, native client applications of either PowerShell or CLI.</span></span>

## <a name="other-applications"></a><span data-ttu-id="c9e13-154">Más alkalmazások</span><span class="sxs-lookup"><span data-stu-id="c9e13-154">Other applications</span></span>

- <span data-ttu-id="c9e13-155">SAS-tokenje használatával a Key Vault tárfiókok kulcsait, még több ellenőrzött hozzáférés tooan Azure storage-fiókot adjon meg.</span><span class="sxs-lookup"><span data-stu-id="c9e13-155">SAS tokens, constructed using Key Vault storage account keys, provide even more controlled access tooan Azure storage account.</span></span> <span data-ttu-id="c9e13-156">További információkért lásd: [használata közös hozzáférésű jogosultságkód](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1).</span><span class="sxs-lookup"><span data-stu-id="c9e13-156">For more information, see [Using shared access signatures](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1).</span></span>

## <a name="see-also"></a><span data-ttu-id="c9e13-157">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="c9e13-157">See also</span></span>

- [<span data-ttu-id="c9e13-158">A kulcsok, titkos kódok és tanúsítványok ismertetése</span><span class="sxs-lookup"><span data-stu-id="c9e13-158">About keys, secrets, and certificates</span></span>](https://docs.microsoft.com/rest/api/keyvault/)
- [<span data-ttu-id="c9e13-159">Kulcstároló csapat blogja</span><span class="sxs-lookup"><span data-stu-id="c9e13-159">Key Vault Team Blog</span></span>](https://blogs.technet.microsoft.com/kv/)
