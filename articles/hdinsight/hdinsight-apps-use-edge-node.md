---
title: "aaaUse üres Hadoop-fürtöket a HDInsight - Azure peremhálózati csomópontok |} Microsoft Docs"
description: "Hogyan tooadd egy üres biztonsági csomópont tooan HDInsight fürt, amely ügyfélként is használható, és ezután vizsgálati és a gazdagép a HDInsight-alkalmazások."
services: hdinsight
editor: cgronlun
manager: jhubbard
author: mumian
tags: azure-portal
documentationcenter: 
ms.assetid: cdc7d1b4-15d7-4d4d-a13f-c7d3a694b4fb
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: jgao
ms.openlocfilehash: 9c910905b51f2fe92e6e5d47d86a32bd5247c2cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-empty-edge-nodes-on-hadoop-clusters-in-hdinsight"></a>Üres peremhálózati csomópontok használata a hdinsight Hadoop-fürtök

Ismerje meg, hogyan tooadd egy üres él csomópont tooan HDInsight-fürthöz. Egy üres élcsomópontot hello ugyanazon ügyfél-eszközök telepítve, és mint hello headnodes konfigurálva, de nem Hadoop-szolgáltatást futtató Linux virtuális gép. Hello fürt eléréséhez, az ügyfél alkalmazások tesztelése és az ügyfélalkalmazások üzemeltető hello élcsomópontot is használhatja. 

Hozzáadhat egy üres biztonsági csomópont tooan meglévő HDInsight-fürtre, tooa új fürt hello fürt létrehozásakor. Történik, egy üres élcsomópontot hozzáadása Azure Resource Manager-sablon használatával.  hello következő példa bemutatja, hogyan történik a sablon használatával:

    "resources": [
        {
            "name": "[concat(parameters('clusterName'),'/', variables('applicationName'))]",
            "type": "Microsoft.HDInsight/clusters/applications",
            "apiVersion": "2015-03-01-preview",
            "dependsOn": [ "[concat('Microsoft.HDInsight/clusters/',parameters('clusterName'))]" ],
            "properties": {
                "marketPlaceIdentifier": "EmptyNode",
                "computeProfile": {
                    "roles": [{
                        "name": "edgenode",
                        "targetInstanceCount": 1,
                        "hardwareProfile": {
                            "vmSize": "Standard_D3"
                        }
                    }]
                },
                "installScriptActions": [{
                    "name": "[concat('emptynode','-' ,uniquestring(variables('applicationName')))]",
                    "uri": "[parameters('installScriptAction')]",
                    "roles": ["edgenode"]
                }],
                "uninstallScriptActions": [],
                "httpsEndpoints": [],
                "applicationType": "CustomApplication"
            }
        }
    ],

Mintában látható módon hello, opcionálisan hívása egy [parancsfájl-művelet](hdinsight-hadoop-customize-cluster-linux.md) tooperform további konfigurálást, mint például telepítése [Apache Hue](hdinsight-hadoop-hue-linux.md) hello peremhálózati csomópontban. hello parancsfájl parancsfájlművelet nyilvánosan elérhető hello weben kell lennie.  Például ha az Azure storage hello parancsfájl, használja a nyilvános tárolók vagy nyilvános blobok.

hello peremhálózati csomópont virtuális gép méretének meg kell felelnie az hello HDInsight fürt munkavégző csomópont virtuális gép. Hello ajánlott munkavégző csomópont virtuálisgép-méretek, lásd: [Hadoop létrehozása a HDInsight-fürtök](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).

Miután létrehozott egy élcsomópontot, toohello élcsomópontjához SSH csatlakozzon, és futtassa az ügyfelet eszközök tooaccess hello Hadoop-fürt hdinsightban.

> [!WARNING] 
> Egy üres élcsomópontot használata a HDInsight jelenleg előzetes verzió. Egyéni összetevők hello élcsomópont telepített minden üzleti szempontból ésszerű támogatást kaphatnak a Microsofttól. Emiatt előfordulhat, hogy a felmerülő problémák elhárításához. Vagy előfordulhat, hogy a hivatkozott toocommunity erőforrások további segítségért. hello közé tartoznak a következők a legtöbb aktív helyek kapcsolódnak a Súgó hello Közösségtől hello:
>
> * [A HDInsight MSDN fórum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight)
> * [http://stackoverflow.com](http://stackoverflow.com).
>
> Ha egy Apache-okat használ, is képes toofind segítséget hello Apache projekt helyeket a [http://apache.org](http://apache.org), például a hello [Hadoop](http://hadoop.apache.org/) hely.

## <a name="add-an-edge-node-tooan-existing-cluster"></a>Peremhálózati csomópont tooan meglévő fürt hozzáadása
Ebben a szakaszban egy Resource Manager sablon tooadd peremhálózati csomópont tooan meglévő HDInsight-fürtöt használ.  hello Resource Manager-sablon található [GitHub](https://azure.microsoft.com/en-us/resources/templates/101-hdinsight-linux-add-edge-node/). hello Resource Manager-sablon meghívja a parancsfájlművelet https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-add-edge-node/scripts/EmptyNodeSetup.sh helyen. hello parancsfájlok nem hajthatnak végre semmilyen műveletet.  A Resource Manager-sablon parancsfájlművelet előhívásának toodemonstrate.

**üres peremhálózati csomópont tooan meglévő fürt tooadd**

1. HDInsight-fürtök létrehozása, ha még nincs ilyen.  Lásd: [Hadoop oktatóanyag: Hadoop használatának megkezdésében a HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
2. Kattintson a következő kép toosign tooAzure a és a nyitott hello Azure Resource Manager sablon hello Azure-portálon hello. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-add-edge-node%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apps-use-edge-node/deploy-to-azure.png" alt="Deploy tooAzure"></a>
3. A következő tulajdonságok hello konfigurálása:
   
   * **Előfizetés**: válassza ki a hello fürt létrehozásához használt Azure-előfizetéssel.
   * **Erőforráscsoport**: hello meglévő HDInsight-fürthöz használt válassza hello erőforráscsoportot.
   * **Hely**: Válasszon hello meglévő HDInsight-fürt hello helyet.
   * **A fürt neve**: Adjon meg egy meglévő HDInsight-fürt hello nevet.
   * **A szegély csomópont méretének**: Válasszon ki egy Virtuálisgép-méretek hello. Virtuálisgép-méretet hello meg kell felelnie az hello munkavégző csomópont virtuális gép. Hello ajánlott munkavégző csomópont virtuálisgép-méretek, lásd: [Hadoop létrehozása a HDInsight-fürtök](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).
   * **A szegély csomópont előtag**: hello alapértelmezett értéke **új**.  Hello alapértelmezett értéket, hello peremhálózati csomópontnév használata **új edgenode**.  Testre szabhatja a hello előtag hello portálról. Testre szabhatja hello teljes nevét hello sablonból.

4. Ellenőrizze **toohello feltételek és kikötések fenti elfogadom**, és kattintson a **beszerzési** toocreate hello élcsomópont.

>[!IMPORTANT]
> Győződjön meg arról, hogy tooselect hello Azure erőforráscsoport hello meglévő HDInsight-fürthöz.  Ellenkező esetben akkor a hibaüzenet hello üzenet "nem hajtható végre a kért művelet beágyazott erőforráson. Szülő erőforrás "&lt;ClusterName >" nem található. "

## <a name="add-an-edge-node-when-creating-a-cluster"></a>Adja hozzá egy élcsomópontot, ha a fürt létrehozása
Ebben a szakaszban egy Resource Manager sablon toocreate HDInsight-fürt egy élcsomópontot és használja.  hello Resource Manager-sablon hello található [Azure gyors üzembe helyezés sablontárban](https://azure.microsoft.com/documentation/templates/101-hdinsight-linux-with-edge-node/). hello Resource Manager-sablon meghívja a parancsfájlművelet https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-with-edge-node/scripts/EmptyNodeSetup.sh helyen. hello parancsfájlok nem hajthatnak végre semmilyen műveletet.  A Resource Manager-sablon parancsfájlművelet előhívásának toodemonstrate.

**üres peremhálózati csomópont tooan meglévő fürt tooadd**

1. HDInsight-fürtök létrehozása, ha még nincs ilyen.  Lásd: [Hadoop használatának megkezdésében a HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
2. Kattintson a következő kép toosign tooAzure a és a nyitott hello Azure Resource Manager sablon hello Azure-portálon hello. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-edge-node%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apps-use-edge-node/deploy-to-azure.png" alt="Deploy tooAzure"></a>
3. A következő tulajdonságok hello konfigurálása:
   
   * **Előfizetés**: válassza ki a hello fürt létrehozásához használt Azure-előfizetéssel.
   * **Erőforráscsoport**: hozzon létre egy új erőforráscsoportot hello fürt használja.
   * **Hely**: hello erőforráscsoport helyének kiválasztására.
   * **A fürt neve**: hello új fürt toocreate nevét adja meg.
   * **A fürt bejelentkezési felhasználónevét**: hello Hadoop HTTP-felhasználó nevét adja meg.  hello alapértelmezés szerint ez **admin**.
   * **A fürt bejelentkezési jelszó**: hello Hadoop HTTP felhasználói jelszó.
   * **Ssh felhasználónév**: Adja meg a hello SSH-felhasználónév. hello alapértelmezés szerint ez **sshuser**.
   * **Ssh jelszó**: hello SSH felhasználói jelszó.
   * **Telepítse a parancsfájlművelet**: hello alapértelmezett értéke az oktatóanyag lépéseinek megtartása.
     
     Néhány tulajdonság lett volna hello sablonban szoftveresen kötött: fürt típusa, a fürt feldolgozó csomópontok száma, a peremhálózati csomópont mérete és a peremhálózati csomópont neve.
4. Ellenőrizze **toohello feltételek és kikötések fenti elfogadom**, és kattintson a **beszerzési** toocreate hello fürt hello élcsomópont.

## <a name="access-an-edge-node"></a>Hozzáférés egy élcsomópontot
hello élcsomópont ssh-végpont esetében &lt;EdgeNodeName >.&lt; ClusterName >-ssh.azurehdinsight.net:22.  Például új-edgenode.myedgenode0914-ssh.azurehdinsight.net:22.

hello élcsomópont hello Azure-portál alkalmazás jelenik meg.  hello portál által biztosított információk tooaccess hello hello meg az SSH használatával csomópont él.

**tooverify hello peremhálózati csomópont SSH végpont**

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Nyisson meg egy élcsomópontot hello HDInsight-fürthöz.
3. Kattintson a **alkalmazások** hello fürt paneljén. Hello élcsomópont megjelenik.  hello alapértelmezés szerint ez **új edgenode**.
4. Kattintson a hello élcsomópont. Ekkor megjelenik a hello SSH-végpontot.

**toouse Hive hello peremhálózati csomóponton**

1. SSH tooconnect toohello élcsomópont használja. További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Toohello élcsomópontjához SSH kapcsolódás után használja a következő parancs tooopen hello Hive konzol hello:
   
        hive
3. Futtassa a következő parancs tooshow Hive táblák hello fürt hello:
   
        show tables;

## <a name="delete-an-edge-node"></a>Egy élcsomópontot törlése
Egy élcsomópontot hello Azure-portálon törölheti.

**egy élcsomópontot tooaccess**

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Nyisson meg egy élcsomópontot hello HDInsight-fürthöz.
3. Kattintson a **alkalmazások** hello fürt paneljén. Peremhálózati lista megjelenik.  
4. Kattintson a jobb gombbal hello élcsomópont toodelete szeretne, és kattintson a **törlése**.
5. Kattintson a **Igen** tooconfirm.

## <a name="next-steps"></a>Következő lépések
Ebben a cikkben megtanulta, hogyan tooadd egy élcsomópontot, és hogyan tooaccess hello élcsomópont. toolearn több, tekintse meg a következő cikkek hello:

* [HDInsight-alkalmazások telepítése](hdinsight-apps-install-applications.md): megtudhatja, hogyan tooinstall egy HDInsight-alkalmazás tooyour fürtök.
* [Egyéni HDInsight-alkalmazások telepítése](hdinsight-apps-install-custom-applications.md): megtudhatja, hogyan toodeploy egy közzé nem tett HDInsight-alkalmazás tooHDInsight.
* [HDInsight-alkalmazások közzététele](hdinsight-apps-publish-applications.md): megtudhatja, hogyan toopublish az egyéni HDInsight-alkalmazások tooAzure piactéren.
* [MSDN: HDInsight-alkalmazások telepítése](https://msdn.microsoft.com/library/mt706515.aspx): megtudhatja, hogyan toodefine HDInsight-alkalmazások.
* [Parancsfájlművelet Linux-alapú HDInsight-fürtök testreszabása](hdinsight-hadoop-customize-cluster-linux.md): megtudhatja, hogyan toouse parancsfájlművelet tooinstall további alkalmazásokat.
* [Linux-alapú Hadoop-fürtök létrehozása a Resource Manager-sablonok használatával HDInsight](hdinsight-hadoop-create-linux-clusters-arm-templates.md): megtudhatja, hogyan toocall Resource Manager sablonok toocreate HDInsight-fürtök.

