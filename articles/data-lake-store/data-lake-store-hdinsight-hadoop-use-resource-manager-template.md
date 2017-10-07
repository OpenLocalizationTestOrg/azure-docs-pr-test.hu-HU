---
title: "Azure-sablonok toocreate aaaUse HDInsight és a Data Lake Store |} Microsoft Docs"
description: "Azure Resource Manager sablonok toocreate igénybe, valamint a HDInsight-fürtök az Azure Data Lake Store"
services: data-lake-store,hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 8ef8152f-2121-461e-956c-51c55144919d
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/04/2017
ms.author: nitinme
ms.openlocfilehash: eb88a626f2837dcc29295f3f73a91757059c3bb8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-hdinsight-cluster-with-data-lake-store-using-azure-resource-manager-template"></a>HDInsight-fürtök létrehozása a Data Lake Store Azure Resource Manager-sablonnal
> [!div class="op_single_selector"]
> * [A Portal használata](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [PowerShell használatával (az alapértelmezett tároló)](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [(Tárhely) a PowerShell használatával](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Erőforrás-kezelő használatával](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

Ismerje meg, hogyan toouse Azure PowerShell tooconfigure egy HDInsight-fürtöt az Azure Data Lake Store **további tárolóként**.

Támogatott fürttípusok Data Lake Store alapértelmezett tároló vagy további tárfiókot kell használni. Ha a Data Lake Store további tárterületet, hello alapértelmezett tárfiók hello fürtök továbbra is Azure Storage Blobs (WASB), és hello fürt kapcsolatos fájlokhoz (például naplói, stb.) továbbra is írt toohello alapértelmezett tárolási hello adatainak közben, amikor szeretné, hogy tooprocess tárolható Data Lake Store-fiók. További tárhely fiókként használatával a Data Lake Store nem befolyásolja a teljesítményt és a hello képességét tooread/írás toohello tárolást hello fürtből.

## <a name="using-data-lake-store-for-hdinsight-cluster-storage"></a>Data Lake Store használata a HDInsight-fürt tárolására

Az alábbiakban a HDInsight a Data Lake Store használatára vonatkozó szempontokat:

* Beállítás toocreate a HDInsight-fürtök hozzáféréssel rendelkező tooData Lake Store alapértelmezett tárolóként esetén HDInsight 3.5-ös és 3.6 verzió érhető el.

* Beállítás toocreate a HDInsight-fürtök hozzáféréssel rendelkező tooData Lake Store további tárolóként verzióihoz áll rendelkezésre HDInsight 3.2-es, 3.4, 3.5-ös és 3.6.

Ebben a cikkben azt a Data Lake Store a Hadoop fürtök a további tárolóként kell kiépíteni. Hogyan toocreate egy Hadoop-fürtöt Data Lake Store alapértelmezett tárolóként, lásd: [HDInsight-fürtök létrehozása az Azure portál használatával a Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).

## <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag elkezdéséhez hello következő kell rendelkeznie:

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).
* Az **Azure PowerShell 1.0-s vagy újabb verziója**. Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).
* **Az Azure Active Directory szolgáltatás egyszerű**. Ez az oktatóanyag lépéseit ad útmutatást toocreate egy egyszerű Azure AD-ben. Azonban az Azure AD rendszergazdai toobe képes toocreate szolgáltatásnevet kell lennie. Ha az Azure AD-rendszergazdaként, hagyja ki ezt az előfeltételt, és hello oktatóanyag folytatásához.

    **Ha nem az Azure AD-rendszergazda**, nem fogja tudni tooperform hello lépéseket szükséges toocreate egy egyszerű szolgáltatást. Ebben az esetben az Azure AD-rendszergazda először létre kell hoznia egy egyszerű szolgáltatást a Data Lake Store egy HDInsight-fürt létrehozása előtt. Emellett hello szolgáltatás egyszerű segítségével kell létrehozni egy tanúsítványt, részben ismertetett módon [hozzon létre egy egyszerű tanúsítvány](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).

## <a name="create-an-hdinsight-cluster-with-azure-data-lake-store"></a>HDInsight-fürtök létrehozása az Azure Data Lake Store
hello Resource Manager-sablon, és hello előfeltételei a hello sablon legyenek elérhetők a Githubon: [az új Data Lake Store HDInsight Linux fürt központi telepítése](https://github.com/Azure/azure-quickstart-templates/tree/master/201-hdinsight-datalake-store-azure-storage). Kövesse hello megjelenő utasításokat, ez a hivatkozás toocreate HDInsight-fürtök az Azure Data Lake Store hello további tárolóként.

hello található utasítások segítségével: hello hivatkozásra a fenti PowerShell igényelnek. Ezeket az utasításokat a Kezdés előtt győződjön meg arról tooyour Azure-fiók a bejelentkezéskor. Az asztalon nyisson meg egy új Azure PowerShell-ablakot, és adja meg az alábbi kódtöredékek hello. Amikor felszólító toolog, győződjön meg arról, hogy jelentkezik be a rendelkezésre álló hello előfizetés rendszergazdájaként/tulajdonosaként:

```
# Log in tooyour Azure account
Login-AzureRmAccount

# List all hello subscriptions associated tooyour account
Get-AzureRmSubscription

# Select a subscription
Set-AzureRmContext -SubscriptionId <subscription ID>
```

## <a name="upload-sample-data-toohello-azure-data-lake-store"></a>A minta adatok toohello Azure Data Lake Store feltöltése
hello Resource Manager-sablon létrehoz egy új Data Lake Store-fiókot, és társítja azt hello HDInsight-fürthöz. Néhány példa adatok toohello Data Lake Store most kell feltöltenie. Ezeket az adatokat később a hello oktatóanyag toorun feladatok a HDInsight-fürtöt, amely a Data Lake Store hello adatelérés lesz szüksége. Útmutatást tooupload adatokat, lásd: [feltöltése egy Data Lake Store fájl tooyour](data-lake-store-get-started-portal.md#uploaddata). Néhány példa adatok tooupload keres, ha kaphat a hello **Ambulance Data** hello mappát [Azure Data Lake Git-tárház](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData).

## <a name="set-relevant-acls-on-hello-sample-data"></a>Hello mintaadatok vonatkozó hozzáférés-vezérlési listák beállítása
toomake, hogy a HDInsight-fürt hello elérhető-e a feltöltött hello mintaadatok, gondoskodnia kell arról, hogy rendelkezik-e hozzáférési toohello fájl vagy mappa Ön hello Azure AD-alkalmazást, amely hello HDInsight-fürt és a Data Lake Store között használt tooestablish identitás Kísérlet történt tooaccess. toodo, hajtsa végre az alábbi lépésekkel hello.

1. Hello nevét hello HDInsight-fürthöz társított Azure AD-alkalmazást és a Data Lake Store hello található. Egyirányú toolook hello neve tooopen hello HDInsight fürt paneljén hello Resource Manager sablonnal létrehozott, kattintson a hello **fürt AAD-identitása** lapra, és keressen a hello értékének **egyszerű szolgáltatásnév Megjelenítendő név**.
2. Most biztosítanak hozzáférést toothis az Azure AD-alkalmazást hello mappára vagy fájlra, amelyet a HDInsight-fürt hello tooaccess. tooset hello jobb hozzáférés-vezérlési listák hello fájlt vagy mappát a Data Lake Store, lásd: [adatok védelme az Data Lake Store](data-lake-store-secure-data.md#filepermissions).

## <a name="run-test-jobs-on-hello-hdinsight-cluster-toouse-hello-data-lake-store"></a>Hello HDInsight fürt toouse hello Data Lake Store a teszt feladatok futtatása
Miután konfigurálta a HDInsight-fürtöt, futtathatja Tesztfeladatok hello fürt tootest adott hello HDInsight fürt Data Lake Store férhetnek hozzá. toodo úgy, hogy fog futni egy minta Hive-feladatot, amely táblát hoz létre, hogy a korábbi tooyour Data Lake Store feltöltött hello mintaadatok használatával.

Ebben a szakaszban fogja SSH egy HDInsight Linux-fürt és a futási hello minta Hive-lekérdezések. Ha egy Windows ügyfél használ, azt javasoljuk, **PuTTY**, amely letölthető [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

A PuTTY használatával kapcsolatos további információkért lásd: [SSH használata a HDInsight Windows Linux-alapú Hadooppal ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

1. A csatlakozás után indítsa el a hello Hive CLI hello a következő parancs segítségével:

   ```
   hive
   ```
2. Hello CLI, adja meg a következő utasítások toocreate nevű új tábla hello **járművekről gyűjtött** hello mintaadatok használatával a Data Lake Store hello:

   ```
   DROP TABLE vehicles;
   CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestore>.azuredatalakestore.net:443/';
   SELECT * FROM vehicles LIMIT 10;
   ```

   Egy kimeneti hasonló toohello következő kell megjelennie:

   ```
   1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
   1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
   1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
   1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
   1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
   1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
   1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
   1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
   1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
   1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1
   ```


## <a name="access-data-lake-store-using-hdfs-commands"></a>Hozzáférés Data Lake Store HDFS parancs használatával
Miután konfigurálta a hello HDInsight fürt toouse Data Lake Store, hello HDFS rendszerhéj parancsok tooaccess hello tároló is használhatja.

Ebben a szakaszban fogja SSH egy HDInsight Linux-fürt és a futási hello HDFS parancs. Ha egy Windows ügyfél használ, azt javasoljuk, **PuTTY**, amely letölthető [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

A PuTTY használatával kapcsolatos további információkért lásd: [SSH használata a HDInsight Windows Linux-alapú Hadooppal ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

Miután csatlakozott, használja a következő HDFS hello filesystem parancs toolist fájlok hello Data Lake Store hello.

```
hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/
```

Hello fájlt, hogy a korábbi toohello Data Lake Store feltöltött megjelenik.

```
15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
Found 1 items
-rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder
```

Is használhatja a hello `hdfs dfs -put` tooupload bizonyos fájlok toohello Data Lake Store parancsot, és ezután `hdfs dfs -ls` tooverify e hello fájlok sikeresen feltöltve.


## <a name="next-steps"></a>Következő lépések
* [Adatok másolása az Azure Storage Blobs tooData Lake Store-ból](data-lake-store-copy-data-wasb-distcp.md)
