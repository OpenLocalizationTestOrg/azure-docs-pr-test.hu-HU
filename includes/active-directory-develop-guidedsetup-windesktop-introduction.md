# <a name="call-the-microsoft-graph-api-from-a-windows-desktop-app"></a>A Microsoft Graph API hívása korlátozása a Windows asztali alkalmazások

Ez az útmutató ismerteti, hogyan a natív Windows asztali .NET (XAML) alkalmazás szereznie egy hozzáférési jogkivonatot és a Microsoft Graph API vagy egyéb Azure Active Directory v2 végpont a hozzáférési jogkivonatok igénylő API-k hívása.

Amikor befejezte az útmutatóban, az alkalmazás tudják a személyes fiókok (például outlook.com, live.com és mások) használó védett API hívása. Az alkalmazás használata munkahelyi és iskolai fiókok bármely vállalat vagy szervezet, amely az Azure Active Directory is.  

> [!NOTE] 
> Az útmutató a Visual Studio 2015 Update 3 vagy a Visual Studio 2017 van szükség.  Ezek egyikét nincs? [A Visual Studio 2017 ingyenesen letölthető](https://www.visualstudio.com/downloads/).

## <a name="how-this-guide-works"></a>Ez az útmutató működése

![Ez az útmutató működése](./media/active-directory-develop-guidedsetup-windesktop-intro/windesktophowitworks.png)

A mintaalkalmazás, amely létrehozta a jelen útmutató lehetővé teszi, hogy a Windows asztali alkalmazás, amely lekérdezi a Microsoft Graph API vagy egy webes API, amely az Azure Active Directory v2 végpont jogkivonatokat fogad el. Ebben a forgatókönyvben ad hozzá egy jogkivonat hitelesítési fejlécéhez via HTTP-kérelmekre. Microsoft hitelesítési könyvtár (MSAL) token beszerzése és -megújítás kezeli.

## <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Webes API-k eléréséhez token beszerzési kezelése védett.

A felhasználó hitelesítése után a mintaalkalmazás kap egy jogkivonatot, amely segítségével a Microsoft Graph API vagy egy webes API-t, amelyet az Azure Active Directory v2 lekérdezése.

Például a Microsoft Graph API-k egy jogkivonatot, hogy egy adott erőforráshoz való hozzáférést igényel. A token például a felhasználói profil olvasása, a felhasználó naptár elérésére, és e-mailek küldése. Az alkalmazás olyan hozzáférési jogkivonatot kérhetnek MSAL API hatókörök megadásával ezek az erőforrások eléréséhez használt. Ez a jogkivonat majd kerül minden hívás ellen védett erőforrás végrehajtott HTTP hitelesítési fejlécéhez. 

MSAL kezeli és a hozzáférési jogkivonatok frissítése, úgy, hogy az alkalmazás nem szükséges.

## <a name="nuget-packages"></a>NuGet-csomagok

Ez az útmutató a következő NuGet-csomagok használja:

|Részletes ismertetés|Leírás|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Microsoft hitelesítési könyvtár (MSAL)|

