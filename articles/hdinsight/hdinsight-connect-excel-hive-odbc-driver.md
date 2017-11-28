---
title: "az Azure HDInsight Hive ODBC-illesztőprogram - hello aaaConnect Excel tooHadoop |} Microsoft Docs"
description: "Ismerje meg, hogyan tooset fel és -felhasználási hello Excel tooquery adatainak a HDInsight-fürtök a Microsoft Excel a Microsoft Hive ODBC-illesztőprogram."
keywords: hadoop az excel, a hive az excel, a hive odbc
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
tags: azure-portal
editor: cgronlun
ms.assetid: a7665a14-0211-4521-b3e7-3b26e8029cc0
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/22/2017
ms.author: jgao
ms.openlocfilehash: f01f89e7d4203c739d56079dc589fc11f4aa2174
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-excel-toohadoop-in-azure-hdinsight-with-hello-microsoft-hive-odbc-driver"></a><span data-ttu-id="159fe-104">Csatlakozás az Azure HDInsight Excel tooHadoop hello Microsoft Hive ODBC-illesztőprogram</span><span class="sxs-lookup"><span data-stu-id="159fe-104">Connect Excel tooHadoop in Azure HDInsight with hello Microsoft Hive ODBC driver</span></span>

[!INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

<span data-ttu-id="159fe-105">A Microsoft Big Data típusú adatok megoldás integrálódik a Microsoft üzleti Üzletiintelligencia-összetevők az Apache Hadoop-fürtök hello Azure HDInsight által alkalmazott.</span><span class="sxs-lookup"><span data-stu-id="159fe-105">Microsoft's Big Data solution integrates Microsoft Business Intelligence (BI) components with Apache Hadoop clusters that have been deployed by hello Azure HDInsight.</span></span> <span data-ttu-id="159fe-106">Ez az integráció például az hello képességét tooconnect Excel toohello Hive data warehouse a Microsoft Hive megnyitott adatbázis kapcsolat (ODBC) illesztőprogram hello használata a hdinsight Hadoop-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="159fe-106">An example of this integration is hello ability tooconnect Excel toohello Hive data warehouse of a Hadoop cluster in HDInsight using hello Microsoft Hive Open Database Connectivity (ODBC) Driver.</span></span>

<span data-ttu-id="159fe-107">Emellett a HDInsight-fürtök és más adatforrások lehetséges tooconnect hello adatait, beleértve más (nem-HDInsight) Hadoop-fürtök, az Excel alkalmazásban, hello Microsoft Power Query beépülő modul az Excel.</span><span class="sxs-lookup"><span data-stu-id="159fe-107">It is also possible tooconnect hello data associated with an HDInsight cluster and other data sources, including other (non-HDInsight) Hadoop clusters, from Excel using hello Microsoft Power Query add-in for Excel.</span></span> <span data-ttu-id="159fe-108">Telepítés és a Power Query használatával kapcsolatos tudnivalókat lásd: [Power Query az Excel csatlakozás tooHDInsight][hdinsight-power-query].</span><span class="sxs-lookup"><span data-stu-id="159fe-108">For information on installing and using Power Query, see [Connect Excel tooHDInsight with Power Query][hdinsight-power-query].</span></span>

> [!NOTE]
> <span data-ttu-id="159fe-109">Hello szükséges lépések során ez a cikk vagy a Linux használható, vagy Windows-alapú HDInsight-fürtöt, a Windows hello ügyfél munkaállomás szükséges.</span><span class="sxs-lookup"><span data-stu-id="159fe-109">While hello steps in this article can be used with either a Linux or Windows-based HDInsight cluster, Windows is required for hello client workstation.</span></span>
> 
> 

<span data-ttu-id="159fe-110">**Előfeltételek**:</span><span class="sxs-lookup"><span data-stu-id="159fe-110">**Prerequisites**:</span></span>

<span data-ttu-id="159fe-111">Ez a cikk megkezdése előtt rendelkeznie kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="159fe-111">Before you begin this article, you must have hello following items:</span></span>

* <span data-ttu-id="159fe-112">**HDInsight-fürtök**.</span><span class="sxs-lookup"><span data-stu-id="159fe-112">**An HDInsight cluster**.</span></span> <span data-ttu-id="159fe-113">toocreate, lásd: [Ismerkedés az Azure HDInsight][hdinsight-get-started].</span><span class="sxs-lookup"><span data-stu-id="159fe-113">toocreate one, see [Get started with Azure HDInsight][hdinsight-get-started].</span></span>
* <span data-ttu-id="159fe-114">**A munkaállomás** Office 2013 Professional Plus, Office 365 Pro Plus, az Excel 2013 önálló vagy Office 2010 Professional Plus.</span><span class="sxs-lookup"><span data-stu-id="159fe-114">**A workstation** with Office 2013 Professional Plus, Office 365 Pro Plus, Excel 2013 Standalone, or Office 2010 Professional Plus.</span></span>

## <a name="install-microsoft-hive-odbc-driver"></a><span data-ttu-id="159fe-115">Telepítse a Microsoft Hive ODBC-illesztőprogram</span><span class="sxs-lookup"><span data-stu-id="159fe-115">Install Microsoft Hive ODBC driver</span></span>
<span data-ttu-id="159fe-116">Töltse le és telepítse a Microsoft Hive ODBC-illesztő hello [letöltőközpontból][hive-odbc-driver-download].</span><span class="sxs-lookup"><span data-stu-id="159fe-116">Download and install Microsoft Hive ODBC Driver from hello [Download Center][hive-odbc-driver-download].</span></span>

<span data-ttu-id="159fe-117">Az illesztőprogram telepíthető Windows 7, Windows 8, Windows 10, Windows Server 2008 R2 és Windows Server 2012 32 bites vagy 64 bites verzióit.</span><span class="sxs-lookup"><span data-stu-id="159fe-117">This driver can be installed on 32-bit or 64-bit versions of Windows 7, Windows 8, Windows 10, Windows Server 2008 R2, and Windows Server 2012.</span></span> <span data-ttu-id="159fe-118">hello illesztőprogram lehetővé teszi, hogy a kapcsolat tooAzure HDInsight (version 1.6 és újabb verziók) és az Azure HDInsight emulátoron (v.1.0.0.0 és újabb).</span><span class="sxs-lookup"><span data-stu-id="159fe-118">hello driver allows connection tooAzure HDInsight (version 1.6 and later) and Azure HDInsight Emulator (v.1.0.0.0 and later).</span></span> <span data-ttu-id="159fe-119">Hello verziója, ahol hello ODBC-illesztőprogram használ hello alkalmazás hello verziójának megfelelő kell telepítenie.</span><span class="sxs-lookup"><span data-stu-id="159fe-119">You shall install hello version that matches hello version of hello application where you use hello ODBC driver.</span></span> <span data-ttu-id="159fe-120">Ebben az oktatóanyagban a Office Excel hello-illesztőprogram van alkalmazva.</span><span class="sxs-lookup"><span data-stu-id="159fe-120">For this tutorial, hello driver is used from Office Excel.</span></span>

## <a name="create-hive-odbc-data-source"></a><span data-ttu-id="159fe-121">Hive ODBC-adatforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="159fe-121">Create Hive ODBC data source</span></span>
<span data-ttu-id="159fe-122">hello következő lépések bemutatják, hogyan toocreate egy Hive ODBC-adatforrás.</span><span class="sxs-lookup"><span data-stu-id="159fe-122">hello following steps show you how toocreate a Hive ODBC Data Source.</span></span>

1. <span data-ttu-id="159fe-123">A Windows 8 vagy Windows 10, nyomja le az ENTER hello Windows kulcs tooopen hello kezdőképernyőn, és írja be **adatforrások**.</span><span class="sxs-lookup"><span data-stu-id="159fe-123">From Windows 8 or Windows 10, press hello Windows key tooopen hello Start screen, and then type **data sources**.</span></span>
2. <span data-ttu-id="159fe-124">Kattintson a **ODBC adatforrások (32 bites) beállítása** vagy **ODBC adatforrások (64 bites) beállítása** az Office verziójától függően.</span><span class="sxs-lookup"><span data-stu-id="159fe-124">Click **Set up ODBC Data sources (32-bit)** or **Set up ODBC Data Sources (64-bit)** depending on your Office version.</span></span> <span data-ttu-id="159fe-125">Ha Windows 7 használ, válassza a **ODBC adatforrások (32 bites)** vagy **ODBC adatforrások (64 bites)** a **felügyeleti eszközök**.</span><span class="sxs-lookup"><span data-stu-id="159fe-125">If you are using Windows 7, choose **ODBC Data Sources (32 bit)** or **ODBC Data Sources (64 bit)** from **Administrative Tools**.</span></span> <span data-ttu-id="159fe-126">Ekkor megjelenik a hello **ODBC-adatforrás felügyelő** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="159fe-126">You shall see hello **ODBC Data Source Administrator** dialog.</span></span>
   
    <span data-ttu-id="159fe-127">![Adatforrás-felügyelő OBDC](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png "adatforrás-felügyelő ODBC Adatforrásnevet konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="159fe-127">![OBDC data source administrator](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png "Configure a DSN using ODBC Data Source Administrator")</span></span>

3. <span data-ttu-id="159fe-128">A felhasználó DNS-ből, kattintson a **Hozzáadás** tooopen hello **új adatforrás létrehozása** varázsló.</span><span class="sxs-lookup"><span data-stu-id="159fe-128">From User DNS, click **Add** tooopen hello **Create New Data Source** wizard.</span></span>
4. <span data-ttu-id="159fe-129">Válassza ki **Microsoft Hive ODBC-illesztő**, és kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="159fe-129">Select **Microsoft Hive ODBC Driver**, and then click **Finish**.</span></span> <span data-ttu-id="159fe-130">Ekkor megjelenik a hello **Microsoft Hive ODBC illesztőprogram DNS beállítások** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="159fe-130">You shall see hello **Microsoft Hive ODBC Driver DNS Setup** dialog.</span></span>
5. <span data-ttu-id="159fe-131">Írja be vagy válassza ki a következő értékek hello:</span><span class="sxs-lookup"><span data-stu-id="159fe-131">Type or select hello following values:</span></span>
   
   | <span data-ttu-id="159fe-132">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="159fe-132">Property</span></span> | <span data-ttu-id="159fe-133">Leírás</span><span class="sxs-lookup"><span data-stu-id="159fe-133">Description</span></span> |
   | --- | --- |
   |  <span data-ttu-id="159fe-134">Adatforrás neve</span><span class="sxs-lookup"><span data-stu-id="159fe-134">Data Source Name</span></span> |<span data-ttu-id="159fe-135">Adjon nevet tooyour adatforrás</span><span class="sxs-lookup"><span data-stu-id="159fe-135">Give a name tooyour data source</span></span> |
   |  <span data-ttu-id="159fe-136">Gazdagép</span><span class="sxs-lookup"><span data-stu-id="159fe-136">Host</span></span> |<span data-ttu-id="159fe-137">Írja be a következő kifejezést: &lt;HDInsightClusterName>.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="159fe-137">Enter &lt;HDInsightClusterName>.azurehdinsight.net.</span></span> <span data-ttu-id="159fe-138">Például: sajatHDICluster.azurehdinsight.net</span><span class="sxs-lookup"><span data-stu-id="159fe-138">For example, myHDICluster.azurehdinsight.net</span></span> |
   |  <span data-ttu-id="159fe-139">Port</span><span class="sxs-lookup"><span data-stu-id="159fe-139">Port</span></span> |<span data-ttu-id="159fe-140">Használja a <strong>443</strong> számú portot.</span><span class="sxs-lookup"><span data-stu-id="159fe-140">Use <strong>443</strong>.</span></span> <span data-ttu-id="159fe-141">(Ez a port módosult 563 too443.)</span><span class="sxs-lookup"><span data-stu-id="159fe-141">(This port has been changed from 563 too443.)</span></span> |
   |  <span data-ttu-id="159fe-142">Adatbázis</span><span class="sxs-lookup"><span data-stu-id="159fe-142">Database</span></span> |<span data-ttu-id="159fe-143">Használja az <strong>Alapértelmezett</strong> adatbázist.</span><span class="sxs-lookup"><span data-stu-id="159fe-143">Use <strong>Default</strong>.</span></span> |
   |  <span data-ttu-id="159fe-144">Mechanizmus</span><span class="sxs-lookup"><span data-stu-id="159fe-144">Mechanism</span></span> |<span data-ttu-id="159fe-145">Válassza ki az <strong>Azure HDInsight szolgáltatást</strong></span><span class="sxs-lookup"><span data-stu-id="159fe-145">Select <strong>Azure HDInsight Service</strong></span></span> |
   |  <span data-ttu-id="159fe-146">Felhasználónév</span><span class="sxs-lookup"><span data-stu-id="159fe-146">User Name</span></span> |<span data-ttu-id="159fe-147">Adja meg a HDInsight fürt HTTP felhasználó felhasználóneve.</span><span class="sxs-lookup"><span data-stu-id="159fe-147">Enter HDInsight cluster HTTP user username.</span></span> <span data-ttu-id="159fe-148">hello alapértelmezett felhasználónév az <strong>admin</strong>.</span><span class="sxs-lookup"><span data-stu-id="159fe-148">hello default username is <strong>admin</strong>.</span></span> |
   |  <span data-ttu-id="159fe-149">Jelszó</span><span class="sxs-lookup"><span data-stu-id="159fe-149">Password</span></span> |<span data-ttu-id="159fe-150">A HDInsight fürt felhasználói jelszó.</span><span class="sxs-lookup"><span data-stu-id="159fe-150">Enter HDInsight cluster user password.</span></span> |
   
    </table>
   
    <span data-ttu-id="159fe-151">Nincsenek elemre tudomást néhány fontos paraméterek toobe **speciális beállítások**:</span><span class="sxs-lookup"><span data-stu-id="159fe-151">There are some important parameters toobe aware of when you click **Advanced Options**:</span></span>
   
   | <span data-ttu-id="159fe-152">Paraméter</span><span class="sxs-lookup"><span data-stu-id="159fe-152">Parameter</span></span> | <span data-ttu-id="159fe-153">Leírás</span><span class="sxs-lookup"><span data-stu-id="159fe-153">Description</span></span> |
   | --- | --- |
   |  <span data-ttu-id="159fe-154">Natív lekérdezés használata</span><span class="sxs-lookup"><span data-stu-id="159fe-154">Use Native Query</span></span> |<span data-ttu-id="159fe-155">Ha be van jelölve, hello ODBC illesztőprogram nem próbálja tooconvert TSQL HiveQL be.</span><span class="sxs-lookup"><span data-stu-id="159fe-155">When it is selected, hello ODBC driver does NOT try tooconvert TSQL into HiveQL.</span></span> <span data-ttu-id="159fe-156">Azt kell használni, csak ha 100 %-os meg arról, hogy a tiszta HiveQL utasítások küld.</span><span class="sxs-lookup"><span data-stu-id="159fe-156">You shall use it only if you are 100% sure you are submitting pure HiveQL statements.</span></span> <span data-ttu-id="159fe-157">Ha tooSQL Server vagy az Azure SQL Database, akkor hagyja bejelölve.</span><span class="sxs-lookup"><span data-stu-id="159fe-157">When connecting tooSQL Server or Azure SQL Database, you should leave it unchecked.</span></span> |
   |  <span data-ttu-id="159fe-158">/ Blokk lehívott sorok száma</span><span class="sxs-lookup"><span data-stu-id="159fe-158">Rows fetched per block</span></span> |<span data-ttu-id="159fe-159">A rekordok nagy száma beolvasni, ez a paraméter finomhangolás válhat szükséges tooensure optimális teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="159fe-159">When fetching a large number of records, tuning this parameter may be required tooensure optimal performances.</span></span> |
   |  <span data-ttu-id="159fe-160">Alapértelmezett karakterlánc oszlop hossza, a bináris oszlop, decimális oszlop méretezési</span><span class="sxs-lookup"><span data-stu-id="159fe-160">Default string column length, Binary column length, Decimal column scale</span></span> |<span data-ttu-id="159fe-161">hello adattípus hosszak és a szükséges hatással lehet, hogy adatokat ad vissza.</span><span class="sxs-lookup"><span data-stu-id="159fe-161">hello data type lengths and precisions may affect how data is returned.</span></span> <span data-ttu-id="159fe-162">Helytelen információt toobe miatt visszaadott okoznak tooloss pontosság és/vagy csonkolása.</span><span class="sxs-lookup"><span data-stu-id="159fe-162">They cause incorrect information toobe returned due tooloss of precision and/or truncation.</span></span> |

    <span data-ttu-id="159fe-163">![Speciális beállítások](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png "DSN speciális konfigurációs beállítások")</span><span class="sxs-lookup"><span data-stu-id="159fe-163">![Advanced options](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png "Advanced DSN configuration options")</span></span>

1. <span data-ttu-id="159fe-164">Kattintson a **teszt** tootest hello adatforrás.</span><span class="sxs-lookup"><span data-stu-id="159fe-164">Click **Test** tootest hello data source.</span></span> <span data-ttu-id="159fe-165">Hello adatforrás megfelelően van konfigurálva, mutat *teszt sikeresen befejeződött!*.</span><span class="sxs-lookup"><span data-stu-id="159fe-165">When hello data source is configured correctly, it shows *TESTS COMPLETED SUCCESSFULLY!*.</span></span>
2. <span data-ttu-id="159fe-166">Kattintson a **OK** tooclose hello teszt párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="159fe-166">Click **OK** tooclose hello Test dialog.</span></span> <span data-ttu-id="159fe-167">Új adatforrás hello fel kell sorolni hello **ODBC-adatforrás felügyelő**.</span><span class="sxs-lookup"><span data-stu-id="159fe-167">hello new data source shall be listed on hello **ODBC Data Source Administrator**.</span></span>
3. <span data-ttu-id="159fe-168">Kattintson a **OK** tooexit hello varázsló.</span><span class="sxs-lookup"><span data-stu-id="159fe-168">Click **OK** tooexit hello wizard.</span></span>

## <a name="import-data-into-excel-from-hdinsight"></a><span data-ttu-id="159fe-169">Adatok importálása Excel formátumba a HDInsight-ból</span><span class="sxs-lookup"><span data-stu-id="159fe-169">Import data into Excel from HDInsight</span></span>
<span data-ttu-id="159fe-170">hello következő lépések azt mutatják be hello módon tooimport adatait Hive tábla használatával hello ODBC-adatforrás hello előző szakaszban létrehozott Excel-munkafüzetbe.</span><span class="sxs-lookup"><span data-stu-id="159fe-170">hello following steps describe hello way tooimport data from a Hive table into an Excel workbook using hello ODBC data source that you created in hello previous section.</span></span>

1. <span data-ttu-id="159fe-171">Nyisson meg egy új vagy egy meglévő munkafüzetet Excelben.</span><span class="sxs-lookup"><span data-stu-id="159fe-171">Open a new or existing workbook in Excel.</span></span>
2. <span data-ttu-id="159fe-172">A hello **adatok** lapra, majd **adatok beolvasása**, kattintson a **egyéb forrásokból származó**, és kattintson a **az ODBC** toolaunch hello  **Adatkapcsolat varázsló**.</span><span class="sxs-lookup"><span data-stu-id="159fe-172">From hello **Data** tab, click **Get Data**, click **From Other Sources**, and then click **From ODBC** toolaunch hello **Data Connection Wizard**.</span></span>
   
    <span data-ttu-id="159fe-173">![Nyissa meg az Adatkapcsolat varázsló](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png "megnyitott kapcsolat varázsló")</span><span class="sxs-lookup"><span data-stu-id="159fe-173">![Open data connection wizard](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png "Open data connection wizard")</span></span>
4. <span data-ttu-id="159fe-174">Jelölje be hello adatok adatforrásnév hello utolsó szakaszában létrehozott, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="159fe-174">Select hello data source name that you created in hello last section, and then click **OK**.</span></span>
5. <span data-ttu-id="159fe-175">Adja meg a Hadoop-felhasználónevet (hello alapértelmezés szerint ez admin) és hello jelszót, majd **Connect**.</span><span class="sxs-lookup"><span data-stu-id="159fe-175">Enter Hadoop user name (hello default name is admin) and hello password, and then click **Connect**.</span></span>
6. <span data-ttu-id="159fe-176">A Navigátor, bontsa ki **HIVE**, bontsa ki a **alapértelmezett**, kattintson **hivesampletable**, és kattintson a **terhelés**.</span><span class="sxs-lookup"><span data-stu-id="159fe-176">On Navigator, expand **HIVE**, expand **default**, click **hivesampletable**, and then click **Load**.</span></span> <span data-ttu-id="159fe-177">Előtt adatokat lekérdezi az importált tooExcel néhány másodpercet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="159fe-177">It takes a few seconds before data gets imported tooExcel.</span></span>

    <span data-ttu-id="159fe-178">![HDInsight Hive ODBC navigator](./media/hdinsight-connect-excel-hive-ODBC-driver/hdinsight.hive.odbc.navigator.png "megnyitott kapcsolat varázsló")</span><span class="sxs-lookup"><span data-stu-id="159fe-178">![HDInsight Hive ODBC navigator](./media/hdinsight-connect-excel-hive-ODBC-driver/hdinsight.hive.odbc.navigator.png "Open data connection wizard")</span></span>


## <a name="next-steps"></a><span data-ttu-id="159fe-179">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="159fe-179">Next steps</span></span>
<span data-ttu-id="159fe-180">Ebben a cikkben megtanulta, hogyan toouse hello Excelbe Microsoft Hive ODBC illesztőprogram tooretrieve hello HDInsight szolgáltatás adatait.</span><span class="sxs-lookup"><span data-stu-id="159fe-180">In this article, you learned how toouse hello Microsoft Hive ODBC driver tooretrieve data from hello HDInsight Service into Excel.</span></span> <span data-ttu-id="159fe-181">Ehhez hasonlóan beolvasható adat hello HDInsight-szolgáltatás az SQL-adatbázisba.</span><span class="sxs-lookup"><span data-stu-id="159fe-181">Similarly, you can retrieve data from hello HDInsight Service into SQL Database.</span></span> <span data-ttu-id="159fe-182">Akkor is lehetséges tooupload adatok egy HDInsight-szolgáltatásba.</span><span class="sxs-lookup"><span data-stu-id="159fe-182">It is also possible tooupload data into an HDInsight Service.</span></span> <span data-ttu-id="159fe-183">toolearn több, lásd:</span><span class="sxs-lookup"><span data-stu-id="159fe-183">toolearn more, see:</span></span>

* <span data-ttu-id="159fe-184">[HDInsight eszközzel repülési késleltetés adatok elemzése][hdinsight-analyze-flight-data]</span><span class="sxs-lookup"><span data-stu-id="159fe-184">[Analyze flight delay data using HDInsight][hdinsight-analyze-flight-data]</span></span>
* <span data-ttu-id="159fe-185">[Adatok tooHDInsight feltöltése][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="159fe-185">[Upload Data tooHDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="159fe-186">[Use Sqoop with HDInsight][hdinsight-use-sqoop]</span><span class="sxs-lookup"><span data-stu-id="159fe-186">[Use Sqoop with HDInsight][hdinsight-use-sqoop]</span></span>

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-get-started]: hdinsight-hadoop-tutorial-get-started-windows.md

[hive-odbc-driver-download]: http://go.microsoft.com/fwlink/?LinkID=286698


