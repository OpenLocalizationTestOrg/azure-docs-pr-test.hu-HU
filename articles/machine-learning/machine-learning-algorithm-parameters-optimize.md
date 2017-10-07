---
title: aaaOptimize a algoritmusok az Azure Machine Learning |} Microsoft Docs
description: "Ismerteti, hogyan a toochoose hello optimális paraméter beállítása az Azure Machine Learning algoritmus."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 6717e30e-b8d8-4cc1-ad0b-1d4727928d32
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: fbf2f71abdbce19483fb048d67a39cbb368a928e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="choose-parameters-toooptimize-your-algorithms-in-azure-machine-learning"></a>Az Azure Machine Learning a algoritmusok paraméterek toooptimize kiválasztása
Ez a témakör ismerteti, hogyan toochoose hello jobb hyperparameter beállítani az Azure Machine Learning algoritmus. A legtöbb gépi tanulási algoritmusok paraméterek tooset rendelkezik. Amikor egy modell betanításához tooprovide értékeket kell azokat a paramétereket. hello betanított modell hello hatékonyságát hello modell paraméterek, választott függ. hello művelet, hogy a rendszer hello optimális paraméterek készletét néven *kijelölési minta*.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Nincsenek különböző módokon toodo modell kiválasztása. A gépi tanulásban kereszt-ellenőrzési hello legszélesebb körben használt módszerek egyikét a modell kiválasztása, pedig hello alapértelmezett modell kijelölés mechanizmus az Azure Machine Learning. Azure Machine Learning támogatja az R és Python, mert mindig megvalósítható a saját modell kijelölés mechanizmusok R vagy Python használatával.

Hello folyamat során, hogy a legjobb paraméterhalmaz hello négy lépésben történik:

1. **Hello paraméter térköz meghatározása**: hello algoritmushoz, először döntse el, hello pontos paraméterértékek tooconsider szeretné.
2. **Hello kereszt-ellenőrzési beállítások megadása**: döntse el, hogyan toochoose kereszt-ellenőrzési modellrészt hello az adatkészlethez.
3. **Hello mérőszám meghatározása**: döntse el, milyen metrika toouse hello legjobb set paraméterek, például a pontosság meghatározásához, legfelső szintű közepét négyzet hiba, pontosság, visszaírási vagy f-pontszámot.
4. **Betanítása, értékelje ki, és hasonlítsa össze**: hello paraméterértékek egyedi kombinációit kereszt-ellenőrzési által végzett, és megadhatja a hello hiba metrika alapján. Értékelési és összehasonlító után dönthet úgy hello legjobban teljesítő modell.

hello a következő kép bemutatja, hogyan Ez elérhető az Azure Machine Learning mutatja be.

![Hello legjobb paraméterhalmaz keresése](./media/machine-learning-algorithm-parameters-optimize/fig1.png)

## <a name="define-hello-parameter-space"></a>Adja meg a hello paraméter terület
Állítsa be megfelelően hello modell alkalmazásinicializálási lépéshez hello paraméter adhat meg. hello paraméter ablaktábla az összes gépi tanulási algoritmusok trainer szolgáltatásnak két módja van: *egyetlen paraméter* és *paraméter tartomány*. Válassza ki a paraméter tartomány mód. Paraméter tartomány módban mindegyik paraméterhez több értékeket adhat meg. Hello szövegmezőben vesszővel elválasztott értékeket adhat meg.

![Két osztályú súlyozott döntési fa, egyetlen paraméter](./media/machine-learning-algorithm-parameters-optimize/fig2.png)

 Alternatív megoldásként definiálhatja hello maximális és minimális pontok hello rács és hello segítségével létrehozott pontok toobe teljes száma **használata tartomány jelentéskészítő**. Alapértelmezés szerint hello paraméterértékek skálán lineáris jönnek létre. Ha azonban **logaritmikus skála** be van jelölve, hello értékek jönnek létre a Logaritmikus skála hello (Ez azt jelenti, hogy hello arány hello szomszédos pontok, állandó a különbség helyett). Egész paraméterrel meghatározhatja széles kötőjellel. Például, "1 – 10" azt jelenti, hogy minden egész számok 1 és 10 közötti hello paraméter készletet alkotnak (mind a két szélsőértéket beleértve). A kevert üzemmód is támogatott. Például hello paraméterhalmaz "1 – 10, 20, 50" tartalmazhat egész számok 1-10, 20, 50 és.

![Két osztályú súlyozott döntési fa, paraméter](./media/machine-learning-algorithm-parameters-optimize/fig3.png)

## <a name="define-cross-validation-folds"></a>Kereszt-ellenőrzési modellrészt meghatározása
Hello [partíció és minta] [ partition-and-sample] modul használt toorandomly hozzárendelése modellrészt toohello adatok is lehetnek. A következő hello modul mintakonfiguráció hello, azt meg kell határozni öt modellrészt és véletlenszerűen rendelje hozzá egy szám toohello minta példányok.

![Partíció és minta](./media/machine-learning-algorithm-parameters-optimize/fig4.png)

## <a name="define-hello-metric"></a>Hello mérőszám meghatározása
Hello [Finomhangolhatják a modell-Hiperparamétereket] [ tune-model-hyperparameters] modul támogatást nyújt az empirically kiválasztása hello ajánlott egy adott algoritmus és a dataset paraméterek készletét. Ezenkívül képzési hello tooother információhoz modellre: hello **tulajdonságok** Ez a modul ablaktábláján tartalmaz hello metrika hello legjobb paraméterhalmaz meghatározásához. Két különböző legördülő listák a besorolás és regressziós algoritmus, illetve rendelkezik. Ha a szóban forgó hello algoritmus egy osztályozó algoritmus, hello regressziós metrika rendszer figyelmen kívül hagyja, és ez fordítva is igaz. Ebben a példában a hello mérőszáma **pontossága**.   

![Ismétlés paraméterek](./media/machine-learning-algorithm-parameters-optimize/fig5.png)

## <a name="train-evaluate-and-compare"></a>A vonat, kiértékeléséhez és összehasonlítása
hello azonos [Finomhangolhatják a modell-Hiperparamétereket] [ tune-model-hyperparameters] modul betanítja összes hello modellt a toohello paraméterhalmaz megfelelnek, különböző metrikák kiértékeli, és létrehozza a hello legjobban betanított modell hello alapján a metrika választja. Ez a modul két kötelező bemeneti rendelkezik:

* hello kellő tanuló
* hello adatkészlet

hello modul is rendelkezik egy nem kötelező bemeneti adatkészletet. Csatlakozás hello dataset modellrészek információk toohello kötelező dataset bemeneti. Ha hello adatkészlet nincs hozzárendelve semmilyen modellrészek információt, majd a 10-szeres kereszt-ellenőrzési automatikusan végre alapértelmezés szerint. Ha hello modellrészek hozzárendelés nem végezték el és érvényesítési dataset áll hello választható dataset port, egy tanítási-teszt mód van kiválasztva, és hello első adatkészlet egy használt tootrain hello modell minden paraméterkombináció.

![Súlyozott döntési fa osztályozó](./media/machine-learning-algorithm-parameters-optimize/fig6a.png)

hello modell majd kiértékelése hello érvényesítési adatkészlethez. hello hello modul látható kimeneti portjára különböző metrikák állapotban maradt funkciók a paraméterértékek. jobb oldali kimeneti portjára ad hello betanított modell, amely megfelel a kiválasztott metrika toohello szerint toohello legjobban teljesítő modell hello (**pontossága** ebben az esetben).  

![Érvényesítési adatkészlet](./media/machine-learning-algorithm-parameters-optimize/fig6b.png)

Hello pontos paraméterek megjelenítése hello jobb oldali kimeneti portjára által választott tekintheti meg. Ez a modell használható TesztKészlet pontozási vagy egy operationalized webszolgáltatás a modell betanítását, a mentés után.

<!-- Module References -->
[partition-and-sample]: https://msdn.microsoft.com/library/azure/a8726e34-1b3e-4515-b59a-3e4a475654b8/
[tune-model-hyperparameters]: https://msdn.microsoft.com/library/azure/038d91b6-c2f2-42a1-9215-1f2c20ed1b40/
