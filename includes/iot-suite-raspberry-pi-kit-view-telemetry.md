## <a name="view-hello-telemetry"></a>Nézet hello telemetriai adat

hello málna Pi most küld telemetriai toohello távoli figyelési megoldást igényelnek. Hello telemetriai hello megoldás irányítópultján tekintheti meg. Üzenetek tooyour málna Pi hello megoldás irányítópultról is küldhet.

- Keresse meg a toohello megoldás irányítópultja.
- Válassza ki az eszköz hello **eszköz tooView** legördülő menüből.
- a hello hello telemetriai málna Pi hello irányítópulton jeleníti meg.

![Hello málna Pi a telemetriai adatok megjelenítéséhez][img-telemetry-display]

## <a name="act-on-hello-device"></a>Hello eszközön működésre

Hello megoldás irányítópultról módszerek hívhatók meg a málna Pi. Hello málna Pi csatlakozáskor toohello távoli figyelési megoldást igényelnek, elküldi az támogatja-e hello módszerekkel kapcsolatos információk.

- Hello megoldás irányítópulton kattintson **eszközök** toovisit hello **eszközök** lap. Válassza ki a málna Pi hello **eszközlista**. Válassza a **módszerek**:

    ![Az irányítópult eszközök][img-list-devices]

- A hello **metódus meghívása** lapon, válassza ki **LightBlink** a hello **metódus** legördülő menüből.

- Válasszon **InvokeMethod**. hello LED málna Pi tokenkódot többször toohello csatlakoztatva. hello málna Pi hello alkalmazás küld egy visszaigazoló hátsó toohello megoldás irányítópultja:

    ![Módszer előzmények megjelenítése][img-method-history]

- Megváltoztathatja az hello LED be- és kikapcsolását hello segítségével **ChangeLightStatus** metódust egy **LightStatusValue** túl beállítása**1** a a vagy **0** a ki.

> [!WARNING]
> Ha nem adja meg hello távoli felügyeleti megoldás az Azure-fiókjával fut, a hello futásakor kell fizetni. Hello távoli figyelési megoldást futtatása közben felhasználás csökkentése kapcsolatos további információkért lásd: [konfigurálása Azure IoT Suite megoldások bemutató céljára előre konfigurált][lnk-demo-config]. Ha befejezte, használja előre konfigurált hello megoldás törlése az Azure-fiókjával.


[img-telemetry-display]: media/iot-suite-raspberry-pi-kit-view-telemetry/telemetry.png
[img-list-devices]: media/iot-suite-raspberry-pi-kit-view-telemetry/listdevices.png
[img-method-history]: media/iot-suite-raspberry-pi-kit-view-telemetry/methodhistory.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md