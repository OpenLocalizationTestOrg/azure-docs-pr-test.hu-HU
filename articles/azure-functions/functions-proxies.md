---
title: az Azure Functions proxykkal aaaWork |} Microsoft Docs
description: Hogyan toouse Azure Functions proxyk
services: functions
documentationcenter: 
author: mattchenderson
manager: erikre
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/11/2017
ms.author: mahender
ms.openlocfilehash: 4d94c89e8f8f2d2c771b01bae142bf9a4f3b7f2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="work-with-azure-functions-proxies-preview"></a><span data-ttu-id="6bfb7-103">Együttműködik az Azure Functions proxyk (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="6bfb7-103">Work with Azure Functions Proxies (preview)</span></span>

> [!NOTE] 
> <span data-ttu-id="6bfb7-104">Az Azure Functions proxyk jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-104">Azure Functions Proxies is currently in preview.</span></span> <span data-ttu-id="6bfb7-105">Szabad, amíg preview, de a szabványos függvényekben számlázási tooproxy végrehajtások vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-105">It is free while in preview, but standard Functions billing applies tooproxy executions.</span></span> <span data-ttu-id="6bfb7-106">További információkért lásd: [Azure Functions díjszabási](https://azure.microsoft.com/pricing/details/functions/).</span><span class="sxs-lookup"><span data-stu-id="6bfb7-106">For more information, see [Azure Functions pricing](https://azure.microsoft.com/pricing/details/functions/).</span></span>

<span data-ttu-id="6bfb7-107">Ez a cikk azt ismerteti, hogyan tooconfigure és a munkahelyi Azure Functions proxykkal.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-107">This article explains how tooconfigure and work with Azure Functions Proxies.</span></span> <span data-ttu-id="6bfb7-108">Ez a szolgáltatás végpontok is megadhat, a függvény alkalmazások, amelyeket a rendszer egy másik erőforrás.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-108">With this feature, you can specify endpoints on your function app that are implemented by another resource.</span></span> <span data-ttu-id="6bfb7-109">Ezek proxyk toobreak nagy API több függvény alkalmazásokba (például egy mikroszolgáltatási architektúra), miközben továbbra is egyetlen API felülete ügyfelek is használhatja.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-109">You can use these proxies toobreak a large API into multiple function apps (as in a microservice architecture), while still presenting a single API surface for clients.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]


## <span data-ttu-id="6bfb7-110"><a name="enable"></a>Az Azure Functions proxyk engedélyezése</span><span class="sxs-lookup"><span data-stu-id="6bfb7-110"><a name="enable"></a>Enable Azure Functions Proxies</span></span>

<span data-ttu-id="6bfb7-111">Proxyk alapértelmezés szerint nem engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-111">Proxies are not enabled by default.</span></span> <span data-ttu-id="6bfb7-112">Proxyk hozhat létre, amikor hello szolgáltatás le van tiltva, de nem hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-112">You can create proxies while hello feature is disabled, but they will not execute.</span></span> <span data-ttu-id="6bfb7-113">tooenable proxyk hello a következő:</span><span class="sxs-lookup"><span data-stu-id="6bfb7-113">tooenable proxies, do hello following:</span></span>

1. <span data-ttu-id="6bfb7-114">Nyissa meg hello [Azure-portálon], és folytassa a tooyour függvény alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-114">Open hello [Azure portal], and then go tooyour function app.</span></span>
2. <span data-ttu-id="6bfb7-115">Válassza ki **Alkalmazásbeállítások működéséhez**.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-115">Select **Function app settings**.</span></span>
3. <span data-ttu-id="6bfb7-116">Kapcsoló **engedélyezése Azure Functions proxyk (előzetes verzió)** túl**a**.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-116">Switch **Enable Azure Functions Proxies (preview)** too**On**.</span></span>

<span data-ttu-id="6bfb7-117">Is visszatérhet ide tooupdate hello proxyt futtatókörnyezet amint elérhetővé válnak az új szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-117">You can also return here tooupdate hello proxy runtime as new features become available.</span></span>


## <span data-ttu-id="6bfb7-118"><a name="create"></a>A proxy létrehozása</span><span class="sxs-lookup"><span data-stu-id="6bfb7-118"><a name="create"></a>Create a proxy</span></span>

<span data-ttu-id="6bfb7-119">Ez a szakasz bemutatja, hogyan toocreate proxyval a hello Functions portálra.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-119">This section shows you how toocreate a proxy in hello Functions portal.</span></span>

1. <span data-ttu-id="6bfb7-120">Nyissa meg hello [Azure-portálon], és folytassa a tooyour függvény alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-120">Open hello [Azure portal], and then go tooyour function app.</span></span>
2. <span data-ttu-id="6bfb7-121">Hello bal oldali ablaktáblában jelöljön ki **új proxy**.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-121">In hello left pane, select **New proxy**.</span></span>
3. <span data-ttu-id="6bfb7-122">Adja meg a proxykiszolgáló nevét.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-122">Provide a name for your proxy.</span></span>
4. <span data-ttu-id="6bfb7-123">Hello végpontot, amely fel van fedve konfigurálása az függvény alkalmazás hello megadásával **útvonalsablonhoz** és **HTTP-metódus**.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-123">Configure hello endpoint that's exposed on this function app by specifying hello **route template** and **HTTP methods**.</span></span> <span data-ttu-id="6bfb7-124">Ezek a paraméterek viselkednek függően toohello szabályainak [HTTP eseményindítók].</span><span class="sxs-lookup"><span data-stu-id="6bfb7-124">These parameters behave according toohello rules for [HTTP triggers].</span></span>
5. <span data-ttu-id="6bfb7-125">Set hello **háttérkiszolgáló URL-cím** tooanother végpont.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-125">Set hello **backend URL** tooanother endpoint.</span></span> <span data-ttu-id="6bfb7-126">Ehhez a végponthoz lehet egy másik függvény alkalmazásban függvény, vagy más API-k lehet.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-126">This endpoint could be a function in another function app, or it could be any other API.</span></span> <span data-ttu-id="6bfb7-127">hello érték nem kell toobe statikus, és azt is hivatkozást [Alkalmazásbeállítások] és [hello eredeti ügyfélkérés származó paraméterek].</span><span class="sxs-lookup"><span data-stu-id="6bfb7-127">hello value does not need toobe static, and it can reference [application settings] and [parameters from hello original client request].</span></span>
6. <span data-ttu-id="6bfb7-128">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-128">Click **Create**.</span></span>

<span data-ttu-id="6bfb7-129">A proxy most már létezik a függvény alkalmazás egy új végponton.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-129">Your proxy now exists as a new endpoint on your function app.</span></span> <span data-ttu-id="6bfb7-130">Az ügyfél szempontjából egyenértékű tooan az Azure Functions HttpTrigger.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-130">From a client perspective, it is equivalent tooan HttpTrigger in Azure Functions.</span></span> <span data-ttu-id="6bfb7-131">Az új proxy kipróbálhatja hello Proxy URL-cím másolásával, és vizsgálja, hogy a kedvenc HTTP-ügyféllel.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-131">You can try out your new proxy by copying hello Proxy URL and testing it with your favorite HTTP client.</span></span>

## <span data-ttu-id="6bfb7-132"><a name="modify-requests-responses"></a>Kérelem és válasz módosítása</span><span class="sxs-lookup"><span data-stu-id="6bfb7-132"><a name="modify-requests-responses"></a>Modify requests and responses</span></span>

<span data-ttu-id="6bfb7-133">Az Azure Functions proxykat módosíthatja a kérelmek tooand válaszok hello háttérből.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-133">With Azure Functions Proxies, you can modify requests tooand responses from hello back end.</span></span> <span data-ttu-id="6bfb7-134">Az átalakításokat változókat is használhat, a [változókkal].</span><span class="sxs-lookup"><span data-stu-id="6bfb7-134">These transformations can use variables as defined in [Use variables].</span></span>

### <span data-ttu-id="6bfb7-135"><a name="modify-backend-request"></a>Hello háttér-kérelem módosítása</span><span class="sxs-lookup"><span data-stu-id="6bfb7-135"><a name="modify-backend-request"></a>Modify hello back-end request</span></span>

<span data-ttu-id="6bfb7-136">Alapértelmezés szerint hello háttér-kérelem hello eredeti kérelem másolatként inicializálása.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-136">By default, hello back-end request is initialized as a copy of hello original request.</span></span> <span data-ttu-id="6bfb7-137">Továbbá toosetting hello háttér-URL-címet, hogy módosításokat toohello HTTP-metódus, a fejlécek és a lekérdezési karakterlánc paraméterek.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-137">In addition toosetting hello back-end URL, you can make changes toohello HTTP method, headers, and query string parameters.</span></span> <span data-ttu-id="6bfb7-138">hello módosult értékek hivatkozhatnak [Alkalmazásbeállítások] és [hello eredeti ügyfélkérés származó paraméterek].</span><span class="sxs-lookup"><span data-stu-id="6bfb7-138">hello modified values can reference [application settings] and [parameters from hello original client request].</span></span>

<span data-ttu-id="6bfb7-139">Jelenleg nincs portál élmény, a háttér-kérelmek módosítását.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-139">Currently, there is no portal experience for modifying back-end requests.</span></span> <span data-ttu-id="6bfb7-140">toolearn hogyan tooapply ezt a képességet a proxies.json, lásd: [megadása egy requestOverrides objektum].</span><span class="sxs-lookup"><span data-stu-id="6bfb7-140">toolearn how tooapply this capability from proxies.json, see [Define a requestOverrides object].</span></span>

### <span data-ttu-id="6bfb7-141"><a name="modify-response"></a>Hello válasz módosítása</span><span class="sxs-lookup"><span data-stu-id="6bfb7-141"><a name="modify-response"></a>Modify hello response</span></span>

<span data-ttu-id="6bfb7-142">Alapértelmezés szerint a hello ügyfél válaszára inicializálva van hello háttér-válasz egy másolatát.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-142">By default, hello client response is initialized as a copy of hello back-end response.</span></span> <span data-ttu-id="6bfb7-143">Módosítások toohello válasz állapotkódja, indoklás, fejlécek és body végezheti el.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-143">You can make changes toohello response's status code, reason phrase, headers, and body.</span></span> <span data-ttu-id="6bfb7-144">hello módosult értékek hivatkozhatnak [Alkalmazásbeállítások], [hello eredeti ügyfélkérés származó paraméterek], és [hello háttér-válasz származó paraméterek].</span><span class="sxs-lookup"><span data-stu-id="6bfb7-144">hello modified values can reference [application settings], [parameters from hello original client request], and [parameters from hello back-end response].</span></span>

<span data-ttu-id="6bfb7-145">Jelenleg nincs portál élmény a válaszok módosítását.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-145">Currently, there is no portal experience for modifying responses.</span></span> <span data-ttu-id="6bfb7-146">toolearn hogyan tooapply ezt a képességet a proxies.json, lásd: [megadása egy responseOverrides objektum].</span><span class="sxs-lookup"><span data-stu-id="6bfb7-146">toolearn how tooapply this capability from proxies.json, see [Define a responseOverrides object].</span></span>

## <span data-ttu-id="6bfb7-147"><a name="using-variables"></a>Változók használata</span><span class="sxs-lookup"><span data-stu-id="6bfb7-147"><a name="using-variables"></a>Use variables</span></span>

<span data-ttu-id="6bfb7-148">a proxy konfigurációját hello nem kell toobe statikus.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-148">hello configuration for a proxy does not need toobe static.</span></span> <span data-ttu-id="6bfb7-149">Akkor is a feltétel az toouse változók hello eredeti kérelem, hello háttér-válasz vagy alkalmazás beállításait.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-149">You can condition it toouse variables from hello original request, hello back-end response, or application settings.</span></span>

### <span data-ttu-id="6bfb7-150"><a name="request-parameters"></a>Hivatkozás a kérelemben szereplő paraméterek</span><span class="sxs-lookup"><span data-stu-id="6bfb7-150"><a name="request-parameters"></a>Reference request parameters</span></span>

<span data-ttu-id="6bfb7-151">A kérelemben szereplő paraméterek toohello háttér-URL-cím tulajdonsága bemeneti, illetve a kérelem és válasz módosítása részeként használható.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-151">You can use request parameters as inputs toohello back-end URL property or as part of modifying requests and responses.</span></span> <span data-ttu-id="6bfb7-152">Egyes paraméterek sablonból hello útvonal hello alap proxykonfigurációt megadott köthető, és mások hello bejövő kérelem tulajdonságainak származhatnak.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-152">Some parameters can be bound from hello route template that's specified in hello base proxy configuration, and others can come from properties of hello incoming request.</span></span>

#### <a name="route-template-parameters"></a><span data-ttu-id="6bfb7-153">Útvonal-Sablonparaméterek</span><span class="sxs-lookup"><span data-stu-id="6bfb7-153">Route template parameters</span></span>
<span data-ttu-id="6bfb7-154">Hello útvonalsablonhoz használt paraméterei elérhető toobe hivatkozás a név alapján.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-154">Parameters that are used in hello route template are available toobe referenced by name.</span></span> <span data-ttu-id="6bfb7-155">hello paraméternevei vannak kapcsos zárójelek között ({}).</span><span class="sxs-lookup"><span data-stu-id="6bfb7-155">hello parameter names are enclosed in braces ({}).</span></span>

<span data-ttu-id="6bfb7-156">Például, ha a proxy például rendelkezik egy útvonalsablonhoz `/pets/{petId}`, hello háttér-URL-címet tartalmazhat hello értékének `{petId}`, mint a `https://<AnotherApp>.azurewebsites.net/api/pets/{petId}`.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-156">For example, if a proxy has a route template, such as `/pets/{petId}`, hello back-end URL can include hello value of `{petId}`, as in `https://<AnotherApp>.azurewebsites.net/api/pets/{petId}`.</span></span> <span data-ttu-id="6bfb7-157">Ha hello útvonalsablonhoz megszakítja a helyettesítő karakter, például a `/api/{*restOfPath}`, érték hello `{restOfPath}` egy bejövő hello kérelemből szegmenst fennmaradó hello karakterláncos ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-157">If hello route template terminates in a wildcard, such as `/api/{*restOfPath}`, hello value `{restOfPath}` is a string representation of hello remaining path segments from hello incoming request.</span></span>

#### <a name="additional-request-parameters"></a><span data-ttu-id="6bfb7-158">A további kérelemben szereplő paraméterek</span><span class="sxs-lookup"><span data-stu-id="6bfb7-158">Additional request parameters</span></span>
<span data-ttu-id="6bfb7-159">Ezenkívül toohello útvonal-Sablonparaméterek, konfigurációs értékeire hello a következő értékek használhatók:</span><span class="sxs-lookup"><span data-stu-id="6bfb7-159">In addition toohello route template parameters, hello following values can be used in config values:</span></span>

* <span data-ttu-id="6bfb7-160">**{request.method}** : hello hello eredeti kérés használt HTTP-metódus.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-160">**{request.method}**: hello HTTP method that's used on hello original request.</span></span>
* <span data-ttu-id="6bfb7-161">**{request.headers. \<Fejléc neve\>}**: hello eredeti kérelemből olvasható fejléc.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-161">**{request.headers.\<HeaderName\>}**: A header that can be read from hello original request.</span></span> <span data-ttu-id="6bfb7-162">Cserélje le  *\<fejléc neve\>*  hello névvel, amelyet az tooread hello fejléc.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-162">Replace *\<HeaderName\>* with hello name of hello header that you want tooread.</span></span> <span data-ttu-id="6bfb7-163">Ha hello fejléce nem szerepel-e hello kérésre, hello értéket fogja hello üres karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-163">If hello header is not included on hello request, hello value will be hello empty string.</span></span>
* <span data-ttu-id="6bfb7-164">**{request.querystring. \<ParameterName\>}**: A lekérdezési karakterlánc paraméterként hello eredeti kérelemből olvasható.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-164">**{request.querystring.\<ParameterName\>}**: A query string parameter that can be read from hello original request.</span></span> <span data-ttu-id="6bfb7-165">Cserélje le  *\<ParameterName\>*  hello névvel, amelyet az tooread hello paraméter.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-165">Replace *\<ParameterName\>* with hello name of hello parameter that you want tooread.</span></span> <span data-ttu-id="6bfb7-166">Ha hello paraméter nem tartalmaz hello kérésre, hello értéket fogja hello üres karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-166">If hello parameter is not included on hello request, hello value will be hello empty string.</span></span>

### <span data-ttu-id="6bfb7-167"><a name="response-parameters"></a>Háttér-válasz paraméterek</span><span class="sxs-lookup"><span data-stu-id="6bfb7-167"><a name="response-parameters"></a>Reference back-end response parameters</span></span>

<span data-ttu-id="6bfb7-168">Válasz paraméterek módosítása hello válasz toohello ügyfél részeként használhatók.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-168">Response parameters can be used as part of modifying hello response toohello client.</span></span> <span data-ttu-id="6bfb7-169">a következő értékek hello a konfigurációs értékek használhatók:</span><span class="sxs-lookup"><span data-stu-id="6bfb7-169">hello following values can be used in config values:</span></span>

* <span data-ttu-id="6bfb7-170">**{backend.response.statusCode}** : hello háttér-válasz visszaadott HTTP-állapotkód: hello.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-170">**{backend.response.statusCode}**: hello HTTP status code that's returned on hello back-end response.</span></span>
* <span data-ttu-id="6bfb7-171">**{backend.response.statusReason}** : hello HTTP indoklás, amely akkor adja vissza a hello háttér-válasz.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-171">**{backend.response.statusReason}**: hello HTTP reason phrase that's returned on hello back-end response.</span></span>
* <span data-ttu-id="6bfb7-172">**{backend.response.headers. \<Fejléc neve\>}**: egy fejléc a hello háttér-válaszban olvasható.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-172">**{backend.response.headers.\<HeaderName\>}**: A header that can be read from hello back-end response.</span></span> <span data-ttu-id="6bfb7-173">Cserélje le  *\<fejléc neve\>*  hello fejléc hello nevű tooread szeretné.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-173">Replace *\<HeaderName\>* with hello name of hello header you want tooread.</span></span> <span data-ttu-id="6bfb7-174">Ha hello fejléce nem szerepel-e hello kérésre, hello értéket fogja hello üres karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-174">If hello header is not included on hello request, hello value will be hello empty string.</span></span>

### <span data-ttu-id="6bfb7-175"><a name="use-appsettings"></a>Referencia-Alkalmazásbeállítások</span><span class="sxs-lookup"><span data-stu-id="6bfb7-175"><a name="use-appsettings"></a>Reference application settings</span></span>

<span data-ttu-id="6bfb7-176">Is hivatkozhat [hello függvény alkalmazás definiált Alkalmazásbeállítások](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#develop) által körülvevő hello beállítás nevét százalékjelek (%).</span><span class="sxs-lookup"><span data-stu-id="6bfb7-176">You can also reference [application settings defined for hello function app](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#develop) by surrounding hello setting name with percent signs (%).</span></span>

<span data-ttu-id="6bfb7-177">Például a háttér-URL-t *https://%ORDER_PROCESSING_HOST%/api/orders* kellene "% ORDER_PROCESSING_HOST %" hello ORDER_PROCESSING_HOST beállítási hello érték helyett.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-177">For example, a back-end URL of *https://%ORDER_PROCESSING_HOST%/api/orders* would have "%ORDER_PROCESSING_HOST%" replaced with hello value of hello ORDER_PROCESSING_HOST setting.</span></span>

> [!TIP] 
> <span data-ttu-id="6bfb7-178">Háttér-gazdagépek Alkalmazásbeállítások használni, ha több központi telepítését vagy tesztelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-178">Use application settings for back-end hosts when you have multiple deployments or test environments.</span></span> <span data-ttu-id="6bfb7-179">Így biztosíthatja, hogy mindig beszélünk toohello jobb vissza az adott környezet befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-179">That way, you can make sure that you are always talking toohello right back end for that environment.</span></span>

## <a name="advanced-configuration"></a><span data-ttu-id="6bfb7-180">Speciális konfiguráció</span><span class="sxs-lookup"><span data-stu-id="6bfb7-180">Advanced configuration</span></span>

<span data-ttu-id="6bfb7-181">hello proxyk konfigurált függvény app könyvtár hello gyökerében található proxies.json fájlban tárolódnak.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-181">hello proxies that you configure are stored in a proxies.json file, which is located in hello root of a function app directory.</span></span> <span data-ttu-id="6bfb7-182">Manuálisan szerkessze a fájlt és központi telepítése az alkalmazás részeként hello használatakor [telepítési módszerek](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment) a Functions támogatja.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-182">You can manually edit this file and deploy it as part of your app when you use any of hello [deployment methods](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment) that Functions supports.</span></span> <span data-ttu-id="6bfb7-183">hello szolgáltatást kell [engedélyezett](#enable) a hello fájl toobe feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-183">hello feature must be [enabled](#enable) for hello file toobe processed.</span></span> 

> [!TIP] 
> <span data-ttu-id="6bfb7-184">Ha nem állított be hello központi telepítési módszerekkel, akkor is hello proxies.json fájl használatához hello portálon.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-184">If you have not set up one of hello deployment methods, you can also work with hello proxies.json file in hello portal.</span></span> <span data-ttu-id="6bfb7-185">Nyissa meg tooyour függvény alkalmazást, jelölje be **Platform funkciói**, majd válassza ki **App Service-szerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-185">Go tooyour function app, select **Platform features**, and then select **App Service Editor**.</span></span> <span data-ttu-id="6bfb7-186">Ezzel a módszerrel hello teljes fájlstruktúra függvény alkalmazása megtekintheti és majd a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-186">By doing so, you can view hello entire file structure of your function app and then make changes.</span></span>

<span data-ttu-id="6bfb7-187">Proxies.JSON határozza meg a proxyk objektum, amely megnevezett proxyk és a definíciójukat.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-187">Proxies.json is defined by a proxies object, which is composed of named proxies and their definitions.</span></span> <span data-ttu-id="6bfb7-188">Nem kötelező, ha a szerkesztő lehetővé teszi, melyeket referenciaként használhat egy [JSON-séma](http://json.schemastore.org/proxies) kód befejezésére.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-188">Optionally, if your editor supports it, you can reference a [JSON schema](http://json.schemastore.org/proxies) for code completion.</span></span> <span data-ttu-id="6bfb7-189">Egy példa fájl hello következő nézhet ki:</span><span class="sxs-lookup"><span data-stu-id="6bfb7-189">An example file might look like hello following:</span></span>

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "backendUri": "https://<AnotherApp>.azurewebsites.net/api/<FunctionName>"
        }
    }
}
```

<span data-ttu-id="6bfb7-190">Minden egyes proxy van egy rövid nevet, például a *proxy1* a fenti példa hello.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-190">Each proxy has a friendly name, such as *proxy1* in hello preceding example.</span></span> <span data-ttu-id="6bfb7-191">hello megfelelő proxy definíciós objektumban hello a következő tulajdonságok határozzák meg:</span><span class="sxs-lookup"><span data-stu-id="6bfb7-191">hello corresponding proxy definition object is defined by hello following properties:</span></span>

* <span data-ttu-id="6bfb7-192">**matchCondition**: szükséges – az objektum meghatározása, amelynek hatására a proxy hello végrehajtásának hello kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-192">**matchCondition**: Required--an object defining hello requests that trigger hello execution of this proxy.</span></span> <span data-ttu-id="6bfb7-193">A megosztott két tulajdonságok tartalmaz [HTTP eseményindítók]:</span><span class="sxs-lookup"><span data-stu-id="6bfb7-193">It contains two properties that are shared with [HTTP triggers]:</span></span>
    * <span data-ttu-id="6bfb7-194">_módszerek_: hello HTTP hello proxy módszerek tömb válaszol-e.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-194">_methods_: An array of hello HTTP methods that hello proxy responds to.</span></span> <span data-ttu-id="6bfb7-195">Ha nincs megadva, hello proxy válaszol a HTTP-metódus tooall hello útvonalon.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-195">If it is not specified, hello proxy responds tooall HTTP methods on hello route.</span></span>
    * <span data-ttu-id="6bfb7-196">_útvonal_: szükséges – hello útvonalsablonhoz, amely kérelem URL-címeket a proxy szabályozása meghatározása válaszol-e.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-196">_route_: Required--defines hello route template, controlling which request URLs your proxy responds to.</span></span> <span data-ttu-id="6bfb7-197">Ellentétben a HTTP-eseményindítók, nincs alapértelmezett értéke.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-197">Unlike in HTTP triggers, there is no default value.</span></span>
* <span data-ttu-id="6bfb7-198">**backendUri**: hello URL-cím hello háttér-erőforrás toowhich hello kérelem küldése a proxyn keresztül kell lennie.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-198">**backendUri**: hello URL of hello back-end resource toowhich hello request should be proxied.</span></span> <span data-ttu-id="6bfb7-199">Ez az érték hivatkozhat Alkalmazásbeállítások és paraméterek hello eredeti ügyfél kérelemből.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-199">This value can reference application settings and parameters from hello original client request.</span></span> <span data-ttu-id="6bfb7-200">Ha ez a tulajdonság nincs megadva, az Azure Functions válaszol egy HTTP 200 OK gombra.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-200">If this property is not included, Azure Functions responds with an HTTP 200 OK.</span></span>
* <span data-ttu-id="6bfb7-201">**requestOverrides**: átalakítások toohello háttér-kérelem definiáló objektum.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-201">**requestOverrides**: An object that defines transformations toohello back-end request.</span></span> <span data-ttu-id="6bfb7-202">Lásd: [megadása egy requestOverrides objektum].</span><span class="sxs-lookup"><span data-stu-id="6bfb7-202">See [Define a requestOverrides object].</span></span>
* <span data-ttu-id="6bfb7-203">**responseOverrides**: átalakítások toohello ügyfél válaszára definiáló objektum.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-203">**responseOverrides**: An object that defines transformations toohello client response.</span></span> <span data-ttu-id="6bfb7-204">Lásd: [megadása egy responseOverrides objektum].</span><span class="sxs-lookup"><span data-stu-id="6bfb7-204">See [Define a responseOverrides object].</span></span>

> [!NOTE] 
> <span data-ttu-id="6bfb7-205">hello útvonal tulajdonság Azure Functions proxyk nem veszi figyelembe hello routePrefix tulajdonsága hello funkciók a gazdagép konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-205">hello route property Azure Functions Proxies does not honor hello routePrefix property of hello Functions host configuration.</span></span> <span data-ttu-id="6bfb7-206">Ha azt szeretné, hogy egy címelőtagot, például /api tooinclude, akkor hello útvonal tulajdonság kell szerepelnie.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-206">If you want tooinclude a prefix such as /api, it must be included in hello route property.</span></span>

### <span data-ttu-id="6bfb7-207"><a name="requestOverrides"></a>Adja meg a requestOverrides objektum</span><span class="sxs-lookup"><span data-stu-id="6bfb7-207"><a name="requestOverrides"></a>Define a requestOverrides object</span></span>

<span data-ttu-id="6bfb7-208">hello requestOverrides objektum meghatározása változások toohello kérelem hello háttér-erőforrás neve.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-208">hello requestOverrides object defines changes made toohello request when hello back-end resource is called.</span></span> <span data-ttu-id="6bfb7-209">hello objektum következő tulajdonságai hello határozzák meg:</span><span class="sxs-lookup"><span data-stu-id="6bfb7-209">hello object is defined by hello following properties:</span></span>

* <span data-ttu-id="6bfb7-210">**backend.Request.Method**: hello által használt toocall hello háttér HTTP-metódus.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-210">**backend.request.method**: hello HTTP method that's used toocall hello back end.</span></span>
* <span data-ttu-id="6bfb7-211">**backend.Request.QueryString. \<ParameterName\>**: A lekérdezési karakterlánc paraméterként, amely hello hívás toohello háttér állítható be.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-211">**backend.request.querystring.\<ParameterName\>**: A query string parameter that can be set for hello call toohello back end.</span></span> <span data-ttu-id="6bfb7-212">Cserélje le  *\<ParameterName\>*  hello névvel, amelyet az tooset hello paraméter.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-212">Replace *\<ParameterName\>* with hello name of hello parameter that you want tooset.</span></span> <span data-ttu-id="6bfb7-213">Ha üres karakterláncot hello áll rendelkezésre, hello paraméter hello háttér-kérelem nem tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-213">If hello empty string is provided, hello parameter is not included on hello back-end request.</span></span>
* <span data-ttu-id="6bfb7-214">**backend.Request.Headers. \<Fejléc neve\>**: hello hívás toohello háttér beállítható fejléc.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-214">**backend.request.headers.\<HeaderName\>**: A header that can be set for hello call toohello back end.</span></span> <span data-ttu-id="6bfb7-215">Cserélje le  *\<fejléc neve\>*  hello névvel, amelyet az tooset hello fejléc.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-215">Replace *\<HeaderName\>* with hello name of hello header that you want tooset.</span></span> <span data-ttu-id="6bfb7-216">Hello üres karakterláncot adjon meg, ha hello fejléce nem szerepel-e hello háttér-kérelemre.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-216">If you provide hello empty string, hello header is not included on hello back-end request.</span></span>

<span data-ttu-id="6bfb7-217">Értékek hivatkozhatnak Alkalmazásbeállítások és paraméterek hello eredeti ügyfél kérelemből.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-217">Values can reference application settings and parameters from hello original client request.</span></span>

<span data-ttu-id="6bfb7-218">Hello következő egy példa konfiguráció látható:</span><span class="sxs-lookup"><span data-stu-id="6bfb7-218">An example configuration might look like hello following:</span></span>

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "backendUri": "https://<AnotherApp>.azurewebsites.net/api/<FunctionName>",
            "requestOverrides": {
                "backend.request.headers.Accept": "application/xml",
                "backend.request.headers.x-functions-key": "%ANOTHERAPP_API_KEY%"
            }
        }
    }
}
```

### <span data-ttu-id="6bfb7-219"><a name="responseOverrides"></a>Adja meg a responseOverrides objektum</span><span class="sxs-lookup"><span data-stu-id="6bfb7-219"><a name="responseOverrides"></a>Define a responseOverrides object</span></span>

<span data-ttu-id="6bfb7-220">hello requestOverrides objektum, amely megfelelt hátsó toohello ügyfél toohello válasz végzett módosításokat határozza meg.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-220">hello requestOverrides object defines changes that are made toohello response that's passed back toohello client.</span></span> <span data-ttu-id="6bfb7-221">hello objektum következő tulajdonságai hello határozzák meg:</span><span class="sxs-lookup"><span data-stu-id="6bfb7-221">hello object is defined by hello following properties:</span></span>

* <span data-ttu-id="6bfb7-222">**response.statusCode**: hello HTTP-állapot kódját toobe toohello ügyfél adott vissza.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-222">**response.statusCode**: hello HTTP status code toobe returned toohello client.</span></span>
* <span data-ttu-id="6bfb7-223">**response.statusReason**: hello HTTP OK kifejezés toobe toohello ügyfél adott vissza.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-223">**response.statusReason**: hello HTTP reason phrase toobe returned toohello client.</span></span>
* <span data-ttu-id="6bfb7-224">**Response.body**: hello karakterláncos ábrázolása törzsében toobe hello toohello ügyfél adott vissza.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-224">**response.body**: hello string representation of hello body toobe returned toohello client.</span></span>
* <span data-ttu-id="6bfb7-225">**Response.Headers. \<Fejléc neve\>**: hello válasz toohello ügyfél beállítható fejléc.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-225">**response.headers.\<HeaderName\>**: A header that can be set for hello response toohello client.</span></span> <span data-ttu-id="6bfb7-226">Cserélje le  *\<fejléc neve\>*  hello névvel, amelyet az tooset hello fejléc.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-226">Replace *\<HeaderName\>* with hello name of hello header that you want tooset.</span></span> <span data-ttu-id="6bfb7-227">Hello üres karakterláncot adjon meg, ha hello fejléce nem hello válasz szerepel.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-227">If you provide hello empty string, hello header is not included on hello response.</span></span>

<span data-ttu-id="6bfb7-228">Értékek hivatkozhatnak alkalmazás beállításait, a paraméterek kérelemből hello eredeti ügyfél és a paraméterek a hello háttér-válaszban.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-228">Values can reference application settings, parameters from hello original client request, and parameters from hello back-end response.</span></span>

<span data-ttu-id="6bfb7-229">Hello következő egy példa konfiguráció látható:</span><span class="sxs-lookup"><span data-stu-id="6bfb7-229">An example configuration might look like hello following:</span></span>

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "responseOverrides": {
                "response.body": "Hello, {test}",
                "response.headers.Content-Type": "text/plain"
            }
        }
    }
}
```
> [!NOTE] 
> <span data-ttu-id="6bfb7-230">Ebben a példában hello törzs beállítása közvetlenül, ezért nem `backendUri` tulajdonság van szükség.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-230">In this example, hello body is being set directly, so no `backendUri` property is needed.</span></span> <span data-ttu-id="6bfb7-231">hello példa bemutatja, hogyan használhatja Azure Functions proxyk mocking API-k esetében.</span><span class="sxs-lookup"><span data-stu-id="6bfb7-231">hello example shows how you might use Azure Functions Proxies for mocking APIs.</span></span>

[Azure-portálon]: https://portal.azure.com
[HTTP eseményindítók]: https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook#http-trigger
[Modify hello back-end request]: #modify-backend-request
[Modify hello response]: #modify-response
[megadása egy requestOverrides objektum]: #requestOverrides
[megadása egy responseOverrides objektum]: #responseOverrides
[Alkalmazásbeállítások]: #use-appsettings
[változókkal]: #using-variables
[hello eredeti ügyfélkérés származó paraméterek]: #request-parameters
[hello háttér-válasz származó paraméterek]: #response-parameters
