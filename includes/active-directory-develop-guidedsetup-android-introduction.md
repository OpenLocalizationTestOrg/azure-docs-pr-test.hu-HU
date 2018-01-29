
# <a name="call-the-microsoft-graph-api-from-an-android-app"></a>A Microsoft Graph API hívása az Android-alkalmazás

Ez az útmutató ismerteti, hogyan natív Android-alkalmazás szereznie egy hozzáférési jogkivonatot és a Microsoft Graph API vagy egyéb szükséges hozzáférési jogkivonatok az Azure Active Directory v2 végpont az API-k hívása.

Amikor befejezte az útmutatóban, az alkalmazás képes fogadni a személyes fiókok (például outlook.com, live.com, és egyéb) és a munkahelyi és iskolai fiókok bármely vállalat vagy szervezet, amely az Azure Active Directory bejelentkezések lesz. Az alkalmazás ezután hívja az API-k, az Azure Active Directory v2 végpont által védett.  

## <a name="how-this-sample-works"></a>Ez a minta működése
![Ez a minta működése](media/active-directory-develop-guidedsetup-android-intro/android-intro.png)

A mintaalkalmazás, amely létrehozta a jelen útmutató egy olyan forgatókönyvet, ahol egy Android-alkalmazás, amely az Azure Active Directory v2 végpont (Microsoft Graph API, ebben az esetben) származó jogkivonatokat fogad el egy webes API lekérdezésére szolgál alapul. Ebben a forgatókönyvben az alkalmazás kiegészíti a megszerzett jogkivonattal hitelesítési fejlécéhez via HTTP-kérelmekre. A Microsoft hitelesítési könyvtár (MSAL) kezeli a token beszerzése és -megújítás meg.

## <a name="prerequisites"></a>Előfeltételek
* Az interaktív telepítés Android Studio összpontosít, de bármely olyan Android-alkalmazás fejlesztői környezetben is elfogadható. 
* Android SDK 21 vagy újabb rendszer szükséges (SDK 25 ajánlott).
* Google Chrome vagy egy olyan webböngésző, használja az egyéni lapok ezen kiadásával a MSAL Android szükség.

> [!NOTE]
> Google Chrome nincs megadva, a Visual Studio emulátorral Android rendszerhez. Azt javasoljuk, hogy tesztelje az emulátor API 25 vagy lemezkép API 21 vagy újabb, amely rendelkezik a telepített Google Chrome ezt a kódot.

## <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Webes API-k eléréséhez token beszerzési kezelése védett.

A felhasználó hitelesítése után a mintaalkalmazás kap egy hozzáférési jogkivonatot, amely segítségével a Microsoft Graph API vagy egy webes API-t, amelyet az Azure Active Directory v2 lekérdezése.

Például a Microsoft Graph API-k olyan hozzáférési jogkivonatot, hogy egy adott erőforráshoz való hozzáférést igényel. Például egy jogkivonatot a felhasználói profil olvasása, a felhasználó naptár elérésére, és e-mailek küldése. Az alkalmazás olyan hozzáférési jogkivonatot kérhetnek MSAL API hatókörök megadásával ezek az erőforrások eléréséhez használt. Ez a jogkivonat majd kerül minden hívás ellen védett erőforrás végrehajtott HTTP hitelesítési fejlécéhez. 

MSAL kezeli és a hozzáférési jogkivonatok frissítése, úgy, hogy az alkalmazás nem szükséges.

## <a name="libraries"></a>Szalagtárak

Ez az útmutató a következő tárakat használ:

|Részletes ismertetés|Leírás|
|---|---|
|[com.microsoft.identity.client](http://javadoc.io/doc/com.microsoft.identity.client/msal)|Microsoft hitelesítési könyvtár (MSAL)|
