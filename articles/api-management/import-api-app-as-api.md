---
title: "API-alkalmazás importálása egy API-t az Azure portálon |} Microsoft Docs"
description: "Ez az oktatóanyag bemutatja, hogyan API Management (APIM) segítségével importálja az API-alkalmazásba, az API-k."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/22/2017
ms.author: apimpm
ms.openlocfilehash: d0e1aa6763d96b5a84bbc5fba7dcae690c051eb3
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/04/2017
---
# <a name="import-an-api-app-as-an-api"></a>Az API-k API-alkalmazás importálása

Ez a cikk bemutatja, hogyan importálhatja az API-alkalmazások, mint az API-k. A cikk azt is bemutatja, hogyan tesztelheti a APIM API.

Ebből a cikkből megismerheti, hogyan:

> [!div class="checklist"]
> * Az API-k API-alkalmazás importálása
> * Az API tesztelése az Azure-portálon
> * Az API-t a fejlesztői portálra tesztelése

## <a name="prerequisites"></a>Előfeltételek

+ Fejezze be a következő gyorsindítási: [Azure API Management példányt létrehozni](get-started-create-service-instance.md)
+ Ellenőrizze, hogy egy API-alkalmazás van az előfizetésben. További információ a dokumentációban [App Service] [https://docs.microsoft.com/azure/app-service/]

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"></a>Importálása és a háttér-API közzététele

1. Válassza ki **API-k** a alatt **API MANAGEMENT**.
2. Válassza ki **API-alkalmazás** a a **hozzáadása egy új API** listája.

    ! (API-alkalmazás) [. / media/import-api-app-as-api/api-app.png]
3. Nyomja le az **Tallózás** az előfizetésében API-alkalmazások listájának megtekintéséhez.
4. Válassza ki az alkalmazást. APIM megkeresi a kijelölt alkalmazáshoz társított swagger lekéri azt, és importálja azokat. 

    Abban az esetben APIM nem talál swagger, akkor az API-t egy "csatlakoztatott" API mutatja. 
5. Adjon hozzá egy API URL-címe utótagot. A utótag, amely azonosítja az adott API-nak a APIM példány nevét. Rendelkezik a APIM példány belül egyedinek kell lennie.
6. Tegye közzé az API által az API-t társít egy termék. Ebben az esetben a "*korlátlan*" termék szolgál.  Ha azt szeretné, az API számára tehető közzé, és a fejlesztők számára érhető el, adja hozzá a termék. API létrehozása során teheti meg, vagy állítsa be úgy később.

    A termékeket társítását, egy vagy több API-k. Számos olyan API-k, és a fejlesztői portálon keresztül a fejlesztők számára biztosíthat számukra. A fejlesztők a termék az API eléréséhez először elő kell fizetnie. Fizet elő, amikor azok beolvasása, amely a termék API-k ideális előfizetés kulcsa. A APIM példányt hozott létre, ha rendszergazdaként jelentkezett már, így minden egyes termék előfizetett alapértelmezés szerint.

    Alapértelmezés szerint az API Management minden példányához az alábbi két mintatermék jár:

    * **Kezdő**
    * **Korlátlan**   
7. Kattintson a **Létrehozás** gombra.

## <a name="test-the-new-apim-api-in-the-azure-portal"></a>Az új APIM API tesztelése az Azure-portálon

Műveletek hívható közvetlenül a megtekintése, és az API-k működésének teszteléséhez kényelmes megoldást kínál az Azure portálon.  

1. Válassza ki az előző lépésben létrehozott API-t.
2. Nyomja meg a **teszt** fülre.
3. Válassza ki a valamilyen művelet.

    A lap megjeleníti a lekérdezés-paraméterek és a fejlécek mezőket. A fejléc egyik "Ocp-Apim-előfizetés-kulcsot", a termék, ez az API társított előfizetés kulcshoz. A APIM példányt hozott létre, ha rendszergazdaként jelentkezett már, így a kulcs automatikusan kitölti. 
1. Nyomja le az **küldése**.

    Háttér válaszol, **200 OK** és néhány adat.

## <a name="call-operation"></a>Művelet meghívása a fejlesztői portálról

Műveletek is hívható **fejlesztői portálján** API-k teszteléséhez. 

1. Válassza ki a létrehozott API a "Import és közzététele egy háttér-API" lépéssel.
2. Nyomja le az **fejlesztői portálján**.

    A "Fejlesztői portálján" hely megnyílik.
3. Válassza ki a **API** létrehozott.
4. Kattintson a vizsgálni kívánt műveletet.
5. Nyomja le az **kipróbálás**.
6. Nyomja le az **küldése**.
    
    A művelet meghívása után a fejlesztői portál megjeleníti a **Válasz állapota**, a **Válasz fejlécei** és a **Válasz tartalma** minden információját.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Következő lépések

> [!div class="nextstepaction"]
> [Az átalakítási és egy közzétett API védelme](transform-api.md)