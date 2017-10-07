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
# <a name="specifying-volume-plugins-and-logging-drivers-for-your-container"></a>Adja meg a kötet beépülő modulok és illesztőprogramok a tároló-naplózás

A Service Fabric támogatja megadó [Docker kötet beépülő modulok](https://docs.docker.com/engine/extend/plugins_volume/) és [Docker naplózási illesztőprogramok](https://docs.docker.com/engine/admin/logging/overview/) a tárolószolgáltatás számára. hello beépülő modulok hello alkalmazásjegyzékben vannak megadva, ahogy az a következő jegyzékfájl hello:


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

A fenti példa hello, hello `Source` hello címkéjének `Volume` toohello forrásmappa hivatkozik. hello forrásmappa lehet hello hello tárolók vagy egy állandó távoli tároló üzemeltető virtuális gép egyik mappájába. Hello `Destination` címke hello hello helyre `Source` csatlakoztatott toowithin hello fut tároló. 

Egy kötet beépülő modul megadásakor a Service Fabric automatikusan létrehozza a hello kötet hello paraméterek vannak megadva. Hello `Source` címke hello nevét, valamint hello kötet hello `Driver` kódon hello kötet illesztőprogram beépülő modul. Beállítások adhatók meg hello `DriverOption` címkét, ahogy az alábbi részlet hello:

```xml
<Volume Source="myvolume1" Destination="c:\testmountlocation4" Driver="azurefile" IsReadOnly="true">
           <DriverOption Name="share" Value="models"/>
</Volume>
```

A Docker napló illesztőprogram meg van adva, esetén szükséges toodeploy ügynökök (vagy a tárolókat) toohandle hello hello fürt naplózza.  Hello `DriverOption` címke használt toospecify illesztőprogram naplóbeállításainak is lehet.

Tekintse meg a következő cikkek toodeploy tárolók tooa Service Fabric-fürt toohello:


[Üzembe egy tárolót a Service Fabric](service-fabric-deploy-container.md)

