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
# <a name="get-started-with-sql-database-auditing"></a>Ismerkedés az SQL-adatbázis naplózási szolgáltatásával
Azure SQL database auditing követi az adatbázisban, és beírja őket az Azure-tárfiókot tooan audit napló. A naplózás is:

* Segít törvényi megfelelőség fenntartásában, ismerje meg adatbázis-tevékenység, és azok az eltérések és rendellenességek, amelyek üzleti problémát jelenthetnek, vagy a biztonság megsértésére betekintést.

* Lehetővé teszi, és elősegíti a való toocompliance szabványok, de azt nem garantálja a megfelelőségi. További információ az Azure programokat, hogy támogatási szabványoknak való megfelelés, a következő témakörben: hello [Azure biztonsági és adatkezelési központ](https://azure.microsoft.com/support/trust-center/compliance/).


## <a id="subheading-1"></a>Az Azure SQL adatbázis naplózásának áttekintése
SQL-adatbázis a naplózás is használhatja:


* **Tartsa meg** kijelölt események egy naplóban. Adatbázis-műveletek toobe naplózva kategóriáinak adhat meg.
* **A jelentés** adatbázis-tevékenység. Használhat előre konfigurált jelentések és egy irányítópult tooget használatának gyors megkezdése tevékenységet és az események naplózásához.
* **Elemezze** jelentéseket. Gyanús események, a szokatlan tevékenység és a trendeket találja.

Konfigurálhatja a kategóriák, különböző típusú naplózás hello leírtak [az adatbázis naplózásának beállítása](#subheading-2) szakasz.

Naplók az Azure-előfizetése írt tooAzure Blob-tároló.


## <a id="subheading-8"></a>Adja meg a kiszolgálói szintű és adatbázis-szintű naplózási házirend

A naplózási házirend meghatározása egy adott adatbázis vagy az alapértelmezett házirend-kiszolgáló:

* Kiszolgálói házirend vonatkozik tooall meglévő és az újonnan létrehozott adatbázisok hello kiszolgálón.

* Ha *server blobnaplózási funkció engedélyezve van*, azt *mindig érvényes toohello adatbázis* (Ez azt jelenti, hogy hello adatbázishoz lesznek naplózva), függetlenül hello adatbázis naplózási beállításait.

* Engedélyezi a naplózást hello adatbázis hozzáadása tooenabling azt hello kiszolgálón blob lesz *nem* felülbírálás vagy hello hello blobnaplózási funkció beállításainak módosításához. Mindkét naplózás jelen egymás mellett. Más szóval hello adatbázishoz lesznek naplózva kétszer párhuzamosan (egyszer hello kiszolgálószabályzat és egyszer hello adatbázis házirend).

   > [!NOTE]
   > Kerülje engedélyezésével server blob naplózási, mind az adatbázis blobnaplózási funkció együtt, kivéve, ha:
    > * Azt szeretné, hogy egy másik toouse *tárfiók* vagy *megőrzési időszak* adott adatbázishoz.
    > * Egy adott adatbázis, amely eltér a eseménytípusok vagy hello adatbázisok hello kiszolgálón hello többi naplóz kategóriákat kívánt tooaudit eseménytípusok vagy kategóriák. Például lehetséges, hogy csak az adott adatbázis naplózva toobe igénylő tábla beszúrása.
   > 
   > Ellenkező esetben azt javasoljuk, hogy engedélyezze a csak a kiszolgálói szintű blobnaplózási funkció, és hagyja hello adatbázis szintű naplózás le van tiltva az összes olyan adatbázis.


## <a id="subheading-2"></a>Az adatbázis naplózásának beállítása
hello következő szakasz írja le hello naplózási hello Azure-portál használatával.

1. Nyissa meg toohello [Azure-portálon](https://portal.azure.com).
2. Nyissa meg toohello **beállítások** hello SQL adatbázis vagy SQL server tooaudit kívánt panel. A hello **beállítások** panelen válassza **naplózási & Threat detection**.

    <a id="auditing-screenshot"></a>![Navigációs ablaktábla][1]
3. Kiszolgáló naplózási házirend (amelyek érvényben tooall meglévő és az újonnan létrehozott adatbázisok ezen a kiszolgálón) tooset tetszés szerint kiválaszthatja hello **beállításainak megtekintéséhez** hello adatbázis naplózási paneljén hivatkozásra. Ezt követően megtekintheti vagy hello kiszolgáló naplózási beállításainak módosítása.

    ![Navigációs ablaktábla][2]
4. Ha inkább tooenable blobnaplózási funkció hello adatbázis szintjén (hozzáadása tooor kiszolgálószintű naplózás helyett), a **naplózási**, jelölje be **ON**, és a **típus naplózás** , válassza ki **Blob**.

    Ha a kiszolgáló blobnaplózási funkció engedélyezve van, hello adatbázis konfigurált naplózási jelen hello server blob naplózási és.  

    ![Navigációs ablaktábla][3]
5. tooopen hello **naplózási naplók tárolási** panelen válassza **tárolási részletek**. Válassza ki a hello Azure storage-fiók ahol naplókat a rendszer menti, és válassza a hello megőrzési időtartam, mely hello után a régi naplókat törlődik. Ezután kattintson az **OK** gombra. 
   >[!TIP] 
   >tooget hello hatékonyabb működtetését hello naplózási jelentések sablonok, használja ugyanazt a tárfiókot, az összes naplózott adatbázis hello. 

    <a id="storage-screenshot"></a>![Navigációs ablaktábla][4]
6. Ha azt szeretné, hogy toocustomize hello naplózott események, ehhez a PowerShell segítségével, vagy hello REST API-t. További részletekért lásd: hello [automatizálási (PowerShell/REST API-t)](#subheading-7) szakasz.
7. A naplózási beállítások konfigurálása után hello új threat detection szolgáltatás, és e-mailek tooreceive biztonsági riasztások konfigurálása. Fenyegetésészlelés használatakor kapni proaktív riasztások a adatbázist érintő rendellenes tevékenységeket, amely azt jelzi, hogy a esetleges biztonsági fenyegetéseket jelezhetnek. További részletekért lásd: [Ismerkedés a fenyegetésészlelés](sql-database-threat-detection-get-started.md).
8. Kattintson a **Save** (Mentés) gombra.





## <a id="subheading-3"></a>Elemezze a vizsgálati naplók és jelentések
Naplók az Azure storage-fiók a telepítés során választott hello összesítése. Eszköz használatával felfedezheti a vizsgálati naplók [Azure Tártallózó](http://storageexplorer.com/).

Menti a BLOB naplóinak blob fájlok belül nevű tárolót **sqldbauditlogs**.

Hello blob naplózási naplók tároló mappát, blob elnevezési konvenciókat és időformátumát hello hierarchia további részleteiért lásd: hello [Blob naplózási napló fájlformátum referenciája (.docx fájl letöltése)](https://go.microsoft.com/fwlink/?linkid=829599).

Többféleképpen tooview blob naplófájlokat is használhatja:

* Használjon hello [Azure-portálon](https://portal.azure.com).  Nyissa meg hello vonatkozó adatbázis. At hello felső hello adatbázis **naplózási & Threat detection** panelen kattintson a **nézet naplók**.

    ![Navigációs ablaktábla][7]

    Egy **rekordok naplózása** panelen nyitja meg, amelyből be fog tudni tooview hello naplókat.

    - Gombra kattintva megtekintheti az adott dátumok **szűrő** hello hello tetején **rekordok naplózása** panelen.
    - Naplózási azt jelzi, hogy a kiszolgáló házirend vagy az adatbázis házirend naplózási hozott létre válthat.

       ![Navigációs ablaktábla][8]

* Hello rendszer funkcióval **sys.fn_get_audit_file** (T-SQL) tooreturn hello napló adatairól táblázatos formátumban. Ez a funkció használatáról további információkért lásd: hello [sys.fn_get_audit_file dokumentáció](https://docs.microsoft.com/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql).


* Használjon **naplózási fájlok egyesítése** az SQL Server Management Studio (SSMS 17-től induló):  
    1. Hello SSMS menüben válassza ki a **fájl** > **nyitott** > **naplózási fájlok egyesítése**.

        ![Navigációs ablaktábla][9]
    2. Hello **naplózási fájlok hozzáadása** párbeszédpanel. Válasszon ki egy hello **Hozzáadás** beállítások kiválasztása, hogy toomerge naplózási fájlok helyi lemezre vagy importálja azokat az Azure Storage (akkor szükséges tooprovide lesz az Azure Storage részletek és a fiókkulcsot).

    3. Minden fájlok toomerge hozzáadása után kattintson **OK** toocomplete hello egyesítési művelet.

    4. hello egyesített fájlt az SSMS, ahol megtekintheti és elemezze, továbbá exportálási azt tooan xel-fájlt vagy a fürt megosztott kötetei szolgáltatás fájl vagy tooa tábla nyitja meg.

* Használjon hello [alkalmazás szinkronizálása](https://github.com/Microsoft/Azure-SQL-DB-auditing-OMS-integration) , hogy létrehoztunk. Az Azure-ban fut, és használja az Operations Management Suite (OMS) szolgáltatáshoz nyilvános API-k toopush SQL naplók az OMS Szolgáltatáshoz. alkalmazás hello szinkronizálása leküldéses értesítések SQL naplók OMS Log Analyticshez való felhasználásra hello OMS Naplóelemzési irányítópulton keresztül. 

* Használja a Power bi-ban. Megtekintheti és elemezheti a napló adatairól a Power bi-ban. További információ [Power bi-ban, és hozzáférést egy letölthető sablon](https://blogs.msdn.microsoft.com/azuresqldbsupport/2017/05/26/sql-azure-blob-auditing-basic-power-bi-dashboard/).

* Naplófájlok letölthető az Azure Storage-blob tároló hello portálon vagy az eszköz használatával [Azure Tártallózó](http://storageexplorer.com/).
    * Miután letöltötte a helyi naplófájlt, hello fájl tooopen, duplán kattintva megtekintheti, és a szolgáltatáshoz az ssms hello naplóinak elemzése.
    * Azure Tártallózó keresztül egyszerre több fájl is letölthető. Kattintson a jobb gombbal egy adott almappát (például egy almappát, amely tartalmazza az összes naplófájl egy adott dátumhoz tartozó), és válassza ki **Mentés másként** toosave egy helyi mappába.

* További módszereket:
   * A letöltés több fájlt (vagy egy almappára tartalmaz naplófájlok teljes nap, a hello előző részben leírtak szerint) után egyesítheti helyileg hello SSMS egyesítési naplófájlokat utasításokat lásd a korábbiakban leírtak szerint.

   * Nézet blobnaplózási funkció szoftveres naplói:

     * Használjon hello [kiterjesztett események olvasó](https://blogs.msdn.microsoft.com/extended_events/2011/07/20/introducing-the-extended-events-reader/) C# könyvtár.
     * [Kiterjesztett események fájljainak lekérdezése](https://sqlscope.wordpress.com/2014/11/15/reading-extended-event-files-using-client-side-tools-only/) PowerShell használatával.




## <a id="subheading-5"></a>Éles eljárások
<!--hello description in this section refers toopreceding screen captures.-->

### <a id="subheading-6">Georeplikált adatbázisok naplózás</a>
Georeplikált adatbázist kell használni, amikor nem lehetséges tooset naplózási hello elsődleges adatbázisra, hello másodlagos adatbázis, vagy mindkettőt hello naplózási típusától függően.

A következő lépések követésével (ne feledje, hogy blobnaplózási funkció kapcsolható be- és kikapcsolása csak a naplózási beállítások hello elsődleges adatbázis):

* **Elsődleges adatbázis**. Kapcsolja be a blobnaplózási funkció, hello kiszolgálón vagy hello adatbázison, lásd: hello [az adatbázis naplózásának beállítása](#subheading-2) szakasz.
* **Másodlagos adatbázis**. Kapcsolja be a blobnaplózási funkció hello elsődleges adatbázis, a hello [az adatbázis naplózásának beállítása](#subheading-2) szakasz. 
   * BLOB naplózását engedélyezni kell a hello *maga elsődleges adatbázis*, nem hello kiszolgáló.
   * Blob naplózás engedélyezése után hello elsődleges adatbázison, akkor is válik elérhetővé, hello másodlagos adatbázison.

     >[!IMPORTANT]
     >Hello hello másodlagos adatbázis-tárolási beállításai a kereszt-területi forgalmat, amely alapértelmezés szerint lesz azonos toothose hello elsődleges adatbázis. Ez elkerülheti a blobnaplózási funkció hello másodlagos kiszolgálón, és a helyi tároló konfigurálása a hello másodlagos kiszolgáló tárolási beállítások engedélyezése. Ez a művelet felülírja a hello tárolási helyét hello másodlagos adatbázis és az egyes adatbázisok naplózási naplók toolocal tárolóval mentése eredményez.  
<br>

### <a id="subheading-6">Tárolási kulcs újragenerálása</a>
Éles valószínűleg a tárolási a kulcsok rendszeresen toorefresh áll. A kulcsok frissítése, úgy kell tooresave hello naplózási házirend. hello folyamat a következőképpen történik:

1. Nyissa meg hello **tárolási részletek** panelen. A hello **Tárelérési kulcs** mezőben válassza **másodlagos**, és kattintson a **OK**. Kattintson a **mentése** naplózás konfigurációs panel hello hello tetején.

    ![Navigációs ablaktábla][5]
2. Nyissa meg toohello tárolási konfiguráció panelt, és hello elsődleges elérési kulcs újragenerálása.

    ![Navigációs ablaktábla][6]
3. Lépjen vissza a naplózási konfiguráció panel toohello, hello tárelérési kulcs másodlagos tooprimary a kapcsolóhoz, és kattintson **OK**. Kattintson a **mentése** naplózás konfigurációs panel hello hello tetején.
4. Lépjen vissza a toohello tárolási konfiguráció panel megnyitásához, és az újbóli létrehozás hello másodlagos elérési kulcsát (a frissítés során hello tovább kulcs előkészítése).

## <a id="subheading-7"></a>Automatizálási (PowerShell/REST API)
A következő automation eszközök hello segítségével az Azure SQL Database naplózási is konfigurálhatja:

* **PowerShell-parancsmagok**:

   * [Get-AzureRMSqlDatabaseAuditingPolicy][101]
   * [Get-AzureRMSqlServerAuditingPolicy][102]
   * [Remove-AzureRMSqlDatabaseAuditing][103]
   * [Remove-AzureRMSqlServerAuditing][104]
   * [Set-AzureRMSqlDatabaseAuditingPolicy][105]
   * [Set-AzureRMSqlServerAuditingPolicy][106]
   * [Használjon-AzureRMSqlServerAuditingPolicy][107]

   Tekintse meg a parancsfájl például [konfigurálhatja a naplózás és a fenyegetések észlelésére, a PowerShell használatával](scripts/sql-database-auditing-and-threat-detection-powershell.md).

* **REST API - Blobnaplózási funkció**:

   * [Hozzon létre vagy frissítés adatbázis Blob naplózási házirend](https://msdn.microsoft.com/library/azure/mt695939.aspx)
   * [Hozzon létre vagy frissítési kiszolgáló Blob naplózási házirend](https://msdn.microsoft.com/library/azure/mt771861.aspx)
   * [Adatbázis-Blob naplórendet beolvasása](https://msdn.microsoft.com/library/azure/mt695938.aspx)
   * [Kiszolgáló Blob naplórendet beolvasása](https://msdn.microsoft.com/library/azure/mt771860.aspx)
   * [Műveleti eredmény naplózás Server Blob beolvasása](https://msdn.microsoft.com/library/azure/mt771862.aspx)


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
