---
title: "az IoT Suite aaaAzure csatlakoztatott gyári – gyakori kérdések |} Microsoft Docs"
description: "Az IoT Suite csatlakoztatott factory gyakran ismételt kérdések"
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2017
ms.author: corywink
ms.openlocfilehash: 4ae9beb0daf1b0578850cd652eaca7635b0d039d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-for-iot-suite-connected-factory-preconfigured-solution"></a>Gyakori kérdések az IoT Suite csatlakoztatott beépített, előre konfigurált megoldás

További információ, általános hello [gyakran ismételt kérdések](iot-suite-faq.md) IoT Suite.

### <a name="where-can-i-find-hello-source-code-for-hello-preconfigured-solution"></a>Hol találok hello forráskód előre konfigurált hello megoldáshoz?

a következő GitHub-tárházban hello hello forráskód tárolja:

* [Előre konfigurált csatlakoztatott gyári megoldás](https://github.com/Azure/azure-iot-connected-factory)

### <a name="what-is-opc-ua"></a>Mi az a OPC EE?

OPC egyesített architektúra révén, 2008, amely a szabványos platformfüggetlen, szolgáltatásorientált együttműködési. OPC EE különféle ipari rendszerek és egyéb eszközök például számítógépek iparági PLC vagy érzékelők használja. OPC EE egy bővíthető keretrendszer és a beépített biztonság hello OPC klasszikus specifikációk hello funkcióit integrálja. Olyan szabvány, amelyek célja a hello OPC Foundation. Hello [OPC Foundation](http://opcfoundation.org/) egy nem nonprofit szervezet több mint 440 tagjaival. hello hello szervezet célja toouse OPC specifikációk toofacilitate több szállító, többplatformos, biztonságos és megbízható együttműködési keresztül:

* Infrastruktúra
* Specifikációk
* Technológia
* Folyamatok

### <a name="why-did-microsoft-choose-opc-ua-for-hello-connected-factory-preconfigured-solution"></a>Miért adta meg a Microsoft hello a OPC EE csatlakoztatott előre konfigurált gyári megoldás?

Microsoft OPC EE választotta, mert a nyitott, a nem tulajdonosi platform független, iparági ismeri fel és bevált szabványnak. Ez feltétele Industrie 4.0-s verzióját (RAMI4.0) referencia architektúra megoldások gyártási eljárások széles körét és berendezések együttműködésével biztosítása. Microsoft látja az ügyfelek toobuild Industrie 4.0 megoldásainkkal igény szerint. OPC EE támogatása segít az ügyfeleknek tooachieve alacsonyabb hello korlát a cél, és azonnali üzleti értéket toothem biztosít.

### <a name="how-do-i-add-a-public-ip-address-toohello-simulation-vm"></a>Hogyan adja hozzá a nyilvános IP cím toohello szimuláció VM?

Két beállítások tooadd hello IP-címmel rendelkezik:

* PowerShell-parancsfájl hello `Simulation/Factory/Add-SimulationPublicIp.ps1` a hello [tárház](https://github.com/Azure/azure-iot-connected-factory). A telepítés neve paraméterként adja át. Egy helyi központi telepítésének használata `<your username>ConnFactoryLocal`. hello parancsfájl kinyomtatja hello VM hello IP-címét.

* Hello Azure-portálon keresse meg a központi telepítés hello erőforráscsoport. Egy helyi központi telepítés, kivéve a hello erőforráscsoport rendelkezik hello megoldásként megadott vagy telepítés nevét. Egy helyi központi telepítésének hello build parancsfájllal hello erőforráscsoport hello neve nem `<your username>ConnFactoryLocal`. Most adjon hozzá egy új **nyilvános IP-cím** erőforrás toohello erőforráscsoport.

> [!NOTE]
> Mindkét esetben ügyeljen arra, telepítse legújabb javítások hello hello hello utasításai szerint [Ubuntu webhely](https://wiki.ubuntu.com/Security/Upgrades). Vegye fel a toodate hello telepítési mindaddig, amíg a virtuális Gépet egy nyilvános IP-cím keresztül érhető el.

### <a name="how-do-i-remove-hello-public-ip-address-toohello-simulation-vm"></a>Hogyan távolíthatom hello nyilvános IP cím toohello szimuláció VM?

Két beállítások tooremove hello IP-címmel rendelkezik:

* Hello PowerShell-parancsfájl a hello Simulation/Factory/Remove-SimulationPublicIp.ps1 [tárház](https://github.com/Azure/azure-iot-connected-factory). A telepítés neve paraméterként adja át. Egy helyi központi telepítésének használata `<your username>ConnFactoryLocal`. hello parancsfájl kinyomtatja hello VM hello IP-címét.

* Hello Azure-portálon keresse meg a központi telepítés hello erőforráscsoport. Egy helyi központi telepítés, kivéve a hello erőforráscsoport rendelkezik hello megoldásként megadott vagy telepítés nevét. Egy helyi központi telepítésének hello build parancsfájllal hello erőforráscsoport hello neve nem `<your username>ConnFactoryLocal`. Most eltávolítja a hello **nyilvános IP-cím** hello erőforráscsoportból erőforrás.

### <a name="how-do-i-sign-in-toohello-simulation-vm"></a>Hogyan toohello szimuláció Virtuálisgép jelentkezni?

Aláírási toohello szimuláció VM csak támogatott, ha a megoldás hello PowerShell-parancsfájl használatával telepített `build.ps1` a hello [tárház](https://github.com/Azure/azure-iot-connected-factory).

Ha www.azureiotsuite.com hello megoldást, toohello virtuális gép nem tud bejelentkezni. Nem tud bejelentkezni, mert hello jelszó véletlenszerűen történik, és nem állítható alaphelyzetbe.

1. Adjon hozzá egy nyilvános IP-cím toohello virtuális gép. Lásd: [hogyan adja hozzá a nyilvános IP cím toohello szimuláció VM?](#how-do-i-remove-the-public-ip-address-to-the-simulation-vm)
1. Hozzon létre egy SSH-munkamenet tooyour VM hello VM hello IP-címét használja.
1. hello felhasználónév toouse van: `docker`.
1. hello jelszó toouse toodeploy használt hello verziójától függ:
    * 2017. június 1. előtt hello build.ps1 parancsfájl használatával telepített megoldásainak hello jelszava: `Passw0rd`.
    * Hello build.ps1 parancsfájl használatával után 2017. június 1. telepítve megoldásainak található hello jelszó hello `<name of your deployment>.config.user` fájlt. hello jelszó tárolódik hello **VmAdminPassword** beállítást. hello jelszó jön létre véletlenszerűen központi telepítéskor kivéve, ha megadja azt a hello segítségével `build.ps1` parancsfájl-paraméter`-VmAdminPassword`

### <a name="how-do-i-stop-and-start-all-docker-processes-in-hello-simulation-vm"></a>Hogyan állítsa le és indítsa el az összes docker folyamat hello szimuláció VM?

1. VM toohello szimulálása a bejelentkezés. Lásd: [hogyan jelentkezzen toohello szimuláció VM?](#how-do-i-sign-in-to-the-simulation-vm)
1. Futtassa mely tárolók aktívak, toocheck: `docker ps`.
1. toostop minden szimuláció tároló futtatása: `./stopsimulation`.
1. toostart minden szimuláció tároló:
    * Hello nevű rendszerhéj változó exportálása **IOTHUB_CONNECTIONSTRING**. Hello hello érték **IotHubOwnerConnectionString** hello beállítása `<name of your deployment>.config.user` fájlt. Példa:

        ```
        export IOTHUB_CONNECTIONSTRING="HostName={yourdeployment}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={your key}"
        ```

    * Futtassa az `./startsimulation` parancsot.

### <a name="how-do-i-update-hello-simulation-in-hello-vm"></a>Hogyan frissíthetők a hello szimulálása a virtuális gép hello?

Ha végrehajtott módosítások toohello szimuláció, hello PowerShell parancsfájlt használhatja `build.ps1` a hello [tárház](https://github.com/Azure/azure-iot-connected-factory) hello segítségével `updatedimulation` parancs. A parancsfájl hello szimuláció összetevők alkot, hello szimulálása a hello virtuális gép leáll, feltölti, telepíti és elindítja azokat.

### <a name="how-do-i-find-out-hello-connection-string-of-hello-iot-hub-used-by-my-solution"></a>Hogyan állapítható meg a megoldás által használt hello IoT hub hello kapcsolati karakterláncot?

Ha telepítette a megoldást hello `build.ps1` hello parancsfájl [tárház](https://github.com/Azure/azure-iot-connected-factory), hello kapcsolati karakterlánc: hello értékének **IotHubOwnerConnectionString** a hello `<name of your deployment>.config.user` fájlt.

Hello kapcsolati karakterláncot hello Azure-portálon is tájékozódhat. Hello IoT-központ erőforrás hello erőforráscsoporthoz tartozik, a telepítés keresse meg a hello kapcsolatikarakterlánc-beállításokat.

### <a name="which-iot-hub-devices-does-hello-connected-factory-simulation-use"></a>Mely IoT Hub-eszközöknek hello nem csatlakoztatott gyári szimuláció használata?

önkiszolgáló regisztrálja szimuláció hello hello következő eszközökön:

* proxy.Beijing.Corp.contoso
* proxy.capetown.Corp.contoso
* proxy.Mumbai.Corp.contoso
* proxy.munich0.Corp.contoso
* proxy.Rio.Corp.contoso
* proxy.Seattle.Corp.contoso
* Publisher.Beijing.Corp.contoso
* Publisher.capetown.Corp.contoso
* Publisher.Mumbai.Corp.contoso
* Publisher.munich0.Corp.contoso
* Publisher.Rio.Corp.contoso
* Publisher.Seattle.Corp.contoso

Hello segítségével [DeviceExplorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) vagy [IOT hubbal-explorer](https://github.com/azure/iothub-explorer) eszköz, ellenőrizheti, hogy mely eszközök vannak regisztrálva hello IoT-központ a megoldás használ. Ezek az eszközök toouse, szükség van hello kapcsolati karakterlánc hello IoT-központ a környezetben.

### <a name="how-can-i-get-log-data-from-hello-simulation-components"></a>Hogyan kaphatok naplóadatokat hello szimuláció összetevők?

Hello szimuláció naplóadatok toolog fájlban lévő valamennyi összetevőnél. Ezek a fájlok megtalálhatók hello VM hello mappában `home/docker/Logs`. tooretrieve hello naplókat, hello PowerShell parancsfájlt használhatja `Simulation/Factory/Get-SimulationLogs.ps1` a hello [tárház](https://github.com/Azure/azure-iot-connected-factory).

Ezt a parancsfájlt a virtuális gép toohello toosign kell. A hello bejelentkezéshez szükség lehet tooprovide hitelesítő adatokat. Lásd: [hogyan jelentkezzen toohello szimuláció VM?](#how-do-i-sign-in-to-the-simulation-vm) toofind hello hitelesítő adatokat.

hello parancsfájl hozzáadása/eltávolítása egy nyilvános IP-cím toohello virtuális gép, ha az még nincs ilyen, és eltávolítja azt. hello parancsfájl az összes napló fájlokat archiválhatja, és letölti hello archív tooyour fejlesztői munkaállomáson.

Azt is megteheti toohello SSH-kapcsolaton keresztül Virtuálisgép jelentkezni, és vizsgálja meg a naplófájlok hello futásidőben.

### <a name="how-can-i-check-if-hello-simulation-is-sending-data-toohello-cloud"></a>Hogyan ellenőrizheti meg, hogy ha hello szimuláció adatok toohello felhő küld-e?

A hello [DeviceExplorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) vagy hello [IOT hubbal-explorer](https://github.com/azure/iothub-explorer) eszköz, vizsgálhatja hello adatforgalom tooIoT Hub egyes eszközökről. Ezek az eszközök toouse, szükség van tooknow hello kapcsolati karakterlánc hello IoT-központ a környezetben. Lásd: [hogyan állapíthatom meg hello kapcsolati karakterlánca a megoldás által használt hello IoT-központot?](#how-do-i-find-out-the-connection-string-of-the-iot-hub-used-by-my-solution)

Vizsgálja meg az egyik hello publisher eszköz által küldött hello adatokat:

* Publisher.Beijing.Corp.contoso
* Publisher.capetown.Corp.contoso
* Publisher.Mumbai.Corp.contoso
* Publisher.munich0.Corp.contoso
* Publisher.Rio.Corp.contoso
* Publisher.Seattle.Corp.contoso

Ha nincs központi tooIoT küldött adatokat, majd nincs hello szimuláció kapcsolatos problémát. Első lépésként elemzés elemezni kell hello naplófájlok hello szimuláció összetevőt. Lásd: [Hogyan juthatok naplóadatokat hello a szimuláció összetevőket?](#how-can-i-get-log-data-from-the-simulation-components) A következő toostop próbálja, és indítsa el a szimuláció hello, és ha még nem küldött adatok, frissíteni hello szimuláció teljesen. Lásd: [hogyan frissíthetők a hello szimulálása a virtuális gép hello?](#how-do-i-update-the-simulation-in-the-vm)

### <a name="next-steps"></a>Következő lépések

Akkor is is felfedezheti hello más szolgáltatásokat és képességeket hello előre konfigurált IoT Suite megoldások:

* [Előre konfigurált prediktív karbantartási megoldás áttekintése](iot-suite-predictive-overview.md)
* [Előre konfigurált csatlakoztatott gyári megoldási áttekintés](iot-suite-connected-factory-overview.md)
* [A hello IoT biztonsági szabad](securing-iot-ground-up.md)
