---
title: "Azure Cloud Services alkalmazások toomicroservices aaaConvert |} Microsoft Docs"
description: "Ez az útmutató összehasonlítja a Cloud Services Web, és feldolgozói szerepkörök és a Service Fabric állapotmentes szolgáltatások toohelp Felhőszolgáltatások tooService háló át."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 5880ebb3-8b54-4be8-af4b-95a1bc082603
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: c43b11623b2ba7f6069782a8b7e030c82572a6e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="guide-tooconverting-web-and-worker-roles-tooservice-fabric-stateless-services"></a><span data-ttu-id="b144f-103">Útmutató tooconverting webes és feldolgozói szerepkörök tooService háló állapotmentes szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b144f-103">Guide tooconverting Web and Worker Roles tooService Fabric stateless services</span></span>
<span data-ttu-id="b144f-104">Ez a cikk ismerteti, hogyan toomigrate a felhőalapú szolgáltatások webes és feldolgozói szerepkörök tooService háló állapotmentes szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="b144f-104">This article describes how toomigrate your Cloud Services Web and Worker Roles tooService Fabric stateless services.</span></span> <span data-ttu-id="b144f-105">Ez az hello legegyszerűbb áttelepítési út a Felhőszolgáltatások tooService háló, az alkalmazások, amelyek általános architektúrája nagyjából érintetlen toostay hello azonos.</span><span class="sxs-lookup"><span data-stu-id="b144f-105">This is hello simplest migration path from Cloud Services tooService Fabric for applications whose overall architecture is going toostay roughly hello same.</span></span>

## <a name="cloud-service-project-tooservice-fabric-application-project"></a><span data-ttu-id="b144f-106">Cloud Service projekt tooService háló projekt</span><span class="sxs-lookup"><span data-stu-id="b144f-106">Cloud Service project tooService Fabric application project</span></span>
 <span data-ttu-id="b144f-107">Egy Felhőszolgáltatás-projektet és a Service Fabric-alkalmazás projekt rendelkezik egy hasonló szerkezet és mindkét jelentik hello telepítési egység az alkalmazás - Ez azt jelenti, hogy ezek határozzák meg, amely telepített toorun hello-teljes csomag az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b144f-107">A Cloud Service project and a Service Fabric Application project have a similar structure and both represent hello deployment unit for your application - that is, they each define hello complete package that is deployed toorun your application.</span></span> <span data-ttu-id="b144f-108">Egy Felhőszolgáltatás-projekt tartalmazza a legalább egy webes vagy feldolgozói szerepköröket.</span><span class="sxs-lookup"><span data-stu-id="b144f-108">A Cloud Service project contains one or more Web or Worker Roles.</span></span> <span data-ttu-id="b144f-109">Hasonlóképpen a Service Fabric-alkalmazás projekt tartalmaz egy vagy több szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b144f-109">Similarly, a Service Fabric Application project contains one or more services.</span></span> 

<span data-ttu-id="b144f-110">hello különbség az, hogy a Felhőszolgáltatás-projekt párokat hello hello alkalmazás központi telepítése egy virtuális gép üzembe helyezéséhez, és így tartalmazza a virtuális gép konfigurációs beállításai, mivel a Service Fabric-alkalmazás projekt hello csak határozza meg a központilag telepített alkalmazás a Service Fabric-fürt a meglévő virtuális gépek tooa készlete.</span><span class="sxs-lookup"><span data-stu-id="b144f-110">hello difference is that hello Cloud Service project couples hello application deployment with a VM deployment and thus contains VM configuration settings in it, whereas hello Service Fabric Application project only defines an application that will be deployed tooa set of existing VMs in a Service Fabric cluster.</span></span> <span data-ttu-id="b144f-111">hello Service Fabric-fürt maga csak van telepítve, amennyiben a Resource Manager-sablon vagy hello Azure-portálon, és az alkalmazások lehetnek több Service Fabric telepített tooit.</span><span class="sxs-lookup"><span data-stu-id="b144f-111">hello Service Fabric cluster itself is only deployed once, either through an Resource Manager template or through hello Azure portal, and multiple Service Fabric applications can be deployed tooit.</span></span>

![A Service Fabric és a Felhőszolgáltatások projekt összehasonlítása][3]

## <a name="worker-role-toostateless-service"></a><span data-ttu-id="b144f-113">Feldolgozói szerepkör toostateless szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="b144f-113">Worker Role toostateless service</span></span>
<span data-ttu-id="b144f-114">Elméleti szinten a feldolgozói szerepkör jelöli egy állapot nélküli alkalmazások és szolgáltatások, azaz hello munkaterhelés minden példánya azonos és a kérelmeket a irányított tooany példány bármikor lehet.</span><span class="sxs-lookup"><span data-stu-id="b144f-114">Conceptually, a Worker Role represents a stateless workload, meaning every instance of hello workload is identical and requests can be routed tooany instance at any time.</span></span> <span data-ttu-id="b144f-115">Minden példány várt tooremember hello előző kérést nem.</span><span class="sxs-lookup"><span data-stu-id="b144f-115">Each instance is not expected tooremember hello previous request.</span></span> <span data-ttu-id="b144f-116">Állapot: hello munkaterhelés működik, a külső állapottárolóhoz, például az Azure Table Storage vagy Azure Document DB rendszerbe kezeli.</span><span class="sxs-lookup"><span data-stu-id="b144f-116">State that hello workload operates on is managed by an external state store, such as Azure Table Storage or Azure Document DB.</span></span> <span data-ttu-id="b144f-117">A Service Fabric alkalmazások és szolgáltatások az ilyen típusú állapotmentes szolgáltatások helyőrző jelzi.</span><span class="sxs-lookup"><span data-stu-id="b144f-117">In Service Fabric, this type of workload is represented by a Stateless Service.</span></span> <span data-ttu-id="b144f-118">hello legegyszerűbb módszer toomigrating a feldolgozói szerepkör tooService háló feldolgozói szerepkör kódját tooa állapotmentes szolgáltatások átalakításával végezhető.</span><span class="sxs-lookup"><span data-stu-id="b144f-118">hello simplest approach toomigrating a Worker Role tooService Fabric can be done by converting Worker Role code tooa Stateless Service.</span></span>

![Feldolgozói szerepkör tooStateless szolgáltatás][4]

## <a name="web-role-toostateless-service"></a><span data-ttu-id="b144f-120">Webes szerepkör toostateless szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="b144f-120">Web Role toostateless service</span></span>
<span data-ttu-id="b144f-121">Hasonló tooWorker szerepkör, a webes szerepkör is képviseli egy állapot nélküli alkalmazások és szolgáltatások, és így fogalmilag túl lehet csatlakoztatott tooa Service Fabric állapotmentes szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="b144f-121">Similar tooWorker Role, a Web Role also represents a stateless workload, and so conceptually it too can be mapped tooa Service Fabric stateless service.</span></span> <span data-ttu-id="b144f-122">Azonban eltérően webes szerepkörök a Service Fabric nem támogatja az IIS.</span><span class="sxs-lookup"><span data-stu-id="b144f-122">However, unlike Web Roles, Service Fabric does not support IIS.</span></span> <span data-ttu-id="b144f-123">toomigrate egy webes szerepkör tooa állapotmentes szolgáltatások a webalkalmazás első áthelyezése tooa webes keretrendszer, amely önállóan üzemel, és nem függ az IIS vagy System.Web, például az ASP.NET Core 1 igényel.</span><span class="sxs-lookup"><span data-stu-id="b144f-123">toomigrate a web application from a Web Role tooa stateless service requires first moving tooa web framework that can be self-hosted and does not depend on IIS or System.Web, such as ASP.NET Core 1.</span></span>

| <span data-ttu-id="b144f-124">**Alkalmazás**</span><span class="sxs-lookup"><span data-stu-id="b144f-124">**Application**</span></span> | <span data-ttu-id="b144f-125">**Támogatott**</span><span class="sxs-lookup"><span data-stu-id="b144f-125">**Supported**</span></span> | <span data-ttu-id="b144f-126">**Áttelepítési útvonal**</span><span class="sxs-lookup"><span data-stu-id="b144f-126">**Migration path**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b144f-127">Az ASP.NET Web Forms keretrendszerre</span><span class="sxs-lookup"><span data-stu-id="b144f-127">ASP.NET Web Forms</span></span> |<span data-ttu-id="b144f-128">Nem</span><span class="sxs-lookup"><span data-stu-id="b144f-128">No</span></span> |<span data-ttu-id="b144f-129">Átalakítás tooASP.NET alapvető 1 MVC</span><span class="sxs-lookup"><span data-stu-id="b144f-129">Convert tooASP.NET Core 1 MVC</span></span> |
| <span data-ttu-id="b144f-130">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="b144f-130">ASP.NET MVC</span></span> |<span data-ttu-id="b144f-131">Az áttelepítés</span><span class="sxs-lookup"><span data-stu-id="b144f-131">With Migration</span></span> |<span data-ttu-id="b144f-132">Frissítési tooASP.NET alapvető 1 MVC</span><span class="sxs-lookup"><span data-stu-id="b144f-132">Upgrade tooASP.NET Core 1 MVC</span></span> |
| <span data-ttu-id="b144f-133">ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="b144f-133">ASP.NET Web API</span></span> |<span data-ttu-id="b144f-134">Az áttelepítés</span><span class="sxs-lookup"><span data-stu-id="b144f-134">With Migration</span></span> |<span data-ttu-id="b144f-135">Üzemeltetett önálló kiszolgáló vagy az ASP.NET Core 1</span><span class="sxs-lookup"><span data-stu-id="b144f-135">Use self-hosted server or ASP.NET Core 1</span></span> |
| <span data-ttu-id="b144f-136">Az ASP.NET Core 1</span><span class="sxs-lookup"><span data-stu-id="b144f-136">ASP.NET Core 1</span></span> |<span data-ttu-id="b144f-137">Igen</span><span class="sxs-lookup"><span data-stu-id="b144f-137">Yes</span></span> |<span data-ttu-id="b144f-138">N/A</span><span class="sxs-lookup"><span data-stu-id="b144f-138">N/A</span></span> |

## <a name="entry-point-api-and-lifecycle"></a><span data-ttu-id="b144f-139">Belépési pont API és életciklusa</span><span class="sxs-lookup"><span data-stu-id="b144f-139">Entry point API and lifecycle</span></span>
<span data-ttu-id="b144f-140">Feldolgozói szerepkör és a Service Fabric szolgáltatás az API-k ajánlat hasonló belépési pontok:</span><span class="sxs-lookup"><span data-stu-id="b144f-140">Worker Role and Service Fabric service APIs offer similar entry points:</span></span> 

| <span data-ttu-id="b144f-141">**A belépési pont**</span><span class="sxs-lookup"><span data-stu-id="b144f-141">**Entry Point**</span></span> | <span data-ttu-id="b144f-142">**Feldolgozói szerepkör**</span><span class="sxs-lookup"><span data-stu-id="b144f-142">**Worker Role**</span></span> | <span data-ttu-id="b144f-143">**Service Fabric-szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="b144f-143">**Service Fabric service**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b144f-144">Feldolgozása</span><span class="sxs-lookup"><span data-stu-id="b144f-144">Processing</span></span> |`Run()` |`RunAsync()` |
| <span data-ttu-id="b144f-145">Virtuális gép indítása</span><span class="sxs-lookup"><span data-stu-id="b144f-145">VM start</span></span> |`OnStart()` |<span data-ttu-id="b144f-146">N/A</span><span class="sxs-lookup"><span data-stu-id="b144f-146">N/A</span></span> |
| <span data-ttu-id="b144f-147">Virtuális gép leállítása</span><span class="sxs-lookup"><span data-stu-id="b144f-147">VM stop</span></span> |`OnStop()` |<span data-ttu-id="b144f-148">N/A</span><span class="sxs-lookup"><span data-stu-id="b144f-148">N/A</span></span> |
| <span data-ttu-id="b144f-149">Nyissa meg figyelő az ügyféli kérelmek részére</span><span class="sxs-lookup"><span data-stu-id="b144f-149">Open listener for client requests</span></span> |<span data-ttu-id="b144f-150">N/A</span><span class="sxs-lookup"><span data-stu-id="b144f-150">N/A</span></span> |<ul><li> <span data-ttu-id="b144f-151">`CreateServiceInstanceListener()`az állapot nélküli</span><span class="sxs-lookup"><span data-stu-id="b144f-151">`CreateServiceInstanceListener()` for stateless</span></span></li><li><span data-ttu-id="b144f-152">`CreateServiceReplicaListener()`az állapotalapú alkalmazások és szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b144f-152">`CreateServiceReplicaListener()` for stateful</span></span></li></ul> |

### <a name="worker-role"></a><span data-ttu-id="b144f-153">Feldolgozói szerepkör</span><span class="sxs-lookup"><span data-stu-id="b144f-153">Worker Role</span></span>
```C#

using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override void Run()
        {
        }

        public override bool OnStart()
        {
        }

        public override void OnStop()
        {
        }
    }
}

```

### <a name="service-fabric-stateless-service"></a><span data-ttu-id="b144f-154">Service Fabric állapotmentes szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b144f-154">Service Fabric Stateless Service</span></span>
```C#

using System.Collections.Generic;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

namespace Stateless1
{
    public class Stateless1 : StatelessService
    {
        protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
        {
        }

        protected override Task RunAsync(CancellationToken cancelServiceInstance)
        {
        }
    }
}

```

<span data-ttu-id="b144f-155">A mely toobegin feldolgozása egy elsődleges "Futtatás" felülbírálása egyaránt rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b144f-155">Both have a primary "Run" override in which toobegin processing.</span></span> <span data-ttu-id="b144f-156">A Service Fabric szolgáltatások egyesítése `Run`, `Start`, és `Stop` egy-egy belépési pont, a `RunAsync`.</span><span class="sxs-lookup"><span data-stu-id="b144f-156">Service Fabric services  combine `Run`, `Start`, and `Stop` into a single entry point, `RunAsync`.</span></span> <span data-ttu-id="b144f-157">A szolgáltatás működik, ha használatba `RunAsync` elindul, és le kell állnia, ha hello használata `RunAsync` metódus CancellationToken leállítási jelzést kapott.</span><span class="sxs-lookup"><span data-stu-id="b144f-157">Your service should begin working when `RunAsync` starts, and should stop working when hello `RunAsync` method's CancellationToken is signaled.</span></span> 

<span data-ttu-id="b144f-158">Hello életciklusának és feldolgozói szerepkörök és a Service Fabric szolgáltatás élettartama között számos fontos különbség van:</span><span class="sxs-lookup"><span data-stu-id="b144f-158">There are several key differences between hello lifecycle and lifetime of Worker Roles and Service Fabric services:</span></span>

* <span data-ttu-id="b144f-159">**Életciklus:** hello legnagyobb különbség az, hogy a feldolgozói szerepkör egy virtuális Gépet és annak életciklusa kapcsolt toohello virtuális gép, amely amikor hello VM indítása és leállítása eseményeit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b144f-159">**Lifecycle:** hello biggest difference is that a Worker Role is a VM and so its lifecycle is tied toohello VM, which includes events for when hello VM starts and stops.</span></span> <span data-ttu-id="b144f-160">A Service Fabric-szolgáltatás egy életciklussal, amely nem hello VM életciklusát, ezért nem tartalmazza a eseményeket amikor hello gazdagép, VM vagy a gép elindulása és a Leállítás, mert nem áll(nak) kapcsolatban van.</span><span class="sxs-lookup"><span data-stu-id="b144f-160">A Service Fabric service has a lifecycle that is separate from hello VM lifecycle, so it does not include events for when hello host VM or machine starts and stop, as they are not related.</span></span>
* <span data-ttu-id="b144f-161">**Élettartam:** A feldolgozói szerepkör példánya indul, ha hello `Run` metódus kilép.</span><span class="sxs-lookup"><span data-stu-id="b144f-161">**Lifetime:** A Worker Role instance will recycle if hello `Run` method exits.</span></span> <span data-ttu-id="b144f-162">Hello `RunAsync` metódus a Service Fabric-szolgáltatás azonban futtatható toocompletion és hello szolgáltatáspéldány fel maradnak.</span><span class="sxs-lookup"><span data-stu-id="b144f-162">hello `RunAsync` method in a Service Fabric service however can run toocompletion and hello service instance will stay up.</span></span> 

<span data-ttu-id="b144f-163">A Service Fabric egy választható kommunikációs telepítő belépési pontot nyújt az ügyfélkérelmek szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="b144f-163">Service Fabric provides an optional communication setup entry point for services that listen for client requests.</span></span> <span data-ttu-id="b144f-164">A hello RunAsync, és a kommunikációs belépési pont is választható felülbírálások szolgáltatásban a Service Fabric - a szolgáltatás előfordulhat, hogy válasszon tooonly figyelési tooclient kérelmeket, vagy csak futtassa egy feldolgozási ciklus, vagy mindkettőt - ezért hello RunAsync metódusában engedélyezett tooexit nélkül újraindítása hello szolgáltatáspéldány, mert az ügyféli kérelmek részére toolisten továbbra is.</span><span class="sxs-lookup"><span data-stu-id="b144f-164">Both hello RunAsync and communication entry point are optional overrides in Service Fabric services - your service may choose tooonly listen tooclient requests, or only run a processing loop, or both - which is why hello RunAsync method is allowed tooexit without restarting hello service instance, because it may continue toolisten for client requests.</span></span>

## <a name="application-api-and-environment"></a><span data-ttu-id="b144f-165">Alkalmazás API és a környezet</span><span class="sxs-lookup"><span data-stu-id="b144f-165">Application API and environment</span></span>
<span data-ttu-id="b144f-166">hello Felhőszolgáltatások környezet API információkat és a hello aktuális Virtuálisgép-példány és a többi VM szerepkörpéldányokat kapcsolatos információkat biztosít a.</span><span class="sxs-lookup"><span data-stu-id="b144f-166">hello Cloud Services environment API provides information and functionality for hello current VM instance as well as information about other VM role instances.</span></span> <span data-ttu-id="b144f-167">A Service Fabric tooits futásidejű vonatkozó információkat, és jelenleg fut egy szolgáltatás hello csomópont kapcsolatos információkat biztosít.</span><span class="sxs-lookup"><span data-stu-id="b144f-167">Service Fabric provides information related tooits runtime and some information about hello node a service is currently running on.</span></span> 

| <span data-ttu-id="b144f-168">**Környezet feladat**</span><span class="sxs-lookup"><span data-stu-id="b144f-168">**Environment Task**</span></span> | <span data-ttu-id="b144f-169">**Felhőszolgáltatások**</span><span class="sxs-lookup"><span data-stu-id="b144f-169">**Cloud Services**</span></span> | <span data-ttu-id="b144f-170">**Service Fabric**</span><span class="sxs-lookup"><span data-stu-id="b144f-170">**Service Fabric**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b144f-171">A konfigurációs beállítások és módosítási értesítés</span><span class="sxs-lookup"><span data-stu-id="b144f-171">Configuration Settings and change notification</span></span> |`RoleEnvironment` |`CodePackageActivationContext` |
| <span data-ttu-id="b144f-172">Helyi tároló</span><span class="sxs-lookup"><span data-stu-id="b144f-172">Local Storage</span></span> |`RoleEnvironment` |`CodePackageActivationContext` |
| <span data-ttu-id="b144f-173">Végpont</span><span class="sxs-lookup"><span data-stu-id="b144f-173">Endpoint Information</span></span> |`RoleInstance` <ul><li><span data-ttu-id="b144f-174">Jelenlegi példány:`RoleEnvironment.CurrentRoleInstance`</span><span class="sxs-lookup"><span data-stu-id="b144f-174">Current instance: `RoleEnvironment.CurrentRoleInstance`</span></span></li><li><span data-ttu-id="b144f-175">Egyéb szerepköröket és példány:`RoleEnvironment.Roles`</span><span class="sxs-lookup"><span data-stu-id="b144f-175">Other roles and instance: `RoleEnvironment.Roles`</span></span></li> |<ul><li><span data-ttu-id="b144f-176">`NodeContext`az aktuális csomópont-címe</span><span class="sxs-lookup"><span data-stu-id="b144f-176">`NodeContext` for current Node address</span></span></li><li><span data-ttu-id="b144f-177">`FabricClient`és `ServicePartitionResolver` a szolgáltatási végpont felderítése</span><span class="sxs-lookup"><span data-stu-id="b144f-177">`FabricClient` and `ServicePartitionResolver` for service endpoint discovery</span></span></li> |
| <span data-ttu-id="b144f-178">Az emuláció környezet</span><span class="sxs-lookup"><span data-stu-id="b144f-178">Environment Emulation</span></span> |`RoleEnvironment.IsEmulated` |<span data-ttu-id="b144f-179">N/A</span><span class="sxs-lookup"><span data-stu-id="b144f-179">N/A</span></span> |
| <span data-ttu-id="b144f-180">Egyidejű esemény</span><span class="sxs-lookup"><span data-stu-id="b144f-180">Simultaneous change event</span></span> |`RoleEnvironment` |<span data-ttu-id="b144f-181">N/A</span><span class="sxs-lookup"><span data-stu-id="b144f-181">N/A</span></span> |

## <a name="configuration-settings"></a><span data-ttu-id="b144f-182">Konfigurációs beállítások</span><span class="sxs-lookup"><span data-stu-id="b144f-182">Configuration settings</span></span>
<span data-ttu-id="b144f-183">Konfigurációs beállításai a Felhőszolgáltatások Virtuálisgép-szerepkör állítja be, és alkalmazni az adott virtuális gép szerepkör tooall példányok.</span><span class="sxs-lookup"><span data-stu-id="b144f-183">Configuration settings in Cloud Services are set for a VM role and apply tooall instances of that VM role.</span></span> <span data-ttu-id="b144f-184">Ezek a beállítások olyan ServiceConfiguration.*.cscfg fájlok beállított kulcs-érték párok, és segítségével érhető el közvetlenül roleenvironment-et.</span><span class="sxs-lookup"><span data-stu-id="b144f-184">These settings are key-value pairs set in ServiceConfiguration.*.cscfg files and can be accessed directly through RoleEnvironment.</span></span> <span data-ttu-id="b144f-185">A Service Fabric beállítások érvényesek külön-külön tooeach szolgáltatás és alkalmazás tooeach ahelyett, hogy tooa virtuális gép, mert egy virtuális Gépet, rendelkezhet több szolgáltatásokat és alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="b144f-185">In Service Fabric, settings apply individually tooeach service and tooeach application, rather than tooa VM, because a VM can host multiple services and applications.</span></span> <span data-ttu-id="b144f-186">A szolgáltatás három csomagok áll:</span><span class="sxs-lookup"><span data-stu-id="b144f-186">A service is composed of three packages:</span></span>

* <span data-ttu-id="b144f-187">**Kód:** hello szolgáltatás végrehajtható fájlok, bináris fájljai, dll-EK és egyéb fájlokat tartalmaz egy szolgáltatásnak kell toorun.</span><span class="sxs-lookup"><span data-stu-id="b144f-187">**Code:** contains hello service executables, binaries, DLLs, and any other files a service needs toorun.</span></span>
* <span data-ttu-id="b144f-188">**A Config:** összes konfigurációs fájlokat és a szolgáltatás beállításait.</span><span class="sxs-lookup"><span data-stu-id="b144f-188">**Config:** all configuration files and settings for a service.</span></span>
* <span data-ttu-id="b144f-189">**Adatok:** hello szolgáltatás társított statikus adatok fájlt.</span><span class="sxs-lookup"><span data-stu-id="b144f-189">**Data:** static data files associated with hello service.</span></span>

<span data-ttu-id="b144f-190">Ezeket a csomagokat mindegyikének lehet függetlenül rendszerverzióval ellátott és frissített.</span><span class="sxs-lookup"><span data-stu-id="b144f-190">Each of these packages can be independently versioned and upgraded.</span></span> <span data-ttu-id="b144f-191">Hasonló tooCloud szolgáltatások, a konfigurációs csomag programozott módon is elérhetők az API-n keresztül, és események elérhető toonotify hello szolgáltatást a konfigurációs csomag megváltoztatása.</span><span class="sxs-lookup"><span data-stu-id="b144f-191">Similar tooCloud Services, a config package can be accessed programmatically through an API and events are available toonotify hello service of a config package change.</span></span> <span data-ttu-id="b144f-192">A Settings.xml fájlban kulcs-érték mind programozott hozzáférés hasonló toohello alkalmazás beállításai szakaszában az App.config fájl használható.</span><span class="sxs-lookup"><span data-stu-id="b144f-192">A Settings.xml file can be used for key-value configuration and programmatic access similar toohello app settings section of an App.config file.</span></span> <span data-ttu-id="b144f-193">Azonban eltérően Felhőszolgáltatásokat, a Service Fabric config csomag bármely konfigurációs fájlokat tartalmazza tetszőleges méretű XML, JSON, YAM vagy egyéni bináris formátumot.</span><span class="sxs-lookup"><span data-stu-id="b144f-193">However, unlike Cloud Services, a Service Fabric config package can contain any configuration files in any format, whether it's XML, JSON, YAML, or a custom binary format.</span></span> 

### <a name="accessing-configuration"></a><span data-ttu-id="b144f-194">Konfigurációs elérése</span><span class="sxs-lookup"><span data-stu-id="b144f-194">Accessing configuration</span></span>
#### <a name="cloud-services"></a><span data-ttu-id="b144f-195">Cloud Services</span><span class="sxs-lookup"><span data-stu-id="b144f-195">Cloud Services</span></span>
<span data-ttu-id="b144f-196">Konfigurációs beállítások ServiceConfiguration.*.cscfg keresztül elérhető `RoleEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="b144f-196">Configuration settings from ServiceConfiguration.*.cscfg can be accessed through `RoleEnvironment`.</span></span> <span data-ttu-id="b144f-197">Ezek a beállítások esetén világszerte elérhető tooall szerepkörpéldányt hello azonos Cloud Service-környezetben.</span><span class="sxs-lookup"><span data-stu-id="b144f-197">These settings are globally available tooall role instances in hello same Cloud Service deployment.</span></span>

```C#

string value = RoleEnvironment.GetConfigurationSettingValue("Key");

```

#### <a name="service-fabric"></a><span data-ttu-id="b144f-198">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="b144f-198">Service Fabric</span></span>
<span data-ttu-id="b144f-199">Minden szolgáltatás van a saját egyéni konfigurációs csomagot.</span><span class="sxs-lookup"><span data-stu-id="b144f-199">Each service has its own individual configuration package.</span></span> <span data-ttu-id="b144f-200">Nincs olyan beépített mechanizmus globális konfigurációs beállítások érhető el a fürt valamennyi alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b144f-200">There is no built-in mechanism for global configuration settings accessible by all applications in a cluster.</span></span> <span data-ttu-id="b144f-201">A Service Fabric különleges Settings.xml konfigurációs fájl belül a konfigurációs csomag használata esetén a Settings.xml értékek írhatja felül hello alkalmazás szinten, amely lehetővé teszi az alkalmazás-konfigurációs beállítások.</span><span class="sxs-lookup"><span data-stu-id="b144f-201">When using Service Fabric's special Settings.xml configuration file within a configuration package, values in Settings.xml can be overwritten at hello application level, making application-level configuration settings possible.</span></span>

<span data-ttu-id="b144f-202">Konfigurációs beállítások állnak belül minden szolgáltatáspéldány hello szolgáltatáson keresztül fér hozzá `CodePackageActivationContext`.</span><span class="sxs-lookup"><span data-stu-id="b144f-202">Configuration settings are accesses within each service instance through hello service's `CodePackageActivationContext`.</span></span>

```C#

ConfigurationPackage configPackage = this.Context.CodePackageActivationContext.GetConfigurationPackageObject("Config");

// Access Settings.xml
KeyedCollection<string, ConfigurationProperty> parameters = configPackage.Settings.Sections["MyConfigSection"].Parameters;

string value = parameters["Key"]?.Value;

// Access custom configuration file:
using (StreamReader reader = new StreamReader(Path.Combine(configPackage.Path, "CustomConfig.json")))
{
    MySettings settings = JsonConvert.DeserializeObject<MySettings>(reader.ReadToEnd());
}

```

### <a name="configuration-update-events"></a><span data-ttu-id="b144f-203">A Configuration (update) események</span><span class="sxs-lookup"><span data-stu-id="b144f-203">Configuration update events</span></span>
#### <a name="cloud-services"></a><span data-ttu-id="b144f-204">Cloud Services</span><span class="sxs-lookup"><span data-stu-id="b144f-204">Cloud Services</span></span>
<span data-ttu-id="b144f-205">Hello `RoleEnvironment.Changed` esemény használt toonotify összes szerepkörpéldányt, amikor változás következik be, hello környezetben, például a konfiguráció módosítása.</span><span class="sxs-lookup"><span data-stu-id="b144f-205">hello `RoleEnvironment.Changed` event is used toonotify all role instances when a change occurs in hello environment, such as a configuration change.</span></span> <span data-ttu-id="b144f-206">Ez a használt tooconsume konfigurációfrissítések szerepkörpéldányokat újrahasznosítási, vagy indítsa újra a munkavégző folyamat nélkül.</span><span class="sxs-lookup"><span data-stu-id="b144f-206">This is used tooconsume configuration updates without recycling role instances or restarting a worker process.</span></span>

```C#

RoleEnvironment.Changed += RoleEnvironmentChanged;

private void RoleEnvironmentChanged(object sender, RoleEnvironmentChangedEventArgs e)
{
   // Get hello list of configuration changes
   var settingChanges = e.Changes.OfType<RoleEnvironmentConfigurationSettingChange>();
foreach (var settingChange in settingChanges) 
   {
      Trace.WriteLine("Setting: " + settingChange.ConfigurationSettingName, "Information");
   }
}

```

#### <a name="service-fabric"></a><span data-ttu-id="b144f-207">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="b144f-207">Service Fabric</span></span>
<span data-ttu-id="b144f-208">A csomag háromféle hello szolgáltatásban - kódot, konfiguráció és adatok - rendelkező események, amelyek tájékoztatást adnak a szolgáltatáspéldány, a csomag frissítése, hozzáadásakor vagy eltávolításakor.</span><span class="sxs-lookup"><span data-stu-id="b144f-208">Each of hello three package types in a service - Code, Config, and Data - have events that notify a service instance when a package is updated, added, or removed.</span></span> <span data-ttu-id="b144f-209">Egy szolgáltatás minden típusú több csomagot tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="b144f-209">A service can contain multiple packages of each type.</span></span> <span data-ttu-id="b144f-210">Előfordulhat például, hogy szolgáltatás több konfigurációs csomagot, minden egyes külön-külön rendszerverzióval ellátott és bővíthető.</span><span class="sxs-lookup"><span data-stu-id="b144f-210">For example, a service may have multiple config packages, each individually versioned and upgradeable.</span></span> 

<span data-ttu-id="b144f-211">Ezek az események módosulnak elérhető tooconsume service csomagokban hello szolgáltatáspéldány újraindítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="b144f-211">These events are available tooconsume changes in service packages without restarting hello service instance.</span></span>

```C#

this.Context.CodePackageActivationContext.ConfigurationPackageModifiedEvent +=
                    this.CodePackageActivationContext_ConfigurationPackageModifiedEvent;

private void CodePackageActivationContext_ConfigurationPackageModifiedEvent(object sender, PackageModifiedEventArgs<ConfigurationPackage> e)
{
    this.UpdateCustomConfig(e.NewPackage.Path);
    this.UpdateSettings(e.NewPackage.Settings);
}

```

## <a name="startup-tasks"></a><span data-ttu-id="b144f-212">Indítási feladatok</span><span class="sxs-lookup"><span data-stu-id="b144f-212">Startup tasks</span></span>
<span data-ttu-id="b144f-213">Indítási feladatokat is az alkalmazás indítása előtt végzett műveleteket.</span><span class="sxs-lookup"><span data-stu-id="b144f-213">Startup tasks are actions that are taken before an application starts.</span></span> <span data-ttu-id="b144f-214">Egy indítási tevékenységhez általánosan használt toorun telepítési parancsfájlokat emelt szintű jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="b144f-214">A startup task is typically used toorun setup scripts using elevated privileges.</span></span> <span data-ttu-id="b144f-215">Cloud Services és a Service Fabric támogatja a kezdeti feladatok.</span><span class="sxs-lookup"><span data-stu-id="b144f-215">Both Cloud Services and Service Fabric support start-up tasks.</span></span> <span data-ttu-id="b144f-216">hello fő különbség az, hogy a Felhőszolgáltatás, indítási feladat a feltételekhez tooa virtuális gép, mert része egy szerepkörpéldányt, mivel a Service Fabric egy indítási feladat a feltételekhez tooa szolgáltatás, amely nem a feltételekhez tooany adott virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="b144f-216">hello main difference is that in Cloud Services, a startup task is tied tooa VM because it is part of a role instance, whereas in Service Fabric a startup task is tied tooa service, which is not tied tooany particular VM.</span></span>

| <span data-ttu-id="b144f-217">Cloud Services</span><span class="sxs-lookup"><span data-stu-id="b144f-217">Cloud Services</span></span> | <span data-ttu-id="b144f-218">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="b144f-218">Service Fabric</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b144f-219">Konfiguráció helye</span><span class="sxs-lookup"><span data-stu-id="b144f-219">Configuration location</span></span> |<span data-ttu-id="b144f-220">ServiceDefinition.csdef</span><span class="sxs-lookup"><span data-stu-id="b144f-220">ServiceDefinition.csdef</span></span> |
| <span data-ttu-id="b144f-221">Jogosultságok</span><span class="sxs-lookup"><span data-stu-id="b144f-221">Privileges</span></span> |<span data-ttu-id="b144f-222">"korlátozott" vagy "emelt szintű"</span><span class="sxs-lookup"><span data-stu-id="b144f-222">"limited" or "elevated"</span></span> |
| <span data-ttu-id="b144f-223">Alkalmazás-előkészítés</span><span class="sxs-lookup"><span data-stu-id="b144f-223">Sequencing</span></span> |<span data-ttu-id="b144f-224">"egyszerű", "Háttér", "előtér"</span><span class="sxs-lookup"><span data-stu-id="b144f-224">"simple", "background", "foreground"</span></span> |

### <a name="cloud-services"></a><span data-ttu-id="b144f-225">Cloud Services</span><span class="sxs-lookup"><span data-stu-id="b144f-225">Cloud Services</span></span>
<span data-ttu-id="b144f-226">Cloud Services egy indítási belépési pont úgy van konfigurálva, a ServiceDefinition.csdef szerepkör /.</span><span class="sxs-lookup"><span data-stu-id="b144f-226">In Cloud Services a startup entry point is configured per role in ServiceDefinition.csdef.</span></span> 

```xml

<ServiceDefinition>
    <Startup>
        <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" >
            <Environment>
                <Variable name="MyVersionNumber" value="1.0.0.0" />
            </Environment>
        </Task>
    </Startup>
    ...
</ServiceDefinition>

```

### <a name="service-fabric"></a><span data-ttu-id="b144f-227">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="b144f-227">Service Fabric</span></span>
<span data-ttu-id="b144f-228">A Service Fabric egy indítási belépési pont úgy van konfigurálva, a ServiceManifest.xml-szolgáltatás esetében:</span><span class="sxs-lookup"><span data-stu-id="b144f-228">In Service Fabric a startup entry point is configured per service in ServiceManifest.xml:</span></span>

```xml

<ServiceManifest>
  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>Startup.bat</Program>
      </ExeHost>
    </SetupEntryPoint>
    ...
</ServiceManifest>

``` 

## <a name="a-note-about-development-environment"></a><span data-ttu-id="b144f-229">Fejlesztői környezet Megjegyzés</span><span class="sxs-lookup"><span data-stu-id="b144f-229">A note about development environment</span></span>
<span data-ttu-id="b144f-230">Cloud Services és a Service Fabric amelyek integrálhatók a Visual Studio projektsablonjai, és támogatja a hibakeresés, konfigurálása és telepítése mindkét helyileg és tooAzure.</span><span class="sxs-lookup"><span data-stu-id="b144f-230">Both Cloud Services and Service Fabric are integrated with Visual Studio with project templates and support for debugging, configuring, and deploying both locally and tooAzure.</span></span> <span data-ttu-id="b144f-231">Cloud Services és a Service Fabric is adja meg a helyi futtatási környezet.</span><span class="sxs-lookup"><span data-stu-id="b144f-231">Both Cloud Services and Service Fabric also provide a local development runtime environment.</span></span> <span data-ttu-id="b144f-232">hello különbség, hogy amíg hello fejlesztői futtatókörnyezet emulálja a felhőalapú szolgáltatás hello Azure környezetben az azt futtató, a Service Fabric nem használja az emulátor – hello teljes Service Fabric-futtatókörnyezet fogja használni.</span><span class="sxs-lookup"><span data-stu-id="b144f-232">hello difference is that while hello Cloud Service development runtime emulates hello Azure environment on which it runs, Service Fabric does not use an emulator - it uses hello complete Service Fabric runtime.</span></span> <span data-ttu-id="b144f-233">hello Service Fabric-környezet futtatja a helyi fejlesztési számítógépén hello ugyanabban a környezetben, amely a termelésben futtatja.</span><span class="sxs-lookup"><span data-stu-id="b144f-233">hello Service Fabric environment you run on your local development machine is hello same environment that runs in production.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b144f-234">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b144f-234">Next steps</span></span>
<span data-ttu-id="b144f-235">További információk a Service Fabric Reliable Services és a felhőalapú szolgáltatások és a Service Fabric application architektúra toounderstand hogyan tootake előnyeit teljes hello beállítása a Service Fabric funkciók alapvető különbségei hello.</span><span class="sxs-lookup"><span data-stu-id="b144f-235">Read more about Service Fabric Reliable Services and hello fundamental differences between Cloud Services and Service Fabric application architecture toounderstand how tootake advantage of hello full set of Service Fabric features.</span></span>

* [<span data-ttu-id="b144f-236">Bevezetés a Service Fabric Reliable Services használatába</span><span class="sxs-lookup"><span data-stu-id="b144f-236">Getting started with Service Fabric Reliable Services</span></span>](service-fabric-reliable-services-quick-start.md)
* [<span data-ttu-id="b144f-237">Cloud Services és a Service Fabric közötti fogalmi útmutató toohello különbségek</span><span class="sxs-lookup"><span data-stu-id="b144f-237">Conceptual guide toohello differences between Cloud Services and Service Fabric</span></span>](service-fabric-cloud-services-migration-differences.md)

<!--Image references-->
[3]: ./media/service-fabric-cloud-services-migration-worker-role-stateless-service/service-fabric-cloud-service-projects.png
[4]: ./media/service-fabric-cloud-services-migration-worker-role-stateless-service/worker-role-to-stateless-service.png
