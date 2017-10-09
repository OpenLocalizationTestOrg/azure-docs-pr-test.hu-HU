---
title: "Azure IoT Suite aaaCustomize csatlakoztatott gyári |} Microsoft Docs"
description: "Hogyan toocustomize hello viselkedését hello csatlakoztatva gyári leírása előre konfigurált megoldás."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-suite
ms.devlang: c#
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 53f2fef7a76b5d8e6ad023945a7812dc7fabd12c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="customize-how-hello-connected-factory-solution-displays-data-from-your-opc-ua-servers"></a>Hogyan hello csatlakoztatva a OPC EE-kiszolgálóiról gyári megoldás jelenít meg adatokat testreszabása

## <a name="introduction"></a>Bevezetés

hello csatlakoztatott gyári megoldás összesíti és hello OPC EE kiszolgálók csatlakoztatott toohello megoldás adatait jeleníti meg. Keresse meg és kiszolgálók parancsok toohello OPC EE küldhet a megoldásban. OPC EE kapcsolatos további információkért lásd: hello [gyári gyakran ismételt kérdések csatlakoztatott](iot-suite-faq-cf.md).

Összesített adatot hello megoldás például hello általános berendezések hatékonyságát (OEE) és fő teljesítménymutatók (KPI-k), megtekintheti a hello irányítópult hello gyári, a sor és az állomás szintjén. hello alábbi képernyőfelvételen látható hello OEE és KPI értékeit hello **szerelvény** állomás, a **termelési sor 1**, a hello **München** gyári:

![OEE és KPI értékek hello megoldás – példa][img-oee-kpi]

hello megoldás lehetővé teszi, hogy Ön tooview részletes információkat az adott adatok tételekhez hello nevezett OPC EE-kiszolgálók *állomások*. hello alábbi képernyőfelvételen látható függvényében hello elemszáma gyártott egy adott állomásról:

![Felvétel az előállított elemek száma.][img-manufactured-items]

Ha egy grafikonon hello gombra kattint, ismerje meg a hello adatokat, és további időt adatsorozat Insights (ÁME):

![Adatokba idő adatsorozat Insights segítségével][img-tsi]

Ez a cikk ismerteti:

- Hogyan hello adatok legyen elérhető toohello különböző nézetek hello megoldásban.
- Hogyan szabhatja testre hello módon hello megoldás hello adatokat jeleníthet meg.

## <a name="data-sources"></a>Adatforrások

hello csatlakoztatott gyári megoldás adatait jeleníti meg hello OPC EE kiszolgálók csatlakoztatott toohello megoldás. hello alapértelmezett telepítése magában foglalja a gyári szimuláció futtató több OPC EE. A saját OPC EE-kiszolgálót is hozzáadhat, amelyek [átjárón keresztül csatlakozni] [ lnk-connect-cf] tooyour megoldás.

Tallózással is kikeresheti, hogy a csatlakoztatott OPC EE-kiszolgáló küldhet-e a tooyour megoldás hello irányítópulton hello adatelemek:

1. Keresse meg a toohello **jelöljön ki egy OPC EE** megtekintése:

    ![Keresse meg a toohello válasszon egy OPC EE-kiszolgáló megtekintése][img-select-server]

1. Válasszon kiszolgálót, majd kattintson a **Connect**. Kattintson a **folytatása** hello biztonsági figyelmeztetés megjelenésekor.

    > [!NOTE]
    > Ez a figyelmeztetés csak egyszer jelenik meg minden olyan kiszolgálón, és hello megoldás irányítópultja és hello kiszolgáló közötti megbízhatósági kapcsolatot létesít.

1. Most Tallózás hello adatelemek hello server küldhet toohello megoldás is. Toohello megoldás küldött elemeket zöld pipa rendelkezik:

    ![Közzétett elemek][img-published]

1. Ha Ön egy *rendszergazda* hello megoldást választhatja ki toopublish egy adatok elem toomake hello elérhető csatlakoztatva gyári megoldás. A rendszergazdák is adatelemek hello értékének módosítása és hívható meg metódus hello OPC EE-kiszolgáló.

## <a name="map-hello-data"></a>Hello adatok leképezése

hello gyári megoldás maps csatlakoztatva, és összesítések hello közzétett adatelemek hello OPC EE server toohello a különböző nézetek hello megoldásban. hello csatlakoztatott gyári megoldás telepíti tooyour Azure-fiók amikor hello megoldás. A JSON-fájl, a Visual Studio csatlakoztatott gyári megoldás hello a leképezési információkat tárolja. Megtekintheti és módosíthatja a JSON-konfigurációs fájl hello csatlakoztatott gyárban Visual Studio megoldás. A módosítást követően központilag telepítheti hello megoldás.

A konfigurációs fájlt hello használhatja:

- Hello meglévő szimulált előállítók, éles sorok és állomások szerkesztése.
- Képezze le a csatlakozni, toohello megoldás valós OPC EE-kiszolgálóról származó adatok.

hello másolatát tooclone gyári Visual Studio megoldás, a következő parancsot a git használata hello csatlakoztatva:

`git clone https://github.com/Azure/azure-iot-connected-factory.git`

hello fájl **ContosoTopologyDescription.json** társítást hello OPC EE-kiszolgálóadatok elemek toohello nézetek hello csatlakoztatott gyári megoldás irányítópultjának hello határozza meg. A konfigurációs fájl található hello **Contoso\Topology** hello mappájában **WebApp** projektre a Visual Studio megoldás hello.

hello JSON-fájl tartalmának hello gyári, termelési sor és állomás csomópontok hierarchikus vannak rendezve. Ezt a hierarchiát hello navigációs hierarchia hello csatlakoztatott gyári irányítópult határozza meg. Minden egyes csomóponton hello hierarchia értékek meghatározzák, hogy hello irányítópulton megjelenő hello információ. Például hello JSON-fájl tartalmazza a következő értékek hello München gyári hello:

```json
"Guid": "73B534AE-7C7E-4877-B826-F1C0EA339F65",
"Name": "Munich",
"Description": "Braking system",
"Location": {
    "City": "Munich",
    "Country": "Germany",
    "Latitude": 48.13641,
    "Longitude": 11.57754
},
"Image": "munich.jpg"
```

hello nevét, leírását és helyen jelennek meg ebben a nézetben hello irányítópulton:

![München adatok hello irányítópult][img-munich]

Minden egyes gyári, a termelési sor és a állomás van az image tulajdonság. A JPEG-fájlok találhatók hello **Content\img** hello mappájában **WebApp** projekt. A kép fájlok hello csatlakoztatott gyári irányítópult jeleníti meg.

Minden állomás több társítást hello OPC EE adatelemek hello meghatározó részletes tulajdonságok tartalmazza. Ezeket a tulajdonságokat ismerteti a következő részekben hello:

### <a name="opcuri"></a>OpcUri

Hello **OpcUri** értéke hello OPC EE alkalmazás URI, amely egyedileg azonosítja a hello OPC EE-kiszolgáló. Például hello **OpcUri** hello szerelvény állomás éles sor 1 München a következőhöz hasonló hello értéke: **urn: scada2194:ua:munich:productionline0:assemblystation**.

Hello URI-azonosítók csatlakoztatott hello OPC EE-kiszolgálók hello megoldás irányítópultja tekintheti meg:

![Nézet OPC EE kiszolgáló URI-azonosítók][img-server-uris]

### <a name="simulation"></a>Szimuláció

hello információk hello **szimuláció** csomópont adott toohello OPC EE szimuláció futó hello OPC EE-kiszolgálók alapértelmezés szerint törlődnek. A valós OPC EE-kiszolgáló nem szolgál.

### <a name="kpi1-and-kpi2"></a>Kpi1 és Kpi2

Ezek a csomópontok ismertetik, hogyan hello állomás adatait hozzájárul toohello két KPI értékek hello irányítópulton. Alapértelmezett telepítésében ezen KPI értékei óránként egységek és óránként kWh. hello megoldás kiszámítja a KPI vales állomás hello szintjén, és összesíti a hello termelési sor és a gyári szintjén.

Egyes KPI-KET egy minimális, maximális és célértékéhez rendelkezik. Minden KPI értéket adhat meg csatlakoztatott hello gyári megoldás tooperform riasztási műveletek. hello alábbi kódrészletben láthatja hello KPI definícióit hello szerelvény állomás éles sor München 1:

```json
"Kpi1": {
  "Minimum": 150,
  "Target": 300,
  "Maximum": 600
},
"Kpi2": {
  "Minimum": 50,
  "Target": 100,
  "Maximum": 200,
  "MinimumAlertActions": [
    {
      "Type": "None"
    }
  ]
}
```

hello alábbi képernyőfelvételen látható hello KPI adatok hello irányítópulton.

![Hello irányítópulton KPI-információk][lnk-kpi]

### <a name="opcnodes"></a>OpcNodes

Hello **OpcNodes** csomópontok azonosítása hello hello OPC EE-kiszolgáló a közzétett adatelemeket, és adja meg, hogyan tooprocess adatokat.

Hello **NodeId** érték azonosítja hello hello OPC EE-kiszolgáló az adott OPC EE NodeID. első csomópontjának hello hello szerelvény állomás gyártási sor München 1 értékű **ns = 2, i = 385**. A **NodeId** érték határozza meg, hello adatok elem tooread hello OPC EE-kiszolgáló és a hello **melynek** egy felhasználóbarát név toouse hello irányítópulton biztosítja az adatok.

Más csomópontokra társított értékek hello a következő táblázat foglalja össze:

| Érték | Leírás |
| ----- | ----------- |
| Találati pontosság  | hello KPI-t és OEE értékek ezek az adatok hozzájárul. |
| Műveleti kód     | Hogyan hello adatok összesíti. |
| egység      | hello egységek toouse hello irányítópulton.  |
| Látható    | E toodisplay ez értéket hello irányítópult. Egyes értékek számítások szerepel, de nem jelenik meg.  |
| Maximális    | hello maximális érték, amely riasztást hello irányítópulton. |
| MaximumAlertActions | Egy művelet tootake válasz tooan riasztásban. A parancs tooa állomás küldenek például. |
| ConstValue | A számításhoz használt konstans érték. |

## <a name="deploy-hello-changes"></a>Hello módosítása

Miután végzett a végzett módosítások toohello **ContosoTopologyDescription.json** fájlt, újra kell telepítenie hello gyári megoldás tooyour Azure-fiók csatlakoztatva.

Hello **azure iot-csatlakoztatott-gyári** tárház tartalmaz egy **build.ps1** PowerShell-parancsfájlt használhatja toorebuild és hello megoldás üzembe helyezéséhez.

## <a name="next-steps"></a>Következő lépések

További információ csatlakoztatott hello előre konfigurált gyári megoldásról által olvasása hello a következő cikkeket:

* [Előre konfigurált csatlakoztatott gyár megoldás – bemutató][lnk-rm-walkthrough]
* [A beépített csatlakoztatott egy átjáró üzembe helyezéséhez][lnk-connect-cf]
* [Engedélyek hello azureiotsuite.com webhelyen][lnk-permissions]
* [Csatlakoztatott gyár – GYIK](iot-suite-faq-cf.md)
* [GYAKORI KÉRDÉSEK][lnk-faq]


[img-oee-kpi]: ./media/iot-suite-connected-factory-customize/oeenadkpi.png
[img-manufactured-items]: ./media/iot-suite-connected-factory-customize/manufactured.png
[img-tsi]: ./media/iot-suite-connected-factory-customize/tsi.png
[img-select-server]: ./media/iot-suite-connected-factory-customize/selectserver.png
[img-published]: ./media/iot-suite-connected-factory-customize/published.png
[img-munich]: ./media/iot-suite-connected-factory-customize/munich.png
[img-server-uris]: ./media/iot-suite-connected-factory-customize/serveruris.png
[lnk-kpi]: ./media/iot-suite-connected-factory-customize/kpidisplay.png

[lnk-rm-walkthrough]: iot-suite-connected-factory-sample-walkthrough.md
[lnk-connect-cf]: iot-suite-connected-factory-gateway-deployment.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-faq]: iot-suite-faq.md