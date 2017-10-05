---
title: "Hogyan containerize az Azure Service Fabric mikroszolgáltatások (előzetes verzió)"
description: "Az Azure Service Fabric hozzá van adva a Service Fabric mikroszolgáltatások containerize érdekében új funkciókat. Ez a szolgáltatás jelenleg előzetes kiadásban elérhető."
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
ms.openlocfilehash: 6f8ad0bad8d1ae861e6b72f7e1a32ab0675813c2
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-containerize-your-service-fabric-reliable-services-and-reliable-actors-preview"></a><span data-ttu-id="0dcb5-104">Hogyan containerize a Service Fabric Reliable Services és Reliable Actors (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="0dcb5-104">How to containerize your Service Fabric Reliable Services and Reliable Actors (Preview)</span></span>

<span data-ttu-id="0dcb5-105">A Service Fabric containerizing Service Fabric mikroszolgáltatások (Reliable Services és megbízható alapú aktorszolgáltatások) támogatja.</span><span class="sxs-lookup"><span data-stu-id="0dcb5-105">Service Fabric supports containerizing Service Fabric microservices (Reliable Services, and Reliable Actor based services).</span></span> <span data-ttu-id="0dcb5-106">További információkért lásd: [service fabric-tárolók](service-fabric-containers-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0dcb5-106">For more information, see [service fabric containers](service-fabric-containers-overview.md).</span></span>


 <span data-ttu-id="0dcb5-107">Ez a funkció jelenleg előzetes verzióban érhető, és ez a cikk lépéseit a különböző egy tároló-keretrendszeren belül fut. a szolgáltatás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="0dcb5-107">This feature is in preview and this article provides the various steps to get your service running inside a container.</span></span>  

> [!NOTE]
> <span data-ttu-id="0dcb5-108">Ez a funkció jelenleg előzetes verzióban érhető és éles környezetben nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="0dcb5-108">This feature is in preview and is not supported in production.</span></span> <span data-ttu-id="0dcb5-109">Jelenleg ez a funkció csak akkor működik a Windows.</span><span class="sxs-lookup"><span data-stu-id="0dcb5-109">Currently this feature only works for Windows.</span></span>

## <a name="steps-to-containerize-your-service-fabric-application"></a><span data-ttu-id="0dcb5-110">A Service Fabric-alkalmazás containerize lépései</span><span class="sxs-lookup"><span data-stu-id="0dcb5-110">Steps to containerize your Service Fabric Application</span></span>

1. <span data-ttu-id="0dcb5-111">A Visual Studióban nyissa meg a Service Fabric-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="0dcb5-111">Open your Service Fabric application in Visual Studio.</span></span>

2. <span data-ttu-id="0dcb5-112">Osztály hozzáadása [SFBinaryLoader.cs](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/code/SFBinaryLoaderForContainers/SFBinaryLoader.cs) a projekthez.</span><span class="sxs-lookup"><span data-stu-id="0dcb5-112">Add class [SFBinaryLoader.cs](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/code/SFBinaryLoaderForContainers/SFBinaryLoader.cs) to your project.</span></span> <span data-ttu-id="0dcb5-113">Ez az osztály a kód a Service Fabric futásidejű bináris fájljai az alkalmazáson belüli megfelelően betölteni, ha egy tároló keretrendszeren belül fut. segítő.</span><span class="sxs-lookup"><span data-stu-id="0dcb5-113">The code in this class is a helper to correctly load the Service Fabric runtime binaries inside your application when running inside a container.</span></span>

3. <span data-ttu-id="0dcb5-114">Minden egyes szeretné containerize, a program belépési betöltő inicializálása kódcsomag mutat.</span><span class="sxs-lookup"><span data-stu-id="0dcb5-114">For each code package you would like to containerize, initialize the loader at the program entry point.</span></span> <span data-ttu-id="0dcb5-115">Adja hozzá a statikus konstruktor a következő kódrészletet a belépési pont programfájl látható.</span><span class="sxs-lookup"><span data-stu-id="0dcb5-115">Add the static constructor shown in the following code snippet to your program entry point file.</span></span>

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
          /// This is the entry point of the service host process.
          /// </summary>
          private static void Main()
          {
  ```

4. <span data-ttu-id="0dcb5-116">Build és [csomag](service-fabric-package-apps.md#Package-App) a projekthez.</span><span class="sxs-lookup"><span data-stu-id="0dcb5-116">Build and [package](service-fabric-package-apps.md#Package-App) your project.</span></span> <span data-ttu-id="0dcb5-117">Építsenek, és hozzon létre egy csomagot, kattintson a jobb gombbal a projektre a Megoldáskezelőben, és válassza ki a **csomag** parancsot.</span><span class="sxs-lookup"><span data-stu-id="0dcb5-117">To build and create a package, right-click the application project in Solution Explorer and choose the **Package** command.</span></span>

5. <span data-ttu-id="0dcb5-118">A minden kódcsomag kell containerize, a PowerShell-parancsprogrammal [CreateDockerPackage.ps1](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/scripts/CodePackageToDockerPackage/CreateDockerPackage.ps1).</span><span class="sxs-lookup"><span data-stu-id="0dcb5-118">For every code package you need to containerize, run the PowerShell script [CreateDockerPackage.ps1](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/scripts/CodePackageToDockerPackage/CreateDockerPackage.ps1).</span></span> <span data-ttu-id="0dcb5-119">A használati a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="0dcb5-119">The usage is as follows:</span></span>
  ```powershell
    $codePackagePath = 'Path to the code package to containerize.'
    $dockerPackageOutputDirectoryPath = 'Output path for the generated docker folder.'
    $applicationExeName = 'Name of the ode package executable.'
    CreateDockerPackage.ps1 -CodePackageDirectoryPath $codePackagePath -DockerPackageOutputDirectoryPath $dockerPackageOutputDirectoryPath -ApplicationExeName $applicationExeName
 ```
  <span data-ttu-id="0dcb5-120">A parancsfájl létrehoz egy mappát, $dockerPackageOutputDirectoryPath Docker-összetevők.</span><span class="sxs-lookup"><span data-stu-id="0dcb5-120">The script creates a folder with Docker artifacts at $dockerPackageOutputDirectoryPath.</span></span> <span data-ttu-id="0dcb5-121">A generált Dockerfile teszi közzé a portokat, futtassa a telepítő parancsfájlok stb. a igények alapján módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="0dcb5-121">Modify the generated Dockerfile to expose any ports, run setup scripts etc. based on your needs.</span></span>

6. <span data-ttu-id="0dcb5-122">Ezután meg kell [build](service-fabric-get-started-containers.md#Build-Containers) és [leküldéses](service-fabric-get-started-containers.md#Push-Containers) a Docker-tároló csomag a tárházhoz.</span><span class="sxs-lookup"><span data-stu-id="0dcb5-122">Next you need to [build](service-fabric-get-started-containers.md#Build-Containers) and [push](service-fabric-get-started-containers.md#Push-Containers) your Docker container package to your repository.</span></span>

7. <span data-ttu-id="0dcb5-123">A tároló kép, a tárház információkat, a beállításjegyzék-hitelesítés és a port-állomás leképezés hozzáadása ApplicationManifest.xml és ServiceManifest.xml módosíthatók.</span><span class="sxs-lookup"><span data-stu-id="0dcb5-123">Modify the ApplicationManifest.xml and ServiceManifest.xml to add your container image, repository information, registry authentication, and port-to-host mapping.</span></span> <span data-ttu-id="0dcb5-124">A jegyzékfájlokban módosítása, lásd: [hozzon létre egy Azure Service Fabric-tároló alkalmazás](service-fabric-get-started-containers.md).</span><span class="sxs-lookup"><span data-stu-id="0dcb5-124">For modifying the manifests, see [Create an Azure Service Fabric container application](service-fabric-get-started-containers.md).</span></span> <span data-ttu-id="0dcb5-125">A kód Csomagdefiníció a szolgáltatás jegyzékben meg kell írni a megfelelő tároló-lemezképet.</span><span class="sxs-lookup"><span data-stu-id="0dcb5-125">The code package definition in the service manifest needs to be replaced with corresponding container image.</span></span> <span data-ttu-id="0dcb5-126">Győződjön meg arról, hogy a belépési pont módosítása ContainerHost típusra.</span><span class="sxs-lookup"><span data-stu-id="0dcb5-126">Make sure to change the EntryPoint to a ContainerHost type.</span></span>

  ```xml
<!-- Code package is your service executable. -->
<CodePackage Name="Code" Version="1.0.0">
  <EntryPoint>
    <!-- Follow this link for more information about deploying Windows containers to Service Fabric: https://aka.ms/sfguestcontainers -->
    <ContainerHost>
      <ImageName>myregistry.azurecr.io/samples/helloworldapp</ImageName>
    </ContainerHost>
  </EntryPoint>
  <!-- Pass environment variables to your container: -->    
</CodePackage>
  ```

8. <span data-ttu-id="0dcb5-127">A port-állomás társítását a replikátor és a szolgáltatási végpont hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="0dcb5-127">Add the port-to-host mapping for your replicator and service endpoint.</span></span> <span data-ttu-id="0dcb5-128">Mindkét ezeket a portokat a Service Fabric által kiosztott futásidőben, mivel a ContainerPort értéke nulla esetén pedig a hozzárendelt portot használja a leképezés.</span><span class="sxs-lookup"><span data-stu-id="0dcb5-128">Since both these ports are assigned at runtime by Service Fabric, the ContainerPort is set to zero to use the assigned port for mapping.</span></span>

 ```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="0" EndpointRef="ServiceEndpoint"/>
    <PortBinding ContainerPort="0" EndpointRef="ReplicatorEndpoint"/>
  </ContainerHostPolicies>
</Policies>
 ```

9. <span data-ttu-id="0dcb5-129">Ez az alkalmazás teszteléséhez kell központilag telepítenie kell egy fürt, amely 5.7-es vagy újabb verziót futtat.</span><span class="sxs-lookup"><span data-stu-id="0dcb5-129">To test this application, you need to deploy it to a cluster that is running version 5.7 or higher.</span></span> <span data-ttu-id="0dcb5-130">Emellett meg kell módosítsa és frissítse a fürt beállításait a előzetes funkció engedélyezése érdekében.</span><span class="sxs-lookup"><span data-stu-id="0dcb5-130">In addition, you need to edit and update the cluster settings to enable this preview feature.</span></span> <span data-ttu-id="0dcb5-131">Ezen lépések [cikk](service-fabric-cluster-fabric-settings.md) a következő látható beállítás hozzáadásához.</span><span class="sxs-lookup"><span data-stu-id="0dcb5-131">Follow the steps in this [article](service-fabric-cluster-fabric-settings.md) to add the setting shown next.</span></span>
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
10. <span data-ttu-id="0dcb5-132">Következő [telepítése](service-fabric-deploy-remove-applications.md) a szerkesztett alkalmazáscsomag ehhez a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="0dcb5-132">Next [deploy](service-fabric-deploy-remove-applications.md) the edited application package to this cluster.</span></span>

<span data-ttu-id="0dcb5-133">Most rendelkeznie kell egy indexelése Service Fabric-alkalmazás fut a fürtön.</span><span class="sxs-lookup"><span data-stu-id="0dcb5-133">You should now have a containerized Service Fabric application running your cluster.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0dcb5-134">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0dcb5-134">Next steps</span></span>
* <span data-ttu-id="0dcb5-135">További információk a [tárolók futtatásáról a Service Fabricban](service-fabric-get-started-containers.md).</span><span class="sxs-lookup"><span data-stu-id="0dcb5-135">Learn more about running [containers on Service Fabric](service-fabric-get-started-containers.md).</span></span>
* <span data-ttu-id="0dcb5-136">További információk a Service Fabric [alkalmazásainak élettartamáról](service-fabric-application-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="0dcb5-136">Learn about the Service Fabric [application life-cycle](service-fabric-application-lifecycle.md).</span></span>
