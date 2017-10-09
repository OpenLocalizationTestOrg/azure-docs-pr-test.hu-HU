---
title: "a Microsoft Azure Storage Python aaaClient-kiszolgálóoldali titkosítás |} Microsoft Docs"
description: "hello Azure Storage ügyféloldali kódtára a Pythonhoz ügyféloldali titkosítás támogatja az Azure Storage-alkalmazások a maximális biztonság érdekében."
services: storage
documentationcenter: python
author: lakasa
manager: jahogg
editor: tysonn
ms.assetid: f9bf7981-9948-4f83-8931-b15679a09b8a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 05/11/2017
ms.author: lakasa
ms.openlocfilehash: 3a52b64f93daf85a55308f8a4bee9c98b2315d4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="client-side-encryption-with-python-for-microsoft-azure-storage"></a>A Microsoft Azure Storage Python ügyféloldali titkosítás
[!INCLUDE [storage-selector-client-side-encryption-include](../../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Áttekintés
Hello [Azure Storage ügyféloldali kódtára a Pythonhoz](https://pypi.python.org/pypi/azure-storage) támogatja a titkosított adatok ügyfélalkalmazásokon belüli előtt tooAzure tárolási feltöltését, és az adatok visszafejtése toohello ügyfél letöltése során.

> [!NOTE]
> hello Azure Storage Python kódtár előzetes verzió van.
> 
> 

## <a name="encryption-and-decryption-via-hello-envelope-technique"></a>Titkosítás és visszafejtés hello boríték technika keresztül
hello folyamatok titkosítási és visszafejtési hello boríték technika kövesse.

### <a name="encryption-via-hello-envelope-technique"></a>Titkosítási hello boríték technika keresztül
Titkosítási hello boríték technika keresztül működik, a következő módon hello:

1. hello Azure storage ügyféloldali kódtár állít elő, a tartalom titkosítási kulcs (CEK), amely a szimmetrikus kulcs egy egyszeri használata.
2. Felhasználói adatok titkosítása a CEK használatával.
3. hello CEK ezt követően (titkosított) hello kulcs titkosítási kulcs (KEK) használatával. hello KEK kulcsazonosítójával azonosíthatók és aszimmetrikus kulcspár vagy egy szimmetrikus kulcs, amely helyileg kezelt lehet.
   hello a storage ügyféloldali kódtára maga soha nem hozzáférés tooKEK. hello könyvtár hello kulcs alkalmazásburkoló algoritmus hello KEK által biztosított hív meg. -Felhasználók eldönthetik toouse egyéni szolgáltatók a kulcs alkalmazásburkoló/kicsomagolásával igény.
4. hello titkosított adatok van majd toohello Azure Storage szolgáltatás feltöltve. hello becsomagolt kulcs néhány további titkosítási metaadatokból együtt tárolt metaadatok (a blob) vagy köztes értékkel (üzenetsor-üzeneteket és táblaentitásokat) titkosított hello adatokkal.

### <a name="decryption-via-hello-envelope-technique"></a>Visszafejtési hello boríték technika keresztül
Visszafejtési hello boríték technika keresztül működik, a következő módon hello:

1. hello ügyféloldali kódtár azt feltételezi, hogy hello felhasználó helyileg hello kulcs titkosítási kulcs-(KEK) kezel. hello felhasználónak nem kell tooknow hello adott használt kulcs titkosításhoz. Ehelyett egy kulcs feloldó, amely különböző kulcs azonosítók tookeys mutat, beállítása is, és használja.
2. hello ügyféloldali kódtár hello titkosított adatok hello szolgáltatásban tárolt titkosítási anyagok és tölti le.
3. hello burkolt tartalom titkosítási kulcs (CEK) nem burkolatlan (visszafejtett) használatával hello kulcs titkosítási kulcs-(KEK). Itt újra, hello ügyféloldali kódtár nem rendelkezik hozzáféréssel tooKEK. Egyszerűen meghívja a hello egyéni szolgáltató kibontásához algoritmus.
4. tartalom titkosítási kulcsot (CEK) meg kell majd hello használt toodecrypt Titkosított hello felhasználói adatokat.

## <a name="encryption-mechanism"></a>Titkosítási módszer
hello a storage ügyféloldali kódtára használ [AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) rendelés tooencrypt felhasználói adatok. Pontosabban [Cipher Block Chaining (CBC)](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) AES módban. Minden szolgáltatás működik némileg eltér, azok itt ismertetik.

### <a name="blobs"></a>Blobok
hello ügyféloldali kódtára jelenleg csak a teljes blobok titkosítása. Pontosabban, titkosítás támogatott hello használatakor **létrehozása*** módszerek. A letöltések, mindkettő teljes és a tartomány letöltések támogatottak, és párhuzamos folyamatkezelést biztosítja a feltöltés és letöltés érhető el.

Titkosítás során hello ügyféloldali kódtár létre egy véletlenszerű inicializálási vektor (IV) 16 bájt, 32 bájt véletlenszerű tartalom titkosítási kulcs (CEK) együtt, és hajtsa végre a boríték titkosítási hello a blob típusú adatok ezen információk alapján. hello CEK burkolt, és néhány további titkosítási metaadatokból majd tárolja a blob-metaadatok hello titkosított blob hello szolgáltatás együtt.

> [!WARNING]
> Ha szerkeszti, vagy saját metaadatok hello blob feltöltése, módosítani kell, hogy megőrzi a metaadatok tooensure. A metaadatok nélkül új metaadatokkal tölt fel, ha hello burkolt CEK, IV és egyéb metaadatokat elvész, és hello blobtartalom soha nem lesznek lekérhető újra.
> 
> 

Egy titkosított blob letöltése magában foglalja a hello teljes BLOB hello segítségével hello tartalom lekérésének **beolvasása*** kényelmi módszerek. hello burkolt CEK kicsomagolják, és hello IV együtt használja (tárolt blob metaadatai ebben az esetben) tooreturn visszafejteni hello adatok toohello felhasználók.

Egy tetszőleges tartomány letöltése (**beolvasása*** átadott tartomány paraméterekkel módszerek) hello a titkosított blob magában foglalja a megadott sorrendben tooget használható további adatokat kis mennyiségű felhasználók hello tartomány beállítása a kért tartomány toosuccessfully visszafejtése hello.

Blokk blobokat és lapblobokat csak lehet titkosított/visszafejteni a rendszer. Jelenleg nem támogatja a titkosított hozzáfűző blobokat.

### <a name="queues"></a>Üzenetsorok
Üzenetsor-üzeneteket tetszőleges méretű lehet, mert a hello ügyféloldali kódtára tartalmaz hello inicializálási vektor (IV) és hello titkosított tartalom titkosítási kulcs (CEK) hello üzenetszöveg a egyéni formátum határozza meg.

Titkosítás során hello ügyféloldali kódtár hoz létre egy véletlenszerű IV 16 bájt 32 bájt véletlenszerű CEK együtt, és ezen információk alapján hello várólista üzenetszöveg boríték titkosítást végez. hello CEK burkolt, és néhány további titkosítási metaadatokból kerülnek toohello titkosított üzenetsorban lévő üzenetet. A módosított (lásd alább) tárolása pedig hello szolgáltatást.

```
<MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>
```

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
3. hello CEK burkolt, és néhány további titkosítási metaadatokból két további fenntartott tulajdonságokat, majd tárolódnak. hello először fenntartott tulajdonság (\_ClientEncryptionMetadata1) egy karakterlánc-tulajdonság, amely a IV, a verziót és a becsomagolt kulcs hello információkat tárolja. második fenntartott tulajdonság hello (\_ClientEncryptionMetadata2), amely tárolja a Titkosított hello tulajdonságok hello információ bináris tulajdonsága. a második tulajdonságban tárolt adatok hello (\_ClientEncryptionMetadata2) maga is titkosítva.
4. Lejáró toothese fenntartott szükség tovább tulajdonságokra a titkosításhoz felhasználók most már rendelkezik 252 helyett csak 250 egyéni tulajdonságokat. hello entitás hello teljes mérete 1MB-nál kevesebb kell lennie.
   
   Vegye figyelembe, hogy a tulajdonságok csak string titkosíthatók. Ha más típusú tulajdonságok titkosított toobe, átalakított toostrings kell. Titkosított hello karakterláncok bináris tulajdonságként hello szolgáltatásban tárolja, és a visszafejtés után vissza toostrings (nyers karakterláncok, nem típusú EdmType.STRING EntityProperties) telepítésekké lesznek átalakítva.
   
   A táblák, továbbá toohello titkosítási házirendnek, felhasználók meg kell adnia hello tulajdonságok toobe titkosítva. Végezhető el hello típus set tooEdmType.STRING TableEntity objektumok vagy tárolja ezeket a tulajdonságokat, és a set tootrue vagy beállítás hello encryption_resolver_function hello tableservice objektum titkosítására. Egy titkosítási feloldó, akkor a függvény egy partíciókulcsot, sorkulcsot és tulajdonságnév veszi, és logikai érték beolvasása, amely jelzi, hogy a tulajdonság titkosítani kell-e. Titkosítás során hello ügyféloldali kódtár fogja használni a információk toodecide e tulajdonság titkosítani toohello vezetékes írása közben. hello delegált is körül hogyan tulajdonságok vannak titkosítva logika hello lehetőségét biztosítja. (Például, ha X, majd titkosítása A tulajdonság; ellenkező esetben a Tulajdonságok A és b titkosítása) Vegye figyelembe, hogy az nem szükséges tooprovide ezen információ olvasása vagy entitás lekérdezése közben.

### <a name="batch-operations"></a>Kötegelt műveletek
Egy titkosítási házirendek vonatkoznak a tooall sorok hello kötegben. hello ügyféloldali kódtár belső létrehoz egy új véletlenszerű IV és a véletlenszerű CEK soronkénti hello kötegben. Azt is kiválaszthatják tooencrypt minden művelethez más tulajdonságokkal hello kötegben definiálásával Ez a viselkedés a hello titkosítási feloldó.
Ha egy kötegelt hello tableservice batch() metódussal környezetben vezető jön létre, hello tableservice titkosítási házirend automatikusan lesz alkalmazott toohello kötegelt. Egy kötegelt hello konstruktor meghívásával explicit módon jön létre, ha hello titkosítási házirend paraméter és módosítani a hello kötegelt hello élettartama balra kell átadni.
Vegye figyelembe, hogy az entitások titkosítva legyenek, hello kötegelt hello kötegelt titkosítási házirenddel (entitások nincs titkosítva a hello tableservice titkosítási házirenddel hello kötegelt véglegesítése hello időpontjában) való beszúrása.

### <a name="queries"></a>Lekérdezések
tooperform lekérdezési műveletek, meg kell adnia egy kulcs feloldó, amely képes tooresolve összes hello hello eredménykészletben kulcsok. Ha hello lekérdezés eredménye található entitás nem lehet feloldani tooa szolgáltató, hello ügyféloldali kódtár kivételhibát hiba. A lekérdezés, amely végrehajtja a kiszolgáló oldalán leképezések, hello ügyféloldali kódtár felveszi hello különleges titkosítási metaadat-tulajdonságainak (\_ClientEncryptionMetadata1 és \_ClientEncryptionMetadata2) alapértelmezett toohello jelölve oszlopok .

> [!IMPORTANT]
> Vegye figyelembe a fontos pontok ügyféloldali titkosítás használata esetén:
> 
> * A olvasása vagy írása tooan mikor titkosítsa a blob, használjon teljes blob feltöltése és tartomány/egész blob letöltési parancsok. Hogy ne kelljen tooan titkosított blob használatával protokoll műveletek, például a Put blokk, Put tiltólista, lapok írási vagy törölje a jelet lapok; Ellenkező esetben előfordulhat, hogy hello titkosított blob sérült, és könnyebben olvasható.
> * A táblákhoz hasonló korlátozás létezik. Gondos toonot frissítés titkosított tulajdonságok hello paramétertitkosítási metaadatokat frissítése nélkül lehet.
> * Metaadatok hello titkosított blob állít be, ha szükséges, mivel a metaadatok beállítása nem additívak hello titkosítással kapcsolatos metaadatok felülírhatja. Ez akkor is igaz, a pillanatképek; Ne adjon meg a metaadat egy titkosított blob pillanatképének létrehozása során. Metaadatok be kell állítani, ha kell, hogy toocall hello **get_blob_metadata** metódus első tooget hello aktuális paramétertitkosítási metaadatokat, és elkerülheti a egyidejű írási műveleteket, amíg metaadatok beállítása.
> * Hello engedélyezése **require_encryption** jelző hello szolgáltatás objektum olyan felhasználóknál, akik csak a titkosított adatok együtt kell működnie. További információkért lásd az alábbi.
> 
> 

hello a storage ügyféloldali kódtára a hello megadott KEK és kulcs feloldó felület következő tooimplement hello vár. [Az Azure Key Vault](https://azure.microsoft.com/services/key-vault/) támogatja a Python KEK felügyeleti függőben, és integrálni kell a befejezésekor a tárba.

## <a name="client-api--interface"></a>Ügyfél API-ja / felület
Tárolási szolgáltatás objektum (azaz blockblobservice) létrehozását követően hello felhasználói előfordulhat, hogy rendeljen értéket egy titkosítási házirend alkotó toohello mezők: key_encryption_key, key_resolver_function, és require_encryption. KEK csak a felhasználók megadhatják csak egy feloldó, vagy mindkettőt. key_encryption_key hello alapvető kulcs típusa, amely azonosítja a kulcsazonosító és hello logika biztosít, amely alkalmazásburkoló/kicsomagolásával. key_resolver_function hello feloldási folyamat során használt tooresolve kulcs. A megadott kulcs azonosítója érvényes KEK adja vissza. Ez lehetővé teszi a felhasználók hello képességét toochoose több helyen felügyelt több kulcsok között.

hello KEK hello következőt kell megvalósítania módszerek toosuccessfully titkosítsa az adatokat:

* wrap_key(cek): hello becsomagolja megadott CEK (bájt) hello felhasználó választott algoritmust használ. Beolvasása hello becsomagolt kulcs.
* get_key_wrap_algorithm(): beolvasása hello algoritmust használnak toowrap kulcsok.
* get_kid(): beolvasása hello karakterlánc kulcs azonosítója a KEK.
  hello KEK implementálnia kell a következő módszerek toosuccessfully visszafejtése adatok hello:
* (cek, algoritmus) unwrap_key: hello kicsomagolják hello formája értéket ad vissza a megadott CEK hello karakterlánc algoritmus használatával.
* get_kid(): egy karakterlánc-azonosítója a KEK adja vissza.

hello kulcs feloldó legalább meg kell valósítania egy metódust,, hogy egy azonosítója, adott hello megfelelő KEK végrehajtási hello illesztő fenti adja vissza. Ez a módszer csak toohello key_resolver_function tulajdonság hello szolgáltatás objektumhoz rendelt toobe.

* A titkosításhoz hello kulccsal mindig, és egy kulcs hiányában hello hibát eredményez.
* A visszafejtéshez:
  
  * Ha meg van adva tooget hello kulcs hello kulcs feloldó meghívták. Hello feloldó van megadva, de nem rendelkezik hello kulcsazonosító társítás, ha hiba fordul elő.
  * Ha nincs megadva a feloldó, de egy kulcs van megadva, hello kulcs akkor használatos, ha az azonosító szükséges hello kulcs azonosítója megegyezik-e. Hello azonosítója nem egyezik, ha hiba fordul elő.
    
    titkosítási minták azure.storage.samples hello <fix URL>egy BLOB, üzenetsorok és táblák részletesebb végpont forgatókönyv bemutatásához.
      Minta implementációja hello KEK és kulcs feloldó találhatók: hello mintafájlok KeyWrapper és KeyResolver kulcsattribútumokkal.

### <a name="requireencryption-mode"></a>RequireEncryption mód
Felhasználók engedélyezheti üzemmódot ahol feltöltések és a letöltött fájl titkosítva kell lennie. Ebben a módban a kísérletek tooupload adatok egy titkosítási házirend és letöltés választható adatokat nem titkosított hello szolgáltatás nélkül az hello ügyfél sikertelen lesz. Hello **require_encryption** hello szolgáltatás objektum vezérlők jelzőt ezt a viselkedést.

### <a name="blob-service-encryption"></a>BLOB szolgáltatás titkosítási
Hello titkosítási házirendmezők hello blockblobservice objektum állítható be. Minden más kezelik hello ügyféloldali kódtár által belsőleg.

```python
# Create hello KEK used for encryption.
# KeyWrapper is hello provided sample implementation, but hello user may use their own object as long as it implements hello interface above.
kek = KeyWrapper('local:key1') # Key identifier

# Create hello key resolver used for decryption.
# KeyResolver is hello provided sample implementation, but hello user may use whatever implementation they choose so long as hello function set on hello service object behaves appropriately.
key_resolver = KeyResolver()
key_resolver.put_key(kek)

# Set hello KEK and key resolver on hello service object.
my_block_blob_service.key_encryption_key = kek
my_block_blob_service.key_resolver_funcion = key_resolver.resolve_key

# Upload hello encrypted contents toohello blob.
my_block_blob_service.create_blob_from_stream(container_name, blob_name, stream)

# Download and decrypt hello encrypted contents from hello blob.
blob = my_block_blob_service.get_blob_to_bytes(container_name, blob_name)
```

### <a name="queue-service-encryption"></a>Várólista titkosítását
Hello titkosítási házirendmezők hello queueservice objektum állítható be. Minden más kezelik hello ügyféloldali kódtár által belsőleg.

```python
# Create hello KEK used for encryption.
# KeyWrapper is hello provided sample implementation, but hello user may use their own object as long as it implements hello interface above.
kek = KeyWrapper('local:key1') # Key identifier

# Create hello key resolver used for decryption.
# KeyResolver is hello provided sample implementation, but hello user may use whatever implementation they choose so long as hello function set on hello service object behaves appropriately.
key_resolver = KeyResolver()
key_resolver.put_key(kek)

# Set hello KEK and key resolver on hello service object.
my_queue_service.key_encryption_key = kek
my_queue_service.key_resolver_funcion = key_resolver.resolve_key

# Add message
my_queue_service.put_message(queue_name, content)

# Retrieve message
retrieved_message_list = my_queue_service.get_messages(queue_name)
```

### <a name="table-service-encryption"></a>TABLE szolgáltatás titkosítási
Ezenkívül egy titkosítási házirend toocreating és beállítása a kérelem lehetőségekről, meg kell adnia egy **encryption_resolver_function** a hello **tableservice**, vagy a set hello attribútum titkosítása a hello EntityProperty.

### <a name="using-hello-resolver"></a>Hello feloldó használata

```python
# Create hello KEK used for encryption.
# KeyWrapper is hello provided sample implementation, but hello user may use their own object as long as it implements hello interface above.
kek = KeyWrapper('local:key1') # Key identifier

# Create hello key resolver used for decryption.
# KeyResolver is hello provided sample implementation, but hello user may use whatever implementation they choose so long as hello function set on hello service object behaves appropriately.
key_resolver = KeyResolver()
key_resolver.put_key(kek)

# Define hello encryption resolver_function.
def my_encryption_resolver(pk, rk, property_name):
        if property_name == 'foo':
                return True
        return False

# Set hello KEK and key resolver on hello service object.
my_table_service.key_encryption_key = kek
my_table_service.key_resolver_funcion = key_resolver.resolve_key
my_table_service.encryption_resolver_function = my_encryption_resolver

# Insert Entity
my_table_service.insert_entity(table_name, entity)

# Retrieve Entity
# Note: No need toospecify an encryption resolver for retrieve, but it is harmless tooleave hello property set.
my_table_service.get_entity(table_name, entity['PartitionKey'], entity['RowKey'])
```

### <a name="using-attributes"></a>Attribútumok használata
Fent említett tulajdonság lehetséges, hogy megjelölve titkosításhoz EntityProperty objektum tárolja őket, és mező hello beállítása titkosításához.

```python
encrypted_property_1 = EntityProperty(EdmType.STRING, value, encrypt=True)
```

## <a name="encryption-and-performance"></a>Titkosítás és teljesítmény
Vegye figyelembe, hogy a tároló eredményezi további teljesítményigény titkosítása. hello kulcs és IV kell létrejönnie, hello tartalom titkosítva kell lennie és további metaadatokat kell formázva és feltöltött tartalom. Ez a terhelés hello titkosított adatok mennyisége függvényében. Azt javasoljuk, hogy az ügyfelek mindig tesztelje az alkalmazások fejlesztése során teljesítmény.

## <a name="next-steps"></a>Következő lépések
* Töltse le a hello [Azure Storage ügyféloldali kódtára a Java PyPi csomag](https://pypi.python.org/pypi/azure-storage)
* Töltse le a hello [Azure Storage ügyféloldali kódtára a Python forráskód a Githubról](https://github.com/Azure/azure-storage-python)
