---
title: "Az adatokat az Azure HDInsight Hive táblák |} Microsoft Docs"
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
ms.openlocfilehash: d46297dfaf85976114fbf610803e5f1a997041e0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="sample-data-in-azure-hdinsight-hive-tables"></a>Adatmintavétel az Azure HDInsight Hive-táblákban
Ez a cikk azt ismertetik lefelé-minta Hive-lekérdezésekkel Azure HDInsight Hive táblákban tárolt adatokat. Azt a három popularly használt mintavételi módszerek terjed ki:

* Egységes véletlenszerű mintavétel
* A csoportok véletlenszerű mintavétel
* Rétegzett mintavétel

A következő **menü** az adatokat a különböző tárolási környezetekben módját leíró témakörök hivatkozásait.

[!INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

**Miért érdemes az az adatokat?**
Ha azt tervezi, hogy elemezheti az adatkészlet túl nagy, akkor általában down kétmintás az adatokat, hogy az kisebb, de reprezentatív és könnyebben kezelhető méretű jó ötlet. Ez lehetővé teszi az adatok ismertetése, feltárása és a szolgáltatás mérnöki csapathoz. A szerepkör az adatok tudományos folyamatban az adatok feldolgozása funkciók és a gépi tanulási modellek gyors prototípusának engedélyezéséhez.

Ez a mintavételi feladat Ez a lépés a [Team adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

## <a name="how-to-submit-hive-queries"></a>Hogyan lehet elküldeni a Hive-lekérdezések
A Hadoop parancssori konzolról a Hadoop-fürt átjárócsomópontjához a Hive-lekérdezések küldheti el. Ehhez jelentkezzen be a Hadoop-fürt átjárócsomópontjához, nyissa meg a Hadoop parancssori konzolt, és küldje el a Hive-lekérdezések ott. A Hadoop parancssori konzolon Hive-lekérdezések elküldése, lásd: [hogyan küldhetnek Hive-lekérdezések](machine-learning-data-science-move-hive-tables.md#submit).

## <a name="uniform"></a>Egységes véletlenszerű mintavétel
Egységes véletlenszerű mintavételi jelenti, hogy az adatkészlet egyes soraihoz alatt mintát egyenlő esélyét. Ez az ad hozzá egy kiegészítő mezőt rand() az adatkészletben a belső "select" lekérdezési, és a külső "select" lekérdezési feltételhez véletlenszerű mező valósítható meg.

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

Itt `<sample rate, 0-1>` azt jelzi, hogy a felhasználók mintát szeretne arányát határozza meg.

## <a name="group"></a>A csoportok véletlenszerű mintavétel
Ha mintavételi kategorikus adatok, érdemes lehet akár vagy kizárja a minden egyes adott kategorikus változó értékének előfordulása. Ez a "csoport mintavételi" Mit jelent.
Például ha kategorikus változó "Állapot", amelynek értékek NY MA, CA, NJ, PA, stb akkor, rekordjának olyan állapotban kell mindig együtt, hogy azok lekérdező vagy nem.

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
Véletlenszerű mintavételi van műanyaggal rétegezett tekintetében kategorikus változó Ha kapott minták értékeit, hogy kategorikus, amelyek egy-egy beállítani, mint a szülő feltöltését, ahol a minták származik. Az előző példát, a fenti tegyük fel, hogy az adatok részleges feltöltések rendelkezik államok, például NJ 100 megfigyelések, NY 60 megfigyelések van, pedig WA 300 megfigyelések. Ha 0,5 kell rétegzett mintavételi arány ad meg, majd a mintát a kell rendelkeznie körülbelül 50, 30 és 150 megfigyelések NJ, NY és WA rendre.

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

