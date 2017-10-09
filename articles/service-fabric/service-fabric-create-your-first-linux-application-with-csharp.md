---
title: "aaaCreate az első Azure mikroszolgáltatások alkalmazás Linux C# használatával |} Microsoft Docs"
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
ms.openlocfilehash: 68d685e130be338ebcdb2f1af24b66d1e14f580a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-azure-service-fabric-application"></a><span data-ttu-id="04c82-103">Az első Azure Service Fabric-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="04c82-103">Create your first Azure Service Fabric application</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="04c82-104">C# – Windows</span><span class="sxs-lookup"><span data-stu-id="04c82-104">C# - Windows</span></span>](service-fabric-create-your-first-application-in-visual-studio.md)
> * [<span data-ttu-id="04c82-105">Java – Linux</span><span class="sxs-lookup"><span data-stu-id="04c82-105">Java - Linux</span></span>](service-fabric-create-your-first-linux-application-with-java.md)
> * [<span data-ttu-id="04c82-106">C# – Linux</span><span class="sxs-lookup"><span data-stu-id="04c82-106">C# - Linux</span></span>](service-fabric-create-your-first-linux-application-with-csharp.md)
>
>

<span data-ttu-id="04c82-107">A Service Fabric SDK-kat biztosít Linux-szolgáltatások létrehozásához a .NET és a Java használatával egyaránt.</span><span class="sxs-lookup"><span data-stu-id="04c82-107">Service Fabric provides SDKs for building services on Linux in both .NET Core and Java.</span></span> <span data-ttu-id="04c82-108">Ebben az oktatóanyagban úgy tekintünk, hogyan toocreate a Linux és a build C# (.NET Core) szolgáltatás egy alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="04c82-108">In this tutorial, we look at how toocreate an application for Linux and build a service using C# (.NET Core).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="04c82-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="04c82-109">Prerequisites</span></span>
<span data-ttu-id="04c82-110">Mielőtt elkezdené, győződjön meg arról, hogy [beállította a Linux-fejlesztőkörnyezetet](service-fabric-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="04c82-110">Before you get started, make sure that you have [set up your Linux development environment](service-fabric-get-started-linux.md).</span></span> <span data-ttu-id="04c82-111">Amennyiben a Mac OS X rendszert használja, [beállíthat egy beépített Linux-környezetet egy virtuális gépen a Vagrant használatával](service-fabric-get-started-mac.md).</span><span class="sxs-lookup"><span data-stu-id="04c82-111">If you are using Mac OS X, you can [set up a Linux one-box environment in a virtual machine using Vagrant](service-fabric-get-started-mac.md).</span></span>

<span data-ttu-id="04c82-112">Érdemes is tooinstall hello [Service Fabric parancssori felület](service-fabric-cli.md)</span><span class="sxs-lookup"><span data-stu-id="04c82-112">You will also want tooinstall hello [Service Fabric CLI](service-fabric-cli.md)</span></span>

### <a name="install-and-set-up-hello-generators-for-csharp"></a><span data-ttu-id="04c82-113">Telepítse és állítsa be hello generátorokat CSharp</span><span class="sxs-lookup"><span data-stu-id="04c82-113">Install and set up hello generators for CSharp</span></span>
<span data-ttu-id="04c82-114">A Service Fabric olyan szerkezetkialakító eszközöket biztosít, amelyekkel Service Fabric CSharp-alkalmazásokat hozhat létre a terminálról a Yeoman sablongenerátor használatával.</span><span class="sxs-lookup"><span data-stu-id="04c82-114">Service Fabric provides scaffolding tools which will help you create a Service Fabric CSharp application from terminal using Yeoman template generator.</span></span> <span data-ttu-id="04c82-115">Kövesse hello lépéseket alatti tooensure hello Service Fabric yeoman sablon generátor rendelkezik a csharp nyelvű működik-e a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="04c82-115">Please follow hello steps below tooensure you have hello Service Fabric yeoman template generator for CSharp working on your machine.</span></span>
1. <span data-ttu-id="04c82-116">A node.js és az NPM telepítése a gépre</span><span class="sxs-lookup"><span data-stu-id="04c82-116">Install nodejs and NPM on your machine</span></span>

  ```bash
  sudo apt-get install npm
  sudo apt install nodejs-legacy
  ```
2. <span data-ttu-id="04c82-117">A [Yeoman](http://yeoman.io/) sablongenerátor telepítése a gépre az NPM-ből</span><span class="sxs-lookup"><span data-stu-id="04c82-117">Install [Yeoman](http://yeoman.io/) template generator on your machine from NPM</span></span>

  ```bash
  sudo npm install -g yo
  ```
3. <span data-ttu-id="04c82-118">Telepítse a Service Fabric Yeo Java-alkalmazás generátor hello NPM a</span><span class="sxs-lookup"><span data-stu-id="04c82-118">Install hello Service Fabric Yeo Java application generator from NPM</span></span>

  ```bash
  sudo npm install -g generator-azuresfcsharp
  ```

## <a name="create-hello-application"></a><span data-ttu-id="04c82-119">Hello alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="04c82-119">Create hello application</span></span>
<span data-ttu-id="04c82-120">A Service Fabric-alkalmazás tartalmazhat egy vagy több szolgáltatást, az egy adott szerepkör a postai hello alkalmazás működését.</span><span class="sxs-lookup"><span data-stu-id="04c82-120">A Service Fabric application can contain one or more services, each with a specific role in delivering hello application's functionality.</span></span> <span data-ttu-id="04c82-121">a Service Fabric hello [Yeoman](http://yeoman.io/) az előző lépésben telepítette, c Sharp generátor teszi, hogy könnyen toocreate az első szolgáltatás- és tooadd később.</span><span class="sxs-lookup"><span data-stu-id="04c82-121">hello Service Fabric [Yeoman](http://yeoman.io/) generator for CSharp, which you installed in last step, makes it easy toocreate your first service and tooadd more later.</span></span> <span data-ttu-id="04c82-122">Most használja Yeoman toocreate egy alkalmazás egy szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="04c82-122">Let's use Yeoman toocreate an application with a single service.</span></span>

1. <span data-ttu-id="04c82-123">Egy terminált írja be a következő parancs toostart hello állványok felépítése hello:`yo azuresfcsharp`</span><span class="sxs-lookup"><span data-stu-id="04c82-123">In a terminal, type hello following command toostart building hello scaffolding: `yo azuresfcsharp`</span></span>
2. <span data-ttu-id="04c82-124">Adjon nevet az alkalmazásnak.</span><span class="sxs-lookup"><span data-stu-id="04c82-124">Name your application.</span></span>
3. <span data-ttu-id="04c82-125">Válassza ki az első szolgáltatás hello típusú, és adjon neki nevet.</span><span class="sxs-lookup"><span data-stu-id="04c82-125">Choose hello type of your first service and name it.</span></span> <span data-ttu-id="04c82-126">Ez az oktatóanyag hello céljából azt megbízható szereplő szolgáltatás kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="04c82-126">For hello purposes of this tutorial, we choose a Reliable Actor Service.</span></span>

   ![C# nyelvhez készült Service Fabric Yeoman-generátor][sf-yeoman]

> [!NOTE]
> <span data-ttu-id="04c82-128">Hello beállításokkal kapcsolatos további információkért lásd: [programozási modell áttekintése a Service Fabric](service-fabric-choose-framework.md).</span><span class="sxs-lookup"><span data-stu-id="04c82-128">For more information about hello options, see [Service Fabric programming model overview](service-fabric-choose-framework.md).</span></span>
>
>

## <a name="build-hello-application"></a><span data-ttu-id="04c82-129">Hello alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="04c82-129">Build hello application</span></span>
<span data-ttu-id="04c82-130">hello Service Fabric Yeoman-sablonjai tartalmazzák a build parancsfájl használható toobuild hello alkalmazást a Terminálszolgáltatások hello (után Navigálás toohello alkalmazásmappa).</span><span class="sxs-lookup"><span data-stu-id="04c82-130">hello Service Fabric Yeoman templates include a build script that you can use toobuild hello app from hello terminal (after navigating toohello application folder).</span></span>

  ```sh
 cd myapp
 ./build.sh
  ```

## <a name="deploy-hello-application"></a><span data-ttu-id="04c82-131">Hello alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="04c82-131">Deploy hello application</span></span>

<span data-ttu-id="04c82-132">Miután hello alkalmazás épül, ezután telepítheti azt toohello helyi fürthöz.</span><span class="sxs-lookup"><span data-stu-id="04c82-132">Once hello application is built, you can deploy it toohello local cluster.</span></span>

1. <span data-ttu-id="04c82-133">Csatlakozás helyi Service Fabric-fürt toohello.</span><span class="sxs-lookup"><span data-stu-id="04c82-133">Connect toohello local Service Fabric cluster.</span></span>

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```

2. <span data-ttu-id="04c82-134">Hello sablon toocopy megadott futtatási hello telepítési parancsfájl alkalmazás csomag toohello fürt lemezképtárolóhoz hello hello alkalmazástípus regisztrálása és hello alkalmazás egy példányának létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="04c82-134">Run hello install script provided in hello template toocopy hello application package toohello cluster's image store, register hello application type, and create an instance of hello application.</span></span>

    ```bash
    ./install.sh
    ```

<span data-ttu-id="04c82-135">Központi telepítés beépített hello alkalmazás hello ugyanaz, mint bármely más Service Fabric-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="04c82-135">Deploying hello built application is hello same as any other Service Fabric application.</span></span> <span data-ttu-id="04c82-136">Hello dokumentációjában talál [kezelése a Service Fabric-alkalmazás a Service Fabric CLI hello](service-fabric-application-lifecycle-sfctl.md) részletes információkra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="04c82-136">See hello documentation on [managing a Service Fabric application with hello Service Fabric CLI](service-fabric-application-lifecycle-sfctl.md) for detailed instructions.</span></span>

<span data-ttu-id="04c82-137">Paraméterek toothese parancsok generált hello jegyzékfájlokban hello alkalmazáscsomag belül található.</span><span class="sxs-lookup"><span data-stu-id="04c82-137">Parameters toothese commands can be found in hello generated manifests inside hello application package.</span></span>

<span data-ttu-id="04c82-138">Miután hello alkalmazás telepítve van, nyisson meg egy böngészőt, és navigáljon a [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) : [19080/Explorer](http://localhost:19080/Explorer).</span><span class="sxs-lookup"><span data-stu-id="04c82-138">Once hello application has been deployed, open a browser and navigate to [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) at [http://localhost:19080/Explorer](http://localhost:19080/Explorer).</span></span> <span data-ttu-id="04c82-139">Ezt követően bontsa ki a hello **alkalmazások** csomópont, és vegye figyelembe, hogy már létezik egy bejegyzést, az alkalmazás típusának és egy másikat a hello adott típus első példányát.</span><span class="sxs-lookup"><span data-stu-id="04c82-139">Then, expand hello **Applications** node and note that there is now an entry for your application type and another for hello first instance of that type.</span></span>

## <a name="start-hello-test-client-and-perform-a-failover"></a><span data-ttu-id="04c82-140">Hello teszt ügyfél elindítása, és végezzen el egy feladatátvételt</span><span class="sxs-lookup"><span data-stu-id="04c82-140">Start hello test client and perform a failover</span></span>
<span data-ttu-id="04c82-141">Az aktorprojektek önmagukban nem csinálnak semmit.</span><span class="sxs-lookup"><span data-stu-id="04c82-141">Actor projects do not do anything on their own.</span></span> <span data-ttu-id="04c82-142">Szükségük van egy másik szolgáltatás vagy ügyfél toosend őket üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="04c82-142">They require another service or client toosend them messages.</span></span> <span data-ttu-id="04c82-143">hello szereplő sablon tartalmaz egy egyszerű parancsprogram használható toointeract hello szereplő szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="04c82-143">hello actor template includes a simple test script that you can use toointeract with hello actor service.</span></span>

1. <span data-ttu-id="04c82-144">Hello parancsfájl segítségével történő hello figyelési segédprogram toosee hello kimeneti hello szereplő szolgáltatás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="04c82-144">Run hello script using hello watch utility toosee hello output of hello actor service.</span></span>

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```
2. <span data-ttu-id="04c82-145">A Service Fabric Explorerben keresse meg a csomópont hello elsődleges replika hello szereplő szolgáltatás üzemeltetéséhez.</span><span class="sxs-lookup"><span data-stu-id="04c82-145">In Service Fabric Explorer, locate node hosting hello primary replica for hello actor service.</span></span> <span data-ttu-id="04c82-146">Hello alábbi képernyőképen látható hogy a rendszer csomópont 3.</span><span class="sxs-lookup"><span data-stu-id="04c82-146">In hello screenshot below, it is node 3.</span></span>

    ![A Service Fabric Explorerben hello elsődleges replika keresése][sfx-primary]
3. <span data-ttu-id="04c82-148">Kattintson az előző lépésben hello található, majd válassza ki hello csomópontra **inaktiválás (újraindítás)** hello műveletek menüjében.</span><span class="sxs-lookup"><span data-stu-id="04c82-148">Click hello node you found in hello previous step, then select **Deactivate (restart)** from hello Actions menu.</span></span> <span data-ttu-id="04c82-149">Ez a művelet egy csomópont újraindul a helyi fürt kényszerített feladatátvételi tooa másodlagos replika fut egy másik csomópontjára.</span><span class="sxs-lookup"><span data-stu-id="04c82-149">This action restarts one node in your local cluster forcing a failover tooa secondary replica running on another node.</span></span> <span data-ttu-id="04c82-150">Ezt a műveletet hajt végre, nagy figyelmet toohello kimeneti hello teszt ügyfél, és vegye figyelembe, hogy hello számláló továbbra is fennáll, annak ellenére, hogy hello feladatátvételi tooincrement.</span><span class="sxs-lookup"><span data-stu-id="04c82-150">As you perform this action, pay attention toohello output from hello test client and note that hello counter continues tooincrement despite hello failover.</span></span>

## <a name="adding-more-services-tooan-existing-application"></a><span data-ttu-id="04c82-151">További szolgáltatások tooan meglévő alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="04c82-151">Adding more services tooan existing application</span></span>

<span data-ttu-id="04c82-152">tooadd egy másik tooan szolgáltatásalkalmazás már létrehozott `yo`, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="04c82-152">tooadd another service tooan application already created using `yo`, perform hello following steps:</span></span>
1. <span data-ttu-id="04c82-153">Hello meglévő alkalmazás gyökérkönyvtárának toohello módosítása.</span><span class="sxs-lookup"><span data-stu-id="04c82-153">Change directory toohello root of hello existing application.</span></span>  <span data-ttu-id="04c82-154">Például `cd ~/YeomanSamples/MyApplication`, ha `MyApplication` Yeoman által létrehozott hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="04c82-154">For example, `cd ~/YeomanSamples/MyApplication`, if `MyApplication` is hello application created by Yeoman.</span></span>
2. <span data-ttu-id="04c82-155">Futtassa a `yo azuresfcsharp:AddService` parancsot.</span><span class="sxs-lookup"><span data-stu-id="04c82-155">Run `yo azuresfcsharp:AddService`</span></span>

## <a name="migrating-from-projectjson-toocsproj"></a><span data-ttu-id="04c82-156">Project.json too.csproj áttelepítése</span><span class="sxs-lookup"><span data-stu-id="04c82-156">Migrating from project.json too.csproj</span></span>
1. <span data-ttu-id="04c82-157">"Dotnet át" a projekt gyökérkönyvtárában futó összes hello project.json toocsproj formátumban kell áttelepítenie.</span><span class="sxs-lookup"><span data-stu-id="04c82-157">Running 'dotnet migrate' in project root directory will migrate all hello project.json toocsproj format.</span></span>
2. <span data-ttu-id="04c82-158">Frissítés hello projekt ennek megfelelően toocsproj fájlok projektfájlokban hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="04c82-158">Update hello project references accordingly toocsproj files in project files.</span></span>
3. <span data-ttu-id="04c82-159">Hello projekt fájl nevének toocsproj fájljainak build.sh frissítésére.</span><span class="sxs-lookup"><span data-stu-id="04c82-159">Update hello project file names toocsproj files in build.sh.</span></span>

## <a name="next-steps"></a><span data-ttu-id="04c82-160">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="04c82-160">Next steps</span></span>

* [<span data-ttu-id="04c82-161">További tudnivalók a Reliable Actorsről</span><span class="sxs-lookup"><span data-stu-id="04c82-161">Learn more about Reliable Actors</span></span>](service-fabric-reliable-actors-introduction.md)
* [<span data-ttu-id="04c82-162">Service Fabric-fürtök hello Service Fabric parancssori felület használatával való interakció</span><span class="sxs-lookup"><span data-stu-id="04c82-162">Interacting with Service Fabric clusters using hello Service Fabric CLI</span></span>](service-fabric-cli.md)
* <span data-ttu-id="04c82-163">A [Service Fabric támogatási lehetőségeinek](service-fabric-support.md) ismertetése</span><span class="sxs-lookup"><span data-stu-id="04c82-163">Learn about [Service Fabric support options](service-fabric-support.md)</span></span>
* [<span data-ttu-id="04c82-164">A Service Fabric parancssori felület használatának első lépései</span><span class="sxs-lookup"><span data-stu-id="04c82-164">Getting started with Service Fabric CLI</span></span>](service-fabric-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-csharp/yeoman-csharp.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-csharp/sfx-primary.png
