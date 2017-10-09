---
title: "Hadoop YARN alkalmazás aaaAccess programozott módon - naplók Azure |} Microsoft Docs"
description: "Hozzáférési kérelem programozott módon bejelentkezik a HDInsight Hadoop-fürthöz."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 0198d6c9-7767-4682-bd34-42838cf48fc5
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 064efee1ea6a864c29ab897692ead0152c926c0b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="access-yarn-application-logs-on-windows-based-hdinsight"></a>Windows-alapú HDInsight bejelentkezik hozzáférést YARN alkalmazás
Ez a témakör ismerteti, hogyan tooaccess hello YARN (még egy másik erőforrás egyeztető) alkalmazás esetében, amely a Windows-alapú Hadoop-fürthöz az Azure HDInsight végzett naplók

> [!IMPORTANT]
> Ez a dokumentum hello információk csak tooWindows-alapú HDInsight-fürtök vonatkoznak. Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement). YARN eléréséről a Linux-alapú HDInsight-fürtök bejelentkezik, a következő témakörben: [Linux-based Hadoop on HDInsight bejelentkezik hozzáférést YARN alkalmazás](hdinsight-hadoop-access-yarn-app-logs-linux.md)
>


### <a name="prerequisites"></a>Előfeltételek
* Egy Windows-alapú HDInsight-fürtöt.  Lásd: [hdinsight-fürtök létrehozása Windows-alapú Hadoop](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="yarn-timeline-server"></a>YARN ütemterv kiszolgáló
Hello <a href="http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">YARN ütemterv Server</a> általános információkat nyújt a befejeződött alkalmazásokkal, valamint a keretrendszer-specifikus alkalmazás adatait két különböző felületeken keresztül. Konkrétan:

* Tárolása és lekérése általános alkalmazás információk a HDInsight-fürtökön engedélyezett 3.1.1.374 verziójával vagy magasabb volt.
* hello keretrendszer-specifikus alkalmazás információk hello ütemterv Server összetevője nem érhető el a HDInsight-fürtökön.

Alkalmazások általános információkat tartalmaz az adatok rendezése a következő hello:

* hello Alkalmazásazonosító, az alkalmazás egyedi azonosítója
* hello felhasználói hello alkalmazást
* Információk próbálkozások végrehajtott toocomplete hello alkalmazás
* minden, az adott alkalmazáshoz az általa által használt hello tárolók

A HDInsight-fürtökön ezeket az információkat tárolja az Azure Resource Manager tooa előzmények áruházak hello alapértelmezett tárolóban, az alapértelmezett Azure Storage-fiók. Az általános kész alkalmazások adatokat lehet beolvasni a REST API-n keresztül:

    GET on https://<cluster-dns-name>.azurehdinsight.net/ws/v1/applicationhistory/apps


## <a name="yarn-applications-and-logs"></a>YARN alkalmazások és a naplókat
YARN erőforrás-kezelés az ütemezés/alkalmazásfigyelés leválasztásával több programozási modellek (MapReduce egyik folyamatban) támogatja. Ez egy globális segítségével történik *ResourceManager* (RM) worker-csomópontonként *NodeManagers* (NMs), és alkalmazásonként *ApplicationMasters* (AMs). hello alkalmazásonkénti AM egyezteti erőforrások (Processzor, memória, lemez, hálózati) az alkalmazás futtatásához a hello RM Ezeket az erőforrásokat, amelyek kiadott NMs toogrant működik hello RM *tárolók*. hello AM felelős hello rendelve tárolók tooit hello előrehaladását nyomkövetés hello RM szerint Az alkalmazások sok tárolók hello alkalmazás hello jellege függően előfordulhat, hogy igényelnek.

Továbbá minden alkalmazás több állhat *alkalmazás kísérletek* a rendelés toofinish hello jelenlétében összeomlik vagy egy óra közötti kommunikáció toohello elvesztése miatt és egy RM Ezért tárolók kapnak tooa adott kísérlet egy alkalmazás. Bizonyos értelemben egy tárolót biztosít a YARN alkalmazások által végzett munka alapvető egysége hello környezetben, és a tároló hello környezetben végrehajtott összes munka történik, mely hello a tároló lett lefoglalva hello egyetlen munkavégző csomóponton. Lásd: [YARN fogalmak] [ YARN-concepts] további referenciaként.

Alkalmazás naplók (és társított hello tároló naplók) is kritikus megoldani a problémát okozó Hadoop-alkalmazások. YARN töltött keretrendszerében gyűjtése, összesítése és tárolja a hello alkalmazásnaplók [napló összesítési] [ log-aggregation] szolgáltatás. hello napló összesítési szolgáltatás révén elérése során alkalmazásnaplók sokkal kiszámíthatóbb, naplók von össze az összes tároló, a munkavégző csomópont, és tárolja őket egy összesített naplófájlhoz munkavégző csomópont hello alapértelmezett fájlrendszeren alkalmazás befejeződése után. Az alkalmazás használhat több száz vagy ezer tárolók, de egyetlen munkavégző csomóponton futtassa az összes tároló naplókat mindig lesz összesített tooa egyfájlos, ami azt eredményezi, amelyet az alkalmazás munkavégző csomópont kiszolgálónként egy naplófájlba. A HDInsight-fürtökön alapértelmezés szerint engedélyezve van a napló összesítési (3.0-s verzió vagy újabb verzió), és összesített naplók hello alapértelmezett tárolóban a fürt: hello a következő helyen találhatók:

    wasb:///app-logs/<user>/logs/<applicationId>

Az adott helyre *felhasználói* hello név hello felhasználó hello alkalmazást, és *applicationId* van egy alkalmazás egyedi azonosítója hello hello YARN RM által hozzárendelt

hello összesített naplók nincsenek közvetlenül is olvasható, mivel az oktatóprogram egy [TFile][T-file], [bináris formátum] [ binary-format] indexelik a tárolóban. YARN biztosít a parancssori eszközök toodump ezek a naplók egyszerű szövegként alkalmazások vagy a tárolókat iránt. Egyszerű szövegként a naplók YARN parancsok után közvetlenül fürtcsomópontokon hello (miután tooit RDP keresztül) hello egyikének futtatásával tekintheti meg:

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>


## <a name="yarn-resourcemanager-ui"></a>YARN erőforrás-kezelő felhasználói felületen
hello YARN erőforrás-kezelő felhasználói felületén hello fürt headnode fut, és hello Azure-portál irányítópultjának keresztül érhetők el:

1. Jelentkezzen be a túl[Azure-portálon](https://portal.azure.com/).
2. Hello bal oldali menüben kattintson **Tallózás**, kattintson a **a HDInsight-fürtök**, kattintson a Windows-alapú fürt, amelyet tooaccess hello YARN alkalmazásnaplók.
3. Kattintson a felső menüben hello **irányítópult**. Megjelenik egy lap nyílik meg egy új böngészőt nevű lap **HDInsight lekérdezés konzol**.
4. A **HDInsight lekérdezés konzol**, kattintson a **Yarn felhasználói felületen**.

[YARN-timeline-server]:http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[log-aggregation]:http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/
[T-file]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[binary-format]:https://issues.apache.org/jira/browse/HADOOP-3315
[YARN-concepts]:http://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/
