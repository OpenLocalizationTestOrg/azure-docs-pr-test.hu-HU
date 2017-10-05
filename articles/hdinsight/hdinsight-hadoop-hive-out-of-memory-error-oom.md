---
title: "Hárítsa el a kevés a memória az Azure HDInsight Hive |} Microsoft Docs"
description: "Javítsa ki a kevés a memória hdinsight Hive. A forgatókönyv az a lekérdezés sok nagy táblák között."
keywords: "Hiba történt, a memória, Hive memóriabeállításait kívül"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 7bce3dff-9825-4fa0-a568-c52a9f7d1dad
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/17/2017
ms.author: jgao
ms.openlocfilehash: da1247070ade11f78b505524f5e970e18eb16d10
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="fix-a-hive-out-of-memory-error-in-azure-hdinsight"></a><span data-ttu-id="15eb0-105">Hárítsa el a kevés a memória az Azure HDInsight Hive</span><span class="sxs-lookup"><span data-stu-id="15eb0-105">Fix a Hive out of memory error in Azure HDInsight</span></span>

<span data-ttu-id="15eb0-106">Kevés a memória a struktúra kijavításához folyamat nagy táblák konfigurálásával memóriabeállításokat struktúra.</span><span class="sxs-lookup"><span data-stu-id="15eb0-106">Learn how to fix a Hive out of memory error when process large tables by configuring Hive memory settings.</span></span>

## <a name="run-hive-query-against-large-tables"></a><span data-ttu-id="15eb0-107">Nagy táblák Hive-lekérdezések futtatásához</span><span class="sxs-lookup"><span data-stu-id="15eb0-107">Run Hive query against large tables</span></span>

<span data-ttu-id="15eb0-108">Az ügyfél Hive-lekérdezések futtatása:</span><span class="sxs-lookup"><span data-stu-id="15eb0-108">A customer ran a Hive query:</span></span>

    SELECT
        COUNT (T1.COLUMN1) as DisplayColumn1,
        …
        …
        ….
    FROM
        TABLE1 T1,
        TABLE2 T2,
        TABLE3 T3,
        TABLE5 T4,
        TABLE6 T5,
        TABLE7 T6
    where (T1.KEY1 = T2.KEY1….
        …
        …

<span data-ttu-id="15eb0-109">A lekérdezés néhány apró:</span><span class="sxs-lookup"><span data-stu-id="15eb0-109">Some nuances of this query:</span></span>

* <span data-ttu-id="15eb0-110">A T1 nagy táblához, TABLE1, amelynek karakterlánc típusú oszlopokat rengeteg alias.</span><span class="sxs-lookup"><span data-stu-id="15eb0-110">T1 is an alias to a big table, TABLE1, which has lots of STRING column types.</span></span>
* <span data-ttu-id="15eb0-111">Más táblák, amely nem nagy, de rendelkezik sok oszlopot.</span><span class="sxs-lookup"><span data-stu-id="15eb0-111">Other tables are not that big but do have many columns.</span></span>
* <span data-ttu-id="15eb0-112">Minden olyan táblát csatlakozik egymáshoz, bizonyos esetekben több oszlopból álló TABLE1 és mások számára.</span><span class="sxs-lookup"><span data-stu-id="15eb0-112">All tables are joining each other, in some cases with multiple columns in TABLE1 and others.</span></span>

<span data-ttu-id="15eb0-113">A Hive-lekérdezés befejezéséhez 24 csomóponton a3 méretű HDInsight-fürt 26 percet vett igénybe.</span><span class="sxs-lookup"><span data-stu-id="15eb0-113">The Hive query took 26 minutes to finish on a 24 node A3 HDInsight cluster.</span></span> <span data-ttu-id="15eb0-114">Az ügyfél a következő figyelmeztető üzenetek első fellépése:</span><span class="sxs-lookup"><span data-stu-id="15eb0-114">The customer noticed the following warning messages:</span></span>

    Warning: Map Join MAPJOIN[428][bigTable=?] in task 'Stage-21:MAPRED' is a cross product
    Warning: Shuffle Join JOIN[8][tables = [t1933775, t1932766]] in Stage 'Stage-4:MAPRED' is a cross product

<span data-ttu-id="15eb0-115">A Tez végrehajtómotor használatával.</span><span class="sxs-lookup"><span data-stu-id="15eb0-115">By using the Tez execution engine.</span></span> <span data-ttu-id="15eb0-116">Ugyanabban a lekérdezésben 15 percig futott, és ezután váltott ki. a következő hiba:</span><span class="sxs-lookup"><span data-stu-id="15eb0-116">The same query ran for 15 minutes, and then threw the following error:</span></span>

    Status: Failed
    Vertex failed, vertexName=Map 5, vertexId=vertex_1443634917922_0008_1_05, diagnostics=[Task failed, taskId=task_1443634917922_0008_1_05_000006, diagnostics=[TaskAttempt 0 failed, info=[Error: Failure while running task:java.lang.RuntimeException: java.lang.OutOfMemoryError: Java heap space
        at
    org.apache.hadoop.hive.ql.exec.tez.TezProcessor.initializeAndRunProcessor(TezProcessor.java:172)
        at org.apache.hadoop.hive.ql.exec.tez.TezProcessor.run(TezProcessor.java:138)
        at
    org.apache.tez.runtime.LogicalIOProcessorRuntimeTask.run(LogicalIOProcessorRuntimeTask.java:324)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:176)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:168)
        at java.security.AccessController.doPrivileged(Native Method)
        at javax.security.auth.Subject.doAs(Subject.java:415)
        at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1628)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:168)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:163)
        at java.util.concurrent.FutureTask.run(FutureTask.java:262)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
        at java.lang.Thread.run(Thread.java:745)
    Caused by: java.lang.OutOfMemoryError: Java heap space

<span data-ttu-id="15eb0-117">A hiba továbbra is, a nagyobb virtuális gépek (például D12) használatakor.</span><span class="sxs-lookup"><span data-stu-id="15eb0-117">The error remains when using a bigger virtual machine (for example, D12).</span></span>


## <a name="debug-the-out-of-memory-error"></a><span data-ttu-id="15eb0-118">A kevés a memória hibakeresése</span><span class="sxs-lookup"><span data-stu-id="15eb0-118">Debug the out of memory error</span></span>

<span data-ttu-id="15eb0-119">A támogatási és a mérnöki munkacsoportok együtt találta az egyik a kevés a memória, amely egy [ismert probléma az Apache JIRA ismertetett](https://issues.apache.org/jira/browse/HIVE-8306):</span><span class="sxs-lookup"><span data-stu-id="15eb0-119">Our support and engineering teams together found one of the issues causing the out of memory error was a [known issue described in the Apache JIRA](https://issues.apache.org/jira/browse/HIVE-8306):</span></span>

    When hive.auto.convert.join.noconditionaltask = true we check noconditionaltask.size and if the sum  of tables sizes in the map join is less than noconditionaltask.size the plan would generate a Map join, the issue with this is that the calculation doesnt take into account the overhead introduced by different HashTable implementation as results if the sum of input sizes is smaller than the noconditionaltask size by a small margin queries will hit OOM.

<span data-ttu-id="15eb0-120">A **hive.auto.convert.join.noconditionaltask** a hive-site.xml fájl értékre volt állítva **igaz**:</span><span class="sxs-lookup"><span data-stu-id="15eb0-120">The **hive.auto.convert.join.noconditionaltask** in the hive-site.xml file was set to **true**:</span></span>

    <property>
        <name>hive.auto.convert.join.noconditionaltask</name>
        <value>true</value>
        <description>
              Whether Hive enables the optimization about converting common join into mapjoin based on the input file size.
              If this parameter is on, and the sum of size for n-1 of the tables/partitions for a n-way join is smaller than the
              specified size, the join is directly converted to a mapjoin (there is no conditional task).
        </description>
      </property>

<span data-ttu-id="15eb0-121">Valószínűleg térkép illesztési okozta a Java halommemória terület a memóriahiba.</span><span class="sxs-lookup"><span data-stu-id="15eb0-121">It is likely map join was the cause of the Java Heap Space our of memory error.</span></span> <span data-ttu-id="15eb0-122">A blogbejegyzésben leírtaknak [hdinsight Hadoop Yarn memóriabeállításait](http://blogs.msdn.com/b/shanyu/archive/2014/07/31/hadoop-yarn-memory-settings-in-hdinsigh.aspx), a Tez végrehajtó motorja a halommemória használt használt lemezterület ténylegesen a Tez tárolóhoz tartozik.</span><span class="sxs-lookup"><span data-stu-id="15eb0-122">As explained in the blog post [Hadoop Yarn memory settings in HDInsight](http://blogs.msdn.com/b/shanyu/archive/2014/07/31/hadoop-yarn-memory-settings-in-hdinsigh.aspx), when Tez execution engine is used the heap space used actually belongs to the Tez container.</span></span> <span data-ttu-id="15eb0-123">Tekintse meg az alábbi képen a Tez tároló memória leíró.</span><span class="sxs-lookup"><span data-stu-id="15eb0-123">See the following image describing the Tez container memory.</span></span>

![Tez tároló memória diagramja: kevés a memória struktúra](./media/hdinsight-hadoop-hive-out-of-memory-error-oom/hive-out-of-memory-error-oom-tez-container-memory.png)

<span data-ttu-id="15eb0-125">A blogbejegyzés javasol, a következő két memória beállítások megadása a tároló memóriát a halommemória: **hive.tez.container.size** és **hive.tez.java.opts**.</span><span class="sxs-lookup"><span data-stu-id="15eb0-125">As the blog post suggests, the following two memory settings define the container memory for the heap: **hive.tez.container.size** and **hive.tez.java.opts**.</span></span> <span data-ttu-id="15eb0-126">A felhasználói élmény, a memória kivétel kívüli nem jelenti a tároló mérete túl kicsi.</span><span class="sxs-lookup"><span data-stu-id="15eb0-126">From our experience, the out of memory exception does not mean the container size is too small.</span></span> <span data-ttu-id="15eb0-127">Az azt jelenti, hogy a Java halommemória mérete (hive.tez.java.opts) értéke túl kicsi.</span><span class="sxs-lookup"><span data-stu-id="15eb0-127">It means the Java heap size (hive.tez.java.opts) is too small.</span></span> <span data-ttu-id="15eb0-128">Így kevés a memória jelenik meg, amikor megpróbálja növelése **hive.tez.java.opts**.</span><span class="sxs-lookup"><span data-stu-id="15eb0-128">So whenever you see out of memory, you can try to increase **hive.tez.java.opts**.</span></span> <span data-ttu-id="15eb0-129">Szükség esetén lehetséges, hogy növelje **hive.tez.container.size**.</span><span class="sxs-lookup"><span data-stu-id="15eb0-129">If needed you might have to increase **hive.tez.container.size**.</span></span> <span data-ttu-id="15eb0-130">A **java.opts** beállítás körülbelül 80 %-át kell **container.size**.</span><span class="sxs-lookup"><span data-stu-id="15eb0-130">The **java.opts** setting should be around 80% of **container.size**.</span></span>

> [!NOTE]
> <span data-ttu-id="15eb0-131">A beállítás **hive.tez.java.opts** mindig kisebbnek kell lennie **hive.tez.container.size**.</span><span class="sxs-lookup"><span data-stu-id="15eb0-131">The setting **hive.tez.java.opts** must always be smaller than **hive.tez.container.size**.</span></span>
> 
> 

<span data-ttu-id="15eb0-132">Mivel a D12 gépek 28GB memória, a tároló mérete 10 GB-os (10240MB) használja, és 80 % rendel java.opts döntöttünk:</span><span class="sxs-lookup"><span data-stu-id="15eb0-132">Because a D12 machine has 28GB memory, we decided to use a container size of 10GB (10240MB) and assign 80% to java.opts:</span></span>

    SET hive.tez.container.size=10240
    SET hive.tez.java.opts=-Xmx8192m

<span data-ttu-id="15eb0-133">Az új beállításokkal a lekérdezés sikeresen futtatta-e a 10 perc múlva.</span><span class="sxs-lookup"><span data-stu-id="15eb0-133">With the new settings, the query successfully ran in under 10 minutes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="15eb0-134">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="15eb0-134">Next steps</span></span>

<span data-ttu-id="15eb0-135">A memória a hiba nem feltétlenül jelenti azt a tároló mérete túl kicsi.</span><span class="sxs-lookup"><span data-stu-id="15eb0-135">Getting an OOM error doesn't necessarily mean the container size is too small.</span></span> <span data-ttu-id="15eb0-136">Ehelyett az memória beállításokat kell konfigurálnia, hogy a halommemória mérete nő, és a tároló memória mérete legalább 80 %-át.</span><span class="sxs-lookup"><span data-stu-id="15eb0-136">Instead, you should configure the memory settings so that the heap size is increased and is at least 80% of the container memory size.</span></span> <span data-ttu-id="15eb0-137">Hive-lekérdezések optimalizálása, lásd: [a hdinsight Hadoop Hive optimalizálása lekérdezések](hdinsight-hadoop-optimize-hive-query.md).</span><span class="sxs-lookup"><span data-stu-id="15eb0-137">For optimizing Hive queries, see [Optimize Hive queries for Hadoop in HDInsight](hdinsight-hadoop-optimize-hive-query.md).</span></span>