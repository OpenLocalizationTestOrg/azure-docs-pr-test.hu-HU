---
title: az Azure SQL Data Warehouse aaaAuditing |} Microsoft Docs
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
ms.openlocfilehash: 948de74fa052ef206cf1aa65c0d81f084b18cb00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="auditing-in-azure-sql-data-warehouse"></a><span data-ttu-id="d94f8-103">Az Azure SQL Data Warehouse naplózás</span><span class="sxs-lookup"><span data-stu-id="d94f8-103">Auditing in Azure SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="d94f8-104">Naplózás</span><span class="sxs-lookup"><span data-stu-id="d94f8-104">Auditing</span></span>](sql-data-warehouse-auditing-overview.md)
> * [<span data-ttu-id="d94f8-105">Fenyegetések észlelése</span><span class="sxs-lookup"><span data-stu-id="d94f8-105">Threat detection</span></span>](sql-data-warehouse-security-threat-detection.md)
> 
> 

<span data-ttu-id="d94f8-106">Az SQL Data Warehouse naplózás segítségével toorecord események az adatbázis tooan naplóban az Azure Storage-fiókban.</span><span class="sxs-lookup"><span data-stu-id="d94f8-106">SQL Data Warehouse auditing allows you toorecord events in your database tooan audit log in your Azure Storage account.</span></span> <span data-ttu-id="d94f8-107">Naplózás segít törvényi megfelelőség fenntartásában, ismerje meg adatbázis-tevékenység, és azok az eltérések és rendellenességek, amelyek üzleti problémát jelenthetnek, vagy a biztonság megsértésére betekintést.</span><span class="sxs-lookup"><span data-stu-id="d94f8-107">Auditing can help you maintain regulatory compliance, understand  database activity, and gain insight into discrepancies and anomalies that could indicate business concerns or suspected security violations.</span></span> <span data-ttu-id="d94f8-108">Az SQL Data Warehouse naplózását is integrálható a Microsoft Power BI leásási jelentéskészítés és elemzés céljára.</span><span class="sxs-lookup"><span data-stu-id="d94f8-108">SQL Data Warehouse auditing also integrates with Microsoft Power BI for drill-down reporting and analysis.</span></span>

<span data-ttu-id="d94f8-109">A naplózási eszközök engedélyezése és betartása toocompliance szabványok megkönnyítése, de nem garantálják a megfelelőségi.</span><span class="sxs-lookup"><span data-stu-id="d94f8-109">Auditing tools enable and facilitate adherence toocompliance standards but don't guarantee compliance.</span></span> <span data-ttu-id="d94f8-110">További információ az Azure programokat, hogy támogatási szabványoknak való megfelelés, a következő témakörben: hello <a href="http://azure.microsoft.com/support/trust-center/compliance/" target="_blank">Azure biztonsági és adatkezelési központ</a>.</span><span class="sxs-lookup"><span data-stu-id="d94f8-110">For more information about Azure programs that support standards compliance, see hello <a href="http://azure.microsoft.com/support/trust-center/compliance/" target="_blank">Azure Trust Center</a>.</span></span>

* <span data-ttu-id="d94f8-111">[Adatbázis naplózási alapjai]</span><span class="sxs-lookup"><span data-stu-id="d94f8-111">[Database Auditing basics]</span></span>
* <span data-ttu-id="d94f8-112">[Az adatbázis naplózásának beállítása]</span><span class="sxs-lookup"><span data-stu-id="d94f8-112">[Set up auditing for your database]</span></span>
* <span data-ttu-id="d94f8-113">[Elemezze a vizsgálati naplók és jelentések]</span><span class="sxs-lookup"><span data-stu-id="d94f8-113">[Analyze audit logs and reports]</span></span>

## <span data-ttu-id="d94f8-114"><a id="subheading-1"></a>Az Azure SQL Data Warehouse Database Auditing alapjai</span><span class="sxs-lookup"><span data-stu-id="d94f8-114"><a id="subheading-1"></a>Azure SQL Data Warehouse Database Auditing basics</span></span>
<span data-ttu-id="d94f8-115">Az SQL Data Warehouse-adatbázis naplózásának teszi lehetővé:</span><span class="sxs-lookup"><span data-stu-id="d94f8-115">SQL Data Warehouse database auditing allows you to:</span></span>

* <span data-ttu-id="d94f8-116">**Tartsa meg** kijelölt események egy naplóban.</span><span class="sxs-lookup"><span data-stu-id="d94f8-116">**Retain** an audit trail of selected events.</span></span> <span data-ttu-id="d94f8-117">Adatbázis-műveletek toobe naplózva kategóriáinak adhat meg.</span><span class="sxs-lookup"><span data-stu-id="d94f8-117">You can define categories of database actions  toobe audited.</span></span>
* <span data-ttu-id="d94f8-118">**A jelentés** adatbázis-tevékenység.</span><span class="sxs-lookup"><span data-stu-id="d94f8-118">**Report** on database activity.</span></span> <span data-ttu-id="d94f8-119">Használhat előre konfigurált jelentések és egy irányítópult tooget használatának gyors megkezdése tevékenységet és az események naplózásához.</span><span class="sxs-lookup"><span data-stu-id="d94f8-119">You can use preconfigured reports and a dashboard tooget started quickly with activity and event reporting.</span></span>
* <span data-ttu-id="d94f8-120">**Elemezze** jelentéseket.</span><span class="sxs-lookup"><span data-stu-id="d94f8-120">**Analyze** reports.</span></span> <span data-ttu-id="d94f8-121">Gyanús események, a szokatlan tevékenység és a trendeket találja.</span><span class="sxs-lookup"><span data-stu-id="d94f8-121">You can find suspicious events, unusual activity, and trends.</span></span>

<span data-ttu-id="d94f8-122">Konfigurálhatja a naplózás a következő kategóriák hello:</span><span class="sxs-lookup"><span data-stu-id="d94f8-122">You can configure auditing for hello following event categories:</span></span>

<span data-ttu-id="d94f8-123">**Egyszerű SQL** és **paraméteres SQL** mely hello a gyűjtött naplók besorolt</span><span class="sxs-lookup"><span data-stu-id="d94f8-123">**Plain SQL** and **Parameterized SQL** for which hello collected audit logs are classified as</span></span>  

* <span data-ttu-id="d94f8-124">**Hozzáférés toodata**</span><span class="sxs-lookup"><span data-stu-id="d94f8-124">**Access toodata**</span></span>
* <span data-ttu-id="d94f8-125">**Sémaváltozások (DDL)**</span><span class="sxs-lookup"><span data-stu-id="d94f8-125">**Schema changes (DDL)**</span></span>
* <span data-ttu-id="d94f8-126">**Adatok módosításait (DML)**</span><span class="sxs-lookup"><span data-stu-id="d94f8-126">**Data changes (DML)**</span></span>
* <span data-ttu-id="d94f8-127">**Fiókok, szerepkörök és engedélyeket (DCL)**</span><span class="sxs-lookup"><span data-stu-id="d94f8-127">**Accounts, roles, and permissions (DCL)**</span></span>
* <span data-ttu-id="d94f8-128">**Tárolt eljárás**, **bejelentkezési** , és **tranzakció felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="d94f8-128">**Stored Procedure**, **Login** and, **Transaction Management**.</span></span>

<span data-ttu-id="d94f8-129">Minden esemény kategóriához a naplózási **sikeres** és **hiba** műveleteket külön-külön vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="d94f8-129">For each Event Category, Auditing of **Success** and **Failure** operations are configured separately.</span></span>

<span data-ttu-id="d94f8-130">További információ a hello tevékenységeket és naplóz eseményeket: hello <a href="http://go.microsoft.com/fwlink/?LinkId=506733" target="_blank">naplózási napló fájlformátum referenciája (doc fájl letöltése)</a>.</span><span class="sxs-lookup"><span data-stu-id="d94f8-130">For more information about hello activities and events audited, see hello <a href="http://go.microsoft.com/fwlink/?LinkId=506733" target="_blank">Audit Log Format Reference (doc file download)</a>.</span></span>

<span data-ttu-id="d94f8-131">Naplók az Azure storage-fiókok vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="d94f8-131">Audit logs are stored in your Azure storage account.</span></span> <span data-ttu-id="d94f8-132">Megadhatja, hogy az ellenőrzési napló megőrzési időtartam.</span><span class="sxs-lookup"><span data-stu-id="d94f8-132">You can define an audit log retention period.</span></span>

<span data-ttu-id="d94f8-133">A naplózási házirend meghatározása egy adott adatbázis vagy az alapértelmezett házirend-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="d94f8-133">An auditing policy can be defined for a specific database or as a default server policy.</span></span> <span data-ttu-id="d94f8-134">Kiszolgáló alapértelmezett naplózási házirend vonatkozik tooall adatbázisok egy kiszolgálón, amely nem rendelkezik egy adott felülbíráló adatbázis naplózási házirend lett meghatározva.</span><span class="sxs-lookup"><span data-stu-id="d94f8-134">A default server auditing policy applies tooall databases on a server, which do not have a specific overriding database auditing policy defined.</span></span>

<span data-ttu-id="d94f8-135">Jelölőnégyzet naplózás használata naplózási beállítása előtt egy ["Régebbi típusú ügyfelekhez."](sql-data-warehouse-auditing-downlevel-clients.md)</span><span class="sxs-lookup"><span data-stu-id="d94f8-135">Before setting up audit auditing check if you are using a ["Downlevel Client."](sql-data-warehouse-auditing-downlevel-clients.md)</span></span>

## <span data-ttu-id="d94f8-136"><a id="subheading-2"></a>Az adatbázis naplózásának beállítása</span><span class="sxs-lookup"><span data-stu-id="d94f8-136"><a id="subheading-2"></a>Set up auditing for your database</span></span>
1. <span data-ttu-id="d94f8-137">Indítsa el a hello <a href="https://portal.azure.com" target="_blank">Azure-portálon</a>.</span><span class="sxs-lookup"><span data-stu-id="d94f8-137">Launch hello <a href="https://portal.azure.com" target="_blank">Azure portal</a>.</span></span>
2. <span data-ttu-id="d94f8-138">Nyissa meg toohello **beállítások** hello SQL Data Warehouse tooaudit kívánt panel.</span><span class="sxs-lookup"><span data-stu-id="d94f8-138">Go toohello **Settings** blade of hello SQL Data Warehouse you want tooaudit.</span></span> <span data-ttu-id="d94f8-139">A hello **beállítások** panelen válassza **naplózási & Threat detection**.</span><span class="sxs-lookup"><span data-stu-id="d94f8-139">In hello **Settings** blade, select **Auditing & Threat detection**.</span></span>
   
    ![][1]
3. <span data-ttu-id="d94f8-140">Ezután engedélyezi a naplózást hello kattintva **ON** gombra.</span><span class="sxs-lookup"><span data-stu-id="d94f8-140">Next, enable auditing by clicking hello **ON** button.</span></span>
   
    ![][3]
4. <span data-ttu-id="d94f8-141">Hello naplózás konfigurációs panelen, jelölje ki **tárolási részletek** tooopen hello naplózási naplók tárolás panel.</span><span class="sxs-lookup"><span data-stu-id="d94f8-141">In hello auditing configuration blade, select **STORAGE DETAILS** tooopen hello Audit Logs Storage blade.</span></span> <span data-ttu-id="d94f8-142">Válassza ki a hello Azure storage-fiók naplók menteni és hello megőrzési időtartam.</span><span class="sxs-lookup"><span data-stu-id="d94f8-142">Select hello Azure storage account where logs will be saved and, hello retention period.</span></span> 
>[!TIP]
><span data-ttu-id="d94f8-143">Használjon hello ugyanazt a tárfiókot, az összes naplózott adatbázisok tooget hello hatékonyabb működtetését hello előre konfigurált jelentések sablonok.</span><span class="sxs-lookup"><span data-stu-id="d94f8-143">Use hello same storage account for all audited databases tooget hello most out of hello pre-configured reports templates.</span></span>
   
    ![][4]
5. <span data-ttu-id="d94f8-144">Kattintson a hello **OK** gomb részletek toosave hello tárkonfiguráció.</span><span class="sxs-lookup"><span data-stu-id="d94f8-144">Click hello **OK** button toosave hello storage details configuration.</span></span>
6. <span data-ttu-id="d94f8-145">A **-NAPLÓZÁS által esemény**, kattintson **sikeres** és **hiba** toolog összes eseményt, vagy válassza ki az egyes kategóriák.</span><span class="sxs-lookup"><span data-stu-id="d94f8-145">Under **LOGGING BY EVENT**, click **SUCCESS** and **FAILURE** toolog all events, or choose individual event categories.</span></span>
7. <span data-ttu-id="d94f8-146">Naplózási adatbázis beállításakor, szükség lehet az adatok naplózását megfelelően rögzített ügyfél tooensure tooalter hello kapcsolati karakterlánca.</span><span class="sxs-lookup"><span data-stu-id="d94f8-146">If you are configuring Auditing for a database, you may need tooalter hello connection string of your client tooensure data auditing is correctly captured.</span></span> <span data-ttu-id="d94f8-147">Ellenőrizze a hello [módosíthatja a kiszolgáló teljes Tartománynevet hello kapcsolat-karakterláncban](sql-data-warehouse-auditing-downlevel-clients.md) témakör korábbi verziójú ügyfél-kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="d94f8-147">Check hello [Modify Server FDQN in hello connection string](sql-data-warehouse-auditing-downlevel-clients.md) topic for downlevel client connections.</span></span>
8. <span data-ttu-id="d94f8-148">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="d94f8-148">Click **OK**.</span></span>

## <span data-ttu-id="d94f8-149"><a id="subheading-3"></a>Elemezze a vizsgálati naplók és jelentések</span><span class="sxs-lookup"><span data-stu-id="d94f8-149"><a id="subheading-3"></a>Analyze audit logs and reports</span></span>
<span data-ttu-id="d94f8-150">Naplófájlok tárolási táblákon, amelyekhez a gyűjtemény összesítése egy **SQLDBAuditLogs** hello Azure storage-fiók a telepítés során választott előtagot.</span><span class="sxs-lookup"><span data-stu-id="d94f8-150">Audit logs are aggregated in a collection of Store Tables with a **SQLDBAuditLogs** prefix in hello Azure storage account you chose during setup.</span></span> <span data-ttu-id="d94f8-151">Megtekintheti a naplófájlok egy eszközzel, mint <a href="http://azurestorageexplorer.codeplex.com/" target="_blank">Azure Tártallózó</a>.</span><span class="sxs-lookup"><span data-stu-id="d94f8-151">You can view log files using a tool such as <a href="http://azurestorageexplorer.codeplex.com/" target="_blank">Azure Storage Explorer</a>.</span></span>

<span data-ttu-id="d94f8-152">Előre konfigurált irányítópult jelentéssablon áll rendelkezésre, mert egy <a href="http://go.microsoft.com/fwlink/?LinkId=403540" target="_blank">letölthető Excel-táblázat</a> toohelp gyorsan elemez naplóadatait.</span><span class="sxs-lookup"><span data-stu-id="d94f8-152">A preconfigured dashboard report template is available as a <a href="http://go.microsoft.com/fwlink/?LinkId=403540" target="_blank">downloadable Excel spreadsheet</a> toohelp you quickly analyze log data.</span></span> <span data-ttu-id="d94f8-153">hello sablonjának toouse a naplókat, és van szükség az Excel 2013 vagy újabb verzió Power Query, amely letölthető <a href="http://www.microsoft.com/download/details.aspx?id=39379">Itt</a>.</span><span class="sxs-lookup"><span data-stu-id="d94f8-153">toouse hello template on your audit logs, you need Excel 2013 or later and Power Query, which you can download <a href="http://www.microsoft.com/download/details.aspx?id=39379">here</a>.</span></span>

<span data-ttu-id="d94f8-154">hello sablon a képzeletbeli mintaadatok rendelkezik, és állíthatja be a Power Query tooimport a napló-ről az Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="d94f8-154">hello template has fictional sample data in it, and you can set up Power Query tooimport your audit log directly from your Azure storage account.</span></span>

## <span data-ttu-id="d94f8-155"><a id="subheading-4"></a>Tárolási kulcs újragenerálása</span><span class="sxs-lookup"><span data-stu-id="d94f8-155"><a id="subheading-4"></a>Storage key regeneration</span></span>
<span data-ttu-id="d94f8-156">Éles valószínűleg a tárolási a kulcsok rendszeresen toorefresh áll.</span><span class="sxs-lookup"><span data-stu-id="d94f8-156">In production, you are likely toorefresh your storage keys periodically.</span></span> <span data-ttu-id="d94f8-157">A kulcsok frissítése, úgy kell toosave hello házirend.</span><span class="sxs-lookup"><span data-stu-id="d94f8-157">When refreshing your keys, you need toosave hello policy.</span></span> <span data-ttu-id="d94f8-158">hello folyamat a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="d94f8-158">hello process is as follows:</span></span>

1. <span data-ttu-id="d94f8-159">A naplózási konfiguráció panel (naplózás szakasz hello beállítása a fent leírt) hello kapcsoló hello **Tárelérési kulcs** a *elsődleges* túl*másodlagos* és  **MENTÉS**.</span><span class="sxs-lookup"><span data-stu-id="d94f8-159">In hello auditing configuration blade (described above in hello setup auditing section) switch hello **Storage Access Key** from *Primary* too*Secondary* and **SAVE**.</span></span>

   ![][4]
2. <span data-ttu-id="d94f8-160">Nyissa meg toohello tárolási konfiguráció panel és **újragenerálja** hello *elsődleges elérési kulcsot*.</span><span class="sxs-lookup"><span data-stu-id="d94f8-160">Go toohello storage configuration blade and **regenerate** hello *Primary Access Key*.</span></span>
3. <span data-ttu-id="d94f8-161">Lépjen vissza a naplózási konfiguráció blade, kapcsoló hello toohello **Tárelérési kulcs** a *másodlagos* túl*elsődleges* nyomja le az ENTER **mentése**.</span><span class="sxs-lookup"><span data-stu-id="d94f8-161">Go back toohello auditing configuration blade, switch hello **Storage Access Key** from *Secondary* too*Primary* and press **SAVE**.</span></span>
4. <span data-ttu-id="d94f8-162">Lépjen vissza a felhasználói felület toohello tárolási és **újragenerálja** hello *másodlagos elérési kulcsot* (mint hello előkészítése tovább kulcsok a frissítés.</span><span class="sxs-lookup"><span data-stu-id="d94f8-162">Go back toohello storage UI and **regenerate** hello *Secondary Access Key* (as preparation for hello next keys refresh cycle.</span></span>

## <span data-ttu-id="d94f8-163"><a id="subheading-5"></a>Automatizálási (PowerShell/REST API)</span><span class="sxs-lookup"><span data-stu-id="d94f8-163"><a id="subheading-5"></a>Automation (PowerShell/REST API)</span></span>
<span data-ttu-id="d94f8-164">A következő automation eszközök hello segítségével az Azure SQL Data Warehouse naplózás is konfigurálhatja:</span><span class="sxs-lookup"><span data-stu-id="d94f8-164">You can also configure auditing in Azure SQL Data Warehouse by using hello following automation tools:</span></span>

* <span data-ttu-id="d94f8-165">**PowerShell-parancsmagok**:</span><span class="sxs-lookup"><span data-stu-id="d94f8-165">**PowerShell cmdlets**:</span></span>

   * <span data-ttu-id="d94f8-166">[Get-AzureRMSqlDatabaseAuditingPolicy][101]</span><span class="sxs-lookup"><span data-stu-id="d94f8-166">[Get-AzureRMSqlDatabaseAuditingPolicy][101]</span></span>
   * <span data-ttu-id="d94f8-167">[Get-AzureRMSqlServerAuditingPolicy][102]</span><span class="sxs-lookup"><span data-stu-id="d94f8-167">[Get-AzureRMSqlServerAuditingPolicy][102]</span></span>
   * <span data-ttu-id="d94f8-168">[Remove-AzureRMSqlDatabaseAuditing][103]</span><span class="sxs-lookup"><span data-stu-id="d94f8-168">[Remove-AzureRMSqlDatabaseAuditing][103]</span></span>
   * <span data-ttu-id="d94f8-169">[Remove-AzureRMSqlServerAuditing][104]</span><span class="sxs-lookup"><span data-stu-id="d94f8-169">[Remove-AzureRMSqlServerAuditing][104]</span></span>
   * <span data-ttu-id="d94f8-170">[Set-AzureRMSqlDatabaseAuditingPolicy][105]</span><span class="sxs-lookup"><span data-stu-id="d94f8-170">[Set-AzureRMSqlDatabaseAuditingPolicy][105]</span></span>
   * <span data-ttu-id="d94f8-171">[Set-AzureRMSqlServerAuditingPolicy][106]</span><span class="sxs-lookup"><span data-stu-id="d94f8-171">[Set-AzureRMSqlServerAuditingPolicy][106]</span></span>
   * <span data-ttu-id="d94f8-172">[Használjon-AzureRMSqlServerAuditingPolicy][107]</span><span class="sxs-lookup"><span data-stu-id="d94f8-172">[Use-AzureRMSqlServerAuditingPolicy][107]</span></span>

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