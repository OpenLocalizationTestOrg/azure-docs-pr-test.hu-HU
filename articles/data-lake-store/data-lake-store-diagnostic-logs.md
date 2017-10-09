---
title: "Azure Data Lake Store aaaViewing diagnosztikai naplókat |} Microsoft Docs"
description: "Megértéséhez hogyan toosetup és hozzáférés az Azure Data Lake Store diagnosztikai naplók "
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
ms.openlocfilehash: 11fbf7f517f97abdcaf809c1ebeeb51424ab2c1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-diagnostic-logs-for-azure-data-lake-store"></a><span data-ttu-id="4408e-103">Diagnosztikai naplók az Azure Data Lake Store elérése</span><span class="sxs-lookup"><span data-stu-id="4408e-103">Accessing diagnostic logs for Azure Data Lake Store</span></span>
<span data-ttu-id="4408e-104">További információk a hogyan tooenable diagnosztikai naplózás a Data Lake Store-fiókot, és hogy miként naplózza az tooview hello gyűjtése a fiókját.</span><span class="sxs-lookup"><span data-stu-id="4408e-104">Learn about how tooenable diagnostic logging for your Data Lake Store account and how tooview hello logs collected for your account.</span></span>

<span data-ttu-id="4408e-105">A szervezetek is az Azure Data Lake Store fiók toocollect adatok a fájlhozzáférés napló ellenőrzését, amely bemutatja, például a lista kapcsolódó felhasználók hello adatok, milyen gyakran hello hozzá az adatokhoz, mennyi adatot tárolja hello diagnosztikai naplózás engedélyezése fiók, stb.</span><span class="sxs-lookup"><span data-stu-id="4408e-105">Organizations can enable diagnostic logging for their Azure Data Lake Store account toocollect data access audit trails that provides information such as list of users accessing hello data, how frequently hello data is accessed, how much data is stored in hello account, etc.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4408e-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4408e-106">Prerequisites</span></span>
* <span data-ttu-id="4408e-107">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="4408e-107">**An Azure subscription**.</span></span> <span data-ttu-id="4408e-108">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4408e-108">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="4408e-109">**Azure Data Lake Store-fiók**.</span><span class="sxs-lookup"><span data-stu-id="4408e-109">**Azure Data Lake Store account**.</span></span> <span data-ttu-id="4408e-110">Hajtsa végre a hello található utasítások segítségével: [Ismerkedés az Azure Data Lake Store használatának hello Azure Portal](data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="4408e-110">Follow hello instructions at [Get started with Azure Data Lake Store using hello Azure Portal](data-lake-store-get-started-portal.md).</span></span>

## <a name="enable-diagnostic-logging-for-your-data-lake-store-account"></a><span data-ttu-id="4408e-111">A Data Lake Store-fiók diagnosztikai naplózás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="4408e-111">Enable diagnostic logging for your Data Lake Store account</span></span>
1. <span data-ttu-id="4408e-112">Jelentkezzen be az új toohello [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4408e-112">Sign on toohello new [Azure Portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="4408e-113">Nyissa meg a Data Lake Store-fiókot, és a Data Lake Store-fiók panelen kattintson **beállítások**, és kattintson a **diagnosztikai naplók**.</span><span class="sxs-lookup"><span data-stu-id="4408e-113">Open your Data Lake Store account, and from your Data Lake Store account blade, click **Settings**, and then click **Diagnostic logs**.</span></span>
3. <span data-ttu-id="4408e-114">A hello **diagnosztikai naplók** panelen kattintson a **a diagnosztika bekapcsolásához**.</span><span class="sxs-lookup"><span data-stu-id="4408e-114">In hello **Diagnostics logs** blade, click **Turn on diagnostics**.</span></span>

    <span data-ttu-id="4408e-115">![Diagnosztikai naplózás engedélyezése](./media/data-lake-store-diagnostic-logs/turn-on-diagnostics.png "diagnosztikai naplók engedélyezése")</span><span class="sxs-lookup"><span data-stu-id="4408e-115">![Enable diagnostic logging](./media/data-lake-store-diagnostic-logs/turn-on-diagnostics.png "Enable diagnostic logs")</span></span>

3. <span data-ttu-id="4408e-116">A hello **diagnosztikai** panelen ellenőrizze a következő módosításokat tooconfigure diagnosztikai naplózás hello.</span><span class="sxs-lookup"><span data-stu-id="4408e-116">In hello **Diagnostic** blade, make hello following changes tooconfigure diagnostic logging.</span></span>
   
    <span data-ttu-id="4408e-117">![Diagnosztikai naplózás engedélyezése](./media/data-lake-store-diagnostic-logs/enable-diagnostic-logs.png "diagnosztikai naplók engedélyezése")</span><span class="sxs-lookup"><span data-stu-id="4408e-117">![Enable diagnostic logging](./media/data-lake-store-diagnostic-logs/enable-diagnostic-logs.png "Enable diagnostic logs")</span></span>
   
   * <span data-ttu-id="4408e-118">Állítsa be **állapot** túl**a** tooenable diagnosztikai naplózás.</span><span class="sxs-lookup"><span data-stu-id="4408e-118">Set **Status** too**On** tooenable diagnostic logging.</span></span>
   * <span data-ttu-id="4408e-119">Kiválaszthatja a toostore/folyamat hello adatokat különböző módon.</span><span class="sxs-lookup"><span data-stu-id="4408e-119">You can choose toostore/process hello data in different ways.</span></span>
     
        * <span data-ttu-id="4408e-120">A beállításnak a hello túl**tooa tárfiók archiválására** toostore naplózza tooan Azure Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="4408e-120">Select hello option too**Archive tooa storage account** toostore logs tooan Azure Storage account.</span></span> <span data-ttu-id="4408e-121">Ezt a beállítást használja, ha azt szeretné, hogy tooarchive hello adatot kötegelt feldolgozásra egy későbbi időpontban.</span><span class="sxs-lookup"><span data-stu-id="4408e-121">You use this option if you want tooarchive hello data that will be batch-processed at a later date.</span></span> <span data-ttu-id="4408e-122">Ha ezt a beállítást meg kell adnia egy Azure Storage-fiók toosave hello naplókat.</span><span class="sxs-lookup"><span data-stu-id="4408e-122">If you select this option you must provide an Azure Storage account toosave hello logs to.</span></span>
        
        * <span data-ttu-id="4408e-123">A beállításnak a hello túl**adatfolyam tooan eseményközpont** toostream napló adatok tooan Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="4408e-123">Select hello option too**Stream tooan event hub** toostream log data tooan Azure Event Hub.</span></span> <span data-ttu-id="4408e-124">Valószínűleg fogja használni ezt a beállítást, ha egy alárendelt feldolgozási tooanalyze bejövő naplók valós időben a következő feldolgozási sorban.</span><span class="sxs-lookup"><span data-stu-id="4408e-124">Most likely you will use this option if you have a downstream processing pipeline tooanalyze incoming logs at real time.</span></span> <span data-ttu-id="4408e-125">Ha ezt a lehetőséget választja, meg kell adnia hello Azure Event Hubs toouse kívánt hello részletei.</span><span class="sxs-lookup"><span data-stu-id="4408e-125">If you select this option, you must provide hello details for hello Azure Event Hub you want toouse.</span></span>

        * <span data-ttu-id="4408e-126">A beállításnak a hello túl**tooLog Analytics küldése** toouse hello Azure Naplóelemzés tooanalyze generált hello Szolgáltatásnapló-adatait.</span><span class="sxs-lookup"><span data-stu-id="4408e-126">Select hello option too**Send tooLog Analytics** toouse hello Azure Log Analytics service tooanalyze hello generated log data.</span></span> <span data-ttu-id="4408e-127">Ha ezt a lehetőséget választja, meg kell adnia, hello a kapcsolatos részleteket hello Operations Management Suite-munkaterülettel, hogy használjon hello hajtaná végre a webhelynapló elemzése.</span><span class="sxs-lookup"><span data-stu-id="4408e-127">If you select this option, you must provide hello details for hello Operations Management Suite workspace that you would use hello perform log analysis.</span></span>
     
   * <span data-ttu-id="4408e-128">Adja meg, hogy tooget naplókat, a kérelmek naplóit vagy mindkettőhöz.</span><span class="sxs-lookup"><span data-stu-id="4408e-128">Specify whether you want tooget audit logs or request logs or both.</span></span>
   * <span data-ttu-id="4408e-129">Adja meg, amelynek meg kell őrizni hello adatok napok hello számát.</span><span class="sxs-lookup"><span data-stu-id="4408e-129">Specify hello number of days for which hello data must be retained.</span></span> <span data-ttu-id="4408e-130">Megőrzési csak akkor alkalmazható, ha az Azure storage-fiók tooarchive naplóadatokat használ.</span><span class="sxs-lookup"><span data-stu-id="4408e-130">Retention is only applicable if you are using Azure storage account tooarchive log data.</span></span>
   * <span data-ttu-id="4408e-131">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="4408e-131">Click **Save**.</span></span>

<span data-ttu-id="4408e-132">Miután engedélyezte a diagnosztikai beállítások, figyelheti az hello bejelentkezik hello **diagnosztikai naplók** fülre.</span><span class="sxs-lookup"><span data-stu-id="4408e-132">Once you have enabled diagnostic settings, you can watch hello logs in hello **Diagnostic Logs** tab.</span></span>

## <a name="view-diagnostic-logs-for-your-data-lake-store-account"></a><span data-ttu-id="4408e-133">A Data Lake Store-fiók diagnosztikai naplók megtekintése</span><span class="sxs-lookup"><span data-stu-id="4408e-133">View diagnostic logs for your Data Lake Store account</span></span>
<span data-ttu-id="4408e-134">Két módon tooview hello adatainak naplózása a Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="4408e-134">There are two ways tooview hello log data for your Data Lake Store account.</span></span>

* <span data-ttu-id="4408e-135">A Data Lake Store-fiók hello beállítások megtekintése</span><span class="sxs-lookup"><span data-stu-id="4408e-135">From hello Data Lake Store account settings view</span></span>
* <span data-ttu-id="4408e-136">A hello hello adatokat tároló Azure Storage-fiókban</span><span class="sxs-lookup"><span data-stu-id="4408e-136">From hello Azure Storage account where hello data is stored</span></span>

### <a name="using-hello-data-lake-store-settings-view"></a><span data-ttu-id="4408e-137">Data Lake Store beállítások megtekintése hello használata</span><span class="sxs-lookup"><span data-stu-id="4408e-137">Using hello Data Lake Store Settings view</span></span>
1. <span data-ttu-id="4408e-138">A Data Lake Store-fiókból **beállítások** panelen kattintson a **diagnosztikai naplók**.</span><span class="sxs-lookup"><span data-stu-id="4408e-138">From your Data Lake Store account **Settings** blade, click **Diagnostic Logs**.</span></span>
   
    <span data-ttu-id="4408e-139">![Diagnosztikai naplózás nézet](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs.png "diagnosztikai naplók megtekintése")</span><span class="sxs-lookup"><span data-stu-id="4408e-139">![View diagnostic logging](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs.png "View diagnostic logs")</span></span> 
2. <span data-ttu-id="4408e-140">A hello **diagnosztikai naplók** panelen megtekintheti az kategorizálta hello naplók **naplók** és **kérelem naplók**.</span><span class="sxs-lookup"><span data-stu-id="4408e-140">In hello **Diagnostic Logs** blade, you should see hello logs categorized by **Audit Logs** and **Request Logs**.</span></span>
   
   * <span data-ttu-id="4408e-141">Kérelem naplók rögzítése a Data Lake Store-fiók hello minden API kérelmet.</span><span class="sxs-lookup"><span data-stu-id="4408e-141">Request logs capture every API request made on hello Data Lake Store account.</span></span>
   * <span data-ttu-id="4408e-142">Naplók hasonló toorequest naplókat azonban sokkal részletesebben bontást hello műveletek végrehajtása hello Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="4408e-142">Audit Logs are similar toorequest Logs but provide a much more detailed breakdown of hello operations being performed on hello Data Lake Store account.</span></span> <span data-ttu-id="4408e-143">Például egy egyetlen feltöltés API-hívás a kérelem naplókban hello naplók több "Append" műveletei eredményezheti.</span><span class="sxs-lookup"><span data-stu-id="4408e-143">For example, a single upload API call in request logs might result in multiple "Append" operations in hello audit logs.</span></span>
3. <span data-ttu-id="4408e-144">Kattintson a hello **letöltése** egyes hivatkozás jelentkezzen bejegyzés toodownload hello naplókat.</span><span class="sxs-lookup"><span data-stu-id="4408e-144">Click hello **Download** link against each log entry toodownload hello logs.</span></span>

### <a name="from-hello-azure-storage-account-that-contains-log-data"></a><span data-ttu-id="4408e-145">Az Azure Storage-fiók hello naplóadatokat tartalmazó</span><span class="sxs-lookup"><span data-stu-id="4408e-145">From hello Azure Storage account that contains log data</span></span>
1. <span data-ttu-id="4408e-146">Nyissa meg a Data Lake Store társított naplózás hello Azure Storage-fiók panelen, és kattintson a Blobok.</span><span class="sxs-lookup"><span data-stu-id="4408e-146">Open hello Azure Storage account blade associated with Data Lake Store for logging, and then click Blobs.</span></span> <span data-ttu-id="4408e-147">Hello **Blob szolgáltatás** panel két tárolók listája.</span><span class="sxs-lookup"><span data-stu-id="4408e-147">hello **Blob service** blade lists two containers.</span></span>
   
    <span data-ttu-id="4408e-148">![Diagnosztikai naplózás nézet](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account.png "diagnosztikai naplók megtekintése")</span><span class="sxs-lookup"><span data-stu-id="4408e-148">![View diagnostic logging](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account.png "View diagnostic logs")</span></span>
   
   * <span data-ttu-id="4408e-149">hello tároló **insights-logs-naplózási** hello naplók tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="4408e-149">hello container **insights-logs-audit** contains hello audit logs.</span></span>
   * <span data-ttu-id="4408e-150">hello tároló **insights-logs-kérelmek** hello kérelmek naplóit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="4408e-150">hello container **insights-logs-requests** contains hello request logs.</span></span>
2. <span data-ttu-id="4408e-151">Ezek a tárolók belül hello struktúra a következő tárolt hello naplókat.</span><span class="sxs-lookup"><span data-stu-id="4408e-151">Within these containers, hello logs are stored under hello following structure.</span></span>
   
    <span data-ttu-id="4408e-152">![Diagnosztikai naplózás nézet](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account-structure.png "diagnosztikai naplók megtekintése")</span><span class="sxs-lookup"><span data-stu-id="4408e-152">![View diagnostic logging](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account-structure.png "View diagnostic logs")</span></span>
   
    <span data-ttu-id="4408e-153">Tegyük fel a teljes elérési tooan napló hello lehet`https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=04/m=00/PT1H.json`</span><span class="sxs-lookup"><span data-stu-id="4408e-153">As an example, hello complete path tooan audit log could be `https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=04/m=00/PT1H.json`</span></span>
   
    <span data-ttu-id="4408e-154">Similary, teljes elérési útja tooa napló hello lehet`https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=14/m=00/PT1H.json`</span><span class="sxs-lookup"><span data-stu-id="4408e-154">Similary, hello complete path tooa request log could be `https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=14/m=00/PT1H.json`</span></span>

## <a name="understand-hello-structure-of-hello-log-data"></a><span data-ttu-id="4408e-155">Hello naplóadatokat hello szerkezete ismertetése</span><span class="sxs-lookup"><span data-stu-id="4408e-155">Understand hello structure of hello log data</span></span>
<span data-ttu-id="4408e-156">hello naplózási és kérelem feldolgozásra JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="4408e-156">hello audit and request logs are in a JSON format.</span></span> <span data-ttu-id="4408e-157">Ez a szakasz azt hello szerkezete JSON kérelem tekintse meg és a naplók.</span><span class="sxs-lookup"><span data-stu-id="4408e-157">In this section, we look at hello structure of JSON for request and audit logs.</span></span>

### <a name="request-logs"></a><span data-ttu-id="4408e-158">Naplók kérése</span><span class="sxs-lookup"><span data-stu-id="4408e-158">Request logs</span></span>
<span data-ttu-id="4408e-159">Íme egy minta bejegyzés hello JSON-formátumú kérelem naplóban.</span><span class="sxs-lookup"><span data-stu-id="4408e-159">Here's a sample entry in hello JSON-formatted request log.</span></span> <span data-ttu-id="4408e-160">Minden egyes blob tartozik egy legfelső szintű objektum nevű **rekordok** , amely tartalmazza a napló objektumokból álló tömb.</span><span class="sxs-lookup"><span data-stu-id="4408e-160">Each blob has one root object called **records** that contains an array of log objects.</span></span>

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

#### <a name="request-log-schema"></a><span data-ttu-id="4408e-161">Kérelem séma</span><span class="sxs-lookup"><span data-stu-id="4408e-161">Request log schema</span></span>
| <span data-ttu-id="4408e-162">Név</span><span class="sxs-lookup"><span data-stu-id="4408e-162">Name</span></span> | <span data-ttu-id="4408e-163">Típus</span><span class="sxs-lookup"><span data-stu-id="4408e-163">Type</span></span> | <span data-ttu-id="4408e-164">Leírás</span><span class="sxs-lookup"><span data-stu-id="4408e-164">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="4408e-165">time</span><span class="sxs-lookup"><span data-stu-id="4408e-165">time</span></span> |<span data-ttu-id="4408e-166">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4408e-166">String</span></span> |<span data-ttu-id="4408e-167">hello időbélyegzőnek (UTC) hello napló</span><span class="sxs-lookup"><span data-stu-id="4408e-167">hello timestamp (in UTC) of hello log</span></span> |
| <span data-ttu-id="4408e-168">resourceId</span><span class="sxs-lookup"><span data-stu-id="4408e-168">resourceId</span></span> |<span data-ttu-id="4408e-169">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4408e-169">String</span></span> |<span data-ttu-id="4408e-170">Helyezze el, hogy a művelet hello erőforrás hello azonosítója</span><span class="sxs-lookup"><span data-stu-id="4408e-170">hello ID of hello resource that operation took place on</span></span> |
| <span data-ttu-id="4408e-171">category</span><span class="sxs-lookup"><span data-stu-id="4408e-171">category</span></span> |<span data-ttu-id="4408e-172">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4408e-172">String</span></span> |<span data-ttu-id="4408e-173">hello napló kategóriát.</span><span class="sxs-lookup"><span data-stu-id="4408e-173">hello log category.</span></span> <span data-ttu-id="4408e-174">Például **kérelmek**.</span><span class="sxs-lookup"><span data-stu-id="4408e-174">For example, **Requests**.</span></span> |
| <span data-ttu-id="4408e-175">operationName</span><span class="sxs-lookup"><span data-stu-id="4408e-175">operationName</span></span> |<span data-ttu-id="4408e-176">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4408e-176">String</span></span> |<span data-ttu-id="4408e-177">Bejelentkezve hello művelet neve.</span><span class="sxs-lookup"><span data-stu-id="4408e-177">Name of hello operation that is logged.</span></span> <span data-ttu-id="4408e-178">Például getfilestatus.</span><span class="sxs-lookup"><span data-stu-id="4408e-178">For example, getfilestatus.</span></span> |
| <span data-ttu-id="4408e-179">resultType</span><span class="sxs-lookup"><span data-stu-id="4408e-179">resultType</span></span> |<span data-ttu-id="4408e-180">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4408e-180">String</span></span> |<span data-ttu-id="4408e-181">hello művelet, például 200 hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="4408e-181">hello status of hello operation, For example, 200.</span></span> |
| <span data-ttu-id="4408e-182">callerIpAddress</span><span class="sxs-lookup"><span data-stu-id="4408e-182">callerIpAddress</span></span> |<span data-ttu-id="4408e-183">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4408e-183">String</span></span> |<span data-ttu-id="4408e-184">hello kérés hello ügyfél hello IP-címe</span><span class="sxs-lookup"><span data-stu-id="4408e-184">hello IP address of hello client making hello request</span></span> |
| <span data-ttu-id="4408e-185">correlationId</span><span class="sxs-lookup"><span data-stu-id="4408e-185">correlationId</span></span> |<span data-ttu-id="4408e-186">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4408e-186">String</span></span> |<span data-ttu-id="4408e-187">hello azonosítója, amelyet használhat toogroup egymáshoz kapcsolódó naplóbejegyzések készlete hello napló</span><span class="sxs-lookup"><span data-stu-id="4408e-187">hello id of hello log that can used toogroup together a set of related log entries</span></span> |
| <span data-ttu-id="4408e-188">identity</span><span class="sxs-lookup"><span data-stu-id="4408e-188">identity</span></span> |<span data-ttu-id="4408e-189">Objektum</span><span class="sxs-lookup"><span data-stu-id="4408e-189">Object</span></span> |<span data-ttu-id="4408e-190">hello napló okozó hello identitás</span><span class="sxs-lookup"><span data-stu-id="4408e-190">hello identity that generated hello log</span></span> |
| <span data-ttu-id="4408e-191">properties</span><span class="sxs-lookup"><span data-stu-id="4408e-191">properties</span></span> |<span data-ttu-id="4408e-192">JSON</span><span class="sxs-lookup"><span data-stu-id="4408e-192">JSON</span></span> |<span data-ttu-id="4408e-193">További információ alább olvasható</span><span class="sxs-lookup"><span data-stu-id="4408e-193">See below for details</span></span> |

#### <a name="request-log-properties-schema"></a><span data-ttu-id="4408e-194">Kérelem tulajdonságok séma</span><span class="sxs-lookup"><span data-stu-id="4408e-194">Request log properties schema</span></span>
| <span data-ttu-id="4408e-195">Név</span><span class="sxs-lookup"><span data-stu-id="4408e-195">Name</span></span> | <span data-ttu-id="4408e-196">Típus</span><span class="sxs-lookup"><span data-stu-id="4408e-196">Type</span></span> | <span data-ttu-id="4408e-197">Leírás</span><span class="sxs-lookup"><span data-stu-id="4408e-197">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="4408e-198">HttpMethod</span><span class="sxs-lookup"><span data-stu-id="4408e-198">HttpMethod</span></span> |<span data-ttu-id="4408e-199">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4408e-199">String</span></span> |<span data-ttu-id="4408e-200">hello HTTP-metódus használt hello a művelethez.</span><span class="sxs-lookup"><span data-stu-id="4408e-200">hello HTTP Method used for hello operation.</span></span> <span data-ttu-id="4408e-201">Például beolvasása.</span><span class="sxs-lookup"><span data-stu-id="4408e-201">For example, GET.</span></span> |
| <span data-ttu-id="4408e-202">Elérési út</span><span class="sxs-lookup"><span data-stu-id="4408e-202">Path</span></span> |<span data-ttu-id="4408e-203">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4408e-203">String</span></span> |<span data-ttu-id="4408e-204">hello elérési hello művelet végrehajtásának ideje</span><span class="sxs-lookup"><span data-stu-id="4408e-204">hello path hello operation was performed on</span></span> |
| <span data-ttu-id="4408e-205">RequestContentLength</span><span class="sxs-lookup"><span data-stu-id="4408e-205">RequestContentLength</span></span> |<span data-ttu-id="4408e-206">int</span><span class="sxs-lookup"><span data-stu-id="4408e-206">int</span></span> |<span data-ttu-id="4408e-207">hello tartalom hossza hello HTTP-kérelem</span><span class="sxs-lookup"><span data-stu-id="4408e-207">hello content length of hello HTTP request</span></span> |
| <span data-ttu-id="4408e-208">clientRequestId</span><span class="sxs-lookup"><span data-stu-id="4408e-208">ClientRequestId</span></span> |<span data-ttu-id="4408e-209">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4408e-209">String</span></span> |<span data-ttu-id="4408e-210">hello azonosítója, amely egyedileg azonosítja az ehhez a kérelemhez</span><span class="sxs-lookup"><span data-stu-id="4408e-210">hello Id that uniquely identifies this request</span></span> |
| <span data-ttu-id="4408e-211">Kezdő időpont</span><span class="sxs-lookup"><span data-stu-id="4408e-211">StartTime</span></span> |<span data-ttu-id="4408e-212">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4408e-212">String</span></span> |<span data-ttu-id="4408e-213">mely hello kiszolgálótól kapott hello kérésére hello idő</span><span class="sxs-lookup"><span data-stu-id="4408e-213">hello time at which hello server received hello request</span></span> |
| <span data-ttu-id="4408e-214">Befejezés időpontja</span><span class="sxs-lookup"><span data-stu-id="4408e-214">EndTime</span></span> |<span data-ttu-id="4408e-215">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4408e-215">String</span></span> |<span data-ttu-id="4408e-216">mely hello a kiszolgáló által küldött választ hello idő</span><span class="sxs-lookup"><span data-stu-id="4408e-216">hello time at which hello server sent a response</span></span> |

### <a name="audit-logs"></a><span data-ttu-id="4408e-217">Naplók</span><span class="sxs-lookup"><span data-stu-id="4408e-217">Audit logs</span></span>
<span data-ttu-id="4408e-218">Íme egy minta bejegyzés hello JSON-formátumú naplóban.</span><span class="sxs-lookup"><span data-stu-id="4408e-218">Here's a sample entry in hello JSON-formatted audit log.</span></span> <span data-ttu-id="4408e-219">Minden egyes blob tartozik egy legfelső szintű objektum nevű **rekordok** napló objektumok tömbjét tartalmazza, amelyek</span><span class="sxs-lookup"><span data-stu-id="4408e-219">Each blob has one root object called **records** that contains an array of log objects</span></span>

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

#### <a name="audit-log-schema"></a><span data-ttu-id="4408e-220">Naplózási séma</span><span class="sxs-lookup"><span data-stu-id="4408e-220">Audit log schema</span></span>
| <span data-ttu-id="4408e-221">Név</span><span class="sxs-lookup"><span data-stu-id="4408e-221">Name</span></span> | <span data-ttu-id="4408e-222">Típus</span><span class="sxs-lookup"><span data-stu-id="4408e-222">Type</span></span> | <span data-ttu-id="4408e-223">Leírás</span><span class="sxs-lookup"><span data-stu-id="4408e-223">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="4408e-224">time</span><span class="sxs-lookup"><span data-stu-id="4408e-224">time</span></span> |<span data-ttu-id="4408e-225">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4408e-225">String</span></span> |<span data-ttu-id="4408e-226">hello időbélyegzőnek (UTC) hello napló</span><span class="sxs-lookup"><span data-stu-id="4408e-226">hello timestamp (in UTC) of hello log</span></span> |
| <span data-ttu-id="4408e-227">resourceId</span><span class="sxs-lookup"><span data-stu-id="4408e-227">resourceId</span></span> |<span data-ttu-id="4408e-228">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4408e-228">String</span></span> |<span data-ttu-id="4408e-229">Helyezze el, hogy a művelet hello erőforrás hello azonosítója</span><span class="sxs-lookup"><span data-stu-id="4408e-229">hello ID of hello resource that operation took place on</span></span> |
| <span data-ttu-id="4408e-230">category</span><span class="sxs-lookup"><span data-stu-id="4408e-230">category</span></span> |<span data-ttu-id="4408e-231">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4408e-231">String</span></span> |<span data-ttu-id="4408e-232">hello napló kategóriát.</span><span class="sxs-lookup"><span data-stu-id="4408e-232">hello log category.</span></span> <span data-ttu-id="4408e-233">Például **naplózási**.</span><span class="sxs-lookup"><span data-stu-id="4408e-233">For example, **Audit**.</span></span> |
| <span data-ttu-id="4408e-234">operationName</span><span class="sxs-lookup"><span data-stu-id="4408e-234">operationName</span></span> |<span data-ttu-id="4408e-235">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4408e-235">String</span></span> |<span data-ttu-id="4408e-236">Bejelentkezve hello művelet neve.</span><span class="sxs-lookup"><span data-stu-id="4408e-236">Name of hello operation that is logged.</span></span> <span data-ttu-id="4408e-237">Például getfilestatus.</span><span class="sxs-lookup"><span data-stu-id="4408e-237">For example, getfilestatus.</span></span> |
| <span data-ttu-id="4408e-238">resultType</span><span class="sxs-lookup"><span data-stu-id="4408e-238">resultType</span></span> |<span data-ttu-id="4408e-239">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4408e-239">String</span></span> |<span data-ttu-id="4408e-240">hello művelet, például 200 hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="4408e-240">hello status of hello operation, For example, 200.</span></span> |
| <span data-ttu-id="4408e-241">correlationId</span><span class="sxs-lookup"><span data-stu-id="4408e-241">correlationId</span></span> |<span data-ttu-id="4408e-242">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4408e-242">String</span></span> |<span data-ttu-id="4408e-243">hello azonosítója, amelyet használhat toogroup egymáshoz kapcsolódó naplóbejegyzések készlete hello napló</span><span class="sxs-lookup"><span data-stu-id="4408e-243">hello id of hello log that can used toogroup together a set of related log entries</span></span> |
| <span data-ttu-id="4408e-244">identity</span><span class="sxs-lookup"><span data-stu-id="4408e-244">identity</span></span> |<span data-ttu-id="4408e-245">Objektum</span><span class="sxs-lookup"><span data-stu-id="4408e-245">Object</span></span> |<span data-ttu-id="4408e-246">hello napló okozó hello identitás</span><span class="sxs-lookup"><span data-stu-id="4408e-246">hello identity that generated hello log</span></span> |
| <span data-ttu-id="4408e-247">properties</span><span class="sxs-lookup"><span data-stu-id="4408e-247">properties</span></span> |<span data-ttu-id="4408e-248">JSON</span><span class="sxs-lookup"><span data-stu-id="4408e-248">JSON</span></span> |<span data-ttu-id="4408e-249">További információ alább olvasható</span><span class="sxs-lookup"><span data-stu-id="4408e-249">See below for details</span></span> |

#### <a name="audit-log-properties-schema"></a><span data-ttu-id="4408e-250">Naplózási tulajdonságai séma</span><span class="sxs-lookup"><span data-stu-id="4408e-250">Audit log properties schema</span></span>
| <span data-ttu-id="4408e-251">Név</span><span class="sxs-lookup"><span data-stu-id="4408e-251">Name</span></span> | <span data-ttu-id="4408e-252">Típus</span><span class="sxs-lookup"><span data-stu-id="4408e-252">Type</span></span> | <span data-ttu-id="4408e-253">Leírás</span><span class="sxs-lookup"><span data-stu-id="4408e-253">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="4408e-254">StreamName</span><span class="sxs-lookup"><span data-stu-id="4408e-254">StreamName</span></span> |<span data-ttu-id="4408e-255">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4408e-255">String</span></span> |<span data-ttu-id="4408e-256">hello elérési hello művelet végrehajtásának ideje</span><span class="sxs-lookup"><span data-stu-id="4408e-256">hello path hello operation was performed on</span></span> |

## <a name="samples-tooprocess-hello-log-data"></a><span data-ttu-id="4408e-257">Minták tooprocess hello naplóadatok</span><span class="sxs-lookup"><span data-stu-id="4408e-257">Samples tooprocess hello log data</span></span>
<span data-ttu-id="4408e-258">Azure Data Lake Store biztosít egy minta tooprocess és elemezheti a hello naplóadatokat.</span><span class="sxs-lookup"><span data-stu-id="4408e-258">Azure Data Lake Store provides a sample on how tooprocess and analyze hello log data.</span></span> <span data-ttu-id="4408e-259">Hello minta a található [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample).</span><span class="sxs-lookup"><span data-stu-id="4408e-259">You can find hello sample at [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample).</span></span> 

## <a name="see-also"></a><span data-ttu-id="4408e-260">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="4408e-260">See also</span></span>
* [<span data-ttu-id="4408e-261">Az Azure Data Lake Store áttekintése</span><span class="sxs-lookup"><span data-stu-id="4408e-261">Overview of Azure Data Lake Store</span></span>](data-lake-store-overview.md)
* [<span data-ttu-id="4408e-262">Biztonságos adattárolás a Data Lake Store-ban</span><span class="sxs-lookup"><span data-stu-id="4408e-262">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)

