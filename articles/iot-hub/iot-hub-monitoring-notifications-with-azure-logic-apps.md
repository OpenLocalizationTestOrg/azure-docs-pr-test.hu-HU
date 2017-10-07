---
title: "aaaIoT távoli megfigyelési és az Azure Logic Apps értesítések |} Microsoft Docs"
description: "E-mail értesítések tooyour postaláda bármely észlelhető rendellenességeket hőmérséklet figyelés az IoT hub és automatikusan küldése IoT Azure Logic Apps használja."
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
ms.openlocfilehash: 89396528ed63c37258e1b49f342f0723e686ecb3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="iot-remote-monitoring-and-notifications-with-azure-logic-apps-connecting-your-iot-hub-and-mailbox"></a><span data-ttu-id="66fc2-104">Az IoT távoli figyelés és az értesítések az Azure Logic Apps csatlakoztatása az IoT-központ és postaláda</span><span class="sxs-lookup"><span data-stu-id="66fc2-104">IoT remote monitoring and notifications with Azure Logic Apps connecting your IoT hub and mailbox</span></span>

![Végpontok közötti diagramja](media/iot-hub-get-started-e2e-diagram/7.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

<span data-ttu-id="66fc2-106">Az Azure Logic Apps lehetőséget biztosít a lépések egy sorozatát tooautomate folyamatokat.</span><span class="sxs-lookup"><span data-stu-id="66fc2-106">Azure Logic Apps provides a way tooautomate processes as a series of steps.</span></span> <span data-ttu-id="66fc2-107">Logikai alkalmazás különböző szolgáltatások és protokollok keresztül is elérheti.</span><span class="sxs-lookup"><span data-stu-id="66fc2-107">A logic app can connect across various services and protocols.</span></span> <span data-ttu-id="66fc2-108">Megkezdi az eseményindító például a "Ha a fiók hozzáadása" és a műveletek, például "a leküldéses értesítések küldését" egyik kombinációja követ.</span><span class="sxs-lookup"><span data-stu-id="66fc2-108">It begins with a trigger such as 'When an account is added', and followed by a combination of actions, one like 'sending a push notification'.</span></span> <span data-ttu-id="66fc2-109">A szolgáltatás elérhetővé teszi a Logic Apps tökéletes megoldás az IoT-megoldás az IoT figyelésére, ilyen például a riasztás a rendellenességek észlelését, más használati forgatókönyvek között marad.</span><span class="sxs-lookup"><span data-stu-id="66fc2-109">This feature makes Logic Apps a perfect IoT solution for IoT monitoring, such as staying alert for anomalies, among other usage scenarios.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="66fc2-110">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="66fc2-110">What you learn</span></span>

<span data-ttu-id="66fc2-111">Megismerheti, hogyan toocreate logikai alkalmazás, amely összeköti az IoT hub és a postaláda hőmérséklet figyelési és értesítések.</span><span class="sxs-lookup"><span data-stu-id="66fc2-111">You learn how toocreate a logic app that connects your IoT hub and your mailbox for temperature monitoring and notifications.</span></span> <span data-ttu-id="66fc2-112">Amikor hello hőmérséklet túllépi ezt az értéket 30 C, hello ügyfél alkalmazás jelek `temperatureAlert = "true"` hello üzenetet küld tooyour IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="66fc2-112">When hello temperature is above 30 C, hello client application marks `temperatureAlert = "true"` in hello message it sends tooyour IoT hub.</span></span> <span data-ttu-id="66fc2-113">Eseményindítók hello logic app toosend üdvözlőüzenetére, e-mailben értesítést.</span><span class="sxs-lookup"><span data-stu-id="66fc2-113">hello message triggers hello logic app toosend you an email notification.</span></span>

## <a name="what-you-do"></a><span data-ttu-id="66fc2-114">Mit</span><span class="sxs-lookup"><span data-stu-id="66fc2-114">What you do</span></span>

* <span data-ttu-id="66fc2-115">Service bus-névtér létrehozása, és adja hozzá a várólista tooit.</span><span class="sxs-lookup"><span data-stu-id="66fc2-115">Create a service bus namespace and add a queue tooit.</span></span>
* <span data-ttu-id="66fc2-116">Adja hozzá a végpont és útválasztási szabály tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="66fc2-116">Add an endpoint and a routing rule tooyour IoT hub.</span></span>
* <span data-ttu-id="66fc2-117">Hozzon létre, beállította és ellenőrizte a logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="66fc2-117">Create, configure, and test a logic app.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="66fc2-118">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="66fc2-118">What you need</span></span>

* <span data-ttu-id="66fc2-119">Az oktatóanyag [beállítani az eszközét](iot-hub-raspberry-pi-kit-node-get-started.md) fejeződött be, amely hozzá van rendelve hello követelményeknek:</span><span class="sxs-lookup"><span data-stu-id="66fc2-119">Tutorial [Setup your device](iot-hub-raspberry-pi-kit-node-get-started.md) completed which covers hello following requirements:</span></span>
  * <span data-ttu-id="66fc2-120">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="66fc2-120">An active Azure subscription.</span></span>
  * <span data-ttu-id="66fc2-121">Az előfizetéshez tartozó Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="66fc2-121">An Azure IoT hub under your subscription.</span></span>
  * <span data-ttu-id="66fc2-122">Egy ügyfélalkalmazás által küldött üzenetek tooyour Azure IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="66fc2-122">A client application that sends messages tooyour Azure IoT hub.</span></span>

## <a name="create-service-bus-namespace-and-add-a-queue-tooit"></a><span data-ttu-id="66fc2-123">Service bus-névtér létrehozása és a várólista tooit hozzáadása</span><span class="sxs-lookup"><span data-stu-id="66fc2-123">Create service bus namespace and add a queue tooit</span></span>

### <a name="create-a-service-bus-namespace"></a><span data-ttu-id="66fc2-124">Service bus-névtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="66fc2-124">Create a service bus namespace</span></span>

1. <span data-ttu-id="66fc2-125">A hello [Azure-portálon](https://portal.azure.com/), kattintson a **új** > **vállalati integrációs** > **Service Bus**.</span><span class="sxs-lookup"><span data-stu-id="66fc2-125">On hello [Azure portal](https://portal.azure.com/), click **New** > **Enterprise Integration** > **Service Bus**.</span></span>
1. <span data-ttu-id="66fc2-126">Adja meg a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="66fc2-126">Provide hello following information:</span></span>

   <span data-ttu-id="66fc2-127">**Név**: hello a service bus hello nevét.</span><span class="sxs-lookup"><span data-stu-id="66fc2-127">**Name**: hello name of hello service bus.</span></span>

   <span data-ttu-id="66fc2-128">**A tarifacsomag**: kattintson a **alapvető** > **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="66fc2-128">**Pricing tier**: Click **Basic** > **Select**.</span></span> <span data-ttu-id="66fc2-129">az alapszintű csomag hello is elegendő ehhez az oktatóanyaghoz.</span><span class="sxs-lookup"><span data-stu-id="66fc2-129">hello Basic tier is sufficient for this tutorial.</span></span>

   <span data-ttu-id="66fc2-130">**Erőforráscsoport**: használata hello ugyanaz az IoT hub használó erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="66fc2-130">**Resource group**: Use hello same resource group that your IoT hub uses.</span></span>

   <span data-ttu-id="66fc2-131">**Hely**: használata hello ugyanaz az IoT hub használó hely.</span><span class="sxs-lookup"><span data-stu-id="66fc2-131">**Location**: Use hello same location that your IoT hub uses.</span></span>
1. <span data-ttu-id="66fc2-132">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="66fc2-132">Click **Create**.</span></span>

   ![Hozzon létre egy service bus-névtér hello Azure-portálon](media/iot-hub-monitoring-notifications-with-azure-logic-apps/1_create-service-bus-namespace-azure-portal.png)

### <a name="add-a-service-bus-queue"></a><span data-ttu-id="66fc2-134">Adja hozzá a service bus-üzenetsorba</span><span class="sxs-lookup"><span data-stu-id="66fc2-134">Add a service bus queue</span></span>

1. <span data-ttu-id="66fc2-135">Nyissa meg a service bus-névtér hello, és kattintson a **+ várólista**.</span><span class="sxs-lookup"><span data-stu-id="66fc2-135">Open hello service bus namespace, and then click **+ Queue**.</span></span>
1. <span data-ttu-id="66fc2-136">Adja meg a hello várólista nevét, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="66fc2-136">Enter a name for hello queue and then click **Create**.</span></span>
1. <span data-ttu-id="66fc2-137">Nyissa meg a hello service bus-üzenetsorba, és kattintson a **megosztott elérési házirendek** > **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="66fc2-137">Open hello service bus queue, and then click **Shared access policies** > **+ Add**.</span></span>
1. <span data-ttu-id="66fc2-138">Adjon meg egy nevet hello házirend, ellenőrzés **kezelése**, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="66fc2-138">Enter a name for hello policy, check **Manage**, and then click **Create**.</span></span>

   ![Adja hozzá a service bus-üzenetsorba hello Azure-portálon](media/iot-hub-monitoring-notifications-with-azure-logic-apps/2_add-service-bus-queue-azure-portal.png)

## <a name="add-an-endpoint-and-a-routing-rule-tooyour-iot-hub"></a><span data-ttu-id="66fc2-140">Adja hozzá a végpont és útválasztási szabály tooyour IoT-központ</span><span class="sxs-lookup"><span data-stu-id="66fc2-140">Add an endpoint and a routing rule tooyour IoT hub</span></span>

### <a name="add-an-endpoint"></a><span data-ttu-id="66fc2-141">Végpont hozzáadása</span><span class="sxs-lookup"><span data-stu-id="66fc2-141">Add an endpoint</span></span>

1. <span data-ttu-id="66fc2-142">Nyissa meg az IoT hub, kattintson a végpontok > + Hozzáadás.</span><span class="sxs-lookup"><span data-stu-id="66fc2-142">Open your IoT hub, click Endpoints > + Add.</span></span>
1. <span data-ttu-id="66fc2-143">Adja meg a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="66fc2-143">Enter hello following information:</span></span>

   <span data-ttu-id="66fc2-144">**Név**: hello hello végpont nevét.</span><span class="sxs-lookup"><span data-stu-id="66fc2-144">**Name**: hello name of hello endpoint.</span></span>

   <span data-ttu-id="66fc2-145">**Típusú végpont**: válasszon **Service Bus-üzenetsorba**.</span><span class="sxs-lookup"><span data-stu-id="66fc2-145">**Endpoint type**: Select **Service Bus Queue**.</span></span>

   <span data-ttu-id="66fc2-146">**Service Bus-névtér**: válassza ki a létrehozott hello névtér.</span><span class="sxs-lookup"><span data-stu-id="66fc2-146">**Service Bus namespace**: Select hello namespace you created.</span></span>

   <span data-ttu-id="66fc2-147">**Service Bus-üzenetsorba**: Select hello létrehozott üzenetsorba.</span><span class="sxs-lookup"><span data-stu-id="66fc2-147">**Service Bus queue**: Select hello queue you created.</span></span>
1. <span data-ttu-id="66fc2-148">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="66fc2-148">Click **OK**.</span></span>

   ![Adja hozzá az végpont tooyour IoT-központ hello Azure-portálon](media/iot-hub-monitoring-notifications-with-azure-logic-apps/3_add-iot-hub-endpoint-azure-portal.png)

### <a name="add-a-routing-rule"></a><span data-ttu-id="66fc2-150">Útválasztási szabály hozzáadása</span><span class="sxs-lookup"><span data-stu-id="66fc2-150">Add a routing rule</span></span>

1. <span data-ttu-id="66fc2-151">Kattintson az IoT hub **útvonalak** > **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="66fc2-151">In your IoT hub, click **Routes** > **+ Add**.</span></span>
1. <span data-ttu-id="66fc2-152">Adja meg a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="66fc2-152">Enter hello following information:</span></span>

   <span data-ttu-id="66fc2-153">**Név**: hello hello útválasztási szabály nevét.</span><span class="sxs-lookup"><span data-stu-id="66fc2-153">**Name**: hello name of hello routing rule.</span></span>

   <span data-ttu-id="66fc2-154">**Az adatforrás**: válasszon **DeviceMessages**.</span><span class="sxs-lookup"><span data-stu-id="66fc2-154">**Data source**: Select **DeviceMessages**.</span></span>

   <span data-ttu-id="66fc2-155">**Végpont**: válassza ki a létrehozott hello végpont.</span><span class="sxs-lookup"><span data-stu-id="66fc2-155">**Endpoint**: Select hello endpoint you created.</span></span>

   <span data-ttu-id="66fc2-156">**Lekérdezési karakterlánc**: Adjon meg `temperatureAlert = "true"`.</span><span class="sxs-lookup"><span data-stu-id="66fc2-156">**Query string**: Enter `temperatureAlert = "true"`.</span></span>
1. <span data-ttu-id="66fc2-157">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="66fc2-157">Click **Save**.</span></span>

   ![Az Azure-portálon hello egy útválasztási szabály hozzáadása](media/iot-hub-monitoring-notifications-with-azure-logic-apps/4_add-routing-rule-azure-portal.png)

## <a name="create-and-configure-a-logic-app"></a><span data-ttu-id="66fc2-159">Hozza létre és konfigurálja a logikai alkalmazás</span><span class="sxs-lookup"><span data-stu-id="66fc2-159">Create and configure a logic app</span></span>

### <a name="create-a-logic-app"></a><span data-ttu-id="66fc2-160">Logikai alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="66fc2-160">Create a logic app</span></span>

1. <span data-ttu-id="66fc2-161">A hello [Azure-portálon](https://portal.azure.com/), kattintson a **új** > **vállalati integrációs** > **logikai alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="66fc2-161">In hello [Azure portal](https://portal.azure.com/), click **New** > **Enterprise Integration** > **Logic App**.</span></span>
1. <span data-ttu-id="66fc2-162">Adja meg a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="66fc2-162">Enter hello following information:</span></span>

   <span data-ttu-id="66fc2-163">**Név**: hello logikai alkalmazás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="66fc2-163">**Name**: hello name of hello logic app.</span></span>

   <span data-ttu-id="66fc2-164">**Erőforráscsoport**: használata hello ugyanaz az IoT hub használó erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="66fc2-164">**Resource group**: Use hello same resource group that your IoT hub uses.</span></span>

   <span data-ttu-id="66fc2-165">**Hely**: használata hello ugyanaz az IoT hub használó hely.</span><span class="sxs-lookup"><span data-stu-id="66fc2-165">**Location**: Use hello same location that your IoT hub uses.</span></span>
1. <span data-ttu-id="66fc2-166">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="66fc2-166">Click **Create**.</span></span>

### <a name="configure-hello-logic-app"></a><span data-ttu-id="66fc2-167">Hello logikai alkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="66fc2-167">Configure hello logic app</span></span>

1. <span data-ttu-id="66fc2-168">Nyissa meg a Logic Apps Designer hello megnyíló hello logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="66fc2-168">Open hello logic app that opens into hello Logic Apps Designer.</span></span>
1. <span data-ttu-id="66fc2-169">A Logic Apps Designer hello, kattintson **üres logikai alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="66fc2-169">In hello Logic Apps Designer, click **Blank Logic App**.</span></span>

   ![Egy üres logikai alkalmazást az Azure-portálon hello kezdődnie](media/iot-hub-monitoring-notifications-with-azure-logic-apps/5_start-with-blank-logic-app-azure-portal.png)

1. <span data-ttu-id="66fc2-171">Kattintson a **Service Bus**.</span><span class="sxs-lookup"><span data-stu-id="66fc2-171">Click **Service Bus**.</span></span>

   ![Válassza ki a Service Bus toostart hello Azure-portálon a logikai alkalmazás létrehozása](media/iot-hub-monitoring-notifications-with-azure-logic-apps/6_select-service-bus-when-creating-blank-logic-app-azure-portal.png)

1. <span data-ttu-id="66fc2-173">Kattintson a **Service Bus – egy vagy több üzenet a várólistában egyszerre várakozó (automatikusan hajthat végre) érkezésekor**.</span><span class="sxs-lookup"><span data-stu-id="66fc2-173">Click **Service Bus – When one or more messages arrive in a queue (auto-complete)**.</span></span>
1. <span data-ttu-id="66fc2-174">Hozzon létre egy service bus-kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="66fc2-174">Create a service bus connection.</span></span>
   1. <span data-ttu-id="66fc2-175">Adja meg a kapcsolat neve.</span><span class="sxs-lookup"><span data-stu-id="66fc2-175">Enter a connection name.</span></span>
   1. <span data-ttu-id="66fc2-176">Kattintson a service bus-névtér hello > service bus házirend hello > **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="66fc2-176">Click hello service bus namespace > hello service bus policy > **Create**.</span></span>

      ![Hozzon létre egy service bus-kapcsolat a logikai alkalmazásnak hello Azure-portálon](media/iot-hub-monitoring-notifications-with-azure-logic-apps/7_create-service-bus-connection-in-logic-app-azure-portal.png)

   1. <span data-ttu-id="66fc2-178">Kattintson a **Folytatás** hello service bus-kapcsolat létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="66fc2-178">Click **Continue** after hello service bus connection is created.</span></span>
   1. <span data-ttu-id="66fc2-179">Válassza ki a létrehozott hello várólista, és írja be `175` a **maximális üzenetek száma**</span><span class="sxs-lookup"><span data-stu-id="66fc2-179">Select hello queue that you created and enter `175` for **Maximum message count**</span></span>

      ![Adja meg a Logic Apps alkalmazást hello service bus-kapcsolat hello üzenetek maximális száma](media/iot-hub-monitoring-notifications-with-azure-logic-apps/8_specify-maximum-message-count-for-service-bus-connection-logic-app-azure-portal.png)
   1. <span data-ttu-id="66fc2-181">Kattintson a "Mentés" gombra toosave hello módosításokat.</span><span class="sxs-lookup"><span data-stu-id="66fc2-181">Click "Save" button toosave hello changes.</span></span>

1. <span data-ttu-id="66fc2-182">SMTP-szolgáltatás kapcsolat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="66fc2-182">Create an SMTP service connection.</span></span>
   1. <span data-ttu-id="66fc2-183">Kattintson a **új lépés** > **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="66fc2-183">Click **New step** > **Add an action**.</span></span>
   1. <span data-ttu-id="66fc2-184">Típus `SMTP`, kattintson a hello **SMTP** hello keresési eredmény szolgáltatásra, és kattintson a **SMTP - E-mail küldése**.</span><span class="sxs-lookup"><span data-stu-id="66fc2-184">Type `SMTP`, click hello **SMTP** service in hello search result, and then click **SMTP - Send Email**.</span></span>

      ![A Logic Apps alkalmazást az Azure-portálon hello egy SMTP-kapcsolat létrehozása](media/iot-hub-monitoring-notifications-with-azure-logic-apps/9_create-smtp-connection-logic-app-azure-portal.png)

   1. <span data-ttu-id="66fc2-186">Adja meg a postaláda hello SMTP-adatokat, és kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="66fc2-186">Enter hello SMTP information of your mailbox, and then click **Create**.</span></span>

      ![Adja meg a Logic Apps alkalmazást az Azure-portálon hello SMTP-kapcsolati adatok](media/iot-hub-monitoring-notifications-with-azure-logic-apps/10_enter-smtp-connection-info-logic-app-azure-portal.png)

      <span data-ttu-id="66fc2-188">Hello SMTP adatainak [Hotmail/Outlook.com](https://support.office.com/en-us/article/Add-your-Outlook-com-account-to-another-mail-app-73f3b178-0009-41ae-aab1-87b80fa94970), [Gmail](https://support.google.com/a/answer/176600?hl=en), és [Yahoo Mail](https://help.yahoo.com/kb/SLN4075.html).</span><span class="sxs-lookup"><span data-stu-id="66fc2-188">Get hello SMTP information for [Hotmail/Outlook.com](https://support.office.com/en-us/article/Add-your-Outlook-com-account-to-another-mail-app-73f3b178-0009-41ae-aab1-87b80fa94970), [Gmail](https://support.google.com/a/answer/176600?hl=en), and [Yahoo Mail](https://help.yahoo.com/kb/SLN4075.html).</span></span>
   1. <span data-ttu-id="66fc2-189">Adja meg az e-mail címet a **a** és **való**, és `High temperature detected` a **tulajdonos** és **törzs**.</span><span class="sxs-lookup"><span data-stu-id="66fc2-189">Enter your email address for **From** and **To**, and `High temperature detected` for **Subject** and **Body**.</span></span>
   1. <span data-ttu-id="66fc2-190">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="66fc2-190">Click **Save**.</span></span>

<span data-ttu-id="66fc2-191">hello logikai alkalmazás megfelelően működik történő mentésekor.</span><span class="sxs-lookup"><span data-stu-id="66fc2-191">hello logic app is in working order when you save it.</span></span>

## <a name="test-hello-logic-app"></a><span data-ttu-id="66fc2-192">Teszt hello logikai alkalmazás</span><span class="sxs-lookup"><span data-stu-id="66fc2-192">Test hello logic app</span></span>

1. <span data-ttu-id="66fc2-193">Indítsa el a tooyour eszköz üzembe helyezése hello ügyfélalkalmazás [csatlakozás ESP8266 tooAzure IoT-központ](iot-hub-arduino-huzzah-esp8266-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="66fc2-193">Start hello client application that you deploy tooyour device in [Connect ESP8266 tooAzure IoT Hub](iot-hub-arduino-huzzah-esp8266-get-started.md).</span></span>
1. <span data-ttu-id="66fc2-194">Hello környezet hőmérséklet körül fent 30 c hello SensorTag toobe növelése Például világos a gyertyák a SensorTag körül.</span><span class="sxs-lookup"><span data-stu-id="66fc2-194">Increase hello environment temperature around hello SensorTag toobe above 30 C. For example, light a candle around your SensorTag.</span></span>
1. <span data-ttu-id="66fc2-195">Hello logikai alkalmazás által küldött e-mailben értesítést kell kapnia.</span><span class="sxs-lookup"><span data-stu-id="66fc2-195">You should receive an email notification sent by hello logic app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="66fc2-196">Az e-mail-szolgáltató esetleg tooverify hello feladó identitása toomake róla, hogy Ön hello e-mailt küld.</span><span class="sxs-lookup"><span data-stu-id="66fc2-196">Your email service provider may need tooverify hello sender identity toomake sure it is you who sends hello email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="66fc2-197">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="66fc2-197">Next steps</span></span>

<span data-ttu-id="66fc2-198">Sikeresen létrehozta a logikai alkalmazás, amely összeköti az IoT hub és a postaláda hőmérséklet figyelési és értesítések.</span><span class="sxs-lookup"><span data-stu-id="66fc2-198">You have successfully created a logic app that connects your IoT hub and your mailbox for temperature monitoring and notifications.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
