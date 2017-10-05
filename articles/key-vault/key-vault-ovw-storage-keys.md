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
# <a name="azure-key-vault-storage-account-keys"></a><span data-ttu-id="adde8-103">Az Azure Key Vault Tárfiókkulcsok</span><span class="sxs-lookup"><span data-stu-id="adde8-103">Azure Key Vault Storage Account Keys</span></span>

<span data-ttu-id="adde8-104">Azure Key Vault Tárfiókok kulcsait, mielőtt a fejlesztők kellett a saját Azure Storage-fiók (ASA) kulcsok kezelése és elforgatása őket manuálisan vagy egy külső automation.</span><span class="sxs-lookup"><span data-stu-id="adde8-104">Before Azure Key Vault Storage Account Keys, developers had to manage their own Azure Storage Account (ASA) keys and rotate them manually or through an external automation.</span></span> <span data-ttu-id="adde8-105">Most, a Key Vault Tárfiókok kulcsait, megvalósítva [Key Vault titkok](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates#BKMK_WorkingWithSecrets) hitelesítéséhez az Azure Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="adde8-105">Now, Key Vault Storage Account Keys are implemented as [Key Vault secrets](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates#BKMK_WorkingWithSecrets) for authenticating with an Azure Storage Account.</span></span> 

<span data-ttu-id="adde8-106">Az ASA alapfunkciója kezeli az Ön titkos elforgatás, és többé nem kell a közvetlen forduljon ASA kulccsal rendelkező megosztott hozzáférési aláírásokkal (SAS) módszerként felajánlásával.</span><span class="sxs-lookup"><span data-stu-id="adde8-106">The ASA key feature manages secret rotation for you and removes the need for your direct contact with an ASA key by offering Shared Access Signatures (SAS) as a method.</span></span> 

<span data-ttu-id="adde8-107">Az Azure Storage-fiókokról további általános információkért lásd: [tudnivalók az Azure storage-fiókok](https://docs.microsoft.com/azure/storage/storage-create-storage-account).</span><span class="sxs-lookup"><span data-stu-id="adde8-107">For more general information on Azure Storage Accounts, see [About Azure storage accounts](https://docs.microsoft.com/azure/storage/storage-create-storage-account).</span></span>

## <a name="supporting-interfaces"></a><span data-ttu-id="adde8-108">Kapcsolatok támogatása</span><span class="sxs-lookup"><span data-stu-id="adde8-108">Supporting interfaces</span></span>

<span data-ttu-id="adde8-109">Az Azure Storage-fiók kulcsok funkció kezdetben érhető el a többi, .NET útján / C# és PowerShell-felületek.</span><span class="sxs-lookup"><span data-stu-id="adde8-109">The Azure Storage Account keys feature is initially available through the REST, .NET/C# and PowerShell interfaces.</span></span> <span data-ttu-id="adde8-110">További információkért lásd: [Key Vault hivatkozás](https://docs.microsoft.com/azure/key-vault/).</span><span class="sxs-lookup"><span data-stu-id="adde8-110">For more information, see [Key Vault Reference](https://docs.microsoft.com/azure/key-vault/).</span></span>


## <a name="storage-account-keys-behavior"></a><span data-ttu-id="adde8-111">Tárolási fiók kulcsok viselkedése</span><span class="sxs-lookup"><span data-stu-id="adde8-111">Storage account keys behavior</span></span>

### <a name="what-key-vault-manages"></a><span data-ttu-id="adde8-112">Key Vault kezeli</span><span class="sxs-lookup"><span data-stu-id="adde8-112">What Key Vault manages</span></span>

<span data-ttu-id="adde8-113">Key Vault több belső felügyeleti funkciókat az Ön nevében hajtja végre a Tárfiókkulcsok használatakor.</span><span class="sxs-lookup"><span data-stu-id="adde8-113">Key Vault performs several internal management functions on your behalf when you use Storage Account Keys.</span></span>

1. <span data-ttu-id="adde8-114">Az Azure Key Vault kezeli a kulcsokat egy Azure Storage figyelembe (ASA).</span><span class="sxs-lookup"><span data-stu-id="adde8-114">Azure Key Vault manages keys of an Azure Storage Account (ASA).</span></span> 
    - <span data-ttu-id="adde8-115">Belsőleg az Azure Key Vault készíthetünk egy Azure Storage-fiók (sync) kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="adde8-115">Internally, Azure Key Vault can list (sync) keys with an Azure Storage Account.</span></span>  
    - <span data-ttu-id="adde8-116">Az Azure Key Vault újragenerálja (forog) a kulcsok rendszeres időközönként.</span><span class="sxs-lookup"><span data-stu-id="adde8-116">Azure Key Vault regenerates (rotates) the keys periodically.</span></span> 
    - <span data-ttu-id="adde8-117">Értékek soha nem visszakerülnek a hívó adott válaszként.</span><span class="sxs-lookup"><span data-stu-id="adde8-117">Key values are never returned in response to caller.</span></span> 
    - <span data-ttu-id="adde8-118">Az Azure Key Vault kezeli a Storage-fiókok és a klasszikus Tárfiókokat kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="adde8-118">Azure Key Vault manages keys of both Storage Accounts and Classic Storage Accounts.</span></span> 
2. <span data-ttu-id="adde8-119">Az Azure Key Vault lehetővé teszi, a tároló/objektum tulajdonosa, SAS (fiók vagy szolgáltatás SAS)-meghatározások létrehozása.</span><span class="sxs-lookup"><span data-stu-id="adde8-119">Azure Key Vault allows you, the vault/object owner, to create SAS (account or service SAS) definitions.</span></span> 
    - <span data-ttu-id="adde8-120">A SAS értéket, SAS-definícióban használatával létrehozott egy titkos kulcsként adja vissza a többi URI elérési útján.</span><span class="sxs-lookup"><span data-stu-id="adde8-120">The SAS value, created using SAS definition, is returned as a secret via the REST URI path.</span></span> <span data-ttu-id="adde8-121">További információkért lásd: [Azure Key Vault tárolási fiók műveletek](https://docs.microsoft.com/rest/api/keyvault/storage-account-key-operations).</span><span class="sxs-lookup"><span data-stu-id="adde8-121">For more information, see [Azure Key Vault storage account operations](https://docs.microsoft.com/rest/api/keyvault/storage-account-key-operations).</span></span>

### <a name="naming-guidance"></a><span data-ttu-id="adde8-122">Elnevezési irányelvei</span><span class="sxs-lookup"><span data-stu-id="adde8-122">Naming guidance</span></span>

<span data-ttu-id="adde8-123">A tárfiókok neve 3–24 karakter hosszúságú lehet, és csak számokból és kisbetűkből állhat.</span><span class="sxs-lookup"><span data-stu-id="adde8-123">Storage account names must be between 3 and 24 characters in length and may contain numbers and lowercase letters only.</span></span>  
 
<span data-ttu-id="adde8-124">A SAS-definíció nevét 1-102 karakter hosszúságú-csak 0-9, a – z, A-Z kell lennie.</span><span class="sxs-lookup"><span data-stu-id="adde8-124">A SAS definition name must be 1-102 characters in length containing only 0-9, a-z, A-Z.</span></span>

## <a name="developer-experience"></a><span data-ttu-id="adde8-125">A fejlesztői változat</span><span class="sxs-lookup"><span data-stu-id="adde8-125">Developer experience</span></span> 

### <a name="before-azure-key-vault-storage-keys"></a><span data-ttu-id="adde8-126">Az Azure Key Vault tárolási kulcsok előtt</span><span class="sxs-lookup"><span data-stu-id="adde8-126">Before Azure Key Vault Storage Keys</span></span> 

<span data-ttu-id="adde8-127">A fejlesztők kell tennie az alábbi eljárásokkal a tárfiók kulcsa a használatával is elérheti az Azure storage.</span><span class="sxs-lookup"><span data-stu-id="adde8-127">Developers used to need to do the following practices with a storage account key to get access to Azure storage.</span></span> 
 
 ```
//create storage account using connection string containing account name 
// and the storage key 

var storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));
var blobClient = storageAccount.CreateCloudBlobClient();
 ```
 
### <a name="after-azure-key-vault-storage-keys"></a><span data-ttu-id="adde8-128">Az Azure Key Vault tárolási kulcsok után</span><span class="sxs-lookup"><span data-stu-id="adde8-128">After Azure Key Vault Storage Keys</span></span> 

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
 
 ### <a name="developer-best-practices"></a><span data-ttu-id="adde8-129">Fejlesztői gyakorlati tanácsok</span><span class="sxs-lookup"><span data-stu-id="adde8-129">Developer best practices</span></span> 

- <span data-ttu-id="adde8-130">Engedélyezése csak a Key Vault a ASA kulcsok kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="adde8-130">Only allow Key Vault to manage your ASA keys.</span></span> <span data-ttu-id="adde8-131">Ne kísérelje meg magának kezelhetők, a Key Vault folyamatok zavarja meg.</span><span class="sxs-lookup"><span data-stu-id="adde8-131">Do not attempt to manage them yourself, you will interfere with Key Vault's processes.</span></span> 
- <span data-ttu-id="adde8-132">Több Key Vault objektum által kezelt ASA kulcsok nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="adde8-132">Do not allow ASA keys to be managed by more than one Key Vault object.</span></span> 
- <span data-ttu-id="adde8-133">Manuálisan újragenerálja a ASA kulcsok van szüksége, azt javasoljuk keresztül Key Vault generálni.</span><span class="sxs-lookup"><span data-stu-id="adde8-133">If you need to manually regenerate your ASA keys, we recommend that you regenerate them via Key Vault.</span></span> 

## <a name="getting-started"></a><span data-ttu-id="adde8-134">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="adde8-134">Getting started</span></span>

### <a name="setup-for-role-based-access-control-rbac-permissions"></a><span data-ttu-id="adde8-135">A szerepköralapú hozzáférés-vezérlést (RBAC) engedélyek beállítása</span><span class="sxs-lookup"><span data-stu-id="adde8-135">Setup for role-based access control (RBAC) permissions</span></span>

<span data-ttu-id="adde8-136">Key Vault engedélyeket kell *lista* és *újragenerálja* tárfiókok esetén a kulcsok.</span><span class="sxs-lookup"><span data-stu-id="adde8-136">Key Vault needs permissions to *list* and *regenerate* keys for a storage account.</span></span> <span data-ttu-id="adde8-137">Állítsa be ezeket az engedélyeket az alábbi lépéseket követve:</span><span class="sxs-lookup"><span data-stu-id="adde8-137">Set up these permissions using the following steps:</span></span>

- <span data-ttu-id="adde8-138">A kulcstároló ObjectId beolvasása:</span><span class="sxs-lookup"><span data-stu-id="adde8-138">Get ObjectId of Key Vault:</span></span> 

    `Get-AzureRmADServicePrincipal -SearchString "AzureKeyVault"`

- <span data-ttu-id="adde8-139">Tárolási kulcs kezelői szerepkör hozzárendelése az Azure Key Vault Identity:</span><span class="sxs-lookup"><span data-stu-id="adde8-139">Assign Storage Key Operator role to Azure Key Vault Identity:</span></span> 

    `New-AzureRmRoleAssignment -ObjectId <objectId of AzureKeyVault from previous command> -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope '<azure resource id of storage account>'`

    >[!NOTE]
    > <span data-ttu-id="adde8-140">A klasszikus fióktípus be a szerepkör-paraméter *"Klasszikus tárolási fiók kulcs operátori szerepkör-szolgáltatás"*.</span><span class="sxs-lookup"><span data-stu-id="adde8-140">For a classic account type, set the role parameter to *"Classic Storage Account Key Operator Service Role"*.</span></span>

### <a name="storage-account-onboarding"></a><span data-ttu-id="adde8-141">Tárolási fiók bevezetése</span><span class="sxs-lookup"><span data-stu-id="adde8-141">Storage account onboarding</span></span> 

<span data-ttu-id="adde8-142">Példa: tulajdonosaként Key Vault objektumot ad hozzá egy tárolóobjektumot fiók az Azure Key Vault szolgáltatás bevezetésében egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="adde8-142">Example: As a Key Vault object owner you add a storage account object to your Azure Key Vault to onboard a storage account.</span></span>

<span data-ttu-id="adde8-143">Bevezetési, során Key Vault kell győződjön meg arról, hogy a bevezetési fiók identitását jogosult legyen *lista* , és *újragenerálja* tárolási kulcsok.</span><span class="sxs-lookup"><span data-stu-id="adde8-143">During onboarding, Key Vault needs to verify that the identity of the onboarding account has permissions to *list* and to *regenerate* storage keys.</span></span> <span data-ttu-id="adde8-144">Ahhoz, hogy ezek az engedélyek ellenőrzéséhez a Key Vault lekérése egy OBO (a nevében a) token a hitelesítési szolgáltatás, állítsa be az Azure Resource Manager célközönség, és lehetővé teszi egy *lista* kulcsát az Azure Storage szolgáltatás hívása.</span><span class="sxs-lookup"><span data-stu-id="adde8-144">In order to verify these permissions, Key Vault gets an OBO (On Behalf Of) token from the authentication service, audience set to Azure Resource Manager, and makes a *list* key call to the Azure Storage service.</span></span> <span data-ttu-id="adde8-145">Ha a *lista* hívás sikertelen lesz, az objektum-létrehozás sikertelen, és a HTTP-állapotkód: a Key Vault *tiltott*.</span><span class="sxs-lookup"><span data-stu-id="adde8-145">If the *list* call fails, the Key Vault object creation fails with a HTTP status code of *Forbidden*.</span></span> <span data-ttu-id="adde8-146">Ilyen módon szereplő kulcsok gyorsítótárba kerüljenek-e a kulcstartót entitás tárolóval.</span><span class="sxs-lookup"><span data-stu-id="adde8-146">The keys listed in this fashion are cached with your key vault entity storage.</span></span> 

<span data-ttu-id="adde8-147">Key Vault ellenőriznie kell, hogy rendelkezik-e az identity *újragenerálja* engedélyek előtt tulajdonjogát, a kulcsok újragenerálása is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="adde8-147">Key Vault must verify that the identity has *regenerate* permissions before it can take ownership of regenerating your keys.</span></span> <span data-ttu-id="adde8-148">Győződjön meg arról, hogy rendelkezik-e ezeket az engedélyeket az identitást, keresztül OBO jogkivonatot, valamint a Key Vault első fél identitás: a</span><span class="sxs-lookup"><span data-stu-id="adde8-148">To verify that the identity, via OBO token, as well as the Key Vault first party identity has these permissions:</span></span>

- <span data-ttu-id="adde8-149">Key Vault a tárolási fiók erőforrás RBAC engedélyeinek sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="adde8-149">Key Vault lists RBAC permissions on the storage account resource.</span></span>
- <span data-ttu-id="adde8-150">Key Vault érvényesíti a válaszból reguláris kifejezések egyeztetésének műveletek és a nem műveletek keresztül.</span><span class="sxs-lookup"><span data-stu-id="adde8-150">Key Vault validates the response via regular expression matching of actions and non-actions.</span></span> 

<span data-ttu-id="adde8-151">A támogató példákat található [Key Vault - kezelt tárolási fiók kulcsok minták](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/KeyVault/dataPlane/Microsoft.Azure.KeyVault.Samples/samples/HelloKeyVault/Program.cs#L167).</span><span class="sxs-lookup"><span data-stu-id="adde8-151">Find some supporting examples at [Key Vault - Managed Storage Account Keys Samples](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/KeyVault/dataPlane/Microsoft.Azure.KeyVault.Samples/samples/HelloKeyVault/Program.cs#L167).</span></span>

<span data-ttu-id="adde8-152">Ha az identitás nem rendelkezik *újragenerálja* engedélyeket, vagy ha a Key Vault első fél identitása nem rendelkezik *lista* vagy *újragenerálja* engedélyt, majd a Bevezetés kérelem sikertelen lesz, egy megfelelő hibakód és az üzenet vissza.</span><span class="sxs-lookup"><span data-stu-id="adde8-152">If the identity does not have *regenerate* permissions or if Key Vault's first party identity doesn’t have *list* or *regenerate* permission, then the onboarding request fails returning an appropriate error code and message.</span></span> 

<span data-ttu-id="adde8-153">A OBO jogkivonat-k, a PowerShell vagy a CLI natív ügyfélalkalmazások használatakor csak működik.</span><span class="sxs-lookup"><span data-stu-id="adde8-153">The OBO token will only work when you use first-party, native client applications of either PowerShell or CLI.</span></span>

## <a name="other-applications"></a><span data-ttu-id="adde8-154">Más alkalmazások</span><span class="sxs-lookup"><span data-stu-id="adde8-154">Other applications</span></span>

- <span data-ttu-id="adde8-155">SAS-tokenje használatával a Key Vault tárfiókok kulcsait, az Azure-tárfiók még több ellenőrzött hozzáférést biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="adde8-155">SAS tokens, constructed using Key Vault storage account keys, provide even more controlled access to an Azure storage account.</span></span> <span data-ttu-id="adde8-156">További információkért lásd: [használata közös hozzáférésű jogosultságkód](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1).</span><span class="sxs-lookup"><span data-stu-id="adde8-156">For more information, see [Using shared access signatures](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1).</span></span>

## <a name="see-also"></a><span data-ttu-id="adde8-157">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="adde8-157">See also</span></span>

- [<span data-ttu-id="adde8-158">A kulcsok, titkos kódok és tanúsítványok ismertetése</span><span class="sxs-lookup"><span data-stu-id="adde8-158">About keys, secrets, and certificates</span></span>](https://docs.microsoft.com/rest/api/keyvault/)
- [<span data-ttu-id="adde8-159">Kulcstároló csapat blogja</span><span class="sxs-lookup"><span data-stu-id="adde8-159">Key Vault Team Blog</span></span>](https://blogs.technet.microsoft.com/kv/)
