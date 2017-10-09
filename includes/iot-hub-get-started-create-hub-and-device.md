## <a name="create-an-iot-hub"></a>IoT Hub létrehozása

1. A hello [Azure-portálon](https://portal.azure.com/), kattintson a **új** > **az eszközök internetes hálózatát** > **IoT-központ**.

   ![Hello Azure-portálon az IoT hub létrehozása](../articles/iot-hub/media/iot-hub-create-hub-and-device/1_create-azure-iot-hub-portal.png)
2. A hello **IoT-központ** panelen adja meg a következő adatokat az IoT hub hello:

     **Név**: Adja meg az IoT hub hello nevét. Ha hello név érvényes, egy zöld pipa jelenik meg.

     **Tarifa- és méretcsomag**: Select hello **F1 - szabad** réteg. Ebben a demóban elegendő ezt a beállítást megadni. További információkért lásd: hello [árazás és méretcsomag](https://azure.microsoft.com/pricing/details/iot-hub/).

     **Erőforráscsoport**: létrehoz egy erőforrás csoport toohost hello IoT-központot, vagy használjon egy meglévőt. További információkért lásd: [használata erőforráscsoportokat toomanage az Azure-erőforrások](../articles/azure-resource-manager/resource-group-portal.md).

     **Hely**: válassza ki a legközelebbi helyen tooyou hello IoT-központ létrehozási helyének hello.

     **PIN-kód toodashboard**: válassza ezt a beállítást, mennyire egyszerű a hozzáférés tooyour IoT hub hello irányítópulton.

   ![Adjon meg információt toocreate az IoT hub](../articles/iot-hub/media/iot-hub-create-hub-and-device/2_fill-in-fields-for-azure-iot-hub-portal.png)

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

3. Kattintson a **Create** (Létrehozás) gombra. Az IoT hub eltarthat néhány percig toocreate. Láthatja, hogy a folyamat állapotát a hello **értesítések** ablaktáblán.

   ![Az IoT Hub folyamatértesítéseinek megtekintése](../articles/iot-hub/media/iot-hub-create-hub-and-device/3_notification-azure-iot-hub-creation-progress-portal.png)

4. Az IoT hub létrehozása után kattintson rá a hello irányítópulton. Jegyezze fel a hello **állomásnév**, és kattintson a **megosztott elérési házirendek**.

   ![Az IoT hub hello nevének beolvasása](../articles/iot-hub/media/iot-hub-create-hub-and-device/4_get-azure-iot-hub-hostname-portal.png)

5. A hello **megosztott elérési házirendek** ablaktáblán hello kattintson **iothubowner** házirend, és ezután másolása és jegyezze fel az hello **kapcsolati karakterlánc** az IoT hub. További információkért lásd: [vezérlő hozzáférés tooIoT Hub](../articles/iot-hub/iot-hub-devguide-security.md).

> [!NOTE] 
Ennek a beállítási oktatóanyagnak az elvégzéséhez szüksége lesz erre az iothubowner kapcsolati karakterláncra. Azonban szükség lehet az egyes hello oktatóanyagok a különböző IoT-forgatókönyvek esetén a telepítés befejezése után.

   ![Az IoT Hub kapcsolati karakterláncának beszerzése](../articles/iot-hub/media/iot-hub-create-hub-and-device/5_get-azure-iot-hub-connection-string-portal.png)

## <a name="register-a-device-in-hello-iot-hub-for-your-device"></a>Eszköz regisztrálása az IoT-központ hello az eszközhöz

1. A hello [Azure-portálon](https://portal.azure.com/), nyissa meg az IoT hub.

2. Kattintson a **Device Explorer** elemre.
3. Hello eszköz Explorer ablaktáblában kattintson **Hozzáadás** tooadd eszköz tooyour IoT-központot. Majd hello a következő:

   **Eszközazonosító**: Adja meg az új eszköz hello hello Azonosítóját. Az eszközazonosítókban különbözőnek számítanak a kis- és nagybetűk.

   **Hitelesítés típusa**: Válassza a **Szimmetrikus kulcs** lehetőséget.

   **Kulcsok automatikus létrehozása**: Jelölje be ezt a jelölőnégyzetet.

   **Eszköz tooIoT központi csatlakozás**: kattintson a **engedélyezése**.

   ![Hozzáad egy eszközt a hello eszköz Explorer, az IoT hub](../articles/iot-hub/media/iot-hub-create-hub-and-device/6_add-device-in-azure-iot-hub-device-explorer-portal.png)

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

4. Kattintson a **Save** (Mentés) gombra.
5. Hello eszköz létrehozását követően nyissa meg hello eszköz hello **eszköz Explorer** ablaktáblán.
6. Jegyezze fel az elsődleges kulcs hello hello kapcsolati karakterlánc.

   ![Hello eszköz kapcsolati karakterlánc beolvasása](../articles/iot-hub/media/iot-hub-create-hub-and-device/7_get-device-connection-string-in-device-explorer-portal.png)
