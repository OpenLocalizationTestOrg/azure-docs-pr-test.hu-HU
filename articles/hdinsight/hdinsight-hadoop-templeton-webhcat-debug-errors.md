---
title: "Ismertetés és hárítsa el a WebHCat-hibákat a HDInsight - Azure |} Microsoft Docs"
description: "Ismerje meg, hogyan körülbelül gyakori hibák által visszaadott WebHCat a HDInsight és azok megoldását."
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
ms.openlocfilehash: 6d8162e0d64ec9fc42690392b7c822593c0c2767
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="understand-and-resolve-errors-received-from-webhcat-on-hdinsight"></a><span data-ttu-id="d6781-103">Ismertetés és a HDInsight WebHCat hibák megoldásához</span><span class="sxs-lookup"><span data-stu-id="d6781-103">Understand and resolve errors received from WebHCat on HDInsight</span></span>

<span data-ttu-id="d6781-104">További információk a HDInsight, és azok megoldását WebHCat használatakor hibák.</span><span class="sxs-lookup"><span data-stu-id="d6781-104">Learn about errors received when using WebHCat with HDInsight, and how to resolve them.</span></span> <span data-ttu-id="d6781-105">WebHCat belsőleg ügyféloldali eszközök, például Azure PowerShell és a Data Lake Tools for Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d6781-105">WebHCat is used internally by client-side tools such as Azure PowerShell and the Data Lake Tools for Visual Studio.</span></span>

## <a name="what-is-webhcat"></a><span data-ttu-id="d6781-106">Mi az a WebHCat</span><span class="sxs-lookup"><span data-stu-id="d6781-106">What is WebHCat</span></span>

<span data-ttu-id="d6781-107">[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) egy REST API a [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog), egy tábla és a Hadoop tárolási alkalmazáskezelési réteg.</span><span class="sxs-lookup"><span data-stu-id="d6781-107">[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) is a REST API for [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog), a table, and storage management layer for Hadoop.</span></span> <span data-ttu-id="d6781-108">WebHCat a HDInsight-fürtökön alapértelmezés szerint engedélyezve van, és segítségével különböző eszközök feladatokat küldhet el, beolvasni a feladat állapotát, stb. a fürthöz való bejelentkezés nélkül.</span><span class="sxs-lookup"><span data-stu-id="d6781-108">WebHCat is enabled by default on HDInsight clusters, and is used by various tools to submit jobs, get job status, etc. without logging in to the cluster.</span></span>

## <a name="modifying-configuration"></a><span data-ttu-id="d6781-109">Konfigurációjának módosítása</span><span class="sxs-lookup"><span data-stu-id="d6781-109">Modifying configuration</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d6781-110">A jelen dokumentumban felsorolt hibák számos fordulhat elő, mert túllépte a beállított maximális.</span><span class="sxs-lookup"><span data-stu-id="d6781-110">Several of the errors listed in this document occur because a configured maximum has been exceeded.</span></span> <span data-ttu-id="d6781-111">A feloldási lépés akkor említi, hogy egy érték módosítható, ha kell használnia az alábbiak egyikét a módosítás végrehajtásához:</span><span class="sxs-lookup"><span data-stu-id="d6781-111">When the resolution step mentions that you can change a value, you must use one of the following to perform the change:</span></span>

* <span data-ttu-id="d6781-112">A **Windows** fürtök: egy Parancsfájlművelettel az érték beállítása a fürt létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="d6781-112">For **Windows** clusters: Use a script action to configure the value during cluster creation.</span></span> <span data-ttu-id="d6781-113">További információkért lásd: [Parancsfájlműveletek fejlesztése](hdinsight-hadoop-script-actions.md).</span><span class="sxs-lookup"><span data-stu-id="d6781-113">For more information, see [Develop script actions](hdinsight-hadoop-script-actions.md).</span></span>

* <span data-ttu-id="d6781-114">A **Linux** fürtök: használata Ambari (web vagy a REST API) értékét módosítani.</span><span class="sxs-lookup"><span data-stu-id="d6781-114">For **Linux** clusters: Use Ambari (web or REST API) to modify the value.</span></span> <span data-ttu-id="d6781-115">További információkért lásd: [kezelése HDInsight Ambari használatával](hdinsight-hadoop-manage-ambari.md)</span><span class="sxs-lookup"><span data-stu-id="d6781-115">For more information, see [Manage HDInsight using Ambari](hdinsight-hadoop-manage-ambari.md)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d6781-116">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="d6781-116">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="d6781-117">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="d6781-117">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

### <a name="default-configuration"></a><span data-ttu-id="d6781-118">Alapértelmezett konfigurációja</span><span class="sxs-lookup"><span data-stu-id="d6781-118">Default configuration</span></span>

<span data-ttu-id="d6781-119">Ha a következő alapértelmezett értékek számát, azt WebHCat teljesítményét, vagy hibákat okozhatnak:</span><span class="sxs-lookup"><span data-stu-id="d6781-119">If the following default values are exceeded, it can degrade WebHCat performance or cause errors:</span></span>

| <span data-ttu-id="d6781-120">Beállítás</span><span class="sxs-lookup"><span data-stu-id="d6781-120">Setting</span></span> | <span data-ttu-id="d6781-121">Funkció</span><span class="sxs-lookup"><span data-stu-id="d6781-121">What it does</span></span> | <span data-ttu-id="d6781-122">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="d6781-122">Default value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d6781-123">[yarn.Scheduler.Capacity.maximum-alkalmazások][maximum-applications]</span><span class="sxs-lookup"><span data-stu-id="d6781-123">[yarn.scheduler.capacity.maximum-applications][maximum-applications]</span></span> |<span data-ttu-id="d6781-124">Az aktív lehet egyidejű feladatok maximális száma (folyamatban vagy fut)</span><span class="sxs-lookup"><span data-stu-id="d6781-124">The maximum number of jobs that can be active concurrently (pending or running)</span></span> |<span data-ttu-id="d6781-125">10,000</span><span class="sxs-lookup"><span data-stu-id="d6781-125">10,000</span></span> |
| <span data-ttu-id="d6781-126">[templeton.Exec.max-procs][max-procs]</span><span class="sxs-lookup"><span data-stu-id="d6781-126">[templeton.exec.max-procs][max-procs]</span></span> |<span data-ttu-id="d6781-127">Egyidejűleg szolgáltatható kérelmek maximális száma</span><span class="sxs-lookup"><span data-stu-id="d6781-127">The maximum number of requests that can be served concurrently</span></span> |<span data-ttu-id="d6781-128">20</span><span class="sxs-lookup"><span data-stu-id="d6781-128">20</span></span> |
| <span data-ttu-id="d6781-129">[mapreduce.jobhistory.max-kor-ms][max-age-ms]</span><span class="sxs-lookup"><span data-stu-id="d6781-129">[mapreduce.jobhistory.max-age-ms][max-age-ms]</span></span> |<span data-ttu-id="d6781-130">Az, hogy hány napig feladatelőzmények megmaradnak</span><span class="sxs-lookup"><span data-stu-id="d6781-130">The number of days that job history are retained</span></span> |<span data-ttu-id="d6781-131">7 nap</span><span class="sxs-lookup"><span data-stu-id="d6781-131">7 days</span></span> |

## <a name="too-many-requests"></a><span data-ttu-id="d6781-132">Túl sok kérelem</span><span class="sxs-lookup"><span data-stu-id="d6781-132">Too many requests</span></span>

<span data-ttu-id="d6781-133">**HTTP-állapotkód**: 429</span><span class="sxs-lookup"><span data-stu-id="d6781-133">**HTTP Status code**: 429</span></span>

| <span data-ttu-id="d6781-134">Ok</span><span class="sxs-lookup"><span data-stu-id="d6781-134">Cause</span></span> | <span data-ttu-id="d6781-135">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="d6781-135">Resolution</span></span> |
| --- | --- |
| <span data-ttu-id="d6781-136">Túllépte a maximális egyidejű kérelmek / perc (alapértelmezett érték 20) WebHCat által kiszolgált</span><span class="sxs-lookup"><span data-stu-id="d6781-136">You have exceeded the maximum concurrent requests served by WebHCat per minute (default 20)</span></span> |<span data-ttu-id="d6781-137">Csökkentse a munkaterhelés győződjön meg arról, hogy Ön nem küldenek több, mint az egyidejű kérelmek maximális számát, vagy növelje az egyidejűleg futtatható kérelmek maximális módosításával `templeton.exec.max-procs`.</span><span class="sxs-lookup"><span data-stu-id="d6781-137">Reduce your workload to ensure that you do not submit more than the maximum number of concurrent requests or increase the concurrent request limit by modifying `templeton.exec.max-procs`.</span></span> <span data-ttu-id="d6781-138">További információkért lásd: [konfiguráció módosítása](#modifying-configuration)</span><span class="sxs-lookup"><span data-stu-id="d6781-138">For more information, see [Modifying configuration](#modifying-configuration)</span></span> |

## <a name="server-unavailable"></a><span data-ttu-id="d6781-139">A kiszolgáló nem érhető el</span><span class="sxs-lookup"><span data-stu-id="d6781-139">Server unavailable</span></span>

<span data-ttu-id="d6781-140">**HTTP-állapotkód**: 503-as</span><span class="sxs-lookup"><span data-stu-id="d6781-140">**HTTP Status code**: 503</span></span>

| <span data-ttu-id="d6781-141">Ok</span><span class="sxs-lookup"><span data-stu-id="d6781-141">Cause</span></span> | <span data-ttu-id="d6781-142">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="d6781-142">Resolution</span></span> |
| --- | --- |
| <span data-ttu-id="d6781-143">Ez az állapot kód általában akkor fordul elő, az elsődleges és másodlagos HeadNode a fürt közötti feladatátvétel során</span><span class="sxs-lookup"><span data-stu-id="d6781-143">This status code usually occurs during failover between the primary and secondary HeadNode for the cluster</span></span> |<span data-ttu-id="d6781-144">Várjon két percet, majd próbálja megismételni a műveletet</span><span class="sxs-lookup"><span data-stu-id="d6781-144">Wait two minutes, then retry the operation</span></span> |

## <a name="bad-request-content-could-not-find-job"></a><span data-ttu-id="d6781-145">Hibás kérés tartalma: nem található feladat</span><span class="sxs-lookup"><span data-stu-id="d6781-145">Bad request Content: Could not find job</span></span>

<span data-ttu-id="d6781-146">**HTTP-állapotkód**: 400</span><span class="sxs-lookup"><span data-stu-id="d6781-146">**HTTP Status code**: 400</span></span>

| <span data-ttu-id="d6781-147">Ok</span><span class="sxs-lookup"><span data-stu-id="d6781-147">Cause</span></span> | <span data-ttu-id="d6781-148">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="d6781-148">Resolution</span></span> |
| --- | --- |
| <span data-ttu-id="d6781-149">Feladat részleteinek törölve lettek a feladatelőzmények által tisztító</span><span class="sxs-lookup"><span data-stu-id="d6781-149">Job details have been cleaned up by the job history cleaner</span></span> |<span data-ttu-id="d6781-150">Feladatelőzmények megőrzési idő alapértelmezés szerint 7 nap.</span><span class="sxs-lookup"><span data-stu-id="d6781-150">The default retention period for job history is 7 days.</span></span> <span data-ttu-id="d6781-151">Az alapértelmezett megőrzési időtartamot a módosításával lehet megváltoztatni `mapreduce.jobhistory.max-age-ms`.</span><span class="sxs-lookup"><span data-stu-id="d6781-151">The default retention period can be changed by modifying `mapreduce.jobhistory.max-age-ms`.</span></span> <span data-ttu-id="d6781-152">További információkért lásd: [konfiguráció módosítása](#modifying-configuration)</span><span class="sxs-lookup"><span data-stu-id="d6781-152">For more information, see [Modifying configuration](#modifying-configuration)</span></span> |
| <span data-ttu-id="d6781-153">Feladat leállította a feladatátvétel miatt</span><span class="sxs-lookup"><span data-stu-id="d6781-153">Job has been killed due to a failover</span></span> |<span data-ttu-id="d6781-154">Ismételje meg a feladat elküldése legfeljebb két percig</span><span class="sxs-lookup"><span data-stu-id="d6781-154">Retry job submission for up to two minutes</span></span> |
| <span data-ttu-id="d6781-155">Feladatazonosító érvénytelen lett megadva.</span><span class="sxs-lookup"><span data-stu-id="d6781-155">An Invalid job id was used</span></span> |<span data-ttu-id="d6781-156">Annak ellenőrzése, hogy a feladat azonosítója helyes-e</span><span class="sxs-lookup"><span data-stu-id="d6781-156">Check if the job id is correct</span></span> |

## <a name="bad-gateway"></a><span data-ttu-id="d6781-157">Hibás átjáró</span><span class="sxs-lookup"><span data-stu-id="d6781-157">Bad gateway</span></span>

<span data-ttu-id="d6781-158">**HTTP-állapotkód**: 502-es</span><span class="sxs-lookup"><span data-stu-id="d6781-158">**HTTP Status code**: 502</span></span>

| <span data-ttu-id="d6781-159">Ok</span><span class="sxs-lookup"><span data-stu-id="d6781-159">Cause</span></span> | <span data-ttu-id="d6781-160">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="d6781-160">Resolution</span></span> |
| --- | --- |
| <span data-ttu-id="d6781-161">Belső szemétgyűjtés folyamatban van a WebHCat folyamaton belül</span><span class="sxs-lookup"><span data-stu-id="d6781-161">Internal garbage collection is occurring within the WebHCat process</span></span> |<span data-ttu-id="d6781-162">Várjon, amíg a szemétgyűjtő befejeződését, vagy indítsa újra a WebHCat szolgáltatást</span><span class="sxs-lookup"><span data-stu-id="d6781-162">Wait for garbage collection to finish or restart the WebHCat service</span></span> |
| <span data-ttu-id="d6781-163">Az erőforrás-kezelő szolgáltatás válaszára várakozás időkorlátja lejárt.</span><span class="sxs-lookup"><span data-stu-id="d6781-163">Time out waiting on a response from the ResourceManager service.</span></span> <span data-ttu-id="d6781-164">Ez a hiba akkor fordulhat elő, amikor aktív kérelmek száma a megadott maximális értéket (alapértelmezett érték 10 000)</span><span class="sxs-lookup"><span data-stu-id="d6781-164">This error can occur when the number of active applications goes the configured maximum (default 10,000)</span></span> |<span data-ttu-id="d6781-165">Várjon, amíg a jelenleg futó feladat befejeződik, vagy növelje az egyidejű feladat korlát módosításával `yarn.scheduler.capacity.maximum-applications`.</span><span class="sxs-lookup"><span data-stu-id="d6781-165">Wait for currently running jobs to complete or increase the concurrent job limit by modifying `yarn.scheduler.capacity.maximum-applications`.</span></span> <span data-ttu-id="d6781-166">További információkért lásd: a [módosítása konfigurációs](#modifying-configuration) szakasz.</span><span class="sxs-lookup"><span data-stu-id="d6781-166">For more information, see the [Modifying configuration](#modifying-configuration) section.</span></span> |
| <span data-ttu-id="d6781-167">Minden feladat keresztül beolvasására tett kísérlet a [GET /jobs](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs) hívás közben `Fields` beállítása`*`</span><span class="sxs-lookup"><span data-stu-id="d6781-167">Attempting to retrieve all jobs through the [GET /jobs](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs) call while `Fields` is set to `*`</span></span> |<span data-ttu-id="d6781-168">Nem beolvasni a *összes* feladat részletei.</span><span class="sxs-lookup"><span data-stu-id="d6781-168">Do not retrieve *all* job details.</span></span> <span data-ttu-id="d6781-169">Ehelyett használja `jobid` beolvasni csak nagyobb, mint egyes feladatazonosítót a feladat részleteit.</span><span class="sxs-lookup"><span data-stu-id="d6781-169">Instead use `jobid` to retrieve details for jobs only greater than certain job id.</span></span> <span data-ttu-id="d6781-170">Vagy, ne használja`Fields`</span><span class="sxs-lookup"><span data-stu-id="d6781-170">Or, do not use `Fields`</span></span> |
| <span data-ttu-id="d6781-171">A WebHCat-szolgáltatás nem működik HeadNode feladatátvétel során</span><span class="sxs-lookup"><span data-stu-id="d6781-171">The WebHCat service is down during HeadNode failover</span></span> |<span data-ttu-id="d6781-172">Két perc várakozás, majd próbálja megismételni a műveletet</span><span class="sxs-lookup"><span data-stu-id="d6781-172">Wait for two minutes and retry the operation</span></span> |
| <span data-ttu-id="d6781-173">Webhcaten keresztül küldött 500-nál több függőben lévő feladatok</span><span class="sxs-lookup"><span data-stu-id="d6781-173">There are more than 500 pending jobs submitted through WebHCat</span></span> |<span data-ttu-id="d6781-174">Várjon, amíg jelenleg függő feladatok már befejeződtek több feladat elküldése előtt</span><span class="sxs-lookup"><span data-stu-id="d6781-174">Wait until currently pending jobs have completed before submitting more jobs</span></span> |

[maximum-applications]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.3/bk_system-admin-guide/content/setting_application_limits.html
[max-procs]: https://hive.apache.org/javadocs/hcat-r0.5.0/configuration.html
[max-age-ms]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.6.0/ds_Hadoop/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml
