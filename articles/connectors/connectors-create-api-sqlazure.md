---
title: "Adja hozzá az Azure SQL Database-összekötőt a Logic Apps |} Microsoft Docs"
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
ms.openlocfilehash: a3d5cb909dbfcb00f3fbfa0165bb6cd58eb18688
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-azure-sql-database-connector"></a><span data-ttu-id="66846-103">Ismerkedés az Azure SQL Database-összekötő</span><span class="sxs-lookup"><span data-stu-id="66846-103">Get started with the Azure SQL Database connector</span></span>
<span data-ttu-id="66846-104">Az Azure SQL Database-összekötővel, munkafolyamatokat a szervezet számára, hogy a tábla adatainak kezelése.</span><span class="sxs-lookup"><span data-stu-id="66846-104">Using the Azure SQL Database connector, create workflows for your organization that manage data in your tables.</span></span> 

<span data-ttu-id="66846-105">Az SQL Database meg:</span><span class="sxs-lookup"><span data-stu-id="66846-105">With SQL Database, you:</span></span>

* <span data-ttu-id="66846-106">A munkafolyamat egy új ügyfél ügyfelek adatbázishoz, vagy frissítésétől egészen a rendelések adatbázisban egy rendelés hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="66846-106">Build your workflow by adding a new customer to a customers database, or updating an order in an orders database.</span></span>
* <span data-ttu-id="66846-107">Műveletek segítségével az adatok egy sor lekéréséhez, új sor beszúrására, és akkor is törli.</span><span class="sxs-lookup"><span data-stu-id="66846-107">Use actions to get a row of data, insert a new row, and even delete.</span></span> <span data-ttu-id="66846-108">Például egy rekord létrehozásakor a Dynamics CRM Online (trigger), majd sor beszúrása egy Azure SQL Database (a műveletet).</span><span class="sxs-lookup"><span data-stu-id="66846-108">For example,  when a record is created in Dynamics CRM Online (a trigger), then insert a row in an Azure SQL Database (an action).</span></span> 

<span data-ttu-id="66846-109">Ez a témakör bemutatja, hogyan használható az SQL-adatbázis összekötő logikai alkalmazás, és is felsorolja azokat a műveleteket.</span><span class="sxs-lookup"><span data-stu-id="66846-109">This topic shows you how to use the SQL Database connector in a logic app, and also lists the actions.</span></span>

<span data-ttu-id="66846-110">A Logic Apps kapcsolatos további információkért lásd: [Mik azok a logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) és [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="66846-110">To learn more about Logic Apps, see [What are logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-to-azure-sql-database"></a><span data-ttu-id="66846-111">Csatlakozás az Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="66846-111">Connect to Azure SQL Database</span></span>
<span data-ttu-id="66846-112">A Logic Apps alkalmazást bármely szolgáltatás hozzáférni, először létre kell hoznia egy *kapcsolat* a szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="66846-112">Before your logic app can access any service, you first create a *connection* to the service.</span></span> <span data-ttu-id="66846-113">Egy kapcsolat egy logikai alkalmazást és egy másik szolgáltatás közötti kapcsolatot biztosít.</span><span class="sxs-lookup"><span data-stu-id="66846-113">A connection provides connectivity between a logic app and another service.</span></span> <span data-ttu-id="66846-114">SQL-adatbázishoz való kapcsolódáshoz, először hozzon létre például egy SQL-adatbázis *kapcsolat*.</span><span class="sxs-lookup"><span data-stu-id="66846-114">For example, to connect to SQL Database, you first create a SQL Database *connection*.</span></span> <span data-ttu-id="66846-115">VPN-kapcsolat létrehozásához adja meg a hitelesítő adatokkal kell általában hozzáférhetnek a szolgáltatáshoz való kapcsolódás esetén.</span><span class="sxs-lookup"><span data-stu-id="66846-115">To create a connection, you enter the credentials you normally use to access the service you are connecting to.</span></span> <span data-ttu-id="66846-116">Igen az SQL-adatbázis, adja meg az SQL-adatbázis hitelesítő adatokat a VPN-kapcsolat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="66846-116">So, in SQL Database, enter your SQL Database credentials to create the connection.</span></span> 

#### <a name="create-the-connection"></a><span data-ttu-id="66846-117">A kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="66846-117">Create the connection</span></span>
> [!INCLUDE [Create the connection to SQL Azure](../../includes/connectors-create-api-sqlazure.md)]
> 
> 

## <a name="use-a-trigger"></a><span data-ttu-id="66846-118">Eseményindítók</span><span class="sxs-lookup"><span data-stu-id="66846-118">Use a trigger</span></span>
<span data-ttu-id="66846-119">Ez az összekötő nem rendelkezik a eseményindítókat.</span><span class="sxs-lookup"><span data-stu-id="66846-119">This connector does not have any triggers.</span></span> <span data-ttu-id="66846-120">Más eseményindítók segítségével indítsa el a logikai alkalmazás, például az ismétlődési eseményindítót, egy HTTP Webhook eseményindító, eseményindítók érhető el a többi összekötőt, és több.</span><span class="sxs-lookup"><span data-stu-id="66846-120">Use other triggers to start the logic app, such as a Recurrence trigger, an HTTP Webhook trigger, triggers available with other connectors, and more.</span></span> <span data-ttu-id="66846-121">[Logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md) példaként szolgál.</span><span class="sxs-lookup"><span data-stu-id="66846-121">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md) provides an example.</span></span>

## <a name="use-an-action"></a><span data-ttu-id="66846-122">Egy művelettel</span><span class="sxs-lookup"><span data-stu-id="66846-122">Use an action</span></span>
<span data-ttu-id="66846-123">Egy művelet során a logikai alkalmazás definiált munkafolyamat által végzett.</span><span class="sxs-lookup"><span data-stu-id="66846-123">An action is an operation carried out by the workflow defined in a logic app.</span></span> <span data-ttu-id="66846-124">[További információ a műveletek](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="66846-124">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

1. <span data-ttu-id="66846-125">Kattintson a plusz ikonra.</span><span class="sxs-lookup"><span data-stu-id="66846-125">Select the plus sign.</span></span> <span data-ttu-id="66846-126">Több beállítások megtekintéséhez: **művelet hozzáadása**, **feltétel hozzáadása**, vagy az egyik a **további** beállítások.</span><span class="sxs-lookup"><span data-stu-id="66846-126">You see several choices: **Add an action**, **Add a condition**, or one of the **More** options.</span></span>
   
    ![](./media/connectors-create-api-sqlazure/add-action.png)
2. <span data-ttu-id="66846-127">Válasszon **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="66846-127">Choose **Add an action**.</span></span>
3. <span data-ttu-id="66846-128">A szövegmezőben írja be a rendelkezésre álló műveletek listájának "sql".</span><span class="sxs-lookup"><span data-stu-id="66846-128">In the text box, type “sql” to get a list of all the available actions.</span></span>
   
    ![](./media/connectors-create-api-sqlazure/sql-1.png) 
4. <span data-ttu-id="66846-129">Válassza ki a fenti példában **SQL Server - Get sor**.</span><span class="sxs-lookup"><span data-stu-id="66846-129">In our example, choose **SQL Server - Get row**.</span></span> <span data-ttu-id="66846-130">Ha már létezik egy kapcsolat, majd válassza ki a **táblanév** a legördülő listában, és adja meg a **Sorazonosító** szeretne visszaállítani.</span><span class="sxs-lookup"><span data-stu-id="66846-130">If a connection already exists, then select the **Table name** from the drop-down list, and enter the **Row ID** you want to return.</span></span>
   
    ![](./media/connectors-create-api-sqlazure/sample-table.png)
   
    <span data-ttu-id="66846-131">Ha a kapcsolati adatokat kéri, adja meg a részleteket a VPN-kapcsolat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="66846-131">If you are prompted for the connection information, then enter the details to create the connection.</span></span> <span data-ttu-id="66846-132">[A kapcsolat létrehozása](connectors-create-api-sqlazure.md#create-the-connection) a témakörben ismertetett ezeket a tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="66846-132">[Create the connection](connectors-create-api-sqlazure.md#create-the-connection) in this topic describes these properties.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="66846-133">Ebben a példában azt egy táblából egy sort ad vissza.</span><span class="sxs-lookup"><span data-stu-id="66846-133">In this example, we return a row from a table.</span></span> <span data-ttu-id="66846-134">A sor adatai, vegye fel egy újabb műveletet, amely létrehoz egy fájlt, a mezők a táblából.</span><span class="sxs-lookup"><span data-stu-id="66846-134">To see the data in this row, add another action that creates a file using the fields from the table.</span></span> <span data-ttu-id="66846-135">Adja hozzá például a Keresztnév és Vezetéknév mező használatával hozzon létre egy új fájlt a felhőalapú társzolgáltatás fiókja a onedrive vállalati verzió végrehajtandó.</span><span class="sxs-lookup"><span data-stu-id="66846-135">For example, add a OneDrive action that uses the FirstName and LastName fields to create a new file in the cloud storage account.</span></span> 
   > 
   > 
5. <span data-ttu-id="66846-136">**Mentés** a módosításokat (bal felső sarkában az eszköztár).</span><span class="sxs-lookup"><span data-stu-id="66846-136">**Save** your changes (top left corner of the toolbar).</span></span> <span data-ttu-id="66846-137">A Logic Apps alkalmazást menti, és lehet, hogy automatikusan engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="66846-137">Your logic app is saved and may be automatically enabled.</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="66846-138">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="66846-138">Connector-specific details</span></span>

<span data-ttu-id="66846-139">Bármely eseményindítók és a swagger definiált műveletek megtekintése, és semmilyen határnak a Lásd még: a [connector részleteket](/connectors/sql/).</span><span class="sxs-lookup"><span data-stu-id="66846-139">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/sql/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="66846-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="66846-140">Next steps</span></span>
<span data-ttu-id="66846-141">[Logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="66846-141">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="66846-142">Az egyéb rendelkezésre álló összekötők Logic Apps, megismerkedhet a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="66846-142">Explore the other available connectors in Logic Apps at our [APIs list](apis-list.md).</span></span>

