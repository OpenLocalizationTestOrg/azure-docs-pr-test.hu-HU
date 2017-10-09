---
title: "aaaUpgrade toohello legújabb elastic database ügyféloldali kódtárának |} Microsoft Docs"
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
ms.openlocfilehash: cc2c9179be4c53ca59cd24d832127cf277c6e695
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-an-app-toouse-hello-latest-elastic-database-client-library"></a><span data-ttu-id="5abd6-103">Frissítse az alkalmazás toouse hello legújabb elastic database ügyféloldali kódtár</span><span class="sxs-lookup"><span data-stu-id="5abd6-103">Upgrade an app toouse hello latest elastic database client library</span></span>
<span data-ttu-id="5abd6-104">Új verzióit hello [Elastic Database ügyféloldali kódtárának](sql-database-elastic-database-client-library.md) NuGetand hello NuGetPackage Manager felületén a Visual Studio elérhető.</span><span class="sxs-lookup"><span data-stu-id="5abd6-104">New versions of hello [Elastic Database client library](sql-database-elastic-database-client-library.md) are  available through NuGetand hello NuGetPackage Manager interface in Visual Studio.</span></span> <span data-ttu-id="5abd6-105">Frissítések hibajavításokat tartalmaz, és új képességeket hello ügyféloldali kódtár támogatja.</span><span class="sxs-lookup"><span data-stu-id="5abd6-105">Upgrades contain bug fixes and support for new capabilities of hello client library.</span></span>

<span data-ttu-id="5abd6-106">**Hello legújabb verziójához:** túl Ugrás[Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).</span><span class="sxs-lookup"><span data-stu-id="5abd6-106">**For hello latest version:** Go too[Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).</span></span>

<span data-ttu-id="5abd6-107">Az alkalmazás hello új könyvtárral, valamint módosíthatja a meglévő Shard térkép Manager metaadatok tárolódnak az Azure SQL-adatbázisok toosupport új funkciókat.</span><span class="sxs-lookup"><span data-stu-id="5abd6-107">Rebuild your application with hello new library, as well as change your existing Shard Map Manager metadata stored in your Azure SQL Databases toosupport new features.</span></span>

<span data-ttu-id="5abd6-108">Ezeket a lépéseket hajtja végre sorrendben biztosítja, hogy hello ügyféloldali kódtár régi verziói már nem telepítve legyen a környezetben metaadat-objektum frissítésekor, ami azt jelenti, hogy a régi verziójú metaadat-objektum nem jön létre a frissítés után.</span><span class="sxs-lookup"><span data-stu-id="5abd6-108">Performing these steps in order ensures that old versions of hello client library are no longer present in your environment when metadata objects are updated, which means that old-version metadata objects won’t be created after upgrade.</span></span>   

## <a name="upgrade-steps"></a><span data-ttu-id="5abd6-109">Frissítési lépések</span><span class="sxs-lookup"><span data-stu-id="5abd6-109">Upgrade steps</span></span>
<span data-ttu-id="5abd6-110">**1. Az alkalmazások frissítése.**</span><span class="sxs-lookup"><span data-stu-id="5abd6-110">**1. Upgrade your applications.**</span></span> <span data-ttu-id="5abd6-111">A Visual Studio, a letöltés és a hivatkozás hello ügyfél könyvtár legújabb be a fejlesztési projektek hello Library; szalagtárat használó összes majd újraépítése, és telepítheti.</span><span class="sxs-lookup"><span data-stu-id="5abd6-111">In Visual Studio, download and reference hello latest client library version into all of your development projects that use hello library; then rebuild and deploy.</span></span> 

* <span data-ttu-id="5abd6-112">Válassza ki a Visual Studio megoldás **eszközök** --> **NuGet-Csomagkezelő** -->  **NuGet-csomagok kezelése megoldáshoz**.</span><span class="sxs-lookup"><span data-stu-id="5abd6-112">In your Visual Studio solution, select **Tools** --> **NuGet Package Manager** -->  **Manage NuGet Packages for Solution**.</span></span> 
* <span data-ttu-id="5abd6-113">(A visual Studio 2013) Hello bal oldali panelen, jelölje ki a **frissítések**, majd válassza ki a hello **frissítés** hello csomag gombjára **Azure SQL Database rugalmas méretezési ügyféloldali kódtár** hello megjelenő ablak.</span><span class="sxs-lookup"><span data-stu-id="5abd6-113">(Visual Studio 2013) In hello left panel, select **Updates**, and then select hello **Update** button on hello package **Azure SQL Database Elastic Scale Client Library** that appears in hello window.</span></span>
* <span data-ttu-id="5abd6-114">(A visual Studio 2015) Állítsa be a szűrőmezőbe hello túl**elérhető frissítés**.</span><span class="sxs-lookup"><span data-stu-id="5abd6-114">(Visual Studio 2015) Set hello Filter box too**Upgrade available**.</span></span> <span data-ttu-id="5abd6-115">Jelölje ki a hello csomag tooupdate, majd kattintson a hello **frissítés** gombra.</span><span class="sxs-lookup"><span data-stu-id="5abd6-115">Select hello package tooupdate, and click hello **Update** button.</span></span>
* <span data-ttu-id="5abd6-116">(A visual Studio 2017) Hello felső hello párbeszédpanel, válassza ki a **frissítések**.</span><span class="sxs-lookup"><span data-stu-id="5abd6-116">(Visual Studio 2017) At hello top of hello dialog, select **Updates**.</span></span> <span data-ttu-id="5abd6-117">Jelölje ki a hello csomag tooupdate, majd kattintson a hello **frissítés** gombra.</span><span class="sxs-lookup"><span data-stu-id="5abd6-117">Select hello package tooupdate, and click hello **Update** button.</span></span>
* <span data-ttu-id="5abd6-118">Hozza létre és telepítheti.</span><span class="sxs-lookup"><span data-stu-id="5abd6-118">Build and Deploy.</span></span> 

<span data-ttu-id="5abd6-119">**2. Frissítse a parancsfájlokat.**</span><span class="sxs-lookup"><span data-stu-id="5abd6-119">**2. Upgrade your scripts.**</span></span> <span data-ttu-id="5abd6-120">Ha használ **PowerShell** toomanage szilánkok, a parancsfájlok [hello új szalagtár verzió letöltéséhez](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) , és másolja azt hello könyvtárba, amelyből végre parancsfájlokat.</span><span class="sxs-lookup"><span data-stu-id="5abd6-120">If you are using **PowerShell** scripts toomanage shards, [download hello new library version](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) and copy it into hello directory from which you execute scripts.</span></span> 

<span data-ttu-id="5abd6-121">**3. Frissítse a felosztás egyesítéses szolgáltatást.**</span><span class="sxs-lookup"><span data-stu-id="5abd6-121">**3. Upgrade your split-merge service.**</span></span> <span data-ttu-id="5abd6-122">Ha hello rugalmas adatbázis vegyes egyesítéses eszköz tooreorganize horizontálisan skálázott adatok, [töltse le és telepítse a legújabb verziójának hello hello eszköz](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/).</span><span class="sxs-lookup"><span data-stu-id="5abd6-122">If you use hello elastic database split-merge tool tooreorganize sharded data, [download and deploy hello latest version of hello tool](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/).</span></span> <span data-ttu-id="5abd6-123">Hello szolgáltatás található a frissítési lépések részletes [Itt](sql-database-elastic-scale-overview-split-and-merge.md).</span><span class="sxs-lookup"><span data-stu-id="5abd6-123">Detailed upgrade steps for hello Service can be found [here](sql-database-elastic-scale-overview-split-and-merge.md).</span></span> 

<span data-ttu-id="5abd6-124">**4. Frissítse a Shard térkép Manager adatbázisainak**.</span><span class="sxs-lookup"><span data-stu-id="5abd6-124">**4. Upgrade your Shard Map Manager databases**.</span></span> <span data-ttu-id="5abd6-125">A szilánkok Maps támogatása az Azure SQL Database hello metaadatok frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="5abd6-125">Upgrade hello metadata supporting your Shard Maps in Azure SQL Database.</span></span>  <span data-ttu-id="5abd6-126">Ez elvégezhető, PowerShell vagy a C# segítségével két módja van.</span><span class="sxs-lookup"><span data-stu-id="5abd6-126">There are two ways you can accomplish this, using PowerShell or C#.</span></span> <span data-ttu-id="5abd6-127">Mindkét lehetőség alább láthatók.</span><span class="sxs-lookup"><span data-stu-id="5abd6-127">Both options are shown below.</span></span>

<span data-ttu-id="5abd6-128">***1. lehetőség: A frissítési metaadatok PowerShell használatával***</span><span class="sxs-lookup"><span data-stu-id="5abd6-128">***Option 1: Upgrade metadata using PowerShell***</span></span>

1. <span data-ttu-id="5abd6-129">Hello legújabb parancssori segédprogramot letölteni a NuGet [Itt](http://nuget.org/nuget.exe) és tooa mappa mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="5abd6-129">Download hello latest command-line utility for NuGet from [here](http://nuget.org/nuget.exe) and save tooa folder.</span></span> 
2. <span data-ttu-id="5abd6-130">Nyisson meg egy parancssort, lépjen a toohello ugyanabban a mappában, és probléma hello parancsot:`nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Client`</span><span class="sxs-lookup"><span data-stu-id="5abd6-130">Open a Command Prompt, navigate toohello same folder, and issue hello command: `nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Client`</span></span>
3. <span data-ttu-id="5abd6-131">Keresse meg a toohello almappa tartalmazó hello új DLL ügyfélverzió imént letöltött, például:`cd .\Microsoft.Azure.SqlDatabase.ElasticScale.Client.1.0.0\lib\net45`</span><span class="sxs-lookup"><span data-stu-id="5abd6-131">Navigate toohello subfolder containing hello new client DLL version you have just downloaded, for example: `cd .\Microsoft.Azure.SqlDatabase.ElasticScale.Client.1.0.0\lib\net45`</span></span>
4. <span data-ttu-id="5abd6-132">Hello rugalmas adatbázis ügyfél frissítési szkriptlet letöltését hello [Script Center](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-Elastic-6442e6a9), és menteni a hello azonos tartalmazó mappa hello dll-Fájljában.</span><span class="sxs-lookup"><span data-stu-id="5abd6-132">Download hello elastic database client upgrade scriptlet from hello [Script Center](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-Elastic-6442e6a9), and save it into hello same folder containing hello DLL.</span></span>
5. <span data-ttu-id="5abd6-133">Ebből a könyvtárból futtassa a "PowerShell.\upgrade.ps1" hello parancssorból, és hello útmutatást követve.</span><span class="sxs-lookup"><span data-stu-id="5abd6-133">From that folder, run “PowerShell .\upgrade.ps1” from hello command prompt and follow hello prompts.</span></span>

<span data-ttu-id="5abd6-134">***2. lehetőség: A frissítési metaadatok használatával C#***</span><span class="sxs-lookup"><span data-stu-id="5abd6-134">***Option 2: Upgrade metadata using C#***</span></span>

<span data-ttu-id="5abd6-135">Alternatív megoldásként hozzon létre egy Visual Studio alkalmazás, amely megnyitja a ShardMapManager elemein végiglépkedve összes szilánkok és hello metaadatok frissítést végez hello metódusok meghívása [UpgradeLocalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradelocalstore.aspx) és [ UpgradeGlobalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradeglobalstore.aspx) ebben a példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="5abd6-135">Alternatively, create a Visual Studio application that opens your ShardMapManager, iterates over all shards, and performs hello metadata upgrade by calling hello methods [UpgradeLocalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradelocalstore.aspx) and [UpgradeGlobalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradeglobalstore.aspx) as in this example:</span></span> 

    ShardMapManager smm =
       ShardMapManagerFactory.GetSqlShardMapManager
       (connStr, ShardMapManagerLoadPolicy.Lazy); 
    smm.UpgradeGlobalStore(); 

    foreach (ShardLocation loc in
     smm.GetDistinctShardLocations()) 
    {   
       smm.UpgradeLocalStore(loc); 
    } 

<span data-ttu-id="5abd6-136">Ezek a technológiák a metaadatok frissítésekre alkalmazható többször kárt nélkül.</span><span class="sxs-lookup"><span data-stu-id="5abd6-136">These techniques for metadata upgrades can be applied multiple times without harm.</span></span> <span data-ttu-id="5abd6-137">Például ha egy régebbi ügyfélverzió véletlenül egy shard hoz létre, miután már frissítette, frissítést futtathatja újra közötti összes szilánkok tooensure adott hello legfrissebb metaadatok megtalálható az infrastruktúra egészében.</span><span class="sxs-lookup"><span data-stu-id="5abd6-137">For example, if an older client version inadvertently creates a shard after you have already updated, you can run upgrade again across all shards tooensure that hello latest metadata version is present throughout your infrastructure.</span></span> 

<span data-ttu-id="5abd6-138">**Megjegyzés:** új hello ügyféloldali kódtár közzétett-date továbbra is toowork előzetes verzióihoz hello Shard térkép kezelő metaadatainak az Azure SQL Database, és fordítva.</span><span class="sxs-lookup"><span data-stu-id="5abd6-138">**Note:**  New versions of hello client library published to-date continue toowork with prior versions of hello Shard Map Manager metadata on Azure SQL DB, and vice-versa.</span></span>   <span data-ttu-id="5abd6-139">Tootake előnyeit, néhány új szolgáltatását hello hello legújabb ügyfélprogram, metaadatok azonban toobe frissíteni kell.</span><span class="sxs-lookup"><span data-stu-id="5abd6-139">However tootake advantage of some of hello new features in hello latest client, metadata needs toobe upgraded.</span></span>   <span data-ttu-id="5abd6-140">Vegye figyelembe, hogy metaadat-frissítések nem érinti a felhasználó-adatokat és vonatkozó adatokat, csak objektumok hello Shard térkép Manager által létrehozott és használt.</span><span class="sxs-lookup"><span data-stu-id="5abd6-140">Note that metadata upgrades will not affect any user-data or application-specific data, only objects created and used by hello Shard Map Manager.</span></span>  <span data-ttu-id="5abd6-141">És alkalmazások továbbra is toooperate hello frissítési sorrend a fent leírt keresztül.</span><span class="sxs-lookup"><span data-stu-id="5abd6-141">And applications continue toooperate through hello upgrade sequence described above.</span></span> 

## <a name="elastic-database-client-version-history"></a><span data-ttu-id="5abd6-142">A rugalmas adatbázis ügyfél verziójának előzményei</span><span class="sxs-lookup"><span data-stu-id="5abd6-142">Elastic database client version history</span></span>
<span data-ttu-id="5abd6-143">A korábbi verziók go túl[Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)</span><span class="sxs-lookup"><span data-stu-id="5abd6-143">For version history, go too[Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)</span></span>

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]:./media/sql-database-elastic-scale-upgrade-client-library/nuget-upgrade.png

