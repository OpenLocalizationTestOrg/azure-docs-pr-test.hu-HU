## <a name="create-an-iot-hub"></a>IoT Hub létrehozása
A szimulált eszköz alkalmazás tooconnect, hogy az IoT hub létrehozása. hello következő lépések bemutatják, hogyan toocomplete ennek a feladatnak a által használatával hello Azure-portálon.

1. Jelentkezzen be toohello [Azure-portálon][lnk-portal].
1. Hello Ugrósávon, kattintson **új** > **az eszközök internetes hálózatát** > **IoT-központ**.
   
    ![Azure Portal – ugrósáv][1]
1. A hello **IoT-központ** panelen válassza ki az IoT hub hello konfigurációját.
   
    ![IoT Hub panel][2]
   
   1. A hello **neve** mezőbe írja be az IoT hub nevét. Ha hello **neve** érvényes és elérhető, egy zöld pipa jelenik meg hello **neve** mezőbe.
    [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]
   
   1. Válasszon ki egy [tarifacsomagot és méretet][lnk-pricing]. Az oktatóanyag teljesítéséhez nem kell egy konkrét csomagot kiválasztani. Ebben az oktatóanyagban hello ingyenes F1 csomagot használja.
   1. Az **Erőforráscsoport** mezőben hozzon létre egy erőforráscsoportot, vagy válasszon ki egy meglévőt. További információkért lásd: [erőforrás segítségével csoportosítja toomanage az Azure-erőforrások][lnk-resource-groups].
   1. A **hely**, válassza ki hello hely toohost az IoT hub. A jelen oktatóanyag esetében válassza az Önhöz legközelebb eső helyet.
1. Az IoT Hub konfigurációs beállításainak kiválasztása után kattintson a **Létrehozás** gombra.  Az IoT hub az Azure toocreate néhány percig is tarthat. toocheck hello állapotát, figyelheti a Kezdőpulton hello vagy hello értesítések panelen hello előrehaladását.
   
    ![Az új IoT Hub állapota][3]
1. Ha hello IoT hub létrehozása sikeres, kattintson a hello új csempe az IoT hub hello Azure portál tooopen hello panelen hello új IoT hub. Jegyezze fel a hello **állomásnév**, és kattintson a **megosztott elérési házirendek**.
   
    ![Új IoT Hub panel][4]
1. A hello **megosztott elérési házirendek** panelen kattintson hello **iothubowner** házirend, majd másolja ki és jegyezze fel az IoT-központ kapcsolati karakterlánc hello hello a **iothubowner** panelen. További információkért lásd: [hozzáférés-vezérlés] [ lnk-access-control] a hello "IoT Hub developer guide."
   
    ![Megosztott hozzáférési házirend panel][5]

<!-- Images. -->
[1]: ./media/iot-hub-get-started-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-get-started-create-hub/create-iot-hub2.png
[3]: ./media/iot-hub-get-started-create-hub/create-iot-hub3.png
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-resource-groups]: ../articles/azure-resource-manager/resource-group-portal.md
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide-security.md
