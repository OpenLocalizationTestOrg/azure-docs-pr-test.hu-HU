---
title: "-Microsoft fenyegetések modellezése eszköz - aaaAuthorization Azure |} Microsoft Docs"
description: "a fenyegetések modellezése eszköz hello felfedett fenyegetések megoldást"
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 3ea7ae2b46baa8578e574e6006b98dfe172829e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="security-frame-authorization--mitigations"></a>Biztonsági keret: Engedélyezési |} Megoldást 
| A termék vagy szolgáltatás | Cikk |
| --------------- | ------- |
| **Gép megbízhatósági kapcsolat határán** | <ul><li>[Győződjön meg arról, hogy megfelelő hozzáférés-vezérlési listák konfigurált toorestrict jogosulatlan hozzáférés toodata hello eszközön](#acl-restricted-access)</li><li>[Győződjön meg arról, hogy a bizalmas felhasználó-specifikus alkalmazás tartalma található-e felhasználóiprofil könyvtárban](#sensitive-directory)</li><li>[Győződjön meg arról, hogy hello központilag telepített alkalmazások legkevesebb jogosultsággal futtatja](#deployed-privileges)</li></ul> |
| **Webalkalmazás** | <ul><li>[Egymást követő lépés rendelés kényszerítése üzleti logika adatfolyamok feldolgozásakor](#sequential-logic)</li><li>[Mechanizmus tooprevent számbavételi sebességével megvalósítása](#rate-enumeration)</li><li>[Győződjön meg arról, hogy megfelelő engedély van beállítva, és a legalacsonyabb jogosultságok elvét az azt követő](#principle-least-privilege)</li><li>[Üzleti logika és az erőforrás hozzáférés felhasználását engedélyezési döntésekhez nem kell a bejövő kérelemben szereplő paraméterek alapján](#logic-request-parameters)</li><li>[Győződjön meg arról, hogy a tartalom és erőforrások nem enumerálható vagy közötti választásra böngészés keresztül érhető el](#enumerable-browsing)</li></ul> |
| **Adatbázis** | <ul><li>[Gondoskodjon arról, hogy legalább jogosultságú fiókok használt tooconnect tooDatabase kiszolgáló](#privileged-server)</li><li>[Sor szintű biztonsági RLS tooprevent bérlők hozzáférését az adatok egymás megvalósítása](#rls-tenants)</li><li>[SysAdmin (rendszergazda) szerepkör csak kell érvényes szükséges felhasználók](#sysadmin-users)</li></ul> |
| **Az IoT átjáró** | <ul><li>[Csatlakozás tooCloud átjáró legkevésbé jogosultságú tokenek használatára](#cloud-least-privileged)</li></ul> |
| **Az Azure Event Hubs** | <ul><li>[A csak küldési engedélyeket SAS-kulcsot használ az eszköz jogkivonatokat előállító](#sendonly-sas)</li><li>[Ne használja a jogkivonatot, amely közvetlen hozzáférést toohello Event Hubs](#access-tokens-hub)</li><li>[Csatlakozás tooEvent SAS használatával Hub kulcsok, hogy rendelkezik hello minimálisan szükséges jogosultságok](#sas-minimum-permissions)</li></ul> |
| **Az Azure Document DB rendszerbe** | <ul><li>[Erőforrás jogkivonatok tooconnect tooDocumentDB lehetőség használata](#resource-docdb)</li></ul> |
| **Az Azure megbízhatósági kapcsolat határán** | <ul><li>[Részletes access management tooAzure előfizetés engedélyezése az RBAC használata](#grained-rbac)</li></ul> |
| **Service Fabric megbízhatósági kapcsolat határán** | <ul><li>[Az RBAC használata ügyfél toocluster műveletek korlátozása](#cluster-rbac)</li></ul> |
| **Dynamics CRM** | <ul><li>[Biztonsági modellezési mező biztonság használható, és ha szükséges](#modeling-field)</li></ul> |
| **Dynamics CRM-portál** | <ul><li>[Hajtsa végre a biztonsági modellezési tartva, hogy hello biztonsági modell hello portal CRM hello részeinek eltér a portál fiókok](#portal-security)</li></ul> |
| **Azure Storage** | <ul><li>[Az Azure Table Storage-ban entitástartományának részletes engedély megadása](#permission-entities)</li><li>[Engedélyezze a szerepköralapú hozzáférés-vezérlés (RBAC) storage-fiók tooAzure Azure Resource Manager használatával](#rbac-azure-manager)</li></ul> |
| **Mobileszköz ügyfél** | <ul><li>[Implicit függetlenítés vagy észlelési telepítés végrehajtása](#rooting-detection)</li></ul> |
| **WCF** | <ul><li>[A WCF gyenge Osztályhivatkozást](#weak-class-wcf)</li><li>[WCF-megvalósítása engedélyezési vezérlő](#wcf-authz)</li></ul> |
| **Webes API** | <ul><li>[Valósítja meg a megfelelő engedélyezési mechanizmus ASP.NET Web API-ban.](#authz-aspnet)</li></ul> |
| **IoT-eszközök** | <ul><li>[Engedélyezési ellenőrzéseket hajtanak végre hello eszköz, ha az támogatja-e különböző jogosultsági szintek különböző műveleteket](#device-permission)</li></ul> |
| **Az IoT-mező átjáró** | <ul><li>[Engedélyezési ellenőrzéseket hajtanak végre hello mező átjáró, ha az támogatja-e különböző jogosultsági szintek különböző műveleteket](#field-permission)</li></ul> |

## <a id="acl-restricted-access"></a>Győződjön meg arról, hogy megfelelő hozzáférés-vezérlési listák konfigurált toorestrict jogosulatlan hozzáférés toodata hello eszközön

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Gép megbízhatósági kapcsolat határán | 
| **SDL fázis**               | Környezet |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Győződjön meg arról, hogy megfelelő hozzáférés-vezérlési listák konfigurált toorestrict jogosulatlan hozzáférés toodata hello eszközön|

## <a id="sensitive-directory"></a>Győződjön meg arról, hogy a bizalmas felhasználó-specifikus alkalmazás tartalma található-e felhasználóiprofil könyvtárban

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Gép megbízhatósági kapcsolat határán | 
| **SDL fázis**               | Környezet |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Győződjön meg arról, hogy bizalmas felhasználó-specifikus alkalmazás tartalmát a felhasználóiprofil könyvtárban tárolja. Ez a tooprevent hello több felhasználója gép egymás adatok hozzáférését.|

## <a id="deployed-privileges"></a>Győződjön meg arról, hogy hello központilag telepített alkalmazások legkevesebb jogosultsággal futtatja

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Gép megbízhatósági kapcsolat határán | 
| **SDL fázis**               | Környezet |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Győződjön meg arról, hogy fut-e telepített hello alkalmazás legkevesebb jogosultsággal. |

## <a id="sequential-logic"></a>Egymást követő lépés rendelés kényszerítése üzleti logika adatfolyamok feldolgozásakor

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Ebben a szakaszban egy eredeti tooenforce hello alkalmazás tooonly folyamat üzleti logika folyamatok egymást követő lépés ahhoz, szeretné, hogy az emberi valós időben feldolgozott összes lépést, és nem dolgozza fel nem megfelelő sorrendben, felhasználó által futtatott tooverify kihagyva sorrendben lépéseket, egy másik felhasználó lépés, feldolgozott vagy túl gyorsan benyújtott tranzakciók.|

## <a id="rate-enumeration"></a>Mechanizmus tooprevent számbavételi sebességével megvalósítása

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Győződjön meg arról, hogy bizalmas azonosítók véletlenszerű. CAPTCHA vezérléséhez a névtelen lapokon. Győződjön meg arról, hogy hiba és a kivétel nem kódproblémájáról adatokat|

## <a id="principle-least-privilege"></a>Győződjön meg arról, hogy megfelelő engedély van beállítva, és a legalacsonyabb jogosultságok elvét az azt követő

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | <p>hello elve azt jelenti, hogy csak a felhasználók munka alapvető toothat képező jogosultságok jogosultságot ad a felhasználói fiók. Például egy biztonsági mentési felhasználónak nem kell tooinstall szoftver: emiatt hello biztonsági mentési felhasználó rendelkezik-e a jogok csak toorun biztonsági mentési és biztonsági mentésével kapcsolatos alkalmazások. Más jogosultságokat, mint például az új szoftverek telepítése le vannak tiltva. hello elvet is tooa személyi számítógép felhasználó általában normál felhasználói fiókok használhatók, és megnyílik egy rendszerjogosultságú, jelszóval védett fiókot (Ez azt jelenti, hogy a rendszeradminisztrátor) csak amikor hello helyzet feltétlenül megköveteli azt. </p><p>Ez az elv alkalmazott tooyour-webalkalmazások is lehet. Ahelyett, hogy kizárólag attól függően, szerepkör-alapú hitelesítési módszerek használatával munkamenetek, ahelyett, hogy szeretnénk tooassign toousers jogosultságok olyan adatbázis-alapú hitelesítés rendszerrel. Továbbra is használunk a rendelés tooidentify munkamenetek Ha hello be voltak jelentkezve helyesen, csak most nem rendeli egy adott szerepkör azt hozzárendelése neki a jogosultságok tooverify milyen műveleteket, hogy a felhasználó jogosultsági szintű tooperform hello rendszeren. A módszer nagy pro is, amikor a felhasználó rendelkezik-e hozzárendelve a rendszer alkalmazza a beállításokat a hello menet közben óta hello hozzárendelése nem függ a hello munkamenet, melyből egyéb tooexpire először kevesebb jogosultsággal toobe.</p>|

## <a id="logic-request-parameters"></a>Üzleti logika és az erőforrás hozzáférés felhasználását engedélyezési döntésekhez nem kell a bejövő kérelemben szereplő paraméterek alapján

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Amikor a rendszer annak ellenőrzése, hogy a felhasználó áll-e a korlátozott tooreview bizonyos, hello hozzáférési korlátozásokat kell adatfeldolgozási kiszolgálóoldali. hello userID a bejelentkezési munkamenet változó belül kell tárolni, és használt tooretrieve felhasználói adatok hello adatbázisból kell lennie. |

### <a name="example"></a>Példa
```SQL
SELECT data 
FROM personaldata 
WHERE userID=:id < - session var 
```
Most egy esetleges támadó nem védelmet is módosítható hello alkalmazás műveletet, mert hello azonosítója hello adatok lekérése kezelt kiszolgálóoldali.

## <a id="enumerable-browsing"></a>Győződjön meg arról, hogy a tartalom és erőforrások nem enumerálható vagy közötti választásra böngészés keresztül érhető el

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | <p>Bizalmas statikus és konfigurációs fájljai nem hello web-legfelső szintű kell tartani. A nyilvános nem szükséges tartalom toobe vagy a megfelelő hozzáférés-vezérlők alkalmazni, vagy a hello eltávolítása a tartalom magát.</p><p>Is közötti választásra böngészés általában együtt találgatásos technikák toogather információk létrehozására tett kísérlettel tooaccess annyi URL-címek lehetséges tooenumerate könyvtárak és fájlok a kiszolgálón. A támadók gyakran meglévő összes változatát előfordulhat, hogy keressen fájlokat. Például egy jelszó keresési psswd.txt, password.htm, password.dat és egyéb változatok beleértve fájlok volna foglalnia.</p><p>toomitigate, ezt a képességet biztosít a találgatásos észlelése kísérletek tartalmaznia kell.</p>|

## <a id="privileged-server"></a>Gondoskodjon arról, hogy legalább jogosultságú fiókok használt tooconnect tooDatabase kiszolgáló

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Adatbázis | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [SQL Database engedélyekkel hierarchia](https://msdn.microsoft.com/library/ms191465), [SQL adatbázis securables](https://msdn.microsoft.com/library/ms190401) |
| **Lépések** | Legkevesebb jogosultságú fiókok használt tooconnect toohello adatbázis kell lennie. Alkalmazás bejelentkezési hello adatbázisban kell korlátozódnia, és csak a kijelölt tárolt eljárások kell végrehajtani. Az alkalmazás bejelentkezési nincs közvetlen tábla hozzáféréssel kell rendelkeznie. |

## <a id="rls-tenants"></a>Sor szintű biztonsági RLS tooprevent bérlők hozzáférését az adatok egymás megvalósítása

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Adatbázis | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Az SQL Azure, a helyi üzemeltetésű |
| **Attribútumok**              | SQL - 12-es verzió, az SQL-verzió - verzió MsSQL2016 |
| **Hivatkozások**              | [SQL Server a sorszintű biztonságot (RLS)](https://msdn.microsoft.com/library/azure/dn765131.aspx) |
| **Lépések** | <p>A sorszintű biztonsági lehetővé teszi, hogy az ügyfelek toocontrol hozzáférés toorows egy adatbázis tábláinak hello jellemzők hello felhasználó (pl. csoport tagsági vagy a végrehajtási környezet) lekérdezése alapján.</p><p>A sorszintű biztonságot (RLS) egyszerűbbé teszi a hello tervezési és az alkalmazás biztonsági kódolása. RLS lehetővé teszi sor adatelérési tooimplement korlátozásait. Például annak érdekében, hogy munkavállalók férhessenek hozzá csak ezen adatok sorokat, amelyek a megfelelő tootheir részleg, vagy a felhasználói adatok hozzáférés tooonly hello adatok vonatkozó tootheir vállalati korlátozása.</p><p>hello hozzáférés korlátozási logika hello adatbázis rétegben található, hanem egy másik alkalmazás réteg adataiból hello funkcióval. hello adatbázisrendszer hello hozzáférési korlátozásokat alkalmazza, minden alkalommal, amikor az adott adatok próbál meg elérni a réteg alapján. Így hello biztonsági rendszer megbízhatóbb és robusztus hello biztonsági rendszer hello felületének csökkentése révén.</p><p>|

Vegye figyelembe, hogy az a-kész adatbázis szolgáltatás RLS alkalmazható csak tooSQL 2016 kezdődően kiszolgáló és az Azure SQL-adatbázis. Ha hello out-of-az-box RLS funkció nincs megvalósítva, biztosítani kell, hogy adat-hozzáférési-e a korlátozott jelenlegi nézetek és eljárások

## <a id="sysadmin-users"></a>SysAdmin (rendszergazda) szerepkör csak kell érvényes szükséges felhasználók

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Adatbázis | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [SQL Database engedélyekkel hierarchia](https://msdn.microsoft.com/library/ms191465), [SQL adatbázis securables](https://msdn.microsoft.com/library/ms190401) |
| **Lépések** | Hello SysAdmin (rendszergazda) rögzített kiszolgálói szerepkör tagjai legyen erősen korlátozott, és soha nem tartalmazza az alkalmazások által használt fiókok.  Nézze át a hello hello a szerepkörben levő felhasználók listáját, és távolítsa el a felesleges fiókokat|

## <a id="cloud-least-privileged"></a>Csatlakozás tooCloud átjáró legkevésbé jogosultságú tokenek használatára

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Az IoT átjáró | 
| **SDL fázis**               | Környezet |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | Átjáró choice - Azure IoT Hub |
| **Hivatkozások**              | [Az IOT-központ hozzáférés-vezérlés](https://azure.microsoft.com/documentation/articles/iot-hub-devguide/#Security) |
| **Lépések** | Adja meg a legalacsonyabb jogosultsági szint engedélyek toovarious összetevők, amelyek kapcsolódnak tooCloud átjáró (IoT-központ). Tipikus példája – eszközt felügyeletre/kiépített összetevő használja registryread írása, esemény processzor (ASA) használja a szolgáltatás csatlakozzon. Egyes eszközök csatlakoznak a hitelesítő adatai segítségével|

## <a id="sendonly-sas"></a>A csak küldési engedélyeket SAS-kulcsot használ az eszköz jogkivonatokat előállító

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Az Azure Event Hubs | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Event Hubs hitelesítés és a biztonsági modell – áttekintés](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Lépések** | Az SAS-kulcsot használt toogenerate egyes eszközök tokenjeit. Előállítása hello eszköz jogkivonatát közben csak küldési engedéllyel az SAS-kulcsot használ a megadott közzétevő|

## <a id="access-tokens-hub"></a>Ne használja a jogkivonatot, amely közvetlen hozzáférést toohello Event Hubs

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Az Azure Event Hubs | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Event Hubs hitelesítés és a biztonsági modell – áttekintés](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Lépések** | Nem kell biztosítani a jogkivonatot, amely engedélyezi a közvetlen hozzáférést toohello eseményközpont toohello eszköz. Egy minimális jogosultságokkal rendelkező token használatával csak tooa publisher segít azonosítani és tiltólistára akkor, ha hozzáférést biztosító eszköz hello toobe egy engedélyezetlen található, vagy sérült eszköz.|

## <a id="sas-minimum-permissions"></a>Csatlakozás tooEvent SAS használatával Hub kulcsok, hogy rendelkezik hello minimálisan szükséges jogosultságok

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Az Azure Event Hubs | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Event Hubs hitelesítés és a biztonsági modell – áttekintés](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Lépések** | Adja meg a legalacsonyabb jogosultsági szint engedélyek toovarious háttér-alkalmazások toohello Eseményközpont-hez. Minden háttér-alkalmazáshoz külön SAS-kulcsok létrehozása, és csak olyan küldési, Receive vagy kezelése toothem az hello szükséges engedélyek –.|

## <a id="resource-docdb"></a>Erőforrás jogkivonatok tooconnect tooCosmos DB, amikor csak lehetséges használata

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Az Azure Document DB rendszerbe | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Egy erőforrás-jogkivonat DocumentDB engedély-erőforrással társítva, és rögzíti hello közötti kapcsolat hello felhasználó az adatbázis és hello engedély, hogy a felhasználó rendelkezik-e egy adott DocumentDB alkalmazás erőforrás (pl. gyűjtemény és dokumentum). Egy erőforrás-jogkivonat tooaccess hello DocumentDB mindig használja, ha hello ügyfél nem adható meg megbízhatóként a fő vagy csak olvasható kulcsot – például a hordozható vagy asztali ügyfél végfelhasználói alkalmazás kezelése. A fő oszlopkulcs vagy csak olvasható kulcsokat biztonságosan tárolhatja ezeket a kulcsokat háttér-alkalmazással.|

## <a id="grained-rbac"></a>Részletes access management tooAzure előfizetés engedélyezése az RBAC használata

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Az Azure megbízhatósági kapcsolat határán | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Szerepkör hozzárendelések toomanage hozzáférés tooyour Azure-előfizetés erőforrásainak használatához](https://azure.microsoft.com/documentation/articles/role-based-access-control-configure/)  |
| **Lépések** | Az Azure Szerepköralapú hozzáférés-vezérlés (RBAC) részletes hozzáférés-vezérlést biztosít az Azure-hoz. Az RBAC használata, biztosíthat csak hello mértékű hozzáférést a felhasználóknak frissíteniük kell tooperform a munkájukat.|

## <a id="cluster-rbac"></a>Az RBAC használata ügyfél toocluster műveletek korlátozása

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Service Fabric megbízhatósági kapcsolat határán | 
| **SDL fázis**               | Környezet |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | Környezet – Azure |
| **Hivatkozások**              | [A Service Fabric ügyfelek szerepköralapú hozzáférés-vezérlés](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security-roles/) |
| **Lépések** | <p>Az Azure Service Fabric két különböző hozzáférést vezérlő típusokat támogatja az ügyfelek, amelyek csatlakoztatott tooa Service Fabric-fürt: rendszergazdai és felhasználói. Hozzáférés-vezérlés lehetővé teszi, hogy hello rendszergazda toolimit hozzáférés toocertain fürt fürtműveletekben hello fürt biztonságosabbá tétele a felhasználók különböző csoportok számára.</p><p>A rendszergazdák teljes hozzáféréssel toomanagement képességek (beleértve az olvasási/írási képességek) rendelkeznek. Felhasználók, alapértelmezés szerint rendelkezik csak olvasási hozzáféréssel toomanagement képességek (például lekérdezési lehetőségek), és hello képességét tooresolve alkalmazások és szolgáltatások.</p><p>Hello két ügyfél szerepkörök (a rendszergazda és az ügyfél) a fürt létrehozása hello időpontjában adja meg az egyes különálló tanúsítványok megadásával.</p>|

## <a id="modeling-field"></a>Biztonsági modellezési mező biztonság használható, és ha szükséges

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Dynamics CRM | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Biztonsági modellezési mező biztonság használható, és ha szükséges|

## <a id="portal-security"></a>Hajtsa végre a biztonsági modellezési tartva, hogy hello biztonsági modell hello portal CRM hello részeinek eltér a portál fiókok

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Dynamics CRM-portál | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Hajtsa végre a biztonsági modellezési tartva, hogy hello biztonsági modell hello portal CRM hello részeinek eltér a portál fiókok|

## <a id="permission-entities"></a>Az Azure Table Storage-ban entitástartományának részletes engedély megadása

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Azure Storage | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | StorageType - tábla |
| **Hivatkozások**              | [Hogyan érik el toodelegate tooobjects a SAS használatával az Azure storage-fiókban](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_data-plane-security) |
| **Lépések** | Bizonyos üzleti forgatókönyvek Azure Table Storage lehet szükséges toostore toodifferent felek caters bizalmas adatokat. Például bizalmas adatokat toodifferent országok alkalmas. Ebben az esetben SAS-aláírások lehet létrehozni hello partíció- és sorfejlécek kulcstartományokkal, megadásával úgy, hogy egy felhasználó hozzáférhessen az adatok adott tooa adott ország.| 

## <a id="rbac-azure-manager"></a>Engedélyezze a szerepköralapú hozzáférés-vezérlés (RBAC) storage-fiók tooAzure Azure Resource Manager használatával

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Azure Storage | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Hogyan toosecure a tárolási fiók szerepköralapú hozzáférés-vezérlést (RBAC)](https://azure.microsoft.com/documentation/articles/storage-security-guide/#management-plane-security) |
| **Lépések** | <p>Amikor létrehoz egy új tárfiókot, Azure Resource Manager és klasszikus telepítési modell választja. hello Klasszikus modell erőforrások létrehozása az Azure-ban csak lehetővé teszi, hogy mindent hozzáférés toohello előfizetés, és a tárfiók viszont hello.</p><p>Hello Azure Resource Manager modellt put hello tárfiók egy erőforrás csoport és a vezérlés hozzáférési toohello felügyeleti vezérlősík bizonyos tárolási fiók az Azure Active Directoryval. Például biztosíthat bizonyos felhasználók hello képességét tooaccess hello tárfiókok kulcsait, amíg más felhasználók hello tárfiók kapcsolatos információk is megtekinthetők, de nem hello tárfiókkulcsok.</p>|

## <a id="rooting-detection"></a>Implicit függetlenítés vagy észlelési telepítés végrehajtása

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Mobileszköz ügyfél | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | <p>Alkalmazás megvédik az esetben, ha a telefonszám feltörték vagy függetlenített saját konfigurációs és a felhasználó adatait. Telepítés/jailbreakelve megtörje azt jelenti, hogy jogosulatlan hozzáférés, mely a normál felhasználók a saját telefonokon nem tegye. Ezért az alkalmazás kell rendelkeznie az implicit észlelési logika alkalmazás következő indításakor, ha hello phone rootolt toodetect.</p><p>hello észlelési logikát is egyszerűen hozzáférhet az melyik általában csak gyökér szintű felhasználó férhet hozzá, például fájlok:</p><ul><li>/System/App/SUPERUSER.apk</li><li>/ sbin/su</li><li>/System/bin/su</li><li>/System/xbin/su</li><li>/Data/Local/xbin/su</li><li>/Data/local/bin/su</li><li>/System/SD/xbin/su</li><li>/System/bin/FailSafe/su</li><li>/Data/Local/su</li></ul><p>Hello alkalmazások hozzáférhetnek az ezeket a fájlokat, ha azt jelöli, hogy fut-e hello alkalmazás legfelső szintű felhasználóként.</p>|

## <a id="weak-class-wcf"></a>A WCF gyenge Osztályhivatkozást

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | WCF | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános, NET-keretrendszer 3 |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [erősítse meg Királyság](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Lépések** | <p>hello rendszer gyenge osztály hivatkozást, amely előfordulhat, hogy egy támadó nem engedélyezett tooexecute kódot használja. hello program egy felhasználói osztály, amely egyedileg azonosítja nem hivatkozik. Ha .NET betölti a gyengén azonosított osztály hello CLR típus betöltési keres hello osztály alábbi hello a helyek hello a megadott sorrendben:</p><ol><li>Ha hello szerelvény hello típusú ismert, hello betöltő keresések hello konfigurációs fájl átirányítási helyek, a GAC-ban, hello aktuális szerelvény konfigurációs információk segítségével, és alkalmazás alapkönyvtárának hello</li><li>Ha hello szerelvény ismeretlen, hello betöltő keresések hello aktuális szerelvény mscorlib használatát és hello TypeResolve eseménykezelő által visszaadott hello helye</li><li>A CLR-keresés sorrendjét módosíthatók hurkok például hello típus továbbítási mechanizmus és hello AppDomain.TypeResolve esemény</li></ol><p>Ha egy támadó kihasználja hello CLR keresési sorrendje hozzon létre egy másik osztály hello azonos névvel és helyezi el azt egy másik elérési utat adott hello CLR előbb betölti, hello CLR véletlenül végrehajtja a támadó által megadott kód hello</p>|

### <a name="example"></a>Példa
Hello `<behaviorExtensions/>` hello WCF konfigurációs fájl az alábbi elem arra utasítja a WCF tooadd egyéni viselkedés tooa adott WCF osztálykiterjesztések.
```
<system.serviceModel>
    <extensions>
        <behaviorExtensions>
            <add name=""myBehavior"" type=""MyBehavior"" />
        </behaviorExtensions>
    </extensions>
</system.serviceModel>
```
Nevekkel teljesen minősített (erős) egyedileg azonosítja a típusát, és tovább növeli a rendszer biztonsági. Használja a szerelvény teljesen minősített nevek, típusok hello machine.config és az app.config fájlban regisztrálásakor.

### <a name="example"></a>Példa
Hello `<behaviorExtensions/>` hello WCF konfigurációs fájl az alábbi elem arra utasítja a WCF tooadd egyéni viselkedés erősen hivatkozott osztály tooa adott WCF kiterjesztést.
```
<system.serviceModel>
    <extensions>
        <behaviorExtensions>
            <add name=""myBehavior"" type=""Microsoft.ServiceModel.Samples.MyBehaviorSection, MyBehavior,
            Version=1.0.0.0, Culture=neutral, PublicKeyToken=null"" />
        </behaviorExtensions>
    </extensions>
</system.serviceModel>
```

## <a id="wcf-authz"></a>WCF-megvalósítása engedélyezési vezérlő

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | WCF | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános, NET-keretrendszer 3 |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [erősítse meg Királyság](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Lépések** | <p>Ez a szolgáltatás nem használ olyan engedélyezési vezérlőt. Amikor egy ügyfél meghívja az adott WCF-szolgáltatások, WCF biztosít a különböző hitelesítési sémák, győződjön meg arról, hogy hello hívó rendelkezik engedéllyel tooexecute hello szolgáltatás metódus hello kiszolgáló. Ha engedélyezési vezérlők WCF-szolgáltatások nincsenek engedélyezve, a hitelesített felhasználók jogosultság-eszkalációs érhető el.</p>|

### <a name="example"></a>Példa
a következő konfigurációs hello WCF toonot hello engedély szintje hello ügyfél arra utasítja a hello szolgáltatás végrehajtásakor:
```
<behaviors>
    <serviceBehaviors>
        <behavior>
            ...
            <serviceAuthorization principalPermissionMode=""None"" />
        </behavior>
    </serviceBehaviors>
</behaviors>
```
Egy szolgáltatás engedélyezési sémát tooverify, amely a hívó hello szolgáltatás metódus hello használata engedélyezett toodo stb. WCF két üzemmódot biztosít, és lehetővé teszi, hogy egy egyéni engedélyezési sémát hello meghatározását. hello UseWindowsGroups módban használja a Windows-alapú szerepkörök és a felhasználók, valamint hello UseAspNetRoles mód az ASP.NET szerepkör-szolgáltató, például az SQL Server, tooauthenticate.

### <a name="example"></a>Példa
hello következő konfigurációs arra utasítja a WCF toomake meg arról, hogy az ügyfél által hello hello rendszergazdák csoport tagja hello Hozzáadás szolgáltatás végrehajtása előtt:
```
<behaviors>
    <serviceBehaviors>
        <behavior>
            ...
            <serviceAuthorization principalPermissionMode=""UseWindowsGroups"" />
        </behavior>
    </serviceBehaviors>
</behaviors>
```
hello szolgáltatás majd van deklarálva, hello következő:
```
[PrincipalPermission(SecurityAction.Demand,
Role = ""Builtin\\Administrators"")]
public double Add(double n1, double n2)
{
double result = n1 + n2;
return result;
}
```

## <a id="authz-aspnet"></a>Valósítja meg a megfelelő engedélyezési mechanizmus ASP.NET Web API-ban.

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Webes API | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános, MVC5 |
| **Attribútumok**              | Nincs; Identity Provider - ADFS, identitásszolgáltató - az Azure AD |
| **Hivatkozások**              | [Hitelesítési és engedélyezési ASP.NET webes API](http://www.asp.net/web-api/overview/security/authentication-and-authorization-in-aspnet-web-api) |
| **Lépések** | <p>Szerepköri információkat hello alkalmazás felhasználóinak származtatható az Azure AD, vagy az AD FS-jogcímek Ha hello alkalmazás támaszkodik őket identitás-szolgáltatóként, vagy előfordulhat, hogy hello alkalmazás maga biztosítja azt. Ezekben az esetekben valamelyikében hello egyéni engedélyezési megvalósítási hello felhasználói szerepkörökkel kapcsolatos információi érdemes ellenőrizni.</p><p>Szerepköri információkat hello alkalmazás felhasználóinak származtatható az Azure AD, vagy az AD FS-jogcímek Ha hello alkalmazás támaszkodik őket identitás-szolgáltatóként, vagy előfordulhat, hogy hello alkalmazás maga biztosítja azt. Ezekben az esetekben valamelyikében hello egyéni engedélyezési megvalósítási hello felhasználói szerepkörökkel kapcsolatos információi érdemes ellenőrizni.</p>

### <a name="example"></a>Példa
```C#
[AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, Inherited = true, AllowMultiple = true)]
public class ApiAuthorizeAttribute : System.Web.Http.AuthorizeAttribute
{
        public async override Task OnAuthorizationAsync(HttpActionContext actionContext, CancellationToken cancellationToken)
        {
            if (actionContext == null)
            {
                throw new Exception();
            }

            if (!string.IsNullOrEmpty(base.Roles))
            {
                bool isAuthorized = ValidateRoles(actionContext);
                if (!isAuthorized)
                {
                    HandleUnauthorizedRequest(actionContext);
                }
            }

            base.OnAuthorization(actionContext);
        }

public bool ValidateRoles(actionContext)
{
   //Authorization logic here; returns true or false
}

}
```
Az összes hello, tartományvezérlői és tooprotected kell művelet módszerek kell attribútummal rendelkező attribútum felett.
```C#
[ApiAuthorize]
public class CustomController : ApiController
{
     //Application code goes here
}
```

## <a id="device-permission"></a>Engedélyezési ellenőrzéseket hajtanak végre hello eszköz, ha az támogatja-e különböző jogosultsági szintek különböző műveleteket

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | IoT-eszközök | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | <p>hello eszköz hello hívó toocheck kell engedélyezni, ha hello hívó hello szükséges engedélyek tooperform hello kért művelet. Megadható például, hogy a mondja ki hello eszköz egy intelligens ajtó zárolás hello felhőből figyelhető, valamint a Funkciók, például a távoli zárolás hello ajtó biztosít.</p><p>hello ajtó Smart Lock feloldásának funkciókat biztosít, csak ha valaki fizikailag közel hello ajtó kártya. Ebben az esetben hello hello távoli parancskiadási és vezérlési végrehajtásának kell elvégezni oly módon, hogy nem biztosít semmilyen funkció toounlock hello ajtó, hello átjáró nincs engedélyezett toosend parancs toounlock hello ajtó.</p>|

## <a id="field-permission"></a>Engedélyezési ellenőrzéseket hajtanak végre hello mező átjáró, ha az támogatja-e különböző jogosultsági szintek különböző műveleteket

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Az IoT-mező átjáró | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | hello mező átjáró hello hívó toocheck engedélyezhetik hello szükséges engedélyek tooperform hello művelet kért hello hívó-e. A például nem lehet egy rendszergazdai jogú felhasználó különböző engedélyeinek felület/API használt tooconfigure egy átjáró mező-v/s tooit csatlakozó eszközökön.|
