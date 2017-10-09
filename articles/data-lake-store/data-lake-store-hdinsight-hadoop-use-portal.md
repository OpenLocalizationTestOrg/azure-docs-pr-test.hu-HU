---
title: "a Data Lake Store aaaUse hello Azure portál toocreate Azure HDInsight-fürtök |} Microsoft Docs"
description: "Az Azure portál toocreate hello igénybe, valamint a HDInsight-fürtök az Azure Data Lake Store"
services: data-lake-store,hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: a8c45a83-a8e3-4227-8b02-1bc1e1de6767
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/14/2017
ms.author: nitinme
ms.openlocfilehash: f23113d444a3c5a01894dba29f75f3621b2d16bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-hdinsight-clusters-with-data-lake-store-by-using-hello-azure-portal"></a>A HDInsight-fürtök létrehozása a Data Lake Store hello Azure-portál használatával
> [!div class="op_single_selector"]
> * [Hello Azure portál használata](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [A PowerShell (az alapértelmezett tároló)](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [Használja a Powershellt (tárhely)](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Erőforrás-kezelő használata](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

Ismerje meg, hogyan toouse hello Azure portál toocreate egy HDInsight-fürtöt az Azure Data Lake Store-fiók hello alapértelmezett tároló vagy egy további tárterületet. Annak ellenére, hogy a további tárhely nem kötelező megadni egy HDInsight-fürt, akkor ajánlott toostore hello további tárfiókokat az üzleti adatokat.

## <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag megkezdése előtt győződjön meg arról, hogy teljesítette-követelményeknek hello:

* **Azure-előfizetés**. Nyissa meg túl[beolvasása az Azure ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/).
* **Egy Azure Data Lake Store-fiók**. Kövesse az utasításokat hello [hello Azure-portál használatával Ismerkedés az Azure Data Lake Store](data-lake-store-get-started-portal.md). Hello fiók is létre kell hoznia egy legfelső szintű mappát.  Ebben az oktatóanyagban egy legfelső szintű mappa neve a __/fürtök__ szolgál.
* **Egy Azure Active Directory szolgáltatás egyszerű**. Ez az oktatóanyag útmutatás toocreate egy egyszerű szolgáltatást az Azure Active Directory (Azure AD). Azonban toocreate egy egyszerű szolgáltatást, kell lennie az Azure AD-rendszergazda. Ha Ön rendszergazda, akkor hagyja ki ezt az előfeltételt, és folytassa a hello oktatóanyag.

    >[!NOTE]
    >Létrehozhat egy szolgáltatás egyszerű csak akkor, ha az Azure AD-rendszergazdaként. Az Azure AD rendszergazdának létre kell hoznia egy szolgáltatás egyszerű HDInsight-fürtök létrehozása a Data Lake Store előtt. Emellett hello szolgáltatás egyszerű kell létrehozni a tanúsítvánnyal részben ismertetett módon [hozzon létre egy egyszerű tanúsítvány](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-self-signed-certificate).
    >

## <a name="create-an-hdinsight-cluster"></a>HDInsight-fürtök létrehozása

Ebben a szakaszban egy HDInsight-fürt hello alapértelmezett, vagy további tárterület hello Data Lake Store-fiókok létrehozása. Ez a cikk csak a Data Lake Store-fiókok konfigurálása hello része foglalkozik.  Hello általános fürt létrehozása információkat és eljárásokat, [Hadoop létrehozása a HDInsight-fürtök](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md).

### <a name="create-a-cluster-with-data-lake-store-as-default-storage"></a>Fürt létrehozása a Data Lake Store alapértelmezett tárolóként

**toocreate egy HDInsight-fürtöt egy Data Lake Store hello alapértelmezett tárolási fiók**

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hajtsa végre a [fürtöket létrehozni](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md#create-clusters) hello általános információk a HDInsight-fürtök létrehozása.
3. A hello **tárolási** panelen, a **elsődleges tárolótípus**, jelölje be **Data Lake Store**, majd adja meg a következő információ hello:

    ![Hozzáadás szolgáltatás egyszerű tooHDInsight fürt](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.1.adls.storage.png "Hozzáadás szolgáltatás egyszerű tooHDInsight fürt")

    - **Válassza ki Data Lake Store-fiók**: Válasszon egy meglévő Data Lake Store-fiókot. Egy meglévő Data Lake Store-fiókot kell megadni.  Lásd: [Előfeltételek](#prereuisites).
    - **Elérési útjának gyökeréhez**: Adjon meg útvonalat, ahol hello fürt-specifikus fájlok toobe tárolja. Hello képernyőképe, a rendszer __/fürtök/myhdiadlcluster/__, mely hello a __/fürtök__ mappának léteznie kell, és hello portál létrehoz egyet *myhdicluster* mappa.  Hello *myhdicluster* hello fürtnév.
    - **Data Lake Store hozzáférés**: hello Data Lake Store-fiók és a HDInsight-fürt közötti elérésének konfigurálása. Útmutatásért lásd: [konfigurálása Data Lake Store hozzáférés](#configure-data-lake-store-access).
    - **További tárfiókok**: hello fürt fiókok hozzáadása Azure Storage-fiókokról további tárolóként. tooadd további Data Lake tárolók hello fürt engedélyeket ad a további Data Lake Store-fiók adatainak hello elsődleges tárolási típus, egy Data Lake Store-fiók konfigurálása során végezhető el. Lásd: [A Data Lake Store-hoz történő hozzáférés konfigurálása](#configure-data-lake-store-access).

4. A hello **Data Lake Store hozzáférés**, kattintson **kiválasztása**, majd folytassa a fürt létrehozása a [Hadoop létrehozása a HDInsight-fürtök](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md).


### <a name="create-a-cluster-with-data-lake-store-as-additional-storage"></a>A fürt létrehozása a Data Lake Store további tárhely

hello alábbi utasítások alapján hozzon létre egy HDInsight-fürtöt egy Azure Storage-fiók hello alapértelmezett tárolóként, és további tárterületként egy Data Lake Store-fiókot.
**toocreate egy HDInsight-fürtöt egy Data Lake Store hello alapértelmezett tárolási fiók**

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hajtsa végre a [fürtöket létrehozni](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md#create-clusters) hello általános információk a HDInsight-fürtök létrehozása.
3. A hello **tárolási** panelen, a **elsődleges tárolótípus**, jelölje be **Azure Storage**, majd adja meg a következő információ hello:

    ![Hozzáadás szolgáltatás egyszerű tooHDInsight fürt](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.1.png "Hozzáadás szolgáltatás egyszerű tooHDInsight fürt")

    - **Kijelöléséről**: hello a következő lehetőségek valamelyikével:

        * toospecify egy tárfiókot, amely része az Azure-előfizetéshez, válasszon **a saját előfizetések**, majd válassza ki a tárfiók hello.
        * toospecify kívül az Azure-előfizetéshez, jelölje be a tárfiók **hozzáférési kulcs**, majd adja meg a tárfiók kívül hello hello adatait.

    - **Alapértelmezett tároló**: vagy hello alapértelmezett értéket használja, vagy adja meg saját nevét.

    - További tárfiókok: hello további tárhely további Azure Storage-fiókokat hozzáadni.
    - Data Lake Store hozzáférés: hello Data Lake Store-fiók és a HDInsight-fürt közötti elérésének konfigurálása. Útmutatásért lásd: [konfigurálása Data Lake Store hozzáférés](#configure-data-lake-store-access).

## <a name="configure-data-lake-store-access"></a>Data Lake Store-hozzáférés konfigurálása 

Ebben a szakaszban egy Azure Active Directory szolgáltatás egyszerű HDInsight-fürtök a Data Lake Store-hozzáférés konfigurálásához. 

### <a name="specify-a-service-principal"></a>Adjon meg egy szolgáltatásnevet

Az Azure-portálon hello használjon egy meglévő egyszerű szolgáltatást, vagy hozzon létre egy újat.

**toocreate egy egyszerű szolgáltatást hello Azure-portálon**

1. Kattintson a **Data Lake Store hozzáférés** hello Store panelen.
2. A hello **Data Lake Store hozzáférés** panelen kattintson a **hozzon létre új**.
3. Kattintson a **egyszerű**, és kövesse a hello utasításokat toocreate egy egyszerű szolgáltatást.
4. Hello tanúsítvány letöltése, ha úgy dönt, hogy toouse azt újra a jövőbeli hello. Letöltés hello tanúsítvány akkor hasznos, ha azt szeretné, hogy toouse hello azonos szolgáltatás egyszerű, amikor további HDInsight-fürtök létrehozásához.

    ![Hozzáadás szolgáltatás egyszerű tooHDInsight fürt](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.2.png "Hozzáadás szolgáltatás egyszerű tooHDInsight fürt")

4. Kattintson a **hozzáférés** tooconfigure hello mappákhoz való hozzáférést.  Lásd: [fájl engedélyeit](#configure-file-permissions).


**toouse egy meglévő egyszerű szolgáltatásnév a hello Azure-portálon**

1. Kattintson a **Data Lake Store hozzáférés**.
1. A hello **Data Lake Store hozzáférés** panelen kattintson a **meglévő**.
2. Kattintson a **egyszerű**, majd válassza ki a szolgáltatásnevet. 
3. Töltse fel a kijelölt szolgáltatás egyszerű társított hello tanúsítvány (.pfx-fájl), és írja be a hello tanúsítvány jelszavát.

    ![Hozzáadás szolgáltatás egyszerű tooHDInsight fürt](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.5.png "Hozzáadás szolgáltatás egyszerű tooHDInsight fürt")

4. Kattintson a **hozzáférés** tooconfigure hello mappákhoz való hozzáférést.  Lásd: [fájl engedélyeit](#configure-file-permissions).


### <a name="configure-file-permissions"></a>Fájl-engedélyek konfigurálása

hello konfigurálása eltér attól függően, hogy hello fiókjával hello alapértelmezett tároló vagy egy további storage-fiókot:

- Alapértelmezett tárolójaként használni kívánt

    - a gyökérszinten hello hello Data Lake Store-fiókot az engedélyt
    - engedély gyökérszinten hello a hello HDInsight-fürt tárolására. Például hello __/fürtök__ hello az oktatóanyag korábbi részében használt mappát.
- További tárterületként használata

    - A hello mappákat, ahol kell fájl hozzáférési engedélyt.

**a Data Lake Store-fiók gyökérszinten hello tooassign engedély**

1. A hello **Data Lake Store hozzáférés** panelen kattintson a **hozzáférés**. Hello **válassza ki a fájl engedélyeit** panel már meg van nyitva. Felsorolja az összes hello Data Lake Store-fiók az előfizetéshez.
2. Vigye (ne kattintson) hello egér keresztül hello hello toomake hello jelölőnégyzet látható, majd válassza ki a jelölőnégyzetet hello Data Lake Store-fiók nevét.

    ![Hozzáadás szolgáltatás egyszerű tooHDInsight fürt](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3.png "Hozzáadás szolgáltatás egyszerű tooHDInsight fürt")

  Alapértelmezés szerint __OLVASÁSI__, __írási__, és __EXECUTE__ lehetőségek egyaránt be vannak jelölve.

3. Kattintson a **válasszon** a hello hello lap alján.
4. Kattintson a **futtatása** tooassign engedéllyel.
5. Kattintson a **Done** (Kész) gombra.

**a HDInsight fürt gyökérszinten hello tooassign engedély**

1. A hello **Data Lake Store hozzáférés** panelen kattintson a **hozzáférés**. Hello **válassza ki a fájl engedélyeit** panel már meg van nyitva. Felsorolja az összes hello Data Lake Store-fiók az előfizetéshez.
1. A hello **válassza ki a fájl engedélyeit** paneljén kattintson hello Data Lake Store nevét tooshow benne lévő tartalom.
2. Jelölje ki a hello HDInsight fürt tárológyökérhez hello bal oldali hello mappa hello jelölőnégyzet bejelölésével. Hello fürt tárológyökérhez van toohello képernyőképe alapján történik a korábbi, __/fürtök__ alapértelmezett tárolóként hello Data Lake Store kijelölés közben megadott mappában.
3. Hello mappa hello engedélyeket.  Alapértelmezés szerint olvasási, írási és végrehajtási lehetőségek egyaránt be vannak jelölve.
4. Kattintson a **válasszon** a hello hello lap alján.
5. Kattintson a **Run** (Futtatás) parancsra.
6. Kattintson a **Done** (Kész) gombra.

Ha a Data Lake Store további tárolóként használ, hozzá kell rendelnie engedély csak hello mappák, amelyet a HDInsight-fürt hello tooaccess. Például az alábbi hello képernyőképet, kívánja megadni a hozzáférést csak túl**hdiaddonstorage** mappát a Data Lake Store-fiók.

![Rendelje hozzá a szolgáltatás egyszerű engedélyek toohello HDInsight-fürt](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3-1.png "hozzárendelése szolgáltatás egyszerű engedélyek toohello HDInsight-fürt")


## <a name="verify-cluster-set-up"></a>Ellenőrizze a fürt beállítása

Hello fürt telepítés befejezése után, a hello fürt paneljén, ellenőrizze az eredményeket valamelyike vagy mindegyike hello lépések végrehajtásával:

* a társított tárolási hello hello fürt hello Data Lake Store-fiók, amely a megadott érték, kattintson a tooverify **Storage-fiókok** hello bal oldali ablaktáblán.

    ![Hozzáadás szolgáltatás egyszerű tooHDInsight fürt](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.6-1.png "Hozzáadás szolgáltatás egyszerű tooHDInsight fürt")

* amely a szolgáltatás egyszerű hello tooverify tartozik megfelelő hello HDInsight-fürt, kattintson a **Data Lake Store hozzáférés** hello bal oldali ablaktáblán.

    ![Hozzáadás szolgáltatás egyszerű tooHDInsight fürt](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.6.png "Hozzáadás szolgáltatás egyszerű tooHDInsight fürt")


## <a name="examples"></a>Példák

Után a tárolására állította be a Data Lake Store hello fürt, tekintse meg a toothese példák hogyan toouse HDInsight fürt tooanalyze hello Data Lake Store-ban tárolt adatokat.

### <a name="run-a-hive-query-against-data-in-a-data-lake-store-as-primary-storage"></a>Hive-lekérdezések futtatása egy Data Lake Store-ban adatok alapján (elsődleges tárolóként)

Hive-lekérdezések toorun hello Hive nézetek illesztőfelületet hello Ambari portálon használja. Hogyan toouse Ambari Hive megtekinti, lásd: [használata hello a HDInsight Hadoop Hive nézet](../hdinsight/hdinsight-hadoop-use-hive-ambari-view.md).

Ha az egy Data Lake Store-adatokkal dolgozik, van néhány karakterláncok toochange.

Ha használ, például a Data Lake Store elsődleges tárolóként létrehozott hello fürt hello elérési toohello adatok nem: *adl: / / < data_lake_store_account_name > /azuredatalakestore.net/path/to/file*. A Hive lekérdezés toocreate hello Data Lake Store-fiókban tárolt mintaadatokat egy táblát a következőképpen néz hello utasítás a következő:

    CREATE EXTERNAL TABLE websitelog (str string) LOCATION 'adl://hdiadlsstorage.azuredatalakestore.net/clusters/myhdiadlcluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/'

Leírása:
* `adl://hdiadlstorage.azuredatalakestore.net/`hello hello Data Lake Store-fiók gyökérkönyvtárában van.
* `/clusters/myhdiadlcluster`hello fürt adatok hello fürt létrehozásakor megadott hello gyökérkönyvtárában van.
* `/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/`az hello hely hello minta fájl hello lekérdezésben használt.

### <a name="run-a-hive-query-against-data-in-a-data-lake-store-as-additional-storage"></a>Hive-lekérdezések futtatása egy Data Lake Store-ban adatok alapján (a további tárhely)

Ha létrehozott hello fürt Blob-tároló alapértelmezett tárolóként használ, hello mintaadatok nem szerepel hello további tárolóként használt Azure Data Lake Store-fiókot. Ebben az esetben először hello adatok átviteléhez a Blob storage toohello Data Lake Store, és futtassa újra hello lekérdezések, ahogy az előző példa hello.

Hogyan toocopy adatokat a Blob storage tooa Data Lake tárolja a további információkért lásd: a következő cikkek hello:

* [Azure Storage-BLOB és a Data Lake Store ból a Distcp toocopy adatok használata](data-lake-store-copy-data-wasb-distcp.md)
* [A használandó AdlCopy toocopy adatok Azure Storage-blobok tooData Lake Store](data-lake-store-copy-data-azure-storage-blob.md)

### <a name="use-data-lake-store-with-a-spark-cluster"></a>Használjon Data Lake Store egy Spark-fürt
Egy Spark-fürt toorun Spark feladatok egy Data Lake Store-ban tárolt adatokat is használhatja. További információkért lásd: [használata a HDInsight Spark fürt tooanalyze adatok a Data Lake Store](../hdinsight/hdinsight-apache-spark-use-with-data-lake-store.md).


### <a name="use-data-lake-store-in-a-storm-topology"></a>Használjon Data Lake Store a Storm-topológia
A Storm-topológia a hello Data Lake Store toowrite adatokat is használhatja. Útmutatást tooachieve ebben az esetben lásd: [használata Azure Data Lake Store a HDInsight alatt futó Apache Storm](../hdinsight/hdinsight-storm-write-data-lake-store.md).

## <a name="see-also"></a>Lásd még:
* [PowerShell: Hozzon létre egy HDInsight-fürt toouse Data Lake Store](data-lake-store-hdinsight-hadoop-use-powershell.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
