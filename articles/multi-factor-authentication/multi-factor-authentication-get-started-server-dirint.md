---
title: "aaaDirectory integráció az Azure többtényezős hitelesítés és az Active Directory között"
description: "Ez a hello Azure többtényezős hitelesítés lap, amely leírja, hogyan toointegrate hello-e Azure multi-factor Authentication kiszolgáló és az Active Directory, szinkronizálhatja a hello könyvtárak."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: def7a534-cfb2-492a-9124-87fb1148ab1f
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/16/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: fbff518b4641010d5f7745096e0ff658864d0805
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="directory-integration-between-azure-mfa-server-and-active-directory"></a>Címtár-integráció az Azure MFA-kiszolgáló és az Active Directory között
Hello címtár-integrációs szakaszában hello Azure MFA kiszolgáló toointegrate használja az Active Directoryval vagy LDAP-címtárhoz. Attribútumok toomatch hello directory-séma konfigurálása, és állítsa be az automatikus felhasználó szinkronizálást.

## <a name="settings"></a>Beállítások
Alapértelmezés szerint hello Azure multi-factor Authentication (MFA) kiszolgáló konfigurált tooimport vagy felhasználók Active Directory szinkronizálását.  hello címtár-integráció lap lehetővé teszi, hogy Ön toooverride hello alapértelmezett viselkedést és toobind tooa másik LDAP-címtárhoz, ADAM-címtárhoz vagy megadott Active Directory-tartományvezérlőhöz.  Emellett biztosítja az LDAP-hitelesítés tooproxy LDAP hello használatát vagy az LDAP-kötés RADIUS-célként, az IIS-hitelesítést, vagy a felhasználói portál elsődleges hitelesítéseként történő előhitelesítési.  a következő táblázat hello hello egyedi beállításokat ismerteti.

![Beállítások](./media/multi-factor-authentication-get-started-server-dirint/dirint.png)

| Szolgáltatás | Leírás |
| --- | --- |
| Active Directory használata |Válassza ki a hello Active Directory használata lehetőséget toouse Active Directory az importáláshoz és szinkronizáláshoz.  Ez az hello alapértelmezett beállítása. <br>Megjegyzés: Az Active Directory integráció toowork megfelelően, csatlakozás hello tooa tartomány és jelentkezzen be egy olyan tartományi fiók. |
| Megbízható tartományok belefoglalása |Ellenőrizze **megbízható tartományok közé tartoznak** toohave hello ügynök kísérlet tooconnect toodomains megbízhatónak hello az aktuális tartomány, egy másik hello erdő vagy erdőszintű megbízhatóság részt tartományok.  Ha nem importálja, de bármelyik hello felhasználók szinkronizálása megbízható tartományokhoz, törölje a jelet hello jelölőnégyzet tooimprove teljesítményét.  hello alapértelmezés szerint be van jelölve. |
| Meghatározott LDAP-konfiguráció használata |Válassza ki a hello LDAP használata beállítást toouse hello LDAP-beállítások az importáláshoz és szinkronizáláshoz megadott. Megjegyzés: Ha az LDAP használata lehetőség be van jelölve, hello felhasználói felület hivatkozásai az Active Directory tooLDAP. |
| Szerkesztés gomb |hello Szerkesztés gomb lehetővé teszi, hogy a hello aktuális LDAP konfigurációs beállítások toomodified. |
| Attribútumhatókör-lekérdezések használata |Azt jelzi, hogy a rendszer használ-e attribútumhatókör-lekérdezéseket.  Attribútum hatókörű lekérdezések lehetővé teszik a jogosult hello tételek egy másik rekord attribútumaiban szereplő bejegyzések hatékony directory keresésekhez.  hello Azure multi-factor Authentication kiszolgálót használja attribútum hatókör lekérdezések tooefficiently lekérdezés hello felhasználók biztonsági csoport tagja.   <br>Megjegyzés: Bizonyos esetekben az attribútumhatókör-lekérdezés támogatott, de nem érdemes használni.  Például az Active Directory problémákba ütközhet az attribútumhatókör-lekérdezéskor, ha egy biztonsági csoport tagjai több tartományhoz tartoznak. Ebben az esetben törölje a hello jelölőnégyzet kijelölését. |

a következő táblázat hello hello LDAP konfigurációs beállításokat ismerteti.

| Szolgáltatás | Leírás |
| --- | --- |
| Kiszolgáló |Adja meg a hello állomásnév vagy IP-címet futtató hello LDAP-címtár hello kiszolgáló.  Tartalékkiszolgálót is megadhat pontosvesszővel elválasztva. <br>Megjegyzés: Ha a kötés SSL típusú, teljes állomásnévre van szükség. |
| Alap DN |Adja meg hello alap könyvtárobjektum, amelyből az összes directory lekérdezés start hello megkülönböztető nevét.  Például dc=abc,dc=com. |
| Kötéstípus – Lekérdezések |Válassza ki a hello megfelelő kötési típust toosearch hello LDAP-címtárhoz kötéskor.  Ezt a rendszer az importálásokhoz, a szinkronizáláshoz és a felhasználónevek feloldásához használja. <br><br>  Névtelen – A rendszer névtelen kötést hajt végre.  Nem használ kötési DN-t és kötésjelszót.  Ez csak akkor működik, ha hello LDAP-címtár engedélyezi a névtelen kötést, és engedélyei lehetővé hello hello megfelelő rekordok és attribútumok lekérdezését.  <br><br> Egyszerű – a kötési megkülönböztető Nevet és a kötési jelszót átadása pedig egyszerű szöveges toobind toohello LDAP-címtárhoz.  Ez tesztelési célokra, tooverify, amely hello kiszolgáló elérhető és, hogy hello bind fióknak megfelelő hozzáférése hello. Hello megfelelő tanúsítvány telepítése után SSL használatára.  <br><br> SSL - kötési megkülönböztető név és kötési jelszó titkosítása SSL toobind toohello LDAP-címtár használatával.  Telepítse a tanúsítványt helyileg, amely az LDAP-címtár Megbízhatóságok hello.  <br><br> Windows - kötési felhasználónév és kötési jelszó vannak használt toosecurely csatlakozás tooan Active Directory-tartományvezérlőhöz vagy ADAM-címtárhoz.  Ha a kötési felhasználónevet nem üres, a hello bejelentkezett felhasználói fiók használt toobind. |
| Kötéstípus – Hitelesítések |Jelölje be hello használja, ha az LDAP-kötési hitelesítés végrehajtásához megfelelő kötési típust.  Lásd: hello kötési típus leírását a kötési típusok – lekérdezések.  Például ez lehetővé teszi a névtelen kötés toobe használatát a lekérdezésekhez, míg az SSL-kötés az LDAP-kötési hitelesítéseket használt toosecure. |
| Kötési DN vagy kötési felhasználónév |Adjon meg hello megkülönböztető nevet hello felhasználói rekord hello fiók toouse toohello LDAP-címtárhoz kötéskor.<br><br>hello kötési megkülönböztető nevet csak akkor használható, ha kötés típusa egyszerű vagy SSL.  <br><br>Adja meg a Windows-fiók toouse hello hello felhasználóneve toohello LDAP-címtárhoz kötéskor, ha a Kötéstípushoz a Windows.  Ha üresen marad, a hello bejelentkezett felhasználó fiókjának használt toobind. |
| Kötésjelszó |Adja meg a hello kötési jelszót a hello kötési megkülönböztető Nevet vagy a használt toobind toohello LDAP-címtár folyamatban felhasználónév.  tooconfigure hello jelszó hello multi-factor Authentication kiszolgáló AdSync szolgáltatás, a szinkronizálás engedélyezése, és győződjön meg arról, hogy fut-e hello szolgáltatás hello helyi számítógépen.  hello jelszó mentése hello Windows tárolt felhasználónevek és jelszavak hello fiók hello multi-factor Authentication kiszolgáló AdSync szolgáltatás futtatásához használt alatt.  hello jelszó is menti a hello fiók hello multi-factor Auth kiszolgáló felhasználói felületét futtató, és a hello fiók hello multi-factor Authentication kiszolgáló szolgáltatás futtatásához használt.  <br><br>Mivel hello jelszót a rendszer csak hello helyi kiszolgáló Windows tárolt felhasználónevei és jelszavai, ismételje meg ezt a lépést minden multi-factor Auth kiszolgálón, amelyet a hozzáférés toohello jelszó. |
| Lekérdezés méretkorlátja |Adja meg a címtárbeli keresés visszaadó felhasználók maximális száma hello hello méretkorlátot.  Ezt a határt, meg kell felelnie hello hello LDAP-címtár konfigurációjához.  Nagy keresésekhez, ahol nem támogatott a lapozás importálása és szinkronizálás megpróbál tooretrieve felhasználókat kötegekben.  Ha hello méretkorlátot itt megadott nagyobb megadott hello LDAP-címtár hello korlátot, egyes felhasználók kimaradhatnak. |
| Teszt gomb |Kattintson a **teszt** tootest kötés toohello LDAP-kiszolgálóval.  <br><br>Nem kell tooselect hello **LDAP használata lehetőség** tootest kötés lehetőséget. Ez lehetővé teszi a hello kötés toobe tesztelt hello LDAP-konfiguráció használata előtt. |

## <a name="filters"></a>Szűrők
Szűrők lehetővé teszik tooset feltételek tooqualify rekordok címtárbeli keresés végrehajtása során.  Hello szűrő beállítása hatókör hello objektumokat szeretné toosynchronize.  

![Szűrők](./media/multi-factor-authentication-get-started-server-dirint/dirint2.png)

Az Azure multi-factor Authentication rendelkezik a következő három szűrőbeállítások hello:

* **Tároló szűrő** -hello szűrési feltételeket használandó tooqualify tárolórekordok címtárbeli keresés végrehajtása során.  Active Directory és ADAM esetén általában a következő feltétel használatos: (|(objectClass=organizationalUnit)(objectClass=container)).  Más LDAP-címtárak esetén használja az adott címtársémának megfelelően minden objektum, attól függően, hogy hello címtárséma szűrési feltételeket.  <br>Megjegyzés: Ha üresen hagyja, a rendszer alapértelmezés szerint a következő feltételt használja: ((objectClass=organizationalUnit)(objectClass=container)).
* **Biztonságicsoport-szűrőt** -hello szűrési feltételeket használandó tooqualify biztonságicsoport-rekordok címtárbeli keresés végrehajtása során.  Active Directory és ADAM esetén általában a következő feltétel használatos: (&(objectCategory=group)(groupType:1.2.840.113556.1.4.804:=-2147483648)).  Más LDAP-címtárak esetén használja az adott címtársémának megfelelően minden egyes biztonsági csoport objektumba, attól függően, hogy hello címtárséma szűrési feltételeket.  <br>Megjegyzés: Ha üresen hagyja, a rendszer alapértelmezés szerint a következő feltételt használja: (&(objectCategory=group)(groupType:1.2.840.113556.1.4.804:=-2147483648)).
* **Felhasználó szűrő** -hello szűrési feltételeket használandó tooqualify felhasználói rekordok címtárbeli keresés végrehajtása során.  Active Directory és ADAM esetén általában a következő feltétel használatos: (&(objectClass=user)(objectCategory=person)).  Más LDAP-címtárak esetén használja (objectClass = inetOrgPerson) vagy valami hasonló, attól függően, hogy hello directory-séma. <br>Megjegyzés: Ha üresen hagyja, a rendszer alapértelmezés szerint a következő feltételt használja: (&(objectCategory=person)(objectClass=user)).

## <a name="attributes"></a>Attribútumok
Az attribútumok igény szerint testreszabhatók egy adott címtárhoz.  Ez lehetővé teszi az egyéni attribútumok tooadd és finomhangolásához hello szinkronizálási tooonly hello attribútumok, amelyekre szüksége van. Hello attricute hello nevét használja, az egyes attribútummezők értékének hello hello directory-sémát. hello következő táblázat további információt az egyes szolgáltatásokhoz.

Attribútumok megadhatók manuálisan, és nincsenek szükséges toomatch attribútum hello attribútum listájában.

![Attribútumok](./media/multi-factor-authentication-get-started-server-dirint/dirint3.png)

| Szolgáltatás | Leírás |
| --- | --- |
| Egyedi azonosító |Adja meg, amely a tároló, a biztonsági csoport és a felhasználói rekordok hello egyedi azonosítóként szolgál hello attribútum hello attribútum nevét.  Az Active Directoryban ez általában az objectGUID. Egyéb LDAP-megvalósításokban az entryUUID vagy valami ehhez hasonló is előfordulhat.  hello alapértelmezett az objectGUID. |
| Egyedi azonosító típusa |Válassza ki a hello típusú hello egyedi azonosító attribútum.  Az Active Directory hello objectGUID attribútum GUID típusú. Egyéb LDAP-megvalósításokban az ASCII bájttömb vagy Karakterlánc típusok is előfordulhatnak.  hello alapértelmezett érték a GUID. <br><br>Ez a típus megfelelően a szinkronizálási elemek óta az egyedi azonosító által hivatkozott fontos tooset. hello egyedi azonosító típusa használt toodirectly keresés hello objektum hello könyvtárban.  A típus tooString beállítást, amikor hello directory ténylegesen tárolja hello érték egy ASCII-karakterekből álló bájttömbben megakadályozza, hogy a szinkronizálás alapjául szolgáló megfelelően működik-e. |
| Megkülönböztető név |Adja meg hello hello attribútum hello megkülönböztető nevet az egyes rekordokhoz tartalmazó attribútum nevét.  Active Directoryban ez általában a distinguishedName. Egyéb LDAP-megvalósításokban az entryDN vagy valami ehhez hasonló is előfordulhat.  hello alapértelmezett érték a distinguishedName. <br><br>Ha csak hello megkülönböztető nevet tartalmazó attribútum nem létezik, hello adspath attribútum is használható.  hello "LDAP: / /\<server\>/" hello elérési út része az automatikusan levágja, csak hello objektumának megkülönböztető nevét, hello hagyja. |
| Tárolónév |Adja meg hello hello attribútum hello nevet egy tárolórekordban tartalmazó attribútum nevét.  Ennek az attribútumnak hello értéke megjelenik hello Tárolóhierarchiában az Active Directoryból történő importálás során vagy a szinkronizálási elemek hozzáadásakor.  hello alapértelmezett érték a name. <br><br>Ha különböző tárolók különböző attribútumokat használnak nevükhöz, pontosvesszővel kell elválasztani tooseparate használja több Tárolónév attribútum.  hello első Tárolónév attribútumot a tárolóobjektumban talált használt toodisplay van a neve. |
| Biztonsági csoport neve |Adja meg hello hello attribútum hello neve a biztonsági csoport rekordjában tartalmazó attribútum nevét.  Ez az attribútum értékének hello hello biztonsági csoport listában akkor látható, ha az Active Directoryból történő importálás vagy a szinkronizálási elemek hozzáadásakor.  hello alapértelmezett érték a name. |
| Felhasználónév |Adja meg a hello hello attribútum hello felhasználónevet felhasználói rekordban tartalmazó attribútum nevét.  Ez az attribútum értékének hello hello multi-factor Authentication kiszolgáló felhasználónév szolgál.  Egy második attribútum is adhatók meg a biztonsági mentési toohello először.  hello második attribútumot csak akkor használja, ha hello első attribútum nem tartalmaz értéket hello felhasználóhoz.  hello azok userPrincipalName és a sAMAccountName. |
| Utónév |Adja meg hello hello attribútum hello keresztnevet felhasználói rekordban tartalmazó attribútum nevét.  hello alapértelmezett érték a givenName. |
| Vezetéknév |Adja meg hello hello attribútum hello vezetéknevet felhasználói rekordban tartalmazó attribútum nevét.  hello alapértelmezett érték az sn. |
| E-mail-cím |Adja meg a hello hello attribútum hello e-mail címet felhasználói rekordban tartalmazó attribútum nevét.  Az e-mail cím használt toosend üdvözlő és frissítés toohello felhasználói e-maileket.  hello alapértelmezett érték a mail. |
| Felhasználói csoport |Adja meg hello hello attribútum hello felhasználói csoportot felhasználói rekordban tartalmazó attribútum nevét.  Felhasználói csoport lehet használt toofilter felhasználók hello ügynök, valamint hello multi-factor Auth Server kezelési portál jelentéseiben. |
| Leírás |Adja meg hello hello attribútum hello leírást felhasználói rekordban tartalmazó attribútum nevét.  A leírás csak keresésekhez használható.  hello alapértelmezett érték a description. |
| Telefonhívás nyelve |Adja meg, amely hello rövid nevét tartalmazza hello nyelvi toouse beszélgetésre hello felhasználó hello attribútum hello attribútum nevét. |
| Szöveges üzenetek nyelve |Adja meg, amely hello rövid nevét tartalmazza hello nyelvi toouse hello felhasználó szöveges üzeneteiben SMS hello attribútum hello attribútum nevét. |
| Mobilalkalmazások nyelve |Adja meg, amely hello rövid nevét tartalmazza hello nyelvi toouse telefonos alkalmazás szöveges üzeneteiben hello felhasználói hello attribútum hello attribútum nevét. |
| OATH token nyelve |Adja meg, amely hello rövid nevét tartalmazza hello nyelvi toouse az OATH jogkivonat szöveges üzeneteinek hello felhasználó hello attribútum hello attribútum nevét. |
| Céges telefon |Adja meg hello hello attribútum hello munkahelyi telefonszámot felhasználói rekordban tartalmazó attribútum nevét.  hello alapértelmezett érték a telephoneNumber. |
| Otthoni telefon |Adja meg hello hello attribútum hello otthoni telefonszámot felhasználói rekordban tartalmazó attribútum nevét.  hello alapértelmezett érték a homePhone. |
| Személyhívó |Adja meg hello hello attribútum hello személyhívó számát felhasználói rekordban tartalmazó attribútum nevét.  hello alapértelmezett érték a pager. |
| Mobiltelefon |Adja meg a hello hello attribútum hello mobiltelefonszámot felhasználói rekordban tartalmazó attribútum nevét.  hello alapértelmezett érték a mobile. |
| Fax |Adja meg hello hello attribútum hello faxszámot felhasználói rekordban tartalmazó attribútum nevét.  hello alapértelmezett érték a facsimileTelephoneNumber. |
| IP-telefon |Adja meg hello hello attribútum hello IP-telefon számát felhasználói rekordban tartalmazó attribútum nevét.  hello alapértelmezett érték az ipPhone. |
| Egyéni |Adja meg egy egyéni telefonszámot felhasználói rekordban tartalmazó attribútum hello hello attribútum nevét.  hello alapértelmezett érték üres. |
| Mellék |Adja meg hello hello attribútum hello mellékszáma felhasználói rekordban tartalmazó attribútum nevét.  hello mellék mező értékét hello hello bővítmény toohello csak elsődleges telefonszám szolgál.  hello alapértelmezett érték üres. <br><br>Ha hello mellék attribútum nincs megadva, bővítmények hello telefonszám attribútum része lehet. Ebben az esetben előtt hello kiterjesztés "x", így lekérdezi megfelelően értelmezni.  Például 555-123-4567 x890 eredményeznének 555-123-4567 hello telefonszám, pedig a 890 lesz hello bővítmény. |
| Alapértelmezések visszaállítása gomb |Kattintson a **alapértelmezett értékek visszaállítása** tooreturn attribútumainak biztonsági tootheir alapértelmezett értéket.  hello alapértelmezett értékek általában megfelelően működnek hello normál Active Directory vagy az ADAM-sémával. |

tooedit attribútumok, kattintson a **szerkesztése** hello attribútumok lapon.  Ekkor megjelenik egy ablak, ahol szerkesztheti a hello attribútumok. Jelölje be hello **...**  következő tooany attribútum tooopen egy ablak, ahol kiválaszthatja, melyik attribútumok toodisplay.

![Attribútumok szerkesztése](./media/multi-factor-authentication-get-started-server-dirint/dirint4.png)

## <a name="synchronization"></a>Szinkronizálás
Szinkronizálás tartja hello Azure MFA felhasználói adatbázis szinkronizálva az Active Directory vagy egy másik Lightweight Directory Access Protocol (LDAP) directory hello felhasználóinak. hello folyamat hasonló tooimporting felhasználók manuálisan az Active Directory rendszerből, de rendszeres időközönként lekérdezi az Active Directory-felhasználók és biztonsági csoport módosításainak tooprocess.  Emellett letiltja vagy eltávolítja azon felhasználókat, amelyek el lettek távolítva egy tárolóból, biztonsági csoportból vagy az Active Directoryból.

a multi-factor Authentication ADSync szolgáltatás hello egy Windows-szolgáltatás, amely elvégzi az Active Directory lekérdezésével rendszeres hello.  Ez az Azure AD Sync vagy az Azure AD Connect összetéveszthető toobe.  hello multi-factor Authentication ADSync, bár egy hasonló kódbázisra épül az adott toohello Azure multi-factor Authentication kiszolgáló.  Leállított állapotban van telepítve, és indítja el a multi-factor Authentication kiszolgáló szolgáltatás konfigurálásakor hello toorun.  Ha többkiszolgálós multi-factor Authentication kiszolgáló konfigurációval rendelkezik, a multi-factor Authentication ADSync hello csak futtatható egyetlen kiszolgálón.

a multi-factor Authentication ADSync szolgáltatás hello hello módosítások Microsoft tooefficiently lekérdezési által biztosított DirSync LDAP-kiszolgálóbővítményt használja.  A DirSync vezérléshívónak hello "directory beolvasása módosítások" jobb és a DS-Replication-Get-Changes kiterjesztett vezérlési hozzáférési joggal kell rendelkeznie.  Alapértelmezés szerint ezek a jogosultságok toohello rendszergazda és a LocalSystem fiókhoz tartományvezérlőkön vannak hozzárendelve.  a multi-factor Authentication AdSync szolgáltatás hello konfigurált toorun LocalSystem alapértelmezés szerint ki.  Ezért a legegyszerűbb toorun hello szolgáltatás egy tartományvezérlőn.  Ha a teljes szinkronizálást végezzen hello szolgáltatás tooalways, kevesebb jogosultsággal való futtatás fiókként.  Ez kevésbé hatékony megoldás, de kevesebb fiókjogosultságot igényel.

Ha hello LDAP-címtár támogatja, és a DirSync konfigurálva, majd lekérdezési a felhasználó és biztonsági csoportok változásainak fog működni hello ugyanaz, mint az Active Directory.  Ha hello LDAP-címtár nem támogatja a DirSync vezérlőt hello, majd a teljes szinkronizálás történik minden ciklusban.

![Szinkronizálás](./media/multi-factor-authentication-get-started-server-dirint/dirint5.png)

hello következő táblázat további információkat tartalmaz az egyes hello szinkronizálás lap beállításai.

| Szolgáltatás | Leírás |
| --- | --- |
| Active Directory-szinkronizálás engedélyezése |Ha be van jelölve, hello multi-factor Authentication kiszolgáló szolgáltatás rendszeresen lekérdezi az Active Directory változásait. <br><br>Megjegyzés: legalább egy szinkronizálási elemet hozzá kell adni, és a egy szinkronizálás most kell végrehajtani, mielőtt hello multi-factor Authentication kiszolgáló szolgáltatás elkezdi feldolgozni a módosításokat. |
| Szinkronizálás gyakorisága |Adja meg a hello idő időköz hello multi-factor Authentication kiszolgáló szolgáltatásnak várnia kell a lekérdezés és a módosítások feldolgozása között. <br><br> Megjegyzés: hello megadott időköz hello az egyes ciklusok kezdete közötti hello idő.  Ha a hello idő feldolgozási módosítások hello időközt ennél nagyobb, hello szolgáltatást fog azonnal új lekérdezést végez. |
| Az Active Directoryban már nem szereplő felhasználók eltávolítása |Ha be van jelölve, a multi-factor Authentication kiszolgáló szolgáltatás hello dolgozza fel az Active Directory törlésjelzőit, és távolítsa el hello kapcsolatos multi-factor Authentication kiszolgáló felhasználói. |
| Mindig végezzen teljes szinkronizálást |Ha be van jelölve, a multi-factor Authentication kiszolgáló szolgáltatás hello mindig elvégzik a teljes szinkronizálás.  Ha nincs bejelölve, hello multi-factor Authentication kiszolgáló szolgáltatás növekményes szinkronizálást végez, amely, a felhasználók csak lekérdezésével.  hello alapértelmezés szerint nincs bejelölve. <br><br>Ha nincs bejelölve, Azure MFA kiszolgáló a csak akkor, ha hello címtár támogatja hello DirSync vezérlőt, és hello fiók kötés toohello directory engedélyek tooperform DirSync növekményes lekérdezéseinek növekményes szinkronizálást hajt végre.  Ha hello fiók nem rendelkezik megfelelő engedélyekkel hello, vagy több tartomány is érintett a szinkronizálásban hello, az Azure MFA kiszolgáló teljes szinkronizálást hajt végre. |
| Rendszergazdai jóváhagyás megkövetelése több mint X felhasználó letiltása vagy eltávolítása esetén |Szinkronizálási elemek konfigurált toodisable vagy távolítsa el a felhasználókat, akik már nem hello elem tárolóból vagy biztonsági csoportjának tagjává teszi.  A biztonságos működés érdekében, rendszergazdai jóváhagyás kérhető, ha a felhasználók toodisable, vagy távolítsa el hello száma meghaladja a küszöbértéket.  Ha be van jelölve, az adott küszöbérték meghaladása esetén jóváhagyásra van szükség.  hello alapértelmezés szerint 5 és hello értéktartomány: 1 too999. <br><br> Jóváhagyást a rendszer az e-mail értesítési tooadministrators elküldésével. hello e-mail értesítés áttekintését és jóváhagyását, a felhasználók hello letiltásának vagy eltávolításának ismerteti.  Hello multi-factor Authentication kiszolgáló felhasználói felületének indításakor, akkor arra fogja kérni a jóváhagyásra. |

Hello **szinkronizálás most** gomb lehetővé teszi a teljes szinkronizálás hello szinkronizálás toorun megadott elemek.  Teljes szinkronizálás szükséges szinkronizált elemek hozzáadásakor, módosításakor, eltávolításakor vagy átrendezésekor.  Célszerű is szükséges előtt hello multi-factor Authentication AdSync szolgáltatás is működési, mivel a kiindulási pontjaként, amelyből hello szolgáltatás lekérdezi a növekményes módosításokat hello állítja be.  Ha végzett módosítások toosynchronization elemeket, de a teljes szinkronizálás még nem lett végrehajtva, most fogja felszólító tooSynchronize.

Hello **eltávolítása** gomb lehetővé teszi hello rendszergazda toodelete egy vagy több szinkronizálási elem hello multi-factor Authentication kiszolgáló szinkronizálási elemek listájából.

> [!WARNING]
> Ha egy szinkronizált elem rekordját eltávolítja, az nem állítható vissza. Szüksége lesz tooadd hello szinkronizálási elem rekordot újra Ha véletlenül törli.

hello szinkronizálási elem vagy a szinkronizálási elemeket a multi-factor Authentication kiszolgálóról eltávolították.  hello multi-factor Authentication kiszolgáló szolgáltatás már nem fogja feldolgozni hello szinkronizálási elemeket.

hello fel és le gombokkal hello rendszergazda hello szinkronizálási elemek sorrendjének toochange hello.  hello sorrendje is fontos, mivel hello adott felhasználó is lehet (pl. egy tároló és a biztonsági csoport) egynél több szinkronizálási elem tagja.  hello beállítások hello lista toowhich hello felhasználói hello első szinkronizálási eleme határozza meg a szinkronizálás során alkalmazott toohello felhasználó hozzá rendelve.  Ezért a szinkronizálási elemek hello kell rendezni prioritási sorrendben.

> [!TIP]
> Szinkronizált elemek eltávolítása után teljes szinkronizálást kell végrehajtani.  Szinkronizált elemek átrendezése után teljes szinkronizálást kell végrehajtani.  Kattintson a **szinkronizálás most** tooperform teljes szinkronizálást.

## <a name="multi-factor-auth-servers"></a>Multi-Factor Auth-kiszolgálók
További multi-factor Authentication kiszolgálók is beállíthatók meg tooserve, biztonsági mentési RADIUS-proxyként, LDAP-proxy vagy az IIS-hitelesítés. hello szinkronizálási konfiguráció összes hello ügynököt osztozik. Azonban csak az egyik ilyen ügynökök előfordulhat, hogy hello multi-factor Auth Server szolgáltatást futtató. Ezen a lapon adhatók meg tooselect hello multi-factor Auth kiszolgáló, amely a szinkronizálást engedélyezni kell.

![Multi-Factor-Auth-kiszolgálók](./media/multi-factor-authentication-get-started-server-dirint/dirint6.png)
