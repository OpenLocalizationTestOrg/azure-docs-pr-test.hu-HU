
hello előző példa bemutatta, egy szabványos bejelentkezéshez, ami hello ügyfél toocontact szükséges mindkét hello identity provider és hello Azure háttérszolgáltatásnak hello alkalmazás minden indításakor. Ez a módszer nem hatékony, és ha sok ügyfél próbálja toostart az alkalmazás egyidejűleg használati kapcsolatos probléma lehet. A jobb megoldás, toocache hello engedélyezési jogkivonat által visszaadott hello Azure-szolgáltatás, és próbálja meg toouse ez használata előtt a szolgáltató alapú bejelentkezés.

> [!NOTE]
> Hello háttér-függetlenül használja-e ügyfél által felügyelt vagy a szolgáltatás által kezelt hitelesítés az Azure szolgáltatás által kiállított hello token képes gyorsítótárazni. Ez az oktatóanyag a szolgáltatás által kezelt hitelesítést használ.
>
>

1. Nyissa meg a hello ToDoActivity.java fájlban, és adja hozzá a következő importálási utasításokat hello:

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;
2. Adja hozzá a következő tagok toohello hello `ToDoActivity` osztály.

        public static final String SHAREDPREFFILE = "temp";    
        public static final String USERIDPREF = "uid";    
        public static final String TOKENPREF = "tkn";    
3. Hello ToDoActivity.java fájlban adja hozzá a következő hello definíciója hello `cacheUserToken` metódust.

        private void cacheUserToken(MobileServiceUser user)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            Editor editor = prefs.edit();
            editor.putString(USERIDPREF, user.getUserId());
            editor.putString(TOKENPREF, user.getAuthenticationToken());
            editor.commit();
        }    

    Ez a módszer hello felhasználói Azonosítót és a token titkos megjelölt beállításokat szabályozó fájlban tárolja. Ez kell hozzáférést toohello gyorsítótár védelmére is, hogy hello eszközön található egyéb alkalmazások nem rendelkeznek toohello jogkivonatot. hello beállítása az elkülönített hello alkalmazás. Azonban valaki kap hozzáférést toohello eszköz, esetén lehetséges, hogy hozzáférési toohello jogkivonat gyorsítótára más módon is kapnak.

   > [!NOTE]
   > További védelmet biztosíthat az hello jogkivonat a titkosítás, ha a hozzáférés a jogkivonatokhoz tooyour adatok szigorúan bizalmas minősülnek, és valaki szerezhetnek toohello eszközt. Teljesen biztonságos megoldást ebben az oktatóanyagban hello terjed, és attól függ, a biztonsági követelményeinek.
   >
   >
4. Hello ToDoActivity.java fájlban adja hozzá a következő hello definíciója hello `loadUserTokenCache` metódust.

        private boolean loadUserTokenCache(MobileServiceClient client)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            String userId = prefs.getString(USERIDPREF, null);
            if (userId == null)
                return false;
            String token = prefs.getString(TOKENPREF, null);
            if (token == null)
                return false;

            MobileServiceUser user = new MobileServiceUser(userId);
            user.setAuthenticationToken(token);
            client.setCurrentUser(user);

            return true;
        }
5. A hello *ToDoActivity.java* fájlt, cserélje le a hello `authenticate` hello metódus, amely a token gyorsítótárát használja a következő metódust. Ha azt szeretné, hogy nem Google toouse hello bejelentkezési szolgáltató módosítása

        private void authenticate() {
            // We first try tooload a token cache if one exists.
            if (loadUserTokenCache(mClient))
            {
                createTable();
            }
            // If we failed tooload a token cache, login and create a token cache
            else
            {
                // Login using hello Google provider.    
                ListenableFuture<MobileServiceUser> mLogin = mClient.login(MobileServiceAuthenticationProvider.Google);

                Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                    @Override
                    public void onFailure(Throwable exc) {
                        createAndShowDialog("You must log in. Login Required", "Error");
                    }           
                    @Override
                    public void onSuccess(MobileServiceUser user) {
                        createAndShowDialog(String.format(
                                "You are now logged in - %1$2s",
                                user.getUserId()), "Success");
                        cacheUserToken(mClient.getCurrentUser());
                        createTable();    
                    }
                });
            }
        }
6. Build hello alkalmazás és a teszt hitelesítés egy érvényes fiókot. Legalább két alkalommal futtassa. Során hello először futtatja az a kérdés toosign fogadni és hello token gyorsítótár létrehozása. Minden egyes futtatásához ezt követően próbálja meg tooload hello jogkivonatok gyorsítótárát a hitelesítéshez. A szükséges toosign nem lehetnek.
