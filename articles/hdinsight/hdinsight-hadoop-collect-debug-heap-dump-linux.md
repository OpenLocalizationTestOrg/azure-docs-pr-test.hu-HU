---
title: "a HDInsight - Azure Hadoop szolgáltatások listázása aaaEnable halommemória |} Microsoft Docs"
description: "A Hibakeresés és elemzésére szolgáló Hadoop Linux-alapú HDInsight-fürtök szolgáltatásai halommemória memóriaképek engedélyezése."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 8f151adb-f687-41e4-aca0-82b551953725
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: 49e30f26e1a83f19e068e9da253b5548caec70d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-heap-dumps-for-hadoop-services-on-linux-based-hdinsight"></a>Halommemória memóriaképek a Linux-alapú HDInsight Hadoop-szolgáltatások engedélyezése

[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

Halommemória memóriaképek tartalmazhat hello alkalmazás memória, beleértve a változók értékeinek hello hello időpontban hello memóriakép létrehozása egy pillanatkép. Ezért futás közben felmerülő problémák diagnosztizálásához.

> [!IMPORTANT]
> hello ebben a dokumentumban csak a lépések Linux használó HDInsight-fürtökkel. Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="whichServices"></a>Szolgáltatások

A következő szolgáltatások hello halommemória memóriaképek engedélyezéséhez:

* **hcatalog** -tempelton
* **Hive** -hiveserver2-n, metaadattárhoz, derbyserver
* **mapreduce** -jobhistoryserver
* **yarn** -resourcemanager, nodemanager, timelineserver
* **hdfs** -datanode, secondarynamenode, namenode

Is engedélyezheti a halommemória memóriaképek hello térkép és csökkentse a HDInsight által futtatott folyamatok.

## <a name="configuration"></a>Understanding halommemória memóriakép konfiguráció

Úgy, hogy a beállítások engedélyezve vannak a halommemória memóriaképek (néven is ismert, azt, vagy a paraméterek) toohello JVM-et, a szolgáltatás indításakor. A legtöbb Hadoop-szolgáltatásokra módosíthatja hello rendszerhéj használt parancsfájl toostart hello szolgáltatás toopass ezeket a beállításokat.

Minden parancsprogram esetén nincs az exportálás  **\* \_OPTS**, hello beállításokat tartalmazó átadott toohello JVM-et. Például a hello **hadoop-env.sh** parancsprogramot, kezdődő hello sort `export HADOOP_NAMENODE_OPTS=` hello NameNode szolgáltatás hello beállításait tartalmazza.

Rendelve, és csökkentse folyamatok kissé eltérő, mivel ezek a műveletek egy hello MapReduce szolgáltatás folyamat. Minden egyes hozzárendelését, vagy csökkentse folyamat fut egy gyermek tárolóban, és két hello JVM beállításokat tartalmazó bejegyzést is tartalmaz. Mindkét szereplő **mapred-site.xml**:

* **mapreduce.Admin.Map.child.Java.opts**
* **mapreduce.Admin.reduce.child.Java.opts**

> [!NOTE]
> Javasoljuk az Ambari segítségével mindkét toomodify hello parancsfájlok és mapred-site.xml beállítások, mint Ambari kezelni replikálni a módosításokat hello fürt csomópontjai között. Lásd: hello [Ambari használatával](#using-ambari) szakasz lépéseit.

### <a name="enable-heap-dumps"></a>Halomürítések engedélyezése

hello következő beállítással halommemória memóriaképek egy OutOfMemoryError esetén:

    -XX:+HeapDumpOnOutOfMemoryError

Hello  **+**  azt jelzi, hogy ez a beállítás engedélyezve van-e. hello alapértelmezett le van tiltva.

> [!WARNING]
> Halommemória memóriaképek nem engedélyezettek a HDInsight Hadoop-szolgáltatás alapértelmezés szerint, hello memóriaképek tekintélyes lehet. Ha engedélyezi ezeket a hibaelhárításhoz, ne felejtse el azokat, amennyiben rendelkezik másolható hello problémát és az összegyűjtött hello memóriaképek toodisable.

### <a name="dump-location"></a>Biztonsági másolat helye

hello alapértelmezett hello memóriakép helye hello aktuális munkakönyvtárban. Szabályozhatja, ahol hello fájlt tárolja a következő beállítás hello használata:

    -XX:HeapDumpPath=/path

Használata esetén például `-XX:HeapDumpPath=/tmp` hello memóriaképek toobe hello könyvtárban találhatók okozza.

### <a name="scripts"></a>Parancsprogramok

Egy parancsfájlt is el lehet indítani amikor egy **OutOfMemoryError** következik be. Például egy értesítés időt. így megtudhatja, hogy hello hiba történt. Használjon hello következő beállítást tootrigger parancsfájl egy __OutOfMemoryError__:

    -XX:OnOutOfMemoryError=/path/to/script

> [!NOTE]
> Mivel a Hadoop elosztott rendszer, bármely használt parancsfájl hello hello szolgáltatás fut a fürt minden csomópontján kell elhelyezni.
> 
> hello parancsfájl kell is lehet, hogy elérhető-e hello fiók hello szolgáltatás fut, és biztosítania kell a helyre végrehajtási engedélyeket. Például előfordulhat, hogy kívánja toostore parancsfájlok `/usr/local/bin` és `chmod go+rx /usr/local/bin/filename.sh` toogrant olvasási és végrehajtási engedélyeket.

## <a name="using-ambari"></a>Ambari használatával

egy szolgáltatás, a lépéseket követve használata hello toomodify hello konfigurációja:

1. Nyissa meg a fürt hello Ambari webes felhasználói Felületét. hello URL-címe: https://YOURCLUSTERNAME.azurehdinsight.net.

    Amikor a rendszer kéri, hitelesítéshez toohello hely hello HTTP-fiók nevének (alapértelmezett: admin) és a jelszót a fürt számára.

   > [!NOTE]
   > Kérheti másodszor által Ambari hello felhasználónevet és jelszót. Ha igen, adja meg ugyanazt a fióknevet és jelszót hello

2. Hello listája hello bal oldali meg és jelölje ki azt szeretné, hogy toomodify hello szolgáltatási terület. Például **HDFS**. Hello center területen jelölje ki a hello **Configs** fülre.

    ![Ambari webes kijelölt HDFS Configs lap képe](./media/hdinsight-hadoop-heap-dump-linux/serviceconfig.png)

3. Hello segítségével **szűrő...**  bejegyzést, írjon be **jelentésküldési**. Csak a tartalmazó ezt a szöveget elemek jelennek meg.

    ![Szűrt lista](./media/hdinsight-hadoop-heap-dump-linux/filter.png)

4. Hello található  **\* \_OPTS** tooenable halommemória kiírása az szeretné, majd adja meg hello lehetőségeket hello szolgáltatás bejegyzése tooenable kívánja. A kép a következő hello, felvett `-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp/` toohello **HADOOP\_NAMENODE\_OPTS** bejegyzést:

    ![A - XX HADOOP_NAMENODE_OPTS: + HeapDumpOnOutOfMemoryError - XX: HeapDumpPath = / tmp /](./media/hdinsight-hadoop-heap-dump-linux/opts.png)

   > [!NOTE]
   > Ha engedélyezése halommemória memóriaképek hello a hozzárendelését, vagy csökkentse a gyermekfolyamat, keresni, nevű hello mezők **mapreduce.admin.map.child.java.opts** és **mapreduce.admin.reduce.child.java.opts**.

    Használjon hello **mentése** toosave hello módosítások gombra. Hello módosítások leíró rövid megjegyzés adhat meg.

5. Miután hello módosítások történtek, hello **újraindítás szükséges** ikon jelenik meg egy vagy több szolgáltatás mellett.

    ![Indítsa újra a szükséges ikonra, és indítsa újra a gomb](./media/hdinsight-hadoop-heap-dump-linux/restartrequiredicon.png)

6. Jelöljön ki minden egyes szolgáltatás újraindítását igénylő, és hello **szolgáltatás műveletek** túl gomb**kapcsolja be a karbantartási mód**. A karbantartási mód megakadályozza, hogy a riasztások azt újraindításakor hello szolgáltatás elő.

    ![Kapcsolja be a karbantartási mód menü](./media/hdinsight-hadoop-heap-dump-linux/maintenancemode.png)

7. Miután engedélyezte a karbantartási mód, használja a hello **indítsa újra a** hello szolgáltatás túl gomb**indítsa újra az összes végrehajtott**

    ![Indítsa újra az összes érintett bejegyzés](./media/hdinsight-hadoop-heap-dump-linux/restartbutton.png)

   > [!NOTE]
   > hello bejegyzéseket hello **indítsa újra a** gomb más szolgáltatásaihoz eltérő lehet.

8. Ha hello szolgáltatás újraindítása, a hello **szolgáltatás műveletek** túl gomb**kapcsolja ki a karbantartási mód**. Az Ambari tooresume riasztások hello szolgáltatás figyelését.

