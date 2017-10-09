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
# <a name="get-started-with-azure-cdn-development"></a><span data-ttu-id="0cfb3-103">Ismerkedés az Azure CDN-fejlesztéssel</span><span class="sxs-lookup"><span data-stu-id="0cfb3-103">Get started with Azure CDN development</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0cfb3-104">Node.js</span><span class="sxs-lookup"><span data-stu-id="0cfb3-104">Node.js</span></span>](cdn-app-dev-node.md)
> * [<span data-ttu-id="0cfb3-105">.NET</span><span class="sxs-lookup"><span data-stu-id="0cfb3-105">.NET</span></span>](cdn-app-dev-net.md)
> 
> 

<span data-ttu-id="0cfb3-106">Használhatja a hello [Azure CDN .NET-keretrendszerhez készült](https://msdn.microsoft.com/library/mt657769.aspx) tooautomate létrehozását és a CDN-profil és a végpontok.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-106">You can use hello [Azure CDN Library for .NET](https://msdn.microsoft.com/library/mt657769.aspx) tooautomate creation and management of CDN profiles and endpoints.</span></span>  <span data-ttu-id="0cfb3-107">Ez az oktatóanyag végigvezeti egy egyszerű .NET Konzolalkalmazás azt mutatja be, hogy több hello elérhető műveletek hello létrehozását.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-107">This tutorial walks through hello creation of a simple .NET console application that demonstrates several of hello available operations.</span></span>  <span data-ttu-id="0cfb3-108">Ez az oktatóanyag van nem tervezett toodescribe minden szempontját hello Azure CDN könyvtár a .NET-hez részletesen.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-108">This tutorial is not intended toodescribe all aspects of hello Azure CDN Library for .NET in detail.</span></span>

<span data-ttu-id="0cfb3-109">Ez az oktatóanyag szüksége van a Visual Studio 2015 toocomplete.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-109">You need Visual Studio 2015 toocomplete this tutorial.</span></span>  <span data-ttu-id="0cfb3-110">[A Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) szabadon letölthető.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-110">[Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) is freely available for download.</span></span>

> [!TIP]
> <span data-ttu-id="0cfb3-111">Hello [az oktatóanyagot befejezett projekt](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c) letölthető az MSDN Webhelyén.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-111">hello [completed project from this tutorial](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c) is available for download on MSDN.</span></span>
> 
> 

[!INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-nuget-packages"></a><span data-ttu-id="0cfb3-112">A projekt létrehozása, és adja hozzá a Nuget-csomagok</span><span class="sxs-lookup"><span data-stu-id="0cfb3-112">Create your project and add Nuget packages</span></span>
<span data-ttu-id="0cfb3-113">Most, hogy előre létrehozott erőforráscsoport a CDN-profilok és az Azure AD alkalmazás engedély toomanage CDN-profil és a csoporton belüli végpontok, nem lehet elindítani, az alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-113">Now that we've created a resource group for our CDN profiles and given our Azure AD application permission toomanage CDN profiles and endpoints within that group, we can start creating our application.</span></span>

<span data-ttu-id="0cfb3-114">A Visual Studio 2015-öt, belül kattintson **fájl**, **új**, **projekt...**  tooopen hello új projekt párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-114">From within Visual Studio 2015, click **File**, **New**, **Project...** tooopen hello new project dialog.</span></span>  <span data-ttu-id="0cfb3-115">Bontsa ki a **Visual C#**, majd jelölje be **Windows** hello bal hello területen.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-115">Expand **Visual C#**, then select **Windows** in hello pane on hello left.</span></span>  <span data-ttu-id="0cfb3-116">Kattintson a **Konzolalkalmazás** hello középső ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-116">Click **Console Application** in hello center pane.</span></span>  <span data-ttu-id="0cfb3-117">Nevezze el a projektet, majd kattintson az **OK**.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-117">Name your project, then click **OK**.</span></span>  

![Új projekt](./media/cdn-app-dev-net/cdn-new-project.png)

<span data-ttu-id="0cfb3-119">A projekt Nuget-csomagok szereplő Azure könyvtárak toouse lesz.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-119">Our project is going toouse some Azure libraries contained in Nuget packages.</span></span>  <span data-ttu-id="0cfb3-120">E toohello projekt adjuk hozzá.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-120">Let's add those toohello project.</span></span>

1. <span data-ttu-id="0cfb3-121">Kattintson a hello **eszközök** menü **Nuget-Csomagkezelő**, majd **Csomagkezelő konzol**.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-121">Click hello **Tools** menu, **Nuget Package Manager**, then **Package Manager Console**.</span></span>
   
    ![Nuget-csomagok kezelése](./media/cdn-app-dev-net/cdn-manage-nuget.png)
2. <span data-ttu-id="0cfb3-123">A Csomagkezelő konzol hello, hajtsa végre a következő parancs tooinstall hello hello **Active Directory Authentication Library (ADAL)**:</span><span class="sxs-lookup"><span data-stu-id="0cfb3-123">In hello Package Manager Console, execute hello following command tooinstall hello **Active Directory Authentication Library (ADAL)**:</span></span>
   
    `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory`
3. <span data-ttu-id="0cfb3-124">Hajtsa végre a következő tooinstall hello hello **Azure CDN könyvtár**:</span><span class="sxs-lookup"><span data-stu-id="0cfb3-124">Execute hello following tooinstall hello **Azure CDN Management Library**:</span></span>
   
    `Install-Package Microsoft.Azure.Management.Cdn`

## <a name="directives-constants-main-method-and-helper-methods"></a><span data-ttu-id="0cfb3-125">Irányelvek, állandók, fő metódus és segédmódszereket</span><span class="sxs-lookup"><span data-stu-id="0cfb3-125">Directives, constants, main method, and helper methods</span></span>
<span data-ttu-id="0cfb3-126">Folytassuk hello alapszintű struktúrát a program írása.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-126">Let's get hello basic structure of our program written.</span></span>

1. <span data-ttu-id="0cfb3-127">Vissza a hello Program.cs lapon cserélje le a hello `using` hello felső hello alábbi irányelveket:</span><span class="sxs-lookup"><span data-stu-id="0cfb3-127">Back in hello Program.cs tab, replace hello `using` directives at hello top with hello following:</span></span>
   
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
2. <span data-ttu-id="0cfb3-128">Toodefine kell néhány állandók a módszerek fogja használni.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-128">We need toodefine some constants our methods will use.</span></span>  <span data-ttu-id="0cfb3-129">A hello `Program` osztály, de előtt hello `Main` módszer, adja hozzá a következő hello.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-129">In hello `Program` class, but before hello `Main` method, add hello following.</span></span>  <span data-ttu-id="0cfb3-130">Lehet, hogy tooreplace hello helyőrzők, beleértve a hello  **&lt;csúcsos zárójelek&gt;**, igény szerint a saját értékekkel.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-130">Be sure tooreplace hello placeholders, including hello **&lt;angle brackets&gt;**, with your own values as needed.</span></span>
   
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
3. <span data-ttu-id="0cfb3-131">Hello osztály szinten is meghatározhat változókat.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-131">Also at hello class level, define these two variables.</span></span>  <span data-ttu-id="0cfb3-132">Ha a profil és a végpont már létezik ezen újabb toodetermine fogjuk használni.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-132">We'll use these later toodetermine if our profile and endpoint already exist.</span></span>
   
    ```csharp
    static bool profileAlreadyExists = false;
    static bool endpointAlreadyExists = false;
    ```
4. <span data-ttu-id="0cfb3-133">Cserélje le a hello `Main` módszert az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="0cfb3-133">Replace hello `Main` method as follows:</span></span>
   
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
5. <span data-ttu-id="0cfb3-134">Az egyéb módszerek közül fog tooprompt hello felhasználó az "Igen/nem" kérdésekre.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-134">Some of our other methods are going tooprompt hello user with "Yes/No" questions.</span></span>  <span data-ttu-id="0cfb3-135">A következő metódus toomake hello hozzáadása, amely kissé egyszerűbbé:</span><span class="sxs-lookup"><span data-stu-id="0cfb3-135">Add hello following method toomake that a little easier:</span></span>
   
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

<span data-ttu-id="0cfb3-136">Most, hogy a program alapvető szerkezete hello írása, igazolnia kell létrehoznia hello által meghívott hello módszerek `Main` metódust.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-136">Now that hello basic structure of our program is written, we should create hello methods called by hello `Main` method.</span></span>

## <a name="authentication"></a><span data-ttu-id="0cfb3-137">Authentication</span><span class="sxs-lookup"><span data-stu-id="0cfb3-137">Authentication</span></span>
<span data-ttu-id="0cfb3-138">Ahhoz, hogy hello Azure CDN könyvtár, azt kell tooauthenticate szolgáltatás egyszerű, és szerezzen be egy hitelesítési jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-138">Before we can use hello Azure CDN Management Library, we need tooauthenticate our service principal and obtain an authentication token.</span></span>  <span data-ttu-id="0cfb3-139">Ezt a módszert használja az ADAL tooretrieve hello jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-139">This method uses ADAL tooretrieve hello token.</span></span>

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

<span data-ttu-id="0cfb3-140">Ha egyéni felhasználói hitelesítést használ, hello `GetAccessToken` metódus némileg eltérő fog kinézni.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-140">If you are using individual user authentication, hello `GetAccessToken` method will look slightly different.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0cfb3-141">Ha használja a kódmintában toohave egyes felhasználói hitelesítés helyett egy egyszerű szolgáltatás kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-141">Only use this code sample if you are choosing toohave individual user authentication instead of a service principal.</span></span>
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

<span data-ttu-id="0cfb3-142">Lehet, hogy tooreplace `<redirect URI>` hello az átirányítási URI-címe hello alkalmazás az Azure AD-ban történő regisztrálásakor megadott.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-142">Be sure tooreplace `<redirect URI>` with hello redirect URI you entered when you registered hello application in Azure AD.</span></span>

## <a name="list-cdn-profiles-and-endpoints"></a><span data-ttu-id="0cfb3-143">Lista CDN-profil és -végpontok</span><span class="sxs-lookup"><span data-stu-id="0cfb3-143">List CDN profiles and endpoints</span></span>
<span data-ttu-id="0cfb3-144">Most már készen áll a tooperform CDN műveleteket a rendszer.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-144">Now we're ready tooperform CDN operations.</span></span>  <span data-ttu-id="0cfb3-145">hello elsőként a metódus végzi lista hello-profilok és a végpontok az erőforráscsoportban, és ha egyezést talál az állandókat megadott hello-profil és a végpont nevek válik, jegyezze fel, amelyek később így azt nem próbálja toocreate ismétlődések.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-145">hello first thing our method does is list all hello profiles and endpoints in our resource group, and if it finds a match for hello profile and endpoint names specified in our constants, makes a note of that for later so we don't try toocreate duplicates.</span></span>

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

## <a name="create-cdn-profiles-and-endpoints"></a><span data-ttu-id="0cfb3-146">CDN-profil és a végpontok létrehozása</span><span class="sxs-lookup"><span data-stu-id="0cfb3-146">Create CDN profiles and endpoints</span></span>
<span data-ttu-id="0cfb3-147">Ezután létrehozunk egy profilt.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-147">Next, we'll create a profile.</span></span>

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

<span data-ttu-id="0cfb3-148">Hello-profil létrehozása után létrehozunk egy végpontot.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-148">Once hello profile is created, we'll create an endpoint.</span></span>

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
> <span data-ttu-id="0cfb3-149">a fenti hello példa hozzárendeli hello végpont egy eredeti adatforrást nevű *Contoso* állomásnévvel rendelkező `www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-149">hello example above assigns hello endpoint an origin named *Contoso* with a hostname `www.contoso.com`.</span></span>  <span data-ttu-id="0cfb3-150">Meg kell változtatni a toopoint tooyour saját forrás állomásneve.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-150">You should change this toopoint tooyour own origin's hostname.</span></span>
> 
> 

## <a name="purge-an-endpoint"></a><span data-ttu-id="0cfb3-151">A végpont törlése</span><span class="sxs-lookup"><span data-stu-id="0cfb3-151">Purge an endpoint</span></span>
<span data-ttu-id="0cfb3-152">Ha hello végpont létrejött, egy közös feladat, hogy a program célszerű lehet tooperform van kiürítése hello tartalma a végpont.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-152">Assuming hello endpoint has been created, one common task that we might want tooperform in our program is purging hello content in our endpoint.</span></span>

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
> <span data-ttu-id="0cfb3-153">Hello a fenti példában a karakterlánc hello `/*` azt jelzi, hogy szeretném toopurge hello gyökérmappájában hello végpont elérési útja a tartalmát.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-153">In hello example above, hello string `/*` denotes that I want toopurge everything in hello root of hello endpoint path.</span></span>  <span data-ttu-id="0cfb3-154">Ez az egyenértékű toochecking **összes kiürítése** hello az Azure portálon "kiürítése" párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-154">This is equivalent toochecking **Purge All** in hello Azure portal's "purge" dialog.</span></span> <span data-ttu-id="0cfb3-155">A hello `CreateCdnProfile` módszer, mint a profil létrehozott egy **Azure CDN Verizon** hello kóddal profil `Sku = new Sku(SkuName.StandardVerizon)`, így ez sikeres lesz.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-155">In hello `CreateCdnProfile` method, I created our profile as an **Azure CDN from Verizon** profile using hello code `Sku = new Sku(SkuName.StandardVerizon)`, so this will be successful.</span></span>  <span data-ttu-id="0cfb3-156">Azonban **Akamai Azure CDN** profilok nem támogatják a **összes kiürítése**, így a rendszer egy Akamai profilt alkalmaz az oktatóanyag, lenne szükség tooinclude egyedi elérési utak toopurge.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-156">However, **Azure CDN from Akamai** profiles do not support **Purge All**, so if I was using an Akamai profile for this tutorial, I would need tooinclude specific paths toopurge.</span></span>
> 
> 

## <a name="delete-cdn-profiles-and-endpoints"></a><span data-ttu-id="0cfb3-157">CDN-profil és a végpontok törlése</span><span class="sxs-lookup"><span data-stu-id="0cfb3-157">Delete CDN profiles and endpoints</span></span>
<span data-ttu-id="0cfb3-158">hello utolsó módszerek törli a végpont és a profilt.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-158">hello last methods will delete our endpoint and profile.</span></span>

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

## <a name="running-hello-program"></a><span data-ttu-id="0cfb3-159">Hello programot futtat</span><span class="sxs-lookup"><span data-stu-id="0cfb3-159">Running hello program</span></span>
<span data-ttu-id="0cfb3-160">Azt mostantól összeállítása és hello programfuttatást hello kattintva **Start** gombra a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-160">We can now compile and run hello program by clicking hello **Start** button in Visual Studio.</span></span>

![A program fut](./media/cdn-app-dev-net/cdn-program-running-1.png)

<span data-ttu-id="0cfb3-162">Újabb kérés hello hello program elérésekor legyen képes tooreturn tooyour erőforráscsoport hello Azure-portálon és tekintse meg, hogy hello-profil létrehozása.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-162">When hello program reaches hello above prompt, you should be able tooreturn tooyour resource group in hello Azure portal and see that hello profile has been created.</span></span>

![Sikerült!](./media/cdn-app-dev-net/cdn-success.png)

<span data-ttu-id="0cfb3-164">A Microsoft hello kér toorun hello többi hello program majd ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="0cfb3-164">We can then confirm hello prompts toorun hello rest of hello program.</span></span>

![Program befejezése](./media/cdn-app-dev-net/cdn-program-running-2.png)

## <a name="next-steps"></a><span data-ttu-id="0cfb3-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0cfb3-166">Next Steps</span></span>
<span data-ttu-id="0cfb3-167">toosee befejeződött hello projektet ebben a forgatókönyvben a [hello minta letöltése](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c).</span><span class="sxs-lookup"><span data-stu-id="0cfb3-167">toosee hello completed project from this walkthrough, [download hello sample](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c).</span></span>

<span data-ttu-id="0cfb3-168">toofind további dokumentációiért hello Azure CDN könyvtár a .NET-hez, a nézet hello [hivatkozást az MSDN-en](https://msdn.microsoft.com/library/mt657769.aspx).</span><span class="sxs-lookup"><span data-stu-id="0cfb3-168">toofind additional documentation on hello Azure CDN Management Library for .NET, view hello [reference on MSDN](https://msdn.microsoft.com/library/mt657769.aspx).</span></span>

<span data-ttu-id="0cfb3-169">A CDN erőforrások kezelése [PowerShell](cdn-manage-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="0cfb3-169">Manage your CDN resources with [PowerShell](cdn-manage-powershell.md).</span></span>

