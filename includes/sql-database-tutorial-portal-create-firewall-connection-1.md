## <a name="log-in-to-the-azure-portal"></a>Jelentkezzen be az Azure portálra.

Jelentkezzen be az [Azure portálra](https://portal.azure.com/).

## <a name="create-a-blank-sql-database-using-the-azure-portal"></a>Az Azure portál használatával üres SQL-adatbázis létrehozása

Az Azure SQL-adatbázis [számítási és tárolási erőforrások](../articles/sql-database/sql-database-service-tiers.md) egy meghatározott készletével együtt jön létre. Az adatbázis egy [Azure-erőforráscsoporton](../articles/azure-resource-manager/resource-group-overview.md) belül egy [Azure SQL Database logikai kiszolgálón](../articles/sql-database/sql-database-features.md) jön létre. 

Kövesse az alábbi lépéseket egy üres SQL-adatbázis létrehozásához. 

1. Kattintson az Azure Portal bal felső sarkában található **Új** gombra.

2. Az **Új** panelen válassza az **Adatbázisok** lehetőséget, majd az **Adatbázisok** panelen válassza az **SQL Database** lehetőséget. 

   ![Üres-adatbázis létrehozása](../articles/sql-database/media/sql-database-design-first-database/create-empty-database.png)

3. Töltse ki az SQL Database űrlapját a következő információkkal az előző képen látható módon:   

   | Beállítás | Ajánlott érték | Leírás |
   | --------| --------------- | ----------- | 
   | **Adatbázis neve** | mySampleDatabase | Az érvényes adatbázisnevekkel kapcsolatban lásd az [adatbázis-azonosítókat](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers) ismertető cikket. | 
   | **Előfizetés** | Az Ön előfizetése  | Az előfizetései részleteivel kapcsolatban lásd az [előfizetéseket](https://account.windowsazure.com/Subscriptions) ismertető cikket. |
   | **Erőforráscsoport** | myResourceGroup | Az érvényes erőforráscsoport-nevekkel kapcsolatban lásd az [elnevezési szabályokat és korlátozásokat](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) ismertető cikket. |
   | **Forrás kiválasztása** | Az üres adatbázis | Meghatározza, hogy egy üres adatbázist kell létrehozni. |
   ||||

4. Kattintson a **Kiszolgáló** lehetőségre új kiszolgáló létrehozásához és konfigurálásához az új adatbázis számára. Töltse ki a **új kiszolgáló űrlap** a következő információkat: 

   | Beállítás | Ajánlott érték | Leírás |
   | --------| --------------- | ----------- | 
   | **Kiszolgálónév** | A globálisan egyedi nevet. | Az érvényes kiszolgálónevekkel kapcsolatban lásd az [elnevezési szabályokat és korlátozásokat](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) ismertető cikket. | 
   | **Kiszolgálói rendszergazdai bejelentkezés** | Bármilyen érvényes nevet. | Az érvényes bejelentkezési nevekkel kapcsolatban lásd az [adatbázis-azonosítókat](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers) ismertető cikket.|
   | **Jelszó** | Bármilyen érvényes jelszót. | A jelszó legalább 8 karakterből kell állnia, és az alábbiak közül hármat tartalmaznia kell: nagybetűk, kisbetűk, számok és nem alfanumerikus karakterek száma. |
   | **Hely** | Egyik érvényes helyen sem. | A régiókkal kapcsolatos információkért lásd [az Azure régióit](https://azure.microsoft.com/regions/) ismertető cikket. |
   ||||

   ![adatbázis-kiszolgáló létrehozása](../articles/sql-database/media/sql-database-design-first-database/create-database-server.png)

5. Kattintson a **Kiválasztás** gombra.

6. Kattintson a **Tarifacsomag** parancsra az új adatbázis szolgáltatás- és teljesítményszintjének megadásához. A jelen oktatóanyag esetében válassza ki a **20 Dtu** és **250** GB tárhelyet.

   ![adatbázis létrehozása-s1](../articles/sql-database/media/sql-database-design-first-database/create-empty-database-pricing-tier.png)

7. Kattintson az **Alkalmaz** gombra.  

8. Válassza ki a **rendezés** az üres adatbázis (a jelen oktatóanyag esetében használja az alapértelmezett érték). Rendezések kapcsolatos további információkért lásd: [rendezések](https://docs.microsoft.com/sql/t-sql/statements/collations)

9. Kattintson a **Létrehozás** elemre az adatbázis létrehozásához. Kiépítés kapcsolatos egy perc és fél befejezéséhez vesz igénybe. 

10. Az eszköztáron kattintson az **Értesítések** parancsra az üzembe helyezési folyamat megfigyeléséhez.

   ![értesítés](../articles/sql-database/media/sql-database-get-started-portal/notification.png)

## <a name="create-a-server-level-firewall-rule-using-the-azure-portal"></a>Hozzon létre egy kiszolgálószintű tűzfalszabályt, az Azure portál használatával

Az SQL Database szolgáltatás tűzfal a kiszolgáló szintjén hoz létre. Kezdetben a tűzfal megakadályozza, hogy a külső eszközök és alkalmazások nem tudnak csatlakozni a kiszolgálóhoz, vagy a kiszolgálón lévő összes adatbázis. Kapcsolatainak engedélyezését követően egy tűzfalszabály IP-címek megnyitásához. Hajtsa végre az alábbi lépéseket követve létrehozhat egy [SQL-adatbázis kiszolgálószintű tűzfalszabály](../articles/sql-database/sql-database-firewall-configure.md) az ügyfél IP-címhez, és az IP-cím csak a SQL Database tűzfalon külső kapcsolódásának engedélyezése. 


> [!NOTE]
> Az Azure SQL Database 1433-as porton keresztül kommunikál. SQL-adatbázishoz való csatlakozás csak azt követően a hálózat tűzfala lehetővé teszi, hogy a kimenő adatforgalmat a 1433-as porton keresztül.


1. Az üzembe helyezés befejezése után kattintson az **SQL-adatbázisok** elemre a bal oldali menüben, majd kattintson a **mySampleDatabase** adatbázisra az **SQL-adatbázisok** lapon. Megnyílik az adatbázis áttekintő oldala, amelyen látható a teljes kiszolgálónév (például: **mynewserver20170313.database.windows.net**), valamint a további konfigurálható beállítások. Későbbi felhasználás céljára másolja ki ezt a teljes kiszolgálónevet.

   > [!IMPORTANT]
   > A későbbi rövid útmutatók során szüksége lesz erre a teljes kiszolgálónévre a kiszolgálóhoz és az adatbázisokhoz való csatlakozáshoz.
   > 

   ![kiszolgáló neve](../articles/sql-database/media/sql-database-get-started-portal/server-name.png) 

2. Kattintson a **Kiszolgálótűzfal beállítása** lehetőségre az eszköztáron az előző képen látható módon. Megnyílik az SQL Database kiszolgálóhoz tartozó **Tűzfalbeállítások** oldal. 

   ![kiszolgálói tűzfalszabály](../articles/sql-database/media/sql-database-get-started-portal/server-firewall-rule.png) 


3. Az eszköztár **Ügyfél IP-címének hozzáadása** elemére kattintva vegye fel aktuális IP-címét egy új tűzfalszabályba. A tűzfalszabály az 1433-as portot egy egyedi IP-cím vagy egy IP-címtartomány számára nyithatja meg.

4. Kattintson a **Save** (Mentés) gombra. A rendszer létrehoz egy kiszolgálószintű tűzfalszabályt az aktuális IP-címhez, és megnyitja az 1433-as portot a logikai kiszolgálón.

   ![kiszolgálótűzfal-szabály beállítása](../articles/sql-database/media/sql-database-get-started-portal/server-firewall-rule-set.png) 

4. Kattintson az **OK** gombra, majd zárja be a **Tűzfalbeállítások** lapot.

Most csatlakozhat az Azure SQL adatbázis-kiszolgáló és az adatbázisok egy eszköz, például az SQL Server Management Studio (SSMS) használatával. A kapcsolat az IP-címről, és a korábban létrehozott kiszolgáló-rendszergazdai fiókot használja.


> [!IMPORTANT]
> Alapértelmezés szerint az összes Azure-szolgáltatás számára engedélyezett a hozzáférés az SQL Database tűzfalán keresztül. Kattintson a **KI** gombra ezen az oldalon az összes Azure-szolgáltatás hozzáférésének letiltásához.


## <a name="get-connection-string-values-using-the-azure-portal"></a>Az Azure portál használatával karakterlánc-értékek-kapcsolat lekérdezése

Kérje le az Azure SQL Database kiszolgáló teljes kiszolgálónevét az Azure Portalon. Használja a teljes kiszolgálónevet az SQL Server Management Studióban a kiszolgálóhoz történő csatlakozáshoz.

1. Jelentkezzen be az [Azure portálra](https://portal.azure.com/).

2. Válassza az **SQL-adatbázisok** elemet a bal oldali menüben, majd kattintson az új adatbázisra az **SQL-adatbázisok** oldalon. 

3. Az Azure Portalon az adatbázishoz tartozó lap **Alapvető erőforrások** ablaktábláján keresse meg, majd másolja ki a **Kiszolgáló nevét**.

   ![kapcsolatadatok](../articles/sql-database/media/sql-database-get-started-portal/server-name.png) 
