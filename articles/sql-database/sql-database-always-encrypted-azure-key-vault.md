---
title: "Mindig titkosítja: SQL-adatbázishoz – az Azure Key Vault |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan toosecure adatok titkosítás használata az SQL-adatbázisban lévő bizalmas adatokat hello mindig titkosítja az SQL Server Management Studio varázsló. Az olyan utasításokat is tartalmaz, amely bemutatja, hogyan toostore minden Azure Key Vault a titkosítási kulcs."
keywords: "adatok titkosítását, a titkosítási kulcs, a felhő titkosítás"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: cgronlun
ms.assetid: 6ca16644-5969-497b-a413-d28c3b835c9b
ms.service: sql-database
ms.custom: security
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: sstein
ms.openlocfilehash: 8226bfef584e979643f5bb0747d4df16569f8204
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-azure-key-vault"></a>Mindig titkosítja: Az SQL-adatbázis bizalmas adatok védelmét, és a titkosítási kulcsok tárolása az Azure Key Vault

Ez a cikk bemutatja, hogyan toosecure bizalmas adatait egy SQL-adatbázis használati ideje az adattitkosítás hello segítségével [varázsló mindig titkosítja](https://msdn.microsoft.com/library/mt459280.aspx) a [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx). Az olyan utasításokat is tartalmaz, amely bemutatja, hogyan toostore minden Azure Key Vault a titkosítási kulcs.

Titkosított mindig egy új adatok titkosítás technológia az Azure SQL Database, és, amely segít az SQL Server hello kiszolgálón inaktív bizalmas adat védelme adatátviteli ügyfél és kiszolgáló között, és miközben hello adatok használatban van. Mindig titkosított biztosítja, hogy bizalmas adatok soha nem jelenik meg szövegként hello adatbázis rendszerében. Miután konfigurálta az adattitkosítást, csak az ügyfélalkalmazások vagy toohello hívóbetűket alkalmazáskiszolgálókra férhet hozzá egyszerű szöveges adatokhoz. Részletes információkért lásd: [(adatbázismotor) mindig titkosítja](https://msdn.microsoft.com/library/mt163865.aspx).

Miután konfigurálta a hello adatbázis toouse mindig titkosítja, C# nyelven íródtak a Visual Studio toowork hello titkosított adatok hoz létre egy ügyfélalkalmazást.

Kövesse az ebben a cikkben hello lépéseket, és megtudhatja, hogyan tooset mentése mindig titkosítja az Azure SQL-adatbázis. Ebben a cikkben megtudhatja, hogyan tooperform hello a következő feladatokat:

* Hello mindig titkosítja varázsló használata a szolgáltatáshoz az SSMS toocreate [mindig titkosított kulcsok](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).
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
* [Az Azure PowerShell](/powershell/azure/overview), 1.0-ás vagy újabb verziója. Típus **(Get-Module azure - ListAvailable). Verzió** toosee PowerShell melyik verzióját futtatja.

## <a name="enable-your-client-application-tooaccess-hello-sql-database-service"></a>Az ügyfél alkalmazás tooaccess hello SQL Database szolgáltatás engedélyezése
Engedélyeznie kell az ügyfél alkalmazás tooaccess hello SQL Database szolgáltatás hello szükséges hitelesítés és az hello beállításával *ClientId* és *titkos* , hogy szüksége lesz a tooauthenticate a következő kód hello az alkalmazást.

1. Nyissa meg hello [a klasszikus Azure portálon](http://manage.windowsazure.com).
2. Válassza ki **Active Directory** , és kattintson az alkalmazás által használt hello Active Directory-példányban.
3. Kattintson a **alkalmazások**, és kattintson a **hozzáadása**.
4. Írja be az alkalmazás nevét (például: *myClientApp*) elemre, jelölje be **WEBALKALMAZÁS**, és kattintson a nyílra toocontinue hello.
5. A hello **SIGN-ON URL** és **APP ID URI** adhatja meg egy érvényes URL-címet (például *http://myClientApp*) és a folytatáshoz.
6. Kattintson a **KONFIGURÁLÁSA**.
7. Másolás a **ügyfél-azonosító**. (Később szüksége lesz ezt az értéket a kódban.)
8. A hello **kulcsok** szakaszban jelölje be **1 év** hello a **időtartam kiválasztása** legördülő listában. (Fogja másolni hello kulcs, miután menti a 13.)
9. Görgessen le, majd kattintson a **alkalmazás hozzáadása**.
10. Hagyja **megjelenítése** túl beállítása**Microsoft Apps** válassza **Microsoft Azure szolgáltatásfelügyeleti API**. Kattintson a pipa toocontinue hello.
11. Válassza ki **Azure szolgáltatásfelügyelet eléréséhez...**  a hello **delegált engedélyek** legördülő listából.
12. Kattintson a **SAVE** (Mentés) gombra.
13. Hello mentése befejeződik, után másolása hello kulcsérték hello **kulcsok** szakasz. (Később szüksége lesz ezt az értéket a kódban.)

## <a name="create-a-key-vault-toostore-your-keys"></a>A kulcstároló toostore a kulcsok létrehozása
Most, hogy az ügyfél alkalmazás van konfigurálva, és az ügyfél-Azonosítóval rendelkezik, idő toocreate kulcstároló, és a hozzáférési házirend konfigurálása a szolgáltatást, és az alkalmazás számára hello tároló titkos kulcsokat (hello mindig titkosított kulcsok). Hello *létrehozása*, *beolvasása*, *lista*, *bejelentkezési*, *ellenőrizze*, *wrapKey*, és *unwrapKey* engedélyekre szükség, egy új oszlop főkulcsának létrehozásához és az SQL Server Management Studio titkosítási beállításának.

Gyorsan létrehozhat egy kulcstartót hello a következő parancsfájl futtatásával. Ezek a parancsmagok és további információ a létrehozásáról és konfigurálásáról a kulcstároló részletes ismertetése [Ismerkedés az Azure Key Vault](../key-vault/key-vault-get-started.md).

    $subscriptionName = '<your Azure subscription name>'
    $userPrincipalName = '<username@domain.com>'
    $clientId = '<client ID that you copied in step 7 above>'
    $resourceGroupName = '<resource group name>'
    $location = '<datacenter location>'
    $vaultName = 'AeKeyVault'


    Login-AzureRmAccount
    $subscriptionId = (Get-AzureRmSubscription -SubscriptionName $subscriptionName).Id
    Set-AzureRmContext -SubscriptionId $subscriptionId

    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    New-AzureRmKeyVault -VaultName $vaultName -ResourceGroupName $resourceGroupName -Location $location

    Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName -ResourceGroupName $resourceGroupName -PermissionsToKeys create,get,wrapKey,unwrapKey,sign,verify,list -UserPrincipalName $userPrincipalName
    Set-AzureRmKeyVaultAccessPolicy  -VaultName $vaultName  -ResourceGroupName $resourceGroupName -ServicePrincipalName $clientId -PermissionsToKeys get,wrapKey,unwrapKey,sign,verify,list




## <a name="create-a-blank-sql-database"></a>Üres SQL-adatbázis létrehozása
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. Nyissa meg túl**új** > **adatok + tárolás** > **SQL-adatbázis**.
3. Hozzon létre egy **üres** nevű adatbázis **klinikán** egy új vagy meglévő kiszolgálóra. Hogyan toocreate egy adatbázis hello Azure-portálon: kapcsolatos részletes utasításokat a [az első Azure SQL-adatbázis](sql-database-get-started-portal.md).
   
    ![Hozzon létre egy üres adatbázist](./media/sql-database-always-encrypted-azure-key-vault/create-database.png)

Akkor lesz szüksége hello kapcsolati karakterlánc hello oktatóanyag későbbi részében, hello adatbázis létrehozása után Tallózás toohello új klinikán adatbázis, és másolja hello kapcsolati karakterláncot. Bármikor kaphat a hello kapcsolati karakterláncot, de egyszerű toocopy legyen hello Azure-portálon.

1. Nyissa meg túl**SQL-adatbázisok** > **klinikán** > **adatbázis-kapcsolati karakterláncok megjelenítése**.
2. Másolja a kapcsolati karakterláncot hello **ADO.NET**.
   
    ![Másolja a hello kapcsolati karakterlánc](./media/sql-database-always-encrypted-azure-key-vault/connection-strings.png)

## <a name="connect-toohello-database-with-ssms"></a>Csatlakozás toohello adatbázis ssms alkalmazásával
Nyissa meg a szolgáltatáshoz az SSMS és toohello kiszolgáló kapcsolódni hello klinikán adatbázishoz.

1. Nyissa meg a szolgáltatáshoz az SSMS. (Nyissa meg túl**Connect** > **adatbázismotor** tooopen hello **tooServer csatlakozás** ablakot, ha nem, akkor a megnyitott.)
2. Adja meg a kiszolgáló nevét és hitelesítő adatait. hello kiszolgálónév hello SQL adatbázis paneljén található, és a kapcsolati karakterláncban hello korábban kimásolt. Hello teljes kiszolgáló neve, beleértve a *database.windows.net*.
   
    ![Másolja a hello kapcsolati karakterlánc](./media/sql-database-always-encrypted-azure-key-vault/ssms-connect.png)

Ha hello **Új tűzfalszabály** ablak nyílik tooAzure bejelentkezhet, és lehetővé teszik az SSMS, hozzon létre egy új tűzfalszabályt.

## <a name="create-a-table"></a>Tábla létrehozása
Ebben a szakaszban egy tábla toohold beteg adatokat hoz létre. Nincs kezdetben titkosított--állít titkosítási hello a következő szakaszban.

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
SSMS egy varázsló, amellyel könnyen beállítható mindig titkosítja hello oszlop főkulcs, az oszlop titkosítási kulcsának és a titkosított oszlopokat beállítása az Ön által itt.

1. Bontsa ki a **adatbázisok** > **klinikán** > **táblák**.
2. Kattintson a jobb gombbal hello **betegek** tábla, és válassza ki **titkosítása oszlopok** tooopen hello mindig titkosítja varázsló:
   
    ![Oszlopok titkosítása](./media/sql-database-always-encrypted-azure-key-vault/encrypt-columns.png)

hello mindig titkosítja varázsló tartalmazza a következő szakaszok hello: **Oszlopválasztás**, **főkulcs konfigurációs**, **érvényesítési**, és **összegzése** .

### <a name="column-selection"></a>Oszlop kiválasztása
Kattintson a **következő** a hello **bemutatása** lap tooopen hello **Oszlopválasztás** lap. Ezen a lapon kiválaszthatja oszlopok tooencrypt, kívánt [hello típusú titkosítást, és milyen oszlop titkosítási kulcsának (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) toouse.

Titkosítani **SSN** és **születési dátumot** minden türelmet adatait. hello SSN oszlop determinisztikus titkosítás, mely támogatja a egyenlőség keresések, társítások és csoportosítás fogja használni. hello születési dátumot oszlop véletlenszerű titkosítás, mely nem támogatja a műveleteket fogja használni.

Set hello **titkosítási típus** hello SSN oszlop túl**Deterministic** és születési dátumot oszlop túl hello**Randomized**. Kattintson a **Tovább** gombra.

![Oszlopok titkosítása](./media/sql-database-always-encrypted-azure-key-vault/column-selection.png)

### <a name="master-key-configuration"></a>A főkulcs konfiguráció
Hello **főkulcs konfigurációs** lap, ahol beállíthatja a CMK és select hello kulcstároló-szolgáltatóval hello CMK tárolásához. Jelenleg egy CMK tárolhatja hello Windows tanúsítványtárolójába, az Azure Key Vault vagy egy hardveres biztonsági modul (HSM).

Ez az oktatóanyag bemutatja, hogyan toostore az Azure Key Vault a kulcsokat.

1. Válassza ki **az Azure Key Vault**.
2. Válassza ki a kívánt kulcstároló hello hello legördülő listából.
3. Kattintson a **Tovább** gombra.

![A főkulcs konfiguráció](./media/sql-database-always-encrypted-azure-key-vault/master-key-configuration.png)

### <a name="validation"></a>Ellenőrzés
Hello oszlopok most titkosítására, vagy egy PowerShell-parancsfájl toorun később mentse. A jelen oktatóanyag esetében válassza ki a **toofinish továbblépni** kattintson **következő**.

### <a name="summary"></a>Összefoglalás
Győződjön meg arról, hogy hello beállításainak helyességét, és kattintson a **Befejezés** toocomplete hello beállítása mindig titkosítja.

![Összefoglalás](./media/sql-database-always-encrypted-azure-key-vault/summary.png)

### <a name="verify-hello-wizards-actions"></a>Ellenőrizze a hello varázsló műveletek
Hello varázsló befejezése után az adatbázis be van állítva mindig titkosítja. hello végre varázsló hello a következő műveleteket:

* Egy oszlop főkulcs létrehozva és tárolva az Azure Key Vault.
* Egy oszlop titkosítási kulcsának létrehozva és tárolva az Azure Key Vault.
* Konfigurált hello kijelölt oszlopok titkosításhoz. hello betegek tábla jelenleg nem tartalmaz adatokat, de most Titkosított hello a kijelölt oszlopokban szereplő adatokat.

Ellenőrizheti a szolgáltatáshoz az ssms hello kulcsok hello létrehozását kibontásával **klinikán** > **biztonsági** > **mindig a titkosított kulcsok**.

## <a name="create-a-client-application-that-works-with-hello-encrypted-data"></a>Hozzon létre egy ügyfélalkalmazást, amely kompatibilis a hello titkosított adatok
Most, hogy mindig titkosítja be van állítva, egy alkalmazás, amely elvégzi hozhat létre *beszúrása* és *kiválasztja* hello a titkosított oszlopokat.  

> [!IMPORTANT]
> Az alkalmazást kell használnia [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) történő átadásakor egyszerű szöveges adatok toohello kiszolgáló mindig titkosítja oszlopokkal objektumokat. Értékek szövegkonstansban SqlParameter objektumok használata nélkül átadja azt eredményezi, hogy kivételt.
> 
> 

1. Nyissa meg a Visual Studio, és hozzon létre egy új C# **Konzolalkalmazás** (Visual Studio 2015-ös vagy korábbi) vagy **Konzolalkalmazás (.NET-keretrendszer)** (Visual Studio 2017 és újabb). Ellenőrizze, hogy túl van-e állítva a projekt**.NET-keretrendszer 4.6** vagy újabb.
2. Név hello projekt **AlwaysEncryptedConsoleAKVApp** kattintson **OK**.
3. Telepítse a következő NuGet-csomagok túl címen hello**eszközök** > **NuGet-Csomagkezelő** > **Csomagkezelő konzol**.

E két sornyi kód futtatása hello Csomagkezelő konzol.

    Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory



## <a name="modify-your-connection-string-tooenable-always-encrypted"></a>A kapcsolati karakterlánc tooenable mindig titkosítja módosítása
Ez a szakasz azt ismerteti, hogyan tooenable mindig titkosítja az adatbázis-kapcsolati karakterláncban.

tooenable mindig titkosítja, kell tooadd hello **Oszloptitkosítási beállítás** kulcsszó tooyour kapcsolati karakterláncot, és állítsa be úgy túl**engedélyezve**.

Közvetlen kapcsolat-karakterláncban hello állíthat, vagy beállíthatja a [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx). Hogyan csak a következő szakasz azt mutatja be hello mintaalkalmazáshoz hello toouse **SqlConnectionStringBuilder**.

### <a name="enable-always-encrypted-in-hello-connection-string"></a>Mindig titkosítja engedélyezése hello kapcsolati karakterlánc
Adja hozzá a következő kulcsszó tooyour kapcsolati karakterlánc hello.

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-sqlconnectionstringbuilder"></a>Mindig titkosítja a SqlConnectionStringBuilder engedélyezése
a következő kód bemutatja hogyan hello úgy, hogy mindig titkosítja tooenable [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) túl[engedélyezve](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;

## <a name="register-hello-azure-key-vault-provider"></a>Hello Azure Key Vault-szolgáltató regisztrálása
hello következő kód bemutatja, hogyan tooregister hello Azure Key Vault szolgáltató hello ADO.NET illesztővel.

    private static ClientCredential _clientCredential;

    static void InitializeAzureKeyVaultProvider()
    {
       _clientCredential = new ClientCredential(clientId, clientSecret);

       SqlColumnEncryptionAzureKeyVaultProvider azureKeyVaultProvider =
          new SqlColumnEncryptionAzureKeyVaultProvider(GetToken);

       Dictionary<string, SqlColumnEncryptionKeyStoreProvider> providers =
          new Dictionary<string, SqlColumnEncryptionKeyStoreProvider>();

       providers.Add(SqlColumnEncryptionAzureKeyVaultProvider.ProviderName, azureKeyVaultProvider);
       SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers);
    }



## <a name="always-encrypted-sample-console-application"></a>Mindig titkosított minta-Konzolalkalmazás
Ez a példa bemutatja, hogyan:

* Módosítsa a kapcsolati karakterlánc tooenable mindig titkosítja.
* Az Azure Key Vault regisztrálása hello alkalmazás kulcstároló-szolgáltatóval.  
* Adatok beszúrása hello titkosított oszlopokat.
* Válasszon ki egy olyan rekordot szűrést használ a megadott titkosított oszlop az értéket.

Cserélje le a hello tartalmát **Program.cs** a következő kód hello. Cserélje le a kapcsolati karakterlánc hello hello globális connectionString változó hello sorban hello fő metódus az érvénytelen kapcsolati karakterlánccal hello Azure-portálon a közvetlenül megelőző. Ez a hello mindössze annyi a változás toomake toothis kód van szüksége.

Futtassa a hello app toosee mindig titkosítja a művelet.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Data;
    using System.Data.SqlClient;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider;

    namespace AlwaysEncryptedConsoleAKVApp
    {
    class Program
    {
        // Update this line with your Clinic database connection string from hello Azure portal.
        static string connectionString = @"<connection string from hello portal>";
        static string clientId = @"<client id from step 7 above>";
        static string clientSecret = "<key from step 13 above>";


        static void Main(string[] args)
        {
            InitializeAzureKeyVaultProvider();

            Console.WriteLine("Signed in as: " + _clientCredential.ClientId);

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

            InsertPatient(new Patient()
            {
                SSN = "999-99-0001",
                FirstName = "Orlando",
                LastName = "Gee",
                BirthDate = DateTime.Parse("01/04/1964")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0002",
                FirstName = "Keith",
                LastName = "Harris",
                BirthDate = DateTime.Parse("06/20/1977")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0003",
                FirstName = "Donna",
                LastName = "Carreras",
                BirthDate = DateTime.Parse("02/09/1973")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0004",
                FirstName = "Janet",
                LastName = "Gates",
                BirthDate = DateTime.Parse("08/31/1985")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0005",
                FirstName = "Lucy",
                LastName = "Harrington",
                BirthDate = DateTime.Parse("05/06/1993")
            });


            // Fetch and display all patients.
            Console.WriteLine(Environment.NewLine + "All hello records currently in hello Patients table:");

            foreach (Patient patient in SelectAllPatients())
            {
                Console.WriteLine(patient.FirstName + " " + patient.LastName + "\tSSN: " + patient.SSN + "\tBirthdate: " + patient.BirthDate);
            }

            // Get patients by SSN.
            Console.WriteLine(Environment.NewLine + "Now lets locate records by searching hello encrypted SSN column.");

            string ssn;

            // This very simple validation only checks that hello user entered 11 characters.
            // In production be sure toocheck all user input and use hello best validation for your specific application.
            do
            {
                Console.WriteLine("Please enter a valid SSN (ex. 999-99-0003):");
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


        private static ClientCredential _clientCredential;

        static void InitializeAzureKeyVaultProvider()
        {

            _clientCredential = new ClientCredential(clientId, clientSecret);

            SqlColumnEncryptionAzureKeyVaultProvider azureKeyVaultProvider =
              new SqlColumnEncryptionAzureKeyVaultProvider(GetToken);

            Dictionary<string, SqlColumnEncryptionKeyStoreProvider> providers =
              new Dictionary<string, SqlColumnEncryptionKeyStoreProvider>();

            providers.Add(SqlColumnEncryptionAzureKeyVaultProvider.ProviderName, azureKeyVaultProvider);
            SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers);
        }

        public async static Task<string> GetToken(string authority, string resource, string scope)
        {
            var authContext = new AuthenticationContext(authority);
            AuthenticationResult result = await authContext.AcquireTokenAsync(resource, _clientCredential);

            if (result == null)
                throw new InvalidOperationException("Failed tooobtain hello access token");
            return result.AccessToken;
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
Gyorsan ellenőrizheti, hogy hello tényleges hello kiszolgálón adattitkosítás hello betegek adatok ssms alkalmazásával lekérdezésével (az aktuális keresztül ahol **Oszloptitkosítási beállítás** még nincs engedélyezve).

Futtassa a következő lekérdezés hello klinikán adatbázison hello.

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

Láthatja, hogy hello titkosított oszlop nem tartalmaz az egyszerű szöveges adatokat.

   ![Új Konzolalkalmazás](./media/sql-database-always-encrypted-azure-key-vault/ssms-encrypted.png)

toouse SSMS tooaccess hello adatok egyszerű szövegként, hozzáadhat hello *Oszloptitkosítási beállítás = engedélyezve* paraméter toohello kapcsolat.

1. Az SSMS, kattintson a jobb gombbal a kiszolgáló **Object Explorer** válassza **Disconnect**.
2. Kattintson a **Connect** > **adatbázismotor** tooopen hello **tooServer csatlakozás** ablakot, és kattintson **beállítások**.
3. Kattintson a **további kapcsolódási paraméterek** és típus **Oszloptitkosítási beállítás = engedélyezve**.
   
    ![Új Konzolalkalmazás](./media/sql-database-always-encrypted-azure-key-vault/ssms-connection-parameter.png)
4. Futtassa a következő lekérdezés hello klinikán adatbázison hello.
   
        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;
   
     Hello egyszerű szöveges adatok titkosítva hello oszlopban láthatja.

    ![Új Konzolalkalmazás](./media/sql-database-always-encrypted-azure-key-vault/ssms-plaintext.png)


## <a name="next-steps"></a>Következő lépések
Miután létrehozott egy adatbázist, amely mindig titkosítja használ, érdemes lehet toodo hello következő:

* [Forgassa el, és a kulcsok tisztítása](https://msdn.microsoft.com/library/mt607048.aspx).
* [Már mindig titkosítja a titkosított adatok áttelepítése](https://msdn.microsoft.com/library/mt621539.aspx).

## <a name="related-information"></a>Kapcsolódó információk
* [Mindig titkosítja (ügyféloldali fejlesztés)](https://msdn.microsoft.com/library/mt147923.aspx)
* [Transzparens adattitkosítás](https://msdn.microsoft.com/library/bb934049.aspx)
* [SQL Server-titkosítás](https://msdn.microsoft.com/library/bb510663.aspx)
* [Mindig titkosított varázsló](https://msdn.microsoft.com/library/mt459280.aspx)
* [Mindig titkosított blog](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)

