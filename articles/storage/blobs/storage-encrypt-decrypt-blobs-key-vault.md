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
ms.openlocfilehash: e387dd419a51b5b1df62d10ead97268e8295ff56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-encrypt-and-decrypt-blobs-in-microsoft-azure-storage-using-azure-key-vault"></a>Oktatóanyag: Titkosításához és visszafejtéséhez az Azure Key Vault használatával a Microsoft Azure Storage blobs
## <a name="introduction"></a>Bevezetés
Ez az oktatóanyag ismerteti, hogyan toomake használata az Azure Key Vault ügyféloldali tárolás titkosítása. Az végigvezeti tooencrypt és egy konzolalkalmazásban ezeknek a technológiáknak blob visszafejtéséhez.

**Becsült idő toocomplete:** 20 perc

Áttekintés az Azure Key Vault kapcsolatos információkért lásd: [Mi az Azure Key Vault?](../../key-vault/key-vault-whatis.md).

Áttekintés az Azure Storage ügyféloldali titkosítással kapcsolatos információkért lásd: [ügyféloldali titkosítás és a Microsoft Azure tárolás az Azure Key Vault](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="prerequisites"></a>Előfeltételek
toocomplete ebben az oktatóanyagban rendelkeznie kell a következő hello:

* Egy Azure Storage-fiók
* A Visual Studio 2013 vagy újabb verzió
* Azure PowerShell

## <a name="overview-of-client-side-encryption"></a>Ügyféloldali titkosítás áttekintése
Az Azure Storage ügyféloldali titkosítás áttekintéséért lásd: [ügyféloldali titkosítás és a Microsoft Azure tárolás az Azure Key Vault](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

Ügyféloldali titkosítása működése rövid leírása itt található:

1. hello Azure Storage ügyfél SDK állít elő, a tartalom titkosítási kulcs (CEK), amely a szimmetrikus kulcs egy egyszeri használata.
2. Felhasználói adatok titkosítása a CEK használatával.
3. hello CEK ezt követően (titkosított) hello kulcs titkosítási kulcs (KEK) használatával. hello KEK kulcsazonosítójával azonosíthatók és aszimmetrikus kulcspár vagy egy szimmetrikus kulcsot kell és is kezelhetők helyileg vagy az Azure Key Vault tárolja. hello tárolási ügyfélen soha nem hozzáférés toohello KEK. Egyszerűen meghívja a Key Vault által biztosított hello kulcs alkalmazásburkoló algoritmus. Az ügyfelek választható toouse egyéni szolgáltatók alkalmazásburkoló/kicsomagolásával Ha szeretné, hogy azok kulcshoz.
4. hello titkosított adatok van majd toohello Azure Storage szolgáltatás feltöltve.

## <a name="set-up-your-azure-key-vault"></a>Az Azure Key Vault beállítása
Rendelés tooproceed az oktatóanyaghoz, a következő lépésekkel, amelyek hello oktatóprogram vázolt toodo hello kell [Ismerkedés az Azure Key Vault](../../key-vault/key-vault-get-started.md):

* Hozzon létre egy kulcstartót.
* Vegye fel a kulcs vagy titkos toohello kulcstároló.
* Alkalmazás regisztrálása az Azure Active Directoryban.
* Engedélyezi a hello alkalmazás toouse hello kulcsok vagy titkos kulcsok.

Jegyezze fel, hello ClientID és a ClientSecret, amikor egy alkalmazás regisztrálása az Azure Active Directoryval létrehozott ellenőrizze.

Mindkét kulcsot hello kulcstároló létrehozása. Feltételezzük, hogy a hello rest oktatóanyag hello használja a következő neveket hello: ContosoKeyVault és TestRSAKey1.

## <a name="create-a-console-application-with-packages-and-appsettings"></a>Hozzon létre egy konzolalkalmazást a csomagok és AppSettings
A Visual Studio új Konzolalkalmazás létrehozása.

Adja hozzá a szükséges nuget-csomagok a hello Csomagkezelő konzol.

```
Install-Package WindowsAzure.Storage

// This is hello latest stable release for ADAL.
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

Install-Package Microsoft.Azure.KeyVault
Install-Package Microsoft.Azure.KeyVault.Extensions
```

Adja hozzá az AppSettings toohello App.Config.

```xml
<appSettings>
    <add key="accountName" value="myaccount"/>
    <add key="accountKey" value="theaccountkey"/>
    <add key="clientId" value="theclientid"/>
    <add key="clientSecret" value="theclientsecret"/>
    <add key="container" value="stuff"/>
</appSettings>
```

Adja hozzá a következő hello `using` utasításokat, és győződjön meg arról, hogy tooadd egy hivatkozás tooSystem.Configuration toohello projektet.

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

## <a name="add-a-method-tooget-a-token-tooyour-console-application"></a>A metódus tooget token tooyour Konzolalkalmazás hozzáadása
hello következő módszert használják kulcstároló olyan osztállyal, amelynek tooauthenticate szükséges hozzáférési tooyour kulcstároló.

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

## <a name="access-storage-and-key-vault-in-your-program"></a>Tárolás és a Key Vault hozzáférni a program
Hello Main függvényre adja hozzá a következő kód hello.

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
> Kulcstároló Hálózatiobjektum-modelljei
> 
> Fontos, hogy vannak-e tényleges két Key Vault objektum toounderstand modellek tudomást toobe: egy REST API-t (névtér: KeyVault) hello alapul, és más hello ügyféloldali titkosítás modul bővítménye.
> 
> hello Key Vault ügyfél kommunikál a hello REST API-t, és megértette a JSON webes kulcsok és titkos hello két dolog, amelyek szerepelnek a Key Vault típusú.
> 
> hello Key Vault bővítmények olyan osztályok, úgy tűnik, az Azure Storage ügyféloldali titkosítás kifejezetten létrehozott. Kulcsok (IKey) és egy kulcs feloldó hello fogalma alapján osztályok illesztőfelület tartalmaznak. Nincsenek két implementációja, hogy kell-e tooknow IKey: RSAKey és SymmetricKey. Most akkor fordulhat elő, a hello dolog, amelyek szerepelnek a kulcstároló toocoincide, de ezen a ponton független osztályok (így hello kulcs és a titkos kulcsot tároló ügyfél hello által beolvasott nem valósítja meg a IKey).
> 
> 

## <a name="encrypt-blob-and-upload"></a>A blob titkosításához, és töltse fel
Adja hozzá a hello alábbi tooencrypt blob code, és töltse fel az Azure storage-fiók tooyour. Hello **ResolveKeyAsync** használt módszer egy IKey adja vissza.

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

Az alábbiakban látható egy képernyőkép hello [klasszikus Azure portál](https://manage.windowsazure.com) egy BLOB, a Key Vault szolgáltatókban tárolt kulcsok ügyféloldali titkosítással titkosított. Hello **KeyId** tulajdonsága hello URI kulcstároló, eleget kell hello KEK hello kulcsában. Hello **EncryptedKey** tulajdonság hello hello CEK titkosított verzióját tartalmazza.

![A Blob metaadatai paramétertitkosítási metaadatokat tartalmazó ábrázoló képernyőfelvétel](./media/storage-encrypt-decrypt-blobs-key-vault/blobmetadata.png)

> [!NOTE]
> Ha hello BlobEncryptionPolicy konstruktor tekinti meg, látni fogja, hogy el tudja fogadni a kulcs és/vagy a feloldó. Vegye figyelembe, hogy most egy feloldó nem használata titkosításhoz, mert jelenleg nem támogatja egy alapértelmezett kulcs.
> 
> 

## <a name="decrypt-blob-and-download"></a>Blob visszafejtésére és letöltése
Visszafejtési valóban amikor használatával hello feloldó osztályok értelmezhető. hello azonosító hello kulcs titkosításához használt társítva a metaadatokban hello blob, így nem ok arra, hogy tooretrieve hello kulcs, és ne feledje kulcs és a blob hello társítását. Most, hogy arra hello kulcs továbbra is a Key Vault toomake.   

hello titkos kulcsot egy RSA-kulcs marad a kulcstároló, így a visszafejtési toooccur hello titkosított kulcs metaadatokból hello blob CEK küldött tooKey tároló visszafejtéshez hello tartalmazó.

Adja hozzá a következő toodecrypt hello blob imént feltöltött hello.

```csharp
// In this case, we will not pass a key and only pass hello resolver because
// this policy will only be used for downloading / decrypting.
BlobEncryptionPolicy policy = new BlobEncryptionPolicy(null, cloudResolver);
BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

using (var np = File.Open(@"C:\data\MyFileDecrypted.txt", FileMode.Create))
    blob.DownloadToStream(np, null, options, null);
```

> [!NOTE]
> Többféle feloldókat toomake kulcskezelés más típusú egyszerűbb, többek között: AggregateKeyResolver és CachingKeyResolver.
> 
> 

## <a name="use-key-vault-secrets"></a>Key Vault titkos kulcsok használata
hello módon toouse ügyféloldali titkosítással titkos kulcs oka keresztül hello SymmetricKey osztály titkos kulcs lényegében egy szimmetrikus kulcsot. De a fentieknek megfelelően a Key Vault titkos kulcs nem felel meg pontosan tooa SymmetricKey. Van néhány dolog toounderstand:

* egy SymmetricKey hello kulcs hossza toobe rögzített: 128, 192, 256-ot, 384 vagy 512 bites.
* egy SymmetricKey hello kulcsot kell Base64-kódolású.
* A Key Vault titkos kulcsot, amely egy SymmetricKey lesz kell toohave egy "application/octet-stream" a Key Vault tartalomtípusa.

Íme egy példa a titkos kulcs létrehozásának a kulcstároló, egy SymmetricKey használható PowerShell.
Vegye figyelembe, hogy hello rögzített kódolt, $key, értéke csak bemutató célra. A kód célszerű toogenerate ezt a kulcsot.

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

A Konzolalkalmazás azonos hívás tooretrieve előtt a titkos, mint egy SymmetricKey hello is használhatja.

```csharp
SymmetricKey sec = (SymmetricKey) cloudResolver.ResolveKeyAsync(
    "https://contosokeyvault.vault.azure.net/secrets/TestSecret2/",
    CancellationToken.None).GetAwaiter().GetResult();
```
Ennyi az egész. Jó munkát!

## <a name="next-steps"></a>Következő lépések
Microsoft Azure Storage a C# használatával kapcsolatos további információkért lásd: [Microsoft Azure Storage ügyféloldali kódtára a .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Hello Blob REST API kapcsolatos további információkért lásd: [Blob szolgáltatás REST API](https://msdn.microsoft.com/library/azure/dd135733.aspx).

Hello legújabb információ a Microsoft Azure Storage: toohello [Microsoft Azure tárolás fejlesztői Blog](http://blogs.msdn.com/b/windowsazurestorage/).
