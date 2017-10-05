---
title: "A Machine Learning javaslatok API általános műveleteket |} Microsoft Docs"
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
redirect_document_id: TRUE
ms.openlocfilehash: 8d8efa93e820f4a745ed93c0f4d13b2438dfa1eb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="recommendations-api-sample-application-walkthrough"></a>A Recommendations API mintaalkalmazásának részletes ismertetése
> [!NOTE]
> El kell kezdenie a javaslatok API kognitív szolgáltatás helyett jelen verziójában. A javaslatok kognitív szolgáltatás adatokéval ezt a szolgáltatást, és az új szolgáltatások nincs fejlesztik ki. Rendelkezik új szolgáltatásokat, például a kötegelés támogatása, a megfelelőbb API Explorer, a tisztító API felület, egységesebb előfizetési/számlázási élményt, stb.
> További információ [áttelepítése az új kognitív szolgáltatáshoz](http://aka.ms/recomigrate)
> 
> 

## <a name="purpose"></a>Cél
Ez a dokumentum bemutatja az Azure Machine Learning javaslatok API keresztül használatát egy [mintaalkalmazás](https://code.msdn.microsoft.com/Recommendations-144df403).

Ez az alkalmazás nem célja, hogy tartalmazzák az összes funkcióját, és nem használja az összes API-k. Bemutatja, hogy az egyes közös műveleteket végrehajtani, ha először lejátszani kívánt a Machine Learning javaslat szolgáltatással. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="introduction-to-machine-learning-recommendation-service"></a>Bevezetés a Machine Learning-javaslási szolgáltatása
A Machine Learning javaslat szolgáltatásával javaslatok engedélyezve vannak, az alábbi adat ajánlás modellben építésekor:

* Javasoljuk, más néven a katalógus kívánt elemek tára
* Az elemek száma a munkamenetek (Ez szerezhető keresztül az adatgyűjtést, nem a mintaalkalmazás részeként időbeli) használatát képviselő adatokat

Miután egy javaslat modelljei épülnek, használhatja előre jelezni lehet, hogy a felhasználó elemekre érdekli, meghatározott elemek (vagy csak egy elemet) alapján a felhasználó kiválasztja.

Ahhoz, hogy az előző helyzethez, tegye a következőket a Machine Learning-javaslási szolgáltatása az:

* A modell létrehozása: Ez az az adatokat (katalógus és használati) és az előrejelzési modellek logikai tárolója. Minden egyes modell tároló egy egyedi Azonosítót, amelynek van lefoglalva létrehozásakor keresztül azonosítja. Ezt az Azonosítót a Modellazonosító nevezik, és a legtöbb az API-k használják. 
* Feltöltése a katalógust: amikor egy modell tároló jön létre, társíthatja azt egy katalógust.

**Megjegyzés:**: a modell létrehozása és feltöltése a katalógus általában az során végrehajtott egyszer a modell életciklusát.

* Töltse fel a használat: használati adatok hozzáadása a modell tároló.
* A javaslat modell létrehozása: Miután elég adatokat, az ajánlás modellt hozhat létre. Ez a művelet a legfelső szintű gépi tanulási algoritmusok javaslat modell létrehozásához használ. Minden build tartozik egy egyedi azonosítót. Nyilvántarthatja ezt az Azonosítót, mert a szükséges, hogy egyes API-k funkcióját kell.
* Az épület folyamat figyelésére: javaslat modell build egy aszinkron művelet, és is igénybe vehet néhány percet adatokat (a katalógus és a használati) és a build paraméterek mennyiségétől függően több órát. Ezért kell figyelni a build. A javaslat modell jön létre, csak akkor, ha a kapcsolódó összeállítási sikeresen befejeződik.
* (Választható) Válasszon egy aktív javaslat modell build: Ez a lépés csak akkor szükség, ha a modell tároló beépített egynél több javaslat modell. A javaslatok beolvasása, amely jelzi, az aktív javaslat modell nélkül minden kérést a rendszer a rendszer az alapértelmezett active buildre automatikusan átirányítja. 

**Megjegyzés:**: egy aktív javaslat modell éles készen áll, és be van építve az üzemi alkalmazások és szolgáltatások. Ez eltér egy nem aktív javaslat modell, amely a test-szerű környezetet (más néven átmeneti) marad.

* Javaslatok beszerzése: Miután egy javaslat modell, csak egy elemet vagy a kiválasztott elemek listájának ajánlásokat is elindíthatja. 

Első javaslat általában egy bizonyos ideig által aktivált. Ezen időszak alatt az idő a Machine Learning javaslat rendszerben, ami ezeket az adatokat ad hozzá a megadott modell tároló használati adatok irányíthatja. Ha elegendő használati adatokat, hozhat létre új ajánlást modell, amely magában foglalja a további használati adatok. 

## <a name="prerequisites"></a>Előfeltételek
* A Visual Studio 2013 vagy újabb verzió
* Internetelérés 
* A javaslatok (https://datamarket.azure.com/dataset/amla/recommendations) API-előfizetéssel.

## <a name="azure-machine-learning-sample-app-solution"></a>Az Azure Machine Learning app megoldást
Ez a megoldás tartalmaz a forráskódot, példa, katalógusfájlt és letöltése a csomagok, amelyek szükségesek a fordítási direktíváit.

## <a name="the-apis-used"></a>A használt API-k
Az alkalmazás a Machine Learning javaslat funkció keresztül elérhető API-t használja. A következő API-k egy az alkalmazásban:

* Modell létrehozása: hozzon létre egy logikai tároló adatok és az ajánlott modellek tárolására. A modell azonosít egy nevet, és nem hozható létre egynél több modell ugyanazzal a névvel.
* Katalógus-fájl feltöltése: töltse fel az eseménykatalógus-adatok használatával.
* Használati-fájl feltöltése: töltse fel a használati adatok használatával.
* Build indítható el: a javaslat modellek létrehozásához használja.
* Build végrehajtási figyelni: javaslat modell build állapotának figyelheti.
* Válassza ki a beépített modelljét javaslat: azt jelzi egy bizonyos modell tároló alapértelmezés szerint használandó javaslat modellt. Ez a lépés szükség, csak akkor, ha egynél több javaslat modell rendelkezik, és az aktív javaslat modell nem aktív build aktiválásához.
* A javaslat beolvasása: egy adott vagy elemek készlete alapján ajánlott elemek lekéréséhez használja. 

Az API-k teljes leírását adja a Microsoft Azure piactérről dokumentációjában talál. 

**Megjegyzés:**: A modell rendelkezhet több buildek időbeli (nem egyszerre). Minden build ugyanaz vagy egy frissített katalógus és további használati adatok hozza létre.

## <a name="common-pitfalls"></a>Közös nehézségek
* Meg kell adnia a felhasználónevet és a Microsoft Azure piactérről elsődleges fiókkulcs mintaalkalmazás futtatásához.
* A mintaalkalmazás futtatása egymás után sikertelen lesz. Az alkalmazás folyamata magában foglalja a létrehozása, feltöltése, a figyelő létrehozása és javaslatok lekérése egy előre meghatározott modell; Ezért, ha sikertelen lesz egymást követő végrehajtásakor ne változtassa meg a modell neve között.
* Javaslatok adatok nélkül előfordulhat, hogy vissza. A mintaalkalmazás nagyon kis katalógus és használati fájlt használ. Néhány elemet a katalógusból, ezért ajánlott elemek fog rendelkezni.

## <a name="disclaimer"></a>Jogi nyilatkozat
A mintaalkalmazás nem kívánják éles környezetben futtatható. A katalógusban szereplő adatok nagyon kicsi, és nem biztosít egy jelentéssel bíró javaslat modell. Az adatok szerint bemutatója valósul meg. 

