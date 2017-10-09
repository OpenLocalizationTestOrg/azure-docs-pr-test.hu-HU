---
title: "aaaViewing diagnosztikai naplók az Azure Data Lake Analytics |} Microsoft Docs"
description: "Hogyan toosetup és hozzáférési diagnosztikai naplók az Azure Data Lake analytics ismertetése "
services: data-lake-analytics
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: cf5633d4-bc43-444e-90fc-f90fbd0b7935
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: 4cd1eb6f585c1ef96c358340232ef85721a972b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-diagnostic-logs-for-azure-data-lake-analytics"></a><span data-ttu-id="82c4d-103">Diagnosztikai naplók elérése az Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="82c4d-103">Accessing diagnostic logs for Azure Data Lake Analytics</span></span>

<span data-ttu-id="82c4d-104">Diagnosztikai naplózás lehetővé teszi a toocollect adatok a fájlhozzáférés napló ellenőrzését.</span><span class="sxs-lookup"><span data-stu-id="82c4d-104">Diagnostic logging allows you toocollect data access audit trails.</span></span> <span data-ttu-id="82c4d-105">Ezek a naplók meg információkat, mint:</span><span class="sxs-lookup"><span data-stu-id="82c4d-105">These logs provide information such as:</span></span>

* <span data-ttu-id="82c4d-106">Azoknak a felhasználóknak, hello adatok érhetők el.</span><span class="sxs-lookup"><span data-stu-id="82c4d-106">A list of users that accessed hello data.</span></span>
* <span data-ttu-id="82c4d-107">Milyen gyakran hello adatokhoz.</span><span class="sxs-lookup"><span data-stu-id="82c4d-107">How frequently hello data is accessed.</span></span>
* <span data-ttu-id="82c4d-108">Mennyi adatot hello fiók tárolva van.</span><span class="sxs-lookup"><span data-stu-id="82c4d-108">How much data is stored in hello account.</span></span>

## <a name="enable-logging"></a><span data-ttu-id="82c4d-109">Naplózás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="82c4d-109">Enable logging</span></span>

1. <span data-ttu-id="82c4d-110">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="82c4d-110">Sign on toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="82c4d-111">Nyissa meg a Data Lake Analytics-fiókja, és válassza ki **diagnosztikai naplók** a hello __figyelő__ szakasz.</span><span class="sxs-lookup"><span data-stu-id="82c4d-111">Open your Data Lake Analytics account and select **Diagnostic logs** from hello __Monitor__ section.</span></span> <span data-ttu-id="82c4d-112">Válassza ki, __a diagnosztika bekapcsolásához__.</span><span class="sxs-lookup"><span data-stu-id="82c4d-112">Next, select __Turn on diagnostics__.</span></span>

    ![Diagnosztika toocollect naplózás bekapcsolásához és a naplók kérése](./media/data-lake-analytics-diagnostic-logs/turn-on-logging.png)

3. <span data-ttu-id="82c4d-114">A __diagnosztikai beállítások__, hello állapot too__On__ beállítva, és válassza ki a naplózási beállításokat.</span><span class="sxs-lookup"><span data-stu-id="82c4d-114">From __Diagnostics settings__, set hello status too__On__ and select logging options.</span></span>

    <span data-ttu-id="82c4d-115">![Diagnosztika toocollect naplózás bekapcsolásához és a naplók kérése](./media/data-lake-analytics-diagnostic-logs/enable-diagnostic-logs.png "diagnosztikai naplók engedélyezése")</span><span class="sxs-lookup"><span data-stu-id="82c4d-115">![Turn on diagnostics toocollect audit and request logs](./media/data-lake-analytics-diagnostic-logs/enable-diagnostic-logs.png "Enable diagnostic logs")</span></span>

   * <span data-ttu-id="82c4d-116">Állítsa be **állapot** túl**a** tooenable diagnosztikai naplózás.</span><span class="sxs-lookup"><span data-stu-id="82c4d-116">Set **Status** too**On** tooenable diagnostic logging.</span></span>

   * <span data-ttu-id="82c4d-117">Kiválaszthatja a toostore/folyamat hello adatok három különböző módon.</span><span class="sxs-lookup"><span data-stu-id="82c4d-117">You can choose toostore/process hello data in three different ways.</span></span>

     * <span data-ttu-id="82c4d-118">Válassza ki __tooa tárfiók archiválására__ toostore jelentkezik be egy Azure storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="82c4d-118">Select __Archive tooa storage account__ toostore logs in an Azure storage account.</span></span> <span data-ttu-id="82c4d-119">Használja ezt a beállítást, ha azt szeretné, hogy tooarchive hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="82c4d-119">Use this option if you want tooarchive hello data.</span></span> <span data-ttu-id="82c4d-120">Ha ezt a lehetőséget választja, meg kell adnia egy Azure storage-toosave hello naplók a.</span><span class="sxs-lookup"><span data-stu-id="82c4d-120">If you select this option, you must provide an Azure storage account toosave hello logs to.</span></span>

     * <span data-ttu-id="82c4d-121">Válassza ki **tooan Eseményközpont adatfolyam** toostream napló adatok tooan Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="82c4d-121">Select **Stream tooan Event Hub** toostream log data tooan Azure Event Hub.</span></span> <span data-ttu-id="82c4d-122">Használja ezt a beállítást, ha egy alárendelt feldolgozási folyamatot, amely elemzi a bejövő naplók valós időben.</span><span class="sxs-lookup"><span data-stu-id="82c4d-122">Use this option if you have a downstream processing pipeline that is analyzing incoming logs in real time.</span></span> <span data-ttu-id="82c4d-123">Ha ezt a lehetőséget választja, meg kell adnia hello Azure Event Hubs toouse kívánt hello részletei.</span><span class="sxs-lookup"><span data-stu-id="82c4d-123">If you select this option, you must provide hello details for hello Azure Event Hub you want toouse.</span></span>

     * <span data-ttu-id="82c4d-124">Válassza ki __tooLog Analytics küldése__ toosend hello adatok toohello Naplóelemzés szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="82c4d-124">Select __Send tooLog Analytics__ toosend hello data toohello Log Analytics service.</span></span> <span data-ttu-id="82c4d-125">Használja ezt a beállítást, ha szeretné, hogy toouse Naplóelemzési toogather és naplóinak elemzése.</span><span class="sxs-lookup"><span data-stu-id="82c4d-125">Use this option if you want toouse Log Analytics toogather and analyze logs.</span></span>
   * <span data-ttu-id="82c4d-126">Adja meg, hogy tooget naplókat, a kérelmek naplóit vagy mindkettőhöz.</span><span class="sxs-lookup"><span data-stu-id="82c4d-126">Specify whether you want tooget audit logs or request logs or both.</span></span>  <span data-ttu-id="82c4d-127">A napló minden API-kérelem rögzíti.</span><span class="sxs-lookup"><span data-stu-id="82c4d-127">A request log captures every API request.</span></span> <span data-ttu-id="82c4d-128">Egy naplórekordok az adott API-kérelem által kezdeményezett összes műveletet.</span><span class="sxs-lookup"><span data-stu-id="82c4d-128">An audit log records all operations that are triggered by that API request.</span></span>

   * <span data-ttu-id="82c4d-129">A __tooa tárfiók archiválására__, adja meg a napok tooretain hello adatok hello száma.</span><span class="sxs-lookup"><span data-stu-id="82c4d-129">For __Archive tooa storage account__, specify hello number of days tooretain hello data.</span></span>

   * <span data-ttu-id="82c4d-130">Kattintson a __Save__ (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="82c4d-130">Click __Save__.</span></span>

        > [!NOTE]
        > <span data-ttu-id="82c4d-131">Ki kell választania, vagy __tooa tárfiók archiválására__, __tooan Eseményközpont adatfolyam__ vagy __tooLog Analytics küldése__ hello való kattintás előtt __mentése__gombra.</span><span class="sxs-lookup"><span data-stu-id="82c4d-131">You must select either __Archive tooa storage account__, __Stream tooan Event Hub__ or __Send tooLog Analytics__ before clicking hello __Save__ button.</span></span>

<span data-ttu-id="82c4d-132">Miután engedélyezte a diagnosztikai beállítások, bármikor visszatérhet toohello __diagnosztikai naplók__ panel tooview hello naplókat.</span><span class="sxs-lookup"><span data-stu-id="82c4d-132">Once you have enabled diagnostic settings, you can return toohello __Diagnostics logs__ blade tooview hello logs.</span></span>

## <a name="view-logs"></a><span data-ttu-id="82c4d-133">Naplók megtekintése</span><span class="sxs-lookup"><span data-stu-id="82c4d-133">View logs</span></span>

### <a name="use-hello-data-lake-analytics-view"></a><span data-ttu-id="82c4d-134">Hello Data Lake Analytics nézetben</span><span class="sxs-lookup"><span data-stu-id="82c4d-134">Use hello Data Lake Analytics view</span></span>

1. <span data-ttu-id="82c4d-135">A Data Lake Analytics a fiók panelen a **figyelés**, jelölje be **diagnosztikai naplók** és a jelölje ki egy bejegyzést toodisplay naplókat.</span><span class="sxs-lookup"><span data-stu-id="82c4d-135">From your Data Lake Analytics account blade, under **Monitoring**, select **Diagnostic Logs** and then select an entry toodisplay logs for.</span></span>

    <span data-ttu-id="82c4d-136">![Diagnosztikai naplózás nézet](./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs.png "diagnosztikai naplók megtekintése")</span><span class="sxs-lookup"><span data-stu-id="82c4d-136">![View diagnostic logging](./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs.png "View diagnostic logs")</span></span>

2. <span data-ttu-id="82c4d-137">hello naplók szerint vannak kategóriába által **naplók** és **kérelem naplók**.</span><span class="sxs-lookup"><span data-stu-id="82c4d-137">hello logs are categorized by **Audit Logs** and **Request Logs**.</span></span>

    ![naplóbejegyzéseket](./media/data-lake-analytics-diagnostic-logs/diagnostic-log-entries.png)

   * <span data-ttu-id="82c4d-139">Kérelem naplók rögzítése a Data Lake Analytics-fiók hello minden API kérelmet.</span><span class="sxs-lookup"><span data-stu-id="82c4d-139">Request logs capture every API request made on hello Data Lake Analytics account.</span></span>
   * <span data-ttu-id="82c4d-140">Naplók hasonló toorequest naplókat azonban hello műveletek sokkal részletesebb információkat biztosít.</span><span class="sxs-lookup"><span data-stu-id="82c4d-140">Audit Logs are similar toorequest Logs but provide a much more detailed breakdown of hello operations.</span></span> <span data-ttu-id="82c4d-141">Például egy kérelem naplóban egyetlen feltöltés API hívása több "Append" műveletet a naplóban eredményezhet.</span><span class="sxs-lookup"><span data-stu-id="82c4d-141">For example, a single upload API call in a request log can result in multiple "Append" operations in its audit log.</span></span>

3. <span data-ttu-id="82c4d-142">Kattintson a hello **letöltése** a napló bejegyzés toodownload, hogy a hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="82c4d-142">Click hello **Download** link for a log entry toodownload that log.</span></span>

### <a name="use-hello-azure-storage-account-that-contains-log-data"></a><span data-ttu-id="82c4d-143">Naplóadatokat tartalmazó hello Azure Storage-fiók használata</span><span class="sxs-lookup"><span data-stu-id="82c4d-143">Use hello Azure Storage account that contains log data</span></span>

1. <span data-ttu-id="82c4d-144">Nyissa meg a Data Lake Analytics társított naplózás hello Azure Storage-fiók panelen, és kattintson a __Blobok__.</span><span class="sxs-lookup"><span data-stu-id="82c4d-144">Open hello Azure Storage account blade associated with Data Lake Analytics for logging, and then click __Blobs__.</span></span> <span data-ttu-id="82c4d-145">Hello **Blob szolgáltatás** panel két tárolók listája.</span><span class="sxs-lookup"><span data-stu-id="82c4d-145">hello **Blob service** blade lists two containers.</span></span>

    <span data-ttu-id="82c4d-146">![Diagnosztikai naplózás nézet](./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs-storage-account.png "diagnosztikai naplók megtekintése")</span><span class="sxs-lookup"><span data-stu-id="82c4d-146">![View diagnostic logging](./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs-storage-account.png "View diagnostic logs")</span></span>

   * <span data-ttu-id="82c4d-147">hello tároló **insights-logs-naplózási** hello naplók tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="82c4d-147">hello container **insights-logs-audit** contains hello audit logs.</span></span>
   * <span data-ttu-id="82c4d-148">hello tároló **insights-logs-kérelmek** hello kérelmek naplóit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="82c4d-148">hello container **insights-logs-requests** contains hello request logs.</span></span>
2. <span data-ttu-id="82c4d-149">Ezek a tárolók belül hello naplók tárolt hello struktúra a következő:</span><span class="sxs-lookup"><span data-stu-id="82c4d-149">Within these containers, hello logs are stored under hello following structure:</span></span>

        resourceId=/
          SUBSCRIPTIONS/
            <<SUBSCRIPTION_ID>>/
              RESOURCEGROUPS/
                <<RESOURCE_GRP_NAME>>/
                  PROVIDERS/
                    MICROSOFT.DATALAKEANALYTICS/
                      ACCOUNTS/
                        <DATA_LAKE_ANALYTICS_NAME>>/
                          y=####/
                            m=##/
                              d=##/
                                h=##/
                                  m=00/
                                    PT1H.json

   > [!NOTE]
   > <span data-ttu-id="82c4d-150">Hello `##` hello elérési bejegyzés tartalmazhat hello év, hónap, nap és mely hello napló létrehozta óra.</span><span class="sxs-lookup"><span data-stu-id="82c4d-150">hello `##` entries in hello path contain hello year, month, day, and hour in which hello log was created.</span></span> <span data-ttu-id="82c4d-151">A Data Lake Analytics egy fájl minden órában létrehoz, így `m=` mindig szereplő érték `00`.</span><span class="sxs-lookup"><span data-stu-id="82c4d-151">Data Lake Analytics creates one file every hour, so `m=` always contains a value of `00`.</span></span>

    <span data-ttu-id="82c4d-152">Tegyük fel hello teljes elérési útja tooan napló lehet:</span><span class="sxs-lookup"><span data-stu-id="82c4d-152">As an example, hello complete path tooan audit log could be:</span></span>

        https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=04/m=00/PT1H.json

    <span data-ttu-id="82c4d-153">Hasonlóképpen teljes elérési útja tooa napló hello lehet:</span><span class="sxs-lookup"><span data-stu-id="82c4d-153">Similarly, hello complete path tooa request log could be:</span></span>

        https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=14/m=00/PT1H.json

## <a name="log-structure"></a><span data-ttu-id="82c4d-154">Napló struktúra</span><span class="sxs-lookup"><span data-stu-id="82c4d-154">Log structure</span></span>

<span data-ttu-id="82c4d-155">hello naplózási és kérelem feldolgozásra strukturált JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="82c4d-155">hello audit and request logs are in a structured JSON format.</span></span>

### <a name="request-logs"></a><span data-ttu-id="82c4d-156">Naplók kérése</span><span class="sxs-lookup"><span data-stu-id="82c4d-156">Request logs</span></span>

<span data-ttu-id="82c4d-157">Íme egy minta bejegyzés hello JSON-formátumú kérelem naplóban.</span><span class="sxs-lookup"><span data-stu-id="82c4d-157">Here's a sample entry in hello JSON-formatted request log.</span></span> <span data-ttu-id="82c4d-158">Minden egyes blob tartozik egy legfelső szintű objektum nevű **rekordok** , amely tartalmazza a napló objektumokból álló tömb.</span><span class="sxs-lookup"><span data-stu-id="82c4d-158">Each blob has one root object called **records** that contains an array of log objects.</span></span>

    {
    "records":
      [        
        . . . .
        ,
        {
             "time": "2016-07-07T21:02:53.456Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_analytics_account_name>",
             "category": "Requests",
             "operationName": "GetAggregatedJobHistory",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {
                 "HttpMethod":"POST",
                 "Path":"/JobAggregatedHistory",
                 "RequestContentLength":122,
                 "ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8",
                 "StartTime":"2016-07-07T21:02:52.472Z",
                 "EndTime":"2016-07-07T21:02:53.456Z"
                 }
        }
        ,
        . . . .
      ]
    }

#### <a name="request-log-schema"></a><span data-ttu-id="82c4d-159">Kérelem séma</span><span class="sxs-lookup"><span data-stu-id="82c4d-159">Request log schema</span></span>

| <span data-ttu-id="82c4d-160">Név</span><span class="sxs-lookup"><span data-stu-id="82c4d-160">Name</span></span> | <span data-ttu-id="82c4d-161">Típus</span><span class="sxs-lookup"><span data-stu-id="82c4d-161">Type</span></span> | <span data-ttu-id="82c4d-162">Leírás</span><span class="sxs-lookup"><span data-stu-id="82c4d-162">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="82c4d-163">time</span><span class="sxs-lookup"><span data-stu-id="82c4d-163">time</span></span> |<span data-ttu-id="82c4d-164">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="82c4d-164">String</span></span> |<span data-ttu-id="82c4d-165">hello időbélyegzőnek (UTC) hello napló</span><span class="sxs-lookup"><span data-stu-id="82c4d-165">hello timestamp (in UTC) of hello log</span></span> |
| <span data-ttu-id="82c4d-166">resourceId</span><span class="sxs-lookup"><span data-stu-id="82c4d-166">resourceId</span></span> |<span data-ttu-id="82c4d-167">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="82c4d-167">String</span></span> |<span data-ttu-id="82c4d-168">Helyezze művelet hello erőforrás hello azonosítója</span><span class="sxs-lookup"><span data-stu-id="82c4d-168">hello identifier of hello resource that operation took place on</span></span> |
| <span data-ttu-id="82c4d-169">category</span><span class="sxs-lookup"><span data-stu-id="82c4d-169">category</span></span> |<span data-ttu-id="82c4d-170">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="82c4d-170">String</span></span> |<span data-ttu-id="82c4d-171">hello napló kategóriát.</span><span class="sxs-lookup"><span data-stu-id="82c4d-171">hello log category.</span></span> <span data-ttu-id="82c4d-172">Például **kérelmek**.</span><span class="sxs-lookup"><span data-stu-id="82c4d-172">For example, **Requests**.</span></span> |
| <span data-ttu-id="82c4d-173">operationName</span><span class="sxs-lookup"><span data-stu-id="82c4d-173">operationName</span></span> |<span data-ttu-id="82c4d-174">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="82c4d-174">String</span></span> |<span data-ttu-id="82c4d-175">Bejelentkezve hello művelet neve.</span><span class="sxs-lookup"><span data-stu-id="82c4d-175">Name of hello operation that is logged.</span></span> <span data-ttu-id="82c4d-176">Például GetAggregatedJobHistory.</span><span class="sxs-lookup"><span data-stu-id="82c4d-176">For example, GetAggregatedJobHistory.</span></span> |
| <span data-ttu-id="82c4d-177">resultType</span><span class="sxs-lookup"><span data-stu-id="82c4d-177">resultType</span></span> |<span data-ttu-id="82c4d-178">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="82c4d-178">String</span></span> |<span data-ttu-id="82c4d-179">hello művelet, például 200 hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="82c4d-179">hello status of hello operation, For example, 200.</span></span> |
| <span data-ttu-id="82c4d-180">callerIpAddress</span><span class="sxs-lookup"><span data-stu-id="82c4d-180">callerIpAddress</span></span> |<span data-ttu-id="82c4d-181">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="82c4d-181">String</span></span> |<span data-ttu-id="82c4d-182">hello kérés hello ügyfél hello IP-címe</span><span class="sxs-lookup"><span data-stu-id="82c4d-182">hello IP address of hello client making hello request</span></span> |
| <span data-ttu-id="82c4d-183">correlationId</span><span class="sxs-lookup"><span data-stu-id="82c4d-183">correlationId</span></span> |<span data-ttu-id="82c4d-184">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="82c4d-184">String</span></span> |<span data-ttu-id="82c4d-185">hello hello napló azonosítója.</span><span class="sxs-lookup"><span data-stu-id="82c4d-185">hello identifier of hello log.</span></span> <span data-ttu-id="82c4d-186">Az érték lehet használt toogroup kapcsolódó naplóbejegyzések készlete.</span><span class="sxs-lookup"><span data-stu-id="82c4d-186">This value can be used toogroup a set of related log entries.</span></span> |
| <span data-ttu-id="82c4d-187">identity</span><span class="sxs-lookup"><span data-stu-id="82c4d-187">identity</span></span> |<span data-ttu-id="82c4d-188">Objektum</span><span class="sxs-lookup"><span data-stu-id="82c4d-188">Object</span></span> |<span data-ttu-id="82c4d-189">hello napló okozó hello identitás</span><span class="sxs-lookup"><span data-stu-id="82c4d-189">hello identity that generated hello log</span></span> |
| <span data-ttu-id="82c4d-190">properties</span><span class="sxs-lookup"><span data-stu-id="82c4d-190">properties</span></span> |<span data-ttu-id="82c4d-191">JSON</span><span class="sxs-lookup"><span data-stu-id="82c4d-191">JSON</span></span> |<span data-ttu-id="82c4d-192">Lásd az hello következő szakaszát (kérelem tulajdonságok séma)</span><span class="sxs-lookup"><span data-stu-id="82c4d-192">See hello next section (Request log properties schema) for details</span></span> |

#### <a name="request-log-properties-schema"></a><span data-ttu-id="82c4d-193">Kérelem tulajdonságok séma</span><span class="sxs-lookup"><span data-stu-id="82c4d-193">Request log properties schema</span></span>

| <span data-ttu-id="82c4d-194">Név</span><span class="sxs-lookup"><span data-stu-id="82c4d-194">Name</span></span> | <span data-ttu-id="82c4d-195">Típus</span><span class="sxs-lookup"><span data-stu-id="82c4d-195">Type</span></span> | <span data-ttu-id="82c4d-196">Leírás</span><span class="sxs-lookup"><span data-stu-id="82c4d-196">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="82c4d-197">HttpMethod</span><span class="sxs-lookup"><span data-stu-id="82c4d-197">HttpMethod</span></span> |<span data-ttu-id="82c4d-198">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="82c4d-198">String</span></span> |<span data-ttu-id="82c4d-199">hello HTTP-metódus használt hello a művelethez.</span><span class="sxs-lookup"><span data-stu-id="82c4d-199">hello HTTP Method used for hello operation.</span></span> <span data-ttu-id="82c4d-200">Például beolvasása.</span><span class="sxs-lookup"><span data-stu-id="82c4d-200">For example, GET.</span></span> |
| <span data-ttu-id="82c4d-201">Elérési út</span><span class="sxs-lookup"><span data-stu-id="82c4d-201">Path</span></span> |<span data-ttu-id="82c4d-202">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="82c4d-202">String</span></span> |<span data-ttu-id="82c4d-203">hello elérési hello művelet végrehajtásának ideje</span><span class="sxs-lookup"><span data-stu-id="82c4d-203">hello path hello operation was performed on</span></span> |
| <span data-ttu-id="82c4d-204">RequestContentLength</span><span class="sxs-lookup"><span data-stu-id="82c4d-204">RequestContentLength</span></span> |<span data-ttu-id="82c4d-205">int</span><span class="sxs-lookup"><span data-stu-id="82c4d-205">int</span></span> |<span data-ttu-id="82c4d-206">hello tartalom hossza hello HTTP-kérelem</span><span class="sxs-lookup"><span data-stu-id="82c4d-206">hello content length of hello HTTP request</span></span> |
| <span data-ttu-id="82c4d-207">clientRequestId</span><span class="sxs-lookup"><span data-stu-id="82c4d-207">ClientRequestId</span></span> |<span data-ttu-id="82c4d-208">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="82c4d-208">String</span></span> |<span data-ttu-id="82c4d-209">hello azonosítója, amely egyedileg azonosítja az ehhez a kérelemhez</span><span class="sxs-lookup"><span data-stu-id="82c4d-209">hello identifier that uniquely identifies this request</span></span> |
| <span data-ttu-id="82c4d-210">Kezdő időpont</span><span class="sxs-lookup"><span data-stu-id="82c4d-210">StartTime</span></span> |<span data-ttu-id="82c4d-211">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="82c4d-211">String</span></span> |<span data-ttu-id="82c4d-212">mely hello kiszolgálótól kapott hello kérésére hello idő</span><span class="sxs-lookup"><span data-stu-id="82c4d-212">hello time at which hello server received hello request</span></span> |
| <span data-ttu-id="82c4d-213">Befejezés időpontja</span><span class="sxs-lookup"><span data-stu-id="82c4d-213">EndTime</span></span> |<span data-ttu-id="82c4d-214">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="82c4d-214">String</span></span> |<span data-ttu-id="82c4d-215">mely hello a kiszolgáló által küldött választ hello idő</span><span class="sxs-lookup"><span data-stu-id="82c4d-215">hello time at which hello server sent a response</span></span> |

### <a name="audit-logs"></a><span data-ttu-id="82c4d-216">Naplók</span><span class="sxs-lookup"><span data-stu-id="82c4d-216">Audit logs</span></span>

<span data-ttu-id="82c4d-217">Íme egy minta bejegyzés hello JSON-formátumú naplóban.</span><span class="sxs-lookup"><span data-stu-id="82c4d-217">Here's a sample entry in hello JSON-formatted audit log.</span></span> <span data-ttu-id="82c4d-218">Minden egyes blob tartozik egy legfelső szintű objektum nevű **rekordok** , amely tartalmazza a napló objektumokból álló tömb.</span><span class="sxs-lookup"><span data-stu-id="82c4d-218">Each blob has one root object called **records** that contains an array of log objects.</span></span>

    {
    "records":
      [        
        . . . .
        ,
        {
             "time": "2016-07-28T19:15:16.245Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_ANALYTICS_account_name>",
             "category": "Audit",
             "operationName": "JobSubmitted",
             "identity": "user@somewhere.com",
             "properties": {
                 "JobId":"D74B928F-5194-4E6C-971F-C27026C290E6",
                 "JobName": "New Job",
                 "JobRuntimeName": "default",
                 "SubmitTime": "7/28/2016 7:14:57 PM"
                 }
        }
        ,
        . . . .
      ]
    }

#### <a name="audit-log-schema"></a><span data-ttu-id="82c4d-219">Naplózási séma</span><span class="sxs-lookup"><span data-stu-id="82c4d-219">Audit log schema</span></span>

| <span data-ttu-id="82c4d-220">Név</span><span class="sxs-lookup"><span data-stu-id="82c4d-220">Name</span></span> | <span data-ttu-id="82c4d-221">Típus</span><span class="sxs-lookup"><span data-stu-id="82c4d-221">Type</span></span> | <span data-ttu-id="82c4d-222">Leírás</span><span class="sxs-lookup"><span data-stu-id="82c4d-222">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="82c4d-223">time</span><span class="sxs-lookup"><span data-stu-id="82c4d-223">time</span></span> |<span data-ttu-id="82c4d-224">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="82c4d-224">String</span></span> |<span data-ttu-id="82c4d-225">hello időbélyegzőnek (UTC) hello napló</span><span class="sxs-lookup"><span data-stu-id="82c4d-225">hello timestamp (in UTC) of hello log</span></span> |
| <span data-ttu-id="82c4d-226">resourceId</span><span class="sxs-lookup"><span data-stu-id="82c4d-226">resourceId</span></span> |<span data-ttu-id="82c4d-227">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="82c4d-227">String</span></span> |<span data-ttu-id="82c4d-228">Helyezze művelet hello erőforrás hello azonosítója</span><span class="sxs-lookup"><span data-stu-id="82c4d-228">hello identifier of hello resource that operation took place on</span></span> |
| <span data-ttu-id="82c4d-229">category</span><span class="sxs-lookup"><span data-stu-id="82c4d-229">category</span></span> |<span data-ttu-id="82c4d-230">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="82c4d-230">String</span></span> |<span data-ttu-id="82c4d-231">hello napló kategóriát.</span><span class="sxs-lookup"><span data-stu-id="82c4d-231">hello log category.</span></span> <span data-ttu-id="82c4d-232">Például **naplózási**.</span><span class="sxs-lookup"><span data-stu-id="82c4d-232">For example, **Audit**.</span></span> |
| <span data-ttu-id="82c4d-233">operationName</span><span class="sxs-lookup"><span data-stu-id="82c4d-233">operationName</span></span> |<span data-ttu-id="82c4d-234">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="82c4d-234">String</span></span> |<span data-ttu-id="82c4d-235">Bejelentkezve hello művelet neve.</span><span class="sxs-lookup"><span data-stu-id="82c4d-235">Name of hello operation that is logged.</span></span> <span data-ttu-id="82c4d-236">Például JobSubmitted.</span><span class="sxs-lookup"><span data-stu-id="82c4d-236">For example, JobSubmitted.</span></span> |
| <span data-ttu-id="82c4d-237">resultType</span><span class="sxs-lookup"><span data-stu-id="82c4d-237">resultType</span></span> |<span data-ttu-id="82c4d-238">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="82c4d-238">String</span></span> |<span data-ttu-id="82c4d-239">A részállapot hello feladat állapot (operationName).</span><span class="sxs-lookup"><span data-stu-id="82c4d-239">A substatus for hello job status (operationName).</span></span> |
| <span data-ttu-id="82c4d-240">resultSignature</span><span class="sxs-lookup"><span data-stu-id="82c4d-240">resultSignature</span></span> |<span data-ttu-id="82c4d-241">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="82c4d-241">String</span></span> |<span data-ttu-id="82c4d-242">További részletek a hello feladat állapota (operationName).</span><span class="sxs-lookup"><span data-stu-id="82c4d-242">Additional details on hello job status (operationName).</span></span> |
| <span data-ttu-id="82c4d-243">identity</span><span class="sxs-lookup"><span data-stu-id="82c4d-243">identity</span></span> |<span data-ttu-id="82c4d-244">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="82c4d-244">String</span></span> |<span data-ttu-id="82c4d-245">a kért műveletet hello hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="82c4d-245">hello user that requested hello operation.</span></span> <span data-ttu-id="82c4d-246">Például: susan@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="82c4d-246">For example, susan@contoso.com.</span></span> |
| <span data-ttu-id="82c4d-247">properties</span><span class="sxs-lookup"><span data-stu-id="82c4d-247">properties</span></span> |<span data-ttu-id="82c4d-248">JSON</span><span class="sxs-lookup"><span data-stu-id="82c4d-248">JSON</span></span> |<span data-ttu-id="82c4d-249">Lásd az hello következő szakaszát (naplózási tulajdonságai séma)</span><span class="sxs-lookup"><span data-stu-id="82c4d-249">See hello next section (Audit log properties schema) for details</span></span> |

> [!NOTE]
> <span data-ttu-id="82c4d-250">**resultType** és **resultSignature** adatokat hello művelet eredményét, és csak tartalmaz értéket, ha egy művelet befejeződött.</span><span class="sxs-lookup"><span data-stu-id="82c4d-250">**resultType** and **resultSignature** provide information on hello result of an operation, and only contain a value if an operation has completed.</span></span> <span data-ttu-id="82c4d-251">Például csak tartalmaznak egy érték amikor **operationName** szereplő érték **JobStarted** vagy **JobEnded**.</span><span class="sxs-lookup"><span data-stu-id="82c4d-251">For example, they only contain a value when **operationName** contains a value of **JobStarted** or **JobEnded**.</span></span>
>
>

#### <a name="audit-log-properties-schema"></a><span data-ttu-id="82c4d-252">Naplózási tulajdonságai séma</span><span class="sxs-lookup"><span data-stu-id="82c4d-252">Audit log properties schema</span></span>

| <span data-ttu-id="82c4d-253">Név</span><span class="sxs-lookup"><span data-stu-id="82c4d-253">Name</span></span> | <span data-ttu-id="82c4d-254">Típus</span><span class="sxs-lookup"><span data-stu-id="82c4d-254">Type</span></span> | <span data-ttu-id="82c4d-255">Leírás</span><span class="sxs-lookup"><span data-stu-id="82c4d-255">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="82c4d-256">JobId</span><span class="sxs-lookup"><span data-stu-id="82c4d-256">JobId</span></span> |<span data-ttu-id="82c4d-257">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="82c4d-257">String</span></span> |<span data-ttu-id="82c4d-258">hello azonosító hozzárendelt toohello feladat</span><span class="sxs-lookup"><span data-stu-id="82c4d-258">hello ID assigned toohello job</span></span> |
| <span data-ttu-id="82c4d-259">Feladat neve</span><span class="sxs-lookup"><span data-stu-id="82c4d-259">JobName</span></span> |<span data-ttu-id="82c4d-260">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="82c4d-260">String</span></span> |<span data-ttu-id="82c4d-261">a megadott hello feladat hello neve</span><span class="sxs-lookup"><span data-stu-id="82c4d-261">hello name that was provided for hello job</span></span> |
| <span data-ttu-id="82c4d-262">JobRunTime</span><span class="sxs-lookup"><span data-stu-id="82c4d-262">JobRunTime</span></span> |<span data-ttu-id="82c4d-263">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="82c4d-263">String</span></span> |<span data-ttu-id="82c4d-264">hello futásidejű használt tooprocess hello feladat</span><span class="sxs-lookup"><span data-stu-id="82c4d-264">hello runtime used tooprocess hello job</span></span> |
| <span data-ttu-id="82c4d-265">SubmitTime</span><span class="sxs-lookup"><span data-stu-id="82c4d-265">SubmitTime</span></span> |<span data-ttu-id="82c4d-266">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="82c4d-266">String</span></span> |<span data-ttu-id="82c4d-267">hello ideje (UTC) az adott hello feladat el lett küldve</span><span class="sxs-lookup"><span data-stu-id="82c4d-267">hello time (in UTC) that hello job was submitted</span></span> |
| <span data-ttu-id="82c4d-268">Kezdő időpont</span><span class="sxs-lookup"><span data-stu-id="82c4d-268">StartTime</span></span> |<span data-ttu-id="82c4d-269">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="82c4d-269">String</span></span> |<span data-ttu-id="82c4d-270">hello idő hello feladat indulása után küldése (az UTC)</span><span class="sxs-lookup"><span data-stu-id="82c4d-270">hello time hello job started running after submission (in UTC)</span></span> |
| <span data-ttu-id="82c4d-271">Befejezés időpontja</span><span class="sxs-lookup"><span data-stu-id="82c4d-271">EndTime</span></span> |<span data-ttu-id="82c4d-272">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="82c4d-272">String</span></span> |<span data-ttu-id="82c4d-273">hello idő hello feladat befejeződött</span><span class="sxs-lookup"><span data-stu-id="82c4d-273">hello time hello job ended</span></span> |
| <span data-ttu-id="82c4d-274">Párhuzamos végrehajtás</span><span class="sxs-lookup"><span data-stu-id="82c4d-274">Parallelism</span></span> |<span data-ttu-id="82c4d-275">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="82c4d-275">String</span></span> |<span data-ttu-id="82c4d-276">Ez a feladat kért végzett leadásakor Data Lake Analytics egységek hello száma</span><span class="sxs-lookup"><span data-stu-id="82c4d-276">hello number of Data Lake Analytics units requested for this job during submission</span></span> |

> [!NOTE]
> <span data-ttu-id="82c4d-277">**SubmitTime**, **StartTime**, **EndTime**, és **párhuzamossági** művelet kapcsolatban nyújtanak információkat.</span><span class="sxs-lookup"><span data-stu-id="82c4d-277">**SubmitTime**, **StartTime**, **EndTime**, and **Parallelism** provide information on an operation.</span></span> <span data-ttu-id="82c4d-278">Ezek a bejegyzések csak tartalmaz értéket ha művelet indítása vagy befejeződött.</span><span class="sxs-lookup"><span data-stu-id="82c4d-278">These entries only contain a value if that operation has started or completed.</span></span> <span data-ttu-id="82c4d-279">Például **SubmitTime** csak értéke után **operationName** hello értéke **JobSubmitted**.</span><span class="sxs-lookup"><span data-stu-id="82c4d-279">For example, **SubmitTime** only contains a value after **operationName** has hello value **JobSubmitted**.</span></span>

## <a name="process-hello-log-data"></a><span data-ttu-id="82c4d-280">Folyamatok hello naplóadatok</span><span class="sxs-lookup"><span data-stu-id="82c4d-280">Process hello log data</span></span>

<span data-ttu-id="82c4d-281">Az Azure Data Lake Analytics biztosít egy minta tooprocess és elemezheti a hello naplóadatokat.</span><span class="sxs-lookup"><span data-stu-id="82c4d-281">Azure Data Lake Analytics provides a sample on how tooprocess and analyze hello log data.</span></span> <span data-ttu-id="82c4d-282">Hello minta a található [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample).</span><span class="sxs-lookup"><span data-stu-id="82c4d-282">You can find hello sample at [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample).</span></span>

## <a name="next-steps"></a><span data-ttu-id="82c4d-283">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="82c4d-283">Next steps</span></span>
* [<span data-ttu-id="82c4d-284">Az Azure Data Lake Analytics áttekintése</span><span class="sxs-lookup"><span data-stu-id="82c4d-284">Overview of Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
