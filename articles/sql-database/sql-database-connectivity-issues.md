---
title: "aaaFix egy SQL-kapcsolati hiba átmeneti hiba |} Microsoft Docs"
description: "Ismerje meg, hogyan tootroubleshoot, diagnosztizálása és megelőzése egy SQL-kapcsolati hiba vagy az Azure SQL Database átmeneti hiba. "
keywords: "SQL-kapcsolatot, kapcsolati karakterláncot, problémák, átmeneti hiba, csatlakozási hiba"
services: sql-database
documentationcenter: 
author: dalechen
manager: cshepard
editor: 
ms.assetid: efb35451-3fed-4264-bf86-72b350f67d50
ms.service: sql-database
ms.custom: develop apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: daleche
ms.openlocfilehash: d225e610b9e88170ab53ca16d615bd07220603cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-diagnose-and-prevent-sql-connection-errors-and-transient-errors-for-sql-database"></a>SQL-csatlakozási hibák és átmeneti hibák elhárítása, diagnosztizálása és elkerülése az SQL Database szolgáltatásban
Ez a cikk ismerteti, hogyan tooprevent, hibáinak elhárítása, diagnosztizálása és csatlakozási hibáinak és, hogy az ügyfélalkalmazás tapasztal, amikor hatással van az Azure SQL Database átmeneti hibák elhárítása érdekében. Ismerje meg, hogyan tooconfigure újrapróbálkozási logika, hello kapcsolati karakterlánc felépítéséhez és további kapcsolati beállításokat.

<a id="i-transient-faults" name="i-transient-faults"></a>

## <a name="transient-errors-transient-faults"></a>Átmeneti hibák átmeneti
Egy átmeneti hiba - is, átmeneti hiba - rendelkezik egy alapul szolgáló ok, amely hamarosan magától megoldódik. Az alkalmi átmeneti hibák oka amikor hello Azure rendszer gyorsan átvált hardveres erőforrások toobetter terheléselosztásához különböző munkaterhelések. A legtöbb gyakran teljes 60 másodpercnél kevesebb újrakonfigurálását eseményekből. Az újrakonfigurálás időtartam során előfordulhat, hogy kapcsolódási problémák tooAzure SQL-adatbázis. Csatlakozás SQL Database kell tooAzure alkalmazások beépített tooexpect ezek átmeneti hibák őket implementálásával ismételje meg a kód helyett, az alkalmazáshibák toousers felszínre hozza a logikai leíró.

Ha az ügyfélprogram ADO.NET használ, a program van a kevésbé hello átmeneti hiba hello throw a egy **SqlException**. Hello **szám** tulajdonság hasonlítható hello témakör hello tetején átmeneti hibák hello listája alapján: [SQL Database-ügyfélalkalmazások SQL hibakódok](sql-database-develop-error-messages.md).

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a>Kapcsolat és a parancs
Lesz hello SQL-kapcsolatot. Próbálkozzon újra, vagy létre újra, attól függően, hogy a következő hello:

* **Egy átmeneti hiba akkor fordul elő, a kapcsolódási kísérlet során**: hello kapcsolat meg kell ismételni után késleltetése néhány másodpercig.
* **Egy átmeneti hiba jelentkezik egy SQL-lekérdezés parancs közben**: hello parancsot kell nem azonnal kísérli meg újra. Ehelyett késleltetés után hello kell frissen létesíthető kapcsolat. Majd hello parancs követően újra.

<a id="j-retry-logic-transient-faults" name="j-retry-logic-transient-faults"></a>

### <a name="retry-logic-for-transient-errors"></a>Újrapróbálkozási logika átmeneti hibák esetén
Ügyfél programok, amelyek találkozik, egy átmeneti hiba is robusztusabb, ha újrapróbálkozási logika tartalmaznak.

Amikor a program a 3. fél köztes keresztül kommunikál az Azure SQL Database, lekérdezése hello gyártójánál, hogy hello köztes újrapróbálkozási logika átmeneti hibák tartalmaz-e.

<a id="principles-for-retry" name="principles-for-retry"></a>

#### <a name="principles-for-retry"></a>Az újrapróbálkozási alapelvei
* Egy kísérlet tooopen kapcsolatot meg kell ismételni, átmeneti hello hiba esetén.
* Olyan SQL SELECT utasításban, amely egy átmeneti hiba miatt sikertelen nem közvetlenül végrehajtásával lehet újrapróbálkozni.
  
  * Ehelyett a friss kapcsolatot, majd próbálkozzon újra az hello VÁLASSZA.
* Ha egy frissítés az SQL-utasítás egy átmeneti hiba miatt nem sikerül, egy friss kell kapcsolatot frissítés a rendszer ismét megkísérli hello előtt.
  
  * hello újrapróbálkozási logika győződjön meg arról, hogy a hello teljes adatbázis tranzakció befejeződött, vagy adott hello teljes tranzakció vissza lesz állítva.

#### <a name="other-considerations-for-retry"></a>Újrapróbálkozási egyéb szempontjai
* Egy parancsfájlban, amely automatikusan elindul a munkaidőn, és amely reggel, mielőtt befejezi az újrapróbálkozások között hosszú idő időközökkel toovery türelmet is biztosít.
* A felhasználói felület program kell figyelembe hello emberi legtöbben volna toogive mentése túl hosszú várakozás után.
  
  * Azonban hello megoldást nem lehet tooretry minden néhány másodpercben, mert az adott házirendnek is kéréssekkel hello rendszer kérések.

#### <a name="interval-increase-between-retries"></a>Az újrapróbálkozások közötti időköz növekedése
Azt javasoljuk, hogy addig elhalasztani, az 5 másodperc az első újrapróbálkozás előtti. Újrapróbálkozás rövidebb, mint 5 másodperces várakozás után kockázatok túlságosan hello felhőalapú szolgáltatás. Minden ezt követő újrapróbálkozásra hello késleltetés kell nő exponenciálisan növekszik, mentése tooa legfeljebb 60 másodperc.

Hello tárgyalja *blokkolási időtartam* ADO.NET használó ügyfelek számára érhető el [SQL Server készletezését (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).

Is érdemes tooset az újrapróbálkozások maximális számát, mielőtt hello program önálló leáll.

#### <a name="code-samples-with-retry-logic"></a>Az újrapróbálkozási logika mintakódok
Az újrapróbálkozási logika, a különböző programnyelveken, Kódminták webhelyen érhetők el:

* [Adatkapcsolattárak SQL Database és SQL Server](sql-database-libraries.md)

<a id="k-test-retry-logic" name="k-test-retry-logic"></a>

#### <a name="test-your-retry-logic"></a>Az újrapróbálkozási logika tesztelése
tootest az újrapróbálkozási logika kell szimulálása vagy hibát okoz, mint javítható, a program futása közben.

##### <a name="test-by-disconnecting-from-hello-network"></a>Tesztelje hello hálózati kapcsolat bontása
Az újrapróbálkozási logika tesztelheti egyike toodisconnect az ügyfélszámítógépen, a hello hálózati hello program futása közben. hello hiba a következő lesz:

* **SqlException.Number** = 11001
* Üzenet: "ilyen állomás nem található"

Hello részét először próbálkozzon újra a kísérlet, mert a program is javítsa hello helyesírási, és próbálja meg tooconnect.

a gyakorlati toomake, akkor eltávolíthatja a számítógép hello hálózatról a program indítása előtt. Majd a program felismeri a futási idő paraméter, ami miatt hello program:

1. Ideiglenesen hozzáadni átmeneti jellegű hibák tooconsider 11001 tooits listája.
2. Az első csatlakozás a megszokott módon történt kísérlet.
3. Hello után hiba lépett fel, távolítsa el a 11001 hello listáról.
4. Hello hálózatra szólítja fel a hello felhasználói tooplug hello számítógép egy üzenetet jelenít meg.
   * További végrehajtásának felfüggesztése vagy hello segítségével **Console.ReadLine** metódus vagy egy párbeszédpanelen az OK gombra. hello felhasználó megnyomja hello Enter billentyűt, miután hello számítógép hello hálózathoz csatlakoztatva.
5. Próbálja megismételni az tooconnect, a program sikeres vár.

##### <a name="test-by-misspelling-hello-database-name-when-connecting"></a>Kapcsolódás esetén helyesírási hello adatbázisnév tesztelése
A program szándékosan is hibásan hello felhasználónév hello első kapcsolódási kísérlet előtt. hello hiba a következő lesz:

* **SqlException.Number** = 18456
* Üzenet: "bejelentkezés sikertelen volt a felhasználói"WRONG_MyUserName"."

Hello részét először próbálkozzon újra a kísérlet, mert a program is javítsa hello helyesírási, és próbálja meg tooconnect.

a gyakorlati toomake, a program sikerült ismeri fel a futási idő paraméter, ami miatt hello program:

1. Ideiglenesen hozzáadni átmeneti jellegű hibák tooconsider 18456 tooits listája.
2. Szándékosan hozzáadása "WRONG_" toohello felhasználónevet.
3. Hello után hiba lépett fel, távolítsa el a 18456 hello listáról.
4. Távolítsa el a "WRONG_" hello felhasználónévben.
5. Próbálja megismételni az tooconnect, a program sikeres vár.

<a id="net-sqlconnection-parameters-for-connection-retry" name="net-sqlconnection-parameters-for-connection-retry"></a>

### <a name="net-sqlconnection-parameters-for-connection-retry"></a>Újrapróbálkozási .NET SqlConnection paramétereinek
Ha az ügyfélprogram tootooAzure SQL-adatbázis hello .NET-keretrendszer osztály használatával **System.Data.SqlClient.SqlConnection**, használjon .NET 4.6.1 vagy újabb (vagy a .NET Core), a kapcsolat újrapróbálkozási funkcióját. Hello szolgáltatás részletei [Itt](http://go.microsoft.com/fwlink/?linkid=393996).

<!--
2015-11-30, FwLink 393996 points toodn632678.aspx, which links tooa downloadable .docx related tooSqlClient and SQL Server 2014.
-->


Hello összeállításakor [kapcsolati karakterlánc](http://msdn.microsoft.com/library/System.Data.SqlClient.SqlConnection.connectionstring.aspx) a a **SqlConnection** objektum hello értékek között a következő paraméterek hello kell koordinálni:

* ConnectRetryCount &nbsp; &nbsp; *(alapértelmezett érték 1. Tartománya 0 és 255 között.)*
* A ConnectRetryInterval &nbsp; &nbsp; *(alapértelmezett értéke 1 másodperc. Tartomány: 1 – 60.)*
* Kapcsolat időtúllépése &nbsp; &nbsp; *(alapértelmezett érték 15 másodpercre. A tartománya 0 és 2147483647 között)*

Pontosabban a kiválasztott értékek győződjön hello egyenlőség igaz a következő:

* Kapcsolat időtúllépése = ConnectRetryCount * ConnectionRetryInterval

Például ha hello száma = 3, és időköz = 10 másodperc, a időtúllépés csak 29 másodperc nem elég adna hello rendszer elegendő időt a 3. és végső újrapróbálkozási: Kapcsolódás: 29 < 3 * 10.

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a>Kapcsolat és a parancs
Hello **ConnectRetryCount** és **ConnectRetryInterval** paraméterek lehetővé teszik a **SqlConnection** objektum újrapróbálkozási hello kapcsolódási műveletet szólítja fel vagy bothering nélkül a program, például a vezérlő tooyour program ad vissza. hello újrapróbálkozások hello a következő helyzetekben fordul elő:

* mySqlConnection.Open metódus hívása
* mySqlConnection.Execute metódus hívása

Nincs olyan subtlety. Egy átmeneti hiba akkor fordul elő, ha közben az *lekérdezés* végrehajtott, a **SqlConnection** objektum nem nem az újrapróbálkozási hello kapcsolódási műveletet, és hogy biztosan nem próbálja meg újra a lekérdezést. Azonban **SqlConnection** nagyon gyorsan ellenőrzések hello kapcsolat végrehajtásra a lekérdezés elküldése előtt. Ha a Gyorsellenőrzés hello észleli a kapcsolat hibája **SqlConnection** újrapróbálkozások hello kapcsolódási műveletet. Ha hello újrapróbálkozási sikeres, akkor lekérdezést továbbítja a végrehajtása.

#### <a name="should-connectretrycount-be-combined-with-application-retry-logic"></a>Kombinálható ConnectRetryCount alkalmazás újrapróbálkozási logika?
Tegyük fel, hogy az alkalmazás rendelkezik robusztus egyéni újrapróbálkozási logika. Hello újra kapcsolódási műveletet. 4 alkalommal. Ha ad hozzá **ConnectRetryInterval** és **ConnectRetryCount** = 3 tooyour kapcsolati karakterláncot, megnöveli a hello újrapróbálkozási számláló too4 * 3 = 12 újrapróbálkozik. Előfordulhat, hogy nem kíván ilyen újrapróbálkozások magas száma.

<a id="a-connection-connection-string" name="a-connection-connection-string"></a>

## <a name="connections-tooazure-sql-database"></a>Kapcsolatok tooAzure SQL-adatbázis
<a id="c-connection-string" name="c-connection-string"></a>

### <a name="connection-connection-string"></a>Kapcsolat: Kapcsolati karakterlánc
hello kapcsolati tooAzure csatlakoztatásához szükséges SQL-adatbázis karakterlánca némileg eltérő csatlakozni az SQL Server tooMicrosoft hello karakterláncból. Másolhatja az adatbázis hello kapcsolati karakterlánc hello [Azure-portálon](https://portal.azure.com/).

[!INCLUDE [sql-database-include-connection-string-20-portalshots](../../includes/sql-database-include-connection-string-20-portalshots.md)]

<a id="b-connection-ip-address" name="b-connection-ip-address"></a>

### <a name="connection-ip-address"></a>Kapcsolat: IP-cím
Konfigurálnia kell a hello adatbázis SQL server tooaccept kommunikációt hello IP-címről érkező hello számítógép, amelyen az ügyfélprogram. Tűzfalbeállítások hello keresztül hello szerkesztésével ehhez [Azure-portálon](https://portal.azure.com/).

Ha elfelejti tooconfigure hello IP-cím, a a program sikertelen lesz, és egy kényelmes hibaüzenet hello szükséges IP-címet.

[!INCLUDE [sql-database-include-ip-address-22-portal](../../includes/sql-database-include-ip-address-22-v12portal.md)]

További információkért lásd: [hogyan: SQL Database tűzfalbeállításainak konfigurálása](sql-database-configure-firewall-settings.md)

<a id="c-connection-ports" name="c-connection-ports"></a>

### <a name="connection-ports"></a>Kapcsolat: portok
Általában csak akkor kell, hogy a 1433-as port kimenő kommunikációra hello üzemeltető számítógépen akkor ügyfélprogram nyitva-e tooensure.

Például az ügyfélprogram Windows-számítógép üzemelteti, a Windows tűzfal hello hello gazdagépen lehetővé teszi tooopen 1433-as port:

1. Nyissa meg a Vezérlőpult hello
2. &gt;Minden Vezérlőpultelem
3. &gt;A Windows tűzfal
4. &gt;Speciális beállítások
5. &gt;Kimenő szabályok
6. &gt;Műveletek
7. &gt;Új szabály

Ha az ügyfélprogram egy Azure virtuális gép (VM) üzemelteti, olvassa el:<br/>[Az ADO.NET 4.5-ös és az SQL Database 1433 kívüli portokon](sql-database-develop-direct-route-ports-adonet-v12.md).

Háttér-információkat a portok és IP-cím cofiguration, lásd: [Azure SQL Database-tűzfal](sql-database-firewall-configure.md)

<a id="d-connection-ado-net-4-5" name="d-connection-ado-net-4-5"></a>

### <a name="connection-adonet-461"></a>Kapcsolat: ADO.NET 4.6.1.
Ha a program használja az ADO.NET osztályok hasonló **System.Data.SqlClient.SqlConnection** tooconnect tooAzure SQL-adatbázis, azt javasoljuk, hogy használja-e a .NET-keretrendszer 4.6.1 verzió vagy újabb verzióját.

ADO.NET 4.6.1:

* Az Azure SQL Database esetén nincs továbbfejlesztett megbízhatósági kapcsolatot hello használatával való megnyitásakor **SqlConnection.Open** metódust. Hello **nyitott** metódus most magában foglalja a legjobb elérhető újrapróbálkozási mechanizmusok válasz tootransient hibaüzenetekben, bizonyos hibák hello kapcsolat időkorláton belül.
* Kapcsolat készletezését támogatja. Ez magában foglalja egy hatékony annak ellenőrzése, hogy a kapcsolat hello működik-e a program biztosít objektum.

Egy olyan kapcsolatobjektum kapcsolat készletből használatakor javasoljuk, hogy a program ideiglenesen zárja be a hello kapcsolat nem azonnal használata esetén. A kapcsolat újbóli megnyitásával nincs költséges hello módja az új kapcsolat létrehozása.

Ha az ADO.NET 4.0-s vagy korábbi, azt javasoljuk, hogy frissítse toohello legújabb ADO.NET.

* 2015. November frissítésétől is [töltse le az ADO.NET 4.6.1](http://blogs.msdn.com/b/dotnet/archive/2015/11/30/net-framework-4-6-1-is-now-available.aspx).

<a id="e-diagnostics-test-utilities-connect" name="e-diagnostics-test-utilities-connect"></a>

## <a name="diagnostics"></a>Diagnosztika
<a id="d-test-whether-utilities-can-connect" name="d-test-whether-utilities-can-connect"></a>

### <a name="diagnostics-test-whether-utilities-can-connect"></a>Diagnosztika: Ellenőrizze, hogy csatlakozni tud-segédprogramok
Ha a program sikertelen tooconnect tooAzure SQL-adatbázist, egy diagnosztikai beállítás akkor tootry tooconnect segédprogram programmal. Ideális esetben hello segédprogram hello segítségével kapcsolódnának ugyanazt a szalagtárat, amelyek a program.

Minden Windows rendszeren ezeket a segédprogramokat próbálkozhat:

* SQL Server Management Studio (ssms.exe), amely összeköti az ADO.NET használatával.
* Az Sqlcmd.exe, amelyek használatával kapcsolódik [ODBC](http://msdn.microsoft.com/library/jj730308.aspx).

A csatlakozás után tesztelje, hogy egy rövid SQL SELECT lekérdezés működése.

<a id="f-diagnostics-check-open-ports" name="f-diagnostics-check-open-ports"></a>

### <a name="diagnostics-check-hello-open-ports"></a>Diagnosztika: Hello megnyitott portok ellenőrzése
Tegyük fel, hogy azt feltételezi, hogy a kapcsolódási kísérletek tooport problémák miatt nem működnek. A számítógépen futtathatja egy segédprogram, amely hello portbeállításokat kapcsolatos jelentések.

A Linux hello alábbi segédprogramokat hasznosak lehetnek:

* `netstat -nap`
* `nmap -sS -O 127.0.0.1`
  * (Hello példa érték toobe az IP-cím módosításához.)

A Windows hello [PortQry.exe](http://www.microsoft.com/download/details.aspx?id=17148) segédprogram akkor lehetnek hasznosak. Íme egy példa végrehajtását, amely egy Azure SQL adatbázis-kiszolgáló port helyzet hello kérdezhetők le, és amelyek futtatták hordozható számítógépen:

```
[C:\Users\johndoe\]
>> portqry.exe -n johndoesvr9.database.windows.net -p tcp -e 1433

Querying target system called:
 johndoesvr9.database.windows.net

Attempting tooresolve name tooIP address...
Name resolved too23.100.117.95

querying...
TCP port 1433 (ms-sql-s service): LISTENING

[C:\Users\johndoe\]
>>
```


<a id="g-diagnostics-log-your-errors" name="g-diagnostics-log-your-errors"></a>

### <a name="diagnostics-log-your-errors"></a>Diagnosztika: A hibák naplózására.
Probléma néha legjobb felderítette általános a minta az észlelést keresztül nap vagy hét.

Az ügyfél segít a diagnosztikai naplózás minden hibákat. Előfordulhat, hogy képes toocorrelate hello naplóbejegyzések hiba adatokkal az Azure SQL Database belső naplók saját magát.

Vállalati könyvtár 6 (EntLib60) által felügyelt .NET osztályok tooassist naplózással kínálja:

* [5 - as könnyen mint alá tartozó ki egy naplófájl: hello alkalmazás blokk-naplózás használatával](http://msdn.microsoft.com/library/dn440731.aspx)

<a id="h-diagnostics-examine-logs-errors" name="h-diagnostics-examine-logs-errors"></a>

### <a name="diagnostics-examine-system-logs-for-errors"></a>Diagnosztika: Ellenőrizze a rendszer eseménynaplóit az hibák
Hiba a lekérdezés naplók és egyéb adatokat az alábbiakban néhány Transact-SQL SELECT utasítás.

| A lekérdezési napló | Leírás |
|:--- |:--- |
| `SELECT e.*`<br/>`FROM sys.event_log AS e`<br/>`WHERE e.database_name = 'myDbName'`<br/>`AND e.event_category = 'connectivity'`<br/>`AND 2 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, e.end_time, GetUtcDate())`<br/>`ORDER BY e.event_category,`<br/>&nbsp;&nbsp;`e.event_type, e.end_time;` |Hello [sys.event_log](http://msdn.microsoft.com/library/dn270018.aspx) nézet nyújt információkat események, köztük egyes, amelyek átmeneti hibák vagy csatlakozási hibák okozhatják.<br/><br/>Ideális esetben hozhatók hello **start_time** vagy **end_time** értékek információkkal, ha az ügyfélprogram észlelt problémákat.<br/><br/>**Tipp:** csatlakoznia kell a toohello **fő** adatbázis toorun ez. |
| `SELECT c.*`<br/>`FROM sys.database_connection_stats AS c`<br/>`WHERE c.database_name = 'myDbName'`<br/>`AND 24 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, c.end_time, GetUtcDate())`<br/>`ORDER BY c.end_time;` |Hello [sys.database_connection_stats](http://msdn.microsoft.com/library/dn269986.aspx) nézet összesített számát az eseménytípusok, a további diagnosztikához is kínál.<br/><br/>**Tipp:** csatlakoznia kell a toohello **fő** adatbázis toorun ez. |

<a id="d-search-for-problem-events-in-the-sql-database-log" name="d-search-for-problem-events-in-the-sql-database-log"></a>

### <a name="diagnostics-search-for-problem-events-in-hello-sql-database-log"></a>Diagnosztika: Keresse meg a probléma események hello SQL-adatbázis naplója
Kereshet kapcsolatos probléma az Azure SQL Database hello naplója bejegyzéseket. Próbálja meg a következő Transact-SQL SELECT utasítás hello hello **fő** adatbázis:

```
SELECT
   object_name
  ,CAST(f.event_data as XML).value
      ('(/event/@timestamp)[1]', 'datetime2')                      AS [timestamp]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="error"]/value)[1]', 'int')             AS [error]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="state"]/value)[1]', 'int')             AS [state]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="is_success"]/value)[1]', 'bit')        AS [is_success]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="database_name"]/value)[1]', 'sysname') AS [database_name]
FROM
  sys.fn_xe_telemetry_blob_target_read_file('el', null, null, null) AS f
WHERE
  object_name != 'login_event'  -- Login events are numerous.
  and
  '2015-06-21' < CAST(f.event_data as XML).value
        ('(/event/@timestamp)[1]', 'datetime2')
ORDER BY
  [timestamp] DESC
;
```


#### <a name="a-few-returned-rows-from-sysfnxetelemetryblobtargetreadfile"></a>A sys.fn_xe_telemetry_blob_target_read_file néhány visszaadott sorok
Ezután van, hogyan nézhet ki egy visszaadott sort. hello null látható értékei gyakran nem null értékű a többi sort.

```
object_name                   timestamp                    error  state  is_success  database_name

database_xml_deadlock_report  2015-10-16 20:28:01.0090000  NULL   NULL   NULL        AdventureWorks
```


<a id="l-enterprise-library-6" name="l-enterprise-library-6"></a>

## <a name="enterprise-library-6"></a>Vállalati könyvtár 6
Vállalati könyvtár 6 (EntLib60) rendszer keretrendszere a .NET-osztályok, amely segít a felhőszolgáltatások, hatékony ügyfelek megvalósítása melyek egyike hello Azure SQL Database szolgáltatásban. Témakörök dedikált tooeach terület, amelyben a EntLib60 első ellátogatva segít keresheti meg:

* [Vállalati könyvtár 6 – 2013. április](http://msdn.microsoft.com/library/dn169621%28v=pandp.60%29.aspx)

Újrapróbálkozási logika átmeneti hibák kezelése pedig segít a EntLib60 egy területen:

* [4 - perseverance, a titkos kulcsot, annak összes Triumphs: átmeneti hiba kezelési alkalmazás blokk hello használata](http://msdn.microsoft.com/library/dn440719%28v=pandp.60%29.aspx)

> [!NOTE]
> hello EntLib60 forráskódja érhető el a nyilvános [letöltése](http://go.microsoft.com/fwlink/p/?LinkID=290898). A Microsoftnál nem tervek toomake további szolgáltatás frissítések és karbantartás tooEntLib frissíti.
> 
> 

<a id="entlib60-classes-for-transient-errors-and-retry" name="entlib60-classes-for-transient-errors-and-retry"></a>

### <a name="entlib60-classes-for-transient-errors-and-retry"></a>Átmeneti hibák, és próbálkozzon ismét EntLib60 osztályok
a következő EntLib60 osztályok hello különösen hasznosak újrapróbálkozási logika. Ezek a, vagy további alatt, az összes névtér hello **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:

*Hello névtérben **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:*

* **A RetryPolicy** osztály
  
  * **ExecuteAction** módszer
* **ExponentialBackoff** osztály
* **SqlDatabaseTransientErrorDetectionStrategy** osztály
* **ReliableSqlConnection** osztály
  
  * **Az ExecuteCommand** módszer

Hello névtérben **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.TestSupport**:

* **AlwaysTransientErrorDetectionStrategy** osztály
* **NeverTransientErrorDetectionStrategy** osztály

Az alábbiakban a hivatkozások tooinformation EntLib60 kapcsolatban:

* Szabad [könyv letöltése: Fejlesztői útmutató tooMicrosoft vállalati könyvtárba, 2. kiadás](http://www.microsoft.com/download/details.aspx?id=41145)
* Gyakorlati tanácsok: [ismételje meg az általános útmutatást](../best-practices-retry-general.md) az újrapróbálkozási logika egy kiváló részletes ismertető rendelkezik.
* A NuGet letöltésének [vállalati Library - alkalmazás blokk átmeneti hiba kezelő 6.0](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/)

<a id="entlib60-the-logging-block" name="entlib60-the-logging-block"></a>

### <a name="entlib60-hello-logging-block"></a>EntLib60: hello naplózás letiltása
* hello naplózás letiltása, amely lehetővé teszi magas rugalmas és konfigurálható megoldás:
  
  * Hozzon létre, és számos különböző helyek naplóüzenetek tárolja.
  * Kategorizálását, és üzenetek szűréséhez.
  * Információgyűjtés környezetfüggő, amely hasznos a Hibakeresés és nyomkövetés, illetve a naplózási és általános naplózási követelményeknek.
* hello naplózás letiltása kivonatolja hello naplózási funkciót a hello naplócél, úgy, hogy hello alkalmazáskód konzisztens, függetlenül az hello cél naplózási tároló hello helye és típusa.

További információ:: [5 –, egyszerűen mint alá tartozó ki egy naplófájl: hello alkalmazás blokk-naplózás használatával](https://msdn.microsoft.com/library/dn440731%28v=pandp.60%29.aspx)

<a id="entlib60-istransient-method-source-code" name="entlib60-istransient-method-source-code"></a>

### <a name="entlib60-istransient-method-source-code"></a>EntLib60 IsTransient metódus forráskód
Ezt követően a hello **SqlDatabaseTransientErrorDetectionStrategy** osztály, a C# hello forráskódja hello **IsTransient** metódust. hello forráskód tisztázza, hogy mely hibák toobe átmeneti jellegű, és az Ismét gombra, 2013. április frissítésétől worthy vették.

Számos **//comment** sorok el lettek távolítva a másolási tooemphasize olvashatóság érdekében.

```
public bool IsTransient(Exception ex)
{
  if (ex != null)
  {
    SqlException sqlException;
    if ((sqlException = ex as SqlException) != null)
    {
      // Enumerate through all errors found in hello exception.
      foreach (SqlError err in sqlException.Errors)
      {
        switch (err.Number)
        {
            // SQL Error Code: 40501
            // hello service is currently busy. Retry hello request after 10 seconds.
            // Code: (reason code toobe decoded).
          case ThrottlingCondition.ThrottlingErrorNumber:
            // Decode hello reason code from hello error message to
            // determine hello grounds for throttling.
            var condition = ThrottlingCondition.FromError(err);

            // Attach hello decoded values as additional attributes to
            // hello original SQL exception.
            sqlException.Data[condition.ThrottlingMode.GetType().Name] =
              condition.ThrottlingMode.ToString();
            sqlException.Data[condition.GetType().Name] = condition;

            return true;

          case 10928:
          case 10929:
          case 10053:
          case 10054:
          case 10060:
          case 40197:
          case 40540:
          case 40613:
          case 40143:
          case 233:
          case 64:
            // DBNETLIB Error Code: 20
            // hello instance of SQL Server you attempted tooconnect to
            // does not support encryption.
          case (int)ProcessNetLibErrorCode.EncryptionNotSupported:
            return true;
        }
      }
    }
    else if (ex is TimeoutException)
    {
      return true;
    }
    else
    {
      EntityException entityException;
      if ((entityException = ex as EntityException) != null)
      {
        return this.IsTransient(entityException.InnerException);
      }
    }
  }

  return false;
}
```


## <a name="next-steps"></a>Következő lépések
* Más közös Azure SQL-adatbázis kapcsolati problémák elhárítása, látogasson el [kapcsolatos problémák elhárítása kapcsolódási problémák tooAzure SQL-adatbázis](sql-database-troubleshoot-common-connection-issues.md).
* [SQL Server-kapcsolatkészlet (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx)
* [*Újrapróbálkozás* van egy Apache 2.0 licenccel rendelkező általános célú könyvtár írt újrapróbálkozás **Python**, toosimplify hello tevékenység hozzáadásának újrapróbálkozási viselkedés toojust kapcsolatos semmit.](https://pypi.python.org/pypi/retrying)

