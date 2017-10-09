---
title: "keresztül hello JDBC-illesztőt - Azure HDInsight Hive aaaQuery |} Microsoft Docs"
description: "Hello JDBC-illesztőt a egy Java application toosubmit Hive-lekérdezések tooHadoop használata a HDInsight. Csatlakoztassa a szoftveres és hello SQuirrel SQL ügyfélről."
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
ms.openlocfilehash: 93178da3b8d497faa4c788e91dba89c4e45d3fff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="query-hive-through-hello-jdbc-driver-in-hdinsight"></a><span data-ttu-id="1e5f4-104">Keresztül hello JDBC-illesztőt hdinsight Hive lekérdezés</span><span class="sxs-lookup"><span data-stu-id="1e5f4-104">Query Hive through hello JDBC driver in HDInsight</span></span>

[!INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

<span data-ttu-id="1e5f4-105">Ismerje meg, hogyan toouse hello JDBC-illesztőt a egy Java-alkalmazás toosubmit Hive lekérdezi az Azure HDInsight tooHadoop.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-105">Learn how toouse hello JDBC driver from a Java application toosubmit Hive queries tooHadoop in Azure HDInsight.</span></span> <span data-ttu-id="1e5f4-106">a dokumentumban szereplő információk hello bemutatja, hogyan tooconnect programozott módon, és a hello SQuirrel SQL ügyfél.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-106">hello information in this document demonstrates how tooconnect programmatically and from hello SQuirrel SQL client.</span></span>

<span data-ttu-id="1e5f4-107">A Hive JDBC felület hello további információkért lásd: [HiveJDBCInterface](https://cwiki.apache.org/confluence/display/Hive/HiveJDBCInterface).</span><span class="sxs-lookup"><span data-stu-id="1e5f4-107">For more information on hello Hive JDBC Interface, see [HiveJDBCInterface](https://cwiki.apache.org/confluence/display/Hive/HiveJDBCInterface).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1e5f4-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1e5f4-108">Prerequisites</span></span>

* <span data-ttu-id="1e5f4-109">A Hadoop on HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-109">A Hadoop on HDInsight cluster.</span></span> <span data-ttu-id="1e5f4-110">Linux-alapú vagy a Windows-alapú fürtök működik.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-110">Either Linux-based or Windows-based clusters work.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="1e5f4-111">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-111">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="1e5f4-112">További információkért lásd: [HDInsight 3.3 használatból való kivonást](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="1e5f4-112">For more information, see [HDInsight 3.3 retirement](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="1e5f4-113">[SQuirreL SQL](http://squirrel-sql.sourceforge.net/).</span><span class="sxs-lookup"><span data-stu-id="1e5f4-113">[SQuirreL SQL](http://squirrel-sql.sourceforge.net/).</span></span> <span data-ttu-id="1e5f4-114">SQuirreL JDBC ügyfélalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-114">SQuirreL is a JDBC client application.</span></span>

* <span data-ttu-id="1e5f4-115">Hello [Java fejlesztői készlet (JDK) 7-es verzió](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) vagy újabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-115">hello [Java Developer Kit (JDK) version 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) or higher.</span></span>

* <span data-ttu-id="1e5f4-116">[Apache Maven](https://maven.apache.org).</span><span class="sxs-lookup"><span data-stu-id="1e5f4-116">[Apache Maven](https://maven.apache.org).</span></span> <span data-ttu-id="1e5f4-117">Maven, a projekt felépítéséhez rendszer Java-projektek esetében ez a cikk társított hello projekt által használt.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-117">Maven is a project build system for Java projects that is used by hello project associated with this article.</span></span>

## <a name="jdbc-connection-string"></a><span data-ttu-id="1e5f4-118">JDBC-kapcsolati karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1e5f4-118">JDBC connection string</span></span>

<span data-ttu-id="1e5f4-119">JDBC kapcsolatok tooan HDInsight-fürt Azure válnak, mint a 443-as és hello forgalmát az SSL használatával lett biztonságossá.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-119">JDBC connections tooan HDInsight cluster on Azure are made over 443, and hello traffic is secured using SSL.</span></span> <span data-ttu-id="1e5f4-120">hello nyilvános átjáró mögötti elhelyezkedik hello fürtök hello forgalom toohello port, amelyet a hiveserver2-n figyel ténylegesen irányítja át.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-120">hello public gateway that hello clusters sit behind redirects hello traffic toohello port that HiveServer2 is actually listening on.</span></span> <span data-ttu-id="1e5f4-121">hello az alábbiakban látható egy példa kapcsolati karakterlánc:</span><span class="sxs-lookup"><span data-stu-id="1e5f4-121">hello following is an example connection string:</span></span>

    jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2

<span data-ttu-id="1e5f4-122">Cserélje le `CLUSTERNAME` hello nevet, a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-122">Replace `CLUSTERNAME` with hello name of your HDInsight cluster.</span></span>

## <a name="authentication"></a><span data-ttu-id="1e5f4-123">Authentication</span><span class="sxs-lookup"><span data-stu-id="1e5f4-123">Authentication</span></span>

<span data-ttu-id="1e5f4-124">Hello-kapcsolat létesítéséhez hello HDInsight fürt admin nevét és jelszavát tooauthenticate toohello fürt átjáró kell használnia.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-124">When establishing hello connection, you must use hello HDInsight cluster admin name and password tooauthenticate toohello cluster gateway.</span></span> <span data-ttu-id="1e5f4-125">JDBC-ügyfelek például SQuirreL SQL kapcsolódáskor meg kell adnia hello admin nevét és jelszavát az ügyfélbeállításokban.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-125">When connecting from JDBC clients such as SQuirreL SQL, you must enter hello admin name and password in client settings.</span></span>

<span data-ttu-id="1e5f4-126">Java-alkalmazás kell használnia hello nevet és jelszót egy kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-126">From a Java application, you must use hello name and password when establishing a connection.</span></span> <span data-ttu-id="1e5f4-127">Például hello következő Java-kóddal megnyit egy új kapcsolatot hello kapcsolati karakterláncot, admin név és jelszó használatával:</span><span class="sxs-lookup"><span data-stu-id="1e5f4-127">For example, hello following Java code opens a new connection using hello connection string, admin name, and password:</span></span>

```java
DriverManager.getConnection(connectionString,clusterAdmin,clusterPassword);
```

## <a name="connect-with-squirrel-sql-client"></a><span data-ttu-id="1e5f4-128">Csatlakozás SQuirreL SQL-ügyféllel</span><span class="sxs-lookup"><span data-stu-id="1e5f4-128">Connect with SQuirreL SQL client</span></span>

<span data-ttu-id="1e5f4-129">SQuirreL SQL, amely a HDInsight-fürthöz Hive-lekérdezések futtatásához használt tooremotely JDBC ügyfél.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-129">SQuirreL SQL is a JDBC client that can be used tooremotely run Hive queries with your HDInsight cluster.</span></span> <span data-ttu-id="1e5f4-130">hello következő lépések azt feltételezik, hogy még telepítette SQuirreL SQL.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-130">hello following steps assume that you have already installed SQuirreL SQL.</span></span>

1. <span data-ttu-id="1e5f4-131">Hello Hive JDBC illesztőprogramok másolása a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-131">Copy hello Hive JDBC drivers from your HDInsight cluster.</span></span>

    * <span data-ttu-id="1e5f4-132">A **Linux-alapú HDInsight**, használjon hello alábbi lépéseit toodownload szükséges hello jar-fájlok.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-132">For **Linux-based HDInsight**, use hello following steps toodownload hello required jar files.</span></span>

        1. <span data-ttu-id="1e5f4-133">Hozzon létre egy hello fájlokat tartalmazó könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-133">Create a directory that contains hello files.</span></span> <span data-ttu-id="1e5f4-134">Például: `mkdir hivedriver`.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-134">For example, `mkdir hivedriver`.</span></span>

        2. <span data-ttu-id="1e5f4-135">A parancssorból használjon hello következő parancsok toocopy hello fájlok hello HDInsight-fürt:</span><span class="sxs-lookup"><span data-stu-id="1e5f4-135">From a command line, use hello following commands toocopy hello files from hello HDInsight cluster:</span></span>

            ```bash
            scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/hive-jdbc*standalone.jar .
            scp USERNAME@CLUSTERNAME:/usr/hdp/current/hadoop-client/hadoop-common.jar .
            scp USERNAME@CLUSTERNAME:/usr/hdp/current/hadoop-client/hadoop-auth.jar .
            ```

            <span data-ttu-id="1e5f4-136">Cserélje le `USERNAME` nevű hello SSH felhasználói fiók hello fürthöz.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-136">Replace `USERNAME` with hello SSH user account name for hello cluster.</span></span> <span data-ttu-id="1e5f4-137">Cserélje le `CLUSTERNAME` hello HDInsight fürt névvel.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-137">Replace `CLUSTERNAME` with hello HDInsight cluster name.</span></span>

    * <span data-ttu-id="1e5f4-138">A **Windows-alapú HDInsight**, használjon hello alábbi lépéseit toodownload hello jar-fájlok.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-138">For **Windows-based HDInsight**, use hello following steps toodownload hello jar files.</span></span>

        1. <span data-ttu-id="1e5f4-139">A hello Azure-portálon, a HDInsight-fürt, majd válassza ki és hello **távoli asztal** ikonra.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-139">From hello Azure portal, select your HDInsight cluster, and then select hello **Remote Desktop** icon.</span></span>

            ![Távoli asztali ikon](./media/hdinsight-connect-hive-jdbc-driver/remotedesktopicon.png)

        2. <span data-ttu-id="1e5f4-141">A távoli asztal panelen hello használata hello **Connect** gomb tooconnect toohello fürt.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-141">On hello Remote Desktop blade, use hello **Connect** button tooconnect toohello cluster.</span></span> <span data-ttu-id="1e5f4-142">Hello távoli asztal nincs engedélyezve, ha hello űrlap tooprovide egy felhasználónevet és jelszót használják, és válasszon **engedélyezése** tooenable távoli asztal hello fürthöz.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-142">If hello Remote Desktop is not enabled, use hello form tooprovide a user name and password, then select **Enable** tooenable Remote Desktop for hello cluster.</span></span>

            ![Távoli asztali panel](./media/hdinsight-connect-hive-jdbc-driver/remotedesktopblade.png)

            <span data-ttu-id="1e5f4-144">Miután kiválasztott **Connect**, egy .rdp le.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-144">After selecting **Connect**, a .rdp file is downloaded.</span></span> <span data-ttu-id="1e5f4-145">A fájl toolaunch hello távoli asztali ügyfél használja.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-145">Use this file toolaunch hello Remote Desktop client.</span></span> <span data-ttu-id="1e5f4-146">Amikor a rendszer kéri, hello felhasználónév és a távoli asztal eléréséhez megadott jelszó használata.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-146">When prompted, use hello user name and password you entered for Remote Desktop access.</span></span>

        3. <span data-ttu-id="1e5f4-147">A csatlakozás után másolja a következő fájlok a hello távoli asztali munkamenet tooyour helyi számítógépről hello.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-147">Once connected, copy hello following files from hello Remote Desktop session tooyour local machine.</span></span> <span data-ttu-id="1e5f4-148">Nevű helyi könyvtárba helyezze őket `hivedriver`.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-148">Put them in a local directory named `hivedriver`.</span></span>

            * <span data-ttu-id="1e5f4-149">C:\apps\dist\hive-0.14.0.2.2.9.1-7\lib\hive-JDBC-0.14.0.2.2.9.1-7-Standalone.JAR</span><span class="sxs-lookup"><span data-stu-id="1e5f4-149">C:\apps\dist\hive-0.14.0.2.2.9.1-7\lib\hive-jdbc-0.14.0.2.2.9.1-7-standalone.jar</span></span>
            * <span data-ttu-id="1e5f4-150">C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\hadoop-Common-2.6.0.2.2.9.1-7.JAR</span><span class="sxs-lookup"><span data-stu-id="1e5f4-150">C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\hadoop-common-2.6.0.2.2.9.1-7.jar</span></span>
            * <span data-ttu-id="1e5f4-151">C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\lib\hadoop-auth-2.6.0.2.2.9.1-7.JAR</span><span class="sxs-lookup"><span data-stu-id="1e5f4-151">C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\lib\hadoop-auth-2.6.0.2.2.9.1-7.jar</span></span>

            > [!NOTE]
            > <span data-ttu-id="1e5f4-152">hello verzió hello elérési útját és fájlnevét szereplő számok lehetnek a fürt számára.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-152">hello version numbers included in hello paths and file names may be different for your cluster.</span></span>

        4. <span data-ttu-id="1e5f4-153">Válassza le a hello távoli asztali munkamenet befejezése hello fájlok másolása után.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-153">Disconnect hello Remote Desktop session once you have finished copying hello files.</span></span>

2. <span data-ttu-id="1e5f4-154">Hello SQuirreL SQL alkalmazás indításához.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-154">Start hello SQuirreL SQL application.</span></span> <span data-ttu-id="1e5f4-155">Hello ablak bal oldalán hello, válassza ki **illesztőprogramok**.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-155">From hello left of hello window, select **Drivers**.</span></span>

    ![Illesztőprogramok lap hello bal oldali hello ablak](./media/hdinsight-connect-hive-jdbc-driver/squirreldrivers.png)

3. <span data-ttu-id="1e5f4-157">A hello ikonok hello hello tetején **illesztőprogramok** párbeszédpanelen jelölje be hello  **+**  ikon toocreate illesztőprogramot.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-157">From hello icons at hello top of hello **Drivers** dialog, select hello **+** icon toocreate a driver.</span></span>

    ![Illesztőprogramok ikonok](./media/hdinsight-connect-hive-jdbc-driver/driversicons.png)

4. <span data-ttu-id="1e5f4-159">Hello illesztőprogram hozzáadása párbeszédpanelen adja hozzá a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="1e5f4-159">In hello Add Driver dialog, add hello following information:</span></span>

    * <span data-ttu-id="1e5f4-160">**Név**: struktúra</span><span class="sxs-lookup"><span data-stu-id="1e5f4-160">**Name**: Hive</span></span>
    * <span data-ttu-id="1e5f4-161">**Példa URL-cím**:`jdbc:hive2://localhost:443/default;transportMode=http;ssl=true;httpPath=/hive2`</span><span class="sxs-lookup"><span data-stu-id="1e5f4-161">**Example URL**: `jdbc:hive2://localhost:443/default;transportMode=http;ssl=true;httpPath=/hive2`</span></span>
    * <span data-ttu-id="1e5f4-162">**Extra osztály elérési útja**: hello Hozzáadás gomb tooadd hello jar fájlok korábban letöltött</span><span class="sxs-lookup"><span data-stu-id="1e5f4-162">**Extra Class Path**: Use hello Add button tooadd hello jar files downloaded earlier</span></span>
    * <span data-ttu-id="1e5f4-163">**Osztálynév**: org.apache.hive.jdbc.HiveDriver</span><span class="sxs-lookup"><span data-stu-id="1e5f4-163">**Class Name**: org.apache.hive.jdbc.HiveDriver</span></span>

   ![illesztőprogram párbeszédpanel hozzáadása](./media/hdinsight-connect-hive-jdbc-driver/adddriver.png)

   <span data-ttu-id="1e5f4-165">Kattintson a **OK** toosave ezeket a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-165">Click **OK** toosave these settings.</span></span>

5. <span data-ttu-id="1e5f4-166">Hello hello SQuirreL SQL ablak bal oldali, válassza ki a **aliasok**.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-166">On hello left of hello SQuirreL SQL window, select **Aliases**.</span></span> <span data-ttu-id="1e5f4-167">Kattintson a hello  **+**  ikon toocreate egy kapcsolat aliast.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-167">Then click hello **+** icon toocreate a connection alias.</span></span>

    ![Új alias hozzáadása](./media/hdinsight-connect-hive-jdbc-driver/aliases.png)

6. <span data-ttu-id="1e5f4-169">Hello használata hello következő értékei **hozzáadása Alias** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-169">Use hello following values for hello **Add Alias** dialog.</span></span>

    * <span data-ttu-id="1e5f4-170">**Név**: hdinsight Hive</span><span class="sxs-lookup"><span data-stu-id="1e5f4-170">**Name**: Hive on HDInsight</span></span>

    * <span data-ttu-id="1e5f4-171">**Illesztőprogram**: használata hello legördülő tooselect hello **Hive** illesztőprogram</span><span class="sxs-lookup"><span data-stu-id="1e5f4-171">**Driver**: Use hello dropdown tooselect hello **Hive** driver</span></span>

    * <span data-ttu-id="1e5f4-172">**URL-cím**: jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2</span><span class="sxs-lookup"><span data-stu-id="1e5f4-172">**URL**: jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2</span></span>

        <span data-ttu-id="1e5f4-173">Cserélje le **CLUSTERNAME** hello nevet, a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-173">Replace **CLUSTERNAME** with hello name of your HDInsight cluster.</span></span>

    * <span data-ttu-id="1e5f4-174">**Felhasználónév**: hello fürt bejelentkezési fiók nevét a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-174">**User Name**: hello cluster login account name for your HDInsight cluster.</span></span> <span data-ttu-id="1e5f4-175">hello alapértelmezett érték a `admin`.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-175">hello default is `admin`.</span></span>

    * <span data-ttu-id="1e5f4-176">**Jelszó**: hello hello fürt bejelentkezési fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-176">**Password**: hello password for hello cluster login account.</span></span>

 ![Adja hozzá a alias párbeszédpanel](./media/hdinsight-connect-hive-jdbc-driver/addalias.png)

    <span data-ttu-id="1e5f4-178">Használjon hello **teszt** gomb tooverify, amely hello kapcsolat működik.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-178">Use hello **Test** button tooverify that hello connection works.</span></span> <span data-ttu-id="1e5f4-179">Ha **csatlakozni: hdinsight Hive** kiválasztása párbeszédpanel jelenik meg, **Connect** tooperform hello tesztelése.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-179">When **Connect to: Hive on HDInsight** dialog appears, select **Connect** tooperform hello test.</span></span> <span data-ttu-id="1e5f4-180">Ha hello teszt sikeres, megjelenik egy **sikeres kapcsolat** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-180">If hello test succeeds, you see a **Connection successful** dialog.</span></span>

    <span data-ttu-id="1e5f4-181">toosave hello kapcsolat alias, használjon hello **Ok** hello hello alján gomb **hozzáadása Alias** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-181">toosave hello connection alias, use hello **Ok** button at hello bottom of hello **Add Alias** dialog.</span></span>

7. <span data-ttu-id="1e5f4-182">A hello **csatlakozni** SQuirreL SQL hello felső legördülő menüből válassza **hdinsight Hive**.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-182">From hello **Connect to** dropdown at hello top of SQuirreL SQL, select **Hive on HDInsight**.</span></span> <span data-ttu-id="1e5f4-183">Amikor a rendszer kéri, válassza ki a **Connect**.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-183">When prompted, select **Connect**.</span></span>

    ![kapcsolódási párbeszédpanel](./media/hdinsight-connect-hive-jdbc-driver/connect.png)

8. <span data-ttu-id="1e5f4-185">A csatlakozás után adja meg a következő lekérdezés be hello SQL lekérdezési párbeszédpanel megnyitása, és válassza a hello hello **futtatása** ikonra.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-185">Once connected, enter hello following query into hello SQL query dialog, and then select hello **Run** icon.</span></span> <span data-ttu-id="1e5f4-186">hello eredmények terület hello lekérdezési eredmények hello kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-186">hello results area should show hello results of hello query.</span></span>

        select * from hivesampletable limit 10;

    ![SQL lekérdezési párbeszédpanel megnyitása, beleértve az eredmények](./media/hdinsight-connect-hive-jdbc-driver/sqlquery.png)

## <a name="connect-from-an-example-java-application"></a><span data-ttu-id="1e5f4-188">Csatlakoztatja a Java-alkalmazást például a</span><span class="sxs-lookup"><span data-stu-id="1e5f4-188">Connect from an example Java application</span></span>

<span data-ttu-id="1e5f4-189">Példa egy Java-ügyfél tooquery Hive használata a HDInsight érhető el: [https://github.com/Azure-Samples/hdinsight-java-hive-jdbc](https://github.com/Azure-Samples/hdinsight-java-hive-jdbc).</span><span class="sxs-lookup"><span data-stu-id="1e5f4-189">An example of using a Java client tooquery Hive on HDInsight is available at [https://github.com/Azure-Samples/hdinsight-java-hive-jdbc](https://github.com/Azure-Samples/hdinsight-java-hive-jdbc).</span></span> <span data-ttu-id="1e5f4-190">Hello tárház toobuild hello utasításait követve, és futtasson hello mintát.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-190">Follow hello instructions in hello repository toobuild and run hello sample.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="1e5f4-191">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="1e5f4-191">Troubleshooting</span></span>

### <a name="unexpected-error-occurred-attempting-tooopen-an-sql-connection"></a><span data-ttu-id="1e5f4-192">Váratlan hiba történt a(z) tooopen egy SQL-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-192">Unexpected Error occurred attempting tooopen an SQL connection</span></span>

<span data-ttu-id="1e5f4-193">**A jelenség**: tooan HDInsight-fürt 3.3-as vagy 3.4-es verziójú kapcsolódáskor jelenhet meg, amely a váratlan hiba történt hiba.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-193">**Symptoms**: When connecting tooan HDInsight cluster that is version 3.3 or 3.4, you may receive an error that an unexpected error occurred.</span></span> <span data-ttu-id="1e5f4-194">Ezt a hibát hello Veremkivonat alábbi hello kezdődik:</span><span class="sxs-lookup"><span data-stu-id="1e5f4-194">hello stack trace for this error begins with hello following lines:</span></span>

```java
java.util.concurrent.ExecutionException: java.lang.RuntimeException: java.lang.NoSuchMethodError: org.apache.commons.codec.binary.Base64.<init>(I)V
at java.util.concurrent.FutureTas...(FutureTask.java:122)
at java.util.concurrent.FutureTask.get(FutureTask.java:206)
```

<span data-ttu-id="1e5f4-195">**OK**: Ez a hiba oka egy hello hello commons-codec.jar fájl SQuirreL és hello egy kötelező hello Hive JDBC-összetevők által használt verziója nem egyezik.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-195">**Cause**: This error is caused by a mismatch in hello version of hello commons-codec.jar file used by SQuirreL and hello one required by hello Hive JDBC components.</span></span>

<span data-ttu-id="1e5f4-196">**Megoldási**: Ez a hiba, a következő használatát hello lépések toofix:</span><span class="sxs-lookup"><span data-stu-id="1e5f4-196">**Resolution**: toofix this error, use hello following steps:</span></span>

1. <span data-ttu-id="1e5f4-197">Hello commons-kodek jar-fájlra letölthető a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-197">Download hello commons-codec jar file from your HDInsight cluster.</span></span>

        scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/commons-codec*.jar ./commons-codec.jar

2. <span data-ttu-id="1e5f4-198">SQuirreL kilép, és keresse toohello directory ahol a SQuirreL telepítve van a rendszeren.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-198">Exit SQuirreL, and then go toohello directory where SQuirreL is installed on your system.</span></span> <span data-ttu-id="1e5f4-199">A hello SquirreL könyvtárat, hello `lib` directory, a név felülírandó hello meglévő commons-codec.jar hello hello HDInsight-fürt egyik letölti a.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-199">In hello SquirreL directory, under hello `lib` directory, replace hello existing commons-codec.jar with hello one downloaded from hello HDInsight cluster.</span></span>

3. <span data-ttu-id="1e5f4-200">Indítsa újra az SQuirreL.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-200">Restart SQuirreL.</span></span> <span data-ttu-id="1e5f4-201">hello hiba már nem kell végrehajtani, a HDInsight tooHive kapcsolódáskor.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-201">hello error should no longer occur when connecting tooHive on HDInsight.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1e5f4-202">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1e5f4-202">Next steps</span></span>

<span data-ttu-id="1e5f4-203">Most, hogy megtanulta, hogyan toouse JDBC toowork a Hive, a következő használatát hello hivatkozásait tooexplore más módokon toowork Azure hdinsightban.</span><span class="sxs-lookup"><span data-stu-id="1e5f4-203">Now that you have learned how toouse JDBC toowork with Hive, use hello following links tooexplore other ways toowork with Azure HDInsight.</span></span>

* [<span data-ttu-id="1e5f4-204">Adatok tooHDInsight feltöltése</span><span class="sxs-lookup"><span data-stu-id="1e5f4-204">Upload data tooHDInsight</span></span>](hdinsight-upload-data.md)
* [<span data-ttu-id="1e5f4-205">A Hive használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="1e5f4-205">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="1e5f4-206">A Pig használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="1e5f4-206">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="1e5f4-207">MapReduce-feladatok használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="1e5f4-207">Use MapReduce jobs with HDInsight</span></span>](hdinsight-use-mapreduce.md)
