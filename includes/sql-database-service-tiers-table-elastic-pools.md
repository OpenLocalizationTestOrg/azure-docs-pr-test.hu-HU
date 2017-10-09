<!--
Used in:
sql-database-elastic-pool.md   
sql-database-resource-limits.md
sql-database-service-tiers.md  
-->
 
### <a name="basic-elastic-pool-limits"></a>Alapszintű rugalmas készletek korlátai

| Készlet mérete (eDTU)  | **50** | **100** | **200** | **300** | **400** | **800** | **1200** | **1600** |
|:---|---:|---:|---:| ---: | ---: | ---: | ---: | ---: |
| Maximális adattárterület készletenként* | 5 GB | 10 GB | 20 GB | 29 GB | 39 GB | 78 GB | 117 GB | 156 GB |
| Memóriában tárolt OLTP-k maximális tárterülete készletenként | N/A | N/A | N/A | N/A | N/A | N/A | N/A | N/A |
| Adatbázisok maximális száma készletenként | 100 | 200 | 500 | 500 | 500 | 500 | 500 | 500 |
| Egyidejű feldolgozók (kérelmek) maximális száma készletenként | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| Egyidejű bejelentkezések maximális száma készletenként | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| Egyidejű munkamenetek maximális száma készletenként | 30000 | 30000 | 30000 | 30000 |30000 | 30000 | 30000 | 30000 |
| eDTU-k minimális száma adatbázisonként | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 |
| eDTU-k maximális száma adatbázisonként | 5 | 5 | 5 | 5 | 5 | 5 | 5 | 5 |
| Maximális adattárterület adatbázisonként | 2 GB | 2 GB | 2 GB | 2 GB | 2 GB | 2 GB | 2 GB | 2 GB | 
||||||||

### <a name="standard-elastic-pool-limits"></a>Standard rugalmas készletek korlátai

| Készlet mérete (eDTU)  | **50** | **100** | **200**** | **300**** | **400**** | **800****| 
|:---|---:|---:|---:| ---: | ---: | ---: | 
| Maximális adattárterület készletenként* | 50 GB| 100 GB| 200 GB | 300 GB| 400 GB | 800 GB | 
| Memóriában tárolt OLTP-k maximális tárterülete készletenként | N/A | N/A | N/A | N/A | N/A | N/A | 
| Adatbázisok maximális száma készletenként | 100 | 200 | 500 | 500 | 500 | 500 | 
| Egyidejű feldolgozók (kérelmek) maximális száma készletenként | 100 | 200 | 400 | 600 |  800 | 1600 |
| Egyidejű bejelentkezések maximális száma készletenként | 100 | 200 | 400 | 600 |  800 | 1600 |
| Egyidejű munkamenetek maximális száma készletenként | 30000 | 30000 | 30000 | 30000 | 30000 | 30000 |
| eDTU-k minimális száma adatbázisonként** | 0, 10, 20, 50 | 0, 10, 20, 50, 100 | 0, 10, 20, 50, 100, 200 | 0, 10, 20, 50, 100, 200, 300 | 0, 10, 20, 50, 100, 200, 300, 400 | 0, 10, 20, 50, 100, 200, 300, 400, 800 |
| eDTU-k maximális száma adatbázisonként** | 10, 20, 50 | 10, 20, 50, 100 | 10, 20, 50, 100, 200 | 10, 20, 50, 100, 200, 300 | 10, 20, 50, 100, 200, 300, 400 | 10, 20, 50, 100, 200, 300, 400, 800 | 
| Maximális adattárterület adatbázisonként | 50 GB | 100 GB | 200 GB | 250 GB | 250 GB | 250 GB |
||||||||

### <a name="standard-elastic-pool-limits-continued"></a>Standard rugalmas készletek korlátai (folytatás) 

| Készlet mérete (eDTU)  |  **1200**** | **1600**** | **2000**** | **2500**** | **3000**** |
|:---|---:|---:|---:| ---: | ---: |
| Maximális adattárterület készletenként* | 1,2 TB | 1,6 TB | 2 TB | 2,4 TB | 2,9 TB | 
| Memóriában tárolt OLTP-k maximális tárterülete készletenként | N/A | N/A | N/A | N/A | N/A | 
| Adatbázisok maximális száma készletenként | 500 | 500 | 500 | 500 | 500 | 
| Egyidejű feldolgozók (kérelmek) maximális száma készletenként |  2400 | 3200 | 4000 | 5000 | 6000 |
| Egyidejű bejelentkezések maximális száma készletenként |  2400 | 3200 | 4000 | 5000 | 6000 |
| Egyidejű munkamenetek maximális száma készletenként | 30000 | 30000 | 30000 | 30000 | 30000 | 
| eDTU-k minimális száma adatbázisonként** | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500, 3000 |
| eDTU-k maximális száma adatbázisonként** | 10, 20, 50, 100, 200, 300, 400, 800, 1200 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500, 3000 | 
| Maximális adattárterület adatbázisonként | 250 GB | 250 GB | 250 GB | 250 GB | 250 GB | 
||||||||

### <a name="premium-elastic-pool-limits"></a>Prémium rugalmas készletek korlátai

| Készlet mérete (eDTU)  | **125** | **250** | **500** | **1000** | **1500*****| 
|:---|---:|---:|---:| ---: | ---: | 
| Maximális adattárterület készletenként* | 250 GB | 500 GB | 750 GB | 1 TB | 1,5 TB | 
| Memóriában tárolt OLTP-k maximális tárterülete készletenként | 1 GB| 2 GB| 4 GB| 10 GB| 12 GB| 
| Adatbázisok maximális száma készletenként | 50 | 100 | 100 | 100 | 100 |  
| Egyidejű feldolgozók (kérelmek) maximális száma készletenként | 200 | 400 | 800 | 1600 |  2400 | 
| Egyidejű bejelentkezések maximális száma készletenként | 200 | 400 | 800 | 1600 |  2400 |
| Egyidejű munkamenetek maximális száma készletenként | 30000 | 30000 | 30000 | 30000 | 30000 | 
| eDTU-k minimális száma adatbázisonként | 0, 25, 50, 75, 125 | 0, 25, 50, 75, 125, 250 | 0, 25, 50, 75, 125, 250, 500 | 0, 25, 50, 75, 125, 250, 500, 1000 | 0, 25, 50, 75, 125, 250, 500, 1000 | 
| eDTU-k maximális száma adatbázisonként | 25, 50, 75, 125 | 25, 50, 75, 125, 250 | 25, 50, 75, 125, 250, 500 | 25, 50, 75, 125, 250, 500, 1000 | 25, 50, 75, 125, 250, 500, 1000 |
| Maximális adattárterület adatbázisonként | 250 GB | 500 GB | 500 GB | 500 GB | 500 GB | 
||||||||

### <a name="premium-elastic-pool-limits-continued"></a>Prémium rugalmas készletek korlátai (folytatás) 

| Készlet mérete (eDTU) | **2000***** | **2500***** | **3000***** | **3500***** | **4000*****|
|:---|---:|---:|---:| ---: | ---: | 
| Maximális adattárterület készletenként* | 2 TB | 2,5 TB | 3 TB | 3,5 TB | 4 TB |
| Memóriában tárolt OLTP-k maximális tárterülete készletenként | 16 GB | 20 GB | 24 GB | 28 GB | 32 GB |
| Adatbázisok maximális száma készletenként | 100 | 100 | 100 | 100 | 100 | 
| Egyidejű feldolgozók (kérelmek) maximális száma készletenként |  3200 | 4000 | 4800 | 5600 | 6400 |
| Egyidejű bejelentkezések maximális száma készletenként |  3200 | 4000 | 4800 | 5600 | 6400 |
| Egyidejű munkamenetek maximális száma készletenként | 30000 | 30000 | 30000 | 30000 | 30000 | 
| eDTU-k minimális száma adatbázisonként | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 |  0, 25, 50, 75, 125, 250, 500, 1000, 1750, 4000 | 
| eDTU-k maximális száma adatbázisonként | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750, 4000 | 
| Maximális adattárterület adatbázisonként | 500 GB | 500 GB | 500 GB | 500 GB | 500 GB | 
||||||||

### <a name="premium-rs-elastic-pool-limits"></a>Premium RS rugalmas készletek korlátai

| Készlet mérete (eDTU)  | **125** | **250** | **500** | **1000** |
|:---|---:|---:|---:| ---: | ---: | 
| Maximális adattárterület készletenként* | 250 GB| 500 GB | 750 GB | 750 GB |
| Memóriában tárolt OLTP-k maximális tárterülete készletenként | 1 GB | 2 GB | 4 GB | 10 GB |
| Adatbázisok maximális száma készletenként | 50 | 100 | 100 | 100 |
| Egyidejű feldolgozók (kérelmek) maximális száma készletenként | 200 | 400 | 800 | 1600 |
| Egyidejű bejelentkezések maximális száma készletenként | 200 | 400 | 800 | 1600 |
| Egyidejű munkamenetek maximális száma készletenként | 30000 | 30000 | 30000 | 30000 |
| eDTU-k minimális száma adatbázisonként | 0, 25, 50, 75, 125 | 0, 25, 50, 75, 125, 250 | 0, 25, 50, 75, 125, 250, 500 | 0, 25, 50, 75, 125, 250, 500, 1000 |
| eDTU-k maximális száma adatbázisonként | 25, 50, 75, 125 | 25, 50, 75, 125, 250 | 25, 50, 75, 125, 250, 500 | 25, 50, 75, 125, 250, 500, 1000 | 
| Maximális adattárterület adatbázisonként | 250 GB | 500 GB | 500 GB | 500 GB | 
||||||||

> [!IMPORTANT]
>\*Készletezett adatbázisok megosztani a tárolókészlet, így a rugalmas készletekben található adattároló korlátozott toohello hello fennmaradó tárolókészlet vagy adatbázisonkénti maximális tárolási kisebb. 
>
>
>\*\* Az adatbázisonkénti minimális/maximális eDTU-k 200 eDTU és nagyobb mennyiségekkel már elérhetők a nyilvános előzetes verzióban.
>
>\*\*\*hello alapértelmezett maximális adattárolás készletenként prémium készletek az 500 edtu-k esetében, vagy több nem 750 GB. tooobtain hello magasabb adatok maximális tárhelyméretet a prémium készletbe 1000 vagy több edtu-k esetében, ez a méret explicit módon választani hello Azure-portál használatával [PowerShell](../articles/sql-database/sql-database-elastic-pool.md#manage-sql-database-elastic-pools-using-powershell), hello [Azure CLI](../articles/sql-database/sql-database-elastic-pool.md#manage-sql-database-elastic-pools-using-the-azure-cli), vagy hello [REST API-t](../articles/sql-database/sql-database-elastic-pool.md#manage-sql-database-elastic-pools-using-the-rest-api). Prémium szintű készletek a több mint 1 TB-nyi tárhelyre van jelenleg a következő régiókban hello nyilvános előzetes verzió: US East2, USA nyugati régiója, Velünk – (kormányzati) Virginia, Nyugat-Európa, Németország központi, Dél Kelet-Ázsiában, kelet-japán, Kelet-Ausztrália, Kanada központi és Kanada keleti. hello maximális tárolási készletenként minden más területen jelenleg korlátozott too750 GB.
>
