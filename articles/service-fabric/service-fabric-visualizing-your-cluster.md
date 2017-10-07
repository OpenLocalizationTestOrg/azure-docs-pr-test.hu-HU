---
title: "aaaVisualizing a Service Fabric Explorerrel fürt |} Microsoft Docs"
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
ms.openlocfilehash: 73adc4fc254cf6b949b4419b02a046cee3f6a83d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-your-cluster-with-service-fabric-explorer"></a><span data-ttu-id="075e4-103">A fürt megjelenítése a Service Fabric Explorerrel</span><span class="sxs-lookup"><span data-stu-id="075e4-103">Visualize your cluster with Service Fabric Explorer</span></span>
<span data-ttu-id="075e4-104">Service Fabric Explorerben talál egy olyan webalapú eszköz ellenőrzi, és az alkalmazások és az Azure Service Fabric-fürt csomópontjának kezelése.</span><span class="sxs-lookup"><span data-stu-id="075e4-104">Service Fabric Explorer is a web-based tool for inspecting and managing applications and nodes in an Azure Service Fabric cluster.</span></span> <span data-ttu-id="075e4-105">Service Fabric Explorer gazdája közvetlenül hello fürt, ezért mindig elérhető, függetlenül attól, amelyen fut a fürtön.</span><span class="sxs-lookup"><span data-stu-id="075e4-105">Service Fabric Explorer is hosted directly within hello cluster, so it is always available, regardless of where your cluster is running.</span></span>

## <a name="video-tutorial"></a><span data-ttu-id="075e4-106">Oktatóvideó</span><span class="sxs-lookup"><span data-stu-id="075e4-106">Video tutorial</span></span>

<span data-ttu-id="075e4-107">Hogyan tekintse meg a Service Fabric Explorer toouse toolearn hello a Microsoft Virtual Academy videót a következő:</span><span class="sxs-lookup"><span data-stu-id="075e4-107">toolearn how toouse Service Fabric Explorer, watch hello following Microsoft Virtual Academy video:</span></span>

[<center><img src="./media/service-fabric-visualizing-your-cluster/SfxVideo.png" WIDTH="360" HEIGHT="244"></center>](https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=bBTFg46yC_9806218965)

## <a name="connect-tooservice-fabric-explorer"></a><span data-ttu-id="075e4-108">Csatlakozás tooService Fabric Explorerrel</span><span class="sxs-lookup"><span data-stu-id="075e4-108">Connect tooService Fabric Explorer</span></span>
<span data-ttu-id="075e4-109">Ha már elvégezte túl hello utasításokat[a fejlesztőkörnyezet előkészítése](service-fabric-get-started.md), elindíthatja a Service Fabric Explorer a helyi fürtön lévő toohttp://localhost:19080 navigálva / Explorer.</span><span class="sxs-lookup"><span data-stu-id="075e4-109">If you have followed hello instructions too[prepare your development environment](service-fabric-get-started.md), you can launch Service Fabric Explorer on your local cluster by navigating toohttp://localhost:19080/Explorer.</span></span>

## <a name="understand-hello-service-fabric-explorer-layout"></a><span data-ttu-id="075e4-110">Service Fabric Explorer elrendezés hello ismertetése</span><span class="sxs-lookup"><span data-stu-id="075e4-110">Understand hello Service Fabric Explorer layout</span></span>
<span data-ttu-id="075e4-111">Service Fabric Explorer bontható hello bal oldali fában hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="075e4-111">You can navigate through Service Fabric Explorer by using hello tree on hello left.</span></span> <span data-ttu-id="075e4-112">Hello fa hello gyökerében hello fürt irányítópult áttekintése a fürt, beleértve az alkalmazás és a csomópont állapotának összegzését.</span><span class="sxs-lookup"><span data-stu-id="075e4-112">At hello root of hello tree, hello cluster dashboard provides an overview of your cluster, including a summary of application and node health.</span></span>

![Service Fabric Explorer fürt irányítópult][sfx-cluster-dashboard]

### <a name="view-hello-clusters-layout"></a><span data-ttu-id="075e4-114">Hello fürt Elrendezés megtekintése</span><span class="sxs-lookup"><span data-stu-id="075e4-114">View hello cluster's layout</span></span>
<span data-ttu-id="075e4-115">A Service Fabric-fürt csomópontjának kerülnek, egy kétdimenziós rácsot a tartalék tartományok között, és a frissítési tartományt.</span><span class="sxs-lookup"><span data-stu-id="075e4-115">Nodes in a Service Fabric cluster are placed across a two-dimensional grid of fault domains and upgrade domains.</span></span> <span data-ttu-id="075e4-116">Az elhelyezési biztosítja, hogy az alkalmazások továbbra is elérhető, a hardver meghibásodása és alkalmazásfrissítések hello jelenlétét.</span><span class="sxs-lookup"><span data-stu-id="075e4-116">This placement ensures that your applications remain available in hello presence of hardware failures and application upgrades.</span></span> <span data-ttu-id="075e4-117">Hogyan hello a jelenlegi fürthöz van leírva hello betűcsoport-leképezés használatával tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="075e4-117">You can view how hello current cluster is laid out by using hello cluster map.</span></span>

![Service Fabric Explorer betűcsoport-leképezés][sfx-cluster-map]

### <a name="view-applications-and-services"></a><span data-ttu-id="075e4-119">Nézet alkalmazások és szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="075e4-119">View applications and services</span></span>
<span data-ttu-id="075e4-120">hello fürt két részfák tartalmaz: egyet az alkalmazások és más csomópontok.</span><span class="sxs-lookup"><span data-stu-id="075e4-120">hello cluster contains two subtrees: one for applications and another for nodes.</span></span>

<span data-ttu-id="075e4-121">Hello alkalmazás nézet toonavigate Service Fabric logikai hierarchia keresztül is használhatja: alkalmazások, szolgáltatások, partíciók és replikáit.</span><span class="sxs-lookup"><span data-stu-id="075e4-121">You can use hello application view toonavigate through Service Fabric's logical hierarchy: applications, services, partitions, and replicas.</span></span>

<span data-ttu-id="075e4-122">Hello az alábbi példában a hello alkalmazás **MyApp** két szolgáltatásból áll **MyStatefulService** és **WebService**.</span><span class="sxs-lookup"><span data-stu-id="075e4-122">In hello example below, hello application **MyApp** consists of two services, **MyStatefulService** and **WebService**.</span></span> <span data-ttu-id="075e4-123">Mivel a **MyStatefulService** állapotalapú, egy elsődleges és két másodlagos replika partíciót tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="075e4-123">Since **MyStatefulService** is stateful, it includes a partition with one primary and two secondary replicas.</span></span> <span data-ttu-id="075e4-124">Ezzel szemben WebSvcService állapot nélküli, és egyetlen példányt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="075e4-124">By contrast, WebSvcService is stateless and contains a single instance.</span></span>

![Service Fabric Explorer alkalmazás megtekintése][sfx-application-tree]

<span data-ttu-id="075e4-126">Hello fa egyes szintjein a fő ablaktáblán hello hello elem vonatkozó információkat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="075e4-126">At each level of hello tree, hello main pane shows pertinent information about hello item.</span></span> <span data-ttu-id="075e4-127">Például láthatja hello állapotát és egy adott szolgáltatáshoz verziója.</span><span class="sxs-lookup"><span data-stu-id="075e4-127">For example, you can see hello health status and version for a particular service.</span></span>

![Service Fabric Explorer essentials ablaktábla][sfx-service-essentials]

### <a name="view-hello-clusters-nodes"></a><span data-ttu-id="075e4-129">Hello fürt csomópontjai megtekintése</span><span class="sxs-lookup"><span data-stu-id="075e4-129">View hello cluster's nodes</span></span>
<span data-ttu-id="075e4-130">hello csomópont nézetben látható hello hello fürt fizikai elrendezését.</span><span class="sxs-lookup"><span data-stu-id="075e4-130">hello node view shows hello physical layout of hello cluster.</span></span> <span data-ttu-id="075e4-131">Az egyes csomópontoknál megtekintheti, hogy melyik alkalmazások kódja üzemel az adott csomóponton.</span><span class="sxs-lookup"><span data-stu-id="075e4-131">For a given node, you can inspect which applications have code deployed on that node.</span></span> <span data-ttu-id="075e4-132">Pontosabban láthatja, melyik replikák futó van.</span><span class="sxs-lookup"><span data-stu-id="075e4-132">More specifically, you can see which replicas are currently running there.</span></span>

## <a name="actions"></a><span data-ttu-id="075e4-133">Műveletek</span><span class="sxs-lookup"><span data-stu-id="075e4-133">Actions</span></span>
<span data-ttu-id="075e4-134">Service Fabric Explorer biztosít egy gyorsan tooinvoke műveleteket az egyes csomópontok, alkalmazások és szolgáltatások a fürtön belül.</span><span class="sxs-lookup"><span data-stu-id="075e4-134">Service Fabric Explorer offers a quick way tooinvoke actions on nodes, applications, and services within your cluster.</span></span>

<span data-ttu-id="075e4-135">Például toodelete egy alkalmazáspéldány hello fa hello bal oldali lehetőségek közül választhat hello alkalmazást, és válassza **műveletek** > **alkalmazás törlése**.</span><span class="sxs-lookup"><span data-stu-id="075e4-135">For example, toodelete an application instance, choose hello application from hello tree on hello left, and then choose **Actions** > **Delete Application**.</span></span>

![A Service Fabric Explorerben alkalmazás törlése][sfx-delete-application]

> [!TIP]
> <span data-ttu-id="075e4-137">Végezhet ugyanazokat a műveleteket hello tooeach elem következő hello három pont gombra kattintva.</span><span class="sxs-lookup"><span data-stu-id="075e4-137">You can perform hello same actions by clicking hello ellipsis next tooeach element.</span></span>
>
>

<span data-ttu-id="075e4-138">hello alábbi táblázat minden egyes entitásnál elérhető hello műveletek:</span><span class="sxs-lookup"><span data-stu-id="075e4-138">hello following table lists hello actions available for each entity:</span></span>

| <span data-ttu-id="075e4-139">**Entitás**</span><span class="sxs-lookup"><span data-stu-id="075e4-139">**Entity**</span></span> | <span data-ttu-id="075e4-140">**Művelet**</span><span class="sxs-lookup"><span data-stu-id="075e4-140">**Action**</span></span> | <span data-ttu-id="075e4-141">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="075e4-141">**Description**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="075e4-142">Alkalmazás típusa</span><span class="sxs-lookup"><span data-stu-id="075e4-142">Application type</span></span> |<span data-ttu-id="075e4-143">Típus leépítése</span><span class="sxs-lookup"><span data-stu-id="075e4-143">Unprovision type</span></span> |<span data-ttu-id="075e4-144">Hello alkalmazáscsomag eltávolítása hello fürt lemezképtárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="075e4-144">Removes hello application package from hello cluster's image store.</span></span> <span data-ttu-id="075e4-145">Adott típusú toobe előbb eltávolítja az összes alkalmazás szükséges.</span><span class="sxs-lookup"><span data-stu-id="075e4-145">Requires all applications of that type toobe removed first.</span></span> |
| <span data-ttu-id="075e4-146">Alkalmazás</span><span class="sxs-lookup"><span data-stu-id="075e4-146">Application</span></span> |<span data-ttu-id="075e4-147">Alkalmazás törlése</span><span class="sxs-lookup"><span data-stu-id="075e4-147">Delete Application</span></span> |<span data-ttu-id="075e4-148">Törli a hello alkalmazást, beleértve a hozzá tartozó szolgáltatások és azok állapota (ha van ilyen).</span><span class="sxs-lookup"><span data-stu-id="075e4-148">Delete hello application, including all its services and their state (if any).</span></span> |
| <span data-ttu-id="075e4-149">Szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="075e4-149">Service</span></span> |<span data-ttu-id="075e4-150">Szolgáltatás törlése</span><span class="sxs-lookup"><span data-stu-id="075e4-150">Delete Service</span></span> |<span data-ttu-id="075e4-151">(Ha van ilyen), törölje a hello szolgáltatást és annak állapotát.</span><span class="sxs-lookup"><span data-stu-id="075e4-151">Delete hello service and its state (if any).</span></span> |
| <span data-ttu-id="075e4-152">Csomópont</span><span class="sxs-lookup"><span data-stu-id="075e4-152">Node</span></span> |<span data-ttu-id="075e4-153">Aktiválás</span><span class="sxs-lookup"><span data-stu-id="075e4-153">Activate</span></span> |<span data-ttu-id="075e4-154">Hello csomópont aktiválása.</span><span class="sxs-lookup"><span data-stu-id="075e4-154">Activate hello node.</span></span> |
| <span data-ttu-id="075e4-155">Csomópont</span><span class="sxs-lookup"><span data-stu-id="075e4-155">Node</span></span> | <span data-ttu-id="075e4-156">(Szünet) inaktiválása</span><span class="sxs-lookup"><span data-stu-id="075e4-156">Deactivate (pause)</span></span> | <span data-ttu-id="075e4-157">A jelenlegi állapotában hello csomópont felfüggesztése.</span><span class="sxs-lookup"><span data-stu-id="075e4-157">Pause hello node in its current state.</span></span> <span data-ttu-id="075e4-158">Szolgáltatások továbbra is toorun, de a Service Fabric proaktív helyezi át a semmit, vagy ki, kivéve, ha ez szükséges tooprevent kimaradás vagy adatinkonzisztenciát.</span><span class="sxs-lookup"><span data-stu-id="075e4-158">Services continue toorun but Service Fabric does not proactively move anything onto or off it unless it is required tooprevent an outage or data inconsistency.</span></span> <span data-ttu-id="075e4-159">Ez a művelet akkor általánosan használt tooenable szolgáltatásokat egy adott csomópont tooensure, hogy azok helyezi át az ellenőrzés során a hibakeresést.</span><span class="sxs-lookup"><span data-stu-id="075e4-159">This action is typically used tooenable debugging services on a specific node tooensure that they do not move during inspection.</span></span> | |
| <span data-ttu-id="075e4-160">Csomópont</span><span class="sxs-lookup"><span data-stu-id="075e4-160">Node</span></span> | <span data-ttu-id="075e4-161">Inaktiválás (újraindítás)</span><span class="sxs-lookup"><span data-stu-id="075e4-161">Deactivate (restart)</span></span> | <span data-ttu-id="075e4-162">Biztonságosan haladhat minden memórián belüli szolgáltatás ki olyan csomópont, és zárja be az állandó szolgáltatásokban.</span><span class="sxs-lookup"><span data-stu-id="075e4-162">Safely move all in-memory services off a node and close persistent services.</span></span> <span data-ttu-id="075e4-163">Jellemzően hello gazdafolyamat vagy a gép kell toobe újraindításakor.</span><span class="sxs-lookup"><span data-stu-id="075e4-163">Typically used when hello host processes or machine need toobe restarted.</span></span> | |
| <span data-ttu-id="075e4-164">Csomópont</span><span class="sxs-lookup"><span data-stu-id="075e4-164">Node</span></span> | <span data-ttu-id="075e4-165">(Az adatok eltávolítása) inaktiválása</span><span class="sxs-lookup"><span data-stu-id="075e4-165">Deactivate (remove data)</span></span> | <span data-ttu-id="075e4-166">Biztonságosan zárja be a megfelelő tartalék replikák létrehozása után hello csomóponton futó összes szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="075e4-166">Safely close all services running on hello node after building sufficient spare replicas.</span></span> <span data-ttu-id="075e4-167">Jellemzően akkor csomópont (vagy legalább a tárhely) kívül Bizottság véglegesen foglalja.</span><span class="sxs-lookup"><span data-stu-id="075e4-167">Typically used when a node (or at least its storage) is being permanently taken out of commission.</span></span> | |
| <span data-ttu-id="075e4-168">Csomópont</span><span class="sxs-lookup"><span data-stu-id="075e4-168">Node</span></span> | <span data-ttu-id="075e4-169">Távolítsa el a csomópont állapota</span><span class="sxs-lookup"><span data-stu-id="075e4-169">Remove node state</span></span> | <span data-ttu-id="075e4-170">Távolítsa el a Tudásbázis egy csomópont replikák hello fürtből.</span><span class="sxs-lookup"><span data-stu-id="075e4-170">Remove knowledge of a node's replicas from hello cluster.</span></span> <span data-ttu-id="075e4-171">Általában akkor használható, ha egy már leállt csomópont helyreállíthatatlan tekintendő.</span><span class="sxs-lookup"><span data-stu-id="075e4-171">Typically used when an already failed node is deemed unrecoverable.</span></span> | |
| <span data-ttu-id="075e4-172">Csomópont</span><span class="sxs-lookup"><span data-stu-id="075e4-172">Node</span></span> | <span data-ttu-id="075e4-173">Újraindítás</span><span class="sxs-lookup"><span data-stu-id="075e4-173">Restart</span></span> | <span data-ttu-id="075e4-174">Egy Csomóponthiba szimulálása hello csomópont újraindításával.</span><span class="sxs-lookup"><span data-stu-id="075e4-174">Simulate a node failure by restarting hello node.</span></span> <span data-ttu-id="075e4-175">További információ [Itt](/powershell/module/servicefabric/restart-servicefabricnode?view=azureservicefabricps)</span><span class="sxs-lookup"><span data-stu-id="075e4-175">More information [here](/powershell/module/servicefabric/restart-servicefabricnode?view=azureservicefabricps)</span></span> | |

<span data-ttu-id="075e4-176">Mivel számos műveletet károsak, akkor lehetnek kéri, a célt tooconfirm hello művelet befejezése előtt.</span><span class="sxs-lookup"><span data-stu-id="075e4-176">Since many actions are destructive, you may be asked tooconfirm your intent before hello action is completed.</span></span>

> [!TIP]
> <span data-ttu-id="075e4-177">Minden művelet, amely a Service Fabric Explorer segítségével végezheti el is PowerShell vagy a REST API-t tooenable automation végezhető el.</span><span class="sxs-lookup"><span data-stu-id="075e4-177">Every action that can be performed through Service Fabric Explorer can also be performed through PowerShell or a REST API, tooenable automation.</span></span>
>
>

<span data-ttu-id="075e4-178">Service Fabric Explorer toocreate alkalmazáspéldányok használ egy adott alkalmazás típusától és verziójától.</span><span class="sxs-lookup"><span data-stu-id="075e4-178">You can also use Service Fabric Explorer toocreate application instances for a given application type and version.</span></span> <span data-ttu-id="075e4-179">Hello alkalmazástípus válasszon hello faszerkezetes nézetben, majd kattintson a hello **app-példány létrehozása** hivatkozás toohello verzióra szeretné hello jobb oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="075e4-179">Choose hello application type in hello tree view, then click hello **Create app instance** link next toohello version you'd like in hello right pane.</span></span>

![Az alkalmazáspéldány létrehozása a Service Fabric Explorerben][sfx-create-app-instance]

> [!NOTE]
> <span data-ttu-id="075e4-181">Jelenleg nem paraméterezett Service Fabric Explorer használatával létrehozott alkalmazás-példányokon.</span><span class="sxs-lookup"><span data-stu-id="075e4-181">Application instances created through Service Fabric Explorer cannot currently be parameterized.</span></span> <span data-ttu-id="075e4-182">Létrehozásuk alapértelmezett paraméterértékek használata.</span><span class="sxs-lookup"><span data-stu-id="075e4-182">They are created using default parameter values.</span></span>
>
>

## <a name="connect-tooa-remote-service-fabric-cluster"></a><span data-ttu-id="075e4-183">Csatlakozás távoli tooa Service Fabric-fürt</span><span class="sxs-lookup"><span data-stu-id="075e4-183">Connect tooa remote Service Fabric cluster</span></span>
<span data-ttu-id="075e4-184">Ha tudja hello a fürt végpontja, és rendelkezik a szükséges engedélyekkel a Service Fabric Explorer minden böngészőből végezheti el.</span><span class="sxs-lookup"><span data-stu-id="075e4-184">If you know hello cluster's endpoint and have sufficient permissions you can access Service Fabric Explorer from any browser.</span></span> <span data-ttu-id="075e4-185">Ez azért, mert Service Fabric Explorer egy másik szolgáltatás, amely az hello fürtben.</span><span class="sxs-lookup"><span data-stu-id="075e4-185">This is because Service Fabric Explorer is just another service that runs in hello cluster.</span></span>

### <a name="discover-hello-service-fabric-explorer-endpoint-for-a-remote-cluster"></a><span data-ttu-id="075e4-186">Távoli fürt hello Service Fabric Explorer-végpont felderítése</span><span class="sxs-lookup"><span data-stu-id="075e4-186">Discover hello Service Fabric Explorer endpoint for a remote cluster</span></span>
<span data-ttu-id="075e4-187">tooreach Service Fabric Explorer egy adott fürtben, irányítsa a böngészőt, hogy:</span><span class="sxs-lookup"><span data-stu-id="075e4-187">tooreach Service Fabric Explorer for a given cluster, point your browser to:</span></span>

<span data-ttu-id="075e4-188">http://&lt;a fürt végpontja&gt;: 19080/Explorer</span><span class="sxs-lookup"><span data-stu-id="075e4-188">http://&lt;your-cluster-endpoint&gt;:19080/Explorer</span></span>

<span data-ttu-id="075e4-189">Az Azure-fürtök esetén hello teljes URL-címet is rendelkezésre áll a hello fürt essentials ablaktábláján hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="075e4-189">For Azure clusters, hello full URL is also available in hello cluster essentials pane of hello Azure portal.</span></span>

### <a name="connect-tooa-secure-cluster"></a><span data-ttu-id="075e4-190">Csatlakoztassa tooa biztonságos fürtöt</span><span class="sxs-lookup"><span data-stu-id="075e4-190">Connect tooa secure cluster</span></span>
<span data-ttu-id="075e4-191">Tanúsítványok, valamint az Azure Active Directory (AAD) használó ügyfél hozzáférési tooyour Service Fabric fürt szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="075e4-191">You can control client access tooyour Service Fabric cluster either with certificates or using Azure Active Directory (AAD).</span></span>

<span data-ttu-id="075e4-192">Tooconnect tooService Fabric Explorer egy biztonságos fürtön kísérel meg, majd hello fürt konfigurációjától függően lesz szükséges toopresent ügyféltanúsítvánnyal kell, illetve az AAD használja-e jelentkezni.</span><span class="sxs-lookup"><span data-stu-id="075e4-192">If you attempt tooconnect tooService Fabric Explorer on a secure cluster, then depending on hello cluster's configuration you'll be required toopresent a client certificate or log in using AAD.</span></span>

## <a name="next-steps"></a><span data-ttu-id="075e4-193">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="075e4-193">Next steps</span></span>
* [<span data-ttu-id="075e4-194">Tesztelhetőségi áttekintése</span><span class="sxs-lookup"><span data-stu-id="075e4-194">Testability overview</span></span>](service-fabric-testability-overview.md)
* [<span data-ttu-id="075e4-195">A Visual Studio a Service Fabric-alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="075e4-195">Managing your Service Fabric applications in Visual Studio</span></span>](service-fabric-manage-application-in-visual-studio.md)
* [<span data-ttu-id="075e4-196">A Service Fabric-alkalmazás központi telepítésének PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="075e4-196">Service Fabric application deployment using PowerShell</span></span>](service-fabric-deploy-remove-applications.md)

<!--Image references-->
[sfx-cluster-dashboard]: ./media/service-fabric-visualizing-your-cluster/SfxClusterDashboard.png
[sfx-cluster-map]: ./media/service-fabric-visualizing-your-cluster/SfxClusterMap.png
[sfx-application-tree]: ./media/service-fabric-visualizing-your-cluster/SfxApplicationTree.png
[sfx-service-essentials]: ./media/service-fabric-visualizing-your-cluster/SfxServiceEssentials.png
[sfx-delete-application]: ./media/service-fabric-visualizing-your-cluster/SfxDeleteApplication.png
[sfx-create-app-instance]: ./media/service-fabric-visualizing-your-cluster/SfxCreateAppInstance.png
