---
title: "az Azure CDN aaaLog elemzése |} Microsoft Docs"
description: "Ügyfél engedélyezheti a webhelynapló elemzése Azure CDN szolgáltatás használata."
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 95e18b3c-b987-46c2-baa8-a27a029e3076
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: v-semcev
ms.openlocfilehash: 56e5a4fec46fd156cf38252732afb4522741d009
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="diagnostics-logs-for-azure-cdn"></a><span data-ttu-id="d37d1-103">Az Azure CDN diagnosztikai naplókat</span><span class="sxs-lookup"><span data-stu-id="d37d1-103">Diagnostics Logs for Azure CDN</span></span>

<span data-ttu-id="d37d1-104">Miután engedélyezte a CDN az alkalmazáshoz, akkor lesz valószínűleg toomonitor hello CDN használati szeretné a szállítási hello állapotának ellenőrzése és lehetséges problémák elhárítása.</span><span class="sxs-lookup"><span data-stu-id="d37d1-104">After enabling CDN for your application, you will likely want toomonitor hello CDN usage, check hello health of your delivery, and troubleshoot potential issues.</span></span> <span data-ttu-id="d37d1-105">Az Azure CDN további olyan funkciókat kínál az [CDN egyszerűsített analitika](cdn-analyze-usage-patterns.md) és [diagnosztikai naplók](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)</span><span class="sxs-lookup"><span data-stu-id="d37d1-105">Azure CDN provides these capabilities with [CDN Core Analytics](cdn-analyze-usage-patterns.md) and [Diagnostic Logs](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)</span></span>

## <a name="cdn-core-analytics"></a><span data-ttu-id="d37d1-106">CDN egyszerűsített analitika</span><span class="sxs-lookup"><span data-stu-id="d37d1-106">CDN Core Analytics</span></span>
<span data-ttu-id="d37d1-107">Aktuális Azure CDN rendelkező felhasználóként Verizon standard vagy prémium profilhoz akkor még képes tooview egyszerűsített analitika hello kiegészítő portálon hello "Kezelése" lehetőséget a hello Azure-portálon keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="d37d1-107">As a current Azure CDN user with Verizon standard or premium profile, you are already able tooview core analytics in hello supplemental portal accessible via hello "Manage" option from hello Azure portal.</span></span> 

## <a name="azure-diagnostic-logs"></a><span data-ttu-id="d37d1-108">Az Azure diagnosztikai naplók</span><span class="sxs-lookup"><span data-stu-id="d37d1-108">Azure Diagnostic Logs</span></span>

<span data-ttu-id="d37d1-109">Az új szolgáltatással az Azure, most már megtekintheti egyszerűsített analitika és mentse el egy vagy több célt, beleértve azokat:</span><span class="sxs-lookup"><span data-stu-id="d37d1-109">Azure With this new feature, you can now view core analytics and save them into one or more destinations including:</span></span>

 - <span data-ttu-id="d37d1-110">Azure Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="d37d1-110">Azure Storage account</span></span>
 - <span data-ttu-id="d37d1-111">Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="d37d1-111">Azure Event Hubs</span></span>
 - [<span data-ttu-id="d37d1-112">A Naplóelemzési OMS-tárház</span><span class="sxs-lookup"><span data-stu-id="d37d1-112">OMS Log Analytics repository</span></span>](https://docs.microsoft.com/azure/log-analytics/log-analytics-get-started)
 
 <span data-ttu-id="d37d1-113">Ez a szolgáltatás az összes CDN-végpontok tooVerizon (Standard és prémium) és a CDN-profilra Akamai (általános) tartozó érhető el.</span><span class="sxs-lookup"><span data-stu-id="d37d1-113">This feature is available for all CDN endpoints belonging tooVerizon (Standard & Premium) and Akamai (Standard) CDN Profiles.</span></span>

<span data-ttu-id="d37d1-114">Diagnosztikai naplók lehetővé teszik a CDN végpont tooa különböző forrásokból származó tooexport alapvető a szoftverhasználati mérési adatok, hogy tudja felhasználni azokat testreszabásához.</span><span class="sxs-lookup"><span data-stu-id="d37d1-114">Diagnostics logs allow you tooexport basic usage metrics from your CDN endpoint tooa variety of sources so that you can consume them in a customized way.</span></span> <span data-ttu-id="d37d1-115">Megteheti például hello a következő típusú adatok exportálása:</span><span class="sxs-lookup"><span data-stu-id="d37d1-115">For example, you can do hello following types of data export:</span></span>

- <span data-ttu-id="d37d1-116">Adattárolás tooblob exportálása, tooCSV exportálása és diagramjait létrehozása Excelben.</span><span class="sxs-lookup"><span data-stu-id="d37d1-116">Export data tooblob storage, export tooCSV, and generate graphs in excel.</span></span>
- <span data-ttu-id="d37d1-117">Adatok tooevent hubok exportálja, és más azure-szolgáltatásokkal együtt összefüggéseket.</span><span class="sxs-lookup"><span data-stu-id="d37d1-117">Export data tooevent hubs and correlate with data from other azure services.</span></span>
- <span data-ttu-id="d37d1-118">Adatok toolog elemzés és tekintse meg a saját OMS-munkaterület az adatok exportálása</span><span class="sxs-lookup"><span data-stu-id="d37d1-118">Export data toolog analytics and view data in your own OMS work space</span></span>

<span data-ttu-id="d37d1-119">hello alábbi ábrán egy tipikus CDN egyszerűsített analitika nézet adatokká.</span><span class="sxs-lookup"><span data-stu-id="d37d1-119">hello following figure shows a typical CDN Core Analytics view into data.</span></span>

![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/01_OMS-workspace.png)

<span data-ttu-id="d37d1-121">*1. ábra – CDN egyszerűsített analitika megtekintése*</span><span class="sxs-lookup"><span data-stu-id="d37d1-121">*Figure 1 - CDN Core Analytics view*</span></span>

<span data-ttu-id="d37d1-122">a következő forgatókönyv hello végighalad hello sémája hello alapvető analitikai adatok, hello szolgáltatás engedélyezése és azok toovarious célok számítógépeket, és fel a helyre lépéseit.</span><span class="sxs-lookup"><span data-stu-id="d37d1-122">hello following walkthrough goes through hello schema of hello core analytics data, steps involved in enabling hello feature and delivering them toovarious destinations, and consuming from these destinations.</span></span>

## <a name="enable-logging-with-azure-portal"></a><span data-ttu-id="d37d1-123">Az Azure-portálon naplózásának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="d37d1-123">Enable logging with Azure portal</span></span>

> [!NOTE]
> <span data-ttu-id="d37d1-124">hello diagnosztikai naplók vannak kapcsolva **ki** alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="d37d1-124">hello diagnostics logs are turned **off** by default.</span></span> 

<span data-ttu-id="d37d1-125">Kövesse az alábbi tooenable naplózást a CDN egyszerűsített analitika hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="d37d1-125">Follow hello steps below tooenable logging with CDN Core Analytics:</span></span>

<span data-ttu-id="d37d1-126">Jelentkezzen be toohello [Azure-portálon](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d37d1-126">Sign in toohello [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="d37d1-127">Ha már nincs engedélyezve a munkafolyamat a CDN [Azure CDN engedélyezése](cdn-create-new-endpoint.md) a folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="d37d1-127">If you don't already have CDN enabled for your workflow, [Enable Azure CDN](cdn-create-new-endpoint.md) before you continue.</span></span>

1. <span data-ttu-id="d37d1-128">Hello portálon lépjen túl**CDN-profil**.</span><span class="sxs-lookup"><span data-stu-id="d37d1-128">In hello portal, navigate too**CDN profile**.</span></span>
2. <span data-ttu-id="d37d1-129">Válassza ki a CDN-profilt, majd válassza ki a megjeleníteni kívánt tooenable hello CDN-végpont **diagnosztikai naplók**.</span><span class="sxs-lookup"><span data-stu-id="d37d1-129">Select a CDN profile, then select hello CDN endpoint that you want tooenable **Diagnostics Logs**.</span></span>

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/02_Browse-to-Diagnostics-logs.png)

3. <span data-ttu-id="d37d1-131">Nyissa meg túl**diagnosztikai naplók** részen **figyelés** területen, majd hello állapotmódosítás túl**a**.</span><span class="sxs-lookup"><span data-stu-id="d37d1-131">Go too**Diagnostics Logs** blade Under **Monitoring** section, then change hello status too**On**.</span></span>

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/03_Diagnostics-logs-options.png)

### <a name="enable-logging-with-azure-storage"></a><span data-ttu-id="d37d1-133">Az Azure Storage naplózásának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="d37d1-133">Enable logging with Azure Storage</span></span>
    
<span data-ttu-id="d37d1-134">toouse Azure Storage toostore hello naplókat, válassza ki **tooa tárfiók archiválására**, válassza ki, megőrzés (nap), és kattintson **CoreAnalytics** alatt **napló**.</span><span class="sxs-lookup"><span data-stu-id="d37d1-134">toouse Azure Storage toostore hello logs, select **Archive tooa storage account**, select retention days, and click **CoreAnalytics** under **Log**.</span></span>

![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/04_Diagnostics-logs-storage.png)

<span data-ttu-id="d37d1-136">*2. ábra – az Azure Storage-naplózás*</span><span class="sxs-lookup"><span data-stu-id="d37d1-136">*Figure 2 - Logging with Azure Storage*</span></span>

### <a name="logging-with-oms-log-analytics"></a><span data-ttu-id="d37d1-137">Az OMS szolgáltatáshoz naplózása</span><span class="sxs-lookup"><span data-stu-id="d37d1-137">Logging with OMS Log Analytics</span></span>

<span data-ttu-id="d37d1-138">toouse OMS Naplóelemzési toostore hello naplókat, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="d37d1-138">toouse OMS Log Analytics toostore hello logs, follow these steps:</span></span>

1. <span data-ttu-id="d37d1-139">A hello **diagnosztikai naplók** részen **figyelés**, jelölje be **tooLog Analytics küldése** a</span><span class="sxs-lookup"><span data-stu-id="d37d1-139">From hello **Diagnostics Logs** blade Under **Monitoring**, select **Send tooLog Analytics** from</span></span> 

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/05_Ready-to-Configure.png)    

2. <span data-ttu-id="d37d1-141">Hello Naplóelemzési naplózás konfigurálásához kattintson a konfigurálás.</span><span class="sxs-lookup"><span data-stu-id="d37d1-141">Configure hello Log Analytics logging by clicking on Configure.</span></span> <span data-ttu-id="d37d1-142">Ezzel megnyitná tooa párbeszédpanel, amelyen egy előző munkaterületen válassza ki vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="d37d1-142">This takes you tooa dialog where you can select a previous workspace or create a new one.</span></span>

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/06_Choose-workspace.png)

3. <span data-ttu-id="d37d1-144">Kattintson a **hozzon létre új munkaterületet**.</span><span class="sxs-lookup"><span data-stu-id="d37d1-144">Click **Create New Workspace**.</span></span>

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/07_Create-new.png)

4. <span data-ttu-id="d37d1-146">Ezután ki kell választania egy új nevet a munkaterület, meglévő előfizetés, erőforráscsoport (új vagy meglévő), hely és a tarifacsomag.</span><span class="sxs-lookup"><span data-stu-id="d37d1-146">Next you must select a new workspace name, existing subscription, resource group (new or existing), location, and pricing tier.</span></span> <span data-ttu-id="d37d1-147">Rendelkezik a konfigurációs tooyour irányítópulton rögzítési hello lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d37d1-147">You have hello option of pinning this configuration tooyour dashboard.</span></span> <span data-ttu-id="d37d1-148">Kattintson az OK toocomplete hello konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="d37d1-148">Click OK toocomplete hello configuration.</span></span>

    <span data-ttu-id="d37d1-149">Ezután meg kell jelennie a munkaterület az OMS-munkaterület és a erőforrás csoport nevével.</span><span class="sxs-lookup"><span data-stu-id="d37d1-149">Next you should see your workspace with your OMS Workspace and Resource group names.</span></span> <span data-ttu-id="d37d1-150">Nevének egyedinek kell lennie, és csak betűket, számokat és kötőjeleket tartalmazhat használhat.</span><span class="sxs-lookup"><span data-stu-id="d37d1-150">Names must be unique and can only use letters, numbers, and hyphens.</span></span> <span data-ttu-id="d37d1-151">Szóközöket és aláhúzásjeleket tartalmazhat nem engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="d37d1-151">Spaces and underscores are not allowed.</span></span> 

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/08_Workspace-resource.png)

5. <span data-ttu-id="d37d1-153">Ezután arról, hogy a munkaterület elkészült, és a naplózás konfigurálására szolgáló képernyőn tooyour ismét rövid üzenetet kapni.</span><span class="sxs-lookup"><span data-stu-id="d37d1-153">You next get a short message saying that your workspace has been created and you are returned tooyour logging configuration screen.</span></span> <span data-ttu-id="d37d1-154">Ellenőrizheti a Naplóelemzési munkaterület hello nevét.</span><span class="sxs-lookup"><span data-stu-id="d37d1-154">You can confirm hello name of your Log Analytics workspace.</span></span>

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/09_Return-to-logging.png)

    <span data-ttu-id="d37d1-156">Hello Naplóelemzési konfigurációs beállítása után ellenőrizze, hogy akkor is jelölőnégyzetet hello CoreAnalytics CDN naplózásához.</span><span class="sxs-lookup"><span data-stu-id="d37d1-156">Once you have set up hello Log Analytics configuration, make sure you also check hello CoreAnalytics box for CDN logging.</span></span>

6. <span data-ttu-id="d37d1-157">Ha minden tooyour tetszőlegesen, kattintson a hello **mentése** hello konfigurációs párbeszédpaneléről hello tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="d37d1-157">If everything is tooyour liking, click hello **Save** button at hello top of hello configuration dialog.</span></span>

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/10_Save-me.png)

    <span data-ttu-id="d37d1-159">Hello **mentése** gomb már nem aktív, és adott hello a/GOMBRÓL most ON, de a kék lila helyett.</span><span class="sxs-lookup"><span data-stu-id="d37d1-159">hello **Save** button is no longer active and that hello ON/OFF button is now ON, but blue instead of purple.</span></span>

7. <span data-ttu-id="d37d1-160">Toosee az új OMS-munkaterület, lépjen tooyour Azure portál irányítópultján kattintson a Naplóelemzési munkaterület hello nevét.</span><span class="sxs-lookup"><span data-stu-id="d37d1-160">If you want toosee your new OMS workspace, go tooyour Azure portal Dashboard, click hello name of your Log Analytics workspace.</span></span> <span data-ttu-id="d37d1-161">Ezután megjelenik a munkaterület (Győződjön meg arról, hogy az OMS-munkaterület hello bal oldali van-e jelölve).</span><span class="sxs-lookup"><span data-stu-id="d37d1-161">Next you will see your workspace (make sure that OMS Workspace is highlighted on hello left).</span></span> <span data-ttu-id="d37d1-162">Kattintson a hello OMS-portálon csempe toosee a munkaterület hello OMS-tárházban.</span><span class="sxs-lookup"><span data-stu-id="d37d1-162">Click on hello OMS Portal tile toosee your workspace in hello OMS repository.</span></span> 

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/11_OMS-dashboard.png) 

    <span data-ttu-id="d37d1-164">Az OMS-tárház mostantól készen áll a toolog adatokat.</span><span class="sxs-lookup"><span data-stu-id="d37d1-164">Your OMS repository is now ready toolog data.</span></span> <span data-ttu-id="d37d1-165">A rendezés tooconsume adatok-verziót kell használnia egy [OMS megoldás](#consuming-oms-log-analytics-data), az érintett a cikk későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="d37d1-165">In order tooconsume that data, you must use an [OMS Solution](#consuming-oms-log-analytics-data), covered later in this article.</span></span>

<span data-ttu-id="d37d1-166">További információt a naplózási adatok késések [Itt](#log-data-delays).</span><span class="sxs-lookup"><span data-stu-id="d37d1-166">For more information about log data delays, go [here](#log-data-delays).</span></span>

## <a name="enable-logging-with-powershell"></a><span data-ttu-id="d37d1-167">A PowerShell-lel naplózásának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="d37d1-167">Enable logging with PowerShell</span></span>

<span data-ttu-id="d37d1-168">Az alábbiakban látható egy példa a hogyan keresztül tooenable és get diagnosztikai naplók hello Azure PowerShell-parancsmagok.</span><span class="sxs-lookup"><span data-stu-id="d37d1-168">Below is an example on how tooenable and get Diagnostic Logs via hello Azure PowerShell Cmdlets.</span></span>

###<a name="enabling-diagnostic-logs-in-a-storage-account"></a><span data-ttu-id="d37d1-169">Diagnosztika engedélyezése bejelentkezik a Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="d37d1-169">Enabling Diagnostic Logs in a Storage Account</span></span>

<span data-ttu-id="d37d1-170">Először jelentkezzen be, és válasszon egy előfizetést:</span><span class="sxs-lookup"><span data-stu-id="d37d1-170">First log in and select a subscription:</span></span>

    Login-AzureRmAccount 

    Select-AzureSubscription -SubscriptionId 


<span data-ttu-id="d37d1-171">tooEnable diagnosztikai naplófájlok egy Tárfiókot, az alábbi parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="d37d1-171">tooEnable Diagnostic Logs in a Storage Account, use this command:</span></span>

```powershell
    Set-AzureRmDiagnosticSetting -ResourceId "/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Cdn/profiles/{profileName}/endpoints/{endpointName}" -StorageAccountId "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicStorage/storageAccounts/{storageAccountName}" -Enabled $true -Categories CoreAnalytics
```
<span data-ttu-id="d37d1-172">tooEnable diagnosztikai naplófájlok az OMS-munkaterület az alábbi parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="d37d1-172">tooEnable Diagnostics Logs in an OMS workspace, use this command:</span></span>

```powershell
    Set-AzureRmDiagnosticSetting -ResourceId "/subscriptions/`{subscriptionId}<subscriptionId>
    .<subscriptionName>" -WorkspaceId "/subscriptions/<workspaceId>.<workspaceName>" -Enabled $true -Categories CoreAnalytics 
```



## <a name="consuming-diagnostics-logs-from-azure-storage"></a><span data-ttu-id="d37d1-173">Diagnosztikai naplók az Azure Storage felhasználása</span><span class="sxs-lookup"><span data-stu-id="d37d1-173">Consuming diagnostics logs from Azure Storage</span></span>
<span data-ttu-id="d37d1-174">Ez a szakasz hello CDN egyszerűsített analitika hello sémája ismerteti, hogyan ezek egy Azure Storage-fiók belül vannak rendezve, és itt minta kód toodownload hello tooa CSV-fájlban naplózza.</span><span class="sxs-lookup"><span data-stu-id="d37d1-174">This section describes hello schema of hello CDN core analytics, how these are organized inside of an Azure Storage Account and provides sample code toodownload hello logs in tooa CSV file.</span></span>

### <a name="using-microsoft-azure-storage-explorer"></a><span data-ttu-id="d37d1-175">A Microsoft Azure Tártallózó használatával</span><span class="sxs-lookup"><span data-stu-id="d37d1-175">Using Microsoft Azure Storage Explorer</span></span>
<span data-ttu-id="d37d1-176">Az Azure Storage-fiók hello hello alapvető analitikai adatok próbál hozzáférni, először egy eszköz tooaccess hello tartalmát egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="d37d1-176">Before you can access hello core analytics data from hello Azure Storage Account, you first need a tool tooaccess hello contents in a storage account.</span></span> <span data-ttu-id="d37d1-177">Eszközök is elérhetők több hello piacon, egy, az ajánlott hello napjainkban hello Microsoft Azure Tártallózó.</span><span class="sxs-lookup"><span data-stu-id="d37d1-177">While there are several tools available in hello market, hello one that we recommend is hello Microsoft Azure Storage Explorer.</span></span> <span data-ttu-id="d37d1-178">Hello eszközt letöltheti [Itt](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="d37d1-178">You can download hello tool from [here](http://storageexplorer.com/).</span></span> <span data-ttu-id="d37d1-179">Ha hello szoftver letöltése és telepítése adja meg azt a toouse hello azonos Azure Storage-fiókot, amely a célként megadott toohello CDN diagnosztikai naplók lett konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="d37d1-179">After downloading and installing hello software, configure it toouse hello same Azure Storage Account that was configured as a destination toohello CDN Diagnostics Logs.</span></span>

1.  <span data-ttu-id="d37d1-180">Nyissa meg **Microsoft Azure Tártallózó**</span><span class="sxs-lookup"><span data-stu-id="d37d1-180">Open **Microsoft Azure Storage Explorer**</span></span>
2.  <span data-ttu-id="d37d1-181">Keresse meg a tárfiók hello</span><span class="sxs-lookup"><span data-stu-id="d37d1-181">Locate hello storage account</span></span>
3.  <span data-ttu-id="d37d1-182">Nyissa meg toohello **"Blobtárolók"** csomópont alatt ez a tároló fiókot, és bontsa ki a hello csomópont</span><span class="sxs-lookup"><span data-stu-id="d37d1-182">Go toohello **“Blob Containers”** node under this storage account and expand hello node</span></span>
4.  <span data-ttu-id="d37d1-183">Jelölje be hello nevű tárolót **"insights-logs-coreanalytics"** , és kattintson rá duplán</span><span class="sxs-lookup"><span data-stu-id="d37d1-183">Select hello container named **“insights-logs-coreanalytics”** and double-click it</span></span>
5.  <span data-ttu-id="d37d1-184">Annak az eredménye megjelenítése fel a hello jobb oldali panelen kezdve hello első szintjét, mely tűnik **"resourceId ="**.</span><span class="sxs-lookup"><span data-stu-id="d37d1-184">Results show up on hello right-hand pane starting with hello first level, which looks like **“resourceId=”**.</span></span> <span data-ttu-id="d37d1-185">Továbbra is az összes hello módon kattint, amíg megjelenik a hello fájl **PT1H.json**.</span><span class="sxs-lookup"><span data-stu-id="d37d1-185">Continue clicking all hello way until you see hello file **PT1H.json**.</span></span> <span data-ttu-id="d37d1-186">Hello Megjegyzés hello elérési ismertetése a következő témakörben talál.</span><span class="sxs-lookup"><span data-stu-id="d37d1-186">See hello following note for explanation of hello path.</span></span>
6.  <span data-ttu-id="d37d1-187">Minden egyes blob **PT1H.json** jelöli analytics naplók hello egy adott CDN-végpont vagy tartozó egyéni tartomány egy óra.</span><span class="sxs-lookup"><span data-stu-id="d37d1-187">Each blob **PT1H.json** represents hello analytics logs for one hour for a specific CDN endpoint or its custom domain.</span></span>
7.  <span data-ttu-id="d37d1-188">a JSON-fájl tartalmának hello hello sémája szakaszban leírt hello séma hello Core Analytics naplókat</span><span class="sxs-lookup"><span data-stu-id="d37d1-188">hello schema of hello contents of this JSON file is described in hello section Schema of hello Core Analytics Logs</span></span>


> [!NOTE]
> <span data-ttu-id="d37d1-189">**A BLOB elérési út formátuma**</span><span class="sxs-lookup"><span data-stu-id="d37d1-189">**Blob path format**</span></span>
> 
> <span data-ttu-id="d37d1-190">Core Analytics naplók óránként akkor jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="d37d1-190">Core Analytics logs are generated every hour.</span></span> <span data-ttu-id="d37d1-191">Minden adat egy óráig gyűjtött és tárolt JSON-adatként egyetlen Azure Blob.</span><span class="sxs-lookup"><span data-stu-id="d37d1-191">All data for an hour are collected and stored inside a single Azure Blob as a JSON payload.</span></span> <span data-ttu-id="d37d1-192">hello elérési toothis Azure Blob jelenik meg, hogy van-e olyan hierarchikus struktúra.</span><span class="sxs-lookup"><span data-stu-id="d37d1-192">hello path toothis Azure Blob appears as if there is a hierarchical structure.</span></span> <span data-ttu-id="d37d1-193">Ez mert hello tárolók explorer eszköz értelmezi "/" értelmezi, és látható hello hierarchia kényelmét szolgálja.</span><span class="sxs-lookup"><span data-stu-id="d37d1-193">This is because hello Storage explorer tool interprets '/' as a directory separator and shows hello hierarchy for convenience.</span></span> <span data-ttu-id="d37d1-194">Ténylegesen hello teljes elérési útja csak hello blob nevét jelöli.</span><span class="sxs-lookup"><span data-stu-id="d37d1-194">Actually, hello whole path just represents hello blob name.</span></span> <span data-ttu-id="d37d1-195">Ez a név hello BLOB követi a következő elnevezési konvenció hello</span><span class="sxs-lookup"><span data-stu-id="d37d1-195">This name of hello blob follows hello following naming convention</span></span> 
    
    resourceId=/SUBSCRIPTIONS/{Subscription Id}/RESOURCEGROUPS/{Resource Group Name}/PROVIDERS/MICROSOFT.CDN/PROFILES/{Profile Name}/ENDPOINTS/{Endpoint Name}/ y={Year}/m={Month}/d={Day}/h={Hour}/m={Minutes}/PT1H.json

<span data-ttu-id="d37d1-196">**Mezők leírása:**</span><span class="sxs-lookup"><span data-stu-id="d37d1-196">**Description of fields:**</span></span>

|<span data-ttu-id="d37d1-197">érték</span><span class="sxs-lookup"><span data-stu-id="d37d1-197">value</span></span>|<span data-ttu-id="d37d1-198">leírás</span><span class="sxs-lookup"><span data-stu-id="d37d1-198">description</span></span>|
|-------|---------|
|<span data-ttu-id="d37d1-199">Előfizetés azonosítója</span><span class="sxs-lookup"><span data-stu-id="d37d1-199">Subscription ID</span></span>    |<span data-ttu-id="d37d1-200">Hello Azure előfizetés-azonosítója.</span><span class="sxs-lookup"><span data-stu-id="d37d1-200">ID of hello Azure Subscription.</span></span> <span data-ttu-id="d37d1-201">Ez a hello Guid formátumban van.</span><span class="sxs-lookup"><span data-stu-id="d37d1-201">This is in hello Guid format.</span></span>|
|<span data-ttu-id="d37d1-202">Erőforrás</span><span class="sxs-lookup"><span data-stu-id="d37d1-202">Resource</span></span> |<span data-ttu-id="d37d1-203">Csoport neve neve hello erőforrás csoport toowhich hello CDN erőforrások tartoznak.</span><span class="sxs-lookup"><span data-stu-id="d37d1-203">Group Name   Name of hello resource group toowhich hello CDN resources belong.</span></span>|
|<span data-ttu-id="d37d1-204">Profilnév</span><span class="sxs-lookup"><span data-stu-id="d37d1-204">Profile Name</span></span> |<span data-ttu-id="d37d1-205">Hello CDN-profil neve</span><span class="sxs-lookup"><span data-stu-id="d37d1-205">Name of hello CDN Profile</span></span>|
|<span data-ttu-id="d37d1-206">A végpont neve</span><span class="sxs-lookup"><span data-stu-id="d37d1-206">Endpoint Name</span></span> |<span data-ttu-id="d37d1-207">CDN-végpont hello neve</span><span class="sxs-lookup"><span data-stu-id="d37d1-207">Name of hello CDN Endpoint</span></span>|
|<span data-ttu-id="d37d1-208">Év</span><span class="sxs-lookup"><span data-stu-id="d37d1-208">Year</span></span>|  <span data-ttu-id="d37d1-209">hello év például 2017 4 számjegyből álló ábrázolása</span><span class="sxs-lookup"><span data-stu-id="d37d1-209">4-digit representation of hello year for example, 2017</span></span>|
|<span data-ttu-id="d37d1-210">Hónap</span><span class="sxs-lookup"><span data-stu-id="d37d1-210">Month</span></span>| <span data-ttu-id="d37d1-211">hello hónap száma 2 számjegyű ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="d37d1-211">2-digit representation of hello month number.</span></span> <span data-ttu-id="d37d1-212">01 január =... 12 decembert jelenti – =</span><span class="sxs-lookup"><span data-stu-id="d37d1-212">01=January ... 12=December</span></span>|
|<span data-ttu-id="d37d1-213">Nap</span><span class="sxs-lookup"><span data-stu-id="d37d1-213">Day</span></span>|   <span data-ttu-id="d37d1-214">hello hónap napja hello 2 számjegy ábrázolása</span><span class="sxs-lookup"><span data-stu-id="d37d1-214">2 digit representation of hello day of hello month</span></span>|
|<span data-ttu-id="d37d1-215">PT1H.JSON</span><span class="sxs-lookup"><span data-stu-id="d37d1-215">PT1H.json</span></span>| <span data-ttu-id="d37d1-216">Hello analitikai adatok tárolására tényleges JSON-fájl</span><span class="sxs-lookup"><span data-stu-id="d37d1-216">Actual JSON file where hello analytics data is stored</span></span>|

### <a name="exporting-hello-core-analytics-data-tooa-csv-file"></a><span data-ttu-id="d37d1-217">Hello alapvető analitikai adatok tooa CSV-fájl exportálása</span><span class="sxs-lookup"><span data-stu-id="d37d1-217">Exporting hello Core Analytics Data tooa CSV File</span></span>

<span data-ttu-id="d37d1-218">toomake könnyen tooaccess hello egyszerűsített analitika nyújtunk informatikai egy mintakód egy eszközt, amely lehetővé teszi, hogy egy egyszerű vesszővel tagolt fájl formátumra, amely lehet használt tooeasily hello JSON-fájlok letöltése a diagramok vagy más összesítéseket létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d37d1-218">toomake it easy tooaccess hello Core Analytics, we provide a sample code for a tool, which allows downloading hello JSON files into a flat comma-separated file format, which can be used tooeasily create charts or other aggregations.</span></span>

<span data-ttu-id="d37d1-219">Íme hello eszköz használatát:</span><span class="sxs-lookup"><span data-stu-id="d37d1-219">Here is how you can use hello tool:</span></span>

1.  <span data-ttu-id="d37d1-220">Látogasson el a hello github hivatkozás: [https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv](https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv )</span><span class="sxs-lookup"><span data-stu-id="d37d1-220">Visit hello github link: [https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv ](https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv )</span></span>
2.  <span data-ttu-id="d37d1-221">Hello kód letöltése</span><span class="sxs-lookup"><span data-stu-id="d37d1-221">Download hello code</span></span>
3.  <span data-ttu-id="d37d1-222">Kövesse az utasításokat toocompile és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d37d1-222">Follow instructions toocompile and configure</span></span>
4.  <span data-ttu-id="d37d1-223">Hello eszköz futtatása</span><span class="sxs-lookup"><span data-stu-id="d37d1-223">Run hello tool</span></span>
5.  <span data-ttu-id="d37d1-224">Letöltött CSV-fájlt egy egyszerű strukturálatlan hierarchia hello analytics adatainak megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="d37d1-224">Resulting CSV file shows hello analytics data in a simple flat hierarchy.</span></span>

## <a name="consuming-diagnostics-logs-from-an-oms-log-analytics-repository"></a><span data-ttu-id="d37d1-225">Diagnosztikai naplók az OMS szolgáltatáshoz tárházból felhasználása</span><span class="sxs-lookup"><span data-stu-id="d37d1-225">Consuming diagnostics logs from an OMS Log Analytics repository</span></span>
<span data-ttu-id="d37d1-226">A Naplóelemzési egy olyan szolgáltatás, az Operations Management Suite (OMS), amely figyeli a felhőalapú és helyszíni környezetben toomaintain azok rendelkezésre állását és teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="d37d1-226">Log Analytics is a service in Operations Management Suite (OMS) that monitors your cloud and on-premises environments toomaintain their availability and performance.</span></span> <span data-ttu-id="d37d1-227">Összegyűjti az több forrás erőforrások a felhőalapú és helyszíni környezetben és az egyéb felügyeleti eszközök tooprovide elemzés által létrehozott adatok.</span><span class="sxs-lookup"><span data-stu-id="d37d1-227">It collects data generated by resources in your cloud and on-premises environments and from other monitoring tools tooprovide analysis across multiple sources.</span></span> 

<span data-ttu-id="d37d1-228">Naplóelemzési toouse, meg kell [naplózását](#enable-logging-with-azure-storage) cikkben korábban tárgyalt toohello Azure OMS Naplóelemzés tárház.</span><span class="sxs-lookup"><span data-stu-id="d37d1-228">toouse Log Analytics, you must [enable logging](#enable-logging-with-azure-storage) toohello Azure OMS Log Analytics repository, which is discussed earlier in this article.</span></span>

### <a name="using-hello-oms-repository"></a><span data-ttu-id="d37d1-229">Hello OMS-tárház használatával</span><span class="sxs-lookup"><span data-stu-id="d37d1-229">Using hello OMS Repository</span></span>

 <span data-ttu-id="d37d1-230">a következő ábra azt mutatja be hello architektúra hello ráfordítások és hello tárház kimenetek hello:</span><span class="sxs-lookup"><span data-stu-id="d37d1-230">hello following diagram shows hello architecture of hello inputs and outputs of hello repository:</span></span>

![OMS napló Analytics tárház](./media/cdn-diagnostics-log/12_Repo-overview.png)

<span data-ttu-id="d37d1-232">*3. ábra - napló Analytics tárház*</span><span class="sxs-lookup"><span data-stu-id="d37d1-232">*Figure 3 - Log Analytics Repository*</span></span>

<span data-ttu-id="d37d1-233">Hello adatok megoldások használatával megjelenítheti az sokféleképpen.</span><span class="sxs-lookup"><span data-stu-id="d37d1-233">You can display hello data in a variety of ways by using Management Solutions.</span></span> <span data-ttu-id="d37d1-234">Megoldásokat szerezhet be a hello [Azure piactér](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/monitoring-management?page=1&subcategories=management-solutions).</span><span class="sxs-lookup"><span data-stu-id="d37d1-234">You can obtain Management Solutions from hello [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/monitoring-management?page=1&subcategories=management-solutions).</span></span>

<span data-ttu-id="d37d1-235">Telepíthető megoldások Azure piactérről hello kattintva **most töltse le innen** hello alján lévő egyes megoldások hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="d37d1-235">You can install management solutions from Azure marketplace by clicking hello **Get it now** link at hello bottom of each solution.</span></span>

### <a name="adding-an-oms-cdn-management-solution"></a><span data-ttu-id="d37d1-236">Egy OMS CDN-felügyeleti megoldás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d37d1-236">Adding an OMS CDN Management Solution</span></span>

<span data-ttu-id="d37d1-237">Kövesse ezeket a lépéseket tooadd olyan felügyeleti megoldást:</span><span class="sxs-lookup"><span data-stu-id="d37d1-237">Follow these steps tooadd a Management Solution:</span></span>

1.   <span data-ttu-id="d37d1-238">Ha még nem tette meg, toohello bejelentkezés az Azure-előfizetéshez az Azure portálon, és nyissa meg tooyour irányítópult.</span><span class="sxs-lookup"><span data-stu-id="d37d1-238">If you haven't already done so, sign in toohello Azure portal using your Azure subscription and go tooyour Dashboard.</span></span>
    <span data-ttu-id="d37d1-239">![Az Azure irányítópult](./media/cdn-diagnostics-log/13_Azure-dashboard.png)</span><span class="sxs-lookup"><span data-stu-id="d37d1-239">![Azure Dashboard](./media/cdn-diagnostics-log/13_Azure-dashboard.png)</span></span>

2. <span data-ttu-id="d37d1-240">A hello **új** részen **piactér**, jelölje be **figyelés + felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="d37d1-240">In hello **New** blade under **Marketplace**, select **Monitoring + management**.</span></span>

    ![Piactér](./media/cdn-diagnostics-log/14_Marketplace.png)

3. <span data-ttu-id="d37d1-242">A hello **figyelés + felügyeleti** panelen kattintson a **láthatja az összes**.</span><span class="sxs-lookup"><span data-stu-id="d37d1-242">In hello **Monitoring + management** blade, click **See all**.</span></span>

    ![Az összes megtekintése](./media/cdn-diagnostics-log/15_See-all.png)

4.  <span data-ttu-id="d37d1-244">Keresse meg a CDN hello Keresés mezőbe.</span><span class="sxs-lookup"><span data-stu-id="d37d1-244">Search for CDN in hello search box.</span></span>

    ![Az összes megtekintése](./media/cdn-diagnostics-log/16_Search-for.png)

5.  <span data-ttu-id="d37d1-246">Válassza ki **Azure CDN egyszerűsített analitika**.</span><span class="sxs-lookup"><span data-stu-id="d37d1-246">Select **Azure CDN Core Analytics**.</span></span> 

    ![Az összes megtekintése](./media/cdn-diagnostics-log/17_Core-analytics.png)

6.  <span data-ttu-id="d37d1-248">Miután rákattintott **létrehozása**, fogja ismételt toocreate egy új OMS-munkaterület, vagy használjon egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="d37d1-248">After clicking **Create**, you will be asked toocreate a new OMS workspace or use an existing one.</span></span> 

    ![Az összes megtekintése](./media/cdn-diagnostics-log/18_Adding-solution.png)

7.  <span data-ttu-id="d37d1-250">Válassza ki a létrehozott előtt hello munkaterület.</span><span class="sxs-lookup"><span data-stu-id="d37d1-250">Select hello workspace you created before.</span></span> <span data-ttu-id="d37d1-251">Akkor kell tooadd automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="d37d1-251">You then need tooadd an automation account.</span></span>

    ![Az összes megtekintése](./media/cdn-diagnostics-log/19_Add-automation.png)

8. <span data-ttu-id="d37d1-253">hello alábbi képernyőn látható hello automatizálási fiókot formában adja meg.</span><span class="sxs-lookup"><span data-stu-id="d37d1-253">hello following screen shows hello automation account form you must fill out.</span></span> 

    ![Az összes megtekintése](./media/cdn-diagnostics-log/20_Automation.png)

9. <span data-ttu-id="d37d1-255">Hello automation-fiók létrehozását követően készen áll a tooadd áll a megoldás.</span><span class="sxs-lookup"><span data-stu-id="d37d1-255">Once you have created hello automation account, you are ready tooadd your solution.</span></span> <span data-ttu-id="d37d1-256">Kattintson a hello **létrehozása** gombra.</span><span class="sxs-lookup"><span data-stu-id="d37d1-256">Click hello **Create** button.</span></span>

    ![Az összes megtekintése](./media/cdn-diagnostics-log/21_Ready.png)

10. <span data-ttu-id="d37d1-258">A megoldás most lett felvéve tooyour munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="d37d1-258">Your solution has now been added tooyour workspace.</span></span> <span data-ttu-id="d37d1-259">Lépjen vissza az Azure portál irányítópultján tooyour.</span><span class="sxs-lookup"><span data-stu-id="d37d1-259">Go back tooyour Azure portal Dashboard.</span></span>

    ![Az összes megtekintése](./media/cdn-diagnostics-log/22_Dashboard.png)

    <span data-ttu-id="d37d1-261">Kattintson a létrehozott toogo tooyour munkaterület hello Naplóelemzési munkaterület.</span><span class="sxs-lookup"><span data-stu-id="d37d1-261">Click hello Log Analytics workspace you created toogo tooyour workspace.</span></span> 

11. <span data-ttu-id="d37d1-262">Kattintson a hello **OMS-portálon** toosee az új megoldás az OMS-portálon hello csempére.</span><span class="sxs-lookup"><span data-stu-id="d37d1-262">Click hello **OMS Portal** tile toosee your new solution in hello OMS portal.</span></span>

    ![Az összes megtekintése](./media/cdn-diagnostics-log/23_workspace.png)

12. <span data-ttu-id="d37d1-264">Az OMS-portálon mostantól a következő képernyő hello kell hasonlítania:</span><span class="sxs-lookup"><span data-stu-id="d37d1-264">Your OMS portal should now look like hello following screen:</span></span>

    ![Az összes megtekintése](./media/cdn-diagnostics-log/24_OMS-solution.png)

    <span data-ttu-id="d37d1-266">Kattintson az egyik hello csempék toosee számos nézet azokat az adatokat.</span><span class="sxs-lookup"><span data-stu-id="d37d1-266">Click one of hello tiles toosee several views into your data.</span></span>

    ![Az összes megtekintése](./media/cdn-diagnostics-log/25_Interior-view.png)

    <span data-ttu-id="d37d1-268">Balra görgetve, vagy további jobb toosee tartalmazó csempék éppen úgy egyes nézetek képviselő hello adatokká.</span><span class="sxs-lookup"><span data-stu-id="d37d1-268">You can scroll left or right toosee further tiles representing individual views into hello data.</span></span> 

    <span data-ttu-id="d37d1-269">Hello csempék valamelyikére kattintva lehetővé teszi az adatok további információt.</span><span class="sxs-lookup"><span data-stu-id="d37d1-269">Clicking one of hello tiles gives you more details about your data.</span></span>

     ![Az összes megtekintése](./media/cdn-diagnostics-log/26_Further-detail.png)

### <a name="offers-and-pricing-tiers"></a><span data-ttu-id="d37d1-271">Ajánlatok és tarifacsomagok</span><span class="sxs-lookup"><span data-stu-id="d37d1-271">Offers and pricing tiers</span></span>

<span data-ttu-id="d37d1-272">Ajánlatok és az OMS-kezelési megoldások tarifacsomagok [Itt](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-add-solutions#offers-and-pricing-tiers).</span><span class="sxs-lookup"><span data-stu-id="d37d1-272">You can see offers and pricing tiers for OMS management solutions [here](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-add-solutions#offers-and-pricing-tiers).</span></span>

### <a name="customizing-views"></a><span data-ttu-id="d37d1-273">Nézetek testreszabása</span><span class="sxs-lookup"><span data-stu-id="d37d1-273">Customizing views</span></span>

<span data-ttu-id="d37d1-274">Testre szabhatja hello nézet a adatokká hello segítségével **adatforrásnézet-tervezőből**.</span><span class="sxs-lookup"><span data-stu-id="d37d1-274">You can customize hello view into your data by using hello **View Designer**.</span></span> <span data-ttu-id="d37d1-275">Nyissa meg tooyour OMS-munkaterület és újra kell kezdenie designing hello kattintva **adatforrásnézet-tervezőből** csempére.</span><span class="sxs-lookup"><span data-stu-id="d37d1-275">Go tooyour OMS workspace and begin designing by clicking hello **View Designer** tile.</span></span>

![Nézettervező](./media/cdn-diagnostics-log/27_Designer.png)

<span data-ttu-id="d37d1-277">Húzza és dobja el a diagramok típusú hello balról, és töltse ki a bal oldali hello tooanalyze kívánt hello az adatait.</span><span class="sxs-lookup"><span data-stu-id="d37d1-277">You can drag and drop types of charts from hello left and fill in hello data details you want tooanalyze on hello left.</span></span>

![Nézettervező](./media/cdn-diagnostics-log/28_Designer.png)

    
## <a name="log-data-delays"></a><span data-ttu-id="d37d1-279">Naplózási adatok késleltetése</span><span class="sxs-lookup"><span data-stu-id="d37d1-279">Log data delays</span></span>

<span data-ttu-id="d37d1-280">Verizon napló adatok késleltetése</span><span class="sxs-lookup"><span data-stu-id="d37d1-280">Verizon log data delays</span></span> | <span data-ttu-id="d37d1-281">Akamai napló adatok késleltetése</span><span class="sxs-lookup"><span data-stu-id="d37d1-281">Akamai log data delays</span></span>
--- | ---
<span data-ttu-id="d37d1-282">Verizon naplóadatokat 1 óra késleltetett, és eltarthat, mire too2 óra toostart végpont propagálás befejezését követően megjelenne.</span><span class="sxs-lookup"><span data-stu-id="d37d1-282">Verizon log data is 1 hour delayed, and take up too2 hours toostart appearing after endpoint propagation completion.</span></span> | <span data-ttu-id="d37d1-283">Akamai naplóadatokat késleltetett 24 óra, és foglaljon too2 óra toostart jelenik meg, ha több mint 24 órája hozták létre.</span><span class="sxs-lookup"><span data-stu-id="d37d1-283">Akamai log data is 24 hours delayed, and takes up too2 hours toostart appearing if it was created more than 24 hours ago.</span></span> <span data-ttu-id="d37d1-284">Ha nemrég készült, hello naplók toostart szereplő too25 órát is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="d37d1-284">If it was recently created, it can take up too25 hours for hello logs toostart appearing.</span></span>

## <a name="diagnostic-log-types-for-cdn-core-analytics"></a><span data-ttu-id="d37d1-285">Diagnosztikai naplófájl típusokat CDN egyszerűsített analitika</span><span class="sxs-lookup"><span data-stu-id="d37d1-285">Diagnostic log types for CDN Core Analytics</span></span>

<span data-ttu-id="d37d1-286">Jelenleg csak egyszerűsített analitika naplók, metrikák HTTP-válaszok statisztikai adatainak és a kimenő forgalom statisztika, amint az hello CDN POP/szélén látható tartalmazó fel.</span><span class="sxs-lookup"><span data-stu-id="d37d1-286">We currently offer only Core Analytics logs, which contain metrics showing HTTP response statistics and egress statistics as seen from hello CDN POPs/edges.</span></span>

### <a name="core-analytics-metrics-details"></a><span data-ttu-id="d37d1-287">Core Analytics metrikák részletei</span><span class="sxs-lookup"><span data-stu-id="d37d1-287">Core Analytics Metrics Details</span></span>
<span data-ttu-id="d37d1-288">a következő táblázat hello hello egyszerűsített analitika naplózza az elérhető mérőszámok listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="d37d1-288">hello following table shows a list of metrics available in hello Core Analytics logs.</span></span> <span data-ttu-id="d37d1-289">Nem minden metrikák érhetők el minden szolgáltató, annak ellenére, hogy ezek az eltérések minimális.</span><span class="sxs-lookup"><span data-stu-id="d37d1-289">Not all metrics are available from all providers, although such differences are minimal.</span></span> <span data-ttu-id="d37d1-290">a következő táblázat is hello jeleníti meg, ha egy metrika érhető el a szolgáltató által.</span><span class="sxs-lookup"><span data-stu-id="d37d1-290">hello following table also shows if a given metric is available from a provider.</span></span> <span data-ttu-id="d37d1-291">Vegye figyelembe, hogy hello metrikák érhetők el csak ezek CDN-végpontok, amelyek azokat a forgalmat.</span><span class="sxs-lookup"><span data-stu-id="d37d1-291">Please note that hello metrics are available for only those CDN endpoints that have traffic on them.</span></span>


|<span data-ttu-id="d37d1-292">Metrika</span><span class="sxs-lookup"><span data-stu-id="d37d1-292">Metric</span></span>                     | <span data-ttu-id="d37d1-293">Leírás</span><span class="sxs-lookup"><span data-stu-id="d37d1-293">Description</span></span>   | <span data-ttu-id="d37d1-294">Verizon</span><span class="sxs-lookup"><span data-stu-id="d37d1-294">Verizon</span></span>  | <span data-ttu-id="d37d1-295">Akamai</span><span class="sxs-lookup"><span data-stu-id="d37d1-295">Akamai</span></span> 
|---------------------------|---------------|---|---|
| <span data-ttu-id="d37d1-296">RequestCountTotal</span><span class="sxs-lookup"><span data-stu-id="d37d1-296">RequestCountTotal</span></span>         |<span data-ttu-id="d37d1-297">Ebben az időszakban kérést találatok száma összesen</span><span class="sxs-lookup"><span data-stu-id="d37d1-297">Total number of request hits during this period</span></span>| <span data-ttu-id="d37d1-298">Igen</span><span class="sxs-lookup"><span data-stu-id="d37d1-298">Yes</span></span>  |<span data-ttu-id="d37d1-299">Igen</span><span class="sxs-lookup"><span data-stu-id="d37d1-299">Yes</span></span>   |
| <span data-ttu-id="d37d1-300">RequestCountHttpStatus2xx</span><span class="sxs-lookup"><span data-stu-id="d37d1-300">RequestCountHttpStatus2xx</span></span> |<span data-ttu-id="d37d1-301">Egy 2xx HTTP kódot (pl. 200, 202) eredményezett összes kérelem száma</span><span class="sxs-lookup"><span data-stu-id="d37d1-301">Count of all requests that resulted in a 2xx HTTP code (e.g. 200, 202)</span></span>              | <span data-ttu-id="d37d1-302">Igen</span><span class="sxs-lookup"><span data-stu-id="d37d1-302">Yes</span></span>  |<span data-ttu-id="d37d1-303">Igen</span><span class="sxs-lookup"><span data-stu-id="d37d1-303">Yes</span></span>   |
| <span data-ttu-id="d37d1-304">RequestCountHttpStatus3xx</span><span class="sxs-lookup"><span data-stu-id="d37d1-304">RequestCountHttpStatus3xx</span></span> | <span data-ttu-id="d37d1-305">Egy 3xx HTTP kódot (pl. 300, 302) eredményezett összes kérelem száma</span><span class="sxs-lookup"><span data-stu-id="d37d1-305">Count of all requests that resulted in a 3xx HTTP code (e.g. 300, 302)</span></span>              | <span data-ttu-id="d37d1-306">Igen</span><span class="sxs-lookup"><span data-stu-id="d37d1-306">Yes</span></span>  |<span data-ttu-id="d37d1-307">Igen</span><span class="sxs-lookup"><span data-stu-id="d37d1-307">Yes</span></span>   |
| <span data-ttu-id="d37d1-308">RequestCountHttpStatus4xx</span><span class="sxs-lookup"><span data-stu-id="d37d1-308">RequestCountHttpStatus4xx</span></span> |<span data-ttu-id="d37d1-309">Egy 4xx HTTP kódot (pl. 400, 404) eredményezett összes kérelem száma</span><span class="sxs-lookup"><span data-stu-id="d37d1-309">Count of all requests that resulted in a 4xx HTTP code (e.g. 400, 404)</span></span>               | <span data-ttu-id="d37d1-310">Igen</span><span class="sxs-lookup"><span data-stu-id="d37d1-310">Yes</span></span>   |<span data-ttu-id="d37d1-311">Igen</span><span class="sxs-lookup"><span data-stu-id="d37d1-311">Yes</span></span>   |
| <span data-ttu-id="d37d1-312">RequestCountHttpStatus5xx</span><span class="sxs-lookup"><span data-stu-id="d37d1-312">RequestCountHttpStatus5xx</span></span> | <span data-ttu-id="d37d1-313">5xx HTTP kódot (pl. 500, 504) eredményezett összes kérelem száma</span><span class="sxs-lookup"><span data-stu-id="d37d1-313">Count of all requests that resulted in a 5xx HTTP code (e.g. 500, 504)</span></span>              | <span data-ttu-id="d37d1-314">Igen</span><span class="sxs-lookup"><span data-stu-id="d37d1-314">Yes</span></span>  |<span data-ttu-id="d37d1-315">Igen</span><span class="sxs-lookup"><span data-stu-id="d37d1-315">Yes</span></span>   |
| <span data-ttu-id="d37d1-316">RequestCountHttpStatusOthers</span><span class="sxs-lookup"><span data-stu-id="d37d1-316">RequestCountHttpStatusOthers</span></span> |  <span data-ttu-id="d37d1-317">Minden más HTTP-kód (kívül 2xx-5xx) száma</span><span class="sxs-lookup"><span data-stu-id="d37d1-317">Count of all other HTTP codes (outside of 2xx-5xx)</span></span> | <span data-ttu-id="d37d1-318">Igen</span><span class="sxs-lookup"><span data-stu-id="d37d1-318">Yes</span></span>  |<span data-ttu-id="d37d1-319">Igen</span><span class="sxs-lookup"><span data-stu-id="d37d1-319">Yes</span></span>   |
| <span data-ttu-id="d37d1-320">RequestCountHttpStatus200</span><span class="sxs-lookup"><span data-stu-id="d37d1-320">RequestCountHttpStatus200</span></span> | <span data-ttu-id="d37d1-321">200 HTTP kód válaszul eredményező összes kérelmek száma</span><span class="sxs-lookup"><span data-stu-id="d37d1-321">Count of all requests that resulted in a 200 HTTP code response</span></span>              |<span data-ttu-id="d37d1-322">Nem</span><span class="sxs-lookup"><span data-stu-id="d37d1-322">No</span></span>   |<span data-ttu-id="d37d1-323">Igen</span><span class="sxs-lookup"><span data-stu-id="d37d1-323">Yes</span></span>   |
| <span data-ttu-id="d37d1-324">RequestCountHttpStatus206</span><span class="sxs-lookup"><span data-stu-id="d37d1-324">RequestCountHttpStatus206</span></span> | <span data-ttu-id="d37d1-325">A 206-os HTTP kód válaszul eredményező összes kérelmek száma</span><span class="sxs-lookup"><span data-stu-id="d37d1-325">Count of all requests that resulted in a 206 HTTP code response</span></span>              |<span data-ttu-id="d37d1-326">Nem</span><span class="sxs-lookup"><span data-stu-id="d37d1-326">No</span></span>   |<span data-ttu-id="d37d1-327">Igen</span><span class="sxs-lookup"><span data-stu-id="d37d1-327">Yes</span></span>   |
| <span data-ttu-id="d37d1-328">RequestCountHttpStatus302</span><span class="sxs-lookup"><span data-stu-id="d37d1-328">RequestCountHttpStatus302</span></span> | <span data-ttu-id="d37d1-329">HTTP 302-es kód választ eredményező összes kérelmek száma</span><span class="sxs-lookup"><span data-stu-id="d37d1-329">Count of all requests that resulted in a 302 HTTP code response</span></span>              |<span data-ttu-id="d37d1-330">Nem</span><span class="sxs-lookup"><span data-stu-id="d37d1-330">No</span></span>   |<span data-ttu-id="d37d1-331">Igen</span><span class="sxs-lookup"><span data-stu-id="d37d1-331">Yes</span></span>   |
| <span data-ttu-id="d37d1-332">RequestCountHttpStatus304</span><span class="sxs-lookup"><span data-stu-id="d37d1-332">RequestCountHttpStatus304</span></span> |  <span data-ttu-id="d37d1-333">304-es HTTP-kód választ eredményező összes kérelmek száma</span><span class="sxs-lookup"><span data-stu-id="d37d1-333">Count of all requests that resulted in a 304 HTTP code response</span></span>             |<span data-ttu-id="d37d1-334">Nem</span><span class="sxs-lookup"><span data-stu-id="d37d1-334">No</span></span>   |<span data-ttu-id="d37d1-335">Igen</span><span class="sxs-lookup"><span data-stu-id="d37d1-335">Yes</span></span>   |
| <span data-ttu-id="d37d1-336">RequestCountHttpStatus404</span><span class="sxs-lookup"><span data-stu-id="d37d1-336">RequestCountHttpStatus404</span></span> | <span data-ttu-id="d37d1-337">HTTP 404-es kód választ eredményező összes kérelmek száma</span><span class="sxs-lookup"><span data-stu-id="d37d1-337">Count of all requests that resulted in a 404 HTTP code response</span></span>              |<span data-ttu-id="d37d1-338">Nem</span><span class="sxs-lookup"><span data-stu-id="d37d1-338">No</span></span>   |<span data-ttu-id="d37d1-339">Igen</span><span class="sxs-lookup"><span data-stu-id="d37d1-339">Yes</span></span>   |
| <span data-ttu-id="d37d1-340">RequestCountCacheHit</span><span class="sxs-lookup"><span data-stu-id="d37d1-340">RequestCountCacheHit</span></span> |<span data-ttu-id="d37d1-341">A gyorsítótár találati eredményező összes kérelmek száma.</span><span class="sxs-lookup"><span data-stu-id="d37d1-341">Count of all requests that resulted in a Cache Hit.</span></span> <span data-ttu-id="d37d1-342">Ez azt jelenti, hogy hello eszköz kiszolgálásának hello POP toohello ügyfél-ről.</span><span class="sxs-lookup"><span data-stu-id="d37d1-342">This means hello asset was served directly from hello POP toohello Client.</span></span>               | <span data-ttu-id="d37d1-343">Igen</span><span class="sxs-lookup"><span data-stu-id="d37d1-343">Yes</span></span>  |<span data-ttu-id="d37d1-344">Nem</span><span class="sxs-lookup"><span data-stu-id="d37d1-344">No</span></span>   |
| <span data-ttu-id="d37d1-345">RequestCountCacheMiss</span><span class="sxs-lookup"><span data-stu-id="d37d1-345">RequestCountCacheMiss</span></span> | <span data-ttu-id="d37d1-346">A gyorsítótár-tévesztései eredményező összes kérelmek száma.</span><span class="sxs-lookup"><span data-stu-id="d37d1-346">Count of all requests that resulted in a Cache Miss.</span></span> <span data-ttu-id="d37d1-347">Ez azt jelenti, hogy hello eszköz hello POP legközelebbi toohello ügyfél nem található, és ezért be lett olvasva a hello forrása.</span><span class="sxs-lookup"><span data-stu-id="d37d1-347">This means hello asset was not found on hello POP closest toohello client, and therefore was retrieved from hello Origin.</span></span>              |<span data-ttu-id="d37d1-348">Igen</span><span class="sxs-lookup"><span data-stu-id="d37d1-348">Yes</span></span>   | <span data-ttu-id="d37d1-349">Nem</span><span class="sxs-lookup"><span data-stu-id="d37d1-349">No</span></span>  |
| <span data-ttu-id="d37d1-350">RequestCountCacheNoCache</span><span class="sxs-lookup"><span data-stu-id="d37d1-350">RequestCountCacheNoCache</span></span> | <span data-ttu-id="d37d1-351">A kérelmek tooan eszköz, amely megakadályozta a gyorsítótárba hello oldal tooa felhasználói konfiguráció miatt az összes száma.</span><span class="sxs-lookup"><span data-stu-id="d37d1-351">Count of all requests tooan asset that are prevented from being cached due tooa user configuration on hello edge.</span></span>              |<span data-ttu-id="d37d1-352">Igen</span><span class="sxs-lookup"><span data-stu-id="d37d1-352">Yes</span></span>   | <span data-ttu-id="d37d1-353">Nem</span><span class="sxs-lookup"><span data-stu-id="d37d1-353">No</span></span>  |
| <span data-ttu-id="d37d1-354">RequestCountCacheUncacheable</span><span class="sxs-lookup"><span data-stu-id="d37d1-354">RequestCountCacheUncacheable</span></span> | <span data-ttu-id="d37d1-355">Teljes számát, amely megakadályozta a gyorsítótárba által hello eszköz Cache-Control tooassets kér, és a lejárati fejléceket, amely jelzi, hogy azt nem gyorsítótárazza a POP vagy hello HTTP-ügyfél által</span><span class="sxs-lookup"><span data-stu-id="d37d1-355">Count of all requests tooassets that are prevented from being cached by hello asset's Cache-Control and Expires headers, which indicate that it should not be cached on a POP or by hello HTTP client</span></span>                |<span data-ttu-id="d37d1-356">Igen</span><span class="sxs-lookup"><span data-stu-id="d37d1-356">Yes</span></span>   |<span data-ttu-id="d37d1-357">Nem</span><span class="sxs-lookup"><span data-stu-id="d37d1-357">No</span></span>   |
| <span data-ttu-id="d37d1-358">RequestCountCacheOthers</span><span class="sxs-lookup"><span data-stu-id="d37d1-358">RequestCountCacheOthers</span></span> | <span data-ttu-id="d37d1-359">Gyorsítótár állapotú fent nem vonatkozik minden kérelmek száma.</span><span class="sxs-lookup"><span data-stu-id="d37d1-359">Count of all requests with cache status not covered by above.</span></span>              |<span data-ttu-id="d37d1-360">Igen</span><span class="sxs-lookup"><span data-stu-id="d37d1-360">Yes</span></span>   | <span data-ttu-id="d37d1-361">Nem</span><span class="sxs-lookup"><span data-stu-id="d37d1-361">No</span></span>  |
| <span data-ttu-id="d37d1-362">EgressTotal</span><span class="sxs-lookup"><span data-stu-id="d37d1-362">EgressTotal</span></span> | <span data-ttu-id="d37d1-363">Kimenő adatátvitel GB-ban</span><span class="sxs-lookup"><span data-stu-id="d37d1-363">Outbound data transfer in GB</span></span>              |<span data-ttu-id="d37d1-364">Igen</span><span class="sxs-lookup"><span data-stu-id="d37d1-364">Yes</span></span>   |<span data-ttu-id="d37d1-365">Igen</span><span class="sxs-lookup"><span data-stu-id="d37d1-365">Yes</span></span>   |
| <span data-ttu-id="d37d1-366">EgressHttpStatus2xx</span><span class="sxs-lookup"><span data-stu-id="d37d1-366">EgressHttpStatus2xx</span></span> | <span data-ttu-id="d37d1-367">Kimenő adatátviteli * a válaszok a 2xx HTTP-állapotkódok GB-ban</span><span class="sxs-lookup"><span data-stu-id="d37d1-367">Outbound data transfer* for responses with 2xx HTTP status codes in GB</span></span>            |<span data-ttu-id="d37d1-368">Igen</span><span class="sxs-lookup"><span data-stu-id="d37d1-368">Yes</span></span>   |<span data-ttu-id="d37d1-369">Nem</span><span class="sxs-lookup"><span data-stu-id="d37d1-369">No</span></span>   |
| <span data-ttu-id="d37d1-370">EgressHttpStatus3xx</span><span class="sxs-lookup"><span data-stu-id="d37d1-370">EgressHttpStatus3xx</span></span> | <span data-ttu-id="d37d1-371">Kimenő adatátvitel a válaszok a 3xx HTTP-állapotkódok GB-ban</span><span class="sxs-lookup"><span data-stu-id="d37d1-371">Outbound data transfer for responses with 3xx HTTP status codes in GB</span></span>              |<span data-ttu-id="d37d1-372">Igen</span><span class="sxs-lookup"><span data-stu-id="d37d1-372">Yes</span></span>   |<span data-ttu-id="d37d1-373">Nem</span><span class="sxs-lookup"><span data-stu-id="d37d1-373">No</span></span>   |
| <span data-ttu-id="d37d1-374">EgressHttpStatus4xx</span><span class="sxs-lookup"><span data-stu-id="d37d1-374">EgressHttpStatus4xx</span></span> | <span data-ttu-id="d37d1-375">Kimenő adatátvitel a válaszok a 4xx HTTP-állapotkódok GB-ban</span><span class="sxs-lookup"><span data-stu-id="d37d1-375">Outbound data transfer for responses with 4xx HTTP status codes in GB</span></span>               |<span data-ttu-id="d37d1-376">Igen</span><span class="sxs-lookup"><span data-stu-id="d37d1-376">Yes</span></span>   | <span data-ttu-id="d37d1-377">Nem</span><span class="sxs-lookup"><span data-stu-id="d37d1-377">No</span></span>  |
| <span data-ttu-id="d37d1-378">EgressHttpStatus5xx</span><span class="sxs-lookup"><span data-stu-id="d37d1-378">EgressHttpStatus5xx</span></span> | <span data-ttu-id="d37d1-379">Kimenő adatátvitel 5xx HTTP-állapotkódok GB-ban a válaszok</span><span class="sxs-lookup"><span data-stu-id="d37d1-379">Outbound data transfer for responses with 5xx HTTP status codes in GB</span></span>               |<span data-ttu-id="d37d1-380">Igen</span><span class="sxs-lookup"><span data-stu-id="d37d1-380">Yes</span></span>   |  <span data-ttu-id="d37d1-381">Nem</span><span class="sxs-lookup"><span data-stu-id="d37d1-381">No</span></span> |
| <span data-ttu-id="d37d1-382">EgressHttpStatusOthers</span><span class="sxs-lookup"><span data-stu-id="d37d1-382">EgressHttpStatusOthers</span></span> | <span data-ttu-id="d37d1-383">Kimenő adatátvitel válaszok az egyéb HTTP-állapotkódok GB-ban</span><span class="sxs-lookup"><span data-stu-id="d37d1-383">Outbound data transfer for responses with other HTTP status codes in GB</span></span>                |<span data-ttu-id="d37d1-384">Igen</span><span class="sxs-lookup"><span data-stu-id="d37d1-384">Yes</span></span>   |<span data-ttu-id="d37d1-385">Nem</span><span class="sxs-lookup"><span data-stu-id="d37d1-385">No</span></span>   |
| <span data-ttu-id="d37d1-386">EgressCacheHit</span><span class="sxs-lookup"><span data-stu-id="d37d1-386">EgressCacheHit</span></span> |  <span data-ttu-id="d37d1-387">Kimenő adatátviteli kapott válaszok hello CDN gyorsítótár-ről a hello CDN POP/élei számára</span><span class="sxs-lookup"><span data-stu-id="d37d1-387">Outbound data transfer for responses that were delivered directly from hello CDN cache on hello CDN POPs/Edges</span></span>  |<span data-ttu-id="d37d1-388">Igen</span><span class="sxs-lookup"><span data-stu-id="d37d1-388">Yes</span></span>   |  <span data-ttu-id="d37d1-389">Nem</span><span class="sxs-lookup"><span data-stu-id="d37d1-389">No</span></span> |
| <span data-ttu-id="d37d1-390">EgressCacheMiss</span><span class="sxs-lookup"><span data-stu-id="d37d1-390">EgressCacheMiss</span></span> | <span data-ttu-id="d37d1-391">Kimenő adatátvitel a válaszok nem található a legközelebbi kiszolgáló POP hello, és lekérése hello forráskiszolgálóról</span><span class="sxs-lookup"><span data-stu-id="d37d1-391">Outbound data transfer for responses that were not found on hello nearest POP server, and retrieved from hello origin server</span></span>              |<span data-ttu-id="d37d1-392">Igen</span><span class="sxs-lookup"><span data-stu-id="d37d1-392">Yes</span></span>   |  <span data-ttu-id="d37d1-393">Nem</span><span class="sxs-lookup"><span data-stu-id="d37d1-393">No</span></span> |
| <span data-ttu-id="d37d1-394">EgressCacheNoCache</span><span class="sxs-lookup"><span data-stu-id="d37d1-394">EgressCacheNoCache</span></span> | <span data-ttu-id="d37d1-395">Kimenő adatátvitel eszközök, amely megakadályozta a gyorsítótárba hello oldal tooa felhasználói konfiguráció miatt.</span><span class="sxs-lookup"><span data-stu-id="d37d1-395">Outbound data transfer for assets that are prevented from being cached due tooa user configuration on hello edge.</span></span>                |<span data-ttu-id="d37d1-396">Igen</span><span class="sxs-lookup"><span data-stu-id="d37d1-396">Yes</span></span>   |<span data-ttu-id="d37d1-397">Nem</span><span class="sxs-lookup"><span data-stu-id="d37d1-397">No</span></span>   |
| <span data-ttu-id="d37d1-398">EgressCacheUncacheable</span><span class="sxs-lookup"><span data-stu-id="d37d1-398">EgressCacheUncacheable</span></span> | <span data-ttu-id="d37d1-399">Kimenő adatátvitel eszközök, amely megakadályozhatja, hogy a hello eszköz Cache-Control vagy Expires fejléc, amely jelzi, hogy azt nem gyorsítótárazza a POP vagy hello HTTP-ügyfél által a gyorsítótárba</span><span class="sxs-lookup"><span data-stu-id="d37d1-399">Outbound data transfer for assets that are prevented from being cached by hello asset's Cache-Control and/or Expires headers, which indicate that it should not be cached on a POP or by hello HTTP client</span></span>                    |<span data-ttu-id="d37d1-400">Igen</span><span class="sxs-lookup"><span data-stu-id="d37d1-400">Yes</span></span>   | <span data-ttu-id="d37d1-401">Nem</span><span class="sxs-lookup"><span data-stu-id="d37d1-401">No</span></span>  |
| <span data-ttu-id="d37d1-402">EgressCacheOthers</span><span class="sxs-lookup"><span data-stu-id="d37d1-402">EgressCacheOthers</span></span> |  <span data-ttu-id="d37d1-403">Kimenő adatátvitel más gyorsítótár forgatókönyvek esetén.</span><span class="sxs-lookup"><span data-stu-id="d37d1-403">Outbound data transfers for other cache scenarios.</span></span>             |<span data-ttu-id="d37d1-404">Igen</span><span class="sxs-lookup"><span data-stu-id="d37d1-404">Yes</span></span>   | <span data-ttu-id="d37d1-405">Nem</span><span class="sxs-lookup"><span data-stu-id="d37d1-405">No</span></span>  |

<span data-ttu-id="d37d1-406">* A kimenő adatforgalom CDN POP-ra kiszolgálók toohello ügyfélről kézbesíteni tootraffic hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="d37d1-406">*Outbound data transfer refers tootraffic delivered from CDN POP servers toohello client.</span></span>


### <a name="schema-of-hello-core-analytics-logs"></a><span data-ttu-id="d37d1-407">Hello Core Analytics naplók sémája</span><span class="sxs-lookup"><span data-stu-id="d37d1-407">Schema of hello Core Analytics Logs</span></span> 

<span data-ttu-id="d37d1-408">Összes napló JSON formátumban vannak tárolva, és mindegyik bejegyzés rendelkezik sztringek mezőinek hello alatt séma a következő:</span><span class="sxs-lookup"><span data-stu-id="d37d1-408">All logs are stored in JSON format and each entry has string fields following hello below schema:</span></span>

```json
    "records": [
        {
            "time": "2017-04-27T01:00:00",
            "resourceId": "<ARM Resource Id of hello CDN Endpoint>",
            "operationName": "Microsoft.Cdn/profiles/endpoints/contentDelivery",
            "category": "CoreAnalytics",
            "properties": {
                "DomainName": "<Name of hello domain for which hello statistics is reported>",
                "RequestCountTotal": integer value,
                "RequestCountHttpStatus2xx": integer value,
                "RequestCountHttpStatus3xx": integer value,
                "RequestCountHttpStatus4xx": integer value,
                "RequestCountHttpStatus5xx": integer value,
                "RequestCountHttpStatusOthers": integer value,
                "RequestCountHttpStatus200": integer value,
                "RequestCountHttpStatus206": integer value,
                "RequestCountHttpStatus302": integer value,
                "RequestCountHttpStatus304": integer value,
                "RequestCountHttpStatus404": integer value,
                "RequestCountCacheHit": integer value,
                "RequestCountCacheMiss": integer value,
                "RequestCountCacheNoCache": integer value,
                "RequestCountCacheUncacheable": integer value,
                "RequestCountCacheOthers": integer value,
                "EgressTotal": double value,
                "EgressHttpStatus2xx": double value,
                "EgressHttpStatus3xx": double value,
                "EgressHttpStatus4xx": double value,
                "EgressHttpStatus5xx": double value,
                "EgressHttpStatusOthers": double value,
                "EgressCacheHit": double value,
                "EgressCacheMiss": double value,
                "EgressCacheNoCache": double value,
                "EgressCacheUncacheable": double value,
                "EgressCacheOthers": double value,
            }
        }

    ]
}
```

<span data-ttu-id="d37d1-409">Ahol a hello "time" hello kezdési időt, amelynek hello statisztika jelentett hello óra határ jelenti.</span><span class="sxs-lookup"><span data-stu-id="d37d1-409">Where hello ‘time’ represents hello start time of hello hour boundary for which hello statistics is reported.</span></span> <span data-ttu-id="d37d1-410">Ha egy metrika nem támogatott a CDN-szolgáltató helyett egy double vagy egész szám, null értékű lesz.</span><span class="sxs-lookup"><span data-stu-id="d37d1-410">When a metric is not supported by a CDN provider, instead of a double or integer value, there will be a null value.</span></span> <span data-ttu-id="d37d1-411">A null érték metrika hello hiányában, és ez nem azonos a 0 értéket.</span><span class="sxs-lookup"><span data-stu-id="d37d1-411">This null value indicates hello absence of a metric, and this is different from a 0 value.</span></span> <span data-ttu-id="d37d1-412">Ne feledje, hogy az egyetlen halmazát hello végponthoz tartományonként metrikákat lesz.</span><span class="sxs-lookup"><span data-stu-id="d37d1-412">Also note that there will be one set of these metrics per domain configured on hello endpoint.</span></span>

<span data-ttu-id="d37d1-413">Az alábbi példa tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="d37d1-413">Example properties below:</span></span>

```json
{
     "DomainName": "manlingakamaitest2.azureedge.net",
     "RequestCountTotal": 480,
     "RequestCountHttpStatus2xx": 480,
     "RequestCountHttpStatus3xx": 0,
     "RequestCountHttpStatus4xx": 0,
     "RequestCountHttpStatus5xx": 0,
     "RequestCountHttpStatusOthers": 0,
     "RequestCountHttpStatus200": 480,
     "RequestCountHttpStatus206": 0,
     "RequestCountHttpStatus302": 0,
     "RequestCountHttpStatus304": 0,
     "RequestCountHttpStatus404": 0,
     "RequestCountCacheHit": null,
     "RequestCountCacheMiss": null,
     "RequestCountCacheNoCache": null,
     "RequestCountCacheUncacheable": null,
     "RequestCountCacheOthers": null,
     "EgressTotal": 0.09,
     "EgressHttpStatus2xx": null,
     "EgressHttpStatus3xx": null,
     "EgressHttpStatus4xx": null,
     "EgressHttpStatus5xx": null,
     "EgressHttpStatusOthers": null,
     "EgressCacheHit": null,
     "EgressCacheMiss": null,
     "EgressCacheNoCache": null,
     "EgressCacheUncacheable": null,
     "EgressCacheOthers": null
}

```

## <a name="additional-resources"></a><span data-ttu-id="d37d1-414">További források</span><span class="sxs-lookup"><span data-stu-id="d37d1-414">Additional resources</span></span>

* [<span data-ttu-id="d37d1-415">Az Azure diagnosztikai naplók</span><span class="sxs-lookup"><span data-stu-id="d37d1-415">Azure Diagnostic logs</span></span>](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)
* [<span data-ttu-id="d37d1-416">Egyszerűsített analitika Azure CDN kiegészítő portálon keresztül</span><span class="sxs-lookup"><span data-stu-id="d37d1-416">Core analytics via Azure CDN supplemental portal</span></span>](https://docs.microsoft.com/azure/cdn/cdn-analyze-usage-patterns)
* [<span data-ttu-id="d37d1-417">Az Azure OMS szolgáltatáshoz</span><span class="sxs-lookup"><span data-stu-id="d37d1-417">Azure OMS Log Analytics</span></span>](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-overview)
* [<span data-ttu-id="d37d1-418">Az Azure Naplóelemzés REST API-n</span><span class="sxs-lookup"><span data-stu-id="d37d1-418">Azure Log Analytics REST API</span></span>](https://docs.microsoft.com/en-us/rest/api/loganalytics)







