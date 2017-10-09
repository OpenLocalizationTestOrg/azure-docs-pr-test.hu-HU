---
title: "Data Lake Store MapReduce teljesítményének hangolása irányelveit aaaAzure |} Microsoft Docs"
description: "Az Azure Data Lake Store MapReduce teljesítményének hangolása irányelvek"
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
ms.openlocfilehash: e21414a23530e65613c85156af0209c88ec54d2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="performance-tuning-guidance-for-mapreduce-on-hdinsight-and-azure-data-lake-store"></a>Útmutató a HDInsight és az Azure Data Lake Store MapReduce teljesítményhangolása


## <a name="prerequisites"></a>Előfeltételek

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).
* **Egy Azure Data Lake Store-fiók**. Útmutatást toocreate egy, lásd: [Ismerkedés az Azure Data Lake Store](data-lake-store-get-started-portal.md)
* **Az Azure HDInsight-fürt** a Data Lake Store-fiók hozzáférési tooa. Lásd: [HDInsight-fürtök létrehozása a Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md). Ellenőrizze, hogy engedélyezte a távoli asztal hello fürt.
* **MapReduce használata a HDInsight**.  További információkért lásd: [használata MapReduce a Hadoop on HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-use-mapreduce)  
* **Teljesítményhangolás ADLS iránymutatást**.  Általános teljesítmény fogalmakat, lásd: [Data Lake Store teljesítmény hangolása útmutató](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance)  

## <a name="parameters"></a>Paraméterek

MapReduce-feladatok futtatásakor az alábbiakban is konfigurált tooincrease teljesítmény ADLS hello legfontosabb paraméterek:

* **Mapreduce.Map.Memory.MB** – hello mennyiségű memória tooallocate tooeach leképezője
* **Mapreduce.job.Maps** – hello térkép feladatok feladat másodpercenkénti száma
* **Mapreduce.reduce.Memory.MB** – hello mennyiségű memória tooallocate tooeach nyomáscsökkentő
* **Mapreduce.job.reduces** – hello csökkentse feladatok feladat másodpercenkénti száma

**Mapreduce.Map.Memory / Mapreduce.reduce.memory** ezt a számot kell igazítani, hello térkép szükséges mekkora memóriát és/vagy csökkentse a tevékenység.  hello alapértelmezett értékek mapreduce.map.memory és mapreduce.reduce.memory hello Yarn konfigurációs az Ambari tekinthetők.  Az Ambari keresse meg a tooYARN, és hello Configs lapon.  hello YARN memória jelenik meg.  

**Mapreduce.job.Maps / Mapreduce.job.reduces** Ez határozza meg a létrehozott mappers vagy szűkítő toobe hello maximális számát.  elágazást hello számát határozza meg, hány mappers hello MapReduce feladatot hoz létre.  Ezért kevesebb, mint a kért vannak-e a kért mappers hello számánál kisebb elágazást mappers jelenhet meg.       

## <a name="guidance"></a>Útmutatás

**1. lépés: A futó feladatok száma meghatározása** -MapReduce alapértelmezés szerint hello fürtözési használja a feladatot.  Kisebb hello fürt kisebb, mint amennyi a rendelkezésre álló tárolók mappers segítségével is használhatók.  Ez a dokumentum útmutatást hello azt feltételezi, hogy az alkalmazás hello csak az alkalmazáshoz a fürtben futó.      

**2. lépés:, Állítsa be mapreduce.map.memory/mapreduce.reduce.memory** – hello térkép hello memória méretét, és csökkentse feladatok lesz az adott feladat függ.  Ha azt szeretné, hogy tooincrease egyidejű csökkentése érdekében hello memória méretét.  hello egyidejűleg futó feladatok száma attól függ, hogy a tárolók hello száma.  Eseményleképező vagy nyomáscsökkentő memória mennyisége hello csökkentésével létrehozhatók, amelyek lehetővé teszik több mappers vagy szűkítő toorun egyidejűleg több tároló.  Memória mennyisége csökkenő hello túlságosan okozhat egyes folyamatok toorun kevés a memória.  Ha egy halommemóriában hibaüzenet jelenik meg a feladat futtatásakor, növelni kell hello memória mennyisége vagy az nyomáscsökkentő.  Vegye figyelembe, hogy több tároló felveszi extra általános, minden további tároló, amely potenciálisan ronthatja a teljesítményt.  Egy másik alternatív az tooget további memória nagyobb mennyiségű memóriával rendelkező fürtnek használatával, vagy hello a fürtben található csomópontok számának növelését.  További memória lehetővé teszi további tárolók toobe használja, ami azt jelenti, hogy több egyidejű.  

**3. lépés: Teljes YARN memória meghatározása** -tootune mapreduce.job.maps/mapreduce.job.reduces, érdemes lehet hello memóriamennyiség teljes YARN használatra.  Ez az információ Ambari érhető el.  Keresse meg a tooYARN és hello Configs lapon.  hello YARN memória ebben az ablakban jelenik meg.  A fürt tooget hello teljes YARN memóriában kell szorozza meg a csomópontok száma hello hello YARN memória.

    Total YARN memory = nodes * YARN memory per node
Ha egy üres fürtöt használ, memória hello teljes YARN memória a fürt lehet.  Ha más alkalmazások használják memóriát, majd kiválaszthatja tooonly használja a fürt memória azon része tárolók mappers vagy szűkítő toohello száma hello számának csökkentésével toouse szeretné.  

**4. lépés: A YARN a tárolók száma kiszámításához** – YARN tárolók diktálni hello párhuzamossági hello feladat érhető el.  Teljes YARN memória eltarthat, és osztás, amely mapreduce.map.memory.  

    # of YARN containers = total YARN memory / mapreduce.map.memory

**5. lépés:, Állítsa be mapreduce.job.maps/mapreduce.job.reduces** mapreduce.job.maps/mapreduce.job.reduces tooat beállítása rendelkezésre álló tárolók száma legalább hello.  Kísérletezhet további mappers és szűkítő toosee hello száma növelésével, ha a jobb teljesítmény eléréséhez.  Ne feledje, hogy a további mappers lesz-e további erőforrásokra, hogy túl sok mappers ronthatja a teljesítményt.  

CPU ütemezés és a CPU-elkülönítési ki vannak kapcsolva alapértelmezés szerint, a memória YARN tárolók hello száma korlátozza.

## <a name="example-calculation"></a>Példa kiszámítása

Tegyük fel, a fürt 8 D14 csomópont álló-tal rendelkezik, és azt szeretné, hogy egy i/o-igényes feladat toorun.  Az alábbiakban hello számítások kell tennie:

**1. lépés: A futó feladatok száma meghatározása** -példában feltételezzük, hogy a feladat csak egy futó program hello.  

**2. lépés:, Állítsa be mapreduce.map.memory/mapreduce.reduce.memory** – a fenti példában egy i/o-igényes feladat fut, és döntse el, hogy elegendő lesz-e a 3 GB memóriát a térkép feladatokhoz.

    mapreduce.map.memory = 3GB
**3. lépés: Teljes YARN memória meghatározása**

    total memory from hello cluster is 8 nodes * 96GB of YARN memory for a D14 = 768GB
**4. lépés: # YARN tároló kiszámítása**

    # of YARN containers = 768GB of available memory / 3 GB of memory =   256

**5. lépés: Mapreduce.job.maps/mapreduce.job.reduces beállítása**

    mapreduce.map.jobs = 256

## <a name="limitations"></a>Korlátozások

**ADLS-szabályozás**

Több-bérlős szolgáltatásként ADLS állítja be a fiók szintű sávszélesség korlátja.  Ha ezek a korlátozások kattint, elindul a toosee feladat hibáihoz. Ez úgy azonosítható betartásával szabályozási hibák feladat naplókban által.  Ha a feladat több sávszélesség van szüksége, lépjen kapcsolatba velünk a következő címen.   

Ha Ön első szabályozott toocheck, tooenable hello hibakeresési naplózás hello ügyféloldalon kell. Ez hogyan azt teheti meg:

1. PUT hello tulajdonság hello log4j tulajdonságai Ambari a következő > YARN > konfigurációs > speciális yarn-log4j: log4j.logger.com.microsoft.azure.datalake.store=DEBUG

2. Indítsa újra az összes hello csomópontok/szolgáltatást hello config tootake hatást.

3. Ha Ön első szabályozott, látni fogja, hello HTTP 429 hibakód hello YARN naplófájlban. hello YARN napló fájl van-e /tmp/&lt;felhasználói&gt;/yarn.log

## <a name="examples-toorun"></a>Példák tooRun

toodemonstrate hogyan MapReduce fut az Azure Data Lake Store alábbiakban található néhány mintakód hello-beállítások a következő fürtön futtatott:

* 16 csomópont D14v2
* Hadoop-fürt HDI 3.6 fut

A kiindulási pont az alábbiakban néhány példa parancsok toorun MapReduce Teragen Terasort és Teravalidate.  Beállíthatja, hogy ezek a parancsok a erőforrások alapján.

**Teragen**

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapreduce.job.maps=2048 -Dmapreduce.map.memory.mb=3072 10000000000 adl://example/data/1TB-sort-input

**Terasort**

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapreduce.job.maps=2048 -Dmapreduce.map.memory.mb=3072 -Dmapreduce.job.reduces=512 -Dmapreduce.reduce.memory.mb=3072 adl://example/data/1TB-sort-input adl://example/data/1TB-sort-output

**Teravalidate**

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapreduce.job.maps=512 -Dmapreduce.map.memory.mb=3072 adl://example/data/1TB-sort-output adl://example/data/1TB-sort-validate
