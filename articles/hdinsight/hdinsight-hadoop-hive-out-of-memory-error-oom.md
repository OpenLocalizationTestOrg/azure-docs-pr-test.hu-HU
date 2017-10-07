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
# <a name="fix-a-hive-out-of-memory-error-in-azure-hdinsight"></a>Hárítsa el a kevés a memória az Azure HDInsight Hive

Megtudhatja, hogyan toofix kevés a memória a Hive folyamat nagy táblák konfigurálásával memóriabeállításokat struktúra.

## <a name="run-hive-query-against-large-tables"></a>Nagy táblák Hive-lekérdezések futtatásához

Az ügyfél Hive-lekérdezések futtatása:

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

A lekérdezés néhány apró:

* A T1 egy alias tooa nagy tábla, TABLE1, amelynek nagy mennyiségű karakterlánc típusú oszlopokat.
* Más táblák, amely nem nagy, de rendelkezik sok oszlopot.
* Minden olyan táblát csatlakozik egymáshoz, bizonyos esetekben több oszlopból álló TABLE1 és mások számára.

hello Hive-lekérdezések 26 percet toofinish 24 csomópont a3 méretű HDInsight fürt vett igénybe. hello ügyfél észrevesz hello figyelmeztető üzenetek a következő:

    Warning: Map Join MAPJOIN[428][bigTable=?] in task 'Stage-21:MAPRED' is a cross product
    Warning: Shuffle Join JOIN[8][tables = [t1933775, t1932766]] in Stage 'Stage-4:MAPRED' is a cross product

Hello Tez végrehajtómotor használatával. hello ugyanabban a lekérdezésben 15 percig futott, és ezután kivételt okozott a következő hiba hello:

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

hello hiba marad, a nagyobb virtuális gépek (például D12) használatakor.


## <a name="debug-hello-out-of-memory-error"></a>Kevés a memória hello hibakeresése

A támogatási és a mérnöki munkacsoportok együtt találta az egyik hello hello kevés a memória, amely egy [hello Apache JIRA ismertetett probléma ismert](https://issues.apache.org/jira/browse/HIVE-8306):

    When hive.auto.convert.join.noconditionaltask = true we check noconditionaltask.size and if hello sum  of tables sizes in hello map join is less than noconditionaltask.size hello plan would generate a Map join, hello issue with this is that hello calculation doesnt take into account hello overhead introduced by different HashTable implementation as results if hello sum of input sizes is smaller than hello noconditionaltask size by a small margin queries will hit OOM.

Hello **hive.auto.convert.join.noconditionaltask** a hello hive-site.xml fájl túl lett beállítva**igaz**:

    <property>
        <name>hive.auto.convert.join.noconditionaltask</name>
        <value>true</value>
        <description>
              Whether Hive enables hello optimization about converting common join into mapjoin based on hello input file size.
              If this parameter is on, and hello sum of size for n-1 of hello tables/partitions for a n-way join is smaller than the
              specified size, hello join is directly converted tooa mapjoin (there is no conditional task).
        </description>
      </property>

Valószínűleg térkép illesztési lett hello Java halommemória terület hello okát a memóriahiba. Hello blogbejegyzésben leírtaknak [hdinsight Hadoop Yarn memóriabeállításait](http://blogs.msdn.com/b/shanyu/archive/2014/07/31/hadoop-yarn-memory-settings-in-hdinsigh.aspx), ha a végrehajtó motorja megtelt használt hello halommemória használt Tez ténylegesen toohello Tez tárolóhoz tartozik. Tekintse meg a következő kép leíró hello Tez tároló memória hello.

![Tez tároló memória diagramja: kevés a memória struktúra](./media/hdinsight-hadoop-hive-out-of-memory-error-oom/hive-out-of-memory-error-oom-tez-container-memory.png)

Hello blogbejegyzés javasol, mert a következő két memóriabeállításait hello meghatározása hello tároló memória hello halommemória: **hive.tez.container.size** és **hive.tez.java.opts**. A felhasználói élmény, a memória kivételhiba hello nem jelenti azt hello tároló mérete túl kicsi. Ez azt jelenti, hogy hello Java halommemória mérete (hive.tez.java.opts) értéke túl kicsi. Így ha kevés a memória jelenik meg, megpróbálhatja tooincrease **hive.tez.java.opts**. Szükség esetén előfordulhat, hogy tooincrease **hive.tez.container.size**. Hello **java.opts** beállítás körülbelül 80 %-át kell **container.size**.

> [!NOTE]
> hello beállítás **hive.tez.java.opts** mindig kisebbnek kell lennie **hive.tez.container.size**.
> 
> 

A D12 gépek 28GB memóriával rendelkezik, mert azt úgy döntött, toouse egy tároló mérete 10 GB-os (10240MB), és rendelje hozzá a 80 % toojava.opts:

    SET hive.tez.container.size=10240
    SET hive.tez.java.opts=-Xmx8192m

Az új beállítások hello hello lekérdezés sikeresen futtatta-e a 10 perc múlva.

## <a name="next-steps"></a>Következő lépések

A memória a hiba nem feltétlenül jelenti azt hello tároló mérete túl kicsi. Ehelyett konfigurálni kell hello memória beállításait, hogy hello halommemória mérete nő, és legalább 80 %-a hello tároló memória méretét. Hive-lekérdezések optimalizálása, lásd: [a hdinsight Hadoop Hive optimalizálása lekérdezések](hdinsight-hadoop-optimize-hive-query.md).
