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
# <a name="how-toocontainerize-your-service-fabric-reliable-services-and-reliable-actors-preview"></a>Hogyan toocontainerize a Service Fabric megbízható Services és Reliable Actors (előzetes verzió)

A Service Fabric containerizing Service Fabric mikroszolgáltatások (Reliable Services és megbízható alapú aktorszolgáltatások) támogatja. További információkért lásd: [service fabric-tárolók](service-fabric-containers-overview.md).


 Ez a funkció jelenleg előzetes verzióban érhető, és ez a cikk ismerteti a különböző lépéseket tooget hello egy tároló-keretrendszeren belül fut. a szolgáltatás.  

> [!NOTE]
> Ez a funkció jelenleg előzetes verzióban érhető és éles környezetben nem támogatott. Jelenleg ez a funkció csak akkor működik a Windows.

## <a name="steps-toocontainerize-your-service-fabric-application"></a>Lépések toocontainerize a Service Fabric-alkalmazás

1. A Visual Studióban nyissa meg a Service Fabric-alkalmazás.

2. Osztály hozzáadása [SFBinaryLoader.cs](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/code/SFBinaryLoaderForContainers/SFBinaryLoader.cs) tooyour projekt. Ez az osztály a hello kód egy segítő toocorrectly terhelés hello Service Fabric futásidejű bináris fájljai az alkalmazáson belüli esetén egy tároló-keretrendszeren belül fut..

3. Az egyes kód szeretné toocontainerize, inicializálási hello betöltő hello program belépési ponton. Adja hozzá a hello statikus konstruktor a következő kód részlet tooyour program belépési pont fájl hello látható.

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

4. Build és [csomag](service-fabric-package-apps.md#Package-App) a projekthez. toobuild hozzon létre egy csomagot, majd kattintson a jobb gombbal a projektre a Solution Explorer hello és válassza ki a hello **csomag** parancsot.

5. Minden kódcsomag toocontainerize, PowerShell-parancsfájl futtatása hello kell [CreateDockerPackage.ps1](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/scripts/CodePackageToDockerPackage/CreateDockerPackage.ps1). hello használata a következőképpen történik:
  ```powershell
    $codePackagePath = 'Path toohello code package toocontainerize.'
    $dockerPackageOutputDirectoryPath = 'Output path for hello generated docker folder.'
    $applicationExeName = 'Name of hello ode package executable.'
    CreateDockerPackage.ps1 -CodePackageDirectoryPath $codePackagePath -DockerPackageOutputDirectoryPath $dockerPackageOutputDirectoryPath -ApplicationExeName $applicationExeName
 ```
  hello parancsfájl mappát hoz létre, $dockerPackageOutputDirectoryPath Docker-összetevők. Módosítsa a generált hello Dockerfile tooexpose bármely portokat, futtassa a telepítő parancsfájlok stb. a igények alapján.

6. Ezután kell túl[build](service-fabric-get-started-containers.md#Build-Containers) és [leküldéses](service-fabric-get-started-containers.md#Push-Containers) a Docker-tároló csomag tooyour tárház.

7. Módosítása hello ApplicationManifest.xml és ServiceManifest.xml tooadd a tároló kép, tárház, beállításjegyzék-hitelesítés, és port-állomás leképezés. A hello jegyzékfájlokban módosítását, lásd: [hozzon létre egy Azure Service Fabric-tároló alkalmazás](service-fabric-get-started-containers.md). kód-Csomagdefiníció hello a hello service manifest igényeinek toobe írni a megfelelő tároló-lemezképet. Győződjön meg arról, hogy toochange hello BelépésiPont-tooa ContainerHost típusa.

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

8. Hello port-állomás leképezés a replikátor és a szolgáltatási végpont hozzáadása. Mindkét ezeket a portokat a Service Fabric által kiosztott futásidőben, mivel hello ContainerPort toozero toouse hello hozzárendelt leképezés port van beállítva.

 ```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="0" EndpointRef="ServiceEndpoint"/>
    <PortBinding ContainerPort="0" EndpointRef="ReplicatorEndpoint"/>
  </ContainerHostPolicies>
</Policies>
 ```

9. tootest az alkalmazás toodeploy kell azt tooa fürt, amely 5.7-es vagy újabb verziót futtat. Továbbá tooedit kell, és frissítse hello fürt beállítások tooenable ezt az előzetes funkciót. Ezen hello lépésekkel [cikk](service-fabric-cluster-fabric-settings.md) tooadd hello beállítás látható a következő.
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
10. Következő [telepítése](service-fabric-deploy-remove-applications.md) hello alkalmazás csomag toothis fürt szerkeszteni.

Most rendelkeznie kell egy indexelése Service Fabric-alkalmazás fut a fürtön.

## <a name="next-steps"></a>Következő lépések
* További információk a [tárolók futtatásáról a Service Fabricban](service-fabric-get-started-containers.md).
* További tudnivalók a Service Fabric hello [alkalmazás-életciklus](service-fabric-application-lifecycle.md).
