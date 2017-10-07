---
title: "aaaAdd további az Azure storage-fiókok tooHDInsight |} Microsoft Docs"
description: "Ismerje meg, hogyan tooadd további az Azure storage accounts tooan meglévő HDInsight-fürtre."
services: hdinsight
documentationCenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/04/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: ce5acfa4b61bf7e83b1fb374d64a1eaa3182fbec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-additional-storage-accounts-toohdinsight"></a>További tárhely fiókok tooHDInsight hozzáadása

Ismerje meg, hogyan toouse parancsfájl műveletek tooadd további az Azure storage accounts tooHDInsight. hello jelen dokumentumban leírt lépések hozzáadása a tárolási fiók tooan meglévő Linux-alapú HDInsight-fürtre.

> [!IMPORTANT]
> a dokumentumban szereplő információk hello tárgya további tárhely tooa fürt hozzáadása után lett létrehozva. A storage-fiókok hozzáadása a fürt létrehozása során további információkért lásd: [állítsa be a HDInsight Hadoop, Spark, Kafka és több fürt](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="how-it-works"></a>Működés

Ezt a parancsfájlt hello a következő paramétereket fogadja:

* __Az Azure storage-fiók neve__: hello hello tárolási fiók tooadd toohello HDInsight-fürt nevét. Hello parancsprogram futtatása után HDInsight olvashat és írhat adatokat tárolja ezt a tárfiókot.

* __Azure storage-fiók kulcs__: egy kulcs, amely engedélyezi a hozzáférést toohello tárfiók.

* __-p__ (nem kötelező): Ha meg van adva, hello kulcs nem titkosított, és egyszerű szövegként hello core-site.xml fájl tárolja.

A feldolgozás során hello parancsfájl hello a következő műveleteket hajtja végre:

* Ha hello tárfiók már létezik a hello fürt hello core-site.xml konfigurációjában, hello parancsfájl kilép, és nincs több teendő történik.

* Ellenőrzi, hogy hello storage-fiók létezik-e, és hello kulcs segítségével férhetők el.

* Hello kulcs hello fürt hitelesítő adat segítségével titkosítja.

* Hello tárolási fiók toohello core-site.xml fájl hozzáadása.

* Leállítja és újraindítja hello Oozie, YARN, MapReduce2 és HDFS szolgáltatásokat. Ezek a szolgáltatások indítása és leállítása lehetővé teszi, hogy azok toouse hello új tárfiókot.

> [!WARNING]
> A storage-fiók egy másik helyen található mint hello HDInsight-fürt használata nem támogatott.

## <a name="hello-script"></a>hello parancsfájl

__Parancsfájl-hely__: [https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh)

__Követelmények__:

* hello parancsfájl kell alkalmazni a hello __Átjárócsomópontokat__.

## <a name="toouse-hello-script"></a>toouse hello parancsfájl

Ezt a parancsfájlt a hello Azure-portálon az Azure PowerShell is használható, vagy Azure CLI 1.0 hello. További információkért lásd: hello [testreszabása Linux-alapú HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) dokumentum.

> [!IMPORTANT]
> Ha hello itt hello testreszabási dokumentumban ismertetett lépéseket, használja ezt a parancsfájlt a következő információk tooapply hello:
>
> * Cserélje le a példa parancsfájlművelet URI hello URI ehhez a parancsprogramhoz (https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh).
> * Cserélje le a példa paramétereket hello az Azure storage fióknevet és kulcsot hello tárolási fiók toobe hozzáadott toohello fürt. Ha az Azure portál használatával hello, ezeket a paramétereket szóközzel kell elválasztani.
> * Nem kell toomark ezt a parancsfájlt, __megőrzött__, azt közvetlenül hello Ambari konfigurációs hello fürt frissítéséhez.

## <a name="known-issues"></a>Ismert problémák

### <a name="storage-accounts-not-displayed-in-azure-portal-or-tools"></a>Nem jelenik meg az Azure portálon vagy az eszközök a Storage-fiókok

Amikor megtekinti hello HDInsight fürthöz hello Azure-portálon, válassza a hello __Storage-fiókok__ bejegyzésre az __tulajdonságok__ nem jelennek meg a parancsfájlművelet hozzáadva storage-fiókok. Az Azure PowerShell és az Azure parancssori felület ne jelenjen meg hello további tárfiók vagy.

hello tárolással kapcsolatos nem jelenik meg, mert hello parancsfájl csak módosítja hello fürt hello core-site.xml konfigurációjában. Ezeket az információkat nem használja az Azure felügyeleti API-k használatával hello fürt információk beolvasása.

tooview tárfiókadatok hozzáadott toohello fürt ezt a parancsfájlt használja hello Ambari REST API használatával. A következő parancsok tooretrieve hello ezt az információt használja a fürt:

```PowerShell
$creds = Get-Credential -UserName "admin" -Message "Enter hello cluster login credentials"
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.configurations.properties."fs.azure.account.key.$storageAccountName.blob.core.windows.net"
```

> [!NOTE]
> Állítsa be `$clusterName` toohello hello HDInsight-fürt nevét. Állítsa be `$storageAccountName` toohello hello storage-fiók nevét. Amikor a rendszer kéri, adja meg a hello fürt bejelentkezési (rendszergazda) és jelszavát.

```Bash
curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.azure.account.key.$STORAGEACCOUNTNAME.blob.core.windows.net"] | select(. != null)'
```

> [!NOTE]
> Állítsa be `$PASSWORD` toohello fürt (rendszergazda) bejelentkezési fiók jelszavát. Állítsa be `$CLUSTERNAME` toohello hello HDInsight-fürt nevét. Állítsa be `$STORAGEACCOUNTNAME` toohello hello storage-fiók nevét.
>
> Ez a példa [curl (http://curl.haxx.se/)](http://curl.haxx.se/) és [jq (https://stedolan.github.io/jq/)](https://stedolan.github.io/jq/) tooretrieve és elemzési JSON-adatokat.

Ha ezzel a paranccsal cserélje le __CLUSTERNAME__ hello nevű hello HDInsight-fürt. Cserélje le __jelszó__ hello fürt hello HTTP bejelentkezési jelszót. Cserélje le __STORAGEACCOUNT__ hello nevű hello tárfiók hozzáadása parancsfájlművelet használatával. Ez a parancs által visszaadott adatokat a következő szöveg hasonló toohello jelenik meg:

    "MIIB+gYJKoZIhvcNAQcDoIIB6zCCAecCAQAxggFaMIIBVgIBADA+MCoxKDAmBgNVBAMTH2RiZW5jcnlwdGlvbi5henVyZWhkaW5zaWdodC5uZXQCEA6GDZMW1oiESKFHFOOEgjcwDQYJKoZIhvcNAQEBBQAEggEATIuO8MJ45KEQAYBQld7WaRkJOWqaCLwFub9zNpscrquA2f3o0emy9Vr6vu5cD3GTt7PmaAF0pvssbKVMf/Z8yRpHmeezSco2y7e9Qd7xJKRLYtRHm80fsjiBHSW9CYkQwxHaOqdR7DBhZyhnj+DHhODsIO2FGM8MxWk4fgBRVO6CZ5eTmZ6KVR8wYbFLi8YZXb7GkUEeSn2PsjrKGiQjtpXw1RAyanCagr5vlg8CicZg1HuhCHWf/RYFWM3EBbVz+uFZPR3BqTgbvBhWYXRJaISwssvxotppe0ikevnEgaBYrflB2P+PVrwPTZ7f36HQcn4ifY1WRJQ4qRaUxdYEfzCBgwYJKoZIhvcNAQcBMBQGCCqGSIb3DQMHBAhRdscgRV3wmYBg3j/T1aEnO3wLWCRpgZa16MWqmfQPuansKHjLwbZjTpeirqUAQpZVyXdK/w4gKlK+t1heNsNo1Wwqu+Y47bSAX1k9Ud7+Ed2oETDI7724IJ213YeGxvu4Ngcf2eHW+FRK"

Ez a szöveg látható egy példa egy titkosított kulcsot, amely tooaccess hello storage-fiók.

### <a name="unable-tooaccess-storage-after-changing-key"></a>Nem lehet tooaccess tárolási kulcs módosítása után

Ha módosítja egy hello kulcsának, HDInsight már nem tud hozzáférni a hello tárfiók. HDInsight kulcs gyorsítótárazott másolatának hello core-site.xml hello fürt használja. A gyorsítótárban található példányát a frissített toomatch hello új kulcsot kell lennie.

Hello parancsfájlművelet újra fut does __nem__ hello kulcs frissíti hello parancsfájl toosee ellenőrzi, hogy hello storage-fiókhoz tartozó bejegyzés már létezik. Ha már létezik egy bejegyzést, tegye a módosításokat.

a probléma megoldásához toowork, el kell távolítania hello meglévő bejegyzés hello tárfiók. A következő lépéseket tooremove hello meglévő bejegyzés hello használata:

1. Egy böngészőben nyissa meg a hello Ambari webes felhasználói felületén a HDInsight-fürthöz. hello URI https://CLUSTERNAME.azurehdinsight.net. Cserélje le __CLUSTERNAME__ hello néven a fürt.

    Amikor a rendszer kéri, adja meg bejelentkezési felhasználói hello HTTP és a jelszavát a fürt.

2. Szolgáltatások hello bal oldali hello lap hello listában jelölje ki __HDFS__. Válassza ki hello __Configs__ hello center hello lap lapján.

3. A hello __szűrő...__  mezőbe írja be az érték __fs.azure.account__. Ez visszaad minden további tárfiókok toohello fürt hozzáadott bejegyzéseket. Két különböző bejegyzések; __keyprovider__ és __kulcs__. Mindkettő rendelkezik hello tárfiók hello kulcsnév részeként hello nevére.

    hello bejegyzések a következők példa egy nevű tárfiók __mystorage__:

        fs.azure.account.keyprovider.mystorage.blob.core.windows.net
        fs.azure.account.key.mystorage.blob.core.windows.net

4. Miután azonosította hello kulcsokat hello tooremove van szüksége, a vörös hello '-' ikon toohello sarkában hello bejegyzés toodelete azt. Ezután használja az hello __mentése__ toosave gombra a módosítások.

5. Módosítások mentése után hello parancsfájl művelet tooadd hello tárfiókot és az új kulcs értéke toohello fürt használja.

### <a name="poor-performance"></a>Gyenge teljesítményt

Ha hello tárfiók mint hello HDInsight-fürt egy másik régióban, gyenge teljesítményt tapasztalhat. Egy másik régióban küld a hálózati forgalom, hello regionális Azure adatközponton kívül és keresztben elérése során adatokat a nyilvános internethez, ami vethet fel késés hello.

> [!WARNING]
> A storage-fiók egy másik régióban, mint a HDInsight-fürt hello használata nem támogatott.

### <a name="additional-charges"></a>További díjak

Ha hello tárfiók más régióban, mint a HDInsight-fürt hello, további kilépő díjak Észreveheti az Azure számlázás a. Egy kimenő kell fizetni akkor érvényes, ha az adatok egy regionális adatközpont hagyja. Ez a díj lesz alkalmazva, akkor is, ha egy másik Azure-adatközpont egy másik régióban szánt hello forgalmat.

> [!WARNING]
> A storage-fiók egy másik régióban, mint a HDInsight-fürt hello használata nem támogatott.

## <a name="next-steps"></a>Következő lépések

Megtanulta, hogyan tooadd további tárfiókok tooan meglévő HDInsight-fürtre. A Parancsfájlműveletek további információkért lásd: [testreszabása Linux-alapú HDInsight-fürtök parancsfájlművelet használatával](hdinsight-hadoop-customize-cluster-linux.md)
