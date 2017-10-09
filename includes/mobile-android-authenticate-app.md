
1. <span data-ttu-id="4d6e2-101">Nyissa meg a hello projekt az Android Studio.</span><span class="sxs-lookup"><span data-stu-id="4d6e2-101">Open hello project in Android Studio.</span></span>

2. <span data-ttu-id="4d6e2-102">A **Project Explorer** az Android Studióban nyissa meg a hello ToDoActivity.java fájlt, és adja hozzá a következő importálási utasításokat hello:</span><span class="sxs-lookup"><span data-stu-id="4d6e2-102">In **Project Explorer** in Android Studio, open hello ToDoActivity.java file and add hello following import statements:</span></span>

        import java.util.concurrent.ExecutionException;
        import java.util.concurrent.atomic.AtomicBoolean;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceAuthenticationProvider;
        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceUser;

3. <span data-ttu-id="4d6e2-103">Adja hozzá a következő metódus toohello hello **ToDoActivity** osztály:</span><span class="sxs-lookup"><span data-stu-id="4d6e2-103">Add hello following method toohello **ToDoActivity** class:</span></span>

        // You can choose any unique number here toodifferentiate auth providers from each other. Note this is hello same code at login() and onActivityResult().
        public static final int GOOGLE_LOGIN_REQUEST_CODE = 1;

        private void authenticate() {
            // Login using hello Google provider.
            mClient.login("Google", "{url_scheme_of_your_app}", GOOGLE_LOGIN_REQUEST_CODE);
        }

        @Override
        protected void onActivityResult(int requestCode, int resultCode, Intent data) {
            // When request completes
            if (resultCode == RESULT_OK) {
                // Check hello request code matches hello one we send in hello login request
                if (requestCode == GOOGLE_LOGIN_REQUEST_CODE) {
                    MobileServiceActivityResult result = mClient.onActivityResult(data);
                    if (result.isLoggedIn()) {
                        // login succeeded
                        createAndShowDialog(String.format("You are now logged in - %1$2s", mClient.getCurrentUser().getUserId()), "Success");
                        createTable();
                    } else {
                        // login failed, check hello error message
                        String errorMessage = result.getErrorMessage();
                        createAndShowDialog(errorMessage, "Error");
                    }
                }
            }
        }

    <span data-ttu-id="4d6e2-104">Ez a kód létrehoz egy metódus toohandle hello Google hitelesítési folyamat.</span><span class="sxs-lookup"><span data-stu-id="4d6e2-104">This code creates a method toohandle hello Google authentication process.</span></span> <span data-ttu-id="4d6e2-105">A párbeszédpanel hello Azonosítójú hello hitelesített felhasználó jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="4d6e2-105">A dialog displays hello ID of hello authenticated user.</span></span> <span data-ttu-id="4d6e2-106">Csak a sikeres hitelesítés lépne.</span><span class="sxs-lookup"><span data-stu-id="4d6e2-106">You can only proceed on a successful authentication.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4d6e2-107">Ha eltérő Google identitásszolgáltató használ, módosítsa hello toohello átadott **bejelentkezési** metódus tooone a következő értékek hello: _MicrosoftAccount_, _Facebook_, _Twitter_, vagy _windowsazureactivedirectory_.</span><span class="sxs-lookup"><span data-stu-id="4d6e2-107">If you are using an identity provider other than Google, change hello value passed toohello **login** method tooone of hello following values: _MicrosoftAccount_, _Facebook_, _Twitter_, or _windowsazureactivedirectory_.</span></span>

4. <span data-ttu-id="4d6e2-108">A hello **onCreate** módszer, adja hozzá a következő kódsort hello kód, amely hello példányosítja után hello `MobileServiceClient` objektum.</span><span class="sxs-lookup"><span data-stu-id="4d6e2-108">In hello **onCreate** method, add hello following line of code after hello code that instantiates hello `MobileServiceClient` object.</span></span>

        authenticate();

    <span data-ttu-id="4d6e2-109">Ez a hívás hello hitelesítési folyamat elindul.</span><span class="sxs-lookup"><span data-stu-id="4d6e2-109">This call starts hello authentication process.</span></span>

5. <span data-ttu-id="4d6e2-110">Helyezze át a kód után fennmaradó hello `authenticate();` a hello **onCreate** metódus tooa új **createTable** módszert:</span><span class="sxs-lookup"><span data-stu-id="4d6e2-110">Move hello remaining code after `authenticate();` in hello **onCreate** method tooa new **createTable** method:</span></span>

        private void createTable() {

            // Get hello table instance toouse.
            mToDoTable = mClient.getTable(ToDoItem.class);

            mTextNewToDo = (EditText) findViewById(R.id.textNewToDo);

            // Create an adapter toobind hello items with hello view.
            mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);

            // Load hello items from Azure.
            refreshItemsFromTable();
        }

6. <span data-ttu-id="4d6e2-111">tooensure átirányítási működik megfelelően, adja hozzá a következő szövegrészletet hello _RedirectUrlActivity_ too_AndroidManifest.xml_:</span><span class="sxs-lookup"><span data-stu-id="4d6e2-111">tooensure redirection works as expected, add hello following snippet of _RedirectUrlActivity_ too_AndroidManifest.xml_:</span></span>

        <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity">
            <intent-filter>
                <action android:name="android.intent.action.VIEW" />
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />
                <data android:scheme="{url_scheme_of_your_app}"
                    android:host="easyauth.callback"/>
            </intent-filter>
        </activity>

7. <span data-ttu-id="4d6e2-112">Adja hozzá az Android alkalmazás redirectUriScheme too_build.gradle_.</span><span class="sxs-lookup"><span data-stu-id="4d6e2-112">Add redirectUriScheme too_build.gradle_ of your Android application.</span></span>

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

8. <span data-ttu-id="4d6e2-113">Adja hozzá a build.gradle com.android.support:customtabs:23.0.1 toohello függőségek:</span><span class="sxs-lookup"><span data-stu-id="4d6e2-113">Add com.android.support:customtabs:23.0.1 toohello dependencies in your build.gradle:</span></span>

      <span data-ttu-id="4d6e2-114">függőségek {/ /... "com.android.support:customtabs:23.0.1" fordítási}</span><span class="sxs-lookup"><span data-stu-id="4d6e2-114">dependencies {        // ...        compile 'com.android.support:customtabs:23.0.1'    }</span></span>

9. <span data-ttu-id="4d6e2-115">A hello **futtatása** menüben kattintson a **-alkalmazás futtatása** toostart hello alkalmazás, és jelentkezzen be a választott identitásszolgáltató.</span><span class="sxs-lookup"><span data-stu-id="4d6e2-115">From hello **Run** menu, click **Run app** toostart hello app and sign in with your chosen identity provider.</span></span>

> [!WARNING]
> <span data-ttu-id="4d6e2-116">hello említett URL-séma a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="4d6e2-116">hello URL Scheme mentioned is case-sensitive.</span></span>  <span data-ttu-id="4d6e2-117">Győződjön meg arról, hogy minden előfordulását `{url_scheme_of_you_app}` használata hello azonos eset.</span><span class="sxs-lookup"><span data-stu-id="4d6e2-117">Ensure that all occurrences of `{url_scheme_of_you_app}` use hello same case.</span></span>

<span data-ttu-id="4d6e2-118">Ha sikeresen jelentkezett be, hello app hibák nélkül futnak, és legyen képes tooquery hello háttér-szolgáltatás és frissítések toodata ellenőrizze.</span><span class="sxs-lookup"><span data-stu-id="4d6e2-118">When you are successfully signed in, hello app should run without errors, and you should be able tooquery hello back-end service and make updates toodata.</span></span>
