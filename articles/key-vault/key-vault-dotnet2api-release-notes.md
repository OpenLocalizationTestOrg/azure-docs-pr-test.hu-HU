---
title: "aaaKey tároló .NET 2.x API kibocsátási megjegyzései |} Microsoft Docs"
description: ".NET-fejlesztők számára az API toocode fogja használni az Azure Key Vault"
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
ms.openlocfilehash: d95b84cf73c155f117f37e93893f27b02a75855c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-net-20---release-notes-and-migration-guide"></a><span data-ttu-id="52d15-103">Az Azure Key Vault .NET 2.0 – kibocsátási megjegyzések és az áttelepítési útmutató</span><span class="sxs-lookup"><span data-stu-id="52d15-103">Azure Key Vault .NET 2.0 - Release Notes and Migration Guide</span></span>
<span data-ttu-id="52d15-104">hello következő notes és útmutatókat is Azure Key Vault .NET dolgozó fejlesztők számára / C# könyvtár.</span><span class="sxs-lookup"><span data-stu-id="52d15-104">hello following notes and guidance are for developers working with Azure Key Vault .NET / C# library.</span></span> <span data-ttu-id="52d15-105">Hello áttűnés hello 1.0-s verzió toohello 2.0-s verziójáról, a frissítések száma végzett áttelepítés munkahelyi ahhoz, hogy a hello funkciók javításai toobenefit a kódban szükség és kiegészítéseit, mint szolgáltatás teszi **Key Vault tanúsítványok** támogatja.</span><span class="sxs-lookup"><span data-stu-id="52d15-105">In hello transition from hello 1.0 version toohello 2.0 version, a number of updates have been made that will require migration work in your code in order for you toobenefit from hello functional improvements and feature additions such as **Key Vault certificates** support.</span></span>

## <a name="key-vault-certificates"></a><span data-ttu-id="52d15-106">Key Vault tanúsítványok</span><span class="sxs-lookup"><span data-stu-id="52d15-106">Key Vault certificates</span></span>

<span data-ttu-id="52d15-107">Key Vault tanúsítványok támogatás lehetővé teszi a x509 vezetése a tanúsítványok és a következő viselkedés hello:</span><span class="sxs-lookup"><span data-stu-id="52d15-107">Key Vault certificates support provides for management of your x509 certificates and hello following behaviors:</span></span>  

* <span data-ttu-id="52d15-108">Lehetővé teszi, hogy a tanúsítvány tulajdonos toocreate egy tanúsítványt, vagy egy meglévő tanúsítvány importálása hello keresztül a Key Vault létrehozási folyamata.</span><span class="sxs-lookup"><span data-stu-id="52d15-108">Allows a certificate owner toocreate a certificate through a Key Vault creation process or through hello import of an existing certificate.</span></span> <span data-ttu-id="52d15-109">Mindkét önaláírt és a hitelesítésszolgáltató tanúsítványokat jön létre.</span><span class="sxs-lookup"><span data-stu-id="52d15-109">This includes both self-signed and Certificate Authority generated certificates.</span></span>
* <span data-ttu-id="52d15-110">Lehetővé teszi, hogy a Key Vault tanúsítvány tulajdonosa tooimplement biztonságos tárolása és kezelése X509 tanúsítványok és titkos kulcs adatai közötti interakció nélkül.</span><span class="sxs-lookup"><span data-stu-id="52d15-110">Allows a Key Vault certificate owner tooimplement secure storage and management of X509 certificates without interaction with private key material.</span></span>  
* <span data-ttu-id="52d15-111">Lehetővé teszi, hogy a tanúsítvány tulajdonos toocreate egy házirendet, amely arra utasítja a Key Vault toomanage hello életciklus-tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="52d15-111">Allows a certificate owner toocreate a policy that directs Key Vault toomanage hello life-cycle of a certificate.</span></span>  
* <span data-ttu-id="52d15-112">Lehetővé teszi a tanúsítvány tulajdonosainak tooprovide értesítési kapcsolatos információkat életciklus-események lejárati és a tanúsítvány megújítása.</span><span class="sxs-lookup"><span data-stu-id="52d15-112">Allows certificate owners tooprovide contact information for notification about life-cycle events of expiration and renewal of certificate.</span></span>  
* <span data-ttu-id="52d15-113">Támogatja az automatikus megújítását a kiválasztott kiállítók - Key Vault partner X509 szolgáltatók tanúsítvány / hitelesítésszolgáltatók.</span><span class="sxs-lookup"><span data-stu-id="52d15-113">Supports automatic renewal with selected issuers - Key Vault partner X509 certificate providers / certificate authorities.</span></span>
  
  * <span data-ttu-id="52d15-114">Megjegyzés: - nem közösen szolgáltatók/hitelesítésszolgáltatók is használhat, de nem fogja támogatni a hello automatikus megújítási szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="52d15-114">NOTE - Non-partnered providers/authorities are also allowed but, will not support hello auto renewal feature.</span></span>

## <a name="net-support"></a><span data-ttu-id="52d15-115">.NET-támogatás</span><span class="sxs-lookup"><span data-stu-id="52d15-115">.NET support</span></span>

* <span data-ttu-id="52d15-116">**A .NET 4.0** hello Azure Key Vault .NET hello 2.0-s verziója nem támogatott / C# könyvtár</span><span class="sxs-lookup"><span data-stu-id="52d15-116">**.NET 4.0** is not supported by hello 2.0 version of hello Azure Key Vault .NET/C# library</span></span>
* <span data-ttu-id="52d15-117">**A .NET core** hello Azure Key Vault .NET hello 2.0-s verzióját támogatja / C# könyvtár</span><span class="sxs-lookup"><span data-stu-id="52d15-117">**.NET Core** is supported by hello 2.0 version of hello Azure Key Vault .NET/C# library</span></span>

## <a name="namespaces"></a><span data-ttu-id="52d15-118">Névterek</span><span class="sxs-lookup"><span data-stu-id="52d15-118">Namespaces</span></span>

* <span data-ttu-id="52d15-119">a névtér hello **modellek** változtatják **Microsoft.Azure.KeyVault** túl**Microsoft.Azure.KeyVault.Models**.</span><span class="sxs-lookup"><span data-stu-id="52d15-119">hello namespace for **models** is changed from **Microsoft.Azure.KeyVault** too**Microsoft.Azure.KeyVault.Models**.</span></span>
* <span data-ttu-id="52d15-120">Hello **Microsoft.Azure.KeyVault.Internal** névtér megszakad.</span><span class="sxs-lookup"><span data-stu-id="52d15-120">hello **Microsoft.Azure.KeyVault.Internal** namespace is dropped.</span></span>
* <span data-ttu-id="52d15-121">hello Azure SDK függőségek névtér nem változtatják **Hyak.Common** és **Hyak.Common.Internals** túl**Microsoft.Rest** és  **Microsoft.Rest.Serialization**</span><span class="sxs-lookup"><span data-stu-id="52d15-121">hello Azure SDK dependencies namespace are changed from **Hyak.Common** and **Hyak.Common.Internals** too**Microsoft.Rest** and **Microsoft.Rest.Serialization**</span></span>

## <a name="type-changes"></a><span data-ttu-id="52d15-122">Módosításai</span><span class="sxs-lookup"><span data-stu-id="52d15-122">Type changes</span></span>

* <span data-ttu-id="52d15-123">*Titkos kulcs* túl megváltozott*SecretBundle*</span><span class="sxs-lookup"><span data-stu-id="52d15-123">*Secret* changed too*SecretBundle*</span></span>
* <span data-ttu-id="52d15-124">*Szótár* túl megváltozott*IDictionary*</span><span class="sxs-lookup"><span data-stu-id="52d15-124">*Dictionary* changed too*IDictionary*</span></span>
* <span data-ttu-id="52d15-125">*Lista<T>, string []* túl megváltozott*IList elemet.<T>*</span><span class="sxs-lookup"><span data-stu-id="52d15-125">*List<T>, string []* changed too*IList<T>*</span></span>
* <span data-ttu-id="52d15-126">*NextList* túl megváltozott *NextPageLink*</span><span class="sxs-lookup"><span data-stu-id="52d15-126">*NextList* changed too *NextPageLink*</span></span>

## <a name="return-types"></a><span data-ttu-id="52d15-127">Visszatérési típusokat</span><span class="sxs-lookup"><span data-stu-id="52d15-127">Return types</span></span>

* <span data-ttu-id="52d15-128">**KeyList** és **SecretList** visszaadható *IPage<T>*  helyett *ListKeysResponseMessage*</span><span class="sxs-lookup"><span data-stu-id="52d15-128">**KeyList** and **SecretList** will return *IPage<T>* instead of *ListKeysResponseMessage*</span></span>
* <span data-ttu-id="52d15-129">generált hello **BackupKeyAsync** visszaadható *BackupKeyResult* tartalmazó *érték* (biztonsági másolat blob).</span><span class="sxs-lookup"><span data-stu-id="52d15-129">hello generated **BackupKeyAsync** will return *BackupKeyResult* which contains *Value* (back-up blob).</span></span> <span data-ttu-id="52d15-130">Hello előtt metódus burkolt és adatszolgáltató egyetlen hello érték volt.</span><span class="sxs-lookup"><span data-stu-id="52d15-130">Before hello method was wrapped and returning only hello value.</span></span>

## <a name="exceptions"></a><span data-ttu-id="52d15-131">Kivételek</span><span class="sxs-lookup"><span data-stu-id="52d15-131">Exceptions</span></span>

* <span data-ttu-id="52d15-132">*KeyVaultClientException* túl megváltozott*KeyVaultErrorException*</span><span class="sxs-lookup"><span data-stu-id="52d15-132">*KeyVaultClientException* is changed too*KeyVaultErrorException*</span></span>
* <span data-ttu-id="52d15-133">hello szolgáltatás hiba változtatják *kivétel. Hiba* túl*kivétel. Body.Error.Message*.</span><span class="sxs-lookup"><span data-stu-id="52d15-133">hello service error is changed from *exception.Error* too*exception.Body.Error.Message*.</span></span>
* <span data-ttu-id="52d15-134">További információ távolítva hello hibaüzenetet **[JsonExtensionData]**.</span><span class="sxs-lookup"><span data-stu-id="52d15-134">Removed additional info from hello error message for **[JsonExtensionData]**.</span></span>

## <a name="constructors"></a><span data-ttu-id="52d15-135">Konstruktorok</span><span class="sxs-lookup"><span data-stu-id="52d15-135">Constructors</span></span>

* <span data-ttu-id="52d15-136">Elfogadása helyett egy *HttpClient* konstruktor argumentumként hello konstruktor csak fogad *HttpClientHandler* vagy *DelegatingHandler []*.</span><span class="sxs-lookup"><span data-stu-id="52d15-136">Instead of accepting an *HttpClient* as a constructor argument, hello constructor only accepts *HttpClientHandler* or *DelegatingHandler[]*.</span></span>

## <a name="downloaded-packages"></a><span data-ttu-id="52d15-137">A letöltött csomagok</span><span class="sxs-lookup"><span data-stu-id="52d15-137">Downloaded packages</span></span>

<span data-ttu-id="52d15-138">Amikor egy ügyfél feldolgozása folyamatban van egy függőséget meg Key Vault hello következő letöltött</span><span class="sxs-lookup"><span data-stu-id="52d15-138">When a client is processing a  dependency on Key Vault hello following were downloaded</span></span>

### <a name="previous-package-list"></a><span data-ttu-id="52d15-139">Előző csomaglista</span><span class="sxs-lookup"><span data-stu-id="52d15-139">Previous package list</span></span>

* <span data-ttu-id="52d15-140">csomag id="Hyak.Common" verzió = "1.0.2-es" targetFramework = "net45"</span><span class="sxs-lookup"><span data-stu-id="52d15-140">package id="Hyak.Common" version="1.0.2" targetFramework="net45"</span></span>
* <span data-ttu-id="52d15-141">csomag id="Microsoft.Azure.Common" verzió = "2.0.4" targetFramework = "net45"</span><span class="sxs-lookup"><span data-stu-id="52d15-141">package id="Microsoft.Azure.Common" version="2.0.4" targetFramework="net45"</span></span>
* <span data-ttu-id="52d15-142">csomag id="Microsoft.Azure.Common.Dependencies" verzió = "1.0.0" targetFramework = "net45"</span><span class="sxs-lookup"><span data-stu-id="52d15-142">package id="Microsoft.Azure.Common.Dependencies" version="1.0.0" targetFramework="net45"</span></span>
* <span data-ttu-id="52d15-143">csomag id="Microsoft.Azure.KeyVault" verzió = "1.0.0" targetFramework = "net45"</span><span class="sxs-lookup"><span data-stu-id="52d15-143">package id="Microsoft.Azure.KeyVault" version="1.0.0" targetFramework="net45"</span></span>
* <span data-ttu-id="52d15-144">csomag id="Microsoft.Bcl" verzió = "1.1.9" targetFramework = "net45"</span><span class="sxs-lookup"><span data-stu-id="52d15-144">package id="Microsoft.Bcl" version="1.1.9" targetFramework="net45"</span></span>
* <span data-ttu-id="52d15-145">csomag id="Microsoft.Bcl.Async" verzió = "1.0.168" targetFramework = "net45"</span><span class="sxs-lookup"><span data-stu-id="52d15-145">package id="Microsoft.Bcl.Async" version="1.0.168" targetFramework="net45"</span></span>
* <span data-ttu-id="52d15-146">csomag id="Microsoft.Bcl.Build" verzió = "1.0.14" targetFramework = "net45"</span><span class="sxs-lookup"><span data-stu-id="52d15-146">package id="Microsoft.Bcl.Build" version="1.0.14" targetFramework="net45"</span></span>
* <span data-ttu-id="52d15-147">csomag id="Microsoft.Net.Http" verzió = "2.2.22" targetFramework = "net45"</span><span class="sxs-lookup"><span data-stu-id="52d15-147">package id="Microsoft.Net.Http" version="2.2.22" targetFramework="net45"</span></span>

### <a name="current-package-list"></a><span data-ttu-id="52d15-148">Aktuális csomaglista</span><span class="sxs-lookup"><span data-stu-id="52d15-148">Current package list</span></span>

* <span data-ttu-id="52d15-149">csomag id="Microsoft.Azure.KeyVault" verzió = "2.0.0-preview" targetFramework = "net45"</span><span class="sxs-lookup"><span data-stu-id="52d15-149">package id="Microsoft.Azure.KeyVault" version="2.0.0-preview" targetFramework="net45"</span></span>
* <span data-ttu-id="52d15-150">csomag id="Microsoft.Rest.ClientRuntime" verzió = "2.2.0" targetFramework = "net45"</span><span class="sxs-lookup"><span data-stu-id="52d15-150">package id="Microsoft.Rest.ClientRuntime" version="2.2.0" targetFramework="net45"</span></span>
* <span data-ttu-id="52d15-151">csomag id="Microsoft.Rest.ClientRuntime.Azure" verzió = "3.2.0" targetFramework = "net45"</span><span class="sxs-lookup"><span data-stu-id="52d15-151">package id="Microsoft.Rest.ClientRuntime.Azure" version="3.2.0" targetFramework="net45"</span></span>

## <a name="class-changes"></a><span data-ttu-id="52d15-152">Osztály változások</span><span class="sxs-lookup"><span data-stu-id="52d15-152">Class changes</span></span>

* <span data-ttu-id="52d15-153">**UnixEpoch** osztály el lett távolítva.</span><span class="sxs-lookup"><span data-stu-id="52d15-153">**UnixEpoch** class has been removed</span></span>
* <span data-ttu-id="52d15-154">**Base64UrlConverter** osztály túl a rendszer átnevezi**Base64UrlJsonConverter**</span><span class="sxs-lookup"><span data-stu-id="52d15-154">**Base64UrlConverter** class is renamed too**Base64UrlJsonConverter**</span></span>

## <a name="other-changes"></a><span data-ttu-id="52d15-155">Az egyéb módosítások</span><span class="sxs-lookup"><span data-stu-id="52d15-155">Other changes</span></span>

* <span data-ttu-id="52d15-156">Hello konfiguráció KV művelet újrapróbálkozási házirend átmeneti hibáiról már támogatja az toothis hello API verziója.</span><span class="sxs-lookup"><span data-stu-id="52d15-156">Support for hello configuration of KV operation retry policy on transient failures has been added toothis version of hello API.</span></span>

## <a name="microsoftazuremanagementkeyvault-nuget"></a><span data-ttu-id="52d15-157">Microsoft.Azure.Management.KeyVault NuGet</span><span class="sxs-lookup"><span data-stu-id="52d15-157">Microsoft.Azure.Management.KeyVault NuGet</span></span>

* <span data-ttu-id="52d15-158">A visszaadott hello műveletek egy *tároló*, hello visszatérési típusa történt egy osztály, amely egy tároló tulajdonság található.</span><span class="sxs-lookup"><span data-stu-id="52d15-158">For hello operations that returned a *vault*, hello return type was a class that contained a Vault property.</span></span> <span data-ttu-id="52d15-159">hello visszatérési típus már *tároló*.</span><span class="sxs-lookup"><span data-stu-id="52d15-159">hello return type is now *Vault*.</span></span>
* <span data-ttu-id="52d15-160">*PermissionsToKeys* és *PermissionsToSecrets* most már *Permissions.Keys* és *Permissions.Secrets*</span><span class="sxs-lookup"><span data-stu-id="52d15-160">*PermissionsToKeys* and *PermissionsToSecrets* are now *Permissions.Keys* and *Permissions.Secrets*</span></span>
* <span data-ttu-id="52d15-161">Néhány hello visszatérési típusok módosítások toohello vezérlő-vezérlősík is.</span><span class="sxs-lookup"><span data-stu-id="52d15-161">Some of hello return types changes apply toohello control-plane as well.</span></span>

## <a name="microsoftazurekeyvaultextensions-nuget"></a><span data-ttu-id="52d15-162">Microsoft.Azure.KeyVault.Extensions NuGet</span><span class="sxs-lookup"><span data-stu-id="52d15-162">Microsoft.Azure.KeyVault.Extensions NuGet</span></span>

* <span data-ttu-id="52d15-163">hello csomag rendszer darabolja túl**Microsoft.Azure.KeyVault.Extensions** és **Microsoft.Azure.KeyVault.Cryptography** hello titkosítási műveletek.</span><span class="sxs-lookup"><span data-stu-id="52d15-163">hello package is broken up too**Microsoft.Azure.KeyVault.Extensions** and **Microsoft.Azure.KeyVault.Cryptography** for hello cryptography operations.</span></span>

