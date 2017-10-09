---
title: "aaaData katalógus fejlesztői fogalmak |} Microsoft Docs"
description: "Bevezetés toohello alapfogalmak Azure Data Catalog fogalmi modell, mint kitett hello Catalog REST API-n keresztül."
services: data-catalog
documentationcenter: 
author: spelluru
manager: jhubbard
editor: 
tags: 
ms.assetid: 89de9137-a0a4-40d1-9f8d-625acad31619
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/03/2017
ms.author: spelluru
ms.openlocfilehash: d0b1628ff6c31458cb650efef852244f0c139b1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-catalog-developer-concepts"></a>Az Azure Data Catalog fejlesztői fogalmak
Microsoft **Azure Data Catalog** egy teljes körűen felügyelt felhőszolgáltatás, amely az adatforrás-felderítés és a közösségi adatforrások metaadatainak képességeket biztosít. A fejlesztők a hello szolgáltatást a REST API-kon keresztül. A fejlesztők toosuccessfully integrálása fontos hello szolgáltatás megvalósított hello alapfogalmainak ismertetése **Azure Data Catalog**.

## <a name="key-concepts"></a>Fő fogalmak
Hello **Azure Data Catalog** fogalmi modell négy alapvető fogalmakat alapul: hello **katalógus**, **felhasználók**, **eszközök**, és **Jegyzetek**.

![A koncepció][1]

*1. ábra – az Azure Data Catalog egyszerűsített fogalmi modellt*

### <a name="catalog"></a>Alkalmazáskatalógus
A **katalógus** hello legfelső szintű tárolója van, az összes hello metaadatok szervezet tárolja. Van egy **katalógus** egy Azure-fiók lehet. Katalógusok a feltételekhez tooan Azure-előfizetésre, de csak egy **katalógus** hozható létre a megadott Azure-fiókra, annak ellenére, hogy a fiók több előfizetéssel is rendelkezhet.

A katalógus tartalmaz **felhasználók** és **eszközök**.

### <a name="users"></a>Felhasználók
Felhasználók a rendszerbiztonsági tag engedélyek tooperform műveletet kell végrehajtani (hello katalógust, hozzáadásához, szerkesztéséhez vagy távolítsa el az elemeket, stb.) a katalógus hello.

Egy felhasználó rendelkezhet több különböző szerepkör is létezik. A szerepkörök további információkért lásd: hello szakasz szerepkörök és az engedélyezés.

Az egyes felhasználók és biztonsági csoportok is hozzáadhatók.

Az Azure Data Catalog az Azure Active Directory az identitások és hozzáférések felügyeletéhez. Minden egyes Alkalmazáskatalógus felhasználói az Active Directory hello hello fiók tagjának kell lennie.

### <a name="assets"></a>Objektumok
A **katalógus** adategységeket tartalmazza. **Eszközök** hello katalógus által kezelt granularitási hello egységének vannak.

egy eszköz hello granularitási adatforrás függ. Az SQL Server vagy Oracle-adatbázishoz az eszköz lehet egy táblát vagy nézetet. Az SQL Server Analysis Services az eszköz lehet egy mérték, a dimenzió vagy egy fő teljesítménymutató (KPI). SQL Server Reporting Services egy jelentést.

Egy **eszköz** hozzáadása vagy eltávolítása a katalógus hello jelenti. Vissza az eredmény hello egység **keresési**.

Egy **eszköz** a nevét, helyét, és típusa, és további leíró jegyzetek épül fel.

### <a name="annotations"></a>Jegyzetek
Jegyzetek olyan elemek, amelyek megfelelnek az eszközökre vonatkozó metaadatok.

Jegyzetek többek között a leírás, a címkék, a séma, a dokumentáció, a stb. Teljes listáját és a Jegyzet típusok hello eszköz esetében hello eszköz objektum modell szakaszban szerepelnek.

## <a name="crowdsourcing-annotations-and-user-perspective-multiplicity-of-opinion"></a>Közösségi jegyzeteket és a felhasználó szempontból (véleményével multiplicitása)
Az Azure Data Catalog egyik legfőbb szempontja hello közösségi metaadat támogatás hello rendszerben. Megakadályozását tooa wiki megközelítést, – ha nem csak egy véleményével és hello utolsó író wins – hello Azure Data Catalog modell lehetővé teszi, hogy több vélemények toolive hello rendszerben egymás mellett.

Ez a megközelítés tükrözi hello valós vállalati adatok ahol más felhasználók is rendelkezik különböző szempontok szerint egy adott eszköz:

* Adatbázis-rendszergazda rendelkezhetnek információk a szolgáltatásszint-szerződések és hello elérhető feldolgozási időszakában tömeges ETL műveletek
* Egy adatok steward hello üzleti folyamatok toowhich hello eszköz adatainak vonatkozik, vagy üzleti hello hello besorolásoknak alkalmazva van-e tooit rendelkezhetnek.
* Egy pénzügyi elemző előfordulhat, hogy ismertetik, hogyan használja fel hello adatok időszak vége jelentéskészítési műveletek során

toosupport ebben a példában minden egyes felhasználói – hello DBA hello adatok steward és hello elemző – leírás tooa egy táblát a katalógus hello regisztrált adhat hozzá. Minden leírás hello rendszer karbantartása, és hello Azure Data Catalog-portál összes leírások jelennek meg.

Ebben a mintában alkalmazott toomost hello elemek hello hálózatiobjektum-modellt, úgy, hogy objektumtípusok a JSON-adattartalmat hello gyakran azokhoz a tulajdonságokhoz tömbök előfordulhat, hogy várt egypéldányos.

Például a hello eszköz gyökere leírás objektumokból álló tömb. hello tömb tulajdonság neve "leírások". A leírás objektumnak egy tulajdonság - leírás. hello a mintája, hogy minden felhasználónak, aki típusok leírás lekérdezi a leírás objektum létre hello felhasználó által megadott hello érték.

hello UX választhat, hogyan toodisplay hello kombinációja. Nincsenek megjelenítendő három különböző kombinációját.

* hello legegyszerűbb a mintája, "Minden látszik". Ebben a mintában lista nézetben minden hello objektumok jelennek meg. hello Azure Data Catalog-portál UX leírás ezt a mintát használ.
* Egy másik a mintája, "Merge". Ebben a mintában minden hello értéket a különböző felhasználók hello egyesítve lesznek együtt, az ismétlődő eltávolítva. Ebben a mintában szereplő hello Azure Data Catalog-portál UX példák hello címkék és szakértők tulajdonságait.
* A harmadik minta "utolsó író wins". Ebben a mintában csak hello legutóbbi értéke adta-e a megjelenik-e. rövid név ebben a mintában példája.

## <a name="asset-object-model"></a>Eszköz hálózatiobjektum-modellje
Hello alapfogalmak szakaszban jelent, hello **Azure Data Catalog** hálózatiobjektum-modellt tartalmaz elemeket, amely eszközök vagy széljegyzeteket. Elemek tulajdonság tartozik, amely választható vagy szükséges lehet. Néhány tulajdonság alkalmazza tooall elemeket. Néhány tulajdonság tooall eszközökre vonatkoznak. Néhány tulajdonság csak a toospecific eszköz típusok érvényesek.

### <a name="system-properties"></a>Rendszertulajdonságok
<table><tr><td><b>Tulajdonság neve</b></td><td><b>Adattípus</b></td><td><b>Megjegyzések</b></td></tr><tr><td>időbélyeg</td><td>Dátum és idő</td><td>hello utolsó idő hello eleme módosítva lett. Ez a mező hello kiszolgáló által létrehozott, ha egy elem szerepel, és minden alkalommal, amikor frissül egy elemet. hello ezt a tulajdonságot nem veszi figyelembe a bemeneti műveletek közzététele.</td></tr><tr><td>id</td><td>URI</td><td>Abszolút URL-címe hello elem (csak olvasható). Hello elem egyedi megcímezhető URI hello.  hello ezt a tulajdonságot nem veszi figyelembe a bemeneti műveletek közzététele.</td></tr><tr><td>type</td><td>Karakterlánc</td><td>hello típusa hello eszköz (csak olvasható).</td></tr><tr><td>ETag</td><td>Karakterlánc</td><td>Egy karakterlánc megfelelő toohello hello elem verziója használható az egyidejű hozzáférések optimista vezérlését hello katalógus elemeinek frissítő műveletek végrehajtása során. "*" használt toomatch bármekkora érték lehet.</td></tr></table>

### <a name="common-properties"></a>Általános tulajdonságai
Ezek a Tulajdonságok tooall legfelső szintű eszköz és az összes jegyzetet típusainak alkalmazni.

<table>
<tr><td><b>Tulajdonság neve</b></td><td><b>Adattípus</b></td><td><b>Megjegyzések</b></td></tr>
<tr><td>fromSourceSystem</td><td>Logikai érték</td><td>Azt jelzi, hogy elem adatait (például az Sql Server-adatbázis, Oracle-adatbázishoz) a forrásrendszerben származó vagy felhasználó által létrehozott.</td></tr>
</table>

### <a name="common-root-properties"></a>A gyökérszintű általános tulajdonságai
<p>
Ezek a Tulajdonságok tooall legfelső szintű eszköz típusok érvényesek.

<table><tr><td><b>Tulajdonság neve</b></td><td><b>Adattípus</b></td><td><b>Megjegyzések</b></td></tr><tr><td>név</td><td>Karakterlánc</td><td>Az hello adatforrás hely származtatott nevét</td></tr><tr><td>DSL</td><td>DataSourceLocation</td><td>Egyedi hello adatforrás ismerteti, és egyik hello azonosítók hello eszköz. (Lásd a kettős identitási szakaszban).  hello dsl hello szerkezete függ a hello protokoll és az adatforrás típusa.</td></tr><tr><td>Adatforrás</td><td>Érhető el</td><td>További részletek hello típusú eszköz.</td></tr><tr><td>lastRegisteredBy</td><td>SecurityPrincipal</td><td>Ez az eszköz legutóbb regisztrált hello felhasználó ismerteti.  Egyedi azonosító hello hello felhasználói (hello upn) és egy megjelenítési nevet (Keresztnév és Vezetéknév) tartalmaz.</td></tr><tr><td>Tárolóazonosító</td><td>Karakterlánc</td><td>Hello tároló eszköz hello adatforrás azonosítója. Ez a tulajdonság nem támogatott hello tárolótípus.</td></tr></table>

### <a name="common-non-singleton-annotation-properties"></a>Közös nem egypéldányos jegyzet tulajdonságai
Ezek a Tulajdonságok alkalmazása tooall nem egypéldányos jegyzet típusok (jegyzeteket, ami toobe engedélyezett eszközönként több).

<table>
<tr><td><b>Tulajdonság neve</b></td><td><b>Adattípus</b></td><td><b>Megjegyzések</b></td></tr>
<tr><td>kulcs</td><td>Karakterlánc</td><td>A felhasználó által megadott kulcs, amely egyedileg azonosítja az aktuális gyűjtemény hello hello jegyzet. hello kulcs hossza nem lehet hosszabb 256 karakternél.</td></tr>
</table>

### <a name="root-asset-types"></a>Legfelső szintű eszköz típusa
Legfelső szintű eszköz típusok a következők e képviselő típusok hello adategységeiről, amelyeket regisztrálni lehet hello katalógus különböző típusú. Minden legfelső szintű típushoz nincs nézet, amely ismerteti az eszköz és a jegyzetek hello nézetben. Nézet neve használandó hello megfelelő {view_name} URL-szegmenst a REST API-n keresztül közzétételekor.

<table><tr><td><b>Eszköz típusa (nézet neve)</b></td><td><b>További tulajdonságok</b></td><td><b>Adattípus</b></td><td><b>Engedélyezett jegyzetek</b></td><td><b>Megjegyzések</b></td></tr><tr><td>A tábla ("tábla")</td><td></td><td></td><td>Leírás<p>Rövid név<p>Címke<p>Séma<p>ColumnDescription<p>ColumnTag<p> szakértői<p>Előzetes verzió<p>AccessInstruction<p>TableDataProfile<p>ColumnDataProfile<p>ColumnDataClassification<p>Dokumentáció<p></td><td>Egy tábla táblázatos adatokat jelöli.  Például: SQL-tábla, az SQL-nézet, Analysis Services táblázatos tábla, Analysis Services többdimenziós dimenzió, Oracle-tábla stb.   </td></tr><tr><td>A mérték ("intézkedések")</td><td></td><td></td><td>Leírás<p>Rövid név<p>Címke<p>szakértői<p>AccessInstruction<p>Dokumentáció<p></td><td>Ez a típus jelképezi egy Analysis Services-mértéket.</td></tr><tr><td></td><td>mértékcsoport</td><td>Oszlop</td><td></td><td>Metaadatok leíró hello mérték</td></tr><tr><td></td><td>isCalculated </td><td>Logikai érték</td><td></td><td>Itt adhatja meg, ha hello mérték kiszámítása vagy sem.</td></tr><tr><td></td><td>MeasureGroup</td><td>Karakterlánc</td><td></td><td>A mérték fizikai tároló</td></tr><td>KPI-t ("KPI-k")</td><td></td><td></td><td>Leírás<p>Rövid név<p>Címke<p>szakértői<p>AccessInstruction<p>Dokumentáció</td><td></td></tr><tr><td></td><td>MeasureGroup</td><td>Karakterlánc</td><td></td><td>A mérték fizikai tároló</td></tr><tr><td></td><td>goalExpression</td><td>Karakterlánc</td><td></td><td>Numerikus MDX-kifejezés vagy a számítás, amely hello KPI hello cél értékét adja vissza.</td></tr><tr><td></td><td>valueExpression</td><td>Karakterlánc</td><td></td><td>MDX numerikus kifejezés hello KPI hello tényleges értékét adja vissza.</td></tr><tr><td></td><td>statusExpression</td><td>Karakterlánc</td><td></td><td>MDX-kifejezés, amely egy megadott időpontban KPI hello hello állapotát jeleníti meg időben.</td></tr><tr><td></td><td>trendExpression</td><td>Karakterlánc</td><td></td><td>MDX-kifejezés, amely kiértékeli a hello KPI hello értékének adott idő alatt. hello trend minden időalapú nézeteket, amelyek egy adott üzleti környezetben hasznos lehet.</td>
<tr><td>A jelentés ("jelentés")</td><td></td><td></td><td>Leírás<p>Rövid név<p>Címke<p>szakértői<p>AccessInstruction<p>Dokumentáció<p></td><td>Ez a típus jelképezi egy SQL Server Reporting Services jelentés </td></tr><tr><td></td><td>assetCreatedDate</td><td>Karakterlánc</td><td></td><td></td></tr><tr><td></td><td>assetCreatedBy</td><td>Karakterlánc</td><td></td><td></td></tr><tr><td></td><td>assetModifiedDate</td><td>Karakterlánc</td><td></td><td></td></tr><tr><td></td><td>assetModifiedBy</td><td>Karakterlánc</td><td></td><td></td></tr><tr><td>A tároló ("tárolók")</td><td></td><td></td><td>Leírás<p>Rövid név<p>Címke<p>szakértői<p>AccessInstruction<p>Dokumentáció<p></td><td>Ez a típus jelképezi más eszközök, például az SQL-adatbázis, az Azure BLOB-tároló vagy egy Analysis Services-modell tárolója.</td></tr></table>

### <a name="annotation-types"></a>Jegyzet típusok
Jegyzet típusok határoz meg, amely hozzárendelhető tooother típushoz hello katalógus metaadat típusú.

<table>
<tr><td><b>Jegyzet típusát (beágyazott nézet neve)</b></td><td><b>További tulajdonságok</b></td><td><b>Adattípus</b></td><td><b>Megjegyzések</b></td></tr>

<tr><td>Leírás ("leírások")</td><td></td><td></td><td>Ez a tulajdonság az adott eszköz számára leírását tartalmazza. Minden felhasználó hello rendszer adhat hozzá a saját leírása.  Csak a felhasználó szerkesztheti-hello leírás objektum.  (A rendszergazdák és az eszköz tulajdonosait hello leírás objektum törlését, azonban nem szerkeszthető). hello rendszer külön-külön fenntartja a felhasználói leírása.  Így nincs minden eszköz (egy minden felhasználó számára a hello eszköz, továbbá toopossibly hello adatforrás származó adatokat tartalmazó ismerete hozzájárult) leírások tömbjét.</td></tr>
<tr><td></td><td>leírás</td><td>Karakterlánc</td><td>Egy rövid leírást (sorok 2-3) hello eszköz</td></tr>

<tr><td>Címke ("címkék")</td><td></td><td></td><td>Ez a tulajdonság határozza meg egy címkét az adott eszköz számára. Minden felhasználó hello rendszer adhat hozzá az adott eszköz számára több címke.  Csak hello felhasználó, aki létrehozta a címke objektumok szerkesztheti azokat.  (A rendszergazdák és az eszköz tulajdonosait hello címke objektum törlését, azonban nem szerkeszthető). hello rendszer külön kezeli a felhasználói címkék.  Így nincs minden eszköz a kód objektumokból álló tömb.</td></tr>
<tr><td></td><td>Címke</td><td>Karakterlánc</td><td>A címke leíró hello eszköz.</td></tr>

<tr><td>Rövid név ("friendlyName")</td><td></td><td></td><td>Ez a tulajdonság egy eszköz rövid nevét tartalmazza. Rövid név egy egypéldányos megjegyzés – csak egy rövid név tooan eszköz lehet hozzáadni.  Csak a rövid név objektumot létrehozó hello felhasználó szerkeszthető. (A rendszergazdák és az eszköz tulajdonosait hello FriendlyName objektum törlését, azonban nem szerkeszthető). hello rendszer külön-külön kezeli a felhasználó rövid neve.</td></tr>
<tr><td></td><td>Rövid név</td><td>Karakterlánc</td><td>Hello eszköz rövid neve.</td></tr>

<tr><td>A séma ("schema")</td><td></td><td></td><td>hello séma hello adatok szerkezete hello ismerteti.  Hello attribútum (oszlop, attribútum, mező, stb.) neve, meg kell adnia, valamint egyéb metaadatokat.  Ez az információ hello adatforrásból származik.  Séma egy egypéldányos megjegyzés – csak egy séma felveheti az adott eszköz számára.</td></tr>
<tr><td></td><td>oszlopok</td><td>[Oszlop]</td><td>Egy oszlop objektumokból álló tömb. Hello oszlop leírják hello adatforrásból származó információkkal.</td></tr>

<tr><td>ColumnDescription ("columnDescriptions")</td><td></td><td></td><td>Ez a tulajdonság egy oszlop leírását tartalmazza.  Minden felhasználó hello rendszer adhat hozzá több olyan oszlop, (legfeljebb egyetlen oszloponként) leírása. Csak hello felhasználó, aki létrehozta ColumnDescription objektumok szerkesztheti azokat.  (A rendszergazdák és az eszköz tulajdonosait hello ColumnDescription objektum törlését, azonban nem szerkeszthető). hello rendszer külön kezeli a felhasználó az oszlopok leírása.  Így nem minden eszköz ColumnDescription objektumokból álló tömb (minden felhasználóhoz, aki a hello oszlop ismerete hozzájárult oszloponként egy továbbá egy adatokat tartalmazó toopossibly származó hello adatforrás).  hello ColumnDescription lazán kötött toohello séma, így a szinkronizált kérheti le. egy oszlop, amely már nem létezik a hello séma hello ColumnDescription ír le.  Toohello író tookeep leírás és séma szinkronban van.  hello adatforrás is oszlopok leírása információk és további ColumnDescription objektumok létrehozott hello eszköz futtatásakor.</td></tr>
<tr><td></td><td>Oszlopnév</td><td>Karakterlánc</td><td>Ez a megnevezés hivatkozik hello oszlop hello nevét.</td></tr>
<tr><td></td><td>leírás</td><td>Karakterlánc</td><td>egy rövid leírása (2-3 sorok) hello oszlop.</td></tr>

<tr><td>ColumnTag ("columnTags")</td><td></td><td></td><td>Ez a tulajdonság egy oszlop címkét tartalmaz. Minden felhasználó hello rendszer az adott oszlop több címke is hozzáadható, és több oszlop címkéket adhat hozzá. Csak hello felhasználó, aki létrehozta ColumnTag objektumok szerkesztheti azokat. (A rendszergazdák és az eszköz tulajdonosait hello ColumnTag objektum törlését, azonban nem szerkeszthető). hello rendszer külön-külön tart fenn a felhasználók oszlop címkék.  Így nincs minden eszköz ColumnTag objektumokból álló tömb.  hello ColumnTag lazán kötött toohello séma, így a szinkronizált kérheti le. egy oszlop, amely már nem létezik a hello séma hello ColumnTag ír le.  Toohello író tookeep oszlop címke és séma szinkronban van.</td></tr>
<tr><td></td><td>Oszlopnév</td><td>Karakterlánc</td><td>a címke hivatkozik hello oszlop hello nevét.</td></tr>
<tr><td></td><td>Címke</td><td>Karakterlánc</td><td>Egy címke leíró hello oszlop.</td></tr>

<tr><td>A szakértő ("szakértők")</td><td></td><td></td><td>Ez a tulajdonság egy felhasználót, aki a hello adatkészletben szakértő tekinthető tartalmazza. hello szakértők opinions(descriptions) buborék toohello felső részén hello UX leírások listázása során. Minden felhasználó megadhatja a saját szakértői. Csak a felhasználó szerkesztheti-hello szakértők objektum. (A rendszergazdák és az eszköz tulajdonosait hello szakértői objektumok törlése, de nem szerkeszthető).</td></tr>
<tr><td></td><td>szakértői</td><td>SecurityPrincipal</td><td></td></tr>

<tr><td>Előzetes verzió ("előzetes verziójú funkciók")</td><td></td><td></td><td>hello preview hello első 20 sorok hello eszköz adatok pillanatképe tartalmazza. Előzetes verzió csak a célszerű bizonyos típusú eszközöket (érdemes táblához, de a mérték nem).</td></tr>
<tr><td></td><td>Előzetes verzió</td><td>[objektum]</td><td>Egy oszlop képviselő objektumok tömbje.  Minden objektum az adott oszlop hello sorára értékkel tooa oszlop leképezése tulajdonsággal rendelkezik.</td></tr>

<tr><td>AccessInstruction ("accessInstructions")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>mimeType</td><td>Karakterlánc</td><td>hello tartalom hello MIME-típusát.</td></tr>
<tr><td></td><td>Tartalom</td><td>Karakterlánc</td><td>Hogyan érik el tooget toothis adategységet hello utasításokat. hello tartalom URL-címet, az e-mail címet vagy utasítások lehet.</td></tr>

<tr><td>TableDataProfile ("tableDataProfiles")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>numberOfRows</td></td><td>int</td><td>hello hello adatkészlet sorainak száma</td></tr>
<tr><td></td><td>Méret</td><td>hosszú</td><td>hello mérete bájtban hello adatkészlet.  </td></tr>
<tr><td></td><td>schemaModifiedTime</td><td>Karakterlánc</td><td>hello utolsó idő hello sémája módosult.</td></tr>
<tr><td></td><td>dataModifiedTime</td><td>Karakterlánc</td><td>hello utolsó idő hello adatkészlet módosítva lett (adatok lett hozzáadva, módosított vagy törlése)</td></tr>

<tr><td>ColumnsDataProfile ("columnsDataProfiles")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>oszlopok</td></td><td>ColumnDataProfile]</td><td>Egy oszlop adatainak profilok tömbjét.</td></tr>

<tr><td>ColumnDataClassification ("columnDataClassifications")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>Oszlopnév</td><td>Karakterlánc</td><td>Ez a besorolás hivatkozik hello oszlop hello nevét.</td></tr>
<tr><td></td><td>Besorolás</td><td>Karakterlánc</td><td>Ebben az oszlopban hello adatok hello besorolása.</td></tr>

<tr><td>A dokumentáció ("dokumentáció")</td><td></td><td></td><td>Egy adott eszköz lehet társítva csak egy dokumentációját.</td></tr>
<tr><td></td><td>mimeType</td><td>Karakterlánc</td><td>hello tartalom hello MIME-típusát.</td></tr>
<tr><td></td><td>Tartalom</td><td>Karakterlánc</td><td>hello dokumentáció tartalmat.</td></tr>

</table>

### <a name="common-types"></a>Általános típusok
Általános tulajdonságok hello típusokkal használható, de nincsenek elemek.

<table>
<tr><td><b>Közös típusa</b></td><td><b>Tulajdonságok</b></td><td><b>Adattípus</b></td><td><b>Megjegyzések</b></td></tr>
<tr><td>Érhető el</td><td></td><td></td><td></td></tr>
<tr><td></td><td>Forrástípus</td><td>Karakterlánc</td><td>Adatforrás típusa hello ismerteti.  Például: SQL Server, az Oracle-adatbázishoz, stb.  </td></tr>
<tr><td></td><td>Objektumtípus</td><td>Karakterlánc</td><td>Hello típusú objektum hello adatforrás ismerteti. Például: Table, és tekintse meg az SQL Server.</td></tr>

<tr><td>DataSourceLocation</td><td></td><td></td><td></td></tr>
<tr><td></td><td>Protokoll</td><td>Karakterlánc</td><td>Kötelező. A használt protokoll toocommunicate hello adatforrással ismerteti. Például: "tds" SQl-kiszolgáló, "oracle" Oracle, stb. Tekintse meg a túl[adatforrás-hivatkozás megadását - DSL struktúra](data-catalog-dsr.md) hello jelenleg támogatott protokollok listáját.</td></tr>
<tr><td></td><td>Cím</td><td>Könyvtár<string, object></td><td>Kötelező. Cím olyan adatokat adott toohello protokoll, amely használt tooidentify hello hivatkozott adatforrás. hello címadatokat hatókörű tooa adott protokoll, ami azt jelenti, értelmetlen hello protokoll ismerete nélkül.</td></tr>
<tr><td></td><td>Hitelesítés</td><td>Karakterlánc</td><td>Választható. hello hitelesítési séma használható toocommunicate hello adatforrással. Például: windows, az oauth, stb.</td></tr>
<tr><td></td><td>connectionProperties</td><td>Könyvtár<string, object></td><td>Választható. További információt tooconnect tooa adatforrás.</td></tr>

<tr><td>SecurityPrincipal</td><td></td><td></td><td>hello háttér adatellenőrzése nem bármely elleni aad-ben megadott tulajdonságok közzététele során.</td></tr>
<tr><td></td><td>egyszerű felhasználónév</td><td>Karakterlánc</td><td>Felhasználó egyedi e-mail címét. Meg kell adni, ha nem áll rendelkezésre objectId vagy hello "lastRegisteredBy" tulajdonság, ellenkező esetben nem kötelező.</td></tr>
<tr><td></td><td>Objektumazonosító</td><td>GUID</td><td>Felhasználó vagy biztonsági csoport AAD-identitása. Választható. Kötelező megadni, ha egyszerű felhasználónév nincs megadva, ellenkező esetben nem kötelező.</td></tr>
<tr><td></td><td>Utónév</td><td>Karakterlánc</td><td>Utónév felhasználó (megjelenítési célokra). Választható. Csak érvényes hello környezetben "lastRegisteredBy" tulajdonság. Nem lehet megadni, ha a rendszerbiztonsági tag megadása "szerepkörök", "engedélyek" és "szakértők".</td></tr>
<tr><td></td><td>Vezetéknév</td><td>Karakterlánc</td><td>Utolsó felhasználó neve (megjelenítési célokra). Választható. Csak érvényes hello környezetben "lastRegisteredBy" tulajdonság. Nem lehet megadni, ha a rendszerbiztonsági tag megadása "szerepkörök", "engedélyek" és "szakértők".</td></tr>

<tr><td>Oszlop</td><td></td><td></td><td></td></tr>
<tr><td></td><td>név</td><td>Karakterlánc</td><td>Hello oszlop vagy attribútum neve.</td></tr>
<tr><td></td><td>type</td><td>Karakterlánc</td><td>hello oszlop vagy az attribútum adattípusa. hello engedélyezett típusok adatok forrástípus hello eszköz függ.  A támogatott típusok csak egy részét.</td></tr>
<tr><td></td><td>MaxLength</td><td>int</td><td>hello engedélyezett maximális hosszt hello oszlop vagy attribútum. Az adatforrás származik. Csak a megfelelő toosome forrás típusok.</td></tr>
<tr><td></td><td>Pontosság</td><td>Bájt</td><td>hello pontosság hello oszlop vagy attribútum. Az adatforrás származik. Csak a megfelelő toosome forrás típusok.</td></tr>
<tr><td></td><td>isNullable</td><td>Logikai érték</td><td>Hogy van-e hello oszlop engedélyezett a null értékű toohave vagy sem. Az adatforrás származik. Csak a megfelelő toosome forrás típusok.</td></tr>
<tr><td></td><td>kifejezés</td><td>Karakterlánc</td><td>Ha hello érték számított oszlop, ez a mező hello kifejezést tartalmaz, amely kifejezi hello érték. Az adatforrás származik. Csak a megfelelő toosome forrás típusok.</td></tr>

<tr><td>ColumnDataProfile</td><td></td><td></td><td></td></tr>
<tr><td></td><td>Oszlopnév </td><td>Karakterlánc</td><td>hello hello oszlop neve</td></tr>
<tr><td></td><td>type </td><td>Karakterlánc</td><td>hello oszlop hello típusa</td></tr>
<tr><td></td><td>perc </td><td>Karakterlánc</td><td>hello minimális értékének hello adatkészlet</td></tr>
<tr><td></td><td>maximális </td><td>Karakterlánc</td><td>hello maximális értékének hello adatkészlet</td></tr>
<tr><td></td><td>átlagos </td><td>Dupla</td><td>átlagos érték hello hello adatkészlet</td></tr>
<tr><td></td><td>szórás </td><td>Dupla</td><td>hello adatkészlet hello szórása</td></tr>
<tr><td></td><td>nullCount </td><td>int</td><td>hello adatkészlet null értékekkel hello száma</td></tr>
<tr><td></td><td>distinctCount  </td><td>int</td><td>különböző értékeket hello adathalmaz hello száma</td></tr>


</table>

## <a name="asset-identity"></a>Eszköz identitása
Az Azure Data Catalog "protokoll" használ, és azonosító tulajdonságainak a hello "cím" tulajdonságcsomag hello DataSourceLocation "dsl" tulajdonság toogenerate identitás hello eszköz, amely használt tooaddress hello eszköz hello katalógus belül.
Például hello "tds" protokoll tulajdonságai Identitás "kiszolgáló", "database", "schema" és "object". hello kombinációk hello és hello azonosító tulajdonságainak használt toogenerate hello identitásának hello SQL Server tábla eszköz.
Az Azure Data Catalog biztosít több beépített forrás protokollok, amelyek leírása megtalálható a [adatforrás-hivatkozás megadását - DSL struktúra](data-catalog-dsr.md).
hello támogatott protokollok készletét is terjeszthető programokon keresztül (lásd a tooData Catalog REST API-referencia). A katalógus hello rendszergazdái regisztrálhatja az egyéni forrás protokollok. hello következő táblázatban hello tulajdonságot, ami szükséges tooregister egy egyéni protokoll.

### <a name="custom-data-source-protocol-specification"></a>Egyéni adatok forrás protokoll specifikációja
<table>
<tr><td><b>Típus</b></td><td><b>Tulajdonságok</b></td><td><b>Adattípus</b></td><td><b>Megjegyzések</b></td></tr>

<tr><td>DataSourceProtocol</td><td></td><td></td><td></td></tr>
<tr><td></td><td>Namespace</td><td>Karakterlánc</td><td>hello névtér hello protokoll. Namespace kell származnia 1 too255 karakter hosszú, legalább egy nem üres részeit, pontot (.) elválasztva tartalmaz. Minden egyes részben az 1 too255 karakter hosszú lehet, betűvel kezdődhet és csak betűket és számokat tartalmazhat.</td></tr>
<tr><td></td><td>név</td><td>Karakterlánc</td><td>hello protokoll hello neve. Nevét kell a 1 too255 karakter hosszú lehet, betűvel kezdődhet, és csak betűket, számokat és kötőjelet (-) karakter hello.</td></tr>
<tr><td></td><td>identityProperties</td><td>DataSourceProtocolIdentityProperty]</td><td>Azonosító tulajdonságainak listája, tartalmaznia kell legalább egy, de nem több mint 20 tulajdonságait. Például: "kiszolgáló", "database", "schema", "object" hello "tds" protokoll identitása tulajdonságainak.</td></tr>
<tr><td></td><td>identitySets</td><td>DataSourceProtocolIdentitySet]</td><td>Beállítja a identitás listája. Meghatározza a határoz meg érvényes eszköznév identitás identitás tulajdonságainak beállítása. Tartalmaznia kell legalább egy, de nem több mint 20 beállítása. Például: {"kiszolgáló", "adatbázis", "schema" és "objektum"} "tds" protokoll, amely határozza meg az Sql Server tábla eszköz identitásának beállítása identitást.</td></tr>

<tr><td>DataSourceProtocolIdentityProperty</td><td></td><td></td><td></td></tr>
<tr><td></td><td>név</td><td>Karakterlánc</td><td>hello tulajdonság hello neve. Nevét kell származnia 1 too100 karakter hosszú, betűvel kezdődik, és csak betűket és számokat tartalmazhat.</td></tr>
<tr><td></td><td>type</td><td>Karakterlánc</td><td>hello hello tulajdonság típusát. Támogatott értékek: "logikai" boolean ","bájt","guid","int","integer","hosszú","string","url"</td></tr>
<tr><td></td><td>az ignoreCase</td><td>logikai érték</td><td>Azt jelzi, hogy esetben figyelmen kívül kell tulajdonság értékét használja. Csak adható meg a "string" típusú tulajdonságokhoz. Alapértelmezett értéke hamis.</td></tr>
<tr><td></td><td>urlPathSegmentsIgnoreCase</td><td>logikai]</td><td>Azt jelzi, hogy esetben figyelmen kívül hagyja egyes szegmenseinek hello URL-címe. Csak adható meg "url" típusú tulajdonságokhoz. Alapértelmezett érték: [false].</td></tr>

<tr><td>DataSourceProtocolIdentitySet</td><td></td><td></td><td></td></tr>
<tr><td></td><td>név</td><td>Karakterlánc</td><td>Állítsa be a hello identitás hello nevét.</td></tr>
<tr><td></td><td>properties</td><td>String]</td><td>Ezzel az identitással bekerült azonosító tulajdonságainak listája hello meg. Nem tartalmazhat ismétlődéseket. Minden egyes tulajdonsága identitás készlet által hivatkozott "identityProperties" hello protokoll hello listája definiálni kell.</td></tr>

</table>

## <a name="roles-and-authorization"></a>Szerepkörök és engedélyezés
Microsoft Azure Data Catalog eszközökre és a jegyzetek CRUD műveleteihez engedélyezési képességeket biztosít.

## <a name="key-concepts"></a>Fő fogalmak
Azure Data Catalog hello kétféle hitelesítési módszer használ:

* Szerepköralapú hitelesítés
* Engedély-alapú engedélyezési

### <a name="roles"></a>Szerepkörök
Három szerepkör: **rendszergazda**, **tulajdonos**, és **közreműködő**.  Minden szerepkörhöz, a hatókör és a jogok, amely hello a következő táblázat foglalja össze.

<table><tr><td><b>Szerepkör</b></td><td><b>Hatókör</b></td><td><b>Jogok</b></td></tr><tr><td>Rendszergazda</td><td>Katalógus (eszközök/szereplő összes megjegyzést hello katalógus)</td><td>Olvasási Delete ViewRoles

ChangeOwnership ChangeVisibility ViewPermissions</td></tr><tr><td>Tulajdonos</td><td>Minden eszköz (legfelső szintű elem)</td><td>Olvasási Delete ViewRoles

ChangeOwnership ChangeVisibility ViewPermissions</td></tr><tr><td>Közreműködő</td><td>Minden egyes eszköz és a jegyzet</td><td>Olvasási frissítés ViewRoles Megjegyzés törlése: minden hello jog vissza lenne vonva, ha hello olvassa el a jobbra hello elem közreműködői hello a visszavonva</td></tr></table>

> [!NOTE]
> **Olvasási**, **frissítés**, **törlése**, **ViewRoles** jogok a következők alkalmazható tooany elem (eszköz vagy jegyzet) során **TakeOwnership**, **ChangeOwnership**, **ChangeVisibility**, **ViewPermissions** csak a megfelelő toohello legfelső szintű eszköz vannak.
> 
> **Törlés** jobb tooan elem és alelemeket vagy alatta egyelemű vonatkozik. Például egy eszköz is törlésekor a jegyzeteket az adott eszköz számára.
> 
> 

### <a name="permissions"></a>Engedélyek
Engedély van, mint a hozzáférés-vezérlő bejegyzések listája. Minden egyes hozzáférés-vezérlési bejegyzés jogok tooa rendszerbiztonsági tagok készletét rendeli hozzá. Engedélyek csak adhatók meg egy eszköz (Ez azt jelenti, hogy a legfelső szintű elem), és alkalmazza a toohello eszköz és bármely alelemeket.

Hello során **Azure Data Catalog** csak előzetes **olvasási** jobb hello engedélyek lista tooenable forgatókönyv toorestrict látható-e az eszköz használata támogatott.

Alapértelmezés szerint minden hitelesített felhasználó rendelkezik **olvasási** hello katalógus valamely elemére a jobb gombbal, kivéve, ha látható a korlátozott toohello hello engedélyek rendszerbiztonsági tagok készletét.

## <a name="rest-api"></a>REST API
**PUT** és **POST** eleme kérelmek lehet használt toocontrol szerepkörökről és engedélyekről: tooitem adattartalom hozzáadása, a két rendszer tulajdonságok is megadhatók **szerepkörök** és  **engedélyek**.

> [!NOTE]
> **engedélyek** csak a megfelelő tooa legfelső szintű elemet.
> 
> **Tulajdonos** szerepkörhöz csak megfelelő tooa gyökérelemet.
> 
> Egy elem hello katalógusban létrehozásakor alapértelmezés szerint a **közreműködő** toohello jelenleg hitelesített felhasználó van beállítva. Ha kell, hogy a cikk frissíthető mindenki számára, **közreműködő** túl meg kell&lt;mindenki&gt; különleges rendszerbiztonsági tag a(z) hello **szerepkörök** tulajdonságot, ha az elem az első közzététel () Tekintse meg a következő példa toohello). **Közreműködő** nem módosítható és marad hello azonos elem élettartama során (még akkor is **rendszergazda** vagy **tulajdonos** nem rendelkezik a megfelelő toochange hello hello  **A közreműködői**). csak explicit beállításának hello hello támogatott érték hello **közreműködő** van &lt;mindenki&gt;: **közreműködő** csak egy elemet létrehozó felhasználó lehet vagy &lt; Mindenki&gt;.
> 
> 

### <a name="examples"></a>Példák
**Állítsa be a közreműködői túl&lt;mindenki&gt; cikk közzétételekor.**
Különleges rendszerbiztonsági tag &lt;mindenki&gt; objectId "00000000-0000-0000-0000-000000000201" rendelkezik.
  **POST** https://api.azuredatacatalog.com/catalogs/default/views/tables/?api-version=2016-03-30

> [!NOTE]
> Bizonyos ügyfélmegvalósítások esetén HTTP előfordulhat, hogy automatikusan újból kiadja azokat a kérelmeket a válasz tooa 302 hello kiszolgálóról, de általában sáv hello kérelemből engedélyezési fejléceket. Mivel hello engedélyezési fejléc szükséges toomake kérelmek tooAzure Data Catalog, gondoskodnia kell hello Authorization fejlécet Azure Data Catalog által meghatározott kérelem tooa átirányítási helyre hosszkorlátját továbbra is biztosított. hello következő mintakód bemutatja hello .NET HttpWebRequest objektum használatával.
> 
> 

**Törzs**

    {
        "roles": [
            {
                "role": "Contributor",
                "members": [
                    {
                        "objectId": "00000000-0000-0000-0000-000000000201"
                    }
                ]
            }
        ]
    }

  **Rendelje hozzá a tulajdonosok és korlátozható a láthatóságuk egy meglévő legfelső szintű elem**: **PUT** https://api.azuredatacatalog.com/catalogs/default/views/tables/042297b0...1be45ecd462a?api-version=2016-03-30

    {
        "roles": [
            {
                "role": "Owner",
                "members": [
                    {
                        "objectId": "c4159539-846a-45af-bdfb-58efd3772b43",
                        "upn": "user1@contoso.com"
                    },
                    {
                        "objectId": "fdabd95b-7c56-47d6-a6ba-a7c5f264533f",
                        "upn": "user2@contoso.com"
                    }
                ]
            }
        ],
        "permissions": [
            {
                "principal": {
                    "objectId": "27b9a0eb-bb71-4297-9f1f-c462dab7192a",
                    "upn": "user3@contoso.com"
                },
                "rights": [
                    {
                        "right": "Read"
                    }
                ]
            },
            {
                "principal": {
                    "objectId": "4c8bc8ce-225c-4fcf-b09a-047030baab31",
                    "upn": "user4@contoso.com"
                },
                "rights": [
                    {
                        "right": "Read"
                    }
                ]
            }
        ]
    }

> [!NOTE]
> A PUT nem kötelező toospecify hello törzsében elem payloadot: PUT használt tooupdate csak szerepkörök és/vagy lehet.
> 
> 

<!--Image references-->
[1]: ./media/data-catalog-developer-concepts/concept2.png
