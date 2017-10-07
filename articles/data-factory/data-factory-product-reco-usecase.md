---
title: "aaaData gyári használati eset - termék javaslatok"
description: "További tudnivalók az Azure Data Factory használatával más szolgáltatásokkal együtt használati eset."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 6f1523c7-46c3-4b8d-9ed6-b847ae5ec4ae
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: shlo
ms.openlocfilehash: d7912965fe4762d64e8ca3c28381ea6187f36631
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-case---product-recommendations"></a>Használati eset – Termékajánlások
Az Azure Data Factory sok használt szolgáltatások tooimplement hello Cortana Intelligence Suite a megoldás gyorsítók egyike.  Lásd: [Cortana Intelligence Suite](http://www.microsoft.com/cortanaanalytics) lap ennek a programcsomagnak vonatkozó további információért. Ebben a dokumentumban azt ismertetik, amelyek az Azure felhasználók már lehet megoldani, és Azure Data Factory és az egyéb Cortana Intelligence Komponensszolgáltatások készletével megvalósított gyakori használati eset.

## <a name="scenario"></a>Forgatókönyv
Online kiskereskedőktől gyakran szeretné tooentice ügyfelek toopurchase termékeik segítségével őket azok termékekkel valószínűleg toobe érdekli, és ezért valószínűleg toobuy. tooaccomplish, online kiskereskedőktől kell toocustomize a felhasználó online élményt, hogy az adott felhasználó személyre szabott termék javaslatok használatával. Ezek a személyre szabott javaslatok még toobe vélemények alapján aktuális és korábbi bevásárlási viselkedés adataikat, termékinformációk, újonnan bevezetett márkákat, és a termék- és felhasználói szegmentálást adatok.  Emellett azok megadhatják hello felhasználói termék ajánlások összes felhasználóik kombinált összesített használati viselkedés elemzése alapján.

hello célja az ezeket a kiskereskedőktől toooptimize felhasználói kattintson pénztári átalakítás és vett szolgáltatás érvényessége alatt magasabb értékesítési bevétel.  Környezetfüggő, viselkedés alapú termék ajánlásainkat ügyfél érdeklődési és műveletek mivoltát elérésének az átalakításhoz. A használati eset, az online kiskereskedőktől készült toooptimize szeretné, hogy az ügyfelek számára, például vesszük. Az alapelvek azonban tooany üzleti, hogy tooengage próbál alkalmazni az ügyfelek a termékek és szolgáltatások köré, és az ügyfelek vásárló élmény személyre szabott termék ajánlásokkal.

## <a name="challenges"></a>Kihívásai
Sok akadályok merülnek, hogy online kiskereskedőktől arcfelismerési használati eset az ilyen típusú tooimplement közben. 

Először különböző méretű és alakú adatainak kell fogyasztanak több adatforrásokból, mind a helyszíni és felhőben hello. Ezen adatok tartalmazzák a termék adatokat, a korábbi felhasználói viselkedési adatokat és a felhasználói adatok, hello felhasználói böngészik hello online kereskedelmi webhelyen. 

Második, személyre szabott termék ajánlásokat kell ésszerűen és pontosan számított és előre jelezni. Továbbá tooproduct, a márka és a viselkedést és a böngésző ügyféladatok, online kiskereskedőktől is kell tooinclude ügyfelek visszajelzései alapján, az elmúlt vásárlás toofactor hello legjobb termék javaslatok hello meghatározásakor hello felhasználó számára. 

Harmadik hello ajánlásokat kell azonnal termék toohello felhasználói tooprovide egy zökkenőmentes keresse meg és a felhasználói élmény beszerzési, és hello legutóbbi és a vonatkozó javaslatokkal. 

Végezetül kiskereskedőktől kell toomeasure hello hatékonyságát, a módszer teljes felfelé-értékesít nyomon követésével és kereszt-értékesít kattintson-átalakítás értékesítési sikeres, és tootheir újabb ajánlások beállítása.

## <a name="solution-overview"></a>Megoldási áttekintés
A példa használati eset orvosolhatók és Azure Data Factory és az egyéb Cortana Intelligence Komponensszolgáltatások, beleértve a valódi Azure-felhasználók által megvalósított [HDInsight](https://azure.microsoft.com/services/hdinsight/) és [Power BI](https://powerbi.microsoft.com/).

hello online közvetítő használja, mint az adatok tárolási lehetőségek hello munkafolyamaton keresztül egy Azure Blob-tároló, egy a helyszíni SQL server, Azure SQL Database és a relációs adatközpont.  hello blob a tárolóban ügyféladatok, a felhasználói viselkedési adatokat és a termék adatok. adatok tartalmazzák a termék márkáját adatokat és a termékkatalógus hello termékinformációk a helyszíni SQL data warehouse tárolja. 

Minden hello adatok kombinált és táplált be egy termék javaslat rendszer személyre szabott toodeliver javaslatok alapján, felhasználói érdeklődési és a műveletek, amíg hello felhasználói böngészik hello webhelyen hello katalógusban szereplő termékek. hello ügyfelek is látni a kapcsolódó általános webhely használati szokásokról, amelyek nem kapcsolódó tooany egy felhasználó alapján azok nézi toohello termék termékek.

![nagybetűk diagramok](./media/data-factory-product-reco-usecase/diagram-1.png)

A nyers webes naplófájlok gigabájt jönnek létre naponta hello online közvetítő webhelyről félig strukturált fájlként. hello nyers webes naplófájlokat, és hello ügyfél és a termék katalógusadatokat rendszeresen okozhatnak globálisan telepített adatátvitelt jelölik a Data Factory használatával szolgáltatásként Azure Blob-tárolóba. hello hello napon nyers naplófájlok (által évhez és hónaphoz) használata particionálja a blob Storage tárolóban, a hosszú távú tároláshoz.  [Az Azure HDInsight](https://azure.microsoft.com/services/hdinsight/) használt toopartition hello nyers naplófájlok hello BLOB tároló, a folyamat okozhatnak hello naplókat Hive és a Pig-parancsfájlok segítségével léptékű van. hello adatok particionált webes naplókat, majd a feldolgozott tooextract hello szükséges bemeneti adatok machine learning-javaslat rendszer toogenerate személyre szabott hello termék javaslatokat.

hello javaslat használt hello gépi tanulás ebben a példában a rendszer egy nyílt forráskódú gépi, az ajánlás platform tanulási [Apache Mahout](http://mahout.apache.org/).  Bármely [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) vagy egyéni modell alkalmazott toohello forgatókönyv lehet.  hello Mahout modell használt toopredict hello hasonlóság közötti elemek összesített használati minták alapján hello webhelyen, illetve a toogenerate személyre szabott hello hello egyes felhasználó alapján.

Végezetül hello eredménykészlet személyre szabott termék ajánlások áthelyezett tooa relációs adatközpontba hello közvetítő webhely felhasználás céljából.  hello eredménykészlet is érhető el. a blob storage-ről egy másik alkalmazás, vagy tooadditional tárolók áthelyezése más fogyasztók és a használati eseteket.

## <a name="benefits"></a>Előnyök
Termék javaslat stratégiai optimalizálása, és azt az üzleti céljaihoz igazítása, a hello megoldás hello online közvetítő termékkihelyezési és célok marketing teljesül. Emellett képes toooperationalize volt, és hello termék javaslat munkafolyamat egy hatékony, megbízható és költséghatékony módon kezelése. hello megközelítés megkönnyítette számukra tooupdate a minta és értékesítési kattintson-átalakítás sikeres hello mértékeinek alapján hatékonyságának finomhangolásához. Azure Data Factory használatával azok képes tooabandon azok időigényes és költséges manuális felhő erőforrás-kezelés és helyezze át a tooon igény felhő erőforrás-kezelést. Ezért képes toosave idő, pénzt, volt, és csökkentheti a toosolution üzembe helyezés. Leszármaztatás adatnézetek és működési szolgáltatásának állapota könnyen toovisualize vált, és hello intuitív adat-előállító ellenőrzése és kezelése a felhasználói felület érhetők el az Azure-portálon hello elhárítása. A megoldás most ütemezett és felügyelt, hogy befejeződött az adatok megbízhatóan előállított és toousers-i, adatokat és feldolgozási függőségek automatikusan kezeli emberi beavatkozás nélkül.

Ez a személyre szabott vásárlásra szolgáló felület biztosításával hello létrehozott verseny, kommunikáció zajlik az ügyfél online közvetítő észlelnek, és ezért az értékesítési és a teljes ügyfelek elégedettségének növelése.

