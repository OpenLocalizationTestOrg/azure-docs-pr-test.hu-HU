---
title: "Az IoT távoli megfigyelési és az Azure Logic Apps értesítések |} Microsoft Docs"
description: "IoT az IoT hub figyelését hőmérséklet Azure Logic Apps használni, és automatikusan az e-mail értesítéseket küldhet a bármely észlelhető rendellenességeket postaládájához."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "figyelés, az iot-értesítések, IOT iot hőmérséklet figyelése"
ms.assetid: 43043067-2e1f-42c9-953d-e2dce8fd86df
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: xshi
ms.openlocfilehash: 7a611912ae55eb22103539dbba9f1a06aaa543b7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="iot-remote-monitoring-and-notifications-with-azure-logic-apps-connecting-your-iot-hub-and-mailbox"></a><span data-ttu-id="8c8d1-104">Az IoT távoli figyelés és az értesítések az Azure Logic Apps csatlakoztatása az IoT-központ és postaláda</span><span class="sxs-lookup"><span data-stu-id="8c8d1-104">IoT remote monitoring and notifications with Azure Logic Apps connecting your IoT hub and mailbox</span></span>

![Végpontok közötti diagramja](media/iot-hub-get-started-e2e-diagram/7.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

<span data-ttu-id="8c8d1-106">Az Azure Logic Apps segítségével lépések sorozataként folyamatok automatizálásához.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-106">Azure Logic Apps provides a way to automate processes as a series of steps.</span></span> <span data-ttu-id="8c8d1-107">Logikai alkalmazás különböző szolgáltatások és protokollok keresztül is elérheti.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-107">A logic app can connect across various services and protocols.</span></span> <span data-ttu-id="8c8d1-108">Megkezdi az eseményindító például a "Ha a fiók hozzáadása" és a műveletek, például "a leküldéses értesítések küldését" egyik kombinációja követ.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-108">It begins with a trigger such as 'When an account is added', and followed by a combination of actions, one like 'sending a push notification'.</span></span> <span data-ttu-id="8c8d1-109">A szolgáltatás elérhetővé teszi a Logic Apps tökéletes megoldás az IoT-megoldás az IoT figyelésére, ilyen például a riasztás a rendellenességek észlelését, más használati forgatókönyvek között marad.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-109">This feature makes Logic Apps a perfect IoT solution for IoT monitoring, such as staying alert for anomalies, among other usage scenarios.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="8c8d1-110">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="8c8d1-110">What you learn</span></span>

<span data-ttu-id="8c8d1-111">Megismerheti az IoT hub és a postaláda hőmérséklet figyelési és értesítések csatlakozó logikai alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-111">You learn how to create a logic app that connects your IoT hub and your mailbox for temperature monitoring and notifications.</span></span> <span data-ttu-id="8c8d1-112">Amikor a hőmérséklet 30 C fölött van, az ügyfélalkalmazás jelöli meg `temperatureAlert = "true"` küld az IoT hub üzenetben.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-112">When the temperature is above 30 C, the client application marks `temperatureAlert = "true"` in the message it sends to your IoT hub.</span></span> <span data-ttu-id="8c8d1-113">Az üzenet a logikai alkalmazás e-mail értesítés küldése váltja ki.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-113">The message triggers the logic app to send you an email notification.</span></span>

## <a name="what-you-do"></a><span data-ttu-id="8c8d1-114">Mit</span><span class="sxs-lookup"><span data-stu-id="8c8d1-114">What you do</span></span>

* <span data-ttu-id="8c8d1-115">Service bus-névtér létrehozása, és egy sor felvétele.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-115">Create a service bus namespace and add a queue to it.</span></span>
* <span data-ttu-id="8c8d1-116">A végpont és útválasztási szabály hozzáadása az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-116">Add an endpoint and a routing rule to your IoT hub.</span></span>
* <span data-ttu-id="8c8d1-117">Hozzon létre, beállította és ellenőrizte a logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-117">Create, configure, and test a logic app.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="8c8d1-118">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="8c8d1-118">What you need</span></span>

* <span data-ttu-id="8c8d1-119">Az oktatóanyag [beállítani az eszközét](iot-hub-raspberry-pi-kit-node-get-started.md) fejeződött be, amely hozzá van rendelve az alábbi követelményeknek:</span><span class="sxs-lookup"><span data-stu-id="8c8d1-119">Tutorial [Setup your device](iot-hub-raspberry-pi-kit-node-get-started.md) completed which covers the following requirements:</span></span>
  * <span data-ttu-id="8c8d1-120">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-120">An active Azure subscription.</span></span>
  * <span data-ttu-id="8c8d1-121">Az előfizetéshez tartozó Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-121">An Azure IoT hub under your subscription.</span></span>
  * <span data-ttu-id="8c8d1-122">Egy ügyfélalkalmazást, amely üzeneteket küld az Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-122">A client application that sends messages to your Azure IoT hub.</span></span>

## <a name="create-service-bus-namespace-and-add-a-queue-to-it"></a><span data-ttu-id="8c8d1-123">Service bus-névtér létrehozása, és egy sor felvétele</span><span class="sxs-lookup"><span data-stu-id="8c8d1-123">Create service bus namespace and add a queue to it</span></span>

### <a name="create-a-service-bus-namespace"></a><span data-ttu-id="8c8d1-124">Service bus-névtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="8c8d1-124">Create a service bus namespace</span></span>

1. <span data-ttu-id="8c8d1-125">Az a [Azure-portálon](https://portal.azure.com/), kattintson a **új** > **vállalati integrációs** > **Service Bus**.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-125">On the [Azure portal](https://portal.azure.com/), click **New** > **Enterprise Integration** > **Service Bus**.</span></span>
1. <span data-ttu-id="8c8d1-126">Adja meg a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="8c8d1-126">Provide the following information:</span></span>

   <span data-ttu-id="8c8d1-127">**Név**: a service bus nevét.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-127">**Name**: The name of the service bus.</span></span>

   <span data-ttu-id="8c8d1-128">**A tarifacsomag**: kattintson a **alapvető** > **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-128">**Pricing tier**: Click **Basic** > **Select**.</span></span> <span data-ttu-id="8c8d1-129">Az alapszintű rétegben is elegendő ehhez az oktatóanyaghoz.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-129">The Basic tier is sufficient for this tutorial.</span></span>

   <span data-ttu-id="8c8d1-130">**Erőforráscsoport**: használja ugyanazt az erőforráscsoportot, amely az IoT hub használja.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-130">**Resource group**: Use the same resource group that your IoT hub uses.</span></span>

   <span data-ttu-id="8c8d1-131">**Hely**: ugyanazt a helyet, amely az IoT hub használja.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-131">**Location**: Use the same location that your IoT hub uses.</span></span>
1. <span data-ttu-id="8c8d1-132">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-132">Click **Create**.</span></span>

   ![A service bus-névtér létrehozása az Azure portálon](media/iot-hub-monitoring-notifications-with-azure-logic-apps/1_create-service-bus-namespace-azure-portal.png)

### <a name="add-a-service-bus-queue"></a><span data-ttu-id="8c8d1-134">Adja hozzá a service bus-üzenetsorba</span><span class="sxs-lookup"><span data-stu-id="8c8d1-134">Add a service bus queue</span></span>

1. <span data-ttu-id="8c8d1-135">Nyissa meg a service bus-névtér, és kattintson a **+ várólista**.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-135">Open the service bus namespace, and then click **+ Queue**.</span></span>
1. <span data-ttu-id="8c8d1-136">Adja meg a várólista nevét, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-136">Enter a name for the queue and then click **Create**.</span></span>
1. <span data-ttu-id="8c8d1-137">Nyissa meg a service bus-üzenetsorba, és kattintson a **megosztott elérési házirendek** > **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-137">Open the service bus queue, and then click **Shared access policies** > **+ Add**.</span></span>
1. <span data-ttu-id="8c8d1-138">Adjon meg egy nevet a házirend, jelölőnégyzet **kezelése**, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-138">Enter a name for the policy, check **Manage**, and then click **Create**.</span></span>

   ![A service bus-üzenetsorba hozzáadása az Azure-portálon](media/iot-hub-monitoring-notifications-with-azure-logic-apps/2_add-service-bus-queue-azure-portal.png)

## <a name="add-an-endpoint-and-a-routing-rule-to-your-iot-hub"></a><span data-ttu-id="8c8d1-140">A végpont és útválasztási szabály hozzáadása az IoT hub</span><span class="sxs-lookup"><span data-stu-id="8c8d1-140">Add an endpoint and a routing rule to your IoT hub</span></span>

### <a name="add-an-endpoint"></a><span data-ttu-id="8c8d1-141">Végpont hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8c8d1-141">Add an endpoint</span></span>

1. <span data-ttu-id="8c8d1-142">Nyissa meg az IoT hub, kattintson a végpontok > + Hozzáadás.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-142">Open your IoT hub, click Endpoints > + Add.</span></span>
1. <span data-ttu-id="8c8d1-143">Adja meg a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="8c8d1-143">Enter the following information:</span></span>

   <span data-ttu-id="8c8d1-144">**Név**: a végpont neve.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-144">**Name**: The name of the endpoint.</span></span>

   <span data-ttu-id="8c8d1-145">**Típusú végpont**: válasszon **Service Bus-üzenetsorba**.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-145">**Endpoint type**: Select **Service Bus Queue**.</span></span>

   <span data-ttu-id="8c8d1-146">**Service Bus-névtér**: válassza ki a létrehozott névtér.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-146">**Service Bus namespace**: Select the namespace you created.</span></span>

   <span data-ttu-id="8c8d1-147">**Service Bus-üzenetsorba**: válassza ki a létrehozott üzenetsorba.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-147">**Service Bus queue**: Select the queue you created.</span></span>
1. <span data-ttu-id="8c8d1-148">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-148">Click **OK**.</span></span>

   ![A végpont hozzáadása az Azure-portálon az IoT hub](media/iot-hub-monitoring-notifications-with-azure-logic-apps/3_add-iot-hub-endpoint-azure-portal.png)

### <a name="add-a-routing-rule"></a><span data-ttu-id="8c8d1-150">Útválasztási szabály hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8c8d1-150">Add a routing rule</span></span>

1. <span data-ttu-id="8c8d1-151">Kattintson az IoT hub **útvonalak** > **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-151">In your IoT hub, click **Routes** > **+ Add**.</span></span>
1. <span data-ttu-id="8c8d1-152">Adja meg a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="8c8d1-152">Enter the following information:</span></span>

   <span data-ttu-id="8c8d1-153">**Név**: az útválasztási szabály nevét.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-153">**Name**: The name of the routing rule.</span></span>

   <span data-ttu-id="8c8d1-154">**Az adatforrás**: válasszon **DeviceMessages**.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-154">**Data source**: Select **DeviceMessages**.</span></span>

   <span data-ttu-id="8c8d1-155">**Végpont**: válassza ki a létrehozott végpontot.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-155">**Endpoint**: Select the endpoint you created.</span></span>

   <span data-ttu-id="8c8d1-156">**Lekérdezési karakterlánc**: Adjon meg `temperatureAlert = "true"`.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-156">**Query string**: Enter `temperatureAlert = "true"`.</span></span>
1. <span data-ttu-id="8c8d1-157">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-157">Click **Save**.</span></span>

   ![Útválasztási szabály hozzáadása az Azure-portálon](media/iot-hub-monitoring-notifications-with-azure-logic-apps/4_add-routing-rule-azure-portal.png)

## <a name="create-and-configure-a-logic-app"></a><span data-ttu-id="8c8d1-159">Hozza létre és konfigurálja a logikai alkalmazás</span><span class="sxs-lookup"><span data-stu-id="8c8d1-159">Create and configure a logic app</span></span>

### <a name="create-a-logic-app"></a><span data-ttu-id="8c8d1-160">Logikai alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="8c8d1-160">Create a logic app</span></span>

1. <span data-ttu-id="8c8d1-161">Az a [Azure-portálon](https://portal.azure.com/), kattintson a **új** > **vállalati integrációs** > **logikai alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-161">In the [Azure portal](https://portal.azure.com/), click **New** > **Enterprise Integration** > **Logic App**.</span></span>
1. <span data-ttu-id="8c8d1-162">Adja meg a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="8c8d1-162">Enter the following information:</span></span>

   <span data-ttu-id="8c8d1-163">**Név**: a logikai alkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-163">**Name**: The name of the logic app.</span></span>

   <span data-ttu-id="8c8d1-164">**Erőforráscsoport**: használja ugyanazt az erőforráscsoportot, amely az IoT hub használja.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-164">**Resource group**: Use the same resource group that your IoT hub uses.</span></span>

   <span data-ttu-id="8c8d1-165">**Hely**: ugyanazt a helyet, amely az IoT hub használja.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-165">**Location**: Use the same location that your IoT hub uses.</span></span>
1. <span data-ttu-id="8c8d1-166">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-166">Click **Create**.</span></span>

### <a name="configure-the-logic-app"></a><span data-ttu-id="8c8d1-167">A logikai alkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8c8d1-167">Configure the logic app</span></span>

1. <span data-ttu-id="8c8d1-168">Nyissa meg a logikai alkalmazást, amely megnyitja a Logic Apps Tervező be.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-168">Open the logic app that opens into the Logic Apps Designer.</span></span>
1. <span data-ttu-id="8c8d1-169">A Logic Apps tervezőben kattintson **üres logikai alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-169">In the Logic Apps Designer, click **Blank Logic App**.</span></span>

   ![Indítsa el egy üres logikai alkalmazást az Azure-portálon](media/iot-hub-monitoring-notifications-with-azure-logic-apps/5_start-with-blank-logic-app-azure-portal.png)

1. <span data-ttu-id="8c8d1-171">Kattintson a **Service Bus**.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-171">Click **Service Bus**.</span></span>

   ![Válassza ki a Service Bus számára, hogy a logikai alkalmazás létrehozása az Azure-portálon](media/iot-hub-monitoring-notifications-with-azure-logic-apps/6_select-service-bus-when-creating-blank-logic-app-azure-portal.png)

1. <span data-ttu-id="8c8d1-173">Kattintson a **Service Bus – egy vagy több üzenet a várólistában egyszerre várakozó (automatikusan hajthat végre) érkezésekor**.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-173">Click **Service Bus – When one or more messages arrive in a queue (auto-complete)**.</span></span>
1. <span data-ttu-id="8c8d1-174">Hozzon létre egy service bus-kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-174">Create a service bus connection.</span></span>
   1. <span data-ttu-id="8c8d1-175">Adja meg a kapcsolat neve.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-175">Enter a connection name.</span></span>
   1. <span data-ttu-id="8c8d1-176">Kattintson a service bus-névtér > a service bus házirend > **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-176">Click the service bus namespace > the service bus policy > **Create**.</span></span>

      ![Hozzon létre egy service bus kapcsolatot a Logic Apps alkalmazást az Azure-portálon](media/iot-hub-monitoring-notifications-with-azure-logic-apps/7_create-service-bus-connection-in-logic-app-azure-portal.png)

   1. <span data-ttu-id="8c8d1-178">Kattintson a **Folytatás** a service bus-kapcsolat létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-178">Click **Continue** after the service bus connection is created.</span></span>
   1. <span data-ttu-id="8c8d1-179">Jelölje ki a létrehozott várólistát, és írja be `175` a **maximális üzenetek száma**</span><span class="sxs-lookup"><span data-stu-id="8c8d1-179">Select the queue that you created and enter `175` for **Maximum message count**</span></span>

      ![A service bus kapcsolati üzenetek maximális számát adja meg a Logic Apps alkalmazást](media/iot-hub-monitoring-notifications-with-azure-logic-apps/8_specify-maximum-message-count-for-service-bus-connection-logic-app-azure-portal.png)
   1. <span data-ttu-id="8c8d1-181">Kattintson a "Mentés" gombra a módosítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-181">Click "Save" button to save the changes.</span></span>

1. <span data-ttu-id="8c8d1-182">SMTP-szolgáltatás kapcsolat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-182">Create an SMTP service connection.</span></span>
   1. <span data-ttu-id="8c8d1-183">Kattintson a **új lépés** > **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-183">Click **New step** > **Add an action**.</span></span>
   1. <span data-ttu-id="8c8d1-184">Típus `SMTP`, kattintson a **SMTP** a keresési eredmény szolgáltatásra, és kattintson a **SMTP - E-mail küldése**.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-184">Type `SMTP`, click the **SMTP** service in the search result, and then click **SMTP - Send Email**.</span></span>

      ![Az SMTP-kapcsolat létrehozása a Logic Apps alkalmazást az Azure-portálon](media/iot-hub-monitoring-notifications-with-azure-logic-apps/9_create-smtp-connection-logic-app-azure-portal.png)

   1. <span data-ttu-id="8c8d1-186">Adja meg a postaláda az SMTP-adatokat, és kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-186">Enter the SMTP information of your mailbox, and then click **Create**.</span></span>

      ![SMTP-kapcsolati adatok adja meg a Logic Apps alkalmazást az Azure-portálon](media/iot-hub-monitoring-notifications-with-azure-logic-apps/10_enter-smtp-connection-info-logic-app-azure-portal.png)

      <span data-ttu-id="8c8d1-188">Az SMTP adatait az beszerzése [Hotmail/Outlook.com](https://support.office.com/en-us/article/Add-your-Outlook-com-account-to-another-mail-app-73f3b178-0009-41ae-aab1-87b80fa94970), [Gmail](https://support.google.com/a/answer/176600?hl=en), és [Yahoo Mail](https://help.yahoo.com/kb/SLN4075.html).</span><span class="sxs-lookup"><span data-stu-id="8c8d1-188">Get the SMTP information for [Hotmail/Outlook.com](https://support.office.com/en-us/article/Add-your-Outlook-com-account-to-another-mail-app-73f3b178-0009-41ae-aab1-87b80fa94970), [Gmail](https://support.google.com/a/answer/176600?hl=en), and [Yahoo Mail](https://help.yahoo.com/kb/SLN4075.html).</span></span>
   1. <span data-ttu-id="8c8d1-189">Adja meg az e-mail címet a **a** és **való**, és `High temperature detected` a **tulajdonos** és **törzs**.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-189">Enter your email address for **From** and **To**, and `High temperature detected` for **Subject** and **Body**.</span></span>
   1. <span data-ttu-id="8c8d1-190">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-190">Click **Save**.</span></span>

<span data-ttu-id="8c8d1-191">A logikai alkalmazás megfelelően működik történő mentésekor.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-191">The logic app is in working order when you save it.</span></span>

## <a name="test-the-logic-app"></a><span data-ttu-id="8c8d1-192">A logikai alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="8c8d1-192">Test the logic app</span></span>

1. <span data-ttu-id="8c8d1-193">Indítsa el az ügyfélalkalmazás az eszköz üzembe helyezett [ESP8266 csatlakozzon az Azure IoT Hub](iot-hub-arduino-huzzah-esp8266-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="8c8d1-193">Start the client application that you deploy to your device in [Connect ESP8266 to Azure IoT Hub](iot-hub-arduino-huzzah-esp8266-get-started.md).</span></span>
1. <span data-ttu-id="8c8d1-194">A környezet hőmérséklet körül fent 30 c kell SensorTag növelése Például világos a gyertyák a SensorTag körül.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-194">Increase the environment temperature around the SensorTag to be above 30 C. For example, light a candle around your SensorTag.</span></span>
1. <span data-ttu-id="8c8d1-195">A logikai alkalmazás által küldött e-mailben értesítést kell kapnia.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-195">You should receive an email notification sent by the logic app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="8c8d1-196">Az e-mail-szolgáltató kell gondoskodjon róla, hogy Ön az e-mailt küld a feladónak identitásának ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-196">Your email service provider may need to verify the sender identity to make sure it is you who sends the email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8c8d1-197">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8c8d1-197">Next steps</span></span>

<span data-ttu-id="8c8d1-198">Sikeresen létrehozta a logikai alkalmazás, amely összeköti az IoT hub és a postaláda hőmérséklet figyelési és értesítések.</span><span class="sxs-lookup"><span data-stu-id="8c8d1-198">You have successfully created a logic app that connects your IoT hub and your mailbox for temperature monitoring and notifications.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
