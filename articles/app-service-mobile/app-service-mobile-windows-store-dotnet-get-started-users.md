---
title: "aaaAdd authentication tooyour univerzális Windows Platform (UWP-) alkalmazás |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Azure App Service Mobile Apps tooauthenticate Identitásszolgáltatók, beleértve a különböző univerzális Windows Platform (UWP) alkalmazása felhasználóit: aad-ben, a Google, a Facebook, a Twitter és a Microsoft."
services: app-service\mobile
documentationcenter: windows
author: ggailey777
manager: panarasi
editor: 
ms.assetid: 6cffd951-893e-4ce5-97ac-86e3f5ad9466
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 07/05/2017
ms.author: panarasi
ms.openlocfilehash: ad4477e9509f1c40c33e71818e268f6857fe1e80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-authentication-tooyour-windows-app"></a><span data-ttu-id="54ae8-103">Hitelesítési tooyour Windows-alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="54ae8-103">Add authentication tooyour Windows app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

<span data-ttu-id="54ae8-104">Ez a témakör bemutatja, hogyan tooadd felhőalapú hitelesítés tooyour mobilalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="54ae8-104">This topic shows you how tooadd cloud-based authentication tooyour mobile app.</span></span> <span data-ttu-id="54ae8-105">Ebben az oktatóanyagban az Azure App Service által támogatott identitásszolgáltató használatával Mobile Apps adja hozzá hitelesítési toohello univerzális Windows Platform (UWP) gyorsútmutató-projekt.</span><span class="sxs-lookup"><span data-stu-id="54ae8-105">In this tutorial, you add authentication toohello Universal Windows Platform (UWP) quickstart project for Mobile Apps using an identity provider that is supported by Azure App Service.</span></span> <span data-ttu-id="54ae8-106">Sikeresen alatt hitelesítése és engedélyezése által a mobil-háttéralkalmazást, után hello felhasználói azonosító érték jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="54ae8-106">After being successfully authenticated and authorized by your Mobile App backend, hello user ID value is displayed.</span></span>

<span data-ttu-id="54ae8-107">Ez az oktatóanyag hello Mobile Apps gyors üzembe helyezés alapul.</span><span class="sxs-lookup"><span data-stu-id="54ae8-107">This tutorial is based on hello Mobile Apps quickstart.</span></span> <span data-ttu-id="54ae8-108">Először el kell végeznie hello oktatóanyag [Ismerkedés a Mobile Apps](app-service-mobile-windows-store-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="54ae8-108">You must first complete hello tutorial [Get started with Mobile Apps](app-service-mobile-windows-store-dotnet-get-started.md).</span></span>

## <span data-ttu-id="54ae8-109"><a name="register"></a>Regisztrálja az alkalmazást a hitelesítéshez, és az App Service hello konfigurálása</span><span class="sxs-lookup"><span data-stu-id="54ae8-109"><a name="register"></a>Register your app for authentication and configure hello App Service</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <span data-ttu-id="54ae8-110"><a name="redirecturl"></a>A toohello engedélyezett külső átirányítási URL-címek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="54ae8-110"><a name="redirecturl"></a>Add your app toohello Allowed External Redirect URLs</span></span>

<span data-ttu-id="54ae8-111">Biztonságos hitelesítéshez az szükséges, hogy az alkalmazás adja meg egy új URL-sémát.</span><span class="sxs-lookup"><span data-stu-id="54ae8-111">Secure authentication requires that you define a new URL scheme for your app.</span></span> <span data-ttu-id="54ae8-112">Ez lehetővé teszi, hogy hello authentication rendszer tooredirect hátsó tooyour alkalmazás hello hitelesítési folyamat befejezése után.</span><span class="sxs-lookup"><span data-stu-id="54ae8-112">This allows hello authentication system tooredirect back tooyour app once hello authentication process is complete.</span></span> <span data-ttu-id="54ae8-113">Ebben az oktatóanyagban hello URL-séma használjuk _appname_ egész.</span><span class="sxs-lookup"><span data-stu-id="54ae8-113">In this tutorial, we use hello URL scheme _appname_ throughout.</span></span> <span data-ttu-id="54ae8-114">Bármely választja URL-sémát is használhatja.</span><span class="sxs-lookup"><span data-stu-id="54ae8-114">However, you can use any URL scheme you choose.</span></span> <span data-ttu-id="54ae8-115">Egyedi tooyour mobilalkalmazás kell lennie.</span><span class="sxs-lookup"><span data-stu-id="54ae8-115">It should be unique tooyour mobile application.</span></span> <span data-ttu-id="54ae8-116">tooenable hello átirányítási hello kiszolgáló oldalán:</span><span class="sxs-lookup"><span data-stu-id="54ae8-116">tooenable hello redirection on hello server side:</span></span>

1. <span data-ttu-id="54ae8-117">Hello [Azure-portálon] válassza ki az App Service.</span><span class="sxs-lookup"><span data-stu-id="54ae8-117">In hello [Azure portal], select your App Service.</span></span>

2. <span data-ttu-id="54ae8-118">Kattintson a hello **hitelesítési / engedélyezési** menüjét.</span><span class="sxs-lookup"><span data-stu-id="54ae8-118">Click hello **Authentication / Authorization** menu option.</span></span>

3. <span data-ttu-id="54ae8-119">A hello **engedélyezett külső átirányítási URL-t**, adja meg `url_scheme_of_your_app://easyauth.callback`.</span><span class="sxs-lookup"><span data-stu-id="54ae8-119">In hello **Allowed External Redirect URLs**, enter `url_scheme_of_your_app://easyauth.callback`.</span></span>  <span data-ttu-id="54ae8-120">Hello **url_scheme_of_your_app** hello URL-sémát a mobilalkalmazás Ez a karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="54ae8-120">hello **url_scheme_of_your_app** in this string is hello URL Scheme for your mobile application.</span></span>  <span data-ttu-id="54ae8-121">Akkor érdemes követnie normál URL-cím meghatározása (használata betűket és számokat csak, és betűvel kezdődik) protokoll.</span><span class="sxs-lookup"><span data-stu-id="54ae8-121">It should follow normal URL specification for a protocol (use letters and numbers only, and start with a letter).</span></span>  <span data-ttu-id="54ae8-122">Meg kell győződnie, jegyezze fel az Ön által mivel kell tooadjust hello több helyen URL-sémát a mobilalkalmazás kódot hello karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="54ae8-122">You should make a note of hello string that you choose as you will need tooadjust your mobile application code with hello URL Scheme in several places.</span></span>

4. <span data-ttu-id="54ae8-123">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="54ae8-123">Click **OK**.</span></span>

5. <span data-ttu-id="54ae8-124">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="54ae8-124">Click **Save**.</span></span>

## <span data-ttu-id="54ae8-125"><a name="permissions"></a>Engedélyek tooauthenticated felhasználók korlátozása</span><span class="sxs-lookup"><span data-stu-id="54ae8-125"><a name="permissions"></a>Restrict permissions tooauthenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

<span data-ttu-id="54ae8-126">Most ellenőrizheti, hogy a névtelen hozzáférés tooyour háttér le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="54ae8-126">Now, you can verify that anonymous access tooyour backend has been disabled.</span></span> <span data-ttu-id="54ae8-127">Hello UWP alkalmazás projekt esetében állítsa be indítási projektként hello telepítheti és futtathatja hello alkalmazást; Győződjön meg arról, hogy a 401-es (nem engedélyezett) egy állapotkód: nem kezelt kivétel hello alkalmazás indítása után jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="54ae8-127">With hello UWP app project set as hello start-up project, deploy and run hello app; verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after hello app starts.</span></span> <span data-ttu-id="54ae8-128">Ez akkor fordul elő, mert megpróbál tooaccess a Mobile App kód, nem hitelesített felhasználónak hello alkalmazás, de hello *TodoItem* tábla most hitelesítést igényel.</span><span class="sxs-lookup"><span data-stu-id="54ae8-128">This happens because hello app attempts tooaccess your Mobile App Code as an unauthenticated user, but hello *TodoItem* table now requires authentication.</span></span>

<span data-ttu-id="54ae8-129">Hello app tooauthenticate felhasználók frissíti a következő erőforrások kér az App Service előtt.</span><span class="sxs-lookup"><span data-stu-id="54ae8-129">Next, you will update hello app tooauthenticate users before requesting resources from your App Service.</span></span>

## <span data-ttu-id="54ae8-130"><a name="add-authentication"></a>Hitelesítési toohello alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="54ae8-130"><a name="add-authentication"></a>Add authentication toohello app</span></span>
1. <span data-ttu-id="54ae8-131">Hello UWP-alkalmazásprojektet a MainPage.xaml.cs fájlban, és adja hozzá a következő kódrészletet hello:</span><span class="sxs-lookup"><span data-stu-id="54ae8-131">In hello UWP app project file MainPage.xaml.cs and add hello following code snippet:</span></span>
   
        // Define a member variable for storing hello signed-in user. 
        private MobileServiceUser user;
   
        // Define a method that performs hello authentication process
        // using a Facebook sign-in. 
        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;
            try
            {
                // Change 'MobileService' toohello name of your MobileServiceClient instance.
                // Sign-in using Facebook authentication.
                user = await App.MobileService
                    .LoginAsync(MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
                message =
                    string.Format("You are now signed in - {0}", user.UserId);
   
                success = true;
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }
   
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
            return success;
        }
   
    <span data-ttu-id="54ae8-132">Ezt a kódot a Facebook-bejelentkezéskor hello felhasználó hitelesíti.</span><span class="sxs-lookup"><span data-stu-id="54ae8-132">This code authenticates hello user with a Facebook login.</span></span> <span data-ttu-id="54ae8-133">Ha használja az identitásszolgáltató Facebook-on kívül, hello értékének módosítása **MobileServiceAuthenticationProvider** a szolgáltató toohello érték felett.</span><span class="sxs-lookup"><span data-stu-id="54ae8-133">If you are using an identity provider other than Facebook, change hello value of **MobileServiceAuthenticationProvider** above toohello value for your provider.</span></span>
2. <span data-ttu-id="54ae8-134">Cserélje le a hello **OnNavigatedTo()** MainPage.xaml.cs metódust.</span><span class="sxs-lookup"><span data-stu-id="54ae8-134">Replace hello **OnNavigatedTo()** method in MainPage.xaml.cs.</span></span> <span data-ttu-id="54ae8-135">Ezután hozzáadhat egy **bejelentkezés** gomb toohello alkalmazás, amely elindítja a hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="54ae8-135">Next, you will add a **Sign in** button toohello app that triggers authentication.</span></span>

        protected override async void OnNavigatedTo(NavigationEventArgs e)
        {
            if (e.Parameter is Uri)
            {
                App.MobileService.ResumeWithURL(e.Parameter as Uri);
            }
        }

3. <span data-ttu-id="54ae8-136">Adja hozzá a következő kód részlet toohello MainPage.xaml.cs hello:</span><span class="sxs-lookup"><span data-stu-id="54ae8-136">Add hello following code snippet toohello MainPage.xaml.cs:</span></span>
   
        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login hello user and then load data from hello mobile app.
            if (await AuthenticateAsync())
            {
                // Switch hello buttons and load items from hello mobile app.
                ButtonLogin.Visibility = Visibility.Collapsed;
                ButtonSave.Visibility = Visibility.Visible;
                //await InitLocalStoreAsync(); //offline sync support.
                await RefreshTodoItems();
            }
        }
4. <span data-ttu-id="54ae8-137">Nyissa meg a hello MainPage.xaml projektfájlt, keresse meg, amely meghatározza a hello hello elem **mentése** gombra, és cserélje le a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="54ae8-137">Open hello MainPage.xaml project file, locate hello element that defines hello **Save** button and replace it with hello following code:</span></span>
   
        <Button Name="ButtonSave" Visibility="Collapsed" Margin="0,8,8,0" 
                Click="ButtonSave_Click">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Add"/>
                <TextBlock Margin="5">Save</TextBlock>
            </StackPanel>
        </Button>
        <Button Name="ButtonLogin" Visibility="Visible" Margin="0,8,8,0" 
                Click="ButtonLogin_Click" TabIndex="0">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Permissions"/>
                <TextBlock Margin="5">Sign in</TextBlock> 
            </StackPanel>
        </Button>
5. <span data-ttu-id="54ae8-138">Adja hozzá a következő kód részlet toohello App.xaml.cs hello:</span><span class="sxs-lookup"><span data-stu-id="54ae8-138">Add hello following code snippet toohello App.xaml.cs:</span></span>

        protected override void OnActivated(IActivatedEventArgs args)
        {
            if (args.Kind == ActivationKind.Protocol)
            {
                ProtocolActivatedEventArgs protocolArgs = args as ProtocolActivatedEventArgs;
                Frame content = Window.Current.Content as Frame;
                if (content.Content.GetType() == typeof(MainPage))
                {
                    content.Navigate(typeof(MainPage), protocolArgs.Uri);
                }
            }
            Window.Current.Activate();
            base.OnActivated(args);
        }
6. <span data-ttu-id="54ae8-139">Nyissa meg Package.appxmanifest fájlt, keresse meg a túl**nyilatkozatok**, a **elérhető nyilatkozatok** legördülő listában válassza ki **protokoll** kattintson **hozzáadása** gombra.</span><span class="sxs-lookup"><span data-stu-id="54ae8-139">Open Package.appxmanifest file, navigate too**Declarations**, in **Available Declarations** dropdown list, select **Protocol** and click **Add** button.</span></span> <span data-ttu-id="54ae8-140">Mostantól konfigurálhatja az hello **tulajdonságok** a hello **protokoll** nyilatkozatot.</span><span class="sxs-lookup"><span data-stu-id="54ae8-140">Now configure hello **Properties** of hello **Protocol** declaration.</span></span> <span data-ttu-id="54ae8-141">A **megjelenített név**, adja hozzá az alkalmazás toodisplay toousers kívánja hello nevét.</span><span class="sxs-lookup"><span data-stu-id="54ae8-141">In **Display name**, add hello name you wish toodisplay toousers of your application.</span></span> <span data-ttu-id="54ae8-142">A **neve**, vegye fel a {url_scheme_of_your_app}.</span><span class="sxs-lookup"><span data-stu-id="54ae8-142">In **Name**, add your {url_scheme_of_your_app}.</span></span>
7. <span data-ttu-id="54ae8-143">Nyomja le az ENTER hello F5 kulcs toorun hello app, kattintson a hello **bejelentkezés** gombra, és jelentkezzen be a választott identitásszolgáltató hello az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="54ae8-143">Press hello F5 key toorun hello app, click hello **Sign in** button, and sign into hello app with your chosen identity provider.</span></span> <span data-ttu-id="54ae8-144">Miután a bejelentkezés sikeres, hello app hibák nélkül fut, és képes tooquery háttéralkalmazásának, és gondoskodjon a frissítések toodata.</span><span class="sxs-lookup"><span data-stu-id="54ae8-144">After your sign-in is successful, hello app runs without errors and you are able tooquery your backend and make updates toodata.</span></span>

## <span data-ttu-id="54ae8-145"><a name="tokens"></a>Tárolja a hello hitelesítésére szolgáló jogkivonat hello ügyfél</span><span class="sxs-lookup"><span data-stu-id="54ae8-145"><a name="tokens"></a>Store hello authentication token on hello client</span></span>
<span data-ttu-id="54ae8-146">hello előző példa mindkét hello identitásszolgáltató bemutatta, egy szabványos bejelentkezéshez, ami hello ügyfél toocontact igényel, és az App Service hello hello alkalmazás minden indításakor.</span><span class="sxs-lookup"><span data-stu-id="54ae8-146">hello previous example showed a standard sign-in, which requires hello client toocontact both hello identity provider and hello App Service every time that hello app starts.</span></span> <span data-ttu-id="54ae8-147">Nem csak nem hatékony, futtathatja az ezzel a módszerrel történő használat-konfigurációelemmel kapcsolatos problémák toostart végezzen sok ügyfél app: hello ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="54ae8-147">Not only is this method inefficient, you can run into usage-relates issues should many customers try toostart you app at hello same time.</span></span> <span data-ttu-id="54ae8-148">A jobb megoldás, toocache hello engedélyezési jogkivonat által visszaadott az App Service és a try toouse ez használata előtt a szolgáltató alapú bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="54ae8-148">A better approach is toocache hello authorization token returned by your App Service and try toouse this first before using a provider-based sign-in.</span></span>

> [!NOTE]
> <span data-ttu-id="54ae8-149">Használ-e ügyfél által felügyelt vagy a szolgáltatás által kezelt hitelesítési függetlenül alkalmazásszolgáltatások által kibocsátott hello token képes gyorsítótárazni.</span><span class="sxs-lookup"><span data-stu-id="54ae8-149">You can cache hello token issued by App Services regardless of whether you are using client-managed or service-managed authentication.</span></span> <span data-ttu-id="54ae8-150">Ez az oktatóanyag a szolgáltatás által kezelt hitelesítést használ.</span><span class="sxs-lookup"><span data-stu-id="54ae8-150">This tutorial uses service-managed authentication.</span></span>
> 
> 

[!INCLUDE [mobile-windows-universal-dotnet-authenticate-app-with-token](../../includes/mobile-windows-universal-dotnet-authenticate-app-with-token.md)]

## <a name="next-steps"></a><span data-ttu-id="54ae8-151">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="54ae8-151">Next steps</span></span>
<span data-ttu-id="54ae8-152">Most, hogy elvégezte az oktatóanyag az egyszerű hitelesítés, vegye figyelembe az alábbi oktatóanyagok hello tooone a Folytatás:</span><span class="sxs-lookup"><span data-stu-id="54ae8-152">Now that you completed this basic authentication tutorial, consider continuing on tooone of hello following tutorials:</span></span>

* [<span data-ttu-id="54ae8-153">Leküldéses értesítések tooyour alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="54ae8-153">Add push notifications tooyour app</span></span>](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  <span data-ttu-id="54ae8-154">Ismerje meg, hogyan támogatják a különböző tooadd leküldéses értesítések tooyour alkalmazást, és a mobilalkalmazás háttér toouse Azure Notification Hubs toosend leküldéses értesítések konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="54ae8-154">Learn how tooadd push notifications support tooyour app and configure your Mobile App backend toouse Azure Notification Hubs toosend push notifications.</span></span>
* [<span data-ttu-id="54ae8-155">Az offline szinkronizálás engedélyezése az alkalmazás számára</span><span class="sxs-lookup"><span data-stu-id="54ae8-155">Enable offline sync for your app</span></span>](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  <span data-ttu-id="54ae8-156">Ismerje meg, hogyan támogatják a kapcsolat nélküli tooadd a az alkalmazás egy Mobile Apps-háttéralkalmazás segítségével.</span><span class="sxs-lookup"><span data-stu-id="54ae8-156">Learn how tooadd offline support your app using an Mobile App backend.</span></span> <span data-ttu-id="54ae8-157">Kapcsolat nélküli szinkronizálás lehetővé teszi, hogy a végfelhasználók toointeract mobilalkalmazást&mdash;megtekintését, hozzáadását és módosítását adatok&mdash;akkor is, ha nincs hálózati kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="54ae8-157">Offline sync allows end-users toointeract with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span>

<!-- URLs. -->
[Get started with your mobile app]: app-service-mobile-windows-store-dotnet-get-started.md
