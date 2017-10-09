
1. <span data-ttu-id="52934-101">A hello MainPage.xaml.cs projektfájlban, adja hozzá a hello következő **használatával** utasításokat:</span><span class="sxs-lookup"><span data-stu-id="52934-101">In hello MainPage.xaml.cs project file, add hello following **using** statements:</span></span>
   
        using System.Linq;        
        using Windows.Security.Credentials;
2. <span data-ttu-id="52934-102">Cserélje le a hello **AuthenticateAsync** hello kód a következő metódust:</span><span class="sxs-lookup"><span data-stu-id="52934-102">Replace hello **AuthenticateAsync** method with hello following code:</span></span>
   
        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;
   
            // This sample uses hello Facebook provider.
            var provider = MobileServiceAuthenticationProvider.Facebook;
   
            // Use hello PasswordVault toosecurely store and access credentials.
            PasswordVault vault = new PasswordVault();
            PasswordCredential credential = null;
   
            try
            {
                // Try tooget an existing credential from hello vault.
                credential = vault.FindAllByResource(provider.ToString()).FirstOrDefault();
            }
            catch (Exception)
            {
                // When there is no matching resource an error occurs, which we ignore.
            }
   
            if (credential != null)
            {
                // Create a user from hello stored credentials.
                user = new MobileServiceUser(credential.UserName);
                credential.RetrievePassword();
                user.MobileServiceAuthenticationToken = credential.Password;
   
                // Set hello user from hello stored credentials.
                App.MobileService.CurrentUser = user;
   
                // Consider adding a check toodetermine if hello token is 
                // expired, as shown in this post: http://aka.ms/jww5vp.
   
                success = true;
                message = string.Format("Cached credentials for user - {0}", user.UserId);
            }
            else
            {
                try
                {
                    // Login with hello identity provider.
                    user = await App.MobileService
                        .LoginAsync(provider);
   
                    // Create and store hello user credentials.
                    credential = new PasswordCredential(provider.ToString(),
                        user.UserId, user.MobileServiceAuthenticationToken);
                    vault.Add(credential);
   
                    success = true;
                    message = string.Format("You are now logged in - {0}", user.UserId);
                }
                catch (MobileServiceInvalidOperationException)
                {
                    message = "You must log in. Login Required";
                }
            }
   
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
   
            return success;
        }
   
    <span data-ttu-id="52934-103">Ezen verziója **AuthenticateAsync**, hello app megpróbál hello tárolt toouse hitelesítő adatok **PasswordVault** tooaccess hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="52934-103">In this version of **AuthenticateAsync**, hello app tries toouse credentials stored in hello **PasswordVault** tooaccess hello service.</span></span> <span data-ttu-id="52934-104">Rendszeres bejelentkezés is történik, amikor nincs nem tárolt hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="52934-104">A regular sign-in is also performed when there is no stored credential.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="52934-105">Előfordulhat, hogy egy gyorsítótárazott token lejárt, és jogkivonat lejáratáról is esetén fordul elő a hitelesítés után hello alkalmazás használatban van.</span><span class="sxs-lookup"><span data-stu-id="52934-105">A cached token may be expired, and token expiration can also occur after authentication when hello app is in use.</span></span> <span data-ttu-id="52934-106">Hogyan toodetermine a jogkivonat érvényessége lejárt, ha: toolearn [lejárt a hitelesítési tokenek keressen](http://aka.ms/jww5vp).</span><span class="sxs-lookup"><span data-stu-id="52934-106">toolearn how toodetermine if a token is expired, see [Check for expired authentication tokens](http://aka.ms/jww5vp).</span></span> <span data-ttu-id="52934-107">A megoldás toohandling hitelesítési hibák kapcsolódó tooexpiring jogkivonatokat, lásd: hello utáni [gyorsítótárazáshoz és kezelése az Azure Mobile Services lejárt jogkivonatok felügyelt SDK](http://blogs.msdn.com/b/carlosfigueira/archive/2014/03/13/caching-and-handling-expired-tokens-in-azure-mobile-services-managed-sdk.aspx).</span><span class="sxs-lookup"><span data-stu-id="52934-107">For a solution toohandling authorization errors related tooexpiring tokens, see hello post [Caching and handling expired tokens in Azure Mobile Services managed SDK](http://blogs.msdn.com/b/carlosfigueira/archive/2014/03/13/caching-and-handling-expired-tokens-in-azure-mobile-services-managed-sdk.aspx).</span></span> 
   > 
   > 
3. <span data-ttu-id="52934-108">Indítsa újra kétszer hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="52934-108">Restart hello app twice.</span></span>
   
    <span data-ttu-id="52934-109">Figyelje meg, hogy hello első indításkor, jelentkezzen be hello szolgáltató újra szükség.</span><span class="sxs-lookup"><span data-stu-id="52934-109">Notice that on hello first start-up, sign-in with hello provider is again required.</span></span> <span data-ttu-id="52934-110">Azonban hello második újraindításkor hello gyorsítótárazott hitelesítő adatokat használja, és a bejelentkezési elmarad.</span><span class="sxs-lookup"><span data-stu-id="52934-110">However, on hello second restart hello cached credentials are used and sign-in is bypassed.</span></span> 

