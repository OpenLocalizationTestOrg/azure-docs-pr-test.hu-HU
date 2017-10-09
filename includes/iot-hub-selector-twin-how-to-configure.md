> [!div class="op_single_selector"]
> * [Node.js](../articles/iot-hub/iot-hub-node-node-twin-how-to-configure.md)
> * [C#/node.js](../articles/iot-hub/iot-hub-csharp-node-twin-how-to-configure.md)
> * [C#](../articles/iot-hub/iot-hub-csharp-csharp-twin-how-to-configure.md)
> 
> 

## <a name="introduction"></a>Bevezetés

A [Ismerkedés az IoT Hub eszköz twins][lnk-twin-tutorial], megtudta, hogyan tooset eszköz metaadatait a megoldás háttérrendszere a befejező használata *címkék*, jelentést eszköz feltételek egy eszköz alkalmazásból használatával *tulajdonságok jelentett*, és lekérdezheti az SQL-szerű nyelv használatával adatokat.

Ebből az oktatóanyagból megtudhatja, hogyan toouse hello hello eszköz iker *szükséges tulajdonságok* együtt *tulajdonságok jelentett*, tooremotely eszköz alkalmazások konfigurálása. Pontosabban Ez az oktatóanyag bemutatja, hogyan egy eszköz iker jelentett és kívánt tulajdonságok engedélyezése egy alkalmazást többlépéses konfigurálása, és hello látható toohello megoldás háttérrendszerének művelet hello állapotáról biztosít minden eszköz. Eszközkonfigurációk a hello szerepkör kapcsolatos további információkat is megtalálhatja [IoT-központ az eszközkezelés áttekintése][lnk-dm-overview].

Magas szinten az eszköz twins használata lehetővé hello megoldás háttér toospecify hello szükségeskonfiguráció hello felügyelt eszközök esetén a konkrét parancsok küldése helyett. Ez helyezi hello eszköz beállítása hello legjobb módja tooupdate konfigurációjában (nagyon fontos, IoT forgatókönyvekben, ahol adott eszközhöz feltételek hatással hello képességét tooimmediately végeznek konkrét parancsok) feladata folyamatosan reporting toohello közben megoldás háttérrendszere hello aktuális állapot és a lehetséges hibaállapotok hello frissítési folyamat befejezéséhez. Ebben a mintában műszeres toohello felügyeleti eszközöket, a nagy megegyezik lehetővé teszi, hogy hello megoldás háttér toohave teljes látható-e hello konfigurációs folyamat hello állapotát az összes eszközön.

> [!NOTE]
> Olyan esetekben, ahol vezérelt eszközök több interaktív módon (egy felhasználó által felügyelt alkalmazásból ventilátor bekapcsolása), érdemes lehet [módszerek közvetlen][lnk-methods].
> 
> 

Ebben az oktatóanyagban hello megoldás háttér módosítások hello telemetriai konfigurációs a céleszközön, és, hogy a hello eszközalkalmazás miatt egy folyamat tooapply konfiguráció követi (például a számítógép újraindítására szoftver modul, amely ezt frissítése az oktatóanyag szimulálja egyszerű késéssel).

hello megoldás háttérrendszerének tárol hello eszköz iker kívánt tulajdonságokkal a következő módon hello hello konfigurálása:

        {
            ...
            "properties": {
                ...
                "desired": {
                    "telemetryConfig": {
                        "configId": "{id of hello configuration}",
                        "sendFrequency": "{config}"
                    }
                }
                ...
            }
            ...
        }

> [!NOTE]
> Konfigurációk lehetnek összetett objektumra, mert általában hozzárendeli egyedi azonosítók (kivonatok vagy [GUID][lnk-guid]) toosimplify az összehasonlítást.
> 
> 

hello eszközalkalmazás jelentések szükséges hello tulajdonság tükrözés aktuális konfigurációja **telemetryConfig** hello a jelentett tulajdonságok:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of hello current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Success",
                    }
                }
                ...
            }
        }

Megjegyzés: a hogyan hello **telemetryConfig** további tulajdonsága **állapot**, használt tooreport hello hello konfigurációs frissítési folyamat állapotát.

Amikor egy új szükségeskonfiguráció érkezik, hello eszközalkalmazás egy függőben lévő konfigurációs hello információk módosításával jelentések:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of hello current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Pending",
                        "pendingConfig": {
                            "changeId": "{id of hello pending configuration}",
                            "sendFrequency": "{pending configuration}"
                        }
                    }
                }
                ...
            }
        }

Ezt követően egy későbbi időpontban hello eszközalkalmazás jelentést hello sikeres vagy sikertelen volt a művelet hello fent tulajdonság frissítése.
Vegye figyelembe, hogyan hello megoldás háttérrendszerének képes, bármikor hello konfigurációs folyamat összes hello eszközön tooquery hello állapotát.

Ez az oktatóanyag a következőket mutatja be:

* Létrehoz egy szimulált eszköz alkalmazást, amely konfigurációs frissítések kap hello megoldás háttérrendszeréhez, és több frissítések jelentések *tulajdonságok jelentett* hello konfiguráció folyamatot nem lehet frissíteni.
* Létrehoz egy háttér-alkalmazást, hogy a frissítések hello szükségeskonfiguráció egy eszköz, és majd lekérdezések hello konfigurációs frissítési folyamat.

<!-- links -->

[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: ../articles/iot-hub/iot-hub-device-management-overview.md
[lnk-twin-tutorial]: ../articles/iot-hub/iot-hub-node-node-twin-getstarted.md
[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier
