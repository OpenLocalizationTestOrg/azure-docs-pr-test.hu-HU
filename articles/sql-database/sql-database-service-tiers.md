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
ms.openlocfilehash: b27b78788614d32e6c0c4267fe0ce504ebd2f444
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-performance-options-are-available-for-an-azure-sql-database"></a>Milyen teljesítménybeállításai érhetők el az Azure SQL-adatbázis

[Az Azure SQL Database](sql-database-technical-overview.md) négy szolgáltatásszintek mindkét egyetlen kínál és [készletezett](sql-database-elastic-pool.md) adatbázisok. A szolgáltatási rétegekkel vannak: **alapvető**, **szabványos**, **prémium**, és **prémium RS**. Egyes szolgáltatásszinteken belül több teljesítményszint vannak ([dtu-k](sql-database-what-is-a-dtu.md)) és a tárolási beállítások toohandle különböző számítási feladatok és az adatok méretét. Magasabb teljesítményszintet biztosít további számítási és tárolási erőforrások tervezett toodeliver egyre nagyobb átviteli teljesítményt és kapacitást. Szolgáltatási szinteket, teljesítményszintjeivel és tárterületének leállás nélküli dinamikusan módosíthatja. 
- **Alapszintű**, **szabványos** és **prémium** összes szolgáltatásszintek szolgáltatásiszint-szerződés 99.99 %, rugalmas üzletmenet-folytonossági funkciókat, biztonsági szolgáltatásokat és óradíj alapú számlázást nyújtanak hasznos üzemidőt rendelkezik. 
- Hello **prémium RS** réteg nyújt hello azonos teljesítményszintet hello prémium csomagban csökkentett SLA-t, mivel redundáns másolatok alacsonyabb több mint egy adatbázis fut hello más szolgáltatási szinteket. Ezért hello esemény a szolgáltatás hibák, lehetséges, hogy kell toorecover, az adatbázis egy biztonsági másolatból való mentése tooa 5 perces késéssel.

> [!IMPORTANT]
> Azure SQL-adatbázis erőforrásainak garantált készletét kap, és hello várt teljesítményt nyújt az adatbázis nem érinti a bármely más adatbázis Azure-ban. 

## <a name="choosing-a-service-tier"></a>Szolgáltatásszint kiválasztása
hello következő táblázat példákat hello rétegek ajánlott olyan másik alkalmazás igénylő munkaterhelésekhez ajánlott.

| Szolgáltatásszint | Kívánt teljesítményprofilok |
| :--- | --- |
| **Basic** | Leginkább kis adatbázisokhoz felel meg, egyidejűleg általában egyetlen aktív műveletet támogat. Alkalmas például fejlesztési vagy tesztelési célokra használt adatbázisokhoz vagy kisméretű, ritkán használt alkalmazásokhoz. |
| **Standard** |hello Ugrás-toooption alacsony toomedium IO teljesítmény-követelményekkel rendelkező, több egyidejű lekérdezéseket támogató felhőalapú alkalmazásokhoz. Alkalmas például munkacsoportokhoz vagy webalkalmazásokhoz. |
| **Prémium** | Nagy mennyiségű, nagy I/O-teljesítményigényű tranzakcióhoz tervezve, számos egyidejű felhasználót támogat. Alkalmas például az üzletmenet szempontjából kritikus alkalmazásokat támogató adatbázisokhoz. |
| **Prémium szintű RS** | Tervezett IO-igényes munkaterhelések esetén, amelyek nem igényelnek hello legmagasabb rendelkezésre állását garantálja. Például nagy teljesítményű munkaterhelések, vagy egy analitikai munkaterhelés, ahol hello adatbázis nincs rekord hello rendszer tesztelése. |
|||

Dedikált erőforrások belül egy adott szolgáltatásréteg hozhat létre önálló adatbázisok [teljesítményszintet](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) vagy adatbázisok belül is létrehozhat egy [SQL rugalmas készlet](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus). SQL rugalmas készletben hello számítási és tárolási erőforrások több adatbázis egyetlen logikai kiszolgáló között vannak megosztva. 

Forrásanyag is elérhető az önálló adatbázisok adatbázis-tranzakciós egységek (dtu-k) vannak kifejezve, és egy SQL rugalmas készletek erőforrásait rugalmas adatbázis-tranzakciós egységek (edtu-k) vannak kifejezve. Dtu és edtu-k kapcsolatban bővebben lásd: [Dtu és edtu-k?](sql-database-what-is-a-dtu.md).

toodecide meghatározása hello minimális adatbázis funkciókra van szüksége egy szolgáltatási réteg elindításához:

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
> Tárolási mentése too4 TB érhető el jelenleg a következő régiókban hello: US East2, USA nyugati régiója, Velünk – (kormányzati) Virginia, Nyugat-Európa, Németország központi, Dél Kelet-Ázsiában, kelet-japán, Kelet-Ausztrália, Kanada központi és Kanada keleti. Lásd: [jelenlegi 4 TB-os korlátozásai](sql-database-service-tiers.md#current-limitations-of-p11-and-p15-databases-with-4-tb-maxsize)
>

Miután megadta, hogy hello megfelelő szolgáltatási rétegben, áll készen toodetermine hello teljesítményszintet (dtu-inak száma hello) és hello adatbázis hello mennyisége. 

- hello S2 és S3 teljesítményszintet hello **szabványos** réteg általában jó kiindulási pont. 
- Olyan adatbázisok esetén magas CPU, vagy az I/O követelmények, a hello teljesítményszintet hello **prémium** réteg hello megfelelő kiindulási pontot. 
- Hello **prémium** kínál, több Processzor- és további I/O x 10 kezdődik képest toohello hello a legmagasabb teljesítményszintet **szabványos** réteg.
- Hello **PremiumRS** kínál, hello hello teljesítményének **prémium** réteg alacsonyabb költségekkel, de a csökkentett SLA-t.

> [!IMPORTANT]
> Felülvizsgálati hello [SQL rugalmas készletek](sql-database-elastic-pool.md) SQL rugalmas adatbázisok csoportosításával hello információt a témakör a készletbe tooshare számítási és tárolási erőforrásokat. Ez a témakör további része hello szolgáltatásszintek és teljesítményszintek az önálló adatbázisok összpontosít.
>

## <a name="single-database-service-tiers-and-performance-levels"></a>Önálló adatbázis szolgáltatásszintjei és teljesítményszintjei
Az önálló adatbázisok vannak több teljesítményszintet és tárolási összegek egyes szolgáltatásszinteken belül. 

[!INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

## <a name="scaling-up-or-scaling-down-a-single-database"></a>Önálló adatbázis vertikális fel- és leskálázása

Miután kiválasztotta a kezdeti szolgáltatásszintet és teljesítményszintet, a tényleges tapasztalatok alapján dinamikusan fel- vagy leskálázhatja az önálló adatbázist.  

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]
>

Hello szolgáltatás réteg és/vagy a teljesítmény-adatbázis szintjének módosítása alkalmazás egy replikát készít hello eredeti adatbázis szinten hello új teljesítményét, és kapcsolatok majd átvált toohello replika. Adatok nem vesztek el a folyamat során, de során hello rövid jelenleg amikor azt vált át toohello replika, kapcsolatok toohello adatbázis le van tiltva, ezért néhány útban is lehet tranzakciók visszaállítása. hello mennyi ideig hello kell váltania a változó, de általában a 4 másodperc érték kisebb, mint 30 másodperc hello idő 99 %. Ha nagyszámú: hello jelenleg kapcsolatok útban tranzakciók le vannak tiltva, akkor hello hello kell váltania időtartamot is hosszabb lehet.  

hello teljes méretezett folyamat időtartamának hello mindkét hello méretétől függ, és a szolgáltatási szint hello adatbázis-előtt és után hello módosítása. Például egy 250 GB-os adatbázis esetén, amelyet a Standard szolgáltatásszintről/szolgáltatásszintre vagy a Standard szolgáltatásszinten belül módosítunk, legfeljebb 6 órát vehet igénybe. Hello-adatbázis azonos méretezés, amely változó teljesítményszintet belül hello prémium szolgáltatásszintet, 3 órán belül kell végrehajtani.

> [!TIP]
> toocheck hello állapotát egy folyamatban lévő művelet méretezés SQL-adatbázis, használhatja a következő lekérdezés hello: ```select * from sys.dm_operation_status```.
>

* Tooa magasabb réteg vagy teljesítményének szolgáltatásszint frissít, hello maximális adatbázis mérete nem növeli hacsak Ön kifejezetten megad egy nagyobb maximális méretét.
* egy adatbázis toodowngrade, hello adatbázis hello maximálisan megengedett méretet hello cél szolgáltatásréteg kisebbnek kell lennie. 
* Ha az adatbázis frissítése [georeplikáció](sql-database-geo-replication-portal.md) engedélyezve van, frissítse a másodlagos adatbázisok toohello kívánt teljesítményszinttel csak azután frissítse hello elsődleges adatbázis (általános útmutatást). Különböző tooa frissítése edition ugrading hello másodlagos adatbázis először szükség. 
* Amikor alacsonyabb verziójúra változtatása adatbázis [georeplikáció](sql-database-geo-replication-portal.md) az elsődleges adatbázis toohello kívánt teljesítményszinttel engedélyezve van, alacsonyabb előtt alacsonyabb verziójúra változtatása hello másodlagos adatbázis (általános útmutatást). Alacsonyabb verziójúra változtatása különböző tooa edition visszaminősítés hello elsődleges adatbázis először szükség. 

* hello visszaállítási szolgáltatásajánlatok különbözőek a különböző szolgáltatási szinteket hello. Ha toohello alacsonyabb verziójúra változtatása **alapvető** réteg, hogy alacsonyabb biztonsági mentés megőrzési időtartamot - lásd [Azure SQL adatbázis biztonsági másolatait](sql-database-automated-backups.md).
* hello új adatbázis tulajdonságainak megadása hello nem érvényesek, amíg a hello módosítások nem teljes.


## <a name="current-limitations-of-p11-and-p15-databases-with-4-tb-maxsize"></a>4 TB-os maxsize P11 és P15 adatbázisokkal aktuális korlátozásai

A maximális mérete 4 TB-os P11 és P15 adatbázis támogatott egyes régiókban (előbb tárgyalt módon). hello következő szempontokat és korlátozások vonatkoznak tooP11 és 4 TB-os maxsize P15 adatbázisokkal:

- Ha egy adatbázist (értéke 4 TB-os vagy 4096 GB) létrehozásához hello 4 TB-os maxsize lehetőséget választja, hello parancs sikertelen hiba hozzon létre Ha hello adatbázis egy nem támogatott régióban lett beállítva.
- A már létező P11 és P15 adatbázisok hello támogatott régiók egyikéhez sem található hello maxsize tárolási too4 TB növelhető. Ez ellenőrizhető hello segítségével [válasszon DATABASEPROPERTYEX](https://msdn.microsoft.com/library/ms186823.aspx) vagy ellenőrizze az adatbázis mérete hello hello Azure-portálon. Egy meglévő P11 vagy P15 frissítése adatbázis csak hajtható végre egy kiszolgálószintű fő bejelentkezéssel vagy hello dbmanager adatbázis-szerepkör tagjai. 
- Ha a frissítési művelet végrehajtása egy támogatott régióban hello konfigurációja azonnal frissül. hello adatbázis online állapotban marad, hello frissítési folyamat során. Azonban nem használhatja a teljes hello 4 TB-nyi tárhelyre, amíg meg nem hello tényleges adatbázisfájlok toohello új maxsize frissítve. a frissítés alatt hello adatbázis mérete hello függ hello mennyi idő szükséges.  
- Létrehozásakor, vagy egy P11 vagy P15 adatbázis frissítése, csak választhat 1 TB-os és 4 TB-os maxsize között. Köztes tárolási méretek jelenleg nem támogatottak. Egy P11/P15 létrehozásakor hello alapértelmezett tárolási lehetőség 1 TB-os előre kiválasztott. Az adatbázisok hello támogatott régiók egyikéhez sem található növelheti a hello tároló maximális too4TB egy új vagy meglévő adatbázis esetén. Minden más területen maximális mérete 1 TB-os fent nem növelhető. hello ár nem változtatja meg, ha 4 TB-nyi tárhelyre belefoglalt választja.
- hello 4 TB-os adatbázis maxsize nem lehet módosított too1 TB akkor is, ha hello tényleges tárolási használt 1 TB-os alatt van. Ebből kifolyólag nem visszaminősítését egy P11 - 4 TB-os/P15 - 4 TB-os tooa P11 - 1 TB-os/P15 - 1 TB-os vagy egy alacsonyabb teljesítményszinttel például tooP1 P6) addig, amíg azt ad hello hello részeinek további tárolási lehetőséget teljesítmény rétegek. Ez a korlátozás érvényes toohello visszaállítási és -időpontban, például másolás forgatókönyvek georedundáns helyreállítás, hosszú-távú biztonsági mentés-adatmegőrzés és az adatbázis másolása. Miután egy adatbázis hello 4 TB-os beállítással van konfigurálva, az adatbázis összes visszaállítási műveletek a 4 TB-os maxsize P11/P15 be kell futnia.
- Ha létrehozásakor vagy egy P11/P15 adatbázis frissítése nem támogatott régióban hello létrehozása vagy frissítési művelet sikertelen, és a következő hibaüzenet hello: **P11 és P15 adatbázis másolatot tároló too4TB érhetők el Nekünk East2, USA nyugati régiója, Velünk – (kormányzati) Virginia Nyugat-Európában, Németország központi, Délkelet-Ázsia, kelet-japán, Kelet-Ausztrália, Kanada központi és Kanada keleti.**
- Aktív georeplikáció forgatókönyvek esetén:
   - A georeplikáció kapcsolat beállítása: Ha hello elsődleges adatbázis P11 vagy P15, hello secondary(ies) is kell P11 vagy P15; alacsonyabb teljesítményt rétegek visszautasítja másodlagos adatbázist, mert azok nem képes a 4 TB-os.
   - Frissítés hello elsődleges adatbázis georeplikáció kapcsolatban: hello maxsize too4 TB az elsődleges adatbázis módosítása elindítja a hello azonos hello másodlagos adatbázison módosítása. Két frissítést hello módosítás hello elsődleges tootake hatása a sikeresnek kell lennie. Hello 4 TB-os beállítás régió korlátozások érvényesek, (lásd fent). Ha másodlagos hello egy régióban található, amely nem támogatja a 4 TB-os, elsődleges hello nincs frissítve.
- P11 - 4 TB-os/P15 - 4 TB-os adatbázisok feltöltését hello Import/Export szolgáltatás használata nem támogatott. Használjon túl SqlPackage.exe[importálása](sql-database-import.md) és [exportálása](sql-database-export.md) adatokat.

## <a name="manage-single-database-service-tiers-and-performance-levels-using-hello-azure-portal"></a>Önálló adatbázis szolgáltatásszintjei és teljesítményszintjei hello Azure-portál használatával kezelése

tooset, vagy módosítsa a hello szolgáltatási rétegben, teljesítményszintet vagy egy új vagy meglévő Azure SQL-adatbázis használatával hello Azure-portálon nyitott hello mennyisége **konfigurálása a** ablakban kattintson az adatbázis  **Tarifacsomag (skála dtu-k)** – ahogy az alábbi képernyőfelvétel a hello. 

- Vagy módosítsa a hello szolgáltatásréteg hello szolgáltatási rétegben a munkaterheléshez kiválasztásával. 
- Beállítása vagy módosítása hello teljesítményszintet (**dtu-k**) belül egy szolgáltatási réteg hello segítségével **DTU** csúszkát.
- Beállítása vagy módosítása hello mennyisége hello teljesítményszintet hello segítségével a **tárolási** csúszkát. 

  ![Szolgáltatási szint és a teljesítmény szint konfigurálása](./media/sql-database-service-tiers/service-tier-performance-level.png)

> [!IMPORTANT]
> Felülvizsgálati [4 TB-os maxsize P11 és P15 adatbázisokkal aktuális korlátozásai](sql-database-service-tiers.md#current-limitations-of-p11-and-p15-databases-with-4-tb-maxsize) P11 vagy P15 szolgáltatásréteg kiválasztásakor.
>

## <a name="manage-single-database-service-tiers-and-performance-levels-using-powershell"></a>Önálló adatbázis szolgáltatásszintjei és teljesítményszintjei PowerShell-lel kezelése

tooset, vagy módosítsa Azure SQL adatbázis szolgáltatásszintjeinek, teljesítményszintjeivel és tárterület powershellel, használja a következő PowerShell-parancsmagok hello. Ha tooinstall kell, vagy PowerShell frissítése, lásd: [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps). 

| Parancsmag | Leírás |
| --- | --- |
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Létrehoz egy adatbázis |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Egy vagy több adatbázis beolvasása|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Az adatbázis tulajdonságainak megadása, vagy a meglévő adatbázis áthelyezése rugalmas készletbe|


> [!TIP]
> Egy PowerShell-példa parancsfájlt, amely figyeli a hello teljesítménymutatók egy adatbázis tooa magasabb teljesítményszintre méretezi azt, és egy riasztási szabályt hoz létre egy hello teljesítménymutatók, a következő témakörben: [figyelő és a skála egyetlen SQL-adatbázis használatával PowerShell](scripts/sql-database-monitor-and-scale-database-powershell.md).

## <a name="manage-single-database-service-tiers-and-performance-levels-using-hello-azure-cli"></a>Önálló adatbázis szolgáltatásszintjei és teljesítményszintjei hello Azure parancssori felület használatával kezelése

tooset, vagy módosítsa Azure SQL adatbázis szolgáltatásszintjeinek, teljesítményszintjeivel és hello Azure parancssori felület használatával mennyisége használja hello következő [Azure CLI SQL Database](/cli/azure/sql/db) parancsok. Használjon hello [felhő rendszerhéj](/azure/cloud-shell/overview) toorun hello CLI-t a böngészőben vagy [telepítése](/cli/azure/install-azure-cli) azt macOS, Linux vagy a Windows. Létrehozása és kezelése SQL rugalmas készletek: [rugalmas készletek](sql-database-elastic-pool.md).

| Parancsmag | Leírás |
| --- | --- |
|[az sql-adatbázis létrehozása](/cli/azure/sql/db#create) |Létrehoz egy adatbázis|
|[az sql db listája](/cli/azure/sql/db#list)|Felsorolja az összes adatbázist és az adatraktárak egy kiszolgálón, vagy minden adatbázisok rugalmas készlethez|
|[az sql db lista-verziók](/cli/azure/sql/db#list-editions)|Listák elérhető szolgáltatási célkitűzések és tárolási korlátai|
|[az sql db lista-módjait](/cli/azure/sql/db#list-usages)|Adatbázis-módjait beolvasása|
|[az sql db megjelenítése](/cli/azure/sql/db#show)|Egy adatbázis vagy adatraktár beolvasása|
|[az sql-adatbázis frissítése](/cli/azure/sql/db#update)|Frissíti az adatbázist|

> [!TIP]
> Az Azure parancssori felület példa parancsfájl, amely az Azure SQL adatbázis tooa más-más teljesítménybeli egyszintű méretezi után hello információi hello adatbázis lekérdezésére, lásd: [használata CLI toomonitor és a skála egy SQL-adatbázis](scripts/sql-database-monitor-and-scale-database-cli.md).
>

## <a name="manage-single-database-service-tiers-and-performance-levels-using-transact-sql"></a>Önálló adatbázis szolgáltatásszintjei és teljesítményszintjei Transact-SQL használatával kezelése

tooset, vagy módosítsa Azure SQL adatbázis szolgáltatásszintjei, teljesítményszintjeivel és tárterület a Transact-SQL, használja a következő T-SQL parancsokkal hello. Ezek a parancsok használata Azure-portálon hello kiadhatja [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs), vagy bármely más programot, amely tooan Azure SQL Database-kiszolgálóhoz csatlakozhat, és adja át a Transact-SQL parancsok. 

| Parancs | Leírás |
| --- | --- |
|[ADATBÁZIS (az Azure SQL Database) létrehozása](/sql/t-sql/statements/create-database-azure-sql-database)|Egy új adatbázist hoz létre. Csatlakoztatott toohello főadatbázis toocreate egy új adatbázist kell lennie.|
| [Az ALTER DATABASE (Azure SQL Database)](/sql/t-sql/statements/alter-database-azure-sql-database) |Azure SQL-adatbázis módosítása. |
|[sys.database_service_objectives (az Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Értéket ad vissza a edition (szolgáltatási réteg), a szolgáltatási cél (IP-címek) és a rugalmas készlet nevét, ha van egy Azure SQL database vagy az Azure SQL Data Warehouse hello. Egy Azure SQL adatbázis-kiszolgáló toohello főadatbázis jelentkezik be, az összes adatbázis ad vissza adatokat. Az Azure SQL Data Warehouse csatlakoztatott toohello master adatbázisban kell lennie.|
|[sys.database_usage (az Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-usage-azure-sql-database)|Hello száma, típusa és időtartama az Azure SQL Database-kiszolgálón lévő adatbázis sorolja fel.|

hello alábbi példa bemutatja hello maxsize hello ALTER DATABASE parancs használatával a következőre változik:

 ```sql
ALTER DATABASE <myDatabaseName> 
   MODIFY (MAXSIZE = 4096 GB);
```

## <a name="manage-single-databases-using-hello-rest-api"></a>Hello REST API használatával önálló adatbázisok kezelése

tooset, vagy módosítsa Azure SQL adatbázis szolgáltatásszintjeinek, teljesítményszintet és hello REST API-t használó mennyisége [Azure SQL Database REST API](/rest/api/sql/).

## <a name="next-steps"></a>Következő lépések

* További információ [dtu-k](sql-database-what-is-a-dtu.md).
* DTU-használatát, figyelésével kapcsolatos toolearn lásd [figyelés és a teljesítmény hangolására](sql-database-troubleshoot-performance.md).

