---
title: "aaaGet használatába hello Azure CDN könyvtár a .NET-hez |} Microsoft Docs"
description: "Megtudhatja, hogyan toowrite .NET alkalmazások toomanage Azure CDN Visual Studio használatával."
services: cdn
documentationcenter: .net
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 63cf4101-92e7-49dd-a155-a90e54a792ca
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 9753e48c7469072cef6b2ac728e18c78121c97f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-cdn-development"></a>Ismerkedés az Azure CDN-fejlesztéssel
> [!div class="op_single_selector"]
> * [Node.js](cdn-app-dev-node.md)
> * [.NET](cdn-app-dev-net.md)
> 
> 

Használhatja a hello [Azure CDN .NET-keretrendszerhez készült](https://msdn.microsoft.com/library/mt657769.aspx) tooautomate létrehozását és a CDN-profil és a végpontok.  Ez az oktatóanyag végigvezeti egy egyszerű .NET Konzolalkalmazás azt mutatja be, hogy több hello elérhető műveletek hello létrehozását.  Ez az oktatóanyag van nem tervezett toodescribe minden szempontját hello Azure CDN könyvtár a .NET-hez részletesen.

Ez az oktatóanyag szüksége van a Visual Studio 2015 toocomplete.  [A Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) szabadon letölthető.

> [!TIP]
> Hello [az oktatóanyagot befejezett projekt](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c) letölthető az MSDN Webhelyén.
> 
> 

[!INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-nuget-packages"></a>A projekt létrehozása, és adja hozzá a Nuget-csomagok
Most, hogy előre létrehozott erőforráscsoport a CDN-profilok és az Azure AD alkalmazás engedély toomanage CDN-profil és a csoporton belüli végpontok, nem lehet elindítani, az alkalmazás létrehozása.

A Visual Studio 2015-öt, belül kattintson **fájl**, **új**, **projekt...**  tooopen hello új projekt párbeszédpanel.  Bontsa ki a **Visual C#**, majd jelölje be **Windows** hello bal hello területen.  Kattintson a **Konzolalkalmazás** hello középső ablaktáblán.  Nevezze el a projektet, majd kattintson az **OK**.  

![Új projekt](./media/cdn-app-dev-net/cdn-new-project.png)

A projekt Nuget-csomagok szereplő Azure könyvtárak toouse lesz.  E toohello projekt adjuk hozzá.

1. Kattintson a hello **eszközök** menü **Nuget-Csomagkezelő**, majd **Csomagkezelő konzol**.
   
    ![Nuget-csomagok kezelése](./media/cdn-app-dev-net/cdn-manage-nuget.png)
2. A Csomagkezelő konzol hello, hajtsa végre a következő parancs tooinstall hello hello **Active Directory Authentication Library (ADAL)**:
   
    `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory`
3. Hajtsa végre a következő tooinstall hello hello **Azure CDN könyvtár**:
   
    `Install-Package Microsoft.Azure.Management.Cdn`

## <a name="directives-constants-main-method-and-helper-methods"></a>Irányelvek, állandók, fő metódus és segédmódszereket
Folytassuk hello alapszintű struktúrát a program írása.

1. Vissza a hello Program.cs lapon cserélje le a hello `using` hello felső hello alábbi irányelveket:
   
    ```csharp
    using System;
    using System.Collections.Generic;
    using Microsoft.Azure.Management.Cdn;
    using Microsoft.Azure.Management.Cdn.Models;
    using Microsoft.Azure.Management.Resources;
    using Microsoft.Azure.Management.Resources.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```
2. Toodefine kell néhány állandók a módszerek fogja használni.  A hello `Program` osztály, de előtt hello `Main` módszer, adja hozzá a következő hello.  Lehet, hogy tooreplace hello helyőrzők, beleértve a hello  **&lt;csúcsos zárójelek&gt;**, igény szerint a saját értékekkel.
   
    ```csharp
    //Tenant app constants
    private const string clientID = "<YOUR CLIENT ID>";
    private const string clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
    private const string authority = "https://login.microsoftonline.com/<YOUR TENANT ID>/<YOUR TENANT DOMAIN NAME>";
   
    //Application constants
    private const string subscriptionId = "<YOUR SUBSCRIPTION ID>";
    private const string profileName = "CdnConsoleApp";
    private const string endpointName = "<A UNIQUE NAME FOR YOUR CDN ENDPOINT>";
    private const string resourceGroupName = "CdnConsoleTutorial";
    private const string resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
    ```
3. Hello osztály szinten is meghatározhat változókat.  Ha a profil és a végpont már létezik ezen újabb toodetermine fogjuk használni.
   
    ```csharp
    static bool profileAlreadyExists = false;
    static bool endpointAlreadyExists = false;
    ```
4. Cserélje le a hello `Main` módszert az alábbiak szerint:
   
   ```csharp
   static void Main(string[] args)
   {
       //Get a token
       AuthenticationResult authResult = GetAccessToken();
   
       // Create CDN client
       CdnManagementClient cdn = new CdnManagementClient(new TokenCredentials(authResult.AccessToken))
           { SubscriptionId = subscriptionId };
   
       ListProfilesAndEndpoints(cdn);
   
       // Create CDN Profile
       CreateCdnProfile(cdn);
   
       // Create CDN Endpoint
       CreateCdnEndpoint(cdn);
   
       Console.WriteLine();
   
       // Purge CDN Endpoint
       PromptPurgeCdnEndpoint(cdn);
   
       // Delete CDN Endpoint
       PromptDeleteCdnEndpoint(cdn);
   
       // Delete CDN Profile
       PromptDeleteCdnProfile(cdn);
   
       Console.WriteLine("Press Enter tooend program.");
       Console.ReadLine();
   }
   ```
5. Az egyéb módszerek közül fog tooprompt hello felhasználó az "Igen/nem" kérdésekre.  A következő metódus toomake hello hozzáadása, amely kissé egyszerűbbé:
   
    ```csharp
    private static bool PromptUser(string Question)
    {
        Console.Write(Question + " (Y/N): ");
        var response = Console.ReadKey();
        Console.WriteLine();
        if (response.Key == ConsoleKey.Y)
        {
            return true;
        }
        else if (response.Key == ConsoleKey.N)
        {
            return false;
        }
        else
        {
            // They pressed something other than Y or N.  Let's ask them again.
            return PromptUser(Question);
        }
    }
    ```

Most, hogy a program alapvető szerkezete hello írása, igazolnia kell létrehoznia hello által meghívott hello módszerek `Main` metódust.

## <a name="authentication"></a>Authentication
Ahhoz, hogy hello Azure CDN könyvtár, azt kell tooauthenticate szolgáltatás egyszerű, és szerezzen be egy hitelesítési jogkivonatot.  Ezt a módszert használja az ADAL tooretrieve hello jogkivonatot.

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority); 
    ClientCredential credential = new ClientCredential(clientID, clientSecret);
    AuthenticationResult authResult = 
        authContext.AcquireTokenAsync("https://management.core.windows.net/", credential).Result;

    return authResult;
}
```

Ha egyéni felhasználói hitelesítést használ, hello `GetAccessToken` metódus némileg eltérő fog kinézni.

> [!IMPORTANT]
> Ha használja a kódmintában toohave egyes felhasználói hitelesítés helyett egy egyszerű szolgáltatás kiválasztása.
> 
> 

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority);
    AuthenticationResult authResult = authContext.AcquireTokenAsync("https://management.core.windows.net/",
        clientID, new Uri("http://<redirect URI>"), new PlatformParameters(PromptBehavior.RefreshSession)).Result;

    return authResult;
}
```

Lehet, hogy tooreplace `<redirect URI>` hello az átirányítási URI-címe hello alkalmazás az Azure AD-ban történő regisztrálásakor megadott.

## <a name="list-cdn-profiles-and-endpoints"></a>Lista CDN-profil és -végpontok
Most már készen áll a tooperform CDN műveleteket a rendszer.  hello elsőként a metódus végzi lista hello-profilok és a végpontok az erőforráscsoportban, és ha egyezést talál az állandókat megadott hello-profil és a végpont nevek válik, jegyezze fel, amelyek később így azt nem próbálja toocreate ismétlődések.

```csharp
private static void ListProfilesAndEndpoints(CdnManagementClient cdn)
{
    // List all hello CDN profiles in this resource group
    var profileList = cdn.Profiles.ListByResourceGroup(resourceGroupName);
    foreach (Profile p in profileList)
    {
        Console.WriteLine("CDN profile {0}", p.Name);
        if (p.Name.Equals(profileName, StringComparison.OrdinalIgnoreCase))
        {
            // Hey, that's hello name of hello CDN profile we want toocreate!
            profileAlreadyExists = true;
        }

        //List all hello CDN endpoints on this CDN profile
        Console.WriteLine("Endpoints:");
        var endpointList = cdn.Endpoints.ListByProfile(p.Name, resourceGroupName);
        foreach (Endpoint e in endpointList)
        {
            Console.WriteLine("-{0} ({1})", e.Name, e.HostName);
            if (e.Name.Equals(endpointName, StringComparison.OrdinalIgnoreCase))
            {
                // hello unique endpoint name already exists.
                endpointAlreadyExists = true;
            }
        }
        Console.WriteLine();
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a>CDN-profil és a végpontok létrehozása
Ezután létrehozunk egy profilt.

```csharp
private static void CreateCdnProfile(CdnManagementClient cdn)
{
    if (profileAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating profile {0}.", profileName);
        ProfileCreateParameters profileParms =
            new ProfileCreateParameters() { Location = resourceLocation, Sku = new Sku(SkuName.StandardVerizon) };
        cdn.Profiles.Create(profileName, profileParms, resourceGroupName);
    }
}
```

Hello-profil létrehozása után létrehozunk egy végpontot.

```csharp
private static void CreateCdnEndpoint(CdnManagementClient cdn)
{
    if (endpointAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating endpoint {0} on profile {1}.", endpointName, profileName);
        EndpointCreateParameters endpointParms =
            new EndpointCreateParameters()
            {
                Origins = new List<DeepCreatedOrigin>() { new DeepCreatedOrigin("Contoso", "www.contoso.com") },
                IsHttpAllowed = true,
                IsHttpsAllowed = true,
                Location = resourceLocation
            };
        cdn.Endpoints.Create(endpointName, endpointParms, profileName, resourceGroupName);
    }
}
```

> [!NOTE]
> a fenti hello példa hozzárendeli hello végpont egy eredeti adatforrást nevű *Contoso* állomásnévvel rendelkező `www.contoso.com`.  Meg kell változtatni a toopoint tooyour saját forrás állomásneve.
> 
> 

## <a name="purge-an-endpoint"></a>A végpont törlése
Ha hello végpont létrejött, egy közös feladat, hogy a program célszerű lehet tooperform van kiürítése hello tartalma a végpont.

```csharp
private static void PromptPurgeCdnEndpoint(CdnManagementClient cdn)
{
    if (PromptUser(String.Format("Purge CDN endpoint {0}?", endpointName)))
    {
        Console.WriteLine("Purging endpoint. Please wait...");
        cdn.Endpoints.PurgeContent(endpointName, profileName, resourceGroupName, new List<string>() { "/*" });
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

> [!NOTE]
> Hello a fenti példában a karakterlánc hello `/*` azt jelzi, hogy szeretném toopurge hello gyökérmappájában hello végpont elérési útja a tartalmát.  Ez az egyenértékű toochecking **összes kiürítése** hello az Azure portálon "kiürítése" párbeszédpanelen. A hello `CreateCdnProfile` módszer, mint a profil létrehozott egy **Azure CDN Verizon** hello kóddal profil `Sku = new Sku(SkuName.StandardVerizon)`, így ez sikeres lesz.  Azonban **Akamai Azure CDN** profilok nem támogatják a **összes kiürítése**, így a rendszer egy Akamai profilt alkalmaz az oktatóanyag, lenne szükség tooinclude egyedi elérési utak toopurge.
> 
> 

## <a name="delete-cdn-profiles-and-endpoints"></a>CDN-profil és a végpontok törlése
hello utolsó módszerek törli a végpont és a profilt.

```csharp
private static void PromptDeleteCdnEndpoint(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN endpoint {0} on profile {1}?", endpointName, profileName)))
    {
        Console.WriteLine("Deleting endpoint. Please wait...");
        cdn.Endpoints.DeleteIfExists(endpointName, profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}

private static void PromptDeleteCdnProfile(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN profile {0}?", profileName)))
    {
        Console.WriteLine("Deleting profile. Please wait...");
        cdn.Profiles.DeleteIfExists(profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

## <a name="running-hello-program"></a>Hello programot futtat
Azt mostantól összeállítása és hello programfuttatást hello kattintva **Start** gombra a Visual Studio.

![A program fut](./media/cdn-app-dev-net/cdn-program-running-1.png)

Újabb kérés hello hello program elérésekor legyen képes tooreturn tooyour erőforráscsoport hello Azure-portálon és tekintse meg, hogy hello-profil létrehozása.

![Sikerült!](./media/cdn-app-dev-net/cdn-success.png)

A Microsoft hello kér toorun hello többi hello program majd ellenőrizheti.

![Program befejezése](./media/cdn-app-dev-net/cdn-program-running-2.png)

## <a name="next-steps"></a>Következő lépések
toosee befejeződött hello projektet ebben a forgatókönyvben a [hello minta letöltése](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c).

toofind további dokumentációiért hello Azure CDN könyvtár a .NET-hez, a nézet hello [hivatkozást az MSDN-en](https://msdn.microsoft.com/library/mt657769.aspx).

A CDN erőforrások kezelése [PowerShell](cdn-manage-powershell.md).

