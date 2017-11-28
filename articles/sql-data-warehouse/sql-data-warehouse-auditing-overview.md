---
title: "Az Azure SQL Data Warehouse naplózása |} Microsoft Docs"
description: "Ismerkedés az Azure SQL Data Warehouse naplózás"
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: 0e6af148-b218-4b43-bb5f-907917d20330
ms.service: sql-data-warehouse
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: security
ms.date: 08/21/2017
ms.author: rortloff;barbkess
ms.openlocfilehash: f851c82ebeaa647f663d499a4d327c3479e36121
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="auditing-in-azure-sql-data-warehouse"></a><span data-ttu-id="7b9d8-103">Az Azure SQL Data Warehouse naplózás</span><span class="sxs-lookup"><span data-stu-id="7b9d8-103">Auditing in Azure SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7b9d8-104">Naplózás</span><span class="sxs-lookup"><span data-stu-id="7b9d8-104">Auditing</span></span>](sql-data-warehouse-auditing-overview.md)
> * [<span data-ttu-id="7b9d8-105">Fenyegetések észlelése</span><span class="sxs-lookup"><span data-stu-id="7b9d8-105">Threat detection</span></span>](sql-data-warehouse-security-threat-detection.md)
> 
> 

<span data-ttu-id="7b9d8-106">Az SQL Data Warehouse naplózás segítségével az adatbázist a naplózási események jelentkezzen be az Azure Storage-fiók bejegyzéshez.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-106">SQL Data Warehouse auditing allows you to record events in your database to an audit log in your Azure Storage account.</span></span> <span data-ttu-id="7b9d8-107">Naplózás segít törvényi megfelelőség fenntartásában, ismerje meg adatbázis-tevékenység, és azok az eltérések és rendellenességek, amelyek üzleti problémát jelenthetnek, vagy a biztonság megsértésére betekintést.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-107">Auditing can help you maintain regulatory compliance, understand  database activity, and gain insight into discrepancies and anomalies that could indicate business concerns or suspected security violations.</span></span> <span data-ttu-id="7b9d8-108">Az SQL Data Warehouse naplózását is integrálható a Microsoft Power BI leásási jelentéskészítés és elemzés céljára.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-108">SQL Data Warehouse auditing also integrates with Microsoft Power BI for drill-down reporting and analysis.</span></span>

<span data-ttu-id="7b9d8-109">A naplózási eszközök engedélyezése, és lehetővé teszi a megfelelést a megfelelőségi követelményeket, de nem garantálják megfelelőségi.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-109">Auditing tools enable and facilitate adherence to compliance standards but don't guarantee compliance.</span></span> <span data-ttu-id="7b9d8-110">További információ az Azure programokat, hogy támogatási szabványoknak való megfelelés, a következő témakörben: a <a href="http://azure.microsoft.com/support/trust-center/compliance/" target="_blank">Azure biztonsági és adatkezelési központ</a>.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-110">For more information about Azure programs that support standards compliance, see the <a href="http://azure.microsoft.com/support/trust-center/compliance/" target="_blank">Azure Trust Center</a>.</span></span>

* <span data-ttu-id="7b9d8-111">[Adatbázis naplózási alapjai]</span><span class="sxs-lookup"><span data-stu-id="7b9d8-111">[Database Auditing basics]</span></span>
* <span data-ttu-id="7b9d8-112">[Az adatbázis naplózásának beállítása]</span><span class="sxs-lookup"><span data-stu-id="7b9d8-112">[Set up auditing for your database]</span></span>
* <span data-ttu-id="7b9d8-113">[Elemezze a vizsgálati naplók és jelentések]</span><span class="sxs-lookup"><span data-stu-id="7b9d8-113">[Analyze audit logs and reports]</span></span>

## <span data-ttu-id="7b9d8-114"><a id="subheading-1"></a>Az Azure SQL Data Warehouse Database Auditing alapjai</span><span class="sxs-lookup"><span data-stu-id="7b9d8-114"><a id="subheading-1"></a>Azure SQL Data Warehouse Database Auditing basics</span></span>
<span data-ttu-id="7b9d8-115">Az SQL Data Warehouse-adatbázis naplózásának teszi lehetővé:</span><span class="sxs-lookup"><span data-stu-id="7b9d8-115">SQL Data Warehouse database auditing allows you to:</span></span>

* <span data-ttu-id="7b9d8-116">**Tartsa meg** kijelölt események egy naplóban.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-116">**Retain** an audit trail of selected events.</span></span> <span data-ttu-id="7b9d8-117">Adatbázis-műveleteket kell naplózni kategóriáinak adhat meg.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-117">You can define categories of database actions  to be audited.</span></span>
* <span data-ttu-id="7b9d8-118">**A jelentés** adatbázis-tevékenység.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-118">**Report** on database activity.</span></span> <span data-ttu-id="7b9d8-119">Előre konfigurált jelentések és egy irányítópult segítségével gyorsan Ismerkedés a tevékenységet és az események naplózásához.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-119">You can use preconfigured reports and a dashboard to get started quickly with activity and event reporting.</span></span>
* <span data-ttu-id="7b9d8-120">**Elemezze** jelentéseket.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-120">**Analyze** reports.</span></span> <span data-ttu-id="7b9d8-121">Gyanús események, a szokatlan tevékenység és a trendeket találja.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-121">You can find suspicious events, unusual activity, and trends.</span></span>

<span data-ttu-id="7b9d8-122">Konfigurálhatja a naplózás a következő kategóriák:</span><span class="sxs-lookup"><span data-stu-id="7b9d8-122">You can configure auditing for the following event categories:</span></span>

<span data-ttu-id="7b9d8-123">**Egyszerű SQL** és **paraméteres SQL** , amelynek a gyűjtött naplók besorolt</span><span class="sxs-lookup"><span data-stu-id="7b9d8-123">**Plain SQL** and **Parameterized SQL** for which the collected audit logs are classified as</span></span>  

* <span data-ttu-id="7b9d8-124">**Adatokhoz való hozzáférés**</span><span class="sxs-lookup"><span data-stu-id="7b9d8-124">**Access to data**</span></span>
* <span data-ttu-id="7b9d8-125">**Sémaváltozások (DDL)**</span><span class="sxs-lookup"><span data-stu-id="7b9d8-125">**Schema changes (DDL)**</span></span>
* <span data-ttu-id="7b9d8-126">**Adatok módosításait (DML)**</span><span class="sxs-lookup"><span data-stu-id="7b9d8-126">**Data changes (DML)**</span></span>
* <span data-ttu-id="7b9d8-127">**Fiókok, szerepkörök és engedélyeket (DCL)**</span><span class="sxs-lookup"><span data-stu-id="7b9d8-127">**Accounts, roles, and permissions (DCL)**</span></span>
* <span data-ttu-id="7b9d8-128">**Tárolt eljárás**, **bejelentkezési** , és **tranzakció felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-128">**Stored Procedure**, **Login** and, **Transaction Management**.</span></span>

<span data-ttu-id="7b9d8-129">Minden esemény kategóriához a naplózási **sikeres** és **hiba** műveleteket külön-külön vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-129">For each Event Category, Auditing of **Success** and **Failure** operations are configured separately.</span></span>

<span data-ttu-id="7b9d8-130">A tevékenységek és a naplózott események kapcsolatos további információkért tekintse meg a <a href="http://go.microsoft.com/fwlink/?LinkId=506733" target="_blank">naplózási napló fájlformátum referenciája (doc fájl letöltése)</a>.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-130">For more information about the activities and events audited, see the <a href="http://go.microsoft.com/fwlink/?LinkId=506733" target="_blank">Audit Log Format Reference (doc file download)</a>.</span></span>

<span data-ttu-id="7b9d8-131">Naplók az Azure storage-fiókok vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-131">Audit logs are stored in your Azure storage account.</span></span> <span data-ttu-id="7b9d8-132">Megadhatja, hogy az ellenőrzési napló megőrzési időtartam.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-132">You can define an audit log retention period.</span></span>

<span data-ttu-id="7b9d8-133">A naplózási házirend meghatározása egy adott adatbázis vagy az alapértelmezett házirend-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-133">An auditing policy can be defined for a specific database or as a default server policy.</span></span> <span data-ttu-id="7b9d8-134">Kiszolgáló alapértelmezett naplózási házirend vonatkozik egy kiszolgálón lévő összes adatbázis, amely nem rendelkezik egy adott felülbíráló adatbázis naplózási házirend lett meghatározva.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-134">A default server auditing policy applies to all databases on a server, which do not have a specific overriding database auditing policy defined.</span></span>

<span data-ttu-id="7b9d8-135">Jelölőnégyzet naplózás használata naplózási beállítása előtt egy ["Régebbi típusú ügyfelekhez."](sql-data-warehouse-auditing-downlevel-clients.md)</span><span class="sxs-lookup"><span data-stu-id="7b9d8-135">Before setting up audit auditing check if you are using a ["Downlevel Client."](sql-data-warehouse-auditing-downlevel-clients.md)</span></span>

## <span data-ttu-id="7b9d8-136"><a id="subheading-2"></a>Az adatbázis naplózásának beállítása</span><span class="sxs-lookup"><span data-stu-id="7b9d8-136"><a id="subheading-2"></a>Set up auditing for your database</span></span>
1. <span data-ttu-id="7b9d8-137">Indítsa el a <a href="https://portal.azure.com" target="_blank">Azure-portálon</a>.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-137">Launch the <a href="https://portal.azure.com" target="_blank">Azure portal</a>.</span></span>
2. <span data-ttu-id="7b9d8-138">Lépjen a **beállítások** a naplózni kívánt SQL Data warehouse panelre.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-138">Go to the **Settings** blade of the SQL Data Warehouse you want to audit.</span></span> <span data-ttu-id="7b9d8-139">Az a **beállítások** panelen válassza **naplózási & Threat detection**.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-139">In the **Settings** blade, select **Auditing & Threat detection**.</span></span>
   
    ![][1]
3. <span data-ttu-id="7b9d8-140">Következő lépésként engedélyezze a nyomkövetést oly módon, kattintson a **ON** gombra.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-140">Next, enable auditing by clicking the **ON** button.</span></span>
   
    ![][3]
4. <span data-ttu-id="7b9d8-141">A naplózási konfiguráció paneljén válassza **tárolási részletek** naplózási naplók tárolási panel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-141">In the auditing configuration blade, select **STORAGE DETAILS** to open the Audit Logs Storage blade.</span></span> <span data-ttu-id="7b9d8-142">Válassza ki az Azure storage-fiók naplók menteni és, a megőrzési időn.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-142">Select the Azure storage account where logs will be saved and, the retention period.</span></span> 
>[!TIP]
><span data-ttu-id="7b9d8-143">Ugyanazt a tárfiókot az összes naplózott adatbázis segítségével a legtöbbet hozhatja ki az előre konfigurált jelentések sablonokat.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-143">Use the same storage account for all audited databases to get the most out of the pre-configured reports templates.</span></span>
   
    ![][4]
5. <span data-ttu-id="7b9d8-144">Kattintson a **OK** gombra kattintva mentse a tárkonfiguráció részleteit.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-144">Click the **OK** button to save the storage details configuration.</span></span>
6. <span data-ttu-id="7b9d8-145">A **-NAPLÓZÁS által esemény**, kattintson a **sikeres** és **hiba** lehetőséget az összes esemény naplózása, vagy az egyes kategóriák.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-145">Under **LOGGING BY EVENT**, click **SUCCESS** and **FAILURE** to log all events, or choose individual event categories.</span></span>
7. <span data-ttu-id="7b9d8-146">Naplózási adatbázis beállításakor, szükség lehet megváltoztatni a kapcsolati karakterlánc az ügyfél annak érdekében, hogy megfelelően rögzített adatok naplózását.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-146">If you are configuring Auditing for a database, you may need to alter the connection string of your client to ensure data auditing is correctly captured.</span></span> <span data-ttu-id="7b9d8-147">Ellenőrizze a [módosíthatja a kiszolgáló teljes Tartománynevet a kapcsolódási karakterláncban](sql-data-warehouse-auditing-downlevel-clients.md) témakör korábbi verziójú ügyfél-kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-147">Check the [Modify Server FDQN in the connection string](sql-data-warehouse-auditing-downlevel-clients.md) topic for downlevel client connections.</span></span>
8. <span data-ttu-id="7b9d8-148">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-148">Click **OK**.</span></span>

## <span data-ttu-id="7b9d8-149"><a id="subheading-3"></a>Elemezze a vizsgálati naplók és jelentések</span><span class="sxs-lookup"><span data-stu-id="7b9d8-149"><a id="subheading-3"></a>Analyze audit logs and reports</span></span>
<span data-ttu-id="7b9d8-150">Naplófájlok tárolási táblákon, amelyekhez a gyűjtemény összesítése egy **SQLDBAuditLogs** a telepítéskor választott Azure-tárfiók-előtagot.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-150">Audit logs are aggregated in a collection of Store Tables with a **SQLDBAuditLogs** prefix in the Azure storage account you chose during setup.</span></span> <span data-ttu-id="7b9d8-151">Megtekintheti a naplófájlok egy eszközzel, mint <a href="http://azurestorageexplorer.codeplex.com/" target="_blank">Azure Tártallózó</a>.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-151">You can view log files using a tool such as <a href="http://azurestorageexplorer.codeplex.com/" target="_blank">Azure Storage Explorer</a>.</span></span>

<span data-ttu-id="7b9d8-152">Előre konfigurált irányítópult jelentéssablon áll rendelkezésre, mert egy <a href="http://go.microsoft.com/fwlink/?LinkId=403540" target="_blank">letölthető Excel-táblázat</a> naplóadatokat gyorsan elemzéséhez.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-152">A preconfigured dashboard report template is available as a <a href="http://go.microsoft.com/fwlink/?LinkId=403540" target="_blank">downloadable Excel spreadsheet</a> to help you quickly analyze log data.</span></span> <span data-ttu-id="7b9d8-153">A naplók a sablon használatához kell Excel 2013 vagy újabb verzió és a Power Query, amely letölthető <a href="http://www.microsoft.com/download/details.aspx?id=39379">Itt</a>.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-153">To use the template on your audit logs, you need Excel 2013 or later and Power Query, which you can download <a href="http://www.microsoft.com/download/details.aspx?id=39379">here</a>.</span></span>

<span data-ttu-id="7b9d8-154">A sablon a képzeletbeli mintaadatok rendelkezik, és a napló közvetlenül importálása az Azure-tárfiókot Power Query állíthat be.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-154">The template has fictional sample data in it, and you can set up Power Query to import your audit log directly from your Azure storage account.</span></span>

## <span data-ttu-id="7b9d8-155"><a id="subheading-4"></a>Tárolási kulcs újragenerálása</span><span class="sxs-lookup"><span data-stu-id="7b9d8-155"><a id="subheading-4"></a>Storage key regeneration</span></span>
<span data-ttu-id="7b9d8-156">Éles akkor valószínűleg a kulcsok rendszeresen frissíteni.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-156">In production, you are likely to refresh your storage keys periodically.</span></span> <span data-ttu-id="7b9d8-157">Ha a kulcsok frissítése, kell menteni a házirendet.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-157">When refreshing your keys, you need to save the policy.</span></span> <span data-ttu-id="7b9d8-158">A folyamat a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="7b9d8-158">The process is as follows:</span></span>

1. <span data-ttu-id="7b9d8-159">A naplózási konfiguráció panelen (a szakasz naplózásának beállítása fent leírt) kapcsoló a **Tárelérési kulcs** a *elsődleges* való *másodlagos* és **mentése**.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-159">In the auditing configuration blade (described above in the setup auditing section) switch the **Storage Access Key** from *Primary* to *Secondary* and **SAVE**.</span></span>

   ![][4]
2. <span data-ttu-id="7b9d8-160">Nyissa meg a tárolási konfiguráció paneljét és **újragenerálja** a *elsődleges elérési kulcsot*.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-160">Go to the storage configuration blade and **regenerate** the *Primary Access Key*.</span></span>
3. <span data-ttu-id="7b9d8-161">Lépjen vissza a naplózási konfiguráció blade, kapcsoló a **Tárelérési kulcs** a *másodlagos* való *elsődleges* nyomja le az ENTER **mentése**.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-161">Go back to the auditing configuration blade, switch the **Storage Access Key** from *Secondary* to *Primary* and press **SAVE**.</span></span>
4. <span data-ttu-id="7b9d8-162">Lépjen vissza a felhasználói felület tárolására és **újragenerálja** a *másodlagos elérési kulcsot* (mint a következő előkészítése kulcsok a frissítés.</span><span class="sxs-lookup"><span data-stu-id="7b9d8-162">Go back to the storage UI and **regenerate** the *Secondary Access Key* (as preparation for the next keys refresh cycle.</span></span>

## <span data-ttu-id="7b9d8-163"><a id="subheading-5"></a>Automatizálási (PowerShell/REST API)</span><span class="sxs-lookup"><span data-stu-id="7b9d8-163"><a id="subheading-5"></a>Automation (PowerShell/REST API)</span></span>
<span data-ttu-id="7b9d8-164">A következő automation eszközök használatával az Azure SQL Data Warehouse naplózását is konfigurálhatja:</span><span class="sxs-lookup"><span data-stu-id="7b9d8-164">You can also configure auditing in Azure SQL Data Warehouse by using the following automation tools:</span></span>

* <span data-ttu-id="7b9d8-165">**PowerShell-parancsmagok**:</span><span class="sxs-lookup"><span data-stu-id="7b9d8-165">**PowerShell cmdlets**:</span></span>

   * <span data-ttu-id="7b9d8-166">[Get-AzureRMSqlDatabaseAuditingPolicy][101]</span><span class="sxs-lookup"><span data-stu-id="7b9d8-166">[Get-AzureRMSqlDatabaseAuditingPolicy][101]</span></span>
   * <span data-ttu-id="7b9d8-167">[Get-AzureRMSqlServerAuditingPolicy][102]</span><span class="sxs-lookup"><span data-stu-id="7b9d8-167">[Get-AzureRMSqlServerAuditingPolicy][102]</span></span>
   * <span data-ttu-id="7b9d8-168">[Remove-AzureRMSqlDatabaseAuditing][103]</span><span class="sxs-lookup"><span data-stu-id="7b9d8-168">[Remove-AzureRMSqlDatabaseAuditing][103]</span></span>
   * <span data-ttu-id="7b9d8-169">[Remove-AzureRMSqlServerAuditing][104]</span><span class="sxs-lookup"><span data-stu-id="7b9d8-169">[Remove-AzureRMSqlServerAuditing][104]</span></span>
   * <span data-ttu-id="7b9d8-170">[Set-AzureRMSqlDatabaseAuditingPolicy][105]</span><span class="sxs-lookup"><span data-stu-id="7b9d8-170">[Set-AzureRMSqlDatabaseAuditingPolicy][105]</span></span>
   * <span data-ttu-id="7b9d8-171">[Set-AzureRMSqlServerAuditingPolicy][106]</span><span class="sxs-lookup"><span data-stu-id="7b9d8-171">[Set-AzureRMSqlServerAuditingPolicy][106]</span></span>
   * <span data-ttu-id="7b9d8-172">[Használjon-AzureRMSqlServerAuditingPolicy][107]</span><span class="sxs-lookup"><span data-stu-id="7b9d8-172">[Use-AzureRMSqlServerAuditingPolicy][107]</span></span>

<!--Anchors-->
[Adatbázis naplózási alapjai]: #subheading-1
[Az adatbázis naplózásának beállítása]: #subheading-2
[Elemezze a vizsgálati naplók és jelentések]: #subheading-3


<!--Image references-->
[1]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing.png
[2]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-inherit.png
[3]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-enable.png
[4]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-storage-account.png
[5]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-dashboard.png


<!--Link references-->
[101]: /powershell/module/azurerm.sql/get-azurermsqldatabaseauditingpolicy
[102]: /powershell/module/azurerm.sql/Get-AzureRMSqlServerAuditingPolicy
[103]: /powershell/module/azurerm.sql/Remove-AzureRMSqlDatabaseAuditing
[104]: /powershell/module/azurerm.sql/Remove-AzureRMSqlServerAuditing
[105]: /powershell/module/azurerm.sql/Set-AzureRMSqlDatabaseAuditingPolicy
[106]: /powershell/module/azurerm.sql/Set-AzureRMSqlServerAuditingPolicy
[107]: /powershell/module/azurerm.sql/Use-AzureRMSqlServerAuditingPolicy