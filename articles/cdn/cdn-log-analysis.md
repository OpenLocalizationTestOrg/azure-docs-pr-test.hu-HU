---
title: "Elemzés keresse meg a Azure CDN |} Microsoft Docs"
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
ms.openlocfilehash: 03ff74ae4e40d3f2279caaf4f73e9b4aac6a2ebb
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="diagnostics-logs-for-azure-cdn"></a><span data-ttu-id="6594f-103">Az Azure CDN diagnosztikai naplókat</span><span class="sxs-lookup"><span data-stu-id="6594f-103">Diagnostics Logs for Azure CDN</span></span>

<span data-ttu-id="6594f-104">Miután engedélyezte a CDN az alkalmazáshoz, valószínűleg érdemes a CDN használata, ellenőrizze, a kézbesítés állapotát, és lehetséges problémák hibaelhárítása.</span><span class="sxs-lookup"><span data-stu-id="6594f-104">After enabling CDN for your application, you will likely want to monitor the CDN usage, check the health of your delivery, and troubleshoot potential issues.</span></span> <span data-ttu-id="6594f-105">Az Azure CDN további olyan funkciókat kínál az [CDN egyszerűsített analitika](cdn-analyze-usage-patterns.md) és [diagnosztikai naplók](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)</span><span class="sxs-lookup"><span data-stu-id="6594f-105">Azure CDN provides these capabilities with [CDN Core Analytics](cdn-analyze-usage-patterns.md) and [Diagnostic Logs](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)</span></span>

## <a name="cdn-core-analytics"></a><span data-ttu-id="6594f-106">CDN egyszerűsített analitika</span><span class="sxs-lookup"><span data-stu-id="6594f-106">CDN Core Analytics</span></span>
<span data-ttu-id="6594f-107">Aktuális Azure CDN rendelkező felhasználóként Verizon standard vagy prémium profilhoz akkor még egyszerűsített analitika megtekintheti a kiegészítő portálon a "Kezelése" lehetőséget az Azure-portálon keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="6594f-107">As a current Azure CDN user with Verizon standard or premium profile, you are already able to view core analytics in the supplemental portal accessible via the "Manage" option from the Azure portal.</span></span> 

## <a name="azure-diagnostic-logs"></a><span data-ttu-id="6594f-108">Az Azure diagnosztikai naplók</span><span class="sxs-lookup"><span data-stu-id="6594f-108">Azure Diagnostic Logs</span></span>

<span data-ttu-id="6594f-109">Az új szolgáltatással az Azure, most már megtekintheti egyszerűsített analitika és mentse el egy vagy több célt, beleértve azokat:</span><span class="sxs-lookup"><span data-stu-id="6594f-109">Azure With this new feature, you can now view core analytics and save them into one or more destinations including:</span></span>

 - <span data-ttu-id="6594f-110">Azure Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="6594f-110">Azure Storage account</span></span>
 - <span data-ttu-id="6594f-111">Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="6594f-111">Azure Event Hubs</span></span>
 - [<span data-ttu-id="6594f-112">A Naplóelemzési OMS-tárház</span><span class="sxs-lookup"><span data-stu-id="6594f-112">OMS Log Analytics repository</span></span>](https://docs.microsoft.com/azure/log-analytics/log-analytics-get-started)
 
 <span data-ttu-id="6594f-113">Ez a szolgáltatás az összes CDN-végpontok Verizon (Standard és prémium) és a CDN-profilra Akamai (általános) tartozó érhető el.</span><span class="sxs-lookup"><span data-stu-id="6594f-113">This feature is available for all CDN endpoints belonging to Verizon (Standard & Premium) and Akamai (Standard) CDN Profiles.</span></span>

<span data-ttu-id="6594f-114">Diagnosztikai naplók alapvető a szoftverhasználati mérési adatok exportálása a CDN-végpontot különböző forrásokból, így képes felhasználni azokat egy egyedi módon teszik lehetővé.</span><span class="sxs-lookup"><span data-stu-id="6594f-114">Diagnostics logs allow you to export basic usage metrics from your CDN endpoint to a variety of sources so that you can consume them in a customized way.</span></span> <span data-ttu-id="6594f-115">Például a következő típusú adatok exportálása teheti meg:</span><span class="sxs-lookup"><span data-stu-id="6594f-115">For example, you can do the following types of data export:</span></span>

- <span data-ttu-id="6594f-116">Blob-tároló, Exportálás CSV-FÁJLBA, és létre diagramokat az adatok exportálása az excel.</span><span class="sxs-lookup"><span data-stu-id="6594f-116">Export data to blob storage, export to CSV, and generate graphs in excel.</span></span>
- <span data-ttu-id="6594f-117">Adatok exportálása az event hubs, és más azure-szolgáltatásokkal együtt összefüggéseket.</span><span class="sxs-lookup"><span data-stu-id="6594f-117">Export data to event hubs and correlate with data from other azure services.</span></span>
- <span data-ttu-id="6594f-118">Exportálhatja az adatokat a analytics naplózása, és a saját OMS-munkaterület adatainak megtekintéséhez</span><span class="sxs-lookup"><span data-stu-id="6594f-118">Export data to log analytics and view data in your own OMS work space</span></span>

<span data-ttu-id="6594f-119">Az alábbi ábrán egy tipikus CDN egyszerűsített analitika nézet adatokká.</span><span class="sxs-lookup"><span data-stu-id="6594f-119">The following figure shows a typical CDN Core Analytics view into data.</span></span>

![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/01_OMS-workspace.png)

<span data-ttu-id="6594f-121">*1. ábra – CDN egyszerűsített analitika megtekintése*</span><span class="sxs-lookup"><span data-stu-id="6594f-121">*Figure 1 - CDN Core Analytics view*</span></span>

<span data-ttu-id="6594f-122">A következő forgatókönyv végighalad a Adatséma core analytics, a funkció engedélyezése kézbesítéséhez azokat különböző helyekre és az ezekre a célokra fel lépéseit.</span><span class="sxs-lookup"><span data-stu-id="6594f-122">The following walkthrough goes through the schema of the core analytics data, steps involved in enabling the feature and delivering them to various destinations, and consuming from these destinations.</span></span>

## <a name="enable-logging-with-azure-portal"></a><span data-ttu-id="6594f-123">Az Azure-portálon naplózásának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="6594f-123">Enable logging with Azure portal</span></span>

> [!NOTE]
> <span data-ttu-id="6594f-124">A diagnosztikai naplók vannak kapcsolva **ki** alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="6594f-124">The diagnostics logs are turned **off** by default.</span></span> 

<span data-ttu-id="6594f-125">A CDN egyszerűsített analitika naplózás engedélyezéséhez az alábbi lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="6594f-125">Follow the steps below to enable logging with CDN Core Analytics:</span></span>

<span data-ttu-id="6594f-126">Jelentkezzen be az [Azure Portalra](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6594f-126">Sign in to the [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="6594f-127">Ha már nincs engedélyezve a munkafolyamat a CDN [Azure CDN engedélyezése](cdn-create-new-endpoint.md) a folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="6594f-127">If you don't already have CDN enabled for your workflow, [Enable Azure CDN](cdn-create-new-endpoint.md) before you continue.</span></span>

1. <span data-ttu-id="6594f-128">A portálon lépjen a **CDN-profil**.</span><span class="sxs-lookup"><span data-stu-id="6594f-128">In the portal, navigate to **CDN profile**.</span></span>
2. <span data-ttu-id="6594f-129">Válassza ki a CDN-profil, majd válassza ki, hogy engedélyezni szeretné a CDN-végpont **diagnosztikai naplók**.</span><span class="sxs-lookup"><span data-stu-id="6594f-129">Select a CDN profile, then select the CDN endpoint that you want to enable **Diagnostics Logs**.</span></span>

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/02_Browse-to-Diagnostics-logs.png)

3. <span data-ttu-id="6594f-131">Ugrás a **diagnosztikai naplók** részen **figyelés** területen, majd módosítsa az állapot **a**.</span><span class="sxs-lookup"><span data-stu-id="6594f-131">Go to **Diagnostics Logs** blade Under **Monitoring** section, then change the status to **On**.</span></span>

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/03_Diagnostics-logs-options.png)

### <a name="enable-logging-with-azure-storage"></a><span data-ttu-id="6594f-133">Az Azure Storage naplózásának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="6594f-133">Enable logging with Azure Storage</span></span>
    
<span data-ttu-id="6594f-134">A naplók tárolásához Azure Storage használatához válassza **tárfiókba archív**, válassza ki, megőrzés (nap), és kattintson **CoreAnalytics** alatt **napló**.</span><span class="sxs-lookup"><span data-stu-id="6594f-134">To use Azure Storage to store the logs, select **Archive to a storage account**, select retention days, and click **CoreAnalytics** under **Log**.</span></span>

![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/04_Diagnostics-logs-storage.png)

<span data-ttu-id="6594f-136">*2. ábra – az Azure Storage-naplózás*</span><span class="sxs-lookup"><span data-stu-id="6594f-136">*Figure 2 - Logging with Azure Storage*</span></span>

### <a name="logging-with-oms-log-analytics"></a><span data-ttu-id="6594f-137">Az OMS szolgáltatáshoz naplózása</span><span class="sxs-lookup"><span data-stu-id="6594f-137">Logging with OMS Log Analytics</span></span>

<span data-ttu-id="6594f-138">A naplók tárolásához OMS Naplóelemzési használatához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6594f-138">To use OMS Log Analytics to store the logs, follow these steps:</span></span>

1. <span data-ttu-id="6594f-139">Az a **diagnosztikai naplók** részen **figyelés**, jelölje be **küldeni a Naplóelemzési** a</span><span class="sxs-lookup"><span data-stu-id="6594f-139">From the **Diagnostics Logs** blade Under **Monitoring**, select **Send to Log Analytics** from</span></span> 

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/05_Ready-to-Configure.png)    

2. <span data-ttu-id="6594f-141">A Naplóelemzési naplózás konfigurálásához kattintson a konfigurálás.</span><span class="sxs-lookup"><span data-stu-id="6594f-141">Configure the Log Analytics logging by clicking on Configure.</span></span> <span data-ttu-id="6594f-142">Ezzel megnyitná egy párbeszédpanelt, amelyen egy előző munkaterületen válassza ki vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="6594f-142">This takes you to a dialog where you can select a previous workspace or create a new one.</span></span>

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/06_Choose-workspace.png)

3. <span data-ttu-id="6594f-144">Kattintson a **hozzon létre új munkaterületet**.</span><span class="sxs-lookup"><span data-stu-id="6594f-144">Click **Create New Workspace**.</span></span>

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/07_Create-new.png)

4. <span data-ttu-id="6594f-146">Ezután ki kell választania egy új nevet a munkaterület, meglévő előfizetés, erőforráscsoport (új vagy meglévő), hely és a tarifacsomag.</span><span class="sxs-lookup"><span data-stu-id="6594f-146">Next you must select a new workspace name, existing subscription, resource group (new or existing), location, and pricing tier.</span></span> <span data-ttu-id="6594f-147">Lehetősége van az ebben a konfigurációban az irányítópulton való rögzítéshez.</span><span class="sxs-lookup"><span data-stu-id="6594f-147">You have the option of pinning this configuration to your dashboard.</span></span> <span data-ttu-id="6594f-148">Kattintson az OK gombra a konfiguráció befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="6594f-148">Click OK to complete the configuration.</span></span>

    <span data-ttu-id="6594f-149">Ezután meg kell jelennie a munkaterület az OMS-munkaterület és a erőforrás csoport nevével.</span><span class="sxs-lookup"><span data-stu-id="6594f-149">Next you should see your workspace with your OMS Workspace and Resource group names.</span></span> <span data-ttu-id="6594f-150">Nevének egyedinek kell lennie, és csak betűket, számokat és kötőjeleket tartalmazhat használhat.</span><span class="sxs-lookup"><span data-stu-id="6594f-150">Names must be unique and can only use letters, numbers, and hyphens.</span></span> <span data-ttu-id="6594f-151">Szóközöket és aláhúzásjeleket tartalmazhat nem engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="6594f-151">Spaces and underscores are not allowed.</span></span> 

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/08_Workspace-resource.png)

5. <span data-ttu-id="6594f-153">Ezután arról, hogy a munkaterület elkészült, és ismét a naplózási konfigurálására szolgáló képernyőn rövid üzenetet kapni.</span><span class="sxs-lookup"><span data-stu-id="6594f-153">You next get a short message saying that your workspace has been created and you are returned to your logging configuration screen.</span></span> <span data-ttu-id="6594f-154">Ellenőrizheti a Naplóelemzési munkaterület nevét.</span><span class="sxs-lookup"><span data-stu-id="6594f-154">You can confirm the name of your Log Analytics workspace.</span></span>

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/09_Return-to-logging.png)

    <span data-ttu-id="6594f-156">A Naplóelemzési konfigurációs beállítása után győződjön meg arról is jelölje be a CDN a naplózás a CoreAnalytics jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="6594f-156">Once you have set up the Log Analytics configuration, make sure you also check the CoreAnalytics box for CDN logging.</span></span>

6. <span data-ttu-id="6594f-157">Ha minden tetszőlegesen a, kattintson a **mentése** gombra a konfigurációs párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="6594f-157">If everything is to your liking, click the **Save** button at the top of the configuration dialog.</span></span>

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/10_Save-me.png)

    <span data-ttu-id="6594f-159">A **mentése** gomb már nem aktív, és, hogy a be-és kikapcsolása gomb jelenleg a, de a kék lila helyett.</span><span class="sxs-lookup"><span data-stu-id="6594f-159">The **Save** button is no longer active and that the ON/OFF button is now ON, but blue instead of purple.</span></span>

7. <span data-ttu-id="6594f-160">Ha azt szeretné, az új OMS-munkaterület megtekintéséhez nyissa meg az Azure-portálra irányítópultot, kattintson a Naplóelemzési munkaterület nevét.</span><span class="sxs-lookup"><span data-stu-id="6594f-160">If you want to see your new OMS workspace, go to your Azure portal Dashboard, click the name of your Log Analytics workspace.</span></span> <span data-ttu-id="6594f-161">Ezután megjelenik a munkaterület (Győződjön meg arról, hogy OMS-munkaterület ki van jelölve, a bal oldalon).</span><span class="sxs-lookup"><span data-stu-id="6594f-161">Next you will see your workspace (make sure that OMS Workspace is highlighted on the left).</span></span> <span data-ttu-id="6594f-162">Kattintson a csempére a munkaterület az OMS-tárházban megjelenítéséhez OMS-portálon.</span><span class="sxs-lookup"><span data-stu-id="6594f-162">Click on the OMS Portal tile to see your workspace in the OMS repository.</span></span> 

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/11_OMS-dashboard.png) 

    <span data-ttu-id="6594f-164">Az OMS-tárház adatokat naplózhatnak készen áll.</span><span class="sxs-lookup"><span data-stu-id="6594f-164">Your OMS repository is now ready to log data.</span></span> <span data-ttu-id="6594f-165">Adatok felhasználásához, használjon egy [OMS megoldás](#consuming-oms-log-analytics-data), az érintett a cikk későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="6594f-165">In order to consume that data, you must use an [OMS Solution](#consuming-oms-log-analytics-data), covered later in this article.</span></span>

<span data-ttu-id="6594f-166">További információt a naplózási adatok késések [Itt](#log-data-delays).</span><span class="sxs-lookup"><span data-stu-id="6594f-166">For more information about log data delays, go [here](#log-data-delays).</span></span>

## <a name="enable-logging-with-powershell"></a><span data-ttu-id="6594f-167">A PowerShell-lel naplózásának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="6594f-167">Enable logging with PowerShell</span></span>

<span data-ttu-id="6594f-168">Alább példája engedélyezése és az Azure PowerShell-parancsmagok segítségével diagnosztikai naplófájlok.</span><span class="sxs-lookup"><span data-stu-id="6594f-168">Below is an example on how to enable and get Diagnostic Logs via the Azure PowerShell Cmdlets.</span></span>

###<a name="enabling-diagnostic-logs-in-a-storage-account"></a><span data-ttu-id="6594f-169">Diagnosztika engedélyezése bejelentkezik a Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="6594f-169">Enabling Diagnostic Logs in a Storage Account</span></span>

<span data-ttu-id="6594f-170">Először jelentkezzen be, és válasszon egy előfizetést:</span><span class="sxs-lookup"><span data-stu-id="6594f-170">First log in and select a subscription:</span></span>

    Login-AzureRmAccount 

    Select-AzureSubscription -SubscriptionId 


<span data-ttu-id="6594f-171">Engedélyezze a diagnosztikai naplók, a Tárfiók használja ezt a parancsot:</span><span class="sxs-lookup"><span data-stu-id="6594f-171">To Enable Diagnostic Logs in a Storage Account, use this command:</span></span>

```powershell
    Set-AzureRmDiagnosticSetting -ResourceId "/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Cdn/profiles/{profileName}/endpoints/{endpointName}" -StorageAccountId "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicStorage/storageAccounts/{storageAccountName}" -Enabled $true -Categories CoreAnalytics
```
<span data-ttu-id="6594f-172">Engedélyezése diagnosztikai naplófájlok az OMS-munkaterület használja ezt a parancsot:</span><span class="sxs-lookup"><span data-stu-id="6594f-172">To Enable Diagnostics Logs in an OMS workspace, use this command:</span></span>

```powershell
    Set-AzureRmDiagnosticSetting -ResourceId "/subscriptions/`{subscriptionId}<subscriptionId>
    .<subscriptionName>" -WorkspaceId "/subscriptions/<workspaceId>.<workspaceName>" -Enabled $true -Categories CoreAnalytics 
```



## <a name="consuming-diagnostics-logs-from-azure-storage"></a><span data-ttu-id="6594f-173">Diagnosztikai naplók az Azure Storage felhasználása</span><span class="sxs-lookup"><span data-stu-id="6594f-173">Consuming diagnostics logs from Azure Storage</span></span>
<span data-ttu-id="6594f-174">Ez a szakasz ismerteti a séma, a CDN egyszerűsített analitika hogyan ezek egy Azure Storage-fiók belül vannak rendezve, és a naplók letöltése CSV-fájlba példakódot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="6594f-174">This section describes the schema of the CDN core analytics, how these are organized inside of an Azure Storage Account and provides sample code to download the logs in to a CSV file.</span></span>

### <a name="using-microsoft-azure-storage-explorer"></a><span data-ttu-id="6594f-175">A Microsoft Azure Tártallózó használatával</span><span class="sxs-lookup"><span data-stu-id="6594f-175">Using Microsoft Azure Storage Explorer</span></span>
<span data-ttu-id="6594f-176">Az alapvető analitikai adatok eléréséhez az Azure Storage-fiókjához, először egy eszköz tárfiókokban tartalmához való hozzáféréshez.</span><span class="sxs-lookup"><span data-stu-id="6594f-176">Before you can access the core analytics data from the Azure Storage Account, you first need a tool to access the contents in a storage account.</span></span> <span data-ttu-id="6594f-177">Eszközök is elérhetők több a piacon, amíg azt, amelyik ajánlott a Microsoft Azure Tártallózó.</span><span class="sxs-lookup"><span data-stu-id="6594f-177">While there are several tools available in the market, the one that we recommend is the Microsoft Azure Storage Explorer.</span></span> <span data-ttu-id="6594f-178">Letöltheti a eszközt [Itt](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="6594f-178">You can download the tool from [here](http://storageexplorer.com/).</span></span> <span data-ttu-id="6594f-179">Után a szoftver letöltése és telepítése a, konfigurálja úgy, hogy az azonos Azure Storage-fiókot adott meg, hova szeretné a CDN diagnosztikai naplók használja.</span><span class="sxs-lookup"><span data-stu-id="6594f-179">After downloading and installing the software, configure it to use the same Azure Storage Account that was configured as a destination to the CDN Diagnostics Logs.</span></span>

1.  <span data-ttu-id="6594f-180">Nyissa meg **Microsoft Azure Tártallózó**</span><span class="sxs-lookup"><span data-stu-id="6594f-180">Open **Microsoft Azure Storage Explorer**</span></span>
2.  <span data-ttu-id="6594f-181">Keresse meg a storage-fiók</span><span class="sxs-lookup"><span data-stu-id="6594f-181">Locate the storage account</span></span>
3.  <span data-ttu-id="6594f-182">Lépjen a **"Blobtárolók"** csomópont alatt ez a tároló fiókot, és bontsa ki a csomópontot</span><span class="sxs-lookup"><span data-stu-id="6594f-182">Go to the **“Blob Containers”** node under this storage account and expand the node</span></span>
4.  <span data-ttu-id="6594f-183">Válassza ki a tárolót **"insights-logs-coreanalytics"** , és kattintson rá duplán</span><span class="sxs-lookup"><span data-stu-id="6594f-183">Select the container named **“insights-logs-coreanalytics”** and double-click it</span></span>
5.  <span data-ttu-id="6594f-184">Annak az eredménye megjelenítése fel az első szintjét, mely tűnik kezdve a jobb oldali ablaktáblában lévő **"resourceId ="**.</span><span class="sxs-lookup"><span data-stu-id="6594f-184">Results show up on the right-hand pane starting with the first level, which looks like **“resourceId=”**.</span></span> <span data-ttu-id="6594f-185">Továbbra is, egészen amíg meg nem jelenik a fájl kattintva **PT1H.json**.</span><span class="sxs-lookup"><span data-stu-id="6594f-185">Continue clicking all the way until you see the file **PT1H.json**.</span></span> <span data-ttu-id="6594f-186">Lásd a következő megjegyzést, az elérési út ismertetése.</span><span class="sxs-lookup"><span data-stu-id="6594f-186">See the following note for explanation of the path.</span></span>
6.  <span data-ttu-id="6594f-187">Minden egyes blob **PT1H.json** számára egy adott CDN-végpont vagy tartozó egyéni tartomány egy órában az elemzési naplókat jelöli.</span><span class="sxs-lookup"><span data-stu-id="6594f-187">Each blob **PT1H.json** represents the analytics logs for one hour for a specific CDN endpoint or its custom domain.</span></span>
7.  <span data-ttu-id="6594f-188">A sémát a JSON-fájl tartalmának a Core Analytics naplók a séma szakaszban ismertetett</span><span class="sxs-lookup"><span data-stu-id="6594f-188">The schema of the contents of this JSON file is described in the section Schema of the Core Analytics Logs</span></span>


> [!NOTE]
> <span data-ttu-id="6594f-189">**A BLOB elérési út formátuma**</span><span class="sxs-lookup"><span data-stu-id="6594f-189">**Blob path format**</span></span>
> 
> <span data-ttu-id="6594f-190">Core Analytics naplók óránként akkor jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="6594f-190">Core Analytics logs are generated every hour.</span></span> <span data-ttu-id="6594f-191">Minden adat egy óráig gyűjtött és tárolt JSON-adatként egyetlen Azure Blob.</span><span class="sxs-lookup"><span data-stu-id="6594f-191">All data for an hour are collected and stored inside a single Azure Blob as a JSON payload.</span></span> <span data-ttu-id="6594f-192">Az Azure Blob elérési jelenik meg, hogy van-e olyan hierarchikus struktúra.</span><span class="sxs-lookup"><span data-stu-id="6594f-192">The path to this Azure Blob appears as if there is a hierarchical structure.</span></span> <span data-ttu-id="6594f-193">Ez van, mert a tárolók explorer eszköz értelmezi "/" értelmezi, és megjeleníti a hierarchia kényelmét szolgálja.</span><span class="sxs-lookup"><span data-stu-id="6594f-193">This is because the Storage explorer tool interprets '/' as a directory separator and shows the hierarchy for convenience.</span></span> <span data-ttu-id="6594f-194">A teljes elérési útja ténylegesen, csak a blob nevét jelöli.</span><span class="sxs-lookup"><span data-stu-id="6594f-194">Actually, the whole path just represents the blob name.</span></span> <span data-ttu-id="6594f-195">Ez a blob neve követi a következő elnevezés szabály szerint</span><span class="sxs-lookup"><span data-stu-id="6594f-195">This name of the blob follows the following naming convention</span></span>   
    
    resourceId=/SUBSCRIPTIONS/{Subscription Id}/RESOURCEGROUPS/{Resource Group Name}/PROVIDERS/MICROSOFT.CDN/PROFILES/{Profile Name}/ENDPOINTS/{Endpoint Name}/ y={Year}/m={Month}/d={Day}/h={Hour}/m={Minutes}/PT1H.json

<span data-ttu-id="6594f-196">**Mezők leírása:**</span><span class="sxs-lookup"><span data-stu-id="6594f-196">**Description of fields:**</span></span>

|<span data-ttu-id="6594f-197">érték</span><span class="sxs-lookup"><span data-stu-id="6594f-197">value</span></span>|<span data-ttu-id="6594f-198">Leírás</span><span class="sxs-lookup"><span data-stu-id="6594f-198">description</span></span>|
|-------|---------|
|<span data-ttu-id="6594f-199">Előfizetés azonosítója</span><span class="sxs-lookup"><span data-stu-id="6594f-199">Subscription ID</span></span>    |<span data-ttu-id="6594f-200">Az Azure-előfizetés azonosítója.</span><span class="sxs-lookup"><span data-stu-id="6594f-200">ID of the Azure Subscription.</span></span> <span data-ttu-id="6594f-201">Ez a Guid formátumban van.</span><span class="sxs-lookup"><span data-stu-id="6594f-201">This is in the Guid format.</span></span>|
|<span data-ttu-id="6594f-202">Erőforrás</span><span class="sxs-lookup"><span data-stu-id="6594f-202">Resource</span></span> |<span data-ttu-id="6594f-203">Csoport neve neve az erőforráscsoport, amelybe a CDN-erőforrások tartoznak.</span><span class="sxs-lookup"><span data-stu-id="6594f-203">Group Name   Name of the resource group to which the CDN resources belong.</span></span>|
|<span data-ttu-id="6594f-204">Profilnév</span><span class="sxs-lookup"><span data-stu-id="6594f-204">Profile Name</span></span> |<span data-ttu-id="6594f-205">A CDN-profil neve</span><span class="sxs-lookup"><span data-stu-id="6594f-205">Name of the CDN Profile</span></span>|
|<span data-ttu-id="6594f-206">A végpont neve</span><span class="sxs-lookup"><span data-stu-id="6594f-206">Endpoint Name</span></span> |<span data-ttu-id="6594f-207">A CDN-végpont neve</span><span class="sxs-lookup"><span data-stu-id="6594f-207">Name of the CDN Endpoint</span></span>|
|<span data-ttu-id="6594f-208">Év</span><span class="sxs-lookup"><span data-stu-id="6594f-208">Year</span></span>|  <span data-ttu-id="6594f-209">az év például 2017 4 számjegyből álló ábrázolása</span><span class="sxs-lookup"><span data-stu-id="6594f-209">4-digit representation of the year for example, 2017</span></span>|
|<span data-ttu-id="6594f-210">Hónap</span><span class="sxs-lookup"><span data-stu-id="6594f-210">Month</span></span>| <span data-ttu-id="6594f-211">a hónapok sorszáma 2 számjegyű ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="6594f-211">2-digit representation of the month number.</span></span> <span data-ttu-id="6594f-212">01 január =... 12 decembert jelenti – =</span><span class="sxs-lookup"><span data-stu-id="6594f-212">01=January ... 12=December</span></span>|
|<span data-ttu-id="6594f-213">Nap</span><span class="sxs-lookup"><span data-stu-id="6594f-213">Day</span></span>|   <span data-ttu-id="6594f-214">a hónap napját 2 számjegy ábrázolása</span><span class="sxs-lookup"><span data-stu-id="6594f-214">2 digit representation of the day of the month</span></span>|
|<span data-ttu-id="6594f-215">PT1H.JSON</span><span class="sxs-lookup"><span data-stu-id="6594f-215">PT1H.json</span></span>| <span data-ttu-id="6594f-216">Az analitikai adatok tárolására tényleges JSON-fájl</span><span class="sxs-lookup"><span data-stu-id="6594f-216">Actual JSON file where the analytics data is stored</span></span>|

### <a name="exporting-the-core-analytics-data-to-a-csv-file"></a><span data-ttu-id="6594f-217">Az alapvető analitikai adatok exportálása CSV-fájlba</span><span class="sxs-lookup"><span data-stu-id="6594f-217">Exporting the Core Analytics Data to a CSV File</span></span>

<span data-ttu-id="6594f-218">Abba, hogy könnyen elérhetők az egyszerűsített analitika, azt adja meg egy minta tartozó kódot egy eszközt, amely lehetővé teszi, hogy a JSON-fájlok letöltésére olyan egyszerű vesszővel tagolt fájl formátumra, amely könnyen hozzanak létre diagramokat vagy más összesítéseket használható.</span><span class="sxs-lookup"><span data-stu-id="6594f-218">To make it easy to access the Core Analytics, we provide a sample code for a tool, which allows downloading the JSON files into a flat comma-separated file format, which can be used to easily create charts or other aggregations.</span></span>

<span data-ttu-id="6594f-219">Ez az eszköz használatát:</span><span class="sxs-lookup"><span data-stu-id="6594f-219">Here is how you can use the tool:</span></span>

1.  <span data-ttu-id="6594f-220">Látogasson el a github-címre: [https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv](https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv )</span><span class="sxs-lookup"><span data-stu-id="6594f-220">Visit the github link: [https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv ](https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv )</span></span>
2.  <span data-ttu-id="6594f-221">A kód letöltése</span><span class="sxs-lookup"><span data-stu-id="6594f-221">Download the code</span></span>
3.  <span data-ttu-id="6594f-222">Útmutatás alapján fordításához és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6594f-222">Follow instructions to compile and configure</span></span>
4.  <span data-ttu-id="6594f-223">Futtassa az eszközt</span><span class="sxs-lookup"><span data-stu-id="6594f-223">Run the tool</span></span>
5.  <span data-ttu-id="6594f-224">Letöltött CSV-fájlt egy egyszerű strukturálatlan hierarchia analytics adatainak megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="6594f-224">Resulting CSV file shows the analytics data in a simple flat hierarchy.</span></span>

## <a name="consuming-diagnostics-logs-from-an-oms-log-analytics-repository"></a><span data-ttu-id="6594f-225">Diagnosztikai naplók az OMS szolgáltatáshoz tárházból felhasználása</span><span class="sxs-lookup"><span data-stu-id="6594f-225">Consuming diagnostics logs from an OMS Log Analytics repository</span></span>
<span data-ttu-id="6594f-226">A Naplóelemzési egy olyan szolgáltatás, az Operations Management Suite (OMS), amely figyeli a felhőalapú és helyszíni környezetek karbantartásához azok rendelkezésre állását és teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="6594f-226">Log Analytics is a service in Operations Management Suite (OMS) that monitors your cloud and on-premises environments to maintain their availability and performance.</span></span> <span data-ttu-id="6594f-227">A felhőben és a helyszíni környezetben található erőforrások által létrehozott, valamint egyéb figyelési eszközök által biztosított adatokat gyűjtésével biztosítsa elemzést több forráson.</span><span class="sxs-lookup"><span data-stu-id="6594f-227">It collects data generated by resources in your cloud and on-premises environments and from other monitoring tools to provide analysis across multiple sources.</span></span> 

<span data-ttu-id="6594f-228">Naplóelemzési használandó kell [naplózását](#enable-logging-with-azure-storage) Azure OMS Log Analytics-tárházba, amely tárgyalt az ebben a cikkben.</span><span class="sxs-lookup"><span data-stu-id="6594f-228">To use Log Analytics, you must [enable logging](#enable-logging-with-azure-storage) to the Azure OMS Log Analytics repository, which is discussed earlier in this article.</span></span>

### <a name="using-the-oms-repository"></a><span data-ttu-id="6594f-229">Az OMS-tárház használatával</span><span class="sxs-lookup"><span data-stu-id="6594f-229">Using the OMS Repository</span></span>

 <span data-ttu-id="6594f-230">Az alábbi ábrán látható, a tárház kimenetek és a bemenetek architektúrája:</span><span class="sxs-lookup"><span data-stu-id="6594f-230">The following diagram shows the architecture of the inputs and outputs of the repository:</span></span>

![OMS napló Analytics tárház](./media/cdn-diagnostics-log/12_Repo-overview.png)

<span data-ttu-id="6594f-232">*3. ábra - napló Analytics tárház*</span><span class="sxs-lookup"><span data-stu-id="6594f-232">*Figure 3 - Log Analytics Repository*</span></span>

<span data-ttu-id="6594f-233">Az sokféleképpen megoldások használatával megjelenítheti az adatokat.</span><span class="sxs-lookup"><span data-stu-id="6594f-233">You can display the data in a variety of ways by using Management Solutions.</span></span> <span data-ttu-id="6594f-234">Ezt úgy szerezheti be a felügyeleti megoldás a [Azure piactér](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/monitoring-management?page=1&subcategories=management-solutions).</span><span class="sxs-lookup"><span data-stu-id="6594f-234">You can obtain Management Solutions from the [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/monitoring-management?page=1&subcategories=management-solutions).</span></span>

<span data-ttu-id="6594f-235">Telepíthető megoldások Azure piactérről kattintva a **most töltse le innen** hivatkozás az egyes megoldások alján.</span><span class="sxs-lookup"><span data-stu-id="6594f-235">You can install management solutions from Azure marketplace by clicking the **Get it now** link at the bottom of each solution.</span></span>

### <a name="adding-an-oms-cdn-management-solution"></a><span data-ttu-id="6594f-236">Egy OMS CDN-felügyeleti megoldás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="6594f-236">Adding an OMS CDN Management Solution</span></span>

<span data-ttu-id="6594f-237">Kövesse az alábbi lépéseket a felügyeleti megoldás hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="6594f-237">Follow these steps to add a Management Solution:</span></span>

1.   <span data-ttu-id="6594f-238">Ha még nem tette meg, jelentkezzen be az Azure-előfizetéshez az Azure portálra, és nyissa meg az irányítópulton való rögzítéséhez.</span><span class="sxs-lookup"><span data-stu-id="6594f-238">If you haven't already done so, sign in to the Azure portal using your Azure subscription and go to your Dashboard.</span></span>
    <span data-ttu-id="6594f-239">![Az Azure irányítópult](./media/cdn-diagnostics-log/13_Azure-dashboard.png)</span><span class="sxs-lookup"><span data-stu-id="6594f-239">![Azure Dashboard](./media/cdn-diagnostics-log/13_Azure-dashboard.png)</span></span>

2. <span data-ttu-id="6594f-240">Az a **új** részen **piactér**, jelölje be **figyelés + felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="6594f-240">In the **New** blade under **Marketplace**, select **Monitoring + management**.</span></span>

    ![Piactér](./media/cdn-diagnostics-log/14_Marketplace.png)

3. <span data-ttu-id="6594f-242">Az a **figyelés + felügyeleti** panelen kattintson a **láthatja az összes**.</span><span class="sxs-lookup"><span data-stu-id="6594f-242">In the **Monitoring + management** blade, click **See all**.</span></span>

    ![Az összes megtekintése](./media/cdn-diagnostics-log/15_See-all.png)

4.  <span data-ttu-id="6594f-244">Keresse meg a CDN a Keresés mezőbe.</span><span class="sxs-lookup"><span data-stu-id="6594f-244">Search for CDN in the search box.</span></span>

    ![Az összes megtekintése](./media/cdn-diagnostics-log/16_Search-for.png)

5.  <span data-ttu-id="6594f-246">Válassza ki **Azure CDN egyszerűsített analitika**.</span><span class="sxs-lookup"><span data-stu-id="6594f-246">Select **Azure CDN Core Analytics**.</span></span> 

    ![Az összes megtekintése](./media/cdn-diagnostics-log/17_Core-analytics.png)

6.  <span data-ttu-id="6594f-248">Miután rákattintott **létrehozása**, kérni fog egy új OMS-munkaterület létrehozása, vagy használjon egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="6594f-248">After clicking **Create**, you will be asked to create a new OMS workspace or use an existing one.</span></span> 

    ![Az összes megtekintése](./media/cdn-diagnostics-log/18_Adding-solution.png)

7.  <span data-ttu-id="6594f-250">Válassza ki a munkaterületet előtt.</span><span class="sxs-lookup"><span data-stu-id="6594f-250">Select the workspace you created before.</span></span> <span data-ttu-id="6594f-251">Majd kell hozzáadnia az automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="6594f-251">You then need to add an automation account.</span></span>

    ![Az összes megtekintése](./media/cdn-diagnostics-log/19_Add-automation.png)

8. <span data-ttu-id="6594f-253">Az alábbi képernyőn látható az automatizálási fiók formában adja meg az.</span><span class="sxs-lookup"><span data-stu-id="6594f-253">The following screen shows the automation account form you must fill out.</span></span> 

    ![Az összes megtekintése](./media/cdn-diagnostics-log/20_Automation.png)

9. <span data-ttu-id="6594f-255">Az automation-fiók létrehozását követően készen áll a megoldás hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="6594f-255">Once you have created the automation account, you are ready to add your solution.</span></span> <span data-ttu-id="6594f-256">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="6594f-256">Click the **Create** button.</span></span>

    ![Az összes megtekintése](./media/cdn-diagnostics-log/21_Ready.png)

10. <span data-ttu-id="6594f-258">A megoldás most már fel lett véve a munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="6594f-258">Your solution has now been added to your workspace.</span></span> <span data-ttu-id="6594f-259">Térjen vissza az Azure portál irányítópultján.</span><span class="sxs-lookup"><span data-stu-id="6594f-259">Go back to your Azure portal Dashboard.</span></span>

    ![Az összes megtekintése](./media/cdn-diagnostics-log/22_Dashboard.png)

    <span data-ttu-id="6594f-261">Kattintson a Naplóelemzési munkaterület létrehozott nyissa meg a munkaterületet.</span><span class="sxs-lookup"><span data-stu-id="6594f-261">Click the Log Analytics workspace you created to go to your workspace.</span></span> 

11. <span data-ttu-id="6594f-262">Kattintson a **OMS-portálon** csempe megtekintéséhez az új megoldás az OMS-portálon.</span><span class="sxs-lookup"><span data-stu-id="6594f-262">Click the **OMS Portal** tile to see your new solution in the OMS portal.</span></span>

    ![Az összes megtekintése](./media/cdn-diagnostics-log/23_workspace.png)

12. <span data-ttu-id="6594f-264">Az OMS-portálon mostantól a következő képernyő hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="6594f-264">Your OMS portal should now look like the following screen:</span></span>

    ![Az összes megtekintése](./media/cdn-diagnostics-log/24_OMS-solution.png)

    <span data-ttu-id="6594f-266">Kattintson az áttekintőlapon megjeleníteni az adatokat több nézet.</span><span class="sxs-lookup"><span data-stu-id="6594f-266">Click one of the tiles to see several views into your data.</span></span>

    ![Az összes megtekintése](./media/cdn-diagnostics-log/25_Interior-view.png)

    <span data-ttu-id="6594f-268">Tekintse meg az adatokat az egyes nézetek képviselő további csempék jobbra vagy balra görgetve.</span><span class="sxs-lookup"><span data-stu-id="6594f-268">You can scroll left or right to see further tiles representing individual views into the data.</span></span> 

    <span data-ttu-id="6594f-269">A csempék valamelyikére kattintva lehetővé teszi az adatok további információt.</span><span class="sxs-lookup"><span data-stu-id="6594f-269">Clicking one of the tiles gives you more details about your data.</span></span>

     ![Az összes megtekintése](./media/cdn-diagnostics-log/26_Further-detail.png)

### <a name="offers-and-pricing-tiers"></a><span data-ttu-id="6594f-271">Ajánlatok és tarifacsomagok</span><span class="sxs-lookup"><span data-stu-id="6594f-271">Offers and pricing tiers</span></span>

<span data-ttu-id="6594f-272">Ajánlatok és az OMS-kezelési megoldások tarifacsomagok [Itt](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-add-solutions#offers-and-pricing-tiers).</span><span class="sxs-lookup"><span data-stu-id="6594f-272">You can see offers and pricing tiers for OMS management solutions [here](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-add-solutions#offers-and-pricing-tiers).</span></span>

### <a name="customizing-views"></a><span data-ttu-id="6594f-273">Nézetek testreszabása</span><span class="sxs-lookup"><span data-stu-id="6594f-273">Customizing views</span></span>

<span data-ttu-id="6594f-274">Testre szabhatja a nézet a adatokká használatával a **adatforrásnézet-tervezőből**.</span><span class="sxs-lookup"><span data-stu-id="6594f-274">You can customize the view into your data by using the **View Designer**.</span></span> <span data-ttu-id="6594f-275">Nyissa meg az OMS-munkaterület és designing megkezdéséhez kattintson a **adatforrásnézet-tervezőből** csempére.</span><span class="sxs-lookup"><span data-stu-id="6594f-275">Go to your OMS workspace and begin designing by clicking the **View Designer** tile.</span></span>

![Nézettervező](./media/cdn-diagnostics-log/27_Designer.png)

<span data-ttu-id="6594f-277">Húzza és diagramok típusú elvetni a bal oldali, és töltse ki a bal oldali elemezni kívánt adatok részleteit.</span><span class="sxs-lookup"><span data-stu-id="6594f-277">You can drag and drop types of charts from the left and fill in the data details you want to analyze on the left.</span></span>

![Nézettervező](./media/cdn-diagnostics-log/28_Designer.png)

    
## <a name="log-data-delays"></a><span data-ttu-id="6594f-279">Naplózási adatok késleltetése</span><span class="sxs-lookup"><span data-stu-id="6594f-279">Log data delays</span></span>

<span data-ttu-id="6594f-280">Verizon napló adatok késleltetése</span><span class="sxs-lookup"><span data-stu-id="6594f-280">Verizon log data delays</span></span> | <span data-ttu-id="6594f-281">Akamai napló adatok késleltetése</span><span class="sxs-lookup"><span data-stu-id="6594f-281">Akamai log data delays</span></span>
--- | ---
<span data-ttu-id="6594f-282">Verizon naplóadatokat 1 óra késleltetett, és indítsa el a végpont-propagálás befejezését követően megjelenő 2 órát igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="6594f-282">Verizon log data is 1 hour delayed, and take up to 2 hours to start appearing after endpoint propagation completion.</span></span> | <span data-ttu-id="6594f-283">Akamai naplóadatokat késleltetett 24 óra, és a start jelenik meg, ha több mint 24 órája létrehozták 2 órát vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="6594f-283">Akamai log data is 24 hours delayed, and takes up to 2 hours to start appearing if it was created more than 24 hours ago.</span></span> <span data-ttu-id="6594f-284">Ha nemrég készült, start jelenik meg a naplófájlokat akár 25 órát is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="6594f-284">If it was recently created, it can take up to 25 hours for the logs to start appearing.</span></span>

## <a name="diagnostic-log-types-for-cdn-core-analytics"></a><span data-ttu-id="6594f-285">Diagnosztikai naplófájl típusokat CDN egyszerűsített analitika</span><span class="sxs-lookup"><span data-stu-id="6594f-285">Diagnostic log types for CDN Core Analytics</span></span>

<span data-ttu-id="6594f-286">Jelenleg csak egyszerűsített analitika naplók, metrikák HTTP-válaszok statisztikai adatainak és a kimenő forgalom statisztika alapegységét megjelenítő a CDN POP/széleit tartalmazó fel.</span><span class="sxs-lookup"><span data-stu-id="6594f-286">We currently offer only Core Analytics logs, which contain metrics showing HTTP response statistics and egress statistics as seen from the CDN POPs/edges.</span></span>

### <a name="core-analytics-metrics-details"></a><span data-ttu-id="6594f-287">Core Analytics metrikák részletei</span><span class="sxs-lookup"><span data-stu-id="6594f-287">Core Analytics Metrics Details</span></span>
<span data-ttu-id="6594f-288">A következő táblázat az egyszerűsített analitika naplókban elérhető metrikák listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="6594f-288">The following table shows a list of metrics available in the Core Analytics logs.</span></span> <span data-ttu-id="6594f-289">Nem minden metrikák érhetők el minden szolgáltató, annak ellenére, hogy ezek az eltérések minimális.</span><span class="sxs-lookup"><span data-stu-id="6594f-289">Not all metrics are available from all providers, although such differences are minimal.</span></span> <span data-ttu-id="6594f-290">Az alábbi táblázatban is látható, ha egy metrika érhető el a szolgáltató által.</span><span class="sxs-lookup"><span data-stu-id="6594f-290">The following table also shows if a given metric is available from a provider.</span></span> <span data-ttu-id="6594f-291">Vegye figyelembe, hogy a metrikák érhetők el csak ezek CDN-végpontok, amelyek azokat a forgalmat.</span><span class="sxs-lookup"><span data-stu-id="6594f-291">Please note that the metrics are available for only those CDN endpoints that have traffic on them.</span></span>


|<span data-ttu-id="6594f-292">Metrika</span><span class="sxs-lookup"><span data-stu-id="6594f-292">Metric</span></span>                     | <span data-ttu-id="6594f-293">Leírás</span><span class="sxs-lookup"><span data-stu-id="6594f-293">Description</span></span>   | <span data-ttu-id="6594f-294">Verizon</span><span class="sxs-lookup"><span data-stu-id="6594f-294">Verizon</span></span>  | <span data-ttu-id="6594f-295">Akamai</span><span class="sxs-lookup"><span data-stu-id="6594f-295">Akamai</span></span> 
|---------------------------|---------------|---|---|
| <span data-ttu-id="6594f-296">RequestCountTotal</span><span class="sxs-lookup"><span data-stu-id="6594f-296">RequestCountTotal</span></span>         |<span data-ttu-id="6594f-297">Ebben az időszakban kérést találatok száma összesen</span><span class="sxs-lookup"><span data-stu-id="6594f-297">Total number of request hits during this period</span></span>| <span data-ttu-id="6594f-298">Igen</span><span class="sxs-lookup"><span data-stu-id="6594f-298">Yes</span></span>  |<span data-ttu-id="6594f-299">Igen</span><span class="sxs-lookup"><span data-stu-id="6594f-299">Yes</span></span>   |
| <span data-ttu-id="6594f-300">RequestCountHttpStatus2xx</span><span class="sxs-lookup"><span data-stu-id="6594f-300">RequestCountHttpStatus2xx</span></span> |<span data-ttu-id="6594f-301">Egy 2xx HTTP kódot (pl. 200, 202) eredményezett összes kérelem száma</span><span class="sxs-lookup"><span data-stu-id="6594f-301">Count of all requests that resulted in a 2xx HTTP code (e.g. 200, 202)</span></span>              | <span data-ttu-id="6594f-302">Igen</span><span class="sxs-lookup"><span data-stu-id="6594f-302">Yes</span></span>  |<span data-ttu-id="6594f-303">Igen</span><span class="sxs-lookup"><span data-stu-id="6594f-303">Yes</span></span>   |
| <span data-ttu-id="6594f-304">RequestCountHttpStatus3xx</span><span class="sxs-lookup"><span data-stu-id="6594f-304">RequestCountHttpStatus3xx</span></span> | <span data-ttu-id="6594f-305">Egy 3xx HTTP kódot (pl. 300, 302) eredményezett összes kérelem száma</span><span class="sxs-lookup"><span data-stu-id="6594f-305">Count of all requests that resulted in a 3xx HTTP code (e.g. 300, 302)</span></span>              | <span data-ttu-id="6594f-306">Igen</span><span class="sxs-lookup"><span data-stu-id="6594f-306">Yes</span></span>  |<span data-ttu-id="6594f-307">Igen</span><span class="sxs-lookup"><span data-stu-id="6594f-307">Yes</span></span>   |
| <span data-ttu-id="6594f-308">RequestCountHttpStatus4xx</span><span class="sxs-lookup"><span data-stu-id="6594f-308">RequestCountHttpStatus4xx</span></span> |<span data-ttu-id="6594f-309">Egy 4xx HTTP kódot (pl. 400, 404) eredményezett összes kérelem száma</span><span class="sxs-lookup"><span data-stu-id="6594f-309">Count of all requests that resulted in a 4xx HTTP code (e.g. 400, 404)</span></span>               | <span data-ttu-id="6594f-310">Igen</span><span class="sxs-lookup"><span data-stu-id="6594f-310">Yes</span></span>   |<span data-ttu-id="6594f-311">Igen</span><span class="sxs-lookup"><span data-stu-id="6594f-311">Yes</span></span>   |
| <span data-ttu-id="6594f-312">RequestCountHttpStatus5xx</span><span class="sxs-lookup"><span data-stu-id="6594f-312">RequestCountHttpStatus5xx</span></span> | <span data-ttu-id="6594f-313">5xx HTTP kódot (pl. 500, 504) eredményezett összes kérelem száma</span><span class="sxs-lookup"><span data-stu-id="6594f-313">Count of all requests that resulted in a 5xx HTTP code (e.g. 500, 504)</span></span>              | <span data-ttu-id="6594f-314">Igen</span><span class="sxs-lookup"><span data-stu-id="6594f-314">Yes</span></span>  |<span data-ttu-id="6594f-315">Igen</span><span class="sxs-lookup"><span data-stu-id="6594f-315">Yes</span></span>   |
| <span data-ttu-id="6594f-316">RequestCountHttpStatusOthers</span><span class="sxs-lookup"><span data-stu-id="6594f-316">RequestCountHttpStatusOthers</span></span> |  <span data-ttu-id="6594f-317">Minden más HTTP-kód (kívül 2xx-5xx) száma</span><span class="sxs-lookup"><span data-stu-id="6594f-317">Count of all other HTTP codes (outside of 2xx-5xx)</span></span> | <span data-ttu-id="6594f-318">Igen</span><span class="sxs-lookup"><span data-stu-id="6594f-318">Yes</span></span>  |<span data-ttu-id="6594f-319">Igen</span><span class="sxs-lookup"><span data-stu-id="6594f-319">Yes</span></span>   |
| <span data-ttu-id="6594f-320">RequestCountHttpStatus200</span><span class="sxs-lookup"><span data-stu-id="6594f-320">RequestCountHttpStatus200</span></span> | <span data-ttu-id="6594f-321">200 HTTP kód válaszul eredményező összes kérelmek száma</span><span class="sxs-lookup"><span data-stu-id="6594f-321">Count of all requests that resulted in a 200 HTTP code response</span></span>              |<span data-ttu-id="6594f-322">Nem</span><span class="sxs-lookup"><span data-stu-id="6594f-322">No</span></span>   |<span data-ttu-id="6594f-323">Igen</span><span class="sxs-lookup"><span data-stu-id="6594f-323">Yes</span></span>   |
| <span data-ttu-id="6594f-324">RequestCountHttpStatus206</span><span class="sxs-lookup"><span data-stu-id="6594f-324">RequestCountHttpStatus206</span></span> | <span data-ttu-id="6594f-325">A 206-os HTTP kód válaszul eredményező összes kérelmek száma</span><span class="sxs-lookup"><span data-stu-id="6594f-325">Count of all requests that resulted in a 206 HTTP code response</span></span>              |<span data-ttu-id="6594f-326">Nem</span><span class="sxs-lookup"><span data-stu-id="6594f-326">No</span></span>   |<span data-ttu-id="6594f-327">Igen</span><span class="sxs-lookup"><span data-stu-id="6594f-327">Yes</span></span>   |
| <span data-ttu-id="6594f-328">RequestCountHttpStatus302</span><span class="sxs-lookup"><span data-stu-id="6594f-328">RequestCountHttpStatus302</span></span> | <span data-ttu-id="6594f-329">HTTP 302-es kód választ eredményező összes kérelmek száma</span><span class="sxs-lookup"><span data-stu-id="6594f-329">Count of all requests that resulted in a 302 HTTP code response</span></span>              |<span data-ttu-id="6594f-330">Nem</span><span class="sxs-lookup"><span data-stu-id="6594f-330">No</span></span>   |<span data-ttu-id="6594f-331">Igen</span><span class="sxs-lookup"><span data-stu-id="6594f-331">Yes</span></span>   |
| <span data-ttu-id="6594f-332">RequestCountHttpStatus304</span><span class="sxs-lookup"><span data-stu-id="6594f-332">RequestCountHttpStatus304</span></span> |  <span data-ttu-id="6594f-333">304-es HTTP-kód választ eredményező összes kérelmek száma</span><span class="sxs-lookup"><span data-stu-id="6594f-333">Count of all requests that resulted in a 304 HTTP code response</span></span>             |<span data-ttu-id="6594f-334">Nem</span><span class="sxs-lookup"><span data-stu-id="6594f-334">No</span></span>   |<span data-ttu-id="6594f-335">Igen</span><span class="sxs-lookup"><span data-stu-id="6594f-335">Yes</span></span>   |
| <span data-ttu-id="6594f-336">RequestCountHttpStatus404</span><span class="sxs-lookup"><span data-stu-id="6594f-336">RequestCountHttpStatus404</span></span> | <span data-ttu-id="6594f-337">HTTP 404-es kód választ eredményező összes kérelmek száma</span><span class="sxs-lookup"><span data-stu-id="6594f-337">Count of all requests that resulted in a 404 HTTP code response</span></span>              |<span data-ttu-id="6594f-338">Nem</span><span class="sxs-lookup"><span data-stu-id="6594f-338">No</span></span>   |<span data-ttu-id="6594f-339">Igen</span><span class="sxs-lookup"><span data-stu-id="6594f-339">Yes</span></span>   |
| <span data-ttu-id="6594f-340">RequestCountCacheHit</span><span class="sxs-lookup"><span data-stu-id="6594f-340">RequestCountCacheHit</span></span> |<span data-ttu-id="6594f-341">A gyorsítótár találati eredményező összes kérelmek száma.</span><span class="sxs-lookup"><span data-stu-id="6594f-341">Count of all requests that resulted in a Cache Hit.</span></span> <span data-ttu-id="6594f-342">Ez azt jelenti, hogy az eszköz állítása és kiszolgálása között a POP-ről az ügyfélnek.</span><span class="sxs-lookup"><span data-stu-id="6594f-342">This means the asset was served directly from the POP to the Client.</span></span>               | <span data-ttu-id="6594f-343">Igen</span><span class="sxs-lookup"><span data-stu-id="6594f-343">Yes</span></span>  |<span data-ttu-id="6594f-344">Nem</span><span class="sxs-lookup"><span data-stu-id="6594f-344">No</span></span>   |
| <span data-ttu-id="6594f-345">RequestCountCacheMiss</span><span class="sxs-lookup"><span data-stu-id="6594f-345">RequestCountCacheMiss</span></span> | <span data-ttu-id="6594f-346">A gyorsítótár-tévesztései eredményező összes kérelmek száma.</span><span class="sxs-lookup"><span data-stu-id="6594f-346">Count of all requests that resulted in a Cache Miss.</span></span> <span data-ttu-id="6594f-347">Ez azt jelenti, hogy az eszköz nem található meg az ügyfél legközelebb POP, és ezért be lett olvasva a forrásból.</span><span class="sxs-lookup"><span data-stu-id="6594f-347">This means the asset was not found on the POP closest to the client, and therefore was retrieved from the Origin.</span></span>              |<span data-ttu-id="6594f-348">Igen</span><span class="sxs-lookup"><span data-stu-id="6594f-348">Yes</span></span>   | <span data-ttu-id="6594f-349">Nem</span><span class="sxs-lookup"><span data-stu-id="6594f-349">No</span></span>  |
| <span data-ttu-id="6594f-350">RequestCountCacheNoCache</span><span class="sxs-lookup"><span data-stu-id="6594f-350">RequestCountCacheNoCache</span></span> | <span data-ttu-id="6594f-351">Az eszköz minden kérelemhez, amely megakadályozta a gyorsítótárba egy felhasználói konfiguráció az oldal miatt száma.</span><span class="sxs-lookup"><span data-stu-id="6594f-351">Count of all requests to an asset that are prevented from being cached due to a user configuration on the edge.</span></span>              |<span data-ttu-id="6594f-352">Igen</span><span class="sxs-lookup"><span data-stu-id="6594f-352">Yes</span></span>   | <span data-ttu-id="6594f-353">Nem</span><span class="sxs-lookup"><span data-stu-id="6594f-353">No</span></span>  |
| <span data-ttu-id="6594f-354">RequestCountCacheUncacheable</span><span class="sxs-lookup"><span data-stu-id="6594f-354">RequestCountCacheUncacheable</span></span> | <span data-ttu-id="6594f-355">Az eszközökhöz, amely megakadályozza az eszköz a Cache-Control és Expires fejléc, amely jelzi, hogy azt nem gyorsítótárazza a POP- vagy HTTP-ügyfél által a gyorsítótárba összes kérelmek száma</span><span class="sxs-lookup"><span data-stu-id="6594f-355">Count of all requests to assets that are prevented from being cached by the asset's Cache-Control and Expires headers, which indicate that it should not be cached on a POP or by the HTTP client</span></span>                |<span data-ttu-id="6594f-356">Igen</span><span class="sxs-lookup"><span data-stu-id="6594f-356">Yes</span></span>   |<span data-ttu-id="6594f-357">Nem</span><span class="sxs-lookup"><span data-stu-id="6594f-357">No</span></span>   |
| <span data-ttu-id="6594f-358">RequestCountCacheOthers</span><span class="sxs-lookup"><span data-stu-id="6594f-358">RequestCountCacheOthers</span></span> | <span data-ttu-id="6594f-359">Gyorsítótár állapotú fent nem vonatkozik minden kérelmek száma.</span><span class="sxs-lookup"><span data-stu-id="6594f-359">Count of all requests with cache status not covered by above.</span></span>              |<span data-ttu-id="6594f-360">Igen</span><span class="sxs-lookup"><span data-stu-id="6594f-360">Yes</span></span>   | <span data-ttu-id="6594f-361">Nem</span><span class="sxs-lookup"><span data-stu-id="6594f-361">No</span></span>  |
| <span data-ttu-id="6594f-362">EgressTotal</span><span class="sxs-lookup"><span data-stu-id="6594f-362">EgressTotal</span></span> | <span data-ttu-id="6594f-363">Kimenő adatátvitel GB-ban</span><span class="sxs-lookup"><span data-stu-id="6594f-363">Outbound data transfer in GB</span></span>              |<span data-ttu-id="6594f-364">Igen</span><span class="sxs-lookup"><span data-stu-id="6594f-364">Yes</span></span>   |<span data-ttu-id="6594f-365">Igen</span><span class="sxs-lookup"><span data-stu-id="6594f-365">Yes</span></span>   |
| <span data-ttu-id="6594f-366">EgressHttpStatus2xx</span><span class="sxs-lookup"><span data-stu-id="6594f-366">EgressHttpStatus2xx</span></span> | <span data-ttu-id="6594f-367">Kimenő adatátviteli * a válaszok a 2xx HTTP-állapotkódok GB-ban</span><span class="sxs-lookup"><span data-stu-id="6594f-367">Outbound data transfer* for responses with 2xx HTTP status codes in GB</span></span>            |<span data-ttu-id="6594f-368">Igen</span><span class="sxs-lookup"><span data-stu-id="6594f-368">Yes</span></span>   |<span data-ttu-id="6594f-369">Nem</span><span class="sxs-lookup"><span data-stu-id="6594f-369">No</span></span>   |
| <span data-ttu-id="6594f-370">EgressHttpStatus3xx</span><span class="sxs-lookup"><span data-stu-id="6594f-370">EgressHttpStatus3xx</span></span> | <span data-ttu-id="6594f-371">Kimenő adatátvitel a válaszok a 3xx HTTP-állapotkódok GB-ban</span><span class="sxs-lookup"><span data-stu-id="6594f-371">Outbound data transfer for responses with 3xx HTTP status codes in GB</span></span>              |<span data-ttu-id="6594f-372">Igen</span><span class="sxs-lookup"><span data-stu-id="6594f-372">Yes</span></span>   |<span data-ttu-id="6594f-373">Nem</span><span class="sxs-lookup"><span data-stu-id="6594f-373">No</span></span>   |
| <span data-ttu-id="6594f-374">EgressHttpStatus4xx</span><span class="sxs-lookup"><span data-stu-id="6594f-374">EgressHttpStatus4xx</span></span> | <span data-ttu-id="6594f-375">Kimenő adatátvitel a válaszok a 4xx HTTP-állapotkódok GB-ban</span><span class="sxs-lookup"><span data-stu-id="6594f-375">Outbound data transfer for responses with 4xx HTTP status codes in GB</span></span>               |<span data-ttu-id="6594f-376">Igen</span><span class="sxs-lookup"><span data-stu-id="6594f-376">Yes</span></span>   | <span data-ttu-id="6594f-377">Nem</span><span class="sxs-lookup"><span data-stu-id="6594f-377">No</span></span>  |
| <span data-ttu-id="6594f-378">EgressHttpStatus5xx</span><span class="sxs-lookup"><span data-stu-id="6594f-378">EgressHttpStatus5xx</span></span> | <span data-ttu-id="6594f-379">Kimenő adatátvitel 5xx HTTP-állapotkódok GB-ban a válaszok</span><span class="sxs-lookup"><span data-stu-id="6594f-379">Outbound data transfer for responses with 5xx HTTP status codes in GB</span></span>               |<span data-ttu-id="6594f-380">Igen</span><span class="sxs-lookup"><span data-stu-id="6594f-380">Yes</span></span>   |  <span data-ttu-id="6594f-381">Nem</span><span class="sxs-lookup"><span data-stu-id="6594f-381">No</span></span> |
| <span data-ttu-id="6594f-382">EgressHttpStatusOthers</span><span class="sxs-lookup"><span data-stu-id="6594f-382">EgressHttpStatusOthers</span></span> | <span data-ttu-id="6594f-383">Kimenő adatátvitel válaszok az egyéb HTTP-állapotkódok GB-ban</span><span class="sxs-lookup"><span data-stu-id="6594f-383">Outbound data transfer for responses with other HTTP status codes in GB</span></span>                |<span data-ttu-id="6594f-384">Igen</span><span class="sxs-lookup"><span data-stu-id="6594f-384">Yes</span></span>   |<span data-ttu-id="6594f-385">Nem</span><span class="sxs-lookup"><span data-stu-id="6594f-385">No</span></span>   |
| <span data-ttu-id="6594f-386">EgressCacheHit</span><span class="sxs-lookup"><span data-stu-id="6594f-386">EgressCacheHit</span></span> |  <span data-ttu-id="6594f-387">Kimenő adatátvitel kapott válaszok közvetlenül a CDN-gyorsítótárból a CDN POP/szegély</span><span class="sxs-lookup"><span data-stu-id="6594f-387">Outbound data transfer for responses that were delivered directly from the CDN cache on the CDN POPs/Edges</span></span>  |<span data-ttu-id="6594f-388">Igen</span><span class="sxs-lookup"><span data-stu-id="6594f-388">Yes</span></span>   |  <span data-ttu-id="6594f-389">Nem</span><span class="sxs-lookup"><span data-stu-id="6594f-389">No</span></span> |
| <span data-ttu-id="6594f-390">EgressCacheMiss</span><span class="sxs-lookup"><span data-stu-id="6594f-390">EgressCacheMiss</span></span> | <span data-ttu-id="6594f-391">Kimenő adatátvitel a válaszok nem található a legközelebbi POP-kiszolgálón, és lekérése a forráskiszolgálóról</span><span class="sxs-lookup"><span data-stu-id="6594f-391">Outbound data transfer for responses that were not found on the nearest POP server, and retrieved from the origin server</span></span>              |<span data-ttu-id="6594f-392">Igen</span><span class="sxs-lookup"><span data-stu-id="6594f-392">Yes</span></span>   |  <span data-ttu-id="6594f-393">Nem</span><span class="sxs-lookup"><span data-stu-id="6594f-393">No</span></span> |
| <span data-ttu-id="6594f-394">EgressCacheNoCache</span><span class="sxs-lookup"><span data-stu-id="6594f-394">EgressCacheNoCache</span></span> | <span data-ttu-id="6594f-395">Kimenő adatátvitel eszközök, amely megakadályozta a felhasználói konfiguráció az oldal miatt a gyorsítótárba.</span><span class="sxs-lookup"><span data-stu-id="6594f-395">Outbound data transfer for assets that are prevented from being cached due to a user configuration on the edge.</span></span>                |<span data-ttu-id="6594f-396">Igen</span><span class="sxs-lookup"><span data-stu-id="6594f-396">Yes</span></span>   |<span data-ttu-id="6594f-397">Nem</span><span class="sxs-lookup"><span data-stu-id="6594f-397">No</span></span>   |
| <span data-ttu-id="6594f-398">EgressCacheUncacheable</span><span class="sxs-lookup"><span data-stu-id="6594f-398">EgressCacheUncacheable</span></span> | <span data-ttu-id="6594f-399">Kimenő adatátvitel eszközök, amelyek a rendszer megakadályozza az eszköz Cache-Control vagy Expires fejléc, amely jelzi, hogy azt nem gyorsítótárazza a POP vagy a HTTP-ügyfél által a gyorsítótárba</span><span class="sxs-lookup"><span data-stu-id="6594f-399">Outbound data transfer for assets that are prevented from being cached by the asset's Cache-Control and/or Expires headers, which indicate that it should not be cached on a POP or by the HTTP client</span></span>                    |<span data-ttu-id="6594f-400">Igen</span><span class="sxs-lookup"><span data-stu-id="6594f-400">Yes</span></span>   | <span data-ttu-id="6594f-401">Nem</span><span class="sxs-lookup"><span data-stu-id="6594f-401">No</span></span>  |
| <span data-ttu-id="6594f-402">EgressCacheOthers</span><span class="sxs-lookup"><span data-stu-id="6594f-402">EgressCacheOthers</span></span> |  <span data-ttu-id="6594f-403">Kimenő adatátvitel más gyorsítótár forgatókönyvek esetén.</span><span class="sxs-lookup"><span data-stu-id="6594f-403">Outbound data transfers for other cache scenarios.</span></span>             |<span data-ttu-id="6594f-404">Igen</span><span class="sxs-lookup"><span data-stu-id="6594f-404">Yes</span></span>   | <span data-ttu-id="6594f-405">Nem</span><span class="sxs-lookup"><span data-stu-id="6594f-405">No</span></span>  |

<span data-ttu-id="6594f-406">* Kimenő forgalom CDN POP-ra kiszolgálókról kézbesítve lenne az ügyfél hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="6594f-406">*Outbound data transfer refers to traffic delivered from CDN POP servers to the client.</span></span>


### <a name="schema-of-the-core-analytics-logs"></a><span data-ttu-id="6594f-407">A Core Analytics naplók séma</span><span class="sxs-lookup"><span data-stu-id="6594f-407">Schema of the Core Analytics Logs</span></span> 

<span data-ttu-id="6594f-408">Összes napló JSON formátumban vannak tárolva, és mindegyik bejegyzés rendelkezik a következő karakterlánc mezők a séma alatt:</span><span class="sxs-lookup"><span data-stu-id="6594f-408">All logs are stored in JSON format and each entry has string fields following the below schema:</span></span>

```json
    "records": [
        {
            "time": "2017-04-27T01:00:00",
            "resourceId": "<ARM Resource Id of the CDN Endpoint>",
            "operationName": "Microsoft.Cdn/profiles/endpoints/contentDelivery",
            "category": "CoreAnalytics",
            "properties": {
                "DomainName": "<Name of the domain for which the statistics is reported>",
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

<span data-ttu-id="6594f-409">Ahol a "time" jelenti az óra határ, amelynek a statisztikáit jelentett kezdési idejét.</span><span class="sxs-lookup"><span data-stu-id="6594f-409">Where the ‘time’ represents the start time of the hour boundary for which the statistics is reported.</span></span> <span data-ttu-id="6594f-410">Ha egy metrika nem támogatott a CDN-szolgáltató helyett egy double vagy egész szám, null értékű lesz.</span><span class="sxs-lookup"><span data-stu-id="6594f-410">When a metric is not supported by a CDN provider, instead of a double or integer value, there will be a null value.</span></span> <span data-ttu-id="6594f-411">A null érték jelezné metrika, és ez nem azonos a 0 értéket.</span><span class="sxs-lookup"><span data-stu-id="6594f-411">This null value indicates the absence of a metric, and this is different from a 0 value.</span></span> <span data-ttu-id="6594f-412">Ne feledje, hogy az a metrikák a végponthoz tartományonként egy készletét lesz.</span><span class="sxs-lookup"><span data-stu-id="6594f-412">Also note that there will be one set of these metrics per domain configured on the endpoint.</span></span>

<span data-ttu-id="6594f-413">Az alábbi példa tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="6594f-413">Example properties below:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="6594f-414">További források</span><span class="sxs-lookup"><span data-stu-id="6594f-414">Additional resources</span></span>

* [<span data-ttu-id="6594f-415">Az Azure diagnosztikai naplók</span><span class="sxs-lookup"><span data-stu-id="6594f-415">Azure Diagnostic logs</span></span>](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)
* [<span data-ttu-id="6594f-416">Egyszerűsített analitika Azure CDN kiegészítő portálon keresztül</span><span class="sxs-lookup"><span data-stu-id="6594f-416">Core analytics via Azure CDN supplemental portal</span></span>](https://docs.microsoft.com/azure/cdn/cdn-analyze-usage-patterns)
* [<span data-ttu-id="6594f-417">Az Azure OMS szolgáltatáshoz</span><span class="sxs-lookup"><span data-stu-id="6594f-417">Azure OMS Log Analytics</span></span>](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-overview)
* [<span data-ttu-id="6594f-418">Az Azure Naplóelemzés REST API-n</span><span class="sxs-lookup"><span data-stu-id="6594f-418">Azure Log Analytics REST API</span></span>](https://docs.microsoft.com/en-us/rest/api/loganalytics)







