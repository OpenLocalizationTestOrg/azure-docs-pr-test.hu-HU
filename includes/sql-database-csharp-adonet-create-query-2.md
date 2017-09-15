
<a name="cs_0_csharpprogramexample_h2"/>

## <a name="c-program-example"></a><span data-ttu-id="8989c-101">C# programban – példa</span><span class="sxs-lookup"><span data-stu-id="8989c-101">C# program example</span></span>

<span data-ttu-id="8989c-102">Ez a cikk a következő szakaszokban jelent-e az SQL database Transact-SQL-utasítások küldendő ADO.NET használó C# programot.</span><span class="sxs-lookup"><span data-stu-id="8989c-102">The next sections of this article present a C# program that uses ADO.NET to send Transact-SQL statements to the SQL database.</span></span> <span data-ttu-id="8989c-103">A C# programban a következő műveleteket hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="8989c-103">The C# program performs the following actions:</span></span>

1. <span data-ttu-id="8989c-104">[Az ADO.NET használatával SQL Database adatbázishoz kapcsolódó](#cs_1_connect).</span><span class="sxs-lookup"><span data-stu-id="8989c-104">[Connects to our SQL database using ADO.NET](#cs_1_connect).</span></span>
2. <span data-ttu-id="8989c-105">[Táblák létrehozása](#cs_2_createtables).</span><span class="sxs-lookup"><span data-stu-id="8989c-105">[Creates tables](#cs_2_createtables).</span></span>
3. <span data-ttu-id="8989c-106">[Helyezze be a T-SQL utasítás kiadása tölti fel adatokkal, a táblák](#cs_3_insert).</span><span class="sxs-lookup"><span data-stu-id="8989c-106">[Populates the tables with data, by issuing T-SQL INSERT statements](#cs_3_insert).</span></span>
4. <span data-ttu-id="8989c-107">[Frissíti az adatokat a csatlakozzon használatával](#cs_4_updatejoin).</span><span class="sxs-lookup"><span data-stu-id="8989c-107">[Updates data by use of a join](#cs_4_updatejoin).</span></span>
5. <span data-ttu-id="8989c-108">[Törli az adatokat a csatlakozzon használatával](#cs_5_deletejoin).</span><span class="sxs-lookup"><span data-stu-id="8989c-108">[Deletes data by use of a join](#cs_5_deletejoin).</span></span>
6. <span data-ttu-id="8989c-109">[Kiválasztja a adatsorok használata illesztés](#cs_6_selectrows).</span><span class="sxs-lookup"><span data-stu-id="8989c-109">[Selects data rows by use of a join](#cs_6_selectrows).</span></span>
7. <span data-ttu-id="8989c-110">Bezárja a kapcsolatot (amely elutasítja azokat az ideiglenes táblák bármelyikét tempdb).</span><span class="sxs-lookup"><span data-stu-id="8989c-110">Closes the connection (which drops any temporary tables from tempdb).</span></span>

<span data-ttu-id="8989c-111">A C# programban tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="8989c-111">The C# program contains:</span></span>

- <span data-ttu-id="8989c-112">C#-kódban az adatbázishoz való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="8989c-112">C# code to connect to the database.</span></span>
- <span data-ttu-id="8989c-113">Az módszereket, amelyek a T-SQL-forrás kóddal tér vissza.</span><span class="sxs-lookup"><span data-stu-id="8989c-113">Methods that return the T-SQL source code.</span></span>
- <span data-ttu-id="8989c-114">Küldje el a T-SQL az adatbázis két módszert.</span><span class="sxs-lookup"><span data-stu-id="8989c-114">Two methods that submit the T-SQL to the database.</span></span>

#### <a name="to-compile-and-run"></a><span data-ttu-id="8989c-115">Fordítása és futtatása</span><span class="sxs-lookup"><span data-stu-id="8989c-115">To compile and run</span></span>

<span data-ttu-id="8989c-116">A C# program logikailag egy .cs fájl.</span><span class="sxs-lookup"><span data-stu-id="8989c-116">This C# program is logically one .cs file.</span></span> <span data-ttu-id="8989c-117">De itt a program fizikailag osztva több kódblokkok valamennyi blokkja könnyebben áttekinthetők és megérteni.</span><span class="sxs-lookup"><span data-stu-id="8989c-117">But here the program is physically divided into several code blocks, to make each block easier to see and understand.</span></span> <span data-ttu-id="8989c-118">Fordítsa le, és futtassa a programot, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="8989c-118">To compile and run this program, do the following:</span></span>

1. <span data-ttu-id="8989c-119">Hozzon létre egy C#-projektet a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="8989c-119">Create a C# project in Visual Studio.</span></span>
    - <span data-ttu-id="8989c-120">A projekt típusnak kell lennie egy *konzol* alkalmazás, a következőhöz hasonlóan a következő hierarchia: **sablonok** > **Visual C#** >  **Klasszikus Windows asztal** > **Konzolalkalmazásból (.NET-keretrendszer)**.</span><span class="sxs-lookup"><span data-stu-id="8989c-120">The project type should be a *console* application, from something like the following hierarchy: **Templates** > **Visual C#** > **Windows Classic Desktop** > **Console App (.NET Framework)**.</span></span>
3. <span data-ttu-id="8989c-121">A fájl **Program.cs**, a kis alapszintű sornyi kód törlésére.</span><span class="sxs-lookup"><span data-stu-id="8989c-121">In the file **Program.cs**, erase the small starter lines of code.</span></span>
3. <span data-ttu-id="8989c-122">A program.cs fájlt másolja be a következő blokkok mindegyikének itt bemutatott ugyanabban a sorrendben.</span><span class="sxs-lookup"><span data-stu-id="8989c-122">Into Program.cs, copy and paste each of the following blocks, in the same sequence they are presented here.</span></span>
4. <span data-ttu-id="8989c-123">A program.cs fájlban szerkessze a következő értékeket a **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="8989c-123">In Program.cs, edit the following values in the **Main** method:</span></span>

   - <span data-ttu-id="8989c-124">**CB. Adatforrás**</span><span class="sxs-lookup"><span data-stu-id="8989c-124">**cb.DataSource**</span></span>
   - <span data-ttu-id="8989c-125">**CD-ről. Felhasználói azonosítóját**</span><span class="sxs-lookup"><span data-stu-id="8989c-125">**cd.UserID**</span></span>
   - <span data-ttu-id="8989c-126">**CB. Jelszó**</span><span class="sxs-lookup"><span data-stu-id="8989c-126">**cb.Password**</span></span>
   - <span data-ttu-id="8989c-127">**InitialCatalog**</span><span class="sxs-lookup"><span data-stu-id="8989c-127">**InitialCatalog**</span></span>

5. <span data-ttu-id="8989c-128">Ellenőrizze, hogy a szerelvény **System.Data.dll** hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="8989c-128">Verify that the assembly **System.Data.dll** is referenced.</span></span> <span data-ttu-id="8989c-129">Annak ellenőrzéséhez, bontsa ki a **hivatkozások** csomópontja a **Megoldáskezelőben** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="8989c-129">To verify, expand the **References** node in the **Solution Explorer** pane.</span></span>
6. <span data-ttu-id="8989c-130">A Visual Studio program létrehozásához kattintson a **Build** menü.</span><span class="sxs-lookup"><span data-stu-id="8989c-130">To build the program in Visual Studio, click the **Build** menu.</span></span>
7. <span data-ttu-id="8989c-131">A program futtatásához a Visual Studio eszközből, kattintson a **Start** gombra.</span><span class="sxs-lookup"><span data-stu-id="8989c-131">To run the program from Visual Studio, click the **Start** button.</span></span> <span data-ttu-id="8989c-132">A jelentés kimenetében a cmd.exe ablakban jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="8989c-132">The report output is displayed in a cmd.exe window.</span></span>

> [!NOTE]
> <span data-ttu-id="8989c-133">Lehetősége van a T-SQL szerkesztési hozzáadása egy bevezető  **#**  a táblanevek, amely létrehozza azokat az ideiglenes táblák **tempdb**.</span><span class="sxs-lookup"><span data-stu-id="8989c-133">You have the option of editing the T-SQL to add a leading **#** to the table names, which creates them as temporary tables in **tempdb**.</span></span> <span data-ttu-id="8989c-134">Ez akkor lehet hasznos, bemutatási célokra, ha vizsgálat adatbázis érhető el.</span><span class="sxs-lookup"><span data-stu-id="8989c-134">This can be useful for demonstration purposes, when no test database is available.</span></span> <span data-ttu-id="8989c-135">Az ideiglenes táblák automatikusan törli a kapcsolat bezárása után.</span><span class="sxs-lookup"><span data-stu-id="8989c-135">Temporary tables are deleted automatically when the connection closes.</span></span> <span data-ttu-id="8989c-136">Az idegen kulcsok HIVATKOZÁSOKAT ideiglenes táblák nem kényszeríti ki.</span><span class="sxs-lookup"><span data-stu-id="8989c-136">Any REFERENCES for foreign keys are not enforced for temporary tables.</span></span>
>

<a name="cs_1_connect"/>
### <a name="c-block-1-connect-by-using-adonet"></a><span data-ttu-id="8989c-137">C# block 1: Csatlakozás az ADO.NET használatával</span><span class="sxs-lookup"><span data-stu-id="8989c-137">C# block 1: Connect by using ADO.NET</span></span>

- [<span data-ttu-id="8989c-138">Következő</span><span class="sxs-lookup"><span data-stu-id="8989c-138">Next</span></span>](#cs_2_createtables)


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
         Console.WriteLine("View the report output here, then press any key to end the program...");
         Console.ReadKey();
      }
```


<a name="cs_2_createtables"/>
### <a name="c-block-2-t-sql-to-create-tables"></a><span data-ttu-id="8989c-139">C# block 2: T-SQL-táblák létrehozása</span><span class="sxs-lookup"><span data-stu-id="8989c-139">C# block 2: T-SQL to create tables</span></span>

- <span data-ttu-id="8989c-140">[Előző](#cs_1_connect) &nbsp;  /  &nbsp; [tovább](#cs_3_insert)</span><span class="sxs-lookup"><span data-stu-id="8989c-140">[Previous](#cs_1_connect) &nbsp; / &nbsp; [Next](#cs_3_insert)</span></span>

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

#### <a name="entity-relationship-diagram-erd"></a><span data-ttu-id="8989c-141">Entitás kapcsolati Diagram (helyreállító lemez)</span><span class="sxs-lookup"><span data-stu-id="8989c-141">Entity Relationship Diagram (ERD)</span></span>

<span data-ttu-id="8989c-142">Az előző CREATE TABLE utasítás tartalmaz, amely a **hivatkozások** kulcsszó létrehozásához egy *idegen kulcs* két tábla közötti kapcsolat (FK).</span><span class="sxs-lookup"><span data-stu-id="8989c-142">The preceding CREATE TABLE statements involve the **REFERENCES** keyword to create a *foreign key* (FK) relationship between two tables.</span></span>  <span data-ttu-id="8989c-143">Ha a TempDB adatbázist használ, megjegyzéssé a `--REFERENCES` kulcsszó használatával vezető kötőjelek két.</span><span class="sxs-lookup"><span data-stu-id="8989c-143">If you are using tempdb, comment out the `--REFERENCES` keyword using a pair of leading dashes.</span></span>

<span data-ttu-id="8989c-144">Ezután van Helyreállító, amely megjeleníti a kapcsolat a két tábla között.</span><span class="sxs-lookup"><span data-stu-id="8989c-144">Next is an ERD that displays the relationship between the two tables.</span></span> <span data-ttu-id="8989c-145">A #tabEmployee.DepartmentCode értékeinek *gyermek* oszlop szerepel a #tabDepartment.Department értékek korlátozódnak *szülő* oszlop.</span><span class="sxs-lookup"><span data-stu-id="8989c-145">The values in the #tabEmployee.DepartmentCode *child* column are limited to the values present in the #tabDepartment.Department *parent* column.</span></span>

![Helyreállító lemez ábrázoló idegen kulcs](./media/sql-database-csharp-adonet-create-query-2/erd-dept-empl-fky-2.png)


<a name="cs_3_insert"/>
### <a name="c-block-3-t-sql-to-insert-data"></a><span data-ttu-id="8989c-147">C# block 3: T-SQL adatok beszúrása</span><span class="sxs-lookup"><span data-stu-id="8989c-147">C# block 3: T-SQL to insert data</span></span>

- <span data-ttu-id="8989c-148">[Előző](#cs_2_createtables) &nbsp;  /  &nbsp; [tovább](#cs_4_updatejoin)</span><span class="sxs-lookup"><span data-stu-id="8989c-148">[Previous](#cs_2_createtables) &nbsp; / &nbsp; [Next](#cs_4_updatejoin)</span></span>


```csharp
      static string Build_3_Tsql_Inserts()
      {
         return @"
-- The company has these departments.
INSERT INTO tabDepartment
   (DepartmentCode, DepartmentName)
      VALUES
   ('acct', 'Accounting'),
   ('hres', 'Human Resources'),
   ('legl', 'Legal');

-- The company has these employees, each in one department.
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
### <a name="c-block-4-t-sql-to-update-join"></a><span data-ttu-id="8989c-149">C# block 4: T-SQL frissítési-illesztés</span><span class="sxs-lookup"><span data-stu-id="8989c-149">C# block 4: T-SQL to update-join</span></span>

- <span data-ttu-id="8989c-150">[Előző](#cs_3_insert) &nbsp;  /  &nbsp; [tovább](#cs_5_deletejoin)</span><span class="sxs-lookup"><span data-stu-id="8989c-150">[Previous](#cs_3_insert) &nbsp; / &nbsp; [Next](#cs_5_deletejoin)</span></span>


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
### <a name="c-block-5-t-sql-to-delete-join"></a><span data-ttu-id="8989c-151">C# block 5: T-SQL delete-illesztés</span><span class="sxs-lookup"><span data-stu-id="8989c-151">C# block 5: T-SQL to delete-join</span></span>

- <span data-ttu-id="8989c-152">[Előző](#cs_4_updatejoin) &nbsp;  /  &nbsp; [tovább](#cs_6_selectrows)</span><span class="sxs-lookup"><span data-stu-id="8989c-152">[Previous](#cs_4_updatejoin) &nbsp; / &nbsp; [Next](#cs_6_selectrows)</span></span>


```csharp
      static string Build_5_Tsql_DeleteJoin()
      {
         return @"
DECLARE @DName2  nvarchar(128);
SET @DName2 = @csharpParmDepartmentName;  --'Legal';


-- Right size the Legal department.
DELETE empl
   FROM
      tabEmployee   as empl
      INNER JOIN
      tabDepartment as dept ON dept.DepartmentCode = empl.DepartmentCode
   WHERE
      dept.DepartmentName = @DName2

-- Disband the Legal department.
DELETE tabDepartment
   WHERE DepartmentName = @DName2;
";
      }
```



<a name="cs_6_selectrows"/>
### <a name="c-block-6-t-sql-to-select-rows"></a><span data-ttu-id="8989c-153">C# block 6: T-SQL sorok kiválasztásához</span><span class="sxs-lookup"><span data-stu-id="8989c-153">C# block 6: T-SQL to select rows</span></span>

- <span data-ttu-id="8989c-154">[Előző](#cs_5_deletejoin) &nbsp;  /  &nbsp; [tovább](#cs_6b_datareader)</span><span class="sxs-lookup"><span data-stu-id="8989c-154">[Previous](#cs_5_deletejoin) &nbsp; / &nbsp; [Next](#cs_6b_datareader)</span></span>


```csharp
      static string Build_6_Tsql_SelectEmployees()
      {
         return @"
-- Look at all the final Employees.
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
### <a name="c-block-6b-executereader"></a><span data-ttu-id="8989c-155">C# block 6b: ExecuteReader</span><span class="sxs-lookup"><span data-stu-id="8989c-155">C# block 6b: ExecuteReader</span></span>

- <span data-ttu-id="8989c-156">[Előző](#cs_6_selectrows) &nbsp;  /  &nbsp; [tovább](#cs_7_executenonquery)</span><span class="sxs-lookup"><span data-stu-id="8989c-156">[Previous](#cs_6_selectrows) &nbsp; / &nbsp; [Next](#cs_7_executenonquery)</span></span>

<span data-ttu-id="8989c-157">Ez a módszer az célja, hogy a T-SQL SELECT utasítás által épített a **Build_6_Tsql_SelectEmployees** metódust.</span><span class="sxs-lookup"><span data-stu-id="8989c-157">This method is designed to run the T-SQL SELECT statement that is built by the **Build_6_Tsql_SelectEmployees** method.</span></span>


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
### <a name="c-block-7-executenonquery"></a><span data-ttu-id="8989c-158">C# block 7: ExecuteNonQuery</span><span class="sxs-lookup"><span data-stu-id="8989c-158">C# block 7: ExecuteNonQuery</span></span>

- <span data-ttu-id="8989c-159">[Előző](#cs_6b_datareader) &nbsp;  /  &nbsp; [tovább](#cs_8_output)</span><span class="sxs-lookup"><span data-stu-id="8989c-159">[Previous](#cs_6b_datareader) &nbsp; / &nbsp; [Next](#cs_8_output)</span></span>

<span data-ttu-id="8989c-160">Ezt a módszert nevezik műveletekhez táblák adatok tartalmát módosító adatok sorokat visszatérés nélkül.</span><span class="sxs-lookup"><span data-stu-id="8989c-160">This method is called for operations that modify the data content of tables without returning any data rows.</span></span>


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
         Console.WriteLine("T-SQL to {0}...", tsqlPurpose);

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
### <a name="c-block-8-actual-test-output-to-the-console"></a><span data-ttu-id="8989c-161">C# block 8: tényleges tesztkimenet a konzolhoz</span><span class="sxs-lookup"><span data-stu-id="8989c-161">C# block 8: Actual test output to the console</span></span>

- [<span data-ttu-id="8989c-162">Előző</span><span class="sxs-lookup"><span data-stu-id="8989c-162">Previous</span></span>](#cs_7_executenonquery)

<span data-ttu-id="8989c-163">Ez a szakasz a program elküldött a konzol kimenetét rögzíti.</span><span class="sxs-lookup"><span data-stu-id="8989c-163">This section captures the output that the program sent to the console.</span></span> <span data-ttu-id="8989c-164">A guid értékek csak tesztelési futtatják változhat.</span><span class="sxs-lookup"><span data-stu-id="8989c-164">Only the guid values vary between test runs.</span></span>


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
