---
title: "Mindig titkosítja: Azure SQL Database - Windows-tanúsítványtároló |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan bizalmas adatok védelmének az adatbázis-titkosítás az SQL-adatbázisban az SQL Server Management Studio (SSMS) mindig titkosítja varázsló használatával. Azt is bemutatja, hogyan tárolja a titkosítási kulcsok a Windows tanúsítványtárolójában."
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
ms.openlocfilehash: d1fdfc4f739e65ff532b159eefaffe1622ad0963
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-the-windows-certificate-store"></a><span data-ttu-id="4f071-105">Mindig titkosítja: Az SQL-adatbázis bizalmas adatok védelmét, és a titkosítási kulcsok tárolása a Windows tanúsítványtároló</span><span class="sxs-lookup"><span data-stu-id="4f071-105">Always Encrypted: Protect sensitive data in SQL Database and store your encryption keys in the Windows certificate store</span></span>

<span data-ttu-id="4f071-106">A cikkből megtudhatja, mennyire biztonságos a bizalmas adatokat az SQL-adatbázisok adatbázis-titkosítás segítségével a [varázsló mindig titkosítja](https://msdn.microsoft.com/library/mt459280.aspx) a [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx).</span><span class="sxs-lookup"><span data-stu-id="4f071-106">This article shows you how to secure sensitive data in a SQL database with database encryption by using the [Always Encrypted Wizard](https://msdn.microsoft.com/library/mt459280.aspx) in [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx).</span></span> <span data-ttu-id="4f071-107">Azt is bemutatja, hogyan tárolja a titkosítási kulcsok a Windows tanúsítványtárolójában.</span><span class="sxs-lookup"><span data-stu-id="4f071-107">It also shows you how to store your encryption keys in the Windows certificate store.</span></span>

<span data-ttu-id="4f071-108">Mindig titkosított technológiája új adatok titkosítása az Azure SQL-adatbázis, amely segít az SQL Server a kiszolgálón, inaktív bizalmas adat védelme adatátviteli ügyfél és kiszolgáló közötti és az adatok használata közben, bizalmas adatok soha nem biztosítására adatbázis rendszerében szövegként jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="4f071-108">Always Encrypted is a new data encryption technology in Azure SQL Database and SQL Server that helps protect sensitive data at rest on the server, during movement between client and server, and while the data is in use, ensuring that sensitive data never appears as plaintext inside the database system.</span></span> <span data-ttu-id="4f071-109">Adatok titkosítása csak az ügyfélalkalmazások vagy a kulcsoknak access app kiszolgálók is férnek hozzá a titkosítatlan szöveges adatokat.</span><span class="sxs-lookup"><span data-stu-id="4f071-109">After you encrypt data, only client applications or app servers that have access to the keys can access plaintext data.</span></span> <span data-ttu-id="4f071-110">Részletes információkért lásd: [(adatbázismotor) mindig titkosítja](https://msdn.microsoft.com/library/mt163865.aspx).</span><span class="sxs-lookup"><span data-stu-id="4f071-110">For detailed information, see [Always Encrypted (Database Engine)](https://msdn.microsoft.com/library/mt163865.aspx).</span></span>

<span data-ttu-id="4f071-111">Miután mindig titkosítja az adatbázist, a C# a Visual Studio, a titkosított adatok hoz létre egy ügyfélalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4f071-111">After configuring the database to use Always Encrypted, you will create a client application in C# with Visual Studio to work with the encrypted data.</span></span>

<span data-ttu-id="4f071-112">Kövesse a cikkben megtudhatja, hogyan állíthat be mindig titkosítja az Azure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="4f071-112">Follow the steps in this article to learn how to set up Always Encrypted for an Azure SQL database.</span></span> <span data-ttu-id="4f071-113">Ebből a cikkből megtudhatja, hogyan a következő feladatok végezhetők el:</span><span class="sxs-lookup"><span data-stu-id="4f071-113">In this article, you will learn how to perform the following tasks:</span></span>

* <span data-ttu-id="4f071-114">A varázslóval mindig titkosítja az SSMS létrehozásához [mindig a titkosított kulcsok](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).</span><span class="sxs-lookup"><span data-stu-id="4f071-114">Use the Always Encrypted wizard in SSMS to create [Always Encrypted Keys](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).</span></span>
  * <span data-ttu-id="4f071-115">Hozzon létre egy [oszlop főkulcs (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).</span><span class="sxs-lookup"><span data-stu-id="4f071-115">Create a [Column Master Key (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).</span></span>
  * <span data-ttu-id="4f071-116">Hozzon létre egy [oszlop titkosítási kulcsának (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).</span><span class="sxs-lookup"><span data-stu-id="4f071-116">Create a [Column Encryption Key (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).</span></span>
* <span data-ttu-id="4f071-117">Hozzon létre egy adatbázis tábláinak és oszlopok titkosításához.</span><span class="sxs-lookup"><span data-stu-id="4f071-117">Create a database table and encrypt columns.</span></span>
* <span data-ttu-id="4f071-118">Hozzon létre egy alkalmazást, amely beszúrja, kiválasztása és a titkosított oszlopokban adatait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="4f071-118">Create an application that inserts, selects, and displays data from the encrypted columns.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4f071-119">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4f071-119">Prerequisites</span></span>
<span data-ttu-id="4f071-120">Ebben az oktatóanyagban lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="4f071-120">For this tutorial, you'll need:</span></span>

* <span data-ttu-id="4f071-121">Azure-fiók és -előfizetés.</span><span class="sxs-lookup"><span data-stu-id="4f071-121">An Azure account and subscription.</span></span> <span data-ttu-id="4f071-122">Ha még nincs fiókja, regisztráljon egy [ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4f071-122">If you don't have one, sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="4f071-123">[SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) 13.0.700.242 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="4f071-123">[SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) version 13.0.700.242 or later.</span></span>
* <span data-ttu-id="4f071-124">[.NET-keretrendszer 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) vagy újabb (az ügyfélszámítógépen).</span><span class="sxs-lookup"><span data-stu-id="4f071-124">[.NET Framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) or later (on the client computer).</span></span>
* <span data-ttu-id="4f071-125">[Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="4f071-125">[Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).</span></span>

## <a name="create-a-blank-sql-database"></a><span data-ttu-id="4f071-126">Üres SQL-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="4f071-126">Create a blank SQL database</span></span>
1. <span data-ttu-id="4f071-127">Jelentkezzen be az [Azure Portalra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="4f071-127">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="4f071-128">Kattintson a **új** > **adatok + tárolás** > **SQL-adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="4f071-128">Click **New** > **Data + Storage** > **SQL Database**.</span></span>
3. <span data-ttu-id="4f071-129">Hozzon létre egy **üres** nevű adatbázis **klinikán** egy új vagy meglévő kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="4f071-129">Create a **Blank** database named **Clinic** on a new or existing server.</span></span> <span data-ttu-id="4f071-130">Adatbázis létrehozása az Azure portálon részletes utasításokért lásd: [az első Azure SQL-adatbázis](sql-database-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="4f071-130">For detailed instructions about creating a database in the Azure portal, see [Your first Azure SQL database](sql-database-get-started-portal.md).</span></span>
   
    ![Hozzon létre egy üres adatbázist](./media/sql-database-always-encrypted/create-database.png)

<span data-ttu-id="4f071-132">Az oktatóanyag későbbi részében szüksége lesz a kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="4f071-132">You will need the connection string later in the tutorial.</span></span> <span data-ttu-id="4f071-133">Az adatbázis létrehozását követően nyissa meg az új klinikán adatbázishoz, és másolja a kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="4f071-133">After the database is created, go to the new Clinic database and copy the connection string.</span></span> <span data-ttu-id="4f071-134">A kapcsolati karakterlánc kaphat bármikor, de könnyen másolásához, amikor az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="4f071-134">You can get the connection string at any time, but it's easy to copy it when you're in the Azure portal.</span></span>

1. <span data-ttu-id="4f071-135">Kattintson a **SQL-adatbázisok** > **klinikán** > **adatbázis-kapcsolati karakterláncok megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="4f071-135">Click **SQL databases** > **Clinic** > **Show database connection strings**.</span></span>
2. <span data-ttu-id="4f071-136">Másolja a kapcsolati karakterláncot a **ADO.NET**.</span><span class="sxs-lookup"><span data-stu-id="4f071-136">Copy the connection string for **ADO.NET**.</span></span>
   
    ![Másolja a kapcsolati karakterláncot](./media/sql-database-always-encrypted/connection-strings.png)

## <a name="connect-to-the-database-with-ssms"></a><span data-ttu-id="4f071-138">Kapcsolódni az adatbázishoz ssms alkalmazásával</span><span class="sxs-lookup"><span data-stu-id="4f071-138">Connect to the database with SSMS</span></span>
<span data-ttu-id="4f071-139">Nyissa meg a szolgáltatáshoz az SSMS, és csatlakozzon a kiszolgálóhoz, az egészségügyi ellátó intézmény adatbázissal.</span><span class="sxs-lookup"><span data-stu-id="4f071-139">Open SSMS and connect to the server with the Clinic database.</span></span>

1. <span data-ttu-id="4f071-140">Nyissa meg a szolgáltatáshoz az SSMS.</span><span class="sxs-lookup"><span data-stu-id="4f071-140">Open SSMS.</span></span> <span data-ttu-id="4f071-141">(Kattintson **Connect** > **adatbázismotor** megnyitásához a **kapcsolódás a kiszolgálóhoz** ablakot, ha még nincs nyitva).</span><span class="sxs-lookup"><span data-stu-id="4f071-141">(Click **Connect** > **Database Engine** to open the **Connect to Server** window if it is not open).</span></span>
2. <span data-ttu-id="4f071-142">Adja meg a kiszolgáló nevét és hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="4f071-142">Enter your server name and credentials.</span></span> <span data-ttu-id="4f071-143">Az SQL-adatbázis paneljén található a kiszolgáló nevét, és a kapcsolati karakterláncban korábban kimásolt.</span><span class="sxs-lookup"><span data-stu-id="4f071-143">The server name can be found on the SQL database blade and in the connection string you copied earlier.</span></span> <span data-ttu-id="4f071-144">Írja be a teljes kiszolgáló neve például *database.windows.net*.</span><span class="sxs-lookup"><span data-stu-id="4f071-144">Type the complete server name including *database.windows.net*.</span></span>
   
    ![Másolja a kapcsolati karakterláncot](./media/sql-database-always-encrypted/ssms-connect.png)

<span data-ttu-id="4f071-146">Ha a **Új tűzfalszabály** ablakban nyitja meg, jelentkezzen be Azure, és lehetővé teszik az SSMS, hozzon létre egy új tűzfalszabályt.</span><span class="sxs-lookup"><span data-stu-id="4f071-146">If the **New Firewall Rule** window opens, sign in to Azure and let SSMS create a new firewall rule for you.</span></span>

## <a name="create-a-table"></a><span data-ttu-id="4f071-147">Tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="4f071-147">Create a table</span></span>
<span data-ttu-id="4f071-148">Ebben a szakaszban egy beteg adatokat tároló tábla hoz létre.</span><span class="sxs-lookup"><span data-stu-id="4f071-148">In this section, you will create a table to hold patient data.</span></span> <span data-ttu-id="4f071-149">Ez kezdetben--lesz normál tábla titkosítási konfigurál a következő szakaszban.</span><span class="sxs-lookup"><span data-stu-id="4f071-149">This will be a normal table initially--you will configure encryption in the next section.</span></span>

1. <span data-ttu-id="4f071-150">Bontsa ki a **adatbázisok**.</span><span class="sxs-lookup"><span data-stu-id="4f071-150">Expand **Databases**.</span></span>
2. <span data-ttu-id="4f071-151">Kattintson a jobb gombbal a **klinikán** adatbázis, és kattintson a **új lekérdezés**.</span><span class="sxs-lookup"><span data-stu-id="4f071-151">Right-click the **Clinic** database and click **New Query**.</span></span>
3. <span data-ttu-id="4f071-152">Illessze be a következő Transact-SQL (T-SQL) az új lekérdezési ablakba és **Execute** azt.</span><span class="sxs-lookup"><span data-stu-id="4f071-152">Paste the following Transact-SQL (T-SQL) into the new query window and **Execute** it.</span></span>

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


## <a name="encrypt-columns-configure-always-encrypted"></a><span data-ttu-id="4f071-153">Titkosítani az oszlopok (mindig titkosítja konfigurálása)</span><span class="sxs-lookup"><span data-stu-id="4f071-153">Encrypt columns (configure Always Encrypted)</span></span>
<span data-ttu-id="4f071-154">SSMS egy varázslón keresztül történő egyszerű beállítását mindig titkosítja a CMK CEK és titkosított oszlopokat beállítása az Ön által lehetővé.</span><span class="sxs-lookup"><span data-stu-id="4f071-154">SSMS provides a wizard to easily configure Always Encrypted by setting up the CMK, CEK, and encrypted columns for you.</span></span>

1. <span data-ttu-id="4f071-155">Bontsa ki a **adatbázisok** > **klinikán** > **táblák**.</span><span class="sxs-lookup"><span data-stu-id="4f071-155">Expand **Databases** > **Clinic** > **Tables**.</span></span>
2. <span data-ttu-id="4f071-156">Kattintson a jobb gombbal a **betegek** tábla, és válassza ki **titkosítása oszlopok** mindig titkosítja varázsló megnyitásához:</span><span class="sxs-lookup"><span data-stu-id="4f071-156">Right-click the **Patients** table and select **Encrypt Columns** to open the Always Encrypted wizard:</span></span>
   
    ![Oszlopok titkosítása](./media/sql-database-always-encrypted/encrypt-columns.png)

<span data-ttu-id="4f071-158">A mindig titkosítja varázsló az alábbi szakaszokat tartalmazza: **Oszlopválasztás**, **főkulcs konfigurációs** (CMK) **érvényesítési**, és **összegzés**.</span><span class="sxs-lookup"><span data-stu-id="4f071-158">The Always Encrypted wizard includes the following sections: **Column Selection**, **Master Key Configuration** (CMK), **Validation**, and **Summary**.</span></span>

### <a name="column-selection"></a><span data-ttu-id="4f071-159">Oszlop kiválasztása</span><span class="sxs-lookup"><span data-stu-id="4f071-159">Column Selection</span></span>
<span data-ttu-id="4f071-160">Kattintson a **tovább** a a **bemutatása** lapon nyissa meg a **Oszlopválasztás** lap.</span><span class="sxs-lookup"><span data-stu-id="4f071-160">Click **Next** on the **Introduction** page to open the **Column Selection** page.</span></span> <span data-ttu-id="4f071-161">Ezen a lapon kiválaszthatja titkosítására, oszlopok [a típusú titkosítást, és milyen oszlop titkosítási kulcsának (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) használatára.</span><span class="sxs-lookup"><span data-stu-id="4f071-161">On this page, you will select which columns you want to encrypt, [the type of encryption, and what column encryption key (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) to use.</span></span>

<span data-ttu-id="4f071-162">Titkosítani **SSN** és **születési dátumot** minden türelmet adatait.</span><span class="sxs-lookup"><span data-stu-id="4f071-162">Encrypt **SSN** and **BirthDate** information for each patient.</span></span> <span data-ttu-id="4f071-163">A **SSN** oszlop determinisztikus titkosítás, mely támogatja a egyenlőség keresések, társítások és csoportosítás fogja használni.</span><span class="sxs-lookup"><span data-stu-id="4f071-163">The **SSN** column will use deterministic encryption, which supports equality lookups, joins, and group by.</span></span> <span data-ttu-id="4f071-164">A **születési dátumot** oszlop véletlenszerű titkosítás, mely nem támogatja a műveleteket fogja használni.</span><span class="sxs-lookup"><span data-stu-id="4f071-164">The **BirthDate** column will use randomized encryption, which does not support operations.</span></span>

<span data-ttu-id="4f071-165">Állítsa be a **titkosítási típus** a a **SSN** oszlop **Deterministic** és a **születési dátumot** oszlop **Randomized**.</span><span class="sxs-lookup"><span data-stu-id="4f071-165">Set the **Encryption Type** for the **SSN** column to **Deterministic** and the **BirthDate** column to **Randomized**.</span></span> <span data-ttu-id="4f071-166">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="4f071-166">Click **Next**.</span></span>

![Oszlopok titkosítása](./media/sql-database-always-encrypted/column-selection.png)

### <a name="master-key-configuration"></a><span data-ttu-id="4f071-168">A főkulcs konfiguráció</span><span class="sxs-lookup"><span data-stu-id="4f071-168">Master Key Configuration</span></span>
<span data-ttu-id="4f071-169">A **főkulcs konfigurációs** lap ahol állítsa be a CMK, és válassza ki a kulcstároló-szolgáltatóval a CMK tárolásához.</span><span class="sxs-lookup"><span data-stu-id="4f071-169">The **Master Key Configuration** page is where you set up your CMK and select the key store provider where the CMK will be stored.</span></span> <span data-ttu-id="4f071-170">Jelenleg egy CMK tárolhatja a Windows tanúsítványtárolóban, az Azure Key Vault vagy egy hardveres biztonsági modul (HSM).</span><span class="sxs-lookup"><span data-stu-id="4f071-170">Currently, you can store a CMK in the Windows certificate store, Azure Key Vault, or a hardware security module (HSM).</span></span> <span data-ttu-id="4f071-171">Ez az oktatóanyag bemutatja a kulcsok tárolása Windows tanúsítványtárolójába.</span><span class="sxs-lookup"><span data-stu-id="4f071-171">This tutorial shows how to store your keys in the Windows certificate store.</span></span>

<span data-ttu-id="4f071-172">Ellenőrizze, hogy **Windows tanúsítványtároló** van kiválasztva, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="4f071-172">Verify that **Windows certificate store** is selected and click **Next**.</span></span>

![A főkulcs konfiguráció](./media/sql-database-always-encrypted/master-key-configuration.png)

### <a name="validation"></a><span data-ttu-id="4f071-174">Ellenőrzés</span><span class="sxs-lookup"><span data-stu-id="4f071-174">Validation</span></span>
<span data-ttu-id="4f071-175">Most titkosítani az oszlopokat, vagy mentse később futtatni egy PowerShell-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="4f071-175">You can encrypt the columns now or save a PowerShell script to run later.</span></span> <span data-ttu-id="4f071-176">A jelen oktatóanyag esetében válassza ki a **Befejezés most már továbbléphet** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="4f071-176">For this tutorial, select **Proceed to finish now** and click **Next**.</span></span>

### <a name="summary"></a><span data-ttu-id="4f071-177">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="4f071-177">Summary</span></span>
<span data-ttu-id="4f071-178">Ellenőrizze, hogy a beállítások helyességét, és kattintson a **Befejezés** mindig titkosítja az a telepítés befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="4f071-178">Verify that the settings are all correct and click **Finish** to complete the setup for Always Encrypted.</span></span>

![Összefoglalás](./media/sql-database-always-encrypted/summary.png)

### <a name="verify-the-wizards-actions"></a><span data-ttu-id="4f071-180">Ellenőrizze a varázsló műveletek</span><span class="sxs-lookup"><span data-stu-id="4f071-180">Verify the wizard's actions</span></span>
<span data-ttu-id="4f071-181">A varázsló befejezése után az adatbázis be van állítva mindig titkosítja.</span><span class="sxs-lookup"><span data-stu-id="4f071-181">After the wizard is finished, your database is set up for Always Encrypted.</span></span> <span data-ttu-id="4f071-182">A varázsló a következő műveletek végre:</span><span class="sxs-lookup"><span data-stu-id="4f071-182">The wizard performed the following actions:</span></span>

* <span data-ttu-id="4f071-183">Létrehozott egy CMK.</span><span class="sxs-lookup"><span data-stu-id="4f071-183">Created a CMK.</span></span>
* <span data-ttu-id="4f071-184">Létrehozott egy CEK.</span><span class="sxs-lookup"><span data-stu-id="4f071-184">Created a CEK.</span></span>
* <span data-ttu-id="4f071-185">A kijelölt oszlopokat a titkosításhoz konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="4f071-185">Configured the selected columns for encryption.</span></span> <span data-ttu-id="4f071-186">A **betegek** tábla jelenleg nem tartalmaz adatokat, de a meglévő adatokat a kijelölt oszlopokban szereplő most már titkosítva van.</span><span class="sxs-lookup"><span data-stu-id="4f071-186">Your **Patients** table currently has no data, but any existing data in the selected columns is now encrypted.</span></span>

<span data-ttu-id="4f071-187">A szolgáltatáshoz az ssms kulcsok létrehozása megnyitásával ellenőrizheti **klinikán** > **biztonsági** > **mindig a titkosított kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="4f071-187">You can verify the creation of the keys in SSMS by going to **Clinic** > **Security** > **Always Encrypted Keys**.</span></span> <span data-ttu-id="4f071-188">Most láthatja a varázsló által létrehozott, új kulcsokkal.</span><span class="sxs-lookup"><span data-stu-id="4f071-188">You can now see the new keys that the wizard generated for you.</span></span>

## <a name="create-a-client-application-that-works-with-the-encrypted-data"></a><span data-ttu-id="4f071-189">Hozzon létre egy ügyfélalkalmazást, amely kompatibilis a titkosított adatok</span><span class="sxs-lookup"><span data-stu-id="4f071-189">Create a client application that works with the encrypted data</span></span>
<span data-ttu-id="4f071-190">Most, hogy mindig titkosítja be van állítva, egy alkalmazás, amely elvégzi hozhat létre *beszúrása* és *kiválasztja* a titkosított oszlopok.</span><span class="sxs-lookup"><span data-stu-id="4f071-190">Now that Always Encrypted is set up, you can build an application that performs *inserts* and *selects* on the encrypted columns.</span></span> <span data-ttu-id="4f071-191">Sikeresen futtassa a mintaalkalmazást, futtatnia kell az ugyanazon a számítógépen, ahol futtatta a mindig titkosítja varázslót.</span><span class="sxs-lookup"><span data-stu-id="4f071-191">To successfully run the sample application, you must run it on the same computer where you ran the Always Encrypted wizard.</span></span> <span data-ttu-id="4f071-192">Futtassa az alkalmazást egy másik számítógépen, mindig titkosítja tanúsítványait az ügyfél alkalmazást futtató számítógépre kell telepítenie.</span><span class="sxs-lookup"><span data-stu-id="4f071-192">To run the application on another computer, you must deploy your Always Encrypted certificates to the computer running the client app.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="4f071-193">Az alkalmazást kell használnia [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) objektumok számára történő átadásakor adatokat egyszerű szöveges oszlopok mindig titkosítja a kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="4f071-193">Your application must use [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) objects when passing plaintext data to the server with Always Encrypted columns.</span></span> <span data-ttu-id="4f071-194">Értékek szövegkonstansban SqlParameter objektumok használata nélkül átadja azt eredményezi, hogy kivételt.</span><span class="sxs-lookup"><span data-stu-id="4f071-194">Passing literal values without using SqlParameter objects will result in an exception.</span></span>
> 
> 

1. <span data-ttu-id="4f071-195">Nyissa meg a Visual Studio, és hozzon létre egy új C# konzolalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4f071-195">Open Visual Studio and create a new C# console application.</span></span> <span data-ttu-id="4f071-196">Győződjön meg arról, hogy a projekt értéke **.NET-keretrendszer 4.6** vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="4f071-196">Make sure your project is set to **.NET Framework 4.6** or later.</span></span>
2. <span data-ttu-id="4f071-197">Nevet a projektnek **AlwaysEncryptedConsoleApp** kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="4f071-197">Name the project **AlwaysEncryptedConsoleApp** and click **OK**.</span></span>

![Új Konzolalkalmazás](./media/sql-database-always-encrypted/console-app.png)

## <a name="modify-your-connection-string-to-enable-always-encrypted"></a><span data-ttu-id="4f071-199">Ahhoz, hogy mindig titkosítja a kapcsolati karakterlánc módosítása</span><span class="sxs-lookup"><span data-stu-id="4f071-199">Modify your connection string to enable Always Encrypted</span></span>
<span data-ttu-id="4f071-200">Ez a szakasz azt ismerteti, hogyan is engedélyezhető az mindig titkosítja az adatbázis-kapcsolati karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="4f071-200">This section explains how to enable Always Encrypted in your database connection string.</span></span> <span data-ttu-id="4f071-201">A "Mindig titkosítja minta-Konzolalkalmazás." a következő szakaszban létrehozott Konzolalkalmazás fog módosítani</span><span class="sxs-lookup"><span data-stu-id="4f071-201">You will modify the console app you just created in the next section, "Always Encrypted sample console application."</span></span>

<span data-ttu-id="4f071-202">Mindig titkosítja engedélyezéséhez kell hozzáadnia a **Oszloptitkosítási beállítás** kulcsszót a kapcsolati karakterláncot, és állítsa az értékét **engedélyezve**.</span><span class="sxs-lookup"><span data-stu-id="4f071-202">To enable Always Encrypted, you need to add the **Column Encryption Setting** keyword to your connection string and set it to **Enabled**.</span></span>

<span data-ttu-id="4f071-203">Ez közvetlenül a kapcsolódási karakterláncban állíthatja be, vagy használatával állíthatja be a [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx).</span><span class="sxs-lookup"><span data-stu-id="4f071-203">You can set this directly in the connection string, or you can set it by using a [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx).</span></span> <span data-ttu-id="4f071-204">A mintaalkalmazást a következő szakasz bemutatja, hogyan használható **SqlConnectionStringBuilder**.</span><span class="sxs-lookup"><span data-stu-id="4f071-204">The sample application in the next section shows how to use **SqlConnectionStringBuilder**.</span></span>

> [!NOTE]
> <span data-ttu-id="4f071-205">Ez az ügyfélalkalmazás adott mindig titkosítja a szükséges mindössze annyi a változás.</span><span class="sxs-lookup"><span data-stu-id="4f071-205">This is the only change required in a client application specific to Always Encrypted.</span></span> <span data-ttu-id="4f071-206">Ha egy meglévő alkalmazást, amely tárolja a kapcsolati karakterlánc külsőleg (Ez azt jelenti, hogy a konfigurációs fájlban), valószínűleg létre tudja ahhoz, hogy mindig titkosítja a programkód módosítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="4f071-206">If you have an existing application that stores its connection string externally (that is, in a config file), you might be able to enable Always Encrypted without changing any code.</span></span>
> 
> 

### <a name="enable-always-encrypted-in-the-connection-string"></a><span data-ttu-id="4f071-207">Engedélyezze a mindig titkosítja a kapcsolódási karakterláncban</span><span class="sxs-lookup"><span data-stu-id="4f071-207">Enable Always Encrypted in the connection string</span></span>
<span data-ttu-id="4f071-208">A kapcsolati karakterláncot adja hozzá a következő kulcsszó:</span><span class="sxs-lookup"><span data-stu-id="4f071-208">Add the following keyword to your connection string:</span></span>

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-a-sqlconnectionstringbuilder"></a><span data-ttu-id="4f071-209">Egy SqlConnectionStringBuilder mindig titkosítva engedélyezése</span><span class="sxs-lookup"><span data-stu-id="4f071-209">Enable Always Encrypted with a SqlConnectionStringBuilder</span></span>
<span data-ttu-id="4f071-210">A következő kód bemutatja, hogyan lehet engedélyezni a úgy, hogy mindig titkosítja a [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) való [engedélyezve](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).</span><span class="sxs-lookup"><span data-stu-id="4f071-210">The following code shows how to enable Always Encrypted by setting the [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) to [Enabled](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).</span></span>

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;



## <a name="always-encrypted-sample-console-application"></a><span data-ttu-id="4f071-211">Mindig titkosított minta-Konzolalkalmazás</span><span class="sxs-lookup"><span data-stu-id="4f071-211">Always Encrypted sample console application</span></span>
<span data-ttu-id="4f071-212">Ez a példa bemutatja, hogyan:</span><span class="sxs-lookup"><span data-stu-id="4f071-212">This sample demonstrates how to:</span></span>

* <span data-ttu-id="4f071-213">Módosítsa a kapcsolati karakterlánc ahhoz, hogy mindig titkosítja.</span><span class="sxs-lookup"><span data-stu-id="4f071-213">Modify your connection string to enable Always Encrypted.</span></span>
* <span data-ttu-id="4f071-214">Adatok beszúrása a titkosított oszlopokat.</span><span class="sxs-lookup"><span data-stu-id="4f071-214">Insert data into the encrypted columns.</span></span>
* <span data-ttu-id="4f071-215">Válasszon ki egy olyan rekordot szűrést használ a megadott titkosított oszlop az értéket.</span><span class="sxs-lookup"><span data-stu-id="4f071-215">Select a record by filtering for a specific value in an encrypted column.</span></span>

<span data-ttu-id="4f071-216">Cserélje le a tartalmát **Program.cs** a következő kóddal.</span><span class="sxs-lookup"><span data-stu-id="4f071-216">Replace the contents of **Program.cs** with the following code.</span></span> <span data-ttu-id="4f071-217">Cserélje le a kapcsolati karakterláncot a globális connectionString változó a sorban fölött a fő metódus a érvényes kapcsolati karakterláncot az Azure portálról.</span><span class="sxs-lookup"><span data-stu-id="4f071-217">Replace the connection string for the global connectionString variable in the line directly above the Main method with your valid connection string from the Azure portal.</span></span> <span data-ttu-id="4f071-218">Ez akkor tegye ezt a kódot kell mindössze annyi a változás.</span><span class="sxs-lookup"><span data-stu-id="4f071-218">This is the only change you need to make to this code.</span></span>

<span data-ttu-id="4f071-219">Futtassa az alkalmazást, hogy mindig titkosítja a művelet.</span><span class="sxs-lookup"><span data-stu-id="4f071-219">Run the app to see Always Encrypted in action.</span></span>

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
        // Update this line with your Clinic database connection string from the Azure portal.
        static string connectionString = @"Replace with your connection string";

        static void Main(string[] args)
        {
            Console.WriteLine("Original connection string copied from the Azure portal:");
            Console.WriteLine(connectionString);

            // Create a SqlConnectionStringBuilder.
            SqlConnectionStringBuilder connStringBuilder =
                new SqlConnectionStringBuilder(connectionString);

            // Enable Always Encrypted for the connection.
            // This is the only change specific to Always Encrypted
            connStringBuilder.ColumnEncryptionSetting =
                SqlConnectionColumnEncryptionSetting.Enabled;

            Console.WriteLine(Environment.NewLine + "Updated connection string with Always Encrypted enabled:");
            Console.WriteLine(connStringBuilder.ConnectionString);

            // Update the connection string with a password supplied at runtime.
            Console.WriteLine(Environment.NewLine + "Enter server password:");
            connStringBuilder.Password = Console.ReadLine();


            // Assign the updated connection string to our global variable.
            connectionString = connStringBuilder.ConnectionString;


            // Delete all records to restart this demo app.
            ResetPatientsTable();

            // Add sample data to the Patients table.
            Console.Write(Environment.NewLine + "Adding sample patient data to the database...");

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
            Console.WriteLine(Environment.NewLine + "All the records currently in the Patients table:");

            foreach (Patient patient in SelectAllPatients())
            {
                Console.WriteLine(patient.FirstName + " " + patient.LastName + "\tSSN: " + patient.SSN + "\tBirthdate: " + patient.BirthDate);
            }

            // Get patients by SSN.
            Console.WriteLine(Environment.NewLine + "Now let's locate records by searching the encrypted SSN column.");

            string ssn;

            // This very simple validation only checks that the user entered 11 characters.
            // In production be sure to check all user input and use the best validation for your specific application.
            do
            {
                Console.WriteLine("Please enter a valid SSN (ex. 123-45-6789):");
                ssn = Console.ReadLine();
            } while (ssn.Length != 11);

            // The example allows duplicate SSN entries so we will return all records
            // that match the provided value and store the results in selectedPatients.
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

            Console.WriteLine("Press Enter to exit...");
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
                    Console.WriteLine("The following error was encountered: ");
                    Console.WriteLine(ex.Message);
                    Console.WriteLine(Environment.NewLine + "Press Enter key to exit");
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


        // This method simply deletes all records in the Patients table to reset our demo.
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


## <a name="verify-that-the-data-is-encrypted"></a><span data-ttu-id="4f071-220">Győződjön meg arról, hogy az adatok titkosítása</span><span class="sxs-lookup"><span data-stu-id="4f071-220">Verify that the data is encrypted</span></span>
<span data-ttu-id="4f071-221">Gyorsan ellenőrizheti, hogy a tényleges adatokat a kiszolgáló titkosított lekérdezésével a **betegek** ssms alkalmazásával adatokat.</span><span class="sxs-lookup"><span data-stu-id="4f071-221">You can quickly check that the actual data on the server is encrypted by querying the **Patients** data with SSMS.</span></span> <span data-ttu-id="4f071-222">(Az aktuális kapcsolat használatát ahol a oszloptitkosítási beállítás még nincs engedélyezve.)</span><span class="sxs-lookup"><span data-stu-id="4f071-222">(Use your current connection where the column encryption setting is not yet enabled.)</span></span>

<span data-ttu-id="4f071-223">Futtassa a következő lekérdezést a klinikán adatbázison.</span><span class="sxs-lookup"><span data-stu-id="4f071-223">Run the following query on the Clinic database.</span></span>

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

<span data-ttu-id="4f071-224">Láthatja, hogy a titkosított oszlop nem tartalmaz az egyszerű szöveges adatokat.</span><span class="sxs-lookup"><span data-stu-id="4f071-224">You can see that the encrypted columns do not contain any plaintext data.</span></span>

   ![Új Konzolalkalmazás](./media/sql-database-always-encrypted/ssms-encrypted.png)

<span data-ttu-id="4f071-226">Az egyszerű szöveges adatok eléréséhez használja az SSMS, adja hozzá a **Oszloptitkosítási beállítás = engedélyezve** paraméter a kapcsolatra.</span><span class="sxs-lookup"><span data-stu-id="4f071-226">To use SSMS to access the plaintext data, you can add the **Column Encryption Setting=enabled** parameter to the connection.</span></span>

1. <span data-ttu-id="4f071-227">Az SSMS, kattintson a jobb gombbal a kiszolgáló **Object Explorer**, és kattintson a **Disconnect**.</span><span class="sxs-lookup"><span data-stu-id="4f071-227">In SSMS, right-click your server in **Object Explorer**, and then click **Disconnect**.</span></span>
2. <span data-ttu-id="4f071-228">Kattintson a **Connect** > **adatbázismotor** megnyitásához a **kapcsolódás a kiszolgálóhoz** ablakot, és kattintson **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="4f071-228">Click **Connect** > **Database Engine** to open the **Connect to Server** window, and then click **Options**.</span></span>
3. <span data-ttu-id="4f071-229">Kattintson a **további kapcsolódási paraméterek** és típus **Oszloptitkosítási beállítás = engedélyezve**.</span><span class="sxs-lookup"><span data-stu-id="4f071-229">Click **Additional Connection Parameters** and type **Column Encryption Setting=enabled**.</span></span>
   
    ![Új Konzolalkalmazás](./media/sql-database-always-encrypted/ssms-connection-parameter.png)
4. <span data-ttu-id="4f071-231">A következő lekérdezés futtatása a **klinikán** adatbázis.</span><span class="sxs-lookup"><span data-stu-id="4f071-231">Run the following query on the **Clinic** database.</span></span>
   
        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;
   
     <span data-ttu-id="4f071-232">Most már megtekintheti a titkosított oszlopokban az egyszerű szöveges adatokat.</span><span class="sxs-lookup"><span data-stu-id="4f071-232">You can now see the plaintext data in the encrypted columns.</span></span>

    ![Új Konzolalkalmazás](./media/sql-database-always-encrypted/ssms-plaintext.png)



> [!NOTE]
> <span data-ttu-id="4f071-234">Ha a szolgáltatáshoz az SSMS (vagy bármely olyan ügyfél) egy másik számítógépről csatlakozik, nem kell férnie a titkosítási kulcsokhoz, és nem tudják fejteni az adatokat.</span><span class="sxs-lookup"><span data-stu-id="4f071-234">If you connect with SSMS (or any client) from a different computer, it will not have access to the encryption keys and will not be able to decrypt the data.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="4f071-235">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4f071-235">Next steps</span></span>
<span data-ttu-id="4f071-236">Miután létrehozott egy adatbázist, amely mindig titkosítja használ, érdemes lehet tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="4f071-236">After you create a database that uses Always Encrypted, you may want to do the following:</span></span>

* <span data-ttu-id="4f071-237">A minta futtatásához egy másik számítógépen.</span><span class="sxs-lookup"><span data-stu-id="4f071-237">Run this sample from a different computer.</span></span> <span data-ttu-id="4f071-238">A titkosítási kulcsoknak access azt nem rendelkezik, ezért nem lesz a titkosítatlan szöveges adataihoz való hozzáférés, és nem futtathatók sikeresen.</span><span class="sxs-lookup"><span data-stu-id="4f071-238">It won't have access to the encryption keys, so it will not have access to the plaintext data and will not run successfully.</span></span>
* <span data-ttu-id="4f071-239">[Forgassa el, és a kulcsok tisztítása](https://msdn.microsoft.com/library/mt607048.aspx).</span><span class="sxs-lookup"><span data-stu-id="4f071-239">[Rotate and clean up your keys](https://msdn.microsoft.com/library/mt607048.aspx).</span></span>
* <span data-ttu-id="4f071-240">[Már mindig titkosítja a titkosított adatok áttelepítése](https://msdn.microsoft.com/library/mt621539.aspx).</span><span class="sxs-lookup"><span data-stu-id="4f071-240">[Migrate data that is already encrypted with Always Encrypted](https://msdn.microsoft.com/library/mt621539.aspx).</span></span>
* <span data-ttu-id="4f071-241">[Mindig titkosítja tanúsítványok telepítése a más ügyfélgépek](https://msdn.microsoft.com/library/mt723359.aspx#Anchor_1) (lásd az "Így tanúsítványok érhető el az alkalmazások és felhasználók").</span><span class="sxs-lookup"><span data-stu-id="4f071-241">[Deploy Always Encrypted certificates to other client machines](https://msdn.microsoft.com/library/mt723359.aspx#Anchor_1) (see the "Making Certificates Available to Applications and Users" section).</span></span>

## <a name="related-information"></a><span data-ttu-id="4f071-242">Kapcsolódó információk</span><span class="sxs-lookup"><span data-stu-id="4f071-242">Related information</span></span>
* [<span data-ttu-id="4f071-243">Mindig titkosítja (ügyféloldali fejlesztés)</span><span class="sxs-lookup"><span data-stu-id="4f071-243">Always Encrypted (client development)</span></span>](https://msdn.microsoft.com/library/mt147923.aspx)
* [<span data-ttu-id="4f071-244">Átlátható adattitkosítás</span><span class="sxs-lookup"><span data-stu-id="4f071-244">Transparent Data Encryption</span></span>](https://msdn.microsoft.com/library/bb934049.aspx)
* [<span data-ttu-id="4f071-245">SQL Server-titkosítás</span><span class="sxs-lookup"><span data-stu-id="4f071-245">SQL Server Encryption</span></span>](https://msdn.microsoft.com/library/bb510663.aspx)
* [<span data-ttu-id="4f071-246">Mindig titkosított varázsló</span><span class="sxs-lookup"><span data-stu-id="4f071-246">Always Encrypted Wizard</span></span>](https://msdn.microsoft.com/library/mt459280.aspx)
* [<span data-ttu-id="4f071-247">Mindig titkosított Blog</span><span class="sxs-lookup"><span data-stu-id="4f071-247">Always Encrypted Blog</span></span>](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)

