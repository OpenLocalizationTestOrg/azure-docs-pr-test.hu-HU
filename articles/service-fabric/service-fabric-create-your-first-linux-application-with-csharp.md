---
title: "Az első Azure-alapú mikroszolgáltatás-alkalmazás létrehozása Linux rendszeren C# használatával | Microsoft Docs"
description: "Service Fabric-alkalmazás létrehozása és telepítése C# használatával"
services: service-fabric
documentationcenter: csharp
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 5a96d21d-fa4a-4dc2-abe8-a830a3482fb1
ms.service: service-fabric
ms.devlang: csharp
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/21/2017
ms.author: subramar
ms.openlocfilehash: adcafaa5522fcddc0a01eb1dc8deba04ebfc38f2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-your-first-azure-service-fabric-application"></a><span data-ttu-id="17f37-103">Az első Azure Service Fabric-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="17f37-103">Create your first Azure Service Fabric application</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="17f37-104">C# – Windows</span><span class="sxs-lookup"><span data-stu-id="17f37-104">C# - Windows</span></span>](service-fabric-create-your-first-application-in-visual-studio.md)
> * [<span data-ttu-id="17f37-105">Java – Linux</span><span class="sxs-lookup"><span data-stu-id="17f37-105">Java - Linux</span></span>](service-fabric-create-your-first-linux-application-with-java.md)
> * [<span data-ttu-id="17f37-106">C# – Linux</span><span class="sxs-lookup"><span data-stu-id="17f37-106">C# - Linux</span></span>](service-fabric-create-your-first-linux-application-with-csharp.md)
>
>

<span data-ttu-id="17f37-107">A Service Fabric SDK-kat biztosít Linux-szolgáltatások létrehozásához a .NET és a Java használatával egyaránt.</span><span class="sxs-lookup"><span data-stu-id="17f37-107">Service Fabric provides SDKs for building services on Linux in both .NET Core and Java.</span></span> <span data-ttu-id="17f37-108">A jelen oktatóanyagban áttekintjük, hogyan hozhat létre alkalmazásokat a Linux rendszerre, valamint szolgáltatásokat a C# (.NET Core) használatával.</span><span class="sxs-lookup"><span data-stu-id="17f37-108">In this tutorial, we look at how to create an application for Linux and build a service using C# (.NET Core).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="17f37-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="17f37-109">Prerequisites</span></span>
<span data-ttu-id="17f37-110">Mielőtt elkezdené, győződjön meg arról, hogy [beállította a Linux-fejlesztőkörnyezetet](service-fabric-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="17f37-110">Before you get started, make sure that you have [set up your Linux development environment](service-fabric-get-started-linux.md).</span></span> <span data-ttu-id="17f37-111">Amennyiben a Mac OS X rendszert használja, [beállíthat egy beépített Linux-környezetet egy virtuális gépen a Vagrant használatával](service-fabric-get-started-mac.md).</span><span class="sxs-lookup"><span data-stu-id="17f37-111">If you are using Mac OS X, you can [set up a Linux one-box environment in a virtual machine using Vagrant](service-fabric-get-started-mac.md).</span></span>

<span data-ttu-id="17f37-112">Telepítse a [Service Fabric parancssori felületét](service-fabric-cli.md) is</span><span class="sxs-lookup"><span data-stu-id="17f37-112">You will also want to install the [Service Fabric CLI](service-fabric-cli.md)</span></span>

### <a name="install-and-set-up-the-generators-for-csharp"></a><span data-ttu-id="17f37-113">A CSharp generátorainak telepítése és beállítása</span><span class="sxs-lookup"><span data-stu-id="17f37-113">Install and set up the generators for CSharp</span></span>
<span data-ttu-id="17f37-114">A Service Fabric olyan szerkezetkialakító eszközöket biztosít, amelyekkel Service Fabric CSharp-alkalmazásokat hozhat létre a terminálról a Yeoman sablongenerátor használatával.</span><span class="sxs-lookup"><span data-stu-id="17f37-114">Service Fabric provides scaffolding tools which will help you create a Service Fabric CSharp application from terminal using Yeoman template generator.</span></span> <span data-ttu-id="17f37-115">Az alábbi lépések végrehajtásával biztosíthatja, hogy a Service Fabric Yeoman sablongenerátor elérhető legyen a gépen lévő CSharp használatához.</span><span class="sxs-lookup"><span data-stu-id="17f37-115">Please follow the steps below to ensure you have the Service Fabric yeoman template generator for CSharp working on your machine.</span></span>
1. <span data-ttu-id="17f37-116">A node.js és az NPM telepítése a gépre</span><span class="sxs-lookup"><span data-stu-id="17f37-116">Install nodejs and NPM on your machine</span></span>

  ```bash
  sudo apt-get install npm
  sudo apt install nodejs-legacy
  ```
2. <span data-ttu-id="17f37-117">A [Yeoman](http://yeoman.io/) sablongenerátor telepítése a gépre az NPM-ből</span><span class="sxs-lookup"><span data-stu-id="17f37-117">Install [Yeoman](http://yeoman.io/) template generator on your machine from NPM</span></span>

  ```bash
  sudo npm install -g yo
  ```
3. <span data-ttu-id="17f37-118">A Service Fabric Yeo Java-alkalmazásgenerátor telepítése az NPM-ből</span><span class="sxs-lookup"><span data-stu-id="17f37-118">Install the Service Fabric Yeo Java application generator from NPM</span></span>

  ```bash
  sudo npm install -g generator-azuresfcsharp
  ```

## <a name="create-the-application"></a><span data-ttu-id="17f37-119">Az alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="17f37-119">Create the application</span></span>
<span data-ttu-id="17f37-120">A Service Fabric-alkalmazás egy vagy több szolgáltatást tartalmazhat, melyek mindegyike adott szerepkörrel rendelkezik az alkalmazás funkcióinak biztosításához.</span><span class="sxs-lookup"><span data-stu-id="17f37-120">A Service Fabric application can contain one or more services, each with a specific role in delivering the application's functionality.</span></span> <span data-ttu-id="17f37-121">A CSharphoz készült Service Fabric [Yeoman](http://yeoman.io/) generátor, amelyet az utolsó lépésben telepített, megkönnyíti az első szolgáltatás létrehozását, és a továbbiak hozzáadását a későbbiekben.</span><span class="sxs-lookup"><span data-stu-id="17f37-121">The Service Fabric [Yeoman](http://yeoman.io/) generator for CSharp, which you installed in last step, makes it easy to create your first service and to add more later.</span></span> <span data-ttu-id="17f37-122">Hozzunk létre egy egyetlen szolgáltatással rendelkező alkalmazást a Yeoman használatával.</span><span class="sxs-lookup"><span data-stu-id="17f37-122">Let's use Yeoman to create an application with a single service.</span></span>

1. <span data-ttu-id="17f37-123">Írja be a terminálba az alábbi parancsot a keret létrehozásához: `yo azuresfcsharp`</span><span class="sxs-lookup"><span data-stu-id="17f37-123">In a terminal, type the following command to start building the scaffolding: `yo azuresfcsharp`</span></span>
2. <span data-ttu-id="17f37-124">Adjon nevet az alkalmazásnak.</span><span class="sxs-lookup"><span data-stu-id="17f37-124">Name your application.</span></span>
3. <span data-ttu-id="17f37-125">Válassza ki az első szolgáltatása típusát, majd nevezze el.</span><span class="sxs-lookup"><span data-stu-id="17f37-125">Choose the type of your first service and name it.</span></span> <span data-ttu-id="17f37-126">A jelen oktatóanyag esetén egy Reliable aktorszolgáltatást választunk.</span><span class="sxs-lookup"><span data-stu-id="17f37-126">For the purposes of this tutorial, we choose a Reliable Actor Service.</span></span>

   ![C# nyelvhez készült Service Fabric Yeoman-generátor][sf-yeoman]

> [!NOTE]
> <span data-ttu-id="17f37-128">További tudnivalók a beállításokról: [Service Fabric programming model overview](service-fabric-choose-framework.md) (A Service Fabric programozási modell áttekintése).</span><span class="sxs-lookup"><span data-stu-id="17f37-128">For more information about the options, see [Service Fabric programming model overview](service-fabric-choose-framework.md).</span></span>
>
>

## <a name="build-the-application"></a><span data-ttu-id="17f37-129">Az alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="17f37-129">Build the application</span></span>
<span data-ttu-id="17f37-130">A Service Fabric Yeoman-sablonok tartalmaznak egy felépítési szkriptet, amelyet felhasználhat az alkalmazás terminálból történő létrehozásához (miután megnyitotta az alkalmazás mappáját).</span><span class="sxs-lookup"><span data-stu-id="17f37-130">The Service Fabric Yeoman templates include a build script that you can use to build the app from the terminal (after navigating to the application folder).</span></span>

  ```sh
 cd myapp
 ./build.sh
  ```

## <a name="deploy-the-application"></a><span data-ttu-id="17f37-131">Az alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="17f37-131">Deploy the application</span></span>

<span data-ttu-id="17f37-132">Az alkalmazást a létrehozása után telepítheti a helyi fürtben.</span><span class="sxs-lookup"><span data-stu-id="17f37-132">Once the application is built, you can deploy it to the local cluster.</span></span>

1. <span data-ttu-id="17f37-133">Csatlakozzon a helyi Service Fabric-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="17f37-133">Connect to the local Service Fabric cluster.</span></span>

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```

2. <span data-ttu-id="17f37-134">Futtassa a sablonban megadott telepítési szkriptet az alkalmazáscsomagnak a fürt lemezképtárolójába való másolásához, regisztrálja az alkalmazás típusát, és hozza létre az alkalmazás egy példányát.</span><span class="sxs-lookup"><span data-stu-id="17f37-134">Run the install script provided in the template to copy the application package to the cluster's image store, register the application type, and create an instance of the application.</span></span>

    ```bash
    ./install.sh
    ```

<span data-ttu-id="17f37-135">A kész alkalmazás a többi Service Fabric-alkalmazással azonos módon telepíthető.</span><span class="sxs-lookup"><span data-stu-id="17f37-135">Deploying the built application is the same as any other Service Fabric application.</span></span> <span data-ttu-id="17f37-136">Részletesebb útmutatást talál a [Service Fabric-alkalmazás kezelése a Service Fabric parancssori felülettel](service-fabric-application-lifecycle-sfctl.md) című dokumentációban.</span><span class="sxs-lookup"><span data-stu-id="17f37-136">See the documentation on [managing a Service Fabric application with the Service Fabric CLI](service-fabric-application-lifecycle-sfctl.md) for detailed instructions.</span></span>

<span data-ttu-id="17f37-137">Ezen parancsok paraméterezése megtalálható az alkalmazáscsomagon belül, a generált jegyzékfájlokban.</span><span class="sxs-lookup"><span data-stu-id="17f37-137">Parameters to these commands can be found in the generated manifests inside the application package.</span></span>

<span data-ttu-id="17f37-138">Az alkalmazás telepítése után nyisson meg egy böngészőt, és keresse fel a [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)-t a [http://localhost:19080/Explorer](http://localhost:19080/Explorer) URL-címen.</span><span class="sxs-lookup"><span data-stu-id="17f37-138">Once the application has been deployed, open a browser and navigate to [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) at [http://localhost:19080/Explorer](http://localhost:19080/Explorer).</span></span> <span data-ttu-id="17f37-139">Bontsa ki az **Alkalmazások** csomópontot, és figyelje meg, hogy most már megjelenik benne egy bejegyzés az alkalmazás típusához, és egy másik a típus első példányához.</span><span class="sxs-lookup"><span data-stu-id="17f37-139">Then, expand the **Applications** node and note that there is now an entry for your application type and another for the first instance of that type.</span></span>

## <a name="start-the-test-client-and-perform-a-failover"></a><span data-ttu-id="17f37-140">Tesztügyfél elindítása és feladatátvétel végrehajtása</span><span class="sxs-lookup"><span data-stu-id="17f37-140">Start the test client and perform a failover</span></span>
<span data-ttu-id="17f37-141">Az aktorprojektek önmagukban nem csinálnak semmit.</span><span class="sxs-lookup"><span data-stu-id="17f37-141">Actor projects do not do anything on their own.</span></span> <span data-ttu-id="17f37-142">Egy másik szolgáltatást vagy alkalmazást igényelnek, amely üzeneteket küld a számukra.</span><span class="sxs-lookup"><span data-stu-id="17f37-142">They require another service or client to send them messages.</span></span> <span data-ttu-id="17f37-143">Az aktorsablon egy egyszerű tesztszkriptet tartalmaz, amelyet az aktorszolgáltatással való kommunikációra használhat.</span><span class="sxs-lookup"><span data-stu-id="17f37-143">The actor template includes a simple test script that you can use to interact with the actor service.</span></span>

1. <span data-ttu-id="17f37-144">Futtassa a szkriptet a figyelési segédprogram használatával az aktorszolgáltatás kimenetének megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="17f37-144">Run the script using the watch utility to see the output of the actor service.</span></span>

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```
2. <span data-ttu-id="17f37-145">Keresse meg az aktorszolgáltatás elsődleges replikáját futtató csomópontot a Service Fabric Explorerben.</span><span class="sxs-lookup"><span data-stu-id="17f37-145">In Service Fabric Explorer, locate node hosting the primary replica for the actor service.</span></span> <span data-ttu-id="17f37-146">Az alábbi képernyőképen ez a 3. csomópont.</span><span class="sxs-lookup"><span data-stu-id="17f37-146">In the screenshot below, it is node 3.</span></span>

    ![Az elsődleges replika megkeresése a Service Fabric Explorerben][sfx-primary]
3. <span data-ttu-id="17f37-148">Kattintson az előző lépésben megtalált csomópontra, majd válassza a **Inaktiválás (újraindítás)** elemet a Műveletek menüből.</span><span class="sxs-lookup"><span data-stu-id="17f37-148">Click the node you found in the previous step, then select **Deactivate (restart)** from the Actions menu.</span></span> <span data-ttu-id="17f37-149">Ezzel a művelettel újraindítja a helyi fürt egy csomópontját, és feladatátvételt kényszerít ki egy másik csomóponton futó másodlagos replikára.</span><span class="sxs-lookup"><span data-stu-id="17f37-149">This action restarts one node in your local cluster forcing a failover to a secondary replica running on another node.</span></span> <span data-ttu-id="17f37-150">A művelet végrehajtása közben figyelje meg a tesztügyfél kimenetét, amelyből láthatja, hogy a számláló a feladatátvétel ellenére továbbra is növekszik.</span><span class="sxs-lookup"><span data-stu-id="17f37-150">As you perform this action, pay attention to the output from the test client and note that the counter continues to increment despite the failover.</span></span>

## <a name="adding-more-services-to-an-existing-application"></a><span data-ttu-id="17f37-151">További szolgáltatások hozzáadása meglévő alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="17f37-151">Adding more services to an existing application</span></span>

<span data-ttu-id="17f37-152">Ha egy másik szolgáltatást szeretne hozzáadni a `yo` használatával már létrehozott alkalmazáshoz, hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="17f37-152">To add another service to an application already created using `yo`, perform the following steps:</span></span>
1. <span data-ttu-id="17f37-153">Lépjen a meglevő alkalmazás gyökérkönyvtárába.</span><span class="sxs-lookup"><span data-stu-id="17f37-153">Change directory to the root of the existing application.</span></span>  <span data-ttu-id="17f37-154">Például `cd ~/YeomanSamples/MyApplication`, ha a `MyApplication` a Yeoman által létrehozott alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="17f37-154">For example, `cd ~/YeomanSamples/MyApplication`, if `MyApplication` is the application created by Yeoman.</span></span>
2. <span data-ttu-id="17f37-155">Futtassa a `yo azuresfcsharp:AddService` parancsot.</span><span class="sxs-lookup"><span data-stu-id="17f37-155">Run `yo azuresfcsharp:AddService`</span></span>

## <a name="migrating-from-projectjson-to-csproj"></a><span data-ttu-id="17f37-156">project.json migrálása .csproj-ra</span><span class="sxs-lookup"><span data-stu-id="17f37-156">Migrating from project.json to .csproj</span></span>
1. <span data-ttu-id="17f37-157">A 'dotnet migrate' parancs a projekt gyökérkönyvtárában futtatva a teljes project.json-t átmigrálja csproj formátumra.</span><span class="sxs-lookup"><span data-stu-id="17f37-157">Running 'dotnet migrate' in project root directory will migrate all the project.json to csproj format.</span></span>
2. <span data-ttu-id="17f37-158">A projektfájlokban ennek megfelelően frissíti a csproj-fájlokra mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="17f37-158">Update the project references accordingly to csproj files in project files.</span></span>
3. <span data-ttu-id="17f37-159">A projekt-fájlok neveit átírja csproj fájlokká a build.sh fájlban.</span><span class="sxs-lookup"><span data-stu-id="17f37-159">Update the project file names to csproj files in build.sh.</span></span>

## <a name="next-steps"></a><span data-ttu-id="17f37-160">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="17f37-160">Next steps</span></span>

* [<span data-ttu-id="17f37-161">További tudnivalók a Reliable Actorsről</span><span class="sxs-lookup"><span data-stu-id="17f37-161">Learn more about Reliable Actors</span></span>](service-fabric-reliable-actors-introduction.md)
* [<span data-ttu-id="17f37-162">Service Fabric-fürtök használata a Service Fabric parancssori felületén</span><span class="sxs-lookup"><span data-stu-id="17f37-162">Interacting with Service Fabric clusters using the Service Fabric CLI</span></span>](service-fabric-cli.md)
* <span data-ttu-id="17f37-163">A [Service Fabric támogatási lehetőségeinek](service-fabric-support.md) ismertetése</span><span class="sxs-lookup"><span data-stu-id="17f37-163">Learn about [Service Fabric support options](service-fabric-support.md)</span></span>
* [<span data-ttu-id="17f37-164">A Service Fabric parancssori felület használatának első lépései</span><span class="sxs-lookup"><span data-stu-id="17f37-164">Getting started with Service Fabric CLI</span></span>](service-fabric-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-csharp/yeoman-csharp.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-csharp/sfx-primary.png
