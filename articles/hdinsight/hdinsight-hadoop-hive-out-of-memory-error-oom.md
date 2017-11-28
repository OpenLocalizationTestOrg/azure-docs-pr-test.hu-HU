---
title: "egy kevés a memória az Azure HDInsight Hive aaaFix |} Microsoft Docs"
description: "Javítsa ki a kevés a memória hdinsight Hive. hello forgatókönyv sok nagy táblák között az a lekérdezés."
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
ms.openlocfilehash: 00a12969322c1e74434ba6593ffd098f342edd84
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="fix-a-hive-out-of-memory-error-in-azure-hdinsight"></a><span data-ttu-id="69706-105">Hárítsa el a kevés a memória az Azure HDInsight Hive</span><span class="sxs-lookup"><span data-stu-id="69706-105">Fix a Hive out of memory error in Azure HDInsight</span></span>

<span data-ttu-id="69706-106">Megtudhatja, hogyan toofix kevés a memória a Hive folyamat nagy táblák konfigurálásával memóriabeállításokat struktúra.</span><span class="sxs-lookup"><span data-stu-id="69706-106">Learn how toofix a Hive out of memory error when process large tables by configuring Hive memory settings.</span></span>

## <a name="run-hive-query-against-large-tables"></a><span data-ttu-id="69706-107">Nagy táblák Hive-lekérdezések futtatásához</span><span class="sxs-lookup"><span data-stu-id="69706-107">Run Hive query against large tables</span></span>

<span data-ttu-id="69706-108">Az ügyfél Hive-lekérdezések futtatása:</span><span class="sxs-lookup"><span data-stu-id="69706-108">A customer ran a Hive query:</span></span>

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

<span data-ttu-id="69706-109">A lekérdezés néhány apró:</span><span class="sxs-lookup"><span data-stu-id="69706-109">Some nuances of this query:</span></span>

* <span data-ttu-id="69706-110">A T1 egy alias tooa nagy tábla, TABLE1, amelynek nagy mennyiségű karakterlánc típusú oszlopokat.</span><span class="sxs-lookup"><span data-stu-id="69706-110">T1 is an alias tooa big table, TABLE1, which has lots of STRING column types.</span></span>
* <span data-ttu-id="69706-111">Más táblák, amely nem nagy, de rendelkezik sok oszlopot.</span><span class="sxs-lookup"><span data-stu-id="69706-111">Other tables are not that big but do have many columns.</span></span>
* <span data-ttu-id="69706-112">Minden olyan táblát csatlakozik egymáshoz, bizonyos esetekben több oszlopból álló TABLE1 és mások számára.</span><span class="sxs-lookup"><span data-stu-id="69706-112">All tables are joining each other, in some cases with multiple columns in TABLE1 and others.</span></span>

<span data-ttu-id="69706-113">hello Hive-lekérdezések 26 percet toofinish 24 csomópont a3 méretű HDInsight fürt vett igénybe.</span><span class="sxs-lookup"><span data-stu-id="69706-113">hello Hive query took 26 minutes toofinish on a 24 node A3 HDInsight cluster.</span></span> <span data-ttu-id="69706-114">hello ügyfél észrevesz hello figyelmeztető üzenetek a következő:</span><span class="sxs-lookup"><span data-stu-id="69706-114">hello customer noticed hello following warning messages:</span></span>

    Warning: Map Join MAPJOIN[428][bigTable=?] in task 'Stage-21:MAPRED' is a cross product
    Warning: Shuffle Join JOIN[8][tables = [t1933775, t1932766]] in Stage 'Stage-4:MAPRED' is a cross product

<span data-ttu-id="69706-115">Hello Tez végrehajtómotor használatával.</span><span class="sxs-lookup"><span data-stu-id="69706-115">By using hello Tez execution engine.</span></span> <span data-ttu-id="69706-116">hello ugyanabban a lekérdezésben 15 percig futott, és ezután kivételt okozott a következő hiba hello:</span><span class="sxs-lookup"><span data-stu-id="69706-116">hello same query ran for 15 minutes, and then threw hello following error:</span></span>

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

<span data-ttu-id="69706-117">hello hiba marad, a nagyobb virtuális gépek (például D12) használatakor.</span><span class="sxs-lookup"><span data-stu-id="69706-117">hello error remains when using a bigger virtual machine (for example, D12).</span></span>


## <a name="debug-hello-out-of-memory-error"></a><span data-ttu-id="69706-118">Kevés a memória hello hibakeresése</span><span class="sxs-lookup"><span data-stu-id="69706-118">Debug hello out of memory error</span></span>

<span data-ttu-id="69706-119">A támogatási és a mérnöki munkacsoportok együtt találta az egyik hello hello kevés a memória, amely egy [hello Apache JIRA ismertetett probléma ismert](https://issues.apache.org/jira/browse/HIVE-8306):</span><span class="sxs-lookup"><span data-stu-id="69706-119">Our support and engineering teams together found one of hello issues causing hello out of memory error was a [known issue described in hello Apache JIRA](https://issues.apache.org/jira/browse/HIVE-8306):</span></span>

    When hive.auto.convert.join.noconditionaltask = true we check noconditionaltask.size and if hello sum  of tables sizes in hello map join is less than noconditionaltask.size hello plan would generate a Map join, hello issue with this is that hello calculation doesnt take into account hello overhead introduced by different HashTable implementation as results if hello sum of input sizes is smaller than hello noconditionaltask size by a small margin queries will hit OOM.

<span data-ttu-id="69706-120">Hello **hive.auto.convert.join.noconditionaltask** a hello hive-site.xml fájl túl lett beállítva**igaz**:</span><span class="sxs-lookup"><span data-stu-id="69706-120">hello **hive.auto.convert.join.noconditionaltask** in hello hive-site.xml file was set too**true**:</span></span>

    <property>
        <name>hive.auto.convert.join.noconditionaltask</name>
        <value>true</value>
        <description>
              Whether Hive enables hello optimization about converting common join into mapjoin based on hello input file size.
              If this parameter is on, and hello sum of size for n-1 of hello tables/partitions for a n-way join is smaller than the
              specified size, hello join is directly converted tooa mapjoin (there is no conditional task).
        </description>
      </property>

<span data-ttu-id="69706-121">Valószínűleg térkép illesztési lett hello Java halommemória terület hello okát a memóriahiba.</span><span class="sxs-lookup"><span data-stu-id="69706-121">It is likely map join was hello cause of hello Java Heap Space our of memory error.</span></span> <span data-ttu-id="69706-122">Hello blogbejegyzésben leírtaknak [hdinsight Hadoop Yarn memóriabeállításait](http://blogs.msdn.com/b/shanyu/archive/2014/07/31/hadoop-yarn-memory-settings-in-hdinsigh.aspx), ha a végrehajtó motorja megtelt használt hello halommemória használt Tez ténylegesen toohello Tez tárolóhoz tartozik.</span><span class="sxs-lookup"><span data-stu-id="69706-122">As explained in hello blog post [Hadoop Yarn memory settings in HDInsight](http://blogs.msdn.com/b/shanyu/archive/2014/07/31/hadoop-yarn-memory-settings-in-hdinsigh.aspx), when Tez execution engine is used hello heap space used actually belongs toohello Tez container.</span></span> <span data-ttu-id="69706-123">Tekintse meg a következő kép leíró hello Tez tároló memória hello.</span><span class="sxs-lookup"><span data-stu-id="69706-123">See hello following image describing hello Tez container memory.</span></span>

![Tez tároló memória diagramja: kevés a memória struktúra](./media/hdinsight-hadoop-hive-out-of-memory-error-oom/hive-out-of-memory-error-oom-tez-container-memory.png)

<span data-ttu-id="69706-125">Hello blogbejegyzés javasol, mert a következő két memóriabeállításait hello meghatározása hello tároló memória hello halommemória: **hive.tez.container.size** és **hive.tez.java.opts**.</span><span class="sxs-lookup"><span data-stu-id="69706-125">As hello blog post suggests, hello following two memory settings define hello container memory for hello heap: **hive.tez.container.size** and **hive.tez.java.opts**.</span></span> <span data-ttu-id="69706-126">A felhasználói élmény, a memória kivételhiba hello nem jelenti azt hello tároló mérete túl kicsi.</span><span class="sxs-lookup"><span data-stu-id="69706-126">From our experience, hello out of memory exception does not mean hello container size is too small.</span></span> <span data-ttu-id="69706-127">Ez azt jelenti, hogy hello Java halommemória mérete (hive.tez.java.opts) értéke túl kicsi.</span><span class="sxs-lookup"><span data-stu-id="69706-127">It means hello Java heap size (hive.tez.java.opts) is too small.</span></span> <span data-ttu-id="69706-128">Így ha kevés a memória jelenik meg, megpróbálhatja tooincrease **hive.tez.java.opts**.</span><span class="sxs-lookup"><span data-stu-id="69706-128">So whenever you see out of memory, you can try tooincrease **hive.tez.java.opts**.</span></span> <span data-ttu-id="69706-129">Szükség esetén előfordulhat, hogy tooincrease **hive.tez.container.size**.</span><span class="sxs-lookup"><span data-stu-id="69706-129">If needed you might have tooincrease **hive.tez.container.size**.</span></span> <span data-ttu-id="69706-130">Hello **java.opts** beállítás körülbelül 80 %-át kell **container.size**.</span><span class="sxs-lookup"><span data-stu-id="69706-130">hello **java.opts** setting should be around 80% of **container.size**.</span></span>

> [!NOTE]
> <span data-ttu-id="69706-131">hello beállítás **hive.tez.java.opts** mindig kisebbnek kell lennie **hive.tez.container.size**.</span><span class="sxs-lookup"><span data-stu-id="69706-131">hello setting **hive.tez.java.opts** must always be smaller than **hive.tez.container.size**.</span></span>
> 
> 

<span data-ttu-id="69706-132">A D12 gépek 28GB memóriával rendelkezik, mert azt úgy döntött, toouse egy tároló mérete 10 GB-os (10240MB), és rendelje hozzá a 80 % toojava.opts:</span><span class="sxs-lookup"><span data-stu-id="69706-132">Because a D12 machine has 28GB memory, we decided toouse a container size of 10GB (10240MB) and assign 80% toojava.opts:</span></span>

    SET hive.tez.container.size=10240
    SET hive.tez.java.opts=-Xmx8192m

<span data-ttu-id="69706-133">Az új beállítások hello hello lekérdezés sikeresen futtatta-e a 10 perc múlva.</span><span class="sxs-lookup"><span data-stu-id="69706-133">With hello new settings, hello query successfully ran in under 10 minutes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="69706-134">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="69706-134">Next steps</span></span>

<span data-ttu-id="69706-135">A memória a hiba nem feltétlenül jelenti azt hello tároló mérete túl kicsi.</span><span class="sxs-lookup"><span data-stu-id="69706-135">Getting an OOM error doesn't necessarily mean hello container size is too small.</span></span> <span data-ttu-id="69706-136">Ehelyett konfigurálni kell hello memória beállításait, hogy hello halommemória mérete nő, és legalább 80 %-a hello tároló memória méretét.</span><span class="sxs-lookup"><span data-stu-id="69706-136">Instead, you should configure hello memory settings so that hello heap size is increased and is at least 80% of hello container memory size.</span></span> <span data-ttu-id="69706-137">Hive-lekérdezések optimalizálása, lásd: [a hdinsight Hadoop Hive optimalizálása lekérdezések](hdinsight-hadoop-optimize-hive-query.md).</span><span class="sxs-lookup"><span data-stu-id="69706-137">For optimizing Hive queries, see [Optimize Hive queries for Hadoop in HDInsight](hdinsight-hadoop-optimize-hive-query.md).</span></span>
