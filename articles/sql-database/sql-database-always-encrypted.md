---
title: "Mindig titkosítja: Azure SQL Database - Windows-tanúsítványtároló |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan toosecure bizalmas adatokat az SQL-adatbázisok adatbázis-titkosítás használatával hello mindig titkosítja varázsló az SQL Server Management Studio (SSMS). Azt is bemutatja, hogyan toostore hello Windows tanúsítvány a titkosítási kulcsok tárolásához."
keywords: "Mindig titkosítja adatok, sql-titkosítás, az adatbázis-titkosítás, bizalmas adatok titkosításához"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: cgronlun
ms.assetid: ce7e052e-8bf6-4d7c-9204-4c6f4afeba4b
ms.service: sql-database
ms.custom: security
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/02/2017
ms.author: sstein
ms.openlocfilehash: 483f9a2120cc42b732142fc07807d3d8830a0c33
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-hello-windows-certificate-store"></a>Mindig titkosítja: Az SQL-adatbázis bizalmas adatok védelmét, és a titkosítási kulcsok tárolása hello Windows tanúsítványtároló

Ez a cikk bemutatja, hogyan toosecure bizalmas adatait egy SQL-adatbázis az adatbázis-titkosítás hello segítségével [varázsló mindig titkosítja](https://msdn.microsoft.com/library/mt459280.aspx) a [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx). Azt is bemutatja, hogyan toostore hello Windows tanúsítvány a titkosítási kulcsok tárolásához.

Mindig titkosított technológiája új adatok titkosítása az Azure SQL-adatbázis, amely segít az SQL Server hello kiszolgálón inaktív bizalmas adat védelme adatátviteli ügyfél és kiszolgáló közötti és hello adatok használata közben, bizalmas adatok soha nem biztosítására jelenik meg egyszerű szöveges hello adatbázis rendszerében. Adatok titkosítása csak az ügyfélalkalmazások vagy toohello hívóbetűket alkalmazáskiszolgálókra is férnek hozzá a titkosítatlan szöveges adatokat. Részletes információkért lásd: [(adatbázismotor) mindig titkosítja](https://msdn.microsoft.com/library/mt163865.aspx).

Miután hello adatbázis toouse mindig titkosítja, C# nyelven íródtak a Visual Studio toowork hello titkosított adatokat hoz létre egy ügyfélalkalmazást.

Hogyan kövesse a cikk toolearn hello lépéseit tooset mentése mindig titkosítja az Azure SQL-adatbázis. Ebből a cikkből megtudhatja, hogyan tooperform hello a következő feladatokat:

* Hello mindig titkosítja varázsló használata a szolgáltatáshoz az SSMS toocreate [mindig a titkosított kulcsok](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).
  * Hozzon létre egy [oszlop főkulcs (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).
  * Hozzon létre egy [oszlop titkosítási kulcsának (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).
* Hozzon létre egy adatbázis tábláinak és oszlopok titkosításához.
* Hozzon létre olyan alkalmazás, amely beszúrja, kiválasztása és hello titkosított oszlopok adatait jeleníti meg.

## <a name="prerequisites"></a>Előfeltételek
Ebben az oktatóanyagban lesz szüksége:

* Azure-fiók és -előfizetés. Ha még nincs fiókja, regisztráljon egy [ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/).
* [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) 13.0.700.242 verzió vagy újabb.
* [.NET-keretrendszer 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) vagy újabb (ügyfélszámítógépen hello).
* [Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).

## <a name="create-a-blank-sql-database"></a>Üres SQL-adatbázis létrehozása
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. Kattintson a **új** > **adatok + tárolás** > **SQL-adatbázis**.
3. Hozzon létre egy **üres** nevű adatbázis **klinikán** egy új vagy meglévő kiszolgálóra. Adatbázis létrehozása az Azure-portálon hello részletes utasításokért lásd: [az első Azure SQL-adatbázis](sql-database-get-started-portal.md).
   
    ![Hozzon létre egy üres adatbázist](./media/sql-database-always-encrypted/create-database.png)

Hello oktatóanyag későbbi részében szüksége lesz hello kapcsolati karakterláncot. Hello adatbázis létrehozását követően nyissa meg a toohello új klinikán adatbázis, és másolja hello kapcsolati karakterláncot. Bármikor kaphat a hello kapcsolati karakterláncot, de egyszerű toocopy, ha éppen hello Azure-portálon.

1. Kattintson a **SQL-adatbázisok** > **klinikán** > **adatbázis-kapcsolati karakterláncok megjelenítése**.
2. Másolja a kapcsolati karakterláncot hello **ADO.NET**.
   
    ![Másolja a hello kapcsolati karakterlánc](./media/sql-database-always-encrypted/connection-strings.png)

## <a name="connect-toohello-database-with-ssms"></a>Csatlakozás toohello adatbázis ssms alkalmazásával
Nyissa meg a szolgáltatáshoz az SSMS és toohello kiszolgáló kapcsolódni hello klinikán adatbázishoz.

1. Nyissa meg a szolgáltatáshoz az SSMS. (Kattintson **Connect** > **adatbázismotor** tooopen hello **tooServer csatlakozás** ablakot, ha még nincs nyitva).
2. Adja meg a kiszolgáló nevét és hitelesítő adatait. hello kiszolgálónév hello SQL adatbázis paneljén található, és a kapcsolati karakterláncban hello korábban kimásolt. Típus hello teljes kiszolgáló neve például *database.windows.net*.
   
    ![Másolja a hello kapcsolati karakterlánc](./media/sql-database-always-encrypted/ssms-connect.png)

Ha hello **Új tűzfalszabály** ablak nyílik tooAzure bejelentkezhet, és lehetővé teszik az SSMS, hozzon létre egy új tűzfalszabályt.

## <a name="create-a-table"></a>Tábla létrehozása
Ebben a szakaszban egy tábla toohold beteg adatokat hoz létre. Ez kezdetben--lesz normál tábla titkosítási állít hello a következő szakaszban.

1. Bontsa ki a **adatbázisok**.
2. Kattintson a jobb gombbal hello **klinikán** adatbázis, és kattintson a **új lekérdezés**.
3. Beillesztés hello Transact-SQL (T-SQL) következő hello új lekérdezési ablakba és **Execute** azt.

        CREATE TABLE [dbo].[Patients](
         [PatientId] [int] IDENTITY(1,1),
         [SSN] [char](11) NOT NULL,
         [FirstName] [nvarchar](50) NULL,
         [LastName] [nvarchar](50) NULL,
         [MiddleName] [nvarchar](50) NULL,
         [StreetAddress] [nvarchar](50) NULL,
         [City] [nvarchar](50) NULL,
         [ZipCode] [char](5) NULL,
         [State] [char](2) NULL,
         [BirthDate] [date] NOT NULL
         PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY] );
         GO


## <a name="encrypt-columns-configure-always-encrypted"></a>Titkosítani az oszlopok (mindig titkosítja konfigurálása)
SSMS biztosít egy varázsló tooeasily konfigurálása mindig titkosítja hello CMK CEK és titkosított oszlopokat beállítását meg.

1. Bontsa ki a **adatbázisok** > **klinikán** > **táblák**.
2. Kattintson a jobb gombbal hello **betegek** tábla, és válassza ki **titkosítása oszlopok** tooopen hello mindig titkosítja varázsló:
   
    ![Oszlopok titkosítása](./media/sql-database-always-encrypted/encrypt-columns.png)

hello mindig titkosítja varázsló tartalmazza a következő szakaszok hello: **Oszlopválasztás**, **főkulcs konfigurációs** (CMK) **érvényesítési**, és  **Összefoglalás**.

### <a name="column-selection"></a>Oszlop kiválasztása
Kattintson a **következő** a hello **bemutatása** lap tooopen hello **Oszlopválasztás** lap. Ezen a lapon kiválaszthatja oszlopok tooencrypt, kívánt [hello típusú titkosítást, és milyen oszlop titkosítási kulcsának (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) toouse.

Titkosítani **SSN** és **születési dátumot** minden türelmet adatait. Hello **SSN** oszlop determinisztikus titkosítás, mely támogatja a egyenlőség keresések, társítások és csoportosítás fogja használni. Hello **születési dátumot** oszlop véletlenszerű titkosítás, mely nem támogatja a műveleteket fogja használni.

Set hello **titkosítási típus** a hello **SSN** oszlop túl**Deterministic** és hello **születési dátumot** oszlop túl **Véletlenszerű**. Kattintson a **Tovább** gombra.

![Oszlopok titkosítása](./media/sql-database-always-encrypted/column-selection.png)

### <a name="master-key-configuration"></a>A főkulcs konfiguráció
Hello **főkulcs konfigurációs** lap, ahol beállíthatja a CMK és select hello kulcstároló-szolgáltatóval hello CMK tárolásához. Jelenleg egy CMK tárolhatja hello Windows tanúsítványtárolójába, az Azure Key Vault vagy egy hardveres biztonsági modul (HSM). Ez az oktatóanyag bemutatja, hogyan toostore hello Windows tanúsítvány a kulcsok tárolásához.

Ellenőrizze, hogy **Windows tanúsítványtároló** van kiválasztva, és kattintson a **következő**.

![A főkulcs konfiguráció](./media/sql-database-always-encrypted/master-key-configuration.png)

### <a name="validation"></a>Ellenőrzés
Hello oszlopok most titkosítására, vagy egy PowerShell-parancsfájl toorun később mentse. A jelen oktatóanyag esetében válassza ki a **toofinish továbblépni** kattintson **következő**.

### <a name="summary"></a>Összefoglalás
Győződjön meg arról, hogy hello beállításainak helyességét, és kattintson a **Befejezés** toocomplete hello beállítása mindig titkosítja.

![Összefoglalás](./media/sql-database-always-encrypted/summary.png)

### <a name="verify-hello-wizards-actions"></a>Ellenőrizze a hello varázsló műveletek
Hello varázsló befejezése után az adatbázis be van állítva mindig titkosítja. hello végre varázsló hello a következő műveleteket:

* Létrehozott egy CMK.
* Létrehozott egy CEK.
* Konfigurált hello kijelölt oszlopok titkosításhoz. A **betegek** tábla jelenleg nem tartalmaz adatokat, de most Titkosított hello a kijelölt oszlopokban szereplő adatokat.

Hello kulcsok szolgáltatáshoz az ssms hello létrehozását túl megnyitásával ellenőrizheti**klinikán** > **biztonsági** > **mindig a titkosított kulcsok**. Most már megtekintheti hello hello varázsló által létrehozott új kulcsokkal.

## <a name="create-a-client-application-that-works-with-hello-encrypted-data"></a>Hozzon létre egy ügyfélalkalmazást, amely kompatibilis a hello titkosított adatok
Most, hogy mindig titkosítja be van állítva, egy alkalmazás, amely elvégzi hozhat létre *beszúrása* és *kiválasztja* hello a titkosított oszlopokat. Futtassa a mintaalkalmazást hello toosuccessfully, futtatnia kell a hello ahol hello mindig titkosítja varázsló futtatása ugyanazon a számítógépen. toorun hello alkalmazás egy másik számítógépen, mindig titkosítja tanúsítványok toohello futtató számítógép hello ügyfélalkalmazás kell telepítenie.  

> [!IMPORTANT]
> Az alkalmazást kell használnia [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) történő átadásakor egyszerű szöveges adatok toohello kiszolgáló mindig titkosítja oszlopokkal objektumokat. Értékek szövegkonstansban SqlParameter objektumok használata nélkül átadja azt eredményezi, hogy kivételt.
> 
> 

1. Nyissa meg a Visual Studio, és hozzon létre egy új C# konzolalkalmazást. Ellenőrizze, hogy túl van-e állítva a projekt**.NET-keretrendszer 4.6** vagy újabb.
2. Név hello projekt **AlwaysEncryptedConsoleApp** kattintson **OK**.

![Új Konzolalkalmazás](./media/sql-database-always-encrypted/console-app.png)

## <a name="modify-your-connection-string-tooenable-always-encrypted"></a>A kapcsolati karakterlánc tooenable mindig titkosítja módosítása
Ez a szakasz azt ismerteti, hogyan tooenable mindig titkosítja az adatbázis-kapcsolati karakterláncban. Most létrehozott hello következő szakaszban "Mindig titkosítja minta-Konzolalkalmazás." hello konzolalkalmazást fog módosítani

tooenable mindig titkosítja, kell tooadd hello **Oszloptitkosítási beállítás** kulcsszó tooyour kapcsolati karakterláncot, és állítsa be úgy túl**engedélyezve**.

Közvetlen kapcsolat-karakterláncban hello állíthat, vagy beállíthatja a egy [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx). Hogyan csak a következő szakasz azt mutatja be hello mintaalkalmazáshoz hello toouse **SqlConnectionStringBuilder**.

> [!NOTE]
> Ez a hello mindössze annyi a változás egy ügyfél konkrét tooAlways titkosított szükséges. Ha egy meglévő alkalmazást, amely tárolja a kapcsolati karakterlánc külsőleg (Ez azt jelenti, hogy a konfigurációs fájlban), is képes tooenable mindig titkosítja a programkód módosítása nélkül.
> 
> 

### <a name="enable-always-encrypted-in-hello-connection-string"></a>Mindig titkosítja engedélyezése hello kapcsolati karakterlánc
Adja hozzá a következő kulcsszó tooyour kapcsolati karakterlánc hello:

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-a-sqlconnectionstringbuilder"></a>Egy SqlConnectionStringBuilder mindig titkosítva engedélyezése
hello következő kód bemutatja, hogyan úgy, hogy mindig titkosítja tooenable hello [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) túl[engedélyezve](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;



## <a name="always-encrypted-sample-console-application"></a>Mindig titkosított minta-Konzolalkalmazás
Ez a példa bemutatja, hogyan:

* Módosítsa a kapcsolati karakterlánc tooenable mindig titkosítja.
* Adatok beszúrása hello titkosított oszlopokat.
* Válasszon ki egy olyan rekordot szűrést használ a megadott titkosított oszlop az értéket.

Cserélje le a hello tartalmát **Program.cs** a következő kód hello. Cserélje le a érvényes kapcsolati karakterláncot az Azure-portálon hello hello kapcsolati karakterlánc hello globális connectionString változó hello sorban fölött hello fő metódus. Ez a hello mindössze annyi a változás toomake toothis kód van szüksége.

Futtassa a hello app toosee mindig titkosítja a művelet.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Data;
    using System.Data.SqlClient;

    namespace AlwaysEncryptedConsoleApp
    {
    class Program
    {
        // Update this line with your Clinic database connection string from hello Azure portal.
        static string connectionString = @"Replace with your connection string";

        static void Main(string[] args)
        {
            Console.WriteLine("Original connection string copied from hello Azure portal:");
            Console.WriteLine(connectionString);

            // Create a SqlConnectionStringBuilder.
            SqlConnectionStringBuilder connStringBuilder =
                new SqlConnectionStringBuilder(connectionString);

            // Enable Always Encrypted for hello connection.
            // This is hello only change specific tooAlways Encrypted
            connStringBuilder.ColumnEncryptionSetting =
                SqlConnectionColumnEncryptionSetting.Enabled;

            Console.WriteLine(Environment.NewLine + "Updated connection string with Always Encrypted enabled:");
            Console.WriteLine(connStringBuilder.ConnectionString);

            // Update hello connection string with a password supplied at runtime.
            Console.WriteLine(Environment.NewLine + "Enter server password:");
            connStringBuilder.Password = Console.ReadLine();


            // Assign hello updated connection string tooour global variable.
            connectionString = connStringBuilder.ConnectionString;


            // Delete all records toorestart this demo app.
            ResetPatientsTable();

            // Add sample data toohello Patients table.
            Console.Write(Environment.NewLine + "Adding sample patient data toohello database...");

            InsertPatient(new Patient() {
                SSN = "999-99-0001", FirstName = "Orlando", LastName = "Gee", BirthDate = DateTime.Parse("01/04/1964") });
            InsertPatient(new Patient() {
                SSN = "999-99-0002", FirstName = "Keith", LastName = "Harris", BirthDate = DateTime.Parse("06/20/1977") });
            InsertPatient(new Patient() {
                SSN = "999-99-0003", FirstName = "Donna", LastName = "Carreras", BirthDate = DateTime.Parse("02/09/1973") });
            InsertPatient(new Patient() {
                SSN = "999-99-0004", FirstName = "Janet", LastName = "Gates", BirthDate = DateTime.Parse("08/31/1985") });
            InsertPatient(new Patient() {
                SSN = "999-99-0005", FirstName = "Lucy", LastName = "Harrington", BirthDate = DateTime.Parse("05/06/1993") });


            // Fetch and display all patients.
            Console.WriteLine(Environment.NewLine + "All hello records currently in hello Patients table:");

            foreach (Patient patient in SelectAllPatients())
            {
                Console.WriteLine(patient.FirstName + " " + patient.LastName + "\tSSN: " + patient.SSN + "\tBirthdate: " + patient.BirthDate);
            }

            // Get patients by SSN.
            Console.WriteLine(Environment.NewLine + "Now let's locate records by searching hello encrypted SSN column.");

            string ssn;

            // This very simple validation only checks that hello user entered 11 characters.
            // In production be sure toocheck all user input and use hello best validation for your specific application.
            do
            {
                Console.WriteLine("Please enter a valid SSN (ex. 123-45-6789):");
                ssn = Console.ReadLine();
            } while (ssn.Length != 11);

            // hello example allows duplicate SSN entries so we will return all records
            // that match hello provided value and store hello results in selectedPatients.
            Patient selectedPatient = SelectPatientBySSN(ssn);

            // Check if any records were returned and display our query results.
            if (selectedPatient != null)
            {
                Console.WriteLine("Patient found with SSN = " + ssn);
                Console.WriteLine(selectedPatient.FirstName + " " + selectedPatient.LastName + "\tSSN: "
                    + selectedPatient.SSN + "\tBirthdate: " + selectedPatient.BirthDate);
            }
            else
            {
                Console.WriteLine("No patients found with SSN = " + ssn);
            }

            Console.WriteLine("Press Enter tooexit...");
            Console.ReadLine();
        }


        static int InsertPatient(Patient newPatient)
        {
            int returnValue = 0;

            string sqlCmdText = @"INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate])
         VALUES (@SSN, @FirstName, @LastName, @BirthDate);";

            SqlCommand sqlCmd = new SqlCommand(sqlCmdText);


            SqlParameter paramSSN = new SqlParameter(@"@SSN", newPatient.SSN);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            SqlParameter paramFirstName = new SqlParameter(@"@FirstName", newPatient.FirstName);
            paramFirstName.DbType = DbType.String;
            paramFirstName.Direction = ParameterDirection.Input;

            SqlParameter paramLastName = new SqlParameter(@"@LastName", newPatient.LastName);
            paramLastName.DbType = DbType.String;
            paramLastName.Direction = ParameterDirection.Input;

            SqlParameter paramBirthDate = new SqlParameter(@"@BirthDate", newPatient.BirthDate);
            paramBirthDate.SqlDbType = SqlDbType.Date;
            paramBirthDate.Direction = ParameterDirection.Input;

            sqlCmd.Parameters.Add(paramSSN);
            sqlCmd.Parameters.Add(paramFirstName);
            sqlCmd.Parameters.Add(paramLastName);
            sqlCmd.Parameters.Add(paramBirthDate);

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();
                }
                catch (Exception ex)
                {
                    returnValue = 1;
                    Console.WriteLine("hello following error was encountered: ");
                    Console.WriteLine(ex.Message);
                    Console.WriteLine(Environment.NewLine + "Press Enter key tooexit");
                    Console.ReadLine();
                    Environment.Exit(0);
                }
            }
            return returnValue;
        }


        static List<Patient> SelectAllPatients()
        {
            List<Patient> patients = new List<Patient>();


            SqlCommand sqlCmd = new SqlCommand(
              "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients]",
                new SqlConnection(connectionString));


            using (sqlCmd.Connection = new SqlConnection(connectionString))

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patients.Add(new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            });
                        }
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }

            return patients;
        }


        static Patient SelectPatientBySSN(string ssn)
        {
            Patient patient = new Patient();

            SqlCommand sqlCmd = new SqlCommand(
                "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN]=@SSN",
                new SqlConnection(connectionString));

            SqlParameter paramSSN = new SqlParameter(@"@SSN", ssn);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            sqlCmd.Parameters.Add(paramSSN);


            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patient = new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            };
                        }
                    }
                    else
                    {
                        patient = null;
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }
            return patient;
        }


        // This method simply deletes all records in hello Patients table tooreset our demo.
        static int ResetPatientsTable()
        {
            int returnValue = 0;

            SqlCommand sqlCmd = new SqlCommand("DELETE FROM Patients");
            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();

                }
                catch (Exception ex)
                {
                    returnValue = 1;
                }
            }
            return returnValue;
        }
    }

    class Patient
    {
        public string SSN { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public DateTime BirthDate { get; set; }
    }
    }


## <a name="verify-that-hello-data-is-encrypted"></a>Győződjön meg arról, hogy hello adattitkosítás
Gyorsan ellenőrizheti, hogy hello tényleges hello kiszolgálón adattitkosítás hello lekérdezésével **betegek** ssms alkalmazásával adatokat. (Az aktuális kapcsolat használatát ahol hello oszloptitkosítási beállítás még nincs engedélyezve.)

Futtassa a következő lekérdezés hello klinikán adatbázison hello.

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

Láthatja, hogy hello titkosított oszlop nem tartalmaz az egyszerű szöveges adatokat.

   ![Új Konzolalkalmazás](./media/sql-database-always-encrypted/ssms-encrypted.png)

toouse SSMS tooaccess hello adatok egyszerű szövegként, hozzáadhat hello **Oszloptitkosítási beállítás = engedélyezve** paraméter toohello kapcsolat.

1. Az SSMS, kattintson a jobb gombbal a kiszolgáló **Object Explorer**, és kattintson a **Disconnect**.
2. Kattintson a **Connect** > **adatbázismotor** tooopen hello **tooServer csatlakozás** ablakot, és kattintson **beállítások**.
3. Kattintson a **további kapcsolódási paraméterek** és típus **Oszloptitkosítási beállítás = engedélyezve**.
   
    ![Új Konzolalkalmazás](./media/sql-database-always-encrypted/ssms-connection-parameter.png)
4. Futtatási hello lekérdezés hello a következő **klinikán** adatbázis.
   
        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;
   
     Hello egyszerű szöveges adatok titkosítva hello oszlopban láthatja.

    ![Új Konzolalkalmazás](./media/sql-database-always-encrypted/ssms-plaintext.png)



> [!NOTE]
> Ha az SSMS (vagy bármely olyan ügyfél) egy másik számítógépről csatlakozik, nem rendelkeznek hozzáféréssel toohello titkosítási kulcsokat, és nem lesz képes toodecrypt hello adatokat.
> 
> 

## <a name="next-steps"></a>Következő lépések
Miután létrehozott egy adatbázist, amely mindig titkosítja használ, érdemes lehet toodo hello következő:

* A minta futtatásához egy másik számítógépen. Toohello titkosítási kulcsot, akkor nem rendelkezik, ezért nem lesz access toohello egyszerű szöveges adatokat, és nem futtathatók sikeresen.
* [Forgassa el, és a kulcsok tisztítása](https://msdn.microsoft.com/library/mt607048.aspx).
* [Már mindig titkosítja a titkosított adatok áttelepítése](https://msdn.microsoft.com/library/mt621539.aspx).
* [Mindig titkosítja tanúsítványok tooother ügyfél gépek telepítése](https://msdn.microsoft.com/library/mt723359.aspx#Anchor_1) (lásd a hello "Tanúsítványok elérhető tooApplications és a felhasználók így" szakaszt).

## <a name="related-information"></a>Kapcsolódó információk
* [Mindig titkosítja (ügyféloldali fejlesztés)](https://msdn.microsoft.com/library/mt147923.aspx)
* [Átlátható adattitkosítás](https://msdn.microsoft.com/library/bb934049.aspx)
* [SQL Server-titkosítás](https://msdn.microsoft.com/library/bb510663.aspx)
* [Mindig titkosított varázsló](https://msdn.microsoft.com/library/mt459280.aspx)
* [Mindig titkosított Blog](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)

