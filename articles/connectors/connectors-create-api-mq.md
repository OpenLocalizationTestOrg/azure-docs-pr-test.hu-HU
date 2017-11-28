---
title: "aaaLearn hogyan toouse hello Azure Logic Apps MQ-összekötőjét |} Microsoft Docs"
description: "A logic app munkafolyamat toobrowse tooan a helyszíni vagy Azure MQ Server kiszolgálóhoz kapcsolódó, fogadására és üzenetek tooWebSphere MQ küldése"
services: logic-apps
author: valthom
manager: anneta
documentationcenter: 
editor: 
tags: connectors
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 06/01/2017
ms.author: valthom; ladocs
ms.openlocfilehash: 8b36d53b457ced1a7461c229aecfcf8e4ae668a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooan-ibm-mq-server-from-logic-apps-using-hello-mq-connector"></a><span data-ttu-id="96b6d-103">IBM MQ server tooan csatlakozzon a logic Apps alkalmazásokból hello MQ-összekötő segítségével</span><span class="sxs-lookup"><span data-stu-id="96b6d-103">Connect tooan IBM MQ server from logic apps using hello MQ connector</span></span> 

<span data-ttu-id="96b6d-104">Microsoft Connector MQ küld, és beolvassa a MQ Server helyszíni, vagy az Azure-ban tárolja.</span><span class="sxs-lookup"><span data-stu-id="96b6d-104">Microsoft Connector for MQ sends and retrieves messages stored in an MQ Server on-premises, or in Azure.</span></span> <span data-ttu-id="96b6d-105">Ez az összekötő tartalmazza a Microsoft MQ-ügyfél, amely a TCP/IP-hálózaton keresztül egy távoli IBM MQ-kiszolgálóval kommunikál.</span><span class="sxs-lookup"><span data-stu-id="96b6d-105">This connector includes a Microsoft MQ client that communicates with a remote IBM MQ server across a TCP/IP network.</span></span> <span data-ttu-id="96b6d-106">Ez a dokumentum egy olyan alapszintű útmutató toouse hello MQ összekötő.</span><span class="sxs-lookup"><span data-stu-id="96b6d-106">This document is a starter guide toouse hello MQ connector.</span></span> <span data-ttu-id="96b6d-107">Javasoljuk, hogy keresse meg a várólista egyetlen üzenet megkezdése és más műveletek majd próbált hello.</span><span class="sxs-lookup"><span data-stu-id="96b6d-107">We recommended you begin by browsing a single message on a queue, and then trying hello other actions.</span></span>    

<span data-ttu-id="96b6d-108">hello MQ összekötő a következő műveletek hello tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="96b6d-108">hello MQ connector includes hello following actions.</span></span> <span data-ttu-id="96b6d-109">Nincsenek nincsenek eseményindítók.</span><span class="sxs-lookup"><span data-stu-id="96b6d-109">There are no triggers.</span></span>

-   <span data-ttu-id="96b6d-110">Egyetlen üzenet Tallózás üdvözlőüzenetére eltávolítása IBM MQ Server hello nélkül</span><span class="sxs-lookup"><span data-stu-id="96b6d-110">Browse a single message without deleting hello message from hello IBM MQ Server</span></span>
-   <span data-ttu-id="96b6d-111">Keresse meg az üzenetkötegek köszönőüzenetei eltávolítása IBM MQ Server hello nélkül</span><span class="sxs-lookup"><span data-stu-id="96b6d-111">Browse a batch of messages without deleting hello messages from hello IBM MQ Server</span></span>
-   <span data-ttu-id="96b6d-112">Egy üzenetet kap, és törölni üdvözlőüzenetére hello IBM MQ Server kiszolgálóhoz</span><span class="sxs-lookup"><span data-stu-id="96b6d-112">Receive a single message and delete hello message from hello IBM MQ Server</span></span>
-   <span data-ttu-id="96b6d-113">Az üzenetkötegek kap, és törölni köszönőüzenetei hello IBM MQ Server kiszolgálóhoz</span><span class="sxs-lookup"><span data-stu-id="96b6d-113">Receive a batch of messages and delete hello messages from hello IBM MQ Server</span></span>
-   <span data-ttu-id="96b6d-114">Egy egyszeri üzenet toohello IBM MQ Server küldése</span><span class="sxs-lookup"><span data-stu-id="96b6d-114">Send a single message toohello IBM MQ Server</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="96b6d-115">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="96b6d-115">Prerequisites</span></span>

* <span data-ttu-id="96b6d-116">Ha egy helyszíni MQ server használatával [hello helyszíni adatátjáró telepítése](../logic-apps/logic-apps-gateway-install.md) egy kiszolgálón, a hálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="96b6d-116">If using an on-premises MQ server, [install hello on-premises data gateway](../logic-apps/logic-apps-gateway-install.md) on a server within your network.</span></span> <span data-ttu-id="96b6d-117">Ha hello MQ Server nyilvánosan elérhető, vagy Azure-ban érhető el, majd hello adatátjáró nem használt vagy szükséges.</span><span class="sxs-lookup"><span data-stu-id="96b6d-117">If hello MQ Server is publicly available, or available within Azure, then hello data gateway is not used or required.</span></span>

    > [!NOTE]
    > <span data-ttu-id="96b6d-118">hello kiszolgáló, ahol telepítve van a helyszíni adatátjáró hello is rendelkeznie kell a .net keretrendszer 4.6-os hello MQ összekötő toofunction telepítve.</span><span class="sxs-lookup"><span data-stu-id="96b6d-118">hello server where hello On-Premises Data Gateway is installed must also have .Net Framework 4.6 installed for hello MQ Connector toofunction.</span></span>

* <span data-ttu-id="96b6d-119">Hozzon létre hello Azure-erőforrás hello a helyszíni adatok átjáró - [hello data gateway kapcsolat](../logic-apps/logic-apps-gateway-connection.md).</span><span class="sxs-lookup"><span data-stu-id="96b6d-119">Create hello Azure resource for hello on-premises data gateway - [Set up hello data gateway connection](../logic-apps/logic-apps-gateway-connection.md).</span></span>

* <span data-ttu-id="96b6d-120">Hivatalos támogatott IBM WebSphere MQ-verziók:</span><span class="sxs-lookup"><span data-stu-id="96b6d-120">Officially supported IBM WebSphere MQ versions:</span></span>
   * <span data-ttu-id="96b6d-121">7.5 MQ</span><span class="sxs-lookup"><span data-stu-id="96b6d-121">MQ 7.5</span></span>
   * <span data-ttu-id="96b6d-122">MQ 8.0</span><span class="sxs-lookup"><span data-stu-id="96b6d-122">MQ 8.0</span></span>

## <a name="create-a-logic-app"></a><span data-ttu-id="96b6d-123">Logikai alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="96b6d-123">Create a logic app</span></span>

1. <span data-ttu-id="96b6d-124">A hello **Azure start Bizottsága**, jelölje be  **+**  (pluszjel) **Web + mobil**, majd **Logic App**.</span><span class="sxs-lookup"><span data-stu-id="96b6d-124">In hello **Azure start board**, select **+** (plus sign), **Web + Mobile**, and then **Logic App**.</span></span> 
2. <span data-ttu-id="96b6d-125">Adja meg a hello **neve**, MQTestApp, például **előfizetés**, **erőforráscsoport**, és **hely** (használhatnak hello adott hello a helyszíni Data Gateway kapcsolat van konfigurálva).</span><span class="sxs-lookup"><span data-stu-id="96b6d-125">Enter hello **Name**, such as MQTestApp, **Subscription**, **Resource group**, and **Location** (use hello location where hello on-premises Data Gateway connection is configured).</span></span> <span data-ttu-id="96b6d-126">Válassza ki **PIN-kód toodashboard**, és válassza ki **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="96b6d-126">Select **Pin toodashboard**, and select **Create**.</span></span>  
<span data-ttu-id="96b6d-127">![Logikai alkalmazás létrehozása](media/connectors-create-api-mq/Create_Logic_App.png)</span><span class="sxs-lookup"><span data-stu-id="96b6d-127">![Create Logic App](media/connectors-create-api-mq/Create_Logic_App.png)</span></span>

## <a name="add-a-trigger"></a><span data-ttu-id="96b6d-128">Egy eseményindító hozzáadása</span><span class="sxs-lookup"><span data-stu-id="96b6d-128">Add a trigger</span></span>

> [!NOTE]
> <span data-ttu-id="96b6d-129">hello MQ-összekötő nem rendelkezik a eseményindítókat.</span><span class="sxs-lookup"><span data-stu-id="96b6d-129">hello MQ Connector does not have any triggers.</span></span> <span data-ttu-id="96b6d-130">Igen, használja a logikai alkalmazás, például a hello az egy másik eseményindító toostart **ismétlődési** eseményindító.</span><span class="sxs-lookup"><span data-stu-id="96b6d-130">So, use another trigger toostart your logic app, such as hello **Recurrence** trigger.</span></span> 

1. <span data-ttu-id="96b6d-131">Hello **Logic Apps Designer** megnyílik, jelölje be **ismétlődési** közös eseményindítók hello listájában.</span><span class="sxs-lookup"><span data-stu-id="96b6d-131">hello **Logic Apps Designer** opens, select **Recurrence** in hello list of common triggers.</span></span>
2. <span data-ttu-id="96b6d-132">Válassza ki **szerkesztése** hello ismétlődési eseményindító belül.</span><span class="sxs-lookup"><span data-stu-id="96b6d-132">Select **Edit** within hello Recurrence Trigger.</span></span> 
3. <span data-ttu-id="96b6d-133">Set hello **gyakoriság** túl**nap**, és a set hello **időköz** túl**7**.</span><span class="sxs-lookup"><span data-stu-id="96b6d-133">Set hello **Frequency** too**Day**, and set hello **Interval** too**7**.</span></span> 

## <a name="browse-a-single-message"></a><span data-ttu-id="96b6d-134">Egyetlen üzenet tallózása</span><span class="sxs-lookup"><span data-stu-id="96b6d-134">Browse a single message</span></span>
1. <span data-ttu-id="96b6d-135">Válassza ki **+ új lépés**, és válassza ki **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="96b6d-135">Select **+ New step**, and select **Add an action**.</span></span>
2. <span data-ttu-id="96b6d-136">Hello keresési mezőbe, írja be a `mq`, majd válassza ki **MQ - Tallózás üzenet**.</span><span class="sxs-lookup"><span data-stu-id="96b6d-136">In hello search box, type `mq`, and then select **MQ - Browse message**.</span></span>  
<span data-ttu-id="96b6d-137">![Keresse meg az üzenet](media/connectors-create-api-mq/Browse_message.png)</span><span class="sxs-lookup"><span data-stu-id="96b6d-137">![Browse message](media/connectors-create-api-mq/Browse_message.png)</span></span>

3. <span data-ttu-id="96b6d-138">Ha nincs meglévő kapcsolat MQ, majd hello kapcsolat létrehozása:</span><span class="sxs-lookup"><span data-stu-id="96b6d-138">If there isn't an existing MQ connection, then create hello connection:</span></span>  

    1. <span data-ttu-id="96b6d-139">Válassza ki **keresztül, a helyszíni adatátjáró**, és adja meg a MQ Server hello tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="96b6d-139">Select **Connect via on-premises data gateway**, and enter hello properties of your MQ server.</span></span>  
    <span data-ttu-id="96b6d-140">A **Server**, adja meg a hello MQ kiszolgálónevet, vagy adjon meg egy kettőspontot és hello portszám követ hello IP-cím.</span><span class="sxs-lookup"><span data-stu-id="96b6d-140">For **Server**, you can enter hello MQ server name, or enter hello IP address followed by a colon and hello port number.</span></span> 
    2. <span data-ttu-id="96b6d-141">Hello **átjáró** legördülő lista ismerteti a meglévő átjáró kapcsolatokat, amelyek vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="96b6d-141">hello **gateway** dropdown lists any existing gateway connections that have been configured.</span></span> <span data-ttu-id="96b6d-142">Válassza ki az átjárót.</span><span class="sxs-lookup"><span data-stu-id="96b6d-142">Select your gateway.</span></span>
    3. <span data-ttu-id="96b6d-143">Válassza ki **létrehozása** befejezésekor.</span><span class="sxs-lookup"><span data-stu-id="96b6d-143">Select **Create** when finished.</span></span> <span data-ttu-id="96b6d-144">A kapcsolat a következőhöz hasonló toohello következő:</span><span class="sxs-lookup"><span data-stu-id="96b6d-144">Your connection looks similar toohello following:</span></span>   
    <span data-ttu-id="96b6d-145">![Kapcsolat tulajdonságai](media/connectors-create-api-mq/Connection_Properties.png)</span><span class="sxs-lookup"><span data-stu-id="96b6d-145">![Connection Properties](media/connectors-create-api-mq/Connection_Properties.png)</span></span>

4. <span data-ttu-id="96b6d-146">Hello művelet tulajdonságai, a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="96b6d-146">In hello action properties, you can:</span></span>  

    * <span data-ttu-id="96b6d-147">Használjon hello **várólista** tulajdonság tooaccess hello kapcsolat meghatározott mint egy másik várólista neve</span><span class="sxs-lookup"><span data-stu-id="96b6d-147">Use hello **Queue** property tooaccess a different queue name than what is defined in hello connection</span></span>
    * <span data-ttu-id="96b6d-148">Használjon hello **MessageId**, **CorrelationId**, **GroupId**, és egyéb tulajdonságok toobrowse hello különböző MQ üzenet tulajdonságai alapján üzenet</span><span class="sxs-lookup"><span data-stu-id="96b6d-148">Use hello **MessageId**, **CorrelationId**, **GroupId**, and other properties toobrowse for a message based on hello different MQ message properties</span></span>
    * <span data-ttu-id="96b6d-149">Állítsa be **IncludeInfo** túl**igaz** tooinclude további üzenetadatok hello kimenet.</span><span class="sxs-lookup"><span data-stu-id="96b6d-149">Set **IncludeInfo** too**True** tooinclude additional message information in hello output.</span></span> <span data-ttu-id="96b6d-150">Vagy állítsa be úgy túl**hamis** toonot is tartalmazza. további üzenet hello kimeneti.</span><span class="sxs-lookup"><span data-stu-id="96b6d-150">Or, set it too**False** toonot include additional message information in hello output.</span></span>
    * <span data-ttu-id="96b6d-151">Adjon meg egy **időtúllépés** érték toodetermine az egy üres várólistában lévő üzenetek tooarrive mennyi ideig toowait.</span><span class="sxs-lookup"><span data-stu-id="96b6d-151">Enter a **Timeout** value toodetermine how long toowait for a message tooarrive in an empty queue.</span></span> <span data-ttu-id="96b6d-152">Ha nem ad meg, hello hello várólista első üzenetébe lekérésére, és nincs várakozás a egy üzenet tooappear töltött idő.</span><span class="sxs-lookup"><span data-stu-id="96b6d-152">If nothing is entered, hello first message in hello queue is retrieved, and there is no time spent waiting for a message tooappear.</span></span>  
    <span data-ttu-id="96b6d-153">![Keresse meg az üzenet tulajdonságai](media/connectors-create-api-mq/Browse_message_Props.png)</span><span class="sxs-lookup"><span data-stu-id="96b6d-153">![Browse Message Properties](media/connectors-create-api-mq/Browse_message_Props.png)</span></span>

5. <span data-ttu-id="96b6d-154">**Mentés** a módosításokat, majd **futtatása** a Logic Apps alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="96b6d-154">**Save** your changes, and then **Run** your logic app:</span></span>  
<span data-ttu-id="96b6d-155">![Mentse és futtatása](media/connectors-create-api-mq/Save_Run.png)</span><span class="sxs-lookup"><span data-stu-id="96b6d-155">![Save and run](media/connectors-create-api-mq/Save_Run.png)</span></span>

6. <span data-ttu-id="96b6d-156">Néhány másodperc múlva futtassa hello hello lépésein is látható, és megnézheti hello kimeneti.</span><span class="sxs-lookup"><span data-stu-id="96b6d-156">After a few seconds, hello steps of hello run are shown, and you can look at hello output.</span></span> <span data-ttu-id="96b6d-157">Válassza ki a hello zöld pipa toosee részletek minden egyes lépést.</span><span class="sxs-lookup"><span data-stu-id="96b6d-157">Select hello green checkmark toosee details of each step.</span></span> <span data-ttu-id="96b6d-158">Válassza ki **tekintse meg a nyers kimenetek** toosee további részleteket a hello kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="96b6d-158">Select **See raw outputs** toosee additional details on hello output data.</span></span>  
<span data-ttu-id="96b6d-159">![Keresse meg a kimeneti üzenet](media/connectors-create-api-mq/Browse_message_output.png)</span><span class="sxs-lookup"><span data-stu-id="96b6d-159">![Browse message output](media/connectors-create-api-mq/Browse_message_output.png)</span></span>  

    <span data-ttu-id="96b6d-160">Nyers kimenete:</span><span class="sxs-lookup"><span data-stu-id="96b6d-160">Raw output:</span></span>  
    ![Keresse meg a nyers kimeneti üzenet](media/connectors-create-api-mq/Browse_message_raw_output.png)

7. <span data-ttu-id="96b6d-162">Ha hello **IncludeInfo** tootrue beállítás, a következő kimeneti hello jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="96b6d-162">When hello **IncludeInfo** option is set tootrue, hello following output is displayed:</span></span>  
<span data-ttu-id="96b6d-163">![Tallózás hibaüzenet tartalmazza adatai](media/connectors-create-api-mq/Browse_message_Include_Info.png)</span><span class="sxs-lookup"><span data-stu-id="96b6d-163">![Browse message include info](media/connectors-create-api-mq/Browse_message_Include_Info.png)</span></span>

## <a name="browse-multiple-messages"></a><span data-ttu-id="96b6d-164">Keresse meg a több üzenetet</span><span class="sxs-lookup"><span data-stu-id="96b6d-164">Browse multiple messages</span></span>
<span data-ttu-id="96b6d-165">Hello **keresse meg az üzenetek** művelet tartalmaz egy **BatchSize** beállítás tooindicate üzenetek számának vissza kell adni az hello üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="96b6d-165">hello **Browse messages** action includes a **BatchSize** option tooindicate how many messages should be returned from hello queue.</span></span>  <span data-ttu-id="96b6d-166">Ha **BatchSize** nem tartozik bejegyzés, a rendszer visszairányítja az összes üzenet.</span><span class="sxs-lookup"><span data-stu-id="96b6d-166">If **BatchSize** has no entry, all messages are returned.</span></span> <span data-ttu-id="96b6d-167">hello érkezett kimeneti üzenetek tömbjét.</span><span class="sxs-lookup"><span data-stu-id="96b6d-167">hello returned output is an array of messages.</span></span>

1. <span data-ttu-id="96b6d-168">Hello hozzáadásakor **keresse meg az üzenetek** hello első kapcsolat van konfigurálva, a művelet egyben az alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="96b6d-168">When adding hello **Browse messages** action, hello first connection that is configured is selected by default.</span></span> <span data-ttu-id="96b6d-169">Válassza ki **kapcsolat módosítása** toocreate új kapcsolatot, vagy válasszon ki egy másik kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="96b6d-169">Select **Change connection** toocreate a new connection, or select a different connection.</span></span>

2. <span data-ttu-id="96b6d-170">Tallózás hello kimenete üzenetek megjelenítése:</span><span class="sxs-lookup"><span data-stu-id="96b6d-170">hello output of Browse messages shows:</span></span>  
![Keresse meg az üzenetek kimeneti](media/connectors-create-api-mq/Browse_messages_output.png)

## <a name="receive-a-single-message"></a><span data-ttu-id="96b6d-172">Egyetlen üzenet</span><span class="sxs-lookup"><span data-stu-id="96b6d-172">Receive a single message</span></span>
<span data-ttu-id="96b6d-173">Hello **Receive üzenet** művelet rendelkezik hello azonos bemenetekhez és kimenetekhez, hello **Tallózás üzenet** művelet.</span><span class="sxs-lookup"><span data-stu-id="96b6d-173">hello **Receive message** action has hello same inputs and outputs as hello **Browse message** action.</span></span> <span data-ttu-id="96b6d-174">Használata esetén **Receive üzenet**, üdvözlőüzenetére hello várólista törlődik.</span><span class="sxs-lookup"><span data-stu-id="96b6d-174">When using **Receive message**, hello message is deleted from hello queue.</span></span>

## <a name="receive-multiple-messages"></a><span data-ttu-id="96b6d-175">Több üzenetet</span><span class="sxs-lookup"><span data-stu-id="96b6d-175">Receive multiple messages</span></span>
<span data-ttu-id="96b6d-176">Hello **üzeneteket fogadni** művelet rendelkezik hello azonos bemenetekhez és kimenetekhez, hello **keresse meg az üzenetek** művelet.</span><span class="sxs-lookup"><span data-stu-id="96b6d-176">hello **Receive messages** action has hello same inputs and outputs as hello **Browse messages** action.</span></span> <span data-ttu-id="96b6d-177">Használata esetén **üzeneteket fogadni**, köszönőüzenetei hello várólista törlődnek.</span><span class="sxs-lookup"><span data-stu-id="96b6d-177">When using **Receive messages**, hello messages are deleted from hello queue.</span></span>

<span data-ttu-id="96b6d-178">Ha nincsenek üzenetek hello várólista végrehajtásakor a Tallózás gombra, vagy egy fogadási, hello lépés meghiúsul, és a következő kimeneti hello:</span><span class="sxs-lookup"><span data-stu-id="96b6d-178">If there are no messages in hello queue when doing a browse or a receive, hello step fails with hello following output:</span></span>  
![MQ nem üzenet hiba](media/connectors-create-api-mq/MQ_No_Msg_Error.png)

## <a name="send-a-message"></a><span data-ttu-id="96b6d-180">Üzenet küldése</span><span class="sxs-lookup"><span data-stu-id="96b6d-180">Send a message</span></span>
1. <span data-ttu-id="96b6d-181">Hello hozzáadásakor **üzenet küldése** hello első kapcsolat van konfigurálva, a művelet egyben az alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="96b6d-181">When adding hello **Send message** action, hello first connection that is configured is selected by default.</span></span> <span data-ttu-id="96b6d-182">Válassza ki **kapcsolat módosítása** toocreate új kapcsolatot, vagy válasszon ki egy másik kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="96b6d-182">Select **Change connection** toocreate a new connection, or select a different connection.</span></span> <span data-ttu-id="96b6d-183">érvényes hello **üzenettípusok** vannak **Datagram**, **válasz**, vagy **kérelem**.</span><span class="sxs-lookup"><span data-stu-id="96b6d-183">hello valid **Message Types** are **Datagram**, **Reply**, or **Request**.</span></span>  
<span data-ttu-id="96b6d-184">![Üzenet tulajdonságai küldése](media/connectors-create-api-mq/Send_Msg_Props.png)</span><span class="sxs-lookup"><span data-stu-id="96b6d-184">![Send Msg Props](media/connectors-create-api-mq/Send_Msg_Props.png)</span></span>

2. <span data-ttu-id="96b6d-185">hello kimeneti üzenet küldése a következőhöz hasonló hello:</span><span class="sxs-lookup"><span data-stu-id="96b6d-185">hello output of Send message looks like hello following:</span></span>  
![Üzenet kimenetként](media/connectors-create-api-mq/Send_Msg_Output.png)

## <a name="connector-specific-details"></a><span data-ttu-id="96b6d-187">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="96b6d-187">Connector-specific details</span></span>

<span data-ttu-id="96b6d-188">Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/mq/).</span><span class="sxs-lookup"><span data-stu-id="96b6d-188">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/mq/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="96b6d-189">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="96b6d-189">Next steps</span></span>
<span data-ttu-id="96b6d-190">[Logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="96b6d-190">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="96b6d-191">Fedezze fel más rendelkezésre álló összekötők Logic Apps: hello a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="96b6d-191">Explore hello other available connectors in Logic Apps at our [APIs list](apis-list.md).</span></span>
