---
title: "a tartományhoz csatlakoztatott HDInsight - Azure aaaConfigure Hive házirendek |} Microsoft Docs"
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
ms.openlocfilehash: 56f2bf9d872abc5f772b886fcf91c2e2422092f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hive-policies-in-domain-joined-hdinsight-preview"></a><span data-ttu-id="f10b8-103">Hive-házirendek konfigurálása a tartományhoz csatlakoztatott HDInsight-ban (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="f10b8-103">Configure Hive policies in Domain-joined HDInsight (Preview)</span></span>
<span data-ttu-id="f10b8-104">Megtudhatja, hogyan tooconfigure Apache Pletyka házirendek a Hive.</span><span class="sxs-lookup"><span data-stu-id="f10b8-104">Learn how tooconfigure Apache Ranger policies for Hive.</span></span> <span data-ttu-id="f10b8-105">Ebben a cikkben létrehoz két Pletyka házirendek toorestrict hozzáférés toohello hivesampletable.</span><span class="sxs-lookup"><span data-stu-id="f10b8-105">In this article, you create two Ranger policies toorestrict access toohello hivesampletable.</span></span> <span data-ttu-id="f10b8-106">a HDInsight-fürtök hello hivesampletable tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="f10b8-106">hello hivesampletable comes with HDInsight clusters.</span></span> <span data-ttu-id="f10b8-107">Miután konfigurálta hello házirendeket, a Hdinsightban az Excel és az ODBC illesztőprogram tooconnect tooHive táblák.</span><span class="sxs-lookup"><span data-stu-id="f10b8-107">After you have configured hello policies, you use Excel and ODBC driver tooconnect tooHive tables in HDInsight.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f10b8-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f10b8-108">Prerequisites</span></span>
* <span data-ttu-id="f10b8-109">Tartományhoz csatlakoztatott HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="f10b8-109">A Domain-joined HDInsight cluster.</span></span> <span data-ttu-id="f10b8-110">Lásd a [Tartományhoz csatlakoztatott HDInsight-fürtök konfigurálása](hdinsight-domain-joined-configure.md) című részt.</span><span class="sxs-lookup"><span data-stu-id="f10b8-110">See [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md).</span></span>
* <span data-ttu-id="f10b8-111">Office 2016, Office 2013 Professional Plus, Office 365 Pro Plus, az Excel 2013 Standalone vagy Office 2010 Professional Plus rendszert futtató munkaállomás.</span><span class="sxs-lookup"><span data-stu-id="f10b8-111">A workstation with Office 2016, Office 2013 Professional Plus, Office 365 Pro Plus, Excel 2013 Standalone, or Office 2010 Professional Plus.</span></span>

## <a name="connect-tooapache-ranger-admin-ui"></a><span data-ttu-id="f10b8-112">Csatlakozás tooApache Pletyka felügyeleti felhasználói felület</span><span class="sxs-lookup"><span data-stu-id="f10b8-112">Connect tooApache Ranger Admin UI</span></span>
<span data-ttu-id="f10b8-113">**tooconnect tooRanger felügyeleti felhasználói felület**</span><span class="sxs-lookup"><span data-stu-id="f10b8-113">**tooconnect tooRanger Admin UI**</span></span>

1. <span data-ttu-id="f10b8-114">Egy böngészőből csatlakozás tooRanger felügyeleti felhasználói felület.</span><span class="sxs-lookup"><span data-stu-id="f10b8-114">From a browser, connect tooRanger Admin UI.</span></span> <span data-ttu-id="f10b8-115">hello URL-címe: https://&lt;ClusterName >.azurehdinsight.net/Ranger/.</span><span class="sxs-lookup"><span data-stu-id="f10b8-115">hello URL is https://&lt;ClusterName>.azurehdinsight.net/Ranger/.</span></span>

   > [!NOTE]
   > <span data-ttu-id="f10b8-116">A Ranger más hitelesítő adatokat használ, mint a Hadoop-fürt.</span><span class="sxs-lookup"><span data-stu-id="f10b8-116">Ranger uses different credentials than Hadoop cluster.</span></span> <span data-ttu-id="f10b8-117">gyorsítótárazott hitelesítő adatok használata Hadoop, tooprevent böngészők arra használják új InPrivate-böngésző ablak tooconnect toohello Pletyka felügyeleti felhasználói felület.</span><span class="sxs-lookup"><span data-stu-id="f10b8-117">tooprevent browsers using cached Hadoop credentials, use new inprivate browser window tooconnect toohello Ranger Admin UI.</span></span>
   >
   >
2. <span data-ttu-id="f10b8-118">Jelentkezzen be hello fürt rendszergazdai tartományi felhasználónevet és jelszót:</span><span class="sxs-lookup"><span data-stu-id="f10b8-118">Log in using hello cluster administrator domain user name and password:</span></span>

    ![HDInsight-tartományhoz csatlakoztatott Ranger kezdőlapja](./media/hdinsight-domain-joined-run-hive/hdinsight-domain-joined-ranger-home-page.png)

    <span data-ttu-id="f10b8-120">A Ranger jelenleg csak a Yarn és a Hive rendszerrel működik.</span><span class="sxs-lookup"><span data-stu-id="f10b8-120">Currently, Ranger only works with Yarn and Hive.</span></span>

## <a name="create-domain-users"></a><span data-ttu-id="f10b8-121">Tartományi felhasználók létrehozása</span><span class="sxs-lookup"><span data-stu-id="f10b8-121">Create Domain users</span></span>
<span data-ttu-id="f10b8-122">A [Tartományhoz csatlakoztatott HDInsight-fürtök konfigurálása](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad) című részben létrehozott egy hiveuser1 és egy hiveuser2 nevű felhasználót.</span><span class="sxs-lookup"><span data-stu-id="f10b8-122">In [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad), you have created hiveruser1 and hiveuser2.</span></span> <span data-ttu-id="f10b8-123">Ebben az oktatóanyagban hello két felhasználói fiókot fogja használni.</span><span class="sxs-lookup"><span data-stu-id="f10b8-123">You will use hello two user account in this tutorial.</span></span>

## <a name="create-ranger-policies"></a><span data-ttu-id="f10b8-124">Ranger-házirendek létrehozása</span><span class="sxs-lookup"><span data-stu-id="f10b8-124">Create Ranger policies</span></span>
<span data-ttu-id="f10b8-125">Ebben a szakaszban két Ranger-házirendet fog létrehozni a hivesampletable eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="f10b8-125">In this section, you will create two Ranger policies for accessing hivesampletable.</span></span> <span data-ttu-id="f10b8-126">Adjon kiválasztási engedélyt a különböző oszlopcsoportokra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="f10b8-126">You give select permission on different set of columns.</span></span> <span data-ttu-id="f10b8-127">Mindkét felhasználó a [Tartományhoz csatlakoztatott HDInsight-fürtök konfigurálása](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad) című részben lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="f10b8-127">Both users were created in [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad).</span></span>  <span data-ttu-id="f10b8-128">A következő szakaszban hello tesztelni hello két házirend, az Excel programban.</span><span class="sxs-lookup"><span data-stu-id="f10b8-128">In hello next section, you will test hello two policies in Excel.</span></span>

<span data-ttu-id="f10b8-129">**toocreate Pletyka házirendek**</span><span class="sxs-lookup"><span data-stu-id="f10b8-129">**toocreate Ranger policies**</span></span>

1. <span data-ttu-id="f10b8-130">Nyissa meg a Ranger felügyeleti felhasználói felületét.</span><span class="sxs-lookup"><span data-stu-id="f10b8-130">Open Ranger Admin UI.</span></span> <span data-ttu-id="f10b8-131">Lásd: [tooApache Pletyka felügyeleti felhasználói felület csatlakozás](#connect-to-apache-ranager-admin-ui).</span><span class="sxs-lookup"><span data-stu-id="f10b8-131">See [Connect tooApache Ranger Admin UI](#connect-to-apache-ranager-admin-ui).</span></span>
2. <span data-ttu-id="f10b8-132">A **Hive** alatt kattintson a **&lt;ClusterName>_hive** elemre.</span><span class="sxs-lookup"><span data-stu-id="f10b8-132">Click **&lt;ClusterName>_hive**, under **Hive**.</span></span> <span data-ttu-id="f10b8-133">Két előre konfigurált házirendnek kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="f10b8-133">You shall see two pre-configure policies.</span></span>
3. <span data-ttu-id="f10b8-134">Kattintson a **új házirend hozzáadása**, és írja be a következő értékek hello:</span><span class="sxs-lookup"><span data-stu-id="f10b8-134">Click **Add New Policy**, and then enter hello following values:</span></span>

   * <span data-ttu-id="f10b8-135">Házirend neve: read-hivesampletable-all</span><span class="sxs-lookup"><span data-stu-id="f10b8-135">Policy name: read-hivesampletable-all</span></span>
   * <span data-ttu-id="f10b8-136">Hive-adatbázis: alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="f10b8-136">Hive Database: default</span></span>
   * <span data-ttu-id="f10b8-137">tábla: hivesampletable</span><span class="sxs-lookup"><span data-stu-id="f10b8-137">table: hivesampletable</span></span>
   * <span data-ttu-id="f10b8-138">Hive-oszlop: *</span><span class="sxs-lookup"><span data-stu-id="f10b8-138">Hive column: *</span></span>
   * <span data-ttu-id="f10b8-139">Felhasználó kiválasztása: hiveuser1</span><span class="sxs-lookup"><span data-stu-id="f10b8-139">Select User: hiveuser1</span></span>
   * <span data-ttu-id="f10b8-140">Engedélyek: kiválasztás</span><span class="sxs-lookup"><span data-stu-id="f10b8-140">Permissions: select</span></span>

     ![HDInsight-tartományhoz csatlakoztatott Ranger Hive-házirendjének konfigurálása](./media/hdinsight-domain-joined-run-hive/hdinsight-domain-joined-configure-ranger-policy.png)<span data-ttu-id="f10b8-142">.</span><span class="sxs-lookup"><span data-stu-id="f10b8-142">.</span></span>

     > [!NOTE]
     > <span data-ttu-id="f10b8-143">Ha egy tartományi felhasználó nincs feltöltve a felhasználó kiválasztása, pár perc múlva a Pletyka toosync AAD-ben.</span><span class="sxs-lookup"><span data-stu-id="f10b8-143">If a domain user is not populated in Select User, wait a few moments for Ranger toosync with AAD.</span></span>
     >
     >
4. <span data-ttu-id="f10b8-144">Kattintson a **Hozzáadás** toosave hello házirend.</span><span class="sxs-lookup"><span data-stu-id="f10b8-144">Click **Add** toosave hello policy.</span></span>
5. <span data-ttu-id="f10b8-145">Ismételje meg a hello utolsó két lépést toocreate egy másik házirendet a következő tulajdonságai hello:</span><span class="sxs-lookup"><span data-stu-id="f10b8-145">Repeat hello last two steps toocreate another policy with hello following properties:</span></span>

   * <span data-ttu-id="f10b8-146">Házirend neve: read-hivesampletable-devicemake</span><span class="sxs-lookup"><span data-stu-id="f10b8-146">Policy name: read-hivesampletable-devicemake</span></span>
   * <span data-ttu-id="f10b8-147">Hive-adatbázis: alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="f10b8-147">Hive Database: default</span></span>
   * <span data-ttu-id="f10b8-148">tábla: hivesampletable</span><span class="sxs-lookup"><span data-stu-id="f10b8-148">table: hivesampletable</span></span>
   * <span data-ttu-id="f10b8-149">Hive-oszlop: clientid devicemake</span><span class="sxs-lookup"><span data-stu-id="f10b8-149">Hive column: clientid, devicemake</span></span>
   * <span data-ttu-id="f10b8-150">Felhasználó kiválasztása: hiveuser2</span><span class="sxs-lookup"><span data-stu-id="f10b8-150">Select User: hiveuser2</span></span>
   * <span data-ttu-id="f10b8-151">Engedélyek: kiválasztás</span><span class="sxs-lookup"><span data-stu-id="f10b8-151">Permissions: select</span></span>

## <a name="create-hive-odbc-data-source"></a><span data-ttu-id="f10b8-152">Hive ODBC-adatforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="f10b8-152">Create Hive ODBC data source</span></span>
<span data-ttu-id="f10b8-153">hello utasítások megtalálhatók [létrehozása Hive ODBC-adatforrás](hdinsight-connect-excel-hive-odbc-driver.md).</span><span class="sxs-lookup"><span data-stu-id="f10b8-153">hello instructions can be found in [Create Hive ODBC data source](hdinsight-connect-excel-hive-odbc-driver.md).</span></span>  

    <span data-ttu-id="f10b8-154">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="f10b8-154">Property</span></span>|<span data-ttu-id="f10b8-155">Leírás</span><span class="sxs-lookup"><span data-stu-id="f10b8-155">Description</span></span>
    ---|---
    <span data-ttu-id="f10b8-156">Adatforrás neve</span><span class="sxs-lookup"><span data-stu-id="f10b8-156">Data Source Name</span></span>|<span data-ttu-id="f10b8-157">Adjon nevet tooyour adatforrás</span><span class="sxs-lookup"><span data-stu-id="f10b8-157">Give a name tooyour data source</span></span>
    <span data-ttu-id="f10b8-158">Gazdagép</span><span class="sxs-lookup"><span data-stu-id="f10b8-158">Host</span></span>|<span data-ttu-id="f10b8-159">Írja be a következő kifejezést: &lt;HDInsightClusterName>.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="f10b8-159">Enter &lt;HDInsightClusterName>.azurehdinsight.net.</span></span> <span data-ttu-id="f10b8-160">Például: sajatHDICluster.azurehdinsight.net</span><span class="sxs-lookup"><span data-stu-id="f10b8-160">For example, myHDICluster.azurehdinsight.net</span></span>
    <span data-ttu-id="f10b8-161">Port</span><span class="sxs-lookup"><span data-stu-id="f10b8-161">Port</span></span>|<span data-ttu-id="f10b8-162">Használja a <strong>443</strong> számú portot.</span><span class="sxs-lookup"><span data-stu-id="f10b8-162">Use <strong>443</strong>.</span></span> <span data-ttu-id="f10b8-163">(Ez a port módosult 563 too443.)</span><span class="sxs-lookup"><span data-stu-id="f10b8-163">(This port has been changed from 563 too443.)</span></span>
    <span data-ttu-id="f10b8-164">Adatbázis</span><span class="sxs-lookup"><span data-stu-id="f10b8-164">Database</span></span>|<span data-ttu-id="f10b8-165">Használja az <strong>Alapértelmezett</strong> adatbázist.</span><span class="sxs-lookup"><span data-stu-id="f10b8-165">Use <strong>Default</strong>.</span></span>
    <span data-ttu-id="f10b8-166">Hive Server típusa</span><span class="sxs-lookup"><span data-stu-id="f10b8-166">Hive Server Type</span></span>|<span data-ttu-id="f10b8-167">Válassza ki a <strong>Hive Server 2</strong> típust</span><span class="sxs-lookup"><span data-stu-id="f10b8-167">Select <strong>Hive Server 2</strong></span></span>
    <span data-ttu-id="f10b8-168">Mechanizmus</span><span class="sxs-lookup"><span data-stu-id="f10b8-168">Mechanism</span></span>|<span data-ttu-id="f10b8-169">Válassza ki az <strong>Azure HDInsight szolgáltatást</strong></span><span class="sxs-lookup"><span data-stu-id="f10b8-169">Select <strong>Azure HDInsight Service</strong></span></span>
    <span data-ttu-id="f10b8-170">HTTP elérési útja</span><span class="sxs-lookup"><span data-stu-id="f10b8-170">HTTP Path</span></span>|<span data-ttu-id="f10b8-171">Hagyja üresen.</span><span class="sxs-lookup"><span data-stu-id="f10b8-171">Leave it blank.</span></span>
    <span data-ttu-id="f10b8-172">Felhasználónév</span><span class="sxs-lookup"><span data-stu-id="f10b8-172">User Name</span></span>|<span data-ttu-id="f10b8-173">Adja meg hiveuser1@contoso158.onmicrosoft.com. Ha más, frissítse a hello tartomány nevét.</span><span class="sxs-lookup"><span data-stu-id="f10b8-173">Enter hiveuser1@contoso158.onmicrosoft.com. Update hello domain name if it is different.</span></span>
    <span data-ttu-id="f10b8-174">Jelszó</span><span class="sxs-lookup"><span data-stu-id="f10b8-174">Password</span></span>|<span data-ttu-id="f10b8-175">Adjon meg hiveuser1 hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="f10b8-175">Enter hello password for hiveuser1.</span></span>
    </table>

<span data-ttu-id="f10b8-176">Győződjön meg arról, hogy tooclick **teszt** hello adatforrás mentése előtt.</span><span class="sxs-lookup"><span data-stu-id="f10b8-176">Make sure tooclick **Test** before saving hello data source.</span></span>

## <a name="import-data-into-excel-from-hdinsight"></a><span data-ttu-id="f10b8-177">Adatok importálása Excel formátumba a HDInsight-ból</span><span class="sxs-lookup"><span data-stu-id="f10b8-177">Import data into Excel from HDInsight</span></span>
<span data-ttu-id="f10b8-178">Hello utolsó szakaszában konfigurálta a két házirendeket.</span><span class="sxs-lookup"><span data-stu-id="f10b8-178">In hello last section, you have configured two policies.</span></span>  <span data-ttu-id="f10b8-179">hiveuser1 hello válassza ki az összes hello oszlopához engedéllyel rendelkezik, és hiveuser2 rendelkezik engedéllyel a két olyan oszlopot válasszon hello.</span><span class="sxs-lookup"><span data-stu-id="f10b8-179">hiveuser1 has hello select permission on all hello columns, and hiveuser2 has hello select permission on two columns.</span></span> <span data-ttu-id="f10b8-180">Ebben a szakaszban hello két felhasználók tooimport adatokat Excelbe megszemélyesíteni.</span><span class="sxs-lookup"><span data-stu-id="f10b8-180">In this section, you impersonate hello two users tooimport data into Excel.</span></span>

1. <span data-ttu-id="f10b8-181">Nyisson meg egy új vagy egy meglévő munkafüzetet Excelben.</span><span class="sxs-lookup"><span data-stu-id="f10b8-181">Open a new or existing workbook in Excel.</span></span>
2. <span data-ttu-id="f10b8-182">A hello **adatok** lapra, majd **az egyéb adatforrások**, és kattintson a **az Adatkapcsolat varázsló** toolaunch hello **Adatkapcsolat varázsló**.</span><span class="sxs-lookup"><span data-stu-id="f10b8-182">From hello **Data** tab, click **From Other Data Sources**, and then click **From Data Connection Wizard** toolaunch hello **Data Connection Wizard**.</span></span>

    <span data-ttu-id="f10b8-183">![Adatkapcsolat varázsló megnyitása] [img-hdi-simbahiveodbc.excel.dataconnection]</span><span class="sxs-lookup"><span data-stu-id="f10b8-183">![Open data connection wizard][img-hdi-simbahiveodbc.excel.dataconnection]</span></span>
3. <span data-ttu-id="f10b8-184">Válassza ki **ODBC Adatforrásnevet** hello adatforrás lehetőséget, majd kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="f10b8-184">Select **ODBC DSN** as hello data source, and then click **Next**.</span></span>
4. <span data-ttu-id="f10b8-185">Az ODBC adatforrások, válassza hello adatok adatforrásnév hello előző lépésben létrehozott, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="f10b8-185">From ODBC data sources, select hello data source name that you created in hello previous step, and then  click **Next**.</span></span>
5. <span data-ttu-id="f10b8-186">Jelszó ismételt hello hello fürt hello varázslóban, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="f10b8-186">Re-enter hello password for hello cluster in hello wizard, and then click **OK**.</span></span> <span data-ttu-id="f10b8-187">Várjon, amíg hello **adatbázis és tábla kijelölése** párbeszédpanel tooopen.</span><span class="sxs-lookup"><span data-stu-id="f10b8-187">Wait for hello **Select Database and Table** dialog tooopen.</span></span> <span data-ttu-id="f10b8-188">Ez eltarthat néhány másodpercig.</span><span class="sxs-lookup"><span data-stu-id="f10b8-188">This can take a few seconds.</span></span>
6. <span data-ttu-id="f10b8-189">Válassza ki a **hivesampletable** táblát, majd kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="f10b8-189">Select **hivesampletable**, and then click **Next**.</span></span>
7. <span data-ttu-id="f10b8-190">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="f10b8-190">Click **Finish**.</span></span>
8. <span data-ttu-id="f10b8-191">A hello **és adatokat importálhat** párbeszédpanelen módosíthatja vagy hello lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="f10b8-191">In hello **Import Data** dialog, you can change or specify hello query.</span></span> <span data-ttu-id="f10b8-192">toodo kattintson **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="f10b8-192">toodo so, click **Properties**.</span></span> <span data-ttu-id="f10b8-193">Ez eltarthat néhány másodpercig.</span><span class="sxs-lookup"><span data-stu-id="f10b8-193">This can take a few seconds.</span></span>
9. <span data-ttu-id="f10b8-194">Kattintson a hello **Definition** külön-külön hello parancs szövege:</span><span class="sxs-lookup"><span data-stu-id="f10b8-194">Click hello **Definition** tab. hello command text is:</span></span>

       SELECT * FROM "HIVE"."default"."hivesampletable"

   <span data-ttu-id="f10b8-195">Hello Pletyka szabályzatok az Ön által definiált hiveuser1 válassza megfelelő jogosultságokkal rendelkezik az összes hello oszlop.</span><span class="sxs-lookup"><span data-stu-id="f10b8-195">By hello Ranger policies you defined,  hiveuser1 has select permission on all hello columns.</span></span>  <span data-ttu-id="f10b8-196">Ezért ez a lekérdezés hiveuser1 hitelesítő adataival működik, azonban hiveuser2 hitelesítő adataival nem működik.</span><span class="sxs-lookup"><span data-stu-id="f10b8-196">So this query works with hiveuser1's credentials, but this query does not not work with hiveuser2's credentials.</span></span>

   <span data-ttu-id="f10b8-197">![Kapcsolat tulajdonságai] [img-hdi-simbahiveodbc-excel-connectionproperties]</span><span class="sxs-lookup"><span data-stu-id="f10b8-197">![Connection Properties][img-hdi-simbahiveodbc-excel-connectionproperties]</span></span>
10. <span data-ttu-id="f10b8-198">Kattintson a **OK** tooclose hello kapcsolat tulajdonságai párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="f10b8-198">Click **OK** tooclose hello Connection Properties dialog.</span></span>
11. <span data-ttu-id="f10b8-199">Kattintson a **OK** tooclose hello **és adatokat importálhat** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f10b8-199">Click **OK** tooclose hello **Import Data** dialog.</span></span>  
12. <span data-ttu-id="f10b8-200">Írja be újból az hiveuser1 hello jelszót, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="f10b8-200">Reenter hello password for hiveuser1, and then click **OK**.</span></span> <span data-ttu-id="f10b8-201">Előtt adatokat lekérdezi az importált tooExcel néhány másodpercet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="f10b8-201">It takes a few seconds before data gets imported tooExcel.</span></span> <span data-ttu-id="f10b8-202">Amikor kész van, 11 adatoszlopnak kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="f10b8-202">When it is done, you shall see 11 columns of data.</span></span>

<span data-ttu-id="f10b8-203">hello utolsó szakaszában létrehozott tootest hello második házirend (olvasás-hivesampletable-devicemake)</span><span class="sxs-lookup"><span data-stu-id="f10b8-203">tootest hello second policy (read-hivesampletable-devicemake) you created in hello last section</span></span>

1. <span data-ttu-id="f10b8-204">Adjon hozzá egy új munkalapot az Excelben.</span><span class="sxs-lookup"><span data-stu-id="f10b8-204">Add a new sheet in Excel.</span></span>
2. <span data-ttu-id="f10b8-205">Hajtsa végre a hello utolsó eljárás tooimport hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="f10b8-205">Follow hello last procedure tooimport hello data.</span></span>  <span data-ttu-id="f10b8-206">hello mindössze annyi a változás teheti toouse hiveuser2 hitelesítő adatok helyett hiveuser1 tartozó.</span><span class="sxs-lookup"><span data-stu-id="f10b8-206">hello only change you will make is toouse hiveuser2's credentials instead of hiveuser1's.</span></span> <span data-ttu-id="f10b8-207">Ez sikertelen lesz, mert hiveuser2, csak a engedély toosee két oszlopot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="f10b8-207">This will fail because hiveuser2 only has permission toosee two columns.</span></span> <span data-ttu-id="f10b8-208">Hiba a következő hello kell beolvasása:</span><span class="sxs-lookup"><span data-stu-id="f10b8-208">You shall get hello following error:</span></span>

        [Microsoft][HiveODBC] (35) Error from Hive: error code: '40000' error message: 'Error while compiling statement: FAILED: HiveAccessControlException Permission denied: user [hiveuser2] does not have [SELECT] privilege on [default/hivesampletable/clientid,country ...]'.
3. <span data-ttu-id="f10b8-209">Hajtsa végre az azonos hello eljárás tooimport adatokat.</span><span class="sxs-lookup"><span data-stu-id="f10b8-209">Follow hello same procedure tooimport data.</span></span> <span data-ttu-id="f10b8-210">Ennek az időnek hiveuser2 tartozó hitelesítő adatokat használja, és hello select utasítás a is módosíthatók:</span><span class="sxs-lookup"><span data-stu-id="f10b8-210">This time, use hiveuser2's credentials, and also modify hello select statement from:</span></span>

        SELECT * FROM "HIVE"."default"."hivesampletable"

    <span data-ttu-id="f10b8-211">erre:</span><span class="sxs-lookup"><span data-stu-id="f10b8-211">to:</span></span>

        SELECT clientid, devicemake FROM "HIVE"."default"."hivesampletable"

    <span data-ttu-id="f10b8-212">Amikor kész van, két importáltadat-oszlopnak kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="f10b8-212">When it is done, you shall see two columns of data imported.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f10b8-213">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f10b8-213">Next steps</span></span>
* <span data-ttu-id="f10b8-214">A tartományhoz csatlakoztatott HDInsight-fürtök konfigurálásához lásd: [Tartományhoz csatlakoztatott HDInsight-fürtök konfigurálása](hdinsight-domain-joined-configure.md).</span><span class="sxs-lookup"><span data-stu-id="f10b8-214">For configuring a Domain-joined HDInsight cluster, see [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md).</span></span>
* <span data-ttu-id="f10b8-215">A tartományhoz csatlakoztatott HDInsight-fürtök kezeléséhhez lásd: [Tartományhoz csatlakoztatott HDInsight-fürtök kezelése](hdinsight-domain-joined-manage.md).</span><span class="sxs-lookup"><span data-stu-id="f10b8-215">For managing a Domain-joined HDInsight clusters, see [Manage Domain-joined HDInsight clusters](hdinsight-domain-joined-manage.md).</span></span>
* <span data-ttu-id="f10b8-216">Az SSH használata a tartományhoz csatlakoztatott HDInsight-fürtökön Hive-lekérdezéseket futtat, tekintse meg a [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).</span><span class="sxs-lookup"><span data-stu-id="f10b8-216">For running Hive queries using SSH on Domain-joined HDInsight clusters, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).</span></span>
* <span data-ttu-id="f10b8-217">Csatlakozás Hive Hive JDBC használatával, lásd: [tooHive on Azure HDInsight Hive JDBC-illesztőt hello használata csatlakozáshoz](hdinsight-connect-hive-jdbc-driver.md)</span><span class="sxs-lookup"><span data-stu-id="f10b8-217">For Connecting Hive using Hive JDBC, see [Connect tooHive on Azure HDInsight using hello Hive JDBC driver](hdinsight-connect-hive-jdbc-driver.md)</span></span>
* <span data-ttu-id="f10b8-218">Csatlakozás Excel tooHadoop Hive ODBC használatával, lásd: [kapcsolódás Excel tooHadoop a Microsoft Hive ODBC-meghajtó hello](hdinsight-connect-excel-hive-odbc-driver.md)</span><span class="sxs-lookup"><span data-stu-id="f10b8-218">For connecting Excel tooHadoop using Hive ODBC, see [Connect Excel tooHadoop with hello Microsoft Hive ODBC drive](hdinsight-connect-excel-hive-odbc-driver.md)</span></span>
* <span data-ttu-id="f10b8-219">Csatlakozás a Power Query használatával Excel tooHadoop, lásd: [kapcsolódás Excel tooHadoop Power Query használatával](hdinsight-connect-excel-power-query.md)</span><span class="sxs-lookup"><span data-stu-id="f10b8-219">For connecting Excel tooHadoop using Power Query, see [Connect Excel tooHadoop by using Power Query](hdinsight-connect-excel-power-query.md)</span></span>
