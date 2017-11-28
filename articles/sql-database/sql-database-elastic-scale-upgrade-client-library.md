---
title: "Frissítsen a legújabb elastic database ügyféloldali kódtára a |} Microsoft Docs"
description: "Frissítse az alkalmazások és a Nuget segítségével könyvtárak"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
ms.assetid: 0a546510-76e7-465e-9271-f15ff0cfa959
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: ddove
ms.openlocfilehash: 7e28d128645e856c0efa6e4f78d8f9f36982a76c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="upgrade-an-app-to-use-the-latest-elastic-database-client-library"></a><span data-ttu-id="e1ce9-103">A legújabb elastic database ügyféloldali kódtár használata az alkalmazások frissítése</span><span class="sxs-lookup"><span data-stu-id="e1ce9-103">Upgrade an app to use the latest elastic database client library</span></span>
<span data-ttu-id="e1ce9-104">Az új verzióit a [Elastic Database ügyféloldali kódtárának](sql-database-elastic-database-client-library.md) NuGetand a Visual Studio a NuGetPackage Manager felületén keresztül elérhető.</span><span class="sxs-lookup"><span data-stu-id="e1ce9-104">New versions of the [Elastic Database client library](sql-database-elastic-database-client-library.md) are  available through NuGetand the NuGetPackage Manager interface in Visual Studio.</span></span> <span data-ttu-id="e1ce9-105">Frissítések hibajavításokat tartalmaz, és új képességeket az ügyféloldali kódtár támogatja.</span><span class="sxs-lookup"><span data-stu-id="e1ce9-105">Upgrades contain bug fixes and support for new capabilities of the client library.</span></span>

<span data-ttu-id="e1ce9-106">**A legújabb verzió:** Ugrás [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).</span><span class="sxs-lookup"><span data-stu-id="e1ce9-106">**For the latest version:** Go to [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).</span></span>

<span data-ttu-id="e1ce9-107">Az alkalmazás az új gyűjteménnyel, valamint módosíthatja a meglévő Shard térkép Manager metaadatok új funkciók támogatásához az Azure SQL-adatbázisban tárolja.</span><span class="sxs-lookup"><span data-stu-id="e1ce9-107">Rebuild your application with the new library, as well as change your existing Shard Map Manager metadata stored in your Azure SQL Databases to support new features.</span></span>

<span data-ttu-id="e1ce9-108">Ezeket a lépéseket hajtja végre sorrendben biztosítja, hogy az ügyféloldali kódtár régi verziói már nem telepítve legyen a környezetben metaadat-objektum frissítésekor, ami azt jelenti, hogy a régi verziójú metaadat-objektum nem jön létre a frissítés után.</span><span class="sxs-lookup"><span data-stu-id="e1ce9-108">Performing these steps in order ensures that old versions of the client library are no longer present in your environment when metadata objects are updated, which means that old-version metadata objects won’t be created after upgrade.</span></span>   

## <a name="upgrade-steps"></a><span data-ttu-id="e1ce9-109">Frissítési lépések</span><span class="sxs-lookup"><span data-stu-id="e1ce9-109">Upgrade steps</span></span>
<span data-ttu-id="e1ce9-110">**1. Az alkalmazások frissítése.**</span><span class="sxs-lookup"><span data-stu-id="e1ce9-110">**1. Upgrade your applications.**</span></span> <span data-ttu-id="e1ce9-111">A Visual Studio töltse le, és az ügyfél legújabb könyvtárverzió hivatkozik a fejlesztési projektek; a szalagtárat használó összes majd újraépítése, és telepítheti.</span><span class="sxs-lookup"><span data-stu-id="e1ce9-111">In Visual Studio, download and reference the latest client library version into all of your development projects that use the library; then rebuild and deploy.</span></span> 

* <span data-ttu-id="e1ce9-112">Válassza ki a Visual Studio megoldás **eszközök** --> **NuGet-Csomagkezelő** -->  **NuGet-csomagok kezelése megoldáshoz**.</span><span class="sxs-lookup"><span data-stu-id="e1ce9-112">In your Visual Studio solution, select **Tools** --> **NuGet Package Manager** -->  **Manage NuGet Packages for Solution**.</span></span> 
* <span data-ttu-id="e1ce9-113">(A visual Studio 2013) A bal oldali panelen válassza ki a **frissítések**, majd válassza ki a **frissítés** gombra a csomag **Azure SQL Database rugalmas méretezési ügyféloldali kódtár** , amely megjelenik az ablakban.</span><span class="sxs-lookup"><span data-stu-id="e1ce9-113">(Visual Studio 2013) In the left panel, select **Updates**, and then select the **Update** button on the package **Azure SQL Database Elastic Scale Client Library** that appears in the window.</span></span>
* <span data-ttu-id="e1ce9-114">(A visual Studio 2015) A Szűrő mezőbe beállítása **elérhető frissítés**.</span><span class="sxs-lookup"><span data-stu-id="e1ce9-114">(Visual Studio 2015) Set the Filter box to **Upgrade available**.</span></span> <span data-ttu-id="e1ce9-115">Válassza ki a csomagot, majd kattintson a **frissítése** gombra.</span><span class="sxs-lookup"><span data-stu-id="e1ce9-115">Select the package to update, and click the **Update** button.</span></span>
* <span data-ttu-id="e1ce9-116">(A visual Studio 2017) A párbeszédpanel tetején válassza **frissítések**.</span><span class="sxs-lookup"><span data-stu-id="e1ce9-116">(Visual Studio 2017) At the top of the dialog, select **Updates**.</span></span> <span data-ttu-id="e1ce9-117">Válassza ki a csomagot, majd kattintson a **frissítése** gombra.</span><span class="sxs-lookup"><span data-stu-id="e1ce9-117">Select the package to update, and click the **Update** button.</span></span>
* <span data-ttu-id="e1ce9-118">Hozza létre és telepítheti.</span><span class="sxs-lookup"><span data-stu-id="e1ce9-118">Build and Deploy.</span></span> 

<span data-ttu-id="e1ce9-119">**2. Frissítse a parancsfájlokat.**</span><span class="sxs-lookup"><span data-stu-id="e1ce9-119">**2. Upgrade your scripts.**</span></span> <span data-ttu-id="e1ce9-120">Használata **PowerShell** parancsfájlokat, amelyek kezelik a szilánkok, [töltse le az új szalagtár](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) , és másolja a könyvtárba, amelyből végre parancsfájlokat.</span><span class="sxs-lookup"><span data-stu-id="e1ce9-120">If you are using **PowerShell** scripts to manage shards, [download the new library version](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) and copy it into the directory from which you execute scripts.</span></span> 

<span data-ttu-id="e1ce9-121">**3. Frissítse a felosztás egyesítéses szolgáltatást.**</span><span class="sxs-lookup"><span data-stu-id="e1ce9-121">**3. Upgrade your split-merge service.**</span></span> <span data-ttu-id="e1ce9-122">Ha eszközzel a rugalmas adatbázis vegyes egyesítéses horizontálisan skálázott adatok átszervezése [töltse le és telepítse a legfrissebb verziót az eszköz](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/).</span><span class="sxs-lookup"><span data-stu-id="e1ce9-122">If you use the elastic database split-merge tool to reorganize sharded data, [download and deploy the latest version of the tool](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/).</span></span> <span data-ttu-id="e1ce9-123">A szolgáltatás található a frissítési lépések részletes [Itt](sql-database-elastic-scale-overview-split-and-merge.md).</span><span class="sxs-lookup"><span data-stu-id="e1ce9-123">Detailed upgrade steps for the Service can be found [here](sql-database-elastic-scale-overview-split-and-merge.md).</span></span> 

<span data-ttu-id="e1ce9-124">**4. Frissítse a Shard térkép Manager adatbázisainak**.</span><span class="sxs-lookup"><span data-stu-id="e1ce9-124">**4. Upgrade your Shard Map Manager databases**.</span></span> <span data-ttu-id="e1ce9-125">A metaadatok a Shard Maps támogatása az Azure SQL-adatbázis frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="e1ce9-125">Upgrade the metadata supporting your Shard Maps in Azure SQL Database.</span></span>  <span data-ttu-id="e1ce9-126">Ez elvégezhető, PowerShell vagy a C# segítségével két módja van.</span><span class="sxs-lookup"><span data-stu-id="e1ce9-126">There are two ways you can accomplish this, using PowerShell or C#.</span></span> <span data-ttu-id="e1ce9-127">Mindkét lehetőség alább láthatók.</span><span class="sxs-lookup"><span data-stu-id="e1ce9-127">Both options are shown below.</span></span>

<span data-ttu-id="e1ce9-128">***1. lehetőség: A frissítési metaadatok PowerShell használatával***</span><span class="sxs-lookup"><span data-stu-id="e1ce9-128">***Option 1: Upgrade metadata using PowerShell***</span></span>

1. <span data-ttu-id="e1ce9-129">A legújabb parancssori segédprogramot letölteni a NuGet [Itt](http://nuget.org/nuget.exe) és egy mappába.</span><span class="sxs-lookup"><span data-stu-id="e1ce9-129">Download the latest command-line utility for NuGet from [here](http://nuget.org/nuget.exe) and save to a folder.</span></span> 
2. <span data-ttu-id="e1ce9-130">Nyisson meg egy parancssort, keresse meg az ugyanabban a mappában, és adja ki a parancsot:`nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Client`</span><span class="sxs-lookup"><span data-stu-id="e1ce9-130">Open a Command Prompt, navigate to the same folder, and issue the command: `nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Client`</span></span>
3. <span data-ttu-id="e1ce9-131">Keresse meg az imént letöltött, például új ügyfél DLL-fájl verzióját tartalmazó almappa:`cd .\Microsoft.Azure.SqlDatabase.ElasticScale.Client.1.0.0\lib\net45`</span><span class="sxs-lookup"><span data-stu-id="e1ce9-131">Navigate to the subfolder containing the new client DLL version you have just downloaded, for example: `cd .\Microsoft.Azure.SqlDatabase.ElasticScale.Client.1.0.0\lib\net45`</span></span>
4. <span data-ttu-id="e1ce9-132">A rugalmas adatbázis ügyfél frissítési szkriptlet a töltse le a [Script Center](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-Elastic-6442e6a9), és mentse a dll-fájlt tartalmazó ugyanabba a mappába.</span><span class="sxs-lookup"><span data-stu-id="e1ce9-132">Download the elastic database client upgrade scriptlet from the [Script Center](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-Elastic-6442e6a9), and save it into the same folder containing the DLL.</span></span>
5. <span data-ttu-id="e1ce9-133">Ebből a könyvtárból "PowerShell.\upgrade.ps1" futtassa a parancssorból, és kövesse az utasításokat.</span><span class="sxs-lookup"><span data-stu-id="e1ce9-133">From that folder, run “PowerShell .\upgrade.ps1” from the command prompt and follow the prompts.</span></span>

<span data-ttu-id="e1ce9-134">***2. lehetőség: A frissítési metaadatok használatával C#***</span><span class="sxs-lookup"><span data-stu-id="e1ce9-134">***Option 2: Upgrade metadata using C#***</span></span>

<span data-ttu-id="e1ce9-135">Alternatív megoldásként hozzon létre egy Visual Studio alkalmazás, amely megnyitja a ShardMapManager elemein végiglépkedve összes szilánkok és a metaadatok frissítést végez a metódusok meghívása [UpgradeLocalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradelocalstore.aspx) és [UpgradeGlobalStore ](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradeglobalstore.aspx) ebben a példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="e1ce9-135">Alternatively, create a Visual Studio application that opens your ShardMapManager, iterates over all shards, and performs the metadata upgrade by calling the methods [UpgradeLocalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradelocalstore.aspx) and [UpgradeGlobalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradeglobalstore.aspx) as in this example:</span></span> 

    ShardMapManager smm =
       ShardMapManagerFactory.GetSqlShardMapManager
       (connStr, ShardMapManagerLoadPolicy.Lazy); 
    smm.UpgradeGlobalStore(); 

    foreach (ShardLocation loc in
     smm.GetDistinctShardLocations()) 
    {   
       smm.UpgradeLocalStore(loc); 
    } 

<span data-ttu-id="e1ce9-136">Ezek a technológiák a metaadatok frissítésekre alkalmazható többször kárt nélkül.</span><span class="sxs-lookup"><span data-stu-id="e1ce9-136">These techniques for metadata upgrades can be applied multiple times without harm.</span></span> <span data-ttu-id="e1ce9-137">Például ha egy régebbi ügyfélverzió véletlenül egy shard hoz létre, miután már frissítette, frissítést futtathatja újra közötti összes szilánkok gondoskodjon arról, hogy a legújabb metaadat az infrastruktúra egészében.</span><span class="sxs-lookup"><span data-stu-id="e1ce9-137">For example, if an older client version inadvertently creates a shard after you have already updated, you can run upgrade again across all shards to ensure that the latest metadata version is present throughout your infrastructure.</span></span> 

<span data-ttu-id="e1ce9-138">**Megjegyzés:** új az ügyféloldali kódtár közzétett-date folytatja a munkát a Shard térkép Manager metaadatok előzetes verziói a Azure SQL Database, és fordítva.</span><span class="sxs-lookup"><span data-stu-id="e1ce9-138">**Note:**  New versions of the client library published to-date continue to work with prior versions of the Shard Map Manager metadata on Azure SQL DB, and vice-versa.</span></span>   <span data-ttu-id="e1ce9-139">Azonban az előnyeit néhány új szolgáltatását a legújabb ügyfélprogram, metaadatok frissíteni kell.</span><span class="sxs-lookup"><span data-stu-id="e1ce9-139">However to take advantage of some of the new features in the latest client, metadata needs to be upgraded.</span></span>   <span data-ttu-id="e1ce9-140">Vegye figyelembe, hogy metaadat-frissítések nem érinti a felhasználó-adatokat és vonatkozó adatokat, csak objektumok a Shard térkép Manager által létrehozott és használt.</span><span class="sxs-lookup"><span data-stu-id="e1ce9-140">Note that metadata upgrades will not affect any user-data or application-specific data, only objects created and used by the Shard Map Manager.</span></span>  <span data-ttu-id="e1ce9-141">És alkalmazások folytatják működésüket egészen a frissítési sorrend a fent leírt keresztül.</span><span class="sxs-lookup"><span data-stu-id="e1ce9-141">And applications continue to operate through the upgrade sequence described above.</span></span> 

## <a name="elastic-database-client-version-history"></a><span data-ttu-id="e1ce9-142">A rugalmas adatbázis ügyfél verziójának előzményei</span><span class="sxs-lookup"><span data-stu-id="e1ce9-142">Elastic database client version history</span></span>
<span data-ttu-id="e1ce9-143">Verzióelőzmények, látogasson el [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)</span><span class="sxs-lookup"><span data-stu-id="e1ce9-143">For version history, go to [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)</span></span>

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]:./media/sql-database-elastic-scale-upgrade-client-library/nuget-upgrade.png

