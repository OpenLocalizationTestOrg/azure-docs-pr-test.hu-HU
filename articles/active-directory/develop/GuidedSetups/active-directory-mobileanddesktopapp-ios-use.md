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
## <a name="use-hello-microsoft-authentication-library-msal-tooget-a-token-for-hello-microsoft-graph-api"></a>Hello Microsoft hitelesítési könyvtár (MSAL) tooget jogkivonat használata hello Microsoft Graph API-val

Nyissa meg `ViewController.swift` , és cserélje le a hello kódot:

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
### <a name="more-information"></a>További információ
#### <a name="getting-a-user-token-interactively"></a>A felhasználói token első interaktív módon
Hívása hello `acquireToken` egy böngésző ablak arra kéri a módszer eredményezi a felhasználó toosign hello. Alkalmazások általában egy felhasználó toosign az interaktív hello tooaccess van szükségük, amikor első alkalommal védett erőforrás, vagy sikertelen, ha egy figyelő művelet tooacquire jogkivonat (pl. hello jelszó lejárt).

#### <a name="getting-a-user-token-silently"></a>A felhasználói token első beavatkozás nélkül
Hello `acquireTokenSilent` metódus kezeli a token kérése és -megújítás felhasználói beavatkozás nélkül. Után `acquireToken` hajtanak végre hello első alkalommal `acquireTokenSilent` hello általánosan használt módszer tooobtain használt a jogkivonatokat tooaccess későbbi hívások - erőforrások védelmét szolgálja hívások toorequest vagy megújítása jogkivonatok beavatkozás nélkül történik.

Végül `acquireTokenSilent` sikertelen lesz, – pl. hello felhasználó rendelkezik-e kijelentkezteti, vagy egy másik eszközön a jelszó megváltozott. Amikor MSAL azt észleli, hogy hello probléma oldható meg azzal, hogy egy interaktív műveletet, akkor következik be egy `MSALErrorCode.interactionRequired` kivétel. Az alkalmazás kezeli ezt a kivételt, két módon:

1.  Hívás elleni `acquireToken` azonnal, mely eredmények arra kéri a hello a felhasználó toosign. Ebben a mintában általában az online alkalmazások használják, ha nincs kapcsolat nélküli tartalom hello alkalmazásban hello felhasználó számára elérhető. az interaktív telepítő által létrehozott hello mintaalkalmazás használja ezt a mintát: tekintheti meg a művelet hello hello alkalmazás végrehajtása első alkalommal. Felhasználó nem használta a hello alkalmazást, mert `applicationContext.users().first` fogja tartalmazni null értékű, és egy ` MSALErrorCode.interactionRequired ` kivételt fog jelezni. hello hello mintában kód, majd leírók kivétel hello meghívásával `acquireToken` hello felhasználói toosign a kérdés eredményez.

2.  Alkalmazások is készíthet, amelyek egy interaktív bejelentkezés szükség, hogy hello felhasználó kiválaszthatja a hello megfelelő időben toosign, vagy megpróbálhatja hello alkalmazás jelzést toohello felhasználó `acquireTokenSilent` egy későbbi időpontban. Ez általában akkor használatos, amikor hello felhasználói alatt megszakad nélkül tudja használni hello alkalmazás vonatkozó egyéb funkciók – például érhető el kapcsolat nélküli tartalmat hello alkalmazásban. Ebben az esetben hello felhasználói dönthet, ha szeretnék toosign tooaccess hello védett erőforrás, vagy toorefresh hello elavult információkat vagy eldöntheti, hogy az alkalmazás tooretry `acquireTokenSilent` amikor hálózati átmenetileg nem érhető el állapota után állítják vissza.

<!--end-collapse-->

## <a name="call-hello-microsoft-graph-api-using-hello-token-you-just-obtained"></a>Csak kapott hello tokent hello Microsoft Graph API hívása

Adja hozzá az alábbi új módszer hello túl`ViewController.swift`. Ez a módszer akkor használható toomake egy `GET` hello Microsoft Graph API segítségével kérelmet egy *HTTP Authorization fejlécet*:

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
### <a name="making-a-rest-call-against-a-protected-api"></a>Így egy REST-hívást ellen védett API

A mintaalkalmazás az hello `getContentWithToken()` metódus használt toomake HTTP `GET` egy jogkivonat és majd visszatérési hello tartalom toohello hívó igénylő védett erőforrás kérelmet. Ez a módszer szerzett hello token helyez hello *HTTP Authorization fejlécet*. Ez a minta hello erőforrás hello Microsoft Graph API *me* végpont – hello felhasználói profil adatait jeleníti meg.
<!--end-collapse-->

## <a name="set-up-sign-out"></a>Kijelentkezési beállítása

Adja hozzá a következő metódus túl hello`ViewController.swift` toosign hello felhasználói ki:

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
### <a name="more-info-on-sign-out"></a>További információ a kijelentkezési

Hello `signoutButton` metódus hello felhasználó eltávolítása hello MSAL felhasználói gyorsítótár – ez gyakorlatilag jelzi a MSAL tooforget hello aktuális felhasználó, egy későbbi kérés tooacquire jogkivonat csak sikeres lesz, ha azt toobe interaktív.

Bár ez a példa hello alkalmazás támogatja az egy-egy felhasználóhoz, MSAL támogatja-e a forgatókönyvekben, ahol több fiók is bejelentkezhet a következő hello ugyanannyi időt vesz igénybe – példa, hogy e-mail alkalmazást ahol a felhasználó rendelkezik-e a több fiók.
<!--end-collapse-->

## <a name="register-hello-callback"></a>Hello visszahívási regisztrálása

Miután hello felhasználó hitelesíti magát, a hello böngésző hello felhasználói hátsó toohello alkalmazás irányítja át. Lépésekkel hello tooregister alatt a visszahívási:

1.  Nyissa meg `AppDelegate.swift` és MSAL importálása:

```swift
import MSAL
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
Adja hozzá a következő metódus tooyour hello <code>AppDelegate</code> toohandle visszahívások osztályban:
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

