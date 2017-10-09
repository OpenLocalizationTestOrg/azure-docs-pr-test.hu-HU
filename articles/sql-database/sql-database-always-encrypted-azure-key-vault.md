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
# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-azure-key-vault"></a><span data-ttu-id="956cd-105">Mindig titkosítja: Az SQL-adatbázis bizalmas adatok védelmét, és a titkosítási kulcsok tárolása az Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="956cd-105">Always Encrypted: Protect sensitive data in SQL Database and store your encryption keys in Azure Key Vault</span></span>

<span data-ttu-id="956cd-106">Ez a cikk bemutatja, hogyan toosecure bizalmas adatait egy SQL-adatbázis használati ideje az adattitkosítás hello segítségével [varázsló mindig titkosítja](https://msdn.microsoft.com/library/mt459280.aspx) a [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx).</span><span class="sxs-lookup"><span data-stu-id="956cd-106">This article shows you how toosecure sensitive data in a SQL database with data encryption using hello [Always Encrypted Wizard](https://msdn.microsoft.com/library/mt459280.aspx) in [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx).</span></span> <span data-ttu-id="956cd-107">Az olyan utasításokat is tartalmaz, amely bemutatja, hogyan toostore minden Azure Key Vault a titkosítási kulcs.</span><span class="sxs-lookup"><span data-stu-id="956cd-107">It also includes instructions that will show you how toostore each encryption key in Azure Key Vault.</span></span>

<span data-ttu-id="956cd-108">Titkosított mindig egy új adatok titkosítás technológia az Azure SQL Database, és, amely segít az SQL Server hello kiszolgálón inaktív bizalmas adat védelme adatátviteli ügyfél és kiszolgáló között, és miközben hello adatok használatban van.</span><span class="sxs-lookup"><span data-stu-id="956cd-108">Always Encrypted is a new data encryption technology in Azure SQL Database and SQL Server that helps protect sensitive data at rest on hello server, during movement between client and server, and while hello data is in use.</span></span> <span data-ttu-id="956cd-109">Mindig titkosított biztosítja, hogy bizalmas adatok soha nem jelenik meg szövegként hello adatbázis rendszerében.</span><span class="sxs-lookup"><span data-stu-id="956cd-109">Always Encrypted ensures that sensitive data never appears as plaintext inside hello database system.</span></span> <span data-ttu-id="956cd-110">Miután konfigurálta az adattitkosítást, csak az ügyfélalkalmazások vagy toohello hívóbetűket alkalmazáskiszolgálókra férhet hozzá egyszerű szöveges adatokhoz.</span><span class="sxs-lookup"><span data-stu-id="956cd-110">After you configure data encryption, only client applications or app servers that have access toohello keys can access plaintext data.</span></span> <span data-ttu-id="956cd-111">Részletes információkért lásd: [(adatbázismotor) mindig titkosítja](https://msdn.microsoft.com/library/mt163865.aspx).</span><span class="sxs-lookup"><span data-stu-id="956cd-111">For detailed information, see [Always Encrypted (Database Engine)](https://msdn.microsoft.com/library/mt163865.aspx).</span></span>

<span data-ttu-id="956cd-112">Miután konfigurálta a hello adatbázis toouse mindig titkosítja, C# nyelven íródtak a Visual Studio toowork hello titkosított adatok hoz létre egy ügyfélalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="956cd-112">After you configure hello database toouse Always Encrypted, you will create a client application in C# with Visual Studio toowork with hello encrypted data.</span></span>

<span data-ttu-id="956cd-113">Kövesse az ebben a cikkben hello lépéseket, és megtudhatja, hogyan tooset mentése mindig titkosítja az Azure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="956cd-113">Follow hello steps in this article and learn how tooset up Always Encrypted for an Azure SQL database.</span></span> <span data-ttu-id="956cd-114">Ebben a cikkben megtudhatja, hogyan tooperform hello a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="956cd-114">In this article you will learn how tooperform hello following tasks:</span></span>

* <span data-ttu-id="956cd-115">Hello mindig titkosítja varázsló használata a szolgáltatáshoz az SSMS toocreate [mindig titkosított kulcsok](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).</span><span class="sxs-lookup"><span data-stu-id="956cd-115">Use hello Always Encrypted wizard in SSMS toocreate [Always Encrypted keys](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).</span></span>
  * <span data-ttu-id="956cd-116">Hozzon létre egy [oszlop főkulcs (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).</span><span class="sxs-lookup"><span data-stu-id="956cd-116">Create a [column master key (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).</span></span>
  * <span data-ttu-id="956cd-117">Hozzon létre egy [oszlop titkosítási kulcsának (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).</span><span class="sxs-lookup"><span data-stu-id="956cd-117">Create a [column encryption key (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).</span></span>
* <span data-ttu-id="956cd-118">Hozzon létre egy adatbázis tábláinak és oszlopok titkosításához.</span><span class="sxs-lookup"><span data-stu-id="956cd-118">Create a database table and encrypt columns.</span></span>
* <span data-ttu-id="956cd-119">Hozzon létre olyan alkalmazás, amely beszúrja, kiválasztása és hello titkosított oszlopok adatait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="956cd-119">Create an application that inserts, selects, and displays data from hello encrypted columns.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="956cd-120">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="956cd-120">Prerequisites</span></span>
<span data-ttu-id="956cd-121">Ebben az oktatóanyagban lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="956cd-121">For this tutorial, you'll need:</span></span>

* <span data-ttu-id="956cd-122">Azure-fiók és -előfizetés.</span><span class="sxs-lookup"><span data-stu-id="956cd-122">An Azure account and subscription.</span></span> <span data-ttu-id="956cd-123">Ha még nincs fiókja, regisztráljon egy [ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="956cd-123">If you don't have one, sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="956cd-124">[SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) 13.0.700.242 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="956cd-124">[SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) version 13.0.700.242 or later.</span></span>
* <span data-ttu-id="956cd-125">[.NET-keretrendszer 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) vagy újabb (ügyfélszámítógépen hello).</span><span class="sxs-lookup"><span data-stu-id="956cd-125">[.NET Framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) or later (on hello client computer).</span></span>
* <span data-ttu-id="956cd-126">[Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="956cd-126">[Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).</span></span>
* <span data-ttu-id="956cd-127">[Az Azure PowerShell](/powershell/azure/overview), 1.0-ás vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="956cd-127">[Azure PowerShell](/powershell/azure/overview), version  1.0 or later.</span></span> <span data-ttu-id="956cd-128">Típus **(Get-Module azure - ListAvailable). Verzió** toosee PowerShell melyik verzióját futtatja.</span><span class="sxs-lookup"><span data-stu-id="956cd-128">Type **(Get-Module azure -ListAvailable).Version** toosee what version of PowerShell you are running.</span></span>

## <a name="enable-your-client-application-tooaccess-hello-sql-database-service"></a><span data-ttu-id="956cd-129">Az ügyfél alkalmazás tooaccess hello SQL Database szolgáltatás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="956cd-129">Enable your client application tooaccess hello SQL Database service</span></span>
<span data-ttu-id="956cd-130">Engedélyeznie kell az ügyfél alkalmazás tooaccess hello SQL Database szolgáltatás hello szükséges hitelesítés és az hello beállításával *ClientId* és *titkos* , hogy szüksége lesz a tooauthenticate a következő kód hello az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="956cd-130">You must enable your client application tooaccess hello SQL Database service by setting up hello required authentication and acquiring hello *ClientId* and *Secret* that you will need tooauthenticate your application in hello following code.</span></span>

1. <span data-ttu-id="956cd-131">Nyissa meg hello [a klasszikus Azure portálon](http://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="956cd-131">Open hello [Azure classic portal](http://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="956cd-132">Válassza ki **Active Directory** , és kattintson az alkalmazás által használt hello Active Directory-példányban.</span><span class="sxs-lookup"><span data-stu-id="956cd-132">Select **Active Directory** and click hello Active Directory instance that your application will use.</span></span>
3. <span data-ttu-id="956cd-133">Kattintson a **alkalmazások**, és kattintson a **hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="956cd-133">Click **Applications**, and then click **ADD**.</span></span>
4. <span data-ttu-id="956cd-134">Írja be az alkalmazás nevét (például: *myClientApp*) elemre, jelölje be **WEBALKALMAZÁS**, és kattintson a nyílra toocontinue hello.</span><span class="sxs-lookup"><span data-stu-id="956cd-134">Type a name for your application (for example: *myClientApp*), select **WEB APPLICATION**, and click hello arrow toocontinue.</span></span>
5. <span data-ttu-id="956cd-135">A hello **SIGN-ON URL** és **APP ID URI** adhatja meg egy érvényes URL-címet (például *http://myClientApp*) és a folytatáshoz.</span><span class="sxs-lookup"><span data-stu-id="956cd-135">For hello **SIGN-ON URL** and **APP ID URI** you can type a valid URL (for example, *http://myClientApp*) and continue.</span></span>
6. <span data-ttu-id="956cd-136">Kattintson a **KONFIGURÁLÁSA**.</span><span class="sxs-lookup"><span data-stu-id="956cd-136">Click **CONFIGURE**.</span></span>
7. <span data-ttu-id="956cd-137">Másolás a **ügyfél-azonosító**.</span><span class="sxs-lookup"><span data-stu-id="956cd-137">Copy your **CLIENT ID**.</span></span> <span data-ttu-id="956cd-138">(Később szüksége lesz ezt az értéket a kódban.)</span><span class="sxs-lookup"><span data-stu-id="956cd-138">(You will need this value in your code later.)</span></span>
8. <span data-ttu-id="956cd-139">A hello **kulcsok** szakaszban jelölje be **1 év** hello a **időtartam kiválasztása** legördülő listában.</span><span class="sxs-lookup"><span data-stu-id="956cd-139">In hello **keys** section, select **1 year** from hello  **Select duration** drop-down list.</span></span> <span data-ttu-id="956cd-140">(Fogja másolni hello kulcs, miután menti a 13.)</span><span class="sxs-lookup"><span data-stu-id="956cd-140">(You will copy hello key after you save in step 13.)</span></span>
9. <span data-ttu-id="956cd-141">Görgessen le, majd kattintson a **alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="956cd-141">Scroll down and click **Add application**.</span></span>
10. <span data-ttu-id="956cd-142">Hagyja **megjelenítése** túl beállítása**Microsoft Apps** válassza **Microsoft Azure szolgáltatásfelügyeleti API**.</span><span class="sxs-lookup"><span data-stu-id="956cd-142">Leave **SHOW** set too**Microsoft Apps** and select **Microsoft Azure Service Management API**.</span></span> <span data-ttu-id="956cd-143">Kattintson a pipa toocontinue hello.</span><span class="sxs-lookup"><span data-stu-id="956cd-143">Click hello checkmark toocontinue.</span></span>
11. <span data-ttu-id="956cd-144">Válassza ki **Azure szolgáltatásfelügyelet eléréséhez...**  a hello **delegált engedélyek** legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="956cd-144">Select **Access Azure Service Management...** from hello **Delegated Permissions** drop-down list.</span></span>
12. <span data-ttu-id="956cd-145">Kattintson a **SAVE** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="956cd-145">Click **SAVE**.</span></span>
13. <span data-ttu-id="956cd-146">Hello mentése befejeződik, után másolása hello kulcsérték hello **kulcsok** szakasz.</span><span class="sxs-lookup"><span data-stu-id="956cd-146">After hello save finishes, copy hello key value in hello **keys** section.</span></span> <span data-ttu-id="956cd-147">(Később szüksége lesz ezt az értéket a kódban.)</span><span class="sxs-lookup"><span data-stu-id="956cd-147">(You will need this value in your code later.)</span></span>

## <a name="create-a-key-vault-toostore-your-keys"></a><span data-ttu-id="956cd-148">A kulcstároló toostore a kulcsok létrehozása</span><span class="sxs-lookup"><span data-stu-id="956cd-148">Create a key vault toostore your keys</span></span>
<span data-ttu-id="956cd-149">Most, hogy az ügyfél alkalmazás van konfigurálva, és az ügyfél-Azonosítóval rendelkezik, idő toocreate kulcstároló, és a hozzáférési házirend konfigurálása a szolgáltatást, és az alkalmazás számára hello tároló titkos kulcsokat (hello mindig titkosított kulcsok).</span><span class="sxs-lookup"><span data-stu-id="956cd-149">Now that your client app is configured and you have your client ID, it's time toocreate a key vault and configure its access policy so you and your application can access hello vault's secrets (hello Always Encrypted keys).</span></span> <span data-ttu-id="956cd-150">Hello *létrehozása*, *beolvasása*, *lista*, *bejelentkezési*, *ellenőrizze*, *wrapKey*, és *unwrapKey* engedélyekre szükség, egy új oszlop főkulcsának létrehozásához és az SQL Server Management Studio titkosítási beállításának.</span><span class="sxs-lookup"><span data-stu-id="956cd-150">hello *create*, *get*, *list*, *sign*, *verify*, *wrapKey*, and *unwrapKey* permissions are required for creating a new column master key and for setting up encryption with SQL Server Management Studio.</span></span>

<span data-ttu-id="956cd-151">Gyorsan létrehozhat egy kulcstartót hello a következő parancsfájl futtatásával.</span><span class="sxs-lookup"><span data-stu-id="956cd-151">You can quickly create a key vault by running hello following script.</span></span> <span data-ttu-id="956cd-152">Ezek a parancsmagok és további információ a létrehozásáról és konfigurálásáról a kulcstároló részletes ismertetése [Ismerkedés az Azure Key Vault](../key-vault/key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="956cd-152">For a detailed explanation of these cmdlets and more information about creating and configuring a key vault, see [Get started with Azure Key Vault](../key-vault/key-vault-get-started.md).</span></span>

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




## <a name="create-a-blank-sql-database"></a><span data-ttu-id="956cd-153">Üres SQL-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="956cd-153">Create a blank SQL database</span></span>
1. <span data-ttu-id="956cd-154">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="956cd-154">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="956cd-155">Nyissa meg túl**új** > **adatok + tárolás** > **SQL-adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="956cd-155">Go too**New** > **Data + Storage** > **SQL Database**.</span></span>
3. <span data-ttu-id="956cd-156">Hozzon létre egy **üres** nevű adatbázis **klinikán** egy új vagy meglévő kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="956cd-156">Create a **Blank** database named **Clinic** on a new or existing server.</span></span> <span data-ttu-id="956cd-157">Hogyan toocreate egy adatbázis hello Azure-portálon: kapcsolatos részletes utasításokat a [az első Azure SQL-adatbázis](sql-database-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="956cd-157">For detailed directions about how toocreate a database in hello Azure portal, see [Your first Azure SQL database](sql-database-get-started-portal.md).</span></span>
   
    ![Hozzon létre egy üres adatbázist](./media/sql-database-always-encrypted-azure-key-vault/create-database.png)

<span data-ttu-id="956cd-159">Akkor lesz szüksége hello kapcsolati karakterlánc hello oktatóanyag későbbi részében, hello adatbázis létrehozása után Tallózás toohello új klinikán adatbázis, és másolja hello kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="956cd-159">You will need hello connection string later in hello tutorial, so after you create hello database, browse toohello new  Clinic database and copy hello connection string.</span></span> <span data-ttu-id="956cd-160">Bármikor kaphat a hello kapcsolati karakterláncot, de egyszerű toocopy legyen hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="956cd-160">You can get hello connection string at any time, but it's easy toocopy it in hello Azure portal.</span></span>

1. <span data-ttu-id="956cd-161">Nyissa meg túl**SQL-adatbázisok** > **klinikán** > **adatbázis-kapcsolati karakterláncok megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="956cd-161">Go too**SQL databases** > **Clinic** > **Show database connection strings**.</span></span>
2. <span data-ttu-id="956cd-162">Másolja a kapcsolati karakterláncot hello **ADO.NET**.</span><span class="sxs-lookup"><span data-stu-id="956cd-162">Copy hello connection string for **ADO.NET**.</span></span>
   
    ![Másolja a hello kapcsolati karakterlánc](./media/sql-database-always-encrypted-azure-key-vault/connection-strings.png)

## <a name="connect-toohello-database-with-ssms"></a><span data-ttu-id="956cd-164">Csatlakozás toohello adatbázis ssms alkalmazásával</span><span class="sxs-lookup"><span data-stu-id="956cd-164">Connect toohello database with SSMS</span></span>
<span data-ttu-id="956cd-165">Nyissa meg a szolgáltatáshoz az SSMS és toohello kiszolgáló kapcsolódni hello klinikán adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="956cd-165">Open SSMS and connect toohello server with hello Clinic database.</span></span>

1. <span data-ttu-id="956cd-166">Nyissa meg a szolgáltatáshoz az SSMS.</span><span class="sxs-lookup"><span data-stu-id="956cd-166">Open SSMS.</span></span> <span data-ttu-id="956cd-167">(Nyissa meg túl**Connect** > **adatbázismotor** tooopen hello **tooServer csatlakozás** ablakot, ha nem, akkor a megnyitott.)</span><span class="sxs-lookup"><span data-stu-id="956cd-167">(Go too**Connect** > **Database Engine** tooopen hello **Connect tooServer** window if it isn't open.)</span></span>
2. <span data-ttu-id="956cd-168">Adja meg a kiszolgáló nevét és hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="956cd-168">Enter your server name and credentials.</span></span> <span data-ttu-id="956cd-169">hello kiszolgálónév hello SQL adatbázis paneljén található, és a kapcsolati karakterláncban hello korábban kimásolt.</span><span class="sxs-lookup"><span data-stu-id="956cd-169">hello server name can be found on hello SQL database blade and in hello connection string you copied earlier.</span></span> <span data-ttu-id="956cd-170">Hello teljes kiszolgáló neve, beleértve a *database.windows.net*.</span><span class="sxs-lookup"><span data-stu-id="956cd-170">Type hello complete server name, including *database.windows.net*.</span></span>
   
    ![Másolja a hello kapcsolati karakterlánc](./media/sql-database-always-encrypted-azure-key-vault/ssms-connect.png)

<span data-ttu-id="956cd-172">Ha hello **Új tűzfalszabály** ablak nyílik tooAzure bejelentkezhet, és lehetővé teszik az SSMS, hozzon létre egy új tűzfalszabályt.</span><span class="sxs-lookup"><span data-stu-id="956cd-172">If hello **New Firewall Rule** window opens, sign in tooAzure and let SSMS create a new firewall rule for you.</span></span>

## <a name="create-a-table"></a><span data-ttu-id="956cd-173">Tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="956cd-173">Create a table</span></span>
<span data-ttu-id="956cd-174">Ebben a szakaszban egy tábla toohold beteg adatokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="956cd-174">In this section, you will create a table toohold patient data.</span></span> <span data-ttu-id="956cd-175">Nincs kezdetben titkosított--állít titkosítási hello a következő szakaszban.</span><span class="sxs-lookup"><span data-stu-id="956cd-175">It's not initially encrypted--you will configure encryption in hello next section.</span></span>

1. <span data-ttu-id="956cd-176">Bontsa ki a **adatbázisok**.</span><span class="sxs-lookup"><span data-stu-id="956cd-176">Expand **Databases**.</span></span>
2. <span data-ttu-id="956cd-177">Kattintson a jobb gombbal hello **klinikán** adatbázis, és kattintson a **új lekérdezés**.</span><span class="sxs-lookup"><span data-stu-id="956cd-177">Right-click hello **Clinic** database and click **New Query**.</span></span>
3. <span data-ttu-id="956cd-178">Beillesztés hello Transact-SQL (T-SQL) következő hello új lekérdezési ablakba és **Execute** azt.</span><span class="sxs-lookup"><span data-stu-id="956cd-178">Paste hello following Transact-SQL (T-SQL) into hello new query window and **Execute** it.</span></span>

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


## <a name="encrypt-columns-configure-always-encrypted"></a><span data-ttu-id="956cd-179">Titkosítani az oszlopok (mindig titkosítja konfigurálása)</span><span class="sxs-lookup"><span data-stu-id="956cd-179">Encrypt columns (configure Always Encrypted)</span></span>
<span data-ttu-id="956cd-180">SSMS egy varázsló, amellyel könnyen beállítható mindig titkosítja hello oszlop főkulcs, az oszlop titkosítási kulcsának és a titkosított oszlopokat beállítása az Ön által itt.</span><span class="sxs-lookup"><span data-stu-id="956cd-180">SSMS provides a wizard that helps you easily configure Always Encrypted by setting up hello column master key, column encryption key, and encrypted columns for you.</span></span>

1. <span data-ttu-id="956cd-181">Bontsa ki a **adatbázisok** > **klinikán** > **táblák**.</span><span class="sxs-lookup"><span data-stu-id="956cd-181">Expand **Databases** > **Clinic** > **Tables**.</span></span>
2. <span data-ttu-id="956cd-182">Kattintson a jobb gombbal hello **betegek** tábla, és válassza ki **titkosítása oszlopok** tooopen hello mindig titkosítja varázsló:</span><span class="sxs-lookup"><span data-stu-id="956cd-182">Right-click hello **Patients** table and select **Encrypt Columns** tooopen hello Always Encrypted wizard:</span></span>
   
    ![Oszlopok titkosítása](./media/sql-database-always-encrypted-azure-key-vault/encrypt-columns.png)

<span data-ttu-id="956cd-184">hello mindig titkosítja varázsló tartalmazza a következő szakaszok hello: **Oszlopválasztás**, **főkulcs konfigurációs**, **érvényesítési**, és **összegzése** .</span><span class="sxs-lookup"><span data-stu-id="956cd-184">hello Always Encrypted wizard includes hello following sections: **Column Selection**, **Master Key Configuration**, **Validation**, and **Summary**.</span></span>

### <a name="column-selection"></a><span data-ttu-id="956cd-185">Oszlop kiválasztása</span><span class="sxs-lookup"><span data-stu-id="956cd-185">Column Selection</span></span>
<span data-ttu-id="956cd-186">Kattintson a **következő** a hello **bemutatása** lap tooopen hello **Oszlopválasztás** lap.</span><span class="sxs-lookup"><span data-stu-id="956cd-186">Click **Next** on hello **Introduction** page tooopen hello **Column Selection** page.</span></span> <span data-ttu-id="956cd-187">Ezen a lapon kiválaszthatja oszlopok tooencrypt, kívánt [hello típusú titkosítást, és milyen oszlop titkosítási kulcsának (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) toouse.</span><span class="sxs-lookup"><span data-stu-id="956cd-187">On this page, you will select which columns you want tooencrypt, [hello type of encryption, and what column encryption key (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) toouse.</span></span>

<span data-ttu-id="956cd-188">Titkosítani **SSN** és **születési dátumot** minden türelmet adatait.</span><span class="sxs-lookup"><span data-stu-id="956cd-188">Encrypt **SSN** and **BirthDate** information for each patient.</span></span> <span data-ttu-id="956cd-189">hello SSN oszlop determinisztikus titkosítás, mely támogatja a egyenlőség keresések, társítások és csoportosítás fogja használni.</span><span class="sxs-lookup"><span data-stu-id="956cd-189">hello SSN column will use deterministic encryption, which supports equality lookups, joins, and group by.</span></span> <span data-ttu-id="956cd-190">hello születési dátumot oszlop véletlenszerű titkosítás, mely nem támogatja a műveleteket fogja használni.</span><span class="sxs-lookup"><span data-stu-id="956cd-190">hello BirthDate column will use randomized encryption, which does not support operations.</span></span>

<span data-ttu-id="956cd-191">Set hello **titkosítási típus** hello SSN oszlop túl**Deterministic** és születési dátumot oszlop túl hello**Randomized**.</span><span class="sxs-lookup"><span data-stu-id="956cd-191">Set hello **Encryption Type** for hello SSN column too**Deterministic** and hello BirthDate column too**Randomized**.</span></span> <span data-ttu-id="956cd-192">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="956cd-192">Click **Next**.</span></span>

![Oszlopok titkosítása](./media/sql-database-always-encrypted-azure-key-vault/column-selection.png)

### <a name="master-key-configuration"></a><span data-ttu-id="956cd-194">A főkulcs konfiguráció</span><span class="sxs-lookup"><span data-stu-id="956cd-194">Master Key Configuration</span></span>
<span data-ttu-id="956cd-195">Hello **főkulcs konfigurációs** lap, ahol beállíthatja a CMK és select hello kulcstároló-szolgáltatóval hello CMK tárolásához.</span><span class="sxs-lookup"><span data-stu-id="956cd-195">hello **Master Key Configuration** page is where you set up your CMK and select hello key store provider where hello CMK will be stored.</span></span> <span data-ttu-id="956cd-196">Jelenleg egy CMK tárolhatja hello Windows tanúsítványtárolójába, az Azure Key Vault vagy egy hardveres biztonsági modul (HSM).</span><span class="sxs-lookup"><span data-stu-id="956cd-196">Currently, you can store a CMK in hello Windows certificate store, Azure Key Vault, or a hardware security module (HSM).</span></span>

<span data-ttu-id="956cd-197">Ez az oktatóanyag bemutatja, hogyan toostore az Azure Key Vault a kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="956cd-197">This tutorial shows how toostore your keys in Azure Key Vault.</span></span>

1. <span data-ttu-id="956cd-198">Válassza ki **az Azure Key Vault**.</span><span class="sxs-lookup"><span data-stu-id="956cd-198">Select **Azure Key Vault**.</span></span>
2. <span data-ttu-id="956cd-199">Válassza ki a kívánt kulcstároló hello hello legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="956cd-199">Select hello desired key vault from hello drop-down list.</span></span>
3. <span data-ttu-id="956cd-200">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="956cd-200">Click **Next**.</span></span>

![A főkulcs konfiguráció](./media/sql-database-always-encrypted-azure-key-vault/master-key-configuration.png)

### <a name="validation"></a><span data-ttu-id="956cd-202">Ellenőrzés</span><span class="sxs-lookup"><span data-stu-id="956cd-202">Validation</span></span>
<span data-ttu-id="956cd-203">Hello oszlopok most titkosítására, vagy egy PowerShell-parancsfájl toorun később mentse.</span><span class="sxs-lookup"><span data-stu-id="956cd-203">You can encrypt hello columns now or save a PowerShell script toorun later.</span></span> <span data-ttu-id="956cd-204">A jelen oktatóanyag esetében válassza ki a **toofinish továbblépni** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="956cd-204">For this tutorial, select **Proceed toofinish now** and click **Next**.</span></span>

### <a name="summary"></a><span data-ttu-id="956cd-205">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="956cd-205">Summary</span></span>
<span data-ttu-id="956cd-206">Győződjön meg arról, hogy hello beállításainak helyességét, és kattintson a **Befejezés** toocomplete hello beállítása mindig titkosítja.</span><span class="sxs-lookup"><span data-stu-id="956cd-206">Verify that hello settings are all correct and click **Finish** toocomplete hello setup for Always Encrypted.</span></span>

![Összefoglalás](./media/sql-database-always-encrypted-azure-key-vault/summary.png)

### <a name="verify-hello-wizards-actions"></a><span data-ttu-id="956cd-208">Ellenőrizze a hello varázsló műveletek</span><span class="sxs-lookup"><span data-stu-id="956cd-208">Verify hello wizard's actions</span></span>
<span data-ttu-id="956cd-209">Hello varázsló befejezése után az adatbázis be van állítva mindig titkosítja.</span><span class="sxs-lookup"><span data-stu-id="956cd-209">After hello wizard is finished, your database is set up for Always Encrypted.</span></span> <span data-ttu-id="956cd-210">hello végre varázsló hello a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="956cd-210">hello wizard performed hello following actions:</span></span>

* <span data-ttu-id="956cd-211">Egy oszlop főkulcs létrehozva és tárolva az Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="956cd-211">Created a column master key and stored it in Azure Key Vault.</span></span>
* <span data-ttu-id="956cd-212">Egy oszlop titkosítási kulcsának létrehozva és tárolva az Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="956cd-212">Created a column encryption key and stored it in Azure Key Vault.</span></span>
* <span data-ttu-id="956cd-213">Konfigurált hello kijelölt oszlopok titkosításhoz.</span><span class="sxs-lookup"><span data-stu-id="956cd-213">Configured hello selected columns for encryption.</span></span> <span data-ttu-id="956cd-214">hello betegek tábla jelenleg nem tartalmaz adatokat, de most Titkosított hello a kijelölt oszlopokban szereplő adatokat.</span><span class="sxs-lookup"><span data-stu-id="956cd-214">hello Patients table currently has no data, but any existing data in hello selected columns is now encrypted.</span></span>

<span data-ttu-id="956cd-215">Ellenőrizheti a szolgáltatáshoz az ssms hello kulcsok hello létrehozását kibontásával **klinikán** > **biztonsági** > **mindig a titkosított kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="956cd-215">You can verify hello creation of hello keys in SSMS by expanding **Clinic** > **Security** > **Always Encrypted Keys**.</span></span>

## <a name="create-a-client-application-that-works-with-hello-encrypted-data"></a><span data-ttu-id="956cd-216">Hozzon létre egy ügyfélalkalmazást, amely kompatibilis a hello titkosított adatok</span><span class="sxs-lookup"><span data-stu-id="956cd-216">Create a client application that works with hello encrypted data</span></span>
<span data-ttu-id="956cd-217">Most, hogy mindig titkosítja be van állítva, egy alkalmazás, amely elvégzi hozhat létre *beszúrása* és *kiválasztja* hello a titkosított oszlopokat.</span><span class="sxs-lookup"><span data-stu-id="956cd-217">Now that Always Encrypted is set up, you can build an application that performs *inserts* and *selects* on hello encrypted columns.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="956cd-218">Az alkalmazást kell használnia [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) történő átadásakor egyszerű szöveges adatok toohello kiszolgáló mindig titkosítja oszlopokkal objektumokat.</span><span class="sxs-lookup"><span data-stu-id="956cd-218">Your application must use [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) objects when passing plaintext data toohello server with Always Encrypted columns.</span></span> <span data-ttu-id="956cd-219">Értékek szövegkonstansban SqlParameter objektumok használata nélkül átadja azt eredményezi, hogy kivételt.</span><span class="sxs-lookup"><span data-stu-id="956cd-219">Passing literal values without using SqlParameter objects will result in an exception.</span></span>
> 
> 

1. <span data-ttu-id="956cd-220">Nyissa meg a Visual Studio, és hozzon létre egy új C# **Konzolalkalmazás** (Visual Studio 2015-ös vagy korábbi) vagy **Konzolalkalmazás (.NET-keretrendszer)** (Visual Studio 2017 és újabb).</span><span class="sxs-lookup"><span data-stu-id="956cd-220">Open Visual Studio and create a new C# **Console Application** (Visual Studio 2015 and earlier) or **Console App (.NET Framework)** (Visual Studio 2017 and later).</span></span> <span data-ttu-id="956cd-221">Ellenőrizze, hogy túl van-e állítva a projekt**.NET-keretrendszer 4.6** vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="956cd-221">Make sure your project is set too**.NET Framework 4.6** or later.</span></span>
2. <span data-ttu-id="956cd-222">Név hello projekt **AlwaysEncryptedConsoleAKVApp** kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="956cd-222">Name hello project **AlwaysEncryptedConsoleAKVApp** and click **OK**.</span></span>
3. <span data-ttu-id="956cd-223">Telepítse a következő NuGet-csomagok túl címen hello**eszközök** > **NuGet-Csomagkezelő** > **Csomagkezelő konzol**.</span><span class="sxs-lookup"><span data-stu-id="956cd-223">Install hello following NuGet packages by going too**Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

<span data-ttu-id="956cd-224">E két sornyi kód futtatása hello Csomagkezelő konzol.</span><span class="sxs-lookup"><span data-stu-id="956cd-224">Run these two lines of code in hello Package Manager Console.</span></span>

    Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory



## <a name="modify-your-connection-string-tooenable-always-encrypted"></a><span data-ttu-id="956cd-225">A kapcsolati karakterlánc tooenable mindig titkosítja módosítása</span><span class="sxs-lookup"><span data-stu-id="956cd-225">Modify your connection string tooenable Always Encrypted</span></span>
<span data-ttu-id="956cd-226">Ez a szakasz azt ismerteti, hogyan tooenable mindig titkosítja az adatbázis-kapcsolati karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="956cd-226">This section  explains how tooenable Always Encrypted in your database connection string.</span></span>

<span data-ttu-id="956cd-227">tooenable mindig titkosítja, kell tooadd hello **Oszloptitkosítási beállítás** kulcsszó tooyour kapcsolati karakterláncot, és állítsa be úgy túl**engedélyezve**.</span><span class="sxs-lookup"><span data-stu-id="956cd-227">tooenable Always Encrypted, you need tooadd hello **Column Encryption Setting** keyword tooyour connection string and set it too**Enabled**.</span></span>

<span data-ttu-id="956cd-228">Közvetlen kapcsolat-karakterláncban hello állíthat, vagy beállíthatja a [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx).</span><span class="sxs-lookup"><span data-stu-id="956cd-228">You can set this directly in hello connection string, or you can set it by using [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx).</span></span> <span data-ttu-id="956cd-229">Hogyan csak a következő szakasz azt mutatja be hello mintaalkalmazáshoz hello toouse **SqlConnectionStringBuilder**.</span><span class="sxs-lookup"><span data-stu-id="956cd-229">hello sample application in hello next section shows how toouse **SqlConnectionStringBuilder**.</span></span>

### <a name="enable-always-encrypted-in-hello-connection-string"></a><span data-ttu-id="956cd-230">Mindig titkosítja engedélyezése hello kapcsolati karakterlánc</span><span class="sxs-lookup"><span data-stu-id="956cd-230">Enable Always Encrypted in hello connection string</span></span>
<span data-ttu-id="956cd-231">Adja hozzá a következő kulcsszó tooyour kapcsolati karakterlánc hello.</span><span class="sxs-lookup"><span data-stu-id="956cd-231">Add hello following keyword tooyour connection string.</span></span>

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-sqlconnectionstringbuilder"></a><span data-ttu-id="956cd-232">Mindig titkosítja a SqlConnectionStringBuilder engedélyezése</span><span class="sxs-lookup"><span data-stu-id="956cd-232">Enable Always Encrypted with SqlConnectionStringBuilder</span></span>
<span data-ttu-id="956cd-233">a következő kód bemutatja hogyan hello úgy, hogy mindig titkosítja tooenable [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) túl[engedélyezve](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).</span><span class="sxs-lookup"><span data-stu-id="956cd-233">hello following code shows how tooenable Always Encrypted by setting [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) too[Enabled](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).</span></span>

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;

## <a name="register-hello-azure-key-vault-provider"></a><span data-ttu-id="956cd-234">Hello Azure Key Vault-szolgáltató regisztrálása</span><span class="sxs-lookup"><span data-stu-id="956cd-234">Register hello Azure Key Vault provider</span></span>
<span data-ttu-id="956cd-235">hello következő kód bemutatja, hogyan tooregister hello Azure Key Vault szolgáltató hello ADO.NET illesztővel.</span><span class="sxs-lookup"><span data-stu-id="956cd-235">hello following code shows how tooregister hello Azure Key Vault provider with hello ADO.NET driver.</span></span>

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



## <a name="always-encrypted-sample-console-application"></a><span data-ttu-id="956cd-236">Mindig titkosított minta-Konzolalkalmazás</span><span class="sxs-lookup"><span data-stu-id="956cd-236">Always Encrypted sample console application</span></span>
<span data-ttu-id="956cd-237">Ez a példa bemutatja, hogyan:</span><span class="sxs-lookup"><span data-stu-id="956cd-237">This sample demonstrates how to:</span></span>

* <span data-ttu-id="956cd-238">Módosítsa a kapcsolati karakterlánc tooenable mindig titkosítja.</span><span class="sxs-lookup"><span data-stu-id="956cd-238">Modify your connection string tooenable Always Encrypted.</span></span>
* <span data-ttu-id="956cd-239">Az Azure Key Vault regisztrálása hello alkalmazás kulcstároló-szolgáltatóval.</span><span class="sxs-lookup"><span data-stu-id="956cd-239">Register Azure Key Vault as hello application's key store provider.</span></span>  
* <span data-ttu-id="956cd-240">Adatok beszúrása hello titkosított oszlopokat.</span><span class="sxs-lookup"><span data-stu-id="956cd-240">Insert data into hello encrypted columns.</span></span>
* <span data-ttu-id="956cd-241">Válasszon ki egy olyan rekordot szűrést használ a megadott titkosított oszlop az értéket.</span><span class="sxs-lookup"><span data-stu-id="956cd-241">Select a record by filtering for a specific value in an encrypted column.</span></span>

<span data-ttu-id="956cd-242">Cserélje le a hello tartalmát **Program.cs** a következő kód hello.</span><span class="sxs-lookup"><span data-stu-id="956cd-242">Replace hello contents of **Program.cs** with hello following code.</span></span> <span data-ttu-id="956cd-243">Cserélje le a kapcsolati karakterlánc hello hello globális connectionString változó hello sorban hello fő metódus az érvénytelen kapcsolati karakterlánccal hello Azure-portálon a közvetlenül megelőző.</span><span class="sxs-lookup"><span data-stu-id="956cd-243">Replace hello connection string for hello global connectionString variable in hello line that directly precedes hello Main method with your valid connection string from hello Azure portal.</span></span> <span data-ttu-id="956cd-244">Ez a hello mindössze annyi a változás toomake toothis kód van szüksége.</span><span class="sxs-lookup"><span data-stu-id="956cd-244">This is hello only change you need toomake toothis code.</span></span>

<span data-ttu-id="956cd-245">Futtassa a hello app toosee mindig titkosítja a művelet.</span><span class="sxs-lookup"><span data-stu-id="956cd-245">Run hello app toosee Always Encrypted in action.</span></span>

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



## <a name="verify-that-hello-data-is-encrypted"></a><span data-ttu-id="956cd-246">Győződjön meg arról, hogy hello adattitkosítás</span><span class="sxs-lookup"><span data-stu-id="956cd-246">Verify that hello data is encrypted</span></span>
<span data-ttu-id="956cd-247">Gyorsan ellenőrizheti, hogy hello tényleges hello kiszolgálón adattitkosítás hello betegek adatok ssms alkalmazásával lekérdezésével (az aktuális keresztül ahol **Oszloptitkosítási beállítás** még nincs engedélyezve).</span><span class="sxs-lookup"><span data-stu-id="956cd-247">You can quickly check that hello actual data on hello server is encrypted by querying hello Patients data with SSMS (using your current connection where **Column Encryption Setting** is not yet enabled).</span></span>

<span data-ttu-id="956cd-248">Futtassa a következő lekérdezés hello klinikán adatbázison hello.</span><span class="sxs-lookup"><span data-stu-id="956cd-248">Run hello following query on hello Clinic database.</span></span>

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

<span data-ttu-id="956cd-249">Láthatja, hogy hello titkosított oszlop nem tartalmaz az egyszerű szöveges adatokat.</span><span class="sxs-lookup"><span data-stu-id="956cd-249">You can see that hello encrypted columns do not contain any plaintext data.</span></span>

   ![Új Konzolalkalmazás](./media/sql-database-always-encrypted-azure-key-vault/ssms-encrypted.png)

<span data-ttu-id="956cd-251">toouse SSMS tooaccess hello adatok egyszerű szövegként, hozzáadhat hello *Oszloptitkosítási beállítás = engedélyezve* paraméter toohello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="956cd-251">toouse SSMS tooaccess hello plaintext data, you can add hello *Column Encryption Setting=enabled* parameter toohello connection.</span></span>

1. <span data-ttu-id="956cd-252">Az SSMS, kattintson a jobb gombbal a kiszolgáló **Object Explorer** válassza **Disconnect**.</span><span class="sxs-lookup"><span data-stu-id="956cd-252">In SSMS, right-click your server in **Object Explorer** and choose **Disconnect**.</span></span>
2. <span data-ttu-id="956cd-253">Kattintson a **Connect** > **adatbázismotor** tooopen hello **tooServer csatlakozás** ablakot, és kattintson **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="956cd-253">Click **Connect** > **Database Engine** tooopen hello **Connect tooServer** window and click **Options**.</span></span>
3. <span data-ttu-id="956cd-254">Kattintson a **további kapcsolódási paraméterek** és típus **Oszloptitkosítási beállítás = engedélyezve**.</span><span class="sxs-lookup"><span data-stu-id="956cd-254">Click **Additional Connection Parameters** and type **Column Encryption Setting=enabled**.</span></span>
   
    ![Új Konzolalkalmazás](./media/sql-database-always-encrypted-azure-key-vault/ssms-connection-parameter.png)
4. <span data-ttu-id="956cd-256">Futtassa a következő lekérdezés hello klinikán adatbázison hello.</span><span class="sxs-lookup"><span data-stu-id="956cd-256">Run hello following query on hello Clinic database.</span></span>
   
        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;
   
     <span data-ttu-id="956cd-257">Hello egyszerű szöveges adatok titkosítva hello oszlopban láthatja.</span><span class="sxs-lookup"><span data-stu-id="956cd-257">You can now see hello plaintext data in hello encrypted columns.</span></span>

    ![Új Konzolalkalmazás](./media/sql-database-always-encrypted-azure-key-vault/ssms-plaintext.png)


## <a name="next-steps"></a><span data-ttu-id="956cd-259">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="956cd-259">Next steps</span></span>
<span data-ttu-id="956cd-260">Miután létrehozott egy adatbázist, amely mindig titkosítja használ, érdemes lehet toodo hello következő:</span><span class="sxs-lookup"><span data-stu-id="956cd-260">After you create a database that uses Always Encrypted, you may want toodo hello following:</span></span>

* <span data-ttu-id="956cd-261">[Forgassa el, és a kulcsok tisztítása](https://msdn.microsoft.com/library/mt607048.aspx).</span><span class="sxs-lookup"><span data-stu-id="956cd-261">[Rotate and clean up your keys](https://msdn.microsoft.com/library/mt607048.aspx).</span></span>
* <span data-ttu-id="956cd-262">[Már mindig titkosítja a titkosított adatok áttelepítése](https://msdn.microsoft.com/library/mt621539.aspx).</span><span class="sxs-lookup"><span data-stu-id="956cd-262">[Migrate data that is already encrypted with Always Encrypted](https://msdn.microsoft.com/library/mt621539.aspx).</span></span>

## <a name="related-information"></a><span data-ttu-id="956cd-263">Kapcsolódó információk</span><span class="sxs-lookup"><span data-stu-id="956cd-263">Related information</span></span>
* [<span data-ttu-id="956cd-264">Mindig titkosítja (ügyféloldali fejlesztés)</span><span class="sxs-lookup"><span data-stu-id="956cd-264">Always Encrypted (client development)</span></span>](https://msdn.microsoft.com/library/mt147923.aspx)
* [<span data-ttu-id="956cd-265">Transzparens adattitkosítás</span><span class="sxs-lookup"><span data-stu-id="956cd-265">Transparent data encryption</span></span>](https://msdn.microsoft.com/library/bb934049.aspx)
* [<span data-ttu-id="956cd-266">SQL Server-titkosítás</span><span class="sxs-lookup"><span data-stu-id="956cd-266">SQL Server encryption</span></span>](https://msdn.microsoft.com/library/bb510663.aspx)
* [<span data-ttu-id="956cd-267">Mindig titkosított varázsló</span><span class="sxs-lookup"><span data-stu-id="956cd-267">Always Encrypted wizard</span></span>](https://msdn.microsoft.com/library/mt459280.aspx)
* [<span data-ttu-id="956cd-268">Mindig titkosított blog</span><span class="sxs-lookup"><span data-stu-id="956cd-268">Always Encrypted blog</span></span>](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)

