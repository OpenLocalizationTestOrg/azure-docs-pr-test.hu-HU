---
title: "aaaHow tooenable az adal-t használó Android alkalmazások közötti SSO |} Microsoft Docs"
description: "Hogyan toouse hello funkcióit hello az alkalmazások között az egyszeri bejelentkezés ADAL SDK tooenable. "
services: active-directory
documentationcenter: 
author: danieldobalian
manager: mbaldwin
editor: 
ms.assetid: 40710225-05ab-40a3-9aec-8b4e96b6b5e7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: android
ms.devlang: java
ms.topic: article
ms.date: 04/07/2017
ms.author: dadobali
ms.custom: aaddev
ms.openlocfilehash: 3867e15030e5516464e4dbd92ba35894430daf00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooenable-cross-app-sso-on-android-using-adal"></a>Hogyan tooenable alkalmazások közötti SSO-ADAL használatával Android rendszeren
Egyszeri bejelentkezés (SSO) biztosít, így a felhasználók csak kell tooenter hitelesítő adataikkal egyszer, és ezen hitelesítő adatok automatikusan munkahelyi rendelkezik a különböző alkalmazások most várt ügyfelek. hello nehéz kis képernyőjű, gyakran alkalommal együtt egy további tényezőt (2FA) például telefonhívást vagy egy fogva kódot, a felhasználónév és jelszó megadása eredményezi, ha egy felhasználó gyors kapcsolatos van toodo ennek a termékhez egynél többször.

Ezenkívül ha alkalmaz egy identitás-platform, amely más alkalmazások is használhatnak, például a Microsoft Accounts vagy az Office365 munkahelyi fiókkal, az ügyfelek várt, hogy ezen hitelesítő adatok toobe elérhető toouse az alkalmazások között nem számít, hello szállító.

hello Microsoft Identity platform, a Microsoft Identity SDK-k, valamint elvégzi a rögzített munka, és képes toodelight egyszeri Bejelentkezést, vagy az alkalmazások vagy, mint a saját suite belül az ügyfelek hello biztosítja a broker funkció és a hitelesítő az alkalmazások, az eszköz teljes hello.

Ez a forgatókönyv megtudhatja, hogyan tooconfigure az SDK az alkalmazás tooprovide belül a juttatás tooyour ügyfelek.

Ez a forgatókönyv a következőkre vonatkozik:

* Azure Active Directory
* Azure Active Directory B2C
* Az Azure Active Directory B2B
* Azure Active Directory feltételes hozzáférés

hello előző lépéseknél túl hogyan tudja[alkalmazások hello örökölt portálon az Azure Active Directory kiépítése](active-directory-how-to-integrate.md) és az alkalmazás integrálható hello [Microsoft Identity Android SDK](https://github.com/AzureAD/azure-activedirectory-library-for-android) .

## <a name="sso-concepts-in-hello-microsoft-identity-platform"></a>A Microsoft Identity Platform hello SSO fogalmak
### <a name="microsoft-identity-brokers"></a>Microsoft – identitáskezelési brókerek
A Microsoft lehetővé teszi a minden mobileszköz platform, amelyek lehetővé teszik a különböző alkalmazások különböző szállítóktól származó hello hídképzést, a hitelesítő adatok, és lehetővé teszi, hogy honnan egy biztonságos helyen igénylő különleges továbbfejlesztett funkciók toovalidate hitelesítő adatokat. Ezek nevezzük **brókerek**. IOS és Android rendszeren ezek szolgáltatáson keresztül letölthető alkalmazás, hogy az ügyfelek telepítése függetlenül vagy egy vállalati, akár az összeset hello eszköz az alkalmazott felügyelő a toohello eszköz továbbíthatja. Ezek a brókerek kezelése biztonsági csak az egyes alkalmazások vagy hello: az eszköz teljes alapján a rendszergazdák törlését támogatja. A Windows rendszerben a funkcióit egy toohello operációs rendszerben, technikailag néven hello Webeshitelesítés-szervező beépített fiók kiválasztásakor.

További információt használjuk, a brókerek, és hogyan az ügyfelek előfordulhat, hogy lássák a bejelentkezési folyamat hello Microsoft Identity platform olvasni.

### <a name="patterns-for-logging-in-on-mobile-devices"></a>Bejelentkezés mobileszközök minták
Hozzáférés toocredentials eszközökön két alapvető mintázatokból hello Microsoft Identity platform kövesse:

* Nem közvetített támogatott bejelentkezések
* Támogatott bejelentkezések replikaszervező

#### <a name="non-broker-assisted-logins"></a>Nem közvetített támogatott bejelentkezések
Nem közvetített támogatott bejelentkezések hello alkalmazás együtt, beágyazottan történik, és hello helyi tárhelyet használó hello eszközön az alkalmazás bejelentkezési élmény. Ez a tároló lehet osztani alkalmazásra, de hello hitelesítő adatok szorosan kötött toohello alkalmazás vagy csomag alkalmazásaikat, hogy a hitelesítő adatok. Valószínűleg tapasztal Ez sok mobilalkalmazások a felhasználónév és jelszó maga hello alkalmazásban beírásakor.

Ezek bejelentkezések a következő előnyöket hello rendelkezik:

* Felhasználói élmény létezik-e teljes mértékben hello alkalmazáson belül.
* Hitelesítő adatok meg lehet osztani összes hello által aláírt alkalmazások ugyanazt a tanúsítványt, egy egyszeri bejelentkezéses élményt tooyour suite alkalmazások biztosítása.
* Bejelentkezés hello élménye körül vezérlő toohello alkalmazás előtt és után bejelentkezhet valósul meg.

Ezek bejelentkezések a következő hátrányokkal hello rendelkezik:

* A felhasználó csak azokat az alkalmazás van-e beállítva a Microsoft Identities között a Microsoft Identity használó összes alkalmazások között nem számára az egyszeri bejelentkezést.
* Az alkalmazás nem használható például a feltételes hozzáférés, vagy használjon hello InTune termékcsomagot speciális üzleti szolgáltatásokat.
* Az alkalmazás nem támogatja a Tanúsítványalapú hitelesítés üzleti felhasználók számára.

Íme egy hello Microsoft Identity SDK-k működése megosztott hello tárolására az alkalmazások tooenable SSO ábrázolása:

```
+------------+ +------------+  +-------------+
|            | |            |  |             |
|   App 1    | |   App 2    |  |   App 3     |
|            | |            |  |             |
|            | |            |  |             |
+------------+ +------------+  +-------------+
| Azure SDK  | | Azure SDK  |  | Azure SDK   |
+------------+-+------------+--+-------------+
|                                            |
|            App Shared Storage              |
+--------------------------------------------+
```

#### <a name="broker-assisted-logins"></a>Támogatott bejelentkezések replikaszervező
Bejelentkezések Broker támogatású bejelentkezési élmény hello broker alkalmazáson belül és hello tárolási és biztonsági hello broker tooshare hitelesítő adatok használata minden alkalmazásra hello eszközön, amelyek érvényesek a Microsoft Identity platform hello. Ez azt jelenti, hogy az alkalmazások támaszkodnak hello broker toosign felhasználók számára. IOS és Android rendszeren a brókerek szolgáltatáson keresztül letölthető alkalmazás, hogy az ügyfelek telepítése függetlenül vagy olyan cég, amely kezeli a felhasználó hello az eszközön a toohello eszköz továbbíthatja. Ez az alkalmazástípus példa: hello Microsoft Authenticator alkalmazás IOS rendszerű eszközökön. A Windows e funkcióit egy toohello operációs rendszerben, technikailag néven hello Webeshitelesítés-szervező beépített fiók kiválasztásakor.
hello élmény platformonként változó, és előfordulhat zavaró toousers nem esetén megfelelően. Ismeri valószínűleg legtöbb ebben a mintában Ha hello Facebook-alkalmazás telepítve, és Facebook csatlakozni egy másik alkalmazás használja. hello Microsoft – identitáskezelési platform által használt hello ugyanilyen mintájú.

Az IOS-es ennek eredménye tooa "átmenet" animáció ahol az alkalmazás küldése közben hello Microsoft Authenticator alkalmazások toohello háttér elérhető lesz a hello felhasználói tooselect toohello előtér melyik fiókot szeretné toosign be.  

Android és Windows hello fiók kiválasztásakor az alkalmazás, amely kevésbé zavaró toohello felhasználói felett jelenik meg.

#### <a name="how-hello-broker-gets-invoked"></a>Hogyan hello broker meghívott beolvasása
Ha egy kompatibilis broker hello eszköz, például Microsoft Authenticator alkalmazást, a Microsoft Identity SDK-k hello hello automatikusan telepítve van a hello hívásának hello broker meg, amikor a felhasználó azt jelzi, hogy kívánják toolog bármely fiókkal való munka hello Microsoft Identity platform. Ez a fiók lehet személyes Microsoft-Account, munkahelyi vagy iskolai fiókját, vagy egy fiókot, amely megadja és az Azure-ban B2C és B2B termékeink állomás. 
 
 #### <a name="how-we-ensure-hello-application-is-valid"></a>Hogyan biztosítható hello alkalmazás érvénytelen.
 
 hello kell tooensure hello identitását egy alkalmazás hívás hello broker nyújtunk a támogatott broker bejelentkezési adatok számára elengedhetetlen toohello biztonsági. IOS és Android nem kényszeríti az egyedi azonosítók, amelyek csak egy adott alkalmazáshoz tartozó érvényes, így rosszindulatú alkalmazások előfordulhat, hogy "hamis" a jogos alkalmazásazonosító, és azt jelentette, hogy hello jogos alkalmazás hello jogkivonatokat fogadni. mindig tájékoztatjuk futásidőben hello jobb alkalmazással tooensure, kérjük hello fejlesztői tooprovide egyéni redirectURI Microsoft az alkalmazás regisztrálásakor. **Az alábbiakban tárgyalt hogyan a fejlesztők kézműipari az átirányítási URI-t kell.** A egyéni redirectURI hello tanúsítvány ujjlenyomata hello alkalmazás tartalmazza, és a Google Play áruház hello toobe egyedi toohello alkalmazás biztosítja. Amikor egy alkalmazás meghívja hello broker hello broker kéri hello Android operációs rendszer tooprovide hello a tanúsítvány ujjlenyomata a hívott hello broker. hello broker hello hívás tooour identitásrendszere a tanúsítvány ujjlenyomata tooMicrosoft nyújt. Ha hello alkalmazás ujjlenyomat nem egyezik meg a tanúsítvány-ujjlenyomat hello hello tanúsítvány a regisztráció során megadott toous hello fejlesztő, azt megtagadja a hozzáférést toohello jogkivonatok hello erőforrás hello alkalmazás számára. Ez az ellenőrzés biztosítja, hogy csak a regisztrált hello fejlesztő hello alkalmazás-jogkivonatokat kap.

**hello fejlesztői hello választott rendelkezik, ha a Microsoft Identity SDK hello hello broker meghívja vagy hello nem közvetített támogatott folyamat használja.** Azonban ha hello fejlesztői nem toouse hello broker támogatású folyamata megszakad a hello előnye, hogy egyszeri bejelentkezési hitelesítő adataival, hogy hello a felhasználó már valószínűleg hello eszközön, és megakadályozza, hogy az alkalmazás használatát a Microsoft üzleti szolgáltatásokkal az ügyfelek, például a feltételes hozzáférés, a Intune kezelési képességek és a tanúsítvány alapú hitelesítést nyújt.

Ezek bejelentkezések a következő előnyöket hello rendelkezik:

* Felhasználói hello szállító függetlenül az alkalmazások közötti SSO észlel.
* Az alkalmazás használhatja a Speciális üzleti szolgáltatásokat, mint a feltételes hozzáférés, de hello InTune termékcsomagot használja.
* Az alkalmazás tanúsítvány alapú hitelesítést is támogatja az üzleti felhasználók.
* Sokkal biztonságosabb bejelentkezés felhasználói élmény, hello alkalmazás- és hello felhasználói hello identitásának ellenőrzése hello broker alkalmazás további biztonsági algoritmusokkal és a titkosítás.

Ezek bejelentkezések a következő hátrányokkal hello rendelkezik:

* IOS-hello felhasználó van állapotra váltott ki az alkalmazás élmény közben a hitelesítő adatok közül választ.
* Hello képességét toomanage hello bejelentkezési megszűnését tapasztalhat az ügyfeleknek az alkalmazáson belül.

Íme egy hogyan hello Microsoft Identity SDK-k használata hello replikaszervező alkalmazások tooenable SSO ábrázolása:

```
+------------+ +------------+   +-------------+
|            | |            |   |             |
|   App 1    | |   App 2    |   |   Someone   |
|            | |            |   |    Else's   |
|            | |            |   |     App     |
+------------+ +------------+   +-------------+
|  ADAL SDK  | |  ADAL SDK  |   |  ADAL SDK   |
+-----+------+-+-----+------+-  +-------+-----+
      |              |                  |
      |       +------v------+           |
      |       |             |           |
      |       | Microsoft   |           |
      +-------> Broker      |^----------+
              | Application
              |             |
              +-------------+
              |             |
              |   Broker    |
              |   Storage   |
              |             |
              +-------------+

```

Volt képes a háttér-információkat felvértezve meg kell tudni toobetter megismerheti és alkalmazhatja a SSO hello Microsoft Identity platform és SDK-k segítségével az alkalmazáson belül.

## <a name="enabling-cross-app-sso-using-adal"></a>Adal-t használó alkalmazások közötti SSO engedélyezése
Jelen példában használjuk hello ADAL Android SDK számára:

* Nem közvetített támogatott egyszeri Bejelentkezést a csomag az alkalmazások bekapcsolása
* Kapcsolja be az SSO broker támogatású támogatása

### <a name="turning-on-sso-for-non-broker-assisted-sso"></a>Nem közvetített egyszeri Bejelentkezést bekapcsolás támogatott az egyszeri bejelentkezés
Nem közvetített támogatott egyszeri bejelentkezéshez alkalmazásra hello Microsoft Identity SDK-k kezelése nagy részét SSO hello összetettsége meg. Ez magában foglalja a hello gyorsítótár hello jobb felhasználói keresése és karbantartása az Ön tooquery bejelentkezett felhasználók listáját.

egyszeri bejelentkezés tooenable alkalmazásra van szüksége a következő toodo hello saját:

1. Győződjön meg arról az összes az alkalmazások felhasználói hello ugyanazon ügyfél-Azonosítót vagy az alkalmazás azonosítóját.
2. Győződjön meg arról az alkalmazások hello azonos SharedUserID beállítása.
3. Ügyeljen arra, hogy az alkalmazások megosztás hello hello Google Play azonos aláíró tanúsítványt tárolni, így a tárolás megoszthatja a.

#### <a name="step-1-using-hello-same-client-id--application-id-for-all-hello-applications-in-your-suite-of-apps"></a>1. lépés: Használatával hello ugyanazon ügyfél-azonosító / Application ID, az összes hello az alkalmazások a csomagban található alkalmazások
Hello Microsoft Identity platform tooknow ahhoz, hogy az alkalmazások között tooshare jogkivonatok engedélyezett az alkalmazások egyes kell tooshare hello ugyanazon ügyfél-Azonosítót vagy az alkalmazás azonosítóját. Ez az egyedi azonosító hello tooyou megadott hello portálon az első alkalmazás regisztrálásakor.

Előfordulhat, hogy lehet szeretné megtudni, hogy hogyan határozható meg különböző alkalmazások toohello Ha használja a Microsoft Identity service hello ugyanazon alkalmazás azonosítóját. hello válasz van hello **átirányítási URI-azonosítók**. Minden alkalmazás több átirányítási URI-azonosítók hello bevezetési portálon regisztrált rendelkezhet. A csomagban található összes alkalmazás egy másik átirányítási URI-t fog rendelkezni. Ez megjelenésének például nem éri el:

Az App1 átirányítási URI-ja:`msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D`

App2 átirányítási URI-ja:`msauth://com.example.userapp1/KmB7PxIytyLkbGHuI%2UitkW%2Fejk%4E`

App3 átirányítási URI-ja:`msauth://com.example.userapp2/Pt85PxIyvbLkbKUtBI%2SitkW%2Fejk%9F`

....

Ezek a beágyazott hello ugyanazon ügyfél-azonosító / Alkalmazásazonosítót és alapján hello kereshető átirányítási URI-címe toous SDK konfigurációs adja vissza.

```
+-------------------+
|                   |
|  Client ID        |
+---------+---------+
          |
          |           +-----------------------------------+
          |           |  App 1 Redirect URI               |
          +----------^+                                   |
          |           +-----------------------------------+
          |
          |           +-----------------------------------+
          +----------^+  App 2 Redirect URI               |
          |           |                                   |
          |           +-----------------------------------+
          |
          +----------^+-----------------------------------+
                      |  App 3 Redirect URI               |
                      |                                   |
                      +-----------------------------------+

```


*Vegye figyelembe, hogy az átirányítási URI-k formátuma hello ismertetésre vannak. Minden átirányítási URI-t használhatja kivéve toosupport hello broker kívánja, ebben az esetben azok kell kinéznie például a fenti hello*

#### <a name="step-2-configuring-shared-storage-in-android"></a>2. lépés: Az Android megosztott tárolás konfigurálása
A beállítás hello `SharedUserID` nem ez a dokumentum hello terjed, de hello hello Google Android rendszerhez dokumentáció olvasásával ismernie kell [Manifest](http://developer.android.com/guide/topics/manifest/manifest-element.html). Fontos, hogy úgy dönt, hogy mi a sharedUserID hívja meg a rendszer, és azt használja az alkalmazások között.

Ha elvégezte a hello `SharedUserID` az összes alkalmazás készen áll a toouse SSO.

> [!WARNING]
> Ha tárolási megosztani az alkalmazások bármely alkalmazás törölheti felhasználók, vagy rosszabb összes hello jogkivonatot az alkalmazás minden részében. Ez a különösen katasztrofális, ha a hello jogkivonatok toodo háttérműveletek használó alkalmazásokat. Megosztása azt jelenti, hogy a Microsoft Identity SDK-k hello keresztül minden eltávolítása műveletek nagyon gondosan kell.
> 
> 

Ennyi az egész! a Microsoft Identity SDK hello most fájlmegosztási hitelesítő adatokat az alkalmazások között. hello Felhasználólista lesznek megosztva alkalmazáspéldányok között.

### <a name="turning-on-sso-for-broker-assisted-sso"></a>Egyszeri bejelentkezés SSO Broker bekapcsolása esetén támogatott.
az egy bármely Broker összetevője hello eszközön telepített alkalmazás toouse képesek hello **alapértelmezés szerint ki van kapcsolva**. A rendezés toouse az alkalmazás a hello broker kell tegye néhány további beállításra, és néhány kódot tooyour alkalmazás hozzáadása.

hello lépéseket toofollow a következők:

1. Az alkalmazás kódjában hívás toohello MS SDK broker mód engedélyezése
2. Állítson be egy új átirányítási URI-t, és tooboth hello alkalmazást és az alkalmazás regisztrálása
3. Hello megfelelő engedélyeket az Android-jegyzékfájlból hello beállítása

#### <a name="step-1-enable-broker-mode-in-your-application"></a>1. lépés: Az alkalmazás broker üzemmód engedélyezése
az alkalmazás toouse hello Broker hello lehetősége van kapcsolva, hello "beállítások" vagy a kezdeti beállítás a hitelesítési példány létrehozásakor. Ehhez a ApplicationSettings beállítástípus a kódban:

```
AuthenticationSettings.Instance.setUseBroker(true);
```


#### <a name="step-2-establish-a-new-redirect-uri-with-your-url-scheme"></a>2. lépés: Egy új átirányítási URI-t és az URL-séma létrehozása
A sorrend tooensure, hogy a rendszer mindig visszaadja a hello credential jogkivonatok toohello megfelelő alkalmazás igazolnia kell a toomake meg arról, hogy a telepítésnek vissza oly módon, hogy az Android operációs rendszer hello tooyour alkalmazás ellenőrizheti. hello Android operációs rendszer hello tanúsítvány hello kivonatának hello Google Play áruház használja. Ez a nem egy engedélyezetlen alkalmazás megtévesztésre. Ezért azt használja ki a hello az, hogy a hello jogkivonatok kerüljenek-e vissza a megfelelő alkalmazás toohello broker alkalmazás tooensure URI-val együtt. Kérjük, tooestablish ez egyedi átirányítási URI-t, mind az alkalmazás és egy átirányítási URI-t, a developer portálon.

Az átirányítási URI-t a hello megfelelő formátumban kell lennie:

`msauth://packagename/Base64UrlencodedSignature`

például: *msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D*

Az átirányítási URI-t kell az alkalmazás-regisztrációk hello megadott toobe [Azure-portálon](https://portal.azure.com/). További információ az Azure AD alkalmazás-regisztrációval, lásd: [integrálása az Azure Active Directoryval](active-directory-how-to-integrate.md).

#### <a name="step-3-set-up-hello-correct-permissions-in-your-application"></a>3. lépés: Hello megfelelő engedélyekkel az alkalmazás beállítása
Az Android broker alkalmazás alkalmazások hello fiókkezelő szolgáltatás hello Android operációs rendszer toomanage hitelesítő adatokat használ. Az alkalmazás jegyzékének rendelés toouse hello broker Android engedélyek toouse AccountManager fiókokat kell rendelkeznie. Erről szól hello részletes [fiókkezelő a Google-dokumentáció Itt](http://developer.android.com/reference/android/accounts/AccountManager.html)

Ezek az engedélyek különösen a következőket is:

```
GET_ACCOUNTS
USE_CREDENTIALS
MANAGE_ACCOUNTS
```

### <a name="youve-configured-sso"></a>Egyszeri bejelentkezés konfigurálása!
Most hello Microsoft Identity SDK automatikusan a hitelesítő adatok megosztása az alkalmazások között és hello broker meghívni, ha az megtalálható az eszközükön.

