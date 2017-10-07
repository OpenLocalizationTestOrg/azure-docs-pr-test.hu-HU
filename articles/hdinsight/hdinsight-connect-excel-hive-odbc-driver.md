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
# <a name="connect-excel-toohadoop-in-azure-hdinsight-with-hello-microsoft-hive-odbc-driver"></a>Csatlakozás az Azure HDInsight Excel tooHadoop hello Microsoft Hive ODBC-illesztőprogram

[!INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

A Microsoft Big Data típusú adatok megoldás integrálódik a Microsoft üzleti Üzletiintelligencia-összetevők az Apache Hadoop-fürtök hello Azure HDInsight által alkalmazott. Ez az integráció például az hello képességét tooconnect Excel toohello Hive data warehouse a Microsoft Hive megnyitott adatbázis kapcsolat (ODBC) illesztőprogram hello használata a hdinsight Hadoop-fürthöz.

Emellett a HDInsight-fürtök és más adatforrások lehetséges tooconnect hello adatait, beleértve más (nem-HDInsight) Hadoop-fürtök, az Excel alkalmazásban, hello Microsoft Power Query beépülő modul az Excel. Telepítés és a Power Query használatával kapcsolatos tudnivalókat lásd: [Power Query az Excel csatlakozás tooHDInsight][hdinsight-power-query].

> [!NOTE]
> Hello szükséges lépések során ez a cikk vagy a Linux használható, vagy Windows-alapú HDInsight-fürtöt, a Windows hello ügyfél munkaállomás szükséges.
> 
> 

**Előfeltételek**:

Ez a cikk megkezdése előtt rendelkeznie kell a következő elemek hello:

* **HDInsight-fürtök**. toocreate, lásd: [Ismerkedés az Azure HDInsight][hdinsight-get-started].
* **A munkaállomás** Office 2013 Professional Plus, Office 365 Pro Plus, az Excel 2013 önálló vagy Office 2010 Professional Plus.

## <a name="install-microsoft-hive-odbc-driver"></a>Telepítse a Microsoft Hive ODBC-illesztőprogram
Töltse le és telepítse a Microsoft Hive ODBC-illesztő hello [letöltőközpontból][hive-odbc-driver-download].

Az illesztőprogram telepíthető Windows 7, Windows 8, Windows 10, Windows Server 2008 R2 és Windows Server 2012 32 bites vagy 64 bites verzióit. hello illesztőprogram lehetővé teszi, hogy a kapcsolat tooAzure HDInsight (version 1.6 és újabb verziók) és az Azure HDInsight emulátoron (v.1.0.0.0 és újabb). Hello verziója, ahol hello ODBC-illesztőprogram használ hello alkalmazás hello verziójának megfelelő kell telepítenie. Ebben az oktatóanyagban a Office Excel hello-illesztőprogram van alkalmazva.

## <a name="create-hive-odbc-data-source"></a>Hive ODBC-adatforrás létrehozása
hello következő lépések bemutatják, hogyan toocreate egy Hive ODBC-adatforrás.

1. A Windows 8 vagy Windows 10, nyomja le az ENTER hello Windows kulcs tooopen hello kezdőképernyőn, és írja be **adatforrások**.
2. Kattintson a **ODBC adatforrások (32 bites) beállítása** vagy **ODBC adatforrások (64 bites) beállítása** az Office verziójától függően. Ha Windows 7 használ, válassza a **ODBC adatforrások (32 bites)** vagy **ODBC adatforrások (64 bites)** a **felügyeleti eszközök**. Ekkor megjelenik a hello **ODBC-adatforrás felügyelő** párbeszédpanel.
   
    ![Adatforrás-felügyelő OBDC](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png "adatforrás-felügyelő ODBC Adatforrásnevet konfigurálása")

3. A felhasználó DNS-ből, kattintson a **Hozzáadás** tooopen hello **új adatforrás létrehozása** varázsló.
4. Válassza ki **Microsoft Hive ODBC-illesztő**, és kattintson a **Befejezés**. Ekkor megjelenik a hello **Microsoft Hive ODBC illesztőprogram DNS beállítások** párbeszédpanel.
5. Írja be vagy válassza ki a következő értékek hello:
   
   | Tulajdonság | Leírás |
   | --- | --- |
   |  Adatforrás neve |Adjon nevet tooyour adatforrás |
   |  Gazdagép |Írja be a következő kifejezést: &lt;HDInsightClusterName>.azurehdinsight.net. Például: sajatHDICluster.azurehdinsight.net |
   |  Port |Használja a <strong>443</strong> számú portot. (Ez a port módosult 563 too443.) |
   |  Adatbázis |Használja az <strong>Alapértelmezett</strong> adatbázist. |
   |  Mechanizmus |Válassza ki az <strong>Azure HDInsight szolgáltatást</strong> |
   |  Felhasználónév |Adja meg a HDInsight fürt HTTP felhasználó felhasználóneve. hello alapértelmezett felhasználónév az <strong>admin</strong>. |
   |  Jelszó |A HDInsight fürt felhasználói jelszó. |
   
    </table>
   
    Nincsenek elemre tudomást néhány fontos paraméterek toobe **speciális beállítások**:
   
   | Paraméter | Leírás |
   | --- | --- |
   |  Natív lekérdezés használata |Ha be van jelölve, hello ODBC illesztőprogram nem próbálja tooconvert TSQL HiveQL be. Azt kell használni, csak ha 100 %-os meg arról, hogy a tiszta HiveQL utasítások küld. Ha tooSQL Server vagy az Azure SQL Database, akkor hagyja bejelölve. |
   |  / Blokk lehívott sorok száma |A rekordok nagy száma beolvasni, ez a paraméter finomhangolás válhat szükséges tooensure optimális teljesítményét. |
   |  Alapértelmezett karakterlánc oszlop hossza, a bináris oszlop, decimális oszlop méretezési |hello adattípus hosszak és a szükséges hatással lehet, hogy adatokat ad vissza. Helytelen információt toobe miatt visszaadott okoznak tooloss pontosság és/vagy csonkolása. |

    ![Speciális beállítások](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png "DSN speciális konfigurációs beállítások")

1. Kattintson a **teszt** tootest hello adatforrás. Hello adatforrás megfelelően van konfigurálva, mutat *teszt sikeresen befejeződött!*.
2. Kattintson a **OK** tooclose hello teszt párbeszédpanel. Új adatforrás hello fel kell sorolni hello **ODBC-adatforrás felügyelő**.
3. Kattintson a **OK** tooexit hello varázsló.

## <a name="import-data-into-excel-from-hdinsight"></a>Adatok importálása Excel formátumba a HDInsight-ból
hello következő lépések azt mutatják be hello módon tooimport adatait Hive tábla használatával hello ODBC-adatforrás hello előző szakaszban létrehozott Excel-munkafüzetbe.

1. Nyisson meg egy új vagy egy meglévő munkafüzetet Excelben.
2. A hello **adatok** lapra, majd **adatok beolvasása**, kattintson a **egyéb forrásokból származó**, és kattintson a **az ODBC** toolaunch hello  **Adatkapcsolat varázsló**.
   
    ![Nyissa meg az Adatkapcsolat varázsló](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png "megnyitott kapcsolat varázsló")
4. Jelölje be hello adatok adatforrásnév hello utolsó szakaszában létrehozott, és kattintson **OK**.
5. Adja meg a Hadoop-felhasználónevet (hello alapértelmezés szerint ez admin) és hello jelszót, majd **Connect**.
6. A Navigátor, bontsa ki **HIVE**, bontsa ki a **alapértelmezett**, kattintson **hivesampletable**, és kattintson a **terhelés**. Előtt adatokat lekérdezi az importált tooExcel néhány másodpercet vesz igénybe.

    ![HDInsight Hive ODBC navigator](./media/hdinsight-connect-excel-hive-ODBC-driver/hdinsight.hive.odbc.navigator.png "megnyitott kapcsolat varázsló")


## <a name="next-steps"></a>Következő lépések
Ebben a cikkben megtanulta, hogyan toouse hello Excelbe Microsoft Hive ODBC illesztőprogram tooretrieve hello HDInsight szolgáltatás adatait. Ehhez hasonlóan beolvasható adat hello HDInsight-szolgáltatás az SQL-adatbázisba. Akkor is lehetséges tooupload adatok egy HDInsight-szolgáltatásba. toolearn több, lásd:

* [HDInsight eszközzel repülési késleltetés adatok elemzése][hdinsight-analyze-flight-data]
* [Adatok tooHDInsight feltöltése][hdinsight-upload-data]
* [Use Sqoop with HDInsight][hdinsight-use-sqoop]

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-get-started]: hdinsight-hadoop-tutorial-get-started-windows.md

[hive-odbc-driver-download]: http://go.microsoft.com/fwlink/?LinkID=286698


