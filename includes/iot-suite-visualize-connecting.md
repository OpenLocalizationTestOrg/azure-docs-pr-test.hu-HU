## <a name="view-device-telemetry-in-hello-dashboard"></a>Nézet telemetriát hello irányítópult
a távoli figyelési megoldás lehetővé teszi, hogy Ön tooview hello telemetriai az eszközök elküldik tooIoT Hub hello irányítópult hello.

1. A böngészőben, visszatérési toohello távoli figyelési megoldást irányítópulton kattintson **eszközök** a hello bal oldali panelen toonavigate toohello **eszközök listában**.
2. A hello **eszközök listában**, láthatja, hogy van-e az eszköz állapotának hello **futtató**. Ha nem, kattintson a **eszköz engedélyezése** a hello **eszközadatok** panel.
   
    ![Eszközállapot megtekintése][18]
3. Kattintson a **irányítópult** tooreturn toohello irányítópultot, válassza ki az eszköz hello **eszköz tooView** legördülő tooview a telemetriai adatok. hello telemetriai hello mintaalkalmazást a belső hőmérséklet 50 egységek, a külső hőmérséklet 55 mértékegységét és a páratartalom 50 egység.
   
    ![Eszköztelemetria megtekintése][img-telemetry]

## <a name="invoke-a-method-on-your-device"></a>Metódus meghívása az eszközön
hello irányítópult hello távoli figyelési megoldás lehetővé teszi tooinvoke módszerek a eszközökön az IoT-központ használatával. Például a távoli felügyeleti megoldás hello hívhat meg a metódus toosimulate eszköz újraindul.

1. Hello távoli figyelési megoldást irányítópulton kattintson **eszközök** a hello bal oldali panelen toonavigate toohello **eszközök listában**.
2. Kattintson a **Eszközazonosító** az eszközt hello **eszközök listában**.
3. A hello **eszközadatok** panelen, kattintson a **módszerek**.
   
    ![Eszközmetódusok][13]
4. A hello **metódus** legördülő listából válassza **InitiateFirmwareUpdate**, majd a **FWPACKAGEURI** meg egy üres URL-címet. Kattintson a **metódus meghívása** toocall hello metódus hello eszközön.
   
    ![Eszközmetódus meghívása][14]
   

5. Megjelenik egy üzenet, a kód fut, amikor az eszköz hello hello metódus hello konzolon. hello metódus hello eredményei toohello előzmények hello megoldás portal jelentek meg:

    ![Metódus előzményeinek megtekintése][img-method-history]

## <a name="next-steps"></a>Következő lépések
hello cikk [testreszabása, előre konfigurált megoldások] [ lnk-customize] néhány bővítheti ezt a mintát módját ismerteti. A lehetséges bővítmények közé tartozik a valódi érzékelők használata és további parancsok implementálása.

További tudnivalók hello [hello azureiotsuite.com hely engedélyeinek][lnk-permissions].

[13]: ./media/iot-suite-visualize-connecting/suite4.png
[14]: ./media/iot-suite-visualize-connecting/suite7-1.png
[18]: ./media/iot-suite-visualize-connecting/suite10.png
[img-telemetry]: ./media/iot-suite-visualize-connecting/telemetry.png
[img-method-history]: ./media/iot-suite-visualize-connecting/history.png
[lnk-customize]: ../articles/iot-suite/iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-permissions]: ../articles/iot-suite/iot-suite-permissions.md
