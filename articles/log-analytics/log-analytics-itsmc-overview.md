---
title: "Service Management-összekötő az OMS aaaIT |} Microsoft Docs"
description: "Hello IT Service Management-összekötő toocentrally figyelheti és hello OMS ITSM munkaelemek kezeléséhez, és gyorsan hárítson el minden problémát."
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
ms.openlocfilehash: 33ed5d432591b836eb41ba982c66c96f22879444
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="centrally-manage-itsm-work-items-using-it-service-management-connector-preview"></a><span data-ttu-id="df5f3-103">Központi kezelését az informatikai szolgáltatás Management Connector (előzetes verzió) segítségével ITSM munkaelemek</span><span class="sxs-lookup"><span data-stu-id="df5f3-103">Centrally manage ITSM work items using IT Service Management Connector (Preview)</span></span>

![Informatikai Management-összekötő szimbólum](./media/log-analytics-itsmc/itsmc-symbol.png)

<span data-ttu-id="df5f3-105">Hello IT Service Management Connector (ITSMC) az OMS szolgáltatáshoz toocentrally monitor használata, és a munkaelemek kezelésére a ITSM termékek vagy szolgáltatások között.</span><span class="sxs-lookup"><span data-stu-id="df5f3-105">You can use hello IT Service Management Connector (ITSMC) in OMS Log Analytics toocentrally monitor and manage work items across your ITSM products/services.</span></span>

<span data-ttu-id="df5f3-106">hello IT Service Management-összekötő integrálható a meglévő IT Service Management (ITSM) termékek és szolgáltatások a OMS szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="df5f3-106">hello IT Service Management Connector integrates your existing IT Service Management (ITSM) products and services with OMS Log Analytics.</span></span>  <span data-ttu-id="df5f3-107">hello megoldás ITSM termékek vagy szolgáltatások kétirányú integrációt, akkor ha biztosít hello OMS felhasználók egy beállítás toocreate incidensek, a riasztásokat vagy a események ITSM megoldásban.</span><span class="sxs-lookup"><span data-stu-id="df5f3-107">hello solution has bidirectional integration with ITSM products/services, where it provides hello OMS users an option toocreate incidents, alerts, or events in ITSM solution.</span></span> <span data-ttu-id="df5f3-108">hello összekötő is különféle adatok, például incidensek és változáskérések ITSM megoldásból az OMS szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="df5f3-108">hello connector also  imports data such as incidents, and change requests from ITSM solution into OMS Log Analytics.</span></span>

<span data-ttu-id="df5f3-109">Az informatikai szolgáltatás Management-összekötő segítségével:</span><span class="sxs-lookup"><span data-stu-id="df5f3-109">With IT Service Management Connector, you can:</span></span>

  - <span data-ttu-id="df5f3-110">Központilag figyelheti és ITSM termékek vagy szolgáltatások a szervezetben használt munkaelemek kezelésére.</span><span class="sxs-lookup"><span data-stu-id="df5f3-110">Centrally monitor and manage work items for ITSM products/services used across your organization.</span></span>
  - <span data-ttu-id="df5f3-111">Hozzon létre ITSM munkaelemeket (például a riasztást, esemény, incidens) ITSM az OMS-riasztások és a keresési napló.</span><span class="sxs-lookup"><span data-stu-id="df5f3-111">Create ITSM work items (like alert, event, incident) in ITSM from OMS alerts and through log search.</span></span>
  - <span data-ttu-id="df5f3-112">Olvassa el az incidensek és változáskérések az ITSM megoldásból, és vonatkozó naplóadatok Naplóelemzési munkaterület függ.</span><span class="sxs-lookup"><span data-stu-id="df5f3-112">Read incidents and change requests from your ITSM solution and correlate with relevant log data in Log Analytics workspace.</span></span>
  - <span data-ttu-id="df5f3-113">Váratlan és szokatlan eseményeket található, és a megoldásukkal együtt, még mielőtt a végfelhasználók hello hívja, és jelentést őket toohello segélyszolgálat.</span><span class="sxs-lookup"><span data-stu-id="df5f3-113">Find any unexpected and unusual events and resolve them, even before hello end users call and report them toohello helpdesk.</span></span>
  - <span data-ttu-id="df5f3-114">Munkahelyi elemek adatok importálása a Naplóelemzési és fő teljesítménymutatók (KPI) jelentések létrehozása.</span><span class="sxs-lookup"><span data-stu-id="df5f3-114">Import work items data into Log Analytics and create key performance indicator (KPI) reports.</span></span>  <span data-ttu-id="df5f3-115">Ezekre a jelentésekre, segítségével azonosíthatja, értékeléséhez és számos fontos elemek, például a kártevő szoftverek assessment működjön.</span><span class="sxs-lookup"><span data-stu-id="df5f3-115">Using these reports, you can identify, assess and act on several important items such as malware assessment.</span></span>
  - <span data-ttu-id="df5f3-116">Részletesebben is elemezheti válogatott irányítópultok incidensek megtekintéséhez a változáskérésekhez és az érintett rendszerek.</span><span class="sxs-lookup"><span data-stu-id="df5f3-116">View curated dashboards for deeper insights on incidents, change requests and impacted systems.</span></span>
  - <span data-ttu-id="df5f3-117">Végezzen hibaelhárítást a gyorsabb egyéb megoldások hello Naplóelemzési munkaterület az adatok.</span><span class="sxs-lookup"><span data-stu-id="df5f3-117">Troubleshoot faster by correlating with other management solutions in hello Log Analytics workspace.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="df5f3-118">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="df5f3-118">Prerequisites</span></span>

<span data-ttu-id="df5f3-119">tooimport hello ITSM munkaelemek az OMS szolgáltatáshoz, hello megoldás hello IT Service Management-összekötő az hello OMS és hello IT Service Manager termék vagy szolgáltatás, ahonnan importálni hello munkaelemek közötti kapcsolat szükséges.</span><span class="sxs-lookup"><span data-stu-id="df5f3-119">tooimport hello ITSM work items into OMS Log Analytics, hello solution requires a connection between hello IT Service Management Connector in hello OMS and hello IT SM product/service from which you import hello work items.</span></span>


## <a name="configuration"></a><span data-ttu-id="df5f3-120">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="df5f3-120">Configuration</span></span>

<span data-ttu-id="df5f3-121">Hozzáadás hello IT Service Management-összekötő megoldás tooyour OMS munkaterület, ismertetett hello eljárással [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjtemény](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="df5f3-121">Add hello IT Service Management Connector solution tooyour OMS work space, using hello process described in [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md).</span></span>

<span data-ttu-id="df5f3-122">Informatikai szolgáltatás Management-összekötő csempe hello megoldások katalógusban látható módon:</span><span class="sxs-lookup"><span data-stu-id="df5f3-122">IT Service Management Connector tile as you see in hello Solutions gallery:</span></span>

![összekötő csempe](./media/log-analytics-itsmc/itsmc-solutions-tile.png)

<span data-ttu-id="df5f3-124">Sikeres hozzáadása után megjelenik IT Service Management-összekötő a hello **OMS** > **beállítások** > **csatlakoztatott források.**</span><span class="sxs-lookup"><span data-stu-id="df5f3-124">After successful addition, you will see hello IT Service Management Connector under **OMS** > **Settings** > **Connected Sources.**</span></span>

![Csatlakoztatott ITSMC](./media/log-analytics-itsmc/itsmc-overview-solution-in-connected-sources.png)

> [!NOTE]

> <span data-ttu-id="df5f3-126">Alapértelmezés szerint a hello IT Service Management-összekötő frissítése 24 óránként egyszer a hello kapcsolati adatait.</span><span class="sxs-lookup"><span data-stu-id="df5f3-126">By default, hello IT Service Management Connector refreshes hello connection's data once in every 24 hours.</span></span> <span data-ttu-id="df5f3-127">toorefresh a kapcsolat azonnal az esetleges módosításokat, vagy a sablon adatfrissítések, amelyek miatt, kattintson a hello frissítési gomb látható következő tooyour kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="df5f3-127">toorefresh your connection's data instantly for any edits or template updates that you make, click hello refresh button displayed next tooyour connection.</span></span>

 ![ITSMC frissítése](./media/log-analytics-itsmc/itsmc-connection-refresh.png)

## <a name="management-packs"></a><span data-ttu-id="df5f3-129">Felügyeleti csomagok</span><span class="sxs-lookup"><span data-stu-id="df5f3-129">Management packs</span></span>
<span data-ttu-id="df5f3-130">Ez a megoldás nem igényel minden olyan felügyeleti csomagot.</span><span class="sxs-lookup"><span data-stu-id="df5f3-130">This solution does not require any management packs.</span></span>

## <a name="connected-sources"></a><span data-ttu-id="df5f3-131">Összekapcsolt források</span><span class="sxs-lookup"><span data-stu-id="df5f3-131">Connected sources</span></span>

<span data-ttu-id="df5f3-132">hello következő ITSM termékek és szolgáltatások által támogatott IT Service Management-összekötő hello:</span><span class="sxs-lookup"><span data-stu-id="df5f3-132">hello following ITSM products/services are supported by hello IT Service Management Connector:</span></span>

- [<span data-ttu-id="df5f3-133">A System Center Service Manager (SCSM)</span><span class="sxs-lookup"><span data-stu-id="df5f3-133">System Center Service Manager (SCSM)</span></span>](log-analytics-itsmc-connections.md#connect-system-center-service-manager-to-it-service-management-connector-in-oms)

- [<span data-ttu-id="df5f3-134">A ServiceNow</span><span class="sxs-lookup"><span data-stu-id="df5f3-134">ServiceNow</span></span>](log-analytics-itsmc-connections.md#connect-servicenow-to-it-service-management-connector-in-oms)

- [<span data-ttu-id="df5f3-135">Provance</span><span class="sxs-lookup"><span data-stu-id="df5f3-135">Provance</span></span>](log-analytics-itsmc-connections.md#connect-provance-to-it-service-management-connector-in-oms)  

- [<span data-ttu-id="df5f3-136">Cherwell</span><span class="sxs-lookup"><span data-stu-id="df5f3-136">Cherwell</span></span>](log-analytics-itsmc-connections.md#connect-cherwell-to-it-service-management-connector-in-oms)

## <a name="using-hello-solution"></a><span data-ttu-id="df5f3-137">Hello megoldással</span><span class="sxs-lookup"><span data-stu-id="df5f3-137">Using hello solution</span></span>

<span data-ttu-id="df5f3-138">Hello OMS IT Service Management-összekötő csatlakozott a ITSM szolgáltatással, miután a hello Naplóelemzési szolgáltatások hello adatgyűjtés csatlakoztatott hello ITSM termékek/szolgáltatás elindul.</span><span class="sxs-lookup"><span data-stu-id="df5f3-138">Once you connect hello OMS IT Service Management Connector with your ITSM service, hello Log Analytics services starts gathering hello data from hello connected ITSM products/service.</span></span>

> [!NOTE]
> - <span data-ttu-id="df5f3-139">Log Analytics nevű eseményként is megjelenik megoldás IT Service Management-összekötő által importált adatok **ServiceDesk_CL**.</span><span class="sxs-lookup"><span data-stu-id="df5f3-139">Data imported by IT Service Management Connector solution appears in Log Analytics as events named **ServiceDesk_CL**.</span></span>
- <span data-ttu-id="df5f3-140">Esemény nevű mezőt tartalmaz **ServiceDeskWorkItemType_s**.</span><span class="sxs-lookup"><span data-stu-id="df5f3-140">Event contains a field named **ServiceDeskWorkItemType_s**.</span></span> <span data-ttu-id="df5f3-141">amely incidens, mint az értékét vagy változáskérés is magával attól függően, hogy hello munkaelem hello szereplő adatok **ServiceDesk_CL** esemény.</span><span class="sxs-lookup"><span data-stu-id="df5f3-141">which can take its value as incident, or change request, depending on hello work item data contained in hello **ServiceDesk_CL** event.</span></span>

## <a name="input-data"></a><span data-ttu-id="df5f3-142">A bemeneti adatok</span><span class="sxs-lookup"><span data-stu-id="df5f3-142">Input data</span></span>
<span data-ttu-id="df5f3-143">A munkaelemek hello ITSM termékek vagy szolgáltatások importálására.</span><span class="sxs-lookup"><span data-stu-id="df5f3-143">Work items imported from hello ITSM products/services.</span></span>

<span data-ttu-id="df5f3-144">hello alábbi információkat jeleníti meg hello IT Service Management-összekötő által összegyűjtött adatok:</span><span class="sxs-lookup"><span data-stu-id="df5f3-144">hello following information shows examples of data gathered by hello IT Service Management connector:</span></span>

> [!NOTE]
> <span data-ttu-id="df5f3-145">Attól függően, hogy hello munkaelem típusa Log Analyticshez importálni **ServiceDesk_CL** hello a következő mezőket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="df5f3-145">Depending on hello work item type imported into Log Analytics, **ServiceDesk_CL** contains hello following fields:</span></span>

<span data-ttu-id="df5f3-146">**Munkaelem:** **incidensek**</span><span class="sxs-lookup"><span data-stu-id="df5f3-146">**Work item:** **Incidents**</span></span>  
<span data-ttu-id="df5f3-147">ServiceDeskWorkItemType_s = "Esemény"</span><span class="sxs-lookup"><span data-stu-id="df5f3-147">ServiceDeskWorkItemType_s="Incident"</span></span>

<span data-ttu-id="df5f3-148">**Mezők**</span><span class="sxs-lookup"><span data-stu-id="df5f3-148">**Fields**</span></span>

- <span data-ttu-id="df5f3-149">ServiceDeskConnectionName</span><span class="sxs-lookup"><span data-stu-id="df5f3-149">ServiceDeskConnectionName</span></span>
- <span data-ttu-id="df5f3-150">Szolgáltatás ügyfélszolgálati azonosítója</span><span class="sxs-lookup"><span data-stu-id="df5f3-150">Service Desk ID</span></span>
- <span data-ttu-id="df5f3-151">Állapot</span><span class="sxs-lookup"><span data-stu-id="df5f3-151">State</span></span>
- <span data-ttu-id="df5f3-152">Sürgős</span><span class="sxs-lookup"><span data-stu-id="df5f3-152">Urgency</span></span>
- <span data-ttu-id="df5f3-153">Gyakorolt hatás</span><span class="sxs-lookup"><span data-stu-id="df5f3-153">Impact</span></span>
- <span data-ttu-id="df5f3-154">Prioritás</span><span class="sxs-lookup"><span data-stu-id="df5f3-154">Priority</span></span>
- <span data-ttu-id="df5f3-155">Eszkalációs</span><span class="sxs-lookup"><span data-stu-id="df5f3-155">Escalation</span></span>
- <span data-ttu-id="df5f3-156">Hozta létre</span><span class="sxs-lookup"><span data-stu-id="df5f3-156">Created By</span></span>
- <span data-ttu-id="df5f3-157">Megoldó</span><span class="sxs-lookup"><span data-stu-id="df5f3-157">Resolved By</span></span>
- <span data-ttu-id="df5f3-158">Lezárt</span><span class="sxs-lookup"><span data-stu-id="df5f3-158">Closed By</span></span>
- <span data-ttu-id="df5f3-159">Forrás</span><span class="sxs-lookup"><span data-stu-id="df5f3-159">Source</span></span>
- <span data-ttu-id="df5f3-160">Rendelt</span><span class="sxs-lookup"><span data-stu-id="df5f3-160">Assigned To</span></span>
- <span data-ttu-id="df5f3-161">Kategória</span><span class="sxs-lookup"><span data-stu-id="df5f3-161">Category</span></span>
- <span data-ttu-id="df5f3-162">Cím</span><span class="sxs-lookup"><span data-stu-id="df5f3-162">Title</span></span>
- <span data-ttu-id="df5f3-163">Leírás</span><span class="sxs-lookup"><span data-stu-id="df5f3-163">Description</span></span>
- <span data-ttu-id="df5f3-164">Létrehozás dátuma</span><span class="sxs-lookup"><span data-stu-id="df5f3-164">Created Date</span></span>
- <span data-ttu-id="df5f3-165">Lezárás dátuma</span><span class="sxs-lookup"><span data-stu-id="df5f3-165">Closed Date</span></span>
- <span data-ttu-id="df5f3-166">Megoldás dátuma</span><span class="sxs-lookup"><span data-stu-id="df5f3-166">Resolved Date</span></span>
- <span data-ttu-id="df5f3-167">Utolsó módosítás dátuma</span><span class="sxs-lookup"><span data-stu-id="df5f3-167">Last Modified Date</span></span>
- <span data-ttu-id="df5f3-168">Computer</span><span class="sxs-lookup"><span data-stu-id="df5f3-168">Computer</span></span>


<span data-ttu-id="df5f3-169">**Munkaelem:** **Változáskérések**</span><span class="sxs-lookup"><span data-stu-id="df5f3-169">**Work item:** **Change Requests**</span></span>

<span data-ttu-id="df5f3-170">ServiceDeskWorkItemType_s = "módosítási kérés"</span><span class="sxs-lookup"><span data-stu-id="df5f3-170">ServiceDeskWorkItemType_s="ChangeRequest"</span></span>

<span data-ttu-id="df5f3-171">**Mezők**</span><span class="sxs-lookup"><span data-stu-id="df5f3-171">**Fields**</span></span>
- <span data-ttu-id="df5f3-172">ServiceDeskConnectionName</span><span class="sxs-lookup"><span data-stu-id="df5f3-172">ServiceDeskConnectionName</span></span>
- <span data-ttu-id="df5f3-173">Szolgáltatás ügyfélszolgálati azonosítója</span><span class="sxs-lookup"><span data-stu-id="df5f3-173">Service Desk ID</span></span>
- <span data-ttu-id="df5f3-174">Hozta létre</span><span class="sxs-lookup"><span data-stu-id="df5f3-174">Created By</span></span>
- <span data-ttu-id="df5f3-175">Lezárt</span><span class="sxs-lookup"><span data-stu-id="df5f3-175">Closed By</span></span>
- <span data-ttu-id="df5f3-176">Forrás</span><span class="sxs-lookup"><span data-stu-id="df5f3-176">Source</span></span>
- <span data-ttu-id="df5f3-177">Rendelt</span><span class="sxs-lookup"><span data-stu-id="df5f3-177">Assigned To</span></span>
- <span data-ttu-id="df5f3-178">Cím</span><span class="sxs-lookup"><span data-stu-id="df5f3-178">Title</span></span>
- <span data-ttu-id="df5f3-179">Típus</span><span class="sxs-lookup"><span data-stu-id="df5f3-179">Type</span></span>
- <span data-ttu-id="df5f3-180">Kategória</span><span class="sxs-lookup"><span data-stu-id="df5f3-180">Category</span></span>
- <span data-ttu-id="df5f3-181">Állapot</span><span class="sxs-lookup"><span data-stu-id="df5f3-181">State</span></span>
- <span data-ttu-id="df5f3-182">Eszkalációs</span><span class="sxs-lookup"><span data-stu-id="df5f3-182">Escalation</span></span>
- <span data-ttu-id="df5f3-183">Ütközés állapota</span><span class="sxs-lookup"><span data-stu-id="df5f3-183">Conflict Status</span></span>
- <span data-ttu-id="df5f3-184">Sürgős</span><span class="sxs-lookup"><span data-stu-id="df5f3-184">Urgency</span></span>
- <span data-ttu-id="df5f3-185">Prioritás</span><span class="sxs-lookup"><span data-stu-id="df5f3-185">Priority</span></span>
- <span data-ttu-id="df5f3-186">Kockázati</span><span class="sxs-lookup"><span data-stu-id="df5f3-186">Risk</span></span>
- <span data-ttu-id="df5f3-187">Gyakorolt hatás</span><span class="sxs-lookup"><span data-stu-id="df5f3-187">Impact</span></span>
- <span data-ttu-id="df5f3-188">Rendelt</span><span class="sxs-lookup"><span data-stu-id="df5f3-188">Assigned To</span></span>
- <span data-ttu-id="df5f3-189">Létrehozás dátuma</span><span class="sxs-lookup"><span data-stu-id="df5f3-189">Created Date</span></span>
- <span data-ttu-id="df5f3-190">Lezárás dátuma</span><span class="sxs-lookup"><span data-stu-id="df5f3-190">Closed Date</span></span>
- <span data-ttu-id="df5f3-191">Utolsó módosítás dátuma</span><span class="sxs-lookup"><span data-stu-id="df5f3-191">Last Modified Date</span></span>
- <span data-ttu-id="df5f3-192">Kért dátuma</span><span class="sxs-lookup"><span data-stu-id="df5f3-192">Requested Date</span></span>
- <span data-ttu-id="df5f3-193">Tervezett kezdő dátuma</span><span class="sxs-lookup"><span data-stu-id="df5f3-193">Planned Start Date</span></span>
- <span data-ttu-id="df5f3-194">Tervezett befejezési dátum</span><span class="sxs-lookup"><span data-stu-id="df5f3-194">Planned End Date</span></span>
- <span data-ttu-id="df5f3-195">Munkahelyi kezdő dátuma</span><span class="sxs-lookup"><span data-stu-id="df5f3-195">Work Start Date</span></span>
- <span data-ttu-id="df5f3-196">Munka befejezési dátum</span><span class="sxs-lookup"><span data-stu-id="df5f3-196">Work End Date</span></span>
- <span data-ttu-id="df5f3-197">Leírás</span><span class="sxs-lookup"><span data-stu-id="df5f3-197">Description</span></span>
- <span data-ttu-id="df5f3-198">Computer</span><span class="sxs-lookup"><span data-stu-id="df5f3-198">Computer</span></span>

## <a name="output-data-for-a-servicenow-incident"></a><span data-ttu-id="df5f3-199">A ServiceNow, amelyhez a kimeneti adatok</span><span class="sxs-lookup"><span data-stu-id="df5f3-199">Output data for a ServiceNow incident</span></span>

| <span data-ttu-id="df5f3-200">OMS mező</span><span class="sxs-lookup"><span data-stu-id="df5f3-200">OMS field</span></span> | <span data-ttu-id="df5f3-201">ITSM mező</span><span class="sxs-lookup"><span data-stu-id="df5f3-201">ITSM field</span></span> |
|:--- |:--- |
| <span data-ttu-id="df5f3-202">ServiceDeskId_s</span><span class="sxs-lookup"><span data-stu-id="df5f3-202">ServiceDeskId_s</span></span>| <span data-ttu-id="df5f3-203">Szám</span><span class="sxs-lookup"><span data-stu-id="df5f3-203">Number</span></span> |
| <span data-ttu-id="df5f3-204">IncidentState_s</span><span class="sxs-lookup"><span data-stu-id="df5f3-204">IncidentState_s</span></span> | <span data-ttu-id="df5f3-205">Állapot</span><span class="sxs-lookup"><span data-stu-id="df5f3-205">State</span></span> |
| <span data-ttu-id="df5f3-206">Urgency_s</span><span class="sxs-lookup"><span data-stu-id="df5f3-206">Urgency_s</span></span> |<span data-ttu-id="df5f3-207">Sürgős</span><span class="sxs-lookup"><span data-stu-id="df5f3-207">Urgency</span></span> |
| <span data-ttu-id="df5f3-208">Impact_s</span><span class="sxs-lookup"><span data-stu-id="df5f3-208">Impact_s</span></span> |<span data-ttu-id="df5f3-209">Gyakorolt hatás</span><span class="sxs-lookup"><span data-stu-id="df5f3-209">Impact</span></span>|
| <span data-ttu-id="df5f3-210">Priority_s</span><span class="sxs-lookup"><span data-stu-id="df5f3-210">Priority_s</span></span> | <span data-ttu-id="df5f3-211">Prioritás</span><span class="sxs-lookup"><span data-stu-id="df5f3-211">Priority</span></span> |
| <span data-ttu-id="df5f3-212">CreatedBy_s</span><span class="sxs-lookup"><span data-stu-id="df5f3-212">CreatedBy_s</span></span> | <span data-ttu-id="df5f3-213">Által megnyitott</span><span class="sxs-lookup"><span data-stu-id="df5f3-213">Opened by</span></span> |
| <span data-ttu-id="df5f3-214">ResolvedBy_s</span><span class="sxs-lookup"><span data-stu-id="df5f3-214">ResolvedBy_s</span></span> | <span data-ttu-id="df5f3-215">Megoldó</span><span class="sxs-lookup"><span data-stu-id="df5f3-215">Resolved by</span></span>|
| <span data-ttu-id="df5f3-216">ClosedBy_s</span><span class="sxs-lookup"><span data-stu-id="df5f3-216">ClosedBy_s</span></span>  | <span data-ttu-id="df5f3-217">Lezárt</span><span class="sxs-lookup"><span data-stu-id="df5f3-217">Closed by</span></span> |
| <span data-ttu-id="df5f3-218">Source_s</span><span class="sxs-lookup"><span data-stu-id="df5f3-218">Source_s</span></span>| <span data-ttu-id="df5f3-219">Kapcsolat típusa</span><span class="sxs-lookup"><span data-stu-id="df5f3-219">Contact type</span></span> |
| <span data-ttu-id="df5f3-220">AssignedTo_s</span><span class="sxs-lookup"><span data-stu-id="df5f3-220">AssignedTo_s</span></span> | <span data-ttu-id="df5f3-221">Hozzárendelt túl</span><span class="sxs-lookup"><span data-stu-id="df5f3-221">Assigned too</span></span> |
| <span data-ttu-id="df5f3-222">Category_s</span><span class="sxs-lookup"><span data-stu-id="df5f3-222">Category_s</span></span> | <span data-ttu-id="df5f3-223">Kategória</span><span class="sxs-lookup"><span data-stu-id="df5f3-223">Category</span></span> |
| <span data-ttu-id="df5f3-224">Title_s</span><span class="sxs-lookup"><span data-stu-id="df5f3-224">Title_s</span></span>|  <span data-ttu-id="df5f3-225">Rövid leírás</span><span class="sxs-lookup"><span data-stu-id="df5f3-225">Short description</span></span> |
| <span data-ttu-id="df5f3-226">Description_s</span><span class="sxs-lookup"><span data-stu-id="df5f3-226">Description_s</span></span>|  <span data-ttu-id="df5f3-227">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="df5f3-227">Notes</span></span> |
| <span data-ttu-id="df5f3-228">CreatedDate_t</span><span class="sxs-lookup"><span data-stu-id="df5f3-228">CreatedDate_t</span></span>|  <span data-ttu-id="df5f3-229">Megnyitása</span><span class="sxs-lookup"><span data-stu-id="df5f3-229">Opened</span></span> |
| <span data-ttu-id="df5f3-230">ClosedDate_t</span><span class="sxs-lookup"><span data-stu-id="df5f3-230">ClosedDate_t</span></span>| <span data-ttu-id="df5f3-231">Lezárt</span><span class="sxs-lookup"><span data-stu-id="df5f3-231">closed</span></span>|
| <span data-ttu-id="df5f3-232">ResolvedDate_t</span><span class="sxs-lookup"><span data-stu-id="df5f3-232">ResolvedDate_t</span></span>|<span data-ttu-id="df5f3-233">Feloldva</span><span class="sxs-lookup"><span data-stu-id="df5f3-233">Resolved</span></span>|
| <span data-ttu-id="df5f3-234">Computer</span><span class="sxs-lookup"><span data-stu-id="df5f3-234">Computer</span></span>  | <span data-ttu-id="df5f3-235">konfigurációs elem</span><span class="sxs-lookup"><span data-stu-id="df5f3-235">Configuration item</span></span> |

## <a name="output-data-for-a-servicenow-change-request"></a><span data-ttu-id="df5f3-236">A ServiceNow kimeneti adatok változáskérés</span><span class="sxs-lookup"><span data-stu-id="df5f3-236">Output data for a ServiceNow change request</span></span>

| <span data-ttu-id="df5f3-237">OMS mező</span><span class="sxs-lookup"><span data-stu-id="df5f3-237">OMS field</span></span> | <span data-ttu-id="df5f3-238">ITSM mező</span><span class="sxs-lookup"><span data-stu-id="df5f3-238">ITSM field</span></span> |
|:--- |:--- |
| <span data-ttu-id="df5f3-239">ServiceDeskId_s</span><span class="sxs-lookup"><span data-stu-id="df5f3-239">ServiceDeskId_s</span></span>| <span data-ttu-id="df5f3-240">Szám</span><span class="sxs-lookup"><span data-stu-id="df5f3-240">Number</span></span> |
| <span data-ttu-id="df5f3-241">CreatedBy_s</span><span class="sxs-lookup"><span data-stu-id="df5f3-241">CreatedBy_s</span></span> | <span data-ttu-id="df5f3-242">Által kért</span><span class="sxs-lookup"><span data-stu-id="df5f3-242">Requested by</span></span> |
| <span data-ttu-id="df5f3-243">ClosedBy_s</span><span class="sxs-lookup"><span data-stu-id="df5f3-243">ClosedBy_s</span></span> | <span data-ttu-id="df5f3-244">Lezárt</span><span class="sxs-lookup"><span data-stu-id="df5f3-244">Closed by</span></span> |
| <span data-ttu-id="df5f3-245">AssignedTo_s</span><span class="sxs-lookup"><span data-stu-id="df5f3-245">AssignedTo_s</span></span> | <span data-ttu-id="df5f3-246">Hozzárendelt túl</span><span class="sxs-lookup"><span data-stu-id="df5f3-246">Assigned too</span></span> |
| <span data-ttu-id="df5f3-247">Title_s</span><span class="sxs-lookup"><span data-stu-id="df5f3-247">Title_s</span></span>|  <span data-ttu-id="df5f3-248">Rövid leírás</span><span class="sxs-lookup"><span data-stu-id="df5f3-248">Short description</span></span> |
| <span data-ttu-id="df5f3-249">Type_s</span><span class="sxs-lookup"><span data-stu-id="df5f3-249">Type_s</span></span>|  <span data-ttu-id="df5f3-250">Típus</span><span class="sxs-lookup"><span data-stu-id="df5f3-250">Type</span></span> |
| <span data-ttu-id="df5f3-251">Category_s</span><span class="sxs-lookup"><span data-stu-id="df5f3-251">Category_s</span></span>|  <span data-ttu-id="df5f3-252">Catgory</span><span class="sxs-lookup"><span data-stu-id="df5f3-252">Catgory</span></span> |
| <span data-ttu-id="df5f3-253">CRState_s</span><span class="sxs-lookup"><span data-stu-id="df5f3-253">CRState_s</span></span>|  <span data-ttu-id="df5f3-254">Állapot</span><span class="sxs-lookup"><span data-stu-id="df5f3-254">State</span></span>|
| <span data-ttu-id="df5f3-255">Urgency_s</span><span class="sxs-lookup"><span data-stu-id="df5f3-255">Urgency_s</span></span>|  <span data-ttu-id="df5f3-256">Sürgős</span><span class="sxs-lookup"><span data-stu-id="df5f3-256">Urgency</span></span> |
| <span data-ttu-id="df5f3-257">Priority_s</span><span class="sxs-lookup"><span data-stu-id="df5f3-257">Priority_s</span></span>| <span data-ttu-id="df5f3-258">Prioritás</span><span class="sxs-lookup"><span data-stu-id="df5f3-258">Priority</span></span>|
| <span data-ttu-id="df5f3-259">Risk_s</span><span class="sxs-lookup"><span data-stu-id="df5f3-259">Risk_s</span></span>| <span data-ttu-id="df5f3-260">Kockázati</span><span class="sxs-lookup"><span data-stu-id="df5f3-260">Risk</span></span>|
| <span data-ttu-id="df5f3-261">Impact_s</span><span class="sxs-lookup"><span data-stu-id="df5f3-261">Impact_s</span></span>| <span data-ttu-id="df5f3-262">Gyakorolt hatás</span><span class="sxs-lookup"><span data-stu-id="df5f3-262">Impact</span></span>|
| <span data-ttu-id="df5f3-263">RequestedDate_t</span><span class="sxs-lookup"><span data-stu-id="df5f3-263">RequestedDate_t</span></span>  | <span data-ttu-id="df5f3-264">A kért dátum szerint</span><span class="sxs-lookup"><span data-stu-id="df5f3-264">Requested by date</span></span> |
| <span data-ttu-id="df5f3-265">ClosedDate_t</span><span class="sxs-lookup"><span data-stu-id="df5f3-265">ClosedDate_t</span></span> | <span data-ttu-id="df5f3-266">Lezárás dátuma</span><span class="sxs-lookup"><span data-stu-id="df5f3-266">Closed date</span></span> |
| <span data-ttu-id="df5f3-267">PlannedStartDate_t</span><span class="sxs-lookup"><span data-stu-id="df5f3-267">PlannedStartDate_t</span></span>  |     <span data-ttu-id="df5f3-268">Tervezett kezdő dátuma</span><span class="sxs-lookup"><span data-stu-id="df5f3-268">Planned start date</span></span> |
| <span data-ttu-id="df5f3-269">PlannedEndDate_t</span><span class="sxs-lookup"><span data-stu-id="df5f3-269">PlannedEndDate_t</span></span>  |   <span data-ttu-id="df5f3-270">Tervezett befejezési dátum</span><span class="sxs-lookup"><span data-stu-id="df5f3-270">Planned end date</span></span> |
| <span data-ttu-id="df5f3-271">WorkStartDate_t</span><span class="sxs-lookup"><span data-stu-id="df5f3-271">WorkStartDate_t</span></span>  | <span data-ttu-id="df5f3-272">Tényleges kezdési dátuma</span><span class="sxs-lookup"><span data-stu-id="df5f3-272">Actual start date</span></span> |
| <span data-ttu-id="df5f3-273">WorkEndDate_t</span><span class="sxs-lookup"><span data-stu-id="df5f3-273">WorkEndDate_t</span></span> | <span data-ttu-id="df5f3-274">Tényleges befejezési dátum</span><span class="sxs-lookup"><span data-stu-id="df5f3-274">Actual end date</span></span>|
| <span data-ttu-id="df5f3-275">Description_s</span><span class="sxs-lookup"><span data-stu-id="df5f3-275">Description_s</span></span> | <span data-ttu-id="df5f3-276">Leírás</span><span class="sxs-lookup"><span data-stu-id="df5f3-276">Description</span></span> |
| <span data-ttu-id="df5f3-277">Computer</span><span class="sxs-lookup"><span data-stu-id="df5f3-277">Computer</span></span>  | <span data-ttu-id="df5f3-278">Konfigurációs elem</span><span class="sxs-lookup"><span data-stu-id="df5f3-278">Configuration Item</span></span> |

<span data-ttu-id="df5f3-279">**A minta Naplóelemzési képernyő ITSM adatokat:**</span><span class="sxs-lookup"><span data-stu-id="df5f3-279">**Sample Log Analytics screen for ITSM data:**</span></span>

![Napló elemzési képernyő](./media/log-analytics-itsmc/itsmc-overview-sample-log-analytics.png)

## <a name="it-service-management-connector--integration-with-other-oms-solutions"></a><span data-ttu-id="df5f3-281">Informatikai szolgáltatás Management-összekötő – integrálva más OMS-megoldásokkal</span><span class="sxs-lookup"><span data-stu-id="df5f3-281">IT Service Management connector – integration with other OMS solutions</span></span>

<span data-ttu-id="df5f3-282">Informatikai szolgáltatás Management-összekötő jelenleg hello Szolgáltatástérkép megoldással való integráció.</span><span class="sxs-lookup"><span data-stu-id="df5f3-282">IT Service Management Connector, currently supports integration with hello Service Map solution.</span></span>

<span data-ttu-id="df5f3-283">Szolgáltatástérkép automatikus felderítésére szolgál az alkalmazás-összetevők a Windows hello- és Linux rendszerek és a maps hello szolgáltatások közötti kommunikáció.</span><span class="sxs-lookup"><span data-stu-id="df5f3-283">Service Map automatically discovers hello application components on Windows and Linux systems and maps hello communication between services.</span></span> <span data-ttu-id="df5f3-284">Ez lehetővé teszi, hogy a kiszolgálók gondolunk, hogy –, hogy a kritikus szolgáltatások összekapcsolt rendszerként tooview.</span><span class="sxs-lookup"><span data-stu-id="df5f3-284">It allows you tooview your servers as you think of them – as interconnected systems that deliver critical services.</span></span> <span data-ttu-id="df5f3-285">Szolgáltatástérkép jeleníti meg a kiszolgálók, a folyamatok közötti kapcsolatokat, és portok között bármely TCP-csatlakoztatott architektúra a konfiguráció nem szükséges másik ügynököt telepíteni.</span><span class="sxs-lookup"><span data-stu-id="df5f3-285">Service Map shows connections between servers, processes, and ports across any TCP-connected architecture with no configuration required other than installation of an agent.</span></span> <span data-ttu-id="df5f3-286">További információ: [Szolgáltatástérkép](../operations-management-suite/operations-management-suite-service-map.md).</span><span class="sxs-lookup"><span data-stu-id="df5f3-286">More information: [Service Map](../operations-management-suite/operations-management-suite-service-map.md).</span></span>

<span data-ttu-id="df5f3-287">Ez az integráció létrehozott hello ITSM megoldásokban, ahogy az alábbi példa hello hello szolgáltatás ügyfélszolgálati elemek tekintheti meg:</span><span class="sxs-lookup"><span data-stu-id="df5f3-287">With this integration, you can view hello service desk items created in hello ITSM solutions as shown in hello following example:</span></span>

![<span data-ttu-id="df5f3-288">Az integrált megoldást</span><span class="sxs-lookup"><span data-stu-id="df5f3-288">Integrated solution</span></span> ](./media/log-analytics-itsmc/itsmc-overview-integrated-solutions.png)
## <a name="create-itsm-work-items-for-oms-alerts"></a><span data-ttu-id="df5f3-289">Az OMS-értesítések ITSM munkaelemek létrehozása</span><span class="sxs-lookup"><span data-stu-id="df5f3-289">Create ITSM work items for OMS alerts</span></span>

<span data-ttu-id="df5f3-290">Hello OMS riasztások létrehozhat társított munkaelemekhez kapcsolódó hello ITSM források.</span><span class="sxs-lookup"><span data-stu-id="df5f3-290">For hello OMS alerts, you can create associated work items in hello connected ITSM sources.</span></span>  <span data-ttu-id="df5f3-291">toodo a, a következő eljárás használata hello:</span><span class="sxs-lookup"><span data-stu-id="df5f3-291">toodo this, use hello following procedure:</span></span>

1. <span data-ttu-id="df5f3-292">A **naplófájl-keresési** ablakban futtassa egy keresési lekérdezés tooview naplóadatokat.</span><span class="sxs-lookup"><span data-stu-id="df5f3-292">From **Log Search** window, run a log search query tooview data.</span></span> <span data-ttu-id="df5f3-293">Lekérdezés eredményei munkaelemek hello forrása.</span><span class="sxs-lookup"><span data-stu-id="df5f3-293">Query results are hello source for work items.</span></span>
2. <span data-ttu-id="df5f3-294">A **naplófájl-keresési**, kattintson a **riasztási** tooopen hello **riasztási szabály hozzáadása** lap.</span><span class="sxs-lookup"><span data-stu-id="df5f3-294">In **Log Search**, click **Alert** tooopen hello **Add Alert Rule** page.</span></span>

    ![Napló elemzési képernyő](./media/log-analytics-itsmc/itsmc-work-items-for-oms-alerts.png)

3. <span data-ttu-id="df5f3-296">A hello **riasztási szabály hozzáadása** ablakban adja meg a szükséges hello adatait **neve**, **súlyossági**, **keresési lekérdezés**, és  **Riasztási feltételek** (ablakban/Időmetrika mérési).</span><span class="sxs-lookup"><span data-stu-id="df5f3-296">On hello **Add Alert Rule** window, provide hello required details for **Name**, **Severity**,  **Search query**, and **Alert criteria** (Time Window/Metric measurement).</span></span>
4. <span data-ttu-id="df5f3-297">Válassza ki **Igen** a **ITSM műveletek**.</span><span class="sxs-lookup"><span data-stu-id="df5f3-297">Select **Yes** for **ITSM Actions**.</span></span>
5. <span data-ttu-id="df5f3-298">Válassza ki a ITSM kapcsolatot a hello **válassza ki a kapcsolat** listája.</span><span class="sxs-lookup"><span data-stu-id="df5f3-298">Select your ITSM connection from hello **Select Connection** list.</span></span>
6. <span data-ttu-id="df5f3-299">Adja meg szükség szerint hello adatait.</span><span class="sxs-lookup"><span data-stu-id="df5f3-299">Provide hello details as required.</span></span>
7. <span data-ttu-id="df5f3-300">az egyes, a riasztások területen válassza hello naplóbejegyzések külön munkaelem toocreate **minden naplóbejegyzés egyes munkaelemek létrehozása** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="df5f3-300">toocreate a separate work item for each log entry of this alert, select hello **Create individual work items for each log entry** checkbox.</span></span>

    <span data-ttu-id="df5f3-301">Vagy</span><span class="sxs-lookup"><span data-stu-id="df5f3-301">Or</span></span>

    <span data-ttu-id="df5f3-302">a jelölőnégyzet nem kijelölt toocreate csak egy munkaelem naplóbejegyzések ebben a riasztásban vonta tetszőleges számú hagyja.</span><span class="sxs-lookup"><span data-stu-id="df5f3-302">leave this checkbox unselected toocreate only one work item for any number of log entries under this alert.</span></span>

7. <span data-ttu-id="df5f3-303">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="df5f3-303">Click **Save**.</span></span>

<span data-ttu-id="df5f3-304">hello OMS riasztás jön létre a **riasztások**.</span><span class="sxs-lookup"><span data-stu-id="df5f3-304">hello OMS alert will be created under **Alerts**.</span></span> <span data-ttu-id="df5f3-305">hello megfelelő ITSM kapcsolat munkahelyi elemek jönnek létre, ha hello megadott riasztás feltétel teljesül.</span><span class="sxs-lookup"><span data-stu-id="df5f3-305">hello corresponding ITSM connection's work items are created when hello specified alert's condition is met.</span></span>

## <a name="create-itsm-work-items-from-oms-logs"></a><span data-ttu-id="df5f3-306">Az OMS-naplók ITSM munkaelemek létrehozása</span><span class="sxs-lookup"><span data-stu-id="df5f3-306">Create ITSM work items from OMS logs</span></span>

<span data-ttu-id="df5f3-307">A csatlakoztatott hello ITSM forrásokban OMS napló keresési segítségével létrehozható munkaelemek.</span><span class="sxs-lookup"><span data-stu-id="df5f3-307">You can create work items in hello connected ITSM sources by using OMS Log Search.</span></span> <span data-ttu-id="df5f3-308">toodo a, a következő eljárás használata hello:</span><span class="sxs-lookup"><span data-stu-id="df5f3-308">toodo this, use hello following procedure:</span></span>

1. <span data-ttu-id="df5f3-309">A **naplófájl-keresési**, hello szükséges adatokat, válassza ki a hello részletes, és kattintson **létrehozás munkaelem**.</span><span class="sxs-lookup"><span data-stu-id="df5f3-309">From **Log Search**,  search hello required data, select hello detail, and click **Create work item**.</span></span>

    <span data-ttu-id="df5f3-310">Hello **ITSM munkaelem létrehozása** ablak jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="df5f3-310">hello **Create ITSM Work item** window appears:</span></span>

    ![Napló elemzési képernyő](media/log-analytics-itsmc/itsmc-work-items-from-oms-logs.png)

2.   <span data-ttu-id="df5f3-312">Adja hozzá a következő adatok hello:</span><span class="sxs-lookup"><span data-stu-id="df5f3-312">Add hello following details:</span></span>

  - <span data-ttu-id="df5f3-313">**Munkaelem-cím**: hello munkaelem címét.</span><span class="sxs-lookup"><span data-stu-id="df5f3-313">**Work item Title**: Title for hello work item.</span></span>
  - <span data-ttu-id="df5f3-314">**Munkaelem-leírás**: hello új munkaelem leírását.</span><span class="sxs-lookup"><span data-stu-id="df5f3-314">**Work item Description**: Description for hello new work item.</span></span>
  - <span data-ttu-id="df5f3-315">**Érintett számítógép**: Ha a napló adatokat talált hello számítógép nevét.</span><span class="sxs-lookup"><span data-stu-id="df5f3-315">**Affected Computer**: Name of hello computer where this log data was found.</span></span>
  - <span data-ttu-id="df5f3-316">**Válassza ki a kapcsolat**: ITSM kapcsolat használni kívánt toocreate ezt a munkaelemet.</span><span class="sxs-lookup"><span data-stu-id="df5f3-316">**Select Connection**:  ITSM connection in which you want toocreate this work item.</span></span>
  - <span data-ttu-id="df5f3-317">**Munkaelem**: típusú munkaelemet.</span><span class="sxs-lookup"><span data-stu-id="df5f3-317">**Work item**:  Type of work item.</span></span>

3. <span data-ttu-id="df5f3-318">az incidensek egy meglévő munkaelemsablonból toouse kattintson **Igen** alatt **Generate munkaelem hello sablon alapján** lehetőséget, majd kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="df5f3-318">toouse an existing work item template for an incident, click **Yes** under **Generate work item based on hello template** option and then click **Create**.</span></span>

    <span data-ttu-id="df5f3-319">Vagy</span><span class="sxs-lookup"><span data-stu-id="df5f3-319">Or,</span></span>

    <span data-ttu-id="df5f3-320">Kattintson a **nem** Ha azt szeretné, tooprovide a szabott értékekhez.</span><span class="sxs-lookup"><span data-stu-id="df5f3-320">Click **No** if you want tooprovide your customized values.</span></span>

4. <span data-ttu-id="df5f3-321">Adja meg a megfelelő értékeket hello hello **ügyfél típusú**, **hatás**, **sürgősség**, **kategória**, és **Sub kategória**  szövegmezőbe, és kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="df5f3-321">Provide hello appropriate values in hello **Contact Type**, **Impact**, **Urgency**, **Category**, and **Sub Category** text boxes, and then click **Create**.</span></span>

<span data-ttu-id="df5f3-322">hello munkaelem hello ITSM, amelyen megtekintheti a OMS létrejön.</span><span class="sxs-lookup"><span data-stu-id="df5f3-322">hello work item will be created in hello ITSM, which you can also view in OMS.</span></span>

## <a name="troubleshoot-itsm-connections-in-oms"></a><span data-ttu-id="df5f3-323">Az OMS ITSM kapcsolatok hibáinak elhárítása</span><span class="sxs-lookup"><span data-stu-id="df5f3-323">Troubleshoot ITSM connections in OMS</span></span>
1.  <span data-ttu-id="df5f3-324">Ha a csatlakoztatott adatforrás felhasználói Felületről való kapcsolódás sikertelen, és elérhetővé hello **hiba történt a kapcsolat mentése** üzenet, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="df5f3-324">If connection fails from connected source's UI and you get hello **Error in saving connection** message, do hello following:</span></span>
 - <span data-ttu-id="df5f3-325">A ServiceNow, Cherwell és Provance kapcsolatok esetén győződjön meg arról, helytelenül beírt hello felhasználónév/jelszó és az ügyfél azonosítója/titkos ügyfélkulcsot az egyes hello kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="df5f3-325">In case of ServiceNow, Cherwell and Provance connections, ensure you correctly entered  hello username/password and  client ID/client secret  for each of hello connections.</span></span> <span data-ttu-id="df5f3-326">Ha hello hiba továbbra is fennáll, ellenőrizheti a hello megfelelő ITSM termék toomake hello kapcsolatban, hogy megfelelő jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="df5f3-326">If hello error persists, check if you have sufficient privileges  in hello corresponding ITSM product toomake hello connection.</span></span>
 - <span data-ttu-id="df5f3-327">Esetén Service Manager alkalmazást, győződjön meg arról, hogy hello webalkalmazás telepítése sikeres volt, és a hibrid kapcsolat jön létre.</span><span class="sxs-lookup"><span data-stu-id="df5f3-327">In case of Service Manager, ensure that hello Web app is successfully deployed and hybrid connection is created.</span></span> <span data-ttu-id="df5f3-328">tooverify hello kapcsolat sikeresen létrejött hello helyszíni Service Manager géppel, látogasson el a hello webes alkalmazás URL-címet adja meg a megfelelő hello hello dokumentációjának részletes [a hibrid kapcsolat](log-analytics-itsmc-connections.md#configure-the-hybrid-connection).</span><span class="sxs-lookup"><span data-stu-id="df5f3-328">tooverify hello connection is successfully established with hello on-prem Service Manager machine, visit hello  Web app URL as detailed in hello documentation for making hello [hybrid connection](log-analytics-itsmc-connections.md#configure-the-hybrid-connection).</span></span>

2.  <span data-ttu-id="df5f3-329">Ha a ServiceNow adatait van nem történt-e szinkronizálva az OMS, győződjön meg arról, hello ServiceNow példánynak nem alvó állapotban van.</span><span class="sxs-lookup"><span data-stu-id="df5f3-329">If data from ServiceNow is not getting synced in OMS, ensure that hello ServiceNow instance is not sleeping.</span></span> <span data-ttu-id="df5f3-330">Ez egy kis ideig fordulhat elő, a hello ServiceNow fejlesztői esetben, ha inaktív.</span><span class="sxs-lookup"><span data-stu-id="df5f3-330">This might sometime happen in hello ServiceNow Dev instances, when idle.</span></span> <span data-ttu-id="df5f3-331">Más, a jelentés hello probléma.</span><span class="sxs-lookup"><span data-stu-id="df5f3-331">Else, report hello issue.</span></span>
3.  <span data-ttu-id="df5f3-332">Ha a riasztások elindulása esetén első az OMS Szolgáltatáshoz, azonban munkaelemek első létrehozása ITSM termék vagy a konfigurációs elemek nem kapják meg a létrehozott/kapcsolódó toowork elemek vagy bármely általános információkat, hello a következő:</span><span class="sxs-lookup"><span data-stu-id="df5f3-332">If Alerts are getting fired from OMS but work items are not getting created in ITSM product or configuration items are not getting created/linked toowork items or for any generic information, do hello following:</span></span>
 -  <span data-ttu-id="df5f3-333">Az OMS-portálon IT Service Management-összekötő megoldás lehet használt tooget, kapcsolatok munkahelyi elemek/számítógépek stb. Hello hibaüzenet hello állapot paneljén kattintson, keresse meg a túl**naplófájl-keresési** és hello kapcsolat hello hibát hello részletei használatával a hello hibaüzenet megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="df5f3-333">IT Service Management Connector solution in OMS portal could be used tooget a summary of connections/work items/computers etc. Click hello error message in hello status blade, navigate too**Log Search** and view hello connection that has hello error by using hello details in hello error message.</span></span>
 - <span data-ttu-id="df5f3-334">közvetlenül megtekintheti hello hibák/kapcsolatos információkat a hello **naplófájl-keresési** használatával lapon *típus = ServiceDeskLog_CL*.</span><span class="sxs-lookup"><span data-stu-id="df5f3-334">you can directly view hello errors/related information in hello **Log Search** page using *Type=ServiceDeskLog_CL*.</span></span>

## <a name="troubleshoot-service-manager-web-app-deployment"></a><span data-ttu-id="df5f3-335">A Service Manager webes alkalmazás telepítésének hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="df5f3-335">Troubleshoot Service Manager Web App deployment</span></span>
1.  <span data-ttu-id="df5f3-336">Esetén a webes alkalmazás központi telepítési problémák léptek fel, ellenőrizze, hogy megfelelő engedélyekkel hello előfizetésben említett toocreate/telepítés erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="df5f3-336">In case of any trouble with web app deployment, ensure you have sufficient permissions in hello subscription mentioned toocreate/deploy resources.</span></span>
2.  <span data-ttu-id="df5f3-337">Ha **az objektumhivatkozás nincs beállítva az objektum tooinstance** hello futtatása során hibaüzenet jelenik meg [parancsfájl](log-analytics-itsmc-service-manager-script.md) győződjön meg arról, hogy az érvényes értékek **felhasználói konfiguráció**szakasz.</span><span class="sxs-lookup"><span data-stu-id="df5f3-337">If **Object reference not set tooinstance of an object** error message appears while running hello [script](log-analytics-itsmc-service-manager-script.md) ensure that you entered valid values  under **User Configuration** section.</span></span>
3.  <span data-ttu-id="df5f3-338">Ha nem toocreate service bus relay-névtér, győződjön meg arról, adott hello igényel hello előfizetés erőforrás-szolgáltató regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="df5f3-338">If you fail toocreate service bus relay namespace, ensure that hello required resource provider is registered in hello subscription.</span></span> <span data-ttu-id="df5f3-339">Ha nincs regisztrálva, manuálisan hozza létre a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="df5f3-339">If not registered, manually create it from hello Azure portal.</span></span> <span data-ttu-id="df5f3-340">Akkor is létrehozható közben [hello hibrid kapcsolat létrehozása](log-analytics-itsmc-connections.md#configure-the-hybrid-connection) a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="df5f3-340">You can also create it while [creating hello hybrid connection](log-analytics-itsmc-connections.md#configure-the-hybrid-connection) from hello Azure portal.</span></span>


## <a name="contact-us"></a><span data-ttu-id="df5f3-341">Kapcsolat</span><span class="sxs-lookup"><span data-stu-id="df5f3-341">Contact us</span></span>

<span data-ttu-id="df5f3-342">A lekérdezést vagy hello IT Service Management-összekötő visszajelzést, lépjen velünk kapcsolatba [ omsitsmfeedback@microsoft.com ](mailto:omsitsmfeedback@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="df5f3-342">For any queries or feedback on hello IT Service Management Connector, contact us at [omsitsmfeedback@microsoft.com](mailto:omsitsmfeedback@microsoft.com).</span></span>

## <a name="next-steps"></a><span data-ttu-id="df5f3-343">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="df5f3-343">Next steps</span></span>
<span data-ttu-id="df5f3-344">[Adja hozzá a ITSM termékek vagy szolgáltatások tooIT Management-összekötő](log-analytics-itsmc-connections.md).</span><span class="sxs-lookup"><span data-stu-id="df5f3-344">[Add ITSM products/services tooIT Service Management Connector](log-analytics-itsmc-connections.md).</span></span>
