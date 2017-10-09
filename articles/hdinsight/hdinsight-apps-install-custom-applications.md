---
title: "aaaInstall a saját egyéni Hadoop-alkalmazások Azure HDInsight |} Microsoft Docs"
description: "Megtudhatja, hogyan tooinstall HDInsight-alkalmazások HDInsight-alkalmazásokra."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: e556b29c-8176-4bc5-a90b-aa01abfd3aee
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: ed3148f2c4d4d2b568d84e44fa6d76bb5a001902
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-custom-hadoop-applications-on-azure-hdinsight"></a>Egyéni Hadoop-alkalmazások telepítése Azure HDInsight platformon

Ebből a cikkből megtudhatja, hogyan tooinstall egy Hadoop-alkalmazás Azure hdinsight, amely nem lett közzétéve toohello Azure-portálon. Ez a cikk telepíteni kívánt hello alkalmazás [Hue](http://gethue.com/).

A HDInsight-alkalmazások olyan alkalmazások, amelyeket a felhasználók egy Linux-alapú HDInsight-fürtre telepíthetnek.  Ezek az alkalmazások lehetnek a Microsoft, független szoftvergyártók (ISV-k) vagy a felhasználók fejlesztései.  

Egyéb kapcsolódó cikkek:

* [HDInsight-alkalmazások telepítése](hdinsight-apps-install-applications.md): megtudhatja, hogyan tooinstall egy HDInsight-alkalmazás tooyour fürtök.
* [HDInsight-alkalmazások közzététele](hdinsight-apps-publish-applications.md): megtudhatja, hogyan toopublish az egyéni HDInsight-alkalmazások tooAzure piactéren.
* [MSDN: HDInsight-alkalmazások telepítése](https://msdn.microsoft.com/library/mt706515.aspx): megtudhatja, hogyan toodefine HDInsight-alkalmazások.

## <a name="prerequisites"></a>Előfeltételek
Ha azt szeretné, hogy egy meglévő HDInsight-fürt a HDInsight-alkalmazások tooinstall, rendelkeznie kell egy HDInsight-fürtre. toocreate, lásd: [fürtöket létrehozni](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster). HDInsight-alkalmazásokat HDInsight-fürt létrehozása közben is telepíthet.

## <a name="install-hdinsight-applications"></a>HDInsight-alkalmazások telepítése
HDInsight-alkalmazások telepíthetők fürtben vagy tooan meglévő HDInsight-fürt létrehozásakor. Azure Resource Manager-sablonok meghatározása: [MSDN: Install an HDInsight application](https://msdn.microsoft.com/library/mt706515.aspx) (MSDN: HDInsight-alkalmazás telepítése).

az alkalmazás (Hue) üzembe helyezéséhez szükséges hello fájlok:

* [azuredeploy.JSON](https://github.com/hdinsight/Iaas-Applications/blob/master/Hue/azuredeploy.json): hello Resource Manager-sablon a HDInsight-alkalmazás telepítésére. Lásd: [MSDN: Install an HDInsight application](https://msdn.microsoft.com/library/mt706515.aspx) (MSDN: HDInsight-alkalmazás telepítése).
* [hue-install_v0.sh](https://github.com/hdinsight/Iaas-Applications/blob/master/Hue/scripts/Hue-install_v0.sh): hello hello élcsomópont konfigurálásához hello Resource Manager-sablon által meghívott parancsfájlművelet.
* [hue-binaries.tgz](https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv01/hue-binaries-14-04.tgz): hello bináris hue-fájl hui-install_v0.sh fájlból.
* [hue-bináris-14-04.tgz](https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv01/hue-binaries-14-04.tgz): hello bináris hue-fájl hui-install_v0.sh fájlból.
* [webwasb-tomcat.tar.gz](https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv01/webwasb-tomcat.tar.gz): A hui-install_v0.sh fájlból meghívott webes mintaalkalmazás (Tomcat).

**tooinstall Hue tooan meglévő HDInsight-fürt**

1. Kattintson a következő kép toosign tooAzure a és a nyitott hello Resource Manager sablon hello Azure Portal hello.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2FIaas-Applications%2Fmaster%2FHue%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apps-install-custom-applications/deploy-to-azure.png" alt="Deploy tooAzure"></a>

    Ezzel a gombbal megnyithatja a Resource Manager-sablon a hello Azure-portálon.  hello Resource Manager-sablon itt található: [https://github.com/hdinsight/Iaas-Applications/tree/master/Hue](https://github.com/hdinsight/Iaas-Applications/tree/master/Hue).  toolearn hogyan toowrite a Resource Manager-sablon, lásd: [MSDN: HDInsight-alkalmazások telepítése](https://msdn.microsoft.com/library/mt706515.aspx).
2. A hello **paraméterek** panelen írja be a következő hello:

   * **ClusterName**: Adja meg hello hello fürt nevét, ahol szeretné tooinstall hello alkalmazás. Ennek létező fürtnek kell lennie.
3. Kattintson a **OK** toosave hello paraméterek.
4. A hello **egyéni telepítési** panelen adjon meg **erőforráscsoport**.  hello erőforráscsoport egy olyan tároló, amely csoportosítja hello fürtöt, hello függő tárfiókot és egyéb erőforrásokat. Szükség rá toouse hello hello fürtként ugyanabban az erőforráscsoportban.
5. Kattintson a **Legal terms** (Jogi feltételek), majd a **Create** (Létrehozás) gombra.
6. Ellenőrizze a hello **PIN-kód toodashboard** jelölőnégyzet van kiválasztva, és kattintson **létrehozása**. Hello csempe rögzített toohello portál Irányítópultjára és hello portál értesítései (hello harang ikonra hello portál hello legfelül) hello telepítési állapotát tekintheti meg.  Körülbelül 10 percig tooinstall hello alkalmazás vesz igénybe.

**Hue tooinstall fürt létrehozása közben**

1. Kattintson a következő kép toosign tooAzure a és a nyitott hello Resource Manager sablon hello Azure Portal hello.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fhdinsightapps%2Fcreate-linux-based-hadoop-cluster-in-hdinsight.json" target="_blank"><img src="./media/hdinsight-apps-install-custom-applications/deploy-to-azure.png" alt="Deploy tooAzure"></a>

    Ezzel a gombbal megnyithatja a Resource Manager-sablon a hello Azure-portálon.  hello Resource Manager-sablon itt található: [https://hditutorialdata.blob.core.windows.net/hdinsightapps/create-linux-based-hadoop-cluster-in-hdinsight.json](https://hditutorialdata.blob.core.windows.net/hdinsightapps/create-linux-based-hadoop-cluster-in-hdinsight.json).  toolearn hogyan toowrite a Resource Manager-sablon, lásd: [MSDN: HDInsight-alkalmazások telepítése](https://msdn.microsoft.com/library/mt706515.aspx).
2. Hajtsa végre a hello utasítás toocreate fürt és a Hue telepítése. További információk a HDInsight-fürtökről: [Create Linux-based Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md) (Linux-alapú Hadoop-fürtök létrehozása a HDInsight szolgáltatásban).

Ezenkívül toohello Azure-portálon, használhatja [Azure PowerShell](hdinsight-hadoop-create-linux-clusters-arm-templates.md#deploy-with-powershell) és [Azure CLI](hdinsight-hadoop-create-linux-clusters-arm-templates.md#deploy-with-cli) toocall Resource Manager-sablonok.

## <a name="validate-hello-installation"></a>Hello a telepítés ellenőrzése
Ellenőrizheti az Azure portál toovalidate hello alkalmazástelepítés hello hello alkalmazási állapota. Ezenkívül is ellenőrizheti az összes HTTP végpontok várt megjelenésük és hello weblap ha van ilyen:

**tooopen hello Hue portál**

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Kattintson a **a HDInsight-fürtök** hello bal oldali menüben.  Ha nem látja, kattintson a **Browse** (Tallózás), majd a **HDInsight Clusters** (HDInsight-fürtök) elemre.
3. Kattintson a hello fürtre, ahol hello alkalmazást telepítette.
4. A hello **beállítások** panelen kattintson a **alkalmazások** alatt hello **általános** kategória. A következő szöveggel: **hue** hello felsorolt **telepített alkalmazások** panelen.
5. Kattintson a **hue** hello lista toolist hello tulajdonságai közül.  
6. Kattintson a hello weblap hivatkozás toovalidate hello webhely; Nyissa meg a hello HTTP-végpont egy böngésző toovalidate hello Hue webes felhasználói felület, nyissa meg hello SSH végpont SSH használatával. További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="troubleshoot-hello-installation"></a>Hello telepítésével kapcsolatos hibaelhárítás
Ellenőrizheti a portál értesítései hello hello alkalmazás telepítési állapotát (kattintson a hello felül hello portál hello harang ikonra).

Ha egy alkalmazás telepítése sikertelen volt, hello hibaüzenetekben, és hibakeresési 3 vagy származó információkat:

* HDInsight-alkalmazások: általános hibainformációk.

    Nyissa meg a hello fürt hello portálról, és kattintson a alkalmazások hello-beállítások panelen:

    ![hdinsight applications application installation error](./media/hdinsight-apps-install-applications/hdinsight-apps-error.png)
* HDInsight-parancsfájlművelet: Ha hello HDInsight-alkalmazás hibaüzenete parancsfájlművelet-hibát jelez, hello parancsfájl hibával kapcsolatos további részletekért jelenik-e hello parancsfájl műveletek panel.

    Kattintson a parancsfájl művelet hello-beállítások panelen. Hello hibaüzeneteket megjelenítő parancsfájlművelet-előzmény

    ![hdinsight applications script action error](./media/hdinsight-apps-install-applications/hdinsight-apps-script-action-error.png)
* Ambari webes felhasználói felület: Hello telepítési parancsfájl volt hello hello hiba okát, használja az Ambari webes felhasználói felületén toocheck teljes naplók hello telepítését parancsfájlokkal kapcsolatban.

    További információk: [Hibaelhárítás](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting).

## <a name="remove-hdinsight-applications"></a>HDInsight-alkalmazások eltávolítása
Számos módon toodelete HDInsight alkalmazások vannak.

### <a name="use-portal"></a>A portál használatával
**tooremove alkalmazás hello portál használatával**

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Kattintson a **a HDInsight-fürtök** hello bal oldali menüben.  Ha nem látja, kattintson a **Browse** (Tallózás), majd a **HDInsight Clusters** (HDInsight-fürtök) elemre.
3. Kattintson a hello fürtre, ahol hello alkalmazást telepítette.
4. A hello **beállítások** panelen kattintson a **alkalmazások** alatt hello **általános** kategória. Ekkor a telepített alkalmazások listája jelenik meg. Ebben az oktatóanyagban **hue** hello felsorolt **telepített alkalmazások** panelen.
5. Kattintson a jobb gombbal a kívánt tooremove, és kattintson a hello alkalmazás **törlése**.
6. Kattintson a **Igen** tooconfirm.

Hello portálról is törölheti hello fürt vagy hello alkalmazást tartalmazó erőforráscsoportot hello törlése.

### <a name="use-azure-powershell"></a>Azure PowerShell használatával
Az Azure PowerShell használatával törölheti hello fürtöt vagy hello erőforráscsoportot. Lásd: [Fürtök törlése az Azure PowerShell használatával](hdinsight-administer-use-powershell.md#delete-clusters).

### <a name="use-azure-cli"></a>Az Azure parancssori felület használatával
Azure parancssori felület használatával törölheti hello fürtöt vagy hello erőforráscsoportot. Lásd: [Fürtök törlése az Azure parancssori felület használatával](hdinsight-administer-use-command-line.md#delete-clusters).

## <a name="next-steps"></a>Következő lépések
* [MSDN: HDInsight-alkalmazások telepítése](https://msdn.microsoft.com/library/mt706515.aspx): megtudhatja, hogyan toodevelop Resource Manager-sablonok a HDInsight-alkalmazások telepítéséhez.
* [HDInsight-alkalmazások telepítése](hdinsight-apps-install-applications.md): megtudhatja, hogyan tooinstall egy HDInsight-alkalmazás tooyour fürtök.
* [HDInsight-alkalmazások közzététele](hdinsight-apps-publish-applications.md): megtudhatja, hogyan toopublish az egyéni HDInsight-alkalmazások tooAzure piactéren.
* [Parancsfájlművelet Linux-alapú HDInsight-fürtök testreszabása](hdinsight-hadoop-customize-cluster-linux.md): megtudhatja, hogyan toouse parancsfájlművelet tooinstall további alkalmazásokat.
* [Linux-alapú Hadoop-fürtök létrehozása a Resource Manager-sablonok használatával HDInsight](hdinsight-hadoop-create-linux-clusters-arm-templates.md): megtudhatja, hogyan toocall Resource Manager sablonok toocreate HDInsight-fürtök.
* [Üres peremhálózati csomópontok használata a Hdinsightban](hdinsight-apps-use-edge-node.md): megtudhatja, hogyan toouse egy üres él HDInsight-fürthöz való hozzáférés, a HDInsight-alkalmazások tesztelése és üzemeltetési csomópont HDInsight-alkalmazások.
