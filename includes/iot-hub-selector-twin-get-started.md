> [!div class="op_single_selector"]
> * [Node.js](../articles/iot-hub/iot-hub-node-node-twin-getstarted.md)
> * [C#/node.js](../articles/iot-hub/iot-hub-csharp-node-twin-getstarted.md)
> * [C#](../articles/iot-hub/iot-hub-csharp-csharp-twin-getstarted.md)
> * [Java](../articles/iot-hub/iot-hub-java-java-twin-getstarted.md)

Az ikereszközök JSON-dokumentumok, amelyek az eszközök állapotinformációit (metaadatokat, konfigurációkat és állapotokat) tárolják. Az IoT-központ továbbra is fennáll, egy eszköz iker tooit csatlakozó eszközökről.

Az eszköz twins használja:

* A megoldás háttérből eszköz metaadatait tárolja.
* Például a rendelkezésre álló lehetőségeket, és a feltételek (például hello kapcsolat használt módszer) aktuális állapotadatokat jelentést az eszköz alkalmazásból.
* Egy eszköz alkalmazás és a háttér-alkalmazások között (például a belső vezérlőprogram és konfigurációja frissítések) hosszan futó munkafolyamatok hello állapotának szinkronizálása.
* Az eszköz metaadatait, konfigurációs vagy az állapot lekérdezése.

> [!NOTE]
> Eszköz twins úgy tervezték, a szinkronizálás és eszköz-konfigurációk és a kikötések lekérdezése. Ha toouse eszköz twins itt található: a további informations [eszköz twins megértéséhez][lnk-twins].

Eszköz twins az IoT-központ tárolódnak, és tartalmazza:

* *címkék*, csak hello megoldás háttérrendszerének; által elérhető eszköz metaadatait
* *szükségeskonfiguráció-tulajdonságok*, hello megoldás által módosítható JSON-objektumok biztonsági vége és megfigyelhető hello eszközalkalmazás; és
* *Tulajdonságok jelentett*, JSON-objektumok hello eszköz alkalmazás által módosítható és hello megoldás háttérrendszerének által is olvasható. Címkék és a Tulajdonságok tömb nem tartalmazhat, de az objektumok egymásba ágyazható.

![][img-twin]

Emellett hello megoldás háttérrendszerének lekérdezheti az eszköz twins összes hello fenti adatok alapján.
Tekintse meg a túl[eszköz twins megértéséhez] [ lnk-twins] további információt az eszköz twins és toohello [IoT-központ lekérdezési nyelv] [ lnk-query] referencia a lekérdezésre.

> [!NOTE]
> Ilyenkor eszköz twins elérhetők, csak az eszközök, tooIoT központi csatlakozás hello MQTT protokoll használatával. Tekintse meg a toohello [MQTT támogatási] [ lnk-devguide-mqtt] a cikk útmutatást tooconvert meglévő eszköz alkalmazás toouse MQTT.

Ez az oktatóanyag a következőket mutatja be:

* Hozzon létre egy háttér-alkalmazást, amely *címkék* tooa eszköz iker, és a szimulált eszköz alkalmazást, amely kapcsolatát jelentések csatorna egy *tulajdonság jelentett* a hello eszköz iker.
* A háttér-alkalmazás szűrők használata a hello címkék és a korábban létrehozott tulajdonságok eszközök lekérdezése.

<!-- images -->
[img-twin]: media/iot-hub-selector-twin-get-started/twin.png

<!-- links -->
[lnk-query]: ../articles/iot-hub/iot-hub-devguide-query-language.md
[lnk-twins]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-d2c]: ../articles/iot-hub/iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: ../articles/iot-hub/iot-hub-mqtt-support.md