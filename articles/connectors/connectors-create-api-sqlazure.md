---
title: "aaaAdd hello Azure SQL Database-összekötőt a Logic Apps a |} Microsoft Docs"
description: "REST API-paraméterek az Azure SQL Database-összekötő áttekintése"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: d8a319d0-e4df-40cf-88f0-29a6158c898c
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: a9ca0f446d05dc00f310a908eee8d50e41fcd82b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-azure-sql-database-connector"></a>Ismerkedés az Azure SQL Database összekötő hello
Hello Azure SQL Database-összekötő használatával hozhat létre a munkafolyamatok a szervezet számára, hogy a tábla adatainak kezelése. 

Az SQL Database meg:

* A munkafolyamat egy új ügyfél tooa ügyfelek adatbázist, vagy frissítésétől egészen a rendelések adatbázisban egy rendelés hozhat létre.
* Műveletek tooget soraiban levő adatok használni, új sor beszúrására és még akkor is törli. Például egy rekord létrehozásakor a Dynamics CRM Online (trigger), majd sor beszúrása egy Azure SQL Database (a műveletet). 

Ez a témakör bemutatja, hogyan toouse hello SQL adatbázis-összekötőt a logikai alkalmazás, és is listák hello műveletek.

További információk a Logic Apps toolearn lásd [Mik azok a logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) és [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-tooazure-sql-database"></a>Csatlakozás SQL Database tooAzure
A Logic Apps alkalmazást bármely szolgáltatás hozzáférni, először létre kell hoznia egy *kapcsolat* toohello szolgáltatás. Egy kapcsolat egy logikai alkalmazást és egy másik szolgáltatás közötti kapcsolatot biztosít. Tooconnect tooSQL adatbázis, akkor először hozzon létre például egy SQL-adatbázis *kapcsolat*. toocreate kapcsolatot, akkor adja meg a hello hitelesítő adatokkal kell általában tooaccess hello szolgáltatáshoz való kapcsolódás esetén. Igen az SQL-adatbázis, adja meg az SQL-adatbázis hitelesítő adatai toocreate hello kapcsolat. 

#### <a name="create-hello-connection"></a>Hello kapcsolat létrehozása
> [!INCLUDE [Create hello connection tooSQL Azure](../../includes/connectors-create-api-sqlazure.md)]
> 
> 

## <a name="use-a-trigger"></a>Eseményindítók
Ez az összekötő nem rendelkezik a eseményindítókat. Használjon más eseményindítók toostart hello logikai alkalmazás, például az ismétlődési eseményindítót, egy HTTP Webhook eseményindító, eseményindítók érhető el a többi összekötőt, és több. [Logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md) példaként szolgál.

## <a name="use-an-action"></a>Egy művelettel
Egy művelet által definiált logikai alkalmazás hello munkafolyamat során. [További információ a műveletek](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

1. Válassza ki a hello plusz jel. Több beállítások megtekintéséhez: **művelet hozzáadása**, **feltétel hozzáadása**, vagy egy hello **további** beállítások.
   
    ![](./media/connectors-create-api-sqlazure/add-action.png)
2. Válasszon **művelet hozzáadása**.
3. Hello szövegmezőbe írja be a "sql" tooget összes hello elérhető műveletek listáját.
   
    ![](./media/connectors-create-api-sqlazure/sql-1.png) 
4. Válassza ki a fenti példában **SQL Server - Get sor**. Ha már létezik egy kapcsolat, majd válassza ki hello **táblanév** a hello legördülő listában, és adja meg a hello **Sorazonosító** tooreturn szeretné.
   
    ![](./media/connectors-create-api-sqlazure/sample-table.png)
   
    Ha hello kapcsolati adatokat kéri, adja meg hello részletek toocreate hello kapcsolat. [Hello kapcsolat létrehozása](connectors-create-api-sqlazure.md#create-the-connection) a témakörben ismertetett ezeket a tulajdonságokat. 
   
   > [!NOTE]
   > Ebben a példában azt egy táblából egy sort ad vissza. a sorhoz toosee hello adatok hozzáadása egy másik művelet, amely létrehoz egy hello mezőkkel hello táblából. Adja hozzá például a onedrive vállalati verzió művelet hello Utónév és Vezetéknév mező toocreate hello a felhőalapú társzolgáltatás fiókja egy új fájlt használó. 
   > 
   > 
5. **Mentés** a módosításokat (bal felső sarkában hello eszköztár). A Logic Apps alkalmazást menti, és lehet, hogy automatikusan engedélyezve.

## <a name="connector-specific-details"></a>Összekötő-specifikus részletei

Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/sql/). 

## <a name="next-steps"></a>Következő lépések
[Logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md). Fedezze fel más rendelkezésre álló összekötők Logic Apps: hello a [API-k lista](apis-list.md).

