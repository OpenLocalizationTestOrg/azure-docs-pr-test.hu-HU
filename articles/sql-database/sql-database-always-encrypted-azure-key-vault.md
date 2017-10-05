---
title: "Mindig titkosítja: SQL-adatbázishoz – az Azure Key Vault |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan bizalmas adatok védelmének SQL-adatbázisban a varázslóval mindig titkosítja az SQL Server Management Studio adatok titkosításával. Bemutatja, hogyan egyes titkosítási kulcsok tárolására az Azure Key Vault utasításokat is tartalmaz."
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
ms.openlocfilehash: 61bfd420425b4740f6d4ebc01a403a88ff351382
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-azure-key-vault"></a><span data-ttu-id="3d079-105">Mindig titkosítja: Az SQL-adatbázis bizalmas adatok védelmét, és a titkosítási kulcsok tárolása az Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="3d079-105">Always Encrypted: Protect sensitive data in SQL Database and store your encryption keys in Azure Key Vault</span></span>

<span data-ttu-id="3d079-106">A cikkből megtudhatja, mennyire biztonságos adatok titkosítás használata az SQL-adatbázisban lévő bizalmas adatokat a [varázsló mindig titkosítja](https://msdn.microsoft.com/library/mt459280.aspx) a [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx).</span><span class="sxs-lookup"><span data-stu-id="3d079-106">This article shows you how to secure sensitive data in a SQL database with data encryption using the [Always Encrypted Wizard](https://msdn.microsoft.com/library/mt459280.aspx) in [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx).</span></span> <span data-ttu-id="3d079-107">Bemutatja, hogyan egyes titkosítási kulcsok tárolására az Azure Key Vault utasításokat is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="3d079-107">It also includes instructions that will show you how to store each encryption key in Azure Key Vault.</span></span>

<span data-ttu-id="3d079-108">Mindig titkosított egy olyan új adatok titkosítás technológia az Azure SQL Database és SQL Server, amely segít megvédeni a bizalmas adatok aktívan a kiszolgálón, ügyfél és kiszolgáló között, és miközben használatban van az adatok mozgása során.</span><span class="sxs-lookup"><span data-stu-id="3d079-108">Always Encrypted is a new data encryption technology in Azure SQL Database and SQL Server that helps protect sensitive data at rest on the server, during movement between client and server, and while the data is in use.</span></span> <span data-ttu-id="3d079-109">Mindig titkosított biztosítja, hogy bizalmas adatok soha nem jelenik meg az adatbázis rendszerében szövegként.</span><span class="sxs-lookup"><span data-stu-id="3d079-109">Always Encrypted ensures that sensitive data never appears as plaintext inside the database system.</span></span> <span data-ttu-id="3d079-110">Miután konfigurálta az adattitkosítást, csak az ügyfélalkalmazások vagy a kulcsoknak access app kiszolgálók férhet hozzá egyszerű szöveges adatokhoz.</span><span class="sxs-lookup"><span data-stu-id="3d079-110">After you configure data encryption, only client applications or app servers that have access to the keys can access plaintext data.</span></span> <span data-ttu-id="3d079-111">Részletes információkért lásd: [(adatbázismotor) mindig titkosítja](https://msdn.microsoft.com/library/mt163865.aspx).</span><span class="sxs-lookup"><span data-stu-id="3d079-111">For detailed information, see [Always Encrypted (Database Engine)](https://msdn.microsoft.com/library/mt163865.aspx).</span></span>

<span data-ttu-id="3d079-112">Miután konfigurálta az adatbázist mindig titkosítja, a C# a Visual Studio, a titkosított adatok hoz létre egy ügyfélalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3d079-112">After you configure the database to use Always Encrypted, you will create a client application in C# with Visual Studio to work with the encrypted data.</span></span>

<span data-ttu-id="3d079-113">Kövesse a cikkben leírt lépéseket, és megtudhatja, hogyan állíthat be mindig titkosítja az Azure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="3d079-113">Follow the steps in this article and learn how to set up Always Encrypted for an Azure SQL database.</span></span> <span data-ttu-id="3d079-114">Ebben a cikkben megtudhatja, hogyan a következő feladatok végezhetők el:</span><span class="sxs-lookup"><span data-stu-id="3d079-114">In this article you will learn how to perform the following tasks:</span></span>

* <span data-ttu-id="3d079-115">A varázslóval mindig titkosítja az SSMS létrehozásához [mindig titkosított kulcsok](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).</span><span class="sxs-lookup"><span data-stu-id="3d079-115">Use the Always Encrypted wizard in SSMS to create [Always Encrypted keys](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).</span></span>
  * <span data-ttu-id="3d079-116">Hozzon létre egy [oszlop főkulcs (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).</span><span class="sxs-lookup"><span data-stu-id="3d079-116">Create a [column master key (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).</span></span>
  * <span data-ttu-id="3d079-117">Hozzon létre egy [oszlop titkosítási kulcsának (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).</span><span class="sxs-lookup"><span data-stu-id="3d079-117">Create a [column encryption key (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).</span></span>
* <span data-ttu-id="3d079-118">Hozzon létre egy adatbázis tábláinak és oszlopok titkosításához.</span><span class="sxs-lookup"><span data-stu-id="3d079-118">Create a database table and encrypt columns.</span></span>
* <span data-ttu-id="3d079-119">Hozzon létre egy alkalmazást, amely beszúrja, kiválasztása és a titkosított oszlopokban adatait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="3d079-119">Create an application that inserts, selects, and displays data from the encrypted columns.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3d079-120">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3d079-120">Prerequisites</span></span>
<span data-ttu-id="3d079-121">Ebben az oktatóanyagban lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="3d079-121">For this tutorial, you'll need:</span></span>

* <span data-ttu-id="3d079-122">Azure-fiók és -előfizetés.</span><span class="sxs-lookup"><span data-stu-id="3d079-122">An Azure account and subscription.</span></span> <span data-ttu-id="3d079-123">Ha még nincs fiókja, regisztráljon egy [ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3d079-123">If you don't have one, sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="3d079-124">[SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) 13.0.700.242 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="3d079-124">[SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) version 13.0.700.242 or later.</span></span>
* <span data-ttu-id="3d079-125">[.NET-keretrendszer 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) vagy újabb (az ügyfélszámítógépen).</span><span class="sxs-lookup"><span data-stu-id="3d079-125">[.NET Framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) or later (on the client computer).</span></span>
* <span data-ttu-id="3d079-126">[Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="3d079-126">[Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).</span></span>
* <span data-ttu-id="3d079-127">[Az Azure PowerShell](/powershell/azure/overview), 1.0-ás vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="3d079-127">[Azure PowerShell](/powershell/azure/overview), version  1.0 or later.</span></span> <span data-ttu-id="3d079-128">Típus **(Get-Module azure - ListAvailable). Verzió** futnak PowerShell verziójának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="3d079-128">Type **(Get-Module azure -ListAvailable).Version** to see what version of PowerShell you are running.</span></span>

## <a name="enable-your-client-application-to-access-the-sql-database-service"></a><span data-ttu-id="3d079-129">Az ügyfélalkalmazás számára az SQL Database szolgáltatás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="3d079-129">Enable your client application to access the SQL Database service</span></span>
<span data-ttu-id="3d079-130">Engedélyeznie kell az ügyfélalkalmazás az SQL Database szolgáltatás elérését a szükséges hitelesítés és az beszerzése a *ClientId* és *titkos* hitelesítéséhez szükséges a a következő kódot az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3d079-130">You must enable your client application to access the SQL Database service by setting up the required authentication and acquiring the *ClientId* and *Secret* that you will need to authenticate your application in the following code.</span></span>

1. <span data-ttu-id="3d079-131">Nyissa meg a [a klasszikus Azure portálon](http://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="3d079-131">Open the [Azure classic portal](http://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="3d079-132">Válassza ki **Active Directory** , és kattintson az alkalmazás által használt Active Directory-példányban.</span><span class="sxs-lookup"><span data-stu-id="3d079-132">Select **Active Directory** and click the Active Directory instance that your application will use.</span></span>
3. <span data-ttu-id="3d079-133">Kattintson a **alkalmazások**, és kattintson a **hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="3d079-133">Click **Applications**, and then click **ADD**.</span></span>
4. <span data-ttu-id="3d079-134">Adjon meg egy nevet az alkalmazáshoz (például: *myClientApp*) elemre, jelölje be **WEBALKALMAZÁS**, és kattintson a nyílra, a folytatáshoz.</span><span class="sxs-lookup"><span data-stu-id="3d079-134">Type a name for your application (for example: *myClientApp*), select **WEB APPLICATION**, and click the arrow to continue.</span></span>
5. <span data-ttu-id="3d079-135">Az a **SIGN-ON URL** és **APP ID URI** adhatja meg egy érvényes URL-címet (például *http://myClientApp*) és a folytatáshoz.</span><span class="sxs-lookup"><span data-stu-id="3d079-135">For the **SIGN-ON URL** and **APP ID URI** you can type a valid URL (for example, *http://myClientApp*) and continue.</span></span>
6. <span data-ttu-id="3d079-136">Kattintson a **KONFIGURÁLÁSA**.</span><span class="sxs-lookup"><span data-stu-id="3d079-136">Click **CONFIGURE**.</span></span>
7. <span data-ttu-id="3d079-137">Másolás a **ügyfél-azonosító**.</span><span class="sxs-lookup"><span data-stu-id="3d079-137">Copy your **CLIENT ID**.</span></span> <span data-ttu-id="3d079-138">(Később szüksége lesz ezt az értéket a kódban.)</span><span class="sxs-lookup"><span data-stu-id="3d079-138">(You will need this value in your code later.)</span></span>
8. <span data-ttu-id="3d079-139">A a **kulcsok** szakaszban jelölje be **1 év** a a **válassza ki a duration** legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="3d079-139">In the **keys** section, select **1 year** from the  **Select duration** drop-down list.</span></span> <span data-ttu-id="3d079-140">(Fogja másolni a kulcsot, miután menti a 13.)</span><span class="sxs-lookup"><span data-stu-id="3d079-140">(You will copy the key after you save in step 13.)</span></span>
9. <span data-ttu-id="3d079-141">Görgessen le, majd kattintson a **alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="3d079-141">Scroll down and click **Add application**.</span></span>
10. <span data-ttu-id="3d079-142">Hagyja **megjelenítése** beállítása **Microsoft Apps** válassza **Microsoft Azure szolgáltatásfelügyeleti API**.</span><span class="sxs-lookup"><span data-stu-id="3d079-142">Leave **SHOW** set to **Microsoft Apps** and select **Microsoft Azure Service Management API**.</span></span> <span data-ttu-id="3d079-143">Kattintson a pipa ikonra, a folytatáshoz.</span><span class="sxs-lookup"><span data-stu-id="3d079-143">Click the checkmark to continue.</span></span>
11. <span data-ttu-id="3d079-144">Válassza ki **Azure szolgáltatásfelügyelet eléréséhez...**  a a **delegált engedélyek** legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="3d079-144">Select **Access Azure Service Management...** from the **Delegated Permissions** drop-down list.</span></span>
12. <span data-ttu-id="3d079-145">Kattintson a **SAVE** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="3d079-145">Click **SAVE**.</span></span>
13. <span data-ttu-id="3d079-146">A mentés befejezése után másolja a kulcs értékét a **kulcsok** szakasz.</span><span class="sxs-lookup"><span data-stu-id="3d079-146">After the save finishes, copy the key value in the **keys** section.</span></span> <span data-ttu-id="3d079-147">(Később szüksége lesz ezt az értéket a kódban.)</span><span class="sxs-lookup"><span data-stu-id="3d079-147">(You will need this value in your code later.)</span></span>

## <a name="create-a-key-vault-to-store-your-keys"></a><span data-ttu-id="3d079-148">Hozzon létre egy kulcstartót a kulcsok tárolására</span><span class="sxs-lookup"><span data-stu-id="3d079-148">Create a key vault to store your keys</span></span>
<span data-ttu-id="3d079-149">Most, hogy az ügyfél alkalmazás van konfigurálva, és az ügyfél-Azonosítóval rendelkezik, akkor hozzon létre egy kulcstartót és a hozzáférési házirend konfigurálása a szolgáltatást, és az alkalmazás számára a tároló kulcsait (mindig titkosított kulcsok).</span><span class="sxs-lookup"><span data-stu-id="3d079-149">Now that your client app is configured and you have your client ID, it's time to create a key vault and configure its access policy so you and your application can access the vault's secrets (the Always Encrypted keys).</span></span> <span data-ttu-id="3d079-150">A *létrehozása*, *beolvasása*, *lista*, *bejelentkezési*, *ellenőrizze*, *wrapKey*, és *unwrapKey* engedélyekre szükség, egy új oszlop főkulcsának létrehozásához és az SQL Server Management Studio titkosítási beállításának.</span><span class="sxs-lookup"><span data-stu-id="3d079-150">The *create*, *get*, *list*, *sign*, *verify*, *wrapKey*, and *unwrapKey* permissions are required for creating a new column master key and for setting up encryption with SQL Server Management Studio.</span></span>

<span data-ttu-id="3d079-151">A következő parancsfájl futtatásával gyorsan létrehozhat egy kulcstartót.</span><span class="sxs-lookup"><span data-stu-id="3d079-151">You can quickly create a key vault by running the following script.</span></span> <span data-ttu-id="3d079-152">Ezek a parancsmagok és további információ a létrehozásáról és konfigurálásáról a kulcstároló részletes ismertetése [Ismerkedés az Azure Key Vault](../key-vault/key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="3d079-152">For a detailed explanation of these cmdlets and more information about creating and configuring a key vault, see [Get started with Azure Key Vault](../key-vault/key-vault-get-started.md).</span></span>

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




## <a name="create-a-blank-sql-database"></a><span data-ttu-id="3d079-153">Üres SQL-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="3d079-153">Create a blank SQL database</span></span>
1. <span data-ttu-id="3d079-154">Jelentkezzen be az [Azure Portalra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="3d079-154">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="3d079-155">Ugrás a **új** > **adatok + tárolás** > **SQL-adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="3d079-155">Go to **New** > **Data + Storage** > **SQL Database**.</span></span>
3. <span data-ttu-id="3d079-156">Hozzon létre egy **üres** nevű adatbázis **klinikán** egy új vagy meglévő kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="3d079-156">Create a **Blank** database named **Clinic** on a new or existing server.</span></span> <span data-ttu-id="3d079-157">Az Azure portálon adatbázis létrehozásával kapcsolatos részletes utasításokat lásd: [az első Azure SQL-adatbázis](sql-database-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="3d079-157">For detailed directions about how to create a database in the Azure portal, see [Your first Azure SQL database](sql-database-get-started-portal.md).</span></span>
   
    ![Hozzon létre egy üres adatbázist](./media/sql-database-always-encrypted-azure-key-vault/create-database.png)

<span data-ttu-id="3d079-159">Szüksége lesz a kapcsolati karakterlánc az oktatóanyag későbbi részében, ezért az adatbázis létrehozása után keresse meg az új klinikán adatbázis, és másolja a kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="3d079-159">You will need the connection string later in the tutorial, so after you create the database, browse to the new  Clinic database and copy the connection string.</span></span> <span data-ttu-id="3d079-160">A kapcsolati karakterlánc kaphat bármikor, de könnyen másolja az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="3d079-160">You can get the connection string at any time, but it's easy to copy it in the Azure portal.</span></span>

1. <span data-ttu-id="3d079-161">Ugrás a **SQL-adatbázisok** > **klinikán** > **adatbázis-kapcsolati karakterláncok megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="3d079-161">Go to **SQL databases** > **Clinic** > **Show database connection strings**.</span></span>
2. <span data-ttu-id="3d079-162">Másolja a kapcsolati karakterláncot a **ADO.NET**.</span><span class="sxs-lookup"><span data-stu-id="3d079-162">Copy the connection string for **ADO.NET**.</span></span>
   
    ![Másolja a kapcsolati karakterláncot](./media/sql-database-always-encrypted-azure-key-vault/connection-strings.png)

## <a name="connect-to-the-database-with-ssms"></a><span data-ttu-id="3d079-164">Kapcsolódni az adatbázishoz ssms alkalmazásával</span><span class="sxs-lookup"><span data-stu-id="3d079-164">Connect to the database with SSMS</span></span>
<span data-ttu-id="3d079-165">Nyissa meg a szolgáltatáshoz az SSMS, és csatlakozzon a kiszolgálóhoz, az egészségügyi ellátó intézmény adatbázissal.</span><span class="sxs-lookup"><span data-stu-id="3d079-165">Open SSMS and connect to the server with the Clinic database.</span></span>

1. <span data-ttu-id="3d079-166">Nyissa meg a szolgáltatáshoz az SSMS.</span><span class="sxs-lookup"><span data-stu-id="3d079-166">Open SSMS.</span></span> <span data-ttu-id="3d079-167">(Ugrás **Connect** > **adatbázismotor** megnyitásához a **kapcsolódás a kiszolgálóhoz** ablakot, ha nem, akkor a megnyitott.)</span><span class="sxs-lookup"><span data-stu-id="3d079-167">(Go to **Connect** > **Database Engine** to open the **Connect to Server** window if it isn't open.)</span></span>
2. <span data-ttu-id="3d079-168">Adja meg a kiszolgáló nevét és hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="3d079-168">Enter your server name and credentials.</span></span> <span data-ttu-id="3d079-169">Az SQL-adatbázis paneljén található a kiszolgáló nevét, és a kapcsolati karakterláncban korábban kimásolt.</span><span class="sxs-lookup"><span data-stu-id="3d079-169">The server name can be found on the SQL database blade and in the connection string you copied earlier.</span></span> <span data-ttu-id="3d079-170">Adja meg a teljes kiszolgálónevet, beleértve a *database.windows.net*.</span><span class="sxs-lookup"><span data-stu-id="3d079-170">Type the complete server name, including *database.windows.net*.</span></span>
   
    ![Másolja a kapcsolati karakterláncot](./media/sql-database-always-encrypted-azure-key-vault/ssms-connect.png)

<span data-ttu-id="3d079-172">Ha a **Új tűzfalszabály** ablakban nyitja meg, jelentkezzen be Azure, és lehetővé teszik az SSMS, hozzon létre egy új tűzfalszabályt.</span><span class="sxs-lookup"><span data-stu-id="3d079-172">If the **New Firewall Rule** window opens, sign in to Azure and let SSMS create a new firewall rule for you.</span></span>

## <a name="create-a-table"></a><span data-ttu-id="3d079-173">Tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="3d079-173">Create a table</span></span>
<span data-ttu-id="3d079-174">Ebben a szakaszban egy beteg adatokat tároló tábla hoz létre.</span><span class="sxs-lookup"><span data-stu-id="3d079-174">In this section, you will create a table to hold patient data.</span></span> <span data-ttu-id="3d079-175">Nincs kezdetben titkosított – titkosítás fog konfigurálása a következő szakaszban.</span><span class="sxs-lookup"><span data-stu-id="3d079-175">It's not initially encrypted--you will configure encryption in the next section.</span></span>

1. <span data-ttu-id="3d079-176">Bontsa ki a **adatbázisok**.</span><span class="sxs-lookup"><span data-stu-id="3d079-176">Expand **Databases**.</span></span>
2. <span data-ttu-id="3d079-177">Kattintson a jobb gombbal a **klinikán** adatbázis, és kattintson a **új lekérdezés**.</span><span class="sxs-lookup"><span data-stu-id="3d079-177">Right-click the **Clinic** database and click **New Query**.</span></span>
3. <span data-ttu-id="3d079-178">Illessze be a következő Transact-SQL (T-SQL) az új lekérdezési ablakba és **Execute** azt.</span><span class="sxs-lookup"><span data-stu-id="3d079-178">Paste the following Transact-SQL (T-SQL) into the new query window and **Execute** it.</span></span>

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


## <a name="encrypt-columns-configure-always-encrypted"></a><span data-ttu-id="3d079-179">Titkosítani az oszlopok (mindig titkosítja konfigurálása)</span><span class="sxs-lookup"><span data-stu-id="3d079-179">Encrypt columns (configure Always Encrypted)</span></span>
<span data-ttu-id="3d079-180">SSMS egy varázsló, amellyel könnyen beállítható mindig titkosítja az oszlop, az oszlop titkosítási kulcsának és a titkosított oszlopok beállítása az Ön által itt.</span><span class="sxs-lookup"><span data-stu-id="3d079-180">SSMS provides a wizard that helps you easily configure Always Encrypted by setting up the column master key, column encryption key, and encrypted columns for you.</span></span>

1. <span data-ttu-id="3d079-181">Bontsa ki a **adatbázisok** > **klinikán** > **táblák**.</span><span class="sxs-lookup"><span data-stu-id="3d079-181">Expand **Databases** > **Clinic** > **Tables**.</span></span>
2. <span data-ttu-id="3d079-182">Kattintson a jobb gombbal a **betegek** tábla, és válassza ki **titkosítása oszlopok** mindig titkosítja varázsló megnyitásához:</span><span class="sxs-lookup"><span data-stu-id="3d079-182">Right-click the **Patients** table and select **Encrypt Columns** to open the Always Encrypted wizard:</span></span>
   
    ![Oszlopok titkosítása](./media/sql-database-always-encrypted-azure-key-vault/encrypt-columns.png)

<span data-ttu-id="3d079-184">A mindig titkosítja varázsló az alábbi szakaszokat tartalmazza: **Oszlopválasztás**, **főkulcs konfigurációs**, **érvényesítési**, és **összegzése**.</span><span class="sxs-lookup"><span data-stu-id="3d079-184">The Always Encrypted wizard includes the following sections: **Column Selection**, **Master Key Configuration**, **Validation**, and **Summary**.</span></span>

### <a name="column-selection"></a><span data-ttu-id="3d079-185">Oszlop kiválasztása</span><span class="sxs-lookup"><span data-stu-id="3d079-185">Column Selection</span></span>
<span data-ttu-id="3d079-186">Kattintson a **tovább** a a **bemutatása** lapon nyissa meg a **Oszlopválasztás** lap.</span><span class="sxs-lookup"><span data-stu-id="3d079-186">Click **Next** on the **Introduction** page to open the **Column Selection** page.</span></span> <span data-ttu-id="3d079-187">Ezen a lapon kiválaszthatja titkosítására, oszlopok [a típusú titkosítást, és milyen oszlop titkosítási kulcsának (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) használatára.</span><span class="sxs-lookup"><span data-stu-id="3d079-187">On this page, you will select which columns you want to encrypt, [the type of encryption, and what column encryption key (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) to use.</span></span>

<span data-ttu-id="3d079-188">Titkosítani **SSN** és **születési dátumot** minden türelmet adatait.</span><span class="sxs-lookup"><span data-stu-id="3d079-188">Encrypt **SSN** and **BirthDate** information for each patient.</span></span> <span data-ttu-id="3d079-189">A társadalombiztosítási szám oszlop determinisztikus titkosítás, mely támogatja a egyenlőség keresések, társítások és csoportosítás fogja használni.</span><span class="sxs-lookup"><span data-stu-id="3d079-189">The SSN column will use deterministic encryption, which supports equality lookups, joins, and group by.</span></span> <span data-ttu-id="3d079-190">A születési dátumot oszlop véletlenszerű titkosítás, mely nem támogatja a műveleteket fogja használni.</span><span class="sxs-lookup"><span data-stu-id="3d079-190">The BirthDate column will use randomized encryption, which does not support operations.</span></span>

<span data-ttu-id="3d079-191">Állítsa be a **titkosítási típus** SSN oszlop **Deterministic** és a születési dátumot oszlop **Randomized**.</span><span class="sxs-lookup"><span data-stu-id="3d079-191">Set the **Encryption Type** for the SSN column to **Deterministic** and the BirthDate column to **Randomized**.</span></span> <span data-ttu-id="3d079-192">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="3d079-192">Click **Next**.</span></span>

![Oszlopok titkosítása](./media/sql-database-always-encrypted-azure-key-vault/column-selection.png)

### <a name="master-key-configuration"></a><span data-ttu-id="3d079-194">A főkulcs konfiguráció</span><span class="sxs-lookup"><span data-stu-id="3d079-194">Master Key Configuration</span></span>
<span data-ttu-id="3d079-195">A **főkulcs konfigurációs** lap ahol állítsa be a CMK, és válassza ki a kulcstároló-szolgáltatóval a CMK tárolásához.</span><span class="sxs-lookup"><span data-stu-id="3d079-195">The **Master Key Configuration** page is where you set up your CMK and select the key store provider where the CMK will be stored.</span></span> <span data-ttu-id="3d079-196">Jelenleg egy CMK tárolhatja a Windows tanúsítványtárolóban, az Azure Key Vault vagy egy hardveres biztonsági modul (HSM).</span><span class="sxs-lookup"><span data-stu-id="3d079-196">Currently, you can store a CMK in the Windows certificate store, Azure Key Vault, or a hardware security module (HSM).</span></span>

<span data-ttu-id="3d079-197">Ez az oktatóanyag bemutatja, hogyan tárolja a kulcsokat az Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="3d079-197">This tutorial shows how to store your keys in Azure Key Vault.</span></span>

1. <span data-ttu-id="3d079-198">Válassza ki **az Azure Key Vault**.</span><span class="sxs-lookup"><span data-stu-id="3d079-198">Select **Azure Key Vault**.</span></span>
2. <span data-ttu-id="3d079-199">A legördülő listából válassza ki a kívánt kulcstároló.</span><span class="sxs-lookup"><span data-stu-id="3d079-199">Select the desired key vault from the drop-down list.</span></span>
3. <span data-ttu-id="3d079-200">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="3d079-200">Click **Next**.</span></span>

![A főkulcs konfiguráció](./media/sql-database-always-encrypted-azure-key-vault/master-key-configuration.png)

### <a name="validation"></a><span data-ttu-id="3d079-202">Ellenőrzés</span><span class="sxs-lookup"><span data-stu-id="3d079-202">Validation</span></span>
<span data-ttu-id="3d079-203">Most titkosítani az oszlopokat, vagy mentse később futtatni egy PowerShell-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="3d079-203">You can encrypt the columns now or save a PowerShell script to run later.</span></span> <span data-ttu-id="3d079-204">A jelen oktatóanyag esetében válassza ki a **Befejezés most már továbbléphet** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="3d079-204">For this tutorial, select **Proceed to finish now** and click **Next**.</span></span>

### <a name="summary"></a><span data-ttu-id="3d079-205">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="3d079-205">Summary</span></span>
<span data-ttu-id="3d079-206">Ellenőrizze, hogy a beállítások helyességét, és kattintson a **Befejezés** mindig titkosítja az a telepítés befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="3d079-206">Verify that the settings are all correct and click **Finish** to complete the setup for Always Encrypted.</span></span>

![Összefoglalás](./media/sql-database-always-encrypted-azure-key-vault/summary.png)

### <a name="verify-the-wizards-actions"></a><span data-ttu-id="3d079-208">Ellenőrizze a varázsló műveletek</span><span class="sxs-lookup"><span data-stu-id="3d079-208">Verify the wizard's actions</span></span>
<span data-ttu-id="3d079-209">A varázsló befejezése után az adatbázis be van állítva mindig titkosítja.</span><span class="sxs-lookup"><span data-stu-id="3d079-209">After the wizard is finished, your database is set up for Always Encrypted.</span></span> <span data-ttu-id="3d079-210">A varázsló a következő műveletek végre:</span><span class="sxs-lookup"><span data-stu-id="3d079-210">The wizard performed the following actions:</span></span>

* <span data-ttu-id="3d079-211">Egy oszlop főkulcs létrehozva és tárolva az Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="3d079-211">Created a column master key and stored it in Azure Key Vault.</span></span>
* <span data-ttu-id="3d079-212">Egy oszlop titkosítási kulcsának létrehozva és tárolva az Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="3d079-212">Created a column encryption key and stored it in Azure Key Vault.</span></span>
* <span data-ttu-id="3d079-213">A kijelölt oszlopokat a titkosításhoz konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="3d079-213">Configured the selected columns for encryption.</span></span> <span data-ttu-id="3d079-214">A betegek tábla jelenleg nem tartalmaz adatokat, de most a kijelölt oszlopokban szereplő adatokat titkosított.</span><span class="sxs-lookup"><span data-stu-id="3d079-214">The Patients table currently has no data, but any existing data in the selected columns is now encrypted.</span></span>

<span data-ttu-id="3d079-215">Ellenőrizheti a szolgáltatáshoz az ssms kulcsok létrehozásának kibontásával **klinikán** > **biztonsági** > **mindig a titkosított kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="3d079-215">You can verify the creation of the keys in SSMS by expanding **Clinic** > **Security** > **Always Encrypted Keys**.</span></span>

## <a name="create-a-client-application-that-works-with-the-encrypted-data"></a><span data-ttu-id="3d079-216">Hozzon létre egy ügyfélalkalmazást, amely kompatibilis a titkosított adatok</span><span class="sxs-lookup"><span data-stu-id="3d079-216">Create a client application that works with the encrypted data</span></span>
<span data-ttu-id="3d079-217">Most, hogy mindig titkosítja be van állítva, egy alkalmazás, amely elvégzi hozhat létre *beszúrása* és *kiválasztja* a titkosított oszlopok.</span><span class="sxs-lookup"><span data-stu-id="3d079-217">Now that Always Encrypted is set up, you can build an application that performs *inserts* and *selects* on the encrypted columns.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="3d079-218">Az alkalmazást kell használnia [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) objektumok számára történő átadásakor adatokat egyszerű szöveges oszlopok mindig titkosítja a kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="3d079-218">Your application must use [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) objects when passing plaintext data to the server with Always Encrypted columns.</span></span> <span data-ttu-id="3d079-219">Értékek szövegkonstansban SqlParameter objektumok használata nélkül átadja azt eredményezi, hogy kivételt.</span><span class="sxs-lookup"><span data-stu-id="3d079-219">Passing literal values without using SqlParameter objects will result in an exception.</span></span>
> 
> 

1. <span data-ttu-id="3d079-220">Nyissa meg a Visual Studio, és hozzon létre egy új C# **Konzolalkalmazás** (Visual Studio 2015-ös vagy korábbi) vagy **Konzolalkalmazás (.NET-keretrendszer)** (Visual Studio 2017 és újabb).</span><span class="sxs-lookup"><span data-stu-id="3d079-220">Open Visual Studio and create a new C# **Console Application** (Visual Studio 2015 and earlier) or **Console App (.NET Framework)** (Visual Studio 2017 and later).</span></span> <span data-ttu-id="3d079-221">Győződjön meg arról, hogy a projekt értéke **.NET-keretrendszer 4.6** vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="3d079-221">Make sure your project is set to **.NET Framework 4.6** or later.</span></span>
2. <span data-ttu-id="3d079-222">Nevet a projektnek **AlwaysEncryptedConsoleAKVApp** kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="3d079-222">Name the project **AlwaysEncryptedConsoleAKVApp** and click **OK**.</span></span>
3. <span data-ttu-id="3d079-223">A következő NuGet-csomagok telepítése a **eszközök** > **NuGet-Csomagkezelő** > **Csomagkezelő konzol**.</span><span class="sxs-lookup"><span data-stu-id="3d079-223">Install the following NuGet packages by going to **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

<span data-ttu-id="3d079-224">A Csomagkezelő konzol futtatása a kód két sort.</span><span class="sxs-lookup"><span data-stu-id="3d079-224">Run these two lines of code in the Package Manager Console.</span></span>

    Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory



## <a name="modify-your-connection-string-to-enable-always-encrypted"></a><span data-ttu-id="3d079-225">Ahhoz, hogy mindig titkosítja a kapcsolati karakterlánc módosítása</span><span class="sxs-lookup"><span data-stu-id="3d079-225">Modify your connection string to enable Always Encrypted</span></span>
<span data-ttu-id="3d079-226">Ez a szakasz azt ismerteti, hogyan is engedélyezhető az mindig titkosítja az adatbázis-kapcsolati karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="3d079-226">This section  explains how to enable Always Encrypted in your database connection string.</span></span>

<span data-ttu-id="3d079-227">Mindig titkosítja engedélyezéséhez kell hozzáadnia a **Oszloptitkosítási beállítás** kulcsszót a kapcsolati karakterláncot, és állítsa az értékét **engedélyezve**.</span><span class="sxs-lookup"><span data-stu-id="3d079-227">To enable Always Encrypted, you need to add the **Column Encryption Setting** keyword to your connection string and set it to **Enabled**.</span></span>

<span data-ttu-id="3d079-228">Közvetlenül a kapcsolódási karakterláncban állíthat, vagy beállíthatja a [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx).</span><span class="sxs-lookup"><span data-stu-id="3d079-228">You can set this directly in the connection string, or you can set it by using [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx).</span></span> <span data-ttu-id="3d079-229">A mintaalkalmazást a következő szakasz bemutatja, hogyan használható **SqlConnectionStringBuilder**.</span><span class="sxs-lookup"><span data-stu-id="3d079-229">The sample application in the next section shows how to use **SqlConnectionStringBuilder**.</span></span>

### <a name="enable-always-encrypted-in-the-connection-string"></a><span data-ttu-id="3d079-230">Engedélyezze a mindig titkosítja a kapcsolódási karakterláncban</span><span class="sxs-lookup"><span data-stu-id="3d079-230">Enable Always Encrypted in the connection string</span></span>
<span data-ttu-id="3d079-231">Adja hozzá a következő kulcsszó a kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="3d079-231">Add the following keyword to your connection string.</span></span>

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-sqlconnectionstringbuilder"></a><span data-ttu-id="3d079-232">Mindig titkosítja a SqlConnectionStringBuilder engedélyezése</span><span class="sxs-lookup"><span data-stu-id="3d079-232">Enable Always Encrypted with SqlConnectionStringBuilder</span></span>
<span data-ttu-id="3d079-233">A következő kód bemutatja, hogyan lehet engedélyezni a úgy, hogy mindig titkosítja [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) való [engedélyezve](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).</span><span class="sxs-lookup"><span data-stu-id="3d079-233">The following code shows how to enable Always Encrypted by setting [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) to [Enabled](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).</span></span>

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;

## <a name="register-the-azure-key-vault-provider"></a><span data-ttu-id="3d079-234">Az Azure Key Vault-szolgáltató regisztrálása</span><span class="sxs-lookup"><span data-stu-id="3d079-234">Register the Azure Key Vault provider</span></span>
<span data-ttu-id="3d079-235">A következő kód bemutatja, hogyan regisztrálja az Azure Key Vault-szolgáltató az ADO.NET illesztőprogram.</span><span class="sxs-lookup"><span data-stu-id="3d079-235">The following code shows how to register the Azure Key Vault provider with the ADO.NET driver.</span></span>

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



## <a name="always-encrypted-sample-console-application"></a><span data-ttu-id="3d079-236">Mindig titkosított minta-Konzolalkalmazás</span><span class="sxs-lookup"><span data-stu-id="3d079-236">Always Encrypted sample console application</span></span>
<span data-ttu-id="3d079-237">Ez a példa bemutatja, hogyan:</span><span class="sxs-lookup"><span data-stu-id="3d079-237">This sample demonstrates how to:</span></span>

* <span data-ttu-id="3d079-238">Módosítsa a kapcsolati karakterlánc ahhoz, hogy mindig titkosítja.</span><span class="sxs-lookup"><span data-stu-id="3d079-238">Modify your connection string to enable Always Encrypted.</span></span>
* <span data-ttu-id="3d079-239">Az Azure Key Vault regisztrálni az alkalmazás kulcstároló-szolgáltatóként.</span><span class="sxs-lookup"><span data-stu-id="3d079-239">Register Azure Key Vault as the application's key store provider.</span></span>  
* <span data-ttu-id="3d079-240">Adatok beszúrása a titkosított oszlopokat.</span><span class="sxs-lookup"><span data-stu-id="3d079-240">Insert data into the encrypted columns.</span></span>
* <span data-ttu-id="3d079-241">Válasszon ki egy olyan rekordot szűrést használ a megadott titkosított oszlop az értéket.</span><span class="sxs-lookup"><span data-stu-id="3d079-241">Select a record by filtering for a specific value in an encrypted column.</span></span>

<span data-ttu-id="3d079-242">Cserélje le a tartalmát **Program.cs** a következő kóddal.</span><span class="sxs-lookup"><span data-stu-id="3d079-242">Replace the contents of **Program.cs** with the following code.</span></span> <span data-ttu-id="3d079-243">Cserélje le a kapcsolati karakterláncot a globális connectionString változó a közvetlenül megelőző a fő metódus az Azure-portálon a érvényes kapcsolati karakterlánccal sorban.</span><span class="sxs-lookup"><span data-stu-id="3d079-243">Replace the connection string for the global connectionString variable in the line that directly precedes the Main method with your valid connection string from the Azure portal.</span></span> <span data-ttu-id="3d079-244">Ez akkor tegye ezt a kódot kell mindössze annyi a változás.</span><span class="sxs-lookup"><span data-stu-id="3d079-244">This is the only change you need to make to this code.</span></span>

<span data-ttu-id="3d079-245">Futtassa az alkalmazást, hogy mindig titkosítja a művelet.</span><span class="sxs-lookup"><span data-stu-id="3d079-245">Run the app to see Always Encrypted in action.</span></span>

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
        // Update this line with your Clinic database connection string from the Azure portal.
        static string connectionString = @"<connection string from the portal>";
        static string clientId = @"<client id from step 7 above>";
        static string clientSecret = "<key from step 13 above>";


        static void Main(string[] args)
        {
            InitializeAzureKeyVaultProvider();

            Console.WriteLine("Signed in as: " + _clientCredential.ClientId);

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
            Console.WriteLine(Environment.NewLine + "All the records currently in the Patients table:");

            foreach (Patient patient in SelectAllPatients())
            {
                Console.WriteLine(patient.FirstName + " " + patient.LastName + "\tSSN: " + patient.SSN + "\tBirthdate: " + patient.BirthDate);
            }

            // Get patients by SSN.
            Console.WriteLine(Environment.NewLine + "Now lets locate records by searching the encrypted SSN column.");

            string ssn;

            // This very simple validation only checks that the user entered 11 characters.
            // In production be sure to check all user input and use the best validation for your specific application.
            do
            {
                Console.WriteLine("Please enter a valid SSN (ex. 999-99-0003):");
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
                throw new InvalidOperationException("Failed to obtain the access token");
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



## <a name="verify-that-the-data-is-encrypted"></a><span data-ttu-id="3d079-246">Győződjön meg arról, hogy az adatok titkosítása</span><span class="sxs-lookup"><span data-stu-id="3d079-246">Verify that the data is encrypted</span></span>
<span data-ttu-id="3d079-247">Gyorsan ellenőrizheti, hogy a tényleges adatokat a kiszolgáló titkosítja az ssms alkalmazásával betegek adatok lekérdezése (az aktuális keresztül ahol **Oszloptitkosítási beállítás** még nincs engedélyezve).</span><span class="sxs-lookup"><span data-stu-id="3d079-247">You can quickly check that the actual data on the server is encrypted by querying the Patients data with SSMS (using your current connection where **Column Encryption Setting** is not yet enabled).</span></span>

<span data-ttu-id="3d079-248">Futtassa a következő lekérdezést a klinikán adatbázison.</span><span class="sxs-lookup"><span data-stu-id="3d079-248">Run the following query on the Clinic database.</span></span>

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

<span data-ttu-id="3d079-249">Láthatja, hogy a titkosított oszlop nem tartalmaz az egyszerű szöveges adatokat.</span><span class="sxs-lookup"><span data-stu-id="3d079-249">You can see that the encrypted columns do not contain any plaintext data.</span></span>

   ![Új Konzolalkalmazás](./media/sql-database-always-encrypted-azure-key-vault/ssms-encrypted.png)

<span data-ttu-id="3d079-251">Az egyszerű szöveges adatok eléréséhez használja az SSMS, adja hozzá a *Oszloptitkosítási beállítás = engedélyezve* paraméter a kapcsolatra.</span><span class="sxs-lookup"><span data-stu-id="3d079-251">To use SSMS to access the plaintext data, you can add the *Column Encryption Setting=enabled* parameter to the connection.</span></span>

1. <span data-ttu-id="3d079-252">Az SSMS, kattintson a jobb gombbal a kiszolgáló **Object Explorer** válassza **Disconnect**.</span><span class="sxs-lookup"><span data-stu-id="3d079-252">In SSMS, right-click your server in **Object Explorer** and choose **Disconnect**.</span></span>
2. <span data-ttu-id="3d079-253">Kattintson a **Connect** > **adatbázismotor** megnyitásához a **kapcsolódás a kiszolgálóhoz** ablakot, és kattintson **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="3d079-253">Click **Connect** > **Database Engine** to open the **Connect to Server** window and click **Options**.</span></span>
3. <span data-ttu-id="3d079-254">Kattintson a **további kapcsolódási paraméterek** és típus **Oszloptitkosítási beállítás = engedélyezve**.</span><span class="sxs-lookup"><span data-stu-id="3d079-254">Click **Additional Connection Parameters** and type **Column Encryption Setting=enabled**.</span></span>
   
    ![Új Konzolalkalmazás](./media/sql-database-always-encrypted-azure-key-vault/ssms-connection-parameter.png)
4. <span data-ttu-id="3d079-256">Futtassa a következő lekérdezést a klinikán adatbázison.</span><span class="sxs-lookup"><span data-stu-id="3d079-256">Run the following query on the Clinic database.</span></span>
   
        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;
   
     <span data-ttu-id="3d079-257">Most már megtekintheti a titkosított oszlopokban az egyszerű szöveges adatokat.</span><span class="sxs-lookup"><span data-stu-id="3d079-257">You can now see the plaintext data in the encrypted columns.</span></span>

    ![Új Konzolalkalmazás](./media/sql-database-always-encrypted-azure-key-vault/ssms-plaintext.png)


## <a name="next-steps"></a><span data-ttu-id="3d079-259">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3d079-259">Next steps</span></span>
<span data-ttu-id="3d079-260">Miután létrehozott egy adatbázist, amely mindig titkosítja használ, érdemes lehet tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="3d079-260">After you create a database that uses Always Encrypted, you may want to do the following:</span></span>

* <span data-ttu-id="3d079-261">[Forgassa el, és a kulcsok tisztítása](https://msdn.microsoft.com/library/mt607048.aspx).</span><span class="sxs-lookup"><span data-stu-id="3d079-261">[Rotate and clean up your keys](https://msdn.microsoft.com/library/mt607048.aspx).</span></span>
* <span data-ttu-id="3d079-262">[Már mindig titkosítja a titkosított adatok áttelepítése](https://msdn.microsoft.com/library/mt621539.aspx).</span><span class="sxs-lookup"><span data-stu-id="3d079-262">[Migrate data that is already encrypted with Always Encrypted](https://msdn.microsoft.com/library/mt621539.aspx).</span></span>

## <a name="related-information"></a><span data-ttu-id="3d079-263">Kapcsolódó információk</span><span class="sxs-lookup"><span data-stu-id="3d079-263">Related information</span></span>
* [<span data-ttu-id="3d079-264">Mindig titkosítja (ügyféloldali fejlesztés)</span><span class="sxs-lookup"><span data-stu-id="3d079-264">Always Encrypted (client development)</span></span>](https://msdn.microsoft.com/library/mt147923.aspx)
* [<span data-ttu-id="3d079-265">Transzparens adattitkosítás</span><span class="sxs-lookup"><span data-stu-id="3d079-265">Transparent data encryption</span></span>](https://msdn.microsoft.com/library/bb934049.aspx)
* [<span data-ttu-id="3d079-266">SQL Server-titkosítás</span><span class="sxs-lookup"><span data-stu-id="3d079-266">SQL Server encryption</span></span>](https://msdn.microsoft.com/library/bb510663.aspx)
* [<span data-ttu-id="3d079-267">Mindig titkosított varázsló</span><span class="sxs-lookup"><span data-stu-id="3d079-267">Always Encrypted wizard</span></span>](https://msdn.microsoft.com/library/mt459280.aspx)
* [<span data-ttu-id="3d079-268">Mindig titkosított blog</span><span class="sxs-lookup"><span data-stu-id="3d079-268">Always Encrypted blog</span></span>](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)

