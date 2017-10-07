---
title: "LDAP-összekötő aaaGeneric |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooconfigure Microsoft általános LDAP-összekötő."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 984beeb0-4d91-4908-ad81-c19797c4891b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 25031b4da196bd073902b04b0705762bfa0118b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="generic-ldap-connector-technical-reference"></a>Általános LDAP-összekötő műszaki útmutatója
Ez a cikk ismerteti a hello általános LDAP-összekötő. hello cikk vonatkozik toohello a következő termékek:

* A Microsoft Identity Manager 2016 (MIM2016)
* A Forefront Identity Manager 2010 R2 (FIM2010R2)
  * Kell használnia a 4.1.3671.0 gyorsjavítás vagy újabb [KB3092178](https://support.microsoft.com/kb/3092178).

MIM2016 és FIM2010R2, hello összekötő rendelkezésre áll hello letölthető [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495).

Hivatkozni tooIETF RFC-dokumentumokat, ez a dokumentum használ hello formátuma (RFC [RFC szám] / [section RFC dokumentumban]), például (RFC 4512/4.3).
A http://tools.ietf.org/html/rfc4500 (a szükséges tooreplace 4500 hello helyes RFC szám) talál további információkat.

## <a name="overview-of-hello-generic-ldap-connector"></a>Általános LDAP-összekötő hello áttekintése
hello általános LDAP-összekötő lehetővé teszi, hogy Ön toointegrate hello szinkronizálási szolgáltatás az LDAP v3-kiszolgálóval.

Bizonyos műveleteket és a séma elemei, például a szükséges tooperform különbözeti importálás, nincsenek megadva hello IETF RFC. Ezeket a műveleteket csak explicit módon megadott LDAP-címtárak esetén támogatottak.

Magas szintű szempontjából a következő funkciók hello hello hello összekötő aktuális kiadása támogatja:

| Szolgáltatás | Támogatás |
| --- | --- |
| Csatlakoztatott adatforrás |hello összekötő minden LDAP v3-as kiszolgáló (RFC 4510 szabványnak megfelelő) használata támogatott. Hello következő tesztelték: <li>Microsoft Active Directory Lightweight Directory-szolgáltatások (AD LDS)</li><li>Microsoft Active Directory globális katalógus (GC-AD)</li><li>A címtárkiszolgáló 389</li><li>Apache címtárkiszolgálóra</li><li>IBM Tivoli DS</li><li>Isode könyvtár</li><li>NetIQ eDirectory</li><li>Novell eDirectory</li><li>Nyissa meg DJ</li><li>Nyissa meg DS</li><li>Nyissa meg az LDAP (openldap.org)</li><li>Oracle (korábban Sun) Directory Server Enterprise Edition</li><li>RadiantOne virtuális könyvtár Server (VDS)</li><li>Egy Sun Directory Server</li>**Nem támogatott a figyelmet a jelentősebb könyvtárak:** <li>Microsoft Active Directory tartományi szolgáltatások (AD DS) [használata hello beépített Active Directory-összekötő helyette]</li><li>Oracle Internet könyvtár (OID)</li> |
| Forgatókönyvek |<li>Objektum életciklusának kezelésére</li><li>Csoportok kezelése</li><li>Jelszókezelés</li> |
| Műveletek |a következő műveletek hello támogatottak minden LDAP-könyvtárak: <li>Teljes importálás</li><li>Exportálás</li>a következő műveletek hello csak a meghatározott könyvtárakban támogatottak:<li>Különbözeti importálás</li><li>Jelszó beállítása, a jelszó módosítása</li> |
| Séma |<li>Séma észlelésekor hello LDAP séma (RFC3673 és RFC4512/4.2)</li><li>Támogatja a szerkezeti osztályok, aux osztályok és extensibleObject objektumosztály (RFC4512/4.3)</li> |

### <a name="delta-import-and-password-management-support"></a>Különbözeti importálás és a jelszó támogatása
Különbözeti importálás és a jelszófelügyeletet támogatott könyvtárak:

* Microsoft Active Directory Lightweight Directory-szolgáltatások (AD LDS)
  * Különbözeti importálás összes műveletet támogatja
  * Támogatja a jelszó beállítása
* Microsoft Active Directory globális katalógus (GC-AD)
  * Különbözeti importálás összes műveletet támogatja
  * Támogatja a jelszó beállítása
* A címtárkiszolgáló 389
  * Különbözeti importálás összes műveletet támogatja
  * Támogatja a állítsa be a jelszót és a jelszó módosítása
* Apache címtárkiszolgálóra
  * Nem támogatott a különbözeti importálás a könyvtár nem tartozik egy állandó Változásnapló
  * Támogatja a jelszó beállítása
* IBM Tivoli DS
  * Különbözeti importálás összes műveletet támogatja
  * Támogatja a állítsa be a jelszót és a jelszó módosítása
* Isode könyvtár
  * Különbözeti importálás összes műveletet támogatja
  * Támogatja a állítsa be a jelszót és a jelszó módosítása
* Novell eDirectory és NetIQ eDirectory
  * Különbözeti importálás hozzáadása, frissítése és átnevezési műveleteket
  * Nem támogatja a Delete művelet különbözeti importálás
  * Támogatja a állítsa be a jelszót és a jelszó módosítása
* Nyissa meg DJ
  * Különbözeti importálás összes műveletet támogatja
  * Támogatja a állítsa be a jelszót és a jelszó módosítása
* Nyissa meg DS
  * Különbözeti importálás összes műveletet támogatja
  * Támogatja a állítsa be a jelszót és a jelszó módosítása
* Nyissa meg az LDAP (openldap.org)
  * Különbözeti importálás összes műveletet támogatja
  * Támogatja a jelszó beállítása
  * Nem támogatja a jelszó módosítása
* Oracle (korábban Sun) Directory Server Enterprise Edition
  * Különbözeti importálás összes műveletet támogatja
  * Támogatja a állítsa be a jelszót és a jelszó módosítása
* RadiantOne virtuális könyvtár Server (VDS)
  * 7.1.1 verzióját kell használnia vagy újabb
  * Különbözeti importálás összes műveletet támogatja
  * Támogatja a állítsa be a jelszót és a jelszó módosítása
* Egy Sun Directory Server
  * Különbözeti importálás összes műveletet támogatja
  * Támogatja a állítsa be a jelszót és a jelszó módosítása

### <a name="prerequisites"></a>Előfeltételek
Összekötő hello használata előtt győződjön meg arról hello következő hello szinkronizálási kiszolgálón rendelkezik:

* Microsoft keretrendszer 4.5.2-es vagy újabb verzió

### <a name="detecting-hello-ldap-server"></a>Hello LDAP-kiszolgáló észlelése
hello összekötő különböző módszereket toodetect alapul, és hello LDAP-kiszolgáló azonosítására szolgál. hello összekötő által használt hello legfelső szintű DSE, a gyártó neve/verziója, és azt megvizsgálja a hello séma toofind egyedi objektumok és attribútumok az egyes LDAP-kiszolgálókba tooexist ismert. Ezeket az adatokat, ha található, a használt toopre-hello konfigurációs beállítások hello összekötő feltöltéséhez.

### <a name="connected-data-source-permissions"></a>Csatlakoztatott adatforrás-engedélyek
tooperform importálása és exportálása a műveletek olyan objektumokon, hello hello csatlakoztatott könyvtárban, hello összekötő fiókja megfelelő engedélyekkel kell rendelkeznie. hello összekötő engedélyek toobe képes tooexport írási és olvasási engedélyek toobe képes tooimport kell. Engedély konfigurációs hello felhasználói élmény mellett magát hello cél könyvtár belül történik.

### <a name="ports-and-protocols"></a>Portok és protokollok
hello összekötő hello konfigurációban, amely alapértelmezés szerint 389 LDAP, és a LDAPS 636 hello portszámot használja.

LDAPS SSL 3.0 és TLS kell használnia. Az SSL 2.0 nem támogatott, és nem lehet aktiválni.

### <a name="required-controls-and-features"></a>Szükséges vezérlők és a szolgáltatások
hello következő LDAP-vezérlők/szolgáltatások rendelkezésre kell állnia hello LDAP-kiszolgáló hello összekötő toowork megfelelően:  
`1.3.6.1.4.1.4203.1.5.3`Igaz/hamis szűrők

hello igaz/hamis szűrő gyakran jelentése nem támogatja az LDAP-könyvtárak, előfordulhat, hogy a hello **globális lap** alatt **kötelező funkciók nem található**. Használt toocreate **vagy** szűrők az LDAP-lekérdezések, például amikor több Objektumtípusok importálása. Ha egynél több objektumtípus importálásához az LDAP-kiszolgáló támogatja ezt a szolgáltatást.

Ha egy könyvtárat használhatja, ahol van egy egyedi azonosítóra hello horgonyzási hello következő is elérhetőnek kell lennie (további információkért lásd: hello [horgonyok konfigurálása](#configure-anchors) szakaszban):  
`1.3.6.1.4.1.4203.1.5.1`Az összes műveleti attribútumok

Ha hello directory mi egy hívás toohello címtárban fér több objektum van, majd ajánlott toouse lapozást. Lapozófájl toowork szükséges hello alábbi beállítások egyikét:

**1. lehetőség:**  
`1.2.840.113556.1.4.319`pagedResultsControl

**2. lehetőség:**  
`2.16.840.1.113730.3.4.9`VLVControl  
`1.2.840.113556.1.4.473`SortControl

Ha mindkét lehetőség engedélyezve vannak a hello összekötő-konfiguráció, pagedResultsControl szolgál.

`1.2.840.113556.1.4.417`ShowDeletedControl

ShowDeletedControl csak az hello USNChanged különbözeti importálás módszer toobe képes toosee törölt objektumok használják.

hello összekötő megkísérli toodetect hello beállítások jelen hello kiszolgálón. Hello-beállítások nem észlelhető, ha figyelmeztetés a globális lap hello hello összekötő tulajdonságainál. Nem minden LDAP kiszolgálókon található összes vezérlők/funkciókat támogatja, és akkor is, ha ez a figyelmeztetés megtalálható, hello összekötő működhetnek hibák nélkül.

### <a name="delta-import"></a>Különbözeti importálás
Különbözeti importálás csak akkor használható, ha egy támogatási könyvtárat már telepítve van. a következő módszerek hello jelenleg használatban van:

* LDAP Accesslog. Lásd: [http://www.openldap.org/doc/admin24/overlays.html#Access naplózás](http://www.openldap.org/doc/admin24/overlays.html#Access Logging)
* LDAP változásnaplója. Lásd: [http://tools.ietf.org/html/draft-good-ldap-changelog-04](http://tools.ietf.org/html/draft-good-ldap-changelog-04)
* Timestamp típusú. Novell/NetIQ eDirectory hello összekötő által használt utolsó dátum/idő tooget létrehozott, és objektumokat frissíteni. Novell/NetIQ eDirectory nem biztosít egy azzal egyenértékű azt jelenti, hogy a törölt tooretrieve objektumok. Ezt a lehetőséget is használható, ha nincs más különbözeti importálás módszer aktív hello LDAP-kiszolgálón. Ez a beállítás akkor nem tudja tooimport törölt objektumok.
* USNChanged. Lásd: [https://msdn.microsoft.com/library/ms677627.aspx](https://msdn.microsoft.com/library/ms677627.aspx)

### <a name="not-supported"></a>Nem támogatott
a következő LDAP szolgáltatások hello nem támogatottak:

* LDAP-átirányítások (RFC 4511/4.1.10) kiszolgálók között

## <a name="create-a-new-connector"></a>Új összekötő létrehozása
Általános LDAP-összekötő tooCreate, a **szinkronizálási szolgáltatás** válassza ki **kezelőügynök** és **létrehozása**. Jelölje be hello **általános LDAP (Microsoft)** összekötő.

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericldap/createconnector.png)

### <a name="connectivity"></a>Kapcsolatok
Hello kapcsolat oldalon meg kell adnia hello Host, Port és kötés. Attól függően, amelyek kötés kijelölt, további információt a következő részekben hello lehet adni.

![Kapcsolatok](./media/active-directory-aadconnectsync-connector-genericldap/connectivity.png)

* hello kapcsolódási időtúllépés beállítása csak hello első kapcsolat toohello kiszolgálóhoz használt amikor hello séma észlelése.
* Ha a kötés névtelen, majd sem felhasználónév / jelszó és a tanúsítvány nem használható.
* Más kötésekben, írja be adatokat vagy a felhasználónév / jelszó, vagy válasszon egy tanúsítványt.
* Ha Kerberos tooauthenticate használ, majd adni hello tartomány/tartomány hello felhasználó.

Hello **aliasok attribútum** szövegmező RFC4522 szintaxissal hello sémában meghatározott attribútumok szolgál. Ezek az attribútumok séma észlelési nem észlelhető, és hello összekötő kell az azok tooidentify támogatása. Például a következő kell beírni hello attribútum aliasok mezőben toocorrectly hello határozza hello userCertificate attribútum bináris attribútumaként:

`userCertificate;binary`

hello az alábbiakban látható egy példa hogyan Ez a konfiguráció nézhet:

![Kapcsolatok](./media/active-directory-aadconnectsync-connector-genericldap/connectivityattributes.png)

Jelölje be hello **működési attribútumait tartalmazza a sémában** jelölőnégyzet tooalso hello kiszolgáló által létrehozott attribútumait tartalmazza. Ezek közé tartozik például amikor hello objektum lett létrehozva, és a legutóbbi frissítésének ideje attribútumok.

Válassza ki **bővíthető attribútumait tartalmazza a sémában** Ha bővíthető objektumok (RFC4512/4.3) használja, és ez a beállítás lehetővé teszi, hogy minden attribútum toobe használt összes objektum. Ezzel a beállítással lehetővé teszi hello séma túl nagy, kivéve ha hello csatlakoztatott címtárhoz használ a szolgáltatás hello javaslaton-e tookeep hello beállítást.

### <a name="global-parameters"></a>Globális paraméterek
Hello globális paraméterek lapján konfigurálja a hello DN toohello különbözeti Változásnapló és további LDAP-szolgáltatások. hello abban az előre megadott hello LDAP-kiszolgáló által biztosított hello adatokkal.

![Kapcsolatok](./media/active-directory-aadconnectsync-connector-genericldap/globalparameters.png)

hello felső részében a hello kiszolgálójának nevével, például hello nevét hello kiszolgáló által biztosított információit tartalmazza. Összekötő hello is ellenőrzi, hogy hello kötelező vezérlők hello legfelső szintű DSE szerepel. Ezek az intézkedések nem szerepelnek a listán, ha figyelmeztetés jelenik meg. Egyes LDAP-könyvtárak nem szerepelhet a legfelső szintű DSE hello összes szolgáltatás, és előfordulhat, hogy a hello hibák nélkül a Connector működik, akkor is, ha jelen egy figyelmeztetés.

Hello **vezérlők támogatott** checkboxes szabályozhatja az egyes műveletek hello működését:

* A kiválasztott fa törlése, a hierarchia törlődik egy LDAP-hívással. A fa törlése nincs bekapcsolva hello összekötő rekurzív törlési funkciója, szükség esetén.
* A kiválasztott lapozható eredmények összekötő hello nem lapozható importálási futtatása hello lépéseket a megadott hello méret.
* hello VLVControl SortControl pedig egy LDAP-címtár hello alternatív toohello pagedResultsControl tooread adatait.
* Ha mind a három beállítást (pagedResultsControl, VLVControl és SortControl) nincsenek bejelölve hello összekötő importálja az összes objektum meghiúsulhat, ha nagy könyvtár egy műveletben.
* ShowDeletedControl csak akkor használja, ha hello különbözeti importálás módszer USNChanged.

hello Változásnapló DN hello névhasználati környezet használják a különbözeti Változásnapló hello, például **cn = változásnaplója**. Ennek az értéknek kell lennie a megadott toobe képes toodo különbözeti importálás.

hello az alábbiakban olvashat egy listát alapértelmezett módosítási napló DNs:

| Címtár | Különbözeti Változásnapló |
| --- | --- |
| Microsoft AD LDS és AD globális Katalógus |Automatikusan észleli. USNChanged. |
| Apache címtárkiszolgálóra |Nem érhető el. |
| Directory 389 |Változásnapló. Alapértelmezett érték toouse: **cn = változásnaplója** |
| IBM Tivoli DS |Változásnapló. Alapértelmezett érték toouse: **cn = változásnaplója** |
| Isode könyvtár |Változásnapló. Alapértelmezett érték toouse: **cn = változásnaplója** |
| Novell/NetIQ eDirectory |Nem érhető el. Timestamp típusú. hello összekötő által használt utolsó frissített dátum/idő tooget hozzáadja, és rögzíti. |
| Nyissa meg a DJ/DS-ben |Változásnapló.  Alapértelmezett érték toouse: **cn = változásnaplója** |
| Nyissa meg az LDAP |Hozzáférési napló. Alapértelmezett érték toouse: **cn = accesslog** |
| Oracle DSEE |Változásnapló. Alapértelmezett érték toouse: **cn = változásnaplója** |
| A virtuális Lemezszolgáltatás RadiantOne |Virtuális könyvtár. Hello kapcsolódó directory tooVDS függ. |
| Egy Sun Directory Server |Változásnapló. Alapértelmezett érték toouse: **cn = változásnaplója** |

hello jelszó attribútum hello attribútum hello összekötő neve hello használandó tooset hello jelszavát a jelszó módosítása és a jelszó set műveletek.
Ez az érték az alapértelmezett értéke túl**userPassword** , de egy adott LDAP-rendszer szükség esetén módosítható.

Hello további partíciók listája hogy a rendszer lehetséges tooadd további névteret automatikusan nem észlelt. Például használható ezt a beállítást, ha több kiszolgáló logikai fürt, amely az összes importálni kell: hello ugyanannyi időt vesz igénybe. Ahogy az Active Directory képes több tartományban vannak egy erdőben, de minden olyan tartománynál megosztása több séma is ebbe a mezőbe írja be a hello további névteret szimulált azonos hello. Minden névtér különböző kiszolgálókról származó importálhatja, és további hello konfigurálása partíciók és hierarchiák lapon van konfigurálva. Használja a Ctrl + Enter tooget új sor.

### <a name="configure-provisioning-hierarchy"></a>Kiépítési hierarchia konfigurálása
Ezen a lapon egy használt toomap hello DN összetevő, például OU toohello objektum típus, amely kell megadva lennie, például organizationalUnit.

![Telepítési hierarchia](./media/active-directory-aadconnectsync-connector-genericldap/provisioninghierarchy.png)

Kiépítési hierarchia konfigurálása, beállíthatja hello összekötő tooautomatically hozzon létre egy struktúra, amikor szükséges. Például ha egy névtér dc = contoso, dc = com és egy új objektum cn Joe, ou = = budapesti, c = US, dc = contoso, dc = com ki van építve, majd hello összekötő objektum típusa ország az USA és egy organizationalUnit Budapest számára hozhat létre, ha azok még nem hello könyvtárban található.

### <a name="configure-partitions-and-hierarchies"></a>Partíciók és hierarchiák konfigurálása
Hello partíciók és hierarchiák lapon jelölje be az összes névterek objektumok megtervezése tooimport és exportálása.

![Partíciók](./media/active-directory-aadconnectsync-connector-genericldap/partitions.png)

Minden névtér esetében is, akkor bírálja felül a hello kapcsolat képernyőn megadott hello értékek lehetséges tooconfigure kapcsolati beállításai. Ha ezek az értékek tootheir alapértelmezett üres érték megmaradnak, hello hello kapcsolat képernyőjéről adatok.

Akkor is lehetséges tooselect mely tárolók és szervezeti egységek hello összekötő importálhat és exportálhat.

Keresés végrehajtása során ez a hello partíció az összes tároló keresztül történik. Azokban az esetekben, ahol nagyszámú tárolók e működés tooperformance teljesítménycsökkenést.

>[!NOTE]
Hello 2017. márciusi frissítés toohello általános LDAP kezdve összekötő keresések hatókör tooonly kijelölt hello tárolókban lévő korlátozott a is. Ezt megteheti "Keresési csak a kijelölt tárolók a" hello jelölőnégyzet kiválasztásával, az alábbi hello ábrán látható módon.

![Keresés csak a kijelölt tároló](./media/active-directory-aadconnectsync-connector-genericldap/partitions-only-selected-containers.png)

### <a name="configure-anchors"></a>Horgonyok konfigurálása
Ezen a lapon mindig rendelkezik előre beállított értékkel, és nem módosítható. Ha hello server szállító van megadva, majd hello horgonyzási előfordulhat, hogy megjelenni megváltoztathatatlan attribútum, például hello GUID egy objektum. Ha nem észlelhető, vagy toonot ismert attribútum nem módosítható rendelkezik, majd hello összekötő használ hello kapcsolatainak megkülönböztető név (megkülönböztető neve).

![a központi jellegűek](./media/active-directory-aadconnectsync-connector-genericldap/anchors.png)


hello az alábbiakban olvashat az LDAP-kiszolgálókba és hello horgonyzási használja listáját:

| Címtár | Horgonyattribútum |
| --- | --- |
| Microsoft AD LDS és AD globális Katalógus |objectGUID |
| A címtárkiszolgáló 389 |megkülönböztető név |
| Apache könyvtár |megkülönböztető név |
| IBM Tivoli DS |megkülönböztető név |
| Isode könyvtár |megkülönböztető név |
| Novell/NetIQ eDirectory |GUID |
| Nyissa meg a DJ/DS-ben |megkülönböztető név |
| Nyissa meg az LDAP |megkülönböztető név |
| Oracle ODSEE |megkülönböztető név |
| A virtuális Lemezszolgáltatás RadiantOne |megkülönböztető név |
| Egy Sun Directory Server |megkülönböztető név |

## <a name="other-notes"></a>Egyéb megjegyzések
Ez a szakasz az adott toothis összekötő vagy egyéb okból fontos tooknow szempontok információival.

### <a name="delta-import"></a>Különbözeti importálás
Nyissa meg az LDAP hello különbözeti vízjel UTC dátum/idő. Emiatt hello FIM szinkronizálási szolgáltatás, és nyissa meg az LDAP hello között kell szinkronizálni. Ha nem, akkor néhány elemet hello különbözeti módosítási naplóban lehet megadva.

Novell eDirectory a hello különbözeti importálás nem érzékeli bármely objektum törlése. Ezért fontos toorun teljes rendszeresen toofind az összes törölt objektumokat importálhat.

A különbözeti mappák módosításnapló dátum és idő alapján, magas van-e toorun rendszeres időközönként a teljes importálás ajánlott. Ez a folyamat lehetővé teszi, hogy hello szinkronizálási motor toofind és eltérő hello LDAP-kiszolgáló és a jelenleg a kapcsolódási térbe hello között.

## <a name="troubleshooting"></a>Hibaelhárítás
* Hogyan tooenable naplózási tootroubleshoot hello összekötő információkért lásd: hello [hogyan ETW-nyomkövetés összekötők tooEnable](http://go.microsoft.com/fwlink/?LinkId=335731).
