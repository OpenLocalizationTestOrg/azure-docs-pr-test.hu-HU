---
title: "aaaUnderstand és hárítsa el a HDInsight - Azure WebHCat-hibák |} Microsoft Docs"
description: "Megtudhatja, hogyan hdinsight WebHCat által visszaadott tooabout előforduló hibákat, és hogyan tooresolve őket."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 1b3d94b1-207d-4550-aece-21dc45485549
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/26/2017
ms.author: larryfr
ms.openlocfilehash: 0071a1e9ed448ae146b93c8f4f518e31b95d27c9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understand-and-resolve-errors-received-from-webhcat-on-hdinsight"></a><span data-ttu-id="57a2f-103">Ismertetés és a HDInsight WebHCat hibák megoldásához</span><span class="sxs-lookup"><span data-stu-id="57a2f-103">Understand and resolve errors received from WebHCat on HDInsight</span></span>

<span data-ttu-id="57a2f-104">Tudnivalók a WebHCat használata a hdinsight eszközzel, és hogyan hibák tooresolve őket.</span><span class="sxs-lookup"><span data-stu-id="57a2f-104">Learn about errors received when using WebHCat with HDInsight, and how tooresolve them.</span></span> <span data-ttu-id="57a2f-105">WebHCat belső használatára szolgál az ügyféloldali eszközök például az Azure PowerShell és hello Data Lake Tools for Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="57a2f-105">WebHCat is used internally by client-side tools such as Azure PowerShell and hello Data Lake Tools for Visual Studio.</span></span>

## <a name="what-is-webhcat"></a><span data-ttu-id="57a2f-106">Mi az a WebHCat</span><span class="sxs-lookup"><span data-stu-id="57a2f-106">What is WebHCat</span></span>

<span data-ttu-id="57a2f-107">[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) egy REST API a [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog), egy tábla és a Hadoop tárolási alkalmazáskezelési réteg.</span><span class="sxs-lookup"><span data-stu-id="57a2f-107">[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) is a REST API for [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog), a table, and storage management layer for Hadoop.</span></span> <span data-ttu-id="57a2f-108">WebHCat a HDInsight-fürtökön alapértelmezés szerint engedélyezve van, és különböző eszközök toosubmit feladatok által használt, majd a feladat állapotát és hasonló toohello fürt naplózása nélkül.</span><span class="sxs-lookup"><span data-stu-id="57a2f-108">WebHCat is enabled by default on HDInsight clusters, and is used by various tools toosubmit jobs, get job status, etc. without logging in toohello cluster.</span></span>

## <a name="modifying-configuration"></a><span data-ttu-id="57a2f-109">Konfigurációjának módosítása</span><span class="sxs-lookup"><span data-stu-id="57a2f-109">Modifying configuration</span></span>

> [!IMPORTANT]
> <span data-ttu-id="57a2f-110">A jelen dokumentumban szereplő hello hibák számos fordulhat elő, mert túllépte a beállított maximális.</span><span class="sxs-lookup"><span data-stu-id="57a2f-110">Several of hello errors listed in this document occur because a configured maximum has been exceeded.</span></span> <span data-ttu-id="57a2f-111">Hello feloldási lépés akkor említi, hogy egy érték módosítható, ha Ön hello tooperform hello módosítása a következő egyikét kell használnia:</span><span class="sxs-lookup"><span data-stu-id="57a2f-111">When hello resolution step mentions that you can change a value, you must use one of hello following tooperform hello change:</span></span>

* <span data-ttu-id="57a2f-112">A **Windows** fürtök: parancsfájl művelet tooconfigure hello értéket használjon a fürt létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="57a2f-112">For **Windows** clusters: Use a script action tooconfigure hello value during cluster creation.</span></span> <span data-ttu-id="57a2f-113">További információkért lásd: [Parancsfájlműveletek fejlesztése](hdinsight-hadoop-script-actions.md).</span><span class="sxs-lookup"><span data-stu-id="57a2f-113">For more information, see [Develop script actions](hdinsight-hadoop-script-actions.md).</span></span>

* <span data-ttu-id="57a2f-114">A **Linux** fürtök: (web- vagy REST API-t) használja Ambari toomodify hello érték.</span><span class="sxs-lookup"><span data-stu-id="57a2f-114">For **Linux** clusters: Use Ambari (web or REST API) toomodify hello value.</span></span> <span data-ttu-id="57a2f-115">További információkért lásd: [kezelése HDInsight Ambari használatával](hdinsight-hadoop-manage-ambari.md)</span><span class="sxs-lookup"><span data-stu-id="57a2f-115">For more information, see [Manage HDInsight using Ambari](hdinsight-hadoop-manage-ambari.md)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="57a2f-116">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="57a2f-116">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="57a2f-117">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="57a2f-117">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

### <a name="default-configuration"></a><span data-ttu-id="57a2f-118">Alapértelmezett konfigurációja</span><span class="sxs-lookup"><span data-stu-id="57a2f-118">Default configuration</span></span>

<span data-ttu-id="57a2f-119">Ha hello a következő alapértelmezett értékek számát, azt WebHCat teljesítményét, vagy hibákat okozhatnak:</span><span class="sxs-lookup"><span data-stu-id="57a2f-119">If hello following default values are exceeded, it can degrade WebHCat performance or cause errors:</span></span>

| <span data-ttu-id="57a2f-120">Beállítás</span><span class="sxs-lookup"><span data-stu-id="57a2f-120">Setting</span></span> | <span data-ttu-id="57a2f-121">Funkció</span><span class="sxs-lookup"><span data-stu-id="57a2f-121">What it does</span></span> | <span data-ttu-id="57a2f-122">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="57a2f-122">Default value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="57a2f-123">[yarn.Scheduler.Capacity.maximum-alkalmazások][maximum-applications]</span><span class="sxs-lookup"><span data-stu-id="57a2f-123">[yarn.scheduler.capacity.maximum-applications][maximum-applications]</span></span> |<span data-ttu-id="57a2f-124">hello aktív lehet egyidejű feladatok maximális száma (folyamatban vagy fut)</span><span class="sxs-lookup"><span data-stu-id="57a2f-124">hello maximum number of jobs that can be active concurrently (pending or running)</span></span> |<span data-ttu-id="57a2f-125">10,000</span><span class="sxs-lookup"><span data-stu-id="57a2f-125">10,000</span></span> |
| <span data-ttu-id="57a2f-126">[templeton.Exec.max-procs][max-procs]</span><span class="sxs-lookup"><span data-stu-id="57a2f-126">[templeton.exec.max-procs][max-procs]</span></span> |<span data-ttu-id="57a2f-127">hello egyidejűleg szolgáltatható kérelmek maximális száma</span><span class="sxs-lookup"><span data-stu-id="57a2f-127">hello maximum number of requests that can be served concurrently</span></span> |<span data-ttu-id="57a2f-128">20</span><span class="sxs-lookup"><span data-stu-id="57a2f-128">20</span></span> |
| <span data-ttu-id="57a2f-129">[mapreduce.jobhistory.max-kor-ms][max-age-ms]</span><span class="sxs-lookup"><span data-stu-id="57a2f-129">[mapreduce.jobhistory.max-age-ms][max-age-ms]</span></span> |<span data-ttu-id="57a2f-130">Hello, hogy hány napig feladatelőzmények megmaradnak</span><span class="sxs-lookup"><span data-stu-id="57a2f-130">hello number of days that job history are retained</span></span> |<span data-ttu-id="57a2f-131">7 nap</span><span class="sxs-lookup"><span data-stu-id="57a2f-131">7 days</span></span> |

## <a name="too-many-requests"></a><span data-ttu-id="57a2f-132">Túl sok kérelem</span><span class="sxs-lookup"><span data-stu-id="57a2f-132">Too many requests</span></span>

<span data-ttu-id="57a2f-133">**HTTP-állapotkód**: 429</span><span class="sxs-lookup"><span data-stu-id="57a2f-133">**HTTP Status code**: 429</span></span>

| <span data-ttu-id="57a2f-134">Ok</span><span class="sxs-lookup"><span data-stu-id="57a2f-134">Cause</span></span> | <span data-ttu-id="57a2f-135">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="57a2f-135">Resolution</span></span> |
| --- | --- |
| <span data-ttu-id="57a2f-136">Túllépte a hello maximális párhuzamos által kiszolgált kérelmek WebHCat / perc (alapértelmezett érték 20)</span><span class="sxs-lookup"><span data-stu-id="57a2f-136">You have exceeded hello maximum concurrent requests served by WebHCat per minute (default 20)</span></span> |<span data-ttu-id="57a2f-137">Csökkentse a munkaterhelés tooensure, hogy Ön nem nyújt több mint hello egyidejű kérelmek maximális számát, vagy növelje hello egyidejűleg futtatható kérelmek maximális módosításával `templeton.exec.max-procs`.</span><span class="sxs-lookup"><span data-stu-id="57a2f-137">Reduce your workload tooensure that you do not submit more than hello maximum number of concurrent requests or increase hello concurrent request limit by modifying `templeton.exec.max-procs`.</span></span> <span data-ttu-id="57a2f-138">További információkért lásd: [konfiguráció módosítása](#modifying-configuration)</span><span class="sxs-lookup"><span data-stu-id="57a2f-138">For more information, see [Modifying configuration](#modifying-configuration)</span></span> |

## <a name="server-unavailable"></a><span data-ttu-id="57a2f-139">A kiszolgáló nem érhető el</span><span class="sxs-lookup"><span data-stu-id="57a2f-139">Server unavailable</span></span>

<span data-ttu-id="57a2f-140">**HTTP-állapotkód**: 503-as</span><span class="sxs-lookup"><span data-stu-id="57a2f-140">**HTTP Status code**: 503</span></span>

| <span data-ttu-id="57a2f-141">Ok</span><span class="sxs-lookup"><span data-stu-id="57a2f-141">Cause</span></span> | <span data-ttu-id="57a2f-142">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="57a2f-142">Resolution</span></span> |
| --- | --- |
| <span data-ttu-id="57a2f-143">Ez az állapot kód általában akkor fordul elő, hello elsődleges és másodlagos közötti feladatátvételkor HeadNode hello fürt</span><span class="sxs-lookup"><span data-stu-id="57a2f-143">This status code usually occurs during failover between hello primary and secondary HeadNode for hello cluster</span></span> |<span data-ttu-id="57a2f-144">Várjon két percet, majd próbálja megismételni a műveletet hello</span><span class="sxs-lookup"><span data-stu-id="57a2f-144">Wait two minutes, then retry hello operation</span></span> |

## <a name="bad-request-content-could-not-find-job"></a><span data-ttu-id="57a2f-145">Hibás kérés tartalma: nem található feladat</span><span class="sxs-lookup"><span data-stu-id="57a2f-145">Bad request Content: Could not find job</span></span>

<span data-ttu-id="57a2f-146">**HTTP-állapotkód**: 400</span><span class="sxs-lookup"><span data-stu-id="57a2f-146">**HTTP Status code**: 400</span></span>

| <span data-ttu-id="57a2f-147">Ok</span><span class="sxs-lookup"><span data-stu-id="57a2f-147">Cause</span></span> | <span data-ttu-id="57a2f-148">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="57a2f-148">Resolution</span></span> |
| --- | --- |
| <span data-ttu-id="57a2f-149">Feladat részleteinek törölve lettek által hello feladatelőzmények tisztító</span><span class="sxs-lookup"><span data-stu-id="57a2f-149">Job details have been cleaned up by hello job history cleaner</span></span> |<span data-ttu-id="57a2f-150">hello alapértelmezett megőrzési időtartamot feladatelőzmények 7 nap.</span><span class="sxs-lookup"><span data-stu-id="57a2f-150">hello default retention period for job history is 7 days.</span></span> <span data-ttu-id="57a2f-151">hello alapértelmezett megőrzési időtartamot a módosításával lehet megváltoztatni `mapreduce.jobhistory.max-age-ms`.</span><span class="sxs-lookup"><span data-stu-id="57a2f-151">hello default retention period can be changed by modifying `mapreduce.jobhistory.max-age-ms`.</span></span> <span data-ttu-id="57a2f-152">További információkért lásd: [konfiguráció módosítása](#modifying-configuration)</span><span class="sxs-lookup"><span data-stu-id="57a2f-152">For more information, see [Modifying configuration](#modifying-configuration)</span></span> |
| <span data-ttu-id="57a2f-153">Feladat leállította tooa feladatátvétel miatt</span><span class="sxs-lookup"><span data-stu-id="57a2f-153">Job has been killed due tooa failover</span></span> |<span data-ttu-id="57a2f-154">Ismételje meg a feladat elküldése a mentést tootwo perc</span><span class="sxs-lookup"><span data-stu-id="57a2f-154">Retry job submission for up tootwo minutes</span></span> |
| <span data-ttu-id="57a2f-155">Feladatazonosító érvénytelen lett megadva.</span><span class="sxs-lookup"><span data-stu-id="57a2f-155">An Invalid job id was used</span></span> |<span data-ttu-id="57a2f-156">Ellenőrizze, hogy helyes-e-e hello feladatazonosító:</span><span class="sxs-lookup"><span data-stu-id="57a2f-156">Check if hello job id is correct</span></span> |

## <a name="bad-gateway"></a><span data-ttu-id="57a2f-157">Hibás átjáró</span><span class="sxs-lookup"><span data-stu-id="57a2f-157">Bad gateway</span></span>

<span data-ttu-id="57a2f-158">**HTTP-állapotkód**: 502-es</span><span class="sxs-lookup"><span data-stu-id="57a2f-158">**HTTP Status code**: 502</span></span>

| <span data-ttu-id="57a2f-159">Ok</span><span class="sxs-lookup"><span data-stu-id="57a2f-159">Cause</span></span> | <span data-ttu-id="57a2f-160">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="57a2f-160">Resolution</span></span> |
| --- | --- |
| <span data-ttu-id="57a2f-161">Belső szemétgyűjtés belül hello WebHCat folyamat folyamatban van</span><span class="sxs-lookup"><span data-stu-id="57a2f-161">Internal garbage collection is occurring within hello WebHCat process</span></span> |<span data-ttu-id="57a2f-162">Szemétgyűjtési gyűjtemény toofinish várja meg, vagy hello WebHCat szolgáltatás újraindítása</span><span class="sxs-lookup"><span data-stu-id="57a2f-162">Wait for garbage collection toofinish or restart hello WebHCat service</span></span> |
| <span data-ttu-id="57a2f-163">Erőforrás-kezelő szolgáltatás hello válaszára várakozás időkorlátja lejárt.</span><span class="sxs-lookup"><span data-stu-id="57a2f-163">Time out waiting on a response from hello ResourceManager service.</span></span> <span data-ttu-id="57a2f-164">Ez a hiba akkor fordulhat elő, amikor aktív kérelmek száma hello konfigurált hello maximálisan engedélyezett (alapértelmezés szerint 10 000)</span><span class="sxs-lookup"><span data-stu-id="57a2f-164">This error can occur when hello number of active applications goes hello configured maximum (default 10,000)</span></span> |<span data-ttu-id="57a2f-165">Várjon, amíg a jelenleg futó feladatok toocomplete, vagy növelje a hello egyidejű feladat korlát módosításával `yarn.scheduler.capacity.maximum-applications`.</span><span class="sxs-lookup"><span data-stu-id="57a2f-165">Wait for currently running jobs toocomplete or increase hello concurrent job limit by modifying `yarn.scheduler.capacity.maximum-applications`.</span></span> <span data-ttu-id="57a2f-166">További információkért lásd: hello [módosítása konfigurációs](#modifying-configuration) szakasz.</span><span class="sxs-lookup"><span data-stu-id="57a2f-166">For more information, see hello [Modifying configuration](#modifying-configuration) section.</span></span> |
| <span data-ttu-id="57a2f-167">Kísérlet tooretrieve keresztül hello összes feladat [GET /jobs](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs) hívás közben `Fields` túl van beállítva`*`</span><span class="sxs-lookup"><span data-stu-id="57a2f-167">Attempting tooretrieve all jobs through hello [GET /jobs](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs) call while `Fields` is set too`*`</span></span> |<span data-ttu-id="57a2f-168">Nem beolvasni a *összes* feladat részletei.</span><span class="sxs-lookup"><span data-stu-id="57a2f-168">Do not retrieve *all* job details.</span></span> <span data-ttu-id="57a2f-169">Ehelyett használja `jobid` feladatok csak az egyes feladatazonosítót nagyobb tooretrieve részleteit. Vagy, ne használja`Fields`</span><span class="sxs-lookup"><span data-stu-id="57a2f-169">Instead use `jobid` tooretrieve details for jobs only greater than certain job id. Or, do not use `Fields`</span></span> |
| <span data-ttu-id="57a2f-170">hello WebHCat-szolgáltatás nem működik HeadNode feladatátvétel során</span><span class="sxs-lookup"><span data-stu-id="57a2f-170">hello WebHCat service is down during HeadNode failover</span></span> |<span data-ttu-id="57a2f-171">Két perc várakozás, majd próbálja megismételni a műveletet hello</span><span class="sxs-lookup"><span data-stu-id="57a2f-171">Wait for two minutes and retry hello operation</span></span> |
| <span data-ttu-id="57a2f-172">Webhcaten keresztül küldött 500-nál több függőben lévő feladatok</span><span class="sxs-lookup"><span data-stu-id="57a2f-172">There are more than 500 pending jobs submitted through WebHCat</span></span> |<span data-ttu-id="57a2f-173">Várjon, amíg jelenleg függő feladatok már befejeződtek több feladat elküldése előtt</span><span class="sxs-lookup"><span data-stu-id="57a2f-173">Wait until currently pending jobs have completed before submitting more jobs</span></span> |

[maximum-applications]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.3/bk_system-admin-guide/content/setting_application_limits.html
[max-procs]: https://hive.apache.org/javadocs/hcat-r0.5.0/configuration.html
[max-age-ms]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.6.0/ds_Hadoop/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml
