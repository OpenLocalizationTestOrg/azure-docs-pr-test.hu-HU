---
title: "A Service Fabric több környezet kezelése |} Microsoft Docs"
description: "Service Fabric-alkalmazások futtatható fürtökben, amelyek több ezer gép egyik gépről mérete között. Bizonyos esetekben érdemes állítsa be az alkalmazását eltérően ezeket változatos környezetekben. Ez a cikk bemutatja, hogyan adhat környezet egy másik alkalmazás paramétereit."
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
ms.openlocfilehash: 9317b3f0b7984e795c4205360ed58e2c4f3fbcb1
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="manage-application-parameters-for-multiple-environments"></a><span data-ttu-id="52a71-105">Alkalmazás paramétereinek több környezet kezelése</span><span class="sxs-lookup"><span data-stu-id="52a71-105">Manage application parameters for multiple environments</span></span>
<span data-ttu-id="52a71-106">Azure Service Fabric-fürtök hozhat létre egyet a sok ezer gépek bárhol használva.</span><span class="sxs-lookup"><span data-stu-id="52a71-106">You can create Azure Service Fabric clusters by using anywhere from one to many thousands of machines.</span></span> <span data-ttu-id="52a71-107">Amíg a bináris alkalmazásfájlokat módosítás nélkül futtathatja a széles skáláját környezetek között, gyakran konfigurálni szeretné az alkalmazás másképp, attól függően, hogy hány számítógépre történő telepítése esetén.</span><span class="sxs-lookup"><span data-stu-id="52a71-107">While application binaries can run without modification across this wide spectrum of environments, you often want to configure the application differently, depending on the number of machines you're deploying to.</span></span>

<span data-ttu-id="52a71-108">Egy egyszerű példa, fontolja meg a `InstanceCount` az állapotmentes szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="52a71-108">As a simple example, consider `InstanceCount` for a stateless service.</span></span> <span data-ttu-id="52a71-109">Ha alkalmazások futnak az Azure-ban, általában szeretné a paraméter értéke a speciális érték-1".</span><span class="sxs-lookup"><span data-stu-id="52a71-109">When you are running applications in Azure, you generally want to set this parameter to the special value of "-1".</span></span> <span data-ttu-id="52a71-110">Ez a konfiguráció biztosítja, hogy fut-e a szolgáltatás minden csomóponton a fürtben (vagy minden csomópont, a csomópont típusban, ha egy elhelyezési korlátozás van beállítva).</span><span class="sxs-lookup"><span data-stu-id="52a71-110">This configuration ensures that your service is running on every node in the cluster (or every node in the node type if you have set a placement constraint).</span></span> <span data-ttu-id="52a71-111">Azonban ez a konfiguráció nem alkalmas egyszámítógépes fürt figyel egy gépen azonos végponton több folyamat nem tartozik.</span><span class="sxs-lookup"><span data-stu-id="52a71-111">However, this configuration is not suitable for a single-machine cluster since you cannot have multiple processes listening on the same endpoint on a single machine.</span></span> <span data-ttu-id="52a71-112">Ehelyett általában beállított `InstanceCount` "1".</span><span class="sxs-lookup"><span data-stu-id="52a71-112">Instead, you typically set `InstanceCount` to "1".</span></span>

## <a name="specifying-environment-specific-parameters"></a><span data-ttu-id="52a71-113">Környezetfüggő paramétereinek megadása</span><span class="sxs-lookup"><span data-stu-id="52a71-113">Specifying environment-specific parameters</span></span>
<span data-ttu-id="52a71-114">A megoldás a konfigurációs probléma a paraméteres alapértelmezett szolgáltatások és alkalmazásparaméter-fájlokat az adott értékei az adott környezetben.</span><span class="sxs-lookup"><span data-stu-id="52a71-114">The solution to this configuration issue is a set of parameterized default services and application parameter files that fill in those parameter values for a given environment.</span></span> <span data-ttu-id="52a71-115">Az alkalmazás és szolgáltatás jegyzékfájlokban alapértelmezett szolgáltatások és alkalmazások paraméterek vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="52a71-115">Default services and application parameters are configured in the application and service manifests.</span></span> <span data-ttu-id="52a71-116">A séma meghatározása a ServiceManifest.xml és ApplicationManifest.xml fájlok telepítve van a Service Fabric SDK-val, és az eszközök *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span><span class="sxs-lookup"><span data-stu-id="52a71-116">The schema definition for the ServiceManifest.xml and ApplicationManifest.xml files is installed with the Service Fabric SDK and tools to *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span></span>

### <a name="default-services"></a><span data-ttu-id="52a71-117">Alapértelmezett szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="52a71-117">Default services</span></span>
<span data-ttu-id="52a71-118">Service Fabric-alkalmazások szolgáltatáspéldány gyűjteménye épülnek fel.</span><span class="sxs-lookup"><span data-stu-id="52a71-118">Service Fabric applications are made up of a collection of service instances.</span></span> <span data-ttu-id="52a71-119">Is lehetséges, hogy hozzon létre egy üres alkalmazást, majd minden szolgáltatáspéldány dinamikusan, a legtöbb alkalmazás rendelkezik alapvető szolgáltatások mindig jöjjenek létre, amikor az alkalmazás létrejön.</span><span class="sxs-lookup"><span data-stu-id="52a71-119">While it is possible for you to create an empty application and then create all service instances dynamically, most applications have a set of core services that should always be created when the application is instantiated.</span></span> <span data-ttu-id="52a71-120">Ezek "alapértelmezett szolgáltatások" nevezzük.</span><span class="sxs-lookup"><span data-stu-id="52a71-120">These are referred to as "default services".</span></span> <span data-ttu-id="52a71-121">Az alkalmazás jegyzékében szögletes zárójelek között szerepel környezeti konfiguráció helyőrzőkkel vannak megadva:</span><span class="sxs-lookup"><span data-stu-id="52a71-121">They are specified in the application manifest, with placeholders for per-environment configuration included in square brackets:</span></span>

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

<span data-ttu-id="52a71-122">Az elnevezett paraméterek az alkalmazás jegyzékének paraméterek elemen belül kell megadni:</span><span class="sxs-lookup"><span data-stu-id="52a71-122">Each of the named parameters must be defined within the Parameters element of the application manifest:</span></span>

```xml
    <Parameters>
        <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="3" />
        <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
        <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
    </Parameters>
```

<span data-ttu-id="52a71-123">A DefaultValue attribútumot egy több-specifikus paraméter hiányában egy adott környezetben használandó értékét adja meg.</span><span class="sxs-lookup"><span data-stu-id="52a71-123">The DefaultValue attribute specifies the value to be used in the absence of a more-specific parameter for a given environment.</span></span>

> [!NOTE]
> <span data-ttu-id="52a71-124">Nem minden szolgáltatás példány paraméterei megfelelő környezet konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="52a71-124">Not all service instance parameters are suitable for per-environment configuration.</span></span> <span data-ttu-id="52a71-125">A fenti példában a szolgáltatás particionálási sémát LowKey és HighKey értékeit explicit módon határozzák meg a szolgáltatás minden példányának óta a partíciótartomány feladata az adatok tartományi, nem a környezetben.</span><span class="sxs-lookup"><span data-stu-id="52a71-125">In the example above, the LowKey and HighKey values for the service's partitioning scheme are explicitly defined for all instances of the service since the partition range is a function of the data domain, not the environment.</span></span>
> 
> 

### <a name="per-environment-service-configuration-settings"></a><span data-ttu-id="52a71-126">Környezet szolgáltatás konfigurációs beállításai</span><span class="sxs-lookup"><span data-stu-id="52a71-126">Per-environment service configuration settings</span></span>
<span data-ttu-id="52a71-127">A [Service Fabric-alkalmazás modell](service-fabric-application-model.md) lehetővé teszi, hogy a szolgáltatások futási időben olvasható egyéni kulcs-érték párokat tartalmazó konfigurációs csomagokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="52a71-127">The [Service Fabric application model](service-fabric-application-model.md) enables services to include configuration packages that contain custom key-value pairs that are readable at run time.</span></span> <span data-ttu-id="52a71-128">Ezek a beállítások értékeit is is lehet szerint megkülönböztetett környezet megadásával egy `ConfigOverride` az alkalmazásjegyzékben.</span><span class="sxs-lookup"><span data-stu-id="52a71-128">The values of these settings can also be differentiated by environment by specifying a `ConfigOverride` in the application manifest.</span></span>

<span data-ttu-id="52a71-129">Tegyük fel, hogy rendelkezik-e a következő beállítást Config\Settings.xml fájljában a `Stateful1` szolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="52a71-129">Suppose that you have the following setting in the Config\Settings.xml file for the `Stateful1` service:</span></span>

```xml
  <Section Name="MyConfigSection">
     <Parameter Name="MaxQueueSize" Value="25" />
  </Section>
```
<span data-ttu-id="52a71-130">Bírálja felül ezt az értéket egy adott alkalmazás-környezet párhoz, hozzon létre egy `ConfigOverride` importálásakor a szolgáltatás jegyzékben található az alkalmazás jegyzékében.</span><span class="sxs-lookup"><span data-stu-id="52a71-130">To override this value for a specific application/environment pair, create a `ConfigOverride` when you import the service manifest in the application manifest.</span></span>

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
<span data-ttu-id="52a71-131">Ennek a paraméternek, majd konfigurálhatja környezet a fentiek szerint.</span><span class="sxs-lookup"><span data-stu-id="52a71-131">This parameter can then be configured by environment as shown above.</span></span> <span data-ttu-id="52a71-132">Ehhez a Paraméterek szakaszban az alkalmazás jegyzékének deklaráló azt és környezetfüggő értékek megadása a alkalmazásparaméter-fájlokat.</span><span class="sxs-lookup"><span data-stu-id="52a71-132">You can do this by declaring it in the parameters section of the application manifest and specifying environment-specific values in the application parameter files.</span></span>

> [!NOTE]
> <span data-ttu-id="52a71-133">Szolgáltatás konfigurációs beállításait, ha nincsenek három helyen, ahol a kulcsnak az értéke beállítható: a szolgáltatás konfigurációs csomagot, az alkalmazás jegyzékében és az alkalmazás paraméterfájl.</span><span class="sxs-lookup"><span data-stu-id="52a71-133">In the case of service configuration settings, there are three places where the value of a key can be set: the service configuration package, the application manifest, and the application parameter file.</span></span> <span data-ttu-id="52a71-134">A Service Fabric mindig választhat az paraméter fájl első (ha az meg van adva), majd az alkalmazás jegyzékében, és végül a konfigurációs csomagot.</span><span class="sxs-lookup"><span data-stu-id="52a71-134">Service Fabric will always choose from the application parameter file first (if specified), then the application manifest, and finally the configuration package.</span></span>
> 
> 

### <a name="setting-and-using-environment-variables"></a><span data-ttu-id="52a71-135">És környezeti változók használatához</span><span class="sxs-lookup"><span data-stu-id="52a71-135">Setting and using environment variables</span></span> 
<span data-ttu-id="52a71-136">Adja meg, és a ServiceManifest.xml fájlt a környezeti változók értékét, és ezt felülbírálhatja a ApplicationManifest.xml fájlba / példány alapon.</span><span class="sxs-lookup"><span data-stu-id="52a71-136">You can specify and set environment variables in the ServiceManifest.xml file and then override these in the ApplicationManifest.xml file on a per instance basis.</span></span>
<span data-ttu-id="52a71-137">Az alábbi példában látható, két környezeti változókat, egy értéket állítja be, és a másik felülbírálja.</span><span class="sxs-lookup"><span data-stu-id="52a71-137">The example below shows two environment variables, one with a value set and the other is overridden.</span></span> <span data-ttu-id="52a71-138">Környezeti változók értékeinek beállításához, hogy ezek legyenek érvényben van megadva a felülbírálásokhoz config ugyanúgy használhatja alkalmazás paramétereit.</span><span class="sxs-lookup"><span data-stu-id="52a71-138">You can use application parameters to set environment variables values in the same way that these were used for config overrides.</span></span>

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
<span data-ttu-id="52a71-139">A környezeti változók a ApplicationManifest.xml felülbírálásához a kódcsomag a ServiceManifest a hivatkozik a `EnvironmentOverrides` elemet.</span><span class="sxs-lookup"><span data-stu-id="52a71-139">To override the environment variables in the ApplicationManifest.xml, reference the code package in the ServiceManifest with the `EnvironmentOverrides` element.</span></span>

```xml
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="FrontEndServicePkg" ServiceManifestVersion="1.0.0" />
    <EnvironmentOverrides CodePackageRef="MyCode">
      <EnvironmentVariable Name="MyEnvVariable" Value="mydata"/>
    </EnvironmentOverrides>
  </ServiceManifestImport>
 ``` 
 <span data-ttu-id="52a71-140">A nevesített szolgáltatáspéldány létrehozását követően érheti el a környezeti változók kódból.</span><span class="sxs-lookup"><span data-stu-id="52a71-140">Once the named service instance is created you can access the environment variables from code.</span></span> <span data-ttu-id="52a71-141">például a C# tegye a következőket</span><span class="sxs-lookup"><span data-stu-id="52a71-141">e.g. In C# you can do the following</span></span>

```csharp
    string EnvVariable = Environment.GetEnvironmentVariable("MyEnvVariable");
```

### <a name="service-fabric-environment-variables"></a><span data-ttu-id="52a71-142">A Service Fabric környezeti változók</span><span class="sxs-lookup"><span data-stu-id="52a71-142">Service Fabric environment variables</span></span>
<span data-ttu-id="52a71-143">A Service Fabric környezeti változók beállítása minden szolgáltatáspéldány létrehozta.</span><span class="sxs-lookup"><span data-stu-id="52a71-143">Service Fabric has built in environment variables set for each service instance.</span></span> <span data-ttu-id="52a71-144">A teljes listát a környezeti változók nem éri el, ahol az félkövér azok, amelyekre szüksége lesz a szolgáltatás, a másik Service Fabric-futtatókörnyezet által használt.</span><span class="sxs-lookup"><span data-stu-id="52a71-144">The full list of environment variables is below, where the ones in bold are the ones that you will use in your service, the other being used by Service Fabric runtime.</span></span> 

* <span data-ttu-id="52a71-145">Fabric_ApplicationHostId</span><span class="sxs-lookup"><span data-stu-id="52a71-145">Fabric_ApplicationHostId</span></span>
* <span data-ttu-id="52a71-146">Fabric_ApplicationHostType</span><span class="sxs-lookup"><span data-stu-id="52a71-146">Fabric_ApplicationHostType</span></span>
* <span data-ttu-id="52a71-147">Fabric_ApplicationId</span><span class="sxs-lookup"><span data-stu-id="52a71-147">Fabric_ApplicationId</span></span>
* <span data-ttu-id="52a71-148">**Fabric_ApplicationName**</span><span class="sxs-lookup"><span data-stu-id="52a71-148">**Fabric_ApplicationName**</span></span>
* <span data-ttu-id="52a71-149">Fabric_CodePackageInstanceId</span><span class="sxs-lookup"><span data-stu-id="52a71-149">Fabric_CodePackageInstanceId</span></span>
* <span data-ttu-id="52a71-150">**Fabric_CodePackageName**</span><span class="sxs-lookup"><span data-stu-id="52a71-150">**Fabric_CodePackageName**</span></span>
* <span data-ttu-id="52a71-151">**[YourServiceName] Fabric_Endpoint_ TypeEndpoint**</span><span class="sxs-lookup"><span data-stu-id="52a71-151">**Fabric_Endpoint_[YourServiceName]TypeEndpoint**</span></span>
* <span data-ttu-id="52a71-152">**Fabric_Folder_App_Log**</span><span class="sxs-lookup"><span data-stu-id="52a71-152">**Fabric_Folder_App_Log**</span></span>
* <span data-ttu-id="52a71-153">**Fabric_Folder_App_Temp**</span><span class="sxs-lookup"><span data-stu-id="52a71-153">**Fabric_Folder_App_Temp**</span></span>
* <span data-ttu-id="52a71-154">**Fabric_Folder_App_Work**</span><span class="sxs-lookup"><span data-stu-id="52a71-154">**Fabric_Folder_App_Work**</span></span>
* <span data-ttu-id="52a71-155">**Fabric_Folder_Application**</span><span class="sxs-lookup"><span data-stu-id="52a71-155">**Fabric_Folder_Application**</span></span>
* <span data-ttu-id="52a71-156">Fabric_NodeId</span><span class="sxs-lookup"><span data-stu-id="52a71-156">Fabric_NodeId</span></span>
* <span data-ttu-id="52a71-157">**Fabric_NodeIPOrFQDN**</span><span class="sxs-lookup"><span data-stu-id="52a71-157">**Fabric_NodeIPOrFQDN**</span></span>
* <span data-ttu-id="52a71-158">**Fabric_NodeName**</span><span class="sxs-lookup"><span data-stu-id="52a71-158">**Fabric_NodeName**</span></span>
* <span data-ttu-id="52a71-159">Fabric_RuntimeConnectionAddress</span><span class="sxs-lookup"><span data-stu-id="52a71-159">Fabric_RuntimeConnectionAddress</span></span>
* <span data-ttu-id="52a71-160">Fabric_ServicePackageInstanceId</span><span class="sxs-lookup"><span data-stu-id="52a71-160">Fabric_ServicePackageInstanceId</span></span>
* <span data-ttu-id="52a71-161">Fabric_ServicePackageName</span><span class="sxs-lookup"><span data-stu-id="52a71-161">Fabric_ServicePackageName</span></span>
* <span data-ttu-id="52a71-162">Fabric_ServicePackageVersionInstance</span><span class="sxs-lookup"><span data-stu-id="52a71-162">Fabric_ServicePackageVersionInstance</span></span>
* <span data-ttu-id="52a71-163">FabricPackageFileName</span><span class="sxs-lookup"><span data-stu-id="52a71-163">FabricPackageFileName</span></span>

<span data-ttu-id="52a71-164">A kód belows ismerteti a Service Fabric környezeti változók felsorolása</span><span class="sxs-lookup"><span data-stu-id="52a71-164">The code belows shows how to list the Service Fabric environment variables</span></span>
 ```csharp
    foreach (DictionaryEntry de in Environment.GetEnvironmentVariables())
    {
        if (de.Key.ToString().StartsWith("Fabric"))
        {
            Console.WriteLine(" Environment variable {0} = {1}", de.Key, de.Value);
        }
    }
```
<span data-ttu-id="52a71-165">A következő példákban a környezeti változók nevű alkalmazás típusú `GuestExe.Application` egy szolgáltatás típusának neve `FrontEndService` futása közben a helyi fejlesztési számítógépén.</span><span class="sxs-lookup"><span data-stu-id="52a71-165">The following are examples of environment variables for an application type called `GuestExe.Application` with a service type called `FrontEndService` when run on your local dev machine.</span></span>

* <span data-ttu-id="52a71-166">**Fabric_ApplicationName = fabric:/GuestExe.Application**</span><span class="sxs-lookup"><span data-stu-id="52a71-166">**Fabric_ApplicationName = fabric:/GuestExe.Application**</span></span>
* <span data-ttu-id="52a71-167">**Fabric_CodePackageName kód =**</span><span class="sxs-lookup"><span data-stu-id="52a71-167">**Fabric_CodePackageName = Code**</span></span>
* <span data-ttu-id="52a71-168">**Fabric_Endpoint_FrontEndServiceTypeEndpoint = 80**</span><span class="sxs-lookup"><span data-stu-id="52a71-168">**Fabric_Endpoint_FrontEndServiceTypeEndpoint = 80**</span></span>
* <span data-ttu-id="52a71-169">**Fabric_NodeIPOrFQDN = localhost**</span><span class="sxs-lookup"><span data-stu-id="52a71-169">**Fabric_NodeIPOrFQDN = localhost**</span></span>
* <span data-ttu-id="52a71-170">**Fabric_NodeName = _Node_2**</span><span class="sxs-lookup"><span data-stu-id="52a71-170">**Fabric_NodeName = _Node_2**</span></span>

### <a name="application-parameter-files"></a><span data-ttu-id="52a71-171">Alkalmazásparaméter-fájlokat</span><span class="sxs-lookup"><span data-stu-id="52a71-171">Application parameter files</span></span>
<span data-ttu-id="52a71-172">A Service Fabric-alkalmazás projekt tartalmazhat egy vagy több alkalmazásparaméter-fájlokat.</span><span class="sxs-lookup"><span data-stu-id="52a71-172">The Service Fabric application project can include one or more application parameter files.</span></span> <span data-ttu-id="52a71-173">Azok meghatározza, hogy az adott értékekre, az alkalmazás jegyzékében definiált paraméterek:</span><span class="sxs-lookup"><span data-stu-id="52a71-173">Each of them defines the specific values for the parameters that are defined in the application manifest:</span></span>

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
<span data-ttu-id="52a71-174">Új alkalmazás alapértelmezés szerint három alkalmazásparaméter-fájlokat, Local.1Node.xml, Local.5Node.xml és Cloud.xml nevű foglalja magában:</span><span class="sxs-lookup"><span data-stu-id="52a71-174">By default, a new application includes three application parameter files, named Local.1Node.xml, Local.5Node.xml, and Cloud.xml:</span></span>

![A Solution Explorer alkalmazásparaméter-fájlokat][app-parameters-solution-explorer]

<span data-ttu-id="52a71-176">Paraméter-fájl létrehozásához egyszerűen másolja és illessze be egy meglévő és új nevet.</span><span class="sxs-lookup"><span data-stu-id="52a71-176">To create a parameter file, simply copy and paste an existing one and give it a new name.</span></span>

## <a name="identifying-environment-specific-parameters-during-deployment"></a><span data-ttu-id="52a71-177">Azonosító környezetfüggő paraméterek központi telepítése során</span><span class="sxs-lookup"><span data-stu-id="52a71-177">Identifying environment-specific parameters during deployment</span></span>
<span data-ttu-id="52a71-178">A központi telepítéskor kell választani a megfelelő paraméterfájl alkalmazni az alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="52a71-178">At deployment time, you need to choose the appropriate parameter file to apply with your application.</span></span> <span data-ttu-id="52a71-179">Ehhez a Visual Studio Publish párbeszédpaneléről vagy a Powershellen keresztül.</span><span class="sxs-lookup"><span data-stu-id="52a71-179">You can do this through the Publish dialog in Visual Studio or through PowerShell.</span></span>

### <a name="deploy-from-visual-studio"></a><span data-ttu-id="52a71-180">Üzembe helyezés a Visual Studióból</span><span class="sxs-lookup"><span data-stu-id="52a71-180">Deploy from Visual Studio</span></span>
<span data-ttu-id="52a71-181">A rendelkezésre álló paraméter fájlok listáját választhat az alkalmazást a Visual Studio közzétételekor.</span><span class="sxs-lookup"><span data-stu-id="52a71-181">You can choose from the list of available parameter files when you publish your application in Visual Studio.</span></span>

![Válassza ki a paraméterfájl közzététele párbeszédpanelen][publishdialog]

### <a name="deploy-from-powershell"></a><span data-ttu-id="52a71-183">A PowerShell telepítése</span><span class="sxs-lookup"><span data-stu-id="52a71-183">Deploy from PowerShell</span></span>
<span data-ttu-id="52a71-184">A `Deploy-FabricApplication.ps1` PowerShell-parancsfájlt tartalmazza az alkalmazás projektsablon fogad paraméterként közzétételi profilt, és a PublishProfile az alkalmazás paraméterfájl hivatkozást tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="52a71-184">The `Deploy-FabricApplication.ps1` PowerShell script included in the application project template accepts a publish profile as a parameter and the PublishProfile contains a reference to the application parameters file.</span></span>

  ```PowerShell
    ./Deploy-FabricApplication -ApplicationPackagePath <app_package_path> -PublishProfileFile <publishprofile_path>
  ```

## <a name="next-steps"></a><span data-ttu-id="52a71-185">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="52a71-185">Next steps</span></span>
<span data-ttu-id="52a71-186">Ebben a témakörben tárgyalt alapfogalmakat némelyike kapcsolatos további tudnivalókért tekintse meg a [Service Fabric a műszaki áttekintés](service-fabric-technical-overview.md).</span><span class="sxs-lookup"><span data-stu-id="52a71-186">To learn more about some of the core concepts that are discussed in this topic, see the [Service Fabric technical overview](service-fabric-technical-overview.md).</span></span> <span data-ttu-id="52a71-187">Más elérhető a Visual Studio alkalmazás-felügyeleti képességekkel kapcsolatos információkért lásd: [kezelése a Service Fabric-alkalmazások, a Visual Studio](service-fabric-manage-application-in-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="52a71-187">For information about other app management capabilities that are available in Visual Studio, see [Manage your Service Fabric applications in Visual Studio](service-fabric-manage-application-in-visual-studio.md).</span></span>

<!-- Image references -->

[publishdialog]: ./media/service-fabric-manage-multiple-environment-app-configuration/publish-dialog-choose-app-config.png
[app-parameters-solution-explorer]:./media/service-fabric-manage-multiple-environment-app-configuration/app-parameters-in-solution-explorer.png
