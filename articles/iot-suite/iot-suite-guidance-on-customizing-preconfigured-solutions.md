---
title: "aaaCustomizing előre konfigurált megoldások |} Microsoft Docs"
description: "Hogyan toocustomize hello Azure IoT Suite előre konfigurált megoldásokat nyújt útmutatást."
services: 
suite: iot-suite
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 4653ae53-4110-4a10-bd6c-7dc034c293a8
ms.service: iot-suite
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2017
ms.author: corywink
ms.openlocfilehash: 1a8573f5ac6ed944c44459df495446f15174d513
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="customize-a-preconfigured-solution"></a>Előre konfigurált megoldás testreszabása

hello Azure IoT Suite előre konfigurált hello megoldásai hello szolgáltatások belül hello suite működik együtt toodeliver egy végpont megoldás bemutatása. A kiindulási pont, a különböző helyek, amelyeknek kiterjesztése és testre szabhatja az adott forgatókönyveket hello megoldás vannak. hello a következő szakaszok ismertetik a közös testreszabási pontok.

## <a name="find-hello-source-code"></a>Hello Forráskód keresése

előre konfigurált hello megoldások hello forráskódja érhető el a Githubon hello tárházak találhatók a következő:

* Távoli megfigyelési: [https://www.github.com/Azure/azure-iot-remote-monitoring](https://github.com/Azure/azure-iot-remote-monitoring)
* A prediktív karbantartási: [https://github.com/Azure/azure-iot-predictive-maintenance](https://github.com/Azure/azure-iot-predictive-maintenance)
* Csatlakoztatott gyári: [https://github.com/Azure/azure-iot-connected-factory](https://github.com/Azure/azure-iot-connected-factory)

előre konfigurált hello megoldások hello forráskódja kerül a toodemonstrate hello minták és gyakorlatok használt tooimplement hello végpont funkciók egy IoT-megoldás Azure IoT Suite használata. További információ található toobuild és központi telepítése a GitHub-adattárak hello hello megoldásokat.

## <a name="change-hello-preconfigured-rules"></a>Hello előre konfigurált szabályok módosítása

hello távoli figyelési megoldás tartalmaz három [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) feladatok toohandle eszközadatokat, telemetriai, szabálylogika hello megoldásban.

hello három stream analytics-feladatok és a hello részletesen ismerteti a szintaxisát [távoli megfigyelési előre konfigurált megoldás forgatókönyv](iot-suite-remote-monitoring-sample-walkthrough.md). 

Ezek a feladatok közvetlen tooalter logika hello, vagy vegye fel a logikai adott tooyour forgatókönyv szerkesztheti. Található hello Stream Analytics-feladatok az alábbiak szerint:

1. Nyissa meg túl[Azure-portálon](https://portal.azure.com).
2. Keresse meg az erőforráscsoport toohello hello azonos nevet, amint az IoT-megoldásból. 
3. Válassza ki azt szeretné, hogy toomodify hello Azure Stream Analytics-feladat. 
4. Hello feladat leállításával kiválasztásával **leállítása** hello készletében lévő parancsok. 
5. Hello bemenet, a lekérdezés és a kimenetek szerkesztése.
   
    Egy egyszerű módosítás toochange hello lekérdezést hello **szabályok** feladat toouse egy **"<"** ahelyett, hogy egy **">"**. hello megoldás portálra még mindig **">"** amikor szabály szerkesztése, de figyelje meg, hogyan hello viselkedés tükrözött toohello változást az alapul szolgáló feladat hello miatt.
6. Hello feladat indítása

> [!NOTE]
> hello távoli figyelési irányítópult adatokat, függ, így hello irányítópult toofail hello feladatok módosítása okozhatja.

## <a name="add-your-own-rules"></a>A saját szabályok hozzáadása

Ezenkívül toochanging hello előre konfigurált Azure Stream Analytics-feladatok, hello Azure portál tooadd új feladatok használhatja, vagy vegyen fel új lekérdezések tooexisting feladatokat.

## <a name="customize-devices"></a>Eszközök testreszabása

Hello leggyakoribb bővítmény tevékenységek egyikét és eszközök meghatározott tooyour forgatókönyv működik. Többféleképpen eszközökre való munkához. Ezek a metódusok lehetnek a szimulált eszköz toomatch adott esetben módosítása, vagy a hello segítségével [IoT eszköz SDK] [ IoT Device SDK] tooconnect a fizikai eszköz toohello megoldás.

Egy részletes útmutató tooadding eszközök esetében lásd: hello [Iot Suite csatlakozó eszközök](iot-suite-connecting-devices.md) cikk és hello [C SDK minta figyelési távoli](https://github.com/Azure/azure-iot-sdk-c/tree/master/serializer/samples/remote_monitoring). Ez a minta a távoli felügyeleti előkonfigurált megoldás hello tervezett toowork.

### <a name="create-your-own-simulated-device"></a>A saját szimulált eszköz létrehozása

Hello szereplő [távoli figyelési megoldás forráskódját](https://github.com/Azure/azure-iot-remote-monitoring), .NET szimulátor van. A szimulátor egy részét hello megoldás, és módosíthatja a toosend különböző metaadatokat, telemetriai adatokat, és válaszoljon toodifferent parancsok és módszerek kiépített hello.

hello előre konfigurált szimulátor a távoli felügyeleti előkonfigurált megoldás hello bocsát ki a hőmérséklet és a páratartalom telemetriai hűtő eszköz szimulálja. Módosíthatja a hello hello szimulátor [Simulator.WebJob](https://github.com/Azure/azure-iot-remote-monitoring/tree/master/Simulator/Simulator.WebJob) projektre, ha már ágazik el hello GitHub-tárházban.

### <a name="available-locations-for-simulated-devices"></a>A szimulált eszköz elérhető helyről

hello alapértelmezés szerint a helyek Budapest/Redmond, Washington, az Amerikai Egyesült Államok van. Módosíthatja a következő helyeken és [SampleDeviceFactory.cs][lnk-sample-device-factory].

### <a name="add-a-desired-property-update-handler-toohello-simulator"></a>Adja hozzá a kívánt tulajdonság frissítési kezelő toohello szimulátor

Egy eszköz számára a kívánt tulajdonság értékét hello megoldás portálon állíthatja be. Feladata hello hello eszköz toohandle hello tulajdonság változáskérés amikor hello eszköz kér le a szükséges hello tulajdonság értéke. a tulajdonság értékének módosítása tooadd támogatása a kívánt tulajdonságon keresztül kell tooadd kezelő toohello szimulátor.

hello szimulátor tartalmaz hello kezelőkkel **SetPointTemp** és **TelemetryInterval** beállításával frissíthető tulajdonságok kívánt hello megoldás portálon értékeit.

hello alábbi példa bemutatja a hello hello kezelő **SetPointTemp** hello tulajdonság szükséges **CoolerDevice** osztály:

```csharp
protected async Task OnSetPointTempUpdate(object value)
{
    var telemetry = _telemetryController as ITelemetryWithSetPointTemperature;
    telemetry.SetPointTemperature = Convert.ToDouble(value);

    await SetReportedPropertyAsync(SetPointTempPropertyName, telemetry.SetPointTemperature);
}
```

Ez a módszer hello telemetriai pont hőmérséklet és jelentések hello módosítása hátsó tooIoT központ által jelentett tulajdonság beállítása frissíti.

A saját kezelőit a saját tulajdonságokat adhat hozzá a következő hello minta a fenti példa hello által.

Is kötni kell hello kívánt tulajdonság toohello kezelő látható módon a következő példa hello hello **CoolerDevice** konstruktor:

```csharp
_desiredPropertyUpdateHandlers.Add(SetPointTempPropertyName, OnSetPointTempUpdate);
```

Vegye figyelembe, hogy **SetPointTempPropertyName** egy meghatározott "Config.SetPointTemp" konstans.

### <a name="add-support-for-a-new-method-toohello-simulator"></a>Adja hozzá egy új módszer toohello szimulátor támogatása

Testre szabhatja a hello szimulátor tooadd támogatása egy új [módszer (közvetlen módszer)][lnk-direct-methods]. Szükséges két fő lépésből áll:

- hello szimulátor értesítenie kell az IoT-központ hello előre konfigurált hello megoldásban hello metódus adatokkal.
- hello szimulátor kell tartalmazniuk a kód toohandle hello metódus hívása a hello meghívása **eszközadatok** hello megoldástallózó vagy egy feladat panelen.

hello távoli megfigyelési előre konfigurált megoldás által használt *tulajdonságok jelentett* támogatott módszerek tooIoT hub toosend részleteit. hello megoldás háttérrendszerének fenntart egy listát a metódus meghívásához előzményeit együtt minden eszköz által támogatott összes hello módszert. Tekintse meg az eszközök és a metódusok hello megoldás portálon.

toonotify hello IoT-központot, hogy egy eszköz támogatja-e egy módszert, hello eszköz hozzá kell adnia a hello metódus toohello részleteit **SupportedMethods** hello csomópontja jelentett tulajdonságok:

```json
"SupportedMethods": {
  "<method signature>": "<method description>",
  "<method signature>": "<method description>"
}
```

hello metódus-aláírás rendelkezik hello a következő formátumban: `<method name>--<parameter #0 name>-<parameter #1 type>-...-<parameter #n name>-<parameter #n type>`. Például toospecify hello **InitiateFirmwareUpdate** metódus azt várja, egy karakterlánc-paramétert nevű **FwPackageURI**, a következő metódus-aláírás hello használata:

```json
InitiateFirmwareUpate--FwPackageURI-string: "description of method"
```

A támogatott paraméter típusainak listáját lásd: hello **CommandTypes** osztály hello infrastruktúra projektben.

egy metódus toodelete hello metódus-aláírás túl beállítása`null` hello a jelentett tulajdonságok.

> [!NOTE]
> hello megoldás háttérrendszerének csak frissíti információ a támogatott módszerek fogadásakor egy *eszközadatokat* üzenet hello eszközről.

kódminta hello a következő hello **SampleDeviceFactory** hello közös osztály hogyan projekt mutat be tooadd metódus toohello listáját a **SupportedMethods** hello a jelentett hello által küldött tulajdonságai eszköz:

```csharp
device.Commands.Add(new Command(
    "InitiateFirmwareUpdate",
    DeliveryType.Method,
    "Updates device Firmware. Use parameter 'FwPackageUri' toospecifiy hello URI of hello firmware file, e.g. https://iotrmassets.blob.core.windows.net/firmwares/FW20.bin",
    new[] { new Parameter("FwPackageUri", "string") }
));
```

A kódrészletet hozzáadja hello részleteit **InitiateFirmwareUpdate** többek között a szöveg toodisplay a hello megoldás portal és hello részleteit metódus szükséges, a metódusok paramétereihez.

hello szimulátor küld jelentett tulajdonságai, beleértve a hello támogatott módszerek tooIoT Hub hello szimulátor indításakor.

Adja hozzá az egyes módszerek, amely támogatja a kezelő toohello szimulátor kódot. Megtekintheti a meglévő kezelőket hello hello **CoolerDevice** osztály hello Simulator.WebJob projektben. hello alábbi példa bemutatja a hello kezelő **InitiateFirmwareUpdate** módszert:

```csharp
public async Task<MethodResponse> OnInitiateFirmwareUpdate(MethodRequest methodRequest, object userContext)
{
    if (_deviceManagementTask != null && !_deviceManagementTask.IsCompleted)
    {
        return await Task.FromResult(BuildMethodRespose(new
        {
            Message = "Device is busy"
        }, 409));
    }

    try
    {
        var operation = new FirmwareUpdate(methodRequest);
        _deviceManagementTask = operation.Run(Transport).ContinueWith(async task =>
        {
            // after firmware completed, we reset telemetry
            var telemetry = _telemetryController as ITelemetryWithTemperatureMeanValue;
            if (telemetry != null)
            {
                telemetry.TemperatureMeanValue = 34.5;
            }

            await UpdateReportedTemperatureMeanValue();
        });

        return await Task.FromResult(BuildMethodRespose(new
        {
            Message = "FirmwareUpdate accepted",
            Uri = operation.Uri
        }));
    }
    catch (Exception ex)
    {
        return await Task.FromResult(BuildMethodRespose(new
        {
            Message = ex.Message
        }, 400));
    }
}
```

Kezelő metódusnevek kell kezdődnie `On` hello metódus hello neve követ. Hello **methodRequest** paraméter hello megoldás háttérből hello metódushívás átadni paramétereket tartalmaz. hello visszatérési érték típusúnak kell lennie **feladat&lt;MethodResponse&gt;**. Hello **BuildMethodResponse** segédprogram módszer segítségével hozhat létre a hello visszatérési érték.

Belül hello metódus-kezelőjének sikerült:

- Aszinkron feladat elindítása.
- Lekérdezni kívánt tulajdonságokkal hello *eszköz iker* az IoT-központot.
- Hello segítségével egyetlen jelentett tulajdonság frissítéséhez **SetReportedPropertyAsync** metódus a hello **CoolerDevice** osztály.
- Hozzon létre több jelentett tulajdonságainak frissítése egy **TwinCollection** példány és a hívó hello **Transport.UpdateReportedPropertiesAsync** metódust.

hello belső vezérlőprogram-frissítés példában hajtja végre a lépéseket követve hello:

- Ellenőrzések hello eszköz képes tooaccept hello belső vezérlőprogram frissítési kérelem.
- Aszinkron módon hello belső vezérlőprogram frissítési művelet elindul, és alaphelyzetbe állítja a hello telemetriai hello művelet befejezésekor.
- Azonnal vissza hello "FirmwareUpdate elfogadta" üzenetet tooindicate hello kérés elfogadva hello eszköz

### <a name="build-and-use-your-own-physical-device"></a>Hozza létre, és a saját (fizikai) eszköz

Hello [Azure IoT SDK-k](https://github.com/Azure/azure-iot-sdks) adja meg a szalagtárak számos eszköztípus (nyelv és operációs rendszerek) csatlakozni az IoT-megoldásokhoz.

## <a name="modify-dashboard-limits"></a>Irányítópult korlátok módosítása

### <a name="number-of-devices-displayed-in-dashboard-dropdown"></a>Irányítópult legördülő megjelenő eszközök száma

hello alapértelmezett érték 200. Módosíthatja ezt a számot [DashboardController.cs][lnk-dashboard-controller].

### <a name="number-of-pins-toodisplay-in-bing-map-control"></a>A Bing térképek vezérlő PIN-kódok toodisplay száma

hello alapértelmezett érték 200. Módosíthatja ezt a számot [TelemetryApiController.cs][lnk-telemetry-api-controller-01].

### <a name="time-period-of-telemetry-graph"></a>Telemetria gráf időszakban

hello alapértelmezett érték 10 perc. Ez az érték módosítható [TelmetryApiController.cs][lnk-telemetry-api-controller-02].

## <a name="manually-set-up-application-roles"></a>Alkalmazási szerepköröknek manuális beállítása

hello alábbi eljárás ismerteti, hogyan tooadd **Admin** és **ReadOnly** alkalmazás szerepkörök tooa előre konfigurált megoldás. Fontos megjegyezni, hogy már kiépített hello azureiotsuite.com helyről előkonfigurált megoldásokat tartalmazza-e a hello **Admin** és **ReadOnly** szerepkörök.

Hello tagjai **ReadOnly** szerepkör hello irányítópult és hello eszközök listája látható, de nem megengedett tooadd eszközök, eszköz-attribútumok módosítása vagy küldési parancsok.  Hello tagjai **Admin** szerepkör rendelkezik teljes körű hozzáférési tooall hello funkcióit a hello megoldás.

1. Nyissa meg toohello [a klasszikus Azure portálon][lnk-classic-portal].
2. Válassza ki **Active Directory**.
3. Kattintson a megoldás létesített használt hello AAD-bérlőt hello nevére.
4. Kattintson a **alkalmazások**.
5. Kattintson a hello alkalmazás, amely megfelel az előkonfigurált megoldás neve hello nevét. Ha nem látja az alkalmazás hello listán, válassza ki a **a vállalat tulajdonában lévő alkalmazások** a hello **megjelenítése** legördülő menüből, majd kattintson a pipa jelre hello.
6. Hello a hello lap alján, kattintson **kezelése Manifest** , majd **jegyzékfájl letöltése**.
7. Ez az eljárás egy .JSON kiterjesztésű fájl tooyour helyi számítógép tölti le. Nyissa meg ezt a fájlt egy szövegszerkesztőben, az Ön által választott szerkesztésre.
8. Hello harmadik sor hello .JSON kiterjesztésű fájl láthatja:

   ```json
   "appRoles" : [],
   ```
   Cserélje le ezt a sort a következő kód hello:

   ```json
   "appRoles": [
   {
   "allowedMemberTypes": [
   "User"
   ],
   "description": "Administrator access toohello application",
   "displayName": "Admin",
   "id": "a400a00b-f67c-42b7-ba9a-f73d8c67e433",
   "isEnabled": true,
   "value": "Admin"
   },
   {
   "allowedMemberTypes": [
   "User"
   ],
   "description": "Read only access toodevice information",
   "displayName": "Read Only",
   "id": "e5bbd0f5-128e-4362-9dd1-8f253c6082d7",
   "isEnabled": true,
   "value": "ReadOnly"
   } ],
   ```

9. Hello frissített .JSON kiterjesztésű fájlt (felülírhatja hello meglévő fájl) mentése.
10. Hello alján hello hello, klasszikus Azure portálon válassza **kezelése Manifest** majd **feltöltése Manifest** hello előző lépésben mentett tooupload hello .JSON kiterjesztésű fájlt.
11. Ezzel hozzáadta a hello **Admin** és **ReadOnly** szerepkörök tooyour alkalmazás.
12. a szerepkörök tooa felhasználónak számít a címtárában, egyik tooassign lásd: [hello azureiotsuite.com hely engedélyeinek][lnk-permissions].

## <a name="feedback"></a>Visszajelzés

Rendelkezik a dokumentum által ismertetett toosee milyen testreszabást? Adja hozzá a szolgáltatási javaslataikat túl[User Voice](https://feedback.azure.com/forums/321918-azure-iot), vagy ez a cikk megjegyzés. 

## <a name="next-steps"></a>Következő lépések

További információ az előre konfigurált hello megoldások testreszabásának hello beállításai toolearn lásd:

* [Csatlakozás a Logic App tooyour Azure IoT Suite távoli figyelésére szolgáló előre konfigurált megoldás][lnk-logicapp]
* [Dinamikus telemetriai adatok használata a távoli felügyeleti előkonfigurált megoldás hello][lnk-dynamic]
* [Eszköz információk metaadatait a távoli felügyeleti előkonfigurált megoldás hello][lnk-devinfo]
* [Hogyan hello csatlakoztatva a OPC EE-kiszolgálóiról gyári megoldás jelenít meg adatokat testreszabása][lnk-cf-customize]

[lnk-logicapp]: iot-suite-logic-apps-tutorial.md
[lnk-dynamic]: iot-suite-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[IoT Device SDK]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-permissions]: iot-suite-permissions.md
[lnk-dashboard-controller]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/Controllers/DashboardController.cs#L27
[lnk-telemetry-api-controller-01]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L27
[lnk-telemetry-api-controller-02]: https://github.com/Azure/azure-iot-remote-monitoring/blob/e7003339f73e21d3930f71ceba1e74fb5c0d9ea0/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L25 
[lnk-sample-device-factory]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Common/Factory/SampleDeviceFactory.cs#L40
[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-cf-customize]: iot-suite-connected-factory-customize.md