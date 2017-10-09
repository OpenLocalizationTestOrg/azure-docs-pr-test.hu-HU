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
# <a name="troubleshoot-hbase-by-using-azure-hdinsight"></a>A HBase hibaelhárítása az Azure HDInsight használatával

Hello legfőbb problémákat és azok megoldásait ismerje meg az Apache Ambari az Apache HBase hasznos adat található használatakor.

## <a name="how-do-i-run-hbck-command-reports-with-multiple-unassigned-regions"></a>Hogyan futtathatok hbck parancs jelentéseket a ki nem osztott több régióba

Egy általános hibaüzenetet, hogy mikor fusson a hello megjelenhet `hbase hbck` parancs a "több régióba alatt hozzá nem rendelt vagy régiókban hello láncában lyuk."

A HBase fő felhasználói felület hello megtekintheti a hello több régióban, amely minden régióban kiszolgálóján is tette. Ezt követően futtathatja `hbase hbck` toosee lyuk hello régió lánc parancsot.

Lyuk oka lehet hello offline régiókból, így javítás hello hozzárendelések először. 

toobring hello ki nem osztott régiók hátsó tooa normál állapotban, végezze el az alábbi lépésekkel hello:

1. Jelentkezzen be toohello HDInsight HBase-fürtöt SSH használatával.
2. a hello ZooKeeper rendszerhéj, futtassa a hello tooconnect `hbase zkcli` parancsot.
3. Futtassa a hello `rmr /hbase/regions-in-transition` parancs vagy hello `rmr /hbase-unsecure/regions-in-transition` parancsot.
4. a hello tooexit `hbase zkcli` rendszerhéj, használja a hello `exit` parancsot.
5. Nyissa meg a hello Apache Ambari felhasználói felület, és indítsa újra hello aktív HBase fő szolgáltatás.
6. Futtassa a hello `hbase hbck` parancsot ismét (kapcsolók) nélkül. Ellenőrizze a parancs tooensure minden egyes hozzárendelt hello kimenetét.


## <a name="how-do-i-fix-timeout-issues-with-hbck-commands-for-region-assignments"></a>Hogyan oldja meg időtúllépés problémák, régió hozzárendelések hbck parancsok használata esetén

### <a name="issue"></a>Probléma

Egyik lehetséges oka az időtúllépési problémák hello használatakor `hbck` parancs lehet, hogy több régiók a következők a hello "az átmenet" állapot hosszú ideig. Azokban a régiókban, offline állapotú hello HBase fő felhasználói felületén tekintheti meg. Régiók nagy mennyiségű próbált tootransition, mert a HBase fő időtúllépés lehet, és nem toobring újra online azokban a régiókban kell.

### <a name="resolution-steps"></a>Megoldási lépések

1. Jelentkezzen be toohello HDInsight HBase-fürtöt SSH használatával.
2. a hello ZooKeeper rendszerhéj, futtassa a hello tooconnect `hbase zkcli` parancsot.
3. Futtassa a hello `rmr /hbase/regions-in-transition` vagy hello `rmr /hbase-unsecure/regions-in-transition` parancsot.
4. tooexit hello `hbase zkcli` rendszerhéj, használja a hello `exit` parancsot.
5. Hello Ambari felhasználói felület indítsa újra az hello aktív HBase fő szolgáltatás.
6. Futtassa a hello `hbase hbck -fixAssignments` újra a parancsot.

## <a name="how-do-i-force-disable-hdfs-safe-mode-in-a-cluster"></a>Hogyan tegye I kényszerített – letiltása HDFS csökkentett módban egy fürtben

### <a name="issue"></a>Probléma

hello helyi Hadoop elosztott fájlrendszerrel (HDFS) Beragadt hello HDInsight-fürt csökkentett módban.

### <a name="detailed-description"></a>Részletes leírás

Ez a hiba oka lehet egy hiba hello HDFS parancs a következő futtatásakor:

```apache
hdfs dfs -D "fs.default.name=hdfs://mycluster/" -mkdir /temp
```

hello hiba láthatja, amikor megpróbál toorun hello parancs így néz ki:

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

### <a name="probable-cause"></a>Lehetséges ok

hello HDInsight-fürt már le tooa méretezhető nagyon kevés csomópontot. hello csomópontok száma nem éri el, vagy zárja be a toohello HDFS replikációs tényező.

### <a name="resolution-steps"></a>Megoldási lépések 

1. A HDFS a hello HDInsight fürt hello a következő parancsok futtatásával hello hello állapotának beolvasása:

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
2. HDFS a hello HDInsight fürt hello a következő parancsok használatával hello hello integritását is ellenőrizheti:

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

3. Ha azt állapítja meg, hogy vannak-e nem hiányzik, sérült vagy under-replikált blokkolja, vagy az, hogy ezek a blokkok figyelmen kívül hagyható, futtassa a következő parancs tootake hello neve csomópont biztonságos módból hello:

   ```apache
   hdfs dfsadmin -D "fs.default.name=hdfs://mycluster/" -safemode leave
   ```


## <a name="how-do-i-fix-jdbc-or-sqlline-connectivity-issues-with-apache-phoenix"></a>Hogyan állítsa helyre JDBC vagy SQLLine elérhetőségét Apache Phoenix problémái

### <a name="resolution-steps"></a>Megoldási lépések

a Phoenix tooconnect, meg kell adnia az aktív ZooKeeper csomópont hello IP-címét. Győződjön meg arról, hogy hello szolgáltatás toowhich sqlline.py próbál ZooKeeper tooconnect megfelelően működik, és.
1. Jelentkezzen be toohello HDInsight-fürthöz SSH használatával.
2. Adja meg a következő parancs hello:
                
   ```apache
           "/usr/hdp/current/phoenix-client/bin/sqlline.py <IP of machine where Active Zookeeper is running"
   ```

   > [!Note] 
   > Hello IP-cím hello aktív ZooKeeper csomópontjának az Ambari felhasználói felület hello kérheti le. Nyissa meg túl**HBase** > **Gyorshivatkozások** > **ZK\* (aktív)** > **Zookeeperadatai**. 

3. Ha hello sqlline.py tooPhoenix csatlakozik, és amelyen nincs időtúllépés, a futtatási hello következő parancsot a toovalidate hello rendelkezésre állás és a Phoenix állapotát:

   ```apache
           !tables
           !quit
   ```      
4. Ez a parancs működik, ha nincs probléma. hello felhasználó által megadott hello IP-cím helytelen lehet. Azonban ha majd jeleníti meg a következő hiba hello hello parancs hosszabb ideig szünetelteti, továbbra is toostep 5.

   ```apache
           Error while connecting toosqlline.py (Hbase - phoenix) Setting property: [isolation, TRANSACTION_READ_COMMITTED] issuing: !connect jdbc:phoenix:10.2.0.7 none none org.apache.phoenix.jdbc.PhoenixDriver Connecting toojdbc:phoenix:10.2.0.7 SLF4J: Class path contains multiple SLF4J bindings. 
   ```

5. Futtassa a parancsokat követően – hello átjárócsomópont (hn0) toodiagnose hello feltétele hello Phoenix rendszer hello. KATALÓGUS tábla:

   ```apache
            hbase shell
                
           count 'SYSTEM.CATALOG'
   ```

   hello parancs visszaadja-e egy hiba hasonló toohello következő: 

   ```apache
           ERROR: org.apache.hadoop.hbase.NotServingRegionException: Region SYSTEM.CATALOG,,1485464083256.c0568c94033870c517ed36c45da98129. is not online on 10.2.0.5,16020,1489466172189) 
   ```
6. Hello Ambari felhasználói felület végeznie hello lépéseket toorestart hello HMaster szolgáltatás minden ZooKeeper csomópontján a következő:

    1. A hello **összegzés** szakasz a HBase, nyissa meg túl**HBase** > **aktív HBase fő**. 
    2. A hello **összetevők** területen hello HBase fő szolgáltatás újraindításához.
    3. Ismételje meg ezeket a lépéseket minden fennmaradó **készenléti HBase fő** szolgáltatások. 

Ez hello HBase fő szolgáltatás toostabilize toofive percig tarthat, és hello helyreállítási folyamat befejezéséhez. Néhány perc elteltével ismételje meg hello sqlline.py parancsok tooconfirm adott hello rendszer. KATALÓGUS tábla működik-e, és, hogy az informatikai kérdezhetők le. 

Amikor a rendszer hello. KATALÓGUS tábla hátsó toonormal, hello csatlakozási probléma tooPhoenix automatikusan feloldja kell lennie.


## <a name="what-causes-a-master-server-toofail-toostart"></a>Mi okozza a főkiszolgálóval toofail toostart

### <a name="error"></a>Hiba 

Egy atomi átnevezési hiba történik.

### <a name="detailed-description"></a>Részletes leírás

Hello indítási folyamat során HMaster sok inicializálási lépések befejeződik. Ezek közé tartozik a hello teljesen (.tmp) adatok áthelyezése toohello adatok mappáját. HMaster is ellenőrzi, hogy az hello írási előre naplók (WALs) mappa toosee ha összes nem válaszoló régió kiszolgálók, és így tovább. 

Rendszerindítás közben HMaster does alapvető `list` parancs ezeket a mappákat. Ha bármikor HMaster meglátja ezt egy váratlan fájlt ezeket a mappákat, kivételt jelez, és nem indul el.  

### <a name="probable-cause"></a>Lehetséges ok

Hello régió server naplófájlokban próbálja tooidentify hello ütemterv hello fájl létrehozásának, és majd meg, ha hiba történt egy folyamat összeomlási körül hello idő hello fájl jött létre. (A HBase támogatási tooassist Forduljon, ennek során.) Ez segítséget nyújt nekünk nyújtanak robusztusabb mechanizmusok, így elkerülheti, hogy elérte-e ezt a hibát, és szabályos folyamat leállítások biztosítása.

### <a name="resolution-steps"></a>Megoldási lépések

Ellenőrizze a hello hívási verem, és próbálkozzon előfordulhat, hogy a mappa hello hibát okozó toodetermine (például lehet hello WALs vagy hello .tmp mappában). Ezután a Cloud Explorer vagy a HDFS parancs használatával próbálja meg toolocate hello probléma fájlt. Általában ez jelenti a \*-renamePending.json fájlt. (hello \*-renamePending.json fájlt egy olyan napló fájl hello WASB illesztőprogram tooimplement hello atomi átnevezési használatban van. Kellő toobugs ebben a megvalósításban, ezeket a fájlokat is hagyható után folyamat leállásából eredő hiba, és így tovább.) Ez a fájl, a Cloud Explorer vagy a HDFS-parancsok segítségével kényszerített-törlés. 

Egyes esetekben előfordulhat, hogy is van egy ideiglenes fájlt hasonlót *$$$. $$$* ezen a helyen. Toouse HDFS rendelkezik `ls` ; -nem jelennek meg a Cloud Explorer hello fájl utasítás toosee ezt a fájlt. toodelete ezt a fájlt, használjon hello HDFS parancs `hdfs dfs -rm /\<path>\/\$\$\$.\$\$\$`.  

Ezek a parancsok futtatása után HMaster azonnal elindul. 

### <a name="error"></a>Hiba

Nincs kiszolgálói cím szerepel *hbase: meta* a régió xxx.

### <a name="detailed-description"></a>Részletes leírás

Előfordulhat, hogy megjelenik egy üzenet, amely azt jelzi, hogy hello a Linux-fürt *hbase: meta* tábla nem érhető el. Futó `hbck` előfordulhat, hogy jelenti, hogy "hbase: meta tábla Replikaazonosítójú 0 nem található a bármely régióban." hello probléma lehet, hogy HMaster nem tudta inicializálni a HBase újraindítása után. Hello HMaster naplókat, előfordulhat, hogy látja üdvözlőüzenetére: "nem kiszolgálói címet, a hbase szerepel: meta a terület hbase: biztonsági mentés \<terület neve\>".  

### <a name="resolution-steps"></a>Megoldási lépések

1. Hello HBase rendszerhéj adja meg a következő parancsokat (tényleges értékek módosítása megfelelő) hello:  

   ```apache
   > scan 'hbase:meta'  
   ```

   ```apache
   > delete 'hbase:meta','hbase:backup <region name>','<column name>'  
   ```

2. Törölje a hello *hbase: névtér* bejegyzés. Ez a bejegyzés lehet ugyanaz a hiba, ha hello ügynökökről hello *hbase: névtér* tábla üzemeltet.

3. mentése fut, hello Ambari felhasználói felületén, a HBase toobring hello aktív HMaster szolgáltatás újraindítása.  

4. A HBase rendszerhéj, az összes kapcsolat nélküli tábláit toobring hello futtassa a következő parancs hello:

   ```apache 
   hbase hbck -ignorePreCheckPermission -fixAssignments 
   ```

### <a name="additional-reading"></a>További olvasnivaló

[Nem lehet tooprocess hello HBase tábla](http://stackoverflow.com/questions/4794092/unable-to-access-hbase-table)


### <a name="error"></a>Hiba

Egy végzetes kivétel hasonló too"java.io.IOException időtúllépése HMaster: Várakozás a névtér tábla toobe hozzárendelt időtúllépésbe került 300000ms."

### <a name="detailed-description"></a>Részletes leírás

A probléma tapasztalhat, ha sok táblák és régiók, amely nem ki lettek ürítve a HMaster szolgáltatás újraindításakor. Újraindítás sikertelen lehet, és látni fogja, hibaüzenet megelőző hello.  

### <a name="probable-cause"></a>Lehetséges ok

Ez az egy ismert probléma az hello HMaster szolgáltatás. Általános fürt indítási feladatok hosszú időt vehet igénybe. HMaster leáll, mert hello névtér tábla még nincs hozzárendelve. Ez akkor fordul elő, csak a forgatókönyvek, ahol nagy adatmennyiség unflushed létezik, és egy öt perces időkorlát túl kicsi.
  
### <a name="resolution-steps"></a>Megoldási lépések

1. Hello Ambari felhasználói felület, a go túl**HBase** > **Configs**. Hello egyéni hbase-site.xml fájlban adja hozzá a következő beállítás hello: 

   ```apache
   Key: hbase.master.namespace.init.timeout Value: 2400000  
   ```

2. Indítsa újra a hello szükséges szolgáltatásokat (HMaster, és valószínűleg más HBase szolgáltatások).  


## <a name="what-causes-a-restart-failure-on-a-region-server"></a>Mi újraindítás hibát okoz a terület-kiszolgálón

### <a name="issue"></a>Probléma

Előfordulhat, hogy régió kiszolgáló újraindítása meghibásodása megakadályozása következő bevált gyakorlatát. Azt javasoljuk, hogy nagy munkaterhelést tevékenység szünetelteti az toorestart HBase régió kiszolgálók tervezése során. Ha egy alkalmazás régió kiszolgálókkal tooconnect továbbra is, ha leállítása folyamatban van, hello régió server Újraindítási művelet lassabb lesz által néhány percig. Azt is egy jó ötlet toofirst kiürítési összes hello táblákat. Hogyan referenciáért tooflush táblák, lásd: [HDInsight HBase: hogyan indítsa újra a tooimprove hello HBase-fürtöt a idő, táblák kiürítési](https://blogs.msdn.microsoft.com/azuredatalake/2016/09/19/hdinsight-hbase-how-to-improve-hbase-cluster-restart-time-by-flushing-tables/).

Ha elindította hello Újraindítási művelet hello Ambari felhasználói felület a HBase régió kiszolgálókon, azonnal látni hello régió kiszolgálók csökkent, de azok azonnal ne indítsa újra. 

Mi történik a háttérben hello itt található: 

1. hello Ambari ügynök elküld egy leállítási kérelem toohello régió kiszolgáló.
2. hello Ambari ügynök megvárja-e a 30 másodpercet hello régió server tooshut le szabályosan. 
3. Ha az alkalmazás továbbra is tooconnect hello régió kiszolgálóval, hello kiszolgáló nem áll le azonnal. hello 30 másodperces időtúllépési előtt leállítás lejár. 
4. 30 másodperc hello Ambari ügynök küld egy kényszerített kill (`kill -9`) parancs toohello régió kiszolgáló. Erre úgy tekinthet lévő hello ambari-ügynök (hello /var/napló/directory hello megfelelő munkavégző csomópont):

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
Hello lezárás miatt hello folyamathoz tartozó hello port előfordulhat, hogy nem oldhatók fel, annak ellenére, hogy hello régió kiszolgáló folyamata leállt. Ez a helyzet vezethet tooan AddressBindException hello régió kiszolgáló indításakor hello naplók a következő ábrán. A hello régió-Server.log elérési úton található hello /var/log/hbase könyvtárában, ha hello régió kiszolgáló nem toostart hello munkavégző csomópontokhoz ellenőrizheti. 

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

### <a name="resolution-steps"></a>Megoldási lépések

1. Próbálja meg tooreduce hello terhelés hello HBase régió kiszolgálókon, még újraindítás elindítása előtt. 
2. Másik lehetőségként (ha 1. lépésben nem oldották), a próbálja toomanually újraindítás régió hello munkavégző csomóponton a kiszolgálókon hello a következő parancsokat:

   ```apache
   sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh stop regionserver"
   sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh start regionserver"   
   ```

