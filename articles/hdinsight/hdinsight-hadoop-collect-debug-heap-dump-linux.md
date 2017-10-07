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
# <a name="enable-heap-dumps-for-hadoop-services-on-linux-based-hdinsight"></a><span data-ttu-id="6125a-103">Halommemória memóriaképek a Linux-alapú HDInsight Hadoop-szolgáltatások engedélyezése</span><span class="sxs-lookup"><span data-stu-id="6125a-103">Enable heap dumps for Hadoop services on Linux-based HDInsight</span></span>

[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

<span data-ttu-id="6125a-104">Halommemória memóriaképek tartalmazhat hello alkalmazás memória, beleértve a változók értékeinek hello hello időpontban hello memóriakép létrehozása egy pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="6125a-104">Heap dumps contain a snapshot of hello application's memory, including hello values of variables at hello time hello dump was created.</span></span> <span data-ttu-id="6125a-105">Ezért futás közben felmerülő problémák diagnosztizálásához.</span><span class="sxs-lookup"><span data-stu-id="6125a-105">So they are useful for diagnosing problems that occur at run-time.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6125a-106">hello ebben a dokumentumban csak a lépések Linux használó HDInsight-fürtökkel.</span><span class="sxs-lookup"><span data-stu-id="6125a-106">hello steps in this document only work with HDInsight clusters that use Linux.</span></span> <span data-ttu-id="6125a-107">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="6125a-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="6125a-108">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="6125a-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <span data-ttu-id="6125a-109"><a name="whichServices"></a>Szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="6125a-109"><a name="whichServices"></a>Services</span></span>

<span data-ttu-id="6125a-110">A következő szolgáltatások hello halommemória memóriaképek engedélyezéséhez:</span><span class="sxs-lookup"><span data-stu-id="6125a-110">You can enable heap dumps for hello following services:</span></span>

* <span data-ttu-id="6125a-111">**hcatalog** -tempelton</span><span class="sxs-lookup"><span data-stu-id="6125a-111">**hcatalog** - tempelton</span></span>
* <span data-ttu-id="6125a-112">**Hive** -hiveserver2-n, metaadattárhoz, derbyserver</span><span class="sxs-lookup"><span data-stu-id="6125a-112">**hive** - hiveserver2, metastore, derbyserver</span></span>
* <span data-ttu-id="6125a-113">**mapreduce** -jobhistoryserver</span><span class="sxs-lookup"><span data-stu-id="6125a-113">**mapreduce** - jobhistoryserver</span></span>
* <span data-ttu-id="6125a-114">**yarn** -resourcemanager, nodemanager, timelineserver</span><span class="sxs-lookup"><span data-stu-id="6125a-114">**yarn** - resourcemanager, nodemanager, timelineserver</span></span>
* <span data-ttu-id="6125a-115">**hdfs** -datanode, secondarynamenode, namenode</span><span class="sxs-lookup"><span data-stu-id="6125a-115">**hdfs** - datanode, secondarynamenode, namenode</span></span>

<span data-ttu-id="6125a-116">Is engedélyezheti a halommemória memóriaképek hello térkép és csökkentse a HDInsight által futtatott folyamatok.</span><span class="sxs-lookup"><span data-stu-id="6125a-116">You can also enable heap dumps for hello map and reduce processes ran by HDInsight.</span></span>

## <span data-ttu-id="6125a-117"><a name="configuration"></a>Understanding halommemória memóriakép konfiguráció</span><span class="sxs-lookup"><span data-stu-id="6125a-117"><a name="configuration"></a>Understanding heap dump configuration</span></span>

<span data-ttu-id="6125a-118">Úgy, hogy a beállítások engedélyezve vannak a halommemória memóriaképek (néven is ismert, azt, vagy a paraméterek) toohello JVM-et, a szolgáltatás indításakor.</span><span class="sxs-lookup"><span data-stu-id="6125a-118">Heap dumps are enabled by passing options (sometimes known as opts, or parameters) toohello JVM when a service is started.</span></span> <span data-ttu-id="6125a-119">A legtöbb Hadoop-szolgáltatásokra módosíthatja hello rendszerhéj használt parancsfájl toostart hello szolgáltatás toopass ezeket a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="6125a-119">For most Hadoop services, you can modify hello shell script used toostart hello service toopass these options.</span></span>

<span data-ttu-id="6125a-120">Minden parancsprogram esetén nincs az exportálás  **\* \_OPTS**, hello beállításokat tartalmazó átadott toohello JVM-et.</span><span class="sxs-lookup"><span data-stu-id="6125a-120">In each script, there is an export for **\*\_OPTS**, which contains hello options passed toohello JVM.</span></span> <span data-ttu-id="6125a-121">Például a hello **hadoop-env.sh** parancsprogramot, kezdődő hello sort `export HADOOP_NAMENODE_OPTS=` hello NameNode szolgáltatás hello beállításait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="6125a-121">For example, in hello **hadoop-env.sh** script, hello line that begins with `export HADOOP_NAMENODE_OPTS=` contains hello options for hello NameNode service.</span></span>

<span data-ttu-id="6125a-122">Rendelve, és csökkentse folyamatok kissé eltérő, mivel ezek a műveletek egy hello MapReduce szolgáltatás folyamat.</span><span class="sxs-lookup"><span data-stu-id="6125a-122">Map and reduce processes are slightly different, as these operations are a child process of hello MapReduce service.</span></span> <span data-ttu-id="6125a-123">Minden egyes hozzárendelését, vagy csökkentse folyamat fut egy gyermek tárolóban, és két hello JVM beállításokat tartalmazó bejegyzést is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="6125a-123">Each map or reduce process runs in a child container, and there are two entries that contain hello JVM options.</span></span> <span data-ttu-id="6125a-124">Mindkét szereplő **mapred-site.xml**:</span><span class="sxs-lookup"><span data-stu-id="6125a-124">Both contained in **mapred-site.xml**:</span></span>

* <span data-ttu-id="6125a-125">**mapreduce.Admin.Map.child.Java.opts**</span><span class="sxs-lookup"><span data-stu-id="6125a-125">**mapreduce.admin.map.child.java.opts**</span></span>
* <span data-ttu-id="6125a-126">**mapreduce.Admin.reduce.child.Java.opts**</span><span class="sxs-lookup"><span data-stu-id="6125a-126">**mapreduce.admin.reduce.child.java.opts**</span></span>

> [!NOTE]
> <span data-ttu-id="6125a-127">Javasoljuk az Ambari segítségével mindkét toomodify hello parancsfájlok és mapred-site.xml beállítások, mint Ambari kezelni replikálni a módosításokat hello fürt csomópontjai között.</span><span class="sxs-lookup"><span data-stu-id="6125a-127">We recommend using Ambari toomodify both hello scripts and mapred-site.xml settings, as Ambari handle replicating changes across nodes in hello cluster.</span></span> <span data-ttu-id="6125a-128">Lásd: hello [Ambari használatával](#using-ambari) szakasz lépéseit.</span><span class="sxs-lookup"><span data-stu-id="6125a-128">See hello [Using Ambari](#using-ambari) section for specific steps.</span></span>

### <a name="enable-heap-dumps"></a><span data-ttu-id="6125a-129">Halomürítések engedélyezése</span><span class="sxs-lookup"><span data-stu-id="6125a-129">Enable heap dumps</span></span>

<span data-ttu-id="6125a-130">hello következő beállítással halommemória memóriaképek egy OutOfMemoryError esetén:</span><span class="sxs-lookup"><span data-stu-id="6125a-130">hello following option enables heap dumps when an OutOfMemoryError occurs:</span></span>

    -XX:+HeapDumpOnOutOfMemoryError

<span data-ttu-id="6125a-131">Hello  **+**  azt jelzi, hogy ez a beállítás engedélyezve van-e.</span><span class="sxs-lookup"><span data-stu-id="6125a-131">hello **+** indicates that this option is enabled.</span></span> <span data-ttu-id="6125a-132">hello alapértelmezett le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="6125a-132">hello default is disabled.</span></span>

> [!WARNING]
> <span data-ttu-id="6125a-133">Halommemória memóriaképek nem engedélyezettek a HDInsight Hadoop-szolgáltatás alapértelmezés szerint, hello memóriaképek tekintélyes lehet.</span><span class="sxs-lookup"><span data-stu-id="6125a-133">Heap dumps are not enabled for Hadoop services on HDInsight by default, as hello dump files can be large.</span></span> <span data-ttu-id="6125a-134">Ha engedélyezi ezeket a hibaelhárításhoz, ne felejtse el azokat, amennyiben rendelkezik másolható hello problémát és az összegyűjtött hello memóriaképek toodisable.</span><span class="sxs-lookup"><span data-stu-id="6125a-134">If you do enable them for troubleshooting, remember toodisable them once you have reproduced hello problem and gathered hello dump files.</span></span>

### <a name="dump-location"></a><span data-ttu-id="6125a-135">Biztonsági másolat helye</span><span class="sxs-lookup"><span data-stu-id="6125a-135">Dump location</span></span>

<span data-ttu-id="6125a-136">hello alapértelmezett hello memóriakép helye hello aktuális munkakönyvtárban.</span><span class="sxs-lookup"><span data-stu-id="6125a-136">hello default location for hello dump file is hello current working directory.</span></span> <span data-ttu-id="6125a-137">Szabályozhatja, ahol hello fájlt tárolja a következő beállítás hello használata:</span><span class="sxs-lookup"><span data-stu-id="6125a-137">You can control where hello file is stored using hello following option:</span></span>

    -XX:HeapDumpPath=/path

<span data-ttu-id="6125a-138">Használata esetén például `-XX:HeapDumpPath=/tmp` hello memóriaképek toobe hello könyvtárban találhatók okozza.</span><span class="sxs-lookup"><span data-stu-id="6125a-138">For example, using `-XX:HeapDumpPath=/tmp` causes hello dumps toobe stored in hello /tmp directory.</span></span>

### <a name="scripts"></a><span data-ttu-id="6125a-139">Parancsprogramok</span><span class="sxs-lookup"><span data-stu-id="6125a-139">Scripts</span></span>

<span data-ttu-id="6125a-140">Egy parancsfájlt is el lehet indítani amikor egy **OutOfMemoryError** következik be.</span><span class="sxs-lookup"><span data-stu-id="6125a-140">You can also trigger a script when an **OutOfMemoryError** occurs.</span></span> <span data-ttu-id="6125a-141">Például egy értesítés időt. így megtudhatja, hogy hello hiba történt.</span><span class="sxs-lookup"><span data-stu-id="6125a-141">For example, triggering a notification so you know that hello error has occurred.</span></span> <span data-ttu-id="6125a-142">Használjon hello következő beállítást tootrigger parancsfájl egy __OutOfMemoryError__:</span><span class="sxs-lookup"><span data-stu-id="6125a-142">Use hello following option tootrigger a script on an __OutOfMemoryError__:</span></span>

    -XX:OnOutOfMemoryError=/path/to/script

> [!NOTE]
> <span data-ttu-id="6125a-143">Mivel a Hadoop elosztott rendszer, bármely használt parancsfájl hello hello szolgáltatás fut a fürt minden csomópontján kell elhelyezni.</span><span class="sxs-lookup"><span data-stu-id="6125a-143">Since Hadoop is a distributed system, any script used must be placed on all nodes in hello cluster that hello service runs on.</span></span>
> 
> <span data-ttu-id="6125a-144">hello parancsfájl kell is lehet, hogy elérhető-e hello fiók hello szolgáltatás fut, és biztosítania kell a helyre végrehajtási engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="6125a-144">hello script must also be in a location that is accessible by hello account hello service runs as, and must provide execute permissions.</span></span> <span data-ttu-id="6125a-145">Például előfordulhat, hogy kívánja toostore parancsfájlok `/usr/local/bin` és `chmod go+rx /usr/local/bin/filename.sh` toogrant olvasási és végrehajtási engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="6125a-145">For example, you may wish toostore scripts in `/usr/local/bin` and use `chmod go+rx /usr/local/bin/filename.sh` toogrant read and execute permissions.</span></span>

## <a name="using-ambari"></a><span data-ttu-id="6125a-146">Ambari használatával</span><span class="sxs-lookup"><span data-stu-id="6125a-146">Using Ambari</span></span>

<span data-ttu-id="6125a-147">egy szolgáltatás, a lépéseket követve használata hello toomodify hello konfigurációja:</span><span class="sxs-lookup"><span data-stu-id="6125a-147">toomodify hello configuration for a service, use hello following steps:</span></span>

1. <span data-ttu-id="6125a-148">Nyissa meg a fürt hello Ambari webes felhasználói Felületét.</span><span class="sxs-lookup"><span data-stu-id="6125a-148">Open hello Ambari web UI for your cluster.</span></span> <span data-ttu-id="6125a-149">hello URL-címe: https://YOURCLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="6125a-149">hello URL is https://YOURCLUSTERNAME.azurehdinsight.net.</span></span>

    <span data-ttu-id="6125a-150">Amikor a rendszer kéri, hitelesítéshez toohello hely hello HTTP-fiók nevének (alapértelmezett: admin) és a jelszót a fürt számára.</span><span class="sxs-lookup"><span data-stu-id="6125a-150">When prompted, authenticate toohello site using hello HTTP account name (default: admin) and password for your cluster.</span></span>

   > [!NOTE]
   > <span data-ttu-id="6125a-151">Kérheti másodszor által Ambari hello felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="6125a-151">You may be prompted a second time by Ambari for hello user name and password.</span></span> <span data-ttu-id="6125a-152">Ha igen, adja meg ugyanazt a fióknevet és jelszót hello</span><span class="sxs-lookup"><span data-stu-id="6125a-152">If so, enter hello same account name and password</span></span>

2. <span data-ttu-id="6125a-153">Hello listája hello bal oldali meg és jelölje ki azt szeretné, hogy toomodify hello szolgáltatási terület.</span><span class="sxs-lookup"><span data-stu-id="6125a-153">Using hello list of on hello left, select hello service area you want toomodify.</span></span> <span data-ttu-id="6125a-154">Például **HDFS**.</span><span class="sxs-lookup"><span data-stu-id="6125a-154">For example, **HDFS**.</span></span> <span data-ttu-id="6125a-155">Hello center területen jelölje ki a hello **Configs** fülre.</span><span class="sxs-lookup"><span data-stu-id="6125a-155">In hello center area, select hello **Configs** tab.</span></span>

    ![Ambari webes kijelölt HDFS Configs lap képe](./media/hdinsight-hadoop-heap-dump-linux/serviceconfig.png)

3. <span data-ttu-id="6125a-157">Hello segítségével **szűrő...**  bejegyzést, írjon be **jelentésküldési**.</span><span class="sxs-lookup"><span data-stu-id="6125a-157">Using hello **Filter...** entry, enter **opts**.</span></span> <span data-ttu-id="6125a-158">Csak a tartalmazó ezt a szöveget elemek jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="6125a-158">Only items containing this text are displayed.</span></span>

    ![Szűrt lista](./media/hdinsight-hadoop-heap-dump-linux/filter.png)

4. <span data-ttu-id="6125a-160">Hello található  **\* \_OPTS** tooenable halommemória kiírása az szeretné, majd adja meg hello lehetőségeket hello szolgáltatás bejegyzése tooenable kívánja.</span><span class="sxs-lookup"><span data-stu-id="6125a-160">Find hello **\*\_OPTS** entry for hello service you want tooenable heap dumps for, and add hello options you wish tooenable.</span></span> <span data-ttu-id="6125a-161">A kép a következő hello, felvett `-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp/` toohello **HADOOP\_NAMENODE\_OPTS** bejegyzést:</span><span class="sxs-lookup"><span data-stu-id="6125a-161">In hello following image, I've added `-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp/` toohello **HADOOP\_NAMENODE\_OPTS** entry:</span></span>

    ![A - XX HADOOP_NAMENODE_OPTS: + HeapDumpOnOutOfMemoryError - XX: HeapDumpPath = / tmp /](./media/hdinsight-hadoop-heap-dump-linux/opts.png)

   > [!NOTE]
   > <span data-ttu-id="6125a-163">Ha engedélyezése halommemória memóriaképek hello a hozzárendelését, vagy csökkentse a gyermekfolyamat, keresni, nevű hello mezők **mapreduce.admin.map.child.java.opts** és **mapreduce.admin.reduce.child.java.opts**.</span><span class="sxs-lookup"><span data-stu-id="6125a-163">When enabling heap dumps for hello map or reduce child process, look for hello fields named **mapreduce.admin.map.child.java.opts** and **mapreduce.admin.reduce.child.java.opts**.</span></span>

    <span data-ttu-id="6125a-164">Használjon hello **mentése** toosave hello módosítások gombra.</span><span class="sxs-lookup"><span data-stu-id="6125a-164">Use hello **Save** button toosave hello changes.</span></span> <span data-ttu-id="6125a-165">Hello módosítások leíró rövid megjegyzés adhat meg.</span><span class="sxs-lookup"><span data-stu-id="6125a-165">You can enter a short note describing hello changes.</span></span>

5. <span data-ttu-id="6125a-166">Miután hello módosítások történtek, hello **újraindítás szükséges** ikon jelenik meg egy vagy több szolgáltatás mellett.</span><span class="sxs-lookup"><span data-stu-id="6125a-166">Once hello changes have been applied, hello **Restart required** icon appears beside one or more services.</span></span>

    ![Indítsa újra a szükséges ikonra, és indítsa újra a gomb](./media/hdinsight-hadoop-heap-dump-linux/restartrequiredicon.png)

6. <span data-ttu-id="6125a-168">Jelöljön ki minden egyes szolgáltatás újraindítását igénylő, és hello **szolgáltatás műveletek** túl gomb**kapcsolja be a karbantartási mód**.</span><span class="sxs-lookup"><span data-stu-id="6125a-168">Select each service that needs a restart, and use hello **Service Actions** button too**Turn On Maintenance Mode**.</span></span> <span data-ttu-id="6125a-169">A karbantartási mód megakadályozza, hogy a riasztások azt újraindításakor hello szolgáltatás elő.</span><span class="sxs-lookup"><span data-stu-id="6125a-169">Maintenance mode prevents alerts from being generated from hello service when you restart it.</span></span>

    ![Kapcsolja be a karbantartási mód menü](./media/hdinsight-hadoop-heap-dump-linux/maintenancemode.png)

7. <span data-ttu-id="6125a-171">Miután engedélyezte a karbantartási mód, használja a hello **indítsa újra a** hello szolgáltatás túl gomb**indítsa újra az összes végrehajtott**</span><span class="sxs-lookup"><span data-stu-id="6125a-171">Once you have enabled maintenance mode, use hello **Restart** button for hello service too**Restart All Effected**</span></span>

    ![Indítsa újra az összes érintett bejegyzés](./media/hdinsight-hadoop-heap-dump-linux/restartbutton.png)

   > [!NOTE]
   > <span data-ttu-id="6125a-173">hello bejegyzéseket hello **indítsa újra a** gomb más szolgáltatásaihoz eltérő lehet.</span><span class="sxs-lookup"><span data-stu-id="6125a-173">hello entries for hello **Restart** button may be different for other services.</span></span>

8. <span data-ttu-id="6125a-174">Ha hello szolgáltatás újraindítása, a hello **szolgáltatás műveletek** túl gomb**kapcsolja ki a karbantartási mód**.</span><span class="sxs-lookup"><span data-stu-id="6125a-174">Once hello services have been restarted, use hello **Service Actions** button too**Turn Off Maintenance Mode**.</span></span> <span data-ttu-id="6125a-175">Az Ambari tooresume riasztások hello szolgáltatás figyelését.</span><span class="sxs-lookup"><span data-stu-id="6125a-175">This Ambari tooresume monitoring for alerts for hello service.</span></span>

