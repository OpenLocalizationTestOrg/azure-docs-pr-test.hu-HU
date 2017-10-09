---
title: "aaaAzure IoT Suite és a Logic Apps |} Microsoft Docs"
description: "Egy útmutatót a Logic Apps tooAzure IoT Suite üzleti folyamat másolatot toohook."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 4629a7af-56ca-4b21-a769-5fa18bc3ab07
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: corywink
ms.openlocfilehash: 6ef7311ac38f4e2ddb032cff0fb73591da5f76c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-connect-logic-app-tooyour-azure-iot-suite-remote-monitoring-preconfigured-solution"></a>Oktatóanyag: Logikai alkalmazás tooyour Azure IoT Suite távoli figyelésére szolgáló előre konfigurált megoldás csatlakozás
Hello [Microsoft Azure IoT Suite] [ lnk-internetofthings] távoli felügyeleti előkonfigurált megoldás egy kiváló módja tooget parancsot gyorsan egy végpont készlet, amely egy IoT-megoldás exemplifies. Ez az oktatóanyag bemutatja, hogyan hogyan tooadd logikai alkalmazás tooyour Microsoft Azure IoT Suite távoli megfigyelési előre konfigurált a megoldás. Ezen lépések azt ismertetik, hogyan készíthet az IoT-megoldásból még tovább tooa üzleti folyamat csatlakoztatásával.

*Ha egy általános bemutató hogyan egy távoli megfigyelési tooprovision előre konfigurált megoldást keres, tekintse meg [oktatóanyag: Ismerkedés a hello előre konfigurált IoT-megoldások][lnk-getstarted].*

Ez az oktatóanyag megkezdése előtt a következőket:

* Kiépítés hello távoli felügyeleti előkonfigurált megoldás az Azure-előfizetése.
* A SendGrid fiók tooenable hozzon létre egy e-mailt, amely elindítja az üzleti folyamat toosend. Regisztrálhat egy ingyenes próbafiók, a [SendGrid](https://sendgrid.com/) kattintva **próbálja szabad**. A próbafiókot regisztrálása után toocreate kell egy [API-kulcs](https://sendgrid.com/docs/User_Guide/Settings/api_keys.html) a SendGrid, amely engedélyt toosend mail. Az API-kulcs hello oktatóanyag későbbi részében szüksége.

toocomplete ebben az oktatóanyagban Visual Studio 2015-öt vagy a Visual Studio 2017 toomodify hello műveletek hello előkonfigurált megoldás háttérrendszerének van szüksége.

Feltéve, hogy már megtörtént a távoli figyelésének előre konfigurált megoldást, keresse meg az adott megoldás hello toohello erőforráscsoport [Azure-portálon][lnk-azureportal]. hello erőforráscsoport rendelkezik azonos hello megoldás nevét, nevet hello döntött, hogy amikor a távoli felügyeleti megoldás létesített. Hello erőforráscsoportban láthatja, minden hello kiépített Azure-erőforrások kivételével hello Azure Active Directory-alkalmazás, amely megtalálható a klasszikus Azure portál hello megoldást. hello alábbi képernyőfelvételen szereplő példán látható **erőforráscsoport** egy távoli figyelés panel előre konfigurált megoldást:

![](media/iot-suite-logic-apps-tutorial/resourcegroup.png)

toobegin, hello logic app toouse a hello beállítása előre konfigurált megoldás.

## <a name="set-up-hello-logic-app"></a>Hello logikai alkalmazás beállítása
1. Kattintson a **Hozzáadás** az erőforráscsoport panel az Azure-portálon hello hello tetején.
2. Keresse meg **logikai alkalmazás**, válassza ki azt, és kattintson a **létrehozása**.
3. Töltse ki a hello **neve** és használja az azonos hello **előfizetés** és **erőforráscsoport** használja, ha a távoli felügyeleti megoldás létesített. Kattintson a **Create** (Létrehozás) gombra.
   
    ![](media/iot-suite-logic-apps-tutorial/createlogicapp.png)
4. A telepítés befejezését követően az erőforráscsoportban hello logikai alkalmazás szerepel a listán látható erőforrásként.
5. Kattintson a hello logikai alkalmazás toonavigate toohello logikai alkalmazás paneljén válassza hello **üres logikai alkalmazás** sablon tooopen hello **Logic Apps Designer**.
   
    ![](media/iot-suite-logic-apps-tutorial/logicappsdesigner.png)
6. Válassza ki **kérelem**. Ez a művelet határozza meg, hogy egy adott JSON azonosítójú bejövő HTTP-kérelem hasznos tevékenységéért formázva eseményindítót.
7. Illessze be a következő kódot a kérelem törzsében JSON-séma hello hello:
   
    ```json
    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "id": "/",
      "properties": {
        "DeviceId": {
          "id": "DeviceId",
          "type": "string"
        },
        "measuredValue": {
          "id": "measuredValue",
          "type": "integer"
        },
        "measurementName": {
          "id": "measurementName",
          "type": "string"
        }
      },
      "required": [
        "DeviceId",
        "measurementName",
        "measuredValue"
      ],
      "type": "object"
    }
    ```
   
   > [!NOTE]
   > Hello HTTP post hello URL-címe másolhatja után mentenie hello logikai alkalmazás, de először fel kell venni egy műveletet.
   > 
   > 
8. Kattintson a **+ új lépés** a kézi indítási alatt. Kattintson a **művelet hozzáadása**.
   
    ![](media/iot-suite-logic-apps-tutorial/logicappcode.png)
9. Keresse meg **SendGrid - e-mail küldése** , és kattintson rá.
   
    ![](media/iot-suite-logic-apps-tutorial/logicappaction.png)
10. Adja meg például hello kapcsolat nevét **SendGridConnection**, adja meg a hello **SendGrid API-kulcs** alapteljesítményhez képest, a SendGrid fiók beállítását, majd kattintson az **létrehozása**.
    
    ![](media/iot-suite-logic-apps-tutorial/sendgridconnection.png)
11. Adja hozzá a e-mail címek saját tooboth hello **a** és **való** mezőket. Adja hozzá **távoli figyelési figyelmeztetés [DeviceId]** toohello **tulajdonos** mező. A hello **E-mail törzsének** mezőbe hozzáadása **[DeviceId] jelzett [measurementName] [measuredValue] értékű.** Hozzáadhat **[DeviceId]**, **[measurementName]**, és **[measuredValue]** hello elemre kattintva **adatok beszúrásához előző lépéseiből**szakasz.
    
    ![](media/iot-suite-logic-apps-tutorial/sendgridaction.png)
12. Kattintson a **mentése** hello felső menüjében.
13. Hello kattintson **kérelem** eseményindítója és másolási hello **Http Post toothis URL-cím** érték. Az oktatóanyag későbbi részében szüksége az URL-cím.

> [!NOTE]
> A Logic Apps lehetővé teszik a toorun [művelet számos különböző típusú] [ lnk-logic-apps-actions] műveletek beleértve az Office 365-ben. 
> 
> 

## <a name="set-up-hello-eventprocessor-web-job"></a>Hello EventProcessor webes projekt beállítása
Ebben a szakaszban az előkonfigurált megoldás toohello létrehozott logikai alkalmazás csatlakozzon. toocomplete ebben a feladatban hozzáadása hello URL-tootrigger hello logikai alkalmazás toohello művelet, amely akkor következik be, ha egy eszköz érzékelő érték meghaladja a küszöbértéket.

1. A git ügyfél tooclone hello legújabb verzióját használja hello [azure iot-távoli-ellenőrző github-tárházban][lnk-rmgithub]. Példa:
   
    ```cmd
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```
2. A Visual Studióban nyissa meg a hello **RemoteMonitoring.sln** hello hello tárház helyi másolatát.
3. Nyissa meg hello **ActionRepository.cs** hello fájlban **infrastruktúra\\tárház** mappát.
4. Frissítés hello **actionIds** hello szótár **Http Post toothis URL-cím** , itt megjegyezni a Logic Apps alkalmazást az alábbiak szerint:
   
    ```csharp
    private Dictionary<string,string> actionIds = new Dictionary<string, string>()
    {
        { "Send Message", "<Http Post toothis URL>" },
        { "Raise Alarm", "<Http Post toothis URL>" }
    };
    ```
5. Hello módosítások mentése a megoldásban, és zárja be a Visual Studio.

## <a name="deploy-from-hello-command-line"></a>Hello parancssorból telepítése
Ebben a szakaszban központi telepítése a frissített verzió hello távoli figyelési megoldás tooreplace hello verziója fut az Azure-ban.

1. Következő hello [fejlesztői beállításról] [ lnk-devsetup] utasításokat tooset telepítési környezet.
2. toodeploy helyileg, hajtsa végre a hello [helyi telepítés] [ lnk-localdeploy] utasításokat.
3. toodeploy toohello a felhő és a meglévő felhőalapú telepítés frissítéséhez hajtsa végre az hello [felhőalapú üzemelő példány] [ lnk-clouddeploy] utasításokat. Használja az eredeti telepítés hello nevét hello üzemelő példány neve. Például ha az eredeti telepítési hello hívták **demologicapp**, a következő parancs hello használata:
   
   ```cmd
   build.cmd cloud release demologicapp
   ```
   
   Hello összeállítása a parancsfájl futtatása, ha mindenképpen toouse hello azonos Azure-fiók, előfizetés, valamint régió és hello megoldás létesített használt Active Directory-példányban.

## <a name="see-your-logic-app-in-action"></a>Tekintse meg a Logic Apps alkalmazást működés közben
távoli felügyeleti előkonfigurált megoldás hello beállítása alapértelmezés szerint, ha a megoldás két szabályokkal rendelkeznek. Szabályokat is vannak a hello **SampleDevice001** eszköz:

* Hőmérséklet > 38.00
* Páratartalom > 48.00

hello hőmérséklet szabály indítja hello **előléptetése riasztás** művelet és hello páratartalom szabály indítja hello **SendMessage** művelet. Feltéve, hogy a használt hello mindkét műveletek hello azonos URL-címe **ActionRepository** osztály, vagy a szabály a logic app eseményindítók. Szabályokat is használja a SendGrid toosend egy e-mailek toohello **való** cím hello riasztás részleteit.

> [!NOTE]
> hello logikai alkalmazás tootrigger továbbra is, minden alkalommal, amikor hello küszöbérték elérése. tooavoid szükségtelen e-mailek, vagy letilthatja hello szabályok a megoldás portal vagy letilthatja az hello logikai alkalmazást a hello a [Azure-portálon][lnk-azureportal].
> 
> 

Ezenkívül tooreceiving e-mailt is látható hello portálon hello logikai alkalmazás futtatásakor:

![](media/iot-suite-logic-apps-tutorial/logicapprun.png)

## <a name="next-steps"></a>Következő lépések
Most, hogy már használta a Logic App tooconnect előre konfigurált hello megoldás tooa üzleti folyamatokat, további kapcsolatos előre konfigurált hello megoldások testreszabásának hello beállításai:

* [Dinamikus telemetriai adatok használata a távoli felügyeleti előkonfigurált megoldás hello][lnk-dynamic]
* [Eszköz információk metaadatait a távoli felügyeleti előkonfigurált megoldás hello][lnk-devinfo]

[lnk-dynamic]: iot-suite-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[lnk-internetofthings]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-getstarted]: iot-suite-getstarted-preconfigured-solutions.md
[lnk-azureportal]: https://portal.azure.com
[lnk-logic-apps-actions]: ../connectors/apis-list.md
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devsetup]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/dev-setup.md
[lnk-localdeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/local-deployment.md
[lnk-clouddeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/cloud-deployment.md
