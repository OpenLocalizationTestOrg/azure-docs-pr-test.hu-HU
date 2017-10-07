---
title: "Data Lake Store kereszt-régió áttelepítési aaaAzure |} Microsoft Docs"
description: "További tudnivalók az Azure Data Lake Store kereszt-régió történő áttelepítés."
services: data-lake-store
documentationcenter: 
author: swums
manager: amitkul
editor: swums
ms.assetid: ebde7b9f-2e51-4d43-b7ab-566417221335
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/27/2017
ms.author: stewu
ms.openlocfilehash: 561ac821c1bd555886035867678cb685997564eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-data-lake-store-across-regions"></a>Telepítse át a Data Lake Store régiók között

Amint az Azure Data Lake Store új régióban elérhetővé válik, akkor célszerű használni toodo egyszeri áttelepítés, hello új régió tootake előnyeit. Ismerje meg milyen tooconsider, tervezze meg és hello áttelepítése.

## <a name="prerequisites"></a>Előfeltételek

* **Azure-előfizetés**. További információkért lásd: [ma létrehozása az ingyenes Azure-fiókjával](https://azure.microsoft.com/pricing/free-trial/).
* **A Data Lake Store-fiók két különböző régiókban**. További információkért lásd: [Ismerkedés az Azure Data Lake Store](data-lake-store-get-started-portal.md).
* **Az Azure Data Factory**. További információkért lásd: [Data Factory bemutatása tooAzure](../data-factory/data-factory-introduction.md).


## <a name="migration-considerations"></a>Az áttelepítés szempontjai

Először azonosítsa hello áttelepítési stratégia legjobban az alkalmazáshoz, amely írja, beolvassa és feldolgozza a Data Lake Store-adatokat. Ha úgy dönt, hogy a stratégiát, fontolja meg az alkalmazás rendelkezésre állási követelményeinek, és hello állásidő áttelepítés során előforduló. A legegyszerűbb módszere előfordulhat például, toouse hello "növekedési-és-shift" felhő áttelepítési modell. Ez a megközelítés a szüneteltetése hello alkalmazás a meglévő régióban közben minden az adatok másolni toohello új terület. Hello másolási folyamat végén, akkor folytathatja az alkalmazás hello új régióban, és törölje a hello régi Data Lake Store-fiók. Hello áttelepítéskor állásidőre szükség.

tooreduce állásidő, előfordulhat, hogy azonnal elindíthatja hello új régióban új adatok bevitele. Ha hello minimálisan szükséges adatokat, futtassa az alkalmazást hello új régióban. Hello háttérben továbbra is régebbi adatok toocopy hello meglévő Data Lake Store fiók toohello új Data Lake Store-fiók hello új régióban. Ezt a módszert használja, hogy hello kapcsoló toohello új régió minimális állásidővel. Ha minden hello régebbi adatok másolta, hello régi Data Lake Store-fiók törlése

Más fontos részleteket tooconsider az áttelepítés tervezése során a következők:

* **Adatmennyiség**. (gigabájtban, fájlok és mappák, és így tovább hello száma) adatok mennyiségét hello hello időt és erőforrásokat hello áttelepítési szüksége van hatással.

* **Data Lake Store-fiók neve**. Új fiók nevét hello hello új régióban globálisan egyedinek kell lennie. Előfordulhat például, hogy hello nevét a régi Data Lake Store-fiókot az USA keleti régiója 2 contosoeastus2.azuredatalakestore.net. Új Data Lake Store-fiókja az Észak-Európa contosonortheu.azuredatalakestore.net neve lehet.

* **Eszközök**. Azt javasoljuk, hogy használja-e hello [Azure Data Factory másolási tevékenység](../data-factory/data-factory-azure-datalake-connector.md) toocopy Data Lake Store-fájlokat. Adat-előállító adatátvitel nagy teljesítményt és a megbízhatóság támogatja. Ne feledje, hogy a Data Factory csak hello mappahierarchia és hello fájlok tartalmának másolja. Toomanually kell alkalmazni a hozzáférés-vezérlési listák (ACL), amelyek hello régi toohello új fiókok használatával. További információ hányad forgatókönyvek esetén a teljesítmény célokat is beleértve: hello [másolási tevékenység teljesítmény- és hangolási útmutató](../data-factory/data-factory-copy-activity-performance.md). Ha azt szeretné, hogy gyorsabban másolt adatokra, szükség lehet a toouse további Felhőbeli adatok adatátviteli egység. Olyan eszközöket, például a AdlCopy, nem támogatják az adatok másolását a régiók között.  

* **Sávszélesség-költségek**. [Sávszélesség-költségek](https://azure.microsoft.com/en-us/pricing/details/bandwidth/) alkalmazni, mert egy Azure-régiót kivitt adatok.

* **Az adatok ACL-ek**. Az adatok hello új régióban biztonságos hozzáférés-vezérlési listák toofiles és mappák alkalmazásával. További információkért lásd: [biztonságossá tétele az Azure Data Lake Store-ban tárolt adatok](data-lake-store-secure-data.md). Javasoljuk, hogy hello áttelepítési tooupdate használni, és állítsa be a hozzáférés-vezérlési listák. Érdemes lehet toouse hasonló tooyour aktuális beállításait. Megtekintheti a hello hozzáférés-vezérlési listákat, amelyek alkalmazott tooany fájl hello Azure-portál használatával [PowerShell-parancsmagok](/powershell/module/azurerm.datalakestore/get-azurermdatalakestoreitempermission), vagy az SDK-k.  

* **Az elemzés services helyének**. A legjobb teljesítmény érdekében az analytics szolgáltatások, például az Azure Data Lake Analytics vagy az Azure HDInsight hello kell lennie az adatok és ugyanabban a régióban.  

## <a name="next-steps"></a>Következő lépések
* [Az Azure Data Lake Store áttekintése](data-lake-store-overview.md)
