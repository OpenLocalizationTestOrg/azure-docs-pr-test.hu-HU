---
title: "Adja hozzá az Oracle-adatbázishoz összekötő az Azure Logic Apps |} Microsoft Docs"
description: "A logikai alkalmazás az Oracle-adatbázishoz összekötő használatára"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/29/2017
ms.author: mandia; ladocs
ms.openlocfilehash: cc64441617eb5e7d5e70c1cf5c491a672428bc51
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="get-started-with-the-oracle-database-connector"></a><span data-ttu-id="0b3c7-103">Az Oracle-adatbázishoz összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="0b3c7-103">Get started with the Oracle Database connector</span></span>

<span data-ttu-id="0b3c7-104">Az Oracle adatbázis-összekötő segítségével hoz létre a meglévő adatbázis adatokat használó szervezeti munkafolyamatok.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-104">Using the Oracle Database connector, you create organizational workflows that use data in your existing database.</span></span> <span data-ttu-id="0b3c7-105">Az összekötő telepítve Oracle-adatbázishoz csatlakozhat helyszíni Oracle-adatbázishoz, vagy egy Azure virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-105">This connector can connect to an on-premises Oracle Database, or an Azure virtual machine with Oracle Database installed.</span></span> <span data-ttu-id="0b3c7-106">Ez az összekötő segítségével:</span><span class="sxs-lookup"><span data-stu-id="0b3c7-106">With this connector, you can:</span></span>

* <span data-ttu-id="0b3c7-107">A munkafolyamat egy új ügyfél ügyfelek adatbázishoz, vagy frissítésétől egészen a rendelések adatbázisban egy rendelés hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-107">Build your workflow by adding a new customer to a customers database, or updating an order in an orders database.</span></span>
* <span data-ttu-id="0b3c7-108">Műveletek segítségével az adatok egy sor lekéréséhez, új sor beszúrására, és akkor is törli.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-108">Use actions to get a row of data, insert a new row, and even delete.</span></span> <span data-ttu-id="0b3c7-109">Például egy rekord létrehozásakor a Dynamics CRM Online (trigger), majd sor beszúrása az Oracle-adatbázisban (művelet).</span><span class="sxs-lookup"><span data-stu-id="0b3c7-109">For example, when a record is created in Dynamics CRM Online (a trigger), then insert a row in an Oracle Database (an action).</span></span> 

<span data-ttu-id="0b3c7-110">Ez a témakör bemutatja, hogyan az Oracle-adatbázishoz összekötő használatára a logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-110">This topic shows you how to use the Oracle Database connector in a logic app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0b3c7-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0b3c7-111">Prerequisites</span></span>

* <span data-ttu-id="0b3c7-112">Oracle támogatott verziók:</span><span class="sxs-lookup"><span data-stu-id="0b3c7-112">Supported Oracle versions:</span></span> 
    * <span data-ttu-id="0b3c7-113">Oracle 9-es és újabb verziók</span><span class="sxs-lookup"><span data-stu-id="0b3c7-113">Oracle 9 and later</span></span>
    * <span data-ttu-id="0b3c7-114">Oracle ügyfélszoftvert 8.1.7-es vagy újabb</span><span class="sxs-lookup"><span data-stu-id="0b3c7-114">Oracle client software 8.1.7 and later</span></span>

* <span data-ttu-id="0b3c7-115">Telepítse a helyszíni adatátjáró.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-115">Install the on-premises data gateway.</span></span> <span data-ttu-id="0b3c7-116">[A helyszíni adatokhoz csatlakozva a logic Apps alkalmazásokból](../logic-apps/logic-apps-gateway-connection.md) lépéseit sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-116">[Connect to on-premises data from logic apps](../logic-apps/logic-apps-gateway-connection.md) lists the steps.</span></span> <span data-ttu-id="0b3c7-117">Az átjáró egy helyszíni Oracle-adatbázishoz való kapcsolódáshoz szükséges, vagy egy Azure virtuális Gépen az Oracle-adatbázis telepítve.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-117">The gateway is required to connect to an on-premises Oracle Database, or an Azure VM with Oracle DB installed.</span></span> 

    > [!NOTE]
    > <span data-ttu-id="0b3c7-118">Az a helyszíni átjáró egy hidat és biztonságos helyszíni (adatokat, amely nem a felhőben) és a logic Apps alkalmazások közötti adatátvitel tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-118">The on-premises data gateway acts as a bridge, and provides a secure data transfer between on-premises data (data that is not in the cloud) and your logic apps.</span></span> <span data-ttu-id="0b3c7-119">Több szolgáltatásra, és több adatforrást ugyanahhoz az átjáróhoz használható.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-119">The same gateway can be used with multiple services, and multiple data sources.</span></span> <span data-ttu-id="0b3c7-120">Igen csak szükség lehet az átjáró egyszer telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-120">So, you may only need to install the gateway once.</span></span>

* <span data-ttu-id="0b3c7-121">Az Oracle-ügyfél telepítéséhez a számítógépen, amelyre telepítette a helyszíni adatátjáró.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-121">Install the Oracle Client on the machine where you installed the on-premises data gateway.</span></span> <span data-ttu-id="0b3c7-122">Győződjön meg arról, a 64 bites Oracle-adatszolgáltatóban telepítéséhez a .NET-hez, az Oracle:</span><span class="sxs-lookup"><span data-stu-id="0b3c7-122">Be sure to install the 64-bit Oracle Data Provider for .NET from Oracle:</span></span>  

  [<span data-ttu-id="0b3c7-123">64 bites ODAC Windows x64 12c kiadás 4 (12.1.0.2.4)</span><span class="sxs-lookup"><span data-stu-id="0b3c7-123">64-bit ODAC 12c Release 4 (12.1.0.2.4) for Windows x64</span></span>](http://www.oracle.com/technetwork/database/windows/downloads/index-090165.html)

    > [!TIP]
    > <span data-ttu-id="0b3c7-124">Az Oracle-ügyfél nincs telepítve, ha hiba lép fel, amikor megpróbálja létrehozni, vagy a kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-124">If the Oracle client is not installed, an error occurs when you try to create or use the connection.</span></span> <span data-ttu-id="0b3c7-125">Tekintse meg a közös hibákat ebben a témakörben.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-125">See the common errors in this topic.</span></span>


## <a name="add-the-connector"></a><span data-ttu-id="0b3c7-126">Az összekötő hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0b3c7-126">Add the connector</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0b3c7-127">Ez az összekötő nem rendelkezik a eseményindítókat.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-127">This connector does not have any triggers.</span></span> <span data-ttu-id="0b3c7-128">Csak a műveleteket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-128">It has only actions.</span></span> <span data-ttu-id="0b3c7-129">Ezért a logikai alkalmazás létrehozásakor hozzá indítsa el a logikai alkalmazás, például egy másik eseményindító **ütemezés - ismétlődési**, vagy **kérelem / válasz - válasz**.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-129">So when you create your logic app, add another trigger to start your logic app, such as **Schedule - Recurrence**, or **Request / Response - Response**.</span></span> 

1. <span data-ttu-id="0b3c7-130">Az a [Azure-portálon](https://portal.azure.com), egy üres logikai alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-130">In the [Azure portal](https://portal.azure.com), create a blank logic app.</span></span>

2. <span data-ttu-id="0b3c7-131">A Logic Apps alkalmazást elején, jelölje ki a **kérelem / válasz - kérelem** eseményindító:</span><span class="sxs-lookup"><span data-stu-id="0b3c7-131">At the start of your logic app, select the **Request / Response - Request** trigger:</span></span> 

    ![](./media/connectors-create-api-oracledatabase/request-trigger.png)

3. <span data-ttu-id="0b3c7-132">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-132">Select **Save**.</span></span> <span data-ttu-id="0b3c7-133">Mentésekor, a rendszer automatikusan előállítja a kérelem URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-133">When you save, a request URL is automatically generated.</span></span> 

4. <span data-ttu-id="0b3c7-134">Válassza ki **új lépés**, és válassza ki **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-134">Select **New step**, and select **Add an action**.</span></span> <span data-ttu-id="0b3c7-135">Írja be a `oracle` a rendelkezésre álló műveletek megjelenítéséhez:</span><span class="sxs-lookup"><span data-stu-id="0b3c7-135">Type in `oracle` to see the available actions:</span></span> 

    ![](./media/connectors-create-api-oracledatabase/oracledb-actions.png)

    > [!TIP]
    > <span data-ttu-id="0b3c7-136">Ez egyben a leggyorsabban úgy tudja megtekinteni, az eseményindítók és műveletek a csatlakozókat érhető el.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-136">This is also the quickest way to see the triggers and actions available for any connector.</span></span> <span data-ttu-id="0b3c7-137">Írja be az összekötő neve részeként például `oracle`.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-137">Type in part of the connector name, such as `oracle`.</span></span> <span data-ttu-id="0b3c7-138">A Tervező bármely eseményindítók és műveletek sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-138">The designer lists any triggers and any actions.</span></span> 

5. <span data-ttu-id="0b3c7-139">Válasszon egyet a a műveletek, például a **Oracle adatbázis - Get sor**.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-139">Select one of the actions, such as **Oracle Database - Get row**.</span></span> <span data-ttu-id="0b3c7-140">Válassza ki **keresztül, a helyszíni adatátjáró**.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-140">Select **Connect via on-premises data gateway**.</span></span> <span data-ttu-id="0b3c7-141">Adja meg az Oracle-kiszolgáló nevét, a hitelesítési módszert, a felhasználónév, a jelszó, és válassza ki az átjáró:</span><span class="sxs-lookup"><span data-stu-id="0b3c7-141">Enter the Oracle server name, authentication method, username, password, and select the gateway:</span></span>

    ![](./media/connectors-create-api-oracledatabase/create-oracle-connection.png)

6. <span data-ttu-id="0b3c7-142">Miután csatlakozott, válasszon ki egy táblát a listából, és adja meg a Sorazonosító a táblában.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-142">Once connected, select a table from the list, and enter the row ID to your table.</span></span> <span data-ttu-id="0b3c7-143">Meg kell tudni, hogy a tábla azonosítója.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-143">You need to know the identifier to the table.</span></span> <span data-ttu-id="0b3c7-144">Ha nem ismeri, a Oracle adatbázis rendszergazdájától, és eredményének beolvasása `select * from yourTableName`.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-144">If you don't know, contact your Oracle DB administrator, and get the output from `select * from yourTableName`.</span></span> <span data-ttu-id="0b3c7-145">Ez lehetővé teszi a kell folytatnia azonosításra alkalmas adatokat.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-145">This gives you the identifiable information you need to proceed.</span></span>

    <span data-ttu-id="0b3c7-146">A következő példában feladatadatok egy az emberi erőforrások adatbázisból ad vissza:</span><span class="sxs-lookup"><span data-stu-id="0b3c7-146">In the following example, job data is being returned from a Human Resources database:</span></span> 

    ![](./media/connectors-create-api-oracledatabase/table-rowid.png)

7. <span data-ttu-id="0b3c7-147">A következő lépésben a munkafolyamat létrehozásához használhatja a többi összekötőt bármelyikét.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-147">In this next step, you can use any of the other connectors to build your workflow.</span></span> <span data-ttu-id="0b3c7-148">Ha azt szeretné, az Oracle beolvasásakor adatok tesztelésére, majd küldhet magának egy e-mailek küldése e-mailek összekötők, például Office 365 vagy Gmailes egyikével Oracle-adatokkal.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-148">If you want to test getting data from Oracle, then send yourself an email with the Oracle data using one of the send email connectors, such Office 365 or Gmail.</span></span> <span data-ttu-id="0b3c7-149">A dinamikus jogkivonatok az Oracle-táblából az lehetővé teszi a `Subject` és `Body` az e-mail:</span><span class="sxs-lookup"><span data-stu-id="0b3c7-149">Use the dynamic tokens from the Oracle table to build the `Subject` and `Body` of your email:</span></span>

    ![](./media/connectors-create-api-oracledatabase/oracle-send-email.png)

8. <span data-ttu-id="0b3c7-150">**Mentés** a Logic Apps alkalmazást, és válassza **futtatása**.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-150">**Save** your logic app, and then select **Run**.</span></span> <span data-ttu-id="0b3c7-151">A Tervező bezárása, és tekintse meg az állapot fut előzményeit.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-151">Close the designer, and look at the runs history for the status.</span></span> <span data-ttu-id="0b3c7-152">Ha sikertelen, jelölje ki a hibás üzenetek.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-152">If it fails, select the failed message row.</span></span> <span data-ttu-id="0b3c7-153">A Tervező megnyílik, és mutatja meg, amely lépés sikertelen volt, és a hiba információk is láthatók.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-153">The designer opens, and shows you which step failed, and also shows the error information.</span></span> <span data-ttu-id="0b3c7-154">Ha ez sikeres, akkor a hozzáadott információkat e-mailt kell kapnia.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-154">If it succeeds, then you should receive an email with the information you added.</span></span>


### <a name="workflow-ideas"></a><span data-ttu-id="0b3c7-155">Munkafolyamat ötletek</span><span class="sxs-lookup"><span data-stu-id="0b3c7-155">Workflow ideas</span></span>

* <span data-ttu-id="0b3c7-156">Szeretné a #oracle hashtaggel történő figyelése és a Twitter-üzeneteket be egy adatbázist, így kérdezhetők le, és más alkalmazásokban használt.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-156">You want to monitor the #oracle hashtag, and put the tweets in a database so they can be queried, and used within other applications.</span></span> <span data-ttu-id="0b3c7-157">A logikai alkalmazás, adja hozzá a `Twitter - When a new tweet is posted` indítható el, és írja be a **#oracle** hashtaggel történő.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-157">In a logic app, add the `Twitter - When a new tweet is posted` trigger, and enter the **#oracle** hashtag.</span></span> <span data-ttu-id="0b3c7-158">Adja hozzá a `Oracle Database - Insert row` művelet, és válassza ki a táblázatban:</span><span class="sxs-lookup"><span data-stu-id="0b3c7-158">Then, add the `Oracle Database - Insert row` action, and select your table:</span></span>

    ![](./media/connectors-create-api-oracledatabase/twitter-oracledb.png)

* <span data-ttu-id="0b3c7-159">Üzenetek küldése a Service Bus-üzenetsorba.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-159">Messages are sent to a Service Bus queue.</span></span> <span data-ttu-id="0b3c7-160">Szeretné ezeket az üzeneteket, és azokat egy adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-160">You want to get these messages, and put them in a database.</span></span> <span data-ttu-id="0b3c7-161">A logikai alkalmazás, adja hozzá a `Service Bus - when a message is received in a queue` aktiválhatja, és jelölje ki a várólistát.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-161">In a logic app, add the `Service Bus - when a message is received in a queue` trigger, and select the queue.</span></span> <span data-ttu-id="0b3c7-162">Adja hozzá a `Oracle Database - Insert row` művelet, és válassza ki a táblázatban:</span><span class="sxs-lookup"><span data-stu-id="0b3c7-162">Then, add the `Oracle Database - Insert row` action, and select your table:</span></span>

    ![](./media/connectors-create-api-oracledatabase/sbqueue-oracledb.png)

## <a name="common-errors"></a><span data-ttu-id="0b3c7-163">Gyakori hibák</span><span class="sxs-lookup"><span data-stu-id="0b3c7-163">Common errors</span></span>

#### <a name="error-cannot-reach-the-gateway"></a><span data-ttu-id="0b3c7-164">**Hiba**: az átjáró nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-164">**Error**: Cannot reach the Gateway</span></span>

<span data-ttu-id="0b3c7-165">**OK**: a helyszíni az adatátjáró nem tud csatlakozni a felhőben.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-165">**Cause**: The on-premises data gateway is not able to connect to the cloud.</span></span> 

<span data-ttu-id="0b3c7-166">**Megoldás**: Győződjön meg arról, hogy az átjáró fut a helyi számítógépen, amelyen telepítették őket, és, hogy kapcsolódni tud az internethez.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-166">**Mitigation**: Make sure your gateway is running on the on-premises machine where you installed it, and that it can connect to the internet.</span></span>  <span data-ttu-id="0b3c7-167">Azt javasoljuk, hogy nem telepíti az átjáró a számítógépre, amely ki van kapcsolva vagy alvó.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-167">We recommend not installing the gateway on a computer that may be turned off or sleep.</span></span> <span data-ttu-id="0b3c7-168">A helyszíni átjáró szolgáltatás (PBIEgwService) is újraindíthatja.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-168">You can also restart the on-premises data gateway service (PBIEgwService).</span></span>

#### <a name="error-the-provider-being-used-is-deprecated-systemdataoracleclient-requires-oracle-client-software-version-817-or-greater-please-visit-httpsgomicrosoftcomfwlinkplinkid272376httpsgomicrosoftcomfwlinkplinkid272376-to-install-the-official-provider"></a><span data-ttu-id="0b3c7-169">**Hiba**: A használt szolgáltató elavult: "System.Data.OracleClient szükséges verziójú Oracle ügyfélszoftvert 8.1.7-es vagy újabb.".</span><span class="sxs-lookup"><span data-stu-id="0b3c7-169">**Error**: The provider being used is deprecated: 'System.Data.OracleClient requires Oracle client software version 8.1.7 or greater.'.</span></span> <span data-ttu-id="0b3c7-170">Látogasson el a [https://go.microsoft.com/fwlink/p/?LinkID=272376](https://go.microsoft.com/fwlink/p/?LinkID=272376) a hivatalos szolgáltató telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-170">Please visit [https://go.microsoft.com/fwlink/p/?LinkID=272376](https://go.microsoft.com/fwlink/p/?LinkID=272376) to install the official provider.</span></span>

<span data-ttu-id="0b3c7-171">**OK**: az Oracle ügyfél SDK a helyszíni adatok átjárót futtató gépen nincs telepítve.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-171">**Cause**: The Oracle client SDK is not installed on the machine where the on-premises data gateway is running.</span></span>  

<span data-ttu-id="0b3c7-172">**Megoldási**: Töltse le és telepítse az Oracle ügyfél SDK az helyszíni átjáró ugyanazon a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-172">**Resolution**: Download and install the Oracle client SDK on the same computer as the on-premises data gateway.</span></span>

#### <a name="error-table-tablename-does-not-define-any-key-columns"></a><span data-ttu-id="0b3c7-173">**Hiba**: "[Tablename]" tábla nem definiál bármely Kulcsoszlopok</span><span class="sxs-lookup"><span data-stu-id="0b3c7-173">**Error**: Table '[Tablename]' does not define any key columns</span></span>

<span data-ttu-id="0b3c7-174">**OK**: A táblának nincs elsődleges kulcs.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-174">**Cause**: The table does not have any primary key.</span></span>  

<span data-ttu-id="0b3c7-175">**Megoldási**: az Oracle-adatbázishoz connector megköveteli, hogy a tábla elsődleges kulcsként megadott oszlop használható.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-175">**Resolution**: The Oracle Database connector requires that a table with a primary key column be used.</span></span>

#### <a name="currently-not-supported"></a><span data-ttu-id="0b3c7-176">Jelenleg nem támogatott</span><span class="sxs-lookup"><span data-stu-id="0b3c7-176">Currently not supported</span></span>

* <span data-ttu-id="0b3c7-177">Nézetek és tárolt eljárások</span><span class="sxs-lookup"><span data-stu-id="0b3c7-177">Views and stored procedures</span></span> 
* <span data-ttu-id="0b3c7-178">A tábla az összetett kulcsok</span><span class="sxs-lookup"><span data-stu-id="0b3c7-178">Any table with composite keys</span></span>
* <span data-ttu-id="0b3c7-179">Táblák egymásba ágyazott objektumtípusok</span><span class="sxs-lookup"><span data-stu-id="0b3c7-179">Nested object types in tables</span></span>
 
## <a name="connector-specific-details"></a><span data-ttu-id="0b3c7-180">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="0b3c7-180">Connector-specific details</span></span>

<span data-ttu-id="0b3c7-181">Bármely eseményindítók és a swagger definiált műveletek megtekintése, és semmilyen határnak a Lásd még: a [connector részleteket](/connectors/oracle/).</span><span class="sxs-lookup"><span data-stu-id="0b3c7-181">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/oracle/).</span></span> 

## <a name="get-some-help"></a><span data-ttu-id="0b3c7-182">Segítséget</span><span class="sxs-lookup"><span data-stu-id="0b3c7-182">Get some help</span></span>

<span data-ttu-id="0b3c7-183">A [Azure Logic Apps-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) mutassa kérdései vannak, kérdést, és tekintse meg a többi Logic Apps felhasználók tevékenységeit.</span><span class="sxs-lookup"><span data-stu-id="0b3c7-183">The [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) is a great place to ask questions, answer questions, and see what other Logic Apps users are doing.</span></span> 

<span data-ttu-id="0b3c7-184">Növelheti Logic Apps alkalmazások és összekötők szavazás, és a következő ötletek elküldésével [http://aka.ms/logicapps-wish](http://aka.ms/logicapps-wish).</span><span class="sxs-lookup"><span data-stu-id="0b3c7-184">You can help improve Logic Apps and connectors by voting and submitting your ideas at [http://aka.ms/logicapps-wish](http://aka.ms/logicapps-wish).</span></span> 


## <a name="next-steps"></a><span data-ttu-id="0b3c7-185">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0b3c7-185">Next steps</span></span>
<span data-ttu-id="0b3c7-186">[Logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md), és vizsgálja meg a rendelkezésre álló összekötők Logic Apps, az [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="0b3c7-186">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md), and explore the available connectors in Logic Apps at our [APIs list](apis-list.md).</span></span>
