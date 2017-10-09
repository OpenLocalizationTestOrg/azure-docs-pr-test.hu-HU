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
## <a name="use-hello-microsoft-authentication-library-msal-tooget-a-token-for-hello-microsoft-graph-api"></a><span data-ttu-id="5c562-103">Hello Microsoft hitelesítési könyvtár (MSAL) tooget jogkivonat használata hello Microsoft Graph API-val</span><span class="sxs-lookup"><span data-stu-id="5c562-103">Use hello Microsoft Authentication Library (MSAL) tooget a token for hello Microsoft Graph API</span></span>

<span data-ttu-id="5c562-104">Ez a szakasz bemutatja, hogyan toouse MSAL tooget jogkivonat hello Microsoft Graph API-val.</span><span class="sxs-lookup"><span data-stu-id="5c562-104">This section shows how toouse MSAL tooget a token hello Microsoft Graph API.</span></span>

1.  <span data-ttu-id="5c562-105">A `MainWindow.xaml.cs`, MSAL könyvtár toohello osztály hello hivatkozás hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="5c562-105">In `MainWindow.xaml.cs`, add hello reference for MSAL library toohello class:</span></span>

```csharp
using Microsoft.Identity.Client;
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
<span data-ttu-id="5c562-106">Cserélje le <code>MainWindow</code> osztály kódot:</span><span class="sxs-lookup"><span data-stu-id="5c562-106">Replace <code>MainWindow</code> class code with:</span></span>
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
### <a name="more-information"></a><span data-ttu-id="5c562-107">További információ</span><span class="sxs-lookup"><span data-stu-id="5c562-107">More Information</span></span>
#### <a name="getting-a-user-token-interactive"></a><span data-ttu-id="5c562-108">Egy felhasználói jogkivonat interaktív beolvasása</span><span class="sxs-lookup"><span data-stu-id="5c562-108">Getting a user token interactive</span></span>
<span data-ttu-id="5c562-109">Hívása hello `AcquireTokenAsync` módszer eredményezi ablak, amely felszólítja a felhasználó toosign hello.</span><span class="sxs-lookup"><span data-stu-id="5c562-109">Calling hello `AcquireTokenAsync` method results in a window prompting hello user toosign in.</span></span> <span data-ttu-id="5c562-110">Alkalmazások általában egy felhasználó toosign az interaktív hello tooaccess van szükségük, amikor első alkalommal védett erőforrás, vagy sikertelen, ha egy figyelő művelet tooacquire jogkivonat (pl. hello jelszó lejárt).</span><span class="sxs-lookup"><span data-stu-id="5c562-110">Applications usually require a user toosign in interactively hello first time they need tooaccess a protected resource, or when a silent operation tooacquire a token fails (e.g. hello user’s password expired).</span></span>

#### <a name="getting-a-user-token-silently"></a><span data-ttu-id="5c562-111">A felhasználói token első beavatkozás nélkül</span><span class="sxs-lookup"><span data-stu-id="5c562-111">Getting a user token silently</span></span>
<span data-ttu-id="5c562-112">`AcquireTokenSilentAsync`kezeli a token kérése és -megújítás felhasználói beavatkozás nélkül.</span><span class="sxs-lookup"><span data-stu-id="5c562-112">`AcquireTokenSilentAsync` handles token acquisitions and renewal without any user interaction.</span></span> <span data-ttu-id="5c562-113">Után `AcquireTokenAsync` hajtanak végre hello első alkalommal `AcquireTokenSilentAsync` hello használt szokásos módszer tooobtain használt a jogkivonatokat tooaccess későbbi hívások - erőforrások védelmét szolgálja hívások toorequest vagy megújítása jogkivonatok beavatkozás nélkül történik.</span><span class="sxs-lookup"><span data-stu-id="5c562-113">After `AcquireTokenAsync` is executed for hello first time, `AcquireTokenSilentAsync` is hello usual method used tooobtain tokens used tooaccess protected resources for subsequent calls - as calls toorequest or renew tokens are made silently.</span></span>
<span data-ttu-id="5c562-114">Végül `AcquireTokenSilentAsync` sikertelen lesz, – pl. hello felhasználó rendelkezik-e kijelentkezteti, vagy egy másik eszközön a jelszó megváltozott.</span><span class="sxs-lookup"><span data-stu-id="5c562-114">Eventually, `AcquireTokenSilentAsync` will fail – e.g. hello user has signed out, or has changed their password on another device.</span></span> <span data-ttu-id="5c562-115">Amikor MSAL azt észleli, hogy hello probléma oldható meg azzal, hogy egy interaktív műveletet, akkor következik be egy `MsalUiRequiredException`.</span><span class="sxs-lookup"><span data-stu-id="5c562-115">When MSAL detects that hello issue can be resolved by requiring an interactive action, it fires an `MsalUiRequiredException`.</span></span> <span data-ttu-id="5c562-116">Az alkalmazás kezeli ezt a kivételt, két módon:</span><span class="sxs-lookup"><span data-stu-id="5c562-116">Your application can handle this exception in two ways:</span></span>

1.  <span data-ttu-id="5c562-117">Hívás elleni `AcquireTokenAsync` azonnal, ennek eredményeképpen hello toosign a felhasználótól.</span><span class="sxs-lookup"><span data-stu-id="5c562-117">Make a call against `AcquireTokenAsync` immediately, which results in prompting hello user toosign-in.</span></span> <span data-ttu-id="5c562-118">Ebben a mintában általában az online alkalmazások használják, ha nincs kapcsolat nélküli tartalom hello alkalmazásban hello felhasználó számára elérhető.</span><span class="sxs-lookup"><span data-stu-id="5c562-118">This pattern is usually used in online applications where there is no offline content in hello application available for hello user.</span></span> <span data-ttu-id="5c562-119">hello interaktív telepítő által létrehozott mintánkban Ez a minta: tekintheti meg a művelet hello hello minta végrehajtása először: felhasználó nem használta a hello alkalmazást, mert `PublicClientApp.Users.FirstOrDefault()` fogja tartalmazni null értékű, és egy `MsalUiRequiredException` kivételt fog jelezni.</span><span class="sxs-lookup"><span data-stu-id="5c562-119">hello sample generated by this guided setup uses this pattern: you can see it in action hello first time you execute hello sample: because no user ever used hello application, `PublicClientApp.Users.FirstOrDefault()` will contain a null value, and an `MsalUiRequiredException` exception will be thrown.</span></span> <span data-ttu-id="5c562-120">hello hello mintában kód, majd leírók kivétel hello meghívásával `AcquireTokenAsync` eredményezve hello toosign a felhasználótól.</span><span class="sxs-lookup"><span data-stu-id="5c562-120">hello code in hello sample then handles hello exception by calling `AcquireTokenAsync` resulting in prompting hello user toosign-in.</span></span>

2.  <span data-ttu-id="5c562-121">Alkalmazások is készíthet, amelyek egy interaktív bejelentkezés szükség, hogy hello felhasználó kiválaszthatja a hello megfelelő időben toosign, vagy megpróbálhatja hello alkalmazás jelzést toohello felhasználó `AcquireTokenSilentAsync` egy későbbi időpontban.</span><span class="sxs-lookup"><span data-stu-id="5c562-121">Applications can also make a visual indication toohello user that an interactive sign-in is required, so hello user can select hello right time toosign in, or hello application can retry `AcquireTokenSilentAsync` at a later time.</span></span> <span data-ttu-id="5c562-122">Ez általában akkor használatos, amikor hello felhasználói alatt megszakad nélkül tudja használni hello alkalmazás vonatkozó egyéb funkciók – például érhető el kapcsolat nélküli tartalmat hello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="5c562-122">This is usually used when hello user can use other functionality of hello application without being disrupted - for example, there is offline content available in hello application.</span></span> <span data-ttu-id="5c562-123">Ebben az esetben hello felhasználói dönthet, ha szeretnék toosign tooaccess hello védett erőforrás, vagy toorefresh hello elavult információkat vagy eldöntheti, hogy az alkalmazás tooretry `AcquireTokenSilentAsync` amikor hálózati átmenetileg nem érhető el állapota után állítják vissza.</span><span class="sxs-lookup"><span data-stu-id="5c562-123">In this case, hello user can decide when they want toosign in tooaccess hello protected resource, or toorefresh hello outdated information, or your application can decide tooretry `AcquireTokenSilentAsync` when network is restored after being unavailable temporarily.</span></span>
<!--end-collapse-->

## <a name="call-hello-microsoft-graph-api-using-hello-token-you-just-obtained"></a><span data-ttu-id="5c562-124">Csak kapott hello tokent hello Microsoft Graph API hívása</span><span class="sxs-lookup"><span data-stu-id="5c562-124">Call hello Microsoft Graph API using hello token you just obtained</span></span>

1. <span data-ttu-id="5c562-125">Adja hozzá a hello alatti tooyour új módszer `MainWindow.xaml.cs`.</span><span class="sxs-lookup"><span data-stu-id="5c562-125">Add hello new method below tooyour `MainWindow.xaml.cs`.</span></span> <span data-ttu-id="5c562-126">hello metódus használt toomake egy `GET` kérelmet az engedélyezés fejlécet Graph API:</span><span class="sxs-lookup"><span data-stu-id="5c562-126">hello method is used toomake a `GET` request against Graph API using an Authorize header:</span></span>

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
### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a><span data-ttu-id="5c562-127">További információ a ellen védett API REST hívás</span><span class="sxs-lookup"><span data-stu-id="5c562-127">More information on making a REST call against a protected API</span></span>

<span data-ttu-id="5c562-128">A mintaalkalmazás az hello `GetHttpContentWithToken` metódus használt toomake HTTP `GET` egy jogkivonat és majd visszatérési hello tartalom toohello hívó igénylő védett erőforrás kérelmet.</span><span class="sxs-lookup"><span data-stu-id="5c562-128">In this sample application, hello `GetHttpContentWithToken` method is used toomake an HTTP `GET` request against a protected resource that requires a token and then return hello content toohello caller.</span></span> <span data-ttu-id="5c562-129">Ez a módszer szerzett hello token helyez hello *HTTP Authorization fejlécet*.</span><span class="sxs-lookup"><span data-stu-id="5c562-129">This method adds hello acquired token in hello *HTTP Authorization header*.</span></span> <span data-ttu-id="5c562-130">Ez a minta hello erőforrás hello Microsoft Graph API *me* végpont – hello felhasználói profil adatait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="5c562-130">For this sample, hello resource is hello Microsoft Graph API *me* endpoint – which displays hello user's profile information.</span></span>
<!--end-collapse-->

## <a name="add-a-method-toosign-out-hello-user"></a><span data-ttu-id="5c562-131">Egy metódus toosign kimenő hello felhasználó hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5c562-131">Add a method toosign out hello user</span></span>

1. <span data-ttu-id="5c562-132">Adja hozzá a következő metódus tooyour hello `MainWindow.xaml.cs` toosign hello felhasználói ki:</span><span class="sxs-lookup"><span data-stu-id="5c562-132">Add hello following method tooyour `MainWindow.xaml.cs` toosign out hello user:</span></span>

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
### <a name="more-info-on-sign-out"></a><span data-ttu-id="5c562-133">További információ a kijelentkezési</span><span class="sxs-lookup"><span data-stu-id="5c562-133">More info on Sign-Out</span></span>

<span data-ttu-id="5c562-134">`SignOutButton_Click`eltávolítja a felhasználó MSAL felhasználói gyorsítótárból hello – ez gyakorlatilag jelzi a MSAL tooforget hello aktuális felhasználó, egy későbbi kérés tooacquire jogkivonat csak sikeres lesz, ha azt toobe interaktív.</span><span class="sxs-lookup"><span data-stu-id="5c562-134">`SignOutButton_Click` removes hello user from MSAL user cache – this will effectively tell MSAL tooforget hello current user so a future request tooacquire a token will only succeed if it is made toobe interactive.</span></span>
<span data-ttu-id="5c562-135">Bár ez a példa hello alkalmazás támogatja az egy-egy felhasználóhoz, MSAL támogatja-e a forgatókönyvekben, ahol több fiók lehet bejelentkezve: hello ugyanannyi időt vesz igénybe – példa, hogy e-mail alkalmazást ahol a felhasználó rendelkezik-e a több fiók.</span><span class="sxs-lookup"><span data-stu-id="5c562-135">Although hello application in this sample supports a single user, MSAL supports scenarios where multiple accounts can be signed-in at hello same time – an example is an email application where a user has multiple accounts.</span></span>
<!--end-collapse-->

## <a name="display-basic-token-information"></a><span data-ttu-id="5c562-136">Alapvető lexikális elem adatainak megjelenítése</span><span class="sxs-lookup"><span data-stu-id="5c562-136">Display Basic Token Information</span></span>

1. <span data-ttu-id="5c562-137">Adja hozzá a következő metódus tootooyour hello `MainWindow.xaml.cs` toodisplay hello token alapvető információkat:</span><span class="sxs-lookup"><span data-stu-id="5c562-137">Add hello following method tootooyour `MainWindow.xaml.cs` toodisplay basic information about hello token:</span></span>

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
### <a name="more-information"></a><span data-ttu-id="5c562-138">További információ</span><span class="sxs-lookup"><span data-stu-id="5c562-138">More Information</span></span>

<span data-ttu-id="5c562-139">Jogkivonatok keresztül szerzett *OpenID Connect* egy felhasználó által érintett konfigurációkon toohello szűk köre is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="5c562-139">Tokens acquired via *OpenID Connect* also contain a small subset of information pertinent toohello user.</span></span> <span data-ttu-id="5c562-140">`DisplayBasicTokenInfo`hello token alapvető információkat jeleníti meg: például hello felhasználó megjelenítendő nevét és Azonosítóját, valamint hello jogkivonat lejárati dátum- és hello karakterlánc, amely hello hozzáférési jogkivonat magát.</span><span class="sxs-lookup"><span data-stu-id="5c562-140">`DisplayBasicTokenInfo` displays basic information contained in hello token: for example, hello user's display name and ID, as well as hello token expiration date and hello string representing hello access token itself.</span></span> <span data-ttu-id="5c562-141">Ez az információ akkor toosee jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="5c562-141">This information is displayed for you toosee.</span></span> <span data-ttu-id="5c562-142">Azt, kattintson a hello *Microsoft Graph API hívása* többször gombra, és tekintse meg, hogy hello ugyanezt a tokent további kérelmeknél használja fel újra.</span><span class="sxs-lookup"><span data-stu-id="5c562-142">You can hit hello *Call Microsoft Graph API* button multiple times and see that hello same token was reused for subsequent requests.</span></span> <span data-ttu-id="5c562-143">Amikor MSAL úgy dönt, hogy az idő toorenew hello token kiterjesztendő hello lejárati dátumot is megtekinthető.</span><span class="sxs-lookup"><span data-stu-id="5c562-143">You can also see hello expiration date being extended when MSAL decides it is time toorenew hello token.</span></span>
<!--end-collapse-->

