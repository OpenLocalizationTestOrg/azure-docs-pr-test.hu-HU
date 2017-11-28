---
title: "PowerShell-lel rugalmas feladatok létrehozásához és kezeléséhez |} Microsoft Docs"
description: "Az Azure SQL Database-készletek kezelésére szolgáló PowerShell"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
ms.assetid: 737d8d13-5632-4e18-9cb0-4d3b8a19e495
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: b4c97e8f51581f9a3f7c5a8d8e82562255fe7b48
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-and-manage-sql-database-elastic-jobs-using-powershell-preview"></a><span data-ttu-id="c7ba6-103">SQL Database PowerShell (előzetes verzió) segítségével a rugalmas feladatok létrehozásához és kezeléséhez</span><span class="sxs-lookup"><span data-stu-id="c7ba6-103">Create and manage SQL Database elastic jobs using PowerShell (preview)</span></span>

<span data-ttu-id="c7ba6-104">A PowerShell API-khoz, **rugalmas adatbázis-feladatok** (az előzetes verzió), lehetővé teszik, hogy olyan adatbázisok, amely végrehajtja a parancsfájlok csoportját.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-104">The PowerShell APIs for **Elastic Database jobs** (in preview), let you define a group of databases against which scripts will execute.</span></span> <span data-ttu-id="c7ba6-105">Ez a cikk bemutatja, hogyan hozhatja létre és kezelheti **rugalmas adatbázis-feladatok** PowerShell-parancsmagok használatával.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-105">This article shows how to create and manage **Elastic Database jobs** using PowerShell cmdlets.</span></span> <span data-ttu-id="c7ba6-106">Lásd: [rugalmas feladatok áttekintése](sql-database-elastic-jobs-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c7ba6-106">See [Elastic jobs overview](sql-database-elastic-jobs-overview.md).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="c7ba6-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c7ba6-107">Prerequisites</span></span>
* <span data-ttu-id="c7ba6-108">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-108">An Azure subscription.</span></span> <span data-ttu-id="c7ba6-109">Ingyenes próbaverzió, lásd: [ingyenes egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c7ba6-109">For a free trial, see [Free one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="c7ba6-110">A rugalmas adatbázis eszközzel létrehozott adatbázisok készleteit.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-110">A set of databases created with the Elastic Database tools.</span></span> <span data-ttu-id="c7ba6-111">Lásd: [Ismerkedés a rugalmas adatbáziseszközöket](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="c7ba6-111">See [Get started with Elastic Database tools](sql-database-elastic-scale-get-started.md).</span></span>
* <span data-ttu-id="c7ba6-112">Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-112">Azure PowerShell.</span></span> <span data-ttu-id="c7ba6-113">Részletes információk: [Az Azure PowerShell telepítése és konfigurálása](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c7ba6-113">For detailed information, see [How to install and configure Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span></span>
* <span data-ttu-id="c7ba6-114">**Rugalmas adatbázis-feladatok** PowerShell csomag: lásd: [telepítése rugalmas adatbázis-feladatok](sql-database-elastic-jobs-service-installation.md)</span><span class="sxs-lookup"><span data-stu-id="c7ba6-114">**Elastic Database jobs** PowerShell package: See [Installing Elastic Database jobs](sql-database-elastic-jobs-service-installation.md)</span></span>

### <a name="select-your-azure-subscription"></a><span data-ttu-id="c7ba6-115">Válassza ki az Azure-előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="c7ba6-115">Select your Azure subscription</span></span>
<span data-ttu-id="c7ba6-116">Válassza ki az előfizetést az előfizetés-azonosítót kell (**- SubscriptionId**) vagy az előfizetés nevét (**- SubscriptionName**).</span><span class="sxs-lookup"><span data-stu-id="c7ba6-116">To select the subscription you need your subscription Id (**-SubscriptionId**) or subscription name (**-SubscriptionName**).</span></span> <span data-ttu-id="c7ba6-117">Ha több előfizetéssel rendelkezik futtathatja a **Get-AzureRmSubscription** parancsmagot, és másolja a kívánt előfizetés-adatokat, az eredmény az beállítása.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-117">If you have multiple subscriptions you can run the **Get-AzureRmSubscription** cmdlet and copy the desired subscription information from the result set.</span></span> <span data-ttu-id="c7ba6-118">Miután az előfizetési adatai, futtassa az alapértelmezett, nevezetesen a cél a feladatok létrehozását és kezelését az előfizetés beállításához a következő parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="c7ba6-118">Once you have your subscription information, run the following commandlet to set this subscription as the default, namely the target for creating and managing jobs:</span></span>

    Select-AzureRmSubscription -SubscriptionId {SubscriptionID}

<span data-ttu-id="c7ba6-119">A [PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx) ajánlott használatra történő fejlesztéséhez és a PowerShell-szkriptek használatát a rugalmas adatbázis-feladatok végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-119">The [PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx) is recommended for usage to develop and execute PowerShell scripts against the Elastic Database jobs.</span></span>

## <a name="elastic-database-jobs-objects"></a><span data-ttu-id="c7ba6-120">A rugalmas adatbázis-feladatok objektumok</span><span class="sxs-lookup"><span data-stu-id="c7ba6-120">Elastic Database jobs objects</span></span>
<span data-ttu-id="c7ba6-121">A következő táblázat felsorolja az összes objektum típusú kimenő **rugalmas adatbázis-feladatok** együtt a leírása és a megfelelő PowerShell API-k.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-121">The following table lists out all the object types of **Elastic Database jobs** along with its description and relevant PowerShell APIs.</span></span>

<table style="width:100%">
  <tr>
    <th><span data-ttu-id="c7ba6-122">Objektumtípus</span><span class="sxs-lookup"><span data-stu-id="c7ba6-122">Object Type</span></span></th>
    <th><span data-ttu-id="c7ba6-123">Leírás</span><span class="sxs-lookup"><span data-stu-id="c7ba6-123">Description</span></span></th>
    <th><span data-ttu-id="c7ba6-124">Kapcsolódó PowerShell API-k</span><span class="sxs-lookup"><span data-stu-id="c7ba6-124">Related PowerShell APIs</span></span></th>
  </tr>
  <tr>
    <td><span data-ttu-id="c7ba6-125">Hitelesítő adat</span><span class="sxs-lookup"><span data-stu-id="c7ba6-125">Credential</span></span></td>
    <td><span data-ttu-id="c7ba6-126">Felhasználónév és jelszó parancsprogramok végrehajtását vagy DACPACs alkalmazásának adatbázisok való kapcsolódáskor használ.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-126">Username and password to use when connecting to databases for execution of scripts or application of DACPACs.</span></span> <p><span data-ttu-id="c7ba6-127">A jelszó küldése és a rugalmas adatbázis-feladatok adatbázis tárolási előtt titkosítva.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-127">The password is encrypted before sending to and storing in the Elastic Database Jobs database.</span></span>  <span data-ttu-id="c7ba6-128">A rugalmas adatbázis-feladatok szolgáltatás a hitelesítő adat létrehozása és fel kell tölteni a telepítési parancsfájl segítségével visszafejti a jelszót.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-128">The password is decrypted by the Elastic Database Jobs service via the credential created and uploaded from the installation script.</span></span></td>
    <td><p><span data-ttu-id="c7ba6-129">Get-AzureSqlJobCredential</span><span class="sxs-lookup"><span data-stu-id="c7ba6-129">Get-AzureSqlJobCredential</span></span></p>
    <p><span data-ttu-id="c7ba6-130">Új AzureSqlJobCredential</span><span class="sxs-lookup"><span data-stu-id="c7ba6-130">New-AzureSqlJobCredential</span></span></p><p><span data-ttu-id="c7ba6-131">Set-AzureSqlJobCredential</span><span class="sxs-lookup"><span data-stu-id="c7ba6-131">Set-AzureSqlJobCredential</span></span></p></td></td>
  </tr>

  <tr>
    <td><span data-ttu-id="c7ba6-132">Szkript</span><span class="sxs-lookup"><span data-stu-id="c7ba6-132">Script</span></span></td>
    <td><span data-ttu-id="c7ba6-133">Transact-SQL parancsfájl végrehajtása az adatbázisok közötti használt.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-133">Transact-SQL script to be used for execution across databases.</span></span>  <span data-ttu-id="c7ba6-134">A parancsfájl a kell kell lennie az idempotent, mivel a szolgáltatás megpróbálja hibák után a parancsfájl végrehajtása lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-134">The script should be authored to be idempotent since the service will retry execution of the script upon failures.</span></span>
    </td>
    <td>
    <p><span data-ttu-id="c7ba6-135">Get-AzureSqlJobContent</span><span class="sxs-lookup"><span data-stu-id="c7ba6-135">Get-AzureSqlJobContent</span></span></p>
    <p><span data-ttu-id="c7ba6-136">Get-AzureSqlJobContentDefinition</span><span class="sxs-lookup"><span data-stu-id="c7ba6-136">Get-AzureSqlJobContentDefinition</span></span></p>
    <p><span data-ttu-id="c7ba6-137">Új AzureSqlJobContent</span><span class="sxs-lookup"><span data-stu-id="c7ba6-137">New-AzureSqlJobContent</span></span></p>
    <p><span data-ttu-id="c7ba6-138">Set-AzureSqlJobContentDefinition</span><span class="sxs-lookup"><span data-stu-id="c7ba6-138">Set-AzureSqlJobContentDefinition</span></span></p>
    </td>
  </tr>

  <tr>
    <td><span data-ttu-id="c7ba6-139">DACPAC</span><span class="sxs-lookup"><span data-stu-id="c7ba6-139">DACPAC</span></span></td>
    <td><span data-ttu-id="c7ba6-140"><a href="https://msdn.microsoft.com/library/ee210546.aspx">Adatrétegbeli alkalmazás </a> az adatbázisok közötti alkalmazni kívánt csomagot.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-140"><a href="https://msdn.microsoft.com/library/ee210546.aspx">Data-tier application </a> package to be applied across databases.</span></span>

    </td>
    <td>
    <p><span data-ttu-id="c7ba6-141">Get-AzureSqlJobContent</span><span class="sxs-lookup"><span data-stu-id="c7ba6-141">Get-AzureSqlJobContent</span></span></p>
    <p><span data-ttu-id="c7ba6-142">Új AzureSqlJobContent</span><span class="sxs-lookup"><span data-stu-id="c7ba6-142">New-AzureSqlJobContent</span></span></p>
    <p><span data-ttu-id="c7ba6-143">Set-AzureSqlJobContentDefinition</span><span class="sxs-lookup"><span data-stu-id="c7ba6-143">Set-AzureSqlJobContentDefinition</span></span></p>
    </td>
  </tr>
  <tr>
    <td><span data-ttu-id="c7ba6-144">Adatbázis-cél</span><span class="sxs-lookup"><span data-stu-id="c7ba6-144">Database Target</span></span></td>
    <td><span data-ttu-id="c7ba6-145">Egy Azure SQL-adatbázisra mutató adatbázis és a kiszolgáló nevét.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-145">Database and server name pointing to an Azure SQL Database.</span></span>

    </td>
    <td>
    <p><span data-ttu-id="c7ba6-146">Get-AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="c7ba6-146">Get-AzureSqlJobTarget</span></span></p>
    <p><span data-ttu-id="c7ba6-147">Új AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="c7ba6-147">New-AzureSqlJobTarget</span></span></p>
    </td>
  </tr>
  <tr>
    <td><span data-ttu-id="c7ba6-148">A shard térkép cél</span><span class="sxs-lookup"><span data-stu-id="c7ba6-148">Shard Map Target</span></span></td>
    <td><span data-ttu-id="c7ba6-149">Egy adatbázis célként és egy rugalmas adatbázist shard leképezés tárolt információk meghatározásához használt hitelesítő adatot kombinációja.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-149">Combination of a database target and a credential to be used to determine information stored within an Elastic Database shard map.</span></span>
    </td>
    <td>
    <p><span data-ttu-id="c7ba6-150">Get-AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="c7ba6-150">Get-AzureSqlJobTarget</span></span></p>
    <p><span data-ttu-id="c7ba6-151">Új AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="c7ba6-151">New-AzureSqlJobTarget</span></span></p>
    <p><span data-ttu-id="c7ba6-152">Set-AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="c7ba6-152">Set-AzureSqlJobTarget</span></span></p>
    </td>
  </tr>
<tr>
    <td><span data-ttu-id="c7ba6-153">Egyéni gyűjtemény célja</span><span class="sxs-lookup"><span data-stu-id="c7ba6-153">Custom Collection Target</span></span></td>
    <td><span data-ttu-id="c7ba6-154">A végrehajtás együttesen használandó adatbázisok meghatározott csoportja.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-154">Defined group of databases to collectively use for execution.</span></span></td>
    <td>
    <p><span data-ttu-id="c7ba6-155">Get-AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="c7ba6-155">Get-AzureSqlJobTarget</span></span></p>
    <p><span data-ttu-id="c7ba6-156">Új AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="c7ba6-156">New-AzureSqlJobTarget</span></span></p>
    </td>
  </tr>
<tr>
    <td><span data-ttu-id="c7ba6-157">Egyéni gyűjtemény gyermek cél</span><span class="sxs-lookup"><span data-stu-id="c7ba6-157">Custom Collection Child Target</span></span></td>
    <td><span data-ttu-id="c7ba6-158">Egy egyéni gyűjteményből hivatkozott adatbázis cél.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-158">Database target that is referenced from a custom collection.</span></span></td>
    <td>
    <p><span data-ttu-id="c7ba6-159">Adja hozzá AzureSqlJobChildTarget</span><span class="sxs-lookup"><span data-stu-id="c7ba6-159">Add-AzureSqlJobChildTarget</span></span></p>
    <p><span data-ttu-id="c7ba6-160">Remove-AzureSqlJobChildTarget</span><span class="sxs-lookup"><span data-stu-id="c7ba6-160">Remove-AzureSqlJobChildTarget</span></span></p>
    </td>
  </tr>

<tr>
    <td><span data-ttu-id="c7ba6-161">Feladat</span><span class="sxs-lookup"><span data-stu-id="c7ba6-161">Job</span></span></td>
    <td>
    <p><span data-ttu-id="c7ba6-162">Egy feladat végrehajtásának elindítása vagy ütemezés szerint teljesítéséhez használható paramétereinek meghatározása.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-162">Definition of parameters for a job that can be used to trigger execution or to fulfill a schedule.</span></span></p>
    </td>
    <td>
    <p><span data-ttu-id="c7ba6-163">Get-AzureSqlJob</span><span class="sxs-lookup"><span data-stu-id="c7ba6-163">Get-AzureSqlJob</span></span></p>
    <p><span data-ttu-id="c7ba6-164">Új AzureSqlJob</span><span class="sxs-lookup"><span data-stu-id="c7ba6-164">New-AzureSqlJob</span></span></p>
    <p><span data-ttu-id="c7ba6-165">Set-AzureSqlJob</span><span class="sxs-lookup"><span data-stu-id="c7ba6-165">Set-AzureSqlJob</span></span></p>
    </td>
  </tr>

<tr>
    <td><span data-ttu-id="c7ba6-166">Feladat végrehajtása</span><span class="sxs-lookup"><span data-stu-id="c7ba6-166">Job Execution</span></span></td>
    <td>
    <p><span data-ttu-id="c7ba6-167">Egy parancsfájl vagy egy DACPAC alkalmazott hitelesítő adatok használatával az adatbázis-kapcsolat hibákkal célja az teljesítéséhez szükséges feladatokat tartalmazó tároló egy végrehajtási házirend megfelelően kezeli.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-167">Container of tasks necessary to fulfill either executing a script or applying a DACPAC to a target using credentials for database connections with failures handled in accordance to an execution policy.</span></span></p>
    </td>
    <td>
    <p><span data-ttu-id="c7ba6-168">Get-AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="c7ba6-168">Get-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="c7ba6-169">Start-AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="c7ba6-169">Start-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="c7ba6-170">STOP-AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="c7ba6-170">Stop-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="c7ba6-171">Várjon, amíg-AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="c7ba6-171">Wait-AzureSqlJobExecution</span></span></p>
  </tr>

<tr>
    <td><span data-ttu-id="c7ba6-172">A feladat a végrehajtás feladat</span><span class="sxs-lookup"><span data-stu-id="c7ba6-172">Job Task Execution</span></span></td>
    <td>
    <p><span data-ttu-id="c7ba6-173">Egyetlen munkaegység a feladatok teljesítése érdekében kapcsolódott.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-173">Single unit of work to fulfill a job.</span></span></p>
    <p><span data-ttu-id="c7ba6-174">Ha egy feladat sikeresen nem képes hajtható végre, az eredményül kapott Kivételüzenet naplózza a rendszer, és egy új egyező feladata létrejön és a megadott végrehajtási házirend megfelelően hajtotta végre.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-174">If a job task is not able to successfully execute, the resulting exception message will be logged and a new matching job task will be created and executed in accordance to the specified execution policy.</span></span></p></p>
    </td>
    <td>
    <p><span data-ttu-id="c7ba6-175">Get-AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="c7ba6-175">Get-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="c7ba6-176">Start-AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="c7ba6-176">Start-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="c7ba6-177">STOP-AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="c7ba6-177">Stop-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="c7ba6-178">Várjon, amíg-AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="c7ba6-178">Wait-AzureSqlJobExecution</span></span></p>
  </tr>

<tr>
    <td><span data-ttu-id="c7ba6-179">Feladat-végrehajtási házirend</span><span class="sxs-lookup"><span data-stu-id="c7ba6-179">Job Execution Policy</span></span></td>
    <td>
    <p><span data-ttu-id="c7ba6-180">Vezérlők sikertelen feladat-végrehajtási időtúllépés, a újrapróbálkozási korlátozások és a próbálkozások közötti időintervallumot.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-180">Controls job execution timeouts, retry limits and intervals between retries.</span></span></p>
    <p><span data-ttu-id="c7ba6-181">Rugalmas adatbázis-feladatok tartalmaz egy alapértelmezett feladat végrehajtási házirendet, amely a feladat sikertelen feladatok gyakorlatilag végtelen újrapróbálkozások okozhat a exponenciális leállítási az intervallumok között minden próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-181">Elastic Database jobs includes a default job execution policy which cause essentially infinite retries of job task failures with exponential backoff of intervals between each retry.</span></span></p>
    </td>
    <td>
    <p><span data-ttu-id="c7ba6-182">Get-AzureSqlJobExecutionPolicy</span><span class="sxs-lookup"><span data-stu-id="c7ba6-182">Get-AzureSqlJobExecutionPolicy</span></span></p>
    <p><span data-ttu-id="c7ba6-183">Új AzureSqlJobExecutionPolicy</span><span class="sxs-lookup"><span data-stu-id="c7ba6-183">New-AzureSqlJobExecutionPolicy</span></span></p>
    <p><span data-ttu-id="c7ba6-184">Set-AzureSqlJobExecutionPolicy</span><span class="sxs-lookup"><span data-stu-id="c7ba6-184">Set-AzureSqlJobExecutionPolicy</span></span></p>
    </td>
  </tr>

<tr>
    <td><span data-ttu-id="c7ba6-185">Ütemezés</span><span class="sxs-lookup"><span data-stu-id="c7ba6-185">Schedule</span></span></td>
    <td>
    <p><span data-ttu-id="c7ba6-186">Ideje alapján kerül sor, vagy a feladatról időköz, vagy egy alkalommal végrehajtási előírása.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-186">Time based specification for execution to take place either on a reoccurring interval or at a single time.</span></span></p>
    </td>
    <td>
    <p><span data-ttu-id="c7ba6-187">Get-AzureSqlJobSchedule</span><span class="sxs-lookup"><span data-stu-id="c7ba6-187">Get-AzureSqlJobSchedule</span></span></p>
    <p><span data-ttu-id="c7ba6-188">Új AzureSqlJobSchedule</span><span class="sxs-lookup"><span data-stu-id="c7ba6-188">New-AzureSqlJobSchedule</span></span></p>
    <p><span data-ttu-id="c7ba6-189">Set-AzureSqlJobSchedule</span><span class="sxs-lookup"><span data-stu-id="c7ba6-189">Set-AzureSqlJobSchedule</span></span></p>
    </td>
  </tr>

<tr>
    <td><span data-ttu-id="c7ba6-190">Feladat indítja el</span><span class="sxs-lookup"><span data-stu-id="c7ba6-190">Job Triggers</span></span></td>
    <td>
    <p><span data-ttu-id="c7ba6-191">A feladatok és az ütemezés szerint eseményindító feladat végrehajtása a kívánt ütemezést közötti leképezést.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-191">A mapping between a job and a schedule to trigger job execution according to the schedule.</span></span></p>
    </td>
    <td>
    <p><span data-ttu-id="c7ba6-192">Új AzureSqlJobTrigger</span><span class="sxs-lookup"><span data-stu-id="c7ba6-192">New-AzureSqlJobTrigger</span></span></p>
    <p><span data-ttu-id="c7ba6-193">Remove-AzureSqlJobTrigger</span><span class="sxs-lookup"><span data-stu-id="c7ba6-193">Remove-AzureSqlJobTrigger</span></span></p>
    </td>
  </tr>
</table>

## <a name="supported-elastic-database-jobs-group-types"></a><span data-ttu-id="c7ba6-194">Támogatott rugalmas adatbázis-feladatok csoportban típusok</span><span class="sxs-lookup"><span data-stu-id="c7ba6-194">Supported Elastic Database jobs group types</span></span>
<span data-ttu-id="c7ba6-195">A feladat végrehajtja a Transact-SQL (T-SQL) parancsfájlok vagy DACPACs alkalmazásának adatbázisok csoportja között.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-195">The job executes Transact-SQL (T-SQL) scripts or application of DACPACs across a group of databases.</span></span> <span data-ttu-id="c7ba6-196">Amikor egy feladat adatbázisok csoportja között hajtható végre, a feladat "kiterjeszti" a gyermek feladatok, ahol minden egyes hajtja végre a kért végrehajtási egyetlen adatbázis csoport be.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-196">When a job is submitted to be executed across a group of databases, the job “expands” the into child jobs where each performs the requested execution against a single database in the group.</span></span> 

<span data-ttu-id="c7ba6-197">A csoportokat, amelyek hozhat létre két típusa van:</span><span class="sxs-lookup"><span data-stu-id="c7ba6-197">There are two types of groups that you can create:</span></span> 

* <span data-ttu-id="c7ba6-198">[A shard térkép](sql-database-elastic-scale-shard-map-management.md) csoport: amikor egy feladatot, amelyekre a shard térképet, a feladat lekérdezi az aktuális meg a szilánkok a shard térkép, és majd létrehozza gyermek minden shard feladataihoz a shard térkép.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-198">[Shard Map](sql-database-elastic-scale-shard-map-management.md) group: When a job is submitted to target a shard map, the job queries the shard map to determine its current set of shards, and then creates child jobs for each shard in the shard map.</span></span>
* <span data-ttu-id="c7ba6-199">Egyéni gyűjtési csoportjának: egy egyénileg definiált adatbázisok készleteit.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-199">Custom Collection group: A custom defined set of databases.</span></span> <span data-ttu-id="c7ba6-200">Ha egy feladat egyéni gyűjtemény célja, azt gyermek feladatok létrehozása az egyes adatbázisok jelenleg az egyéni gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-200">When a job targets a custom collection, it creates child jobs for each database currently in the custom collection.</span></span>

## <a name="to-set-the-elastic-database-jobs-connection"></a><span data-ttu-id="c7ba6-201">Állítsa be a rugalmas feladatok a kapcsolat</span><span class="sxs-lookup"><span data-stu-id="c7ba6-201">To set the Elastic Database jobs connection</span></span>
<span data-ttu-id="c7ba6-202">A kapcsolat kell állítani a feladat *feladatvezérlő adatbázishoz* a feladatok API-k használata előtt.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-202">A connection needs to be set to the jobs *control database* prior to using the jobs APIs.</span></span> <span data-ttu-id="c7ba6-203">Egy hitelesítő adat ablakot, ahol a felhasználónevet és a rugalmas feladatok telepítésekor létrehozott jelszót kérő felugró futtatja ezt a parancsmagot váltja ki.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-203">Running this cmdlet triggers a credential window to pop up requesting the user name and password created when installing Elastic Database jobs.</span></span> <span data-ttu-id="c7ba6-204">Ebben a témakörben közölt összes példák feltételezik, hogy az első lépéshez már végbementek.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-204">All examples provided within this topic assume that this first step has already been performed.</span></span>

<span data-ttu-id="c7ba6-205">Nyissa meg a rugalmas feladatok kapcsolatot:</span><span class="sxs-lookup"><span data-stu-id="c7ba6-205">Open a connection to the Elastic Database jobs:</span></span>

    Use-AzureSqlJobConnection -CurrentAzureSubscription 

## <a name="encrypted-credentials-within-the-elastic-database-jobs"></a><span data-ttu-id="c7ba6-206">A rugalmas feladatok belül titkosított hitelesítő adatokat</span><span class="sxs-lookup"><span data-stu-id="c7ba6-206">Encrypted credentials within the Elastic Database jobs</span></span>
<span data-ttu-id="c7ba6-207">Adatbázis-hitelesítő adatok szúrhatók be, a feladatok *feladatvezérlő adatbázishoz* a jelszóval titkosított.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-207">Database credentials can be inserted into the jobs *control database*  with its password encrypted.</span></span> <span data-ttu-id="c7ba6-208">Fontos engedélyezhetik a feladatokat hajthatnak végre egy későbbi időpontban (használatával a feladatok ütemezésének) hitelesítő adatok tárolását.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-208">It is necessary to store credentials to enable jobs to be executed at a later time, (using job schedules).</span></span>

<span data-ttu-id="c7ba6-209">Titkosítási működik, hozza létre a telepítési parancsfájl részeként tanúsítvány keresztül.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-209">Encryption works through a certificate created as part of the installation script.</span></span> <span data-ttu-id="c7ba6-210">A telepítési parancsfájlt hoz létre, és a tanúsítvány tölt be az Azure-Felhőszolgáltatásban tárolt titkosított jelszavak visszafejtéséhez.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-210">The installation script creates and uploads the certificate into the Azure Cloud Service for decryption of the stored encrypted passwords.</span></span> <span data-ttu-id="c7ba6-211">Az Azure Cloud Service később tárolja a nyilvános kulcsot a feladatok belül *feladatvezérlő adatbázishoz* lehetővé teszi a megadott jelszóval titkosításához anélkül, hogy helyileg telepíteni kell a tanúsítványt a PowerShell API-t vagy a klasszikus Azure portál felület.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-211">The Azure Cloud Service later stores the public key within the jobs *control database* which enables the PowerShell API or Azure Classic Portal interface to encrypt a provided password without requiring the certificate to be locally installed.</span></span>

<span data-ttu-id="c7ba6-212">A hitelesítő adatok jelszavak titkosított és védett, csak olvasási hozzáféréssel rendelkező felhasználók a rugalmas adatbázis-feladatok objektumok.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-212">The credential passwords are encrypted and secure from users with read-only access to Elastic Database jobs objects.</span></span> <span data-ttu-id="c7ba6-213">Azonban lehetséges, hogy egy rosszindulatú felhasználó rugalmas adatbázis-feladatok objektumok olvasási és írási hozzáféréssel rendelkező bontsa ki a jelszót.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-213">But it is possible for a malicious user with read-write access to Elastic Database Jobs objects to extract a password.</span></span> <span data-ttu-id="c7ba6-214">Hitelesítő adatok is felhasználják feladat végrehajtások készültek.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-214">Credentials are designed to be reused across job executions.</span></span> <span data-ttu-id="c7ba6-215">Hitelesítő adatai továbbítódnak céladatbázisokhoz kapcsolatok létesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-215">Credentials are passed to target databases when establishing connections.</span></span> <span data-ttu-id="c7ba6-216">Jelenleg nincs korlátozás az egyes hitelesítő adatot használja a céladatbázisokhoz, rosszindulatú felhasználó hozzáadhatja a rosszindulatú felhasználók vezérlése alatt adatbázis egy adatbázis cél.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-216">There are currently no restrictions on the target databases used for each credential, malicious user could add a database target for a database under the malicious user's control.</span></span> <span data-ttu-id="c7ba6-217">A felhasználó később volt az adatbázis ahhoz, hogy a hitelesítésre szolgáló jelszó célzó feladat indítása.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-217">The user could subsequently start a job targeting this database to gain the credential's password.</span></span>

<span data-ttu-id="c7ba6-218">Ajánlott biztonsági eljárások a rugalmas adatbázis-feladatok a következők:</span><span class="sxs-lookup"><span data-stu-id="c7ba6-218">Security best practices for Elastic Database jobs include:</span></span>

* <span data-ttu-id="c7ba6-219">A megbízható személyek API-használatát korlátozása.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-219">Limit usage of the APIs to trusted individuals.</span></span>
* <span data-ttu-id="c7ba6-220">Hitelesítő adatok a feladat végrehajtásához szükséges a lehető legkevesebb jogosultsággal kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-220">Credentials should have the least privileges necessary to perform the job task.</span></span>  <span data-ttu-id="c7ba6-221">További információ látható belül ez [engedélyezési és engedélyek](https://msdn.microsoft.com/library/bb669084.aspx) SQL Server MSDN-cikk tárgyalja.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-221">More information can be seen within this [Authorization and Permissions](https://msdn.microsoft.com/library/bb669084.aspx) SQL Server MSDN article.</span></span>

### <a name="to-create-an-encrypted-credential-for-job-execution-across-databases"></a><span data-ttu-id="c7ba6-222">Az adatbázisok közötti egy titkosított hitelesítő adatokat, a feladat végrehajtása létrehozásához</span><span class="sxs-lookup"><span data-stu-id="c7ba6-222">To create an encrypted credential for job execution across databases</span></span>
<span data-ttu-id="c7ba6-223">Egy új titkosított hitelesítő adat létrehozása a [ **Get-Credential parancsmag** ](https://technet.microsoft.com/library/hh849815.aspx) egy felhasználónevet és jelszót, amely átadhatók kérni fogja a [ **New-AzureSqlJobCredential parancsmag**](/powershell/module/elasticdatabasejobs/new-azuresqljobcredential).</span><span class="sxs-lookup"><span data-stu-id="c7ba6-223">To create a new encrypted credential, the [**Get-Credential cmdlet**](https://technet.microsoft.com/library/hh849815.aspx) prompts for a user name and password that can be passed to the [**New-AzureSqlJobCredential cmdlet**](/powershell/module/elasticdatabasejobs/new-azuresqljobcredential).</span></span>

    $credentialName = "{Credential Name}"
    $databaseCredential = Get-Credential
    $credential = New-AzureSqlJobCredential -Credential $databaseCredential -CredentialName $credentialName
    Write-Output $credential

### <a name="to-update-credentials"></a><span data-ttu-id="c7ba6-224">Hitelesítő adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="c7ba6-224">To update credentials</span></span>
<span data-ttu-id="c7ba6-225">Ha jelszó módosításához használja a [ **Set-AzureSqlJobCredential parancsmag** ](/powershell/module/elasticdatabasejobs/set-azuresqljobcredential) és állítsa be a **CredentialName** paraméter.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-225">When passwords change, use the [**Set-AzureSqlJobCredential cmdlet**](/powershell/module/elasticdatabasejobs/set-azuresqljobcredential) and set the **CredentialName** parameter.</span></span>

    $credentialName = "{Credential Name}"
    Set-AzureSqlJobCredential -CredentialName $credentialName -Credential $credential 

## <a name="to-define-an-elastic-database-shard-map-target"></a><span data-ttu-id="c7ba6-226">Egy rugalmas adatbázist shard térkép célhoz meghatározása</span><span class="sxs-lookup"><span data-stu-id="c7ba6-226">To define an Elastic Database shard map target</span></span>
<span data-ttu-id="c7ba6-227">Shard csoportban lévő összes adatbázisokhoz egy feladat végrehajtásához (használatával létrehozott [Elastic Database ügyféloldali kódtárának](sql-database-elastic-database-client-library.md)), használja a shard leképezését adatbázis céljaként.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-227">To execute a job against all databases in a shard set (created using [Elastic Database client library](sql-database-elastic-database-client-library.md)), use a shard map as the database target.</span></span> <span data-ttu-id="c7ba6-228">Ebben a példában az Elastic Database ügyféloldali kódtár használatával létrehozott szilánkos alkalmazás szükséges.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-228">This example requires a sharded application created using the Elastic Database client library.</span></span> <span data-ttu-id="c7ba6-229">Lásd: [Ismerkedés a rugalmas adatbázis eszközök minta](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="c7ba6-229">See [Getting started with Elastic Database tools sample](sql-database-elastic-scale-get-started.md).</span></span>

<span data-ttu-id="c7ba6-230">A szilánkok manager adatbázist be kell állítani egy adatbázis célként, és majd a megadott shard térkép meg kell adni, célként.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-230">The shard map manager database must be set as a database target and then the specific shard map must be specified as a target.</span></span>

    $shardMapCredentialName = "{Credential Name}"
    $shardMapDatabaseName = "{ShardMapDatabaseName}" #example: ElasticScaleStarterKit_ShardMapManagerDb
    $shardMapDatabaseServerName = "{ShardMapServerName}"
    $shardMapName = "{MyShardMap}" #example: CustomerIDShardMap
    $shardMapDatabaseTarget = New-AzureSqlJobTarget -DatabaseName $shardMapDatabaseName -ServerName $shardMapDatabaseServerName
    $shardMapTarget = New-AzureSqlJobTarget -ShardMapManagerCredentialName $shardMapCredentialName -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapDatabaseServerName -ShardMapName $shardMapName
    Write-Output $shardMapTarget

## <a name="create-a-t-sql-script-for-execution-across-databases"></a><span data-ttu-id="c7ba6-231">Az adatbázisok közötti végrehajtási T-SQL parancsfájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="c7ba6-231">Create a T-SQL Script for execution across databases</span></span>
<span data-ttu-id="c7ba6-232">T-SQL-parancsfájlok végrehajtásra létrehozásakor ajánlott hozhat létre, azok [idempotent](https://en.wikipedia.org/wiki/Idempotence) és refs-hibákkal szemben.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-232">When creating T-SQL scripts for execution, it is highly recommended to build them to be [idempotent](https://en.wikipedia.org/wiki/Idempotence) and resilient against failures.</span></span> <span data-ttu-id="c7ba6-233">Rugalmas adatbázis-feladatok parancsfájl végrehajtásának próbálkozik, ha végrehajtási hiba, függetlenül a besorolás, a hiba lép fel.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-233">Elastic Database jobs will retry execution of a script whenever execution encounters a failure, regardless of the classification of the failure.</span></span>

<span data-ttu-id="c7ba6-234">Használja a [ **New-AzureSqlJobContent parancsmag** ](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent) létrehozása és mentése a parancsfájl végrehajtása és a beállított a **- ContentName** és **- CommandText** paraméterek.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-234">Use the [**New-AzureSqlJobContent cmdlet**](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent) to create and save a script for execution and set the **-ContentName** and **-CommandText** parameters.</span></span>

    $scriptName = "Create a TestTable"

    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
        CREATE TABLE TestTable(
            TestTableId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="create-a-new-script-from-a-file"></a><span data-ttu-id="c7ba6-235">Új parancsfájl létrehozása fájlból</span><span class="sxs-lookup"><span data-stu-id="c7ba6-235">Create a new script from a file</span></span>
<span data-ttu-id="c7ba6-236">Ha a T-SQL parancsfájl fájlban van definiálva, ennek segítségével importálja a parancsfájl:</span><span class="sxs-lookup"><span data-stu-id="c7ba6-236">If the T-SQL script is defined within a file, use this to import the script:</span></span>

    $scriptName = "My Script Imported from a File"
    $scriptPath = "{Path to SQL File}"
    $scriptCommandText = Get-Content -Path $scriptPath
    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="to-update-a-t-sql-script-for-execution-across-databases"></a><span data-ttu-id="c7ba6-237">Az adatbázisok közötti egy T-SQL-parancsfájlt végrehajtó frissítése</span><span class="sxs-lookup"><span data-stu-id="c7ba6-237">To update a T-SQL script for execution across databases</span></span>
<span data-ttu-id="c7ba6-238">A PowerShell parancsfájl a T-SQL egy meglévő parancsfájl parancsszövege frissíti.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-238">This PowerShell script updates the T-SQL command text for an existing script.</span></span>

<span data-ttu-id="c7ba6-239">Állítsa be a következő változókat kell beállítani a kívánt parancsfájl definíciójának megfelelően:</span><span class="sxs-lookup"><span data-stu-id="c7ba6-239">Set the following variables to reflect the desired script definition to be set:</span></span>

    $scriptName = "Create a TestTable"
    $scriptUpdateComment = "Adding AdditionalInformation column to TestTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
    CREATE TABLE TestTable(
        TestTableId INT PRIMARY KEY IDENTITY,
        InsertionTime DATETIME2
    );
    END
    GO

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN
    ALTER TABLE TestTable
    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO"

### <a name="to-update-the-definition-to-an-existing-script"></a><span data-ttu-id="c7ba6-240">A definíció frissíteni egy meglévő parancsfájl</span><span class="sxs-lookup"><span data-stu-id="c7ba6-240">To update the definition to an existing script</span></span>
    Set-AzureSqlJobContentDefinition -ContentName $scriptName -CommandText $scriptCommandText -Comment $scriptUpdateComment 

## <a name="to-create-a-job-to-execute-a-script-across-a-shard-map"></a><span data-ttu-id="c7ba6-241">A parancsfájl végrehajtása shard térképre között feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="c7ba6-241">To create a job to execute a script across a shard map</span></span>
<span data-ttu-id="c7ba6-242">A PowerShell parancsfájl keresztül minden shard rugalmasan méretezhető shard leképezés indít el egy parancsfájl végrehajtásának feladatot.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-242">This PowerShell script starts a job for execution of a script across each shard in an Elastic Scale shard map.</span></span>

<span data-ttu-id="c7ba6-243">Állítsa be a következő változókat, hogy tükrözze a kívánt parancsfájlt és a cél:</span><span class="sxs-lookup"><span data-stu-id="c7ba6-243">Set the following variables to reflect the desired script and target:</span></span>

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $credentialName = "{Credential Name}"
    $shardMapTarget = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName 
    $job = New-AzureSqlJob -ContentName $scriptName -CredentialName $credentialName -JobName $jobName -TargetId $shardMapTarget.TargetId
    Write-Output $job

## <a name="to-execute-a-job"></a><span data-ttu-id="c7ba6-244">A feladat végrehajtásához</span><span class="sxs-lookup"><span data-stu-id="c7ba6-244">To execute a job</span></span>
<span data-ttu-id="c7ba6-245">A PowerShell-parancsfájl egy létező feladat végrehajtása:</span><span class="sxs-lookup"><span data-stu-id="c7ba6-245">This PowerShell script executes an existing job:</span></span>

<span data-ttu-id="c7ba6-246">Frissítse a következő változót a kívánt feladat neve végrehajtott megfelelően:</span><span class="sxs-lookup"><span data-stu-id="c7ba6-246">Update the following variable to reflect the desired job name to have executed:</span></span>

    $jobName = "{Job Name}"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName 
    Write-Output $jobExecution

## <a name="to-retrieve-the-state-of-a-single-job-execution"></a><span data-ttu-id="c7ba6-247">Egyetlen feladat-végrehajtási állapotának beolvasása</span><span class="sxs-lookup"><span data-stu-id="c7ba6-247">To retrieve the state of a single job execution</span></span>
<span data-ttu-id="c7ba6-248">Használja a [ **Get-AzureSqlJobExecution parancsmag** ](/powershell/module/elasticdatabasejobs/get-azuresqljobexecution) és állítsa be a **JobExecutionId** paraméter segítségével megtekintheti a feladat végrehajtási állapotát.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-248">Use the [**Get-AzureSqlJobExecution cmdlet**](/powershell/module/elasticdatabasejobs/get-azuresqljobexecution) and set the **JobExecutionId** parameter to view the state of job execution.</span></span>

    $jobExecutionId = "{Job Execution Id}"
    $jobExecution = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId
    Write-Output $jobExecution

<span data-ttu-id="c7ba6-249">Ugyanazon **Get-AzureSqlJobExecution** parancsmagot a **IncludeChildren** paraméter gyermek feladat végrehajtások, nevezetesen a minden feladat végrehajtása a feladat által megcélzott minden adatbázison meghatározott állapotban állapotának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-249">Use the same **Get-AzureSqlJobExecution** cmdlet with the **IncludeChildren** parameter to view the state of child job executions, namely the specific state for each job execution against each database targeted by the job.</span></span>

    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions 

## <a name="to-view-the-state-across-multiple-job-executions"></a><span data-ttu-id="c7ba6-250">Több feladat végrehajtások közötti az állapot megtekintése</span><span class="sxs-lookup"><span data-stu-id="c7ba6-250">To view the state across multiple job executions</span></span>
<span data-ttu-id="c7ba6-251">A [ **Get-AzureSqlJobExecution parancsmag** ](/powershell/module/elasticdatabasejobs/new-azuresqljob) több választható paraméterek: több feladat végrehajtások, a megadott paramétereknek szűrt megjelenítéséhez használható.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-251">The [**Get-AzureSqlJobExecution cmdlet**](/powershell/module/elasticdatabasejobs/new-azuresqljob) has multiple optional parameters that can be used to display multiple job executions, filtered through the provided parameters.</span></span> <span data-ttu-id="c7ba6-252">A következő mutatja be a Get-AzureSqlJobExecution használatának lehetséges módjai közül:</span><span class="sxs-lookup"><span data-stu-id="c7ba6-252">The following demonstrates some of the possible ways to use Get-AzureSqlJobExecution:</span></span>

<span data-ttu-id="c7ba6-253">Lekéri az összes aktív felső szintű feladat végrehajtások:</span><span class="sxs-lookup"><span data-stu-id="c7ba6-253">Retrieve all active top level job executions:</span></span>

    Get-AzureSqlJobExecution

<span data-ttu-id="c7ba6-254">Lekéri az összes felső szintű feladat végrehajtások, beleértve az inaktív feladat végrehajtások:</span><span class="sxs-lookup"><span data-stu-id="c7ba6-254">Retrieve all top level job executions, including inactive job executions:</span></span>

    Get-AzureSqlJobExecution -IncludeInactive

<span data-ttu-id="c7ba6-255">Lekéri az összes gyermek feladat végrehajtások inaktív feladat végrehajtások többek között a megadott feladat végrehajtási Azonosítóhoz:</span><span class="sxs-lookup"><span data-stu-id="c7ba6-255">Retrieve all child job executions of a provided job execution ID, including inactive job executions:</span></span>

    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId -IncludeInactive -IncludeChildren

<span data-ttu-id="c7ba6-256">Ütemezés használatával létrehozott összes feladat végrehajtások beolvasása / feladat-kombináció, beleértve az inaktív feladatok:</span><span class="sxs-lookup"><span data-stu-id="c7ba6-256">Retrieve all job executions created using a schedule / job combination, including inactive jobs:</span></span>

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive

<span data-ttu-id="c7ba6-257">Lekéri az összes feladat megadott shard térképre, beleértve az inaktív feladatokat célcsoportkezelést:</span><span class="sxs-lookup"><span data-stu-id="c7ba6-257">Retrieve all jobs targeting a specified shard map, including inactive jobs:</span></span>

    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive

<span data-ttu-id="c7ba6-258">Lekéri az összes feladat a megadott egyéni gyűjtemény, beleértve az inaktív feladatokat célcsoportkezelést:</span><span class="sxs-lookup"><span data-stu-id="c7ba6-258">Retrieve all jobs targeting a specified custom collection, including inactive jobs:</span></span>

    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive

<span data-ttu-id="c7ba6-259">Feladat feladat végrehajtások belül egy adott feladat végrehajtási listájának beolvasása:</span><span class="sxs-lookup"><span data-stu-id="c7ba6-259">Retrieve the list of job task executions within a specific job execution:</span></span>

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions 

<span data-ttu-id="c7ba6-260">Feladat végrehajtási részlete beolvasása:</span><span class="sxs-lookup"><span data-stu-id="c7ba6-260">Retrieve job task execution details:</span></span>

<span data-ttu-id="c7ba6-261">A következő PowerShell-parancsfájl segítségével egy feladat a feladat a végrehajtás, amely akkor különösen hasznos, ha végrehajtási hibakeresésére részleteinek megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-261">The following PowerShell script can be used to view the details of a job task execution, which is particularly useful when debugging execution failures.</span></span>

    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution

## <a name="to-retrieve-failures-within-job-task-executions"></a><span data-ttu-id="c7ba6-262">Sikertelen feladat feladat végrehajtások belül beolvasása</span><span class="sxs-lookup"><span data-stu-id="c7ba6-262">To retrieve failures within job task executions</span></span>
<span data-ttu-id="c7ba6-263">A **JobTaskExecution objektum** egy tulajdonság az életciklus a feladat együtt egy üzenettulajdonságot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-263">The **JobTaskExecution object** includes a property for the lifecycle of the task along with a message property.</span></span> <span data-ttu-id="c7ba6-264">Ha egy feladat a feladat végrehajtása sikertelen volt, a életciklus tulajdonság úgy lesz beállítva, *sikertelen* és üzenettulajdonság állítja be az eredményül kapott hibaüzenetet, és a verem.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-264">If a job task execution failed, the lifecycle property will be set to *Failed* and the message property will be set to the resulting exception message and its stack.</span></span> <span data-ttu-id="c7ba6-265">Ha egy feladat sikertelen volt, fontos, amely egy adott feladat sikertelen feladat-feladatok részletes adatainak megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-265">If a job did not succeed, it is important to view the details of job tasks that did not succeed for a given job.</span></span>

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions) 
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }

## <a name="to-wait-for-a-job-execution-to-complete"></a><span data-ttu-id="c7ba6-266">Várjon a feladat-végrehajtás befejeződésére</span><span class="sxs-lookup"><span data-stu-id="c7ba6-266">To wait for a job execution to complete</span></span>
<span data-ttu-id="c7ba6-267">A következő PowerShell-parancsfájl segítségével Várjon, amíg a feladat feladat elvégzéséhez:</span><span class="sxs-lookup"><span data-stu-id="c7ba6-267">The following PowerShell script can be used to wait for a job task to complete:</span></span>

    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId 

## <a name="create-a-custom-execution-policy"></a><span data-ttu-id="c7ba6-268">Egyéni végrehajtási házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="c7ba6-268">Create a custom execution policy</span></span>
<span data-ttu-id="c7ba6-269">Rugalmas adatbázis-feladatok feladat indításakor alkalmazható egyéni végrehajtási házirendek létrehozását támogatja.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-269">Elastic Database jobs supports creating custom execution policies that can be applied when starting jobs.</span></span>

<span data-ttu-id="c7ba6-270">Végrehajtási házirendek definiálása jelenleg engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="c7ba6-270">Execution policies currently allow for defining:</span></span>

* <span data-ttu-id="c7ba6-271">Name: A végrehajtási házirend azonosítója.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-271">Name: Identifier for the execution policy.</span></span>
* <span data-ttu-id="c7ba6-272">Feladat időtúllépése: Teljes idő előtt rugalmas adatbázis-feladatok megszakítja a feladatot.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-272">Job Timeout: Total time before a job will be canceled by Elastic Database Jobs.</span></span>
* <span data-ttu-id="c7ba6-273">Kezdeti újrapróbálkozási időköz: Intervallum várjon próbálkozik újra.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-273">Initial Retry Interval: Interval to wait before first retry.</span></span>
* <span data-ttu-id="c7ba6-274">Maximális újrapróbálkozási időköz: Cap újrapróbálkozási intervallumok használatára.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-274">Maximum Retry Interval: Cap of retry intervals to use.</span></span>
* <span data-ttu-id="c7ba6-275">Újrapróbálkozási időköz leállítási együttható: A következő intervallum próbálkozások közötti különbség kifejezésére szolgáló együttható.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-275">Retry Interval Backoff Coefficient: Coefficient used to calculate the next interval between retries.</span></span>  <span data-ttu-id="c7ba6-276">Az alábbi képlet használható: (kezdeti újrapróbálkozási időközét) * Math.pow ((időköz leállítási együttható), (próbálkozások száma) - 2).</span><span class="sxs-lookup"><span data-stu-id="c7ba6-276">The following formula is used: (Initial Retry Interval) * Math.pow((Interval Backoff Coefficient), (Number of Retries) - 2).</span></span> 
* <span data-ttu-id="c7ba6-277">Kísérletek maximális száma: Az újrapróbálkozások maximális számát megkísérel végrehajtani egy feladatot belül.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-277">Maximum Attempts: The maximum number of retry attempts to perform within a job.</span></span>

<span data-ttu-id="c7ba6-278">Az alapértelmezett végrehajtási házirendet a következő értékeket használja:</span><span class="sxs-lookup"><span data-stu-id="c7ba6-278">The default execution policy uses the following values:</span></span>

* <span data-ttu-id="c7ba6-279">Name: Alapértelmezett végrehajtási házirend</span><span class="sxs-lookup"><span data-stu-id="c7ba6-279">Name: Default execution policy</span></span>
* <span data-ttu-id="c7ba6-280">Feladat időtúllépése: 1 hét</span><span class="sxs-lookup"><span data-stu-id="c7ba6-280">Job Timeout: 1 week</span></span>
* <span data-ttu-id="c7ba6-281">Kezdeti újrapróbálkozási időköz: 100 milliomod másodperc</span><span class="sxs-lookup"><span data-stu-id="c7ba6-281">Initial Retry Interval:  100 milliseconds</span></span>
* <span data-ttu-id="c7ba6-282">Maximális újrapróbálkozási időköz: 30 perc</span><span class="sxs-lookup"><span data-stu-id="c7ba6-282">Maximum Retry Interval: 30 minutes</span></span>
* <span data-ttu-id="c7ba6-283">Ismételje meg a időköz együttható: 2. régiója</span><span class="sxs-lookup"><span data-stu-id="c7ba6-283">Retry Interval Coefficient: 2</span></span>
* <span data-ttu-id="c7ba6-284">Kísérletek maximális száma: 2 147 483 647</span><span class="sxs-lookup"><span data-stu-id="c7ba6-284">Maximum Attempts: 2,147,483,647</span></span>

<span data-ttu-id="c7ba6-285">A kívánt végrehajtási szabályzat létrehozása:</span><span class="sxs-lookup"><span data-stu-id="c7ba6-285">Create the desired execution policy:</span></span>

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 10
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval 
    -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $executionPolicy

### <a name="update-a-custom-execution-policy"></a><span data-ttu-id="c7ba6-286">Egy egyéni végrehajtási házirend frissítése</span><span class="sxs-lookup"><span data-stu-id="c7ba6-286">Update a custom execution policy</span></span>
<span data-ttu-id="c7ba6-287">A frissíteni kívánt végrehajtási házirend frissítése:</span><span class="sxs-lookup"><span data-stu-id="c7ba6-287">Update the desired execution policy to update:</span></span>

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 15
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $updatedExecutionPolicy = Set-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $updatedExecutionPolicy

## <a name="cancel-a-job"></a><span data-ttu-id="c7ba6-288">Feladatok megszakítása</span><span class="sxs-lookup"><span data-stu-id="c7ba6-288">Cancel a job</span></span>
<span data-ttu-id="c7ba6-289">Rugalmas adatbázis-feladatok a feladatok megszakítását kérelmeket támogatja.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-289">Elastic Database Jobs supports cancellation requests of jobs.</span></span>  <span data-ttu-id="c7ba6-290">Ha a rugalmas adatbázis-feladatok jelenleg végrehajtás alatt álló feladat a lemondási kérelmet észlel, akkor megkísérli a feladat leállítása.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-290">If Elastic Database Jobs detects a cancellation request for a job currently being executed, it will attempt to stop the job.</span></span>

<span data-ttu-id="c7ba6-291">Rugalmas adatbázis-feladatok is hajtsa végre a megszakítási két különböző módja van:</span><span class="sxs-lookup"><span data-stu-id="c7ba6-291">There are two different ways that Elastic Database Jobs can perform a cancellation:</span></span>

1. <span data-ttu-id="c7ba6-292">Jelenleg feldolgozás alatt álló feladatok Mégse: törlése közben jelenleg fut egy feladat észleli, ha a megszakítási kísérli meg a rendszer a jelenleg végrehajtás alatt álló szempontja, hogy a feladat belül.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-292">Cancel currently executing tasks: If a cancellation is detected while a task is currently running, a cancellation will be attempted within the currently executing aspect of the task.</span></span>  <span data-ttu-id="c7ba6-293">Példa: Ha egy hosszú ideig tartó jelenleg végrehajtás alatt álló lekérdezés során a törlése megkísérlése, az a lekérdezés kísérlet lesz.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-293">For example: If there is a long running query currently being performed when a cancellation is attempted, there will be an attempt to cancel the query.</span></span>
2. <span data-ttu-id="c7ba6-294">Tevékenység-újrapróbálkozások megszakítása: törlése a vezérlő szál észlelésekor feladat a végrehajtás elindítása előtt, a vezérlő szál elkerülése érdekében a feladat futtatására és a kérelem deklarálható megszakítottként.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-294">Canceling task retries: If a cancellation is detected by the control thread before a task is launched for execution, the control thread will avoid launching the task and declare the request as canceled.</span></span>

<span data-ttu-id="c7ba6-295">A feladat megszakításának szülő-feladat van szükség, ha a lemondási kérelmet szembeni szerződéses kötelezettségeket a szülő feladat és összes a gyermek-feladatokkal.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-295">If a job cancellation is requested for a parent job, the cancellation request will be honored for the parent job and for all of its child jobs.</span></span>

<span data-ttu-id="c7ba6-296">Megszakítási kérelmet küldeni, használja a [ **Stop-AzureSqlJobExecution parancsmag** ](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) és állítsa be a **JobExecutionId** paraméter.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-296">To submit a cancellation request, use the [**Stop-AzureSqlJobExecution cmdlet**](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) and set the **JobExecutionId** parameter.</span></span>

    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId

## <a name="to-delete-a-job-and-job-history-asynchronously"></a><span data-ttu-id="c7ba6-297">Törli a feladatot, és aszinkron módon feladatelőzmények</span><span class="sxs-lookup"><span data-stu-id="c7ba6-297">To delete a job and job history asynchronously</span></span>
<span data-ttu-id="c7ba6-298">Rugalmas adatbázis-feladatok támogatja az aszinkron feladatok törlése.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-298">Elastic Database jobs supports asynchronous deletion of jobs.</span></span> <span data-ttu-id="c7ba6-299">Egy feladat is törlésre, és a rendszer törli a feladatot, és a feladatelőzményekben összes feladat végrehajtások a feladat befejezése után.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-299">A job can be marked for deletion and the system will delete the job and all its job history after all job executions have completed for the job.</span></span> <span data-ttu-id="c7ba6-300">A rendszer nem fogja automatikusan megszakítja a aktív feladat végrehajtások.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-300">The system will not automatically cancel active job executions.</span></span>  

<span data-ttu-id="c7ba6-301">Invoke [ **Stop-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) megszakítja az aktív feladat végrehajtások.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-301">Invoke [**Stop-AzureSqlJobExecution**](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) to cancel active job executions.</span></span>

<span data-ttu-id="c7ba6-302">Törlési feladat indításához használja a [ **Remove-AzureSqlJob parancsmag** ](/powershell/module/elasticdatabasejobs/remove-azuresqljob) és állítsa be a **JobName** paraméter.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-302">To trigger job deletion, use the [**Remove-AzureSqlJob cmdlet**](/powershell/module/elasticdatabasejobs/remove-azuresqljob) and set the **JobName** parameter.</span></span>

    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName

## <a name="to-create-a-custom-database-target"></a><span data-ttu-id="c7ba6-303">Egy egyéni adatbázis-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="c7ba6-303">To create a custom database target</span></span>
<span data-ttu-id="c7ba6-304">Egyéni adatbázis célok közvetlen végrehajtásra vagy egy egyéni adatbázis csoportban foglalható adhat meg.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-304">You can define custom database targets either for direct execution or for inclusion within a custom database group.</span></span> <span data-ttu-id="c7ba6-305">Például mert **rugalmas készletek** van még nem támogatott PowerShell API-val, létrehozhat egy egyéni adatbázis célként és egy egyéni adatbázis gyűjtemény célként, ami magában foglalja a készlet összes adatbázisát.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-305">For example, because **elastic pools** are not yet directly supported using PowerShell APIs, you can create a custom database target and custom database collection target which encompasses all the databases in the pool.</span></span>

<span data-ttu-id="c7ba6-306">Állítsa be a következő változókat, hogy tükrözze a kívánt adatbázis adatait:</span><span class="sxs-lookup"><span data-stu-id="c7ba6-306">Set the following variables to reflect the desired database information:</span></span>

    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName 

## <a name="to-create-a-custom-database-collection-target"></a><span data-ttu-id="c7ba6-307">Egy egyéni adatbázis gyűjtemény tároló létrehozásához</span><span class="sxs-lookup"><span data-stu-id="c7ba6-307">To create a custom database collection target</span></span>
<span data-ttu-id="c7ba6-308">Használja a [ **New-AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) parancsmag végrehajtásának engedélyezéséhez több meghatározott adatbázis cél között egyéni adatbázis gyűjtemény célja meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-308">Use the [**New-AzureSqlJobTarget**](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) cmdlet to define a custom database collection target to enable execution across multiple defined database targets.</span></span> <span data-ttu-id="c7ba6-309">Egy adatbázis-csoport létrehozása után adatbázisok társítható egyéni gyűjtemény célja.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-309">After creating a database group, databases can be associated with the custom collection target.</span></span>

<span data-ttu-id="c7ba6-310">Állítsa be a kívánt egyéni gyűjtemény konfigurációjához megfelelően a következő változókat:</span><span class="sxs-lookup"><span data-stu-id="c7ba6-310">Set the following variables to reflect the desired custom collection target configuration:</span></span>

    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 

### <a name="to-add-databases-to-a-custom-database-collection-target"></a><span data-ttu-id="c7ba6-311">Adatbázisok hozzáadása egy egyéni adatbázis-gyűjtemény célja</span><span class="sxs-lookup"><span data-stu-id="c7ba6-311">To add databases to a custom database collection target</span></span>
<span data-ttu-id="c7ba6-312">Egy adatbázis hozzáadása egy adott egyéni gyűjtemény használja a [ **Add-AzureSqlJobChildTarget** ](/powershell/module/elasticdatabasejobs/add-azuresqljobchildtarget) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-312">To add a database to a specific custom collection use the [**Add-AzureSqlJobChildTarget**](/powershell/module/elasticdatabasejobs/add-azuresqljobchildtarget) cmdlet.</span></span>

    $databaseServerName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName 

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a><span data-ttu-id="c7ba6-313">Tekintse át az adatbázisokat egy egyéni adatbázis gyűjtemény a cél</span><span class="sxs-lookup"><span data-stu-id="c7ba6-313">Review the databases within a custom database collection target</span></span>
<span data-ttu-id="c7ba6-314">Használja a [ **Get-AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) parancsmag egyéni adatbázis gyűjtemény célja a gyermek adatbázis beolvasása.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-314">Use the [**Get-AzureSqlJobTarget**](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) cmdlet to retrieve the child databases within a custom database collection target.</span></span> 

    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a><span data-ttu-id="c7ba6-315">Hozzon létre egy feladatot, a parancsfájl végrehajtása egy egyéni adatbázis-gyűjtemény célja között</span><span class="sxs-lookup"><span data-stu-id="c7ba6-315">Create a job to execute a script across a custom database collection target</span></span>
<span data-ttu-id="c7ba6-316">Használja a [ **New-AzureSqlJob** ](/powershell/module/elasticdatabasejobs/new-azuresqljob) parancsmag segítségével hozzon létre egy feladatot, egy egyéni adatbázis gyűjtemény tároló által definiált adatbázisok csoportja ellen.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-316">Use the [**New-AzureSqlJob**](/powershell/module/elasticdatabasejobs/new-azuresqljob) cmdlet to create a job against a group of databases defined by a custom database collection target.</span></span> <span data-ttu-id="c7ba6-317">Rugalmas adatbázis-feladatok fog kibővítheti a feladat több gyermek feladat minden egyes adatbázis megfelelő társított egyéni adatbázis gyűjtemény célja, és győződjön meg arról, hogy a parancsfájl végrehajtása minden egyes adatbázison.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-317">Elastic Database jobs will expand the job into multiple child jobs each corresponding to a database associated with the custom database collection target and ensure that the script is executed against each database.</span></span> <span data-ttu-id="c7ba6-318">Ebben az esetben fontos, hogy-e parancsfájlokat idempotent való ismételt próbálkozás rugalmasak lehetnek.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-318">Again, it is important that scripts are idempotent to be resilient to retries.</span></span>

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job

## <a name="data-collection-across-databases"></a><span data-ttu-id="c7ba6-319">Az adatbázisok közötti adatok gyűjtése</span><span class="sxs-lookup"><span data-stu-id="c7ba6-319">Data collection across databases</span></span>
<span data-ttu-id="c7ba6-320">Egy feladat használatával lekérdezés végrehajtása adatbázisok csoportja között, és az eredményt elküldik egy adott táblához.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-320">You can use a job to execute a query across a group of databases and send the results to a specific table.</span></span> <span data-ttu-id="c7ba6-321">A tábla minden egyes adatbázisból a lekérdezési eredmények megtekintése érdekében bekövetkeztek kérdezhetők le.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-321">The table can be queried after the fact to see the query’s results from each database.</span></span> <span data-ttu-id="c7ba6-322">Ez a lekérdezés végrehajtása több adatbázis közötti aszinkron módszert kínál.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-322">This provides an asynchronous method to execute a query across many databases.</span></span> <span data-ttu-id="c7ba6-323">Sikertelen bejelentkezési kísérletek újrapróbálkozások keresztül automatikusan kezeli.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-323">Failed attempts are handled automatically via retries.</span></span>

<span data-ttu-id="c7ba6-324">A megadott célhely tábla automatikusan létrejön, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-324">The specified destination table will be automatically created if it does not yet exist.</span></span> <span data-ttu-id="c7ba6-325">Az új táblázat felel meg a séma, a visszaadott eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-325">The new table matches the schema of the returned result set.</span></span> <span data-ttu-id="c7ba6-326">A parancsfájl több eredménykészlet adja vissza, ha a rugalmas adatbázis-feladatok csak küldeni az első a célként megadott táblája.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-326">If a script returns multiple result sets, Elastic Database jobs will only send the first to the destination table.</span></span>

<span data-ttu-id="c7ba6-327">A következő PowerShell-parancsfájl hajt végre egy parancsfájlt, és összegyűjti az eredményeket egy megadott táblába.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-327">The following PowerShell script executes a script and collects its results into a specified table.</span></span> <span data-ttu-id="c7ba6-328">Ez a parancsfájl feltételezi, hogy egy T-SQL parancsfájl létrejött-e amelyek kimenete egy eredményhalmaz és, hogy létrejött-e egy egyéni adatbázis-gyűjtemény célja.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-328">This script assumes that a T-SQL script has been created which outputs a single result set and that a custom database collection target has been created.</span></span>

<span data-ttu-id="c7ba6-329">A parancsfájl a [ **Get-AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-329">This script uses the [**Get-AzureSqlJobTarget**](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) cmdlet.</span></span> <span data-ttu-id="c7ba6-330">A parancsfájl, a hitelesítő adatokat és a végrehajtási cél paraméterek beállításához:</span><span class="sxs-lookup"><span data-stu-id="c7ba6-330">Set the parameters for script, credentials, and execution target:</span></span>

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $executionCredentialName = "{Execution Credential Name}"
    $customCollectionName = "{Custom Collection Name}"
    $destinationCredentialName = "{Destination Credential Name}"
    $destinationServerName = "{Destination Server Name}"
    $destinationDatabaseName = "{Destination Database Name}"
    $destinationSchemaName = "{Destination Schema Name}"
    $destinationTableName = "{Destination Table Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName

### <a name="to-create-and-start-a-job-for-data-collection-scenarios"></a><span data-ttu-id="c7ba6-331">Hozzon létre, és elindíthat egy feladatot a adatáttelepítések gyűjtemény esetében</span><span class="sxs-lookup"><span data-stu-id="c7ba6-331">To create and start a job for data collection scenarios</span></span>
<span data-ttu-id="c7ba6-332">A parancsfájl a [ **Start-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/start-azuresqljobexecution) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-332">This script uses the [**Start-AzureSqlJobExecution**](/powershell/module/elasticdatabasejobs/start-azuresqljobexecution) cmdlet.</span></span>

    $job = New-AzureSqlJob -JobName $jobName 
    -CredentialName $executionCredentialName 
    -ContentName $scriptName 
    -ResultSetDestinationServerName $destinationServerName 
    -ResultSetDestinationDatabaseName $destinationDatabaseName 
    -ResultSetDestinationSchemaName $destinationSchemaName 
    -ResultSetDestinationTableName $destinationTableName 
    -ResultSetDestinationCredentialName $destinationCredentialName 
    -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution

## <a name="to-schedule-a-job-execution-trigger"></a><span data-ttu-id="c7ba6-333">A feladat végrehajtási eseményindító ütemezése</span><span class="sxs-lookup"><span data-stu-id="c7ba6-333">To schedule a job execution trigger</span></span>
<span data-ttu-id="c7ba6-334">A következő PowerShell-parancsfájl segítségével hozzon létre egy ismétlődő ütemezés szerint.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-334">The following PowerShell script can be used to create a recurring schedule.</span></span> <span data-ttu-id="c7ba6-335">A parancsfájl a perces időközt, de [ **New-AzureSqlJobSchedule** ](/powershell/module/elasticdatabasejobs/new-azuresqljobschedule) - DayInterval, - HourInterval, - MonthInterval, és - WeekInterval paramétereket is támogatja.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-335">This script uses a minute interval, but [**New-AzureSqlJobSchedule**](/powershell/module/elasticdatabasejobs/new-azuresqljobschedule) also supports -DayInterval, -HourInterval, -MonthInterval, and -WeekInterval parameters.</span></span> <span data-ttu-id="c7ba6-336">Csak egyszer hajtható végre ütemezéseket is létrehozható, hogy - alkalommal.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-336">Schedules that execute only once can be created by passing -OneTime.</span></span>

<span data-ttu-id="c7ba6-337">Új ütemezés létrehozása:</span><span class="sxs-lookup"><span data-stu-id="c7ba6-337">Create a new schedule:</span></span>

    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule 
    -MinuteInterval $minuteInterval 
    -ScheduleName $scheduleName 
    -StartTime $startTime 
    Write-Output $schedule

### <a name="to-trigger-a-job-executed-on-a-time-schedule"></a><span data-ttu-id="c7ba6-338">Elindítható egy feladat végrehajtása ütemezéssel</span><span class="sxs-lookup"><span data-stu-id="c7ba6-338">To trigger a job executed on a time schedule</span></span>
<span data-ttu-id="c7ba6-339">Egy feladat eseményindító szeretné, hogy a feladat végrehajtása idő ütemezés szerint lehet megadni.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-339">A job trigger can be defined to have a job executed according to a time schedule.</span></span> <span data-ttu-id="c7ba6-340">A következő PowerShell-parancsfájl segítségével hozzon létre egy feladat eseményindító.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-340">The following PowerShell script can be used to create a job trigger.</span></span>

<span data-ttu-id="c7ba6-341">Használjon [New-AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/new-azuresqljobtrigger) és a következő változókat, hogy a kívánt feladat és ütemezése:</span><span class="sxs-lookup"><span data-stu-id="c7ba6-341">Use [New-AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/new-azuresqljobtrigger) and set the following variables to correspond to the desired job and schedule:</span></span>

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger
    -ScheduleName $scheduleName
    -JobName $jobName
    Write-Output $jobTrigger

### <a name="to-remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a><span data-ttu-id="c7ba6-342">A feladat ütemezés futtatásának leállítása ütemezett társításának megszüntetése</span><span class="sxs-lookup"><span data-stu-id="c7ba6-342">To remove a scheduled association to stop job from executing on schedule</span></span>
<span data-ttu-id="c7ba6-343">Megszüntetheti a feladatról feladat végrehajtása a feladat indítási keresztül, a feladat indítási távolíthatja el.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-343">To discontinue reoccurring job execution through a job trigger, the job trigger can be removed.</span></span> <span data-ttu-id="c7ba6-344">Távolítsa el a feladat eseményindító megfelelően egy ütemezés használatával végrehajtott egy feladatot leállítja a [ **Remove-AzureSqlJobTrigger parancsmag**](/powershell/module/elasticdatabasejobs/remove-azuresqljobtrigger).</span><span class="sxs-lookup"><span data-stu-id="c7ba6-344">Remove a job trigger to stop a job from being executed according to a schedule using the [**Remove-AzureSqlJobTrigger cmdlet**](/powershell/module/elasticdatabasejobs/remove-azuresqljobtrigger).</span></span>

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger 
    -ScheduleName $scheduleName 
    -JobName $jobName

### <a name="retrieve-job-triggers-bound-to-a-time-schedule"></a><span data-ttu-id="c7ba6-345">Feladat eseményindítók ütemezést kötve beolvasása</span><span class="sxs-lookup"><span data-stu-id="c7ba6-345">Retrieve job triggers bound to a time schedule</span></span>
<span data-ttu-id="c7ba6-346">A következő PowerShell-parancsfájl segítségével beszerzése és megjelenítése a feladat eseményindítók adott ütemezést regisztrálva.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-346">The following PowerShell script can be used to obtain and display the job triggers registered to a particular time schedule.</span></span>

    $scheduleName = "{Schedule Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -ScheduleName $scheduleName
    Write-Output $jobTriggers

### <a name="to-retrieve-job-triggers-bound-to-a-job"></a><span data-ttu-id="c7ba6-347">Egy feladat kötött feladat eseményindítók beolvasása</span><span class="sxs-lookup"><span data-stu-id="c7ba6-347">To retrieve job triggers bound to a job</span></span>
<span data-ttu-id="c7ba6-348">Használjon [Get-AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/get-azuresqljobtrigger) beszerzése és tartalmazó regisztrált feladat ütemezésének megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-348">Use [Get-AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/get-azuresqljobtrigger) to obtain and display schedules containing a registered job.</span></span>

    $jobName = "{Job Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -JobName $jobName
    Write-Output $jobTriggers

## <a name="to-create-a-data-tier-application-dacpac-for-execution-across-databases"></a><span data-ttu-id="c7ba6-349">Az adatbázisok közötti végrehajtásra adatrétegbeli alkalmazás (DACPAC) létrehozása</span><span class="sxs-lookup"><span data-stu-id="c7ba6-349">To create a data-tier application (DACPAC) for execution across databases</span></span>
<span data-ttu-id="c7ba6-350">Egy DACPAC létrehozásához lásd: [Adatrétegbeli alkalmazások](https://msdn.microsoft.com/library/ee210546.aspx).</span><span class="sxs-lookup"><span data-stu-id="c7ba6-350">To create a DACPAC, see [Data-Tier applications](https://msdn.microsoft.com/library/ee210546.aspx).</span></span> <span data-ttu-id="c7ba6-351">Egy DACPAC telepítéséhez használja a [New-AzureSqlJobContent parancsmag](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent).</span><span class="sxs-lookup"><span data-stu-id="c7ba6-351">To deploy a DACPAC, use the [New-AzureSqlJobContent cmdlet](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent).</span></span> <span data-ttu-id="c7ba6-352">A DACPAC a szolgáltatás elérhetőnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-352">The DACPAC must be accessible to the service.</span></span> <span data-ttu-id="c7ba6-353">Javasoljuk, hogy a létrehozott DACPAC feltöltése az Azure Storage, és hozzon létre egy [közös hozzáférésű Jogosultságkód](../storage/common/storage-dotnet-shared-access-signature-part-1.md) a DACPAC számára.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-353">It is recommended to upload a created DACPAC to Azure Storage and create a [Shared Access Signature](../storage/common/storage-dotnet-shared-access-signature-part-1.md) for the DACPAC.</span></span>

    $dacpacUri = "{Uri}"
    $dacpacName = "{Dacpac Name}"
    $dacpac = New-AzureSqlJobContent -DacpacUri $dacpacUri -ContentName $dacpacName 
    Write-Output $dacpac

### <a name="to-update-a-data-tier-application-dacpac-for-execution-across-databases"></a><span data-ttu-id="c7ba6-354">Az adatbázisok közötti egy adatrétegbeli alkalmazás (DACPAC) végrehajtásra frissítése</span><span class="sxs-lookup"><span data-stu-id="c7ba6-354">To update a data-tier application (DACPAC) for execution across databases</span></span>
<span data-ttu-id="c7ba6-355">Rugalmas adatbázis-feladatok belül regisztrálva meglévő DACPACs frissíthető új URI mutasson.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-355">Existing DACPACs registered within Elastic Database Jobs can be updated to point to new URIs.</span></span> <span data-ttu-id="c7ba6-356">Használja a [ **Set-AzureSqlJobContentDefinition parancsmag** ](/powershell/module/elasticdatabasejobs/set-azuresqljobcontentdefinition) frissítésére a meglévő DACPAC URI regisztrált DACPAC:</span><span class="sxs-lookup"><span data-stu-id="c7ba6-356">Use the [**Set-AzureSqlJobContentDefinition cmdlet**](/powershell/module/elasticdatabasejobs/set-azuresqljobcontentdefinition) to update the DACPAC URI on an existing registered DACPAC:</span></span>

    $dacpacName = "{Dacpac Name}"
    $newDacpacUri = "{Uri}"
    $updatedDacpac = Set-AzureSqlJobDacpacDefinition -ContentName $dacpacName -DacpacUri $newDacpacUri
    Write-Output $updatedDacpac

## <a name="to-create-a-job-to-apply-a-data-tier-application-dacpac-across-databases"></a><span data-ttu-id="c7ba6-357">Az adatbázisok közötti egy adatrétegbeli alkalmazás (DACPAC) alkalmazandó feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="c7ba6-357">To create a job to apply a data-tier application (DACPAC) across databases</span></span>
<span data-ttu-id="c7ba6-358">Miután egy DACPAC rugalmas adatbázis-feladatok belül létrehozott, egy feladat a DACPAC alkalmazhatók a adatbázisok csoportja is létrehozható.</span><span class="sxs-lookup"><span data-stu-id="c7ba6-358">After a DACPAC has been created within Elastic Database Jobs, a job can be created to apply the DACPAC across a group of databases.</span></span> <span data-ttu-id="c7ba6-359">A következő PowerShell-parancsfájl segítségével hozzon létre egy DACPAC feladatot egy egyéni gyűjtéshez. az adatbázisok között:</span><span class="sxs-lookup"><span data-stu-id="c7ba6-359">The following PowerShell script can be used to create a DACPAC job across a custom collection of databases:</span></span>

    $jobName = "{Job Name}"
    $dacpacName = "{Dacpac Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget 
    -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob 
    -JobName $jobName 
    -CredentialName $credentialName 
    -ContentName $dacpacName -TargetId $target.TargetId
    Write-Output $job 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-powershell/cmd-prompt.png
[2]: ./media/sql-database-elastic-jobs-powershell/portal.png
<!--anchors-->
