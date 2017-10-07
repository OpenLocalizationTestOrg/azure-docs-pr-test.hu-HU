---
title: "az Azure IoT Suite csatlakoztatott aaaDeploy gyári átjáró |} Microsoft Docs"
description: "Hogyan toodeploy Windows vagy Linux tooenable kapcsolat toohello átjáró csatlakoztatva gyári előre konfigurált megoldás."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2017
ms.author: dobett
ms.openlocfilehash: 72436dec60eda0de20143f362fe740b0c4412f36
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-gateway-on-windows-or-linux-for-hello-connected-factory-preconfigured-solution"></a>Telepítsen egy átjárót a Windows vagy Linux csatlakoztatott hello előre konfigurált gyári megoldás

hello szükséges szoftver toodeploy egy átjáró csatlakoztatva hello előre konfigurált gyári megoldás két részből áll:

* Hello *OPC Proxy* hoz létre a kapcsolat tooIoT központ. Hello *OPC Proxy* majd vár hello parancs és a vezérlő üzeneteit integrált hello csatlakoztatott gyári megoldás portálon futó OPC böngésző.
* Hello *OPC Publisher* csatlakoztatja tooexisting helyszíni OPC EE-kiszolgálókat és telemetriai üzenetek továbbítja őket a tooIoT központ.

Mindkét összetevők nyílt forráskódú és érhetők el a Githubon forrásként és a Docker tárolóként:

| GitHub | DockerHub |
| ------ | --------- |
| [OPC-közzétevő][lnk-publisher-github] | [OPC-közzétevő][lnk-publisher-docker] |
| [OPC Proxy][lnk-proxy-github] | [OPC Proxy][lnk-proxy-docker] |

Nincs nyilvánosan elérhető IP-cím vagy a hello átjáró tűzfal lyuk vagy összetevőhöz szükségesek. hello OPC Proxy és OPC szoftvergyártó használja a 443-as, 5671 és 8883 csak kimenő portok.

hello cikkben leírt lépéseket mutatja be egy átjárót Docker akár toodeploy [Windows](#windows-deployment) vagy [Linux](#linux-deployment). hello átjáró lehetővé teszi, hogy a kapcsolat kapcsolódó toohello előre konfigurált gyári megoldás.

> [!NOTE]
> hello átjáró szoftver hello Docker-tároló futó [Azure IoT peremhálózati].

## <a name="windows-deployment"></a>Windows központi telepítése

> [!NOTE]
> Ha még nem rendelkezik egy átjáróeszköz, a Microsoft azt javasolja, vásárol egy kereskedelmi átjáró partnereink egyikéből. A Microsoft hello [Azure IoT eszköz katalógus] átjáró eszközöket hello kompatibilis csatlakozó gyári megoldás. Hajtsa végre a hello vonatkozó utasításokat hello eszköz tooset hello átjáró mentése. Másik megoldásként használhatja a következő utasításokat toomanually állítsa be a meglévő átjáró egyik hello.

### <a name="install-docker"></a>Docker telepítése

Telepítés [Docker for Windows] átjáró Windows-alapú eszközén. Windows Docker a telepítés során válassza ki a gazdagép gép tooshare a Docker egyik meghajtójára. a következő képernyőkép azt mutatja be, a Windows rendszeren hello D meghajtó megosztása hello:

![Docker telepítése][img-install-docker]

Ezután hozzon létre egy nevű **docker** hello hello gyökérmappájában megosztott meghajtót.
Ezt a lépést hajtsa végre a hello docker telepítése után **beállítások** menü.

### <a name="configure-hello-gateway"></a>Hello átjáró konfigurálása

1. Hello kell **iothubowner** kapcsolati karakterlánca a Azure IoT Suite csatlakoztatott gyári telepítési toocomplete hello átjárójának telepítéséhez. A hello [Azure-portálon], keresse meg az IoT-központ hello erőforráscsoportban csatlakoztatott hello gyári megoldás üzembe helyezésekor létrehozott tooyour. Kattintson a **megosztott elérési házirendek** tooaccess hello **iothubowner** kapcsolati karakterlánc:

    ![Az IoT-központ kapcsolati karakterlánc hello keresése][img-hub-connection]

    Másolás hello **kapcsolati karakterlánc – elsődleges kulcs** érték.

1. Az IoT Hub hello két átjáró modulok futtatásával hello átjáró konfigurálása **után** a parancssorból:

    `docker run -it --rm -h <ApplicationName> -v //D/docker:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/CertificateStores -v //D/docker:/root/.dotnet/corefx/cryptography/x509stores microsoft/iot-gateway-opc-ua:1.0.0 <ApplicationName> "<IoTHubOwnerConnectionString>"`

    `docker run -it --rm -v //D/docker:/mapped microsoft/iot-gateway-opc-ua-proxy:0.1.3 -i -c "<IoTHubOwnerConnectionString>" -D /mapped/cs.db`

    * **&lt;ApplicationName&gt;**  hello neve toogive tooyour OPC EE Publisher hello formátumban **publisher.&lt; a teljesen minősített tartománynevét&gt;**. Például, ha a gyári hálózati nevezik **myfactorynetwork.com**, hello **ApplicationName** értéke **publisher.myfactorynetwork.com**.
    * **&lt;IoTHubOwnerConnectionString&gt;**  hello van **iothubowner** hello előző lépésben másolt kapcsolati karakterláncot. Ez a kapcsolati karakterlánc csak ebben a lépésben követően nem kell hello a következő lépéseket:

    Használhat hello leképezett D:\\docker mappa (hello `-v` argumentum) későbbi toopersist hello hello átjáró modulok használata két X.509-tanúsítványokat.

### <a name="run-hello-gateway"></a>Hello átjáró futtatása

1. Indítsa újra a hello átjárót hello a következő parancsok használatával:

    `docker run -it --rm -h <ApplicationName> --expose 62222 -p 62222:62222 -v //D/docker:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/Logs -v //D/docker:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/CertificateStores -v //D/docker:/shared -v //D/docker:/root/.dotnet/corefx/cryptography/x509stores -e _GW_PNFP="/shared/publishednodes.JSON" microsoft/iot-gateway-opc-ua:1.0.0 <ApplicationName>`

    `docker run -it --rm -v //D/docker:/mapped microsoft/iot-gateway-opc-ua-proxy:0.1.3 -D /mapped/cs.db`

1. Biztonsági okokból hello két X.509-tanúsítványokat a hello D: megőrzött\\docker mappát tartalmaz hello titkos kulcsot. Korlátozza a hozzáférést toothis mappa toohello hitelesítő adatait (jellemzően **rendszergazdák**) toorun hello Docker-tároló használja. Kattintson a jobb gombbal hello D:\\docker mappát, válassza a **tulajdonságok**, majd **biztonsági**, majd **szerkesztése**. Adjon **rendszergazdák** teljes hozzáférés, és távolítsa el a többi felhasználó:

    ![Támogatás engedélyek tooDocker megosztás][img-docker-share]

1. Ellenőrizze a hálózati kapcsolatot. A parancssorból, írja be a hello parancsot `ping publisher.<your fully qualified domain name>` tooping az átjárót. Ha hello cél nem érhető el, adja hozzá a hello IP-cím és az átjáró toohello hosts fájl meg az átjáró nevét. hello hosts fájl található hello **Windows\\System32\\illesztőprogramok\\stb** mappát.

1. Ezt követően próbálja tooconnect toohello publisher hello átjáró futó helyi OPC EE-ügyfél használatával. hello OPC EE-végpont URL-cím `opc.tcp://publisher.<your fully qualified domain name>:62222`. Ha egy OPC EE-ügyfél nincs telepítve, töltse le és használja az [nyílt forráskódú OPC EE ügyfél].

1. Ha ezek a helyi tesztek sikeresen befejeződött, keresse meg a toohello **csatlakoztassa a saját OPC EE kiszolgálót** lapjára hello csatlakoztatott gyári megoldás. Adja meg a hello publisher végponti URL-cím (`tcp://publisher.<your fully qualified domain name>:62222`), majd **Connect**. Egy tanúsítvány figyelmeztetés, majd kattintson a **folytatása.** Ezután hibaüzenetet kap, hogy hello publisher hello EE webes ügyfél nem megbízható. tooresolve a hiba, a Másolás hello **EE-webügyfél** hello tanúsítványt **D:\\docker\\elutasított tanúsítványok\\Tanúsítványos** mappa toohello **D:\\docker\\EE-alkalmazások\\Tanúsítványos** hello átjáró mappájába. Nem kell toorestart hello átjáró. Ismételje meg ezt a lépést. Csatlakoztathatja a toohello átjáró hello felhőből, és készen áll a tooadd OPC EE kiszolgálók toohello megoldás.

### <a name="add-your-opc-ua-servers"></a>A OPC EE-kiszolgálók hozzáadása

1. Keresse meg a toohello **csatlakoztassa a saját OPC EE kiszolgálót** lapjára hello csatlakoztatott gyári megoldás. Ugyanezek a lépések szakasz tooestablish megelőző hello hasonlóan csatlakoztatott gyári portal hello és hello OPC EE-kiszolgáló közötti megbízható hello kövesse. Ebben a lépésben a hello hello tanúsítványok kölcsönös megbízható gyári portal és hello OPC EE-kiszolgáló csatlakoztatva, és kapcsolatot hoz létre hoz létre.

1. Keresse a hello OPC EE csomópontok fájának OPC EE-kiszolgálóját, kattintson a jobb gombbal a hello OPC csomópontok, és válassza ki **közzététele**. Az ilyen módon toowork közzétételi hello OPC EE-kiszolgáló és hello publisher hello kell lennie ugyanazon a hálózaton. Ez azt jelenti, ha hello teljesen minősített tartománynév hello kiadó lesz **publisher.mydomain.com** majd teljesen minősített tartománynevét hello OPC EE kiszolgálónak kell lennie, például hello **myopcuaserver.mydomain.com**. Ha a telepítő különböző, kézzel is felveheti a csomópontok toohello publishesnodes.json fájl található a hello **D:\\docker** mappa. hello publishesnodes.json fájl automatikusan létrejön a hello először sikeres közzététele egy OPC-csomópont.

1. Telemetria most forgalomáramlás hello átjáró eszközről. Hello telemetriai megtekintheti a hello **gyári helyek** hello csatlakoztatott gyári portál ábrázolása **új előállító**.

## <a name="linux-deployment"></a>Linux-telepítés

> [!NOTE]
> Ha még nem rendelkezik egy átjáróeszköz, a Microsoft azt javasolja, vásárol egy kereskedelmi átjáró partnereink egyikéből. A Microsoft hello [Azure IoT eszköz katalógus] átjáró eszközöket hello kompatibilis csatlakozó gyári megoldás. Hajtsa végre a hello vonatkozó utasításokat hello eszköz tooset hello átjáró mentése. Másik megoldásként használhatja a következő utasításokat toomanually állítsa be a meglévő átjáró egyik hello.

[Telepítse a Docker] Linux átjáró eszközén.

### <a name="configure-hello-gateway"></a>Hello átjáró konfigurálása

1. Hello kell **iothubowner** kapcsolati karakterlánca a Azure IoT Suite csatlakoztatott gyári telepítési toocomplete hello átjárójának telepítéséhez. A hello [Azure-portálon], keresse meg az IoT-központ hello erőforráscsoportban csatlakoztatott hello gyári megoldás üzembe helyezésekor létrehozott tooyour. Kattintson a **megosztott elérési házirendek** tooaccess hello **iothubowner** kapcsolati karakterlánc:

    ![Az IoT-központ kapcsolati karakterlánc hello keresése][img-hub-connection]

    Másolás hello **kapcsolati karakterlánc – elsődleges kulcs** érték.

1. Az IoT Hub hello két átjáró modulok futtatásával hello átjáró konfigurálása **után** egy rendszerhéjból rendelkező:

    `sudo docker run -it --rm -h <ApplicationName> -v /shared:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/ -v /shared:/root/.dotnet/corefx/cryptography/x509stores microsoft/iot-gateway-opc-ua:1.0.0 <ApplicationName> "<IoTHubOwnerConnectionString>"`

    `sudo docker run --rm -it -v /shared:/mapped microsoft/iot-gateway-opc-ua-proxy:0.1.3 -i -c "<IoTHubOwnerConnectionString>" -D /mapped/cs.db`

    * **&lt;ApplicationName&gt;**  OPC EE hello Alkalmazásátjáró hello formátumú hoz létre hello hello neve **publisher.&lt; a teljesen minősített tartománynevét&gt;**. Például **publisher.microsoft.com**.
    * **&lt;IoTHubOwnerConnectionString&gt;**  hello van **iothubowner** hello előző lépésben másolt kapcsolati karakterláncot. Ez a kapcsolati karakterlánc csak ebben a lépésben követően nem kell hello a következő lépéseket:

    Hello használata **/ megosztott** mappa (hello `-v` argumentum) későbbi toopersist hello hello átjáró modulok használata két X.509-tanúsítványokat.

### <a name="run-hello-gateway"></a>Hello átjáró futtatása

1. Indítsa újra a hello átjárót hello a következő parancsok használatával:

    `sudo docker run -it -h <ApplicationName> --expose 62222 -p 62222:62222 –-rm -v /shared:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/Logs -v /shared:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/CertificateStores -v /shared:/shared -v /shared:/root/.dotnet/corefx/cryptography/x509stores -e _GW_PNFP="/shared/publishednodes.JSON" microsoft/iot-gateway-opc-ua:1.0.0 <ApplicationName>`

    `sudo docker run -it -v /shared:/mapped microsoft/iot-gateway-opc-ua-proxy:0.1.3 -D /mapped/cs.db`

1. Biztonsági okokból hello két X.509-tanúsítványokat a hello megőrzött **/ megosztott** mappát tartalmaz hello titkos kulcsot. Korlát hozzáférés toothis mappa toohello hitelesítő adatokkal kell toorun hello Docker-tároló. tooset hello engedélyeinek **legfelső szintű** csak, használja a hello `chmod` rendszerhéj-parancs hello mappára.

1. Ellenőrizze a hálózati kapcsolatot. Adja meg a rendszerhéj hello parancs `ping publisher.<your fully qualified domain name>` tooping az átjárót. Ha hello cél nem érhető el, adja hozzá a hello IP-cím és az átjáró tooyour hosts fájl meg az átjáró nevét. hello hosts fájl található hello **/stb** mappát.

1. Ezt követően próbálja tooconnect toohello publisher hello átjáró futó helyi OPC EE-ügyfél használatával. hello OPC EE-végpont URL-cím `opc.tcp://publisher.<your fully qualified domain name>:62222`. Ha egy OPC EE-ügyfél nincs telepítve, töltse le és használja az [nyílt forráskódú OPC EE ügyfél].

1. Ha ezek a helyi tesztek sikeresen befejeződött, keresse meg a toohello **csatlakoztassa a saját OPC EE kiszolgálót** lapjára hello csatlakoztatott gyári megoldás. Adja meg a hello publisher végponti URL-cím (`tcp://publisher.<your fully qualified domain name>:62222`), majd **Connect**. Egy tanúsítvány figyelmeztetés, majd kattintson a **folytatása.** Ezután hibaüzenetet kap, hogy hello publisher hello EE webes ügyfél nem megbízható. tooresolve a hiba, a Másolás hello **EE-webügyfél** hello tanúsítványt **/megosztott/elutasított tanúsítványok/Tanúsítványos** mappa toohello **/shared/UA alkalmazások/Tanúsítványos** hello átjáró mappájába. Nem kell toorestart hello átjáró. Ismételje meg ezt a lépést. Csatlakoztathatja a toohello átjáró hello felhőből, és készen áll a tooadd OPC EE kiszolgálók toohello megoldás.

### <a name="add-your-opc-ua-servers"></a>A OPC EE-kiszolgálók hozzáadása

1. Keresse meg a toohello **csatlakoztassa a saját OPC EE kiszolgálót** lapjára hello csatlakoztatott gyári megoldás. Ugyanezek a lépések szakasz tooestablish megelőző hello hasonlóan csatlakoztatott gyári portal hello és hello OPC EE-kiszolgáló közötti megbízható hello kövesse. Ebben a lépésben a hello hello tanúsítványok kölcsönös megbízható gyári portal és hello OPC EE-kiszolgáló csatlakoztatva, és kapcsolatot hoz létre hoz létre.

1. Keresse a hello OPC EE csomópontok fájának OPC EE-kiszolgálóját, kattintson a jobb gombbal a hello OPC csomópontok, és válassza ki **közzététele**. Az ilyen módon toowork közzétételi hello OPC EE-kiszolgáló és hello publisher hello kell lennie ugyanazon a hálózaton. Ez azt jelenti, ha hello teljesen minősített tartománynév hello kiadó lesz **publisher.mydomain.com** majd teljesen minősített tartománynevét hello OPC EE kiszolgálónak kell lennie, például hello **myopcuaserver.mydomain.com**. Ha a telepítő különböző, kézzel is felveheti a csomópontok toohello publishesnodes.json fájl található a hello **/ megosztott** mappát. hello publishesnodes.json automatikusan létrejön a hello először sikeres közzététele egy OPC-csomópont.

1. Telemetria most forgalomáramlás hello átjáró eszközről. Hello telemetriai megtekintheti a hello **gyári helyek** hello csatlakoztatott gyári portál ábrázolása **új előállító**.

## <a name="next-steps"></a>Következő lépések

hello csatlakoztatott gyári hello architektúrájával kapcsolatos további információkért toolearn előre konfigurált megoldás című [csatlakoztatott gyári előre konfigurált megoldás forgatókönyv][lnk-walkthrough].

[img-install-docker]: ./media/iot-suite-connected-factory-gateway-deployment/image1.png
[img-hub-connection]: ./media/iot-suite-connected-factory-gateway-deployment/image2.png
[img-docker-share]: ./media/iot-suite-connected-factory-gateway-deployment/image3.png

[Docker for Windows]: https://www.docker.com/docker-windows
[Azure IoT eszköz katalógus]: https://catalog.azureiotsuite.com/?q=opc
[Azure-portálon]: http://portal.azure.com/
[nyílt forráskódú OPC EE ügyfél]: https://github.com/OPCFoundation/UA-.NETStandardLibrary/tree/master/SampleApplications/Samples/Client.Net4
[Telepítse a Docker]: https://www.docker.com/community-edition#/download
[lnk-walkthrough]: iot-suite-connected-factory-sample-walkthrough.md
[Azure IoT peremhálózati]: https://github.com/Azure/iot-edge

[lnk-publisher-github]: https://github.com/Azure/iot-edge-opc-publisher
[lnk-publisher-docker]: https://hub.docker.com/r/microsoft/iot-gateway-opc-ua
[lnk-proxy-github]: https://github.com/Azure/iot-edge-opc-proxy
[lnk-proxy-docker]: https://hub.docker.com/r/microsoft/iot-gateway-opc-ua-proxy