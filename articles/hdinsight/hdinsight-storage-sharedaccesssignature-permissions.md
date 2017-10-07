---
title: "Megosztott hozzáférési aláírásokkal - Azure HDInsight aaaRestrict hozzáférés |} Microsoft Docs"
description: "Ismerje meg, hogyan férnek hozzá a toouse megosztott hozzáférési aláírásokkal toorestrict HDInsight az Azure storage blobs szolgáltatásában tárolja toodata."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 7bcad2dd-edea-467c-9130-44cffc005ff3
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/11/2017
ms.author: larryfr
ms.openlocfilehash: a34a2f8e52e47a15b09f09bc1fc67fc6159ec75f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-storage-shared-access-signatures-toorestrict-access-toodata-in-hdinsight"></a>Azure Storage megosztott hozzáférési aláírásokkal toorestrict hozzáférés toodata használata a Hdinsightban

HDInsight hello Azure Storage-fiókok hello-fürthöz tartozó teljes körű hozzáférési toodata van. Megosztott hozzáférési aláírásokkal hello blob tároló toorestrict access toohello adatokat is használhatja. Például tooprovide csak olvasási hozzáféréssel toohello adatok. Megosztott hozzáférési aláírásokkal (SAS) az Azure storage-fiókok egy szolgáltatása, amely lehetővé teszi toolimit hozzáférés toodata. Például a csak olvasási hozzáféréssel toodata biztosítása.

> [!IMPORTANT]
> Apache Pletyka használó megoldás érdemes a HDInsight-tartományhoz. További információkért lásd: hello [konfigurálása tartományhoz csatlakoztatott HDInsight](hdinsight-domain-joined-configure.md) dokumentum.

> [!WARNING]
> HDInsight teljes körű hozzáférési toohello alapértelmezett hello fürt tárolóhelyét kell rendelkeznie.

## <a name="requirements"></a>Követelmények

* Azure-előfizetés
* C# vagy Python. C#-példakód a Visual Studio megoldás valósul meg.

  * A Visual Studio 2013, 2015-öt vagy 2017 verziót kell lennie.
  * Python 2.7 vagy újabb verzióját kell lennie.

* A Linux-alapú HDInsight-fürt vagy [Azure PowerShell] [ powershell] – Ha egy meglévő Linux-alapú fürtöt, használhatja az Ambari tooadd egy közös hozzáférésű Jogosultságkód toohello fürt. Ha nem, akkor az Azure PowerShell toocreate egy fürt használja, és egy közös hozzáférésű Jogosultságkód hozzáadása a fürt létrehozása során.

    > [!IMPORTANT]
    > Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Példa fájlok hello [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature). Ebben a tárházban hello a következő elemeket tartalmazza:

  * Egy tároló, a tárolt házirend, és a SAS hozhat létre, és a HDInsight együttes használata a Visual Studio-projekt
  * Egy olyan tároló, a tárolt házirend és a SAS hozhat létre, és a HDInsight együttes használata a Python-parancsfájl
  * Egy PowerShell-parancsfájlt, amely hozhat létre a HDInsight fürt és toouse hello SAS konfigurálja.

## <a name="shared-access-signatures"></a>Megosztott hozzáférési aláírásokkal

Nincsenek megosztott hozzáférési aláírásokkal kétféle:

* Az ad hoc: hello start idő, a lejárat időpontjának és a SAS összes adhatók meg hello SAS URI hello engedélyekkel.

* Hozzáférési házirendben tárolt: A tárolt házirend erőforrás tárolóba, mint a blobtárolót van definiálva. A házirend legalább egy közös hozzáférésű jogosultságkód használt toomanage megkötéseit lehet. Ha SAS-kód társítja a tárolt házirend, hello SAS hello korlátozásokat örököl - hello start idő, a lejárati idő és a tárolt hello hozzáférési házirend definiált - engedélyek.

hello hello két űrlapok közötti különbség fontos egyik-forgatókönyvben: visszavont tanúsítványok. SAS-kód egy URL-címet, így bárki, aki jut hozzá a biztonsági Társítások hello használható, függetlenül attól, aki kérte, hogy a toobegin. Ha SAS-kód nyilvánosságra, azt bármely hello world használható. Terjesztett SAS-kód nem érvényes, amíg a négy dolog történik:

1. hello lejárati idejének megadott hello SAS éri el.

2. hello lejárati idejének megadott hello tárolt házirend SAS elérésekor hello hivatkozik. hello következő forgatókönyvek következtében hello lejárati idejének toobe érhető el:

    * hello időtartam lejárt.
    * hello tárolt házirend módosított toohave egy korábbi hello lejárati idő. Egyirányú toorevoke hello SAS hello lejárati idejének módosításával.

3. hello hivatkozás által hello SAS törlődik, amely egy másik módja toorevoke hello SAS hozzáférési házirendben tárolt. Ha azonos nevet, a SAS-tokenje hello tárolt hello hozzáférési házirend hozza létre újra hello előző házirend érvényesek (Ha nem ment hello hello lejárati ideje és a SAS). Ha azt tervezi, toorevoke hello SAS, esetén toouse meg arról, hogy egy másik nevet hello hozzáférési házirend a jövőbeli hello egy lejárati idővel hozza létre újra.

4. hello kulcsára, de a használt toocreate hello SAS újragenerálják. Hello kulcsának újragenerálása hatására az összes hello előző kulcs toofail hitelesítést használó alkalmazások. Az összes összetevő toohello új kulcs frissítése.

> [!IMPORTANT]
> A közös hozzáférésű jogosultságkód URI hello fiók kulcs használt toocreate hello aláírás társítva, és hello tartozó tárolt házirend (ha van ilyen). Ha nincs tárolt házirend van megadva, hello csak úgy toorevoke egy közös hozzáférésű jogosultságkódot toochange hello fiókkulcs.

Javasoljuk, hogy mindig használjon tárolt hozzáférési házirendeket. Tárolt házirendek használatakor aláírások visszavonására, és hello lejárati dátum meghosszabbításához igény szerint. jelen dokumentumban leírt lépések hello tárolt hozzáférési házirendek toogenerate SAS használja.

További információ a megosztott hozzáférési aláírásokkal: [ismertetése hello SAS-modell](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

### <a name="create-a-stored-policy-and-sas-using-c"></a>A tárolt házirend és a C használatával SAS létrehozása\#

1. Nyissa meg a hello megoldást a Visual Studio.

2. A Megoldáskezelőben kattintson a jobb gombbal a hello **SASToken** projektre, és válassza ki **tulajdonságok**.

3. Válassza ki **beállítások** , és adja hozzá a következő tételek hello értékeit:

   * StorageConnectionString: hello kapcsolati karakterlánc, amelyet az toocreate hello tárfiók a tárolt házirend és az SAS-kód. hello formátumúnak kell lennie `DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey` ahol `myaccount` hello a tárfiók neve és `mykey` hello hello tárfiók kulcsa.

   * ContainerName: hello tároló toorestrict eléréséhez használni kívánt hello tárfiókban.

   * SASPolicyName: hello neve toouse hello a tárolt házirend toocreate.

   * FileToUpload: hello elérési tooa fájl feltöltött toohello tároló.

4. Futtassa a hello projektet. Információk a következő szöveg hasonló toohello után hello SAS létrejött jelenik meg:

        Container SAS token using stored access policy: sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14

    Mentse a hello SAS-házirend jogkivonat, a tárfiók nevét és a tároló nevét. Hello storage-fiók társítása a HDInsight-fürt használja ezeket az értékeket.

### <a name="create-a-stored-policy-and-sas-using-python"></a>Hozzon létre egy tárolt házirend és a SAS pythonos környezetekben

1. Nyissa meg a hello SASToken.py fájlt, és módosítsa a következő értékek hello:

   * házirend\_name: hello neve toouse hello a tárolt házirend toocreate.

   * tárolási\_fiók\_name: hello a tárfiók nevét.

   * tárolási\_fiók\_kulcs: hello hello kulcsának.

   * tárolási\_tároló\_name: hello tároló toorestrict eléréséhez használni kívánt hello tárfiókban.

   * Példa\_fájl\_elérési út: hello elérési tooa fájl feltöltött toohello tároló.

2. Hello parancsprogrammal. Hello SAS-token hasonló toohello hello parancsfájl befejeződésekor a következő szöveg jeleníti meg:

        sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14

    Mentse a hello SAS-házirend jogkivonat, a tárfiók nevét és a tároló nevét. Hello storage-fiók társítása a HDInsight-fürt használja ezeket az értékeket.

## <a name="use-hello-sas-with-hdinsight"></a>Hello SAS használata a hdinsight eszközzel

HDInsight-fürtök létrehozásakor meg kell adnia egy elsődleges tárfiók, és opcionálisan megadhat további tárfiókokat. Mindkét módszer tárolási hozzáadásának szükséges teljes körű hozzáférési toohello storage-fiókok és a tárolók használt.

egy közös hozzáférésű Jogosultságkód toolimit hozzáférés tooa tároló toouse hozzáadása egy egyéni bejegyzés toohello **core-hely** hello fürt konfigurációjában.

* A **Windows-alapú** vagy **Linux-alapú** HDInsight-fürtök, a PowerShell használatával, a fürt létrehozása során hello bejegyzést adhat.
* A **Linux-alapú** a HDInsight-fürtök Ambari használatával fürt létrehozása után hello konfigurációjának módosítása.

### <a name="create-a-cluster-that-uses-hello-sas"></a>Hello SAS használó fürt létrehozása

Például, hogy SAS megtalálható hello használ hello HDInsight fürtök létrehozásával `CreateCluster` hello tárház könyvtárába. toouse azt használja hello a következő lépéseket:

1. Nyissa meg hello `CreateCluster\HDInsightSAS.ps1` fájlt egy szövegszerkesztőben, és módosítsa a következő értékek hello dokumentum hello elején hello.

    ```powershell
    # Replace 'mycluster' with hello name of hello cluster toobe created
    $clusterName = 'mycluster'
    # Valid values are 'Linux' and 'Windows'
    $osType = 'Linux'
    # Replace 'myresourcegroup' with hello name of hello group toobe created
    $resourceGroupName = 'myresourcegroup'
    # Replace with hello Azure data center you want toohello cluster toolive in
    $location = 'North Europe'
    # Replace with hello name of hello default storage account toobe created
    $defaultStorageAccountName = 'mystorageaccount'
    # Replace with hello name of hello SAS container created earlier
    $SASContainerName = 'sascontainer'
    # Replace with hello name of hello SAS storage account created earlier
    $SASStorageAccountName = 'sasaccount'
    # Replace with hello SAS token generated earlier
    $SASToken = 'sastoken'
    # Set hello number of worker nodes in hello cluster
    $clusterSizeInNodes = 3
    ```

    Például `'mycluster'` toohello neve hello fürt toocreate szeretné. hello SAS értéket meg kell felelnie hello értékek hello előző lépéseiből, egy tárfiók és a SAS-jogkivonat létrehozásakor.

    Miután hello értékek megváltoztak, mentse a hello fájlt.

2. Nyisson meg egy új Azure PowerShell-parancssorba. Ha nem ismeri az Azure PowerShell, vagy nem telepítette azt, lásd: [telepítse és konfigurálja az Azure Powershellt][powershell].

1. Hello parancssorába a következő parancs tooauthenticate tooyour Azure-előfizetés hello használata:

    ```powershell
    Login-AzureRmAccount
    ```

    Amikor a rendszer kéri, jelentkezzen be Azure-előfizetése hello fiók.

    Ha a fiók több Azure-előfizetéssel társítva, szükség lehet a toouse `Select-AzureRmSubscription` tooselect hello előfizetés toouse kívánja.

4. Hello parancssorból módosítsa a könyvtárakat toohello `CreateCluster` hello HDInsightSAS.ps1 fájlt tartalmazó könyvtár. Ezután használja a következő parancsfájl toorun hello hello

    ```powershell
    .\HDInsightSAS.ps1
    ```

    Hello parancsfájlt futtat, mint naplózza kimeneti toohello PowerShell-parancssorba csoport és a storage-fiókokat készít hello erőforrás. Biztosan felszólító tooenter hello HTTP felhasználó hello HDInsight-fürthöz. Ez a fiók akkor használt toosecure HTTP/s hozzáférést toohello fürt.

    A Linux-alapú fürt létrehozásakor egy SSH felhasználói fiók felhasználónevét és jelszavát kéri. Ez a fiók akkor használt tooremotely napló toohello fürtben.

   > [!IMPORTANT]
   > Ha a HTTP/s hello vagy SSH-felhasználónév és jelszó megadására kéri, meg kell adnia egy jelszót, amely megfelel a következő feltételek hello:
   >
   > * Legalább 10 karakter hosszúságúnak kell lennie.
   > * Legalább egy számot kell tartalmaznia.
   > * Legalább egy nem alfanumerikus karaktert kell tartalmaznia
   > * Tartalmaznia kell legalább egy nagy- vagy kisbetűt

A parancsfájl toocomplete, míg általában körülbelül 15 percet vesz igénybe. Amikor hello parancsfájl hiba nélkül befejeződött, hello fürt létrehozását.

### <a name="use-hello-sas-with-an-existing-cluster"></a>Meglévő fürt hello SAS használata

Ha egy meglévő Linux-alapú fürtöt, adhat hozzá hello SAS toohello **core-hely** konfigurációs lépések hello segítségével:

1. Nyissa meg a fürt hello Ambari webes felhasználói Felületét. Ezen a lapon hello címet https://YOURCLUSTERNAME.azurehdinsight.net. Amikor a rendszer kéri, toohello fürt hello felügyeleti neve (rendszergazda) használatával hitelesíteni és jelszó használatával mikor hello fürtöt hoz létre.

2. Hello bal oldalán található hello Ambari webes felhasználói felület, válassza ki **HDFS** , és válassza a hello **Configs** hello középső hello lap fülre.

3. Jelölje be hello **speciális** lapot, és görgessen, amíg meg nem látja hello **egyéni core-hely** szakasz.

4. Bontsa ki a hello **egyéni core-hely** területen, majd görgessen toohello end és select hello **tulajdonság hozzáadása... ** hivatkozásra. Hello használata hello következő értékei **kulcs** és **érték** mezők:

   * **Kulcs**: fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net
   * **Érték**: hello futtatta korábban C# vagy Python-alkalmazás által visszaadott SAS hello

     Cserélje le **CONTAINERNAME** hello tároló nevű használt hello C# vagy SAS alkalmazást. Cserélje le **STORAGEACCOUNTNAME** a tárfiók neve hello használt.

5. Hello kattintson **Hozzáadás** toosave a kulcs-érték gombra, majd kattintson az hello **mentése** toosave hello konfigurációs módosítások gombra. Amikor a rendszer kéri, adjon meg egy leírást ("hozzáadása SAS tárolók eléréséhez" például) hello változás, és kattintson a **mentése**.

    Kattintson a **OK** amikor hello módosítások elvégzése után.

   > [!IMPORTANT]
   > Hello módosítás érvénybe léptetéséhez újra kell indítania számos szolgáltatást.

6. Hello Ambari webes felhasználói felület, válassza ki **HDFS** hello listából hello maradt, és válassza a **indítsa újra az összes** a hello **szolgáltatás műveletek** legördülő listából a megfelelő hello. Amikor a rendszer kéri, válassza ki a **kapcsolja be a karbantartási mód** és majd válassza ki __Conform indítsa újra az összes ".

    Ismételje meg ezt a folyamatot MapReduce2 és YARN.

7. Hello szolgáltatás újraindítása, ha mindegyiknél válassza ki, és tiltsa le a karbantartási mód a hello **szolgáltatás műveletek** legördülő listán.

## <a name="test-restricted-access"></a>Korlátozott hozzáférés tesztelése

tooverify, hogy korlátozott hozzáféréssel rendelkező, a következő módszerek használatát hello:

* A **Windows-alapú** a HDInsight-fürtök, a távoli asztal tooconnect toohello-fürt használatára. További információkért lásd: [csatlakozás RDP Funkciót használnak tooHDInsight](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

    Miután csatlakozott, a hello **parancssori Hadoop** hello asztali tooopen egy parancs parancssori futtatásával ikonra.

* A **Linux-alapú** a HDInsight-fürtök SSH tooconnect toohello-fürt használatára. További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

Miután csatlakozott toohello fürt, használja a következő lépéseket tooverify, hogy a csak olvasási és listában található elemek hello SAS tárfiók is hello:

1. hello tároló, toolist hello tartalmának parancs hello parancssorból a következő hello használata: 

    ```bash
    hdfs dfs -ls wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/
    ```

    Cserélje le **SASCONTAINER** hello nevű hello tároló hello tárfiók SAS létre. Cserélje le **SASACCOUNTNAME** hello nevű hello tárfiók hello SAS használatos.

    hello listán hello fájl feltöltése, amikor hello tároló és a SAS hoztak létre.

2. A következő parancs tooverify hello fájl tartalmának hello elolvasni hello használata. Cserélje le a hello **SASCONTAINER** és **SASACCOUNTNAME** ahogy hello előző lépésben. Cserélje le **Fájlnév** hello nevű hello fájl hello előző parancs jelenik meg:

    ```bash
    hdfs dfs -text wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME
    ```

    Ez a parancs kilistázza hello hello fájl tartalmát.

3. A következő parancs toodownload hello fájl toohello helyi fájlrendszer hello használata:

    ```bash
    hdfs dfs -get wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME testfile.txt
    ```

    Ez a parancs letölti a fájl tooa helyi fájl nevű hello **példa.txt**.

4. Használjon hello következő parancsot a tooupload hello helyi fájl tooa új fájlt **testupload.txt** a hello SAS-tárolót:

    ```bash
    hdfs dfs -put testfile.txt wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/testupload.txt
    ```

    Megjelenik egy üzenet hasonló toohello, a következő szöveget:

        put: java.io.IOException

    Ez a hiba akkor fordul elő, mert hello tárolási helye nem olvasható + lista csak. Parancs tooput hello adatok hello alapértelmezett tároló hello fürt, írható a következő hello használata:

    ```bash
    hdfs dfs -put testfile.txt wasb:///testupload.txt
    ```

    Megadott idő hello műveletet kell végrehajtani.

## <a name="troubleshooting"></a>Hibaelhárítás

### <a name="a-task-was-canceled"></a>A feladat meg lett szakítva

**A jelenség**: hello PowerShell-parancsfájlt a fürt létrehozásakor jelenhet meg a következő hibaüzenet hello:

    New-AzureRmHDInsightCluster : A task was canceled.
    At C:\Users\larryfr\Documents\GitHub\hdinsight-azure-storage-sas\CreateCluster\HDInsightSAS.ps1:62 char:5
    +     New-AzureRmHDInsightCluster `
    +     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (:) [New-AzureRmHDInsightCluster], CloudException
        + FullyQualifiedErrorId : Hyak.Common.CloudException,Microsoft.Azure.Commands.HDInsight.NewAzureHDInsightClusterCommand

**OK**: Ez a hiba akkor fordulhat elő, ha jelszót használhat hello admin/HTTP felhasználói hello fürt, vagy (a Linux-alapú fürtök) hello SSH-felhasználó.

**Megoldási**: használhat olyan jelszót, amely megfelel a következő feltételek hello:

* Legalább 10 karakter hosszúságúnak kell lennie.
* Legalább egy számot kell tartalmaznia.
* Legalább egy nem alfanumerikus karaktert kell tartalmaznia
* Tartalmaznia kell legalább egy nagy- vagy kisbetűt

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta, hogyan tooadd korlátozott hozzáférésű tárolási tooyour HDInsight-fürtjéhez, ismerje meg, más módokon toowork a fürtön lévő adatokkal:

* [A Hive használata a HDInsightban](hdinsight-use-hive.md)
* [A Pig használata a HDInsightban](hdinsight-use-pig.md)
* [Use MapReduce with HDInsight](hdinsight-use-mapreduce.md)

[powershell]: /powershell/azureps-cmdlets-docs
