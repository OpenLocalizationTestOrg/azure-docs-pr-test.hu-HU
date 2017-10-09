---
title: "aaaGet használatába a HDInsight - Azure HBase például |} Microsoft Docs"
description: "Hajtsa végre az Apache HBase példa toostart a HDInsight hadoop használatával. A HBase rendszerhéjjal hello táblák létrehozása, és lekérdezheti Hive eszközzel."
keywords: "hbasecommand,hbase példa"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 4d6a2658-6b19-4268-95ee-822890f5a33a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: jgao
ms.openlocfilehash: 43419780142b320b16180a2b1f25020dee2f7a11
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-an-apache-hbase-example-in-hdinsight"></a>Bevezetés a HDInsight egy Apache HBase-példájába

Ismerje meg, hogyan toocreate a HDInsight HBase-fürtöt létre HBase táblákat és lekérdezéstáblákat a Hive. A HBase-re vonatkozó általános információért lásd: [HDInsight HBase overview][hdinsight-hbase-overview] (A HDInsight HBase áttekintése).

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a>Előfeltételek
A HBase példa megkísérlése előtt a következő elemek hello kell rendelkeznie:

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* [Biztonságos rendszerhéj (SSH)](hdinsight-hadoop-linux-use-ssh-unix.md). 
* [curl](http://curl.haxx.se/download.html).

## <a name="create-hbase-cluster"></a>HBase-fürt létrehozása
hello alábbi eljárás egy Azure Resource Manager sablon toocreate 3.4-es verziójú Linux-alapú HBase fürt és hello függő alapértelmezett Azure Storage-fiókot használ. hello eljárás és egyéb Fürtlétrehozási módszerek használt toounderstand hello paraméterek lásd [hdinsight létrehozása Linux-alapú Hadoop-fürtök](hdinsight-hadoop-provision-linux-clusters.md).

1. Kattintson a következő kép tooopen hello sablon hello Azure-portálon hello. hello sablon a következő nyilvános blobtárolóban található. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-linux%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy tooAzure"></a>
2. A hello **egyéni telepítési** panelen adja meg a következő értékek hello:
   
   * **Előfizetés**: válassza ki az Azure-előfizetéssel, amely használt toocreate hello fürt.
   * **Erőforráscsoport**: Hozzon létre egy Azure Resource Management-csoportot, vagy használjon egy meglévőt.
   * **Hely**: hello erőforráscsoport hello helyének megadása. 
   * **ClusterName**: Adjon meg egy nevet hello HBase-fürtöt.
   * **A fürt bejelentkezési nevet és jelszót**: hello alapértelmezett bejelentkezési név az **admin**.
   * **SSH-felhasználónév és jelszó**: hello alapértelmezett felhasználónév az **sshuser**.  Ezt át lehet nevezni.
     
     Más paraméterek opcionálisak.  
     
     Minden egyes fürt az Azure Storage-fióktól függ. Ha töröl egy fürtöt, hello adatok megmaradnak, hello tárfiókban. hello fürt alapértelmezett tárfiókneve hello fürt neve a "store" kifejezéssel kiegészítve. Szoftveresen kötött a sablonváltozók szakaszban hello.
3. Válassza ki **toohello feltételek és kikötések fenti elfogadom**, és kattintson a **beszerzési**. Körülbelül 20 percet toocreate fürt szükséges.

> [!NOTE]
> HBase-fürtök törlése után egy másik HBase-fürtöt hello használatával létrehozhat alapértelmezett blob tárolóhoz. Új fürt hello szerzi be hello eredeti fürtben létrehozott hello HBase táblákat. tooavoid inkonzisztenciákat, ajánlott letiltani hello HBase táblákat hello fürt törlése előtt.
> 
> 

## <a name="create-tables-and-insert-data"></a>Táblák létrehozása és adatok beszúrása
SSH tooconnect tooHBase fürtök használata, és használhatja a HBase rendszerhéjjal toocreate HBase táblákat, adatok, illetve adatait kérdezi le. További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

A legtöbbek számára adatokat hello táblázatos formátumban jelenik meg:

![HDInsight HBase táblázatos adatok][img-hbase-sample-data-tabular]

A HBase (BigTable megvalósítása), hello azonos adatok láthatóhoz hasonló:

![HDInsight HBase BigTable-adatok][img-hbase-sample-data-bigtable]


**toouse hello HBase rendszerhéj**

1. Az SSH-ból futtassa a következő HBase parancs hello:
   
    ```bash
    hbase shell
    ```

2. Hozzon létre egy HBase-t kétoszlopos családokkal:

    ```hbaseshell   
    create 'Contacts', 'Personal', 'Office'
    list
    ```
3. Adatok beszúrása:
    
    ```hbaseshell   
    put 'Contacts', '1000', 'Personal:Name', 'John Dole'
    put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
    put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
    put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
    scan 'Contacts'
    ```
   
    ![HDInsight Hadoop HBase-rendszerhéj][img-hbase-shell]
4. Egyetlen sor lekérése
   
    ```hbaseshell
    get 'Contacts', '1000'
    ```
   
    Azonos eredmények hello látnak kell hello vizsgálat parancs használatával, mert csak egy sorból áll.
   
    Hello HBase táblasémát kapcsolatos további információkért lásd: [bemutatása tooHBase Schema Design][hbase-schema]. További Hbase-parancsokért lásd: [Apache HBase reference guide][hbase-quick-start] (Apache HBase referencia-útmutató).
5. Kilépés hello rendszerhéj
   
    ```hbaseshell
    exit
    ```

**toobulk adatok betöltése hello névjegyek HBase táblájába**

A HBase több módszert tartalmaz az adatok táblába töltéséhez.  További információ: [Bulk loading](http://hbase.apache.org/book.html#arch.bulk.load) (Kötegelt betöltés).

Egy minta adatfájl található a következő nyilvános blobtárolóban található: *wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt*.  hello hello adatfájl tartalma:

    8396    Calvin Raji      230-555-0191    230-555-0191    5415 San Gabriel Dr.
    16600   Karen Wu         646-555-0113    230-555-0192    9265 La Paz
    4324    Karl Xie         508-555-0163    230-555-0193    4912 La Vuelta
    16891   Jonn Jackson     674-555-0110    230-555-0194    40 Ellis St.
    3273    Miguel Miller    397-555-0155    230-555-0195    6696 Anchor Drive
    3588    Osa Agbonile     592-555-0152    230-555-0196    1873 Lion Circle
    10272   Julia Lee        870-555-0110    230-555-0197    3148 Rose Street
    4868    Jose Hayes       599-555-0171    230-555-0198    793 Crawford Street
    4761    Caleb Alexander  670-555-0141    230-555-0199    4775 Kentucky Dr.
    16443   Terry Chander    998-555-0171    230-555-0200    771 Northridge Drive

Lehetősége van hozzon létre egy szövegfájlt, és töltse fel a hello fájl tooyour saját tárfiók. Hello útmutatásért lásd: [feltölteni az adatokat a HDInsight Hadoop-feladatok][hdinsight-upload-data].

> [!NOTE]
> Ez az eljárás során létrehozott az utolsó eljárás hello hello Contacts HBase táblát használja.
> 

1. Az SSH-ból futtassa a következő parancs tootransform hello tooStoreFiles fájlt, és a Dimporttsv.bulk.output által meghatározott relatív elérési úton tárolja hello.  Ha a HBase rendszerhéjban, használja a hello kilépési parancs tooexit.

    ```bash   
    hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name,Personal:Phone,Office:Phone,Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt
    ```

2. Futtassa a következő parancs tooupload hello adatok /example/data/storeDataFileOutput toohello HBase tábla hello:
   
    ```bash
    hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts
    ```

3. Nyissa meg a HBase rendszerhéjjal hello és hello vizsgálat parancs toolist hello tábla tartalmát használja.

## <a name="use-hive-tooquery-hbase"></a>Hive tooquery HBase használata

A HBase táblákban lévő adatokat a Hive eszközzel kérdezheti le. Ebben a szakaszban hoz létre olyan Hive táblát, amely leképezhető toohello HBase tábla és a HBase táblában tooquery hello adatokat használja.

1. Nyissa meg **PuTTY**, és csatlakoztassa toohello fürtöt.  Lásd: hello hello az előző eljárás utasításait.
2. Hello SSH-munkamenetet használja a következő parancs toostart Beeline hello:

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin
    ```

    A Beeline-nal kapcsolatos további információkért lásd [a Hive és a Hadoop együttes, a Beeline-nal történő használatát a HDInsightban](hdinsight-hadoop-use-hive-beeline.md) ismertető cikket.
       
3. Futtassa a következő HiveQL-parancsfájlt toocreate hello olyan Hive táblát, amely leképezhető toohello HBase tábla. Győződjön meg arról, hogy létrehozta hello HBase rendszerhéj használatával, az utasítás futtatása előtt az oktatóanyag korábbi részében hivatkozott mintatáblát hello.

    ```hiveql   
    CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
    STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
    WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
    TBLPROPERTIES ('hbase.table.name' = 'Contacts');
    ```

4. Futtassa a következő HiveQL parancsfájl tooquery hello adatok hello HBase tábla hello:

    ```hiveql   
    SELECT count(rowkey) FROM hbasecontacts;
    ```

## <a name="use-hbase-rest-apis-using-curl"></a>HBase REST API-k használata Curl használatával

hello REST API védelméről [az egyszerű hitelesítés](http://en.wikipedia.org/wiki/Basic_access_authentication). Kérések mindig biztonságos HTTP (HTTPS) toohelp győződjön meg arról, hogy a hitelesítő adatait biztonságos módon küldje toohello server kell végezzen.

2. A következő parancs toolist hello meglévő HBase táblák hello használata:

    ```bash
    curl -u <UserName>:<Password> \
    -G https://<ClusterName>.azurehdinsight.net/hbaserest/
    ```

3. A következő parancs toocreate egy új HBase tábla két oszlopcsaláddal hello használata:

    ```bash   
    curl -u <UserName>:<Password> \
    -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/schema" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d "{\"@name\":\"Contact1\",\"ColumnSchema\":[{\"name\":\"Personal\"},{\"name\":\"Office\"}]}" \
    -v
    ```

    hello séma hello JSon formátumban van megadva.
4. Használja a következő parancs tooinsert hello néhány adat:

    ```bash   
    curl -u <UserName>:<Password> \
    -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/false-row-key" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d "{\"Row\":[{\"key\":\"MTAwMA==\",\"Cell\": [{\"column\":\"UGVyc29uYWw6TmFtZQ==\", \"$\":\"Sm9obiBEb2xl\"}]}]}" \
    -v
    ```
   
    Meg kell, hogy base64 kódolása hello -d kapcsolón megadott hello értékeket. Hello példa:
   
   * MTAwMA==: 1000
   * UGVyc29uYWw6TmFtZQ==: Personal:Name
   * Sm9obiBEb2xl: John Dole
     
     [hamis sorkulcsa](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/rest/package-summary.html#operation_cell_store_single) tooinsert lehetővé teszi több (kötegelt) értéket.
5. A következő parancs tooget sor hello használata:
   
    ```bash 
    curl -u <UserName>:<Password> \
    -X GET "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/1000" \
    -H "Accept: application/json" \
    -v
    ```

További információ a HBase REST-ről: [Apache HBase Reference Guide](https://hbase.apache.org/book.html#_rest) (Apache HBase referencia-útmutató).

> [!NOTE]
> A HBase nem támogatja a Thriftet a HDInsightban.
>
> Használata Curl vagy más REST kommunikációt használ a Webhcattel, hitelesítenie kell hello kérelmek hello felhasználónevet és jelszót hello HDInsight fürt rendszergazdája által. Hello egységes erőforrás-azonosító (URI) részeként használt toosend hello kérelmek toohello server hello fürtnév is kell használnia:
> 
>   
>        curl -u <UserName>:<Password> \
>        -G https://<ClusterName>.azurehdinsight.net/templeton/v1/status
>   
>    Egy hasonló toohello választ, a következő választ kell kapnia:
>   
>        {"status":"ok","version":"v1"}
   


## <a name="check-cluster-status"></a>A fürt állapotának ellenőrzése
A HBase a HDInsightban a fürtök megfigyelésére szolgáló webes felhasználói felülettel kapható. Hello webes felhasználói felület használatával, kérhet statisztikáit vagy információit régiók.

**tooaccess hello HBase fő felhasználói felület**

1. Jelentkezzen be a hello hello Ambari webes felhasználói felületén a https://&lt;Clustername >. azurehdinsight.net.
2. Kattintson a **HBase** hello bal oldali menüből.
3. Kattintson a **Gyorshivatkozások** hello hello oldal, pont toohello a Zookeeper csomópont hivatkozás aktív, és kattintson a **HBase fő felhasználói felület**.  felhasználói felület hello meg van nyitva egy másik böngészőben lapon:

  ![HDInsight HBase HMaster felhasználói felülete](./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-hmaster-ui.png)

  hello HBase fő felhasználói felülete a következő szakaszok hello tartalmazza:

  - régiós kiszolgálók
  - biztonsági mentési főkiszolgálók
  - táblák
  - feladatok
  - szoftverattribútumok

## <a name="delete-hello-cluster"></a>Hello fürt törlése
tooavoid inkonzisztenciákat, ajánlott letiltani hello HBase táblákat hello fürt törlése előtt.

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Hibaelhárítás

Ha problémába ütközik a HDInsight-fürtök létrehozása során, tekintse meg [a hozzáférés-vezérlésre vonatkozó követelményeket](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Következő lépések
Ebben a cikkben megtanulta, hogyan toocreate HBase-fürtöt, és hogyan toocreate tábla és nézet hello ezen táblák adatait hello HBase rendszerhéjból. Is megtanulta, hogyan toouse a struktúra a HBase táblákat, és hogyan toouse hello HBase C# REST API-k toocreate lévő adatok lekérdezése egy HBase tábla és hello tábla adatainak lekérése.

toolearn több, lásd:

* [HDInsight HBase overview][hdinsight-hbase-overview] (A HDInsight HBase áttekintése): A HBase egy Apache, nyílt forráskódú, a Hadoopra épülő NoSQL-adatbázis, amely véletlenszerű hozzáférést és erős konzisztenciát biztosít a nagy mennyiségű strukturálatlan és félig strukturált adatok számára.

[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hbase-reference]: http://hbase.apache.org/book.html#importtsv
[hbase-schema]: http://0b4af6cdc2f0c5998459-c0245c5c937c5dedcca3f1764ecc9b2f.r43.cf2.rackcdn.com/9353-login1210_khurana.pdf
[hbase-quick-start]: http://hbase.apache.org/book.html#quickstart





[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-versions]: hdinsight-component-versioning.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-portal]: https://portal.azure.com/
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-quick-create.png
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-hive-editor.png
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-file-browser.png
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-bigtable.png
