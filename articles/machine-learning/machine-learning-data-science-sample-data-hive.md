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
# <a name="sample-data-in-azure-hdinsight-hive-tables"></a><span data-ttu-id="9a0f3-103">Adatmintavétel az Azure HDInsight Hive-táblákban</span><span class="sxs-lookup"><span data-stu-id="9a0f3-103">Sample data in Azure HDInsight Hive tables</span></span>
<span data-ttu-id="9a0f3-104">Ebben a cikkben azt ismertetik a Hive-lekérdezésekkel Azure HDInsight Hive táblák hogyan toodown-minta adataihoz.</span><span class="sxs-lookup"><span data-stu-id="9a0f3-104">In this article, we describe how toodown-sample data stored in Azure HDInsight Hive tables using Hive queries.</span></span> <span data-ttu-id="9a0f3-105">Azt a három popularly használt mintavételi módszerek terjed ki:</span><span class="sxs-lookup"><span data-stu-id="9a0f3-105">We cover three popularly used sampling methods:</span></span>

* <span data-ttu-id="9a0f3-106">Egységes véletlenszerű mintavétel</span><span class="sxs-lookup"><span data-stu-id="9a0f3-106">Uniform random sampling</span></span>
* <span data-ttu-id="9a0f3-107">A csoportok véletlenszerű mintavétel</span><span class="sxs-lookup"><span data-stu-id="9a0f3-107">Random sampling by groups</span></span>
* <span data-ttu-id="9a0f3-108">Rétegzett mintavétel</span><span class="sxs-lookup"><span data-stu-id="9a0f3-108">Stratified sampling</span></span>

<span data-ttu-id="9a0f3-109">hello következő **menü** hivatkozásokat tartalmaz, amelyek ismertetik tootopics hogyan toosample adatait tároló különböző környezetekben.</span><span class="sxs-lookup"><span data-stu-id="9a0f3-109">hello following **menu** links tootopics that describe how toosample data from various storage environments.</span></span>

[!INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

<span data-ttu-id="9a0f3-110">**Miért érdemes az az adatokat?**</span><span class="sxs-lookup"><span data-stu-id="9a0f3-110">**Why sample your data?**</span></span>
<span data-ttu-id="9a0f3-111">Ha azt tervezi, hogy tooanalyze hello adatkészlet túl nagy, a rendszer általában egy jó ötlet toodown-minta hello adatok tooreduce azt tooa kisebb, de reprezentatív és könnyebben kezelhető méretét.</span><span class="sxs-lookup"><span data-stu-id="9a0f3-111">If hello dataset you plan tooanalyze is large, it's usually a good idea toodown-sample hello data tooreduce it tooa smaller but representative and more manageable size.</span></span> <span data-ttu-id="9a0f3-112">Ez lehetővé teszi az adatok ismertetése, feltárása és a szolgáltatás mérnöki csapathoz.</span><span class="sxs-lookup"><span data-stu-id="9a0f3-112">This facilitates data understanding, exploration, and feature engineering.</span></span> <span data-ttu-id="9a0f3-113">Hello csapat az tudományos folyamata a szerepköre tooenable gyors prototípusának hello adatfeldolgozási funkciók és a gépi tanulási modellek.</span><span class="sxs-lookup"><span data-stu-id="9a0f3-113">Its role in hello Team Data Science Process is tooenable fast prototyping of hello data processing functions and machine learning models.</span></span>

<span data-ttu-id="9a0f3-114">Ez a mintavételi feladat ez hello lépés [Team adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="9a0f3-114">This sampling task is a step in hello [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

## <a name="how-toosubmit-hive-queries"></a><span data-ttu-id="9a0f3-115">Hogyan toosubmit Hive-lekérdezések</span><span class="sxs-lookup"><span data-stu-id="9a0f3-115">How toosubmit Hive queries</span></span>
<span data-ttu-id="9a0f3-116">Hive-lekérdezéseket a hello hello Hadoop-fürt átjárócsomópontjához hello Hadoop parancssori konzolról küldheti el.</span><span class="sxs-lookup"><span data-stu-id="9a0f3-116">Hive queries can be submitted from hello Hadoop Command Line console on hello head node of hello Hadoop cluster.</span></span> <span data-ttu-id="9a0f3-117">toodo ez, jelentkezzen be a hello hello Hadoop-fürt átjárócsomópontjához nyissa meg a hello Hadoop parancssori konzolt, és küldje el onnan hello Hive-lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="9a0f3-117">toodo this, log into hello head node of hello Hadoop cluster, open hello Hadoop Command Line console, and submit hello Hive queries from there.</span></span> <span data-ttu-id="9a0f3-118">Hive-lekérdezések hello Hadoop parancssori konzolon elküldése, lásd: [hogyan tooSubmit Hive-lekérdezések](machine-learning-data-science-move-hive-tables.md#submit).</span><span class="sxs-lookup"><span data-stu-id="9a0f3-118">For instructions on submitting Hive queries in hello Hadoop Command Line console, see [How tooSubmit Hive Queries](machine-learning-data-science-move-hive-tables.md#submit).</span></span>

## <span data-ttu-id="9a0f3-119"><a name="uniform"></a>Egységes véletlenszerű mintavétel</span><span class="sxs-lookup"><span data-stu-id="9a0f3-119"><a name="uniform"></a> Uniform random sampling</span></span>
<span data-ttu-id="9a0f3-120">Egységes véletlenszerű mintavételi jelenti, hogy minden egyes sorára hello adatkészlet alatt mintát egyenlő esélyét.</span><span class="sxs-lookup"><span data-stu-id="9a0f3-120">Uniform random sampling means that each row in hello data set has an equal chance of being sampled.</span></span> <span data-ttu-id="9a0f3-121">Egy kiegészítő mezőt rand() toohello adatkészlet hello belső "select" lekérdezési való hozzáadásával valósítható, és a hello külső "select" lekérdezési feltételtől véletlenszerű a mezőhöz.</span><span class="sxs-lookup"><span data-stu-id="9a0f3-121">This can be implemented by adding an extra field rand() toohello data set in hello inner "select" query, and in hello outer "select" query that condition on that random field.</span></span>

<span data-ttu-id="9a0f3-122">Íme egy példa lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="9a0f3-122">Here is an example query:</span></span>

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

<span data-ttu-id="9a0f3-123">Itt `<sample rate, 0-1>` hello felhasználók kívánt toosample rekordok hello arányát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="9a0f3-123">Here, `<sample rate, 0-1>` specifies hello proportion of records that hello users want toosample.</span></span>

## <span data-ttu-id="9a0f3-124"><a name="group"></a>A csoportok véletlenszerű mintavétel</span><span class="sxs-lookup"><span data-stu-id="9a0f3-124"><a name="group"></a> Random sampling by groups</span></span>
<span data-ttu-id="9a0f3-125">Ha a mintavételi kategorikus adatok, érdemes lehet tooeither hatókörébe felvenni és kizárni összes hello példányai néhány adott egy kategorikus változó értékét.</span><span class="sxs-lookup"><span data-stu-id="9a0f3-125">When sampling categorical data, you may want tooeither include or exclude all of hello instances of some particular value of a categorical variable.</span></span> <span data-ttu-id="9a0f3-126">Ez a "csoport mintavételi" Mit jelent.</span><span class="sxs-lookup"><span data-stu-id="9a0f3-126">This is what is meant by "sampling by group".</span></span>
<span data-ttu-id="9a0f3-127">Például ha kategorikus változó "Állapot", amelynek értékek szerepelnek NY, MA, CA, NJ, PA stb, rekordokat szeretne hello olyan állapotban lennie mindig együtt, hogy azok lekérdező vagy sem.</span><span class="sxs-lookup"><span data-stu-id="9a0f3-127">For example, if you have a categorical variable "State", which has values NY, MA, CA, NJ, PA, etc, you want records of hello same state be always together, whether they are sampled or not.</span></span>

<span data-ttu-id="9a0f3-128">Íme példalekérdezést adott minták csoport szerint:</span><span class="sxs-lookup"><span data-stu-id="9a0f3-128">Here is an example query that samples by group:</span></span>

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

## <span data-ttu-id="9a0f3-129"><a name="stratified"></a>Rétegzett mintavétel</span><span class="sxs-lookup"><span data-stu-id="9a0f3-129"><a name="stratified"></a>Stratified sampling</span></span>
<span data-ttu-id="9a0f3-130">Véletlenszerű mintavételi műanyaggal van rétegezett tekintetben tooa kategorikus változóval Ha kapott hello minták rendelkezik, amely kategorikus értékének a hello méretarány hasonlóan hello szülő feltöltése a mely hello minták származik.</span><span class="sxs-lookup"><span data-stu-id="9a0f3-130">Random sampling is stratified with respect tooa categorical variable when hello samples obtained have values of that categorical that are in hello same ratio as in hello parent population from which hello samples were obtained.</span></span> <span data-ttu-id="9a0f3-131">Ugyanebben a példában a fenti, használatával hello tegyük fel, hogy az adatok részleges feltöltések államok, azaz NJ van 100 megfigyelések, NY rendelkezik 60 megfigyelések, pedig WA 300 megfigyelések.</span><span class="sxs-lookup"><span data-stu-id="9a0f3-131">Using hello same example as above, suppose your data has sub-populations by states, say NJ has 100 observations, NY has 60 observations, and WA has 300 observations.</span></span> <span data-ttu-id="9a0f3-132">Ha hello aránya adja meg mintavételi toobe 0,5 műanyaggal rétegezett, majd hello mintát a kell körülbelül 50, 30 és 150 megfigyelések NJ, NY és WA kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="9a0f3-132">If you specify hello rate of stratified sampling toobe 0.5, then hello sample obtained should have approximately 50, 30, and 150 observations of NJ, NY, and WA respectively.</span></span>

<span data-ttu-id="9a0f3-133">Íme egy példa lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="9a0f3-133">Here is an example query:</span></span>

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


<span data-ttu-id="9a0f3-134">Információ a speciális mintavételi felhasználható módszerek közül a Hive: [LanguageManual mintavételi](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Sampling).</span><span class="sxs-lookup"><span data-stu-id="9a0f3-134">For information on more advanced sampling methods that are available in Hive, see [LanguageManual Sampling](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Sampling).</span></span>

