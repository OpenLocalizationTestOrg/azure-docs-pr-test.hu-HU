---
title: "Informatikai Management-összekötő az OMS szolgáltatással |} Microsoft Docs"
description: "Az informatikai szolgáltatás Management-összekötő használatával központilag figyelheti és az OMS ITSM munkaelemek kezeléséhez, és gyorsan hárítson el minden problémát."
services: log-analytics
documentationcenter: 
author: JYOTHIRMAISURI
manager: riyazp
editor: 
ms.assetid: 0b1414d9-b0a7-4e4e-a652-d3a6ff1118c4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: v-jysur
ms.openlocfilehash: 54974ef06efdae69ddbfa12b1ba9278b48b113d3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="centrally-manage-itsm-work-items-using-it-service-management-connector-preview"></a><span data-ttu-id="ae4ae-103">Központi kezelését az informatikai szolgáltatás Management Connector (előzetes verzió) segítségével ITSM munkaelemek</span><span class="sxs-lookup"><span data-stu-id="ae4ae-103">Centrally manage ITSM work items using IT Service Management Connector (Preview)</span></span>

![Informatikai Management-összekötő szimbólum](./media/log-analytics-itsmc/itsmc-symbol.png)

<span data-ttu-id="ae4ae-105">Segítségével az informatikai szolgáltatás Management Connector (ITSMC) az OMS szolgáltatáshoz központilag figyelheti és munkaelemek kezelésére a ITSM termékek vagy szolgáltatások között.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-105">You can use the IT Service Management Connector (ITSMC) in OMS Log Analytics to centrally monitor and manage work items across your ITSM products/services.</span></span>

<span data-ttu-id="ae4ae-106">Az informatikai szolgáltatás Management-összekötő integrálható a meglévő IT Service Management (ITSM) termékek és szolgáltatások a OMS szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-106">The IT Service Management Connector integrates your existing IT Service Management (ITSM) products and services with OMS Log Analytics.</span></span>  <span data-ttu-id="ae4ae-107">A megoldás rendelkezik kétirányú integrációt ITSM termékek vagy szolgáltatások, ahol azt az OMS-felhasználók lehetőséget biztosít az incidensek, a riasztásokat vagy a események létrehozása ITSM megoldásban.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-107">The solution has bidirectional integration with ITSM products/services, where it provides the OMS users an option to create incidents, alerts, or events in ITSM solution.</span></span> <span data-ttu-id="ae4ae-108">Az összekötő is különféle adatok, például incidensek és változáskérések ITSM megoldásból az OMS szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-108">The connector also  imports data such as incidents, and change requests from ITSM solution into OMS Log Analytics.</span></span>

<span data-ttu-id="ae4ae-109">Az informatikai szolgáltatás Management-összekötő segítségével:</span><span class="sxs-lookup"><span data-stu-id="ae4ae-109">With IT Service Management Connector, you can:</span></span>

  - <span data-ttu-id="ae4ae-110">Központilag figyelheti és ITSM termékek vagy szolgáltatások a szervezetben használt munkaelemek kezelésére.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-110">Centrally monitor and manage work items for ITSM products/services used across your organization.</span></span>
  - <span data-ttu-id="ae4ae-111">Hozzon létre ITSM munkaelemeket (például a riasztást, esemény, incidens) ITSM az OMS-riasztások és a keresési napló.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-111">Create ITSM work items (like alert, event, incident) in ITSM from OMS alerts and through log search.</span></span>
  - <span data-ttu-id="ae4ae-112">Olvassa el az incidensek és változáskérések az ITSM megoldásból, és vonatkozó naplóadatok Naplóelemzési munkaterület függ.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-112">Read incidents and change requests from your ITSM solution and correlate with relevant log data in Log Analytics workspace.</span></span>
  - <span data-ttu-id="ae4ae-113">Váratlan és szokatlan eseményeket található, és a megoldásukkal együtt, még mielőtt a végfelhasználók hívja, és jelentse őket a támogató szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-113">Find any unexpected and unusual events and resolve them, even before the end users call and report them to the helpdesk.</span></span>
  - <span data-ttu-id="ae4ae-114">Munkahelyi elemek adatok importálása a Naplóelemzési és fő teljesítménymutatók (KPI) jelentések létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-114">Import work items data into Log Analytics and create key performance indicator (KPI) reports.</span></span>  <span data-ttu-id="ae4ae-115">Ezekre a jelentésekre, segítségével azonosíthatja, értékeléséhez és számos fontos elemek, például a kártevő szoftverek assessment működjön.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-115">Using these reports, you can identify, assess and act on several important items such as malware assessment.</span></span>
  - <span data-ttu-id="ae4ae-116">Részletesebben is elemezheti válogatott irányítópultok incidensek megtekintéséhez a változáskérésekhez és az érintett rendszerek.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-116">View curated dashboards for deeper insights on incidents, change requests and impacted systems.</span></span>
  - <span data-ttu-id="ae4ae-117">Végezzen hibaelhárítást a gyorsabb egyéb megoldások a Naplóelemzési munkaterület az adatok.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-117">Troubleshoot faster by correlating with other management solutions in the Log Analytics workspace.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="ae4ae-118">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ae4ae-118">Prerequisites</span></span>

<span data-ttu-id="ae4ae-119">A ITSM munkaelemek importálása az OMS szolgáltatáshoz, a megoldás az informatikai szolgáltatás Management-összekötő az OMS a és a informatikai SM termék vagy szolgáltatás, amelyből importál a munkaelemek közötti kapcsolat szükséges.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-119">To import the ITSM work items into OMS Log Analytics, the solution requires a connection between the IT Service Management Connector in the OMS and the IT SM product/service from which you import the work items.</span></span>


## <a name="configuration"></a><span data-ttu-id="ae4ae-120">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="ae4ae-120">Configuration</span></span>

<span data-ttu-id="ae4ae-121">Az informatikai szolgáltatás Management-összekötő megoldás felvétele a OMS munkahelyi tárhelyre, ismertetett eljárással [hozzáadni a Naplóelemzési megoldások a megoldások gyűjteményből](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="ae4ae-121">Add the IT Service Management Connector solution to your OMS work space, using the process described in [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md).</span></span>

<span data-ttu-id="ae4ae-122">Informatikai szolgáltatás Management-összekötő csempére a megoldások katalógusban látható módon:</span><span class="sxs-lookup"><span data-stu-id="ae4ae-122">IT Service Management Connector tile as you see in the Solutions gallery:</span></span>

![összekötő csempe](./media/log-analytics-itsmc/itsmc-solutions-tile.png)

<span data-ttu-id="ae4ae-124">Sikeres hozzáadása után az IT Service Management-összekötő alatt megjelenik **OMS** > **beállítások** > **csatlakoztatott források.**</span><span class="sxs-lookup"><span data-stu-id="ae4ae-124">After successful addition, you will see the IT Service Management Connector under **OMS** > **Settings** > **Connected Sources.**</span></span>

![Csatlakoztatott ITSMC](./media/log-analytics-itsmc/itsmc-overview-solution-in-connected-sources.png)

> [!NOTE]

> <span data-ttu-id="ae4ae-126">Alapértelmezés szerint az IT Service Management-összekötő 24 óránként egyszer a a kapcsolati adatok frissítése.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-126">By default, the IT Service Management Connector refreshes the connection's data once in every 24 hours.</span></span> <span data-ttu-id="ae4ae-127">Azonnal az esetleges módosításokat, vagy a Szolgáltatássablon-frissítések, amelyek miatt, a frissítés gombra, a kapcsolat mellett megjelenik a kapcsolati adatok frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-127">To refresh your connection's data instantly for any edits or template updates that you make, click the refresh button displayed next to your connection.</span></span>

 ![ITSMC frissítése](./media/log-analytics-itsmc/itsmc-connection-refresh.png)

## <a name="management-packs"></a><span data-ttu-id="ae4ae-129">Felügyeleti csomagok</span><span class="sxs-lookup"><span data-stu-id="ae4ae-129">Management packs</span></span>
<span data-ttu-id="ae4ae-130">Ez a megoldás nem igényel minden olyan felügyeleti csomagot.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-130">This solution does not require any management packs.</span></span>

## <a name="connected-sources"></a><span data-ttu-id="ae4ae-131">Összekapcsolt források</span><span class="sxs-lookup"><span data-stu-id="ae4ae-131">Connected sources</span></span>

<span data-ttu-id="ae4ae-132">A következő ITSM termékek vagy szolgáltatások az informatikai szolgáltatás Management-összekötő által támogatott:</span><span class="sxs-lookup"><span data-stu-id="ae4ae-132">The following ITSM products/services are supported by the IT Service Management Connector:</span></span>

- [<span data-ttu-id="ae4ae-133">A System Center Service Manager (SCSM)</span><span class="sxs-lookup"><span data-stu-id="ae4ae-133">System Center Service Manager (SCSM)</span></span>](log-analytics-itsmc-connections.md#connect-system-center-service-manager-to-it-service-management-connector-in-oms)

- [<span data-ttu-id="ae4ae-134">A ServiceNow</span><span class="sxs-lookup"><span data-stu-id="ae4ae-134">ServiceNow</span></span>](log-analytics-itsmc-connections.md#connect-servicenow-to-it-service-management-connector-in-oms)

- [<span data-ttu-id="ae4ae-135">Provance</span><span class="sxs-lookup"><span data-stu-id="ae4ae-135">Provance</span></span>](log-analytics-itsmc-connections.md#connect-provance-to-it-service-management-connector-in-oms)  

- [<span data-ttu-id="ae4ae-136">Cherwell</span><span class="sxs-lookup"><span data-stu-id="ae4ae-136">Cherwell</span></span>](log-analytics-itsmc-connections.md#connect-cherwell-to-it-service-management-connector-in-oms)

## <a name="using-the-solution"></a><span data-ttu-id="ae4ae-137">A megoldás használata</span><span class="sxs-lookup"><span data-stu-id="ae4ae-137">Using the solution</span></span>

<span data-ttu-id="ae4ae-138">Az OMS informatikai Management-összekötő csatlakozott a ITSM szolgáltatással, miután a Naplóelemzési szolgáltatások kezdődik, az adatgyűjtés a csatlakoztatott ITSM termékek/szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-138">Once you connect the OMS IT Service Management Connector with your ITSM service, the Log Analytics services starts gathering the data from the connected ITSM products/service.</span></span>

> [!NOTE]
> - <span data-ttu-id="ae4ae-139">Log Analytics nevű eseményként is megjelenik megoldás IT Service Management-összekötő által importált adatok **ServiceDesk_CL**.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-139">Data imported by IT Service Management Connector solution appears in Log Analytics as events named **ServiceDesk_CL**.</span></span>
- <span data-ttu-id="ae4ae-140">Esemény nevű mezőt tartalmaz **ServiceDeskWorkItemType_s**.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-140">Event contains a field named **ServiceDeskWorkItemType_s**.</span></span> <span data-ttu-id="ae4ae-141">amely a számot, incidens, vagy változáskérés, attól függően, hogy a munka elem szereplő adatok a **ServiceDesk_CL** esemény.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-141">which can take its value as incident, or change request, depending on the work item data contained in the **ServiceDesk_CL** event.</span></span>

## <a name="input-data"></a><span data-ttu-id="ae4ae-142">A bemeneti adatok</span><span class="sxs-lookup"><span data-stu-id="ae4ae-142">Input data</span></span>
<span data-ttu-id="ae4ae-143">A munkaelemek ITSM termékek vagy szolgáltatások importálására.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-143">Work items imported from the ITSM products/services.</span></span>

<span data-ttu-id="ae4ae-144">A következő információkat az informatikai szolgáltatás Management-összekötő által összegyűjtött adatok láthatók:</span><span class="sxs-lookup"><span data-stu-id="ae4ae-144">The following information shows examples of data gathered by the IT Service Management connector:</span></span>

> [!NOTE]
> <span data-ttu-id="ae4ae-145">Attól függően, hogy a munkaelem-típusban importálni a Log Analyticshez **ServiceDesk_CL** a következő mezőket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="ae4ae-145">Depending on the work item type imported into Log Analytics, **ServiceDesk_CL** contains the following fields:</span></span>

<span data-ttu-id="ae4ae-146">**Munkaelem:** **incidensek**</span><span class="sxs-lookup"><span data-stu-id="ae4ae-146">**Work item:** **Incidents**</span></span>  
<span data-ttu-id="ae4ae-147">ServiceDeskWorkItemType_s = "Esemény"</span><span class="sxs-lookup"><span data-stu-id="ae4ae-147">ServiceDeskWorkItemType_s="Incident"</span></span>

<span data-ttu-id="ae4ae-148">**Mezők**</span><span class="sxs-lookup"><span data-stu-id="ae4ae-148">**Fields**</span></span>

- <span data-ttu-id="ae4ae-149">ServiceDeskConnectionName</span><span class="sxs-lookup"><span data-stu-id="ae4ae-149">ServiceDeskConnectionName</span></span>
- <span data-ttu-id="ae4ae-150">Szolgáltatás ügyfélszolgálati azonosítója</span><span class="sxs-lookup"><span data-stu-id="ae4ae-150">Service Desk ID</span></span>
- <span data-ttu-id="ae4ae-151">Állapot</span><span class="sxs-lookup"><span data-stu-id="ae4ae-151">State</span></span>
- <span data-ttu-id="ae4ae-152">Sürgős</span><span class="sxs-lookup"><span data-stu-id="ae4ae-152">Urgency</span></span>
- <span data-ttu-id="ae4ae-153">Gyakorolt hatás</span><span class="sxs-lookup"><span data-stu-id="ae4ae-153">Impact</span></span>
- <span data-ttu-id="ae4ae-154">Prioritás</span><span class="sxs-lookup"><span data-stu-id="ae4ae-154">Priority</span></span>
- <span data-ttu-id="ae4ae-155">Eszkalációs</span><span class="sxs-lookup"><span data-stu-id="ae4ae-155">Escalation</span></span>
- <span data-ttu-id="ae4ae-156">Hozta létre</span><span class="sxs-lookup"><span data-stu-id="ae4ae-156">Created By</span></span>
- <span data-ttu-id="ae4ae-157">Megoldó</span><span class="sxs-lookup"><span data-stu-id="ae4ae-157">Resolved By</span></span>
- <span data-ttu-id="ae4ae-158">Lezárt</span><span class="sxs-lookup"><span data-stu-id="ae4ae-158">Closed By</span></span>
- <span data-ttu-id="ae4ae-159">Forrás</span><span class="sxs-lookup"><span data-stu-id="ae4ae-159">Source</span></span>
- <span data-ttu-id="ae4ae-160">Rendelt</span><span class="sxs-lookup"><span data-stu-id="ae4ae-160">Assigned To</span></span>
- <span data-ttu-id="ae4ae-161">Kategória</span><span class="sxs-lookup"><span data-stu-id="ae4ae-161">Category</span></span>
- <span data-ttu-id="ae4ae-162">Cím</span><span class="sxs-lookup"><span data-stu-id="ae4ae-162">Title</span></span>
- <span data-ttu-id="ae4ae-163">Leírás</span><span class="sxs-lookup"><span data-stu-id="ae4ae-163">Description</span></span>
- <span data-ttu-id="ae4ae-164">Létrehozás dátuma</span><span class="sxs-lookup"><span data-stu-id="ae4ae-164">Created Date</span></span>
- <span data-ttu-id="ae4ae-165">Lezárás dátuma</span><span class="sxs-lookup"><span data-stu-id="ae4ae-165">Closed Date</span></span>
- <span data-ttu-id="ae4ae-166">Megoldás dátuma</span><span class="sxs-lookup"><span data-stu-id="ae4ae-166">Resolved Date</span></span>
- <span data-ttu-id="ae4ae-167">Utolsó módosítás dátuma</span><span class="sxs-lookup"><span data-stu-id="ae4ae-167">Last Modified Date</span></span>
- <span data-ttu-id="ae4ae-168">Computer</span><span class="sxs-lookup"><span data-stu-id="ae4ae-168">Computer</span></span>


<span data-ttu-id="ae4ae-169">**Munkaelem:** **Változáskérések**</span><span class="sxs-lookup"><span data-stu-id="ae4ae-169">**Work item:** **Change Requests**</span></span>

<span data-ttu-id="ae4ae-170">ServiceDeskWorkItemType_s = "módosítási kérés"</span><span class="sxs-lookup"><span data-stu-id="ae4ae-170">ServiceDeskWorkItemType_s="ChangeRequest"</span></span>

<span data-ttu-id="ae4ae-171">**Mezők**</span><span class="sxs-lookup"><span data-stu-id="ae4ae-171">**Fields**</span></span>
- <span data-ttu-id="ae4ae-172">ServiceDeskConnectionName</span><span class="sxs-lookup"><span data-stu-id="ae4ae-172">ServiceDeskConnectionName</span></span>
- <span data-ttu-id="ae4ae-173">Szolgáltatás ügyfélszolgálati azonosítója</span><span class="sxs-lookup"><span data-stu-id="ae4ae-173">Service Desk ID</span></span>
- <span data-ttu-id="ae4ae-174">Hozta létre</span><span class="sxs-lookup"><span data-stu-id="ae4ae-174">Created By</span></span>
- <span data-ttu-id="ae4ae-175">Lezárt</span><span class="sxs-lookup"><span data-stu-id="ae4ae-175">Closed By</span></span>
- <span data-ttu-id="ae4ae-176">Forrás</span><span class="sxs-lookup"><span data-stu-id="ae4ae-176">Source</span></span>
- <span data-ttu-id="ae4ae-177">Rendelt</span><span class="sxs-lookup"><span data-stu-id="ae4ae-177">Assigned To</span></span>
- <span data-ttu-id="ae4ae-178">Cím</span><span class="sxs-lookup"><span data-stu-id="ae4ae-178">Title</span></span>
- <span data-ttu-id="ae4ae-179">Típus</span><span class="sxs-lookup"><span data-stu-id="ae4ae-179">Type</span></span>
- <span data-ttu-id="ae4ae-180">Kategória</span><span class="sxs-lookup"><span data-stu-id="ae4ae-180">Category</span></span>
- <span data-ttu-id="ae4ae-181">Állapot</span><span class="sxs-lookup"><span data-stu-id="ae4ae-181">State</span></span>
- <span data-ttu-id="ae4ae-182">Eszkalációs</span><span class="sxs-lookup"><span data-stu-id="ae4ae-182">Escalation</span></span>
- <span data-ttu-id="ae4ae-183">Ütközés állapota</span><span class="sxs-lookup"><span data-stu-id="ae4ae-183">Conflict Status</span></span>
- <span data-ttu-id="ae4ae-184">Sürgős</span><span class="sxs-lookup"><span data-stu-id="ae4ae-184">Urgency</span></span>
- <span data-ttu-id="ae4ae-185">Prioritás</span><span class="sxs-lookup"><span data-stu-id="ae4ae-185">Priority</span></span>
- <span data-ttu-id="ae4ae-186">Kockázati</span><span class="sxs-lookup"><span data-stu-id="ae4ae-186">Risk</span></span>
- <span data-ttu-id="ae4ae-187">Gyakorolt hatás</span><span class="sxs-lookup"><span data-stu-id="ae4ae-187">Impact</span></span>
- <span data-ttu-id="ae4ae-188">Rendelt</span><span class="sxs-lookup"><span data-stu-id="ae4ae-188">Assigned To</span></span>
- <span data-ttu-id="ae4ae-189">Létrehozás dátuma</span><span class="sxs-lookup"><span data-stu-id="ae4ae-189">Created Date</span></span>
- <span data-ttu-id="ae4ae-190">Lezárás dátuma</span><span class="sxs-lookup"><span data-stu-id="ae4ae-190">Closed Date</span></span>
- <span data-ttu-id="ae4ae-191">Utolsó módosítás dátuma</span><span class="sxs-lookup"><span data-stu-id="ae4ae-191">Last Modified Date</span></span>
- <span data-ttu-id="ae4ae-192">Kért dátuma</span><span class="sxs-lookup"><span data-stu-id="ae4ae-192">Requested Date</span></span>
- <span data-ttu-id="ae4ae-193">Tervezett kezdő dátuma</span><span class="sxs-lookup"><span data-stu-id="ae4ae-193">Planned Start Date</span></span>
- <span data-ttu-id="ae4ae-194">Tervezett befejezési dátum</span><span class="sxs-lookup"><span data-stu-id="ae4ae-194">Planned End Date</span></span>
- <span data-ttu-id="ae4ae-195">Munkahelyi kezdő dátuma</span><span class="sxs-lookup"><span data-stu-id="ae4ae-195">Work Start Date</span></span>
- <span data-ttu-id="ae4ae-196">Munka befejezési dátum</span><span class="sxs-lookup"><span data-stu-id="ae4ae-196">Work End Date</span></span>
- <span data-ttu-id="ae4ae-197">Leírás</span><span class="sxs-lookup"><span data-stu-id="ae4ae-197">Description</span></span>
- <span data-ttu-id="ae4ae-198">Computer</span><span class="sxs-lookup"><span data-stu-id="ae4ae-198">Computer</span></span>

## <a name="output-data-for-a-servicenow-incident"></a><span data-ttu-id="ae4ae-199">A ServiceNow, amelyhez a kimeneti adatok</span><span class="sxs-lookup"><span data-stu-id="ae4ae-199">Output data for a ServiceNow incident</span></span>

| <span data-ttu-id="ae4ae-200">OMS mező</span><span class="sxs-lookup"><span data-stu-id="ae4ae-200">OMS field</span></span> | <span data-ttu-id="ae4ae-201">ITSM mező</span><span class="sxs-lookup"><span data-stu-id="ae4ae-201">ITSM field</span></span> |
|:--- |:--- |
| <span data-ttu-id="ae4ae-202">ServiceDeskId_s</span><span class="sxs-lookup"><span data-stu-id="ae4ae-202">ServiceDeskId_s</span></span>| <span data-ttu-id="ae4ae-203">Szám</span><span class="sxs-lookup"><span data-stu-id="ae4ae-203">Number</span></span> |
| <span data-ttu-id="ae4ae-204">IncidentState_s</span><span class="sxs-lookup"><span data-stu-id="ae4ae-204">IncidentState_s</span></span> | <span data-ttu-id="ae4ae-205">Állapot</span><span class="sxs-lookup"><span data-stu-id="ae4ae-205">State</span></span> |
| <span data-ttu-id="ae4ae-206">Urgency_s</span><span class="sxs-lookup"><span data-stu-id="ae4ae-206">Urgency_s</span></span> |<span data-ttu-id="ae4ae-207">Sürgős</span><span class="sxs-lookup"><span data-stu-id="ae4ae-207">Urgency</span></span> |
| <span data-ttu-id="ae4ae-208">Impact_s</span><span class="sxs-lookup"><span data-stu-id="ae4ae-208">Impact_s</span></span> |<span data-ttu-id="ae4ae-209">Gyakorolt hatás</span><span class="sxs-lookup"><span data-stu-id="ae4ae-209">Impact</span></span>|
| <span data-ttu-id="ae4ae-210">Priority_s</span><span class="sxs-lookup"><span data-stu-id="ae4ae-210">Priority_s</span></span> | <span data-ttu-id="ae4ae-211">Prioritás</span><span class="sxs-lookup"><span data-stu-id="ae4ae-211">Priority</span></span> |
| <span data-ttu-id="ae4ae-212">CreatedBy_s</span><span class="sxs-lookup"><span data-stu-id="ae4ae-212">CreatedBy_s</span></span> | <span data-ttu-id="ae4ae-213">Által megnyitott</span><span class="sxs-lookup"><span data-stu-id="ae4ae-213">Opened by</span></span> |
| <span data-ttu-id="ae4ae-214">ResolvedBy_s</span><span class="sxs-lookup"><span data-stu-id="ae4ae-214">ResolvedBy_s</span></span> | <span data-ttu-id="ae4ae-215">Megoldó</span><span class="sxs-lookup"><span data-stu-id="ae4ae-215">Resolved by</span></span>|
| <span data-ttu-id="ae4ae-216">ClosedBy_s</span><span class="sxs-lookup"><span data-stu-id="ae4ae-216">ClosedBy_s</span></span>  | <span data-ttu-id="ae4ae-217">Lezárt</span><span class="sxs-lookup"><span data-stu-id="ae4ae-217">Closed by</span></span> |
| <span data-ttu-id="ae4ae-218">Source_s</span><span class="sxs-lookup"><span data-stu-id="ae4ae-218">Source_s</span></span>| <span data-ttu-id="ae4ae-219">Kapcsolat típusa</span><span class="sxs-lookup"><span data-stu-id="ae4ae-219">Contact type</span></span> |
| <span data-ttu-id="ae4ae-220">AssignedTo_s</span><span class="sxs-lookup"><span data-stu-id="ae4ae-220">AssignedTo_s</span></span> | <span data-ttu-id="ae4ae-221">Rendelt</span><span class="sxs-lookup"><span data-stu-id="ae4ae-221">Assigned to</span></span>  |
| <span data-ttu-id="ae4ae-222">Category_s</span><span class="sxs-lookup"><span data-stu-id="ae4ae-222">Category_s</span></span> | <span data-ttu-id="ae4ae-223">Kategória</span><span class="sxs-lookup"><span data-stu-id="ae4ae-223">Category</span></span> |
| <span data-ttu-id="ae4ae-224">Title_s</span><span class="sxs-lookup"><span data-stu-id="ae4ae-224">Title_s</span></span>|  <span data-ttu-id="ae4ae-225">Rövid leírás</span><span class="sxs-lookup"><span data-stu-id="ae4ae-225">Short description</span></span> |
| <span data-ttu-id="ae4ae-226">Description_s</span><span class="sxs-lookup"><span data-stu-id="ae4ae-226">Description_s</span></span>|  <span data-ttu-id="ae4ae-227">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="ae4ae-227">Notes</span></span> |
| <span data-ttu-id="ae4ae-228">CreatedDate_t</span><span class="sxs-lookup"><span data-stu-id="ae4ae-228">CreatedDate_t</span></span>|  <span data-ttu-id="ae4ae-229">Megnyitása</span><span class="sxs-lookup"><span data-stu-id="ae4ae-229">Opened</span></span> |
| <span data-ttu-id="ae4ae-230">ClosedDate_t</span><span class="sxs-lookup"><span data-stu-id="ae4ae-230">ClosedDate_t</span></span>| <span data-ttu-id="ae4ae-231">Lezárt</span><span class="sxs-lookup"><span data-stu-id="ae4ae-231">closed</span></span>|
| <span data-ttu-id="ae4ae-232">ResolvedDate_t</span><span class="sxs-lookup"><span data-stu-id="ae4ae-232">ResolvedDate_t</span></span>|<span data-ttu-id="ae4ae-233">Feloldva</span><span class="sxs-lookup"><span data-stu-id="ae4ae-233">Resolved</span></span>|
| <span data-ttu-id="ae4ae-234">Computer</span><span class="sxs-lookup"><span data-stu-id="ae4ae-234">Computer</span></span>  | <span data-ttu-id="ae4ae-235">konfigurációs elem</span><span class="sxs-lookup"><span data-stu-id="ae4ae-235">Configuration item</span></span> |

## <a name="output-data-for-a-servicenow-change-request"></a><span data-ttu-id="ae4ae-236">A ServiceNow kimeneti adatok változáskérés</span><span class="sxs-lookup"><span data-stu-id="ae4ae-236">Output data for a ServiceNow change request</span></span>

| <span data-ttu-id="ae4ae-237">OMS mező</span><span class="sxs-lookup"><span data-stu-id="ae4ae-237">OMS field</span></span> | <span data-ttu-id="ae4ae-238">ITSM mező</span><span class="sxs-lookup"><span data-stu-id="ae4ae-238">ITSM field</span></span> |
|:--- |:--- |
| <span data-ttu-id="ae4ae-239">ServiceDeskId_s</span><span class="sxs-lookup"><span data-stu-id="ae4ae-239">ServiceDeskId_s</span></span>| <span data-ttu-id="ae4ae-240">Szám</span><span class="sxs-lookup"><span data-stu-id="ae4ae-240">Number</span></span> |
| <span data-ttu-id="ae4ae-241">CreatedBy_s</span><span class="sxs-lookup"><span data-stu-id="ae4ae-241">CreatedBy_s</span></span> | <span data-ttu-id="ae4ae-242">Által kért</span><span class="sxs-lookup"><span data-stu-id="ae4ae-242">Requested by</span></span> |
| <span data-ttu-id="ae4ae-243">ClosedBy_s</span><span class="sxs-lookup"><span data-stu-id="ae4ae-243">ClosedBy_s</span></span> | <span data-ttu-id="ae4ae-244">Lezárt</span><span class="sxs-lookup"><span data-stu-id="ae4ae-244">Closed by</span></span> |
| <span data-ttu-id="ae4ae-245">AssignedTo_s</span><span class="sxs-lookup"><span data-stu-id="ae4ae-245">AssignedTo_s</span></span> | <span data-ttu-id="ae4ae-246">Rendelt</span><span class="sxs-lookup"><span data-stu-id="ae4ae-246">Assigned to</span></span>  |
| <span data-ttu-id="ae4ae-247">Title_s</span><span class="sxs-lookup"><span data-stu-id="ae4ae-247">Title_s</span></span>|  <span data-ttu-id="ae4ae-248">Rövid leírás</span><span class="sxs-lookup"><span data-stu-id="ae4ae-248">Short description</span></span> |
| <span data-ttu-id="ae4ae-249">Type_s</span><span class="sxs-lookup"><span data-stu-id="ae4ae-249">Type_s</span></span>|  <span data-ttu-id="ae4ae-250">Típus</span><span class="sxs-lookup"><span data-stu-id="ae4ae-250">Type</span></span> |
| <span data-ttu-id="ae4ae-251">Category_s</span><span class="sxs-lookup"><span data-stu-id="ae4ae-251">Category_s</span></span>|  <span data-ttu-id="ae4ae-252">Catgory</span><span class="sxs-lookup"><span data-stu-id="ae4ae-252">Catgory</span></span> |
| <span data-ttu-id="ae4ae-253">CRState_s</span><span class="sxs-lookup"><span data-stu-id="ae4ae-253">CRState_s</span></span>|  <span data-ttu-id="ae4ae-254">Állapot</span><span class="sxs-lookup"><span data-stu-id="ae4ae-254">State</span></span>|
| <span data-ttu-id="ae4ae-255">Urgency_s</span><span class="sxs-lookup"><span data-stu-id="ae4ae-255">Urgency_s</span></span>|  <span data-ttu-id="ae4ae-256">Sürgős</span><span class="sxs-lookup"><span data-stu-id="ae4ae-256">Urgency</span></span> |
| <span data-ttu-id="ae4ae-257">Priority_s</span><span class="sxs-lookup"><span data-stu-id="ae4ae-257">Priority_s</span></span>| <span data-ttu-id="ae4ae-258">Prioritás</span><span class="sxs-lookup"><span data-stu-id="ae4ae-258">Priority</span></span>|
| <span data-ttu-id="ae4ae-259">Risk_s</span><span class="sxs-lookup"><span data-stu-id="ae4ae-259">Risk_s</span></span>| <span data-ttu-id="ae4ae-260">Kockázati</span><span class="sxs-lookup"><span data-stu-id="ae4ae-260">Risk</span></span>|
| <span data-ttu-id="ae4ae-261">Impact_s</span><span class="sxs-lookup"><span data-stu-id="ae4ae-261">Impact_s</span></span>| <span data-ttu-id="ae4ae-262">Gyakorolt hatás</span><span class="sxs-lookup"><span data-stu-id="ae4ae-262">Impact</span></span>|
| <span data-ttu-id="ae4ae-263">RequestedDate_t</span><span class="sxs-lookup"><span data-stu-id="ae4ae-263">RequestedDate_t</span></span>  | <span data-ttu-id="ae4ae-264">A kért dátum szerint</span><span class="sxs-lookup"><span data-stu-id="ae4ae-264">Requested by date</span></span> |
| <span data-ttu-id="ae4ae-265">ClosedDate_t</span><span class="sxs-lookup"><span data-stu-id="ae4ae-265">ClosedDate_t</span></span> | <span data-ttu-id="ae4ae-266">Lezárás dátuma</span><span class="sxs-lookup"><span data-stu-id="ae4ae-266">Closed date</span></span> |
| <span data-ttu-id="ae4ae-267">PlannedStartDate_t</span><span class="sxs-lookup"><span data-stu-id="ae4ae-267">PlannedStartDate_t</span></span>  |     <span data-ttu-id="ae4ae-268">Tervezett kezdő dátuma</span><span class="sxs-lookup"><span data-stu-id="ae4ae-268">Planned start date</span></span> |
| <span data-ttu-id="ae4ae-269">PlannedEndDate_t</span><span class="sxs-lookup"><span data-stu-id="ae4ae-269">PlannedEndDate_t</span></span>  |   <span data-ttu-id="ae4ae-270">Tervezett befejezési dátum</span><span class="sxs-lookup"><span data-stu-id="ae4ae-270">Planned end date</span></span> |
| <span data-ttu-id="ae4ae-271">WorkStartDate_t</span><span class="sxs-lookup"><span data-stu-id="ae4ae-271">WorkStartDate_t</span></span>  | <span data-ttu-id="ae4ae-272">Tényleges kezdési dátuma</span><span class="sxs-lookup"><span data-stu-id="ae4ae-272">Actual start date</span></span> |
| <span data-ttu-id="ae4ae-273">WorkEndDate_t</span><span class="sxs-lookup"><span data-stu-id="ae4ae-273">WorkEndDate_t</span></span> | <span data-ttu-id="ae4ae-274">Tényleges befejezési dátum</span><span class="sxs-lookup"><span data-stu-id="ae4ae-274">Actual end date</span></span>|
| <span data-ttu-id="ae4ae-275">Description_s</span><span class="sxs-lookup"><span data-stu-id="ae4ae-275">Description_s</span></span> | <span data-ttu-id="ae4ae-276">Leírás</span><span class="sxs-lookup"><span data-stu-id="ae4ae-276">Description</span></span> |
| <span data-ttu-id="ae4ae-277">Computer</span><span class="sxs-lookup"><span data-stu-id="ae4ae-277">Computer</span></span>  | <span data-ttu-id="ae4ae-278">Konfigurációs elem</span><span class="sxs-lookup"><span data-stu-id="ae4ae-278">Configuration Item</span></span> |

<span data-ttu-id="ae4ae-279">**A minta Naplóelemzési képernyő ITSM adatokat:**</span><span class="sxs-lookup"><span data-stu-id="ae4ae-279">**Sample Log Analytics screen for ITSM data:**</span></span>

![Napló elemzési képernyő](./media/log-analytics-itsmc/itsmc-overview-sample-log-analytics.png)

## <a name="it-service-management-connector--integration-with-other-oms-solutions"></a><span data-ttu-id="ae4ae-281">Informatikai szolgáltatás Management-összekötő – integrálva más OMS-megoldásokkal</span><span class="sxs-lookup"><span data-stu-id="ae4ae-281">IT Service Management connector – integration with other OMS solutions</span></span>

<span data-ttu-id="ae4ae-282">Informatikai szolgáltatás Management-összekötő jelenleg a Service Map megoldással való integráció.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-282">IT Service Management Connector, currently supports integration with the Service Map solution.</span></span>

<span data-ttu-id="ae4ae-283">Szolgáltatástérkép automatikusan észleli az alkalmazás-összetevők, a Windows és Linux rendszerek, és leképezi a szolgáltatások közötti kommunikáció.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-283">Service Map automatically discovers the application components on Windows and Linux systems and maps the communication between services.</span></span> <span data-ttu-id="ae4ae-284">Lehetővé teszi a kiszolgálók, úgy gondolja, hogy azok –, hogy a kritikus szolgáltatások összekapcsolt rendszerként.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-284">It allows you to view your servers as you think of them – as interconnected systems that deliver critical services.</span></span> <span data-ttu-id="ae4ae-285">Szolgáltatástérkép jeleníti meg a kiszolgálók, a folyamatok közötti kapcsolatokat, és portok között bármely TCP-csatlakoztatott architektúra a konfiguráció nem szükséges másik ügynököt telepíteni.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-285">Service Map shows connections between servers, processes, and ports across any TCP-connected architecture with no configuration required other than installation of an agent.</span></span> <span data-ttu-id="ae4ae-286">További információ: [Szolgáltatástérkép](../operations-management-suite/operations-management-suite-service-map.md).</span><span class="sxs-lookup"><span data-stu-id="ae4ae-286">More information: [Service Map](../operations-management-suite/operations-management-suite-service-map.md).</span></span>

<span data-ttu-id="ae4ae-287">Ez az integráció tekintheti meg a szolgáltatás ügyfélszolgálati létrehozott elemek szerepelnek a ITSM megoldások a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="ae4ae-287">With this integration, you can view the service desk items created in the ITSM solutions as shown in the following example:</span></span>

![<span data-ttu-id="ae4ae-288">Az integrált megoldást</span><span class="sxs-lookup"><span data-stu-id="ae4ae-288">Integrated solution</span></span> ](./media/log-analytics-itsmc/itsmc-overview-integrated-solutions.png)
## <a name="create-itsm-work-items-for-oms-alerts"></a><span data-ttu-id="ae4ae-289">Az OMS-értesítések ITSM munkaelemek létrehozása</span><span class="sxs-lookup"><span data-stu-id="ae4ae-289">Create ITSM work items for OMS alerts</span></span>

<span data-ttu-id="ae4ae-290">Az OMS riasztásokhoz kapcsolódó munkaelemek a csatlakoztatott ITSM forrásokban is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-290">For the OMS alerts, you can create associated work items in the connected ITSM sources.</span></span>  <span data-ttu-id="ae4ae-291">Ehhez használja az alábbi eljárást:</span><span class="sxs-lookup"><span data-stu-id="ae4ae-291">To do this, use the following procedure:</span></span>

1. <span data-ttu-id="ae4ae-292">A **naplófájl-keresési** ablakban futtassa a napló keresési lekérdezés adatainak megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-292">From **Log Search** window, run a log search query to view data.</span></span> <span data-ttu-id="ae4ae-293">Lekérdezés eredményei munkaelemek forrását.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-293">Query results are the source for work items.</span></span>
2. <span data-ttu-id="ae4ae-294">A **naplófájl-keresési**, kattintson a **riasztási** megnyitásához a **riasztási szabály hozzáadása** lap.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-294">In **Log Search**, click **Alert** to open the **Add Alert Rule** page.</span></span>

    ![Napló elemzési képernyő](./media/log-analytics-itsmc/itsmc-work-items-for-oms-alerts.png)

3. <span data-ttu-id="ae4ae-296">Az a **riasztási szabály hozzáadása** ablakban adja meg a szükséges adatokat a **neve**, **súlyossági**, **keresési lekérdezés**, és **riasztás feltételek** (ablakban/Időmetrika mérési).</span><span class="sxs-lookup"><span data-stu-id="ae4ae-296">On the **Add Alert Rule** window, provide the required details for **Name**, **Severity**,  **Search query**, and **Alert criteria** (Time Window/Metric measurement).</span></span>
4. <span data-ttu-id="ae4ae-297">Válassza ki **Igen** a **ITSM műveletek**.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-297">Select **Yes** for **ITSM Actions**.</span></span>
5. <span data-ttu-id="ae4ae-298">Válassza ki a ITSM kapcsolatot a **válassza ki a kapcsolat** listája.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-298">Select your ITSM connection from the **Select Connection** list.</span></span>
6. <span data-ttu-id="ae4ae-299">Adja meg szükség szerint adatait.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-299">Provide the details as required.</span></span>
7. <span data-ttu-id="ae4ae-300">Minden naplóbejegyzés a riasztás egy külön munkaelemet létrehozásához válassza a **minden naplóbejegyzés egyes munkaelemek létrehozása** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-300">To create a separate work item for each log entry of this alert, select the **Create individual work items for each log entry** checkbox.</span></span>

    <span data-ttu-id="ae4ae-301">Vagy</span><span class="sxs-lookup"><span data-stu-id="ae4ae-301">Or</span></span>

    <span data-ttu-id="ae4ae-302">hagyja üresen ezt a jelölőnégyzetet, jelölje ki az ebben a riasztásban vonta naplóbejegyzések tetszőleges számú csak egy munkaelem létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-302">leave this checkbox unselected to create only one work item for any number of log entries under this alert.</span></span>

7. <span data-ttu-id="ae4ae-303">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-303">Click **Save**.</span></span>

<span data-ttu-id="ae4ae-304">Az OMS-riasztás alapján jön létre **riasztások**.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-304">The OMS alert will be created under **Alerts**.</span></span> <span data-ttu-id="ae4ae-305">A megfelelő ITSM kapcsolat munkaelemek jönnek létre, ha a megadott riasztás feltétel teljesül.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-305">The corresponding ITSM connection's work items are created when the specified alert's condition is met.</span></span>

## <a name="create-itsm-work-items-from-oms-logs"></a><span data-ttu-id="ae4ae-306">Az OMS-naplók ITSM munkaelemek létrehozása</span><span class="sxs-lookup"><span data-stu-id="ae4ae-306">Create ITSM work items from OMS logs</span></span>

<span data-ttu-id="ae4ae-307">A csatlakoztatott ITSM forrásokban munkaelemek OMS napló keresési használatával hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-307">You can create work items in the connected ITSM sources by using OMS Log Search.</span></span> <span data-ttu-id="ae4ae-308">Ehhez használja az alábbi eljárást:</span><span class="sxs-lookup"><span data-stu-id="ae4ae-308">To do this, use the following procedure:</span></span>

1. <span data-ttu-id="ae4ae-309">A **naplófájl-keresési**, keresse meg a szükséges adatokat, válassza ki a részleteket, és kattintson **létrehozás munkaelem**.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-309">From **Log Search**,  search the required data, select the detail, and click **Create work item**.</span></span>

    <span data-ttu-id="ae4ae-310">A **ITSM munkaelem létrehozása** ablak jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="ae4ae-310">The **Create ITSM Work item** window appears:</span></span>

    ![Napló elemzési képernyő](media/log-analytics-itsmc/itsmc-work-items-from-oms-logs.png)

2.   <span data-ttu-id="ae4ae-312">Adja hozzá a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="ae4ae-312">Add the following details:</span></span>

  - <span data-ttu-id="ae4ae-313">**Munkaelem-cím**: a munkaelem címét.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-313">**Work item Title**: Title for the work item.</span></span>
  - <span data-ttu-id="ae4ae-314">**Munkaelem-leírás**: az új munkaelemhez leírását.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-314">**Work item Description**: Description for the new work item.</span></span>
  - <span data-ttu-id="ae4ae-315">**Érintett számítógép**: Ha a napló adatokat talált a számítógép nevét.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-315">**Affected Computer**: Name of the computer where this log data was found.</span></span>
  - <span data-ttu-id="ae4ae-316">**Válassza ki a kapcsolat**: ITSM kapcsolat, amelyben szeretné létrehozni ezt a munkaelemet.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-316">**Select Connection**:  ITSM connection in which you want to create this work item.</span></span>
  - <span data-ttu-id="ae4ae-317">**Munkaelem**: típusú munkaelemet.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-317">**Work item**:  Type of work item.</span></span>

3. <span data-ttu-id="ae4ae-318">Incidens egy meglévő munkaelemsablonból használatához kattintson **Igen** alatt **Generate elemet a sablon alapján** lehetőséget, majd kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-318">To use an existing work item template for an incident, click **Yes** under **Generate work item based on the template** option and then click **Create**.</span></span>

    <span data-ttu-id="ae4ae-319">Vagy</span><span class="sxs-lookup"><span data-stu-id="ae4ae-319">Or,</span></span>

    <span data-ttu-id="ae4ae-320">Kattintson a **nem** Ha lehetővé szeretné tenni a szabott értékekhez.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-320">Click **No** if you want to provide your customized values.</span></span>

4. <span data-ttu-id="ae4ae-321">Adja meg a megfelelő értékeket a **ügyfél típusú**, **hatás**, **sürgősség**, **kategória**, és **alkategória** szövegmezőbe, és kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-321">Provide the appropriate values in the **Contact Type**, **Impact**, **Urgency**, **Category**, and **Sub Category** text boxes, and then click **Create**.</span></span>

<span data-ttu-id="ae4ae-322">A munkaelem létrejön a ITSM, amelyen megtekintheti az OMS Szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-322">The work item will be created in the ITSM, which you can also view in OMS.</span></span>

## <a name="troubleshoot-itsm-connections-in-oms"></a><span data-ttu-id="ae4ae-323">Az OMS ITSM kapcsolatok hibáinak elhárítása</span><span class="sxs-lookup"><span data-stu-id="ae4ae-323">Troubleshoot ITSM connections in OMS</span></span>
1.  <span data-ttu-id="ae4ae-324">Ha a csatlakoztatott adatforrás felhasználói Felületről való kapcsolódás sikertelen, és kapott a **hiba történt a kapcsolat mentése** üzenet, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="ae4ae-324">If connection fails from connected source's UI and you get the **Error in saving connection** message, do the following:</span></span>
 - <span data-ttu-id="ae4ae-325">A ServiceNow, Cherwell és Provance kapcsolatok esetén győződjön meg arról megfelelően a felhasználónév/jelszó és az ügyfél azonosítója/ügyfélkulcs az egyes kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-325">In case of ServiceNow, Cherwell and Provance connections, ensure you correctly entered  the username/password and  client ID/client secret  for each of the connections.</span></span> <span data-ttu-id="ae4ae-326">Ha a probléma továbbra is fennáll, ellenőrizze, ha a megfelelő engedélyekkel rendelkezik a megfelelő ITSM termékben való csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-326">If the error persists, check if you have sufficient privileges  in the corresponding ITSM product to make the connection.</span></span>
 - <span data-ttu-id="ae4ae-327">Esetén Service Manager alkalmazást, győződjön meg arról, hogy a webalkalmazás telepítése sikeres volt, és a hibrid kapcsolat jön létre.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-327">In case of Service Manager, ensure that the Web app is successfully deployed and hybrid connection is created.</span></span> <span data-ttu-id="ae4ae-328">Ellenőrizze, hogy sikeresen létrejött a kapcsolat a helyszíni Service Manager számítógéppel, látogasson el a webes alkalmazás URL-CÍMÉT, hogy dokumentációjában ismertetett módon a [a hibrid kapcsolat](log-analytics-itsmc-connections.md#configure-the-hybrid-connection).</span><span class="sxs-lookup"><span data-stu-id="ae4ae-328">To verify the connection is successfully established with the on-prem Service Manager machine, visit the  Web app URL as detailed in the documentation for making the [hybrid connection](log-analytics-itsmc-connections.md#configure-the-hybrid-connection).</span></span>

2.  <span data-ttu-id="ae4ae-329">Ha ServiceNow adatait van nem történt-e szinkronizálva az OMS, győződjön meg arról, hogy a példány nem alszik ServiceNow.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-329">If data from ServiceNow is not getting synced in OMS, ensure that the ServiceNow instance is not sleeping.</span></span> <span data-ttu-id="ae4ae-330">Ez egy kis ideig fordulhat elő, a ServiceNow fejlesztői esetekben üresjáratban.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-330">This might sometime happen in the ServiceNow Dev instances, when idle.</span></span> <span data-ttu-id="ae4ae-331">Más jelentse a hibát.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-331">Else, report the issue.</span></span>
3.  <span data-ttu-id="ae4ae-332">Ha riasztások elindulása esetén első az OMS Szolgáltatáshoz, de a munkaelemek nem ITSM első létrehozott termék vagy a konfigurációs elemek nem kapják meg a létrehozott/kapcsolódó munkaelemek vagy bármely általános információkat, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="ae4ae-332">If Alerts are getting fired from OMS but work items are not getting created in ITSM product or configuration items are not getting created/linked to work items or for any generic information, do the following:</span></span>
 -  <span data-ttu-id="ae4ae-333">Informatikai Management-összekötő megoldás az OMS-portálon is használható a beolvasandó, kapcsolatok munkahelyi elemek/számítógépek stb. A hibaüzenet a állapot paneljén kattintson, keresse meg **naplófájl-keresési** és a kapcsolat, amely rendelkezik a hiba részleteit a hibaüzenet megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-333">IT Service Management Connector solution in OMS portal could be used to get a summary of connections/work items/computers etc. Click the error message in the status blade, navigate to **Log Search** and view the connection that has the error by using the details in the error message.</span></span>
 - <span data-ttu-id="ae4ae-334">közvetlenül megtekintheti a hibák/kapcsolatos információk a **naplófájl-keresési** használatával lapon *típus = ServiceDeskLog_CL*.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-334">you can directly view the errors/related information in the **Log Search** page using *Type=ServiceDeskLog_CL*.</span></span>

## <a name="troubleshoot-service-manager-web-app-deployment"></a><span data-ttu-id="ae4ae-335">A Service Manager webes alkalmazás telepítésének hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="ae4ae-335">Troubleshoot Service Manager Web App deployment</span></span>
1.  <span data-ttu-id="ae4ae-336">Esetén a webes alkalmazás központi telepítési problémák léptek fel győződjön meg arról, hogy megfelelő engedélyekkel rendelkezik az előfizetés említett erőforrások létrehozása vagy telepítése.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-336">In case of any trouble with web app deployment, ensure you have sufficient permissions in the subscription mentioned to create/deploy resources.</span></span>
2.  <span data-ttu-id="ae4ae-337">Ha **hivatkozás nincs beállítva egy objektumpéldányra objektum** futtatása során hibaüzenet jelenik meg a [parancsfájl](log-analytics-itsmc-service-manager-script.md) győződjön meg arról, hogy az érvényes értékek **felhasználói konfiguráció** a szakasz.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-337">If **Object reference not set to instance of an object** error message appears while running the [script](log-analytics-itsmc-service-manager-script.md) ensure that you entered valid values  under **User Configuration** section.</span></span>
3.  <span data-ttu-id="ae4ae-338">Ha Ön nem tudja létrehozni a service bus relay-névtér, győződjön meg arról, hogy a szükséges erőforrás-szolgáltató regisztrálva van az előfizetés.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-338">If you fail to create service bus relay namespace, ensure that the required resource provider is registered in the subscription.</span></span> <span data-ttu-id="ae4ae-339">Ha nem regisztrált, manuálisan hozza létre az Azure portálról.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-339">If not registered, manually create it from the Azure portal.</span></span> <span data-ttu-id="ae4ae-340">Akkor is létrehozható közben [a hibrid kapcsolat létrehozása](log-analytics-itsmc-connections.md#configure-the-hybrid-connection) Azure-portálról.</span><span class="sxs-lookup"><span data-stu-id="ae4ae-340">You can also create it while [creating the hybrid connection](log-analytics-itsmc-connections.md#configure-the-hybrid-connection) from the Azure portal.</span></span>


## <a name="contact-us"></a><span data-ttu-id="ae4ae-341">Kapcsolat</span><span class="sxs-lookup"><span data-stu-id="ae4ae-341">Contact us</span></span>

<span data-ttu-id="ae4ae-342">A lekérdezést vagy az informatikai szolgáltatás Management-összekötő visszajelzést, lépjen velünk kapcsolatba [ omsitsmfeedback@microsoft.com ](mailto:omsitsmfeedback@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="ae4ae-342">For any queries or feedback on the IT Service Management Connector, contact us at [omsitsmfeedback@microsoft.com](mailto:omsitsmfeedback@microsoft.com).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ae4ae-343">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ae4ae-343">Next steps</span></span>
<span data-ttu-id="ae4ae-344">[ITSM termékek és szolgáltatások hozzáadása IT Service Management-összekötő](log-analytics-itsmc-connections.md).</span><span class="sxs-lookup"><span data-stu-id="ae4ae-344">[Add ITSM products/services to IT Service Management Connector](log-analytics-itsmc-connections.md).</span></span>
