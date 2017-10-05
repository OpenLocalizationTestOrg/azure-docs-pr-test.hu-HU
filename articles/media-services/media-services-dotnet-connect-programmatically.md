---
title: "Kapcsolódás a Media Services-fiók .NET használatával"
description: "Ebben a témakörben bemutatjuk, hogyan csatlakozhat a Media Services előrejelzése .NET."
services: media-services
documentationcenter: 
author: juliako
manager: erikre
editor: 
ms.assetid: a8412a29-59dc-44a0-ace0-be79a97dab63
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 09/26/2016
ms.author: juliako
ms.openlocfilehash: 892932116934952265a21ab17aac3434b5760136
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="connecting-to-media-services-account-using-media-services-sdk-for-net"></a><span data-ttu-id="73117-103">Kapcsolódás a Media Services SDK használatával a .NET-keretrendszerhez készült Media Services-fiók</span><span class="sxs-lookup"><span data-stu-id="73117-103">Connecting to Media Services Account using Media Services SDK for .NET</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="73117-104">REST</span><span class="sxs-lookup"><span data-stu-id="73117-104">REST</span></span>](media-services-rest-connect-programmatically.md)
> * [<span data-ttu-id="73117-105">.NET</span><span class="sxs-lookup"><span data-stu-id="73117-105">.NET</span></span>](media-services-dotnet-connect-programmatically.md)
> 
> 

<span data-ttu-id="73117-106">Ez a témakör ismerteti az beszerzése programozott kapcsolódni a Microsoft Azure Media Services, ha a .NET-hez programozási a Media Services SDK-val.</span><span class="sxs-lookup"><span data-stu-id="73117-106">This topic describes how to obtain a programmatic connection to Microsoft Azure Media Services when you are programming with the Media Services SDK for .NET.</span></span>

## <a name="connecting-to-media-services"></a><span data-ttu-id="73117-107">Csatlakozás a Media Services</span><span class="sxs-lookup"><span data-stu-id="73117-107">Connecting to Media Services</span></span>
<span data-ttu-id="73117-108">Csatlakozhat a Media Services programozott módon, meg kell rendelkezik korábban már beállított egy Azure-fiókra, a Media Services konfigurált fiók és hozzon létre egy Visual Studio-projekt fejlesztési a Media Services SDK-t a .NET-hez.</span><span class="sxs-lookup"><span data-stu-id="73117-108">To connect to Media Services programmatically, you must have previously set up an Azure account, configured Media Services on that account, and then set up a Visual Studio project for development with the Media Services SDK for .NET.</span></span> <span data-ttu-id="73117-109">További információkért lásd: a telepítő fejlesztési a Media Services SDK-t a .NET-hez.</span><span class="sxs-lookup"><span data-stu-id="73117-109">For more information, see Setup for Development with the Media Services SDK for .NET.</span></span>

<span data-ttu-id="73117-110">A Media Services-fiók telepítési folyamat végén szerezte be a következő kapcsolathoz szükséges értékeket.</span><span class="sxs-lookup"><span data-stu-id="73117-110">At the end of the Media Services account setup process, you obtained the following required connection values.</span></span> <span data-ttu-id="73117-111">Ezek segítségével programozott kapcsolat létrehozása a Media Services.</span><span class="sxs-lookup"><span data-stu-id="73117-111">Use these to make programmatic connections to Media Services.</span></span>

* <span data-ttu-id="73117-112">A Media Services-fiók neve.</span><span class="sxs-lookup"><span data-stu-id="73117-112">Your Media Services account name.</span></span>
* <span data-ttu-id="73117-113">A Media Services kulcsára.</span><span class="sxs-lookup"><span data-stu-id="73117-113">Your Media Services account key.</span></span>

<span data-ttu-id="73117-114">Ezek az értékek megkereséséhez nyissa meg az Azure Managmentet portálra, válassza ki a Media Services-fiókját, és kattintson a a "**kulcsok kezelése**" ikon a portál ablakának alján.</span><span class="sxs-lookup"><span data-stu-id="73117-114">To find these values, go to the Azure Managment Portal, select your Media Service account, and click on the “**MANAGE KEYS**” icon on the bottom of the portal window.</span></span> <span data-ttu-id="73117-115">A szövegdobozok melletti ikonokra való kattintás a rendszer vágólapjára másolja az értékeket.</span><span class="sxs-lookup"><span data-stu-id="73117-115">Clicking on the icon next to each text box copies the value to the system clipboard.</span></span>

## <a name="creating-a-cloudmediacontext-instance"></a><span data-ttu-id="73117-116">CloudMediaContext példány létrehozása</span><span class="sxs-lookup"><span data-stu-id="73117-116">Creating a CloudMediaContext Instance</span></span>
<span data-ttu-id="73117-117">Elleni kell létrehoznia a Media Services programozási elindítani egy **CloudMediaContext** példányt jelenti. a kiszolgáló a környezetben.</span><span class="sxs-lookup"><span data-stu-id="73117-117">To start programming against Media Services you need to create a **CloudMediaContext** instance that represents the server context.</span></span> <span data-ttu-id="73117-118">A **CloudMediaContext** többek között a feladatok, eszközök, fájlok, hozzáférési házirendek és keresők fontos gyűjtemények mutató hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="73117-118">The **CloudMediaContext** includes references to important collections including jobs, assets, files, access policies, and locators.</span></span>

> [!NOTE]
> <span data-ttu-id="73117-119">A **CloudMediaContext** osztály nincs többszálú futtatásra.</span><span class="sxs-lookup"><span data-stu-id="73117-119">The **CloudMediaContext** class is not thread safe.</span></span> <span data-ttu-id="73117-120">Hozzon létre egy új CloudMediaContext szálankénti, vagy a műveletek.</span><span class="sxs-lookup"><span data-stu-id="73117-120">You should create a new CloudMediaContext per thread or per set of operations.</span></span>
> 
> 

<span data-ttu-id="73117-121">CloudMediaContext öt konstruktor túlterheléssel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="73117-121">CloudMediaContext has five constructor overloads.</span></span> <span data-ttu-id="73117-122">Javasoljuk, hogy használja a konstruktorok használatát, amelyek **MediaServicesCredentials** paraméterként.</span><span class="sxs-lookup"><span data-stu-id="73117-122">It is recommended to use constructors that take **MediaServicesCredentials** as a parameter.</span></span> <span data-ttu-id="73117-123">További információkért lásd: a **újból felhasználja a vezérlő szolgáltatás jogkivonatot** , amely a következő.</span><span class="sxs-lookup"><span data-stu-id="73117-123">For more information, see the **Reusing Access Control Service Tokens** that follows.</span></span> 

<span data-ttu-id="73117-124">A következő példában a CloudMediaContext(MediaServicesCredentials credentials) nyilvános konstruktora:</span><span class="sxs-lookup"><span data-stu-id="73117-124">The following example uses the public CloudMediaContext(MediaServicesCredentials credentials) constructor:</span></span>

    // _cachedCredentials and _context are class member variables. 
    _cachedCredentials = new MediaServicesCredentials(
                    _mediaServicesAccountName,
                    _mediaServicesAccountKey);

    _context = new CloudMediaContext(_cachedCredentials);


## <a name="reusing-access-control-service-tokens"></a><span data-ttu-id="73117-125">Újbóli felhasználása a vezérlő szolgáltatás jogkivonatot</span><span class="sxs-lookup"><span data-stu-id="73117-125">Reusing Access Control Service Tokens</span></span>
<span data-ttu-id="73117-126">Ez a szakasz bemutatja, hogyan használja fel a hozzáférés-vezérlési szolgáltatásban jogkivonatok CloudMediaContext konstruktorok használatát, amelyek paraméterként MediaServicesCredentials használatával.</span><span class="sxs-lookup"><span data-stu-id="73117-126">This section shows how to reuse Access Control Service tokens by using CloudMediaContext constructors that take MediaServicesCredentials as a parameter.</span></span>

<span data-ttu-id="73117-127">[Az Azure Active Directory hozzáférés-vezérlés](https://msdn.microsoft.com/library/hh147631.aspx) (más néven a hozzáférés-vezérlési szolgáltatásban vagy az ACS) ahhoz, hogy hozzáférjenek a webes alkalmazások és felhasználók hitelesítésére egyszerűen biztosító felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="73117-127">[Azure Active Directory Access Control](https://msdn.microsoft.com/library/hh147631.aspx) (also known as Access Control Service or ACS) is a cloud-based service that provides an easy way of authenticating and authorizing users to gain access to their web applications.</span></span> <span data-ttu-id="73117-128">A Microsoft Azure Media Services, ha meghatározza a a szolgáltatásokhoz való hozzáférés, OAuth protokollt igényel az ACS-jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="73117-128">Microsoft Azure Media Services controls access to its services though OAuth protocol that requires an ACS token.</span></span> <span data-ttu-id="73117-129">A Media Services az ACS-jogkivonatokat kap egy engedélyezési kiszolgálóról.</span><span class="sxs-lookup"><span data-stu-id="73117-129">Media Services receives the ACS tokens from an authorization server.</span></span>

<span data-ttu-id="73117-130">A Media Services SDK-val fejlesztésekor választhat foglalkozik a jogkivonatokat, mert az SDK-kód kezelők meg őket.</span><span class="sxs-lookup"><span data-stu-id="73117-130">When developing with the Media Services SDK, you can choose to not deal with the tokens because the SDK code managers them for you.</span></span> <span data-ttu-id="73117-131">Azonban ha engedélyezi, hogy a teljes körű felügyeletéhez az ACS-jogkivonatokat SDK vezet szükségtelen jogkivonat-kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="73117-131">However, letting the SDK fully manage the ACS tokens leads to unnecessary token requests.</span></span> <span data-ttu-id="73117-132">A kért jogkivonatok időt vesz igénybe, és az ügyfél és kiszolgáló-erőforrásokat használ fel.</span><span class="sxs-lookup"><span data-stu-id="73117-132">Requesting tokens takes time and consumes the client and server resources.</span></span> <span data-ttu-id="73117-133">Emellett az ACS-kiszolgálónak azelőtt gyorsítja fel a kérelmeket, ha az érték túl magas.</span><span class="sxs-lookup"><span data-stu-id="73117-133">Also, the ACS server throttles the requests if the rate is too high.</span></span> <span data-ttu-id="73117-134">A határérték 30 kérelmek / másodperc, lásd: [ACS Service korlátozásai](https://msdn.microsoft.com/library/gg185909.aspx) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="73117-134">The limit is 30 requests per second, see [ACS Service Limitations](https://msdn.microsoft.com/library/gg185909.aspx) for more details.</span></span>

<span data-ttu-id="73117-135">A Media Services SDK verzió 3.0.0.0 verziótól kezdődően az ACS-jogkivonatokat is felhasználhatja.</span><span class="sxs-lookup"><span data-stu-id="73117-135">Starting with the Media Services SDK version 3.0.0.0, you can reuse the ACS tokens.</span></span> <span data-ttu-id="73117-136">A **CloudMediaContext** konstruktorok használatát, amelyek **MediaServicesCredentials** paraméterként a ACS jogkivonatok több környezet közötti megosztásának engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="73117-136">The **CloudMediaContext** constructors that take **MediaServicesCredentials** as a parameter enable sharing the ACS tokens between multiple contexts.</span></span> <span data-ttu-id="73117-137">A MediaServicesCredentials osztály magában foglalja a Media Services hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="73117-137">The MediaServicesCredentials class encapsulates the Media Services credentials.</span></span> <span data-ttu-id="73117-138">Ha rendelkezésre áll az ACS-jogkivonatot, és a lejárati ideje ismert, hozzon létre egy új MediaServicesCredentials-példányt a jogkivonatot, és adja át CloudMediaContext konstruktorának.</span><span class="sxs-lookup"><span data-stu-id="73117-138">If an ACS token is available and its expiration time is known, you can create a new MediaServicesCredentials instance with the token and pass it to the constructor of CloudMediaContext.</span></span> <span data-ttu-id="73117-139">Vegye figyelembe, hogy a Media Services SDK automatikusan frissíti jogkivonatokat, amikor a várósorban.</span><span class="sxs-lookup"><span data-stu-id="73117-139">Note that the Media Services SDK automatically refreshes tokens whenever they expire.</span></span> <span data-ttu-id="73117-140">Két módon újból az ACS-jogkivonatokat, az alábbi példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="73117-140">There are two ways to reuse ACS tokens, as shown in the examples below.</span></span>

* <span data-ttu-id="73117-141">Gyorsítótárazhatja a **MediaServicesCredentials** a memóriában (például a statikus osztály változóban) objektum.</span><span class="sxs-lookup"><span data-stu-id="73117-141">You can cache the **MediaServicesCredentials** object in memory (for example, in a static class variable).</span></span> <span data-ttu-id="73117-142">Ezt követően adja át a gyorsítótárazott objektum a CloudMediaContext konstruktor.</span><span class="sxs-lookup"><span data-stu-id="73117-142">Then, pass the cached object to the CloudMediaContext constructor.</span></span> <span data-ttu-id="73117-143">A MediaServicesCredentials objektum tartalmaz, amelyek felhasználhatók, ha továbbra is érvényes ACS-jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="73117-143">The MediaServicesCredentials object contains an ACS token that can be reused if it is still valid.</span></span> <span data-ttu-id="73117-144">Ha a jogkivonat nem érvényes, akkor frissülnek a Media Services SDK-ban a MediaServicesCredentials konstruktor megadott hitelesítő adatok használatával.</span><span class="sxs-lookup"><span data-stu-id="73117-144">If the token is not valid, it will be refreshed by the Media Services SDK using the credentials given to the MediaServicesCredentials constructor.</span></span>
  
    <span data-ttu-id="73117-145">Vegye figyelembe, hogy a **MediaServicesCredentials** objektum egy érvényes tokent kap, miután a RefreshToken nevezik.</span><span class="sxs-lookup"><span data-stu-id="73117-145">Note that the **MediaServicesCredentials** object gets a valid token after the RefreshToken is called.</span></span> <span data-ttu-id="73117-146">A **CloudMediaContext** hívások a **RefreshToken** módszer a konstruktorban.</span><span class="sxs-lookup"><span data-stu-id="73117-146">The **CloudMediaContext** calls the **RefreshToken** method in the constructor.</span></span> <span data-ttu-id="73117-147">Ha azt tervezi, hogy a jogkivonat értékeket menteni egy külső tárhelyen, mindenképpen ellenőrizze, hogy TokenExpiration értéke érvénytelen a jogkivonat-adatainak mentése előtt.</span><span class="sxs-lookup"><span data-stu-id="73117-147">If you are planning to save the token values to an external storage, make sure to check whether the TokenExpiration value is valid before saving the token data.</span></span> <span data-ttu-id="73117-148">Érvénytelen, hívjuk RefreshToken gyorsítótárazás előtt.</span><span class="sxs-lookup"><span data-stu-id="73117-148">If it is not valid, call RefreshToken before caching.</span></span>
  
        // Create and cache the Media Services credentials in a static class variable.
        _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);

        // Use the cached credentials to create a new CloudMediaContext object.
        if(_cachedCredentials == null)
        {
            _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);
        }

        CloudMediaContext context = new CloudMediaContext(_cachedCredentials);

* <span data-ttu-id="73117-149">Az AccessToken karakterlánc és a TokenExpiration értékek is gyorsítótárazza.</span><span class="sxs-lookup"><span data-stu-id="73117-149">You can also cache the AccessToken string and the TokenExpiration values.</span></span> <span data-ttu-id="73117-150">Az értékek később használható hozzon létre egy új MediaServicesCredentials objektumot a gyorsítótárazott token adatokkal.</span><span class="sxs-lookup"><span data-stu-id="73117-150">The values could later be used to create a new MediaServicesCredentials object with the cached token data.</span></span>  <span data-ttu-id="73117-151">Ez különösen fontos a forgatókönyvek, ahol a token biztonságosan megoszthatók több folyamatok vagy számítógépek között.</span><span class="sxs-lookup"><span data-stu-id="73117-151">This is especially useful for scenarios where the token can be securely shared among multiple processes or computers.</span></span>
  
    <span data-ttu-id="73117-152">Az alábbi kódrészleteket metódushívások a SaveTokenDataToExternalStorage GetTokenDataFromExternalStorage és UpdateTokenDataInExternalStorageIfNeeded, amelyek nincsenek meghatározva ebben a példában.</span><span class="sxs-lookup"><span data-stu-id="73117-152">The following code snippets call the SaveTokenDataToExternalStorage, GetTokenDataFromExternalStorage, and UpdateTokenDataInExternalStorageIfNeeded methods that are not defined in this example.</span></span> <span data-ttu-id="73117-153">Ezek a módszerek tárolásához, beolvasása, és frissíti egy külső tároló token adatait adható meg.</span><span class="sxs-lookup"><span data-stu-id="73117-153">You could define these methods to store, retrieve, and update token data in an external storage.</span></span> 
  
        CloudMediaContext context1 = new CloudMediaContext(_mediaServicesAccountName, _mediaServicesAccountKey);
  
        // Get token values from the context.
        var accessToken = context1.Credentials.AccessToken;
        var tokenExpiration = context1.Credentials.TokenExpiration;
  
        // Save token values for later use. 
        // The SaveTokenDataToExternalStorage method should check 
        // whether the TokenExpiration value is valid before saving the token data. 
        // If it is not valid, call MediaServicesCredentials’s RefreshToken before caching.
        SaveTokenDataToExternalStorage(accessToken, tokenExpiration);
  
    <span data-ttu-id="73117-154">A mentett lexikális elemek értékének MediaServicesCredentials létrehozásához használja.</span><span class="sxs-lookup"><span data-stu-id="73117-154">Use the saved token values to create MediaServicesCredentials.</span></span>

        var accessToken = "";
        var tokenExpiration = DateTime.UtcNow;

        // Retrieve saved token values.
        GetTokenDataFromExternalStorage(out accessToken, out tokenExpiration);

        // Create a new MediaServicesCredentials object using saved token values.
        MediaServicesCredentials credentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey)
        {
            AccessToken = accessToken,
            TokenExpiration = tokenExpiration
        };

        CloudMediaContext context2 = new CloudMediaContext(credentials);

    <span data-ttu-id="73117-155">A token példány frissítése, abban az esetben, ha a jogkivonat a Media Services SDK frissítette.</span><span class="sxs-lookup"><span data-stu-id="73117-155">Update the token copy in case the token was updated by the Media Services SDK.</span></span> 

        if(tokenExpiration != context2.Credentials.TokenExpiration)
        {
            UpdateTokenDataInExternalStorageIfNeeded(accessToken, context2.Credentials.TokenExpiration);
        }


* <span data-ttu-id="73117-156">Ha több Media Services-fiókot (például földrajzi terjesztési vagy célú terheléselosztási) gyorsítótárazhatja a System.Collections.Concurrent.ConcurrentDictionary gyűjtemény (a ConcurrentDictionary MediaServicesCredentials az objektumok gyűjtemény számoknak szálbiztos kulcs/érték párok elérhető szálak egyidejű).</span><span class="sxs-lookup"><span data-stu-id="73117-156">If you have multiple Media Services accounts (for example, for load sharing purposes or Geo-distribution) you can cache MediaServicesCredentials objects using the System.Collections.Concurrent.ConcurrentDictionary collection (the ConcurrentDictionary collection represents a thread-safe collection of key/value pairs that can be accessed by multiple threads concurrently).</span></span> <span data-ttu-id="73117-157">Ezután használhatja a GetOrAdd módszert a gyorsítótárazott hitelesítő adatok elkérése céljából.</span><span class="sxs-lookup"><span data-stu-id="73117-157">You can then use the GetOrAdd method to get the cached credentials.</span></span> 
  
        // Declare a static class variable of the ConcurrentDictionary type in which the Media Services credentials will be cached.  
        private static readonly ConcurrentDictionary<string, MediaServicesCredentials> mediaServicesCredentialsCache = 
            new ConcurrentDictionary<string, MediaServicesCredentials>();

        // Cache (or get already cached) Media Services credentials. Use these credentials to create a new CloudMediaContext object.
        static public CloudMediaContext CreateMediaServicesContext(string accountName, string accountKey)
        {
            CloudMediaContext cloudMediaContext;
            MediaServicesCredentials mediaServicesCredentials;

            mediaServicesCredentials = mediaServicesCredentialsCache.GetOrAdd(
                accountName,
                valueFactory => new MediaServicesCredentials(accountName, accountKey));

            cloudMediaContext = new CloudMediaContext(mediaServicesCredentials);

            return cloudMediaContext;
        }

## <a name="connecting-to-a-media-services-account-located-in-the-north-china-region"></a><span data-ttu-id="73117-158">Észak-Kína régióban Media Services-fiókhoz való csatlakozást</span><span class="sxs-lookup"><span data-stu-id="73117-158">Connecting to a Media Services account located in the North China region</span></span>
<span data-ttu-id="73117-159">Ha a fiók az Észak-Kína régióban található, használja az alábbi konstruktort:</span><span class="sxs-lookup"><span data-stu-id="73117-159">If your account is located in the North China region, use the following constructor:</span></span>

    public CloudMediaContext(Uri apiServer, string accountName, string accountKey, string scope, string acsBaseAddress)

<span data-ttu-id="73117-160">Példa:</span><span class="sxs-lookup"><span data-stu-id="73117-160">For example:</span></span>

    _context = new CloudMediaContext(
        new Uri("https://wamsbjbclus001rest-hs.chinacloudapp.cn/API/"),
        _mediaServicesAccountName,
        _mediaServicesAccountKey,
        "urn:WindowsAzureMediaServices",
        "https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn");


## <a name="storing-connection-values-in-configuration"></a><span data-ttu-id="73117-161">A konfigurációban kapcsolat értékek tárolására</span><span class="sxs-lookup"><span data-stu-id="73117-161">Storing Connection Values in Configuration</span></span>
<span data-ttu-id="73117-162">Erősen ajánlott gyakorlat konfigurációban kapcsolat értékek, a fiók nevét és jelszavát, például különösen bizalmas értékek tárolására is.</span><span class="sxs-lookup"><span data-stu-id="73117-162">It is a highly recommended practice to store connection values, especially sensitive values such as your account name and password, in configuration.</span></span> <span data-ttu-id="73117-163">Azt is ajánlott gyakorlat bizalmas konfigurációs adatainak titkosításához.</span><span class="sxs-lookup"><span data-stu-id="73117-163">Also, it is a recommended practice to encrypt sensitive configuration data.</span></span> <span data-ttu-id="73117-164">A Windows titkosított fájlrendszer (EFS) használatával titkosíthatja a teljes konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="73117-164">You can encrypt the entire configuration file by using the Windows Encrypting File System (EFS).</span></span> <span data-ttu-id="73117-165">Ahhoz, hogy a fájl EFS, kattintson jobb gombbal a fájlra, válassza ki **tulajdonságok**, és a titkosítás engedélyezéséhez a **speciális** beállítások lapon.</span><span class="sxs-lookup"><span data-stu-id="73117-165">To enable EFS on a file, right-click the file, select **Properties**, and enable encryption in the **Advanced** settings tab.</span></span> <span data-ttu-id="73117-166">Vagy egy egyéni megoldás kijelölt részei egy konfigurációs fájl titkosítása védett konfigurációs használatával hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="73117-166">Or you can create a custom solution for encrypting selected portions of a configuration file by using protected configuration.</span></span> <span data-ttu-id="73117-167">Lásd: [titkosított konfigurációs információit védett konfigurációs](https://msdn.microsoft.com/library/53tyfkaw.aspx).</span><span class="sxs-lookup"><span data-stu-id="73117-167">See [Encrypting Configuration Information Using Protected Configuration](https://msdn.microsoft.com/library/53tyfkaw.aspx).</span></span>

<span data-ttu-id="73117-168">A következő App.config fájl kapcsolathoz szükséges értékeket tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="73117-168">The following App.config file contains the required connection values.</span></span> <span data-ttu-id="73117-169">Az értékek a <appSettings> elem, amely a Media Services-fiók telepítő folyamat során kapott azonosítóértékeket szükséges értékeket.</span><span class="sxs-lookup"><span data-stu-id="73117-169">The values in the <appSettings> element are the required values that you got from the Media Services account setup process.</span></span>

    <configuration>
      <appSettings>
        <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
        <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
      </appSettings>
    </configuration>


<span data-ttu-id="73117-170">Konfigurációs kapcsolati értékek lekéréséhez használja a **ConfigurationManager** osztályhoz, és hozzárendelheti a értékek mezők a kódban:</span><span class="sxs-lookup"><span data-stu-id="73117-170">To retrieve connection values from configuration, you can use the **ConfigurationManager** class and then assign the values to fields in your code:</span></span>

    private static readonly string _accountName = ConfigurationManager.AppSettings["MediaServicesAccountName"];
    private static readonly string _accountKey = ConfigurationManager.AppSettings["MediaServicesAccountKey"];



## <a name="media-services-learning-paths"></a><span data-ttu-id="73117-171">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="73117-171">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="73117-172">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="73117-172">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

