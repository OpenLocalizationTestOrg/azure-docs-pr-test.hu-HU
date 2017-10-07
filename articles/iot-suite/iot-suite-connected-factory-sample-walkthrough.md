---
title: "aaaConnected gyári Azure IoT Suite megoldás forgatókönyv |} Microsoft Docs"
description: "Hello előre konfigurált Azure IoT-megoldás leírása factory és az architektúra csatlakoztatva."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 31fe13af-0482-47be-b4c8-e98e36625855
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2017
ms.author: dobett
ms.openlocfilehash: 7fd55c51351659401349cfde91a20fce1045b4f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connected-factory-preconfigured-solution-walkthrough"></a>Előre konfigurált csatlakoztatott gyár megoldás – bemutató

hello IoT Suite csatlakoztatott gyári [előre konfigurált megoldás] [ lnk-preconfigured-solutions] egy végpontok közötti ipari megoldás megvalósítása, amely:

* Szimulált tooboth ipari eszközökön futó OPC EE-kiszolgálók szimulált gyári éles sorok és valós OPC EE-kiszolgálóeszközökkel a kapcsolatot. OPC EE kapcsolatos további információkért lásd: hello [gyári gyakran ismételt kérdések csatlakoztatott](iot-suite-faq-cf.md).
* Megjeleníti ezen eszközök és gyártósorok működési KPI- és OEE-értékeit.
* Bemutatja, hogyan egy felhőalapú alkalmazás OPC EE server rendszerrel használt toointeract lehet-e.
* Lehetővé teszi, hogy Ön tooconnect saját OPC EE kiszolgálóeszközökkel.
* Lehetővé teszi a toobrowse és hello OPC EE-kiszolgáló adatainak módosítása.
* Integrálható a hello Azure idő adatsorozat Insights (ÁME) szolgáltatás tooprovide egyéni nézetek hello adatok a OPC EE-kiszolgálókról.

Használhat hello megoldás kiindulási pontként a saját megvalósítási és [testreszabása] [ lnk-customize] azt toomeet saját speciális üzleti igényeinek.

Ez a cikk bemutatja, hogyan néhány hello fontosabb elemei csatlakozó hello gyári megoldás tooenable toounderstand, hogyan működik. Ezeknek az ismereteknek a birtokában:

* Problémamegoldás hello megoldásban.
* Tervezze meg hogyan toocustomize toohello megoldás toomeet a saját konkrét követelmények.
* Kialakíthatja saját, Azure-szolgáltatásokat használó IoT-megoldását.

További információkért lásd: hello [gyári gyakran ismételt kérdések csatlakoztatott](iot-suite-faq-cf.md).

## <a name="logical-architecture"></a>Logikai architektúra

a következő diagram hello hello logikai összetevőinek előre konfigurált hello megoldás ismerteti:

![Csatlakoztatott gyár logikai architektúrája][connected-factory-logical]

## <a name="communication-patterns"></a>Kommunikációs minták

hello megoldás az hello [OPC EE Pub/Sub specification](https://opcfoundation.org/news/opc-foundation-news/opc-foundation-announces-support-of-publish-subscribe-for-opc-ua/) toosend OPC EE telemetriai adatok tooIoT központ JSON formátumban. hello megoldás az hello [OPC Publisher](https://github.com/Azure/iot-edge-opc-publisher) IoT peremhálózati modul erre a célra.

hello megoldás is rendelkezik egy OPC EE-ügyfél integrálva egy webes alkalmazás, amely képes kapcsolatot létrehozni a helyszíni OPC EE-kiszolgálókkal. hello ügyfél használ egy [fordított proxy](https://wikipedia.org/wiki/Reverse_proxy) és súgó fogad az IoT-központ toomake hello kapcsolat hello helyszíni tűzfal portjainak megnyitása nélkül. Ezt a kommunikációs mintát [szolgáltatás által támogatott kommunikációnak](https://blogs.msdn.microsoft.com/clemensv/2014/02/09/service-assisted-communication-for-connected-devices/) hívják. hello megoldás az hello [OPC Proxy](https://github.com/Azure/iot-edge-opc-proxy/) IoT peremhálózati modul erre a célra.


## <a name="simulation"></a>Szimuláció

hello szimulált állomások és a szimulált hello gyártási végrehajtási rendszerek (MES) jött létre a gyári termelési sor. hello szimulált eszközök és hello OPC Publisher modul alapuló hello [OPC EE .NET szabványos] [ lnk-OPC-UA-NET-Standard] hello OPC Foundation tett közzé.

hello OPC Proxy és OPC szoftvergyártó valósíthatók alapján modulok [Azure IoT peremhálózati][lnk-Azure-IoT-Gateway]. Mindegyik szimulált gyártósorhoz kijelölt átjáró van csatlakoztatva.

Minden szimulációs összetevő Azure Linux virtuális gépen futtatott Docker-tárolókban fut. hello szimuláció konfigurált toorun nyolc szimulált éles sorok alapértelmezés szerint.

## <a name="simulated-production-line"></a>Szimulált gyártósor

A gyártósorok alkatrészeket gyártanak. Különböző állomásokból állnak: összeszerelő állomásból, tesztelő állomásból és csomagoló állomásból.

hello szimuláció fut, és frissíti a hello adatok hello OPC EE csomóponton keresztül elérhetővé. Az összes szimulált termelési sor állomás hello MES keresztül OPC EE által összehangolva. vannak.

## <a name="simulated-manufacturing-execution-system"></a>Szimulált gyártási végrehajtó rendszer

hello MES figyeli minden állomáshoz hello termelési sorban OPC EE toodetect állomás állapotmódosítások keresztül. Meghívja a OPC EE módszerek toocontrol hello állomás, és adja át a termék egy állomás toohello a következő mindaddig, amíg nem fejeződik be.

## <a name="gateway-opc-publisher-module"></a>Átjáró OPC kiadói modul

OPC Publisher modul csatlakoztatja toohello állomás OPC EE-kiszolgálókat, és előfizet toohello OPC csomópontok toobe közzé. hello modul hello csomópont adatait JSON formátumba konvertálja, titkosítja azokat, és elküldi azt OPC EE Pub/Sub üzenetekként tooIoT központ.

hello OPC Publisher modul csak egy kimenő https (443) portra van szüksége, és a meglévő vállalati infrastruktúra is használhatók.

## <a name="gateway-opc-proxy-module"></a>Átjáró OPC-proxymodul

hello átjáró OPC EE Proxy modul bújtatja bináris OPC EE parancs és a vezérlő üzeneteket, és csak igényel az egy kimenő https (443-as) porton. Képes a meglévő vállalati infrastruktúrát használni, beleértve a webes proxykat is.

Az IoT Hub eszközhöz módszerek tootransfer packetized TCP/IP-adatok hello alkalmazás rétegben használ, és így biztosítja a végpont megbízható, az adattitkosítás és az SSL/TLS használatával integritása.

hello OPC EE bináris protokoll maga hello proxyn keresztül történő továbbítását EE-hitelesítést és titkosítást használ.

## <a name="azure-time-series-insights"></a>Azure Time Series Insights

Átjáró OPC Publisher modul hello előfizet tooOPC EE server csomópontok toodetect változása hello adatértékekkel. Ha egy adatok valamelyik hello csomópontokat észlelt, ez a modul ezután elküldi üzenetek tooAzure IoT-központot.

IoT-központot egy esemény forrás tooAzure ÁME biztosít. ÁME tárolók adatok alapján időbélyegeket 30 napig csatolt toohello üzeneteket. A adatok tartalma:

* OPC UA ApplicationUri
* OPC UA NodeId
* Hello csomópont értéke
* Forrás időbélyege
* OPC UA DisplayName

Jelenleg ÁME nem teszi lehetővé az ügyfelek mennyi ideig toocustomize kívánják tookeep hello adatait.

A TSI a csomópontadatok lekérdezését egy SearchSpan (Time.From, Time.To), az összesítést pedig az OPC UA ApplicationUri, az OPC UA NodeId vagy az OPC UA DisplayName attribútum alapján végzi.

tooretrieve hello adatok hello OEE és KPI mérőműszer, és az adatsorozat hello idődiagramokat, adatok összesítve van az események, Sum, Avg, minimális és maximális számát.

hello a time series egy másik folyamat használatával készített. Állomás alap adatokból számított és hello alkalmazás feliratkozott hello topológia (éles sorok, előállítók, vállalati) átbuborékoltatni OEE és a KPI-k.

Emellett OEE és a KPI-topológia a time series kiszámítása hello alkalmazásban, amikor készen áll a megjelenített timespan érték. Például a hello mai nézete teljes óránként frissül.

hello idő adatsorozat nézet csomópont adatok közvetlenül a összesítést használ timespan ÁME származik.

## <a name="iot-hub"></a>IoT Hub
Hello [IoT-központ] [ lnk-IoT Hub] hello OPC Publisher modul hello felhőbe küldött adatokat fogad, és lehetővé teszi az elérhető toohello Azure ÁME szolgáltatás. 

az IoT-központ hello hello megoldásban is:
- Kezeli az identitásjegyzékhez hello azonosítók tároló összes OPC Publisher modult és minden OPC Proxy modul.
- Használható szállítási csatorna hello OPC Proxy modul közötti kommunikáció kétirányú.

## <a name="azure-storage"></a>Azure Storage
hello megoldás használ az Azure blob-tároló lemez tárolóként hello VM és toostore központi telepítésével kapcsolatos adatokat.

## <a name="web-app"></a>Webalkalmazás
hello webalkalmazás üzembe az előre konfigurált hello megoldás részét képező egy integrált OPC EE-ügyfél, a riasztások feldolgozása és a telemetriai adatokat a képi megjelenítés foglalja magában.

## <a name="next-steps"></a>Következő lépések

Továbbra is a következő cikkek hello olvasásával Ismerkedés az IoT Suite:

* [Engedélyek hello azureiotsuite.com webhelyen][lnk-permissions]
* [Telepítsen egy átjárót a Windows vagy Linux csatlakoztatott hello előre konfigurált gyári megoldás](iot-suite-connected-factory-gateway-deployment.md)

[connected-factory-logical]:media/iot-suite-connected-factory-walkthrough/cf-logical-architecture.png

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-IoT Hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-OPC-UA-NET-Standard]:https://github.com/OPCFoundation/UA-.NETStandardLibrary
[lnk-Azure-IoT-Gateway]: https://github.com/azure/iot-edge
[lnk-permissions]: iot-suite-permissions.md
