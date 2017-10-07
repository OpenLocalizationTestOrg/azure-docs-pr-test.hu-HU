---
title: "az Azure Logic Apps aaaAdd hello Oracle adatbázis-összekötőjét |} Microsoft Docs"
description: "A logikai alkalmazás hello Oracle-adatbázishoz összekötő használatára"
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
ms.openlocfilehash: 8a802a6c4782e210ff71848614152cb46ba5d651
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-oracle-database-connector"></a><span data-ttu-id="79523-103">Hello Oracle-adatbázishoz összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="79523-103">Get started with hello Oracle Database connector</span></span>

<span data-ttu-id="79523-104">Hello Oracle-adatbázishoz connector segítségével hoz létre a meglévő adatbázis adatokat használó szervezeti munkafolyamatok.</span><span class="sxs-lookup"><span data-stu-id="79523-104">Using hello Oracle Database connector, you create organizational workflows that use data in your existing database.</span></span> <span data-ttu-id="79523-105">Ez az összekötő a helyszíni Oracle-adatbázishoz, vagy Oracle Database rendszert futtató Azure virtuális gépként telepített tooan is elérheti.</span><span class="sxs-lookup"><span data-stu-id="79523-105">This connector can connect tooan on-premises Oracle Database, or an Azure virtual machine with Oracle Database installed.</span></span> <span data-ttu-id="79523-106">Ez az összekötő segítségével:</span><span class="sxs-lookup"><span data-stu-id="79523-106">With this connector, you can:</span></span>

* <span data-ttu-id="79523-107">A munkafolyamat egy új ügyfél tooa ügyfelek adatbázist, vagy frissítésétől egészen a rendelések adatbázisban egy rendelés hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="79523-107">Build your workflow by adding a new customer tooa customers database, or updating an order in an orders database.</span></span>
* <span data-ttu-id="79523-108">Műveletek tooget soraiban levő adatok használni, új sor beszúrására és még akkor is törli.</span><span class="sxs-lookup"><span data-stu-id="79523-108">Use actions tooget a row of data, insert a new row, and even delete.</span></span> <span data-ttu-id="79523-109">Például egy rekord létrehozásakor a Dynamics CRM Online (trigger), majd sor beszúrása az Oracle-adatbázisban (művelet).</span><span class="sxs-lookup"><span data-stu-id="79523-109">For example, when a record is created in Dynamics CRM Online (a trigger), then insert a row in an Oracle Database (an action).</span></span> 

<span data-ttu-id="79523-110">Ez a témakör bemutatja, hogyan toouse hello Oracle adatbázis-összekötőt a logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="79523-110">This topic shows you how toouse hello Oracle Database connector in a logic app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="79523-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="79523-111">Prerequisites</span></span>

* <span data-ttu-id="79523-112">Oracle támogatott verziók:</span><span class="sxs-lookup"><span data-stu-id="79523-112">Supported Oracle versions:</span></span> 
    * <span data-ttu-id="79523-113">Oracle 9-es és újabb verziók</span><span class="sxs-lookup"><span data-stu-id="79523-113">Oracle 9 and later</span></span>
    * <span data-ttu-id="79523-114">Oracle ügyfélszoftvert 8.1.7-es vagy újabb</span><span class="sxs-lookup"><span data-stu-id="79523-114">Oracle client software 8.1.7 and later</span></span>

* <span data-ttu-id="79523-115">Hello helyszíni adatátjáró telepítése.</span><span class="sxs-lookup"><span data-stu-id="79523-115">Install hello on-premises data gateway.</span></span> <span data-ttu-id="79523-116">[A logic apps tooon helyszíni adatokhoz kapcsolódó](../logic-apps/logic-apps-gateway-connection.md) listák hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="79523-116">[Connect tooon-premises data from logic apps](../logic-apps/logic-apps-gateway-connection.md) lists hello steps.</span></span> <span data-ttu-id="79523-117">hello átjáró szükséges tooconnect tooan helyszíni Oracle-adatbázishoz, vagy egy Azure virtuális Gépen az Oracle-adatbázis telepítve.</span><span class="sxs-lookup"><span data-stu-id="79523-117">hello gateway is required tooconnect tooan on-premises Oracle Database, or an Azure VM with Oracle DB installed.</span></span> 

    > [!NOTE]
    > <span data-ttu-id="79523-118">hello helyszíni adatátjáró egy hidat és biztonságos helyszíni (adatokat, amely nincs a hello) és a logic Apps alkalmazások közötti adatátvitel tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="79523-118">hello on-premises data gateway acts as a bridge, and provides a secure data transfer between on-premises data (data that is not in hello cloud) and your logic apps.</span></span> <span data-ttu-id="79523-119">hello ugyanahhoz az átjáróhoz használható több szolgáltatásra, és több adatforrást.</span><span class="sxs-lookup"><span data-stu-id="79523-119">hello same gateway can be used with multiple services, and multiple data sources.</span></span> <span data-ttu-id="79523-120">Igen előfordulhat, hogy csak akkor kell tooinstall hello átjáró egyszer.</span><span class="sxs-lookup"><span data-stu-id="79523-120">So, you may only need tooinstall hello gateway once.</span></span>

* <span data-ttu-id="79523-121">Telepítse a hello Oracle ügyfél telepítési hello helyszíni adatátjáró hello számítógépen.</span><span class="sxs-lookup"><span data-stu-id="79523-121">Install hello Oracle Client on hello machine where you installed hello on-premises data gateway.</span></span> <span data-ttu-id="79523-122">Győződjön meg arról, hogy az Oracle .NET tooinstall hello 64 bites Oracle-adatszolgáltatóban:</span><span class="sxs-lookup"><span data-stu-id="79523-122">Be sure tooinstall hello 64-bit Oracle Data Provider for .NET from Oracle:</span></span>  

  [<span data-ttu-id="79523-123">64 bites ODAC Windows x64 12c kiadás 4 (12.1.0.2.4)</span><span class="sxs-lookup"><span data-stu-id="79523-123">64-bit ODAC 12c Release 4 (12.1.0.2.4) for Windows x64</span></span>](http://www.oracle.com/technetwork/database/windows/downloads/index-090165.html)

    > [!TIP]
    > <span data-ttu-id="79523-124">Hello Oracle ügyfél nincs telepítve, ha hiba lép fel, próbálkozzon toocreate vagy hello kapcsolat használata.</span><span class="sxs-lookup"><span data-stu-id="79523-124">If hello Oracle client is not installed, an error occurs when you try toocreate or use hello connection.</span></span> <span data-ttu-id="79523-125">Gyakori hibák hello ebben a témakörben talál.</span><span class="sxs-lookup"><span data-stu-id="79523-125">See hello common errors in this topic.</span></span>


## <a name="add-hello-connector"></a><span data-ttu-id="79523-126">Hello összekötő hozzáadása</span><span class="sxs-lookup"><span data-stu-id="79523-126">Add hello connector</span></span>

> [!IMPORTANT]
> <span data-ttu-id="79523-127">Ez az összekötő nem rendelkezik a eseményindítókat.</span><span class="sxs-lookup"><span data-stu-id="79523-127">This connector does not have any triggers.</span></span> <span data-ttu-id="79523-128">Csak a műveleteket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="79523-128">It has only actions.</span></span> <span data-ttu-id="79523-129">Így a logikai alkalmazás létrehozásakor adja hozzá egy másik eseményindító toostart a Logic Apps alkalmazást, mint **ütemezés - ismétlődési**, vagy **kérelem / válasz - válasz**.</span><span class="sxs-lookup"><span data-stu-id="79523-129">So when you create your logic app, add another trigger toostart your logic app, such as **Schedule - Recurrence**, or **Request / Response - Response**.</span></span> 

1. <span data-ttu-id="79523-130">A hello [Azure-portálon](https://portal.azure.com), egy üres logikai alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="79523-130">In hello [Azure portal](https://portal.azure.com), create a blank logic app.</span></span>

2. <span data-ttu-id="79523-131">Elején hello a Logic Apps alkalmazást, válassza ki a hello **kérelem / válasz - kérelem** eseményindító:</span><span class="sxs-lookup"><span data-stu-id="79523-131">At hello start of your logic app, select hello **Request / Response - Request** trigger:</span></span> 

    ![](./media/connectors-create-api-oracledatabase/request-trigger.png)

3. <span data-ttu-id="79523-132">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="79523-132">Select **Save**.</span></span> <span data-ttu-id="79523-133">Mentésekor, a rendszer automatikusan előállítja a kérelem URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="79523-133">When you save, a request URL is automatically generated.</span></span> 

4. <span data-ttu-id="79523-134">Válassza ki **új lépés**, és válassza ki **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="79523-134">Select **New step**, and select **Add an action**.</span></span> <span data-ttu-id="79523-135">Írja be a `oracle` toosee hello elérhető műveletek:</span><span class="sxs-lookup"><span data-stu-id="79523-135">Type in `oracle` toosee hello available actions:</span></span> 

    ![](./media/connectors-create-api-oracledatabase/oracledb-actions.png)

    > [!TIP]
    > <span data-ttu-id="79523-136">Ez egyben hello leggyorsabb módon toosee hello eseményindítók és műveletek a csatlakozókat érhető el.</span><span class="sxs-lookup"><span data-stu-id="79523-136">This is also hello quickest way toosee hello triggers and actions available for any connector.</span></span> <span data-ttu-id="79523-137">Írjon be egy részén hello összekötő nevét, például `oracle`.</span><span class="sxs-lookup"><span data-stu-id="79523-137">Type in part of hello connector name, such as `oracle`.</span></span> <span data-ttu-id="79523-138">hello designer bármely eseményindítók és műveletek sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="79523-138">hello designer lists any triggers and any actions.</span></span> 

5. <span data-ttu-id="79523-139">Válasszon egyet a hello műveletek, például a **Oracle adatbázis - Get sor**.</span><span class="sxs-lookup"><span data-stu-id="79523-139">Select one of hello actions, such as **Oracle Database - Get row**.</span></span> <span data-ttu-id="79523-140">Válassza ki **keresztül, a helyszíni adatátjáró**.</span><span class="sxs-lookup"><span data-stu-id="79523-140">Select **Connect via on-premises data gateway**.</span></span> <span data-ttu-id="79523-141">Adja meg a hello Oracle-kiszolgáló neve, a hitelesítési módszert, a felhasználónevet, a jelszó és a válassza hello átjáró:</span><span class="sxs-lookup"><span data-stu-id="79523-141">Enter hello Oracle server name, authentication method, username, password, and select hello gateway:</span></span>

    ![](./media/connectors-create-api-oracledatabase/create-oracle-connection.png)

6. <span data-ttu-id="79523-142">A csatlakozás után hello listából válasszon táblát, és adja meg a hello sor azonosítója tooyour tábla.</span><span class="sxs-lookup"><span data-stu-id="79523-142">Once connected, select a table from hello list, and enter hello row ID tooyour table.</span></span> <span data-ttu-id="79523-143">Tooknow hello azonosító toohello táblában van szüksége.</span><span class="sxs-lookup"><span data-stu-id="79523-143">You need tooknow hello identifier toohello table.</span></span> <span data-ttu-id="79523-144">Ha nem ismeri, a Oracle adatbázis rendszergazdájától, és hello eredményének beolvasása `select * from yourTableName`.</span><span class="sxs-lookup"><span data-stu-id="79523-144">If you don't know, contact your Oracle DB administrator, and get hello output from `select * from yourTableName`.</span></span> <span data-ttu-id="79523-145">Ez lehetőséget biztosít, hello tooproceed kell azonosításra alkalmas adatokat.</span><span class="sxs-lookup"><span data-stu-id="79523-145">This gives you hello identifiable information you need tooproceed.</span></span>

    <span data-ttu-id="79523-146">A következő példa hello, a feladat visszaadott adat egy az emberi erőforrások adatbázisból:</span><span class="sxs-lookup"><span data-stu-id="79523-146">In hello following example, job data is being returned from a Human Resources database:</span></span> 

    ![](./media/connectors-create-api-oracledatabase/table-rowid.png)

7. <span data-ttu-id="79523-147">A következő lépésben használhatja bármelyik hello más összekötők toobuild a munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="79523-147">In this next step, you can use any of hello other connectors toobuild your workflow.</span></span> <span data-ttu-id="79523-148">Ha szeretné, hogy Oracle tootest beolvasásakor adatait, majd küldhet magának hello egyikével hello Oracle adatokkal egy e-mailt küldjön e-mailek összekötők, például Office 365 vagy Gmailes.</span><span class="sxs-lookup"><span data-stu-id="79523-148">If you want tootest getting data from Oracle, then send yourself an email with hello Oracle data using one of hello send email connectors, such Office 365 or Gmail.</span></span> <span data-ttu-id="79523-149">Használjon dinamikus tokeneket hello a hello Oracle tábla toobuild hello `Subject` és `Body` az e-mail:</span><span class="sxs-lookup"><span data-stu-id="79523-149">Use hello dynamic tokens from hello Oracle table toobuild hello `Subject` and `Body` of your email:</span></span>

    ![](./media/connectors-create-api-oracledatabase/oracle-send-email.png)

8. <span data-ttu-id="79523-150">**Mentés** a Logic Apps alkalmazást, és válassza **futtatása**.</span><span class="sxs-lookup"><span data-stu-id="79523-150">**Save** your logic app, and then select **Run**.</span></span> <span data-ttu-id="79523-151">Hello-Tervező bezárása, és nézze meg hello futtatása előzmények hello állapot.</span><span class="sxs-lookup"><span data-stu-id="79523-151">Close hello designer, and look at hello runs history for hello status.</span></span> <span data-ttu-id="79523-152">Ha a hiba, válassza ki a hello sikertelen üzenet sor.</span><span class="sxs-lookup"><span data-stu-id="79523-152">If it fails, select hello failed message row.</span></span> <span data-ttu-id="79523-153">hello designer megnyílik, és azt mutatja be, amely lépés sikertelen, valamint azt mutatja be hello hibainformáció.</span><span class="sxs-lookup"><span data-stu-id="79523-153">hello designer opens, and shows you which step failed, and also shows hello error information.</span></span> <span data-ttu-id="79523-154">Ha ez sikeres, akkor hozzáadott hello információkat e-mailt kell kapnia.</span><span class="sxs-lookup"><span data-stu-id="79523-154">If it succeeds, then you should receive an email with hello information you added.</span></span>


### <a name="workflow-ideas"></a><span data-ttu-id="79523-155">Munkafolyamat ötletek</span><span class="sxs-lookup"><span data-stu-id="79523-155">Workflow ideas</span></span>

* <span data-ttu-id="79523-156">Szeretné, hogy toomonitor hello #oracle hashtaggel történő, és hello Twitter-üzeneteket be egy adatbázist, így kérdezhetők le, és más alkalmazásokban használt.</span><span class="sxs-lookup"><span data-stu-id="79523-156">You want toomonitor hello #oracle hashtag, and put hello tweets in a database so they can be queried, and used within other applications.</span></span> <span data-ttu-id="79523-157">A logikai alkalmazás, adja hozzá a hello `Twitter - When a new tweet is posted` indítható el, és írja be a hello **#oracle** hashtaggel történő.</span><span class="sxs-lookup"><span data-stu-id="79523-157">In a logic app, add hello `Twitter - When a new tweet is posted` trigger, and enter hello **#oracle** hashtag.</span></span> <span data-ttu-id="79523-158">Adja hozzá a hello `Oracle Database - Insert row` művelet, és válassza ki a táblázatban:</span><span class="sxs-lookup"><span data-stu-id="79523-158">Then, add hello `Oracle Database - Insert row` action, and select your table:</span></span>

    ![](./media/connectors-create-api-oracledatabase/twitter-oracledb.png)

* <span data-ttu-id="79523-159">Üzenetek küldése történik tooa Service Bus-üzenetsorba.</span><span class="sxs-lookup"><span data-stu-id="79523-159">Messages are sent tooa Service Bus queue.</span></span> <span data-ttu-id="79523-160">Szeretné, hogy ezek az üzenetek tooget, és helyezze őket egy adatbázis.</span><span class="sxs-lookup"><span data-stu-id="79523-160">You want tooget these messages, and put them in a database.</span></span> <span data-ttu-id="79523-161">A logikai alkalmazás, adja hozzá a hello `Service Bus - when a message is received in a queue` eseményindító, és jelölje be hello várólista.</span><span class="sxs-lookup"><span data-stu-id="79523-161">In a logic app, add hello `Service Bus - when a message is received in a queue` trigger, and select hello queue.</span></span> <span data-ttu-id="79523-162">Adja hozzá a hello `Oracle Database - Insert row` művelet, és válassza ki a táblázatban:</span><span class="sxs-lookup"><span data-stu-id="79523-162">Then, add hello `Oracle Database - Insert row` action, and select your table:</span></span>

    ![](./media/connectors-create-api-oracledatabase/sbqueue-oracledb.png)

## <a name="common-errors"></a><span data-ttu-id="79523-163">Gyakori hibák</span><span class="sxs-lookup"><span data-stu-id="79523-163">Common errors</span></span>

#### <a name="error-cannot-reach-hello-gateway"></a><span data-ttu-id="79523-164">**Hiba**: hello átjáró nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="79523-164">**Error**: Cannot reach hello Gateway</span></span>

<span data-ttu-id="79523-165">**OK**: hello helyszíni az adatátjáró nem tud tooconnect toohello felhő.</span><span class="sxs-lookup"><span data-stu-id="79523-165">**Cause**: hello on-premises data gateway is not able tooconnect toohello cloud.</span></span> 

<span data-ttu-id="79523-166">**Megoldás**: Győződjön meg arról, hogy az átjáró fut hello a helyi számítógépen, amelyen telepítették őket, valamint, hogy az informatikai csatlakozhatnak toohello internet.</span><span class="sxs-lookup"><span data-stu-id="79523-166">**Mitigation**: Make sure your gateway is running on hello on-premises machine where you installed it, and that it can connect toohello internet.</span></span>  <span data-ttu-id="79523-167">Azt javasoljuk, hogy nem hello-átjáró telepítése egy számítógépre, amely ki van kapcsolva vagy alvó.</span><span class="sxs-lookup"><span data-stu-id="79523-167">We recommend not installing hello gateway on a computer that may be turned off or sleep.</span></span> <span data-ttu-id="79523-168">Indítsa újra a hello helyszíni átjáró szolgáltatás (PBIEgwService) is.</span><span class="sxs-lookup"><span data-stu-id="79523-168">You can also restart hello on-premises data gateway service (PBIEgwService).</span></span>

#### <a name="error-hello-provider-being-used-is-deprecated-systemdataoracleclient-requires-oracle-client-software-version-817-or-greater-please-visit-httpsgomicrosoftcomfwlinkplinkid272376httpsgomicrosoftcomfwlinkplinkid272376-tooinstall-hello-official-provider"></a><span data-ttu-id="79523-169">**Hiba**: hello használt szolgáltató elavult: "System.Data.OracleClient szükséges verziójú Oracle ügyfélszoftvert 8.1.7-es vagy újabb.".</span><span class="sxs-lookup"><span data-stu-id="79523-169">**Error**: hello provider being used is deprecated: 'System.Data.OracleClient requires Oracle client software version 8.1.7 or greater.'.</span></span> <span data-ttu-id="79523-170">Látogasson el a [https://go.microsoft.com/fwlink/p/?LinkID=272376](https://go.microsoft.com/fwlink/p/?LinkID=272376) tooinstall hello hivatalos szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="79523-170">Please visit [https://go.microsoft.com/fwlink/p/?LinkID=272376](https://go.microsoft.com/fwlink/p/?LinkID=272376) tooinstall hello official provider.</span></span>

<span data-ttu-id="79523-171">**OK**: hello Oracle ügyfél SDK nem települ hello számítógépen, ahol hello a helyszíni adatok átjáró fut-e.</span><span class="sxs-lookup"><span data-stu-id="79523-171">**Cause**: hello Oracle client SDK is not installed on hello machine where hello on-premises data gateway is running.</span></span>  

<span data-ttu-id="79523-172">**Megoldási**: Töltse le és telepítse hello Oracle ügyfél SDK hello hello helyszíni adatátjáró ugyanarra a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="79523-172">**Resolution**: Download and install hello Oracle client SDK on hello same computer as hello on-premises data gateway.</span></span>

#### <a name="error-table-tablename-does-not-define-any-key-columns"></a><span data-ttu-id="79523-173">**Hiba**: "[Tablename]" tábla nem definiál bármely Kulcsoszlopok</span><span class="sxs-lookup"><span data-stu-id="79523-173">**Error**: Table '[Tablename]' does not define any key columns</span></span>

<span data-ttu-id="79523-174">**OK**: hello táblának nincs elsődleges kulcs.</span><span class="sxs-lookup"><span data-stu-id="79523-174">**Cause**: hello table does not have any primary key.</span></span>  

<span data-ttu-id="79523-175">**Megoldási**: hello Oracle-adatbázishoz connector megköveteli, hogy a tábla elsődleges kulcsként megadott oszlop használható.</span><span class="sxs-lookup"><span data-stu-id="79523-175">**Resolution**: hello Oracle Database connector requires that a table with a primary key column be used.</span></span>

#### <a name="currently-not-supported"></a><span data-ttu-id="79523-176">Jelenleg nem támogatott</span><span class="sxs-lookup"><span data-stu-id="79523-176">Currently not supported</span></span>

* <span data-ttu-id="79523-177">Nézetek és tárolt eljárások</span><span class="sxs-lookup"><span data-stu-id="79523-177">Views and stored procedures</span></span> 
* <span data-ttu-id="79523-178">A tábla az összetett kulcsok</span><span class="sxs-lookup"><span data-stu-id="79523-178">Any table with composite keys</span></span>
* <span data-ttu-id="79523-179">Táblák egymásba ágyazott objektumtípusok</span><span class="sxs-lookup"><span data-stu-id="79523-179">Nested object types in tables</span></span>
 
## <a name="connector-specific-details"></a><span data-ttu-id="79523-180">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="79523-180">Connector-specific details</span></span>

<span data-ttu-id="79523-181">Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/oracle/).</span><span class="sxs-lookup"><span data-stu-id="79523-181">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/oracle/).</span></span> 

## <a name="get-some-help"></a><span data-ttu-id="79523-182">Segítséget</span><span class="sxs-lookup"><span data-stu-id="79523-182">Get some help</span></span>

<span data-ttu-id="79523-183">Hello [Azure Logic Apps-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) nagyszerű tooask kérdések helyezze, kérdések megválaszolása, és tekintse meg a többi Logic Apps felhasználók tevékenységeit.</span><span class="sxs-lookup"><span data-stu-id="79523-183">hello [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) is a great place tooask questions, answer questions, and see what other Logic Apps users are doing.</span></span> 

<span data-ttu-id="79523-184">Növelheti Logic Apps alkalmazások és összekötők szavazás, és a következő ötletek elküldésével [http://aka.ms/logicapps-wish](http://aka.ms/logicapps-wish).</span><span class="sxs-lookup"><span data-stu-id="79523-184">You can help improve Logic Apps and connectors by voting and submitting your ideas at [http://aka.ms/logicapps-wish](http://aka.ms/logicapps-wish).</span></span> 


## <a name="next-steps"></a><span data-ttu-id="79523-185">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="79523-185">Next steps</span></span>
<span data-ttu-id="79523-186">[Logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md), és vizsgálja meg hello rendelkezésre álló összekötők Logic Apps, az [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="79523-186">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md), and explore hello available connectors in Logic Apps at our [APIs list](apis-list.md).</span></span>
