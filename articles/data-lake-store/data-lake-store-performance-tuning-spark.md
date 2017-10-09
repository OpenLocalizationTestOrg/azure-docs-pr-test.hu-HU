---
title: "Data Lake Store Spark teljesítményének hangolása irányelveit aaaAzure |} Microsoft Docs"
description: "Az Azure Data Lake Store Spark teljesítményének hangolása irányelvek"
services: data-lake-store
documentationcenter: 
author: stewu
manager: amitkul
editor: stewu
ms.assetid: ebde7b9f-2e51-4d43-b7ab-566417221335
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/19/2016
ms.author: stewu
ms.openlocfilehash: da1d172e9cb1199ad95605ea1718e78559f79650
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="performance-tuning-guidance-for-spark-on-hdinsight-and-azure-data-lake-store"></a>A Spark on HDInsight és az Azure Data Lake Store útmutatást teljesítményhangolása

Spark teljesítményének hangolása, úgy kell tooconsider hello számát, a fürtön futó alkalmazások.  Alapértelmezés szerint 4 futtatható egyidejűleg a HDI-fürtön lévő alkalmazások (Megjegyzés: hello alapértelmezett beállítás: tulajdonos toochange).  Dönthet toouse kevesebb alkalmazások így hello alapértelmezett beállításainak felülbírálása és az alkalmazások több hello fürt használja.  

## <a name="prerequisites"></a>Előfeltételek

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).
* **Egy Azure Data Lake Store-fiók**. Útmutatást toocreate egy, lásd: [Ismerkedés az Azure Data Lake Store](data-lake-store-get-started-portal.md)
* **Az Azure HDInsight-fürt** a Data Lake Store-fiók hozzáférési tooa. Lásd: [HDInsight-fürtök létrehozása a Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md). Ellenőrizze, hogy engedélyezte a távoli asztal hello fürt.
* **Spark-fürtön futó Azure Data Lake Store**.  További információkért lásd: [használata a HDInsight Spark fürt tooanalyze adatok Data Lake Store-ban](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-use-with-data-lake-store)
* **Teljesítményhangolás ADLS iránymutatást**.  Általános teljesítmény fogalmakat, lásd: [Data Lake Store teljesítmény hangolása útmutató](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance) 

## <a name="parameters"></a>Paraméterek

Amikor Spark futó feladatok, az alábbiakban hello legfontosabb beállítások ADLS bennünket tooincrease teljesítményére lehet:

* **NUM-végrehajtója** -hello száma párhuzamos feladatokat hajt végre.

* **Végrehajtó memóriában** -hello tooeach végrehajtó lefoglalt memória mennyiségét.

* **Végrehajtó-magok** -magok száma hello lefoglalt tooeach végrehajtó.                     

**NUM-végrehajtója** Num-végrehajtója állítja hello párhuzamosan futó feladatok maximális számát.  hello tényleges száma párhuzamos futtatható tevékenységek hello memória és a fürt rendelkezésre álló Processzor-erőforrások korlátozódik.

**Végrehajtó memóriában** Ez az hello tooeach végrehajtó lefoglalt memória mennyiségét.  az egyes végrehajtó szükséges hello memória hello feladat függ.  Összetett műveletek hello memória nagyobb toobe kell.  Egyszerű műveletek tartoznak, mint az olvasási és írási alacsonyabb lesz memóriára vonatkozó követelményeknek.  az egyes végrehajtó memória mennyisége hello Ambari tekintheti meg.  Az Ambari keresse meg a tooSpark, és hello Configs lapon.  

**Végrehajtó-magok** végrehajtó, amely megadja, hogy futtatható végrehajtó száma párhuzamos szálak száma hello használt magok hello mennyisége állít be.  Például ha végrehajtó-magok = 2, majd minden egyes végrehajtó hello végrehajtó 2 párhuzamos feladatokat futtathat.  hello végrehajtó-mag szükséges fognak függeni hello feladat.  Intenzív i/o-feladatok nem igényelnek nagy mennyiségű memória feladat, ezért minden végrehajtó kezelni tud a további párhuzamos tevékenységeket.

Alapértelmezés szerint két virtuális YARN magok megadva minden fizikai magok a HDInsight Spark futtatásakor.  Ez a szám concurrecy és a környezetben több szál való váltás egyensúlyt biztosít.  

## <a name="guidance"></a>Útmutatás

Futtatásakor a Spark koncepción alapuló adatelemzési célokra toowork a Data Lake Store-adatokkal, azt javasoljuk, hogy a Data Lake Store hello legutóbbi HDInsight verzió tooget hello legjobb teljesítmény érdekében használjon. Ha a feladat több i/o-igényes, bizonyos paraméterek konfigurált tooimprove teljesítmény lehet.  Azure Data Lake Store egy kiválóan méretezhető tárolás platform, amely képes kezelni a magas teljesítmény.  Ha hello feladat főként olvasási vagy írási műveleteket, majd növelése az Azure Data Lake Store az i/o-tooand CONCURRENCY paraméterének értékét sikerült teljesítmény növelése érdekében.

Nincsenek néhány általános módon tooincrease egyidejűségi i/o-igényes feladatok.

**1. lépés: Annak ellenőrzése, hogy hány alkalmazások futnak, a fürt** – tudnia kell, beleértve az aktuális hello hello fürtön hány alkalmazások futnak.  hello alapértelmezett értékeinek minden Spark beállítás feltételezi, hogy problémamentes-4 egyidejűleg futó alkalmazások.  Ezért csak akkor hello fürt minden alkalmazás elérhető 25 %-át.  tooget jobb teljesítmény érdekében felülírhatja hello alapértelmezett végrehajtója hello számának módosítása.  

**2. lépés: Állítsa be az executor-memória** – hello először thing tooset hello végrehajtó memóriában.  hello memória fogja, hogy-e folyamatban toorun hello feladat függ.  Párhuzamossági növelheti, ha a végrehajtó kevesebb memória lefoglalásakor.  Ha memória kivételek kívül a feladat futtatásakor, majd növelje a paraméter értéke hello.  Egy alternatív az tooget további memória nagyobb mennyiségű memóriával rendelkező fürtnek használatával, vagy a fürt hello méretének növelését.  További memória lehetővé teszi több végrehajtója toobe használja, ami azt jelenti, hogy több egyidejű.

**3. lépés: Állítsa be az executor-magok** – i/o teljesítményigényű munkaterhelések kiszolgálásához, amelyek nem rendelkeznek összetett műveletek a helyes toostart végrehajtó-magok tooincrease hello száma párhuzamos tevékenységek maximális száma végrehajtó nagy számú esetén.  Remek kezdőpont végrehajtó-magok too4 beállítás.   

    executor-cores = 4
Hello végrehajtó-magok száma növelése Erre azért van szükség további párhuzamossági, különböző végrehajtó-magok kísérletezhet.  Az összetettebb műveleteket, feladatok csökkentse hello végrehajtó magok számát.  Ha végrehajtó-magok nagyobb, mint 4, majd szemétgyűjtés előfordulhat, hogy nem elég hatékony lesz és csökkentheti a teljesítményt.

**4. lépés: Fürt YARN memóriamennyiség meghatározása** – ezt az információt az Ambari érhető el.  Keresse meg a tooYARN és hello Configs lapon.  hello YARN memória ebben az ablakban jelenik meg.  
Megjegyzés: amikor hello ablakban van, látható az is hello alapértelmezett YARN tároló mérete.  hello YARN tároló mérete van hello ugyanaz, mint a memória mennyisége végrehajtó paramétere.

    Total YARN memory = nodes * YARN memory per node
**5. lépés: Num-végrehajtója kiszámítása**

**Memória megkötés kiszámításához** -hello num-végrehajtója paraméter memória vagy CPU korlátozza.  hello memória megkötés hello az alkalmazáshoz rendelkezésre álló YARN memória mennyisége határozza meg.  Teljes YARN memória érvénybe kell, és osztás, amely végrehajtó memóriában.  hello korlátozást kell deszerializálni méretezhető alkalmazások hello számát, azt az alkalmazások számának hello nullával toobe.

    Memory constraint = (total YARN memory / executor memory) / # of apps   
**CPU-korlátozás kiszámításához** -hello CPU-korlátozás, végrehajtó magok számát hello végzett teljes virtuális magok hello kiszámítása.  Nincsenek minden fizikai magok virtuális 2 magos.  Hasonló toohello memória megkötés van osztási alkalmazások hello száma alapján.

    virtual cores = (nodes in cluster * # of physical cores in node * 2)
    CPU constraint = (total virtual cores / # of cores per executor) / # of apps
**Num-végrehajtója beállítása** – hello num-végrehajtója paraméter határozza meg véve hello legalább hello memória korlátozás és a hello CPU-korlátozás. 

    num-executors = Min (total virtual Cores / # of cores per executor, available YARN memory / executor-memory)   
Ha egy nagyobb num-végrehajtója nem feltétlenül teljesítmény növelése.  Vegye figyelembe, hogy további végrehajtója felveszi extra általános az egyes további végrehajtó, amely potenciálisan ronthatja a teljesítményt.  NUM-végrehajtója hello fürterőforrások korlátozódik.    

## <a name="example-calculation"></a>Példa kiszámítása

Tételezzük fel alkalmazásokat hello akár egy fog toorun futó 2 álló 8 D4v2 csomópont fürtökben-tal rendelkezik.  

**1. lépés: Annak ellenőrzése, hogy hány alkalmazások futnak, a fürt** –, hogy rendelkezik-e 2 ismeri a fürtben, beleértve az egyik toorun fog hello alkalmazásokat.  

**2. lépés: Állítsa be az executor-memória** – ebben a példában azt határozza meg, hogy 6 GB memória végrehajtó intenzív i/o-feladat elegendő lesz-e.  

    executor-memory = 6GB
**3. lépés: Állítsa be az executor-magok** – mivel ez egy i/o-igényes feladat, azt állíthatja be az egyes végrehajtó too4 magok hello száma.  Magok száma végrehajtó toolarger beállítást, mint 4 szemétgyűjtési gyűjtemény problémákat okozhat.  

    executor-cores = 4
**4. lépés: Fürt YARN memóriamennyiség meghatározása** – azt keresse meg a tooAmbari toofind ki, hogy minden D4v2 rendelkezik-e a YARN memória 25 GB.  Mivel nincsenek 8 csomópont, a rendszer megszorozza hello YARN memória 8.

    Total YARN memory = nodes * YARN memory* per node
    Total YARN memory = 8 nodes * 25GB = 200GB
**5. lépés: Kiszámításához num-végrehajtója** – hello num-végrehajtója paraméter határozza meg véve hello hello memória korlátozás és a hello CPU-korlátozás hello osztva a minimális száma Spark futó alkalmazásokra.    

**Memória megkötés kiszámításához** – hello memória megkötés számítása hello YARN memória összesen hello memória mennyisége végrehajtó hányadosa.

    Memory constraint = (total YARN memory / executor memory) / # of apps   
    Memory constraint = (200GB / 6GB) / 2   
    Memory constraint = 16 (rounded)
**CPU-korlátozás kiszámításához** -hello CPU-korlátozás, teljes yarn magok hello végrehajtó magok száma osztva hello kiszámítása.
    
    YARN cores = nodes in cluster * # of cores per node * 2   
    YARN cores = 8 nodes * 8 cores per D14 * 2 = 128
    CPU constraint = (total YARN cores / # of cores per executor) / # of apps
    CPU constraint = (128 / 4) / 2
    CPU constraint = 16
**Num-végrehajtója beállítása**

    num-executors = Min (memory constraint, CPU constraint)
    num-executors = Min (16, 16)
    num-executors = 16    

