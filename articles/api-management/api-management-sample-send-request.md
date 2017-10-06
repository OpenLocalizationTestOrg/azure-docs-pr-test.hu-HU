---
title: "aaaUsing API-kezelés szolgáltatás toogenerate HTTP-kérelmek"
description: "Ismerje meg az API Management toocall külső szolgáltatásai az API-kérelem-válasz házirendek toouse"
services: api-management
documentationcenter: 
author: darrelmiller
manager: erikre
editor: 
ms.assetid: 4539c0fa-21ef-4b1c-a1d4-d89a38c242fa
ms.service: api-management
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 8002ee453057513340328d99f298703c3b3a9531
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-external-services-from-hello-azure-api-management-service"></a><span data-ttu-id="6c429-103">Az Azure API Management szolgáltatás hello külső szolgáltatások használata</span><span class="sxs-lookup"><span data-stu-id="6c429-103">Using external services from hello Azure API Management service</span></span>
<span data-ttu-id="6c429-104">hello házirendek az Azure API Management service-ben rendelkezésre álló számos különböző probléma pusztán a hello bejövő kérelem, hello kimenő válasz és alapvető konfigurációs adatai alapján hasznos munkahelyi teheti meg.</span><span class="sxs-lookup"><span data-stu-id="6c429-104">hello policies available in Azure API Management service can do a wide range of useful work based purely on hello incoming request, hello outgoing response and basic configuration information.</span></span> <span data-ttu-id="6c429-105">Azonban az API Management külső szolgáltatásokkal képes toointeract alatt számos további lehetőségek megnyílik házirendek.</span><span class="sxs-lookup"><span data-stu-id="6c429-105">However, being able toointeract with external services from API Management policies opens up many more opportunities.</span></span>

<span data-ttu-id="6c429-106">Korábban úgy találtuk, hogy azt léphetnek interakcióba hello [Azure Event Hubs szolgáltatás naplózási, megfigyelésének és elemzés](api-management-log-to-eventhub-sample.md).</span><span class="sxs-lookup"><span data-stu-id="6c429-106">We have previously seen how we can interact with hello [Azure Event Hub service for logging, monitoring and analytics](api-management-log-to-eventhub-sample.md).</span></span> <span data-ttu-id="6c429-107">Ebben a cikkben a házirendekben, amelyek lehetővé teszik toointeract bármely külső HTTP-alapú szolgáltatás bemutatjuk.</span><span class="sxs-lookup"><span data-stu-id="6c429-107">In this article we will demonstrate policies that allow you toointeract with any external HTTP based service.</span></span> <span data-ttu-id="6c429-108">Ezek a házirendek indítására, távoli események vagy az információt, amely használt toomanipulate hello eredeti kérelem-válasz valamilyen módon lesz használható.</span><span class="sxs-lookup"><span data-stu-id="6c429-108">These policies can be used for triggering remote events or for retrieving information that will be used toomanipulate hello original request and response in some way.</span></span>

## <a name="send-one-way-request"></a><span data-ttu-id="6c429-109">Küldési-egy-módon-kérelmek</span><span class="sxs-lookup"><span data-stu-id="6c429-109">Send-One-Way-Request</span></span>
<span data-ttu-id="6c429-110">Valószínűleg hello legegyszerűbb külső beavatkozásra hello kérésére, amely lehetővé teszi, hogy egy külső szolgáltatás toobe fontos esemény valamilyen értesítés tűz-és-elfelejti stílusát.</span><span class="sxs-lookup"><span data-stu-id="6c429-110">Possibly hello simplest external interaction is hello fire-and-forget style of request that allows an external service toobe notified of some kind of important event.</span></span> <span data-ttu-id="6c429-111">Használhatunk hello vezérlő adatfolyam házirend `choose` semmilyen feltételt, azt is érdeklődik, és ezután Ha hello feltétel teljesül, tökéletesíthetjük hello használata külső HTTP-kérelem toodetect [küldési-egy-módon-kérelmek](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) házirend.</span><span class="sxs-lookup"><span data-stu-id="6c429-111">We can use hello control flow policy `choose` toodetect any kind of condition that we are interested in and then, if hello condition is satisfied, we can make an external HTTP request using hello [send-one-way-request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) policy.</span></span> <span data-ttu-id="6c429-112">Ennek oka lehet egy kérelem tooa rendszer Hipchat vagy a Slackhez, illetve egy levelezési API SendGrid vagy MailChimp például üzenetküldési, vagy a kritikus fontosságú támogatási incidensek az alábbihoz hasonló PagerDuty.</span><span class="sxs-lookup"><span data-stu-id="6c429-112">This could be a request tooa messaging system like Hipchat or Slack, or a mail API like SendGrid or MailChimp, or for critical support incidents something like PagerDuty.</span></span> <span data-ttu-id="6c429-113">E üzenetkezelési rendszerek rendelkezik egyszerű HTTP API-k könnyen hívhat meg azt.</span><span class="sxs-lookup"><span data-stu-id="6c429-113">All of these messaging systems have simple HTTP APIs that we can easily invoke.</span></span>

### <a name="alerting-with-slack"></a><span data-ttu-id="6c429-114">A Slackhez riasztás</span><span class="sxs-lookup"><span data-stu-id="6c429-114">Alerting with Slack</span></span>
<span data-ttu-id="6c429-115">hello a következő példa bemutatja, hogyan toosend egy üzenet tooa Csevegés hely Slack-Ha hello HTTP-válasz állapotkódja érték nagyobb, mint vagy egyenlő too500.</span><span class="sxs-lookup"><span data-stu-id="6c429-115">hello following example demonstrates how toosend a message tooa Slack chat room if hello HTTP response status code is greater than or equal too500.</span></span> <span data-ttu-id="6c429-116">Egy 500 tartomány hiba azt jelzi, hogy a háttér-API, amely az API felületen ügyfelének hello problémáját nem maguktól megoldódhatnak.</span><span class="sxs-lookup"><span data-stu-id="6c429-116">A 500 range error indicates a problem with our backend API that hello client of our API cannot resolve themselves.</span></span> <span data-ttu-id="6c429-117">Általában szüksége van valamilyen beavatkozás az eszközeiken.</span><span class="sxs-lookup"><span data-stu-id="6c429-117">It usually requires some kind of intervention on our part.</span></span>  

```xml
<choose>
    <when condition="@(context.Response.StatusCode >= 500)">
      <send-one-way-request mode="new">
        <set-url>https://hooks.slack.com/services/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</set-url>
        <set-method>POST</set-method>
        <set-body>@{
                return new JObject(
                        new JProperty("username","APIM Alert"),
                        new JProperty("icon_emoji", ":ghost:"),
                        new JProperty("text", String.Format("{0} {1}\nHost: {2}\n{3} {4}\n User: {5}",
                                                context.Request.Method,
                                                context.Request.Url.Path + context.Request.Url.QueryString,
                                                context.Request.Url.Host,
                                                context.Response.StatusCode,
                                                context.Response.StatusReason,
                                                context.User.Email
                                                ))
                        ).ToString();
            }</set-body>
      </send-one-way-request>
    </when>
</choose>
```

<span data-ttu-id="6c429-118">Slackhez rendelkezik a bejövő webes hurkok hello fogalmát.</span><span class="sxs-lookup"><span data-stu-id="6c429-118">Slack has hello notion of inbound web hooks.</span></span> <span data-ttu-id="6c429-119">Egy bejövő webes hook konfigurálásakor a Slackhez egy különleges URL-címet, amely lehetővé teszi egy egyszerű POST kérelem toodo és toopass üzenet hello közzététele a Slack-csatorna a állít elő.</span><span class="sxs-lookup"><span data-stu-id="6c429-119">When configuring an inbound web hook, Slack generates a special URL which allows you toodo a simple POST request and toopass a message into hello Slack channel.</span></span> <span data-ttu-id="6c429-120">hello létrehozhatunk JSON-törzsére Slackhez által meghatározott formátumban alapul.</span><span class="sxs-lookup"><span data-stu-id="6c429-120">hello JSON body that we create is based on a format defined by Slack.</span></span>

![Slack webes Hook](./media/api-management-sample-send-request/api-management-slack-webhook.png)

### <a name="is-fire-and-forget-good-enough"></a><span data-ttu-id="6c429-122">Tűz és elég jól elfelejti?</span><span class="sxs-lookup"><span data-stu-id="6c429-122">Is fire and forget good enough?</span></span>
<span data-ttu-id="6c429-123">Nincsenek bizonyos mellékhatásokkal kérelem tűz-és-elfelejti stílus használatával.</span><span class="sxs-lookup"><span data-stu-id="6c429-123">There are certain tradeoffs when using a fire-and-forget style of request.</span></span> <span data-ttu-id="6c429-124">Ha valamilyen okból, hello kérelem sikertelen lesz, majd hello hibája nem fog szerepelni.</span><span class="sxs-lookup"><span data-stu-id="6c429-124">If for some reason, hello request fails, then hello failure will not be reported.</span></span> <span data-ttu-id="6c429-125">Ez az adott esetben nem igazolható hello összetettségét, hogy a másodlagos hibájának jelentéskészítő rendszer és az hello további teljesítményarányos költsége hello válaszra való várakozás közben.</span><span class="sxs-lookup"><span data-stu-id="6c429-125">In this particular situation, hello complexity of having a secondary failure reporting system and hello additional performance cost of waiting for hello response is not warranted.</span></span> <span data-ttu-id="6c429-126">A forgatókönyvekben, ahol az alapvető toocheck hello választ, majd hello [küldési-kérelmek](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) házirend jobb megoldás.</span><span class="sxs-lookup"><span data-stu-id="6c429-126">For scenarios where it is essential toocheck hello response, then hello [send-request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) policy is a better option.</span></span>

## <a name="send-request"></a><span data-ttu-id="6c429-127">Küldési-kérelmek</span><span class="sxs-lookup"><span data-stu-id="6c429-127">Send-Request</span></span>
<span data-ttu-id="6c429-128">Hello `send-request` házirend lehetővé teszi, hogy egy külső szolgáltatás tooperform összetett feldolgozási funkciók és további házirend feldolgozásának nem használható visszatérési adatainak toohello API felügyeleti szolgáltatás használatával.</span><span class="sxs-lookup"><span data-stu-id="6c429-128">hello `send-request` policy enables using an external service tooperform complex processing functions and return data toohello API management service that can be used for further policy processing.</span></span>

### <a name="authorizing-reference-tokens"></a><span data-ttu-id="6c429-129">Referencia-tokenek engedélyezése</span><span class="sxs-lookup"><span data-stu-id="6c429-129">Authorizing reference tokens</span></span>
<span data-ttu-id="6c429-130">Az API Management főbb funkcióját háttér erőforrások védi.</span><span class="sxs-lookup"><span data-stu-id="6c429-130">A major function of API Management is protecting backend resources.</span></span> <span data-ttu-id="6c429-131">Ha létrehozza az API által használt hello engedélyezési kiszolgáló [JWT-jogkivonatokat](http://jwt.io/) az OAuth2 folyamat részeként, [Azure Active Directory](../active-directory/active-directory-aadconnect.md) Igen, akkor használhatja a hello `validate-jwt` házirend tooverify hello érvényességét hello lexikális eleme.</span><span class="sxs-lookup"><span data-stu-id="6c429-131">If hello authorization server used by your API creates [JWT tokens](http://jwt.io/) as part of its OAuth2 flow, as [Azure Active Directory](../active-directory/active-directory-aadconnect.md) does, then you can use hello `validate-jwt` policy tooverify hello validity of hello token.</span></span> <span data-ttu-id="6c429-132">Azonban néhány engedélyezési kiszolgálók létrehozása, mi nevezzük [jogkivonatok hivatkozhat](http://leastprivilege.com/2015/11/25/reference-tokens-and-introspection/) anélkül, hogy a hívás hátsó toohello engedélyezési kiszolgáló, amely nem ellenőrizhető.</span><span class="sxs-lookup"><span data-stu-id="6c429-132">However, some authorization servers create what are called [reference tokens](http://leastprivilege.com/2015/11/25/reference-tokens-and-introspection/) that cannot be verified without making a call back toohello authorization server.</span></span>

### <a name="standardized-introspection"></a><span data-ttu-id="6c429-133">Szabványos introspection</span><span class="sxs-lookup"><span data-stu-id="6c429-133">Standardized introspection</span></span>
<span data-ttu-id="6c429-134">Az elmúlt hello nincs szabványosított lehetőség van egy engedélyezési kiszolgáló és a hivatkozás-jogkivonat ellenőrzése le lett.</span><span class="sxs-lookup"><span data-stu-id="6c429-134">In hello past there has been no standardized way of verifying a reference token with an authorization server.</span></span> <span data-ttu-id="6c429-135">Azonban a közelmúltban javasolt szabvány [RFC 7662](https://tools.ietf.org/html/rfc7662) hello IETF, amely meghatározza, hogyan ellenőrizheti egy erőforrás-kiszolgáló, a token érvényességének hello tett közzé.</span><span class="sxs-lookup"><span data-stu-id="6c429-135">However a recently proposed standard [RFC 7662](https://tools.ietf.org/html/rfc7662) was published by hello IETF that defines how a resource server can verify hello validity of a token.</span></span>

### <a name="extracting-hello-token"></a><span data-ttu-id="6c429-136">Hello jogkivonat beolvasása</span><span class="sxs-lookup"><span data-stu-id="6c429-136">Extracting hello token</span></span>
<span data-ttu-id="6c429-137">hello első lépéseként tooextract hello jogkivonatot hello engedélyezési fejléc.</span><span class="sxs-lookup"><span data-stu-id="6c429-137">hello first step is tooextract hello token from hello Authorization header.</span></span> <span data-ttu-id="6c429-138">hello állomásfejléc-érték formátumban kell lennie a hello `Bearer` engedélyezési sémát, egy szóközzel és majd hello engedélyezési jogkivonatot megfelelően [RFC 6750](http://tools.ietf.org/html/rfc6750#section-2.1).</span><span class="sxs-lookup"><span data-stu-id="6c429-138">hello header value should be formatted with hello `Bearer` authorization scheme, a single space and then hello authorization token as per [RFC 6750](http://tools.ietf.org/html/rfc6750#section-2.1).</span></span> <span data-ttu-id="6c429-139">Sajnos előfordulhatnak olyan esetek, ahol hello hitelesítési séma meg van adva.</span><span class="sxs-lookup"><span data-stu-id="6c429-139">Unfortunately there are cases where hello authorization scheme is omitted.</span></span> <span data-ttu-id="6c429-140">a tooaccount elemzésekor, azt vágási hello Fejlécérték egy helyet, és válassza hello utolsó karakterláncnak a következőről hello karakterláncokból álló tömböt adott vissza.</span><span class="sxs-lookup"><span data-stu-id="6c429-140">tooaccount for this when parsing, we split hello header value on a space and select hello last string from hello returned array of strings.</span></span> <span data-ttu-id="6c429-141">Így lehetővé teszi a probléma megoldásához rosszul formázott engedélyezési fejléceket.</span><span class="sxs-lookup"><span data-stu-id="6c429-141">This provides a workaround for badly formatted authorization headers.</span></span>

```xml
<set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />
```

### <a name="making-hello-validation-request"></a><span data-ttu-id="6c429-142">Hello ellenőrzési kérés</span><span class="sxs-lookup"><span data-stu-id="6c429-142">Making hello validation request</span></span>
<span data-ttu-id="6c429-143">Miután tudunk hello engedélyezési jogkivonat, tökéletesíthetjük hello kérelem toovalidate hello jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="6c429-143">Once we have hello authorization token, we can make hello request toovalidate hello token.</span></span> <span data-ttu-id="6c429-144">RFC 7662 meghívja a folyamat introspection, és megköveteli, hogy Ön `POST` HTML formátumban toohello introspection erőforrás.</span><span class="sxs-lookup"><span data-stu-id="6c429-144">RFC 7662 calls this process introspection and requires that you `POST` a HTML form toohello introspection resource.</span></span> <span data-ttu-id="6c429-145">HTML-űrlaphoz hello legalább tartalmaznia kell egy kulcs/érték pár hello kulccsal `token`.</span><span class="sxs-lookup"><span data-stu-id="6c429-145">hello HTML form must at least contain a key/value pair with hello key `token`.</span></span> <span data-ttu-id="6c429-146">A kérelem toohello engedélyezési kiszolgáló is kell lennie, amely rosszindulatú ügyfelek nem Ugrás ezeken érvényes jogkivonatokat hitelesített tooensure.</span><span class="sxs-lookup"><span data-stu-id="6c429-146">This request toohello authorization server must also be authenticated tooensure that malicious clients cannot go trawling for valid tokens.</span></span>

```xml
<send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">
  <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>
  <set-method>POST</set-method>
  <set-header name="Authorization" exists-action="override">
    <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>
  </set-header>
  <set-header name="Content-Type" exists-action="override">
    <value>application/x-www-form-urlencoded</value>
  </set-header>
  <set-body>@($"token={(string)context.Variables["token"]}")</set-body>
</send-request>
```

### <a name="checking-hello-response"></a><span data-ttu-id="6c429-147">Hello válasz ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="6c429-147">Checking hello response</span></span>
<span data-ttu-id="6c429-148">Hello `response-variable-name` attribútum használt toogive hozzáférés hello választ adott vissza.</span><span class="sxs-lookup"><span data-stu-id="6c429-148">hello `response-variable-name` attribute is used toogive access hello returned response.</span></span> <span data-ttu-id="6c429-149">hello név ebben a tulajdonságban meghatározott használható kulcsként történő hello `context.Variables` szótár tooaccess hello `IResponse` objektum.</span><span class="sxs-lookup"><span data-stu-id="6c429-149">hello name defined in this property can be used as a key into hello `context.Variables` dictionary tooaccess hello `IResponse` object.</span></span>

<span data-ttu-id="6c429-150">Hello válasz objektumból hello törzs beolvasható és RFC 7622 közli velünk, hogy hello válasz egy JSON-objektumnak kell lennie, és tartalmaznia kell legalább egy tulajdonságot nevű `active` , amely egy logikai érték.</span><span class="sxs-lookup"><span data-stu-id="6c429-150">From hello response object we can retrieve hello body and RFC 7622 tells us that hello response must be a JSON object and must contain at least a property called `active` that is a boolean value.</span></span> <span data-ttu-id="6c429-151">Ha `active` értéke igaz, akkor hello token érvényes.</span><span class="sxs-lookup"><span data-stu-id="6c429-151">When `active` is true then hello token is considered valid.</span></span>

### <a name="reporting-failure"></a><span data-ttu-id="6c429-152">Jelentéskészítési hiba</span><span class="sxs-lookup"><span data-stu-id="6c429-152">Reporting failure</span></span>
<span data-ttu-id="6c429-153">Használjuk a `<choose>` házirend toodetect hello token érvénytelen, és ha igen, térjen vissza a 401-es választ.</span><span class="sxs-lookup"><span data-stu-id="6c429-153">We use a `<choose>` policy toodetect if hello token is invalid and if so, return a 401 response.</span></span>

```xml
<choose>
  <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">
    <return-response response-variable-name="existing response variable">
      <set-status code="401" reason="Unauthorized" />
      <set-header name="WWW-Authenticate" exists-action="override">
        <value>Bearer error="invalid_token"</value>
      </set-header>
    </return-response>
  </when>
</choose>
```

<span data-ttu-id="6c429-154">Megfelelően [RFC 6750](https://tools.ietf.org/html/rfc6750#section-3) amely ismerteti, hogyan `bearer` jogkivonatok kell használni, azt is adja vissza egy `WWW-Authenticate` hello 401-es válasz fejléce.</span><span class="sxs-lookup"><span data-stu-id="6c429-154">As per [RFC 6750](https://tools.ietf.org/html/rfc6750#section-3) which describes how `bearer` tokens should be used, we also return a `WWW-Authenticate` header with hello 401 response.</span></span> <span data-ttu-id="6c429-155">WWW-Authenticate hello tervezett tooinstruct egy ügyfelet, hogy miként tooconstruct megfelelően jogosult kérelmet.</span><span class="sxs-lookup"><span data-stu-id="6c429-155">hello WWW-Authenticate is intended tooinstruct a client on how tooconstruct a properly authorized request.</span></span> <span data-ttu-id="6c429-156">Hello OAuth2-keretrendszerben készült lehetséges módszer számos toohello, miatt is nehézkes toocommunicate hello összes szükséges információt.</span><span class="sxs-lookup"><span data-stu-id="6c429-156">Due toohello wide variety of approaches possible with hello OAuth2 framework, it is difficult toocommunicate all hello needed information.</span></span> <span data-ttu-id="6c429-157">Szerencsére nincsenek erőfeszítéseket folyamatban toohelp [ügyfelek felderítése hogyan tooproperly kérelmek tooa erőforrás kiszolgáló engedélyezése](http://tools.ietf.org/html/draft-jones-oauth-discovery-00).</span><span class="sxs-lookup"><span data-stu-id="6c429-157">Fortunately there are efforts underway toohelp [clients discover how tooproperly authorize requests tooa resource server](http://tools.ietf.org/html/draft-jones-oauth-discovery-00).</span></span>

### <a name="final-solution"></a><span data-ttu-id="6c429-158">Végső megoldás</span><span class="sxs-lookup"><span data-stu-id="6c429-158">Final solution</span></span>
<span data-ttu-id="6c429-159">Üzembe összes hello kódrészletek, azt lekérése hello házirendet a következő:</span><span class="sxs-lookup"><span data-stu-id="6c429-159">Putting all hello pieces together, we get hello following policy:</span></span>

```xml
<inbound>
  <!-- Extract Token from Authorization header parameter -->
  <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />

  <!-- Send request tooToken Server toovalidate token (see RFC 7662) -->
  <send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">
    <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>
    <set-method>POST</set-method>
    <set-header name="Authorization" exists-action="override">
      <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>
    </set-header>
    <set-header name="Content-Type" exists-action="override">
      <value>application/x-www-form-urlencoded</value>
    </set-header>
    <set-body>@($"token={(string)context.Variables["token"]}")</set-body>
  </send-request>

  <choose>
          <!-- Check active property in response -->
          <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">
              <!-- Return 401 Unauthorized with http-problem payload -->
              <return-response response-variable-name="existing response variable">
                  <set-status code="401" reason="Unauthorized" />
                  <set-header name="WWW-Authenticate" exists-action="override">
                      <value>Bearer error="invalid_token"</value>
                  </set-header>
              </return-response>
          </when>
      </choose>
  <base />
</inbound>
```

<span data-ttu-id="6c429-160">Ez sok példák közül csak az hello `send-request` házirend lehet használt toointegrate hasznos külső szolgáltatások kérések és válaszok API-kezelés szolgáltatás hello átfutó hello folyamatba.</span><span class="sxs-lookup"><span data-stu-id="6c429-160">This is only one of many examples of how hello `send-request` policy can be used toointegrate useful external services into hello process of requests and responses flowing through hello API Management service.</span></span>

## <a name="response-composition"></a><span data-ttu-id="6c429-161">Válasz összeállítás</span><span class="sxs-lookup"><span data-stu-id="6c429-161">Response Composition</span></span>
<span data-ttu-id="6c429-162">Hello `send-request` házirend használható egy elsődleges kérelem tooa háttérrendszer növelésére, az előző példában hello látott, vagy azt egy teljes cserélje le a hello háttér hívás használható.</span><span class="sxs-lookup"><span data-stu-id="6c429-162">hello `send-request` policy can be used for enhancing a primary request tooa backend system, as we saw in hello previous example, or it can be used as a complete replace for of hello backend call.</span></span> <span data-ttu-id="6c429-163">Ez a módszer használatával könnyen létrehozhatjuk összesítése összetett erőforrásokat több különböző rendszerekből.</span><span class="sxs-lookup"><span data-stu-id="6c429-163">Using this technique we can easily create composite resources that are aggregated from multiple different systems.</span></span>

### <a name="building-a-dashboard"></a><span data-ttu-id="6c429-164">Irányítópult létrehozása</span><span class="sxs-lookup"><span data-stu-id="6c429-164">Building a dashboard</span></span>
<span data-ttu-id="6c429-165">Néha toobe képes tooexpose információt szeretne, hogy megtalálható-e több háttérrendszerek, például toodrive egy irányítópultot.</span><span class="sxs-lookup"><span data-stu-id="6c429-165">Sometimes you want toobe able tooexpose information that exists in multiple backend systems, for example, toodrive a dashboard.</span></span> <span data-ttu-id="6c429-166">hello KPI-k minden különböző biztonsági-végpontok, származhat, de nem tooprovide közvetlen hozzáférést toothem inkább, és célszerű, ha minden hello információi olvashatók az egy kérelemhez.</span><span class="sxs-lookup"><span data-stu-id="6c429-166">hello KPIs come from all different back-ends, but you would prefer not tooprovide direct access toothem and it would be nice if all hello information could be retrieved in a single request.</span></span> <span data-ttu-id="6c429-167">Lehet, hogy néhány hello háttér-információkat kell néhány szeletelhető és feldarabolható, és először egy kis tisztítására!</span><span class="sxs-lookup"><span data-stu-id="6c429-167">Perhaps some of hello backend information needs some slicing and dicing and a little sanitizing first!</span></span> <span data-ttu-id="6c429-168">Tudja, hogy összetett erőforrás lenne egy hasznos tooreduce toocache alatt hello háttér tölthető be, mint tudja a felhasználók rendelkeznek-e egy válhat a beütés hello F5 billentyűt a rendelés toosee, ha azok underperforming metrikák változhatnak.</span><span class="sxs-lookup"><span data-stu-id="6c429-168">Being able toocache that composite resource would be a useful tooreduce hello backend load as you know users have a habit of hammering hello F5 key in order toosee if their underperforming metrics might change.</span></span>    

### <a name="faking-hello-resource"></a><span data-ttu-id="6c429-169">Faking hello erőforrás</span><span class="sxs-lookup"><span data-stu-id="6c429-169">Faking hello resource</span></span>
<span data-ttu-id="6c429-170">első lépés toobuilding hello az irányítópult erőforrás tooconfigure hello API Management publisher portálon új művelet.</span><span class="sxs-lookup"><span data-stu-id="6c429-170">hello first step toobuilding our dashboard resource is tooconfigure a new operation in hello API Management publisher portal.</span></span> <span data-ttu-id="6c429-171">Ez lesz a helyőrző használt művelet tooconfigure az összeállítás házirend toobuild a dinamikus erőforrás.</span><span class="sxs-lookup"><span data-stu-id="6c429-171">This will be a placeholder operation used tooconfigure our composition policy toobuild our dynamic resource.</span></span>

![Irányítópult-művelet](./media/api-management-sample-send-request/api-management-dashboard-operation.png)

### <a name="making-hello-requests"></a><span data-ttu-id="6c429-173">Hello kérést</span><span class="sxs-lookup"><span data-stu-id="6c429-173">Making hello requests</span></span>
<span data-ttu-id="6c429-174">Egyszer hello `dashboard` művelet létrejött konfigurálhatjuk egy kifejezetten az adott műveletre vonatkozó házirendet.</span><span class="sxs-lookup"><span data-stu-id="6c429-174">Once hello `dashboard` operation has been created we can configure a policy specifically for that operation.</span></span> 

![Irányítópult-művelet](./media/api-management-sample-send-request/api-management-dashboard-policy.png)

<span data-ttu-id="6c429-176">első lépés hello van a hello bejövő kérelem, a lekérdezési paramétereket tooextract, hogy azt is továbbítják őket tooour háttér.</span><span class="sxs-lookup"><span data-stu-id="6c429-176">hello first step  is tooextract any query parameters from hello incoming request, so that we can forward them tooour backend.</span></span> <span data-ttu-id="6c429-177">Ebben a példában az irányítópulton látható információk alapján egy adott időn belül egy ezért egy `fromDate` és `toDate` paraméter.</span><span class="sxs-lookup"><span data-stu-id="6c429-177">In this example our dashboard is showing information based on a period of time an therefore has a `fromDate` and `toDate` parameter.</span></span> <span data-ttu-id="6c429-178">Használhatunk hello `set-variable` házirend tooextract hello adatait hello kérelem URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="6c429-178">We can use hello `set-variable` policy tooextract hello information from hello request URL.</span></span>

```xml
<set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
<set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">
```

<span data-ttu-id="6c429-179">Ha ezt az információt kell tökéletesíthetjük tooall hello háttérrendszerek kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="6c429-179">Once we have this information we can make requests tooall hello backend systems.</span></span> <span data-ttu-id="6c429-180">Minden kérést hoz létre egy új URL-cím elé hello paraméterinformáció, és meghívja a megfelelő kiszolgálóhoz, és hello válasz egy környezeti változó tárolja.</span><span class="sxs-lookup"><span data-stu-id="6c429-180">Each request constructs a new URL with hello parameter information and calls its respective server and stores hello response in a context variable.</span></span>

```xml
<send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
  <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
  <set-method>GET</set-method>
</send-request>

<send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
  <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
  <set-method>GET</set-method>
</send-request>

<send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
<set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
  <set-method>GET</set-method>
</send-request>

<send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
<set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
  <set-method>GET</set-method>
</send-request>
```

<span data-ttu-id="6c429-181">Ezek a kérelmek végrehajtja a sorozatban, ami nem ideális.</span><span class="sxs-lookup"><span data-stu-id="6c429-181">These requests will execute in sequence, which is not ideal.</span></span> <span data-ttu-id="6c429-182">Egy jövőbeli verzióban azt fogja bevezetése nevű új házirend `wait` , amely engedélyezi ezeket kérelmek tooexecute párhuzamosan mindegyikét.</span><span class="sxs-lookup"><span data-stu-id="6c429-182">In an upcoming release we will be introducing a new policy called `wait` that will enable all of these requests tooexecute in parallel.</span></span>

### <a name="responding"></a><span data-ttu-id="6c429-183">Válaszol</span><span class="sxs-lookup"><span data-stu-id="6c429-183">Responding</span></span>
<span data-ttu-id="6c429-184">tooconstruct hello összetett válasz használhatjuk hello [visszatérési-válasz](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) házirend.</span><span class="sxs-lookup"><span data-stu-id="6c429-184">tooconstruct hello composite response we can use hello [return-response](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) policy.</span></span> <span data-ttu-id="6c429-185">Hello `set-body` elem használható egy kifejezésben tooconstruct egy új `JObject` az összes hello összetevő felelősséget tulajdonságként beágyazott.</span><span class="sxs-lookup"><span data-stu-id="6c429-185">hello `set-body` element can use an expression tooconstruct a new `JObject` with all hello component representations embedded as properties.</span></span>

```xml
<return-response response-variable-name="existing response variable">
  <set-status code="200" reason="OK" />
  <set-header name="Content-Type" exists-action="override">
    <value>application/json</value>
  </set-header>
  <set-body>
    @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                  new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                  new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                  new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                  ).ToString())
  </set-body>
</return-response>
```

<span data-ttu-id="6c429-186">hello teljes házirendet a következőképpen néz ki:</span><span class="sxs-lookup"><span data-stu-id="6c429-186">hello complete policy looks as follows:</span></span>

```xml
<policies>
    <inbound>

  <set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
  <set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">

    <send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
      <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
      <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <return-response response-variable-name="existing response variable">
      <set-status code="200" reason="OK" />
      <set-header name="Content-Type" exists-action="override">
        <value>application/json</value>
      </set-header>
      <set-body>
        @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                      new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                      new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                      new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                      ).ToString())
      </set-body>
    </return-response>
    </inbound>
    <backend>
        <base />
    </backend>
    <outbound>
        <base />
    </outbound>
</policies>
```

<span data-ttu-id="6c429-187">Hello konfigurációban hello helyőrző művelet konfigurálhatjuk hello irányítópult erőforrás toobe gyorsítótárba helyezett legalább egy órával mert tudjuk hello adatok hello jellege azt jelenti, hogy akkor is, ha egy elavult, még mindig elég hatékony óra tooconvey értékes információk toohello felhasználók.</span><span class="sxs-lookup"><span data-stu-id="6c429-187">In hello configuration of hello placeholder operation we can configure hello dashboard resource toobe cached for at least an hour because we understand hello nature of hello data means that even if it is an hour out of date, it will still be sufficiently effective tooconvey valuable information toohello users.</span></span>

## <a name="summary"></a><span data-ttu-id="6c429-188">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="6c429-188">Summary</span></span>
<span data-ttu-id="6c429-189">Az Azure API Management szolgáltatás szelektív módon lehet rugalmas házirendek alkalmazása tooHTTP forgalom biztosít, és lehetővé teszi, hogy a háttér szolgáltatások összeállításban.</span><span class="sxs-lookup"><span data-stu-id="6c429-189">Azure API Management service provides flexible policies that can be selectively applied tooHTTP traffic and enables composition of backend services.</span></span> <span data-ttu-id="6c429-190">Szeretné, hogy a riasztás funkciók, ellenőrzési, érvényesítési képességek API átjáró tooenhance, vagy hozzon létre új összetett erőforrások több háttér-szolgáltatásoknak, hello `send-request` és a lehetőségek tárházát nyissa meg a kapcsolódó házirendek.</span><span class="sxs-lookup"><span data-stu-id="6c429-190">Whether you want tooenhance your API gateway with alerting functions, verification, validation capabilities or create new composite resources based on multiple backend services, hello `send-request` and related policies open a world of possibilities.</span></span>

## <a name="watch-a-video-overview-of-these-policies"></a><span data-ttu-id="6c429-191">A házirendek áttekintő videó megtekintése</span><span class="sxs-lookup"><span data-stu-id="6c429-191">Watch a video overview of these policies</span></span>
<span data-ttu-id="6c429-192">További információt a hello [küldési-egy-módon-kérelmek](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest), [küldési-kérelmek](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest), és [visszatérési-válasz](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) a cikkben szereplő házirendek adjon tekintse meg a következő videó hello.</span><span class="sxs-lookup"><span data-stu-id="6c429-192">For more information on hello [send-one-way-request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest), [send-request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest), and [return-response](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) policies covered in this article, please watch hello following video.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Send-Request-and-Return-Response-Policies/player]
> 
> 

