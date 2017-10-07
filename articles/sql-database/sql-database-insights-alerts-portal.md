---
title: "aaaUse az Azure portál toocreate SQL adatbázis-értesítések |} Microsoft Docs"
description: "Hello Azure portál toocreate SQL-adatbázis riasztások, indítására képes értesítéseket vagy automation megadott hello feltételek teljesülése esetén használja."
author: aamalvea
manager: jhubbard
editor: 
services: sql-database
documentationcenter: 
ms.assetid: f7457655-ced6-4102-a9dd-7ddf2265c0e2
ms.service: sql-database
ms.custom: monitor and tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: aamalvea
ms.openlocfilehash: 4e494b130a26c4cdf42445cb49648fce9bf4d300
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-portal-toocreate-alerts-for-azure-sql-database-and-data-warehouse"></a>Az Azure portál toocreate riasztások használja az Azure SQL adatbázishoz és Adatraktárhoz

## <a name="overview"></a>Áttekintés
Ez a cikk bemutatja, hogyan használja az Azure SQL Database és az adatraktár figyelmeztetéseket tooset hello Azure-portálon. Ez a cikk a riasztási időszakok beállítása ajánlott eljárásokat is tartalmaz.    

A figyelési metrikákat, vagy események, az Azure-szolgáltatások alapuló riasztást kaphat.

* **Metrika értékek** – hello eseményindítók riasztást, ha a megadott metrika értékét hello mindkét irányban rendel a küszöbérték keverve használ. Ez azt jelenti, hogy elindítja a mindkét amikor először hello feltétel teljesül, és majd ezt követően, hogy a feltétel mikor van már nem teljesül.    
* **Tevékenység naplóeseményeket** -riasztást aktiválhatók *minden* esemény, vagy csak akkor, ha bizonyos számú esemény történik.

Egy riasztás toodo hello követően amikor elindítja a konfigurálhatja:

* e-mail értesítések toohello szolgáltatás-rendszergazda és a társadminisztrátorok küldése
* e-mail küldése a megadott tooadditional e-maileket.
* A webhook hívása

Konfigurálhatja, és a riasztási szabályok használatával adatainak beolvasása

* [Azure Portal](../monitoring-and-diagnostics/insights-alerts-portal.md)
* [PowerShell](../monitoring-and-diagnostics/insights-alerts-powershell.md)
* [parancssori felület (CLI)](../monitoring-and-diagnostics/insights-alerts-command-line-interface.md)
* [Az Azure figyelő REST API-n](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-with-hello-azure-portal"></a>Riasztási szabályt létrehozni egy metrika hello Azure-portálon
1. A hello [portal](https://portal.azure.com/), keresse meg a hello erőforrás figyelési érdekli, és válassza ki azt.
2. Ez a lépés nem egyezik az SQL-adatbázis és a rugalmas készletek és SQL DW: 

   - **SQL-adatbázis és a rugalmas készletek csak**: válasszon **riasztások** vagy **riasztási szabályok** hello figyelés szakasz alatt. hello szöveg és ikon eltérő lehet attól függően némileg különböző erőforrások.  
   
     ![Figyelés](../monitoring-and-diagnostics/media/insights-alerts-portal/AlertRulesButton.png)
  
   - **CSAK SQL DW**: válasszon **figyelés** hello GYAKORI feladatok szakasz alatt. Kattintson a hello **DWU használati** grafikon.

     ![GYAKORI FELADATOK](../monitoring-and-diagnostics/media/insights-alerts-portal/AlertRulesButtonDW.png)

3. Jelölje be hello **riasztás hozzáadása** parancsot, majd hello mezők kitöltésével.
   
    ![Riasztások hozzáadása](../monitoring-and-diagnostics/media/insights-alerts-portal/AddDBAlertPage.png)
4. **Név** a riasztás szabályt, majd válassza ki a **leírása**, amely értesítési e-mailt is mutatja.
5. Jelölje be hello **metrika** toomonitor szeretné, majd kattintson egy **feltétel** és **küszöbérték** hello metrika értékét. Hello is választott **időszak** metrika hello idő szabály kell teljesíteni hello riasztási eseményindítók előtt. Így például hello időszak "PT5M" használ, és 80 % fölötti CPU keresi a riasztás, ha hello riasztás váltja ki, ha hello CPU már következetesen fenti 80 % 5 perc. Amennyiben az első eseményindító hello következik be, azt újra váltja ki, ha 5 percig 80 % alatt marad hello CPU. hello CPU mérési 1 percenként történik.   
6. Ellenőrizze **E-mail-tulajdonosok...**  Ha azt szeretné, hogy a rendszergazdák és a társadminisztrátorok toobe e-mailben amikor hello riasztási következik be.
7. Ha azt szeretné, hogy további e-mailek tooreceive egy értesítést, ha hello riasztási következik be, vegye fel a hello **további rendszergazda email(s)** mező. Több e-mailek külön és pontosvesszővel kell elválasztani -  *email@contoso.com;email2@contoso.com*
8. Be egy érvényes URI-azonosító található hello **Webhook** mező Ha azt szeretné, az úgynevezett amikor hello riasztási következik be.
9. Válassza ki **OK** Amikor kész toocreate hello riasztás.   

Néhány percen belül hello riasztás aktív, és elindítja a leírt módon.

## <a name="managing-your-alerts"></a>A riasztások kezelése
Miután létrehozott egy riasztást, kijelölheti azt és:

* Egy grafikonon hello metrika küszöbérték és a tényleges értékek hello hello előző nap megtekintése.
* Szerkesztheti és törölheti azt.
* **Tiltsa le a** vagy **engedélyezése** azt, ha azt szeretné tootemporarily stop, vagy folytassa a riasztás-mailjeire.


## <a name="sql-database-alert-values"></a>SQL-adatbázis riasztási értékek

| Erőforrás típusa | Metrika neve | Rövid név | Aggregáció típusa | Minimális riasztási időkerete|
| --- | --- | --- | --- | --- |
| SQL-adatbázis | cpu_percent | Processzorhasználat (%) | Átlagos | 5 perc |
| SQL-adatbázis | physical_data_read_percent | Adat IO kihasználtsága (%) | Átlagos | 5 perc |
| SQL-adatbázis | log_write_percent | Napló IO százalékos aránya | Átlagos | 5 perc |
| SQL-adatbázis | dtu_consumption_percent | DTU-kihasználtság (%) | Átlagos | 5 perc |
| SQL-adatbázis | Tárolás | Adatbázis teljes mérete | Maximális | 30 perc |
| SQL-adatbázis | connection_successful | Sikeres kapcsolatok | Összes | 10 perc |
| SQL-adatbázis | connection_failed | Nem sikerült kapcsolatok | Összes | 10 perc |
| SQL-adatbázis | blocked_by_firewall | Tiltsa le tűzfal | Összes | 10 perc |
| SQL-adatbázis | Holtpont | Holtpont | Összes | 10 perc |
| SQL-adatbázis | storage_percent | Adatbázis méretének kihasználtsága | Maximális | 30 perc |
| SQL-adatbázis | xtp_storage_percent | A memórián belüli online Tranzakciófeldolgozási tárolási percent(Preview) | Átlagos | 5 perc |
| SQL-adatbázis | workers_percent | Feldolgozók százalékos aránya | Átlagos | 5 perc |
| SQL-adatbázis | sessions_percent | Munkamenetek százaléka | Átlagos | 5 perc |
| SQL-adatbázis | dtu_limit | DTU korlátot | Átlagos | 5 perc |
| SQL-adatbázis | dtu_used | Felhasznált DTU | Átlagos | 5 perc |
||||||
| A rugalmas készlet | cpu_percent | Processzorhasználat (%) | Átlagos | 10 perc |
| A rugalmas készlet | physical_data_read_percent | Adat IO kihasználtsága (%) | Átlagos | 10 perc |
| A rugalmas készlet | log_write_percent | Napló IO százalékos aránya | Átlagos | 10 perc |
| A rugalmas készlet | dtu_consumption_percent | DTU-kihasználtság (%) | Átlagos | 10 perc |
| A rugalmas készlet | storage_percent | Tárolási százalékos aránya | Átlagos | 10 perc |
| A rugalmas készlet | workers_percent | Feldolgozók százalékos aránya | Átlagos | 10 perc |
| A rugalmas készlet | eDTU_limit | eDTU korlátot | Átlagos | 10 perc |
| A rugalmas készlet | storage_limit | Tárolási kapacitása | Átlagos | 10 perc |
| A rugalmas készlet | eDTU_used | felhasznált edtu-ra | Átlagos | 10 perc |
| A rugalmas készlet | storage_used | Felhasznált tárterület | Átlagos | 10 perc |
||||||               
| SQL data warehouse-bA | cpu_percent | Processzorhasználat (%) | Átlagos | 10 perc |
| SQL data warehouse-bA | physical_data_read_percent | Adat IO kihasználtsága (%) | Átlagos | 10 perc |
| SQL data warehouse-bA | Tárolás | Adatbázis teljes mérete | Maximális | 10 perc |
| SQL data warehouse-bA | connection_successful | Sikeres kapcsolatok | Összes | 10 perc |
| SQL data warehouse-bA | connection_failed | Nem sikerült kapcsolatok | Összes | 10 perc |
| SQL data warehouse-bA | blocked_by_firewall | Tiltsa le tűzfal | Összes | 10 perc |
| SQL data warehouse-bA | service_level_objective | Szolgáltatási szint célkitűzésének hello adatbázis | Összes | 10 perc |
| SQL data warehouse-bA | dwu_limit | dwu korlátot | Maximális | 10 perc |
| SQL data warehouse-bA | dwu_consumption_percent | DWU százalékos aránya | Átlagos | 10 perc |
| SQL data warehouse-bA | dwu_used | A DWU használt | Átlagos | 10 perc |
||||||


## <a name="next-steps"></a>Következő lépések
* [Az Azure Figyelés áttekintése](../monitoring-and-diagnostics/monitoring-overview.md) például hello típusú információkat gyűjt, és figyelheti.
* További információ [konfigurálása webhookokkal a riasztások](../monitoring-and-diagnostics/insights-webhooks-alerts.md).
* Első egy [diagnosztikai naplók áttekintése](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) és begyűjtése részletes nagyon gyakori a szolgáltatásban.
* Első egy [metrikák gyűjtemény áttekintése](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) toomake meg arról, hogy a szolgáltatás megfelelően üzemel és rugalmas.
