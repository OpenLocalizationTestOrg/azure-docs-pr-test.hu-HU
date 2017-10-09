---
title: "Azure AD Connect szinkronizálása: hello architektúra ismertetése |} Microsoft Docs"
description: "Ez a témakör ismerteti az Azure AD Connect szinkronizálási szolgáltatás hello architektúrájának, és elmagyarázza, hello használt kifejezések."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 465bcbe9-3bdd-4769-a8ca-f8905abf426d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 9fb979fcf8feb7b4d406789102239480b0b4bc94
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-understanding-hello-architecture"></a>Azure AD Connect szinkronizálása: hello architektúra ismertetése
Ez a témakör ismerteti az Azure AD Connect szinkronizálási szolgáltatás hello alapvető architektúráját. Sok szempontból hogy a rendszer hasonló tooits megelőző MIIS 2003 ILM 2007 és a FIM 2010. Azure AD Connect szinkronizálása, ezek a technológiák hello fejlődéséhez. Ha ismeri az összes ilyen korábbi technológiát, ez a témakör tartalma hello lesz, valamint a megszokott tooyou. Ha új toosynchronization, akkor ez a témakör értéke meg. Van azonban nem ez a témakör toobe sikeres testreszabási tooAzure AD Connect szinkronizálása (ebben a témakörben a hívott szinkronizálási motor) egy követelmény tooknow hello részleteit.

## <a name="architecture"></a>Architektúra
hello szinkronizálási motor egy integrált nézetben több csatlakoztatott adatforrások tárolt objektumok létrehozása, és azonosító adatokat az adatforrások kezelése. Ebben a nézetben integrált határozza meg hello azonosító adatok veszi át a csatlakoztatott adatforrások és a szabályok, amelyek meghatározzák, hogyan tooprocess ezt az információt.

### <a name="connected-data-sources-and-connectors"></a>Csatlakoztatott adatforrások és összekötők
hello szinkronizálási motor dolgozza fel a azonosító adatokat a különböző adattárolók, például az Active Directory vagy egy SQL Server-adatbázist. Minden adat tárház, amely rendszerezi az adatbázis-szerű formátumú adatokat, illetve szabványos adat-hozzáférési metódusokat biztosít, amely egy potenciális adatok forrás jelölt hello szinkronizálási motor. szinkronizálási motor által szinkronizált hello adattárolók nevezzük **csatlakoztatott adatforrások** vagy **csatlakoztatott könyvtárak** (CD).

hello szinkronizálási motor hívása modulon belül csatlakoztatott adatforráshoz való együttműködéshez magában foglalja a **összekötő**. Minden csatlakoztatott adatforrás rendelkezik egy adott összekötőt. hello összekötő fordítja le a szükséges műveletet csatlakoztatott adatok hello hello formátumba forrás együttműködik.

Összekötők hívások API tooexchange azonosító adatokat (olvasási és írási) olyan csatlakoztatott adatforráshoz. Az is lehetséges tooadd egyéni összekötő hello bővíthető kapcsolat keretrendszer használatával. hello alábbi ábrán hogyan összekötő csatlakozzon egy csatlakoztatott adatok forrás toohello szinkronizálási motor.

![Ossz1](./media/active-directory-aadconnectsync-understanding-architecture/arch1.png)

Mindkét irányban áramolhasson az adatokat, de azt nem flow mindkét irányban egyidejűleg. Más szóval összekötő konfigurált tooallow adatok tooflow hello csatlakoztatott adatok forrásprogram toosync vagy a szinkronizálási motor toohello csatlakoztatott adatforrás lehet, de ezek a műveletek közül csak akkor fordulhat elő, és egy attribútum egyszerre. hello lehet különböző, a különböző objektumok esetében, és különböző attribútumokhoz.

Összekötő tooconfigure, megadhatja objektumtípus hello megjeleníteni kívánt toosynchronize. Hello szinkronizálási folyamat szereplő beilleszthető objektumok hatóköre hello hello objektumtípusok megadásával határozza meg. hello következő lépés egy attribútum listában ismert tooselect hello attribútumok toosynchronize. Ezek a beállítások a válasz toochanges tooyour üzleti szabályok bármikor módosíthatja. Az Azure AD Connect telepítővarázsló hello használatakor ezeket a beállításokat meg vannak konfigurálva.

tooexport objektumok tooa csatlakoztatott adatforráshoz, hello attribútum listában tartalmaznia kell legalább minimális hello attribútumok egy adott objektumához írja be a csatlakoztatott adatforrás szükséges toocreate. Például hello **sAMAccountName** attribútumot kell foglalni a hello attribútum befoglalási lista tooexport a felhasználó tooActive Directory objektumot, mert rendelkeznie kell az összes felhasználói objektumok az Active Directoryban egy **sAMAccountName**  attribútummal. Ebben az esetben hello telepítési varázsló hajtja végre ezt a konfigurációt.

Ha csatlakoztatott adatforrás hello strukturális összetevőket használ, partíciók vagy a tárolókat tooorganize objektumok, például korlátozhatja a hello területek hello csatlakoztatott adatforráshoz, amelyeket egy adott megoldás.

### <a name="internal-structure-of-hello-sync-engine-namespace"></a>Hello szinkronizálási motor névtér belső szerkezete
hello teljes szinkronizálási motor névtér áll két névtér hello azonosító adatok tárolására. hello két van:

* hello kapcsolódási térbe (CS)
* hello metaverse (MV)

Hello **kapcsolódási térbe** van egy átmeneti területre, amely tartalmazza a kijelölt objektumokat egy csatlakoztatott adatok forrás- és hello attribútumok hello attribútum listában megadott hello ábrázolásai. hello szinkronizálási motor hello összekötő terület toodetermine csatlakoztatott hello forrás- és toostage bejövő adatváltozásokat változásai használja. hello szinkronizálási motor hello összekötő terület toostage kimenő módosítások exportálás toohello csatlakoztatott adatforrás is használ. hello szinkronizálási motor egy különálló kapcsolódási térbe egy átmeneti területre, az egyes összekötő kezeli.

Egy átmeneti területre használatával hello szinkronizálási motor független hello csatlakoztatott adatforrások marad, és nem érinti a rendelkezésre állás és a kisegítő lehetőségek. Ennek eredményeképpen azonosító adatok bármikor feldolgozhatja hello átmeneti területen hello adatok használatával. hello szinkronizálási motor kérheti csak hello módosítások belül hello csatlakoztatott adatforrás hello legutóbbi kommunikációs munkamenet megszakadt, vagy csak hello módosítások tooidentity adatok, amelyek csatlakoztatott adatforrás hello leküldéses még nem kapta meg, amely csökkenti a óta hello hálózati forgalom hello szinkronizálási motor és a csatlakoztatott adatforrás hello között.

Emellett a szinkronizálási motor minden objektumot, hogy a kapcsolódási térbe hello előkészítette állapot adatait tárolja. Amikor új adatok érkezik, szinkronizálási motor mindig kiértékeli e hello adatok már be lett szinkronizálva.

Hello **metaverse** egyik tárolóhelye hello összevont identitás vonatkozó információk több csatlakoztatott adatforrások, így az összes kombinált objektum egyetlen globális, integrált nézetben. Metaverzum-objektum hello csatlakoztatott adatforrások és a szabályok, amelyek lehetővé teszik toocustomize hello szinkronizálási folyamat lekért hello identitásinformációi alapján hozzák létre.

hello alábbi ábrán hello összekötő terület névtér és hello metaverse névterének hello szinkronizálási motor.

![Arch2](./media/active-directory-aadconnectsync-understanding-architecture/arch2.png)

## <a name="sync-engine-identity-objects"></a>Szinkronizálási motor identitásobjektumok
a szinkronizálási motor hello hello objektumok vagy objektumképviseleteket hello csatlakoztatott adatforrás vagy hello integrált nézet, amelyek szinkronizálják a motor rendelkezik az adott objektumok. Minden szinkronizálási motor objektum egy globálisan egyedi azonosítóját (GUID) kell rendelkeznie. GUID azonosítók biztosít adatintegritást és -objektumok között kifejezett kapcsolatok.

### <a name="connector-space-objects"></a>Összekötő terület objektumok
Amikor a szinkronizálási motor kommunikál a csatlakoztatott adatforráshoz, hello csatlakoztatott adatforrás hello azonosító adatokat olvas, és használja, hogy információkat toocreate hello identitás objektum ábrázolása hello kapcsolódási térbe. Nem hozható létre, vagy külön-külön törölje ezeket az objektumokat. Azonban manuálisan törölheti a kapcsolódási térbe található összes objektumot.

Hello kapcsolódási térbe található összes objektumnak két attribútumokkal rendelkezik:

* A globálisan egyedi azonosítóját (GUID)
* A megkülönböztető név (DN)

Ha hello csatlakoztatott adatforrás hozzárendel egy egyedi attribútum toohello objektumot, majd hello kapcsolódási térbe objektumokat is lehet horgonyattribútum. hello horgonyattribútum egyedileg azonosít egy objektumot, hello csatlakoztatott adatforrás. hello szinkronizálási motor hello csatlakoztatott adatforrás hello horgonyzási toolocate hello megfelelő ábrázolását ezt az objektumot használja. Szinkronizálási motor azt feltételezi, hogy az objektumok adott hello horgonyzási soha nem módosítások hello objektum hello élettartamuk során.

Hello összekötők számos egy ismert egyedi azonosító toogenerate horgonya automatikusan használni az egyes objektumok Amikor importálja. Például az Active Directory-összekötő hello használ hello **objectGUID** horgonya attribútuma. Csatlakoztatott adatforrások, amelyek nem adnak meg egyértelműen meghatározott egyedi azonosítója horgonyzási generációs hello összekötő-konfiguráció részeként is megadhat.

Abban az esetben hello horgonyzási épül egy vagy több egyedi attribútum egy objektum írja be, sem mely változtatásokat, és hogy az egyedi hello kapcsolódási térbe (például az alkalmazott számát vagy a felhasználói azonosító) hello objektumot azonosítja.

Egy összekötő terület objektum hello következő egyike lehet:

* Egy átmeneti objektum
* A helyőrző

### <a name="staging-objects"></a>Átmeneti objektumok
Egy átmeneti objektum kijelölt objektumtípusok hello csatlakoztatott adatforrásból hello egyik példányát képviseli. Ezenkívül toohello GUID és hello megkülönböztető nevét, egy átmeneti mindig objektumnak hello objektumtípus jelző értéket.

Átmeneti mindig importált objektumoknak hello horgonyattribútum értékét. Szinkronizálási motor által újonnan kiépített, hello folyamata hello csatlakoztatott adatforrás létrehozása folyamatban lévő átmeneti objektumok nem rendelkeznek hello horgonyattribútum értéket.

Átmeneti objektumok továbbítani az aktuális értékek üzleti, és a szinkronizálási motor tooperform hello szinkronizálási folyamat számára szükséges működési adatokat is. Működéssel kapcsolatos adatokat tartalmaz, hogy hello típusú vannak készítve objektum átmeneti hello a frissítéseket. Ha egy átmeneti objektum, amely még nem dolgozott hello csatlakoztatott adatforrásból kapott új identitásadatok, hello objektum megjelölt **függőben lévő importálás**. Ha egy átmeneti objektum még nincs exportált toohello csatlakoztatott adatforrás új identitás információit, mint a megjelölt **exportálási függőben lévő**.

Az importálási vagy exportálási objektum egy átmeneti objektum lehet. hello szinkronizálási motor importálási objektum hello csatlakoztatott adatforrásból kapott objektuminformáció használatával hozza létre. Hello fenntartása információt egy új objektum, amely megfelel a kiválasztott hello összekötő hello objektumtípusok egyike fogadásakor a szinkronizálási motor hozna létre az importálás objektum hello kapcsolódási térbe, a csatlakoztatott adatforrás hello hello objektum megjelenítése.

a következő ábra hello hello csatlakoztatott adatforrás objektum jelölő importálási objektum jeleníti meg.

![Arch3](./media/active-directory-aadconnectsync-understanding-architecture/arch3.png)

hello szinkronizálási motor exportálási objektum hello metaverse objektuminformáció használatával hoz létre. Objektumok exportálása exportált toohello csatlakoztatott adatforrás hello következő kommunikációs munkamenet alatt is. Hello szinkronizálási motor hello szempontjából objektumok exportálása nem hello csatlakoztatott adatforrás még létezik. Ezért az Exportálás objektum hello horgonyattribútum nem áll rendelkezésre. Miután hello objektum megkapja szinkronizálási motor, hello csatlakoztatott adatforrás hello horgonyattribútum hello objektum egyedi értéket hoz létre.

hello alábbi ábrán hogyan exportálási objektum hello metaverse identitásinformációi segítségével hozták létre.

![Arch4](./media/active-directory-aadconnectsync-understanding-architecture/arch4.png)

hello szinkronizálási motor megerősíti, hogy a hello exportálási hello objektum által importintézkedésekkel hello objektum hello csatlakoztatott adatforrásból. Objektumok exportálása objektum importálása válnak, amikor a szinkronizálási motor megkapja azokat az csatlakoztatott adatforrásból hello a következő importálása során.

### <a name="placeholders"></a>Helyőrzők
hello szinkronizálási motor egy egyszerű névtér toostore-objektumokat használ. Azonban néhány csatlakoztatott adatforrások, például az Active Directory használata a hierarchikus névtér. tootransform információ a hierarchikus névtér strukturálatlan névtérbe szinkronizálási motor helyőrzők toopreserve hello hierarchia használja.

Minden egyes helyőrző hierarchikus neve az objektum, amely a szinkronizálási motor nem lett importálva, de szükséges tooconstruct hello hierarchikus neve (például egy szervezeti egységhez) összetevője. Kitöltés hozta létre, amelyek nem átmeneti hello kapcsolódási térbe objektumok csatlakoztatott hello-forrás tooobjects elemben található hivatkozások hézagok meghatározása érdekében.

hello szinkronizálási motor is használja a helyőrzők toostore hivatkozott objektumokat, amelyek még nem lettek importálva. Például, ha a szinkronizálás konfigurált tooinclude hello manager attribútum hello *Abbie Spencer* objektumot, és hello kapott értéket egy objektumot, amely nem lett még, importált, többek között *CN Kálmán Sperry, CN = Users, DC = = fabrikam DC = com*, hello manager információkat tárolja a kapcsolódási térbe hello helyőrzőként. Hello objektum később importálható, ha a rendszer felülírja a hello helyőrző objektum által hello átmeneti hello manager képviselő objektum.

### <a name="metaverse-objects"></a>Metaverzum-objektum
A metaverzum-objektum hello összevont nézetet tartalmaz, hogy a szinkronizálási motor hello kapcsolódási térbe objektumok átmeneti hello rendelkezik. Szinkronizálási motor metaverse objektumokat hoz létre. az objektum importálása hello információk segítségével. Több összekötő terület objektumok csatolt tooa egyetlen metaverzum-objektum, de egy összekötő terület objektum nem lehet csatolt toomore, mint egyetlen metaverzum-objektumhoz.

Metaverzum-objektum nem lehet manuálisan létrehozott vagy törölt. hello szinkronizálási motor, amely nem rendelkezik egy tooany összekötő terület objektum hello kapcsolódási térbe metaverzum-objektumok automatikusan törli.

csatlakoztatott tooa megfelelő objektum adatforrástípust belül hello metaverse belüli objektumok toomap, a szinkronizálási motor egy bővíthető séma objektumtípusokra és a kapcsolódó attribútumok előre meghatározott számú biztosít. Új objektumtípusok és metaverzum-objektum attribútumainak hozhat létre. Attribútumok egyértékű vagy többértékű lehet, és hello attribútumtípust karakterláncok, hivatkozások, számok és logikai értékek lehetnek.

### <a name="relationships-between-staging-objects-and-metaverse-objects"></a>Átmeneti és metaverzum objektumok közötti kapcsolatok
Hello szinkronizálási motor névtéren belül hello adatfolyama szerint hello hivatkozás kapcsolat átmeneti és metaverzum-objektumok között engedélyezett. Egy átmeneti csatolt tooa metaverzum-objektum neve, amely objektum egy **illesztett objektum** (vagy **összekötő objektum**). Egy átmeneti az objektumot, amely nem csatolt tooa metaverzum-objektum egy **objektum leválasztják** (vagy **Terheléskapcsoló objektumok**). hello feltételek csatlakoztatva, és leválasztják vannak előnyben részesített toonot tévessze össze az adatok importálása és exportálása a csatlakoztatott könyvtárból felelős összekötők hello.

Helyőrzői soha nem csatolt tooa metaverzum-objektum

Illesztett objektum magában foglalja, egy átmeneti objektum és a csatolt kapcsolat tooa egyetlen metaverzum-objektum. Illesztett objektum használt toosynchronize attribútumértékek a connector terület objektum és a metaverzum-objektum között.

Amikor egy átmeneti objektum illesztett objektum szinkronizálás során, attribútumok áramolhasson az objektum és a metaverzum-objektum hello átmeneti hello között. Attribútumfolyam kétirányú és attribútum szabályok importálása és exportálása attribútum szabályok segítségével konfigurálhatók.

Egy egyetlen összekötőt terület objektum csatolt tooonly egy metaverzum-objektum lehet. Azonban minden metaverzum-objektum nem csatolt toomultiple összekötő terület objektumok hello azonos vagy különböző összekötőterek, ahogy az ábra a következő hello.

![Arch5](./media/active-directory-aadconnectsync-understanding-architecture/arch5.png)

hello kapcsolódó hello átmeneti objektum közötti kapcsolatot, és a metaverzum-objektum állandó, és csak a megadott szabályok szerint távolíthatja el.

Egy kilépett célja egy átmeneti objektum, amely nem csatolt tooany metaverzum-objektum. hello attribútum kilépett objektum értékei nem dolgozza fel, minden további, hello metaverse belül. hello hello megfelelő objektum hello csatlakoztatott adatforrás nem frissíti a szinkronizálási motor attribútum.

Kilépett objektumok használatával a szinkronizálási motor azonosító adatok tárolására, és később feldolgozni azt. Egy átmeneti objektum tartása hello kapcsolódási térbe kilépett objektumként számos előnye van. Hello rendszer már rendelkezik előkészített hello szükséges információ ezt az objektumot, mert nincs szükség toocreate hello során újra ez az objektum ábrázolása következő importálása hello csatlakoztatott adatforrásból. Ezzel a módszerrel szinkronizálási motor mindig egy teljes pillanatkép tartozik hello csatlakoztatott adatforrás, akkor is, ha nincs aktuális kapcsolati toohello csatlakoztatott adatforrások. Kilépett objektumok alakíthatók illesztett objektumokat, és fordítva, attól függően, hogy Ön meghatározott hello szabályok.

Az importálás objektum kilépett objektumként jön létre. Az Exportálás objektum illesztett objektumnak kell lennie. hello rendszer programot érvénybe lépteti az e szabály, és töröl minden exportálási objektum, amely nem egy illesztett objektum.

## <a name="sync-engine-identity-management-process"></a>Szinkronizálási motor identity management folyamat
hello identity management folyamat szabályozza, hogyan történik a azonosító adatok között különböző csatlakoztatott adatforrások frissítése. Az Identitáskezelés három folyamat akkor fordul elő:

* Importálás
* Szinkronizálás
* Exportálás

Hello importálási folyamat során a szinkronizálási motor hello bejövő azonosító adatokat a csatlakoztatott adatforrás értékeli ki. Észlel, amikor az átmeneti új objektumot hoz létre vagy hello kapcsolódási térbe a szinkronizálás a meglévő átmeneti objektumokhoz.

Hello szinkronizálási folyamat a szinkronizálási motor hello metaverse tooreflect végrehajtott módosításokat a kapcsolódási térbe hello frissíti, és frissíti a hello összekötő terület tooreflect végrehajtott módosításokat a hello metaverse.

Hello exportálás során a szinkronizálási motor, amely elő van készítve az objektumok fejlesztői és, exportálás függőben vannak megjelölve módosításokat leküldéses értesítések.

hello a következő ábrán látható, ahol hello folyamatok mindegyikének akkor következik be, mint identitás információáramlás egy csatlakoztatott adatok forrás tooanother a.

![Arch6](./media/active-directory-aadconnectsync-understanding-architecture/arch6.png)

### <a name="import-process"></a>Az importálási folyamat
Hello importálási folyamat során a szinkronizálási motor kiértékeli tooidentity információkat. Szinkronizálási motor összehasonlítja a csatlakoztatott adatforrás hello hello azonosító adatok átmeneti tárolási objektumra vonatkozó kapott hello azonosító adatok, és meghatározza, hogy átmeneti objektum hello frissítésekre van szüksége. Ha új adatokkal objektum átmeneti szükséges tooupdate hello, hello objektum átmeneti, függőben lévő importálás van megjelölve.

Által a szinkronizálás előtt hello kapcsolódási térbe objektumok átmeneti, a szinkronizálási motor csak hello azonosító adatok módosított tud feldolgozni. Ez a folyamat hello a következő előnyöket biztosítja:

* **Hatékony szinkronizálási**. a szinkronizálás során feldolgozandó adatok mennyiségétől hello másodpercekre csökken.
* **Az újraszinkronizálás hatékony**. Módosíthatja, miként dolgozza fel a szinkronizálási motor az azonosító adatok újracsatlakozására hello szinkronizálási motor toohello adatforrás nélkül.
* **Lehetőség toopreview szinkronizálási**. Megtekintheti, hogy helyesen-e a hello identity management folyamat feltételezéseket szinkronizálási tooverify.

Az egyes objektumok hello összekötő megadott hello szinkronizálási motor először próbálja toolocate hello objektum hello összekötő címtere hello összekötő ábrázolása. Szinkronizálási motor hello kapcsolódási térbe átmeneti objektumaira megvizsgálja, és próbálja meg a megfelelő átmeneti objektum, amely rendelkezik a megfelelő horgonyattribútum toofind. Ha nincs meglévő átmeneti objektum megfelelője az eszközobjektumok attribútum, a szinkronizálás a motor záma toofind hello a megfelelő átmeneti objektum azonos megkülönböztető név.

Ha a szinkronizálási motor egy átmeneti objektum, amely megfelel a megkülönböztető név szerint, de nem a horgony talál, hello következő különleges történik:

* Hello hello kapcsolódási térbe található objektumnak nincs kapcsolati majd szinkronizálási motor hello kapcsolódási térbe eltávolítja ezt az objektumot, és jelek hello metaverzum-objektum esetén csatolt tooas **ismételje meg a következő szinkronizálás alkalmával futtatása kiépítés**. Ezután azt hello új objektumot hoz létre importálása.
* Ha hello objektum hello kapcsolódási térbe található horgonyra, majd szinkronizálási motor feltételezi, hogy ez az objektum rendelkezik vagy átnevezték vagy törölték a hello csatlakoztatott címtárhoz. Egy új, ideiglenes objektum megkülönböztető nevét hello összekötő terület ki, hogy azt is készítheti hello bejövő objektum. hello régi objektum lesz **átmeneti**, hello összekötő tooimport hello átnevezése vagy törlése tooresolve hello helyzet való várakozás közben.

Ha a szinkronizálási motor egy átmeneti objektum, amely megfelel a megadott összekötő hello toohello objektum megkeresi, meghatározza, hogy milyen változtatások tooapply. Például szinkronizálási motor lehet, hogy nevezze át vagy törölje hello csatlakoztatott adatforrás hello objektumát, vagy lehet, hogy az attribútumértékek hello objektum csak frissíti.

Frissített adatok átmeneti objektumok, függőben lévő importálás lesznek megjelölve. Különböző típusú importálja függőben lévő érhetők el. Attól függően, hogy az importálási folyamat hello hello eredményét egy átmeneti hello kapcsolódási térbe objektum rendelkezik-e függőben lévő importálás típusa a következő hello:

* **Nincs**. Nincs módosítások tooany hello átmeneti objektum attribútumainak hello érhetők el. Szinkronizálási motor nem ez a jelző ehhez a típushoz, függőben lévő importálás.
* **Adja hozzá**. átmeneti objektum hello hello kapcsolódási térbe új importálási objektumot. Szinkronizálási motor jelzők ehhez a típushoz, függőben lévő importálás hello metaverse további feldolgozásra.
* **Frissítés**. Szinkronizálási motor hello kapcsolódási térbe egy megfelelő tesztelési objektumot talál, és ez típusú, mint a függőben lévő importálás tartalomként jelöli meg, hogy a frissítések toohello attribútumok dolgozhatók fel hello metaverse. Frissítések tartalmaznak objektum átnevezése.
* **Törlés**. Szinkronizálási motor hello kapcsolódási térbe egy megfelelő tesztelési objektumot talál, és a függőben lévő importálás típusú tartalomként jelöli meg, hogy hello illesztett objektum törlése.
* **Törlés/felvétele**. Szinkronizálási motor hello kapcsolódási térbe egy megfelelő tesztelési objektumot talál, de hello objektum típusa nem egyezik. Ebben az esetben a delete hozzáadási módosítása elő van készítve. A delete-hozzáadása módosítása toohello szinkronizálási motor, amely az objektum teljes újraszinkronizálási kell fordulhat elő, mert más-más részhalmazához szabályok alkalmazása toothis objektum hello objektumtípus megváltozásakor jelzi.

Függőben lévő importálás egy átmeneti objektum állapotának beállítása hello esetén előfordulhat tooreduce jelentősen hello szinkronizálás során adatfeldolgozási, mert ezzel lehetővé teszik hello rendszer tooprocess csak azokat az objektumokat, amelyek frissítették adatok mennyisége.

### <a name="synchronization-process"></a>Szinkronizálási folyamat
Szinkronizálás két kapcsolódó folyamatokat foglalja magában:

* Bejövő szinkronizálás, hello metaverse hello tartalmának frissítésekor hello kapcsolódási térbe hello adatok használatával.
* Kimenő szinkronizálási, hello kapcsolódási térbe hello tartalmának frissítésekor hello metaverse adatok használatával.

A kapcsolódási térbe hello előkészített hello információk segítségével hello vonatkozó bejövő szinkronizálási folyamat hoz létre hello metaverse integrált hello hello hello csatlakoztatott adatforrások tárolt adatok ábrázolása. Minden átmeneti objektumot, vagy csak azokat a függőben lévő importálás információkkal összesítése, attól függően, hogy hogyan hello szabályok úgy vannak konfigurálva.

hello kimenő szinkronizálási folyamat frissítések objektumok exportálása metaverzum-objektumok módosításakor.

Bejövő szinkronizálás integrált hello nézet hello metaverse hello azonosító adatok hello csatlakoztatott adatforrások érkezett hoz létre. Szinkronizálási motor képes a azonosító adatok bármikor hello, hogy azt a hello csatlakozott adatforrás legújabb azonosító adatok használatával.

**Bejövő szinkronizálás**

Vonatkozó bejövő szinkronizálási folyamat a következő hello tartalmazza:

* **Kiépítés** (más néven **leképezése** esetén fontos toodistinguish ezt a folyamatot, a kimenő szinkronizálási kiépítése). hello szinkronizálási motor hoz létre egy új metaverzum-objektum, egy átmeneti objektum alapján, és csatolja őket. Kiépítés objektumszintű során.
* **Csatlakozás**. hello szinkronizálási motor egy átmeneti objektum tooan meglévő metaverzum-objektum hivatkozásokat tartalmaz. Illesztés objektumszintű során.
* **Attribútumfolyam importálása**. Szinkronizálási motor frissíti hello attribútumértékeket Attribútumfolyam hello hello metaverzum-objektum neve. Attribútumfolyam importálása során attribútum szintű, amely egy átmeneti objektum és a metaverzum-objektum közötti kapcsolat szükséges.

Kiépítés hello a hello metaverzum-objektumokat hoz létre egyetlen folyamat. Kiépítés csak kilépett objektumok importálása objektumok hatással van. Során kiépítését a szinkronizálási motor létrehozza a metaverzum-objektum, amely megfelel-e toohello objektumtípus hello importálási objektum, és mindkét objektumok, így a csatlakoztatott objektum létrehozása közötti kapcsolatot hoz létre.

hello csatlakozási folyamat is az objektum importálása és a metaverzum-objektum közötti kapcsolatot hoz létre. hello közötti csatlakozást és a kiépítés különbség, hogy hello illesztési folyamathoz az szükséges objektumokat, amelyek hello importálási csatolt tooan meglévő metaverzum-objektum, ahol hello kiépítési folyamat létrehoz egy új metaverzum-objektum.

Szinkronizálási motor megpróbál egy importálási objektum tooa metaverzum-objektum toojoin hello szinkronizálási szabály konfigurációjában megadott feltételek alapján.

Hello létesítése és a join folyamatok során szinkronizálási motor egy kilépett objektum tooa metaverzum-objektum, így csatlakoztatott hivatkozásokat tartalmaz. Ezen objektumszintű műveletek elvégzése után a szinkronizálási motor frissítheti hello attribútumértékek hello társított metaverzum-objektum. A folyamat elnevezése importálási Attribútumfolyam.

Összes importálási objektum, amely új adatot, illetve csatolt tooa metaverzum-objektum importálási Attribútumfolyam történik.

**Kimenő szinkronizálási**

Kimenő szinkronizálási frissítések exportálhatja az objektumokat, ha módosítja a metaverzum-objektum, de nem törlődik. hello kimenő szinkronizálási célja tooevaluate e módosítások toometaverse objektumok szükséges frissítések toostaging objektumok hello összekötőterek. Bizonyos esetekben hello módosítások megkövetelheti, hogy átmeneti összes összekötőterek objektumokat frissíteni. Módosított átmeneti objektumok exportálása, így azok az objektumok exportálása függőben vannak megjelölve. Ezek az exportálási objektumok vannak később leküldött toohello csatlakoztatott adatforrás hello exportálási folyamat során.

Kimenő szinkronizálási három folyamat rendelkezik:

* **Kiépítés**
* **Megszüntetés**
* **Attribútumfolyam exportálása**

Kiépítés és megszüntetés mindkét objektumszintű műveletek, amelyek. Megszüntetés attól függ, hogy csak kiépítés is kezdeményezhető, mivel kiépítés. Megszüntetés lesz kiváltva, ha a kiépítés eltávolítja hello hivatkozás metaverzum-objektum és az Exportálás objektum között.

Kiépítés mindig kiváltva, ha a változtatások a hello metaverse alkalmazott tooobjects. Ha toometaverse objektumok módosításai, szinkronizálási motor is elvégezheti a feladatok hello kiépítési folyamat részeként a következő hello bármelyikét:

* Hozzon létre csatlakoztatott objektumainak, ahol a metaverzum-objektum az az újonnan létrehozott csatolt tooa exportálási objektum.
* Nevezze át a csatlakoztatott objektum.
* Metaverzum-objektum és az átmeneti kilépett objektum létrehozása objektumok közötti kapcsolatok disjoin.

Kiépítés van szüksége a szinkronizálási motor toocreate összekötő új objektumot, ha átmeneti objektum toowhich hello metaverzum-objektum hello kapcsolódó pedig mindig exportálási objektum, mert hello objektum még nem létezik a csatlakoztatott adatforrás hello.

Ha a kiépítés van szükség a szinkronizálási motor toodisjoin egy-egy kilépett objektum létrehozása illesztett objektum megszüntetés elindul. megszüntetés folyamat hello hello objektum törlése.

Megszüntetés, során exportálási objektum nem fizikailag törlése hello objektum. hello objektum van megjelölt **törölt**, ami azt jelenti, hogy hello törlési művelet előkészített hello objektumon.

Exportálás Attribútumfolyam is hello kimenő szinkronizálási folyamat során előforduló toohello Attribútumfolyam importálása hasonló módon történik vonatkozó bejövő szinkronizálási. Exportálás Attribútumfolyam csak tartományhoz csatlakoztatott metaverse és exportálási objektumok között történik.

### <a name="export-process"></a>Exportálási folyamata
Hello exportálás során szinkronizálási motor megvizsgálja az összes objektumok exportálása, amely az hello kapcsolódási térbe exportálása függőben vannak megjelölve, és ezután elküldi frissítések toohello csatlakoztatott adatforrás.

hello szinkronizálási motor megállapíthatja, hogy az Exportálás sikerességének hello, de nem tudja megfelelően megállapítani, hogy hello identity management folyamat be nem fejeződik. Objektumok hello csatlakoztatott adatforrás mindig módosítható úgy, hogy más folyamatokkal. Szinkronizálási motor nem rendelkezik egy állandó kapcsolatot toohello csatlakoztatott adatforráshoz, mert nincs elegendő toomake feltételezéseket hello hello csatlakoztatott adatforrás csak az Exportálás értesítést alapján objektum tulajdonságainak.

Például egy folyamat hello csatlakoztatott adatforrás sikerült módosítás hello címtárobjektum-attribútumok biztonsági tootheir az eredeti értékek (Ez azt jelenti, hogy hello csatlakoztatott adatforrás felülírhatja hello értékek hello adatok fejlesztőre ki szinkronizálási motor által, és sikeresen után azonnal alkalmazza a csatlakoztatott adatforrás hello).

hello szinkronizálási motor tárolók exportálja és importálja a minden átmeneti objektumot állapotának adatait. Ha hello attribútum listában megadott hello attribútumainak értékeit hello utolsó exportálási óta módosult, hello importálási tárolására, és állapot lehetővé teszi, hogy szinkronizálási motor tooreact megfelelően exportálása. Szinkronizálási motor hello importálási folyamat tooconfirm attribútumértékek exportált toohello csatlakoztatott adatforrás is használ. Hello összehasonlítása importálja, és az exportált adatok, ahogy az a következő ábrán hello lehetővé teszi a szinkronizálási motor toodetermine hogy hello exportálása sikeres volt-e, vagy hogy szükséges-e ismétlődő toobe.

![Arch7](./media/active-directory-aadconnectsync-understanding-architecture/arch7.png)

Például ha a szinkronizálási motor exportálja attribútum C, melynek értéke 5, tooa csatlakoztatott adatforrás, C = 5 tárol az Exportálás állapot memória. Minden további exportálási ezen az objektumon újra eredményezi egy kísérlet tooexport C = 5 toohello csatlakoztatott adatforráshoz, mert szinkronizálási motor feltételezi, hogy ez az érték nem állandó alkalmazott toohello objektum (Ez azt jelenti, kivéve, ha egy másik értéket nemrég lett importálva hello összekapcsolt adatforrás). hello exportálási memória nincs bejelölve, C = 5 hello objektumon az importálási művelet során fogadásakor.

## <a name="next-steps"></a>Következő lépések
További tudnivalók hello [az Azure AD Connect szinkronizálási szolgáltatás](active-directory-aadconnectsync-whatis.md) konfigurációs.

További információ: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).

