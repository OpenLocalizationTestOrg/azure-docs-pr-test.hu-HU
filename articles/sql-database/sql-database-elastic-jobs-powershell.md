---
title: "aaaCreate és a PowerShell segítségével a rugalmas feladatok kezelése |} Microsoft Docs"
description: "PowerShell használt toomanage Azure SQL Database-készletek"
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
ms.openlocfilehash: f6c18aecfa7e8c0b102a3b7cd2f266f5542ae400
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-sql-database-elastic-jobs-using-powershell-preview"></a><span data-ttu-id="679b1-103">SQL Database PowerShell (előzetes verzió) segítségével a rugalmas feladatok létrehozásához és kezeléséhez</span><span class="sxs-lookup"><span data-stu-id="679b1-103">Create and manage SQL Database elastic jobs using PowerShell (preview)</span></span>

<span data-ttu-id="679b1-104">hello PowerShell API-k a **rugalmas adatbázis-feladatok** (az előzetes verzió), lehetővé teszik, hogy olyan adatbázisok, amely végrehajtja a parancsfájlok csoportját.</span><span class="sxs-lookup"><span data-stu-id="679b1-104">hello PowerShell APIs for **Elastic Database jobs** (in preview), let you define a group of databases against which scripts will execute.</span></span> <span data-ttu-id="679b1-105">Ez a cikk bemutatja, hogyan toocreate és kezelése **rugalmas adatbázis-feladatok** PowerShell-parancsmagok használatával.</span><span class="sxs-lookup"><span data-stu-id="679b1-105">This article shows how toocreate and manage **Elastic Database jobs** using PowerShell cmdlets.</span></span> <span data-ttu-id="679b1-106">Lásd: [rugalmas feladatok áttekintése](sql-database-elastic-jobs-overview.md).</span><span class="sxs-lookup"><span data-stu-id="679b1-106">See [Elastic jobs overview](sql-database-elastic-jobs-overview.md).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="679b1-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="679b1-107">Prerequisites</span></span>
* <span data-ttu-id="679b1-108">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="679b1-108">An Azure subscription.</span></span> <span data-ttu-id="679b1-109">Ingyenes próbaverzió, lásd: [ingyenes egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="679b1-109">For a free trial, see [Free one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="679b1-110">Hello rugalmas adatbázis eszközzel létrehozott adatbázisok készleteit.</span><span class="sxs-lookup"><span data-stu-id="679b1-110">A set of databases created with hello Elastic Database tools.</span></span> <span data-ttu-id="679b1-111">Lásd: [Ismerkedés a rugalmas adatbáziseszközöket](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="679b1-111">See [Get started with Elastic Database tools](sql-database-elastic-scale-get-started.md).</span></span>
* <span data-ttu-id="679b1-112">Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="679b1-112">Azure PowerShell.</span></span> <span data-ttu-id="679b1-113">Részletes információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="679b1-113">For detailed information, see [How tooinstall and configure Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span></span>
* <span data-ttu-id="679b1-114">**Rugalmas adatbázis-feladatok** PowerShell csomag: lásd: [telepítése rugalmas adatbázis-feladatok](sql-database-elastic-jobs-service-installation.md)</span><span class="sxs-lookup"><span data-stu-id="679b1-114">**Elastic Database jobs** PowerShell package: See [Installing Elastic Database jobs](sql-database-elastic-jobs-service-installation.md)</span></span>

### <a name="select-your-azure-subscription"></a><span data-ttu-id="679b1-115">Válassza ki az Azure-előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="679b1-115">Select your Azure subscription</span></span>
<span data-ttu-id="679b1-116">az előfizetés-azonosítót kell tooselect hello előfizetés (**- SubscriptionId**) vagy előfizetés neve (**- SubscriptionName**).</span><span class="sxs-lookup"><span data-stu-id="679b1-116">tooselect hello subscription you need your subscription Id (**-SubscriptionId**) or subscription name (**-SubscriptionName**).</span></span> <span data-ttu-id="679b1-117">Ha több előfizetéssel rendelkezik futtatása hello **Get-AzureRmSubscription** parancsmag és a példány hello szükséges hello eredményhalmazából előfizetési adatokat.</span><span class="sxs-lookup"><span data-stu-id="679b1-117">If you have multiple subscriptions you can run hello **Get-AzureRmSubscription** cmdlet and copy hello desired subscription information from hello result set.</span></span> <span data-ttu-id="679b1-118">Miután az előfizetési adatai, futtassa a következő parancsmag tooset hello ehhez az előfizetéshez hello alapértelmezett, nevezetesen a cél a feladatok létrehozását és kezelését hello:</span><span class="sxs-lookup"><span data-stu-id="679b1-118">Once you have your subscription information, run hello following commandlet tooset this subscription as hello default, namely hello target for creating and managing jobs:</span></span>

    Select-AzureRmSubscription -SubscriptionId {SubscriptionID}

<span data-ttu-id="679b1-119">Hello [PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx) használati toodevelop ajánlott, és a PowerShell-parancsfájlok elleni hello rugalmas adatbázis-feladatok végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="679b1-119">hello [PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx) is recommended for usage toodevelop and execute PowerShell scripts against hello Elastic Database jobs.</span></span>

## <a name="elastic-database-jobs-objects"></a><span data-ttu-id="679b1-120">A rugalmas adatbázis-feladatok objektumok</span><span class="sxs-lookup"><span data-stu-id="679b1-120">Elastic Database jobs objects</span></span>
<span data-ttu-id="679b1-121">a következő táblázat fel minden hello objektum típusú hello **rugalmas adatbázis-feladatok** együtt a leírása és a megfelelő PowerShell API-k.</span><span class="sxs-lookup"><span data-stu-id="679b1-121">hello following table lists out all hello object types of **Elastic Database jobs** along with its description and relevant PowerShell APIs.</span></span>

<table style="width:100%">
  <tr>
    <th><span data-ttu-id="679b1-122">Objektumtípus</span><span class="sxs-lookup"><span data-stu-id="679b1-122">Object Type</span></span></th>
    <th><span data-ttu-id="679b1-123">Leírás</span><span class="sxs-lookup"><span data-stu-id="679b1-123">Description</span></span></th>
    <th><span data-ttu-id="679b1-124">Kapcsolódó PowerShell API-k</span><span class="sxs-lookup"><span data-stu-id="679b1-124">Related PowerShell APIs</span></span></th>
  </tr>
  <tr>
    <td><span data-ttu-id="679b1-125">Hitelesítő adat</span><span class="sxs-lookup"><span data-stu-id="679b1-125">Credential</span></span></td>
    <td><span data-ttu-id="679b1-126">Felhasználónév és jelszó toouse parancsprogramok végrehajtását vagy DACPACs alkalmazásának toodatabases kapcsolódáskor.</span><span class="sxs-lookup"><span data-stu-id="679b1-126">Username and password toouse when connecting toodatabases for execution of scripts or application of DACPACs.</span></span> <p><span data-ttu-id="679b1-127">hello jelszó titkosított hello rugalmas adatbázis-feladatok adatbázis tárolási tooand elküldése előtt.</span><span class="sxs-lookup"><span data-stu-id="679b1-127">hello password is encrypted before sending tooand storing in hello Elastic Database Jobs database.</span></span>  <span data-ttu-id="679b1-128">hello jelszó visszafejti hello rugalmas adatbázis-feladatok szolgáltatás hello hitelesítő adat létrehozása és fel kell tölteni a hello telepítési parancsfájl használatával.</span><span class="sxs-lookup"><span data-stu-id="679b1-128">hello password is decrypted by hello Elastic Database Jobs service via hello credential created and uploaded from hello installation script.</span></span></td>
    <td><p><span data-ttu-id="679b1-129">Get-AzureSqlJobCredential</span><span class="sxs-lookup"><span data-stu-id="679b1-129">Get-AzureSqlJobCredential</span></span></p>
    <p><span data-ttu-id="679b1-130">Új AzureSqlJobCredential</span><span class="sxs-lookup"><span data-stu-id="679b1-130">New-AzureSqlJobCredential</span></span></p><p><span data-ttu-id="679b1-131">Set-AzureSqlJobCredential</span><span class="sxs-lookup"><span data-stu-id="679b1-131">Set-AzureSqlJobCredential</span></span></p></td></td>
  </tr>

  <tr>
    <td><span data-ttu-id="679b1-132">Szkript</span><span class="sxs-lookup"><span data-stu-id="679b1-132">Script</span></span></td>
    <td><span data-ttu-id="679b1-133">Transact-SQL parancsfájl toobe az adatbázisok közötti végrehajtási használatos.</span><span class="sxs-lookup"><span data-stu-id="679b1-133">Transact-SQL script toobe used for execution across databases.</span></span>  <span data-ttu-id="679b1-134">hello parancsfájl kell lennie a szerzői toobe idempotent, mivel hello szolgáltatás újra próbálkozik a hibák után hello parancsprogram végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="679b1-134">hello script should be authored toobe idempotent since hello service will retry execution of hello script upon failures.</span></span>
    </td>
    <td>
    <p><span data-ttu-id="679b1-135">Get-AzureSqlJobContent</span><span class="sxs-lookup"><span data-stu-id="679b1-135">Get-AzureSqlJobContent</span></span></p>
    <p><span data-ttu-id="679b1-136">Get-AzureSqlJobContentDefinition</span><span class="sxs-lookup"><span data-stu-id="679b1-136">Get-AzureSqlJobContentDefinition</span></span></p>
    <p><span data-ttu-id="679b1-137">Új AzureSqlJobContent</span><span class="sxs-lookup"><span data-stu-id="679b1-137">New-AzureSqlJobContent</span></span></p>
    <p><span data-ttu-id="679b1-138">Set-AzureSqlJobContentDefinition</span><span class="sxs-lookup"><span data-stu-id="679b1-138">Set-AzureSqlJobContentDefinition</span></span></p>
    </td>
  </tr>

  <tr>
    <td><span data-ttu-id="679b1-139">DACPAC</span><span class="sxs-lookup"><span data-stu-id="679b1-139">DACPAC</span></span></td>
    <td><span data-ttu-id="679b1-140"><a href="https://msdn.microsoft.com/library/ee210546.aspx">Adatrétegbeli alkalmazás </a> alkalmazza az adatbázisok közötti toobe csomag.</span><span class="sxs-lookup"><span data-stu-id="679b1-140"><a href="https://msdn.microsoft.com/library/ee210546.aspx">Data-tier application </a> package toobe applied across databases.</span></span>

    </td>
    <td>
    <p><span data-ttu-id="679b1-141">Get-AzureSqlJobContent</span><span class="sxs-lookup"><span data-stu-id="679b1-141">Get-AzureSqlJobContent</span></span></p>
    <p><span data-ttu-id="679b1-142">Új AzureSqlJobContent</span><span class="sxs-lookup"><span data-stu-id="679b1-142">New-AzureSqlJobContent</span></span></p>
    <p><span data-ttu-id="679b1-143">Set-AzureSqlJobContentDefinition</span><span class="sxs-lookup"><span data-stu-id="679b1-143">Set-AzureSqlJobContentDefinition</span></span></p>
    </td>
  </tr>
  <tr>
    <td><span data-ttu-id="679b1-144">Adatbázis-cél</span><span class="sxs-lookup"><span data-stu-id="679b1-144">Database Target</span></span></td>
    <td><span data-ttu-id="679b1-145">Adatbázis és a kiszolgáló neve mutató tooan Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="679b1-145">Database and server name pointing tooan Azure SQL Database.</span></span>

    </td>
    <td>
    <p><span data-ttu-id="679b1-146">Get-AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="679b1-146">Get-AzureSqlJobTarget</span></span></p>
    <p><span data-ttu-id="679b1-147">Új AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="679b1-147">New-AzureSqlJobTarget</span></span></p>
    </td>
  </tr>
  <tr>
    <td><span data-ttu-id="679b1-148">A shard térkép cél</span><span class="sxs-lookup"><span data-stu-id="679b1-148">Shard Map Target</span></span></td>
    <td><span data-ttu-id="679b1-149">Egy adatbázis cél és a hitelesítő adatok toobe használt toodetermine adatok egy rugalmas adatbázist shard leképezés tárolja.</span><span class="sxs-lookup"><span data-stu-id="679b1-149">Combination of a database target and a credential toobe used toodetermine information stored within an Elastic Database shard map.</span></span>
    </td>
    <td>
    <p><span data-ttu-id="679b1-150">Get-AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="679b1-150">Get-AzureSqlJobTarget</span></span></p>
    <p><span data-ttu-id="679b1-151">Új AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="679b1-151">New-AzureSqlJobTarget</span></span></p>
    <p><span data-ttu-id="679b1-152">Set-AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="679b1-152">Set-AzureSqlJobTarget</span></span></p>
    </td>
  </tr>
<tr>
    <td><span data-ttu-id="679b1-153">Egyéni gyűjtemény célja</span><span class="sxs-lookup"><span data-stu-id="679b1-153">Custom Collection Target</span></span></td>
    <td><span data-ttu-id="679b1-154">Adatbázisok toocollectively meghatározott csoportjára használja a végrehajtáshoz.</span><span class="sxs-lookup"><span data-stu-id="679b1-154">Defined group of databases toocollectively use for execution.</span></span></td>
    <td>
    <p><span data-ttu-id="679b1-155">Get-AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="679b1-155">Get-AzureSqlJobTarget</span></span></p>
    <p><span data-ttu-id="679b1-156">Új AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="679b1-156">New-AzureSqlJobTarget</span></span></p>
    </td>
  </tr>
<tr>
    <td><span data-ttu-id="679b1-157">Egyéni gyűjtemény gyermek cél</span><span class="sxs-lookup"><span data-stu-id="679b1-157">Custom Collection Child Target</span></span></td>
    <td><span data-ttu-id="679b1-158">Egy egyéni gyűjteményből hivatkozott adatbázis cél.</span><span class="sxs-lookup"><span data-stu-id="679b1-158">Database target that is referenced from a custom collection.</span></span></td>
    <td>
    <p><span data-ttu-id="679b1-159">Adja hozzá AzureSqlJobChildTarget</span><span class="sxs-lookup"><span data-stu-id="679b1-159">Add-AzureSqlJobChildTarget</span></span></p>
    <p><span data-ttu-id="679b1-160">Remove-AzureSqlJobChildTarget</span><span class="sxs-lookup"><span data-stu-id="679b1-160">Remove-AzureSqlJobChildTarget</span></span></p>
    </td>
  </tr>

<tr>
    <td><span data-ttu-id="679b1-161">Feladat</span><span class="sxs-lookup"><span data-stu-id="679b1-161">Job</span></span></td>
    <td>
    <p><span data-ttu-id="679b1-162">Paraméterek használt tootrigger végrehajtási vagy toofulfill ütemezett feladat meghatározását.</span><span class="sxs-lookup"><span data-stu-id="679b1-162">Definition of parameters for a job that can be used tootrigger execution or toofulfill a schedule.</span></span></p>
    </td>
    <td>
    <p><span data-ttu-id="679b1-163">Get-AzureSqlJob</span><span class="sxs-lookup"><span data-stu-id="679b1-163">Get-AzureSqlJob</span></span></p>
    <p><span data-ttu-id="679b1-164">Új AzureSqlJob</span><span class="sxs-lookup"><span data-stu-id="679b1-164">New-AzureSqlJob</span></span></p>
    <p><span data-ttu-id="679b1-165">Set-AzureSqlJob</span><span class="sxs-lookup"><span data-stu-id="679b1-165">Set-AzureSqlJob</span></span></p>
    </td>
  </tr>

<tr>
    <td><span data-ttu-id="679b1-166">Feladat végrehajtása</span><span class="sxs-lookup"><span data-stu-id="679b1-166">Job Execution</span></span></td>
    <td>
    <p><span data-ttu-id="679b1-167">Tároló szükséges toofulfill tevékenységek a parancsfájl végrehajtása vagy a hitelesítő adatok használatával az adatbázis-kapcsolat hibákkal DACPAC tooa target alkalmazása kezelje a megfelelően tooan végrehajtási házirend.</span><span class="sxs-lookup"><span data-stu-id="679b1-167">Container of tasks necessary toofulfill either executing a script or applying a DACPAC tooa target using credentials for database connections with failures handled in accordance tooan execution policy.</span></span></p>
    </td>
    <td>
    <p><span data-ttu-id="679b1-168">Get-AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="679b1-168">Get-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="679b1-169">Start-AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="679b1-169">Start-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="679b1-170">STOP-AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="679b1-170">Stop-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="679b1-171">Várjon, amíg-AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="679b1-171">Wait-AzureSqlJobExecution</span></span></p>
  </tr>

<tr>
    <td><span data-ttu-id="679b1-172">A feladat a végrehajtás feladat</span><span class="sxs-lookup"><span data-stu-id="679b1-172">Job Task Execution</span></span></td>
    <td>
    <p><span data-ttu-id="679b1-173">Munkahelyi toofulfill egy feladatot egyetlen egységként.</span><span class="sxs-lookup"><span data-stu-id="679b1-173">Single unit of work toofulfill a job.</span></span></p>
    <p><span data-ttu-id="679b1-174">Ha egy feladat nem tud toosuccessfully hajtható végre, hello eredményül kapott Kivételüzenet naplózza a rendszer és egy új egyező feladata létrejön és hajtotta végre a megadott összhangban toohello végrehajtási házirendet.</span><span class="sxs-lookup"><span data-stu-id="679b1-174">If a job task is not able toosuccessfully execute, hello resulting exception message will be logged and a new matching job task will be created and executed in accordance toohello specified execution policy.</span></span></p></p>
    </td>
    <td>
    <p><span data-ttu-id="679b1-175">Get-AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="679b1-175">Get-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="679b1-176">Start-AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="679b1-176">Start-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="679b1-177">STOP-AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="679b1-177">Stop-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="679b1-178">Várjon, amíg-AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="679b1-178">Wait-AzureSqlJobExecution</span></span></p>
  </tr>

<tr>
    <td><span data-ttu-id="679b1-179">Feladat-végrehajtási házirend</span><span class="sxs-lookup"><span data-stu-id="679b1-179">Job Execution Policy</span></span></td>
    <td>
    <p><span data-ttu-id="679b1-180">Vezérlők sikertelen feladat-végrehajtási időtúllépés, a újrapróbálkozási korlátozások és a próbálkozások közötti időintervallumot.</span><span class="sxs-lookup"><span data-stu-id="679b1-180">Controls job execution timeouts, retry limits and intervals between retries.</span></span></p>
    <p><span data-ttu-id="679b1-181">Rugalmas adatbázis-feladatok tartalmaz egy alapértelmezett feladat végrehajtási házirendet, amely a feladat sikertelen feladatok gyakorlatilag végtelen újrapróbálkozások okozhat a exponenciális leállítási az intervallumok között minden próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="679b1-181">Elastic Database jobs includes a default job execution policy which cause essentially infinite retries of job task failures with exponential backoff of intervals between each retry.</span></span></p>
    </td>
    <td>
    <p><span data-ttu-id="679b1-182">Get-AzureSqlJobExecutionPolicy</span><span class="sxs-lookup"><span data-stu-id="679b1-182">Get-AzureSqlJobExecutionPolicy</span></span></p>
    <p><span data-ttu-id="679b1-183">Új AzureSqlJobExecutionPolicy</span><span class="sxs-lookup"><span data-stu-id="679b1-183">New-AzureSqlJobExecutionPolicy</span></span></p>
    <p><span data-ttu-id="679b1-184">Set-AzureSqlJobExecutionPolicy</span><span class="sxs-lookup"><span data-stu-id="679b1-184">Set-AzureSqlJobExecutionPolicy</span></span></p>
    </td>
  </tr>

<tr>
    <td><span data-ttu-id="679b1-185">Ütemezés</span><span class="sxs-lookup"><span data-stu-id="679b1-185">Schedule</span></span></td>
    <td>
    <p><span data-ttu-id="679b1-186">Ideje végrehajtási tootake hely feladatról intervallumban vagy egyetlen egyszerre specifikáció alapján.</span><span class="sxs-lookup"><span data-stu-id="679b1-186">Time based specification for execution tootake place either on a reoccurring interval or at a single time.</span></span></p>
    </td>
    <td>
    <p><span data-ttu-id="679b1-187">Get-AzureSqlJobSchedule</span><span class="sxs-lookup"><span data-stu-id="679b1-187">Get-AzureSqlJobSchedule</span></span></p>
    <p><span data-ttu-id="679b1-188">Új AzureSqlJobSchedule</span><span class="sxs-lookup"><span data-stu-id="679b1-188">New-AzureSqlJobSchedule</span></span></p>
    <p><span data-ttu-id="679b1-189">Set-AzureSqlJobSchedule</span><span class="sxs-lookup"><span data-stu-id="679b1-189">Set-AzureSqlJobSchedule</span></span></p>
    </td>
  </tr>

<tr>
    <td><span data-ttu-id="679b1-190">Feladat indítja el</span><span class="sxs-lookup"><span data-stu-id="679b1-190">Job Triggers</span></span></td>
    <td>
    <p><span data-ttu-id="679b1-191">Egy feladat és egy ütemezés tootrigger feladat végrehajtása toohello ütemezés szerint közötti leképezést.</span><span class="sxs-lookup"><span data-stu-id="679b1-191">A mapping between a job and a schedule tootrigger job execution according toohello schedule.</span></span></p>
    </td>
    <td>
    <p><span data-ttu-id="679b1-192">Új AzureSqlJobTrigger</span><span class="sxs-lookup"><span data-stu-id="679b1-192">New-AzureSqlJobTrigger</span></span></p>
    <p><span data-ttu-id="679b1-193">Remove-AzureSqlJobTrigger</span><span class="sxs-lookup"><span data-stu-id="679b1-193">Remove-AzureSqlJobTrigger</span></span></p>
    </td>
  </tr>
</table>

## <a name="supported-elastic-database-jobs-group-types"></a><span data-ttu-id="679b1-194">Támogatott rugalmas adatbázis-feladatok csoportban típusok</span><span class="sxs-lookup"><span data-stu-id="679b1-194">Supported Elastic Database jobs group types</span></span>
<span data-ttu-id="679b1-195">hello feladat végrehajtja a Transact-SQL (T-SQL) parancsfájlok vagy DACPACs alkalmazásának adatbázisok csoportja között.</span><span class="sxs-lookup"><span data-stu-id="679b1-195">hello job executes Transact-SQL (T-SQL) scripts or application of DACPACs across a group of databases.</span></span> <span data-ttu-id="679b1-196">Ha egy feladat végrehajtása között elküldött toobe egy csoportot az adatbázisok, hello feladat "kibontja" hello gyermek feladatok, ahol minden egyes végez hello kért végrehajtási hello csoport egyetlen adatbázis.</span><span class="sxs-lookup"><span data-stu-id="679b1-196">When a job is submitted toobe executed across a group of databases, hello job “expands” hello into child jobs where each performs hello requested execution against a single database in hello group.</span></span> 

<span data-ttu-id="679b1-197">A csoportokat, amelyek hozhat létre két típusa van:</span><span class="sxs-lookup"><span data-stu-id="679b1-197">There are two types of groups that you can create:</span></span> 

* <span data-ttu-id="679b1-198">[A shard térkép](sql-database-elastic-scale-shard-map-management.md) csoport: Ha egy feladat elküldött tootarget shard térképet, hello feladat hello shard térkép toodetermine lekérdezi az aktuális készletében lévő szilánkok, és majd létrehoz gyermek minden shard feladataihoz hello shard leképezés.</span><span class="sxs-lookup"><span data-stu-id="679b1-198">[Shard Map](sql-database-elastic-scale-shard-map-management.md) group: When a job is submitted tootarget a shard map, hello job queries hello shard map toodetermine its current set of shards, and then creates child jobs for each shard in hello shard map.</span></span>
* <span data-ttu-id="679b1-199">Egyéni gyűjtési csoportjának: egy egyénileg definiált adatbázisok készleteit.</span><span class="sxs-lookup"><span data-stu-id="679b1-199">Custom Collection group: A custom defined set of databases.</span></span> <span data-ttu-id="679b1-200">Ha egy feladat egyéni gyűjtemény célozza, hozna létre gyermek feladatokat az egyes adatbázisok jelenleg hello egyéni gyűjtéshez.</span><span class="sxs-lookup"><span data-stu-id="679b1-200">When a job targets a custom collection, it creates child jobs for each database currently in hello custom collection.</span></span>

## <a name="tooset-hello-elastic-database-jobs-connection"></a><span data-ttu-id="679b1-201">tooset hello rugalmas adatbázis-feladatok kapcsolat</span><span class="sxs-lookup"><span data-stu-id="679b1-201">tooset hello Elastic Database jobs connection</span></span>
<span data-ttu-id="679b1-202">A kapcsolat kell toohello feladatok toobe beállítása *feladatvezérlő adatbázishoz* előzetes toousing hello feladatok API-k.</span><span class="sxs-lookup"><span data-stu-id="679b1-202">A connection needs toobe set toohello jobs *control database* prior toousing hello jobs APIs.</span></span> <span data-ttu-id="679b1-203">A hitelesítő adatok ablak toopop fel a kért hello felhasználónevet és jelszót a rugalmas adatbázis-feladatok telepítésekor létrehozott futtatja ezt a parancsmagot váltja ki.</span><span class="sxs-lookup"><span data-stu-id="679b1-203">Running this cmdlet triggers a credential window toopop up requesting hello user name and password created when installing Elastic Database jobs.</span></span> <span data-ttu-id="679b1-204">Ebben a témakörben közölt összes példák feltételezik, hogy az első lépéshez már végbementek.</span><span class="sxs-lookup"><span data-stu-id="679b1-204">All examples provided within this topic assume that this first step has already been performed.</span></span>

<span data-ttu-id="679b1-205">Nyissa meg a kapcsolat toohello rugalmas feladatok:</span><span class="sxs-lookup"><span data-stu-id="679b1-205">Open a connection toohello Elastic Database jobs:</span></span>

    Use-AzureSqlJobConnection -CurrentAzureSubscription 

## <a name="encrypted-credentials-within-hello-elastic-database-jobs"></a><span data-ttu-id="679b1-206">Hello rugalmas adatbázis-feladatok belül titkosított hitelesítő adatokat</span><span class="sxs-lookup"><span data-stu-id="679b1-206">Encrypted credentials within hello Elastic Database jobs</span></span>
<span data-ttu-id="679b1-207">Adatbázis-hitelesítő adatok szúrhatók be hello feladatok *feladatvezérlő adatbázishoz* a jelszóval titkosított.</span><span class="sxs-lookup"><span data-stu-id="679b1-207">Database credentials can be inserted into hello jobs *control database*  with its password encrypted.</span></span> <span data-ttu-id="679b1-208">Akkor szükséges toostore hitelesítő adatok tooenable feladatok toobe egy későbbi időpontban végre (a feladatok ütemezésének használatával).</span><span class="sxs-lookup"><span data-stu-id="679b1-208">It is necessary toostore credentials tooenable jobs toobe executed at a later time, (using job schedules).</span></span>

<span data-ttu-id="679b1-209">Titkosítási működik keresztül hello telepítési parancsfájl részeként létrehozott tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="679b1-209">Encryption works through a certificate created as part of hello installation script.</span></span> <span data-ttu-id="679b1-210">hello telepítési parancsfájlja létrehozza, és az Azure Cloud Service hello hello visszafejtése a feltöltések hello tanúsítvány titkosított jelszavak.</span><span class="sxs-lookup"><span data-stu-id="679b1-210">hello installation script creates and uploads hello certificate into hello Azure Cloud Service for decryption of hello stored encrypted passwords.</span></span> <span data-ttu-id="679b1-211">Azure Cloud Service hello később hello nyilvános kulcsot hello feladatok belül tárolja *feladatvezérlő adatbázishoz* amely hello PowerShell API-t vagy a klasszikus Azure portál felület tooencrypt megadott jelszóval lehetővé anélkül, hogy hello tanúsítvány helyileg telepített toobe.</span><span class="sxs-lookup"><span data-stu-id="679b1-211">hello Azure Cloud Service later stores hello public key within hello jobs *control database* which enables hello PowerShell API or Azure Classic Portal interface tooencrypt a provided password without requiring hello certificate toobe locally installed.</span></span>

<span data-ttu-id="679b1-212">titkosított és védett tooElastic adatbázis-feladatok objektumok csak olvasási hozzáféréssel rendelkező felhasználókat a rendszer a hello hitelesítő adat jelszavak.</span><span class="sxs-lookup"><span data-stu-id="679b1-212">hello credential passwords are encrypted and secure from users with read-only access tooElastic Database jobs objects.</span></span> <span data-ttu-id="679b1-213">Azonban lehetséges, hogy egy rosszindulatú felhasználó olvasási és írási hozzáférése tooElastic adatbázis feladatok objektumok tooextract jelszót.</span><span class="sxs-lookup"><span data-stu-id="679b1-213">But it is possible for a malicious user with read-write access tooElastic Database Jobs objects tooextract a password.</span></span> <span data-ttu-id="679b1-214">Hitelesítő adatok tervezett toobe feladat végrehajtások ismételten.</span><span class="sxs-lookup"><span data-stu-id="679b1-214">Credentials are designed toobe reused across job executions.</span></span> <span data-ttu-id="679b1-215">Hitelesítő adatok tootarget adatbázisok átadott kapcsolatok létesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="679b1-215">Credentials are passed tootarget databases when establishing connections.</span></span> <span data-ttu-id="679b1-216">Jelenleg nincs korlátozás az egyes hitelesítési használt hello céladatbázisokhoz, rosszindulatú felhasználó hozzáadhat egy adatbázis cél adatbázis a hello támadásokat.</span><span class="sxs-lookup"><span data-stu-id="679b1-216">There are currently no restrictions on hello target databases used for each credential, malicious user could add a database target for a database under hello malicious user's control.</span></span> <span data-ttu-id="679b1-217">hello felhasználó később volt az adatbázis toogain hello hitelesítő adatához tartozó jelszó célzó feladat indítása.</span><span class="sxs-lookup"><span data-stu-id="679b1-217">hello user could subsequently start a job targeting this database toogain hello credential's password.</span></span>

<span data-ttu-id="679b1-218">Ajánlott biztonsági eljárások a rugalmas adatbázis-feladatok a következők:</span><span class="sxs-lookup"><span data-stu-id="679b1-218">Security best practices for Elastic Database jobs include:</span></span>

* <span data-ttu-id="679b1-219">Hello API-k tootrusted egyének-használatát korlátozása.</span><span class="sxs-lookup"><span data-stu-id="679b1-219">Limit usage of hello APIs tootrusted individuals.</span></span>
* <span data-ttu-id="679b1-220">Hitelesítő adatok lehetnek hello legalacsonyabb jogosultságok szükséges tooperform hello feladata.</span><span class="sxs-lookup"><span data-stu-id="679b1-220">Credentials should have hello least privileges necessary tooperform hello job task.</span></span>  <span data-ttu-id="679b1-221">További információ látható belül ez [engedélyezési és engedélyek](https://msdn.microsoft.com/library/bb669084.aspx) SQL Server MSDN-cikk tárgyalja.</span><span class="sxs-lookup"><span data-stu-id="679b1-221">More information can be seen within this [Authorization and Permissions](https://msdn.microsoft.com/library/bb669084.aspx) SQL Server MSDN article.</span></span>

### <a name="toocreate-an-encrypted-credential-for-job-execution-across-databases"></a><span data-ttu-id="679b1-222">egy titkosított hitelesítő adatokat, a feladat végrehajtása az adatbázisok közötti toocreate</span><span class="sxs-lookup"><span data-stu-id="679b1-222">toocreate an encrypted credential for job execution across databases</span></span>
<span data-ttu-id="679b1-223">egy új titkosított toocreate hitelesítőadat-, hello [ **Get-Credential parancsmag** ](https://technet.microsoft.com/library/hh849815.aspx) megadását kéri a felhasználónevet és jelszót, amelyek átadhatók toohello [ **New-AzureSqlJobCredential a parancsmag**](/powershell/module/elasticdatabasejobs/new-azuresqljobcredential).</span><span class="sxs-lookup"><span data-stu-id="679b1-223">toocreate a new encrypted credential, hello [**Get-Credential cmdlet**](https://technet.microsoft.com/library/hh849815.aspx) prompts for a user name and password that can be passed toohello [**New-AzureSqlJobCredential cmdlet**](/powershell/module/elasticdatabasejobs/new-azuresqljobcredential).</span></span>

    $credentialName = "{Credential Name}"
    $databaseCredential = Get-Credential
    $credential = New-AzureSqlJobCredential -Credential $databaseCredential -CredentialName $credentialName
    Write-Output $credential

### <a name="tooupdate-credentials"></a><span data-ttu-id="679b1-224">tooupdate hitelesítő adatok</span><span class="sxs-lookup"><span data-stu-id="679b1-224">tooupdate credentials</span></span>
<span data-ttu-id="679b1-225">Ha jelszavak módosításához használja a hello [ **Set-AzureSqlJobCredential parancsmag** ](/powershell/module/elasticdatabasejobs/set-azuresqljobcredential) és set hello **CredentialName** paraméter.</span><span class="sxs-lookup"><span data-stu-id="679b1-225">When passwords change, use hello [**Set-AzureSqlJobCredential cmdlet**](/powershell/module/elasticdatabasejobs/set-azuresqljobcredential) and set hello **CredentialName** parameter.</span></span>

    $credentialName = "{Credential Name}"
    Set-AzureSqlJobCredential -CredentialName $credentialName -Credential $credential 

## <a name="toodefine-an-elastic-database-shard-map-target"></a><span data-ttu-id="679b1-226">egy rugalmas adatbázist shard térkép célhoz toodefine</span><span class="sxs-lookup"><span data-stu-id="679b1-226">toodefine an Elastic Database shard map target</span></span>
<span data-ttu-id="679b1-227">tooexecute egy feladat shard csoportban lévő összes adatbázis ellen (használatával létrehozott [Elastic Database ügyféloldali kódtárának](sql-database-elastic-database-client-library.md)), használja a shard leképezését hello adatbázis célként.</span><span class="sxs-lookup"><span data-stu-id="679b1-227">tooexecute a job against all databases in a shard set (created using [Elastic Database client library](sql-database-elastic-database-client-library.md)), use a shard map as hello database target.</span></span> <span data-ttu-id="679b1-228">Ebben a példában létrehozott hello Elastic Database ügyféloldali kódtárának szilánkos alkalmazás szükséges.</span><span class="sxs-lookup"><span data-stu-id="679b1-228">This example requires a sharded application created using hello Elastic Database client library.</span></span> <span data-ttu-id="679b1-229">Lásd: [Ismerkedés a rugalmas adatbázis eszközök minta](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="679b1-229">See [Getting started with Elastic Database tools sample](sql-database-elastic-scale-get-started.md).</span></span>

<span data-ttu-id="679b1-230">hello shard manager adatbázist be kell állítani egy adatbázis célként, és majd hello adott shard térkép meg kell adni, célként.</span><span class="sxs-lookup"><span data-stu-id="679b1-230">hello shard map manager database must be set as a database target and then hello specific shard map must be specified as a target.</span></span>

    $shardMapCredentialName = "{Credential Name}"
    $shardMapDatabaseName = "{ShardMapDatabaseName}" #example: ElasticScaleStarterKit_ShardMapManagerDb
    $shardMapDatabaseServerName = "{ShardMapServerName}"
    $shardMapName = "{MyShardMap}" #example: CustomerIDShardMap
    $shardMapDatabaseTarget = New-AzureSqlJobTarget -DatabaseName $shardMapDatabaseName -ServerName $shardMapDatabaseServerName
    $shardMapTarget = New-AzureSqlJobTarget -ShardMapManagerCredentialName $shardMapCredentialName -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapDatabaseServerName -ShardMapName $shardMapName
    Write-Output $shardMapTarget

## <a name="create-a-t-sql-script-for-execution-across-databases"></a><span data-ttu-id="679b1-231">Az adatbázisok közötti végrehajtási T-SQL parancsfájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="679b1-231">Create a T-SQL Script for execution across databases</span></span>
<span data-ttu-id="679b1-232">T-SQL-parancsfájlok végrehajtásra létrehozásakor ajánlott toobuild őket toobe [idempotent](https://en.wikipedia.org/wiki/Idempotence) és refs-hibákkal szemben.</span><span class="sxs-lookup"><span data-stu-id="679b1-232">When creating T-SQL scripts for execution, it is highly recommended toobuild them toobe [idempotent](https://en.wikipedia.org/wiki/Idempotence) and resilient against failures.</span></span> <span data-ttu-id="679b1-233">Rugalmas adatbázis-feladatok parancsfájl végrehajtásának próbálkozik, ha végrehajtási hiba, függetlenül hello besorolás hello hiba észlel.</span><span class="sxs-lookup"><span data-stu-id="679b1-233">Elastic Database jobs will retry execution of a script whenever execution encounters a failure, regardless of hello classification of hello failure.</span></span>

<span data-ttu-id="679b1-234">Használjon hello [ **New-AzureSqlJobContent parancsmag** ](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent) toocreate és mentse a parancsfájlt a végrehajtás, és állítsa be a hello **- ContentName** és **- CommandText**paraméterek.</span><span class="sxs-lookup"><span data-stu-id="679b1-234">Use hello [**New-AzureSqlJobContent cmdlet**](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent) toocreate and save a script for execution and set hello **-ContentName** and **-CommandText** parameters.</span></span>

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

### <a name="create-a-new-script-from-a-file"></a><span data-ttu-id="679b1-235">Új parancsfájl létrehozása fájlból</span><span class="sxs-lookup"><span data-stu-id="679b1-235">Create a new script from a file</span></span>
<span data-ttu-id="679b1-236">Ha a T-SQL parancsfájl hello fájlban van definiálva, a tooimport hello parancsprogram használata:</span><span class="sxs-lookup"><span data-stu-id="679b1-236">If hello T-SQL script is defined within a file, use this tooimport hello script:</span></span>

    $scriptName = "My Script Imported from a File"
    $scriptPath = "{Path tooSQL File}"
    $scriptCommandText = Get-Content -Path $scriptPath
    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="tooupdate-a-t-sql-script-for-execution-across-databases"></a><span data-ttu-id="679b1-237">az adatbázisok közötti végrehajtásra tooupdate egy T-SQL parancsfájl</span><span class="sxs-lookup"><span data-stu-id="679b1-237">tooupdate a T-SQL script for execution across databases</span></span>
<span data-ttu-id="679b1-238">A PowerShell parancsfájl frissítések hello T-SQL egy meglévő parancsfájl parancsszövege.</span><span class="sxs-lookup"><span data-stu-id="679b1-238">This PowerShell script updates hello T-SQL command text for an existing script.</span></span>

<span data-ttu-id="679b1-239">A következő változók tooreflect hello beállítása hello parancsfájl definition toobe set szükséges:</span><span class="sxs-lookup"><span data-stu-id="679b1-239">Set hello following variables tooreflect hello desired script definition toobe set:</span></span>

    $scriptName = "Create a TestTable"
    $scriptUpdateComment = "Adding AdditionalInformation column tooTestTable"
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

### <a name="tooupdate-hello-definition-tooan-existing-script"></a><span data-ttu-id="679b1-240">tooupdate hello definition tooan meglévő parancsfájl</span><span class="sxs-lookup"><span data-stu-id="679b1-240">tooupdate hello definition tooan existing script</span></span>
    Set-AzureSqlJobContentDefinition -ContentName $scriptName -CommandText $scriptCommandText -Comment $scriptUpdateComment 

## <a name="toocreate-a-job-tooexecute-a-script-across-a-shard-map"></a><span data-ttu-id="679b1-241">egy feladat tooexecute keresztül shard térképre parancsfájl toocreate</span><span class="sxs-lookup"><span data-stu-id="679b1-241">toocreate a job tooexecute a script across a shard map</span></span>
<span data-ttu-id="679b1-242">A PowerShell parancsfájl keresztül minden shard rugalmasan méretezhető shard leképezés indít el egy parancsfájl végrehajtásának feladatot.</span><span class="sxs-lookup"><span data-stu-id="679b1-242">This PowerShell script starts a job for execution of a script across each shard in an Elastic Scale shard map.</span></span>

<span data-ttu-id="679b1-243">A következő változók tooreflect hello beállítása hello szükséges parancsfájl és a cél:</span><span class="sxs-lookup"><span data-stu-id="679b1-243">Set hello following variables tooreflect hello desired script and target:</span></span>

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $credentialName = "{Credential Name}"
    $shardMapTarget = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName 
    $job = New-AzureSqlJob -ContentName $scriptName -CredentialName $credentialName -JobName $jobName -TargetId $shardMapTarget.TargetId
    Write-Output $job

## <a name="tooexecute-a-job"></a><span data-ttu-id="679b1-244">egy feladat tooexecute</span><span class="sxs-lookup"><span data-stu-id="679b1-244">tooexecute a job</span></span>
<span data-ttu-id="679b1-245">A PowerShell-parancsfájl egy létező feladat végrehajtása:</span><span class="sxs-lookup"><span data-stu-id="679b1-245">This PowerShell script executes an existing job:</span></span>

<span data-ttu-id="679b1-246">Frissítés hello változó tooreflect hello kívánt feladat neve toohave hajtotta végre a következő:</span><span class="sxs-lookup"><span data-stu-id="679b1-246">Update hello following variable tooreflect hello desired job name toohave executed:</span></span>

    $jobName = "{Job Name}"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName 
    Write-Output $jobExecution

## <a name="tooretrieve-hello-state-of-a-single-job-execution"></a><span data-ttu-id="679b1-247">egyetlen feladat-végrehajtás tooretrieve hello állapota</span><span class="sxs-lookup"><span data-stu-id="679b1-247">tooretrieve hello state of a single job execution</span></span>
<span data-ttu-id="679b1-248">Használjon hello [ **Get-AzureSqlJobExecution parancsmag** ](/powershell/module/elasticdatabasejobs/get-azuresqljobexecution) és set hello **JobExecutionId** paraméter tooview hello állapotának feladat végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="679b1-248">Use hello [**Get-AzureSqlJobExecution cmdlet**](/powershell/module/elasticdatabasejobs/get-azuresqljobexecution) and set hello **JobExecutionId** parameter tooview hello state of job execution.</span></span>

    $jobExecutionId = "{Job Execution Id}"
    $jobExecution = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId
    Write-Output $jobExecution

<span data-ttu-id="679b1-249">Használja az azonos hello **Get-AzureSqlJobExecution** hello parancsmagot **IncludeChildren** paraméter tooview hello állapotának gyermek feladat végrehajtások, nevezetesen hello minden egyes feladatok végrehajtásának meghatározott állapotban hello feladat által megcélzott adatbázis.</span><span class="sxs-lookup"><span data-stu-id="679b1-249">Use hello same **Get-AzureSqlJobExecution** cmdlet with hello **IncludeChildren** parameter tooview hello state of child job executions, namely hello specific state for each job execution against each database targeted by hello job.</span></span>

    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions 

## <a name="tooview-hello-state-across-multiple-job-executions"></a><span data-ttu-id="679b1-250">több feladat végrehajtások közötti tooview hello állapota</span><span class="sxs-lookup"><span data-stu-id="679b1-250">tooview hello state across multiple job executions</span></span>
<span data-ttu-id="679b1-251">Hello [ **Get-AzureSqlJobExecution parancsmag** ](/powershell/module/elasticdatabasejobs/new-azuresqljob) több nem kötelező paraméter, amely használt toodisplay több feladat végrehajtások, megadott hello paramétereknek szűrve van.</span><span class="sxs-lookup"><span data-stu-id="679b1-251">hello [**Get-AzureSqlJobExecution cmdlet**](/powershell/module/elasticdatabasejobs/new-azuresqljob) has multiple optional parameters that can be used toodisplay multiple job executions, filtered through hello provided parameters.</span></span> <span data-ttu-id="679b1-252">hello következő hello lehetséges módjait toouse Get-AzureSqlJobExecution némelyike mutatja be:</span><span class="sxs-lookup"><span data-stu-id="679b1-252">hello following demonstrates some of hello possible ways toouse Get-AzureSqlJobExecution:</span></span>

<span data-ttu-id="679b1-253">Lekéri az összes aktív felső szintű feladat végrehajtások:</span><span class="sxs-lookup"><span data-stu-id="679b1-253">Retrieve all active top level job executions:</span></span>

    Get-AzureSqlJobExecution

<span data-ttu-id="679b1-254">Lekéri az összes felső szintű feladat végrehajtások, beleértve az inaktív feladat végrehajtások:</span><span class="sxs-lookup"><span data-stu-id="679b1-254">Retrieve all top level job executions, including inactive job executions:</span></span>

    Get-AzureSqlJobExecution -IncludeInactive

<span data-ttu-id="679b1-255">Lekéri az összes gyermek feladat végrehajtások inaktív feladat végrehajtások többek között a megadott feladat végrehajtási Azonosítóhoz:</span><span class="sxs-lookup"><span data-stu-id="679b1-255">Retrieve all child job executions of a provided job execution ID, including inactive job executions:</span></span>

    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId -IncludeInactive -IncludeChildren

<span data-ttu-id="679b1-256">Ütemezés használatával létrehozott összes feladat végrehajtások beolvasása / feladat-kombináció, beleértve az inaktív feladatok:</span><span class="sxs-lookup"><span data-stu-id="679b1-256">Retrieve all job executions created using a schedule / job combination, including inactive jobs:</span></span>

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive

<span data-ttu-id="679b1-257">Lekéri az összes feladat megadott shard térképre, beleértve az inaktív feladatokat célcsoportkezelést:</span><span class="sxs-lookup"><span data-stu-id="679b1-257">Retrieve all jobs targeting a specified shard map, including inactive jobs:</span></span>

    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive

<span data-ttu-id="679b1-258">Lekéri az összes feladat a megadott egyéni gyűjtemény, beleértve az inaktív feladatokat célcsoportkezelést:</span><span class="sxs-lookup"><span data-stu-id="679b1-258">Retrieve all jobs targeting a specified custom collection, including inactive jobs:</span></span>

    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive

<span data-ttu-id="679b1-259">Feladat feladat végrehajtások belül egy adott feladat végrehajtási hello listájának beolvasása:</span><span class="sxs-lookup"><span data-stu-id="679b1-259">Retrieve hello list of job task executions within a specific job execution:</span></span>

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions 

<span data-ttu-id="679b1-260">Feladat végrehajtási részlete beolvasása:</span><span class="sxs-lookup"><span data-stu-id="679b1-260">Retrieve job task execution details:</span></span>

<span data-ttu-id="679b1-261">a következő PowerShell-parancsfájl hello lehet egy feladat a feladat a végrehajtás, amely akkor különösen hasznos, ha végrehajtási hibakeresésére használt tooview hello részleteit.</span><span class="sxs-lookup"><span data-stu-id="679b1-261">hello following PowerShell script can be used tooview hello details of a job task execution, which is particularly useful when debugging execution failures.</span></span>

    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution

## <a name="tooretrieve-failures-within-job-task-executions"></a><span data-ttu-id="679b1-262">feladat feladat végrehajtások belül tooretrieve hibák</span><span class="sxs-lookup"><span data-stu-id="679b1-262">tooretrieve failures within job task executions</span></span>
<span data-ttu-id="679b1-263">Hello **JobTaskExecution objektum** hello életciklus hello feladat egy üzenettulajdonság együtt egy tulajdonság tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="679b1-263">hello **JobTaskExecution object** includes a property for hello lifecycle of hello task along with a message property.</span></span> <span data-ttu-id="679b1-264">Ha egy feladat a feladat végrehajtása sikertelen volt, hello életciklus tulajdonság túl állítható*sikertelen* és hello üzenettulajdonság lesz toohello eredményül kapott hibaüzenetet, és a verem.</span><span class="sxs-lookup"><span data-stu-id="679b1-264">If a job task execution failed, hello lifecycle property will be set too*Failed* and hello message property will be set toohello resulting exception message and its stack.</span></span> <span data-ttu-id="679b1-265">Ha egy feladat sikertelen volt, akkor fontos tooview hello részleteit munka feladatai, amelyek egy adott feladat sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="679b1-265">If a job did not succeed, it is important tooview hello details of job tasks that did not succeed for a given job.</span></span>

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions) 
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }

## <a name="toowait-for-a-job-execution-toocomplete"></a><span data-ttu-id="679b1-266">a feladat végrehajtási toocomplete a toowait</span><span class="sxs-lookup"><span data-stu-id="679b1-266">toowait for a job execution toocomplete</span></span>
<span data-ttu-id="679b1-267">a következő PowerShell-parancsfájl hello lehet egy feladat feladat toocomplete a használt toowait:</span><span class="sxs-lookup"><span data-stu-id="679b1-267">hello following PowerShell script can be used toowait for a job task toocomplete:</span></span>

    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId 

## <a name="create-a-custom-execution-policy"></a><span data-ttu-id="679b1-268">Egyéni végrehajtási házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="679b1-268">Create a custom execution policy</span></span>
<span data-ttu-id="679b1-269">Rugalmas adatbázis-feladatok feladat indításakor alkalmazható egyéni végrehajtási házirendek létrehozását támogatja.</span><span class="sxs-lookup"><span data-stu-id="679b1-269">Elastic Database jobs supports creating custom execution policies that can be applied when starting jobs.</span></span>

<span data-ttu-id="679b1-270">Végrehajtási házirendek definiálása jelenleg engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="679b1-270">Execution policies currently allow for defining:</span></span>

* <span data-ttu-id="679b1-271">Name: Azonosítója hello végrehajtási házirendet.</span><span class="sxs-lookup"><span data-stu-id="679b1-271">Name: Identifier for hello execution policy.</span></span>
* <span data-ttu-id="679b1-272">Feladat időtúllépése: Teljes idő előtt rugalmas adatbázis-feladatok megszakítja a feladatot.</span><span class="sxs-lookup"><span data-stu-id="679b1-272">Job Timeout: Total time before a job will be canceled by Elastic Database Jobs.</span></span>
* <span data-ttu-id="679b1-273">Kezdeti újrapróbálkozási időköz: Intervallum toowait előtt próbálkozik újra.</span><span class="sxs-lookup"><span data-stu-id="679b1-273">Initial Retry Interval: Interval toowait before first retry.</span></span>
* <span data-ttu-id="679b1-274">Maximális újrapróbálkozási időköz: Az újrapróbálkozási intervallumok toouse kap.</span><span class="sxs-lookup"><span data-stu-id="679b1-274">Maximum Retry Interval: Cap of retry intervals toouse.</span></span>
* <span data-ttu-id="679b1-275">Újrapróbálkozási időköz leállítási együttható: Együttható használt toocalculate hello következő intervallum újrapróbálkozások között.</span><span class="sxs-lookup"><span data-stu-id="679b1-275">Retry Interval Backoff Coefficient: Coefficient used toocalculate hello next interval between retries.</span></span>  <span data-ttu-id="679b1-276">hello alábbi képlet használható: (kezdeti újrapróbálkozási időközét) * Math.pow ((időköz leállítási együttható), (próbálkozások száma) - 2).</span><span class="sxs-lookup"><span data-stu-id="679b1-276">hello following formula is used: (Initial Retry Interval) * Math.pow((Interval Backoff Coefficient), (Number of Retries) - 2).</span></span> 
* <span data-ttu-id="679b1-277">Kísérletek maximális száma: hello maximális száma újrapróbálkozási kísérletek tooperform feladat.</span><span class="sxs-lookup"><span data-stu-id="679b1-277">Maximum Attempts: hello maximum number of retry attempts tooperform within a job.</span></span>

<span data-ttu-id="679b1-278">hello alapértelmezett végrehajtási házirend hello a következő értékeket használja:</span><span class="sxs-lookup"><span data-stu-id="679b1-278">hello default execution policy uses hello following values:</span></span>

* <span data-ttu-id="679b1-279">Name: Alapértelmezett végrehajtási házirend</span><span class="sxs-lookup"><span data-stu-id="679b1-279">Name: Default execution policy</span></span>
* <span data-ttu-id="679b1-280">Feladat időtúllépése: 1 hét</span><span class="sxs-lookup"><span data-stu-id="679b1-280">Job Timeout: 1 week</span></span>
* <span data-ttu-id="679b1-281">Kezdeti újrapróbálkozási időköz: 100 milliomod másodperc</span><span class="sxs-lookup"><span data-stu-id="679b1-281">Initial Retry Interval:  100 milliseconds</span></span>
* <span data-ttu-id="679b1-282">Maximális újrapróbálkozási időköz: 30 perc</span><span class="sxs-lookup"><span data-stu-id="679b1-282">Maximum Retry Interval: 30 minutes</span></span>
* <span data-ttu-id="679b1-283">Ismételje meg a időköz együttható: 2. régiója</span><span class="sxs-lookup"><span data-stu-id="679b1-283">Retry Interval Coefficient: 2</span></span>
* <span data-ttu-id="679b1-284">Kísérletek maximális száma: 2 147 483 647</span><span class="sxs-lookup"><span data-stu-id="679b1-284">Maximum Attempts: 2,147,483,647</span></span>

<span data-ttu-id="679b1-285">Szükségeskonfiguráció-hello végrehajtási szabályzat létrehozása:</span><span class="sxs-lookup"><span data-stu-id="679b1-285">Create hello desired execution policy:</span></span>

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 10
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval 
    -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $executionPolicy

### <a name="update-a-custom-execution-policy"></a><span data-ttu-id="679b1-286">Egy egyéni végrehajtási házirend frissítése</span><span class="sxs-lookup"><span data-stu-id="679b1-286">Update a custom execution policy</span></span>
<span data-ttu-id="679b1-287">Frissítés hello végrehajtási házirend tooupdate szükséges:</span><span class="sxs-lookup"><span data-stu-id="679b1-287">Update hello desired execution policy tooupdate:</span></span>

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 15
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $updatedExecutionPolicy = Set-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $updatedExecutionPolicy

## <a name="cancel-a-job"></a><span data-ttu-id="679b1-288">Feladatok megszakítása</span><span class="sxs-lookup"><span data-stu-id="679b1-288">Cancel a job</span></span>
<span data-ttu-id="679b1-289">Rugalmas adatbázis-feladatok a feladatok megszakítását kérelmeket támogatja.</span><span class="sxs-lookup"><span data-stu-id="679b1-289">Elastic Database Jobs supports cancellation requests of jobs.</span></span>  <span data-ttu-id="679b1-290">Rugalmas adatbázis-feladatok jelenleg végrehajtás alatt álló feladat a lemondási kérelmet észleli, ha a program megpróbálja toostop hello feladat.</span><span class="sxs-lookup"><span data-stu-id="679b1-290">If Elastic Database Jobs detects a cancellation request for a job currently being executed, it will attempt toostop hello job.</span></span>

<span data-ttu-id="679b1-291">Rugalmas adatbázis-feladatok is hajtsa végre a megszakítási két különböző módja van:</span><span class="sxs-lookup"><span data-stu-id="679b1-291">There are two different ways that Elastic Database Jobs can perform a cancellation:</span></span>

1. <span data-ttu-id="679b1-292">Jelenleg feldolgozás alatt álló feladatok Mégse: törlése közben jelenleg fut egy feladat észleli, ha a megszakítási belül jelenleg végrehajtás alatt álló hello feladat aspektusa hello történt kísérlet.</span><span class="sxs-lookup"><span data-stu-id="679b1-292">Cancel currently executing tasks: If a cancellation is detected while a task is currently running, a cancellation will be attempted within hello currently executing aspect of hello task.</span></span>  <span data-ttu-id="679b1-293">Példa: Ha egy hosszú ideig tartó jelenleg végrehajtás alatt álló lekérdezés törlése megkísérelt van, nem lesznek egy kísérlet toocancel hello lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="679b1-293">For example: If there is a long running query currently being performed when a cancellation is attempted, there will be an attempt toocancel hello query.</span></span>
2. <span data-ttu-id="679b1-294">Tevékenység-újrapróbálkozások megszakítása: törlése észlelésekor hello vezérlő szál feladat a végrehajtás elindítása előtt hello vezérlő szál elkerülése hello feladat elindítása és hello kérelem deklarálható megszakítottként.</span><span class="sxs-lookup"><span data-stu-id="679b1-294">Canceling task retries: If a cancellation is detected by hello control thread before a task is launched for execution, hello control thread will avoid launching hello task and declare hello request as canceled.</span></span>

<span data-ttu-id="679b1-295">Ha a feladat megszakításának szülő-feladat van szükség, hello lemondási kérelmet szerződéses kötelezettségeket a hello szülő feladat és összes a gyermek-feladatokkal.</span><span class="sxs-lookup"><span data-stu-id="679b1-295">If a job cancellation is requested for a parent job, hello cancellation request will be honored for hello parent job and for all of its child jobs.</span></span>

<span data-ttu-id="679b1-296">a megszakítási kérelmet, toosubmit hello használata [ **Stop-AzureSqlJobExecution parancsmag** ](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) és set hello **JobExecutionId** paraméter.</span><span class="sxs-lookup"><span data-stu-id="679b1-296">toosubmit a cancellation request, use hello [**Stop-AzureSqlJobExecution cmdlet**](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) and set hello **JobExecutionId** parameter.</span></span>

    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId

## <a name="toodelete-a-job-and-job-history-asynchronously"></a><span data-ttu-id="679b1-297">egy feladat toodelete és aszinkron módon feladatelőzmények</span><span class="sxs-lookup"><span data-stu-id="679b1-297">toodelete a job and job history asynchronously</span></span>
<span data-ttu-id="679b1-298">Rugalmas adatbázis-feladatok támogatja az aszinkron feladatok törlése.</span><span class="sxs-lookup"><span data-stu-id="679b1-298">Elastic Database jobs supports asynchronous deletion of jobs.</span></span> <span data-ttu-id="679b1-299">Egy feladat is törlésre, és hello rendszer törölni fogja a hello feladat és a feladatelőzményekben összes feladat végrehajtások hello feladat befejezése után.</span><span class="sxs-lookup"><span data-stu-id="679b1-299">A job can be marked for deletion and hello system will delete hello job and all its job history after all job executions have completed for hello job.</span></span> <span data-ttu-id="679b1-300">hello rendszer nem fogja automatikusan megszakítja a aktív feladat végrehajtások.</span><span class="sxs-lookup"><span data-stu-id="679b1-300">hello system will not automatically cancel active job executions.</span></span>  

<span data-ttu-id="679b1-301">Invoke [ **Stop-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) toocancel aktív feladat végrehajtások.</span><span class="sxs-lookup"><span data-stu-id="679b1-301">Invoke [**Stop-AzureSqlJobExecution**](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) toocancel active job executions.</span></span>

<span data-ttu-id="679b1-302">tootrigger feladat törlése, használjon hello [ **Remove-AzureSqlJob parancsmag** ](/powershell/module/elasticdatabasejobs/remove-azuresqljob) és set hello **JobName** paraméter.</span><span class="sxs-lookup"><span data-stu-id="679b1-302">tootrigger job deletion, use hello [**Remove-AzureSqlJob cmdlet**](/powershell/module/elasticdatabasejobs/remove-azuresqljob) and set hello **JobName** parameter.</span></span>

    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName

## <a name="toocreate-a-custom-database-target"></a><span data-ttu-id="679b1-303">Egyéni adatbázis target toocreate</span><span class="sxs-lookup"><span data-stu-id="679b1-303">toocreate a custom database target</span></span>
<span data-ttu-id="679b1-304">Egyéni adatbázis célok közvetlen végrehajtásra vagy egy egyéni adatbázis csoportban foglalható adhat meg.</span><span class="sxs-lookup"><span data-stu-id="679b1-304">You can define custom database targets either for direct execution or for inclusion within a custom database group.</span></span> <span data-ttu-id="679b1-305">Például mert **rugalmas készletek** van még nem támogatott PowerShell API-val, létrehozhat egy egyéni adatbázis célként és egy egyéni adatbázis gyűjtemény célként, ami magában foglalja az összes hello adatbázis hello készletben.</span><span class="sxs-lookup"><span data-stu-id="679b1-305">For example, because **elastic pools** are not yet directly supported using PowerShell APIs, you can create a custom database target and custom database collection target which encompasses all hello databases in hello pool.</span></span>

<span data-ttu-id="679b1-306">Állítsa be a következő változók tooreflect hello szükséges adatbázis-információ hello:</span><span class="sxs-lookup"><span data-stu-id="679b1-306">Set hello following variables tooreflect hello desired database information:</span></span>

    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName 

## <a name="toocreate-a-custom-database-collection-target"></a><span data-ttu-id="679b1-307">Egyéni adatbázis gyűjtemény célja toocreate</span><span class="sxs-lookup"><span data-stu-id="679b1-307">toocreate a custom database collection target</span></span>
<span data-ttu-id="679b1-308">Használjon hello [ **New-AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) parancsmag toodefine egy egyéni adatbázis gyűjtemény cél tooenable végrehajtása több meghatározott adatbázis cél között.</span><span class="sxs-lookup"><span data-stu-id="679b1-308">Use hello [**New-AzureSqlJobTarget**](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) cmdlet toodefine a custom database collection target tooenable execution across multiple defined database targets.</span></span> <span data-ttu-id="679b1-309">Egy adatbázis-csoport létrehozása után adatbázisok társítható hello egyéni gyűjtemény célja.</span><span class="sxs-lookup"><span data-stu-id="679b1-309">After creating a database group, databases can be associated with hello custom collection target.</span></span>

<span data-ttu-id="679b1-310">Állítsa be a következő változók tooreflect hello kívánt egyéni gyűjtemény konfigurációjához hello:</span><span class="sxs-lookup"><span data-stu-id="679b1-310">Set hello following variables tooreflect hello desired custom collection target configuration:</span></span>

    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 

### <a name="tooadd-databases-tooa-custom-database-collection-target"></a><span data-ttu-id="679b1-311">tooadd adatbázisok tooa egyéni adatbázis gyűjtemény célja</span><span class="sxs-lookup"><span data-stu-id="679b1-311">tooadd databases tooa custom database collection target</span></span>
<span data-ttu-id="679b1-312">egy adatbázis tooa egyéni gyűjteményre tooadd hello használata [ **Add-AzureSqlJobChildTarget** ](/powershell/module/elasticdatabasejobs/add-azuresqljobchildtarget) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="679b1-312">tooadd a database tooa specific custom collection use hello [**Add-AzureSqlJobChildTarget**](/powershell/module/elasticdatabasejobs/add-azuresqljobchildtarget) cmdlet.</span></span>

    $databaseServerName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName 

#### <a name="review-hello-databases-within-a-custom-database-collection-target"></a><span data-ttu-id="679b1-313">Tekintse át a hello adatbázisok belül egy egyéni adatbázis-gyűjtemény célja</span><span class="sxs-lookup"><span data-stu-id="679b1-313">Review hello databases within a custom database collection target</span></span>
<span data-ttu-id="679b1-314">Használjon hello [ **Get-AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) parancsmag tooretrieve hello gyermek adatbázis egy egyéni adatbázis-gyűjtemény célja.</span><span class="sxs-lookup"><span data-stu-id="679b1-314">Use hello [**Get-AzureSqlJobTarget**](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) cmdlet tooretrieve hello child databases within a custom database collection target.</span></span> 

    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets

### <a name="create-a-job-tooexecute-a-script-across-a-custom-database-collection-target"></a><span data-ttu-id="679b1-315">Hozzon létre egy feladat tooexecute egy parancsprogramot egy egyéni adatbázis-gyűjtemény célja között</span><span class="sxs-lookup"><span data-stu-id="679b1-315">Create a job tooexecute a script across a custom database collection target</span></span>
<span data-ttu-id="679b1-316">Használjon hello [ **New-AzureSqlJob** ](/powershell/module/elasticdatabasejobs/new-azuresqljob) parancsmag toocreate egy feladatot, egy egyéni adatbázis gyűjtemény tároló által definiált adatbázisok csoportja ellen.</span><span class="sxs-lookup"><span data-stu-id="679b1-316">Use hello [**New-AzureSqlJob**](/powershell/module/elasticdatabasejobs/new-azuresqljob) cmdlet toocreate a job against a group of databases defined by a custom database collection target.</span></span> <span data-ttu-id="679b1-317">Rugalmas adatbázis-feladatok hello feladat kibővítése minden megfelelő tooa adatbázis társított hello egyéni adatbázis-gyűjtemény célja, és győződjön meg arról, hogy minden egyes adatbázison hello parancsfájl végrehajtása több gyermek feladat.</span><span class="sxs-lookup"><span data-stu-id="679b1-317">Elastic Database jobs will expand hello job into multiple child jobs each corresponding tooa database associated with hello custom database collection target and ensure that hello script is executed against each database.</span></span> <span data-ttu-id="679b1-318">Ebben az esetben fontos, hogy a parancsfájlok az idempotent toobe rugalmas tooretries.</span><span class="sxs-lookup"><span data-stu-id="679b1-318">Again, it is important that scripts are idempotent toobe resilient tooretries.</span></span>

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job

## <a name="data-collection-across-databases"></a><span data-ttu-id="679b1-319">Az adatbázisok közötti adatok gyűjtése</span><span class="sxs-lookup"><span data-stu-id="679b1-319">Data collection across databases</span></span>
<span data-ttu-id="679b1-320">Egy feladat tooexecute lekérdezés használja az adatbázisok csoportja és küldhet hello eredmények tooa adott táblához.</span><span class="sxs-lookup"><span data-stu-id="679b1-320">You can use a job tooexecute a query across a group of databases and send hello results tooa specific table.</span></span> <span data-ttu-id="679b1-321">hello tábla hello tény toosee hello lekérdezés eredményében minden adatbázisból után kérdezhetők le.</span><span class="sxs-lookup"><span data-stu-id="679b1-321">hello table can be queried after hello fact toosee hello query’s results from each database.</span></span> <span data-ttu-id="679b1-322">Így lehetővé teszi egy aszinkron metódus tooexecute lekérdezés több adatbázis közötti.</span><span class="sxs-lookup"><span data-stu-id="679b1-322">This provides an asynchronous method tooexecute a query across many databases.</span></span> <span data-ttu-id="679b1-323">Sikertelen bejelentkezési kísérletek újrapróbálkozások keresztül automatikusan kezeli.</span><span class="sxs-lookup"><span data-stu-id="679b1-323">Failed attempts are handled automatically via retries.</span></span>

<span data-ttu-id="679b1-324">hello megadott céltábla automatikusan létrejön, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="679b1-324">hello specified destination table will be automatically created if it does not yet exist.</span></span> <span data-ttu-id="679b1-325">Új tábla hello eredményhalmazt adott hello hello sémája megegyezik.</span><span class="sxs-lookup"><span data-stu-id="679b1-325">hello new table matches hello schema of hello returned result set.</span></span> <span data-ttu-id="679b1-326">Egy parancsfájl több eredménykészlet adja vissza, ha a rugalmas adatbázis-feladatok csak küldi hello első toohello célként megadott táblája.</span><span class="sxs-lookup"><span data-stu-id="679b1-326">If a script returns multiple result sets, Elastic Database jobs will only send hello first toohello destination table.</span></span>

<span data-ttu-id="679b1-327">hello következő PowerShell-parancsfájl végrehajtja egy parancsfájlt, és összegyűjti az eredményeket egy megadott táblába.</span><span class="sxs-lookup"><span data-stu-id="679b1-327">hello following PowerShell script executes a script and collects its results into a specified table.</span></span> <span data-ttu-id="679b1-328">Ez a parancsfájl feltételezi, hogy egy T-SQL parancsfájl létrejött-e amelyek kimenete egy eredményhalmaz és, hogy létrejött-e egy egyéni adatbázis-gyűjtemény célja.</span><span class="sxs-lookup"><span data-stu-id="679b1-328">This script assumes that a T-SQL script has been created which outputs a single result set and that a custom database collection target has been created.</span></span>

<span data-ttu-id="679b1-329">A parancsfájl a hello [ **Get-AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="679b1-329">This script uses hello [**Get-AzureSqlJobTarget**](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) cmdlet.</span></span> <span data-ttu-id="679b1-330">A parancsfájl, a hitelesítő adatokat és a végrehajtási cél hello paraméterek beállítása:</span><span class="sxs-lookup"><span data-stu-id="679b1-330">Set hello parameters for script, credentials, and execution target:</span></span>

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

### <a name="toocreate-and-start-a-job-for-data-collection-scenarios"></a><span data-ttu-id="679b1-331">toocreate és kezdő egy feladatot az adatok gyűjtése forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="679b1-331">toocreate and start a job for data collection scenarios</span></span>
<span data-ttu-id="679b1-332">A parancsfájl a hello [ **Start-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/start-azuresqljobexecution) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="679b1-332">This script uses hello [**Start-AzureSqlJobExecution**](/powershell/module/elasticdatabasejobs/start-azuresqljobexecution) cmdlet.</span></span>

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

## <a name="tooschedule-a-job-execution-trigger"></a><span data-ttu-id="679b1-333">a feladat végrehajtási eseményindító tooschedule</span><span class="sxs-lookup"><span data-stu-id="679b1-333">tooschedule a job execution trigger</span></span>
<span data-ttu-id="679b1-334">a következő PowerShell-parancsfájl hello lehet használt toocreate ismétlődő ütemezés szerint.</span><span class="sxs-lookup"><span data-stu-id="679b1-334">hello following PowerShell script can be used toocreate a recurring schedule.</span></span> <span data-ttu-id="679b1-335">A parancsfájl a perces időközt, de [ **New-AzureSqlJobSchedule** ](/powershell/module/elasticdatabasejobs/new-azuresqljobschedule) - DayInterval, - HourInterval, - MonthInterval, és - WeekInterval paramétereket is támogatja.</span><span class="sxs-lookup"><span data-stu-id="679b1-335">This script uses a minute interval, but [**New-AzureSqlJobSchedule**](/powershell/module/elasticdatabasejobs/new-azuresqljobschedule) also supports -DayInterval, -HourInterval, -MonthInterval, and -WeekInterval parameters.</span></span> <span data-ttu-id="679b1-336">Csak egyszer hajtható végre ütemezéseket is létrehozható, hogy - alkalommal.</span><span class="sxs-lookup"><span data-stu-id="679b1-336">Schedules that execute only once can be created by passing -OneTime.</span></span>

<span data-ttu-id="679b1-337">Új ütemezés létrehozása:</span><span class="sxs-lookup"><span data-stu-id="679b1-337">Create a new schedule:</span></span>

    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule 
    -MinuteInterval $minuteInterval 
    -ScheduleName $scheduleName 
    -StartTime $startTime 
    Write-Output $schedule

### <a name="tootrigger-a-job-executed-on-a-time-schedule"></a><span data-ttu-id="679b1-338">a feladat végrehajtása ütemezéssel tootrigger</span><span class="sxs-lookup"><span data-stu-id="679b1-338">tootrigger a job executed on a time schedule</span></span>
<span data-ttu-id="679b1-339">Egy feladat eseményindító lehet meghatározott toohave egy feladat végrehajtása függően tooa ütemezéssel.</span><span class="sxs-lookup"><span data-stu-id="679b1-339">A job trigger can be defined toohave a job executed according tooa time schedule.</span></span> <span data-ttu-id="679b1-340">a következő PowerShell-parancsfájl hello lehet használt toocreate feladat eseményindítót.</span><span class="sxs-lookup"><span data-stu-id="679b1-340">hello following PowerShell script can be used toocreate a job trigger.</span></span>

<span data-ttu-id="679b1-341">Használjon [New-AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/new-azuresqljobtrigger) és hello beállítása a következő változók toocorrespond toohello kívánt feladat és ütemezése:</span><span class="sxs-lookup"><span data-stu-id="679b1-341">Use [New-AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/new-azuresqljobtrigger) and set hello following variables toocorrespond toohello desired job and schedule:</span></span>

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger
    -ScheduleName $scheduleName
    -JobName $jobName
    Write-Output $jobTrigger

### <a name="tooremove-a-scheduled-association-toostop-job-from-executing-on-schedule"></a><span data-ttu-id="679b1-342">tooremove egy ütemezett társítás toostop feladat futtatásának ütemezés szerint</span><span class="sxs-lookup"><span data-stu-id="679b1-342">tooremove a scheduled association toostop job from executing on schedule</span></span>
<span data-ttu-id="679b1-343">Ismétlődés feladat végrehajtása a feladat eseményindítót, hello feladat eseményindító keresztül toodiscontinue távolítható el.</span><span class="sxs-lookup"><span data-stu-id="679b1-343">toodiscontinue reoccurring job execution through a job trigger, hello job trigger can be removed.</span></span> <span data-ttu-id="679b1-344">Távolítsa el a egy feladat eseményindító toostop egy feladat a végrehajtás alatt álló hello függően tooa ütemezés [ **Remove-AzureSqlJobTrigger parancsmag**](/powershell/module/elasticdatabasejobs/remove-azuresqljobtrigger).</span><span class="sxs-lookup"><span data-stu-id="679b1-344">Remove a job trigger toostop a job from being executed according tooa schedule using hello [**Remove-AzureSqlJobTrigger cmdlet**](/powershell/module/elasticdatabasejobs/remove-azuresqljobtrigger).</span></span>

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger 
    -ScheduleName $scheduleName 
    -JobName $jobName

### <a name="retrieve-job-triggers-bound-tooa-time-schedule"></a><span data-ttu-id="679b1-345">Eseményindítók kötött tooa idő ütemezett feladat beolvasása</span><span class="sxs-lookup"><span data-stu-id="679b1-345">Retrieve job triggers bound tooa time schedule</span></span>
<span data-ttu-id="679b1-346">a következő PowerShell-parancsfájl hello használt tooobtain lehetnek és hello eseményindítók regisztrált tooa adott időpontban az ütemezett feladat megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="679b1-346">hello following PowerShell script can be used tooobtain and display hello job triggers registered tooa particular time schedule.</span></span>

    $scheduleName = "{Schedule Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -ScheduleName $scheduleName
    Write-Output $jobTriggers

### <a name="tooretrieve-job-triggers-bound-tooa-job"></a><span data-ttu-id="679b1-347">tooretrieve feladat eseményindítók kötött tooa feladat</span><span class="sxs-lookup"><span data-stu-id="679b1-347">tooretrieve job triggers bound tooa job</span></span>
<span data-ttu-id="679b1-348">Használjon [Get-AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/get-azuresqljobtrigger) egy regisztrált feladatot tartalmazó tooobtain és megjelenítési ütemezéseket.</span><span class="sxs-lookup"><span data-stu-id="679b1-348">Use [Get-AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/get-azuresqljobtrigger) tooobtain and display schedules containing a registered job.</span></span>

    $jobName = "{Job Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -JobName $jobName
    Write-Output $jobTriggers

## <a name="toocreate-a-data-tier-application-dacpac-for-execution-across-databases"></a><span data-ttu-id="679b1-349">az adatbázisok közötti végrehajtásra adatrétegbeli alkalmazás (DACPAC) toocreate</span><span class="sxs-lookup"><span data-stu-id="679b1-349">toocreate a data-tier application (DACPAC) for execution across databases</span></span>
<span data-ttu-id="679b1-350">egy DACPAC toocreate lásd [Adatrétegbeli alkalmazások](https://msdn.microsoft.com/library/ee210546.aspx).</span><span class="sxs-lookup"><span data-stu-id="679b1-350">toocreate a DACPAC, see [Data-Tier applications](https://msdn.microsoft.com/library/ee210546.aspx).</span></span> <span data-ttu-id="679b1-351">egy DACPAC toodeploy hello használata [New-AzureSqlJobContent parancsmag](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent).</span><span class="sxs-lookup"><span data-stu-id="679b1-351">toodeploy a DACPAC, use hello [New-AzureSqlJobContent cmdlet](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent).</span></span> <span data-ttu-id="679b1-352">hello DACPAC elérhető toohello szolgáltatásnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="679b1-352">hello DACPAC must be accessible toohello service.</span></span> <span data-ttu-id="679b1-353">Az ajánlott tooupload létrehozott DACPAC tooAzure tárolási, és hozzon létre egy [közös hozzáférésű Jogosultságkód](../storage/common/storage-dotnet-shared-access-signature-part-1.md) hello DACPAC számára.</span><span class="sxs-lookup"><span data-stu-id="679b1-353">It is recommended tooupload a created DACPAC tooAzure Storage and create a [Shared Access Signature](../storage/common/storage-dotnet-shared-access-signature-part-1.md) for hello DACPAC.</span></span>

    $dacpacUri = "{Uri}"
    $dacpacName = "{Dacpac Name}"
    $dacpac = New-AzureSqlJobContent -DacpacUri $dacpacUri -ContentName $dacpacName 
    Write-Output $dacpac

### <a name="tooupdate-a-data-tier-application-dacpac-for-execution-across-databases"></a><span data-ttu-id="679b1-354">az adatbázisok közötti végrehajtásra adatrétegbeli alkalmazás (DACPAC) tooupdate</span><span class="sxs-lookup"><span data-stu-id="679b1-354">tooupdate a data-tier application (DACPAC) for execution across databases</span></span>
<span data-ttu-id="679b1-355">Rugalmas adatbázis-feladatok belül regisztrálva meglévő DACPACs frissített toopoint toonew URI lehet.</span><span class="sxs-lookup"><span data-stu-id="679b1-355">Existing DACPACs registered within Elastic Database Jobs can be updated toopoint toonew URIs.</span></span> <span data-ttu-id="679b1-356">Használjon hello [ **Set-AzureSqlJobContentDefinition parancsmag** ](/powershell/module/elasticdatabasejobs/set-azuresqljobcontentdefinition) tooupdate hello DACPAC URI egy olyan regisztrált DACPAC:</span><span class="sxs-lookup"><span data-stu-id="679b1-356">Use hello [**Set-AzureSqlJobContentDefinition cmdlet**](/powershell/module/elasticdatabasejobs/set-azuresqljobcontentdefinition) tooupdate hello DACPAC URI on an existing registered DACPAC:</span></span>

    $dacpacName = "{Dacpac Name}"
    $newDacpacUri = "{Uri}"
    $updatedDacpac = Set-AzureSqlJobDacpacDefinition -ContentName $dacpacName -DacpacUri $newDacpacUri
    Write-Output $updatedDacpac

## <a name="toocreate-a-job-tooapply-a-data-tier-application-dacpac-across-databases"></a><span data-ttu-id="679b1-357">egy feladat tooapply egy adatrétegbeli alkalmazás (DACPAC) az adatbázisok közötti toocreate</span><span class="sxs-lookup"><span data-stu-id="679b1-357">toocreate a job tooapply a data-tier application (DACPAC) across databases</span></span>
<span data-ttu-id="679b1-358">Miután egy DACPAC rugalmas adatbázis-feladatok belül létrehozott, egy feladat hozhatók létre tooapply hello DACPAC adatbázisok csoportja között.</span><span class="sxs-lookup"><span data-stu-id="679b1-358">After a DACPAC has been created within Elastic Database Jobs, a job can be created tooapply hello DACPAC across a group of databases.</span></span> <span data-ttu-id="679b1-359">a következő PowerShell-parancsfájl hello használt toocreate DACPAC feladat lehet egy egyéni gyűjtéshez. az adatbázisok között:</span><span class="sxs-lookup"><span data-stu-id="679b1-359">hello following PowerShell script can be used toocreate a DACPAC job across a custom collection of databases:</span></span>

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
