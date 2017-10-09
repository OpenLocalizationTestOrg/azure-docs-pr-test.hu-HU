---
title: "aaaAzure AD v2 iOS Getting Started - használata |} Microsoft Docs"
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
ms.openlocfilehash: 22e67850e2e0b14b6d68815d8f23e18ce2e878ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="use-hello-microsoft-authentication-library-msal-tooget-a-token-for-hello-microsoft-graph-api"></a><span data-ttu-id="07156-103">Hello Microsoft hitelesítési könyvtár (MSAL) tooget jogkivonat használata hello Microsoft Graph API-val</span><span class="sxs-lookup"><span data-stu-id="07156-103">Use hello Microsoft Authentication Library (MSAL) tooget a token for hello Microsoft Graph API</span></span>

<span data-ttu-id="07156-104">Nyissa meg `ViewController.swift` , és cserélje le a hello kódot:</span><span class="sxs-lookup"><span data-stu-id="07156-104">Open `ViewController.swift` and replace hello code with:</span></span>

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

     // This button will invoke hello call toohello Microsoft Graph API. It uses the
     // built in Swift libraries toocreate a connection.
    
    @IBAction func callGraphButton(_ sender: UIButton) {
        
        
        do {
            
            // We check toosee if we have a current logged in user. If we don't, then we need toosign someone in.
            // We throw an interactionRequired so that we trigger hello interactive signin.
            
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
            
            // interactionRequired means we need tooask hello user toosign-in. This usually happens
            // when hello user's Refresh Token is expired or if hello user has changed their password
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
            
            // This is hello catch all error.
            
            self.loggingText.text = "Unable tooacquire token. Got error: \(error)"
            
        }
    }

    override func viewDidLoad() {
        super.viewDidLoad()
        
        do {
             // Initialize a MSALPublicClientApplication with a given clientID and authority
            self.applicationContext = try MSALPublicClientApplication.init(clientId: kClientID, authority: kAuthority)
        } catch {
            self.loggingText.text = "Unable toocreate Application Context. Error: \(error)"
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
### <a name="more-information"></a><span data-ttu-id="07156-105">További információ</span><span class="sxs-lookup"><span data-stu-id="07156-105">More Information</span></span>
#### <a name="getting-a-user-token-interactively"></a><span data-ttu-id="07156-106">A felhasználói token első interaktív módon</span><span class="sxs-lookup"><span data-stu-id="07156-106">Getting a user token interactively</span></span>
<span data-ttu-id="07156-107">Hívása hello `acquireToken` egy böngésző ablak arra kéri a módszer eredményezi a felhasználó toosign hello.</span><span class="sxs-lookup"><span data-stu-id="07156-107">Calling hello `acquireToken` method results in a browser window prompting hello user toosign in.</span></span> <span data-ttu-id="07156-108">Alkalmazások általában egy felhasználó toosign az interaktív hello tooaccess van szükségük, amikor első alkalommal védett erőforrás, vagy sikertelen, ha egy figyelő művelet tooacquire jogkivonat (pl. hello jelszó lejárt).</span><span class="sxs-lookup"><span data-stu-id="07156-108">Applications usually require a user toosign in interactively hello first time they need tooaccess a protected resource, or when a silent operation tooacquire a token fails (e.g. hello user’s password expired).</span></span>

#### <a name="getting-a-user-token-silently"></a><span data-ttu-id="07156-109">A felhasználói token első beavatkozás nélkül</span><span class="sxs-lookup"><span data-stu-id="07156-109">Getting a user token silently</span></span>
<span data-ttu-id="07156-110">Hello `acquireTokenSilent` metódus kezeli a token kérése és -megújítás felhasználói beavatkozás nélkül.</span><span class="sxs-lookup"><span data-stu-id="07156-110">hello `acquireTokenSilent` method handles token acquisitions and renewal without any user interaction.</span></span> <span data-ttu-id="07156-111">Után `acquireToken` hajtanak végre hello első alkalommal `acquireTokenSilent` hello általánosan használt módszer tooobtain használt a jogkivonatokat tooaccess későbbi hívások - erőforrások védelmét szolgálja hívások toorequest vagy megújítása jogkivonatok beavatkozás nélkül történik.</span><span class="sxs-lookup"><span data-stu-id="07156-111">After `acquireToken` is executed for hello first time, `acquireTokenSilent` is hello method commonly used tooobtain tokens used tooaccess protected resources for subsequent calls - as calls toorequest or renew tokens are made silently.</span></span>

<span data-ttu-id="07156-112">Végül `acquireTokenSilent` sikertelen lesz, – pl. hello felhasználó rendelkezik-e kijelentkezteti, vagy egy másik eszközön a jelszó megváltozott.</span><span class="sxs-lookup"><span data-stu-id="07156-112">Eventually, `acquireTokenSilent` will fail – e.g. hello user has signed out, or has changed their password on another device.</span></span> <span data-ttu-id="07156-113">Amikor MSAL azt észleli, hogy hello probléma oldható meg azzal, hogy egy interaktív műveletet, akkor következik be egy `MSALErrorCode.interactionRequired` kivétel.</span><span class="sxs-lookup"><span data-stu-id="07156-113">When MSAL detects that hello issue can be resolved by requiring an interactive action, it fires an `MSALErrorCode.interactionRequired` exception.</span></span> <span data-ttu-id="07156-114">Az alkalmazás kezeli ezt a kivételt, két módon:</span><span class="sxs-lookup"><span data-stu-id="07156-114">Your application can handle this exception in two ways:</span></span>

1.  <span data-ttu-id="07156-115">Hívás elleni `acquireToken` azonnal, mely eredmények arra kéri a hello a felhasználó toosign.</span><span class="sxs-lookup"><span data-stu-id="07156-115">Make a call against `acquireToken` immediately, which results in prompting hello user toosign in.</span></span> <span data-ttu-id="07156-116">Ebben a mintában általában az online alkalmazások használják, ha nincs kapcsolat nélküli tartalom hello alkalmazásban hello felhasználó számára elérhető.</span><span class="sxs-lookup"><span data-stu-id="07156-116">This pattern is usually used in online applications where there is no offline content in hello application available for hello user.</span></span> <span data-ttu-id="07156-117">az interaktív telepítő által létrehozott hello mintaalkalmazás használja ezt a mintát: tekintheti meg a művelet hello hello alkalmazás végrehajtása első alkalommal.</span><span class="sxs-lookup"><span data-stu-id="07156-117">hello sample application generated by this guided setup uses this pattern: you can see it in action hello first time you execute hello application.</span></span> <span data-ttu-id="07156-118">Felhasználó nem használta a hello alkalmazást, mert `applicationContext.users().first` fogja tartalmazni null értékű, és egy ` MSALErrorCode.interactionRequired ` kivételt fog jelezni.</span><span class="sxs-lookup"><span data-stu-id="07156-118">Because no user ever used hello application, `applicationContext.users().first` will contain a null value, and an ` MSALErrorCode.interactionRequired ` exception will be thrown.</span></span> <span data-ttu-id="07156-119">hello hello mintában kód, majd leírók kivétel hello meghívásával `acquireToken` hello felhasználói toosign a kérdés eredményez.</span><span class="sxs-lookup"><span data-stu-id="07156-119">hello code in hello sample then handles hello exception by calling `acquireToken` resulting in prompting hello user toosign in.</span></span>

2.  <span data-ttu-id="07156-120">Alkalmazások is készíthet, amelyek egy interaktív bejelentkezés szükség, hogy hello felhasználó kiválaszthatja a hello megfelelő időben toosign, vagy megpróbálhatja hello alkalmazás jelzést toohello felhasználó `acquireTokenSilent` egy későbbi időpontban.</span><span class="sxs-lookup"><span data-stu-id="07156-120">Applications can also make a visual indication toohello user that an interactive sign-in is required, so hello user can select hello right time toosign in, or hello application can retry `acquireTokenSilent` at a later time.</span></span> <span data-ttu-id="07156-121">Ez általában akkor használatos, amikor hello felhasználói alatt megszakad nélkül tudja használni hello alkalmazás vonatkozó egyéb funkciók – például érhető el kapcsolat nélküli tartalmat hello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="07156-121">This is usually used when hello user can use other functionality of hello application without being disrupted - for example, there is offline content available in hello application.</span></span> <span data-ttu-id="07156-122">Ebben az esetben hello felhasználói dönthet, ha szeretnék toosign tooaccess hello védett erőforrás, vagy toorefresh hello elavult információkat vagy eldöntheti, hogy az alkalmazás tooretry `acquireTokenSilent` amikor hálózati átmenetileg nem érhető el állapota után állítják vissza.</span><span class="sxs-lookup"><span data-stu-id="07156-122">In this case, hello user can decide when they want toosign in tooaccess hello protected resource, or toorefresh hello outdated information, or your application can decide tooretry `acquireTokenSilent` when network is restored after being unavailable temporarily.</span></span>

<!--end-collapse-->

## <a name="call-hello-microsoft-graph-api-using-hello-token-you-just-obtained"></a><span data-ttu-id="07156-123">Csak kapott hello tokent hello Microsoft Graph API hívása</span><span class="sxs-lookup"><span data-stu-id="07156-123">Call hello Microsoft Graph API using hello token you just obtained</span></span>

<span data-ttu-id="07156-124">Adja hozzá az alábbi új módszer hello túl`ViewController.swift`.</span><span class="sxs-lookup"><span data-stu-id="07156-124">Add hello new method below too`ViewController.swift`.</span></span> <span data-ttu-id="07156-125">Ez a módszer akkor használható toomake egy `GET` hello Microsoft Graph API segítségével kérelmet egy *HTTP Authorization fejlécet*:</span><span class="sxs-lookup"><span data-stu-id="07156-125">This method is used toomake a `GET` request against hello Microsoft Graph API using an *HTTP Authorization header*:</span></span>

```swift
func getContentWithToken() {
    
    let sessionConfig = URLSessionConfiguration.default
    
    // Specify hello Graph API endpoint
    let url = URL(string: kGraphURI)
    var request = URLRequest(url: url!)
    
    // Set hello Authorization header for hello request. We use Bearer tokens, so we specify Bearer + hello token we got from hello result
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
### <a name="making-a-rest-call-against-a-protected-api"></a><span data-ttu-id="07156-126">Így egy REST-hívást ellen védett API</span><span class="sxs-lookup"><span data-stu-id="07156-126">Making a REST call against a protected API</span></span>

<span data-ttu-id="07156-127">A mintaalkalmazás az hello `getContentWithToken()` metódus használt toomake HTTP `GET` egy jogkivonat és majd visszatérési hello tartalom toohello hívó igénylő védett erőforrás kérelmet.</span><span class="sxs-lookup"><span data-stu-id="07156-127">In this sample application, hello `getContentWithToken()` method is used toomake an HTTP `GET` request against a protected resource that requires a token and then return hello content toohello caller.</span></span> <span data-ttu-id="07156-128">Ez a módszer szerzett hello token helyez hello *HTTP Authorization fejlécet*.</span><span class="sxs-lookup"><span data-stu-id="07156-128">This method adds hello acquired token in hello *HTTP Authorization header*.</span></span> <span data-ttu-id="07156-129">Ez a minta hello erőforrás hello Microsoft Graph API *me* végpont – hello felhasználói profil adatait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="07156-129">For this sample, hello resource is hello Microsoft Graph API *me* endpoint – which displays hello user's profile information.</span></span>
<!--end-collapse-->

## <a name="set-up-sign-out"></a><span data-ttu-id="07156-130">Kijelentkezési beállítása</span><span class="sxs-lookup"><span data-stu-id="07156-130">Set up sign-out</span></span>

<span data-ttu-id="07156-131">Adja hozzá a következő metódus túl hello`ViewController.swift` toosign hello felhasználói ki:</span><span class="sxs-lookup"><span data-stu-id="07156-131">Add hello following method too`ViewController.swift` toosign out hello user:</span></span>

```swift 
@IBAction func signoutButton(_ sender: UIButton) {

    do {
        
        // Removes all tokens from hello cache for this application for hello provided user
        // first parameter:   hello user tooremove from hello cache
        
        try self.applicationContext.remove(self.applicationContext.users().first)
        self.signoutButton.isEnabled = false;
        
    } catch let error {
        self.loggingText.text = "Received error signing user out: \(error)"
    }
}
```
<!--start-collapse-->
### <a name="more-info-on-sign-out"></a><span data-ttu-id="07156-132">További információ a kijelentkezési</span><span class="sxs-lookup"><span data-stu-id="07156-132">More info on sign-out</span></span>

<span data-ttu-id="07156-133">Hello `signoutButton` metódus hello felhasználó eltávolítása hello MSAL felhasználói gyorsítótár – ez gyakorlatilag jelzi a MSAL tooforget hello aktuális felhasználó, egy későbbi kérés tooacquire jogkivonat csak sikeres lesz, ha azt toobe interaktív.</span><span class="sxs-lookup"><span data-stu-id="07156-133">hello `signoutButton` method removes hello user from hello MSAL user cache – this will effectively tell MSAL tooforget hello current user so a future request tooacquire a token will only succeed if it is made toobe interactive.</span></span>

<span data-ttu-id="07156-134">Bár ez a példa hello alkalmazás támogatja az egy-egy felhasználóhoz, MSAL támogatja-e a forgatókönyvekben, ahol több fiók is bejelentkezhet a következő hello ugyanannyi időt vesz igénybe – példa, hogy e-mail alkalmazást ahol a felhasználó rendelkezik-e a több fiók.</span><span class="sxs-lookup"><span data-stu-id="07156-134">Although hello application in this sample supports a single user, MSAL supports scenarios where multiple accounts can be signed in at hello same time – an example is an email application where a user has multiple accounts.</span></span>
<!--end-collapse-->

## <a name="register-hello-callback"></a><span data-ttu-id="07156-135">Hello visszahívási regisztrálása</span><span class="sxs-lookup"><span data-stu-id="07156-135">Register hello callback</span></span>

<span data-ttu-id="07156-136">Miután hello felhasználó hitelesíti magát, a hello böngésző hello felhasználói hátsó toohello alkalmazás irányítja át.</span><span class="sxs-lookup"><span data-stu-id="07156-136">Once hello user authenticates, hello browser redirects hello user back toohello application.</span></span> <span data-ttu-id="07156-137">Lépésekkel hello tooregister alatt a visszahívási:</span><span class="sxs-lookup"><span data-stu-id="07156-137">Follow hello steps below tooregister this callback:</span></span>

1.  <span data-ttu-id="07156-138">Nyissa meg `AppDelegate.swift` és MSAL importálása:</span><span class="sxs-lookup"><span data-stu-id="07156-138">Open `AppDelegate.swift` and import MSAL:</span></span>

```swift
import MSAL
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
<span data-ttu-id="07156-139">Adja hozzá a következő metódus tooyour hello <code>AppDelegate</code> toohandle visszahívások osztályban:</span><span class="sxs-lookup"><span data-stu-id="07156-139">Add hello following method tooyour <code>AppDelegate</code> class toohandle callbacks:</span></span>
</li>
</ol>

```swift
// @brief Handles inbound URLs. Checks if hello URL matches hello redirect URI for a pending AppAuth
// authorization request and if so, will look for hello code in hello response.

func application(_ application: UIApplication, open url: URL, sourceApplication: String?, annotation: Any) -> Bool {
    
    print("Received callback!")
    
    MSALPublicClientApplication.handleMSALResponse(url)
    
    return true
}
```

