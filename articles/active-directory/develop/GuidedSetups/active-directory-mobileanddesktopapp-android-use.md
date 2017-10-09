---
title: "aaaAzure AD v2 Android első lépések – használata |} Microsoft Docs"
description: "Android-alkalmazás hogyan szereznie egy hozzáférési jogkivonatot és hívható meg Microsoft Graph API-val vagy a hozzáférési jogkivonatok az Azure Active Directory v2 végpont igénylő API-k"
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
ms.openlocfilehash: 4480d89eb7638fe7d588c8cebd2b1e3c9d4c6e3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="use-hello-microsoft-authentication-library-msal-tooget-a-token-for-hello-microsoft-graph-api"></a><span data-ttu-id="603a2-103">Hello Microsoft hitelesítési könyvtár (MSAL) tooget jogkivonat használata hello Microsoft Graph API-val</span><span class="sxs-lookup"><span data-stu-id="603a2-103">Use hello Microsoft Authentication Library (MSAL) tooget a token for hello Microsoft Graph API</span></span>

1.  <span data-ttu-id="603a2-104">Nyissa meg: `MainActivity` (alatt `app`  >  `java`  >  `{domain}.{appname}`)</span><span class="sxs-lookup"><span data-stu-id="603a2-104">Open: `MainActivity` (under `app` > `java` > `{domain}.{appname}`)</span></span>
2.  <span data-ttu-id="603a2-105">Importálja a következő hello hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="603a2-105">Add hello following imports:</span></span>

```java
import android.app.Activity;
import android.content.Intent;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;
import com.android.volley.*;
import com.android.volley.toolbox.JsonObjectRequest;
import com.android.volley.toolbox.Volley;
import org.json.JSONObject;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import com.microsoft.identity.client.*;
```
<!-- Workaround for Docs conversion bug -->
<ol start="3">
<li>
<span data-ttu-id="603a2-106">Cserélje le a hello `MainActivity` osztályban az alábbi:</span><span class="sxs-lookup"><span data-stu-id="603a2-106">Replace hello `MainActivity` class with below:</span></span>
</li>
</ol>

```java
public class MainActivity extends AppCompatActivity {

    final static String CLIENT_ID = "[Enter hello application Id here]";
    final static String SCOPES [] = {"https://graph.microsoft.com/User.Read"};
    final static String MSGRAPH_URL = "https://graph.microsoft.com/v1.0/me";

    /* UI & Debugging Variables */
    private static final String TAG = MainActivity.class.getSimpleName();
    Button callGraphButton;
    Button signOutButton;

    /* Azure AD Variables */
    private PublicClientApplication sampleApp;
    private AuthenticationResult authResult;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        callGraphButton = (Button) findViewById(R.id.callGraph);
        signOutButton = (Button) findViewById(R.id.clearCache);

        callGraphButton.setOnClickListener(new View.OnClickListener() {
            public void onClick(View v) {
                onCallGraphClicked();
            }
        });

        signOutButton.setOnClickListener(new View.OnClickListener() {
            public void onClick(View v) {
                onSignOutClicked();
            }
        });

  /* Configure your sample app and save state for this activity */
        sampleApp = null;
        if (sampleApp == null) {
            sampleApp = new PublicClientApplication(
                    this.getApplicationContext(),
                    CLIENT_ID);
        }

  /* Attempt tooget a user and acquireTokenSilent
   * If this fails we do an interactive request
   */
        List<User> users = null;

        try {
            users = sampleApp.getUsers();

            if (users != null && users.size() == 1) {
          /* We have 1 user */

                sampleApp.acquireTokenSilentAsync(SCOPES, users.get(0), getAuthSilentCallback());
            } else {
          /* We have no user */

          /* Let's do an interactive request */
                sampleApp.acquireToken(this, SCOPES, getAuthInteractiveCallback());
            }
        } catch (MsalClientException e) {
            Log.d(TAG, "MSAL Exception Generated while getting users: " + e.toString());

        } catch (IndexOutOfBoundsException e) {
            Log.d(TAG, "User at this position does not exist: " + e.toString());
        }

    }

//
// App callbacks for MSAL
// ======================
// getActivity() - returns activity so we can acquireToken within a callback
// getAuthSilentCallback() - callback defined toohandle acquireTokenSilent() case
// getAuthInteractiveCallback() - callback defined toohandle acquireToken() case
//

    public Activity getActivity() {
        return this;
    }

    /* Callback method for acquireTokenSilent calls 
     * Looks if tokens are in hello cache (refreshes if necessary and if we don't forceRefresh)
     * else errors that we need toodo an interactive request.
     */
    private AuthenticationCallback getAuthSilentCallback() {
        return new AuthenticationCallback() {
            @Override
            public void onSuccess(AuthenticationResult authenticationResult) {
            /* Successfully got a token, call Graph now */
                Log.d(TAG, "Successfully authenticated");

            /* Store hello authResult */
                authResult = authenticationResult;

            /* call graph */
                callGraphAPI();

            /* update hello UI toopost call Graph state */
                updateSuccessUI();
            }

            @Override
            public void onError(MsalException exception) {
            /* Failed tooacquireToken */
                Log.d(TAG, "Authentication failed: " + exception.toString());

                if (exception instanceof MsalClientException) {
                /* Exception inside MSAL, more info inside MsalError.java */
                } else if (exception instanceof MsalServiceException) {
                /* Exception when communicating with hello STS, likely config issue */
                } else if (exception instanceof MsalUiRequiredException) {
                /* Tokens expired or no session, retry with interactive */
                }
            }

            @Override
            public void onCancel() {
            /* User canceled hello authentication */
                Log.d(TAG, "User cancelled login.");
            }
        };
    }


    /* Callback used for interactive request.  If succeeds we use hello access
         * token toocall hello Microsoft Graph. Does not check cache
         */
    private AuthenticationCallback getAuthInteractiveCallback() {
        return new AuthenticationCallback() {
            @Override
            public void onSuccess(AuthenticationResult authenticationResult) {
            /* Successfully got a token, call graph now */
                Log.d(TAG, "Successfully authenticated");
                Log.d(TAG, "ID Token: " + authenticationResult.getIdToken());

            /* Store hello auth result */
                authResult = authenticationResult;

            /* call Graph */
                callGraphAPI();

            /* update hello UI toopost call Graph state */
                updateSuccessUI();
            }

            @Override
            public void onError(MsalException exception) {
            /* Failed tooacquireToken */
                Log.d(TAG, "Authentication failed: " + exception.toString());

                if (exception instanceof MsalClientException) {
                /* Exception inside MSAL, more info inside MsalError.java */
                } else if (exception instanceof MsalServiceException) {
                /* Exception when communicating with hello STS, likely config issue */
                }
            }

            @Override
            public void onCancel() {
            /* User canceled hello authentication */
                Log.d(TAG, "User cancelled login.");
            }
        };
    }

    /* Set hello UI for successful token acquisition data */
    private void updateSuccessUI() {
        callGraphButton.setVisibility(View.INVISIBLE);
        signOutButton.setVisibility(View.VISIBLE);
        findViewById(R.id.welcome).setVisibility(View.VISIBLE);
        ((TextView) findViewById(R.id.welcome)).setText("Welcome, " +
                authResult.getUser().getName());
        findViewById(R.id.graphData).setVisibility(View.VISIBLE);
    }

    /* Use MSAL tooacquireToken for hello end-user
     * Callback will call Graph api w/ access token & update UI
     */
    private void onCallGraphClicked() {
        sampleApp.acquireToken(getActivity(), SCOPES, getAuthInteractiveCallback());
    }

    /* Handles hello redirect from hello System Browser */
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        sampleApp.handleInteractiveRequestRedirect(requestCode, resultCode, data);
    }

}
```
<!--start-collapse-->
### <a name="more-information"></a><span data-ttu-id="603a2-107">További információ</span><span class="sxs-lookup"><span data-stu-id="603a2-107">More Information</span></span>
#### <a name="getting-a-user-token-interactive"></a><span data-ttu-id="603a2-108">Egy felhasználói jogkivonat interaktív beolvasása</span><span class="sxs-lookup"><span data-stu-id="603a2-108">Getting a user token interactive</span></span>
<span data-ttu-id="603a2-109">Hívása hello `AcquireTokenAsync` módszer eredményezi ablak, amely felszólítja a felhasználó toosign hello.</span><span class="sxs-lookup"><span data-stu-id="603a2-109">Calling hello `AcquireTokenAsync` method results in a window prompting hello user toosign in.</span></span> <span data-ttu-id="603a2-110">Alkalmazások általában egy felhasználó toosign az interaktív hello tooaccess van szükségük, amikor első alkalommal védett erőforrás, vagy sikertelen, ha egy figyelő művelet tooacquire jogkivonat (pl. hello jelszó lejárt).</span><span class="sxs-lookup"><span data-stu-id="603a2-110">Applications usually require a user toosign in interactively hello first time they need tooaccess a protected resource, or when a silent operation tooacquire a token fails (e.g. hello user’s password expired).</span></span>

#### <a name="getting-a-user-token-silently"></a><span data-ttu-id="603a2-111">A felhasználói token első beavatkozás nélkül</span><span class="sxs-lookup"><span data-stu-id="603a2-111">Getting a user token silently</span></span>
<span data-ttu-id="603a2-112">`AcquireTokenSilentAsync`kezeli a token kérése és -megújítás felhasználói beavatkozás nélkül.</span><span class="sxs-lookup"><span data-stu-id="603a2-112">`AcquireTokenSilentAsync` handles token acquisitions and renewal without any user interaction.</span></span> <span data-ttu-id="603a2-113">Után `AcquireTokenAsync` hajtanak végre hello első alkalommal `AcquireTokenSilentAsync` hello általánosan használt módszer tooobtain jogkivonatok tooaccess későbbi hívások - erőforrások védelmét szolgálja hívások toorequest vagy újítsa meg a jogkivonatok beavatkozás nélkül történik.</span><span class="sxs-lookup"><span data-stu-id="603a2-113">After `AcquireTokenAsync` is executed for hello first time, `AcquireTokenSilentAsync` is hello method commonly used tooobtain tokens tooaccess protected resources for subsequent calls - as calls toorequest or renew tokens are made silently.</span></span>
<span data-ttu-id="603a2-114">Végül `AcquireTokenSilentAsync` sikertelen lesz, – pl. hello felhasználó rendelkezik-e kijelentkezteti, vagy egy másik eszközön a jelszó megváltozott.</span><span class="sxs-lookup"><span data-stu-id="603a2-114">Eventually, `AcquireTokenSilentAsync` will fail – e.g. hello user has signed out, or has changed their password on another device.</span></span> <span data-ttu-id="603a2-115">Amikor MSAL azt észleli, hogy hello probléma oldható meg azzal, hogy egy interaktív műveletet, akkor következik be egy `MsalUiRequiredException`.</span><span class="sxs-lookup"><span data-stu-id="603a2-115">When MSAL detects that hello issue can be resolved by requiring an interactive action, it fires an `MsalUiRequiredException`.</span></span> <span data-ttu-id="603a2-116">Az alkalmazás kezeli ezt a kivételt, két módon:</span><span class="sxs-lookup"><span data-stu-id="603a2-116">Your application can handle this exception in two ways:</span></span>

1.  <span data-ttu-id="603a2-117">Hívás elleni `AcquireTokenAsync` azonnal, ennek eredményeképpen hello toosign a felhasználótól.</span><span class="sxs-lookup"><span data-stu-id="603a2-117">Make a call against `AcquireTokenAsync` immediately, which results in prompting hello user toosign-in.</span></span> <span data-ttu-id="603a2-118">Ebben a mintában általában az online alkalmazások használják, ha nincs kapcsolat nélküli tartalom hello alkalmazásban hello felhasználó számára elérhető.</span><span class="sxs-lookup"><span data-stu-id="603a2-118">This pattern is usually used in online applications where there is no offline content in hello application available for hello user.</span></span> <span data-ttu-id="603a2-119">hello interaktív telepítő által létrehozott mintánkban Ez a minta: tekintheti meg a művelet hello hello minta végrehajtása először: felhasználó nem használta a hello alkalmazást, mert `PublicClientApp.Users.FirstOrDefault` fogja tartalmazni null értékű, és egy `MsalUiRequiredException` kivételt fog jelezni.</span><span class="sxs-lookup"><span data-stu-id="603a2-119">hello sample generated by this guided setup uses this pattern: you can see it in action hello first time you execute hello sample: because no user ever used hello application, `PublicClientApp.Users.FirstOrDefault` will contain a null value, and an `MsalUiRequiredException` exception will be thrown.</span></span> <span data-ttu-id="603a2-120">hello hello mintában kód, majd leírók kivétel hello meghívásával `AcquireTokenAsync` eredményezve hello toosign a felhasználótól.</span><span class="sxs-lookup"><span data-stu-id="603a2-120">hello code in hello sample then handles hello exception by calling `AcquireTokenAsync` resulting in prompting hello user toosign-in.</span></span>
2.  <span data-ttu-id="603a2-121">Alkalmazások is készíthet, amelyek egy interaktív bejelentkezés szükség, hogy hello felhasználó kiválaszthatja a hello megfelelő időben toosign, vagy megpróbálhatja hello alkalmazás jelzést toohello felhasználó `AcquireTokenSilentAsync` egy későbbi időpontban.</span><span class="sxs-lookup"><span data-stu-id="603a2-121">Applications can also make a visual indication toohello user that an interactive sign-in is required, so hello user can select hello right time toosign in, or hello application can retry `AcquireTokenSilentAsync` at a later time.</span></span> <span data-ttu-id="603a2-122">Ez általában akkor használatos, amikor hello felhasználói hello alkalmazás éppen megszakad nélkül képes tooaccess funkciók – például érhető el kapcsolat nélküli tartalmat hello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="603a2-122">This is commonly used when hello user is able tooaccess functionality of hello application without being disrupted - for example, there is offline content available in hello application.</span></span> <span data-ttu-id="603a2-123">Ebben az esetben hello felhasználói dönthet, ha szeretnék toosign tooaccess hello védett erőforrás, vagy toorefresh hello elavult információkat vagy eldöntheti, hogy az alkalmazás tooretry `AcquireTokenSilentAsync` amikor hálózati átmenetileg nem érhető el állapota után állítják vissza.</span><span class="sxs-lookup"><span data-stu-id="603a2-123">In this case, hello user can decide when they want toosign in tooaccess hello protected resource, or toorefresh hello outdated information, or your application can decide tooretry `AcquireTokenSilentAsync` when network is restored after being unavailable temporarily.</span></span>
<!--end-collapse-->

## <a name="call-hello-microsoft-graph-api-using-hello-token-you-just-obtained"></a><span data-ttu-id="603a2-124">Csak kapott hello tokent hello Microsoft Graph API hívása</span><span class="sxs-lookup"><span data-stu-id="603a2-124">Call hello Microsoft Graph API using hello token you just obtained</span></span>
1.  <span data-ttu-id="603a2-125">Adja hozzá a módszerek hello be a következő hello `MainActivity` osztály:</span><span class="sxs-lookup"><span data-stu-id="603a2-125">Add hello following methods into hello `MainActivity` class:</span></span>

```java
/* Use Volley toomake an HTTP request toohello /me endpoint from MS Graph using an access token */
private void callGraphAPI() {
    Log.d(TAG, "Starting volley request toograph");

    /* Make sure we have a token toosend toograph */
    if (authResult.getAccessToken() == null) {return;}

    RequestQueue queue = Volley.newRequestQueue(this);
    JSONObject parameters = new JSONObject();

    try {
        parameters.put("key", "value");
    } catch (Exception e) {
        Log.d(TAG, "Failed tooput parameters: " + e.toString());
    }
    JsonObjectRequest request = new JsonObjectRequest(Request.Method.GET, MSGRAPH_URL,
            parameters,new Response.Listener<JSONObject>() {
        @Override
        public void onResponse(JSONObject response) {
            /* Successfully called graph, process data and send tooUI */
            Log.d(TAG, "Response: " + response.toString());

            updateGraphUI(response);
        }
    }, new Response.ErrorListener() {
        @Override
        public void onErrorResponse(VolleyError error) {
            Log.d(TAG, "Error: " + error.toString());
        }
    }) {
        @Override
        public Map<String, String> getHeaders() throws AuthFailureError {
            Map<String, String> headers = new HashMap<>();
            headers.put("Authorization", "Bearer " + authResult.getAccessToken());
            return headers;
        }
    };

    Log.d(TAG, "Adding HTTP GET tooQueue, Request: " + request.toString());

    request.setRetryPolicy(new DefaultRetryPolicy(
            3000,
            DefaultRetryPolicy.DEFAULT_MAX_RETRIES,
            DefaultRetryPolicy.DEFAULT_BACKOFF_MULT));
    queue.add(request);
}

/* Sets hello Graph response */
private void updateGraphUI(JSONObject graphResponse) {
    TextView graphText = (TextView) findViewById(R.id.graphData);
    graphText.setText(graphResponse.toString());
}
```
<!--start-collapse-->
### <a name="more-information-about-making-a-rest-call-against-a-protected-api"></a><span data-ttu-id="603a2-126">További információt a REST-hívást ellen védett API</span><span class="sxs-lookup"><span data-stu-id="603a2-126">More information about making a REST call against a protected API</span></span>

<span data-ttu-id="603a2-127">A mintaalkalmazás az `callGraphAPI` hívások `getAccessToken` és majd lehetővé teszi a HTTP `GET` jogkivonat szükséges, és hello tartalom visszaadó erőforrás kérelmet.</span><span class="sxs-lookup"><span data-stu-id="603a2-127">In this sample application, `callGraphAPI` calls `getAccessToken` and then makes an HTTP `GET` request against a resource that requires a token and returns hello content.</span></span> <span data-ttu-id="603a2-128">Ez a módszer szerzett hello token helyez hello *HTTP Authorization fejlécet*.</span><span class="sxs-lookup"><span data-stu-id="603a2-128">This method adds hello acquired token in hello *HTTP Authorization header*.</span></span> <span data-ttu-id="603a2-129">Ez a minta hello erőforrás hello Microsoft Graph API *me* végpont – hello felhasználói profil adatait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="603a2-129">For this sample, hello resource is hello Microsoft Graph API *me* endpoint – which displays hello user's profile information.</span></span>
<!--end-collapse-->

## <a name="setup-sign-out"></a><span data-ttu-id="603a2-130">Kijelentkezési beállítása</span><span class="sxs-lookup"><span data-stu-id="603a2-130">Setup Sign-out</span></span>

1.  <span data-ttu-id="603a2-131">Adja hozzá a módszerek hello be a következő hello `MainActivity` osztály:</span><span class="sxs-lookup"><span data-stu-id="603a2-131">Add hello following methods into hello `MainActivity` class:</span></span>

```java
/* Clears a user's tokens from hello cache.
 * Logically similar too"sign out" but only signs out of this app.
 */
private void onSignOutClicked() {

    /* Attempt tooget a user and remove their cookies from cache */
    List<User> users = null;

    try {
        users = sampleApp.getUsers();

        if (users == null) {
            /* We have no users */

        } else if (users.size() == 1) {
            /* We have 1 user */
            /* Remove from token cache */
            sampleApp.remove(users.get(0));
            updateSignedOutUI();

        }
        else {
            /* We have multiple users */
            for (int i = 0; i < users.size(); i++) {
                sampleApp.remove(users.get(i));
            }
        }

        Toast.makeText(getBaseContext(), "Signed Out!", Toast.LENGTH_SHORT)
                .show();

    } catch (MsalClientException e) {
        Log.d(TAG, "MSAL Exception Generated while getting users: " + e.toString());

    } catch (IndexOutOfBoundsException e) {
        Log.d(TAG, "User at this position does not exist: " + e.toString());
    }
}

/* Set hello UI for signed-out user */
private void updateSignedOutUI() {
    callGraphButton.setVisibility(View.VISIBLE);
    signOutButton.setVisibility(View.INVISIBLE);
    findViewById(R.id.welcome).setVisibility(View.INVISIBLE);
    findViewById(R.id.graphData).setVisibility(View.INVISIBLE);
    ((TextView) findViewById(R.id.graphData)).setText("No Data");
}
```
<!--start-collapse-->
### <a name="more-information"></a><span data-ttu-id="603a2-132">További információ</span><span class="sxs-lookup"><span data-stu-id="603a2-132">More information</span></span>

<span data-ttu-id="603a2-133">`onSignOutClicked`eltávolítja a fenti hello MSAL felhasználói gyorsítótárból – felhasználói Ez hatékonyan jelzi a MSAL tooforget hello aktuális felhasználó, egy későbbi kérés tooacquire jogkivonat csak sikeres lesz, ha azt toobe interaktív.</span><span class="sxs-lookup"><span data-stu-id="603a2-133">`onSignOutClicked` above removes hello user from MSAL user cache – this will effectively tell MSAL tooforget hello current user so a future request tooacquire a token will only succeed if it is made toobe interactive.</span></span>
<span data-ttu-id="603a2-134">Bár ez a példa hello alkalmazás támogatja az egy-egy felhasználóhoz, MSAL támogatja-e a forgatókönyvekben, ahol több fiók lehet bejelentkezve: hello ugyanannyi időt vesz igénybe – példa, hogy e-mail alkalmazást ahol a felhasználó rendelkezik-e a több fiók.</span><span class="sxs-lookup"><span data-stu-id="603a2-134">Although hello application in this sample supports a single user, MSAL supports scenarios where multiple accounts can be signed-in at hello same time – an example is an email application where a user has multiple accounts.</span></span>
<!--end-collapse-->
