### <a name="prerequisites"></a>Előfeltételek
* Az Azure-fiók; létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free)
* Egy [Azure SQL Database](../articles/sql-database/sql-database-get-started.md) és annak kapcsolati adatait, beleértve a hello kiszolgáló neve, az adatbázisnév és a felhasználónév/jelszó. Ez az információ hello SQL adatbázis-kapcsolati karakterlánc szerepel:
  
    Kiszolgáló = tcp:*yoursqlservername*. database.windows.net,1433;Initial katalógus =*yourqldbname*; Biztonsági információ megőrzése = False; Felhasználói azonosító = {your_username}; Jelszó = {your_password}; MultipleActiveResultSets eredménykészleteket = False; Titkosítani = True; TrustServerCertificate = False; Kapcsolódási időtúllépés = 30.
  
    Tudjon meg többet az [Azure SQL-adatbázisok](https://azure.microsoft.com/services/sql-database).

> [!NOTE]
> Egy Azure SQL-adatbázis létrehozásakor részét képező SQL hello mintaadatbázisokat is létrehozhat. 
> 
> 

Mielőtt az Azure SQL Database logikai alkalmazás használ, csatlakoztassa a tooyour SQL-adatbázis. Ehhez egyszerűen a logikai alkalmazásban a hello Azure-portálon.  

Csatlakozás az Azure SQL Database szolgáltatáshoz a következő hello lépések tooyour:  

1. Logikai alkalmazás létrehozása. Hello Logic Apps-tervezőben adja hozzá egy eseményindító, és adja hozzá az műveletet. Válassza ki **megjelenítése Microsoft felügyelt API-k** hello a legördülő listából, és írja be az "sql" hello Keresés mezőbe. Hello műveletek közül választhat:  
   
    ![Az SQL Azure kapcsolat létrehozását lépést](./media/connectors-create-api-sqlazure/sql-actions.png)
2. Ha még nem korábban létrehozott bármely kapcsolatok tooSQL adatbázis kéri hello csatlakozási adatait:  
   
    ![Az SQL Azure kapcsolat létrehozását lépést](./media/connectors-create-api-sqlazure/connection-details.png) 
3. Írja be a hello SQL-adatbázis adatait. Tulajdonságok csillaggal szükség.
   
   | Tulajdonság | Részletek |
   | --- | --- |
   | Csatlakozás az átjárón keresztül |Hagyja üresen ezt. Ez használatos, amikor tooan csatlakozás helyszíni SQL Server. |
   | Kapcsolat neve * |Adjon meg egy tetszőleges nevet a kapcsolat. |
   | SQL Server neve * |Adja meg a kiszolgálónév hello; Ez az hasonlót *servername.database.windows.net*. hello kiszolgálónév hello SQL adatbázis tulajdonságai a hello Azure-portálon jelenik meg, és megjelenik a hello kapcsolati karakterláncot. |
   | SQL-adatbázis neve * |Adja meg az SQL-adatbázis adott hello név. Ez hello SQL adatbázis tulajdonságai hello kapcsolati karakterláncban szereplő: Initial Catalog =*yoursqldbname*. |
   | Felhasználónév * |Adja meg a létrehozott hello SQL-adatbázis létrehozásakor hello felhasználónév. Ez hello SQL adatbázis tulajdonságai a hello Azure-portálon jelenik meg. |
   | Jelszó * |Adja meg a létrehozott hello SQL-adatbázis létrehozásakor hello jelszót. |
   
    Ezek a hitelesítő adatok használt tooauthorize a logic app tooconnect, és az SQL-adatok eléréséhez. Művelet befejeződése után a kapcsolat adatai hasonló toohello következő jelennek meg:  
   
    ![Az SQL Azure kapcsolat létrehozását lépést](./media/connectors-create-api-sqlazure/sample-connection.png) 
4. Kattintson a **Létrehozás** gombra. 
5. Értesítés hello kapcsolat létrejött. Most, folytassa a Logic Apps alkalmazást hello más szükséges lépések: 
   
    ![Az SQL Azure kapcsolat létrehozását lépést](./media/connectors-create-api-sqlazure/table.png)

