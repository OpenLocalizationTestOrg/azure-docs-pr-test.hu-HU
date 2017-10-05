---
title: "HTTP-kérelmek létrehozásához az API Management szolgáltatással"
description: "Ismerkedjen meg az API Management kérelem-válasz házirendek segítségével külső szolgáltatások hívja az API-t"
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
ms.openlocfilehash: e778943715d6ca5256ad612d82bdc1f82197df0d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="using-external-services-from-the-azure-api-management-service"></a><span data-ttu-id="a341a-103">Az Azure API Management szolgáltatás a külső services használatával</span><span class="sxs-lookup"><span data-stu-id="a341a-103">Using external services from the Azure API Management service</span></span>
<span data-ttu-id="a341a-104">Az Azure API Management szolgáltatásban elérhető házirendek alapján csak a bejövő kérelem, a kimenő válasz és alapvető konfigurációs adatai hasznos munkahelyi számos teheti meg.</span><span class="sxs-lookup"><span data-stu-id="a341a-104">The policies available in Azure API Management service can do a wide range of useful work based purely on the incoming request, the outgoing response and basic configuration information.</span></span> <span data-ttu-id="a341a-105">Azonban tudnak kommunikálni az API Management külső szolgáltatások számos további lehetőségek megnyílik házirendek.</span><span class="sxs-lookup"><span data-stu-id="a341a-105">However, being able to interact with external services from API Management policies opens up many more opportunities.</span></span>

<span data-ttu-id="a341a-106">Korábban úgy találtuk, hogy azt léphetnek interakcióba az [Azure Event Hubs szolgáltatás naplózási, megfigyelésének és elemzés](api-management-log-to-eventhub-sample.md).</span><span class="sxs-lookup"><span data-stu-id="a341a-106">We have previously seen how we can interact with the [Azure Event Hub service for logging, monitoring and analytics](api-management-log-to-eventhub-sample.md).</span></span> <span data-ttu-id="a341a-107">Ebben a cikkben a házirendekben, amelyek lehetővé teszik a külső HTTP együttműködhet alapú szolgáltatás bemutatjuk.</span><span class="sxs-lookup"><span data-stu-id="a341a-107">In this article we will demonstrate policies that allow you to interact with any external HTTP based service.</span></span> <span data-ttu-id="a341a-108">Ezekkel a szabályzatokkal használható távoli eseményeket kiváltó vagy az eredeti kérelem és válasz valamilyen módon használt adatok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="a341a-108">These policies can be used for triggering remote events or for retrieving information that will be used to manipulate the original request and response in some way.</span></span>

## <a name="send-one-way-request"></a><span data-ttu-id="a341a-109">Küldési-egy-módon-kérelmek</span><span class="sxs-lookup"><span data-stu-id="a341a-109">Send-One-Way-Request</span></span>
<span data-ttu-id="a341a-110">Valószínűleg a legegyszerűbb külső beavatkozásra, amely lehetővé teszi egy külső szolgáltatás értesítést fontos esemény valamilyen kérelem tűz-és-elfelejti stílusát.</span><span class="sxs-lookup"><span data-stu-id="a341a-110">Possibly the simplest external interaction is the fire-and-forget style of request that allows an external service to be notified of some kind of important event.</span></span> <span data-ttu-id="a341a-111">A vezérlő adatfolyam házirend használhatjuk `choose` feltétel, hogy érdeklődik dolgozunk, és majd, ha a feltétel teljesül, tökéletesíthetjük egy külső HTTP kérelem használatával bármilyen észleléséhez a [küldési-egy-módon-kérelmek](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) házirend.</span><span class="sxs-lookup"><span data-stu-id="a341a-111">We can use the control flow policy `choose` to detect any kind of condition that we are interested in and then, if the condition is satisfied, we can make an external HTTP request using the [send-one-way-request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) policy.</span></span> <span data-ttu-id="a341a-112">Ennek oka lehet egy kérelem egy üzenetküldési rendszerre, például Hipchat vagy a Slackhez, illetve egy levelezési API SendGrid vagy MailChimp, vagy a kritikus fontosságú támogatási incidensek az alábbihoz hasonló PagerDuty.</span><span class="sxs-lookup"><span data-stu-id="a341a-112">This could be a request to a messaging system like Hipchat or Slack, or a mail API like SendGrid or MailChimp, or for critical support incidents something like PagerDuty.</span></span> <span data-ttu-id="a341a-113">E üzenetkezelési rendszerek rendelkezik egyszerű HTTP API-k könnyen hívhat meg azt.</span><span class="sxs-lookup"><span data-stu-id="a341a-113">All of these messaging systems have simple HTTP APIs that we can easily invoke.</span></span>

### <a name="alerting-with-slack"></a><span data-ttu-id="a341a-114">A Slackhez riasztás</span><span class="sxs-lookup"><span data-stu-id="a341a-114">Alerting with Slack</span></span>
<span data-ttu-id="a341a-115">A következő példa bemutatja, hogyan üzenetet küldeni a Slack Csevegés helyiségben, ha a HTTP-válasz állapotkódja nagyobb vagy egyenlő 500.</span><span class="sxs-lookup"><span data-stu-id="a341a-115">The following example demonstrates how to send a message to a Slack chat room if the HTTP response status code is greater than or equal to 500.</span></span> <span data-ttu-id="a341a-116">Egy 500 tartomány hiba a háttér-API, amely az ügyfél az API nem maguktól megoldódhatnak problémájára utal.</span><span class="sxs-lookup"><span data-stu-id="a341a-116">A 500 range error indicates a problem with our backend API that the client of our API cannot resolve themselves.</span></span> <span data-ttu-id="a341a-117">Általában szüksége van valamilyen beavatkozás az eszközeiken.</span><span class="sxs-lookup"><span data-stu-id="a341a-117">It usually requires some kind of intervention on our part.</span></span>  

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

<span data-ttu-id="a341a-118">Slackhez rendelkezik a bejövő webes hurkok fogalmát.</span><span class="sxs-lookup"><span data-stu-id="a341a-118">Slack has the notion of inbound web hooks.</span></span> <span data-ttu-id="a341a-119">Egy bejövő webes hook konfigurálásakor a Slackhez hoz létre egy különleges URL-címet, amely lehetővé teszi egy egyszerű POST kérelem végrehajtásához, és a Slack csatorna üzenetet továbbítani.</span><span class="sxs-lookup"><span data-stu-id="a341a-119">When configuring an inbound web hook, Slack generates a special URL which allows you to do a simple POST request and to pass a message into the Slack channel.</span></span> <span data-ttu-id="a341a-120">A JSON-törzsére, amely létrehozhatunk Slackhez által meghatározott formátumban alapul.</span><span class="sxs-lookup"><span data-stu-id="a341a-120">The JSON body that we create is based on a format defined by Slack.</span></span>

![Slack webes Hook](./media/api-management-sample-send-request/api-management-slack-webhook.png)

### <a name="is-fire-and-forget-good-enough"></a><span data-ttu-id="a341a-122">Tűz és elég jól elfelejti?</span><span class="sxs-lookup"><span data-stu-id="a341a-122">Is fire and forget good enough?</span></span>
<span data-ttu-id="a341a-123">Nincsenek bizonyos mellékhatásokkal kérelem tűz-és-elfelejti stílus használatával.</span><span class="sxs-lookup"><span data-stu-id="a341a-123">There are certain tradeoffs when using a fire-and-forget style of request.</span></span> <span data-ttu-id="a341a-124">Ha valamilyen okból, a kérelem sikertelen lesz, akkor a hiba nem fog szerepelni.</span><span class="sxs-lookup"><span data-stu-id="a341a-124">If for some reason, the request fails, then the failure will not be reported.</span></span> <span data-ttu-id="a341a-125">Adott esetben nem igazolható összetettségét, hogy a másodlagos hibájának jelentéskészítő rendszer és a további teljesítményarányos költsége a válaszra való várakozás közben.</span><span class="sxs-lookup"><span data-stu-id="a341a-125">In this particular situation, the complexity of having a secondary failure reporting system and the additional performance cost of waiting for the response is not warranted.</span></span> <span data-ttu-id="a341a-126">A forgatókönyvekben, ahol a válasz ellenőrzése alapvető fontosságú a [küldési-kérelmek](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) házirend jobb megoldás.</span><span class="sxs-lookup"><span data-stu-id="a341a-126">For scenarios where it is essential to check the response, then the [send-request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) policy is a better option.</span></span>

## <a name="send-request"></a><span data-ttu-id="a341a-127">Küldési-kérelmek</span><span class="sxs-lookup"><span data-stu-id="a341a-127">Send-Request</span></span>
<span data-ttu-id="a341a-128">A `send-request` házirend lehetővé teszi, hogy egy külső szolgáltatás segítségével összetett feldolgozási feladatokat, és térjen vissza az adatokat az API management szolgáltatást, amely nem használható további házirend-feldolgozás.</span><span class="sxs-lookup"><span data-stu-id="a341a-128">The `send-request` policy enables using an external service to perform complex processing functions and return data to the API management service that can be used for further policy processing.</span></span>

### <a name="authorizing-reference-tokens"></a><span data-ttu-id="a341a-129">Referencia-tokenek engedélyezése</span><span class="sxs-lookup"><span data-stu-id="a341a-129">Authorizing reference tokens</span></span>
<span data-ttu-id="a341a-130">Az API Management főbb funkcióját háttér erőforrások védi.</span><span class="sxs-lookup"><span data-stu-id="a341a-130">A major function of API Management is protecting backend resources.</span></span> <span data-ttu-id="a341a-131">Ha a hitelesítési kiszolgáló, az API által használt hoz létre [JWT-jogkivonatokat](http://jwt.io/) az OAuth2 folyamat részeként, [Azure Active Directory](../active-directory/active-directory-aadconnect.md) Igen, akkor a `validate-jwt` házirend a token érvényességének ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="a341a-131">If the authorization server used by your API creates [JWT tokens](http://jwt.io/) as part of its OAuth2 flow, as [Azure Active Directory](../active-directory/active-directory-aadconnect.md) does, then you can use the `validate-jwt` policy to verify the validity of the token.</span></span> <span data-ttu-id="a341a-132">Azonban néhány engedélyezési kiszolgálók létrehozása, mi nevezzük [jogkivonatok hivatkozhat](http://leastprivilege.com/2015/11/25/reference-tokens-and-introspection/) anélkül, hogy az engedélyezési kiszolgálóra történő hívás, amely nem ellenőrizhető.</span><span class="sxs-lookup"><span data-stu-id="a341a-132">However, some authorization servers create what are called [reference tokens](http://leastprivilege.com/2015/11/25/reference-tokens-and-introspection/) that cannot be verified without making a call back to the authorization server.</span></span>

### <a name="standardized-introspection"></a><span data-ttu-id="a341a-133">Szabványos introspection</span><span class="sxs-lookup"><span data-stu-id="a341a-133">Standardized introspection</span></span>
<span data-ttu-id="a341a-134">A múltban nincs szabványosított lehetőség van egy engedélyezési kiszolgáló és a hivatkozás-jogkivonat ellenőrzése le lett.</span><span class="sxs-lookup"><span data-stu-id="a341a-134">In the past there has been no standardized way of verifying a reference token with an authorization server.</span></span> <span data-ttu-id="a341a-135">Azonban a közelmúltban javasolt szabvány [RFC 7662](https://tools.ietf.org/html/rfc7662) az IETF, amely meghatározza, hogyan erőforrás-kiszolgáló a token érvényességének ellenőrzésére tett közzé.</span><span class="sxs-lookup"><span data-stu-id="a341a-135">However a recently proposed standard [RFC 7662](https://tools.ietf.org/html/rfc7662) was published by the IETF that defines how a resource server can verify the validity of a token.</span></span>

### <a name="extracting-the-token"></a><span data-ttu-id="a341a-136">A jogkivonat beolvasása</span><span class="sxs-lookup"><span data-stu-id="a341a-136">Extracting the token</span></span>
<span data-ttu-id="a341a-137">Az első lépés a token kibontani a Authorization fejlécet.</span><span class="sxs-lookup"><span data-stu-id="a341a-137">The first step is to extract the token from the Authorization header.</span></span> <span data-ttu-id="a341a-138">A fejléc értékének fájlrendszerrel kell formázni a `Bearer` engedélyezési sémát, egy szóköz, majd az engedélyezési jogkivonatot megfelelően [RFC 6750](http://tools.ietf.org/html/rfc6750#section-2.1).</span><span class="sxs-lookup"><span data-stu-id="a341a-138">The header value should be formatted with the `Bearer` authorization scheme, a single space and then the authorization token as per [RFC 6750](http://tools.ietf.org/html/rfc6750#section-2.1).</span></span> <span data-ttu-id="a341a-139">Sajnos előfordulhatnak olyan esetek, ahol a hitelesítési séma meg van adva.</span><span class="sxs-lookup"><span data-stu-id="a341a-139">Unfortunately there are cases where the authorization scheme is omitted.</span></span> <span data-ttu-id="a341a-140">A fiók elemzésekor, azt ossza fel a fejléc értékének a tárhelyen, és válassza az utolsó karakterlánc karakterláncokat. a visszaadott tömb.</span><span class="sxs-lookup"><span data-stu-id="a341a-140">To account for this when parsing, we split the header value on a space and select the last string from the returned array of strings.</span></span> <span data-ttu-id="a341a-141">Így lehetővé teszi a probléma megoldásához rosszul formázott engedélyezési fejléceket.</span><span class="sxs-lookup"><span data-stu-id="a341a-141">This provides a workaround for badly formatted authorization headers.</span></span>

```xml
<set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />
```

### <a name="making-the-validation-request"></a><span data-ttu-id="a341a-142">Az ellenőrzési kérés</span><span class="sxs-lookup"><span data-stu-id="a341a-142">Making the validation request</span></span>
<span data-ttu-id="a341a-143">Az engedélyezési jogkivonatot kell, ha a kérelem ellenőrzése a jogkivonat tökéletesíthetjük.</span><span class="sxs-lookup"><span data-stu-id="a341a-143">Once we have the authorization token, we can make the request to validate the token.</span></span> <span data-ttu-id="a341a-144">RFC 7662 meghívja a folyamat introspection, és megköveteli, hogy Ön `POST` egy HTML-űrlaphoz, introspection-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="a341a-144">RFC 7662 calls this process introspection and requires that you `POST` a HTML form to the introspection resource.</span></span> <span data-ttu-id="a341a-145">A HTML-űrlaphoz, legalább tartalmaznia kell egy kulcs/érték pár kulccsal `token`.</span><span class="sxs-lookup"><span data-stu-id="a341a-145">The HTML form must at least contain a key/value pair with the key `token`.</span></span> <span data-ttu-id="a341a-146">A kérést a hitelesítési kiszolgáló is hitelesíteni kell ahhoz, hogy a rosszindulatú ügyfelek nem Ugrás ezeken érvényes jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="a341a-146">This request to the authorization server must also be authenticated to ensure that malicious clients cannot go trawling for valid tokens.</span></span>

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

### <a name="checking-the-response"></a><span data-ttu-id="a341a-147">A válasz ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="a341a-147">Checking the response</span></span>
<span data-ttu-id="a341a-148">A `response-variable-name` attribútum segítségével hozzáférést biztosíthat a visszaadott válaszban.</span><span class="sxs-lookup"><span data-stu-id="a341a-148">The `response-variable-name` attribute is used to give access the returned response.</span></span> <span data-ttu-id="a341a-149">Az ebben a tulajdonságban megadott név lesz a kulcs az használható a `context.Variables` szótár eléréséhez a `IResponse` objektum.</span><span class="sxs-lookup"><span data-stu-id="a341a-149">The name defined in this property can be used as a key into the `context.Variables` dictionary to access the `IResponse` object.</span></span>

<span data-ttu-id="a341a-150">A válasz objektumból beolvasható a törzs és RFC 7622 közli velünk, hogy a válasz egy JSON-objektumnak kell lennie, és tartalmaznia kell legalább egy nevű tulajdonság `active` , amely egy logikai érték.</span><span class="sxs-lookup"><span data-stu-id="a341a-150">From the response object we can retrieve the body and RFC 7622 tells us that the response must be a JSON object and must contain at least a property called `active` that is a boolean value.</span></span> <span data-ttu-id="a341a-151">Ha `active` értéke igaz, akkor a jogkivonat érvényes.</span><span class="sxs-lookup"><span data-stu-id="a341a-151">When `active` is true then the token is considered valid.</span></span>

### <a name="reporting-failure"></a><span data-ttu-id="a341a-152">Jelentéskészítési hiba</span><span class="sxs-lookup"><span data-stu-id="a341a-152">Reporting failure</span></span>
<span data-ttu-id="a341a-153">Használjuk a `<choose>` házirend észleli, ha a token érvénytelen, és ha igen, térjen vissza a 401-es válasz.</span><span class="sxs-lookup"><span data-stu-id="a341a-153">We use a `<choose>` policy to detect if the token is invalid and if so, return a 401 response.</span></span>

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

<span data-ttu-id="a341a-154">Megfelelően [RFC 6750](https://tools.ietf.org/html/rfc6750#section-3) ismerteti, amelyek hogyan `bearer` jogkivonatok kell használni, azt is adja vissza egy `WWW-Authenticate` a 401-es válasz fejléce.</span><span class="sxs-lookup"><span data-stu-id="a341a-154">As per [RFC 6750](https://tools.ietf.org/html/rfc6750#section-3) which describes how `bearer` tokens should be used, we also return a `WWW-Authenticate` header with the 401 response.</span></span> <span data-ttu-id="a341a-155">A WWW-Authenticate célja, hogy kérje meg egy ügyfél egy megfelelően jogosult kérelem létrehozására.</span><span class="sxs-lookup"><span data-stu-id="a341a-155">The WWW-Authenticate is intended to instruct a client on how to construct a properly authorized request.</span></span> <span data-ttu-id="a341a-156">Az OAuth2-keretrendszerben készült lehetséges módszer számos, mert is nehézkes való kommunikációhoz szükséges összes információ.</span><span class="sxs-lookup"><span data-stu-id="a341a-156">Due to the wide variety of approaches possible with the OAuth2 framework, it is difficult to communicate all the needed information.</span></span> <span data-ttu-id="a341a-157">Szerencsére nincsenek azon törekvéseit, hogy folyamatban érdekében [ügyfelek miképpen megfelelően engedélyezik az erőforrás-kiszolgálóhoz intézett kérésekben](http://tools.ietf.org/html/draft-jones-oauth-discovery-00).</span><span class="sxs-lookup"><span data-stu-id="a341a-157">Fortunately there are efforts underway to help [clients discover how to properly authorize requests to a resource server](http://tools.ietf.org/html/draft-jones-oauth-discovery-00).</span></span>

### <a name="final-solution"></a><span data-ttu-id="a341a-158">Végső megoldás</span><span class="sxs-lookup"><span data-stu-id="a341a-158">Final solution</span></span>
<span data-ttu-id="a341a-159">Minden a helyére üzembe, azt lekérése a következő házirendet:</span><span class="sxs-lookup"><span data-stu-id="a341a-159">Putting all the pieces together, we get the following policy:</span></span>

```xml
<inbound>
  <!-- Extract Token from Authorization header parameter -->
  <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />

  <!-- Send request to Token Server to validate token (see RFC 7662) -->
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

<span data-ttu-id="a341a-160">Ez az csak az egyik számos példát bemutatja a `send-request` házirend használható hasznos külső szolgáltatás integrálása kérelmeit és válaszait az API Management szolgáltatáson keresztül áramló folyamatán.</span><span class="sxs-lookup"><span data-stu-id="a341a-160">This is only one of many examples of how the `send-request` policy can be used to integrate useful external services into the process of requests and responses flowing through the API Management service.</span></span>

## <a name="response-composition"></a><span data-ttu-id="a341a-161">Válasz összeállítás</span><span class="sxs-lookup"><span data-stu-id="a341a-161">Response Composition</span></span>
<span data-ttu-id="a341a-162">A `send-request` házirend használható a háttérrendszer elsődleges kérelem növelésére, az előző példában szereplő látott, vagy azt a háttér-hívás a teljes csere használható.</span><span class="sxs-lookup"><span data-stu-id="a341a-162">The `send-request` policy can be used for enhancing a primary request to a backend system, as we saw in the previous example, or it can be used as a complete replace for of the backend call.</span></span> <span data-ttu-id="a341a-163">Ez a módszer használatával könnyen létrehozhatjuk összesítése összetett erőforrásokat több különböző rendszerekből.</span><span class="sxs-lookup"><span data-stu-id="a341a-163">Using this technique we can easily create composite resources that are aggregated from multiple different systems.</span></span>

### <a name="building-a-dashboard"></a><span data-ttu-id="a341a-164">Irányítópult létrehozása</span><span class="sxs-lookup"><span data-stu-id="a341a-164">Building a dashboard</span></span>
<span data-ttu-id="a341a-165">Néha szükség lehet több háttérrendszerek, például található információt teszi közzé, egy irányítópultot alapjául.</span><span class="sxs-lookup"><span data-stu-id="a341a-165">Sometimes you want to be able to expose information that exists in multiple backend systems, for example, to drive a dashboard.</span></span> <span data-ttu-id="a341a-166">A KPI-k minden különböző biztonsági-végpontok, származhat, de nem szeretne közvetlen hozzáférést biztosít, és célszerű, ha minden információi olvashatók az egy kérelemhez.</span><span class="sxs-lookup"><span data-stu-id="a341a-166">The KPIs come from all different back-ends, but you would prefer not to provide direct access to them and it would be nice if all the information could be retrieved in a single request.</span></span> <span data-ttu-id="a341a-167">Lehet, hogy a háttér-adatok egy részét kell néhány szeletelhető és feldarabolható, és először egy kis tisztítására!</span><span class="sxs-lookup"><span data-stu-id="a341a-167">Perhaps some of the backend information needs some slicing and dicing and a little sanitizing first!</span></span> <span data-ttu-id="a341a-168">Igényt, hogy összetett erőforrás gyorsítótárazásához egy hasznos lehet a háttér-terhelés csökkentésére, mint tudja a felhasználók rendelkeznek-e egy, az F5 billentyűt beütés láthatók, ha azok underperforming metrikák változhat válhat.</span><span class="sxs-lookup"><span data-stu-id="a341a-168">Being able to cache that composite resource would be a useful to reduce the backend load as you know users have a habit of hammering the F5 key in order to see if their underperforming metrics might change.</span></span>    

### <a name="faking-the-resource"></a><span data-ttu-id="a341a-169">Az erőforrás faking</span><span class="sxs-lookup"><span data-stu-id="a341a-169">Faking the resource</span></span>
<span data-ttu-id="a341a-170">Az irányítópult erőforrás létrehozásának első lépése, hogy egy új művelet konfigurálható az API Management publisher portálon.</span><span class="sxs-lookup"><span data-stu-id="a341a-170">The first step to building our dashboard resource is to configure a new operation in the API Management publisher portal.</span></span> <span data-ttu-id="a341a-171">Ez egy helyőrző művelet segítségével konfigurálhatja a dinamikus erőforrás létrehozásához az összeállítás házirend lesz.</span><span class="sxs-lookup"><span data-stu-id="a341a-171">This will be a placeholder operation used to configure our composition policy to build our dynamic resource.</span></span>

![Irányítópult-művelet](./media/api-management-sample-send-request/api-management-dashboard-operation.png)

### <a name="making-the-requests"></a><span data-ttu-id="a341a-173">A kérést</span><span class="sxs-lookup"><span data-stu-id="a341a-173">Making the requests</span></span>
<span data-ttu-id="a341a-174">Egyszer a `dashboard` művelet létrejött konfigurálhatjuk egy kifejezetten az adott műveletre vonatkozó házirendet.</span><span class="sxs-lookup"><span data-stu-id="a341a-174">Once the `dashboard` operation has been created we can configure a policy specifically for that operation.</span></span> 

![Irányítópult-művelet](./media/api-management-sample-send-request/api-management-dashboard-policy.png)

<span data-ttu-id="a341a-176">Az első lépés a lekérdezési paramétereket kibontani a bejövő kérelem, azt is továbbítják őket a háttéralkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a341a-176">The first step  is to extract any query parameters from the incoming request, so that we can forward them to our backend.</span></span> <span data-ttu-id="a341a-177">Ebben a példában az irányítópulton látható információk alapján egy adott időn belül egy ezért egy `fromDate` és `toDate` paraméter.</span><span class="sxs-lookup"><span data-stu-id="a341a-177">In this example our dashboard is showing information based on a period of time an therefore has a `fromDate` and `toDate` parameter.</span></span> <span data-ttu-id="a341a-178">Így a `set-variable` házirend kinyerheti az információt a kérelem URL-címről.</span><span class="sxs-lookup"><span data-stu-id="a341a-178">We can use the `set-variable` policy to extract the information from the request URL.</span></span>

```xml
<set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
<set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">
```

<span data-ttu-id="a341a-179">Ha ezt az információt kell tökéletesíthetjük kérések a háttér-rendszereken.</span><span class="sxs-lookup"><span data-stu-id="a341a-179">Once we have this information we can make requests to all the backend systems.</span></span> <span data-ttu-id="a341a-180">Minden kérelmet hoz létre egy új URL-címet a paraméter adatokkal, és meghívja a megfelelő kiszolgálóhoz, és a válasz egy környezeti változó tárolja.</span><span class="sxs-lookup"><span data-stu-id="a341a-180">Each request constructs a new URL with the parameter information and calls its respective server and stores the response in a context variable.</span></span>

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

<span data-ttu-id="a341a-181">Ezek a kérelmek végrehajtja a sorozatban, ami nem ideális.</span><span class="sxs-lookup"><span data-stu-id="a341a-181">These requests will execute in sequence, which is not ideal.</span></span> <span data-ttu-id="a341a-182">Egy jövőbeli verzióban azt fogja bevezetése nevű új házirend `wait` , amely engedélyezi az összes ezeket a kéréseket végre párhuzamosan.</span><span class="sxs-lookup"><span data-stu-id="a341a-182">In an upcoming release we will be introducing a new policy called `wait` that will enable all of these requests to execute in parallel.</span></span>

### <a name="responding"></a><span data-ttu-id="a341a-183">Válaszol</span><span class="sxs-lookup"><span data-stu-id="a341a-183">Responding</span></span>
<span data-ttu-id="a341a-184">Használhatunk összetett válasz összeállítani a [visszatérési-válasz](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) házirend.</span><span class="sxs-lookup"><span data-stu-id="a341a-184">To construct the composite response we can use the [return-response](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) policy.</span></span> <span data-ttu-id="a341a-185">A `set-body` elem kifejezés használatával hozható létre egy új `JObject` a beágyazott tulajdonságként összetevő felelősséget.</span><span class="sxs-lookup"><span data-stu-id="a341a-185">The `set-body` element can use an expression to construct a new `JObject` with all the component representations embedded as properties.</span></span>

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

<span data-ttu-id="a341a-186">A teljes házirend a következőképpen néz ki:</span><span class="sxs-lookup"><span data-stu-id="a341a-186">The complete policy looks as follows:</span></span>

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

<span data-ttu-id="a341a-187">A helyőrző konfigurációjában konfigurálhatjuk gyorsítótárazható legalább egy órát, mert az adatok jellege tudjuk az irányítópult erőforrás a művelet azt jelenti, hogy akkor is, ha egy órával elavult, azokat is, hogy elég hatékony, hogy átadja értékes További információ a felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="a341a-187">In the configuration of the placeholder operation we can configure the dashboard resource to be cached for at least an hour because we understand the nature of the data means that even if it is an hour out of date, it will still be sufficiently effective to convey valuable information to the users.</span></span>

## <a name="summary"></a><span data-ttu-id="a341a-188">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="a341a-188">Summary</span></span>
<span data-ttu-id="a341a-189">Az Azure API Management szolgáltatás biztosítja, hogy a HTTP-forgalom szelektív módon alkalmazható rugalmas házirendek, és lehetővé teszi, hogy a háttér szolgáltatások összeállításban.</span><span class="sxs-lookup"><span data-stu-id="a341a-189">Azure API Management service provides flexible policies that can be selectively applied to HTTP traffic and enables composition of backend services.</span></span> <span data-ttu-id="a341a-190">Hogy meg szeretné javítása érdekében a Funkciók, ellenőrzési, érvényesítési képességek riasztási API átjárót, vagy hozzon létre új összetett erőforrások több háttér-szolgáltatásoknak a `send-request` és a lehetőségek tárházát nyissa meg a kapcsolódó házirendek.</span><span class="sxs-lookup"><span data-stu-id="a341a-190">Whether you want to enhance your API gateway with alerting functions, verification, validation capabilities or create new composite resources based on multiple backend services, the `send-request` and related policies open a world of possibilities.</span></span>

## <a name="watch-a-video-overview-of-these-policies"></a><span data-ttu-id="a341a-191">A házirendek áttekintő videó megtekintése</span><span class="sxs-lookup"><span data-stu-id="a341a-191">Watch a video overview of these policies</span></span>
<span data-ttu-id="a341a-192">További információ a [küldési-egy-módon-kérelmek](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest), [küldési-kérelmek](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest), és [visszatérési-válasz](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) a cikkben szereplő házirendek adja a következő videó megtekintése.</span><span class="sxs-lookup"><span data-stu-id="a341a-192">For more information on the [send-one-way-request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest), [send-request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest), and [return-response](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) policies covered in this article, please watch the following video.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Send-Request-and-Return-Response-Policies/player]
> 
> 

