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
# <a name="get-started-with-hello-azure-sql-database-connector"></a><span data-ttu-id="aaaba-103">Ismerkedés az Azure SQL Database összekötő hello</span><span class="sxs-lookup"><span data-stu-id="aaaba-103">Get started with hello Azure SQL Database connector</span></span>
<span data-ttu-id="aaaba-104">Hello Azure SQL Database-összekötő használatával hozhat létre a munkafolyamatok a szervezet számára, hogy a tábla adatainak kezelése.</span><span class="sxs-lookup"><span data-stu-id="aaaba-104">Using hello Azure SQL Database connector, create workflows for your organization that manage data in your tables.</span></span> 

<span data-ttu-id="aaaba-105">Az SQL Database meg:</span><span class="sxs-lookup"><span data-stu-id="aaaba-105">With SQL Database, you:</span></span>

* <span data-ttu-id="aaaba-106">A munkafolyamat egy új ügyfél tooa ügyfelek adatbázist, vagy frissítésétől egészen a rendelések adatbázisban egy rendelés hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="aaaba-106">Build your workflow by adding a new customer tooa customers database, or updating an order in an orders database.</span></span>
* <span data-ttu-id="aaaba-107">Műveletek tooget soraiban levő adatok használni, új sor beszúrására és még akkor is törli.</span><span class="sxs-lookup"><span data-stu-id="aaaba-107">Use actions tooget a row of data, insert a new row, and even delete.</span></span> <span data-ttu-id="aaaba-108">Például egy rekord létrehozásakor a Dynamics CRM Online (trigger), majd sor beszúrása egy Azure SQL Database (a műveletet).</span><span class="sxs-lookup"><span data-stu-id="aaaba-108">For example,  when a record is created in Dynamics CRM Online (a trigger), then insert a row in an Azure SQL Database (an action).</span></span> 

<span data-ttu-id="aaaba-109">Ez a témakör bemutatja, hogyan toouse hello SQL adatbázis-összekötőt a logikai alkalmazás, és is listák hello műveletek.</span><span class="sxs-lookup"><span data-stu-id="aaaba-109">This topic shows you how toouse hello SQL Database connector in a logic app, and also lists hello actions.</span></span>

<span data-ttu-id="aaaba-110">További információk a Logic Apps toolearn lásd [Mik azok a logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) és [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="aaaba-110">toolearn more about Logic Apps, see [What are logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-tooazure-sql-database"></a><span data-ttu-id="aaaba-111">Csatlakozás SQL Database tooAzure</span><span class="sxs-lookup"><span data-stu-id="aaaba-111">Connect tooAzure SQL Database</span></span>
<span data-ttu-id="aaaba-112">A Logic Apps alkalmazást bármely szolgáltatás hozzáférni, először létre kell hoznia egy *kapcsolat* toohello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="aaaba-112">Before your logic app can access any service, you first create a *connection* toohello service.</span></span> <span data-ttu-id="aaaba-113">Egy kapcsolat egy logikai alkalmazást és egy másik szolgáltatás közötti kapcsolatot biztosít.</span><span class="sxs-lookup"><span data-stu-id="aaaba-113">A connection provides connectivity between a logic app and another service.</span></span> <span data-ttu-id="aaaba-114">Tooconnect tooSQL adatbázis, akkor először hozzon létre például egy SQL-adatbázis *kapcsolat*.</span><span class="sxs-lookup"><span data-stu-id="aaaba-114">For example, tooconnect tooSQL Database, you first create a SQL Database *connection*.</span></span> <span data-ttu-id="aaaba-115">toocreate kapcsolatot, akkor adja meg a hello hitelesítő adatokkal kell általában tooaccess hello szolgáltatáshoz való kapcsolódás esetén.</span><span class="sxs-lookup"><span data-stu-id="aaaba-115">toocreate a connection, you enter hello credentials you normally use tooaccess hello service you are connecting to.</span></span> <span data-ttu-id="aaaba-116">Igen az SQL-adatbázis, adja meg az SQL-adatbázis hitelesítő adatai toocreate hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="aaaba-116">So, in SQL Database, enter your SQL Database credentials toocreate hello connection.</span></span> 

#### <a name="create-hello-connection"></a><span data-ttu-id="aaaba-117">Hello kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="aaaba-117">Create hello connection</span></span>
> [!INCLUDE [Create hello connection tooSQL Azure](../../includes/connectors-create-api-sqlazure.md)]
> 
> 

## <a name="use-a-trigger"></a><span data-ttu-id="aaaba-118">Eseményindítók</span><span class="sxs-lookup"><span data-stu-id="aaaba-118">Use a trigger</span></span>
<span data-ttu-id="aaaba-119">Ez az összekötő nem rendelkezik a eseményindítókat.</span><span class="sxs-lookup"><span data-stu-id="aaaba-119">This connector does not have any triggers.</span></span> <span data-ttu-id="aaaba-120">Használjon más eseményindítók toostart hello logikai alkalmazás, például az ismétlődési eseményindítót, egy HTTP Webhook eseményindító, eseményindítók érhető el a többi összekötőt, és több.</span><span class="sxs-lookup"><span data-stu-id="aaaba-120">Use other triggers toostart hello logic app, such as a Recurrence trigger, an HTTP Webhook trigger, triggers available with other connectors, and more.</span></span> <span data-ttu-id="aaaba-121">[Logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md) példaként szolgál.</span><span class="sxs-lookup"><span data-stu-id="aaaba-121">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md) provides an example.</span></span>

## <a name="use-an-action"></a><span data-ttu-id="aaaba-122">Egy művelettel</span><span class="sxs-lookup"><span data-stu-id="aaaba-122">Use an action</span></span>
<span data-ttu-id="aaaba-123">Egy művelet által definiált logikai alkalmazás hello munkafolyamat során.</span><span class="sxs-lookup"><span data-stu-id="aaaba-123">An action is an operation carried out by hello workflow defined in a logic app.</span></span> <span data-ttu-id="aaaba-124">[További információ a műveletek](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="aaaba-124">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

1. <span data-ttu-id="aaaba-125">Válassza ki a hello plusz jel.</span><span class="sxs-lookup"><span data-stu-id="aaaba-125">Select hello plus sign.</span></span> <span data-ttu-id="aaaba-126">Több beállítások megtekintéséhez: **művelet hozzáadása**, **feltétel hozzáadása**, vagy egy hello **további** beállítások.</span><span class="sxs-lookup"><span data-stu-id="aaaba-126">You see several choices: **Add an action**, **Add a condition**, or one of hello **More** options.</span></span>
   
    ![](./media/connectors-create-api-sqlazure/add-action.png)
2. <span data-ttu-id="aaaba-127">Válasszon **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="aaaba-127">Choose **Add an action**.</span></span>
3. <span data-ttu-id="aaaba-128">Hello szövegmezőbe írja be a "sql" tooget összes hello elérhető műveletek listáját.</span><span class="sxs-lookup"><span data-stu-id="aaaba-128">In hello text box, type “sql” tooget a list of all hello available actions.</span></span>
   
    ![](./media/connectors-create-api-sqlazure/sql-1.png) 
4. <span data-ttu-id="aaaba-129">Válassza ki a fenti példában **SQL Server - Get sor**.</span><span class="sxs-lookup"><span data-stu-id="aaaba-129">In our example, choose **SQL Server - Get row**.</span></span> <span data-ttu-id="aaaba-130">Ha már létezik egy kapcsolat, majd válassza ki hello **táblanév** a hello legördülő listában, és adja meg a hello **Sorazonosító** tooreturn szeretné.</span><span class="sxs-lookup"><span data-stu-id="aaaba-130">If a connection already exists, then select hello **Table name** from hello drop-down list, and enter hello **Row ID** you want tooreturn.</span></span>
   
    ![](./media/connectors-create-api-sqlazure/sample-table.png)
   
    <span data-ttu-id="aaaba-131">Ha hello kapcsolati adatokat kéri, adja meg hello részletek toocreate hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="aaaba-131">If you are prompted for hello connection information, then enter hello details toocreate hello connection.</span></span> <span data-ttu-id="aaaba-132">[Hello kapcsolat létrehozása](connectors-create-api-sqlazure.md#create-the-connection) a témakörben ismertetett ezeket a tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="aaaba-132">[Create hello connection](connectors-create-api-sqlazure.md#create-the-connection) in this topic describes these properties.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="aaaba-133">Ebben a példában azt egy táblából egy sort ad vissza.</span><span class="sxs-lookup"><span data-stu-id="aaaba-133">In this example, we return a row from a table.</span></span> <span data-ttu-id="aaaba-134">a sorhoz toosee hello adatok hozzáadása egy másik művelet, amely létrehoz egy hello mezőkkel hello táblából.</span><span class="sxs-lookup"><span data-stu-id="aaaba-134">toosee hello data in this row, add another action that creates a file using hello fields from hello table.</span></span> <span data-ttu-id="aaaba-135">Adja hozzá például a onedrive vállalati verzió művelet hello Utónév és Vezetéknév mező toocreate hello a felhőalapú társzolgáltatás fiókja egy új fájlt használó.</span><span class="sxs-lookup"><span data-stu-id="aaaba-135">For example, add a OneDrive action that uses hello FirstName and LastName fields toocreate a new file in hello cloud storage account.</span></span> 
   > 
   > 
5. <span data-ttu-id="aaaba-136">**Mentés** a módosításokat (bal felső sarkában hello eszköztár).</span><span class="sxs-lookup"><span data-stu-id="aaaba-136">**Save** your changes (top left corner of hello toolbar).</span></span> <span data-ttu-id="aaaba-137">A Logic Apps alkalmazást menti, és lehet, hogy automatikusan engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="aaaba-137">Your logic app is saved and may be automatically enabled.</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="aaaba-138">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="aaaba-138">Connector-specific details</span></span>

<span data-ttu-id="aaaba-139">Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/sql/).</span><span class="sxs-lookup"><span data-stu-id="aaaba-139">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/sql/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="aaaba-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="aaaba-140">Next steps</span></span>
<span data-ttu-id="aaaba-141">[Logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="aaaba-141">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="aaaba-142">Fedezze fel más rendelkezésre álló összekötők Logic Apps: hello a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="aaaba-142">Explore hello other available connectors in Logic Apps at our [APIs list](apis-list.md).</span></span>

