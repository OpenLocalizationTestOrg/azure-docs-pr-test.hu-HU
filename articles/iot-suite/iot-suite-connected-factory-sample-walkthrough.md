---
title: "Csatlakoztatott factory-megoldás bemutatója – Azure | Microsoft Docs"
description: "Az Azure IoT előre konfigurált csatlakoztatott gyár megoldásának és architektúrájának leírása."
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
ms.date: 12/12/2017
ms.author: dobett
ms.openlocfilehash: 88fe50460baf8b7180da113b33a03120f39cf44f
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/12/2017
---
# <a name="connected-factory-preconfigured-solution-walkthrough"></a>Előre konfigurált csatlakoztatott gyár megoldás – bemutató

Az IoT Suite [előre konfigurált][lnk-preconfigured-solutions] csatlakoztatott gyár megoldással egy teljes körű ipari megoldást implementálhat, amely:

* Szimulált gyártósorokon OPC UA-kiszolgálókat futtató szimulált ipari eszközökhöz és valódi OPC UA-kiszolgálóeszközökhöz is csatlakozik. Az OPC UA architektúráról a [Csatlakoztatott gyár – GYIK](iot-suite-faq-cf.md) fejezetben talál további információt.
* Megjeleníti ezen eszközök és gyártósorok működési KPI- és OEE-értékeit.
* Bemutatja, hogyan lehet kommunikálni az OPC UA-kiszolgálórendszerekkel felhőalapú alkalmazások használatával.
* Lehetővé teszi, hogy összekapcsolja saját OPC UA-kiszolgálóeszközeit.
* Lehetővé teszi az OPC UA-kiszolgálóadatok tallózását és módosítását.
* Integrálható az Azure Time Series Insights (TSI) szolgáltatással, amely testreszabott nézeteket biztosít az OPC UA-kiszolgálók adatairól.

A megoldást kiindulási pontként használhatja a saját implementációjához, és a saját üzleti igényeinek megfelelően [testreszabhatja][lnk-customize] azt.

Ebben a cikkben bemutatjuk a csatlakoztatott gyár megoldás néhány fontos elemét, hogy jobban megismerhesse a szolgáltatás működését. A cikk azt is leírja, hogyan haladnak át az adatok a megoldáson. Ezeknek az ismereteknek a birtokában:

* Elháríthatja a megoldásban felmerülő hibákat.
* Megtervezheti, hogy miképpen érdemes testre szabni a megoldást úgy, hogy az megfeleljen egyedi igényeinek.
* Kialakíthatja saját, Azure-szolgáltatásokat használó IoT-megoldását.

További információ: [Csatlakoztatott gyár – GYIK](iot-suite-faq-cf.md).

## <a name="logical-architecture"></a>Logikai architektúra

A következő diagram az előre konfigurált megoldás logikai összetevőit vázolja fel:

![Csatlakoztatott gyár logikai architektúrája][connected-factory-logical]

## <a name="communication-patterns"></a>Kommunikációs minták

A megoldás az [OPC UA Pub/Sub specifikációt](https://opcfoundation.org/news/opc-foundation-news/opc-foundation-announces-support-of-publish-subscribe-for-opc-ua/) használja az OPC UA telemetriai adatok JSON-formátumban való küldésére az IoT Hub részére. A megoldás az [OPC Publisher](https://github.com/Azure/iot-edge-opc-publisher) IoT Edge-modult használja erre a célra.

A megoldás webalkalmazásba integrált OPC UA-ügyféllel is rendelkezik, amely képes kapcsolatot létesíteni a helyszíni OPC UA-kiszolgálókkal. Az ügyfél [fordított proxyt](https://wikipedia.org/wiki/Reverse_proxy) használ, és az IoT Hub segítségével anélkül képes létrehozni a kapcsolatot, hogy nyitott portokra lenne szüksége a helyszíni tűzfalon. Ezt a kommunikációs mintát [szolgáltatás által támogatott kommunikációnak](https://blogs.msdn.microsoft.com/clemensv/2014/02/09/service-assisted-communication-for-connected-devices/) hívják. A megoldás az [OPC Proxy](https://github.com/Azure/iot-edge-opc-proxy/) IoT Edge-modult használja erre a célra.


## <a name="simulation"></a>Szimuláció

A gyártósorok szimulált állomásokból és szimulált gyártási végrehajtó rendszerekből (MES) állnak. A szimulált eszközök és az OPC kiadói modul az OPC Foundation által kiadott [OPC UA .NET standardon][lnk-OPC-UA-NET-Standard] alapul.

Az OPC Proxy és az OPC Publisher modulként van megvalósítva az [Azure IoT Edge][lnk-Azure-IoT-Gateway] alapján. Mindegyik szimulált gyártósorhoz kijelölt átjáró van csatlakoztatva.

Minden szimulációs összetevő Azure Linux virtuális gépen futtatott Docker-tárolókban fut. A szimuláció úgy van konfigurálva, hogy alapértelmezés szerint nyolc szimulált gyártósort futtasson.

## <a name="simulated-production-line"></a>Szimulált gyártósor

A gyártósorok alkatrészeket gyártanak. Különböző állomásokból állnak: összeszerelő állomásból, tesztelő állomásból és csomagoló állomásból.

A szimuláció futtatja és frissíti az OPC UA-csomópontokon keresztül közzétett adatokat. Minden szimulált gyártósori állomást a MES vezényel az OPC UA-n keresztül.

## <a name="simulated-manufacturing-execution-system"></a>Szimulált gyártási végrehajtó rendszer

A MES az OPC UA-n keresztül figyeli a gyártósor egyes állomásait azok állapotváltozásainak észlelése érdekében. OPC UA-metódusokat hív meg az állomások vezérlése érdekében, és egyik állomásról a következőre továbbítja a termékeket a befejezésükig.

## <a name="gateway-opc-publisher-module"></a>Átjáró OPC kiadói modul

Az OPC kiadói modul az állomás OPC UA-kiszolgálóihoz csatlakozik és feliratkozik a kiadandó OPC-csomópontokra. A modul a csomópontadatokat JSON-formátumba alakítja, titkosítja, majd az IoT Hubra küldi OPC UA Pub/Sub üzenetekként.

Az OPC kiadói modulnak csak kimenő https-portra (443) van szüksége, és képes együttműködni a meglévő vállalati infrastruktúrával.

## <a name="gateway-opc-proxy-module"></a>Átjáró OPC-proxymodul

Az átjáró OPC UA-proxymodul bináris OPC UA-parancs- és vezérlési üzeneteket továbbít, és csak kimenő https-portot (443) igényel. Képes a meglévő vállalati infrastruktúrát használni, beleértve a webes proxykat is.

IoT Hub-eszközmetódusokkal viszi át a csomagolt TCP/IP-adatokat az alkalmazásrétegen, így az SSL/TLS segítségével biztosítja a végpontok megbízhatóságát, az adatok titkosítását és az integritást.

A magán a proxyn keresztül továbbított OPC UA bináris protokoll UA-hitelesítést és -titkosítást használ.

## <a name="azure-time-series-insights"></a>Azure Time Series Insights

Az átjáró OPC kiadói modul feliratkozik az OPC UA-kiszolgálócsomópontokra az adatértékek változásainak észlelése érdekében. Ha a rendszer adatmódosítást észlel valamelyik csomóponton, ez a modul üzeneteket küld az Azure IoT Hubnak.

Az IoT Hub eseményforrást biztosít az Azure TSI-nek. A TSI 30 napig tárol adatokat az üzenetekhez csatolt időbélyegek alapján. A adatok tartalma:

* OPC UA ApplicationUri
* OPC UA NodeId
* A csomópont értéke
* Forrás időbélyege
* OPC UA DisplayName

A TSI jelenleg nem engedélyezi, hogy az ügyfelek testreszabják, mennyi ideig szeretnék megőrizni az adatokat.

A TSI a csomópontadatok lekérdezését egy **SearchSpan** (**Time.From**, **Time.To**), az összesítést pedig az **OPC UA ApplicationUri**, az **OPC UA NodeId** vagy az **OPC UA DisplayName** attribútum alapján végzi.

Az OEE- és a KPI-mérőműszer adatainak és az idősorozat-diagramok lekérdezése érdekében a rendszer az események száma, valamint a Sum, az Avg, a Min és a Max értékek alapján összesíti az adatokat.

Az idősorozatok más folyamattal vannak felépítve. Az OEE és a KPI-k állomások alapadataiból vannak kiszámítva és buborékba vannak rendezve a topológiához (gyártósorok, gyárak, vállalat) az alkalmazásban.

Ezenkívül az OEE- és a KPI-topológia idősorozatai az alkalmazásban lesznek kiszámítva, amikor egy megjelenített időtartomány készen áll. A nap nézet például minden teljes órában frissül.

A csomópontadatok idősorozat nézete közvetlenül a TSI-ból származik az időtartomány összegzése segítségével.

## <a name="iot-hub"></a>IoT Hub
Az [IoT hub][lnk-IoT Hub] fogadja az OPC kiadói modulból a felhőbe küldött adatokat, és elérhetővé teszi azokat az Azure TSI szolgáltatás számára. 

A megoldásban az IoT Hub ezenkívül a következőket teszi:
- Fenntart egy identitásjegyzéket, amely tárolja az összes OPC kiadói modul és az összes OPC-proxymodul azonosítóját.
- Átviteli csatornaként szolgál az OPC-proxymodul kétirányú kommunikációjához.

## <a name="azure-storage"></a>Azure Storage
A megoldás Azure Blob Storage tárolót használ a virtuális gép lemezes tárolójaként, valamint az üzembehelyezési adatok tárolásához.

## <a name="web-app"></a>Webalkalmazás
Az előre konfigurált megoldás részeként üzembe helyezett webalkalmazás integrált OPC UA-ügyfélből, riasztások feldolgozásából és a telemetria megjelenítéséből áll.

## <a name="telemetry-data-flow"></a>Telemetria-adatfolyam

![Telemetria-adatfolyam](media/iot-suite-connected-factory-walkthrough/telemetry_dataflow.png)

### <a name="flow-steps"></a>A folyamat lépései

1. Az OPC-közzétevő beolvassa a szükséges OPC UA X509 tanúsítványokat és IoT Hub biztonsági hitelesítő adatokat a helyi tanúsítványtárolóból.
    - Szükség esetén az OPC-közzétevő létrehozza és tárolja a tanúsítványtárolóból hiányzó tanúsítványokat vagy hitelesítő adatokat.

2. Az OPC-közzétevő regisztrálja magát az IoT Hubbal.
    - A konfigurált protokollt használja. Az IoT Hub ügyfél SDK által támogatott bármelyik protokollt használhatja. Az alapértelmezett az MQTT.
    - A protokollkommunikációt a TLS védi.

3. Az OPC-közzétevő beolvassa a konfigurációs fájlt.

4. Az OPC-közzétevő létrehoz egy OPC-munkamenetet mindegyik konfigurált OPC UA-kiszolgálóval.
    - TCP-kapcsolatot használ.
    - Az OPC-közzétevő és az OPC UA-kiszolgáló X509 tanúsítványokkal hitelesíti egymást.
    - Az összes további OPC UA-forgalmat a konfigurált OPC UA titkosítási mechanizmus titkosítja.
    - Az OPC-közzétevő minden konfigurált közzétételi intervallum OPC-munkamenetében létrehoz egy OPC-előfizetést.
    - OPC által monitorozott elemeket hoz létre az OPC-csomópontokhoz az OPC-előfizetésben való közzétételhez.

5. Ha egy monitorozott OPC-csomópont értéke módosul, az OPC UA-kiszolgáló frissítéseket küld az OPC-közzétevőhöz.

6. Az OPC-közzétevő átkódolja az új értéket.
    - Több módosítást kötegel, ha a kötegelés engedélyezett.
    - IoT Hub-üzenetet hoz létre.

7. Az OPC-közzétevő üzenetet küld az IoT Hubra.
    - A konfigurált protokollt használja.
    - A kommunikációt a TLS védi.

8. A Time Series Insights (TSI) beolvassa az üzeneteket az IoT Hubról.
    - AMQP-t használ TCP/TLS protokollon keresztül.
    - Ez a lépés az adatközpontban történik.

9. Inaktív adat a TSI-ben.

10. Az Azure AppService-lekérdezésekben a csatlakoztatott gyári webalkalmazás a TSI-ből származó adatokat igényelt.
    - TCP/TLS által védett kommunikációt használ.
    - Ez a lépés az adatközpontban történik.

11. A webböngésző a csatlakoztatott gyár webalkalmazásához csatlakozik.
    - Rendereli a csatlakoztatott gyár irányítópultját.
    - HTTPS-en keresztül csatlakozik.
    - A csatlakoztatott gyár alkalmazásának eléréséhez az Azure Active Directoryn keresztül kell hitelesíteni a felhasználót.
    - A csatlakoztatott gyár alkalmazásába irányuló összes WebApi-hívást hamisítás elleni tokenek védik.

12. Amikor adatfrissítés történik, a csatlakoztatott gyári webalkalmazás frissített adatokat küld a webböngészőnek.
    - A SignalR protokollt használja.
    - A TCP/TLS védi.

## <a name="browsing-data-flow"></a>Böngészési adatfolyam

![Böngészési adatfolyam](media/iot-suite-connected-factory-walkthrough/browsing_dataflow.png)

### <a name="flow-steps"></a>A folyamat lépései

1. Elindul az OPC-proxy (kiszolgáló-összetevő).
    - Beolvassa a megosztott hozzáférési kulcsokat a helyi tárolóból.
    - Szükség esetén a tárolóban tárolja a hiányzó hozzáférési kulcsokat.

2. Az OPC-proxy (kiszolgáló-összetevő) regisztrálja magát az IoT Hubbal.
    - Beolvassa az összes ismert eszközét az IoT Hubról.
    - Socketen vagy Secure WebSocketen keresztüli TLS protokollon keresztüli MQTT-t használ.

3. A webböngésző a csatlakoztatott gyár webalkalmazásához csatlakozik, és rendereli a csatlakoztatott gyár irányítópultját.
    - HTTPS-t használ.
    - A felhasználó kiválasztja azt az OPC UA-kiszolgálót, amelyhez csatlakozni kíván.

4. A csatlakoztatott gyári webalkalmazás OPC UA-munkamenetet hoz létre a kiválasztott OPC UA-kiszolgálóval.
    - OPC UA-vermet használ.

5. Az OPC-proxyátvitel kérést kap az OPC UA-veremtől, hogy TCP szoftvercsatorna-kapcsolatot létesítsen az OPC UA-kiszolgálóval.
    - Csak lekéri a TCP hasznos adatokat és módosítás nélkül használja őket.
    - Ez a lépés a csatlakoztatott gyár webalkalmazásában történik.

6. Az OPC-proxy (ügyfélösszetevő) megkeresi az OPC-proxy (kiszolgáló-összetevő) eszközt az IoT Hub eszközjegyzékében. Ezután meghívja az OPC-proxy (kiszolgáló-összetevő) eszközmetódusát az IoT Hubon.
    - TCP/TLS protokollon keresztüli HTTPS-t használ az OPC-proxy kereséséhez.
    - TCP/TLS protokollon keresztüli HTTPS-t használ, hogy TCP szoftvercsatorna-kapcsolatot létesítsen az OPC UA-kiszolgálóval.
    - Ez a lépés az adatközpontban történik.

7. Az IoT Hub meghívja az OPC-proxy (kiszolgáló-összetevő) eszköz egyik eszközmetódusát.
    - Socket vagy Secure WebSocket kapcsolaton keresztüli TLS protokollon keresztüli létrehozott MQTT-t használ, hogy TCP szoftvercsatorna-kapcsolatot létesítsen az OPC UA-kiszolgálóval.

8. Az OPC-proxy (kiszolgáló-összetevő) továbbküldi a TCP hasznos adatait a helyszíni hálózatára.

9. Az OPC UA-kiszolgáló feldolgozza a hasznos adatokat, és visszaküldi a választ.

10. Az OPC-proxy (kiszolgáló-összetevő) szoftvercsatornája fogadja a választ.
    - Az OPC-proxy az eszközmetódus visszatérési értékeként küldi el az adatokat az IoT Hubra és az OPC-proxyra (ügyfélösszetevő).
    - Ezeket az adatokat az OPC UA-verembe kézbesíti a rendszer a csatlakoztatott gyári alkalmazásban.

11. A csatlakoztatott gyári webalkalmazás visszaküldi az OPC UA-kiszolgálótól kapott OPC UA-specifikus adatokkal kiegészült OPC UX-et a webböngészőnek renderelésre.
    - Az OPC-címtérben való böngészés és az OPC-címtérben lévő csomópontokra történő függvényalkalmazás közben az OPC böngészői UX HTTPS-en keresztüli, hamisításgátló jogkivonatokkal védett AJAX-hívásokkal kéri le az adatokat a csatlakoztatott gyár webalkalmazásából.
    - Szükség esetén az ügyfél a 4-10. lépésekben leírt kommunikációval cserél információkat az OPC UA-kiszolgálóval.

> [!NOTE]
> Az OPC-proxy (kiszolgáló-összetevő) és az OPC-proxy (ügyfél) összetevő elvégzi a 4-10. lépéseket az OPC UA-kommunikációhoz kapcsolódó összes TCP-forgalomhoz.

> [!NOTE]
> A csatlakoztatott gyár webalkalmazásán belüli OPC UA-kiszolgáló és OPC UA-verem esetén az OPC–proxy kommunikáció átlátszó, és a hitelesítésre és titkosításra vonatkozó összes OPC UA biztonsági funkció érvényben van.

## <a name="next-steps"></a>Következő lépések

Folytassa az IoT Suite megismerését az alábbi cikkek elolvasásával:

* [Engedélyek az azureiotsuite.com webhelyen][lnk-permissions]
* [Átjáró üzembe helyezése Windows vagy Linux rendszeren az előre konfigurált csatlakoztatott gyár megoldáshoz](iot-suite-connected-factory-gateway-deployment.md)
* [OPC-közzétevő referenciamegvalósítása](iot-suite-connected-factory-publisher.md).

[connected-factory-logical]:media/iot-suite-connected-factory-walkthrough/cf-logical-architecture.png

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-v1-guidance-on-customizing-preconfigured-solutions.md
[lnk-IoT Hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-OPC-UA-NET-Standard]:https://github.com/OPCFoundation/UA-.NETStandardLibrary
[lnk-Azure-IoT-Gateway]: https://github.com/azure/iot-edge
[lnk-permissions]: iot-suite-v1-permissions.md
