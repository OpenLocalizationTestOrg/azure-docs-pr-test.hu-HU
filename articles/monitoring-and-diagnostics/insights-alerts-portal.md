---
title: "Azure-szolgáltatások - aaaCreate értesítések az Azure portálon |} Microsoft Docs"
description: "Eseményindító e-mailek, értesítések, a webhely URL-címek (webhookok), vagy az automation megadott hello feltételek teljesülése esetén hívható."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: f7457655-ced6-4102-a9dd-7ddf2265c0e2
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/23/2016
ms.author: robb
ms.openlocfilehash: 78d862d25255cda9fdfe347329e908a471c39846
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-metric-alerts-in-azure-monitor-for-azure-services---azure-portal"></a><span data-ttu-id="1fb39-103">Hozzon létre metrika riasztások Azure figyelése az Azure-szolgáltatások - Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="1fb39-103">Create metric alerts in Azure Monitor for Azure services - Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1fb39-104">Portál</span><span class="sxs-lookup"><span data-stu-id="1fb39-104">Portal</span></span>](insights-alerts-portal.md)
> * [<span data-ttu-id="1fb39-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="1fb39-105">PowerShell</span></span>](insights-alerts-powershell.md)
> * [<span data-ttu-id="1fb39-106">Parancssori felület</span><span class="sxs-lookup"><span data-stu-id="1fb39-106">CLI</span></span>](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a><span data-ttu-id="1fb39-107">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="1fb39-107">Overview</span></span>
<span data-ttu-id="1fb39-108">Ez a cikk bemutatja, hogyan használja az Azure metrika figyelmeztetéseket tooset hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="1fb39-108">This article shows you how tooset up Azure metric alerts using hello Azure portal.</span></span>   

<span data-ttu-id="1fb39-109">A figyelési metrikákat, vagy események, az Azure-szolgáltatások alapuló riasztást kaphat.</span><span class="sxs-lookup"><span data-stu-id="1fb39-109">You can receive an alert based on monitoring metrics for, or events on, your Azure services.</span></span>

* <span data-ttu-id="1fb39-110">**Metrika értékek** – hello eseményindítók riasztást, ha a megadott metrika értékét hello mindkét irányban rendel a küszöbérték keverve használ.</span><span class="sxs-lookup"><span data-stu-id="1fb39-110">**Metric values** - hello alert triggers when hello value of a specified metric crosses a threshold you assign in either direction.</span></span> <span data-ttu-id="1fb39-111">Ez azt jelenti, hogy elindítja a mindkét amikor először hello feltétel teljesül, és majd ezt követően, hogy a feltétel mikor van már nem teljesül.</span><span class="sxs-lookup"><span data-stu-id="1fb39-111">That is, it triggers both when hello condition is first met and then afterwards when that condition is no longer being met.</span></span>    
* <span data-ttu-id="1fb39-112">**Tevékenység naplóeseményeket** -riasztást aktiválhatók *minden* esemény, vagy csak akkor, ha egy bizonyos események következik be.</span><span class="sxs-lookup"><span data-stu-id="1fb39-112">**Activity log events** - An alert can trigger on *every* event, or, only when a certain events occurs.</span></span> <span data-ttu-id="1fb39-113">További információk a napló tevékenységriasztásokat toolearn [kattintson ide](monitoring-activity-log-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="1fb39-113">toolearn more about activity log alerts [click here](monitoring-activity-log-alerts.md)</span></span>

<span data-ttu-id="1fb39-114">A metrika riasztási toodo hello követően amikor elindítja a konfigurálhatja:</span><span class="sxs-lookup"><span data-stu-id="1fb39-114">You can configure a metric alert toodo hello following when it triggers:</span></span>

* <span data-ttu-id="1fb39-115">e-mail értesítések toohello szolgáltatás-rendszergazda és a társadminisztrátorok küldése</span><span class="sxs-lookup"><span data-stu-id="1fb39-115">send email notifications toohello service administrator and co-administrators</span></span>
* <span data-ttu-id="1fb39-116">e-mail küldése a megadott tooadditional e-maileket.</span><span class="sxs-lookup"><span data-stu-id="1fb39-116">send email tooadditional emails that you specify.</span></span>
* <span data-ttu-id="1fb39-117">A webhook hívása</span><span class="sxs-lookup"><span data-stu-id="1fb39-117">call a webhook</span></span>
* <span data-ttu-id="1fb39-118">egy Azure-runbook (csak az Azure-portálon hello) végrehajtásának elindítása</span><span class="sxs-lookup"><span data-stu-id="1fb39-118">start execution of an Azure runbook (only from hello Azure portal)</span></span>

<span data-ttu-id="1fb39-119">Konfigurálhatja és metrika riasztási szabályok adatainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="1fb39-119">You can configure and get information about metric alert rules using</span></span>

* [<span data-ttu-id="1fb39-120">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="1fb39-120">Azure portal</span></span>](insights-alerts-portal.md)
* [<span data-ttu-id="1fb39-121">PowerShell</span><span class="sxs-lookup"><span data-stu-id="1fb39-121">PowerShell</span></span>](insights-alerts-powershell.md)
* [<span data-ttu-id="1fb39-122">parancssori felület (CLI)</span><span class="sxs-lookup"><span data-stu-id="1fb39-122">command-line interface (CLI)</span></span>](insights-alerts-command-line-interface.md)
* [<span data-ttu-id="1fb39-123">Az Azure figyelő REST API-n</span><span class="sxs-lookup"><span data-stu-id="1fb39-123">Azure Monitor REST API</span></span>](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-with-hello-azure-portal"></a><span data-ttu-id="1fb39-124">Riasztási szabályt létrehozni egy metrika hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="1fb39-124">Create an alert rule on a metric with hello Azure portal</span></span>
1. <span data-ttu-id="1fb39-125">A hello [portal](https://portal.azure.com/), keresse meg a hello erőforrás figyelési érdekli, és válassza ki azt.</span><span class="sxs-lookup"><span data-stu-id="1fb39-125">In hello [portal](https://portal.azure.com/), locate hello resource you are interested in monitoring and select it.</span></span>

2. <span data-ttu-id="1fb39-126">Válassza ki **riasztások** vagy **riasztási szabályok** hello figyelés szakasz alatt.</span><span class="sxs-lookup"><span data-stu-id="1fb39-126">Select **Alerts** or **Alert rules** under hello MONITORING section.</span></span> <span data-ttu-id="1fb39-127">hello szöveg és ikon eltérő lehet attól függően némileg különböző erőforrások.</span><span class="sxs-lookup"><span data-stu-id="1fb39-127">hello text and icon may vary slightly for different resources.</span></span>  

    ![Figyelés](./media/insights-alerts-portal/AlertRulesButton.png)

3. <span data-ttu-id="1fb39-129">Jelölje be hello **riasztás hozzáadása** parancsot, majd hello mezők kitöltésével.</span><span class="sxs-lookup"><span data-stu-id="1fb39-129">Select hello **Add alert** command and fill in hello fields.</span></span>

    ![Riasztások hozzáadása](./media/insights-alerts-portal/AddAlertOnlyParamsPage.png)

4. <span data-ttu-id="1fb39-131">**Név** a riasztás szabályt, majd válassza ki a **leírása**, amely értesítési e-mailt is mutatja.</span><span class="sxs-lookup"><span data-stu-id="1fb39-131">**Name** your alert rule, and choose a **Description**, which also shows in notification emails.</span></span>

5. <span data-ttu-id="1fb39-132">Jelölje be hello **metrika** toomonitor szeretné, majd kattintson egy **feltétel** és **küszöbérték** hello metrika értékét.</span><span class="sxs-lookup"><span data-stu-id="1fb39-132">Select hello **Metric** you want toomonitor, then choose a **Condition** and **Threshold** value for hello metric.</span></span> <span data-ttu-id="1fb39-133">Hello is választott **időszak** metrika hello idő szabály kell teljesíteni hello riasztási eseményindítók előtt.</span><span class="sxs-lookup"><span data-stu-id="1fb39-133">Also chose hello **Period** of time that hello metric rule must be satisfied before hello alert triggers.</span></span> <span data-ttu-id="1fb39-134">Így például hello időszak "PT5M" használ, és 80 % fölötti CPU keresi a riasztás, ha hello riasztás váltja ki, ha hello CPU már következetesen fenti 80 % 5 perc.</span><span class="sxs-lookup"><span data-stu-id="1fb39-134">So for example, if you use hello period "PT5M" and your alert looks for CPU above 80%, hello alert triggers when hello CPU has been consistently above 80% for 5 minutes.</span></span> <span data-ttu-id="1fb39-135">Amennyiben az első eseményindító hello következik be, azt újra váltja ki, ha 5 percig 80 % alatt marad hello CPU.</span><span class="sxs-lookup"><span data-stu-id="1fb39-135">Once hello first trigger occurs, it again triggers when hello CPU stays below 80% for 5 minutes.</span></span> <span data-ttu-id="1fb39-136">hello CPU mérési 1 percenként történik.</span><span class="sxs-lookup"><span data-stu-id="1fb39-136">hello CPU measurement occurs every 1 minute.</span></span>   

6. <span data-ttu-id="1fb39-137">Ellenőrizze **E-mail-tulajdonosok...**  Ha azt szeretné, hogy a rendszergazdák és a társadminisztrátorok toobe e-mailben amikor hello riasztási következik be.</span><span class="sxs-lookup"><span data-stu-id="1fb39-137">Check **Email owners...** if you want administrators and co-administrators toobe emailed when hello alert fires.</span></span>

7. <span data-ttu-id="1fb39-138">Ha azt szeretné, hogy további e-mailek tooreceive egy értesítést, ha hello riasztási következik be, vegye fel a hello **további rendszergazda email(s)** mező.</span><span class="sxs-lookup"><span data-stu-id="1fb39-138">If you want additional emails tooreceive a notification when hello alert fires, add them in hello **Additional Administrator email(s)** field.</span></span> <span data-ttu-id="1fb39-139">Több e-mailek külön és pontosvesszővel kell elválasztani -  *email@contoso.com;email2@contoso.com*</span><span class="sxs-lookup"><span data-stu-id="1fb39-139">Separate multiple emails with semi-colons - *email@contoso.com;email2@contoso.com*</span></span>

8. <span data-ttu-id="1fb39-140">Be egy érvényes URI-azonosító található hello **Webhook** mező Ha azt szeretné, az úgynevezett amikor hello riasztási következik be.</span><span class="sxs-lookup"><span data-stu-id="1fb39-140">Put in a valid URI in hello **Webhook** field if you want it called when hello alert fires.</span></span>

9. <span data-ttu-id="1fb39-141">Azure Automation használatakor választhatja a Runbook toobe hello riasztás aktiválódásakor futtassa.</span><span class="sxs-lookup"><span data-stu-id="1fb39-141">If you use Azure Automation, you can select a Runbook toobe run when hello alert fires.</span></span>

10. <span data-ttu-id="1fb39-142">Válassza ki **OK** Amikor kész toocreate hello riasztás.</span><span class="sxs-lookup"><span data-stu-id="1fb39-142">Select **OK** when done toocreate hello alert.</span></span>   

<span data-ttu-id="1fb39-143">Néhány percen belül hello riasztás aktív, és elindítja a leírt módon.</span><span class="sxs-lookup"><span data-stu-id="1fb39-143">Within a few minutes, hello alert is active and triggers as previously described.</span></span>

## <a name="managing-your-alerts"></a><span data-ttu-id="1fb39-144">A riasztások kezelése</span><span class="sxs-lookup"><span data-stu-id="1fb39-144">Managing your alerts</span></span>
<span data-ttu-id="1fb39-145">Miután létrehozott egy riasztást, kijelölheti azt és:</span><span class="sxs-lookup"><span data-stu-id="1fb39-145">Once you have created an alert, you can select it and:</span></span>

* <span data-ttu-id="1fb39-146">Egy grafikonon hello metrika küszöbérték és a tényleges értékek hello hello előző nap megtekintése.</span><span class="sxs-lookup"><span data-stu-id="1fb39-146">View a graph showing hello metric threshold and hello actual values from hello previous day.</span></span>
* <span data-ttu-id="1fb39-147">Szerkesztheti és törölheti azt.</span><span class="sxs-lookup"><span data-stu-id="1fb39-147">Edit or delete it.</span></span>
* <span data-ttu-id="1fb39-148">**Tiltsa le a** vagy **engedélyezése** azt, ha azt szeretné tootemporarily stop, vagy folytassa a riasztás-mailjeire.</span><span class="sxs-lookup"><span data-stu-id="1fb39-148">**Disable** or **Enable** it if you want tootemporarily stop or resume receiving notifications for that alert.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1fb39-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1fb39-149">Next steps</span></span>
* <span data-ttu-id="1fb39-150">[Az Azure Figyelés áttekintése](monitoring-overview.md) például hello típusú információkat gyűjt, és figyelheti.</span><span class="sxs-lookup"><span data-stu-id="1fb39-150">[Get an overview of Azure monitoring](monitoring-overview.md) including hello types of information you can collect and monitor.</span></span>
* <span data-ttu-id="1fb39-151">További információ [konfigurálása webhookokkal a riasztások](insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="1fb39-151">Learn more about [configuring webhooks in alerts](insights-webhooks-alerts.md).</span></span>
* <span data-ttu-id="1fb39-152">További információ [riasztások konfigurálása a naplózási eseményeket](monitoring-activity-log-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="1fb39-152">Learn more about [configuring alerts on Activity log events](monitoring-activity-log-alerts.md).</span></span>
* <span data-ttu-id="1fb39-153">További információ [Azure Automation-forgatókönyveket](../automation/automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="1fb39-153">Learn more about [Azure Automation Runbooks](../automation/automation-starting-a-runbook.md).</span></span>
* <span data-ttu-id="1fb39-154">Első egy [diagnosztikai naplók áttekintése](monitoring-overview-of-diagnostic-logs.md) és begyűjtése részletes nagyon gyakori a szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="1fb39-154">Get an [overview of diagnostic logs](monitoring-overview-of-diagnostic-logs.md) and collect detailed high-frequency metrics on your service.</span></span>
* <span data-ttu-id="1fb39-155">Első egy [metrikák gyűjtemény áttekintése](insights-how-to-customize-monitoring.md) toomake meg arról, hogy a szolgáltatás megfelelően üzemel és rugalmas.</span><span class="sxs-lookup"><span data-stu-id="1fb39-155">Get an [overview of metrics collection](insights-how-to-customize-monitoring.md) toomake sure your service is available and responsive.</span></span>
