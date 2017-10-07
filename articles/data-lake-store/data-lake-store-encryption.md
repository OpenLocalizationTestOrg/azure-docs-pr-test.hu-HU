---
title: az Azure Data Lake Store aaaEncryption |} Microsoft Docs
description: "A titkosítás és a kulcsrotálás működésének megismerése az Azure Data Lake Store-ban"
services: data-lake-store
documentationcenter: 
author: yagupta
manager: 
editor: 
ms.assetid: 
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 4/14/2017
ms.author: yagupta
ms.openlocfilehash: a9f3a2dce8232deba93005594d1e6a21e9c0cbee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="encryption-of-data-in-azure-data-lake-store"></a>Az adatok titkosítása az Azure Data Lake Store-ban

A titkosítás az Azure Data Lake Store-ban segíti az adatok védelmét, vállalati biztonsági szabályzatok implementálását és az előírt megfelelőségi követelmények teljesítését. Ez a cikk hello kialakítás áttekintése, és megvalósítási hello műszaki szempontjait és.

A Data Lake Store az adatok titkosítását inaktív adatok és adatátvitel esetén egyaránt támogatja. Inaktív adatok esetén a Data Lake Store csak az alapértelmezés szerinti, átlátható titkosítást támogatja. Kicsit részletesebben kifejtve ez az alábbiakat jelenti:

* **Az alapértelmezés szerint**: egy új Data Lake Store-fiók létrehozásakor hello alapértelmezett beállítás engedélyezi-e a titkosítás. Ezt követően Data Lake Store-ban tárolt adatok mindig titkosított előzetes toostoring állandó adathordozón. Ez a hello működés az összes adat, és fiók létrehozása után nem módosítható.
* **Transzparens**: Data Lake Store automatikusan adatok előzetes toopersisting, és fogadott adatok előzetes tooretrieval titkosítása. hello titkosítási és konfigurált hello Data Lake Store szint: egy rendszergazda felügyeli. Nincs módosításai toohello adatok API-k eléréséhez. Ezért a titkosítás miatt nincs szükség módosításokra a Data Lake Store-ral kommunikáló alkalmazásokban és szolgáltatásokban.

Az átvitt adatok (azaz a mozgásban lévő adatok) titkosítása is mindig a Data Lake Store-ban történik. Ezenkívül tooencrypting előzetes toostoring toopersistent adathordozók, hello adatok is minden esetben biztosítva átvitel közben HTTPS használatával. HTTPS hello egyetlen protokoll, amely Data Lake Store REST felületeihez hello támogatott. hello következő ábra bemutatja, hogyan lesz titkosítva az adatok a Data Lake Store:

![A Data Lake Store-ban végbemenő adattitkosítás ábrája](./media/data-lake-store-encryption/fig1.png)


## <a name="set-up-encryption-with-data-lake-store"></a>Titkosítás beállítása a Data Lake Store-ral

A Data Lake Store titkosításának beállítása mindig a fiók létrehozása során történik, és alapértelmezés szerint minden esetben engedélyezve van. Hello kulcsok kezelése, saját magának, vagy engedélyezze a Data Lake Store toomanage meg (Ez az alapértelmezett hello) őket.

További részletekért lásd [az első lépéseket](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal).

## <a name="how-encryption-works-in-data-lake-store"></a>A titkosítás működésének megismerése a Data Lake Store-ban

hello következőket ismerteti hogyan toomanage fő titkosítási kulcsokat, és elmagyarázza, hello három különböző típusú kulcsokat használhatja a Data Lake Store az adattitkosítás.

### <a name="master-encryption-keys"></a>Titkosítási főkulcsok

A Data Lake Store a titkosítási főkulcsok (Master Encryption Key, MEK) kezelését kétféle módon biztosítja. Most feltételezik hello fő titkosítási kulcs hello legfelső szintű kulcs. Hozzáférés toohello fő titkosítási kulcsa szükséges toodecrypt Data Lake Store-ban tárolt adatokat.

hello fő titkosítási kulcsát kezelésére szolgáló hello két módok a következők:

*   Szolgáltatás által kezelt kulcsok
*   Felhasználó által kezelt kulcsok

Mindkét üzemmódban hello fő titkosítási kulcsát az Azure Key Vault elhelyezésével védett. Key Vault szolgáltatás, amely toosafeguard használt titkosítási kulcsok Azure rendkívül biztonságos teljes körűen felügyelt szolgáltatás. További információkért lásd a [Key Vaultról](https://azure.microsoft.com/services/key-vault) szóló cikket.

Itt található hello két módja hello MEKs kezelése által biztosított képességek rövid összehasonlítása.

|  | Szolgáltatás által kezelt kulcsok | Felhasználó által kezelt kulcsok |
| --- | --- | --- |
|Hogyan történik az adatok tárolása?|Mindig titkosított előzetes toobeing tárolja.|Mindig titkosított előzetes toobeing tárolja.|
|Hol tárolja a titkosítási kulcs hello?|Key Vault|Key Vault|
|Bármely titkosítási kulcsok hello találhatók törölje esnek Key Vault? |Nem|Nem|
|Hello MEK lekérhetők Key Vault?|Nem. Hello MEK Key Vault van tárolva, után csak használat titkosításához és visszafejtéséhez.|Nem. Hello MEK Key Vault van tárolva, után csak használat titkosításához és visszafejtéséhez.|
|Hello Key Vault-példány és hello MEK tulajdonosára?|hello Data Lake Store szolgáltatás|Saját tartozik, a saját Azure-előfizetés hello Key Vault példány. a Key Vault MEK hello szoftveres vagy hardveres kezelhető.|
|Vissza tudja vonni hozzáférés toohello MEK a hello Data Lake Store szolgáltatást?|Nem|Igen. Hozzáférés-vezérlési listák a Key Vault kezeléséhez, és hozzáférést vezérlő bejegyzések toohello szolgáltatás identitásának hello Data Lake Store szolgáltatás eltávolítása.|
|Végleg törölhetők hello MEK?|Nem|Igen. Key Vault hello MEK töröl, hello hello Data Lake Store-fiók adatait nem lehet visszafejteni, bárki, beleértve a hello Data Lake Store szolgáltatást. <br><br> Ha explicit módon biztonsági másolatából hello MEK előzetes toodeleting azt a Key Vault, hello MEK visszaállítható, és hello majd lehessen helyreállítani. Azonban nem biztonsági másolatából hello MEK előzetes toodeleting azt a Key Vault, ha a Data Lake Store-fiók hello hello adatok soha nem visszafejthető ezt követően.|


Ezt a különbséget a vezérelt hello MEK kezelő és hello Key Vault példány, amelyben található, hello tervezési hello részeinek van hello azonos mindkét módnál.

Ha úgy dönt, hello hello mód fő titkosítási kulcsok fontos tooremember hello következő:

*   Kiválaszthatja, hogy toouse ügyfél felügyelt kulcsok vagy felügyelt kulcsai amikor Data Lake Store-fiók.
*   Data Lake Store-fiók üzembe helyezése után hello módja nem módosítható.

### <a name="encryption-and-decryption-of-data"></a>Adattitkosítás és -visszafejtés

Háromféle hello kialakításában adattitkosítási kulcsokat van. hello a következő táblázat az összefoglalást tartalmazza:

| Kulcs                   | Rövidítés | Társítva ezzel: | Tárolási hely                             | Típus       | Megjegyzések                                                                                                   |
|-----------------------|--------------|-----------------|----------------------------------------------|------------|---------------------------------------------------------------------------------------------------------|
| Titkosítási főkulcs | MEK          | Data Lake Store-fiók | Key Vault                              | Aszimmetrikus | Kezelheti a Data Lake Store, vagy Ön is.                                                              |
| Adattitkosítási kulcs   | DEK          | Data Lake Store-fiók | Állandó tároló, a Data Lake Store szolgáltatás által kezelve | Szimmetrikus  | hello adattitkosítási kulcs hello MEK titkosítja. hello titkosított adattitkosítási kulcs mi állandó adathordozón tárolódik. |
| Blokktitkosítási kulcs  | BEK          | Egy adatblokk | None                                         | Szimmetrikus  | hello BEK hello adattitkosítási kulcs és hello adatblokk származik.                                                      |

hello a következő ábra azt mutatja be, ezek a fogalmak:

![Adattitkosítási kulcsok](./media/data-lake-store-encryption/fig2.png)

#### <a name="pseudo-algorithm-when-a-file-is-toobe-decrypted"></a>Amikor egy fájl visszafejtése toobe pszeudo algoritmust:
1.  Annak ellenőrzése, hogy hello Data Lake Store-fiókot az adattitkosítási kulcs hello gyorsítótárazott és készen áll a használatra.
    - Ha nem, olvassa el a hello adattitkosítási kulcs titkosítása a állandó tároló, és elküldi a tooKey tároló toobe visszafejteni. Gyorsítótár hello adattitkosítási kulcs visszafejtése a memóriában. Már most már készen áll a toouse.
2.  A minden hello fájlban adatblokk:
    - Állandó tároló hello titkosított adatblokk olvasni.
    - Az adattitkosítási kulcs hello BEK hello és hello titkosított adatblokk létrehozása.
    - Hello BEK toodecrypt adatok felhasználásával.


#### <a name="pseudo-algorithm-when-a-block-of-data-is-toobe-encrypted"></a>Amikor egy adatblokk pszeudo algoritmus toobe titkosítva:
1.  Annak ellenőrzése, hogy hello Data Lake Store-fiókot az adattitkosítási kulcs hello gyorsítótárazott és készen áll a használatra.
    - Ha nem, olvassa el a hello adattitkosítási kulcs titkosítása a állandó tároló, és elküldi a tooKey tároló toobe visszafejteni. Gyorsítótár hello adattitkosítási kulcs visszafejtése a memóriában. Már most már készen áll a toouse.
2.  Hozzon létre egy egyedi BEK a hello adatblokk hello adattitkosítási kulcsot.
3.  Hello adatblokk hello BEK, az AES-256 titkosítás titkosítására.
4.  Hello titkosított adatblokk adatok tárolása az állandó tároló.

> [!NOTE] 
> A megfelelő teljesítmény érdekében hello hello az adattitkosítási kulcs törlése gyorsítótárban való rövid ideig, és ezt követően azonnal törli. Állandó adathordozón mindig tárolt hello MEK titkosítja.

## <a name="key-rotation"></a>Kulcsrotálás

Ha az ügyfél által felügyelt kulcsokat használ, forgathatók hello MEK. Hogyan tooset ügyfél által felügyelt kulcsokkal, Data Lake Store-fiók létrehozásához: toolearn [bevezetés](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal).

### <a name="prerequisites"></a>Előfeltételek

Hello Data Lake Store-fiók beállításakor kiválasztott toouse saját kulcsok. Ez a beállítás nem módosítható, hello fiók létrehozása után. hello következő lépések azt feltételezik, hogy felhasználó által felügyelt kulcsokat használ (Ez azt jelenti, választott saját kulcsok a Key Vault).

Vegye figyelembe, hogy ha hello alapértelmezett beállítások használata titkosításhoz, a mindig adattitkosítás kezeli a Data Lake Store kulcsok használatával. Ezt a beállítást nem kell hello képességét toorotate kulcsok, Data Lake Store kezeli őket.

### <a name="how-toorotate-hello-mek-in-data-lake-store"></a>Hogyan toorotate hello MEK Data Lake Store-ban

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. Keresse meg, amely tárolja a kulcsokat, a Data Lake Store-fiókjához társított toohello Key Vault példány. Válassza a **Kulcsok** elemet.

    ![Képernyőkép a Key Vaultról](./media/data-lake-store-encryption/keyvault.png)

3.  Válassza ki a Data Lake Store-fiókjához társított hello kulcsot, és ez a kulcs új verziót hoz létre. Vegye figyelembe, hogy Data Lake Store jelenleg csak kulcs Elforgatás tooa új verzióját támogatja egy kulcsot. Nem támogatja a forgó tooa másik kulcsot.

   ![Képernyőkép a Kulcsok ablakról, amelyen az Új verzió elem van kiemelve](./media/data-lake-store-encryption/keynewversion.png)

4.  Keresse meg a Data Lake Store tárfiók toohello, és válassza ki **titkosítási**.

    ![Képernyőkép a Data Lake Store-tárfiók ablakról, amelyen a Titkosítás van kiemelve](./media/data-lake-store-encryption/select-encryption.png)

5.  Egy üzenet értesíti hello kulcs kulcs új verziója érhető el. Kattintson a **elforgatása kulcs** tooupdate hello kulcs toohello új verziója.

    ![Képernyőkép a Data Lake Store ablakról, amelyen az üzenet és a Kulcs rotálása van kiemelve](./media/data-lake-store-encryption/rotatekey.png)

Ez a művelet gyorsabban kevesebb mint két perc alatt, és nincs lejáró tookey Elforgatás várható állásidő nélkül. Hello művelet befejezése után új verziójának hello hello kulcs használatban van.
