
<span data-ttu-id="1392f-101">Az előző példában bemutatta szabványos bejelentkezéshez, ami megköveteli, hogy az ügyfél, mind az identitásszolgáltató, és a háttér-Azure szolgáltatás kapcsolódni az alkalmazás minden indításakor.</span><span class="sxs-lookup"><span data-stu-id="1392f-101">The previous example showed a standard sign-in, which requires the client to contact both the identity provider and the back-end Azure service every time the app starts.</span></span> <span data-ttu-id="1392f-102">Ez a módszer nem hatékony, és akkor használati kapcsolatos problémák, ha sok ügyfél próbál meg egyidejűleg indítsa el az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1392f-102">This method is inefficient, and you can have usage-related issues if many customers try to start your app simultaneously.</span></span> <span data-ttu-id="1392f-103">Egy jobb megoldás, a gyorsítótár az engedélyezési jogkivonatot az Azure-szolgáltatás által visszaadott, és próbálja meg használni az első olyan szolgáltató alapú bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="1392f-103">A better approach is to cache the authorization token returned by the Azure service, and try to use this first before using a provider-based sign-in.</span></span>

> [!NOTE]
> <span data-ttu-id="1392f-104">A háttér-függetlenül használja-e ügyfél által felügyelt vagy a szolgáltatás által kezelt hitelesítés az Azure szolgáltatás által kiadott tokennek képes gyorsítótárazni.</span><span class="sxs-lookup"><span data-stu-id="1392f-104">You can cache the token issued by the back-end Azure service regardless of whether you are using client-managed or service-managed authentication.</span></span> <span data-ttu-id="1392f-105">Ez az oktatóanyag a szolgáltatás által kezelt hitelesítést használ.</span><span class="sxs-lookup"><span data-stu-id="1392f-105">This tutorial uses service-managed authentication.</span></span>
>
>

1. <span data-ttu-id="1392f-106">Nyissa meg a ToDoActivity.java fájlban, és adja hozzá a következő importálási utasításokat:</span><span class="sxs-lookup"><span data-stu-id="1392f-106">Open the ToDoActivity.java file and add the following import statements:</span></span>

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;
2. <span data-ttu-id="1392f-107">A következő tagokat adjanak hozzá a `ToDoActivity` osztály.</span><span class="sxs-lookup"><span data-stu-id="1392f-107">Add the following members to the `ToDoActivity` class.</span></span>

        public static final String SHAREDPREFFILE = "temp";    
        public static final String USERIDPREF = "uid";    
        public static final String TOKENPREF = "tkn";    
3. <span data-ttu-id="1392f-108">A ToDoActivity.java fájlban adja hozzá a következő definícióját a `cacheUserToken` metódust.</span><span class="sxs-lookup"><span data-stu-id="1392f-108">In the ToDoActivity.java file, add the following definition for the `cacheUserToken` method.</span></span>

        private void cacheUserToken(MobileServiceUser user)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            Editor editor = prefs.edit();
            editor.putString(USERIDPREF, user.getUserId());
            editor.putString(TOKENPREF, user.getAuthenticationToken());
            editor.commit();
        }    

    <span data-ttu-id="1392f-109">Ez a módszer is meg van jelölve személyes beállításokat szabályozó fájlban tárolja a felhasználói Azonosítót és a jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="1392f-109">This method stores the user ID and token in a preference file that is marked private.</span></span> <span data-ttu-id="1392f-110">Hogy más alkalmazások az eszközön nincs hozzáférése a jogkivonatot, ez a gyorsítótár a hozzáférést kell védeni.</span><span class="sxs-lookup"><span data-stu-id="1392f-110">This should protect access to the cache so that other apps on the device do not have access to the token.</span></span> <span data-ttu-id="1392f-111">A beállítás ki az alkalmazás elkülönített.</span><span class="sxs-lookup"><span data-stu-id="1392f-111">The preference is sandboxed for the app.</span></span> <span data-ttu-id="1392f-112">Azonban ha valaki hozzáfér az eszközre, akkor lehet, hogy előfordulhat, hogy a jogkivonat gyorsítótára más módon hozzáférést kapnak.</span><span class="sxs-lookup"><span data-stu-id="1392f-112">However, if someone gains access to the device, it is possible that they may gain access to the token cache through other means.</span></span>

   > [!NOTE]
   > <span data-ttu-id="1392f-113">Ha a adatokhoz való hozzáférés a jogkivonatokhoz szigorúan bizalmas minősül, és hozzáférhet az eszközt valaki, további védelmet biztosíthat a titkosítás, a jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="1392f-113">You can further protect the token with encryption, if token access to your data is considered highly sensitive and someone may gain access to the device.</span></span> <span data-ttu-id="1392f-114">Teljesen biztonságos megoldást ebben az oktatóanyagban terjed, és attól függ, a biztonsági követelményeinek.</span><span class="sxs-lookup"><span data-stu-id="1392f-114">A completely secure solution is beyond the scope of this tutorial, however, and depends on your security requirements.</span></span>
   >
   >
4. <span data-ttu-id="1392f-115">A ToDoActivity.java fájlban adja hozzá a következő definícióját a `loadUserTokenCache` metódust.</span><span class="sxs-lookup"><span data-stu-id="1392f-115">In the ToDoActivity.java file, add the following definition for the `loadUserTokenCache` method.</span></span>

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
5. <span data-ttu-id="1392f-116">Az a *ToDoActivity.java* fájlt, cserélje le a `authenticate` metódus a következő módszerrel, amely jogkivonatok gyorsítótárát használja.</span><span class="sxs-lookup"><span data-stu-id="1392f-116">In the *ToDoActivity.java* file, replace the `authenticate` method with the following method, which uses a token cache.</span></span> <span data-ttu-id="1392f-117">Módosítsa a bejelentkezés-szolgáltató, ha szeretné használni a Google nem.</span><span class="sxs-lookup"><span data-stu-id="1392f-117">Change the login provider if you want to use an account other than Google.</span></span>

        private void authenticate() {
            // We first try to load a token cache if one exists.
            if (loadUserTokenCache(mClient))
            {
                createTable();
            }
            // If we failed to load a token cache, login and create a token cache
            else
            {
                // Login using the Google provider.    
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
6. <span data-ttu-id="1392f-118">Hozhat létre. az alkalmazás és a teszt hitelesítési érvényes fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="1392f-118">Build the app and test authentication using a valid account.</span></span> <span data-ttu-id="1392f-119">Legalább két alkalommal futtassa.</span><span class="sxs-lookup"><span data-stu-id="1392f-119">Run it at least twice.</span></span> <span data-ttu-id="1392f-120">Az első futtatás során jelenít meg bejelentkezni, és hozzon létre a jogkivonatok gyorsítótárát kell kapnia.</span><span class="sxs-lookup"><span data-stu-id="1392f-120">During the first run, you should receive a prompt to sign in and create the token cache.</span></span> <span data-ttu-id="1392f-121">Ezután minden egyes futtatásához megkísérli betölteni a hitelesítési jogkivonat gyorsítótárában.</span><span class="sxs-lookup"><span data-stu-id="1392f-121">After that, each run attempts to load the token cache for authentication.</span></span> <span data-ttu-id="1392f-122">Ön nem köteles jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="1392f-122">You should not be required to sign in.</span></span>
