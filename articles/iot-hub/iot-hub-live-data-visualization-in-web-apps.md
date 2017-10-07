---
title: "az érzékelő adatokat az Azure IoT hub – webalkalmazások aaaReal idejű adatok vizuális |} Microsoft Docs"
description: "Használja a Microsoft Azure App Service toovisualize hőmérséklet és a páratartalom adatok hello érzékelő gyűjtése történik, és az Iot-központ tooyour küldött hello Web Apps szolgáltatást."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "valós idejű adatok vizuális, az élő adatok vizuális érzékelő adatábrázolási"
ms.assetid: e42b07a8-ddd4-476e-9bfb-903d6b033e91
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/16/2017
ms.author: xshi
ms.openlocfilehash: 72f2dffee1c2f975948820eee9f2e287c3f77255
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-real-time-sensor-data-from-your-azure-iot-hub-by-using-hello-web-apps-feature-of-azure-app-service"></a>Az Azure IoT hub a valós idejű érzékelőadatok megjelenítése hello Azure App Service Web Apps szolgáltatása használatával

![Végpontok közötti diagramja](media/iot-hub-get-started-e2e-diagram/5.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Ismertetett témák

Ebben az oktatóanyagban elsajátíthatja, hogyan, amely az IoT hub fogad, amely a webes alkalmazás futtatásával toovisualize valós idejű érzékelőadatok üzemelteti a webes alkalmazás. Ha szeretné tootry toovisualize hello adatokat az IoT hub a Power BI használatával, lásd: [a Power BI toovisualize valós idejű érzékelőadatok Azure IoT hubról](iot-hub-live-data-visualization-in-power-bi.md).

## <a name="what-you-do"></a>Mit

- A webalkalmazás létrehozása az Azure-portálon hello.
- Felkészülés az IoT hub adatelérés egy felhasználói csoport hozzáadásával.
- Hello web app tooread érzékelő adatait az IoT hub konfigurálása.
- Töltse fel a webes alkalmazás toobe hello webalkalmazás üzemelteti.
- Nyissa meg az IoT hub hello web app toosee valós idejű hőmérséklet és a páratartalom adatokat.

## <a name="what-you-need"></a>Mi szükséges

- [Konfigurálja az eszközt](iot-hub-raspberry-pi-kit-node-get-started.md), amely hozzá van rendelve hello követelményeknek:
  - Aktív Azure-előfizetés
  - Az Iot-központ az előfizetéshez tartozó
  - Egy ügyfélalkalmazás által küldött üzenetek tooyour Iot-központ
- [Töltse le a Git](https://www.git-scm.com/downloads)

## <a name="create-a-web-app"></a>Webalkalmazás létrehozása

1. A hello [Azure-portálon](https://ms.portal.azure.com/), kattintson a **új** > **Web + mobil** > **webalkalmazás**.
2. Adja meg a feladat egyedi nevét, ellenőrizze a hello előfizetés, adjon meg egy erőforráscsoportot és helyet, jelölje be **PIN-kód toodashboard**, és kattintson a **létrehozása**.

   Azt javasoljuk, hogy kiválassza hello helyen, ahol az erőforráscsoportot. Ezzel segítséget nyújt a feldolgozási sebesség, és csökkenti a hello adatátvitel költségeit.

   ![Webalkalmazás létrehozása](media/iot-hub-live-data-visualization-in-web-apps/2_create-web-app-azure.png)

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="configure-hello-web-app-tooread-data-from-your-iot-hub"></a>Hello web app tooread adatokat az IoT hub konfigurálása

1. Nyissa meg az imént létesített hello webalkalmazás.
2. Kattintson a **Alkalmazásbeállítások**, majd az **Alkalmazásbeállítások**, adja hozzá a következő kulcs/érték párok hello:

   | Kulcs                                   | Érték                                                        |
   |---------------------------------------|--------------------------------------------------------------|
   | Azure.IoT.IoTHub.ConnectionString     | Az IOT hubbal-explorer kapott                                |
   | Azure.IoT.IoTHub.ConsumerGroup        | hello felhasználói csoport hozzáadása tooyour IoT-központ hello neve  |

   ![A kulcs/érték párok beállítások tooyour webalkalmazás hozzáadása](media/iot-hub-live-data-visualization-in-web-apps/4_web-app-settings-key-value-azure.png)

3. Kattintson a **Alkalmazásbeállítások**a **általános beállítások**, váltása hello **webes szoftvercsatornák** lehetőséget, majd kattintson a **mentése**.

   ![Webes szoftvercsatornák beállítást váltása hello](media/iot-hub-live-data-visualization-in-web-apps/10_toggle_web_sockets.png)

## <a name="upload-a-web-application-toobe-hosted-by-hello-web-app"></a>A webes alkalmazás toobe hello webalkalmazás által üzemeltetett feltöltése

A Githubon hajtottunk érhető el egy webes alkalmazás, amely az IoT hub valós idejű érzékelő adatait jeleníti meg. Toodo szüksége hello web app toowork állítson be egy Git-tárházat, a Githubról hello webes alkalmazás letöltése és a hello web app toohost tooAzure feltöltése.

1. Hello web app alkalmazásban kattintson **központi telepítési beállítások** > **forrás választása** > **helyi Git-tárház**, és kattintson a **OK**.

   ![A webes alkalmazás központi telepítési toouse hello helyi Git-tárház beállítása](media/iot-hub-live-data-visualization-in-web-apps/5_configure-web-app-deployment-local-git-repository-azure.png)

2. Kattintson a **üzembe helyezési hitelesítő adatok**, hozzon létre egy felhasználói nevet és jelszót toouse tooconnect toohello Git-tárházat az Azure-ban, és kattintson a **mentése**.

3. Kattintson a **áttekintése**, és jegyezze fel a hello értékének **Git-klón URL-címét**.

   ![Hello Git Klónozás webalkalmazás URL-CÍMÉT az beszerzése](media/iot-hub-live-data-visualization-in-web-apps/7_web-app-git-clone-url-azure.png)

4. Nyissa meg a parancsot vagy terminálablakot a helyi számítógépen.

5. Töltse le a hello webalkalmazás a Githubból, és töltse fel a hello web app toohost tooAzure. toodo Igen, futtassa a következő parancsok hello:

   ```bash
   git clone https://github.com/Azure-Samples/web-apps-node-iot-hub-data-visualization.git
   cd web-apps-node-iot-hub-data-visualization
   git remote add webapp <Git clone URL>
   git push webapp master:master
   ```

   > [!NOTE]
   > \<Git-klón URL-cím\> hello található hello Git-tárház URL-cím-hello **áttekintése** hello webalkalmazás oldalán.

## <a name="open-hello-web-app-toosee-real-time-temperature-and-humidity-data-from-your-iot-hub"></a>Az IoT hub hello webes alkalmazás toosee valós idejű hőmérséklet és a páratartalom adatainak megnyitása

A hello **áttekintése** lap webalkalmazás, kattintson a hello URL-cím tooopen hello webalkalmazás.

![A webalkalmazás hello URL-cím beszerzése](media/iot-hub-live-data-visualization-in-web-apps/8_web-app-url-azure.png)

Meg kell jelennie az IoT hub a hello valós idejű hőmérséklet és a páratartalom adatokat.

![Valós idejű hőmérséklet és a páratartalom bemutató alkalmazás weblap](media/iot-hub-live-data-visualization-in-web-apps/9_web-app-page-show-real-time-temperature-humidity-azure.png)

> [!NOTE]
> Ellenőrizze, hogy hello mintaalkalmazás fut-e az eszközön. Ha nem, üres diagram kap, olvassa el a toohello oktatóanyagok alatt [beállítani az eszközét](iot-hub-raspberry-pi-kit-node-get-started.md).

## <a name="next-steps"></a>Következő lépések
Használta a webes alkalmazás toovisualize valós idejű érzékelőadatok az IoT hub sikeres volt.

Egy Azure IoT Hub megadásának alternatív módja toovisualize adatokat, lásd: [a Power BI toovisualize valós idejű érzékelőadatok az IoT hub a](iot-hub-live-data-visualization-in-power-bi.md).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
