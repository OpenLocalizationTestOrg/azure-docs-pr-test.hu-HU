---
title: "-emulátor - Azure HDInsight Hadoop védőfalat használatával aaaLearn |} Microsoft Docs"
description: "learning használatáról toostart hello Hadoop ökoszisztémájának, állíthatja be a Hadoop védőfalak Hortonworks az Azure virtuális géphez. "
keywords: "hadoop-emulátor, hadoop védőfal"
editor: cgronlun
manager: jhubbard
services: hdinsight
author: nitinme
documentationcenter: 
tags: azure-portal
ms.assetid: 6ad5bb58-8215-4e3d-a07f-07fcd8839cc6
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 91e74f0823fd02e9bb812155a7d09357a77b0736
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-a-hadoop-sandbox-an-emulator-on-a-virtual-machine"></a>Ismerkedés a Hadoop védőfalat, az emulátor egy virtuális gépen

Ismerje meg, hogyan tooinstall hello Hadoop védőfal a Hortonworks meg a virtuális gép toolearn hello Hadoop ökoszisztémájának kapcsolatban. hello védőfal biztosít egy helyi fejlesztési környezet toolearn Hadoop, a Hadoop elosztott fájlrendszerrel (HDFS) és a feladat elküldése. Ha ismeri a Hadoop, megkezdheti a Hadoop használatával az Azure HDInsight-fürtök létrehozásával. A tooget indításának további információkért lásd: [beolvasása használatába a HDInsight Hadoop](hdinsight-hadoop-linux-tutorial-get-started.md).

## <a name="prerequisites"></a>Előfeltételek
* [Oracle VirtualBox](https://www.virtualbox.org/). Töltse le és telepítse azt [Itt](https://www.virtualbox.org/wiki/Downloads).



## <a name="download-and-install-hello-virtual-machine"></a>Töltse le és telepítse a virtuális gép hello
1. Keresse meg a toohello [Hortonworks letölti](http://hortonworks.com/downloads/#sandbox).

2. Kattintson a **töltse le a VIRTUALBOX** toodownload hello legújabb Hortonworks védőfal a virtuális gép. A Hortonworks rákérdezéses tooregister hello letöltési megkezdése előtt áll. A hálózat sebességétől függően egy tootwo óra toodownload vesz igénybe.
   
    ![Hivatkozás kép VirtualBox a Hortonworks védőfal letöltése](./media/hdinsight-hadoop-emulator-get-started/download-sandbox.png)
3. Az azonos weblapon hello, kattintson a hello **importálás virtuális mezőbe az** toodownload hello virtuális gép telepítési utasításokat tartalmazó PDF-fájl csatolása.

egy régebbi HDP verzió védőfal toodownload bontsa ki a hello archív:

![Hortonworks védőfal archív](./media/hdinsight-hadoop-emulator-get-started/hortonworks-sandbox-archive.png)


## <a name="start-hello-virtual-machine"></a>Hello virtuális gép elindítása

1. Nyissa meg az Oracle VM VirtualBox.
2. A hello **fájl** menüben kattintson a **importálási készülék**, majd adja meg a hello Hortonworks védőfal kép.
1. Jelölje be hello Hortonworks védőfal, kattintson a **Start**, majd **normál Start**. Miután hello virtuális gép hello rendszerindítási folyamat befejeződött, bejelentkezési utasítások jeleníti meg.
   
    ![Normál indítása](./media/hdinsight-hadoop-emulator-get-started/normal-start.png)
2. Nyisson meg egy webböngészőt, és keresse meg a toohello URL-cím jelenik meg (általában http://127.0.0.1:8888).

## <a name="set-sandbox-passwords"></a>A védőfal jelszavak beállítása

1. A hello **Ismerkedés** hello Hortonworks védőfal lapon válassza ki a lépés **nézet speciális beállítások**. A lap toolog az SSH használatával toohello védőfal hello információkat használja. Hello nevet és jelszót adott meg használja.
   
   > [!NOTE]
   > Ha nincs telepítve egy SSH-ügyfél, használhatja a hello a virtuális gép a megadott webes SSH hello **http://localhost:4200 /**.
   > 
   
    hello először létesítsen SSH-áll felszólító toochange hello hello root fiókjának jelszavát. Adjon meg egy új jelszót, amelyet használhat, amikor bejelentkezik az SSH használatával.

2. Miután bejelentkezett, adja meg a következő parancs hello:
   
        ambari-admin-password-reset
   
    Amikor a rendszer kéri, adja meg hello Ambari rendszergazdai fiók jelszavát. Ez használható hello Ambari webes felhasználói felületén elérésekor.

## <a name="use-hive-commands"></a>Hive-parancsok használata

1. Egy SSH kapcsolat toohello védőfalak használja a következő parancs toostart hello Hive rendszerhéjat hello:
   
        hive
2. Amikor hello rendszerhéj elindult, használja a következő tooview hello táblák hello védőfal által biztosított hello:
   
        show tables;
3. Tooretrieve 10 sorok követően – hello használata hello `sample_07` tábla:
   
        select * from sample_07 limit 10;

## <a name="next-steps"></a>Következő lépések
* [Megtudhatja, hogyan hello Hortonworks védőfal a Visual Studio toouse](hdinsight-hadoop-emulator-visual-studio.md)
* [Learning hello drótkötelek a hello Hortonworks védőfal](http://hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
* [Hadoop oktatóanyag – első lépések HDP](http://hortonworks.com/hadoop-tutorial/hello-world-an-introduction-to-hadoop-hcatalog-hive-and-pig/)

