---
title: "Első lépések AD Android aaaAzure |} Microsoft Docs"
description: "Hogyan toobuild egy Android-alkalmazás, amely az Azure AD bejelentkezési és a hívások Azure AD számára az API-k OAuth használatával védett."
services: active-directory
documentationcenter: android
author: danieldobalian
manager: mbaldwin
editor: 
ms.assetid: da1ee39f-89d3-4d36-96f1-4eabbc662343
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 01/07/2017
ms.author: dadobali
ms.custom: aaddev
ms.openlocfilehash: 1aedc8ff60874b405a182a4ccbfb2c8b4d9d3704
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-into-an-android-app"></a>Az Azure AD integrálása Android-alkalmazás
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

> [!TIP]
> Próbálja meg az új hello előnézete [fejlesztői portálján](https://identity.microsoft.com/Docs/Android), amely segít, amelyekből megismerheti az Azure AD csak néhány perc múlva. hello fejlesztői portálján végigvezeti hello folyamat regisztrálja az alkalmazást, és az Azure AD integrálása a kódot. Amikor elkészült, akkor kell egy egyszerű alkalmazást, amely képes hitelesíteni a felhasználók számára a bérlő és a háttérből fogadni és-ellenőrzéshez.
>
>

Ha az asztali alkalmazások, Azure Active Directory (Azure AD) teszi egyszerű és magától értetődő, tooauthenticate a a felhasználók a helyszíni Active Directory-fiókok használatával. Emellett lehetővé teszi az alkalmazás toosecurely használatához minden webes API-t az Azure AD által védett, például Office 365 API-k hello vagy hello Azure API.

Android-ügyfelek, amelyeket tooaccess védett erőforrások az Azure AD hello Active Directory Authentication Library (ADAL) biztosít. hello kizárólagos ADAL célja toomake megkönnyítik az alkalmazás tooget hozzáférési jogkivonatok. milyen egyszerűen, azt fogja Android feladatlista alkalmazás létrehozásához, amely toodemonstrate:

* Lekérdezi hozzáférési jogkivonatainak egy tennivalók listája API felület meghívásakor hello segítségével [OAuth 2.0 hitelesítési protokoll](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Lekérdezi a felhasználó tennivalók listájára.
* Felhasználók jeleket.

tooget elindult, amelyben felhasználók létrehozása és egy alkalmazás regisztrálása az Azure AD-bérlő kell. Ha még nem rendelkezik a bérlő [megtudhatja, hogyan egy tooget](active-directory-howto-tenant.md).

## <a name="step-1-download-and-run-hello-nodejs-rest-api-todo-sample-server"></a>1. lépés: Töltse le és futtassa a hello Node.js REST API TODO minta kiszolgáló
hello Node.js REST API TODO minta kifejezetten a meglévő mintát eredményez, amely a single-bérlő tennivaló REST API létrehozása az Azure AD elleni toowork írása. Ez a gyors üzembe helyezés hello előfeltétele.

Hogyan tooset ez, tekintse meg a meglévő minták kapcsolatos [Microsoft Azure Active Directory minta REST API szolgáltatás a Node.js](active-directory-devquickstarts-webapi-nodejs.md).


## <a name="step-2-register-your-web-api-with-your-azure-ad-tenant"></a>2. lépés: A webes API regisztrálása az Azure AD-bérlő
Az Active Directory támogatja a két típusú alkalmazások hozzáadása:

- Webes API-k által biztosított szolgáltatások toousers
- (Hello Web vagy az eszközön futó) alkalmazások, azokat elérő webes API-khoz

Ebben a lépésben regisztrálása most az hello webes API-t, hogy ez a minta tesztelési helyileg futtatja. A webes API-k általában az, hogy szeretné-e egy alkalmazás tooaccess ajánlat funkciók REST-szolgáltatást. Az Azure AD segítségével biztosíthatja a tetszőleges végpontot.

Jelenleg folyamatban feltételezve, hogy van-e regisztrálása hello TODO REST API-t korábban hivatkozott. Azonban ez a webes API-t, Azure Active Directory toohelp védeni kívánt működik.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello felső sávon kattintson a fiókját. A hello **Directory** menüben válassza ki a kívánt tooregister hello Azure AD-bérlő az alkalmazást.
3. Kattintson a **több szolgáltatások** hello bal oldali ablaktáblán, és válassza ki **Azure Active Directory**.
4. Kattintson a **App regisztrációk**, majd válassza ki **Hozzáadás**.
5. Adjon meg egy rövid nevet hello alkalmazás (például **TodoListService**) elemre, jelölje be **webalkalmazás és/vagy webes API**, és kattintson a **következő**.
6. Bejelentkezési URL-címhez hello hello minta hello alap URL-cím megadása. Alapértelmezés szerint ez a `https://localhost:8080`.
7. Kattintson a **OK** toocomplete hello regisztrációs.
8. Hello Azure-portálon, a továbbra is tooyour alkalmazáslap lépjen, hello alkalmazás azonosítóérték található, és másolja azt. Ezt később szüksége az alkalmazás konfigurálásakor.
9. A hello **beállítások** -> **tulajdonságok** lapon, hello app ID URI frissítése – adja meg `https://<your_tenant_name>/TodoListService`. Cserélje le `<your_tenant_name>` hello nevet, az Azure AD-bérlő.

## <a name="step-3-register-hello-sample-android-native-client-application"></a>3. lépés: Hello minta Android Native Client alkalmazás regisztrálása
Ez a példa regisztrálnia kell a webalkalmazást. Ez lehetővé teszi az alkalmazás toocommunicate a hello csak regisztrált web API. Az Azure AD fog megtagadják tooeven lehetővé teszi az alkalmazás tooask a bejelentkezéshez, kivéve regisztrálva van. Az hello biztonsági hello modell, amely része.

Azt még feltéve, hogy van-e regisztrálása korábban hivatkozott hello mintaalkalmazást. De bármely alkalmazás, amely kidolgozása Ez az eljárás használható.

> [!NOTE]
> Először talán miért kívánja menteni egy alkalmazás és a webes API-k egy bérlő. Mivel előfordulhat, hogy Ön rendelkezik kitalál, egy alkalmazás olyan külső API-bérlőhöz egy másik Azure AD-ben regisztrált hozzáférő hozhat létre. Ha így tesz, az ügyfelek is hello API hello alkalmazásban tooconsent toohello használatát kéri. Az IOS rendszerhez készült Active Directory Authentication Library gondoskodik a hozzájárulásukat adják meg. Összetettebb funkciók megismeréséhez azt láthatja, hogy ez az hello munkahelyi szükséges tooaccess hello tartalmazó csomag, az Azure és az Office, valamint az egyéb szolgáltató Microsoft APIs fontos része. Most, mert a webes API-t és a hello alatt az alkalmazás regisztrálva azonos bérlői, bármely beleegyezést kér fogja látni. Ez helyzet általában hello Ha az alkalmazás csak a saját vállalati toouse.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello felső sávon kattintson a fiókját. A hello **Directory** menüben válassza ki a kívánt tooregister hello Azure AD-bérlő az alkalmazást.
3. Kattintson a **több szolgáltatások** hello bal oldali ablaktáblán, és válassza ki **Azure Active Directory**.
4. Kattintson a **App regisztrációk**, majd válassza ki **Hozzáadás**.
5. Adjon meg egy rövid nevet hello alkalmazás (például **TodoListClient-Android**) elemre, jelölje be **natív ügyfélalkalmazás**, és kattintson a **következő**.
6. Hello az átirányítási URI-címe, adja meg `http://TodoListClient`. Kattintson a **Befejezés** gombra.
7. Hello alkalmazás oldalról hello alkalmazás azonosítóérték található, és másolja azt. Ezt később szüksége az alkalmazás konfigurálásakor.
8. A hello **beállítások** lapon jelölje be **szükséges engedélyek** válassza **hozzáadása**.  Keresse meg és válassza ki a TodoListService, adja hozzá a hello **hozzáférés TodoListService** engedélyt a **delegált engedélyek**, és kattintson a **végzett**.

a Maven toobuild, pom.xml használhatja hello felső szinten:

1. A tárház klónozása egy olyan könyvtárba, az Ön által választott:

  `$ git clone git@github.com:AzureADSamples/NativeClient-Android.git`  
2. Hello kövesse hello [Előfeltételek tooset Android Maven környezet](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android).
3. Az SDK 19 hello emulátor beállítása.
4. Nyissa meg ahol klónozott tárház hello toohello gyökérmappájába.
5. Futtassa ezt a parancsot:`mvn clean install`
6. Hello directory toohello gyors üzembe helyezési minta módosítása:`cd samples\hello`
7. Futtassa ezt a parancsot:`mvn android:deploy android:run`

   Meg kell jelennie a hello alkalmazás indítása.
8. Adja meg a teszt felhasználói hitelesítő adatok tootry.

JAR csomagok nyújtanak hello AAR csomag mellett.

## <a name="step-4-download-hello-android-adal-and-add-it-tooyour-eclipse-workspace"></a>4. lépés: Töltse le az Android ADAL hello, és adja hozzá tooyour Eclipse munkaterület
Hajtottunk azt Ön toohave egyszerűen több beállítások toouse adal-t a Androidos projekt:

* Használhat hello forrás kód tooimport tárra eclipse-ben és a hivatkozás tooyour alkalmazásba.
* Android Studio használata, hello AAR csomag formázása és a hivatkozás hello bináris fájljait is használhatja.

### <a name="option-1-source-zip"></a>1. lehetőség: Forrás Zip
hello forráskódját, másolatának toodownload kattintson **töltse le a ZIP-** hello jobb oldalán található hello. Illetve [töltse le a Githubról](https://github.com/AzureAD/azure-activedirectory-library-for-android/archive/v1.0.9.tar.gz).

### <a name="option-2-source-via-git"></a>2. lehetőség: Forrás Git keresztül
hello tooget hello forráskódját SDK keresztül Git, írja be:

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

### <a name="option-3-binaries-via-gradle"></a>3. lehetőség: Bináris Gradle keresztül
Hello bináris fájljai az hello Maven központi tárházban kérheti le. az alábbiak szerint hello AAR csomagot is tartalmazza a projekt az Android Studio:

```gradle
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
}
```

### <a name="option-4-aar-via-maven"></a>4. lehetőség: AAR Maven keresztül
Hello M2Eclipse beépülő modul használata, hello függőségi adhat meg a pom.xml fájlt:

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>1.1.1</version>
    <type>aar</type>
</dependency>
```


### <a name="option-5-jar-package-inside-hello-libs-folder"></a>5. lehetőség: JAR csomag hello függvénytárak mappába
Hello JAR-fájlra beszerezni hello Maven-tárházban, és dobja el, a hello **függvénytárak** a projekt mappájára. Toocopy hello szükséges erőforrások tooyour projekt, valamint kell hello JAR csomagok nem tartalmazza azokat.

## <a name="step-5-add-references-tooandroid-adal-tooyour-project"></a>5. lépés: Hivatkozás tooAndroid ADAL tooyour projekt hozzáadása
1. Egy hivatkozási tooyour projekt hozzáadása, és adja meg azt az Android tárként. Ha nem Ön hogyan toodo, hello további tájékoztatást kaphat [Android Studio hely](http://developer.android.com/tools/projects/projects-eclipse.html).
2. Adja hozzá azokat a projektbeállításokat hibakeresési hello projektfüggőségek.
3. A projekt AndroidManifest.xml fájl tooinclude frissítése:

        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <application
            android:allowBackup="true"
            android:debuggable="true"
            android:icon="@drawable/ic_launcher"
            android:label="@string/app_name"
            android:theme="@style/AppTheme" >

            <activity
                android:name="com.microsoft.aad.adal.AuthenticationActivity"
                android:label="@string/title_login_hello_app" >
            </activity>
            ....
        <application/>

4. A fő tevékenységnél AuthenticationContext példányának létrehozása. a hívás hello részleteit Ez a témakör hello terjed, de remek kezdőpont kaphat hello megnézi [Android Native Client minta](https://github.com/AzureADSamples/NativeClient-Android). A következő példa hello, SharedPreferences hello alapértelmezett gyorsítótár, továbbá hatóság hello formájában `https://login.microsoftonline.com/yourtenant.onmicrosoft.com`:

    `mContext = new AuthenticationContext(MainActivity.this, authority, true); // mContext is a field in your activity`

5. Másolja a kód blokk toohandle hello vége AuthenticationActivity hello felhasználó megadja hitelesítő adatait, és megkapja az engedélyezési kód után:

        @Override
         protected void onActivityResult(int requestCode, int resultCode, Intent data) {
             super.onActivityResult(requestCode, resultCode, data);
             if (mContext != null) {
                mContext.onActivityResult(requestCode, resultCode, data);
             }
         }

6. a jogkivonat tooask egy visszahívási határozhat meg:

        private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {

            @Override
            public void onError(Exception exc) {
                if (exc instanceof AuthenticationException) {
                    textViewStatus.setText("Cancelled");
                    Log.d(TAG, "Cancelled");
                } else {
                    textViewStatus.setText("Authentication error:" + exc.getMessage());
                    Log.d(TAG, "Authentication error:" + exc.getMessage());
                }
            }

            @Override
            public void onSuccess(AuthenticationResult result) {
                mResult = result;

                if (result == null || result.getAccessToken() == null
                        || result.getAccessToken().isEmpty()) {
                    textViewStatus.setText("Token is empty");
                    Log.d(TAG, "Token is empty");
                } else {
                    // request is successful
                    Log.d(TAG, "Status:" + result.getStatus() + " Expired:"
                            + result.getExpiresOn().toString());
                    textViewStatus.setText(PASSED);
                }
            }
        };

7. Végül kérje meg a jogkivonat, hogy a visszahívás használatával:

    `mContext.acquireToken(MainActivity.this, resource, clientId, redirect, user_loginhint, PromptBehavior.Auto, "",
                   callback);`

Hello paraméterek leírását itt található:

* *erőforrás* szükséges, azonban tooaccess próbált hello erőforrás.
* *ClientID* szükség, és az Azure AD származik.
* *RedirectUri* nincs szükség toobe hello acquireToken hívás előírt. Állíthat be, mint a csomag neve.
* *PromptBehavior* tooask a hitelesítő adatok tooskip hello gyorsítótár és a cookie-k segítségével.
* *a visszahívási* után hello engedélyezési kód cseréje a jogkivonat neve. AuthenticationResult, amelynek hozzáférési jogkivonat objektum rendelkezik, lejárt, és a lexikális elem adatainak azonosító.
* *acquireTokenSilent* nem kötelező megadni. Hívása akkor toohandle gyorsítótárazás és a frissítési token. Hello Sync szolgáltatás verzióját is tartalmazza. Elfogadja a *userId* paraméterként.

        mContext.acquireTokenSilent(resource, clientid, userId, callback );

Ez a forgatókönyv segítségével kell milyen kell toosuccessfully integrálása az Azure Active Directoryban. További példák a, a Microsoft hello AzureADSamples / GitHub tárházából.

## <a name="important-information"></a>Fontos információk
### <a name="customization"></a>Testreszabás
Az alkalmazás-erőforrásokat felülírhatnak-e projekt könyvtárerőforrásokat. Ez akkor fordul elő, amikor az alkalmazás éppen készül. Emiatt testre szabhatja a hitelesítési tevékenységet elrendezés hello igényeinek megfelelően. Hello szabályozza, hogy tookeep hello Azonosítóját kell, hogy az adal-t használ a (webes Nézet).

### <a name="broker"></a>Broker
hello Microsoft Intune vállalati portál alkalmazás hello broker összetevő biztosít. hello fiók AccountManager jön létre. hello fiók típus: "com.microsoft.workaccount." AccountManager lehetővé teszi, hogy csak egyetlen egyszeri bejelentkezési fiók. Az egyszeri bejelentkezési cookie hello felhasználó hello eszköz kihívás az hello alkalmazások befejezése után hoz létre.

ADAL hello broker fiókot használja, ha egy felhasználói fiók a hitelesítő jön létre, és nem tooskip azt. Kihagyhatja hello broker felhasználót:

   `AuthenticationSettings.Instance.setSkipBroker(true);`

Egy különös RedirectUri tooregister broker használati van szükség. Hello formátumban van RedirectUri `msauth://packagename/Base64UrlencodedSignature`. A RedirectUri hello parancsfájl brokerRedirectPrint.ps1 vagy hello API-hívás mContext.getBrokerRedirectUri használatával kaphat az alkalmazást. hello aláírás aláíró tanúsítványok kapcsolódó tooyour.

hello aktuális broker modell csak egy felhasználóhoz. AuthenticationContext hello API metódus tooget hello broker felhasználói biztosít.

   `String brokerAccount =  mContext.getBrokerUser(); //Broker user is returned if account is valid.`

Az alkalmazás jegyzékének rendelkeznie kell a következő engedélyek toouse AccountManager fiókok hello. További információkért lásd: hello [hello Android webhelyéről AccountManager információk](http://developer.android.com/reference/android/accounts/AccountManager.html).

* GET_ACCOUNTS
* USE_CREDENTIALS
* MANAGE_ACCOUNTS

### <a name="authority-url-and-ad-fs"></a>Szolgáltató URL-címe és az AD FS
Active Directory összevonási szolgáltatások (AD FS) értéke nem értelmezhető éles STS, így példány felderítés tooturn kell, és hamis át, hello AuthenticationContext konstruktor.

hello szolgáltató URL-címe van szüksége az STS-példány és egy [bérlő neve](https://login.microsoftonline.com/yourtenant.onmicrosoft.com).

### <a name="querying-cache-items"></a>Gyorsítótár elemek lekérdezése
Adal-t tartalmaz néhány egyszerű gyorsítótár alapértelmezett gyorsítótár SharedPreferences a lekérdezési funkciók. Hello aktuális gyorsítótár letölthető AuthenticationContext használatával:

    ITokenCacheStore cache = mContext.getCache();

Is megadhatja a gyorsítótár-megvalósítással, ha azt szeretné, hogy toocustomize azt.

    mContext = new AuthenticationContext(MainActivity.this, authority, true, yourCache);

### <a name="prompt-behavior"></a>Parancssor viselkedése
Adal-t biztosít egy beállítás toospecify parancssor viselkedése. PromptBehavior.Auto hello felhasználói felületén jelennek meg, ha hello frissítési jogkivonat érvénytelen, és felhasználói hitelesítő adatok szükségesek. PromptBehavior.Always fog hello gyorsítótár-használati kihagyhatja, és mindig jelenjen meg hello UI.

### <a name="silent-token-request-from-cache-and-refresh"></a>A gyorsítótár és a frissítési csendes jogkivonatkérelem
Csendes kérés hello előugró felhasználói felület nem használ, és nem szükséges egy tevékenység. A jogkivonat hello gyorsítótárból, ha elérhető adja vissza. Ha hello-token érvényessége lejárt, ez a módszer megpróbál toorefresh azt. Ha hello frissítési jogkivonat lejárt vagy nem sikerült, AuthenticationException adja vissza.

    Future<AuthenticationResult> result = mContext.acquireTokenSilent(resource, clientid, userId, callback );

Végezhet szinkronizálást is ez a módszer használatával. Null toocallback beállíthatja vagy acquireTokenSilentSync használja.

### <a name="diagnostics"></a>Diagnosztika
Hello elsődleges információforrások a problémák diagnosztizálása az alábbiak:

* Kivételek
* Logs
* Hálózati nyomkövetés

Ne feledje, hogy korrelációs azonosító központi toohello diagnosztika hello könyvtárban. Beállíthatja a korrelációs azonosítók kérelem alapú, ha azt szeretné, hogy az adal-t a kódban az egyéb műveletek kérelem toocorrelate. Ha nem állít egy korrelációs azonosító, a ADAL véletlenszerű egy hoz létre. Az összes üzenetek naplózása és hálózati hívások majd lesz megjelölve hello korrelációs azonosítót. hello a saját azonosító módosításokat minden kérelemnél meg.

#### <a name="exceptions"></a>Kivételek
Kivételek először diagnosztikai hello vannak. A Microsoft próbálja tooprovide hasznos hibaüzenetek. Ha talál, amely nem lehet hasznos, adjon fájlt az kapcsolatos problémát, és ossza meg velünk. Eszköz információkat, például a modell és SDK számát tartalmazza.

#### <a name="logs"></a>Logs
Hello könyvtár toogenerate is beállíthat, amelyeket felhasználhat toohelp naplóüzenetek eseményadatokat. Naplózási azáltal, hogy hello következő tooconfigure, hogy adal-t használja ki a naplóüzenetekben toohand hozza létre a visszahívás hívása.

    Logger.getInstance().setExternalLogger(new ILogger() {
        @Override
        public void Log(String tag, String message, String additionalMessage, LogLevel level, ADALError errorCode) {
        ...
        // You can write this toolog file depending on level or error code.
        writeToLogFile(getApplicationContext(), tag +":" + message + "-" + additionalMessage);
        }
    }

Üzenetek csak írható tooa egyéni naplófájl, ahogy az a következő kód hello. Sajnos nincs nem szabványos vonható naplók az eszközről. Egyes szolgáltatások, amelyek segítségével a van. A saját, például küldő hello tooa fájlkiszolgáló találjon is ki.

    private syncronized void writeToLogFile(Context ctx, String msg) {
       File directory = ctx.getDir(ctx.getPackageName(), Context.MODE_PRIVATE);
       File logFile = new File(directory, "logfile");
       FileOutputStream outputStream = new FileOutputStream(logFile, true);
       OutputStreamWriter osw = new OutputStreamWriter(outputStream);
       osw.write(msg);
       osw.flush();
       osw.close();
    }

Hello naplózási szintek a következők:
* Hiba (kivételek)
* Figyelmeztetés (figyelmeztetés)
* Info (tájékoztatási céllal)
* Részletes (További részletekért)

Hello naplózási szint ilyen állíthatja be:

    Logger.getInstance().setLogLevel(Logger.LogLevel.Verbose);

 Az összes napló küldés toologcat, továbbá tooany egyéni napló visszahívások.
Letölthető egy naplófájl tooa logcat az alábbiak szerint:

    adb logcat > "C:\logmsg\logfile.txt"

 Adb parancsokkal kapcsolatos részletekért lásd: hello [hello Android webhelyéről logcat információk](https://developer.android.com/tools/debugging/debugging-log.html#startingLogcat).

#### <a name="network-traces"></a>Hálózati nyomkövetés
Használhatja a különböző eszközök toocapture hello HTTP-forgalom, amely az adal-t állít elő.  Ez akkor hasznos, ha jártas a hello OAuth protokollt, vagy ha tooprovide diagnosztikai adatokat tooMicrosoft vagy egyéb támogatási csatornáit van szüksége.

Fiddler hello legegyszerűbb HTTP eszköz. Használjon hello következő hivatkozásait tooset toocorrectly rekord ADAL hálózati forgalom fel azt. Olyan eszköz például a Fiddler vagy Charles toobe hasznos konfigurálnia kell az SSL-titkosítás nélkül toorecord forgalom.  

> [!NOTE]
> Nyomok jön létre, így például a hozzáférési jogkivonatok, felhasználónevek és jelszavak magas szintű jogosultsággal rendelkező adatokat tartalmazhatnak. Éles fiókok használata, ne ossza meg a nyomkövetések harmadik felek számára. Ha toosupply egy nyomkövetési toosomeone rendelés tooget támogatására van szüksége, reprodukálja hello hibát egy ideiglenes fiókot, hogy nincs ellenére megosztása felhasználónevek és jelszavak használatával.

* Hello Telerik webhelyéről: [beállítás mentése Fiddler az Android](http://docs.telerik.com/fiddler/configure-fiddler/tasks/ConfigureForAndroid)
* A Githubból: [ADAL Fiddler szabályainak konfigurálása](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/How-to-listen-to-httpUrlConnection-in-Android-app-from-Fiddler)

### <a name="dialog-mode"></a>Párbeszédpanelen mód
hello acquireToken metódus tevékenység nélkül támogatja a párbeszédpanel megjelenítése.

### <a name="encryption"></a>Titkosítás
ADAL hello jogkivonatokat és SharedPreferences tárban alapértelmezés szerint titkosítja. Hello StorageHelper toosee hello részleteit is megtekinthetik. Android 4.3 (API 18) biztonságos tárolására titkos kulcsok Android Keystore bevezetni. Adal-t használ, amely az API 18 és az annál magasabb. Ha toouse ADAL SDK alacsonyabb verziójához, a titkos kulcs, AuthenticationSettings.INSTANCE.setSecretKey tooprovide kell.

### <a name="oauth2-bearer-challenge"></a>Az OAuth2 tulajdonosi kérdés
hello AuthenticationParameters osztály funkció tooget authorization_uri az OAuth2 tulajdonosi challenge hello biztosít.

### <a name="session-cookies-in-webview"></a>Webes nézet munkamenet cookie-k
Android webes nézet nem törli a munkamenetek cookie-jait hello alkalmazás bezárása után. A mintakód használatával, amely kezelheti:

    CookieSyncManager.createInstance(getApplicationContext());
    CookieManager cookieManager = CookieManager.getInstance();
    cookieManager.removeSessionCookie();
    CookieSyncManager.getInstance().sync();

A cookie-k, lásd: hello [hello Android webhelyéről CookieSyncManager információk](http://developer.android.com/reference/android/webkit/CookieSyncManager.html).

### <a name="resource-overrides"></a>Erőforrás-felülbírálások
hello ADAL-könyvtár ProgressDialog üzenetek angol karakterláncot tartalmaz. Az alkalmazás mindent felülír Ha azt szeretné, hogy a honosított karakterláncok.

     <string name="app_loading">Loading...</string>
     <string name="broker_processing">Broker is processing</string>
     <string name="http_auth_dialog_username">Username</string>
     <string name="http_auth_dialog_password">Password</string>
     <string name="http_auth_dialog_title">Sign In</string>
     <string name="http_auth_dialog_login">Login</string>
     <string name="http_auth_dialog_cancel">Cancel</string>

### <a name="ntlm-dialog-box"></a>NTLM párbeszédpanel
1.1.0-ás ADAL-verziót támogatja az NTLM párbeszédpanel, amely WebViewClient hello onReceivedHttpAuthRequest esemény feldolgozása. Testre szabhatja hello elrendezés és karakterláncok hello párbeszédpanel.

### <a name="cross-app-sso"></a>Alkalmazások közötti SSO
Ismerje meg, [hogyan tooenable az ADAL használatával Android alkalmazások közötti SSO](active-directory-sso-android.md).  

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
