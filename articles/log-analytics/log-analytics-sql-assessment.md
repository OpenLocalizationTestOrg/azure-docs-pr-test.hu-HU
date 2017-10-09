---
title: "aaaOptimize az SQL Server-környezet az Azure Naplóelemzés |} Microsoft Docs"
description: "Az Azure Naplóelemzés hello SQL értékelési megoldás tooassess hello kockázat és az SQL server-környezetek állapotának rendszeres időközönkénti is használhatja."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: e297eb57-1718-4cfe-a241-b9e84b2c42ac
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f31326d8cdad3ef5d5a190614d1a18c1dac826ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-your-sql-server-environment-with-hello-sql-assessment-solution-in-log-analytics"></a><span data-ttu-id="dd746-103">Az SQL-értékelési megoldás a Log Analyticshez hello a SQL Server-környezettel optimalizálása</span><span class="sxs-lookup"><span data-stu-id="dd746-103">Optimize your SQL Server environment with hello SQL Assessment solution in Log Analytics</span></span>

![SQL Assessment szimbólum](./media/log-analytics-sql-assessment/sql-assessment-symbol.png)

<span data-ttu-id="dd746-105">SQL-értékelési megoldás tooassess hello kockázat és a környezetek állapotának rendszeres időközönkénti hello is használhatja.</span><span class="sxs-lookup"><span data-stu-id="dd746-105">You can use hello SQL Assessment solution tooassess hello risk and health of your server environments on a regular interval.</span></span> <span data-ttu-id="dd746-106">Ez a cikk segít telepíteni hello megoldás, hogy a potenciális problémákat korrekciós műveletek hajthatók végre.</span><span class="sxs-lookup"><span data-stu-id="dd746-106">This article will help you install hello solution so that you can take corrective actions for potential problems.</span></span>

<span data-ttu-id="dd746-107">Ez a megoldás a javaslatok adott tooyour telepített kiszolgálói infrastruktúra rangsorolt listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="dd746-107">This solution provides a prioritized list of recommendations specific tooyour deployed server infrastructure.</span></span> <span data-ttu-id="dd746-108">hello javaslatok szerint vannak kategóriába között hat fókusz gyorsan területeket megértéséhez hello kockázat, és hajtsa végre a javítási műveletet.</span><span class="sxs-lookup"><span data-stu-id="dd746-108">hello recommendations are categorized across six focus areas which help you quickly understand hello risk and take corrective action.</span></span>

<span data-ttu-id="dd746-109">hello ajánlásokat hello Tudásbázis és a Microsoft szakemberei ügyfél látogatások ezer tapasztalatai alapulnak.</span><span class="sxs-lookup"><span data-stu-id="dd746-109">hello recommendations made are based on hello knowledge and experience gained by Microsoft engineers from thousands of customer visits.</span></span> <span data-ttu-id="dd746-110">Minden ajánlást ismerteti, miért probléma előfordulhat, hogy számít, tooyou, és hogyan tooimplement hello javasolt módosításokat.</span><span class="sxs-lookup"><span data-stu-id="dd746-110">Each recommendation provides guidance about why an issue might matter tooyou and how tooimplement hello suggested changes.</span></span>

<span data-ttu-id="dd746-111">Kiválaszthatja a legfontosabb tooyour szervezet és a nyomon követni a kockázat szabad és megfelelő környezet futtató felé összpontosít.</span><span class="sxs-lookup"><span data-stu-id="dd746-111">You can choose focus areas that are most important tooyour organization and track your progress toward running a risk free and healthy environment.</span></span>

<span data-ttu-id="dd746-112">Után, hello megoldás felvett értékelését fejezhető be, összefoglaló adatait fókuszterületek jelenik meg hello **SQL Assessment** irányítópult hello infrastruktúra a környezetben.</span><span class="sxs-lookup"><span data-stu-id="dd746-112">After you've added hello solution and an assessment is completed, summary information for focus areas is shown on hello **SQL Assessment** dashboard for hello infrastructure in your environment.</span></span> <span data-ttu-id="dd746-113">hello alábbi szakaszok azt ismertetik, hogyan toouse hello hello információk **SQL Assessment** irányítópultot, ahol megtekintheti, és ezután javasolt műveletek a SQL server-infrastruktúrában.</span><span class="sxs-lookup"><span data-stu-id="dd746-113">hello following sections describe how toouse hello information on hello **SQL Assessment** dashboard, where you can view and then take recommended actions for your SQL server infrastructure.</span></span>

![SQL Assessment csempe képe](./media/log-analytics-sql-assessment/sql-assess-tile.png)

![SQL Assessment irányítópult képe](./media/log-analytics-sql-assessment/sql-assess-dash.png)

## <a name="installing-and-configuring-hello-solution"></a><span data-ttu-id="dd746-116">Telepítése és konfigurálása hello megoldás</span><span class="sxs-lookup"><span data-stu-id="dd746-116">Installing and configuring hello solution</span></span>
<span data-ttu-id="dd746-117">SQL Assessment összes jelenleg támogatott verziója az SQL Server Standard, Developer és Enterprise kiadás hello működik.</span><span class="sxs-lookup"><span data-stu-id="dd746-117">SQL Assessment works with all currently supported versions of SQL Server for hello Standard, Developer, and Enterprise editions.</span></span>

<span data-ttu-id="dd746-118">A következő információk tooinstall hello használja, és hello megoldás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="dd746-118">Use hello following information tooinstall and configure hello solution.</span></span>

* <span data-ttu-id="dd746-119">Ügynökök telepítenie kell a kiszolgálókat, az SQL Server telepítve van.</span><span class="sxs-lookup"><span data-stu-id="dd746-119">Agents must be installed on servers that have SQL Server installed.</span></span>
* <span data-ttu-id="dd746-120">SQL-értékelési megoldás hello minden számítógépen, amelyen OMS-ügynököt telepített .NET-keretrendszer 4 támogatott verziója szükséges.</span><span class="sxs-lookup"><span data-stu-id="dd746-120">hello SQL Assessment solution requires a supported version of .NET Framework 4 installed on each computer that has an OMS agent.</span></span>
* <span data-ttu-id="dd746-121">A sorrend tooinstall hello megoldás hello felhasználói kell rendszergazda vagy közreműködői toohello Azure-előfizetés használata hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="dd746-121">In order tooinstall hello solution, hello user must be an administrator or contributor toohello Azure subscription when using hello Azure portal.</span></span> <span data-ttu-id="dd746-122">Ezenkívül hello felhasználói hello OMS munkaterület közreműködő vagy a rendszergazda szerepkör tagjának az OMS-portálon hello kell lennie.</span><span class="sxs-lookup"><span data-stu-id="dd746-122">In addition, hello user must be a member of hello OMS workspace contributor or administrator role in hello OMS portal.</span></span>
* <span data-ttu-id="dd746-123">Operations Manager-ügynök hello SQL értékelésével használatakor toouse Operations Manager Run-As fiók lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="dd746-123">When using hello Operations Manager agent with SQL Assessment, you'll need toouse an Operations Manager Run-As account.</span></span> <span data-ttu-id="dd746-124">Lásd: [Operations Manager futtató fiókok az OMS](#operations-manager-run-as-accounts-for-oms) alább olvashat.</span><span class="sxs-lookup"><span data-stu-id="dd746-124">See [Operations Manager run-as accounts for OMS](#operations-manager-run-as-accounts-for-oms) below for more information.</span></span>

  > [!NOTE]
  > <span data-ttu-id="dd746-125">hello MMA ügynök nem támogatja az Operations Manager Run-As fiókokat.</span><span class="sxs-lookup"><span data-stu-id="dd746-125">hello MMA agent does not support Operations Manager Run-As accounts.</span></span>
  >
  >
* <span data-ttu-id="dd746-126">Adja hozzá a hello SQL értékelési megoldás tooyour ismertetett eljárással hello OMS-munkaterület [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjtemény](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="dd746-126">Add hello SQL Assessment solution tooyour OMS workspace using hello process described in [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md).</span></span> <span data-ttu-id="dd746-127">Nincs szükség további konfigurációra.</span><span class="sxs-lookup"><span data-stu-id="dd746-127">There is no further configuration required.</span></span>

> [!NOTE]
> <span data-ttu-id="dd746-128">Hello megoldás hozzáadása után hello AdvisorAssessment.exe nincs hozzáadva fájl tooservers ügynökkel.</span><span class="sxs-lookup"><span data-stu-id="dd746-128">After you've added hello solution, hello AdvisorAssessment.exe file is added tooservers with agents.</span></span> <span data-ttu-id="dd746-129">Konfigurációs adatok olvasása és küldi el a rendszer toohello OMS szolgáltatás hello felhőben feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="dd746-129">Configuration data is read and then sent toohello OMS service in hello cloud for processing.</span></span> <span data-ttu-id="dd746-130">Logikai alkalmazott toohello fogadott adatok és hello felhőszolgáltatás hello adatait rögzíti.</span><span class="sxs-lookup"><span data-stu-id="dd746-130">Logic is applied toohello received data and hello cloud service records hello data.</span></span>

## <a name="sql-assessment-data-collection-details"></a><span data-ttu-id="dd746-131">SQL Assessment az gyűjtemény adatait</span><span class="sxs-lookup"><span data-stu-id="dd746-131">SQL Assessment data collection details</span></span>
<span data-ttu-id="dd746-132">SQL Assessment gyűjti a WMI-adatok, a beállításjegyzék-adatok, a teljesítményadatokat és az SQL Server dinamikus felügyeleti nézetben eredményeket hello ügynökök, amelyeken engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="dd746-132">SQL Assessment collects WMI data, registry data, performance data, and SQL Server dynamic management view results using hello agents that you have enabled.</span></span>

<span data-ttu-id="dd746-133">hello következő táblázatban a adatgyűjtési módszerek ügynökök, az Operations Manager (SCOM) szükséges-e, és milyen gyakran adatgyűjtés ügynök által.</span><span class="sxs-lookup"><span data-stu-id="dd746-133">hello following table shows data collection methods for agents, whether Operations Manager (SCOM) is required, and how often data is collected by an agent.</span></span>

| <span data-ttu-id="dd746-134">Platform</span><span class="sxs-lookup"><span data-stu-id="dd746-134">platform</span></span> | <span data-ttu-id="dd746-135">Közvetlen ügynök</span><span class="sxs-lookup"><span data-stu-id="dd746-135">Direct Agent</span></span> | <span data-ttu-id="dd746-136">SCOM-ügynököt</span><span class="sxs-lookup"><span data-stu-id="dd746-136">SCOM agent</span></span> | <span data-ttu-id="dd746-137">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="dd746-137">Azure Storage</span></span> | <span data-ttu-id="dd746-138">SCOM szükséges?</span><span class="sxs-lookup"><span data-stu-id="dd746-138">SCOM required?</span></span> | <span data-ttu-id="dd746-139">Felügyeleti csoport keresztül küldött SCOM ügynök adatok</span><span class="sxs-lookup"><span data-stu-id="dd746-139">SCOM agent data sent via management group</span></span> | <span data-ttu-id="dd746-140">Gyűjtemény gyakorisága</span><span class="sxs-lookup"><span data-stu-id="dd746-140">collection frequency</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="dd746-141">Windows</span><span class="sxs-lookup"><span data-stu-id="dd746-141">Windows</span></span> | <span data-ttu-id="dd746-142">&#8226;</span><span class="sxs-lookup"><span data-stu-id="dd746-142">&#8226;</span></span> | <span data-ttu-id="dd746-143">&#8226;</span><span class="sxs-lookup"><span data-stu-id="dd746-143">&#8226;</span></span> |  |  | <span data-ttu-id="dd746-144">&#8226;</span><span class="sxs-lookup"><span data-stu-id="dd746-144">&#8226;</span></span> |<span data-ttu-id="dd746-145">7 nap</span><span class="sxs-lookup"><span data-stu-id="dd746-145">7 days</span></span> |

## <a name="operations-manager-run-as-accounts-for-oms"></a><span data-ttu-id="dd746-146">Az Operations Manager futtató fiókot a következő OMS</span><span class="sxs-lookup"><span data-stu-id="dd746-146">Operations Manager run-as accounts for OMS</span></span>
<span data-ttu-id="dd746-147">Az OMS szolgáltatáshoz hello Operations Manager-ügynök és a felügyeleti csoport toocollect használ, és küldjön adatokat toohello OMS szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="dd746-147">Log Analytics in OMS uses hello Operations Manager agent and management group toocollect and send data toohello OMS service.</span></span> <span data-ttu-id="dd746-148">Felügyeleti csomagokat a munkaterhelések tooprovide után OMS-buildek érték-szolgáltatások hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="dd746-148">OMS builds upon management packs for workloads tooprovide value-add services.</span></span> <span data-ttu-id="dd746-149">Minden számítási feladat által igényelt, munkaterhelés-specifikus jogosultságokkal toorun felügyeleti csomagok eltérő biztonsági környezetben, például egy olyan tartományi fiók.</span><span class="sxs-lookup"><span data-stu-id="dd746-149">Each workload requires workload-specific privileges toorun management packs in a different security context, such as a domain account.</span></span> <span data-ttu-id="dd746-150">Tooprovide hitelesítő adatok konfigurálása egy Operations Manager futtató fiókja szükséges.</span><span class="sxs-lookup"><span data-stu-id="dd746-150">You need tooprovide credential information by configuring an Operations Manager Run As account.</span></span>

<span data-ttu-id="dd746-151">Információ tooset hello Operations Manager futtató fiókja a SQL ellenőrzéséhez a következő hello használata.</span><span class="sxs-lookup"><span data-stu-id="dd746-151">Use hello following information tooset hello Operations Manager run-as account for SQL Assessment.</span></span>

### <a name="set-hello-run-as-account-for-sql-assessment"></a><span data-ttu-id="dd746-152">Hello futtató fiókot az SQL-értékelés beállítása</span><span class="sxs-lookup"><span data-stu-id="dd746-152">Set hello Run As account for SQL assessment</span></span>
 <span data-ttu-id="dd746-153">Ha már használja az SQL Server felügyeleti csomag hello, a futtató fiókot kell használnia.</span><span class="sxs-lookup"><span data-stu-id="dd746-153">If you are already using hello SQL Server management pack, you should use that Run As account.</span></span>

#### <a name="tooconfigure-hello-sql-run-as-account-in-hello-operations-console"></a><span data-ttu-id="dd746-154">tooconfigure hello SQL Futtatás mint fiók hello operatív konzolon</span><span class="sxs-lookup"><span data-stu-id="dd746-154">tooconfigure hello SQL Run As account in hello Operations console</span></span>
> [!NOTE]
> <span data-ttu-id="dd746-155">Használata hello OMS közvetlen ügynök, nem pedig hello SCOM-ügynököt, hello felügyeleti csomag mindig fut a helyi rendszer fiók hello hello biztonsági környezetében.</span><span class="sxs-lookup"><span data-stu-id="dd746-155">If you are using hello OMS direct agent, rather than hello SCOM agent, hello management pack always runs in hello security context of hello Local System account.</span></span> <span data-ttu-id="dd746-156">Kihagyás lépéseket 1-5, az alábbi, majd futtassa a T-SQL vagy a Powershell sample NT AUTHORITY\SYSTEM hello felhasználónév megadása vagy hello.</span><span class="sxs-lookup"><span data-stu-id="dd746-156">Skip steps 1-5 below, and run either hello T-SQL or Powershell sample, specifying NT AUTHORITY\SYSTEM as hello user name.</span></span>
>
>

1. <span data-ttu-id="dd746-157">Az Operations Manager programban nyissa meg hello operatív konzolt, és kattintson **felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="dd746-157">In Operations Manager, open hello Operations console, and then click **Administration**.</span></span>
2. <span data-ttu-id="dd746-158">A **futtató konfiguráció**, kattintson a **profilok**, és nyissa meg a **OMS SQL Assessment futtató profil**.</span><span class="sxs-lookup"><span data-stu-id="dd746-158">Under **Run As Configuration**, click **Profiles**, and open **OMS SQL Assessment Run As Profile**.</span></span>
3. <span data-ttu-id="dd746-159">A hello **futtató fiókok** kattintson **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="dd746-159">On hello **Run As Accounts** page, click **Add**.</span></span>
4. <span data-ttu-id="dd746-160">Válassza ki a Windows futtató fiókhoz, amely tartalmazza az SQL Server szükséges hello hitelesítő adatokat, vagy kattintson a **új** toocreate egyet.</span><span class="sxs-lookup"><span data-stu-id="dd746-160">Select a Windows Run As account that contains hello credentials needed for SQL Server, or click **New** toocreate one.</span></span>

   > [!NOTE]
   > <span data-ttu-id="dd746-161">Futtató fiók típusú hello Windows kell lennie.</span><span class="sxs-lookup"><span data-stu-id="dd746-161">hello Run As account type must be Windows.</span></span> <span data-ttu-id="dd746-162">hello futtató fiókot is a helyi Rendszergazdák csoport összes SQL Server-példányokat futtató Windows-kiszolgálók részének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="dd746-162">hello Run As account must also be part of Local Administrators group on all Windows Servers hosting SQL Server Instances.</span></span>
   >
   >
5. <span data-ttu-id="dd746-163">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="dd746-163">Click **Save**.</span></span>
6. <span data-ttu-id="dd746-164">Módosíthatja, és hajthat végre a következő T-SQL minta az egyes SQL Server-példány toogrant a minimálisan szükséges tooRun fiók tooperform SQL Assessment hello.</span><span class="sxs-lookup"><span data-stu-id="dd746-164">Modify and then execute hello following T-SQL sample on each SQL Server Instance toogrant minimum permissions required tooRun As Account tooperform SQL Assessment.</span></span> <span data-ttu-id="dd746-165">Azonban nem kell toodo a futtató fiók része már hello SysAdmin (rendszergazda) kiszolgálói szerepkört az SQL Server-példányokat.</span><span class="sxs-lookup"><span data-stu-id="dd746-165">However, you don’t need toodo this if a Run As Account is already part of hello sysadmin server role on SQL Server Instances.</span></span>

```
---
    -- Replace <UserName> with hello actual user name being used as Run As Account.
    USE master

    -- Create login for hello user, comment this line if login is already created.
    CREATE LOGIN [<UserName>] FROM WINDOWS

    -- Grant permissions toouser.
    GRANT VIEW SERVER STATE too[<UserName>]
    GRANT VIEW ANY DEFINITION too[<UserName>]
    GRANT VIEW ANY DATABASE too[<UserName>]

    -- Add database user for all hello databases on SQL Server Instance, this is required for connecting tooindividual databases.
    -- NOTE: This command must be run anytime new databases are added tooSQL Server instances.
    EXEC sp_msforeachdb N'USE [?]; CREATE USER [<UserName>] FOR LOGIN [<UserName>];'

```
#### <a name="tooconfigure-hello-sql-run-as-account-using-windows-powershell"></a><span data-ttu-id="dd746-166">tooconfigure hello SQL futtató fiókot a Windows PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="dd746-166">tooconfigure hello SQL Run As account using Windows PowerShell</span></span>
<span data-ttu-id="dd746-167">Nyisson meg egy PowerShell-ablakot, és futtassa az információkkal már frissítése után a következő parancsfájl hello:</span><span class="sxs-lookup"><span data-stu-id="dd746-167">Open a PowerShell window and run hello following script after you’ve updated it with your information:</span></span>

```

    import-module OperationsManager
    New-SCOMManagementGroupConnection "<your management group name>"

    $profile = Get-SCOMRunAsProfile -DisplayName "OMS SQL Assessment Run As Profile"
    $account = Get-SCOMrunAsAccount | Where-Object {$_.Name -eq "<your run as account name>"}
    Set-SCOMRunAsProfile -Action "Add" -Profile $Profile -Account $Account
```

## <a name="understanding-how-recommendations-are-prioritized"></a><span data-ttu-id="dd746-168">Hogyan kerülnek előrébb a javaslatok megértése</span><span class="sxs-lookup"><span data-stu-id="dd746-168">Understanding how recommendations are prioritized</span></span>
<span data-ttu-id="dd746-169">Minden javaslat végrehajtott, amely azonosítja a hello javaslat relatív fontosságát hello súlyozási értéket kap.</span><span class="sxs-lookup"><span data-stu-id="dd746-169">Every recommendation made is given a weighting value that identifies hello relative importance of hello recommendation.</span></span> <span data-ttu-id="dd746-170">Csak hello tíz legfontosabb javaslatok láthatók.</span><span class="sxs-lookup"><span data-stu-id="dd746-170">Only hello ten most important recommendations are shown.</span></span>

### <a name="how-weights-are-calculated"></a><span data-ttu-id="dd746-171">Hogyan súlyok kiszámítása</span><span class="sxs-lookup"><span data-stu-id="dd746-171">How weights are calculated</span></span>
<span data-ttu-id="dd746-172">Súlyozás alapján három kulcsfontosságú szerepet játszik az összesített értékek:</span><span class="sxs-lookup"><span data-stu-id="dd746-172">Weightings are aggregate values based on three key factors:</span></span>

* <span data-ttu-id="dd746-173">Hello *valószínűség* , hogy azonosított problémát okoz problémát.</span><span class="sxs-lookup"><span data-stu-id="dd746-173">hello *probability* that an issue identified will cause problems.</span></span> <span data-ttu-id="dd746-174">Nagyobb a valószínűsége annak csatlakozás tooa nagyobb összesített pontszám hello javaslat.</span><span class="sxs-lookup"><span data-stu-id="dd746-174">A higher probability equates tooa larger overall score for hello recommendation.</span></span>
* <span data-ttu-id="dd746-175">Hello *hatás* hello probléma szervezetfüggő, ha azt okozzák a problémát.</span><span class="sxs-lookup"><span data-stu-id="dd746-175">hello *impact* of hello issue on your organization if it does cause a problem.</span></span> <span data-ttu-id="dd746-176">Egy magasabb hatása megfelel tooa nagyobb összesített pontszám hello javaslat.</span><span class="sxs-lookup"><span data-stu-id="dd746-176">A higher impact equates tooa larger overall score for hello recommendation.</span></span>
* <span data-ttu-id="dd746-177">Hello *elérhető* tooimplement hello javaslat szükséges.</span><span class="sxs-lookup"><span data-stu-id="dd746-177">hello *effort* required tooimplement hello recommendation.</span></span> <span data-ttu-id="dd746-178">Egy újabb elérhető csatlakozás tooa kisebb összesített pontszám hello javaslat.</span><span class="sxs-lookup"><span data-stu-id="dd746-178">A higher effort equates tooa smaller overall score for hello recommendation.</span></span>

<span data-ttu-id="dd746-179">minden ajánlást a súlyozási hello arányában hello összesített pontszám minden fókusz területen érhető el.</span><span class="sxs-lookup"><span data-stu-id="dd746-179">hello weighting for each recommendation is expressed as a percentage of hello total score available for each focus area.</span></span> <span data-ttu-id="dd746-180">Például ha hello biztonsági és megfelelőségi fókusz területen ajánlást 5 %-os pontszámot, amelyekre érvényes végrehajtási növeli a teljes biztonsági és megfelelőségi pontszám által 5 %.</span><span class="sxs-lookup"><span data-stu-id="dd746-180">For example, if a recommendation in hello Security and Compliance focus area has a score of 5%, implementing that recommendation will increase your overall Security and Compliance score by 5%.</span></span>

### <a name="focus-areas"></a><span data-ttu-id="dd746-181">Fókuszterületek</span><span class="sxs-lookup"><span data-stu-id="dd746-181">Focus areas</span></span>
<span data-ttu-id="dd746-182">**Biztonsági és megfelelőségi** -e fókuszba területen látható esetleges biztonsági fenyegetéseket jelezhetnek és a behatolás, a vállalati házirendeket és a technikai, jogszabályi és szabályozásoknak megfelelőségi előírásokra vonatkozó javaslatok.</span><span class="sxs-lookup"><span data-stu-id="dd746-182">**Security and Compliance** - This focus area shows recommendations for potential security threats and breaches, corporate policies, and technical, legal and regulatory compliance requirements.</span></span>

<span data-ttu-id="dd746-183">**Rendelkezésre állás és üzleti folytonosság** -fókusz itt megtekinthető a szolgáltatás rendelkezésre állása, a rugalmasság, az infrastruktúra és az üzleti védelmét javaslatok.</span><span class="sxs-lookup"><span data-stu-id="dd746-183">**Availability and Business Continuity** - This focus area shows recommendations for service availability, resiliency of your infrastructure, and business protection.</span></span>

<span data-ttu-id="dd746-184">**Teljesítmény és méretezhetőség** -e fókuszba területen látható javaslatok toohelp a szervezet informatikai infrastruktúrájának nő, győződjön meg arról, hogy az informatikai környezet megfelel az aktuális teljesítménykövetelményeknek, és képes toorespond toochanging a szükséges infrastruktúrát.</span><span class="sxs-lookup"><span data-stu-id="dd746-184">**Performance and Scalability** - This focus area shows recommendations toohelp your organization's IT infrastructure grow, ensure that your IT environment meets current performance requirements, and is able toorespond toochanging infrastructure needs.</span></span>

<span data-ttu-id="dd746-185">**Áttelepítés és a telepítés frissítéséhez** - fókusz itt megtekinthető javaslatok toohelp frissít, telepítse át, és SQL Server tooyour meglévő infrastruktúra központi telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="dd746-185">**Upgrade, Migration and Deployment** - This focus area shows recommendations toohelp you upgrade, migrate, and deploy SQL Server tooyour existing infrastructure.</span></span>

<span data-ttu-id="dd746-186">**Műveletek és -figyelő** - e fókuszba területen látható javaslatok toohelp érdekében az IT-üzemeltetők valósíthatja meg megelőző karbantartási és teljesítmény maximalizálása érdekében.</span><span class="sxs-lookup"><span data-stu-id="dd746-186">**Operations and Monitoring** - This focus area shows recommendations toohelp streamline your IT operations, implement preventative maintenance, and maximize performance.</span></span>

<span data-ttu-id="dd746-187">**Változás- és konfigurációkezelés** -e fókuszba területen látható javaslatok toohelp védelme a napi tevékenységek, győződjön meg arról, hogy módosításokat nem negatív hatással vannak az infrastruktúra, Változáskezelő eljárások és tootrack naplózása rendszer-konfigurációkat.</span><span class="sxs-lookup"><span data-stu-id="dd746-187">**Change and Configuration Management** - This focus area shows recommendations toohelp protect day-to-day operations, ensure that changes don't negatively affect your infrastructure, establish change control procedures, and tootrack and audit system configurations.</span></span>

### <a name="should-you-aim-tooscore-100-in-every-focus-area"></a><span data-ttu-id="dd746-188">Akkor érhető el, tooscore minden fókusz területen 100 %-os?</span><span class="sxs-lookup"><span data-stu-id="dd746-188">Should you aim tooscore 100% in every focus area?</span></span>
<span data-ttu-id="dd746-189">Nem feltétlenül.</span><span class="sxs-lookup"><span data-stu-id="dd746-189">Not necessarily.</span></span> <span data-ttu-id="dd746-190">hello javaslatok hello Tudásbázis és a Microsoft szakemberei által ügyfél látogatások ezer keresztül szerzett tapasztalatok alapulnak.</span><span class="sxs-lookup"><span data-stu-id="dd746-190">hello recommendations are based on hello knowledge and experiences gained by Microsoft engineers across thousands of customer visits.</span></span> <span data-ttu-id="dd746-191">Azonban nem két kiszolgáló infrastruktúrák hello azonos, és konkrét javaslatokkal lehet több vagy kevesebb vonatkozó tooyou is.</span><span class="sxs-lookup"><span data-stu-id="dd746-191">However, no two server infrastructures are hello same, and specific recommendations may be more or less relevant tooyou.</span></span> <span data-ttu-id="dd746-192">Például bizonyos biztonsági javaslatok kevesebb megfelelő, ha a virtuális gépek nincsenek kitett toohello Internet lehet.</span><span class="sxs-lookup"><span data-stu-id="dd746-192">For example, some security recommendations might be less relevant if your virtual machines are not exposed toohello Internet.</span></span> <span data-ttu-id="dd746-193">Egyes rendelkezésre állási javaslatok lehet kevésbé alacsony prioritást ad hoc adatgyűjtés és a reporting Services.</span><span class="sxs-lookup"><span data-stu-id="dd746-193">Some availability recommendations may be less relevant for services that provide low priority ad hoc data collection and reporting.</span></span> <span data-ttu-id="dd746-194">Problémákra, amelyek fontos tooa érett üzleti a kevésbé fontos tooa kezdeti lehet.</span><span class="sxs-lookup"><span data-stu-id="dd746-194">Issues that are important tooa mature business may be less important tooa start-up.</span></span> <span data-ttu-id="dd746-195">Előfordulhat, hogy szeretné, hogy mely fókusz területek a következők a prioritások tooidentify, és tekintse hogyan a pontszámokat változnak az idők.</span><span class="sxs-lookup"><span data-stu-id="dd746-195">You may want tooidentify which focus areas are your priorities and then look at how your scores change over time.</span></span>

<span data-ttu-id="dd746-196">Minden javaslat arról, hogy miért fontos útmutatást tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="dd746-196">Every recommendation includes guidance about why it is important.</span></span> <span data-ttu-id="dd746-197">Ez az útmutató tooevaluate kell használnia, hogy hello javaslat végrehajtási, az informatikai szolgáltatások és hello üzleti a szervezet igényeinek hello jellegéből.</span><span class="sxs-lookup"><span data-stu-id="dd746-197">You should use this guidance tooevaluate whether implementing hello recommendation is appropriate for you, given hello nature of your IT services and hello business needs of your organization.</span></span>

## <a name="use-assessment-focus-area-recommendations"></a><span data-ttu-id="dd746-198">Kiértékelés fókusz terület ajánlásai használata</span><span class="sxs-lookup"><span data-stu-id="dd746-198">Use assessment focus area recommendations</span></span>
<span data-ttu-id="dd746-199">Egy értékelési megoldás az OMS használata előtt telepített hello megoldást kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="dd746-199">Before you can use an assessment solution in OMS, you must have hello solution installed.</span></span> <span data-ttu-id="dd746-200">tooread több megoldások telepítéséről lásd: [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjtemény](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="dd746-200">tooread more about installing solutions, see [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md).</span></span> <span data-ttu-id="dd746-201">Azt követően, megtekintheti a javaslatok hello összegzése az OMS-hello áttekintése oldalon hello SQL Assessment csempe használatával.</span><span class="sxs-lookup"><span data-stu-id="dd746-201">After it is installed, you can view hello summary of recommendations by using hello SQL Assessment tile on hello Overview page in OMS.</span></span>

<span data-ttu-id="dd746-202">Nézet hello megfelelőségi értékelése az infrastruktúrát, és a-feltárás javaslatokat foglalja össze.</span><span class="sxs-lookup"><span data-stu-id="dd746-202">View hello summarized compliance assessments for your infrastructure and then drill-into recommendations.</span></span>

### <a name="tooview-recommendations-for-a-focus-area-and-take-corrective-action"></a><span data-ttu-id="dd746-203">a fókusz területről, és hajtsa végre a megfelelő javítási műveletek tooview javaslatok</span><span class="sxs-lookup"><span data-stu-id="dd746-203">tooview recommendations for a focus area and take corrective action</span></span>
1. <span data-ttu-id="dd746-204">A hello **áttekintése** hello kattintson **SQL Assessment** csempére.</span><span class="sxs-lookup"><span data-stu-id="dd746-204">On hello **Overview** page, click hello **SQL Assessment** tile.</span></span>
2. <span data-ttu-id="dd746-205">A hello **SQL Assessment** lapon ellenőrizze a hello összefoglaló információkat valamelyik hello fókusz terület paneleken és majd kattintson egy adott fókusz területre tooview javaslatok.</span><span class="sxs-lookup"><span data-stu-id="dd746-205">On hello **SQL Assessment** page, review hello summary information in one of hello focus area blades and then click one tooview recommendations for that focus area.</span></span>
3. <span data-ttu-id="dd746-206">Bármely hello fókusz terület lapok tekintheti meg a környezetnek előrébb hello ajánlásokat.</span><span class="sxs-lookup"><span data-stu-id="dd746-206">On any of hello focus area pages, you can view hello prioritized recommendations made for your environment.</span></span> <span data-ttu-id="dd746-207">Kattintson az ajánlás **érintett objektumok** tooview ezért hello javaslatokkal kapcsolatos információk.</span><span class="sxs-lookup"><span data-stu-id="dd746-207">Click a recommendation under **Affected Objects** tooview details about why hello recommendation is made.</span></span>  
    <span data-ttu-id="dd746-208">![SQL-kiértékelés ajánlásai képe](./media/log-analytics-sql-assessment/sql-assess-focus.png)</span><span class="sxs-lookup"><span data-stu-id="dd746-208">![image of SQL Assessment recommendations](./media/log-analytics-sql-assessment/sql-assess-focus.png)</span></span>
4. <span data-ttu-id="dd746-209">Az ajánlott javítási műveletek hajthatók végre **javasolt műveletek**.</span><span class="sxs-lookup"><span data-stu-id="dd746-209">You can take corrective actions suggested in **Suggested Actions**.</span></span> <span data-ttu-id="dd746-210">Hello elem intéztek, újabb értékelések fog jegyezze fel, az elvégzett műveletek, és a megfelelőségi pontszám növeli.</span><span class="sxs-lookup"><span data-stu-id="dd746-210">When hello item has been addressed, later assessments will record that recommended actions were taken and your compliance score will increase.</span></span> <span data-ttu-id="dd746-211">Javított elemek jelennek meg **átadott objektumok**.</span><span class="sxs-lookup"><span data-stu-id="dd746-211">Corrected items appear as **Passed Objects**.</span></span>

## <a name="ignore-recommendations"></a><span data-ttu-id="dd746-212">Figyelmen kívül hagyja a javaslatok</span><span class="sxs-lookup"><span data-stu-id="dd746-212">Ignore recommendations</span></span>
<span data-ttu-id="dd746-213">Ha rendelkezik, amelyet az tooignore ajánlásokat, létrehozhat egy szövegfájlt, amely OMS tooprevent javaslatok jelennek meg az értékelési eredmények fogja használni.</span><span class="sxs-lookup"><span data-stu-id="dd746-213">If you have recommendations that you want tooignore, you can create a text file that OMS will use tooprevent recommendations from appearing in your assessment results.</span></span>

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

### <a name="tooidentify-recommendations-that-you-will-ignore"></a><span data-ttu-id="dd746-214">figyelmen kívül hagyja majd tooidentify javaslatok</span><span class="sxs-lookup"><span data-stu-id="dd746-214">tooidentify recommendations that you will ignore</span></span>
1. <span data-ttu-id="dd746-215">Jelentkezzen be tooyour munkaterület, és nyissa meg a keresési napló.</span><span class="sxs-lookup"><span data-stu-id="dd746-215">Sign in tooyour workspace and open Log Search.</span></span> <span data-ttu-id="dd746-216">A következő lekérdezés toolist javaslatok, amelyek nem tudták a környezetében az hello használata.</span><span class="sxs-lookup"><span data-stu-id="dd746-216">Use hello following query toolist recommendations that have failed for computers in your environment.</span></span>

   ```
   Type=SQLAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```

   <span data-ttu-id="dd746-217">Itt található napló keresési lekérdezés ábrázoló képernyőkép hello: ![nem lehetett ajánlásait](./media/log-analytics-sql-assessment/sql-assess-failed-recommendations.png)</span><span class="sxs-lookup"><span data-stu-id="dd746-217">Here's a screen shot showing hello Log Search query: ![failed recommendations](./media/log-analytics-sql-assessment/sql-assess-failed-recommendations.png)</span></span>
2. <span data-ttu-id="dd746-218">Válassza ki a megjeleníteni kívánt tooignore javaslatok.</span><span class="sxs-lookup"><span data-stu-id="dd746-218">Choose recommendations that you want tooignore.</span></span> <span data-ttu-id="dd746-219">A következő eljárással hello hello értékeket szeretné használni a RecommendationId.</span><span class="sxs-lookup"><span data-stu-id="dd746-219">You’ll use hello values for RecommendationId in hello next procedure.</span></span>

### <a name="toocreate-and-use-an-ignorerecommendationstxt-text-file"></a><span data-ttu-id="dd746-220">toocreate és -felhasználási egy IgnoreRecommendations.txt szövegfájl</span><span class="sxs-lookup"><span data-stu-id="dd746-220">toocreate and use an IgnoreRecommendations.txt text file</span></span>
1. <span data-ttu-id="dd746-221">Hozzon létre egy IgnoreRecommendations.txt nevű fájlt.</span><span class="sxs-lookup"><span data-stu-id="dd746-221">Create a file named IgnoreRecommendations.txt.</span></span>
2. <span data-ttu-id="dd746-222">Illessze be, vagy adjon meg minden RecommendationId minden javaslat, hogy azt szeretné, hogy külön sorban OMS tooignore és majd mentse és zárja be hello fájl számára.</span><span class="sxs-lookup"><span data-stu-id="dd746-222">Paste or type each RecommendationId for each recommendation that you want OMS tooignore on a separate line and then save and close hello file.</span></span>
3. <span data-ttu-id="dd746-223">A következő mappa minden olyan számítógépen OMS tooignore ajánlásokat, ahová hello hello fájl helyezze el.</span><span class="sxs-lookup"><span data-stu-id="dd746-223">Put hello file in hello following folder on each computer where you want OMS tooignore recommendations.</span></span>
   * <span data-ttu-id="dd746-224">Azokon a számítógépeken a Microsoft Monitoring Agent (közvetlenül vagy az Operations Manager keresztül csatlakozik) – hello *SystemDrive*: \Program Files\Microsoft figyelés Agent\Agent</span><span class="sxs-lookup"><span data-stu-id="dd746-224">On computers with hello Microsoft Monitoring Agent (connected directly or through Operations Manager) - *SystemDrive*:\Program Files\Microsoft Monitoring Agent\Agent</span></span>
   * <span data-ttu-id="dd746-225">Hello Operations Manager felügyeleti kiszolgálón - *SystemDrive*: System Center 2012 R2\Operations Manager\Server \Program Files\Microsoft</span><span class="sxs-lookup"><span data-stu-id="dd746-225">On hello Operations Manager management server - *SystemDrive*:\Program Files\Microsoft System Center 2012 R2\Operations Manager\Server</span></span>

### <a name="tooverify-that-recommendations-are-ignored"></a><span data-ttu-id="dd746-226">tooverify, hogy javaslatokat figyelmen kívül lesznek hagyva</span><span class="sxs-lookup"><span data-stu-id="dd746-226">tooverify that recommendations are ignored</span></span>
1. <span data-ttu-id="dd746-227">Alapértelmezett gyakoriság 7 nap által a következő ütemezett értékelési hello futtatása után hello megadott javaslatok figyelmen kívül hagyott megjelölve, és nem jelenik meg a hello assessment irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="dd746-227">After hello next scheduled assessment runs, by default every 7 days, hello specified recommendations are marked Ignored and will not appear on hello assessment dashboard.</span></span>
2. <span data-ttu-id="dd746-228">Használhatja a következő naplófájl-keresési lekérdezések toolist hello ajánlások hello figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="dd746-228">You can use hello following Log Search queries toolist all hello ignored recommendations.</span></span>

   ```
   Type=SQLAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```
3. <span data-ttu-id="dd746-229">Ha később úgy dönt, hogy szeretné-e véve toosee javaslatok, távolítsa el a IgnoreRecommendations.txt fájlokat, vagy RecommendationIDs eltávolíthatja őket.</span><span class="sxs-lookup"><span data-stu-id="dd746-229">If you decide later that you want toosee ignored recommendations, remove any IgnoreRecommendations.txt files, or you can remove RecommendationIDs from them.</span></span>

## <a name="sql-assessment-solution-faq"></a><span data-ttu-id="dd746-230">SQL-értékelési megoldás – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="dd746-230">SQL Assessment solution FAQ</span></span>
<span data-ttu-id="dd746-231">*Milyen gyakran fut a felmérés elvégzéséhez?*</span><span class="sxs-lookup"><span data-stu-id="dd746-231">*How often does an assessment run?*</span></span>

* <span data-ttu-id="dd746-232">hello assessment 7 naponként futtatható.</span><span class="sxs-lookup"><span data-stu-id="dd746-232">hello assessment runs every 7 days.</span></span>

<span data-ttu-id="dd746-233">*Van olyan módon tooconfigure milyen gyakran hello assessment fut?*</span><span class="sxs-lookup"><span data-stu-id="dd746-233">*Is there a way tooconfigure how often hello assessment runs?*</span></span>

* <span data-ttu-id="dd746-234">Jelenleg nem.</span><span class="sxs-lookup"><span data-stu-id="dd746-234">Not at this time.</span></span>

<span data-ttu-id="dd746-235">*Ha egy másik kiszolgáló észlelhető után hello SQL értékelési megoldás felvett, az értékelési?*</span><span class="sxs-lookup"><span data-stu-id="dd746-235">*If another server is discovered after I’ve added hello SQL assessment solution, will it be assessed?*</span></span>

* <span data-ttu-id="dd746-236">Igen, ha kiderül, hogy megfelelőségét ellenőrizni kell, majd a gyakoriság 7 nap.</span><span class="sxs-lookup"><span data-stu-id="dd746-236">Yes, once it is discovered it is assessed from then on, every 7 days.</span></span>

<span data-ttu-id="dd746-237">*Ha egy kiszolgáló le van szerelve, ha azt eltávolítja a szolgáltatásból hello assessment?*</span><span class="sxs-lookup"><span data-stu-id="dd746-237">*If a server is decommissioned, when will it be removed from hello assessment?*</span></span>

* <span data-ttu-id="dd746-238">Ha a kiszolgáló nem időközönként adatokat küldenek a 3 hét, a rendszer eltávolítja.</span><span class="sxs-lookup"><span data-stu-id="dd746-238">If a server does not submit data for 3 weeks, it is removed.</span></span>

<span data-ttu-id="dd746-239">*Mi az az adatgyűjtés hello hello folyamat hello neve?*</span><span class="sxs-lookup"><span data-stu-id="dd746-239">*What is hello name of hello process that does hello data collection?*</span></span>

* <span data-ttu-id="dd746-240">AdvisorAssessment.exe</span><span class="sxs-lookup"><span data-stu-id="dd746-240">AdvisorAssessment.exe</span></span>

<span data-ttu-id="dd746-241">*Mennyi időt vesz igénybe a gyűjtött adatok toobe?*</span><span class="sxs-lookup"><span data-stu-id="dd746-241">*How long does it take for data toobe collected?*</span></span>

* <span data-ttu-id="dd746-242">hello tényleges adatgyűjtés hello kiszolgálón körülbelül 1 órát vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="dd746-242">hello actual data collection on hello server takes about 1 hour.</span></span> <span data-ttu-id="dd746-243">Azt is tovább tarthat, amely az SQL Server-példányok vagy adatbázisok sok kiszolgálókon.</span><span class="sxs-lookup"><span data-stu-id="dd746-243">It may take longer on servers that have a large number of SQL instances or databases.</span></span>

<span data-ttu-id="dd746-244">*Milyen típusú adatok gyűjtése?*</span><span class="sxs-lookup"><span data-stu-id="dd746-244">*What type of data is collected?*</span></span>

* <span data-ttu-id="dd746-245">a következő típusú adatok hello gyűjtik:</span><span class="sxs-lookup"><span data-stu-id="dd746-245">hello following types of data are collected:</span></span>
  * <span data-ttu-id="dd746-246">WMI</span><span class="sxs-lookup"><span data-stu-id="dd746-246">WMI</span></span>
  * <span data-ttu-id="dd746-247">Beállításkulcs</span><span class="sxs-lookup"><span data-stu-id="dd746-247">Registry</span></span>
  * <span data-ttu-id="dd746-248">Teljesítményszámlálók</span><span class="sxs-lookup"><span data-stu-id="dd746-248">Performance counters</span></span>
  * <span data-ttu-id="dd746-249">Az SQL-dinamikus felügyeleti nézetek (DMV).</span><span class="sxs-lookup"><span data-stu-id="dd746-249">SQL dynamic management views (DMV).</span></span>

<span data-ttu-id="dd746-250">*Van olyan módon tooconfigure adatainak a gyűjtése?*</span><span class="sxs-lookup"><span data-stu-id="dd746-250">*Is there a way tooconfigure when data is collected?*</span></span>

* <span data-ttu-id="dd746-251">Jelenleg nem.</span><span class="sxs-lookup"><span data-stu-id="dd746-251">Not at this time.</span></span>

<span data-ttu-id="dd746-252">*Miért kell tooconfigure egy futtató fiókot?*</span><span class="sxs-lookup"><span data-stu-id="dd746-252">*Why do I have tooconfigure a Run As Account?*</span></span>

* <span data-ttu-id="dd746-253">Az SQL Server az SQL-lekérdezések kis számú futnak.</span><span class="sxs-lookup"><span data-stu-id="dd746-253">For SQL Server, a small number of SQL queries are run.</span></span> <span data-ttu-id="dd746-254">Ahhoz, hogy azok toorun, egy futtató fiókot a VIEW SERVER STATE engedélyek tooSQL kell használni.</span><span class="sxs-lookup"><span data-stu-id="dd746-254">In order for them toorun, a Run As Account with VIEW SERVER STATE permissions tooSQL must be used.</span></span>  <span data-ttu-id="dd746-255">Emellett a rendelés tooquery WMI, a helyi rendszergazdai hitelesítő adatokra szükség.</span><span class="sxs-lookup"><span data-stu-id="dd746-255">In addition, in order tooquery WMI, local administrator credentials are required.</span></span>

<span data-ttu-id="dd746-256">*Miért meg csak hello első 10 javaslatok?*</span><span class="sxs-lookup"><span data-stu-id="dd746-256">*Why display only hello top 10 recommendations?*</span></span>

* <span data-ttu-id="dd746-257">Helyett felkínálva feladatok túlságosan teljesnek, javasoljuk, összpontosítani előrébb hello javaslatok először címzést.</span><span class="sxs-lookup"><span data-stu-id="dd746-257">Instead of giving you an exhaustive overwhelming list of tasks, we recommend that you focus on addressing hello prioritized recommendations first.</span></span> <span data-ttu-id="dd746-258">Miután foglalkozni velük, további javaslatokat is elérhető lesz.</span><span class="sxs-lookup"><span data-stu-id="dd746-258">After you address them, additional recommendations will become available.</span></span> <span data-ttu-id="dd746-259">Ha jobban szeret toosee hello részletes listáját, hello OMS naplófájl-keresési ajánlások tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="dd746-259">If you prefer toosee hello detailed list, you can view all recommendations using hello OMS log search.</span></span>

<span data-ttu-id="dd746-260">*Van olyan módon tooignore ajánlást?*</span><span class="sxs-lookup"><span data-stu-id="dd746-260">*Is there a way tooignore a recommendation?*</span></span>

* <span data-ttu-id="dd746-261">Igen, tekintse meg [figyelmen kívül hagyja a javaslatok](#ignore-recommendations) fenti szakaszban.</span><span class="sxs-lookup"><span data-stu-id="dd746-261">Yes, see [Ignore recommendations](#ignore-recommendations) section above.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dd746-262">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="dd746-262">Next steps</span></span>
* <span data-ttu-id="dd746-263">[Naplók keresése](log-analytics-log-searches.md) tooview részletes SQL megfelelőségvizsgálati adatai és a javaslatok.</span><span class="sxs-lookup"><span data-stu-id="dd746-263">[Search logs](log-analytics-log-searches.md) tooview detailed SQL Assessment data and recommendations.</span></span>
