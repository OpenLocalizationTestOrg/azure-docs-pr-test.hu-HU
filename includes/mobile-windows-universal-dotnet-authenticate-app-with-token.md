
1. A hello MainPage.xaml.cs projektfájlban, adja hozzá a hello következő **használatával** utasításokat:
   
        using System.Linq;        
        using Windows.Security.Credentials;
2. Cserélje le a hello **AuthenticateAsync** hello kód a következő metódust:
   
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
   
    Ezen verziója **AuthenticateAsync**, hello app megpróbál hello tárolt toouse hitelesítő adatok **PasswordVault** tooaccess hello szolgáltatást. Rendszeres bejelentkezés is történik, amikor nincs nem tárolt hitelesítő adatok.
   
   > [!NOTE]
   > Előfordulhat, hogy egy gyorsítótárazott token lejárt, és jogkivonat lejáratáról is esetén fordul elő a hitelesítés után hello alkalmazás használatban van. Hogyan toodetermine a jogkivonat érvényessége lejárt, ha: toolearn [lejárt a hitelesítési tokenek keressen](http://aka.ms/jww5vp). A megoldás toohandling hitelesítési hibák kapcsolódó tooexpiring jogkivonatokat, lásd: hello utáni [gyorsítótárazáshoz és kezelése az Azure Mobile Services lejárt jogkivonatok felügyelt SDK](http://blogs.msdn.com/b/carlosfigueira/archive/2014/03/13/caching-and-handling-expired-tokens-in-azure-mobile-services-managed-sdk.aspx). 
   > 
   > 
3. Indítsa újra kétszer hello alkalmazást.
   
    Figyelje meg, hogy hello első indításkor, jelentkezzen be hello szolgáltató újra szükség. Azonban hello második újraindításkor hello gyorsítótárazott hitelesítő adatokat használja, és a bejelentkezési elmarad. 

