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
# <a name="iot-remote-monitoring-and-notifications-with-azure-logic-apps-connecting-your-iot-hub-and-mailbox"></a>Az IoT távoli figyelés és az értesítések az Azure Logic Apps csatlakoztatása az IoT-központ és postaláda

![Végpontok közötti diagramja](media/iot-hub-get-started-e2e-diagram/7.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

Az Azure Logic Apps lehetőséget biztosít a lépések egy sorozatát tooautomate folyamatokat. Logikai alkalmazás különböző szolgáltatások és protokollok keresztül is elérheti. Megkezdi az eseményindító például a "Ha a fiók hozzáadása" és a műveletek, például "a leküldéses értesítések küldését" egyik kombinációja követ. A szolgáltatás elérhetővé teszi a Logic Apps tökéletes megoldás az IoT-megoldás az IoT figyelésére, ilyen például a riasztás a rendellenességek észlelését, más használati forgatókönyvek között marad.

## <a name="what-you-learn"></a>Ismertetett témák

Megismerheti, hogyan toocreate logikai alkalmazás, amely összeköti az IoT hub és a postaláda hőmérséklet figyelési és értesítések. Amikor hello hőmérséklet túllépi ezt az értéket 30 C, hello ügyfél alkalmazás jelek `temperatureAlert = "true"` hello üzenetet küld tooyour IoT-központ. Eseményindítók hello logic app toosend üdvözlőüzenetére, e-mailben értesítést.

## <a name="what-you-do"></a>Mit

* Service bus-névtér létrehozása, és adja hozzá a várólista tooit.
* Adja hozzá a végpont és útválasztási szabály tooyour IoT-központot.
* Hozzon létre, beállította és ellenőrizte a logikai alkalmazást.

## <a name="what-you-need"></a>Mi szükséges

* Az oktatóanyag [beállítani az eszközét](iot-hub-raspberry-pi-kit-node-get-started.md) fejeződött be, amely hozzá van rendelve hello követelményeknek:
  * Aktív Azure-előfizetés.
  * Az előfizetéshez tartozó Azure IoT hub.
  * Egy ügyfélalkalmazás által küldött üzenetek tooyour Azure IoT-központot.

## <a name="create-service-bus-namespace-and-add-a-queue-tooit"></a>Service bus-névtér létrehozása és a várólista tooit hozzáadása

### <a name="create-a-service-bus-namespace"></a>Service bus-névtér létrehozása

1. A hello [Azure-portálon](https://portal.azure.com/), kattintson a **új** > **vállalati integrációs** > **Service Bus**.
1. Adja meg a következő információ hello:

   **Név**: hello a service bus hello nevét.

   **A tarifacsomag**: kattintson a **alapvető** > **válasszon**. az alapszintű csomag hello is elegendő ehhez az oktatóanyaghoz.

   **Erőforráscsoport**: használata hello ugyanaz az IoT hub használó erőforráscsoportot.

   **Hely**: használata hello ugyanaz az IoT hub használó hely.
1. Kattintson a **Create** (Létrehozás) gombra.

   ![Hozzon létre egy service bus-névtér hello Azure-portálon](media/iot-hub-monitoring-notifications-with-azure-logic-apps/1_create-service-bus-namespace-azure-portal.png)

### <a name="add-a-service-bus-queue"></a>Adja hozzá a service bus-üzenetsorba

1. Nyissa meg a service bus-névtér hello, és kattintson a **+ várólista**.
1. Adja meg a hello várólista nevét, és kattintson a **létrehozása**.
1. Nyissa meg a hello service bus-üzenetsorba, és kattintson a **megosztott elérési házirendek** > **+ Hozzáadás**.
1. Adjon meg egy nevet hello házirend, ellenőrzés **kezelése**, és kattintson a **létrehozása**.

   ![Adja hozzá a service bus-üzenetsorba hello Azure-portálon](media/iot-hub-monitoring-notifications-with-azure-logic-apps/2_add-service-bus-queue-azure-portal.png)

## <a name="add-an-endpoint-and-a-routing-rule-tooyour-iot-hub"></a>Adja hozzá a végpont és útválasztási szabály tooyour IoT-központ

### <a name="add-an-endpoint"></a>Végpont hozzáadása

1. Nyissa meg az IoT hub, kattintson a végpontok > + Hozzáadás.
1. Adja meg a következő információ hello:

   **Név**: hello hello végpont nevét.

   **Típusú végpont**: válasszon **Service Bus-üzenetsorba**.

   **Service Bus-névtér**: válassza ki a létrehozott hello névtér.

   **Service Bus-üzenetsorba**: Select hello létrehozott üzenetsorba.
1. Kattintson az **OK** gombra.

   ![Adja hozzá az végpont tooyour IoT-központ hello Azure-portálon](media/iot-hub-monitoring-notifications-with-azure-logic-apps/3_add-iot-hub-endpoint-azure-portal.png)

### <a name="add-a-routing-rule"></a>Útválasztási szabály hozzáadása

1. Kattintson az IoT hub **útvonalak** > **+ Hozzáadás**.
1. Adja meg a következő információ hello:

   **Név**: hello hello útválasztási szabály nevét.

   **Az adatforrás**: válasszon **DeviceMessages**.

   **Végpont**: válassza ki a létrehozott hello végpont.

   **Lekérdezési karakterlánc**: Adjon meg `temperatureAlert = "true"`.
1. Kattintson a **Save** (Mentés) gombra.

   ![Az Azure-portálon hello egy útválasztási szabály hozzáadása](media/iot-hub-monitoring-notifications-with-azure-logic-apps/4_add-routing-rule-azure-portal.png)

## <a name="create-and-configure-a-logic-app"></a>Hozza létre és konfigurálja a logikai alkalmazás

### <a name="create-a-logic-app"></a>Logikai alkalmazás létrehozása

1. A hello [Azure-portálon](https://portal.azure.com/), kattintson a **új** > **vállalati integrációs** > **logikai alkalmazás**.
1. Adja meg a következő információ hello:

   **Név**: hello logikai alkalmazás hello nevét.

   **Erőforráscsoport**: használata hello ugyanaz az IoT hub használó erőforráscsoportot.

   **Hely**: használata hello ugyanaz az IoT hub használó hely.
1. Kattintson a **Create** (Létrehozás) gombra.

### <a name="configure-hello-logic-app"></a>Hello logikai alkalmazás konfigurálása

1. Nyissa meg a Logic Apps Designer hello megnyíló hello logikai alkalmazás.
1. A Logic Apps Designer hello, kattintson **üres logikai alkalmazás**.

   ![Egy üres logikai alkalmazást az Azure-portálon hello kezdődnie](media/iot-hub-monitoring-notifications-with-azure-logic-apps/5_start-with-blank-logic-app-azure-portal.png)

1. Kattintson a **Service Bus**.

   ![Válassza ki a Service Bus toostart hello Azure-portálon a logikai alkalmazás létrehozása](media/iot-hub-monitoring-notifications-with-azure-logic-apps/6_select-service-bus-when-creating-blank-logic-app-azure-portal.png)

1. Kattintson a **Service Bus – egy vagy több üzenet a várólistában egyszerre várakozó (automatikusan hajthat végre) érkezésekor**.
1. Hozzon létre egy service bus-kapcsolat.
   1. Adja meg a kapcsolat neve.
   1. Kattintson a service bus-névtér hello > service bus házirend hello > **létrehozása**.

      ![Hozzon létre egy service bus-kapcsolat a logikai alkalmazásnak hello Azure-portálon](media/iot-hub-monitoring-notifications-with-azure-logic-apps/7_create-service-bus-connection-in-logic-app-azure-portal.png)

   1. Kattintson a **Folytatás** hello service bus-kapcsolat létrehozása után.
   1. Válassza ki a létrehozott hello várólista, és írja be `175` a **maximális üzenetek száma**

      ![Adja meg a Logic Apps alkalmazást hello service bus-kapcsolat hello üzenetek maximális száma](media/iot-hub-monitoring-notifications-with-azure-logic-apps/8_specify-maximum-message-count-for-service-bus-connection-logic-app-azure-portal.png)
   1. Kattintson a "Mentés" gombra toosave hello módosításokat.

1. SMTP-szolgáltatás kapcsolat létrehozásához.
   1. Kattintson a **új lépés** > **művelet hozzáadása**.
   1. Típus `SMTP`, kattintson a hello **SMTP** hello keresési eredmény szolgáltatásra, és kattintson a **SMTP - E-mail küldése**.

      ![A Logic Apps alkalmazást az Azure-portálon hello egy SMTP-kapcsolat létrehozása](media/iot-hub-monitoring-notifications-with-azure-logic-apps/9_create-smtp-connection-logic-app-azure-portal.png)

   1. Adja meg a postaláda hello SMTP-adatokat, és kattintson **létrehozása**.

      ![Adja meg a Logic Apps alkalmazást az Azure-portálon hello SMTP-kapcsolati adatok](media/iot-hub-monitoring-notifications-with-azure-logic-apps/10_enter-smtp-connection-info-logic-app-azure-portal.png)

      Hello SMTP adatainak [Hotmail/Outlook.com](https://support.office.com/en-us/article/Add-your-Outlook-com-account-to-another-mail-app-73f3b178-0009-41ae-aab1-87b80fa94970), [Gmail](https://support.google.com/a/answer/176600?hl=en), és [Yahoo Mail](https://help.yahoo.com/kb/SLN4075.html).
   1. Adja meg az e-mail címet a **a** és **való**, és `High temperature detected` a **tulajdonos** és **törzs**.
   1. Kattintson a **Save** (Mentés) gombra.

hello logikai alkalmazás megfelelően működik történő mentésekor.

## <a name="test-hello-logic-app"></a>Teszt hello logikai alkalmazás

1. Indítsa el a tooyour eszköz üzembe helyezése hello ügyfélalkalmazás [csatlakozás ESP8266 tooAzure IoT-központ](iot-hub-arduino-huzzah-esp8266-get-started.md).
1. Hello környezet hőmérséklet körül fent 30 c hello SensorTag toobe növelése Például világos a gyertyák a SensorTag körül.
1. Hello logikai alkalmazás által küldött e-mailben értesítést kell kapnia.

   > [!NOTE]
   > Az e-mail-szolgáltató esetleg tooverify hello feladó identitása toomake róla, hogy Ön hello e-mailt küld.

## <a name="next-steps"></a>Következő lépések

Sikeresen létrehozta a logikai alkalmazás, amely összeköti az IoT hub és a postaláda hőmérséklet figyelési és értesítések.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
