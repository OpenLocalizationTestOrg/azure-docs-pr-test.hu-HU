---
title: "aaaManage több környezeteknek a Service Fabric |} Microsoft Docs"
description: "Service Fabric-alkalmazások futtatható fürtökben, amelyek mérete a egy gép toothousands gépek között. Bizonyos esetekben érdemes tooconfigure eltérően az alkalmazását ezen változatos környezetekben. Ez a cikk ismerteti hogyan toodefine különböző alkalmazás paraméterei / környezetben."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: 
ms.assetid: f406eac9-7271-4c37-a0d3-0a2957b60537
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: mikkelhegn
ms.openlocfilehash: 2b3327e0e1a3bbd35a50835e720619f308b1b501
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-application-parameters-for-multiple-environments"></a><span data-ttu-id="65005-105">Alkalmazás paramétereinek több környezet kezelése</span><span class="sxs-lookup"><span data-stu-id="65005-105">Manage application parameters for multiple environments</span></span>
<span data-ttu-id="65005-106">Azure Service Fabric-fürtök használatával bárhol egy toomany több ezer gépek hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="65005-106">You can create Azure Service Fabric clusters by using anywhere from one toomany thousands of machines.</span></span> <span data-ttu-id="65005-107">Bináris alkalmazásfájlokat módosítás nélkül futtathatja a széles skáláját környezetek között, miközben gyakran szeretné tooconfigure hello alkalmazás másképp, attól függően, hogy a gépek való telepítése esetén hello száma.</span><span class="sxs-lookup"><span data-stu-id="65005-107">While application binaries can run without modification across this wide spectrum of environments, you often want tooconfigure hello application differently, depending on hello number of machines you're deploying to.</span></span>

<span data-ttu-id="65005-108">Egy egyszerű példa, fontolja meg a `InstanceCount` az állapotmentes szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="65005-108">As a simple example, consider `InstanceCount` for a stateless service.</span></span> <span data-ttu-id="65005-109">Amikor alkalmazásokat futtat az Azure-ban, általában kívánt tooset a toohello különleges paraméterérték a "-"1.</span><span class="sxs-lookup"><span data-stu-id="65005-109">When you are running applications in Azure, you generally want tooset this parameter toohello special value of "-1".</span></span> <span data-ttu-id="65005-110">Ez a konfiguráció biztosítja, hogy fut-e a szolgáltatás minden egyes csomópontjára hello fürt (vagy minden csomópont hello csomóponttípus, ha egy elhelyezési korlátozás van beállítva).</span><span class="sxs-lookup"><span data-stu-id="65005-110">This configuration ensures that your service is running on every node in hello cluster (or every node in hello node type if you have set a placement constraint).</span></span> <span data-ttu-id="65005-111">Azonban ez a konfiguráció nem alkalmas egyszámítógépes fürt hello figyel azonos több folyamat nem tartozik végpont egy gépen.</span><span class="sxs-lookup"><span data-stu-id="65005-111">However, this configuration is not suitable for a single-machine cluster since you cannot have multiple processes listening on hello same endpoint on a single machine.</span></span> <span data-ttu-id="65005-112">Ehelyett általában beállított `InstanceCount` túl "1".</span><span class="sxs-lookup"><span data-stu-id="65005-112">Instead, you typically set `InstanceCount` too"1".</span></span>

## <a name="specifying-environment-specific-parameters"></a><span data-ttu-id="65005-113">Környezetfüggő paramétereinek megadása</span><span class="sxs-lookup"><span data-stu-id="65005-113">Specifying environment-specific parameters</span></span>
<span data-ttu-id="65005-114">hello megoldás toothis konfigurációs probléma a paraméteres alapértelmezett szolgáltatások és alkalmazásparaméter-fájlokat az adott értékei az adott környezetben.</span><span class="sxs-lookup"><span data-stu-id="65005-114">hello solution toothis configuration issue is a set of parameterized default services and application parameter files that fill in those parameter values for a given environment.</span></span> <span data-ttu-id="65005-115">Alapértelmezett szolgáltatások és alkalmazás paraméterei hello alkalmazásban és a szolgáltatás a jegyzékfájlban.</span><span class="sxs-lookup"><span data-stu-id="65005-115">Default services and application parameters are configured in hello application and service manifests.</span></span> <span data-ttu-id="65005-116">hello sémadefiníciót hello ServiceManifest.xml és ApplicationManifest.xml fájlok hello Service Fabric SDK telepítve van és eszközöket túl*C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span><span class="sxs-lookup"><span data-stu-id="65005-116">hello schema definition for hello ServiceManifest.xml and ApplicationManifest.xml files is installed with hello Service Fabric SDK and tools too*C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span></span>

### <a name="default-services"></a><span data-ttu-id="65005-117">Alapértelmezett szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="65005-117">Default services</span></span>
<span data-ttu-id="65005-118">Service Fabric-alkalmazások szolgáltatáspéldány gyűjteménye épülnek fel.</span><span class="sxs-lookup"><span data-stu-id="65005-118">Service Fabric applications are made up of a collection of service instances.</span></span> <span data-ttu-id="65005-119">Toocreate üres alkalmazás lehet, és hozzon létre minden szolgáltatáspéldány dinamikusan, a legtöbb alkalmazás rendelkezik alapvető szolgáltatások mindig jöjjenek létre, amikor hello alkalmazás létrejön.</span><span class="sxs-lookup"><span data-stu-id="65005-119">While it is possible for you toocreate an empty application and then create all service instances dynamically, most applications have a set of core services that should always be created when hello application is instantiated.</span></span> <span data-ttu-id="65005-120">Ezek a hivatkozott tooas "alapértelmezett szolgáltatások".</span><span class="sxs-lookup"><span data-stu-id="65005-120">These are referred tooas "default services".</span></span> <span data-ttu-id="65005-121">Hello alkalmazásjegyzék szögletes zárójelek között szerepel környezeti konfiguráció helyőrzőkkel vannak megadva:</span><span class="sxs-lookup"><span data-stu-id="65005-121">They are specified in hello application manifest, with placeholders for per-environment configuration included in square brackets:</span></span>

```xml
  <DefaultServices>
      <Service Name="Stateful1">
          <StatefulService
              ServiceTypeName="Stateful1Type"
              TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]"
              MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">

              <UniformInt64Partition
                  PartitionCount="[Stateful1_PartitionCount]"
                  LowKey="-9223372036854775808"
                  HighKey="9223372036854775807"
              />
        </StatefulService>
    </Service>
  </DefaultServices>
```

<span data-ttu-id="65005-122">Egyes paraméterek nevű hello hello paraméterek elem hello az alkalmazás jegyzékének belül kell megadni:</span><span class="sxs-lookup"><span data-stu-id="65005-122">Each of hello named parameters must be defined within hello Parameters element of hello application manifest:</span></span>

```xml
    <Parameters>
        <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="3" />
        <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
        <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
    </Parameters>
```

<span data-ttu-id="65005-123">hello DefaultValue attribútumot hello érték toobe egy adott környezetben használt hello hiányában egy több-specifikus paraméter határozza meg.</span><span class="sxs-lookup"><span data-stu-id="65005-123">hello DefaultValue attribute specifies hello value toobe used in hello absence of a more-specific parameter for a given environment.</span></span>

> [!NOTE]
> <span data-ttu-id="65005-124">Nem minden szolgáltatás példány paraméterei megfelelő környezet konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="65005-124">Not all service instance parameters are suitable for per-environment configuration.</span></span> <span data-ttu-id="65005-125">Hello a fenti példában a hello LowKey és hello szolgáltatás particionálási sémát HighKey értékeinek explicit módon definiálva hello szolgáltatás minden példányának mivel hello partíciótartomány hello adatok tartomány nem hello környezet függvényében.</span><span class="sxs-lookup"><span data-stu-id="65005-125">In hello example above, hello LowKey and HighKey values for hello service's partitioning scheme are explicitly defined for all instances of hello service since hello partition range is a function of hello data domain, not hello environment.</span></span>
> 
> 

### <a name="per-environment-service-configuration-settings"></a><span data-ttu-id="65005-126">Környezet szolgáltatás konfigurációs beállításai</span><span class="sxs-lookup"><span data-stu-id="65005-126">Per-environment service configuration settings</span></span>
<span data-ttu-id="65005-127">Hello [Service Fabric-alkalmazás modell](service-fabric-application-model.md) lehetővé teszi, hogy szolgáltatások futási időben olvasható egyéni kulcs-érték párokat tartalmazó tooinclude konfigurációs csomagokat.</span><span class="sxs-lookup"><span data-stu-id="65005-127">hello [Service Fabric application model](service-fabric-application-model.md) enables services tooinclude configuration packages that contain custom key-value pairs that are readable at run time.</span></span> <span data-ttu-id="65005-128">Ezek a beállítások értékeit hello is is lehet szerint megkülönböztetett környezet megadásával egy `ConfigOverride` hello alkalmazásjegyzékben.</span><span class="sxs-lookup"><span data-stu-id="65005-128">hello values of these settings can also be differentiated by environment by specifying a `ConfigOverride` in hello application manifest.</span></span>

<span data-ttu-id="65005-129">Tegyük fel, hogy rendelkezik-e a következő fájlban hello Config\Settings.xml hello beállítás hello `Stateful1` szolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="65005-129">Suppose that you have hello following setting in hello Config\Settings.xml file for hello `Stateful1` service:</span></span>

```xml
  <Section Name="MyConfigSection">
     <Parameter Name="MaxQueueSize" Value="25" />
  </Section>
```
<span data-ttu-id="65005-130">toooverride ezt az értéket egy adott alkalmazás-környezet párhoz, hozzon létre egy `ConfigOverride` hello szolgáltatás jegyzékfájl hello alkalmazásjegyzékben importálásakor.</span><span class="sxs-lookup"><span data-stu-id="65005-130">toooverride this value for a specific application/environment pair, create a `ConfigOverride` when you import hello service manifest in hello application manifest.</span></span>

```xml
  <ConfigOverrides>
     <ConfigOverride Name="Config">
        <Settings>
           <Section Name="MyConfigSection">
              <Parameter Name="MaxQueueSize" Value="[Stateful1_MaxQueueSize]" />
           </Section>
        </Settings>
     </ConfigOverride>
  </ConfigOverrides>
```
<span data-ttu-id="65005-131">Ennek a paraméternek, majd konfigurálhatja környezet a fentiek szerint.</span><span class="sxs-lookup"><span data-stu-id="65005-131">This parameter can then be configured by environment as shown above.</span></span> <span data-ttu-id="65005-132">Ehhez deklaráló azt hello paraméterek szakaszban hello az alkalmazás jegyzékének és környezetfüggő értékek megadása hello alkalmazásparaméter-fájlokat.</span><span class="sxs-lookup"><span data-stu-id="65005-132">You can do this by declaring it in hello parameters section of hello application manifest and specifying environment-specific values in hello application parameter files.</span></span>

> [!NOTE]
> <span data-ttu-id="65005-133">A szolgáltatás konfigurációs beállításainak esetet hello, nincsenek három helyen, ahol a kulcs értékének hello állítható be: hello szolgáltatás konfigurációs csomagot, hello alkalmazásjegyzék és hello alkalmazás paraméterfájl.</span><span class="sxs-lookup"><span data-stu-id="65005-133">In hello case of service configuration settings, there are three places where hello value of a key can be set: hello service configuration package, hello application manifest, and hello application parameter file.</span></span> <span data-ttu-id="65005-134">A Service Fabric fog mindig válasszon hello alkalmazás paraméterfájl először (ha az meg van adva), majd az alkalmazásjegyzék hello, és végül a konfigurációs csomag hello.</span><span class="sxs-lookup"><span data-stu-id="65005-134">Service Fabric will always choose from hello application parameter file first (if specified), then hello application manifest, and finally hello configuration package.</span></span>
> 
> 

### <a name="setting-and-using-environment-variables"></a><span data-ttu-id="65005-135">És környezeti változók használatához</span><span class="sxs-lookup"><span data-stu-id="65005-135">Setting and using environment variables</span></span> 
<span data-ttu-id="65005-136">Adja meg és állítsa be a környezeti változók hello ServiceManifest.xml fájlban, és ezt felülbírálhatja hello ApplicationManifest.xml fájlba / példány alapon.</span><span class="sxs-lookup"><span data-stu-id="65005-136">You can specify and set environment variables in hello ServiceManifest.xml file and then override these in hello ApplicationManifest.xml file on a per instance basis.</span></span>
<span data-ttu-id="65005-137">hello az alábbi példában két környezeti változókat, egy-egy értéket állítsa be, és más hello felülbírálja.</span><span class="sxs-lookup"><span data-stu-id="65005-137">hello example below shows two environment variables, one with a value set and hello other is overridden.</span></span> <span data-ttu-id="65005-138">Használhatja az alkalmazás paraméterei tooset környezeti változók értékei hello azonos módon, hogy azokat config felülbírálások használták.</span><span class="sxs-lookup"><span data-stu-id="65005-138">You can use application parameters tooset environment variables values in hello same way that these were used for config overrides.</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="MyCode" Version="CodeVersion1">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
    <EnvironmentVariables>
      <EnvironmentVariable Name="MyEnvVariable" Value=""/>
      <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
    </EnvironmentVariables>
  </CodePackage>
  <ConfigPackage Name="MyConfig" Version="ConfigVersion1" />
  <DataPackage Name="MyData" Version="DataVersion1" />
</ServiceManifest>
```
<span data-ttu-id="65005-139">toooverride hello környezeti változók hello ApplicationManifest.xml, hivatkozás hello kódcsomag hello ServiceManifest a hello `EnvironmentOverrides` elemet.</span><span class="sxs-lookup"><span data-stu-id="65005-139">toooverride hello environment variables in hello ApplicationManifest.xml, reference hello code package in hello ServiceManifest with hello `EnvironmentOverrides` element.</span></span>

```xml
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="FrontEndServicePkg" ServiceManifestVersion="1.0.0" />
    <EnvironmentOverrides CodePackageRef="MyCode">
      <EnvironmentVariable Name="MyEnvVariable" Value="mydata"/>
    </EnvironmentOverrides>
  </ServiceManifestImport>
 ``` 
 <span data-ttu-id="65005-140">Hello nevű szolgáltatáspéldány létrehozása után a hello környezeti változók kódból végezheti el.</span><span class="sxs-lookup"><span data-stu-id="65005-140">Once hello named service instance is created you can access hello environment variables from code.</span></span> <span data-ttu-id="65005-141">például a C# teheti hello következő</span><span class="sxs-lookup"><span data-stu-id="65005-141">e.g. In C# you can do hello following</span></span>

```csharp
    string EnvVariable = Environment.GetEnvironmentVariable("MyEnvVariable");
```

### <a name="service-fabric-environment-variables"></a><span data-ttu-id="65005-142">A Service Fabric környezeti változók</span><span class="sxs-lookup"><span data-stu-id="65005-142">Service Fabric environment variables</span></span>
<span data-ttu-id="65005-143">A Service Fabric környezeti változók beállítása minden szolgáltatáspéldány létrehozta.</span><span class="sxs-lookup"><span data-stu-id="65005-143">Service Fabric has built in environment variables set for each service instance.</span></span> <span data-ttu-id="65005-144">hello környezeti változók teljes listáját van az alábbiakban, ahol hello azokat, félkövérrel szedett is hello azokon, szüksége lesz a szolgáltatás hello más alatt Service Fabric-futtatókörnyezet által használt.</span><span class="sxs-lookup"><span data-stu-id="65005-144">hello full list of environment variables is below, where hello ones in bold are hello ones that you will use in your service, hello other being used by Service Fabric runtime.</span></span> 

* <span data-ttu-id="65005-145">Fabric_ApplicationHostId</span><span class="sxs-lookup"><span data-stu-id="65005-145">Fabric_ApplicationHostId</span></span>
* <span data-ttu-id="65005-146">Fabric_ApplicationHostType</span><span class="sxs-lookup"><span data-stu-id="65005-146">Fabric_ApplicationHostType</span></span>
* <span data-ttu-id="65005-147">Fabric_ApplicationId</span><span class="sxs-lookup"><span data-stu-id="65005-147">Fabric_ApplicationId</span></span>
* <span data-ttu-id="65005-148">**Fabric_ApplicationName**</span><span class="sxs-lookup"><span data-stu-id="65005-148">**Fabric_ApplicationName**</span></span>
* <span data-ttu-id="65005-149">Fabric_CodePackageInstanceId</span><span class="sxs-lookup"><span data-stu-id="65005-149">Fabric_CodePackageInstanceId</span></span>
* <span data-ttu-id="65005-150">**Fabric_CodePackageName**</span><span class="sxs-lookup"><span data-stu-id="65005-150">**Fabric_CodePackageName**</span></span>
* <span data-ttu-id="65005-151">**[YourServiceName] Fabric_Endpoint_ TypeEndpoint**</span><span class="sxs-lookup"><span data-stu-id="65005-151">**Fabric_Endpoint_[YourServiceName]TypeEndpoint**</span></span>
* <span data-ttu-id="65005-152">**Fabric_Folder_App_Log**</span><span class="sxs-lookup"><span data-stu-id="65005-152">**Fabric_Folder_App_Log**</span></span>
* <span data-ttu-id="65005-153">**Fabric_Folder_App_Temp**</span><span class="sxs-lookup"><span data-stu-id="65005-153">**Fabric_Folder_App_Temp**</span></span>
* <span data-ttu-id="65005-154">**Fabric_Folder_App_Work**</span><span class="sxs-lookup"><span data-stu-id="65005-154">**Fabric_Folder_App_Work**</span></span>
* <span data-ttu-id="65005-155">**Fabric_Folder_Application**</span><span class="sxs-lookup"><span data-stu-id="65005-155">**Fabric_Folder_Application**</span></span>
* <span data-ttu-id="65005-156">Fabric_NodeId</span><span class="sxs-lookup"><span data-stu-id="65005-156">Fabric_NodeId</span></span>
* <span data-ttu-id="65005-157">**Fabric_NodeIPOrFQDN**</span><span class="sxs-lookup"><span data-stu-id="65005-157">**Fabric_NodeIPOrFQDN**</span></span>
* <span data-ttu-id="65005-158">**Fabric_NodeName**</span><span class="sxs-lookup"><span data-stu-id="65005-158">**Fabric_NodeName**</span></span>
* <span data-ttu-id="65005-159">Fabric_RuntimeConnectionAddress</span><span class="sxs-lookup"><span data-stu-id="65005-159">Fabric_RuntimeConnectionAddress</span></span>
* <span data-ttu-id="65005-160">Fabric_ServicePackageInstanceId</span><span class="sxs-lookup"><span data-stu-id="65005-160">Fabric_ServicePackageInstanceId</span></span>
* <span data-ttu-id="65005-161">Fabric_ServicePackageName</span><span class="sxs-lookup"><span data-stu-id="65005-161">Fabric_ServicePackageName</span></span>
* <span data-ttu-id="65005-162">Fabric_ServicePackageVersionInstance</span><span class="sxs-lookup"><span data-stu-id="65005-162">Fabric_ServicePackageVersionInstance</span></span>
* <span data-ttu-id="65005-163">FabricPackageFileName</span><span class="sxs-lookup"><span data-stu-id="65005-163">FabricPackageFileName</span></span>

<span data-ttu-id="65005-164">hello kód belows bemutatja, hogyan toolist hello Service Fabric környezeti változók</span><span class="sxs-lookup"><span data-stu-id="65005-164">hello code belows shows how toolist hello Service Fabric environment variables</span></span>
 ```csharp
    foreach (DictionaryEntry de in Environment.GetEnvironmentVariables())
    {
        if (de.Key.ToString().StartsWith("Fabric"))
        {
            Console.WriteLine(" Environment variable {0} = {1}", de.Key, de.Value);
        }
    }
```
<span data-ttu-id="65005-165">hello következő példákban a környezeti változók nevű alkalmazás típusú `GuestExe.Application` egy szolgáltatás típusának neve `FrontEndService` futása közben a helyi fejlesztési számítógépén.</span><span class="sxs-lookup"><span data-stu-id="65005-165">hello following are examples of environment variables for an application type called `GuestExe.Application` with a service type called `FrontEndService` when run on your local dev machine.</span></span>

* <span data-ttu-id="65005-166">**Fabric_ApplicationName = fabric:/GuestExe.Application**</span><span class="sxs-lookup"><span data-stu-id="65005-166">**Fabric_ApplicationName = fabric:/GuestExe.Application**</span></span>
* <span data-ttu-id="65005-167">**Fabric_CodePackageName kód =**</span><span class="sxs-lookup"><span data-stu-id="65005-167">**Fabric_CodePackageName = Code**</span></span>
* <span data-ttu-id="65005-168">**Fabric_Endpoint_FrontEndServiceTypeEndpoint = 80**</span><span class="sxs-lookup"><span data-stu-id="65005-168">**Fabric_Endpoint_FrontEndServiceTypeEndpoint = 80**</span></span>
* <span data-ttu-id="65005-169">**Fabric_NodeIPOrFQDN = localhost**</span><span class="sxs-lookup"><span data-stu-id="65005-169">**Fabric_NodeIPOrFQDN = localhost**</span></span>
* <span data-ttu-id="65005-170">**Fabric_NodeName = _Node_2**</span><span class="sxs-lookup"><span data-stu-id="65005-170">**Fabric_NodeName = _Node_2**</span></span>

### <a name="application-parameter-files"></a><span data-ttu-id="65005-171">Alkalmazásparaméter-fájlokat</span><span class="sxs-lookup"><span data-stu-id="65005-171">Application parameter files</span></span>
<span data-ttu-id="65005-172">a Service Fabric-alkalmazás projekt hello tartalmazhat egy vagy több alkalmazásparaméter-fájlokat.</span><span class="sxs-lookup"><span data-stu-id="65005-172">hello Service Fabric application project can include one or more application parameter files.</span></span> <span data-ttu-id="65005-173">Azok meghatározza, hogy hello adott hello alkalmazás jegyzékében definiált hello paraméterek értékeit:</span><span class="sxs-lookup"><span data-stu-id="65005-173">Each of them defines hello specific values for hello parameters that are defined in hello application manifest:</span></span>

```xml
    <!-- ApplicationParameters\Local.xml -->

    <Application Name="fabric:/Application1" xmlns="http://schemas.microsoft.com/2011/01/fabric">
        <Parameters>
            <Parameter Name ="Stateful1_MinReplicaSetSize" Value="3" />
            <Parameter Name="Stateful1_PartitionCount" Value="1" />
            <Parameter Name="Stateful1_TargetReplicaSetSize" Value="3" />
        </Parameters>
    </Application>
```
<span data-ttu-id="65005-174">Új alkalmazás alapértelmezés szerint három alkalmazásparaméter-fájlokat, Local.1Node.xml, Local.5Node.xml és Cloud.xml nevű foglalja magában:</span><span class="sxs-lookup"><span data-stu-id="65005-174">By default, a new application includes three application parameter files, named Local.1Node.xml, Local.5Node.xml, and Cloud.xml:</span></span>

![A Solution Explorer alkalmazásparaméter-fájlokat][app-parameters-solution-explorer]

<span data-ttu-id="65005-176">a paraméterfájl toocreate egyszerűen másolja és illessze be egy meglévő, és adjon neki egy új nevet.</span><span class="sxs-lookup"><span data-stu-id="65005-176">toocreate a parameter file, simply copy and paste an existing one and give it a new name.</span></span>

## <a name="identifying-environment-specific-parameters-during-deployment"></a><span data-ttu-id="65005-177">Azonosító környezetfüggő paraméterek központi telepítése során</span><span class="sxs-lookup"><span data-stu-id="65005-177">Identifying environment-specific parameters during deployment</span></span>
<span data-ttu-id="65005-178">A központi telepítéskor a szükséges toochoose hello megfelelő paraméter fájl tooapply az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="65005-178">At deployment time, you need toochoose hello appropriate parameter file tooapply with your application.</span></span> <span data-ttu-id="65005-179">Ehhez a Visual Studio hello közzététel párbeszédpaneléről vagy a Powershellen keresztül.</span><span class="sxs-lookup"><span data-stu-id="65005-179">You can do this through hello Publish dialog in Visual Studio or through PowerShell.</span></span>

### <a name="deploy-from-visual-studio"></a><span data-ttu-id="65005-180">Üzembe helyezés a Visual Studióból</span><span class="sxs-lookup"><span data-stu-id="65005-180">Deploy from Visual Studio</span></span>
<span data-ttu-id="65005-181">Az alkalmazást a Visual Studio közzétételekor elérhető paraméter fájlok hello listája közül választhatnak.</span><span class="sxs-lookup"><span data-stu-id="65005-181">You can choose from hello list of available parameter files when you publish your application in Visual Studio.</span></span>

![Válassza ki a paraméterfájl hello közzététele párbeszédpanelen][publishdialog]

### <a name="deploy-from-powershell"></a><span data-ttu-id="65005-183">A PowerShell telepítése</span><span class="sxs-lookup"><span data-stu-id="65005-183">Deploy from PowerShell</span></span>
<span data-ttu-id="65005-184">Hello `Deploy-FabricApplication.ps1` hello alkalmazás projektsablon szereplő PowerShell-parancsfájl fogad paraméterként közzétételi profilt, és hello PublishProfile toohello alkalmazás referencia paraméterek-fájlt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="65005-184">hello `Deploy-FabricApplication.ps1` PowerShell script included in hello application project template accepts a publish profile as a parameter and hello PublishProfile contains a reference toohello application parameters file.</span></span>

  ```PowerShell
    ./Deploy-FabricApplication -ApplicationPackagePath <app_package_path> -PublishProfileFile <publishprofile_path>
  ```

## <a name="next-steps"></a><span data-ttu-id="65005-185">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="65005-185">Next steps</span></span>
<span data-ttu-id="65005-186">több azzal kapcsolatban, ebben a témakörben tárgyalt hello alapfogalmak toolearn lásd: hello [Service Fabric a műszaki áttekintés](service-fabric-technical-overview.md).</span><span class="sxs-lookup"><span data-stu-id="65005-186">toolearn more about some of hello core concepts that are discussed in this topic, see hello [Service Fabric technical overview](service-fabric-technical-overview.md).</span></span> <span data-ttu-id="65005-187">Más elérhető a Visual Studio alkalmazás-felügyeleti képességekkel kapcsolatos információkért lásd: [kezelése a Service Fabric-alkalmazások, a Visual Studio](service-fabric-manage-application-in-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="65005-187">For information about other app management capabilities that are available in Visual Studio, see [Manage your Service Fabric applications in Visual Studio](service-fabric-manage-application-in-visual-studio.md).</span></span>

<!-- Image references -->

[publishdialog]: ./media/service-fabric-manage-multiple-environment-app-configuration/publish-dialog-choose-app-config.png
[app-parameters-solution-explorer]:./media/service-fabric-manage-multiple-environment-app-configuration/app-parameters-in-solution-explorer.png
