---
title: "több HDInsight-fürtök fiókkal az Azure Data Lake Store - Azure aaaUse |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse több mint egy HDInsight-fürtöt egy Data Lake Store-fiók"
keywords: "hdinsight-tárolóba, a hdfs, a strukturált adatok, a strukturálatlan adatok, a data lake store"
services: hdinsight,storage
documentationcenter: 
tags: azure-portal
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2017
ms.author: nitinme
ms.openlocfilehash: 3a125b91fcc1e436270a1bd349f7a2d8f27e06fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-multiple-hdinsight-clusters-with-an-azure-data-lake-store-account"></a>Több HDInsight-fürt használata az Azure Data Lake Store-fiók

HDInsight 3.5-ös verziója verziótól kezdődően hozhat létre HDInsight-fürtök az Azure Data Lake Store-fiók, hello alapértelmezett fájlrendszer.
Data Lake Store támogatja a korlátlan tárterület, így ideális, ha nem csak futtató nagy mennyiségű adat; de is üzemeltetéséhez több HDInsight-fürtök ennek a megosztásnak a egyetlen Data Lake Store-fiókból. Hogyan toocreate egy HDInsight-fürtöt Data Lake Store hello tárolóként instructionson, lásd: [HDInsight-fürtök létrehozása a Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

Ez a cikk ismerteti a javaslatok toohello Data Lake store rendszergazda az egyetlen és megosztott Data Lake tárolásához fiók beállítása, amely több használható **aktív** a HDInsight-fürtök. Ezek a javaslatok több biztonságos, valamint a nem biztonságos Hadoop-fürtök megosztott Data Lake store-fiókot a toohosting alkalmazni.


## <a name="data-lake-store-file-and-folder-level-acls"></a>Data Lake Store-fájl- és hozzáférés-vezérlési listák szinten

hello rest Ez a cikk feltételezi, hogy rendelkezik-e a fájl- és szintű hozzáférés-vezérlési listák jó ismerete az Azure Data Lake Store, a részletes információkat: [hozzáférés-vezérlés az Azure Data Lake Store](../data-lake-store/data-lake-store-access-control.md).

## <a name="data-lake-store-setup-for-multiple-hdinsight-clusters"></a>Több HDInsight-fürtök a Data Lake Store-telepítő
Ossza meg velünk igénybe vehet egy kétszintű mappahierarchia tooexplain hello javaslatok mutilple a HDInsight-fürtök használata a Data Lake Store-fiók. Vegye figyelembe a rendelkezik egy Data Lake Store-fiókkal és hello mappaszerkezet **/fürtök/pénzügyi**. Ez a struktúra hello pénzügyi szervezet által igényelt összes hello fürt segítségével /clusters/finance hello tárolási helyeként. A jövőbeli, ha egy másik szervezet mondja ki a Marketing, toocreate HDInsight-fürtök hello azonos Data Lake Store-fiók, akkor létrehozhat /clusters/marketing szeretne hello. Most tegyük használja **/fürtök/pénzügyi**.

a mappa struktúra toobe hatékonyan a HDInsight-fürtök által használt tooenable, hello Data Lake Store rendszergazdájának hozzá kell rendelnie megfelelő engedélyekkel, hello táblázatban ismertetett módon. hello táblázatban hello engedélyek megfelelnek tooAccess-hozzáférés-vezérlési listákat, és nem alapértelmezett-hozzáférés-vezérlési listák. 


|Mappa  |Engedélyek  |Tulajdonos felhasználó  |Tulajdonoscsoport  | Nevesített felhasználó | Nevesített felhasználó engedélyek | Nevesített csoport | Elnevezett csoportját engedélyek |
|---------|---------|---------|---------|---------|---------|---------|---------|
|/ | rwxr-x – x  |Rendszergazda |Rendszergazda  |Egyszerű szolgáltatásnév |--x  |FINGRP   |r-x         |
|/Clusters | rwxr-x – x |Rendszergazda |Rendszergazda |Egyszerű szolgáltatásnév |--x  |FINGRP |r-x         |
|/ fürtök/pénzügyi | rwxr-x – t |Rendszergazda |FINGRP  |Egyszerű szolgáltatásnév |rwx  |-  |-     |

Hello tábla

- **felügyeleti** hello létrehozó és hello Data Lake Store-fiók rendszergazdája.
- **Szolgáltatás egyszerű** hello Azure Active Directory (AAD) szolgáltatás egyszerű hello fiókhoz társított.
- **FINGRP** az aad-ben, amely hello pénzügyi szervezet felhasználóinak tartalmazza létrehozott felhasználói csoport.

Hogyan toocreate egy AAD-alkalmazást, (amelyek is létrehoz egy egyszerű szolgáltatásnév) témakörben talál útmutatást [hozzon létre egy AAD-alkalmazást](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application). Hogyan toocreate az aad-ben, egy felhasználói csoportot: kapcsolatos utasításokat [csoportkezelés az Azure Active Directory](../active-directory/active-directory-accessmanagement-manage-groups.md).

Néhány legfontosabb tooconsider.

- hello két szintű mappaszerkezet (**/fürtök/pénzügyi/**) kell létrehoznia és hozta-e megfelelő engedélyekkel rendelkező hello Data Lake Store admin **előtt** fürtök hello storage-fiók használatával. Ez a struktúra nem készül automatikusan fürtök létrehozása során.
- hello példában azt javasolja, hogy a tulajdonos csoportja hello beállítása **/fürtök/pénzügyi** , **FINGRP** és lehetővé teszi a **r-x** tooFINGRP toohello teljes mappahierarchia eléréséhez hello gyökér-től kezdődő. Ez biztosítja, hogy FINGRP hello tagjai navigálhatnak hello mappaszerkezet legfelső szintű kezdve.
- Hello esetben, ha különböző AAD Szolgáltatásnevekről hozhat létre a fürtök **/fürtök/pénzügyi**, hello állandóságát bites (Ha a hello beállítása **pénzügyi** mappa) biztosítja, hogy mappák egy szolgáltatása által létrehozott Más hello rendszerbiztonsági tag nem törölhető.
- Miután hello mappaszerkezet és az engedélyek vannak érvényben, HDInsight fürt létrehozási folyamata hoz létre a fürt-specifikus tárolási helynek a **/fürtök/pénzügyi/**. Például lehet hello tárolási hello neve fincluster01 a fürt **/clusters/finance/fincluster01**. hello tulajdonjoga és a HDInsight-fürt által létrehozott hello mappákra vonatkozó engedélyek látható itt hello tábla.

    |Mappa  |Engedélyek  |Tulajdonos felhasználó  |Tulajdonoscsoport  | Nevesített felhasználó | Nevesített felhasználó engedélyek | Nevesített csoport | Elnevezett csoportját engedélyek |
    |---------|---------|---------|---------|---------|---------|---------|---------|
    |/Clusters/finanace/fincluster01 | rwxr-x---  |Egyszerű szolgáltatásnév |FINGRP  |- |-  |-   |-  | 
   


## <a name="recommendations-for-job-input-and-output-data"></a>Javaslatok feladat bemeneti és kimeneti adatok

Azt javasoljuk, hogy a bemeneti adatok tooa feladatot, és egy feladat kimeneteinek hello tárolható egy mappát kívül **/fürtök**. Ez biztosítja, hogy akkor is, ha hello fürtre mappa törölt tooreclaim néhány tárolóhely, hello feladat be- és kimenetekkel továbbra is elérhetők a későbbi használat érdekében. Ebben az esetben gondoskodjon arról, hogy hello mappahierarchia hello feladatot bemenetekhez és kimenetekhez tárolásához lehetővé tegye hello hozzáférési megfelelő szintje egyszerű szolgáltatást.

## <a name="limit-on-clusters-sharing-a-single-storage-account"></a>Egyetlen tárfiók megosztása fürtökön korlátozása

hello fürtök, egy Data Lake Store-fiók megoszthat hello számára vonatkozó korlátozást a ezen a fürtön futó hello munkaterheléstől függ. Túl sok fürtök vagy nagyon nehéz munkaterhelések rendelkező hello fürtökön, amelyek egy tárfiókot, előfordulhat, hogy hello tárolási fiók be-és kilépési tooget szabályozva.

## <a name="support-for-default-acls"></a>Alapértelmezett – ACL-ek támogatása

Amikor egy egyszerű szolgáltatás létrehozása nevű felhasználói hozzáférést (a fenti hello táblázatban látható), azt javasoljuk **nem** hello nevű-felhasználót egy alapértelmezett ACL hozzáadása. Üzembe helyezési nevű felhasználói hozzáférés alapértelmezett-hozzáférés-vezérlési listák eredmények hello 770 engedélyek hozzárendelése a tulajdonos felhasználó, a tulajdonos csoport és mások számára. 770 az alapértelmezett értéke nem jár engedélyeket (7) tulajdonos vagy a tulajdonos-csoport (7), amíg tart a számítógépnél összes engedélyt, mások (0). Ez egy ismert probléma az egy adott használati eset hello részletesen ismertetett eredményez [ismert problémák és megoldások](#known-issues-and-workarounds) szakasz.

## <a name="known-issues-and-workarounds"></a>Ismert problémák és megoldások

Ez a szakasz hello ismert problémái HDInsight használata a Data Lake Store, és azok megoldását ismerteti.

### <a name="publicly-visible-localized-yarn-resources"></a>Nyilvánosan látható honosított YARN erőforrások

Egy új Azure Data Lake store-fiók létrehozásakor hello gyökérkönyvtár automatikusan létrehozza a rendszer a hozzáférés-ACL engedély bits set too770. hello legfelső szintű mappa tulajdonos set toohello felhasználó által létrehozott hello fiók (hello Data Lake Store admin) és hello tulajdonos csoportja toohello hello fiókot létrehozó felhasználó hello elsődleges csoportja. Nem lehet hozzáférni az "egyéb" valósul meg.

Ezek a beállítások ismert tooaffect egy adott HDInsight használati eset rögzített [YARN 247](https://hwxmonarch.atlassian.net/browse/YARN-247). Egy hiba üzenet hasonló toothis feladatok elküldésének meghibásodásához:

    Resource XXXX is not publicly accessible and as such cannot be part of hello public cache.

Leírtaknak YARN JIRA korábbi, azaz nyilvános erőforrások hello közben kapcsolódó hello lokalizáló ellenőrzi, hogy az összes hello kért erőforrások közé valóban nyilvános hello távoli fájlrendszer a rájuk vonatkozó engedélyek ellenőrzése. Bármely, amelyek nem felelnek meg, hogy a feltétel LocalResource honosításhoz elutasítva. engedélyek ellenőrzése hello, írásvédett toohello fájl tartalmazza az "egyéb". Ez a forgatókönyv nem működik, a-kész esetén a HDInsight-fürtök az Azure Data Lake, mivel az Azure Data Lake megtagadja minden túl "egyéb" mappa gyökérszinten.

#### <a name="workaround"></a>Megkerülő megoldás
Set olvasási-végrehajtási engedélyeinek **mások** keresztül hello hierarchia, például  **/** , **/fürtök** és   **/fürtök/pénzügyi**  fenti hello táblázatban látható módon.

## <a name="see-also"></a>Lásd még:

* [HDInsight-fürtök létrehozása a Data Lake Store tárolóként](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md)


