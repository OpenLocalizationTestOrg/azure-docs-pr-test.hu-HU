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
# <a name="auditing-in-azure-sql-data-warehouse"></a>Az Azure SQL Data Warehouse naplózás
> [!div class="op_single_selector"]
> * [Naplózás](sql-data-warehouse-auditing-overview.md)
> * [Fenyegetések észlelése](sql-data-warehouse-security-threat-detection.md)
> 
> 

Az SQL Data Warehouse naplózás segítségével toorecord események az adatbázis tooan naplóban az Azure Storage-fiókban. Naplózás segít törvényi megfelelőség fenntartásában, ismerje meg adatbázis-tevékenység, és azok az eltérések és rendellenességek, amelyek üzleti problémát jelenthetnek, vagy a biztonság megsértésére betekintést. Az SQL Data Warehouse naplózását is integrálható a Microsoft Power BI leásási jelentéskészítés és elemzés céljára.

A naplózási eszközök engedélyezése és betartása toocompliance szabványok megkönnyítése, de nem garantálják a megfelelőségi. További információ az Azure programokat, hogy támogatási szabványoknak való megfelelés, a következő témakörben: hello <a href="http://azure.microsoft.com/support/trust-center/compliance/" target="_blank">Azure biztonsági és adatkezelési központ</a>.

* [Adatbázis naplózási alapjai]
* [Az adatbázis naplózásának beállítása]
* [Elemezze a vizsgálati naplók és jelentések]

## <a id="subheading-1"></a>Az Azure SQL Data Warehouse Database Auditing alapjai
Az SQL Data Warehouse-adatbázis naplózásának teszi lehetővé:

* **Tartsa meg** kijelölt események egy naplóban. Adatbázis-műveletek toobe naplózva kategóriáinak adhat meg.
* **A jelentés** adatbázis-tevékenység. Használhat előre konfigurált jelentések és egy irányítópult tooget használatának gyors megkezdése tevékenységet és az események naplózásához.
* **Elemezze** jelentéseket. Gyanús események, a szokatlan tevékenység és a trendeket találja.

Konfigurálhatja a naplózás a következő kategóriák hello:

**Egyszerű SQL** és **paraméteres SQL** mely hello a gyűjtött naplók besorolt  

* **Hozzáférés toodata**
* **Sémaváltozások (DDL)**
* **Adatok módosításait (DML)**
* **Fiókok, szerepkörök és engedélyeket (DCL)**
* **Tárolt eljárás**, **bejelentkezési** , és **tranzakció felügyeleti**.

Minden esemény kategóriához a naplózási **sikeres** és **hiba** műveleteket külön-külön vannak konfigurálva.

További információ a hello tevékenységeket és naplóz eseményeket: hello <a href="http://go.microsoft.com/fwlink/?LinkId=506733" target="_blank">naplózási napló fájlformátum referenciája (doc fájl letöltése)</a>.

Naplók az Azure storage-fiókok vannak tárolva. Megadhatja, hogy az ellenőrzési napló megőrzési időtartam.

A naplózási házirend meghatározása egy adott adatbázis vagy az alapértelmezett házirend-kiszolgáló. Kiszolgáló alapértelmezett naplózási házirend vonatkozik tooall adatbázisok egy kiszolgálón, amely nem rendelkezik egy adott felülbíráló adatbázis naplózási házirend lett meghatározva.

Jelölőnégyzet naplózás használata naplózási beállítása előtt egy ["Régebbi típusú ügyfelekhez."](sql-data-warehouse-auditing-downlevel-clients.md)

## <a id="subheading-2"></a>Az adatbázis naplózásának beállítása
1. Indítsa el a hello <a href="https://portal.azure.com" target="_blank">Azure-portálon</a>.
2. Nyissa meg toohello **beállítások** hello SQL Data Warehouse tooaudit kívánt panel. A hello **beállítások** panelen válassza **naplózási & Threat detection**.
   
    ![][1]
3. Ezután engedélyezi a naplózást hello kattintva **ON** gombra.
   
    ![][3]
4. Hello naplózás konfigurációs panelen, jelölje ki **tárolási részletek** tooopen hello naplózási naplók tárolás panel. Válassza ki a hello Azure storage-fiók naplók menteni és hello megőrzési időtartam. 
>[!TIP]
>Használjon hello ugyanazt a tárfiókot, az összes naplózott adatbázisok tooget hello hatékonyabb működtetését hello előre konfigurált jelentések sablonok.
   
    ![][4]
5. Kattintson a hello **OK** gomb részletek toosave hello tárkonfiguráció.
6. A **-NAPLÓZÁS által esemény**, kattintson **sikeres** és **hiba** toolog összes eseményt, vagy válassza ki az egyes kategóriák.
7. Naplózási adatbázis beállításakor, szükség lehet az adatok naplózását megfelelően rögzített ügyfél tooensure tooalter hello kapcsolati karakterlánca. Ellenőrizze a hello [módosíthatja a kiszolgáló teljes Tartománynevet hello kapcsolat-karakterláncban](sql-data-warehouse-auditing-downlevel-clients.md) témakör korábbi verziójú ügyfél-kommunikációhoz.
8. Kattintson az **OK** gombra.

## <a id="subheading-3"></a>Elemezze a vizsgálati naplók és jelentések
Naplófájlok tárolási táblákon, amelyekhez a gyűjtemény összesítése egy **SQLDBAuditLogs** hello Azure storage-fiók a telepítés során választott előtagot. Megtekintheti a naplófájlok egy eszközzel, mint <a href="http://azurestorageexplorer.codeplex.com/" target="_blank">Azure Tártallózó</a>.

Előre konfigurált irányítópult jelentéssablon áll rendelkezésre, mert egy <a href="http://go.microsoft.com/fwlink/?LinkId=403540" target="_blank">letölthető Excel-táblázat</a> toohelp gyorsan elemez naplóadatait. hello sablonjának toouse a naplókat, és van szükség az Excel 2013 vagy újabb verzió Power Query, amely letölthető <a href="http://www.microsoft.com/download/details.aspx?id=39379">Itt</a>.

hello sablon a képzeletbeli mintaadatok rendelkezik, és állíthatja be a Power Query tooimport a napló-ről az Azure storage-fiók.

## <a id="subheading-4"></a>Tárolási kulcs újragenerálása
Éles valószínűleg a tárolási a kulcsok rendszeresen toorefresh áll. A kulcsok frissítése, úgy kell toosave hello házirend. hello folyamat a következőképpen történik:

1. A naplózási konfiguráció panel (naplózás szakasz hello beállítása a fent leírt) hello kapcsoló hello **Tárelérési kulcs** a *elsődleges* túl*másodlagos* és  **MENTÉS**.

   ![][4]
2. Nyissa meg toohello tárolási konfiguráció panel és **újragenerálja** hello *elsődleges elérési kulcsot*.
3. Lépjen vissza a naplózási konfiguráció blade, kapcsoló hello toohello **Tárelérési kulcs** a *másodlagos* túl*elsődleges* nyomja le az ENTER **mentése**.
4. Lépjen vissza a felhasználói felület toohello tárolási és **újragenerálja** hello *másodlagos elérési kulcsot* (mint hello előkészítése tovább kulcsok a frissítés.

## <a id="subheading-5"></a>Automatizálási (PowerShell/REST API)
A következő automation eszközök hello segítségével az Azure SQL Data Warehouse naplózás is konfigurálhatja:

* **PowerShell-parancsmagok**:

   * [Get-AzureRMSqlDatabaseAuditingPolicy][101]
   * [Get-AzureRMSqlServerAuditingPolicy][102]
   * [Remove-AzureRMSqlDatabaseAuditing][103]
   * [Remove-AzureRMSqlServerAuditing][104]
   * [Set-AzureRMSqlDatabaseAuditingPolicy][105]
   * [Set-AzureRMSqlServerAuditingPolicy][106]
   * [Használjon-AzureRMSqlServerAuditingPolicy][107]

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