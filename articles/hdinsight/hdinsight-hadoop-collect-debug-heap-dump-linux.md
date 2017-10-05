---
title: "A Hadoop-szolgáltatás a HDInsight - Azure halommemória memóriaképek engedélyezése |} Microsoft Docs"
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
ms.openlocfilehash: 59942e989d622c2486edf181d76e13344c71e6f9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="enable-heap-dumps-for-hadoop-services-on-linux-based-hdinsight"></a><span data-ttu-id="2b4a6-103">Halommemória memóriaképek a Linux-alapú HDInsight Hadoop-szolgáltatások engedélyezése</span><span class="sxs-lookup"><span data-stu-id="2b4a6-103">Enable heap dumps for Hadoop services on Linux-based HDInsight</span></span>

[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

<span data-ttu-id="2b4a6-104">Halommemória memóriaképek tartalmazza az alkalmazás memória, beleértve a változók értékeit a biztonsági másolat létrehozásakor pillanatképet.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-104">Heap dumps contain a snapshot of the application's memory, including the values of variables at the time the dump was created.</span></span> <span data-ttu-id="2b4a6-105">Ezért futás közben felmerülő problémák diagnosztizálásához.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-105">So they are useful for diagnosing problems that occur at run-time.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2b4a6-106">A jelen dokumentumban leírt lépések csak a HDInsight-fürtök Linux használó dolgozhat.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-106">The steps in this document only work with HDInsight clusters that use Linux.</span></span> <span data-ttu-id="2b4a6-107">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-107">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="2b4a6-108">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="2b4a6-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <span data-ttu-id="2b4a6-109"><a name="whichServices"></a>Szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="2b4a6-109"><a name="whichServices"></a>Services</span></span>

<span data-ttu-id="2b4a6-110">A következő szolgáltatások halommemória memóriaképek engedélyezéséhez:</span><span class="sxs-lookup"><span data-stu-id="2b4a6-110">You can enable heap dumps for the following services:</span></span>

* <span data-ttu-id="2b4a6-111">**hcatalog** -tempelton</span><span class="sxs-lookup"><span data-stu-id="2b4a6-111">**hcatalog** - tempelton</span></span>
* <span data-ttu-id="2b4a6-112">**Hive** -hiveserver2-n, metaadattárhoz, derbyserver</span><span class="sxs-lookup"><span data-stu-id="2b4a6-112">**hive** - hiveserver2, metastore, derbyserver</span></span>
* <span data-ttu-id="2b4a6-113">**mapreduce** -jobhistoryserver</span><span class="sxs-lookup"><span data-stu-id="2b4a6-113">**mapreduce** - jobhistoryserver</span></span>
* <span data-ttu-id="2b4a6-114">**yarn** -resourcemanager, nodemanager, timelineserver</span><span class="sxs-lookup"><span data-stu-id="2b4a6-114">**yarn** - resourcemanager, nodemanager, timelineserver</span></span>
* <span data-ttu-id="2b4a6-115">**hdfs** -datanode, secondarynamenode, namenode</span><span class="sxs-lookup"><span data-stu-id="2b4a6-115">**hdfs** - datanode, secondarynamenode, namenode</span></span>

<span data-ttu-id="2b4a6-116">Is engedélyezheti a térkép halommemória memóriaképek és csökkentse a HDInsight által futtatott folyamatok.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-116">You can also enable heap dumps for the map and reduce processes ran by HDInsight.</span></span>

## <span data-ttu-id="2b4a6-117"><a name="configuration"></a>Understanding halommemória memóriakép konfiguráció</span><span class="sxs-lookup"><span data-stu-id="2b4a6-117"><a name="configuration"></a>Understanding heap dump configuration</span></span>

<span data-ttu-id="2b4a6-118">Úgy, hogy a beállítások engedélyezve vannak a halommemória memóriaképek (néven is ismert, azt, vagy paraméterek) számára a JVM-et a szolgáltatás indításakor.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-118">Heap dumps are enabled by passing options (sometimes known as opts, or parameters) to the JVM when a service is started.</span></span> <span data-ttu-id="2b4a6-119">A legtöbb Hadoop-szolgáltatásokra módosíthatja a héjparancsfájlt át ezeket a beállításokat a szolgáltatás elindításához használja.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-119">For most Hadoop services, you can modify the shell script used to start the service to pass these options.</span></span>

<span data-ttu-id="2b4a6-120">Minden parancsprogram esetén nincs az exportálás  **\* \_OPTS**, amely tartalmazza a JVM-et átadott beállítást.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-120">In each script, there is an export for **\*\_OPTS**, which contains the options passed to the JVM.</span></span> <span data-ttu-id="2b4a6-121">Például a **hadoop-env.sh** parancsfájl, a sor kezdődő `export HADOOP_NAMENODE_OPTS=` a NameNode szolgáltatás beállításait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-121">For example, in the **hadoop-env.sh** script, the line that begins with `export HADOOP_NAMENODE_OPTS=` contains the options for the NameNode service.</span></span>

<span data-ttu-id="2b4a6-122">Rendelve, és csökkentse folyamatok kissé eltérő, mert ezek a műveletek a MapReduce szolgáltatás egyik gyermekfolyamata.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-122">Map and reduce processes are slightly different, as these operations are a child process of the MapReduce service.</span></span> <span data-ttu-id="2b4a6-123">Minden egyes hozzárendelését, vagy csökkentse folyamat egy gyermek tárolóban fut, és a JVM beállításokat tartalmazó két bejegyzést is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-123">Each map or reduce process runs in a child container, and there are two entries that contain the JVM options.</span></span> <span data-ttu-id="2b4a6-124">Mindkét szereplő **mapred-site.xml**:</span><span class="sxs-lookup"><span data-stu-id="2b4a6-124">Both contained in **mapred-site.xml**:</span></span>

* <span data-ttu-id="2b4a6-125">**mapreduce.Admin.Map.child.Java.opts**</span><span class="sxs-lookup"><span data-stu-id="2b4a6-125">**mapreduce.admin.map.child.java.opts**</span></span>
* <span data-ttu-id="2b4a6-126">**mapreduce.Admin.reduce.child.Java.opts**</span><span class="sxs-lookup"><span data-stu-id="2b4a6-126">**mapreduce.admin.reduce.child.java.opts**</span></span>

> [!NOTE]
> <span data-ttu-id="2b4a6-127">Az Ambari leíró replikálni a módosításokat a fürt csomópontjai között, a parancsfájlok és a mapred-site.xml beállítások módosítása Ambari használatát javasoljuk.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-127">We recommend using Ambari to modify both the scripts and mapred-site.xml settings, as Ambari handle replicating changes across nodes in the cluster.</span></span> <span data-ttu-id="2b4a6-128">Tekintse meg a [Ambari használatával](#using-ambari) szakasz lépéseit.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-128">See the [Using Ambari](#using-ambari) section for specific steps.</span></span>

### <a name="enable-heap-dumps"></a><span data-ttu-id="2b4a6-129">Halomürítések engedélyezése</span><span class="sxs-lookup"><span data-stu-id="2b4a6-129">Enable heap dumps</span></span>

<span data-ttu-id="2b4a6-130">A következő beállítás lehetővé teszi, hogy halommemória memóriaképek egy OutOfMemoryError esetén:</span><span class="sxs-lookup"><span data-stu-id="2b4a6-130">The following option enables heap dumps when an OutOfMemoryError occurs:</span></span>

    -XX:+HeapDumpOnOutOfMemoryError

<span data-ttu-id="2b4a6-131">A  **+**  azt jelzi, hogy ez a beállítás engedélyezve van-e.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-131">The **+** indicates that this option is enabled.</span></span> <span data-ttu-id="2b4a6-132">Alapértelmezés szerint le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-132">The default is disabled.</span></span>

> [!WARNING]
> <span data-ttu-id="2b4a6-133">Halommemória memóriaképek nem engedélyezettek a HDInsight Hadoop-szolgáltatás alapértelmezés szerint, lehet, hogy nagy a memóriakép fájlokhoz.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-133">Heap dumps are not enabled for Hadoop services on HDInsight by default, as the dump files can be large.</span></span> <span data-ttu-id="2b4a6-134">Ha engedélyezi ezeket a hibaelhárításhoz, ne felejtse el őket tiltani, miután a probléma másolható és a memóriaképek összegyűjtött.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-134">If you do enable them for troubleshooting, remember to disable them once you have reproduced the problem and gathered the dump files.</span></span>

### <a name="dump-location"></a><span data-ttu-id="2b4a6-135">Biztonsági másolat helye</span><span class="sxs-lookup"><span data-stu-id="2b4a6-135">Dump location</span></span>

<span data-ttu-id="2b4a6-136">A biztonsági másolat fájl alapértelmezett helye az aktuális munkakönyvtárban.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-136">The default location for the dump file is the current working directory.</span></span> <span data-ttu-id="2b4a6-137">Szabályozhatja, ha a fájl található a következő beállítás használatával:</span><span class="sxs-lookup"><span data-stu-id="2b4a6-137">You can control where the file is stored using the following option:</span></span>

    -XX:HeapDumpPath=/path

<span data-ttu-id="2b4a6-138">Használata esetén például `-XX:HeapDumpPath=/tmp` hatására a memóriaképek könyvtárban kell tárolni.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-138">For example, using `-XX:HeapDumpPath=/tmp` causes the dumps to be stored in the /tmp directory.</span></span>

### <a name="scripts"></a><span data-ttu-id="2b4a6-139">Parancsprogramok</span><span class="sxs-lookup"><span data-stu-id="2b4a6-139">Scripts</span></span>

<span data-ttu-id="2b4a6-140">Egy parancsfájlt is el lehet indítani amikor egy **OutOfMemoryError** következik be.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-140">You can also trigger a script when an **OutOfMemoryError** occurs.</span></span> <span data-ttu-id="2b4a6-141">Például váltanak ki egy értesítést, így megtudhatja, hogy a hiba.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-141">For example, triggering a notification so you know that the error has occurred.</span></span> <span data-ttu-id="2b4a6-142">A következő kapcsoló használatával indul el, a parancsfájl egy __OutOfMemoryError__:</span><span class="sxs-lookup"><span data-stu-id="2b4a6-142">Use the following option to trigger a script on an __OutOfMemoryError__:</span></span>

    -XX:OnOutOfMemoryError=/path/to/script

> [!NOTE]
> <span data-ttu-id="2b4a6-143">Mivel a Hadoop elosztott rendszer, bármely használt parancsfájl kell elhelyezni, amely a szolgáltatás fut a fürt összes csomópontján.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-143">Since Hadoop is a distributed system, any script used must be placed on all nodes in the cluster that the service runs on.</span></span>
> 
> <span data-ttu-id="2b4a6-144">A parancsfájl kell is lehet, amely elérhető a fiók a szolgáltatás fut, és biztosítania kell a helyre végrehajtási engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-144">The script must also be in a location that is accessible by the account the service runs as, and must provide execute permissions.</span></span> <span data-ttu-id="2b4a6-145">Például előfordulhat, hogy a parancsprogramok tárolásához kívánja `/usr/local/bin` és `chmod go+rx /usr/local/bin/filename.sh` adjon olvasási és végrehajtási engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-145">For example, you may wish to store scripts in `/usr/local/bin` and use `chmod go+rx /usr/local/bin/filename.sh` to grant read and execute permissions.</span></span>

## <a name="using-ambari"></a><span data-ttu-id="2b4a6-146">Ambari használatával</span><span class="sxs-lookup"><span data-stu-id="2b4a6-146">Using Ambari</span></span>

<span data-ttu-id="2b4a6-147">A szolgáltatás konfigurációjának módosítása, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="2b4a6-147">To modify the configuration for a service, use the following steps:</span></span>

1. <span data-ttu-id="2b4a6-148">Nyissa meg a fürt Ambari webes felhasználói Felületét.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-148">Open the Ambari web UI for your cluster.</span></span> <span data-ttu-id="2b4a6-149">Az URL-cím https://YOURCLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-149">The URL is https://YOURCLUSTERNAME.azurehdinsight.net.</span></span>

    <span data-ttu-id="2b4a6-150">Amikor a rendszer kéri, a helyhez, a HTTP-fiók nevének hitelesíteni (alapértelmezett: admin) és a jelszót a fürt számára.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-150">When prompted, authenticate to the site using the HTTP account name (default: admin) and password for your cluster.</span></span>

   > [!NOTE]
   > <span data-ttu-id="2b4a6-151">Kérheti másodszor Ambari által a felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-151">You may be prompted a second time by Ambari for the user name and password.</span></span> <span data-ttu-id="2b4a6-152">Ha igen, adja meg az azonos fióknevet és jelszót</span><span class="sxs-lookup"><span data-stu-id="2b4a6-152">If so, enter the same account name and password</span></span>

2. <span data-ttu-id="2b4a6-153">A lista a bal oldali meg és jelölje ki a módosítani kívánt szolgáltatási terület.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-153">Using the list of on the left, select the service area you want to modify.</span></span> <span data-ttu-id="2b4a6-154">Például **HDFS**.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-154">For example, **HDFS**.</span></span> <span data-ttu-id="2b4a6-155">A központ területen válassza ki a **Configs** fülre.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-155">In the center area, select the **Configs** tab.</span></span>

    ![Ambari webes kijelölt HDFS Configs lap képe](./media/hdinsight-hadoop-heap-dump-linux/serviceconfig.png)

3. <span data-ttu-id="2b4a6-157">Használja a **szűrő...**  bejegyzést, írjon be **jelentésküldési**.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-157">Using the **Filter...** entry, enter **opts**.</span></span> <span data-ttu-id="2b4a6-158">Csak a tartalmazó ezt a szöveget elemek jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-158">Only items containing this text are displayed.</span></span>

    ![Szűrt lista](./media/hdinsight-hadoop-heap-dump-linux/filter.png)

4. <span data-ttu-id="2b4a6-160">Keresés a  **\* \_OPTS** szolgáltatás bejegyzése szeretné a halommemória memóriaképek engedélyezése, majd adja meg az engedélyezni kívánt beállításokat.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-160">Find the **\*\_OPTS** entry for the service you want to enable heap dumps for, and add the options you wish to enable.</span></span> <span data-ttu-id="2b4a6-161">Az alábbi képen felvett `-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp/` számára a **HADOOP\_NAMENODE\_OPTS** bejegyzést:</span><span class="sxs-lookup"><span data-stu-id="2b4a6-161">In the following image, I've added `-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp/` to the **HADOOP\_NAMENODE\_OPTS** entry:</span></span>

    ![A - XX HADOOP_NAMENODE_OPTS: + HeapDumpOnOutOfMemoryError - XX: HeapDumpPath = / tmp /](./media/hdinsight-hadoop-heap-dump-linux/opts.png)

   > [!NOTE]
   > <span data-ttu-id="2b4a6-163">Ha halommemória engedélyezése a térkép listázása, vagy csökkentse gyermekfolyamat, keresse meg a mezők nevű **mapreduce.admin.map.child.java.opts** és **mapreduce.admin.reduce.child.java.opts**.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-163">When enabling heap dumps for the map or reduce child process, look for the fields named **mapreduce.admin.map.child.java.opts** and **mapreduce.admin.reduce.child.java.opts**.</span></span>

    <span data-ttu-id="2b4a6-164">Használja a **mentése** gombra a módosítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-164">Use the **Save** button to save the changes.</span></span> <span data-ttu-id="2b4a6-165">A módosítások leíró rövid megjegyzés adhat meg.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-165">You can enter a short note describing the changes.</span></span>

5. <span data-ttu-id="2b4a6-166">A módosítások léptek érvénybe, ha a **újraindítás szükséges** ikon jelenik meg egy vagy több szolgáltatás mellett.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-166">Once the changes have been applied, the **Restart required** icon appears beside one or more services.</span></span>

    ![Indítsa újra a szükséges ikonra, és indítsa újra a gomb](./media/hdinsight-hadoop-heap-dump-linux/restartrequiredicon.png)

6. <span data-ttu-id="2b4a6-168">Válassza ki a számítógép újraindítását igénylő minden szolgáltatást, és használja a **szolgáltatás műveletek** gombra kattint, hogy **kapcsolja be a karbantartási mód**.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-168">Select each service that needs a restart, and use the **Service Actions** button to **Turn On Maintenance Mode**.</span></span> <span data-ttu-id="2b4a6-169">Karbantartási mód megakadályozza, hogy a riasztások generálása a szolgáltatásból, ha indítja újra.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-169">Maintenance mode prevents alerts from being generated from the service when you restart it.</span></span>

    ![Kapcsolja be a karbantartási mód menü](./media/hdinsight-hadoop-heap-dump-linux/maintenancemode.png)

7. <span data-ttu-id="2b4a6-171">Miután engedélyezte a karbantartási mód, használja a **indítsa újra a** gombra a szolgáltatás számára **indítsa újra az összes végrehajtott**</span><span class="sxs-lookup"><span data-stu-id="2b4a6-171">Once you have enabled maintenance mode, use the **Restart** button for the service to **Restart All Effected**</span></span>

    ![Indítsa újra az összes érintett bejegyzés](./media/hdinsight-hadoop-heap-dump-linux/restartbutton.png)

   > [!NOTE]
   > <span data-ttu-id="2b4a6-173">a bejegyzéseket a **indítsa újra a** gomb más szolgáltatásaihoz eltérő lehet.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-173">the entries for the **Restart** button may be different for other services.</span></span>

8. <span data-ttu-id="2b4a6-174">A szolgáltatások újraindítása, ha a **szolgáltatás műveletek** gombra kattint, hogy **kapcsolja ki a karbantartási mód**.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-174">Once the services have been restarted, use the **Service Actions** button to **Turn Off Maintenance Mode**.</span></span> <span data-ttu-id="2b4a6-175">Az Ambari riasztások a szolgáltatás figyelésének folytatása.</span><span class="sxs-lookup"><span data-stu-id="2b4a6-175">This Ambari to resume monitoring for alerts for the service.</span></span>

