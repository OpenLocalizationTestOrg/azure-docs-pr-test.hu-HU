---
title: "aaaAzure IoT Suite – gyakori kérdések |} Microsoft Docs"
description: "Gyakran ismételt kérdések az IoT Suite-ról"
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: cb537749-a8a1-4e53-b3bf-f1b64a38188a
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2017
ms.author: corywink
ms.openlocfilehash: cb39e24af6d1ce2afea554285512d05b2d7c721e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-for-iot-suite"></a>Gyakran ismételt kérdések az IoT Suite-ról

Lásd még hello csatlakoztatott gyári specifikus [gyakran ismételt kérdések](iot-suite-faq-cf.md).

### <a name="where-can-i-find-hello-source-code-for-hello-preconfigured-solutions"></a>Hol találok hello forráskód előre konfigurált hello megoldások?

a következő GitHub-adattárak hello hello forráskód tárolja:
* [Távoli figyelési előkonfigurált megoldás][lnk-remote-monitoring-github]
* [Előre konfigurált prediktív karbantartási megoldás][lnk-predictive-maintenance-github]
* [Előre konfigurált csatlakoztatott gyári megoldás](https://github.com/Azure/azure-iot-connected-factory)

### <a name="how-do-i-update-toohello-latest-version-of-hello-remote-monitoring-preconfigured-solution-that-uses-hello-iot-hub-device-management-features"></a>Hogyan frissíthetők a toohello legújabb verziójának hello távoli figyelési előkonfigurált megoldás, hogy a használt hello IoT Hub eszköz kezelési funkciókat?

* Ha egy előre konfigurált megoldást hello https://www.azureiotsuite.com/ helyet telepít, mindig telepíti hello hello megoldás legújabb verziója egy új példányát.
* Ha telepít egy előre konfigurált megoldás hello parancssorból, frissítheti egy meglévő központi telepítési új kóddal. Lásd: [felhőalapú üzemelő példány] [ lnk-cloud-deployment] a Githubon hello [tárház][lnk-remote-monitoring-github].

### <a name="how-can-i-add-support-for-a-new-device-method-toohello-remote-monitoring-preconfigured-solution"></a>Hogyan vehetek fel egy új eszköz metódus toohello távoli felügyeleti előkonfigurált megoldás támogatását?

Hello című [egy új módszer toohello szimulátor támogatást] [ lnk-add-method] a hello [előkonfigurált megoldás testreszabása] [ lnk-customize] cikk.

### <a name="hello-simulated-device-is-ignoring-my-desired-property-changes-why"></a>hello szimulált eszköz figyelmen kívül hagyja a kívánt tulajdonságok módosítását, miért?
A hello távoli megfigyelési előre konfigurált megoldás, hello szimulált eszköz kód csak akkor használja, hello **Desired.Config.TemperatureMeanValue** és **Desired.Config.TelemetryInterval** szükségeskonfiguráció-tulajdonságok tooupdate hello jelentett tulajdonságait. Az összes többi kívánt tulajdonság változáskérések figyelmen kívül lesznek hagyva.

### <a name="my-device-does-not-appear-in-hello-list-of-devices-in-hello-solution-dashboard-why"></a>Az eszköz nem jelenik meg hello listán szereplő eszközök hello megoldás irányítópultjának, miért?

eszközök hello megoldás irányítópultjának hello listáját használja a lekérdezés tooreturn hello eszközök listáját. Jelenleg a lekérdezés nem adhat vissza több mint 10 KB-os eszközök. Próbáljon meg a lekérdezés hello keresési feltételeit szigorúbb.

### <a name="whats-hello-difference-between-deleting-a-resource-group-in-hello-azure-portal-and-clicking-delete-on-a-preconfigured-solution-in-azureiotsuitecom"></a>Mi az, hogy egy erőforráscsoportot az Azure portál és gombra kattintva törli a azureiotsuite.com előkonfigurált megoldást hello törlése hello különbségének?

* Ha törli az előre konfigurált hello megoldás [azureiotsuite.com][lnk-azureiotsuite], törli az összes hello erőforrásokat kiépített előre konfigurált hello megoldás létrehozása után. Ha további erőforrások toohello erőforráscsoport, ezeket az erőforrásokat is törlődnek. 
* Ha törli a hello erőforráscsoportja hello [Azure-portálon][lnk-azure-portal], csak törli az erőforráscsoport hello erőforrások. Szükség toodelete hello Azure Active Directory-alkalmazás társított előre konfigurált hello megoldás hello [a klasszikus Azure portálon][lnk-classic-portal].

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a>Az IoT-központ hány példányban hozhat létre az előfizetést?

Alapértelmezés szerint oszthat [10 IoT-központok előfizetésenként][link-azuresublimits]. Létrehozhat egy [az Azure támogatási jegy] [ link-azuresupportticket] tooraise ezt a határt. Ennek eredményeképpen minden előkonfigurált megoldás szánt új IoT-központ óta is csak másolatot too10 kiépítése előre konfigurált egy adott előfizetésben megoldások. 

### <a name="how-many-azure-cosmos-db-instances-can-i-provision-in-a-subscription"></a>Azure Cosmos DB hány példányban lehet kiépíteni egy előfizetésben?

Ötven. Létrehozhat egy [az Azure támogatási jegy] [ link-azuresupportticket] tooraise ez korlátozza, de alapértelmezés szerint csak oszthat előfizetésenként legfeljebb 50 Cosmos DB példányt. 

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Hány ingyenes Bing Térképek API-t adhatok meg egy előfizetésben?

Kettőt. Az Azure-előfizetéssel csak két belső tranzakciók szintjét 1 a Bing Maps vállalati terveket hozhat létre. hello távoli figyelési megoldást hello belső tranzakciók szintjét 1 csomaggal alapértelmezés szerint ki van építve. Ennek eredményeképpen csak oszthat tootwo távoli figyelési megoldásoknak a módosítás nélkül előfizetés mentése.

### <a name="i-have-a-remote-monitoring-solution-deployment-with-a-static-map-how-do-i-add-an-interactive-bing-map"></a>Statikus térképpel rendelkező távoli figyelő megoldásom van, hogyan adhatok hozzá interaktív Bing-térképet?

1. A Bing térképek API beszerzése a vállalati QueryKey [Azure-portálon][lnk-azure-portal]: 
   
   1. Keresse meg az erőforráscsoportot, ahol a Bing térképek API-t a vállalati megtalálható-e hello toohello [Azure-portálon][lnk-azure-portal].
   2. Kattintson a **összes beállítás**, majd **kulcskezelés**. 
   3. Két kulcsokat: **főkulcsos** és **QueryKey**. Másolja a hello értéke **QueryKey**.
      
      > [!NOTE]
      > Nem rendelkezik Vállalati Bing Térképek API-fiókkal? Hozzon létre egyet a hello [Azure-portálon] [ lnk-azure-portal] kattintva + új, a Bing térképek API-t a vállalati és kövesse a keresés kéri toocreate.
      > 
      > 
2. Húzza le a legfrissebb kódot hello hello [Azure IoT-távoli-ellenőrző][lnk-remote-monitoring-github].
3. Futtassa a helyi vagy felhőalapú üzemelő példány a következő hello parancssori üzembe helyezési útmutatót hello tárházban hello /docs/ mappában. 
4. Azt követően már futtatta a helyi vagy felhőalapú üzemelő példány, keresse meg a gyökérmappában hello a *. telepítése során létrehozott user.config fájlt. Nyissa meg ezt a fájlt egy szövegszerkesztőben. 
5. Változás hello következő sora tooinclude hello érték, amelyből másolta a **QueryKey**: 
   
   `<setting name="MapApiQueryKey" value="" />`

### <a name="can-i-create-a-preconfigured-solution-if-i-have-microsoft-azure-for-dreamspark"></a>Létrehozhatok előre konfigurált megoldást, ha Microsoft Azure for DreamSpark-fiókom van?

Jelenleg nem hozható létre egy előre konfigurált megoldást egy [dreamspark révén a Microsoft Azure] [ lnk-dreamspark] fiók. Azonban létrehozhat egy [ingyenes próba-fiókot az Azure-] [ lnk-30daytrial] mindössze néhány percet, amely lehetővé teszi az előkonfigurált megoldás létrehozása.

### <a name="can-i-create-a-preconfigured-solution-if-i-have-cloud-solution-provider-csp-subscription"></a>Létrehozhatók előkonfigurált megoldás, ha a Cloud Solution Provider (CSP) előfizetés van?

A Cloud Solution Provider (CSP) előfizetést jelenleg nem hozható létre előre konfigurált megoldást. Azonban létrehozhat egy [ingyenes próba-fiókot az Azure-] [ lnk-30daytrial] mindössze néhány percet, amely lehetővé teszi az előkonfigurált megoldás létrehozása.

### <a name="how-do-i-delete-an-aad-tenant"></a>Hogyan törölhetek egy AAD-bérlőt?

Eric Golpe blogbejegyzésből [bemutató az Azure AD-bérlő törlésével][lnk-delete-aad-tennant].

### <a name="next-steps"></a>Következő lépések

Akkor is is felfedezheti hello más szolgáltatásokat és képességeket hello előre konfigurált IoT Suite megoldások:

* [Előre konfigurált prediktív karbantartási megoldás áttekintése][lnk-predictive-overview]
* [Előre konfigurált csatlakoztatott gyári megoldási áttekintés](iot-suite-connected-factory-overview.md)
* [A hello IoT biztonsági szabad][lnk-security-groundup]

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-security-groundup]: securing-iot-ground-up.md

[link-azuresupportticket]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade 
[link-azuresublimits]: https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/#iot-hub-limits
[lnk-azure-portal]: https://portal.azure.com
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring 
[lnk-dreamspark]: https://www.dreamspark.com/Product/Product.aspx?productid=99 
[lnk-30daytrial]: https://azure.microsoft.com/free/
[lnk-delete-aad-tennant]: http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx
[lnk-cloud-deployment]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/cloud-deployment.md
[lnk-add-method]: iot-suite-guidance-on-customizing-preconfigured-solutions.md#add-support-for-a-new-method-to-the-simulator
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-predictive-maintenance-github]: https://github.com/Azure/azure-iot-predictive-maintenance
