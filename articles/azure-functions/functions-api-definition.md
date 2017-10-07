---
title: az Azure Functions aaaOpenAPI metaadatok |} Microsoft Docs
description: "OpenAPI támogatása az Azure Functions áttekintése"
services: functions
documentationcenter: 
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 03/23/2017
ms.author: alkarche
ms.openlocfilehash: fff8f14110469a002a6c9dca03f641672003a3a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="openapi-20-metadata-support-in-azure-functions-preview"></a>OpenAPI 2.0 metaadatok támogatása az Azure Functions (előzetes verzió)
OpenAPI 2.0 (korábbi nevén Swagger) metaadat-támogatás az Azure Functions nem használható egy OpenAPI 2.0 toowrite definition belül egy függvény alkalmazás előzetes verziójú funkciók. Majd tárolhatja, hogy a fájl hello függvény alkalmazás használatával.

[OpenAPI metaadatok](http://swagger.io/) lehetővé teszi, hogy egy függvény, amely üzemelteti a REST API toobe számos más szoftverek által használt. Ezek közé tartozik például a PowerApps és hello Microsoft ajánlatok [az Azure App Service API Apps szolgáltatásának](https://docs.microsoft.com/azure/app-service-api/app-service-api-dotnet-get-started#a-idcodegena-generate-client-code-for-the-data-tier), külső fejlesztői eszközök, például [Postman](https://www.getpostman.com/docs/importing_swagger), és [számos további csomag](http://swagger.io/tools/).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

>[!TIP]
>Javasoljuk, hogy hello kezdve [használatába bevezető oktatóanyagot](./functions-api-definition-getting-started.md) és majd toothis dokumentum toolearn kapcsolatos funkciók.

## <a name="enable"></a>OpenAPI definition támogatásának engedélyezése
Képes a beállításokat minden OpenAPI hello **API-definíció** lap az függvény alkalmazás **Platform funkciói**.

tooenable hello generálása egy üzemeltetett OpenAPI definíció- és a gyors üzembe helyezés definition beállítása **API definition forrás** túl**függvény (előzetes verzió)**. **Külső URL-cím** lehetővé teszi a funkció toouse egy OpenAPI-definíciót, amely máshol tárolt rendelkezik.

## <a name="generate-definition"></a>A dbfunctionexpression hozzon létre egy Swagger vázat
Sablon segíthetnek az első OpenAPI definition írásához. hello definition sablon funkció használatával hozza létre ritka OpenAPI definícióját minden hello metaadatok hello function.json fájlban az egyes a HTTP-eseményindító funkciók. Az API-t a hello kapcsolatos további információt a toofill lesz szüksége [OpenAPI specification](http://swagger.io/specification/), például a kérés- és sablonok.

Részletes útmutatásért lásd: hello [használatába bevezető oktatóanyagot](./functions-api-definition-getting-started.md).

### <a name="templates"></a>Az elérhető sablonok

|Név| Leírás |
|:-----|:-----|
|Generált meghatározása|A maximális méretű hello következtetni lehet a meglévő metaadatok hello függvény információkat OpenAPI definíciója.|

### <a name="quickstart-details"></a>Generált hello definícióban belefoglalt metaadatok

a következő tábla jelöli hello hello function.json lévő megfelelő adatok és az Azure portál beállításai, mert az előállított csatlakoztatott toohello Swagger vázat.

|Swagger.JSON|Portál felhasználói felület|Function.JSON|
|:----|:-----|:-----|
|[Állomás](http://swagger.io/specification/#fixed-fields-15)|**Alkalmazásbeállítások működéséhez** > **App Service-beállítások** > **áttekintése** > **URL-címe**|*Nem található*
|[Elérési utak](http://swagger.io/specification/#paths-object-29)|**Integrálni** > **kijelölt HTTP-metódus**|Kötések: útvonal
|[Elérési út elem](http://swagger.io/specification/#path-item-object-32)|**Integrálni** > **útvonalsablonhoz**|Kötések: módszerek
|[Biztonság](http://swagger.io/specification/#security-scheme-object-112)|**Kulcsok**|*Nem található*|
|OperationID azonosítójú *|**Útvonal + engedélyezett műveletek**|Útvonal + engedélyezett műveletek|

\*hello műveletet azonosító megadása kötelező, csak a PowerApps és Flow integrálásához.
> [!NOTE]
> hello x-ms-összegzés bővítmény biztosít a Logic Apps, PowerApps és Flow megjelenítendő nevét.
>
> több, lásd: toolearn [testre szabhatja a Swagger-definícióval PowerApps](https://powerapps.microsoft.com/tutorials/customapi-how-to-swagger/).

## <a name="CICD"></a>CI/CD tooset API definition használata

 Engedélyeznie kell a API definition üzemeltető hello portálon forrás vezérlő toomodify az API-definíció verziókövetésből engedélyezése előtt. Kövesse az alábbi utasításokat:

1. Keresse meg a túl**API Definition (előzetes verzió)** az függvény alkalmazás beállításaiban.
  1. Állítsa be **API definition forrás** túl**függvény**.
  1. Kattintson a **készítése API definition sablon** , majd **mentése** toocreate később módosítása sablon definícióját.
  1. Vegye figyelembe a API-definíció URL-címe és a kulcsot.
1. [A folyamatos integrációs/folyamatos üzembe helyezés (CI/CD) beállítása](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment#continuous-deployment-requirements).
2. Módosítsa a következő \site\wwwroot verziókezelő swagger.json\.azurefunctions\swagger\swagger.json.

Most a módosításokat a tárházba tooswagger.json hello API-definíció URL-címe az függvény alkalmazást által tárolt és a kulcs lépés 1.c.

## <a name="next-steps"></a>Következő lépések
* [Használatába bevezető oktatóanyagot](functions-api-definition-getting-started.md). Próbálja meg a forgatókönyv toosee egy OpenAPI definíciója működés közben.
* [Az Azure Functions GitHub-tárházban](https://github.com/Azure/Azure-Functions/). Tekintse meg hello funkciók tárház toogive nekünk visszajelzést a hello API definition támogatási Preview-ban. A GitHub probléma létrehozása semmit toosee frissíteni szeretné.
* [Az Azure Functions fejlesztői segédanyagai](functions-reference.md). További tudnivalók a függvények kódolásához, valamint eseményindítók és kötések meghatározásához.
