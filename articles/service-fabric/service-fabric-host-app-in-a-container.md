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
# <a name="deploy-a-net-application-in-a-windows-container-tooazure-service-fabric"></a><span data-ttu-id="6ca91-104">A Windows-tároló tooAzure Service Fabric a .NET-alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="6ca91-104">Deploy a .NET application in a Windows container tooAzure Service Fabric</span></span>

<span data-ttu-id="6ca91-105">Az oktatóanyag bemutatja, hogyan toodeploy meglévő ASP.NET-alkalmazások az Azure-on Windows tároló.</span><span class="sxs-lookup"><span data-stu-id="6ca91-105">This tutorial shows you how toodeploy an existing ASP.NET application in a Windows container on Azure.</span></span>

<span data-ttu-id="6ca91-106">Eben az oktatóanyagban az alábbiakkal fog megismerkedni:</span><span class="sxs-lookup"><span data-stu-id="6ca91-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6ca91-107">Hozzon létre egy Docker-projektet a Visual Studióban</span><span class="sxs-lookup"><span data-stu-id="6ca91-107">Create a Docker project in Visual Studio</span></span>
> * <span data-ttu-id="6ca91-108">Egy meglévő alkalmazást containerize</span><span class="sxs-lookup"><span data-stu-id="6ca91-108">Containerize an existing application</span></span>
> * <span data-ttu-id="6ca91-109">A telepítő folyamatos integráció a Visual Studio és a VSTS</span><span class="sxs-lookup"><span data-stu-id="6ca91-109">Setup continuous integration with Visual Studio and VSTS</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6ca91-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6ca91-110">Prerequisites</span></span>

1. <span data-ttu-id="6ca91-111">Telepítés [Docker CE Windows](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description) , hogy a Windows 10-tárolók is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="6ca91-111">Install [Docker CE for Windows](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description) so that you can run containers on Windows 10.</span></span>
2. <span data-ttu-id="6ca91-112">Ismerkedjen meg hello [Windows 10-tárolók gyors üzembe helyezés][link-container-quickstart].</span><span class="sxs-lookup"><span data-stu-id="6ca91-112">Familiarize yourself with hello [Windows 10 Containers quickstart][link-container-quickstart].</span></span>
3. <span data-ttu-id="6ca91-113">Töltse le a hello [Fabrikam Fiber CallCenter] [ link-fabrikam-github] mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6ca91-113">Download hello [Fabrikam Fiber CallCenter][link-fabrikam-github] sample application.</span></span>
4. <span data-ttu-id="6ca91-114">Telepítés [Azure PowerShell][link-azure-powershell-install]</span><span class="sxs-lookup"><span data-stu-id="6ca91-114">Install [Azure PowerShell][link-azure-powershell-install]</span></span>
5. <span data-ttu-id="6ca91-115">Telepítse a hello [Visual Studio 2017 folyamatos kézbesítési eszközök kiterjesztése][link-visualstudio-cd-extension]</span><span class="sxs-lookup"><span data-stu-id="6ca91-115">Install hello [Continuous Delivery Tools extension for Visual Studio 2017][link-visualstudio-cd-extension]</span></span>
6. <span data-ttu-id="6ca91-116">Hozzon létre egy [Azure-előfizetés] [ link-azure-subscription] és egy [Visual Studio Team Services-fiók][link-vsts-account].</span><span class="sxs-lookup"><span data-stu-id="6ca91-116">Create an [Azure subscription][link-azure-subscription] and a [Visual Studio Team Services account][link-vsts-account].</span></span> 
7. [<span data-ttu-id="6ca91-117">Fürt létrehozása az Azure-on</span><span class="sxs-lookup"><span data-stu-id="6ca91-117">Create a cluster on Azure</span></span>](service-fabric-tutorial-create-cluster-azure-ps.md)

## <a name="containerize-hello-application"></a><span data-ttu-id="6ca91-118">Hello alkalmazás containerize</span><span class="sxs-lookup"><span data-stu-id="6ca91-118">Containerize hello application</span></span>

<span data-ttu-id="6ca91-119">Most, hogy egy [Azure Service Fabric-fürt fut](service-fabric-tutorial-create-cluster-azure-ps.md) készen toocreate és indexelése alkalmazás központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="6ca91-119">Now that you have a [Service Fabric cluster is running in Azure](service-fabric-tutorial-create-cluster-azure-ps.md) you are ready toocreate and deploy a containerized application.</span></span> <span data-ttu-id="6ca91-120">az alkalmazás fut a tárolóban lévő toostart tooadd kell **Docker támogatási** toohello projektre a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6ca91-120">toostart running our application in a container, we need tooadd **Docker Support** toohello project in Visual Studio.</span></span> <span data-ttu-id="6ca91-121">Amikor felvesz **Docker támogatási** toohello alkalmazás két dolog történik.</span><span class="sxs-lookup"><span data-stu-id="6ca91-121">When you add **Docker support** toohello application, two things happen.</span></span> <span data-ttu-id="6ca91-122">Először egy _Dockerfile_ toohello projekt kerül.</span><span class="sxs-lookup"><span data-stu-id="6ca91-122">First, a _Dockerfile_ is added toohello project.</span></span> <span data-ttu-id="6ca91-123">Ez az új fájl leírja, hogy hello tároló kép beépített toobe.</span><span class="sxs-lookup"><span data-stu-id="6ca91-123">This new file describes how hello container image is toobe built.</span></span> <span data-ttu-id="6ca91-124">Majd a második, egy új _docker compose_ projekt toohello megoldás kerül.</span><span class="sxs-lookup"><span data-stu-id="6ca91-124">Then second, a new _docker-compose_ project is added toohello solution.</span></span> <span data-ttu-id="6ca91-125">hello új projekt tartalmaz néhány docker compose fájlokat.</span><span class="sxs-lookup"><span data-stu-id="6ca91-125">hello new project contains a few docker-compose files.</span></span> <span data-ttu-id="6ca91-126">Docker compose-fájlok lehetnek használt toodescribe hello tároló futtatásának módját.</span><span class="sxs-lookup"><span data-stu-id="6ca91-126">Docker-compose files can be used toodescribe how hello container is run.</span></span>

<span data-ttu-id="6ca91-127">További információ a használata [Visual Studio tároló eszközök][link-visualstudio-container-tools].</span><span class="sxs-lookup"><span data-stu-id="6ca91-127">More info on working with [Visual Studio Container Tools][link-visualstudio-container-tools].</span></span>

>[!NOTE]
><span data-ttu-id="6ca91-128">Ha hello első alkalommal futtatja a Windows-tároló lemezképek a számítógépen, Docker CE kell lekérik a tárolók skálázása hello alap képek.</span><span class="sxs-lookup"><span data-stu-id="6ca91-128">If it is hello first time you are running Windows container images on your computer, Docker CE must pull down hello base images for your containers.</span></span> <span data-ttu-id="6ca91-129">Ebben az oktatóanyagban használt hello képek 14 GB.</span><span class="sxs-lookup"><span data-stu-id="6ca91-129">hello images used in this tutorial are 14 GB.</span></span> <span data-ttu-id="6ca91-130">Lépjen tovább, és futtassa a következő terminál parancs toopull hello alap képek hello:</span><span class="sxs-lookup"><span data-stu-id="6ca91-130">Go ahead and run hello following terminal command toopull hello base images:</span></span>
>```cmd
>docker pull microsoft/mssql-server-windows-developer
>docker pull microsoft/aspnet:4.6.2
>```

### <a name="add-docker-support"></a><span data-ttu-id="6ca91-131">Adja hozzá a Docker-támogatás</span><span class="sxs-lookup"><span data-stu-id="6ca91-131">Add Docker support</span></span>

<span data-ttu-id="6ca91-132">Nyissa meg hello [FabrikamFiber.CallCenter.sln] [ link-fabrikam-github] fájlt a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="6ca91-132">Open hello [FabrikamFiber.CallCenter.sln][link-fabrikam-github] file in Visual Studio.</span></span>

<span data-ttu-id="6ca91-133">Kattintson a jobb gombbal hello **FabrikamFiber.Web** project > **Hozzáadás** > **Docker támogatási**.</span><span class="sxs-lookup"><span data-stu-id="6ca91-133">Right-click hello **FabrikamFiber.Web** project > **Add** > **Docker Support**.</span></span>

### <a name="add-support-for-sql"></a><span data-ttu-id="6ca91-134">Az SQL támogatását</span><span class="sxs-lookup"><span data-stu-id="6ca91-134">Add support for SQL</span></span>

<span data-ttu-id="6ca91-135">Ez az alkalmazás használja az SQL hello adatok szolgáltatóként, így egy SQL server szükséges toorun hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="6ca91-135">This application uses SQL as hello data provider, so a SQL server is required toorun hello application.</span></span> <span data-ttu-id="6ca91-136">Egy SQL Server tároló lemezképet a docker-compose.override.yml fájl hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="6ca91-136">Reference a SQL Server container image in our docker-compose.override.yml file.</span></span>

<span data-ttu-id="6ca91-137">A Visual Studióban nyissa meg a **Megoldáskezelőben**, található **docker compose**, és nyissa meg hello fájl **docker-compose.override.yml**.</span><span class="sxs-lookup"><span data-stu-id="6ca91-137">In Visual Studio, open **Solution Explorer**, find **docker-compose**, and open hello file **docker-compose.override.yml**.</span></span>

<span data-ttu-id="6ca91-138">Keresse meg a toohello `services:` csomópont, nevű csomópont hozzáadása `db:` , amely meghatározza, hogy hello SQL Server bejegyzés hello tároló.</span><span class="sxs-lookup"><span data-stu-id="6ca91-138">Navigate toohello `services:` node, add a node named `db:` that defines hello SQL Server entry for hello container.</span></span>

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
><span data-ttu-id="6ca91-139">Bármely SQL Server helyi hibakeresési inkább használható, amíg a gazdagép elérhető legyen.</span><span class="sxs-lookup"><span data-stu-id="6ca91-139">You can use any SQL Server you prefer for local debugging, as long as it is reachable from your host.</span></span> <span data-ttu-id="6ca91-140">Azonban **localdb** nem támogatja a `container -> host` kommunikációt.</span><span class="sxs-lookup"><span data-stu-id="6ca91-140">However, **localdb** does not support `container -> host` communication.</span></span>

>[!WARNING]
><span data-ttu-id="6ca91-141">A tárolóban lévő SQL Server szoftvert futtató nem támogatja az adatait.</span><span class="sxs-lookup"><span data-stu-id="6ca91-141">Running SQL Server in a container does not support persisting data.</span></span> <span data-ttu-id="6ca91-142">Hello tároló leállásakor a adat törlődik.</span><span class="sxs-lookup"><span data-stu-id="6ca91-142">When hello container stops, your data is erased.</span></span> <span data-ttu-id="6ca91-143">Termelési környezetben ne használja ezt a konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="6ca91-143">Do not use this configuration for production.</span></span>

<span data-ttu-id="6ca91-144">Keresse meg a toohello `fabrikamfiber.web:` csomópont, és adja hozzá a nevű gyermekcsomópontja `depends_on:`.</span><span class="sxs-lookup"><span data-stu-id="6ca91-144">Navigate toohello `fabrikamfiber.web:` node and add a child node named `depends_on:`.</span></span> <span data-ttu-id="6ca91-145">Ez biztosítja, hogy hello `db` (hello SQL Server tároló) szolgáltatás elindulása előtt a webalkalmazást (fabrikamfiber.web).</span><span class="sxs-lookup"><span data-stu-id="6ca91-145">This ensures that hello `db` service (hello SQL Server container) starts before our web application (fabrikamfiber.web).</span></span>

```yml
  fabrikamfiber.web:
    depends_on:
      - db
```

### <a name="update-hello-web-config"></a><span data-ttu-id="6ca91-146">Hello webkonfiguráció frissítése</span><span class="sxs-lookup"><span data-stu-id="6ca91-146">Update hello web config</span></span>

<span data-ttu-id="6ca91-147">Vissza a hello **FabrikamFiber.Web** projektre, frissítés hello kapcsolati karakterláncot a hello **web.config** fájlt, toopoint toohello hello tároló SQL-kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="6ca91-147">Back in hello **FabrikamFiber.Web** project, update hello connection string in hello **web.config** file, toopoint toohello SQL Server in hello container.</span></span>

```xml
<add name="FabrikamFiber-Express" connectionString="Data Source=db,1433;Database=FabrikamFiber;User Id=sa;Password=Password1;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />

<add name="FabrikamFiber-DataWarehouse" connectionString="Data Source=db,1433;Database=FabrikamFiber;User Id=sa;Password=Password1;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />
```

>[!NOTE]
><span data-ttu-id="6ca91-148">Ha azt szeretné, hogy a webalkalmazás létrehozása egy másik SQL Server kiadási fejlesztéskor toouse, adjon hozzá egy másik kapcsolati karakterlánc tooyour web.release.config fájlt.</span><span class="sxs-lookup"><span data-stu-id="6ca91-148">If you want toouse a different SQL Server when building a release build of your web application, add another connection string tooyour web.release.config file.</span></span>

### <a name="test-your-container"></a><span data-ttu-id="6ca91-149">A tároló tesztelése</span><span class="sxs-lookup"><span data-stu-id="6ca91-149">Test your container</span></span>

<span data-ttu-id="6ca91-150">Nyomja le az **F5** toorun és hibakeresési hello alkalmazás a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="6ca91-150">Press **F5** toorun and debug hello application in your container.</span></span>

<span data-ttu-id="6ca91-151">Peremhálózati az alkalmazás meghatározott indítási lap hello tároló hello IP-cím használata hello belső NAT-hálózaton (általában 172.x.x.x) megnyitása.</span><span class="sxs-lookup"><span data-stu-id="6ca91-151">Edge opens your application's defined launch page using hello IP address of hello container on hello internal NAT network (typically 172.x.x.x).</span></span> <span data-ttu-id="6ca91-152">További információ a Visual Studio 2017 használatával tárolókban lévő alkalmazások hibakeresése toolearn lásd: [Ez a cikk][link-debug-container].</span><span class="sxs-lookup"><span data-stu-id="6ca91-152">toolearn more about debugging applications in containers using Visual Studio 2017, see [this article][link-debug-container].</span></span>

![fabrikam tároló – példa][image-web-preview]

<span data-ttu-id="6ca91-154">hello tároló már készen áll a toobe épül, és a Service Fabric-alkalmazás csomagolva.</span><span class="sxs-lookup"><span data-stu-id="6ca91-154">hello container is now ready toobe built and packaged in a Service Fabric application.</span></span> <span data-ttu-id="6ca91-155">Miután hello tároló kép összeállítása a számítógépre, tooany tároló beállításjegyzék leküldeni, és lekérik a tooany állomás toorun.</span><span class="sxs-lookup"><span data-stu-id="6ca91-155">Once you have hello container image built on your machine, you can push it tooany container registry and pull it down tooany host toorun.</span></span>

## <a name="get-hello-application-ready-for-hello-cloud"></a><span data-ttu-id="6ca91-156">Felkészülés az alkalmazás hello hello felhő</span><span class="sxs-lookup"><span data-stu-id="6ca91-156">Get hello application ready for hello cloud</span></span>

<span data-ttu-id="6ca91-157">tooget hello alkalmazás készen áll az Azure Service Fabric futó, igazolnia kell a toocomplete két lépésből áll:</span><span class="sxs-lookup"><span data-stu-id="6ca91-157">tooget hello application ready for running in Service Fabric in Azure, we need toocomplete two steps:</span></span>

1. <span data-ttu-id="6ca91-158">Ha szeretnénk toobe képes tooreach hello Service Fabric-fürt a webes alkalmazás hello port tenni.</span><span class="sxs-lookup"><span data-stu-id="6ca91-158">Expose hello port where we want toobe able tooreach our web application in hello Service Fabric cluster.</span></span>
2. <span data-ttu-id="6ca91-159">Adja meg az alkalmazás üzemi készen SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="6ca91-159">Provide a production ready SQL database for our application.</span></span>

### <a name="expose-hello-port-for-hello-app"></a><span data-ttu-id="6ca91-160">Teszi közzé a hello alkalmazás hello port</span><span class="sxs-lookup"><span data-stu-id="6ca91-160">Expose hello port for hello app</span></span>
<span data-ttu-id="6ca91-161">hello Service Fabric-fürt rendelkezik konfigurált,-porttal rendelkezik *80* nyissa meg az alapértelmezés szerint a hello Azure Load Balancer, elosztja a bejövő forgalom toohello fürt.</span><span class="sxs-lookup"><span data-stu-id="6ca91-161">hello Service Fabric cluster we have configured, has port *80* open by default in hello Azure Load Balancer, that balances incoming traffic toohello cluster.</span></span> <span data-ttu-id="6ca91-162">Is elérhetővé kell tenni a tárolót a porton keresztül a docker-compose.yml fájlt.</span><span class="sxs-lookup"><span data-stu-id="6ca91-162">We can expose our container on this port via our docker-compose.yml file.</span></span>

<span data-ttu-id="6ca91-163">A Visual Studióban nyissa meg a **Megoldáskezelőben**, található **docker compose**, és nyissa meg hello fájl **docker-compose.override.yml**.</span><span class="sxs-lookup"><span data-stu-id="6ca91-163">In Visual Studio, open **Solution Explorer**, find **docker-compose**, and open hello file **docker-compose.override.yml**.</span></span>

<span data-ttu-id="6ca91-164">Módosítsa a hello `fabrikamfiber.web:` csomópont, nevű alárendelt csomópont hozzáadása `ports:`.</span><span class="sxs-lookup"><span data-stu-id="6ca91-164">Modify hello `fabrikamfiber.web:` node, add a child node named `ports:`.</span></span>

<span data-ttu-id="6ca91-165">Adjon hozzá egy karakterlánc-bejegyzést `- "80:80"`.</span><span class="sxs-lookup"><span data-stu-id="6ca91-165">Add a string entry `- "80:80"`.</span></span>

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

### <a name="use-a-production-sql-database"></a><span data-ttu-id="6ca91-166">SQL-adatbázis használata</span><span class="sxs-lookup"><span data-stu-id="6ca91-166">Use a production SQL database</span></span>
<span data-ttu-id="6ca91-167">Ha éles környezetben fut, igazolnia kell-e az adatok maradnak az adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="6ca91-167">When running in production, we need our data persisted in our database.</span></span> <span data-ttu-id="6ca91-168">Jelenleg nincs módja tooguarantee állandó adatokat tároló, ezért nem tárolhatók termelési adatok a tárolóban lévő SQL Server.</span><span class="sxs-lookup"><span data-stu-id="6ca91-168">There is currently no way tooguarantee persistent data in a container, therefore you cannot store production data in SQL Server in a container.</span></span>

<span data-ttu-id="6ca91-169">Azt javasoljuk, hogy egy Azure SQL Database használják.</span><span class="sxs-lookup"><span data-stu-id="6ca91-169">We recommend you utilize an Azure SQL Database.</span></span> <span data-ttu-id="6ca91-170">tooset fel, és futtassa az Azure-ban kezelt SQL Server látogasson el a hello [Azure SQL Database – Gyorsútmutatók] [ link-azure-sql] cikk.</span><span class="sxs-lookup"><span data-stu-id="6ca91-170">tooset up and run a managed SQL Server in Azure, visit hello [Azure SQL Database Quickstarts][link-azure-sql] article.</span></span>

>[!NOTE]
><span data-ttu-id="6ca91-171">Ne feledje toochange hello kapcsolati karakterláncok toohello SQL server hello **web.release.config** hello fájlban **FabrikamFiber.Web** projekt.</span><span class="sxs-lookup"><span data-stu-id="6ca91-171">Remember toochange hello connection strings toohello SQL server in hello **web.release.config** file in hello **FabrikamFiber.Web** project.</span></span>
>
><span data-ttu-id="6ca91-172">Ez az alkalmazás szabályosan meghiúsul, ha az SQL-adatbázis nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="6ca91-172">This application fails gracefully if no SQL database is reachable.</span></span> <span data-ttu-id="6ca91-173">Válassza ki azokat, amelyek toogo, és a nem SQL server hello alkalmazás központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="6ca91-173">You can choose toogo ahead and deploy hello application with no SQL server.</span></span>

## <a name="deploy-with-visual-studio-team-services"></a><span data-ttu-id="6ca91-174">A Visual Studio Team Services telepítéséhez</span><span class="sxs-lookup"><span data-stu-id="6ca91-174">Deploy with Visual Studio Team Services</span></span>

<span data-ttu-id="6ca91-175">tooset központi telepítése a Visual Studio Team Services használata szükséges, kell tooinstall hello [folyamatos kézbesítési eszközök bővítményt a Visual Studio 2017][link-visualstudio-cd-extension].</span><span class="sxs-lookup"><span data-stu-id="6ca91-175">tooset up deployment using Visual Studio Team Services, you need tooinstall hello [Continuous Delivery Tools extension for Visual Studio 2017][link-visualstudio-cd-extension].</span></span> <span data-ttu-id="6ca91-176">A bővítmény lehetővé teszi könnyen toodeploy tooAzure úgy konfigurálja a Visual Studio Team Services és a telepített alkalmazás tooyour Service Fabric-fürt beolvasása.</span><span class="sxs-lookup"><span data-stu-id="6ca91-176">This extension makes it easy toodeploy tooAzure by configuring Visual Studio Team Services and get your app deployed tooyour Service Fabric cluster.</span></span>

<span data-ttu-id="6ca91-177">tooget indult el, a kódot kell futnia a verziókövetési rendszerrel.</span><span class="sxs-lookup"><span data-stu-id="6ca91-177">tooget started, your code must be hosted in source control.</span></span> <span data-ttu-id="6ca91-178">Ez a szakasz többi hello azt feltételezi, hogy **git** használatban van.</span><span class="sxs-lookup"><span data-stu-id="6ca91-178">hello rest of this section assumes **git** is being used.</span></span>

### <a name="set-up-a-vsts-repo"></a><span data-ttu-id="6ca91-179">Egy VSTS-tárház beállítása</span><span class="sxs-lookup"><span data-stu-id="6ca91-179">Set up a VSTS repo</span></span>
<span data-ttu-id="6ca91-180">Visual Studio hello jobb alsó sarkában kattintson **tooSource vezérlő hozzáadása** > **Git** (vagy célszerű beállítás).</span><span class="sxs-lookup"><span data-stu-id="6ca91-180">At hello bottom-right corner of Visual Studio, click **Add tooSource Control** > **Git** (or whichever option you prefer).</span></span>

![hello forrás vezérlő gombra][image-source-control]

<span data-ttu-id="6ca91-182">A hello _Team Explorer_ ablaktáblában kattintson az **Git-tárház közzététele**.</span><span class="sxs-lookup"><span data-stu-id="6ca91-182">In hello _Team Explorer_ pane, press **Publish Git Repo**.</span></span>

<span data-ttu-id="6ca91-183">Válassza ki a VSTS-tárház nevét, és nyomja le az **tárház**.</span><span class="sxs-lookup"><span data-stu-id="6ca91-183">Select your VSTS repository name and press **Repository**.</span></span>

![tárház tooVSTS közzététele][image-publish-repo]

<span data-ttu-id="6ca91-185">Most, hogy a kód egy VSTS-forrás tárházat szinkronizálva van, beállíthatja a folyamatos integrációt és folyamatos kézbesítését.</span><span class="sxs-lookup"><span data-stu-id="6ca91-185">Now that your code is synchronized with a VSTS source repository, you can configure continuous integration and continuous delivery.</span></span>

### <a name="setup-continuous-delivery"></a><span data-ttu-id="6ca91-186">A telepítő folyamatos kézbesítés</span><span class="sxs-lookup"><span data-stu-id="6ca91-186">Setup continuous delivery</span></span>

<span data-ttu-id="6ca91-187">A _Megoldáskezelőben_, kattintson a jobb gombbal hello **megoldás** > **konfigurálása a folyamatos kézbesítési**.</span><span class="sxs-lookup"><span data-stu-id="6ca91-187">In _Solution Explorer_, right-click hello **solution** > **Configure Continuous Delivery**.</span></span>

<span data-ttu-id="6ca91-188">Válassza ki a hello Azure-előfizetést.</span><span class="sxs-lookup"><span data-stu-id="6ca91-188">Select hello Azure Subscription.</span></span>

<span data-ttu-id="6ca91-189">Állítsa be **állomás típusa** túl**Service Fabric-fürt**.</span><span class="sxs-lookup"><span data-stu-id="6ca91-189">Set **Host Type** too**Service Fabric Cluster**.</span></span>

<span data-ttu-id="6ca91-190">Állítsa be **célállomás** toohello service fabric-fürt hello előző szakaszban létrehozott.</span><span class="sxs-lookup"><span data-stu-id="6ca91-190">Set **Target Host** toohello service fabric cluster you created in hello previous section.</span></span>

<span data-ttu-id="6ca91-191">Válasszon egy **tároló beállításjegyzék** toopublish a tároló számára.</span><span class="sxs-lookup"><span data-stu-id="6ca91-191">Choose a **Container Registry** toopublish your container to.</span></span>

>[!TIP]
><span data-ttu-id="6ca91-192">Használjon hello **szerkesztése** gomb toocreate tároló beállításjegyzékbeli.</span><span class="sxs-lookup"><span data-stu-id="6ca91-192">Use hello **Edit** button toocreate a container registry.</span></span>

<span data-ttu-id="6ca91-193">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="6ca91-193">Press **OK**.</span></span>

![a telepítő service fabric folyamatos integrációt][image-setup-ci]
   
   <span data-ttu-id="6ca91-195">Ha hello konfigurációs befejeződött, a tároló telepített tooService háló.</span><span class="sxs-lookup"><span data-stu-id="6ca91-195">Once hello configuration is completed, your container is deployed tooService Fabric.</span></span> <span data-ttu-id="6ca91-196">Amikor a frissítések toohello tárház leküldéses végrehajtása egy új build és kiadása.</span><span class="sxs-lookup"><span data-stu-id="6ca91-196">Whenever you push updates toohello repository a new build and release is executed.</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="6ca91-197">Épület hello tároló képek körülbelül 15 percet igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="6ca91-197">Building hello container images take approximately 15 minutes.</span></span>
   ><span data-ttu-id="6ca91-198">hello első központi telepítési toohello Service Fabric-fürt hello alap Windows Server Core tároló képek toobe letöltött okoz.</span><span class="sxs-lookup"><span data-stu-id="6ca91-198">hello first deployment toohello Service Fabric cluster causes hello base Windows Server Core container images toobe downloaded.</span></span> <span data-ttu-id="6ca91-199">hello letöltési toocomplete további 5-10 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="6ca91-199">hello download takes additional 5-10 minutes toocomplete.</span></span>

<span data-ttu-id="6ca91-200">Keresse meg a fürt hello URL-cím segítségével toohello Fabrikam ügyfélszolgálatával alkalmazás: például *http://mycluster.westeurope.cloudapp.azure.com*</span><span class="sxs-lookup"><span data-stu-id="6ca91-200">Browse toohello Fabrikam Call Center application using hello url of your cluster: for example, *http://mycluster.westeurope.cloudapp.azure.com*</span></span>

<span data-ttu-id="6ca91-201">Most, hogy indexelése, és központilag telepített hello Fabrikam ügyfélszolgálatával megoldás, megnyithatja hello [Azure-portálon] [ link-azure-portal] és tekintse meg a Service Fabric hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6ca91-201">Now that you have containerized and deployed hello Fabrikam Call Center solution, you can open hello [Azure portal][link-azure-portal] and see hello application running in Service Fabric.</span></span> <span data-ttu-id="6ca91-202">tootry hello alkalmazás, nyisson meg egy webböngészőt, és nyissa meg a Service Fabric-fürt toohello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="6ca91-202">tootry hello application, open a web browser and go toohello URL of your Service Fabric cluster.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6ca91-203">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6ca91-203">Next steps</span></span>

<span data-ttu-id="6ca91-204">Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="6ca91-204">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6ca91-205">Hozzon létre egy Docker-projektet a Visual Studióban</span><span class="sxs-lookup"><span data-stu-id="6ca91-205">Create a Docker project in Visual Studio</span></span>
> * <span data-ttu-id="6ca91-206">Egy meglévő alkalmazást containerize</span><span class="sxs-lookup"><span data-stu-id="6ca91-206">Containerize an existing application</span></span>
> * <span data-ttu-id="6ca91-207">A telepítő folyamatos integráció a Visual Studio és a VSTS</span><span class="sxs-lookup"><span data-stu-id="6ca91-207">Setup continuous integration with Visual Studio and VSTS</span></span>

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
