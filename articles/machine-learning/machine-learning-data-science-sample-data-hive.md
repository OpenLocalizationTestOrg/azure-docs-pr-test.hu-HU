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
# <a name="sample-data-in-azure-hdinsight-hive-tables"></a><span data-ttu-id="b67a6-103">Adatmintavétel az Azure HDInsight Hive-táblákban</span><span class="sxs-lookup"><span data-stu-id="b67a6-103">Sample data in Azure HDInsight Hive tables</span></span>
<span data-ttu-id="b67a6-104">Ez a cikk azt ismertetik lefelé-minta Hive-lekérdezésekkel Azure HDInsight Hive táblákban tárolt adatokat.</span><span class="sxs-lookup"><span data-stu-id="b67a6-104">In this article, we describe how to down-sample data stored in Azure HDInsight Hive tables using Hive queries.</span></span> <span data-ttu-id="b67a6-105">Azt a három popularly használt mintavételi módszerek terjed ki:</span><span class="sxs-lookup"><span data-stu-id="b67a6-105">We cover three popularly used sampling methods:</span></span>

* <span data-ttu-id="b67a6-106">Egységes véletlenszerű mintavétel</span><span class="sxs-lookup"><span data-stu-id="b67a6-106">Uniform random sampling</span></span>
* <span data-ttu-id="b67a6-107">A csoportok véletlenszerű mintavétel</span><span class="sxs-lookup"><span data-stu-id="b67a6-107">Random sampling by groups</span></span>
* <span data-ttu-id="b67a6-108">Rétegzett mintavétel</span><span class="sxs-lookup"><span data-stu-id="b67a6-108">Stratified sampling</span></span>

<span data-ttu-id="b67a6-109">A következő **menü** az adatokat a különböző tárolási környezetekben módját leíró témakörök hivatkozásait.</span><span class="sxs-lookup"><span data-stu-id="b67a6-109">The following **menu** links to topics that describe how to sample data from various storage environments.</span></span>

[!INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

<span data-ttu-id="b67a6-110">**Miért érdemes az az adatokat?**</span><span class="sxs-lookup"><span data-stu-id="b67a6-110">**Why sample your data?**</span></span>
<span data-ttu-id="b67a6-111">Ha azt tervezi, hogy elemezheti az adatkészlet túl nagy, akkor általában down kétmintás az adatokat, hogy az kisebb, de reprezentatív és könnyebben kezelhető méretű jó ötlet.</span><span class="sxs-lookup"><span data-stu-id="b67a6-111">If the dataset you plan to analyze is large, it's usually a good idea to down-sample the data to reduce it to a smaller but representative and more manageable size.</span></span> <span data-ttu-id="b67a6-112">Ez lehetővé teszi az adatok ismertetése, feltárása és a szolgáltatás mérnöki csapathoz.</span><span class="sxs-lookup"><span data-stu-id="b67a6-112">This facilitates data understanding, exploration, and feature engineering.</span></span> <span data-ttu-id="b67a6-113">A szerepkör az adatok tudományos folyamatban az adatok feldolgozása funkciók és a gépi tanulási modellek gyors prototípusának engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="b67a6-113">Its role in the Team Data Science Process is to enable fast prototyping of the data processing functions and machine learning models.</span></span>

<span data-ttu-id="b67a6-114">Ez a mintavételi feladat Ez a lépés a [Team adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="b67a6-114">This sampling task is a step in the [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

## <a name="how-to-submit-hive-queries"></a><span data-ttu-id="b67a6-115">Hogyan lehet elküldeni a Hive-lekérdezések</span><span class="sxs-lookup"><span data-stu-id="b67a6-115">How to submit Hive queries</span></span>
<span data-ttu-id="b67a6-116">A Hadoop parancssori konzolról a Hadoop-fürt átjárócsomópontjához a Hive-lekérdezések küldheti el.</span><span class="sxs-lookup"><span data-stu-id="b67a6-116">Hive queries can be submitted from the Hadoop Command Line console on the head node of the Hadoop cluster.</span></span> <span data-ttu-id="b67a6-117">Ehhez jelentkezzen be a Hadoop-fürt átjárócsomópontjához, nyissa meg a Hadoop parancssori konzolt, és küldje el a Hive-lekérdezések ott.</span><span class="sxs-lookup"><span data-stu-id="b67a6-117">To do this, log into the head node of the Hadoop cluster, open the Hadoop Command Line console, and submit the Hive queries from there.</span></span> <span data-ttu-id="b67a6-118">A Hadoop parancssori konzolon Hive-lekérdezések elküldése, lásd: [hogyan küldhetnek Hive-lekérdezések](machine-learning-data-science-move-hive-tables.md#submit).</span><span class="sxs-lookup"><span data-stu-id="b67a6-118">For instructions on submitting Hive queries in the Hadoop Command Line console, see [How to Submit Hive Queries](machine-learning-data-science-move-hive-tables.md#submit).</span></span>

## <span data-ttu-id="b67a6-119"><a name="uniform"></a>Egységes véletlenszerű mintavétel</span><span class="sxs-lookup"><span data-stu-id="b67a6-119"><a name="uniform"></a> Uniform random sampling</span></span>
<span data-ttu-id="b67a6-120">Egységes véletlenszerű mintavételi jelenti, hogy az adatkészlet egyes soraihoz alatt mintát egyenlő esélyét.</span><span class="sxs-lookup"><span data-stu-id="b67a6-120">Uniform random sampling means that each row in the data set has an equal chance of being sampled.</span></span> <span data-ttu-id="b67a6-121">Ez az ad hozzá egy kiegészítő mezőt rand() az adatkészletben a belső "select" lekérdezési, és a külső "select" lekérdezési feltételhez véletlenszerű mező valósítható meg.</span><span class="sxs-lookup"><span data-stu-id="b67a6-121">This can be implemented by adding an extra field rand() to the data set in the inner "select" query, and in the outer "select" query that condition on that random field.</span></span>

<span data-ttu-id="b67a6-122">Íme egy példa lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="b67a6-122">Here is an example query:</span></span>

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

<span data-ttu-id="b67a6-123">Itt `<sample rate, 0-1>` azt jelzi, hogy a felhasználók mintát szeretne arányát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="b67a6-123">Here, `<sample rate, 0-1>` specifies the proportion of records that the users want to sample.</span></span>

## <span data-ttu-id="b67a6-124"><a name="group"></a>A csoportok véletlenszerű mintavétel</span><span class="sxs-lookup"><span data-stu-id="b67a6-124"><a name="group"></a> Random sampling by groups</span></span>
<span data-ttu-id="b67a6-125">Ha mintavételi kategorikus adatok, érdemes lehet akár vagy kizárja a minden egyes adott kategorikus változó értékének előfordulása.</span><span class="sxs-lookup"><span data-stu-id="b67a6-125">When sampling categorical data, you may want to either include or exclude all of the instances of some particular value of a categorical variable.</span></span> <span data-ttu-id="b67a6-126">Ez a "csoport mintavételi" Mit jelent.</span><span class="sxs-lookup"><span data-stu-id="b67a6-126">This is what is meant by "sampling by group".</span></span>
<span data-ttu-id="b67a6-127">Például ha kategorikus változó "Állapot", amelynek értékek NY MA, CA, NJ, PA, stb akkor, rekordjának olyan állapotban kell mindig együtt, hogy azok lekérdező vagy nem.</span><span class="sxs-lookup"><span data-stu-id="b67a6-127">For example, if you have a categorical variable "State", which has values NY, MA, CA, NJ, PA, etc, you want records of the same state be always together, whether they are sampled or not.</span></span>

<span data-ttu-id="b67a6-128">Íme példalekérdezést adott minták csoport szerint:</span><span class="sxs-lookup"><span data-stu-id="b67a6-128">Here is an example query that samples by group:</span></span>

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

## <span data-ttu-id="b67a6-129"><a name="stratified"></a>Rétegzett mintavétel</span><span class="sxs-lookup"><span data-stu-id="b67a6-129"><a name="stratified"></a>Stratified sampling</span></span>
<span data-ttu-id="b67a6-130">Véletlenszerű mintavételi van műanyaggal rétegezett tekintetében kategorikus változó Ha kapott minták értékeit, hogy kategorikus, amelyek egy-egy beállítani, mint a szülő feltöltését, ahol a minták származik.</span><span class="sxs-lookup"><span data-stu-id="b67a6-130">Random sampling is stratified with respect to a categorical variable when the samples obtained have values of that categorical that are in the same ratio as in the parent population from which the samples were obtained.</span></span> <span data-ttu-id="b67a6-131">Az előző példát, a fenti tegyük fel, hogy az adatok részleges feltöltések rendelkezik államok, például NJ 100 megfigyelések, NY 60 megfigyelések van, pedig WA 300 megfigyelések.</span><span class="sxs-lookup"><span data-stu-id="b67a6-131">Using the same example as above, suppose your data has sub-populations by states, say NJ has 100 observations, NY has 60 observations, and WA has 300 observations.</span></span> <span data-ttu-id="b67a6-132">Ha 0,5 kell rétegzett mintavételi arány ad meg, majd a mintát a kell rendelkeznie körülbelül 50, 30 és 150 megfigyelések NJ, NY és WA rendre.</span><span class="sxs-lookup"><span data-stu-id="b67a6-132">If you specify the rate of stratified sampling to be 0.5, then the sample obtained should have approximately 50, 30, and 150 observations of NJ, NY, and WA respectively.</span></span>

<span data-ttu-id="b67a6-133">Íme egy példa lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="b67a6-133">Here is an example query:</span></span>

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


<span data-ttu-id="b67a6-134">Információ a speciális mintavételi felhasználható módszerek közül a Hive: [LanguageManual mintavételi](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Sampling).</span><span class="sxs-lookup"><span data-stu-id="b67a6-134">For information on more advanced sampling methods that are available in Hive, see [LanguageManual Sampling](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Sampling).</span></span>

