---
title: "Azure Portal: SQL-adatbázis létrehozása | Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy SQL Database logikai kiszolgálóhoz kiszolgálószintű tűzfalszabályt és adatbázisok hello Azure-portálon. Azt is megtudhatja, tooquery hello Azure-portál használatával Azure SQL-adatbázis."
keywords: "oktatóanyag az SQL Database használatához, SQL-adatbázis létrehozása"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: aeb8c4c3-6ae2-45f7-b2c3-fa13e3752eed
ms.service: sql-database
ms.custom: mvc,DBs & servers
ms.workload: data-management
ms.tgt_pltfrm: portal
ms.devlang: na
ms.topic: hero-article
ms.date: 05/30/2017
ms.author: carlrab
ms.openlocfilehash: d30352d834f2007e0b6b3eabfc3c108c61479b22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-sql-database-in-hello-azure-portal"></a>Hozzon létre egy Azure SQL-adatbázis hello Azure-portálon

A gyors üzembe helyezési útmutató végigvezeti hogyan toocreate egy SQL-adatbázis használati ideje az Azure-ban. Az Azure SQL-adatbázis egy "adatbázis-a-szolgáltatás" ajánlat, amely lehetővé teszi toorun és a skála magas rendelkezésre állású SQL Server-adatbázisok hello felhőben. A gyors üzembe helyezési bemutatja, hogyan tooget el hozzon létre egy SQL-adatbázist hello Azure-portálon.

Ha nem rendelkezik Azure-előfizetéssel, első lépésként mindössze néhány perc alatt létrehozhat egy [ingyenes](https://azure.microsoft.com/free/) fiókot.

## <a name="log-in-toohello-azure-portal"></a>Jelentkezzen be toohello Azure-portálon

Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).

## <a name="create-a-sql-database"></a>SQL-adatbázis létrehozása

Az Azure SQL-adatbázis [számítási és tárolási erőforrások](sql-database-service-tiers.md) egy meghatározott készletével együtt jön létre. hello adatbázist a rendszer létrehoz egy [Azure erőforráscsoport](../azure-resource-manager/resource-group-overview.md) és az egy [Azure SQL Database logikai kiszolgáló](sql-database-features.md). 

Kövesse ezeket a lépéseket toocreate hello Adventure Works LT mintaadatokat tartalmazó SQL-adatbázis. 

1. Kattintson a hello **új** hello bal felső sarkában hello Azure-portálon található gombra.

2. Válassza ki **adatbázisok** a hello **új** lapon, és válassza ki **SQL-adatbázis** a hello **adatbázisok** lap.

   ![adatbázis létrehozása-1](./media/sql-database-get-started-portal/create-database-1.png)

3. Hello SQL-adatbázis űrlap kitöltése a következő információ, hello kép megelőző hello szerint:   

   | Beállítás       | Ajánlott érték | Leírás | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Adatbázis neve** | mySampleDatabase | Az érvényes adatbázisnevekkel kapcsolatban lásd az [adatbázis-azonosítókat](https://docs.microsoft.com/en-us/sql/relational-databases/databases/database-identifiers) ismertető cikket. | 
   | **Előfizetés** | Az Ön előfizetése  | Az előfizetései részleteivel kapcsolatban lásd az [előfizetéseket](https://account.windowsazure.com/Subscriptions) ismertető cikket. |
   | **Erőforráscsoport**  | myResourceGroup | Az érvényes erőforráscsoport-nevekkel kapcsolatban lásd az [elnevezési szabályokat és korlátozásokat](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) ismertető cikket. |
   | **Forrás forrása** | Minta (AdventureWorksLT) | Hello AdventureWorksLT séma- és adatok betöltődnek az új adatbázis |

   > [!IMPORTANT]
   > Hello mintaadatbázis az űrlapon kell kijelölni, mert a gyors üzembe helyezési hello maradéka használatban van.
   > 

4. A **Server**, kattintson a **kötelező beállítások konfigurálása** és kitöltése hello az SQL server (a logikai kiszolgáló) képernyőn hello során a következő információkat, mint a kép a következő hello:   

   | Beállítás       | Ajánlott érték | Leírás | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Kiszolgálónév** | Bármely globálisan egyedi név | Az érvényes kiszolgálónevekkel kapcsolatban lásd az [elnevezési szabályokat és korlátozásokat](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) ismertető cikket. | 
   | **Kiszolgálói rendszergazdai bejelentkezés** | Bármely érvényes név | Az érvényes bejelentkezési nevekkel kapcsolatban lásd az [adatbázis-azonosítókat](https://docs.microsoft.com/en-us/sql/relational-databases/databases/database-identifiers) ismertető cikket. |
   | **Jelszó** | Bármely érvényes jelszó | A jelszó legalább 8 karakterből kell állnia, és a következő kategóriák hello hármat tartalmaznia kell: nagybetűk, kisbetűk, számok, és nem alfanumerikus karakterek és. |
   | **Előfizetés** | Az Ön előfizetése | Az előfizetései részleteivel kapcsolatban lásd az [előfizetéseket](https://account.windowsazure.com/Subscriptions) ismertető cikket. |
   | **Erőforráscsoport** | myResourceGroup | Az érvényes erőforráscsoport-nevekkel kapcsolatban lásd az [elnevezési szabályokat és korlátozásokat](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) ismertető cikket. |
   | **Hely** | Bármely érvényes hely | A régiókkal kapcsolatos információkért lásd [az Azure régióit](https://azure.microsoft.com/regions/) ismertető cikket. |

   > [!IMPORTANT]
   > hello kiszolgáló-rendszergazdai bejelentkezés és a jelszót, amely az itt megadott a szükséges toolog toohello Server és az adatbázisok a gyors üzembe helyezési későbbi részében. Jegyezze meg vagy jegyezze fel ezt az információt későbbi használatra. 
   >  

   ![adatbázis-kiszolgáló létrehozása](./media/sql-database-get-started-portal/create-database-server.png)

5. Hello űrlap befejeződésekor kattintson **válasszon**.

6. Kattintson a **tarifacsomag** toospecify hello és teljesítményszintet szolgáltatásszint az új adatbázishoz. Használjon hello csúszkát tooselect **20 Dtu** és **250** GB tárhelyet. További információ a DTU-król: [Mi a DTU?](sql-database-what-is-a-dtu.md)

   ![adatbázis létrehozása-s1](./media/sql-database-get-started-portal/create-database-s1.png)

7. Követően kijelölt hello dtu-k, kattintson a **alkalmaz**.  

8. Most, hogy az SQL-adatbázis űrlap hello befejeződött, kattintson a **létrehozása** tooprovision hello adatbázis. Az üzembe helyezés eltarthat néhány percig. 

9. Hello eszköztáron kattintson **értesítések** toomonitor hello telepítési folyamat.

   ![értesítés](./media/sql-database-get-started-portal/notification.png)

## <a name="create-a-server-level-firewall-rule"></a>Kiszolgálószintű tűzfalszabály létrehozása

SQL Database szolgáltatás hello tűzfal hello kiszolgálói szinten-, amely megakadályozza, hogy a külső alkalmazások és eszközök toohello kiszolgáló vagy hello kiszolgálón lévő összes adatbázis csatlakozzon, kivéve, ha egy tűzfalszabály tooopen hello tűzfal adott IP-címekhez hoz létre. Kövesse az alábbi lépéseket toocreate egy [SQL-adatbázis kiszolgálószintű tűzfalszabály](sql-database-firewall-configure.md) az ügyfél IP-cím, és engedélyezze a külső kapcsolatot csak az IP-cím hello SQL-adatbázis tűzfalon keresztül. 

> [!NOTE]
> Az SQL Database az 1433-as porton kommunikál. Ha a vállalati hálózatból származó tooconnect, a hálózati tűzfal előfordulhat, hogy nem engedélyezett a 1433-as port kimenő forgalmát. Ha igen, kivéve, ha az IT-részleg megnyitja az 1433-as port tooyour Azure SQL adatbázis-kiszolgáló nem lehet csatlakoztatni.
>

1. Hello központi telepítés befejezése után kattintson **SQL-adatbázisok** hello bal oldali menüből, és kattintson a **mySampleDatabase** a hello **SQL-adatbázisok** lap. hello áttekintő lapjára jut a adatbázis megnyílik, teljes mértékben hello megjelenítő minősített kiszolgáló neve (például **mynewserver20170313.database.windows.net**) és további konfigurációs lehetőségeket. Későbbi felhasználás céljára másolja ki ezt a teljes kiszolgálónevet.

   > [!IMPORTANT]
   > A teljesen minősített neve tooconnect tooyour kiszolgálók és a későbbi gyors üzembe helyezések adatbázisainak van szükség.
   > 

   ![kiszolgáló neve](./media/sql-database-connect-query-dotnet/server-name.png) 

2. Kattintson a **kiszolgáló tűzfalának beállítása** hello eszköztár hello előző ábrának megfelelően. Hello **tűzfalbeállítások** hello SQL Database-kiszolgálóhoz tartozó lapon nyílik meg. 

   ![kiszolgálói tűzfalszabály](./media/sql-database-get-started-portal/server-firewall-rule.png) 

3. Kattintson a **ügyfél IP-cím hozzáadása** hello eszköztár tooadd meg az aktuális IP-cím tooa Új tűzfalszabály. A tűzfalszabály az 1433-as portot egy egyedi IP-cím vagy egy IP-címtartomány számára nyithatja meg.

4. Kattintson a **Save** (Mentés) gombra. Az aktuális IP-címek hello logikai kiszolgálón 1433-as port megnyitása egy kiszolgálószintű tűzfalszabályt jön létre.

   ![kiszolgálótűzfal-szabály beállítása](./media/sql-database-get-started-portal/server-firewall-rule-set.png) 

4. Kattintson a **OK** , majd zárja be a hello **tűzfalbeállítások** lap.

Csatlakoztathatja toohello SQL adatbázis-kiszolgáló és az adatbázisok, SQL Server Management Studio vagy az Ön által választott, a korábban létrehozott hello kiszolgáló rendszergazdai fiókjának használatával IP-címről egy másik eszköz használatával.

> [!IMPORTANT]
> Az összes Azure-szolgáltatások alapértelmezés szerint engedélyezve van a hozzáférés hello SQL-adatbázis tűzfalon keresztül. Kattintson a **OFF** meg a lap toodisable az összes Azure-szolgáltatásokhoz.
>

## <a name="query-hello-sql-database"></a>Hello SQL-adatbázis lekérdezése

Most, hogy létrehozta a mintaadatbázis az Azure-ban, most eszközzel hello beépített lekérdezést hello toohello adatbázis és a lekérdezés hello adatokat is elérheti az Azure portál tooconfirm belül. 

1. Az adatbázis hello SQL-adatbázis lapján kattintson **eszközök** hello eszköztáron. Hello **eszközök** lap megnyitásakor.

   ![eszközök menü](./media/sql-database-get-started-portal/tools-menu.png) 

2. Kattintson a **Lekérdezésszerkesztő (előzetes verzió)**, hello kattintson **feltételek előzetes** jelölőnégyzetet, majd **OK**. hello lekérdezés-szerkesztő lap nyílik meg.

3. Kattintson a **bejelentkezési** majd, amikor a rendszer kéri, válassza ki **SQL server-hitelesítés** hello kiszolgáló-rendszergazdai bejelentkezés és a korábban létrehozott jelszót adja meg.

   ![bejelentkezés](./media/sql-database-get-started-portal/login.png) 

4. Kattintson a **OK** a toolog.

5. Ön hitelesítése után vannak, írja be a következő lekérdezés hello lekérdezés-szerkesztő ablaktáblán hello.

   ```sql
   SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
   FROM SalesLT.ProductCategory pc
   JOIN SalesLT.Product p
   ON pc.productcategoryid = p.productcategoryid;
   ```

6. Kattintson a **futtatása** majd tekintse át a lekérdezés eredményének hello hello **eredmények** ablaktáblán.

   ![lekérdezésszerkesztő: eredmények](./media/sql-database-get-started-portal/query-editor-results.png)

7. Bezárás hello **Lekérdezésszerkesztő** lap és hello **eszközök** lap.

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Ha egy másik gyorsindítási/oktatóanyag nem kell ezeket az erőforrásokat (lásd: [további lépések](#next-steps)), törölheti azokat a hello következő tevékenységek végrehajtásával:


1. A hello hello Azure-portálon a bal oldali menüből, kattintson az **erőforráscsoportok** majd **myResourceGroup**. 
2. Az erőforrás csoport lapján kattintson a **törlése**, típus **myResourceGroup** hello szövegmezőbe, és kattintson a **törlése**.

## <a name="next-steps"></a>Következő lépések

Most, hogy rendelkezik egy adatbázissal, csatlakoztathatja a kedvenc eszközeit, és lekérdezéseket hajthat végre velük. További információkért válassza ki az eszközt az alábbiak közül:

- [SQL Server Management Studio](sql-database-connect-query-ssms.md)
- [Visual Studio Code](sql-database-connect-query-vscode.md)
- [.NET](sql-database-connect-query-dotnet.md)
- [PHP](sql-database-connect-query-php.md)
- [Node.js](sql-database-connect-query-nodejs.md)
- [Java](sql-database-connect-query-java.md)
- [Python](sql-database-connect-query-python.md)
- [Ruby](sql-database-connect-query-ruby.md)
