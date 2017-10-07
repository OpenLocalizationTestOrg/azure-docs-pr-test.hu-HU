---
title: "aaaMicrosoft Azure IoT Suite – áttekintés |} Microsoft Docs"
description: "Hogyan nyújt az Azure IoT Suite a dolgot előkonfigurált megoldásokat toocollect, az eszközök internetes áttekintése elemzése, és adatok tárolására, adja meg a képi megjelenítések és integrálása más rendszerekkel."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 2d38d08a-4133-4e5c-8b28-f93cadb5df05
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 385025c5ec0d37c74689a928bc09e85b33439634
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-azure-iot-suite"></a>Az Azure IoT Suite áttekintése

hello Azure internetes hálózata (IoT) szolgáltatások széles skálájával képességeket kínálnak. Ezekkel a vállalati szintű szolgáltatásokkal a következőket teheti:

* Adatok gyűjtése eszközökről
* Streamek elemzése mozgás közben
* Nagy adatkészletek tárolása és lekérdezése
* Valós idejű és előzményadatok megjelenítése
* Integrálás irodai háttérrendszerekkel
* Eszközök kezelése

toodeliver ezeket a képességeket, Azure IoT Suite csomagok egyszerre több Azure-szolgáltatások, az egyéni kiterjesztéseket tartalmazó *előre konfigurált megoldások*. Ezek előkonfigurált megoldásokat olyan alap megvalósítások annak a közös IoT megoldást, amelyek segítenek tooreduce hello időt, toodeliver az IoT-megoldásokhoz. Hello segítségével [IoT szoftverfejlesztői készletek][lnk-sdks], testre szabhatja, és ezek a megoldások toomeet kiterjesztése a saját követelményeinek megfelelően. Ezeket a megoldásokat példaként vagy sablonként is használhatja az új IoT-megoldások fejlesztésekor.

hello következő videó biztosít egy bevezető tooAzure IoT Suite:

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON309/player]
> 
> 

## <a name="azure-iot-services-in-azure-iot-suite"></a>Azure IoT-szolgáltatások az Azure IoT Suite-ban
előre konfigurált hello megoldások általában a következő szolgáltatások hello használata:

* Alapvető tooAzure IoT Suite hello [Azure IoT Hub] [ lnk-iot-hub] szolgáltatás. Ez a szolgáltatás hello eszközről-a-felhőbe és felhő eszközre üzenetkezelési képességeket és tevékenységéért átjáró toohello hello, a felhő és egyéb fontos IoT Suite szolgáltatások hello. hello szolgáltatás lehetővé teszi az eszközök léptékű tooreceive üzeneteit, és parancsok tooyour eszközök küldése. hello szolgáltatás lehetővé teszi túl[az eszközök kezeléséhez][lnk-device-management]. Például konfigurálása, indítsa újra, vagy hajtsa végre a gyári beállításokat egy vagy több eszköz csatlakoztatott toohello központ.
* Az [Azure Stream Analytics][lnk-asa] a mozgásban lévő adatok elemzését nyújtja. IoT Suite használja a szolgáltatás tooprocess bejövő telemetriai, hajtsa végre az összesítő és események észleléséhez. előre konfigurált hello megoldások is használja a stream analytics tooprocess tájékoztató üzeneteket adatok, például az eszközök metaadataira, illetve a parancs válaszát tartalmazó. hello megoldások használja a Stream Analytics tooprocess köszönőüzenetei az eszközökről, és az üzenetek tooother szolgáltatások biztosításához.
* [Az Azure Storage] [ lnk-azure-storage] és [Azure Cosmos DB] [ lnk-document-db] hello adatok tárolási lehetőségeket biztosít. hello előkonfigurált megoldásokat használja a blob storage toostore telemetriai és toomake elérhetővé elemzés céljából. hello megoldások Cosmos DB toostore eszköz metaadatait és hello Eszközkezelési lehetőségeivel foglalkozó hello megoldások engedélyezheti.
* [Az Azure Web Apps] [ lnk-web-apps] és [Microsoft Power BI] [ lnk-power-bi] adja meg a hello adatok képi megjelenítés képességeinek. Power BI hello rugalmasságával lehetővé teszi a tooquickly létre saját IoT Suite adatokat használó interaktív irányítópultokat.

Egy tipikus IoT-megoldás hello architektúrájának áttekintését lásd: [Microsoft Azure és az eszközök internetes hálózatát (IoT) hello][iot-suite-what-is-azure-iot].

## <a name="preconfigured-solutions"></a>Előre konfigurált megoldások

Az IoT Suite előkonfigurált megoldásokat tartalmazza, hogy Ön tooquickly Ismerkedés a engedélyezése és az általános IoT-forgatókönyvek esetén, mint például hello tooexplore:

* Távoli figyelés
* Prediktív karbantartás
* Csatlakoztatott gyár

Ezen megoldások tooyour Azure-előfizetés telepítheti, és futtassa újra a teljes, végpontok közötti IoT-forgatókönyv.

## <a name="next-steps"></a>Következő lépések

Most, hogy az IoT Suite teendők áttekintése rendelkezik, és Mik a fő összetevőit, többet is megtudhat az IoT Suite előre konfigurált hello megoldásokról. További információkért lásd: [hello Azure IoT Mik előre konfigurált megoldások?][lnk-what-are-preconfig]

[lnk-sdks]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-azure-storage]: https://azure.microsoft.com/documentation/services/storage/
[lnk-document-db]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-power-bi]: https://powerbi.microsoft.com/
[lnk-web-apps]: https://azure.microsoft.com/documentation/services/app-service/web/
[iot-suite-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-what-are-preconfig]: iot-suite-what-are-preconfigured-solutions.md
[lnk-device-management]: ../iot-hub/iot-hub-device-management-overview.md
