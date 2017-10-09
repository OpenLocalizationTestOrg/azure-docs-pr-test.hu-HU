---
title: "aaaSet be és használja a Machine Learning javaslatok API hello |} Microsoft Docs"
description: "Az Azure Machine Learning GYIK a beépített Microsoft javaslatok API"
services: machine-learning
documentationcenter: 
author: LuisCabrer
manager: jhubbard
editor: cgronlun
ms.assetid: 1ffc3c16-e040-4225-9d72-105129938dfa
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
ms.openlocfilehash: 980bf1a36f3291275d9ef0fee9b4446f7e0cbecf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="setting-up-and-using-machine-learning-recommendations-api-faq"></a>A Machine Learning Recommendations API beállítása és használata – GYIK
**Mi az az ajánlások?**

> [!NOTE]
> El kell kezdenie hello javaslatok API kognitív szolgáltatás helyett jelen verziójában. hello javaslatok kognitív szolgáltatás adatokéval ezt a szolgáltatást, és minden hello új szolgáltatások nincs fejlesztik ki. Rendelkezik új szolgáltatásokat, például a kötegelés támogatása, a megfelelőbb API Explorer, a tisztító API felület, egységesebb előfizetési/számlázási élményt, stb.
> További információ [áttelepítése toohello új kognitív szolgáltatás](http://aka.ms/recomigrate)
> 
> 

A szervezetek és vállalatok számára a javaslatok toocross-értékesít és felfelé-értékesít termékek és szolgáltatások tootheir ügyfelek használó javaslatok az Azure Machine Learning egy önkiszolgáló javaslatok motor biztosít. A core algoritmus mátrix factorization használó együttműködési szűrés megvalósítása. JAVASLATOK férhetnek hozzá alkalmazásfejlesztők REST API-k használatával. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**Mi a teendő a javaslatok?**

JAVASLATOK vesz igénybe, adjon meg egy elem vagy egy elemet, és a vonatkozó javaslatok listáját adja vissza. Például: az ügyfél egy online közvetítő kattint egy termék. hello online közvetítő elküldi a terméket, bemeneti tooRECOMMENDATIONS, ismét lekérdezi a termékek listáját, és úgy dönt, amelyhez ezeket a termékeket toohello ügyfél megjelenik. Előfordulhat, hogy toouse javaslatok toooptimize szeretné, hogy az online áruházból vagy tooinform még a belső értékesítési részleg vagy hívás center.

**Vannak-e használati korlátozások?**

Javaslatok a következő használati korlátozások hello rendelkezik:

* Előfizetésenként modellek maximális száma: 10
* A katalógus rendelkező elemek maximális számát: 100 000
* mindig használati pontok maximális számát hello ~ 5,000,000. Ha újakat feltöltött vagy fogja jelenteni a legrégebbi hello törlődni fog.
* E-mailben (például katalógus adatokat importálhat, használati adatok importálása) elküldött adatok maximális mérete 200 MB
* Tranzakció nincs aktív javaslatok modell build (TP-k) másodpercenkénti száma nem ~ 2 TP-k. Aktív javaslatok modell build mentése too20 TP-k tárolására képes.

## <a name="purchase-and-billing"></a>A vásárlás és számlázási
**Javaslatok mennyi nem költség hello indítási időszakban?**

Javaslatok egy olyan előfizetés-alapú szolgáltatás. Díjszabási havonta tranzakciók mennyisége alapján történik. Ellenőrizheti a hello [lap kínálnak](https://datamarket.azure.com/dataset/amla/recommendations) a Microsoft Azure piactéren díjszabási információkat.

**-E javaslatok rendelkező társított költségekre nyomon követése és tárolja a felhasználói tevékenység a számomra?**

Nem pillanatban hello.

**Javaslatok rendelkezik egy ingyenes próbaverzióra?**

Nincs szabad nyomokat, amely korlátozott too10, havonta 000 tranzakciók.

**Ha I alapján számlázzuk ajánlások?**

A szolgáltatás fizetős egyetlen előfizetéshez nincs havi díjért. Ha megvásárolja a fizetős verziót választja, akkor azonnal felszámított hello először havi használja. Hello összeg hello ajánlat hello előfizetés lapján (és az adót) a társított van szó. Ez a havi díj havonta készült hello azonos naptári dátum, ahogy az eredeti vásárlás, amíg hello előfizetés megszüntetése. 

**Hogyan lehet frissíteni a tooa magasabb réteg szolgáltatás?**

Vásároljon, vagy frissítse az előfizetést hello [lap kínálnak](https://datamarket.azure.com/dataset/amla/recommendations) oldalon, a Microsoft Azure piactérről.

Amikor frissít egy előfizetést:

* A régi előfizetés hátralévő tranzakciók nem kerülnek tooyour új előfizetés. 
* Kell fizetnie teljes ár hello új előfizetés, annak ellenére, hogy a régi előfizetésben nem használt tranzakciók.

Folyamat tooupgrade előfizetést:

* Nevigate toohello [lap kínálnak](https://datamarket.azure.com/dataset/amla/recommendations).
* Jelentkezzen be a piactér toohello, ha még nem jelentkezett be.
* A jobb oldali hello összes hello elérhető csomagok vannak felsorolva. Kattintson a tooupgrade kívánt hello terv hello választógombra.
* Ha azt szeretné, hogy tooupgrade, kattintson a **OK**. Ha nem szeretné, hogy tooupgrade, kattintson a **Mégse**.

**Fontos** gondosan olvasási hello párbeszédpanel, mert számlázási és -felhasználási implications van frissítése előtt.

**Amikor véget ér az előfizetés tooRecommendations?**

Az előfizetés véget ér, amikor vethető el. Ha azt szeretné, hogy toocancel előfizetése, tekintse meg az utasításoknak hello.

**Hogyan javaslatok előfizetés megszüntetése?**

az előfizetés, a következő használatát hello lépések toocancel. Ha a jelenlegi előfizetés a fizetős verziót választja, az előfizetés továbbra is érvényben aktuális számlázási időszak hello hello végéig. Ha hatékony cancellation toobe azonnal kell hello, írjon nekünk az [Microsoft Support](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).

**Megjegyzés:** visszatérítés nem kap, ha megszakítja a számlázási időszak vagy a nem használt tranzakciók hello vége előtt számlázási időszakban.

* Keresse meg a toohello [lap kínálnak](https://datamarket.azure.com/dataset/amla/recommendations).
* Jelentkezzen be a piactér toohello, ha még nem jelentkezett be.
* Kattintson a **Mégse** toohello sarkában hello adatkészlet nevét és állapotát. Ehhez az előfizetéshez is használhatja, amíg el nem hello végét hello aktuális számlázási időszak vagy tranzakció vonatkozó felső korlát (amelyik következik be előbb).

Ha azt szeretné, hogy az előfizetés azonnal, vásárolhat egy új előfizetés fájlt a jegy toocancel [Microsoft Support](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).

## <a name="getting-started-with-recommendations"></a>Ismerkedés a javaslatok
**Az ajánlások a számomra?** 

A Machine Learning javaslatok a szervezetek és vállalatok számára a javaslatok toocross-értékesít és a fel-értékesít termékek vagy szolgáltatások tootheir ügyfél használó van. Ha egy ügyfél-webhely, az értékesítési szakemberek, a belső értékesítés, vagy egy ügyfélszolgálatával, és, ha csak néhány dozen termékek vagy szolgáltatások katalógus, a lényeg előfordulhat, hogy előnyt az ajánlások. 

Javaslatok ki tervezett toobe viszonylag egyszerű. hello jelenlegi API-alapú verziójával alapszintű programozási szakértelmet igényel. Ha segítségre van szüksége, forduljon a webhely fejlesztett hello szállító. Ha egy belső IT-részlegtől vagy egy belső fejlesztő, képes tooget javaslatok toowork meg kell. 

**Mik azok a javaslatok beállítása hello előfeltételei**

Javaslatok megköveteli, hogy Ön naplózása a felhasználó lehetőségek tooyour katalógus vonatkozik. Ha nincs ilyen naplót, és egy ügyfél elérhető webhelyek rendelkezik, a javaslatok gyűjthet felhasználói tevékenység meg. 

Javaslatok is szükséges a termékek vagy szolgáltatások egy katalógust. Ha hello katalógus nem rendelkezik, javaslatok használhatja hello tényleges felhasználói használati adatok és a katalógus átalakítást. Egy implicit katalógus nem fog tartalmazni, amelyek nem jelentették a felhasználói tranzakció részeként elemeket.

**Hogyan állíthatom be javaslatok hello az első alkalommal?**

Után [előfizető](https://datamarket.azure.com/dataset/amla/recommendations) tooRecommendations, használja a hello hello API dokumentációjában [Azure Machine Learning ajánlásokkal – első lépések útmutatójának](machine-learning-recommendation-api-quick-start-guide.md) tooset hello szolgáltatás.

**Hol található API-JÁNAK dokumentációja?** 

API-JÁNAK dokumentációja hello van [Azure Machine Learning javaslatok – első lépések útmutatójának](machine-learning-recommendation-api-quick-start-guide.md).

**Beállítások mire tooupload katalógus és használati adatok tooRecommendations van?**

A katalógus és használati adatok feltöltése a két lehetőség közül választhat: hello adatok exportálása a CRM-rendszerből vagy más naplókat, és töltse fel az tooRecommendations, vagy hozzáadhat címkék tooyour webhelyet, ahol a felhasználói tevékenységek fogja követni. Ha ez utóbbi hello a módszert használja, hello adattárolás az Azure-ban.

## <a name="maintenance-and-support"></a>Karbantartási és támogatás
**Hogyan nagy a adatkészlet lehet?**

Egyes adatokat is tartalmazhatnak too100, 000 a szolgáltatáskatalógusban található elemek és használati adatok too2048 MB.
Emellett egy előfizetés mentése too10 adatkészletek (modellek) tartalmazhat.

**Hol találhatok ajánlások a technikai támogatási szolgálathoz?**

Technikai támogatás érhető el hello [Microsoft Azure támogatási](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) hely.

**Hol találnak a használati feltételek hello?**

[Microsoft Azure Machine Learning szolgáltatás javaslatok API feltételeit](https://datamarket.azure.com/dataset/amla/recommendations#terms).

