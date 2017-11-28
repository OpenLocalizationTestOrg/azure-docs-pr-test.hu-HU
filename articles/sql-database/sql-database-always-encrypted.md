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
# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-hello-windows-certificate-store"></a><span data-ttu-id="0a542-105">Mindig titkosítja: Az SQL-adatbázis bizalmas adatok védelmét, és a titkosítási kulcsok tárolása hello Windows tanúsítványtároló</span><span class="sxs-lookup"><span data-stu-id="0a542-105">Always Encrypted: Protect sensitive data in SQL Database and store your encryption keys in hello Windows certificate store</span></span>

<span data-ttu-id="0a542-106">Ez a cikk bemutatja, hogyan toosecure bizalmas adatait egy SQL-adatbázis az adatbázis-titkosítás hello segítségével [varázsló mindig titkosítja](https://msdn.microsoft.com/library/mt459280.aspx) a [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx).</span><span class="sxs-lookup"><span data-stu-id="0a542-106">This article shows you how toosecure sensitive data in a SQL database with database encryption by using hello [Always Encrypted Wizard](https://msdn.microsoft.com/library/mt459280.aspx) in [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx).</span></span> <span data-ttu-id="0a542-107">Azt is bemutatja, hogyan toostore hello Windows tanúsítvány a titkosítási kulcsok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="0a542-107">It also shows you how toostore your encryption keys in hello Windows certificate store.</span></span>

<span data-ttu-id="0a542-108">Mindig titkosított technológiája új adatok titkosítása az Azure SQL-adatbázis, amely segít az SQL Server hello kiszolgálón inaktív bizalmas adat védelme adatátviteli ügyfél és kiszolgáló közötti és hello adatok használata közben, bizalmas adatok soha nem biztosítására jelenik meg egyszerű szöveges hello adatbázis rendszerében.</span><span class="sxs-lookup"><span data-stu-id="0a542-108">Always Encrypted is a new data encryption technology in Azure SQL Database and SQL Server that helps protect sensitive data at rest on hello server, during movement between client and server, and while hello data is in use, ensuring that sensitive data never appears as plaintext inside hello database system.</span></span> <span data-ttu-id="0a542-109">Adatok titkosítása csak az ügyfélalkalmazások vagy toohello hívóbetűket alkalmazáskiszolgálókra is férnek hozzá a titkosítatlan szöveges adatokat.</span><span class="sxs-lookup"><span data-stu-id="0a542-109">After you encrypt data, only client applications or app servers that have access toohello keys can access plaintext data.</span></span> <span data-ttu-id="0a542-110">Részletes információkért lásd: [(adatbázismotor) mindig titkosítja](https://msdn.microsoft.com/library/mt163865.aspx).</span><span class="sxs-lookup"><span data-stu-id="0a542-110">For detailed information, see [Always Encrypted (Database Engine)](https://msdn.microsoft.com/library/mt163865.aspx).</span></span>

<span data-ttu-id="0a542-111">Miután hello adatbázis toouse mindig titkosítja, C# nyelven íródtak a Visual Studio toowork hello titkosított adatokat hoz létre egy ügyfélalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="0a542-111">After configuring hello database toouse Always Encrypted, you will create a client application in C# with Visual Studio toowork with hello encrypted data.</span></span>

<span data-ttu-id="0a542-112">Hogyan kövesse a cikk toolearn hello lépéseit tooset mentése mindig titkosítja az Azure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="0a542-112">Follow hello steps in this article toolearn how tooset up Always Encrypted for an Azure SQL database.</span></span> <span data-ttu-id="0a542-113">Ebből a cikkből megtudhatja, hogyan tooperform hello a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="0a542-113">In this article, you will learn how tooperform hello following tasks:</span></span>

* <span data-ttu-id="0a542-114">Hello mindig titkosítja varázsló használata a szolgáltatáshoz az SSMS toocreate [mindig a titkosított kulcsok](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).</span><span class="sxs-lookup"><span data-stu-id="0a542-114">Use hello Always Encrypted wizard in SSMS toocreate [Always Encrypted Keys](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).</span></span>
  * <span data-ttu-id="0a542-115">Hozzon létre egy [oszlop főkulcs (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).</span><span class="sxs-lookup"><span data-stu-id="0a542-115">Create a [Column Master Key (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).</span></span>
  * <span data-ttu-id="0a542-116">Hozzon létre egy [oszlop titkosítási kulcsának (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).</span><span class="sxs-lookup"><span data-stu-id="0a542-116">Create a [Column Encryption Key (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).</span></span>
* <span data-ttu-id="0a542-117">Hozzon létre egy adatbázis tábláinak és oszlopok titkosításához.</span><span class="sxs-lookup"><span data-stu-id="0a542-117">Create a database table and encrypt columns.</span></span>
* <span data-ttu-id="0a542-118">Hozzon létre olyan alkalmazás, amely beszúrja, kiválasztása és hello titkosított oszlopok adatait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="0a542-118">Create an application that inserts, selects, and displays data from hello encrypted columns.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0a542-119">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0a542-119">Prerequisites</span></span>
<span data-ttu-id="0a542-120">Ebben az oktatóanyagban lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="0a542-120">For this tutorial, you'll need:</span></span>

* <span data-ttu-id="0a542-121">Azure-fiók és -előfizetés.</span><span class="sxs-lookup"><span data-stu-id="0a542-121">An Azure account and subscription.</span></span> <span data-ttu-id="0a542-122">Ha még nincs fiókja, regisztráljon egy [ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0a542-122">If you don't have one, sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="0a542-123">[SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) 13.0.700.242 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="0a542-123">[SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) version 13.0.700.242 or later.</span></span>
* <span data-ttu-id="0a542-124">[.NET-keretrendszer 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) vagy újabb (ügyfélszámítógépen hello).</span><span class="sxs-lookup"><span data-stu-id="0a542-124">[.NET Framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) or later (on hello client computer).</span></span>
* <span data-ttu-id="0a542-125">[Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="0a542-125">[Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).</span></span>

## <a name="create-a-blank-sql-database"></a><span data-ttu-id="0a542-126">Üres SQL-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="0a542-126">Create a blank SQL database</span></span>
1. <span data-ttu-id="0a542-127">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="0a542-127">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="0a542-128">Kattintson a **új** > **adatok + tárolás** > **SQL-adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="0a542-128">Click **New** > **Data + Storage** > **SQL Database**.</span></span>
3. <span data-ttu-id="0a542-129">Hozzon létre egy **üres** nevű adatbázis **klinikán** egy új vagy meglévő kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="0a542-129">Create a **Blank** database named **Clinic** on a new or existing server.</span></span> <span data-ttu-id="0a542-130">Adatbázis létrehozása az Azure-portálon hello részletes utasításokért lásd: [az első Azure SQL-adatbázis](sql-database-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="0a542-130">For detailed instructions about creating a database in hello Azure portal, see [Your first Azure SQL database](sql-database-get-started-portal.md).</span></span>
   
    ![Hozzon létre egy üres adatbázist](./media/sql-database-always-encrypted/create-database.png)

<span data-ttu-id="0a542-132">Hello oktatóanyag későbbi részében szüksége lesz hello kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="0a542-132">You will need hello connection string later in hello tutorial.</span></span> <span data-ttu-id="0a542-133">Hello adatbázis létrehozását követően nyissa meg a toohello új klinikán adatbázis, és másolja hello kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="0a542-133">After hello database is created, go toohello new Clinic database and copy hello connection string.</span></span> <span data-ttu-id="0a542-134">Bármikor kaphat a hello kapcsolati karakterláncot, de egyszerű toocopy, ha éppen hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="0a542-134">You can get hello connection string at any time, but it's easy toocopy it when you're in hello Azure portal.</span></span>

1. <span data-ttu-id="0a542-135">Kattintson a **SQL-adatbázisok** > **klinikán** > **adatbázis-kapcsolati karakterláncok megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="0a542-135">Click **SQL databases** > **Clinic** > **Show database connection strings**.</span></span>
2. <span data-ttu-id="0a542-136">Másolja a kapcsolati karakterláncot hello **ADO.NET**.</span><span class="sxs-lookup"><span data-stu-id="0a542-136">Copy hello connection string for **ADO.NET**.</span></span>
   
    ![Másolja a hello kapcsolati karakterlánc](./media/sql-database-always-encrypted/connection-strings.png)

## <a name="connect-toohello-database-with-ssms"></a><span data-ttu-id="0a542-138">Csatlakozás toohello adatbázis ssms alkalmazásával</span><span class="sxs-lookup"><span data-stu-id="0a542-138">Connect toohello database with SSMS</span></span>
<span data-ttu-id="0a542-139">Nyissa meg a szolgáltatáshoz az SSMS és toohello kiszolgáló kapcsolódni hello klinikán adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="0a542-139">Open SSMS and connect toohello server with hello Clinic database.</span></span>

1. <span data-ttu-id="0a542-140">Nyissa meg a szolgáltatáshoz az SSMS.</span><span class="sxs-lookup"><span data-stu-id="0a542-140">Open SSMS.</span></span> <span data-ttu-id="0a542-141">(Kattintson **Connect** > **adatbázismotor** tooopen hello **tooServer csatlakozás** ablakot, ha még nincs nyitva).</span><span class="sxs-lookup"><span data-stu-id="0a542-141">(Click **Connect** > **Database Engine** tooopen hello **Connect tooServer** window if it is not open).</span></span>
2. <span data-ttu-id="0a542-142">Adja meg a kiszolgáló nevét és hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="0a542-142">Enter your server name and credentials.</span></span> <span data-ttu-id="0a542-143">hello kiszolgálónév hello SQL adatbázis paneljén található, és a kapcsolati karakterláncban hello korábban kimásolt.</span><span class="sxs-lookup"><span data-stu-id="0a542-143">hello server name can be found on hello SQL database blade and in hello connection string you copied earlier.</span></span> <span data-ttu-id="0a542-144">Típus hello teljes kiszolgáló neve például *database.windows.net*.</span><span class="sxs-lookup"><span data-stu-id="0a542-144">Type hello complete server name including *database.windows.net*.</span></span>
   
    ![Másolja a hello kapcsolati karakterlánc](./media/sql-database-always-encrypted/ssms-connect.png)

<span data-ttu-id="0a542-146">Ha hello **Új tűzfalszabály** ablak nyílik tooAzure bejelentkezhet, és lehetővé teszik az SSMS, hozzon létre egy új tűzfalszabályt.</span><span class="sxs-lookup"><span data-stu-id="0a542-146">If hello **New Firewall Rule** window opens, sign in tooAzure and let SSMS create a new firewall rule for you.</span></span>

## <a name="create-a-table"></a><span data-ttu-id="0a542-147">Tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="0a542-147">Create a table</span></span>
<span data-ttu-id="0a542-148">Ebben a szakaszban egy tábla toohold beteg adatokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="0a542-148">In this section, you will create a table toohold patient data.</span></span> <span data-ttu-id="0a542-149">Ez kezdetben--lesz normál tábla titkosítási állít hello a következő szakaszban.</span><span class="sxs-lookup"><span data-stu-id="0a542-149">This will be a normal table initially--you will configure encryption in hello next section.</span></span>

1. <span data-ttu-id="0a542-150">Bontsa ki a **adatbázisok**.</span><span class="sxs-lookup"><span data-stu-id="0a542-150">Expand **Databases**.</span></span>
2. <span data-ttu-id="0a542-151">Kattintson a jobb gombbal hello **klinikán** adatbázis, és kattintson a **új lekérdezés**.</span><span class="sxs-lookup"><span data-stu-id="0a542-151">Right-click hello **Clinic** database and click **New Query**.</span></span>
3. <span data-ttu-id="0a542-152">Beillesztés hello Transact-SQL (T-SQL) következő hello új lekérdezési ablakba és **Execute** azt.</span><span class="sxs-lookup"><span data-stu-id="0a542-152">Paste hello following Transact-SQL (T-SQL) into hello new query window and **Execute** it.</span></span>

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


## <a name="encrypt-columns-configure-always-encrypted"></a><span data-ttu-id="0a542-153">Titkosítani az oszlopok (mindig titkosítja konfigurálása)</span><span class="sxs-lookup"><span data-stu-id="0a542-153">Encrypt columns (configure Always Encrypted)</span></span>
<span data-ttu-id="0a542-154">SSMS biztosít egy varázsló tooeasily konfigurálása mindig titkosítja hello CMK CEK és titkosított oszlopokat beállítását meg.</span><span class="sxs-lookup"><span data-stu-id="0a542-154">SSMS provides a wizard tooeasily configure Always Encrypted by setting up hello CMK, CEK, and encrypted columns for you.</span></span>

1. <span data-ttu-id="0a542-155">Bontsa ki a **adatbázisok** > **klinikán** > **táblák**.</span><span class="sxs-lookup"><span data-stu-id="0a542-155">Expand **Databases** > **Clinic** > **Tables**.</span></span>
2. <span data-ttu-id="0a542-156">Kattintson a jobb gombbal hello **betegek** tábla, és válassza ki **titkosítása oszlopok** tooopen hello mindig titkosítja varázsló:</span><span class="sxs-lookup"><span data-stu-id="0a542-156">Right-click hello **Patients** table and select **Encrypt Columns** tooopen hello Always Encrypted wizard:</span></span>
   
    ![Oszlopok titkosítása](./media/sql-database-always-encrypted/encrypt-columns.png)

<span data-ttu-id="0a542-158">hello mindig titkosítja varázsló tartalmazza a következő szakaszok hello: **Oszlopválasztás**, **főkulcs konfigurációs** (CMK) **érvényesítési**, és  **Összefoglalás**.</span><span class="sxs-lookup"><span data-stu-id="0a542-158">hello Always Encrypted wizard includes hello following sections: **Column Selection**, **Master Key Configuration** (CMK), **Validation**, and **Summary**.</span></span>

### <a name="column-selection"></a><span data-ttu-id="0a542-159">Oszlop kiválasztása</span><span class="sxs-lookup"><span data-stu-id="0a542-159">Column Selection</span></span>
<span data-ttu-id="0a542-160">Kattintson a **következő** a hello **bemutatása** lap tooopen hello **Oszlopválasztás** lap.</span><span class="sxs-lookup"><span data-stu-id="0a542-160">Click **Next** on hello **Introduction** page tooopen hello **Column Selection** page.</span></span> <span data-ttu-id="0a542-161">Ezen a lapon kiválaszthatja oszlopok tooencrypt, kívánt [hello típusú titkosítást, és milyen oszlop titkosítási kulcsának (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) toouse.</span><span class="sxs-lookup"><span data-stu-id="0a542-161">On this page, you will select which columns you want tooencrypt, [hello type of encryption, and what column encryption key (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) toouse.</span></span>

<span data-ttu-id="0a542-162">Titkosítani **SSN** és **születési dátumot** minden türelmet adatait.</span><span class="sxs-lookup"><span data-stu-id="0a542-162">Encrypt **SSN** and **BirthDate** information for each patient.</span></span> <span data-ttu-id="0a542-163">Hello **SSN** oszlop determinisztikus titkosítás, mely támogatja a egyenlőség keresések, társítások és csoportosítás fogja használni.</span><span class="sxs-lookup"><span data-stu-id="0a542-163">hello **SSN** column will use deterministic encryption, which supports equality lookups, joins, and group by.</span></span> <span data-ttu-id="0a542-164">Hello **születési dátumot** oszlop véletlenszerű titkosítás, mely nem támogatja a műveleteket fogja használni.</span><span class="sxs-lookup"><span data-stu-id="0a542-164">hello **BirthDate** column will use randomized encryption, which does not support operations.</span></span>

<span data-ttu-id="0a542-165">Set hello **titkosítási típus** a hello **SSN** oszlop túl**Deterministic** és hello **születési dátumot** oszlop túl **Véletlenszerű**.</span><span class="sxs-lookup"><span data-stu-id="0a542-165">Set hello **Encryption Type** for hello **SSN** column too**Deterministic** and hello **BirthDate** column too**Randomized**.</span></span> <span data-ttu-id="0a542-166">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="0a542-166">Click **Next**.</span></span>

![Oszlopok titkosítása](./media/sql-database-always-encrypted/column-selection.png)

### <a name="master-key-configuration"></a><span data-ttu-id="0a542-168">A főkulcs konfiguráció</span><span class="sxs-lookup"><span data-stu-id="0a542-168">Master Key Configuration</span></span>
<span data-ttu-id="0a542-169">Hello **főkulcs konfigurációs** lap, ahol beállíthatja a CMK és select hello kulcstároló-szolgáltatóval hello CMK tárolásához.</span><span class="sxs-lookup"><span data-stu-id="0a542-169">hello **Master Key Configuration** page is where you set up your CMK and select hello key store provider where hello CMK will be stored.</span></span> <span data-ttu-id="0a542-170">Jelenleg egy CMK tárolhatja hello Windows tanúsítványtárolójába, az Azure Key Vault vagy egy hardveres biztonsági modul (HSM).</span><span class="sxs-lookup"><span data-stu-id="0a542-170">Currently, you can store a CMK in hello Windows certificate store, Azure Key Vault, or a hardware security module (HSM).</span></span> <span data-ttu-id="0a542-171">Ez az oktatóanyag bemutatja, hogyan toostore hello Windows tanúsítvány a kulcsok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="0a542-171">This tutorial shows how toostore your keys in hello Windows certificate store.</span></span>

<span data-ttu-id="0a542-172">Ellenőrizze, hogy **Windows tanúsítványtároló** van kiválasztva, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="0a542-172">Verify that **Windows certificate store** is selected and click **Next**.</span></span>

![A főkulcs konfiguráció](./media/sql-database-always-encrypted/master-key-configuration.png)

### <a name="validation"></a><span data-ttu-id="0a542-174">Ellenőrzés</span><span class="sxs-lookup"><span data-stu-id="0a542-174">Validation</span></span>
<span data-ttu-id="0a542-175">Hello oszlopok most titkosítására, vagy egy PowerShell-parancsfájl toorun később mentse.</span><span class="sxs-lookup"><span data-stu-id="0a542-175">You can encrypt hello columns now or save a PowerShell script toorun later.</span></span> <span data-ttu-id="0a542-176">A jelen oktatóanyag esetében válassza ki a **toofinish továbblépni** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="0a542-176">For this tutorial, select **Proceed toofinish now** and click **Next**.</span></span>

### <a name="summary"></a><span data-ttu-id="0a542-177">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="0a542-177">Summary</span></span>
<span data-ttu-id="0a542-178">Győződjön meg arról, hogy hello beállításainak helyességét, és kattintson a **Befejezés** toocomplete hello beállítása mindig titkosítja.</span><span class="sxs-lookup"><span data-stu-id="0a542-178">Verify that hello settings are all correct and click **Finish** toocomplete hello setup for Always Encrypted.</span></span>

![Összefoglalás](./media/sql-database-always-encrypted/summary.png)

### <a name="verify-hello-wizards-actions"></a><span data-ttu-id="0a542-180">Ellenőrizze a hello varázsló műveletek</span><span class="sxs-lookup"><span data-stu-id="0a542-180">Verify hello wizard's actions</span></span>
<span data-ttu-id="0a542-181">Hello varázsló befejezése után az adatbázis be van állítva mindig titkosítja.</span><span class="sxs-lookup"><span data-stu-id="0a542-181">After hello wizard is finished, your database is set up for Always Encrypted.</span></span> <span data-ttu-id="0a542-182">hello végre varázsló hello a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="0a542-182">hello wizard performed hello following actions:</span></span>

* <span data-ttu-id="0a542-183">Létrehozott egy CMK.</span><span class="sxs-lookup"><span data-stu-id="0a542-183">Created a CMK.</span></span>
* <span data-ttu-id="0a542-184">Létrehozott egy CEK.</span><span class="sxs-lookup"><span data-stu-id="0a542-184">Created a CEK.</span></span>
* <span data-ttu-id="0a542-185">Konfigurált hello kijelölt oszlopok titkosításhoz.</span><span class="sxs-lookup"><span data-stu-id="0a542-185">Configured hello selected columns for encryption.</span></span> <span data-ttu-id="0a542-186">A **betegek** tábla jelenleg nem tartalmaz adatokat, de most Titkosított hello a kijelölt oszlopokban szereplő adatokat.</span><span class="sxs-lookup"><span data-stu-id="0a542-186">Your **Patients** table currently has no data, but any existing data in hello selected columns is now encrypted.</span></span>

<span data-ttu-id="0a542-187">Hello kulcsok szolgáltatáshoz az ssms hello létrehozását túl megnyitásával ellenőrizheti**klinikán** > **biztonsági** > **mindig a titkosított kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="0a542-187">You can verify hello creation of hello keys in SSMS by going too**Clinic** > **Security** > **Always Encrypted Keys**.</span></span> <span data-ttu-id="0a542-188">Most már megtekintheti hello hello varázsló által létrehozott új kulcsokkal.</span><span class="sxs-lookup"><span data-stu-id="0a542-188">You can now see hello new keys that hello wizard generated for you.</span></span>

## <a name="create-a-client-application-that-works-with-hello-encrypted-data"></a><span data-ttu-id="0a542-189">Hozzon létre egy ügyfélalkalmazást, amely kompatibilis a hello titkosított adatok</span><span class="sxs-lookup"><span data-stu-id="0a542-189">Create a client application that works with hello encrypted data</span></span>
<span data-ttu-id="0a542-190">Most, hogy mindig titkosítja be van állítva, egy alkalmazás, amely elvégzi hozhat létre *beszúrása* és *kiválasztja* hello a titkosított oszlopokat.</span><span class="sxs-lookup"><span data-stu-id="0a542-190">Now that Always Encrypted is set up, you can build an application that performs *inserts* and *selects* on hello encrypted columns.</span></span> <span data-ttu-id="0a542-191">Futtassa a mintaalkalmazást hello toosuccessfully, futtatnia kell a hello ahol hello mindig titkosítja varázsló futtatása ugyanazon a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="0a542-191">toosuccessfully run hello sample application, you must run it on hello same computer where you ran hello Always Encrypted wizard.</span></span> <span data-ttu-id="0a542-192">toorun hello alkalmazás egy másik számítógépen, mindig titkosítja tanúsítványok toohello futtató számítógép hello ügyfélalkalmazás kell telepítenie.</span><span class="sxs-lookup"><span data-stu-id="0a542-192">toorun hello application on another computer, you must deploy your Always Encrypted certificates toohello computer running hello client app.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="0a542-193">Az alkalmazást kell használnia [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) történő átadásakor egyszerű szöveges adatok toohello kiszolgáló mindig titkosítja oszlopokkal objektumokat.</span><span class="sxs-lookup"><span data-stu-id="0a542-193">Your application must use [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) objects when passing plaintext data toohello server with Always Encrypted columns.</span></span> <span data-ttu-id="0a542-194">Értékek szövegkonstansban SqlParameter objektumok használata nélkül átadja azt eredményezi, hogy kivételt.</span><span class="sxs-lookup"><span data-stu-id="0a542-194">Passing literal values without using SqlParameter objects will result in an exception.</span></span>
> 
> 

1. <span data-ttu-id="0a542-195">Nyissa meg a Visual Studio, és hozzon létre egy új C# konzolalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="0a542-195">Open Visual Studio and create a new C# console application.</span></span> <span data-ttu-id="0a542-196">Ellenőrizze, hogy túl van-e állítva a projekt**.NET-keretrendszer 4.6** vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="0a542-196">Make sure your project is set too**.NET Framework 4.6** or later.</span></span>
2. <span data-ttu-id="0a542-197">Név hello projekt **AlwaysEncryptedConsoleApp** kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="0a542-197">Name hello project **AlwaysEncryptedConsoleApp** and click **OK**.</span></span>

![Új Konzolalkalmazás](./media/sql-database-always-encrypted/console-app.png)

## <a name="modify-your-connection-string-tooenable-always-encrypted"></a><span data-ttu-id="0a542-199">A kapcsolati karakterlánc tooenable mindig titkosítja módosítása</span><span class="sxs-lookup"><span data-stu-id="0a542-199">Modify your connection string tooenable Always Encrypted</span></span>
<span data-ttu-id="0a542-200">Ez a szakasz azt ismerteti, hogyan tooenable mindig titkosítja az adatbázis-kapcsolati karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="0a542-200">This section explains how tooenable Always Encrypted in your database connection string.</span></span> <span data-ttu-id="0a542-201">Most létrehozott hello következő szakaszban "Mindig titkosítja minta-Konzolalkalmazás." hello konzolalkalmazást fog módosítani</span><span class="sxs-lookup"><span data-stu-id="0a542-201">You will modify hello console app you just created in hello next section, "Always Encrypted sample console application."</span></span>

<span data-ttu-id="0a542-202">tooenable mindig titkosítja, kell tooadd hello **Oszloptitkosítási beállítás** kulcsszó tooyour kapcsolati karakterláncot, és állítsa be úgy túl**engedélyezve**.</span><span class="sxs-lookup"><span data-stu-id="0a542-202">tooenable Always Encrypted, you need tooadd hello **Column Encryption Setting** keyword tooyour connection string and set it too**Enabled**.</span></span>

<span data-ttu-id="0a542-203">Közvetlen kapcsolat-karakterláncban hello állíthat, vagy beállíthatja a egy [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx).</span><span class="sxs-lookup"><span data-stu-id="0a542-203">You can set this directly in hello connection string, or you can set it by using a [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx).</span></span> <span data-ttu-id="0a542-204">Hogyan csak a következő szakasz azt mutatja be hello mintaalkalmazáshoz hello toouse **SqlConnectionStringBuilder**.</span><span class="sxs-lookup"><span data-stu-id="0a542-204">hello sample application in hello next section shows how toouse **SqlConnectionStringBuilder**.</span></span>

> [!NOTE]
> <span data-ttu-id="0a542-205">Ez a hello mindössze annyi a változás egy ügyfél konkrét tooAlways titkosított szükséges.</span><span class="sxs-lookup"><span data-stu-id="0a542-205">This is hello only change required in a client application specific tooAlways Encrypted.</span></span> <span data-ttu-id="0a542-206">Ha egy meglévő alkalmazást, amely tárolja a kapcsolati karakterlánc külsőleg (Ez azt jelenti, hogy a konfigurációs fájlban), is képes tooenable mindig titkosítja a programkód módosítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="0a542-206">If you have an existing application that stores its connection string externally (that is, in a config file), you might be able tooenable Always Encrypted without changing any code.</span></span>
> 
> 

### <a name="enable-always-encrypted-in-hello-connection-string"></a><span data-ttu-id="0a542-207">Mindig titkosítja engedélyezése hello kapcsolati karakterlánc</span><span class="sxs-lookup"><span data-stu-id="0a542-207">Enable Always Encrypted in hello connection string</span></span>
<span data-ttu-id="0a542-208">Adja hozzá a következő kulcsszó tooyour kapcsolati karakterlánc hello:</span><span class="sxs-lookup"><span data-stu-id="0a542-208">Add hello following keyword tooyour connection string:</span></span>

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-a-sqlconnectionstringbuilder"></a><span data-ttu-id="0a542-209">Egy SqlConnectionStringBuilder mindig titkosítva engedélyezése</span><span class="sxs-lookup"><span data-stu-id="0a542-209">Enable Always Encrypted with a SqlConnectionStringBuilder</span></span>
<span data-ttu-id="0a542-210">hello következő kód bemutatja, hogyan úgy, hogy mindig titkosítja tooenable hello [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) túl[engedélyezve](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).</span><span class="sxs-lookup"><span data-stu-id="0a542-210">hello following code shows how tooenable Always Encrypted by setting hello [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) too[Enabled](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).</span></span>

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;



## <a name="always-encrypted-sample-console-application"></a><span data-ttu-id="0a542-211">Mindig titkosított minta-Konzolalkalmazás</span><span class="sxs-lookup"><span data-stu-id="0a542-211">Always Encrypted sample console application</span></span>
<span data-ttu-id="0a542-212">Ez a példa bemutatja, hogyan:</span><span class="sxs-lookup"><span data-stu-id="0a542-212">This sample demonstrates how to:</span></span>

* <span data-ttu-id="0a542-213">Módosítsa a kapcsolati karakterlánc tooenable mindig titkosítja.</span><span class="sxs-lookup"><span data-stu-id="0a542-213">Modify your connection string tooenable Always Encrypted.</span></span>
* <span data-ttu-id="0a542-214">Adatok beszúrása hello titkosított oszlopokat.</span><span class="sxs-lookup"><span data-stu-id="0a542-214">Insert data into hello encrypted columns.</span></span>
* <span data-ttu-id="0a542-215">Válasszon ki egy olyan rekordot szűrést használ a megadott titkosított oszlop az értéket.</span><span class="sxs-lookup"><span data-stu-id="0a542-215">Select a record by filtering for a specific value in an encrypted column.</span></span>

<span data-ttu-id="0a542-216">Cserélje le a hello tartalmát **Program.cs** a következő kód hello.</span><span class="sxs-lookup"><span data-stu-id="0a542-216">Replace hello contents of **Program.cs** with hello following code.</span></span> <span data-ttu-id="0a542-217">Cserélje le a érvényes kapcsolati karakterláncot az Azure-portálon hello hello kapcsolati karakterlánc hello globális connectionString változó hello sorban fölött hello fő metódus.</span><span class="sxs-lookup"><span data-stu-id="0a542-217">Replace hello connection string for hello global connectionString variable in hello line directly above hello Main method with your valid connection string from hello Azure portal.</span></span> <span data-ttu-id="0a542-218">Ez a hello mindössze annyi a változás toomake toothis kód van szüksége.</span><span class="sxs-lookup"><span data-stu-id="0a542-218">This is hello only change you need toomake toothis code.</span></span>

<span data-ttu-id="0a542-219">Futtassa a hello app toosee mindig titkosítja a művelet.</span><span class="sxs-lookup"><span data-stu-id="0a542-219">Run hello app toosee Always Encrypted in action.</span></span>

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


## <a name="verify-that-hello-data-is-encrypted"></a><span data-ttu-id="0a542-220">Győződjön meg arról, hogy hello adattitkosítás</span><span class="sxs-lookup"><span data-stu-id="0a542-220">Verify that hello data is encrypted</span></span>
<span data-ttu-id="0a542-221">Gyorsan ellenőrizheti, hogy hello tényleges hello kiszolgálón adattitkosítás hello lekérdezésével **betegek** ssms alkalmazásával adatokat.</span><span class="sxs-lookup"><span data-stu-id="0a542-221">You can quickly check that hello actual data on hello server is encrypted by querying hello **Patients** data with SSMS.</span></span> <span data-ttu-id="0a542-222">(Az aktuális kapcsolat használatát ahol hello oszloptitkosítási beállítás még nincs engedélyezve.)</span><span class="sxs-lookup"><span data-stu-id="0a542-222">(Use your current connection where hello column encryption setting is not yet enabled.)</span></span>

<span data-ttu-id="0a542-223">Futtassa a következő lekérdezés hello klinikán adatbázison hello.</span><span class="sxs-lookup"><span data-stu-id="0a542-223">Run hello following query on hello Clinic database.</span></span>

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

<span data-ttu-id="0a542-224">Láthatja, hogy hello titkosított oszlop nem tartalmaz az egyszerű szöveges adatokat.</span><span class="sxs-lookup"><span data-stu-id="0a542-224">You can see that hello encrypted columns do not contain any plaintext data.</span></span>

   ![Új Konzolalkalmazás](./media/sql-database-always-encrypted/ssms-encrypted.png)

<span data-ttu-id="0a542-226">toouse SSMS tooaccess hello adatok egyszerű szövegként, hozzáadhat hello **Oszloptitkosítási beállítás = engedélyezve** paraméter toohello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="0a542-226">toouse SSMS tooaccess hello plaintext data, you can add hello **Column Encryption Setting=enabled** parameter toohello connection.</span></span>

1. <span data-ttu-id="0a542-227">Az SSMS, kattintson a jobb gombbal a kiszolgáló **Object Explorer**, és kattintson a **Disconnect**.</span><span class="sxs-lookup"><span data-stu-id="0a542-227">In SSMS, right-click your server in **Object Explorer**, and then click **Disconnect**.</span></span>
2. <span data-ttu-id="0a542-228">Kattintson a **Connect** > **adatbázismotor** tooopen hello **tooServer csatlakozás** ablakot, és kattintson **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="0a542-228">Click **Connect** > **Database Engine** tooopen hello **Connect tooServer** window, and then click **Options**.</span></span>
3. <span data-ttu-id="0a542-229">Kattintson a **további kapcsolódási paraméterek** és típus **Oszloptitkosítási beállítás = engedélyezve**.</span><span class="sxs-lookup"><span data-stu-id="0a542-229">Click **Additional Connection Parameters** and type **Column Encryption Setting=enabled**.</span></span>
   
    ![Új Konzolalkalmazás](./media/sql-database-always-encrypted/ssms-connection-parameter.png)
4. <span data-ttu-id="0a542-231">Futtatási hello lekérdezés hello a következő **klinikán** adatbázis.</span><span class="sxs-lookup"><span data-stu-id="0a542-231">Run hello following query on hello **Clinic** database.</span></span>
   
        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;
   
     <span data-ttu-id="0a542-232">Hello egyszerű szöveges adatok titkosítva hello oszlopban láthatja.</span><span class="sxs-lookup"><span data-stu-id="0a542-232">You can now see hello plaintext data in hello encrypted columns.</span></span>

    ![Új Konzolalkalmazás](./media/sql-database-always-encrypted/ssms-plaintext.png)



> [!NOTE]
> <span data-ttu-id="0a542-234">Ha az SSMS (vagy bármely olyan ügyfél) egy másik számítógépről csatlakozik, nem rendelkeznek hozzáféréssel toohello titkosítási kulcsokat, és nem lesz képes toodecrypt hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="0a542-234">If you connect with SSMS (or any client) from a different computer, it will not have access toohello encryption keys and will not be able toodecrypt hello data.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="0a542-235">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0a542-235">Next steps</span></span>
<span data-ttu-id="0a542-236">Miután létrehozott egy adatbázist, amely mindig titkosítja használ, érdemes lehet toodo hello következő:</span><span class="sxs-lookup"><span data-stu-id="0a542-236">After you create a database that uses Always Encrypted, you may want toodo hello following:</span></span>

* <span data-ttu-id="0a542-237">A minta futtatásához egy másik számítógépen.</span><span class="sxs-lookup"><span data-stu-id="0a542-237">Run this sample from a different computer.</span></span> <span data-ttu-id="0a542-238">Toohello titkosítási kulcsot, akkor nem rendelkezik, ezért nem lesz access toohello egyszerű szöveges adatokat, és nem futtathatók sikeresen.</span><span class="sxs-lookup"><span data-stu-id="0a542-238">It won't have access toohello encryption keys, so it will not have access toohello plaintext data and will not run successfully.</span></span>
* <span data-ttu-id="0a542-239">[Forgassa el, és a kulcsok tisztítása](https://msdn.microsoft.com/library/mt607048.aspx).</span><span class="sxs-lookup"><span data-stu-id="0a542-239">[Rotate and clean up your keys](https://msdn.microsoft.com/library/mt607048.aspx).</span></span>
* <span data-ttu-id="0a542-240">[Már mindig titkosítja a titkosított adatok áttelepítése](https://msdn.microsoft.com/library/mt621539.aspx).</span><span class="sxs-lookup"><span data-stu-id="0a542-240">[Migrate data that is already encrypted with Always Encrypted](https://msdn.microsoft.com/library/mt621539.aspx).</span></span>
* <span data-ttu-id="0a542-241">[Mindig titkosítja tanúsítványok tooother ügyfél gépek telepítése](https://msdn.microsoft.com/library/mt723359.aspx#Anchor_1) (lásd a hello "Tanúsítványok elérhető tooApplications és a felhasználók így" szakaszt).</span><span class="sxs-lookup"><span data-stu-id="0a542-241">[Deploy Always Encrypted certificates tooother client machines](https://msdn.microsoft.com/library/mt723359.aspx#Anchor_1) (see hello "Making Certificates Available tooApplications and Users" section).</span></span>

## <a name="related-information"></a><span data-ttu-id="0a542-242">Kapcsolódó információk</span><span class="sxs-lookup"><span data-stu-id="0a542-242">Related information</span></span>
* [<span data-ttu-id="0a542-243">Mindig titkosítja (ügyféloldali fejlesztés)</span><span class="sxs-lookup"><span data-stu-id="0a542-243">Always Encrypted (client development)</span></span>](https://msdn.microsoft.com/library/mt147923.aspx)
* [<span data-ttu-id="0a542-244">Átlátható adattitkosítás</span><span class="sxs-lookup"><span data-stu-id="0a542-244">Transparent Data Encryption</span></span>](https://msdn.microsoft.com/library/bb934049.aspx)
* [<span data-ttu-id="0a542-245">SQL Server-titkosítás</span><span class="sxs-lookup"><span data-stu-id="0a542-245">SQL Server Encryption</span></span>](https://msdn.microsoft.com/library/bb510663.aspx)
* [<span data-ttu-id="0a542-246">Mindig titkosított varázsló</span><span class="sxs-lookup"><span data-stu-id="0a542-246">Always Encrypted Wizard</span></span>](https://msdn.microsoft.com/library/mt459280.aspx)
* [<span data-ttu-id="0a542-247">Mindig titkosított Blog</span><span class="sxs-lookup"><span data-stu-id="0a542-247">Always Encrypted Blog</span></span>](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)

