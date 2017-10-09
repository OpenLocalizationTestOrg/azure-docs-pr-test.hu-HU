
1. Nyissa meg a hello projekt az Android Studio.

2. A **Project Explorer** az Android Studióban nyissa meg a hello ToDoActivity.java fájlt, és adja hozzá a következő importálási utasításokat hello:

        import java.util.concurrent.ExecutionException;
        import java.util.concurrent.atomic.AtomicBoolean;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceAuthenticationProvider;
        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceUser;

3. Adja hozzá a következő metódus toohello hello **ToDoActivity** osztály:

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

    Ez a kód létrehoz egy metódus toohandle hello Google hitelesítési folyamat. A párbeszédpanel hello Azonosítójú hello hitelesített felhasználó jeleníti meg. Csak a sikeres hitelesítés lépne.

    > [!NOTE]
    > Ha eltérő Google identitásszolgáltató használ, módosítsa hello toohello átadott **bejelentkezési** metódus tooone a következő értékek hello: _MicrosoftAccount_, _Facebook_, _Twitter_, vagy _windowsazureactivedirectory_.

4. A hello **onCreate** módszer, adja hozzá a következő kódsort hello kód, amely hello példányosítja után hello `MobileServiceClient` objektum.

        authenticate();

    Ez a hívás hello hitelesítési folyamat elindul.

5. Helyezze át a kód után fennmaradó hello `authenticate();` a hello **onCreate** metódus tooa új **createTable** módszert:

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

6. tooensure átirányítási működik megfelelően, adja hozzá a következő szövegrészletet hello _RedirectUrlActivity_ too_AndroidManifest.xml_:

        <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity">
            <intent-filter>
                <action android:name="android.intent.action.VIEW" />
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />
                <data android:scheme="{url_scheme_of_your_app}"
                    android:host="easyauth.callback"/>
            </intent-filter>
        </activity>

7. Adja hozzá az Android alkalmazás redirectUriScheme too_build.gradle_.

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

8. Adja hozzá a build.gradle com.android.support:customtabs:23.0.1 toohello függőségek:

      függőségek {/ /... "com.android.support:customtabs:23.0.1" fordítási}

9. A hello **futtatása** menüben kattintson a **-alkalmazás futtatása** toostart hello alkalmazás, és jelentkezzen be a választott identitásszolgáltató.

> [!WARNING]
> hello említett URL-séma a kis-és nagybetűket.  Győződjön meg arról, hogy minden előfordulását `{url_scheme_of_you_app}` használata hello azonos eset.

Ha sikeresen jelentkezett be, hello app hibák nélkül futnak, és legyen képes tooquery hello háttér-szolgáltatás és frissítések toodata ellenőrizze.
