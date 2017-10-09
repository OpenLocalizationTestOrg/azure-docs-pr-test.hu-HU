---
title: "a Service Fabric-alkalmazás modell aaaAzure |} Microsoft Docs"
description: "Hogyan toomodel és az alkalmazások és szolgáltatások a Service Fabric ismertetik."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: mani-ramaswamy
ms.assetid: 17a99380-5ed8-4ed9-b884-e9b827431b02
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: ryanwi
ms.openlocfilehash: 54c4d026e7d556be5f697d4a6f2ee886687e1c35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="model-an-application-in-service-fabric"></a><span data-ttu-id="51a0d-103">A Service Fabric alkalmazás minta</span><span class="sxs-lookup"><span data-stu-id="51a0d-103">Model an application in Service Fabric</span></span>
<span data-ttu-id="51a0d-104">Ez a cikk áttekintése hello Azure Service Fabric-alkalmazás modell, és hogyan toodefine egy alkalmazás és szolgáltatás keresztül manifest fájlt.</span><span class="sxs-lookup"><span data-stu-id="51a0d-104">This article provides an overview of hello Azure Service Fabric application model and how toodefine an application and service via manifest files.</span></span>

## <a name="understand-hello-application-model"></a><span data-ttu-id="51a0d-105">Hello alkalmazásmodell ismertetése</span><span class="sxs-lookup"><span data-stu-id="51a0d-105">Understand hello application model</span></span>
<span data-ttu-id="51a0d-106">Egy alkalmazás egy bizonyos funkciója vagy funkciói végző alkotó szolgáltatások gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="51a0d-106">An application is a collection of constituent services that perform a certain function or functions.</span></span> <span data-ttu-id="51a0d-107">Egy szolgáltatás teljes és a különálló funkciót hajt végre és elindíthatja és egyéb szolgáltatások függetlenül futtatni.</span><span class="sxs-lookup"><span data-stu-id="51a0d-107">A service performs a complete and standalone function and can start and run independently of other services.</span></span>  <span data-ttu-id="51a0d-108">A szolgáltatás kódot, a konfiguráció és az adatok tevődik össze.</span><span class="sxs-lookup"><span data-stu-id="51a0d-108">A service is composed of code, configuration, and data.</span></span> <span data-ttu-id="51a0d-109">Minden egyes szolgáltatás kód áll hello végrehajtható bináris fájlok konfigurálása a futási időben betölthető Szolgáltatásbeállítások áll és hello szolgáltatás által használt tetszőleges statikus adatok toobe tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="51a0d-109">For each service, code consists of hello executable binaries, configuration consists of service settings that can be loaded at run time, and data consists of arbitrary static data toobe consumed by hello service.</span></span> <span data-ttu-id="51a0d-110">A hierarchikus alkalmazás modellben minden egyes összetevő csak rendszerverzióval ellátott és frissített egymástól függetlenül.</span><span class="sxs-lookup"><span data-stu-id="51a0d-110">Each component in this hierarchical application model can be versioned and upgraded independently.</span></span>

![hello Service Fabric alkalmazásmodellt.][appmodel-diagram]

<span data-ttu-id="51a0d-112">Alkalmazástípust egy kategorizálási kérelem és a csomag egyik gyermekszoftver található szolgáltatástípusok áll.</span><span class="sxs-lookup"><span data-stu-id="51a0d-112">An application type is a categorization of an application and consists of a bundle of service types.</span></span> <span data-ttu-id="51a0d-113">A szolgáltatás típus egy kategorizálási szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="51a0d-113">A service type is a categorization of a service.</span></span> <span data-ttu-id="51a0d-114">hello kategorizálási különböző beállítások és konfigurációk rendelkezhet, de hello core funkció marad hello azonos.</span><span class="sxs-lookup"><span data-stu-id="51a0d-114">hello categorization can have different settings and configurations, but hello core functionality remains hello same.</span></span> <span data-ttu-id="51a0d-115">hello szolgáltatás példányai hello különböző konfigurációs eltéréseket hello az azonos szolgáltatás típusa.</span><span class="sxs-lookup"><span data-stu-id="51a0d-115">hello instances of a service are hello different service configuration variations of hello same service type.</span></span>  

<span data-ttu-id="51a0d-116">Osztályok (vagy "típusú") az alkalmazások és szolgáltatások leírása XML-fájlok (alkalmazás és szolgáltatás jegyzékfájlokban) keresztül.</span><span class="sxs-lookup"><span data-stu-id="51a0d-116">Classes (or "types") of applications and services are described through XML files (application manifests and service manifests).</span></span>  <span data-ttu-id="51a0d-117">hello jegyzékfájlokat hello sablonok, amely alkalmazások példányosítható hello fürt kép áruházból.</span><span class="sxs-lookup"><span data-stu-id="51a0d-117">hello manifests are hello templates against which applications can be instantiated from hello cluster's image store.</span></span> <span data-ttu-id="51a0d-118">hello hello ServiceManifest.xml és ApplicationManifest.xml fájl sémadefiníciót hello Service Fabric SDK telepítve van és eszközöket túl*C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span><span class="sxs-lookup"><span data-stu-id="51a0d-118">hello schema definition for hello ServiceManifest.xml and ApplicationManifest.xml file is installed with hello Service Fabric SDK and tools too*C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span></span>

<span data-ttu-id="51a0d-119">a Futtatás másként különálló folyamat akkor is, ha a futó hello azonos a Service Fabric-csomópont különböző alkalmazáspéldányok hello kódját.</span><span class="sxs-lookup"><span data-stu-id="51a0d-119">hello code for different application instances run as separate processes even when hosted by hello same Service Fabric node.</span></span> <span data-ttu-id="51a0d-120">Továbbá minden alkalmazáspéldány hello életciklusát kezelhető (például frissítése) egymástól függetlenül.</span><span class="sxs-lookup"><span data-stu-id="51a0d-120">Furthermore, hello lifecycle of each application instance can be managed (for example, upgraded) independently.</span></span> <span data-ttu-id="51a0d-121">hello alábbi ábra bemutatja hogyan alkalmazástípusok szolgáltatástípusok, viszont álló kódot, a konfiguráció és adatok csomagok állnak.</span><span class="sxs-lookup"><span data-stu-id="51a0d-121">hello following diagram shows how application types are composed of service types, which in turn are composed of code, configuration, and data packages.</span></span> <span data-ttu-id="51a0d-122">toosimplify hello diagram, csak hello kódot/config/adatok csomagjai `ServiceType4` láthatók, bár egyes szolgáltatástípusokról tartalmazhat néhány vagy minden csomagtípus.</span><span class="sxs-lookup"><span data-stu-id="51a0d-122">toosimplify hello diagram, only hello code/config/data packages for `ServiceType4` are shown, though each service type would include some or all those package types.</span></span>

![A Service Fabric alkalmazás és szolgáltatás típusainak][cluster-imagestore-apptypes]

<span data-ttu-id="51a0d-124">Két különböző fájlok használt toodescribe alkalmazások és szolgáltatások: hello szolgáltatás jegyzékfájl és az alkalmazásjegyzék.</span><span class="sxs-lookup"><span data-stu-id="51a0d-124">Two different manifest files are used toodescribe applications and services: hello service manifest and application manifest.</span></span> <span data-ttu-id="51a0d-125">A következő részekben hello részletes leírása az jegyzékfájljai.</span><span class="sxs-lookup"><span data-stu-id="51a0d-125">Manifests are covered in detail in hello following sections.</span></span>

<span data-ttu-id="51a0d-126">Egy vagy több példányát egy típusú szolgáltatás lehet hello fürt aktív.</span><span class="sxs-lookup"><span data-stu-id="51a0d-126">There can be one or more instances of a service type active in hello cluster.</span></span> <span data-ttu-id="51a0d-127">Például állapot-nyilvántartó szolgáltatáspéldány, vagy a replikák elérése magas megbízhatóság állapot replikálja a replikák található hello fürt csomópontjai között.</span><span class="sxs-lookup"><span data-stu-id="51a0d-127">For example, stateful service instances, or replicas, achieve high reliability by replicating state between replicas located on different nodes in hello cluster.</span></span> <span data-ttu-id="51a0d-128">Replikációs lényegében biztosít redundanciát hello szolgáltatás toobe érhető el a, még akkor is, ha a fürt egyik csomópontjáról sikertelen.</span><span class="sxs-lookup"><span data-stu-id="51a0d-128">Replication essentially provides redundancy for hello service toobe available even if one node in a cluster fails.</span></span> <span data-ttu-id="51a0d-129">A [szolgáltatás particionálva](service-fabric-concepts-partitioning.md) további felosztása a állapota (és a hozzáférési minták toothat állapot) hello fürt csomópontjai között.</span><span class="sxs-lookup"><span data-stu-id="51a0d-129">A [partitioned service](service-fabric-concepts-partitioning.md) further divides its state (and access patterns toothat state) across nodes in hello cluster.</span></span>

<span data-ttu-id="51a0d-130">hello alábbi ábrán látható alkalmazások és a szolgáltatáspéldány, a partíciók és a replikák hello kapcsolatát.</span><span class="sxs-lookup"><span data-stu-id="51a0d-130">hello following diagram shows hello relationship between applications and service instances, partitions, and replicas.</span></span>

![Partíciók és replikáit a szolgáltatáson belül][cluster-application-instances]

> [!TIP]
> <span data-ttu-id="51a0d-132">Alkalmazások hello elrendezésének megtekintheti a http:// hello Service Fabric Explorer eszközt érhető el a fürt&lt;yourclusteraddress&gt;: 19080/Explorer.</span><span class="sxs-lookup"><span data-stu-id="51a0d-132">You can view hello layout of applications in a cluster using hello Service Fabric Explorer tool available at http://&lt;yourclusteraddress&gt;:19080/Explorer.</span></span> <span data-ttu-id="51a0d-133">További információkért lásd: [a fürt megjelenítése a Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="51a0d-133">For more information, see [Visualizing your cluster with Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span></span>
> 
> 

## <a name="describe-a-service"></a><span data-ttu-id="51a0d-134">A szolgáltatás leírása</span><span class="sxs-lookup"><span data-stu-id="51a0d-134">Describe a service</span></span>
<span data-ttu-id="51a0d-135">hello szolgáltatás deklarációval jegyzékfájl hello szolgáltatás típusától és verziójától.</span><span class="sxs-lookup"><span data-stu-id="51a0d-135">hello service manifest declaratively defines hello service type and version.</span></span> <span data-ttu-id="51a0d-136">Azt adja meg a szolgáltatás metaadatokat, például szolgáltatás típusa, a rendszerállapot-tulajdonságai, a terheléselosztás metrikák, a bináris fájljait és a konfigurációs fájlok.</span><span class="sxs-lookup"><span data-stu-id="51a0d-136">It specifies service metadata such as service type, health properties, load-balancing metrics, service binaries, and configuration files.</span></span>  <span data-ttu-id="51a0d-137">Másképp fogalmazva, ismerteti, hogyan hello kódot, a konfiguráció és az adatok csomagok a service-csomag toosupport alkotó egy vagy több szolgáltatás típust.</span><span class="sxs-lookup"><span data-stu-id="51a0d-137">Put another way, it describes hello code, configuration, and data packages that compose a service package toosupport one or more service types.</span></span> <span data-ttu-id="51a0d-138">Íme egy egyszerű példa service jegyzékfájl:</span><span class="sxs-lookup"><span data-stu-id="51a0d-138">Here is a simple example service manifest:</span></span>

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

<span data-ttu-id="51a0d-139">**Verzió** attribútumok strukturálatlan karakterláncok és hello rendszer nem elemzi.</span><span class="sxs-lookup"><span data-stu-id="51a0d-139">**Version** attributes are unstructured strings and not parsed by hello system.</span></span> <span data-ttu-id="51a0d-140">Verzió attribútumok vannak használt tooversion minden összetevő frissítésre.</span><span class="sxs-lookup"><span data-stu-id="51a0d-140">Version attributes are used tooversion each component for upgrades.</span></span>

<span data-ttu-id="51a0d-141">**ServiceTypes** kijelenti, hogy milyen típusú szolgáltatásokat által támogatott **CodePackages** a jegyzékfájlban.</span><span class="sxs-lookup"><span data-stu-id="51a0d-141">**ServiceTypes** declares what service types are supported by **CodePackages** in this manifest.</span></span> <span data-ttu-id="51a0d-142">Amikor egy szolgáltatás létrejön az említett szolgáltatás ellen, a jegyzékfájlban deklarált összes kódot csomag aktívak a belépési pontok futtatásával.</span><span class="sxs-lookup"><span data-stu-id="51a0d-142">When a service is instantiated against one of these service types, all code packages declared in this manifest are activated by running their entry points.</span></span> <span data-ttu-id="51a0d-143">hello eredményül kapott folyamatok várt tooregister hello támogatott szolgáltatástípusok futási időben.</span><span class="sxs-lookup"><span data-stu-id="51a0d-143">hello resulting processes are expected tooregister hello supported service types at run time.</span></span> <span data-ttu-id="51a0d-144">Szolgáltatástípusok hello jegyzék szint és a nem az hello kód csomag szintjén deklarált.</span><span class="sxs-lookup"><span data-stu-id="51a0d-144">Service types are declared at hello manifest level and not hello code package level.</span></span> <span data-ttu-id="51a0d-145">Ezért ha több kód csomagot, az összes aktiválás amikor hello rendszer keresi szolgáltatástípusok deklarált hello egyikét sem.</span><span class="sxs-lookup"><span data-stu-id="51a0d-145">So when there are multiple code packages, they are all activated whenever hello system looks for any one of hello declared service types.</span></span>

<span data-ttu-id="51a0d-146">**SetupEntryPoint** egy jogosultsági szintű belépési pontja, amelynek ugyanaz, mint a Service Fabric hitelesítő adatok hello (általában hello *LocalSystem* fiók) más belépési pont előtt.</span><span class="sxs-lookup"><span data-stu-id="51a0d-146">**SetupEntryPoint** is a privileged entry point that runs with hello same credentials as Service Fabric (typically hello *LocalSystem* account) before any other entry point.</span></span> <span data-ttu-id="51a0d-147">hello végrehajtható fájl megadott **EntryPoint** általában hello hosszan futó szolgáltatás gazdagép legyen.</span><span class="sxs-lookup"><span data-stu-id="51a0d-147">hello executable specified by **EntryPoint** is typically hello long-running service host.</span></span> <span data-ttu-id="51a0d-148">egy külön telepítő belépési pont hello jelenléte elkerülhető toorun hello szolgáltatásgazda magas jogosultsággal rendelkező huzamosabb ideig.</span><span class="sxs-lookup"><span data-stu-id="51a0d-148">hello presence of a separate setup entry point avoids having toorun hello service host with high privileges for extended periods of time.</span></span> <span data-ttu-id="51a0d-149">hello végrehajtható fájl megadott **EntryPoint** futtatása **SetupEntryPoint** sikeresen kilép.</span><span class="sxs-lookup"><span data-stu-id="51a0d-149">hello executable specified by **EntryPoint** is run after **SetupEntryPoint** exits successfully.</span></span> <span data-ttu-id="51a0d-150">Ha bármikor hello folyamat leáll, vagy összeomlik, hello eredményül kapott folyamat ellenőrzött és újraindítása (újra kezdve **SetupEntryPoint**).</span><span class="sxs-lookup"><span data-stu-id="51a0d-150">If hello process ever terminates or crashes, hello resulting process is monitored and restarted (beginning again with **SetupEntryPoint**).</span></span>  

<span data-ttu-id="51a0d-151">A jellemző forgatókönyvek **SetupEntryPoint** Biztosan futtatni egy végrehajtható fájlt hello szolgáltatás indítása előtt, vagy egy emelt szintű jogosultságokkal műveletét úgy végezheti el.</span><span class="sxs-lookup"><span data-stu-id="51a0d-151">Typical scenarios for using **SetupEntryPoint** are when you run an executable before hello service starts or you perform an operation with elevated privileges.</span></span> <span data-ttu-id="51a0d-152">Példa:</span><span class="sxs-lookup"><span data-stu-id="51a0d-152">For example:</span></span>

* <span data-ttu-id="51a0d-153">Beállítását, valamint inicializálása környezeti változókat, amelyek hello végrehajtható van szüksége.</span><span class="sxs-lookup"><span data-stu-id="51a0d-153">Setting up and initializing environment variables that hello service executable needs.</span></span> <span data-ttu-id="51a0d-154">Ez nem korlátozott tooonly végrehajtható fájlok keresztül hello Service Fabric programozási modellek írása.</span><span class="sxs-lookup"><span data-stu-id="51a0d-154">This is not limited tooonly executables written via hello Service Fabric programming models.</span></span> <span data-ttu-id="51a0d-155">Például npm.exe kell néhány környezetiblokk-változót, egy node.js-alkalmazás telepítéséhez konfigurált.</span><span class="sxs-lookup"><span data-stu-id="51a0d-155">For example, npm.exe needs some environment variables configured for deploying a node.js application.</span></span>
* <span data-ttu-id="51a0d-156">Hozzáférés-vezérlés beállítása biztonsági tanúsítványok telepítésével.</span><span class="sxs-lookup"><span data-stu-id="51a0d-156">Setting up access control by installing security certificates.</span></span>

<span data-ttu-id="51a0d-157">További részletes információt a tooconfigure hello **SetupEntryPoint** lásd: [egy szolgáltatás telepítője a belépési pont hello házirend konfigurálása](service-fabric-application-runas-security.md)</span><span class="sxs-lookup"><span data-stu-id="51a0d-157">For more details on how tooconfigure hello **SetupEntryPoint** see [Configure hello policy for a service setup entry point](service-fabric-application-runas-security.md)</span></span>

<span data-ttu-id="51a0d-158">**Változók** környezeti változókat, amelyek be vannak állítva a kódcsomag listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="51a0d-158">**EnvironmentVariables** provides a list of environment variables that are set for this code package.</span></span> <span data-ttu-id="51a0d-159">Environmentment változók felülbírálhatók hello `ApplicationManifest.xml` különböző szolgáltatáspéldány tooprovide eltérő értékeket.</span><span class="sxs-lookup"><span data-stu-id="51a0d-159">Environmentment variables can be overridden in hello `ApplicationManifest.xml` tooprovide different values for different service instances.</span></span> 

<span data-ttu-id="51a0d-160">**DataPackage** deklarál egy által hello nevű mappát **neve** futási időben hello folyamat által használt tetszőleges statikus adatok toobe tartalmazó attribútum.</span><span class="sxs-lookup"><span data-stu-id="51a0d-160">**DataPackage** declares a folder, named by hello **Name** attribute, that contains arbitrary static data toobe consumed by hello process at run time.</span></span>

<span data-ttu-id="51a0d-161">**ConfigPackage** deklarál egy által hello nevű mappát **neve** attribútum, amely tartalmaz egy *Settings.xml* fájlt.</span><span class="sxs-lookup"><span data-stu-id="51a0d-161">**ConfigPackage** declares a folder, named by hello **Name** attribute, that contains a *Settings.xml* file.</span></span> <span data-ttu-id="51a0d-162">hello beállításfájl a felhasználó által definiált, a kulcs-érték pár beállítások hello folyamat futási időben vissza olvasó részeket tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="51a0d-162">hello settings file contains sections of user-defined, key-value pair settings that hello process reads back at run time.</span></span> <span data-ttu-id="51a0d-163">Ha csak frissítés során hello **ConfigPackage** **verzió** módosult, akkor hello folyamat fut nem indítják újra.</span><span class="sxs-lookup"><span data-stu-id="51a0d-163">During an upgrade, if only hello **ConfigPackage** **version** has changed, then hello running process is not restarted.</span></span> <span data-ttu-id="51a0d-164">Ehelyett egy visszahívási értesíti hello folyamat, amely a konfigurációs beállításai módosultak, így azok dinamikusan kell tölteni.</span><span class="sxs-lookup"><span data-stu-id="51a0d-164">Instead, a callback notifies hello process that configuration settings have changed so they can be reloaded dynamically.</span></span> <span data-ttu-id="51a0d-165">Példa *Settings.xml* fájlt:</span><span class="sxs-lookup"><span data-stu-id="51a0d-165">Here is an example *Settings.xml* file:</span></span>

```xml
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MyConfigurationSection">
    <Parameter Name="MySettingA" Value="Example1" />
    <Parameter Name="MySettingB" Value="Example2" />
  </Section>
</Settings>
```

> [!NOTE]
> <span data-ttu-id="51a0d-166">A szolgáltatás jegyzék több kódot, konfiguráció és adatok csomagok tartalmazhatnak.</span><span class="sxs-lookup"><span data-stu-id="51a0d-166">A service manifest can contain multiple code, configuration, and data packages.</span></span> <span data-ttu-id="51a0d-167">Egyes lehetnek rendszerverzióval ellátott egymástól függetlenül.</span><span class="sxs-lookup"><span data-stu-id="51a0d-167">Each of those can be versioned independently.</span></span>
> 
> 

<!--
For more information about other features supported by service manifests, refer toohello following articles:

*TODO: LoadMetrics, PlacementConstraints, ServicePlacementPolicies
*TODO: Resources
*TODO: Health properties
*TODO: Trace filters
*TODO: Configuration overrides
-->

## <a name="describe-an-application"></a><span data-ttu-id="51a0d-168">Az alkalmazás leírása</span><span class="sxs-lookup"><span data-stu-id="51a0d-168">Describe an application</span></span>
<span data-ttu-id="51a0d-169">hello alkalmazásjegyzék deklarációval ismerteti hello alkalmazástípus és -verzió.</span><span class="sxs-lookup"><span data-stu-id="51a0d-169">hello application manifest declaratively describes hello application type and version.</span></span> <span data-ttu-id="51a0d-170">Azt adja meg a szolgáltatás összeállítás metaadatai köztük, példányok száma vagy replikációs tényező, biztonsági/elkülönítési házirenddel, placement Constraints korlátozásokat, konfigurációs felülbírálások, és a bennük foglalt szolgáltatástípusok stabil nevét.</span><span class="sxs-lookup"><span data-stu-id="51a0d-170">It specifies service composition metadata such as stable names, partitioning scheme, instance count/replication factor, security/isolation policy, placement constraints, configuration overrides, and constituent service types.</span></span> <span data-ttu-id="51a0d-171">hello terheléselosztás tartományok amelybe hello alkalmazás el van helyezve is ismerteti.</span><span class="sxs-lookup"><span data-stu-id="51a0d-171">hello load-balancing domains into which hello application is placed are also described.</span></span>

<span data-ttu-id="51a0d-172">Ebből kifolyólag alkalmazásjegyzéket hello alkalmazás szinten elemeket ismerteti és hivatkozik egy vagy több szolgáltatás jelentkezik toocompose alkalmazástípust.</span><span class="sxs-lookup"><span data-stu-id="51a0d-172">Thus, an application manifest describes elements at hello application level and references one or more service manifests toocompose an application type.</span></span> <span data-ttu-id="51a0d-173">Íme egy egyszerű példa alkalmazásjegyzék:</span><span class="sxs-lookup"><span data-stu-id="51a0d-173">Here is a simple example application manifest:</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ApplicationManifest
      ApplicationTypeName="MyApplicationType"
      ApplicationTypeVersion="AppManifestVersion1"
      xmlns="http://schemas.microsoft.com/2011/01/fabric"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example application manifest</Description>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyServiceManifest" ServiceManifestVersion="SvcManifestVersion1"/>
    <ConfigOverrides/>
    <EnvironmentOverrides CodePackageRef="MyCode"/>
  </ServiceManifestImport>
  <DefaultServices>
     <Service Name="MyService">
         <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
             <SingletonPartition/>
         </StatelessService>
     </Service>
  </DefaultServices>
</ApplicationManifest>
```

<span data-ttu-id="51a0d-174">Szolgáltatás jegyzékfájlban, például **verzió** attribútumok strukturálatlan karakterláncok, és nem elemzésének hello rendszer.</span><span class="sxs-lookup"><span data-stu-id="51a0d-174">Like service manifests, **Version** attributes are unstructured strings and are not parsed by hello system.</span></span> <span data-ttu-id="51a0d-175">Verzió attribútumok minden összetevő frissítésre használt tooversion is vannak.</span><span class="sxs-lookup"><span data-stu-id="51a0d-175">Version attributes are also used tooversion each component for upgrades.</span></span>

<span data-ttu-id="51a0d-176">**ServiceManifestImport** hivatkozások tooservice jegyzékfájlokban, ez az alkalmazástípus alkotó tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="51a0d-176">**ServiceManifestImport** contains references tooservice manifests that compose this application type.</span></span> <span data-ttu-id="51a0d-177">Importált service jegyzékfájlokban meghatározására, hogy milyen típusú szolgáltatásokat érvényes ez az alkalmazástípus belül.</span><span class="sxs-lookup"><span data-stu-id="51a0d-177">Imported service manifests determine what service types are valid within this application type.</span></span> <span data-ttu-id="51a0d-178">Belül hello ServiceManifestImport bírálja felül konfigurációs ServiceManifest.xml fájlok Settings.xml és környezeti változók értékeit.</span><span class="sxs-lookup"><span data-stu-id="51a0d-178">Within hello ServiceManifestImport, you override configuration values in Settings.xml and environment variables in ServiceManifest.xml files.</span></span> 


<span data-ttu-id="51a0d-179">**DefaultServices** szolgáltatáspéldány, amikor egy alkalmazás létrejön az alkalmazás típusa alapján automatikusan létrehozott deklarál.</span><span class="sxs-lookup"><span data-stu-id="51a0d-179">**DefaultServices** declares service instances that are automatically created whenever an application is instantiated against this application type.</span></span> <span data-ttu-id="51a0d-180">Alapértelmezett szolgáltatások csupán a könnyebb elérhetőség érdekében, és minden tekintetben normál szolgáltatások viselkednek, azok létrehozását követően.</span><span class="sxs-lookup"><span data-stu-id="51a0d-180">Default services are just a convenience and behave like normal services in every respect after they have been created.</span></span> <span data-ttu-id="51a0d-181">Azok a hello alkalmazáspéldány bármely más szolgáltatásokkal együtt verzióra frissíti, és is távolíthatók el.</span><span class="sxs-lookup"><span data-stu-id="51a0d-181">They are upgraded along with any other services in hello application instance and can be removed as well.</span></span>

> [!NOTE]
> <span data-ttu-id="51a0d-182">Az alkalmazás jegyzékében több szolgáltatás nyilvánvaló imports és alapértelmezett szolgáltatások tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="51a0d-182">An application manifest can contain multiple service manifest imports and default services.</span></span> <span data-ttu-id="51a0d-183">Minden egyes importált jegyzékből szolgáltatás egymástól függetlenül lehet rendszerverzióval ellátott.</span><span class="sxs-lookup"><span data-stu-id="51a0d-183">Each service manifest import can be versioned independently.</span></span>
> 
> 

<span data-ttu-id="51a0d-184">Hogyan toomaintain különböző alkalmazás és szolgáltatás paraméterei egyes környezetekben: toolearn [alkalmazás paramétereinek több környezet kezelése](service-fabric-manage-multiple-environment-app-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="51a0d-184">toolearn how toomaintain different application and service parameters for individual environments, see [Managing application parameters for multiple environments](service-fabric-manage-multiple-environment-app-configuration.md).</span></span>

<!--
For more information about other features supported by application manifests, refer toohello following articles:

*TODO: Application parameters
*TODO: Security, Principals, RunAs, SecurityAccessPolicy
*TODO: Service Templates
-->



## <a name="next-steps"></a><span data-ttu-id="51a0d-185">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="51a0d-185">Next steps</span></span>
<span data-ttu-id="51a0d-186">[Egy alkalmazás becsomagolása](service-fabric-package-apps.md) , és teszi toodeploy kész.</span><span class="sxs-lookup"><span data-stu-id="51a0d-186">[Package an application](service-fabric-package-apps.md) and make it ready toodeploy.</span></span>

<span data-ttu-id="51a0d-187">[Központi telepítése és távolíthat el alkalmazásokat] [ 10] ismerteti, hogyan toouse PowerShell toomanage alkalmazáspéldányok.</span><span class="sxs-lookup"><span data-stu-id="51a0d-187">[Deploy and remove applications][10] describes how toouse PowerShell toomanage application instances.</span></span>

<span data-ttu-id="51a0d-188">[Alkalmazás paramétereinek több környezet kezelése] [ 11] ismerteti, hogyan tooconfigure paraméterek és a környezeti változók különböző alkalmazás-példányok.</span><span class="sxs-lookup"><span data-stu-id="51a0d-188">[Managing application parameters for multiple environments][11] describes how tooconfigure parameters and environment variables for different application instances.</span></span>

<span data-ttu-id="51a0d-189">[Az alkalmazás biztonsági szabályzatainak konfigurálásához] [ 12] ismerteti, hogyan toorun a biztonsági házirendek toorestrict hozzáférés szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="51a0d-189">[Configure security policies for your application][12] describes how toorun services under security policies toorestrict access.</span></span>

<span data-ttu-id="51a0d-190">[Modellek futtató alkalmazás] [ 13] írják le a replikák (vagy példányok) telepített szolgáltatások és folyamata közötti kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="51a0d-190">[Application hosting models][13] describe relationship between replicas (or instances) of a deployed service and service-host process.</span></span>

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png

<!--Link references--In actual articles, you only need a single period before hello slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md
[13]: service-fabric-hosting-model.md
