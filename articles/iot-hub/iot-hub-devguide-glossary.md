---
title: "az IoT-központ szószedet aaaAzure |} Microsoft Docs"
description: "Fejlesztői útmutató - tooAzure IoT-központ kapcsolatos gyakori kifejezések."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 16ef29ea-a185-48c3-ba13-329325dc6716
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 217eb082c13e06df5c07543c65d498ad3e395939
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="glossary-of-iot-hub-terms"></a>Az IoT-központ szószedet
Ez a cikk soroljuk hello általános kifejezés, amely hello IoT-központ cikkeket.

## <a name="advanced-message-queueing-protocol"></a>Speciális üzenet-Várólistázást protokoll
[Üzenet-Várólistázást protokoll (AMQP) speciális](https://www.amqp.org/) van egy hello üzenetküldési protokollok, amelyek [IoT-központ](#iot-hub) eszközökkel való kommunikáció támogatja. Üzenetküldési protokollok, amely támogatja az IoT-központ hello kapcsolatos további információkért lásd: [üzeneteket küldjön és fogadjon IoT hubbal](iot-hub-devguide-messaging.md).

## <a name="azure-cli"></a>Azure CLI
Hello [Azure CLI](../cli-install-nodejs.md) platformfüggetlen, nyílt forráskódú, rendszerhéj-alapú, a parancs eszköz létrehozására és kezelésére a Microsoft Azure erőforrásait. Ebben a hello CLI Node.js segítségével van megvalósítva.

## <a name="azure-cli-20"></a>Azure CLI 2.0
Hello [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) platformfüggetlen, nyílt forráskódú, rendszerhéj-alapú, a parancs eszköz létrehozására és kezelésére a Microsoft Azure erőforrásait. A parancssori felület hello előzetes verzióját a Python segítségével van megvalósítva.


## <a name="azure-iot-device-sdks"></a>Az Azure SDK-k IoT-eszközök
Nincsenek _eszköz SDK-k_ érhető el, amelyek lehetővé teszik toocreate több nyelven [eszköz alkalmazások](#device-app) , amely kommunikálni az IoT-központ. hello IoT-központ oktatóprogramok bemutatják, hogyan toouse ezek eszközoldali SDK-k. Hello forráskódja és hello eszköz SDK-kkal kapcsolatos további információkat talál a Githubon [tárház](https://github.com/Azure/azure-iot-sdks).

## <a name="azure-iot-edge"></a>Azure IoT Edge
IoT peremhálózati teszi lehetővé, amelyek lehetővé teszik az átjáró-hez csatlakoztatott eszközöket toocommunicate toowrite alkalmazások [IoT-központ](#iot-hub). hello IoT peremhálózati oktatóprogramok bemutatják, hogyan toouse ezt a szolgáltatást. Hello forráskódja és Azure IoT peremhálózati kapcsolatos további információkat talál a Githubon [tárház](https://github.com/Azure/iot-edge).

## <a name="azure-iot-service-sdks"></a>Az Azure IoT szolgáltatás SDK-k
Nincsenek _SDK szolgáltatás_ érhető el, amelyek lehetővé teszik toocreate több nyelven [háttér-alkalmazások](#back-end-app) , amely kommunikálni az IoT-központ. hello IoT-központ oktatóprogramok bemutatják, hogyan toouse ezek szolgáltatás SDK-k. Hello forráskódja és hello szolgáltatás SDK-kkal kapcsolatos további információkat talál a Githubon [tárház](https://github.com/Azure/azure-iot-sdks).

## <a name="azure-portal"></a>Azure Portal
Hello [Microsoft Azure-portálon](https://portal.azure.com) a központi hely, ahol kioszthatja és az Azure erőforrások kezeléséhez. A tartalom használatával szervezi _paneleken_. Egyes hello IoT-központ oktatóanyagok kérheti, toouse hello [a klasszikus Azure portálon](https://manage.windowsazure.com).

## <a name="azure-powershell"></a>Azure PowerShell
[Az Azure PowerShell](/powershell/azure/overview) gyűjteménye parancsmagok használata toomanage Azure Windows PowerShell. Hello parancsmagok toocreate használja, teszteléséhez, telepítheti, és megoldások kezelése és hello Azure platformon kézbesíti szolgáltatások.

## <a name="azure-resource-manager"></a>Azure Resource Manager
[Az Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) toowork hello erőforrásokat csoportként a megoldás lehetővé teszi. Központi telepítése, frissítése vagy törlése hello erőforrások a megoldás egyetlen, koordinált műveletben.

## <a name="azure-service-bus"></a>Azure Service Bus
[A Service Bus](../service-bus/index.md) biztosít a felhő engedélyezve van a vállalati üzenetkezelési és továbbítón keresztüli kommunikációt, amely segít a helyszíni megoldásokkal kapcsolattartásnak hello felhő. Az IoT-központ oktatóprogramok ellenőrizze a Service busszal használni [várólisták](../service-bus-messaging/service-bus-messaging-overview.md).

## <a name="azure-storage"></a>Azure Storage
[Az Azure Storage](../storage/common/storage-introduction.md) egy felhőalapú tárolómegoldás. Hello Blob Storage szolgáltatás strukturálatlan toostore használt tartalmazza objektumadatokat. Az IoT-központ oktatóprogramok a blob storage használata.

## <a name="back-end-app"></a>Háttér-alkalmazás
Hello környezetében [IoT-központ](#iot-hub), egy háttér-alkalmazás, amely a tooone hello szolgáltatás felé néző végpontok az IoT-központ az alkalmazás. Például előfordulhat, hogy lekér egy háttér-alkalmazás [eszközről a felhőbe](#device-to-cloud)üzenetek vagy hello kezelése [identitásjegyzékhez](#identity-registry). Általában hello felhő egy háttér-alkalmazás fut, de számos olyan hello oktatóanyagok hello háttér-alkalmazások a helyi fejlesztési számítógépén futó konzol alkalmazásokra.

## <a name="built-in-endpoints"></a>Beépített végpontok
Minden IoT-központ tartalmazza a beépített [végpont](iot-hub-devguide-endpoints.md) , amely Event Hub-kompatibilis. Bármely mechanizmus, amely az Event Hubs tooread eszközről a felhőbe a végpont üzeneteit is használhatja.

## <a name="cloud-gateway"></a>Átjáró
Egy felhőátjáróhoz lehetővé teszi, hogy a kapcsolatot a eszközöket, amelyek nem kapcsolódnak közvetlenül túl[IoT-központ](#iot-hub). Egy felhőátjáróhoz hajtódik hello felhőt, ezzel szemben tooa [mező átjáró](#field-gateway) , amelyen fut a helyi tooyour eszközök. Egy tipikus használati eset felhő átjáróra tooimplement protokollfordításhoz az eszközökhöz.

## <a name="cloud-to-device"></a>Felhő-eszköz
Az IoT hub tooa csatlakoztatott eszköz által küldött toomessages hivatkozik. Ezek az üzenetek gyakran, parancsok, melyek arra utasítják a hello eszköz tootake művelet. További információkért lásd: [üzeneteket küldjön és fogadjon IoT hubbal](iot-hub-devguide-messaging.md).

## <a name="connection-string"></a>Kapcsolati karakterlánc
Az alkalmazás kódja tooencapsulate hello szükséges adatokat tooconnect tooan végpont használja a kapcsolati karakterláncokat. A kapcsolati karakterlánc jellemzően hello címe hello végpont és a biztonsági adatokat, de a kapcsolati karakterlánc formátumok szolgáltatások eltérőek. A kapcsolati karakterlánc hello IoT-központ szolgáltatáshoz tartozó két típusa van:
- *Eszköz kapcsolati karakterláncok* eszközök tooconnect toohello eszköz felé néző végpontok az IoT-központ engedélyezése.
- *Az IoT-központ kapcsolati karakterláncok* háttér-alkalmazások tooconnect toohello szolgáltatás felé néző végpontok az IoT-központ engedélyezése.

## <a name="custom-endpoints"></a>Egyéni végpontokat
Létrehozhat egyéni [végpontok](iot-hub-devguide-endpoints.md) a az IoT hub toodeliver által küldött üzenetek egy [útválasztási szabály](#routing-rules). Egyéni végpontokat csatlakozzon közvetlenül tooan eseményközpont, a Service Bus-üzenetsorba, vagy egy Service Bus-témakörbe.

## <a name="custom-gateway"></a>Egyéni átjáró
Átjáró lehetővé teszi, hogy a kapcsolatot a eszközöket, amelyek nem kapcsolódnak közvetlenül túl[IoT-központ](#iot-hub). Használhat [Azure IoT peremhálózati](#azure-iot-edge) toobuild egyéni átjárók valósítsa meg az egyéni logika toohandle üzenetek, az egyéni protokoll konverziót, illetve a más feldolgozás a peremhálózati hello.

## <a name="data-point-message"></a>Adatpont üzenet
Adatpont üzenetet egy [eszközről a felhőbe](#device-to-cloud) tartalmazó üzenetet [telemetriai](#telemetry) adatok, például a szél sebesség vagy hőmérséklet.

## <a name="desired-configuration"></a>Szükséges Konfigurációkezelő
Hello környezetében egy [eszköz iker](iot-hub-devguide-device-twins.md), szükséges a konfigurációs tulajdonságok és metaadatok hello eszköz iker, amely hello eszközzel szinkronizálnia kell a teljes készletének toohello hivatkozik.

## <a name="desired-properties"></a>Kívánt tulajdonságai
Hello környezetében egy [eszköz iker](iot-hub-devguide-device-twins.md), a keresett tulajdonságok az egy alkalmazásra vonatkozó hello eszköz iker használt [tulajdonságok jelentett](#reported-properties) toosynchronize eszköz konfigurációs vagy az állapot. Kívánt tulajdonságok csak állítható be egy [háttér-alkalmazás](#back-end-app) és betartják-hello [eszközalkalmazás](#device-app).

## <a name="device-to-cloud"></a>Eszköz-felhő
Egy csatlakoztatott eszközről küldött túl toomessages hivatkozik[IoT-központ](#iot-hub). Lehet, hogy ezek az üzenetek [adatpont](#data-point-message) vagy [interaktív](#interactive-message) üzeneteket. További információkért lásd: [üzeneteket küldjön és fogadjon IoT hubbal](iot-hub-devguide-messaging.md).

## <a name="device"></a>Eszköz
Hello környezetében IoT, a jellemzően csak kevés számítógépet érintő, önálló számítástechnikai eszközről, amely adatokat gyűjteni, vagy más eszközök. Egy eszköz lehet például egy környezeti figyelési eszköz vagy egy tartományvezérlő üvegházhatású hello watering és szellőzőrendszerek rendszerekhez. Hello [eszköz katalógus](https://catalog.azureiotsuite.com/) hardver eszközök hitelesített toowork rendelkező listája [IoT-központ](#iot-hub).

## <a name="device-app"></a>Eszköz alkalmazás
Egy eszköz-alkalmazás fut. a [eszköz](#device) és leírók hello kommunikál a [IoT-központ](#iot-hub). Általában akkor használják, hello egyik [Azure IoT-eszközök SDK-k](#azure-iot-device-sdks) Ha egy eszköz alkalmazás üzembe helyezése. Hello IoT oktatóanyagok számos, használhat egy [szimulált eszköz](#simulated-device) kényelmét szolgálja.

## <a name="device-condition"></a>Eszköz feltétel
Toodevice állapotának adatai, például a hello csatlakozási módszer jelenleg használatban van, hivatkozik által jelentett módon egy [eszközalkalmazás](#device-app). [Eszköz alkalmazások](#device-app) is jelentheti képességeit. Eszköz twins feltételt, és képes adatait is kereshet.

## <a name="device-data"></a>Eszközadatok
Eszközadatok hivatkozik az IoT-központ hello tárolt toohello eszközönkénti adatok [identitásjegyzékhez](#identity-registry). Lehetséges tooimport, és exportálja az adatokat.

## <a name="device-explorer"></a>Eszköz explorer
Hello [eszköz explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) olyan eszköz, amely a Windows rendszeren fut, és lehetővé teszi, hogy Ön toomanage hello eszközt [identitásjegyzékhez](#identity-registry).hello eszköz is küldhet és fogadhat üzeneteket tooyour eszközök.

## <a name="device-identities-rest-api"></a>Eszközidentitások REST API-ja
Hello [eszköz identitások REST API](https://docs.microsoft.com/rest/api/iothub/iothubresource) lehetővé teszi az eszközök regisztrálva hello toomanage [identitásjegyzékhez](#identity-registry) egy REST API használatával. Általában, használjon egy magasabb szintű hello [SDK szolgáltatás](#azure-iot-service-sdks) IoT-központ oktatóanyagok hello ismertetett módon.

## <a name="device-identity"></a>Eszközidentitás
hello eszközidentitás hello hozzárendelt egyedi azonosítója tooevery eszköz regisztrálva a hello [identitásjegyzékhez](#identity-registry).

## <a name="device-management"></a>Eszközfelügyelet
Eszköz felügyelete magába foglalja a hello teljes életciklusát társított hello eszközkezelésről az IoT-megoldásból, beleértve a tervezési, kiépítési, konfigurálása, a figyelés és a rajtuk.

## <a name="device-management-patterns"></a>Eszközfelügyeleti minták
[Az IoT-központ](#iot-hub) lehetővé teszi, hogy a közös eszköz felügyeleti minták újraindul, hajt végre a gyári alaphelyzetbe állítást és az eszközök belső vezérlőprogram frissítések végrehajtásához.

## <a name="device-messaging-rest-api"></a>Eszközök üzenetküldési REST API-ja
Használhatja a hello [Device Messaging REST API-val](https://docs.microsoft.com/rest/api/iothub/httpruntime) egy eszközről toosend eszköz-felhő tooan IoT hub-üzenetek, és fogadni [felhő eszközre](#cloud-to-device) az IoT-központ üzeneteit. Általában, használjon egy magasabb szintű hello [eszköz SDK-k](#azure-iot-device-sdks) IoT-központ oktatóanyagok hello ismertetett módon.

## <a name="device-provisioning"></a>Eszköz kiépítése
Az eszközök kiépítését hello folyamat, hello kezdeti hozzáadása [eszközadatok](#device-data) toohello tárolja a megoldásban. Új eszköz tooconnect tooyour központ tooenable, hozzá kell adnia egy eszköz azonosítója és a kulcsok toohello IoT-központ [identitásjegyzékhez](#identity-registry). Hello kiépítési folyamat részeként tooinitialize eszközre vonatkozó adatok más megoldás üzletekben szüksége lehet.

## <a name="device-twin"></a>Ikereszközök
A [eszköz iker](iot-hub-devguide-device-twins.md) JSON-dokumentum, amely például metaadat, a konfiguráció és a feltételek eszköz állapot adatait tárolja. [Az IoT-központ](#iot-hub) továbbra is fennáll az egyes eszközök, az IoT hub megadó egy eszköz iker. Eszköz twins lehetővé teszik a toosynchronize [eszköz feltételek](#device-condition) és konfigurációk hello eszköz és hello megoldás közötti háttér. Eszköz twins toolocate meghatározott eszközökhöz és a hosszú ideig futó műveletek állapotának lekérdezése hello lekérdezheti.

## <a name="device-twin-queries"></a>Eszköz iker lekérdezések
[Eszköz iker lekérdezések](iot-hub-devguide-query-language.md) hello SQL-szerű IoT Hub lekérdezési nyelv tooretrieve adatait az eszköz twins használja. Használhatja ugyanazt az IoT-központ nyelvi tooretrieve információk lekérdezése hello [feladatok](#job) az IoT hub futtatja.

## <a name="device-twin-rest-api"></a>Eszköz iker REST API-n
Használhatja a hello [eszköz iker REST API](https://docs.microsoft.com/rest/api/iothub/devicetwinapi) hello megoldásból háttérhálózatként toomanage az eszköz twins. hello API lehetővé teszi a tooretrieve és frissítés [eszköz iker](#device-twin) tulajdonságait, és meghívja [módszerek közvetlen](#direct-method). Általában, használjon egy magasabb szintű hello [SDK szolgáltatás](#azure-iot-service-sdks) IoT-központ oktatóanyagok hello ismertetett módon.

## <a name="device-twin-synchronization"></a>Eszköz két szinkronizálás
Eszköz két szinkronizálás használ hello [szükséges tulajdonságok](#desired-properties) az eszköz twins tooconfigure a az eszközök és lekérése [tulajdonságok jelentett](#reported-properties) az eszközök toostore a hello eszköz iker a.

## <a name="direct-method"></a>Közvetlen módszer
A [közvetlen módszer](iot-hub-devguide-direct-methods.md) módja a az Ön tootrigger egy metódus tooexecute az eszközön az API-k az IoT hub a figyelőn.

## <a name="endpoint"></a>Végpont
Az IoT-központ mutatja meg több [végpontok](iot-hub-devguide-endpoints.md) , amely engedélyezi az alkalmazások tooconnect toohello IoT hub. Nincsenek az eszköz felé néző végpontok, amelyek lehetővé teszik az eszközök tooperform műveletek, például a küldő [eszközről a felhőbe](#device-to-cloud) üzenetek és a fogadás [felhő eszközre](#cloud-to-device) üzeneteket. Nincsenek a szolgáltatás elérhető felügyeleti végpontok, amelyek lehetővé teszik [háttér-alkalmazások](#back-end-app) tooperform műveletek, például [eszközidentitás](#device-identity) felügyeleti és a két kezelése. Nincsenek a szolgáltatás felé néző [beépített végpontok](#built-in-endpoints) eszközről a felhőbe üzenetek olvasásához. Létrehozhat [egyéni végpontokat](#custom-endpoints) tooreceive eszközről a felhőbe üzenetek által küldött egy [útválasztási szabály](#routing-rules).

## <a name="event-hubs-service"></a>Event Hubs szolgáltatás
[Az Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) egy kiválóan méretezhető adatbefogadási szolgáltatás, amely több millió fogadására képes van az események másodpercenkénti számát. hello szolgáltatás lehetővé teszi a tooprocess, és a csatlakoztatott eszközök és alkalmazások által létrehozott adatok nagy mennyiségű hello elemzése. Az IoT-központ szolgáltatás hello, lásd: [összehasonlítása az Azure IoT-központ és az Azure Event Hubs](iot-hub-compare-event-hubs.md).

## <a name="event-hub-compatible-endpoint"></a>Event Hub-kompatibilis végpont
tooread [eszközről a felhőbe](#device-to-cloud) üzenetek küldése tooyour IoT-központ, a központ tooan végpont csatlakozhat, és bármely Event Hub-kompatibilis metódus tooread használja azokat az üzeneteket. Event Hub-kompatibilis módszerekre hello segítségével [Event Hubs SDK-k](../event-hubs/event-hubs-programming-guide.md) és [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md).

## <a name="field-gateway"></a>A mező átjáró
Egy mező átjáró lehetővé teszi, hogy a kapcsolatot a eszközöket, amelyek nem kapcsolódnak közvetlenül túl[IoT-központ](#iot-hub) és általában helyileg telepíteni az eszközöket. További információkért lásd: [Mi az Azure IoT Hub?](iot-hub-what-is-iot-hub.md)

## <a name="free-account"></a>Ingyenes fiók
Létrehozhat egy [ingyenes Azure-fiókot](https://azure.microsoft.com/pricing/free-trial/) toocomplete hello IoT-központ oktatóanyagok és kísérletezhet hello IoT-központ szolgáltatás (és más Azure-szolgáltatások).

## <a name="gateway"></a>Átjáró
Átjáró lehetővé teszi, hogy a kapcsolatot a eszközöket, amelyek nem kapcsolódnak közvetlenül túl[IoT-központ](#iot-hub). Lásd még: [átjáró mezőben](#field-gateway), [átjáró Cloud](#cloud-gateway), és [egyéni átjáró](#custom-gateway).

## <a name="identity-registry"></a>Identitásjegyzékhez
Hello [identitásjegyzékhez](iot-hub-devguide-identity-registry.md) hello beépített összetevője, amely hello egyes eszközökkel kapcsolatos információkat tárolja az IoT-központ tooconnect tooan IoT-központ engedélyezett.

## <a name="interactive-message"></a>Interaktív üzenet
Egy interaktív üzenetről, hogy egy [felhő eszközre](#cloud-to-device) üzenet, amely elindítja a hello megoldás háttérrendszerének egy azonnali beavatkozást. Például egy eszköz el tudja küldeni egy kell automatikusan kilépteti tooa CRM-rendszerbe, a hibával kapcsolatos riasztás.

## <a name="iot-hub"></a>IoT Hub
IoT-központot egy teljes körűen felügyelt Azure szolgáltatás, amely lehetővé teszi a megbízható és biztonságos kétirányú kommunikációs eszközök millióira között, és a megoldás háttérrendszere. További információkért lásd: [Mi az Azure IoT Hub?](iot-hub-what-is-iot-hub.md) Használja a [Azure-előfizetés](#subscription), létrehozhat az IoT hub toohandle a munkaterhelések üzenetküldési IoT.

## <a name="iot-hub-metrics"></a>Az IoT-központ metrikák
[Az IoT-központ metrikák](iot-hub-metrics.md) biztosítanak az IoT-központok hello hello állapotával kapcsolatos adatokat a [Azure-előfizetés](#subscription). Az IoT-központ metrikák engedélyezése meg tooassess hello hello szolgáltatást és hello eszközök általános állapotát tooit csatlakoztatva. Az IoT-központ mérőszámok segítségével tekintse meg az IoT hubbal jelenít meg, és vizsgálja meg a problémák kiváltó okának anélkül, hogy az Azure támogatási toocontact.

## <a name="iot-hub-query-language"></a>Az IoT-központ lekérdezési nyelv
Hello [IoT-központ lekérdezési nyelv](iot-hub-devguide-query-language.md) olyan SQL-szerű nyelv, amely lehetővé teszi a tooquery a [feladatok](#job) és eszköz twins.

## <a name="iot-hub-resource-provider-rest-api"></a>Az IoT-központ erőforrás-szolgáltató REST API-n
Használhatja a hello [IoT Hub erőforrás szolgáltató REST API](https://docs.microsoft.com/rest/api/iothub/resourceprovider/iot-hub-resource-provider-rest) toomanage hello IoT-központok a a [Azure-előfizetés](#subscription) például létrehozása, frissítése és törlése hubok műveletet hajt végre.

## <a name="iot-suite"></a>IoT Suite
Az Azure IoT Suite csomagok együtt előkonfigurált megoldások a több Azure-szolgáltatásokhoz. Ezek előre konfigurált megoldások lehetővé teszik a tooget, gyorsan használatába szabhatják IoT-végpontok közötti implementációja. További információkért lásd: [Mi az Azure IoT Suite?](../iot-suite/iot-suite-overview.md)

## <a name="iothub-explorer"></a>IOT hubbal-explorer
Hello [IOT hubbal-explorer](https://github.com/azure/iothub-explorer) platformfüggetlen, a parancssori eszköz. hello eszköz lehetővé teszi, hogy Ön toomanage hello eszközt [identitásjegyzékhez](#identity-registry), küldése és üzenetek és fájlok fogadása az eszközök és az IoT hub operatív figyeléséhez.

## <a name="job"></a>Feladat
A megoldás háttérrendszeréhez használható [feladatok](iot-hub-devguide-jobs.md) tooschedule és nyomon követése tevékenység az eszközök regisztrálva az IoT hub. Tevékenységei közé tartoznak a két eszköz frissítése [szükséges tulajdonságok](#desired-properties), frissítési eszköz iker [címkék](#tags), és hívja [módszerek közvetlen](#direct-method). [Az IoT-központ](#iot-hub) feladatokat is használ túl[import tooand exportálás](iot-hub-devguide-identity-registry.md#import-and-export-device-identities) a hello [identitásjegyzékhez](#identity-registry).

## <a name="jobs-rest-api"></a>Feladatok REST API-n
Hello [feladatok REST API](https://docs.microsoft.com/rest/api/iothub/jobapi) lehetővé teszi a toomanage [feladatok](#job) az IoT hub futtatja.

## <a name="module"></a>Modul
A [Azure IoT peremhálózati](iot-hub-linux-iot-edge-get-started.md), egy [modul](iot-hub-linux-iot-edge-get-started.md) , amely egy adott feladatot végrehajtó összetevője. Feladatok közé tartozik a választásával dolgozhat fel egy eszközről egy üzenetet, üzenet átalakítása vagy elküldeni egy üzenetet tooan IoT-központ. Ügynök felelős továbbításhoz modulok között. Az Azure IoT peremhálózati mintavételi modulok készletét tartalmazza. A saját egyéni modulokat is létrehozhat.

## <a name="mqtt"></a>MQTT
[MQTT](http://mqtt.org/) van egy hello üzenetküldési protokollok, amelyek [IoT-központ](#iot-hub) eszközökkel való kommunikáció támogatja. Üzenetküldési protokollok, amely támogatja az IoT-központ hello kapcsolatos további információkért lásd: [üzeneteket küldjön és fogadjon IoT hubbal](iot-hub-devguide-messaging.md).

## <a name="operations-monitoring"></a>Műveletek figyelése
Az IoT-központ [figyelési műveletek](iot-hub-operations-monitoring.md) lehetővé teszi az IoT hub valós idejű műveletek toomonitor hello állapotát. [Az IoT-központ](#iot-hub) nyomon követi az események között számos modulkategória közül műveletek. Dönthet úgy is, az események küldhetők a egy vagy több kategóriák tooan IoT-központ végpontjának feldolgozásra. Hello adatok hibák figyelése, vagy adatokat minták alapján összetettebb feldolgozás beállítása.

## <a name="physical-device"></a>Fizikai eszköz
Egy fizikai eszköz egy valódi eszköz, például egy málna Pi, amely összeköti az IoT-központ tooan. Az egyszerűség kedvéért hello IoT-központ oktatóanyagok számos használja [eszközök szimulált](#simulated-device) tooenable meg toorun minták a helyi számítógépen.

## <a name="primary-and-secondary-keys"></a>Az elsődleges és másodlagos kulcsok
Tooa eszköz irányuló csatlakoztatásakor vagy az IoT-központ szolgáltatás felé néző végpont a [kapcsolati karakterlánc](#connection-string) van hozzáférési kulcs toogrant tartalmazza. Amikor felvesz egy eszköz toohello [identitásjegyzékhez](#identity-registry) , vagy adja hozzá a [megosztott hozzáférési házirend](#shared-access-policy) tooyour hub, hello szolgáltatást egy elsődleges és másodlagos kulcsot hoz létre. A két kulcs lehetővé teszi tooroll keresztül az egyik kulcs tooanother toohello IoT-központ hozzáférés elvesztése nélkül a kulcs frissítésekor.

## <a name="protocol-gateway"></a>Protokoll-átjáró
A protokoll-átjáró általában hello felhőben üzemel, és túl csatlakozó eszközök fordítási szolgáltatásokat nyújt a protokoll[IoT-központ](#iot-hub). További információkért lásd: [Mi az Azure IoT Hub?](iot-hub-what-is-iot-hub.md)

## <a name="quotas-and-throttling"></a>Kvóták és szabályozás
Nincsenek a különböző [kvóták](iot-hub-devguide-quotas-throttling.md) , amely érvényes tooyour használatát [IoT-központ](#iot-hub), hány hello eltérő kvóták hello IoT-központ hello szintjének alapján. [Az IoT-központ](#iot-hub) is érvényes [azelőtt gyorsítja fel](iot-hub-devguide-quotas-throttling.md) futási időben tooyour hello szolgáltatás használatát.

## <a name="reported-configuration"></a>Jelentett konfiguráció
Hello környezetében egy [eszköz iker](iot-hub-devguide-device-twins.md), jelentett konfigurációs tulajdonságok teljes készletének toohello hivatkozik, és hello eszköz iker, amelyeket a metaadatokat jelentett toohello megoldás háttérrendszerének.

## <a name="reported-properties"></a>Jelentett tulajdonságai
Hello környezetében egy [eszköz iker](iot-hub-devguide-device-twins.md), egy alkalmazásra vonatkozó hello eszköz iker használt tulajdonságok jelentette [szükséges tulajdonságok](#desired-properties) toosynchronize eszköz konfigurációs vagy az állapot. Tulajdonságok csak akkor állítható hello által jelentett [eszközalkalmazás](#device-app) és olvashatók és által lekérdezett egy [háttér-alkalmazás](#back-end-app).

## <a name="resource-group"></a>Erőforráscsoport
[Az Azure Resource Manager](#azure-resource-manager) használ erőforrás csoportok toogroup kapcsolódó erőforrások együtt. Egy erőforrás csoport tooperform összes hello az erőforrásokon végrehajtott műveletek egyidejűleg is használhat a hello csoportban.

## <a name="retry-policy"></a>Ismételje meg a házirend
Használhat egy újrapróbálkozási házirend toohandle [átmeneti hibák](https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx) tooa felhőszolgáltatás csatlakoztatásakor.

## <a name="routing-rules"></a>Útválasztási szabályokat
Konfigurálja [útválasztási szabályok](iot-hub-devguide-messages-read-custom.md) az IoT hub tooroute eszköz a felhőbe küldött üzeneteket tooa a [beépített végpont](#built-in-endpoints) vagy túl[egyéni végpontokat](#custom-endpoints) feldolgozásra a megoldás háttérrendszere vége.

## <a name="sasl-plain"></a>SASL EGYSZERŰ
SASL egyszerű egy protokoll, amely hello [AMQP](#advanced-message-queue-protocol) protokoll tootransfer biztonsági jogkivonatokat használ.

## <a name="shared-access-signature"></a>Közös hozzáférésű jogosultságkódot
Megosztott hozzáférési aláírásokkal (SAS) olyan hitelesítési mechanizmus biztonságos SHA-256 kivonatokkal vagy URI-k alapján. SAS hitelesítési két részből áll: egy _megosztott elérési házirendet_ és egy _közös hozzáférésű Jogosultságkód_ (gyakran nevezik jogkivonat). Egy eszközt az IoT-központ SAS tooauthenticate használ. [Háttér-alkalmazások](#back-end-app) SAS tooauthenticate az IoT-központ szolgáltatás felé néző végpontok hello segítségével is. Általában szerepeltetni kívánt hello SAS-jogkivonat hello [kapcsolati karakterlánc](#connection-string) , hogy egy alkalmazás használ-e a kapcsolat tooan IoT-központ tooestablish.

## <a name="shared-access-policy"></a>Megosztott elérési házirendet
A megosztott elérési házirend meghatározása hello jogosultságaitól tooanyone, aki rendelkezik érvényes [elsődleges vagy másodlagos kulcsot](#primary-and-secondary-keys) házirendhez társított. A központ a hello kezelheti hello megosztott elérési házirendek és kulcsok [portal](#azure-portal).

## <a name="simulated-device"></a>Szimulált eszköz
Kényelmi célokat szolgál az IoT-központ oktatóanyagok használata hello számos szimulált eszköz tooenable meg toorun minták a helyi számítógépen. Ezzel szemben egy [fizikai eszköz](#physical-device) egy valós eszköz, például egy málna Pi tooan IoT-központ csatlakozó.

## <a name="solution"></a>Megoldás
A _megoldás_ hivatkozhat tooa Visual Studio megoldás, amely egy vagy több projekt tartalmazza. A _megoldás_ is utalhat tooan IoT-megoldás, amely tartalmazza az eszközök, például elemek [eszköz alkalmazások](#device-app), az IoT-központ, a más Azure-szolgáltatásokkal, és [háttér-alkalmazások](#back-end-app).

## <a name="subscription"></a>Előfizetés
Az Azure-előfizetésre, ha a számlázási történik. Minden Azure-erőforrás létrehozása, vagy használja az Azure szolgáltatás egy-egy előfizetéshez társítva. Sok kvóták is vonatkoznak az előfizetés hello szinten.

## <a name="system-properties"></a>Rendszertulajdonságok
Hello környezetében egy [eszköz iker](iot-hub-devguide-device-twins.md), rendszer tulajdonság csak olvasható, és utolsó tevékenység idő és a kapcsolat állapota például hello eszköz használatára vonatkozó információval.

## <a name="tags"></a>Címkék
Hello környezetében egy [eszköz iker](iot-hub-devguide-device-twins.md), címke található eszköz metaadatait tárolja, és által hello megoldás háttérrendszerének hello képernyőn egy JSON-dokumentum lekérése. Címke nem található látható tooapps az eszközön.

## <a name="telemetry"></a>Telemetria
Eszközök gyűjt telemetrikus adatokat, például a szél sebesség vagy hőmérséklet, és használjon [adatpont üzenetek](#data-point-messages) toosend hello telemetriai tooan IoT-központot.

## <a name="token-service"></a>Jogkivonat-szolgáltatás
Az eszközök egy jogkivonat-szolgáltatás tooimplement olyan hitelesítési módszert használhat. Az IoT-központ használ [megosztott hozzáférési házirend](#shared-access-policy) rendelkező **DeviceConnect** engedélyek toocreate *eszköz hatókörű* jogkivonatokat. Ezeket a jogkivonatokat engedélyezése az eszköz tooconnect tooyour IoT-központot. Egy eszközt használ, egy egyéni hitelesítési mechanizmus tooauthenticate hello biztonságijogkivonat-szolgáltatás. Hello eszköz sikeresen hitelesíti magát, ha a hello biztonságijogkivonat-szolgáltatás az IoT hub kibocsát egy SAS-jogkivonat hello eszköz toouse tooaccess.

## <a name="x509-client-certificate"></a>Ügyfél X.509-tanúsítvány
Egy eszköz használható egy X.509 tanúsítvány tooauthenticate rendelkező [IoT-központ](#iot-hub). Egy X.509 tanúsítvány használata egy alternatív toousing egy [SAS-jogkivonat](#shared-access-signature).
