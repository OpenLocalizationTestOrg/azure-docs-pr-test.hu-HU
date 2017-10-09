---
title: "a Microsoft Azure Storage a .NET aaaClient-kiszolgálóoldali titkosítás |} Microsoft Docs"
description: "hello Azure Storage ügyféloldali kódtára a .NET ügyféloldali titkosítás és az Azure Key Vault integration támogatja az Azure Storage-alkalmazások a maximális biztonság érdekében."
services: storage
documentationcenter: .net
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: becfccca-510a-479e-a798-2044becd9a64
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2016
ms.author: robinsh
ms.openlocfilehash: e99551925069d5e05bc283039b252cffe8df5383
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="client-side-encryption-and-azure-key-vault-for-microsoft-azure-storage"></a>A Microsoft Azure Storage ügyféloldali titkosítás és az Azure Key Vault
[!INCLUDE [storage-selector-client-side-encryption-include](../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Áttekintés
Hello [Azure Storage ügyféloldali kódtára a .NET Nuget-csomag](https://www.nuget.org/packages/WindowsAzure.Storage) támogatja a titkosított adatok ügyfélalkalmazásokon belüli előtt tooAzure tárolási feltöltését, és az adatok visszafejtése toohello ügyfél letöltése során. hello kódtár emellett támogatja az integrációs [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) a tárfiókkulcs-kezelés.

Egy részletes oktatóanyag, amely végigvezeti Önt a hello folyamat blobok ügyféloldali titkosítás és az Azure Key Vault használatával titkosítja, lásd: [titkosítása és visszafejtése az Azure Key Vault használatával a Microsoft Azure Storage blobs](storage-encrypt-decrypt-blobs-key-vault.md).

Ügyféloldali titkosítás a Java, lásd: [Java a Microsoft Azure Storage ügyféloldali titkosítás](storage-client-side-encryption-java.md).

## <a name="encryption-and-decryption-via-hello-envelope-technique"></a>Titkosítás és visszafejtés hello boríték technika keresztül
hello folyamatok titkosítási és visszafejtési hello boríték technika kövesse.

### <a name="encryption-via-hello-envelope-technique"></a>Titkosítási hello boríték technika keresztül
Titkosítási hello boríték technika keresztül működik, a következő módon hello:

1. hello Azure storage ügyféloldali kódtár állít elő, a tartalom titkosítási kulcs (CEK), amely a szimmetrikus kulcs egy egyszeri használata.
2. Felhasználói adatok titkosítása a CEK használatával.
3. hello CEK ezt követően (titkosított) hello kulcs titkosítási kulcs (KEK) használatával. hello KEK kulcsazonosítójával azonosíthatók és aszimmetrikus kulcspár, vagy egy szimmetrikus kulcsot és is kezelhetők helyileg vagy Azure kulcs tárolók tárolja.
   
    hello a storage ügyféloldali kódtára maga soha nem hozzáférés tooKEK. hello könyvtár hello kulcs alkalmazásburkoló algoritmus Key Vault által biztosított hív meg. -Felhasználók eldönthetik toouse egyéni szolgáltatók a kulcs alkalmazásburkoló/kicsomagolásával igény.

4. hello titkosított adatok van majd toohello Azure Storage szolgáltatás feltöltve. hello becsomagolt kulcs néhány további titkosítási metaadatokból együtt tárolt metaadatok (a blob) vagy köztes értékkel (üzenetsor-üzeneteket és táblaentitásokat) titkosított hello adatokkal.

### <a name="decryption-via-hello-envelope-technique"></a>Visszafejtési hello boríték technika keresztül
Visszafejtési hello boríték technika keresztül működik, a következő módon hello:

1. hello ügyféloldali kódtár azt feltételezi, hogy hello felhasználó kezel hello kulcs titkosítási kulcs-(KEK) helyileg vagy az Azure Key tárolók. hello felhasználónak nem kell tooknow hello adott használt kulcs titkosításhoz. Ehelyett egy kulcs feloldó, amely különböző kulcs azonosítók tookeys mutat beállítása is, és használja.
2. hello ügyféloldali kódtár hello titkosított adatok hello szolgáltatásban tárolt titkosítási anyagok és tölti le.
3. hello burkolt tartalom titkosítási kulcs (CEK) nem burkolatlan (visszafejtett) használatával hello kulcs titkosítási kulcs-(KEK). Itt újra, hello ügyféloldali kódtár nem rendelkezik hozzáféréssel tooKEK. Egyszerűen meghívja a egyéni hello vagy kulcstároló-szolgáltató kibontásához algoritmus.
4. tartalom titkosítási kulcsot (CEK) meg kell majd hello használt toodecrypt Titkosított hello felhasználói adatokat.

## <a name="encryption-mechanism"></a>Titkosítási módszer
hello a storage ügyféloldali kódtára használ [AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) rendelés tooencrypt felhasználói adatok. Pontosabban [Cipher Block Chaining (CBC)](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) AES módban. Minden szolgáltatás működik némileg eltér, azok itt ismertetik.

### <a name="blobs"></a>Blobok
hello ügyféloldali kódtára jelenleg csak a teljes blobok titkosítása. Pontosabban, titkosítás támogatott hello használatakor **UploadFrom*** módszerek vagy hello **OpenWrite** metódust. Letöltések, mind a teljes és tartomány letöltések támogatott.

Titkosítás során hello ügyféloldali kódtár létre egy véletlenszerű inicializálási vektor (IV) 16 bájt, 32 bájt véletlenszerű tartalom titkosítási kulcs (CEK) együtt, és hajtsa végre a boríték titkosítási hello a blob típusú adatok ezen információk alapján. hello CEK burkolt, és néhány további titkosítási metaadatokból majd tárolja a blob-metaadatok hello titkosított blob hello szolgáltatás együtt.

> [!WARNING]
> Ha szerkeszti, vagy saját metaadatok hello blob feltöltése, módosítani kell, hogy megőrzi a metaadatok tooensure. A metaadatok nélkül új metaadatokkal tölt fel, ha hello burkolt CEK, IV és egyéb metaadatokat elvész, és hello blobtartalom soha nem lesznek lekérhető újra.
> 
> 

Egy titkosított blob letöltése magában foglalja a hello teljes BLOB hello segítségével hello tartalom lekérésének **DownloadTo***/**BlobReadStream** kényelmi módszerek. hello burkolt CEK kicsomagolják, és hello IV együtt használja (tárolt blob metaadatai ebben az esetben) tooreturn visszafejteni hello adatok toohello felhasználók.

Egy tetszőleges tartomány letöltése (**DownloadRange*** módszerek) hello a titkosított blob magában foglalja a hello tartomány beállítása a megadott sorrendben tooget a felhasználók által használt toosuccessfully további adatok kis mennyiségű hello visszafejtése a kért tartomány.

Az összes blob-típusok (blokkblobokat, lapblobokat és hozzáfűző blobok) titkosított/visszafejthető használatával a rendszer.

### <a name="queues"></a>Üzenetsorok
Üzenetsor-üzeneteket tetszőleges méretű lehet, mert a hello ügyféloldali kódtára tartalmaz hello inicializálási vektor (IV) és hello titkosított tartalom titkosítási kulcs (CEK) hello üzenetszöveg a egyéni formátum határozza meg.

Titkosítás során hello ügyféloldali kódtár hoz létre egy véletlenszerű IV 16 bájt 32 bájt véletlenszerű CEK együtt, és ezen információk alapján hello várólista üzenetszöveg boríték titkosítást végez. hello CEK burkolt, és néhány további titkosítási metaadatokból kerülnek toohello titkosított üzenetsorban lévő üzenetet. A módosított (lásd alább) tárolása pedig hello szolgáltatást.

    <MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>

A visszafejtés, során hello becsomagolt kulcs várólista üdvözlőüzenetére kivonjuk és kicsomagolják. hello IV is várólista üdvözlőüzenetére kinyert és kicsomagolják hello kulcs toodecrypt hello várólista üzenet adatokkal együtt használja. Vegye figyelembe, hogy hello paramétertitkosítási metaadatokat kis (alatt 500 bájt), ezért azt egy üzenetsor hello 64 KB-os korlát felé száma, amíg a hello hatás kezelhető kell lennie.

### <a name="tables"></a>Táblák
hello ügyfél könyvtár által támogatott titkosítási insert entitás tulajdonságait, és cserélje le a műveleteket.

> [!NOTE]
> Egyesítési jelenleg nem támogatott. Mivel a Tulajdonságok részhalmazát, előfordulhat, hogy korábban használatával titkosított egy másik kulcsot, egyszerűen hello új tulajdonságok egyesítése és hello metaadatainak frissítése adatok elvesztését eredményezi. Így további service hívásai tooread hello már meglévő entitás hello szolgáltatást, vagy tulajdonságonként egy új kulcsot használ, amelyek mindegyikét nem alkalmasak a teljesítményre vonatkozó megfontolásból vagy egyesítés van szükség.
> 
> 

Tábla adattitkosítás a következőképpen működik:  

1. Felhasználók hello tulajdonságok toobe titkosított adja meg.
2. hello ügyféloldali kódtár hoz létre egy "véletlenszerű inicializálási vektor (IV) 16 bájt véletlenszerű tartalom titkosítási kulcs (CEK) 32 bájt összes entitás együtt, és hello egyes tulajdonságok toobe való származtatás egy új IV / titkosítja a boríték titkosítást végez tulajdonság. bináris adatok titkosítva hello tulajdonság tárolja.
3. hello CEK burkolt, és néhány további titkosítási metaadatokból két további fenntartott tulajdonságokat, majd tárolódnak. hello első fenntartott (_ClientEncryptionMetadata1) tulajdonsága egy karakterlánc típusú tulajdonság, amely a IV, a verziót és a becsomagolt kulcs hello információkat tárolja. hello második fenntartott (_ClientEncryptionMetadata2) tulajdonság Titkosított hello tulajdonságok hello információk bináris tulajdonsága. a második tulajdonság (_ClientEncryptionMetadata2) hello információk maga is titkosítva.
4. Lejáró toothese fenntartott szükség tovább tulajdonságokra a titkosításhoz felhasználók most már rendelkezik 252 helyett csak 250 egyéni tulajdonságokat. hello entitás hello teljes mérete 1 MB-nál kevesebb kell lennie.

Vegye figyelembe, hogy a tulajdonságok csak string titkosíthatók. Ha más típusú tulajdonságok titkosított toobe, átalakított toostrings kell. Titkosított hello karakterláncok bináris tulajdonságként hello szolgáltatásban tárolja, és a visszafejtés után hátsó toostrings telepítésekké lesznek átalakítva.

A táblák, továbbá toohello titkosítási házirendnek, felhasználók meg kell adnia hello tulajdonságok toobe titkosítva. Ezt megteheti megadásával vagy (az POCO entitások, amelyek a TableEntity) [EncryptProperty] attribútum vagy egy titkosítási feloldó lehetőségek. Egy titkosítási feloldó egy delegált veszi a partíciós kulcs, sorkulcsot és tulajdonság nevét, és logikai érték beolvasása, amely jelzi, hogy a tulajdonság titkosítani kell-e. Titkosítás során hello ügyféloldali kódtár fogja használni a információk toodecide e tulajdonság titkosítani toohello vezetékes írása közben. hello delegált is körül hogyan tulajdonságok vannak titkosítva logika hello lehetőségét biztosítja. (Például, ha X, majd titkosítása A tulajdonság; ellenkező esetben a Tulajdonságok A és b titkosítása) Vegye figyelembe, hogy az nem szükséges tooprovide ezen információ olvasása vagy entitás lekérdezése közben.

### <a name="batch-operations"></a>Kötegelt műveletek
A kötegelt műveletek hello azonos KEK is használni fogja összes hello sort, hogy kötegelt műveletben, mivel hello ügyféloldali kódtár csak egy beállítások objektum (és ezáltal egy házirend vagy KEK) kötegelt művelet. Azonban hello ügyféloldali kódtár belső létrehoz egy új véletlenszerű IV és soronkénti véletlenszerű CEK hello kötegben. Azt is kiválaszthatják tooencrypt minden művelethez más tulajdonságokkal hello kötegben definiálásával Ez a viselkedés a hello titkosítási feloldó.

### <a name="queries"></a>Lekérdezések
tooperform lekérdezési műveletek, meg kell adnia egy kulcs feloldó, amely képes tooresolve összes hello hello eredménykészletben kulcsok. Ha hello lekérdezés eredménye található entitás nem lehet feloldani tooa szolgáltató, hello ügyféloldali kódtár kivételhibát hiba. A lekérdezés kiszolgálóoldali leképezések végző hello ügyféloldali kódtár adja hozzá hello különleges titkosítási metaadat-tulajdonságainak (_ClientEncryptionMetadata1 és _ClientEncryptionMetadata2) alapértelmezett toohello kijelölt oszlop alapján.

## <a name="azure-key-vault"></a>Azure Key Vault
Az Azure Key Vault segít a felhőalapú alkalmazások és szolgáltatások által használt titkosítási kulcsok és titkos kulcsok védelmében. Az Azure Key Vault használatával felhasználók titkosíthatják a kulcsokat és titkos kulcsokat (például a hitelesítési kulcsokat, a tárfiókok kulcsait, az adattitkosítási kulcsokat. PFX-fájlok és a jelszavakat) hardveres biztonsági modulokkal (HSM) védett kulcsokkal. További információkért lásd: [Mi az Azure Key Vault?](../key-vault/key-vault-whatis.md).

hello a storage ügyféloldali kódtára Azure hello Key Vault alap függvénytár rendelés tooprovide közös keretrendszer a kulcsok felügyeletéhez használ. A felhasználók is kapnak hello további előnye, hogy hello Key Vault bővítmények szalagtárat használ. hello bővítmények kódtár egyszerű és zökkenőmentes Symmetric/RSA helyi és a fő szolgáltatók, valamint összesítő és a gyorsítótár hasznos funkciókat biztosít.

### <a name="interface-and-dependencies"></a>Felület és a függőségek
Nincsenek három Key Vault csomagok:

* Microsoft.Azure.KeyVault.Core hello IKey és IKeyResolver tartalmazza. Nem rendelkezik függőségekkel rendelkező kis csomag is. hello storage ügyféloldali kódtára a .NET határozza meg azt a függőség beállításához.
* Microsoft.Azure.KeyVault hello Key Vault REST ügyfél tartalmazza.
* Microsoft.Azure.KeyVault.Extensions bővítmény kódot tartalmaz, amely tartalmazza többek között a titkosítási algoritmusok és egy RSAKey és egy SymmetricKey. Hello Core és a KeyVault névterek függ, és egy összesített feloldó (Ha a felhasználók több kulcs is szeretné, hogy toouse) és a kulcs gyorsítótárazása funkció toodefine biztosít. Bár hello a storage ügyféloldali kódtára közvetlenül függ a csomag, ha a felhasználó kívánja toouse Azure Key Vault toostore a kulcsok vagy toouse hello Key Vault bővítmények tooconsume hello helyi és felhőbeli kriptográfiai szolgáltatókat, hogy meg kell ezt a csomagot.

Key Vault végzi a nagy értékű főkulcsok, és a Key Vault / szabályozó korlátozások figyelembe ennek kialakítása. A Key Vault ügyféloldali titkosítás elvégzésekor hello modellt toouse szimmetrikus főkulcsok tárolt titkos kulcsot tároló és a helyi gyorsítótárban található. Felhasználók tegye a következőket hello:

1. Kapcsolat nélküli titkos kulcs létrehozása, és töltse fel tooKey tárolóban.
2. Egy paraméter tooresolve hello jelenlegi verziója hello titkosítási titkos kulcsát, és ezek az információk helyileg gyorsítótárazzák használja hello titkos alap azonosítója. Használja a CachingKeyResolver gyorsítótárazás; felhasználók vannak nem várt tooimplement saját gyorsítótárazás logikát.
3. Hello gyorsítótárazása használja bemeneti adatokként hello titkosítási házirend létrehozása közben.

Key Vault használatának kapcsolatos további információk találhatók hello [titkosító mintakódok](https://github.com/Azure/azure-storage-net/tree/master/Samples/GettingStarted/EncryptionSamples).

## <a name="best-practices"></a>Ajánlott eljárások
Titkosítás támogatása csak hello storage ügyféloldali kódtára a .NET érhető el. Windows Phone és Windows-Futtatókörnyezetű jelenleg támogatja a titkosítást.

> [!IMPORTANT]
> Vegye figyelembe a fontos pontok ügyféloldali titkosítás használata esetén:
> 
> * A olvasása vagy írása tooan mikor titkosítsa a blob, használjon teljes blob feltöltése és tartomány/egész blob letöltési parancsok. Hogy ne kelljen tooan titkosított blob használatával protokoll műveletek, például a Put blokk, Put tiltólista, írási lapok, törölje a jelet lapok vagy hozzáfűzése blokk; Ellenkező esetben előfordulhat, hogy hello titkosított blob sérült, és könnyebben olvasható.
> * A táblákhoz hasonló korlátozás létezik. Gondos toonot frissítés titkosított tulajdonságok hello paramétertitkosítási metaadatokat frissítése nélkül lehet.
> * Metaadatok hello titkosított blob állít be, ha szükséges, mivel a metaadatok beállítása nem additívak hello titkosítással kapcsolatos metaadatok felülírhatja. Ez akkor is igaz, a pillanatképek; Ne adjon meg a metaadat egy titkosított blob pillanatképének létrehozása során. Metaadatok be kell állítani, ha kell, hogy toocall hello **FetchAttributes** metódus első tooget hello aktuális paramétertitkosítási metaadatokat, és elkerülheti a egyidejű írási műveleteket, amíg metaadatok beállítása.
> * Hello engedélyezése **RequireEncryption** tulajdonság hello alapértelmezett beállításokat olyan felhasználóknál, akik csak együtt kell működnie a titkosított adatokat. További információkért lásd az alábbi.
> 
> 

## <a name="client-api--interface"></a>Ügyfél API-ja / felület
Egy EncryptionPolicy objektum létrehozása során a felhasználók megadhatják csak kulcsot (végrehajtási IKey), csak egy feloldó (végrehajtási IKeyResolver), vagy mindkettőt. IKey hello alapvető kulcs típusa, amely azonosítja a kulcsazonosító és hello logika biztosít, amely alkalmazásburkoló/kicsomagolásával. IKeyResolver hello feloldási folyamat során használt tooresolve kulcs. Meghatározza a ResolveKey módszere, amely egy megadott kulcsazonosítójával IKey adja vissza. Ez lehetővé teszi a felhasználók hello képességét toochoose több helyen felügyelt több kulcsok között.

* A titkosításhoz hello kulccsal mindig, és egy kulcs hiányában hello hibát eredményez.
* A visszafejtéshez:
  * Ha meg van adva tooget hello kulcs hello kulcs feloldó meghívták. Hello feloldó van megadva, de nem rendelkezik hello kulcsazonosító társítás, ha hiba fordul elő.
  * Ha nincs megadva a feloldó, de egy kulcs van megadva, hello kulcs akkor használatos, ha az azonosító szükséges hello kulcs azonosítója megegyezik-e. Hello azonosítója nem egyezik, ha hiba fordul elő.

Hello [titkosítási minták](https://github.com/Azure/azure-storage-net/tree/master/Samples/GettingStarted/EncryptionSamples) mutassa be a részletes végpont forgatókönyv BLOB, üzenetsorok és táblák, jelszavat Key Vault-integráció.

### <a name="requireencryption-mode"></a>RequireEncryption mód
Felhasználók engedélyezheti üzemmódot ahol feltöltések és a letöltött fájl titkosítva kell lennie. Ebben a módban a kísérletek tooupload adatok egy titkosítási házirend és letöltés választható adatokat nem titkosított hello szolgáltatás nélkül az hello ügyfél sikertelen lesz. Hello **RequireEncryption** hello kérelem beállítások objektum a tulajdonság szabja meg ezt a viselkedést. Ha az alkalmazás titkosítja az Azure Storage-ban tárolt összes objektumot, majd beállíthatja hello **RequireEncryption** hello alapértelmezett lehetőségek hello szolgáltatás ügyfél objektum tulajdonságát. Állítsa például **CloudBlobClient.DefaultRequestOptions.RequireEncryption** túl**igaz** toorequire titkosítási összes blob műveletekhez az ügyfél-objektum használatával végzik.

### <a name="blob-service-encryption"></a>BLOB szolgáltatás titkosítási
Hozzon létre egy **BlobEncryptionPolicy** objektumot, majd állítsa be a hello lehetőségek (API vagy egy ügyfél szinten használatával **DefaultRequestOptions**). Minden más kezelik hello ügyféloldali kódtár által belsőleg.

```csharp
// Create hello IKey used for encryption.
 RsaKey key = new RsaKey("private:key1" /* key identifier */);

 // Create hello encryption policy toobe used for upload and download.
 BlobEncryptionPolicy policy = new BlobEncryptionPolicy(key, null);

 // Set hello encryption policy on hello request options.
 BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

 // Upload hello encrypted contents toohello blob.
 blob.UploadFromStream(stream, size, null, options, null);

 // Download and decrypt hello encrypted contents from hello blob.
 MemoryStream outputStream = new MemoryStream();
 blob.DownloadToStream(outputStream, null, options, null);
```

### <a name="queue-service-encryption"></a>Várólista titkosítását
Hozzon létre egy **QueueEncryptionPolicy** objektumot, majd állítsa be a hello lehetőségek (API vagy egy ügyfél szinten használatával **DefaultRequestOptions**). Minden más kezelik hello ügyféloldali kódtár által belsőleg.

```csharp
// Create hello IKey used for encryption.
 RsaKey key = new RsaKey("private:key1" /* key identifier */);

 // Create hello encryption policy toobe used for upload and download.
 QueueEncryptionPolicy policy = new QueueEncryptionPolicy(key, null);

 // Add message
 QueueRequestOptions options = new QueueRequestOptions() { EncryptionPolicy = policy };
 queue.AddMessage(message, null, null, options, null);

 // Retrieve message
 CloudQueueMessage retrMessage = queue.GetMessage(null, options, null);
```

### <a name="table-service-encryption"></a>TABLE szolgáltatás titkosítási
Ezenkívül egy titkosítási házirend toocreating és beállítása a kérelem lehetőségekről, meg kell adnia egy **EncryptionResolver** a **TableRequestOptions**, vagy a set hello [EncryptProperty] attribútum hello entitás.

#### <a name="using-hello-resolver"></a>Hello feloldó használata

```csharp
// Create hello IKey used for encryption.
 RsaKey key = new RsaKey("private:key1" /* key identifier */);

 // Create hello encryption policy toobe used for upload and download.
 TableEncryptionPolicy policy = new TableEncryptionPolicy(key, null);

 TableRequestOptions options = new TableRequestOptions()
 {
    EncryptionResolver = (pk, rk, propName) =>
     {
        if (propName == "foo")
         {
            return true;
         }
         return false;
     },
     EncryptionPolicy = policy
 };

 // Insert Entity
 currentTable.Execute(TableOperation.Insert(ent), options, null);

 // Retrieve Entity
 // No need toospecify an encryption resolver for retrieve
 TableRequestOptions retrieveOptions = new TableRequestOptions()
 {
    EncryptionPolicy = policy
 };

 TableOperation operation = TableOperation.Retrieve(ent.PartitionKey, ent.RowKey);
 TableResult result = currentTable.Execute(operation, retrieveOptions, null);
```

#### <a name="using-attributes"></a>Attribútumok használata
Említetteknek megfelelően, ha hello entitás TableEntity valósítja meg, majd hello tulajdonságok is látható el hello megadása helyett hello [EncryptProperty] attribútummal **EncryptionResolver**.

```csharp
[EncryptProperty]
 public string EncryptedProperty1 { get; set; }
```

## <a name="encryption-and-performance"></a>Titkosítás és teljesítmény
Vegye figyelembe, hogy a tároló eredményezi további teljesítményigény titkosítása. hello kulcs és IV kell létrejönnie, hello tartalom titkosítani kell, és további metaadatok kell formázva és feltöltött tartalom. Ez a terhelés hello titkosított adatok mennyisége függvényében. Azt javasoljuk, hogy az ügyfelek mindig tesztelje az alkalmazások fejlesztése során teljesítmény.

## <a name="next-steps"></a>Következő lépések
* [Oktatóanyag: Titkosításához és visszafejtéséhez az Azure Key Vault használatával a Microsoft Azure Storage blobs](storage-encrypt-decrypt-blobs-key-vault.md)
* Töltse le a hello [Azure Storage ügyféloldali kódtára a .NET NuGet-csomag](https://www.nuget.org/packages/WindowsAzure.Storage)
* Töltse le az Azure Key Vault NuGet hello [Core](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core/), [ügyfél](http://www.nuget.org/packages/Microsoft.Azure.KeyVault/), és [bővítmények](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions/) csomagok  
* A Microsoft hello [Azure Key Vault dokumentációjában](../key-vault/key-vault-whatis.md)
