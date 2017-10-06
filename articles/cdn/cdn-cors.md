---
title: a CORS Azure CDN aaaUsing |} Microsoft Docs
description: Ismerje meg, hogyan toouse hello Azure Content Delivery Network (CDN) toowith Cross-Origin Resource Sharing (CORS).
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 86740a96-4269-4060-aba3-a69f00e6f14e
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 6c743b56c32a2d3aacc9a77094cb87d61b95d2f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-cdn-with-cors"></a><span data-ttu-id="f2abc-103">Az Azure CDN a CORS használatával</span><span class="sxs-lookup"><span data-stu-id="f2abc-103">Using Azure CDN with CORS</span></span>
## <a name="what-is-cors"></a><span data-ttu-id="f2abc-104">Mi az a CORS?</span><span class="sxs-lookup"><span data-stu-id="f2abc-104">What is CORS?</span></span>
<span data-ttu-id="f2abc-105">A CORS (Cross eredetű erőforrások megosztása) egy HTTP-szolgáltatás, amely lehetővé teszi egy webalkalmazást az egyik tartomány tooaccess erőforrásainak fut egy másik tartományban.</span><span class="sxs-lookup"><span data-stu-id="f2abc-105">CORS (Cross Origin Resource Sharing) is an HTTP feature that enables a web application running under one domain tooaccess resources in another domain.</span></span> <span data-ttu-id="f2abc-106">Rendelés tooreduce hello lehetőségét többhelyes parancsfájlok futtatására, a minden modern webböngésző néven ismert biztonsági korlátozások megvalósítása [azonos eredetű házirend](http://www.w3.org/Security/wiki/Same_Origin_Policy).</span><span class="sxs-lookup"><span data-stu-id="f2abc-106">In order tooreduce hello possibility of cross-site scripting attacks, all modern web browsers implement a security restriction known as [same-origin policy](http://www.w3.org/Security/wiki/Same_Origin_Policy).</span></span>  <span data-ttu-id="f2abc-107">Ez megakadályozza, hogy a hívó API-k egy másik tartományban származó weblap.</span><span class="sxs-lookup"><span data-stu-id="f2abc-107">This prevents a web page from calling APIs in a different domain.</span></span>  <span data-ttu-id="f2abc-108">A CORS egy biztonságos módon tooallow egy eredeti adatforrással (hello forrástartományt) toocall API-k az eredeti biztosít.</span><span class="sxs-lookup"><span data-stu-id="f2abc-108">CORS provides a secure way tooallow one origin (hello origin domain) toocall APIs in another origin.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="f2abc-109">Működés</span><span class="sxs-lookup"><span data-stu-id="f2abc-109">How it works</span></span>
<span data-ttu-id="f2abc-110">A CORS kérelmek két különböző *egyszerű kérelmek* és *összetett kérelmeket.*</span><span class="sxs-lookup"><span data-stu-id="f2abc-110">There are two types of CORS requests, *simple requests* and *complex requests.*</span></span>

### <a name="for-simple-requests"></a><span data-ttu-id="f2abc-111">Egyszerű kérelmeknél:</span><span class="sxs-lookup"><span data-stu-id="f2abc-111">For simple requests:</span></span>

1. <span data-ttu-id="f2abc-112">hello böngésző küldi hello CORS kérelem további **származási** HTTP-kérés fejlécének.</span><span class="sxs-lookup"><span data-stu-id="f2abc-112">hello browser sends hello CORS request with an additional **Origin** HTTP request header.</span></span> <span data-ttu-id="f2abc-113">hello a fejléc értéke hello származási hello szülő lapon, amely hello kombinációja típusúként van definiálva szolgáltatott *protokoll* *tartomány,* és *port.*</span><span class="sxs-lookup"><span data-stu-id="f2abc-113">hello value of this header is hello origin that served hello parent page, which is defined as hello combination of *protocol,* *domain,* and *port.*</span></span>  <span data-ttu-id="f2abc-114">Amikor egy lapot https://www.contoso.com próbál tooaccess hello fabrikam.com forrása a felhasználó adatai, a következő fejléc hello küldheti toofabrikam.com:</span><span class="sxs-lookup"><span data-stu-id="f2abc-114">When a page from https://www.contoso.com attempts tooaccess a user's data in hello fabrikam.com origin, hello following request header would be sent toofabrikam.com:</span></span>

   `Origin: https://www.contoso.com`

2. <span data-ttu-id="f2abc-115">hello server hello következő jelenhetnek meg:</span><span class="sxs-lookup"><span data-stu-id="f2abc-115">hello server may respond with any of hello following:</span></span>

   * <span data-ttu-id="f2abc-116">Egy **hozzáférés-vezérlési-engedélyezése-forrás** jelző a származási hely engedélyezett válaszában fejléc.</span><span class="sxs-lookup"><span data-stu-id="f2abc-116">An **Access-Control-Allow-Origin** header in its response indicating which origin site is allowed.</span></span> <span data-ttu-id="f2abc-117">Példa:</span><span class="sxs-lookup"><span data-stu-id="f2abc-117">For example:</span></span>

     `Access-Control-Allow-Origin: https://www.contoso.com`

   * <span data-ttu-id="f2abc-118">Egy HTTP-hibakód: például a 403-as, ha hello server nem engedélyezi a hello eltérő eredetű kérelem hello származási fejléc ellenőrzése után</span><span class="sxs-lookup"><span data-stu-id="f2abc-118">An HTTP error code such as 403 if hello server does not allow hello cross-origin request after checking hello Origin header</span></span>

   * <span data-ttu-id="f2abc-119">Egy **hozzáférés-vezérlési-engedélyezése-forrás** helyettesítő karakter, amely lehetővé teszi minden eredet fejléc:</span><span class="sxs-lookup"><span data-stu-id="f2abc-119">An **Access-Control-Allow-Origin** header with a wildcard that allows all origins:</span></span>

     `Access-Control-Allow-Origin: *`

### <a name="for-complex-requests"></a><span data-ttu-id="f2abc-120">Összetett kérelmeknél:</span><span class="sxs-lookup"><span data-stu-id="f2abc-120">For complex requests:</span></span>

<span data-ttu-id="f2abc-121">Egy összetett vonatkozó kérés, mert a CORS kérelem hello böngésző esetén szükséges toosend egy *ellenőrzési kérés* (azaz egy előzetes mintavételt) hello tényleges CORS kérelem elküldése előtt.</span><span class="sxs-lookup"><span data-stu-id="f2abc-121">A complex request is a CORS request where hello browser is required toosend a *preflight request* (i.e. a preliminary probe) before sending hello actual CORS request.</span></span> <span data-ttu-id="f2abc-122">hello ellenőrzési kérés hello server engedélyt kér Ha hello eredeti CORS kérelem lépne, és van egy `OPTIONS` toohello kérelem URL-CÍMÉRE.</span><span class="sxs-lookup"><span data-stu-id="f2abc-122">hello preflight request asks hello server permission if hello original CORS request can proceed and is an `OPTIONS` request toohello same URL.</span></span>

> [!TIP]
> <span data-ttu-id="f2abc-123">A CORS-adatfolyamok és közös nehézségek további részletekért tekintse meg a hello [tooCORS útmutató REST API-kat tartalmaz](https://www.moesif.com/blog/technical/cors/Authoritative-Guide-to-CORS-Cross-Origin-Resource-Sharing-for-REST-APIs/).</span><span class="sxs-lookup"><span data-stu-id="f2abc-123">For more details on CORS flows and common pitfalls, view hello [Guide tooCORS for REST APIs](https://www.moesif.com/blog/technical/cors/Authoritative-Guide-to-CORS-Cross-Origin-Resource-Sharing-for-REST-APIs/).</span></span>
>
>

## <a name="wildcard-or-single-origin-scenarios"></a><span data-ttu-id="f2abc-124">Helyettesítő karakteres vagy egyetlen forrás forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="f2abc-124">Wildcard or single origin scenarios</span></span>
<span data-ttu-id="f2abc-125">Az Azure CDN CORS automatikusan együttműködve nincs további konfigurálásra, ha hello **hozzáférés-vezérlési-engedélyezése-forrás** fejléc értéke toowildcard (*) vagy egy egyetlen forrása.</span><span class="sxs-lookup"><span data-stu-id="f2abc-125">CORS on Azure CDN will work automatically with no additional configuration when hello **Access-Control-Allow-Origin** header is set toowildcard (*) or a single origin.</span></span>  <span data-ttu-id="f2abc-126">hello CDN hello első válasz gyorsítótárazni fog, és később fogja használni a hello azonos fejléc.</span><span class="sxs-lookup"><span data-stu-id="f2abc-126">hello CDN will cache hello first response and subsequent requests will use hello same header.</span></span>

<span data-ttu-id="f2abc-127">Ha kérelmek már végzett toohello CDN előzetes tooCORS a hello beállítása a forrás, lesz szükség toopurge tartalmat a végpont tartalom tooreload hello hello tartalom **hozzáférés-vezérlési-engedélyezése-forrás** fejléc.</span><span class="sxs-lookup"><span data-stu-id="f2abc-127">If requests have already been made toohello CDN prior tooCORS being set on hello your origin, you will need toopurge content on your endpoint content tooreload hello content with hello **Access-Control-Allow-Origin** header.</span></span>

## <a name="multiple-origin-scenarios"></a><span data-ttu-id="f2abc-128">Több forrás forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="f2abc-128">Multiple origin scenarios</span></span>
<span data-ttu-id="f2abc-129">Ha módosítania kell a CORS engedélyezett eredetet toobe listának tooallow, részek lesznek egy kicsit bonyolultabb.</span><span class="sxs-lookup"><span data-stu-id="f2abc-129">If you need tooallow a specific list of origins toobe allowed for CORS, things get a little more complicated.</span></span> <span data-ttu-id="f2abc-130">hello probléma akkor fordul elő, amikor gyorsítótárazza a hello CDN hello **hozzáférés-vezérlési-engedélyezése-forrás** hello első CORS forrás fejléc.</span><span class="sxs-lookup"><span data-stu-id="f2abc-130">hello problem occurs when hello CDN caches hello **Access-Control-Allow-Origin** header for hello first CORS origin.</span></span>  <span data-ttu-id="f2abc-131">Ha egy másik CORS származási későbbi kérést, hello CDN teljesíti a gyorsítótárazott hello **hozzáférés-vezérlési-engedélyezése-forrás** fejléc, amelyek nem egyeznek.</span><span class="sxs-lookup"><span data-stu-id="f2abc-131">When a different CORS origin makes a subsequent request, hello CDN will serve hello cached **Access-Control-Allow-Origin** header, which won't match.</span></span>  <span data-ttu-id="f2abc-132">Nincsenek számos módon toocorrect ez.</span><span class="sxs-lookup"><span data-stu-id="f2abc-132">There are several ways toocorrect this.</span></span>

### <a name="azure-cdn-premium-from-verizon"></a><span data-ttu-id="f2abc-133">Prémium szintű Azure CDN a Verizontól</span><span class="sxs-lookup"><span data-stu-id="f2abc-133">Azure CDN Premium from Verizon</span></span>
<span data-ttu-id="f2abc-134">legjobb módja tooenable hello Ez az toouse **verizon Azure CDN Premium**, amely azt mutatja, néhány speciális funkcióinak.</span><span class="sxs-lookup"><span data-stu-id="f2abc-134">hello best way tooenable this is toouse **Azure CDN Premium from Verizon**, which exposes some advanced functionality.</span></span> 

<span data-ttu-id="f2abc-135">Túl kell[hozzon létre egy szabályt](cdn-rules-engine.md) toocheck hello **származási** fejléc hello kérésre.</span><span class="sxs-lookup"><span data-stu-id="f2abc-135">You'll need too[create a rule](cdn-rules-engine.md) toocheck hello **Origin** header on hello request.</span></span>  <span data-ttu-id="f2abc-136">Ha egy érvényes forrás, a szabály állítja be hello **hozzáférés-vezérlési-engedélyezése-forrás** fejléc a következő hello kérelemben megadott hello forrása.</span><span class="sxs-lookup"><span data-stu-id="f2abc-136">If it's a valid origin, your rule will set hello **Access-Control-Allow-Origin** header with hello origin provided in hello request.</span></span>  <span data-ttu-id="f2abc-137">Ha hello származási megadott hello **származási** fejléc nem engedélyezett, a szabály el kell hagynia hello **hozzáférés-vezérlési-engedélyezése-forrás** fejléc, aminek következtében hello böngésző tooreject hello kérelem.</span><span class="sxs-lookup"><span data-stu-id="f2abc-137">If hello origin specified in hello **Origin** header is not allowed, your rule should omit hello **Access-Control-Allow-Origin** header which will cause hello browser tooreject hello request.</span></span> 

<span data-ttu-id="f2abc-138">Nincsenek két módon toodo ez hello szabályok motor.</span><span class="sxs-lookup"><span data-stu-id="f2abc-138">There are two ways toodo this with hello rules engine.</span></span>  <span data-ttu-id="f2abc-139">Mindkét esetben hello **hozzáférés-vezérlési-engedélyezése-forrás** hello fájl forráskiszolgálóról fejléc teljesen figyelmen kívül hagyva, hello CDN szabálymotor teljes körű kezelése hello CORS források engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="f2abc-139">In both cases, hello **Access-Control-Allow-Origin** header from hello file's origin server is completely ignored, hello CDN's rules engine completely manages hello allowed CORS origins.</span></span>

#### <a name="one-regular-expression-with-all-valid-origins"></a><span data-ttu-id="f2abc-140">Minden érvényes eredet egy reguláris kifejezések</span><span class="sxs-lookup"><span data-stu-id="f2abc-140">One regular expression with all valid origins</span></span>
<span data-ttu-id="f2abc-141">Ebben az esetben létre fog hozni egy reguláris kifejezés, amely tartalmazza az összes hello források tooallow szeretne:</span><span class="sxs-lookup"><span data-stu-id="f2abc-141">In this case, you'll create a regular expression that includes all of hello origins you want tooallow:</span></span> 

    https?:\/\/(www\.contoso\.com|contoso\.com|www\.microsoft\.com|microsoft.com\.com)$

> [!TIP]
> <span data-ttu-id="f2abc-142">**Verizon Azure CDN** használ [Perl kompatibilis reguláris kifejezések](http://pcre.org/) , a reguláris kifejezések motor.</span><span class="sxs-lookup"><span data-stu-id="f2abc-142">**Azure CDN from Verizon** uses [Perl Compatible Regular Expressions](http://pcre.org/) as its engine for regular expressions.</span></span>  <span data-ttu-id="f2abc-143">Egy eszköz, például használhatja [reguláris kifejezések 101](https://regex101.com/) toovalidate a reguláris kifejezés.</span><span class="sxs-lookup"><span data-stu-id="f2abc-143">You can use a tool like [Regular Expressions 101](https://regex101.com/) toovalidate your regular expression.</span></span>  <span data-ttu-id="f2abc-144">Vegye figyelembe, hogy hello "/" karakter érvényes reguláris kifejezések, és nem szükséges escape-karaktersorozatot toobe, azonban ez a karakter escape ajánlott eljárás, és néhány regex érvényesség-ellenőrzők által várt.</span><span class="sxs-lookup"><span data-stu-id="f2abc-144">Note that hello "/" character is valid in regular expressions and doesn't need toobe escaped, however, escaping that character is considered a best practice and is expected by some regex validators.</span></span>
> 
> 

<span data-ttu-id="f2abc-145">Hello reguláris kifejezésnek megfelelő, ha a szabály felülírja, hello **hozzáférés-vezérlési-engedélyezése-forrás** fejléc (ha van ilyen) hello forrás a hello kérelmet küldött a hello a forrásból.</span><span class="sxs-lookup"><span data-stu-id="f2abc-145">If hello regular expression matches, your rule will replace hello **Access-Control-Allow-Origin** header (if any) from hello origin with hello origin that sent hello request.</span></span>  <span data-ttu-id="f2abc-146">Azt is megteheti további CORS fejlécek, például a **hozzáférés-vezérlési-engedélyezése-metódusok**.</span><span class="sxs-lookup"><span data-stu-id="f2abc-146">You can also add additional CORS headers, such as **Access-Control-Allow-Methods**.</span></span>

![A reguláris kifejezéssel szabályok – példa](./media/cdn-cors/cdn-cors-regex.png)

#### <a name="request-header-rule-for-each-origin"></a><span data-ttu-id="f2abc-148">Az egyes származási kérelmek fejléc szabályt.</span><span class="sxs-lookup"><span data-stu-id="f2abc-148">Request header rule for each origin.</span></span>
<span data-ttu-id="f2abc-149">Reguláris kifejezések, hanem ehelyett hozzon létre a külön szabály minden forrás hello segítségével tooallow kívánja **kérelem fejléc helyettesítő** [feltételének](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1).</span><span class="sxs-lookup"><span data-stu-id="f2abc-149">Rather than regular expressions, you can instead create a separate rule for each origin you wish tooallow using hello **Request Header Wildcard** [match condition](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1).</span></span> <span data-ttu-id="f2abc-150">Mint hello reguláris kifejezés módszerrel hello szabályok motor különálló beállítása hello CORS fejlécek.</span><span class="sxs-lookup"><span data-stu-id="f2abc-150">As with hello regular expression method, hello rules engine alone sets hello CORS headers.</span></span> 

![Szabályok reguláris kifejezés nélkül – példa](./media/cdn-cors/cdn-cors-no-regex.png)

> [!TIP]
> <span data-ttu-id="f2abc-152">Hello a fenti példában a hello hello helyettesítő karakter használatát * hello szabályok motor toomatch közli a HTTP és HTTPS.</span><span class="sxs-lookup"><span data-stu-id="f2abc-152">In hello example above, hello use of hello wildcard character * tells hello rules engine toomatch both HTTP and HTTPS.</span></span>
> 
> 

### <a name="azure-cdn-standard"></a><span data-ttu-id="f2abc-153">Az Azure CDN Standard</span><span class="sxs-lookup"><span data-stu-id="f2abc-153">Azure CDN Standard</span></span>
<span data-ttu-id="f2abc-154">Az Azure CDN Standard profilok hello csak a több források hello helyettesítő származási hello használata nélkül mechanizmus tooallow toouse [lekérdezési karakterláncok gyorsítótárazása](cdn-query-string.md).</span><span class="sxs-lookup"><span data-stu-id="f2abc-154">On Azure CDN Standard profiles, hello only mechanism tooallow for multiple origins without hello use of hello wildcard origin is toouse [query string caching](cdn-query-string.md).</span></span>  <span data-ttu-id="f2abc-155">Tooenable lekérdezési karakterláncának beállítása szükséges hello CDN-végpontot, és ezután használja az egy egyedi lekérdezési karakterlánc minden olyan engedélyezett tartományhoz érkező kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="f2abc-155">You need tooenable query string setting for hello CDN endpoint and then use a unique query string for requests from each allowed domain.</span></span> <span data-ttu-id="f2abc-156">Ez azt eredményezi, hogy hello CDN gyorsítótárazás egy külön objektum minden egyes egyedi lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="f2abc-156">Doing this will result in hello CDN caching a separate object for each unique query string.</span></span> <span data-ttu-id="f2abc-157">Ez a módszer nem ideális, azonban több példányát hello okoz, ugyanazon fájl a gyorsítótárazott hello CDN.</span><span class="sxs-lookup"><span data-stu-id="f2abc-157">This approach is not ideal, however, as it will result in multiple copies of hello same file cached on hello CDN.</span></span>  

