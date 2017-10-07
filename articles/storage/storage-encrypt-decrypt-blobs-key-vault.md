---
title: "Oktatóanyag: Titkosításához és visszafejtéséhez az Azure Key Vault használatával Azure Storage blobs is |} Microsoft Docs"
description: "Hogyan tooencrypt és fejti vissza a Microsoft Azure tárolás az Azure Key Vault ügyféloldali titkosítással blob."
services: storage
documentationcenter: 
author: adhurwit
manager: jasonsav
editor: tysonn
ms.assetid: 027e8631-c1bf-48c1-9d9b-f6843e88b583
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 01/23/2017
ms.author: adhurwit
ms.openlocfilehash: 3eb64f104f378dd09ef295c94e03167655883391
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-encrypt-and-decrypt-blobs-in-microsoft-azure-storage-using-azure-key-vault"></a><span data-ttu-id="c56c9-103">Oktatóanyag: Titkosításához és visszafejtéséhez az Azure Key Vault használatával a Microsoft Azure Storage blobs</span><span class="sxs-lookup"><span data-stu-id="c56c9-103">Tutorial: Encrypt and decrypt blobs in Microsoft Azure Storage using Azure Key Vault</span></span>
## <a name="introduction"></a><span data-ttu-id="c56c9-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="c56c9-104">Introduction</span></span>
<span data-ttu-id="c56c9-105">Ez az oktatóanyag ismerteti, hogyan toomake használata az Azure Key Vault ügyféloldali tárolás titkosítása.</span><span class="sxs-lookup"><span data-stu-id="c56c9-105">This tutorial covers how toomake use of client-side storage encryption with Azure Key Vault.</span></span> <span data-ttu-id="c56c9-106">Az végigvezeti tooencrypt és egy konzolalkalmazásban ezeknek a technológiáknak blob visszafejtéséhez.</span><span class="sxs-lookup"><span data-stu-id="c56c9-106">It walks you through how tooencrypt and decrypt a blob in a console application using these technologies.</span></span>

<span data-ttu-id="c56c9-107">**Becsült idő toocomplete:** 20 perc</span><span class="sxs-lookup"><span data-stu-id="c56c9-107">**Estimated time toocomplete:** 20 minutes</span></span>

<span data-ttu-id="c56c9-108">Áttekintés az Azure Key Vault kapcsolatos információkért lásd: [Mi az Azure Key Vault?](../key-vault/key-vault-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c56c9-108">For overview information about Azure Key Vault, see [What is Azure Key Vault?](../key-vault/key-vault-whatis.md).</span></span>

<span data-ttu-id="c56c9-109">Áttekintés az Azure Storage ügyféloldali titkosítással kapcsolatos információkért lásd: [ügyféloldali titkosítás és a Microsoft Azure tárolás az Azure Key Vault](storage-client-side-encryption.md).</span><span class="sxs-lookup"><span data-stu-id="c56c9-109">For overview information about client-side encryption for Azure Storage, see [Client-Side Encryption and Azure Key Vault for Microsoft Azure Storage](storage-client-side-encryption.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c56c9-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c56c9-110">Prerequisites</span></span>
<span data-ttu-id="c56c9-111">toocomplete ebben az oktatóanyagban rendelkeznie kell a következő hello:</span><span class="sxs-lookup"><span data-stu-id="c56c9-111">toocomplete this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="c56c9-112">Egy Azure Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="c56c9-112">An Azure Storage account</span></span>
* <span data-ttu-id="c56c9-113">A Visual Studio 2013 vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="c56c9-113">Visual Studio 2013 or later</span></span>
* <span data-ttu-id="c56c9-114">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="c56c9-114">Azure PowerShell</span></span>

## <a name="overview-of-client-side-encryption"></a><span data-ttu-id="c56c9-115">Ügyféloldali titkosítás áttekintése</span><span class="sxs-lookup"><span data-stu-id="c56c9-115">Overview of client-side encryption</span></span>
<span data-ttu-id="c56c9-116">Az Azure Storage ügyféloldali titkosítás áttekintéséért lásd: [ügyféloldali titkosítás és a Microsoft Azure tárolás az Azure Key Vault](storage-client-side-encryption.md)</span><span class="sxs-lookup"><span data-stu-id="c56c9-116">For an overview of client-side encryption for Azure Storage, see [Client-Side Encryption and Azure Key Vault for Microsoft Azure Storage](storage-client-side-encryption.md)</span></span>

<span data-ttu-id="c56c9-117">Ügyféloldali titkosítása működése rövid leírása itt található:</span><span class="sxs-lookup"><span data-stu-id="c56c9-117">Here is a brief description of how client side encryption works:</span></span>

1. <span data-ttu-id="c56c9-118">hello Azure Storage ügyfél SDK állít elő, a tartalom titkosítási kulcs (CEK), amely a szimmetrikus kulcs egy egyszeri használata.</span><span class="sxs-lookup"><span data-stu-id="c56c9-118">hello Azure Storage client SDK generates a content encryption key (CEK), which is a one-time-use symmetric key.</span></span>
2. <span data-ttu-id="c56c9-119">Felhasználói adatok titkosítása a CEK használatával.</span><span class="sxs-lookup"><span data-stu-id="c56c9-119">Customer data is encrypted using this CEK.</span></span>
3. <span data-ttu-id="c56c9-120">hello CEK ezt követően (titkosított) hello kulcs titkosítási kulcs (KEK) használatával.</span><span class="sxs-lookup"><span data-stu-id="c56c9-120">hello CEK is then wrapped (encrypted) using hello key encryption key (KEK).</span></span> <span data-ttu-id="c56c9-121">hello KEK kulcsazonosítójával azonosíthatók és aszimmetrikus kulcspár vagy egy szimmetrikus kulcsot kell és is kezelhetők helyileg vagy az Azure Key Vault tárolja.</span><span class="sxs-lookup"><span data-stu-id="c56c9-121">hello KEK is identified by a key identifier and can be an asymmetric key pair or a symmetric key and can be managed locally or stored in Azure Key Vault.</span></span> <span data-ttu-id="c56c9-122">hello tárolási ügyfélen soha nem hozzáférés toohello KEK.</span><span class="sxs-lookup"><span data-stu-id="c56c9-122">hello Storage client itself never has access toohello KEK.</span></span> <span data-ttu-id="c56c9-123">Egyszerűen meghívja a Key Vault által biztosított hello kulcs alkalmazásburkoló algoritmus.</span><span class="sxs-lookup"><span data-stu-id="c56c9-123">It just invokes hello key wrapping algorithm that is provided by Key Vault.</span></span> <span data-ttu-id="c56c9-124">Az ügyfelek választható toouse egyéni szolgáltatók alkalmazásburkoló/kicsomagolásával Ha szeretné, hogy azok kulcshoz.</span><span class="sxs-lookup"><span data-stu-id="c56c9-124">Customers can choose toouse custom providers for key wrapping/unwrapping if they want.</span></span>
4. <span data-ttu-id="c56c9-125">hello titkosított adatok van majd toohello Azure Storage szolgáltatás feltöltve.</span><span class="sxs-lookup"><span data-stu-id="c56c9-125">hello encrypted data is then uploaded toohello Azure Storage service.</span></span>

## <a name="set-up-your-azure-key-vault"></a><span data-ttu-id="c56c9-126">Az Azure Key Vault beállítása</span><span class="sxs-lookup"><span data-stu-id="c56c9-126">Set up your Azure Key Vault</span></span>
<span data-ttu-id="c56c9-127">Rendelés tooproceed az oktatóanyaghoz, a következő lépésekkel, amelyek hello oktatóprogram vázolt toodo hello kell [Ismerkedés az Azure Key Vault](../key-vault/key-vault-get-started.md):</span><span class="sxs-lookup"><span data-stu-id="c56c9-127">In order tooproceed with this tutorial, you need toodo hello following steps, which are outlined in hello tutorial  [Get started with Azure Key Vault](../key-vault/key-vault-get-started.md):</span></span>

* <span data-ttu-id="c56c9-128">Hozzon létre egy kulcstartót.</span><span class="sxs-lookup"><span data-stu-id="c56c9-128">Create a key vault.</span></span>
* <span data-ttu-id="c56c9-129">Vegye fel a kulcs vagy titkos toohello kulcstároló.</span><span class="sxs-lookup"><span data-stu-id="c56c9-129">Add a key or secret toohello key vault.</span></span>
* <span data-ttu-id="c56c9-130">Alkalmazás regisztrálása az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="c56c9-130">Register an application with Azure Active Directory.</span></span>
* <span data-ttu-id="c56c9-131">Engedélyezi a hello alkalmazás toouse hello kulcsok vagy titkos kulcsok.</span><span class="sxs-lookup"><span data-stu-id="c56c9-131">Authorize hello application toouse hello key or secret.</span></span>

<span data-ttu-id="c56c9-132">Jegyezze fel, hello ClientID és a ClientSecret, amikor egy alkalmazás regisztrálása az Azure Active Directoryval létrehozott ellenőrizze.</span><span class="sxs-lookup"><span data-stu-id="c56c9-132">Make note of hello ClientID and ClientSecret that were generated when registering an application with Azure Active Directory.</span></span>

<span data-ttu-id="c56c9-133">Mindkét kulcsot hello kulcstároló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c56c9-133">Create both keys in hello key vault.</span></span> <span data-ttu-id="c56c9-134">Feltételezzük, hogy a hello rest oktatóanyag hello használja a következő neveket hello: ContosoKeyVault és TestRSAKey1.</span><span class="sxs-lookup"><span data-stu-id="c56c9-134">We assume for hello rest of hello tutorial that you have used hello following names: ContosoKeyVault and TestRSAKey1.</span></span>

## <a name="create-a-console-application-with-packages-and-appsettings"></a><span data-ttu-id="c56c9-135">Hozzon létre egy konzolalkalmazást a csomagok és AppSettings</span><span class="sxs-lookup"><span data-stu-id="c56c9-135">Create a console application with packages and AppSettings</span></span>
<span data-ttu-id="c56c9-136">A Visual Studio új Konzolalkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c56c9-136">In Visual Studio, create a new console application.</span></span>

<span data-ttu-id="c56c9-137">Adja hozzá a szükséges nuget-csomagok a hello Csomagkezelő konzol.</span><span class="sxs-lookup"><span data-stu-id="c56c9-137">Add necessary nuget packages in hello Package Manager Console.</span></span>

```
Install-Package WindowsAzure.Storage

// This is hello latest stable release for ADAL.
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

Install-Package Microsoft.Azure.KeyVault
Install-Package Microsoft.Azure.KeyVault.Extensions
```

<span data-ttu-id="c56c9-138">Adja hozzá az AppSettings toohello App.Config.</span><span class="sxs-lookup"><span data-stu-id="c56c9-138">Add AppSettings toohello App.Config.</span></span>

```xml
<appSettings>
    <add key="accountName" value="myaccount"/>
    <add key="accountKey" value="theaccountkey"/>
    <add key="clientId" value="theclientid"/>
    <add key="clientSecret" value="theclientsecret"/>
    <add key="container" value="stuff"/>
</appSettings>
```

<span data-ttu-id="c56c9-139">Adja hozzá a következő hello `using` utasításokat, és győződjön meg arról, hogy tooadd egy hivatkozás tooSystem.Configuration toohello projektet.</span><span class="sxs-lookup"><span data-stu-id="c56c9-139">Add hello following `using` statements and make sure tooadd a reference tooSystem.Configuration toohello project.</span></span>

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System.Configuration;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.Azure.KeyVault;
using System.Threading;        
using System.IO;
```

## <a name="add-a-method-tooget-a-token-tooyour-console-application"></a><span data-ttu-id="c56c9-140">A metódus tooget token tooyour Konzolalkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c56c9-140">Add a method tooget a token tooyour console application</span></span>
<span data-ttu-id="c56c9-141">hello következő módszert használják kulcstároló olyan osztállyal, amelynek tooauthenticate szükséges hozzáférési tooyour kulcstároló.</span><span class="sxs-lookup"><span data-stu-id="c56c9-141">hello following method is used by Key Vault classes that need tooauthenticate for access tooyour key vault.</span></span>

```csharp
private async static Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);
    ClientCredential clientCred = new ClientCredential(
        ConfigurationManager.AppSettings["clientId"],
        ConfigurationManager.AppSettings["clientSecret"]);
    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)
        throw new InvalidOperationException("Failed tooobtain hello JWT token");

    return result.AccessToken;
}
```

## <a name="access-storage-and-key-vault-in-your-program"></a><span data-ttu-id="c56c9-142">Tárolás és a Key Vault hozzáférni a program</span><span class="sxs-lookup"><span data-stu-id="c56c9-142">Access Storage and Key Vault in your program</span></span>
<span data-ttu-id="c56c9-143">Hello Main függvényre adja hozzá a következő kód hello.</span><span class="sxs-lookup"><span data-stu-id="c56c9-143">In hello Main function, add hello following code.</span></span>

```csharp
// This is standard code toointeract with Blob storage.
StorageCredentials creds = new StorageCredentials(
    ConfigurationManager.AppSettings["accountName"],
       ConfigurationManager.AppSettings["accountKey"]);
CloudStorageAccount account = new CloudStorageAccount(creds, useHttps: true);
CloudBlobClient client = account.CreateCloudBlobClient();
CloudBlobContainer contain = client.GetContainerReference(ConfigurationManager.AppSettings["container"]);
contain.CreateIfNotExists();

// hello Resolver object is used toointeract with Key Vault for Azure Storage.
// This is where hello GetToken method from above is used.
KeyVaultKeyResolver cloudResolver = new KeyVaultKeyResolver(GetToken);
```

> [!NOTE]
> <span data-ttu-id="c56c9-144">Kulcstároló Hálózatiobjektum-modelljei</span><span class="sxs-lookup"><span data-stu-id="c56c9-144">Key Vault Object Models</span></span>
> 
> <span data-ttu-id="c56c9-145">Fontos, hogy vannak-e tényleges két Key Vault objektum toounderstand modellek tudomást toobe: egy REST API-t (névtér: KeyVault) hello alapul, és más hello ügyféloldali titkosítás modul bővítménye.</span><span class="sxs-lookup"><span data-stu-id="c56c9-145">It is important toounderstand that there are actually two Key Vault object models toobe aware of: one is based on hello REST API (KeyVault namespace) and hello other is an extension for client-side encryption.</span></span>
> 
> <span data-ttu-id="c56c9-146">hello Key Vault ügyfél kommunikál a hello REST API-t, és megértette a JSON webes kulcsok és titkos hello két dolog, amelyek szerepelnek a Key Vault típusú.</span><span class="sxs-lookup"><span data-stu-id="c56c9-146">hello Key Vault Client interacts with hello REST API and understands JSON Web Keys and secrets for hello two kinds of things that are contained in Key Vault.</span></span>
> 
> <span data-ttu-id="c56c9-147">hello Key Vault bővítmények olyan osztályok, úgy tűnik, az Azure Storage ügyféloldali titkosítás kifejezetten létrehozott.</span><span class="sxs-lookup"><span data-stu-id="c56c9-147">hello Key Vault Extensions are classes that seem specifically created for client-side encryption in Azure Storage.</span></span> <span data-ttu-id="c56c9-148">Kulcsok (IKey) és egy kulcs feloldó hello fogalma alapján osztályok illesztőfelület tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="c56c9-148">They contain an interface for keys (IKey) and classes based on hello concept of a Key Resolver.</span></span> <span data-ttu-id="c56c9-149">Nincsenek két implementációja, hogy kell-e tooknow IKey: RSAKey és SymmetricKey.</span><span class="sxs-lookup"><span data-stu-id="c56c9-149">There are two implementations of IKey that you need tooknow: RSAKey and SymmetricKey.</span></span> <span data-ttu-id="c56c9-150">Most akkor fordulhat elő, a hello dolog, amelyek szerepelnek a kulcstároló toocoincide, de ezen a ponton független osztályok (így hello kulcs és a titkos kulcsot tároló ügyfél hello által beolvasott nem valósítja meg a IKey).</span><span class="sxs-lookup"><span data-stu-id="c56c9-150">Now they happen toocoincide with hello things that are contained in a Key Vault, but at this point they are independent classes (so hello Key and Secret retrieved by hello Key Vault Client do not implement IKey).</span></span>
> 
> 

## <a name="encrypt-blob-and-upload"></a><span data-ttu-id="c56c9-151">A blob titkosításához, és töltse fel</span><span class="sxs-lookup"><span data-stu-id="c56c9-151">Encrypt blob and upload</span></span>
<span data-ttu-id="c56c9-152">Adja hozzá a hello alábbi tooencrypt blob code, és töltse fel az Azure storage-fiók tooyour.</span><span class="sxs-lookup"><span data-stu-id="c56c9-152">Add hello following code tooencrypt a blob and upload it tooyour Azure storage account.</span></span> <span data-ttu-id="c56c9-153">Hello **ResolveKeyAsync** használt módszer egy IKey adja vissza.</span><span class="sxs-lookup"><span data-stu-id="c56c9-153">hello **ResolveKeyAsync** method that is used returns an IKey.</span></span>

```csharp
// Retrieve hello key that you created previously.
// hello IKey that is returned here is an RsaKey.
// Remember that we used hello names contosokeyvault and testrsakey1.
var rsa = cloudResolver.ResolveKeyAsync("https://contosokeyvault.vault.azure.net/keys/TestRSAKey1", CancellationToken.None).GetAwaiter().GetResult();

// Now you simply use hello RSA key tooencrypt by setting it in hello BlobEncryptionPolicy.
BlobEncryptionPolicy policy = new BlobEncryptionPolicy(rsa, null);
BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

// Reference a block blob.
CloudBlockBlob blob = contain.GetBlockBlobReference("MyFile.txt");

// Upload using hello UploadFromStream method.
using (var stream = System.IO.File.OpenRead(@"C:\data\MyFile.txt"))
    blob.UploadFromStream(stream, stream.Length, null, options, null);
```

<span data-ttu-id="c56c9-154">Az alábbiakban látható egy képernyőkép hello [klasszikus Azure portál](https://manage.windowsazure.com) egy BLOB, a Key Vault szolgáltatókban tárolt kulcsok ügyféloldali titkosítással titkosított.</span><span class="sxs-lookup"><span data-stu-id="c56c9-154">Following is a screenshot from hello [Azure Classic Portal](https://manage.windowsazure.com) for a blob that has been encrypted by using client-side encryption with a key stored in Key Vault.</span></span> <span data-ttu-id="c56c9-155">Hello **KeyId** tulajdonsága hello URI kulcstároló, eleget kell hello KEK hello kulcsában.</span><span class="sxs-lookup"><span data-stu-id="c56c9-155">hello **KeyId** property is hello URI for hello key in Key Vault that acts as hello KEK.</span></span> <span data-ttu-id="c56c9-156">Hello **EncryptedKey** tulajdonság hello hello CEK titkosított verzióját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c56c9-156">hello **EncryptedKey** property contains hello encrypted version of hello CEK.</span></span>

![A Blob metaadatai paramétertitkosítási metaadatokat tartalmazó ábrázoló képernyőfelvétel](./media/storage-encrypt-decrypt-blobs-key-vault/blobmetadata.png)

> [!NOTE]
> <span data-ttu-id="c56c9-158">Ha hello BlobEncryptionPolicy konstruktor tekinti meg, látni fogja, hogy el tudja fogadni a kulcs és/vagy a feloldó.</span><span class="sxs-lookup"><span data-stu-id="c56c9-158">If you look at hello BlobEncryptionPolicy constructor, you will see that it can accept a key and/or a resolver.</span></span> <span data-ttu-id="c56c9-159">Vegye figyelembe, hogy most egy feloldó nem használata titkosításhoz, mert jelenleg nem támogatja egy alapértelmezett kulcs.</span><span class="sxs-lookup"><span data-stu-id="c56c9-159">Be aware that right now you cannot use a resolver for encryption because it does not currently support a default key.</span></span>
> 
> 

## <a name="decrypt-blob-and-download"></a><span data-ttu-id="c56c9-160">Blob visszafejtésére és letöltése</span><span class="sxs-lookup"><span data-stu-id="c56c9-160">Decrypt blob and download</span></span>
<span data-ttu-id="c56c9-161">Visszafejtési valóban amikor használatával hello feloldó osztályok értelmezhető.</span><span class="sxs-lookup"><span data-stu-id="c56c9-161">Decryption is really when using hello Resolver classes make sense.</span></span> <span data-ttu-id="c56c9-162">hello azonosító hello kulcs titkosításához használt társítva a metaadatokban hello blob, így nem ok arra, hogy tooretrieve hello kulcs, és ne feledje kulcs és a blob hello társítását.</span><span class="sxs-lookup"><span data-stu-id="c56c9-162">hello ID of hello key used for encryption is associated with hello blob in its metadata, so there is no reason for you tooretrieve hello key and remember hello association between key and blob.</span></span> <span data-ttu-id="c56c9-163">Most, hogy arra hello kulcs továbbra is a Key Vault toomake.</span><span class="sxs-lookup"><span data-stu-id="c56c9-163">You just have toomake sure that hello key remains in Key Vault.</span></span>   

<span data-ttu-id="c56c9-164">hello titkos kulcsot egy RSA-kulcs marad a kulcstároló, így a visszafejtési toooccur hello titkosított kulcs metaadatokból hello blob CEK küldött tooKey tároló visszafejtéshez hello tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="c56c9-164">hello private key of an RSA Key remains in Key Vault, so for decryption toooccur, hello Encrypted Key from hello blob metadata that contains hello CEK is sent tooKey Vault for decryption.</span></span>

<span data-ttu-id="c56c9-165">Adja hozzá a következő toodecrypt hello blob imént feltöltött hello.</span><span class="sxs-lookup"><span data-stu-id="c56c9-165">Add hello following toodecrypt hello blob that you just uploaded.</span></span>

```csharp
// In this case, we will not pass a key and only pass hello resolver because
// this policy will only be used for downloading / decrypting.
BlobEncryptionPolicy policy = new BlobEncryptionPolicy(null, cloudResolver);
BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

using (var np = File.Open(@"C:\data\MyFileDecrypted.txt", FileMode.Create))
    blob.DownloadToStream(np, null, options, null);
```

> [!NOTE]
> <span data-ttu-id="c56c9-166">Többféle feloldókat toomake kulcskezelés más típusú egyszerűbb, többek között: AggregateKeyResolver és CachingKeyResolver.</span><span class="sxs-lookup"><span data-stu-id="c56c9-166">There are a couple of other kinds of resolvers toomake key management easier, including: AggregateKeyResolver and CachingKeyResolver.</span></span>
> 
> 

## <a name="use-key-vault-secrets"></a><span data-ttu-id="c56c9-167">Key Vault titkos kulcsok használata</span><span class="sxs-lookup"><span data-stu-id="c56c9-167">Use Key Vault secrets</span></span>
<span data-ttu-id="c56c9-168">hello módon toouse ügyféloldali titkosítással titkos kulcs oka keresztül hello SymmetricKey osztály titkos kulcs lényegében egy szimmetrikus kulcsot.</span><span class="sxs-lookup"><span data-stu-id="c56c9-168">hello way toouse a secret with client-side encryption is via hello SymmetricKey class because a secret is essentially a symmetric key.</span></span> <span data-ttu-id="c56c9-169">De a fentieknek megfelelően a Key Vault titkos kulcs nem felel meg pontosan tooa SymmetricKey.</span><span class="sxs-lookup"><span data-stu-id="c56c9-169">But, as noted above, a secret in Key Vault does not map exactly tooa SymmetricKey.</span></span> <span data-ttu-id="c56c9-170">Van néhány dolog toounderstand:</span><span class="sxs-lookup"><span data-stu-id="c56c9-170">There are a few things toounderstand:</span></span>

* <span data-ttu-id="c56c9-171">egy SymmetricKey hello kulcs hossza toobe rögzített: 128, 192, 256-ot, 384 vagy 512 bites.</span><span class="sxs-lookup"><span data-stu-id="c56c9-171">hello key in a SymmetricKey has toobe a fixed length: 128, 192, 256, 384, or 512 bits.</span></span>
* <span data-ttu-id="c56c9-172">egy SymmetricKey hello kulcsot kell Base64-kódolású.</span><span class="sxs-lookup"><span data-stu-id="c56c9-172">hello key in a SymmetricKey should be Base64 encoded.</span></span>
* <span data-ttu-id="c56c9-173">A Key Vault titkos kulcsot, amely egy SymmetricKey lesz kell toohave egy "application/octet-stream" a Key Vault tartalomtípusa.</span><span class="sxs-lookup"><span data-stu-id="c56c9-173">A Key Vault secret that will be used as a SymmetricKey needs toohave a Content Type of "application/octet-stream" in Key Vault.</span></span>

<span data-ttu-id="c56c9-174">Íme egy példa a titkos kulcs létrehozásának a kulcstároló, egy SymmetricKey használható PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c56c9-174">Here is an example in PowerShell of creating a secret in Key Vault that can be used as a SymmetricKey.</span></span>
<span data-ttu-id="c56c9-175">Vegye figyelembe, hogy hello rögzített kódolt, $key, értéke csak bemutató célra.</span><span class="sxs-lookup"><span data-stu-id="c56c9-175">Please note that hello hard coded value, $key, is for demonstration purpose only.</span></span> <span data-ttu-id="c56c9-176">A kód célszerű toogenerate ezt a kulcsot.</span><span class="sxs-lookup"><span data-stu-id="c56c9-176">In your own code you'll want toogenerate this key.</span></span>

```csharp
// Here we are making a 128-bit key so we have 16 characters.
//     hello characters are in hello ASCII range of UTF8 so they are
//    each 1 byte. 16 x 8 = 128.
$key = "qwertyuiopasdfgh"
$b = [System.Text.Encoding]::UTF8.GetBytes($key)
$enc = [System.Convert]::ToBase64String($b)
$secretvalue = ConvertTo-SecureString $enc -AsPlainText -Force

// Substitute hello VaultName and Name in this command.
$secret = Set-AzureKeyVaultSecret -VaultName 'ContoseKeyVault' -Name 'TestSecret2' -SecretValue $secretvalue -ContentType "application/octet-stream"
```

<span data-ttu-id="c56c9-177">A Konzolalkalmazás azonos hívás tooretrieve előtt a titkos, mint egy SymmetricKey hello is használhatja.</span><span class="sxs-lookup"><span data-stu-id="c56c9-177">In your console application, you can use hello same call as before tooretrieve this secret as a SymmetricKey.</span></span>

```csharp
SymmetricKey sec = (SymmetricKey) cloudResolver.ResolveKeyAsync(
    "https://contosokeyvault.vault.azure.net/secrets/TestSecret2/",
    CancellationToken.None).GetAwaiter().GetResult();
```
<span data-ttu-id="c56c9-178">Ennyi az egész.</span><span class="sxs-lookup"><span data-stu-id="c56c9-178">That's it.</span></span> <span data-ttu-id="c56c9-179">Jó munkát!</span><span class="sxs-lookup"><span data-stu-id="c56c9-179">Enjoy!</span></span>

## <a name="next-steps"></a><span data-ttu-id="c56c9-180">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c56c9-180">Next steps</span></span>
<span data-ttu-id="c56c9-181">Microsoft Azure Storage a C# használatával kapcsolatos további információkért lásd: [Microsoft Azure Storage ügyféloldali kódtára a .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span><span class="sxs-lookup"><span data-stu-id="c56c9-181">For more information about using Microsoft Azure Storage with C#, see [Microsoft Azure Storage Client Library for .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span></span>

<span data-ttu-id="c56c9-182">Hello Blob REST API kapcsolatos további információkért lásd: [Blob szolgáltatás REST API](https://msdn.microsoft.com/library/azure/dd135733.aspx).</span><span class="sxs-lookup"><span data-stu-id="c56c9-182">For more information about hello Blob REST API, see [Blob Service REST API](https://msdn.microsoft.com/library/azure/dd135733.aspx).</span></span>

<span data-ttu-id="c56c9-183">Hello legújabb információ a Microsoft Azure Storage: toohello [Microsoft Azure tárolás fejlesztői Blog](http://blogs.msdn.com/b/windowsazurestorage/).</span><span class="sxs-lookup"><span data-stu-id="c56c9-183">For hello latest information on Microsoft Azure Storage, go toohello [Microsoft Azure Storage Team Blog](http://blogs.msdn.com/b/windowsazurestorage/).</span></span>
