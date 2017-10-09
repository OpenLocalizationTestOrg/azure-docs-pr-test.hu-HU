---
title: "a HBase aaaTroubleshoot Azure HDInsight használatával |} Microsoft Docs"
description: "Válaszok a HBase és az Azure HDInsight toocommon kérdésekre."
services: hdinsight
documentationcenter: 
author: nitinver
manager: ashitg
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 7/7/2017
ms.author: nitinver
ms.openlocfilehash: 5210184f8ea95628952a95df8c98f5b98e37c53e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-hbase-by-using-azure-hdinsight"></a><span data-ttu-id="d11c0-103">A HBase hibaelhárítása az Azure HDInsight használatával</span><span class="sxs-lookup"><span data-stu-id="d11c0-103">Troubleshoot HBase by using Azure HDInsight</span></span>

<span data-ttu-id="d11c0-104">Hello legfőbb problémákat és azok megoldásait ismerje meg az Apache Ambari az Apache HBase hasznos adat található használatakor.</span><span class="sxs-lookup"><span data-stu-id="d11c0-104">Learn about hello top issues and their resolutions when working with Apache HBase payloads in Apache Ambari.</span></span>

## <a name="how-do-i-run-hbck-command-reports-with-multiple-unassigned-regions"></a><span data-ttu-id="d11c0-105">Hogyan futtathatok hbck parancs jelentéseket a ki nem osztott több régióba</span><span class="sxs-lookup"><span data-stu-id="d11c0-105">How do I run hbck command reports with multiple unassigned regions</span></span>

<span data-ttu-id="d11c0-106">Egy általános hibaüzenetet, hogy mikor fusson a hello megjelenhet `hbase hbck` parancs a "több régióba alatt hozzá nem rendelt vagy régiókban hello láncában lyuk."</span><span class="sxs-lookup"><span data-stu-id="d11c0-106">A common error message that you might see when you run hello `hbase hbck` command is "multiple regions being unassigned or holes in hello chain of regions."</span></span>

<span data-ttu-id="d11c0-107">A HBase fő felhasználói felület hello megtekintheti a hello több régióban, amely minden régióban kiszolgálóján is tette.</span><span class="sxs-lookup"><span data-stu-id="d11c0-107">In hello HBase Master UI, you can see hello number of regions that are unbalanced across all region servers.</span></span> <span data-ttu-id="d11c0-108">Ezt követően futtathatja `hbase hbck` toosee lyuk hello régió lánc parancsot.</span><span class="sxs-lookup"><span data-stu-id="d11c0-108">Then, you can run `hbase hbck` command toosee holes in hello region chain.</span></span>

<span data-ttu-id="d11c0-109">Lyuk oka lehet hello offline régiókból, így javítás hello hozzárendelések először.</span><span class="sxs-lookup"><span data-stu-id="d11c0-109">Holes might be caused by hello offline regions, so fix hello assignments first.</span></span> 

<span data-ttu-id="d11c0-110">toobring hello ki nem osztott régiók hátsó tooa normál állapotban, végezze el az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d11c0-110">toobring hello unassigned regions back tooa normal state, complete hello following steps:</span></span>

1. <span data-ttu-id="d11c0-111">Jelentkezzen be toohello HDInsight HBase-fürtöt SSH használatával.</span><span class="sxs-lookup"><span data-stu-id="d11c0-111">Sign in toohello HDInsight HBase cluster by using SSH.</span></span>
2. <span data-ttu-id="d11c0-112">a hello ZooKeeper rendszerhéj, futtassa a hello tooconnect `hbase zkcli` parancsot.</span><span class="sxs-lookup"><span data-stu-id="d11c0-112">tooconnect with hello ZooKeeper shell, run hello `hbase zkcli` command.</span></span>
3. <span data-ttu-id="d11c0-113">Futtassa a hello `rmr /hbase/regions-in-transition` parancs vagy hello `rmr /hbase-unsecure/regions-in-transition` parancsot.</span><span class="sxs-lookup"><span data-stu-id="d11c0-113">Run hello `rmr /hbase/regions-in-transition` command or hello `rmr /hbase-unsecure/regions-in-transition` command.</span></span>
4. <span data-ttu-id="d11c0-114">a hello tooexit `hbase zkcli` rendszerhéj, használja a hello `exit` parancsot.</span><span class="sxs-lookup"><span data-stu-id="d11c0-114">tooexit from hello `hbase zkcli` shell, use hello `exit` command.</span></span>
5. <span data-ttu-id="d11c0-115">Nyissa meg a hello Apache Ambari felhasználói felület, és indítsa újra hello aktív HBase fő szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="d11c0-115">Open hello Apache Ambari UI, and then restart hello Active HBase Master service.</span></span>
6. <span data-ttu-id="d11c0-116">Futtassa a hello `hbase hbck` parancsot ismét (kapcsolók) nélkül.</span><span class="sxs-lookup"><span data-stu-id="d11c0-116">Run hello `hbase hbck` command again (without any options).</span></span> <span data-ttu-id="d11c0-117">Ellenőrizze a parancs tooensure minden egyes hozzárendelt hello kimenetét.</span><span class="sxs-lookup"><span data-stu-id="d11c0-117">Check hello output of this command tooensure that all regions are being assigned.</span></span>


## <span data-ttu-id="d11c0-118"><a name="how-do-i-fix-timeout-issues-with-hbck-commands-for-region-assignments"></a>Hogyan oldja meg időtúllépés problémák, régió hozzárendelések hbck parancsok használata esetén</span><span class="sxs-lookup"><span data-stu-id="d11c0-118"><a name="how-do-i-fix-timeout-issues-with-hbck-commands-for-region-assignments"></a>How do I fix timeout issues when using hbck commands for region assignments</span></span>

### <a name="issue"></a><span data-ttu-id="d11c0-119">Probléma</span><span class="sxs-lookup"><span data-stu-id="d11c0-119">Issue</span></span>

<span data-ttu-id="d11c0-120">Egyik lehetséges oka az időtúllépési problémák hello használatakor `hbck` parancs lehet, hogy több régiók a következők a hello "az átmenet" állapot hosszú ideig.</span><span class="sxs-lookup"><span data-stu-id="d11c0-120">A potential cause for timeout issues when you use hello `hbck` command might be that several regions are in hello "in transition" state for a long time.</span></span> <span data-ttu-id="d11c0-121">Azokban a régiókban, offline állapotú hello HBase fő felhasználói felületén tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="d11c0-121">You can see those regions as offline in hello HBase Master UI.</span></span> <span data-ttu-id="d11c0-122">Régiók nagy mennyiségű próbált tootransition, mert a HBase fő időtúllépés lehet, és nem toobring újra online azokban a régiókban kell.</span><span class="sxs-lookup"><span data-stu-id="d11c0-122">Because a high number of regions are attempting tootransition, HBase Master might timeout and be unable toobring those regions back online.</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="d11c0-123">Megoldási lépések</span><span class="sxs-lookup"><span data-stu-id="d11c0-123">Resolution steps</span></span>

1. <span data-ttu-id="d11c0-124">Jelentkezzen be toohello HDInsight HBase-fürtöt SSH használatával.</span><span class="sxs-lookup"><span data-stu-id="d11c0-124">Sign in toohello HDInsight HBase cluster by using SSH.</span></span>
2. <span data-ttu-id="d11c0-125">a hello ZooKeeper rendszerhéj, futtassa a hello tooconnect `hbase zkcli` parancsot.</span><span class="sxs-lookup"><span data-stu-id="d11c0-125">tooconnect with hello ZooKeeper shell, run hello `hbase zkcli` command.</span></span>
3. <span data-ttu-id="d11c0-126">Futtassa a hello `rmr /hbase/regions-in-transition` vagy hello `rmr /hbase-unsecure/regions-in-transition` parancsot.</span><span class="sxs-lookup"><span data-stu-id="d11c0-126">Run hello `rmr /hbase/regions-in-transition` or hello `rmr /hbase-unsecure/regions-in-transition` command.</span></span>
4. <span data-ttu-id="d11c0-127">tooexit hello `hbase zkcli` rendszerhéj, használja a hello `exit` parancsot.</span><span class="sxs-lookup"><span data-stu-id="d11c0-127">tooexit hello `hbase zkcli` shell, use hello `exit` command.</span></span>
5. <span data-ttu-id="d11c0-128">Hello Ambari felhasználói felület indítsa újra az hello aktív HBase fő szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="d11c0-128">In hello Ambari UI, restart hello Active HBase Master service.</span></span>
6. <span data-ttu-id="d11c0-129">Futtassa a hello `hbase hbck -fixAssignments` újra a parancsot.</span><span class="sxs-lookup"><span data-stu-id="d11c0-129">Run hello `hbase hbck -fixAssignments` command again.</span></span>

## <span data-ttu-id="d11c0-130"><a name="how-do-i-force-disable-hdfs-safe-mode-in-a-cluster"></a>Hogyan tegye I kényszerített – letiltása HDFS csökkentett módban egy fürtben</span><span class="sxs-lookup"><span data-stu-id="d11c0-130"><a name="how-do-i-force-disable-hdfs-safe-mode-in-a-cluster"></a>How do I force-disable HDFS safe mode in a cluster</span></span>

### <a name="issue"></a><span data-ttu-id="d11c0-131">Probléma</span><span class="sxs-lookup"><span data-stu-id="d11c0-131">Issue</span></span>

<span data-ttu-id="d11c0-132">hello helyi Hadoop elosztott fájlrendszerrel (HDFS) Beragadt hello HDInsight-fürt csökkentett módban.</span><span class="sxs-lookup"><span data-stu-id="d11c0-132">hello local Hadoop Distributed File System (HDFS) is stuck in safe mode on hello HDInsight cluster.</span></span>

### <a name="detailed-description"></a><span data-ttu-id="d11c0-133">Részletes leírás</span><span class="sxs-lookup"><span data-stu-id="d11c0-133">Detailed description</span></span>

<span data-ttu-id="d11c0-134">Ez a hiba oka lehet egy hiba hello HDFS parancs a következő futtatásakor:</span><span class="sxs-lookup"><span data-stu-id="d11c0-134">This error might be caused by a failure when you run hello following HDFS command:</span></span>

```apache
hdfs dfs -D "fs.default.name=hdfs://mycluster/" -mkdir /temp
```

<span data-ttu-id="d11c0-135">hello hiba láthatja, amikor megpróbál toorun hello parancs így néz ki:</span><span class="sxs-lookup"><span data-stu-id="d11c0-135">hello error you might see when you try toorun hello command looks like this:</span></span>

```apache
hdiuser@hn0-spark2:~$ hdfs dfs -D "fs.default.name=hdfs://mycluster/" -mkdir /temp
17/04/05 16:20:52 WARN retry.RetryInvocationHandler: Exception while invoking ClientNamenodeProtocolTranslatorPB.mkdirs over hn0-spark2.2oyzcdm4sfjuzjmj5dnmvscjpg.dx.internal.cloudapp.net/10.0.0.22:8020. Not retrying because try once and fail.
org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.hdfs.server.namenode.SafeModeException): Cannot create directory /temp. Name node is in safe mode.
It was turned on manually. Use "hdfs dfsadmin -safemode leave" tooturn safe mode off.
        at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.checkNameNodeSafeMode(FSNamesystem.java:1359)
        at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.mkdirs(FSNamesystem.java:4010)
        at org.apache.hadoop.hdfs.server.namenode.NameNodeRpcServer.mkdirs(NameNodeRpcServer.java:1102)
        at org.apache.hadoop.hdfs.protocolPB.ClientNamenodeProtocolServerSideTranslatorPB.mkdirs(ClientNamenodeProtocolServerSideTranslatorPB.java:630)
        at org.apache.hadoop.hdfs.protocol.proto.ClientNamenodeProtocolProtos$ClientNamenodeProtocol$2.callBlockingMethod(ClientNamenodeProtocolProtos.java)
        at org.apache.hadoop.ipc.ProtobufRpcEngine$Server$ProtoBufRpcInvoker.call(ProtobufRpcEngine.java:640)
        at org.apache.hadoop.ipc.RPC$Server.call(RPC.java:982)
        at org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2313)
        at org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2309)
        at java.security.AccessController.doPrivileged(Native Method)
        at javax.security.auth.Subject.doAs(Subject.java:422)
        at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1724)
        at org.apache.hadoop.ipc.Server$Handler.run(Server.java:2307)
        at org.apache.hadoop.ipc.Client.getRpcResponse(Client.java:1552)
        at org.apache.hadoop.ipc.Client.call(Client.java:1496)
        at org.apache.hadoop.ipc.Client.call(Client.java:1396)
        at org.apache.hadoop.ipc.ProtobufRpcEngine$Invoker.invoke(ProtobufRpcEngine.java:233)
        at com.sun.proxy.$Proxy10.mkdirs(Unknown Source)
        at org.apache.hadoop.hdfs.protocolPB.ClientNamenodeProtocolTranslatorPB.mkdirs(ClientNamenodeProtocolTranslatorPB.java:603)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:498)
        at org.apache.hadoop.io.retry.RetryInvocationHandler.invokeMethod(RetryInvocationHandler.java:278)
        at org.apache.hadoop.io.retry.RetryInvocationHandler.invoke(RetryInvocationHandler.java:194)
        at org.apache.hadoop.io.retry.RetryInvocationHandler.invoke(RetryInvocationHandler.java:176)
        at com.sun.proxy.$Proxy11.mkdirs(Unknown Source)
        at org.apache.hadoop.hdfs.DFSClient.primitiveMkdir(DFSClient.java:3061)
        at org.apache.hadoop.hdfs.DFSClient.mkdirs(DFSClient.java:3031)
        at org.apache.hadoop.hdfs.DistributedFileSystem$24.doCall(DistributedFileSystem.java:1162)
        at org.apache.hadoop.hdfs.DistributedFileSystem$24.doCall(DistributedFileSystem.java:1158)
        at org.apache.hadoop.fs.FileSystemLinkResolver.resolve(FileSystemLinkResolver.java:81)
        at org.apache.hadoop.hdfs.DistributedFileSystem.mkdirsInternal(DistributedFileSystem.java:1158)
        at org.apache.hadoop.hdfs.DistributedFileSystem.mkdirs(DistributedFileSystem.java:1150)
        at org.apache.hadoop.fs.FileSystem.mkdirs(FileSystem.java:1898)
        at org.apache.hadoop.fs.shell.Mkdir.processNonexistentPath(Mkdir.java:76)
        at org.apache.hadoop.fs.shell.Command.processArgument(Command.java:273)
        at org.apache.hadoop.fs.shell.Command.processArguments(Command.java:255)
        at org.apache.hadoop.fs.shell.FsCommand.processRawArguments(FsCommand.java:119)
        at org.apache.hadoop.fs.shell.Command.run(Command.java:165)
        at org.apache.hadoop.fs.FsShell.run(FsShell.java:297)
        at org.apache.hadoop.util.ToolRunner.run(ToolRunner.java:76)
        at org.apache.hadoop.util.ToolRunner.run(ToolRunner.java:90)
        at org.apache.hadoop.fs.FsShell.main(FsShell.java:350)
mkdir: Cannot create directory /temp. Name node is in safe mode.
```

### <a name="probable-cause"></a><span data-ttu-id="d11c0-136">Lehetséges ok</span><span class="sxs-lookup"><span data-stu-id="d11c0-136">Probable cause</span></span>

<span data-ttu-id="d11c0-137">hello HDInsight-fürt már le tooa méretezhető nagyon kevés csomópontot.</span><span class="sxs-lookup"><span data-stu-id="d11c0-137">hello HDInsight cluster has been scaled down tooa very few nodes.</span></span> <span data-ttu-id="d11c0-138">hello csomópontok száma nem éri el, vagy zárja be a toohello HDFS replikációs tényező.</span><span class="sxs-lookup"><span data-stu-id="d11c0-138">hello number of nodes is below or close toohello HDFS replication factor.</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="d11c0-139">Megoldási lépések</span><span class="sxs-lookup"><span data-stu-id="d11c0-139">Resolution steps</span></span> 

1. <span data-ttu-id="d11c0-140">A HDFS a hello HDInsight fürt hello a következő parancsok futtatásával hello hello állapotának beolvasása:</span><span class="sxs-lookup"><span data-stu-id="d11c0-140">Get hello status of hello HDFS on hello HDInsight cluster by running hello following commands:</span></span>

   ```apache
   hdfs dfsadmin -D "fs.default.name=hdfs://mycluster/" -report
   ```

   ```apache
   hdiuser@hn0-spark2:~$ hdfs dfsadmin -D "fs.default.name=hdfs://mycluster/" -report
   Safe mode is ON
   Configured Capacity: 3372381241344 (3.07 TB)
   Present Capacity: 3138625077248 (2.85 TB)
   DFS Remaining: 3102710317056 (2.82 TB)
   DFS Used: 35914760192 (33.45 GB)
   DFS Used%: 1.14%
   Under replicated blocks: 0
   Blocks with corrupt replicas: 0
   Missing blocks: 0
   Missing blocks (with replication factor 1): 0

   -------------------------------------------------
   Live datanodes (8):

   Name: 10.0.0.17:30010 (10.0.0.17)
   Hostname: 10.0.0.17
   Decommission Status : Normal
   Configured Capacity: 421547655168 (392.60 GB)
   DFS Used: 5288128512 (4.92 GB)
   Non DFS Used: 29087272960 (27.09 GB)
   DFS Remaining: 387172253696 (360.58 GB)
   DFS Used%: 1.25%
   DFS Remaining%: 91.85%
   Configured Cache Capacity: 0 (0 B)
   Cache Used: 0 (0 B)
   Cache Remaining: 0 (0 B)
   Cache Used%: 100.00%
   Cache Remaining%: 0.00%
   Xceivers: 2
   Last contact: Wed Apr 05 16:22:00 UTC 2017
   ...

   ```
2. <span data-ttu-id="d11c0-141">HDFS a hello HDInsight fürt hello a következő parancsok használatával hello hello integritását is ellenőrizheti:</span><span class="sxs-lookup"><span data-stu-id="d11c0-141">You also can check hello integrity of hello HDFS on hello HDInsight cluster by using hello following commands:</span></span>

   ```apache
   hdiuser@hn0-spark2:~$ hdfs fsck -D "fs.default.name=hdfs://mycluster/" /
   ```

   ```apache
   Connecting toonamenode via http://hn0-spark2.2oyzcdm4sfjuzjmj5dnmvscjpg.dx.internal.cloudapp.net:30070/fsck?ugi=hdiuser&path=%2F
   FSCK started by hdiuser (auth:SIMPLE) from /10.0.0.22 for path / at Wed Apr 05 16:40:28 UTC 2017
   ....................................................................................................

   ....................................................................................................
   ..................Status: HEALTHY
   Total size:    9330539472 B
   Total dirs:    37
   Total files:   2618
   Total symlinks:                0 (Files currently being written: 2)
   Total blocks (validated):      2535 (avg. block size 3680686 B)
   Minimally replicated blocks:   2535 (100.0 %)
   Over-replicated blocks:        0 (0.0 %)
   Under-replicated blocks:       0 (0.0 %)
   Mis-replicated blocks:         0 (0.0 %)
   Default replication factor:    3
   Average block replication:     3.0
   Corrupt blocks:                0
   Missing replicas:              0 (0.0 %)
   Number of data-nodes:          8
   Number of racks:               1
   FSCK ended at Wed Apr 05 16:40:28 UTC 2017 in 187 milliseconds

   hello filesystem under path '/' is HEALTHY
   ```

3. <span data-ttu-id="d11c0-142">Ha azt állapítja meg, hogy vannak-e nem hiányzik, sérült vagy under-replikált blokkolja, vagy az, hogy ezek a blokkok figyelmen kívül hagyható, futtassa a következő parancs tootake hello neve csomópont biztonságos módból hello:</span><span class="sxs-lookup"><span data-stu-id="d11c0-142">If you determine that there are no missing, corrupt, or under-replicated blocks, or that those blocks can be ignored, run hello following command tootake hello name node out of safe mode:</span></span>

   ```apache
   hdfs dfsadmin -D "fs.default.name=hdfs://mycluster/" -safemode leave
   ```


## <a name="how-do-i-fix-jdbc-or-sqlline-connectivity-issues-with-apache-phoenix"></a><span data-ttu-id="d11c0-143">Hogyan állítsa helyre JDBC vagy SQLLine elérhetőségét Apache Phoenix problémái</span><span class="sxs-lookup"><span data-stu-id="d11c0-143">How do I fix JDBC or SQLLine connectivity issues with Apache Phoenix</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="d11c0-144">Megoldási lépések</span><span class="sxs-lookup"><span data-stu-id="d11c0-144">Resolution steps</span></span>

<span data-ttu-id="d11c0-145">a Phoenix tooconnect, meg kell adnia az aktív ZooKeeper csomópont hello IP-címét.</span><span class="sxs-lookup"><span data-stu-id="d11c0-145">tooconnect with Phoenix, you must provide hello IP address of an active ZooKeeper node.</span></span> <span data-ttu-id="d11c0-146">Győződjön meg arról, hogy hello szolgáltatás toowhich sqlline.py próbál ZooKeeper tooconnect megfelelően működik, és.</span><span class="sxs-lookup"><span data-stu-id="d11c0-146">Ensure that hello ZooKeeper service toowhich sqlline.py is trying tooconnect is up and running.</span></span>
1. <span data-ttu-id="d11c0-147">Jelentkezzen be toohello HDInsight-fürthöz SSH használatával.</span><span class="sxs-lookup"><span data-stu-id="d11c0-147">Sign in toohello HDInsight cluster by using SSH.</span></span>
2. <span data-ttu-id="d11c0-148">Adja meg a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="d11c0-148">Enter hello following command:</span></span>
                
   ```apache
           "/usr/hdp/current/phoenix-client/bin/sqlline.py <IP of machine where Active Zookeeper is running"
   ```

   > [!Note] 
   > <span data-ttu-id="d11c0-149">Hello IP-cím hello aktív ZooKeeper csomópontjának az Ambari felhasználói felület hello kérheti le.</span><span class="sxs-lookup"><span data-stu-id="d11c0-149">You can get hello IP address of hello active ZooKeeper node from hello Ambari UI.</span></span> <span data-ttu-id="d11c0-150">Nyissa meg túl**HBase** > **Gyorshivatkozások** > **ZK\* (aktív)** > **Zookeeperadatai**.</span><span class="sxs-lookup"><span data-stu-id="d11c0-150">Go too**HBase** > **Quick Links** > **ZK\* (Active)** > **Zookeeper Info**.</span></span> 

3. <span data-ttu-id="d11c0-151">Ha hello sqlline.py tooPhoenix csatlakozik, és amelyen nincs időtúllépés, a futtatási hello következő parancsot a toovalidate hello rendelkezésre állás és a Phoenix állapotát:</span><span class="sxs-lookup"><span data-stu-id="d11c0-151">If hello sqlline.py connects tooPhoenix and does not timeout, run hello following command toovalidate hello availability and health of Phoenix:</span></span>

   ```apache
           !tables
           !quit
   ```      
4. <span data-ttu-id="d11c0-152">Ez a parancs működik, ha nincs probléma.</span><span class="sxs-lookup"><span data-stu-id="d11c0-152">If this command works, there is no issue.</span></span> <span data-ttu-id="d11c0-153">hello felhasználó által megadott hello IP-cím helytelen lehet.</span><span class="sxs-lookup"><span data-stu-id="d11c0-153">hello IP address provided by hello user might be incorrect.</span></span> <span data-ttu-id="d11c0-154">Azonban ha majd jeleníti meg a következő hiba hello hello parancs hosszabb ideig szünetelteti, továbbra is toostep 5.</span><span class="sxs-lookup"><span data-stu-id="d11c0-154">However, if hello command pauses for an extended time and then displays hello following error, continue toostep 5.</span></span>

   ```apache
           Error while connecting toosqlline.py (Hbase - phoenix) Setting property: [isolation, TRANSACTION_READ_COMMITTED] issuing: !connect jdbc:phoenix:10.2.0.7 none none org.apache.phoenix.jdbc.PhoenixDriver Connecting toojdbc:phoenix:10.2.0.7 SLF4J: Class path contains multiple SLF4J bindings. 
   ```

5. <span data-ttu-id="d11c0-155">Futtassa a parancsokat követően – hello átjárócsomópont (hn0) toodiagnose hello feltétele hello Phoenix rendszer hello. KATALÓGUS tábla:</span><span class="sxs-lookup"><span data-stu-id="d11c0-155">Run hello following commands from hello head node (hn0) toodiagnose hello condition of hello Phoenix SYSTEM.CATALOG table:</span></span>

   ```apache
            hbase shell
                
           count 'SYSTEM.CATALOG'
   ```

   <span data-ttu-id="d11c0-156">hello parancs visszaadja-e egy hiba hasonló toohello következő:</span><span class="sxs-lookup"><span data-stu-id="d11c0-156">hello command should return an error similar toohello following:</span></span> 

   ```apache
           ERROR: org.apache.hadoop.hbase.NotServingRegionException: Region SYSTEM.CATALOG,,1485464083256.c0568c94033870c517ed36c45da98129. is not online on 10.2.0.5,16020,1489466172189) 
   ```
6. <span data-ttu-id="d11c0-157">Hello Ambari felhasználói felület végeznie hello lépéseket toorestart hello HMaster szolgáltatás minden ZooKeeper csomópontján a következő:</span><span class="sxs-lookup"><span data-stu-id="d11c0-157">In hello Ambari UI, complete hello following steps toorestart hello HMaster service on all ZooKeeper nodes:</span></span>

    1. <span data-ttu-id="d11c0-158">A hello **összegzés** szakasz a HBase, nyissa meg túl**HBase** > **aktív HBase fő**.</span><span class="sxs-lookup"><span data-stu-id="d11c0-158">In hello **Summary** section of HBase, go too**HBase** > **Active HBase Master**.</span></span> 
    2. <span data-ttu-id="d11c0-159">A hello **összetevők** területen hello HBase fő szolgáltatás újraindításához.</span><span class="sxs-lookup"><span data-stu-id="d11c0-159">In hello **Components** section, restart hello HBase Master service.</span></span>
    3. <span data-ttu-id="d11c0-160">Ismételje meg ezeket a lépéseket minden fennmaradó **készenléti HBase fő** szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="d11c0-160">Repeat these steps for all remaining **Standby HBase Master** services.</span></span> 

<span data-ttu-id="d11c0-161">Ez hello HBase fő szolgáltatás toostabilize toofive percig tarthat, és hello helyreállítási folyamat befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="d11c0-161">It can take up toofive minutes for hello HBase Master service toostabilize and finish hello recovery process.</span></span> <span data-ttu-id="d11c0-162">Néhány perc elteltével ismételje meg hello sqlline.py parancsok tooconfirm adott hello rendszer. KATALÓGUS tábla működik-e, és, hogy az informatikai kérdezhetők le.</span><span class="sxs-lookup"><span data-stu-id="d11c0-162">After a few minutes, repeat hello sqlline.py commands tooconfirm that hello SYSTEM.CATALOG table is up, and that it can be queried.</span></span> 

<span data-ttu-id="d11c0-163">Amikor a rendszer hello. KATALÓGUS tábla hátsó toonormal, hello csatlakozási probléma tooPhoenix automatikusan feloldja kell lennie.</span><span class="sxs-lookup"><span data-stu-id="d11c0-163">When hello SYSTEM.CATALOG table is back toonormal, hello connectivity issue tooPhoenix should be automatically resolved.</span></span>


## <a name="what-causes-a-master-server-toofail-toostart"></a><span data-ttu-id="d11c0-164">Mi okozza a főkiszolgálóval toofail toostart</span><span class="sxs-lookup"><span data-stu-id="d11c0-164">What causes a master server toofail toostart</span></span>

### <a name="error"></a><span data-ttu-id="d11c0-165">Hiba</span><span class="sxs-lookup"><span data-stu-id="d11c0-165">Error</span></span> 

<span data-ttu-id="d11c0-166">Egy atomi átnevezési hiba történik.</span><span class="sxs-lookup"><span data-stu-id="d11c0-166">An atomic renaming failure occurs.</span></span>

### <a name="detailed-description"></a><span data-ttu-id="d11c0-167">Részletes leírás</span><span class="sxs-lookup"><span data-stu-id="d11c0-167">Detailed description</span></span>

<span data-ttu-id="d11c0-168">Hello indítási folyamat során HMaster sok inicializálási lépések befejeződik.</span><span class="sxs-lookup"><span data-stu-id="d11c0-168">During hello startup process, HMaster completes many initialization steps.</span></span> <span data-ttu-id="d11c0-169">Ezek közé tartozik a hello teljesen (.tmp) adatok áthelyezése toohello adatok mappáját.</span><span class="sxs-lookup"><span data-stu-id="d11c0-169">These include moving data from hello scratch (.tmp) folder toohello data folder.</span></span> <span data-ttu-id="d11c0-170">HMaster is ellenőrzi, hogy az hello írási előre naplók (WALs) mappa toosee ha összes nem válaszoló régió kiszolgálók, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="d11c0-170">HMaster also looks at hello write-ahead logs (WALs) folder toosee if there are any unresponsive region servers, and so on.</span></span> 

<span data-ttu-id="d11c0-171">Rendszerindítás közben HMaster does alapvető `list` parancs ezeket a mappákat.</span><span class="sxs-lookup"><span data-stu-id="d11c0-171">During startup, HMaster does a basic `list` command on these folders.</span></span> <span data-ttu-id="d11c0-172">Ha bármikor HMaster meglátja ezt egy váratlan fájlt ezeket a mappákat, kivételt jelez, és nem indul el.</span><span class="sxs-lookup"><span data-stu-id="d11c0-172">If at any time, HMaster sees an unexpected file in any of these folders, it throws an exception and doesn't start.</span></span>  

### <a name="probable-cause"></a><span data-ttu-id="d11c0-173">Lehetséges ok</span><span class="sxs-lookup"><span data-stu-id="d11c0-173">Probable cause</span></span>

<span data-ttu-id="d11c0-174">Hello régió server naplófájlokban próbálja tooidentify hello ütemterv hello fájl létrehozásának, és majd meg, ha hiba történt egy folyamat összeomlási körül hello idő hello fájl jött létre.</span><span class="sxs-lookup"><span data-stu-id="d11c0-174">In hello region server logs, try tooidentify hello timeline of hello file creation, and then see if there was a process crash around hello time hello file was created.</span></span> <span data-ttu-id="d11c0-175">(A HBase támogatási tooassist Forduljon, ennek során.) Ez segítséget nyújt nekünk nyújtanak robusztusabb mechanizmusok, így elkerülheti, hogy elérte-e ezt a hibát, és szabályos folyamat leállítások biztosítása.</span><span class="sxs-lookup"><span data-stu-id="d11c0-175">(Contact HBase support tooassist you in doing this.) This helps us provide more robust mechanisms, so that you can avoid hitting this bug, and ensure graceful process shutdowns.</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="d11c0-176">Megoldási lépések</span><span class="sxs-lookup"><span data-stu-id="d11c0-176">Resolution steps</span></span>

<span data-ttu-id="d11c0-177">Ellenőrizze a hello hívási verem, és próbálkozzon előfordulhat, hogy a mappa hello hibát okozó toodetermine (például lehet hello WALs vagy hello .tmp mappában).</span><span class="sxs-lookup"><span data-stu-id="d11c0-177">Check hello call stack and try toodetermine which folder might be causing hello problem (for instance, it might be hello WALs folder or hello .tmp folder).</span></span> <span data-ttu-id="d11c0-178">Ezután a Cloud Explorer vagy a HDFS parancs használatával próbálja meg toolocate hello probléma fájlt.</span><span class="sxs-lookup"><span data-stu-id="d11c0-178">Then, in Cloud Explorer or by using HDFS commands, try toolocate hello problem file.</span></span> <span data-ttu-id="d11c0-179">Általában ez jelenti a \*-renamePending.json fájlt.</span><span class="sxs-lookup"><span data-stu-id="d11c0-179">Usually, this is a \*-renamePending.json file.</span></span> <span data-ttu-id="d11c0-180">(hello \*-renamePending.json fájlt egy olyan napló fájl hello WASB illesztőprogram tooimplement hello atomi átnevezési használatban van.</span><span class="sxs-lookup"><span data-stu-id="d11c0-180">(hello \*-renamePending.json file is a journal file that's used tooimplement hello atomic rename operation in hello WASB driver.</span></span> <span data-ttu-id="d11c0-181">Kellő toobugs ebben a megvalósításban, ezeket a fájlokat is hagyható után folyamat leállásából eredő hiba, és így tovább.) Ez a fájl, a Cloud Explorer vagy a HDFS-parancsok segítségével kényszerített-törlés.</span><span class="sxs-lookup"><span data-stu-id="d11c0-181">Due toobugs in this implementation, these files can be left over after process crashes, and so on.) Force-delete this file either in Cloud Explorer or by using HDFS commands.</span></span> 

<span data-ttu-id="d11c0-182">Egyes esetekben előfordulhat, hogy is van egy ideiglenes fájlt hasonlót *$$$. $$$* ezen a helyen.</span><span class="sxs-lookup"><span data-stu-id="d11c0-182">Sometimes, there might also be a temporary file named something like *$$$.$$$* at this location.</span></span> <span data-ttu-id="d11c0-183">Toouse HDFS rendelkezik `ls` ; -nem jelennek meg a Cloud Explorer hello fájl utasítás toosee ezt a fájlt.</span><span class="sxs-lookup"><span data-stu-id="d11c0-183">You have toouse HDFS `ls` command toosee this file; you cannot see hello file in Cloud Explorer.</span></span> <span data-ttu-id="d11c0-184">toodelete ezt a fájlt, használjon hello HDFS parancs `hdfs dfs -rm /\<path>\/\$\$\$.\$\$\$`.</span><span class="sxs-lookup"><span data-stu-id="d11c0-184">toodelete this file, use hello HDFS command `hdfs dfs -rm /\<path>\/\$\$\$.\$\$\$`.</span></span>  

<span data-ttu-id="d11c0-185">Ezek a parancsok futtatása után HMaster azonnal elindul.</span><span class="sxs-lookup"><span data-stu-id="d11c0-185">After you've run these commands, HMaster should start immediately.</span></span> 

### <a name="error"></a><span data-ttu-id="d11c0-186">Hiba</span><span class="sxs-lookup"><span data-stu-id="d11c0-186">Error</span></span>

<span data-ttu-id="d11c0-187">Nincs kiszolgálói cím szerepel *hbase: meta* a régió xxx.</span><span class="sxs-lookup"><span data-stu-id="d11c0-187">No server address is listed in *hbase: meta* for region xxx.</span></span>

### <a name="detailed-description"></a><span data-ttu-id="d11c0-188">Részletes leírás</span><span class="sxs-lookup"><span data-stu-id="d11c0-188">Detailed description</span></span>

<span data-ttu-id="d11c0-189">Előfordulhat, hogy megjelenik egy üzenet, amely azt jelzi, hogy hello a Linux-fürt *hbase: meta* tábla nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="d11c0-189">You might see a message on your Linux cluster that indicates that hello *hbase: meta* table is not online.</span></span> <span data-ttu-id="d11c0-190">Futó `hbck` előfordulhat, hogy jelenti, hogy "hbase: meta tábla Replikaazonosítójú 0 nem található a bármely régióban."</span><span class="sxs-lookup"><span data-stu-id="d11c0-190">Running `hbck` might report that "hbase: meta table replicaId 0 is not found on any region."</span></span> <span data-ttu-id="d11c0-191">hello probléma lehet, hogy HMaster nem tudta inicializálni a HBase újraindítása után.</span><span class="sxs-lookup"><span data-stu-id="d11c0-191">hello problem might be that HMaster could not initialize after you restarted HBase.</span></span> <span data-ttu-id="d11c0-192">Hello HMaster naplókat, előfordulhat, hogy látja üdvözlőüzenetére: "nem kiszolgálói címet, a hbase szerepel: meta a terület hbase: biztonsági mentés \<terület neve\>".</span><span class="sxs-lookup"><span data-stu-id="d11c0-192">In hello HMaster logs, you might see hello message: "No server address listed in hbase: meta for region hbase: backup \<region name\>".</span></span>  

### <a name="resolution-steps"></a><span data-ttu-id="d11c0-193">Megoldási lépések</span><span class="sxs-lookup"><span data-stu-id="d11c0-193">Resolution steps</span></span>

1. <span data-ttu-id="d11c0-194">Hello HBase rendszerhéj adja meg a következő parancsokat (tényleges értékek módosítása megfelelő) hello:</span><span class="sxs-lookup"><span data-stu-id="d11c0-194">In hello HBase shell, enter hello following commands (change actual values as applicable):</span></span>  

   ```apache
   > scan 'hbase:meta'  
   ```

   ```apache
   > delete 'hbase:meta','hbase:backup <region name>','<column name>'  
   ```

2. <span data-ttu-id="d11c0-195">Törölje a hello *hbase: névtér* bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="d11c0-195">Delete hello *hbase: namespace* entry.</span></span> <span data-ttu-id="d11c0-196">Ez a bejegyzés lehet ugyanaz a hiba, ha hello ügynökökről hello *hbase: névtér* tábla üzemeltet.</span><span class="sxs-lookup"><span data-stu-id="d11c0-196">This entry might be hello same error that's being reported when hello *hbase: namespace* table is scanned.</span></span>

3. <span data-ttu-id="d11c0-197">mentése fut, hello Ambari felhasználói felületén, a HBase toobring hello aktív HMaster szolgáltatás újraindítása.</span><span class="sxs-lookup"><span data-stu-id="d11c0-197">toobring up HBase in a running state, in hello Ambari UI, restart hello Active HMaster service.</span></span>  

4. <span data-ttu-id="d11c0-198">A HBase rendszerhéj, az összes kapcsolat nélküli tábláit toobring hello futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="d11c0-198">In hello HBase shell, toobring up all offline tables, run hello following command:</span></span>

   ```apache 
   hbase hbck -ignorePreCheckPermission -fixAssignments 
   ```

### <a name="additional-reading"></a><span data-ttu-id="d11c0-199">További olvasnivaló</span><span class="sxs-lookup"><span data-stu-id="d11c0-199">Additional reading</span></span>

[<span data-ttu-id="d11c0-200">Nem lehet tooprocess hello HBase tábla</span><span class="sxs-lookup"><span data-stu-id="d11c0-200">Unable tooprocess hello HBase table</span></span>](http://stackoverflow.com/questions/4794092/unable-to-access-hbase-table)


### <a name="error"></a><span data-ttu-id="d11c0-201">Hiba</span><span class="sxs-lookup"><span data-stu-id="d11c0-201">Error</span></span>

<span data-ttu-id="d11c0-202">Egy végzetes kivétel hasonló too"java.io.IOException időtúllépése HMaster: Várakozás a névtér tábla toobe hozzárendelt időtúllépésbe került 300000ms."</span><span class="sxs-lookup"><span data-stu-id="d11c0-202">HMaster times out with a fatal exception similar too"java.io.IOException: Timedout 300000ms waiting for namespace table toobe assigned."</span></span>

### <a name="detailed-description"></a><span data-ttu-id="d11c0-203">Részletes leírás</span><span class="sxs-lookup"><span data-stu-id="d11c0-203">Detailed description</span></span>

<span data-ttu-id="d11c0-204">A probléma tapasztalhat, ha sok táblák és régiók, amely nem ki lettek ürítve a HMaster szolgáltatás újraindításakor.</span><span class="sxs-lookup"><span data-stu-id="d11c0-204">You might experience this issue if you have many tables and regions that have not been flushed when you restart your HMaster services.</span></span> <span data-ttu-id="d11c0-205">Újraindítás sikertelen lehet, és látni fogja, hibaüzenet megelőző hello.</span><span class="sxs-lookup"><span data-stu-id="d11c0-205">Restart might fail, and you'll see hello preceding error message.</span></span>  

### <a name="probable-cause"></a><span data-ttu-id="d11c0-206">Lehetséges ok</span><span class="sxs-lookup"><span data-stu-id="d11c0-206">Probable cause</span></span>

<span data-ttu-id="d11c0-207">Ez az egy ismert probléma az hello HMaster szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="d11c0-207">This is a known issue with hello HMaster service.</span></span> <span data-ttu-id="d11c0-208">Általános fürt indítási feladatok hosszú időt vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="d11c0-208">General cluster startup tasks can take a long time.</span></span> <span data-ttu-id="d11c0-209">HMaster leáll, mert hello névtér tábla még nincs hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="d11c0-209">HMaster shuts down because hello namespace table isn’t yet assigned.</span></span> <span data-ttu-id="d11c0-210">Ez akkor fordul elő, csak a forgatókönyvek, ahol nagy adatmennyiség unflushed létezik, és egy öt perces időkorlát túl kicsi.</span><span class="sxs-lookup"><span data-stu-id="d11c0-210">This occurs only in scenarios in which large amount of unflushed data exists, and a timeout of five minutes is not sufficient.</span></span>
  
### <a name="resolution-steps"></a><span data-ttu-id="d11c0-211">Megoldási lépések</span><span class="sxs-lookup"><span data-stu-id="d11c0-211">Resolution steps</span></span>

1. <span data-ttu-id="d11c0-212">Hello Ambari felhasználói felület, a go túl**HBase** > **Configs**.</span><span class="sxs-lookup"><span data-stu-id="d11c0-212">In hello Ambari UI, go too**HBase** > **Configs**.</span></span> <span data-ttu-id="d11c0-213">Hello egyéni hbase-site.xml fájlban adja hozzá a következő beállítás hello:</span><span class="sxs-lookup"><span data-stu-id="d11c0-213">In hello custom hbase-site.xml file, add hello following setting:</span></span> 

   ```apache
   Key: hbase.master.namespace.init.timeout Value: 2400000  
   ```

2. <span data-ttu-id="d11c0-214">Indítsa újra a hello szükséges szolgáltatásokat (HMaster, és valószínűleg más HBase szolgáltatások).</span><span class="sxs-lookup"><span data-stu-id="d11c0-214">Restart hello required services (HMaster, and possibly other HBase services).</span></span>  


## <a name="what-causes-a-restart-failure-on-a-region-server"></a><span data-ttu-id="d11c0-215">Mi újraindítás hibát okoz a terület-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="d11c0-215">What causes a restart failure on a region server</span></span>

### <a name="issue"></a><span data-ttu-id="d11c0-216">Probléma</span><span class="sxs-lookup"><span data-stu-id="d11c0-216">Issue</span></span>

<span data-ttu-id="d11c0-217">Előfordulhat, hogy régió kiszolgáló újraindítása meghibásodása megakadályozása következő bevált gyakorlatát.</span><span class="sxs-lookup"><span data-stu-id="d11c0-217">A restart failure on a region server might be prevented by following best practices.</span></span> <span data-ttu-id="d11c0-218">Azt javasoljuk, hogy nagy munkaterhelést tevékenység szünetelteti az toorestart HBase régió kiszolgálók tervezése során.</span><span class="sxs-lookup"><span data-stu-id="d11c0-218">We recommend that you pause heavy workload activity when you are planning toorestart HBase region servers.</span></span> <span data-ttu-id="d11c0-219">Ha egy alkalmazás régió kiszolgálókkal tooconnect továbbra is, ha leállítása folyamatban van, hello régió server Újraindítási művelet lassabb lesz által néhány percig.</span><span class="sxs-lookup"><span data-stu-id="d11c0-219">If an application continues tooconnect with region servers when shutdown is in progress, hello region server restart operation will be slower by several minutes.</span></span> <span data-ttu-id="d11c0-220">Azt is egy jó ötlet toofirst kiürítési összes hello táblákat.</span><span class="sxs-lookup"><span data-stu-id="d11c0-220">Also, it's a good idea toofirst flush all hello tables.</span></span> <span data-ttu-id="d11c0-221">Hogyan referenciáért tooflush táblák, lásd: [HDInsight HBase: hogyan indítsa újra a tooimprove hello HBase-fürtöt a idő, táblák kiürítési](https://blogs.msdn.microsoft.com/azuredatalake/2016/09/19/hdinsight-hbase-how-to-improve-hbase-cluster-restart-time-by-flushing-tables/).</span><span class="sxs-lookup"><span data-stu-id="d11c0-221">For a reference on how tooflush tables, see [HDInsight HBase: How tooimprove hello HBase cluster restart time by flushing tables](https://blogs.msdn.microsoft.com/azuredatalake/2016/09/19/hdinsight-hbase-how-to-improve-hbase-cluster-restart-time-by-flushing-tables/).</span></span>

<span data-ttu-id="d11c0-222">Ha elindította hello Újraindítási művelet hello Ambari felhasználói felület a HBase régió kiszolgálókon, azonnal látni hello régió kiszolgálók csökkent, de azok azonnal ne indítsa újra.</span><span class="sxs-lookup"><span data-stu-id="d11c0-222">If you initiate hello restart operation on HBase region servers from hello Ambari UI, you immediately see that hello region servers went down, but they don't restart right away.</span></span> 

<span data-ttu-id="d11c0-223">Mi történik a háttérben hello itt található:</span><span class="sxs-lookup"><span data-stu-id="d11c0-223">Here's what's happening behind hello scenes:</span></span> 

1. <span data-ttu-id="d11c0-224">hello Ambari ügynök elküld egy leállítási kérelem toohello régió kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="d11c0-224">hello Ambari agent sends a stop request toohello region server.</span></span>
2. <span data-ttu-id="d11c0-225">hello Ambari ügynök megvárja-e a 30 másodpercet hello régió server tooshut le szabályosan.</span><span class="sxs-lookup"><span data-stu-id="d11c0-225">hello Ambari agent waits for 30 seconds for hello region server tooshut down gracefully.</span></span> 
3. <span data-ttu-id="d11c0-226">Ha az alkalmazás továbbra is tooconnect hello régió kiszolgálóval, hello kiszolgáló nem áll le azonnal.</span><span class="sxs-lookup"><span data-stu-id="d11c0-226">If your application continues tooconnect with hello region server, hello server won't shut down immediately.</span></span> <span data-ttu-id="d11c0-227">hello 30 másodperces időtúllépési előtt leállítás lejár.</span><span class="sxs-lookup"><span data-stu-id="d11c0-227">hello 30-second timeout expires before shutdown occurs.</span></span> 
4. <span data-ttu-id="d11c0-228">30 másodperc hello Ambari ügynök küld egy kényszerített kill (`kill -9`) parancs toohello régió kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="d11c0-228">After 30 seconds, hello Ambari agent sends a force-kill (`kill -9`) command toohello region server.</span></span> <span data-ttu-id="d11c0-229">Erre úgy tekinthet lévő hello ambari-ügynök (hello /var/napló/directory hello megfelelő munkavégző csomópont):</span><span class="sxs-lookup"><span data-stu-id="d11c0-229">You can see this in hello ambari-agent log (in hello /var/log/ directory of hello respective worker node):</span></span>

   ```apache
           2017-03-21 13:22:09,171 - Execute['/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh --config /usr/hdp/current/hbase-regionserver/conf stop regionserver'] {'only_if': 'ambari-sudo.sh  -H -E t
           est -f /var/run/hbase/hbase-hbase-regionserver.pid && ps -p `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid` >/dev/null 2>&1', 'on_timeout': '! ( ambari-sudo.sh  -H -E test -
           f /var/run/hbase/hbase-hbase-regionserver.pid && ps -p `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid` >/dev/null 2>&1 ) || ambari-sudo.sh -H -E kill -9 `ambari-sudo.sh  -H 
           -E cat /var/run/hbase/hbase-hbase-regionserver.pid`', 'timeout': 30, 'user': 'hbase'}
           2017-03-21 13:22:40,268 - Executing '! ( ambari-sudo.sh  -H -E test -f /var/run/hbase/hbase-hbase-regionserver.pid && ps -p `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid` >
           /dev/null 2>&1 ) || ambari-sudo.sh -H -E kill -9 `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid`'. Reason: Execution of 'ambari-sudo.sh su hbase -l -s /bin/bash -c 'export  
           PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/var/lib/ambari-agent ; /usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh --config /usr/hdp/curre
           nt/hbase-regionserver/conf stop regionserver was killed due timeout after 30 seconds
           2017-03-21 13:22:40,285 - File['/var/run/hbase/hbase-hbase-regionserver.pid'] {'action': ['delete']}
           2017-03-21 13:22:40,285 - Deleting File['/var/run/hbase/hbase-hbase-regionserver.pid']
   ```
<span data-ttu-id="d11c0-230">Hello lezárás miatt hello folyamathoz tartozó hello port előfordulhat, hogy nem oldhatók fel, annak ellenére, hogy hello régió kiszolgáló folyamata leállt.</span><span class="sxs-lookup"><span data-stu-id="d11c0-230">Because of hello abrupt shutdown, hello port associated with hello process might not be released, even though hello region server process is stopped.</span></span> <span data-ttu-id="d11c0-231">Ez a helyzet vezethet tooan AddressBindException hello régió kiszolgáló indításakor hello naplók a következő ábrán.</span><span class="sxs-lookup"><span data-stu-id="d11c0-231">This situation can lead tooan AddressBindException when hello region server is starting, as shown in hello following logs.</span></span> <span data-ttu-id="d11c0-232">A hello régió-Server.log elérési úton található hello /var/log/hbase könyvtárában, ha hello régió kiszolgáló nem toostart hello munkavégző csomópontokhoz ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="d11c0-232">You can verify this in hello region-server.log in hello /var/log/hbase directory on hello worker nodes where hello region server fails toostart.</span></span> 

   ```apache

   2017-03-21 13:25:47,061 ERROR [main] regionserver.HRegionServerCommandLine: Region server exiting
   java.lang.RuntimeException: Failed construction of Regionserver: class org.apache.hadoop.hbase.regionserver.HRegionServer
   at org.apache.hadoop.hbase.regionserver.HRegionServer.constructRegionServer(HRegionServer.java:2636)
   at org.apache.hadoop.hbase.regionserver.HRegionServerCommandLine.start(HRegionServerCommandLine.java:64)
   at org.apache.hadoop.hbase.regionserver.HRegionServerCommandLine.run(HRegionServerCommandLine.java:87)
   at org.apache.hadoop.util.ToolRunner.run(ToolRunner.java:76)
   at org.apache.hadoop.hbase.util.ServerCommandLine.doMain(ServerCommandLine.java:126)
   at org.apache.hadoop.hbase.regionserver.HRegionServer.main(HRegionServer.java:2651)
        
   Caused by: java.lang.reflect.InvocationTargetException
   at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
   at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:57)
   at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
   at java.lang.reflect.Constructor.newInstance(Constructor.java:526)
   at org.apache.hadoop.hbase.regionserver.HRegionServer.constructRegionServer(HRegionServer.java:2634)
   ... 5 more
        
   Caused by: java.net.BindException: Problem binding too/10.2.0.4:16020 : Address already in use
   at org.apache.hadoop.hbase.ipc.RpcServer.bind(RpcServer.java:2497)
   at org.apache.hadoop.hbase.ipc.RpcServer$Listener.<init>(RpcServer.java:580)
   at org.apache.hadoop.hbase.ipc.RpcServer.<init>(RpcServer.java:1982)
   at org.apache.hadoop.hbase.regionserver.RSRpcServices.<init>(RSRpcServices.java:863)
   at org.apache.hadoop.hbase.regionserver.HRegionServer.createRpcServices(HRegionServer.java:632)
   at org.apache.hadoop.hbase.regionserver.HRegionServer.<init>(HRegionServer.java:532)
   ... 10 more
        
   Caused by: java.net.BindException: Address already in use
   at sun.nio.ch.Net.bind0(Native Method)
   at sun.nio.ch.Net.bind(Net.java:463)
   at sun.nio.ch.Net.bind(Net.java:455)
   at sun.nio.ch.ServerSocketChannelImpl.bind(ServerSocketChannelImpl.java:223)
   at sun.nio.ch.ServerSocketAdaptor.bind(ServerSocketAdaptor.java:74)
   at org.apache.hadoop.hbase.ipc.RpcServer.bind(RpcServer.java:2495)
   ... 15 more
   ```

### <a name="resolution-steps"></a><span data-ttu-id="d11c0-233">Megoldási lépések</span><span class="sxs-lookup"><span data-stu-id="d11c0-233">Resolution steps</span></span>

1. <span data-ttu-id="d11c0-234">Próbálja meg tooreduce hello terhelés hello HBase régió kiszolgálókon, még újraindítás elindítása előtt.</span><span class="sxs-lookup"><span data-stu-id="d11c0-234">Try tooreduce hello load on hello HBase region servers before you initiate a restart.</span></span> 
2. <span data-ttu-id="d11c0-235">Másik lehetőségként (ha 1. lépésben nem oldották), a próbálja toomanually újraindítás régió hello munkavégző csomóponton a kiszolgálókon hello a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="d11c0-235">Alternatively (if step 1 doesn't help), try toomanually restart region servers on hello worker nodes by using hello following commands:</span></span>

   ```apache
   sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh stop regionserver"
   sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh start regionserver"   
   ```

