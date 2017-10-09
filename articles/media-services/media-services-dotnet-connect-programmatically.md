---
title: "aaaConnecting tooMedia Services-fiók segítségével .NET"
description: "Ez a témakör bemutatja, hogyan tooconnect tooMedia szolgáltatások előrejelzése .NET."
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
ms.openlocfilehash: a23bd285f7cae17ae5831e1e50e73947afbb9a3d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connecting-toomedia-services-account-using-media-services-sdk-for-net"></a><span data-ttu-id="742e8-103">Csatlakozás tooMedia Services-fiók .NET-keretrendszerhez készült Media Services SDK használatával</span><span class="sxs-lookup"><span data-stu-id="742e8-103">Connecting tooMedia Services Account using Media Services SDK for .NET</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="742e8-104">REST</span><span class="sxs-lookup"><span data-stu-id="742e8-104">REST</span></span>](media-services-rest-connect-programmatically.md)
> * [<span data-ttu-id="742e8-105">.NET</span><span class="sxs-lookup"><span data-stu-id="742e8-105">.NET</span></span>](media-services-dotnet-connect-programmatically.md)
> 
> 

<span data-ttu-id="742e8-106">Ez a témakör ismerteti, hogyan tooobtain egy Azure Media Services, amikor a rendszer programozás a szoftveres kapcsolatot tooMicrosoft hello .NET-keretrendszerhez készült Media Services SDK-t.</span><span class="sxs-lookup"><span data-stu-id="742e8-106">This topic describes how tooobtain a programmatic connection tooMicrosoft Azure Media Services when you are programming with hello Media Services SDK for .NET.</span></span>

## <a name="connecting-toomedia-services"></a><span data-ttu-id="742e8-107">Csatlakozás tooMedia szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="742e8-107">Connecting tooMedia Services</span></span>
<span data-ttu-id="742e8-108">tooconnect tooMedia szolgáltatások programozott módon, akkor kell korábban már beállított egy Azure-fiókra, beállította a Media Services a fiókban, és hozzon létre egy Visual Studio-projekt-val történő fejlesztés hello Media Services SDK for .NET.</span><span class="sxs-lookup"><span data-stu-id="742e8-108">tooconnect tooMedia Services programmatically, you must have previously set up an Azure account, configured Media Services on that account, and then set up a Visual Studio project for development with hello Media Services SDK for .NET.</span></span> <span data-ttu-id="742e8-109">További információ: fejlesztési telepítő hello Media Services SDK-t a .NET-keretrendszerhez készült.</span><span class="sxs-lookup"><span data-stu-id="742e8-109">For more information, see Setup for Development with hello Media Services SDK for .NET.</span></span>

<span data-ttu-id="742e8-110">Hello Media Services-fiók telepítési folyamat végén hello szükséges hello következő beszerzett kapcsolati értékek.</span><span class="sxs-lookup"><span data-stu-id="742e8-110">At hello end of hello Media Services account setup process, you obtained hello following required connection values.</span></span> <span data-ttu-id="742e8-111">Ezek toomake programozott kapcsolatokhoz tooMedia szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="742e8-111">Use these toomake programmatic connections tooMedia Services.</span></span>

* <span data-ttu-id="742e8-112">A Media Services-fiók neve.</span><span class="sxs-lookup"><span data-stu-id="742e8-112">Your Media Services account name.</span></span>
* <span data-ttu-id="742e8-113">A Media Services kulcsára.</span><span class="sxs-lookup"><span data-stu-id="742e8-113">Your Media Services account key.</span></span>

<span data-ttu-id="742e8-114">Ezek az értékek toofind toohello Azure Managmentet Portal nyissa, válassza ki a Media Services-fiókját, és kattintson az hello "**kulcsok kezelése**" hello portál ablakának alján hello ikonra.</span><span class="sxs-lookup"><span data-stu-id="742e8-114">toofind these values, go toohello Azure Managment Portal, select your Media Service account, and click on hello “**MANAGE KEYS**” icon on hello bottom of hello portal window.</span></span> <span data-ttu-id="742e8-115">Kattintson a hello ikon következő tooeach szöveg mezőben másolatok hello érték toohello rendszer vágólapjára.</span><span class="sxs-lookup"><span data-stu-id="742e8-115">Clicking on hello icon next tooeach text box copies hello value toohello system clipboard.</span></span>

## <a name="creating-a-cloudmediacontext-instance"></a><span data-ttu-id="742e8-116">CloudMediaContext példány létrehozása</span><span class="sxs-lookup"><span data-stu-id="742e8-116">Creating a CloudMediaContext Instance</span></span>
<span data-ttu-id="742e8-117">Media elleni programozási toocreate kell szolgáltatások toostart egy **CloudMediaContext** hello kiszolgálói környezetbe jelölő példány.</span><span class="sxs-lookup"><span data-stu-id="742e8-117">toostart programming against Media Services you need toocreate a **CloudMediaContext** instance that represents hello server context.</span></span> <span data-ttu-id="742e8-118">Hello **CloudMediaContext** magában foglalja többek között a feladatok, eszközök, fájlok, hozzáférési házirendek és keresők hivatkozások tooimportant gyűjtemények.</span><span class="sxs-lookup"><span data-stu-id="742e8-118">hello **CloudMediaContext** includes references tooimportant collections including jobs, assets, files, access policies, and locators.</span></span>

> [!NOTE]
> <span data-ttu-id="742e8-119">Hello **CloudMediaContext** osztály nincs többszálú futtatásra.</span><span class="sxs-lookup"><span data-stu-id="742e8-119">hello **CloudMediaContext** class is not thread safe.</span></span> <span data-ttu-id="742e8-120">Hozzon létre egy új CloudMediaContext szálankénti, vagy a műveletek.</span><span class="sxs-lookup"><span data-stu-id="742e8-120">You should create a new CloudMediaContext per thread or per set of operations.</span></span>
> 
> 

<span data-ttu-id="742e8-121">CloudMediaContext öt konstruktor túlterheléssel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="742e8-121">CloudMediaContext has five constructor overloads.</span></span> <span data-ttu-id="742e8-122">Ajánlott toouse konstruktorok használatát, amelyek már **MediaServicesCredentials** paraméterként.</span><span class="sxs-lookup"><span data-stu-id="742e8-122">It is recommended toouse constructors that take **MediaServicesCredentials** as a parameter.</span></span> <span data-ttu-id="742e8-123">További információkért lásd: hello **újból felhasználja a vezérlő szolgáltatás jogkivonatot** , amely a következő.</span><span class="sxs-lookup"><span data-stu-id="742e8-123">For more information, see hello **Reusing Access Control Service Tokens** that follows.</span></span> 

<span data-ttu-id="742e8-124">hello következő példában hello nyilvános CloudMediaContext(MediaServicesCredentials credentials) konstruktora:</span><span class="sxs-lookup"><span data-stu-id="742e8-124">hello following example uses hello public CloudMediaContext(MediaServicesCredentials credentials) constructor:</span></span>

    // _cachedCredentials and _context are class member variables. 
    _cachedCredentials = new MediaServicesCredentials(
                    _mediaServicesAccountName,
                    _mediaServicesAccountKey);

    _context = new CloudMediaContext(_cachedCredentials);


## <a name="reusing-access-control-service-tokens"></a><span data-ttu-id="742e8-125">Újbóli felhasználása a vezérlő szolgáltatás jogkivonatot</span><span class="sxs-lookup"><span data-stu-id="742e8-125">Reusing Access Control Service Tokens</span></span>
<span data-ttu-id="742e8-126">Ez a szakasz bemutatja, hogyan tooreuse hozzáférés-vezérlési szolgáltatásban jogkivonatok CloudMediaContext konstruktorok használatát, amelyek paraméterként MediaServicesCredentials használatával.</span><span class="sxs-lookup"><span data-stu-id="742e8-126">This section shows how tooreuse Access Control Service tokens by using CloudMediaContext constructors that take MediaServicesCredentials as a parameter.</span></span>

<span data-ttu-id="742e8-127">[Az Azure Active Directory hozzáférés-vezérlés](https://msdn.microsoft.com/library/hh147631.aspx) (más néven a hozzáférés-vezérlési szolgáltatásban vagy az ACS), amely lehetővé teszi a felhasználók toogain hozzáférés hitelesítése és engedélyező egyszerűen tootheir webes felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="742e8-127">[Azure Active Directory Access Control](https://msdn.microsoft.com/library/hh147631.aspx) (also known as Access Control Service or ACS) is a cloud-based service that provides an easy way of authenticating and authorizing users toogain access tootheir web applications.</span></span> <span data-ttu-id="742e8-128">A Microsoft Azure Media Services szabályozza a hozzáférést tooits szolgáltatások azonban OAuth protokollt igényel az ACS-jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="742e8-128">Microsoft Azure Media Services controls access tooits services though OAuth protocol that requires an ACS token.</span></span> <span data-ttu-id="742e8-129">A Media Services hello ACS-jogkivonatokat kap az engedélyezési kiszolgálóról.</span><span class="sxs-lookup"><span data-stu-id="742e8-129">Media Services receives hello ACS tokens from an authorization server.</span></span>

<span data-ttu-id="742e8-130">A Media Services SDK hello fejlesztésekor toonot üzlet hello jogkivonatokkal választhat, mert SDK kód kezelők hello meg őket.</span><span class="sxs-lookup"><span data-stu-id="742e8-130">When developing with hello Media Services SDK, you can choose toonot deal with hello tokens because hello SDK code managers them for you.</span></span> <span data-ttu-id="742e8-131">Azzal kapcsolatban hello SDK azonban teljes körű felügyeletéhez hello ACS jogkivonatok érdeklődők toounnecessary jogkivonat-kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="742e8-131">However, letting hello SDK fully manage hello ACS tokens leads toounnecessary token requests.</span></span> <span data-ttu-id="742e8-132">A kért jogkivonatok időt vesz igénybe, és hello ügyfél és kiszolgáló-erőforrásokat használ fel.</span><span class="sxs-lookup"><span data-stu-id="742e8-132">Requesting tokens takes time and consumes hello client and server resources.</span></span> <span data-ttu-id="742e8-133">Emellett hello ACS-kiszolgálónak azelőtt gyorsítja fel hello kérelmek Ha hello érték túl magas.</span><span class="sxs-lookup"><span data-stu-id="742e8-133">Also, hello ACS server throttles hello requests if hello rate is too high.</span></span> <span data-ttu-id="742e8-134">hello határérték 30 kérelmek / másodperc, lásd: [ACS Service korlátozásai](https://msdn.microsoft.com/library/gg185909.aspx) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="742e8-134">hello limit is 30 requests per second, see [ACS Service Limitations](https://msdn.microsoft.com/library/gg185909.aspx) for more details.</span></span>

<span data-ttu-id="742e8-135">Media Services SDK verzió 3.0.0.0 hello verziótól kezdődően hello ACS-jogkivonatokat is felhasználhatja.</span><span class="sxs-lookup"><span data-stu-id="742e8-135">Starting with hello Media Services SDK version 3.0.0.0, you can reuse hello ACS tokens.</span></span> <span data-ttu-id="742e8-136">Hello **CloudMediaContext** konstruktorok használatát, amelyek **MediaServicesCredentials** paraméterként engedélyezése a megosztási hello ACS jogkivonatok több környezet között.</span><span class="sxs-lookup"><span data-stu-id="742e8-136">hello **CloudMediaContext** constructors that take **MediaServicesCredentials** as a parameter enable sharing hello ACS tokens between multiple contexts.</span></span> <span data-ttu-id="742e8-137">hello MediaServicesCredentials osztály magában foglalja a hello Media Services hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="742e8-137">hello MediaServicesCredentials class encapsulates hello Media Services credentials.</span></span> <span data-ttu-id="742e8-138">Ha rendelkezésre áll az ACS-jogkivonatot, és a lejárati ideje ismert, hozzon létre egy új MediaServicesCredentials példányt hello jogkivonatot, és adja át a CloudMediaContext toohello konstruktor.</span><span class="sxs-lookup"><span data-stu-id="742e8-138">If an ACS token is available and its expiration time is known, you can create a new MediaServicesCredentials instance with hello token and pass it toohello constructor of CloudMediaContext.</span></span> <span data-ttu-id="742e8-139">Vegye figyelembe, hogy a Media Services SDK hello automatikusan frissíti a jogkivonatokat, amikor járnak.</span><span class="sxs-lookup"><span data-stu-id="742e8-139">Note that hello Media Services SDK automatically refreshes tokens whenever they expire.</span></span> <span data-ttu-id="742e8-140">Két módon tooreuse ACS jogkivonatokat, ahogy az alábbi példák hello.</span><span class="sxs-lookup"><span data-stu-id="742e8-140">There are two ways tooreuse ACS tokens, as shown in hello examples below.</span></span>

* <span data-ttu-id="742e8-141">Gyorsítótárazhatja a hello **MediaServicesCredentials** a memóriában (például a statikus osztály változóban) objektum.</span><span class="sxs-lookup"><span data-stu-id="742e8-141">You can cache hello **MediaServicesCredentials** object in memory (for example, in a static class variable).</span></span> <span data-ttu-id="742e8-142">Ezt követően adja át a gyorsítótárazott hello objektum toohello CloudMediaContext konstruktor.</span><span class="sxs-lookup"><span data-stu-id="742e8-142">Then, pass hello cached object toohello CloudMediaContext constructor.</span></span> <span data-ttu-id="742e8-143">hello MediaServicesCredentials objektum tartalmaz, amelyek felhasználhatók, ha továbbra is érvényes ACS-jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="742e8-143">hello MediaServicesCredentials object contains an ACS token that can be reused if it is still valid.</span></span> <span data-ttu-id="742e8-144">Ha hello jogkivonat nem érvényes, hello frissülnek a Media Services SDK hello hitelesítő adataival megadott toohello MediaServicesCredentials konstruktor.</span><span class="sxs-lookup"><span data-stu-id="742e8-144">If hello token is not valid, it will be refreshed by hello Media Services SDK using hello credentials given toohello MediaServicesCredentials constructor.</span></span>
  
    <span data-ttu-id="742e8-145">Vegye figyelembe, hogy hello **MediaServicesCredentials** objektum egy érvényes tokent kap hello RefreshToken neve után.</span><span class="sxs-lookup"><span data-stu-id="742e8-145">Note that hello **MediaServicesCredentials** object gets a valid token after hello RefreshToken is called.</span></span> <span data-ttu-id="742e8-146">Hello **CloudMediaContext** hívások hello **RefreshToken** hello konstruktor metódust.</span><span class="sxs-lookup"><span data-stu-id="742e8-146">hello **CloudMediaContext** calls hello **RefreshToken** method in hello constructor.</span></span> <span data-ttu-id="742e8-147">Ha azt tervezi, toosave hello lexikális elemek értékének tooan külső tárhelyen, hogy hello TokenExpiration érték érvényes hello token adatok mentése előtt győződjön meg arról, hogy toocheck.</span><span class="sxs-lookup"><span data-stu-id="742e8-147">If you are planning toosave hello token values tooan external storage, make sure toocheck whether hello TokenExpiration value is valid before saving hello token data.</span></span> <span data-ttu-id="742e8-148">Érvénytelen, hívjuk RefreshToken gyorsítótárazás előtt.</span><span class="sxs-lookup"><span data-stu-id="742e8-148">If it is not valid, call RefreshToken before caching.</span></span>
  
        // Create and cache hello Media Services credentials in a static class variable.
        _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);

        // Use hello cached credentials toocreate a new CloudMediaContext object.
        if(_cachedCredentials == null)
        {
            _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);
        }

        CloudMediaContext context = new CloudMediaContext(_cachedCredentials);

* <span data-ttu-id="742e8-149">Hello AccessToken karakterlánc és hello TokenExpiration értékeket is gyorsítótárazza.</span><span class="sxs-lookup"><span data-stu-id="742e8-149">You can also cache hello AccessToken string and hello TokenExpiration values.</span></span> <span data-ttu-id="742e8-150">hello értékek később lehet egy új MediaServicesCredentials objektum gyorsítótárazott hello token adatokkal használt toocreate.</span><span class="sxs-lookup"><span data-stu-id="742e8-150">hello values could later be used toocreate a new MediaServicesCredentials object with hello cached token data.</span></span>  <span data-ttu-id="742e8-151">Ez különösen fontos a forgatókönyvek, ahol hello token biztonságosan megoszthatók több folyamatok vagy számítógépek között.</span><span class="sxs-lookup"><span data-stu-id="742e8-151">This is especially useful for scenarios where hello token can be securely shared among multiple processes or computers.</span></span>
  
    <span data-ttu-id="742e8-152">hello alábbi kódtöredékek metódushívások hello SaveTokenDataToExternalStorage GetTokenDataFromExternalStorage és UpdateTokenDataInExternalStorageIfNeeded, amelyek nincsenek meghatározva ebben a példában.</span><span class="sxs-lookup"><span data-stu-id="742e8-152">hello following code snippets call hello SaveTokenDataToExternalStorage, GetTokenDataFromExternalStorage, and UpdateTokenDataInExternalStorageIfNeeded methods that are not defined in this example.</span></span> <span data-ttu-id="742e8-153">Ezen módszerek toostore, lekérése és token adatok frissítése sikerült megadása egy külső tárhelyen.</span><span class="sxs-lookup"><span data-stu-id="742e8-153">You could define these methods toostore, retrieve, and update token data in an external storage.</span></span> 
  
        CloudMediaContext context1 = new CloudMediaContext(_mediaServicesAccountName, _mediaServicesAccountKey);
  
        // Get token values from hello context.
        var accessToken = context1.Credentials.AccessToken;
        var tokenExpiration = context1.Credentials.TokenExpiration;
  
        // Save token values for later use. 
        // hello SaveTokenDataToExternalStorage method should check 
        // whether hello TokenExpiration value is valid before saving hello token data. 
        // If it is not valid, call MediaServicesCredentials’s RefreshToken before caching.
        SaveTokenDataToExternalStorage(accessToken, tokenExpiration);
  
    <span data-ttu-id="742e8-154">Mentett lexikális elemek értékének toocreate MediaServicesCredentials hello használata.</span><span class="sxs-lookup"><span data-stu-id="742e8-154">Use hello saved token values toocreate MediaServicesCredentials.</span></span>

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

    <span data-ttu-id="742e8-155">Hello token példány frissítése, abban az esetben hello token hello Media Services SDK frissítette.</span><span class="sxs-lookup"><span data-stu-id="742e8-155">Update hello token copy in case hello token was updated by hello Media Services SDK.</span></span> 

        if(tokenExpiration != context2.Credentials.TokenExpiration)
        {
            UpdateTokenDataInExternalStorageIfNeeded(accessToken, context2.Credentials.TokenExpiration);
        }


* <span data-ttu-id="742e8-156">Ha több Media Services-fiókot (például földrajzi terjesztési vagy célú terheléselosztási) gyorsítótárazhatja a MediaServicesCredentials objektumok hello System.Collections.Concurrent.ConcurrentDictionary gyűjtemény (hello használata ConcurrentDictionary gyűjtemény számoknak szálbiztos kulcs/érték párok elérhető szálak egyidejű).</span><span class="sxs-lookup"><span data-stu-id="742e8-156">If you have multiple Media Services accounts (for example, for load sharing purposes or Geo-distribution) you can cache MediaServicesCredentials objects using hello System.Collections.Concurrent.ConcurrentDictionary collection (hello ConcurrentDictionary collection represents a thread-safe collection of key/value pairs that can be accessed by multiple threads concurrently).</span></span> <span data-ttu-id="742e8-157">Ezután használhatja a hello GetOrAdd metódus tooget hello gyorsítótárazott hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="742e8-157">You can then use hello GetOrAdd method tooget hello cached credentials.</span></span> 
  
        // Declare a static class variable of hello ConcurrentDictionary type in which hello Media Services credentials will be cached.  
        private static readonly ConcurrentDictionary<string, MediaServicesCredentials> mediaServicesCredentialsCache = 
            new ConcurrentDictionary<string, MediaServicesCredentials>();

        // Cache (or get already cached) Media Services credentials. Use these credentials toocreate a new CloudMediaContext object.
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

## <a name="connecting-tooa-media-services-account-located-in-hello-north-china-region"></a><span data-ttu-id="742e8-158">Csatlakozás tooa Media Services-fiók hello Észak Kína régióban található</span><span class="sxs-lookup"><span data-stu-id="742e8-158">Connecting tooa Media Services account located in hello North China region</span></span>
<span data-ttu-id="742e8-159">Ha a fiók hello Észak Kína régióban található, használja a hello konstruktor a következő:</span><span class="sxs-lookup"><span data-stu-id="742e8-159">If your account is located in hello North China region, use hello following constructor:</span></span>

    public CloudMediaContext(Uri apiServer, string accountName, string accountKey, string scope, string acsBaseAddress)

<span data-ttu-id="742e8-160">Példa:</span><span class="sxs-lookup"><span data-stu-id="742e8-160">For example:</span></span>

    _context = new CloudMediaContext(
        new Uri("https://wamsbjbclus001rest-hs.chinacloudapp.cn/API/"),
        _mediaServicesAccountName,
        _mediaServicesAccountKey,
        "urn:WindowsAzureMediaServices",
        "https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn");


## <a name="storing-connection-values-in-configuration"></a><span data-ttu-id="742e8-161">A konfigurációban kapcsolat értékek tárolására</span><span class="sxs-lookup"><span data-stu-id="742e8-161">Storing Connection Values in Configuration</span></span>
<span data-ttu-id="742e8-162">Egy erősen ajánlott gyakorlat toostore kapcsolati értékek, különösen bizalmas értékeket, például a fiók nevét és jelszavát, a konfigurációban is.</span><span class="sxs-lookup"><span data-stu-id="742e8-162">It is a highly recommended practice toostore connection values, especially sensitive values such as your account name and password, in configuration.</span></span> <span data-ttu-id="742e8-163">Azt is egy ajánlott gyakorlat tooencrypt bizalmas konfigurációs adatokat.</span><span class="sxs-lookup"><span data-stu-id="742e8-163">Also, it is a recommended practice tooencrypt sensitive configuration data.</span></span> <span data-ttu-id="742e8-164">Hello Windows titkosított fájlrendszer (EFS) használatával titkosíthatja hello teljes konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="742e8-164">You can encrypt hello entire configuration file by using hello Windows Encrypting File System (EFS).</span></span> <span data-ttu-id="742e8-165">Válasszon egy fájlt, kattintson a jobb gombbal hello fájlt, a titkosított fájlrendszer tooenable **tulajdonságok**, és engedélyezheti a titkosítást a hello **speciális** beállítások lapon. Vagy egy egyéni megoldás kijelölt részei egy konfigurációs fájl titkosítása védett konfigurációs használatával hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="742e8-165">tooenable EFS on a file, right-click hello file, select **Properties**, and enable encryption in hello **Advanced** settings tab. Or you can create a custom solution for encrypting selected portions of a configuration file by using protected configuration.</span></span> <span data-ttu-id="742e8-166">Lásd: [titkosított konfigurációs információit védett konfigurációs](https://msdn.microsoft.com/library/53tyfkaw.aspx).</span><span class="sxs-lookup"><span data-stu-id="742e8-166">See [Encrypting Configuration Information Using Protected Configuration](https://msdn.microsoft.com/library/53tyfkaw.aspx).</span></span>

<span data-ttu-id="742e8-167">következő App.config fájl hello szükséges hello kapcsolat értékeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="742e8-167">hello following App.config file contains hello required connection values.</span></span> <span data-ttu-id="742e8-168">hello értékek hello <appSettings> elem hello Media Services-fiók telepítő folyamat során kapott azonosítóértékeket hello szükséges értékeket.</span><span class="sxs-lookup"><span data-stu-id="742e8-168">hello values in hello <appSettings> element are hello required values that you got from hello Media Services account setup process.</span></span>

    <configuration>
      <appSettings>
        <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
        <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
      </appSettings>
    </configuration>


<span data-ttu-id="742e8-169">konfigurációs tooretrieve kapcsolat értékeit, hello használható **ConfigurationManager** osztály, és hozzárendelheti a hello értékek toofields a kódban:</span><span class="sxs-lookup"><span data-stu-id="742e8-169">tooretrieve connection values from configuration, you can use hello **ConfigurationManager** class and then assign hello values toofields in your code:</span></span>

    private static readonly string _accountName = ConfigurationManager.AppSettings["MediaServicesAccountName"];
    private static readonly string _accountKey = ConfigurationManager.AppSettings["MediaServicesAccountKey"];



## <a name="media-services-learning-paths"></a><span data-ttu-id="742e8-170">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="742e8-170">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="742e8-171">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="742e8-171">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

