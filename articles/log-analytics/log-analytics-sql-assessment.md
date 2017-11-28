---
title: "Az SQL Server-környezet az Azure Naplóelemzés optimalizálása |} Microsoft Docs"
description: "Az Azure Naplóelemzés az SQL-értékelési megoldás segítségével rendszeres időközönkénti a kockázat és az SQL server-környezetek állapotának ellenőrzéséhez."
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
ms.openlocfilehash: d2aed3315fe60ace46dfb4176dc13aa417257b0c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="optimize-your-sql-server-environment-with-the-sql-assessment-solution-in-log-analytics"></a><span data-ttu-id="2570d-103">Az SQL Server-környezet a Naplóelemzési SQL értékelési megoldás optimalizálása</span><span class="sxs-lookup"><span data-stu-id="2570d-103">Optimize your SQL Server environment with the SQL Assessment solution in Log Analytics</span></span>

![SQL Assessment szimbólum](./media/log-analytics-sql-assessment/sql-assessment-symbol.png)

<span data-ttu-id="2570d-105">Az SQL-értékelési megoldás segítségével rendszeres időközönkénti kockázat és a környezetek állapotának megállapítása.</span><span class="sxs-lookup"><span data-stu-id="2570d-105">You can use the SQL Assessment solution to assess the risk and health of your server environments on a regular interval.</span></span> <span data-ttu-id="2570d-106">Ez a cikk segít a megoldás telepítéséhez, úgy, hogy a potenciális problémákat korrekciós műveletek hajthatók végre.</span><span class="sxs-lookup"><span data-stu-id="2570d-106">This article will help you install the solution so that you can take corrective actions for potential problems.</span></span>

<span data-ttu-id="2570d-107">Ez a megoldás a telepített kiszolgálói infrastruktúra vonatkozó javaslatok a rangsorolt listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="2570d-107">This solution provides a prioritized list of recommendations specific to your deployed server infrastructure.</span></span> <span data-ttu-id="2570d-108">A javaslatok szerint vannak kategóriába között hat fókuszterületre vonatkozóan, amely gyorsan ismertetése a kockázat, és hajtsa végre a javítási műveletet.</span><span class="sxs-lookup"><span data-stu-id="2570d-108">The recommendations are categorized across six focus areas which help you quickly understand the risk and take corrective action.</span></span>

<span data-ttu-id="2570d-109">Az ajánlásokat tudással és a Microsoft szakemberei ügyfél látogatások ezer tapasztalatai alapulnak.</span><span class="sxs-lookup"><span data-stu-id="2570d-109">The recommendations made are based on the knowledge and experience gained by Microsoft engineers from thousands of customer visits.</span></span> <span data-ttu-id="2570d-110">Minden ajánlást ismerteti, miért probléma előfordulhat, hogy lényeges, hogy Önnek, és előfordulhat, hogy a javasolt.</span><span class="sxs-lookup"><span data-stu-id="2570d-110">Each recommendation provides guidance about why an issue might matter to you and how to implement the suggested changes.</span></span>

<span data-ttu-id="2570d-111">Kiválaszthatja a fókuszterületre vonatkozóan, amely a szervezet számára fontos, és a nyomon követni a kockázat szabad és megfelelő környezet futtató felé.</span><span class="sxs-lookup"><span data-stu-id="2570d-111">You can choose focus areas that are most important to your organization and track your progress toward running a risk free and healthy environment.</span></span>

<span data-ttu-id="2570d-112">Után, a megoldás felvett értékelését fejezhető be, összefoglaló adatait fókuszterületek jelenik meg a **SQL Assessment** irányítópult az infrastruktúrát a környezetben.</span><span class="sxs-lookup"><span data-stu-id="2570d-112">After you've added the solution and an assessment is completed, summary information for focus areas is shown on the **SQL Assessment** dashboard for the infrastructure in your environment.</span></span> <span data-ttu-id="2570d-113">A következő szakaszok ismertetik, hogyan használja az információk a **SQL Assessment** irányítópultot, ahol megtekintheti, és ezután javasolt műveletek a SQL server-infrastruktúrában.</span><span class="sxs-lookup"><span data-stu-id="2570d-113">The following sections describe how to use the information on the **SQL Assessment** dashboard, where you can view and then take recommended actions for your SQL server infrastructure.</span></span>

![SQL Assessment csempe képe](./media/log-analytics-sql-assessment/sql-assess-tile.png)

![SQL Assessment irányítópult képe](./media/log-analytics-sql-assessment/sql-assess-dash.png)

## <a name="installing-and-configuring-the-solution"></a><span data-ttu-id="2570d-116">Telepítése és a megoldás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2570d-116">Installing and configuring the solution</span></span>
<span data-ttu-id="2570d-117">A Standard, Developer és Enterprise kiadás SQL Server jelenleg támogatott verziói SQL Assessment működik.</span><span class="sxs-lookup"><span data-stu-id="2570d-117">SQL Assessment works with all currently supported versions of SQL Server for the Standard, Developer, and Enterprise editions.</span></span>

<span data-ttu-id="2570d-118">Az alábbi információk segítségével telepítse és konfigurálja a megoldást.</span><span class="sxs-lookup"><span data-stu-id="2570d-118">Use the following information to install and configure the solution.</span></span>

* <span data-ttu-id="2570d-119">Ügynökök telepítenie kell a kiszolgálókat, az SQL Server telepítve van.</span><span class="sxs-lookup"><span data-stu-id="2570d-119">Agents must be installed on servers that have SQL Server installed.</span></span>
* <span data-ttu-id="2570d-120">Az SQL-értékelési megoldás minden számítógépen, amelyen OMS-ügynököt telepített .NET-keretrendszer 4 támogatott verziója szükséges.</span><span class="sxs-lookup"><span data-stu-id="2570d-120">The SQL Assessment solution requires a supported version of .NET Framework 4 installed on each computer that has an OMS agent.</span></span>
* <span data-ttu-id="2570d-121">A megoldás telepítéséhez a felhasználónak kell rendszergazdaként vagy közreműködői az Azure-előfizetések az Azure portál használata.</span><span class="sxs-lookup"><span data-stu-id="2570d-121">In order to install the solution, the user must be an administrator or contributor to the Azure subscription when using the Azure portal.</span></span> <span data-ttu-id="2570d-122">Emellett a felhasználó az OMS-munkaterület közreműködői csoportjának tagja vagy az OMS-portál rendszergazdája kell, hogy legyen.</span><span class="sxs-lookup"><span data-stu-id="2570d-122">In addition, the user must be a member of the OMS workspace contributor or administrator role in the OMS portal.</span></span>
* <span data-ttu-id="2570d-123">Ha az Operations Manager-ügynök használ SQL Assessment, szüksége lesz az Operations Manager Run-As fiók használatára.</span><span class="sxs-lookup"><span data-stu-id="2570d-123">When using the Operations Manager agent with SQL Assessment, you'll need to use an Operations Manager Run-As account.</span></span> <span data-ttu-id="2570d-124">Lásd: [Operations Manager futtató fiókok az OMS](#operations-manager-run-as-accounts-for-oms) alább olvashat.</span><span class="sxs-lookup"><span data-stu-id="2570d-124">See [Operations Manager run-as accounts for OMS](#operations-manager-run-as-accounts-for-oms) below for more information.</span></span>

  > [!NOTE]
  > <span data-ttu-id="2570d-125">Az MMA ügynök nem támogatja az Operations Manager Run-As fiókokat.</span><span class="sxs-lookup"><span data-stu-id="2570d-125">The MMA agent does not support Operations Manager Run-As accounts.</span></span>
  >
  >
* <span data-ttu-id="2570d-126">Az SQL-értékelési megoldás hozzáadása az OMS-munkaterület ismertetett eljárással [hozzáadni a Naplóelemzési megoldások a megoldások gyűjteményből](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="2570d-126">Add the SQL Assessment solution to your OMS workspace using the process described in [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md).</span></span> <span data-ttu-id="2570d-127">Nincs szükség további konfigurációra.</span><span class="sxs-lookup"><span data-stu-id="2570d-127">There is no further configuration required.</span></span>

> [!NOTE]
> <span data-ttu-id="2570d-128">A megoldás hozzáadása után a AdvisorAssessment.exe fájl kerül kiszolgálók ügynökkel.</span><span class="sxs-lookup"><span data-stu-id="2570d-128">After you've added the solution, the AdvisorAssessment.exe file is added to servers with agents.</span></span> <span data-ttu-id="2570d-129">Konfigurációs adatok olvasása és küldi el feldolgozásra a felhőben az OMS szolgáltatáshoz a rendszer.</span><span class="sxs-lookup"><span data-stu-id="2570d-129">Configuration data is read and then sent to the OMS service in the cloud for processing.</span></span> <span data-ttu-id="2570d-130">A fogadott adatokhoz logika vonatkozik, és a felhőszolgáltatás-adatait rögzíti.</span><span class="sxs-lookup"><span data-stu-id="2570d-130">Logic is applied to the received data and the cloud service records the data.</span></span>

## <a name="sql-assessment-data-collection-details"></a><span data-ttu-id="2570d-131">SQL Assessment az gyűjtemény adatait</span><span class="sxs-lookup"><span data-stu-id="2570d-131">SQL Assessment data collection details</span></span>
<span data-ttu-id="2570d-132">SQL Assessment gyűjti a WMI-adatok, a beállításjegyzék-adatok, a teljesítményadatokat és az SQL Server dinamikus felügyeleti nézetben eredményeket az ügynökök, amelyeken engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="2570d-132">SQL Assessment collects WMI data, registry data, performance data, and SQL Server dynamic management view results using the agents that you have enabled.</span></span>

<span data-ttu-id="2570d-133">Az alábbi táblázat adatgyűjtési módszerek ügynökök, az Operations Manager (SCOM) szükséges-e, és milyen gyakran adatgyűjtés ügynök által.</span><span class="sxs-lookup"><span data-stu-id="2570d-133">The following table shows data collection methods for agents, whether Operations Manager (SCOM) is required, and how often data is collected by an agent.</span></span>

| <span data-ttu-id="2570d-134">Platform</span><span class="sxs-lookup"><span data-stu-id="2570d-134">platform</span></span> | <span data-ttu-id="2570d-135">Közvetlen ügynök</span><span class="sxs-lookup"><span data-stu-id="2570d-135">Direct Agent</span></span> | <span data-ttu-id="2570d-136">SCOM-ügynököt</span><span class="sxs-lookup"><span data-stu-id="2570d-136">SCOM agent</span></span> | <span data-ttu-id="2570d-137">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="2570d-137">Azure Storage</span></span> | <span data-ttu-id="2570d-138">SCOM szükséges?</span><span class="sxs-lookup"><span data-stu-id="2570d-138">SCOM required?</span></span> | <span data-ttu-id="2570d-139">Felügyeleti csoport keresztül küldött SCOM ügynök adatok</span><span class="sxs-lookup"><span data-stu-id="2570d-139">SCOM agent data sent via management group</span></span> | <span data-ttu-id="2570d-140">Gyűjtemény gyakorisága</span><span class="sxs-lookup"><span data-stu-id="2570d-140">collection frequency</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="2570d-141">Windows</span><span class="sxs-lookup"><span data-stu-id="2570d-141">Windows</span></span> | <span data-ttu-id="2570d-142">&#8226;</span><span class="sxs-lookup"><span data-stu-id="2570d-142">&#8226;</span></span> | <span data-ttu-id="2570d-143">&#8226;</span><span class="sxs-lookup"><span data-stu-id="2570d-143">&#8226;</span></span> |  |  | <span data-ttu-id="2570d-144">&#8226;</span><span class="sxs-lookup"><span data-stu-id="2570d-144">&#8226;</span></span> |<span data-ttu-id="2570d-145">7 nap</span><span class="sxs-lookup"><span data-stu-id="2570d-145">7 days</span></span> |

## <a name="operations-manager-run-as-accounts-for-oms"></a><span data-ttu-id="2570d-146">Az Operations Manager futtató fiókot a következő OMS</span><span class="sxs-lookup"><span data-stu-id="2570d-146">Operations Manager run-as accounts for OMS</span></span>
<span data-ttu-id="2570d-147">Az OMS szolgáltatáshoz használja az Operations Manager ügynök és a felügyeleti csoport gyűjtése és adatok elküldése az OMS szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="2570d-147">Log Analytics in OMS uses the Operations Manager agent and management group to collect and send data to the OMS service.</span></span> <span data-ttu-id="2570d-148">OMS buildek esetén munkaterhelésekhez adja meg a felügyeleti csomagok érték-szolgáltatások hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="2570d-148">OMS builds upon management packs for workloads to provide value-add services.</span></span> <span data-ttu-id="2570d-149">Egyes munkaterhelések jogokkal munkaterhelés-specifikus felügyeleti csomagok eltérő biztonsági környezetben, például egy olyan tartományi fiók futtatásához.</span><span class="sxs-lookup"><span data-stu-id="2570d-149">Each workload requires workload-specific privileges to run management packs in a different security context, such as a domain account.</span></span> <span data-ttu-id="2570d-150">Meg kell adnia a hitelesítő adatokat egy Operations Manager futtató fiókja beállításával.</span><span class="sxs-lookup"><span data-stu-id="2570d-150">You need to provide credential information by configuring an Operations Manager Run As account.</span></span>

<span data-ttu-id="2570d-151">Az alábbi információk segítségével állítsa be az Operations Manager futtató fiókja SQL értékeléséhez.</span><span class="sxs-lookup"><span data-stu-id="2570d-151">Use the following information to set the Operations Manager run-as account for SQL Assessment.</span></span>

### <a name="set-the-run-as-account-for-sql-assessment"></a><span data-ttu-id="2570d-152">A futtató fiókhoz tartozó SQL-értékelés beállítása</span><span class="sxs-lookup"><span data-stu-id="2570d-152">Set the Run As account for SQL assessment</span></span>
 <span data-ttu-id="2570d-153">Ha az SQL Server felügyeleti csomag már használ, a futtató fiókot kell használnia.</span><span class="sxs-lookup"><span data-stu-id="2570d-153">If you are already using the SQL Server management pack, you should use that Run As account.</span></span>

#### <a name="to-configure-the-sql-run-as-account-in-the-operations-console"></a><span data-ttu-id="2570d-154">Az SQL futtató fiók konfigurálása az operatív konzolon</span><span class="sxs-lookup"><span data-stu-id="2570d-154">To configure the SQL Run As account in the Operations console</span></span>
> [!NOTE]
> <span data-ttu-id="2570d-155">Ha a közvetlen OMS-ügynököt, nem pedig az SCOM-ügynököt használ, a felügyeleti csomag mindig a helyi rendszer fiók biztonsági környezetében fut.</span><span class="sxs-lookup"><span data-stu-id="2570d-155">If you are using the OMS direct agent, rather than the SCOM agent, the management pack always runs in the security context of the Local System account.</span></span> <span data-ttu-id="2570d-156">Hagyja ki a lépéseket 1-5 az alábbi, és futtassa a T-SQL vagy a Powershell-példa, adja meg az NT AUTHORITY\SYSTEM felhasználónevet.</span><span class="sxs-lookup"><span data-stu-id="2570d-156">Skip steps 1-5 below, and run either the T-SQL or Powershell sample, specifying NT AUTHORITY\SYSTEM as the user name.</span></span>
>
>

1. <span data-ttu-id="2570d-157">Az Operations Manager programban nyissa meg az operatív konzolt, és kattintson **felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="2570d-157">In Operations Manager, open the Operations console, and then click **Administration**.</span></span>
2. <span data-ttu-id="2570d-158">A **futtató konfiguráció**, kattintson a **profilok**, és nyissa meg a **OMS SQL Assessment futtató profil**.</span><span class="sxs-lookup"><span data-stu-id="2570d-158">Under **Run As Configuration**, click **Profiles**, and open **OMS SQL Assessment Run As Profile**.</span></span>
3. <span data-ttu-id="2570d-159">Az a **futtató fiókok** kattintson **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="2570d-159">On the **Run As Accounts** page, click **Add**.</span></span>
4. <span data-ttu-id="2570d-160">Válassza ki a Windows futtató fiókhoz, amely tartalmazza az SQL Server szükséges hitelesítő adatokat, vagy kattintson a **új** kattintva létrehozhat egyet.</span><span class="sxs-lookup"><span data-stu-id="2570d-160">Select a Windows Run As account that contains the credentials needed for SQL Server, or click **New** to create one.</span></span>

   > [!NOTE]
   > <span data-ttu-id="2570d-161">A futtató fiók típusú Windows kell lennie.</span><span class="sxs-lookup"><span data-stu-id="2570d-161">The Run As account type must be Windows.</span></span> <span data-ttu-id="2570d-162">A futtató fiók a helyi Rendszergazdák csoport összes SQL Server-példányokat futtató Windows-kiszolgálók részét is kell.</span><span class="sxs-lookup"><span data-stu-id="2570d-162">The Run As account must also be part of Local Administrators group on all Windows Servers hosting SQL Server Instances.</span></span>
   >
   >
5. <span data-ttu-id="2570d-163">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="2570d-163">Click **Save**.</span></span>
6. <span data-ttu-id="2570d-164">Módosíthatja, és majd hajtható végre a következő T-SQL-minta SQL értékelése futtató fiók szükséges minimális engedélyeket adni minden SQL Server-példányon.</span><span class="sxs-lookup"><span data-stu-id="2570d-164">Modify and then execute the following T-SQL sample on each SQL Server Instance to grant minimum permissions required to Run As Account to perform SQL Assessment.</span></span> <span data-ttu-id="2570d-165">Azonban nem kell tennie, ha a futtató fiók már tartozik a sysadmin (rendszergazda) kiszolgálói szerepkört az SQL Server-példányok.</span><span class="sxs-lookup"><span data-stu-id="2570d-165">However, you don’t need to do this if a Run As Account is already part of the sysadmin server role on SQL Server Instances.</span></span>

```
---
    -- Replace <UserName> with the actual user name being used as Run As Account.
    USE master

    -- Create login for the user, comment this line if login is already created.
    CREATE LOGIN [<UserName>] FROM WINDOWS

    -- Grant permissions to user.
    GRANT VIEW SERVER STATE TO [<UserName>]
    GRANT VIEW ANY DEFINITION TO [<UserName>]
    GRANT VIEW ANY DATABASE TO [<UserName>]

    -- Add database user for all the databases on SQL Server Instance, this is required for connecting to individual databases.
    -- NOTE: This command must be run anytime new databases are added to SQL Server instances.
    EXEC sp_msforeachdb N'USE [?]; CREATE USER [<UserName>] FOR LOGIN [<UserName>];'

```
#### <a name="to-configure-the-sql-run-as-account-using-windows-powershell"></a><span data-ttu-id="2570d-166">A Windows PowerShell használatával az SQL futtató fiók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2570d-166">To configure the SQL Run As account using Windows PowerShell</span></span>
<span data-ttu-id="2570d-167">Nyisson meg egy PowerShell-ablakot, és futtassa a következő parancsfájlt, miután frissítette a információkkal:</span><span class="sxs-lookup"><span data-stu-id="2570d-167">Open a PowerShell window and run the following script after you’ve updated it with your information:</span></span>

```

    import-module OperationsManager
    New-SCOMManagementGroupConnection "<your management group name>"

    $profile = Get-SCOMRunAsProfile -DisplayName "OMS SQL Assessment Run As Profile"
    $account = Get-SCOMrunAsAccount | Where-Object {$_.Name -eq "<your run as account name>"}
    Set-SCOMRunAsProfile -Action "Add" -Profile $Profile -Account $Account
```

## <a name="understanding-how-recommendations-are-prioritized"></a><span data-ttu-id="2570d-168">Hogyan kerülnek előrébb a javaslatok megértése</span><span class="sxs-lookup"><span data-stu-id="2570d-168">Understanding how recommendations are prioritized</span></span>
<span data-ttu-id="2570d-169">Minden javaslat végrehajtott, amely azonosítja a relatív fontosságát az ajánlás súlyozási értéket kap.</span><span class="sxs-lookup"><span data-stu-id="2570d-169">Every recommendation made is given a weighting value that identifies the relative importance of the recommendation.</span></span> <span data-ttu-id="2570d-170">Csak a tíz legfontosabb javaslatok láthatók.</span><span class="sxs-lookup"><span data-stu-id="2570d-170">Only the ten most important recommendations are shown.</span></span>

### <a name="how-weights-are-calculated"></a><span data-ttu-id="2570d-171">Hogyan súlyok kiszámítása</span><span class="sxs-lookup"><span data-stu-id="2570d-171">How weights are calculated</span></span>
<span data-ttu-id="2570d-172">Súlyozás alapján három kulcsfontosságú szerepet játszik az összesített értékek:</span><span class="sxs-lookup"><span data-stu-id="2570d-172">Weightings are aggregate values based on three key factors:</span></span>

* <span data-ttu-id="2570d-173">A *valószínűség* , hogy azonosított problémát okoz problémát.</span><span class="sxs-lookup"><span data-stu-id="2570d-173">The *probability* that an issue identified will cause problems.</span></span> <span data-ttu-id="2570d-174">Nagyobb a valószínűsége annak megfelel az ajánlás nagyobb általános pontszámot.</span><span class="sxs-lookup"><span data-stu-id="2570d-174">A higher probability equates to a larger overall score for the recommendation.</span></span>
* <span data-ttu-id="2570d-175">A *hatás* , a problémát a szervezetben, ha azt okozzák a problémát.</span><span class="sxs-lookup"><span data-stu-id="2570d-175">The *impact* of the issue on your organization if it does cause a problem.</span></span> <span data-ttu-id="2570d-176">Egy magasabb hatása megfelel az ajánlás nagyobb általános pontszámot.</span><span class="sxs-lookup"><span data-stu-id="2570d-176">A higher impact equates to a larger overall score for the recommendation.</span></span>
* <span data-ttu-id="2570d-177">A *elérhető* a javaslat végrehajtásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="2570d-177">The *effort* required to implement the recommendation.</span></span> <span data-ttu-id="2570d-178">Egy újabb elérhető megfelel az ajánlás kisebb általános pontszámot.</span><span class="sxs-lookup"><span data-stu-id="2570d-178">A higher effort equates to a smaller overall score for the recommendation.</span></span>

<span data-ttu-id="2570d-179">Minden ajánlást a súlyozási arányában a teljes pontszám minden fókusz területen érhető el.</span><span class="sxs-lookup"><span data-stu-id="2570d-179">The weighting for each recommendation is expressed as a percentage of the total score available for each focus area.</span></span> <span data-ttu-id="2570d-180">Például ha a biztonsági és megfelelőségi fókusz területen ajánlást 5 %-os pontszámot, amelyekre érvényes végrehajtási növeli a teljes biztonsági és megfelelőségi pontszám által 5 %.</span><span class="sxs-lookup"><span data-stu-id="2570d-180">For example, if a recommendation in the Security and Compliance focus area has a score of 5%, implementing that recommendation will increase your overall Security and Compliance score by 5%.</span></span>

### <a name="focus-areas"></a><span data-ttu-id="2570d-181">Fókuszterületek</span><span class="sxs-lookup"><span data-stu-id="2570d-181">Focus areas</span></span>
<span data-ttu-id="2570d-182">**Biztonsági és megfelelőségi** -e fókuszba területen látható esetleges biztonsági fenyegetéseket jelezhetnek és a behatolás, a vállalati házirendeket és a technikai, jogszabályi és szabályozásoknak megfelelőségi előírásokra vonatkozó javaslatok.</span><span class="sxs-lookup"><span data-stu-id="2570d-182">**Security and Compliance** - This focus area shows recommendations for potential security threats and breaches, corporate policies, and technical, legal and regulatory compliance requirements.</span></span>

<span data-ttu-id="2570d-183">**Rendelkezésre állás és üzleti folytonosság** -fókusz itt megtekinthető a szolgáltatás rendelkezésre állása, a rugalmasság, az infrastruktúra és az üzleti védelmét javaslatok.</span><span class="sxs-lookup"><span data-stu-id="2570d-183">**Availability and Business Continuity** - This focus area shows recommendations for service availability, resiliency of your infrastructure, and business protection.</span></span>

<span data-ttu-id="2570d-184">**Teljesítmény és méretezhetőség** -e fókuszba területen látható ajánlásokat a szervezet informatikai infrastruktúrájának nő, győződjön meg arról, hogy az informatikai környezet megfelel az aktuális teljesítménykövetelményeknek, és képes reagálni a infrastruktúrához.</span><span class="sxs-lookup"><span data-stu-id="2570d-184">**Performance and Scalability** - This focus area shows recommendations to help your organization's IT infrastructure grow, ensure that your IT environment meets current performance requirements, and is able to respond to changing infrastructure needs.</span></span>

<span data-ttu-id="2570d-185">**Áttelepítés és a telepítés frissítéséhez** -e fókuszba területen látható ajánlásokat frissítéséhez, telepítse át, és az SQL Server telepítéséhez a meglévő infrastruktúra.</span><span class="sxs-lookup"><span data-stu-id="2570d-185">**Upgrade, Migration and Deployment** - This focus area shows recommendations to help you upgrade, migrate, and deploy SQL Server to your existing infrastructure.</span></span>

<span data-ttu-id="2570d-186">**Műveletek és -figyelő** -e fókuszba területen látható az IT-üzemeltetők egyszerűsítésére, megvalósítása megelőző karbantartási és teljesítmény maximalizálása érdekében ajánlott.</span><span class="sxs-lookup"><span data-stu-id="2570d-186">**Operations and Monitoring** - This focus area shows recommendations to help streamline your IT operations, implement preventative maintenance, and maximize performance.</span></span>

<span data-ttu-id="2570d-187">**Változás- és konfigurációkezelés** -e fókuszba területen látható ajánlások a napi tevékenységek védje, biztosítására, hogy módosításokat nem negatív hatással vannak az infrastruktúra Változáskezelő eljárások, létrehozásához, és nyomon követése és naplózása rendszerkonfigurációk.</span><span class="sxs-lookup"><span data-stu-id="2570d-187">**Change and Configuration Management** - This focus area shows recommendations to help protect day-to-day operations, ensure that changes don't negatively affect your infrastructure, establish change control procedures, and to track and audit system configurations.</span></span>

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a><span data-ttu-id="2570d-188">Kell megcéloznia 100 %-os pontozása minden fókusz területen?</span><span class="sxs-lookup"><span data-stu-id="2570d-188">Should you aim to score 100% in every focus area?</span></span>
<span data-ttu-id="2570d-189">Nem feltétlenül.</span><span class="sxs-lookup"><span data-stu-id="2570d-189">Not necessarily.</span></span> <span data-ttu-id="2570d-190">Az ajánlások a Tudásbázis és a Microsoft szakemberei által ügyfél látogatások ezer keresztül szerzett tapasztalatok alapulnak.</span><span class="sxs-lookup"><span data-stu-id="2570d-190">The recommendations are based on the knowledge and experiences gained by Microsoft engineers across thousands of customer visits.</span></span> <span data-ttu-id="2570d-191">Azonban nincs két kiszolgáló infrastruktúrák megegyeznek, és előfordulhat, hogy több vagy kevesebb kapcsolódik, konkrét javaslatokért.</span><span class="sxs-lookup"><span data-stu-id="2570d-191">However, no two server infrastructures are the same, and specific recommendations may be more or less relevant to you.</span></span> <span data-ttu-id="2570d-192">Például bizonyos biztonsági javaslatok kevésbé fontos, ha a virtuális gépek nem érhetők el az Internet lehet.</span><span class="sxs-lookup"><span data-stu-id="2570d-192">For example, some security recommendations might be less relevant if your virtual machines are not exposed to the Internet.</span></span> <span data-ttu-id="2570d-193">Egyes rendelkezésre állási javaslatok lehet kevésbé alacsony prioritást ad hoc adatgyűjtés és a reporting Services.</span><span class="sxs-lookup"><span data-stu-id="2570d-193">Some availability recommendations may be less relevant for services that provide low priority ad hoc data collection and reporting.</span></span> <span data-ttu-id="2570d-194">Lehet, hogy egy összetett üzleti fontos problémák kevésbé fontos, hogy a kezdeti.</span><span class="sxs-lookup"><span data-stu-id="2570d-194">Issues that are important to a mature business may be less important to a start-up.</span></span> <span data-ttu-id="2570d-195">Érdemes lehet azonosítani a fókusz a prioritások, és keresse meg, hogy a pontszámokat változnak az idők.</span><span class="sxs-lookup"><span data-stu-id="2570d-195">You may want to identify which focus areas are your priorities and then look at how your scores change over time.</span></span>

<span data-ttu-id="2570d-196">Minden javaslat arról, hogy miért fontos útmutatást tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="2570d-196">Every recommendation includes guidance about why it is important.</span></span> <span data-ttu-id="2570d-197">Ezt az útmutatást kell használnia annak kiértékelésére, hogy a javaslat végrehajtási, az informatikai szolgáltatások természetét és a szervezete üzleti igényeinek.</span><span class="sxs-lookup"><span data-stu-id="2570d-197">You should use this guidance to evaluate whether implementing the recommendation is appropriate for you, given the nature of your IT services and the business needs of your organization.</span></span>

## <a name="use-assessment-focus-area-recommendations"></a><span data-ttu-id="2570d-198">Kiértékelés fókusz terület ajánlásai használata</span><span class="sxs-lookup"><span data-stu-id="2570d-198">Use assessment focus area recommendations</span></span>
<span data-ttu-id="2570d-199">Egy értékelési megoldás az OMS használata előtt rendelkeznie kell a telepített megoldás.</span><span class="sxs-lookup"><span data-stu-id="2570d-199">Before you can use an assessment solution in OMS, you must have the solution installed.</span></span> <span data-ttu-id="2570d-200">További megoldások telepítéséről lásd: [hozzáadni a Naplóelemzési megoldások a megoldások gyűjteményből](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="2570d-200">To read more about installing solutions, see [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md).</span></span> <span data-ttu-id="2570d-201">Azt követően, megtekintheti az összegzés ajánlások SQL Assessment csempére az OMS-áttekintése oldalon használatával.</span><span class="sxs-lookup"><span data-stu-id="2570d-201">After it is installed, you can view the summary of recommendations by using the SQL Assessment tile on the Overview page in OMS.</span></span>

<span data-ttu-id="2570d-202">Az összesített megfelelőségi értékelése az infrastruktúrát, és a-feltárás javaslatok megtekintése.</span><span class="sxs-lookup"><span data-stu-id="2570d-202">View the summarized compliance assessments for your infrastructure and then drill-into recommendations.</span></span>

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a><span data-ttu-id="2570d-203">Az egy fókuszban terület javaslatok megtekintése és a szükséges javítási műveletek</span><span class="sxs-lookup"><span data-stu-id="2570d-203">To view recommendations for a focus area and take corrective action</span></span>
1. <span data-ttu-id="2570d-204">Az a **áttekintése** lapján kattintson a **SQL Assessment** csempére.</span><span class="sxs-lookup"><span data-stu-id="2570d-204">On the **Overview** page, click the **SQL Assessment** tile.</span></span>
2. <span data-ttu-id="2570d-205">Az a **SQL Assessment** lapon. Ellenőrizze az összefoglaló információkat a fókusz terület paneleken egyikében, majd kattintson egy adott fókusz területre javaslatok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="2570d-205">On the **SQL Assessment** page, review the summary information in one of the focus area blades and then click one to view recommendations for that focus area.</span></span>
3. <span data-ttu-id="2570d-206">A fókusz terület lapok egyikén tekintheti meg a környezetnek a rangsorolt ajánlásokat.</span><span class="sxs-lookup"><span data-stu-id="2570d-206">On any of the focus area pages, you can view the prioritized recommendations made for your environment.</span></span> <span data-ttu-id="2570d-207">Kattintson az ajánlás **érintett objektumok** miért a javaslatokkal kapcsolatos részletek megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="2570d-207">Click a recommendation under **Affected Objects** to view details about why the recommendation is made.</span></span>  
    <span data-ttu-id="2570d-208">![SQL-kiértékelés ajánlásai képe](./media/log-analytics-sql-assessment/sql-assess-focus.png)</span><span class="sxs-lookup"><span data-stu-id="2570d-208">![image of SQL Assessment recommendations](./media/log-analytics-sql-assessment/sql-assess-focus.png)</span></span>
4. <span data-ttu-id="2570d-209">Az ajánlott javítási műveletek hajthatók végre **javasolt műveletek**.</span><span class="sxs-lookup"><span data-stu-id="2570d-209">You can take corrective actions suggested in **Suggested Actions**.</span></span> <span data-ttu-id="2570d-210">A cikk intéztek, újabb értékelések fog jegyezze fel, az elvégzett műveletek, és a megfelelőségi pontszám növeli.</span><span class="sxs-lookup"><span data-stu-id="2570d-210">When the item has been addressed, later assessments will record that recommended actions were taken and your compliance score will increase.</span></span> <span data-ttu-id="2570d-211">Javított elemek jelennek meg **átadott objektumok**.</span><span class="sxs-lookup"><span data-stu-id="2570d-211">Corrected items appear as **Passed Objects**.</span></span>

## <a name="ignore-recommendations"></a><span data-ttu-id="2570d-212">Figyelmen kívül hagyja a javaslatok</span><span class="sxs-lookup"><span data-stu-id="2570d-212">Ignore recommendations</span></span>
<span data-ttu-id="2570d-213">Ha figyelmen kívül hagyása kívánt ajánlásokat, létrehozhat egy szövegfájlt, amely OMS fogja használni az értékelési eredmények jelennek meg javaslatok megelőzése érdekében.</span><span class="sxs-lookup"><span data-stu-id="2570d-213">If you have recommendations that you want to ignore, you can create a text file that OMS will use to prevent recommendations from appearing in your assessment results.</span></span>

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

### <a name="to-identify-recommendations-that-you-will-ignore"></a><span data-ttu-id="2570d-214">Javaslatok, figyelmen kívül hagyja majd azonosításához</span><span class="sxs-lookup"><span data-stu-id="2570d-214">To identify recommendations that you will ignore</span></span>
1. <span data-ttu-id="2570d-215">Jelentkezzen be a munkaterületet, és nyissa meg a keresési napló.</span><span class="sxs-lookup"><span data-stu-id="2570d-215">Sign in to your workspace and open Log Search.</span></span> <span data-ttu-id="2570d-216">A következő lekérdezés futtatásával lista ajánlásokat, amelyek nem tudták használni a környezetében.</span><span class="sxs-lookup"><span data-stu-id="2570d-216">Use the following query to list recommendations that have failed for computers in your environment.</span></span>

   ```
   Type=SQLAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```

   <span data-ttu-id="2570d-217">Ez a naplófájl-keresési lekérdezés ábrázoló képernyőkép: ![nem lehetett ajánlásait](./media/log-analytics-sql-assessment/sql-assess-failed-recommendations.png)</span><span class="sxs-lookup"><span data-stu-id="2570d-217">Here's a screen shot showing the Log Search query: ![failed recommendations](./media/log-analytics-sql-assessment/sql-assess-failed-recommendations.png)</span></span>
2. <span data-ttu-id="2570d-218">Válassza ki a javaslatok, amelyet szeretne figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="2570d-218">Choose recommendations that you want to ignore.</span></span> <span data-ttu-id="2570d-219">A RecommendationId az értékeket fogja használni a következő eljárásban.</span><span class="sxs-lookup"><span data-stu-id="2570d-219">You’ll use the values for RecommendationId in the next procedure.</span></span>

### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a><span data-ttu-id="2570d-220">Hozzon létre, és egy IgnoreRecommendations.txt szövegfájl használata</span><span class="sxs-lookup"><span data-stu-id="2570d-220">To create and use an IgnoreRecommendations.txt text file</span></span>
1. <span data-ttu-id="2570d-221">Hozzon létre egy IgnoreRecommendations.txt nevű fájlt.</span><span class="sxs-lookup"><span data-stu-id="2570d-221">Create a file named IgnoreRecommendations.txt.</span></span>
2. <span data-ttu-id="2570d-222">Illessze be, vagy adjon meg minden egyes RecommendationId a minden javaslat, amelyet az OMS figyelmen kívül hagyása külön sorban, és mentse és zárja be a fájlt.</span><span class="sxs-lookup"><span data-stu-id="2570d-222">Paste or type each RecommendationId for each recommendation that you want OMS to ignore on a separate line and then save and close the file.</span></span>
3. <span data-ttu-id="2570d-223">Helyezze el a fájlt a következő mappában található minden olyan számítógépen OMS figyelmen kívül hagyja a javaslatok, ahová.</span><span class="sxs-lookup"><span data-stu-id="2570d-223">Put the file in the following folder on each computer where you want OMS to ignore recommendations.</span></span>
   * <span data-ttu-id="2570d-224">A Microsoft Monitoring-ügynökkel rendelkező számítógépek (közvetlenül vagy az Operations Manager keresztül csatlakozik) - *SystemDrive*: \Program Files\Microsoft figyelés Agent\Agent</span><span class="sxs-lookup"><span data-stu-id="2570d-224">On computers with the Microsoft Monitoring Agent (connected directly or through Operations Manager) - *SystemDrive*:\Program Files\Microsoft Monitoring Agent\Agent</span></span>
   * <span data-ttu-id="2570d-225">Az Operations Manager felügyeleti kiszolgálón - *SystemDrive*: System Center 2012 R2\Operations Manager\Server \Program Files\Microsoft</span><span class="sxs-lookup"><span data-stu-id="2570d-225">On the Operations Manager management server - *SystemDrive*:\Program Files\Microsoft System Center 2012 R2\Operations Manager\Server</span></span>

### <a name="to-verify-that-recommendations-are-ignored"></a><span data-ttu-id="2570d-226">Győződjön meg arról, hogy javaslatokat figyelmen kívül hagyja a</span><span class="sxs-lookup"><span data-stu-id="2570d-226">To verify that recommendations are ignored</span></span>
1. <span data-ttu-id="2570d-227">Után a következő ütemezett értékelési fut, alapértelmezésben 7 naponta, a megadott javaslatok figyelmen kívül hagyott megjelölve, és nem jelenik meg az értékelés irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="2570d-227">After the next scheduled assessment runs, by default every 7 days, the specified recommendations are marked Ignored and will not appear on the assessment dashboard.</span></span>
2. <span data-ttu-id="2570d-228">A következő naplófájl-keresési lekérdezések segítségével a figyelmen kívül hagyott javaslatok listában.</span><span class="sxs-lookup"><span data-stu-id="2570d-228">You can use the following Log Search queries to list all the ignored recommendations.</span></span>

   ```
   Type=SQLAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```
3. <span data-ttu-id="2570d-229">Ha később úgy dönt, hogy szeretné-e figyelmen kívül hagyott ajánlott megtekintéséhez, távolítsa el a IgnoreRecommendations.txt fájlokat, vagy RecommendationIDs eltávolíthatja őket.</span><span class="sxs-lookup"><span data-stu-id="2570d-229">If you decide later that you want to see ignored recommendations, remove any IgnoreRecommendations.txt files, or you can remove RecommendationIDs from them.</span></span>

## <a name="sql-assessment-solution-faq"></a><span data-ttu-id="2570d-230">SQL-értékelési megoldás – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="2570d-230">SQL Assessment solution FAQ</span></span>
<span data-ttu-id="2570d-231">*Milyen gyakran fut a felmérés elvégzéséhez?*</span><span class="sxs-lookup"><span data-stu-id="2570d-231">*How often does an assessment run?*</span></span>

* <span data-ttu-id="2570d-232">Az értékelés 7 naponta fut.</span><span class="sxs-lookup"><span data-stu-id="2570d-232">The assessment runs every 7 days.</span></span>

<span data-ttu-id="2570d-233">*Van mód konfigurálása, hogy milyen gyakran az értékelés fut?*</span><span class="sxs-lookup"><span data-stu-id="2570d-233">*Is there a way to configure how often the assessment runs?*</span></span>

* <span data-ttu-id="2570d-234">Jelenleg nem.</span><span class="sxs-lookup"><span data-stu-id="2570d-234">Not at this time.</span></span>

<span data-ttu-id="2570d-235">*Ha egy másik kiszolgáló fel van derítve, miután az SQL-értékelési megoldás felvett, az értékelési?*</span><span class="sxs-lookup"><span data-stu-id="2570d-235">*If another server is discovered after I’ve added the SQL assessment solution, will it be assessed?*</span></span>

* <span data-ttu-id="2570d-236">Igen, ha kiderül, hogy megfelelőségét ellenőrizni kell, majd a gyakoriság 7 nap.</span><span class="sxs-lookup"><span data-stu-id="2570d-236">Yes, once it is discovered it is assessed from then on, every 7 days.</span></span>

<span data-ttu-id="2570d-237">*Ha egy kiszolgáló le van szerelve, ha azt eltávolítja a szolgáltatásból az értékelés?*</span><span class="sxs-lookup"><span data-stu-id="2570d-237">*If a server is decommissioned, when will it be removed from the assessment?*</span></span>

* <span data-ttu-id="2570d-238">Ha a kiszolgáló nem időközönként adatokat küldenek a 3 hét, a rendszer eltávolítja.</span><span class="sxs-lookup"><span data-stu-id="2570d-238">If a server does not submit data for 3 weeks, it is removed.</span></span>

<span data-ttu-id="2570d-239">*Mi az a folyamat, amelyet az adatgyűjtést a neve?*</span><span class="sxs-lookup"><span data-stu-id="2570d-239">*What is the name of the process that does the data collection?*</span></span>

* <span data-ttu-id="2570d-240">AdvisorAssessment.exe</span><span class="sxs-lookup"><span data-stu-id="2570d-240">AdvisorAssessment.exe</span></span>

<span data-ttu-id="2570d-241">*Mennyi időt vesz igénybe a gyűjtött adatok?*</span><span class="sxs-lookup"><span data-stu-id="2570d-241">*How long does it take for data to be collected?*</span></span>

* <span data-ttu-id="2570d-242">A tényleges adatok gyűjtése a kiszolgálón a körülbelül 1 órát vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="2570d-242">The actual data collection on the server takes about 1 hour.</span></span> <span data-ttu-id="2570d-243">Azt is tovább tarthat, amely az SQL Server-példányok vagy adatbázisok sok kiszolgálókon.</span><span class="sxs-lookup"><span data-stu-id="2570d-243">It may take longer on servers that have a large number of SQL instances or databases.</span></span>

<span data-ttu-id="2570d-244">*Milyen típusú adatok gyűjtése?*</span><span class="sxs-lookup"><span data-stu-id="2570d-244">*What type of data is collected?*</span></span>

* <span data-ttu-id="2570d-245">A következő típusú adatok gyűjtése történt:</span><span class="sxs-lookup"><span data-stu-id="2570d-245">The following types of data are collected:</span></span>
  * <span data-ttu-id="2570d-246">WMI</span><span class="sxs-lookup"><span data-stu-id="2570d-246">WMI</span></span>
  * <span data-ttu-id="2570d-247">Beállításkulcs</span><span class="sxs-lookup"><span data-stu-id="2570d-247">Registry</span></span>
  * <span data-ttu-id="2570d-248">Teljesítményszámlálók</span><span class="sxs-lookup"><span data-stu-id="2570d-248">Performance counters</span></span>
  * <span data-ttu-id="2570d-249">Az SQL-dinamikus felügyeleti nézetek (DMV).</span><span class="sxs-lookup"><span data-stu-id="2570d-249">SQL dynamic management views (DMV).</span></span>

<span data-ttu-id="2570d-250">*Van mód konfigurálása az adatgyűjtés?*</span><span class="sxs-lookup"><span data-stu-id="2570d-250">*Is there a way to configure when data is collected?*</span></span>

* <span data-ttu-id="2570d-251">Jelenleg nem.</span><span class="sxs-lookup"><span data-stu-id="2570d-251">Not at this time.</span></span>

<span data-ttu-id="2570d-252">*Miért kell konfigurálni egy futtató fiókot?*</span><span class="sxs-lookup"><span data-stu-id="2570d-252">*Why do I have to configure a Run As Account?*</span></span>

* <span data-ttu-id="2570d-253">Az SQL Server az SQL-lekérdezések kis számú futnak.</span><span class="sxs-lookup"><span data-stu-id="2570d-253">For SQL Server, a small number of SQL queries are run.</span></span> <span data-ttu-id="2570d-254">Ahhoz, hogy futtatni egy futtató fiókot az SQL VIEW SERVER STATE engedélyt kell használni.</span><span class="sxs-lookup"><span data-stu-id="2570d-254">In order for them to run, a Run As Account with VIEW SERVER STATE permissions to SQL must be used.</span></span>  <span data-ttu-id="2570d-255">Emellett ahhoz, hogy a lekérdezése a WMI, helyi rendszergazdai jogosultságok szükségesek.</span><span class="sxs-lookup"><span data-stu-id="2570d-255">In addition, in order to query WMI, local administrator credentials are required.</span></span>

<span data-ttu-id="2570d-256">*Miért meg csak az első 10 javaslatokat?*</span><span class="sxs-lookup"><span data-stu-id="2570d-256">*Why display only the top 10 recommendations?*</span></span>

* <span data-ttu-id="2570d-257">Helyett felkínálva feladatok túlságosan teljesnek, javasoljuk, összpontosítani a rangsorolt javaslatok először címzést.</span><span class="sxs-lookup"><span data-stu-id="2570d-257">Instead of giving you an exhaustive overwhelming list of tasks, we recommend that you focus on addressing the prioritized recommendations first.</span></span> <span data-ttu-id="2570d-258">Miután foglalkozni velük, további javaslatokat is elérhető lesz.</span><span class="sxs-lookup"><span data-stu-id="2570d-258">After you address them, additional recommendations will become available.</span></span> <span data-ttu-id="2570d-259">Ha szeretné a részletes listát talál, az OMS-napló keresési ajánlások tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="2570d-259">If you prefer to see the detailed list, you can view all recommendations using the OMS log search.</span></span>

<span data-ttu-id="2570d-260">*Van mód figyelmen kívül hagyja az ajánlás?*</span><span class="sxs-lookup"><span data-stu-id="2570d-260">*Is there a way to ignore a recommendation?*</span></span>

* <span data-ttu-id="2570d-261">Igen, tekintse meg [figyelmen kívül hagyja a javaslatok](#ignore-recommendations) fenti szakaszban.</span><span class="sxs-lookup"><span data-stu-id="2570d-261">Yes, see [Ignore recommendations](#ignore-recommendations) section above.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2570d-262">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2570d-262">Next steps</span></span>
* <span data-ttu-id="2570d-263">[Naplók keresése](log-analytics-log-searches.md) részletes SQL megfelelőségvizsgálati adatai és a javaslatok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="2570d-263">[Search logs](log-analytics-log-searches.md) to view detailed SQL Assessment data and recommendations.</span></span>
