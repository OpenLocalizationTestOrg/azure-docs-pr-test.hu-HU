---
title: "A MQ connector használata az Azure Logic Apps |} Microsoft Docs"
description: "Csatlakozás egy helyszíni vagy a Tallózás, fogadására és üzenetek küldése WebSphere MQ a logic app munkafolyamat Azure MQ server"
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
ms.openlocfilehash: 17c651585b56dae186286f5d8c68c363ae9c524d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="connect-to-an-ibm-mq-server-from-logic-apps-using-the-mq-connector"></a><span data-ttu-id="a5c53-103">A logic Apps alkalmazásokból az MQ-összekötővel IBM MQ kiszolgálóhoz kapcsolódni</span><span class="sxs-lookup"><span data-stu-id="a5c53-103">Connect to an IBM MQ server from logic apps using the MQ connector</span></span> 

<span data-ttu-id="a5c53-104">Microsoft Connector MQ küld, és beolvassa a MQ Server helyszíni, vagy az Azure-ban tárolja.</span><span class="sxs-lookup"><span data-stu-id="a5c53-104">Microsoft Connector for MQ sends and retrieves messages stored in an MQ Server on-premises, or in Azure.</span></span> <span data-ttu-id="a5c53-105">Ez az összekötő tartalmazza a Microsoft MQ-ügyfél, amely a TCP/IP-hálózaton keresztül egy távoli IBM MQ-kiszolgálóval kommunikál.</span><span class="sxs-lookup"><span data-stu-id="a5c53-105">This connector includes a Microsoft MQ client that communicates with a remote IBM MQ server across a TCP/IP network.</span></span> <span data-ttu-id="a5c53-106">Ez a dokumentum egy alapszintű útmutató a MQ-összekötő használatára.</span><span class="sxs-lookup"><span data-stu-id="a5c53-106">This document is a starter guide to use the MQ connector.</span></span> <span data-ttu-id="a5c53-107">Azt javasoljuk, hogy először keresse meg a várólista egyetlen üzenet, és az egyéb műveletek majd közben.</span><span class="sxs-lookup"><span data-stu-id="a5c53-107">We recommended you begin by browsing a single message on a queue, and then trying the other actions.</span></span>    

<span data-ttu-id="a5c53-108">A MQ az összekötő tartalmazza a következő műveleteket.</span><span class="sxs-lookup"><span data-stu-id="a5c53-108">The MQ connector includes the following actions.</span></span> <span data-ttu-id="a5c53-109">Nincsenek nincsenek eseményindítók.</span><span class="sxs-lookup"><span data-stu-id="a5c53-109">There are no triggers.</span></span>

-   <span data-ttu-id="a5c53-110">Egyetlen üzenet Tallózás az IBM MQ kiszolgálóról az üzenet törlése nélkül</span><span class="sxs-lookup"><span data-stu-id="a5c53-110">Browse a single message without deleting the message from the IBM MQ Server</span></span>
-   <span data-ttu-id="a5c53-111">Keresse meg az üzenetkötegek anélkül, hogy az üzenetek az IBM MQ Server való törlésekor</span><span class="sxs-lookup"><span data-stu-id="a5c53-111">Browse a batch of messages without deleting the messages from the IBM MQ Server</span></span>
-   <span data-ttu-id="a5c53-112">Egy üzenetet kap, és az üzenet törlése az IBM MQ Server kiszolgálóhoz</span><span class="sxs-lookup"><span data-stu-id="a5c53-112">Receive a single message and delete the message from the IBM MQ Server</span></span>
-   <span data-ttu-id="a5c53-113">Az üzenetkötegek kap, és törölje az üzenetek az IBM MQ Server kiszolgálóhoz</span><span class="sxs-lookup"><span data-stu-id="a5c53-113">Receive a batch of messages and delete the messages from the IBM MQ Server</span></span>
-   <span data-ttu-id="a5c53-114">Egyetlen üzenet küldése az IBM MQ Server kiszolgálóhoz</span><span class="sxs-lookup"><span data-stu-id="a5c53-114">Send a single message to the IBM MQ Server</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="a5c53-115">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a5c53-115">Prerequisites</span></span>

* <span data-ttu-id="a5c53-116">Ha helyszíni MQ kiszolgálót, [az helyszíni átjáró telepítése](../logic-apps/logic-apps-gateway-install.md) a hálózaton belül a kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="a5c53-116">If using an on-premises MQ server, [install the on-premises data gateway](../logic-apps/logic-apps-gateway-install.md) on a server within your network.</span></span> <span data-ttu-id="a5c53-117">Ha a MQ Server nyilvánosan elérhető, vagy Azure-ban érhető el, majd az átjáró nem használt vagy szükséges.</span><span class="sxs-lookup"><span data-stu-id="a5c53-117">If the MQ Server is publicly available, or available within Azure, then the data gateway is not used or required.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a5c53-118">A kiszolgáló, amelyen telepítve van-e az On-Premises Data Gateway is rendelkeznie kell a .net keretrendszer 4.6-os függvény MQ-összekötő telepítve.</span><span class="sxs-lookup"><span data-stu-id="a5c53-118">The server where the On-Premises Data Gateway is installed must also have .Net Framework 4.6 installed for the MQ Connector to function.</span></span>

* <span data-ttu-id="a5c53-119">A helyszíni adatok Gateway - Azure-erőforrás létrehozása [az átjáró-kapcsolat beállítása](../logic-apps/logic-apps-gateway-connection.md).</span><span class="sxs-lookup"><span data-stu-id="a5c53-119">Create the Azure resource for the on-premises data gateway - [Set up the data gateway connection](../logic-apps/logic-apps-gateway-connection.md).</span></span>

* <span data-ttu-id="a5c53-120">Hivatalos támogatott IBM WebSphere MQ-verziók:</span><span class="sxs-lookup"><span data-stu-id="a5c53-120">Officially supported IBM WebSphere MQ versions:</span></span>
   * <span data-ttu-id="a5c53-121">7.5 MQ</span><span class="sxs-lookup"><span data-stu-id="a5c53-121">MQ 7.5</span></span>
   * <span data-ttu-id="a5c53-122">MQ 8.0</span><span class="sxs-lookup"><span data-stu-id="a5c53-122">MQ 8.0</span></span>

## <a name="create-a-logic-app"></a><span data-ttu-id="a5c53-123">Logikai alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="a5c53-123">Create a logic app</span></span>

1. <span data-ttu-id="a5c53-124">Az a **Azure start Bizottsága**, jelölje be  **+**  (plusz jelre), **Web + mobil**, majd **logikai alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a5c53-124">In the **Azure start board**, select **+** (plus sign), **Web + Mobile**, and then **Logic App**.</span></span> 
2. <span data-ttu-id="a5c53-125">Adja meg a **neve**, például MQTestApp, **előfizetés**, **erőforráscsoport**, és **hely** (használhatja azt a helyet ahol a helyszíni Data Gateway kapcsolat van konfigurálva).</span><span class="sxs-lookup"><span data-stu-id="a5c53-125">Enter the **Name**, such as MQTestApp, **Subscription**, **Resource group**, and **Location** (use the location where the on-premises Data Gateway connection is configured).</span></span> <span data-ttu-id="a5c53-126">Válassza ki **rögzítés az irányítópulton**, és válassza ki **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="a5c53-126">Select **Pin to dashboard**, and select **Create**.</span></span>  
<span data-ttu-id="a5c53-127">![Logikai alkalmazás létrehozása](media/connectors-create-api-mq/Create_Logic_App.png)</span><span class="sxs-lookup"><span data-stu-id="a5c53-127">![Create Logic App](media/connectors-create-api-mq/Create_Logic_App.png)</span></span>

## <a name="add-a-trigger"></a><span data-ttu-id="a5c53-128">Egy eseményindító hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a5c53-128">Add a trigger</span></span>

> [!NOTE]
> <span data-ttu-id="a5c53-129">Az MQ-összekötő nem rendelkezik a eseményindítókat.</span><span class="sxs-lookup"><span data-stu-id="a5c53-129">The MQ Connector does not have any triggers.</span></span> <span data-ttu-id="a5c53-130">Igen, a másik eseményindító segítségével indítsa el a logikai alkalmazás, például a **ismétlődési** eseményindító.</span><span class="sxs-lookup"><span data-stu-id="a5c53-130">So, use another trigger to start your logic app, such as the **Recurrence** trigger.</span></span> 

1. <span data-ttu-id="a5c53-131">A **Logic Apps Designer** megnyílik, jelölje be **ismétlődési** közös eseményindítók listájában.</span><span class="sxs-lookup"><span data-stu-id="a5c53-131">The **Logic Apps Designer** opens, select **Recurrence** in the list of common triggers.</span></span>
2. <span data-ttu-id="a5c53-132">Válassza ki **szerkesztése** belül az ismétlődési eseményindító.</span><span class="sxs-lookup"><span data-stu-id="a5c53-132">Select **Edit** within the Recurrence Trigger.</span></span> 
3. <span data-ttu-id="a5c53-133">Állítsa be a **gyakoriság** való **nap**, és állítsa be a **időköz** való **7**.</span><span class="sxs-lookup"><span data-stu-id="a5c53-133">Set the **Frequency** to **Day**, and set the **Interval** to **7**.</span></span> 

## <a name="browse-a-single-message"></a><span data-ttu-id="a5c53-134">Egyetlen üzenet tallózása</span><span class="sxs-lookup"><span data-stu-id="a5c53-134">Browse a single message</span></span>
1. <span data-ttu-id="a5c53-135">Válassza ki **+ új lépés**, és válassza ki **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="a5c53-135">Select **+ New step**, and select **Add an action**.</span></span>
2. <span data-ttu-id="a5c53-136">Írja be a keresőmezőbe, `mq`, majd válassza ki **MQ - Tallózás üzenet**.</span><span class="sxs-lookup"><span data-stu-id="a5c53-136">In the search box, type `mq`, and then select **MQ - Browse message**.</span></span>  
<span data-ttu-id="a5c53-137">![Keresse meg az üzenet](media/connectors-create-api-mq/Browse_message.png)</span><span class="sxs-lookup"><span data-stu-id="a5c53-137">![Browse message](media/connectors-create-api-mq/Browse_message.png)</span></span>

3. <span data-ttu-id="a5c53-138">Ha nincs meglévő kapcsolat MQ, majd hozza létre a kapcsolat:</span><span class="sxs-lookup"><span data-stu-id="a5c53-138">If there isn't an existing MQ connection, then create the connection:</span></span>  

    1. <span data-ttu-id="a5c53-139">Válassza ki **keresztül, a helyszíni adatátjáró**, és írja be a MQ-kiszolgáló tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="a5c53-139">Select **Connect via on-premises data gateway**, and enter the properties of your MQ server.</span></span>  
    <span data-ttu-id="a5c53-140">A **Server**, adja meg a MQ-kiszolgáló nevét, vagy adja meg az IP-cím, egy kettőspontot és a port számát.</span><span class="sxs-lookup"><span data-stu-id="a5c53-140">For **Server**, you can enter the MQ server name, or enter the IP address followed by a colon and the port number.</span></span> 
    2. <span data-ttu-id="a5c53-141">A **átjáró** legördülő lista ismerteti a meglévő átjáró kapcsolatokat, amelyek vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="a5c53-141">The **gateway** dropdown lists any existing gateway connections that have been configured.</span></span> <span data-ttu-id="a5c53-142">Válassza ki az átjárót.</span><span class="sxs-lookup"><span data-stu-id="a5c53-142">Select your gateway.</span></span>
    3. <span data-ttu-id="a5c53-143">Válassza ki **létrehozása** befejezésekor.</span><span class="sxs-lookup"><span data-stu-id="a5c53-143">Select **Create** when finished.</span></span> <span data-ttu-id="a5c53-144">A kapcsolat az alábbihoz hasonlít:</span><span class="sxs-lookup"><span data-stu-id="a5c53-144">Your connection looks similar to the following:</span></span>   
    <span data-ttu-id="a5c53-145">![Kapcsolat tulajdonságai](media/connectors-create-api-mq/Connection_Properties.png)</span><span class="sxs-lookup"><span data-stu-id="a5c53-145">![Connection Properties](media/connectors-create-api-mq/Connection_Properties.png)</span></span>

4. <span data-ttu-id="a5c53-146">A művelet tulajdonságait, a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="a5c53-146">In the action properties, you can:</span></span>  

    * <span data-ttu-id="a5c53-147">Használja a **várólista** tulajdonság egy másik várólista neve, mint a kapcsolat meghatározott eléréséhez</span><span class="sxs-lookup"><span data-stu-id="a5c53-147">Use the **Queue** property to access a different queue name than what is defined in the connection</span></span>
    * <span data-ttu-id="a5c53-148">Használja a **MessageId**, **CorrelationId**, **GroupId**, és egyéb tulajdonságok alapján különböző MQ üzenet üzenet tallózással</span><span class="sxs-lookup"><span data-stu-id="a5c53-148">Use the **MessageId**, **CorrelationId**, **GroupId**, and other properties to browse for a message based on the different MQ message properties</span></span>
    * <span data-ttu-id="a5c53-149">Állítsa be **IncludeInfo** való **igaz** azzal az információval a kimenet további üzenet.</span><span class="sxs-lookup"><span data-stu-id="a5c53-149">Set **IncludeInfo** to **True** to include additional message information in the output.</span></span> <span data-ttu-id="a5c53-150">Vagy állítsa az értékét **hamis** nem tartalmazza a kimenet további információt.</span><span class="sxs-lookup"><span data-stu-id="a5c53-150">Or, set it to **False** to not include additional message information in the output.</span></span>
    * <span data-ttu-id="a5c53-151">Adjon meg egy **időtúllépés** elemnél várakozási érkezésére üres várólistában lévő üzenetek mennyi ideig.</span><span class="sxs-lookup"><span data-stu-id="a5c53-151">Enter a **Timeout** value to determine how long to wait for a message to arrive in an empty queue.</span></span> <span data-ttu-id="a5c53-152">Ha nem ad meg, a várólista első üzenetébe lekérésére, és nincs várakozás egy hibaüzenet jelenik meg az eltelt idő.</span><span class="sxs-lookup"><span data-stu-id="a5c53-152">If nothing is entered, the first message in the queue is retrieved, and there is no time spent waiting for a message to appear.</span></span>  
    <span data-ttu-id="a5c53-153">![Keresse meg az üzenet tulajdonságai](media/connectors-create-api-mq/Browse_message_Props.png)</span><span class="sxs-lookup"><span data-stu-id="a5c53-153">![Browse Message Properties](media/connectors-create-api-mq/Browse_message_Props.png)</span></span>

5. <span data-ttu-id="a5c53-154">**Mentés** a módosításokat, majd **futtatása** a Logic Apps alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="a5c53-154">**Save** your changes, and then **Run** your logic app:</span></span>  
<span data-ttu-id="a5c53-155">![Mentse és futtatása](media/connectors-create-api-mq/Save_Run.png)</span><span class="sxs-lookup"><span data-stu-id="a5c53-155">![Save and run](media/connectors-create-api-mq/Save_Run.png)</span></span>

6. <span data-ttu-id="a5c53-156">Néhány másodperc múlva Futtatás lépéseket is látható, és megnézheti a kimenetet.</span><span class="sxs-lookup"><span data-stu-id="a5c53-156">After a few seconds, the steps of the run are shown, and you can look at the output.</span></span> <span data-ttu-id="a5c53-157">Válassza ki az egyes lépések részletes zöld pipa.</span><span class="sxs-lookup"><span data-stu-id="a5c53-157">Select the green checkmark to see details of each step.</span></span> <span data-ttu-id="a5c53-158">Válassza ki **tekintse meg a nyers kimenetek** kattintva további információt olvashat a kimeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="a5c53-158">Select **See raw outputs** to see additional details on the output data.</span></span>  
<span data-ttu-id="a5c53-159">![Keresse meg a kimeneti üzenet](media/connectors-create-api-mq/Browse_message_output.png)</span><span class="sxs-lookup"><span data-stu-id="a5c53-159">![Browse message output](media/connectors-create-api-mq/Browse_message_output.png)</span></span>  

    <span data-ttu-id="a5c53-160">Nyers kimenete:</span><span class="sxs-lookup"><span data-stu-id="a5c53-160">Raw output:</span></span>  
    ![Keresse meg a nyers kimeneti üzenet](media/connectors-create-api-mq/Browse_message_raw_output.png)

7. <span data-ttu-id="a5c53-162">Ha a **IncludeInfo** beállítás értéke igaz, a következő eredmény jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="a5c53-162">When the **IncludeInfo** option is set to true, the following output is displayed:</span></span>  
<span data-ttu-id="a5c53-163">![Tallózás hibaüzenet tartalmazza adatai](media/connectors-create-api-mq/Browse_message_Include_Info.png)</span><span class="sxs-lookup"><span data-stu-id="a5c53-163">![Browse message include info](media/connectors-create-api-mq/Browse_message_Include_Info.png)</span></span>

## <a name="browse-multiple-messages"></a><span data-ttu-id="a5c53-164">Keresse meg a több üzenetet</span><span class="sxs-lookup"><span data-stu-id="a5c53-164">Browse multiple messages</span></span>
<span data-ttu-id="a5c53-165">A **keresse meg az üzenetek** művelet tartalmaz egy **BatchSize** beállítást annak jelzésére, hogy hány üzenetek vissza kell adni az üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="a5c53-165">The **Browse messages** action includes a **BatchSize** option to indicate how many messages should be returned from the queue.</span></span>  <span data-ttu-id="a5c53-166">Ha **BatchSize** nem tartozik bejegyzés, a rendszer visszairányítja az összes üzenet.</span><span class="sxs-lookup"><span data-stu-id="a5c53-166">If **BatchSize** has no entry, all messages are returned.</span></span> <span data-ttu-id="a5c53-167">A visszaadott eredménye üzenetek tömbjét.</span><span class="sxs-lookup"><span data-stu-id="a5c53-167">The returned output is an array of messages.</span></span>

1. <span data-ttu-id="a5c53-168">Hozzáadásakor a **keresse meg az üzenetek** művelet, az első csatlakozás konfigurált alapértelmezettként van beállítva.</span><span class="sxs-lookup"><span data-stu-id="a5c53-168">When adding the **Browse messages** action, the first connection that is configured is selected by default.</span></span> <span data-ttu-id="a5c53-169">Válassza ki **kapcsolat módosítása** hozzon létre egy új kapcsolatot, vagy válasszon egy másik kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="a5c53-169">Select **Change connection** to create a new connection, or select a different connection.</span></span>

2. <span data-ttu-id="a5c53-170">Tallózással keresse meg a kimeneti üzenetek megjelenítése:</span><span class="sxs-lookup"><span data-stu-id="a5c53-170">The output of Browse messages shows:</span></span>  
![Keresse meg az üzenetek kimeneti](media/connectors-create-api-mq/Browse_messages_output.png)

## <a name="receive-a-single-message"></a><span data-ttu-id="a5c53-172">Egyetlen üzenet</span><span class="sxs-lookup"><span data-stu-id="a5c53-172">Receive a single message</span></span>
<span data-ttu-id="a5c53-173">A **Receive üzenet** művelet ugyanazokkal a be van, és kiírja, mint a **Tallózás üzenet** művelet.</span><span class="sxs-lookup"><span data-stu-id="a5c53-173">The **Receive message** action has the same inputs and outputs as the **Browse message** action.</span></span> <span data-ttu-id="a5c53-174">Használata esetén **Receive üzenet**, az üzenet törlődik az üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="a5c53-174">When using **Receive message**, the message is deleted from the queue.</span></span>

## <a name="receive-multiple-messages"></a><span data-ttu-id="a5c53-175">Több üzenetet</span><span class="sxs-lookup"><span data-stu-id="a5c53-175">Receive multiple messages</span></span>
<span data-ttu-id="a5c53-176">A **üzeneteket fogadni** művelet ugyanazokkal a be van, és kiírja, mint a **keresse meg az üzenetek** művelet.</span><span class="sxs-lookup"><span data-stu-id="a5c53-176">The **Receive messages** action has the same inputs and outputs as the **Browse messages** action.</span></span> <span data-ttu-id="a5c53-177">Használata esetén **üzeneteket fogadni**, az üzenetek törlődnek a várólistából.</span><span class="sxs-lookup"><span data-stu-id="a5c53-177">When using **Receive messages**, the messages are deleted from the queue.</span></span>

<span data-ttu-id="a5c53-178">Ha nincsenek üzenetek a várólista a Tallózás gombra vagy a fogadás során, a lépés sikertelen lesz, a következő eredménnyel:</span><span class="sxs-lookup"><span data-stu-id="a5c53-178">If there are no messages in the queue when doing a browse or a receive, the step fails with the following output:</span></span>  
![MQ nem üzenet hiba](media/connectors-create-api-mq/MQ_No_Msg_Error.png)

## <a name="send-a-message"></a><span data-ttu-id="a5c53-180">Üzenet küldése</span><span class="sxs-lookup"><span data-stu-id="a5c53-180">Send a message</span></span>
1. <span data-ttu-id="a5c53-181">Hozzáadásakor a **üzenet küldése** művelet, az első csatlakozás konfigurált alapértelmezettként van beállítva.</span><span class="sxs-lookup"><span data-stu-id="a5c53-181">When adding the **Send message** action, the first connection that is configured is selected by default.</span></span> <span data-ttu-id="a5c53-182">Válassza ki **kapcsolat módosítása** hozzon létre egy új kapcsolatot, vagy válasszon egy másik kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="a5c53-182">Select **Change connection** to create a new connection, or select a different connection.</span></span> <span data-ttu-id="a5c53-183">Az érvényes **üzenettípusok** vannak **Datagram**, **válasz**, vagy **kérelem**.</span><span class="sxs-lookup"><span data-stu-id="a5c53-183">The valid **Message Types** are **Datagram**, **Reply**, or **Request**.</span></span>  
<span data-ttu-id="a5c53-184">![Üzenet tulajdonságai küldése](media/connectors-create-api-mq/Send_Msg_Props.png)</span><span class="sxs-lookup"><span data-stu-id="a5c53-184">![Send Msg Props](media/connectors-create-api-mq/Send_Msg_Props.png)</span></span>

2. <span data-ttu-id="a5c53-185">Üzenet küldése kimenete a következőképpen néznek:</span><span class="sxs-lookup"><span data-stu-id="a5c53-185">The output of Send message looks like the following:</span></span>  
![Üzenet kimenetként](media/connectors-create-api-mq/Send_Msg_Output.png)

## <a name="connector-specific-details"></a><span data-ttu-id="a5c53-187">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="a5c53-187">Connector-specific details</span></span>

<span data-ttu-id="a5c53-188">Bármely eseményindítók és a swagger definiált műveletek megtekintése, és semmilyen határnak a Lásd még: a [connector részleteket](/connectors/mq/).</span><span class="sxs-lookup"><span data-stu-id="a5c53-188">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/mq/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a5c53-189">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a5c53-189">Next steps</span></span>
<span data-ttu-id="a5c53-190">[Logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="a5c53-190">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="a5c53-191">Az egyéb rendelkezésre álló összekötők Logic Apps, megismerkedhet a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="a5c53-191">Explore the other available connectors in Logic Apps at our [APIs list](apis-list.md).</span></span>
