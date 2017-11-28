---
title: "az Azure SQL database naplózási lépései aaaGet |} Microsoft Docs"
description: "Ismerkedés az Azure SQL adatbázis-naplózás"
services: sql-database
documentationcenter: 
author: giladm
manager: jhubbard
editor: giladm
ms.assetid: 89c2a155-c2fb-4b67-bc19-9b4e03c6d3bc
ms.service: sql-database
ms.custom: security
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: giladm
ms.openlocfilehash: 5494c602d702ac41992520f900c393a98cc7c989
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-sql-database-auditing"></a><span data-ttu-id="e9d70-103">Ismerkedés az SQL-adatbázis naplózási szolgáltatásával</span><span class="sxs-lookup"><span data-stu-id="e9d70-103">Get started with SQL database auditing</span></span>
<span data-ttu-id="e9d70-104">Azure SQL database auditing követi az adatbázisban, és beírja őket az Azure-tárfiókot tooan audit napló.</span><span class="sxs-lookup"><span data-stu-id="e9d70-104">Azure SQL database auditing tracks database events and writes them tooan audit log in your Azure storage account.</span></span> <span data-ttu-id="e9d70-105">A naplózás is:</span><span class="sxs-lookup"><span data-stu-id="e9d70-105">Auditing also:</span></span>

* <span data-ttu-id="e9d70-106">Segít törvényi megfelelőség fenntartásában, ismerje meg adatbázis-tevékenység, és azok az eltérések és rendellenességek, amelyek üzleti problémát jelenthetnek, vagy a biztonság megsértésére betekintést.</span><span class="sxs-lookup"><span data-stu-id="e9d70-106">Helps you maintain regulatory compliance, understand database activity, and gain insight into discrepancies and anomalies that could indicate business concerns or suspected security violations.</span></span>

* <span data-ttu-id="e9d70-107">Lehetővé teszi, és elősegíti a való toocompliance szabványok, de azt nem garantálja a megfelelőségi.</span><span class="sxs-lookup"><span data-stu-id="e9d70-107">Enables and facilitates adherence toocompliance standards, although it doesn't guarantee compliance.</span></span> <span data-ttu-id="e9d70-108">További információ az Azure programokat, hogy támogatási szabványoknak való megfelelés, a következő témakörben: hello [Azure biztonsági és adatkezelési központ](https://azure.microsoft.com/support/trust-center/compliance/).</span><span class="sxs-lookup"><span data-stu-id="e9d70-108">For more information about Azure programs that support standards compliance, see hello [Azure Trust Center](https://azure.microsoft.com/support/trust-center/compliance/).</span></span>


## <span data-ttu-id="e9d70-109"><a id="subheading-1"></a>Az Azure SQL adatbázis naplózásának áttekintése</span><span class="sxs-lookup"><span data-stu-id="e9d70-109"><a id="subheading-1"></a>Azure SQL database auditing overview</span></span>
<span data-ttu-id="e9d70-110">SQL-adatbázis a naplózás is használhatja:</span><span class="sxs-lookup"><span data-stu-id="e9d70-110">You can use SQL database auditing to:</span></span>


* <span data-ttu-id="e9d70-111">**Tartsa meg** kijelölt események egy naplóban.</span><span class="sxs-lookup"><span data-stu-id="e9d70-111">**Retain** an audit trail of selected events.</span></span> <span data-ttu-id="e9d70-112">Adatbázis-műveletek toobe naplózva kategóriáinak adhat meg.</span><span class="sxs-lookup"><span data-stu-id="e9d70-112">You can define categories of database actions toobe audited.</span></span>
* <span data-ttu-id="e9d70-113">**A jelentés** adatbázis-tevékenység.</span><span class="sxs-lookup"><span data-stu-id="e9d70-113">**Report** on database activity.</span></span> <span data-ttu-id="e9d70-114">Használhat előre konfigurált jelentések és egy irányítópult tooget használatának gyors megkezdése tevékenységet és az események naplózásához.</span><span class="sxs-lookup"><span data-stu-id="e9d70-114">You can use preconfigured reports and a dashboard tooget started quickly with activity and event reporting.</span></span>
* <span data-ttu-id="e9d70-115">**Elemezze** jelentéseket.</span><span class="sxs-lookup"><span data-stu-id="e9d70-115">**Analyze** reports.</span></span> <span data-ttu-id="e9d70-116">Gyanús események, a szokatlan tevékenység és a trendeket találja.</span><span class="sxs-lookup"><span data-stu-id="e9d70-116">You can find suspicious events, unusual activity, and trends.</span></span>

<span data-ttu-id="e9d70-117">Konfigurálhatja a kategóriák, különböző típusú naplózás hello leírtak [az adatbázis naplózásának beállítása](#subheading-2) szakasz.</span><span class="sxs-lookup"><span data-stu-id="e9d70-117">You can configure auditing for different types of event categories, as explained in hello [Set up auditing for your database](#subheading-2) section.</span></span>

<span data-ttu-id="e9d70-118">Naplók az Azure-előfizetése írt tooAzure Blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="e9d70-118">Audit logs are written tooAzure Blob storage on your Azure subscription.</span></span>


## <span data-ttu-id="e9d70-119"><a id="subheading-8"></a>Adja meg a kiszolgálói szintű és adatbázis-szintű naplózási házirend</span><span class="sxs-lookup"><span data-stu-id="e9d70-119"><a id="subheading-8"></a>Define server-level vs. database-level auditing policy</span></span>

<span data-ttu-id="e9d70-120">A naplózási házirend meghatározása egy adott adatbázis vagy az alapértelmezett házirend-kiszolgáló:</span><span class="sxs-lookup"><span data-stu-id="e9d70-120">An auditing policy can be defined for a specific database or as a default server policy:</span></span>

* <span data-ttu-id="e9d70-121">Kiszolgálói házirend vonatkozik tooall meglévő és az újonnan létrehozott adatbázisok hello kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="e9d70-121">A server policy applies tooall existing and newly created databases on hello server.</span></span>

* <span data-ttu-id="e9d70-122">Ha *server blobnaplózási funkció engedélyezve van*, azt *mindig érvényes toohello adatbázis* (Ez azt jelenti, hogy hello adatbázishoz lesznek naplózva), függetlenül hello adatbázis naplózási beállításait.</span><span class="sxs-lookup"><span data-stu-id="e9d70-122">If *server blob auditing is enabled*, it *always applies toohello database* (that is, hello database will be audited), regardless of hello database auditing settings.</span></span>

* <span data-ttu-id="e9d70-123">Engedélyezi a naplózást hello adatbázis hozzáadása tooenabling azt hello kiszolgálón blob lesz *nem* felülbírálás vagy hello hello blobnaplózási funkció beállításainak módosításához.</span><span class="sxs-lookup"><span data-stu-id="e9d70-123">Enabling blob auditing on hello database, in addition tooenabling it on hello server, will *not* override or change any of hello settings of hello server blob auditing.</span></span> <span data-ttu-id="e9d70-124">Mindkét naplózás jelen egymás mellett.</span><span class="sxs-lookup"><span data-stu-id="e9d70-124">Both audits will exist side by side.</span></span> <span data-ttu-id="e9d70-125">Más szóval hello adatbázishoz lesznek naplózva kétszer párhuzamosan (egyszer hello kiszolgálószabályzat és egyszer hello adatbázis házirend).</span><span class="sxs-lookup"><span data-stu-id="e9d70-125">In other words, hello database will be audited twice in parallel (once by hello server policy and once by hello database policy).</span></span>

   > [!NOTE]
   > <span data-ttu-id="e9d70-126">Kerülje engedélyezésével server blob naplózási, mind az adatbázis blobnaplózási funkció együtt, kivéve, ha:</span><span class="sxs-lookup"><span data-stu-id="e9d70-126">You should avoid enabling both server blob auditing and database blob auditing together, unless:</span></span>
    > * <span data-ttu-id="e9d70-127">Azt szeretné, hogy egy másik toouse *tárfiók* vagy *megőrzési időszak* adott adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="e9d70-127">You want toouse a different *storage account* or *retention period* for a specific database.</span></span>
    > * <span data-ttu-id="e9d70-128">Egy adott adatbázis, amely eltér a eseménytípusok vagy hello adatbázisok hello kiszolgálón hello többi naplóz kategóriákat kívánt tooaudit eseménytípusok vagy kategóriák.</span><span class="sxs-lookup"><span data-stu-id="e9d70-128">You want tooaudit event types or categories for a specific database that differ from event types or categories that are being audited for hello rest of hello databases on hello server.</span></span> <span data-ttu-id="e9d70-129">Például lehetséges, hogy csak az adott adatbázis naplózva toobe igénylő tábla beszúrása.</span><span class="sxs-lookup"><span data-stu-id="e9d70-129">For example, you might have table inserts that need toobe audited only for a specific database.</span></span>
   > 
   > <span data-ttu-id="e9d70-130">Ellenkező esetben azt javasoljuk, hogy engedélyezze a csak a kiszolgálói szintű blobnaplózási funkció, és hagyja hello adatbázis szintű naplózás le van tiltva az összes olyan adatbázis.</span><span class="sxs-lookup"><span data-stu-id="e9d70-130">Otherwise, we recommended that you enable only server-level blob auditing and leave hello database-level auditing disabled for all databases.</span></span>


## <span data-ttu-id="e9d70-131"><a id="subheading-2"></a>Az adatbázis naplózásának beállítása</span><span class="sxs-lookup"><span data-stu-id="e9d70-131"><a id="subheading-2"></a>Set up auditing for your database</span></span>
<span data-ttu-id="e9d70-132">hello következő szakasz írja le hello naplózási hello Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="e9d70-132">hello following section describes hello configuration of auditing using hello Azure portal.</span></span>

1. <span data-ttu-id="e9d70-133">Nyissa meg toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e9d70-133">Go toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="e9d70-134">Nyissa meg toohello **beállítások** hello SQL adatbázis vagy SQL server tooaudit kívánt panel.</span><span class="sxs-lookup"><span data-stu-id="e9d70-134">Go toohello **Settings** blade of hello SQL database/SQL server you want tooaudit.</span></span> <span data-ttu-id="e9d70-135">A hello **beállítások** panelen válassza **naplózási & Threat detection**.</span><span class="sxs-lookup"><span data-stu-id="e9d70-135">In hello **Settings** blade, select **Auditing & Threat detection**.</span></span>

    <span data-ttu-id="e9d70-136"><a id="auditing-screenshot"></a>![Navigációs ablaktábla][1]</span><span class="sxs-lookup"><span data-stu-id="e9d70-136"><a id="auditing-screenshot"></a> ![Navigation pane][1]</span></span>
3. <span data-ttu-id="e9d70-137">Kiszolgáló naplózási házirend (amelyek érvényben tooall meglévő és az újonnan létrehozott adatbázisok ezen a kiszolgálón) tooset tetszés szerint kiválaszthatja hello **beállításainak megtekintéséhez** hello adatbázis naplózási paneljén hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="e9d70-137">If you prefer tooset up a server auditing policy (which will apply tooall existing and newly created databases on this server), you can select hello **View server settings** link in hello database auditing blade.</span></span> <span data-ttu-id="e9d70-138">Ezt követően megtekintheti vagy hello kiszolgáló naplózási beállításainak módosítása.</span><span class="sxs-lookup"><span data-stu-id="e9d70-138">You can then view or modify hello server auditing settings.</span></span>

    ![Navigációs ablaktábla][2]
4. <span data-ttu-id="e9d70-140">Ha inkább tooenable blobnaplózási funkció hello adatbázis szintjén (hozzáadása tooor kiszolgálószintű naplózás helyett), a **naplózási**, jelölje be **ON**, és a **típus naplózás** , válassza ki **Blob**.</span><span class="sxs-lookup"><span data-stu-id="e9d70-140">If you prefer tooenable blob auditing on hello database level (in addition tooor instead of server-level auditing), for **Auditing**, select **ON**, and for **Auditing type**, select  **Blob**.</span></span>

    <span data-ttu-id="e9d70-141">Ha a kiszolgáló blobnaplózási funkció engedélyezve van, hello adatbázis konfigurált naplózási jelen hello server blob naplózási és.</span><span class="sxs-lookup"><span data-stu-id="e9d70-141">If server blob auditing is enabled, hello database-configured audit will exist side by side with hello server blob audit.</span></span>  

    ![Navigációs ablaktábla][3]
5. <span data-ttu-id="e9d70-143">tooopen hello **naplózási naplók tárolási** panelen válassza **tárolási részletek**.</span><span class="sxs-lookup"><span data-stu-id="e9d70-143">tooopen hello **Audit Logs Storage** blade, select **Storage Details**.</span></span> <span data-ttu-id="e9d70-144">Válassza ki a hello Azure storage-fiók ahol naplókat a rendszer menti, és válassza a hello megőrzési időtartam, mely hello után a régi naplókat törlődik.</span><span class="sxs-lookup"><span data-stu-id="e9d70-144">Select hello Azure storage account where logs will be saved, and then select hello retention period, after which hello old logs will be deleted.</span></span> <span data-ttu-id="e9d70-145">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="e9d70-145">Then click **OK**.</span></span> 
   >[!TIP] 
   ><span data-ttu-id="e9d70-146">tooget hello hatékonyabb működtetését hello naplózási jelentések sablonok, használja ugyanazt a tárfiókot, az összes naplózott adatbázis hello.</span><span class="sxs-lookup"><span data-stu-id="e9d70-146">tooget hello most out of hello auditing reports templates, use hello same storage account for all audited databases.</span></span> 

    <span data-ttu-id="e9d70-147"><a id="storage-screenshot"></a>![Navigációs ablaktábla][4]</span><span class="sxs-lookup"><span data-stu-id="e9d70-147"><a id="storage-screenshot"></a> ![Navigation pane][4]</span></span>
6. <span data-ttu-id="e9d70-148">Ha azt szeretné, hogy toocustomize hello naplózott események, ehhez a PowerShell segítségével, vagy hello REST API-t.</span><span class="sxs-lookup"><span data-stu-id="e9d70-148">If you want toocustomize hello audited events, you can do this via PowerShell or hello REST API.</span></span> <span data-ttu-id="e9d70-149">További részletekért lásd: hello [automatizálási (PowerShell/REST API-t)](#subheading-7) szakasz.</span><span class="sxs-lookup"><span data-stu-id="e9d70-149">For more details, see hello [Automation (PowerShell/REST API)](#subheading-7) section.</span></span>
7. <span data-ttu-id="e9d70-150">A naplózási beállítások konfigurálása után hello új threat detection szolgáltatás, és e-mailek tooreceive biztonsági riasztások konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="e9d70-150">After you've configured your auditing settings, you can turn on hello new threat detection feature and configure emails tooreceive security alerts.</span></span> <span data-ttu-id="e9d70-151">Fenyegetésészlelés használatakor kapni proaktív riasztások a adatbázist érintő rendellenes tevékenységeket, amely azt jelzi, hogy a esetleges biztonsági fenyegetéseket jelezhetnek.</span><span class="sxs-lookup"><span data-stu-id="e9d70-151">When you use threat detection, you receive proactive alerts on anomalous database activities that can indicate potential security threats.</span></span> <span data-ttu-id="e9d70-152">További részletekért lásd: [Ismerkedés a fenyegetésészlelés](sql-database-threat-detection-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="e9d70-152">For more details, see [Getting started with threat detection](sql-database-threat-detection-get-started.md).</span></span>
8. <span data-ttu-id="e9d70-153">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="e9d70-153">Click **Save**.</span></span>





## <span data-ttu-id="e9d70-154"><a id="subheading-3"></a>Elemezze a vizsgálati naplók és jelentések</span><span class="sxs-lookup"><span data-stu-id="e9d70-154"><a id="subheading-3"></a>Analyze audit logs and reports</span></span>
<span data-ttu-id="e9d70-155">Naplók az Azure storage-fiók a telepítés során választott hello összesítése.</span><span class="sxs-lookup"><span data-stu-id="e9d70-155">Audit logs are aggregated in hello Azure storage account you chose during setup.</span></span> <span data-ttu-id="e9d70-156">Eszköz használatával felfedezheti a vizsgálati naplók [Azure Tártallózó](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="e9d70-156">You can explore audit logs by using a tool such as [Azure Storage Explorer](http://storageexplorer.com/).</span></span>

<span data-ttu-id="e9d70-157">Menti a BLOB naplóinak blob fájlok belül nevű tárolót **sqldbauditlogs**.</span><span class="sxs-lookup"><span data-stu-id="e9d70-157">Blob auditing logs are saved as a collection of blob files within a container named **sqldbauditlogs**.</span></span>

<span data-ttu-id="e9d70-158">Hello blob naplózási naplók tároló mappát, blob elnevezési konvenciókat és időformátumát hello hierarchia további részleteiért lásd: hello [Blob naplózási napló fájlformátum referenciája (.docx fájl letöltése)](https://go.microsoft.com/fwlink/?linkid=829599).</span><span class="sxs-lookup"><span data-stu-id="e9d70-158">For further details about hello hierarchy of hello blob audit logs storage folder, blob naming conventions, and log format, see hello [Blob Audit Log Format Reference (.docx file download)](https://go.microsoft.com/fwlink/?linkid=829599).</span></span>

<span data-ttu-id="e9d70-159">Többféleképpen tooview blob naplófájlokat is használhatja:</span><span class="sxs-lookup"><span data-stu-id="e9d70-159">There are several methods you can use tooview blob auditing logs:</span></span>

* <span data-ttu-id="e9d70-160">Használjon hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e9d70-160">Use hello [Azure portal](https://portal.azure.com).</span></span>  <span data-ttu-id="e9d70-161">Nyissa meg hello vonatkozó adatbázis.</span><span class="sxs-lookup"><span data-stu-id="e9d70-161">Open hello relevant database.</span></span> <span data-ttu-id="e9d70-162">At hello felső hello adatbázis **naplózási & Threat detection** panelen kattintson a **nézet naplók**.</span><span class="sxs-lookup"><span data-stu-id="e9d70-162">At hello top of hello database's **Auditing & Threat detection** blade, click **View audit logs**.</span></span>

    ![Navigációs ablaktábla][7]

    <span data-ttu-id="e9d70-164">Egy **rekordok naplózása** panelen nyitja meg, amelyből be fog tudni tooview hello naplókat.</span><span class="sxs-lookup"><span data-stu-id="e9d70-164">An **Audit records** blade opens, from which you'll be able tooview hello logs.</span></span>

    - <span data-ttu-id="e9d70-165">Gombra kattintva megtekintheti az adott dátumok **szűrő** hello hello tetején **rekordok naplózása** panelen.</span><span class="sxs-lookup"><span data-stu-id="e9d70-165">You can view specific dates by clicking **Filter** at hello top of hello **Audit records** blade.</span></span>
    - <span data-ttu-id="e9d70-166">Naplózási azt jelzi, hogy a kiszolgáló házirend vagy az adatbázis házirend naplózási hozott létre válthat.</span><span class="sxs-lookup"><span data-stu-id="e9d70-166">You can switch between audit records that were created by a server policy or database policy audit.</span></span>

       ![Navigációs ablaktábla][8]

* <span data-ttu-id="e9d70-168">Hello rendszer funkcióval **sys.fn_get_audit_file** (T-SQL) tooreturn hello napló adatairól táblázatos formátumban.</span><span class="sxs-lookup"><span data-stu-id="e9d70-168">Use hello system function **sys.fn_get_audit_file** (T-SQL) tooreturn hello audit log data in tabular format.</span></span> <span data-ttu-id="e9d70-169">Ez a funkció használatáról további információkért lásd: hello [sys.fn_get_audit_file dokumentáció](https://docs.microsoft.com/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="e9d70-169">For more information on using this function, see hello [sys.fn_get_audit_file documentation](https://docs.microsoft.com/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql).</span></span>


* <span data-ttu-id="e9d70-170">Használjon **naplózási fájlok egyesítése** az SQL Server Management Studio (SSMS 17-től induló):</span><span class="sxs-lookup"><span data-stu-id="e9d70-170">Use **Merge Audit Files** in SQL Server Management Studio (starting with SSMS 17):</span></span>  
    1. <span data-ttu-id="e9d70-171">Hello SSMS menüben válassza ki a **fájl** > **nyitott** > **naplózási fájlok egyesítése**.</span><span class="sxs-lookup"><span data-stu-id="e9d70-171">From hello SSMS menu, select **File** > **Open** > **Merge Audit Files**.</span></span>

        ![Navigációs ablaktábla][9]
    2. <span data-ttu-id="e9d70-173">Hello **naplózási fájlok hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e9d70-173">hello **Add Audit Files** dialog box opens.</span></span> <span data-ttu-id="e9d70-174">Válasszon ki egy hello **Hozzáadás** beállítások kiválasztása, hogy toomerge naplózási fájlok helyi lemezre vagy importálja azokat az Azure Storage (akkor szükséges tooprovide lesz az Azure Storage részletek és a fiókkulcsot).</span><span class="sxs-lookup"><span data-stu-id="e9d70-174">Select one of hello **Add** options to choose whether toomerge audit files from a local disk or import them from Azure Storage (you will be required tooprovide your Azure Storage details and account key).</span></span>

    3. <span data-ttu-id="e9d70-175">Minden fájlok toomerge hozzáadása után kattintson **OK** toocomplete hello egyesítési művelet.</span><span class="sxs-lookup"><span data-stu-id="e9d70-175">After all files toomerge have been added, click **OK** toocomplete hello merge operation.</span></span>

    4. <span data-ttu-id="e9d70-176">hello egyesített fájlt az SSMS, ahol megtekintheti és elemezze, továbbá exportálási azt tooan xel-fájlt vagy a fürt megosztott kötetei szolgáltatás fájl vagy tooa tábla nyitja meg.</span><span class="sxs-lookup"><span data-stu-id="e9d70-176">hello merged file opens in SSMS, where you can view and analyze it, as well as export it tooan XEL or CSV file or tooa table.</span></span>

* <span data-ttu-id="e9d70-177">Használjon hello [alkalmazás szinkronizálása](https://github.com/Microsoft/Azure-SQL-DB-auditing-OMS-integration) , hogy létrehoztunk.</span><span class="sxs-lookup"><span data-stu-id="e9d70-177">Use hello [sync application](https://github.com/Microsoft/Azure-SQL-DB-auditing-OMS-integration) that we have created.</span></span> <span data-ttu-id="e9d70-178">Az Azure-ban fut, és használja az Operations Management Suite (OMS) szolgáltatáshoz nyilvános API-k toopush SQL naplók az OMS Szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="e9d70-178">It runs in Azure and utilizes Operations Management Suite (OMS) Log Analytics public APIs toopush SQL audit logs into OMS.</span></span> <span data-ttu-id="e9d70-179">alkalmazás hello szinkronizálása leküldéses értesítések SQL naplók OMS Log Analyticshez való felhasználásra hello OMS Naplóelemzési irányítópulton keresztül.</span><span class="sxs-lookup"><span data-stu-id="e9d70-179">hello sync application pushes SQL audit logs into OMS Log Analytics for consumption via hello OMS Log Analytics dashboard.</span></span> 

* <span data-ttu-id="e9d70-180">Használja a Power bi-ban.</span><span class="sxs-lookup"><span data-stu-id="e9d70-180">Use Power BI.</span></span> <span data-ttu-id="e9d70-181">Megtekintheti és elemezheti a napló adatairól a Power bi-ban.</span><span class="sxs-lookup"><span data-stu-id="e9d70-181">You can view and analyze audit log data in Power BI.</span></span> <span data-ttu-id="e9d70-182">További információ [Power bi-ban, és hozzáférést egy letölthető sablon](https://blogs.msdn.microsoft.com/azuresqldbsupport/2017/05/26/sql-azure-blob-auditing-basic-power-bi-dashboard/).</span><span class="sxs-lookup"><span data-stu-id="e9d70-182">Learn more about [Power BI, and access a downloadable template](https://blogs.msdn.microsoft.com/azuresqldbsupport/2017/05/26/sql-azure-blob-auditing-basic-power-bi-dashboard/).</span></span>

* <span data-ttu-id="e9d70-183">Naplófájlok letölthető az Azure Storage-blob tároló hello portálon vagy az eszköz használatával [Azure Tártallózó](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="e9d70-183">Download log files from your Azure Storage blob container via hello portal or by using a tool such as [Azure Storage Explorer](http://storageexplorer.com/).</span></span>
    * <span data-ttu-id="e9d70-184">Miután letöltötte a helyi naplófájlt, hello fájl tooopen, duplán kattintva megtekintheti, és a szolgáltatáshoz az ssms hello naplóinak elemzése.</span><span class="sxs-lookup"><span data-stu-id="e9d70-184">After you have downloaded a log file locally, you can double-click hello file tooopen, view, and analyze hello logs in SSMS.</span></span>
    * <span data-ttu-id="e9d70-185">Azure Tártallózó keresztül egyszerre több fájl is letölthető.</span><span class="sxs-lookup"><span data-stu-id="e9d70-185">You can also download multiple files simultaneously via Azure Storage Explorer.</span></span> <span data-ttu-id="e9d70-186">Kattintson a jobb gombbal egy adott almappát (például egy almappát, amely tartalmazza az összes naplófájl egy adott dátumhoz tartozó), és válassza ki **Mentés másként** toosave egy helyi mappába.</span><span class="sxs-lookup"><span data-stu-id="e9d70-186">Right-click a specific subfolder (for example, a subfolder that includes all log files for a specific date) and select **Save as** toosave in a local folder.</span></span>

* <span data-ttu-id="e9d70-187">További módszereket:</span><span class="sxs-lookup"><span data-stu-id="e9d70-187">Additional methods:</span></span>
   * <span data-ttu-id="e9d70-188">A letöltés több fájlt (vagy egy almappára tartalmaz naplófájlok teljes nap, a hello előző részben leírtak szerint) után egyesítheti helyileg hello SSMS egyesítési naplófájlokat utasításokat lásd a korábbiakban leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="e9d70-188">After downloading several files (or a subfolder that includes log files for an entire day, as described in hello previous item in this list), you can merge them locally as described in hello SSMS Merge Audit Files instructions described earlier.</span></span>

   * <span data-ttu-id="e9d70-189">Nézet blobnaplózási funkció szoftveres naplói:</span><span class="sxs-lookup"><span data-stu-id="e9d70-189">View blob auditing logs programmatically:</span></span>

     * <span data-ttu-id="e9d70-190">Használjon hello [kiterjesztett események olvasó](https://blogs.msdn.microsoft.com/extended_events/2011/07/20/introducing-the-extended-events-reader/) C# könyvtár.</span><span class="sxs-lookup"><span data-stu-id="e9d70-190">Use hello [Extended Events Reader](https://blogs.msdn.microsoft.com/extended_events/2011/07/20/introducing-the-extended-events-reader/) C# library.</span></span>
     * <span data-ttu-id="e9d70-191">[Kiterjesztett események fájljainak lekérdezése](https://sqlscope.wordpress.com/2014/11/15/reading-extended-event-files-using-client-side-tools-only/) PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="e9d70-191">[Query Extended Events Files](https://sqlscope.wordpress.com/2014/11/15/reading-extended-event-files-using-client-side-tools-only/) by using PowerShell.</span></span>




## <span data-ttu-id="e9d70-192"><a id="subheading-5"></a>Éles eljárások</span><span class="sxs-lookup"><span data-stu-id="e9d70-192"><a id="subheading-5"></a>Production practices</span></span>
<!--hello description in this section refers toopreceding screen captures.-->

### <span data-ttu-id="e9d70-193"><a id="subheading-6">Georeplikált adatbázisok naplózás</a></span><span class="sxs-lookup"><span data-stu-id="e9d70-193"><a id="subheading-6">Auditing geo-replicated databases</a></span></span>
<span data-ttu-id="e9d70-194">Georeplikált adatbázist kell használni, amikor nem lehetséges tooset naplózási hello elsődleges adatbázisra, hello másodlagos adatbázis, vagy mindkettőt hello naplózási típusától függően.</span><span class="sxs-lookup"><span data-stu-id="e9d70-194">When you use geo-replicated databases, it is possible tooset up auditing on either hello primary database, hello secondary database, or both, depending on hello audit type.</span></span>

<span data-ttu-id="e9d70-195">A következő lépések követésével (ne feledje, hogy blobnaplózási funkció kapcsolható be- és kikapcsolása csak a naplózási beállítások hello elsődleges adatbázis):</span><span class="sxs-lookup"><span data-stu-id="e9d70-195">Follow these instructions (remember that blob auditing can be turned on or off only from hello primary database auditing settings):</span></span>

* <span data-ttu-id="e9d70-196">**Elsődleges adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="e9d70-196">**Primary database**.</span></span> <span data-ttu-id="e9d70-197">Kapcsolja be a blobnaplózási funkció, hello kiszolgálón vagy hello adatbázison, lásd: hello [az adatbázis naplózásának beállítása](#subheading-2) szakasz.</span><span class="sxs-lookup"><span data-stu-id="e9d70-197">Turn on blob auditing, either on hello server or on hello database itself, as described in hello [Set up auditing for your database](#subheading-2) section.</span></span>
* <span data-ttu-id="e9d70-198">**Másodlagos adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="e9d70-198">**Secondary database**.</span></span> <span data-ttu-id="e9d70-199">Kapcsolja be a blobnaplózási funkció hello elsődleges adatbázis, a hello [az adatbázis naplózásának beállítása](#subheading-2) szakasz.</span><span class="sxs-lookup"><span data-stu-id="e9d70-199">Turn on blob auditing on hello primary database, as described in hello [Set up auditing for your database](#subheading-2) section.</span></span> 
   * <span data-ttu-id="e9d70-200">BLOB naplózását engedélyezni kell a hello *maga elsődleges adatbázis*, nem hello kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="e9d70-200">Blob auditing must be enabled on hello *primary database itself*, not hello server.</span></span>
   * <span data-ttu-id="e9d70-201">Blob naplózás engedélyezése után hello elsődleges adatbázison, akkor is válik elérhetővé, hello másodlagos adatbázison.</span><span class="sxs-lookup"><span data-stu-id="e9d70-201">After blob auditing is enabled on hello primary database, it will also become enabled on hello secondary database.</span></span>

     >[!IMPORTANT]
     ><span data-ttu-id="e9d70-202">Hello hello másodlagos adatbázis-tárolási beállításai a kereszt-területi forgalmat, amely alapértelmezés szerint lesz azonos toothose hello elsődleges adatbázis.</span><span class="sxs-lookup"><span data-stu-id="e9d70-202">By default, hello storage settings for hello secondary database will be identical toothose of hello primary database, causing cross-regional traffic.</span></span> <span data-ttu-id="e9d70-203">Ez elkerülheti a blobnaplózási funkció hello másodlagos kiszolgálón, és a helyi tároló konfigurálása a hello másodlagos kiszolgáló tárolási beállítások engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="e9d70-203">You can avoid this by enabling blob auditing on hello secondary server and configuring local storage in hello secondary server storage settings.</span></span> <span data-ttu-id="e9d70-204">Ez a művelet felülírja a hello tárolási helyét hello másodlagos adatbázis és az egyes adatbázisok naplózási naplók toolocal tárolóval mentése eredményez.</span><span class="sxs-lookup"><span data-stu-id="e9d70-204">This will override hello storage location for hello secondary database and result in each database saving its audit logs toolocal storage.</span></span>  
<br>

### <span data-ttu-id="e9d70-205"><a id="subheading-6">Tárolási kulcs újragenerálása</a></span><span class="sxs-lookup"><span data-stu-id="e9d70-205"><a id="subheading-6">Storage key regeneration</a></span></span>
<span data-ttu-id="e9d70-206">Éles valószínűleg a tárolási a kulcsok rendszeresen toorefresh áll.</span><span class="sxs-lookup"><span data-stu-id="e9d70-206">In production, you are likely toorefresh your storage keys periodically.</span></span> <span data-ttu-id="e9d70-207">A kulcsok frissítése, úgy kell tooresave hello naplózási házirend.</span><span class="sxs-lookup"><span data-stu-id="e9d70-207">When refreshing your keys, you need tooresave hello auditing policy.</span></span> <span data-ttu-id="e9d70-208">hello folyamat a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="e9d70-208">hello process is as follows:</span></span>

1. <span data-ttu-id="e9d70-209">Nyissa meg hello **tárolási részletek** panelen.</span><span class="sxs-lookup"><span data-stu-id="e9d70-209">Open hello **Storage Details** blade.</span></span> <span data-ttu-id="e9d70-210">A hello **Tárelérési kulcs** mezőben válassza **másodlagos**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="e9d70-210">In hello **Storage Access Key** box, select **Secondary**, and click **OK**.</span></span> <span data-ttu-id="e9d70-211">Kattintson a **mentése** naplózás konfigurációs panel hello hello tetején.</span><span class="sxs-lookup"><span data-stu-id="e9d70-211">Then click **Save** at hello top of hello auditing configuration blade.</span></span>

    ![Navigációs ablaktábla][5]
2. <span data-ttu-id="e9d70-213">Nyissa meg toohello tárolási konfiguráció panelt, és hello elsődleges elérési kulcs újragenerálása.</span><span class="sxs-lookup"><span data-stu-id="e9d70-213">Go toohello storage configuration blade and regenerate hello primary access key.</span></span>

    ![Navigációs ablaktábla][6]
3. <span data-ttu-id="e9d70-215">Lépjen vissza a naplózási konfiguráció panel toohello, hello tárelérési kulcs másodlagos tooprimary a kapcsolóhoz, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="e9d70-215">Go back toohello auditing configuration blade, switch hello storage access key from secondary tooprimary, and then click **OK**.</span></span> <span data-ttu-id="e9d70-216">Kattintson a **mentése** naplózás konfigurációs panel hello hello tetején.</span><span class="sxs-lookup"><span data-stu-id="e9d70-216">Then click **Save** at hello top of hello auditing configuration blade.</span></span>
4. <span data-ttu-id="e9d70-217">Lépjen vissza a toohello tárolási konfiguráció panel megnyitásához, és az újbóli létrehozás hello másodlagos elérési kulcsát (a frissítés során hello tovább kulcs előkészítése).</span><span class="sxs-lookup"><span data-stu-id="e9d70-217">Go back toohello storage configuration blade and regenerate hello secondary access key (in preparation for hello next key's refresh cycle).</span></span>

## <span data-ttu-id="e9d70-218"><a id="subheading-7"></a>Automatizálási (PowerShell/REST API)</span><span class="sxs-lookup"><span data-stu-id="e9d70-218"><a id="subheading-7"></a>Automation (PowerShell/REST API)</span></span>
<span data-ttu-id="e9d70-219">A következő automation eszközök hello segítségével az Azure SQL Database naplózási is konfigurálhatja:</span><span class="sxs-lookup"><span data-stu-id="e9d70-219">You can also configure auditing in Azure SQL Database by using hello following automation tools:</span></span>

* <span data-ttu-id="e9d70-220">**PowerShell-parancsmagok**:</span><span class="sxs-lookup"><span data-stu-id="e9d70-220">**PowerShell cmdlets**:</span></span>

   * <span data-ttu-id="e9d70-221">[Get-AzureRMSqlDatabaseAuditingPolicy][101]</span><span class="sxs-lookup"><span data-stu-id="e9d70-221">[Get-AzureRMSqlDatabaseAuditingPolicy][101]</span></span>
   * <span data-ttu-id="e9d70-222">[Get-AzureRMSqlServerAuditingPolicy][102]</span><span class="sxs-lookup"><span data-stu-id="e9d70-222">[Get-AzureRMSqlServerAuditingPolicy][102]</span></span>
   * <span data-ttu-id="e9d70-223">[Remove-AzureRMSqlDatabaseAuditing][103]</span><span class="sxs-lookup"><span data-stu-id="e9d70-223">[Remove-AzureRMSqlDatabaseAuditing][103]</span></span>
   * <span data-ttu-id="e9d70-224">[Remove-AzureRMSqlServerAuditing][104]</span><span class="sxs-lookup"><span data-stu-id="e9d70-224">[Remove-AzureRMSqlServerAuditing][104]</span></span>
   * <span data-ttu-id="e9d70-225">[Set-AzureRMSqlDatabaseAuditingPolicy][105]</span><span class="sxs-lookup"><span data-stu-id="e9d70-225">[Set-AzureRMSqlDatabaseAuditingPolicy][105]</span></span>
   * <span data-ttu-id="e9d70-226">[Set-AzureRMSqlServerAuditingPolicy][106]</span><span class="sxs-lookup"><span data-stu-id="e9d70-226">[Set-AzureRMSqlServerAuditingPolicy][106]</span></span>
   * <span data-ttu-id="e9d70-227">[Használjon-AzureRMSqlServerAuditingPolicy][107]</span><span class="sxs-lookup"><span data-stu-id="e9d70-227">[Use-AzureRMSqlServerAuditingPolicy][107]</span></span>

   <span data-ttu-id="e9d70-228">Tekintse meg a parancsfájl például [konfigurálhatja a naplózás és a fenyegetések észlelésére, a PowerShell használatával](scripts/sql-database-auditing-and-threat-detection-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="e9d70-228">For a script example, see [Configure auditing and threat detection using PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).</span></span>

* <span data-ttu-id="e9d70-229">**REST API - Blobnaplózási funkció**:</span><span class="sxs-lookup"><span data-stu-id="e9d70-229">**REST API - Blob auditing**:</span></span>

   * [<span data-ttu-id="e9d70-230">Hozzon létre vagy frissítés adatbázis Blob naplózási házirend</span><span class="sxs-lookup"><span data-stu-id="e9d70-230">Create or Update Database Blob Auditing Policy</span></span>](https://msdn.microsoft.com/library/azure/mt695939.aspx)
   * [<span data-ttu-id="e9d70-231">Hozzon létre vagy frissítési kiszolgáló Blob naplózási házirend</span><span class="sxs-lookup"><span data-stu-id="e9d70-231">Create or Update Server Blob Auditing Policy</span></span>](https://msdn.microsoft.com/library/azure/mt771861.aspx)
   * [<span data-ttu-id="e9d70-232">Adatbázis-Blob naplórendet beolvasása</span><span class="sxs-lookup"><span data-stu-id="e9d70-232">Get Database Blob Auditing Policy</span></span>](https://msdn.microsoft.com/library/azure/mt695938.aspx)
   * [<span data-ttu-id="e9d70-233">Kiszolgáló Blob naplórendet beolvasása</span><span class="sxs-lookup"><span data-stu-id="e9d70-233">Get Server Blob Auditing Policy</span></span>](https://msdn.microsoft.com/library/azure/mt771860.aspx)
   * [<span data-ttu-id="e9d70-234">Műveleti eredmény naplózás Server Blob beolvasása</span><span class="sxs-lookup"><span data-stu-id="e9d70-234">Get Server Blob Auditing Operation Result</span></span>](https://msdn.microsoft.com/library/azure/mt771862.aspx)


<!--Anchors-->
[Azure SQL Database Auditing overview]: #subheading-1
[Set up auditing for your database]: #subheading-2
[Analyze audit logs and reports]: #subheading-3
[Practices for usage in production]: #subheading-5
[Storage Key Regeneration]: #subheading-6
[Automation (PowerShell / REST API)]: #subheading-7
[Blob/Table differences in Server auditing policy inheritance]: (#subheading-8)  

<!--Image references-->
[1]: ./media/sql-database-auditing-get-started/1_auditing_get_started_settings.png
[2]: ./media/sql-database-auditing-get-started/2_auditing_get_started_server_inherit.png
[3]: ./media/sql-database-auditing-get-started/3_auditing_get_started_turn_on.png
[4]: ./media/sql-database-auditing-get-started/4_auditing_get_started_storage_details.png
[5]: ./media/sql-database-auditing-get-started/5_auditing_get_started_storage_key_regeneration.png
[6]: ./media/sql-database-auditing-get-started/6_auditing_get_started_regenerate_key.png
[7]: ./media/sql-database-auditing-get-started/7_auditing_get_started_blob_view_audit_logs.png
[8]: ./media/sql-database-auditing-get-started/8_auditing_get_started_blob_audit_records.png
[9]: ./media/sql-database-auditing-get-started/9_auditing_get_started_ssms_1.png
[10]: ./media/sql-database-auditing-get-started/10_auditing_get_started_ssms_2.png

[101]: /powershell/module/azurerm.sql/get-azurermsqldatabaseauditingpolicy
[102]: /powershell/module/azurerm.sql/Get-AzureRMSqlServerAuditingPolicy
[103]: /powershell/module/azurerm.sql/Remove-AzureRMSqlDatabaseAuditing
[104]: /powershell/module/azurerm.sql/Remove-AzureRMSqlServerAuditing
[105]: /powershell/module/azurerm.sql/Set-AzureRMSqlDatabaseAuditingPolicy
[106]: /powershell/module/azurerm.sql/Set-AzureRMSqlServerAuditingPolicy
[107]: /powershell/module/azurerm.sql/Use-AzureRMSqlServerAuditingPolicy
