---
title: "aaaUse SSH-bújtatás tooaccess Azure HDInsight |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse egy SSH-alagút toosecurely keresse meg a Linux-alapú HDInsight-csomópontok webes erőforrásaihoz."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 879834a4-52d0-499c-a3ae-8d28863abf65
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/21/2017
ms.author: larryfr
ms.openlocfilehash: 8a315c53751bbe6950a182674f4108c67c2f2f8e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-ssh-tunneling-tooaccess-ambari-web-ui-jobhistory-namenode-oozie-and-other-web-uis"></a>SSH Tunneling tooaccess Ambari webes felhasználói felület, JobHistory, NameNode, Oozie és egyéb web UI használata

Linux-alapú HDInsight-fürtök biztosítanak hozzáférést tooAmbari webes felhasználói felület hello interneten keresztül, de néhány hello UI-funkciók nem állnak. Például hello webes felhasználói felületen keresztül Ambari illesztett más szolgáltatásokhoz. Hello Ambari webes felhasználói felület funkcióját egy SSH-alagút toohello fürt head kell használnia.

## <a name="why-use-an-ssh-tunnel"></a>Miért érdemes használni az SSH-alagút

Az Ambari hello menük számos csak támogatott eszközplatformokon működnek az SSH-alagúton keresztül. Ezek a menük webhelyek és az egyéb csomóponttípusok, például a feldolgozó csomópontok szolgáltatás támaszkodnak. Gyakran ezek a webhelyek nem biztonságosak, így azt nem teszi közzé a biztonságos toodirectly azokat a hello internet.

a következő Web UI hello SSH-alagút megkövetelése:

* JobHistory
* NameNode
* A szál verem
* Oozie webes felhasználói felület
* A HBase Master és a naplókat a felhasználói felület

A Parancsfájlműveletek toocustomize használatakor a fürt lehetnek szolgáltatások vagy a segédeszközök telepítése a webes felhasználói felület visszaállítását SSH-alagút szükséges. Például ha egy parancsfájl műveletével Hue telepítése kell használnia egy SSH alagút tooaccess hello Hue webes felhasználói felület.

> [!IMPORTANT]
> Ha egy virtuális hálózaton keresztül a közvetlen hozzáférést tooHDInsight, nem kell toouse SSH-alagutat. Közvetlenül a virtuális hálózaton keresztül férnek hozzá a HDInsight példáért lásd: hello [csatlakozás HDInsight tooyour a helyszíni hálózat](connect-on-premises-network.md) dokumentum.

## <a name="what-is-an-ssh-tunnel"></a>Mi az az SSH-alagút

[Secure Shell (SSH) bújtatás](https://en.wikipedia.org/wiki/Tunneling_protocol#Secure_Shell_tunneling) irányítja a forgalmat tooa portot a helyi munkaállomáson. hello forgalmat egy SSH kapcsolat tooyour HDInsight fürt átjárócsomópontjába keresztül történik. hello kérelem meg van oldva, mintha hello átjárócsomópont származna. hello válasz majd vissza hello alagút tooyour munkaállomás keresztül történik.

## <a name="prerequisites"></a>Előfeltételek

* Egy SSH-ügyfél. További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

* Egy olyan webböngésző, lehet toouse SOCKS5 proxy konfigurálva.

    > [!WARNING]
    > beépített Windows hello SOCKS proxy támogatási SOCKS5 nem támogatja, és nem működik a jelen dokumentumban leírt lépések hello. hello alábbi böngészők Windows proxybeállítások alapulnak, és tegye jelenleg nem a jelen dokumentumban leírt lépések hello használata:
    >
    > * A Microsoft Edge
    > * Microsoft Internet Explorer
    >
    > Google Chrome hello Windows proxybeállítások is támaszkodik. Bővítmények SOCKS5 támogató is telepítheti. Ajánlott [FoxyProxy Standard](https://chrome.google.com/webstore/detail/foxyproxy-standard/gcknhkkoolaabfmlnjonogaaifnjlfnp).

## <a name="usessh"></a>Hozzon létre egy alagúton hello SSH-parancs használatával

Használjon hello következő parancsot a toocreate az SSH-alagút segítségével hello `ssh` parancsot. Cserélje le **felhasználónév** az SSH-felhasználó a HDInsight-fürtöt, és cserélje le a **CLUSTERNAME** hello nevet, a HDInsight-fürt:

```bash
ssh -C2qTnNf -D 9876 USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
```

Ez a parancs kapcsolatot hoz létre, amely forgalmat toolocal port 9876 toohello fürthöz SSH-n keresztül. hello lehetőségek közül választhat:

* **D 9876** -hello helyi port irányítja a forgalmat hello alagúton keresztül.
* **C** -tömörítése minden adatot, mert a webes forgalom leginkább szöveget.
* **2** -force SSH tootry protokoll 2-es verziójú csak.
* **a q** -csendes mód.
* **T** -pszeudo-tty kiosztását, tiltsa le, mivel jelenleg csupán továbbít egy portot.
* **n**– Előfordulhat, hogy olvasása STDIN, mivel jelenleg csupán továbbít egy portot.
* **N** -ne hajtsa végre a távoli parancstól, mivel jelenleg csupán továbbít egy portot.
* **f** -hello háttérben futnak.

Miután hello parancs elküldött forgalom tooport 9876 hello helyi számítógépen irányított toohello átjárócsomóponthoz.

## <a name="useputty"></a>A PuTTY használatával-alagút létrehozása

[A puTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty) egy grafikus SSH-ügyfél Windows. A következő lépéseket toocreate használja a PuTTY SSH-alagút hello használata:

1. Nyissa meg a PuTTY, és adja meg a kapcsolódási adatokat. Ha nem ismeri a PuTTY, lásd: hello [dokumentáció (http://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html) PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html).

2. A hello **kategória** szakasz toohello hello párbeszédpanel bal oldali, bontsa ki a **kapcsolat**, bontsa ki a **SSH**, majd válassza ki **alagutak**.

3. Adja meg a következő információ a hello hello **vezérlő beállításokban SSH port átirányítása** űrlap:
   
   * **Forrásport** -port, hogy kívánja-e tooforward hello ügyfélen hello. Például **9876**.

   * **Cél** -hello hello Linux-alapú HDInsight-fürthöz az SSH-cím. Például: **mycluster-ssh.azurehdinsight.net**.

   * **Dynamic** (Dinamikus) - Lehetővé teszi a dinamikus SOCKS proxy útválasztást.
     
     ![beállítások tunneling képe](./media/hdinsight-linux-ambari-ssh-tunnel/puttytunnel.png)

4. Kattintson a **Hozzáadás** tooadd hello beállításait, és kattintson a **nyitott** tooopen az SSH-kapcsolat.

5. Amikor a rendszer kéri, jelentkezzen be toohello kiszolgáló.

## <a name="use-hello-tunnel-from-your-browser"></a>A böngészőből hello alagút használata

> [!IMPORTANT]
> hello lépések Ez a szakasz használata hello Mozilla FireFox böngésző biztosít hello proxybeállításokat minden platformon. Egyéb modern böngésző támogatja, például a Google Chrome, például a hello alagút FoxyProxy toowork bővítmény lehet szükség.

1. Konfigurálhatja a böngésző toouse hello **localhost** és hello port használhatók, ha hello alagút, létre kell hozni egy **SOCKS v5** proxy. Ez hogyan meg hello Firefox beállításait. Ha 9876-as, módosítsa a hello port toohello egy használt:
   
    ![a Firefox beállításai](./media/hdinsight-linux-ambari-ssh-tunnel/firefoxproxy.png)
   
   > [!NOTE]
   > Kiválasztása **távoli DNS** oldja fel a tartománynévrendszer (DNS) kérelmek hello HDInsight-fürt használatával. Ez a beállítás hello fürt átjárócsomópontjához hello segítségével oldja fel.

2. Ellenőrizze, hogy hello alagút működik, mint egy webhely felkeresésével [http://www.whatismyip.com/](http://www.whatismyip.com/). hello IP visszaadott egyik használják hello Microsoft Azure datacenter.

## <a name="verify-with-ambari-web-ui"></a>Ellenőrizze a következővel Ambari webes felhasználói felület

Hello fürt létrehozása után használja a következő lépéseket tooverify, hogy elérhető szolgáltatás web UI hello Ambari Web hello:

1. A böngészőben nyissa meg toohttp://headnodehost:8080. Hello `headnodehost` cím hello alagút toohello fürt átküldött, és oldja meg, hogy fut-Ambari toohello headnode. Amikor a rendszer kéri, adja meg (rendszergazda) hello rendszergazda felhasználónevét és jelszavát a fürt számára. Kérheti másodszor hello Ambari webes felhasználói felület. Ha igen, adja meg újból hello információkat.

   > [!NOTE]
   > Hello http://headnodehost:8080 cím tooconnect toohello fürt használatakor hello alagúton keresztül kapcsolódik. Kommunikációs használatával lett biztonságossá téve hello SSH-alagút HTTPS kapcsolat helyett. tooconnect több mint hello internetkapcsolat, és a HTTPS, használja a https://CLUSTERNAME.azurehdinsight.net, ahol **CLUSTERNAME** hello fürt hello neve.

2. Hello Ambari webes felhasználói felületén HDFS hello bal hello lap hello listáról válassza ki.

    ![A kiválasztott HDFS kép](./media/hdinsight-linux-ambari-ssh-tunnel/hdfsservice.png)

3. Amikor hello HDFS-szolgáltatás adatait megjelenik, válassza ki a **Gyorshivatkozások**. Hello központi fürtcsomópontok listája jelenik meg. Hello átjárócsomópontokkal egyikét, majd válassza ki és **NameNode felhasználói felület**.

    ![Hello QuickLinks menü rendszerképének kibontva](./media/hdinsight-linux-ambari-ssh-tunnel/namenodedropdown.png)

   > [!NOTE]
   > Ha bejelöli __Gyorshivatkozások__, várjon, amíg mutató kaphat. Ez az állapot akkor fordulhat elő, ha lassú internetkapcsolattal rendelkezik. Várjon egy percet, amíg hello adatok toobe hello kiszolgálótól kapott, és próbálkozzon újra a hello listája.
   >
   > Néhány elemet a hello **Gyorshivatkozások** menü előfordulhat, hogy elszigeteli hello jobb oldalán üdvözlő képernyőt. Ha igen, bontsa ki az egér használatával hello menü és hello jobbra mutató nyílra kulcs tooscroll hello képernyő toohello jobb toosee hello részeinek hello menü.

4. A kép a következő lap hasonló toohello jelenik meg:

    ![Hello NameNode felhasználói felület képe](./media/hdinsight-linux-ambari-ssh-tunnel/namenode.png)

   > [!NOTE]
   > Figyelje meg ezen a lapon; hello URL-címe meg kell hasonló túl**http://hn1-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8088/fürt**. Ezt az URI hello belső teljesen minősített tartománynevét (FQDN) hello csomópont használ, és csak elérhető az SSH-alagút használatakor.

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta, hogyan toocreate és használja az SSH-alagút: hello más módokon toouse Ambari a dokumentum a következő:

* [A HDInsight-fürtök kezelése az Ambari segítségével](hdinsight-hadoop-manage-ambari.md)

Az SSH és a HDInsight együttes használatával további információkért lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

