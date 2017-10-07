---
title: "aaaHow toocontainerize az Azure Service Fabric mikroszolgáltatások (előzetes verzió)"
description: "Azure Service Fabric új funkciók toocontainerize hozzáadta a Service Fabric mikroszolgáltatások létrehozására. Ez a szolgáltatás jelenleg előzetes kiadásban elérhető."
services: service-fabric
documentationcenter: .net
author: anmolah
manager: anmolah
editor: anmolah
ms.assetid: 0b41efb3-4063-4600-89f5-b077ea81fa3a
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/04/2017
ms.author: anmola
ms.openlocfilehash: 6edaff73c0828707c7fa736669ba8084663d31ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocontainerize-your-service-fabric-reliable-services-and-reliable-actors-preview"></a><span data-ttu-id="d1983-104">Hogyan toocontainerize a Service Fabric megbízható Services és Reliable Actors (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="d1983-104">How toocontainerize your Service Fabric Reliable Services and Reliable Actors (Preview)</span></span>

<span data-ttu-id="d1983-105">A Service Fabric containerizing Service Fabric mikroszolgáltatások (Reliable Services és megbízható alapú aktorszolgáltatások) támogatja.</span><span class="sxs-lookup"><span data-stu-id="d1983-105">Service Fabric supports containerizing Service Fabric microservices (Reliable Services, and Reliable Actor based services).</span></span> <span data-ttu-id="d1983-106">További információkért lásd: [service fabric-tárolók](service-fabric-containers-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d1983-106">For more information, see [service fabric containers](service-fabric-containers-overview.md).</span></span>


 <span data-ttu-id="d1983-107">Ez a funkció jelenleg előzetes verzióban érhető, és ez a cikk ismerteti a különböző lépéseket tooget hello egy tároló-keretrendszeren belül fut. a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="d1983-107">This feature is in preview and this article provides hello various steps tooget your service running inside a container.</span></span>  

> [!NOTE]
> <span data-ttu-id="d1983-108">Ez a funkció jelenleg előzetes verzióban érhető és éles környezetben nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="d1983-108">This feature is in preview and is not supported in production.</span></span> <span data-ttu-id="d1983-109">Jelenleg ez a funkció csak akkor működik a Windows.</span><span class="sxs-lookup"><span data-stu-id="d1983-109">Currently this feature only works for Windows.</span></span>

## <a name="steps-toocontainerize-your-service-fabric-application"></a><span data-ttu-id="d1983-110">Lépések toocontainerize a Service Fabric-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="d1983-110">Steps toocontainerize your Service Fabric Application</span></span>

1. <span data-ttu-id="d1983-111">A Visual Studióban nyissa meg a Service Fabric-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d1983-111">Open your Service Fabric application in Visual Studio.</span></span>

2. <span data-ttu-id="d1983-112">Osztály hozzáadása [SFBinaryLoader.cs](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/code/SFBinaryLoaderForContainers/SFBinaryLoader.cs) tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="d1983-112">Add class [SFBinaryLoader.cs](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/code/SFBinaryLoaderForContainers/SFBinaryLoader.cs) tooyour project.</span></span> <span data-ttu-id="d1983-113">Ez az osztály a hello kód egy segítő toocorrectly terhelés hello Service Fabric futásidejű bináris fájljai az alkalmazáson belüli esetén egy tároló-keretrendszeren belül fut..</span><span class="sxs-lookup"><span data-stu-id="d1983-113">hello code in this class is a helper toocorrectly load hello Service Fabric runtime binaries inside your application when running inside a container.</span></span>

3. <span data-ttu-id="d1983-114">Az egyes kód szeretné toocontainerize, inicializálási hello betöltő hello program belépési ponton.</span><span class="sxs-lookup"><span data-stu-id="d1983-114">For each code package you would like toocontainerize, initialize hello loader at hello program entry point.</span></span> <span data-ttu-id="d1983-115">Adja hozzá a hello statikus konstruktor a következő kód részlet tooyour program belépési pont fájl hello látható.</span><span class="sxs-lookup"><span data-stu-id="d1983-115">Add hello static constructor shown in hello following code snippet tooyour program entry point file.</span></span>

  ```csharp
  namespace MyApplication
  {
      internal static class Program
      {
          static Program()
          {
              SFBinaryLoader.Initialize();
          }

          /// <summary>
          /// This is hello entry point of hello service host process.
          /// </summary>
          private static void Main()
          {
  ```

4. <span data-ttu-id="d1983-116">Build és [csomag](service-fabric-package-apps.md#Package-App) a projekthez.</span><span class="sxs-lookup"><span data-stu-id="d1983-116">Build and [package](service-fabric-package-apps.md#Package-App) your project.</span></span> <span data-ttu-id="d1983-117">toobuild hozzon létre egy csomagot, majd kattintson a jobb gombbal a projektre a Solution Explorer hello és válassza ki a hello **csomag** parancsot.</span><span class="sxs-lookup"><span data-stu-id="d1983-117">toobuild and create a package, right-click hello application project in Solution Explorer and choose hello **Package** command.</span></span>

5. <span data-ttu-id="d1983-118">Minden kódcsomag toocontainerize, PowerShell-parancsfájl futtatása hello kell [CreateDockerPackage.ps1](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/scripts/CodePackageToDockerPackage/CreateDockerPackage.ps1).</span><span class="sxs-lookup"><span data-stu-id="d1983-118">For every code package you need toocontainerize, run hello PowerShell script [CreateDockerPackage.ps1](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/scripts/CodePackageToDockerPackage/CreateDockerPackage.ps1).</span></span> <span data-ttu-id="d1983-119">hello használata a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="d1983-119">hello usage is as follows:</span></span>
  ```powershell
    $codePackagePath = 'Path toohello code package toocontainerize.'
    $dockerPackageOutputDirectoryPath = 'Output path for hello generated docker folder.'
    $applicationExeName = 'Name of hello ode package executable.'
    CreateDockerPackage.ps1 -CodePackageDirectoryPath $codePackagePath -DockerPackageOutputDirectoryPath $dockerPackageOutputDirectoryPath -ApplicationExeName $applicationExeName
 ```
  <span data-ttu-id="d1983-120">hello parancsfájl mappát hoz létre, $dockerPackageOutputDirectoryPath Docker-összetevők.</span><span class="sxs-lookup"><span data-stu-id="d1983-120">hello script creates a folder with Docker artifacts at $dockerPackageOutputDirectoryPath.</span></span> <span data-ttu-id="d1983-121">Módosítsa a generált hello Dockerfile tooexpose bármely portokat, futtassa a telepítő parancsfájlok stb. a igények alapján.</span><span class="sxs-lookup"><span data-stu-id="d1983-121">Modify hello generated Dockerfile tooexpose any ports, run setup scripts etc. based on your needs.</span></span>

6. <span data-ttu-id="d1983-122">Ezután kell túl[build](service-fabric-get-started-containers.md#Build-Containers) és [leküldéses](service-fabric-get-started-containers.md#Push-Containers) a Docker-tároló csomag tooyour tárház.</span><span class="sxs-lookup"><span data-stu-id="d1983-122">Next you need too[build](service-fabric-get-started-containers.md#Build-Containers) and [push](service-fabric-get-started-containers.md#Push-Containers) your Docker container package tooyour repository.</span></span>

7. <span data-ttu-id="d1983-123">Módosítása hello ApplicationManifest.xml és ServiceManifest.xml tooadd a tároló kép, tárház, beállításjegyzék-hitelesítés, és port-állomás leképezés.</span><span class="sxs-lookup"><span data-stu-id="d1983-123">Modify hello ApplicationManifest.xml and ServiceManifest.xml tooadd your container image, repository information, registry authentication, and port-to-host mapping.</span></span> <span data-ttu-id="d1983-124">A hello jegyzékfájlokban módosítását, lásd: [hozzon létre egy Azure Service Fabric-tároló alkalmazás](service-fabric-get-started-containers.md).</span><span class="sxs-lookup"><span data-stu-id="d1983-124">For modifying hello manifests, see [Create an Azure Service Fabric container application](service-fabric-get-started-containers.md).</span></span> <span data-ttu-id="d1983-125">kód-Csomagdefiníció hello a hello service manifest igényeinek toobe írni a megfelelő tároló-lemezképet.</span><span class="sxs-lookup"><span data-stu-id="d1983-125">hello code package definition in hello service manifest needs toobe replaced with corresponding container image.</span></span> <span data-ttu-id="d1983-126">Győződjön meg arról, hogy toochange hello BelépésiPont-tooa ContainerHost típusa.</span><span class="sxs-lookup"><span data-stu-id="d1983-126">Make sure toochange hello EntryPoint tooa ContainerHost type.</span></span>

  ```xml
<!-- Code package is your service executable. -->
<CodePackage Name="Code" Version="1.0.0">
  <EntryPoint>
    <!-- Follow this link for more information about deploying Windows containers tooService Fabric: https://aka.ms/sfguestcontainers -->
    <ContainerHost>
      <ImageName>myregistry.azurecr.io/samples/helloworldapp</ImageName>
    </ContainerHost>
  </EntryPoint>
  <!-- Pass environment variables tooyour container: -->    
</CodePackage>
  ```

8. <span data-ttu-id="d1983-127">Hello port-állomás leképezés a replikátor és a szolgáltatási végpont hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="d1983-127">Add hello port-to-host mapping for your replicator and service endpoint.</span></span> <span data-ttu-id="d1983-128">Mindkét ezeket a portokat a Service Fabric által kiosztott futásidőben, mivel hello ContainerPort toozero toouse hello hozzárendelt leképezés port van beállítva.</span><span class="sxs-lookup"><span data-stu-id="d1983-128">Since both these ports are assigned at runtime by Service Fabric, hello ContainerPort is set toozero toouse hello assigned port for mapping.</span></span>

 ```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="0" EndpointRef="ServiceEndpoint"/>
    <PortBinding ContainerPort="0" EndpointRef="ReplicatorEndpoint"/>
  </ContainerHostPolicies>
</Policies>
 ```

9. <span data-ttu-id="d1983-129">tootest az alkalmazás toodeploy kell azt tooa fürt, amely 5.7-es vagy újabb verziót futtat.</span><span class="sxs-lookup"><span data-stu-id="d1983-129">tootest this application, you need toodeploy it tooa cluster that is running version 5.7 or higher.</span></span> <span data-ttu-id="d1983-130">Továbbá tooedit kell, és frissítse hello fürt beállítások tooenable ezt az előzetes funkciót.</span><span class="sxs-lookup"><span data-stu-id="d1983-130">In addition, you need tooedit and update hello cluster settings tooenable this preview feature.</span></span> <span data-ttu-id="d1983-131">Ezen hello lépésekkel [cikk](service-fabric-cluster-fabric-settings.md) tooadd hello beállítás látható a következő.</span><span class="sxs-lookup"><span data-stu-id="d1983-131">Follow hello steps in this [article](service-fabric-cluster-fabric-settings.md) tooadd hello setting shown next.</span></span>
```
      {
        "name": "Hosting",
        "parameters": [
          {
            "name": "FabricContainerAppsEnabled",
            "value": "true"
          }
        ]
      }
```
10. <span data-ttu-id="d1983-132">Következő [telepítése](service-fabric-deploy-remove-applications.md) hello alkalmazás csomag toothis fürt szerkeszteni.</span><span class="sxs-lookup"><span data-stu-id="d1983-132">Next [deploy](service-fabric-deploy-remove-applications.md) hello edited application package toothis cluster.</span></span>

<span data-ttu-id="d1983-133">Most rendelkeznie kell egy indexelése Service Fabric-alkalmazás fut a fürtön.</span><span class="sxs-lookup"><span data-stu-id="d1983-133">You should now have a containerized Service Fabric application running your cluster.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d1983-134">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d1983-134">Next steps</span></span>
* <span data-ttu-id="d1983-135">További információk a [tárolók futtatásáról a Service Fabricban](service-fabric-get-started-containers.md).</span><span class="sxs-lookup"><span data-stu-id="d1983-135">Learn more about running [containers on Service Fabric](service-fabric-get-started-containers.md).</span></span>
* <span data-ttu-id="d1983-136">További tudnivalók a Service Fabric hello [alkalmazás-életciklus](service-fabric-application-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="d1983-136">Learn about hello Service Fabric [application life-cycle](service-fabric-application-lifecycle.md).</span></span>
