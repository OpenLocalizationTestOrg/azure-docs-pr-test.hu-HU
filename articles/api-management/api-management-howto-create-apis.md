---
title: aaaHow toocreate Azure API Management API-k
description: "Megtudhatja, hogyan toocreate és API-k konfigurálása az Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 14c20da4-f29f-4b28-bec7-3d4c50b734da
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 48ed8d93947253aa1e67ad995927ed6101cac072
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-apis-in-azure-api-management"></a>Hogyan toocreate Azure API Management API-k
Az API Management API ügyfélalkalmazások meghívható műveletkészlet jelöli. Új API-k jönnek létre hello publisher portálon, és majd hello szükséges műveletek kerülnek. Miután hello műveletek kerülnek, a hello API tooa termék kerül, és tehetők közzé. Miután közzétette az API-k, azok előfizetett tooand fejlesztők használják.

Ez az útmutató bemutatja a hello első lépéseként hello folyamat: hogyan toocreate és egy új API-t konfigurálhatja az API Management. Műveletek hozzáadását és közzététele a termék további információkért lásd: [hogyan tooadd műveletek tooan API] [ How tooadd operations tooan API] és [hogyan toocreate és közzététele egy termék] [ How toocreate and publish a product].

## <a name="create-new-api"></a>Egy új API-t létrehozni
API-k létrehozása és hello publisher portal konfigurálva. tooaccess hello publisher portálon kattintson **Publisher portal** a hello Azure portál, az API Management szolgáltatás.

![Közzétevő portál][api-management-management-console]

> Ha még nem hozott létre az API Management szolgáltatáspéldány, lásd: [hozzon létre egy API-kezelés szolgáltatás példányt] [ Create an API Management service instance] a hello [Ismerkedés az Azure API Management] [ Get started with Azure API Management] oktatóanyag.
> 
> 

Kattintson a **API-k** a hello **API Management** hello maradt, és válassza a menü **API hozzáadása**.

![API-t létrehozni][api-management-create-api]

Használjon hello **adja hozzá az új API** ablak tooconfigure hello új API-t.

![Új API hozzáadása][api-management-add-new-api]

a következő mezők hello használt tooconfigure hello új API.

* **Webes API neve** hello API egyedinek és leírónak elnevezi. Hello fejlesztői és publisher portálon megjelenik.
* **Webszolgáltatás URL-címe** hivatkozások hello végrehajtási hello API HTTP-szolgáltatás. Az API management toothis cím továbbítja a kérelmeket.
* **Webes API URL-címe utótag** hozzáfűzött toohello alap URL-cím hello API management szolgáltatás. hello alap URL-cím esetében gyakori, az API Management szolgáltatás példánya által üzemeltetett minden API-k. API Management az API-k által az utótag különbözteti meg, és ezért hello utótag minden API-hoz. a megadott közzétevő egyedinek kell lennie.
* **Webes API URL-séma** határozza meg, mely protokollok használt tooaccess hello API lehet. HTTPs alapértelmezés szerint van megadva.
* toooptionally hozzáadni az új API tooa terméket, kattintson a hello **(nem kötelező) termékek** legördülő listán, és válassza ki a termék. Ez a lépés tooadd hello API toomultiple termékek ismételt többször is lehet.

Amikor hello szükséges értékek vannak konfigurálva, kattintson a **mentése**. Hello új API létrehozása után hello hello API az összefoglalás lapon megjelenik hello publisher portálon.

![API összefoglaló][api-management-api-summary]

## <a name="configure-api-settings"></a>Konfigurálhatja az API-beállítások
Használhatja a hello **beállítások** tooverify lapra, és az API-k a hello konfigurációjának a szerkesztésével. **Webes API neve**, **webszolgáltatás URL-címe**, és **webes API URL-címe utótag** kezdetben állítottak hello API jön létre, és módosíthatja itt. **Leírás** biztosít az opcionális leírást, és **webes API URL-séma** határozza meg, mely protokollok használt tooaccess hello API lehet.

![Az API-beállítások][api-management-api-settings]

tooconfigure átjáró hitelesítési hello háttér Service végrehajtási hello API, válassza ki a hello **biztonsági** külön-külön hello **hitelesítő adatokkal rendelkező** legördülő lehet használt tooconfigure **HTTP Alapszintű** vagy **ügyféltanúsítványok** hitelesítés. toouse HTTP alapszintű hitelesítés, egyszerűen adja meg a hello szükséges hitelesítő adatokat. Ügyféltanúsítvány-alapú hitelesítés használatával kapcsolatos információkért lásd: [hogyan tanúsítvány toosecure háttér-szolgáltatásaihoz ügyfél-hitelesítés az Azure API Management][How toosecure back-end services using client certificate authentication in Azure API Management].

Hello **biztonsági** lapon is használt tooconfigure **felhasználói engedélyezési** az OAuth 2.0 verziót használja. További információkért lásd: [hogyan tooauthorize fejlesztői fiókok az Azure API Management OAuth 2.0-s][How tooauthorize developer accounts using OAuth 2.0 in Azure API Management].

![Egyszerű hitelesítési beállítások][api-management-api-settings-credentials]

Kattintson a **mentése** toosave módosítások toohello API-beállítások.

## <a name="next-steps"></a>Következő lépések
Miután az API-k létrehozása, és hello beállításai, hello lépések vannak tooadd hello műveletek toohello API-t hello API tooa termék hozzáadása, és közzéteszi az, hogy elérhető a fejlesztők számára. További információkért tekintse meg a következő cikkek hello.

* [Hogyan tooadd műveletek tooan API][How tooadd operations tooan API]
* [Hogyan toocreate és a termék közzététele][How toocreate and publish a product]

[api-management-create-api]: ./media/api-management-howto-create-apis/api-management-create-api.png
[api-management-management-console]: ./media/api-management-howto-create-apis/api-management-management-console.png
[api-management-add-new-api]: ./media/api-management-howto-create-apis/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-create-apis/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-create-apis/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-create-apis/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-create-apis/api-management-echo-operations.png

[What is an API?]: #what-is-api
[Create a new API]: #create-new-api
[Configure API settings]: #configure-api-settings
[Configure API operations]: #configure-api-operations
[Next steps]: #next-steps

[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How toocreate and publish a product]: api-management-howto-add-products.md

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[How toosecure back-end services using client certificate authentication in Azure API Management]: api-management-howto-mutual-certificates.md
[How tooauthorize developer accounts using OAuth 2.0 in Azure API Management]: api-management-howto-oauth2.md
