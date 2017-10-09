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
# <a name="azure-key-vault-net-20---release-notes-and-migration-guide"></a>Az Azure Key Vault .NET 2.0 – kibocsátási megjegyzések és az áttelepítési útmutató
hello következő notes és útmutatókat is Azure Key Vault .NET dolgozó fejlesztők számára / C# könyvtár. Hello áttűnés hello 1.0-s verzió toohello 2.0-s verziójáról, a frissítések száma végzett áttelepítés munkahelyi ahhoz, hogy a hello funkciók javításai toobenefit a kódban szükség és kiegészítéseit, mint szolgáltatás teszi **Key Vault tanúsítványok** támogatja.

## <a name="key-vault-certificates"></a>Key Vault tanúsítványok

Key Vault tanúsítványok támogatás lehetővé teszi a x509 vezetése a tanúsítványok és a következő viselkedés hello:  

* Lehetővé teszi, hogy a tanúsítvány tulajdonos toocreate egy tanúsítványt, vagy egy meglévő tanúsítvány importálása hello keresztül a Key Vault létrehozási folyamata. Mindkét önaláírt és a hitelesítésszolgáltató tanúsítványokat jön létre.
* Lehetővé teszi, hogy a Key Vault tanúsítvány tulajdonosa tooimplement biztonságos tárolása és kezelése X509 tanúsítványok és titkos kulcs adatai közötti interakció nélkül.  
* Lehetővé teszi, hogy a tanúsítvány tulajdonos toocreate egy házirendet, amely arra utasítja a Key Vault toomanage hello életciklus-tanúsítványt.  
* Lehetővé teszi a tanúsítvány tulajdonosainak tooprovide értesítési kapcsolatos információkat életciklus-események lejárati és a tanúsítvány megújítása.  
* Támogatja az automatikus megújítását a kiválasztott kiállítók - Key Vault partner X509 szolgáltatók tanúsítvány / hitelesítésszolgáltatók.
  
  * Megjegyzés: - nem közösen szolgáltatók/hitelesítésszolgáltatók is használhat, de nem fogja támogatni a hello automatikus megújítási szolgáltatást.

## <a name="net-support"></a>.NET-támogatás

* **A .NET 4.0** hello Azure Key Vault .NET hello 2.0-s verziója nem támogatott / C# könyvtár
* **A .NET core** hello Azure Key Vault .NET hello 2.0-s verzióját támogatja / C# könyvtár

## <a name="namespaces"></a>Névterek

* a névtér hello **modellek** változtatják **Microsoft.Azure.KeyVault** túl**Microsoft.Azure.KeyVault.Models**.
* Hello **Microsoft.Azure.KeyVault.Internal** névtér megszakad.
* hello Azure SDK függőségek névtér nem változtatják **Hyak.Common** és **Hyak.Common.Internals** túl**Microsoft.Rest** és  **Microsoft.Rest.Serialization**

## <a name="type-changes"></a>Módosításai

* *Titkos kulcs* túl megváltozott*SecretBundle*
* *Szótár* túl megváltozott*IDictionary*
* *Lista<T>, string []* túl megváltozott*IList elemet.<T>*
* *NextList* túl megváltozott *NextPageLink*

## <a name="return-types"></a>Visszatérési típusokat

* **KeyList** és **SecretList** visszaadható *IPage<T>*  helyett *ListKeysResponseMessage*
* generált hello **BackupKeyAsync** visszaadható *BackupKeyResult* tartalmazó *érték* (biztonsági másolat blob). Hello előtt metódus burkolt és adatszolgáltató egyetlen hello érték volt.

## <a name="exceptions"></a>Kivételek

* *KeyVaultClientException* túl megváltozott*KeyVaultErrorException*
* hello szolgáltatás hiba változtatják *kivétel. Hiba* túl*kivétel. Body.Error.Message*.
* További információ távolítva hello hibaüzenetet **[JsonExtensionData]**.

## <a name="constructors"></a>Konstruktorok

* Elfogadása helyett egy *HttpClient* konstruktor argumentumként hello konstruktor csak fogad *HttpClientHandler* vagy *DelegatingHandler []*.

## <a name="downloaded-packages"></a>A letöltött csomagok

Amikor egy ügyfél feldolgozása folyamatban van egy függőséget meg Key Vault hello következő letöltött

### <a name="previous-package-list"></a>Előző csomaglista

* csomag id="Hyak.Common" verzió = "1.0.2-es" targetFramework = "net45"
* csomag id="Microsoft.Azure.Common" verzió = "2.0.4" targetFramework = "net45"
* csomag id="Microsoft.Azure.Common.Dependencies" verzió = "1.0.0" targetFramework = "net45"
* csomag id="Microsoft.Azure.KeyVault" verzió = "1.0.0" targetFramework = "net45"
* csomag id="Microsoft.Bcl" verzió = "1.1.9" targetFramework = "net45"
* csomag id="Microsoft.Bcl.Async" verzió = "1.0.168" targetFramework = "net45"
* csomag id="Microsoft.Bcl.Build" verzió = "1.0.14" targetFramework = "net45"
* csomag id="Microsoft.Net.Http" verzió = "2.2.22" targetFramework = "net45"

### <a name="current-package-list"></a>Aktuális csomaglista

* csomag id="Microsoft.Azure.KeyVault" verzió = "2.0.0-preview" targetFramework = "net45"
* csomag id="Microsoft.Rest.ClientRuntime" verzió = "2.2.0" targetFramework = "net45"
* csomag id="Microsoft.Rest.ClientRuntime.Azure" verzió = "3.2.0" targetFramework = "net45"

## <a name="class-changes"></a>Osztály változások

* **UnixEpoch** osztály el lett távolítva.
* **Base64UrlConverter** osztály túl a rendszer átnevezi**Base64UrlJsonConverter**

## <a name="other-changes"></a>Az egyéb módosítások

* Hello konfiguráció KV művelet újrapróbálkozási házirend átmeneti hibáiról már támogatja az toothis hello API verziója.

## <a name="microsoftazuremanagementkeyvault-nuget"></a>Microsoft.Azure.Management.KeyVault NuGet

* A visszaadott hello műveletek egy *tároló*, hello visszatérési típusa történt egy osztály, amely egy tároló tulajdonság található. hello visszatérési típus már *tároló*.
* *PermissionsToKeys* és *PermissionsToSecrets* most már *Permissions.Keys* és *Permissions.Secrets*
* Néhány hello visszatérési típusok módosítások toohello vezérlő-vezérlősík is.

## <a name="microsoftazurekeyvaultextensions-nuget"></a>Microsoft.Azure.KeyVault.Extensions NuGet

* hello csomag rendszer darabolja túl**Microsoft.Azure.KeyVault.Extensions** és **Microsoft.Azure.KeyVault.Cryptography** hello titkosítási műveletek.

