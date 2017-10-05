---
title: "Tartományhoz csatlakoztatott HDInsight - Azure Hive szabályzatok konfigurálására |} Microsoft Docs"
description: "Információk ...."
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 3fade1e5-c2e1-4ad5-b371-f95caea23f6d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/25/2016
ms.author: saurinsh
ms.openlocfilehash: de537d5e39dd0d3f75ff802948c7372e4d65d127
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-hive-policies-in-domain-joined-hdinsight-preview"></a><span data-ttu-id="b207b-103">Hive-házirendek konfigurálása a tartományhoz csatlakoztatott HDInsight-ban (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="b207b-103">Configure Hive policies in Domain-joined HDInsight (Preview)</span></span>
<span data-ttu-id="b207b-104">Útmutató ahhoz, hogyan lehet az Apache Ranger-házirendeket a Hive számára konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="b207b-104">Learn how to configure Apache Ranger policies for Hive.</span></span> <span data-ttu-id="b207b-105">Ebben a cikkben két Ranger-házirendet hoz létre a hivesampletable nevű táblához való hozzáférés korlátozása érdekében.</span><span class="sxs-lookup"><span data-stu-id="b207b-105">In this article, you create two Ranger policies to restrict access to the hivesampletable.</span></span> <span data-ttu-id="b207b-106">A hivesampletable HDInsight-fürtöket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b207b-106">The hivesampletable comes with HDInsight clusters.</span></span> <span data-ttu-id="b207b-107">Miután konfigurálta a házirendeket, az Excel és az ODBC-illesztőprogram használatával kapcsolódjon a HDInsight Hive-tábláihoz.</span><span class="sxs-lookup"><span data-stu-id="b207b-107">After you have configured the policies, you use Excel and ODBC driver to connect to Hive tables in HDInsight.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b207b-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b207b-108">Prerequisites</span></span>
* <span data-ttu-id="b207b-109">Tartományhoz csatlakoztatott HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="b207b-109">A Domain-joined HDInsight cluster.</span></span> <span data-ttu-id="b207b-110">Lásd a [Tartományhoz csatlakoztatott HDInsight-fürtök konfigurálása](hdinsight-domain-joined-configure.md) című részt.</span><span class="sxs-lookup"><span data-stu-id="b207b-110">See [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md).</span></span>
* <span data-ttu-id="b207b-111">Office 2016, Office 2013 Professional Plus, Office 365 Pro Plus, az Excel 2013 Standalone vagy Office 2010 Professional Plus rendszert futtató munkaállomás.</span><span class="sxs-lookup"><span data-stu-id="b207b-111">A workstation with Office 2016, Office 2013 Professional Plus, Office 365 Pro Plus, Excel 2013 Standalone, or Office 2010 Professional Plus.</span></span>

## <a name="connect-to-apache-ranger-admin-ui"></a><span data-ttu-id="b207b-112">Csatlakozás az Apache Ranger felügyeleti felhasználói felületéhez</span><span class="sxs-lookup"><span data-stu-id="b207b-112">Connect to Apache Ranger Admin UI</span></span>
<span data-ttu-id="b207b-113">**Csatlakozás az Apache Ranger felügyeleti felhasználói felületéhez**</span><span class="sxs-lookup"><span data-stu-id="b207b-113">**To connect to Ranger Admin UI**</span></span>

1. <span data-ttu-id="b207b-114">Egy böngészőből csatlakozzon a Ranger felügyeleti felhasználói felületéhez.</span><span class="sxs-lookup"><span data-stu-id="b207b-114">From a browser, connect to Ranger Admin UI.</span></span> <span data-ttu-id="b207b-115">Az URL-cím: https://&lt;ClusterName>.azurehdinsight.net/Ranger/.</span><span class="sxs-lookup"><span data-stu-id="b207b-115">The URL is https://&lt;ClusterName>.azurehdinsight.net/Ranger/.</span></span>

   > [!NOTE]
   > <span data-ttu-id="b207b-116">A Ranger más hitelesítő adatokat használ, mint a Hadoop-fürt.</span><span class="sxs-lookup"><span data-stu-id="b207b-116">Ranger uses different credentials than Hadoop cluster.</span></span> <span data-ttu-id="b207b-117">Ha szeretné megakadályozni, hogy a böngészők gyorsítótárazott Hadoop hitelesítő adatokat használjanak, új InPrivate-böngészőablakból csatlakozzon a Ranger felügyeleti felhasználói felületéhez.</span><span class="sxs-lookup"><span data-stu-id="b207b-117">To prevent browsers using cached Hadoop credentials, use new inprivate browser window to connect to the Ranger Admin UI.</span></span>
   >
   >
2. <span data-ttu-id="b207b-118">Jelentkezzen be a fürt rendszergazdai tartományi felhasználónevével és jelszavával:</span><span class="sxs-lookup"><span data-stu-id="b207b-118">Log in using the cluster administrator domain user name and password:</span></span>

    ![HDInsight-tartományhoz csatlakoztatott Ranger kezdőlapja](./media/hdinsight-domain-joined-run-hive/hdinsight-domain-joined-ranger-home-page.png)

    <span data-ttu-id="b207b-120">A Ranger jelenleg csak a Yarn és a Hive rendszerrel működik.</span><span class="sxs-lookup"><span data-stu-id="b207b-120">Currently, Ranger only works with Yarn and Hive.</span></span>

## <a name="create-domain-users"></a><span data-ttu-id="b207b-121">Tartományi felhasználók létrehozása</span><span class="sxs-lookup"><span data-stu-id="b207b-121">Create Domain users</span></span>
<span data-ttu-id="b207b-122">A [Tartományhoz csatlakoztatott HDInsight-fürtök konfigurálása](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad) című részben létrehozott egy hiveuser1 és egy hiveuser2 nevű felhasználót.</span><span class="sxs-lookup"><span data-stu-id="b207b-122">In [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad), you have created hiveruser1 and hiveuser2.</span></span> <span data-ttu-id="b207b-123">A két felhasználói fiókot fogja ebben az oktatóprogramban használni.</span><span class="sxs-lookup"><span data-stu-id="b207b-123">You will use the two user account in this tutorial.</span></span>

## <a name="create-ranger-policies"></a><span data-ttu-id="b207b-124">Ranger-házirendek létrehozása</span><span class="sxs-lookup"><span data-stu-id="b207b-124">Create Ranger policies</span></span>
<span data-ttu-id="b207b-125">Ebben a szakaszban két Ranger-házirendet fog létrehozni a hivesampletable eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="b207b-125">In this section, you will create two Ranger policies for accessing hivesampletable.</span></span> <span data-ttu-id="b207b-126">Adjon kiválasztási engedélyt a különböző oszlopcsoportokra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="b207b-126">You give select permission on different set of columns.</span></span> <span data-ttu-id="b207b-127">Mindkét felhasználó a [Tartományhoz csatlakoztatott HDInsight-fürtök konfigurálása](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad) című részben lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="b207b-127">Both users were created in [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad).</span></span>  <span data-ttu-id="b207b-128">A következő szakaszban a két házirendet Excelben fogja tesztelni.</span><span class="sxs-lookup"><span data-stu-id="b207b-128">In the next section, you will test the two policies in Excel.</span></span>

<span data-ttu-id="b207b-129">**Ranger-házirendek létrehozása**</span><span class="sxs-lookup"><span data-stu-id="b207b-129">**To create Ranger policies**</span></span>

1. <span data-ttu-id="b207b-130">Nyissa meg a Ranger felügyeleti felhasználói felületét.</span><span class="sxs-lookup"><span data-stu-id="b207b-130">Open Ranger Admin UI.</span></span> <span data-ttu-id="b207b-131">Lásd a [Csatlakozás az Apache Ranger felügyeleti felhasználói felületéhez](#connect-to-apache-ranager-admin-ui) című részt.</span><span class="sxs-lookup"><span data-stu-id="b207b-131">See [Connect to Apache Ranger Admin UI](#connect-to-apache-ranager-admin-ui).</span></span>
2. <span data-ttu-id="b207b-132">A **Hive** alatt kattintson a **&lt;ClusterName>_hive** elemre.</span><span class="sxs-lookup"><span data-stu-id="b207b-132">Click **&lt;ClusterName>_hive**, under **Hive**.</span></span> <span data-ttu-id="b207b-133">Két előre konfigurált házirendnek kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="b207b-133">You shall see two pre-configure policies.</span></span>
3. <span data-ttu-id="b207b-134">Kattintson az **Új házirend hozzáadása** gombra, majd adja meg a következő értékeket:</span><span class="sxs-lookup"><span data-stu-id="b207b-134">Click **Add New Policy**, and then enter the following values:</span></span>

   * <span data-ttu-id="b207b-135">Házirend neve: read-hivesampletable-all</span><span class="sxs-lookup"><span data-stu-id="b207b-135">Policy name: read-hivesampletable-all</span></span>
   * <span data-ttu-id="b207b-136">Hive-adatbázis: alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="b207b-136">Hive Database: default</span></span>
   * <span data-ttu-id="b207b-137">tábla: hivesampletable</span><span class="sxs-lookup"><span data-stu-id="b207b-137">table: hivesampletable</span></span>
   * <span data-ttu-id="b207b-138">Hive-oszlop: *</span><span class="sxs-lookup"><span data-stu-id="b207b-138">Hive column: *</span></span>
   * <span data-ttu-id="b207b-139">Felhasználó kiválasztása: hiveuser1</span><span class="sxs-lookup"><span data-stu-id="b207b-139">Select User: hiveuser1</span></span>
   * <span data-ttu-id="b207b-140">Engedélyek: kiválasztás</span><span class="sxs-lookup"><span data-stu-id="b207b-140">Permissions: select</span></span>

     ![HDInsight-tartományhoz csatlakoztatott Ranger Hive-házirendjének konfigurálása](./media/hdinsight-domain-joined-run-hive/hdinsight-domain-joined-configure-ranger-policy.png)<span data-ttu-id="b207b-142">.</span><span class="sxs-lookup"><span data-stu-id="b207b-142">.</span></span>

     > [!NOTE]
     > <span data-ttu-id="b207b-143">Ha egy tartományi felhasználó nem töltődik be a Felhasználó kiválasztása részben, várjon néhány másodpercig, amíg a Ranger szinkronizálódik az AAD-val.</span><span class="sxs-lookup"><span data-stu-id="b207b-143">If a domain user is not populated in Select User, wait a few moments for Ranger to sync with AAD.</span></span>
     >
     >
4. <span data-ttu-id="b207b-144">A házirend mentéséhez kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="b207b-144">Click **Add** to save the policy.</span></span>
5. <span data-ttu-id="b207b-145">Ismételje meg az utolsó két lépést egy másik házirend létrehozásához, amely a következő tulajdonságokkal rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="b207b-145">Repeat the last two steps to create another policy with the following properties:</span></span>

   * <span data-ttu-id="b207b-146">Házirend neve: read-hivesampletable-devicemake</span><span class="sxs-lookup"><span data-stu-id="b207b-146">Policy name: read-hivesampletable-devicemake</span></span>
   * <span data-ttu-id="b207b-147">Hive-adatbázis: alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="b207b-147">Hive Database: default</span></span>
   * <span data-ttu-id="b207b-148">tábla: hivesampletable</span><span class="sxs-lookup"><span data-stu-id="b207b-148">table: hivesampletable</span></span>
   * <span data-ttu-id="b207b-149">Hive-oszlop: clientid devicemake</span><span class="sxs-lookup"><span data-stu-id="b207b-149">Hive column: clientid, devicemake</span></span>
   * <span data-ttu-id="b207b-150">Felhasználó kiválasztása: hiveuser2</span><span class="sxs-lookup"><span data-stu-id="b207b-150">Select User: hiveuser2</span></span>
   * <span data-ttu-id="b207b-151">Engedélyek: kiválasztás</span><span class="sxs-lookup"><span data-stu-id="b207b-151">Permissions: select</span></span>

## <a name="create-hive-odbc-data-source"></a><span data-ttu-id="b207b-152">Hive ODBC-adatforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="b207b-152">Create Hive ODBC data source</span></span>
<span data-ttu-id="b207b-153">Az utasítások a [Hive ODBC-adatforrás létrehozása](hdinsight-connect-excel-hive-odbc-driver.md) című részben találhatók.</span><span class="sxs-lookup"><span data-stu-id="b207b-153">The instructions can be found in [Create Hive ODBC data source](hdinsight-connect-excel-hive-odbc-driver.md).</span></span>  

    <span data-ttu-id="b207b-154">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b207b-154">Property</span></span>|<span data-ttu-id="b207b-155">Leírás</span><span class="sxs-lookup"><span data-stu-id="b207b-155">Description</span></span>
    ---|---
    <span data-ttu-id="b207b-156">Adatforrás neve</span><span class="sxs-lookup"><span data-stu-id="b207b-156">Data Source Name</span></span>|<span data-ttu-id="b207b-157">Adjon nevet az adatforrásának</span><span class="sxs-lookup"><span data-stu-id="b207b-157">Give a name to your data source</span></span>
    <span data-ttu-id="b207b-158">Gazdagép</span><span class="sxs-lookup"><span data-stu-id="b207b-158">Host</span></span>|<span data-ttu-id="b207b-159">Írja be a következő kifejezést: &lt;HDInsightClusterName>.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="b207b-159">Enter &lt;HDInsightClusterName>.azurehdinsight.net.</span></span> <span data-ttu-id="b207b-160">Például: sajatHDICluster.azurehdinsight.net</span><span class="sxs-lookup"><span data-stu-id="b207b-160">For example, myHDICluster.azurehdinsight.net</span></span>
    <span data-ttu-id="b207b-161">Port</span><span class="sxs-lookup"><span data-stu-id="b207b-161">Port</span></span>|<span data-ttu-id="b207b-162">Használja a <strong>443</strong> számú portot.</span><span class="sxs-lookup"><span data-stu-id="b207b-162">Use <strong>443</strong>.</span></span> <span data-ttu-id="b207b-163">(Ez a port megváltozott a 563-ról 443-ra.)</span><span class="sxs-lookup"><span data-stu-id="b207b-163">(This port has been changed from 563 to 443.)</span></span>
    <span data-ttu-id="b207b-164">Adatbázis</span><span class="sxs-lookup"><span data-stu-id="b207b-164">Database</span></span>|<span data-ttu-id="b207b-165">Használja az <strong>Alapértelmezett</strong> adatbázist.</span><span class="sxs-lookup"><span data-stu-id="b207b-165">Use <strong>Default</strong>.</span></span>
    <span data-ttu-id="b207b-166">Hive Server típusa</span><span class="sxs-lookup"><span data-stu-id="b207b-166">Hive Server Type</span></span>|<span data-ttu-id="b207b-167">Válassza ki a <strong>Hive Server 2</strong> típust</span><span class="sxs-lookup"><span data-stu-id="b207b-167">Select <strong>Hive Server 2</strong></span></span>
    <span data-ttu-id="b207b-168">Mechanizmus</span><span class="sxs-lookup"><span data-stu-id="b207b-168">Mechanism</span></span>|<span data-ttu-id="b207b-169">Válassza ki az <strong>Azure HDInsight szolgáltatást</strong></span><span class="sxs-lookup"><span data-stu-id="b207b-169">Select <strong>Azure HDInsight Service</strong></span></span>
    <span data-ttu-id="b207b-170">HTTP elérési útja</span><span class="sxs-lookup"><span data-stu-id="b207b-170">HTTP Path</span></span>|<span data-ttu-id="b207b-171">Hagyja üresen.</span><span class="sxs-lookup"><span data-stu-id="b207b-171">Leave it blank.</span></span>
    <span data-ttu-id="b207b-172">Felhasználónév</span><span class="sxs-lookup"><span data-stu-id="b207b-172">User Name</span></span>|<span data-ttu-id="b207b-173">Adja meg hiveuser1@contoso158.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="b207b-173">Enter hiveuser1@contoso158.onmicrosoft.com.</span></span> <span data-ttu-id="b207b-174">Frissítse a tartomány nevét, ha más.</span><span class="sxs-lookup"><span data-stu-id="b207b-174">Update the domain name if it is different.</span></span>
    <span data-ttu-id="b207b-175">Jelszó</span><span class="sxs-lookup"><span data-stu-id="b207b-175">Password</span></span>|<span data-ttu-id="b207b-176">Adja meg a hiveuser1 jelszavát.</span><span class="sxs-lookup"><span data-stu-id="b207b-176">Enter the password for hiveuser1.</span></span>
    </table>

<span data-ttu-id="b207b-177">Az adatforrás mentése előtt kattintson a **Tesztelés** gombra.</span><span class="sxs-lookup"><span data-stu-id="b207b-177">Make sure to click **Test** before saving the data source.</span></span>

## <a name="import-data-into-excel-from-hdinsight"></a><span data-ttu-id="b207b-178">Adatok importálása Excel formátumba a HDInsight-ból</span><span class="sxs-lookup"><span data-stu-id="b207b-178">Import data into Excel from HDInsight</span></span>
<span data-ttu-id="b207b-179">Az utolsó szakaszban két házirendet konfigurált.</span><span class="sxs-lookup"><span data-stu-id="b207b-179">In the last section, you have configured two policies.</span></span>  <span data-ttu-id="b207b-180">A hiveuser1 nevű felhasználó az összes oszlopra, míg hiveuser2 csak két oszlopra vonatkozó kiválasztási engedéllyel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b207b-180">hiveuser1 has the select permission on all the columns, and hiveuser2 has the select permission on two columns.</span></span> <span data-ttu-id="b207b-181">Ebben a szakaszban Ön megszemélyesíti a két felhasználót, hogy adatokat importáljon Excel formátumba.</span><span class="sxs-lookup"><span data-stu-id="b207b-181">In this section, you impersonate the two users to import data into Excel.</span></span>

1. <span data-ttu-id="b207b-182">Nyisson meg egy új vagy egy meglévő munkafüzetet Excelben.</span><span class="sxs-lookup"><span data-stu-id="b207b-182">Open a new or existing workbook in Excel.</span></span>
2. <span data-ttu-id="b207b-183">Az **Adatok** lapon kattintson az **Egyéb adatforrásokból** elemre, majd kattintson az **Adatkapcsolat varázslóból** elemre, hogy elindítsa az **Adatkapcsolat varázslót**.</span><span class="sxs-lookup"><span data-stu-id="b207b-183">From the **Data** tab, click **From Other Data Sources**, and then click **From Data Connection Wizard** to launch the **Data Connection Wizard**.</span></span>

    <span data-ttu-id="b207b-184">![Adatkapcsolat varázsló megnyitása] [img-hdi-simbahiveodbc.excel.dataconnection]</span><span class="sxs-lookup"><span data-stu-id="b207b-184">![Open data connection wizard][img-hdi-simbahiveodbc.excel.dataconnection]</span></span>
3. <span data-ttu-id="b207b-185">Válassza ki az **ODBC DSN** adatforrást, majd kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="b207b-185">Select **ODBC DSN** as the data source, and then click **Next**.</span></span>
4. <span data-ttu-id="b207b-186">Az ODBC-adatforrások közül válassza ki az előző lépésben létrehozott adatforrásnevet, majd kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="b207b-186">From ODBC data sources, select the data source name that you created in the previous step, and then  click **Next**.</span></span>
5. <span data-ttu-id="b207b-187">Írja be újból a fürthöz tartozó jelszót a varázslóban, majd kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="b207b-187">Re-enter the password for the cluster in the wizard, and then click **OK**.</span></span> <span data-ttu-id="b207b-188">Várja meg, amíg megnyílik az **Adatbázis és tábla kiválasztása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b207b-188">Wait for the **Select Database and Table** dialog to open.</span></span> <span data-ttu-id="b207b-189">Ez eltarthat néhány másodpercig.</span><span class="sxs-lookup"><span data-stu-id="b207b-189">This can take a few seconds.</span></span>
6. <span data-ttu-id="b207b-190">Válassza ki a **hivesampletable** táblát, majd kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="b207b-190">Select **hivesampletable**, and then click **Next**.</span></span>
7. <span data-ttu-id="b207b-191">Kattintson a **Finish** (Befejezés) gombra.</span><span class="sxs-lookup"><span data-stu-id="b207b-191">Click **Finish**.</span></span>
8. <span data-ttu-id="b207b-192">Az **Adatok importálása** párbeszédpanelen módosíthatja vagy megadhatja a lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="b207b-192">In the **Import Data** dialog, you can change or specify the query.</span></span> <span data-ttu-id="b207b-193">Ehhez kattintson a **Tulajdonságok** elemre.</span><span class="sxs-lookup"><span data-stu-id="b207b-193">To do so, click **Properties**.</span></span> <span data-ttu-id="b207b-194">Ez eltarthat néhány másodpercig.</span><span class="sxs-lookup"><span data-stu-id="b207b-194">This can take a few seconds.</span></span>
9. <span data-ttu-id="b207b-195">Kattintson a **Definíció** fülre.</span><span class="sxs-lookup"><span data-stu-id="b207b-195">Click the **Definition** tab.</span></span> <span data-ttu-id="b207b-196">A parancs szövege a következő:</span><span class="sxs-lookup"><span data-stu-id="b207b-196">The command text is:</span></span>

       SELECT * FROM "HIVE"."default"."hivesampletable"

   <span data-ttu-id="b207b-197">A definiált Ranger-házirendek alapján a hiveuser1 az összes oszlopra vonatkozó kiválasztási engedéllyel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b207b-197">By the Ranger policies you defined,  hiveuser1 has select permission on all the columns.</span></span>  <span data-ttu-id="b207b-198">Ezért ez a lekérdezés hiveuser1 hitelesítő adataival működik, azonban hiveuser2 hitelesítő adataival nem működik.</span><span class="sxs-lookup"><span data-stu-id="b207b-198">So this query works with hiveuser1's credentials, but this query does not not work with hiveuser2's credentials.</span></span>

   <span data-ttu-id="b207b-199">![Kapcsolat tulajdonságai] [img-hdi-simbahiveodbc-excel-connectionproperties]</span><span class="sxs-lookup"><span data-stu-id="b207b-199">![Connection Properties][img-hdi-simbahiveodbc-excel-connectionproperties]</span></span>
10. <span data-ttu-id="b207b-200">A kapcsolat tulajdonságai párbeszédpanel bezárásához kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="b207b-200">Click **OK** to close the Connection Properties dialog.</span></span>
11. <span data-ttu-id="b207b-201">Az **Adatok importálása** párbeszédpanel bezárásához kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="b207b-201">Click **OK** to close the **Import Data** dialog.</span></span>  
12. <span data-ttu-id="b207b-202">Írja be újra a hiveuser1 jelszavát, majd kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="b207b-202">Reenter the password for hiveuser1, and then click **OK**.</span></span> <span data-ttu-id="b207b-203">Az adatok importálása az Excelbe néhány másodpercet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="b207b-203">It takes a few seconds before data gets imported to Excel.</span></span> <span data-ttu-id="b207b-204">Amikor kész van, 11 adatoszlopnak kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="b207b-204">When it is done, you shall see 11 columns of data.</span></span>

<span data-ttu-id="b207b-205">Az utolsó szakaszban létrehozott második (read-hivesampletable-devicemake) házirend tesztelése</span><span class="sxs-lookup"><span data-stu-id="b207b-205">To test the second policy (read-hivesampletable-devicemake) you created in the last section</span></span>

1. <span data-ttu-id="b207b-206">Adjon hozzá egy új munkalapot az Excelben.</span><span class="sxs-lookup"><span data-stu-id="b207b-206">Add a new sheet in Excel.</span></span>
2. <span data-ttu-id="b207b-207">Az adatok importálásához kövesse az utolsó eljárást.</span><span class="sxs-lookup"><span data-stu-id="b207b-207">Follow the last procedure to import the data.</span></span>  <span data-ttu-id="b207b-208">Csupán annyit kell változtatnia, hogy a hiveuser2 hitelesítő adatai helyett a hiveuser1 hitelesítő adatait használja.</span><span class="sxs-lookup"><span data-stu-id="b207b-208">The only change you will make is to use hiveuser2's credentials instead of hiveuser1's.</span></span> <span data-ttu-id="b207b-209">Ez sikertelenséghez vezet, mert hiveuser2 csak két oszlop megtekintésére jogosult.</span><span class="sxs-lookup"><span data-stu-id="b207b-209">This will fail because hiveuser2 only has permission to see two columns.</span></span> <span data-ttu-id="b207b-210">A következő hibaüzenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="b207b-210">You shall get the following error:</span></span>

        [Microsoft][HiveODBC] (35) Error from Hive: error code: '40000' error message: 'Error while compiling statement: FAILED: HiveAccessControlException Permission denied: user [hiveuser2] does not have [SELECT] privilege on [default/hivesampletable/clientid,country ...]'.
3. <span data-ttu-id="b207b-211">Az adatok importálásához kövesse ugyanazt az eljárást.</span><span class="sxs-lookup"><span data-stu-id="b207b-211">Follow the same procedure to import data.</span></span> <span data-ttu-id="b207b-212">Ez alkalommal hiveuser2 hitelesítő adatait használja, és módosítsa a kiválasztási utasítást erről:</span><span class="sxs-lookup"><span data-stu-id="b207b-212">This time, use hiveuser2's credentials, and also modify the select statement from:</span></span>

        SELECT * FROM "HIVE"."default"."hivesampletable"

    <span data-ttu-id="b207b-213">erre:</span><span class="sxs-lookup"><span data-stu-id="b207b-213">to:</span></span>

        SELECT clientid, devicemake FROM "HIVE"."default"."hivesampletable"

    <span data-ttu-id="b207b-214">Amikor kész van, két importáltadat-oszlopnak kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="b207b-214">When it is done, you shall see two columns of data imported.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b207b-215">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b207b-215">Next steps</span></span>
* <span data-ttu-id="b207b-216">A tartományhoz csatlakoztatott HDInsight-fürtök konfigurálásához lásd: [Tartományhoz csatlakoztatott HDInsight-fürtök konfigurálása](hdinsight-domain-joined-configure.md).</span><span class="sxs-lookup"><span data-stu-id="b207b-216">For configuring a Domain-joined HDInsight cluster, see [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md).</span></span>
* <span data-ttu-id="b207b-217">A tartományhoz csatlakoztatott HDInsight-fürtök kezeléséhhez lásd: [Tartományhoz csatlakoztatott HDInsight-fürtök kezelése](hdinsight-domain-joined-manage.md).</span><span class="sxs-lookup"><span data-stu-id="b207b-217">For managing a Domain-joined HDInsight clusters, see [Manage Domain-joined HDInsight clusters](hdinsight-domain-joined-manage.md).</span></span>
* <span data-ttu-id="b207b-218">Az SSH használata a tartományhoz csatlakoztatott HDInsight-fürtökön Hive-lekérdezéseket futtat, tekintse meg a [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).</span><span class="sxs-lookup"><span data-stu-id="b207b-218">For running Hive queries using SSH on Domain-joined HDInsight clusters, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).</span></span>
* <span data-ttu-id="b207b-219">A Hive JDBC segítségével történő Hive-csatlakoztatáshoz olvassa el a [Csatlakozás a Hive-hoz az Azure HDInsight rendszerben a Hive JDBC-illesztőprogrammal](hdinsight-connect-hive-jdbc-driver.md) című részt</span><span class="sxs-lookup"><span data-stu-id="b207b-219">For Connecting Hive using Hive JDBC, see [Connect to Hive on Azure HDInsight using the Hive JDBC driver](hdinsight-connect-hive-jdbc-driver.md)</span></span>
* <span data-ttu-id="b207b-220">Ha az Excelt a Hive ODBC segítségével szeretné a Hadoophoz csatlakoztatni, olvassa el [Az Excel csatlakoztatása a Hadoophoz a Microsoft Hive ODBC-meghajtó segítségével](hdinsight-connect-excel-hive-odbc-driver.md) című részt</span><span class="sxs-lookup"><span data-stu-id="b207b-220">For connecting Excel to Hadoop using Hive ODBC, see [Connect Excel to Hadoop with the Microsoft Hive ODBC drive](hdinsight-connect-excel-hive-odbc-driver.md)</span></span>
* <span data-ttu-id="b207b-221">Ha az Excelt a Power Query segítségével szeretné a Hadoophoz csatlakoztatni, olvassa el [Az Excel csatlakoztatása a Hadoophoz a Power Query segítségével](hdinsight-connect-excel-power-query.md) című részt</span><span class="sxs-lookup"><span data-stu-id="b207b-221">For connecting Excel to Hadoop using Power Query, see [Connect Excel to Hadoop by using Power Query](hdinsight-connect-excel-power-query.md)</span></span>
