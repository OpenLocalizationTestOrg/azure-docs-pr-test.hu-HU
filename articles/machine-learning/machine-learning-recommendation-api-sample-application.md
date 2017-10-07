---
title: "a Machine Learning javaslatok API hello aaaCommon műveletek |} Microsoft Docs"
description: "Az Azure ML javaslat mintaalkalmazás"
services: machine-learning
documentationcenter: 
author: LuisCabrer
manager: jhubbard
editor: cgronlun
ms.assetid: 0220bc17-3315-47d7-84a3-ef490263a343
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: luisca
ROBOTS: NOINDEX
redirect_url: machine-learning-datamarket-deprecation
redirect_document_id: True
ms.openlocfilehash: da16767134a1386617e1184e4a4850f1f346e972
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="recommendations-api-sample-application-walkthrough"></a>A Recommendations API mintaalkalmazásának részletes ismertetése
> [!NOTE]
> El kell kezdenie hello javaslatok API kognitív szolgáltatás helyett jelen verziójában. hello javaslatok kognitív szolgáltatás adatokéval ezt a szolgáltatást, és minden hello új szolgáltatások nincs fejlesztik ki. Rendelkezik új szolgáltatásokat, például a kötegelés támogatása, a megfelelőbb API Explorer, a tisztító API felület, egységesebb előfizetési/számlázási élményt, stb.
> További információ [áttelepítése toohello új kognitív szolgáltatás](http://aka.ms/recomigrate)
> 
> 

## <a name="purpose"></a>Cél
Ez a dokumentum bemutatja hello hello használata az Azure Machine Learning javaslatok API keresztül egy [mintaalkalmazás](https://code.msdn.microsoft.com/Recommendations-144df403).

Ez az alkalmazás nem tervezett tooinclude összes funkcióját, és nem használja az összes hello API-k. Néhány gyakori műveletek tooperform azt mutatja be, ha először a Machine Learning-javaslási szolgáltatása hello tooplay. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="introduction-toomachine-learning-recommendation-service"></a>Bevezetés tooMachine tanulási javaslási szolgáltatása
Javaslatok keresztül hello Machine Learning-javaslási szolgáltatása engedélyezve vannak a következő adatok hello javaslat modellben építésekor:

* Azt szeretné, hogy toorecommend, más néven a katalógus hello elemek tára
* Adatok képviselő elemek száma a munkamenetek (Ez szerezhető keresztül az adatgyűjtést, nem hello mintaalkalmazás részeként időbeli) hello használata

Után egy javaslat modelljei épülnek, használhatja a felhasználó, előfordulhat, hogy érdekelheti toopredict elemekre tooa megfelelő elemet (vagy csak egy elemet) hello felhasználói választja ki.

tooenable hello előző helyzethez, a következő hello hello Machine Learning-javaslási szolgáltatása:

* A modell létrehozása: Ez az hello adatokat (a katalógus és a használati) és hello előrejelzési modellek logikai tárolója. Minden egyes modell tároló egy egyedi Azonosítót, amelynek van lefoglalva létrehozásakor keresztül azonosítja. Ezt az Azonosítót hello Modellazonosító nevezik, és a legtöbb hello API-k használják. 
* Töltse fel a toocatalog: egy modell tároló jön létre, amikor a katalógus tooit is társíthat.

**Megjegyzés:**: a modell létrehozása és feltöltése tooa katalógus általában az során végrehajtott egyszer hello modell életciklusát.

* Töltse fel a használat: Ez biztosítja, hogy a használati adatok toohello modell tároló.
* A javaslat modell létrehozása: Miután elég adat, hello javaslat modellt hozhat létre. Ez a művelet hello felső gépi tanulási algoritmusok toocreate egy javaslat modellt használja. Minden build tartozik egy egyedi azonosítót. Akkor tookeep feljegyzés az ezt az Azonosítót kell végrehajtani, mert a szükséges, hogy egyes API-k hello funkcióit.
* A figyelő hello létrehozási folyamatának: javaslat modell build egy aszinkron művelet, és igénybe perc tooseveral függően több órába, hello adatmennyiség (katalógus és használati) és hello build paraméterek. Ezért toomonitor hello build van szüksége. A javaslat modell jön létre, csak akkor, ha a kapcsolódó összeállítási sikeresen befejeződik.
* (Választható) Válasszon egy aktív javaslat modell build: Ez a lépés csak akkor szükség, ha a modell tároló beépített egynél több javaslat modell. Nem jelzi hello aktív javaslat modell tooget kérelem javaslatokkal hello toohello alapértelmezett aktív létre automatikusan átirányítani. 

**Megjegyzés:**: egy aktív javaslat modell éles készen áll, és be van építve az üzemi alkalmazások és szolgáltatások. Ez eltér egy nem aktív javaslat modell, amely a test-szerű környezetet (más néven átmeneti) marad.

* Javaslatok beszerzése: Miután egy javaslat modell, csak egy elemet vagy a kiválasztott elemek listájának ajánlásokat is elindíthatja. 

Első javaslat általában egy bizonyos ideig által aktivált. Ezen időszak alatt az idő a használati adatok toohello Machine Learning javaslat rendszer, amely hozzáadja a megadott modell tároló adatok toohello irányíthatja. Ha elegendő használati adatokat, hozhat létre új ajánlást modell, amely magában foglalja a hello további használati adatok. 

## <a name="prerequisites"></a>Előfeltételek
* A Visual Studio 2013 vagy újabb verzió
* Internetelérés 
* Előfizetés toohello javaslatok API (https://datamarket.azure.com/dataset/amla/recommendations).

## <a name="azure-machine-learning-sample-app-solution"></a>Az Azure Machine Learning app megoldást
Ez a megoldás tartalmaz hello forráskódját, példa, katalógusfájlt és irányelvek toodownload hello csomagok, amelyek szükségesek a fordításhoz.

## <a name="hello-apis-used"></a>hello használt API-k
hello alkalmazás Machine Learning javaslat funkcióit keresztül elérhető API-t használja. az alkalmazás hello egy következő API-k hello:

* Modell létrehozása: hozzon létre egy logikai tárolója toohold adatok és az ajánlott modellek. A modell azonosíthatók nevét, és meg nem hozható létre egynél több modell hello ugyanazzal a névvel.
* Katalógus-fájl feltöltése: tooupload eseménykatalógus-adatok használata.
* Használati-fájl feltöltése: tooupload használati adatok felhasználásával.
* Indítás, build: toocreate egy javaslat modellt használja.
* Build végrehajtási figyelni: javaslat modell build toomonitor hello állapotának használja.
* Válassza ki a beépített modelljét javaslat: az tooindicate mely javaslat modell toouse alapértelmezett bizonyos modell tárolója. Ez a lépés szükség, ha egynél több javaslat modell és azt szeretné, hogy egy nem aktív build hello aktív javaslat modell tooactivate.
* A javaslat beolvasása: tooretrieve ajánlott elemek megadott egyetlen elemet vagy elemeket készlete tooa szerint használja. 

Hello API-k teljes leírását adja hello Microsoft Azure piactérről dokumentációjában talál. 

**Megjegyzés:**: A modell rendelkezhet több buildek időbeli (nem egyszerre). Minden build jön létre a hello ugyanabban vagy egy frissített katalógus és további használati adatok.

## <a name="common-pitfalls"></a>Közös nehézségek
* A felhasználónév és a Microsoft Azure piactérről fiók elsődleges kulcs toorun hello mintaalkalmazás tooprovide kell.
* Hello futó mintaalkalmazást egymás után sikertelen lesz. hello alkalmazási folyamatot tartalmazzák a létrehozása, feltöltése, hello figyelő létrehozása és javaslatok lekérése egy előre meghatározott modell; Emiatt meghiúsul egymást követő végrehajtásakor módosításakor nem hello modellnév között.
* Javaslatok adatok nélkül előfordulhat, hogy vissza. hello mintaalkalmazás nagyon kis katalógus és használati fájlt használ. Bizonyos elemek hello katalógusból, ezért ajánlott elemek fog rendelkezni.

## <a name="disclaimer"></a>Jogi nyilatkozat
hello mintaalkalmazás nem éles környezetben futtatni kívánt toobe. hello katalógus hello adatok nagyon kicsi, és nem biztosít egy jelentéssel bíró javaslat modell. a bemutató, megadva hello adatai. 

