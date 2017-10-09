---
title: "aaaInstall külső Hadoop-alkalmazások Azure hdinsight |} Microsoft Docs"
description: "Megtudhatja, hogyan tooinstall külső Hadoop-alkalmazások Azure hdinsight."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: eaf5904d-41e2-4a5f-8bec-9dde069039c2
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/16/2017
ms.author: jgao
ms.openlocfilehash: 00071517c81a17c01dccedf9e8dd5d0cabb38567
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-third-party-hadoop-applications-on-azure-hdinsight"></a>Külső gyártótól származó Hadoop-alkalmazások telepítése az Azure HDInsighton

Ebből a cikkből megismerheti, hogyan tooinstall Azure HDInsight már közzétett külső Hadoop alkalmazás. A saját alkalmazások telepítéséről az [Egyéni HDInsight-alkalmazások telepítése](hdinsight-apps-install-custom-applications.md) című cikk tartalmaz útmutatást.

A HDInsight-alkalmazások olyan alkalmazások, amelyeket a felhasználók egy Linux-alapú HDInsight-fürtre telepíthetnek. Ezek az alkalmazások lehetnek a Microsoft, független szoftvergyártók (ISV-k) vagy a felhasználók fejlesztései.  

Jelenleg négy közzétett alkalmazás érhető el:

* **A HDInsight DATAIKU DDS**: Dataiku DSS (adatok tudományos Studio) olyan szoftver, amely lehetővé teszi az adatok szakemberek (adatszakértőkön, az üzleti elemzők, fejlesztők...) tooprototype, elkészítéséhez és magas adott szolgáltatások telepítését, amelyek a nyers adatok átalakítása impactful üzleti előrejelzéseket.
* **Datameer**: [Datameer](http://www.datameer.com/documentation/display/DAS50/Home?ls=Partners&lsd=Microsoft&c=Partners&cd=Microsoft) kínál az elemzők egy interaktív módon toodiscover elemzése és a Big Data hello eredményeinek képi megjelenítése. Húzza a további adatforrások könnyen toodiscover új kapcsolatokat, és adott válaszok hello gyorsan van szüksége.
* **A HDnsight Streamsets adatgyűjtő** biztosít egy teljes körű integrált fejlesztőkörnyezetet (IDE) kialakítása, teszteléséhez, telepítése és kezelhető bármely közöttiként folyamatok, amelyek a háló adatfolyam és kötegelt adatok betöltési, és számos adatfolyam-átalakítások – toowrite egyéni kód nélkül. 
* **A HDInsight CDAP cask** hello először egyesített integrációs platformján a big data, amely csökkenti az adatok alkalmazások és adatok tavakat hello idő tooproduction 80 %-kal biztosít. Ez az alkalmazás kizárólag a Standard HBase 3.4-fürtöket támogatja.
* **A HDInsight (béta) H2O mesterséges Eszközintelligencia** H2O készült vízjel támogatja a következő elosztott algoritmusok hello: GLM, natív Bayes, elosztott véletlenszerű erdő, átmenetes kiemelése gép, Neurális hálózatokat, mély tanulási, a K-közép, PEM, Alacsony Rank modellek, Anomáliadetektálás és Autoencoders általánosítva van.
* **Kyligence Analytics Platform** Kyligence Analytics Platform (KAP) Apache Kylin és Apache Hadoop kapcsolva, nagyvállalati használatra kész adatraktár; a lehetővé teszi az alárendelt második lekérdezés késés jelentős mértékű adatkészlethez, és leegyszerűsíti az adatelemzés üzleti felhasználók és az elemzők. 
* **SnapLogic Hadooplex** hello hdinsighton futó SnapLogic Hadooplex lehetővé teszi, hogy az ügyfelek tooget toobusiness insights gyorsabb önkiszolgáló adatfeldolgozást és szinte bármilyen forrás toohello Microsoft Azure felhőbe a előkészítése megadásával Platform.
* **Spark-feladatkiszolgálót KNIME Spark végrehajtó** KNIME Spark végrehajtó Spark-feladatkiszolgálót használt tooconnect hello KNIME Analytics Platform tooHDInsight fürtök.

Ez a cikk utasításokat hello használata Azure-portálon. Hello Azure Resource Manager sablon exportálása hello portálról vagy szállítóktól származó hozzájutni hello Resource Manager-sablon, és Azure PowerShell és az Azure parancssori felület toodeploy hello sablont.  Lásd: [Create Linux-based Hadoop clusters in HDInsight using Resource Manager templates](hdinsight-hadoop-create-linux-clusters-arm-templates.md) (Linux-alapú Hadoop-fürtök létrehozása a HDInsightban Resource Manager-sablonok segítségével).

## <a name="prerequisites"></a>Előfeltételek
Ha azt szeretné, hogy egy meglévő HDInsight-fürt a HDInsight-alkalmazások tooinstall, rendelkeznie kell egy HDInsight-fürtre. toocreate, lásd: [fürtöket létrehozni](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster). HDInsight-alkalmazásokat HDInsight-fürt létrehozása közben is telepíthet.

## <a name="install-applications-tooexisting-clusters"></a>Alkalmazások tooexisting fürtök telepítése
hello alábbi eljárás bemutatja, hogyan tooinstall HDInsight alkalmazások tooan meglévő HDInsight-fürtre.

**HDInsight-alkalmazások tooinstall**

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Kattintson a **a HDInsight-fürtök** hello bal oldali menüben.  Ha nem jelenik meg, kattintson a **További szolgáltatások**, majd a **HDInsight-fürtök** elemre.
3. Kattintson a kívánt HDInsight-fürtre.  Ha még nincs ilyen fürtje, hozzon létre egyet most.  Lásd: [Fürtök létrehozása](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).
4. Kattintson a **alkalmazások** alatt hello **konfigurációk** kategóriát. Láthatja, hogy a telepített alkalmazások listáját. Ha az alkalmazások nem talál, ez azt jelenti, hello HDInsight-fürt verzióhoz készült alkalmazás nem áll.
   
    ![HDInsight-alkalmazások menü a portálon](./media/hdinsight-apps-install-applications/hdinsight-apps-portal-menu.png)
5. Kattintson a **Hozzáadás** hello panel menüből. 
   
    ![HDInsight-alkalmazások, telepített alkalmazások](./media/hdinsight-apps-install-applications/hdinsight-apps-installed-apps.png)
   
    Megjelenik a meglévő HDInsight-alkalmazások listája.
   
    ![HDInsight-alkalmazások, elérhető alkalmazások](./media/hdinsight-apps-install-applications/hdinsight-apps-list.png)
6. Kattintson a hello alkalmazások közül, fogadja el hello jogi feltételeket, majd **válasszon**.

Megtekintheti az hello portál értesítések hello telepítési állapota (kattintson a hello felül hello portál hello harang ikonra). Hello után alkalmazás telepítve van, hello telepített alkalmazások panelről hello alkalmazás fog megjelenni.

## <a name="install-applications-during-cluster-creation"></a>Alkalmazások telepítése fürtlétrehozás során
A fürt létrehozásakor rendelkezik hello beállítás tooinstall HDInsight-alkalmazások. Hello folyamat során a HDInsight-alkalmazások telepített után hello fürt jön létre, és hello futó állapotban van. hello alábbi eljárás bemutatja, hogyan tooinstall HDInsight-alkalmazások fürt létrehozásakor.

**HDInsight-alkalmazások tooinstall**

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Kattintson az **ÚJ** elemre, majd az **Adatok + analitika**, végül a **HDInsight** elemre.
3. A **Fürt neve** mezőben adja meg a fürt nevét, amelynek globálisan egyedinek kell lennie.
4. Kattintson a **előfizetés** tooselect hello hello fürt Azure-előfizetéshez.
5. Kattintson a **Fürt típusának kiválasztása** elemre, majd válasszon:
   
   * **Fürt típusa**: Ha nem tudja, milyen toochoose, válassza ki a **Hadoop**. Hello legnépszerűbb fürt típusa.
   * **Operációs rendszer**: válassza a **Linux** lehetőséget.
   * **Verzió**: hello alapértelmezett verzióját használja, ha nem tudja, milyen toochoose. További tájékoztatás a [HDInsight cluster versions](hdinsight-component-versioning.md) (A HDInsight-fürtök verziói) című cikkben olvasható.
   * **A fürt réteg**: Azure HDInsight big data felhőajánlatokat hello két kategóriába biztosít: Standard csomagban és a prémium csomagban. További tájékoztatás a [Cluster tiers](hdinsight-hadoop-provision-linux-clusters.md#cluster-tiers) (A fürtök szintjei) című cikkben olvasható.
6. Kattintson a **alkalmazások**, kattintson egy hello közzétett alkalmazásokat, és kattintson a **válasszon**.
7. Kattintson a **hitelesítő adatok** és hello rendszergazdai jogú felhasználó adja meg egy jelszót. Tooenter is szükség van egy **SSH felhasználónév** és vagy egy **jelszó** vagy **nyilvános kulcs**, vagyis használt tooauthenticate hello SSH-felhasználó. A nyilvános kulcs használata ajánlott megközelítést alkalmazva hello. Kattintson a **válasszon** hello alsó toosave hello hitelesítői adatainak konfigurációja a következő.
8. Kattintson a **adatforrás**, válasszon egyet a meglévő tárfiókok hello, vagy hozzon létre egy új tárolási fiók toobe hello fürt hello alapértelmezett tárfiókot használja.
9. Kattintson a **erőforráscsoport** tooselect meglévő erőforrás csoportban, vagy kattintson a **új** toocreate egy új erőforráscsoportot
10. A hello **új HDInsight-fürt** panelen ellenőrizze, hogy **PIN-kód tooStartboard** van kiválasztva, és kattintson **létrehozása**. 

## <a name="list-installed-hdinsight-apps-and-properties"></a>Telepített HDInsight-alkalmazások és az alkalmazástulajdonságok listázása
hello portal hello listája telepített HDInsight-alkalmazások fürt, és minden telepített alkalmazás hello tulajdonságait tartalmazza.

**toolist HDInsight alkalmazáshoz és a megjelenített tulajdonságok**

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Kattintson a **a HDInsight-fürtök** hello bal oldali menüben.  Ha nem látja, kattintson a **Browse** (Tallózás), majd a **HDInsight Clusters** (HDInsight-fürtök) elemre.
3. Kattintson a kívánt HDInsight-fürtre.
4. A hello **beállítások** panelen kattintson a **alkalmazások** alatt hello **általános** kategória. hello telepített alkalmazások panelről hello telepített összes alkalmazás felsorolása 
   
    ![HDInsight-alkalmazások, telepített alkalmazások](./media/hdinsight-apps-install-applications/hdinsight-apps-installed-apps-with-apps.png)
5. Kattintson az egyik hello telepített alkalmazások tooshow hello tulajdonság. hello panel tulajdonságlisták:
   
   * Alkalmazásnév: az alkalmazás neve.
   * Állapot: az alkalmazás állapota. 
   * A weblap: hello, hogy telepítette-e toohello élcsomópont hello webes alkalmazás URL-CÍMÉT. hello hitelesítő adatok megegyeznek a hello HTTP-felhasználó hitelesítő adatait, amikor hello fürtben konfigurált hello.
   * HTTP-végpont: hello hitelesítő adatok megegyeznek a hello HTTP-felhasználó hitelesítő adatait, amikor hello fürtben konfigurált hello. 
   * SSH-végpont: SSH tooconnect toohello élcsomópontot is használhatja. hello SSH hitelesítő adatok megegyeznek a hello SSH-felhasználó hitelesítő adatai hello fürtben konfigurált hello. További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).
6. toodelete egy alkalmazáskészletet, kattintson a jobb gombbal a hello alkalmazást, és kattintson **törlése** hello helyi menüből.

## <a name="connect-toohello-edge-node"></a>Csatlakozás az élcsomóponthoz toohello
A HTTP és SSH toohello élcsomópontot is elérheti. hello végpont információt talál a hello [portal](#list-installed-hdinsight-apps-and-properties). További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

hello HTTP végpont hitelesítő adatok hello HTTP felhasználói hitelesítő adatokkal, konfigurálta a HDInsight-fürthöz hello; hello SSH végpont hitelesítő adatok hello SSH hitelesítő adatokat, amelyek a konfigurált hello HDInsight-fürthöz.

## <a name="troubleshoot"></a>Hibaelhárítás
Lásd: [hello telepítésével kapcsolatos hibaelhárítás](hdinsight-apps-install-custom-applications.md#troubleshoot-the-installation).

## <a name="next-steps"></a>Következő lépések
* [Egyéni HDInsight-alkalmazások telepítése](hdinsight-apps-install-custom-applications.md): megtudhatja, hogyan toodeploy egy közzé nem tett HDInsight-alkalmazás tooHDInsight.
* [HDInsight-alkalmazások közzététele](hdinsight-apps-publish-applications.md): megtudhatja, hogyan toopublish az egyéni HDInsight-alkalmazások tooAzure piactéren.
* [MSDN: HDInsight-alkalmazások telepítése](https://msdn.microsoft.com/library/mt706515.aspx): megtudhatja, hogyan toodefine HDInsight-alkalmazások.
* [Parancsfájlművelet Linux-alapú HDInsight-fürtök testreszabása](hdinsight-hadoop-customize-cluster-linux.md): megtudhatja, hogyan toouse parancsfájlművelet tooinstall további alkalmazásokat.
* [Linux-alapú Hadoop-fürtök létrehozása a Resource Manager-sablonok használatával HDInsight](hdinsight-hadoop-create-linux-clusters-arm-templates.md): megtudhatja, hogyan toocall Resource Manager sablonok toocreate HDInsight-fürtök.
* [Üres peremhálózati csomópontok használata a Hdinsightban](hdinsight-apps-use-edge-node.md): megtudhatja, hogyan toouse egy üres él HDInsight-fürthöz való hozzáférés, a HDInsight-alkalmazások tesztelése és üzemeltetési csomópont HDInsight-alkalmazások.

