---
title: "az R Server on HDInsight - Azure az Rstudióból aaaInstall |} Microsoft Docs"
description: "Hogyan tooinstall az R Server on HDInsight az Rstudióból."
services: hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 918abb0d-8248-4bc5-98dc-089c0e007d49
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/19/2017
ms.author: bradsev
ms.openlocfilehash: b3a23021fcf99217e8f551f8b2e89bf1f1e5b967
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="installing-rstudio-with-r-server-on-hdinsight"></a>Az R Server on HDInsight az Rstudióból telepítése

Ez a cikk ismerteti, hogyan tooinstall hello community (ingyenes) verzióját [Rstudióból Server](https://www.rstudio.com/products/rstudio-server/) hello peremhálózati egy fürt csomópontján egyéni parancsfájl használatával. Rstudióból kiszolgáló távoli ügyfelek számára biztosít egy webböngésző-alapú IDE, és Linux széles körben használható. Van több integrált fejlesztési környezet (IDEs) érhető el, az R ma, beleértve:

- A Microsoft [R Tools for Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS) 
- [Rstudióból kiszolgáló](https://www.rstudio.com/products/rstudio-server/) 
- Walware tartozó Eclipse-alapú [StatET](http://www.walware.de/goto/statet).

hello Rstudióból kiszolgáló telepítése egy HDInsight-fürt hello peremhálózati csomóponton előnye, hogy az teljes IDE élményt nyújt hello fejlesztési és az R parancsfájlok végrehajtása az R Server hello fürtön. Lehet, hogy ez a konfiguráció jelentősen hatékonyabb, mint hello R konzol alapértelmezett használatát.

> [!NOTE]
> Ebben a cikkben leírt hello eljárás csak akkor megfelelő, ha nem jelölte be tooinstall Rstudióból Server community edition a fürt kiépítésekor. Ha hozzáadott telepítése során, majd, kattintson a hello tooaccess **R Server irányítópultok** csempe az Azure portál bejegyzés a fürt számára, majd a hello hello **R Studio Server** csempére. 

Toouse kereskedelmileg licencelt hello Pro Rstudióból Server verziója tetszés szerint hajtsa végre az hello telepítési utasításokat a [Rstudióból Server](https://www.rstudio.com/products/rstudio/download-server/).

> [!NOTE]
> Egy HDInsight-fürt, amelynek R telepítette hello használata [R parancsfájlművelet telepítése](hdinsight-hadoop-r-scripts-linux.md), nem működnek a jelen dokumentumban leírt lépések hello igényelnek-e az R Server on HDInsight-fürt hello megfelelően.
>
> 

## <a name="prerequisites"></a>Előfeltételek

* R Server telepített Azure HDInsight-fürtöt. Útmutatásért lásd: [az R Server első lépései a HDInsight-fürtök](hdinsight-hadoop-r-server-get-started.md).
* Egy SSH-ügyfél. A Linux és Unix megfelelő kiadásának vagy Macintosh-OS X, hello `ssh` parancs hello operációs rendszer által biztosított. A Windows, ajánlott [Cygwin](http://www.redhat.com/services/custom/cygwin/) a hello [OpenSSH beállítás](https://www.youtube.com/watch?v=CwYSvvGaiWU), vagy [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).  

## <a name="install-rstudio-on-hello-cluster-using-a-custom-script"></a>Egyéni parancsfájl használata hello fürtön telepíteni a Rstudióból

Az alábbiakban hello lépéseket:

1. Hello élcsomópont hello fürt azonosításához. A HDInsight-fürtök az R Server az alábbiakban az átjárócsomópont és élcsomópont hello elnevezési.
   * Átjárócsomópont`CLUSTERNAME-ssh.azurehdinsight.net`
   * Élcsomópont`CLUSTERNAME-ed-ssh.azurehdinsight.net` 

2. SSH-ból hello élcsomópont hello fürt hello elnevezési minta az 1. lépésben megadott használatával. További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

3. Miután csatlakozott, vált a gyökér szintű felhasználó hello fürtön. Hello SSH-munkamenetet, a parancs a következő hello használata:

        sudo su -

4. Töltse le a hello egyéni parancsfájl tooinstall Rstudióból. A következő parancs hello használata:

        wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/InstallRStudio.sh

5. Egyéni parancsfájl hello hello engedélyeinek módosítása, és hello parancsprogrammal. A következő parancsok hello használata:

        chmod 755 InstallRStudio.sh
        ./InstallRStudio.sh

6. R Server a HDInsight-fürtök létrehozásakor egy SSH-jelszó használata esetén ezt a lépést kihagyhatja, és folytassa tovább toohello. Ha SSH-kulcs helyette toocreate hello fürtöt, meg kell adni az SSH-felhasználó jelszót. Ezt a jelszót kell tooRStudio kapcsolódáskor. Futtassa a következő parancsok hello:

        passwd USERNAME
        Current Kerberos password:
        New password:
        Retype new password:
        Current Kerberos password:


7. Ha a rendszer kéri **aktuális Kerberos jelszó**, nyomja le az ENTER **ENTER**.  Vegye figyelembe, hogy le kell cserélnie `USERNAME` rendelkező a HDInsight-fürthöz az SSH-felhasználó. Ha sikeresen beállította a jelszavát, a következő üzenet hello kell megjelennie:

        passwd: password updated successfully

    Lépjen ki a hello SSH-munkamenetet.

8. Hozzon létre egy SSH-alagút toohello fürt leképezési `ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` hello HDInsight fürtön toohello ügyfélszámítógépen. Az SSH-alagút létre kell hoznia egy új böngésző-munkamenet megnyitása előtt.

   * A Linux-ügyfél vagy egy Windows ügyfél [Cygwin](http://www.redhat.com/services/custom/cygwin/)nyisson meg egy terminál-munkamenetet, és használja a következő parancs hello:

             ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

       Cserélje le **felhasználónév** az SSH-felhasználó a HDInsight-fürtöt, és cserélje le a **CLUSTERNAME** hello nevet, a HDInsight-fürthöz.
       Használhatja a jelszó helyett egy SSH-kulcs hozzáadásával `-i id_rsa_key`.        
   * Ha a Windows-ügyfél majd a PuTTY használatával

     1. Nyissa meg a PuTTY, és adja meg a kapcsolódási adatokat.
     2. A hello **kategória** szakasz toohello hello párbeszédpanel bal oldali, bontsa ki a **kapcsolat**, bontsa ki a **SSH**, majd válassza ki **alagutak**.
     3. Adja meg a következő információ a hello hello **vezérlő beállításokban SSH port átirányítása** űrlap:

        * **Forrásport** -port, hogy kívánja-e tooforward hello ügyfélen hello. Például **8787**.
        * **Cél** – hello kell lennie a cél leképezése toohello helyi ügyfélszámítógépen. Például **localhost:8787**.

            ![SSH-alagút létrehozása](./media/hdinsight-hadoop-r-server-install-r-studio/createsshtunnel.png "SSH-alagút létrehozása")

     4. Kattintson a **Hozzáadás** tooadd hello beállításait, és kattintson a **nyitott** tooopen az SSH-kapcsolat.
     5. Amikor a rendszer kéri, jelentkezzen be toohello server tooestablish SSH munkamenet és engedélyezése hello-alagutat.

9. Nyisson meg egy webböngészőt, és írja be a következő URL-cím a megadott hello alagúthoz hello port alapján hello:

        http://localhost:8787/ 

10. Rákérdezéses tooenter hello SSH felhasználónév és jelszó tooconnect toohello fürt áll. Ha SSH-kulcs hello fürt létrehozása során, 5. lépésben létrehozott hello jelszót kell megadnia.

    ![Csatlakozás bemutató Studio](./media/hdinsight-hadoop-r-server-install-r-studio/connecttostudio.png "SSH-alagút létrehozása")

11. tootest hello Rstudióból telepítése sikeres volt, akkor futtatható-e a teszt parancsfájl, amely végrehajtja az R-alapú MapReduce és Spark feladatok hello fürtön. toodownload hello teszt parancsfájl toorun az Rstudióból, lépjen vissza toohello SSH-konzolon, és adja meg a következő parancsok hello:

    *    A Hadoop fürtök R hozta létre, ha az alábbi parancsot használja:

            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi.r
    *    Ha létrehozott egy Spark-fürt az R, az alábbi parancsot használja:

            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi_spark.r

12. Az Rstudióból lásd: hello letöltött parancsprogram teszteléséhez. Hello fájl tooopen, jelölje ki a hello hello fájl tartalmát, és kattintson duplán **futtatása**. A hello hello kimenetet kell látnia **konzol** panelen:

   ![Hello-telepítés sikerességének ellenőrzése](./media/hdinsight-hadoop-r-server-install-r-studio/test-r-script.png "hello-telepítés sikerességének ellenőrzése")

Egy másik lehetőség lenne tootype `source(testhdi.r)` vagy `source(testhdi_spark.r)` tooexecute hello parancsfájl.

## <a name="see-also"></a>Lásd még:

* [Számítási környezet lehetőségek R Server a HDInsight-fürtökön](hdinsight-hadoop-r-server-compute-contexts.md)
* [Azure Storage lehetőségek a HDInsighton belüli R Server esetében](hdinsight-hadoop-r-server-storage.md)

