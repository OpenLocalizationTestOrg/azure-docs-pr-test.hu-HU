---
title: "aaaHow tooenable adal-t használó IOS alkalmazások közötti SSO |} Microsoft Docs"
description: "Hogyan toouse hello funkcióit hello az alkalmazások között az egyszeri bejelentkezés ADAL SDK tooenable. "
services: active-directory
documentationcenter: 
author: brandwe
manager: mbaldwin
editor: 
ms.assetid: d042d6da-7503-4e20-bb55-06917de01fcd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 04/07/2017
ms.author: brandwe
ms.custom: aaddev
ms.openlocfilehash: b7b4389a8dcd956211ffa1aaa431aaf21ded8961
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooenable-cross-app-sso-on-ios-using-adal"></a>Hogyan tooenable adal-t használó IOS alkalmazások közötti SSO
Egyszeri bejelentkezés (SSO) biztosít, így a felhasználók csak kell tooenter hitelesítő adataikkal egyszer, és ezen hitelesítő adatok automatikusan munkahelyi rendelkezik a különböző alkalmazások most várt ügyfelek. hello nehéz kis képernyőjű, gyakran alkalommal együtt egy további tényezőt (2FA) például telefonhívást vagy egy fogva kódot, a felhasználónév és jelszó megadása eredményezi, ha egy felhasználó gyors kapcsolatos van toodo ennek a termékhez egynél többször.

Ezenkívül ha alkalmaz egy identitás-platform, amely más alkalmazások is használhatnak, például a Microsoft Accounts vagy az Office365 munkahelyi fiókkal, az ügyfelek várt, hogy ezen hitelesítő adatok toobe elérhető toouse az alkalmazások között nem számít, hello szállító.

hello Microsoft Identity platform, a Microsoft Identity SDK-k, valamint elvégzi a rögzített munka, és képes toodelight egyszeri Bejelentkezést, vagy az alkalmazások vagy, mint a saját suite belül az ügyfelek hello biztosítja a broker funkció és a hitelesítő az alkalmazások, az eszköz teljes hello.

Ez a forgatókönyv megtudhatja, hogyan tooconfigure az SDK az alkalmazás tooprovide belül a juttatás tooyour ügyfelek.

Ez a forgatókönyv a következőkre vonatkozik:

* Azure Active Directory
* Azure Active Directory B2C
* Az Azure Active Directory B2B
* Azure Active Directory feltételes hozzáférés

hello előző lépéseknél túl hogyan tudja[alkalmazások hello örökölt portálon az Azure Active Directory kiépítése](active-directory-how-to-integrate.md) és az alkalmazás integrálható hello [Microsoft Identity iOS SDK](https://github.com/AzureAD/azure-activedirectory-library-for-objc).

## <a name="sso-concepts-in-hello-microsoft-identity-platform"></a>A Microsoft Identity Platform hello SSO fogalmak
### <a name="microsoft-identity-brokers"></a>Microsoft – identitáskezelési brókerek
A Microsoft lehetővé teszi a minden mobileszköz platform, amelyek lehetővé teszik a különböző alkalmazások különböző szállítóktól származó hello hídképzést, a hitelesítő adatok, és lehetővé teszi, hogy honnan egy biztonságos helyen igénylő különleges továbbfejlesztett funkciók toovalidate hitelesítő adatokat. Ezek nevezzük **brókerek**. IOS és Android rendszeren a brókerek szolgáltatáson keresztül letölthető alkalmazás, hogy az ügyfelek telepítése függetlenül vagy egy vállalati, akár az összeset hello eszköz az alkalmazott felügyelő a toohello eszköz továbbíthatja. Ezek a brókerek kezelése biztonsági csak az egyes alkalmazások vagy hello: az eszköz teljes alapján a rendszergazdák törlését támogatja. A Windows rendszerben a funkcióit egy toohello operációs rendszerben, technikailag néven hello Webeshitelesítés-szervező beépített fiók kiválasztásakor.

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
| ADAL SDK  |  |  ADAL SDK  |  |  ADAK SDK   |
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
 
 hello kell tooensure hello identitását egy alkalmazás hívás hello broker nyújtunk a támogatott broker bejelentkezési adatok számára elengedhetetlen toohello biztonsági. IOS és Android nem kényszeríti az egyedi azonosítók, amelyek csak egy adott alkalmazáshoz tartozó érvényes, így rosszindulatú alkalmazások előfordulhat, hogy "hamis" a jogos alkalmazásazonosító, és azt jelentette, hogy hello jogos alkalmazás hello jogkivonatokat fogadni. mindig tájékoztatjuk futásidőben hello jobb alkalmazással tooensure, kérjük hello fejlesztői tooprovide egyéni redirectURI Microsoft az alkalmazás regisztrálásakor. **Az alábbiakban tárgyalt hogyan a fejlesztők kézműipari az átirányítási URI-t kell.** Az egyéni redirectURI hello hello alkalmazás alkalmazáscsomag-Azonosítót tartalmaz, és a hello Apple App Store toobe egyedi toohello alkalmazás biztosítja. Amikor egy alkalmazás hívások hello broker hello broker megkérdezi hello iOS operációs rendszer tooprovide azt hello hello broker nevű alkalmazáscsomag-Azonosítót. hello broker hello hívás tooour identitásrendszere ezt az Alkalmazásköteg-Azonosítót tooMicrosoft nyújt. Ha hello hello alkalmazás Csomagazonosítóját nem egyezik meg a regisztráció során a Csomagazonosító toous hello fejlesztő megadott hello, azt megtagadja a hozzáférést toohello jogkivonatok hello erőforrás hello alkalmazás számára. Ez az ellenőrzés biztosítja, hogy csak a regisztrált hello fejlesztő hello alkalmazás-jogkivonatokat kap.

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
| Azure SDK  | | Azure SDK  |   | Azure SDK   |
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
Jelen példában használjuk hello ADAL iOS SDK számára:

* Nem közvetített támogatott egyszeri Bejelentkezést a csomag az alkalmazások bekapcsolása
* Kapcsolja be az SSO broker támogatású támogatása

### <a name="turning-on-sso-for-non-broker-assisted-sso"></a>Nem közvetített egyszeri Bejelentkezést bekapcsolás támogatott az egyszeri bejelentkezés
Nem közvetített támogatott egyszeri bejelentkezéshez alkalmazásra hello Microsoft Identity SDK-k kezelése nagy részét SSO hello összetettsége meg. Ez magában foglalja a hello gyorsítótár hello jobb felhasználói keresése és karbantartása az Ön tooquery bejelentkezett felhasználók listáját.

egyszeri bejelentkezés tooenable alkalmazásra van szüksége a következő toodo hello saját:

1. Győződjön meg arról az összes az alkalmazások felhasználói hello ugyanazon ügyfél-Azonosítót vagy az alkalmazás azonosítóját.
2. Ügyeljen arra, hogy az alkalmazások megosztás hello azonos aláíró tanúsítvány az Apple-től, így keychains megoszthatja a
3. Kérelem hello azonos kulcslánc jogosultság az összes az alkalmazáshoz.
4. Hogy hello Microsoft Identity SDK-k kapcsolatos hello megosztott kulcslánc velünk toouse.

#### <a name="using-hello-same-client-id--application-id-for-all-hello-applications-in-your-suite-of-apps"></a>Ugyanazon ügyfél-azonosító segítségével hello / Application ID, az összes hello az alkalmazások a csomagban található alkalmazások
Hello Microsoft Identity platform tooknow ahhoz, hogy az alkalmazások között tooshare jogkivonatok engedélyezett az alkalmazások egyes kell tooshare hello ugyanazon ügyfél-Azonosítót vagy az alkalmazás azonosítóját. Ez az egyedi azonosító hello tooyou megadott hello portálon az első alkalmazás regisztrálásakor.

Előfordulhat, hogy lehet szeretné megtudni, hogy hogyan határozható meg különböző alkalmazások toohello Ha használja a Microsoft Identity service hello ugyanazon alkalmazás azonosítóját. hello válasz van hello **átirányítási URI-azonosítók**. Minden alkalmazás több átirányítási URI-azonosítók hello bevezetési portálon regisztrált rendelkezhet. A csomagban található összes alkalmazás egy másik átirányítási URI-t fog rendelkezni. Ez megjelenésének például nem éri el:

Az App1 átirányítási URI-ja:`x-msauth-mytestiosapp://com.myapp.mytestapp`

App2 átirányítási URI-ja:`x-msauth-mytestiosapp://com.myapp.mytestapp2`

App3 átirányítási URI-ja:`x-msauth-mytestiosapp://com.myapp.mytestapp3`

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

#### <a name="create-keychain-sharing-between-applications"></a>A kulcslánc megosztása az alkalmazások között létrehozása
A kulcslánc megosztása nem ez a dokumentum hello terjed, és a dokumentum az Apple által szabályozott [Adding Capabilities](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html). A fontos eldöntheti, milyen a kulcslánc toobe nevezik, és adja hozzá ezt a funkciót az alkalmazások között.

Ha készen áll, megfelelő jogosultságok tekintse meg a projektkönyvtárban jogosult a fájl `entitlements.plist` , amely tartalmazza, amelyet a következőhöz hasonló hello:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>keychain-access-groups</key>
    <array>
        <string>$(AppIdentifierPrefix)com.myapp.mytestapp</string>
        <string>$(AppIdentifierPrefix)com.myapp.mycache</string>
    </array>
</dict>
</plist>
```

Miután hello kulcslánc jogosultság minden alkalmazásból engedélyezve van, és készen áll a toouse SSO, adja hello Microsoft Identity SDK a kulcslánc-beállítás a következő hello segítségével a `ADAuthenticationSettings` a beállítás a következő hello:

```
defaultKeychainSharingGroup=@"com.myapp.mycache";
```

> [!WARNING]
> Az alkalmazások között egy kulcslánc megosztásakor bármely alkalmazás törölheti felhasználók vagy rosszabb összes hello jogkivonatot az alkalmazás minden részében. Ez a különösen katasztrofális, ha a hello jogkivonatok toodo háttérműveletek használó alkalmazásokat. A kulcslánc megosztása azt jelenti, hogy a minden nagyon gondosan kell eltávolítási műveletek hello Microsoft Identity SDK-k használatával.
> 
> 

Ennyi az egész! a Microsoft Identity SDK hello most fájlmegosztási hitelesítő adatokat az alkalmazások között. hello Felhasználólista lesznek megosztva alkalmazáspéldányok között.

### <a name="turning-on-sso-for-broker-assisted-sso"></a>Egyszeri bejelentkezés SSO Broker bekapcsolása esetén támogatott.
az egy bármely Broker összetevője hello eszközön telepített alkalmazás toouse képesek hello **alapértelmezés szerint ki van kapcsolva**. A rendezés toouse az alkalmazás a hello broker kell tegye néhány további beállításra, és néhány kódot tooyour alkalmazás hozzáadása.

hello lépéseket toofollow a következők:

1. Az alkalmazás kódjában hívás toohello MS SDK broker mód engedélyezése.
2. Állítson be egy új átirányítási URI-t, és tooboth hello alkalmazást és az alkalmazás regisztrációját.
3. Regisztrálás egy URL-sémát.
4. iOS9 támogatási: Adjon hozzá egy engedély tooyour info.plist fájlt.

#### <a name="step-1-enable-broker-mode-in-your-application"></a>1. lépés: Az alkalmazás broker üzemmód engedélyezése
az alkalmazás toouse hello Broker hello lehetősége van kapcsolva, hello "context" vagy a kezdeti beállítás és a hitelesítési objektum létrehozásakor. Ehhez úgy, hogy a hitelesítő adatok típusa a kódban:

```
/*! See hello ADCredentialsType enumeration definition for details */
@propertyADCredentialsType credentialsType;
```
Hello `AD_CREDENTIALS_AUTO` beállítás lehetővé teszi a hello Microsoft Identity SDK tootry toocall toohello broker kimenő `AD_CREDENTIALS_EMBEDDED` megakadályozza, hogy a Microsoft Identity SDK hello toohello broker hívja.

#### <a name="step-2-registering-a-url-scheme"></a>2. lépés: Egy URL-séma regisztrálása
hello Microsoft Identity platform URL-címek tooinvoke hello és a vezérlő majd térjen vissza tooyour alkalmazás használja. toofinish, hogy egy URL-sémát kell oda-vissza regisztrálva az alkalmazáshoz, hogy a Microsoft Identity platform fog tudni hello. Ez lehet továbbá tooany más alkalmazás rendszerek, előfordulhat, hogy már korábban regisztrált az alkalmazással.

> [!WARNING]
> Azt javasoljuk, hogy hello URL-cím sémája viszonylag egyedi toominimize hello esélyét hello használata egy másik alkalmazás azonos URL-sémát. Apple nem kényszeríti ki az URL-sémákat hello app Store-ból regisztrált hello egyediségét.
> 
> 

Alább példája hogyan Ez jelenik meg a projekt konfigurációjában. Előfordulhat, hogy is ehhez az xcode-ban, valamint:

```
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>com.myapp.mytestapp</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>x-msauth-mytestiosapp</string>
        </array>
    </dict>
</array>
```

#### <a name="step-3-establish-a-new-redirect-uri-with-your-url-scheme"></a>3. lépés: Egy új átirányítási URI-t és az URL-séma létrehozása
A sorrend tooensure, hogy a rendszer mindig visszaadja a hello credential jogkivonatok toohello megfelelő alkalmazás igazolnia kell a toomake meg arról, hogy a telepítésnek vissza oly módon, hogy az iOS operációs rendszer hello tooyour alkalmazás ellenőrizheti. hello iOS operációs rendszer jelentések toohello Microsoft broker alkalmazások hello hívja az hello alkalmazás Csomagazonosítóját. Ez a nem egy engedélyezetlen alkalmazás megtévesztésre. Ezért azt használja ki a hello az, hogy a hello jogkivonatok kerüljenek-e vissza a megfelelő alkalmazás toohello broker alkalmazás tooensure URI-val együtt. Kérjük, tooestablish ez egyedi átirányítási URI-t, mind az alkalmazás és egy átirányítási URI-t, a developer portálon.

Az átirányítási URI-t a hello megfelelő formátumban kell lennie:

`<app-scheme>://<your.bundle.id>`

például: *x-msauth-mytestiosapp://com.myapp.mytestapp*

Az átirányítási URI-t kell az alkalmazás-regisztrációk hello megadott toobe [Azure-portálon](https://portal.azure.com/). További információ az Azure AD alkalmazás-regisztrációval, lásd: [integrálása az Azure Active Directoryval](active-directory-how-to-integrate.md).

##### <a name="step-3a-add-a-redirect-uri-in-your-app-and-dev-portal-toosupport-certificate-based-authentication"></a>3a. lépés: az alkalmazás és a fejlesztői portál toosupport tanúsítvány alapú hitelesítést egy átirányítási URI hozzáadása
egy második "msauth" toosupport-Tanúsítványalapú hitelesítés kell regisztrálni az alkalmazás- és hello toobe [Azure-portálon](https://portal.azure.com/) toohandle tanúsítvány alapú hitelesítést, ha tooadd, amelyek támogatják az alkalmazásban.

`msauth://code/<broker-redirect-uri-in-url-encoded-form>`

például: *msauth://code/x-msauth-mytestiosapp%3A%2F%2Fcom.myapp.mytestapp*

#### <a name="step-4-ios9-add-a-configuration-parameter-tooyour-app"></a>4. lépés: iOS9: egy konfigurációs paraméter tooyour alkalmazás hozzáadása
Adal-t használ – canOpenURL: toocheck Ha hello broker hello eszközön van telepítve. Az iOS 9-es Apple zárolva mi is kereshet séma egy alkalmazáskészletet. Szüksége lesz a tooadd "msauth" toohello info.plist szakasza a `info.plist file`.

<key>Info.plist</key>

<array><string>msauth</string>
</array>

### <a name="youve-configured-sso"></a>Egyszeri bejelentkezés konfigurálása!
Most hello Microsoft Identity SDK automatikusan a hitelesítő adatok megosztása az alkalmazások között és hello broker meghívni, ha az megtalálható az eszközükön.

