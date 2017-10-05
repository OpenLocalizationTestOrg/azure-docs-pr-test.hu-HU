---
title: "Egyéni gyorsítótárazása az Azure API Management"
description: "Megtudhatja, hogyan gyorsítótárazza a cikkek gombot az Azure API Management"
services: api-management
documentationcenter: 
author: darrelmiller
manager: erikre
editor: 
ms.assetid: 772bc8dd-5cda-41c4-95bf-b9f6f052bc85
ms.service: api-management
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: f5d5f44e34fbcd122a10be0ca5b3001760c4c64d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="custom-caching-in-azure-api-management"></a><span data-ttu-id="bae47-103">Egyéni gyorsítótárazása az Azure API Management</span><span class="sxs-lookup"><span data-stu-id="bae47-103">Custom caching in Azure API Management</span></span>
<span data-ttu-id="bae47-104">Az Azure API Management szolgáltatás rendelkezik beépített támogatása [HTTP-válasz gyorsítótár](api-management-howto-cache.md) kulcsa a forrás URL-cím használatával.</span><span class="sxs-lookup"><span data-stu-id="bae47-104">Azure API Management service has built-in support for [HTTP response caching](api-management-howto-cache.md) using the resource URL as the key.</span></span> <span data-ttu-id="bae47-105">A kulcs használatával kérelemfejléc módosíthatják a `vary-by` tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="bae47-105">The key can be modified by request headers using the `vary-by` properties.</span></span> <span data-ttu-id="bae47-106">Ez akkor hasznos, a teljes HTTP-válaszok (más néven felelősséget) gyorsítótárazáshoz, de egyes esetekben célszerű csak gyorsítótár létrehozása egy részét.</span><span class="sxs-lookup"><span data-stu-id="bae47-106">This is useful for caching entire HTTP responses (aka representations), but sometimes it is useful to just cache a portion of a representation.</span></span> <span data-ttu-id="bae47-107">Az új [gyorsítótár-keresési-érték](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) és [gyorsítótár-tároló-érték](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) -szabályzatok lehetőséget biztosítanak tárolásához és lekéréséhez belül a házirend-definíciók adatait tetszőleges darabjait képes.</span><span class="sxs-lookup"><span data-stu-id="bae47-107">The new [cache-lookup-value](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) and [cache-store-value](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) policies provide the ability to store and retrieve arbitrary pieces of data from within policy definitions.</span></span> <span data-ttu-id="bae47-108">Ez a lehetőség is értéket ad hozzá a korábban bevezetett [küldési-kérelmek](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) házirend mivel most gyorsítótárazhatja a válaszok külső szolgáltatásokból.</span><span class="sxs-lookup"><span data-stu-id="bae47-108">This ability also adds value to the previously introduced [send-request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) policy because you can now cache responses from external services.</span></span>

## <a name="architecture"></a><span data-ttu-id="bae47-109">Architektúra</span><span class="sxs-lookup"><span data-stu-id="bae47-109">Architecture</span></span>
<span data-ttu-id="bae47-110">API-kezelés szolgáltatást használ egy megosztott / bérlői adatgyorsítótár, hogy az, akár méretezhető, továbbra is elérhetővé válik a ugyanaz a hozzáférést, a több egység gyorsítótárazott adatokat.</span><span class="sxs-lookup"><span data-stu-id="bae47-110">API Management service uses a shared per-tenant data cache so that, as you scale up to multiple units you will still get access to the same cached data.</span></span> <span data-ttu-id="bae47-111">Azonban több területi telepítés használatakor vannak független gyorsítótárak belül régióban.</span><span class="sxs-lookup"><span data-stu-id="bae47-111">However, when working with a multi-region deployment there are independent caches within each of the regions.</span></span> <span data-ttu-id="bae47-112">Emiatt fontos a gyorsítótár nem tekinti a tárolóban, néhány adat, csak forrását.</span><span class="sxs-lookup"><span data-stu-id="bae47-112">Due to this, it is important to not treat the cache as a data store, where it is the only source of some piece of information.</span></span> <span data-ttu-id="bae47-113">Ha volt, és később úgy döntött, hogy a több területi telepítési előnyeit, majd haladnak, akik rendelkező ügyfelek is elveszti hozzáférését, hogy a gyorsítótárazott adatokat.</span><span class="sxs-lookup"><span data-stu-id="bae47-113">If you did, and later decided to take advantage of the multi-region deployment, then customers with users that travel may lose access to that cached data.</span></span>

## <a name="fragment-caching"></a><span data-ttu-id="bae47-114">Töredék gyorsítótárazása</span><span class="sxs-lookup"><span data-stu-id="bae47-114">Fragment caching</span></span>
<span data-ttu-id="bae47-115">Nincsenek bizonyos esetekben, ha a válaszokat ad vissza, olcsóbbá teszi az határozza meg, és még marad friss elfogadható időn adatok egy részét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="bae47-115">There are certain cases where responses being returned contain some portion of data that is expensive to determine and yet remains fresh for a reasonable amount of time.</span></span> <span data-ttu-id="bae47-116">Tegyük fel fontolja meg egy szolgáltatás, amely repülési foglalásokat, repülési állapot stb vonatkozó információkat biztosító légitársaság. Ha a felhasználó tagja a légitársaság pontok program, akkor az aktuális állapot és halmozott távolság vonatkozó információkat is.</span><span class="sxs-lookup"><span data-stu-id="bae47-116">As an example, consider a service built by an airline that provides information relating flight reservations, flight status, etc. If the user is a member of the airlines points program, they would also have information relating to their current status and mileage accumulated.</span></span> <span data-ttu-id="bae47-117">Eltérő tárolódhat, a felhasználóval kapcsolatos adatokat, de lehet foglalja azt repülési állapotáról és foglalások adott vissza.</span><span class="sxs-lookup"><span data-stu-id="bae47-117">This user-related information might be stored in a different system, but it may be desirable to include it in responses returned about flight status and reservations.</span></span> <span data-ttu-id="bae47-118">Ezt megteheti egy zónaaláírásnak nevezett töredék gyorsítótárazását.</span><span class="sxs-lookup"><span data-stu-id="bae47-118">This can be done using a process called fragment caching.</span></span> <span data-ttu-id="bae47-119">Az elsődleges ábrázolását adhatók vissza a forráskiszolgálóról, és jelzi, ahol a felhasználóval kapcsolatos adatokat beszúrni valamilyen token használatával.</span><span class="sxs-lookup"><span data-stu-id="bae47-119">The primary representation can be returned from the origin server using some kind of token to indicate where the user-related information is to be inserted.</span></span> 

<span data-ttu-id="bae47-120">Vegye figyelembe a következő JSON-válasz egy API-háttérrendszerből.</span><span class="sxs-lookup"><span data-stu-id="bae47-120">Consider the following JSON response from a backend API.</span></span>

```json
{
  "airline" : "Air Canada",
  "flightno" : "871",
  "status" : "ontime",
  "gate" : "B40",
  "terminal" : "2A",
  "userprofile" : "$userprofile$"
}  
```

<span data-ttu-id="bae47-121">És a másodlagos helyen erőforrás `/userprofile/{userid}` láthatóhoz hasonló,</span><span class="sxs-lookup"><span data-stu-id="bae47-121">And secondary resource at `/userprofile/{userid}` that looks like,</span></span>

```json
{ "username" : "Bob Smith", "Status" : "Gold" }
```

<span data-ttu-id="bae47-122">Annak meghatározására, a megfelelő felhasználói adatok közé tartoznak, igazolnia kell a felhasználó, aki azonosításához.</span><span class="sxs-lookup"><span data-stu-id="bae47-122">In order to determine the appropriate user information to include, we need to identify who the end user is.</span></span> <span data-ttu-id="bae47-123">Ez a módszer használata függő végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="bae47-123">This mechanism is implementation dependent.</span></span> <span data-ttu-id="bae47-124">Tegyük fel, használom a `Subject` a jogcím egy `JWT` token.</span><span class="sxs-lookup"><span data-stu-id="bae47-124">As an example, I am using the `Subject` claim of a `JWT` token.</span></span> 

```xml
<set-variable
  name="enduserid"
  value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />
```

<span data-ttu-id="bae47-125">Ez tároljuk `enduserid` későbbi használatra környezeti változó értékét.</span><span class="sxs-lookup"><span data-stu-id="bae47-125">We store this `enduserid` value in a context variable for later use.</span></span> <span data-ttu-id="bae47-126">A következő lépés annak határozzák meg, ha már rendelkezik-e a felhasználói adatok lekérése a korábbi kérelmekre, és a gyorsítótárban tárolt.</span><span class="sxs-lookup"><span data-stu-id="bae47-126">The next step is to determine if a previous request has already retrieved the user information and stored it in the cache.</span></span> <span data-ttu-id="bae47-127">A használjuk a `cache-lookup-value` házirend.</span><span class="sxs-lookup"><span data-stu-id="bae47-127">For this we use the `cache-lookup-value` policy.</span></span>

```xml
<cache-lookup-value
key="@("userprofile-" + context.Variables["enduserid"])"
variable-name="userprofile" />
```

<span data-ttu-id="bae47-128">Ha nem található bejegyzés a gyorsítótár, a kulcs értékét, akkor a nem megfelelő a `userprofile` környezeti változó jön létre.</span><span class="sxs-lookup"><span data-stu-id="bae47-128">If there is no entry in the cache that corresponds to the key value, then no `userprofile` context variable will be created.</span></span> <span data-ttu-id="bae47-129">A sikeres a keresési használatának ellenőrizzük a `choose` adatfolyam házirend szabályozza.</span><span class="sxs-lookup"><span data-stu-id="bae47-129">We check the success of the lookup using the `choose` control flow policy.</span></span>

```xml
<choose>
    <when condition="@(!context.Variables.ContainsKey("userprofile"))">
        <!-- If the userprofile context variable doesn’t exist, make an HTTP request to retrieve it.  -->
    </when>
</choose>
```

<span data-ttu-id="bae47-130">Ha a `userprofile` környezeti változó nem létezik, akkor el kell végeznie egy HTTP-kérelem lekéréséhez fogjuk.</span><span class="sxs-lookup"><span data-stu-id="bae47-130">If the `userprofile` context variable doesn’t exist, then we are going to have to make an HTTP request to retrieve it.</span></span>

```xml
<send-request
  mode="new"
  response-variable-name="userprofileresponse"
  timeout="10"
  ignore-error="true">

  <!-- Build a URL that points to the profile for the current end-user -->
  <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),
      (string)context.Variables["enduserid"]).AbsoluteUri)
  </set-url>
  <set-method>GET</set-method>
</send-request>
```

<span data-ttu-id="bae47-131">Használjuk a `enduserid` összeállítani a felhasználói profil erőforrás URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="bae47-131">We use the `enduserid` to construct the URL to the user profile resource.</span></span> <span data-ttu-id="bae47-132">Amennyiben van a válasz, azt lekéréses kívül a válasz szövegét, és újra üzembe a környezeti változó tárolja.</span><span class="sxs-lookup"><span data-stu-id="bae47-132">Once we have the response, we can pull the body text out of the response and store it back into a context variable.</span></span>

```xml
<set-variable
    name="userprofile"
    value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />
```

<span data-ttu-id="bae47-133">Velünk, hogy végezze el a HTTP-kérelem újra, ha ugyanaz a felhasználó egy másik kérést kellene elkerüléséhez tároljuk a gyorsítótárban a felhasználói profilt.</span><span class="sxs-lookup"><span data-stu-id="bae47-133">To avoid us having to make this HTTP request again, when the same user makes another request, we can store the user profile in the cache.</span></span>

```xml
<cache-store-value
    key="@("userprofile-" + context.Variables["enduserid"])"
    value="@((string)context.Variables["userprofile"])" duration="100000" />
```

<span data-ttu-id="bae47-134">Az érték, amely azt eredetileg megpróbálta beolvasni a pontos azonos kulcsot használva gyorsítótárában tároljuk.</span><span class="sxs-lookup"><span data-stu-id="bae47-134">We store the value in the cache using the exact same key that we originally attempted to retrieve it with.</span></span> <span data-ttu-id="bae47-135">Az érték tárolására választjuk időtartam alapján hogyan gyakran a változtatások és a felhasználók hogyan hibatűrő történik az elavult adatokat.</span><span class="sxs-lookup"><span data-stu-id="bae47-135">The duration that we choose to store the value should be based on how often the information changes and how tolerant users are to out of date information.</span></span> 

<span data-ttu-id="bae47-136">Fontos, hogy a gyorsítótár lekérdezése még mindig egy folyamaton kívül, a hálózati kérelmek, és potenciálisan továbbra is felvehetőek több tíz ideje (MS) kérésre megvalósításához.</span><span class="sxs-lookup"><span data-stu-id="bae47-136">It is important to realize that retrieving from the cache is still an out-of-process, network request and potentially can still add tens of milliseconds to the request.</span></span> <span data-ttu-id="bae47-137">A következő előnyöket határozza meg a felhasználói profillal kapcsolatos információk, amelyek miatt az adatbázis-lekérdezések vagy több biztonsági-végpontok összesített adatait kellene számottevően hosszabb időbe telik meghatározásakor.</span><span class="sxs-lookup"><span data-stu-id="bae47-137">The benefits come when determining the user profile information takes significantly longer than that due to needing to do database queries or aggregate information from multiple back-ends.</span></span>

<span data-ttu-id="bae47-138">A folyamat az utolsó lépés a felhasználói profil információkkal frissítenie a visszaadott válaszban.</span><span class="sxs-lookup"><span data-stu-id="bae47-138">The final step in the process is to update the returned response with our user profile information.</span></span>

```xml
<!-- Update response body with user profile-->
<find-and-replace
    from='"$userprofile$"'
    to="@((string)context.Variables["userprofile"])" />
```

<span data-ttu-id="bae47-139">Az idézőjelek a jogkivonat részeként, hogy akkor is, ha nem történik, a csere, a válasz volt-e továbbra is érvényes JSON elfogadása.</span><span class="sxs-lookup"><span data-stu-id="bae47-139">I chose to include the quotation marks as part of the token so that even when the replace doesn’t occur, the response was still valid JSON.</span></span> <span data-ttu-id="bae47-140">Ez elsősorban hibakeresési könnyebb annak volt.</span><span class="sxs-lookup"><span data-stu-id="bae47-140">This was primarily to make debugging easier.</span></span>

<span data-ttu-id="bae47-141">Miután együtt egyesíteni ezeket a lépéseket, záró eredménye egy házirendet a következőhöz hasonló a következő egy.</span><span class="sxs-lookup"><span data-stu-id="bae47-141">Once you combine all these steps together, the end result is a policy that looks like the following one.</span></span>

```xml
<policies>
    <inbound>
        <!-- How you determine user identity is application dependent -->
        <set-variable
          name="enduserid"
          value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

        <!--Look for userprofile for this user in the cache -->
        <cache-lookup-value
          key="@("userprofile-" + context.Variables["enduserid"])"
          variable-name="userprofile" />

        <!-- If we don’t find it in the cache, make a request for it and store it -->
        <choose>
            <when condition="@(!context.Variables.ContainsKey("userprofile"))">
                <!-- Make HTTP request to get user profile -->
                <send-request
                  mode="new"
                  response-variable-name="userprofileresponse"
                  timeout="10"
                  ignore-error="true">

                   <!-- Build a URL that points to the profile for the current end-user -->
                    <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),(string)context.Variables["enduserid"]).AbsoluteUri)</set-url>
                    <set-method>GET</set-method>
                </send-request>

                <!-- Store response body in context variable -->
                <set-variable
                  name="userprofile"
                  value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />

                <!-- Store result in cache -->
                <cache-store-value
                  key="@("userprofile-" + context.Variables["enduserid"])"
                  value="@((string)context.Variables["userprofile"])"
                  duration="100000" />
            </when>
        </choose>
        <base />
    </inbound>
    <outbound>
        <!-- Update response body with user profile-->
        <find-and-replace
              from='"$userprofile$"'
              to="@((string)context.Variables["userprofile"])" />
        <base />
    </outbound>
</policies>
```

<span data-ttu-id="bae47-142">Ez a megközelítés gyorsítótárazási elsősorban a webhelyeken ahol HTML jön létre a kiszolgáló oldalán, hogy egyetlen lapként megjeleníthetők.</span><span class="sxs-lookup"><span data-stu-id="bae47-142">This caching approach is primarily used in web sites where HTML is composed on the server side so that it can be rendered as a single page.</span></span> <span data-ttu-id="bae47-143">Azonban ez is hasznos lehet az API-k, ahol az ügyfelek nem ügyfél oldalán található HTTP-gyorsítótárazás, vagy nem kell elhelyezni, amely felelős az ügyfélen.</span><span class="sxs-lookup"><span data-stu-id="bae47-143">However, it can also be useful in APIs where clients cannot do client side HTTP caching or it is desirable not to put that responsibility on the client.</span></span>

<span data-ttu-id="bae47-144">A gyorsítótárazás töredék ugyanolyan típusú is elvégezhető a háttér-webkiszolgálók, a Redis gyorsítótár-kiszolgáló használatával, azonban a munkájuk elvégzéséhez az API Management szolgáltatással akkor hasznos, ha a gyorsítótárazott töredék érkező különböző biztonsági ends mint az elsődleges válaszokat.</span><span class="sxs-lookup"><span data-stu-id="bae47-144">This same kind of fragment caching can also be done on the backend web servers using a Redis caching server, however, using the API Management service to perform this work is useful when the cached fragments are coming from different back-ends than the primary responses.</span></span>

## <a name="transparent-versioning"></a><span data-ttu-id="bae47-145">Transzparens versioning</span><span class="sxs-lookup"><span data-stu-id="bae47-145">Transparent versioning</span></span>
<span data-ttu-id="bae47-146">Általános gyakorlat egy API-t, így egyszerre nem támogatott több különböző végrehajtási verziója.</span><span class="sxs-lookup"><span data-stu-id="bae47-146">It is common practice for multiple different implementation versions of an API to be supported at any one time.</span></span> <span data-ttu-id="bae47-147">Ez lehet, hogy az fejlesztői, tesztelési, éles stb, a különböző környezetek támogatására, vagy lehet támogatására az API-t, hogy API fogyasztók újabb verziókra történő áttelepítéséhez a régebbi verzióit.</span><span class="sxs-lookup"><span data-stu-id="bae47-147">This is perhaps to support different environments, like dev, test, production, etc, or it may be to support older versions of the API to give time for API consumers to migrate to newer versions.</span></span> 

<span data-ttu-id="bae47-148">Ahelyett, hogy az ügyfél fejlesztők számára, hogy módosítsa az URL-címei kezelnek ez egyik módszer `/v1/customers` való `/v2/customers` jelenleg kívánnak használni, és hívja meg a megfelelő háttérkiszolgáló URL-címet a API verziójának a felhasználói profil adatok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="bae47-148">One approach to handling this instead of requiring client developers to change the URLs from `/v1/customers` to `/v2/customers` is to store in the consumer’s profile data which version of the API they currently wish to use and call the appropriate backend URL.</span></span> <span data-ttu-id="bae47-149">Annak meghatározására, a megfelelő háttér URL-címet az adott ügyfél hívja, fontos bizonyos konfigurációs adatait.</span><span class="sxs-lookup"><span data-stu-id="bae47-149">In order to determine the correct backend URL to call for a particular client, it is necessary to query some configuration data.</span></span> <span data-ttu-id="bae47-150">A konfigurációs adatok gyorsítótárazása, azt is lecsökkentheti a teljesítményét, ez a keresés során.</span><span class="sxs-lookup"><span data-stu-id="bae47-150">By caching this configuration data, we can minimize the performance penalty of doing this lookup.</span></span>

<span data-ttu-id="bae47-151">Az első lépés, hogy a kívánt verziójával konfigurálásához használt azonosító meghatározása.</span><span class="sxs-lookup"><span data-stu-id="bae47-151">The first step is to determine the identifier used to configure the desired version.</span></span> <span data-ttu-id="bae47-152">Ebben a példában a verziót, hogy a termékkulcs előfizetés társítása elfogadása.</span><span class="sxs-lookup"><span data-stu-id="bae47-152">In this example, I chose to associate the version to the product subscription key.</span></span> 

```xml
<set-variable name="clientid" value="@(context.Subscription.Key)" />
```

<span data-ttu-id="bae47-153">Majd végezzük a gyorsítótárban való kereséshez megjelenítéséhez, ha azt már rendelkezik olvassa be a kívánt ügyfél verziója.</span><span class="sxs-lookup"><span data-stu-id="bae47-153">We then do a cache lookup to see if we already have retrieved the desired client version.</span></span>

```xml
<cache-lookup-value
key="@("clientversion-" + context.Variables["clientid"])"
variable-name="clientversion" />
```

<span data-ttu-id="bae47-154">Jelenleg így ellenőrizheti, ha azt nem találta azt a gyorsítótárban.</span><span class="sxs-lookup"><span data-stu-id="bae47-154">Then we check to see if we did not find it in the cache.</span></span>

```xml
<choose>
    <when condition="@(!context.Variables.ContainsKey("clientversion"))">
```

<span data-ttu-id="bae47-155">Ha azt nem, akkor nyissa meg azt, és lekéréséhez.</span><span class="sxs-lookup"><span data-stu-id="bae47-155">If we didn’t then we go and retrieve it.</span></span>

```xml
<send-request
    mode="new"
    response-variable-name="clientconfiguresponse"
    timeout="10"
    ignore-error="true">
            <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
            <set-method>GET</set-method>
</send-request>
```

<span data-ttu-id="bae47-156">A válasz törzsében szöveg kinyerése a választ.</span><span class="sxs-lookup"><span data-stu-id="bae47-156">Extract the response body text from the response.</span></span>

```xml
<set-variable
      name="clientversion"
      value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />
```

<span data-ttu-id="bae47-157">Vissza tárolása a későbbi használatra a gyorsítótárban.</span><span class="sxs-lookup"><span data-stu-id="bae47-157">Store it back in the cache for future use.</span></span>

```xml
<cache-store-value
      key="@("clientversion-" + context.Variables["clientid"])"
      value="@((string)context.Variables["clientversion"])"
      duration="100000" />
```

<span data-ttu-id="bae47-158">És végül frissítse a háttér-URL-címet, válassza ki a szolgáltatást, az ügyfél által igényelt verziót.</span><span class="sxs-lookup"><span data-stu-id="bae47-158">And finally update the back-end URL to select the version of the service desired by the client.</span></span>

```xml
<set-backend-service
      base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
```

<span data-ttu-id="bae47-159">A teljesen házirendet a következőképpen történik.</span><span class="sxs-lookup"><span data-stu-id="bae47-159">The completely policy is as follows.</span></span>

```xml
<inbound>
    <base />
    <set-variable name="clientid" value="@(context.Subscription.Key)" />
    <cache-lookup-value key="@("clientversion-" + context.Variables["clientid"])" variable-name="clientversion" />

    <!-- If we don’t find it in the cache, make a request for it and store it -->
    <choose>
        <when condition="@(!context.Variables.ContainsKey("clientversion"))">
            <send-request mode="new" response-variable-name="clientconfiguresponse" timeout="10" ignore-error="true">
                <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
                <set-method>GET</set-method>
            </send-request>
            <!-- Store response body in context variable -->
            <set-variable name="clientversion" value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />
            <!-- Store result in cache -->
            <cache-store-value key="@("clientversion-" + context.Variables["clientid"])" value="@((string)context.Variables["clientversion"])" duration="100000" />
        </when>
    </choose>
    <set-backend-service base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
</inbound>
```

<span data-ttu-id="bae47-160">Sok API versioning problémákat orvosló elegáns megoldás API fogyasztói transzparens módon a háttérrendszer verziószámának ügyfelek anélkül, hogy frissítse és telepítse újra az ügyfelek is hozzáférnek vezérlésére.</span><span class="sxs-lookup"><span data-stu-id="bae47-160">Enabling API consumers to transparently control which backend version is being accessed by clients without having to update and redeploy clients is a elegant solution that addresses many API versioning concerns.</span></span>

## <a name="tenant-isolation"></a><span data-ttu-id="bae47-161">Bérlők elszigetelésére</span><span class="sxs-lookup"><span data-stu-id="bae47-161">Tenant Isolation</span></span>
<span data-ttu-id="bae47-162">A nagyobb, a több-bérlős telepítésekben egyes vállalatok bérlők külön csoportot létre háttér hardver különböző üzemelő példányok esetében.</span><span class="sxs-lookup"><span data-stu-id="bae47-162">In larger, multi-tenant deployments some companies create separate groups of tenants on distinct deployments of backend hardware.</span></span> <span data-ttu-id="bae47-163">Ez minimalizálja a háttérkiszolgálón a hardver probléma által érintett felhasználók száma.</span><span class="sxs-lookup"><span data-stu-id="bae47-163">This minimizes the number of customers who are impacted by a hardware issue on the backend.</span></span> <span data-ttu-id="bae47-164">Emellett lehetővé teszi az új szoftververziók való szakaszban állítható.</span><span class="sxs-lookup"><span data-stu-id="bae47-164">It also enables new software versions to be rolled out in stages.</span></span> <span data-ttu-id="bae47-165">Ideális esetben a háttér-architektúra API-felhasználók számára átlátható legyen.</span><span class="sxs-lookup"><span data-stu-id="bae47-165">Ideally this backend architecture should be transparent to API consumers.</span></span> <span data-ttu-id="bae47-166">Ez érhető el a transzparens versioning hasonló módon, mert a háttérkiszolgáló URL-CÍMÉT egy API-kulcs konfiguráció állapota kezelésére ugyanaz a technika alapul.</span><span class="sxs-lookup"><span data-stu-id="bae47-166">This can be achieved in a similar way to transparent versioning because it is based on the same technique of manipulating the backend URL using configuration state per API key.</span></span>  

<span data-ttu-id="bae47-167">Helyett az API a minden előfizetés kulcs egy előnyben részesített verziója, az azonosítója, hogy a bérlő vonatkozik, a hozzárendelt hardvercsoport alakítanák vissza.</span><span class="sxs-lookup"><span data-stu-id="bae47-167">Instead of returning a preferred version of the API for each subscription key, you would return an identifier that relates a tenant to the assigned hardware group.</span></span> <span data-ttu-id="bae47-168">Ilyen azonosítójú segítségével hozható létre a megfelelő háttérkiszolgáló URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="bae47-168">That identifier can be used to construct the appropriate backend URL.</span></span>

## <a name="summary"></a><span data-ttu-id="bae47-169">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="bae47-169">Summary</span></span>
<span data-ttu-id="bae47-170">Szabadon használható az Azure API management gyorsítótár bármilyen típusú adat tárolásához lehetővé teszi, hogy a konfigurációs adatokat, amelyek hatással lehetnek a módszer egy bejövő kérelem feldolgozását hatékonyabbá teszi a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="bae47-170">The freedom to use the Azure API management cache for storing any kind of data enables efficient access to configuration data that can affect the way an inbound request is processed.</span></span> <span data-ttu-id="bae47-171">Is használható, amelyek is kiegészítheti a válaszokat, egy háttér-API által visszaadott adatok töredék tárolásához.</span><span class="sxs-lookup"><span data-stu-id="bae47-171">It can also be used to store data fragments that can augment responses, returned from a backend API.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bae47-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bae47-172">Next steps</span></span>
<span data-ttu-id="bae47-173">Adja meg a visszajelzést a disqus-beszélgetésben teheti szál a témakör Ha egyéb forgatókönyvek, ezek a házirendek, amelyeken engedélyezve van, vagy ha vannak forgatókönyvek szeretné-e elérése, de nem látja a jelenleg lehetségesek.</span><span class="sxs-lookup"><span data-stu-id="bae47-173">Please give us your feedback in the Disqus thread for this topic if there are other scenarios that these policies have enabled for you, or if there are scenarios you would like to achieve but do not feel are currently possible.</span></span>

