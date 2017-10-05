---
title: "Azure Cloud Services alkalmazások átalakítása mikroszolgáltatások |} Microsoft Docs"
description: "Ez az útmutató a felhőalapú szolgáltatások webes és feldolgozói szerepkörök és a Service Fabric állapotmentes szolgáltatások Felhőszolgáltatások telepítenek át a Service Fabric hasonlítja össze."
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
ms.openlocfilehash: 4ab1f83e88b262b1752300b2786340d9abca8154
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="guide-to-converting-web-and-worker-roles-to-service-fabric-stateless-services"></a><span data-ttu-id="a8c28-103">Útmutató a Service Fabric állapotmentes szolgáltatások webes és feldolgozói szerepkörök alakítása</span><span class="sxs-lookup"><span data-stu-id="a8c28-103">Guide to converting Web and Worker Roles to Service Fabric stateless services</span></span>
<span data-ttu-id="a8c28-104">A cikkből megtudhatja, hogyan telepíthetők át a felhőalapú szolgáltatások webes és feldolgozói szerepkörök a Service Fabric állapotmentes szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="a8c28-104">This article describes how to migrate your Cloud Services Web and Worker Roles to Service Fabric stateless services.</span></span> <span data-ttu-id="a8c28-105">Ez az a legegyszerűbb áttelepítési út a felhőalapú szolgáltatások a Service Fabric az alkalmazások, amelyek általános architektúrája érintetlen marad többé-kevésbé megegyezik.</span><span class="sxs-lookup"><span data-stu-id="a8c28-105">This is the simplest migration path from Cloud Services to Service Fabric for applications whose overall architecture is going to stay roughly the same.</span></span>

## <a name="cloud-service-project-to-service-fabric-application-project"></a><span data-ttu-id="a8c28-106">A Service Fabric-alkalmazás projekt felhőszolgáltatás-projekt</span><span class="sxs-lookup"><span data-stu-id="a8c28-106">Cloud Service project to Service Fabric application project</span></span>
 <span data-ttu-id="a8c28-107">Egy Felhőszolgáltatás-projektet és a Service Fabric-alkalmazás projekt hasonló struktúrával rendelkezik, és mindkét jelentik a telepítési egység, ez azt jelenti, hogy minden egyes alkalmazás - meghatározásához futtassa az alkalmazást a rendszer a teljes csomag.</span><span class="sxs-lookup"><span data-stu-id="a8c28-107">A Cloud Service project and a Service Fabric Application project have a similar structure and both represent the deployment unit for your application - that is, they each define the complete package that is deployed to run your application.</span></span> <span data-ttu-id="a8c28-108">Egy Felhőszolgáltatás-projekt tartalmazza a legalább egy webes vagy feldolgozói szerepköröket.</span><span class="sxs-lookup"><span data-stu-id="a8c28-108">A Cloud Service project contains one or more Web or Worker Roles.</span></span> <span data-ttu-id="a8c28-109">Hasonlóképpen a Service Fabric-alkalmazás projekt tartalmaz egy vagy több szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="a8c28-109">Similarly, a Service Fabric Application project contains one or more services.</span></span> 

<span data-ttu-id="a8c28-110">A különbség az, hogy a Felhőszolgáltatás-projekt párok az alkalmazás központi telepítése egy virtuális gép üzembe helyezéséhez, és így tartalmazza a virtuális gép konfigurációs beállításai, mivel a Service Fabric-alkalmazás projekt csak egy telepített alkalmazás a Service Fabric-fürt a meglévő virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="a8c28-110">The difference is that the Cloud Service project couples the application deployment with a VM deployment and thus contains VM configuration settings in it, whereas the Service Fabric Application project only defines an application that will be deployed to a set of existing VMs in a Service Fabric cluster.</span></span> <span data-ttu-id="a8c28-111">A Service Fabric-fürt maga csak a rendszer egyszer, egy Resource Manager-sablon vagy az Azure-portálon keresztül, és több Service Fabric-alkalmazások telepíthetők.</span><span class="sxs-lookup"><span data-stu-id="a8c28-111">The Service Fabric cluster itself is only deployed once, either through an Resource Manager template or through the Azure portal, and multiple Service Fabric applications can be deployed to it.</span></span>

![A Service Fabric és a Felhőszolgáltatások projekt összehasonlítása][3]

## <a name="worker-role-to-stateless-service"></a><span data-ttu-id="a8c28-113">Az állapotmentes szolgáltatások feldolgozói szerepkör</span><span class="sxs-lookup"><span data-stu-id="a8c28-113">Worker Role to stateless service</span></span>
<span data-ttu-id="a8c28-114">Elméleti szinten a feldolgozói szerepkör jelöli egy állapot nélküli alkalmazások és szolgáltatások, azaz a munkaterhelés minden példánya azonos és a kérelmek bármikor átirányítható tetszőleges példányra.</span><span class="sxs-lookup"><span data-stu-id="a8c28-114">Conceptually, a Worker Role represents a stateless workload, meaning every instance of the workload is identical and requests can be routed to any instance at any time.</span></span> <span data-ttu-id="a8c28-115">Minden példány jegyezze meg az előző kérést nem várt.</span><span class="sxs-lookup"><span data-stu-id="a8c28-115">Each instance is not expected to remember the previous request.</span></span> <span data-ttu-id="a8c28-116">Külső állapottárolóhoz, például az Azure Table Storage vagy Azure Document DB rendszerbe, amely az alkalmazások és szolgáltatások állapotát kezeli.</span><span class="sxs-lookup"><span data-stu-id="a8c28-116">State that the workload operates on is managed by an external state store, such as Azure Table Storage or Azure Document DB.</span></span> <span data-ttu-id="a8c28-117">A Service Fabric alkalmazások és szolgáltatások az ilyen típusú állapotmentes szolgáltatások helyőrző jelzi.</span><span class="sxs-lookup"><span data-stu-id="a8c28-117">In Service Fabric, this type of workload is represented by a Stateless Service.</span></span> <span data-ttu-id="a8c28-118">A legegyszerűbb módja a feldolgozói szerepkör Service Fabric áttelepíti egy állapotmentes szolgáltatások feldolgozói szerepkör kódját alakításával végezhető.</span><span class="sxs-lookup"><span data-stu-id="a8c28-118">The simplest approach to migrating a Worker Role to Service Fabric can be done by converting Worker Role code to a Stateless Service.</span></span>

![Az állapotmentes szolgáltatások feldolgozói szerepkör][4]

## <a name="web-role-to-stateless-service"></a><span data-ttu-id="a8c28-120">Webes szerepkör állapotmentes szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="a8c28-120">Web Role to stateless service</span></span>
<span data-ttu-id="a8c28-121">Feldolgozói szerepkör hasonló, a webes szerepkör is képviseli egy állapot nélküli alkalmazások és szolgáltatások, és így fogalmilag azt túl képezhető le a Service Fabric állapotmentes szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="a8c28-121">Similar to Worker Role, a Web Role also represents a stateless workload, and so conceptually it too can be mapped to a Service Fabric stateless service.</span></span> <span data-ttu-id="a8c28-122">Azonban eltérően webes szerepkörök a Service Fabric nem támogatja az IIS.</span><span class="sxs-lookup"><span data-stu-id="a8c28-122">However, unlike Web Roles, Service Fabric does not support IIS.</span></span> <span data-ttu-id="a8c28-123">Telepítse át a webes alkalmazás a webes szerepkör állapotmentes szolgáltatások megköveteli a egy webes keretrendszer, amely önállóan üzemel, és nem függ az IIS vagy System.Web, például az ASP.NET Core 1 első áthelyezését.</span><span class="sxs-lookup"><span data-stu-id="a8c28-123">To migrate a web application from a Web Role to a stateless service requires first moving to a web framework that can be self-hosted and does not depend on IIS or System.Web, such as ASP.NET Core 1.</span></span>

| <span data-ttu-id="a8c28-124">**Alkalmazás**</span><span class="sxs-lookup"><span data-stu-id="a8c28-124">**Application**</span></span> | <span data-ttu-id="a8c28-125">**Támogatott**</span><span class="sxs-lookup"><span data-stu-id="a8c28-125">**Supported**</span></span> | <span data-ttu-id="a8c28-126">**Áttelepítési útvonal**</span><span class="sxs-lookup"><span data-stu-id="a8c28-126">**Migration path**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a8c28-127">Az ASP.NET Web Forms keretrendszerre</span><span class="sxs-lookup"><span data-stu-id="a8c28-127">ASP.NET Web Forms</span></span> |<span data-ttu-id="a8c28-128">Nem</span><span class="sxs-lookup"><span data-stu-id="a8c28-128">No</span></span> |<span data-ttu-id="a8c28-129">Az ASP.NET Core 1 MVC átalakítása</span><span class="sxs-lookup"><span data-stu-id="a8c28-129">Convert to ASP.NET Core 1 MVC</span></span> |
| <span data-ttu-id="a8c28-130">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="a8c28-130">ASP.NET MVC</span></span> |<span data-ttu-id="a8c28-131">Az áttelepítés</span><span class="sxs-lookup"><span data-stu-id="a8c28-131">With Migration</span></span> |<span data-ttu-id="a8c28-132">Frissítés az ASP.NET Core 1 MVC</span><span class="sxs-lookup"><span data-stu-id="a8c28-132">Upgrade to ASP.NET Core 1 MVC</span></span> |
| <span data-ttu-id="a8c28-133">ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="a8c28-133">ASP.NET Web API</span></span> |<span data-ttu-id="a8c28-134">Az áttelepítés</span><span class="sxs-lookup"><span data-stu-id="a8c28-134">With Migration</span></span> |<span data-ttu-id="a8c28-135">Üzemeltetett önálló kiszolgáló vagy az ASP.NET Core 1</span><span class="sxs-lookup"><span data-stu-id="a8c28-135">Use self-hosted server or ASP.NET Core 1</span></span> |
| <span data-ttu-id="a8c28-136">Az ASP.NET Core 1</span><span class="sxs-lookup"><span data-stu-id="a8c28-136">ASP.NET Core 1</span></span> |<span data-ttu-id="a8c28-137">Igen</span><span class="sxs-lookup"><span data-stu-id="a8c28-137">Yes</span></span> |<span data-ttu-id="a8c28-138">N/A</span><span class="sxs-lookup"><span data-stu-id="a8c28-138">N/A</span></span> |

## <a name="entry-point-api-and-lifecycle"></a><span data-ttu-id="a8c28-139">Belépési pont API és életciklusa</span><span class="sxs-lookup"><span data-stu-id="a8c28-139">Entry point API and lifecycle</span></span>
<span data-ttu-id="a8c28-140">Feldolgozói szerepkör és a Service Fabric szolgáltatás az API-k ajánlat hasonló belépési pontok:</span><span class="sxs-lookup"><span data-stu-id="a8c28-140">Worker Role and Service Fabric service APIs offer similar entry points:</span></span> 

| <span data-ttu-id="a8c28-141">**A belépési pont**</span><span class="sxs-lookup"><span data-stu-id="a8c28-141">**Entry Point**</span></span> | <span data-ttu-id="a8c28-142">**Feldolgozói szerepkör**</span><span class="sxs-lookup"><span data-stu-id="a8c28-142">**Worker Role**</span></span> | <span data-ttu-id="a8c28-143">**Service Fabric-szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="a8c28-143">**Service Fabric service**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a8c28-144">Feldolgozása</span><span class="sxs-lookup"><span data-stu-id="a8c28-144">Processing</span></span> |`Run()` |`RunAsync()` |
| <span data-ttu-id="a8c28-145">Virtuális gép indítása</span><span class="sxs-lookup"><span data-stu-id="a8c28-145">VM start</span></span> |`OnStart()` |<span data-ttu-id="a8c28-146">N/A</span><span class="sxs-lookup"><span data-stu-id="a8c28-146">N/A</span></span> |
| <span data-ttu-id="a8c28-147">Virtuális gép leállítása</span><span class="sxs-lookup"><span data-stu-id="a8c28-147">VM stop</span></span> |`OnStop()` |<span data-ttu-id="a8c28-148">N/A</span><span class="sxs-lookup"><span data-stu-id="a8c28-148">N/A</span></span> |
| <span data-ttu-id="a8c28-149">Nyissa meg figyelő az ügyféli kérelmek részére</span><span class="sxs-lookup"><span data-stu-id="a8c28-149">Open listener for client requests</span></span> |<span data-ttu-id="a8c28-150">N/A</span><span class="sxs-lookup"><span data-stu-id="a8c28-150">N/A</span></span> |<ul><li> <span data-ttu-id="a8c28-151">`CreateServiceInstanceListener()`az állapot nélküli</span><span class="sxs-lookup"><span data-stu-id="a8c28-151">`CreateServiceInstanceListener()` for stateless</span></span></li><li><span data-ttu-id="a8c28-152">`CreateServiceReplicaListener()`az állapotalapú alkalmazások és szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="a8c28-152">`CreateServiceReplicaListener()` for stateful</span></span></li></ul> |

### <a name="worker-role"></a><span data-ttu-id="a8c28-153">Feldolgozói szerepkör</span><span class="sxs-lookup"><span data-stu-id="a8c28-153">Worker Role</span></span>
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

### <a name="service-fabric-stateless-service"></a><span data-ttu-id="a8c28-154">Service Fabric állapotmentes szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="a8c28-154">Service Fabric Stateless Service</span></span>
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

<span data-ttu-id="a8c28-155">Egy elsődleges "Futtatás" felülbírálás feldolgozási kezdőpontjaként egyaránt rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="a8c28-155">Both have a primary "Run" override in which to begin processing.</span></span> <span data-ttu-id="a8c28-156">A Service Fabric szolgáltatások egyesítése `Run`, `Start`, és `Stop` egy-egy belépési pont, a `RunAsync`.</span><span class="sxs-lookup"><span data-stu-id="a8c28-156">Service Fabric services  combine `Run`, `Start`, and `Stop` into a single entry point, `RunAsync`.</span></span> <span data-ttu-id="a8c28-157">A szolgáltatás működik, ha használatba `RunAsync` elindul, és le kell állnia, ha működik a `RunAsync` metódus CancellationToken leállítási jelzést kapott.</span><span class="sxs-lookup"><span data-stu-id="a8c28-157">Your service should begin working when `RunAsync` starts, and should stop working when the `RunAsync` method's CancellationToken is signaled.</span></span> 

<span data-ttu-id="a8c28-158">Az életciklus és feldolgozói szerepkörök és a Service Fabric szolgáltatás élettartama között számos fontos különbség van:</span><span class="sxs-lookup"><span data-stu-id="a8c28-158">There are several key differences between the lifecycle and lifetime of Worker Roles and Service Fabric services:</span></span>

* <span data-ttu-id="a8c28-159">**Életciklus:** a legnagyobb különbség az, hogy a feldolgozói szerepkör egy virtuális Gépet, és így a virtuális gép, ha a virtuális gép indítása és leállítása eseményeket tartalmazó életciklus kötődik.</span><span class="sxs-lookup"><span data-stu-id="a8c28-159">**Lifecycle:** The biggest difference is that a Worker Role is a VM and so its lifecycle is tied to the VM, which includes events for when the VM starts and stops.</span></span> <span data-ttu-id="a8c28-160">A Service Fabric-szolgáltatás egy életciklussal, amely nem a virtuális gép életciklusát, ezért nem tartalmazza a események a gazdagép, VM vagy a gép indítása és leállítása, mert nem áll(nak) kapcsolatban van.</span><span class="sxs-lookup"><span data-stu-id="a8c28-160">A Service Fabric service has a lifecycle that is separate from the VM lifecycle, so it does not include events for when the host VM or machine starts and stop, as they are not related.</span></span>
* <span data-ttu-id="a8c28-161">**Élettartam:** A feldolgozói szerepkör példánya indul, ha a `Run` metódus kilép.</span><span class="sxs-lookup"><span data-stu-id="a8c28-161">**Lifetime:** A Worker Role instance will recycle if the `Run` method exits.</span></span> <span data-ttu-id="a8c28-162">A `RunAsync` metódus a Service Fabric-szolgáltatás azonban is végrehajtása, és a szolgáltatáspéldány fel maradnak.</span><span class="sxs-lookup"><span data-stu-id="a8c28-162">The `RunAsync` method in a Service Fabric service however can run to completion and the service instance will stay up.</span></span> 

<span data-ttu-id="a8c28-163">A Service Fabric egy választható kommunikációs telepítő belépési pontot nyújt az ügyfélkérelmek szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="a8c28-163">Service Fabric provides an optional communication setup entry point for services that listen for client requests.</span></span> <span data-ttu-id="a8c28-164">A RunAsync és a kommunikáció belépési pont a Service Fabric szolgáltatások – a szolgáltatás dönthetnek úgy, hogy az ügyfelek kéréseire csak figyelje, illetve csak futtassa egy feldolgozási ciklus, vagy mindkettőt -, ezért a RunAsync metódusában engedélyezett újraindítása nélküli kilépéshez választható felülbírálások a szolgáltatás példány, mert a figyelő az ügyféli kérelmek részére továbbra is.</span><span class="sxs-lookup"><span data-stu-id="a8c28-164">Both the RunAsync and communication entry point are optional overrides in Service Fabric services - your service may choose to only listen to client requests, or only run a processing loop, or both - which is why the RunAsync method is allowed to exit without restarting the service instance, because it may continue to listen for client requests.</span></span>

## <a name="application-api-and-environment"></a><span data-ttu-id="a8c28-165">Alkalmazás API és a környezet</span><span class="sxs-lookup"><span data-stu-id="a8c28-165">Application API and environment</span></span>
<span data-ttu-id="a8c28-166">A Cloud Services környezet API információkat és az aktuális Virtuálisgép-példány, valamint a más VM szerepkörpéldányokat kapcsolatos információkat biztosít a.</span><span class="sxs-lookup"><span data-stu-id="a8c28-166">The Cloud Services environment API provides information and functionality for the current VM instance as well as information about other VM role instances.</span></span> <span data-ttu-id="a8c28-167">A Service Fabric a futásidejű kapcsolatos információkat tartalmaz, és némi információt a csomópont szolgáltatásként fut rajta.</span><span class="sxs-lookup"><span data-stu-id="a8c28-167">Service Fabric provides information related to its runtime and some information about the node a service is currently running on.</span></span> 

| <span data-ttu-id="a8c28-168">**Környezet feladat**</span><span class="sxs-lookup"><span data-stu-id="a8c28-168">**Environment Task**</span></span> | <span data-ttu-id="a8c28-169">**Cloud Services**</span><span class="sxs-lookup"><span data-stu-id="a8c28-169">**Cloud Services**</span></span> | <span data-ttu-id="a8c28-170">**Service Fabric**</span><span class="sxs-lookup"><span data-stu-id="a8c28-170">**Service Fabric**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a8c28-171">A konfigurációs beállítások és módosítási értesítés</span><span class="sxs-lookup"><span data-stu-id="a8c28-171">Configuration Settings and change notification</span></span> |`RoleEnvironment` |`CodePackageActivationContext` |
| <span data-ttu-id="a8c28-172">Helyi tároló</span><span class="sxs-lookup"><span data-stu-id="a8c28-172">Local Storage</span></span> |`RoleEnvironment` |`CodePackageActivationContext` |
| <span data-ttu-id="a8c28-173">Végpont</span><span class="sxs-lookup"><span data-stu-id="a8c28-173">Endpoint Information</span></span> |`RoleInstance` <ul><li><span data-ttu-id="a8c28-174">Jelenlegi példány:`RoleEnvironment.CurrentRoleInstance`</span><span class="sxs-lookup"><span data-stu-id="a8c28-174">Current instance: `RoleEnvironment.CurrentRoleInstance`</span></span></li><li><span data-ttu-id="a8c28-175">Egyéb szerepköröket és példány:`RoleEnvironment.Roles`</span><span class="sxs-lookup"><span data-stu-id="a8c28-175">Other roles and instance: `RoleEnvironment.Roles`</span></span></li> |<ul><li><span data-ttu-id="a8c28-176">`NodeContext`az aktuális csomópont-címe</span><span class="sxs-lookup"><span data-stu-id="a8c28-176">`NodeContext` for current Node address</span></span></li><li><span data-ttu-id="a8c28-177">`FabricClient`és `ServicePartitionResolver` a szolgáltatási végpont felderítése</span><span class="sxs-lookup"><span data-stu-id="a8c28-177">`FabricClient` and `ServicePartitionResolver` for service endpoint discovery</span></span></li> |
| <span data-ttu-id="a8c28-178">Az emuláció környezet</span><span class="sxs-lookup"><span data-stu-id="a8c28-178">Environment Emulation</span></span> |`RoleEnvironment.IsEmulated` |<span data-ttu-id="a8c28-179">N/A</span><span class="sxs-lookup"><span data-stu-id="a8c28-179">N/A</span></span> |
| <span data-ttu-id="a8c28-180">Egyidejű esemény</span><span class="sxs-lookup"><span data-stu-id="a8c28-180">Simultaneous change event</span></span> |`RoleEnvironment` |<span data-ttu-id="a8c28-181">N/A</span><span class="sxs-lookup"><span data-stu-id="a8c28-181">N/A</span></span> |

## <a name="configuration-settings"></a><span data-ttu-id="a8c28-182">Konfigurációs beállítások</span><span class="sxs-lookup"><span data-stu-id="a8c28-182">Configuration settings</span></span>
<span data-ttu-id="a8c28-183">Konfigurációs beállításai a Felhőszolgáltatások Virtuálisgép-szerepkör állítja be, és a Virtuálisgép-szerepkör minden példányára vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="a8c28-183">Configuration settings in Cloud Services are set for a VM role and apply to all instances of that VM role.</span></span> <span data-ttu-id="a8c28-184">Ezek a beállítások olyan ServiceConfiguration.*.cscfg fájlok beállított kulcs-érték párok, és segítségével érhető el közvetlenül roleenvironment-et.</span><span class="sxs-lookup"><span data-stu-id="a8c28-184">These settings are key-value pairs set in ServiceConfiguration.*.cscfg files and can be accessed directly through RoleEnvironment.</span></span> <span data-ttu-id="a8c28-185">A Service Fabric beállítások érvényesek külön-külön minden szolgáltatáshoz, és mindegyik alkalmazás, nem pedig egy virtuális géphez, mert egy virtuális Gépet, rendelkezhet több szolgáltatásokat és alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="a8c28-185">In Service Fabric, settings apply individually to each service and to each application, rather than to a VM, because a VM can host multiple services and applications.</span></span> <span data-ttu-id="a8c28-186">A szolgáltatás három csomagok áll:</span><span class="sxs-lookup"><span data-stu-id="a8c28-186">A service is composed of three packages:</span></span>

* <span data-ttu-id="a8c28-187">**Kód:** a szolgáltatás végrehajtható fájlok, bináris fájljai, dll-EK és a szolgáltatás futtatásához szükséges egyéb fájlokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="a8c28-187">**Code:** contains the service executables, binaries, DLLs, and any other files a service needs to run.</span></span>
* <span data-ttu-id="a8c28-188">**A Config:** összes konfigurációs fájlokat és a szolgáltatás beállításait.</span><span class="sxs-lookup"><span data-stu-id="a8c28-188">**Config:** all configuration files and settings for a service.</span></span>
* <span data-ttu-id="a8c28-189">**Adatok:** a szolgáltatáshoz társított statikus adatfájlokat.</span><span class="sxs-lookup"><span data-stu-id="a8c28-189">**Data:** static data files associated with the service.</span></span>

<span data-ttu-id="a8c28-190">Ezeket a csomagokat mindegyikének lehet függetlenül rendszerverzióval ellátott és frissített.</span><span class="sxs-lookup"><span data-stu-id="a8c28-190">Each of these packages can be independently versioned and upgraded.</span></span> <span data-ttu-id="a8c28-191">Cloud Services hasonló, a konfigurációs csomag programozott módon a következőkkel érhetők el az API-k és események állnak rendelkezésre a szolgáltatás konfigurációs csomag változás értesíteni.</span><span class="sxs-lookup"><span data-stu-id="a8c28-191">Similar to Cloud Services, a config package can be accessed programmatically through an API and events are available to notify the service of a config package change.</span></span> <span data-ttu-id="a8c28-192">A Settings.xml fájlban, mind az alkalmazás beállításainak szakaszában az App.config fájl hasonló programozott hozzáférés kulcs-érték használható.</span><span class="sxs-lookup"><span data-stu-id="a8c28-192">A Settings.xml file can be used for key-value configuration and programmatic access similar to the app settings section of an App.config file.</span></span> <span data-ttu-id="a8c28-193">Azonban eltérően Felhőszolgáltatásokat, a Service Fabric config csomag bármely konfigurációs fájlokat tartalmazza tetszőleges méretű XML, JSON, YAM vagy egyéni bináris formátumot.</span><span class="sxs-lookup"><span data-stu-id="a8c28-193">However, unlike Cloud Services, a Service Fabric config package can contain any configuration files in any format, whether it's XML, JSON, YAML, or a custom binary format.</span></span> 

### <a name="accessing-configuration"></a><span data-ttu-id="a8c28-194">Konfigurációs elérése</span><span class="sxs-lookup"><span data-stu-id="a8c28-194">Accessing configuration</span></span>
#### <a name="cloud-services"></a><span data-ttu-id="a8c28-195">Cloud Services</span><span class="sxs-lookup"><span data-stu-id="a8c28-195">Cloud Services</span></span>
<span data-ttu-id="a8c28-196">Konfigurációs beállítások ServiceConfiguration.*.cscfg keresztül elérhető `RoleEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="a8c28-196">Configuration settings from ServiceConfiguration.*.cscfg can be accessed through `RoleEnvironment`.</span></span> <span data-ttu-id="a8c28-197">Ezek a beállítások globálisan elérhetők összes szerepkörpéldányt azonos Cloud Service-környezetben.</span><span class="sxs-lookup"><span data-stu-id="a8c28-197">These settings are globally available to all role instances in the same Cloud Service deployment.</span></span>

```C#

string value = RoleEnvironment.GetConfigurationSettingValue("Key");

```

#### <a name="service-fabric"></a><span data-ttu-id="a8c28-198">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="a8c28-198">Service Fabric</span></span>
<span data-ttu-id="a8c28-199">Minden szolgáltatás van a saját egyéni konfigurációs csomagot.</span><span class="sxs-lookup"><span data-stu-id="a8c28-199">Each service has its own individual configuration package.</span></span> <span data-ttu-id="a8c28-200">Nincs olyan beépített mechanizmus globális konfigurációs beállítások érhető el a fürt valamennyi alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a8c28-200">There is no built-in mechanism for global configuration settings accessible by all applications in a cluster.</span></span> <span data-ttu-id="a8c28-201">A Service Fabric különleges Settings.xml konfigurációs fájl belül a konfigurációs csomag használata esetén a Settings.xml értékek írhatja felül az alkalmazás szintjén, amely lehetővé teszi az alkalmazás-konfigurációs beállítások.</span><span class="sxs-lookup"><span data-stu-id="a8c28-201">When using Service Fabric's special Settings.xml configuration file within a configuration package, values in Settings.xml can be overwritten at the application level, making application-level configuration settings possible.</span></span>

<span data-ttu-id="a8c28-202">Konfigurációs beállítások állnak belül minden szolgáltatáspéldány, a szolgáltatáson keresztül fér hozzá `CodePackageActivationContext`.</span><span class="sxs-lookup"><span data-stu-id="a8c28-202">Configuration settings are accesses within each service instance through the service's `CodePackageActivationContext`.</span></span>

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

### <a name="configuration-update-events"></a><span data-ttu-id="a8c28-203">A Configuration (update) események</span><span class="sxs-lookup"><span data-stu-id="a8c28-203">Configuration update events</span></span>
#### <a name="cloud-services"></a><span data-ttu-id="a8c28-204">Cloud Services</span><span class="sxs-lookup"><span data-stu-id="a8c28-204">Cloud Services</span></span>
<span data-ttu-id="a8c28-205">A `RoleEnvironment.Changed` eseménnyel összes szerepkörpéldányt értesítése, ha olyan változás esetén a környezetben, például a konfiguráció módosítása.</span><span class="sxs-lookup"><span data-stu-id="a8c28-205">The `RoleEnvironment.Changed` event is used to notify all role instances when a change occurs in the environment, such as a configuration change.</span></span> <span data-ttu-id="a8c28-206">Ennek segítségével konfigurációfrissítések felhasználása nélkül újrahasznosítási szerepkörpéldányt beállítani, vagy indítsa újra a munkavégző folyamat.</span><span class="sxs-lookup"><span data-stu-id="a8c28-206">This is used to consume configuration updates without recycling role instances or restarting a worker process.</span></span>

```C#

RoleEnvironment.Changed += RoleEnvironmentChanged;

private void RoleEnvironmentChanged(object sender, RoleEnvironmentChangedEventArgs e)
{
   // Get the list of configuration changes
   var settingChanges = e.Changes.OfType<RoleEnvironmentConfigurationSettingChange>();
foreach (var settingChange in settingChanges) 
   {
      Trace.WriteLine("Setting: " + settingChange.ConfigurationSettingName, "Information");
   }
}

```

#### <a name="service-fabric"></a><span data-ttu-id="a8c28-207">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="a8c28-207">Service Fabric</span></span>
<span data-ttu-id="a8c28-208">Egy szolgáltatás - kódot, konfiguráció és adatok - három csomag típusa rendelkező események, amelyek tájékoztatást adnak a szolgáltatáspéldány, a csomag frissítése, hozzáadásakor vagy eltávolításakor.</span><span class="sxs-lookup"><span data-stu-id="a8c28-208">Each of the three package types in a service - Code, Config, and Data - have events that notify a service instance when a package is updated, added, or removed.</span></span> <span data-ttu-id="a8c28-209">Egy szolgáltatás minden típusú több csomagot tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="a8c28-209">A service can contain multiple packages of each type.</span></span> <span data-ttu-id="a8c28-210">Előfordulhat például, hogy szolgáltatás több konfigurációs csomagot, minden egyes külön-külön rendszerverzióval ellátott és bővíthető.</span><span class="sxs-lookup"><span data-stu-id="a8c28-210">For example, a service may have multiple config packages, each individually versioned and upgradeable.</span></span> 

<span data-ttu-id="a8c28-211">Ezek az események felhasználásához szolgáltatáscsomagok változásairól a szolgáltatáspéldány újraindítása nélkül érhetők el.</span><span class="sxs-lookup"><span data-stu-id="a8c28-211">These events are available to consume changes in service packages without restarting the service instance.</span></span>

```C#

this.Context.CodePackageActivationContext.ConfigurationPackageModifiedEvent +=
                    this.CodePackageActivationContext_ConfigurationPackageModifiedEvent;

private void CodePackageActivationContext_ConfigurationPackageModifiedEvent(object sender, PackageModifiedEventArgs<ConfigurationPackage> e)
{
    this.UpdateCustomConfig(e.NewPackage.Path);
    this.UpdateSettings(e.NewPackage.Settings);
}

```

## <a name="startup-tasks"></a><span data-ttu-id="a8c28-212">Indítási feladatok</span><span class="sxs-lookup"><span data-stu-id="a8c28-212">Startup tasks</span></span>
<span data-ttu-id="a8c28-213">Indítási feladatokat is az alkalmazás indítása előtt végzett műveleteket.</span><span class="sxs-lookup"><span data-stu-id="a8c28-213">Startup tasks are actions that are taken before an application starts.</span></span> <span data-ttu-id="a8c28-214">Egy indítási tevékenységhez általában emelt szintű jogosultságokkal telepítő parancsfájlok futtatásához használt.</span><span class="sxs-lookup"><span data-stu-id="a8c28-214">A startup task is typically used to run setup scripts using elevated privileges.</span></span> <span data-ttu-id="a8c28-215">Cloud Services és a Service Fabric támogatja a kezdeti feladatok.</span><span class="sxs-lookup"><span data-stu-id="a8c28-215">Both Cloud Services and Service Fabric support start-up tasks.</span></span> <span data-ttu-id="a8c28-216">A fő különbség, hogy a felhőalapú szolgáltatások indítási feladat van kötve egy virtuális gép már része egy szerepkörpéldányt, mert mivel a Service Fabric egy indítási feladat a szolgáltatást, amely nem kötődik azonban bármely adott virtuális géphez van kötve.</span><span class="sxs-lookup"><span data-stu-id="a8c28-216">The main difference is that in Cloud Services, a startup task is tied to a VM because it is part of a role instance, whereas in Service Fabric a startup task is tied to a service, which is not tied to any particular VM.</span></span>

| <span data-ttu-id="a8c28-217">Cloud Services</span><span class="sxs-lookup"><span data-stu-id="a8c28-217">Cloud Services</span></span> | <span data-ttu-id="a8c28-218">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="a8c28-218">Service Fabric</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a8c28-219">Konfiguráció helye</span><span class="sxs-lookup"><span data-stu-id="a8c28-219">Configuration location</span></span> |<span data-ttu-id="a8c28-220">ServiceDefinition.csdef</span><span class="sxs-lookup"><span data-stu-id="a8c28-220">ServiceDefinition.csdef</span></span> |
| <span data-ttu-id="a8c28-221">Jogosultságok</span><span class="sxs-lookup"><span data-stu-id="a8c28-221">Privileges</span></span> |<span data-ttu-id="a8c28-222">"korlátozott" vagy "emelt szintű"</span><span class="sxs-lookup"><span data-stu-id="a8c28-222">"limited" or "elevated"</span></span> |
| <span data-ttu-id="a8c28-223">Alkalmazás-előkészítés</span><span class="sxs-lookup"><span data-stu-id="a8c28-223">Sequencing</span></span> |<span data-ttu-id="a8c28-224">"egyszerű", "Háttér", "előtér"</span><span class="sxs-lookup"><span data-stu-id="a8c28-224">"simple", "background", "foreground"</span></span> |

### <a name="cloud-services"></a><span data-ttu-id="a8c28-225">Cloud Services</span><span class="sxs-lookup"><span data-stu-id="a8c28-225">Cloud Services</span></span>
<span data-ttu-id="a8c28-226">Cloud Services egy indítási belépési pont úgy van konfigurálva, a ServiceDefinition.csdef szerepkör /.</span><span class="sxs-lookup"><span data-stu-id="a8c28-226">In Cloud Services a startup entry point is configured per role in ServiceDefinition.csdef.</span></span> 

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

### <a name="service-fabric"></a><span data-ttu-id="a8c28-227">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="a8c28-227">Service Fabric</span></span>
<span data-ttu-id="a8c28-228">A Service Fabric egy indítási belépési pont úgy van konfigurálva, a ServiceManifest.xml-szolgáltatás esetében:</span><span class="sxs-lookup"><span data-stu-id="a8c28-228">In Service Fabric a startup entry point is configured per service in ServiceManifest.xml:</span></span>

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

## <a name="a-note-about-development-environment"></a><span data-ttu-id="a8c28-229">Fejlesztői környezet Megjegyzés</span><span class="sxs-lookup"><span data-stu-id="a8c28-229">A note about development environment</span></span>
<span data-ttu-id="a8c28-230">Cloud Services és a Service Fabric vannak integrálva a Visual Studio projektsablonjai, és támogatja a hibakeresés konfigurálását, és a helyi és központi telepítése mind az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="a8c28-230">Both Cloud Services and Service Fabric are integrated with Visual Studio with project templates and support for debugging, configuring, and deploying both locally and to Azure.</span></span> <span data-ttu-id="a8c28-231">Cloud Services és a Service Fabric is adja meg a helyi futtatási környezet.</span><span class="sxs-lookup"><span data-stu-id="a8c28-231">Both Cloud Services and Service Fabric also provide a local development runtime environment.</span></span> <span data-ttu-id="a8c28-232">Az különbség, hogy a Felhőszolgáltatás fejlesztői futtatókörnyezetet az Azure környezetbe, az azt futtató emulálja, amíg a Service Fabric nem használja az emulátor – a teljes Service Fabric-futtatókörnyezet fogja használni.</span><span class="sxs-lookup"><span data-stu-id="a8c28-232">The difference is that while the Cloud Service development runtime emulates the Azure environment on which it runs, Service Fabric does not use an emulator - it uses the complete Service Fabric runtime.</span></span> <span data-ttu-id="a8c28-233">A Service Fabric futtatja a helyi fejlesztési számítógépén környezete ugyanabban a környezetben, amely a termelésben futtatja.</span><span class="sxs-lookup"><span data-stu-id="a8c28-233">The Service Fabric environment you run on your local development machine is the same environment that runs in production.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a8c28-234">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a8c28-234">Next steps</span></span>
<span data-ttu-id="a8c28-235">További információk a Service Fabric Reliable Services és a Cloud Services és a Service Fabric-alkalmazás architektúra annak megértése, hogyan előnyeit a Service Fabric-szolgáltatások teljes készletének alapvető eltérései.</span><span class="sxs-lookup"><span data-stu-id="a8c28-235">Read more about Service Fabric Reliable Services and the fundamental differences between Cloud Services and Service Fabric application architecture to understand how to take advantage of the full set of Service Fabric features.</span></span>

* [<span data-ttu-id="a8c28-236">Bevezetés a Service Fabric Reliable Services használatába</span><span class="sxs-lookup"><span data-stu-id="a8c28-236">Getting started with Service Fabric Reliable Services</span></span>](service-fabric-reliable-services-quick-start.md)
* [<span data-ttu-id="a8c28-237">Cloud Services és a Service Fabric közötti különbségekről fogalmi útmutató</span><span class="sxs-lookup"><span data-stu-id="a8c28-237">Conceptual guide to the differences between Cloud Services and Service Fabric</span></span>](service-fabric-cloud-services-migration-differences.md)

<!--Image references-->
[3]: ./media/service-fabric-cloud-services-migration-worker-role-stateless-service/service-fabric-cloud-service-projects.png
[4]: ./media/service-fabric-cloud-services-migration-worker-role-stateless-service/worker-role-to-stateless-service.png
