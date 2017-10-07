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
# <a name="custom-caching-in-azure-api-management"></a>Egyéni gyorsítótárazása az Azure API Management
Az Azure API Management szolgáltatás rendelkezik beépített támogatása [HTTP-válasz gyorsítótár](api-management-howto-cache.md) hello erőforrás URL-cím segítségével hello kulcsként. hello kulcs által módosítható hello segítségével kérelemfejléc `vary-by` tulajdonságait. Ez akkor hasznos, a teljes HTTP-válaszok (más néven felelősséget) gyorsítótárazáshoz, de egyes esetekben hasznos toojust gyorsítótár létrehozása egy részét. új hello [gyorsítótár-keresési-érték](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) és [gyorsítótár-tároló-érték](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) házirendek hello lehetőséget nyújtanak toostore és lekérése tetszőleges darabjai belül a házirend-definíciók adatait. Ez a lehetőség is hozzáadja a korábban bemutatott érték toohello [küldési-kérelmek](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) házirend mivel most gyorsítótárazhatja a válaszok külső szolgáltatásokból.

## <a name="architecture"></a>Architektúra
API-kezelés szolgáltatás egy megosztott / bérlői adatgyorsítótár használ, úgy, hogy, hogy toomultiple egységek méretezése hozzáférés toohello továbbra is megkapja azonos gyorsítótárazott adatokat. Azonban több területi telepítés használatakor vannak független gyorsítótárak belül hello régióban. Esedékes toothis, fontos toonot hello gyorsítótár tekinti a tárolóban, néhány adat, hello csak forrását. Ha volt, és később úgy döntött, hello több területi telepítési tootake előnyeit, haladnak, akik rendelkező ügyfelek hozzáférési toothat gyorsítótárazott adatok elveszhetnek.

## <a name="fragment-caching"></a>Töredék gyorsítótárazása
Nincsenek bizonyos esetekben, ahol válaszokat ad vissza adatokat, olcsóbbá toodetermine, és még marad friss elfogadható időn néhány részét tartalmazzák. Tegyük fel fontolja meg egy szolgáltatás, amely repülési foglalásokat, repülési állapot stb vonatkozó információkat biztosító légitársaság. Hello felhasználó tagja hello légitársaság pontok program, ha azok kellene tootheir aktuális állapot és halmozott távolság vonatkozó információkat is. Eltérő tárolódhat, a felhasználóval kapcsolatos adatokat, de lehet, hogy a válaszok által visszaadott repülési állapotáról és foglalások kívánatos tooinclude. Ezt megteheti egy zónaaláírásnak nevezett töredék gyorsítótárazását. hello elsődleges ábrázolását adhatók vissza használatával token tooindicate valamilyen ahol hello felhasználóval kapcsolatos adatokat beszúrni toobe hello forráskiszolgálóról. 

Vegye figyelembe a következő JSON-válasz egy háttérrendszerből API hello.

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

És a másodlagos helyen erőforrás `/userprofile/{userid}` láthatóhoz hasonló,

```json
{ "username" : "Bob Smith", "Status" : "Gold" }
```

A sorrend toodetermine hello megfelelő felhasználói információ tooinclude igazolnia kell a hello felhasználó, aki tooidentify. Ez a módszer használata függő végrehajtása. Tegyük fel, használok hello `Subject` a jogcím egy `JWT` token. 

```xml
<set-variable
  name="enduserid"
  value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />
```

Ez tároljuk `enduserid` későbbi használatra környezeti változó értékét. hello következő lépésre toodetermine, ha a korábbi kérelmekre már hello felhasználói adatok beolvasását és hello-gyorsítótárában tárolja azt. A hello használjuk `cache-lookup-value` házirend.

```xml
<cache-lookup-value
key="@("userprofile-" + context.Variables["enduserid"])"
variable-name="userprofile" />
```

Ha nem található bejegyzés toohello kulcs értékét, akkor a nem megfelelő hello gyorsítótárában `userprofile` környezeti változó jön létre. Hello segítségével hello keresési hello sikerességének ellenőrizzük `choose` adatfolyam házirend szabályozza.

```xml
<choose>
    <when condition="@(!context.Variables.ContainsKey("userprofile"))">
        <!-- If hello userprofile context variable doesn’t exist, make an HTTP request tooretrieve it.  -->
    </when>
</choose>
```

Ha hello `userprofile` környezeti változó nem létezik, majd folyamatban, amelyet toohave toomake HTTP kérelem tooretrieve azt.

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

Hello használjuk `enduserid` tooconstruct hello URL-cím toohello felhasználói profil erőforrás. Tudunk hello választ, ha azt lekéréses hello szövegtörzs hello választ ki, és újra üzembe a környezeti változó tárolja.

```xml
<set-variable
    name="userprofile"
    value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />
```

tooavoid velünk, hogy toomake a HTTP-kérelem újra, amikor hello ugyanaz a felhasználó egy másik kérelmet, azt tárolhat hello felhasználói profil hello gyorsítótár.

```xml
<cache-store-value
    key="@("userprofile-" + context.Variables["enduserid"])"
    value="@((string)context.Variables["userprofile"])" duration="100000" />
```

Hello érték hello pontos azonos kulccsal, hogy azt eredetileg megkísérelt tooretrieve hello gyorsítótárában tároljuk azt. hello időtartamot, választjuk toostore hello érték alapján milyen gyakran hello változtatások és hogyan hibatűrő felhasználók is tooout a naprakész információkat. 

Fontos, hogy hello gyorsítótár lekérése még egy folyamaton kívül, a hálózati kérelmek toorealize és potenciálisan továbbra is felvehetőek ezredmásodperc toohello kérelem több. hello előnyei származnak, amikor meghatározó hello felhasználói profillal kapcsolatos információk hosszabb időbe telik jelentősen, amely megfelelő tooneeding toodo adatbázis-lekérdezést vagy több biztonsági-végpontok összesített adatait.

hello hello folyamat utolsó lépése tooupdate hello választ a felhasználó profil adataival.

```xml
<!-- Update response body with user profile-->
<find-and-replace
    from='"$userprofile$"'
    to="@((string)context.Variables["userprofile"])" />
```

Elfogadása tooinclude hello idézőjelek hello jogkivonat részeként, hogy akkor is, ha a név felülírandó hello nem következik be, hello válasz volt-e továbbra is érvényes JSON-adatokat. Elsősorban hibakeresési könnyebb toomake volt.

Együtt egyesíteni ezeket a lépéseket, miután a hello végeredménynek egy olyan házirend, egyet a következő hello tűnik.

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

Ez a megközelítés gyorsítótárazási elsősorban a webhelyeken ahol HTML állnak hello kiszolgáló oldalán, hogy egyetlen lapként megjeleníthetők. Azonban ez is hasznos lehet az API-k, ahol az ügyfelek nem ügyfél oldalán található HTTP gyorsítótárazását, vagy kívánatos nem tooput hello ügyfél, amely felelős.

A töredék gyorsítótárazás ugyanolyan típusú hello webes a háttérkiszolgálókon lévő a Redis gyorsítótár-kiszolgáló használatával is elvégezhető, azonban hello API-kezelés szolgáltatás tooperform használatával a munkahelyi akkor hasznos, ha különböző biztonsági ends mint hello érkező gyorsítótárazott hello töredék elsődleges válaszokat.

## <a name="transparent-versioning"></a>Transzparens versioning
Általános gyakorlat egy API-toobe támogatott egyszerre több különböző végrehajtási verziója. Ez lehet, hogy toosupport különböző környezetekben, például a fejlesztői, tesztelési, éles, stb., vagy lehet, hogy toosupport hello API toogive idő API fogyasztók toomigrate toonewer verzióihoz régebbi verzióit. 

Ehelyett ez az ügyfél fejlesztők toochange hello URL-címei igénylő egyik módszer toohandling `/v1/customers` túl`/v2/customers` hello felhasználói profil adatok toostore van hello API verziójának azok jelenleg toouse kívánja, és hívja meg hello megfelelő háttérkiszolgáló URL-címe. Rendelés toodetermine hello megfelelő háttér URL-cím toocall egy adott ügyfél számára, hogy a rendszer szükséges tooquery egyes konfigurációs adatokat. A konfigurációs adatok gyorsítótárazása, azt is lecsökkentheti hello teljesítményét, ez a keresés során.

hello első lépése a, toodetermine hello azonosítója használt tooconfigure hello kívánt verziójával. Ebben a példában elfogadása tooassociate hello verzió toohello előfizetés termékkulcsot. 

```xml
<set-variable name="clientid" value="@(context.Subscription.Key)" />
```

A gyorsítótár keresési toosee azt majd hajtsa végre, ha azt már beolvasását hello kívánt ügyfél verziója.

```xml
<cache-lookup-value
key="@("clientversion-" + context.Variables["clientid"])"
variable-name="clientversion" />
```

Ezután toosee azt ellenőrizze, hogy azt nem találta azt a hello gyorsítótárban.

```xml
<choose>
    <when condition="@(!context.Variables.ContainsKey("clientversion"))">
```

Ha azt nem, akkor nyissa meg azt, és lekéréséhez.

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

Hello válasz törzsében szöveg kinyerése hello válasz.

```xml
<set-variable
      name="clientversion"
      value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />
```

Vissza tárolása a későbbi használatra hello gyorsítótár.

```xml
<cache-store-value
      key="@("clientversion-" + context.Variables["clientid"])"
      value="@((string)context.Variables["clientversion"])"
      duration="100000" />
```

És végül frissítse hello háttér-URL-cím tooselect hello hello szolgáltatás hello ügyfél szükséges.

```xml
<set-backend-service
      base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
```

hello teljesen házirendet a következőképpen történik.

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

API fogyasztók tootransparently vezérlő melyik háttérrendszer verzió ügyfelek tooupdate és helyezze üzembe újra ügyfelek anélkül is hozzáférnek engedélyezése egy elegáns megoldás, amely sok API versioning vonatkozik.

## <a name="tenant-isolation"></a>Bérlők elszigetelésére
A nagyobb, a több-bérlős telepítésekben egyes vállalatok bérlők külön csoportot létre háttér hardver különböző üzemelő példányok esetében. Ez minimalizálja a hello hello háttérkiszolgálón a hardver probléma által érintett felhasználók száma. Emellett lehetővé teszi az új szoftverfrissítési verziók toobe szakaszaiban megkezdődött. Ideális esetben a háttér-architektúra átlátszó tooAPI fogyasztók kell lennie. Ez megvalósítható a egy hasonló módon tootransparent versioning mert hello alapul azonos módszerrel kezelésére hello háttérkiszolgáló URL-címet konfiguráció állapota / API-kulcs használatával.  

Helyett visszaadó API hello minden előfizetés kulcs egy előnyben részesített verziója, a bérlő hozzárendelt toohello hardvercsoport kapcsolódó azonosítót alakítanák vissza. Ilyen azonosítójú használt tooconstruct hello megfelelő háttérkiszolgáló URL-cím lehet.

## <a name="summary"></a>Összefoglalás
hello szabadsága toouse hello bármilyen típusú adatok tárolásához Azure API management gyorsítótár lehetővé teszi, hogy hatékony hozzáférés tooconfiguration, amelyek hatással lehetnek a hello módja egy bejövő kérelem feldolgozását. Lehet, hogy is kiegészítheti a válaszokat, egy háttér-API által visszaadott használt toostore adatok töredék is.

## <a name="next-steps"></a>Következő lépések
Adjon küldjön visszajelzést a hello disqus-beszélgetésben teheti a szál az az ebben a témakörben ha vannak a többi olyan forgatókönyvet, hogy ezeket a házirendeket az Ön engedélyezte, vagy ha vannak forgatókönyvek szeretné tooachieve, de nem érzi, hogy jelenleg lehetségesek.

