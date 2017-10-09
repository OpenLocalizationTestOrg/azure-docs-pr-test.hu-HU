
<a name="cs_0_csharpprogramexample_h2"/>

## <a name="c-program-example"></a>C# programban – példa

a cikk hello következő szakaszaiban jelent-e egy C# programban, amely használja az ADO.NET toosend Transact-SQL utasítás toohello SQL-adatbázis. hello C# programban hello a következő műveleteket hajtja végre:

1. [Tooour SQL-adatbázis ADO.NET használatával csatlakozik](#cs_1_connect).
2. [Táblák létrehozása](#cs_2_createtables).
3. [T-SQL INSERT utasítás kiadása tölti fel adatokkal, hello táblák](#cs_3_insert).
4. [Frissíti az adatokat a csatlakozzon használatával](#cs_4_updatejoin).
5. [Törli az adatokat a csatlakozzon használatával](#cs_5_deletejoin).
6. [Kiválasztja a adatsorok használata illesztés](#cs_6_selectrows).
7. (Amely elutasítja azokat a tempdb ideiglenes táblák bármelyikét) hello kapcsolat bezárása.

hello C# programban tartalmazza:

- C# kóddal tooconnect toohello adatbázis.
- Az módszereket, amelyek hello T-SQL-forrás kóddal tér vissza.
- Küldje el a T-SQL hello toohello adatbázis két módszert.

#### <a name="toocompile-and-run"></a>toocompile és futtatása

A C# program logikailag egy .cs fájl. De itt hello program fizikailag osztva több kódblokkok, toomake minden blokk könnyebb toosee és megérteni. toocompile és futtatja a programot, a következő hello:

1. Hozzon létre egy C#-projektet a Visual Studióban.
    - hello projekt típusnak kell lennie egy *konzol* alkalmazás, a következő hierarchia hello hasonlót: **sablonok** > **Visual C#** > **Klasszikus Windows asztal** > **Konzolalkalmazásból (.NET-keretrendszer)**.
3. Hello fájlban **Program.cs**, hello kis alapszintű sornyi kód törlésére.
3. A program.cs fájlt a másolás és Beillesztés minden blokkolja, a következő hello az itt bemutatott ugyanabban a sorrendben hello.
4. A program.cs fájlban Szerkesztés hello következő értékei hello **fő** módszert:

   - **CB. Adatforrás**
   - **CD-ről. Felhasználói azonosítóját**
   - **CB. Jelszó**
   - **InitialCatalog**

5. Győződjön meg arról, hogy hello szerelvény **System.Data.dll** hivatkozik. tooverify, bontsa ki a hello **hivatkozások** hello csomópontja **Megoldáskezelőben** ablaktáblán.
6. a Visual Studio toobuild hello program kattintson hello **Build** menü.
7. a Visual Studio eszközből toorun hello program kattintson hello **Start** gombra. hello jelentés kimenetében a cmd.exe ablakban jelenik meg.

> [!NOTE]
> Lehetősége van hello hello T-SQL tooadd egy bevezető szerkesztési  **#**  toohello táblanevek, amely az ideiglenes táblát hoz létre **tempdb**. Ez akkor lehet hasznos, bemutatási célokra, ha vizsgálat adatbázis érhető el. Az ideiglenes táblák hello kapcsolat bezárása után automatikusan törli. Az idegen kulcsok HIVATKOZÁSOKAT ideiglenes táblák nem kényszeríti ki.
>

<a name="cs_1_connect"/>
### <a name="c-block-1-connect-by-using-adonet"></a>C# block 1: Csatlakozás az ADO.NET használatával

- [Következő](#cs_2_createtables)


```csharp
using System;
using System.Data.SqlClient;   // System.Data.dll 
//using System.Data;           // For:  SqlDbType , ParameterDirection

namespace csharp_db_test
{
   class Program
   {
      static void Main(string[] args)
      {
         try
         {
            var cb = new SqlConnectionStringBuilder();
            cb.DataSource = "your_server.database.windows.net";
            cb.UserID = "your_user";
            cb.Password = "your_password";
            cb.InitialCatalog = "your_database";

            using (var connection = new SqlConnection(cb.ConnectionString))
            {
               connection.Open();

               Submit_Tsql_NonQuery(connection, "2 - Create-Tables",
                  Build_2_Tsql_CreateTables());

               Submit_Tsql_NonQuery(connection, "3 - Inserts",
                  Build_3_Tsql_Inserts());

               Submit_Tsql_NonQuery(connection, "4 - Update-Join",
                  Build_4_Tsql_UpdateJoin(),
                  "@csharpParmDepartmentName", "Accounting");

               Submit_Tsql_NonQuery(connection, "5 - Delete-Join",
                  Build_5_Tsql_DeleteJoin(),
                  "@csharpParmDepartmentName", "Legal");

               Submit_6_Tsql_SelectEmployees(connection);
            }
         }
         catch (SqlException e)
         {
            Console.WriteLine(e.ToString());
         }
         Console.WriteLine("View hello report output here, then press any key tooend hello program...");
         Console.ReadKey();
      }
```


<a name="cs_2_createtables"/>
### <a name="c-block-2-t-sql-toocreate-tables"></a>C# block 2: T-SQL toocreate táblák

- [Előző](#cs_1_connect) &nbsp;  /  &nbsp; [tovább](#cs_3_insert)

```csharp
      static string Build_2_Tsql_CreateTables()
      {
         return @"
DROP TABLE IF EXISTS tabEmployee;
DROP TABLE IF EXISTS tabDepartment;  -- Drop parent table last.


CREATE TABLE tabDepartment
(
   DepartmentCode  nchar(4)          not null
      PRIMARY KEY,
   DepartmentName  nvarchar(128)     not null
);

CREATE TABLE tabEmployee
(
   EmployeeGuid    uniqueIdentifier  not null  default NewId()
      PRIMARY KEY,
   EmployeeName    nvarchar(128)     not null,
   EmployeeLevel   int               not null,
   DepartmentCode  nchar(4)              null
      REFERENCES tabDepartment (DepartmentCode)  -- (REFERENCES would be disallowed on temporary tables.)
);
";
      }
```

#### <a name="entity-relationship-diagram-erd"></a>Entitás kapcsolati Diagram (helyreállító lemez)

hello előző CREATE TABLE utasítás tartalmaz, amely hello **hivatkozások** kulcsszó toocreate egy *idegen kulcs* két tábla közötti kapcsolat (FK).  A tempdb használatakor megjegyzéssé hello `--REFERENCES` kulcsszó vezető kötőjelek két használatával.

Ezután van Helyreállító, amely megjeleníti a hello kapcsolat hello két tábla között. hello #tabEmployee.DepartmentCode szereplő értékek hello *gyermek* oszlop értékei korlátozott toohello hello #tabDepartment.Department szerepel *szülő* oszlop.

![Helyreállító lemez ábrázoló idegen kulcs](./media/sql-database-csharp-adonet-create-query-2/erd-dept-empl-fky-2.png)


<a name="cs_3_insert"/>
### <a name="c-block-3-t-sql-tooinsert-data"></a>C# block 3: T-SQL tooinsert adatok

- [Előző](#cs_2_createtables) &nbsp;  /  &nbsp; [tovább](#cs_4_updatejoin)


```csharp
      static string Build_3_Tsql_Inserts()
      {
         return @"
-- hello company has these departments.
INSERT INTO tabDepartment
   (DepartmentCode, DepartmentName)
      VALUES
   ('acct', 'Accounting'),
   ('hres', 'Human Resources'),
   ('legl', 'Legal');

-- hello company has these employees, each in one department.
INSERT INTO tabEmployee
   (EmployeeName, EmployeeLevel, DepartmentCode)
      VALUES
   ('Alison'  , 19, 'acct'),
   ('Barbara' , 17, 'hres'),
   ('Carol'   , 21, 'acct'),
   ('Deborah' , 24, 'legl'),
   ('Elle'    , 15, null);
";
      }
```


<a name="cs_4_updatejoin"/>
### <a name="c-block-4-t-sql-tooupdate-join"></a>C# block 4: T-SQL tooupdate-csatlakozás

- [Előző](#cs_3_insert) &nbsp;  /  &nbsp; [tovább](#cs_5_deletejoin)


```csharp
      static string Build_4_Tsql_UpdateJoin()
      {
         return @"
DECLARE @DName1  nvarchar(128) = @csharpParmDepartmentName;  --'Accounting';


-- Promote everyone in one department (see @parm...).
UPDATE empl
   SET
      empl.EmployeeLevel += 1
   FROM
      tabEmployee   as empl
      INNER JOIN
      tabDepartment as dept ON dept.DepartmentCode = empl.DepartmentCode
   WHERE
      dept.DepartmentName = @DName1;
";
      }
```


<a name="cs_5_deletejoin"/>
### <a name="c-block-5-t-sql-toodelete-join"></a>C# block 5: T-SQL toodelete-csatlakozás

- [Előző](#cs_4_updatejoin) &nbsp;  /  &nbsp; [tovább](#cs_6_selectrows)


```csharp
      static string Build_5_Tsql_DeleteJoin()
      {
         return @"
DECLARE @DName2  nvarchar(128);
SET @DName2 = @csharpParmDepartmentName;  --'Legal';


-- Right size hello Legal department.
DELETE empl
   FROM
      tabEmployee   as empl
      INNER JOIN
      tabDepartment as dept ON dept.DepartmentCode = empl.DepartmentCode
   WHERE
      dept.DepartmentName = @DName2

-- Disband hello Legal department.
DELETE tabDepartment
   WHERE DepartmentName = @DName2;
";
      }
```



<a name="cs_6_selectrows"/>
### <a name="c-block-6-t-sql-tooselect-rows"></a>C# block 6: T-SQL tooselect sorok

- [Előző](#cs_5_deletejoin) &nbsp;  /  &nbsp; [tovább](#cs_6b_datareader)


```csharp
      static string Build_6_Tsql_SelectEmployees()
      {
         return @"
-- Look at all hello final Employees.
SELECT
      empl.EmployeeGuid,
      empl.EmployeeName,
      empl.EmployeeLevel,
      empl.DepartmentCode,
      dept.DepartmentName
   FROM
      tabEmployee   as empl
      LEFT OUTER JOIN
      tabDepartment as dept ON dept.DepartmentCode = empl.DepartmentCode
   ORDER BY
      EmployeeName;
";
      }
```


<a name="cs_6b_datareader"/>
### <a name="c-block-6b-executereader"></a>C# block 6b: ExecuteReader

- [Előző](#cs_6_selectrows) &nbsp;  /  &nbsp; [tovább](#cs_7_executenonquery)

Ez a módszer akkor tervezett toorun hello T-SQL SELECT utasításhoz hello által épített **Build_6_Tsql_SelectEmployees** metódust.


```csharp
      static void Submit_6_Tsql_SelectEmployees(SqlConnection connection)
      {
         Console.WriteLine();
         Console.WriteLine("=================================");
         Console.WriteLine("Now, SelectEmployees (6)...");

         string tsql = Build_6_Tsql_SelectEmployees();

         using (var command = new SqlCommand(tsql, connection))
         {
            using (SqlDataReader reader = command.ExecuteReader())
            {
               while (reader.Read())
               {
                  Console.WriteLine("{0} , {1} , {2} , {3} , {4}",
                     reader.GetGuid(0),
                     reader.GetString(1),
                     reader.GetInt32(2),
                     (reader.IsDBNull(3)) ? "NULL" : reader.GetString(3),
                     (reader.IsDBNull(4)) ? "NULL" : reader.GetString(4));
               }
            }
         }
      }
```


<a name="cs_7_executenonquery"/>
### <a name="c-block-7-executenonquery"></a>C# block 7: ExecuteNonQuery

- [Előző](#cs_6b_datareader) &nbsp;  /  &nbsp; [tovább](#cs_8_output)

Ezt a módszert nevezik műveletekhez táblák hello adatok tartalmát módosító adatok sorokat visszatérés nélkül.


```csharp
      static void Submit_Tsql_NonQuery(
         SqlConnection connection,
         string tsqlPurpose,
         string tsqlSourceCode,
         string parameterName = null,
         string parameterValue = null
         )
      {
         Console.WriteLine();
         Console.WriteLine("=================================");
         Console.WriteLine("T-SQL too{0}...", tsqlPurpose);

         using (var command = new SqlCommand(tsqlSourceCode, connection))
         {
            if (parameterName != null)
            {
               command.Parameters.AddWithValue(  // Or, use SqlParameter class.
                  parameterName,
                  parameterValue);
            }
            int rowsAffected = command.ExecuteNonQuery();
            Console.WriteLine(rowsAffected + " = rows affected.");
         }
      }
   } // EndOfClass
}
```


<a name="cs_8_output"/>
### <a name="c-block-8-actual-test-output-toohello-console"></a>C# block 8: tényleges vizsgálati kimeneti toohello konzol

- [Előző](#cs_7_executenonquery)

Ez a szakasz hello kimenete, a program elküldött toohello konzol hello rögzíti. Csak a hello guid értékek teszt futtatása változhat.


```text
[C:\csharp_db_test\csharp_db_test\bin\Debug\]
>> csharp_db_test.exe

=================================
Now, CreateTables (10)...

=================================
Now, Inserts (20)...

=================================
Now, UpdateJoin (30)...
2 rows affected, by UpdateJoin.

=================================
Now, DeleteJoin (40)...

=================================
Now, SelectEmployees (50)...
0199be49-a2ed-4e35-94b7-e936acf1cd75 , Alison , 20 , acct , Accounting
f0d3d147-64cf-4420-b9f9-76e6e0a32567 , Barbara , 17 , hres , Human Resources
cf4caede-e237-42d2-b61d-72114c7e3afa , Carol , 22 , acct , Accounting
cdde7727-bcfd-4f72-a665-87199c415f8b , Elle , 15 , NULL , NULL

[C:\csharp_db_test\csharp_db_test\bin\Debug\]
>>
```
