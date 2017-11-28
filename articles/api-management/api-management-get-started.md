---
title: "aaaManage az első API-nak Azure API Management |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate API-k, adja hozzá a műveleteket, és ismerkedjen meg az API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 51b7df8b-1c43-43c6-90c9-0aa24f48206b
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 7d43f33aa359c4d1e605e9fb41e43d323ca6a777
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-first-api-in-azure-api-management"></a><span data-ttu-id="05b39-103">Az első API kezelése az Azure API Management szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="05b39-103">Manage your first API in Azure API Management</span></span>
## <span data-ttu-id="05b39-104"><a name="overview"></a>Áttekintés</span><span class="sxs-lookup"><span data-stu-id="05b39-104"><a name="overview"> </a>Overview</span></span>
<span data-ttu-id="05b39-105">Ez az útmutató bemutatja, hogyan tooquickly Ismerkedés az Azure API Management használata, és az első API-hívás.</span><span class="sxs-lookup"><span data-stu-id="05b39-105">This guide shows you how tooquickly get started in using Azure API Management and make your first API call.</span></span>

## <span data-ttu-id="05b39-106"><a name="concepts"></a>Mi az Azure API Management?</span><span class="sxs-lookup"><span data-stu-id="05b39-106"><a name="concepts"> </a>What is Azure API Management?</span></span>
<span data-ttu-id="05b39-107">Használja az Azure API Management tootake bármilyen háttérrendszerből, és indítsa el a teljes körű API programot alapján.</span><span class="sxs-lookup"><span data-stu-id="05b39-107">You can use Azure API Management tootake any backend and launch a full-fledged API program based on it.</span></span>

<span data-ttu-id="05b39-108">A gyakori forgatókönyvek a következők:</span><span class="sxs-lookup"><span data-stu-id="05b39-108">Common scenarios include:</span></span>

* <span data-ttu-id="05b39-109">**A mobil infrastruktúra biztonságossá tétele** a hozzáférés API-kulcsokkal történő korlátozásával, a szolgáltatásmegtagadási támadások szabályozással történő megelőzésével vagy speciális biztonsági házirendek, például a JWT-jogkivonat alapú érvényesítés használatával.</span><span class="sxs-lookup"><span data-stu-id="05b39-109">**Securing mobile infrastructure** by gating access with API keys, preventing DOS attacks by using throttling, or using advanced security policies like JWT token validation.</span></span>
* <span data-ttu-id="05b39-110">**ISV partner ökoszisztéma engedélyezése** gyors partner bevezetési keresztül hello fejlesztői felajánlásával portal és felépítése az API homlokzati toodecouple a belső implementációiból, amelyek nincsenek érett a partner felhasználásához.</span><span class="sxs-lookup"><span data-stu-id="05b39-110">**Enabling ISV partner ecosystems** by offering fast partner onboarding through hello developer portal and building an API facade toodecouple from internal implementations that are not ripe for partner consumption.</span></span>
* <span data-ttu-id="05b39-111">**Egy belső API programot futtat** felajánlásával egy központi helyen hello szervezet toocommunicate hello rendelkezésre állás és a legújabb tooAPIs módosul, változástábla munkahelyi és iskolai fiókok alapján hozzáférésének korlátozásához összes alapján közötti biztonságos csatornán hello API átjáró és a háttérkiszolgáló hello.</span><span class="sxs-lookup"><span data-stu-id="05b39-111">**Running an internal API program** by offering a centralized location for hello organization toocommunicate about hello availability and latest changes tooAPIs, gating access based on organizational accounts, all based on a secured channel between hello API gateway and hello backend.</span></span>

<span data-ttu-id="05b39-112">hello rendszer a következő összetevők hello tevődik össze:</span><span class="sxs-lookup"><span data-stu-id="05b39-112">hello system is made up of hello following components:</span></span>

* <span data-ttu-id="05b39-113">Hello **API átjáró** hello végpontja, amely:</span><span class="sxs-lookup"><span data-stu-id="05b39-113">hello **API gateway** is hello endpoint that:</span></span>
  
  * <span data-ttu-id="05b39-114">API-t hívja, és továbbítja a dokumentumokat tooyour háttérkiszolgálókon fogad el.</span><span class="sxs-lookup"><span data-stu-id="05b39-114">Accepts API calls and routes them tooyour backends.</span></span>
  * <span data-ttu-id="05b39-115">Ellenőrzi az API-kulcsokat, JWT-jogkivonatokat, tanúsítványokat és más hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="05b39-115">Verifies API keys, JWT tokens, certificates, and other credentials.</span></span>
  * <span data-ttu-id="05b39-116">Betartatja a használati kvótákat és a sebességkorlátokat.</span><span class="sxs-lookup"><span data-stu-id="05b39-116">Enforces usage quotas and rate limits.</span></span>
  * <span data-ttu-id="05b39-117">Átalakítja az API-t a hello parancsprogramok kód módosítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="05b39-117">Transforms your API on hello fly without code modifications.</span></span>
  * <span data-ttu-id="05b39-118">Gyorsítótárazza a háttérrendszer válaszait, ahol ez be van állítva.</span><span class="sxs-lookup"><span data-stu-id="05b39-118">Caches backend responses where set up.</span></span>
  * <span data-ttu-id="05b39-119">Elemzési céllal naplózza a hívások metaadatait.</span><span class="sxs-lookup"><span data-stu-id="05b39-119">Logs call metadata for analytics purposes.</span></span>
* <span data-ttu-id="05b39-120">Hello **publisher portal** hello felügyeleti felület, ahol állít be az API program.</span><span class="sxs-lookup"><span data-stu-id="05b39-120">hello **publisher portal** is hello administrative interface where you set up your API program.</span></span> <span data-ttu-id="05b39-121">A következőkre lehet használni:</span><span class="sxs-lookup"><span data-stu-id="05b39-121">Use it to:</span></span>
  
  * <span data-ttu-id="05b39-122">API-séma meghatározása vagy importálása.</span><span class="sxs-lookup"><span data-stu-id="05b39-122">Define or import API schema.</span></span>
  * <span data-ttu-id="05b39-123">API-k termékekbe csomagolása.</span><span class="sxs-lookup"><span data-stu-id="05b39-123">Package APIs into products.</span></span>
  * <span data-ttu-id="05b39-124">Kvóták vagy átalakítások hello API-k a házirendek beállítása.</span><span class="sxs-lookup"><span data-stu-id="05b39-124">Set up policies like quotas or transformations on hello APIs.</span></span>
  * <span data-ttu-id="05b39-125">Elemzések lekérése.</span><span class="sxs-lookup"><span data-stu-id="05b39-125">Get insights from analytics.</span></span>
  * <span data-ttu-id="05b39-126">Felhasználók kezelése.</span><span class="sxs-lookup"><span data-stu-id="05b39-126">Manage users.</span></span>
* <span data-ttu-id="05b39-127">Hello **fejlesztői portálján** funkcionál hello fő webalkalmazás jelenlétét a fejlesztők számára, ahol a következőkre:</span><span class="sxs-lookup"><span data-stu-id="05b39-127">hello **developer portal** serves as hello main web presence for developers, where they can:</span></span>
  
  * <span data-ttu-id="05b39-128">Elolvashatják az API-dokumentációt.</span><span class="sxs-lookup"><span data-stu-id="05b39-128">Read API documentation.</span></span>
  * <span data-ttu-id="05b39-129">Próbálja ki az API-k hello interaktív konzolon keresztül.</span><span class="sxs-lookup"><span data-stu-id="05b39-129">Try out an API via hello interactive console.</span></span>
  * <span data-ttu-id="05b39-130">Hozzon létre egy fiókot, és feliratkozhat tooget API-kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="05b39-130">Create an account and subscribe tooget API keys.</span></span>
  * <span data-ttu-id="05b39-131">Hozzáférhetnek a használat adataikról készült elemzésekhez.</span><span class="sxs-lookup"><span data-stu-id="05b39-131">Access analytics on their own usage.</span></span>

## <span data-ttu-id="05b39-132"><a name="create-service-instance"></a>API Management-példány létrehozása</span><span class="sxs-lookup"><span data-stu-id="05b39-132"><a name="create-service-instance"> </a>Create an API Management instance</span></span>
> [!NOTE]
> <span data-ttu-id="05b39-133">toocomplete ebben az oktatóanyagban egy Azure-fiókra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="05b39-133">toocomplete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="05b39-134">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes fiókot.</span><span class="sxs-lookup"><span data-stu-id="05b39-134">If you don't have an account, you can create a free account in just a couple of minutes.</span></span> <span data-ttu-id="05b39-135">További információ: [Ingyenes Azure-fiók létrehozása][Azure Free Trial].</span><span class="sxs-lookup"><span data-stu-id="05b39-135">For details, see [Azure Free Trial][Azure Free Trial].</span></span>
> 
> 

<span data-ttu-id="05b39-136">az API Management használata első lépéseként hello toocreate egy szolgáltatáspéldány.</span><span class="sxs-lookup"><span data-stu-id="05b39-136">hello first step in working with API Management is toocreate a service instance.</span></span> <span data-ttu-id="05b39-137">Jelentkezzen be toohello [Azure Portal] [ Azure Portal] kattintson **új**, **Web + mobil**, **API Management**.</span><span class="sxs-lookup"><span data-stu-id="05b39-137">Sign in toohello [Azure Portal][Azure Portal] and click **New**, **Web + Mobile**, **API Management**.</span></span>

![Új API Management-példány][api-management-create-instance-menu]

<span data-ttu-id="05b39-139">A **neve**, adjon meg egy egyedi altartomány neve toouse hello szolgáltatás URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="05b39-139">For **Name**, specify a unique sub-domain name toouse for hello service URL.</span></span>

<span data-ttu-id="05b39-140">Válassza ki a kívánt hello **előfizetés**, **erőforráscsoport** és **hely** a szolgáltatáspéldány.</span><span class="sxs-lookup"><span data-stu-id="05b39-140">Choose hello desired **Subscription**, **Resource group** and **Location** for your service instance.</span></span>

<span data-ttu-id="05b39-141">Adja meg **Contoso Ltd** a hello **szervezet neve**, és írja be az e-mail cím hello **rendszergazdai E-Mail** mező.</span><span class="sxs-lookup"><span data-stu-id="05b39-141">Enter **Contoso Ltd.** for hello **Organization Name**, and enter your email address in hello **Administrator E-Mail** field.</span></span>

> [!NOTE]
> <span data-ttu-id="05b39-142">Ez az e-mail cím az API Management rendszer hello értesítésekhez használatos.</span><span class="sxs-lookup"><span data-stu-id="05b39-142">This email address is used for notifications from hello API Management system.</span></span> <span data-ttu-id="05b39-143">További információkért lásd: [hogyan tooconfigure értesítések és az e-mail sablonok az Azure API Management][How tooconfigure notifications and email templates in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="05b39-143">For more information, see [How tooconfigure notifications and email templates in Azure API Management][How tooconfigure notifications and email templates in Azure API Management].</span></span>
> 
> 

![Új API Management szolgáltatás][api-management-create-instance-step1]

<span data-ttu-id="05b39-145">Az API Management szolgáltatáspéldányok három csomagban érhetők el: Fejlesztői, Standard és Prémium.</span><span class="sxs-lookup"><span data-stu-id="05b39-145">API Management service instances are available in three tiers: Developer, Standard, and Premium.</span></span>

> [!NOTE]
> <span data-ttu-id="05b39-146">hello fejlesztői réteg fejlesztési, tesztelési és magas rendelkezésre állás esetén nem szempont kísérleti API programok van.</span><span class="sxs-lookup"><span data-stu-id="05b39-146">hello Developer Tier is for development, testing, and pilot API programs where high availability is not a concern.</span></span> <span data-ttu-id="05b39-147">A hello Standard és prémium csomag szükséges a fenntartott egységek számának toohandle nagyobb forgalmat is méretezhető.</span><span class="sxs-lookup"><span data-stu-id="05b39-147">In hello Standard and Premium tiers, you can scale your reserved unit count toohandle more traffic.</span></span> <span data-ttu-id="05b39-148">hello Standard és Premium rétegek adja meg az API Management hello szolgáltatást, és a legtöbb feldolgozási teljesítmény.</span><span class="sxs-lookup"><span data-stu-id="05b39-148">hello Standard and Premium tiers provide your API Management service with hello most processing power and performance.</span></span> <span data-ttu-id="05b39-149">Jelen oktatóanyagot bármely csomaggal elvégezheti.</span><span class="sxs-lookup"><span data-stu-id="05b39-149">You can complete this tutorial by using any tier.</span></span> <span data-ttu-id="05b39-150">További információt az API Management csomagjairól az [API Management Díjszabás][API Management pricing] oldalon talál.</span><span class="sxs-lookup"><span data-stu-id="05b39-150">For more information about API Management tiers, see [API Management pricing][API Management pricing].</span></span>
> 
> 

<span data-ttu-id="05b39-151">Kattintson a **létrehozása** kiépítése a szolgáltatáspéldány toostart.</span><span class="sxs-lookup"><span data-stu-id="05b39-151">Click **Create** toostart provisioning your service instance.</span></span>

![Új API Management szolgáltatás][api-management-instance-created]

<span data-ttu-id="05b39-153">Hello szolgáltatáspéldány létrehozása után tovább hello toocreate vagy importálja az API-k.</span><span class="sxs-lookup"><span data-stu-id="05b39-153">Once hello service instance is created, hello next step is toocreate or import an API.</span></span>

## <span data-ttu-id="05b39-154"><a name="create-api"></a>API importálása</span><span class="sxs-lookup"><span data-stu-id="05b39-154"><a name="create-api"> </a>Import an API</span></span>
<span data-ttu-id="05b39-155">Az API egy ügyfélalkalmazásokból meghívható műveletkészletből áll.</span><span class="sxs-lookup"><span data-stu-id="05b39-155">An API consists of a set of operations that can be invoked from a client application.</span></span> <span data-ttu-id="05b39-156">API-műveleteket a proxy tooexisting webes szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="05b39-156">API operations are proxied tooexisting web services.</span></span>

<span data-ttu-id="05b39-157">Az API-kat létre lehet hozni (és a műveleteket hozzá lehet adni) manuálisan, vagy importálni is lehet.</span><span class="sxs-lookup"><span data-stu-id="05b39-157">APIs can be created (and operations can be added) manually, or they can be imported.</span></span> <span data-ttu-id="05b39-158">Ebben az oktatóanyagban fogunk importálni egy minta Számológép webes szolgáltatás, a Microsoft által biztosított és az Azure-platformon futó API hello.</span><span class="sxs-lookup"><span data-stu-id="05b39-158">In this tutorial, we will import hello API for a sample calculator web service provided by Microsoft and hosted on Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="05b39-159">Az API-k létrehozására, illetve manuális hozzáadásával a műveletek útmutatóért lásd: [hogyan toocreate API-k](api-management-howto-create-apis.md) és [hogyan tooadd műveletek tooan API](api-management-howto-add-operations.md).</span><span class="sxs-lookup"><span data-stu-id="05b39-159">For guidance on creating an API and manually adding operations, see [How toocreate APIs](api-management-howto-create-apis.md) and [How tooadd operations tooan API](api-management-howto-add-operations.md).</span></span>
> 
> 

<span data-ttu-id="05b39-160">API-k hello publisher portálról vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="05b39-160">APIs are configured from hello publisher portal.</span></span> <span data-ttu-id="05b39-161">tooreach, kattintson a **Publisher portal** hello szolgáltatás eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="05b39-161">tooreach it, click **Publisher portal** from hello service toolbar.</span></span>

![Közzétevő portál][api-management-management-console]

<span data-ttu-id="05b39-163">tooimport hello Számológép API, kattintson a **API-k** a hello **API Management** hello maradt, és válassza a menü **importálási API**.</span><span class="sxs-lookup"><span data-stu-id="05b39-163">tooimport hello calculator API, click **APIs** from hello **API Management** menu on hello left, and then click **Import API**.</span></span>

![API importálása gomb][api-management-import-api]

<span data-ttu-id="05b39-165">Hajtsa végre a következő lépéseket tooconfigure hello Számológép API hello:</span><span class="sxs-lookup"><span data-stu-id="05b39-165">Perform hello following steps tooconfigure hello calculator API:</span></span>

1. <span data-ttu-id="05b39-166">Kattintson a **az URL**, adja meg **http://calcapi.cloudapp.net/calcapi.json** történő hello **Specification dokumentum URL-cím** szöveg mezőbe, majd kattintson a hello **Swagger**  választógombot.</span><span class="sxs-lookup"><span data-stu-id="05b39-166">Click **From URL**, enter **http://calcapi.cloudapp.net/calcapi.json** into hello **Specification document URL** text box, and click hello **Swagger** radio button.</span></span>
2. <span data-ttu-id="05b39-167">Típus **Számológép** hello történő **webes API URL-címe utótag** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="05b39-167">Type **calc** into hello **Web API URL suffix** text box.</span></span>
3. <span data-ttu-id="05b39-168">Kattintson a hello **(nem kötelező) termékek** listát, és válassza **alapszintű**.</span><span class="sxs-lookup"><span data-stu-id="05b39-168">Click in hello **Products (optional)** box and choose **Starter**.</span></span>
4. <span data-ttu-id="05b39-169">Kattintson a **mentése** tooimport hello API.</span><span class="sxs-lookup"><span data-stu-id="05b39-169">Click **Save** tooimport hello API.</span></span>

![Új API hozzáadása][api-management-import-new-api]

> [!NOTE]
> <span data-ttu-id="05b39-171">Az **API Management** jelenleg az 1.2-es és a 2.0-ás verziójú Swagger-dokumentumok importálását is támogatja.</span><span class="sxs-lookup"><span data-stu-id="05b39-171">**API Management** currently supports both 1.2 and 2.0 version of Swagger document for import.</span></span> <span data-ttu-id="05b39-172">Ügyeljen arra, hogy még ha a [Swagger 2.0 specifikációja](http://swagger.io/specification) szerint a `host`, `basePath` és `schemes` tulajdonságok nem is kötelezőek, a Swagger 2.0-ás dokumentumnak akkor is tartalmaznia **KELL** ezeket a tulajdonságokat, máskülönben nem lesz importálva.</span><span class="sxs-lookup"><span data-stu-id="05b39-172">Make sure that, even though [Swagger 2.0 specification](http://swagger.io/specification) declares that `host`, `basePath`, and `schemes` properties are optional, your Swagger 2.0 document **MUST** contain those properties; otherwise it won't get imported.</span></span> 
> 
> 

<span data-ttu-id="05b39-173">Hello API importálása után hello hello API az összefoglalás lapon megjelenik hello publisher portálon.</span><span class="sxs-lookup"><span data-stu-id="05b39-173">Once hello API is imported, hello summary page for hello API is displayed in hello publisher portal.</span></span>

![API összefoglaló][api-management-imported-api-summary]

<span data-ttu-id="05b39-175">hello API szakasz több lapot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="05b39-175">hello API section has several tabs.</span></span> <span data-ttu-id="05b39-176">Hello **összegzés** lap alapvető metrikákat és hello API kapcsolatos információkat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="05b39-176">hello **Summary** tab displays basic metrics and information about hello API.</span></span> <span data-ttu-id="05b39-177">Hello [beállítások](api-management-howto-create-apis.md#configure-api-settings) a lap majdnem használt tooview és Szerkesztés hello konfigurációs API.</span><span class="sxs-lookup"><span data-stu-id="05b39-177">hello [Settings](api-management-howto-create-apis.md#configure-api-settings) tab is used tooview and edit hello configuration for an API.</span></span> <span data-ttu-id="05b39-178">Hello [műveletek](api-management-howto-add-operations.md) lap használt toomanage hello API műveletek esetén.</span><span class="sxs-lookup"><span data-stu-id="05b39-178">hello [Operations](api-management-howto-add-operations.md) tab is used toomanage hello API's operations.</span></span> <span data-ttu-id="05b39-179">Hello **biztonsági** lapon lehet használt tooconfigure átjáró hitelesítés hello háttérkiszolgáló az egyszerű hitelesítés használatával vagy [kölcsönös hitelesítés](api-management-howto-mutual-certificates.md), és tooconfigure [ felhasználói engedélyezési OAuth 2.0 használatával](api-management-howto-oauth2.md).</span><span class="sxs-lookup"><span data-stu-id="05b39-179">hello **Security** tab can be used tooconfigure gateway authentication for hello backend server by using Basic authentication or [mutual certificate authentication](api-management-howto-mutual-certificates.md), and tooconfigure [user authorization by using OAuth 2.0](api-management-howto-oauth2.md).</span></span>  <span data-ttu-id="05b39-180">Hello **problémák** lapon látható az API-kat használó hello fejlesztők által jelentett használt tooview hibák.</span><span class="sxs-lookup"><span data-stu-id="05b39-180">hello **Issues** tab is used tooview issues reported by hello developers who are using your APIs.</span></span> <span data-ttu-id="05b39-181">Hello **termékek** lap, amely tartalmazza az API használt tooconfigure hello termékek.</span><span class="sxs-lookup"><span data-stu-id="05b39-181">hello **Products** tab is used tooconfigure hello products that contain this API.</span></span>

<span data-ttu-id="05b39-182">Alapértelmezés szerint az API Management minden példányához az alábbi két mintatermék jár:</span><span class="sxs-lookup"><span data-stu-id="05b39-182">By default, each API Management instance comes with two sample products:</span></span>

* <span data-ttu-id="05b39-183">**Kezdő**</span><span class="sxs-lookup"><span data-stu-id="05b39-183">**Starter**</span></span>
* <span data-ttu-id="05b39-184">**Korlátlan**</span><span class="sxs-lookup"><span data-stu-id="05b39-184">**Unlimited**</span></span>

<span data-ttu-id="05b39-185">Ebben az oktatóanyagban hello alapvető Számológép API toohello alapszintű termék bővült hello API importálásakor.</span><span class="sxs-lookup"><span data-stu-id="05b39-185">In this tutorial, hello Basic Calculator API was added toohello Starter product when hello API was imported.</span></span>

<span data-ttu-id="05b39-186">Rendelés toomake hívások tooan API fejlesztők először elő kell fizetnie tooa termék tooit hozzáférést biztosít a számukra.</span><span class="sxs-lookup"><span data-stu-id="05b39-186">In order toomake calls tooan API, developers must first subscribe tooa product that gives them access tooit.</span></span> <span data-ttu-id="05b39-187">A fejlesztők hello developer portálon tooproducts kérhet le, vagy rendszergazdák fejlesztők tooproducts hello publisher portálon kérhet le.</span><span class="sxs-lookup"><span data-stu-id="05b39-187">Developers can subscribe tooproducts in hello developer portal, or administrators can subscribe developers tooproducts in hello publisher portal.</span></span> <span data-ttu-id="05b39-188">Rendszergazdaként jelentkezett létrehozása óta hello API Management példány hello az előző lépéseket hello oktatóanyagban úgy, hogy Ön már előfizetett tooevery termék alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="05b39-188">You are an administrator since you created hello API Management instance in hello previous steps in hello tutorial, so you are already subscribed tooevery product by default.</span></span>

## <span data-ttu-id="05b39-189"><a name="call-operation"></a>Művelet hívása hello developer portálról</span><span class="sxs-lookup"><span data-stu-id="05b39-189"><a name="call-operation"> </a>Call an operation from hello developer portal</span></span>
<span data-ttu-id="05b39-190">Műveletek hívható közvetlenül a hello fejlesztői portálján, amely egy kényelmes módszert arra tooview biztosít, és tesztelje az API-k hello működésére.</span><span class="sxs-lookup"><span data-stu-id="05b39-190">Operations can be called directly from hello developer portal, which provides a convenient way tooview and test hello operations of an API.</span></span> <span data-ttu-id="05b39-191">Az oktatóanyag lépésben hello alapvető Számológép API telefonhívásokhoz **két egész számok hozzáadása** műveletet.</span><span class="sxs-lookup"><span data-stu-id="05b39-191">In this tutorial step, you will call hello Basic Calculator API's **Add two integers** operation.</span></span> <span data-ttu-id="05b39-192">Kattintson a **fejlesztői portálján** hello hello menüből hello publisher portál jobb felső.</span><span class="sxs-lookup"><span data-stu-id="05b39-192">Click **Developer portal** from hello menu at hello top right of hello publisher portal.</span></span>

![Fejlesztői portál][api-management-developer-portal-menu]

<span data-ttu-id="05b39-194">Kattintson a **API-k** hello felső menüben, majd kattintson a **alapvető Számológép** toosee hello elérhető műveletek.</span><span class="sxs-lookup"><span data-stu-id="05b39-194">Click **APIs** from hello top menu, and then click **Basic Calculator** toosee hello available operations.</span></span>

![Fejlesztői portál][api-management-developer-portal-calc-api]

<span data-ttu-id="05b39-196">Vegye figyelembe a hello minta leírásokat és paraméterek importált együtt hello API és a műveleteket, dokumentáció hello fejlesztők számára, amely használja ezt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="05b39-196">Note hello sample descriptions and parameters that were imported along with hello API and operations, providing documentation for hello developers that will use this operation.</span></span> <span data-ttu-id="05b39-197">Ezeket a leírásokat a műveletek manuális hozzáadásakor is hozzá lehet adni.</span><span class="sxs-lookup"><span data-stu-id="05b39-197">These descriptions can also be added when operations are added manually.</span></span>

<span data-ttu-id="05b39-198">toocall hello **két egész számok hozzáadása** műveletet, kattintson a **kipróbálás**.</span><span class="sxs-lookup"><span data-stu-id="05b39-198">toocall hello **Add two integers** operation, click **Try it**.</span></span>

![Kipróbálom][api-management-developer-portal-calc-api-console]

<span data-ttu-id="05b39-200">Adjon meg néhány értéket hello paraméterek vagy hello alapértelmezett tartása, és kattintson **küldése**.</span><span class="sxs-lookup"><span data-stu-id="05b39-200">You can enter some values for hello parameters or keep hello defaults, and then click **Send**.</span></span>

![HTTP Get][api-management-invoke-get]

<span data-ttu-id="05b39-202">Egy művelet indítása, miután hello fejlesztői portálján megjeleníti-e a hello **válaszállapot**, hello **válaszfejlécek**, és az egyik **válasz tartalom**.</span><span class="sxs-lookup"><span data-stu-id="05b39-202">After an operation is invoked, hello developer portal displays hello **Response status**, hello **Response headers**, and any **Response content**.</span></span>

![Válasz][api-management-invoke-get-response]

## <span data-ttu-id="05b39-204"><a name="view-analytics"></a>Elemzés megtekintése</span><span class="sxs-lookup"><span data-stu-id="05b39-204"><a name="view-analytics"> </a>View analytics</span></span>
<span data-ttu-id="05b39-205">Alapszintű Számológép, kapcsoló hátsó toohello publisher portal kiválasztásával tooview analytics **kezelése** hello hello menüből hello fejlesztői portál jobb felső.</span><span class="sxs-lookup"><span data-stu-id="05b39-205">tooview analytics for Basic Calculator, switch back toohello publisher portal by selecting **Manage** from hello menu at hello top right of hello developer portal.</span></span>

![Kezelés][api-management-manage-menu]

<span data-ttu-id="05b39-207">hello hello publisher portál alapértelmezett nézet az hello **irányítópult**, amely áttekintést nyújt az API Management-példány.</span><span class="sxs-lookup"><span data-stu-id="05b39-207">hello default view for hello publisher portal is hello **Dashboard**, which provides an overview of your API Management instance.</span></span>

![Irányítópult][api-management-dashboard]

<span data-ttu-id="05b39-209">Rámutatáskor hello egér keresztül hello diagramot **alapvető Számológép** toosee hello adott mérőszámok hello használatáról hello API egy adott időszakra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="05b39-209">Hover hello mouse over hello chart for **Basic Calculator** toosee hello specific metrics for hello usage of hello API for a given time period.</span></span>

> [!NOTE]
> <span data-ttu-id="05b39-210">Ha a diagram összes sort nem talál, váltson vissza toohello fejlesztői portálján és néhány be hello API-hívások, és várjon egy kis ideig, majd térjen vissza toohello irányítópult.</span><span class="sxs-lookup"><span data-stu-id="05b39-210">If you don't see any lines on your chart, switch back toohello developer portal and make some calls into hello API, wait a few moments, and then come back toohello dashboard.</span></span>
> 
> 

<span data-ttu-id="05b39-211">Kattintson a **részleteinek megtekintése** tooview hello összesítő lapján hello API, többek között a hello megjelenő metrikák nagyobb változatát.</span><span class="sxs-lookup"><span data-stu-id="05b39-211">Click **View Details** tooview hello summary page for hello API, including a larger version of hello displayed metrics.</span></span>

![Elemzés][api-management-mouse-over]

![Összefoglalás][api-management-api-summary-metrics]

<span data-ttu-id="05b39-214">Részletes metrikák és jelentések **Analytics** a hello **API Management** hello bal oldali menüben.</span><span class="sxs-lookup"><span data-stu-id="05b39-214">For detailed metrics and reports, click **Analytics** from hello **API Management** menu on hello left.</span></span>

![Áttekintés][api-management-analytics-overview]

<span data-ttu-id="05b39-216">Hello **Analytics** szakasz hello a következő lap négy fület tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="05b39-216">hello **Analytics** section has hello following four tabs:</span></span>

* <span data-ttu-id="05b39-217">**Egy pillanat alatt** összesített használatát és állapotfigyelő metrikák, valamint felső fejlesztők, a felső termékek, a felső API-k és a felső műveletek hello biztosít.</span><span class="sxs-lookup"><span data-stu-id="05b39-217">**At a glance** provides overall usage and health metrics, as well as hello top developers, top products, top APIs, and top operations.</span></span>
* <span data-ttu-id="05b39-218">A **Használat** részletes tájékoztatást biztosít az API-hívásokról és a sávszélességről, földrajzi ábrázolással.</span><span class="sxs-lookup"><span data-stu-id="05b39-218">**Usage** provides an in-depth look at API calls and bandwidth, including a geographical representation.</span></span>
* <span data-ttu-id="05b39-219">Az **Állapot** az állapotkódokkal, a gyorsítótárazás sikerességének mértékével, a válaszidőkkel, illetve az API-k és szolgáltatások válaszidejével foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="05b39-219">**Health** focuses on status codes, cache success rates, response times, and API and service response times.</span></span>
* <span data-ttu-id="05b39-220">**Tevékenység** részletekbe menően tárhatják jelentéseket biztosít az adott tevékenység hello fejlesztői, a termék, a API és a műveletet.</span><span class="sxs-lookup"><span data-stu-id="05b39-220">**Activity** provides reports that drill down on hello specific activity by developer, product, API, and operation.</span></span>

## <span data-ttu-id="05b39-221"><a name="next-steps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="05b39-221"><a name="next-steps"> </a>Next steps</span></span>
* <span data-ttu-id="05b39-222">Ismerje meg, hogyan túl[a a sebességhatárok API védelme](api-management-howto-product-with-rules.md).</span><span class="sxs-lookup"><span data-stu-id="05b39-222">Learn how too[Protect your API with rate limits](api-management-howto-product-with-rules.md).</span></span>

[Azure Free Trial]: http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=api_management_hero_a

[Create an API Management instance]: #create-service-instance
[Create an API]: #create-api
[Add an operation]: #add-operation
[Add hello new API tooa product]: #add-api-to-product
[Subscribe toohello product that contains hello API]: #subscribe
[Call an operation from hello Developer Portal]: #call-operation
[View analytics]: #view-analytics
[Next steps]: #next-steps


[How toomanage developer accounts in Azure API Management]: api-management-howto-create-or-invite-developers.md
[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[How tooconfigure notifications and email templates in Azure API Management]: api-management-howto-configure-notifications.md
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md
[API Management pricing]: http://azure.microsoft.com/pricing/details/api-management/

[Azure Portal]: https://portal.azure.com/

[api-management-management-console]: ./media/api-management-get-started/api-management-management-console.png
[api-management-create-instance-menu]: ./media/api-management-get-started/api-management-create-instance-menu.png
[api-management-create-instance-step1]: ./media/api-management-get-started/api-management-create-instance-step1.png
[api-management-create-instance-step2]: ./media/api-management-get-started/api-management-create-instance-step2.png
[api-management-instance-created]: ./media/api-management-get-started/api-management-instance-created.png
[api-management-import-api]: ./media/api-management-get-started/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-get-started/api-management-import-new-api.png
[api-management-imported-api-summary]: ./media/api-management-get-started/api-management-imported-api-summary.png
[api-management-calc-operations]: ./media/api-management-get-started/api-management-calc-operations.png
[api-management-list-products]: ./media/api-management-get-started/api-management-list-products.png
[api-management-add-api-to-product]: ./media/api-management-get-started/api-management-add-api-to-product.png
[api-management-add-myechoapi-to-product]: ./media/api-management-get-started/api-management-add-myechoapi-to-product.png
[api-management-api-added-to-product]: ./media/api-management-get-started/api-management-api-added-to-product.png
[api-management-developers]: ./media/api-management-get-started/api-management-developers.png
[api-management-add-subscription]: ./media/api-management-get-started/api-management-add-subscription.png
[api-management-add-subscription-window]: ./media/api-management-get-started/api-management-add-subscription-window.png
[api-management-subscription-added]: ./media/api-management-get-started/api-management-subscription-added.png
[api-management-developer-portal-menu]: ./media/api-management-get-started/api-management-developer-portal-menu.png
[api-management-developer-portal-calc-api]: ./media/api-management-get-started/api-management-developer-portal-calc-api.png
[api-management-developer-portal-calc-api-console]: ./media/api-management-get-started/api-management-developer-portal-calc-api-console.png
[api-management-invoke-get]: ./media/api-management-get-started/api-management-invoke-get.png
[api-management-invoke-get-response]: ./media/api-management-get-started/api-management-invoke-get-response.png
[api-management-manage-menu]: ./media/api-management-get-started/api-management-manage-menu.png
[api-management-dashboard]: ./media/api-management-get-started/api-management-dashboard.png

[api-management-add-response]: ./media/api-management-get-started/api-management-add-response.png
[api-management-add-response-window]: ./media/api-management-get-started/api-management-add-response-window.png
[api-management-developer-key]: ./media/api-management-get-started/api-management-developer-key.png
[api-management-mouse-over]: ./media/api-management-get-started/api-management-mouse-over.png
[api-management-api-summary-metrics]: ./media/api-management-get-started/api-management-api-summary-metrics.png
[api-management-analytics-overview]: ./media/api-management-get-started/api-management-analytics-overview.png
[api-management-analytics-usage]: ./media/api-management-get-started/api-management-analytics-usage.png
[api-management-]: ./media/api-management-get-started/api-management-.png
[api-management-]: ./media/api-management-get-started/api-management-.png
