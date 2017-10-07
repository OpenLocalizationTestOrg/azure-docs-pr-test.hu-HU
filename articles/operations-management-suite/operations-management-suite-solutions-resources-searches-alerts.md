---
title: "aaaSaved keresés és a riasztások az OMS-megoldások |} Microsoft Docs"
description: "Az OMS megoldások rendszerint tartalmazza mentett keresések hello megoldás által összegyűjtött Naplóelemzési tooanalyze adatokban.  Akkor is definiálhat a riasztások toonotify hello felhasználói, vagy automatikusan választ tooa kritikus hiba a művelet végrehajtása.  Ez a cikk ismerteti, hogyan toodefine Naplóelemzési mentett keresések és riasztások egy ARM-sablon, azok megoldások tartalmazhat."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/24/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 93d7c5bbf061473833ca6c0a8e4d8e10d923f3ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="adding-log-analytics-saved-searches-and-alerts-toooms-management-solution-preview"></a><span data-ttu-id="adc8a-105">Naplóelemzési hozzáadása mentett keresések és riasztások tooOMS felügyeleti megoldás (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="adc8a-105">Adding Log Analytics saved searches and alerts tooOMS management solution (Preview)</span></span>

> [!NOTE]
> <span data-ttu-id="adc8a-106">Ez az előzetes dokumentum megoldások létrehozásához az OMS Szolgáltatáshoz, amely jelenleg előzetes verziójúak.</span><span class="sxs-lookup"><span data-stu-id="adc8a-106">This is preliminary documentation for creating management solutions in OMS which are currently in preview.</span></span> <span data-ttu-id="adc8a-107">Az alábbiakban semmilyen sémát tulajdonos toochange.</span><span class="sxs-lookup"><span data-stu-id="adc8a-107">Any schema described below is subject toochange.</span></span>   


<span data-ttu-id="adc8a-108">[Az OMS megoldások](operations-management-suite-solutions.md) rendszerint tartalmazza [mentett keresések](../log-analytics/log-analytics-log-searches.md) hello megoldás által összegyűjtött Naplóelemzési tooanalyze adatokban.</span><span class="sxs-lookup"><span data-stu-id="adc8a-108">[Management solutions in OMS](operations-management-suite-solutions.md) will typically include [saved searches](../log-analytics/log-analytics-log-searches.md) in Log Analytics tooanalyze data collected by hello solution.</span></span>  <span data-ttu-id="adc8a-109">Előfordulhat, hogy is definiálhat [riasztások](../log-analytics/log-analytics-alerts.md) toonotify felhasználói hello, vagy automatikusan választ tooa kritikus hiba a művelet végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="adc8a-109">They may also define [alerts](../log-analytics/log-analytics-alerts.md) toonotify hello user or automatically take action in response tooa critical issue.</span></span>  <span data-ttu-id="adc8a-110">Ez a cikk ismerteti, hogyan toodefine Naplóelemzési a mentett kereséseket a riasztások egy [erőforrás-kezelés sablon](../resource-manager-template-walkthrough.md) , azok tartalmazhat [megoldások](operations-management-suite-solutions-creating.md).</span><span class="sxs-lookup"><span data-stu-id="adc8a-110">This article describes how toodefine Log Analytics saved searches and alerts in a [Resource Management template](../resource-manager-template-walkthrough.md) so they can be included in [management solutions](operations-management-suite-solutions-creating.md).</span></span>

> [!NOTE]
> <span data-ttu-id="adc8a-111">hello minták ebben a cikkben használható paraméterek és változók vagy kötelező vagy közös toomanagement megoldások és ismertetett [létrehozása kezelési megoldásai Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)</span><span class="sxs-lookup"><span data-stu-id="adc8a-111">hello samples in this article use parameters and variables that are either required or common toomanagement solutions  and described in [Creating management solutions in Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="adc8a-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="adc8a-112">Prerequisites</span></span>
<span data-ttu-id="adc8a-113">Ez a cikk feltételezi, hogy most már tudja, hogyan túl[felügyeleti megoldás létrehozása](operations-management-suite-solutions-creating.md) és hello szerkezete egy [ARM-sablon](../resource-group-authoring-templates.md) és megoldásfájlt.</span><span class="sxs-lookup"><span data-stu-id="adc8a-113">This article assumes that you're already familiar with how too[create a management solution](operations-management-suite-solutions-creating.md) and hello structure of an [ARM template](../resource-group-authoring-templates.md) and solution file.</span></span>


## <a name="log-analytics-workspace"></a><span data-ttu-id="adc8a-114">A Naplóelemzési munkaterület</span><span class="sxs-lookup"><span data-stu-id="adc8a-114">Log Analytics Workspace</span></span>
<span data-ttu-id="adc8a-115">Összes erőforrása Naplóelemzési találhatók egy [munkaterület](../log-analytics/log-analytics-manage-access.md).</span><span class="sxs-lookup"><span data-stu-id="adc8a-115">All resources in Log Analytics are contained in a [workspace](../log-analytics/log-analytics-manage-access.md).</span></span>  <span data-ttu-id="adc8a-116">A [OMS munkaterületet, és az Automation-fiók](operations-management-suite-solutions.md#oms-workspace-and-automation-account) hello munkaterület nem tartalmazza a hello felügyeleti megoldás, de már léteznie kell hello megoldás telepítve van.</span><span class="sxs-lookup"><span data-stu-id="adc8a-116">As described in [OMS workspace and Automation account](operations-management-suite-solutions.md#oms-workspace-and-automation-account) hello workspace isn't included in hello management solution but must exist before hello solution is installed.</span></span>  <span data-ttu-id="adc8a-117">Ha nem érhető el, majd hello megoldás telepítése sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="adc8a-117">If it isn't available, then hello solution install will fail.</span></span>

<span data-ttu-id="adc8a-118">hello név hello munkaterület minden Naplóelemzési erőforrás hello neve van.</span><span class="sxs-lookup"><span data-stu-id="adc8a-118">hello name of hello workspace is in hello name of each Log Analytics resource.</span></span>  <span data-ttu-id="adc8a-119">Hello hello megoldást ezt **munkaterület** paraméter a következő példa egy savedsearch erőforrás hello hasonlóan.</span><span class="sxs-lookup"><span data-stu-id="adc8a-119">This is done in hello solution with hello **workspace** parameter as in hello following example of a savedsearch resource.</span></span>

    "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearchId'))]"


## <a name="saved-searches"></a><span data-ttu-id="adc8a-120">Mentett keresések</span><span class="sxs-lookup"><span data-stu-id="adc8a-120">Saved Searches</span></span>
<span data-ttu-id="adc8a-121">Tartalmaznak [mentett keresések](../log-analytics/log-analytics-log-searches.md) egy megoldás tooallow felhasználók tooquery által gyűjtött adatokat a megoldás a.</span><span class="sxs-lookup"><span data-stu-id="adc8a-121">Include [saved searches](../log-analytics/log-analytics-log-searches.md) in a solution tooallow users tooquery data collected by your solution.</span></span>  <span data-ttu-id="adc8a-122">Mentett keresések jelenik meg a **Kedvencek** az OMS-portálon hello és **mentett keresések** a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="adc8a-122">Saved searches will appear under **Favorites** in hello OMS portal and **Saved Searches** in hello Azure portal .</span></span>  <span data-ttu-id="adc8a-123">A mentett kereséseket is meg kell adni egy riasztás.</span><span class="sxs-lookup"><span data-stu-id="adc8a-123">A saved search is also required for each alert.</span></span>   

<span data-ttu-id="adc8a-124">[Naplóelemzési mentett keresés](../log-analytics/log-analytics-log-searches.md) erőforrások típusa lehet `Microsoft.OperationalInsights/workspaces/savedSearches` és a következő struktúra hello rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="adc8a-124">[Log Analytics saved search](../log-analytics/log-analytics-log-searches.md) resources have a type of `Microsoft.OperationalInsights/workspaces/savedSearches` and have hello following structure.</span></span>  <span data-ttu-id="adc8a-125">Ez magában foglalja közös változók és paramétereket, így másolja és illessze be a következő kódrészletet a megoldásfájlt a és módosítására használható hello paraméterek nevei.</span><span class="sxs-lookup"><span data-stu-id="adc8a-125">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change hello parameter names.</span></span> 

    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
        ],
        "tags": { },
        "properties": {
            "etag": "*",
            "query": "[variables('SavedSearch').Query]",
            "displayName": "[variables('SavedSearch').DisplayName]",
            "category": "[variables('SavedSearch').Category]"
        }
    }



<span data-ttu-id="adc8a-126">A mentett kereséseket hello tulajdonságainak mindegyikének hello a következő táblázat ismerteti.</span><span class="sxs-lookup"><span data-stu-id="adc8a-126">Each of hello properties of a saved search are described in hello following table.</span></span> 

| <span data-ttu-id="adc8a-127">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="adc8a-127">Property</span></span> | <span data-ttu-id="adc8a-128">Leírás</span><span class="sxs-lookup"><span data-stu-id="adc8a-128">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="adc8a-129">category</span><span class="sxs-lookup"><span data-stu-id="adc8a-129">category</span></span> | <span data-ttu-id="adc8a-130">hello hello kategóriákhoz mentett keresés.</span><span class="sxs-lookup"><span data-stu-id="adc8a-130">hello category for hello saved search.</span></span>  <span data-ttu-id="adc8a-131">A mentett keresések hello ugyanahhoz a megoldáshoz gyakran megosztja a egyetlen kategória, azok sorolhatók hello konzol.</span><span class="sxs-lookup"><span data-stu-id="adc8a-131">Any saved searches in hello same solution will often share a single category so they are grouped together in hello console.</span></span> |
| <span data-ttu-id="adc8a-132">DisplayName</span><span class="sxs-lookup"><span data-stu-id="adc8a-132">displayname</span></span> | <span data-ttu-id="adc8a-133">A hello neve toodisplay hello portálon mentett keresés.</span><span class="sxs-lookup"><span data-stu-id="adc8a-133">Name toodisplay for hello saved search in hello portal.</span></span> |
| <span data-ttu-id="adc8a-134">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="adc8a-134">query</span></span> | <span data-ttu-id="adc8a-135">Lekérdezés toorun.</span><span class="sxs-lookup"><span data-stu-id="adc8a-135">Query toorun.</span></span> |

> [!NOTE]
> <span data-ttu-id="adc8a-136">Ha a JSON-ként értelmezhetők karaktereket tartalmaz, akkor esetleg hello lekérdezés toouse escape karakter.</span><span class="sxs-lookup"><span data-stu-id="adc8a-136">You may need toouse escape characters in hello query if it includes characters that could be interpreted as JSON.</span></span>  <span data-ttu-id="adc8a-137">Például, ha a lekérdezés **típusa: AzureActivity OperationName:"Microsoft.Compute/virtualMachines/write"**, kell írható hello megoldás fájlt a **típusa: AzureActivity OperationName:\" Microsoft.Compute/virtualMachines/write\"**.</span><span class="sxs-lookup"><span data-stu-id="adc8a-137">For example, if your query was **Type:AzureActivity OperationName:"Microsoft.Compute/virtualMachines/write"**, it should be written in hello solution file as **Type:AzureActivity OperationName:\"Microsoft.Compute/virtualMachines/write\"**.</span></span>

## <a name="alerts"></a><span data-ttu-id="adc8a-138">Riasztások</span><span class="sxs-lookup"><span data-stu-id="adc8a-138">Alerts</span></span>
<span data-ttu-id="adc8a-139">[Naplófájl Analytics riasztások](../log-analytics/log-analytics-alerts.md) mentett keresést futtat rendszeres időközönkénti riasztási szabályok jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="adc8a-139">[Log Analytics alerts](../log-analytics/log-analytics-alerts.md) are created by alert rules that run a saved search on a regular interval.</span></span>  <span data-ttu-id="adc8a-140">Ha hello lekérdezési eredmények hello megfelel a megadott feltételeknek, egy riasztás rekord jön létre, és egy vagy több műveletek futnak.</span><span class="sxs-lookup"><span data-stu-id="adc8a-140">If hello results of hello query match specified criteria, an alert record is created and one or more actions are run.</span></span>  

<span data-ttu-id="adc8a-141">A riasztási szabályok megoldásra a következő három különböző erőforrások hello épülnek fel.</span><span class="sxs-lookup"><span data-stu-id="adc8a-141">Alert rules in a management solution are made up of hello following three different resources.</span></span>

- <span data-ttu-id="adc8a-142">**Mentett keresés.**</span><span class="sxs-lookup"><span data-stu-id="adc8a-142">**Saved search.**</span></span>  <span data-ttu-id="adc8a-143">Határozza meg a futtatni kívánt hello napló keresése.</span><span class="sxs-lookup"><span data-stu-id="adc8a-143">Defines hello log search that will be run.</span></span>  <span data-ttu-id="adc8a-144">Több riasztási szabályok megoszthat egy mentett keresés.</span><span class="sxs-lookup"><span data-stu-id="adc8a-144">Multiple alert rules can share a single saved search.</span></span>
- <span data-ttu-id="adc8a-145">**Ütemezés.**</span><span class="sxs-lookup"><span data-stu-id="adc8a-145">**Schedule.**</span></span>  <span data-ttu-id="adc8a-146">Határozza meg, milyen gyakran hello napló keresése fog futni.</span><span class="sxs-lookup"><span data-stu-id="adc8a-146">Defines how often hello log search will be run.</span></span>  <span data-ttu-id="adc8a-147">Minden riasztási szabályt egy ütemezés fog rendelkezni.</span><span class="sxs-lookup"><span data-stu-id="adc8a-147">Each alert rule will have one and only one schedule.</span></span>
- <span data-ttu-id="adc8a-148">**A műveletet.**</span><span class="sxs-lookup"><span data-stu-id="adc8a-148">**Alert action.**</span></span>  <span data-ttu-id="adc8a-149">Minden riasztási szabály lesz olyan típusú erőforrás a művelet **riasztás** , amely meghatározza, hogy hello riasztást egy riasztási rekord jön létre és riasztás súlyossági hello hello feltételek például hello részleteit.</span><span class="sxs-lookup"><span data-stu-id="adc8a-149">Each alert rule will have one action resource with a type of **Alert** that defines hello details of hello alert such as hello criteria for when an alert record will be created and hello alert's severity.</span></span>  <span data-ttu-id="adc8a-150">hello művelet erőforrás opcionálisan határozza meg az e-mail és runbook választ.</span><span class="sxs-lookup"><span data-stu-id="adc8a-150">hello action resource will optionally define a mail and runbook response.</span></span>
- <span data-ttu-id="adc8a-151">**Webhook művelet (nem kötelező).**</span><span class="sxs-lookup"><span data-stu-id="adc8a-151">**Webhook action (optional).**</span></span>  <span data-ttu-id="adc8a-152">Ha hello riasztási szabály fel fogja hívni a webhook, akkor azt igényli, hogy egy további művelet erőforrás típussal rendelkező **Webhook**.</span><span class="sxs-lookup"><span data-stu-id="adc8a-152">If hello alert rule will call a webhook, then it requires an additional action resource with a type of **Webhook**.</span></span>    

<span data-ttu-id="adc8a-153">Erőforrások fent ismertetett Keresés mentése.</span><span class="sxs-lookup"><span data-stu-id="adc8a-153">Saved search resources are described above.</span></span>  <span data-ttu-id="adc8a-154">hello további erőforrások az alábbiakban található.</span><span class="sxs-lookup"><span data-stu-id="adc8a-154">hello other resources are described below.</span></span>


### <a name="schedule-resource"></a><span data-ttu-id="adc8a-155">Ütemezés erőforrás</span><span class="sxs-lookup"><span data-stu-id="adc8a-155">Schedule resource</span></span>

<span data-ttu-id="adc8a-156">A mentett kereséseket rendelkezhet minden ütemezéssel jelző külön riasztási szabály egy vagy több ütemezés.</span><span class="sxs-lookup"><span data-stu-id="adc8a-156">A saved search can have one or more schedules with each schedule representing a separate alert rule.</span></span> <span data-ttu-id="adc8a-157">hello ütemezése határozza meg milyen gyakran hello keresési fut és hello időtartam, mely hello keresztül lekért adatmennyiséget.</span><span class="sxs-lookup"><span data-stu-id="adc8a-157">hello schedule defines how often hello search is run and hello time interval over which hello data is retrieved.</span></span>  <span data-ttu-id="adc8a-158">Ütemezés erőforrások típusa lehet `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/` és a következő struktúra hello rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="adc8a-158">Schedule resources have a type of `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/` and have hello following structure.</span></span> <span data-ttu-id="adc8a-159">Ez magában foglalja közös változók és paramétereket, így másolja és illessze be a következő kódrészletet a megoldásfájlt a és módosítására használható hello paraméterek nevei.</span><span class="sxs-lookup"><span data-stu-id="adc8a-159">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change hello parameter names.</span></span> 


    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name)]"
        ],
        "properties": {
            "etag": "*",
            "interval": "[variables('Schedule').Interval]",
            "queryTimeSpan": "[variables('Schedule').TimeSpan]",
            "enabled": "[variables('Schedule').Enabled]"
        }
    }



<span data-ttu-id="adc8a-160">a következő táblázat hello ütemezés erőforrás hello tulajdonságait ismerteti.</span><span class="sxs-lookup"><span data-stu-id="adc8a-160">hello properties for schedule resources are described in hello following table.</span></span>

| <span data-ttu-id="adc8a-161">Elem neve</span><span class="sxs-lookup"><span data-stu-id="adc8a-161">Element name</span></span> | <span data-ttu-id="adc8a-162">Szükséges</span><span class="sxs-lookup"><span data-stu-id="adc8a-162">Required</span></span> | <span data-ttu-id="adc8a-163">Leírás</span><span class="sxs-lookup"><span data-stu-id="adc8a-163">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="adc8a-164">engedélyezve</span><span class="sxs-lookup"><span data-stu-id="adc8a-164">enabled</span></span>       | <span data-ttu-id="adc8a-165">Igen</span><span class="sxs-lookup"><span data-stu-id="adc8a-165">Yes</span></span> | <span data-ttu-id="adc8a-166">Meghatározza, hogy az hello riasztás létrehozásakor engedélyezett-e.</span><span class="sxs-lookup"><span data-stu-id="adc8a-166">Specifies whether hello alert is enabled when it's created.</span></span> |
| <span data-ttu-id="adc8a-167">interval</span><span class="sxs-lookup"><span data-stu-id="adc8a-167">interval</span></span>      | <span data-ttu-id="adc8a-168">Igen</span><span class="sxs-lookup"><span data-stu-id="adc8a-168">Yes</span></span> | <span data-ttu-id="adc8a-169">Milyen gyakran hello lekérdezés fut perc múlva.</span><span class="sxs-lookup"><span data-stu-id="adc8a-169">How often hello query runs in minutes.</span></span> |
| <span data-ttu-id="adc8a-170">queryTimeSpan</span><span class="sxs-lookup"><span data-stu-id="adc8a-170">queryTimeSpan</span></span> | <span data-ttu-id="adc8a-171">Igen</span><span class="sxs-lookup"><span data-stu-id="adc8a-171">Yes</span></span> | <span data-ttu-id="adc8a-172">Idő (percben) keresztül mely tooevaluate eredmények hosszát.</span><span class="sxs-lookup"><span data-stu-id="adc8a-172">Length of time in minutes over which tooevaluate results.</span></span> |

<span data-ttu-id="adc8a-173">hello ütemezés erőforrás, mielőtt hello ütemezés létrehozása mentett keresés hello függ.</span><span class="sxs-lookup"><span data-stu-id="adc8a-173">hello schedule resource should depend on hello saved search so that it's created before hello schedule.</span></span>


### <a name="actions"></a><span data-ttu-id="adc8a-174">Műveletek</span><span class="sxs-lookup"><span data-stu-id="adc8a-174">Actions</span></span>
<span data-ttu-id="adc8a-175">Két különböző művelet erőforrás hello által megadott **típus** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="adc8a-175">There are two types of action resource specified by hello **Type** property.</span></span>  <span data-ttu-id="adc8a-176">Ütemezés szükséges **riasztás** hello riasztási szabályt, és milyen műveleteket hajtja végre, amikor létrejön egy riasztás hello részleteit meghatározó művelet.</span><span class="sxs-lookup"><span data-stu-id="adc8a-176">A schedule requires one **Alert** action which defines hello details of hello alert rule and what actions are taken when an alert is created.</span></span>  <span data-ttu-id="adc8a-177">Az is előfordulhat, hogy tartalmazza a **Webhook** művelet, ha egy webhook a hello riasztást kell megnyitni.</span><span class="sxs-lookup"><span data-stu-id="adc8a-177">It may also include a **Webhook** action if a webhook should be called from hello alert.</span></span>  

<span data-ttu-id="adc8a-178">Művelet erőforrások típusa lehet `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions`.</span><span class="sxs-lookup"><span data-stu-id="adc8a-178">Action resources have a type of `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions`.</span></span>  

#### <a name="alert-actions"></a><span data-ttu-id="adc8a-179">Riasztási műveletek</span><span class="sxs-lookup"><span data-stu-id="adc8a-179">Alert actions</span></span>

<span data-ttu-id="adc8a-180">Minden ütemezés kell egy **riasztási** művelet.</span><span class="sxs-lookup"><span data-stu-id="adc8a-180">Every schedule will have one **Alert** action.</span></span>  <span data-ttu-id="adc8a-181">Ez határozza meg a hello riasztást, és opcionálisan értesítési, a javítási műveletek hello részleteit.</span><span class="sxs-lookup"><span data-stu-id="adc8a-181">This defines hello details of hello alert and optionally notification and remediation actions.</span></span>  <span data-ttu-id="adc8a-182">Értesítést küld egy e-mailek tooone vagy több címet.</span><span class="sxs-lookup"><span data-stu-id="adc8a-182">A notification sends an email tooone or more addresses.</span></span>  <span data-ttu-id="adc8a-183">A szervizelés elindít egy Azure Automation tooattempt tooremediate hello észlelhető hiba.</span><span class="sxs-lookup"><span data-stu-id="adc8a-183">A remediation starts a runbook in Azure Automation tooattempt tooremediate hello detected issue.</span></span>

<span data-ttu-id="adc8a-184">Értesítési műveletek struktúra a következő hello rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="adc8a-184">Alert actions have hello following structure.</span></span>  <span data-ttu-id="adc8a-185">Ez magában foglalja közös változók és paramétereket, így másolja és illessze be a következő kódrészletet a megoldásfájlt a és módosítására használható hello paraméterek nevei.</span><span class="sxs-lookup"><span data-stu-id="adc8a-185">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change hello parameter names.</span></span> 



    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name, '/', variables('Alert').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name, '/schedules/', variables('Schedule').Name)]"
        ],
        "properties": {
            "etag": "*",
            "type": "Alert",
            "name": "[variables('Alert').Name]",
            "description": "[variables('Alert').Description]",
            "severity": "[variables('Alert').Severity]",
            "threshold": {
                "operator": "[variables('Alert').Threshold.Operator]",
                "value": "[variables('Alert').Threshold.Value]",
                "metricsTrigger": {
                    "triggerCondition": "[variables('Alert').Threshold.Trigger.Condition]",
                    "operator": "[variables('Alert').Trigger.Operator]",
                    "value": "[variables('Alert').Trigger.Value]"
                },
            },
            "emailNotification": {
                "recipients": [
                    "[variables('Alert').Recipients]"
                ],
                "subject": "[variables('Alert').Subject]"
            },
            "remediation": {
                "runbookName": "[variables('Alert').Remedition.RunbookName]",
                "webhookUri": "[variables('Alert').Remedition.WebhookUri]"
            }
        }
    }

<span data-ttu-id="adc8a-186">a következő táblák hello riasztási művelet erőforrás hello tulajdonságait ismerteti.</span><span class="sxs-lookup"><span data-stu-id="adc8a-186">hello properties for Alert action resources are described in hello following tables.</span></span>

| <span data-ttu-id="adc8a-187">Elem neve</span><span class="sxs-lookup"><span data-stu-id="adc8a-187">Element name</span></span> | <span data-ttu-id="adc8a-188">Szükséges</span><span class="sxs-lookup"><span data-stu-id="adc8a-188">Required</span></span> | <span data-ttu-id="adc8a-189">Leírás</span><span class="sxs-lookup"><span data-stu-id="adc8a-189">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="adc8a-190">Típus</span><span class="sxs-lookup"><span data-stu-id="adc8a-190">Type</span></span> | <span data-ttu-id="adc8a-191">Igen</span><span class="sxs-lookup"><span data-stu-id="adc8a-191">Yes</span></span> | <span data-ttu-id="adc8a-192">Hello művelet típusa.</span><span class="sxs-lookup"><span data-stu-id="adc8a-192">Type of hello action.</span></span>  <span data-ttu-id="adc8a-193">Ez lesz **riasztás** riasztási műveletekhez.</span><span class="sxs-lookup"><span data-stu-id="adc8a-193">This will be **Alert** for alert actions.</span></span> |
| <span data-ttu-id="adc8a-194">Név</span><span class="sxs-lookup"><span data-stu-id="adc8a-194">Name</span></span> | <span data-ttu-id="adc8a-195">Igen</span><span class="sxs-lookup"><span data-stu-id="adc8a-195">Yes</span></span> | <span data-ttu-id="adc8a-196">Hello riasztás megjelenített neve.</span><span class="sxs-lookup"><span data-stu-id="adc8a-196">Display name for hello alert.</span></span>  <span data-ttu-id="adc8a-197">Ez az hello hello riasztási szabály hello konzolon megjelenített neve.</span><span class="sxs-lookup"><span data-stu-id="adc8a-197">This is hello name that's displayed in hello console for hello alert rule.</span></span> |
| <span data-ttu-id="adc8a-198">Leírás</span><span class="sxs-lookup"><span data-stu-id="adc8a-198">Description</span></span> | <span data-ttu-id="adc8a-199">Nem</span><span class="sxs-lookup"><span data-stu-id="adc8a-199">No</span></span> | <span data-ttu-id="adc8a-200">Hello riasztás nem kötelező leírása.</span><span class="sxs-lookup"><span data-stu-id="adc8a-200">Optional description of hello alert.</span></span> |
| <span data-ttu-id="adc8a-201">Súlyosság</span><span class="sxs-lookup"><span data-stu-id="adc8a-201">Severity</span></span> | <span data-ttu-id="adc8a-202">Igen</span><span class="sxs-lookup"><span data-stu-id="adc8a-202">Yes</span></span> | <span data-ttu-id="adc8a-203">A következő értékek hello riasztási rekordját hello súlyossága:</span><span class="sxs-lookup"><span data-stu-id="adc8a-203">Severity of hello alert record from hello following values:</span></span><br><br> <span data-ttu-id="adc8a-204">**Kritikus**</span><span class="sxs-lookup"><span data-stu-id="adc8a-204">**Critical**</span></span><br><span data-ttu-id="adc8a-205">**Figyelmeztetés**</span><span class="sxs-lookup"><span data-stu-id="adc8a-205">**Warning**</span></span><br><span data-ttu-id="adc8a-206">**Tájékoztató**</span><span class="sxs-lookup"><span data-stu-id="adc8a-206">**Informational**</span></span> |


##### <a name="threshold"></a><span data-ttu-id="adc8a-207">Küszöbérték</span><span class="sxs-lookup"><span data-stu-id="adc8a-207">Threshold</span></span>
<span data-ttu-id="adc8a-208">Ez a szakasz meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="adc8a-208">This section is required.</span></span>  <span data-ttu-id="adc8a-209">Azt határozza meg a riasztási küszöbérték hello hello tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="adc8a-209">It defines hello properties for hello alert threshold.</span></span>

| <span data-ttu-id="adc8a-210">Elem neve</span><span class="sxs-lookup"><span data-stu-id="adc8a-210">Element name</span></span> | <span data-ttu-id="adc8a-211">Szükséges</span><span class="sxs-lookup"><span data-stu-id="adc8a-211">Required</span></span> | <span data-ttu-id="adc8a-212">Leírás</span><span class="sxs-lookup"><span data-stu-id="adc8a-212">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="adc8a-213">Operátor</span><span class="sxs-lookup"><span data-stu-id="adc8a-213">Operator</span></span> | <span data-ttu-id="adc8a-214">Igen</span><span class="sxs-lookup"><span data-stu-id="adc8a-214">Yes</span></span> | <span data-ttu-id="adc8a-215">A következő értékek hello hello összehasonlító operátort:</span><span class="sxs-lookup"><span data-stu-id="adc8a-215">Operator for hello comparison from hello following values:</span></span><br><br><span data-ttu-id="adc8a-216">**gt = nagyobb, mint<br>lt = kisebb, mint**</span><span class="sxs-lookup"><span data-stu-id="adc8a-216">**gt = greater than<br>lt = less than**</span></span> |
| <span data-ttu-id="adc8a-217">Érték</span><span class="sxs-lookup"><span data-stu-id="adc8a-217">Value</span></span> | <span data-ttu-id="adc8a-218">Igen</span><span class="sxs-lookup"><span data-stu-id="adc8a-218">Yes</span></span> | <span data-ttu-id="adc8a-219">hello érték toocompare hello eredmények.</span><span class="sxs-lookup"><span data-stu-id="adc8a-219">hello value toocompare hello results.</span></span> |


##### <a name="metricstrigger"></a><span data-ttu-id="adc8a-220">MetricsTrigger</span><span class="sxs-lookup"><span data-stu-id="adc8a-220">MetricsTrigger</span></span>
<span data-ttu-id="adc8a-221">Ez a szakasz nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="adc8a-221">This section is optional.</span></span>  <span data-ttu-id="adc8a-222">Adja hozzá a metrika mérési riasztás.</span><span class="sxs-lookup"><span data-stu-id="adc8a-222">Include it for a metric measurement alert.</span></span>

> [!NOTE]
> <span data-ttu-id="adc8a-223">Metrika mérési riasztások jelenleg nyilvános előzetes verziójához.</span><span class="sxs-lookup"><span data-stu-id="adc8a-223">Metric measurement alerts are currently in public preview.</span></span> 

| <span data-ttu-id="adc8a-224">Elem neve</span><span class="sxs-lookup"><span data-stu-id="adc8a-224">Element name</span></span> | <span data-ttu-id="adc8a-225">Szükséges</span><span class="sxs-lookup"><span data-stu-id="adc8a-225">Required</span></span> | <span data-ttu-id="adc8a-226">Leírás</span><span class="sxs-lookup"><span data-stu-id="adc8a-226">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="adc8a-227">TriggerCondition</span><span class="sxs-lookup"><span data-stu-id="adc8a-227">TriggerCondition</span></span> | <span data-ttu-id="adc8a-228">Igen</span><span class="sxs-lookup"><span data-stu-id="adc8a-228">Yes</span></span> | <span data-ttu-id="adc8a-229">Megadja, hogy hello küszöbérték megszegése vagy a következő értékek hello egymást követő megszegése teljes száma:</span><span class="sxs-lookup"><span data-stu-id="adc8a-229">Specifies whether hello threshold is for total number of breaches or consecutive breaches from hello following values:</span></span><br><br><span data-ttu-id="adc8a-230">**Teljes<br>egymást követő**</span><span class="sxs-lookup"><span data-stu-id="adc8a-230">**Total<br>Consecutive**</span></span> |
| <span data-ttu-id="adc8a-231">Operátor</span><span class="sxs-lookup"><span data-stu-id="adc8a-231">Operator</span></span> | <span data-ttu-id="adc8a-232">Igen</span><span class="sxs-lookup"><span data-stu-id="adc8a-232">Yes</span></span> | <span data-ttu-id="adc8a-233">A következő értékek hello hello összehasonlító operátort:</span><span class="sxs-lookup"><span data-stu-id="adc8a-233">Operator for hello comparison from hello following values:</span></span><br><br><span data-ttu-id="adc8a-234">**gt = nagyobb, mint<br>lt = kisebb, mint**</span><span class="sxs-lookup"><span data-stu-id="adc8a-234">**gt = greater than<br>lt = less than**</span></span> |
| <span data-ttu-id="adc8a-235">Érték</span><span class="sxs-lookup"><span data-stu-id="adc8a-235">Value</span></span> | <span data-ttu-id="adc8a-236">Igen</span><span class="sxs-lookup"><span data-stu-id="adc8a-236">Yes</span></span> | <span data-ttu-id="adc8a-237">Hello időpontokban hello feltételek számának fennállnak tootrigger hello riasztást kell lennie.</span><span class="sxs-lookup"><span data-stu-id="adc8a-237">Number of hello times hello criteria must be met tootrigger hello alert.</span></span> |

##### <a name="throttling"></a><span data-ttu-id="adc8a-238">Szabályozás</span><span class="sxs-lookup"><span data-stu-id="adc8a-238">Throttling</span></span>
<span data-ttu-id="adc8a-239">Ez a szakasz nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="adc8a-239">This section is optional.</span></span>  <span data-ttu-id="adc8a-240">Ez a szakasz tartalmazza, ha a kívánt toosuppress riasztások hello azonos szabály néhány időtartamig riasztás létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="adc8a-240">Include this section if you want toosuppress alerts from hello same rule for some amount of time after an alert is created.</span></span>

| <span data-ttu-id="adc8a-241">Elem neve</span><span class="sxs-lookup"><span data-stu-id="adc8a-241">Element name</span></span> | <span data-ttu-id="adc8a-242">Szükséges</span><span class="sxs-lookup"><span data-stu-id="adc8a-242">Required</span></span> | <span data-ttu-id="adc8a-243">Leírás</span><span class="sxs-lookup"><span data-stu-id="adc8a-243">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="adc8a-244">DurationInMinutes</span><span class="sxs-lookup"><span data-stu-id="adc8a-244">DurationInMinutes</span></span> | <span data-ttu-id="adc8a-245">Igen, ha a sávszélesség-szabályozás elemet tartalmazza</span><span class="sxs-lookup"><span data-stu-id="adc8a-245">Yes if Throttling element included</span></span> | <span data-ttu-id="adc8a-246">Perc toosuppress értesítések száma után hello közül ugyanazon riasztási szabály létrehozása zajlik.</span><span class="sxs-lookup"><span data-stu-id="adc8a-246">Number of minutes toosuppress alerts after one from hello same alert rule is created.</span></span> |

##### <a name="emailnotification"></a><span data-ttu-id="adc8a-247">EmailNotification</span><span class="sxs-lookup"><span data-stu-id="adc8a-247">EmailNotification</span></span>
 <span data-ttu-id="adc8a-248">Ez a szakasz nem kötelező, ha azt szeretné, hello riasztási toosend mail tooone vagy további címzettek Include.</span><span class="sxs-lookup"><span data-stu-id="adc8a-248">This section is optional  Include it if you want hello alert toosend mail tooone or more recipients.</span></span>

| <span data-ttu-id="adc8a-249">Elem neve</span><span class="sxs-lookup"><span data-stu-id="adc8a-249">Element name</span></span> | <span data-ttu-id="adc8a-250">Szükséges</span><span class="sxs-lookup"><span data-stu-id="adc8a-250">Required</span></span> | <span data-ttu-id="adc8a-251">Leírás</span><span class="sxs-lookup"><span data-stu-id="adc8a-251">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="adc8a-252">Címzettek</span><span class="sxs-lookup"><span data-stu-id="adc8a-252">Recipients</span></span> | <span data-ttu-id="adc8a-253">Igen</span><span class="sxs-lookup"><span data-stu-id="adc8a-253">Yes</span></span> | <span data-ttu-id="adc8a-254">Címek vesszővel tagolt listája az e-mailek toosend értesítést, ha riasztás jön létre, mint a következő példa hello.</span><span class="sxs-lookup"><span data-stu-id="adc8a-254">Comma delimited list of email addresses toosend notification when an alert is created such as in hello following example.</span></span><br><br><span data-ttu-id="adc8a-255">**[ "recipient1@contoso.com", "recipient2@contoso.com" ]**</span><span class="sxs-lookup"><span data-stu-id="adc8a-255">**[ "recipient1@contoso.com", "recipient2@contoso.com" ]**</span></span> |
| <span data-ttu-id="adc8a-256">Tárgy</span><span class="sxs-lookup"><span data-stu-id="adc8a-256">Subject</span></span> | <span data-ttu-id="adc8a-257">Igen</span><span class="sxs-lookup"><span data-stu-id="adc8a-257">Yes</span></span> | <span data-ttu-id="adc8a-258">Hello e-mail tárgysora.</span><span class="sxs-lookup"><span data-stu-id="adc8a-258">Subject line of hello mail.</span></span> |
| <span data-ttu-id="adc8a-259">Melléklet</span><span class="sxs-lookup"><span data-stu-id="adc8a-259">Attachment</span></span> | <span data-ttu-id="adc8a-260">Nem</span><span class="sxs-lookup"><span data-stu-id="adc8a-260">No</span></span> | <span data-ttu-id="adc8a-261">Jelenleg nem támogatja a mellékleteket.</span><span class="sxs-lookup"><span data-stu-id="adc8a-261">Attachments are not currently supported.</span></span>  <span data-ttu-id="adc8a-262">Ha ez az elem megtalálható, meg kell **nincs**.</span><span class="sxs-lookup"><span data-stu-id="adc8a-262">If this element is included, it should be **None**.</span></span> |


##### <a name="remediation"></a><span data-ttu-id="adc8a-263">Szervizkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="adc8a-263">Remediation</span></span>
<span data-ttu-id="adc8a-264">Ez a szakasz nem kötelező adja hozzá, ha azt szeretné, hogy egy runbook toostart válasz toohello riasztásban.</span><span class="sxs-lookup"><span data-stu-id="adc8a-264">This section is optional  Include it if you want a runbook toostart in response toohello alert.</span></span> |

| <span data-ttu-id="adc8a-265">Elem neve</span><span class="sxs-lookup"><span data-stu-id="adc8a-265">Element name</span></span> | <span data-ttu-id="adc8a-266">Szükséges</span><span class="sxs-lookup"><span data-stu-id="adc8a-266">Required</span></span> | <span data-ttu-id="adc8a-267">Leírás</span><span class="sxs-lookup"><span data-stu-id="adc8a-267">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="adc8a-268">RunbookName</span><span class="sxs-lookup"><span data-stu-id="adc8a-268">RunbookName</span></span> | <span data-ttu-id="adc8a-269">Igen</span><span class="sxs-lookup"><span data-stu-id="adc8a-269">Yes</span></span> | <span data-ttu-id="adc8a-270">Hello runbook toostart neve.</span><span class="sxs-lookup"><span data-stu-id="adc8a-270">Name of hello runbook toostart.</span></span> |
| <span data-ttu-id="adc8a-271">WebhookUri</span><span class="sxs-lookup"><span data-stu-id="adc8a-271">WebhookUri</span></span> | <span data-ttu-id="adc8a-272">Igen</span><span class="sxs-lookup"><span data-stu-id="adc8a-272">Yes</span></span> | <span data-ttu-id="adc8a-273">Hello runbook hello webhook URI azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="adc8a-273">Uri of hello webhook for hello runbook.</span></span> |
| <span data-ttu-id="adc8a-274">A lejárati</span><span class="sxs-lookup"><span data-stu-id="adc8a-274">Expiry</span></span> | <span data-ttu-id="adc8a-275">Nem</span><span class="sxs-lookup"><span data-stu-id="adc8a-275">No</span></span> | <span data-ttu-id="adc8a-276">Dátum és idő, a szervizelési hello lejár.</span><span class="sxs-lookup"><span data-stu-id="adc8a-276">Date and time that hello remediation expires.</span></span> |

#### <a name="webhook-actions"></a><span data-ttu-id="adc8a-277">Webhookműveletek</span><span class="sxs-lookup"><span data-stu-id="adc8a-277">Webhook actions</span></span>

<span data-ttu-id="adc8a-278">Webhookműveletek egy folyamat megkezdéséhez hívja az egy URL-cím és a nem kötelezően egy hasznos toobe küldött.</span><span class="sxs-lookup"><span data-stu-id="adc8a-278">Webhook actions start a process by calling a URL and optionally providing a payload toobe sent.</span></span> <span data-ttu-id="adc8a-279">Hasonló tooRemediation műveletek kivételével ezek célja a webhookokkal, előfordulhat, hogy aktiválják az Azure Automation-runbook eltérő folyamatok.</span><span class="sxs-lookup"><span data-stu-id="adc8a-279">They are similar tooRemediation actions except they are meant for webhooks that may invoke processes other than Azure Automation runbooks.</span></span> <span data-ttu-id="adc8a-280">Hello további lehetőség a tartalom céleszközökre toobe toohello távoli folyamattal is biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="adc8a-280">They also provide hello additional option of providing a payload toobe delivered toohello remote process.</span></span>

<span data-ttu-id="adc8a-281">Ha a riasztás fel fogja hívni a webhook, akkor el kell egy művelet erőforrás típussal rendelkező **Webhook** hozzáadása toohello a **riasztás** művelet erőforrás.</span><span class="sxs-lookup"><span data-stu-id="adc8a-281">If your alert will call a webhook, then it will need an action resource with a type of **Webhook** in addition toohello **Alert** action resource.</span></span>  

    {
      "name": "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name, '/', variables('Webhook').Name)]",
      "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions/",
      "apiVersion": "[variables('LogAnalyticsApiVersion')]",
      "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name, '/schedules/', variables('Schedule').Name)]"
      ],
      "properties": {
        "etag": "*",
        "type": "[variables('Alert').Webhook.Type]",
        "name": "[variables('Alert').Webhook.Name]",
        "webhookUri": "[variables('Alert').Webhook.webhookUri]",
        "customPayload": "[variables('Alert').Webhook.CustomPayLoad]"
      }
    }

<span data-ttu-id="adc8a-282">a következő táblák hello Webhook művelet erőforrás hello tulajdonságait ismerteti.</span><span class="sxs-lookup"><span data-stu-id="adc8a-282">hello properties for Webhook action resources are described in hello following tables.</span></span>

| <span data-ttu-id="adc8a-283">Elem neve</span><span class="sxs-lookup"><span data-stu-id="adc8a-283">Element name</span></span> | <span data-ttu-id="adc8a-284">Szükséges</span><span class="sxs-lookup"><span data-stu-id="adc8a-284">Required</span></span> | <span data-ttu-id="adc8a-285">Leírás</span><span class="sxs-lookup"><span data-stu-id="adc8a-285">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="adc8a-286">type</span><span class="sxs-lookup"><span data-stu-id="adc8a-286">type</span></span> | <span data-ttu-id="adc8a-287">Igen</span><span class="sxs-lookup"><span data-stu-id="adc8a-287">Yes</span></span> | <span data-ttu-id="adc8a-288">Hello művelet típusa.</span><span class="sxs-lookup"><span data-stu-id="adc8a-288">Type of hello action.</span></span>  <span data-ttu-id="adc8a-289">Ez lesz **Webhook** webhook műveletekhez.</span><span class="sxs-lookup"><span data-stu-id="adc8a-289">This will be **Webhook** for webhook actions.</span></span> |
| <span data-ttu-id="adc8a-290">név</span><span class="sxs-lookup"><span data-stu-id="adc8a-290">name</span></span> | <span data-ttu-id="adc8a-291">Igen</span><span class="sxs-lookup"><span data-stu-id="adc8a-291">Yes</span></span> | <span data-ttu-id="adc8a-292">Megjelenített név hello a művelethez.</span><span class="sxs-lookup"><span data-stu-id="adc8a-292">Display name for hello action.</span></span>  <span data-ttu-id="adc8a-293">Ez nem hello konzolban jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="adc8a-293">This is not displayed in hello console.</span></span> |
| <span data-ttu-id="adc8a-294">wehookUri</span><span class="sxs-lookup"><span data-stu-id="adc8a-294">wehookUri</span></span> | <span data-ttu-id="adc8a-295">Igen</span><span class="sxs-lookup"><span data-stu-id="adc8a-295">Yes</span></span> | <span data-ttu-id="adc8a-296">Hello webhook URI.</span><span class="sxs-lookup"><span data-stu-id="adc8a-296">Uri for hello webhook.</span></span> |
| <span data-ttu-id="adc8a-297">customPayload</span><span class="sxs-lookup"><span data-stu-id="adc8a-297">customPayload</span></span> | <span data-ttu-id="adc8a-298">Nem</span><span class="sxs-lookup"><span data-stu-id="adc8a-298">No</span></span> | <span data-ttu-id="adc8a-299">Egyéni adattartalom küldött toobe toohello webhook.</span><span class="sxs-lookup"><span data-stu-id="adc8a-299">Custom payload toobe sent toohello webhook.</span></span> <span data-ttu-id="adc8a-300">hello formátum függvényében adható meg, milyen hello webhook által várt paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="adc8a-300">hello format will depend on what hello webhook is expecting.</span></span> |




## <a name="sample"></a><span data-ttu-id="adc8a-301">Minta</span><span class="sxs-lookup"><span data-stu-id="adc8a-301">Sample</span></span>

<span data-ttu-id="adc8a-302">Az alábbiakban látható egy minta a megoldás, amely tartalmazza, amely tartalmazza a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="adc8a-302">Following is a sample of a solution that include that includes hello following resources:</span></span>

- <span data-ttu-id="adc8a-303">Mentett keresés</span><span class="sxs-lookup"><span data-stu-id="adc8a-303">Saved search</span></span>
- <span data-ttu-id="adc8a-304">Ütemezés</span><span class="sxs-lookup"><span data-stu-id="adc8a-304">Schedule</span></span>
- <span data-ttu-id="adc8a-305">Riasztási művelet</span><span class="sxs-lookup"><span data-stu-id="adc8a-305">Alert action</span></span>
- <span data-ttu-id="adc8a-306">Webhook művelet</span><span class="sxs-lookup"><span data-stu-id="adc8a-306">Webhook action</span></span>

<span data-ttu-id="adc8a-307">minta használ hello [szokásos megoldást paraméterek](operations-management-suite-solutions-solution-file.md#parameters) változókat, amelyek a megoldás, mint a gyakran használni ellenezte toohardcoding értékek hello erőforrás-definíciókban.</span><span class="sxs-lookup"><span data-stu-id="adc8a-307">hello sample uses [standard solution parameters](operations-management-suite-solutions-solution-file.md#parameters) variables that would commonly be used in a solution as opposed toohardcoding values in hello resource definitions.</span></span>

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0",
        "parameters": {
          "workspaceName": {
            "type": "string",
            "metadata": {
              "Description": "Name of Log Analytics workspace"
            }
          },
          "accountName": {
            "type": "string",
            "metadata": {
              "Description": "Name of Automation account"
            }
          },
          "workspaceregionId": {
            "type": "string",
            "metadata": {
              "Description": "Region of Log Analytics workspace"
            }
          },
          "regionId": {
            "type": "string",
            "metadata": {
              "Description": "Region of Automation account"
            }
          },
          "pricingTier": {
            "type": "string",
            "metadata": {
              "Description": "Pricing tier of both Log Analytics workspace and Azure Automation account"
            }
          },
          "recipients": {
            "type": "string",
            "metadata": {
              "Description": "List of recipients for hello email alert separated by semicolon"
            }
          }
        },
        "variables": {
          "SolutionName": "MySolution",
          "SolutionVersion": "1.0",
          "SolutionPublisher": "Contoso",
          "ProductName": "SampleSolution",
    
          "LogAnalyticsApiVersion": "2015-11-01-preview",
    
          "MySearch": {
            "displayName": "Error records by hour",
            "query": "Type=MyRecord_CL | measure avg(Rating_d) by Instance_s interval 60minutes",
            "category": "Samples",
            "name": "Samples-Count of data"
          },
          "MyAlert": {
            "Name": "[toLower(concat('myalert-',uniqueString(resourceGroup().id, deployment().name)))]",
            "DisplayName": "My alert rule",
            "Description": "Sample alert.  Fires when 3 error records found over hour interval.",
            "Severity": "Critical",
            "ThresholdOperator": "gt",
            "ThresholdValue": 3,
            "Schedule": {
              "Name": "[toLower(concat('myschedule-',uniqueString(resourceGroup().id, deployment().name)))]",
              "Interval": 15,
              "TimeSpan": 60
            },
            "MetricsTrigger": {
              "TriggerCondition": "Consecutive",
              "Operator": "gt",
              "Value": 3
            },
            "ThrottleMinutes": 60,
            "Notification": {
              "Recipients": [
                "[parameters('recipients')]"
              ],
              "Subject": "Sample alert"
            },
            "Remediation": {
              "RunbookName": "MyRemediationRunbook",
              "WebhookUri": "https://s1events.azure-automation.net/webhooks?token=TluBFH3GpX4IEAnFoImoAWLTULkjD%2bTS0yscyrr7ogw%3d"
            },
            "Webhook": {
              "Name": "MyWebhook",
              "Uri": "https://MyService.com/webhook",
              "Payload": "{\"field1\":\"value1\",\"field2\":\"value2\"}"
            }
          }
        },
        "resources": [
          {
            "name": "[concat(variables('SolutionName'), '[' ,parameters('workspacename'), ']')]",
            "location": "[parameters('workspaceRegionId')]",
            "tags": { },
            "type": "Microsoft.OperationsManagement/solutions",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches', parameters('workspacename'), variables('MySearch').Name)]",
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name)]",
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Name)]",
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Webhook.Name)]"
            ],
            "properties": {
              "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspacename'))]",
              "referencedResources": [
              ],
              "containedResources": [
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches', parameters('workspacename'), variables('MySearch').Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Webhook.Name)]"
              ]
            },
            "plan": {
              "name": "[concat(variables('SolutionName'), '[' ,parameters('workspaceName'), ']')]",
              "Version": "[variables('SolutionVersion')]",
              "product": "[variables('ProductName')]",
              "publisher": "[variables('SolutionPublisher')]",
              "promotionCode": ""
            }
          },
          {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [ ],
            "tags": { },
            "properties": {
              "etag": "*",
              "query": "[variables('MySearch').query]",
              "displayName": "[variables('MySearch').displayName]",
              "category": "[variables('MySearch').category]"
            }
          },
          {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/', variables('MyAlert').Schedule.Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('MySearch').Name)]"
            ],
            "properties": {
              "etag": "*",
              "interval": "[variables('MyAlert').Schedule.Interval]",
              "queryTimeSpan": "[variables('MyAlert').Schedule.TimeSpan]",
              "enabled": true
            }
          },
          {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/',  variables('MyAlert').Schedule.Name, '/',  variables('MyAlert').Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/',  variables('MySearch').Name, '/schedules/', variables('MyAlert').Schedule.Name)]"
            ],
            "properties": {
              "etag": "*",
              "Type": "Alert",
              "Name": "[variables('MyAlert').DisplayName]",
              "Description": "[variables('MyAlert').Description]",
              "Severity": "[variables('MyAlert').Severity]",
              "Threshold": {
                "Operator": "[variables('MyAlert').ThresholdOperator]",
                "Value": "[variables('MyAlert').ThresholdValue]",
                "MetricsTrigger": {
                  "TriggerCondition": "[variables('MyAlert').MetricsTrigger.TriggerCondition]",
                  "Operator": "[variables('MyAlert').MetricsTrigger.Operator]",
                  "Value": "[variables('MyAlert').MetricsTrigger.Value]"
                }
              },
              "Throttling": {
                "DurationInMinutes": "[variables('MyAlert').ThrottleMinutes]"
              },
              "EmailNotification": {
                "Recipients": "[variables('MyAlert').Notification.Recipients]",
                "Subject": "[variables('MyAlert').Notification.Subject]",
                "Attachment": "None"
              },
              "Remediation": {
                "RunbookName": "[variables('MyAlert').Remediation.RunbookName]",
                "WebhookUri": "[variables('MyAlert').Remediation.WebhookUri]"
              }
            }
          },
          {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/', variables('MyAlert').Schedule.Name, '/', variables('MyAlert').Webhook.Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('MySearch').Name, '/schedules/', variables('MyAlert').Schedule.Name)]",
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('MySearch').Name, '/schedules/', variables('MyAlert').Schedule.Name, '/actions/',variables('MyAlert').Name)]"
            ],
            "properties": {
              "etag": "*",
              "Type": "Webhook",
              "Name": "[variables('MyAlert').Webhook.Name]",
              "WebhookUri": "[variables('MyAlert').Webhook.Uri]",
              "CustomPayload": "[variables('MyAlert').Webhook.Payload]"
            }
          }
        ]
    }


<span data-ttu-id="adc8a-308">a következő paraméterfájl hello minták értékeket biztosít ebben a megoldásban.</span><span class="sxs-lookup"><span data-stu-id="adc8a-308">hello following parameter file provides samples values for this solution.</span></span>

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspacename": {
                "value": "myWorkspace"
            },
            "accountName": {
                "value": "myAccount"
            },
            "workspaceregionId": {
                "value": "East US"
            },
            "regionId": {
                "value": "East US 2"
            },
            "pricingTier": {
                "value": "Free"
            },
            "recipients": {
                "value": "recipient1@contoso.com;recipient2@contoso.com"
            }
        }
    }


## <a name="next-steps"></a><span data-ttu-id="adc8a-309">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="adc8a-309">Next steps</span></span>
* <span data-ttu-id="adc8a-310">[Nézetek hozzáadása](operations-management-suite-solutions-resources-views.md) tooyour felügyeleti megoldás.</span><span class="sxs-lookup"><span data-stu-id="adc8a-310">[Add views](operations-management-suite-solutions-resources-views.md) tooyour management solution.</span></span>
* <span data-ttu-id="adc8a-311">[Adja hozzá az Automation-forgatókönyveket és egyéb erőforrások](operations-management-suite-solutions-resources-automation.md) tooyour felügyeleti megoldás.</span><span class="sxs-lookup"><span data-stu-id="adc8a-311">[Add Automation runbooks and other resources](operations-management-suite-solutions-resources-automation.md) tooyour management solution.</span></span>

