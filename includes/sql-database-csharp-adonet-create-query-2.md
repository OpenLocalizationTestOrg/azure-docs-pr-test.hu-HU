
<a name="cs_0_csharpprogramexample_h2"/>

## <a name="c-program-example"></a><span data-ttu-id="27913-101">C# programban – példa</span><span class="sxs-lookup"><span data-stu-id="27913-101">C# program example</span></span>

<span data-ttu-id="27913-102">a cikk hello következő szakaszaiban jelent-e egy C# programban, amely használja az ADO.NET toosend Transact-SQL utasítás toohello SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="27913-102">hello next sections of this article present a C# program that uses ADO.NET toosend Transact-SQL statements toohello SQL database.</span></span> <span data-ttu-id="27913-103">hello C# programban hello a következő műveleteket hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="27913-103">hello C# program performs hello following actions:</span></span>

1. <span data-ttu-id="27913-104">[Tooour SQL-adatbázis ADO.NET használatával csatlakozik](#cs_1_connect).</span><span class="sxs-lookup"><span data-stu-id="27913-104">[Connects tooour SQL database using ADO.NET](#cs_1_connect).</span></span>
2. <span data-ttu-id="27913-105">[Táblák létrehozása](#cs_2_createtables).</span><span class="sxs-lookup"><span data-stu-id="27913-105">[Creates tables](#cs_2_createtables).</span></span>
3. <span data-ttu-id="27913-106">[T-SQL INSERT utasítás kiadása tölti fel adatokkal, hello táblák](#cs_3_insert).</span><span class="sxs-lookup"><span data-stu-id="27913-106">[Populates hello tables with data, by issuing T-SQL INSERT statements](#cs_3_insert).</span></span>
4. <span data-ttu-id="27913-107">[Frissíti az adatokat a csatlakozzon használatával](#cs_4_updatejoin).</span><span class="sxs-lookup"><span data-stu-id="27913-107">[Updates data by use of a join](#cs_4_updatejoin).</span></span>
5. <span data-ttu-id="27913-108">[Törli az adatokat a csatlakozzon használatával](#cs_5_deletejoin).</span><span class="sxs-lookup"><span data-stu-id="27913-108">[Deletes data by use of a join](#cs_5_deletejoin).</span></span>
6. <span data-ttu-id="27913-109">[Kiválasztja a adatsorok használata illesztés](#cs_6_selectrows).</span><span class="sxs-lookup"><span data-stu-id="27913-109">[Selects data rows by use of a join](#cs_6_selectrows).</span></span>
7. <span data-ttu-id="27913-110">(Amely elutasítja azokat a tempdb ideiglenes táblák bármelyikét) hello kapcsolat bezárása.</span><span class="sxs-lookup"><span data-stu-id="27913-110">Closes hello connection (which drops any temporary tables from tempdb).</span></span>

<span data-ttu-id="27913-111">hello C# programban tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="27913-111">hello C# program contains:</span></span>

- <span data-ttu-id="27913-112">C# kóddal tooconnect toohello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="27913-112">C# code tooconnect toohello database.</span></span>
- <span data-ttu-id="27913-113">Az módszereket, amelyek hello T-SQL-forrás kóddal tér vissza.</span><span class="sxs-lookup"><span data-stu-id="27913-113">Methods that return hello T-SQL source code.</span></span>
- <span data-ttu-id="27913-114">Küldje el a T-SQL hello toohello adatbázis két módszert.</span><span class="sxs-lookup"><span data-stu-id="27913-114">Two methods that submit hello T-SQL toohello database.</span></span>

#### <a name="toocompile-and-run"></a><span data-ttu-id="27913-115">toocompile és futtatása</span><span class="sxs-lookup"><span data-stu-id="27913-115">toocompile and run</span></span>

<span data-ttu-id="27913-116">A C# program logikailag egy .cs fájl.</span><span class="sxs-lookup"><span data-stu-id="27913-116">This C# program is logically one .cs file.</span></span> <span data-ttu-id="27913-117">De itt hello program fizikailag osztva több kódblokkok, toomake minden blokk könnyebb toosee és megérteni.</span><span class="sxs-lookup"><span data-stu-id="27913-117">But here hello program is physically divided into several code blocks, toomake each block easier toosee and understand.</span></span> <span data-ttu-id="27913-118">toocompile és futtatja a programot, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="27913-118">toocompile and run this program, do hello following:</span></span>

1. <span data-ttu-id="27913-119">Hozzon létre egy C#-projektet a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="27913-119">Create a C# project in Visual Studio.</span></span>
    - <span data-ttu-id="27913-120">hello projekt típusnak kell lennie egy *konzol* alkalmazás, a következő hierarchia hello hasonlót: **sablonok** > **Visual C#** > **Klasszikus Windows asztal** > **Konzolalkalmazásból (.NET-keretrendszer)**.</span><span class="sxs-lookup"><span data-stu-id="27913-120">hello project type should be a *console* application, from something like hello following hierarchy: **Templates** > **Visual C#** > **Windows Classic Desktop** > **Console App (.NET Framework)**.</span></span>
3. <span data-ttu-id="27913-121">Hello fájlban **Program.cs**, hello kis alapszintű sornyi kód törlésére.</span><span class="sxs-lookup"><span data-stu-id="27913-121">In hello file **Program.cs**, erase hello small starter lines of code.</span></span>
3. <span data-ttu-id="27913-122">A program.cs fájlt a másolás és Beillesztés minden blokkolja, a következő hello az itt bemutatott ugyanabban a sorrendben hello.</span><span class="sxs-lookup"><span data-stu-id="27913-122">Into Program.cs, copy and paste each of hello following blocks, in hello same sequence they are presented here.</span></span>
4. <span data-ttu-id="27913-123">A program.cs fájlban Szerkesztés hello következő értékei hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="27913-123">In Program.cs, edit hello following values in hello **Main** method:</span></span>

   - <span data-ttu-id="27913-124">**CB. Adatforrás**</span><span class="sxs-lookup"><span data-stu-id="27913-124">**cb.DataSource**</span></span>
   - <span data-ttu-id="27913-125">**CD-ről. Felhasználói azonosítóját**</span><span class="sxs-lookup"><span data-stu-id="27913-125">**cd.UserID**</span></span>
   - <span data-ttu-id="27913-126">**CB. Jelszó**</span><span class="sxs-lookup"><span data-stu-id="27913-126">**cb.Password**</span></span>
   - <span data-ttu-id="27913-127">**InitialCatalog**</span><span class="sxs-lookup"><span data-stu-id="27913-127">**InitialCatalog**</span></span>

5. <span data-ttu-id="27913-128">Győződjön meg arról, hogy hello szerelvény **System.Data.dll** hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="27913-128">Verify that hello assembly **System.Data.dll** is referenced.</span></span> <span data-ttu-id="27913-129">tooverify, bontsa ki a hello **hivatkozások** hello csomópontja **Megoldáskezelőben** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="27913-129">tooverify, expand hello **References** node in hello **Solution Explorer** pane.</span></span>
6. <span data-ttu-id="27913-130">a Visual Studio toobuild hello program kattintson hello **Build** menü.</span><span class="sxs-lookup"><span data-stu-id="27913-130">toobuild hello program in Visual Studio, click hello **Build** menu.</span></span>
7. <span data-ttu-id="27913-131">a Visual Studio eszközből toorun hello program kattintson hello **Start** gombra.</span><span class="sxs-lookup"><span data-stu-id="27913-131">toorun hello program from Visual Studio, click hello **Start** button.</span></span> <span data-ttu-id="27913-132">hello jelentés kimenetében a cmd.exe ablakban jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="27913-132">hello report output is displayed in a cmd.exe window.</span></span>

> [!NOTE]
> <span data-ttu-id="27913-133">Lehetősége van hello hello T-SQL tooadd egy bevezető szerkesztési  **#**  toohello táblanevek, amely az ideiglenes táblát hoz létre **tempdb**.</span><span class="sxs-lookup"><span data-stu-id="27913-133">You have hello option of editing hello T-SQL tooadd a leading **#** toohello table names, which creates them as temporary tables in **tempdb**.</span></span> <span data-ttu-id="27913-134">Ez akkor lehet hasznos, bemutatási célokra, ha vizsgálat adatbázis érhető el.</span><span class="sxs-lookup"><span data-stu-id="27913-134">This can be useful for demonstration purposes, when no test database is available.</span></span> <span data-ttu-id="27913-135">Az ideiglenes táblák hello kapcsolat bezárása után automatikusan törli.</span><span class="sxs-lookup"><span data-stu-id="27913-135">Temporary tables are deleted automatically when hello connection closes.</span></span> <span data-ttu-id="27913-136">Az idegen kulcsok HIVATKOZÁSOKAT ideiglenes táblák nem kényszeríti ki.</span><span class="sxs-lookup"><span data-stu-id="27913-136">Any REFERENCES for foreign keys are not enforced for temporary tables.</span></span>
>

<a name="cs_1_connect"/>
### <a name="c-block-1-connect-by-using-adonet"></a><span data-ttu-id="27913-137">C# block 1: Csatlakozás az ADO.NET használatával</span><span class="sxs-lookup"><span data-stu-id="27913-137">C# block 1: Connect by using ADO.NET</span></span>

- [<span data-ttu-id="27913-138">Következő</span><span class="sxs-lookup"><span data-stu-id="27913-138">Next</span></span>](#cs_2_createtables)


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
### <a name="c-block-2-t-sql-toocreate-tables"></a><span data-ttu-id="27913-139">C# block 2: T-SQL toocreate táblák</span><span class="sxs-lookup"><span data-stu-id="27913-139">C# block 2: T-SQL toocreate tables</span></span>

- <span data-ttu-id="27913-140">[Előző](#cs_1_connect) &nbsp;  /  &nbsp; [tovább](#cs_3_insert)</span><span class="sxs-lookup"><span data-stu-id="27913-140">[Previous](#cs_1_connect) &nbsp; / &nbsp; [Next](#cs_3_insert)</span></span>

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

#### <a name="entity-relationship-diagram-erd"></a><span data-ttu-id="27913-141">Entitás kapcsolati Diagram (helyreállító lemez)</span><span class="sxs-lookup"><span data-stu-id="27913-141">Entity Relationship Diagram (ERD)</span></span>

<span data-ttu-id="27913-142">hello előző CREATE TABLE utasítás tartalmaz, amely hello **hivatkozások** kulcsszó toocreate egy *idegen kulcs* két tábla közötti kapcsolat (FK).</span><span class="sxs-lookup"><span data-stu-id="27913-142">hello preceding CREATE TABLE statements involve hello **REFERENCES** keyword toocreate a *foreign key* (FK) relationship between two tables.</span></span>  <span data-ttu-id="27913-143">A tempdb használatakor megjegyzéssé hello `--REFERENCES` kulcsszó vezető kötőjelek két használatával.</span><span class="sxs-lookup"><span data-stu-id="27913-143">If you are using tempdb, comment out hello `--REFERENCES` keyword using a pair of leading dashes.</span></span>

<span data-ttu-id="27913-144">Ezután van Helyreállító, amely megjeleníti a hello kapcsolat hello két tábla között.</span><span class="sxs-lookup"><span data-stu-id="27913-144">Next is an ERD that displays hello relationship between hello two tables.</span></span> <span data-ttu-id="27913-145">hello #tabEmployee.DepartmentCode szereplő értékek hello *gyermek* oszlop értékei korlátozott toohello hello #tabDepartment.Department szerepel *szülő* oszlop.</span><span class="sxs-lookup"><span data-stu-id="27913-145">hello values in hello #tabEmployee.DepartmentCode *child* column are limited toohello values present in hello #tabDepartment.Department *parent* column.</span></span>

![Helyreállító lemez ábrázoló idegen kulcs](./media/sql-database-csharp-adonet-create-query-2/erd-dept-empl-fky-2.png)


<a name="cs_3_insert"/>
### <a name="c-block-3-t-sql-tooinsert-data"></a><span data-ttu-id="27913-147">C# block 3: T-SQL tooinsert adatok</span><span class="sxs-lookup"><span data-stu-id="27913-147">C# block 3: T-SQL tooinsert data</span></span>

- <span data-ttu-id="27913-148">[Előző](#cs_2_createtables) &nbsp;  /  &nbsp; [tovább](#cs_4_updatejoin)</span><span class="sxs-lookup"><span data-stu-id="27913-148">[Previous](#cs_2_createtables) &nbsp; / &nbsp; [Next](#cs_4_updatejoin)</span></span>


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
### <a name="c-block-4-t-sql-tooupdate-join"></a><span data-ttu-id="27913-149">C# block 4: T-SQL tooupdate-csatlakozás</span><span class="sxs-lookup"><span data-stu-id="27913-149">C# block 4: T-SQL tooupdate-join</span></span>

- <span data-ttu-id="27913-150">[Előző](#cs_3_insert) &nbsp;  /  &nbsp; [tovább](#cs_5_deletejoin)</span><span class="sxs-lookup"><span data-stu-id="27913-150">[Previous](#cs_3_insert) &nbsp; / &nbsp; [Next](#cs_5_deletejoin)</span></span>


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
### <a name="c-block-5-t-sql-toodelete-join"></a><span data-ttu-id="27913-151">C# block 5: T-SQL toodelete-csatlakozás</span><span class="sxs-lookup"><span data-stu-id="27913-151">C# block 5: T-SQL toodelete-join</span></span>

- <span data-ttu-id="27913-152">[Előző](#cs_4_updatejoin) &nbsp;  /  &nbsp; [tovább](#cs_6_selectrows)</span><span class="sxs-lookup"><span data-stu-id="27913-152">[Previous](#cs_4_updatejoin) &nbsp; / &nbsp; [Next](#cs_6_selectrows)</span></span>


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
### <a name="c-block-6-t-sql-tooselect-rows"></a><span data-ttu-id="27913-153">C# block 6: T-SQL tooselect sorok</span><span class="sxs-lookup"><span data-stu-id="27913-153">C# block 6: T-SQL tooselect rows</span></span>

- <span data-ttu-id="27913-154">[Előző](#cs_5_deletejoin) &nbsp;  /  &nbsp; [tovább](#cs_6b_datareader)</span><span class="sxs-lookup"><span data-stu-id="27913-154">[Previous](#cs_5_deletejoin) &nbsp; / &nbsp; [Next](#cs_6b_datareader)</span></span>


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
### <a name="c-block-6b-executereader"></a><span data-ttu-id="27913-155">C# block 6b: ExecuteReader</span><span class="sxs-lookup"><span data-stu-id="27913-155">C# block 6b: ExecuteReader</span></span>

- <span data-ttu-id="27913-156">[Előző](#cs_6_selectrows) &nbsp;  /  &nbsp; [tovább](#cs_7_executenonquery)</span><span class="sxs-lookup"><span data-stu-id="27913-156">[Previous](#cs_6_selectrows) &nbsp; / &nbsp; [Next](#cs_7_executenonquery)</span></span>

<span data-ttu-id="27913-157">Ez a módszer akkor tervezett toorun hello T-SQL SELECT utasításhoz hello által épített **Build_6_Tsql_SelectEmployees** metódust.</span><span class="sxs-lookup"><span data-stu-id="27913-157">This method is designed toorun hello T-SQL SELECT statement that is built by hello **Build_6_Tsql_SelectEmployees** method.</span></span>


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
### <a name="c-block-7-executenonquery"></a><span data-ttu-id="27913-158">C# block 7: ExecuteNonQuery</span><span class="sxs-lookup"><span data-stu-id="27913-158">C# block 7: ExecuteNonQuery</span></span>

- <span data-ttu-id="27913-159">[Előző](#cs_6b_datareader) &nbsp;  /  &nbsp; [tovább](#cs_8_output)</span><span class="sxs-lookup"><span data-stu-id="27913-159">[Previous](#cs_6b_datareader) &nbsp; / &nbsp; [Next](#cs_8_output)</span></span>

<span data-ttu-id="27913-160">Ezt a módszert nevezik műveletekhez táblák hello adatok tartalmát módosító adatok sorokat visszatérés nélkül.</span><span class="sxs-lookup"><span data-stu-id="27913-160">This method is called for operations that modify hello data content of tables without returning any data rows.</span></span>


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
### <a name="c-block-8-actual-test-output-toohello-console"></a><span data-ttu-id="27913-161">C# block 8: tényleges vizsgálati kimeneti toohello konzol</span><span class="sxs-lookup"><span data-stu-id="27913-161">C# block 8: Actual test output toohello console</span></span>

- [<span data-ttu-id="27913-162">Előző</span><span class="sxs-lookup"><span data-stu-id="27913-162">Previous</span></span>](#cs_7_executenonquery)

<span data-ttu-id="27913-163">Ez a szakasz hello kimenete, a program elküldött toohello konzol hello rögzíti.</span><span class="sxs-lookup"><span data-stu-id="27913-163">This section captures hello output that hello program sent toohello console.</span></span> <span data-ttu-id="27913-164">Csak a hello guid értékek teszt futtatása változhat.</span><span class="sxs-lookup"><span data-stu-id="27913-164">Only hello guid values vary between test runs.</span></span>


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
