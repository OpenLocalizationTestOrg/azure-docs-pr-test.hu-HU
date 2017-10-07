---
title: "egy tároló tooAzure Service Fabric a .NET-alkalmazás aaaDeploy |} Microsoft Docs"
description: "Hogyan útmutatást ad meg toopackage egy Docker-tároló a Visual Studio .NET-alkalmazás. Az új \"container\" alkalmazás telepítve van a majd tooa Service Fabric-fürt."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/19/2017
ms.author: mikhegn
ms.openlocfilehash: 094b0e71d76b2e56ffb9b23638dd8154b3aff5fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-net-application-in-a-windows-container-tooazure-service-fabric"></a>A Windows-tároló tooAzure Service Fabric a .NET-alkalmazás központi telepítése

Az oktatóanyag bemutatja, hogyan toodeploy meglévő ASP.NET-alkalmazások az Azure-on Windows tároló.

Eben az oktatóanyagban az alábbiakkal fog megismerkedni:

> [!div class="checklist"]
> * Hozzon létre egy Docker-projektet a Visual Studióban
> * Egy meglévő alkalmazást containerize
> * A telepítő folyamatos integráció a Visual Studio és a VSTS

## <a name="prerequisites"></a>Előfeltételek

1. Telepítés [Docker CE Windows](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description) , hogy a Windows 10-tárolók is futtathatja.
2. Ismerkedjen meg hello [Windows 10-tárolók gyors üzembe helyezés][link-container-quickstart].
3. Töltse le a hello [Fabrikam Fiber CallCenter] [ link-fabrikam-github] mintaalkalmazást.
4. Telepítés [Azure PowerShell][link-azure-powershell-install]
5. Telepítse a hello [Visual Studio 2017 folyamatos kézbesítési eszközök kiterjesztése][link-visualstudio-cd-extension]
6. Hozzon létre egy [Azure-előfizetés] [ link-azure-subscription] és egy [Visual Studio Team Services-fiók][link-vsts-account]. 
7. [Fürt létrehozása az Azure-on](service-fabric-tutorial-create-cluster-azure-ps.md)

## <a name="containerize-hello-application"></a>Hello alkalmazás containerize

Most, hogy egy [Azure Service Fabric-fürt fut](service-fabric-tutorial-create-cluster-azure-ps.md) készen toocreate és indexelése alkalmazás központi telepítése. az alkalmazás fut a tárolóban lévő toostart tooadd kell **Docker támogatási** toohello projektre a Visual Studio. Amikor felvesz **Docker támogatási** toohello alkalmazás két dolog történik. Először egy _Dockerfile_ toohello projekt kerül. Ez az új fájl leírja, hogy hello tároló kép beépített toobe. Majd a második, egy új _docker compose_ projekt toohello megoldás kerül. hello új projekt tartalmaz néhány docker compose fájlokat. Docker compose-fájlok lehetnek használt toodescribe hello tároló futtatásának módját.

További információ a használata [Visual Studio tároló eszközök][link-visualstudio-container-tools].

>[!NOTE]
>Ha hello első alkalommal futtatja a Windows-tároló lemezképek a számítógépen, Docker CE kell lekérik a tárolók skálázása hello alap képek. Ebben az oktatóanyagban használt hello képek 14 GB. Lépjen tovább, és futtassa a következő terminál parancs toopull hello alap képek hello:
>```cmd
>docker pull microsoft/mssql-server-windows-developer
>docker pull microsoft/aspnet:4.6.2
>```

### <a name="add-docker-support"></a>Adja hozzá a Docker-támogatás

Nyissa meg hello [FabrikamFiber.CallCenter.sln] [ link-fabrikam-github] fájlt a Visual Studióban.

Kattintson a jobb gombbal hello **FabrikamFiber.Web** project > **Hozzáadás** > **Docker támogatási**.

### <a name="add-support-for-sql"></a>Az SQL támogatását

Ez az alkalmazás használja az SQL hello adatok szolgáltatóként, így egy SQL server szükséges toorun hello alkalmazás. Egy SQL Server tároló lemezképet a docker-compose.override.yml fájl hivatkozik.

A Visual Studióban nyissa meg a **Megoldáskezelőben**, található **docker compose**, és nyissa meg hello fájl **docker-compose.override.yml**.

Keresse meg a toohello `services:` csomópont, nevű csomópont hozzáadása `db:` , amely meghatározza, hogy hello SQL Server bejegyzés hello tároló.

```yml
  db:
    image: microsoft/mssql-server-windows-developer
    environment:
      sa_password: "Password1"
      ACCEPT_EULA: "Y"
    ports:
      - "1433"
    healthcheck:
      test: [ "CMD", "sqlcmd", "-U", "sa", "-P", "Password1", "-Q", "select 1" ]
      interval: 1s
      retries: 20
```

>[!NOTE]
>Bármely SQL Server helyi hibakeresési inkább használható, amíg a gazdagép elérhető legyen. Azonban **localdb** nem támogatja a `container -> host` kommunikációt.

>[!WARNING]
>A tárolóban lévő SQL Server szoftvert futtató nem támogatja az adatait. Hello tároló leállásakor a adat törlődik. Termelési környezetben ne használja ezt a konfigurációt.

Keresse meg a toohello `fabrikamfiber.web:` csomópont, és adja hozzá a nevű gyermekcsomópontja `depends_on:`. Ez biztosítja, hogy hello `db` (hello SQL Server tároló) szolgáltatás elindulása előtt a webalkalmazást (fabrikamfiber.web).

```yml
  fabrikamfiber.web:
    depends_on:
      - db
```

### <a name="update-hello-web-config"></a>Hello webkonfiguráció frissítése

Vissza a hello **FabrikamFiber.Web** projektre, frissítés hello kapcsolati karakterláncot a hello **web.config** fájlt, toopoint toohello hello tároló SQL-kiszolgálót.

```xml
<add name="FabrikamFiber-Express" connectionString="Data Source=db,1433;Database=FabrikamFiber;User Id=sa;Password=Password1;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />

<add name="FabrikamFiber-DataWarehouse" connectionString="Data Source=db,1433;Database=FabrikamFiber;User Id=sa;Password=Password1;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />
```

>[!NOTE]
>Ha azt szeretné, hogy a webalkalmazás létrehozása egy másik SQL Server kiadási fejlesztéskor toouse, adjon hozzá egy másik kapcsolati karakterlánc tooyour web.release.config fájlt.

### <a name="test-your-container"></a>A tároló tesztelése

Nyomja le az **F5** toorun és hibakeresési hello alkalmazás a tárolóban.

Peremhálózati az alkalmazás meghatározott indítási lap hello tároló hello IP-cím használata hello belső NAT-hálózaton (általában 172.x.x.x) megnyitása. További információ a Visual Studio 2017 használatával tárolókban lévő alkalmazások hibakeresése toolearn lásd: [Ez a cikk][link-debug-container].

![fabrikam tároló – példa][image-web-preview]

hello tároló már készen áll a toobe épül, és a Service Fabric-alkalmazás csomagolva. Miután hello tároló kép összeállítása a számítógépre, tooany tároló beállításjegyzék leküldeni, és lekérik a tooany állomás toorun.

## <a name="get-hello-application-ready-for-hello-cloud"></a>Felkészülés az alkalmazás hello hello felhő

tooget hello alkalmazás készen áll az Azure Service Fabric futó, igazolnia kell a toocomplete két lépésből áll:

1. Ha szeretnénk toobe képes tooreach hello Service Fabric-fürt a webes alkalmazás hello port tenni.
2. Adja meg az alkalmazás üzemi készen SQL-adatbázis.

### <a name="expose-hello-port-for-hello-app"></a>Teszi közzé a hello alkalmazás hello port
hello Service Fabric-fürt rendelkezik konfigurált,-porttal rendelkezik *80* nyissa meg az alapértelmezés szerint a hello Azure Load Balancer, elosztja a bejövő forgalom toohello fürt. Is elérhetővé kell tenni a tárolót a porton keresztül a docker-compose.yml fájlt.

A Visual Studióban nyissa meg a **Megoldáskezelőben**, található **docker compose**, és nyissa meg hello fájl **docker-compose.override.yml**.

Módosítsa a hello `fabrikamfiber.web:` csomópont, nevű alárendelt csomópont hozzáadása `ports:`.

Adjon hozzá egy karakterlánc-bejegyzést `- "80:80"`.

```yml
  version: '3'

  services:
    fabrikamfiber.web:
      image: fabrikamfiber.web
      build:
        context: .\FabrikamFiber.Web
        dockerfile: Dockerfile
      ports:
        - "80:80"
```

### <a name="use-a-production-sql-database"></a>SQL-adatbázis használata
Ha éles környezetben fut, igazolnia kell-e az adatok maradnak az adatbázisban. Jelenleg nincs módja tooguarantee állandó adatokat tároló, ezért nem tárolhatók termelési adatok a tárolóban lévő SQL Server.

Azt javasoljuk, hogy egy Azure SQL Database használják. tooset fel, és futtassa az Azure-ban kezelt SQL Server látogasson el a hello [Azure SQL Database – Gyorsútmutatók] [ link-azure-sql] cikk.

>[!NOTE]
>Ne feledje toochange hello kapcsolati karakterláncok toohello SQL server hello **web.release.config** hello fájlban **FabrikamFiber.Web** projekt.
>
>Ez az alkalmazás szabályosan meghiúsul, ha az SQL-adatbázis nem érhető el. Válassza ki azokat, amelyek toogo, és a nem SQL server hello alkalmazás központi telepítését.

## <a name="deploy-with-visual-studio-team-services"></a>A Visual Studio Team Services telepítéséhez

tooset központi telepítése a Visual Studio Team Services használata szükséges, kell tooinstall hello [folyamatos kézbesítési eszközök bővítményt a Visual Studio 2017][link-visualstudio-cd-extension]. A bővítmény lehetővé teszi könnyen toodeploy tooAzure úgy konfigurálja a Visual Studio Team Services és a telepített alkalmazás tooyour Service Fabric-fürt beolvasása.

tooget indult el, a kódot kell futnia a verziókövetési rendszerrel. Ez a szakasz többi hello azt feltételezi, hogy **git** használatban van.

### <a name="set-up-a-vsts-repo"></a>Egy VSTS-tárház beállítása
Visual Studio hello jobb alsó sarkában kattintson **tooSource vezérlő hozzáadása** > **Git** (vagy célszerű beállítás).

![hello forrás vezérlő gombra][image-source-control]

A hello _Team Explorer_ ablaktáblában kattintson az **Git-tárház közzététele**.

Válassza ki a VSTS-tárház nevét, és nyomja le az **tárház**.

![tárház tooVSTS közzététele][image-publish-repo]

Most, hogy a kód egy VSTS-forrás tárházat szinkronizálva van, beállíthatja a folyamatos integrációt és folyamatos kézbesítését.

### <a name="setup-continuous-delivery"></a>A telepítő folyamatos kézbesítés

A _Megoldáskezelőben_, kattintson a jobb gombbal hello **megoldás** > **konfigurálása a folyamatos kézbesítési**.

Válassza ki a hello Azure-előfizetést.

Állítsa be **állomás típusa** túl**Service Fabric-fürt**.

Állítsa be **célállomás** toohello service fabric-fürt hello előző szakaszban létrehozott.

Válasszon egy **tároló beállításjegyzék** toopublish a tároló számára.

>[!TIP]
>Használjon hello **szerkesztése** gomb toocreate tároló beállításjegyzékbeli.

Kattintson az **OK** gombra.

![a telepítő service fabric folyamatos integrációt][image-setup-ci]
   
   Ha hello konfigurációs befejeződött, a tároló telepített tooService háló. Amikor a frissítések toohello tárház leküldéses végrehajtása egy új build és kiadása.
   
   >[!NOTE]
   >Épület hello tároló képek körülbelül 15 percet igénybe vehet.
   >hello első központi telepítési toohello Service Fabric-fürt hello alap Windows Server Core tároló képek toobe letöltött okoz. hello letöltési toocomplete további 5-10 percet vesz igénybe.

Keresse meg a fürt hello URL-cím segítségével toohello Fabrikam ügyfélszolgálatával alkalmazás: például *http://mycluster.westeurope.cloudapp.azure.com*

Most, hogy indexelése, és központilag telepített hello Fabrikam ügyfélszolgálatával megoldás, megnyithatja hello [Azure-portálon] [ link-azure-portal] és tekintse meg a Service Fabric hello alkalmazást. tootry hello alkalmazás, nyisson meg egy webböngészőt, és nyissa meg a Service Fabric-fürt toohello URL-CÍMÉT.

## <a name="next-steps"></a>Következő lépések

Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * Hozzon létre egy Docker-projektet a Visual Studióban
> * Egy meglévő alkalmazást containerize
> * A telepítő folyamatos integráció a Visual Studio és a VSTS

<!--   NOTE SURE WHAT WE SHOULD DO YET HERE

Advance toohello next tutorial toolearn how toobind a custom SSL certificate tooit.

> [!div class="nextstepaction"]
> [Bind an existing custom SSL certificate tooAzure Web Apps](app-service-web-tutorial-custom-ssl.md)

## Next steps

- [Container Tooling in Visual Studio][link-visualstudio-container-tools]
- [Get started with containers in Service Fabric][link-servicefabric-containers]
- [Creating Service Fabric applications][link-servicefabric-createapp]
-->

[link-debug-container]: /dotnet/articles/core/docker/visual-studio-tools-for-docker
[link-fabrikam-github]: https://aka.ms/fabrikamcontainer
[link-container-quickstart]: /virtualization/windowscontainers/quick-start/quick-start-windows-10
[link-visualstudio-container-tools]: /dotnet/articles/core/docker/visual-studio-tools-for-docker
[link-azure-powershell-install]: /powershell/azure/install-azurerm-ps
[link-servicefabric-create-secure-clusters]: service-fabric-cluster-creation-via-arm.md
[link-visualstudio-cd-extension]: https://aka.ms/cd4vs
[link-servicefabric-containers]: service-fabric-get-started-containers.md
[link-servicefabric-createapp]: service-fabric-create-your-first-application-in-visual-studio.md
[link-azure-portal]: https://portal.azure.com
[link-sf-clustertemplate]: https://aka.ms/securepreviewonelineclustertemplate
[link-azure-pricing-calculator]: https://azure.microsoft.com/en-us/pricing/calculator/
[link-azure-subscription]: https://azure.microsoft.com/en-us/free/
[link-vsts-account]: https://www.visualstudio.com/team-services/pricing/
[link-azure-sql]: /azure/sql-database/

[image-web-preview]: media/service-fabric-host-app-in-a-container/fabrikam-web-sample.png
[image-source-control]: media/service-fabric-host-app-in-a-container/add-to-source-control.png
[image-publish-repo]: media/service-fabric-host-app-in-a-container/publish-repo.png
[image-setup-ci]: media/service-fabric-host-app-in-a-container/configure-continuous-integration.png
