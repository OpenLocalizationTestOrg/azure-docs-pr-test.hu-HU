---
title: "aaaCustom gyorsítótárazása az Azure API Management"
description: Ismerje meg, hogyan toocache elemek gombot az Azure API Management
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
ms.openlocfilehash: 681380743c8c96af5d0a8e25948a8c0663e9fd35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="custom-caching-in-azure-api-management"></a><span data-ttu-id="490f7-103">Egyéni gyorsítótárazása az Azure API Management</span><span class="sxs-lookup"><span data-stu-id="490f7-103">Custom caching in Azure API Management</span></span>
<span data-ttu-id="490f7-104">Az Azure API Management szolgáltatás rendelkezik beépített támogatása [HTTP-válasz gyorsítótár](api-management-howto-cache.md) hello erőforrás URL-cím segítségével hello kulcsként.</span><span class="sxs-lookup"><span data-stu-id="490f7-104">Azure API Management service has built-in support for [HTTP response caching](api-management-howto-cache.md) using hello resource URL as hello key.</span></span> <span data-ttu-id="490f7-105">hello kulcs által módosítható hello segítségével kérelemfejléc `vary-by` tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="490f7-105">hello key can be modified by request headers using hello `vary-by` properties.</span></span> <span data-ttu-id="490f7-106">Ez akkor hasznos, a teljes HTTP-válaszok (más néven felelősséget) gyorsítótárazáshoz, de egyes esetekben hasznos toojust gyorsítótár létrehozása egy részét.</span><span class="sxs-lookup"><span data-stu-id="490f7-106">This is useful for caching entire HTTP responses (aka representations), but sometimes it is useful toojust cache a portion of a representation.</span></span> <span data-ttu-id="490f7-107">új hello [gyorsítótár-keresési-érték](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) és [gyorsítótár-tároló-érték](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) házirendek hello lehetőséget nyújtanak toostore és lekérése tetszőleges darabjai belül a házirend-definíciók adatait.</span><span class="sxs-lookup"><span data-stu-id="490f7-107">hello new [cache-lookup-value](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) and [cache-store-value](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) policies provide hello ability toostore and retrieve arbitrary pieces of data from within policy definitions.</span></span> <span data-ttu-id="490f7-108">Ez a lehetőség is hozzáadja a korábban bemutatott érték toohello [küldési-kérelmek](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) házirend mivel most gyorsítótárazhatja a válaszok külső szolgáltatásokból.</span><span class="sxs-lookup"><span data-stu-id="490f7-108">This ability also adds value toohello previously introduced [send-request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) policy because you can now cache responses from external services.</span></span>

## <a name="architecture"></a><span data-ttu-id="490f7-109">Architektúra</span><span class="sxs-lookup"><span data-stu-id="490f7-109">Architecture</span></span>
<span data-ttu-id="490f7-110">API-kezelés szolgáltatás egy megosztott / bérlői adatgyorsítótár használ, úgy, hogy, hogy toomultiple egységek méretezése hozzáférés toohello továbbra is megkapja azonos gyorsítótárazott adatokat.</span><span class="sxs-lookup"><span data-stu-id="490f7-110">API Management service uses a shared per-tenant data cache so that, as you scale up toomultiple units you will still get access toohello same cached data.</span></span> <span data-ttu-id="490f7-111">Azonban több területi telepítés használatakor vannak független gyorsítótárak belül hello régióban.</span><span class="sxs-lookup"><span data-stu-id="490f7-111">However, when working with a multi-region deployment there are independent caches within each of hello regions.</span></span> <span data-ttu-id="490f7-112">Esedékes toothis, fontos toonot hello gyorsítótár tekinti a tárolóban, néhány adat, hello csak forrását.</span><span class="sxs-lookup"><span data-stu-id="490f7-112">Due toothis, it is important toonot treat hello cache as a data store, where it is hello only source of some piece of information.</span></span> <span data-ttu-id="490f7-113">Ha volt, és később úgy döntött, hello több területi telepítési tootake előnyeit, haladnak, akik rendelkező ügyfelek hozzáférési toothat gyorsítótárazott adatok elveszhetnek.</span><span class="sxs-lookup"><span data-stu-id="490f7-113">If you did, and later decided tootake advantage of hello multi-region deployment, then customers with users that travel may lose access toothat cached data.</span></span>

## <a name="fragment-caching"></a><span data-ttu-id="490f7-114">Töredék gyorsítótárazása</span><span class="sxs-lookup"><span data-stu-id="490f7-114">Fragment caching</span></span>
<span data-ttu-id="490f7-115">Nincsenek bizonyos esetekben, ahol válaszokat ad vissza adatokat, olcsóbbá toodetermine, és még marad friss elfogadható időn néhány részét tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="490f7-115">There are certain cases where responses being returned contain some portion of data that is expensive toodetermine and yet remains fresh for a reasonable amount of time.</span></span> <span data-ttu-id="490f7-116">Tegyük fel fontolja meg egy szolgáltatás, amely repülési foglalásokat, repülési állapot stb vonatkozó információkat biztosító légitársaság. Hello felhasználó tagja hello légitársaság pontok program, ha azok kellene tootheir aktuális állapot és halmozott távolság vonatkozó információkat is.</span><span class="sxs-lookup"><span data-stu-id="490f7-116">As an example, consider a service built by an airline that provides information relating flight reservations, flight status, etc. If hello user is a member of hello airlines points program, they would also have information relating tootheir current status and mileage accumulated.</span></span> <span data-ttu-id="490f7-117">Eltérő tárolódhat, a felhasználóval kapcsolatos adatokat, de lehet, hogy a válaszok által visszaadott repülési állapotáról és foglalások kívánatos tooinclude.</span><span class="sxs-lookup"><span data-stu-id="490f7-117">This user-related information might be stored in a different system, but it may be desirable tooinclude it in responses returned about flight status and reservations.</span></span> <span data-ttu-id="490f7-118">Ezt megteheti egy zónaaláírásnak nevezett töredék gyorsítótárazását.</span><span class="sxs-lookup"><span data-stu-id="490f7-118">This can be done using a process called fragment caching.</span></span> <span data-ttu-id="490f7-119">hello elsődleges ábrázolását adhatók vissza használatával token tooindicate valamilyen ahol hello felhasználóval kapcsolatos adatokat beszúrni toobe hello forráskiszolgálóról.</span><span class="sxs-lookup"><span data-stu-id="490f7-119">hello primary representation can be returned from hello origin server using some kind of token tooindicate where hello user-related information is toobe inserted.</span></span> 

<span data-ttu-id="490f7-120">Vegye figyelembe a következő JSON-válasz egy háttérrendszerből API hello.</span><span class="sxs-lookup"><span data-stu-id="490f7-120">Consider hello following JSON response from a backend API.</span></span>

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

<span data-ttu-id="490f7-121">És a másodlagos helyen erőforrás `/userprofile/{userid}` láthatóhoz hasonló,</span><span class="sxs-lookup"><span data-stu-id="490f7-121">And secondary resource at `/userprofile/{userid}` that looks like,</span></span>

```json
{ "username" : "Bob Smith", "Status" : "Gold" }
```

<span data-ttu-id="490f7-122">A sorrend toodetermine hello megfelelő felhasználói információ tooinclude igazolnia kell a hello felhasználó, aki tooidentify.</span><span class="sxs-lookup"><span data-stu-id="490f7-122">In order toodetermine hello appropriate user information tooinclude, we need tooidentify who hello end user is.</span></span> <span data-ttu-id="490f7-123">Ez a módszer használata függő végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="490f7-123">This mechanism is implementation dependent.</span></span> <span data-ttu-id="490f7-124">Tegyük fel, használok hello `Subject` a jogcím egy `JWT` token.</span><span class="sxs-lookup"><span data-stu-id="490f7-124">As an example, I am using hello `Subject` claim of a `JWT` token.</span></span> 

```xml
<set-variable
  name="enduserid"
  value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />
```

<span data-ttu-id="490f7-125">Ez tároljuk `enduserid` későbbi használatra környezeti változó értékét.</span><span class="sxs-lookup"><span data-stu-id="490f7-125">We store this `enduserid` value in a context variable for later use.</span></span> <span data-ttu-id="490f7-126">hello következő lépésre toodetermine, ha a korábbi kérelmekre már hello felhasználói adatok beolvasását és hello-gyorsítótárában tárolja azt.</span><span class="sxs-lookup"><span data-stu-id="490f7-126">hello next step is toodetermine if a previous request has already retrieved hello user information and stored it in hello cache.</span></span> <span data-ttu-id="490f7-127">A hello használjuk `cache-lookup-value` házirend.</span><span class="sxs-lookup"><span data-stu-id="490f7-127">For this we use hello `cache-lookup-value` policy.</span></span>

```xml
<cache-lookup-value
key="@("userprofile-" + context.Variables["enduserid"])"
variable-name="userprofile" />
```

<span data-ttu-id="490f7-128">Ha nem található bejegyzés toohello kulcs értékét, akkor a nem megfelelő hello gyorsítótárában `userprofile` környezeti változó jön létre.</span><span class="sxs-lookup"><span data-stu-id="490f7-128">If there is no entry in hello cache that corresponds toohello key value, then no `userprofile` context variable will be created.</span></span> <span data-ttu-id="490f7-129">Hello segítségével hello keresési hello sikerességének ellenőrizzük `choose` adatfolyam házirend szabályozza.</span><span class="sxs-lookup"><span data-stu-id="490f7-129">We check hello success of hello lookup using hello `choose` control flow policy.</span></span>

```xml
<choose>
    <when condition="@(!context.Variables.ContainsKey("userprofile"))">
        <!-- If hello userprofile context variable doesn’t exist, make an HTTP request tooretrieve it.  -->
    </when>
</choose>
```

<span data-ttu-id="490f7-130">Ha hello `userprofile` környezeti változó nem létezik, majd folyamatban, amelyet toohave toomake HTTP kérelem tooretrieve azt.</span><span class="sxs-lookup"><span data-stu-id="490f7-130">If hello `userprofile` context variable doesn’t exist, then we are going toohave toomake an HTTP request tooretrieve it.</span></span>

```xml
<send-request
  mode="new"
  response-variable-name="userprofileresponse"
  timeout="10"
  ignore-error="true">

  <!-- Build a URL that points toohello profile for hello current end-user -->
  <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),
      (string)context.Variables["enduserid"]).AbsoluteUri)
  </set-url>
  <set-method>GET</set-method>
</send-request>
```

<span data-ttu-id="490f7-131">Hello használjuk `enduserid` tooconstruct hello URL-cím toohello felhasználói profil erőforrás.</span><span class="sxs-lookup"><span data-stu-id="490f7-131">We use hello `enduserid` tooconstruct hello URL toohello user profile resource.</span></span> <span data-ttu-id="490f7-132">Tudunk hello választ, ha azt lekéréses hello szövegtörzs hello választ ki, és újra üzembe a környezeti változó tárolja.</span><span class="sxs-lookup"><span data-stu-id="490f7-132">Once we have hello response, we can pull hello body text out of hello response and store it back into a context variable.</span></span>

```xml
<set-variable
    name="userprofile"
    value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />
```

<span data-ttu-id="490f7-133">tooavoid velünk, hogy toomake a HTTP-kérelem újra, amikor hello ugyanaz a felhasználó egy másik kérelmet, azt tárolhat hello felhasználói profil hello gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="490f7-133">tooavoid us having toomake this HTTP request again, when hello same user makes another request, we can store hello user profile in hello cache.</span></span>

```xml
<cache-store-value
    key="@("userprofile-" + context.Variables["enduserid"])"
    value="@((string)context.Variables["userprofile"])" duration="100000" />
```

<span data-ttu-id="490f7-134">Hello érték hello pontos azonos kulccsal, hogy azt eredetileg megkísérelt tooretrieve hello gyorsítótárában tároljuk azt.</span><span class="sxs-lookup"><span data-stu-id="490f7-134">We store hello value in hello cache using hello exact same key that we originally attempted tooretrieve it with.</span></span> <span data-ttu-id="490f7-135">hello időtartamot, választjuk toostore hello érték alapján milyen gyakran hello változtatások és hogyan hibatűrő felhasználók is tooout a naprakész információkat.</span><span class="sxs-lookup"><span data-stu-id="490f7-135">hello duration that we choose toostore hello value should be based on how often hello information changes and how tolerant users are tooout of date information.</span></span> 

<span data-ttu-id="490f7-136">Fontos, hogy hello gyorsítótár lekérése még egy folyamaton kívül, a hálózati kérelmek toorealize és potenciálisan továbbra is felvehetőek ezredmásodperc toohello kérelem több.</span><span class="sxs-lookup"><span data-stu-id="490f7-136">It is important toorealize that retrieving from hello cache is still an out-of-process, network request and potentially can still add tens of milliseconds toohello request.</span></span> <span data-ttu-id="490f7-137">hello előnyei származnak, amikor meghatározó hello felhasználói profillal kapcsolatos információk hosszabb időbe telik jelentősen, amely megfelelő tooneeding toodo adatbázis-lekérdezést vagy több biztonsági-végpontok összesített adatait.</span><span class="sxs-lookup"><span data-stu-id="490f7-137">hello benefits come when determining hello user profile information takes significantly longer than that due tooneeding toodo database queries or aggregate information from multiple back-ends.</span></span>

<span data-ttu-id="490f7-138">hello hello folyamat utolsó lépése tooupdate hello választ a felhasználó profil adataival.</span><span class="sxs-lookup"><span data-stu-id="490f7-138">hello final step in hello process is tooupdate hello returned response with our user profile information.</span></span>

```xml
<!-- Update response body with user profile-->
<find-and-replace
    from='"$userprofile$"'
    to="@((string)context.Variables["userprofile"])" />
```

<span data-ttu-id="490f7-139">Elfogadása tooinclude hello idézőjelek hello jogkivonat részeként, hogy akkor is, ha a név felülírandó hello nem következik be, hello válasz volt-e továbbra is érvényes JSON-adatokat.</span><span class="sxs-lookup"><span data-stu-id="490f7-139">I chose tooinclude hello quotation marks as part of hello token so that even when hello replace doesn’t occur, hello response was still valid JSON.</span></span> <span data-ttu-id="490f7-140">Elsősorban hibakeresési könnyebb toomake volt.</span><span class="sxs-lookup"><span data-stu-id="490f7-140">This was primarily toomake debugging easier.</span></span>

<span data-ttu-id="490f7-141">Együtt egyesíteni ezeket a lépéseket, miután a hello végeredménynek egy olyan házirend, egyet a következő hello tűnik.</span><span class="sxs-lookup"><span data-stu-id="490f7-141">Once you combine all these steps together, hello end result is a policy that looks like hello following one.</span></span>

```xml
<policies>
    <inbound>
        <!-- How you determine user identity is application dependent -->
        <set-variable
          name="enduserid"
          value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

        <!--Look for userprofile for this user in hello cache -->
        <cache-lookup-value
          key="@("userprofile-" + context.Variables["enduserid"])"
          variable-name="userprofile" />

        <!-- If we don’t find it in hello cache, make a request for it and store it -->
        <choose>
            <when condition="@(!context.Variables.ContainsKey("userprofile"))">
                <!-- Make HTTP request tooget user profile -->
                <send-request
                  mode="new"
                  response-variable-name="userprofileresponse"
                  timeout="10"
                  ignore-error="true">

                   <!-- Build a URL that points toohello profile for hello current end-user -->
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

<span data-ttu-id="490f7-142">Ez a megközelítés gyorsítótárazási elsősorban a webhelyeken ahol HTML állnak hello kiszolgáló oldalán, hogy egyetlen lapként megjeleníthetők.</span><span class="sxs-lookup"><span data-stu-id="490f7-142">This caching approach is primarily used in web sites where HTML is composed on hello server side so that it can be rendered as a single page.</span></span> <span data-ttu-id="490f7-143">Azonban ez is hasznos lehet az API-k, ahol az ügyfelek nem ügyfél oldalán található HTTP gyorsítótárazását, vagy kívánatos nem tooput hello ügyfél, amely felelős.</span><span class="sxs-lookup"><span data-stu-id="490f7-143">However, it can also be useful in APIs where clients cannot do client side HTTP caching or it is desirable not tooput that responsibility on hello client.</span></span>

<span data-ttu-id="490f7-144">A töredék gyorsítótárazás ugyanolyan típusú hello webes a háttérkiszolgálókon lévő a Redis gyorsítótár-kiszolgáló használatával is elvégezhető, azonban hello API-kezelés szolgáltatás tooperform használatával a munkahelyi akkor hasznos, ha különböző biztonsági ends mint hello érkező gyorsítótárazott hello töredék elsődleges válaszokat.</span><span class="sxs-lookup"><span data-stu-id="490f7-144">This same kind of fragment caching can also be done on hello backend web servers using a Redis caching server, however, using hello API Management service tooperform this work is useful when hello cached fragments are coming from different back-ends than hello primary responses.</span></span>

## <a name="transparent-versioning"></a><span data-ttu-id="490f7-145">Transzparens versioning</span><span class="sxs-lookup"><span data-stu-id="490f7-145">Transparent versioning</span></span>
<span data-ttu-id="490f7-146">Általános gyakorlat egy API-toobe támogatott egyszerre több különböző végrehajtási verziója.</span><span class="sxs-lookup"><span data-stu-id="490f7-146">It is common practice for multiple different implementation versions of an API toobe supported at any one time.</span></span> <span data-ttu-id="490f7-147">Ez lehet, hogy toosupport különböző környezetekben, például a fejlesztői, tesztelési, éles, stb., vagy lehet, hogy toosupport hello API toogive idő API fogyasztók toomigrate toonewer verzióihoz régebbi verzióit.</span><span class="sxs-lookup"><span data-stu-id="490f7-147">This is perhaps toosupport different environments, like dev, test, production, etc, or it may be toosupport older versions of hello API toogive time for API consumers toomigrate toonewer versions.</span></span> 

<span data-ttu-id="490f7-148">Ehelyett ez az ügyfél fejlesztők toochange hello URL-címei igénylő egyik módszer toohandling `/v1/customers` túl`/v2/customers` hello felhasználói profil adatok toostore van hello API verziójának azok jelenleg toouse kívánja, és hívja meg hello megfelelő háttérkiszolgáló URL-címe.</span><span class="sxs-lookup"><span data-stu-id="490f7-148">One approach toohandling this instead of requiring client developers toochange hello URLs from `/v1/customers` too`/v2/customers` is toostore in hello consumer’s profile data which version of hello API they currently wish toouse and call hello appropriate backend URL.</span></span> <span data-ttu-id="490f7-149">Rendelés toodetermine hello megfelelő háttér URL-cím toocall egy adott ügyfél számára, hogy a rendszer szükséges tooquery egyes konfigurációs adatokat.</span><span class="sxs-lookup"><span data-stu-id="490f7-149">In order toodetermine hello correct backend URL toocall for a particular client, it is necessary tooquery some configuration data.</span></span> <span data-ttu-id="490f7-150">A konfigurációs adatok gyorsítótárazása, azt is lecsökkentheti hello teljesítményét, ez a keresés során.</span><span class="sxs-lookup"><span data-stu-id="490f7-150">By caching this configuration data, we can minimize hello performance penalty of doing this lookup.</span></span>

<span data-ttu-id="490f7-151">hello első lépése a, toodetermine hello azonosítója használt tooconfigure hello kívánt verziójával.</span><span class="sxs-lookup"><span data-stu-id="490f7-151">hello first step is toodetermine hello identifier used tooconfigure hello desired version.</span></span> <span data-ttu-id="490f7-152">Ebben a példában elfogadása tooassociate hello verzió toohello előfizetés termékkulcsot.</span><span class="sxs-lookup"><span data-stu-id="490f7-152">In this example, I chose tooassociate hello version toohello product subscription key.</span></span> 

```xml
<set-variable name="clientid" value="@(context.Subscription.Key)" />
```

<span data-ttu-id="490f7-153">A gyorsítótár keresési toosee azt majd hajtsa végre, ha azt már beolvasását hello kívánt ügyfél verziója.</span><span class="sxs-lookup"><span data-stu-id="490f7-153">We then do a cache lookup toosee if we already have retrieved hello desired client version.</span></span>

```xml
<cache-lookup-value
key="@("clientversion-" + context.Variables["clientid"])"
variable-name="clientversion" />
```

<span data-ttu-id="490f7-154">Ezután toosee azt ellenőrizze, hogy azt nem találta azt a hello gyorsítótárban.</span><span class="sxs-lookup"><span data-stu-id="490f7-154">Then we check toosee if we did not find it in hello cache.</span></span>

```xml
<choose>
    <when condition="@(!context.Variables.ContainsKey("clientversion"))">
```

<span data-ttu-id="490f7-155">Ha azt nem, akkor nyissa meg azt, és lekéréséhez.</span><span class="sxs-lookup"><span data-stu-id="490f7-155">If we didn’t then we go and retrieve it.</span></span>

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

<span data-ttu-id="490f7-156">Hello válasz törzsében szöveg kinyerése hello válasz.</span><span class="sxs-lookup"><span data-stu-id="490f7-156">Extract hello response body text from hello response.</span></span>

```xml
<set-variable
      name="clientversion"
      value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />
```

<span data-ttu-id="490f7-157">Vissza tárolása a későbbi használatra hello gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="490f7-157">Store it back in hello cache for future use.</span></span>

```xml
<cache-store-value
      key="@("clientversion-" + context.Variables["clientid"])"
      value="@((string)context.Variables["clientversion"])"
      duration="100000" />
```

<span data-ttu-id="490f7-158">És végül frissítse hello háttér-URL-cím tooselect hello hello szolgáltatás hello ügyfél szükséges.</span><span class="sxs-lookup"><span data-stu-id="490f7-158">And finally update hello back-end URL tooselect hello version of hello service desired by hello client.</span></span>

```xml
<set-backend-service
      base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
```

<span data-ttu-id="490f7-159">hello teljesen házirendet a következőképpen történik.</span><span class="sxs-lookup"><span data-stu-id="490f7-159">hello completely policy is as follows.</span></span>

```xml
<inbound>
    <base />
    <set-variable name="clientid" value="@(context.Subscription.Key)" />
    <cache-lookup-value key="@("clientversion-" + context.Variables["clientid"])" variable-name="clientversion" />

    <!-- If we don’t find it in hello cache, make a request for it and store it -->
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

<span data-ttu-id="490f7-160">API fogyasztók tootransparently vezérlő melyik háttérrendszer verzió ügyfelek tooupdate és helyezze üzembe újra ügyfelek anélkül is hozzáférnek engedélyezése egy elegáns megoldás, amely sok API versioning vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="490f7-160">Enabling API consumers tootransparently control which backend version is being accessed by clients without having tooupdate and redeploy clients is a elegant solution that addresses many API versioning concerns.</span></span>

## <a name="tenant-isolation"></a><span data-ttu-id="490f7-161">Bérlők elszigetelésére</span><span class="sxs-lookup"><span data-stu-id="490f7-161">Tenant Isolation</span></span>
<span data-ttu-id="490f7-162">A nagyobb, a több-bérlős telepítésekben egyes vállalatok bérlők külön csoportot létre háttér hardver különböző üzemelő példányok esetében.</span><span class="sxs-lookup"><span data-stu-id="490f7-162">In larger, multi-tenant deployments some companies create separate groups of tenants on distinct deployments of backend hardware.</span></span> <span data-ttu-id="490f7-163">Ez minimalizálja a hello hello háttérkiszolgálón a hardver probléma által érintett felhasználók száma.</span><span class="sxs-lookup"><span data-stu-id="490f7-163">This minimizes hello number of customers who are impacted by a hardware issue on hello backend.</span></span> <span data-ttu-id="490f7-164">Emellett lehetővé teszi az új szoftverfrissítési verziók toobe szakaszaiban megkezdődött.</span><span class="sxs-lookup"><span data-stu-id="490f7-164">It also enables new software versions toobe rolled out in stages.</span></span> <span data-ttu-id="490f7-165">Ideális esetben a háttér-architektúra átlátszó tooAPI fogyasztók kell lennie.</span><span class="sxs-lookup"><span data-stu-id="490f7-165">Ideally this backend architecture should be transparent tooAPI consumers.</span></span> <span data-ttu-id="490f7-166">Ez megvalósítható a egy hasonló módon tootransparent versioning mert hello alapul azonos módszerrel kezelésére hello háttérkiszolgáló URL-címet konfiguráció állapota / API-kulcs használatával.</span><span class="sxs-lookup"><span data-stu-id="490f7-166">This can be achieved in a similar way tootransparent versioning because it is based on hello same technique of manipulating hello backend URL using configuration state per API key.</span></span>  

<span data-ttu-id="490f7-167">Helyett visszaadó API hello minden előfizetés kulcs egy előnyben részesített verziója, a bérlő hozzárendelt toohello hardvercsoport kapcsolódó azonosítót alakítanák vissza.</span><span class="sxs-lookup"><span data-stu-id="490f7-167">Instead of returning a preferred version of hello API for each subscription key, you would return an identifier that relates a tenant toohello assigned hardware group.</span></span> <span data-ttu-id="490f7-168">Ilyen azonosítójú használt tooconstruct hello megfelelő háttérkiszolgáló URL-cím lehet.</span><span class="sxs-lookup"><span data-stu-id="490f7-168">That identifier can be used tooconstruct hello appropriate backend URL.</span></span>

## <a name="summary"></a><span data-ttu-id="490f7-169">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="490f7-169">Summary</span></span>
<span data-ttu-id="490f7-170">hello szabadsága toouse hello bármilyen típusú adatok tárolásához Azure API management gyorsítótár lehetővé teszi, hogy hatékony hozzáférés tooconfiguration, amelyek hatással lehetnek a hello módja egy bejövő kérelem feldolgozását.</span><span class="sxs-lookup"><span data-stu-id="490f7-170">hello freedom toouse hello Azure API management cache for storing any kind of data enables efficient access tooconfiguration data that can affect hello way an inbound request is processed.</span></span> <span data-ttu-id="490f7-171">Lehet, hogy is kiegészítheti a válaszokat, egy háttér-API által visszaadott használt toostore adatok töredék is.</span><span class="sxs-lookup"><span data-stu-id="490f7-171">It can also be used toostore data fragments that can augment responses, returned from a backend API.</span></span>

## <a name="next-steps"></a><span data-ttu-id="490f7-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="490f7-172">Next steps</span></span>
<span data-ttu-id="490f7-173">Adjon küldjön visszajelzést a hello disqus-beszélgetésben teheti a szál az az ebben a témakörben ha vannak a többi olyan forgatókönyvet, hogy ezeket a házirendeket az Ön engedélyezte, vagy ha vannak forgatókönyvek szeretné tooachieve, de nem érzi, hogy jelenleg lehetségesek.</span><span class="sxs-lookup"><span data-stu-id="490f7-173">Please give us your feedback in hello Disqus thread for this topic if there are other scenarios that these policies have enabled for you, or if there are scenarios you would like tooachieve but do not feel are currently possible.</span></span>

