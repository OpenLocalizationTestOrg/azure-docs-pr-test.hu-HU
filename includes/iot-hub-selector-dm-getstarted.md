> [!div class="op_single_selector"]
> * [Eszköz: Node.js Szolgáltatás: Node.js.](../articles/iot-hub/iot-hub-node-node-device-management-get-started.md)
> * [Eszköz: Node.js Szolgáltatás: C#](../articles/iot-hub/iot-hub-csharp-node-device-management-get-started.md)
> * [Eszköz: Java szolgáltatás: Java](../articles/iot-hub/iot-hub-java-java-device-management-getstarted.md)

Háttér-alkalmazások használhatják Azure IoT Hub primitívek, például a [eszköz iker] [ lnk-devtwin] és [módszerek közvetlen][lnk-c2dmethod], tooremotely elindítása és figyelése eszköz felügyeleti műveleteket az egyes eszközökön. Az oktatóanyag bemutatja, hogyan egy háttér-alkalmazást és egy eszköz alkalmazásának tooinitiate együttműködése és a távoli eszköz újra kell indítani az IoT-központ használatával figyelheti.

A közvetlen módszer tooinitiate eszköz felügyeleti műveletek (például a rendszer újraindítása, a gyári beállítások visszaállítása és vezérlőprogram-frissítés) használata egy háttér-alkalmazásból hello felhőben. hello eszköz felelős:

* Az IoT-központ által küldött hello metódus kérelem kezelése.
* Kezdeményezi a megfelelő eszközre vonatkozó művelet hello hello eszközön.
* Állapot frissítéseket biztosító *tulajdonságok jelentett* tooIoT központ.

Hello felhő toorun eszköz iker lekérdezések tooreport hello előrehaladását, hogy az eszköz felügyeleti műveletek is használhatja a háttér-alkalmazást.

[lnk-devtwin]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
