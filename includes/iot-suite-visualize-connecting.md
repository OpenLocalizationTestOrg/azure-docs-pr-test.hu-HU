## <a name="view-device-telemetry"></a>Eszköztelemetria megtekintése

Az eszközről küldött telemetriai adatok megtekintheti a **eszközök** lap a megoldásban.

1. Válassza ki az eszközt, az eszközök listájában kiépítve a **eszközök** lap. A panel az eszközt, beleértve a telemetriát rajzot információit jeleníti meg:

    ![Tekintse meg az eszköz részletei](media/iot-suite-visualize-connecting/devicesdetail.png)

1. Válasszon **nyomás** telemetriai megjelenítésének módosítása:

    ![Nézet nyomás telemetriai adat](media/iot-suite-visualize-connecting/devicespressure.png)

1. Az eszköz diagnosztikai adatainak megtekintésére, görgessen le a **diagnosztika**:

    ![Nézet eszköz diagnosztika](media/iot-suite-visualize-connecting/devicesdiagnostics.png)

## <a name="act-on-your-device"></a>Az eszközön működésre

Az eszközök metódusok meghívása, használja a **eszközök** lap a távoli felügyeleti megoldás. Például a távoli felügyeleti megoldás a **hűtő** eszközök valósítja meg a **újraindítás** metódust.

1. Válasszon **eszközök** lehetőségre, és navigáljon a **eszközök** lap a megoldásban.

1. Válassza ki az eszközt, az eszközök listájában kiépítve a **eszközök** lap:

    ![Válassza ki a fizikai eszköz](media/iot-suite-visualize-connecting/devicesselect.png)

1. A metódusok hívása az eszközön listájának megjelenítéséhez kattintson **ütemezés**. Ütemezés módszerét több eszközön futtassa, jelölje ki több eszközre a listában. A **ütemezés** panelen láthatók metódus általános a kiválasztott eszközök.

1. Válasszon **újraindítás**, a feladat neve **RebootPhysicalChiller**, és válassza a **alkalmaz**:

    ![Az Újraindítás ütemezése](media/iot-suite-visualize-connecting/deviceschedule.png)

1. Egy üzenet jelenik meg a konzolon, a kód fut, amikor az eszköz a metódust.

> [!NOTE]
> A megoldásban a feladat állapotának nyomon követése, válassza a **nézet**.

## <a name="next-steps"></a>Következő lépések

A cikk [testre szabhatja a távoli felügyeleti előkonfigurált megoldás](../articles/iot-suite/iot-suite-remote-monitoring-customize.md) testre szabhatja az előkonfigurált megoldás néhány módját ismerteti.