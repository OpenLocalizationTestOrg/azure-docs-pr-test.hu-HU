
1. <span data-ttu-id="44918-101">Nyissa meg a projekt az Android Studio.</span><span class="sxs-lookup"><span data-stu-id="44918-101">Open the project in Android Studio.</span></span>

2. <span data-ttu-id="44918-102">A **Project Explorer** az Android Studióban nyissa meg a ToDoActivity.java fájlban, és adja hozzá a következő importálási utasításokat:</span><span class="sxs-lookup"><span data-stu-id="44918-102">In **Project Explorer** in Android Studio, open the ToDoActivity.java file and add the following import statements:</span></span>

        import java.util.concurrent.ExecutionException;
        import java.util.concurrent.atomic.AtomicBoolean;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceAuthenticationProvider;
        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceUser;

3. <span data-ttu-id="44918-103">Adja hozzá a következő metódust a **ToDoActivity** osztály:</span><span class="sxs-lookup"><span data-stu-id="44918-103">Add the following method to the **ToDoActivity** class:</span></span>

        // You can choose any unique number here to differentiate auth providers from each other. Note this is the same code at login() and onActivityResult().
        public static final int GOOGLE_LOGIN_REQUEST_CODE = 1;

        private void authenticate() {
            // Login using the Google provider.
            mClient.login("Google", "{url_scheme_of_your_app}", GOOGLE_LOGIN_REQUEST_CODE);
        }

        @Override
        protected void onActivityResult(int requestCode, int resultCode, Intent data) {
            // When request completes
            if (resultCode == RESULT_OK) {
                // Check the request code matches the one we send in the login request
                if (requestCode == GOOGLE_LOGIN_REQUEST_CODE) {
                    MobileServiceActivityResult result = mClient.onActivityResult(data);
                    if (result.isLoggedIn()) {
                        // login succeeded
                        createAndShowDialog(String.format("You are now logged in - %1$2s", mClient.getCurrentUser().getUserId()), "Success");
                        createTable();
                    } else {
                        // login failed, check the error message
                        String errorMessage = result.getErrorMessage();
                        createAndShowDialog(errorMessage, "Error");
                    }
                }
            }
        }

    <span data-ttu-id="44918-104">Ez a kód egy metódust a Google hitelesítést hoz létre.</span><span class="sxs-lookup"><span data-stu-id="44918-104">This code creates a method to handle the Google authentication process.</span></span> <span data-ttu-id="44918-105">Egy párbeszédpanel megjeleníti a hitelesített felhasználó Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="44918-105">A dialog displays the ID of the authenticated user.</span></span> <span data-ttu-id="44918-106">Csak a sikeres hitelesítés lépne.</span><span class="sxs-lookup"><span data-stu-id="44918-106">You can only proceed on a successful authentication.</span></span>

    > [!NOTE]
    > <span data-ttu-id="44918-107">Ha eltérő Google identitásszolgáltató használ, módosítsa az értéket, átadott a **bejelentkezési** metódus a következő értékek egyikére: _MicrosoftAccount_, _Facebook_, _Twitter_, vagy _windowsazureactivedirectory_.</span><span class="sxs-lookup"><span data-stu-id="44918-107">If you are using an identity provider other than Google, change the value passed to the **login** method to one of the following values: _MicrosoftAccount_, _Facebook_, _Twitter_, or _windowsazureactivedirectory_.</span></span>

4. <span data-ttu-id="44918-108">Az a **onCreate** módszer, példányosítja kód után adja hozzá a következő kódsort a `MobileServiceClient` objektum.</span><span class="sxs-lookup"><span data-stu-id="44918-108">In the **onCreate** method, add the following line of code after the code that instantiates the `MobileServiceClient` object.</span></span>

        authenticate();

    <span data-ttu-id="44918-109">Ez a hívás a hitelesítési folyamat elindul.</span><span class="sxs-lookup"><span data-stu-id="44918-109">This call starts the authentication process.</span></span>

5. <span data-ttu-id="44918-110">Helyezze át a többi kód után `authenticate();` a a **onCreate** egy új módszer **createTable** módszert:</span><span class="sxs-lookup"><span data-stu-id="44918-110">Move the remaining code after `authenticate();` in the **onCreate** method to a new **createTable** method:</span></span>

        private void createTable() {

            // Get the table instance to use.
            mToDoTable = mClient.getTable(ToDoItem.class);

            mTextNewToDo = (EditText) findViewById(R.id.textNewToDo);

            // Create an adapter to bind the items with the view.
            mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);

            // Load the items from Azure.
            refreshItemsFromTable();
        }

6. <span data-ttu-id="44918-111">Átirányítás works elvárt érdekében adja hozzá a következő szövegrészletet _RedirectUrlActivity_ való _AndroidManifest.xml_:</span><span class="sxs-lookup"><span data-stu-id="44918-111">To ensure redirection works as expected, add the following snippet of _RedirectUrlActivity_ to _AndroidManifest.xml_:</span></span>

        <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity">
            <intent-filter>
                <action android:name="android.intent.action.VIEW" />
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />
                <data android:scheme="{url_scheme_of_your_app}"
                    android:host="easyauth.callback"/>
            </intent-filter>
        </activity>

7. <span data-ttu-id="44918-112">Adja hozzá a redirectUriScheme _build.gradle_ az Android alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="44918-112">Add redirectUriScheme to _build.gradle_ of your Android application.</span></span>

        android {
            buildTypes {
                release {
                    // … …
                    manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
                }
                debug {
                    // … …
                    manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
                }
            }
        }

8. <span data-ttu-id="44918-113">Vegye fel a függőségeket a build.gradle com.android.support:customtabs:23.0.1:</span><span class="sxs-lookup"><span data-stu-id="44918-113">Add com.android.support:customtabs:23.0.1 to the dependencies in your build.gradle:</span></span>

      <span data-ttu-id="44918-114">függőségek {/ /... "com.android.support:customtabs:23.0.1" fordítási}</span><span class="sxs-lookup"><span data-stu-id="44918-114">dependencies {        // ...        compile 'com.android.support:customtabs:23.0.1'    }</span></span>

9. <span data-ttu-id="44918-115">Az a **futtassa** menüben kattintson **-alkalmazás futtatása** el az alkalmazást, és jelentkezzen be a választott identitásszolgáltató számára.</span><span class="sxs-lookup"><span data-stu-id="44918-115">From the **Run** menu, click **Run app** to start the app and sign in with your chosen identity provider.</span></span>

> [!WARNING]
> <span data-ttu-id="44918-116">Az URL-séma említett a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="44918-116">The URL Scheme mentioned is case-sensitive.</span></span>  <span data-ttu-id="44918-117">Győződjön meg arról, hogy minden előfordulását `{url_scheme_of_you_app}` nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="44918-117">Ensure that all occurrences of `{url_scheme_of_you_app}` use the same case.</span></span>

<span data-ttu-id="44918-118">Ha Ön sikeresen bejelentkezett, futtasson-e az alkalmazás nem jelenik meg hibaüzenet, és meg tudják lekérdezni a háttér-szolgáltatást, és hogy adatok.</span><span class="sxs-lookup"><span data-stu-id="44918-118">When you are successfully signed in, the app should run without errors, and you should be able to query the back-end service and make updates to data.</span></span>
