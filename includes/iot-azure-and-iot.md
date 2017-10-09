
# <a name="azure-and-internet-of-things"></a>Az Azure és az eszközök internetes hálózata

Üdvözli az tooMicrosoft Azure, és az eszközök internetes hálózatát (IoT) hello. Ez a cikk egy IoT-megoldásarchitektúra Azure-szolgáltatások használatával telepíthet egy IoT-megoldás gyakori jellemzőit hello írja le, be. Az IoT-megoldások szükséges biztonságos, kétirányú kommunikáció eszközök, amelyek száma akár a több millió hello és a megoldás háttérrendszeréhez között. A megoldás háttérrendszeréhez például az eszköz-felhő eseménystreambe fogja használni automatizált, prediktív elemzések toouncover insights.

Az Azure IoT Hub kulcsfontosságú építőelem a jelen IoT-megoldásarchitektúra Azure-szolgáltatásokkal történő megvalósítása során, az IoT Suite programcsomag pedig biztosítja a jelen architektúra teljes körű megvalósítását bizonyos IoT-forgatókönyvek esetén. Példa:

* Hello *távoli megfigyelési* a megoldás lehetővé teszi az eszközök, például eladóautomaták toomonitor hello állapotát.
* Hello *prediktív karbantartási* megoldással tooanticipate karbantartási igényeire eszközök például távoli szivattyútelepek és tooavoid nem tervezett leállás szivattyúk.
* Hello *csatlakoztatott gyári* megoldás tooconnect nyújt segítséget, és az ipari eszközök figyelésére.

## <a name="iot-solution-architecture"></a>Az IoT-megoldásarchitektúra

hello a következő ábrán egy tipikus IoT-megoldásarchitektúra látható. hello diagram nem tartalmaz egyetlen konkrét Azure-szolgáltatás hello nevét, de hello egy általános IoT-megoldásarchitektúra kulcselemeit ismerteti. Ebben az architektúrában az IoT-eszközök adatgyűjtést, hogy az általuk küldött tooa felhőátjáróhoz. hello felhőátjáróhoz elérhetővé hello adatok más háttér-szolgáltatásaihoz, ahol kerülnek az adatok tooother-üzleti alkalmazások vagy toohuman operátorok egy irányítópultot, vagy más bemutató eszközön keresztül a általi feldolgozás alatt.

![Az IoT-megoldásarchitektúra][img-solution-architecture]

> [!NOTE]
> Egy IoT-architektúra részletes ismertetéséhez lásd: hello [Microsoft Azure IoT-Referenciaarchitektúra][lnk-refarch].

### <a name="device-connectivity"></a>Eszközkapcsolatok

Az IoT-megoldásarchitektúra, az eszközök telemetriát, például az érzékelő szivattyútelepek érzékelőinek adatai egy szivattyútelep, tooa felhővégpontnak tárolás és feldolgozás. A prediktív karbantartási forgatókönyvben hello megoldás háttérrendszerének előfordulhat, hogy használja az érzékelő adatokat toodetermine hello adatfolyam, ha egy adott szivattyú igényel karbantartást. Eszközök is kap, és válaszolhat toocloud-eszközre küldött üzenetek üzenetek olvasásakor a felhővégpontnak. Például hello prediktív karbantartási forgatókönyvben hello megoldásban háttér előfordulhat, hogy üzenetek küldése tooother szivattyúk állomás toobegin adatfolyamok átirányításához csak előtt, az adatok kiolvasása hello toostart. Ez az eljárás akkor ellenőrizze, hogy hello karbantartó mérnök sikerült első lépései, amint megérkeznek ő.

Hello legnagyobb kihívást az IoT-projektek egyik hogyan tooreliably és biztonságosan csatlakozzon az eszközök toohello megoldás háttérrendszerének. Az IoT-eszközök más jellemzőkkel rendelkeznek, például böngészők vagy mobilalkalmazások összehasonlított tooother ügyfélként. IoT-eszközök:

* Általában beágyazott, emberi beavatkozást nem igénylő rendszerek.
* Távoli helyeken is üzembe helyezhetők, ahol a fizikai hozzáférés drága lenne.
* Csak akkor hello megoldás háttérrendszerének keresztül érhető el. Nincs semmilyen más módon toointeract hello eszközzel.
* Áramellátásuk és feldolgozási erőforrásaik korlátozottak lehetnek.
* A hálózati kapcsolat időszakos, lassú vagy drága lehet.
* Esetleg toouse saját fejlesztésű, egyedi vagy iparág-specifikus alkalmazás-protokollokra.
* Számos népszerű hardver- és szoftverplatform használatával létrehozhatók.

Ezenkívül toohello a fenti követelmények minden IoT-megoldás kell is biztosítanak méretezési, biztonságot és megbízhatóságot. hello eredő hálózati kapcsolati követelményeinek nehéz és időigényes tooimplement a hagyományos technológiái, például a webes tárolók vagy üzenetkezelő közvetítők. Azure IoT Hub és hello Azure IoT eszközoldali SDK-k révén könnyebben tooimplement megoldásokat, amely megfelel a fenti követelményeknek.

Eszközök közvetlenül kommunikálhatnak egy végpontjaival, vagy hello eszközt sem hello hello felhő átjáró által támogatott kommunikációs protokollok használható, ha azt kapcsolódhatnak egy köztes átjáróhoz. Például hello [Azure IoT protokoll-átjáró] [ lnk-protocol-gateway] protokollfordításhoz hajthat végre, ha az eszközök nem használható, amely támogatja az IoT-központ hello protokollokat.

### <a name="data-processing-and-analytics"></a>Adatfeldolgozás és -elemzés

Hello felhőben az IoT-megoldás háttérrendszerének, ahol hello adatok feldolgozása a legtöbb következik be, például a szűrést és telemetriák és összegzésére, valamint tooother szolgáltatások. az IoT-megoldás háttérrendszerének hello:

* Telemetria léptékű kap az eszközök, és meghatározza, hogy hogyan tooprocess és tárolja ezeket az adatokat. 
* Lehetővé teheti a toosend parancsok hello felhő toospecific eszközről.
* Eszközregisztrációs képességeket, amelyek lehetővé teszik biztosít tooprovision eszközök és mely eszközök engedélyezett tooconnect tooyour infrastruktúra toocontrol.
* Lehetővé teszi, hogy tootrack hello az eszközök állapotának és tevékenységeik megfigyelését.

Hello prediktív karbantartási forgatókönyvben hello megoldás háttérrendszerének tárolja a korábbi telemetriai adatokat. hello megoldás háttérrendszerének használható az adatok toouse tooidentify jelző minták karbantartási az egy adott szivattyú.

Az IoT-megoldások tartalmazhatnak automatikus visszajelzési hurkokat is. Például egy hello megoldás háttérrendszerének analytics moduljának azonosíthatja a telemetriai adatokból, amely hello egy adott eszköz hőmérséklete a normális üzemi szint fölött van. hello megoldás is elküldheti parancs toohello eszköz, elemzőmodulja tootake kiigazító intézkedéseket.

### <a name="presentation-and-business-connectivity"></a>Megjelenítés és üzleti kapcsolatok

hello megjelenítési és üzleti kapcsolati réteg lehetővé teszi, hogy a végfelhasználók az IoT-megoldás hello toointeract és hello eszközök. Lehetővé teszi, hogy a felhasználók tooview, és az eszközeikről összegyűjtött hello elemzéséhez. Ezek a nézetek hello űrlap irányítópultokon vagy BI-jelentéseket, amellyel megjeleníthetők mindkét előzményadatok vagy közel valós idejű adatokat is igénybe vehet. Például az operátor ellenőrizhesse hello adott szivattyútelep állapotát és lásd: hello rendszer által kiadott riasztásokat. Ez a réteg emellett lehetővé teszi hello IoT megoldás háttérrendszerének integrációját meglévő-üzleti alkalmazások tootie a vállalat üzleti vagy munkafolyamatokba. Például hello prediktív karbantartási megoldás integrálható egy ütemezési rendszerbe, hogy könyvek egy visszafejtés toovisit egy szivattyútelep amikor hello megoldás megállapítja, hogy valamelyik szivattyú karbantartásra szorul.

![Az IoT-megoldás irányítópultja][img-dashboard]

[img-solution-architecture]: ./media/iot-azure-and-iot/iot-reference-architecture.png
[img-dashboard]: ./media/iot-azure-and-iot/iot-suite.png

[lnk-machinelearning]: http://azure.microsoft.com/documentation/services/machine-learning/
[Azure IoT Suite]: http://azure.microsoft.com/solutions/iot
[lnk-protocol-gateway]:  ../articles/iot-hub/iot-hub-protocol-gateway.md
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
