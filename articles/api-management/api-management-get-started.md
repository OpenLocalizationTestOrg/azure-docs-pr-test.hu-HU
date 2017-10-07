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
# <a name="manage-your-first-api-in-azure-api-management"></a>Az első API kezelése az Azure API Management szolgáltatásban
## <a name="overview"></a>Áttekintés
Ez az útmutató bemutatja, hogyan tooquickly Ismerkedés az Azure API Management használata, és az első API-hívás.

## <a name="concepts"></a>Mi az Azure API Management?
Használja az Azure API Management tootake bármilyen háttérrendszerből, és indítsa el a teljes körű API programot alapján.

A gyakori forgatókönyvek a következők:

* **A mobil infrastruktúra biztonságossá tétele** a hozzáférés API-kulcsokkal történő korlátozásával, a szolgáltatásmegtagadási támadások szabályozással történő megelőzésével vagy speciális biztonsági házirendek, például a JWT-jogkivonat alapú érvényesítés használatával.
* **ISV partner ökoszisztéma engedélyezése** gyors partner bevezetési keresztül hello fejlesztői felajánlásával portal és felépítése az API homlokzati toodecouple a belső implementációiból, amelyek nincsenek érett a partner felhasználásához.
* **Egy belső API programot futtat** felajánlásával egy központi helyen hello szervezet toocommunicate hello rendelkezésre állás és a legújabb tooAPIs módosul, változástábla munkahelyi és iskolai fiókok alapján hozzáférésének korlátozásához összes alapján közötti biztonságos csatornán hello API átjáró és a háttérkiszolgáló hello.

hello rendszer a következő összetevők hello tevődik össze:

* Hello **API átjáró** hello végpontja, amely:
  
  * API-t hívja, és továbbítja a dokumentumokat tooyour háttérkiszolgálókon fogad el.
  * Ellenőrzi az API-kulcsokat, JWT-jogkivonatokat, tanúsítványokat és más hitelesítő adatokat.
  * Betartatja a használati kvótákat és a sebességkorlátokat.
  * Átalakítja az API-t a hello parancsprogramok kód módosítása nélkül.
  * Gyorsítótárazza a háttérrendszer válaszait, ahol ez be van állítva.
  * Elemzési céllal naplózza a hívások metaadatait.
* Hello **publisher portal** hello felügyeleti felület, ahol állít be az API program. A következőkre lehet használni:
  
  * API-séma meghatározása vagy importálása.
  * API-k termékekbe csomagolása.
  * Kvóták vagy átalakítások hello API-k a házirendek beállítása.
  * Elemzések lekérése.
  * Felhasználók kezelése.
* Hello **fejlesztői portálján** funkcionál hello fő webalkalmazás jelenlétét a fejlesztők számára, ahol a következőkre:
  
  * Elolvashatják az API-dokumentációt.
  * Próbálja ki az API-k hello interaktív konzolon keresztül.
  * Hozzon létre egy fiókot, és feliratkozhat tooget API-kulcsokat.
  * Hozzáférhetnek a használat adataikról készült elemzésekhez.

## <a name="create-service-instance"></a>API Management-példány létrehozása
> [!NOTE]
> toocomplete ebben az oktatóanyagban egy Azure-fiókra van szüksége. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes fiókot. További információ: [Ingyenes Azure-fiók létrehozása][Azure Free Trial].
> 
> 

az API Management használata első lépéseként hello toocreate egy szolgáltatáspéldány. Jelentkezzen be toohello [Azure Portal] [ Azure Portal] kattintson **új**, **Web + mobil**, **API Management**.

![Új API Management-példány][api-management-create-instance-menu]

A **neve**, adjon meg egy egyedi altartomány neve toouse hello szolgáltatás URL-CÍMÉT.

Válassza ki a kívánt hello **előfizetés**, **erőforráscsoport** és **hely** a szolgáltatáspéldány.

Adja meg **Contoso Ltd** a hello **szervezet neve**, és írja be az e-mail cím hello **rendszergazdai E-Mail** mező.

> [!NOTE]
> Ez az e-mail cím az API Management rendszer hello értesítésekhez használatos. További információkért lásd: [hogyan tooconfigure értesítések és az e-mail sablonok az Azure API Management][How tooconfigure notifications and email templates in Azure API Management].
> 
> 

![Új API Management szolgáltatás][api-management-create-instance-step1]

Az API Management szolgáltatáspéldányok három csomagban érhetők el: Fejlesztői, Standard és Prémium.

> [!NOTE]
> hello fejlesztői réteg fejlesztési, tesztelési és magas rendelkezésre állás esetén nem szempont kísérleti API programok van. A hello Standard és prémium csomag szükséges a fenntartott egységek számának toohandle nagyobb forgalmat is méretezhető. hello Standard és Premium rétegek adja meg az API Management hello szolgáltatást, és a legtöbb feldolgozási teljesítmény. Jelen oktatóanyagot bármely csomaggal elvégezheti. További információt az API Management csomagjairól az [API Management Díjszabás][API Management pricing] oldalon talál.
> 
> 

Kattintson a **létrehozása** kiépítése a szolgáltatáspéldány toostart.

![Új API Management szolgáltatás][api-management-instance-created]

Hello szolgáltatáspéldány létrehozása után tovább hello toocreate vagy importálja az API-k.

## <a name="create-api"></a>API importálása
Az API egy ügyfélalkalmazásokból meghívható műveletkészletből áll. API-műveleteket a proxy tooexisting webes szolgáltatások.

Az API-kat létre lehet hozni (és a műveleteket hozzá lehet adni) manuálisan, vagy importálni is lehet. Ebben az oktatóanyagban fogunk importálni egy minta Számológép webes szolgáltatás, a Microsoft által biztosított és az Azure-platformon futó API hello.

> [!NOTE]
> Az API-k létrehozására, illetve manuális hozzáadásával a műveletek útmutatóért lásd: [hogyan toocreate API-k](api-management-howto-create-apis.md) és [hogyan tooadd műveletek tooan API](api-management-howto-add-operations.md).
> 
> 

API-k hello publisher portálról vannak konfigurálva. tooreach, kattintson a **Publisher portal** hello szolgáltatás eszköztáron.

![Közzétevő portál][api-management-management-console]

tooimport hello Számológép API, kattintson a **API-k** a hello **API Management** hello maradt, és válassza a menü **importálási API**.

![API importálása gomb][api-management-import-api]

Hajtsa végre a következő lépéseket tooconfigure hello Számológép API hello:

1. Kattintson a **az URL**, adja meg **http://calcapi.cloudapp.net/calcapi.json** történő hello **Specification dokumentum URL-cím** szöveg mezőbe, majd kattintson a hello **Swagger**  választógombot.
2. Típus **Számológép** hello történő **webes API URL-címe utótag** szövegmezőben.
3. Kattintson a hello **(nem kötelező) termékek** listát, és válassza **alapszintű**.
4. Kattintson a **mentése** tooimport hello API.

![Új API hozzáadása][api-management-import-new-api]

> [!NOTE]
> Az **API Management** jelenleg az 1.2-es és a 2.0-ás verziójú Swagger-dokumentumok importálását is támogatja. Ügyeljen arra, hogy még ha a [Swagger 2.0 specifikációja](http://swagger.io/specification) szerint a `host`, `basePath` és `schemes` tulajdonságok nem is kötelezőek, a Swagger 2.0-ás dokumentumnak akkor is tartalmaznia **KELL** ezeket a tulajdonságokat, máskülönben nem lesz importálva. 
> 
> 

Hello API importálása után hello hello API az összefoglalás lapon megjelenik hello publisher portálon.

![API összefoglaló][api-management-imported-api-summary]

hello API szakasz több lapot tartalmaz. Hello **összegzés** lap alapvető metrikákat és hello API kapcsolatos információkat jeleníti meg. Hello [beállítások](api-management-howto-create-apis.md#configure-api-settings) a lap majdnem használt tooview és Szerkesztés hello konfigurációs API. Hello [műveletek](api-management-howto-add-operations.md) lap használt toomanage hello API műveletek esetén. Hello **biztonsági** lapon lehet használt tooconfigure átjáró hitelesítés hello háttérkiszolgáló az egyszerű hitelesítés használatával vagy [kölcsönös hitelesítés](api-management-howto-mutual-certificates.md), és tooconfigure [ felhasználói engedélyezési OAuth 2.0 használatával](api-management-howto-oauth2.md).  Hello **problémák** lapon látható az API-kat használó hello fejlesztők által jelentett használt tooview hibák. Hello **termékek** lap, amely tartalmazza az API használt tooconfigure hello termékek.

Alapértelmezés szerint az API Management minden példányához az alábbi két mintatermék jár:

* **Kezdő**
* **Korlátlan**

Ebben az oktatóanyagban hello alapvető Számológép API toohello alapszintű termék bővült hello API importálásakor.

Rendelés toomake hívások tooan API fejlesztők először elő kell fizetnie tooa termék tooit hozzáférést biztosít a számukra. A fejlesztők hello developer portálon tooproducts kérhet le, vagy rendszergazdák fejlesztők tooproducts hello publisher portálon kérhet le. Rendszergazdaként jelentkezett létrehozása óta hello API Management példány hello az előző lépéseket hello oktatóanyagban úgy, hogy Ön már előfizetett tooevery termék alapértelmezés szerint.

## <a name="call-operation"></a>Művelet hívása hello developer portálról
Műveletek hívható közvetlenül a hello fejlesztői portálján, amely egy kényelmes módszert arra tooview biztosít, és tesztelje az API-k hello működésére. Az oktatóanyag lépésben hello alapvető Számológép API telefonhívásokhoz **két egész számok hozzáadása** műveletet. Kattintson a **fejlesztői portálján** hello hello menüből hello publisher portál jobb felső.

![Fejlesztői portál][api-management-developer-portal-menu]

Kattintson a **API-k** hello felső menüben, majd kattintson a **alapvető Számológép** toosee hello elérhető műveletek.

![Fejlesztői portál][api-management-developer-portal-calc-api]

Vegye figyelembe a hello minta leírásokat és paraméterek importált együtt hello API és a műveleteket, dokumentáció hello fejlesztők számára, amely használja ezt a műveletet. Ezeket a leírásokat a műveletek manuális hozzáadásakor is hozzá lehet adni.

toocall hello **két egész számok hozzáadása** műveletet, kattintson a **kipróbálás**.

![Kipróbálom][api-management-developer-portal-calc-api-console]

Adjon meg néhány értéket hello paraméterek vagy hello alapértelmezett tartása, és kattintson **küldése**.

![HTTP Get][api-management-invoke-get]

Egy művelet indítása, miután hello fejlesztői portálján megjeleníti-e a hello **válaszállapot**, hello **válaszfejlécek**, és az egyik **válasz tartalom**.

![Válasz][api-management-invoke-get-response]

## <a name="view-analytics"></a>Elemzés megtekintése
Alapszintű Számológép, kapcsoló hátsó toohello publisher portal kiválasztásával tooview analytics **kezelése** hello hello menüből hello fejlesztői portál jobb felső.

![Kezelés][api-management-manage-menu]

hello hello publisher portál alapértelmezett nézet az hello **irányítópult**, amely áttekintést nyújt az API Management-példány.

![Irányítópult][api-management-dashboard]

Rámutatáskor hello egér keresztül hello diagramot **alapvető Számológép** toosee hello adott mérőszámok hello használatáról hello API egy adott időszakra vonatkozóan.

> [!NOTE]
> Ha a diagram összes sort nem talál, váltson vissza toohello fejlesztői portálján és néhány be hello API-hívások, és várjon egy kis ideig, majd térjen vissza toohello irányítópult.
> 
> 

Kattintson a **részleteinek megtekintése** tooview hello összesítő lapján hello API, többek között a hello megjelenő metrikák nagyobb változatát.

![Elemzés][api-management-mouse-over]

![Összefoglalás][api-management-api-summary-metrics]

Részletes metrikák és jelentések **Analytics** a hello **API Management** hello bal oldali menüben.

![Áttekintés][api-management-analytics-overview]

Hello **Analytics** szakasz hello a következő lap négy fület tartalmaz:

* **Egy pillanat alatt** összesített használatát és állapotfigyelő metrikák, valamint felső fejlesztők, a felső termékek, a felső API-k és a felső műveletek hello biztosít.
* A **Használat** részletes tájékoztatást biztosít az API-hívásokról és a sávszélességről, földrajzi ábrázolással.
* Az **Állapot** az állapotkódokkal, a gyorsítótárazás sikerességének mértékével, a válaszidőkkel, illetve az API-k és szolgáltatások válaszidejével foglalkozik.
* **Tevékenység** részletekbe menően tárhatják jelentéseket biztosít az adott tevékenység hello fejlesztői, a termék, a API és a műveletet.

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[a a sebességhatárok API védelme](api-management-howto-product-with-rules.md).

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
