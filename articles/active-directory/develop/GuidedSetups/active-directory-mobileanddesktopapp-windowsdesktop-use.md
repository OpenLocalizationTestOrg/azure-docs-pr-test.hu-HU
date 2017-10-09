---
title: "aaaAzure AD v2 Windows asztali első lépések – használata |} Microsoft Docs"
description: "Hogyan Windows asztali .NET (XAML) alkalmazások Azure Active Directory-v2 végpontja hozzáférési jogkivonatok igénylő API meghívása"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: bb258fe5f523ec727ca02716fd823d853d3349b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="use-hello-microsoft-authentication-library-msal-tooget-a-token-for-hello-microsoft-graph-api"></a>Hello Microsoft hitelesítési könyvtár (MSAL) tooget jogkivonat használata hello Microsoft Graph API-val

Ez a szakasz bemutatja, hogyan toouse MSAL tooget jogkivonat hello Microsoft Graph API-val.

1.  A `MainWindow.xaml.cs`, MSAL könyvtár toohello osztály hello hivatkozás hozzáadása:

```csharp
using Microsoft.Identity.Client;
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
Cserélje le <code>MainWindow</code> osztály kódot:
</li>
</ol>

```csharp
public partial class MainWindow : Window
{
    //Set hello API Endpoint tooGraph 'me' endpoint
    string _graphAPIEndpoint = "https://graph.microsoft.com/v1.0/me";

    //Set hello scope for API call toouser.read
    string[] _scopes = new string[] { "user.read" };


    public MainWindow()
    {
        InitializeComponent();
    }

    /// <summary>
    /// Call AcquireTokenAsync - tooacquire a token requiring user toosign-in
    /// </summary>
    private async void CallGraphButton_Click(object sender, RoutedEventArgs e)
    {
        AuthenticationResult authResult = null;

        try
        {
            authResult = await App.PublicClientApp.AcquireTokenSilentAsync(_scopes, App.PublicClientApp.Users.FirstOrDefault());
        }
        catch (MsalUiRequiredException ex)
        {
            // A MsalUiRequiredException happened on AcquireTokenSilentAsync. This indicates you need toocall AcquireTokenAsync tooacquire a token
            System.Diagnostics.Debug.WriteLine($"MsalUiRequiredException: {ex.Message}");

            try
            {
                authResult = await App.PublicClientApp.AcquireTokenAsync(_scopes);
            }
            catch (MsalException msalex)
            {
                ResultText.Text = $"Error Acquiring Token:{System.Environment.NewLine}{msalex}";
            }
        }
        catch (Exception ex)
        {
            ResultText.Text = $"Error Acquiring Token Silently:{System.Environment.NewLine}{ex}";
            return;
        }

        if (authResult != null)
        {
            ResultText.Text = await GetHttpContentWithToken(_graphAPIEndpoint, authResult.AccessToken);
            DisplayBasicTokenInfo(authResult);
            this.SignOutButton.Visibility = Visibility.Visible;
        }
    }
}
```

<!--start-collapse-->
### <a name="more-information"></a>További információ
#### <a name="getting-a-user-token-interactive"></a>Egy felhasználói jogkivonat interaktív beolvasása
Hívása hello `AcquireTokenAsync` módszer eredményezi ablak, amely felszólítja a felhasználó toosign hello. Alkalmazások általában egy felhasználó toosign az interaktív hello tooaccess van szükségük, amikor első alkalommal védett erőforrás, vagy sikertelen, ha egy figyelő művelet tooacquire jogkivonat (pl. hello jelszó lejárt).

#### <a name="getting-a-user-token-silently"></a>A felhasználói token első beavatkozás nélkül
`AcquireTokenSilentAsync`kezeli a token kérése és -megújítás felhasználói beavatkozás nélkül. Után `AcquireTokenAsync` hajtanak végre hello első alkalommal `AcquireTokenSilentAsync` hello használt szokásos módszer tooobtain használt a jogkivonatokat tooaccess későbbi hívások - erőforrások védelmét szolgálja hívások toorequest vagy megújítása jogkivonatok beavatkozás nélkül történik.
Végül `AcquireTokenSilentAsync` sikertelen lesz, – pl. hello felhasználó rendelkezik-e kijelentkezteti, vagy egy másik eszközön a jelszó megváltozott. Amikor MSAL azt észleli, hogy hello probléma oldható meg azzal, hogy egy interaktív műveletet, akkor következik be egy `MsalUiRequiredException`. Az alkalmazás kezeli ezt a kivételt, két módon:

1.  Hívás elleni `AcquireTokenAsync` azonnal, ennek eredményeképpen hello toosign a felhasználótól. Ebben a mintában általában az online alkalmazások használják, ha nincs kapcsolat nélküli tartalom hello alkalmazásban hello felhasználó számára elérhető. hello interaktív telepítő által létrehozott mintánkban Ez a minta: tekintheti meg a művelet hello hello minta végrehajtása először: felhasználó nem használta a hello alkalmazást, mert `PublicClientApp.Users.FirstOrDefault()` fogja tartalmazni null értékű, és egy `MsalUiRequiredException` kivételt fog jelezni. hello hello mintában kód, majd leírók kivétel hello meghívásával `AcquireTokenAsync` eredményezve hello toosign a felhasználótól.

2.  Alkalmazások is készíthet, amelyek egy interaktív bejelentkezés szükség, hogy hello felhasználó kiválaszthatja a hello megfelelő időben toosign, vagy megpróbálhatja hello alkalmazás jelzést toohello felhasználó `AcquireTokenSilentAsync` egy későbbi időpontban. Ez általában akkor használatos, amikor hello felhasználói alatt megszakad nélkül tudja használni hello alkalmazás vonatkozó egyéb funkciók – például érhető el kapcsolat nélküli tartalmat hello alkalmazásban. Ebben az esetben hello felhasználói dönthet, ha szeretnék toosign tooaccess hello védett erőforrás, vagy toorefresh hello elavult információkat vagy eldöntheti, hogy az alkalmazás tooretry `AcquireTokenSilentAsync` amikor hálózati átmenetileg nem érhető el állapota után állítják vissza.
<!--end-collapse-->

## <a name="call-hello-microsoft-graph-api-using-hello-token-you-just-obtained"></a>Csak kapott hello tokent hello Microsoft Graph API hívása

1. Adja hozzá a hello alatti tooyour új módszer `MainWindow.xaml.cs`. hello metódus használt toomake egy `GET` kérelmet az engedélyezés fejlécet Graph API:

```csharp
/// <summary>
/// Perform an HTTP GET request tooa URL using an HTTP Authorization header
/// </summary>
/// <param name="url">hello URL</param>
/// <param name="token">hello token</param>
/// <returns>String containing hello results of hello GET operation</returns>
public async Task<string> GetHttpContentWithToken(string url, string token)
{
    var httpClient = new System.Net.Http.HttpClient();
    System.Net.Http.HttpResponseMessage response;
    try
    {
        var request = new System.Net.Http.HttpRequestMessage(System.Net.Http.HttpMethod.Get, url);
        //Add hello token in Authorization header
        request.Headers.Authorization = new System.Net.Http.Headers.AuthenticationHeaderValue("Bearer", token);
        response = await httpClient.SendAsync(request);
        var content = await response.Content.ReadAsStringAsync();
        return content;
    }
    catch (Exception ex)
    {
        return ex.ToString();
    }
}
```
<!--start-collapse-->
### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a>További információ a ellen védett API REST hívás

A mintaalkalmazás az hello `GetHttpContentWithToken` metódus használt toomake HTTP `GET` egy jogkivonat és majd visszatérési hello tartalom toohello hívó igénylő védett erőforrás kérelmet. Ez a módszer szerzett hello token helyez hello *HTTP Authorization fejlécet*. Ez a minta hello erőforrás hello Microsoft Graph API *me* végpont – hello felhasználói profil adatait jeleníti meg.
<!--end-collapse-->

## <a name="add-a-method-toosign-out-hello-user"></a>Egy metódus toosign kimenő hello felhasználó hozzáadása

1. Adja hozzá a következő metódus tooyour hello `MainWindow.xaml.cs` toosign hello felhasználói ki:

```csharp
/// <summary>
/// Sign out hello current user
/// </summary>
private void SignOutButton_Click(object sender, RoutedEventArgs e)
{
    if (App.PublicClientApp.Users.Any())
    {
        try
        {
            App.PublicClientApp.Remove(App.PublicClientApp.Users.FirstOrDefault());
            this.ResultText.Text = "User has signed-out";
            this.CallGraphButton.Visibility = Visibility.Visible;
            this.SignOutButton.Visibility = Visibility.Collapsed;
        }
        catch (MsalException ex)
        {
            ResultText.Text = $"Error signing-out user: {ex.Message}";
        }
    }
}
```
<!--start-collapse-->
### <a name="more-info-on-sign-out"></a>További információ a kijelentkezési

`SignOutButton_Click`eltávolítja a felhasználó MSAL felhasználói gyorsítótárból hello – ez gyakorlatilag jelzi a MSAL tooforget hello aktuális felhasználó, egy későbbi kérés tooacquire jogkivonat csak sikeres lesz, ha azt toobe interaktív.
Bár ez a példa hello alkalmazás támogatja az egy-egy felhasználóhoz, MSAL támogatja-e a forgatókönyvekben, ahol több fiók lehet bejelentkezve: hello ugyanannyi időt vesz igénybe – példa, hogy e-mail alkalmazást ahol a felhasználó rendelkezik-e a több fiók.
<!--end-collapse-->

## <a name="display-basic-token-information"></a>Alapvető lexikális elem adatainak megjelenítése

1. Adja hozzá a következő metódus tootooyour hello `MainWindow.xaml.cs` toodisplay hello token alapvető információkat:

```csharp
/// <summary>
/// Display basic information contained in hello token
/// </summary>
private void DisplayBasicTokenInfo(AuthenticationResult authResult)
{
    TokenInfoText.Text = "";
    if (authResult != null)
    {
        TokenInfoText.Text += $"Name: {authResult.User.Name}" + Environment.NewLine;
        TokenInfoText.Text += $"Username: {authResult.User.DisplayableId}" + Environment.NewLine;
        TokenInfoText.Text += $"Token Expires: {authResult.ExpiresOn.ToLocalTime()}" + Environment.NewLine;
        TokenInfoText.Text += $"Access Token: {authResult.AccessToken}" + Environment.NewLine;
    }
}
```
<!--start-collapse-->
### <a name="more-information"></a>További információ

Jogkivonatok keresztül szerzett *OpenID Connect* egy felhasználó által érintett konfigurációkon toohello szűk köre is tartalmaz. `DisplayBasicTokenInfo`hello token alapvető információkat jeleníti meg: például hello felhasználó megjelenítendő nevét és Azonosítóját, valamint hello jogkivonat lejárati dátum- és hello karakterlánc, amely hello hozzáférési jogkivonat magát. Ez az információ akkor toosee jelenik meg. Azt, kattintson a hello *Microsoft Graph API hívása* többször gombra, és tekintse meg, hogy hello ugyanezt a tokent további kérelmeknél használja fel újra. Amikor MSAL úgy dönt, hogy az idő toorenew hello token kiterjesztendő hello lejárati dátumot is megtekinthető.
<!--end-collapse-->

