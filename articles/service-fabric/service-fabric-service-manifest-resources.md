---
title: "a Service Fabric Szolgáltatásvégpontok aaaSpecifying |} Microsoft Docs"
description: "Hogyan toodescribe végpont erőforrásokat a service manifest, egyebek között be HTTPS-végpontnak tooset"
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: da36cbdb-6531-4dae-88e8-a311ab71520d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: subramar
ms.openlocfilehash: a4ebee353ce5cf86583673674246094f03f368be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="specify-resources-in-a-service-manifest"></a><span data-ttu-id="a9ca9-103">Erőforrások meghatározása szolgáltatásjegyzékben</span><span class="sxs-lookup"><span data-stu-id="a9ca9-103">Specify resources in a service manifest</span></span>
## <a name="overview"></a><span data-ttu-id="a9ca9-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="a9ca9-104">Overview</span></span>
<span data-ttu-id="a9ca9-105">hello szolgáltatás jegyzékfájl lehetővé teszi, hogy a lefordított hello programkód módosítása nélkül deklarált, illetve nem módosítható hello szolgáltatás toobe által használt erőforrások.</span><span class="sxs-lookup"><span data-stu-id="a9ca9-105">hello service manifest allows resources that are used by hello service toobe declared/changed without changing hello compiled code.</span></span> <span data-ttu-id="a9ca9-106">Az Azure Service Fabric endpoint erőforrás hello szolgáltatást támogatja.</span><span class="sxs-lookup"><span data-stu-id="a9ca9-106">Azure Service Fabric supports configuration of endpoint resources for hello service.</span></span> <span data-ttu-id="a9ca9-107">hello toohello erőforrások eléréséhez hello szolgáltatás jegyzékben megadott hello a(z) biztonsági csoporthoz hello alkalmazásjegyzékben keresztül lehet irányítani.</span><span class="sxs-lookup"><span data-stu-id="a9ca9-107">hello access toohello resources that are specified in hello service manifest can be controlled via hello SecurityGroup in hello application manifest.</span></span> <span data-ttu-id="a9ca9-108">erőforrások hello deklaráció lehetővé teszi, hogy ezek a központi telepítéskor megváltozott, azaz hello szolgáltatást nem kell egy olyan új konfigurációs mechanizmus toointroduce erőforrások toobe.</span><span class="sxs-lookup"><span data-stu-id="a9ca9-108">hello declaration of resources allows these resources toobe changed at deployment time, meaning hello service doesn't need toointroduce a new configuration mechanism.</span></span> <span data-ttu-id="a9ca9-109">hello hello ServiceManifest.xml fájl sémadefiníciót hello Service Fabric SDK telepítve van és eszközöket túl*C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span><span class="sxs-lookup"><span data-stu-id="a9ca9-109">hello schema definition for hello ServiceManifest.xml file is installed with hello Service Fabric SDK and tools too*C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span></span>

## <a name="endpoints"></a><span data-ttu-id="a9ca9-110">Végpontok</span><span class="sxs-lookup"><span data-stu-id="a9ca9-110">Endpoints</span></span>
<span data-ttu-id="a9ca9-111">Egy végpont erőforrás hello szolgáltatás jegyzékben van definiálva, amikor a Service Fabric rendeli hozzá portokat fenntartott hello alkalmazás porttartományát megadásakor a port nincs explicit módon.</span><span class="sxs-lookup"><span data-stu-id="a9ca9-111">When an endpoint resource is defined in hello service manifest, Service Fabric assigns ports from hello reserved application port range when a port isn't specified explicitly.</span></span> <span data-ttu-id="a9ca9-112">Hello végpont például meg *ServiceEndpoint1* hello jegyzék részlet e után szerepel.</span><span class="sxs-lookup"><span data-stu-id="a9ca9-112">For example, look at hello endpoint *ServiceEndpoint1* specified in hello manifest snippet provided after this paragraph.</span></span> <span data-ttu-id="a9ca9-113">Emellett szolgáltatások egy adott erőforrás-portot is kérheti.</span><span class="sxs-lookup"><span data-stu-id="a9ca9-113">Additionally, services can also request a specific port in a resource.</span></span> <span data-ttu-id="a9ca9-114">Különböző fürt csomópontjain futó szolgáltatás replikák rendelhetők hozzá különböző portszámokat, amíg futó szolgáltatás replikák hello azonos csomópont megosztás hello portot.</span><span class="sxs-lookup"><span data-stu-id="a9ca9-114">Service replicas running on different cluster nodes can be assigned different port numbers, while replicas of a service running on hello same node share hello port.</span></span> <span data-ttu-id="a9ca9-115">hello szolgáltatás replikák használhatja ezeket a portokat szükség szerint a replikáció és a figyelő az ügyféli kérelmek részére.</span><span class="sxs-lookup"><span data-stu-id="a9ca9-115">hello service replicas can then use these ports as needed for replication and listening for client requests.</span></span>

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
    <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
    <Endpoint Name="ServiceEndpoint3" Protocol="https"/>
  </Endpoints>
</Resources>
```

<span data-ttu-id="a9ca9-116">Tekintse meg a túl[állapotalapú Reliable Services konfigurálása](service-fabric-reliable-services-configuration.md) tooread végpontok hivatkozik a hello konfigurációs csomag beállításfájl (settings.xml) tájékozódhat.</span><span class="sxs-lookup"><span data-stu-id="a9ca9-116">Refer too[Configuring stateful Reliable Services](service-fabric-reliable-services-configuration.md) tooread more about referencing endpoints from hello config package settings file (settings.xml).</span></span>

## <a name="example-specifying-an-http-endpoint-for-your-service"></a><span data-ttu-id="a9ca9-117">Példa: a szolgáltatás HTTP-végponttal megadása</span><span class="sxs-lookup"><span data-stu-id="a9ca9-117">Example: specifying an HTTP endpoint for your service</span></span>
<span data-ttu-id="a9ca9-118">hello szolgáltatás következő jegyzékfájl definiálja a TCP-végpontot egy erőforrást és két HTTP-végpont erőforrás hello &lt;erőforrások&gt; elemet.</span><span class="sxs-lookup"><span data-stu-id="a9ca9-118">hello following service manifest defines one TCP endpoint resource and two HTTP endpoint resources in hello &lt;Resources&gt; element.</span></span>

<span data-ttu-id="a9ca9-119">HTTP-végpontokról a program automatikusan hozzáférés-vezérlési lista a Service Fabric használna.</span><span class="sxs-lookup"><span data-stu-id="a9ca9-119">HTTP endpoints are automatically ACL'd by Service Fabric.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="Stateful1Pkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is hello name of your ServiceType.
         This name must match hello string used in hello RegisterServiceType call in Program.cs. -->
    <StatefulServiceType ServiceTypeName="Stateful1Type" HasPersistedState="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ExeHost>
        <Program>Stateful1.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <!-- Config package is hello contents of hello Config directoy under PackageRoot that contains an
       independently updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by hello communication listener tooobtain hello port number on which to
           listen. Note that if your service is partitioned, this port is shared with
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
      <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
      <Endpoint Name="ServiceEndpoint3" Protocol="https"/>

      <!-- This endpoint is used by hello replicator for replicating hello state of your service.
           This endpoint is configured through hello ReplicatorSettings config section in hello Settings.xml
           file under hello ConfigPackage. -->
      <Endpoint Name="ReplicatorEndpoint" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```

## <a name="example-specifying-an-https-endpoint-for-your-service"></a><span data-ttu-id="a9ca9-120">Példa: a szolgáltatás HTTPS-végpontnak megadása</span><span class="sxs-lookup"><span data-stu-id="a9ca9-120">Example: specifying an HTTPS endpoint for your service</span></span>
<span data-ttu-id="a9ca9-121">hello HTTPS protokollt kiszolgáló hitelesítést biztosít, és ügyfél – kiszolgáló kommunikáció titkosítására is használja.</span><span class="sxs-lookup"><span data-stu-id="a9ca9-121">hello HTTPS protocol provides server authentication and is also used for encrypting client-server communication.</span></span> <span data-ttu-id="a9ca9-122">a Service Fabric-szolgáltatás, a HTTPS tooenable hello protokoll meg hello *erőforrások -> Végpontok végpont ->* hello szolgáltatás jegyzék, amint azt korábban hello végpont szakasz *ServiceEndpoint3* .</span><span class="sxs-lookup"><span data-stu-id="a9ca9-122">tooenable HTTPS on your Service Fabric service, specify hello protocol in hello *Resources -> Endpoints -> Endpoint* section of hello service manifest, as shown earlier for hello endpoint *ServiceEndpoint3*.</span></span>

> [!NOTE]
> <span data-ttu-id="a9ca9-123">A szolgáltatás protokoll nem módosítható az alkalmazásfrissítés során.</span><span class="sxs-lookup"><span data-stu-id="a9ca9-123">A service’s protocol cannot be changed during application upgrade.</span></span> <span data-ttu-id="a9ca9-124">Amennyiben a frissítés során megváltoztak, akkor használhatatlanná tévő változást.</span><span class="sxs-lookup"><span data-stu-id="a9ca9-124">If it is changed during upgrade, it is a breaking change.</span></span>
> 
> 

<span data-ttu-id="a9ca9-125">Íme egy példa applicationmanifest jegyzékben tooset szüksége HTTPS-hez.</span><span class="sxs-lookup"><span data-stu-id="a9ca9-125">Here is an example ApplicationManifest that you need tooset for HTTPS.</span></span> <span data-ttu-id="a9ca9-126">a tanúsítvány ujjlenyomatát hello meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="a9ca9-126">hello thumbprint for your certificate must be provided.</span></span> <span data-ttu-id="a9ca9-127">hello EndpointRef egy hivatkozási tooEndpointResource a ServiceManifest, amelynek hello HTTPS protokoll beállítása.</span><span class="sxs-lookup"><span data-stu-id="a9ca9-127">hello EndpointRef is a reference tooEndpointResource in ServiceManifest, for which you set hello HTTPS protocol.</span></span> <span data-ttu-id="a9ca9-128">Egynél több EndpointCertificate adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="a9ca9-128">You can add more than one EndpointCertificate.</span></span>  

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="Application1Type"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Parameters>
    <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="3" />
    <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
    <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
  </Parameters>
  <!-- Import hello ServiceManifest from hello ServicePackage. hello ServiceManifestName and ServiceManifestVersion
       should match hello Name and Version attributes of hello ServiceManifest element defined in the
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateful1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <EndpointBindingPolicy CertificateRef="TestCert1" EndpointRef="ServiceEndpoint3"/>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- hello section below creates instances of service types when an instance of this
         application type is created. You can also create one or more instances of service type by using the
         Service Fabric PowerShell module.

         hello attribute ServiceTypeName below must match hello name defined in hello imported ServiceManifest.xml file. -->
    <Service Name="Stateful1">
      <StatefulService ServiceTypeName="Stateful1Type" TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]" MinReplicaSetSize="[Stateful1_ ]">
        <UniformInt64Partition PartitionCount="[Stateful1_PartitionCount]" LowKey="-9223372036854775808" HighKey="9223372036854775807" />
      </StatefulService>
    </Service>
  </DefaultServices>
  <Certificates>
    <EndpointCertificate Name="TestCert1" X509FindValue="FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF F0" X509StoreName="MY" />  
  </Certificates>
</ApplicationManifest>
```

## <a name="overriding-endpoints-in-servicemanifestxml"></a><span data-ttu-id="a9ca9-129">Végpontok ServiceManifest.xml felülbírálása</span><span class="sxs-lookup"><span data-stu-id="a9ca9-129">Overriding Endpoints in ServiceManifest.xml</span></span>

<span data-ttu-id="a9ca9-130">Adja hozzá egy testvér tooConfigOverrides szakasz használandó ResourceOverrides szakasz hello applicationmanifest jegyzékben.</span><span class="sxs-lookup"><span data-stu-id="a9ca9-130">In hello ApplicationManifest add a ResourceOverrides section which will be a sibling tooConfigOverrides section.</span></span> <span data-ttu-id="a9ca9-131">Ebben a szakaszban hello végpontok szakasz hello szolgáltatás jegyzékben megadott hello erőforrások szakaszában hello felülbírálást is megadhat.</span><span class="sxs-lookup"><span data-stu-id="a9ca9-131">In this section you can specify hello override for hello Endpoints section in hello resources section specified in hello Service manifest.</span></span>

<span data-ttu-id="a9ca9-132">A sorrend toooverride ServiceManifest használatával ApplicationParameters módosítása a végpont hello applicationmanifest jegyzékben következőként:</span><span class="sxs-lookup"><span data-stu-id="a9ca9-132">In order toooverride EndPoint in ServiceManifest using ApplicationParameters change hello ApplicationManifest as following:</span></span>

<span data-ttu-id="a9ca9-133">Hello ServiceManifestImport szakaszban a "ResourceOverrides" új szakasz hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a9ca9-133">In hello ServiceManifestImport section add a new section "ResourceOverrides"</span></span>

```xml
<ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <ResourceOverrides>
      <Endpoints>
        <Endpoint Name="ServiceEndpoint" Port="[Port]" Protocol="[Protocol]" Type="[Type]" />
        <Endpoint Name="ServiceEndpoint1" Port="[Port1]" Protocol="[Protocol1] "/>
      </Endpoints>
    </ResourceOverrides>
        <Policies>
           <EndpointBindingPolicy CertificateRef="TestCert1" EndpointRef="ServiceEndpoint"/>
        </Policies>
  </ServiceManifestImport>
```

<span data-ttu-id="a9ca9-134">A Paraméterek hozzáadása alatt hello:</span><span class="sxs-lookup"><span data-stu-id="a9ca9-134">In hello Parameters add below:</span></span>

```xml
  <Parameters>
    <Parameter Name="Port" DefaultValue="" />
    <Parameter Name="Protocol" DefaultValue="" />
    <Parameter Name="Type" DefaultValue="" />
    <Parameter Name="Port1" DefaultValue="" />
    <Parameter Name="Protocol1" DefaultValue="" />
  </Parameters>
```

<span data-ttu-id="a9ca9-135">Most hello alkalmazás telepítése közben átadhatók ezeket az értékeket a ApplicationParameters, például:</span><span class="sxs-lookup"><span data-stu-id="a9ca9-135">While deploying hello application now you can pass in these values as ApplicationParameters for example:</span></span>

```powershell
PS C:\> New-ServiceFabricApplication -ApplicationName fabric:/myapp -ApplicationTypeName "AppType" -ApplicationTypeVersion "1.0.0" -ApplicationParameter @{Port='1001'; Protocol='https'; Type='Input'; Port1='2001'; Protocol='http'}
```

<span data-ttu-id="a9ca9-136">Megjegyzés: Ha hello értékek lehetővé teszik a hello ApplicationParameters üres érték azt vissza toohello alapértelmezett hello ServiceManifest hello EndPointName megfelelő a megadott érték.</span><span class="sxs-lookup"><span data-stu-id="a9ca9-136">Note: If hello values provide for hello ApplicationParameters is empty we go back toohello default value provided in hello ServiceManifest for hello corresponding EndPointName.</span></span>

<span data-ttu-id="a9ca9-137">Példa:</span><span class="sxs-lookup"><span data-stu-id="a9ca9-137">For example:</span></span>

<span data-ttu-id="a9ca9-138">Ha a megadott ServiceManifest hello</span><span class="sxs-lookup"><span data-stu-id="a9ca9-138">If in hello ServiceManifest you specified</span></span>

```xml
  <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpoint1" Protocol="tcp"/>
    </Endpoints>
  </Resources>
```

<span data-ttu-id="a9ca9-139">Hello Port1, és Protocol1 alkalmazás paraméter értéke null vagy üres.</span><span class="sxs-lookup"><span data-stu-id="a9ca9-139">And hello Port1 and Protocol1 value for Application parameters is null or empty.</span></span> <span data-ttu-id="a9ca9-140">hello port továbbra is, amelyekről ServiceFabric.</span><span class="sxs-lookup"><span data-stu-id="a9ca9-140">hello port is still decided by ServiceFabric.</span></span> <span data-ttu-id="a9ca9-141">És hello protokoll tcp lesz.</span><span class="sxs-lookup"><span data-stu-id="a9ca9-141">And hello Protocol will tcp.</span></span>

<span data-ttu-id="a9ca9-142">Tegyük fel, adjon meg egy megfelelő értéket.</span><span class="sxs-lookup"><span data-stu-id="a9ca9-142">Suppose you specify a wrong value.</span></span> <span data-ttu-id="a9ca9-143">Például a porthoz megadott karakterlánc-érték "Foo" helyett egy egész szám.  Új ServiceFabricApplication parancs egy hibával meghiúsul: hello felülbíráló paraméter neve "ServiceEndpoint1" attribútum "Port1" szakaszban "ResourceOverrides" értéke érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="a9ca9-143">Like for Port you specified a string value "Foo" instead of an int.  New-ServiceFabricApplication command will fail with an error : hello override parameter with name 'ServiceEndpoint1' attribute 'Port1' in section 'ResourceOverrides' is invalid.</span></span> <span data-ttu-id="a9ca9-144">hello érték van megadva a "PEL", és szükség a "int".</span><span class="sxs-lookup"><span data-stu-id="a9ca9-144">hello value specified is 'Foo' and required is 'int'.</span></span>
