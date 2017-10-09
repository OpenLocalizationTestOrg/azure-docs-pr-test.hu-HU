---
title: "a hozzáférés-vezérlés a Data Lake Store aaaOverview |} Microsoft Docs"
description: "A hozzáférés-vezérlés működésének megismerése az Azure Data Lake Store szolgáltatásban"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: d16f8c09-c954-40d3-afab-c86ffa8c353d
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 1cc5d578f22ef0a123a1547abebfb4795ea09139
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="access-control-in-azure-data-lake-store"></a>Az Azure Data Lake Store szolgáltatásban található hozzáférés-vezérlés

Azure Data Lake Store bevezet egy hozzáférés-vezérlési modell, amely a HDFS, ami viszont származik hello POSIX hozzáférés-vezérlési modell származik. Ez a cikk a hozzáférés-vezérlési modell hello a Data Lake Store hello alapjait foglalja össze. toolearn hello HDFS hozzáférés-vezérlési modell, kapcsolatos további információkért lásd: [HDFS engedélyek útmutató](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html).

## <a name="access-control-lists-on-files-and-folders"></a>Fájlokra és mappákra vonatkozó hozzáférés-vezérlési listák

Kétféle hozzáférés-vezérlési lista (ACL) létezik – a **hozzáférési ACL** és az **alapértelmezett ACL**.

* **ACL-ek hozzáférés**: A hozzáférés tooan vezérlőobjektumot. A fájlok és mappák egyaránt rendelkeznek hozzáférési ACL-lel.

* **Hozzáférés-vezérlési listákat alapértelmezett**: A "sablon" ACL-ek társított egy mappát, amelyekkel meghatározhatja, hogy a mappa alatti létrehozott gyermekelemeket hello hozzáférés-vezérlési listáit. A fájlok nem rendelkeznek alapértelmezett ACL-ekkel.

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-acls-1.png)

Hozzáférés-vezérlési listák és a hozzáférés-vezérlési listákat alapértelmezett rendelkezik hello azonos struktúra.

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-acls-2.png)



> [!NOTE]
> Változó hello alapértelmezett hozzáférés-vezérlési lista egy szülő nincs hatással Access ACL vagy alapértelmezett hozzáférés-vezérlési lista gyermek elemek, amelyek már létezőek hello.
>
>

## <a name="users-and-identities"></a>Felhasználók és identitások

Minden fájl és mappa külön engedélyekkel rendelkezik az alábbi identitásokhoz:

* tulajdonos felhasználó hello fájl hello
* hello tulajdonos csoport
* Nevesített felhasználók
* Nevesített csoportok
* Minden egyéb felhasználó

a felhasználók és csoportok hello identitások olyan Azure Active Directory (Azure AD) identitás. Hacsak "," Data Lake Store hello környezetében műveleteket hajthatja végre, vagy: az Azure AD-felhasználó vagy az Azure Active Directory-biztonsági csoportnak.

## <a name="permissions"></a>Engedélyek

a fájlrendszer objektumon hello engedélyek **olvasási**, **írási**, és **Execute**, és azok is használható a fájlok és mappák hello a következő táblázatban ismertetett módon:

|            |    Fájl     |   Mappa |
|------------|-------------|----------|
| **Olvasás (R)** | A fájl tartalmának hello olvashatja | Szükséges **olvasási** és **Execute** toolist hello hello mappa tartalma|
| **Írás (W)** | Kiírhatja vagy tooa fájl hozzáfűzése | Szükséges **írási** és **Execute** toocreate gyermek elemek mappában |
| **Végrehajtás (X)** | Nem jelenti azt minden Data Lake Store hello kontextusában | Szükséges tootraverse hello gyermekincidenseket mappa |

### <a name="short-forms-for-permissions"></a>Az engedélyek rövid alakjai

**RWX** használt tooindicate van **olvasási + írási + hajtható végre**. Több tömörített numerikus űrlap szerepel, amely **olvasási = 4**, **írási = 2**, és **Execute = 1**, hello sum, amelyek hello engedélyek jelöli. Az alábbiakban néhány példa látható.

| Numerikus alak | Rövid alak |      Jelentés     |
|--------------|------------|------------------------|
| 7            | RWX        | Olvasás + Írás + Végrehajtás |
| 5            | R-X        | Olvasás + Végrehajtás         |
| 4            | R--        | Olvasás                   |
| 0            | ---        | Nincs engedély         |


### <a name="permissions-do-not-inherit"></a>Az engedélyek nem öröklődnek

Egy elem engedélyek hello POSIX-stílusú modellben, melynek használatával a Data Lake Store hello elemet tárolja. Más szóval engedélyek egy elem nem örökölhetők hello szülő elemeket.

## <a name="common-scenarios-related-toopermissions"></a>Gyakori forgatókönyvek kapcsolódó toopermissions

Az alábbiakban néhány gyakori forgatókönyvek toohelp tisztában milyen engedélyekre van szükség tooperform a Data Lake Store-fiók bizonyos műveletek.

### <a name="permissions-needed-tooread-a-file"></a>A fájl tooread szükséges engedélyek

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-acls-3.png)

* Hello fájl toobe olvasni, a hello hívó igények **olvasási** engedélyek.
* Az összes hello hello gyökérmappa-szerkezetében lévő hello fájlt tartalmazó mappához, hello hívó igények **Execute** engedélyek.

### <a name="permissions-needed-tooappend-tooa-file"></a>A szükséges engedélyek tooappend tooa fájl

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-acls-4.png)

* A hello fájl toobe fűzött, hello hívó igények **írási** engedélyek.
* Az összes hello hello fájlt tartalmazó mappához, hello hívó igények **Execute** engedélyek.

### <a name="permissions-needed-toodelete-a-file"></a>A fájl toodelete szükséges engedélyek

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-acls-5.png)

* Hello szülőmappa hello a hívó igények **írási + hajtható végre** engedélyek.
* Az összes hello más mappákat hello fájl elérési útját, hello hívó igények **Execute** engedélyek.



> [!NOTE]
> Az írási hello fájlra vonatkozó engedélyeket nincsenek azt is hello az előző két feltétel teljesülése szükséges toodelete.
>
>

### <a name="permissions-needed-tooenumerate-a-folder"></a>A mappa tooenumerate szükséges engedélyek

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-acls-6.png)

* Hello mappa tooenumerate, hello a hívó igények **olvasási + Execute** engedélyek.
* Az összes hello elődje mappák, hello hívó igények **Execute** engedélyek.

## <a name="viewing-permissions-in-hello-azure-portal"></a>Engedélyek megtekintése a hello Azure-portálon

A hello **adatkezelő** panelen található hello Data Lake Store-fiókot, kattintson a **hozzáférés** toosee hello ACL-ek egy fájl vagy mappa. Kattintson a **hozzáférés** toosee hello hozzáférés-vezérlési listákat a hello **katalógus** hello mappája **mydatastore** fiók.

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-show-acls-1.png)

A panel felső részében a hello hello az engedélyeket, hogy áttekintését mutatja. (Hello képernyőkép hello felhasználó, Bob.) Hogy a következő hello hozzáférési engedélyek jelennek meg. Ezután a hello **hozzáférés** panelen kattintson a **egyszerű nézet** toosee hello egyszerűbb.

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-show-acls-simple-view.png)

Kattintson a **speciális nézet** toosee hello fejlettebb nézet, ahol alapértelmezett hozzáférés-vezérlési listákat, maszk és felettes felhasználói hello elveit jelennek meg.

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-show-acls-advance-view.png)

## <a name="hello-super-user"></a>hello felettes felhasználó

Felettes felhasználói jogosultsága hello legtöbb felhasználók hello a Data Lake Store hello. A felügyelő:

* RWX jogosult túl**összes** fájlok és mappák.
* Módosíthatja bármely fájl vagy mappa hello engedélyeit.
* Tulajdonos felhasználó vagy csoport bármely fájl vagy mappa a tulajdonos hello módosíthatja.

A Data Lake Store-fiók számos szerepkörrel rendelkezik az Azure-ban, többek között:

* Tulajdonosok
* Közreműködők
* Olvasók

Mindenki hello **tulajdonosok** Data Lake Store-fiók szerepköre automatikusan egy felettes felhasználói fiók. több, lásd: toolearn [szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md).
Egy egyéni szerepkör alapú hozzáférés-vezérlést (RBAC) szerepkör, amely felettes felhasználó jogosult toocreate azt szeretné, hogy szükséges-e toohave hello alábbi engedélyek használata:
- Microsoft.DataLakeStore/accounts/Superuser/action
- Microsoft.Authorization/roleAssignments/write


## <a name="hello-owning-user"></a>hello tulajdonos felhasználó

hello hello elemet létrehozó felhasználó a rendszer automatikusan a tulajdonos felhasználó hello elem hello. A tulajdonos felhasználó:

* A fájl birtokolt hello engedélyeinek módosítása.
* Tulajdonos, amelyet a tulajdonosa, csoport, mindaddig, amíg hello tulajdonos felhasználó tagja is hello célcsoport hello módosítása.

> [!NOTE]
> tulajdonos felhasználó hello *nem* hello tulajdonos felhasználó egy másik tulajdonában lévő fájl módosítása. Csak felettes módosíthatók hello tulajdonos felhasználó a fájl vagy mappa.
>
>

## <a name="hello-owning-group"></a>hello tulajdonos csoport

Hello POSIX hozzáférés-vezérlési listákat, az összes felhasználó csoportjához társítva egy "elsődleges." Felhasználói "alice" lehet, hogy például toohello "Pénzügy" csoport tartozik. Alice is lehet, hogy tartozik toomultiple csoportok, de egy csoport mindig az elsődleges csoportja van kijelölve. POSIX Ágnes fájlt hoz létre, amikor tulajdonos, hogy a fájl csoport hello beállítása tooher elsődleges csoport, amely ebben az esetben "Pénzügy."

Amikor egy új fájlrendszer elem jön létre, a Data Lake Store egy érték toohello tulajdonos csoport rendeli hozzá.

* **1. eset**: hello gyökérmappa "/". A mappa a Data Lake Store-fiók létrehozásakor jön létre. Ebben az esetben hello tulajdonos csoportja toohello hello fiókot létrehozó felhasználó.
* **2. eset** (minden más esetben): egy új elem létrehozásakor hello tulajdonos csoport hello szülőmappa átmásolva.

hello tulajdonos csoport módosíthatja:
* Bármely felügyelő.
* tulajdonos felhasználó, ha hello tulajdonos felhasználó tagja is hello célcsoport hello.

## <a name="access-check-algorithm"></a>Hozzáférés-ellenőrzési algoritmus

a következő ábra hello hello hozzáférés ellenőrzése algoritmus Data Lake Store-fiók jelöli.

![Data Lake Store ACL-ek algoritmusa](./media/data-lake-store-access-control/data-lake-store-acls-algorithm.png)


## <a name="hello-mask-and-effective-permissions"></a>hello maszk és a "hatályos engedélyek"

Hello **maszk** van egy RWX érték, amely használt toolimit hozzáférés **megnevezett felhasználó**, hello **tulajdonos csoport**, és **csoportok nevű** közben hello hozzáférés ellenőrzése algoritmus végrehajtása. Az alábbiakban hello alapfogalmakat hello maszk.

* hello maszk hoz "felhasználóra vonatkozó engedélyeket." Ez azt jelenti, hogy módosítja a hello engedélyeket a hozzáférés-ellenőrzés hello időpontjában.
* hello maszk hello fájltulajdonos és felettes felhasználók közvetlenül szerkeszthetik.
* hello maszk engedélyek toocreate hello hatékony engedély eltávolítása. hello maszk *nem* engedélyek toohello hatékony engedély hozzá.

Lássunk néhány példát. A következő példa hello, hello maszk túl van beállítva**RWX**, ami azt jelenti, hogy hello maszk nem távolítja el azokat az engedélyeket. a felhasználó, csoport tulajdonos, és a csoport neve nevű hello hello érvényes engedélyeit nem változnak hello hozzáférés-ellenőrzés során.

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-acls-mask-1.png)

A következő példa hello, hello maszk túl van beállítva**R-X**. Ez azt jelenti, hogy az informatikai **hello írási engedélyek kikapcsolja** a **megnevezett felhasználó**, **tulajdonos csoport**, és **nevű csoport** hozzáférés hello helyreállításkor jelölőnégyzet.

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-acls-mask-2.png)

Összehasonlításul ez hol egy fájl vagy mappa hello maszkját hello Azure-portálon jelenik meg.

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-show-acls-mask-view.png)

> [!NOTE]
> Az új Data Lake Store-fiókba, hello hozzáférés ACL hello maszkját és alapértelmezett hozzáférés-vezérlési lista hello legfelső szintű mappa ("/") tooRWX alapértelmezés szerint.
>
>

## <a name="permissions-on-new-files-and-folders"></a>Új fájlok és mappák engedélyei

Amikor egy új fájl vagy mappa egy meglévő mappában jön létre, hello alapértelmezett hozzáférés-vezérlési listája hello szülőmappa határozza meg:

- Gyermekmappa alapértelmezett ACL-jei és hozzáférési ACL-jei.
- Gyermekfájl hozzáférési ACL-je (a fájloknak nincs alapértelmezett ACL-je).

### <a name="hello-access-acl-of-a-child-file-or-folder"></a>hello hozzáférés ACL gyermek fájl vagy mappa

Gyermek-fájl vagy mappa jön létre, hello szülő alapértelmezett hozzáférés-vezérlési lista másolja a program hello hozzáférés ACL hello gyermek fájl vagy mappa. Emellett ha **más** felhasználói hello szülő alapértelmezett hozzáférés-vezérlési lista RWX jogokkal rendelkezik, a rendszer eltávolítja hello alárendelt elem hozzáférés ACL.

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-acls-child-items-1.png)

A legtöbb esetben hello előző információk is, minden van szüksége arról, hogyan határozza meg, a gyermek-konfigurációelem hozzáférés ACL tooknow. Azonban ha már ismeri POSIX rendszerekkel és a kívánt toounderstand részletes hogyan lehet teljesíteni az átalakítási, című hello [Umask tartozó szerepkör hello hozzáférés ACL, új fájlok és mappák létrehozása a](#umasks-role-in-creating-the-access-acl-for-new-files-and-folders) című cikkben.


### <a name="a-child-folders-default-acl"></a>Gyermekmappa alapértelmezett ACL-je

Amikor egy gyermekmappába létrejön egy fölérendelt mappája, hello szülőmappa alapértelmezett hozzáférés-vezérlési lista másolja keresztül, toohello alárendelt mappát alapértelmezett hozzáférés-vezérlési lista.

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-acls-child-items-2.png)

## <a name="advanced-topics-for-understanding-acls-in-data-lake-store"></a>Haladó témakörök a Data Lake Store-ban található ACL-ek megismeréséhez

Az alábbiakban néhány speciális témakörök toohelp megismerte a hozzáférés-vezérlési listák hogyan határozza meg a Data Lake Store-fájlok és mappák.

### <a name="umasks-role-in-creating-hello-access-acl-for-new-files-and-folders"></a>A hozzáférés ACL hello új fájlok és mappák létrehozása Umask tartozó szerepkör

A POSIX-kompatibilis rendszer hello általános fogalma a umask értéke 9 bites hello szülőmappától tootransform hello engedély által használt **tulajdonos felhasználó**, **tulajdonos csoport**, és  **más** a hello hozzáférés ACL új gyermek-fájl vagy mappa. hello bitek száma egy umask ki mely bits tooturn hello alárendelt elem hozzáférés hozzáférés-vezérlési listában azonosításához. Így a rendszer tooselectively megakadályozása hello propagálás engedélyeinek **tulajdonos felhasználó**, **tulajdonos csoport**, és **más**.

Egy HDFS rendszerben hello umask általában a rendszergazdák által szabályozott sitewide konfigurációs beállítás. A Data Lake Store **egész fiókra kiterjedő umaskot** használ, amelyet nem lehet megváltoztatni. a következő táblázat azt mutatja be hello hello fedje fel a Data Lake Store.

| Felhasználói csoport  | Beállítás | Hatás az új gyermekelemek hozzáférési ACL-jére |
|------------ |---------|---------------------------------------|
| Tulajdonos felhasználó | ---     | Nincs hatás                             |
| Tulajdonoscsoport| ---     | Nincs hatás                             |
| Egyéb       | RWX     | Olvasás + Írás + Végrehajtás eltávolítása         |

a következő ábra hello a umask művelet jeleníti meg. hello nettó hatása az tooremove **olvasási + írási + hajtható végre** a **más** felhasználó. Mivel hello umask nem adta meg a bits **tulajdonos felhasználó** és **tulajdonos csoport**, ezeket az engedélyeket nem átalakításából származnak.

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-acls-umask.png)

### <a name="hello-sticky-bit"></a>hello állandóságát bit

hello állandóságát bit POSIX fájlrendszer egy speciális funkciója. A Data Lake Store hello környezetében nem valószínű, hogy hello állandóságát bit van szükség.

hello következő táblázatban a Data Lake Store hello állandóságát bit működése.

| Felhasználói csoport         | Fájl    | Mappa |
|--------------------|---------|-------------------------|
| Ragadós bit **KI** | Nincs hatás   | Nincs hatás.           |
| Ragadós bit **BE**  | Nincs hatás   | Megakadályozza, hogy a kívül senkivel **felettes felhasználók** és hello **tulajdonos felhasználó** egy alárendelt elem törlését vagy, hogy az alárendelt elem átnevezése.               |

hello állandóságát bit hello Azure-portál nem látható.

## <a name="common-questions-about-acls-in-data-lake-store"></a>A Data Lake Store-ban található ACL-lel kapcsolatos gyakori kérdések

Az alábbiakban néhány gyakori kérdést talál, amelyek a Data Lake Store-ban található ACL-ekkel kapcsolatban merülnek fel.

### <a name="do-i-have-tooenable-support-for-acls"></a>ACL-ek támogatása tooenable van?

Nem. Az ACL-alapú hozzáférés-vezérlés mindig be van kapcsolva egy Data Lake Store-fióknál.

### <a name="which-permissions-are-required-toorecursively-delete-a-folder-and-its-contents"></a>Milyen engedélyek a szükséges toorecursively törölje a mappát és annak tartalmát?

* rendelkeznie kell hello szülőmappa **írási + hajtható végre** engedélyek.
* hello a törölt mappa toobe, és minden mappában, igényel **olvasási + írási + hajtható végre** engedélyek.

> [!NOTE]
> Nem kell a mappákban toodelete fájlok írási engedéllyel. Emellett gyökérmappa hello "/" is **soha nem** törölhető.
>
>

### <a name="who-is-hello-owner-of-a-file-or-folder"></a>Egy fájl vagy mappa hello tulajdonosa?

egy fájl vagy mappa hello létrehozója lesz hello tulajdonosa.

### <a name="which-group-is-set-as-hello-owning-group-of-a-file-or-folder-at-creation"></a>Mely csoportja hello tulajdonos a fájl vagy mappa a létrehozásakor csoport?

hello tulajdonos csoport tulajdonos csoport mely hello alatt új fájl vagy mappa létre hello szülőmappa hello átmásolva.

### <a name="i-am-hello-owning-user-of-a-file-but-i-dont-have-hello-rwx-permissions-i-need-what-do-i-do"></a>Tulajdonos felhasználó fájl hello vagyok, de nincs hello RWX engedélyeket kell. Mit tegyek?

hello tulajdonos a felhasználók módosíthatják hello fájl toogive hello engedélyeit maguk RWX engedéllyel a van szükségük.

### <a name="when-i-look-at-acls-in-hello-azure-portal-i-see-user-names-but-through-apis-i-see-guids-why-is-that"></a>Hozzáférés-vezérlési listák el az Azure-portálon hello felhasználónevek látható, de API-k, segítségével látható GUID, miért van, amely?

Hello ACL bejegyzések GUID-EK toousers megfelelnek az Azure ad-ben tárolódnak. API-k hello hello GUID, adja vissza. hello Azure-portálon toomake hozzáférés-vezérlési listák könnyebb toouse megpróbál által fordítása hello GUID rövid nevekké, amikor lehetséges.

### <a name="why-do-i-sometimes-see-guids-in-hello-acls-when-im-using-hello-azure-portal"></a>Miért néha látható GUID hello ACL hello Azure portál használata során?

A GUID látható hello felhasználó az Azure AD többé nem létezik. Általában ez történik, amikor hello felhasználó elhagyta hello vállalati, vagy ha a fiók törölve lett az Azure ad-ben.

### <a name="does-data-lake-store-support-inheritance-of-acls"></a>Támogatja a Data Lake Store az ACL-ek öröklését?

Nem.

### <a name="what-is-hello-difference-between-mask-and-umask"></a>Mi az, hogy a maszk és umask hello különbségének?

| mask | umask|
|------|------|
| Hello **maszk** tulajdonság érhető el az összes fájlt és mappát. | Hello **umask** hello Data Lake Store-fiók tulajdonsága. Így nincs csak egyetlen umask hello Data Lake Store.    |
| tulajdonos felhasználó vagy a tulajdonos a fájl vagy egy felettes felhasználói csoport hello hello mask tulajdonság egy fájl vagy mappa lehet megváltoztatni. | bármely felhasználó, akár egy felettes felhasználó hello umask tulajdonság nem lehet módosítani. Ez egy megváltoztathatatlan, állandó érték.|
| hello mask tulajdonság használatos hello hozzáférés ellenőrzése algoritmus futásidejű toodetermine, hogy egy felhasználó hello jobb tooperform rendelkezik a művelet egy fájl vagy mappa. hello hello maszk szerepköre toocreate "hatályos engedélyek" a hozzáférés-ellenőrzés hello időpontjában. | hello umask nem minden használt hozzáférés-ellenőrzés során. hello umask használt toodetermine hello hozzáférés ACL az új gyermek cikkek mappa. |
| hello maszk értéke egy 3 bites RWX toonamed felhasználói, a nevesített csoport és a tulajdonos felhasználó alkalmazó a hozzáférés-ellenőrzés hello időpontjában.| hello umask értéke 9 bites tulajdonos felhasználó, csoport, a tulajdonos toohello alkalmazó és **más** új gyermek.|

### <a name="where-can-i-learn-more-about-posix-access-control-model"></a>Hol tudhatok meg többet a POSIX hozzáférés-vezérlési modellről?

* [POSIX hozzáférés-vezérlési listák Linux rendszeren](http://www.vanemery.com/Linux/ACL/POSIX_ACL_on_Linux.html)

* [HDFS-engedélyek útmutatója](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html)

* [POSIX – gyakori kérdések](http://www.opengroup.org/austin/papers/posix_faq.html)

* [POSIX 1003.1 2008](http://standards.ieee.org/findstds/standard/1003.1-2008.html)

* [POSIX 1003.1 2013](http://pubs.opengroup.org/onlinepubs/9699919799.2013edition/)

* [POSIX 1003.1 2016](http://pubs.opengroup.org/onlinepubs/9699919799.2016edition/)

* [POSIX ACL Ubuntu rendszeren](https://help.ubuntu.com/community/FilePermissionsACLs)

* [Hozzáférés-vezérlési listákat használó ACL Linux rendszeren](http://bencane.com/2012/05/27/acl-using-access-control-lists-on-linux/)

## <a name="see-also"></a>Lásd még:

* [Az Azure Data Lake Store áttekintése](data-lake-store-overview.md)
