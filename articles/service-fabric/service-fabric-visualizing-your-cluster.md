---
title: "A Service Fabric Explorerrel fürt megjelenítése |} Microsoft Docs"
description: "Service Fabric Explorerben talál egy olyan webalapú eszköz ellenőrzi, és a felhőalapú alkalmazások és a Microsoft Azure Service Fabric-fürt csomópontjának kezelése."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: c875b993-b4eb-494b-94b5-e02f5eddbd6a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/12/2017
ms.author: ryanwi
ms.openlocfilehash: 789793a7f50170188d688881a9178546c3074018
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="visualize-your-cluster-with-service-fabric-explorer"></a><span data-ttu-id="3e76b-103">A fürt megjelenítése a Service Fabric Explorerrel</span><span class="sxs-lookup"><span data-stu-id="3e76b-103">Visualize your cluster with Service Fabric Explorer</span></span>
<span data-ttu-id="3e76b-104">Service Fabric Explorerben talál egy olyan webalapú eszköz ellenőrzi, és az alkalmazások és az Azure Service Fabric-fürt csomópontjának kezelése.</span><span class="sxs-lookup"><span data-stu-id="3e76b-104">Service Fabric Explorer is a web-based tool for inspecting and managing applications and nodes in an Azure Service Fabric cluster.</span></span> <span data-ttu-id="3e76b-105">Service Fabric Explorer gazdája közvetlenül a fürthöz, így az mindig elérhető, függetlenül attól, amelyen fut a fürtön.</span><span class="sxs-lookup"><span data-stu-id="3e76b-105">Service Fabric Explorer is hosted directly within the cluster, so it is always available, regardless of where your cluster is running.</span></span>

## <a name="video-tutorial"></a><span data-ttu-id="3e76b-106">Oktatóvideó</span><span class="sxs-lookup"><span data-stu-id="3e76b-106">Video tutorial</span></span>

<span data-ttu-id="3e76b-107">Megtudhatja, hogyan használhatja a Service Fabric Explorer, a következő Microsoft Virtual Academy videót:</span><span class="sxs-lookup"><span data-stu-id="3e76b-107">To learn how to use Service Fabric Explorer, watch the following Microsoft Virtual Academy video:</span></span>

[<center><img src="./media/service-fabric-visualizing-your-cluster/SfxVideo.png" WIDTH="360" HEIGHT="244"></center>](https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=bBTFg46yC_9806218965)

## <a name="connect-to-service-fabric-explorer"></a><span data-ttu-id="3e76b-108">Kapcsolódás a Service Fabric Explorerrel</span><span class="sxs-lookup"><span data-stu-id="3e76b-108">Connect to Service Fabric Explorer</span></span>
<span data-ttu-id="3e76b-109">Ha már elvégezte az utasításokat [a fejlesztőkörnyezet előkészítése](service-fabric-get-started.md), elindíthatja a Service Fabric Explorer a helyi fürtön lévő 19080/Explorer útvonalon.</span><span class="sxs-lookup"><span data-stu-id="3e76b-109">If you have followed the instructions to [prepare your development environment](service-fabric-get-started.md), you can launch Service Fabric Explorer on your local cluster by navigating to http://localhost:19080/Explorer.</span></span>

## <a name="understand-the-service-fabric-explorer-layout"></a><span data-ttu-id="3e76b-110">A Service Fabric Explorer elrendezés ismertetése</span><span class="sxs-lookup"><span data-stu-id="3e76b-110">Understand the Service Fabric Explorer layout</span></span>
<span data-ttu-id="3e76b-111">Service Fabric Explorer navigálhat a bal oldali fában használatával.</span><span class="sxs-lookup"><span data-stu-id="3e76b-111">You can navigate through Service Fabric Explorer by using the tree on the left.</span></span> <span data-ttu-id="3e76b-112">A fa gyökerében a fürt irányítópult áttekintése a fürt, beleértve az alkalmazás és a csomópont állapotának összegzését.</span><span class="sxs-lookup"><span data-stu-id="3e76b-112">At the root of the tree, the cluster dashboard provides an overview of your cluster, including a summary of application and node health.</span></span>

![Service Fabric Explorer fürt irányítópult][sfx-cluster-dashboard]

### <a name="view-the-clusters-layout"></a><span data-ttu-id="3e76b-114">A fürt Elrendezés megtekintése</span><span class="sxs-lookup"><span data-stu-id="3e76b-114">View the cluster's layout</span></span>
<span data-ttu-id="3e76b-115">A Service Fabric-fürt csomópontjának kerülnek, egy kétdimenziós rácsot a tartalék tartományok között, és a frissítési tartományt.</span><span class="sxs-lookup"><span data-stu-id="3e76b-115">Nodes in a Service Fabric cluster are placed across a two-dimensional grid of fault domains and upgrade domains.</span></span> <span data-ttu-id="3e76b-116">Az elhelyezési biztosítja, hogy az alkalmazások továbbra is elérhető a hardver meghibásodása és alkalmazásfrissítések.</span><span class="sxs-lookup"><span data-stu-id="3e76b-116">This placement ensures that your applications remain available in the presence of hardware failures and application upgrades.</span></span> <span data-ttu-id="3e76b-117">A jelenlegi fürthöz van elrendezését a betűcsoport-leképezés használatával tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="3e76b-117">You can view how the current cluster is laid out by using the cluster map.</span></span>

![Service Fabric Explorer betűcsoport-leképezés][sfx-cluster-map]

### <a name="view-applications-and-services"></a><span data-ttu-id="3e76b-119">Nézet alkalmazások és szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="3e76b-119">View applications and services</span></span>
<span data-ttu-id="3e76b-120">A fürt két részfák tartalmazza: egyet az alkalmazások és más csomópontok.</span><span class="sxs-lookup"><span data-stu-id="3e76b-120">The cluster contains two subtrees: one for applications and another for nodes.</span></span>

<span data-ttu-id="3e76b-121">Az alkalmazás nézet segítségével haladjon végig a Service Fabric logikai hierarchia: alkalmazások, szolgáltatások, partíciók és replikáit.</span><span class="sxs-lookup"><span data-stu-id="3e76b-121">You can use the application view to navigate through Service Fabric's logical hierarchy: applications, services, partitions, and replicas.</span></span>

<span data-ttu-id="3e76b-122">Az alkalmazás az alábbi példában a **MyApp** két szolgáltatásból áll **MyStatefulService** és **WebService**.</span><span class="sxs-lookup"><span data-stu-id="3e76b-122">In the example below, the application **MyApp** consists of two services, **MyStatefulService** and **WebService**.</span></span> <span data-ttu-id="3e76b-123">Mivel a **MyStatefulService** állapotalapú, egy elsődleges és két másodlagos replika partíciót tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="3e76b-123">Since **MyStatefulService** is stateful, it includes a partition with one primary and two secondary replicas.</span></span> <span data-ttu-id="3e76b-124">Ezzel szemben WebSvcService állapot nélküli, és egyetlen példányt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="3e76b-124">By contrast, WebSvcService is stateless and contains a single instance.</span></span>

![Service Fabric Explorer alkalmazás megtekintése][sfx-application-tree]

<span data-ttu-id="3e76b-126">A fa egyes szintjein a fő ablaktáblán a cikk vonatkozó információkat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="3e76b-126">At each level of the tree, the main pane shows pertinent information about the item.</span></span> <span data-ttu-id="3e76b-127">Például láthatja a állapotát és egy adott szolgáltatáshoz verziója.</span><span class="sxs-lookup"><span data-stu-id="3e76b-127">For example, you can see the health status and version for a particular service.</span></span>

![Service Fabric Explorer essentials ablaktábla][sfx-service-essentials]

### <a name="view-the-clusters-nodes"></a><span data-ttu-id="3e76b-129">A fürtcsomópontok megtekintése</span><span class="sxs-lookup"><span data-stu-id="3e76b-129">View the cluster's nodes</span></span>
<span data-ttu-id="3e76b-130">A csomópontnézet a fürt fizikai elrendezését mutatja.</span><span class="sxs-lookup"><span data-stu-id="3e76b-130">The node view shows the physical layout of the cluster.</span></span> <span data-ttu-id="3e76b-131">Az egyes csomópontoknál megtekintheti, hogy melyik alkalmazások kódja üzemel az adott csomóponton.</span><span class="sxs-lookup"><span data-stu-id="3e76b-131">For a given node, you can inspect which applications have code deployed on that node.</span></span> <span data-ttu-id="3e76b-132">Pontosabban láthatja, melyik replikák futó van.</span><span class="sxs-lookup"><span data-stu-id="3e76b-132">More specifically, you can see which replicas are currently running there.</span></span>

## <a name="actions"></a><span data-ttu-id="3e76b-133">Műveletek</span><span class="sxs-lookup"><span data-stu-id="3e76b-133">Actions</span></span>
<span data-ttu-id="3e76b-134">Service Fabric Explorer műveleteket az egyes csomópontok, alkalmazások és szolgáltatások a fürtön belüli meghívása gyors lehetőséget kínál.</span><span class="sxs-lookup"><span data-stu-id="3e76b-134">Service Fabric Explorer offers a quick way to invoke actions on nodes, applications, and services within your cluster.</span></span>

<span data-ttu-id="3e76b-135">Például egy alkalmazáspéldányt törléséhez válassza ki az alkalmazást a a bal oldali fában, és válassza **műveletek** > **alkalmazás törlése**.</span><span class="sxs-lookup"><span data-stu-id="3e76b-135">For example, to delete an application instance, choose the application from the tree on the left, and then choose **Actions** > **Delete Application**.</span></span>

![A Service Fabric Explorerben alkalmazás törlése][sfx-delete-application]

> [!TIP]
> <span data-ttu-id="3e76b-137">Minden elem jelre kattintva ugyanazokat a műveleteket végezheti el.</span><span class="sxs-lookup"><span data-stu-id="3e76b-137">You can perform the same actions by clicking the ellipsis next to each element.</span></span>
>
>

<span data-ttu-id="3e76b-138">A következő táblázat felsorolja az egyes entitások esetében elérhető műveleteket:</span><span class="sxs-lookup"><span data-stu-id="3e76b-138">The following table lists the actions available for each entity:</span></span>

| <span data-ttu-id="3e76b-139">**Entitás**</span><span class="sxs-lookup"><span data-stu-id="3e76b-139">**Entity**</span></span> | <span data-ttu-id="3e76b-140">**Művelet**</span><span class="sxs-lookup"><span data-stu-id="3e76b-140">**Action**</span></span> | <span data-ttu-id="3e76b-141">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="3e76b-141">**Description**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e76b-142">Alkalmazás típusa</span><span class="sxs-lookup"><span data-stu-id="3e76b-142">Application type</span></span> |<span data-ttu-id="3e76b-143">Típus leépítése</span><span class="sxs-lookup"><span data-stu-id="3e76b-143">Unprovision type</span></span> |<span data-ttu-id="3e76b-144">Eltávolítja az alkalmazáscsomagot a fürt lemezképtárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="3e76b-144">Removes the application package from the cluster's image store.</span></span> <span data-ttu-id="3e76b-145">Először el kell távolítani az adott típusú összes alkalmazásra van szükség.</span><span class="sxs-lookup"><span data-stu-id="3e76b-145">Requires all applications of that type to be removed first.</span></span> |
| <span data-ttu-id="3e76b-146">Alkalmazás</span><span class="sxs-lookup"><span data-stu-id="3e76b-146">Application</span></span> |<span data-ttu-id="3e76b-147">Alkalmazás törlése</span><span class="sxs-lookup"><span data-stu-id="3e76b-147">Delete Application</span></span> |<span data-ttu-id="3e76b-148">Törli az alkalmazásból, beleértve a hozzá tartozó szolgáltatások és azok állapota (ha van ilyen).</span><span class="sxs-lookup"><span data-stu-id="3e76b-148">Delete the application, including all its services and their state (if any).</span></span> |
| <span data-ttu-id="3e76b-149">Szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="3e76b-149">Service</span></span> |<span data-ttu-id="3e76b-150">Szolgáltatás törlése</span><span class="sxs-lookup"><span data-stu-id="3e76b-150">Delete Service</span></span> |<span data-ttu-id="3e76b-151">(Ha van ilyen), törölje a szolgáltatást és annak állapotát.</span><span class="sxs-lookup"><span data-stu-id="3e76b-151">Delete the service and its state (if any).</span></span> |
| <span data-ttu-id="3e76b-152">Csomópont</span><span class="sxs-lookup"><span data-stu-id="3e76b-152">Node</span></span> |<span data-ttu-id="3e76b-153">Aktiválás</span><span class="sxs-lookup"><span data-stu-id="3e76b-153">Activate</span></span> |<span data-ttu-id="3e76b-154">A csomópont aktiválása.</span><span class="sxs-lookup"><span data-stu-id="3e76b-154">Activate the node.</span></span> |
| <span data-ttu-id="3e76b-155">Csomópont</span><span class="sxs-lookup"><span data-stu-id="3e76b-155">Node</span></span> | <span data-ttu-id="3e76b-156">(Szünet) inaktiválása</span><span class="sxs-lookup"><span data-stu-id="3e76b-156">Deactivate (pause)</span></span> | <span data-ttu-id="3e76b-157">A csomópont jelenlegi állapotában felfüggesztése.</span><span class="sxs-lookup"><span data-stu-id="3e76b-157">Pause the node in its current state.</span></span> <span data-ttu-id="3e76b-158">Szolgáltatások tovább fut, de a Service Fabric proaktív helyezi át a semmit, vagy ki, kivéve, ha arra szükség van kimaradás vagy adatok inkonzisztenciája megelőzése érdekében.</span><span class="sxs-lookup"><span data-stu-id="3e76b-158">Services continue to run but Service Fabric does not proactively move anything onto or off it unless it is required to prevent an outage or data inconsistency.</span></span> <span data-ttu-id="3e76b-159">Ez a művelet általában segítségével engedélyezheti a hibakeresési szolgáltatásokat annak érdekében, hogy azok helyezi át az ellenőrzés során adott csomóponton.</span><span class="sxs-lookup"><span data-stu-id="3e76b-159">This action is typically used to enable debugging services on a specific node to ensure that they do not move during inspection.</span></span> | |
| <span data-ttu-id="3e76b-160">Csomópont</span><span class="sxs-lookup"><span data-stu-id="3e76b-160">Node</span></span> | <span data-ttu-id="3e76b-161">Inaktiválás (újraindítás)</span><span class="sxs-lookup"><span data-stu-id="3e76b-161">Deactivate (restart)</span></span> | <span data-ttu-id="3e76b-162">Biztonságosan haladhat minden memórián belüli szolgáltatás ki olyan csomópont, és zárja be az állandó szolgáltatásokban.</span><span class="sxs-lookup"><span data-stu-id="3e76b-162">Safely move all in-memory services off a node and close persistent services.</span></span> <span data-ttu-id="3e76b-163">Általában használhatók, ha a gazdagép folyamatainak vagy a gép újra kell indítani.</span><span class="sxs-lookup"><span data-stu-id="3e76b-163">Typically used when the host processes or machine need to be restarted.</span></span> | |
| <span data-ttu-id="3e76b-164">Csomópont</span><span class="sxs-lookup"><span data-stu-id="3e76b-164">Node</span></span> | <span data-ttu-id="3e76b-165">(Az adatok eltávolítása) inaktiválása</span><span class="sxs-lookup"><span data-stu-id="3e76b-165">Deactivate (remove data)</span></span> | <span data-ttu-id="3e76b-166">Biztonságosan zárja be a megfelelő tartalék replikák létrehozása után a csomóponton futó összes szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="3e76b-166">Safely close all services running on the node after building sufficient spare replicas.</span></span> <span data-ttu-id="3e76b-167">Jellemzően akkor csomópont (vagy legalább a tárhely) kívül Bizottság véglegesen foglalja.</span><span class="sxs-lookup"><span data-stu-id="3e76b-167">Typically used when a node (or at least its storage) is being permanently taken out of commission.</span></span> | |
| <span data-ttu-id="3e76b-168">Csomópont</span><span class="sxs-lookup"><span data-stu-id="3e76b-168">Node</span></span> | <span data-ttu-id="3e76b-169">Távolítsa el a csomópont állapota</span><span class="sxs-lookup"><span data-stu-id="3e76b-169">Remove node state</span></span> | <span data-ttu-id="3e76b-170">Távolítsa el a fürt egy csomópont replikák ismerete.</span><span class="sxs-lookup"><span data-stu-id="3e76b-170">Remove knowledge of a node's replicas from the cluster.</span></span> <span data-ttu-id="3e76b-171">Általában akkor használható, ha egy már leállt csomópont helyreállíthatatlan tekintendő.</span><span class="sxs-lookup"><span data-stu-id="3e76b-171">Typically used when an already failed node is deemed unrecoverable.</span></span> | |
| <span data-ttu-id="3e76b-172">Csomópont</span><span class="sxs-lookup"><span data-stu-id="3e76b-172">Node</span></span> | <span data-ttu-id="3e76b-173">Újraindítás</span><span class="sxs-lookup"><span data-stu-id="3e76b-173">Restart</span></span> | <span data-ttu-id="3e76b-174">Csomópont hiba szimulálása a csomópont újraindításával.</span><span class="sxs-lookup"><span data-stu-id="3e76b-174">Simulate a node failure by restarting the node.</span></span> <span data-ttu-id="3e76b-175">További információ [Itt](/powershell/module/servicefabric/restart-servicefabricnode?view=azureservicefabricps)</span><span class="sxs-lookup"><span data-stu-id="3e76b-175">More information [here](/powershell/module/servicefabric/restart-servicefabricnode?view=azureservicefabricps)</span></span> | |

<span data-ttu-id="3e76b-176">Mivel számos műveletet károsak, a lehetséges, hogy a művelet befejezése előtt, győződjön meg arról, hogy a leképezés kéri.</span><span class="sxs-lookup"><span data-stu-id="3e76b-176">Since many actions are destructive, you may be asked to confirm your intent before the action is completed.</span></span>

> [!TIP]
> <span data-ttu-id="3e76b-177">Minden művelet, amely a Service Fabric Explorer segítségével végezheti el is PowerShell vagy a REST API-t automatizálására végezhető el.</span><span class="sxs-lookup"><span data-stu-id="3e76b-177">Every action that can be performed through Service Fabric Explorer can also be performed through PowerShell or a REST API, to enable automation.</span></span>
>
>

<span data-ttu-id="3e76b-178">Service Fabric Explorer használatával hozzon létre egy adott alkalmazás típusa vagy a termékverzió alkalmazáspéldányok.</span><span class="sxs-lookup"><span data-stu-id="3e76b-178">You can also use Service Fabric Explorer to create application instances for a given application type and version.</span></span> <span data-ttu-id="3e76b-179">Az alkalmazástípus válassza a faszerkezetes nézetben, majd kattintson a **app-példány létrehozása** mellett a verzióra szeretné, hogy a jobb oldali ablaktáblán látható hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="3e76b-179">Choose the application type in the tree view, then click the **Create app instance** link next to the version you'd like in the right pane.</span></span>

![Az alkalmazáspéldány létrehozása a Service Fabric Explorerben][sfx-create-app-instance]

> [!NOTE]
> <span data-ttu-id="3e76b-181">Jelenleg nem paraméterezett Service Fabric Explorer használatával létrehozott alkalmazás-példányokon.</span><span class="sxs-lookup"><span data-stu-id="3e76b-181">Application instances created through Service Fabric Explorer cannot currently be parameterized.</span></span> <span data-ttu-id="3e76b-182">Létrehozásuk alapértelmezett paraméterértékek használata.</span><span class="sxs-lookup"><span data-stu-id="3e76b-182">They are created using default parameter values.</span></span>
>
>

## <a name="connect-to-a-remote-service-fabric-cluster"></a><span data-ttu-id="3e76b-183">Csatlakozzon a távoli Service Fabric-fürt</span><span class="sxs-lookup"><span data-stu-id="3e76b-183">Connect to a remote Service Fabric cluster</span></span>
<span data-ttu-id="3e76b-184">Ha a fürt végpontja tudja, és rendelkezik a szükséges engedélyekkel a Service Fabric Explorer minden böngészőből végezheti el.</span><span class="sxs-lookup"><span data-stu-id="3e76b-184">If you know the cluster's endpoint and have sufficient permissions you can access Service Fabric Explorer from any browser.</span></span> <span data-ttu-id="3e76b-185">Ez azért, mert Service Fabric Explorer egy másik szolgáltatás, amely a fürt futtatja.</span><span class="sxs-lookup"><span data-stu-id="3e76b-185">This is because Service Fabric Explorer is just another service that runs in the cluster.</span></span>

### <a name="discover-the-service-fabric-explorer-endpoint-for-a-remote-cluster"></a><span data-ttu-id="3e76b-186">A távoli fürt Service Fabric Explorer-végpont felderítése</span><span class="sxs-lookup"><span data-stu-id="3e76b-186">Discover the Service Fabric Explorer endpoint for a remote cluster</span></span>
<span data-ttu-id="3e76b-187">Service Fabric Explorer egy adott fürt eléréséhez a böngészőben a pont:</span><span class="sxs-lookup"><span data-stu-id="3e76b-187">To reach Service Fabric Explorer for a given cluster, point your browser to:</span></span>

<span data-ttu-id="3e76b-188">http://&lt;a fürt végpontja&gt;: 19080/Explorer</span><span class="sxs-lookup"><span data-stu-id="3e76b-188">http://&lt;your-cluster-endpoint&gt;:19080/Explorer</span></span>

<span data-ttu-id="3e76b-189">Az Azure-fürtök esetén a teljes URL-címet is rendelkezésre áll a fürt essentials ablaktábláján az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="3e76b-189">For Azure clusters, the full URL is also available in the cluster essentials pane of the Azure portal.</span></span>

### <a name="connect-to-a-secure-cluster"></a><span data-ttu-id="3e76b-190">Csatlakozás biztonságos fürthöz</span><span class="sxs-lookup"><span data-stu-id="3e76b-190">Connect to a secure cluster</span></span>
<span data-ttu-id="3e76b-191">A Service Fabric fürt tanúsítványok, valamint az Azure Active Directory (AAD) használó ügyfél-hozzáférési szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="3e76b-191">You can control client access to your Service Fabric cluster either with certificates or using Azure Active Directory (AAD).</span></span>

<span data-ttu-id="3e76b-192">Ha próbál csatlakozni a Service Fabric Explorer egy biztonságos fürtön, majd függően a fürtkonfiguráció akkor lesz kell ügyféltanúsítványt jelen, vagy jelentkezzen be az AAD használja.</span><span class="sxs-lookup"><span data-stu-id="3e76b-192">If you attempt to connect to Service Fabric Explorer on a secure cluster, then depending on the cluster's configuration you'll be required to present a client certificate or log in using AAD.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3e76b-193">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3e76b-193">Next steps</span></span>
* [<span data-ttu-id="3e76b-194">Tesztelhetőségi áttekintése</span><span class="sxs-lookup"><span data-stu-id="3e76b-194">Testability overview</span></span>](service-fabric-testability-overview.md)
* [<span data-ttu-id="3e76b-195">A Visual Studio a Service Fabric-alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="3e76b-195">Managing your Service Fabric applications in Visual Studio</span></span>](service-fabric-manage-application-in-visual-studio.md)
* [<span data-ttu-id="3e76b-196">A Service Fabric-alkalmazás központi telepítésének PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="3e76b-196">Service Fabric application deployment using PowerShell</span></span>](service-fabric-deploy-remove-applications.md)

<!--Image references-->
[sfx-cluster-dashboard]: ./media/service-fabric-visualizing-your-cluster/SfxClusterDashboard.png
[sfx-cluster-map]: ./media/service-fabric-visualizing-your-cluster/SfxClusterMap.png
[sfx-application-tree]: ./media/service-fabric-visualizing-your-cluster/SfxApplicationTree.png
[sfx-service-essentials]: ./media/service-fabric-visualizing-your-cluster/SfxServiceEssentials.png
[sfx-delete-application]: ./media/service-fabric-visualizing-your-cluster/SfxDeleteApplication.png
[sfx-create-app-instance]: ./media/service-fabric-visualizing-your-cluster/SfxCreateAppInstance.png
