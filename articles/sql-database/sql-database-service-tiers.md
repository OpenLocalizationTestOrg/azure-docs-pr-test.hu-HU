---
title: "Az Azure SQL Database szolgáltatás- és rétegek |} Microsoft Docs"
description: "Hasonlítsa össze az SQL-adatbázis szolgáltatásszintjei és teljesítményszintjei az önálló adatbázisok és SQL rugalmas készletek bevezetése"
keywords: "adatbázis-beállítások, adatbázis-teljesítmény"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: f5c5c596-cd1e-451f-92a7-b70d4916e974
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 06/30/2017
ms.author: carlrab
ms.openlocfilehash: b25ff5331f119efd44c61808f7d1d5decb226bd6
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="what-performance-options-are-available-for-an-azure-sql-database"></a>Milyen teljesítménybeállításai érhetők el az Azure SQL-adatbázis

[Az Azure SQL Database](sql-database-technical-overview.md) négy szolgáltatásszintek mindkét egyetlen kínál és [készletezett](sql-database-elastic-pool.md) adatbázisok. A szolgáltatási rétegekkel vannak: **alapvető**, **szabványos**, **prémium**, és **prémium RS**. Egyes szolgáltatásszinteken belül több teljesítményszint vannak ([dtu-k](sql-database-what-is-a-dtu.md)) és tárolási lehetőségei kezelje a számítási feladatok és az adatok mérete eltér. Magasabb teljesítményszintet továbbít, egyre nagyobb átviteli sebesség és a kapacitás további számítási és tárolási erőforrásokat biztosít. Szolgáltatási szinteket, teljesítményszintjeivel és tárterületének leállás nélküli dinamikusan módosíthatja. 
- **Alapszintű**, **szabványos** és **prémium** összes szolgáltatásszintek szolgáltatásiszint-szerződés 99.99 %, rugalmas üzletmenet-folytonossági funkciókat, biztonsági szolgáltatásokat és óradíj alapú számlázást nyújtanak hasznos üzemidőt rendelkezik. 
- A **prémium RS** réteg csökkentett SLA-t a prémium tarifacsomagra, az azonos teljesítményszintet biztosít, mert a redundáns másolatok alacsonyabb több mint egy adatbázis szolgáltatásrétegeiben használt funkciókkal futása. Igen a szolgáltatás hibája esetén szükség lehet helyreállítani az adatbázist egy biztonsági másolatának legfeljebb egy 5 perces késéssel.

> [!IMPORTANT]
> Azure SQL-adatbázis lekérdezi erőforrások garantált készletét, és az adatbázis elvárt teljesítményjellemzői nem érinti a bármely más adatbázis Azure-ban. 

## <a name="choosing-a-service-tier"></a>Szolgáltatásszint kiválasztása
Az alábbi táblázat példákat tartalmaz arra vonatkozóan, hogy a különböző alkalmazások és teljesítményprofilok esetén melyik a legmegfelelőbb szolgáltatásszint.

| Szolgáltatásszint | Kívánt teljesítményprofilok |
| :--- | --- |
| **Basic** | Leginkább kis adatbázisokhoz felel meg, egyidejűleg általában egyetlen aktív műveletet támogat. Alkalmas például fejlesztési vagy tesztelési célokra használt adatbázisokhoz vagy kisméretű, ritkán használt alkalmazásokhoz. |
| **Standard** |Ez az első számú megoldás az alacsony és közepes I/O-teljesítményigényű felhőalapú alkalmazások többségéhez, egyidejűleg több lekérdezést támogat. Alkalmas például munkacsoportokhoz vagy webalkalmazásokhoz. |
| **Prémium** | Nagy mennyiségű, nagy I/O-teljesítményigényű tranzakcióhoz tervezve, számos egyidejű felhasználót támogat. Alkalmas például az üzletmenet szempontjából kritikus alkalmazásokat támogató adatbázisokhoz. |
| **Prémium szintű RS** | A legnagyobb rendelkezésre állást biztosítja, hogy nem igénylő IO-igényes munkaterhelések tervezték. Például nagy teljesítményű munkaterhelések, vagy egy analitikai munkaterhelés, ahol az adatbázis nincs rekord, a rendszer. |
|||

Dedikált erőforrások belül egy adott szolgáltatásréteg hozhat létre önálló adatbázisok [teljesítményszintet](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) vagy adatbázisok belül is létrehozhat egy [SQL rugalmas készlet](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus). SQL rugalmas készletben a számítási és tárolási erőforrások több adatbázis egyetlen logikai kiszolgáló között vannak megosztva. 

Forrásanyag is elérhető az önálló adatbázisok adatbázis-tranzakciós egységek (dtu-k) vannak kifejezve, és egy SQL rugalmas készletek erőforrásait rugalmas adatbázis-tranzakciós egységek (edtu-k) vannak kifejezve. Dtu és edtu-k kapcsolatban bővebben lásd: [Dtu és edtu-k?](sql-database-what-is-a-dtu.md).

A szolgáltatásszintre vonatkozó döntés meghozatalakor először határozza meg a minimálisan szükséges adatbázis-jellemzőket:

| **Szolgáltatási szint szolgáltatások** | **Basic** | **Standard** | **Prémium** | **Prémium szintű RS**|
| :-- | --: | --: | --: | --: |
| Egyetlen adatbázis maximális mérete | 2 GB | 250 GB | 4 TB -OS *  | 500 GB  |
| A rugalmas készlet maximális mérete | 156 GB | 2,9 TB | 4 TB -OS * | 750 GB |
| A rugalmas készletekben található adatbázis maximális mérete | 2 GB | 250 GB | 500 GB | 500 GB |
| Készletenként adatbázisok maximális számát | 500  | 500 | 100 | 100 |
| Maximális egyetlen adatbázis dtu-k | 5 | 100 | 4000 | 1000 |
| A rugalmas készletekben található adatbázisonkénti maximális dtu-k | 5 | 3000 | 4000 | 1000 |
| Adatbázis biztonsági mentés megőrzési időtartam | 7 nap | 35 nap | 35 nap | 35 nap |
||||||

> [!IMPORTANT]
> Tárolási legfeljebb 4 TB érhető el jelenleg a következő régióban: US East2, USA nyugati régiója, Velünk – (kormányzati) Virginia, Nyugat-Európa, Németország központi, Dél Kelet-Ázsiában, kelet-japán, Kelet-Ausztrália, Kanada központi és Kanada keleti. Lásd: [jelenlegi 4 TB-os korlátozásai](sql-database-service-tiers.md#current-limitations-of-p11-and-p15-databases-with-4-tb-maxsize)
>

Miután megadta, hogy a megfelelő szolgáltatási rétegben, készen áll a teljesítményszintet (dtu-inak száma) és az adatbázis mennyisége határozza meg. 

- A szintek S2 és S3 teljesítményét a **szabványos** réteg általában jó kiindulási pont. 
- Olyan adatbázisok esetén magas CPU, vagy az I/O követelmények, a teljesítmény szintjén a **prémium** réteg kiindulási pontjaként jobb. 
- A **prémium** nyújt további CPU és 10 x a lehető legjobb teljesítmény és további I/O szintjén lévő elindítja a **szabványos** réteg.
- A **PremiumRS** teljesítményének kínál a **prémium** réteg alacsonyabb költségekkel, de a csökkentett SLA-t.

> [!IMPORTANT]
> Tekintse át a [SQL rugalmas készletek](sql-database-elastic-pool.md) csoportosításával adatbázisok SQL rugalmas készletek számítási és tárolási erőforrások megosztása információt a témakör. Ez a témakör további része a szolgáltatásszintek és teljesítményszintek az önálló adatbázisok összpontosít.
>

## <a name="single-database-service-tiers-and-performance-levels"></a>Önálló adatbázis szolgáltatásszintjei és teljesítményszintjei
Az önálló adatbázisok vannak több teljesítményszintet és tárolási összegek egyes szolgáltatásszinteken belül. 

[!INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

## <a name="scaling-up-or-scaling-down-a-single-database"></a>Önálló adatbázis vertikális fel- és leskálázása

Miután kiválasztotta a kezdeti szolgáltatásszintet és teljesítményszintet, a tényleges tapasztalatok alapján dinamikusan fel- vagy leskálázhatja az önálló adatbázist.  

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]
>

Az adatbázis szolgáltatásszintjének és/vagy teljesítményszintjének megváltoztatásakor egy replika jön létre az eredeti adatbázisról az új teljesítményszinten, majd a rendszer átáll a replikához való csatlakozásra. A folyamat során nem történik adatvesztés, de a replikára való átálláskor egy pillanatra az adatbázis felé irányuló kapcsolatok le lesznek tiltva, így lehet, hogy néhány folyamatban lévő tranzakció vissza lesz állítva. Mennyi ideig a kell váltania a változó, de általában a 4 másodperc érték kisebb, mint 30 másodperc 99 %-ában. Ha nagy számát, a pillanatnyilag kapcsolatok útban tranzakciók le vannak tiltva, akkor lehet, hogy mennyi ideig a kell váltania a hosszabb.  

A teljes felskálázási folyamat időtartama az adatbázis a módosítás előtti és utáni méretétől és szolgáltatásszintjétől függ. Például egy 250 GB-os adatbázis esetén, amelyet a Standard szolgáltatásszintről/szolgáltatásszintre vagy a Standard szolgáltatásszinten belül módosítunk, legfeljebb 6 órát vehet igénybe. Egy hasonló méretű adatbázis esetén, amelynél a Prémium szinten belül módosul a teljesítményszint, legfeljebb 3 órát vehet igénybe.

> [!TIP]
> A folyamatban lévő művelet skálázás SQL adatbázis állapotának ellenőrzéséhez használhatja a következő lekérdezés: ```select * from sys.dm_operation_status```.
>

* Szolgáltatási szint vagy a teljesítmény a magasabb frissíti, az adatbázis maximális mérete nem növeli hacsak Ön kifejezetten megad egy nagyobb maximális méretét.
* Egy adatbázis megállapításában, az adatbázis kisebb, mint a maximálisan megengedett méretet a cél szolgáltatási rétegben kell lennie. 
* Ha az adatbázis frissítése [georeplikáció](sql-database-geo-replication-portal.md) engedélyezve van, frissítse a másodlagos adatbázisok a kívánt teljesítményt réteghez csak azután frissítse az elsődleges adatbázis (általános útmutatást). Egy másik kiadásra ugrading való frissítéskor a másodlagos adatbázis először szükség. 
* Amikor alacsonyabb verziójúra változtatása adatbázis [georeplikáció](sql-database-geo-replication-portal.md) engedélyezve van, alacsonyabb a kívánt teljesítményt réteghez elsődleges adatbázisainak előtt alacsonyabb verziójúra változtatása a másodlagos adatbázis (általános útmutatást). Ha az elsődleges adatbázis először visszaminősítése másik kiadásra alacsonyabb verziójúra változtatása meg kell adni. 

* A visszaállítási szolgáltatásajánlatok eltérőek a különböző szolgáltatásszintek esetében. Ha alacsonyabb verzióra való visszatérést meg, hogy a **alapvető** réteg, hogy alacsonyabb biztonsági mentés megőrzési időtartamot - lásd [Azure SQL adatbázis biztonsági másolatait](sql-database-automated-backups.md).
* Az adatbázis új tulajdonságai csak akkor lesznek alkalmazva, ha a módosítások befejeződtek.


## <a name="current-limitations-of-p11-and-p15-databases-with-4-tb-maxsize"></a>4 TB-os maxsize P11 és P15 adatbázisokkal aktuális korlátozásai

A maximális mérete 4 TB-os P11 és P15 adatbázis támogatott egyes régiókban (előbb tárgyalt módon). A következő szempontokat és korlátozások vonatkoznak a 4 TB-os maxsize P11 és P15 adatbázisokkal:

- Ha egy adatbázist (értéke 4 TB-os vagy 4096 GB) létrehozásához a 4 TB-os maxsize beállítást választja, a Létrehozás parancs hibával meghiúsul, ha az adatbázis egy nem támogatott régióban lett kiépítve.
- A támogatott régiók egyikéhez sem található meglévő P11 és P15 adatbázisokhoz növelheti a 4 TB-os maxsize tárhelyet. Ez ellenőrizhető használatával a [válasszon DATABASEPROPERTYEX](https://msdn.microsoft.com/library/ms186823.aspx) vagy ellenőrizze az adatbázis mérete az Azure portálon. Egy meglévő P11 vagy P15 frissítése adatbázis csak hajtható végre egy kiszolgálószintű fő bejelentkezéssel, vagy a dbmanager adatbázis-szerepkör tagjai. 
- Ha egy frissítési művelet végrehajtása egy támogatott régióban a konfigurációja azonnal frissül. Az adatbázis online állapotban marad, a frissítési folyamat során. A teljes 4 TB-nyi tárhelyre azonban csak az aktuális adatbázisfájlok frissítése után az új maxsize nem használják. A frissítés alatt álló adatbázis méretétől függ szükséges idő hosszát.  
- Létrehozásakor, vagy egy P11 vagy P15 adatbázis frissítése, csak választhat 1 TB-os és 4 TB-os maxsize között. Köztes tárolási méretek jelenleg nem támogatottak. Egy P11/P15 létrehozásakor az 1 TB-os alapértelmezett tárolási lehetőség az előre kijelölt. Az adatbázisok a támogatott régiók egyikéhez sem található a tároló maximális egy új vagy meglévő önálló adatbázis 4 TB-os növelhető. Minden más területen maximális mérete 1 TB-os fent nem növelhető. Az ár nem változtatja meg, ha 4 TB-nyi tárhelyre belefoglalt választja.
- A 4 TB-os adatbázis maxsize nem módosítható és 1 TB, akkor is, ha a ténylegesen használt tárhely 1 TB-os alatt van. Így, akkor nem visszaminősítését egy P11 - 4 TB-os/P15 - 4 TB-os egy P11 - 1 TB-os/P15 - 1 TB-os vagy egy alacsonyabb teljesítményszinttel például a P1-P6) addig, amíg azt további tárolási lehetőségeket a teljesítmény rétegek a többi ad. Ez a korlátozás vonatkozik a visszaállítási és is-időpontban, például másolás forgatókönyvek georedundáns helyreállítás, hosszú-távú biztonsági mentés-adatmegőrzés és az adatbázis másolása. Ha egy adatbázist a 4 TB-os beállítással van konfigurálva, az adatbázis összes visszaállítási műveletek a 4 TB-os maxsize P11/P15 be kell futnia.
- Ha létrehozása vagy frissítése egy P11/P15 adatbázis egy nem támogatott régióban, a létrehozás vagy frissítés művelet sikertelen a következő hiba miatt: **akár 4 TB-nyi tárhelyre P11 és P15 adatbázis érhetők el Nekünk East2, USA nyugati régiója Velünk – (kormányzati) Virginia, Nyugat Európa, Németország központi, Délkelet-Ázsia, kelet-japán, Kelet-Ausztrália, Kanada központi és Kanada keleti.**
- Aktív georeplikáció forgatókönyvek esetén:
   - A georeplikáció kapcsolat beállítása: Ha az elsődleges adatbázis P11 vagy P15, a secondary(ies) is kell P11 vagy P15; alacsonyabb teljesítményt rétegek visszautasítja másodlagos adatbázist, mert azok nem képes a 4 TB-os.
   - A georeplikáció kapcsolatban az elsődleges adatbázis frissítése: a maxsize módosítása a következőre: 4 TB-os elsődleges adatbázison váltja ki ezt a változtatást, a másodlagos adatbázison. Két frissítést a változtatás érvénybe léptetéséhez elsődleges sikeresnek kell lennie. A 4 TB-os beállítás régió korlátozások érvényesek, (lásd fent). Ha a másodlagos régióban, amely nem támogatja a 4 TB-os, az elsődleges nincs frissítve.
- Az Import/Export szolgáltatás feltöltését P11 - 4 TB-os/P15 - 4 TB-os adatbázisok használata nem támogatott. Használja a SqlPackage.exe [importálása](sql-database-import.md) és [exportálása](sql-database-export.md) adatokat.

## <a name="manage-single-database-service-tiers-and-performance-levels-using-the-azure-portal"></a>Önálló adatbázis szolgáltatásszintjei és teljesítményszintjei az Azure portál használatával kezelése

Állítsa be, vagy módosítsa a szolgáltatási rétegben, teljesítményszintet vagy tárterület új vagy meglévő Azure SQL-adatbázis az Azure portál használatával, nyissa meg a **konfigurálása a** ablakban kattintson az adatbázis **árképzési szint () méretezhető dtu-k)** – az alábbi képernyőfelvételen látható módon. 

- Vagy módosítsa a szolgáltatási rétegben a szolgáltatási rétegben, a munkaterhelés kiválasztásával. 
- Állítsa be vagy változtassa meg a teljesítményszintet (**dtu-k**) belül egy szolgáltatási réteg használatával a **DTU** csúszkát.
- Állítsa be, vagy módosítsa a teljesítmény szintű használó mennyisége a **tárolási** csúszkát. 

  ![Szolgáltatási szint és a teljesítmény szint konfigurálása](./media/sql-database-service-tiers/service-tier-performance-level.png)

> [!IMPORTANT]
> Felülvizsgálati [4 TB-os maxsize P11 és P15 adatbázisokkal aktuális korlátozásai](sql-database-service-tiers.md#current-limitations-of-p11-and-p15-databases-with-4-tb-maxsize) P11 vagy P15 szolgáltatásréteg kiválasztásakor.
>

## <a name="manage-single-database-service-tiers-and-performance-levels-using-powershell"></a>Önálló adatbázis szolgáltatásszintjei és teljesítményszintjei PowerShell-lel kezelése

A következő PowerShell-parancsmagok segítségével, vagy módosítsa a Azure SQL-adatbázisok szolgáltatásszintek, teljesítményszintjeivel és PowerShell használatával mennyisége. Ha szeretné telepíteni vagy frissíteni a PowerShell, lásd: [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps). 

| Parancsmag | Leírás |
| --- | --- |
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Létrehoz egy adatbázis |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Egy vagy több adatbázis beolvasása|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Az adatbázis tulajdonságainak megadása, vagy a meglévő adatbázis áthelyezése rugalmas készletbe|


> [!TIP]
> Egy PowerShell-példa parancsfájlt, amely figyeli a metrikák egy adatbázis méretezi a magasabb teljesítményszintre, és a metrikák egyik egy riasztási szabályt hoz létre a következő témakörben: [figyelő és a skála egyetlen SQL adatbázis-PowerShellhasználatával](scripts/sql-database-monitor-and-scale-database-powershell.md).

## <a name="manage-single-database-service-tiers-and-performance-levels-using-the-azure-cli"></a>Önálló adatbázis szolgáltatásszintjei és teljesítményszintjei az Azure parancssori felület használatával kezelése

Beállítása vagy módosítása az Azure SQL-adatbázisok szolgáltatási szinteket, teljesítményszintjeivel és tárterület az Azure parancssori felülettel, használja a következő [Azure CLI SQL Database](/cli/azure/sql/db) parancsok. A [Cloud Shell-lel](/azure/cloud-shell/overview) futtassa a parancssori felületet a böngészőben, vagy [telepítse](/cli/azure/install-azure-cli) macOS, Linux, illetve Windows rendszeren. Létrehozása és kezelése SQL rugalmas készletek: [rugalmas készletek](sql-database-elastic-pool.md).

| Parancsmag | Leírás |
| --- | --- |
|[az sql-adatbázis létrehozása](/cli/azure/sql/db#create) |Létrehoz egy adatbázis|
|[az sql db listája](/cli/azure/sql/db#list)|Felsorolja az összes adatbázist és az adatraktárak egy kiszolgálón, vagy minden adatbázisok rugalmas készlethez|
|[az sql db lista-verziók](/cli/azure/sql/db#list-editions)|Listák elérhető szolgáltatási célkitűzések és tárolási korlátai|
|[az sql db lista-módjait](/cli/azure/sql/db#list-usages)|Adatbázis-módjait beolvasása|
|[az sql db megjelenítése](/cli/azure/sql/db#show)|Egy adatbázis vagy adatraktár beolvasása|
|[az sql-adatbázis frissítése](/cli/azure/sql/db#update)|Frissíti az adatbázist|

> [!TIP]
> Az Azure parancssori felület példa parancsfájl, amely egy Azure SQL-adatbázis különböző teljesítményszintet is méretezi a méretre vonatkozó adatok az adatbázis lekérdezése után, lásd: [használata CLI figyelését, valamint egy SQL-adatbázis méretezése](scripts/sql-database-monitor-and-scale-database-cli.md).
>

## <a name="manage-single-database-service-tiers-and-performance-levels-using-transact-sql"></a>Önálló adatbázis szolgáltatásszintjei és teljesítményszintjei Transact-SQL használatával kezelése

Beállítása vagy módosítása az Azure SQL-adatbázisok szolgáltatási szinteket, teljesítményszintjeivel és tárterület a Transact-SQL, használja a következő T-SQL-parancsokat. Ezek a parancsok használata az Azure-portálon kiadhatja [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs), vagy bármely más programot, amely egy Azure SQL Database-kiszolgálóhoz csatlakozhat, és adja át a Transact-SQL-parancsokat. 

| Parancs | Leírás |
| --- | --- |
|[ADATBÁZIS (az Azure SQL Database) létrehozása](/sql/t-sql/statements/create-database-azure-sql-database)|Egy új adatbázist hoz létre. Kell csatlakoznia a főadatbázison való futtatásával hozzon létre egy új adatbázist.|
| [Az ALTER DATABASE (Azure SQL Database)](/sql/t-sql/statements/alter-database-azure-sql-database) |Azure SQL-adatbázis módosítása. |
|[sys.database_service_objectives (az Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Egy Azure SQL database vagy az Azure SQL Data Warehouse esetében adja vissza a edition (szolgáltatási réteg), a szolgáltatási cél (IP-címek) és a rugalmas készlet nevét. Ha be van jelentkezve a főadatbázishoz egy Azure SQL adatbázis-kiszolgáló, az összes adatbázis ad vissza adatokat. Az Azure SQL Data Warehouse kell csatlakoznia a fő adatbázist.|
|[sys.database_usage (az Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-usage-azure-sql-database)|Felsorolja a száma, típusa és az Azure SQL Database-kiszolgálón lévő adatbázis időtartama.|

A következő példa bemutatja a maxsize használja az ALTER DATABASE parancs a következőre változik:

 ```sql
ALTER DATABASE <myDatabaseName> 
   MODIFY (MAXSIZE = 4096 GB);
```

## <a name="manage-single-databases-using-the-rest-api"></a>A REST API használatával önálló adatbázisok kezelése

Állítsa be, vagy módosítsa az Azure SQL adatbázis szolgáltatásszintjeinek, teljesítményszintjeivel és mennyisége a REST API használatával, lásd: [Azure SQL Database REST API](/rest/api/sql/).

## <a name="next-steps"></a>Következő lépések

* További információ [dtu-k](sql-database-what-is-a-dtu.md).
* DTU-használatát figyelésével kapcsolatos további tudnivalókért lásd: [figyelés és a teljesítmény hangolására](sql-database-troubleshoot-performance.md).

