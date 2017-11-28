---
title: "Az Azure AD v2 iOS Getting Started - használata |} Microsoft Docs"
description: "Hogyan iOS (Swift) alkalmazások Azure Active Directory-v2 végpontja hozzáférési jogkivonatok igénylő API meghívása"
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
ms.openlocfilehash: 2ac1117a31a101705539a1f75520ce8de43809a2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
## <a name="use-the-microsoft-authentication-library-msal-to-get-a-token-for-the-microsoft-graph-api"></a><span data-ttu-id="d9522-103">A Microsoft hitelesítési könyvtár (MSAL) segítségével szolgáltatáshitelesítést egy token a Microsoft Graph API-hoz.</span><span class="sxs-lookup"><span data-stu-id="d9522-103">Use the Microsoft Authentication Library (MSAL) to get a token for the Microsoft Graph API</span></span>

<span data-ttu-id="d9522-104">Nyissa meg `ViewController.swift` , és cserélje le a kódot:</span><span class="sxs-lookup"><span data-stu-id="d9522-104">Open `ViewController.swift` and replace the code with:</span></span>

```swift
import UIKit
import MSAL

class ViewController: UIViewController, UITextFieldDelegate, URLSessionDelegate {
    
    let kClientID = "Your_Application_Id_Here"
    let kAuthority = "https://login.microsoftonline.com/common/v2.0"

    let kGraphURI = "https://graph.microsoft.com/v1.0/me/"
    let kScopes: [String] = ["https://graph.microsoft.com/user.read"]
    
    var accessToken = String()
    var applicationContext = MSALPublicClientApplication.init()

    @IBOutlet weak var loggingText: UITextView!
    @IBOutlet weak var signoutButton: UIButton!

     // This button will invoke the call to the Microsoft Graph API. It uses the
     // built in Swift libraries to create a connection.
    
    @IBAction func callGraphButton(_ sender: UIButton) {
        
        
        do {
            
            // We check to see if we have a current logged in user. If we don't, then we need to sign someone in.
            // We throw an interactionRequired so that we trigger the interactive signin.
            
            if  try self.applicationContext.users().isEmpty {
                throw NSError.init(domain: "MSALErrorDomain", code: MSALErrorCode.interactionRequired.rawValue, userInfo: nil)
            } else {
            
            // Acquire a token for an existing user silently
            
            try self.applicationContext.acquireTokenSilent(forScopes: self.kScopes, user: applicationContext.users().first) { (result, error) in
    
                    if error == nil {
                        self.accessToken = (result?.accessToken)!
                        self.loggingText.text = "Refreshing token silently)"
                        self.loggingText.text = "Refreshed Access token is \(self.accessToken)"
                        
                        self.signoutButton.isEnabled = true;
                        self.getContentWithToken()
    
                    } else {
                        self.loggingText.text = "Could not acquire token silently: \(error ?? "No error information" as! Error)"
    
                    }
                }
            }
        }  catch let error as NSError {
            
            // interactionRequired means we need to ask the user to sign-in. This usually happens
            // when the user's Refresh Token is expired or if the user has changed their password
            // among other possible reasons.
            
            if error.code == MSALErrorCode.interactionRequired.rawValue {
                
                self.applicationContext.acquireToken(forScopes: self.kScopes) { (result, error) in
                        if error == nil {
                            self.accessToken = (result?.accessToken)!
                            self.loggingText.text = "Access token is \(self.accessToken)"
                            self.signoutButton.isEnabled = true;
                            self.getContentWithToken()
                            
                        } else  {
                            self.loggingText.text = "Could not acquire token: \(error ?? "No error information" as! Error)"
                        }
                }
                
            }
            
        } catch {
            
            // This is the catch all error.
            
            self.loggingText.text = "Unable to acquire token. Got error: \(error)"
            
        }
    }

    override func viewDidLoad() {
        super.viewDidLoad()
        
        do {
             // Initialize a MSALPublicClientApplication with a given clientID and authority
            self.applicationContext = try MSALPublicClientApplication.init(clientId: kClientID, authority: kAuthority)
        } catch {
            self.loggingText.text = "Unable to create Application Context. Error: \(error)"
        }
    }


    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
    
    override func viewWillAppear(_ animated: Bool) {
        
        if self.accessToken.isEmpty {
            signoutButton.isEnabled = false; 
        }
    }
}
```

<!--start-collapse-->
### <a name="more-information"></a><span data-ttu-id="d9522-105">További információ</span><span class="sxs-lookup"><span data-stu-id="d9522-105">More Information</span></span>
#### <a name="getting-a-user-token-interactively"></a><span data-ttu-id="d9522-106">A felhasználói token első interaktív módon</span><span class="sxs-lookup"><span data-stu-id="d9522-106">Getting a user token interactively</span></span>
<span data-ttu-id="d9522-107">Hívja a `acquireToken` módszer eredményezi egy böngészőablakban, jelentkezzen be a felhasználótól.</span><span class="sxs-lookup"><span data-stu-id="d9522-107">Calling the `acquireToken` method results in a browser window prompting the user to sign in.</span></span> <span data-ttu-id="d9522-108">Az alkalmazásoknak általában egy felhasználó a védett erőforrások eléréséhez szükséges először interaktív bejelentkezéshez, vagy ha egy figyelő művelet szerezzen be egy tokent sikertelen (pl. a jelszó lejárt).</span><span class="sxs-lookup"><span data-stu-id="d9522-108">Applications usually require a user to sign in interactively the first time they need to access a protected resource, or when a silent operation to acquire a token fails (e.g. the user’s password expired).</span></span>

#### <a name="getting-a-user-token-silently"></a><span data-ttu-id="d9522-109">A felhasználói token első beavatkozás nélkül</span><span class="sxs-lookup"><span data-stu-id="d9522-109">Getting a user token silently</span></span>
<span data-ttu-id="d9522-110">A `acquireTokenSilent` metódus kezeli a token kérése és -megújítás felhasználói beavatkozás nélkül.</span><span class="sxs-lookup"><span data-stu-id="d9522-110">The `acquireTokenSilent` method handles token acquisitions and renewal without any user interaction.</span></span> <span data-ttu-id="d9522-111">Miután `acquireToken` végrehajtása az első alkalommal `acquireTokenSilent` az beszerzése a jogkivonatokat, mivel kérelmezése vagy megújítása jogkivonatok hívásainak beavatkozás nélkül történik a további hívások - védett erőforrások eléréséhez használt általánosan használt módszer.</span><span class="sxs-lookup"><span data-stu-id="d9522-111">After `acquireToken` is executed for the first time, `acquireTokenSilent` is the method commonly used to obtain tokens used to access protected resources for subsequent calls - as calls to request or renew tokens are made silently.</span></span>

<span data-ttu-id="d9522-112">Végül `acquireTokenSilent` sikertelen lesz, – például a felhasználó rendelkezik-e kijelentkezteti, vagy egy másik eszközön a jelszó megváltozott.</span><span class="sxs-lookup"><span data-stu-id="d9522-112">Eventually, `acquireTokenSilent` will fail – e.g. the user has signed out, or has changed their password on another device.</span></span> <span data-ttu-id="d9522-113">Amikor MSAL azt észleli, hogy a probléma megoldásához érdemes egy interaktív műveletet igénylő, akkor következik be egy `MSALErrorCode.interactionRequired` kivétel.</span><span class="sxs-lookup"><span data-stu-id="d9522-113">When MSAL detects that the issue can be resolved by requiring an interactive action, it fires an `MSALErrorCode.interactionRequired` exception.</span></span> <span data-ttu-id="d9522-114">Az alkalmazás kezeli ezt a kivételt, két módon:</span><span class="sxs-lookup"><span data-stu-id="d9522-114">Your application can handle this exception in two ways:</span></span>

1.  <span data-ttu-id="d9522-115">Hívás elleni `acquireToken` azonnal, ennek eredményeképpen a felhasználótól a bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="d9522-115">Make a call against `acquireToken` immediately, which results in prompting the user to sign in.</span></span> <span data-ttu-id="d9522-116">Ebben a mintában általában az online alkalmazások használják, ha nincs kapcsolat nélküli tartalom az alkalmazásban a felhasználó számára elérhető.</span><span class="sxs-lookup"><span data-stu-id="d9522-116">This pattern is usually used in online applications where there is no offline content in the application available for the user.</span></span> <span data-ttu-id="d9522-117">A mintaalkalmazás az interaktív telepítő által létrehozott használja ezt a mintát: tekintheti meg a kérelem végrehajtása az első művelet időben.</span><span class="sxs-lookup"><span data-stu-id="d9522-117">The sample application generated by this guided setup uses this pattern: you can see it in action the first time you execute the application.</span></span> <span data-ttu-id="d9522-118">Mivel a felhasználó nem használta az alkalmazás `applicationContext.users().first` fogja tartalmazni null értékű, és egy ` MSALErrorCode.interactionRequired ` kivételt fog jelezni.</span><span class="sxs-lookup"><span data-stu-id="d9522-118">Because no user ever used the application, `applicationContext.users().first` will contain a null value, and an ` MSALErrorCode.interactionRequired ` exception will be thrown.</span></span> <span data-ttu-id="d9522-119">A kód a minta majd kezeli a kivételt meghívásával `acquireToken` eredményezve jelentkezzen be a felhasználótól.</span><span class="sxs-lookup"><span data-stu-id="d9522-119">The code in the sample then handles the exception by calling `acquireToken` resulting in prompting the user to sign in.</span></span>

2.  <span data-ttu-id="d9522-120">Alkalmazások is végezhet egy visual arra utal, hogy a felhasználót, hogy egy interaktív bejelentkezés szükség, hogy a felhasználó kiválaszthatja a megfelelő időben való bejelentkezéshez, vagy az alkalmazás újra `acquireTokenSilent` egy későbbi időpontban.</span><span class="sxs-lookup"><span data-stu-id="d9522-120">Applications can also make a visual indication to the user that an interactive sign-in is required, so the user can select the right time to sign in, or the application can retry `acquireTokenSilent` at a later time.</span></span> <span data-ttu-id="d9522-121">Ez általában akkor használatos, amikor a felhasználó éppen megszakad nélkül tudja használni az alkalmazás egyéb funkciók – például nincs offline tartalma elérhető az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="d9522-121">This is usually used when the user can use other functionality of the application without being disrupted - for example, there is offline content available in the application.</span></span> <span data-ttu-id="d9522-122">Ebben az esetben a felhasználó eldöntheti, ha szeretne jelentkezzen be a védett erőforrások eléréséhez, vagy frissítse a elavult adatait, vagy eldöntheti, hogy az alkalmazást, majd ismételje meg `acquireTokenSilent` amikor hálózati átmenetileg nem érhető el állapota után állítják vissza.</span><span class="sxs-lookup"><span data-stu-id="d9522-122">In this case, the user can decide when they want to sign in to access the protected resource, or to refresh the outdated information, or your application can decide to retry `acquireTokenSilent` when network is restored after being unavailable temporarily.</span></span>

<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a><span data-ttu-id="d9522-123">A Microsoft Graph API használatával csak megszerzett jogkivonattal hívható</span><span class="sxs-lookup"><span data-stu-id="d9522-123">Call the Microsoft Graph API using the token you just obtained</span></span>

<span data-ttu-id="d9522-124">Adja hozzá az új metódust alatt `ViewController.swift`.</span><span class="sxs-lookup"><span data-stu-id="d9522-124">Add the new method below to `ViewController.swift`.</span></span> <span data-ttu-id="d9522-125">Ez a módszer biztosítja a `GET` kérelmet a Microsoft Graph API segítségével egy *HTTP Authorization fejlécet*:</span><span class="sxs-lookup"><span data-stu-id="d9522-125">This method is used to make a `GET` request against the Microsoft Graph API using an *HTTP Authorization header*:</span></span>

```swift
func getContentWithToken() {
    
    let sessionConfig = URLSessionConfiguration.default
    
    // Specify the Graph API endpoint
    let url = URL(string: kGraphURI)
    var request = URLRequest(url: url!)
    
    // Set the Authorization header for the request. We use Bearer tokens, so we specify Bearer + the token we got from the result
    request.setValue("Bearer \(self.accessToken)", forHTTPHeaderField: "Authorization")
    let urlSession = URLSession(configuration: sessionConfig, delegate: self, delegateQueue: OperationQueue.main)
    
    urlSession.dataTask(with: request) { data, response, error in
        let result = try? JSONSerialization.jsonObject(with: data!, options: [])
                    if result != nil {
                
                self.loggingText.text = result.debugDescription
            }
        }.resume()
}
```

<!--start-collapse-->
### <a name="making-a-rest-call-against-a-protected-api"></a><span data-ttu-id="d9522-126">Így egy REST-hívást ellen védett API</span><span class="sxs-lookup"><span data-stu-id="d9522-126">Making a REST call against a protected API</span></span>

<span data-ttu-id="d9522-127">Ebben a minta az alkalmazásban a `getContentWithToken()` módszer használható HTTP `GET` kérelem ellen védett erőforrás jogkivonat szükséges, és térjen vissza a tartalom a hívó.</span><span class="sxs-lookup"><span data-stu-id="d9522-127">In this sample application, the `getContentWithToken()` method is used to make an HTTP `GET` request against a protected resource that requires a token and then return the content to the caller.</span></span> <span data-ttu-id="d9522-128">Ez a módszer hozzáadja a megszerzett lexikális elem szerepel a *HTTP Authorization fejlécet*.</span><span class="sxs-lookup"><span data-stu-id="d9522-128">This method adds the acquired token in the *HTTP Authorization header*.</span></span> <span data-ttu-id="d9522-129">Ez a minta az erőforrás a Microsoft Graph API *me* végpont – amely a felhasználói profil adatait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="d9522-129">For this sample, the resource is the Microsoft Graph API *me* endpoint – which displays the user's profile information.</span></span>
<!--end-collapse-->

## <a name="set-up-sign-out"></a><span data-ttu-id="d9522-130">Kijelentkezési beállítása</span><span class="sxs-lookup"><span data-stu-id="d9522-130">Set up sign-out</span></span>

<span data-ttu-id="d9522-131">Adja hozzá a következő metódust `ViewController.swift` a felhasználó kijelentkezik:</span><span class="sxs-lookup"><span data-stu-id="d9522-131">Add the following method to `ViewController.swift` to sign out the user:</span></span>

```swift 
@IBAction func signoutButton(_ sender: UIButton) {

    do {
        
        // Removes all tokens from the cache for this application for the provided user
        // first parameter:   The user to remove from the cache
        
        try self.applicationContext.remove(self.applicationContext.users().first)
        self.signoutButton.isEnabled = false;
        
    } catch let error {
        self.loggingText.text = "Received error signing user out: \(error)"
    }
}
```
<!--start-collapse-->
### <a name="more-info-on-sign-out"></a><span data-ttu-id="d9522-132">További információ a kijelentkezési</span><span class="sxs-lookup"><span data-stu-id="d9522-132">More info on sign-out</span></span>

<span data-ttu-id="d9522-133">A `signoutButton` metódus eltávolítja a felhasználót a MSAL felhasználói gyorsítótárból – Ez hatékonyan jelzi az aktuális felhasználó törlésére, így a későbbi kérelmek egy jogkivonat csak akkor sikeres, ha történik, hogy interaktív MSAL.</span><span class="sxs-lookup"><span data-stu-id="d9522-133">The `signoutButton` method removes the user from the MSAL user cache – this will effectively tell MSAL to forget the current user so a future request to acquire a token will only succeed if it is made to be interactive.</span></span>

<span data-ttu-id="d9522-134">Bár ez a példa az alkalmazás támogatja az egy-egy felhasználóhoz, MSAL szituációkat ahol több fiók is bejelentkezhet a egyszerre – Ha a felhasználó rendelkezik-e a több fiók például e-mail alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d9522-134">Although the application in this sample supports a single user, MSAL supports scenarios where multiple accounts can be signed in at the same time – an example is an email application where a user has multiple accounts.</span></span>
<!--end-collapse-->

## <a name="register-the-callback"></a><span data-ttu-id="d9522-135">A visszahívási regisztrálása</span><span class="sxs-lookup"><span data-stu-id="d9522-135">Register the callback</span></span>

<span data-ttu-id="d9522-136">Miután a felhasználó hitelesíti magát, a böngésző átirányítja a felhasználót az alkalmazásnak.</span><span class="sxs-lookup"><span data-stu-id="d9522-136">Once the user authenticates, the browser redirects the user back to the application.</span></span> <span data-ttu-id="d9522-137">A visszahívási regisztrálni az alábbi lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="d9522-137">Follow the steps below to register this callback:</span></span>

1.  <span data-ttu-id="d9522-138">Nyissa meg `AppDelegate.swift` és MSAL importálása:</span><span class="sxs-lookup"><span data-stu-id="d9522-138">Open `AppDelegate.swift` and import MSAL:</span></span>

```swift
import MSAL
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
<span data-ttu-id="d9522-139">Adja hozzá a következő metódust a <code>AppDelegate</code> osztály visszahívások kezeléséhez:</span><span class="sxs-lookup"><span data-stu-id="d9522-139">Add the following method to your <code>AppDelegate</code> class to handle callbacks:</span></span>
</li>
</ol>

```swift
// @brief Handles inbound URLs. Checks if the URL matches the redirect URI for a pending AppAuth
// authorization request and if so, will look for the code in the response.

func application(_ application: UIApplication, open url: URL, sourceApplication: String?, annotation: Any) -> Bool {
    
    print("Received callback!")
    
    MSALPublicClientApplication.handleMSALResponse(url)
    
    return true
}
```

