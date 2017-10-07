---
title: "aaaMigrate SQL Server-adatbázis tooAzure SQL-adatbázis |} Microsoft Docs"
description: "Ismerje meg, az SQL Server adatbázis tooAzure SQL-adatbázis toomigrate."
services: sql-database
documentationcenter: 
author: janeng
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,load & move data
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 06/27/2017
ms.author: janeng
ms.openlocfilehash: d10ad1d26576194f1dd6858bae5c3e7c1ec4fb91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-sql-server-database-tooazure-sql-database"></a>Az SQL Server adatbázis tooAzure SQL-adatbázis áttelepítése

Helyezze át az SQL Server adatbázis tooAzure SQL-adatbázis három részből álló folyamat - előkészítenie, majd exportálja, és importálja a hello adatbázis. Ebben az oktatóanyagban elsajátíthatja, hogy:

> [!div class="checklist"]
> * Egy SQL Server adatbázis előkészítése áttelepítési tooAzure SQL-adatbázis használatával hello [adatok áttelepítési Segéd](https://www.microsoft.com/download/details.aspx?id=53595) (DMA)
> * Hello adatbázis tooa BACPAC fájl exportálása
> * Hello BACPAC fájlt importálja az Azure SQL adatbázis

Ha nem rendelkezik Azure-előfizetéssel, [ingyenes fiók létrehozását](https://azure.microsoft.com/free/) megkezdése előtt.

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban a következő előfeltételek győződjön meg arról, hogy hello elvégzése toocomplete:

- Telepített hello legújabb verziójának [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS). SSMS telepítője hello SQLPackage, egy parancssori segédprogram, amely használt tooautomate adatbázis-fejlesztési feladatok számos legújabb verzióját is telepíti. 
- Telepített hello [adatok áttelepítési Segéd](https://www.microsoft.com/download/details.aspx?id=53595) (DMA-).
- Azonosította, és hozzáférést tooa adatbázis toomigrate rendelkezik. Ez az oktatóanyag használja hello [SQL Server 2008R2 AdventureWorks OLTP adatbázis](https://msftdbprodsamples.codeplex.com/releases/view/59211) egy példány SQL Server 2008R2 vagy újabb, de használhatja az Ön által választott bármely adatbázis. kompatibilitási problémák toofix, használjon [SQL Server Data Tools](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt)

## <a name="prepare-for-migration"></a>Felkészülés az áttelepítésre

Az áttelepítéshez készen tooprepare áll. Kövesse az alábbi lépéseket toouse hello  **[adatok áttelepítési Segéd](https://www.microsoft.com/download/details.aspx?id=53595)**  tooassess hello készültségi áttelepítési tooAzure SQL-adatbázis az adatbázis.

1. Nyissa meg hello **adatok áttelepítési Segéd**. Minden olyan számítógépen, a kapcsolat toohello SQL Server példányt tartalmazó hello adatbázis DMA futtathatja, hogy toomigrate tervezi, nincs szükség a hello eszközt futtató számítógépen az SQL Server-példány hello tooinstall.

     ![Nyissa meg az adatok áttelepítési Segéd](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-open.png)

2. Hello bal oldali menüben kattintson a **+ új** toocreate egy **Assessment** projekt. Töltse ki a képernyőn hello egy **projekt neve** (minden más értéket kell balra esetén az alapértelmezett értékek), majd **létrehozása**. Hello **beállítások** lap megnyitásakor.

     ![új adatok áttelepítési Segéd projekt](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-new-project.png)

3. A hello **beállítások** kattintson **következő**. Hello **válassza ki az adatforrások** lap megnyitásakor.

     ![új adatok áttelepítési lehetőségek](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-options.png) 

4. A hello **válassza ki az adatforrások** lapján adja meg azt tervezi, hogy toomigrate hello kiszolgálót tartalmazó SQL Server-példány hello nevét. Változás hello más értékek ezen a lapon, ha szükséges. Kattintson a **Connect** (Csatlakozás) gombra.

     ![új áttelepítési válassza adatforrások](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-sources.png)

5. A hello **források hozzáadását** hello része **válassza ki az adatforrások** lapon, jelölje be a hello adatbázisok toobe kompatibilitás tesztelt hello jelölőnégyzeteket. Kattintson az **Add** (Hozzáadás) parancsra.

     ![új áttelepítési válassza adatforrások](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-sources-add.png)

6. Kattintson a **Start Assessment**.

     ![új adatok áttelepítési start értékelése](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-start-assessment.png)

7. Hello assessment befejezését követően először keresse meg toosee elég kompatibilis toomigrate hello adatbázis esetén. Keresse meg a zöld kör hello jelölve.

     ![új adatok áttelepítési értékelési eredmények kompatibilis](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-assessment-results-compatible.png)

8. Tekintse át a hello eredményeket. Hello **SQL Server szolgáltatásparitást** megjelenített eredmények hello alapértelmezett eredmények tooreview. Kifejezetten át hello nem támogatott és részben támogatott szolgáltatásokkal kapcsolatos, valamint megadott hello kapcsolatos műveleteket. 

     ![új adatok áttelepítési assessment paritás](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-assessment-results-parity.png)

9. Felülvizsgálati hello **kompatibilitási problémák** hello bal felső adott elemére kattintva. Kifejezetten felülvizsgálati hello áttelepítési blockers, viselkedésváltozások, információt, és elavult funkciók minden kompatibilitási szinten. Hello AdventureWorks2008R2 adatbázishoz, tekintse át a hello módosításokat tooFull szöveges keresést az SQL Server 2008 és hello óta tooSERVERPROPERTY('LCID') változtatásokat az SQL Server 2000. További információkért ezekről a változásokról hivatkozások további információkat valósul meg. Számos keresési beállításokat, és a teljes szöveges keresés beállításai lettek módosítva 

   > [!IMPORTANT] 
   > Az adatbázis tooAzure SQL-adatbázis az áttelepítés után dönthet úgy, hogy a jelenlegi kompatibilitási szinten (100. szint hello AdventureWorks2008R2 adatbázis) vagy magasabb szintű toooperate hello adatbázis ki. Hello hatással vannak, és egy adatbázis kompatibilitási szintű működő beállításainak további információkért lásd: [adatbázis kompatibilitási szintje ALTER](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level). Lásd még: [adatbázis HATÓKÖRŰ konfiguráció ALTER](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) további adatbázis-szintű beállítással kapcsolatos információkat a vonatkozó toocompatibility szintjének.
   >

10. Ha úgy gondolja, a **jelentés exportálása** toosave hello jelentés JSON-fájlként.
11. Adatok áttelepítése Segéd Bezárás hello.

## <a name="export-toobacpac-file"></a>TooBACPAC fájl exportálása 

Egy BACPAC egy ZIP-fájlt tartalmazó hello metaadatok és adatainak áttelepítését egy SQL Server-adatbázis BACPAC kiterjesztésű fájl. BACPAC fájl tárolhatók az Azure blob storage vagy a helyi tároló tartalmaznak, vagy az áttelepítés – többek között az SQL Server tooAzure SQL-adatbázis. Az Exportálás toobe tranzakciós úton konzisztens, gondoskodnia kell róla, hogy nincs írási tevékenység hello exportálás során jelentkezik.

Kövesse a lépéseket toouse hello SQLPackage parancssori segédprogram tooexport hello AdventureWorks2008R2 toolocal adatbázistár.

1. Nyisson meg egy Windows parancssort, és módosíthatja a directory tooa mappát, amelyből hello van **130** verzió SQLPackage – például **C:\Program Files (x86) \Microsoft SQL Server\130\DAC\bin**.

2. Hajtható végre a következő SQLPackage parancsot a következő hello parancssor tooexport hello hello **AdventureWorks2008R2** adatbázist **localhost** túl**AdventureWorks2008R2.bacpac**. Megfelelő tooyour környezet, ezeket az értékeket módosíthatja.

    ```SQLPackage
    sqlpackage.exe /Action:Export /ssn:localhost /sdn:AdventureWorks2008R2 /tf:AdventureWorks2008R2.bacpac
    ```

    ![sqlpackage exportálása](./media/sql-database-migrate-your-sql-server-database/sqlpackage-export.png)

Hello végrehajtásának befejezése után létrehozott hello BACPAC fájl hello könyvtárra hello sqlpackage végrehajtható tárolódik. Ebben a példában C:\Program Files (x86) \Microsoft SQL Server\130\DAC\bin. 

## <a name="log-in-toohello-azure-portal"></a>Jelentkezzen be toohello Azure-portálon

Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/). Hello tűzfalszabály létrehozása hello hello számítógépről, amelyen futtatja hello SQLPackage parancssori segédprogram jelentkezik be megkönnyíti az 5.

## <a name="create-a-sql-server-logical-server"></a>SQL server logikai kiszolgáló létrehozása

A [SQL server logikai kiszolgáló](sql-database-features.md) több adatbázis központi felügyeleti pontként működik. Kövesse az alábbi lépéseket egy SQL server logikai kiszolgáló toocontain hello toocreate át az Adventure Works OLTP SQL Server-adatbázist. 

1. Kattintson a hello **új** hello bal felső sarkában hello Azure-portálon található gombra.

2. Típus **sql server** a hello keresési ablak hello **új** lapon, és válassza ki **SQL-adatbázis (logikai kiszolgáló)** hello a szűrt listája.

    ![logikai kiszolgáló kiválasztása](./media/sql-database-migrate-your-sql-server-database/logical-server.png)

3. A hello **mindent** lapján kattintson **SQL-kiszolgáló (logikai kiszolgáló)** , majd **létrehozása** a hello **SQL Server (a logikai kiszolgáló)** lap .

    ![logikai kiszolgáló létrehozása](./media/sql-database-migrate-your-sql-server-database/logical-server-create.png)

4. Hello SQL server (a logikai kiszolgáló) űrlap kitöltése a következő információ, hello kép megelőző hello szerint:     

   | Beállítás       | Ajánlott érték | Leírás | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Kiszolgálónév** | Adja meg a globálisan egyedi neve | Az érvényes kiszolgálónevekkel kapcsolatban lásd az [elnevezési szabályokat és korlátozásokat](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) ismertető cikket. | 
   | **Kiszolgálói rendszergazdai bejelentkezés** | Adjon meg érvényes nevet a | Az érvényes bejelentkezési nevekkel kapcsolatban lásd az [adatbázis-azonosítókat](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers) ismertető cikket. |
   | **Jelszó** | Adjon meg egyetlen érvényes jelszót | A jelszó legalább 8 karakterből kell állnia, és a következő kategóriák hello hármat tartalmaznia kell: nagybetűk, kisbetűk, számok és nem alfanumerikus karakterek száma. |
   | **Előfizetés** | Előfizetés kiválasztása | Az előfizetései részleteivel kapcsolatban lásd az [előfizetéseket](https://account.windowsazure.com/Subscriptions) ismertető cikket. |
   | **Erőforráscsoport** | Válasszon ki egy meglévő erőforráscsoportot, vagy hozzon létre egy új csoportot, például a **myResourceGroup** |  Az érvényes erőforráscsoport-nevekkel kapcsolatban lásd az [elnevezési szabályokat és korlátozásokat](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) ismertető cikket. |
   | **Hely** | Adja meg az egyik érvényes helyen sem hello új kiszolgáló | A régiókkal kapcsolatos információkért lásd [az Azure régióit](https://azure.microsoft.com/regions/) ismertető cikket. |

   ![befejeződött a logikai kiszolgáló űrlap létrehozása](./media/sql-database-migrate-your-sql-server-database/logical-server-create-completed.png)

5. Kattintson a **létrehozása** tooprovision hello logikai kiszolgáló. Az üzembe helyezés eltarthat néhány percig. 

> [!IMPORTANT]
> Ne felejtse el, a kiszolgáló nevét, a rendszergazdai bejelentkezési nevet és a jelszó. Az oktatóanyag későbbi részében szüksége ezeket az értékeket.
>

## <a name="create-a-server-level-firewall-rule"></a>Kiszolgálószintű tűzfalszabály létrehozása

SQL Database szolgáltatás hello létrehoz egy [hello kiszolgálószintű tűzfal](sql-database-firewall-configure.md) , amely megakadályozza, hogy a külső alkalmazások és eszközök toohello kiszolgáló vagy hello kiszolgálón lévő összes adatbázis csatlakozzon, kivéve, ha egy tűzfalszabály tooopen hello tűzfal adott IP-címekhez. Kövesse ezeket a lépéseket toocreate egy SQL-adatbázis kiszolgálószintű tűzfalszabályt hello IP-címe, amelyből hello SQLPackage parancssori segédprogram futtató hello számítógépen. Ez lehetővé teszi SQLPackage tooconnect toohello SQL serverDatabase logikai kiszolgáló hello Azure SQL Database tűzfalon keresztül. 

1. Kattintson a **összes erőforrás** hello bal oldali menüben kattintson az új kiszolgáló hello a **összes erőforrás** lap. a kiszolgálóhoz tartozó hello – Áttekintés lapon nyílik, és további konfigurációs lehetőségeket.

     ![a logikai kiszolgáló – áttekintés](./media/sql-database-migrate-your-sql-server-database/logical-server-overview.png)

2. Kattintson a **tűzfal** hello bal oldali menü alatti **beállítások** hello áttekintése lapon. Hello **tűzfalbeállítások** hello SQL Database-kiszolgálóhoz tartozó lapon nyílik meg. 

3. Kattintson a **ügyfél IP-cím hozzáadása** hello eszköztár tooadd hello IP-cím hello számítógép jelenleg használ, és kattintson a **mentése**. Az IP-cím egy kiszolgálószintű tűzfalszabályt jön létre.

     ![kiszolgálótűzfal-szabály beállítása](./media/sql-database-migrate-your-sql-server-database/server-firewall-rule-set.png)

4. Kattintson az **OK** gombra.

Csatlakoztathatja az IP-címről hello kiszolgáló rendszergazdai fiókjának korábban létrehozott használatával a kiválasztott SQL Server Management Studio vagy egy másik használatával lévő tooall adatbázis.

> [!NOTE]
> Az SQL Database az 1433-as porton kommunikál. Ha a vállalati hálózatból származó tooconnect, a hálózati tűzfal előfordulhat, hogy nem engedélyezett a 1433-as port kimenő forgalmát. Ha igen, kivéve, ha az IT-részleg megnyitja az 1433-as port tooyour Azure SQL adatbázis-kiszolgáló nem lehet csatlakoztatni.
>

## <a name="import-a-bacpac-file-tooazure-sql-database"></a>Importálja a BACPAC fájl tooAzure SQL-adatbázis 

hello legújabb hello SQLPackage parancssori segédprogram támogatást nyújt az Azure SQL-adatbázis létrehozása meglétét a megadott [és teljesítményszintet szolgáltatás](sql-database-service-tiers.md). A legjobb teljesítmény importálás során jelöljön ki egy magas szintű szolgáltatást és teljesítményszintet szintet, és ezután csökkentheti az importálás után ha hello szolgáltatást és teljesítményszintet szintje magasabb, mint azonnal van szüksége.

Kövesse a lépéseket használata hello SQLPackage parancssori segédprogram tooimport hello AdventureWorks2008R2 adatbázis tooAzure SQL-adatbázis. A feladat végrehajtásához használhatja az SQL Server Management Studio, a SQLPackage hello előnyben részesített módszer a legtöbb éles környezetben a legnagyobb rugalmasságot és a lehető legjobb teljesítményt. Lásd: [áttelepítése az SQL Server tooAzure SQL-adatbázis BACPAC-fájlokat használ](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).

- Hajtható végre a következő SQLPackage parancsot a következő hello parancssor tooimport hello hello **AdventureWorks2008R2** adatbázis-, korábban létrehozott tooa új adatbázis, a szolgáltatás a helyi tároló toohello SQL server-alapú logikai kiszolgáló a réteg **prémium**, és a szolgáltatás célkitűzése **P6**. Hello értéket a csúcsos zárójelek cserélje le az SQL server logikai kiszolgáló megfelelő értékeket, és adjon meg egy nevet az új adatbázis hello (is csere hello csúcsos zárójelek). Adatbázis-kiadás és a szolgáltatás objectgive toochange hello értékeket, megfelelő tooyour környezet is beállíthatja. Az oktatóanyagban szereplő hello célra hello áttelepített adatbázis neve **myMigratedDatabase**.

    ```
    SqlPackage.exe /a:import /tcs:"Data Source=<your_server_name>.database.windows.net;Initial Catalog=<your_new_database_name>;User Id=<change_to_your_admin_user_account>;Password=<change_to_your_password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
    ```

   ![sqlpackage importálása](./media/sql-database-migrate-your-sql-server-database/sqlpackage-import.png)

> [!IMPORTANT]
> SQL server logikai kiszolgáló 1433-as porton figyel. Próbált tooconnect tooa SQL server logikai kiszolgáló a vállalati tűzfalon belül, ha ezt a portot kell megnyitni hello vállalati tűzfal az Ön toosuccessfully csatlakozzon.
>

## <a name="connect-using-sql-server-management-studio-ssms"></a>Csatlakozás SQL Server Management Studio (SSMS) használatával

SQL Server Management Studio tooestablish kapcsolat tooyour Azure SQL adatbázis-kiszolgáló és az újonnan áttelepített adatbázis hívása **myMigratedDatabase** ebben az oktatóanyagban. Ha futtatja az SQL Server Management Studio alkalmazást olyan számítógépre, amelyen SQLPackage futtatta, hozzon létre egy tűzfalszabályt ehhez a számítógéphez hello lépéseket követve hello előző eljárásban.

1. Nyissa meg az SQL Server Management Studiót.

2. A hello **tooServer csatlakozás** párbeszédpanelen adja meg a következő információ hello:
   - **Server type** (Kiszolgáló típusa): Adja meg az adatbázismotort
   - **Kiszolgálónév**: Adja meg a teljes kiszolgálónevet, például **mynewserver20170403.database.windows.net**
   - **Authentication** (Hitelesítés): Válassza az SQL Server Authentication (SQL Server-hitelesítés) beállítást
   - **Login** (Bejelentkezés): Adja meg a kiszolgálói rendszergazdai fiókot
   - **Jelszó**: hello jelszó megadása a kiszolgáló-rendszergazdai fiók
 
    ![Csatlakozás az ssms alkalmazásával](./media/sql-database-migrate-your-sql-server-database/connect-ssms.png)

3. Kattintson a **Connect** (Csatlakozás) gombra. hello Object Explorer ablakban nyílik meg. 

4. Az Object Explorerben bontsa ki a **adatbázisok** majd **myMigratedDatabase** tooview hello objektumok hello mintaadatbázis.

## <a name="change-database-properties"></a>Adatbázis tulajdonságainak módosítása

Hello szolgáltatási rétegben, teljesítményszintet és SQL Server Management Studio használatával kompatibilitási szint módosítása Hello importálási fázisban ajánlott tooa magasabb teljesítmény réteg adatbázis a legjobb teljesítmény érdekében importálni, de méretezni után hello importálási toosave pénz még nem kész tooactively használata hello importált adatbázis. Hello kompatibilitási szint módosítása eredményezhet a jobb teljesítmény és a hozzáférés toohello legújabb képességeit hello Azure SQL Database szolgáltatásban. Egy régebbi adatbázis áttelepítésekor az adatbázis kompatibilitási szintje, amely kompatibilis a hello adatbázis hello legalacsonyabb támogatott szint költséget. További információkért lásd: [növeli a lekérdezési teljesítményt kompatibilitási szint 130 az Azure SQL Database](sql-database-compatibility-level-query-performance-130.md).

1. Az Object Explorerben kattintson a jobb gombbal **myMigratedDatabase** kattintson **új lekérdezés**. A lekérdezési ablakban csatlakoztatott tooyour adatbázis megnyitása.

2. Hajtható végre a következő parancs tooset hello szolgáltatásréteg túl hello**szabványos** és teljesítményszintet túl hello**S1**.

    ```
    ALTER DATABASE myMigratedDatabase 
    MODIFY 
        (
        EDITION = 'Standard'
        , MAXSIZE = 250 GB
        , SERVICE_OBJECTIVE = 'S1'
    );
    ```

    ![Módosítsa a szolgáltatási rétegben](./media/sql-database-migrate-your-sql-server-database/service-tier.png)

3. Hajtsa végre a következő parancs toochange hello adatbázis kompatibilitási szintje túl hello**130**.

    ```
    ALTER DATABASE myMigratedDatabase  
    SET COMPATIBILITY_LEVEL = 130;
    ```

   ![kompatibilitási szint módosítása](./media/sql-database-migrate-your-sql-server-database/compat-level.png)

## <a name="next-steps"></a>Következő lépések 
Ebben az oktatóanyagban, előkészítése, exportálhatók és importálhatók az adatbázis. Megtudta, hogy:

> [!div class="checklist"]
> * Egy SQL Server adatbázis előkészítése áttelepítési tooAzure SQL-adatbázis
> * Hello adatbázis tooa BACPAC fájl exportálása
> * Hello BACPAC fájlt importálja az Azure SQL adatbázis

Hogyan előzetes toohello következő útmutató toolearn toosecure az adatbázis.

> [!div class="nextstepaction"]
> [Az Azure SQL-adatbázis védelme](sql-database-security-tutorial.md).


