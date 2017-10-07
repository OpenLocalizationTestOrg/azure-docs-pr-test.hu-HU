---
title: "Azure HDInsight Hive-táblázatok adatait aaaSample |} Microsoft Docs"
description: "Lefelé (Hadopop) Azure HDInsight Hive táblák az adatok mintavétele"
services: machine-learning,hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: f31e8d01-0fd4-4a10-b1a7-35de3c327521
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: hangzh;bradsev
ms.openlocfilehash: 5f86df9b5a18facc875f437abfb004dbe3a06ea4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sample-data-in-azure-hdinsight-hive-tables"></a>Adatmintavétel az Azure HDInsight Hive-táblákban
Ebben a cikkben azt ismertetik a Hive-lekérdezésekkel Azure HDInsight Hive táblák hogyan toodown-minta adataihoz. Azt a három popularly használt mintavételi módszerek terjed ki:

* Egységes véletlenszerű mintavétel
* A csoportok véletlenszerű mintavétel
* Rétegzett mintavétel

hello következő **menü** hivatkozásokat tartalmaz, amelyek ismertetik tootopics hogyan toosample adatait tároló különböző környezetekben.

[!INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

**Miért érdemes az az adatokat?**
Ha azt tervezi, hogy tooanalyze hello adatkészlet túl nagy, a rendszer általában egy jó ötlet toodown-minta hello adatok tooreduce azt tooa kisebb, de reprezentatív és könnyebben kezelhető méretét. Ez lehetővé teszi az adatok ismertetése, feltárása és a szolgáltatás mérnöki csapathoz. Hello csapat az tudományos folyamata a szerepköre tooenable gyors prototípusának hello adatfeldolgozási funkciók és a gépi tanulási modellek.

Ez a mintavételi feladat ez hello lépés [Team adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

## <a name="how-toosubmit-hive-queries"></a>Hogyan toosubmit Hive-lekérdezések
Hive-lekérdezéseket a hello hello Hadoop-fürt átjárócsomópontjához hello Hadoop parancssori konzolról küldheti el. toodo ez, jelentkezzen be a hello hello Hadoop-fürt átjárócsomópontjához nyissa meg a hello Hadoop parancssori konzolt, és küldje el onnan hello Hive-lekérdezéseket. Hive-lekérdezések hello Hadoop parancssori konzolon elküldése, lásd: [hogyan tooSubmit Hive-lekérdezések](machine-learning-data-science-move-hive-tables.md#submit).

## <a name="uniform"></a>Egységes véletlenszerű mintavétel
Egységes véletlenszerű mintavételi jelenti, hogy minden egyes sorára hello adatkészlet alatt mintát egyenlő esélyét. Egy kiegészítő mezőt rand() toohello adatkészlet hello belső "select" lekérdezési való hozzáadásával valósítható, és a hello külső "select" lekérdezési feltételtől véletlenszerű a mezőhöz.

Íme egy példa lekérdezést:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, …, fieldN
    from
        (
        select
            field1, field2, …, fieldN, rand() as samplekey
        from <hive table name>
        )a
    where samplekey<='${hiveconf:sampleRate}'

Itt `<sample rate, 0-1>` hello felhasználók kívánt toosample rekordok hello arányát határozza meg.

## <a name="group"></a>A csoportok véletlenszerű mintavétel
Ha a mintavételi kategorikus adatok, érdemes lehet tooeither hatókörébe felvenni és kizárni összes hello példányai néhány adott egy kategorikus változó értékét. Ez a "csoport mintavételi" Mit jelent.
Például ha kategorikus változó "Állapot", amelynek értékek szerepelnek NY, MA, CA, NJ, PA stb, rekordokat szeretne hello olyan állapotban lennie mindig együtt, hogy azok lekérdező vagy sem.

Íme példalekérdezést adott minták csoport szerint:

    SET sampleRate=<sample rate, 0-1>;
    select
        b.field1, b.field2, …, b.catfield, …, b.fieldN
    from
        (
        select
            field1, field2, …, catfield, …, fieldN
        from <table name>
        )b
    join
        (
        select
            catfield
        from
            (
            select
                catfield, rand() as samplekey
            from <table name>
            group by catfield
            )a
        where samplekey<='${hiveconf:sampleRate}'
        )c
    on b.catfield=c.catfield

## <a name="stratified"></a>Rétegzett mintavétel
Véletlenszerű mintavételi műanyaggal van rétegezett tekintetben tooa kategorikus változóval Ha kapott hello minták rendelkezik, amely kategorikus értékének a hello méretarány hasonlóan hello szülő feltöltése a mely hello minták származik. Ugyanebben a példában a fenti, használatával hello tegyük fel, hogy az adatok részleges feltöltések államok, azaz NJ van 100 megfigyelések, NY rendelkezik 60 megfigyelések, pedig WA 300 megfigyelések. Ha hello aránya adja meg mintavételi toobe 0,5 műanyaggal rétegezett, majd hello mintát a kell körülbelül 50, 30 és 150 megfigyelések NJ, NY és WA kulcsattribútumokkal.

Íme egy példa lekérdezést:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, field3, ..., fieldN, state
    from
        (
        select
            field1, field2, field3, ..., fieldN, state,
            count(*) over (partition by state) as state_cnt,
              rank() over (partition by state order by rand()) as state_rank
          from <table name>
        ) a
    where state_rank <= state_cnt*'${hiveconf:sampleRate}'


Információ a speciális mintavételi felhasználható módszerek közül a Hive: [LanguageManual mintavételi](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Sampling).

