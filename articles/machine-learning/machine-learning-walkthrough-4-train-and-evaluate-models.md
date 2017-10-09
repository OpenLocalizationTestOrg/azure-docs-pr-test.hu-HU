---
title: "4. lépés: Betanítása és kiértékelése hello prediktív elemzési modellek |} Microsoft Docs"
description: "Hello 4. lépés fejlesztése egy prediktív megoldás forgatókönyv: vonat, pontozása, és az Azure Machine Learning Studióban több modellek kiértékeléséhez."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: d905f6b3-9201-4117-b769-5f9ed5ee1cac
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: d86d7c5ae7524f71fe44d985db67c4618b7965a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-step-4-train-and-evaluate-hello-predictive-analytic-models"></a>Útmutató 4. lépés: Betanítása és kiértékelése hello prediktív elemzési modellek
Ez a témakör hello negyedik lépése annak hello forgatókönyv [az Azure Machine Learning a prediktív elemzési megoldás fejlesztése](machine-learning-walkthrough-develop-predictive-solution.md)

1. [Machine Learning-munkaterület létrehozása](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [Meglévő adatok feltöltése](machine-learning-walkthrough-2-upload-data.md)
3. [Új kísérlet létrehozása](machine-learning-walkthrough-3-create-new-experiment.md)
4. **Betanítása és kiértékelése hello modellek**
5. [Hello webszolgáltatás telepítése](machine-learning-walkthrough-5-publish-web-service.md)
6. [Hozzáférés hello webszolgáltatás](machine-learning-walkthrough-6-access-web-service.md)

- - -
A machine learning modellek létrehozásához Azure Machine Learning Studio használatának előnyei egy hello egyik egyetlen kísérlet egyszerre egynél több típus modell hello képességét tootry, és hasonlítsa össze a hello eredmények. Az ilyen típusú kísérletezhet segít található hello ajánlott megoldás a probléma.

A forgatókönyv azt kidolgozása hello kísérletben, azt hozhat létre modellek két különböző típusú, és hasonlítsa össze a pontozási eredményei toodecide algoritmus toouse azt szeretnénk, a végső kísérletben.  

Nincsenek azt kiválaszthatják a különböző modellek. toosee hello modellek érhető el, bontsa ki a hello **Machine Learning** csomópontja hello modulpalettán, majd bontsa ki a **inicializálása modell** és az alatta hello csomópontok. Ez a kísérlet hello célokra hello választania azt [két osztályú támogatást vektoros gép] [ two-class-support-vector-machine] (SVM) és hello [két osztályú súlyozott döntési fa] [ two-class-boosted-decision-tree] modulok.    

> [!TIP]
> tooget súgó dönt a gépi tanulási algoritmus legjobban megfelelő hello adott probléma toosolve című [hogyan toochoose algoritmusok használata a Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md).
> 
> 

## <a name="train-hello-models"></a>Hello modellek betanítása

Fel kell venni mindkét hello [két osztályú súlyozott döntési fa] [ two-class-boosted-decision-tree] modul és [két osztályú támogatást vektoros gép] [ two-class-support-vector-machine] ezen modul kísérlet.

### <a name="two-class-boosted-decision-tree"></a>Két osztályú súlyozott döntési fája

Először is beállíthat hello súlyozott döntési fa modell.

1. Hello található [két osztályú súlyozott döntési fa] [ two-class-boosted-decision-tree] hello modulpalettán modulja, és húzza rá hello vászonra.

2. Hello található [tanítási modell] [ train-model] modul, hello vászonra húzva azt, és csatlakoztassa a hello hello kimenete [két osztályú súlyozott döntési fa] [ two-class-boosted-decision-tree]toohello modul bal oldali bemeneti portját a hello [tanítási modell] [ train-model] modul.
   
   Hello [két osztályú súlyozott döntési fa] [ two-class-boosted-decision-tree] modul hello általános modell, inicializálja és [tanítási modell] [ train-model] használja a betanítási adatok tootrain hello modell. 

3. Csatlakoztassa a bal oldali kimeneti hello hello bal [R-parancsfájl végrehajtása] [ execute-r-script] modul toohello jobb oldali bemeneti portját hello [tanítási modell] [ train-model] (azt modul úgy döntött, a [3. lépés](machine-learning-walkthrough-3-create-new-experiment.md) bal oldalán található hello Split Data modul a képzés hello érkező forgatókönyv toouse hello adatot).
   
   > [!TIP]
   > Nem szükséges két hello bemenetek és hello hello kimenetének egy [R-parancsfájl végrehajtása] [ execute-r-script] modul ehhez a kísérlethez, így azt is hagyhatja őket választani. 
   > 
   > 

Ez a része hello kísérlet most már a következőhöz hasonló:  

![A modell betanítása][1]

Most tootell hello kell [tanítási modell] [ train-model] , hogy szeretnénk hello modell toopredict hello hitelkockázat érték modul.

1. Jelölje be hello [tanítási modell] [ train-model] modul. A hello **tulajdonságok** ablaktáblán kattintson a **Oszlopválasztás**.

2. A hello **csak egy oszlop kiválasztása** párbeszédpanelen írja be a "credit kockázat" hello keresési mező alatt **elérhető oszlopok**, válassza ki a "Credit kockázat" alatt, és hello jobbra mutató nyíl gombra ( **>** ) toomove "Credit kockázat" túl**kijelölt oszlopok**. 

    ![Válassza ki a hitelkockázat oszlop hello hello tanítási modell modulhoz][0]

3. Kattintson a hello **OK** pipára.

### <a name="two-class-support-vector-machine"></a>Kétosztályos tartóvektor-gép

A következő beállítjuk hello SVM modell.  

Első, SVM egy kis leírását. Súlyozott döntési fák működnek jól bármilyen típusú szolgáltatásokat. Azonban hello SVM modul lineáris besorolás állít elő, mert hello létrehozott modellnek hello ajánlott tesztelési hiba a ha összes numerikus szolgáltatás hello ugyanolyan skála. az összes numerikus tooconvert szolgáltatások toohello azonos skálázni, "Tanh" átalakítás használjuk (a hello [normalizálása az adatok] [ normalize-data] modul). A számok ez átalakítja a [0,1] hello tartományba. hello SVM modul karakterlánc szolgáltatások toocategorical majd toobinary 0 vagy 1 szolgáltatásokkal és alakítja, így nem kell toomanually átalakítási karakterlánc szolgáltatásokat. Emellett nem szeretnénk tootransform hello hitelkockázat oszlop (oszlop 21) – numerikus, de ez jelenleg éppen betanítása hello érték hello modell toopredict, ezért ellenőriznünk kell, hogy tooleave azt kizárólag.  

hello SVM modell, tooset hello a következő:

1. Hello található [két osztályú támogatást vektoros gép] [ two-class-support-vector-machine] hello modulpalettán modulja, és húzza rá hello vászonra.

2. Kattintson a jobb gombbal hello [tanítási modell] [ train-model] modulban válassza **másolási**, kattintson a jobb gombbal a vásznon a hello és válassza a **Beillesztés**. hello példányát hello [tanítási modell] [ train-model] modulnak hello eredeti hello azonos oszlop kijelölés.

3. Csatlakozás hello hello kimenete [két osztályú támogatást vektoros gép] [ two-class-support-vector-machine] toohello modul bal oldali bemeneti portját a hello második [tanítási modell] [ train-model] a modul.

4. Hello található [normalizálása az adatok] [ normalize-data] modul, és húzza rá hello vászonra.

5. Csatlakoztassa a bal oldali kimeneti hello hello bal [R-parancsfájl végrehajtása] [ execute-r-script] modul toohello bemenetet ehhez a modul (figyelje meg, hogy egy modul hello kimeneti portjára csatlakoztatott toomore, mint egy másik modul lehetnek).

6. Csatlakozás a bal oldali kimeneti portjára hello hello [normalizálása az adatok] [ normalize-data] modul toohello jobb bemeneti portját hello második [tanítási modell] [ train-model] modul.

Ez a kísérlet része most hasonlóan kell kinéznie ezt:  

![Képzési hello második modell][2]  

Mostantól konfigurálhatja az hello [normalizálása az adatok] [ normalize-data] modul:

1. Kattintson a tooselect hello [normalizálása az adatok] [ normalize-data] modul. A hello **tulajdonságok** ablaktáblán válassza előbb **Tanh** a hello **átalakítási metódus** paraméter.

2. Kattintson a **Oszlopválasztás**, válasszon "Oszlop" **Begin With**, jelölje be **Include** hello első legördülő menüben válassza ki **oszloptípus**hello második legördülő menüből, és válassza ki **numerikus** hello harmadik legördülő. Azt határozza meg, hogy minden hello számoszlopaira (és csak numerikus) átalakításából származnak.

3. Kattintson a plusz jelre (+) toohello sarkában a sor hello – létrejön egy legördülő listák megnyílásának sorát. Válassza ki **kizárása** hello első legördülő menüben válassza ki **oszlopnevek** hello második legördülő menüből, és adja meg a "Credit kockázat" hello szövegmezőben. Azt határozza meg, hogy hello hitelkockázat oszlop figyelmen kívül hagyja (igazolnia kell a mert ebben az oszlopban numerikus, és így lenne alakul Ha azt nem zárja ki toodo).

4. Kattintson a hello **OK** pipára.  

    ![Hello normalizálása az adatok modul egy oszlopot válasszon ki][5]

Hello [normalizálása az adatok] [ normalize-data] modul már set tooperform hello hitelkockázat oszlop kivételével az összes numerikus oszlopokon Tanh átalakítás.  

## <a name="score-and-evaluate-hello-models"></a>Pontszám és hello modellek kiértékelése

Volt elválasztott hello adatok tesztelés hello használjuk [Split Data] [ split] modul tooscore a betanított modellek. Ezután összehasonlíthatja a Microsoft hello két modellek toosee jobb eredményeket létrehozó hello eredményeit.  

### <a name="add-hello-score-model-modules"></a>Hello Score Model modulok hozzáadása

1. Hello található [Score Model] [ score-model] modul, és húzza rá hello vászonra.

2. Csatlakozás hello [tanítási modell] [ train-model] modul, amely csatlakozott toohello [két osztályú súlyozott döntési fa] [ two-class-boosted-decision-tree] modul toohello bal oldali bemeneti port a hello [Score Model] [ score-model] modul.

3. Csatlakozás hello jobb [R-parancsfájl végrehajtása] [ execute-r-script] (a tesztelési adatok) modul toohello jobb bemeneti portját hello [Score Model] [ score-model] modul.

    ![Csatlakoztatott score Model-modul][6]
   
   Hello [Score Model] [ score-model] modul most is szükség adatok futtassa azt a hello modell ellenőrzése hello hello jóváírás adatait, és hasonlítsa össze hello előrejelzéseket hello modell állít elő, a tényleges hello credit kockázat adatok ellenőrzése hello oszlopa.

4. Másolja és illessze be a hello [Score Model] [ score-model] modul toocreate egy másolat.

5. Csatlakozás hello kimeneti hello SVM modell (Ez azt jelenti, hogy hello kimeneti portjára hello [tanítási modell] [ train-model] modul, amely csatlakozott toohello [két osztályú támogatást vektoros gép] [ two-class-support-vector-machine] modul) toohello bemeneti portját hello második [Score Model] [ score-model] modul.

6. Hello SVM modell tudunk toodo azonos átalakítása toohello Tesztadatok hello hasonlóan toohello betanítási adata. Ezért másolja és illessze be a hello [normalizálása az adatok] [ normalize-data] modul toocreate másodpéldányát, és csatlakoztassa toohello jobb [R-parancsfájl végrehajtása] [ execute-r-script] modul.

7. Csatlakozzon a bal oldali kimeneti hello hello a második [normalizálása az adatok] [ normalize-data] modul toohello jobb bemeneti portját hello második [Score Model] [ score-model] a modul.

    ![Mindkét Score Model modulok csatlakoztatva][7]

### <a name="add-hello-evaluate-model-module"></a>Hello modell kiértékelése modul hozzáadása

tooevaluate hello két pontozási eredményeinek, és hasonlítsa össze azokat, használjuk egy [modell kiértékelése] [ evaluate-model] modul.  

1. Hello található [modell kiértékelése] [ evaluate-model] modul, és húzza rá hello vászonra.

2. Csatlakozás hello kimeneti portjára hello [Score Model] [ score-model] hello társított modul súlyozott döntési fa modell toohello bal bemeneti portját hello [modell kiértékelése] [ evaluate-model] modul.

3. Csatlakozás más hello [Score Model] [ score-model] modul toohello jobb bemeneti port.  

    ![Modell kiértékelése modul csatlakoztatva][8]

### <a name="run-hello-experiment-and-check-hello-results"></a>Hello kísérlet futtatásához és hello eredmények ellenőrzése

toorun hello kísérletet, kattintson a hello **futtatása** vásznon a hello gombra. Néhány percig is eltarthat. Minden modul forgó mutató jeleníti meg, hogy fut-e, és majd egy zöld pipa megjelenít, amikor hello modul befejeződött. Ha minden hello modul be van jelölve, a hello kísérlet futása befejeződött.

hello kísérlet most hasonlóan kell kinéznie ezt:  

![Mindkét modell kiértékelése][3]

toocheck hello eredményeiben kattintson hello kimeneti portjára hello [modell kiértékelése] [ evaluate-model] modul, és válassza ki **Visualize**.  

Hello [modell kiértékelése] [ evaluate-model] modul hoz létre két görbék és metrikákat, amelyek lehetővé teszik két pontozott modellek hello toocompare hello eredményeit. Hello eredmények fogadó operátor jellemző (ROC) görbék, pontosság/visszaírási görbék, vagy növekedési tekintheti meg. Megjelenített további adatok zavart mátrix hello görbe alatti terület hello (AUC) összesítő értékeket és más metrikákkal tartalmazza. Hello küszöbérték megváltoztatása által áthelyezése hello csúszka jobbra vagy balra, és tekintse meg a hatása a metrikák hello készletét.  

toohello hello graph sarkában kattintson **dataset program pontozza a mennyiségeket** vagy **dataset toocompare program pontozza a mennyiségeket** toohighlight hello társított görbe és toodisplay hello szövegrészt kapcsolódó alábbi metrikákat. Hello jelmagyarázatban hello görbék, "Program pontozza a mennyiségeket dataset" megfelel-e bal oldali bemeneti portját a hello toohello [modell kiértékelése] [ evaluate-model] modul – ebben az esetben ez az hello súlyozott döntési fa modell. Toohello jobb oldali bemeneti portját - hello SVM modell abban az esetben, ha "Program pontozza a mennyiségeket dataset toocompare" felel meg. Ezek a címkék kiválasztásakor az adott modell hello görbe ki van jelölve, és hello megfelelő metrikák jelennek meg, ahogy az ábra a következő hello.  

![A modellek ROC görbévé][4]

Ezeket az értékeket megvizsgálásával eldöntheti, melyik modellben meg a keresett eredmények hello legközelebbi toogiving. Lépjen vissza, és többször kísérletbe paraméterértékek hello különböző modellek módosításával. 

hello tudományos és az eredmények értelmezéséhez és hello modell teljesítményének hangolása argentin Ez a forgatókönyv külső hello hatókörét. További segítségért előfordulhat, hogy olvassa el a következő cikkek hello:
- [Hogyan tooevaluate modell-teljesítmény az Azure gépi tanulás](machine-learning-evaluate-model-performance.md)
- [Az Azure Machine Learning a algoritmusok paraméterek toooptimize kiválasztása](machine-learning-algorithm-parameters-optimize.md)
- [Az Azure Machine Learning modell eredmények értelmezését](machine-learning-interpret-model-results.md)

> [!TIP]
> Minden egyes futtatásakor hello kísérlet feljegyzés az adott iterációs hello futtatása előzmények maradjanak. Ezeket az ismétlés megtekintése, és térjen vissza a tooany, kattintva **futtatása ELŐZMÉNYEINEK megtekintése** hello vászon alatt. Is **előzetes futtatása** a hello **tulajdonságok** ablaktábla tooreturn toohello iterációs közvetlenül megelőző hello egy megnyitott.
> 
> Hogy ismétlések kísérletbe másolatát kattintva **SAVE AS** hello vászon alatt. 
> Hello kísérlet használja **összegzés** és **leírás** tulajdonságok tookeep mi próbálta az a kísérlet ismétléseinek álló rekord.
> 
> További részletekért lásd: [kísérletismétlések kezelése a Azure Machine Learning Studio](machine-learning-manage-experiment-iterations.md).  
> 
> 

- - -
**Következő: [hello webszolgáltatás telepítése](machine-learning-walkthrough-5-publish-web-service.md)**

[0]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train-model-select-column.png
[1]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/experiment-with-train-model.png
[2]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/svm-model-added.png
[3]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/final-experiment.png
[4]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/roc-curves.png
[5]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/normalize-data-select-column.png
[6]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/score-model-connected.png
[7]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/both-score-models-added.png
[8]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/evaluate-model-added.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
