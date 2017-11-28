---
title: "Az Azure Data Lake Store diagnosztikai naplók megtekintése |} Microsoft Docs"
description: "Ismerje meg, hogyan kell beállítania, és hozzáférés az Azure Data Lake Store diagnosztikai naplók "
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: f6e75eb1-d0ae-47cf-bdb8-06684b7c0a94
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: b7a38ec445ef0ce13f3f1931e8ee246dce6412a5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="accessing-diagnostic-logs-for-azure-data-lake-store"></a><span data-ttu-id="d6938-103">Diagnosztikai naplók az Azure Data Lake Store elérése</span><span class="sxs-lookup"><span data-stu-id="d6938-103">Accessing diagnostic logs for Azure Data Lake Store</span></span>
<span data-ttu-id="d6938-104">Ismerje meg a Data Lake Store-fiók diagnosztikai naplózás engedélyezése és a fiókja gyűjtött naplók megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="d6938-104">Learn about how to enable diagnostic logging for your Data Lake Store account and how to view the logs collected for your account.</span></span>

<span data-ttu-id="d6938-105">A szervezetek diagnosztikai naplózását is az Azure Data Lake Store fiók gyűjthet adatokat a fájlhozzáférés napló ellenőrzését, amely bemutatja, például a listát a felhasználók fér hozzá az adatokhoz, hogy milyen gyakran az adatokhoz, mennyi adatot a fiók tárolva van stb.</span><span class="sxs-lookup"><span data-stu-id="d6938-105">Organizations can enable diagnostic logging for their Azure Data Lake Store account to collect data access audit trails that provides information such as list of users accessing the data, how frequently the data is accessed, how much data is stored in the account, etc.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d6938-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d6938-106">Prerequisites</span></span>
* <span data-ttu-id="d6938-107">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="d6938-107">**An Azure subscription**.</span></span> <span data-ttu-id="d6938-108">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d6938-108">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="d6938-109">**Azure Data Lake Store-fiók**.</span><span class="sxs-lookup"><span data-stu-id="d6938-109">**Azure Data Lake Store account**.</span></span> <span data-ttu-id="d6938-110">Kövesse [Az Azure Data Lake Store használatának első lépései az Azure Portal használatával](data-lake-store-get-started-portal.md) című témakör utasításait.</span><span class="sxs-lookup"><span data-stu-id="d6938-110">Follow the instructions at [Get started with Azure Data Lake Store using the Azure Portal](data-lake-store-get-started-portal.md).</span></span>

## <a name="enable-diagnostic-logging-for-your-data-lake-store-account"></a><span data-ttu-id="d6938-111">A Data Lake Store-fiók diagnosztikai naplózás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="d6938-111">Enable diagnostic logging for your Data Lake Store account</span></span>
1. <span data-ttu-id="d6938-112">Jelentkezzen be az új [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d6938-112">Sign on to the new [Azure Portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="d6938-113">Nyissa meg a Data Lake Store-fiókot, és a Data Lake Store-fiók panelen kattintson **beállítások**, és kattintson a **diagnosztikai naplók**.</span><span class="sxs-lookup"><span data-stu-id="d6938-113">Open your Data Lake Store account, and from your Data Lake Store account blade, click **Settings**, and then click **Diagnostic logs**.</span></span>
3. <span data-ttu-id="d6938-114">Az a **diagnosztikai naplók** panelen kattintson a **a diagnosztika bekapcsolásához**.</span><span class="sxs-lookup"><span data-stu-id="d6938-114">In the **Diagnostics logs** blade, click **Turn on diagnostics**.</span></span>

    <span data-ttu-id="d6938-115">![Diagnosztikai naplózás engedélyezése](./media/data-lake-store-diagnostic-logs/turn-on-diagnostics.png "diagnosztikai naplók engedélyezése")</span><span class="sxs-lookup"><span data-stu-id="d6938-115">![Enable diagnostic logging](./media/data-lake-store-diagnostic-logs/turn-on-diagnostics.png "Enable diagnostic logs")</span></span>

3. <span data-ttu-id="d6938-116">Az a **diagnosztikai** panelen diagnosztikai naplózás konfigurálása a következő módosításokat.</span><span class="sxs-lookup"><span data-stu-id="d6938-116">In the **Diagnostic** blade, make the following changes to configure diagnostic logging.</span></span>
   
    <span data-ttu-id="d6938-117">![Diagnosztikai naplózás engedélyezése](./media/data-lake-store-diagnostic-logs/enable-diagnostic-logs.png "diagnosztikai naplók engedélyezése")</span><span class="sxs-lookup"><span data-stu-id="d6938-117">![Enable diagnostic logging](./media/data-lake-store-diagnostic-logs/enable-diagnostic-logs.png "Enable diagnostic logs")</span></span>
   
   * <span data-ttu-id="d6938-118">Állítsa be **állapot** való **a** diagnosztikai naplózás engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d6938-118">Set **Status** to **On** to enable diagnostic logging.</span></span>
   * <span data-ttu-id="d6938-119">Ha szeretné, az adatok tárolási/folyamat más módon.</span><span class="sxs-lookup"><span data-stu-id="d6938-119">You can choose to store/process the data in different ways.</span></span>
     
        * <span data-ttu-id="d6938-120">Jelölje be a **tárfiókba archív** bejegyzéseit, amelyek egy Azure Storage-fiók tárolásához.</span><span class="sxs-lookup"><span data-stu-id="d6938-120">Select the option to **Archive to a storage account** to store logs to an Azure Storage account.</span></span> <span data-ttu-id="d6938-121">Ha az adatokat, amelyek lesznek kötegelt feldolgozásra egy későbbi időpontban archiválni szeretné ezt a beállítást használja.</span><span class="sxs-lookup"><span data-stu-id="d6938-121">You use this option if you want to archive the data that will be batch-processed at a later date.</span></span> <span data-ttu-id="d6938-122">Ha ezt a beállítást meg kell adnia egy Azure Storage-fiók mentése a naplókat.</span><span class="sxs-lookup"><span data-stu-id="d6938-122">If you select this option you must provide an Azure Storage account to save the logs to.</span></span>
        
        * <span data-ttu-id="d6938-123">Jelölje be a **adatfolyam egy eseményközpontba** adatfolyam napló adatokat az Azure-Eseményközpontok felé.</span><span class="sxs-lookup"><span data-stu-id="d6938-123">Select the option to **Stream to an event hub** to stream log data to an Azure Event Hub.</span></span> <span data-ttu-id="d6938-124">Valószínűleg ezt a beállítást fogja használni, ha egy alárendelt feldolgozási folyamat bejövő naplók valós időben elemezni.</span><span class="sxs-lookup"><span data-stu-id="d6938-124">Most likely you will use this option if you have a downstream processing pipeline to analyze incoming logs at real time.</span></span> <span data-ttu-id="d6938-125">Ha ezt a lehetőséget választja, meg kell adnia a használni kívánt Azure Event Hubs részleteit.</span><span class="sxs-lookup"><span data-stu-id="d6938-125">If you select this option, you must provide the details for the Azure Event Hub you want to use.</span></span>

        * <span data-ttu-id="d6938-126">Jelölje be a **küldeni a Naplóelemzési** használhatja az Azure Naplóelemzés szolgáltatást a előállított naplózási adatok elemzésére.</span><span class="sxs-lookup"><span data-stu-id="d6938-126">Select the option to **Send to Log Analytics** to use the Azure Log Analytics service to analyze the generated log data.</span></span> <span data-ttu-id="d6938-127">Ha ezt a lehetőséget választja, meg kell adnia a részletek az Operations Management Suite-munkaterülettel a végezze el a webhelynapló elemzése használható.</span><span class="sxs-lookup"><span data-stu-id="d6938-127">If you select this option, you must provide the details for the Operations Management Suite workspace that you would use the perform log analysis.</span></span>
     
   * <span data-ttu-id="d6938-128">Adja meg, hogy megkapják a naplók vagy kérelmek naplóit vagy mindkettőt.</span><span class="sxs-lookup"><span data-stu-id="d6938-128">Specify whether you want to get audit logs or request logs or both.</span></span>
   * <span data-ttu-id="d6938-129">Adja meg, hány nap, amelynek meg kell őrizni az adatokat.</span><span class="sxs-lookup"><span data-stu-id="d6938-129">Specify the number of days for which the data must be retained.</span></span> <span data-ttu-id="d6938-130">Megőrzési csak akkor alkalmazható, ha az Azure storage-fiók segítségével archiválja naplóadatokat.</span><span class="sxs-lookup"><span data-stu-id="d6938-130">Retention is only applicable if you are using Azure storage account to archive log data.</span></span>
   * <span data-ttu-id="d6938-131">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="d6938-131">Click **Save**.</span></span>

<span data-ttu-id="d6938-132">Miután engedélyezte a diagnosztikai beállítások, a naplófájlok az figyelemmel követheti a **diagnosztikai naplók** fülre.</span><span class="sxs-lookup"><span data-stu-id="d6938-132">Once you have enabled diagnostic settings, you can watch the logs in the **Diagnostic Logs** tab.</span></span>

## <a name="view-diagnostic-logs-for-your-data-lake-store-account"></a><span data-ttu-id="d6938-133">A Data Lake Store-fiók diagnosztikai naplók megtekintése</span><span class="sxs-lookup"><span data-stu-id="d6938-133">View diagnostic logs for your Data Lake Store account</span></span>
<span data-ttu-id="d6938-134">A Data Lake Store-fiók a naplóadatok megtekintéséhez két módja van.</span><span class="sxs-lookup"><span data-stu-id="d6938-134">There are two ways to view the log data for your Data Lake Store account.</span></span>

* <span data-ttu-id="d6938-135">A Data Lake Store-fiókból beállítások megtekintése</span><span class="sxs-lookup"><span data-stu-id="d6938-135">From the Data Lake Store account settings view</span></span>
* <span data-ttu-id="d6938-136">Az adatokat tároló Azure Storage-fiókból</span><span class="sxs-lookup"><span data-stu-id="d6938-136">From the Azure Storage account where the data is stored</span></span>

### <a name="using-the-data-lake-store-settings-view"></a><span data-ttu-id="d6938-137">Használatával a Data Lake Store beállítások megtekintése</span><span class="sxs-lookup"><span data-stu-id="d6938-137">Using the Data Lake Store Settings view</span></span>
1. <span data-ttu-id="d6938-138">A Data Lake Store-fiókból **beállítások** panelen kattintson a **diagnosztikai naplók**.</span><span class="sxs-lookup"><span data-stu-id="d6938-138">From your Data Lake Store account **Settings** blade, click **Diagnostic Logs**.</span></span>
   
    <span data-ttu-id="d6938-139">![Diagnosztikai naplózás nézet](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs.png "diagnosztikai naplók megtekintése")</span><span class="sxs-lookup"><span data-stu-id="d6938-139">![View diagnostic logging](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs.png "View diagnostic logs")</span></span> 
2. <span data-ttu-id="d6938-140">Az a **diagnosztikai naplók** panelen megjelenik a naplók kategorizálta **naplók** és **kérelem naplók**.</span><span class="sxs-lookup"><span data-stu-id="d6938-140">In the **Diagnostic Logs** blade, you should see the logs categorized by **Audit Logs** and **Request Logs**.</span></span>
   
   * <span data-ttu-id="d6938-141">Kérelem naplók rögzítése a Data Lake Store-fiók minden API kérelmet.</span><span class="sxs-lookup"><span data-stu-id="d6938-141">Request logs capture every API request made on the Data Lake Store account.</span></span>
   * <span data-ttu-id="d6938-142">Naplók hasonlóak naplók kérése, de a műveletek végrehajtása a Data Lake Store-fiók sokkal részletesebb információkat biztosít.</span><span class="sxs-lookup"><span data-stu-id="d6938-142">Audit Logs are similar to request Logs but provide a much more detailed breakdown of the operations being performed on the Data Lake Store account.</span></span> <span data-ttu-id="d6938-143">Például egy egyetlen feltöltés API-hívás a kérelem naplókban több "Append" műveletet a naplófájlban eredményezheti.</span><span class="sxs-lookup"><span data-stu-id="d6938-143">For example, a single upload API call in request logs might result in multiple "Append" operations in the audit logs.</span></span>
3. <span data-ttu-id="d6938-144">Kattintson a **letöltése** hivatkozás minden naplóbejegyzés a naplók letöltéséhez ellen.</span><span class="sxs-lookup"><span data-stu-id="d6938-144">Click the **Download** link against each log entry to download the logs.</span></span>

### <a name="from-the-azure-storage-account-that-contains-log-data"></a><span data-ttu-id="d6938-145">Az Azure Storage-fiókhoz, amely tartalmazza adatainak naplózása</span><span class="sxs-lookup"><span data-stu-id="d6938-145">From the Azure Storage account that contains log data</span></span>
1. <span data-ttu-id="d6938-146">Nyissa meg a naplózás a Data Lake Store társított Azure Storage-fiók panelen, és kattintson a Blobok.</span><span class="sxs-lookup"><span data-stu-id="d6938-146">Open the Azure Storage account blade associated with Data Lake Store for logging, and then click Blobs.</span></span> <span data-ttu-id="d6938-147">A **Blob szolgáltatás** panel két tárolók listája.</span><span class="sxs-lookup"><span data-stu-id="d6938-147">The **Blob service** blade lists two containers.</span></span>
   
    <span data-ttu-id="d6938-148">![Diagnosztikai naplózás nézet](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account.png "diagnosztikai naplók megtekintése")</span><span class="sxs-lookup"><span data-stu-id="d6938-148">![View diagnostic logging](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account.png "View diagnostic logs")</span></span>
   
   * <span data-ttu-id="d6938-149">A tároló **insights-logs-naplózási** tartalmazza a naplókat.</span><span class="sxs-lookup"><span data-stu-id="d6938-149">The container **insights-logs-audit** contains the audit logs.</span></span>
   * <span data-ttu-id="d6938-150">A tároló **insights-logs-kérelmek** tartalmaz a kérelmek naplóit.</span><span class="sxs-lookup"><span data-stu-id="d6938-150">The container **insights-logs-requests** contains the request logs.</span></span>
2. <span data-ttu-id="d6938-151">Ezek a tárolók belül a naplók tárolt az alábbi szerkezettel.</span><span class="sxs-lookup"><span data-stu-id="d6938-151">Within these containers, the logs are stored under the following structure.</span></span>
   
    <span data-ttu-id="d6938-152">![Diagnosztikai naplózás nézet](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account-structure.png "diagnosztikai naplók megtekintése")</span><span class="sxs-lookup"><span data-stu-id="d6938-152">![View diagnostic logging](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account-structure.png "View diagnostic logs")</span></span>
   
    <span data-ttu-id="d6938-153">Tegyük fel a teljes elérési útját, és egy naplófájlba lehet`https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=04/m=00/PT1H.json`</span><span class="sxs-lookup"><span data-stu-id="d6938-153">As an example, the complete path to an audit log could be `https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=04/m=00/PT1H.json`</span></span>
   
    <span data-ttu-id="d6938-154">Similary, teljes elérési útját a napló lehet`https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=14/m=00/PT1H.json`</span><span class="sxs-lookup"><span data-stu-id="d6938-154">Similary, the complete path to a request log could be `https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=14/m=00/PT1H.json`</span></span>

## <a name="understand-the-structure-of-the-log-data"></a><span data-ttu-id="d6938-155">A naplózási adatok szerkezete ismertetése</span><span class="sxs-lookup"><span data-stu-id="d6938-155">Understand the structure of the log data</span></span>
<span data-ttu-id="d6938-156">A naplózási és kérelem naplók JSON formátumban vannak.</span><span class="sxs-lookup"><span data-stu-id="d6938-156">The audit and request logs are in a JSON format.</span></span> <span data-ttu-id="d6938-157">Ez a szakasz azt nézze meg a kérelem JSON szerkezete és a naplók.</span><span class="sxs-lookup"><span data-stu-id="d6938-157">In this section, we look at the structure of JSON for request and audit logs.</span></span>

### <a name="request-logs"></a><span data-ttu-id="d6938-158">Naplók kérése</span><span class="sxs-lookup"><span data-stu-id="d6938-158">Request logs</span></span>
<span data-ttu-id="d6938-159">Íme egy minta-bejegyzést a JSON-formátumú kérelem naplóban.</span><span class="sxs-lookup"><span data-stu-id="d6938-159">Here's a sample entry in the JSON-formatted request log.</span></span> <span data-ttu-id="d6938-160">Minden egyes blob tartozik egy legfelső szintű objektum nevű **rekordok** , amely tartalmazza a napló objektumokból álló tömb.</span><span class="sxs-lookup"><span data-stu-id="d6938-160">Each blob has one root object called **records** that contains an array of log objects.</span></span>

    {
    "records": 
      [        
        . . . .
        ,
        {
             "time": "2016-07-07T21:02:53.456Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Requests",
             "operationName": "GETCustomerIngressEgress",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {"HttpMethod":"GET","Path":"/webhdfs/v1/Samples/Outputs/Drivers.csv","RequestContentLength":0,"ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8","StartTime":"2016-07-07T21:02:52.472Z","EndTime":"2016-07-07T21:02:53.456Z"}
        }
        ,
        . . . .
      ]
    }

#### <a name="request-log-schema"></a><span data-ttu-id="d6938-161">Kérelem séma</span><span class="sxs-lookup"><span data-stu-id="d6938-161">Request log schema</span></span>
| <span data-ttu-id="d6938-162">Név</span><span class="sxs-lookup"><span data-stu-id="d6938-162">Name</span></span> | <span data-ttu-id="d6938-163">Típus</span><span class="sxs-lookup"><span data-stu-id="d6938-163">Type</span></span> | <span data-ttu-id="d6938-164">Leírás</span><span class="sxs-lookup"><span data-stu-id="d6938-164">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d6938-165">time</span><span class="sxs-lookup"><span data-stu-id="d6938-165">time</span></span> |<span data-ttu-id="d6938-166">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="d6938-166">String</span></span> |<span data-ttu-id="d6938-167">Az időbélyeg (UTC szerint) a napló</span><span class="sxs-lookup"><span data-stu-id="d6938-167">The timestamp (in UTC) of the log</span></span> |
| <span data-ttu-id="d6938-168">resourceId</span><span class="sxs-lookup"><span data-stu-id="d6938-168">resourceId</span></span> |<span data-ttu-id="d6938-169">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="d6938-169">String</span></span> |<span data-ttu-id="d6938-170">Helyezze a művelet erőforrás azonosítója</span><span class="sxs-lookup"><span data-stu-id="d6938-170">The ID of the resource that operation took place on</span></span> |
| <span data-ttu-id="d6938-171">category</span><span class="sxs-lookup"><span data-stu-id="d6938-171">category</span></span> |<span data-ttu-id="d6938-172">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="d6938-172">String</span></span> |<span data-ttu-id="d6938-173">A napló kategóriát.</span><span class="sxs-lookup"><span data-stu-id="d6938-173">The log category.</span></span> <span data-ttu-id="d6938-174">Például **kérelmek**.</span><span class="sxs-lookup"><span data-stu-id="d6938-174">For example, **Requests**.</span></span> |
| <span data-ttu-id="d6938-175">operationName</span><span class="sxs-lookup"><span data-stu-id="d6938-175">operationName</span></span> |<span data-ttu-id="d6938-176">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="d6938-176">String</span></span> |<span data-ttu-id="d6938-177">A művelet naplózott neve.</span><span class="sxs-lookup"><span data-stu-id="d6938-177">Name of the operation that is logged.</span></span> <span data-ttu-id="d6938-178">Például getfilestatus.</span><span class="sxs-lookup"><span data-stu-id="d6938-178">For example, getfilestatus.</span></span> |
| <span data-ttu-id="d6938-179">resultType</span><span class="sxs-lookup"><span data-stu-id="d6938-179">resultType</span></span> |<span data-ttu-id="d6938-180">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="d6938-180">String</span></span> |<span data-ttu-id="d6938-181">A művelet, például 200 állapotát.</span><span class="sxs-lookup"><span data-stu-id="d6938-181">The status of the operation, For example, 200.</span></span> |
| <span data-ttu-id="d6938-182">callerIpAddress</span><span class="sxs-lookup"><span data-stu-id="d6938-182">callerIpAddress</span></span> |<span data-ttu-id="d6938-183">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="d6938-183">String</span></span> |<span data-ttu-id="d6938-184">A kérést küldő ügyfél IP-címe</span><span class="sxs-lookup"><span data-stu-id="d6938-184">The IP address of the client making the request</span></span> |
| <span data-ttu-id="d6938-185">correlationId</span><span class="sxs-lookup"><span data-stu-id="d6938-185">correlationId</span></span> |<span data-ttu-id="d6938-186">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="d6938-186">String</span></span> |<span data-ttu-id="d6938-187">A napló, amelyek azonosítója használt csoportba a kapcsolódó naplóbejegyzések készlete</span><span class="sxs-lookup"><span data-stu-id="d6938-187">The id of the log that can used to group together a set of related log entries</span></span> |
| <span data-ttu-id="d6938-188">identity</span><span class="sxs-lookup"><span data-stu-id="d6938-188">identity</span></span> |<span data-ttu-id="d6938-189">Objektum</span><span class="sxs-lookup"><span data-stu-id="d6938-189">Object</span></span> |<span data-ttu-id="d6938-190">Az identitás, amely a napló jön létre</span><span class="sxs-lookup"><span data-stu-id="d6938-190">The identity that generated the log</span></span> |
| <span data-ttu-id="d6938-191">properties</span><span class="sxs-lookup"><span data-stu-id="d6938-191">properties</span></span> |<span data-ttu-id="d6938-192">JSON</span><span class="sxs-lookup"><span data-stu-id="d6938-192">JSON</span></span> |<span data-ttu-id="d6938-193">További információ alább olvasható</span><span class="sxs-lookup"><span data-stu-id="d6938-193">See below for details</span></span> |

#### <a name="request-log-properties-schema"></a><span data-ttu-id="d6938-194">Kérelem tulajdonságok séma</span><span class="sxs-lookup"><span data-stu-id="d6938-194">Request log properties schema</span></span>
| <span data-ttu-id="d6938-195">Név</span><span class="sxs-lookup"><span data-stu-id="d6938-195">Name</span></span> | <span data-ttu-id="d6938-196">Típus</span><span class="sxs-lookup"><span data-stu-id="d6938-196">Type</span></span> | <span data-ttu-id="d6938-197">Leírás</span><span class="sxs-lookup"><span data-stu-id="d6938-197">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d6938-198">HttpMethod</span><span class="sxs-lookup"><span data-stu-id="d6938-198">HttpMethod</span></span> |<span data-ttu-id="d6938-199">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="d6938-199">String</span></span> |<span data-ttu-id="d6938-200">A művelethez használt HTTP-metódust.</span><span class="sxs-lookup"><span data-stu-id="d6938-200">The HTTP Method used for the operation.</span></span> <span data-ttu-id="d6938-201">Például beolvasása.</span><span class="sxs-lookup"><span data-stu-id="d6938-201">For example, GET.</span></span> |
| <span data-ttu-id="d6938-202">Elérési út</span><span class="sxs-lookup"><span data-stu-id="d6938-202">Path</span></span> |<span data-ttu-id="d6938-203">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="d6938-203">String</span></span> |<span data-ttu-id="d6938-204">Az elérési út a művelet végrehajtásának ideje</span><span class="sxs-lookup"><span data-stu-id="d6938-204">The path the operation was performed on</span></span> |
| <span data-ttu-id="d6938-205">RequestContentLength</span><span class="sxs-lookup"><span data-stu-id="d6938-205">RequestContentLength</span></span> |<span data-ttu-id="d6938-206">int</span><span class="sxs-lookup"><span data-stu-id="d6938-206">int</span></span> |<span data-ttu-id="d6938-207">A HTTP-kérelem a tartalom hossza</span><span class="sxs-lookup"><span data-stu-id="d6938-207">The content length of the HTTP request</span></span> |
| <span data-ttu-id="d6938-208">clientRequestId</span><span class="sxs-lookup"><span data-stu-id="d6938-208">ClientRequestId</span></span> |<span data-ttu-id="d6938-209">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="d6938-209">String</span></span> |<span data-ttu-id="d6938-210">Az azonosító, amely egyedileg azonosítja az ehhez a kérelemhez</span><span class="sxs-lookup"><span data-stu-id="d6938-210">The Id that uniquely identifies this request</span></span> |
| <span data-ttu-id="d6938-211">Kezdő időpont</span><span class="sxs-lookup"><span data-stu-id="d6938-211">StartTime</span></span> |<span data-ttu-id="d6938-212">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="d6938-212">String</span></span> |<span data-ttu-id="d6938-213">Az a kiszolgáló fogadja a kérelem ideje</span><span class="sxs-lookup"><span data-stu-id="d6938-213">The time at which the server received the request</span></span> |
| <span data-ttu-id="d6938-214">Befejezés időpontja</span><span class="sxs-lookup"><span data-stu-id="d6938-214">EndTime</span></span> |<span data-ttu-id="d6938-215">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="d6938-215">String</span></span> |<span data-ttu-id="d6938-216">Az idő, ahol a kiszolgáló által küldött választ</span><span class="sxs-lookup"><span data-stu-id="d6938-216">The time at which the server sent a response</span></span> |

### <a name="audit-logs"></a><span data-ttu-id="d6938-217">Naplók</span><span class="sxs-lookup"><span data-stu-id="d6938-217">Audit logs</span></span>
<span data-ttu-id="d6938-218">Íme egy minta-bejegyzést a JSON-formátumú naplóban.</span><span class="sxs-lookup"><span data-stu-id="d6938-218">Here's a sample entry in the JSON-formatted audit log.</span></span> <span data-ttu-id="d6938-219">Minden egyes blob tartozik egy legfelső szintű objektum nevű **rekordok** napló objektumok tömbjét tartalmazza, amelyek</span><span class="sxs-lookup"><span data-stu-id="d6938-219">Each blob has one root object called **records** that contains an array of log objects</span></span>

    {
    "records": 
      [        
        . . . .
        ,
        {
             "time": "2016-07-08T19:08:59.359Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Audit",
             "operationName": "SeOpenStream",
             "resultType": "0",
             "correlationId": "381110fc03534e1cb99ec52376ceebdf;Append_BrEKAmg;25.66.9.145",
             "identity": "A9DAFFAF-FFEE-4BB5-A4A0-1B6CBBF24355",
             "properties": {"StreamName":"adl://<data_lake_store_account_name>.azuredatalakestore.net/logs.csv"}
        }
        ,
        . . . .
      ]
    }

#### <a name="audit-log-schema"></a><span data-ttu-id="d6938-220">Naplózási séma</span><span class="sxs-lookup"><span data-stu-id="d6938-220">Audit log schema</span></span>
| <span data-ttu-id="d6938-221">Név</span><span class="sxs-lookup"><span data-stu-id="d6938-221">Name</span></span> | <span data-ttu-id="d6938-222">Típus</span><span class="sxs-lookup"><span data-stu-id="d6938-222">Type</span></span> | <span data-ttu-id="d6938-223">Leírás</span><span class="sxs-lookup"><span data-stu-id="d6938-223">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d6938-224">time</span><span class="sxs-lookup"><span data-stu-id="d6938-224">time</span></span> |<span data-ttu-id="d6938-225">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="d6938-225">String</span></span> |<span data-ttu-id="d6938-226">Az időbélyeg (UTC szerint) a napló</span><span class="sxs-lookup"><span data-stu-id="d6938-226">The timestamp (in UTC) of the log</span></span> |
| <span data-ttu-id="d6938-227">resourceId</span><span class="sxs-lookup"><span data-stu-id="d6938-227">resourceId</span></span> |<span data-ttu-id="d6938-228">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="d6938-228">String</span></span> |<span data-ttu-id="d6938-229">Helyezze a művelet erőforrás azonosítója</span><span class="sxs-lookup"><span data-stu-id="d6938-229">The ID of the resource that operation took place on</span></span> |
| <span data-ttu-id="d6938-230">category</span><span class="sxs-lookup"><span data-stu-id="d6938-230">category</span></span> |<span data-ttu-id="d6938-231">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="d6938-231">String</span></span> |<span data-ttu-id="d6938-232">A napló kategóriát.</span><span class="sxs-lookup"><span data-stu-id="d6938-232">The log category.</span></span> <span data-ttu-id="d6938-233">Például **naplózási**.</span><span class="sxs-lookup"><span data-stu-id="d6938-233">For example, **Audit**.</span></span> |
| <span data-ttu-id="d6938-234">operationName</span><span class="sxs-lookup"><span data-stu-id="d6938-234">operationName</span></span> |<span data-ttu-id="d6938-235">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="d6938-235">String</span></span> |<span data-ttu-id="d6938-236">A művelet naplózott neve.</span><span class="sxs-lookup"><span data-stu-id="d6938-236">Name of the operation that is logged.</span></span> <span data-ttu-id="d6938-237">Például getfilestatus.</span><span class="sxs-lookup"><span data-stu-id="d6938-237">For example, getfilestatus.</span></span> |
| <span data-ttu-id="d6938-238">resultType</span><span class="sxs-lookup"><span data-stu-id="d6938-238">resultType</span></span> |<span data-ttu-id="d6938-239">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="d6938-239">String</span></span> |<span data-ttu-id="d6938-240">A művelet, például 200 állapotát.</span><span class="sxs-lookup"><span data-stu-id="d6938-240">The status of the operation, For example, 200.</span></span> |
| <span data-ttu-id="d6938-241">correlationId</span><span class="sxs-lookup"><span data-stu-id="d6938-241">correlationId</span></span> |<span data-ttu-id="d6938-242">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="d6938-242">String</span></span> |<span data-ttu-id="d6938-243">A napló, amelyek azonosítója használt csoportba a kapcsolódó naplóbejegyzések készlete</span><span class="sxs-lookup"><span data-stu-id="d6938-243">The id of the log that can used to group together a set of related log entries</span></span> |
| <span data-ttu-id="d6938-244">identity</span><span class="sxs-lookup"><span data-stu-id="d6938-244">identity</span></span> |<span data-ttu-id="d6938-245">Objektum</span><span class="sxs-lookup"><span data-stu-id="d6938-245">Object</span></span> |<span data-ttu-id="d6938-246">Az identitás, amely a napló jön létre</span><span class="sxs-lookup"><span data-stu-id="d6938-246">The identity that generated the log</span></span> |
| <span data-ttu-id="d6938-247">properties</span><span class="sxs-lookup"><span data-stu-id="d6938-247">properties</span></span> |<span data-ttu-id="d6938-248">JSON</span><span class="sxs-lookup"><span data-stu-id="d6938-248">JSON</span></span> |<span data-ttu-id="d6938-249">További információ alább olvasható</span><span class="sxs-lookup"><span data-stu-id="d6938-249">See below for details</span></span> |

#### <a name="audit-log-properties-schema"></a><span data-ttu-id="d6938-250">Naplózási tulajdonságai séma</span><span class="sxs-lookup"><span data-stu-id="d6938-250">Audit log properties schema</span></span>
| <span data-ttu-id="d6938-251">Név</span><span class="sxs-lookup"><span data-stu-id="d6938-251">Name</span></span> | <span data-ttu-id="d6938-252">Típus</span><span class="sxs-lookup"><span data-stu-id="d6938-252">Type</span></span> | <span data-ttu-id="d6938-253">Leírás</span><span class="sxs-lookup"><span data-stu-id="d6938-253">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d6938-254">StreamName</span><span class="sxs-lookup"><span data-stu-id="d6938-254">StreamName</span></span> |<span data-ttu-id="d6938-255">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="d6938-255">String</span></span> |<span data-ttu-id="d6938-256">Az elérési út a művelet végrehajtásának ideje</span><span class="sxs-lookup"><span data-stu-id="d6938-256">The path the operation was performed on</span></span> |

## <a name="samples-to-process-the-log-data"></a><span data-ttu-id="d6938-257">A naplózási adatok feldolgozása a minták</span><span class="sxs-lookup"><span data-stu-id="d6938-257">Samples to process the log data</span></span>
<span data-ttu-id="d6938-258">Azure Data Lake Store minta hogyan feldolgozhatja és elemezheti a naplózási adatokat biztosít.</span><span class="sxs-lookup"><span data-stu-id="d6938-258">Azure Data Lake Store provides a sample on how to process and analyze the log data.</span></span> <span data-ttu-id="d6938-259">A minta a található [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample).</span><span class="sxs-lookup"><span data-stu-id="d6938-259">You can find the sample at [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample).</span></span> 

## <a name="see-also"></a><span data-ttu-id="d6938-260">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="d6938-260">See also</span></span>
* [<span data-ttu-id="d6938-261">Az Azure Data Lake Store áttekintése</span><span class="sxs-lookup"><span data-stu-id="d6938-261">Overview of Azure Data Lake Store</span></span>](data-lake-store-overview.md)
* [<span data-ttu-id="d6938-262">Biztonságos adattárolás a Data Lake Store-ban</span><span class="sxs-lookup"><span data-stu-id="d6938-262">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)

