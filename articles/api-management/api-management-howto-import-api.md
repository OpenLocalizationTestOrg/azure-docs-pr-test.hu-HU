---
title: az Azure API Management az API-k aaaImport |} Microsoft Docs
description: "Megtudhatja, hogyan tooimport az API-k és az Azure API Management azokat a műveleteket."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 40398b0a-ac2c-43f0-89e1-07e4abbf502f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 20fbbb53243aecc24d72833ec0904ae8fab97863
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooimport-hello-definition-of-an-api-with-operations-in-azure-api-management"></a>Hogyan tooimport hello műveleteket az Azure API Management az API-k meghatározása
Az API Management hozhatók létre új API-k, és manuálisan hozzáadni hello műveleteket vagy a hello API együtt egy lépésben hello műveletek importálhatók.

API-k és a műveletek a következő formátumok hello segítségével lehet importálni.

* WADL
* Swagger

Ez az útmutató azt mutatja be hogyan hozzon létre egy új API-t, és importálni egy lépésben műveletei. Létre manuálisan az API-k és műveletek hozzáadásával kapcsolatos tudnivalókat lásd: [hogyan toocreate API-k] [ How toocreate APIs] és [hogyan tooadd műveletek tooan API] [ How tooadd operations tooan API].

## <a name="import-api"></a>API importálása
API-k létrehozása és hello publisher portal konfigurálva. tooaccess hello publisher portálon kattintson **Publisher portal** a hello Azure portál, az API Management szolgáltatás. Ha még nem hozott létre az API Management szolgáltatáspéldány, lásd: [hozzon létre egy API-kezelés szolgáltatás példányt] [ Create an API Management service instance] a hello [Ismerkedés az Azure API Management] [ Get started with Azure API Management] oktatóanyag.

![Közzétevő portál][api-management-management-console]

Kattintson a **API-k** a hello **API Management** hello maradt, és válassza a menü **API importálása**.

![Importálási API][api-management-import-apis]

Hello **importálási API** ablak esetében, amelyek megfelelnek a toohello három módon tooprovide hello API specification három lappal.

* **A vágólapról** lehetővé teszi a hello kijelölt szövegmezőbe toopaste hello API megadását.
* **A fájl** lehetővé teszi a toobrowse tooand válassza hello fájl hello API-meghatározást tartalmaz.
* **URL-címről** toosupply hello URL-cím toohello előírása hello API lehetővé teszi.

![Importálási API formátum][api-management-import-api-clipboard]

Miután megadta a hello API megadását, hello jobb tooindicate hello meghatározás formátuma hello választógombok használatát. a következő formátumok hello támogatott.

* WADL
* Swagger

Ezután adja meg a **webes API URL-címe utótag**. Ez a hozzáfűzött toohello alap URL-címet a API management szolgáltatás. hello alap URL-cím esetében gyakori, minden olyan API-kezelés szolgáltatás minden példányának üzemeltetett API. API Management az API-k által az utótag különbözteti meg, és ezért hello utótag minden API-nak bizonyos API management szolgáltatás példányainak egyedinek kell lennie.

Amennyiben az összes érték lett megadva, kattintson a **mentése** toocreate hello API és hello kapcsolódó műveletek. 

> [!NOTE]
> Tekintse meg a az oktatóanyagban Swagger formátumú API egyszerű számológép importálására [kezelése az Azure API Management az első API](api-management-get-started.md).
> 
> 

## <a name="export-api"></a> Exportálása az API-k
Továbbá tooimporting az új API-k, exportálhatja az API-k meghatározását hello hello publisher portálról. toodo kattintson **exportálása API** a hello **összefoglaló lapon** , a **API**.

![Exportálás API][api-management-export-api]

WADL vagy a Swagger API-k lehet exportálni. Jelölje ki azt a kívánt formátum hello **mentése**, és válassza ki a hello helyet mely toosave hello fájlban.

![API-t az exportálási formátumát][api-management-export-api-format]

## <a name="next-steps"></a>Következő lépések
Miután az API-k létrehozása, és hello műveletek importált, megtekintheti és konfigurálhatja a esetleges egyéb beállításokat, hello API tooa termék hozzáadása, és közzéteszi az, hogy elérhető a fejlesztők számára. További információkért tekintse meg a következő útmutatók hello.

* [Hogyan tooconfigure API-beállítások][How tooconfigure API settings]
* [Hogyan toocreate és a termék közzététele][How toocreate and publish a product]

[api-management-management-console]: ./media/api-management-howto-import-api/api-management-management-console.png
[api-management-import-apis]: ./media/api-management-howto-import-api/api-management-api-import-apis.png
[api-management-import-api-clipboard]: ./media/api-management-howto-import-api/api-management-import-api-wizard.png
[api-management-export-api]: ./media/api-management-howto-import-api/api-management-export-api.png
[api-management-export-api-format]: ./media/api-management-howto-import-api/api-management-export-api-format.png

[Import an API]: #import-api
[Export an API]: #export-api
[Configure API settings]: #configure-api-settings
[Next steps]: #next-steps

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How toocreate and publish a product]: api-management-howto-add-products.md
[How toocreate APIs]: api-management-howto-create-apis.md
[How tooconfigure API settings]: api-management-howto-create-apis.md#configure-api-settings
