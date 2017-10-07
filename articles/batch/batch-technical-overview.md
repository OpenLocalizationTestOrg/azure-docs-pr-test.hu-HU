---
title: "aaaAzure kötegelt hello felhő fut, a nagyméretű párhuzamos számítógépes megoldások |} Microsoft Docs"
description: "A nagyméretű párhuzamos és a HPC-munkaterhelések hello Azure Batch szolgáltatás használata"
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: 93e37d44-7585-495e-8491-312ed584ab79
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/05/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: acc52e46330c465f81951441d9067371098cf63a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-intrinsically-parallel-workloads-with-batch"></a>Belsőleg párhuzamos számítási feladatok futtatása a Batch használatával

Az Azure Batch egy olyan platform szolgáltatás a felügyeleti teendők központjaként párhuzamos és nagy teljesítményű számítástechnikai (HPC) alkalmazások hatékonyan hello felhőben futó. Az Azure Batch számítási igényű munkahelyi toorun virtuális gépek felügyelt szervezendő ütemezi, és képes automatikusan méretezési számítási erőforrások toomeet hello igényeinek a feladatok.

Az Azure Batch egyszerűen definiálhat Azure számítási erőforrások tooexecute az alkalmazások párhuzamosan, és léptékű. Nincs szükség toomanually van létrehozása, konfigurálása és kezelése a HPC-fürt, az egyes virtuális gépek, virtuális hálózatok vagy egy összetett feladat és feladat ütemezési infrastruktúra. Az Azure Batch automatizálja vagy egyszerűbbé teszi ezeket a feladatokat az Ön számára.

## <a name="use-cases-for-batch"></a>A Batch használati esetei
A Batch felügyelt Azure szolgáltatás, amely *kötegelt feldolgozásra* vagy *kötegelt számításra* szolgál – vagyis a kívánt eredmények elérése érdekében a hasonló feladatok nagy mennyiségének a futtatására. A kötegelt feldolgozás olyan szervezetek használják a leggyakrabban, amelyek rendszeresen dolgoznak fel, alakítanak át és elemeznek nagy mennyiségű adatot.

A Batch nagyszerűen működik a belsőleg párhuzamos (más néven „zavaróan párhuzamos”) alkalmazásokkal és számítási feladatokkal. A belsőleg párhuzamos számítási feladatok könnyen oszthatók több olyan feladatra, amelyek egy időben sok számítógépen hajtanak végre munkát.

![Párhuzamos feladatok][1]<br/>

Néhány példa az ezzel a technikával gyakran feldolgozott számítási feladatokra:

* pénzügyi kockázat modellezése,
* időjárási és hidrológiai adatok elemzése,
* képek renderelése, elemzése és feldolgozása,
* adathordozók kódolása és átkódolása,
* génszekvenciák elemzése,
* műszaki feszültségelemzés,
* szoftvertesztelés.

Kötegelt is hello végén csökkentse lépéssel párhuzamos számításokat, és hajtható végre, mint összetett HPC-munkaterhelések [Message Passing Interface (MPI)](batch-mpi.md) alkalmazások.

Az Azure által biztosított Batch szolgáltatás és az egyéb HPC-megoldások összehasonlítását lásd: [Batch és HPC-megoldások](batch-hpc-solutions.md).

[!INCLUDE [batch-pricing-include](../../includes/batch-pricing-include.md)]

## <a name="scenario-scale-out-a-parallel-workload"></a>Forgatókönyv: Párhuzamos számítási feladat horizontális felskálázása
A közös hello Batch szolgáltatás hello kötegelt API-k toointeract használó szükség belsőleg párhuzamos munka – például a 3D színfalak--a számítási csomópontok készletét képek hello megjelenítésének kiterjesztése. A számítási csomópontok készletét is lehet a "Megjelenítés"farm biztosít több tíz, száz vagy akár több ezer magok tooyour megjelenítési feladat, például.

hello következő diagram megjelenítése egy közös kötegelt munkafolyamat egy ügyfélalkalmazást és üzemeltetett szolgáltatás kötegelt toorun egy párhuzamos munkaterhelés használatával.

![Batch-megoldás munkafolyamata][2]

Ez egy gyakori forgatókönyv az alkalmazás vagy szolgáltatás feldolgozza a számítási munkaterhelés Azure kötegben hello lépések végrehajtásával:

1. Töltse fel a hello **bemeneti fájlok** és hello **alkalmazás** fel fogja dolgozni ezen fájlok tooyour Azure Storage-fiók. a bemeneti fájlok hello lehet, amely az alkalmazás feldolgozza, például pénzügyi modellezési adatok vagy videofájlok toobe engedélyezi az átalakítását adatokat. hello alkalmazásfájlok lehet bármely alkalmazás, amely hello adatok, például a 3D megjelenítés alkalmazás vagy media transcoder feldolgozására szolgál.
2. Hozzon létre egy kötegelt **készlet** a számítási csomópontok a Batch-fiók – ezeket a csomópontokat, amely végrehajtja a feladatok hello virtuális gépek. Például a hello tulajdonságait kell előbb megadni [csomópont méretének](../cloud-services/cloud-services-sizes-specs.md), az operációs rendszer, és az Azure Storage helyének hello hello alkalmazás tooinstall Ha hello csomópontok veszi hello címkészlet (a #1. lépésben feltöltött hello alkalmazás). Beállíthatja úgy is hello készlet túl[automatikus méretezése](batch-automatic-scaling.md) a feladatok létrehozása, a válasz toohello terheléseknél engedélyezett. Hello száma hello készlet számítási csomópontjainak automatikus skálázást dinamikusan módosítja.
3. Hozzon létre egy kötegelt **feladat** hello készlet számítási csomópontok terhelése toorun hello. Feladat létrehozásakor társítsa azt a Batch-készlettel.
4. Adja hozzá **feladatok** toohello feladat. Feladatok tooa feladat hozzáadásakor hello Batch szolgáltatás automatikusan ütemezi hello feladatok végrehajtásra hello készletben hello számítási csomóponton. Minden feladat, hogy tooprocess hello bemeneti fájlok feltöltött hello alkalmazás használja.
   
   * 4a. A feladat végrehajtása során, mielőtt hello adatok (hello bemeneti fájlok), hogy ez tooprocess toohello számítási csomópont hozzá van rendelve tölthet le. Ha hello alkalmazás nem már telepítve van a hello csomópont (lásd: #2. lépés), letölthető itt helyette. Ha hello letöltések, hello feladatok hozzárendelt csomópontjaik hajtható végre.
5. Hello feladatok futnak, mint lekérdezheti a kötegelt toomonitor hello előrehaladását hello feladat és a feladatokat. Az ügyfél alkalmazás vagy szolgáltatás kommunikál a Batch szolgáltatás hello HTTPS-KAPCSOLATON keresztül. Mivel előfordulhat, hogy a számítási csomópontok több ezer futó feladatok ezer figyelését, mindenképp túl[hello Batch szolgáltatás hatékony lekérdezéséhez](batch-efficient-list-queries.md).
6. Hello feladat befejeződik, mert az eredmény adatok tooAzure tárolási is feltölthetnek. Közvetlenül a fájlrendszer hello adott számítási csomóponton is beolvasni a fájlok.
7. Ha a figyelést észleli, hogy hello feladatok a feladat befejeződött, az ügyfél alkalmazás vagy szolgáltatás letöltheti további feldolgozás és értékelési hello kimeneti adatait.

Tartsa szem előtt az egyik módja toouse kötegelt, és ez a forgatókönyv leírja, csak az elérhető szolgáltatások néhány. Például, hogy végrehajtható [párhuzamosan több feladat](batch-parallel-node-tasks.md) minden számítási csomópont, és használhatja [feladatelőkészítési és -végrehajtási feladatok feladat](batch-job-prep-release.md) tooprepare hello csomópontok a feladatok, majd ezt követően tisztítása.

## <a name="next-steps"></a>Következő lépések
Most, hogy a Batch szolgáltatás hello magas szintű áttekintését,-e idő toodig mélyebb toolearn használatát az tooprocess a párhuzamos számítási-igényes munkaterhelések.

* Olvasási hello [Batch funkcióinak áttekintése a fejlesztők](batch-api-basics.md), bárki toouse kötegelt előkészítése alapvető információkat. hello cikkben Batch szolgáltatás erőforrásokhoz, mint a készletek, a csomópontok, a feladatok, és a feladatok és a hello részletesebb információkat is használhatja a kötegelt kérelem felépítésekor sok API-funkciókat.
* További tudnivalók: hello [kötegelt API-k és eszközök](batch-apis-tools.md) kötegelt megoldások érhetők el.
* [Hello Azure Batch könyvtár az első lépései a .NET-keretrendszerhez készült](batch-dotnet-get-started.md) toolearn hogyan toouse C# és hello Batch .NET könyvtár tooexecute egy egyszerű munkaterhelés közös kötegelt munkafolyamattal. Ez a cikk a hogyan toouse hello Batch szolgáltatás tanulása közben első nem egyike lehet. Szerepel továbbá egy [Python verzió](batch-python-tutorial.md) hello oktatóanyag.
* Töltse le a hello [minták a Githubon code] [ github_samples] toosee, hogyan lehet a C# mind a Python kommunikáljanak a kötegelt tooschedule és a folyamat minta munkaterhelések.
* Tekintse meg a hello [kötegelt képzési terv] [ learning_path] tooget felmérheti, hello erőforrások tooyou érhető el, ha Ön további kötegelt toowork.


[github_samples]: https://github.com/Azure/azure-batch-samples
[learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/

[1]: ./media/batch-technical-overview/tech_overview_01.png
[2]: ./media/batch-technical-overview/tech_overview_02.png
