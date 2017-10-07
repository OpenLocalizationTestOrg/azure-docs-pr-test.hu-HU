---
title: "aaaUse hello Azure portál toocreate az IoT-központ |} Microsoft Docs"
description: "Hogyan toocreate, kezelése és törlése az Azure IoT hub hello Azure-portálon keresztül. Tarifacsomagok, a méretezés, a biztonsági, és a konfigurációs üzenetküldésre kapcsolatos adatokat tartalmaz."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 0909cd2b-4c1e-49e0-b68a-75532caf0a6a
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: dobett
ms.openlocfilehash: 383968c90ee7ef3bff85a6c90efbf5f0e8fbb208
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-iot-hub-using-hello-azure-portal"></a>Létrehoz egy IoT-központot hello Azure-portál használatával

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Ez a cikk ismerteti:

* Hogyan toofind hello hello Azure-portálon az IoT-központ szolgáltatás.
* Hogyan toocreate és az IoT-központok kezelése.

## <a name="where-toofind-hello-iot-hub-service"></a>Ha toofind hello IoT-központ szolgáltatás

Az alábbi helyek hello portálon hello hello IoT-központ szolgáltatás található:

* Válasszon **+ új**, majd válassza a **az eszközök internetes hálózatát**.
* Hello piactér, válassza a **az eszközök internetes hálózatát**.

## <a name="create-an-iot-hub"></a>IoT Hub létrehozása

Létrehozhat az IoT-központ a következő módszerek hello használata:

* Hello **+ új** beállítás látható a következő képernyőfelvétel hello hello paneljének megnyitása. hello IoT-központ létrehozásának, ezzel a metódussal és hello piactéren keresztül hello lépései megegyeznek.
* Hello piactér, válassza a **létrehozása** tooopen hello panel hello a következő képernyőfelvételen látható.

hello következő részek a hello több lépéseket toocreate az IoT-központ:

### <a name="choose-hello-name-of-hello-iot-hub"></a>Válassza ki az IoT-központ hello hello neve

az IoT-központ toocreate, adjon nevet az IoT-központ hello. Ez a név minden IoT-központok között egyedinek kell lennie.

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

### <a name="choose-hello-pricing-tier"></a>Hello tarifacsomag kiválasztása

Négy szintek közül választhat: **szabad**, **szabványos 1** és **szabványos 2**, és **Standard S3**. ingyenes szint hello lehetővé teszi, hogy csak 500 eszközök toobe csatlakozott toohello IoT-központot, illetve a hierarchiában felfelé too8, 000 üzenet naponta.

**Standard szintű, S1**: hello S1 edition használja, hogy mindegyik generál kis mennyiségű adatokat eszközök nagy számú IoT-megoldások. Hello S1 kiadás tárolóegységekhez lehetővé teszi, hogy másolatot too400, az összes csatlakoztatott eszközön naponta 000 üzenetek.

**Standard S2**: hello S2 edition használja, amelyben eszközök nagy mennyiségű adat készítése az IoT-megoldások. Egyes Munkaegységek hello S2 edition lehetővé teszi, hogy másolatot too6 millió üzenetek / nap közötti minden csatlakoztatott eszközön.

**Standard S3**: hello S3 edition használja, amely nagy mennyiségű adat az IoT-megoldások. Egyes Munkaegységek hello S3 edition lehetővé teszi, hogy másolatot too300 millió üzenetek / nap közötti minden csatlakoztatott eszközön.

![][4]

> [!NOTE]
> Az IoT-központ csak egy ingyenes hub / Azure-előfizetés teszi lehetővé.

### <a name="iot-hub-units"></a>IoT hub-egységek

napi egység értesítésenként engedélyezett üzenetek hello száma a központ tarifacsomag függ. Például ha azt szeretné, hogy az IoT hub toosupport érkező üzenetek 700 000 hello, választja két S1 réteg egység.

### <a name="device-toocloud-partitions-and-resource-group"></a>Eszköz toocloud partíciók és erőforráscsoport

Az IoT-központ tároló partíciók száma hello módosíthatja. partíciók száma hello alapértelmezett érték 4, akkor egy másik számot hello legördülő listából.

Nem kell tooexplicitly hozzon létre egy üres erőforráscsoportot. Amikor létrehoz egy erőforrást, vagy új toocreate válasszon, vagy használjon egy meglévő erőforráscsoportot.

![][5]

### <a name="choose-subscription"></a>Válassza ki az előfizetést

Az Azure IoT Hub automatikusan listák hello Azure-előfizetések hello felhasználói fiók van csatolva. Kiválaszthatja a hello Azure-előfizetés tooassociate hello IoT-központ számára.

### <a name="choose-hello-location"></a>Hello helyének kiválasztása

hello hely beállítás hello régiókban, ahol az IoT-központ lehetőség a listáját jeleníti meg.

### <a name="create-hello-iot-hub"></a>Hello IoT hub létrehozása

Ha az összes előző lépést, hello IoT-központ is létrehozhat. Kattintson a **létrehozása** toostart háttérfolyamatot toocreate hello, és úgy döntött, hogy hello beállításokkal hello IoT-központ telepítése.

Mivel némi időre van a háttér-telepítési toorun hello hello megfelelő helyet kiszolgálókon is igénybe vehet néhány percet toocreate hello IoT-központ.

## <a name="change-hello-settings-of-hello-iot-hub"></a>Az IoT-központ hello hello beállításainak módosítása

Módosíthatja egy meglévő IoT-központot hello beállításainak hello IoT Hub panel a létrehozása után.

![][8]

**Megosztott hozzáférési házirendek**: ezek a házirendek eszközökön és szolgáltatásokon tooconnect tooIoT Hub hello engedélyeinek megadása. Ezek a házirendek eléréséhez kattintson **megosztott elérési házirendek** alatt **általános**. Ezen a panelen módosíthatja a meglévő házirendeket, vagy adjon hozzá egy új házirendet.

### <a name="create-a-policy"></a>Házirend létrehozása

* Kattintson a **Hozzáadás** tooopen egy panel. Itt adhatja meg hello új házirend nevét és hello engedélyeit, hogy a házirend tooassociate hello alábbi ábrán. ábra:

    Nincsenek társítható megosztott szabályzatokról több engedélyeket. Hello **beállításkulcs olvasása** és **beállításjegyzék írási** házirendek adjon olvasási és írási hozzáférési jogok toohello identitásjegyzékhez. A beállítás hello írási automatikusan úgy dönt, hello olvasása lehetőséget.

    Hello **Service csatlakozás** házirend ad engedélyt tooaccess Szolgáltatásvégpontok például **eszközről a felhőbe kap**. Hello **eszköz csatlakozzon** házirend engedélyt ad az IoT-központ eszközoldali hello végpontok használatával üzenetek küldése és fogadása.

* Kattintson a **létrehozása** tooadd ebben az újonnan létrehozott házirend toohello meglévő listájához.

![][10]

## <a name="endpoints"></a>Végpontok

Kattintson a **végpontok** toodisplay hello IoT-központ, amely módosítja a végpontok listáját. A végpontok két típusa van: hello IoT-központ beépített végpontok és, hogy a létrehozása után adja hozzá az IoT-központ toohello végpontok.

![][11]

### <a name="built-in-endpoints"></a>Beépített végpontok

Nincsenek a két beépített végpont: **toodevice visszajelzés Cloud** és **események**.

* **Felhőalapú toodevice visszajelzés** beállítások: Ezzel a beállítással rendelkezik két subsettings: **tooDevice TTL Cloud** (idő TTL) és **megőrzési idő** (órákban) hello üzenetek. Az első létrehoz egy IoT-központot, mindkét ezeket a beállításokat kell egy óra hello alapértelmezett értékét. tooadjust ezeket a beállításokat hello csúszkákkal, vagy írja be a hello értékeket.
* **Események** beállítások: Ezzel a beállítással rendelkezik több subsettings, amelyek némelyike csak olvasható. a következő lista hello ezeket a beállításokat mutatja be:

  * **Partíciók**: egy alapértelmezett értéke hello IoT-központ létrehozásakor. Ez a beállítás a partíciók száma hello módosíthatja.

  * **Event Hub-kompatibilis nevének és végpontjának**: hello IoT-központ létrehozásakor az Eseményközpont létrehozásakor belső hogy előfordulhat, hogy kell-e hozzáférési toounder bizonyos körülmények között. Nem szabhatja testre a hello Event Hub-kompatibilis és a végpont értékét, de kattintva másolhatja **másolási**.

  * **Megőrzési idő**: tooone nap meg alapértelmezés szerint azonban hello legördülő lista használatával módosíthat. Ez az érték nap hello eszközről a felhőbe beállítás van.

  * **Felhasználói csoportok**: felhasználói csoportok lehetővé teszik az több olvasók tooread üzenetet az IoT-központ hello től függetlenül. Minden IoT-központot egy alapértelmezett felhasználói csoport jön létre. Azonban hozzáadása vagy törlése a fogyasztói csoportok tooyour IoT-központok ezt a beállítást használja.

  > [!NOTE]
  > hello alapértelmezett felhasználói csoport nem szerkeszthető, és nem törölhető.

### <a name="custom-endpoints"></a>Egyéni végpontokat

Egyéni végpontokat az IoT hub hello portál használatával is hozzáadhat. A hello **végpontok** panelen kattintson **Hozzáadás** : hello felső tooopen hello **végpont hozzáadása** panel. Adja meg a hello szükséges információkat, majd kattintson a **OK**. Az egyéni végpontjának megjelenik a fő hello **végpontok** panelen.

![][13]

További tudnivalók az egyéni végpontokat [referencia - IoT-központok végpontjai][lnk-devguide-endpoints].

## <a name="routes"></a>Útvonalak

Kattintson a **útvonalak** toomanage hogyan IoT-központ kiszállítja az eszközről a felhőbe üzeneteket.

![][14]

Útvonalak tooyour IoT-központ kattintva vehet fel **Hozzáadás** hello hello tetején **útvonalak*** hello szükséges adatok bevitele, és kattintson a panel **OK**. Az útvonal majd szerepel a fő hello **útvonalak** panelen. Útvonalak listájában hello kattintva szerkesztheti egy útvonalat. tooenable egy útvonalat, kattintson az útvonalak hello listában, és állítsa be a hello **engedélyezve** túl váltása**ki**. toosave hello módosítása, kattintson a **OK** hello hello panel alsó részén.

![][15]

## <a name="pricing-and-scale"></a>Díjszabás és méretezés

egy meglévő IoT-központot árképzése hello hello keresztül módosítható **árazás** beállításait, a következő kivételek hello:

* Hello aktuális megvalósításban egy IoT hubot szabad Termékváltozat nem módosítható rétegek tooone SKU, fizetős hello, vagy fordítva.
* Csak lehet egy ingyenes szint IoT-központ hello Azure-előfizetés.

![][12]

Áthelyezheti a magasabb szintű toolower használható csak akkor, ha adott napon küldött üzenetek hello száma meghaladja a hello kvóta hello alacsonyabb szinten. Például ha naponta üzenetek hello száma meghaladja a 400000, majd hello réteget a hello IoT hub módosítható. Azonban toohello S1 réteg módosítása ezután hello IoT-központ szabályozva van napra vonatkozóan.

## <a name="delete-hello-iot-hub"></a>Az IoT-központ hello törlése

Toohello IoT-központ toodelete kívánt kattintva tallózással is kikeresheti **Tallózás**, és megfelelő hub toodelete lehetőséget választja hello. toodelete hello IoT-központot, kattintson a hello **törlése** hello IoT-központnév gombra.

## <a name="next-steps"></a>Következő lépések

Kövesse az alábbi hivatkozások toolearn Azure IoT Hub kezelésével kapcsolatos további:

* [Tömeges az IoT-eszközök kezelése][lnk-bulk]
* [Az IoT-központ metrikák][lnk-metrics]
* [Figyelési műveletek][lnk-monitor]

toofurther megismerkedhet az IoT-központ hello képességeit, lásd:

* [IoT Hub fejlesztői útmutató][lnk-devguide]
* [Egy eszköz szimulálva IoT oldala][lnk-iotedge]
* [Az IoT-megoldásból hello szabad a biztonságos][lnk-securing]

[4]: ./media/iot-hub-create-through-portal/create-iothub.png
[5]: ./media/iot-hub-create-through-portal/location1.png
[8]: ./media/iot-hub-create-through-portal/portal-settings.png
[10]: ./media/iot-hub-create-through-portal/shared-access-policies.png
[11]: ./media/iot-hub-create-through-portal/messaging-settings.png
[12]: ./media/iot-hub-create-through-portal/pricing-error.png
[13]: ./media/iot-hub-create-through-portal/endpoint-creation.png
[14]: ./media/iot-hub-create-through-portal/routes-list.png
[15]: ./media/iot-hub-create-through-portal/route-edit.png

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
