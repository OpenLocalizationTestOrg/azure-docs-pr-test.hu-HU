
<span data-ttu-id="bb495-101">hello előző példa bemutatta, egy szabványos bejelentkezéshez, ami hello ügyfél toocontact szükséges mindkét hello identity provider és hello Azure háttérszolgáltatásnak hello alkalmazás minden indításakor.</span><span class="sxs-lookup"><span data-stu-id="bb495-101">hello previous example showed a standard sign-in, which requires hello client toocontact both hello identity provider and hello back-end Azure service every time hello app starts.</span></span> <span data-ttu-id="bb495-102">Ez a módszer nem hatékony, és ha sok ügyfél próbálja toostart az alkalmazás egyidejűleg használati kapcsolatos probléma lehet.</span><span class="sxs-lookup"><span data-stu-id="bb495-102">This method is inefficient, and you can have usage-related issues if many customers try toostart your app simultaneously.</span></span> <span data-ttu-id="bb495-103">A jobb megoldás, toocache hello engedélyezési jogkivonat által visszaadott hello Azure-szolgáltatás, és próbálja meg toouse ez használata előtt a szolgáltató alapú bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="bb495-103">A better approach is toocache hello authorization token returned by hello Azure service, and try toouse this first before using a provider-based sign-in.</span></span>

> [!NOTE]
> <span data-ttu-id="bb495-104">Hello háttér-függetlenül használja-e ügyfél által felügyelt vagy a szolgáltatás által kezelt hitelesítés az Azure szolgáltatás által kiállított hello token képes gyorsítótárazni.</span><span class="sxs-lookup"><span data-stu-id="bb495-104">You can cache hello token issued by hello back-end Azure service regardless of whether you are using client-managed or service-managed authentication.</span></span> <span data-ttu-id="bb495-105">Ez az oktatóanyag a szolgáltatás által kezelt hitelesítést használ.</span><span class="sxs-lookup"><span data-stu-id="bb495-105">This tutorial uses service-managed authentication.</span></span>
>
>

1. <span data-ttu-id="bb495-106">Nyissa meg a hello ToDoActivity.java fájlban, és adja hozzá a következő importálási utasításokat hello:</span><span class="sxs-lookup"><span data-stu-id="bb495-106">Open hello ToDoActivity.java file and add hello following import statements:</span></span>

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;
2. <span data-ttu-id="bb495-107">Adja hozzá a következő tagok toohello hello `ToDoActivity` osztály.</span><span class="sxs-lookup"><span data-stu-id="bb495-107">Add hello following members toohello `ToDoActivity` class.</span></span>

        public static final String SHAREDPREFFILE = "temp";    
        public static final String USERIDPREF = "uid";    
        public static final String TOKENPREF = "tkn";    
3. <span data-ttu-id="bb495-108">Hello ToDoActivity.java fájlban adja hozzá a következő hello definíciója hello `cacheUserToken` metódust.</span><span class="sxs-lookup"><span data-stu-id="bb495-108">In hello ToDoActivity.java file, add hello following definition for hello `cacheUserToken` method.</span></span>

        private void cacheUserToken(MobileServiceUser user)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            Editor editor = prefs.edit();
            editor.putString(USERIDPREF, user.getUserId());
            editor.putString(TOKENPREF, user.getAuthenticationToken());
            editor.commit();
        }    

    <span data-ttu-id="bb495-109">Ez a módszer hello felhasználói Azonosítót és a token titkos megjelölt beállításokat szabályozó fájlban tárolja.</span><span class="sxs-lookup"><span data-stu-id="bb495-109">This method stores hello user ID and token in a preference file that is marked private.</span></span> <span data-ttu-id="bb495-110">Ez kell hozzáférést toohello gyorsítótár védelmére is, hogy hello eszközön található egyéb alkalmazások nem rendelkeznek toohello jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="bb495-110">This should protect access toohello cache so that other apps on hello device do not have access toohello token.</span></span> <span data-ttu-id="bb495-111">hello beállítása az elkülönített hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="bb495-111">hello preference is sandboxed for hello app.</span></span> <span data-ttu-id="bb495-112">Azonban valaki kap hozzáférést toohello eszköz, esetén lehetséges, hogy hozzáférési toohello jogkivonat gyorsítótára más módon is kapnak.</span><span class="sxs-lookup"><span data-stu-id="bb495-112">However, if someone gains access toohello device, it is possible that they may gain access toohello token cache through other means.</span></span>

   > [!NOTE]
   > <span data-ttu-id="bb495-113">További védelmet biztosíthat az hello jogkivonat a titkosítás, ha a hozzáférés a jogkivonatokhoz tooyour adatok szigorúan bizalmas minősülnek, és valaki szerezhetnek toohello eszközt.</span><span class="sxs-lookup"><span data-stu-id="bb495-113">You can further protect hello token with encryption, if token access tooyour data is considered highly sensitive and someone may gain access toohello device.</span></span> <span data-ttu-id="bb495-114">Teljesen biztonságos megoldást ebben az oktatóanyagban hello terjed, és attól függ, a biztonsági követelményeinek.</span><span class="sxs-lookup"><span data-stu-id="bb495-114">A completely secure solution is beyond hello scope of this tutorial, however, and depends on your security requirements.</span></span>
   >
   >
4. <span data-ttu-id="bb495-115">Hello ToDoActivity.java fájlban adja hozzá a következő hello definíciója hello `loadUserTokenCache` metódust.</span><span class="sxs-lookup"><span data-stu-id="bb495-115">In hello ToDoActivity.java file, add hello following definition for hello `loadUserTokenCache` method.</span></span>

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
5. <span data-ttu-id="bb495-116">A hello *ToDoActivity.java* fájlt, cserélje le a hello `authenticate` hello metódus, amely a token gyorsítótárát használja a következő metódust.</span><span class="sxs-lookup"><span data-stu-id="bb495-116">In hello *ToDoActivity.java* file, replace hello `authenticate` method with hello following method, which uses a token cache.</span></span> <span data-ttu-id="bb495-117">Ha azt szeretné, hogy nem Google toouse hello bejelentkezési szolgáltató módosítása</span><span class="sxs-lookup"><span data-stu-id="bb495-117">Change hello login provider if you want toouse an account other than Google.</span></span>

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
6. <span data-ttu-id="bb495-118">Build hello alkalmazás és a teszt hitelesítés egy érvényes fiókot.</span><span class="sxs-lookup"><span data-stu-id="bb495-118">Build hello app and test authentication using a valid account.</span></span> <span data-ttu-id="bb495-119">Legalább két alkalommal futtassa.</span><span class="sxs-lookup"><span data-stu-id="bb495-119">Run it at least twice.</span></span> <span data-ttu-id="bb495-120">Során hello először futtatja az a kérdés toosign fogadni és hello token gyorsítótár létrehozása.</span><span class="sxs-lookup"><span data-stu-id="bb495-120">During hello first run, you should receive a prompt toosign in and create hello token cache.</span></span> <span data-ttu-id="bb495-121">Minden egyes futtatásához ezt követően próbálja meg tooload hello jogkivonatok gyorsítótárát a hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="bb495-121">After that, each run attempts tooload hello token cache for authentication.</span></span> <span data-ttu-id="bb495-122">A szükséges toosign nem lehetnek.</span><span class="sxs-lookup"><span data-stu-id="bb495-122">You should not be required toosign in.</span></span>
