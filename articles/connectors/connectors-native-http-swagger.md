---
title: "kiegészítve HTTP Swagger aaaCall REST-végpontok Azure Logic Apps-összekötője |} Microsoft Docs"
description: "Csatlakozás tooREST végpontok a logic Apps alkalmazásokból keresztül Swagger kiegészítve hello HTTP Swagger-összekötő"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: eccfd87c-c5fe-4cf7-b564-9752775fd667
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan; LADocs
ms.openlocfilehash: baaa57689ff41fcd052f9d86086e36619ddec46e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-http--swagger-action"></a>Ismerkedés a hello HTTP + Swagger művelet

Első osztályú összekötő tooany REST-végpont használatával hozhat létre egy [Swagger-dokumentum](https://swagger.io) használatakor hello HTTP + Swagger műveleti a logic app munkafolyamat. Logic apps toocall bármely REST-végpont első osztályú Logic App Designer nyújthassunk is kiterjeszthető.

Hogyan toocreate a logic apps, összekötőkkel: toolearn [új logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="use-http--swagger-as-a-trigger-or-an-action"></a>A HTTP + Swagger egy eseményindító vagy egy műveletet

hello HTTP + Swagger eseményindítója és tevékenysége fel munkahelyi hello ugyanaz, mint hello [HTTP-művelet](connectors-native-http.md) jelentkezik, mintha hello API struktúra és kimeneteinek hello Logic App Designer jobb környezetet biztosítson, de [Swagger-metaadatok](https://swagger.io) . Hello HTTP + Swagger-összekötőt használhatja kiindulópontként. Tooimplement lekérdezési eseményindítót, hajtsa végre hello lekérdezési mintát ismertetett [más API-k, a szolgáltatások és a rendszer hozzon létre egyéni API-k toocall a logic Apps alkalmazásokból](../logic-apps/logic-apps-create-api-app.md#polling-triggers).

További információ [logic app eseményindítók és műveletek](connectors-overview.md).

Íme egy példa a hogyan toouse hello HTTP + Swagger műveletet, mert a logikai alkalmazás munkafolyamata művelet.

1. Jelölje be hello **új lépés** gombra.
2. Válassza ki **művelet hozzáadása**.
3. Hello művelet keresési mezőbe, írja be a **swagger** toolist hello HTTP + Swagger művelet.
   
    ![Válassza ki a HTTP + Swagger művelet](./media/connectors-native-http-swagger/using-action-1.png)
4. Írja be a Swagger-dokumentum hello URL-címe:
   
   * toowork hello Logic App Designer hello URL-címet a HTTPS-végpontnak kell lennie, és a CORS engedélyezve van.
   * Hello Swagger-dokumentum nem felel meg ennek a követelménynek, ha [Azure Storage a CORS engedélyezése mellett](#hosting-swagger-from-storage) toostore hello dokumentum.
5. Kattintson a **következő** tooread és a leképezési hello Swagger-dokumentum.
6. Adja hozzá a hello HTTP hívás a szükséges paramétereket.
   
    ![Teljes HTTP-művelet](./media/connectors-native-http-swagger/using-action-2.png)
7. toosave és a logikai alkalmazás közzétételéhez kattintson **mentése** designer eszköztáron.

### <a name="host-swagger-from-azure-storage"></a>Az Azure Storage állomás Swagger
Érdemes lehet tooreference, amely nem található, vagy, amely nem felel meg a hello biztonsági és eltérő eredetű követelményei hello designer Swagger dokumentumot. tooresolve a probléma hello Swagger-dokumentum tárolására az Azure Storage és a CORS tooreference hello dokumentum engedélyezése.  

Az alábbiakban hello lépéseket toocreate, konfigurálása és Swagger dokumentumok tárolására az Azure Storage:

1. [Az Azure storage-fiók létrehozása az Azure Blob storage szolgáltatással](../storage/common/storage-create-storage-account.md). tooperform ez. lépés:, engedélyek beállítása túl**nyilvános hozzáférés**.

2. A CORS engedélyezése hello blob. 

   tooautomatically konfigurálja ezt a beállítást, használja [a PowerShell parancsfájl](https://github.com/logicappsio/EnableCORSAzureBlob/blob/master/EnableCORSAzureBlob.ps1).

3. Hello Swagger-fájl toohello blob feltöltése. 

   Ebben a lépésben végezhető el hello [Azure-portálon](https://portal.azure.com) vagy egy eszköz, például a [Azure Tártallózó](http://storageexplorer.com/).

4. Egy HTTPS kapcsolat toohello dokumentum az Azure Blob storage hivatkozik. 

   hello hivatkozás ezt a formátumot használja:

   `https://*storageAccountName*.blob.core.windows.net/*container*/*filename*`

## <a name="technical-details"></a>Technikai részletek
Az alábbiakban részletesen hello hello eseményindítók és műveletek, amelyhez a HTTP + Swagger összekötő támogatja.

## <a name="http--swagger-triggers"></a>HTTP + Swagger eseményindítók
Egy eseményindító nem lehet a logikai alkalmazás definiált használt toostart hello munkafolyamat esemény. [További tudnivalók az eseményindítók.](connectors-overview.md) HTTP + Swagger hello összekötő egy eseményindító tartozik.

| Eseményindító | Leírás |
| --- | --- |
| HTTP + Swagger |Egy HTTP-hívást, és térjen vissza a hello válasz tartalom |

## <a name="http--swagger-actions"></a>HTTP + Swagger műveletek
Egy művelet során, amely logikai alkalmazás definiált hello munkafolyamat végzi. [Ismerje meg a műveletet.](connectors-overview.md) HTTP + Swagger hello összekötő tartozik egy lehetséges művelet.

| Műveletek | Leírás |
| --- | --- |
| HTTP + Swagger |Egy HTTP-hívást, és térjen vissza a hello válasz tartalom |

### <a name="action-details"></a>A művelet részletei
HTTP + Swagger hello összekötő egy lehetséges műveletet tartalmaz. Az alábbiakban az egyes hello műveletek, a szükséges és választható beviteli mezőt, és a megfelelő kimeneti részletek használatát társított hello kapcsolatos információkat.

#### <a name="http--swagger"></a>HTTP + Swagger
Ellenőrizze a kimenő HTTP-kérelem a Swagger-metaadatok segítséget.
Egy csillag (*) azt jelenti, hogy a mezőt kötelező kitölteni.

| Megjelenített név | Tulajdonság neve | Leírás |
| --- | --- | --- |
| Módszer * |Módszer |HTTP-művelet toouse. |
| URI * |URI |Hello HTTP-kérelem URI-Azonosítóját. |
| Fejlécek |Fejlécek |A HTTP-fejlécek tooinclude JSON-objektum. |
| Törzs |Törzs |hello HTTP-kérés törzsében. |
| Authentication |Hitelesítés |Hitelesítési toouse kérelem. További információkért lásd: hello [HTTP összekötő](connectors-native-http.md#authentication). |

**Kimeneti részletei**

HTTP-válasz

| Tulajdonság neve | Adattípus | Leírás |
| --- | --- | --- |
| Fejlécek |Objektum |Válaszfejlécek |
| Törzs |Objektum |Válasz objektum |
| Állapotkód |int |HTTP-állapotkód: |

### <a name="http-responses"></a>HTTP-válaszok
Hívások toovarious műveletek meghozásakor bizonyos válaszokat kaphat. Az alábbiakban látható egy táblázat a megfelelő válaszok és leírásokat.

| Név | Leírás |
| --- | --- |
| 200 |OKÉ |
| 202 |Elfogadva |
| 400 |Helytelen kérelem |
| 401 |Nem engedélyezett |
| 403 |Tiltott |
| 404 |Nem található |
| 500 |Belső kiszolgálóhiba. Ismeretlen hiba történt. |

- - -
## <a name="next-steps"></a>Következő lépések

* [Logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md)
* [Más összekötők keresése](apis-list.md)