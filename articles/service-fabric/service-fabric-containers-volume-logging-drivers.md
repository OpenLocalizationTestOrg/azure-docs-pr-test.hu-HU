---
title: Service Fabric Docker Compose Preview aaaAzure |} Microsoft Docs
description: "Az Azure Service Fabric Docker Compose formátum toomake fogad el azt a Service Fabric használatával könnyebben tooorchestrate exsiting tárolók. Ez a támogatás jelenleg előzetes verzió."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 824044fd698f0ed94c4212722bc82187905315dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="specifying-volume-plugins-and-logging-drivers-for-your-container"></a><span data-ttu-id="6083c-104">Adja meg a kötet beépülő modulok és illesztőprogramok a tároló-naplózás</span><span class="sxs-lookup"><span data-stu-id="6083c-104">Specifying volume plugins and logging drivers for your container</span></span>

<span data-ttu-id="6083c-105">A Service Fabric támogatja megadó [Docker kötet beépülő modulok](https://docs.docker.com/engine/extend/plugins_volume/) és [Docker naplózási illesztőprogramok](https://docs.docker.com/engine/admin/logging/overview/) a tárolószolgáltatás számára.</span><span class="sxs-lookup"><span data-stu-id="6083c-105">Service Fabric supports specifying [Docker volume plugins](https://docs.docker.com/engine/extend/plugins_volume/) and [Docker logging drivers](https://docs.docker.com/engine/admin/logging/overview/) for your container service.</span></span> <span data-ttu-id="6083c-106">hello beépülő modulok hello alkalmazásjegyzékben vannak megadva, ahogy az a következő jegyzékfájl hello:</span><span class="sxs-lookup"><span data-stu-id="6083c-106">hello plugins are specified in hello application manifest as shown in hello following manifest:</span></span>


```xml
?xml version="1.0" encoding="UTF-8"?>
<ApplicationManifest ApplicationTypeName="WinNodeJsApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <Description>Calculator Application</Description>
    <Parameters>
        <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
      <Parameter Name="MyCpuShares" DefaultValue="3"></Parameter>
    </Parameters>
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeServicePackage" ServiceManifestVersion="1.0"/>
     <Policies>
       <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="hyperv"> 
        <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
        <RepositoryCredentials PasswordEncrypted="false" Password="****" AccountName="test"/>
        <LogConfig Driver="etwlogs" >
          <DriverOption Name="test" Value="vale"/>
        </LogConfig>
        <Volume Source="c:\workspace" Destination="c:\testmountlocation1" IsReadOnly="false"></Volume>
        <Volume Source="d:\myfolder" Destination="c:\testmountlocation2" IsReadOnly="true"> </Volume>
        <Volume Source="myexternalvolume" Destination="c:\testmountlocation3" Driver="sf" IsReadOnly="true"></Volume>
       </ContainerHostPolicies>
   </Policies>
    </ServiceManifestImport>
    <ServiceTemplates>
        <StatelessService ServiceTypeName="StatelessNodeService" InstanceCount="5">
            <SingletonPartition></SingletonPartition>
        </StatelessService>
    </ServiceTemplates>
</ApplicationManifest>
```

<span data-ttu-id="6083c-107">A fenti példa hello, hello `Source` hello címkéjének `Volume` toohello forrásmappa hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="6083c-107">In hello preceding example, hello `Source` tag for hello `Volume` refers toohello source folder.</span></span> <span data-ttu-id="6083c-108">hello forrásmappa lehet hello hello tárolók vagy egy állandó távoli tároló üzemeltető virtuális gép egyik mappájába.</span><span class="sxs-lookup"><span data-stu-id="6083c-108">hello source folder could be a folder in hello VM that hosts hello containers or a persistent remote store.</span></span> <span data-ttu-id="6083c-109">Hello `Destination` címke hello hello helyre `Source` csatlakoztatott toowithin hello fut tároló.</span><span class="sxs-lookup"><span data-stu-id="6083c-109">hello `Destination` tag is hello location that hello `Source` is mapped toowithin hello running container.</span></span> 

<span data-ttu-id="6083c-110">Egy kötet beépülő modul megadásakor a Service Fabric automatikusan létrehozza a hello kötet hello paraméterek vannak megadva.</span><span class="sxs-lookup"><span data-stu-id="6083c-110">When specifying a volume plugin, Service Fabric automatically creates hello volume using hello parameters specified.</span></span> <span data-ttu-id="6083c-111">Hello `Source` címke hello nevét, valamint hello kötet hello `Driver` kódon hello kötet illesztőprogram beépülő modul.</span><span class="sxs-lookup"><span data-stu-id="6083c-111">hello `Source` tag is hello name of hello volume, and hello `Driver` tag specifies hello volume driver plugin.</span></span> <span data-ttu-id="6083c-112">Beállítások adhatók meg hello `DriverOption` címkét, ahogy az alábbi részlet hello:</span><span class="sxs-lookup"><span data-stu-id="6083c-112">Options can be specified using hello `DriverOption` tag as shown in hello following snippet:</span></span>

```xml
<Volume Source="myvolume1" Destination="c:\testmountlocation4" Driver="azurefile" IsReadOnly="true">
           <DriverOption Name="share" Value="models"/>
</Volume>
```

<span data-ttu-id="6083c-113">A Docker napló illesztőprogram meg van adva, esetén szükséges toodeploy ügynökök (vagy a tárolókat) toohandle hello hello fürt naplózza.</span><span class="sxs-lookup"><span data-stu-id="6083c-113">If a Docker log driver is specified, it is necessary toodeploy agents (or containers) toohandle hello logs in hello cluster.</span></span>  <span data-ttu-id="6083c-114">Hello `DriverOption` címke használt toospecify illesztőprogram naplóbeállításainak is lehet.</span><span class="sxs-lookup"><span data-stu-id="6083c-114">hello `DriverOption` tag can be used toospecify log driver options as well.</span></span>

<span data-ttu-id="6083c-115">Tekintse meg a következő cikkek toodeploy tárolók tooa Service Fabric-fürt toohello:</span><span class="sxs-lookup"><span data-stu-id="6083c-115">Refer toohello following articles toodeploy containers tooa Service Fabric cluster:</span></span>


[<span data-ttu-id="6083c-116">Üzembe egy tárolót a Service Fabric</span><span class="sxs-lookup"><span data-stu-id="6083c-116">Deploy a container on Service Fabric</span></span>](service-fabric-deploy-container.md)

