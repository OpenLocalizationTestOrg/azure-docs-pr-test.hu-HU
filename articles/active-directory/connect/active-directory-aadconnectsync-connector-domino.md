---
title: "aaaLotus Domino-összekötő |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooconfigure Microsoft Lotus Domino-összekötő."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: e07fd469-d862-470f-a3c6-3ed2a8d745bf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: affef1fec91eb39f7e91ec274fdd1b3a9c4a32fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="lotus-domino-connector-technical-reference"></a>Lotus Domino-összekötő műszaki útmutatója
Ez a cikk ismerteti a hello Lotus Domino-összekötő. hello cikk vonatkozik toohello a következő termékek:

* A Microsoft Identity Manager 2016 (MIM2016)
* A Forefront Identity Manager 2010 R2 (FIM2010R2)
  * Kell használnia a 4.1.3671.0 gyorsjavítás vagy újabb [KB3092178](https://support.microsoft.com/kb/3092178).

MIM2016 és FIM2010R2, hello összekötő rendelkezésre áll hello letölthető [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495).

## <a name="overview-of-hello-lotus-domino-connector"></a>Lotus Domino-összekötő hello áttekintése
hello Lotus Domino-összekötő lehetővé teszi toointegrate hello szinkronizálási szolgáltatás az IBM Lotus Domino-kiszolgálóval.

Magas szintű szempontjából a következő funkciók hello hello hello összekötő aktuális kiadása támogatja:

| Szolgáltatás | Támogatás |
| --- | --- |
| Csatlakoztatott adatforrás |Kiszolgáló: <li>Lotus Domino 8.5.x</li><li>Lotus Domino 9.x</li>Ügyfél:<li>Lotus Domino 8.5.x</li><li>Lotus Notes 9.x</li> |
| Forgatókönyvek |<li>Objektum életciklusának kezelésére</li><li>Csoportok kezelése</li><li>Jelszókezelés</li> |
| Műveletek |<li>Teljes és különbözeti importálás</li><li>Exportálás</li><li>Állítsa be, és a jelszó módosításához a HTTP-jelszó</li> |
| Séma |<li>Személy (központi felhasználói, forduljon (tanúsítványa nem rendelkező személy))</li><li>Csoport</li><li>Erőforrás (erőforrás, hely, Online értekezletek)</li><li>Mail-adatbázisban</li><li>A támogatott objektumainak attribútumait dinamikus észlelése</li> |

Lotus Domino-összekötő hello hello Lotus Notes ügyfél toocommunicate Lotus Domino-kiszolgáló használ. A függőség miatt a támogatott Lotus Notes ügyfél hello szinkronizálási kiszolgálón telepítenie kell. hello ügyfél- és hello hello kommunikációját hello Lotus Notes .NET együttműködési (Interop.domino.dll) felületen keresztül valósul meg. Ez az interfész elősegíti a hello Microsoft.NET platform és Lotus Notes ügyfél hello kommunikációját, és támogatja a tooLotus Domino dokumentumokhoz és nézetek. Különbözeti importálás esetén is lehetséges, hogy a natív hello C++ illesztő (attól függően, hogy kiválasztott hello különbözeti importálás módszer) használatos.

### <a name="prerequisites"></a>Előfeltételek
Összekötő hello használatához győződjön meg arról, hogy a következő előfeltételek hello szinkronizálási kiszolgálón hello:

* Microsoft keretrendszer 4.5.2-es vagy újabb verzió
* Lotus Notes ügyfél hello telepíteni kell a szinkronizálási kiszolgáló
* Lotus Domino-összekötő hello hello alapértelmezett Lotus Domino LDAP séma (schema.nsf) adatbázis toobe hello Domino Directory kiszolgálón levő igényel. Ha nincs jelen, telepítheti fut, vagy indítsa újra a hello Domino kiszolgálón hello LDAP szolgáltatást.

### <a name="connected-data-source-permissions"></a>Csatlakoztatott adatforrás-engedélyek
tooperform Lotus Domino-összekötő a feladatok bármelyik hello támogatott, a következő csoport tagjának kell lennie:

* Teljes hozzáférés a rendszergazdák
* Rendszergazdák
* Adatbázis-rendszergazdák

hello alábbi táblázat minden egyes művelethez szükséges hello engedélyek:

| Művelet | Hozzáférési jogok |
| --- | --- |
| Importálás |<li>Olvassa el a nyilvános dokumentumok</li><li> Teljes körű hozzáférési rendszergazda (teljes hozzáférés a Rendszergazdák csoport tagja esetén automatikusan rendelkezik hello hatályos hozzáférés tooin ACL.)</li> |
| Exportálás és a jelszó beállítása |Hatályos hozzáférés: <li>Dokumentumok létrehozása</li><li>Dokumentumok törlése</li><li>Olvassa el a nyilvános dokumentumok</li><li>Nyilvános dokumentumok írása</li><li>Replikálás vagy másolat dokumentumok</li>Exportálási műveleteket is van szüksége a következő szerepkörök hello: <li>CreateResource</li><li>GroupCreator</li><li>GroupModifier</li><li>UserCreator</li><li>UserModifier</li> |

### <a name="direct-operations-and-adminp"></a>Közvetlen műveletek és AdminP
Műveletek lépjen közvetlenül toohello Domino könyvtár vagy keresztül hello AdminP feldolgozni. hello alábbi táblázatok tartalmazzák az összes támogatott objektumokat, műveletek és a, ha lehetséges, hello kapcsolódó metódus végrehajtása:

**Elsődleges címjegyzék**

| Objektum | Létrehozás | Frissítés | Törlés |
| --- | --- | --- | --- |
| Személy |AdminP |Közvetlen |AdminP |
| Csoport |AdminP |Közvetlen |AdminP |
| MailInDB |Közvetlen |Közvetlen |Közvetlen |
| Erőforrás |AdminP |Közvetlen |AdminP |

**Másodlagos címjegyzék**

| Objektum | Létrehozás | Frissítés | Törlés |
| --- | --- | --- | --- |
| Személy |N/A |Közvetlen |Közvetlen |
| Csoport |Közvetlen |Közvetlen |Közvetlen |
| MailInDB |Közvetlen |Közvetlen |Közvetlen |
| Erőforrás |N/A |N/A |N/A |

Amikor egy erőforrást hoz létre, a megjegyzéseket dokumentum jön létre. Hasonlóképpen a törölt egy erőforrás hello megjegyzések dokumentum törlődik.

### <a name="ports-and-protocols"></a>Portok és protokollok
IBM Lotus Notes ügyfél és a kiszolgálók Domino kommunikációhoz megjegyzések távoli eljárás hívása (NRPC) ahol NRPC kell használni a TCP/IP használata. hello alapértelmezett portszám 1352, de hello Domino rendszergazda módosíthatja.

### <a name="not-supported"></a>Nem támogatott
a következő műveletek hello hello jelenlegi kiadásban hello Lotus Domino-összekötő nem támogatja:

* Helyezze át a postaláda-kiszolgáló között.

## <a name="create-a-new-connector"></a>Új összekötő létrehozása
### <a name="client-software-installation-and-configuration"></a>Ügyfélszoftver telepítése és konfigurálása
Lotus Notes hello kiszolgálón telepítendő **előtt** hello összekötő telepítve van.

Amikor telepíti, semmiképpen egy **egyfelhasználós telepítés**. alapértelmezett hello **többfelhasználós telepítése** nem működik.  
![Notes1](./media/active-directory-aadconnectsync-connector-domino/notes1.png)

Hello szolgáltatások lapon telepítése csak a hello szükséges Lotus Notes funkciók és **egyetlen ügyfél-bejelentkezési**. Egyszeri bejelentkezési hello összekötő toobe képes toolog toohello Domino kiszolgálón szükség.  
![Notes2](./media/active-directory-aadconnectsync-connector-domino/notes2.png)

**Megjegyzés:** Start Lotus Notes után egy felhasználói hello található ugyanarra a kiszolgálóra hello fiókhoz, hello összekötő szolgáltatásfiókot használja. Emellett győződjön meg arról, hogy tooclose hello Lotus Notes ügyfél hello kiszolgálón. Még nem fut, hello azonos idő hello összekötő megpróbál tooconnect toohello Domino kiszolgáló.

### <a name="create-connector"></a>Összekötő létrehozása
Lotus Domino-összekötő tooCreate, a **szinkronizálási szolgáltatás** válassza ki **kezelőügynök** és **létrehozása**. Jelölje be hello **Lotus Domino (Microsoft)** összekötő.  
![CreateConnector](./media/active-directory-aadconnectsync-connector-domino/createconnector.png)

Ha a szinkronizálási szolgáltatás verziója nem nyújt hello képességét tooconfigure **architektúra**, ellenőrizze, hogy hello összekötő tooits alapértelmezett érték toorun állítható be **folyamat**.

### <a name="connectivity"></a>Kapcsolatok
Hello kapcsolat oldalon meg kell adnia hello Lotus Domino-kiszolgáló nevét, hello bejelentkezési hitelesítő adatok.  
![Kapcsolatok](./media/active-directory-aadconnectsync-connector-domino/connectivity.png)

hello Domino-kiszolgáló következő tulajdonsága hello kiszolgálónév két formátum támogatja:

* Kiszolgálónév
* ServerName/könyvtárnév

Hello **ServerName/könyvtárnév** formátuma hello előnyben részesített formátumban, ehhez az attribútumhoz hiszen ha hello csatlakozó ügyfelek hello Domino kiszolgáló gyorsabban szeretne választ kapni nyújtja.

a megadott hello hello konfigurációs adatbázis hello szinkronizálási szolgáltatás UserID fájl tárolja.

A **különbözeti importálás** ezek közül:

* **Nincs**. Összekötő hello nem bármely különbözeti importálja.
* **Hozzá/frissíthet**. hello Connector biztosítja különbözeti importálás hozzáadása, és a frissítési műveleteket. A Törlés a **teljes importálás** művelet szükség. Ez a művelet hello .net együttműködési használ.
* **Adja hozzá/frissítés/törlés**. hello Connector biztosítja különbözeti importálás hozzáadása, frissítése és törlési műveletek. Ez a művelet hello natív C++ felületek használatával.

A **Sémabeállításokat** alábbi beállítások hello rendelkezik:

* **Alapértelmezett séma**. hello összekötő észlel hello séma hello Domino-kiszolgálóról. Ez a beállítás nem hello alapértelmezett beállítás.
* **DSML-séma**. Csak akkor használható, ha hello Domino-kiszolgáló nem fed fel hello séma. Ezután hozzon létre egy DSML fájlt hello séma, és ehelyett importálja azt. A DSML további információkért lásd: [OASIS](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=dsml).

Kattintson a Tovább gombra, ha ellenőrizni hello felhasználóazonosító és jelszó konfigurációs paramétert is.

### <a name="global-parameters"></a>Globális paraméterek
Hello globális paraméterek oldalon hello időzóna és hello importálás konfigurálása, és exportálja a műveletet.  
![Globális paraméterek](./media/active-directory-aadconnectsync-connector-domino/globalparameters.png)

Hello **Domino-kiszolgáló időzónája** paraméter határozza meg a Domino kiszolgáló hello helyét.

Ez a konfigurációs beállítás nem szükséges toosupport **különbözeti importálás** műveletek mert lehetővé teszi, hogy a hello szinkronizálási szolgáltatás közötti hello utolsó két importálja változások megállapítása.

>[!Note]
Hello beállítás toodelete hello felhasználó levelezési adatbázis hello 2017. márciusi frissítés hello globális paraméterek képernyő kezdve tartalmaz hello felhasználó törlése közben.

![Törli a felhasználó postaládájához](./media/active-directory-aadconnectsync-connector-domino/AdminP.png)

#### <a name="import-settings-method"></a>Módszer beállításainak importálása
Hello **végre teljes Import alapján** ezeket a lehetőségeket biztosítja:

* Keresés
* Nézet (ajánlott)

**Keresési** van használatával a Domino, de az indexelő esetében gyakori, hogy hello indexek nem frissülnek a valós idejű és hello adatok hello kiszolgáló által visszaadott nem mindig megfelelő. A rendszer sok módosításokkal ezt a beállítást általában nem jól működik és biztosít bizonyos esetekben törli hamis. Azonban **keresési** gyorsabb, mint **nézet**.

**Nézet** hello ajánlott lehetőséget biztosít az adatok megfelelő állapotát hello óta. Némileg lassabban futnak, mint a keresés.

#### <a name="creation-of-virtual-contact-objects"></a>Virtuális kapcsolattartási objektumok létrehozása
Hello **létrehozását lehetővé \_forduljon objektum** ezeket a lehetőségeket biztosítja:

* None
* Nem-hivatkozási érték
* Referencia- és nem hivatkozásos értékek

Domino, a hivatkozás attribútumok sok különböző formátumokban tooreference más objektumok tárolására. toobe képes toorepresent eltérő változata, hello összekötő valósít meg \_objektumok, más néven forduljon **virtuális névjegyek** (VC). Ezek az objektumok van, lehetővé teszi azok csatlakoztatását tooexisting MV-objektum létrehozása, vagy új objektumként várható. Így megőrizhetők attribútum hivatkozik.

Ez a beállítás engedélyezésével és hello a hivatkozási attribútum tartalma nem DN formátumban, ha egy \_forduljon objektum jön létre. Például egy csoport egy tagja attribútum SMTP-címeket tartalmazhat. Azt is lehetséges toohave rövid_név és egyéb attribútumai jelent-e a hivatkozási attribútum. A jelen esetben válassza ki a **nem-hivatkozási érték**. Ez a konfiguráció az hello leggyakoribb beállítása Domino-megvalósítások esetében.

Lotus Domino konfigurált toohave külön címtárakat különböző megkülönböztető névvel rendelkező képviselő hello ugyanahhoz az objektumhoz, az lehetséges tooalso létrehozása \_objektumok kérjen címjegyzék található hivatkozás értékeket. A jelen esetben válassza ki a hello **és a nem-hivatkozási** lehetőséget.

Ha több érték szerepel hello attribútum **FullName** Domino, majd is érdemes virtuális partnerek tooenable hello létrehozása, a hivatkozások feloldhatók legyenek. Ez az attribútum például rendelkezhet több érték házasság vagy felbontására után. Jelölje be a jelölőnégyzetet hello **engedélyezése... Több érték tartozik FullName** ehhez a forgatókönyvhöz.

Hello megfelelő attribútumok összeillesztésével hello \_forduljon objektumok lenne illesztett toohello MV-objektum.

Ezek az objektumok rendelkeznek VC =\_forduljon hozzá tootheir megkülönböztető neve.

#### <a name="import-settings-conflict-object"></a>Beállítások importálása, ütköző objektum
**Ütközés objektum kizárása**

Nagy Domino megvalósításában akkor lehetséges, hogy több objektum van hello azonos DN tooreplication problémák miatt. Ezekben az esetekben hello összekötő két objektumok különböző UniversalIDs, de ugyanazon megkülönböztető neve jelenik meg. Az ütközés okozhatja egy átmeneti objektum hello kapcsolódási térbe létrehozása folyamatban. hello összekötő hello objektumok, mint a replikációs áldozatai Domino a kijelölt figyelmen kívül hagyhatja. hello javaslat jelölve ez a jelölőnégyzet tookeep.

#### <a name="export-settings"></a>Beállítások exportálása
Ha hello lehetőséget **AdminP használja a hivatkozások frissítését** nincs bejelölve, akkor hivatkozás attribútumait, például a tag, exportálásának közvetlen hívásakor és hello AdminP folyamatot használ. Csak akkor használja ezt a beállítást, ha AdminP nem lett konfigurálva toomaintain a hivatkozási integritás.

#### <a name="routing-information"></a>Útvonal-információkat
Az Domino lehetséges, hogy rendelkezik-e a hivatkozási attribútum egy utótagot toohello megkülönböztető Nevet, a beágyazott útvonal-információkat is. Hello tag attribútum egy csoport tartalmazhatnak például **CN =example/organization@ABC**. hello utótag @ABC hello útválasztási adatokat. hello útválasztási adatok Domino toosend e-mailek toohello megfelelő Domino rendszer, a rendszer egy másik vállalatnál legyen. Hello útválasztási információ mezőben megadhat hello útválasztási-utótagokat az összekötő hello hatókörében hello szervezeten belül. Ha a következő értékek egyike a hivatkozási attribútum található utótagként, hello hivatkozás eltávolítja a hello az útválasztási adatokat. Ha útválasztási utótagot hello egy hivatkozási érték nem lehet megadva, ezeket az értékeket az egyező tooone egy \_forduljon objektum létrehozása. Ezek \_forduljon objektumok jönnek létre **RO = @<RoutingSuffix>**  beszúrt hello megkülönböztető neve. Ezek \_forduljon objektumok is vannak a következő attribútumok hello hozzáadott tooa valódi objektum csatlakozás szükség tooallow: \_routingName, \_ügyintéző, \_displayName, és UniversalID.

#### <a name="additional-address-books"></a>További címtárakat
Ha nem rendelkezik **directory segítséget** telepítve, amely biztosítja, hogy másodlagos címtárakat hello nevét, majd manuálisan adja meg ezeket a címtárakat.

#### <a name="multivalued-transformation"></a>Többértékű átalakítása
Lotus Domino sok attribútumokat többértékű adatelemeket. hello megfelelő metaverse attribútumokat általában egyetlen értékelni. Hello importálása és hello exportálási művelet beállítás konfigurálásával hello összekötő toohelp az érintett hello attribútumok szükséges hello fordítását engedélyezi.

**Exportálás**  
hello exportálási művelet beállítás két módot támogat:

* Elem hozzáfűzése
* Cserélje le a cikk

**Cserélje le a cikk** – Ha ezt a beállítást, mindig hello összekötő eltávolítása hello aktuális értékek hello attribútum a Domino és cserélni hello megadott értékeket. a megadott hello egyértékű vagy többértékű adatelemeket értékű lehet.

Példa: hello Segéd attribútum a személy objektum rendelkezik a következő értékek hello:

* CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
* CN = John Smith/OU=Contoso/O=Americas,NAB=names.nsf

Ha egy új Segéd nevű **David Alexander** van hozzárendelve toothis személy objektum, hello eredménye:

* CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf

**Elem hozzáfűzése** – ezt a beállítást, ha hello összekötő megőrzi hello meglévő Domino és insert új értékek hello adatok listája hello tetején hello attribútum értékét.

Példa: hello Segéd attribútum a személy objektum rendelkezik a következő értékek hello:

* CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
* CN = John Smith/OU=Contoso/O=Americas,NAB=names.nsf

Ha egy új Segéd nevű **David Alexander** van hozzárendelve toothis személy objektum, hello eredménye:

* CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf
* CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
* CN = John Smith/OU=Contoso/O=Americas,NAB=names.nsf

**Importálás**  
hello importálási művelet beállítás két módot támogat:

* Alapértelmezett
* Többértékű tooSingle érték

**Alapértelmezett** – Ha hello alapértelmezett beállítást választja, akkor minden hello attribútumok importált összes értékét.

**Többértékű tooSingle érték** – Ha ezt a lehetőséget választja egy többértékű attribútum egy egyértékű attribútum alakítható. Ha egynél több érték már létezik, hello értéket (Ez az érték akkor általában is hello legújabb) hello felül használja.

Példa: hello Segéd attribútum a személy objektum rendelkezik a következő értékek hello:

* CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf
* CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
* CN = John Smith/OU=Contoso/O=Americas,NAB=names.nsf

hello legutóbbi frissítés toothis attribútum **David Alexander**. Hello importálási művelet beállítás tooMultivalued tooSingle érték van beállítva, mert összekötő csak importálja **David Alexander** hello kapcsolódási térbe kerülnek.

hello logika tooconvert többértékű attribútumok be egyértékű attribútumok nem alkalmazza a toohello tag attribútum és toohello személy fullname attribútum.

Azt is lehetséges tooconfigure szabályokat importálhat és exportálhat átalakítása attribútum, egy többértékű attribútumok egy kivétel toohello globális szabály. tooconfigure ezt a beállítást, adja meg a [objecttype]. [attributename] hello a **attribútum kivétellistát importálása** és **attribútum kivétellistát exportálása** szövegmezőket. Például ha Person.Assistant hello globális jelző be van állítva a tooimport összes értéket, egyetlen hello első érték hello Segéd az importált.

#### <a name="certifiers"></a>Képesítést adók engedélyezése
Minden szervezet vagy szervezeti egység hello összekötő alapján vannak listázva. toobe képes tooexport személy objektumok toohello elsődleges címjegyzék, egy certifier a hozzá tartozó jelszó megadása kötelező.

Ha minden képesítést adók engedélyezése hello ugyanazt a jelszót, hello **jelszavát minden Certifers** is használható. Ezután írja be ide a hello jelszót, és csak adja meg a hello certifier fájlt.

Ha csak importálja, majd nincs toospecify bármely képesítést adók engedélyezése.

### <a name="configure-provisioning-hierarchy"></a>Kiépítési hierarchia konfigurálása
Hello Lotus Domino-összekötő konfigurálásakor a párbeszédpanelen lap kihagyása. hello Lotus Domino csatlakozója nem támogatja a kiépítési hierarchia.  
![Telepítési hierarchia](./media/active-directory-aadconnectsync-connector-domino/provisioninghierarchy.png)

### <a name="configure-partitions-and-hierarchies"></a>Partíciók és hierarchiák konfigurálása
Partíciók és hierarchiák konfigurálásakor ki kell választania hello elsődleges címjegyzék NAB=names.nsf nevezik. Továbbá toohello elsődleges címjegyzék, választhat, másodlagos címtárakat ha vannak ilyenek.  
![Partíciók](./media/active-directory-aadconnectsync-connector-domino/partitions.png)

### <a name="select-attributes"></a>Attribútumok kiválasztása
Az attribútumok konfigurálásakor ki kell választania az összes attribútum, amely fűzve előtagként  **\_MMS\_**. Ezek az attribútumok szükség, amikor új objektumok tooLotus Domino

![Attribútumok](./media/active-directory-aadconnectsync-connector-domino/attributes.png)

## <a name="object-lifecycle-management"></a>Objektum életciklusának kezelésére
Ez a szakasz áttekintést Domino hello különböző objektumokat.

### <a name="person-objects"></a>Személy-objektumok
hello személy objektum szervezet és a szervezeti egység felhasználók jelöli. Ezenkívül toohello alapértelmezett attribútumok hello Domino rendszergazda adhat hozzá az egyéni attribútumok tooa személy objektum. Legalább egy személy objektum tartalmaznia kell az összes kötelező attribútumok. A kötelező attribútumok teljes listáját lásd: [Lotus Notes tulajdonságok](#lotus-notes-properties). egy személy objektum, a következő előfeltételek hello tooregister kell teljesülniük:

* hello címjegyzék (names.nsf) kell definiálva, és hello elsődleges címjegyzék kell tenni.
* Kell, hogy legyen hello O/OU certifier azonosítója és hello jelszó tooregister egy adott felhasználó hello szervezet vagy szervezeti egység.
* Meg kell adni egy meghatározott Lotus Notes egy személy objektum tulajdonságait. Ezek a Tulajdonságok hello személy objektum történő üzembe helyezéséhez használt. További információkért lásd: hello terület [Lotus Notes tulajdonságok](#lotus-notes-properties) újabb ebben a dokumentumban.
* hello kezdeti HTTP személy jelszava egy attribútum és a készlet kiépítése során.
* hello személy objektum hello a következő három támogatott típusok egyike lehet:
  1. A normál felhasználók, akik egy levelezési fájlt és egy felhasználói azonosító
  2. Központi felhasználói (a normál felhasználói központi adatbázisfájlokat tartalmazó)
  3. Névjegyek (nincs azonosítója fájl rendelkező felhasználó)

(Kivéve a névjegyek) személyek további sorolhatók Velünk és nemzetközi felhasználók hello hello értéke által meghatározott \_MMS\_IDRegType tulajdonság. Ezen személyek hello megjegyzések ügyfél tooaccess Lotus Domino-kiszolgálók használatához a megjegyzések azonosítót és egy személy dokumentumot. Megjegyzések mail használata, majd is rendelkeznek egy levelezési fájl. hello aktív regisztrált toobecome kell lennie. További információkért lásd:

* [Megjegyzések felhasználók](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_SETTING_UP_NOTES_USERS.html)
* [Felhasználó regisztrálása](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_REGISTERING_USERS.html)
* [Felhasználók kezelése](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_USERS_5151.html)
* [Felhasználók átnevezése](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_USER_AUTOMATICALLY.html)

Ezeket a műveleteket Lotus Domino végez, és ezután importálja a hello szinkronizálási szolgáltatás.

### <a name="resources-and-rooms"></a>Erőforrások és a
Egy erőforrást egy másik típus egy Lotus Domino-adatbázis. Erőforrások lehetnek a különböző típusú berendezések, például a kivetítők konferenciaterem. Nincsenek erőforrások Lotus Domino-összekötő által támogatott hello erőforrástípus attribútum által meghatározott altípusainak:

| Az erőforrás típusát | Erőforrástípus attribútum |
| --- | --- |
| Hely |1 |
| Erőforrás (Other) |2 |
| Online értekezletek |3 |

Hello erőforrás objektum típusa toowork hello következő szükség:

* Erőforrás-foglalás adatbázis már léteznie kell a csatlakoztatott hello Domino server
* hello hely hello erőforrás már definiálva van

hello erőforrás foglalás adatbázis három típusú dokumentumok tartalmazza:

* Hely profil
* Erőforrás
* Foglalás

Az erőforrás-foglalás adatbázis beállításával kapcsolatos további információkért lásd: [hello erőforrás foglalások adatbázisának beállítása](https://www-01.ibm.com/support/knowledgecenter/SSKTMJ_8.0.1/com.ibm.help.domino.admin.doc/DOC/H_SETTING_UP_THE_RESOURCE_RESERVATIONS_DATABASE.html).

**Hozzon létre, frissítsen, és törli az erőforrást**  
hello létrehozási, frissítési és törlési műveleteket hello erőforrás foglalás adatbázis hello Lotus Domino-összekötő által. Erőforrások Names.nsf (Ez azt jelenti, hogy hello elsődleges címjegyzék)-dokumentumokként jönnek létre. Módosítása és törlése erőforrások kapcsolatos további információkért lásd: [szerkesztése és törlése az erőforrás-dokumentumok](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_EDITING_AND_DELETING_RESOURCE_DOCUMENTS.html).

**Importálás és exportálás erőforrások**  
hello erőforrások importált tooand exportált hello szinkronizálási szolgáltatást, mint minden más objektum lehet. Jelölje be hello objektumtípus erőforrásként konfigurálása során. Sikeres exportálási művelet részletei erőforrástípus, konferencia adatbázis, és egy helynév kell rendelkeznie.

### <a name="mail-in-databases"></a>Mail-adatbázisok
Egy E-mail-adatbázisban, tervezett tooreceive üzenetekben adatbázist. Lotus Domino postaládával, amely nincs társítva van egy adott Lotus Domino-felhasználói fiókot (Ez azt jelenti, hogy nincs saját fájl és a jelszót). Egy e-mail-adatbázisban egy egyedi felhasználói azonosítóját ("rövid neve"), és a saját e-mail cím társított rendelkezik.

Ha egy külön postaláda saját e-mail címmel, amely a különböző felhasználók között megosztható legyen szükség van (például group@contoso.com), egy e-mail-adatbázisban jön létre. hello hozzáférés toothis postaláda keresztül a hozzáférés-vezérlési lista (ACL), hello megjegyzések felhasználók, amelyek számára engedélyezett tooopen hello postaláda hello nevét tartalmazó vezérlik.

Hello szükséges attribútumok listáját lásd: hello terület [kötelező attribútumok](#mandatory-attributes) című cikkben.

Tervezett tooreceive egy levelezési adatbázis esetén a Mail-adatbázisban dokumentum Lotus Domino jön létre. Ez a dokumentum léteznie kell a Domino Directory minden olyan kiszolgálóra, hello adatbázis másolatát tárolja. Mail-adatbázisban dokumentum létrehozása részletes ismertetését lásd: [Mail-adatbázisban dokumentum létrehozása](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_A_MAILIN_DATABASE_DOCUMENT_FOR_A_NEW_DATABASE_OVERVIEW.html).

Mielőtt létrehozna egy E-mail-adatbázisban, hello adatbázis már léteznie kell (kell hozott létre Lotus Admin) hello Domino-kiszolgálón.

### <a name="group-management"></a>Csoportok kezelése
Lotus Domino csoportkezelés hello részletes áttekintést nyújt letölthető a következő erőforrások hello:

* [Csoportok használata](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_USING_GROUPS_OVER.html)
* [Csoport létrehozása](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS_MIDTOPIC_55038956829238418.html)
* [Létrehozás és módosítás csoportok](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS.html)
* [Csoportok kezelése](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_GROUPS_1804.html)
* [Egy csoport átnevezése](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_GROUP_STEPS.html)

### <a name="password-management"></a>Jelszókezelés
Lotus Domino regisztrált felhasználó a jelszavak két típusa van:

1. Felhasználói jelszó (User.id fájlban tárolt)
2. Internet / HTTP jelszó

Lotus Domino-összekötő hello támogatja a csak HTTP jelszóval rendelkező műveleteket.

tooperform jelszókezelés, engedélyeznie kell a hello összekötő felügyeleti ügynök Designer hello a jelszókezelést. tooenable jelszókezelés, jelölje be **jelszókezelés engedélyezése** a hello **bővítmények konfigurálása** párbeszédpanel lap.  
![Bővítmények konfigurálása](./media/active-directory-aadconnectsync-connector-domino/configureextensions.png)

hello Lotus Domino connector támogatása a következő műveletek internetes jelszó:

* Jelszó beállítása: Jelszó beállítása beállítása egy új HTTP/IP-jelszó Domino hello felhasználó. Alapértelmezés szerint az hello fiókkal is oldva. hello zárolásának jelzője hello WMI-felületén hello szinkronizálási motor tesz elérhetővé.
* Jelszó módosítása: Ebben a forgatókönyvben egy felhasználó érdemes toochange hello jelszó vagy felszólítás utáni toochange jelszó megadott idő után. Ez a művelet tootake hely a (hello régi és új jelszó hello) is kötelező. Miután módosította, hello új jelszó Lotus Domino frissül.

További információkért lásd:

* [Hello Internet zárolási szolgáltatás használata](http://www.ibm.com/developerworks/lotus/library/domino8-lockout/)
* [Internet-jelszavak kezelése](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_NOTES_AND_INTERNET_PASSWORD_SYNCHRONIZATION_7570_OVER.html)

## <a name="reference-information"></a>Kapcsolódó információk
Ez a rész felsorolja, például attribútum leírások és hello Lotus Domino-összekötő attribútum követelményei.

### <a name="lotus-notes-properties"></a>Lotus Notes tulajdonságok
Amikor személy objektumok tooyour Lotus Domino directory, az objektumok fel az adott értékek rendelkeznie kell egy adott készletét tulajdonságok. Ezek az értékek csak a szükséges műveletek létrehozása.

hello alábbi táblázat ezeket a tulajdonságokat, és azok leírását.

| Tulajdonság | Leírás |
| --- | --- |
| \_MMS_AltFullName |hello másik felhasználó teljes nevét. |
| \_MMS_AltFullNameLanguage |hello nyelvi toobe megadásának hello másik felhasználó teljes nevét használja. |
| \_MMS_CertDaysToExpire |hello napok számát, amelyek hello az aktuális dátum előtt hello tanúsítvány lejár. Ha nincs megadva, a hello alapértelmezett dátum, két évvel hello az aktuális dátumot. |
| \_MMS_Certifier |A tulajdonság, amely hello certifier hello szervezeti hierarchia nevét tartalmazza. Példa: OU = OrganizationUnit, O szervezeti, C = ország =. |
| \_MMS_IDPath |Ha hello tulajdonság üres, nincs felhasználói azonosító fájl létrejön helyileg hello szinkronizálási kiszolgálót. Hello tulajdonsága tartalmazza a fájl nevét, ha egy felhasználó azonosítója fájl hello madata mappában jön létre. hello tulajdonság is tartalmazhat egy teljes elérési útja. |
| \_MMS_IDRegType |Személyek névjegyek, a US felhasználók és a nemzetközi felhasználók is kell sorolni. hello alábbi táblázat hello a lehetséges értékek: <li>0 - ügyfél</li><li>1 - US felhasználó</li><li>2 - nemzetközi felhasználói</li> |
| \_MMS_IDStoreType |Kötelező tulajdonság az Amerikai Egyesült Államok és nemzetközi felhasználók. hello tulajdonsága tartalmazza az egész szám, amely megadja, hogy hello felhasználói azonosító mellékletként hello megjegyzések címjegyzék vagy a hello személy e-mail fájlban tárolja. Ha hello Felhasználóazonosító fájl hello címjegyzék a mellékletet, opcionálisan létrehozás fájlba \_MMS_IDPath. <li>Üres - tároló azonosítója fájl azonosítója tárolóban, nincs azonosító fájlt (a névjegyek).</li><li> 1 – hello megjegyzések címjegyzékében mellékletet. Hello \_MMS_Password tulajdonságot be kell állítani felhasználói azonosító fájlokat mellékletek</li><li>2 - személy E-mail fájl Azonosítóját tárolja. Hello \_MMS_UseAdminP be kell állítani toofalse toolet hello mail fájl hello személy regisztráció során hozható létre. Hello \_MMS_Password tulajdonságot kell beállítani a felhasználói azonosító fájlok.</li> |
| \_MMS_MailQuotaSizeLimit |hello a száma, amelyek jogosultak az üdvözlő e-mail fájl adatbázis mérete (MB). |
| \_MMS_MailQuotaWarningThreshold |hello a száma, amelyek engedélyezettek hello e-mail fájl adatbázis előtt egy figyelmeztetés mérete (MB). |
| \_MMS_MailTemplateName |hello e-mail sablon megnyitott fájl használt toocreate hello felhasználói e-mail fájl. Ha egy sablon meg van adva, a hello mail fájl jön létre a hello megadott sablonnal. Ha nincs sablon meg van adva, a hello alapértelmezett sablonfájl egy használt toocreate hello fájl. |
| \_MMS_OU |Nem kötelező tulajdonság, amely hello Szervezetiegység-nevet a hello certifier. Ez a tulajdonság a névjegyek üresnek kell lennie. |
| \_MMS_Password |A felhasználók számára szükséges tulajdonság. hello tulajdonság hello jelszót hello azonosító fájl hello objektum tartalmazza. |
| \_MMS_UseAdminP |Tulajdonság set tootrue kell lennie, ha hello levelezési fájl hello AdminP folyamat hello (aszinkron toohello exportálási folyamat) Domino kiszolgálón kell létrehozni. Ha tulajdonság értéke toofalse, hello levelezési fájl hozza létre hello Domino felhasználói (szinkron hello exportálási folyamat). |

A felhasználó az azonosító fájllal, hello \_MMS_Password tulajdonság értéket kell tartalmaznia. E-mail hozzáférés hello Lotus Notes ügyfélen keresztül hello levelezokiszolgalo és a felhasználók MailFile tulajdonságait értéket kell tartalmaznia.

tooaccess e-mailt egy webes böngésző következő tulajdonságai hello értékeket kell tartalmaznia:

* MailFile - hello útvonal hello levelezési fájl tárolására hello Lotus Domino kiszolgálón tartalmazó kötelező tulajdonság.
* Levelezokiszolgalo - kötelező tulajdonság, amely hello hello Lotus Domino-kiszolgáló nevét tartalmazza. Ez az érték hello neve toouse esetén hello Lotus levelezési fájl hello Domino kiszolgálón létrehozott.
* HTTPPassword - hello Web access jelszó hello objektum tartalmazó tulajdonságot nem kötelező megadni.

tooaccess hello Domino Server levelezési képesség nélkül, hello HTTPPassword tulajdonság értéket kell tartalmaznia. hello MailFile tulajdonság, és lehet, hogy hello levelezokiszolgalo tulajdonság üres.

A \_MMS_ IDStoreType (Mail fájl azonosító tároló), 2 = hello NotesRegistrationclass MailSystem tulajdonságának értéke tooREG_MAILSYSTEM_INOTES (3).

### <a name="mandatory-attributes"></a>Kötelező attribútumok
hello Lotus Domino-összekötő elsősorban támogatja az ilyen típusú objektumok (a dokumentum esetében):

* Csoport
* Mail-adatbázisban
* Személy
* Ügyfél (nincs certifier rendelkező személy)
* Erőforrás

Ez a szakasz hello attribútumok, amelyek minden támogatott objektum tooexport tooa Domino kiszolgáló esetében kötelező.

| Objektumtípus | Kötelező attribútumok |
| --- | --- |
| Csoport |<li>ListName</li> |
| Fő-adatbázisban |<li>FullName</li><li>MailFile</li><li>Levelezokiszolgalo</li><li>MailDomain</li> |
| Személy |<li>Vezetéknév</li><li>MailFile</li><li>Rövid_név</li><li>\_MMS_Password</li><li>\_MMS_IDStoreType</li><li>\_MMS_Certifier</li><li>\_MMS_IDRegType</li><li>\_MMS_UseAdminP</li> |
| Ügyfél (nincs certifier rendelkező személy) |<li>\_MMS_IDRegType</li> |
| Erőforrás |<li>FullName</li><li>ResourceType</li><li>ConfDB</li><li>Erőforráskapacitás</li><li>Helykiszolgáló</li><li>displayName</li><li>MailFile</li><li>Levelezokiszolgalo</li><li>MailDomain</li> |

## <a name="common-issues-and-questions"></a>Gyakori problémákat és kérdéseket
### <a name="schema-detection-does-not-work"></a>Séma észlelése nem működik.
toobe képes toodetect hello séma, szükség a hello schema.nsf fájl-e a hello Domino kiszolgáló. Ez a fájl csak akkor jelenik meg, ha az LDAP hello kiszolgálóra van telepítve. Ha hello séma nem észlelhető, ellenőrizze a következő hello:

* hello fájl schema.nsf áll rendelkezésre a(z) hello Domino-kiszolgáló gyökérmappájában hello
* hello felhasználó rendelkezik-e engedélyekkel toosee hello schema.nsf fájlt.
* Egy LDAP-kiszolgáló hello újraindítását kényszeríti ki. Nyissa meg **Lotus Domino konzol** és **mondja el LDAP ReloadSchema** parancs tooreload hello séma.

### <a name="not-all-secondary-address-books-are-visible"></a>Nem minden másodlagos címtárakat láthatók.
hello Domino-összekötő támaszkodik hello szolgáltatás **Directory segítséget** toobe képes toofind hello másodlagos címtárakat. Ha másodlagos címtárakat hello hiányzik, ellenőrizze, hogy ha [Directory segítséget](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_DIRECTORY_ASSISTANCE.html) engedélyezve és konfigurálva a hello Domino-kiszolgáló.

### <a name="custom-attributes-in-domino"></a>Egyéni attribútumok Domino
Többféleképpen is Domino tooextend hello sémában így jelenik meg a fogyasztható egyéni attribútumként hello összekötő által.

**1. módszer: Lotus Domino-séma kiterjesztése**

1. Hozzon létre egy másolatot Domino Directory sablon {PUBNAMES. NTF} következő [ezeket a lépéseket](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) (kell hello alapértelmezett IBM Lotus Domino directory sablont nem testre):
2. Nyissa meg hello másolása a Domino directory sablon {CONTOSO. NTF} sablont, amely Domino-tervezőben hozta létre, és kövesse az alábbi lépéseket:
   * Kattintson a megosztott elemek, és bontsa ki a segédűrlapok
   * Kattintson duplán a ${ObjectName} InheritableSchema segédűrlap (ahol a {ObjectName} annak hello neve hello alapértelmezett strukturális objektumosztály, például: személy).
   * Nevezze el a kívánt tooadd séma {MyPersonAtrribute} hello attribútum és a megfelelő toothat attribútumot. Hozzon létre egy mező válassza hello **létrehozása** menüre, és válassza ki azt **mező** menüből.
   * Hello hozzáadott mezőben az típusának, stílusának, méret, betűtípus, és egyéb kapcsolódó paraméterek mező tulajdonságai ablakban kiválasztásával tulajdonságainak beállítása.
   * Tartsa hello attribútum alapértelmezett értéke megegyezik az objektumhoz megadott hello neveként (például MyPersonAttribute attribútum megadása esetén tartsa hello alapértelmezett értéket a hello ugyanazt a nevet).
   * Hello ${ObjectName} InheritableSchema segédűrlap mentse a frissített értékekkel.
3. Cserélje le a hello Domino Directory sablon {PUBNAMES. NTF} hello új egyéni sablon {CONTOSO. NTF} következő [ezeket a lépéseket](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
4. Zárja be a Domino rendszergazda, és nyissa meg a Domino konzol toorestart hello LDAP-szolgáltatás és tooReload hello LDAP-sémák:
   * Domino-konzolon hello parancs alapján beszúrása **Domino parancs** bejegyezve toorestart hello LDAP szolgáltatás - szöveg [indítsa újra a feladat LDAP](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html).
   * tooreload LDAP séma paranccsal kérje meg az LDAP - mondja el LDAP ReloadSchema
5. Nyissa meg az Domino Admin és személyek és csoportok lapon toosee válassza hozzá attribútum domino Hozzáadás személy is megjelenik.
6. Nyissa meg a Schema.nsf **fájlok** lapra, és tekintse meg a hozzáadott attribútum dominoPerson LDAP objektumosztály van megjelennek.

**2. módszer: Hozzon létre egy auxClass egyéni attribútum és hello objektumosztály társítása**

1. Hozzon létre egy másolatot Domino Directory sablon {PUBNAMES. NTF} következő [ezeket a lépéseket](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) (soha nem a hello alapértelmezett IBM Lotus Domino directory sablon testreszabása):
2. Nyissa meg hello másolása a Domino directory sablon {CONTOSO. NTF} létrehozott sablont, Domino-tervezőben.
3. Hello bal oldali ablaktáblában jelölje ki a megosztott kódot, majd a segédűrlapok.
4. Kattintson az új segédűrlap
5. Hello toospecify hello tulajdonságok hello új segédűrlap követően:
   * Hello új segédűrlap nyitva, és válassza ki a Tervező - segédűrlap tulajdonságai
   * Következő toohello Name tulajdonsággal, adja meg a hello kiegészítő objektumosztály – például TestSubform nevét.
   * Tartsa hello beállítások tulajdonság "Felvétele a Beszúrás segédűrlap... párbeszédpanel" beállítás
   * Törölje a hello beállítások tulajdonság "Leképezési továbbítása HTML megjegyzésekben."
   * Hagyja hello egyéb tulajdonságok hello azonos, és hello segédűrlap tulajdonságainak bezárásához.
   * Mentse és zárja be az új segédűrlap hello.
6. Hello követően egy mező toodefine hello kiegészítő objektumosztály tooadd:
   * Nyissa meg a létrehozott hello segédűrlap.
   * Válasszon létrehozása – mezőben.
   * Következő tooName hello mező párbeszédpanel hello alapvető beállítások lapon adja meg az tetszőleges nevet, például: {MyPersonTestAttribute}.
   * Hello hozzáadott mezőben az típusának, stílusának, méret, betűtípus és kapcsolódó tulajdonságok kiválasztásával tulajdonságainak beállítása.
   * Tartsa hello attribútum alapértelmezett értéke megegyezik az objektumhoz megadott hello neveként (például MyPersonTestAttribute attribútum megadása esetén tartsa hello alapértelmezett értéket a hello ugyanazt a nevet).
   * Hello segédűrlap mentse a frissített értékekkel, és a következő hello:
     * Hello bal oldali ablaktáblában jelölje ki a megosztott kódot, majd a segédűrlapok
     * Válassza ki az új segédűrlap hello, és válassza a Tervező - Tervező tulajdonságai.
     * Hello harmadik hello balról majd lapon válassza ki **Ez a Tiltás tervezési változás propagálása**.
7. Nyissa meg a {ObjectName} $ExtensibleSchema segédűrlap (ahol {ObjectName} annak hello alapértelmezett strukturális object osztályt, például – személy hello neve).
8. Erőforrás beszúrása és válassza ki a hello segédűrlap (létrehozott, például – TestSubform), és mentse a hello ${ObjectName} ExtensibleSchema segédűrlap.
9. Cserélje le a hello Domino Directory sablon {PUBNAMES. NTF} hello új egyéni sablon {CONTOSO. NTF} következő [ezeket a lépéseket](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
10. Zárja be a Domino rendszergazda, és nyissa meg a Domino konzol toorestart hello LDAP-szolgáltatás és tooReload hello LDAP-sémák:
    * Domino-konzolon hello parancs alapján beszúrása **Domino parancs** bejegyezve toorestart hello LDAP szolgáltatás - szöveg [indítsa újra a feladat LDAP](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html).
    * Mondja el LDAP paranccsal tooreload LDAP séma **mondja el LDAP ReloadSchema**.
11. Nyissa meg az Domino Admin és személyek és csoportok lapon toosee válassza hozzá attribútum domino Hozzáadás személy is megjelenik (a többi a lap).
12. Nyissa meg a Schema.nsf **fájlok** lapra, és tekintse meg a hozzáadott attribútum TestSubform LDAP kiegészítő objektumosztály alatt is megjelenik.

**3. módszer: Hello egyéni attribútum toohello ExtensibleObject osztály hozzáadása**

1. Hello gyökérkönyvtár helyezve {Schema.nsf} fájl megnyitása
2. Válassza ki az LDAP objektum osztályokat hello bal oldali menü alatti **összes Sémadokumentumokban** kattintson **objektum hozzáadása az osztály** gomb:
3. LDAP nevezze formájában hello {zzzExtensibleSchema} (ahol a zzz azt hello alapértelmezett strukturális objektum osztály, például személy hello neve). Tooextend hello séma személy objektumosztály, adja meg például a következő: {PersonExtensibleSchema} LDAP nevét.
4. Adja meg a legyen tooextend hello séma felső objektum osztálynév. Tooextend hello séma személy objektumosztály, például fölérendelt objektum osztálynév {dominoPerson} adja meg:
5. Adjon meg egy érvényes OID megfelelő toohello objektumosztály.
6. Válassza ki a kötelező vagy választható attribútumtípust mezők szerint hello követelmény kiterjesztett/egyéni attribútumok:
7. Miután hozzáadása a kötelező attribútumok toohello ExtensibleObjectClass, kattintson **mentés és Bezárás**.
8. Egy ExtensibleObjectClass megfelelő alapértelmezett objektumosztály kiterjesztett attribútummal rendelkező jön létre.

## <a name="troubleshooting"></a>Hibaelhárítás
* Hogyan tooenable naplózási tootroubleshoot hello összekötő információkért lásd: hello [hogyan ETW-nyomkövetés összekötők tooEnable](http://go.microsoft.com/fwlink/?LinkId=335731).
