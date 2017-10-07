---
title: "Azure tevékenység aaaView naplózza toomonitor erőforrások |} Microsoft Docs"
description: "Használja a hello tevékenység naplók tooreview felhasználói műveletek és a hibák. Az Azure portál PowerShell, az Azure CLI és REST jeleníti meg."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: fcdb3125-13ce-4c3b-9087-f514c5e41e73
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: tomfitz
ms.openlocfilehash: 8430ed2a9c1dfe5f13423a55d358e590b0facb22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="view-activity-logs-tooaudit-actions-on-resources"></a><span data-ttu-id="b3b6f-104">Tevékenység megtekintése naplózza tooaudit műveleteket az egyes erőforrások</span><span class="sxs-lookup"><span data-stu-id="b3b6f-104">View activity logs tooaudit actions on resources</span></span>
<span data-ttu-id="b3b6f-105">Keresztül tevékenységi naplóit meghatározhatja:</span><span class="sxs-lookup"><span data-stu-id="b3b6f-105">Through activity logs, you can determine:</span></span>

* <span data-ttu-id="b3b6f-106">milyen műveleteket is végezhet a hello erőforrást az előfizetésében</span><span class="sxs-lookup"><span data-stu-id="b3b6f-106">what operations were taken on hello resources in your subscription</span></span>
* <span data-ttu-id="b3b6f-107">kik kezdeményeztek hello művelet (bár a háttérszolgáltatás által kezdeményezett műveletek nem adják vissza egy felhasználó hello hívó)</span><span class="sxs-lookup"><span data-stu-id="b3b6f-107">who initiated hello operation (although operations initiated by a backend service do not return a user as hello caller)</span></span>
* <span data-ttu-id="b3b6f-108">Ha a hello művelet történt</span><span class="sxs-lookup"><span data-stu-id="b3b6f-108">when hello operation occurred</span></span>
* <span data-ttu-id="b3b6f-109">hello művelet hello állapotát</span><span class="sxs-lookup"><span data-stu-id="b3b6f-109">hello status of hello operation</span></span>
* <span data-ttu-id="b3b6f-110">hello értékek, amelyek segíthetnek tulajdonságokat kutatás hello művelet</span><span class="sxs-lookup"><span data-stu-id="b3b6f-110">hello values of other properties that might help you research hello operation</span></span>

[!INCLUDE [resource-manager-audit-limitations](../../includes/resource-manager-audit-limitations.md)]

<span data-ttu-id="b3b6f-111">Adatok lekérését hello tevékenységi naplóit hello portálon, a PowerShell, az Azure parancssori felület, Insights REST API-t vagy [Insights .NET kódtár](https://www.nuget.org/packages/Microsoft.Azure.Insights/).</span><span class="sxs-lookup"><span data-stu-id="b3b6f-111">You can retrieve information from hello activity logs through hello portal, PowerShell, Azure CLI, Insights REST API, or [Insights .NET Library](https://www.nuget.org/packages/Microsoft.Azure.Insights/).</span></span>

## <a name="portal"></a><span data-ttu-id="b3b6f-112">Portál</span><span class="sxs-lookup"><span data-stu-id="b3b6f-112">Portal</span></span>
1. <span data-ttu-id="b3b6f-113">Válassza ki a tooview hello tevékenységi naplóit hello portálon keresztül **figyelő**.</span><span class="sxs-lookup"><span data-stu-id="b3b6f-113">tooview hello activity logs through hello portal, select **Monitor**.</span></span>
   
    ![Válassza ki a tevékenységi naplóit](./media/resource-group-audit/select-monitor.png)

   <span data-ttu-id="b3b6f-115">Vagy tooautomatically szűrő hello tevékenységnapló egy adott erőforrás vagy egy erőforráscsoport, jelölje be a **tevékenységnapló** adott erőforráspanelen.</span><span class="sxs-lookup"><span data-stu-id="b3b6f-115">Or, tooautomatically filter hello activity log for a particular resource or resource group, select **Activity log** from that resource blade.</span></span> <span data-ttu-id="b3b6f-116">Figyelje meg, hogy hello tevékenységnapló hello kiválasztott erőforrás automatikusan szűrve van.</span><span class="sxs-lookup"><span data-stu-id="b3b6f-116">Notice that hello activity log is automatically filtered by hello selected resource.</span></span>
   
    ![Szűrés erőforrás szerint](./media/resource-group-audit/filtered-by-resource.png)
2. <span data-ttu-id="b3b6f-118">A hello **tevékenységnapló** panelen megjelenik a legutóbbi műveletek összegzését.</span><span class="sxs-lookup"><span data-stu-id="b3b6f-118">In hello **Activity Log** blade, you see a summary of recent operations.</span></span>
   
    ![műveletek megjelenítéséhez](./media/resource-group-audit/audit-summary.png)
3. <span data-ttu-id="b3b6f-120">jelenik meg, műveletek száma toorestrict hello különböző feltételek kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="b3b6f-120">toorestrict hello number of operations displayed, select different conditions.</span></span> <span data-ttu-id="b3b6f-121">Például hello következő kép bemutatja hello **Timespan** és **esemény által kezdeményezett** mezők megváltozott tooview hello műveleteit egy adott felhasználó vagy alkalmazás hello az elmúlt hónapban.</span><span class="sxs-lookup"><span data-stu-id="b3b6f-121">For example, hello following image shows hello **Timespan** and **Event initiated by** fields changed tooview hello actions taken by a particular user or application for hello past month.</span></span> <span data-ttu-id="b3b6f-122">Válassza ki **alkalmaz** tooview hello a lekérdezés eredményét.</span><span class="sxs-lookup"><span data-stu-id="b3b6f-122">Select **Apply** tooview hello results of your query.</span></span>
   
    ![szűrő beállításainak megadása](./media/resource-group-audit/set-filter.png)

4. <span data-ttu-id="b3b6f-124">Ha később kell toorun hello lekérdezés, válassza ki a **mentése** és nevezze el hello lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="b3b6f-124">If you need toorun hello query again later, select **Save** and give hello query a name.</span></span>
   
    ![lekérdezés mentése](./media/resource-group-audit/save-query.png)
5. <span data-ttu-id="b3b6f-126">a lekérdezés futtatásához tooquickly, válasszon egyet hello beépített lekérdezések, például a sikertelen központi telepítéssel.</span><span class="sxs-lookup"><span data-stu-id="b3b6f-126">tooquickly run a query, you can select one of hello built-in queries, such as failed deployments.</span></span>

    ![Válassza ki a lekérdezés](./media/resource-group-audit/select-quick-query.png)

   <span data-ttu-id="b3b6f-128">hello kijelölt lekérdezés automatikusan beállítja a szükséges hello szűrőértékek.</span><span class="sxs-lookup"><span data-stu-id="b3b6f-128">hello selected query automatically sets hello required filter values.</span></span>

    ![telepítési hibák megtekintése](./media/resource-group-audit/view-failed-deployment.png)   

6. <span data-ttu-id="b3b6f-130">Válasszon ki egy hello műveletek toosee hello esemény összegzését.</span><span class="sxs-lookup"><span data-stu-id="b3b6f-130">Select one of hello operations toosee a summary of hello event.</span></span>

    ![nézet művelet](./media/resource-group-audit/view-operation.png)  

## <a name="powershell"></a><span data-ttu-id="b3b6f-132">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b3b6f-132">PowerShell</span></span>
1. <span data-ttu-id="b3b6f-133">tooretrieve naplóbejegyzések, futtassa a hello **Get-AzureRmLog** parancsot.</span><span class="sxs-lookup"><span data-stu-id="b3b6f-133">tooretrieve log entries, run hello **Get-AzureRmLog** command.</span></span> <span data-ttu-id="b3b6f-134">További paraméterek toofilter hello listáját bejegyzések tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="b3b6f-134">You provide additional parameters toofilter hello list of entries.</span></span> <span data-ttu-id="b3b6f-135">Ha nem ad meg egy kezdő és záró idő, az elmúlt egy órában hello bejegyzések megjeleníti.</span><span class="sxs-lookup"><span data-stu-id="b3b6f-135">If you do not specify a start and end time, entries for hello last hour are returned.</span></span> <span data-ttu-id="b3b6f-136">Például a tooretrieve hello művelet erőforráscsoport során hello elmúlt egy órában fusson:</span><span class="sxs-lookup"><span data-stu-id="b3b6f-136">For example, tooretrieve hello operations for a resource group during hello past hour run:</span></span>

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup
  ```
   
    <span data-ttu-id="b3b6f-137">hello a következő példa bemutatja, hogyan toouse hello tevékenységet naplózni tooresearch műveletek a megadott időtartam során.</span><span class="sxs-lookup"><span data-stu-id="b3b6f-137">hello following example shows how toouse hello activity log tooresearch operations taken during a specified time.</span></span> <span data-ttu-id="b3b6f-138">hello kezdő és befejező dátumok meg van adva a dátumformátum.</span><span class="sxs-lookup"><span data-stu-id="b3b6f-138">hello start and end dates are specified in a date format.</span></span>

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime 2015-08-28T06:00 -EndTime 2015-09-10T06:00
  ```

    <span data-ttu-id="b3b6f-139">Vagy dátum funkciók toospecify hello dátumtartományt, például a hello elmúlt 14 napban is használhat.</span><span class="sxs-lookup"><span data-stu-id="b3b6f-139">Or, you can use date functions toospecify hello date range, such as hello last 14 days.</span></span>
   
  ```powershell 
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14)
  ```

2. <span data-ttu-id="b3b6f-140">Attól függően, hogy a megadott hello kezdete hello parancsokat adhat vissza hello erőforráscsoport műveletek listája túl hosszú.</span><span class="sxs-lookup"><span data-stu-id="b3b6f-140">Depending on hello start time you specify, hello previous commands can return a long list of operations for hello resource group.</span></span> <span data-ttu-id="b3b6f-141">Szűrheti a hello eredményeit mi keres, adja meg a keresési feltételeket.</span><span class="sxs-lookup"><span data-stu-id="b3b6f-141">You can filter hello results for what you are looking for by providing search criteria.</span></span> <span data-ttu-id="b3b6f-142">Például ha tooresearch hogyan webalkalmazás le lett állítva, a következő parancs hello futtathatja:</span><span class="sxs-lookup"><span data-stu-id="b3b6f-142">For example, if you are trying tooresearch how a web app was stopped, you could run hello following command:</span></span>

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14) | Where-Object OperationName -eq Microsoft.Web/sites/stop/action
  ```

    <span data-ttu-id="b3b6f-143">Amely ehhez a példához mutatja, hogy egy leállítási műveletet végzett el someone@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="b3b6f-143">Which for this example shows that a stop action was performed by someone@contoso.com.</span></span> 

  ```powershell 
  Authorization     :
  Scope     : /subscriptions/xxxxx/resourcegroups/ExampleGroup/providers/Microsoft.Web/sites/ExampleSite
  Action    : Microsoft.Web/sites/stop/action
  Role      : Subscription Admin
  Condition :
  Caller            : someone@contoso.com
  CorrelationId     : 84beae59-92aa-4662-a6fc-b6fecc0ff8da
  EventSource       : Administrative
  EventTimestamp    : 8/28/2015 4:08:18 PM
  OperationName     : Microsoft.Web/sites/stop/action
  ResourceGroupName : ExampleGroup
  ResourceId        : /subscriptions/xxxxx/resourcegroups/ExampleGroup/providers/Microsoft.Web/sites/ExampleSite
  Status            : Succeeded
  SubscriptionId    : xxxxx
  SubStatus         : OK
  ```

3. <span data-ttu-id="b3b6f-144">Megtekintheti egy adott felhasználó, egy erőforráscsoport, amely már nem létezik még a hello műveleteit.</span><span class="sxs-lookup"><span data-stu-id="b3b6f-144">You can look up hello actions taken by a particular user, even for a resource group that no longer exists.</span></span>

  ```powershell 
  Get-AzureRmLog -ResourceGroup deletedgroup -StartTime (Get-Date).AddDays(-14) -Caller someone@contoso.com
  ```

4. <span data-ttu-id="b3b6f-145">A sikertelen műveleteket végezhet.</span><span class="sxs-lookup"><span data-stu-id="b3b6f-145">You can filter for failed operations.</span></span>

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -Status Failed
  ```

5. <span data-ttu-id="b3b6f-146">Egy hiba hello állapotüzenet az adott bejegyzés megtekintésével összpontosíthat.</span><span class="sxs-lookup"><span data-stu-id="b3b6f-146">You can focus on one error by looking at hello status message for that entry.</span></span>
   
        ((Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -DetailedOutput).Properties[1].Content["statusMessage"] | ConvertFrom-Json).error
   
    <span data-ttu-id="b3b6f-147">Amely adja vissza:</span><span class="sxs-lookup"><span data-stu-id="b3b6f-147">Which returns:</span></span>
   
        code           message                                                                        
        ----           -------                                                                        
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. 


## <a name="azure-cli"></a><span data-ttu-id="b3b6f-148">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b3b6f-148">Azure CLI</span></span>
* <span data-ttu-id="b3b6f-149">hello futtatja tooretrieve naplóbejegyzések, **azure-csoportok napló megjelenítése** parancsot.</span><span class="sxs-lookup"><span data-stu-id="b3b6f-149">tooretrieve log entries, you run hello **azure group log show** command.</span></span>

  ```azurecli
  azure group log show ExampleGroup --json
  ```


## <a name="rest-api"></a><span data-ttu-id="b3b6f-150">REST API</span><span class="sxs-lookup"><span data-stu-id="b3b6f-150">REST API</span></span>
<span data-ttu-id="b3b6f-151">hello REST műveleteinek használata hello tevékenységnapló hello részét képező [Insights REST API](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span><span class="sxs-lookup"><span data-stu-id="b3b6f-151">hello REST operations for working with hello activity log are part of hello [Insights REST API](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span></span> <span data-ttu-id="b3b6f-152">napló tooretrieve tevékenységesemények, lásd: [hello felügyeleti események egy előfizetésben listában](https://msdn.microsoft.com/library/azure/dn931934.aspx).</span><span class="sxs-lookup"><span data-stu-id="b3b6f-152">tooretrieve activity log events, see [List hello management events in a subscription](https://msdn.microsoft.com/library/azure/dn931934.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b3b6f-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b3b6f-153">Next steps</span></span>
* <span data-ttu-id="b3b6f-154">Azure tevékenységi naplóit is használható a Power BI toogain nagyobb mélységben hello műveletekről az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="b3b6f-154">Azure Activity logs can be used with Power BI toogain greater insights about hello actions in your subscription.</span></span> <span data-ttu-id="b3b6f-155">Lásd: [megtekintése és elemzése a Power bi-ban és több Azure tevékenységi naplóit](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/).</span><span class="sxs-lookup"><span data-stu-id="b3b6f-155">See [View and analyze Azure Activity Logs in Power BI and more](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/).</span></span>
* <span data-ttu-id="b3b6f-156">Tekintse meg a biztonsági házirendek beállításával kapcsolatos toolearn [Azure szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="b3b6f-156">toolearn about setting security policies, see [Azure Role-based Access Control](../active-directory/role-based-access-control-configure.md).</span></span>
* <span data-ttu-id="b3b6f-157">toolearn telepítési műveleteit, megtekintésre hello parancsokkal kapcsolatban lásd: [üzembe helyezési műveleteinek megtekintése](resource-manager-deployment-operations.md).</span><span class="sxs-lookup"><span data-stu-id="b3b6f-157">toolearn about hello commands for viewing deployment operations, see [View deployment operations](resource-manager-deployment-operations.md).</span></span>
* <span data-ttu-id="b3b6f-158">Hogyan tooprevent törlése az összes felhasználó számára, erőforrás: toolearn [erőforrások az Azure Resource Manager zárolása](resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="b3b6f-158">toolearn how tooprevent deletions on a resource for all users, see [Lock resources with Azure Resource Manager](resource-group-lock-resources.md).</span></span>

