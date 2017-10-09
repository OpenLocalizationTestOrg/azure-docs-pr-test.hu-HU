---
title: a Hadoop - Azure HDInsight SSH aaaUse |} Microsoft Docs
description: "A HDInsight a Secure Shell (SSH) segítségével érhető el. Ez a dokumentum információkat biztosít a Windows, Linux, Unix vagy macOS ügyfelek tooHDInsight használatával hello ssh és az scp parancsok csatlakozó."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
keywords: "hadoop parancsok linux rendszerben, hadoop linux parancsok, hadoop macos, ssh hadoop, ssh hadoop fürt"
ms.assetid: a6a16405-a4a7-4151-9bbf-ab26972216c5
ms.service: hdinsight
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/03/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive,hdiseo17may2017
ms.openlocfilehash: ac9e70ce3c70693c1b81c9514ba4fd47686070ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-toohdinsight-hadoop-using-ssh"></a>Csatlakozzon az SSH használatával tooHDInsight (Hadoop)

Megtudhatja, hogyan toouse [Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) toosecurely csatlakozzon az Azure HDInsight tooHadoop. 

HDInsight használható Linux (Ubuntu) hello operációs rendszerként hello Hadoop-fürt csomópontja. hello következő táblázat ismerteti hello cím és port tooLinux-alapú HDInsight egy SSH-ügyfél a csatlakozáshoz szükséges:

| Cím | Port | A következőhöz csatlakozik: |
| ----- | ----- | ----- |
| `<clustername>-ed-ssh.azurehdinsight.net` | 22 | Élcsomópont (R Server a HDInsightban) |
| `<edgenodename>.<clustername>-ssh.azurehdinsight.net` | 22 | Élcsomópont (bármely egyéb fürttípus, ha létezik élcsomópont) |
| `<clustername>-ssh.azurehdinsight.net` | 22 | Elsődleges átjárócsomópont |
| `<clustername>-ssh.azurehdinsight.net` | 23 | Másodlagos átjárócsomópont |

> [!NOTE]
> Cserélje le `<edgenodename>` hello élcsomópont hello nevére.
>
> Cserélje le `<clustername>` hello néven a fürt.
>
> Ha a fürt egy élcsomópontot tartalmaz, azt javasoljuk, hogy Ön __mindig csatlakozás az élcsomóponthoz toohello__ SSH használatával. hello átjárócsomópontokkal gazdagép hadoop kritikus toohello egészségügyi szolgáltatásokat. hello élcsomópont fut, csak mi helyezünk rajta.
>
> Az élcsomópontok használatával kapcsolatban további információért lásd: [Élcsomópontok használata a HDInsightban](hdinsight-apps-use-edge-node.md#access-an-edge-node).

## <a name="ssh-clients"></a>SSH-ügyfelek

MacOS, Linux és Unix rendszerek biztosítanak hello `ssh` és `scp` parancsok. Hello `ssh` ügyfél távoli parancssori munkamenetet egy Linux vagy Unix-alapú rendszeren a gyakran használt toocreate. Hello `scp` ügyfél használt toosecurely a fájlok másolása az ügyfél és a hello távoli rendszer között.

A Microsoft Windows alapértelmezés szerint nem biztosít SSH-ügyfelet. Hello `ssh` és `scp` ügyfelek érhetők el a Windows hello csomagok a következő használatával:

* [Azure Cloud rendszerhéj](../cloud-shell/quickstart.md): hello felhő rendszerhéj a böngészőben Bash környezetet biztosít, és hello biztosít `ssh`, `scp`, és más közös Linux-parancsok.

* [A Windows 10 Ubuntu bash](https://msdn.microsoft.com/commandline/wsl/about): hello `ssh` és `scp` parancsok Windows parancssorban Bash hello keresztül érhetők el.

* [Git (https://git-scm.com/)](https://git-scm.com/): hello `ssh` és `scp` parancsok is elérhetők hello GitBash parancssor használatával.

* [GitHub asztali (https://desktop.github.com/)](https://desktop.github.com/) hello `ssh` és `scp` parancsok hello GitHub rendszerhéj parancssori keresztül érhetők el. GitHub asztali lehet konfigurált toouse Basht és a Windows parancssor vagy PowerShell hello hello Git rendszerhéj hello parancs sorban.

* [OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH): hello PowerShell csapatának van hordozás OpenSSH tooWindows, és tesztelési kiadásokban biztosít.

    > [!WARNING]
    > hello OpenSSH csomag magában foglalja a hello SSH kiszolgáló-összetevő `sshd`. Ez az összetevő elindítja az SSH-kiszolgálót a rendszer, így mások tooconnect tooit. Ne konfigurálja ezt az összetevőt, és nyissa meg a 22-es portot, kivéve, ha az SSH-kiszolgálót toohost a rendszeren. A hdinsight eszközzel szükséges toocommunicate nincs.

Több grafikus SSH-ügyfél is elérhető, például a [PuTTY (http://www.chiark.greenend.org.uk/~sgtatham/putty/)](http://www.chiark.greenend.org.uk/~sgtatham/putty/) és a [MobaXterm (http://mobaxterm.mobatek.net/)](http://mobaxterm.mobatek.net/). Ezek az ügyfelek lehetnek használt tooconnect tooHDInsight, hello folyamat, amely eltér hello segítségével `ssh` segédprogram. További információ a dokumentációban hello hello grafikus ügyfél használ.

## <a id="sshkey"></a>Hitelesítés: SSH-kulcsok

SSH kulcs használja [nyilvános kulcsú](https://en.wikipedia.org/wiki/Public-key_cryptography) tooauthenticate SSH-munkamenetet. SSH-kulcsok biztonságosabbak jelszavakat, és adjon meg egy egyszerűen toosecure hozzáférés tooyour Hadoop-fürt.

Ha az SSH-fiók használatával lett biztonságossá téve egy kulcs, hello ügyfél biztosítania kell a megfelelő titkos kulccsal, csatlakozás hello:

* A legtöbb ügyfél lehet konfigurált toouse egy __alapértelmezett kulcs__. Például hello `ssh` , keres a titkos kulcs, `~/.ssh/id_rsa` a Linux és Unix-környezetben.

* Megadhatja a hello __tooa titkos kulcs elérési útja__. A hello `ssh` ügyfél, hello `-i` paraméter használt toospecify hello elérési tooprivate kulcs. Például: `ssh -i ~/.ssh/id_rsa sshuser@myedge.mycluster-ssh.azurehdinsight.net`.

* Ha __több titkos kulccsal__ rendelkezik, amelyeket különböző kiszolgálókhoz használ, fontolja meg az olyan segédprogramok használatát, mint például az [ssh-agent (https://en.wikipedia.org/wiki/Ssh-agent)](https://en.wikipedia.org/wiki/Ssh-agent) (ssh-ügynök). Hello `ssh-agent` segédprogram lehet a használt tooautomatically válassza hello kulcs toouse létrehozó SSH-munkamenetet.

> [!IMPORTANT]
>
> Ha biztonságos a titkos kulcsot egy hozzáférési kóddal, meg kell adnia a hello jelszót, hello kulcs használata esetén. Például segédprogramok `ssh-agent` hello jelszót az Ön kényelme képes gyorsítótárazni.

### <a name="create-an-ssh-key-pair"></a>SSH-kulcs létrehozása

Használjon hello `ssh-keygen` toocreate nyilvános és titkos kulcs fájlok parancsot. hello következő parancs létrehoz egy 2048 bites RSA kulcspár, amelyet a hdinsight eszközzel használható:

    ssh-keygen -t rsa -b 2048

Hello kulcs létrehozása során információkat kéri. Például hello kulcsok tárolására, vagy hogy toouse egy hozzáférési kódot. Hello folyamat befejezése után létrejönnek a két fájlt; a nyilvános kulcs és a titkos kulcs.

* Hello __nyilvános kulcs__ van használt toocreate HDInsight-fürtöt. nyilvános kulcs hello kiterjesztése a `.pub`.

* Hello __titkos kulcs__ használt tooauthenticate ügyfél toohello HDInsight-fürtjéhez van.

> [!IMPORTANT]
> A kulcsokat jelszóval védheti. Ez a jelszó lényegében egy kód a titkos kulcson. Akkor is, ha valaki jut hozzá a titkos kulcsot, hello jelszót toouse hello kulccsal kell rendelkezniük.

### <a name="create-hdinsight-using-hello-public-key"></a>HDInsight a hello nyilvános kulcs létrehozása

| Létrehozási metódus | Hogyan toouse hello nyilvános kulcs |
| ------- | ------- |
| **Azure Portal** | Törölje a jelet __ugyanazt a jelszót használják a fürt bejelentkezési__, majd válassza ki __nyilvános kulcs__ , hello SSH hitelesítés típusa. Végül, válasszon nyilvános kulcsfájlt hello, vagy illessze be hello fájl szöveges tartalmának hello hello __nyilvános SSH-kulcs__ mező.</br>![Nyilvános SSH-kulcs párbeszédpanel a HDInsight-fürt létrehozásakor](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-public-key.png) |
| **Azure PowerShell** | Használjon hello `-SshPublicKey` hello paramétere `New-AzureRmHdinsightCluster` hello nyilvános kulcsot karakterláncként parancsmag és pass hello tartalmát.|
| **Azure CLI 1.0** | Használjon hello `--sshPublicKey` hello paramétere `azure hdinsight cluster create` parancsot, és adja át a hello nyilvános kulcs tartalmát hello karakterláncból. |
| **Resource Manager-sablon** | Az SSH-kulcsok sablonnal történő használatának példájáért tekintse meg a [HDInsight Linux rendszeren, SSH-kulccsal való telepítéséről](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-publickey/) szóló témakört. Hello `publicKeys` hello elemének [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-publickey/azuredeploy.json) fájl használt toopass hello kulcsok tooAzure hello fürt létrehozásakor. |

## <a id="sshpassword"></a>Hitelesítés: Jelszó

Az SSH-fiókok jelszóval védhetők. SSH használatával tooHDInsight csatlakoztatásakor felszólító tooenter hello jelszó áll.

> [!WARNING]
> Nem ajánlott jelszavas hitelesítést használni az SSH-hoz. Jelszavak kitalál is lehet, és sebezhető toobrute módszerén alapuló támadások. Ehelyett azt javasoljuk, hogy használjon [SSH-kulcsokat a hitelesítéshez](#sshkey).

### <a name="create-hdinsight-using-a-password"></a>HDInsight létrehozása jelszóval

| Létrehozási metódus | Hogyan toospecify hello jelszó |
| --------------- | ---------------- |
| **Azure Portal** | Alapértelmezés szerint hello SSH felhasználói fiók rendelkezik hello hello fürt bejelentkezési fiókként ugyanazt a jelszót. toouse egy másik jelszót, törölje a jelet __ugyanazt a jelszót használják a fürt bejelentkezési__, majd adja meg hello jelszó hello __SSH-jelszónak__ mező.</br>![SSH-jelszó párbeszédpanel a HDInsight-fürt létrehozásakor](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-password.png)|
| **Azure PowerShell** | Használjon hello `--SshCredential` hello paramétere `New-AzureRmHdinsightCluster` parancsmag, és adja át a `PSCredential` objektum, amely tartalmazza a hello SSH felhasználói fiók nevét és jelszavát. |
| **Azure CLI 1.0** | Használjon hello `--sshPassword` hello paramétere `azure hdinsight cluster create` parancsot, és adjon meg értéket a hello jelszó. |
| **Resource Manager-sablon** | A jelszavak sablonnal történő használatának példájáért tekintse meg a [HDInsight Linux rendszeren, SSH-kulccsal való telepítéséről](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-password/) szóló témakört. Hello `linuxOperatingSystemProfile` hello elemének [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-password/azuredeploy.json) fájl használt toopass hello SSH fiók nevét és jelszavát tooAzure hello fürt létrehozásakor.|

### <a name="change-hello-ssh-password"></a>Hello SSH-jelszó módosítása

Hello SSH felhasználói fiók jelszavának módosításával kapcsolatos információkért lásd: hello __jelszavak módosítása__ hello szakasza [kezelése HDInsight](hdinsight-administer-use-portal-linux.md#change-passwords) dokumentum.

## <a id="domainjoined"></a>Hitelesítés: Tartományhoz csatlakoztatott HDInsight

Ha használ egy __tartományhoz HDInsight-fürt__, hello kell használnia `kinit` csatlakoztassa a SSH parancs. A parancs egy tartományi felhasználó és egy jelszót kér, és hitelesíti a munkamenet hello Azure Active Directory-tartomány hello-fürthöz tartozó.

További információkat itt talál: [Tartományhoz csatlakoztatott HDInsight konfigurálása](hdinsight-domain-joined-configure.md).

## <a name="connect-toonodes"></a>Csatlakozás toonodes

hello átjárócsomópontokkal és élcsomópont (ha van ilyen) hello keresztül is elérhető internet a 22-es és 23 portokat.

* Toohello kapcsolódáskor __átjárócsomópontokat__, port használatára __22__ tooconnect toohello elsődleges csomópont és a port head __23__ tooconnect toohello másodlagos átjárócsomópont. hello teljesen minősített név toouse van `clustername-ssh.azurehdinsight.net`, ahol `clustername` hello a fürt neve van.

    ```bash
    # Connect tooprimary head node
    # port not specified since 22 is hello default
    ssh sshuser@clustername-ssh.azurehdinsight.net

    # Connect toosecondary head node
    ssh -p 23 sshuser@clustername-ssh.azurehdinsight.net
    ```
    
* Ha connectiung toohello __élcsomópont__, 22-es portot használja. hello teljesen minősített tartománynév megadása `edgenodename.clustername-ssh.azurehdinsight.net`, ahol `edgenodename` egy nevet, ha a megadott hoz létre a hello élcsomópont. `clustername`hello hello fürt neve van.

    ```bash
    # Connect tooedge node
    ssh sshuser@edgnodename.clustername-ssh.azurehdinsight.net
    ```

> [!IMPORTANT]
> hello előző példák azt feltételezik, hogy jelszó-hitelesítést használ, vagy a tanúsítványhitelesítés végrehajtását automatikusan. Ha egy SSH-kulcspár hitelesítéshez használandó, és nem használ automatikusan hello tanúsítványt, akkor hello `-i` paraméter toospecify hello titkos kulcsot. Például: `ssh -i ~/.ssh/mykey sshuser@clustername-ssh.azurehdinsight.net`.

A csatlakozás után a hello prompt megváltoztatja tooindicate hello SSH felhasználói nevét és hello csomópont csatlakozik. Például, ha a kapcsolódó toohello elsődleges átjárócsomópont, `sshuser`, hello kérdés van `sshuser@hn0-clustername:~$`.

### <a name="connect-tooworker-and-zookeeper-nodes"></a>Csatlakozás tooworker és Zookeeper csomópontok

hello munkavégző csomópontokhoz, és a Zookeeper csomópontok nem érhetők el közvetlenül az internethez hello. Hello központi fürtcsomópontok vagy peremhálózati csomópontok hozzáférhetők. Az alábbiakban hello hello általános lépéseket tooconnect tooother csomópontok:

1. SSH tooconnect tooa head vagy peremhálózati csomópont használja:

        ssh sshuser@myedge.mycluster-ssh.azurehdinsight.net

2. Az SSH-kapcsolat toohello head hello vagy élcsomópont, használja a hello `ssh` parancs tooconnect tooa munkavégző hello fürt csomópontja:

        ssh sshuser@wn0-myhdi

    tooretrieve hello fürtben található csomópontok hello, hello tartománynevek listáját lásd: hello [kezelése HDInsight használatával hello Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-fqdn-of-cluster-nodes) dokumentum.

Ha SSH-fiókjának hello használatával lett biztonságossá téve a __jelszó__, hello jelszó kapcsolódás esetén.

Ha SSH-fiókjának hello használatával lett biztonságossá téve __SSH-kulcsok__, győződjön meg arról, hogy az SSH-továbbítás engedélyezve van-e hello ügyfélen.

> [!NOTE]
> Egy másik toodirectly hozzáférési hello fürt összes csomópontjának módja tooinstall HDInsight egy Azure virtuális hálózatra. Ezt követően csatlakozhat a távoli gép toohello azonos virtuális hálózatot, és közvetlenül hozzáférni az hello fürt összes csomópontján.
>
> További információ: [Virtuális hálózat használata HDInsighttal](hdinsight-extend-hadoop-virtual-network.md).

### <a name="configure-ssh-agent-forwarding"></a>SSH-ügynöktovábbítás konfigurálása

> [!IMPORTANT]
> hello lépések azt feltételezik, a Linux vagy UNIX-alapú rendszeren, és használata Windows 10 Bash. Ha ezeket a lépéseket nem működnek a rendszerhez, szükség lehet az SSH-ügyfél tooconsult hello dokumentációját.

1. Egy szövegszerkesztővel nyissa meg a `~/.ssh/config` fájlt. Ha a fájl nem létezik, létrehozhatja a parancssoron az `touch ~/.ssh/config` karakterlánc beírásával.

2. Adja hozzá a következő szöveg toohello hello `config` fájlt.

        Host <edgenodename>.<clustername>-ssh.azurehdinsight.net
          ForwardAgent yes

    Cserélje le a hello __állomás__ információk hello csomópont hello címmel toousing SSH csatlakozzon. hello előző példa hello élcsomópont használja. Ez a bejegyzés konfigurálja az SSH-ügynöktovábbítást a megadott csomópont hello.

3. Tesztelje az SSH-ügynöktovábbítást a következő parancsot a Terminálszolgáltatások hello hello:

        echo "$SSH_AUTH_SOCK"

    Ez a parancs visszaadja a szöveg a következő információk hasonló toohello:

        /tmp/ssh-rfSUL1ldCldQ/agent.1792

    Ha a parancs nem ad vissza semmit, a(z) `ssh-agent` nem fut. További információkért lásd: hello ügynök indítási parancsfájlok információt [ssh-agent használata az ssh (http://mah.everybody.org/docs/ssh)](http://mah.everybody.org/docs/ssh) vagy az SSH-ügyfél dokumentációjában.

4. Miután ellenőrizte, hogy **ssh-agent** rendszer fut, a következő tooadd használata hello a SSH titkos kulcs toohello ügynök:

        ssh-add ~/.ssh/id_rsa

    Ha a titkos kulcsot egy másik fájl tárolja, cserélje le `~/.ssh/id_rsa` hello elérési toohello fájllal.

5. Csatlakozás toohello peremhálózati fürtcsomóponton vagy átjárócsomópontokkal SSH használatával. Ezután használja az hello SSH parancs tooconnect tooa munkavégző vagy zookeeper csomópont. hello kapcsolat létrejött továbbított hello kulcs használatával.

## <a name="copy-files"></a>Fájlok másolása

Hello `scp` segédprogram használt toocopy fájlok tooand hello fürt egyes csomópontjáról is lehet. Például a következő parancs másolatok hello hello `test.txt` hello helyi rendszer toohello elsődleges átjárócsomópont a könyvtár:

```bash
scp test.txt sshuser@clustername-ssh.azurehdinsight.net:
```

Mivel nincs megadva után hello `:`, hello fájl kerül hello `sshuser` kezdőkönyvtárát.

Példa másolatok hello következő hello `test.txt` hello fájlt `sshuser` kezdőkönyvtár hello elsődleges átjárócsomópont toohello helyi rendszeren:

```bash
scp sshuser@clustername-ssh.azurehdinsight.net:test.txt .
```

> [!IMPORTANT]
> `scp`hello fájlrendszer hello fürtön belül az egyes csomópontok csak férhetnek hozzá. Nem lehet adatokat használt tooaccess hello fürt tárolóhelyét hello HDFS-kompatibilis.
>
> Használjon `scp` amikor kell tooupload erőforrás SSH-munkamenetet való használatra. Például egy Python-parancsfájl töltse fel, és futtassa a hello parancsfájl az SSH-munkamenetet.
>
> A közvetlenül az adatok betöltését hello HDFS-kompatibilis tárolási információkért lásd: a következő dokumentumok hello:
>
> * [Az Azure Storage-et használó HDInsight](hdinsight-hadoop-use-blob-storage.md).
>
> * [Az Azure Data Lake Store-t használó HDInsight](hdinsight-hadoop-use-data-lake-store.md).

## <a name="next-steps"></a>Következő lépések

* [SSH-alagútkezelés használata a HDInsighttal](hdinsight-linux-ambari-ssh-tunnel.md)
* [Virtuális hálózat használata a HDInsighttal](hdinsight-extend-hadoop-virtual-network.md)
* [Élcsomópontok használata a HDInsightban](hdinsight-apps-use-edge-node.md#access-an-edge-node)