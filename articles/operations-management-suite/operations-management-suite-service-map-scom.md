---
title: "Térkép integráció a System Center Operations Manager aaaService |} Microsoft Docs"
description: "Szolgáltatástérkép Operations Management Suite megoldás, amely automatikusan felderíti az alkalmazás-összetevők a Windows és Linux rendszerek és a maps hello szolgáltatások közötti kommunikáció. Ez a cikk ismerteti a Szolgáltatástérkép használatával tooautomatically elosztott alkalmazás diagramokat hozhat létre az Operations Manager programban."
services: operations-management-suite
documentationcenter: 
author: daveirwin1
manager: jwhit
editor: tysonn
ms.assetid: e8614a5a-9cf8-4c81-8931-896d358ad2cb
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/21/2017
ms.author: bwren;dairwin
ms.openlocfilehash: cff9cce2559448ec3a5fd14087b867f314716560
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-map-integration-with-system-center-operations-manager"></a><span data-ttu-id="679bd-104">Szolgáltatástérkép integráció a System Center Operations Manager</span><span class="sxs-lookup"><span data-stu-id="679bd-104">Service Map integration with System Center Operations Manager</span></span>
  > [!NOTE]
  > <span data-ttu-id="679bd-105">A funkció jelenleg nyilvános előzetes verziójához.</span><span class="sxs-lookup"><span data-stu-id="679bd-105">This feature is in public preview.</span></span>
  > 
  
<span data-ttu-id="679bd-106">Operations Management Suite Szolgáltatástérkép automatikusan észleli a Windows és Linux rendszerek alkalmazás-összetevők és társít hello kommunikációs szolgáltatások között.</span><span class="sxs-lookup"><span data-stu-id="679bd-106">Operations Management Suite Service Map automatically discovers application components on Windows and Linux systems and maps hello communication between services.</span></span> <span data-ttu-id="679bd-107">Szolgáltatástérkép lehetővé teszi a kiszolgálók hello módon úgy gondolja, hogy azok összekapcsolt rendszerekhez, hogy a kritikus szolgáltatások tooview.</span><span class="sxs-lookup"><span data-stu-id="679bd-107">Service Map allows you tooview your servers hello way you think of them, as interconnected systems that deliver critical services.</span></span> <span data-ttu-id="679bd-108">Szolgáltatástérkép kiszolgálók, folyamatok és portok közötti kapcsolatok között bármely TCP-csatlakoztatott architektúra, nem szükséges ügynököt telepíteni hello mellett konfigurációval jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="679bd-108">Service Map shows connections between servers, processes, and ports across any TCP-connected architecture, with no configuration required besides hello installation of an agent.</span></span> <span data-ttu-id="679bd-109">További információkért lásd: hello [Szolgáltatástérkép dokumentáció](operations-management-suite-service-map.md).</span><span class="sxs-lookup"><span data-stu-id="679bd-109">For more information, see hello [Service Map documentation](operations-management-suite-service-map.md).</span></span>

<span data-ttu-id="679bd-110">Ez az integráció Szolgáltatástérkép és a System Center Operations Manager között automatikusan az Operations Manager programban, a Service Map hello dinamikus függőségi maps alapuló hozhat létre elosztott alkalmazás diagramokat.</span><span class="sxs-lookup"><span data-stu-id="679bd-110">With this integration between Service Map and System Center Operations Manager, you can automatically create distributed application diagrams in Operations Manager that are based on hello dynamic dependency maps in Service Map.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="679bd-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="679bd-111">Prerequisites</span></span>
* <span data-ttu-id="679bd-112">Az Operations Manager felügyeleti csoport, amely a kiszolgálók egy csoportja kezel.</span><span class="sxs-lookup"><span data-stu-id="679bd-112">An Operations Manager management group that is managing a set of servers.</span></span>
* <span data-ttu-id="679bd-113">Az Operations Management Suite munkaterület hello Szolgáltatástérkép megoldás.</span><span class="sxs-lookup"><span data-stu-id="679bd-113">An Operations Management Suite workspace with hello Service Map solution enabled.</span></span>
* <span data-ttu-id="679bd-114">Kiszolgálók egy csoportja (legalább egyet), amely az Operations Manager és a küldő adatok tooService térkép felügyeli.</span><span class="sxs-lookup"><span data-stu-id="679bd-114">A set of servers (at least one) that are being managed by Operations Manager and sending data tooService Map.</span></span> <span data-ttu-id="679bd-115">A Windows és Linux-kiszolgálókon támogatott.</span><span class="sxs-lookup"><span data-stu-id="679bd-115">Windows and Linux servers are supported.</span></span>
* <span data-ttu-id="679bd-116">A szolgáltatás egyszerű hozzáférés toohello Azure-előfizetéssel társított hello Operations Management Suite-munkaterülettel.</span><span class="sxs-lookup"><span data-stu-id="679bd-116">A service principal with access toohello Azure subscription that is associated with hello Operations Management Suite workspace.</span></span> <span data-ttu-id="679bd-117">További információ: túl[szolgáltatásnevet létrehozni](#creating-a-service-principal).</span><span class="sxs-lookup"><span data-stu-id="679bd-117">For more information, go too[Create a service principal](#creating-a-service-principal).</span></span>

## <a name="install-hello-service-map-management-pack"></a><span data-ttu-id="679bd-118">Hello Szolgáltatástérkép felügyeleti csomag telepítéséhez</span><span class="sxs-lookup"><span data-stu-id="679bd-118">Install hello Service Map management pack</span></span>
<span data-ttu-id="679bd-119">Hello Microsoft.SystemCenter.ServiceMap felügyeleticsomag-nyaláb (Microsoft.SystemCenter.ServiceMap.mpb) importálása az Operations Manager és a Service Map hello integrációjával engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="679bd-119">You enable hello integration between Operations Manager and Service Map by importing hello Microsoft.SystemCenter.ServiceMap management pack bundle (Microsoft.SystemCenter.ServiceMap.mpb).</span></span> <span data-ttu-id="679bd-120">hello köteg hello a következő felügyeleti csomagokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="679bd-120">hello bundle contains hello following management packs:</span></span>
* <span data-ttu-id="679bd-121">A Microsoft szolgáltatás térkép alkalmazás nézetek</span><span class="sxs-lookup"><span data-stu-id="679bd-121">Microsoft Service Map Application Views</span></span>
* <span data-ttu-id="679bd-122">A Microsoft System Center Service Map belső</span><span class="sxs-lookup"><span data-stu-id="679bd-122">Microsoft System Center Service Map Internal</span></span>
* <span data-ttu-id="679bd-123">A Microsoft System Center Service Map felülbírálások</span><span class="sxs-lookup"><span data-stu-id="679bd-123">Microsoft System Center Service Map Overrides</span></span>
* <span data-ttu-id="679bd-124">A Microsoft System Center Service Map</span><span class="sxs-lookup"><span data-stu-id="679bd-124">Microsoft System Center Service Map</span></span>

## <a name="configure-hello-service-map-integration"></a><span data-ttu-id="679bd-125">Hello Szolgáltatástérkép-integráció konfigurálása</span><span class="sxs-lookup"><span data-stu-id="679bd-125">Configure hello Service Map integration</span></span>
<span data-ttu-id="679bd-126">Hello Szolgáltatástérkép felügyeleti csomagot, egy új csomópont telepítése után **Szolgáltatástérkép**, jelenik meg **Operations Management Suite** a hello **felügyeleti** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="679bd-126">After you install hello Service Map management pack, a new node, **Service Map**, is displayed under **Operations Management Suite** in hello **Administration** pane.</span></span> 

<span data-ttu-id="679bd-127">Szolgáltatástérkép integrációs tooconfigure hello a következő:</span><span class="sxs-lookup"><span data-stu-id="679bd-127">tooconfigure Service Map integration, do hello following:</span></span>

1. <span data-ttu-id="679bd-128">tooopen hello konfigurációs varázsló hello **szolgáltatás – áttekintés** ablaktáblán kattintson a **munkaterület hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="679bd-128">tooopen hello configuration wizard, in hello **Service Map Overview** pane, click **Add workspace**.</span></span>  

    ![Szolgáltatás – áttekintés ablaktáblán](media/oms-service-map/scom-configuration.png)

2. <span data-ttu-id="679bd-130">A hello **kapcsolat konfigurációs** ablak, írja be a hello bérlő neve vagy azonosítója, azonosítója (más néven a hello felhasználónév vagy a clientID) és hello szolgáltatás egyszerű jelszót, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="679bd-130">In hello **Connection Configuration** window, enter hello tenant name or ID, application ID (also known as hello username or clientID), and password of hello service principal, and then click **Next**.</span></span> <span data-ttu-id="679bd-131">További információ: túl[szolgáltatásnevet létrehozni](#creating-a-service-principal).</span><span class="sxs-lookup"><span data-stu-id="679bd-131">For more information, go too[Create a service principal](#creating-a-service-principal).</span></span>

    ![hello kapcsolat konfigurációs ablak](media/oms-service-map/scom-config-spn.png)

3. <span data-ttu-id="679bd-133">A hello **előfizetés kijelölés** ablakban jelölje ki a hello Azure-előfizetéssel, Azure erőforráscsoport (hello Operations Management Suite-munkaterülettel tartalmazó hello) és az Operations Management Suite-munkaterület, és kattintson **Következő**.</span><span class="sxs-lookup"><span data-stu-id="679bd-133">In hello **Subscription Selection** window, select hello Azure subscription, Azure resource group (hello one that contains hello Operations Management Suite workspace), and Operations Management Suite workspace, and then click **Next**.</span></span>

    ![Operations Manager konfigurációs munkaterület hello](media/oms-service-map/scom-config-workspace.png)

4. <span data-ttu-id="679bd-135">A hello **gép Csoportkijelölés** melyik szolgáltatás térkép gép csoportok kiválasztása ablakban azt szeretné, hogy toosync tooOperations Manager.</span><span class="sxs-lookup"><span data-stu-id="679bd-135">In hello **Machine Group Selection** window, you choose which Service Map Machine Groups you want toosync tooOperations Manager.</span></span> <span data-ttu-id="679bd-136">Kattintson a **hozzáadása gép csoportok**, hello listájából válassza ki a csoportok **elérhető számítógép-csoportok**, és kattintson a **hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="679bd-136">Click **Add/Remove Machine Groups**, choose groups from hello list of **Available Machine Groups**, and click **Add**.</span></span>  <span data-ttu-id="679bd-137">Ha befejezte a csoportok kiválasztásával, kattintson a **Ok** toofinish.</span><span class="sxs-lookup"><span data-stu-id="679bd-137">When you are finished selecting groups, click **Ok** toofinish.</span></span>
    
    ![az Operations Manager konfigurációs gép csoportok hello](media/oms-service-map/scom-config-machine-groups.png)
    
5. <span data-ttu-id="679bd-139">A hello **kiszolgáló kiválasztása** ablakban konfigurálnia hello szolgáltatás térkép kiszolgálók csoport, amelyet az Operations Manager és a Service Map közötti toosync hello kiszolgálókkal.</span><span class="sxs-lookup"><span data-stu-id="679bd-139">In hello **Server Selection** window, you configure hello Service Map Servers Group with hello servers that you want toosync between Operations Manager and Service Map.</span></span> <span data-ttu-id="679bd-140">Kattintson a **kiszolgálók hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="679bd-140">Click **Add/Remove Servers**.</span></span>   
    
    <span data-ttu-id="679bd-141">Hello integrációs toobuild egy elosztott alkalmazás diagram kiszolgáló, a hello kiszolgálónak kell lennie:</span><span class="sxs-lookup"><span data-stu-id="679bd-141">For hello integration toobuild a distributed application diagram for a server, hello server must be:</span></span>

    * <span data-ttu-id="679bd-142">Operations Manager által felügyelt</span><span class="sxs-lookup"><span data-stu-id="679bd-142">Managed by Operations Manager</span></span>
    * <span data-ttu-id="679bd-143">Szolgáltatástérkép kezeli</span><span class="sxs-lookup"><span data-stu-id="679bd-143">Managed by Service Map</span></span>
    * <span data-ttu-id="679bd-144">Hello szolgáltatás térkép kiszolgálók csoport szerepel</span><span class="sxs-lookup"><span data-stu-id="679bd-144">Listed in hello Service Map Servers Group</span></span>

    ![Operations Manager konfigurációs csoport hello](media/oms-service-map/scom-config-group.png)

6. <span data-ttu-id="679bd-146">Választható lehetőség: Hello felügyeleti kiszolgáló erőforrás készlet toocommunicate Operations Management Suite válassza ki, és kattintson **hozzáadása munkaterület**.</span><span class="sxs-lookup"><span data-stu-id="679bd-146">Optional: Select hello Management Server resource pool toocommunicate with Operations Management Suite, and then click **Add Workspace**.</span></span>

    ![az Operations Manager konfigurációs erőforráskészlet hello](media/oms-service-map/scom-config-pool.png)

    <span data-ttu-id="679bd-148">Előfordulhat, hogy igénybe vehet egy perc tooconfigure és regisztrálása hello Operations Management Suite-munkaterülettel.</span><span class="sxs-lookup"><span data-stu-id="679bd-148">It might take a minute tooconfigure and register hello Operations Management Suite workspace.</span></span> <span data-ttu-id="679bd-149">Beállítások konfigurálása után az Operations Manager indít el az Operations Management Suite hello első Szolgáltatástérkép szinkronizálás.</span><span class="sxs-lookup"><span data-stu-id="679bd-149">After it is configured, Operations Manager initiates hello first Service Map sync from Operations Management Suite.</span></span>

    ![az Operations Manager konfigurációs erőforráskészlet hello](media/oms-service-map/scom-config-success.png)


## <a name="monitor-service-map"></a><span data-ttu-id="679bd-151">A figyelő Szolgáltatástérkép</span><span class="sxs-lookup"><span data-stu-id="679bd-151">Monitor Service Map</span></span>
<span data-ttu-id="679bd-152">Az Operations Management Suite-munkaterülettel hello csatlakoztatása után jelenik meg egy új mappát, a Service Map hello **figyelés** hello Operations Manager konzol ablaktáblájában.</span><span class="sxs-lookup"><span data-stu-id="679bd-152">After hello Operations Management Suite workspace is connected, a new folder, Service Map, is displayed in hello **Monitoring** pane of hello Operations Manager console.</span></span>

![hello az Operations Manager figyelés ablaktáblán](media/oms-service-map/scom-monitoring.png)

<span data-ttu-id="679bd-154">hello Szolgáltatástérkép mappa négy csomópont rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="679bd-154">hello Service Map folder has four nodes:</span></span>
* <span data-ttu-id="679bd-155">**Aktív riasztások**: minden hello aktív kapcsolatos riasztásokat az Operations Manager és a Service Map hello kommunikációját sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="679bd-155">**Active Alerts**: Lists all hello active alerts about hello communication between Operations Manager and Service Map.</span></span>  <span data-ttu-id="679bd-156">Ne feledje, hogy ezek a riasztások nem szinkronizált tooOperations Manager alatt Operations Management Suite-riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="679bd-156">Note that these alerts are not Operations Management Suite alerts being synced tooOperations Manager.</span></span> 

* <span data-ttu-id="679bd-157">**Kiszolgálók**: listák hello figyelt kiszolgálók, amelyek a Service Map toosync konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="679bd-157">**Servers**: Lists hello monitored servers that are configured toosync from Service Map.</span></span>

    ![hello Operations Manager figyelési kiszolgálók panel](media/oms-service-map/scom-monitoring-servers.png)

* <span data-ttu-id="679bd-159">**Számítógép-függőség Csoportnézeteket**: a Service Map szinkronizált összes számítógép-csoportok listája.</span><span class="sxs-lookup"><span data-stu-id="679bd-159">**Machine Group Dependency Views**: Lists all machine groups that are synced from Service Map.</span></span> <span data-ttu-id="679bd-160">Bármely csoport tooview az elosztott alkalmazás diagram gombra.</span><span class="sxs-lookup"><span data-stu-id="679bd-160">You can click any group tooview its distributed application diagram.</span></span>

    ![hello Operations Manager elosztott alkalmazás diagramja](media/oms-service-map/scom-group-dad.png)

* <span data-ttu-id="679bd-162">**Kiszolgáló függőségi nézetek**: minden olyan kiszolgáló, a Service Map szinkronizált sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="679bd-162">**Server Dependency Views**: Lists all servers that are synced from Service Map.</span></span> <span data-ttu-id="679bd-163">Bármely kiszolgáló tooview az elosztott alkalmazás diagram gombra.</span><span class="sxs-lookup"><span data-stu-id="679bd-163">You can click any server tooview its distributed application diagram.</span></span>

    ![hello Operations Manager elosztott alkalmazás diagramja](media/oms-service-map/scom-dad.png)

## <a name="edit-or-delete-hello-workspace"></a><span data-ttu-id="679bd-165">Szerkesztheti és törölheti a hello munkaterület</span><span class="sxs-lookup"><span data-stu-id="679bd-165">Edit or delete hello workspace</span></span>
<span data-ttu-id="679bd-166">Ön módosíthatja és törölheti a konfigurált hello munkaterületen hello **szolgáltatás – áttekintés** ablaktáblán (**felügyeleti** ablaktábla > **Operations Management Suite**  >  **Térkép szolgáltatás**).</span><span class="sxs-lookup"><span data-stu-id="679bd-166">You can edit or delete hello configured workspace through hello **Service Map Overview** pane (**Administration** pane > **Operations Management Suite** > **Service Map**).</span></span> <span data-ttu-id="679bd-167">Csak egy Operations Management Suite-munkaterület most konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="679bd-167">You can configure only one Operations Management Suite workspace for now.</span></span>

![hello Operations Manager szerkesztése munkaterület ablaktáblán](media/oms-service-map/scom-edit-workspace.png)

## <a name="configure-rules-and-overrides"></a><span data-ttu-id="679bd-169">Szabályok és felülbírálások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="679bd-169">Configure rules and overrides</span></span>
<span data-ttu-id="679bd-170">A szabály _Microsoft.SystemCenter.ServiceMapImport.Rule_, Szolgáltatástérkép tooperiodically fetch információk készült.</span><span class="sxs-lookup"><span data-stu-id="679bd-170">A rule, _Microsoft.SystemCenter.ServiceMapImport.Rule_, is created tooperiodically fetch information from Service Map.</span></span> <span data-ttu-id="679bd-171">toochange szinkronizálási időzítést, a felülbírálások hello szabály konfigurálhatja (**szerzői műveletek** ablaktábla > **szabályok** > **Microsoft.SystemCenter.ServiceMapImport.Rule**).</span><span class="sxs-lookup"><span data-stu-id="679bd-171">toochange sync timings, you can configure overrides of hello rule (**Authoring** pane > **Rules** > **Microsoft.SystemCenter.ServiceMapImport.Rule**).</span></span>

![hello Operations Manager felülbírálások tulajdonságai ablakban](media/oms-service-map/scom-overrides.png)

* <span data-ttu-id="679bd-173">**Engedélyezett**: engedélyezheti vagy tilthatja le az automatikus frissítések.</span><span class="sxs-lookup"><span data-stu-id="679bd-173">**Enabled**: Enable or disable automatic updates.</span></span> 
* <span data-ttu-id="679bd-174">**IntervalMinutes**: hello idő közötti frissítések alaphelyzetbe.</span><span class="sxs-lookup"><span data-stu-id="679bd-174">**IntervalMinutes**: Reset hello time between updates.</span></span> <span data-ttu-id="679bd-175">hello alapértelmezett program óránként.</span><span class="sxs-lookup"><span data-stu-id="679bd-175">hello default interval is one hour.</span></span> <span data-ttu-id="679bd-176">Ha gyakrabban toosync server maps, hello érték módosítására képes.</span><span class="sxs-lookup"><span data-stu-id="679bd-176">If you want toosync server maps more frequently, you can change hello value.</span></span>
* <span data-ttu-id="679bd-177">**TimeoutSeconds**: alaphelyzetbe hello mennyi idő után hello kérelem időkorlátja lejár.</span><span class="sxs-lookup"><span data-stu-id="679bd-177">**TimeoutSeconds**: Reset hello length of time before hello request times out.</span></span> 
* <span data-ttu-id="679bd-178">**TimeWindowMinutes**: hello idő az ablak-visszaállítási adatok lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="679bd-178">**TimeWindowMinutes**: Reset hello time window for querying data.</span></span> <span data-ttu-id="679bd-179">Alapértelmezett érték 60 perc ablak.</span><span class="sxs-lookup"><span data-stu-id="679bd-179">Default is a 60-minute window.</span></span> <span data-ttu-id="679bd-180">hello maximális Szolgáltatástérkép által engedélyezett értéke 60 perc.</span><span class="sxs-lookup"><span data-stu-id="679bd-180">hello maximum value allowed by Service Map is 60 minutes.</span></span>

## <a name="known-issues-and-limitations"></a><span data-ttu-id="679bd-181">Ismert problémák és korlátozások</span><span class="sxs-lookup"><span data-stu-id="679bd-181">Known issues and limitations</span></span>

<span data-ttu-id="679bd-182">hello aktuális terv megadja hello következő problémák és korlátozások:</span><span class="sxs-lookup"><span data-stu-id="679bd-182">hello current design presents hello following issues and limitations:</span></span>
* <span data-ttu-id="679bd-183">Csak kapcsolódhatnak tooa egyetlen Operations Management Suite-munkaterülettel.</span><span class="sxs-lookup"><span data-stu-id="679bd-183">You can only connect tooa single Operations Management Suite workspace.</span></span>
* <span data-ttu-id="679bd-184">Bár hozzáadhatja a kiszolgálók toohello térkép kiszolgálók szolgáltatáscsoport manuálisan keresztül hello **szerzői műveletek** ablaktáblán, az ezeken a kiszolgálókon hello maps azonnal nem lett szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="679bd-184">Although you can add servers toohello Service Map Servers Group manually through hello **Authoring** pane, hello maps for those servers are not synced immediately.</span></span>  <span data-ttu-id="679bd-185">Ezek lesznek szinkronizálva a Szolgáltatástérkép hello következő szinkronizálási ciklusban során.</span><span class="sxs-lookup"><span data-stu-id="679bd-185">They will be synced from Service Map during hello next sync cycle.</span></span>
* <span data-ttu-id="679bd-186">Ha végzett módosításokat toohello elosztott alkalmazás diagramok hello felügyeleti csomag által létrehozott, ezeket a módosításokat valószínűleg felülírja a Szolgáltatástérkép hello következő szinkronban.</span><span class="sxs-lookup"><span data-stu-id="679bd-186">If you make any changes toohello Distributed Application Diagrams created by hello management pack, those changes will likely be overwritten on hello next sync with Service Map.</span></span>

## <a name="create-a-service-principal"></a><span data-ttu-id="679bd-187">Egyszerű szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="679bd-187">Create a service principal</span></span>
<span data-ttu-id="679bd-188">Hivatalos Azure dokumentációjában az egyszerű szolgáltatás létrehozása lásd:</span><span class="sxs-lookup"><span data-stu-id="679bd-188">For official Azure documentation about creating a service principal, see:</span></span>
* [<span data-ttu-id="679bd-189">Egy egyszerű szolgáltatás létrehozása a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="679bd-189">Create a service principal by using PowerShell</span></span>](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [<span data-ttu-id="679bd-190">Hozzon létre egy egyszerű Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="679bd-190">Create a service principal by using Azure CLI</span></span>](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal-cli)
* [<span data-ttu-id="679bd-191">Hozzon létre egy egyszerű hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="679bd-191">Create a service principal by using hello Azure portal</span></span>](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal)

### <a name="feedback"></a><span data-ttu-id="679bd-192">Visszajelzés</span><span class="sxs-lookup"><span data-stu-id="679bd-192">Feedback</span></span>
<span data-ttu-id="679bd-193">Van olyan visszajelzést az USA Szolgáltatástérkép és a jelen dokumentáció?</span><span class="sxs-lookup"><span data-stu-id="679bd-193">Do you have any feedback for us about Service Map or this documentation?</span></span> <span data-ttu-id="679bd-194">Látogasson el a [felhasználói véleményekkel foglalkozó weblapunkra](https://feedback.azure.com/forums/267889-log-analytics/category/184492-service-map), ahol felajánlja a szolgáltatások és arra való szavazás a meglévő javaslatok.</span><span class="sxs-lookup"><span data-stu-id="679bd-194">Visit our [User Voice page](https://feedback.azure.com/forums/267889-log-analytics/category/184492-service-map), where you can suggest features or vote on existing suggestions.</span></span>
