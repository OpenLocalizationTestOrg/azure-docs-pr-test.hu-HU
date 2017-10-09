---
title: "aaaAzure többtényezős hitelesítés – gyakori kérdések |} Microsoft Docs"
description: "Gyakori kérdések és válaszok kapcsolódó tooAzure többtényezős hitelesítést. A multi-factor Authentication a módszer, amely több, mint egy felhasználónevet és jelszót igényel a felhasználók személyazonossága ellenőrzésére. Biztonsági toouser bejelentkezési és a tranzakciók további réteget biztosít."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 50bb8ac3-5559-4d8b-a96a-799a74978b14
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: kgremban
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c8305cf4c41bf8e9802df192fecdb7e463eff9eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-about-azure-multi-factor-authentication"></a>Azure multi-factor Authentication kapcsolatos gyakori kérdések
Ez a GYIK Azure multi-factor Authentication és hello multi-factor Authentication szolgáltatás használatával kapcsolatos kérdésekre ad választ. Az bontani hello szolgáltatással kapcsolatos kérdésekre általában modellek, felhasználói élményt, számlázási és hibaelhárítás.

## <a name="general"></a>Általános kérdések
**K: hogyan Azure multi-factor Authentication kiszolgáló felhasználói adatok kezeléséhez?**

A multi-factor Authentication kiszolgáló felhasználói adatok csak hello a helyszíni kiszolgálók tárolják. Nem állandó felhasználói adatok hello felhő tárolja. Hello felhasználó a kétlépéses ellenőrzést hajt végre, amikor a multi-factor Authentication kiszolgáló küld adatok toohello felhőszolgáltatás Azure multi-factor Authentication hitelesítéshez. A multi-factor Authentication kiszolgáló és a hello multi-factor Authentication felhőszolgáltatás közötti kommunikáció használja a Secure Sockets Layer (SSL) vagy Transport Layer Security (TLS) 443-as kimenő porton keresztül.

Ha hitelesítési kérelmeket küld toohello felhőalapú szolgáltatás, adatgyűjtés a hitelesítés és a használati jelentésekben. Kétlépéses ellenőrzés naplók szereplő adatok mezők a következők:

* **Egyedi azonosító** (vagy felhasználó neve vagy a helyi multi-factor Authentication hitelesítési kiszolgáló azonosítója)
* **Első és utolsó neve** (nem kötelező)
* **E-mail cím** (nem kötelező)
* **Telefonszám** (használatakor hang hívás vagy SMS-hitelesítés)
* **Eszköz jogkivonatát** (mobilalkalmazás-hitelesítés használatakor)
* **Hitelesítési módszer**
* **Hitelesítés eredménye**
* **A multi-factor Authentication kiszolgáló neve**
* **A multi-factor Authentication kiszolgáló IP**
* **Ügyfél IP** (ha elérhető)

hello opcionális mezők értékét a multi-factor Authentication kiszolgáló beállítható.

hello az ellenőrzés eredményét (sikeres vagy megtagadását), és hello OK megtagadva, ha tárolja hello hitelesítési adatokkal. A hitelesítés és használati jelentések érhetők el az adatok.

## <a name="billing"></a>Számlázás
A legtöbb számlázási kérdésekhez tooeither hello hivatkozással is válaszolhatók [multi-factor Authentication árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/multi-factor-authentication/) vagy hello dokumentációjában [hogyan tooget Azure multi-factor Authentication](multi-factor-authentication-versions-plans.md).

**K: van szó, a hello telefonhívásokat és a hitelesítéshez használt szöveges üzeneteket küld a saját szervezetem?**

Nem, nem kell fizetnie helyezni az egyes telefonhívások, vagy a szöveges üzenetek küldése toousers Azure multi-factor Authentication használatával. A hitelesítés MFA-szolgáltatóra használatakor minden hitelesítést, de nem a használt hello módszert kell fizetni.

Előfordulhat, hogy a felhasználók hello telefonhívásokat vagy szöveges üzeneteinek kapnak, függően tootheir saját telefonja szolgáltatás számítjuk fel.

**K: hello felhasználói számlázási modellt díjat számítanak me minden engedélyezett felhasználó, vagy csak hello gazdarendszerhez végre a kétlépéses ellenőrzést?**

Számlázási konfigurált felhasználók toouse többtényezős hitelesítést, függetlenül attól, hogy azok végre kétlépéses ellenőrzés az adott hónapban hello számát alapul.

**K: hogyan működik a multi-factor Authentication számlázási?**

Amikor létrehoz egy felhasználói vagy a hitelesítés MFA-szolgáltatóra, a szervezet Azure-előfizetésre lesz számlázva, havonta-használata alapján. A számlázási modellt hasonlít toohow Azure virtuális gépek és a webhelyek használatának váltók stb.

Az Azure multi-factor Authentication megvásárolt egy előfizetést, a szervezet csak fizet hello éves licenc díj minden felhasználó számára. MFA licencek és Office 365, Azure AD Premium, vagy vállalati mobilitási + biztonsági csomagok számlázása ily módon. 

További információ a rendelkezésére álló lehetőségekről [hogyan tooget Azure multi-factor Authentication](multi-factor-authentication-versions-plans.md).

**K: van egy Azure multi-factor Authentication ingyenes verzióját?**

Bizonyos esetekben igen.

Azure-rendszergazdák a többtényezős hitelesítést biztosít az Access ingyenesen Azure MFA-funkciók egy részéhez tooMicrosoft online szolgáltatások, beleértve a hello Azure és az Office 365 felügyeleti portálon. Ez az ajánlat csak érvényes tooglobal rendszergazdák, amelyek nem rendelkeznek hello teljes verzióját az Azure MFA egy MFA-licencet, a csomag egyik gyermekszoftver vagy egy önálló fogyasztás alapján szolgáltató Azure Active Directory-példányban. Ha a rendszergazdák hello ingyenes verzióját használja, és ezután vásárol egy teljes változatát, az Azure MFA, az összes globális rendszergazda emelt szintű toohello automatikusan fizetős verziója.

Office 365-felhasználók a többtényezős hitelesítést biztosít az Access ingyenesen Azure MFA-funkciók egy részéhez tooOffice 365 szolgáltatásokat, például az Exchange Online és SharePoint online-hoz. Ez az ajánlat az Office 365-licenccel, amikor az Azure Active Directory hello kulcstulajdonságában hello teljes verzióját az Azure MFA egy MFA-licencet, a csomag egyik gyermekszoftver vagy egy önálló fogyasztás alapján szolgáltató nincs rendelkező toousers vonatkozik.

**A szervezet felhasználói és hitelesítési fogyasztás számlázási modell bármikor közötti váltáshoz k:?**

Ha a szervezete vásárolja MFA önálló szolgáltatásként fogyasztás alapján történő számlázáshoz, a számlázási modellt választja az MFA-szolgáltatóra létrehozásakor. Hello számlázási modellt az MFA-szolgáltatóra létrehozása után nem módosítható. Azonban hello többtényezős hitelesítési szolgáltató törlése, és ezután hozzon létre egy-egy másik számlázási modellt.

Amikor az MFA-szolgáltatóra jön létre, azok csatolt tooan Azure Active Directoryban (más néven "Azure AD-bérlő"). Ha hello aktuális MFA-szolgáltató csatolt tooan az Azure AD bérlői, biztonságosan hello többtényezős hitelesítési szolgáltató törlése és hozzon létre egyet csatolt toohello ugyanazt az Azure AD-bérlő. Azt is megteheti Ha vásárolt elég MFA, Azure AD Premium vagy Enterprise Mobility + Security (EMS) licencet toocover minden felhasználó többtényezős hitelesítés engedélyezett, törölheti hello többtényezős hitelesítési szolgáltató regisztrálását.

Ha a többtényezős hitelesítési szolgáltató *nem* csatolt tooan az Azure AD bérlői, vagy hivatkozás hello új többtényezős hitelesítési szolgáltató tooa másik Azure AD bérlői, felhasználói beállítások és a konfigurációs beállítások nem kerülnek. Is az Azure MFA kiszolgáló meglévő kell újraaktiválni során generált aktiválási hitelesítő adatok használatával toobe hello új MFA-szolgáltató. Hello multi-factor Authentication kiszolgálók toolink újraaktiválhatja azokat toohello új MFA-szolgáltató nem befolyásolja a telefonhívás és a szöveges üzenet hitelesítési, de a mobilalkalmazáson keresztüli értesítések működése leáll az összes felhasználó számára addig, amíg azok újraaktiválása hello mobilalkalmazás.

További tudnivalók a többtényezős hitelesítési szolgáltatók [Ismerkedés az Azure multi-factor Auth Provider a](multi-factor-authentication-get-started-auth-provider.md).

**K: a saját szervezetem válthat fogyasztás alapján számlázási és előfizetés (licenc-alapú modell) bármikor?**

Bizonyos esetekben igen.

Ha a címtár egy *felhasználói* Azure többtényezős hitelesítési szolgáltató, MFA-licencet is hozzáadhat. -Licenccel rendelkező felhasználók a hello felhasználói fogyasztás alapján történő számlázáshoz nem számít. Licenc nélküli felhasználók továbbra is engedélyezhető az MFA szolgáltatásra hello többtényezős hitelesítési szolgáltató használatával. Ha vásároljon, és rendelje hozzá a felhasználók licenceinek beállítva toouse többtényezős hitelesítés, akkor hello Azure többtényezős hitelesítési szolgáltató törlése. Bármikor létrehozhat egy másik felhasználói MFA-szolgáltatóra, ha licencek-nál több felhasználó található jövőbeli hello.

Ha a címtár egy *hitelesítési* Azure többtényezős hitelesítési szolgáltató, hogy mindig számlázása minden hitelesítést, amíg hello MFA-szolgáltató csatolt tooyour előfizetés. Többtényezős hitelesítés licencek toousers rendelhet, de meg fogjuk továbbra is számlázni kétlépéses ellenőrzés kérelmek, hogy származik, valaki-e egy multi-factor Authentication licenccel rendelkező.

**K: a saját szervezetem toouse rendelkezik, és szinkronizálja az identitások toouse Azure multi-factor Authentication?**

Ha a szervezet fogyasztás alapján számlázási modellt használ, Azure Active Directory, nem kötelező, de nem szükséges. Ha a többtényezős hitelesítési szolgáltató nem csatolt tooan Azure AD-bérlő, Azure multi-factor Authentication kiszolgáló vagy hello Azure multi-factor Authentication SDK-t a helyszíni csak telepítheti.

Az Azure Active Directory kell hello licenc modell adni, mert a licencek amikor hozzáadja a toohello Azure AD-bérlő vásároljon, és rendeljen hozzájuk toousers hello könyvtárban.

## <a name="manage-and-support-user-accounts"></a>Kezelésére, és támogatja a felhasználói fiókok

**K: Mit mondjak a felhasználók toodo ha azok nem kap választ a telefont, vagy nem rendelkezik a telefont velük?**

A felhasználók remélhetőleg egynél több ellenőrzési módszer konfigurálva. Jelentkezzen be újra tootry mondja el neki, de válasszon ki egy másik ellenőrzési módszert hello bejelentkezési oldal.

A felhasználók toohello mutathat [végfelhasználói hibaelhárítási útmutató](./end-user/multi-factor-authentication-end-user-troubleshoot.md).


**K: Mit tegyek, ha a felhasználók közül nem olvasható be a tootheir fiók?**

Azáltal, hogy azokat újra hello regisztrációs folyamat toogo alaphelyzetbe állíthatja a hello felhasználói fiókot. További információ [hello felhőben Azure multi-factor Authentication felhasználó-eszköz beállításainak kezelése](multi-factor-authentication-manage-users-and-devices.md).

**K: Mit tegyek, ha a felhasználók egyik elveszti a telefon által használt alkalmazásjelszók?**

a jogosulatlan hozzáféréstől, tooprevent minden hello felhasználói alkalmazásjelszók törlése. Miután hello felhasználó helyettesítő eszköz rendelkezik, azok hello jelszavak hozhatja létre. További információ [hello felhőben Azure multi-factor Authentication felhasználó-eszköz beállításainak kezelése](multi-factor-authentication-manage-users-and-devices.md).

**K: Mit tegyek, ha a felhasználó nem tud bejelentkezni, toonon böngészőben megjelenő alkalmazásokba?**

Ha a szervezet továbbra is használja a hagyományos, és [alkalmazásjelszók használatával hello engedélyezett](multi-factor-authentication-whats-next.md#app-passwords), akkor a felhasználók nem jelentkezhetnek be toothese tanúsítványbeléptetési felhasználónevét és jelszavát. Ehelyett szükségük túl[állítson be alkalmazásjelszót](./end-user/multi-factor-authentication-end-user-app-passwords.md). A felhasználók (Törlés) törölje a bejelentkezési adatait, indítsa újra hello alkalmazást, és jelentkezzen be a felhasználónevét és *alkalmazásjelszót* helyett a rendszeres jelszavát.

Ha a szervezet nem rendelkezik a hagyományos, nem engedélyezze a felhasználók toocreate alkalmazásjelszókat.

> [!NOTE]
> Az ügyfelek Office 2013 modern hitelesítés
>
> Az alkalmazásjelszók csak szükség, modern hitelesítést nem támogató alkalmazások esetében. Ügyfelek Office 2013 modern hitelesítési protokollok, azonban az toobe konfigurálni kell. Újabb Office-ügyfelek automatikusan támogatják a modern hitelesítési protokollok megvalósítását végzi. További információkért lásd: hello [Office 2013 modern hitelesítés nyilvános előzetes kiadásának bejelentésében](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

**K: felhasználóim, hogy időnként hello szöveges üzenetet, nem kapott vagy azok válasz tootwo háromirányú szöveges üzenetek, de hello ellenőrzési időtúllépése.**

A szöveges üzenetek kézbesítési és a válaszok a kétirányú SMS átvételét nem garantált mivel ellenőrizhetetlen tényező, amelyek hatással lehetnek a hello szolgáltatás hello megbízhatóságát. Ezek a tényezők közé tartozik a hello cél ország, hello mobiltelefon szolgáltatója és hello jel erőssége.

Ha a felhasználók gyakran problémákat megbízhatóan a szöveges üzenetek fogadására, kérje meg őket, mobil-es számú toouse hello alkalmazás vagy a telefonhívás rendszerhez készült metódus helyett. hello mobilalkalmazás is fogadhatja az értesítéseket is keresztül mobilhálózati és Wi-Fi-kapcsolatok. Ezenkívül hello mobilalkalmazás hozhat létre ellenőrző kódok kezelésére, akkor is, ha hello eszköz egyáltalán nincs jel rendelkeznek. hello Microsoft Authenticator alkalmazás érhető el [Android](http://go.microsoft.com/fwlink/?Linkid=825072), [IOS](http://go.microsoft.com/fwlink/?Linkid=825073), és [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071).

Ha a szöveges üzenetek kell használnia, kétirányú SMS lehetőség helyett egyirányú SMS használatát javasoljuk. Egyirányú SMS megbízhatóbb, és megakadályozza, hogy felhasználók globális SMS díjfizetési a függő tooa szöveges üzenetet küldött egy másik országból.

**K: módosítható hello időn a felhasználóknál tooenter hello készít ellenőrző kódot szöveges üzenetben hello rendszer lejárata előtt?**

Néhány esetben igen. 

Az Azure MFA kiszolgáló v7.0 vagy annál újabb egyirányú SMS beállítás hello timeout beállítás által egy beállításkulcs megadásával. Miután hello MFA felhőalapú szolgáltatás hello szöveges üzenetet küld, hello ellenőrzőkódot (és egyszeri PIN-kód) ad vissza toohello MFA kiszolgáló. hello MFA kiszolgáló hello kód memória 300 másodpercig alapértelmezés szerint tárolja. Ha hello megadása előtt hello hello kód átadott 300 másodperc, a rendszer megtagadja a hitelesítést. Ezen lépések toochange hello alapértelmezett időtúllépési beállítást használja:

1. Nyissa meg tooHKLM\Software\Wow6432Node\Positive Networks\PhoneFactor.
2. Hozzon létre egy DWORD beállításkulcsot nevű **pfsvc_pendingSmsTimeoutSeconds** és hello idő másodpercben, hogy szeretné-e hello Azure MFA kiszolgáló toostore egyszeri PIN-kódok beállítása.

>[!TIP] 
>Ha több multi-factor Authentication kiszolgálón, csak az egyik, amelynek a feldolgozása hello eredeti hitelesítési kérelem hello tudja toohello felhasználói elküldött hello ellenőrzőkódot. Hello felhasználó hello kódot adja meg, ha a hitelesítési kérelem toovalidate el kell küldenie toohello hello ugyanarra a kiszolgálóra. Ha hello érvényesítése küldött tooa másik kiszolgálót, hello authentication megtagadásra kerül. 

Az Azure MFA kiszolgáló kétirányú SMS beállítás hello időtúllépése a multi-factor Authentication kezelési portál hello. Ha felhasználók hello megadott időkorláton belül nem válaszol az toohello SMS, a rendszer megtagadja a hitelesítést. 

Az Azure MFA hello felhő (beleértve az AD FS-adapter vagy hello hálózati házirend-kiszolgáló bővítmény hello) egyirányú SMS hello időkorlátozási beállítást nem konfigurálható. Az Azure AD 180 másodpercig hello ellenőrző kódot tárolja. 

**K: használhatok hardvertokenek Azure multi-factor Authentication kiszolgáló?**

Azure multi-factor Authentication kiszolgálót használ, ha külső nyitott hitelesítési (OATH) időalapú, egyszer használatos jelszót (TOTP)-tokenek importálása, és majd felhasználja a kétlépéses ellenőrzéshez.

Is használhatja, amelyek OATH TOTP Ha hello titkos kulcs be egy CSV-fájlt, és importálja a multi-factor Authentication kiszolgáló tooAzure ActiveIdentity jogkivonatokat. Active Directory összevonási szolgáltatások (ADFS), az Internet Information Server (IIS) űrlapalapú hitelesítés és a távoli Authentication Dial-In User Service (RADIUS) OATH-tokenek használhatja, mindaddig, amíg hello ügyfélrendszeren fogadhat hello felhasználói beavatkozást.

A következő formátumok hello külső OATH TOTP jogkivonatok importálhatja:  

- Hordozható szimmetrikus kulcs tárolója (PSKC)  
- Ha hello fájl tartalmaz egy sorozatszám, titkos kulcsot Base-32 formátumúnak és egy adott időintervallumban CSV  

**K: használhatok Azure multi-factor Authentication kiszolgáló toosecure Terminálszolgáltatások?**

Igen, de ha használ a Windows Server 2012 R2 vagy újabb csak gondoskodhat a Terminálszolgáltatások távoli asztali átjáró (RD átjáró) használatával.

A Windows Server 2012 R2 biztonsági módosítások megváltozott, hogyan csatlakozzon az Azure multi-factor Authentication kiszolgáló a toohello helyi biztonsági szervezet (LSA) biztonsági csomag a Windows Server 2012 és korábbi verziói. Terminálszolgáltatások a Windows Server 2012 vagy régebbi verzióit is [alkalmazást a Windows-hitelesítés biztonságos](multi-factor-authentication-get-started-server-windows.md#to-secure-an-application-with-windows-authentication-use-the-following-procedure). Ha Windows Server 2012 R2 rendszert kell a távoli asztali átjáró.

**K: beállítása Hívóazonosító az MFA-kiszolgálón, de a felhasználók továbbra is kaphat egy névtelen hívó multi-factor Authentication hívásokat.**

Amikor a multi-factor Authentication hívásokat hello telefonhálózatra a hálózaton keresztül, néha keresztül halad a vivőjel, amely nem támogatja a hívó azonosítóját. Ebből kifolyólag Hívóazonosító nem garantált, annak ellenére, hogy a multi-factor Authentication rendszer hello mindig elküldi.

**K: Miért folyamatban van a saját felhasználók felszólító tooregister biztonsági adataikat?**
Számos oka lehet, hogy sikerült-e a felhasználók rákérdezéses tooregister kell a biztonsági adatokat:

- hello felhasználó engedélyezte a többtényezős Hitelesítést a rendszergazda által az Azure ad-ben, de nem tartalmaz biztonsági információt a fiókjuk még regisztrálva.
- hello felhasználó engedélyezve van az önkiszolgáló jelszó-változtatási Azure AD-ben. hello biztonsági információk segítenek a jövőbeli hello jelszó visszaállítása, ha azok bármikor elfelejtené őket.
- hello felhasználói egy feltételes hozzáférési házirend toorequire MFA és korábban még nem regisztrált multi-factor Authentication alkalmazás érhető el.
- hello felhasználó regisztrál egy eszközt az Azure ad-val (beleértve az Azure AD Join), és a szervezet többtényezős Hitelesítést követel meg az eszközregisztrációhoz tartozó, de hello felhasználó korábban már nincs regisztrálva az MFA szolgáltatásra.
- hello felhasználói a vállalati Windows Hello hoz létre a Windows 10-es (amely többtényezős Hitelesítést követel meg) és a korábban még nem regisztrált multi-factor Authentication.
- hello szervezet létrehozott és egy MFA regisztrációs szabályzattal, amely alkalmazott toohello felhasználó lett engedélyezve.
- hello felhasználó korábban az MFA szolgáltatásra regisztrált, de döntött, hogy egy hitelesítési módszert, mivel a rendszergazda letiltotta. hello felhasználói ezért kell végighaladnia MFA regisztrációs újra tooselect egy új alapértelmezett ellenőrzési módszert.


## <a name="errors"></a>Hibák
**K: milyen felhasználók tegye, ha azok egy "hitelesítési kérelme, mert nem egy aktivált fiókhoz" hibaüzenet jelenik meg mobilalkalmazáson keresztüli értesítések használata esetén?**

Mondja el nekik toofollow Ez az eljárás tooremove a fiókját hello mobilalkalmazás, majd vegye fel újra:

1. Nyissa meg túl[az Azure portál profil](https://account.activedirectory.windowsazure.com/profile/) , és jelentkezzen be szervezeti fiókjával.
2. Válassza ki **további biztonsági ellenőrzés**.
3. Hello meglévő fiók eltávolításának hello mobilalkalmazás által.
4. Kattintson a **konfigurálása**, és kövesse a hello utasításokat tooreconfigure hello mobilalkalmazás.

**K: milyen felhasználók tegye, ha azok egy 0x800434D4L hibaüzenet jelenik meg bejelentkezéskor tooa böngészőn kívüli alkalmazások?**

hello 0x800434D4L hiba akkor fordul elő, a helyi számítógépet, amely nem működik együtt a kétlépéses ellenőrzést igénylő fiókok telepített tooa böngészőn kívüli alkalmazások a toosign meg.

A megoldás, ezt a hibát, toohave külön felhasználói fiókjainak admin kapcsolatos és a nem rendszergazdai műveletek. Később a rendszergazdai fiókot és a nem rendszergazdai fiókot között postaládák csatolható, így a bejelentkezhet tooOutlook a nem rendszergazdai fiók használatával. További információt ebben a megoldásban, megtudhatja, hogyan túl[adjon egy rendszergazda hello képességét tooopen és nézet hello tartalmát a felhasználói postaláda](http://help.outlook.com/141/gg709759.aspx?sl=1).

## <a name="next-steps"></a>Következő lépések
Ha a kérdését itt nem válaszolt, hagyja alján hello hello hello megjegyzésekben. Vagy a Súgó néhány további lehetőség:

* Keresési hello [Microsoft támogatási tudásbázisát](https://www.microsoft.com/en-us/Search/result.aspx?form=mssupport&q=phonefactor&form=mssupport) megoldások toocommon technikai problémák megoldásához.
* Keresse meg, keresse meg a technikai kérdések és válaszok hello Közösségtől és keresőszavakat saját hello [Azure Active Directory fórumán](https://social.msdn.microsoft.com/Forums/azure/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).
* Ha Ön egy régebbi PhoneFactor felhasználói és kérdése van vagy segítségre van szüksége a jelszó alaphelyzetbe állításával, akkor hello [jelszó-átállítási](mailto:phonefactorsupport@microsoft.com) tooopen támogatási esetet hivatkozásra.
* A támogatási szakember [Azure multi-factor Authentication kiszolgáló (PhoneFactor) támogatási](https://support.microsoft.com/oas/default.aspx?prid=14947). Lépjen kapcsolatba velünk, ha esetén lehet hasznos információt tartalmazhatnak a lehető problémával kapcsolatos. Megadhat olyan információkat tartalmaz, ahol látta hello hiba, hello adott hibakódot, hello adott munkamenet-azonosító és hello azonosító hello hiba látott hello felhasználó hello lapot.
