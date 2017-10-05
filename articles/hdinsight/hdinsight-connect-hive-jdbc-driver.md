---
title: "Keresztül a JDBC-illesztőt - Azure HDInsight Hive lekérdezés |} Microsoft Docs"
description: "A Java-alkalmazás JDBC-illesztőt segítségével elküldeni a hdinsight Hadoop Hive-lekérdezéseket. Csatlakoztassa a programozott módon, és az SQuirrel SQL ügyfélről."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 928f8d2a-684d-48cb-894c-11c59a5599ae
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/14/2017
ms.author: larryfr
ms.openlocfilehash: f2de58c2763b2ddd0926336164738929c477c4ae
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="query-hive-through-the-jdbc-driver-in-hdinsight"></a><span data-ttu-id="a2e10-104">Lekérdezés Hive hdinsight JDBC-illesztőt keresztül</span><span class="sxs-lookup"><span data-stu-id="a2e10-104">Query Hive through the JDBC driver in HDInsight</span></span>

[!INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

<span data-ttu-id="a2e10-105">Útmutató a Java-alkalmazás JDBC-illesztőt küldeni az Azure HDInsight Hadoop Hive-lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="a2e10-105">Learn how to use the JDBC driver from a Java application to submit Hive queries to Hadoop in Azure HDInsight.</span></span> <span data-ttu-id="a2e10-106">A jelen dokumentumban szereplő információk programozott módon, és az SQuirrel SQL ügyfél mutatja be.</span><span class="sxs-lookup"><span data-stu-id="a2e10-106">The information in this document demonstrates how to connect programmatically and from the SQuirrel SQL client.</span></span>

<span data-ttu-id="a2e10-107">A Hive JDBC kapcsolaton további információkért lásd: [HiveJDBCInterface](https://cwiki.apache.org/confluence/display/Hive/HiveJDBCInterface).</span><span class="sxs-lookup"><span data-stu-id="a2e10-107">For more information on the Hive JDBC Interface, see [HiveJDBCInterface](https://cwiki.apache.org/confluence/display/Hive/HiveJDBCInterface).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a2e10-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a2e10-108">Prerequisites</span></span>

* <span data-ttu-id="a2e10-109">A Hadoop on HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="a2e10-109">A Hadoop on HDInsight cluster.</span></span> <span data-ttu-id="a2e10-110">Linux-alapú vagy a Windows-alapú fürtök működik.</span><span class="sxs-lookup"><span data-stu-id="a2e10-110">Either Linux-based or Windows-based clusters work.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="a2e10-111">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="a2e10-111">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="a2e10-112">További információkért lásd: [HDInsight 3.3 használatból való kivonást](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="a2e10-112">For more information, see [HDInsight 3.3 retirement](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="a2e10-113">[SQuirreL SQL](http://squirrel-sql.sourceforge.net/).</span><span class="sxs-lookup"><span data-stu-id="a2e10-113">[SQuirreL SQL](http://squirrel-sql.sourceforge.net/).</span></span> <span data-ttu-id="a2e10-114">SQuirreL JDBC ügyfélalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a2e10-114">SQuirreL is a JDBC client application.</span></span>

* <span data-ttu-id="a2e10-115">A [Java fejlesztői készlet (JDK) 7-es verzió](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) vagy újabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="a2e10-115">The [Java Developer Kit (JDK) version 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) or higher.</span></span>

* <span data-ttu-id="a2e10-116">[Apache Maven](https://maven.apache.org).</span><span class="sxs-lookup"><span data-stu-id="a2e10-116">[Apache Maven](https://maven.apache.org).</span></span> <span data-ttu-id="a2e10-117">Maven, a projekt felépítéséhez rendszer Java-projektek esetében ez a cikk társított projekt által használt.</span><span class="sxs-lookup"><span data-stu-id="a2e10-117">Maven is a project build system for Java projects that is used by the project associated with this article.</span></span>

## <a name="jdbc-connection-string"></a><span data-ttu-id="a2e10-118">JDBC-kapcsolati karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a2e10-118">JDBC connection string</span></span>

<span data-ttu-id="a2e10-119">Az Azure HDInsight-fürtök JDBC kapcsolatok válnak, több mint a 443-as, és a forgalom SSL használatával lett biztonságossá.</span><span class="sxs-lookup"><span data-stu-id="a2e10-119">JDBC connections to an HDInsight cluster on Azure are made over 443, and the traffic is secured using SSL.</span></span> <span data-ttu-id="a2e10-120">A nyilvános átjáró, amely a fürtök elhelyezkedik mögött átirányítja a forgalmat a portot, amelyet a hiveserver2-n ténylegesen figyel a következőn:.</span><span class="sxs-lookup"><span data-stu-id="a2e10-120">The public gateway that the clusters sit behind redirects the traffic to the port that HiveServer2 is actually listening on.</span></span> <span data-ttu-id="a2e10-121">A következő egy példa kapcsolati karakterláncra:</span><span class="sxs-lookup"><span data-stu-id="a2e10-121">The following is an example connection string:</span></span>

    jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2

<span data-ttu-id="a2e10-122">Cserélje le a `CLUSTERNAME` kifejezést a HDInsight-fürt nevére.</span><span class="sxs-lookup"><span data-stu-id="a2e10-122">Replace `CLUSTERNAME` with the name of your HDInsight cluster.</span></span>

## <a name="authentication"></a><span data-ttu-id="a2e10-123">Authentication</span><span class="sxs-lookup"><span data-stu-id="a2e10-123">Authentication</span></span>

<span data-ttu-id="a2e10-124">A kapcsolat létrehozásakor, a HDInsight fürt admin nevét és jelszavát, hogy a fürt átjáró hitelesítést kell használnia.</span><span class="sxs-lookup"><span data-stu-id="a2e10-124">When establishing the connection, you must use the HDInsight cluster admin name and password to authenticate to the cluster gateway.</span></span> <span data-ttu-id="a2e10-125">JDBC-ügyfelek például SQuirreL SQL csatlakozáshoz meg kell adni a rendszergazda nevét és jelszavát ügyfélbeállításokat.</span><span class="sxs-lookup"><span data-stu-id="a2e10-125">When connecting from JDBC clients such as SQuirreL SQL, you must enter the admin name and password in client settings.</span></span>

<span data-ttu-id="a2e10-126">Java-alkalmazás kell használnia a nevet és jelszót egy kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="a2e10-126">From a Java application, you must use the name and password when establishing a connection.</span></span> <span data-ttu-id="a2e10-127">Például a következő Java-kódot megnyílik egy új kapcsolatot a kapcsolati karakterláncot, a felügyeleti neve és a jelszót:</span><span class="sxs-lookup"><span data-stu-id="a2e10-127">For example, the following Java code opens a new connection using the connection string, admin name, and password:</span></span>

```java
DriverManager.getConnection(connectionString,clusterAdmin,clusterPassword);
```

## <a name="connect-with-squirrel-sql-client"></a><span data-ttu-id="a2e10-128">Csatlakozás SQuirreL SQL-ügyféllel</span><span class="sxs-lookup"><span data-stu-id="a2e10-128">Connect with SQuirreL SQL client</span></span>

<span data-ttu-id="a2e10-129">SQuirreL SQL JDBC-ügyfél, amely segítségével távolról ugyanúgy futtathatják a Hive-lekérdezéseket a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="a2e10-129">SQuirreL SQL is a JDBC client that can be used to remotely run Hive queries with your HDInsight cluster.</span></span> <span data-ttu-id="a2e10-130">A következő lépések azt feltételezik, hogy SQuirreL SQL már telepítve van.</span><span class="sxs-lookup"><span data-stu-id="a2e10-130">The following steps assume that you have already installed SQuirreL SQL.</span></span>

1. <span data-ttu-id="a2e10-131">Másolja a Hive JDBC-illesztőprogramok a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="a2e10-131">Copy the Hive JDBC drivers from your HDInsight cluster.</span></span>

    * <span data-ttu-id="a2e10-132">A **Linux-alapú HDInsight**, tegye a következőket a szükséges jar-fájlok letöltésére.</span><span class="sxs-lookup"><span data-stu-id="a2e10-132">For **Linux-based HDInsight**, use the following steps to download the required jar files.</span></span>

        1. <span data-ttu-id="a2e10-133">Hozzon létre egy könyvtárat, a tároló.</span><span class="sxs-lookup"><span data-stu-id="a2e10-133">Create a directory that contains the files.</span></span> <span data-ttu-id="a2e10-134">Például: `mkdir hivedriver`.</span><span class="sxs-lookup"><span data-stu-id="a2e10-134">For example, `mkdir hivedriver`.</span></span>

        2. <span data-ttu-id="a2e10-135">A parancssorból használja a fájlok másolását a HDInsight-fürthöz a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="a2e10-135">From a command line, use the following commands to copy the files from the HDInsight cluster:</span></span>

            ```bash
            scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/hive-jdbc*standalone.jar .
            scp USERNAME@CLUSTERNAME:/usr/hdp/current/hadoop-client/hadoop-common.jar .
            scp USERNAME@CLUSTERNAME:/usr/hdp/current/hadoop-client/hadoop-auth.jar .
            ```

            <span data-ttu-id="a2e10-136">Cserélje le `USERNAME` az SSH felhasználói fiók nevét a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="a2e10-136">Replace `USERNAME` with the SSH user account name for the cluster.</span></span> <span data-ttu-id="a2e10-137">Cserélje le `CLUSTERNAME` a HDInsight-fürt nevéhez.</span><span class="sxs-lookup"><span data-stu-id="a2e10-137">Replace `CLUSTERNAME` with the HDInsight cluster name.</span></span>

    * <span data-ttu-id="a2e10-138">A **Windows-alapú HDInsight**, tegye a következőket a jar-fájlok letöltésére.</span><span class="sxs-lookup"><span data-stu-id="a2e10-138">For **Windows-based HDInsight**, use the following steps to download the jar files.</span></span>

        1. <span data-ttu-id="a2e10-139">Azure-portálról, jelölje be a HDInsight-fürthöz, és válassza a **távoli asztal** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a2e10-139">From the Azure portal, select your HDInsight cluster, and then select the **Remote Desktop** icon.</span></span>

            ![Távoli asztali ikon](./media/hdinsight-connect-hive-jdbc-driver/remotedesktopicon.png)

        2. <span data-ttu-id="a2e10-141">A távoli asztalt panelt kell használni a **Connect** gombra kattintva csatlakozzon a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="a2e10-141">On the Remote Desktop blade, use the **Connect** button to connect to the cluster.</span></span> <span data-ttu-id="a2e10-142">A távoli asztal nincs engedélyezve, ha ezen a képernyőn adjon meg egy felhasználónevet és jelszót, majd válasszon **engedélyezése** távoli asztal engedélyezése a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="a2e10-142">If the Remote Desktop is not enabled, use the form to provide a user name and password, then select **Enable** to enable Remote Desktop for the cluster.</span></span>

            ![Távoli asztali panel](./media/hdinsight-connect-hive-jdbc-driver/remotedesktopblade.png)

            <span data-ttu-id="a2e10-144">Miután kiválasztott **Connect**, egy .rdp le.</span><span class="sxs-lookup"><span data-stu-id="a2e10-144">After selecting **Connect**, a .rdp file is downloaded.</span></span> <span data-ttu-id="a2e10-145">Ez a fájl segítségével indítsa el a távoli asztali ügyfél.</span><span class="sxs-lookup"><span data-stu-id="a2e10-145">Use this file to launch the Remote Desktop client.</span></span> <span data-ttu-id="a2e10-146">Amikor a rendszer kéri, a felhasználónév és a távoli asztal eléréséhez megadott jelszó használata.</span><span class="sxs-lookup"><span data-stu-id="a2e10-146">When prompted, use the user name and password you entered for Remote Desktop access.</span></span>

        3. <span data-ttu-id="a2e10-147">A csatlakozás után másolja a következő fájlokat a távoli asztali munkamenetből a helyi számítógépre.</span><span class="sxs-lookup"><span data-stu-id="a2e10-147">Once connected, copy the following files from the Remote Desktop session to your local machine.</span></span> <span data-ttu-id="a2e10-148">Nevű helyi könyvtárba helyezze őket `hivedriver`.</span><span class="sxs-lookup"><span data-stu-id="a2e10-148">Put them in a local directory named `hivedriver`.</span></span>

            * <span data-ttu-id="a2e10-149">C:\apps\dist\hive-0.14.0.2.2.9.1-7\lib\hive-JDBC-0.14.0.2.2.9.1-7-Standalone.JAR</span><span class="sxs-lookup"><span data-stu-id="a2e10-149">C:\apps\dist\hive-0.14.0.2.2.9.1-7\lib\hive-jdbc-0.14.0.2.2.9.1-7-standalone.jar</span></span>
            * <span data-ttu-id="a2e10-150">C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\hadoop-Common-2.6.0.2.2.9.1-7.JAR</span><span class="sxs-lookup"><span data-stu-id="a2e10-150">C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\hadoop-common-2.6.0.2.2.9.1-7.jar</span></span>
            * <span data-ttu-id="a2e10-151">C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\lib\hadoop-auth-2.6.0.2.2.9.1-7.JAR</span><span class="sxs-lookup"><span data-stu-id="a2e10-151">C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\lib\hadoop-auth-2.6.0.2.2.9.1-7.jar</span></span>

            > [!NOTE]
            > <span data-ttu-id="a2e10-152">Lehet, hogy a fürt másik a verziószámok szerepel az elérési útját és fájlnevét.</span><span class="sxs-lookup"><span data-stu-id="a2e10-152">The version numbers included in the paths and file names may be different for your cluster.</span></span>

        4. <span data-ttu-id="a2e10-153">Miután befejezte a fájlok másolása a távoli asztali munkamenet leválasztása.</span><span class="sxs-lookup"><span data-stu-id="a2e10-153">Disconnect the Remote Desktop session once you have finished copying the files.</span></span>

2. <span data-ttu-id="a2e10-154">A SQuirreL SQL alkalmazás indításához.</span><span class="sxs-lookup"><span data-stu-id="a2e10-154">Start the SQuirreL SQL application.</span></span> <span data-ttu-id="a2e10-155">Válassza ki a az ablak bal oldalán, **illesztőprogramok**.</span><span class="sxs-lookup"><span data-stu-id="a2e10-155">From the left of the window, select **Drivers**.</span></span>

    ![A bal oldali ablak illesztőprogramok lap](./media/hdinsight-connect-hive-jdbc-driver/squirreldrivers.png)

3. <span data-ttu-id="a2e10-157">A felső részén ikonok a **illesztőprogramok** párbeszédpanelen válassza a  **+**  ikonra kattintva hozzon létre egy illesztőprogramot.</span><span class="sxs-lookup"><span data-stu-id="a2e10-157">From the icons at the top of the **Drivers** dialog, select the **+** icon to create a driver.</span></span>

    ![Illesztőprogramok ikonok](./media/hdinsight-connect-hive-jdbc-driver/driversicons.png)

4. <span data-ttu-id="a2e10-159">Illesztőprogram hozzáadása párbeszédpanelen adja meg a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="a2e10-159">In the Add Driver dialog, add the following information:</span></span>

    * <span data-ttu-id="a2e10-160">**Név**: struktúra</span><span class="sxs-lookup"><span data-stu-id="a2e10-160">**Name**: Hive</span></span>
    * <span data-ttu-id="a2e10-161">**Példa URL-cím**:`jdbc:hive2://localhost:443/default;transportMode=http;ssl=true;httpPath=/hive2`</span><span class="sxs-lookup"><span data-stu-id="a2e10-161">**Example URL**: `jdbc:hive2://localhost:443/default;transportMode=http;ssl=true;httpPath=/hive2`</span></span>
    * <span data-ttu-id="a2e10-162">**Extra osztály elérési útja**: korábban letöltött a Hozzáadás gombra a jar-fájlok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a2e10-162">**Extra Class Path**: Use the Add button to add the jar files downloaded earlier</span></span>
    * <span data-ttu-id="a2e10-163">**Osztálynév**: org.apache.hive.jdbc.HiveDriver</span><span class="sxs-lookup"><span data-stu-id="a2e10-163">**Class Name**: org.apache.hive.jdbc.HiveDriver</span></span>

   ![illesztőprogram párbeszédpanel hozzáadása](./media/hdinsight-connect-hive-jdbc-driver/adddriver.png)

   <span data-ttu-id="a2e10-165">Kattintson a **OK** ezek a beállítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="a2e10-165">Click **OK** to save these settings.</span></span>

5. <span data-ttu-id="a2e10-166">Az SQuirreL SQL ablak bal oldalán válassza ki a **aliasok**.</span><span class="sxs-lookup"><span data-stu-id="a2e10-166">On the left of the SQuirreL SQL window, select **Aliases**.</span></span> <span data-ttu-id="a2e10-167">Kattintson a  **+**  ikonra kattintva hozzon létre egy kapcsolat aliast.</span><span class="sxs-lookup"><span data-stu-id="a2e10-167">Then click the **+** icon to create a connection alias.</span></span>

    ![Új alias hozzáadása](./media/hdinsight-connect-hive-jdbc-driver/aliases.png)

6. <span data-ttu-id="a2e10-169">A következő értékeket használja a **hozzáadása Alias** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a2e10-169">Use the following values for the **Add Alias** dialog.</span></span>

    * <span data-ttu-id="a2e10-170">**Név**: hdinsight Hive</span><span class="sxs-lookup"><span data-stu-id="a2e10-170">**Name**: Hive on HDInsight</span></span>

    * <span data-ttu-id="a2e10-171">**Illesztőprogram**: a legördülő lista segítségével válassza ki a **Hive** illesztőprogram</span><span class="sxs-lookup"><span data-stu-id="a2e10-171">**Driver**: Use the dropdown to select the **Hive** driver</span></span>

    * <span data-ttu-id="a2e10-172">**URL-cím**: jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2</span><span class="sxs-lookup"><span data-stu-id="a2e10-172">**URL**: jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2</span></span>

        <span data-ttu-id="a2e10-173">Cserélje le a **CLUSTERNAME** kifejezést a HDInsight-fürt nevére.</span><span class="sxs-lookup"><span data-stu-id="a2e10-173">Replace **CLUSTERNAME** with the name of your HDInsight cluster.</span></span>

    * <span data-ttu-id="a2e10-174">**Felhasználónév**: a HDInsight-fürt a fürt bejelentkezési fiók neve.</span><span class="sxs-lookup"><span data-stu-id="a2e10-174">**User Name**: The cluster login account name for your HDInsight cluster.</span></span> <span data-ttu-id="a2e10-175">Az alapértelmezett érték `admin`.</span><span class="sxs-lookup"><span data-stu-id="a2e10-175">The default is `admin`.</span></span>

    * <span data-ttu-id="a2e10-176">**Jelszó**: a fürt bejelentkezési fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="a2e10-176">**Password**: The password for the cluster login account.</span></span>

 ![Adja hozzá a alias párbeszédpanel](./media/hdinsight-connect-hive-jdbc-driver/addalias.png)

    <span data-ttu-id="a2e10-178">Használja a **teszt** gombra kattintva győződjön meg arról, hogy működik-e a kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="a2e10-178">Use the **Test** button to verify that the connection works.</span></span> <span data-ttu-id="a2e10-179">Ha **csatlakozni: hdinsight Hive** kiválasztása párbeszédpanel jelenik meg, **Connect** a vizsgálat végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="a2e10-179">When **Connect to: Hive on HDInsight** dialog appears, select **Connect** to perform the test.</span></span> <span data-ttu-id="a2e10-180">Ha a teszt sikeres, megjelenik egy **sikeres kapcsolat** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a2e10-180">If the test succeeds, you see a **Connection successful** dialog.</span></span>

    <span data-ttu-id="a2e10-181">A kapcsolat alias mentéséhez használja a **Ok** gomb alján a **hozzáadása Alias** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a2e10-181">To save the connection alias, use the **Ok** button at the bottom of the **Add Alias** dialog.</span></span>

7. <span data-ttu-id="a2e10-182">Az a **csatlakozni** válassza ki a legördülő menü felső részén SQuirreL SQL **hdinsight Hive**.</span><span class="sxs-lookup"><span data-stu-id="a2e10-182">From the **Connect to** dropdown at the top of SQuirreL SQL, select **Hive on HDInsight**.</span></span> <span data-ttu-id="a2e10-183">Amikor a rendszer kéri, válassza ki a **Connect**.</span><span class="sxs-lookup"><span data-stu-id="a2e10-183">When prompted, select **Connect**.</span></span>

    ![kapcsolódási párbeszédpanel](./media/hdinsight-connect-hive-jdbc-driver/connect.png)

8. <span data-ttu-id="a2e10-185">A csatlakozás után a következő lekérdezés írhat-e az SQL-lekérdezési párbeszédpanel megnyitása, és válassza a **futtatása** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a2e10-185">Once connected, enter the following query into the SQL query dialog, and then select the **Run** icon.</span></span> <span data-ttu-id="a2e10-186">Az eredmények területen meg kell jelennie a lekérdezés eredményeit.</span><span class="sxs-lookup"><span data-stu-id="a2e10-186">The results area should show the results of the query.</span></span>

        select * from hivesampletable limit 10;

    ![SQL lekérdezési párbeszédpanel megnyitása, beleértve az eredmények](./media/hdinsight-connect-hive-jdbc-driver/sqlquery.png)

## <a name="connect-from-an-example-java-application"></a><span data-ttu-id="a2e10-188">Csatlakoztatja a Java-alkalmazást például a</span><span class="sxs-lookup"><span data-stu-id="a2e10-188">Connect from an example Java application</span></span>

<span data-ttu-id="a2e10-189">Példa a HDInsight Hive lekérdezés Java-ügyfél használatával érhető el: [https://github.com/Azure-Samples/hdinsight-java-hive-jdbc](https://github.com/Azure-Samples/hdinsight-java-hive-jdbc).</span><span class="sxs-lookup"><span data-stu-id="a2e10-189">An example of using a Java client to query Hive on HDInsight is available at [https://github.com/Azure-Samples/hdinsight-java-hive-jdbc](https://github.com/Azure-Samples/hdinsight-java-hive-jdbc).</span></span> <span data-ttu-id="a2e10-190">Kövesse az utasításokat a építsenek, és futtathatja a tárházban.</span><span class="sxs-lookup"><span data-stu-id="a2e10-190">Follow the instructions in the repository to build and run the sample.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="a2e10-191">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="a2e10-191">Troubleshooting</span></span>

### <a name="unexpected-error-occurred-attempting-to-open-an-sql-connection"></a><span data-ttu-id="a2e10-192">Váratlan hiba történt egy SQL-kapcsolat megnyitása</span><span class="sxs-lookup"><span data-stu-id="a2e10-192">Unexpected Error occurred attempting to open an SQL connection</span></span>

<span data-ttu-id="a2e10-193">**A jelenség**: HDInsight-fürtök 3.3-as vagy 3.4-es verziójú való csatlakozáskor jelenhet meg, amely a váratlan hiba történt hiba.</span><span class="sxs-lookup"><span data-stu-id="a2e10-193">**Symptoms**: When connecting to an HDInsight cluster that is version 3.3 or 3.4, you may receive an error that an unexpected error occurred.</span></span> <span data-ttu-id="a2e10-194">Az alábbi veremkivonatban találhatók a hiba a következő sorokat kezdődik:</span><span class="sxs-lookup"><span data-stu-id="a2e10-194">The stack trace for this error begins with the following lines:</span></span>

```java
java.util.concurrent.ExecutionException: java.lang.RuntimeException: java.lang.NoSuchMethodError: org.apache.commons.codec.binary.Base64.<init>(I)V
at java.util.concurrent.FutureTas...(FutureTask.java:122)
at java.util.concurrent.FutureTask.get(FutureTask.java:206)
```

<span data-ttu-id="a2e10-195">**OK**: Ez a hiba oka nem egyeznek a commons-codec.jar fájl SQuirreL, és a szükséges a Hive JDBC-összetevők által használt verziójában.</span><span class="sxs-lookup"><span data-stu-id="a2e10-195">**Cause**: This error is caused by a mismatch in the version of the commons-codec.jar file used by SQuirreL and the one required by the Hive JDBC components.</span></span>

<span data-ttu-id="a2e10-196">**Megoldási**: Ez a hiba elhárításához tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="a2e10-196">**Resolution**: To fix this error, use the following steps:</span></span>

1. <span data-ttu-id="a2e10-197">Töltse le a commons-kodek jar-fájlt a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="a2e10-197">Download the commons-codec jar file from your HDInsight cluster.</span></span>

        scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/commons-codec*.jar ./commons-codec.jar

2. <span data-ttu-id="a2e10-198">Lépjen ki a SQuirreL, és keresse meg a könyvtár ahol a SQuirreL telepítve van a rendszeren.</span><span class="sxs-lookup"><span data-stu-id="a2e10-198">Exit SQuirreL, and then go to the directory where SQuirreL is installed on your system.</span></span> <span data-ttu-id="a2e10-199">A SquirreL könyvtárban a a `lib` directory, a név felülírandó a egy meglévő commons-codec.jar le: a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="a2e10-199">In the SquirreL directory, under the `lib` directory, replace the existing commons-codec.jar with the one downloaded from the HDInsight cluster.</span></span>

3. <span data-ttu-id="a2e10-200">Indítsa újra az SQuirreL.</span><span class="sxs-lookup"><span data-stu-id="a2e10-200">Restart SQuirreL.</span></span> <span data-ttu-id="a2e10-201">A hiba már nem kell végrehajtani, a HDInsight Hive történő csatlakozás során.</span><span class="sxs-lookup"><span data-stu-id="a2e10-201">The error should no longer occur when connecting to Hive on HDInsight.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a2e10-202">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a2e10-202">Next steps</span></span>

<span data-ttu-id="a2e10-203">Most, hogy rendelkezik megtudta, hogyan JDBC használhatja a Hive, az alábbi hivatkozások segítségével más módjai Azure HDInsight használata.</span><span class="sxs-lookup"><span data-stu-id="a2e10-203">Now that you have learned how to use JDBC to work with Hive, use the following links to explore other ways to work with Azure HDInsight.</span></span>

* [<span data-ttu-id="a2e10-204">Adatok feltöltése a HDInsight</span><span class="sxs-lookup"><span data-stu-id="a2e10-204">Upload data to HDInsight</span></span>](hdinsight-upload-data.md)
* [<span data-ttu-id="a2e10-205">A Hive használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="a2e10-205">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="a2e10-206">A Pig használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="a2e10-206">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="a2e10-207">MapReduce-feladatok használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="a2e10-207">Use MapReduce jobs with HDInsight</span></span>](hdinsight-use-mapreduce.md)
