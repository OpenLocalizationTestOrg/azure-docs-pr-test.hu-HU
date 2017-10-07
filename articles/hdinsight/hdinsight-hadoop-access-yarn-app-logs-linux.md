---
title: "Linux-alapú HDInsight - Azure bejelentkezik aaaAccess Hadoop YARN alkalmazás |} Microsoft Docs"
description: "Ismerje meg, hogy miként naplózza az tooaccess YARN alkalmazás egy Linux-alapú HDInsight (Hadoop) fürtön parancssori hello és egy webböngésző használatával."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 3ec08d20-4f19-4a8e-ac86-639c04d2f12e
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: 0bab356e3b97114abbb05712c8e7b21a194f2508
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="access-yarn-application-logs-on-linux-based-hdinsight"></a>Linux-alapú HDInsight bejelentkezik hozzáférést YARN alkalmazás

Ismerje meg, hogyan tooaccess hello naplók a YARN (még egy másik erőforrás egyeztető) alkalmazásokhoz az Azure HDInsight Hadoop-fürthöz.

> [!IMPORTANT]
> hello jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Linux igényelnek. Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További információkért lásd: [HDInsight-összetevők verziószámozása](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="YARNTimelineServer"></a>YARN ütemterv kiszolgáló

Hello [YARN ütemterv Server](http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html) általános információkat nyújt a kész alkalmazások és a keretrendszer-specifikus alkalmazással kapcsolatos adatok két különböző felületeken keresztül. Konkrétan:

* Tárolása és lekérése általános alkalmazás információk a HDInsight-fürtökön engedélyezett 3.1.1.374 verziójával vagy magasabb volt.
* hello keretrendszer-specifikus alkalmazás információk hello ütemterv Server összetevője nem érhető el a HDInsight-fürtökön.

Alkalmazások általános információkat tartalmaz a következő típusú adatok hello:

* hello Alkalmazásazonosító, az alkalmazás egyedi azonosítója
* hello felhasználói hello alkalmazást
* Információk próbálkozások végrehajtott toocomplete hello alkalmazás
* minden, az adott alkalmazáshoz az általa által használt hello tárolók

## <a name="YARNAppsAndLogs"></a>YARN alkalmazások és a naplókat

YARN erőforrás-kezelés az ütemezés/alkalmazásfigyelés leválasztásával több programozási modellek (MapReduce egyik folyamatban) támogatja. YARN használ egy globális *ResourceManager* (RM) worker-csomópontonként *NodeManagers* (NMs), és alkalmazásonként *ApplicationMasters* (AMs). hello alkalmazásonkénti AM egyezteti erőforrások (Processzor, memória, lemez, hálózati) az alkalmazás futtatásához a hello RM Ezeket az erőforrásokat, amelyek kiadott NMs toogrant működik hello RM *tárolók*. hello AM felelős hello rendelve tárolók tooit hello előrehaladását nyomkövetés hello RM szerint Az alkalmazások sok tárolók hello alkalmazás hello jellege függően előfordulhat, hogy igényelnek.

Minden alkalmazás több állhat *alkalmazás kísérletek*. Ha egy alkalmazás sikertelen, akkor előfordulhat, hogy újra megkísérli egy új kísérlet. A tárolóban lévő egyes kísérletek fut. Bizonyos értelemben tárolója YARN alkalmazás által végzett munka alapvető egysége hello környezetben biztosít. A tároló hello környezetben végrehajtott összes munka történik, mely hello a tároló lett lefoglalva hello egyetlen munkavégző csomóponton. Lásd: [YARN fogalmak] [ YARN-concepts] további referenciaként.

Alkalmazás naplók (és társított hello tároló naplók) is kritikus megoldani a problémát okozó Hadoop-alkalmazások. YARN töltött keretrendszerében gyűjtése, összesítése és tárolja a hello alkalmazásnaplók [napló összesítési] [ log-aggregation] szolgáltatás. hello napló összesítési szolgáltatás révén elérése során alkalmazásnaplók sokkal kiszámíthatóbb. Naplók von össze az összes tároló, a munkavégző csomópont, és tárolja őket egy összesített naplófájlhoz munkavégző csomópont. hello napló tárolt hello alapértelmezett fájlrendszer egy alkalmazás befejeződése után. Az alkalmazás használhat több száz vagy ezer tárolók, de egyetlen munkavégző csomóponton futtassa az összes tároló naplókat mindig összesített tooa egy fájlból. Így nincs csak egyes munkavégző csomópontok az alkalmazás által használt 1 napló. Napló összesítési HDInsight fürtök 3.0-s verzió vagy újabb rendszeren alapértelmezés szerint engedélyezve van. Összesített naplók hello fürt tárolóhelyét alapértelmezett találhatók. a következő elérési út hello hello HDFS elérési toohello naplók:

    /app-logs/<user>/logs/<applicationId>

Az elérési út hello `user` hello felhasználói hello alkalmazást hello neve. Hello `applicationId` hello hozzárendelt egyedi azonosítója tooan alkalmazás által hello YARN RM

hello összesített naplók nincsenek közvetlenül is olvasható, mivel az oktatóprogram egy [TFile][T-file], [bináris formátum] [ binary-format] indexelik a tárolóban. Használata YARN erőforrás-kezelő naplózza hello vagy a parancssori eszközök tooview ezek a naplók egyszerű szövegként alkalmazások vagy az egyik fontos tárolók.

## <a name="yarn-cli-tools"></a>YARN CLI-eszközei

toouse hello YARN CLI-eszközeit, először kapcsolódnia toohello HDInsight-fürthöz SSH használatával. További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

Egyszerű szövegként a naplók hello a következő parancsok futtatásával tekintheti meg:

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>

Adja meg a hello &lt;applicationId >, &lt;felhasználói-akik-elindult-az-alkalmazási >, &lt;Tárolóazonosító >, és &lt;worker-csomópont-címe > adatokat, amikor a parancsok futtatásával.

## <a name="yarn-resourcemanager-ui"></a>YARN erőforrás-kezelő felhasználói felületen

hello YARN erőforrás-kezelő felhasználói felületén a hello fürt headnode fut. Hello Ambari webes felhasználói felületen keresztül érhető el. Naplózza a következő lépéseket tooview hello YARN használata hello:

1. A böngészőben nyissa meg a toohttps://CLUSTERNAME.azurehdinsight.net. CLUSTERNAME cserélje le a HDInsight fürt hello neve.
2. Hello szolgáltatások hello bal oldali listában jelölje ki **YARN**.

    ![Kijelölt yarn szolgáltatás](./media/hdinsight-hadoop-access-yarn-app-logs-linux/yarnservice.png)
3. A hello **Gyorshivatkozások** legördülő menüben válasszon ki egy hello központi fürtcsomóponton, és válassza ki **ResourceManager napló**.

    ![Yarn Gyorshivatkozások](./media/hdinsight-hadoop-access-yarn-app-logs-linux/yarnquicklinks.png)

    Hivatkozások tooYARN naplók listája jelenik meg.

[YARN-timeline-server]:http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[log-aggregation]:http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/
[T-file]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[binary-format]:https://issues.apache.org/jira/browse/HADOOP-3315
[YARN-concepts]:http://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/
