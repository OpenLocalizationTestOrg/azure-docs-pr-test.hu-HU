---
title: az Azure MFA aaaConfigure |} Microsoft Docs
description: "Ennek az az MFA-val mellett ismerteti, milyen toodo hello Azure multi-factor Authentication hitelesítési lapot.  Ez magában foglalja a jelentések, csalási riasztás, az egyszeri mellőzés, egyéni hangüzenetek, gyorsítótárazás, megbízható IP-címek és az alkalmazás jelszavakat."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 75af734e-4b12-40de-aba4-b68d91064ae8
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/21/2017
ms.author: kgremban
ms.openlocfilehash: 7f6d0b0975a2c1da2de9b52e978b84475c79b218
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-multi-factor-authentication-settings"></a>Azure multi-factor Authentication beállításainak konfigurálása
Ez a cikk segítséget Azure multi-factor Authentication most, hogy működik és elérhető.  Különböző témakörök, amelyek segítenek a lehető legjobban az Azure multi-factor Authentication tooget hello magában foglalja.  Nem minden ezeket a szolgáltatásokat az Azure multi-factor Authentication minden verziójában érhetők el.

| Szolgáltatás | Leírás | 
|:--- |:--- |
| [Csalási riasztás](#fraud-alert) |Csalási riasztás konfigurálhatók, és állítsa be, hogy a felhasználók jelenthetik-e rosszindulatú kísérlet tooaccess erőforrásaikat. |
| [Az egyszeri Mellőzés](#one-time-bypass) |Az egyszeri Mellőzés lehetővé teszi, hogy egy felhasználó tooauthenticate egy alkalommal "kihagyásával" többtényezős hitelesítést. |
| [Egyedi Hangüzenetek](#custom-voice-messages) |Egyedi Hangüzenetek toouse lehetővé teszi a saját felvételek vagy hónap és a többtényezős hitelesítés. |
| [Gyorsítótárazás](#caching-in-azure-multi-factor-authentication) |Gyorsítótárazás lehetővé teszi tooset egy adott időszakra vonatkozóan, hogy a későbbi hitelesítési próbálkozások automatikusan sikeres. |
| [Megbízható IP-címek](#trusted-ips) |Felügyelt vagy összevont bérlő rendszergazdái megbízható IP-címek toobypass kétlépéses ellenőrzés, hogy jelentkezzen be a helyi intranet hello vállalati felhasználók. |
| [Alkalmazásjelszók](#app-passwords) |Az alkalmazásjelszó lehetővé teszi, hogy egy alkalmazás, amely nem MFA-kompatibilis toobypass többtényezős hitelesítést, és folytathatja a munkát. |
| [Ne feledje a multi-factor Authentication a korábban megjegyzett eszközökön és böngészők](#remember-multi-factor-authentication-for-devices-that-users-trust) |Lehetővé teszi tooremember eszközök a meghatározott számú nap elteltével a felhasználó sikeresen bejelentkezett az MFA használata. |
| [Választható hitelesítési módszerek](#selectable-verification-methods) |Lehetővé teszi a toochoose hello hitelesítési módszereket, amelyek a felhasználók toouse érhetők el. |

## <a name="access-hello-azure-mfa-management-portal"></a>Hozzáférés hello Azure MFA-felügyeleti portálon

Ebben a cikkben ismertetett hello szolgáltatások hello Azure multi-factor Authentication kezelési portál konfigurálása. Két módon tooaccess hello multi-factor Authentication kezelési portál hello a klasszikus Azure portálon keresztül. hello először való kezelésekor a többtényezős hitelesítésszolgáltató van. hello második van hello multi-factor Authentication szolgáltatás beállításainak használatával. 

### <a name="use-an-auth-provider"></a>Egy hitelesítésszolgáltató használata

Ha a többtényezős hitelesítésszolgáltató a multi-factor Authentication fogyasztás alapján, használ ezen metódus tooaccess hello a felügyeleti portálon.

tooaccess hello MFA-felügyeleti portálon keresztül az Azure multi-factor Auth Provider, jelentkezzen be hello a klasszikus Azure portálon rendszergazdaként, válassza hello Active Directory lehetőséget. Kattintson a hello **többtényezős hitelesítésszolgáltatók** fülre, majd válassza ki azt a címtárat majd hello **kezelése** hello alsó gomb.

### <a name="use-hello-mfa-service-settings-page"></a>Hello a multi-factor Authentication szolgáltatás beállításai lapon 

Ha a többtényezős hitelesítésszolgáltató vagy egy Azure MFA, Azure AD Premium számára, vagy vállalati mobilitási + biztonsági licenc, használja a metódus tooaccess hello MFA szolgáltatás beállítások lapon.

tooaccess multi-factor Authentication kezelési portál hello hello MFA beállításainak lapján, jelentkezzen be a klasszikus Azure portálon rendszergazdaként hello keresztül, és válassza ki a hello Active Directory lehetőséget. Kattintson arra a címtárra, és kattintson a hello **konfigurálása** fülre. Hello a multi-factor authentication szakaszban válassza ki a **szolgáltatás beállításainak kezelése**. Hello hello multi-factor Authentication szolgáltatás beállításainak lap alsó részén, kattintson a hello **Ugrás toohello portal** hivatkozásra.


## <a name="fraud-alert"></a>Csalási riasztás
Csalási riasztás konfigurálhatók, és állítsa be, hogy a felhasználók jelenthetik-e rosszindulatú kísérlet tooaccess erőforrásaikat.  A felhasználók jelenthetik csalás hello mobilalkalmazás vagy a telefont.

### <a name="set-up-fraud-alert"></a>Csalási riasztás beállítása
1. Keresse meg a multi-factor Authentication kezelési portál toohello / hello utasításokat hello tetején található ezen a lapon.
2. Hello Azure multi-factor Authentication kezelési portál, kattintson **beállítások** hello konfigurálása szakasz alatt.
3. A hello hello-beállítások lap szakasza csalási riasztás, ellenőrizze a hello **engedélyezése a felhasználók toosubmit visszaélési riasztás** jelölőnégyzetet.
4. Válassza ki **mentése** tooapply a módosításokat. 

### <a name="configuration-options"></a>Konfigurációs beállítások

- **Felhasználó blokkolása visszaélés jelentésekor** – Ha egy felhasználó jelentések csalás, a fiók le van tiltva.
- **Csalás során kezdeti köszöntés tooReport Code** -felhasználók általában nyomja meg a # tooconfirm kétlépéses ellenőrzést. Tooreport csalás szeretnék, ha azok kód megadása előtt a # gomb megnyomásával. Ez a kód **0** alapértelmezés szerint azonban testre szabható.

> [!NOTE]
> A Microsoft alapértelmezett hangüdvözléseit kérje meg a felhasználók toopress 0# toosubmit a csalási értesítést. Ha azt szeretné, hogy kódja nem 0 toouse, jegyezze fel, és töltse fel a saját egyéni hangüdvözléseit megfelelő utasításokkal.

![Csalás vonatkozó riasztási beállítások – képernyőkép](./media/multi-factor-authentication-whats-next/fraud.png)

### <a name="how-users-report-fraud"></a>Hogyan felhasználók csalás jelentése 
Csalási riasztás kétféleképpen jelenthetők.  Vagy hello mobilalkalmazás vagy hello telefonon keresztül.  

#### <a name="report-fraud-with-hello-mobile-app"></a>A mobilalkalmazás hello csalás jelentése
1. Egy ellenőrzés tooyour phone küldött, válassza ki azt a tooopen hello Microsoft Authenticator alkalmazást.
2. Válassza ki **Megtagadás** hello értesítésre kattinthat. 
3. Válassza ki **csalás jelentése**.
4. Bezárás hello alkalmazást.

#### <a name="report-fraud-with-a-phone"></a>Visszaélés jelentéséhez a telefon
1. Ha egy ellenőrző hívást tooyour Phone, fogadja a hívást.  
2. tooreport csalás, adja meg a hello visszaéléssel összefüggő kódot (alapértelmezett érték 0), és majd a # jelet hello. Értesítést fog kapni, hogy a csalási értesítést elküldte.
3. Hello hívás befejezéséhez.

### <a name="view-fraud-reports"></a>Csalás jelentések megtekintése
1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).
2. Hello bal oldalon válassza ki az Active Directory.
3. Hello felső válassza ki a **többtényezős hitelesítésszolgáltatók**. Ekkor megjelenik a többtényezős hitelesítésszolgáltatók listáját.
4. Válassza ki a többtényezős hitelesítésszolgáltató, és kattintson a **kezelése** hello lap hello alján. hello Azure multi-factor Authentication kezelési portál megnyitása.
5. Az Azure multi-factor Authentication kezelési portál, A jelentés megtekintése a hello kattintson **csalási riasztás**.
6. Adja meg, hogy kívánja-e tooview hello jelentésben hello dátumtartományt. Megadhatja a felhasználónevek, a telefonszámokat és a hello felhasználói állapotát.
7. Kattintson a **Run** (Futtatás) parancsra. Ekkor megjelenik egy jelentésre az visszaélési riasztás. Kattintson a **tooCSV exportálása** Ha tooexport hello jelentés.

## <a name="one-time-bypass"></a>Az egyszeri Mellőzés
Az egyszeri Mellőzés lehetővé teszi, hogy egy felhasználó tooauthenticate egy alkalommal kétlépéses ellenőrzés végrehajtása nélkül. hello Mellőzés átmeneti, és a megadott számú másodperc után lejár. Olyan esetekben, ahol hello mobilalkalmazás vagy a telefon nem kap egy értesítés, vagy a telefonhívás-engedélyezheti az egyszeri Mellőzés szolgáltatást hello felhasználói számára szükséges hello erőforrás.

### <a name="create-a-one-time-bypass"></a>Az egyszeri Mellőzés létrehozása
1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).
2. Keresse meg a multi-factor Authentication kezelési portál toohello / hello utasításokat hello tetején található ezen a lapon.
3. A hello Azure multi-factor Authentication kezelési portál, ha a bérlő vagy az Azure MFA-szolgáltatóra hello neve megjelenik a hello marad az egy  **+**  következő tooit hello kattintson  **+**  Tekintse meg a különböző MFA kiszolgáló replikációs csoportokhoz és hello Azure alapértelmezett csoport. Válassza ki a megfelelő csoporthoz hello.
4. Válassza ki a felhasználói felügyelet **egyszeri megkerülés**.
5. Hello egyszeri megkerülés lapján kattintson a **új egyszeri megkerülés**.

  ![Felhő](./media/multi-factor-authentication-whats-next/create1.png)

6. Hello felhasználónevét adja meg, a Mellőzés hello másodpercek száma hello létezik, és hello hello mellőzés okát. Kattintson a **áthidaló**.
7. hello határidőn működésbe lép azonnal, így hello felhasználói igények toosign a előtt hello egyszeri Mellőzés lejár. 

### <a name="view-hello-one-time-bypass-report"></a>Nézet hello egyszeri Mellőzés jelentés
1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).
2. Hello bal oldalon válassza ki az Active Directory.
3. Hello felső válassza ki a **többtényezős hitelesítésszolgáltatók**. Ekkor megjelenik a többtényezős hitelesítésszolgáltatók listáját.
4. Válassza ki a többtényezős hitelesítésszolgáltató, és kattintson a **kezelése** hello lap hello alján. hello Azure multi-factor Authentication kezelési portál megnyitása.
5. Az Azure multi-factor Authentication kezelési portál hello hello bal oldali alapján A jelentés megtekintése, kattintson **egyszeri megkerülés**.
6. Adja meg, hogy kívánja-e tooview hello jelentésben hello dátumtartományt. Megadhatja a felhasználónevek, a telefonszámokat és a hello felhasználói állapotát.
7. Kattintson a **Run** (Futtatás) parancsra. Ekkor megjelenik a jelentés a Mellőzés. Kattintson a **tooCSV exportálása** Ha tooexport hello jelentés.

## <a name="custom-voice-messages"></a>Egyedi Hangüzenetek
Egyedi Hangüzenetek toouse lehetővé teszi a saját felvételek vagy a kétlépéses ellenőrzést hónap. Ezek a rekordokban hozzáadása tooor tooreplace hello Microsoft használhatók.

Mielőtt elkezdené hello következő figyelembe vennie:

* hello támogatott formátumok a következők: .wav és .mp3.
* hello méretkorlátot érték 5 MB.
* Hitelesítési üzenetek 20 másodperc karakternél rövidebbnek kell lennie. Hello ellenőrzési toofail semmit ennél hosszabb okozhat, mert hello felhasználói hello üzenet befejeződik, ami hello ellenőrzési tootime kimenő előtt nem válaszol.

### <a name="set-up-a-custom-message"></a>Állítson be egy egyéni üzenet

Nincsenek két részből toocreating az egyéni üzenetet. Először üdvözlőüzenetére feltöltött, és akkor indítsa el a felhasználók számára.

tooupload az egyéni üzenetet:

1. Hozzon létre egy egyéni hangüzenet hello támogatott formátumok valamelyikével.
2. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).
3. Keresse meg a multi-factor Authentication kezelési portál toohello / hello utasításokat hello tetején található ezen a lapon.
4. Hello Azure multi-factor Authentication kezelési portál, kattintson **Hangüzenetek** hello konfigurálása szakasz alatt.
5. A hello konfigurálása: Hangos üzenetek lap, kattintson a **új hangüzenet**.
   ![Felhő](./media/multi-factor-authentication-whats-next/custom1.png)
6. A hello konfigurálása: új hangüzenet lap, kattintson a **hangfájlok kezelése**.
   ![Felhő](./media/multi-factor-authentication-whats-next/custom2.png)
7. A hello konfigurálása: hangkártya fájlok lap, kattintson a **hang-fájl feltöltése**.
   ![Felhő](./media/multi-factor-authentication-whats-next/custom3.png)
8. A hello konfigurálása: hang-fájl feltöltése, kattintson a **Tallózás** , és keresse meg a tooyour hangüzenet, kattintson **nyissa meg**.
9. Adjon meg egy leírást, és kattintson a **feltöltése**.
10. Miután ez befejeződött, az üzenet jelzi, hogy hello fájl sikeresen feltöltötte.

tooturn hello jelenik meg, a felhasználók számára:

1. Hello bal oldalon kattintson **Hangüzenetek**.
2. Csoportban hello Hangüzenetek szakaszban, **új hangüzenet**.
3. A hello nyelvi legördülő válasszon egy nyelvet.
4. Ha ez az üzenet egy adott alkalmazáshoz, adja meg az hello alkalmazás bezárásához.
5. Hello üzenettípus legördülő jelölje ki hello üzenet típusa toobe lesznek felülírva az új egyéni üzenetet.
6. A hangfájl legördülő hello válassza ki a feltöltött hangfájlt hello hello első részében.
7. Kattintson a **Create** (Létrehozás) gombra. Üzenet jelzi, hogy sikeresen létrejött a hangüzenet.
    ![Felhő](./media/multi-factor-authentication-whats-next/custom5.png)</center>

## <a name="caching-in-azure-multi-factor-authentication"></a>Az Azure multi-factor Authentication hitelesítési gyorsítótárazás
Gyorsítótárazás lehetővé teszi tooset egy adott időszakra vonatkozóan, hogy a későbbi hitelesítési próbálkozások adott időn belül automatikusan sikeres. Ez elsősorban a helyszíni rendszerekre, például VPN több ellenőrzési kérés küldött, miközben hello első kérelem még folyamatban van. Ez lehetővé teszi, hogy hello kérelmeknél toosucceed automatikusan után hello felhasználó sikeres hello első ellenőrzése folyamatban. 

Gyorsítótárazás nincs bejelentkezések tooAzure AD használni kívánt toobe.

### <a name="set-up-caching"></a>Gyorsítótárazás beállítása 
1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).
2. Keresse meg a multi-factor Authentication kezelési portál toohello / hello utasításokat hello tetején található ezen a lapon.
3. Hello Azure multi-factor Authentication kezelési portál, kattintson **gyorsítótárazását** hello konfigurálása szakasz alatt.
4. A hello konfigurálása lap gyorsítótárazás kattintson **új gyorsítótár**.
5. Válassza ki a hello gyorsítótártípust és hello másodpercben mért gyorsítótárazási időt. Kattintson a **Create** (Létrehozás) gombra.

<center>![Felhő](./media/multi-factor-authentication-whats-next/cache.png)</center>

## <a name="trusted-ips"></a>Megbízható IP-címek
Megbízható IP-címek csak a rendszergazdák által felügyelt vagy összevont bérlő használható toobypass kétlépéses ellenőrzés olyan felhasználóknál, akik a helyi intranet hello vállalati bejelentkezés az Azure MFA. Ez a szolgáltatás hello Azure multi-factor Authentication nem hello szabad verziója a rendszergazdák teljes verziója érhető el. Hogyan tooget hello Azure multi-factor Authentication teljes verzióját a részletekért lásd: [Azure multi-factor Authentication](multi-factor-authentication.md).

| Az Azure AD-bérlő típusa | Rendelkezésre álló megbízható IP-beállítások |
|:--- |:--- |
| Managed |<li>Adott IP-címtartományok – a rendszergazdák egy adott IP-címek, amelyek jogosultak a kétlépéses ellenőrzést, a felhasználók számára, amely hello vállalati intraneten bejelentkezés adhat meg.</li> |
| Összevont |<li>Minden külső felhasználók – összes összevont felhasználók, akik regisztrálni a a hello belül szervezet mellőzni fog egy AD FS által kiállított jogcímet segítségével kétlépéses ellenőrzést.</li><br><li>Adott IP-címtartományok – a rendszergazdák egy adott IP-címek, amelyek jogosultak a kétlépéses ellenőrzést, a felhasználók számára, amely hello vállalati intraneten bejelentkezés adhat meg. |

Ez kihagyása csak akkor működik a belül egy vállalati intranetből fogadja. Például ha minden összevont felhasználók választotta, és a felhasználó bejelentkezik a külső hello vállalati intraneten, hogy a felhasználó rendelkezik tooauthenticate segítségével kétlépéses hitelesítéssel, még akkor is, ha hello felhasználó bemutat egy AD FS jogcímet. 

**Végfelhasználói élmény a vállalati hálózat belül:**

Megbízható IP-címek is le van tiltva, ha böngésző adatfolyamok szükség a kétlépéses ellenőrzést és alkalmazásjelszók kötelező megadni a régebbi funkciógazdag ügyfélalkalmazások. 

Ha engedélyezve van a megbízható IP-címek, van-e a kétlépéses ellenőrzést *nem* böngésző adatfolyamok szükséges, és hogy alkalmazásjelszókat *nem* szükséges régebbi gazdagügyfél-alkalmazások esetén, feltéve, hogy hello felhasználó még nem már létrehozott egy alkalmazásjelszó. Ha jelszót használja, továbbra is szükséges. 

**Végfelhasználói élmény külső corpnet:**

Megbízható IP-címek engedélyezve van-e, hogy böngésző adatfolyamok szükség a kétlépéses ellenőrzést, és alkalmazásjelszók szükségesek a régebbi funkciógazdag ügyfélalkalmazások. 

### <a name="tooenable-trusted-ips"></a>tooenable megbízható IP-címek
1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).
2. Keresse meg a toohello multi-factor Authentication szolgáltatás beállításainak oldal / Ez a cikk hello elején hello utasításokat.
3. A hello szolgáltatás beállítások lapon, a megbízható IP-címek két lehetőség közül választhat:
   
   * **Az összevont felhasználók intranetről származó kérelmeknél** – hello jelölőnégyzetet. Minden összevont felhasználók bejelentkező hello vállalati hálózatról egy AD FS által kiállított jogcímet segítségével kétlépéses ellenőrzés fog kihagyása.
   * **A nyilvános IP-címek meghatározott számos érkező kéréseket** – hello IP-cím a megadott CIDR jelölésrendszer hello mezőben adja meg. Például: hello tartomány xxx.xxx.xxx.1 – az IP-címekhez xxx.xxx.xxx.0/24 xxx.xxx.xxx.254 vagy xxx.xxx.xxx.xxx/32 egyetlen IP-cím. Másolatot too50 IP-címtartományok adhat meg. Jelentkezzen be az alábbi IP-címekről érkező felhasználók kétlépéses ellenőrzés megkerülését.
4. Kattintson a **Save** (Mentés) gombra.
5. Amikor hello frissítések telepítve lettek, kattintson a **Bezárás**.

![Megbízható IP-címek](./media/multi-factor-authentication-whats-next/trustedips3.png)

## <a name="app-passwords"></a>Alkalmazásjelszók
Apple Mail és alkalmazások, például az Office 2010 vagy korábbi nem támogatják a kétlépéses ellenőrzést. A második ellenőrzési konfigurált tooaccept nem. toouse ezen alkalmazások esetében a hagyományos jelszó helyett toouse "alkalmazásjelszót" van szüksége. hello alkalmazásjelszót lehetővé teszi, hogy a hello alkalmazás toobypass kétlépéses ellenőrzést, és folytathatja a munkát.

> [!NOTE]
> Modern hitelesítéssel az Office 2013 ügyfelek hello
> 
> Office 2013-ügyfelek (például az Outlook) és újabb támogatási modern hitelesítési protokollok és a kétlépéses ellenőrzéshez használttal engedélyezett toowork lehet. Miután engedélyezte őket, alkalmazásjelszók esetén nincs szükség az ügyfelek számára.  További információkért lásd: [Office 2013 modern hitelesítés nyilvános előzetes bejelentette](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

### <a name="important-things-tooknow-about-app-passwords"></a>Tudnivalók az alkalmazásjelszókról fontos részek tooknow
hello egy fontos dolog, amelyet érdemes tudni alkalmazásjelszókról listája látható.

* Az alkalmazásjelszók csak alkalmazásonként egyszer megadott toobe van szükség. Felhasználók a nem rendelkezik tookeep nyomon őket, és minden alkalommal megadniuk azokat.
* hello tényleges jelszót automatikusan jön létre, és nem hello felhasználó által megadott. Ennek az oka, hogy egy támadó tooguess nehezebben hello automatikusan generált jelszót, és biztonságosabb.
* Egy legfeljebb 40 jelszó felhasználónként van. 
* A gyorsítótár-jelszavak és használja azt a helyszíni forgatókönyvekben előfordulhat, hogy start működni, mert hello alkalmazásjelszót hello szervezeti azonosítóval kívül nem ismert alkalmazásokat. Példa: a helyszíni Exchange e-maileket, de hello archivált levelek hello felhőben van. hello ugyanazt a jelszót nem működik.
* Ha a multi-factor authentication felhasználói fiók engedélyezve van, például az Outlook és a Lync legtöbb nem böngésző típusú ügyfeleken alkalmazásjelszót használható, de felügyeleti művelet nem hajtható végre, alkalmazásjelszókat a böngészőn kívüli alkalmazások, például a Windows PowerShell használatával, még akkor is, ha a rendszergazda fiókja van, hogy a felhasználó.  Győződjön meg arról, hozzon létre egy erős jelszót toorun PowerShell-parancsfájlok szolgáltatásfiók, és ne engedélyezze a kétlépéses ellenőrzéshez fiók.

> [!WARNING]
> Alkalmazásjelszók hibrid környezetekben, ahol az ügyfelek kommunikálni mind a helyszíni és a felhőalapú automatikus észlelési végpontok nem működnek. Ennek az az oka a tartományi jelszó szükséges tooauthenticate a helyszíni, illetve alkalmazásjelszók szükséges tooauthenticate hello felhővel.

### <a name="naming-guidance-for-app-passwords"></a>Alkalmazásjelszók elnevezési útmutató
Jelszó alkalmazásnevek tükröznie kell hello eszköz, amelyre használhatók. Például ha a hordozható számítógép, amelyen a böngészőn kívüli alkalmazások, például az Outlook, a Word és Excel, hozzon létre egy, az Laptop elnevezésű alkalmazásjelszót, és azt a ezeket az alkalmazásokat. Ezután hozzon létre egy másik jelszót nevű asztali hello az asztali számítógép ugyanazon alkalmazásokat. 

A Microsoft azt javasolja, hogy nem egy alkalmazásjelszót alkalmazásonként eszközönként egy alkalmazásjelszót létrehozása.

### <a name="federated-sso-app-passwords"></a>Alkalmazásjelszók összevont (SSO)
Az Azure AD összevonás (egyszeri bejelentkezés) az a helyi Windows Server Active Directory tartományi szolgáltatások (AD DS) támogatja. Ha a szervezet össze van vonva az Azure AD és az Azure multi-factor Authentication használatának toobe fog, majd hello alkalmazásjelszók kapcsolatos alábbi információkat fontos meg. Ez a szakasz a toofederated (SSO) az ügyfelek csak vonatkozik.

* Alkalmazásjelszók az Azure AD által ellenőrzött, és ezért az összevonási megkerülése. Összevonási csak aktívan használt alkalmazásjelszók beállítása során.
* Az összevont felhasználókra (SSO) azt soha nem lépjen a toohello identitásszolgáltató (IdP) eltérően hello passzív folyammal. hello jelszavak hello szervezeti azonosító tárolja. Ha hello felhasználó elhagyja a hello vállalati, adott információ kapcsolatazonosítója tooflow tooorganizational valós időben a DirSync használatával. Fiók letiltása/törlése toothree óra toosync, ami késlelteti az alkalmazásjelszó letiltását/törlését az Azure ad-ben is tarthat.
* Az alkalmazásjelszó nem tartja be a helyszíni ügyfél hozzáférés-vezérlési beállításait.
* Az alkalmazásjelszó nincs a helyszíni hitelesítés naplózási/naplózási képesség érhető el.
* Bizonyos speciális architekturális terveket is szervezeti felhasználónévvel és a jelszavak és a jelszó kérése, amikor segítségével kétlépéses hitelesítéssel rendelkező ügyfelek, attól függően, hogy hol hitelesítéshez. A hitelesítést, a helyszíni infrastruktúra-ügyfelek esetében használja a szervezeti felhasználónévvel és jelszóval. A hitelesítést, az Azure AD-ügyfelek esetében használja a hello alkalmazásjelszót.

  Tegyük fel például, egy architektúra, amely a következő hello tevődik össze:

  * Összevonja a helyszíni példányát az Active Directory, az Azure ad szolgáltatással
  * Exchange online használ
  * Lync, amely kifejezetten a helyszíni használ
  * Azure multi-factor Authentication hitelesítés

  ![Proofup](./media/multi-factor-authentication-whats-next/federated.png)

  Ezekben az esetekben, tegye a következőket hello:

  * Bejelentkezés – tooLync, használja a szervezet felhasználónevet és jelszót.
  * Tooaccess hello címjegyzék keresztül az Outlook ügyfélprogram, amely a tooExchange online megkísérelte egy alkalmazásjelszót használnak.

### <a name="allow-app-password-creation"></a>Alkalmazás jelszó létrehozásának engedélyezése
Alapértelmezés szerint a felhasználók nem hozhatják létre alkalmazásjelszókat. Ezt a szolgáltatást engedélyezni kell. tooallow felhasználók hello képességét toocreate alkalmazásjelszókat, használja a következő eljárás hello:

1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).
2. Keresse meg a toohello multi-factor Authentication szolgáltatás beállításainak oldal / Ez a cikk hello elején hello utasításokat.
3. Hello választógombját bejelölve mellett túl**engedélyezése a felhasználók toocreate app jelszavak toosign nem böngészőben megjelenő alkalmazásokba**.

![Alkalmazásjelszók létrehozása](./media/multi-factor-authentication-whats-next/trustedips3.png)

### <a name="create-app-passwords"></a>Alkalmazásjelszók létrehozása
Felhasználók hozhatnak létre alkalmazásjelszókat a kezdeti regisztráció során. Hello regisztrációs folyamat során, ami lehetővé tenné toocreate alkalmazásjelszók hello végén egy lehetőség, számukra.

Felhasználók emellett létrehozhatják alkalmazásjelszók regisztrálás után a hello Azure portál vagy hello Office 365 portál beállításait. További információk és részletes lépéseket a felhasználók számára: [Mik az Azure multi-factor Authentication alkalmazásjelszók](./end-user/multi-factor-authentication-end-user-app-passwords.md).

## <a name="remember-multi-factor-authentication-for-devices-that-users-trust"></a>Ne feledje a multi-factor Authentication-eszközök számára megbízható felhasználók
A multi-factor Authentication megjegyzése böngészők, hogy felhasználók bizalmi kapcsolat lehetővé teszi az ingyenes minden multi-factor Authentication-felhasználók és eszközök esetében. Toogive felhasználók hello beállítás lehetővé teszi tooby-fázis MFA használata az adott számú napon egy sikeres végrehajtása után jelentkezzen be többtényezős hitelesítés használatával. Ez is javító hello szám, ahányszor a felhasználó elvégezheti a kapcsolódó kétlépéses ellenőrzés hello csökkentésével ugyanarra az eszközre.

Azonban ha egy fiók vagy eszköz biztonsága sérül, MFA jelszóelőzmények megbízható eszközök hatással lehet a biztonságra. Ha egy vállalati fiók biztonságának sérülése esetén vagy egy megbízható eszköz elvesztésekor vagy ellopásakor, akkor [többtényezős hitelesítés visszaállítása az összes eszközön](multi-factor-authentication-manage-users-and-devices.md#restore-mfa-on-all-remembered-devices-for-a-user). Ezzel a művelettel visszavonja az összes eszköz megbízható hello állapotát, és szükséges tooperform kétlépéses ellenőrzés visszakapcsolásához hello felhasználó. Azt is beállíthatja, hogy a felhasználók toorestore MFA a saját eszközeik hello utasításokat tartalmaz a [a kétlépéses ellenőrzést beállításainak kezelése](./end-user/multi-factor-authentication-end-user-manage-settings.md#require-two-step-verification-again-on-a-device-youve-marked-as-trusted)

### <a name="how-it-works"></a>Működés

Jelszóelőzmények a multi-factor Authentication működik beállításával állandó cookie a hello böngésző, ha egy felhasználó hello "Ne jelenjen meg többé a **X** nap" bejelentkezéskor a mezőbe. hello nem lehet kérni fogja a felhasználótól a multi-factor Authentication újra az adott Browser amíg hello cookie lejár. Ha hello felhasználó megnyit egy másik böngészőben hello egy eszközre vagy törli a cookie-kat,-e rákérdezéses tooverify újra. 

hello "Ne jelenjen meg többé a **X** nap" jelölőnégyzet nem jelenik meg a böngészőn kívüli alkalmazásokat, a modern hitelesítést támogatják-e. Ezeket az alkalmazásokat, amelyek új hozzáférési jogkivonatok óránként biztosítanak frissítési jogkivonatokat használja. Egy frissítési jogkivonat ellenőrzését, az Azure AD ellenőrzi, hogy a legutóbbi alkalommal kétlépéses ellenőrzés hello lett végre lett konfigurálva hello számú napon belül. 

Ezért tárolása a többtényezős hitelesítés a megbízható eszközökön csökkenti a hello számát webalkalmazásokban (amely általában kérni minden alkalommal), de növeli hello (amely általában kéri a 90 naponta) modern-hitelesítési ügyfelek számát.

> [!NOTE]
>Ez a szolgáltatás esetén nem kompatibilis a hello a "Bejelentkezve szeretnék maradni" az AD FS szolgáltatás felhasználók hajtsa végre a kétlépéses ellenőrzés az AD FS keresztül hello Azure MFA kiszolgáló vagy egy külső MFA-megoldását. Válassza ki a "Bejelentkezve szeretnék maradni" az AD FS-ben és a is megjelölése megbízhatóként eszközüket a multi-factor Authentication a felhasználók, ha azok nem lesznek képesek tooverify hello "Megjegyzése MFA" napok számát, amelyek lejárata után is. Az Azure AD egy friss kétlépéses ellenőrzést igényel, de az AD FS hello eredeti MFA jogcím és a kétlépéses ellenőrzés visszakapcsolásához végzett helyett dátummal jogkivonatot ad vissza. Ez állítja ki az Azure közötti ellenőrzési hurkot AD és az AD FS. 

### <a name="enable-remember-multi-factor-authentication"></a>Ne feledje többtényezős hitelesítés engedélyezése
1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).
2. Keresse meg a toohello multi-factor Authentication szolgáltatás beállításainak oldal / Ez a cikk hello elején hello utasításokat.
3. Hello szolgáltatás beállításai lapon, a felhasználói beállítások kezelése, tekintse meg hello **engedélyezése a felhasználók tooremember többtényezős hitelesítés a megbízható eszközökön** mezőbe.
   ![Ne feledje eszközök](./media/multi-factor-authentication-whats-next/remember.png)
4. Adja meg a hello hány nappal, amelyet az tooallow hello megbízható eszközök toobypass kétlépéses ellenőrzést. hello alapértelmezett érték 14 nap.
5. Kattintson a **Save** (Mentés) gombra.
6. Kattintson a **Bezárás** gombra.

### <a name="mark-a-device-as-trusted"></a>Jelölje meg megbízhatóként eszköz

Ha engedélyezi ezt a szolgáltatást, felhasználók jelölheti meg, ha megbízható eszköz ellenőrzésével bejelentkeznek **rákérdezés nélkül**.

![Ne jelenjen meg többé – képernyőkép](./media/multi-factor-authentication-whats-next/trusted.png)

## <a name="selectable-verification-methods"></a>Választható hitelesítési módszerek
Kiválaszthatja, hogy mely hitelesítési módszerek állnak rendelkezésre a felhasználók számára. az alábbi táblázat hello rövid áttekintést nyújt az egyes módszerek.

Amikor a felhasználók beléptetik a fiókjukat a multi-factor Authentication, választ ki, hogy engedélyezte a hello-beállítások az előnyben részesített ellenőrzési módszert. a regisztrációs folyamat hello útmutatást is ismertetjük [a kétlépéses ellenőrzéshez a fiók beállítása](multi-factor-authentication-end-user-first-time.md)

| Módszer | Leírás |
|:--- |:--- |
| Hívás toophone |Egy automatizált hang hívás helyezi. hello felhasználói válaszok hello hívja, és a hello telefon billentyűzetén tooauthenticate kell nyomnia a #. Ez a telefonszám nem szinkronizált tooon helyszíni Active Directoryba. |
| Szöveges üzenet toophone |Egy megerősítési kódot tartalmazó szöveges üzenetet küld. hello felhasználói az felszólító tooeither válasz toohello szöveges üzenet hello ellenőrzőkódot vagy tooenter hello ellenőrzőkódot hello bejelentkezési felületén. |
| Mobilalkalmazás használatával értesítést |A leküldéses értesítési tooyour telefonokat és a regisztrált eszközre elküldi. hello felhasználói hello értesítési megtekintése és kijelöli **ellenőrizze** toocomplete ellenőrzése. <br>hello Microsoft Authenticator alkalmazás érhető el [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), és [IOS](http://go.microsoft.com/fwlink/?Linkid=825073). |
| Mobilalkalmazás ellenőrzőkódja |hello Microsoft Authenticator alkalmazást hoz létre egy új OATH-ellenőrző kódot 30 másodpercenként. hello felhasználó eléri ezt a megerősítési kódot hello bejelentkezési felületén.<br>hello Microsoft Authenticator alkalmazás érhető el [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), és [IOS](http://go.microsoft.com/fwlink/?Linkid=825073). |

### <a name="how-tooenabledisable-authentication-methods"></a>Hogyan tooenable vagy letiltását hitelesítési módszerek
1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).
2. Keresse meg a toohello multi-factor Authentication szolgáltatás beállításainak oldal / Ez a cikk hello elején hello utasításokat.
3. Hello szolgáltatás beállításai lapon a ellenőrzési beállítások kiválasztása vagy eltávolításához toouse kívánja hello-beállítások.
   ![Ellenőrzési lehetőségek](./media/multi-factor-authentication-whats-next/authmethods.png)
4. Kattintson a **Save** (Mentés) gombra.
5. Kattintson a **Bezárás** gombra.

