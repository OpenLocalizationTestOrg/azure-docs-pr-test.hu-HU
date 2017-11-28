---
title: "Kulcs tároló .NET 2.x API kibocsátási megjegyzései |} Microsoft Docs"
description: ".NET-fejlesztők számára a kód API-t fogja használni az Azure Key Vault"
services: key-vault
author: BrucePerlerMS
manager: mbaldwin
editor: bruceper
ms.assetid: 1cccf21b-5be9-4a49-8145-483b695124ba
ms.service: key-vault
ms.devlang: CSharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/02/2017
ms.author: bruceper
ms.openlocfilehash: c5b5fd7f16faf17d16ecc82269fb1264adf4dd06
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-key-vault-net-20---release-notes-and-migration-guide"></a><span data-ttu-id="b61ee-103">Az Azure Key Vault .NET 2.0 – kibocsátási megjegyzések és az áttelepítési útmutató</span><span class="sxs-lookup"><span data-stu-id="b61ee-103">Azure Key Vault .NET 2.0 - Release Notes and Migration Guide</span></span>
<span data-ttu-id="b61ee-104">A következő notes és útmutatókat is Azure Key Vault .NET dolgozó fejlesztők számára / C# könyvtár.</span><span class="sxs-lookup"><span data-stu-id="b61ee-104">The following notes and guidance are for developers working with Azure Key Vault .NET / C# library.</span></span> <span data-ttu-id="b61ee-105">Az áttérést az 1.0-s verziója a 2.0-s verzióját, a frissítések száma végzett áttelepítés munkahelyi ahhoz, hogy igénybe vehesse az funkciók javításai, és kiegészítéseit, mint a beállítást, a kódban szükség teszi **Key Vault tanúsítványok** támogatja.</span><span class="sxs-lookup"><span data-stu-id="b61ee-105">In the transition from the 1.0 version to the 2.0 version, a number of updates have been made that will require migration work in your code in order for you to benefit from the functional improvements and feature additions such as **Key Vault certificates** support.</span></span>

## <a name="key-vault-certificates"></a><span data-ttu-id="b61ee-106">Key Vault tanúsítványok</span><span class="sxs-lookup"><span data-stu-id="b61ee-106">Key Vault certificates</span></span>

<span data-ttu-id="b61ee-107">Key Vault tanúsítványok támogatás lehetővé teszi a x509 vezetése a tanúsítványok és az alábbiak egyike:</span><span class="sxs-lookup"><span data-stu-id="b61ee-107">Key Vault certificates support provides for management of your x509 certificates and the following behaviors:</span></span>  

* <span data-ttu-id="b61ee-108">Lehetővé teszi, hogy a tanúsítvány tulajdonosa hozzon létre egy tanúsítványt, vagy a meglévő tanúsítvány importálása a Key Vault létrehozási folyamata keresztül.</span><span class="sxs-lookup"><span data-stu-id="b61ee-108">Allows a certificate owner to create a certificate through a Key Vault creation process or through the import of an existing certificate.</span></span> <span data-ttu-id="b61ee-109">Mindkét önaláírt és a hitelesítésszolgáltató tanúsítványokat jön létre.</span><span class="sxs-lookup"><span data-stu-id="b61ee-109">This includes both self-signed and Certificate Authority generated certificates.</span></span>
* <span data-ttu-id="b61ee-110">Lehetővé teszi, hogy a Key Vault tanúsítvány tulajdonosa biztonságos tárolása és kezelése X509 végrehajtásához tanúsítványok és titkos kulcs adatai közötti interakció nélkül.</span><span class="sxs-lookup"><span data-stu-id="b61ee-110">Allows a Key Vault certificate owner to implement secure storage and management of X509 certificates without interaction with private key material.</span></span>  
* <span data-ttu-id="b61ee-111">Lehetővé teszi, hogy a tanúsítvány tulajdonosának egy házirendet, amely arra utasítja a Key Vault kezeléséhez az életciklus-tanúsítvány létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="b61ee-111">Allows a certificate owner to create a policy that directs Key Vault to manage the life-cycle of a certificate.</span></span>  
* <span data-ttu-id="b61ee-112">Lehetővé teszi, hogy a tanúsítvány tulajdonos kapcsolattartási kapcsolatos adatok megadása az értesítéshez életciklus-események lejárati és a tanúsítvány megújítása.</span><span class="sxs-lookup"><span data-stu-id="b61ee-112">Allows certificate owners to provide contact information for notification about life-cycle events of expiration and renewal of certificate.</span></span>  
* <span data-ttu-id="b61ee-113">Támogatja az automatikus megújítását a kiválasztott kiállítók - Key Vault partner X509 szolgáltatók tanúsítvány / hitelesítésszolgáltatók.</span><span class="sxs-lookup"><span data-stu-id="b61ee-113">Supports automatic renewal with selected issuers - Key Vault partner X509 certificate providers / certificate authorities.</span></span>
  
  * <span data-ttu-id="b61ee-114">Megjegyzés: - nem közösen szolgáltatók/hitelesítésszolgáltatók is használhatók, de nem támogatja az automatikus megújítási szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="b61ee-114">NOTE - Non-partnered providers/authorities are also allowed but, will not support the auto renewal feature.</span></span>

## <a name="net-support"></a><span data-ttu-id="b61ee-115">.NET-támogatás</span><span class="sxs-lookup"><span data-stu-id="b61ee-115">.NET support</span></span>

* <span data-ttu-id="b61ee-116">**A .NET 4.0** az Azure Key Vault .NET 2.0-s verziója nem támogatja vagy C# könyvtár</span><span class="sxs-lookup"><span data-stu-id="b61ee-116">**.NET 4.0** is not supported by the 2.0 version of the Azure Key Vault .NET/C# library</span></span>
* <span data-ttu-id="b61ee-117">**A .NET core** az Azure Key Vault .NET 2.0-s verziója támogatja / C# könyvtár</span><span class="sxs-lookup"><span data-stu-id="b61ee-117">**.NET Core** is supported by the 2.0 version of the Azure Key Vault .NET/C# library</span></span>

## <a name="namespaces"></a><span data-ttu-id="b61ee-118">Névterek</span><span class="sxs-lookup"><span data-stu-id="b61ee-118">Namespaces</span></span>

* <span data-ttu-id="b61ee-119">A névtér **modellek** változtatják **Microsoft.Azure.KeyVault** való **Microsoft.Azure.KeyVault.Models**.</span><span class="sxs-lookup"><span data-stu-id="b61ee-119">The namespace for **models** is changed from **Microsoft.Azure.KeyVault** to **Microsoft.Azure.KeyVault.Models**.</span></span>
* <span data-ttu-id="b61ee-120">A **Microsoft.Azure.KeyVault.Internal** névtér megszakad.</span><span class="sxs-lookup"><span data-stu-id="b61ee-120">The **Microsoft.Azure.KeyVault.Internal** namespace is dropped.</span></span>
* <span data-ttu-id="b61ee-121">Az Azure SDK függőségek névtér nem változtatják **Hyak.Common** és **Hyak.Common.Internals** való **Microsoft.Rest** és **Microsoft.Rest.Serialization**</span><span class="sxs-lookup"><span data-stu-id="b61ee-121">The Azure SDK dependencies namespace are changed from **Hyak.Common** and **Hyak.Common.Internals** to **Microsoft.Rest** and **Microsoft.Rest.Serialization**</span></span>

## <a name="type-changes"></a><span data-ttu-id="b61ee-122">Módosításai</span><span class="sxs-lookup"><span data-stu-id="b61ee-122">Type changes</span></span>

* <span data-ttu-id="b61ee-123">*Titkos kulcs* változott *SecretBundle*</span><span class="sxs-lookup"><span data-stu-id="b61ee-123">*Secret* changed to *SecretBundle*</span></span>
* <span data-ttu-id="b61ee-124">*Szótár* változott *IDictionary*</span><span class="sxs-lookup"><span data-stu-id="b61ee-124">*Dictionary* changed to *IDictionary*</span></span>
* <span data-ttu-id="b61ee-125">*Lista<T>, string []* változott *IList elemet.<T>*</span><span class="sxs-lookup"><span data-stu-id="b61ee-125">*List<T>, string []* changed to *IList<T>*</span></span>
* <span data-ttu-id="b61ee-126">*NextList* változott *NextPageLink*</span><span class="sxs-lookup"><span data-stu-id="b61ee-126">*NextList* changed to  *NextPageLink*</span></span>

## <a name="return-types"></a><span data-ttu-id="b61ee-127">Visszatérési típusokat</span><span class="sxs-lookup"><span data-stu-id="b61ee-127">Return types</span></span>

* <span data-ttu-id="b61ee-128">**KeyList** és **SecretList** visszaadható *IPage<T>*  helyett *ListKeysResponseMessage*</span><span class="sxs-lookup"><span data-stu-id="b61ee-128">**KeyList** and **SecretList** will return *IPage<T>* instead of *ListKeysResponseMessage*</span></span>
* <span data-ttu-id="b61ee-129">A létrehozott **BackupKeyAsync** visszaadható *BackupKeyResult* tartalmazó *érték* (biztonsági másolat blob).</span><span class="sxs-lookup"><span data-stu-id="b61ee-129">The generated **BackupKeyAsync** will return *BackupKeyResult* which contains *Value* (back-up blob).</span></span> <span data-ttu-id="b61ee-130">A metódus burkolása előtt, és csak a értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="b61ee-130">Before the method was wrapped and returning only the value.</span></span>

## <a name="exceptions"></a><span data-ttu-id="b61ee-131">Kivételek</span><span class="sxs-lookup"><span data-stu-id="b61ee-131">Exceptions</span></span>

* <span data-ttu-id="b61ee-132">*KeyVaultClientException* változott *KeyVaultErrorException*</span><span class="sxs-lookup"><span data-stu-id="b61ee-132">*KeyVaultClientException* is changed to *KeyVaultErrorException*</span></span>
* <span data-ttu-id="b61ee-133">Hiba a szolgáltatás változtatják *kivétel. Hiba* való *kivétel. Body.Error.Message*.</span><span class="sxs-lookup"><span data-stu-id="b61ee-133">The service error is changed from *exception.Error* to *exception.Body.Error.Message*.</span></span>
* <span data-ttu-id="b61ee-134">További információ eltávolítja a hibaüzenetet **[JsonExtensionData]**.</span><span class="sxs-lookup"><span data-stu-id="b61ee-134">Removed additional info from the error message for **[JsonExtensionData]**.</span></span>

## <a name="constructors"></a><span data-ttu-id="b61ee-135">Konstruktorok</span><span class="sxs-lookup"><span data-stu-id="b61ee-135">Constructors</span></span>

* <span data-ttu-id="b61ee-136">Elfogadása helyett egy *HttpClient* konstruktor argumentumként, a konstruktor csak fogad *HttpClientHandler* vagy *DelegatingHandler []*.</span><span class="sxs-lookup"><span data-stu-id="b61ee-136">Instead of accepting an *HttpClient* as a constructor argument, the constructor only accepts *HttpClientHandler* or *DelegatingHandler[]*.</span></span>

## <a name="downloaded-packages"></a><span data-ttu-id="b61ee-137">A letöltött csomagok</span><span class="sxs-lookup"><span data-stu-id="b61ee-137">Downloaded packages</span></span>

<span data-ttu-id="b61ee-138">Ha egy ügyfél a Kulcstárolónak függőséget feldolgozása folyamatban van a következő volt lesznek letöltve</span><span class="sxs-lookup"><span data-stu-id="b61ee-138">When a client is processing a  dependency on Key Vault the following were downloaded</span></span>

### <a name="previous-package-list"></a><span data-ttu-id="b61ee-139">Előző csomaglista</span><span class="sxs-lookup"><span data-stu-id="b61ee-139">Previous package list</span></span>

* <span data-ttu-id="b61ee-140">csomag id="Hyak.Common" verzió = "1.0.2-es" targetFramework = "net45"</span><span class="sxs-lookup"><span data-stu-id="b61ee-140">package id="Hyak.Common" version="1.0.2" targetFramework="net45"</span></span>
* <span data-ttu-id="b61ee-141">csomag id="Microsoft.Azure.Common" verzió = "2.0.4" targetFramework = "net45"</span><span class="sxs-lookup"><span data-stu-id="b61ee-141">package id="Microsoft.Azure.Common" version="2.0.4" targetFramework="net45"</span></span>
* <span data-ttu-id="b61ee-142">csomag id="Microsoft.Azure.Common.Dependencies" verzió = "1.0.0" targetFramework = "net45"</span><span class="sxs-lookup"><span data-stu-id="b61ee-142">package id="Microsoft.Azure.Common.Dependencies" version="1.0.0" targetFramework="net45"</span></span>
* <span data-ttu-id="b61ee-143">csomag id="Microsoft.Azure.KeyVault" verzió = "1.0.0" targetFramework = "net45"</span><span class="sxs-lookup"><span data-stu-id="b61ee-143">package id="Microsoft.Azure.KeyVault" version="1.0.0" targetFramework="net45"</span></span>
* <span data-ttu-id="b61ee-144">csomag id="Microsoft.Bcl" verzió = "1.1.9" targetFramework = "net45"</span><span class="sxs-lookup"><span data-stu-id="b61ee-144">package id="Microsoft.Bcl" version="1.1.9" targetFramework="net45"</span></span>
* <span data-ttu-id="b61ee-145">csomag id="Microsoft.Bcl.Async" verzió = "1.0.168" targetFramework = "net45"</span><span class="sxs-lookup"><span data-stu-id="b61ee-145">package id="Microsoft.Bcl.Async" version="1.0.168" targetFramework="net45"</span></span>
* <span data-ttu-id="b61ee-146">csomag id="Microsoft.Bcl.Build" verzió = "1.0.14" targetFramework = "net45"</span><span class="sxs-lookup"><span data-stu-id="b61ee-146">package id="Microsoft.Bcl.Build" version="1.0.14" targetFramework="net45"</span></span>
* <span data-ttu-id="b61ee-147">csomag id="Microsoft.Net.Http" verzió = "2.2.22" targetFramework = "net45"</span><span class="sxs-lookup"><span data-stu-id="b61ee-147">package id="Microsoft.Net.Http" version="2.2.22" targetFramework="net45"</span></span>

### <a name="current-package-list"></a><span data-ttu-id="b61ee-148">Aktuális csomaglista</span><span class="sxs-lookup"><span data-stu-id="b61ee-148">Current package list</span></span>

* <span data-ttu-id="b61ee-149">csomag id="Microsoft.Azure.KeyVault" verzió = "2.0.0-preview" targetFramework = "net45"</span><span class="sxs-lookup"><span data-stu-id="b61ee-149">package id="Microsoft.Azure.KeyVault" version="2.0.0-preview" targetFramework="net45"</span></span>
* <span data-ttu-id="b61ee-150">csomag id="Microsoft.Rest.ClientRuntime" verzió = "2.2.0" targetFramework = "net45"</span><span class="sxs-lookup"><span data-stu-id="b61ee-150">package id="Microsoft.Rest.ClientRuntime" version="2.2.0" targetFramework="net45"</span></span>
* <span data-ttu-id="b61ee-151">csomag id="Microsoft.Rest.ClientRuntime.Azure" verzió = "3.2.0" targetFramework = "net45"</span><span class="sxs-lookup"><span data-stu-id="b61ee-151">package id="Microsoft.Rest.ClientRuntime.Azure" version="3.2.0" targetFramework="net45"</span></span>

## <a name="class-changes"></a><span data-ttu-id="b61ee-152">Osztály változások</span><span class="sxs-lookup"><span data-stu-id="b61ee-152">Class changes</span></span>

* <span data-ttu-id="b61ee-153">**UnixEpoch** osztály el lett távolítva.</span><span class="sxs-lookup"><span data-stu-id="b61ee-153">**UnixEpoch** class has been removed</span></span>
* <span data-ttu-id="b61ee-154">**Base64UrlConverter** osztály neve módosult az **Base64UrlJsonConverter**</span><span class="sxs-lookup"><span data-stu-id="b61ee-154">**Base64UrlConverter** class is renamed to **Base64UrlJsonConverter**</span></span>

## <a name="other-changes"></a><span data-ttu-id="b61ee-155">Az egyéb módosítások</span><span class="sxs-lookup"><span data-stu-id="b61ee-155">Other changes</span></span>

* <span data-ttu-id="b61ee-156">A konfiguráció KV művelet újrapróbálkozási házirend átmeneti hibáiról már támogatja az API ezen verziójára.</span><span class="sxs-lookup"><span data-stu-id="b61ee-156">Support for the configuration of KV operation retry policy on transient failures has been added to this version of the API.</span></span>

## <a name="microsoftazuremanagementkeyvault-nuget"></a><span data-ttu-id="b61ee-157">Microsoft.Azure.Management.KeyVault NuGet</span><span class="sxs-lookup"><span data-stu-id="b61ee-157">Microsoft.Azure.Management.KeyVault NuGet</span></span>

* <span data-ttu-id="b61ee-158">A művelet által visszaadott egy *tároló*, a visszatérési típusa történt egy osztály, amely egy tároló tulajdonság található.</span><span class="sxs-lookup"><span data-stu-id="b61ee-158">For the operations that returned a *vault*, the return type was a class that contained a Vault property.</span></span> <span data-ttu-id="b61ee-159">A visszatérési típus már *tároló*.</span><span class="sxs-lookup"><span data-stu-id="b61ee-159">The return type is now *Vault*.</span></span>
* <span data-ttu-id="b61ee-160">*PermissionsToKeys* és *PermissionsToSecrets* most már *Permissions.Keys* és *Permissions.Secrets*</span><span class="sxs-lookup"><span data-stu-id="b61ee-160">*PermissionsToKeys* and *PermissionsToSecrets* are now *Permissions.Keys* and *Permissions.Secrets*</span></span>
* <span data-ttu-id="b61ee-161">Néhány, a visszatérési típusok változtatást a vezérlő-vezérlősík is vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="b61ee-161">Some of the return types changes apply to the control-plane as well.</span></span>

## <a name="microsoftazurekeyvaultextensions-nuget"></a><span data-ttu-id="b61ee-162">Microsoft.Azure.KeyVault.Extensions NuGet</span><span class="sxs-lookup"><span data-stu-id="b61ee-162">Microsoft.Azure.KeyVault.Extensions NuGet</span></span>

* <span data-ttu-id="b61ee-163">A csomag megszakad legfeljebb **Microsoft.Azure.KeyVault.Extensions** és **Microsoft.Azure.KeyVault.Cryptography** a titkosítási műveletek.</span><span class="sxs-lookup"><span data-stu-id="b61ee-163">The package is broken up to **Microsoft.Azure.KeyVault.Extensions** and **Microsoft.Azure.KeyVault.Cryptography** for the cryptography operations.</span></span>

