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
# <a name="using-external-services-from-hello-azure-api-management-service"></a>Az Azure API Management szolgáltatás hello külső szolgáltatások használata
hello házirendek az Azure API Management service-ben rendelkezésre álló számos különböző probléma pusztán a hello bejövő kérelem, hello kimenő válasz és alapvető konfigurációs adatai alapján hasznos munkahelyi teheti meg. Azonban az API Management külső szolgáltatásokkal képes toointeract alatt számos további lehetőségek megnyílik házirendek.

Korábban úgy találtuk, hogy azt léphetnek interakcióba hello [Azure Event Hubs szolgáltatás naplózási, megfigyelésének és elemzés](api-management-log-to-eventhub-sample.md). Ebben a cikkben a házirendekben, amelyek lehetővé teszik toointeract bármely külső HTTP-alapú szolgáltatás bemutatjuk. Ezek a házirendek indítására, távoli események vagy az információt, amely használt toomanipulate hello eredeti kérelem-válasz valamilyen módon lesz használható.

## <a name="send-one-way-request"></a>Küldési-egy-módon-kérelmek
Valószínűleg hello legegyszerűbb külső beavatkozásra hello kérésére, amely lehetővé teszi, hogy egy külső szolgáltatás toobe fontos esemény valamilyen értesítés tűz-és-elfelejti stílusát. Használhatunk hello vezérlő adatfolyam házirend `choose` semmilyen feltételt, azt is érdeklődik, és ezután Ha hello feltétel teljesül, tökéletesíthetjük hello használata külső HTTP-kérelem toodetect [küldési-egy-módon-kérelmek](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) házirend. Ennek oka lehet egy kérelem tooa rendszer Hipchat vagy a Slackhez, illetve egy levelezési API SendGrid vagy MailChimp például üzenetküldési, vagy a kritikus fontosságú támogatási incidensek az alábbihoz hasonló PagerDuty. E üzenetkezelési rendszerek rendelkezik egyszerű HTTP API-k könnyen hívhat meg azt.

### <a name="alerting-with-slack"></a>A Slackhez riasztás
hello a következő példa bemutatja, hogyan toosend egy üzenet tooa Csevegés hely Slack-Ha hello HTTP-válasz állapotkódja érték nagyobb, mint vagy egyenlő too500. Egy 500 tartomány hiba azt jelzi, hogy a háttér-API, amely az API felületen ügyfelének hello problémáját nem maguktól megoldódhatnak. Általában szüksége van valamilyen beavatkozás az eszközeiken.  

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

Slackhez rendelkezik a bejövő webes hurkok hello fogalmát. Egy bejövő webes hook konfigurálásakor a Slackhez egy különleges URL-címet, amely lehetővé teszi egy egyszerű POST kérelem toodo és toopass üzenet hello közzététele a Slack-csatorna a állít elő. hello létrehozhatunk JSON-törzsére Slackhez által meghatározott formátumban alapul.

![Slack webes Hook](./media/api-management-sample-send-request/api-management-slack-webhook.png)

### <a name="is-fire-and-forget-good-enough"></a>Tűz és elég jól elfelejti?
Nincsenek bizonyos mellékhatásokkal kérelem tűz-és-elfelejti stílus használatával. Ha valamilyen okból, hello kérelem sikertelen lesz, majd hello hibája nem fog szerepelni. Ez az adott esetben nem igazolható hello összetettségét, hogy a másodlagos hibájának jelentéskészítő rendszer és az hello további teljesítményarányos költsége hello válaszra való várakozás közben. A forgatókönyvekben, ahol az alapvető toocheck hello választ, majd hello [küldési-kérelmek](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) házirend jobb megoldás.

## <a name="send-request"></a>Küldési-kérelmek
Hello `send-request` házirend lehetővé teszi, hogy egy külső szolgáltatás tooperform összetett feldolgozási funkciók és további házirend feldolgozásának nem használható visszatérési adatainak toohello API felügyeleti szolgáltatás használatával.

### <a name="authorizing-reference-tokens"></a>Referencia-tokenek engedélyezése
Az API Management főbb funkcióját háttér erőforrások védi. Ha létrehozza az API által használt hello engedélyezési kiszolgáló [JWT-jogkivonatokat](http://jwt.io/) az OAuth2 folyamat részeként, [Azure Active Directory](../active-directory/active-directory-aadconnect.md) Igen, akkor használhatja a hello `validate-jwt` házirend tooverify hello érvényességét hello lexikális eleme. Azonban néhány engedélyezési kiszolgálók létrehozása, mi nevezzük [jogkivonatok hivatkozhat](http://leastprivilege.com/2015/11/25/reference-tokens-and-introspection/) anélkül, hogy a hívás hátsó toohello engedélyezési kiszolgáló, amely nem ellenőrizhető.

### <a name="standardized-introspection"></a>Szabványos introspection
Az elmúlt hello nincs szabványosított lehetőség van egy engedélyezési kiszolgáló és a hivatkozás-jogkivonat ellenőrzése le lett. Azonban a közelmúltban javasolt szabvány [RFC 7662](https://tools.ietf.org/html/rfc7662) hello IETF, amely meghatározza, hogyan ellenőrizheti egy erőforrás-kiszolgáló, a token érvényességének hello tett közzé.

### <a name="extracting-hello-token"></a>Hello jogkivonat beolvasása
hello első lépéseként tooextract hello jogkivonatot hello engedélyezési fejléc. hello állomásfejléc-érték formátumban kell lennie a hello `Bearer` engedélyezési sémát, egy szóközzel és majd hello engedélyezési jogkivonatot megfelelően [RFC 6750](http://tools.ietf.org/html/rfc6750#section-2.1). Sajnos előfordulhatnak olyan esetek, ahol hello hitelesítési séma meg van adva. a tooaccount elemzésekor, azt vágási hello Fejlécérték egy helyet, és válassza hello utolsó karakterláncnak a következőről hello karakterláncokból álló tömböt adott vissza. Így lehetővé teszi a probléma megoldásához rosszul formázott engedélyezési fejléceket.

```xml
<set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />
```

### <a name="making-hello-validation-request"></a>Hello ellenőrzési kérés
Miután tudunk hello engedélyezési jogkivonat, tökéletesíthetjük hello kérelem toovalidate hello jogkivonat. RFC 7662 meghívja a folyamat introspection, és megköveteli, hogy Ön `POST` HTML formátumban toohello introspection erőforrás. HTML-űrlaphoz hello legalább tartalmaznia kell egy kulcs/érték pár hello kulccsal `token`. A kérelem toohello engedélyezési kiszolgáló is kell lennie, amely rosszindulatú ügyfelek nem Ugrás ezeken érvényes jogkivonatokat hitelesített tooensure.

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

### <a name="checking-hello-response"></a>Hello válasz ellenőrzése
Hello `response-variable-name` attribútum használt toogive hozzáférés hello választ adott vissza. hello név ebben a tulajdonságban meghatározott használható kulcsként történő hello `context.Variables` szótár tooaccess hello `IResponse` objektum.

Hello válasz objektumból hello törzs beolvasható és RFC 7622 közli velünk, hogy hello válasz egy JSON-objektumnak kell lennie, és tartalmaznia kell legalább egy tulajdonságot nevű `active` , amely egy logikai érték. Ha `active` értéke igaz, akkor hello token érvényes.

### <a name="reporting-failure"></a>Jelentéskészítési hiba
Használjuk a `<choose>` házirend toodetect hello token érvénytelen, és ha igen, térjen vissza a 401-es választ.

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

Megfelelően [RFC 6750](https://tools.ietf.org/html/rfc6750#section-3) amely ismerteti, hogyan `bearer` jogkivonatok kell használni, azt is adja vissza egy `WWW-Authenticate` hello 401-es válasz fejléce. WWW-Authenticate hello tervezett tooinstruct egy ügyfelet, hogy miként tooconstruct megfelelően jogosult kérelmet. Hello OAuth2-keretrendszerben készült lehetséges módszer számos toohello, miatt is nehézkes toocommunicate hello összes szükséges információt. Szerencsére nincsenek erőfeszítéseket folyamatban toohelp [ügyfelek felderítése hogyan tooproperly kérelmek tooa erőforrás kiszolgáló engedélyezése](http://tools.ietf.org/html/draft-jones-oauth-discovery-00).

### <a name="final-solution"></a>Végső megoldás
Üzembe összes hello kódrészletek, azt lekérése hello házirendet a következő:

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

Ez sok példák közül csak az hello `send-request` házirend lehet használt toointegrate hasznos külső szolgáltatások kérések és válaszok API-kezelés szolgáltatás hello átfutó hello folyamatba.

## <a name="response-composition"></a>Válasz összeállítás
Hello `send-request` házirend használható egy elsődleges kérelem tooa háttérrendszer növelésére, az előző példában hello látott, vagy azt egy teljes cserélje le a hello háttér hívás használható. Ez a módszer használatával könnyen létrehozhatjuk összesítése összetett erőforrásokat több különböző rendszerekből.

### <a name="building-a-dashboard"></a>Irányítópult létrehozása
Néha toobe képes tooexpose információt szeretne, hogy megtalálható-e több háttérrendszerek, például toodrive egy irányítópultot. hello KPI-k minden különböző biztonsági-végpontok, származhat, de nem tooprovide közvetlen hozzáférést toothem inkább, és célszerű, ha minden hello információi olvashatók az egy kérelemhez. Lehet, hogy néhány hello háttér-információkat kell néhány szeletelhető és feldarabolható, és először egy kis tisztítására! Tudja, hogy összetett erőforrás lenne egy hasznos tooreduce toocache alatt hello háttér tölthető be, mint tudja a felhasználók rendelkeznek-e egy válhat a beütés hello F5 billentyűt a rendelés toosee, ha azok underperforming metrikák változhatnak.    

### <a name="faking-hello-resource"></a>Faking hello erőforrás
első lépés toobuilding hello az irányítópult erőforrás tooconfigure hello API Management publisher portálon új művelet. Ez lesz a helyőrző használt művelet tooconfigure az összeállítás házirend toobuild a dinamikus erőforrás.

![Irányítópult-művelet](./media/api-management-sample-send-request/api-management-dashboard-operation.png)

### <a name="making-hello-requests"></a>Hello kérést
Egyszer hello `dashboard` művelet létrejött konfigurálhatjuk egy kifejezetten az adott műveletre vonatkozó házirendet. 

![Irányítópult-művelet](./media/api-management-sample-send-request/api-management-dashboard-policy.png)

első lépés hello van a hello bejövő kérelem, a lekérdezési paramétereket tooextract, hogy azt is továbbítják őket tooour háttér. Ebben a példában az irányítópulton látható információk alapján egy adott időn belül egy ezért egy `fromDate` és `toDate` paraméter. Használhatunk hello `set-variable` házirend tooextract hello adatait hello kérelem URL-CÍMÉT.

```xml
<set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
<set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">
```

Ha ezt az információt kell tökéletesíthetjük tooall hello háttérrendszerek kérelmeket. Minden kérést hoz létre egy új URL-cím elé hello paraméterinformáció, és meghívja a megfelelő kiszolgálóhoz, és hello válasz egy környezeti változó tárolja.

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

Ezek a kérelmek végrehajtja a sorozatban, ami nem ideális. Egy jövőbeli verzióban azt fogja bevezetése nevű új házirend `wait` , amely engedélyezi ezeket kérelmek tooexecute párhuzamosan mindegyikét.

### <a name="responding"></a>Válaszol
tooconstruct hello összetett válasz használhatjuk hello [visszatérési-válasz](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) házirend. Hello `set-body` elem használható egy kifejezésben tooconstruct egy új `JObject` az összes hello összetevő felelősséget tulajdonságként beágyazott.

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

hello teljes házirendet a következőképpen néz ki:

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

Hello konfigurációban hello helyőrző művelet konfigurálhatjuk hello irányítópult erőforrás toobe gyorsítótárba helyezett legalább egy órával mert tudjuk hello adatok hello jellege azt jelenti, hogy akkor is, ha egy elavult, még mindig elég hatékony óra tooconvey értékes információk toohello felhasználók.

## <a name="summary"></a>Összefoglalás
Az Azure API Management szolgáltatás szelektív módon lehet rugalmas házirendek alkalmazása tooHTTP forgalom biztosít, és lehetővé teszi, hogy a háttér szolgáltatások összeállításban. Szeretné, hogy a riasztás funkciók, ellenőrzési, érvényesítési képességek API átjáró tooenhance, vagy hozzon létre új összetett erőforrások több háttér-szolgáltatásoknak, hello `send-request` és a lehetőségek tárházát nyissa meg a kapcsolódó házirendek.

## <a name="watch-a-video-overview-of-these-policies"></a>A házirendek áttekintő videó megtekintése
További információt a hello [küldési-egy-módon-kérelmek](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest), [küldési-kérelmek](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest), és [visszatérési-válasz](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) a cikkben szereplő házirendek adjon tekintse meg a következő videó hello.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Send-Request-and-Return-Response-Policies/player]
> 
> 

