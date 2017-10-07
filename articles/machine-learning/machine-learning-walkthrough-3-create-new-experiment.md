---
title: "3. lépés:, Hozzon létre egy új gépi tanulási kísérlet |} Microsoft Docs"
description: "3. lépésében hello fejlesztése egy prediktív megoldás bemutató: az Azure Machine Learning Studióban hozzon létre egy új tanítási kísérletet."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 660e3c27-55ef-4c33-a4e9-dff4d1224630
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: 4697d461a205c50c8d2aa6a3bd56697840cb30f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-step-3-create-a-new-azure-machine-learning-experiment"></a>Az útmutató 3. lépése: Új Azure Machine Learning-kísérlet létrehozása
Ez az hello harmadik lépése annak hello forgatókönyv [az Azure Machine Learning a prediktív elemzési megoldás fejlesztése](machine-learning-walkthrough-develop-predictive-solution.md)

1. [Machine Learning-munkaterület létrehozása](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [Meglévő adatok feltöltése](machine-learning-walkthrough-2-upload-data.md)
3. **Új kísérlet létrehozása**
4. [Betanítása és kiértékelése hello modellek](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [Hello webszolgáltatás telepítése](machine-learning-walkthrough-5-publish-web-service.md)
6. [Hozzáférés hello webszolgáltatás](machine-learning-walkthrough-6-access-web-service.md)

- - -
Ez a forgatókönyv hello következő lépése toocreate egy kísérletben a Machine Learning Studióban feltöltése a Microsoft hello adatkészletet használó.  

1. A Studióban kattintson **+ új** hello ablak hello alján.
2. Válassza ki **kísérlet**, majd válassza az "Üres kísérlet". 

    ![Új kísérlet létrehozása][0]

2. SELECT hello alapértelmezett neve hello vásznon a hello tetején kísérletezhet, és adjon neki jelentéssel bíró toosomething.

    ![Nevezze át a kísérlet][5]
   
   > [!TIP]
   > Van egy célszerű toofill **összegzés** és **leírás** hello kísérleti fázisú funkciókat a hello **tulajdonságok** ablaktáblán. E tulajdonságok adjon alkalommal toodocument hello kísérlet hello, így bárki, aki ellenőrzi, hogy az azt később megértse a célok és módszert.
   > 
   > ![Kísérlet tulajdonságai][6]
   > 
3. A hello modul paletta toohello bal oldalán hello kísérletvászonra, bontsa ki a **mentett adatkészletek**.
4. Keresés hello adatkészlet alapján létrehozott **saját adatkészletek** , és húzza rá hello vászonra. Hello dataset hello hello név beírásával is megtalálhatja **keresési** hello paletta fölött.  

    ![Hello dataset toohello kísérlet hozzáadása][7]

## <a name="prepare-hello-data"></a>Hello adatok előkészítése
Megtekintheti hello első 100 adatsorokat és néhány statisztikai adatot hello teljes adatkészlet hello: hello kimeneti port hello adatkészlet (hello vonatkozó kis kör hello lap alján) gombra, és válassza ki **Visualize**.  

Hello adatfájl nem kapott oszlopának fejlécére kattintva rendezhető, mert Studio általános fejlécére kattintva rendezhető nyújtott (Oszlop1, Col2, *stb*). Jó fejlécek nem alapvető toocreating modell, de teszik könnyebben toowork hello adatokkal hello kísérletben. Is ha azt végül közzéteheti egy webszolgáltatás, hello fejlécére kattintva rendezhető azonosításához hello oszlopok toohello felhasználói hello szolgáltatást.  

Azt is hozzáadhat oszlopának fejlécére kattintva rendezhető, hello segítségével [szerkesztése metaadatok] [ edit-metadata] modul.
Hello használata [szerkesztése metaadatok] [ edit-metadata] társított dataset modul toochange metaadatai. Ebben az esetben használjuk, további rövid nevek tooprovide oszlopfejlécek. 

toouse [szerkesztése metaadatok][edit-metadata], akkor először adja meg, mely oszlopok toomodify (ebben az esetben az összes azokat.) Ezt követően adja meg az hello művelet toobe végre ezek az oszlopok (ebben az esetben az oszlopfejlécek módosítása.)

1. A hello modulpalettán, írja be a "metaadatok" hello **keresési** mezőbe. Hello [szerkesztése metaadatok] [ edit-metadata] hello modul listájában jelenik meg.

2. Kattintással és húzással vigye a hello [szerkesztése metaadatok] [ edit-metadata] alakzatot hello modult, és helyezze a korábban hozzáadott hello dataset alatt.

3. Csatlakozás hello dataset toohello [szerkesztése metaadatok][edit-metadata]: hello kimeneti port hello adatkészlet (hello vonatkozó kis kör hello dataset hello alján) gombra, húzza toohello bemeneti portját a [metaadatok szerkesztése ] [ edit-metadata] (hello kis kör hello modul hello tetején), majd engedje fel hello egérgombot. hello dataset és modul csatlakoztatva tartani akkor is, ha Ön Navigálás vagy hello vászonra.
   
   hello kísérlet most hasonlóan kell kinéznie ezt:  
   
   ![Szerkesztés metaadatok hozzáadása][1]
   
   hello piros felkiáltójel azt jelenti, hogy jelenleg még nem ez a modul hello tulajdonságainak még. Igazolnia kell végeznie, hogy a Tovább gombra.
   
   > [!TIP]
   > A Megjegyzés tooa modul duplán hello modul és a belépés szöveget adhat hozzá. Ez segít gyorsan áttekintse milyen hello modul a kísérletben végez műveletet. Ebben az esetben kattintson duplán a hello [szerkesztése metaadatok] [ edit-metadata] modul és a típus hello Megjegyzés "Add oszlopának fejlécére kattintva rendezhető". Kattintson a bárhol máshol a hello vászonra tooclose hello szövegmezőben. toodisplay hello megjegyzést, hello lefelé mutató nyílra hello modulon.
   > 
   > ![Megjegyzés hozzáadása a metaadatok modul szerkesztése][8]
   > 
4. Válassza ki [szerkesztése metaadatok][edit-metadata], és a hello **tulajdonságok** ablaktábla toohello jog a vásznon a hello, kattintson a **Oszlopválasztás**.

5. A hello **egy oszlopot válasszon ki** párbeszédpanelen válassza ki az összes hello sorainak **elérhető oszlopok** kattintson > toomove őket túl**kijelölt oszlopok**.
   hello párbeszédpanel kell kinéznie:

   ![Az összes kijelölt oszlopok Oszlopválasztó][2]

6. Kattintson a hello **OK** pipára.

7. Vissza a hello **tulajdonságok** ablaktáblán keresse meg hello **új oszlopnevek** paraméter. Ebben a mezőben adja meg a neveket hello 21 oszlopok hello adatkészletben, egymástól vesszővel és az oszlopok sorrendjét. Hello dataset dokumentáció hello UCI webhelyen szerezhet be hello oszlopok nevét, vagy a kényelem másolja és illessze be a következő lista hello:  
   
       Status of checking account, Duration in months, Credit history, Purpose, Credit amount, Savings account/bond, Present employment since, Installment rate in percentage of disposable income, Personal status and sex, Other debtors, Present residence since, Property, Age in years, Other installment plans, Housing, Number of existing credits, Job, Number of people providing maintenance for, Telephone, Foreign worker, Credit risk  
   
   hello tulajdonságok panelen így néz ki:
   
   ![A Szerkesztés metaadatok tulajdonságai][3]

> [!TIP]
> Ha azt szeretné, hogy tooverify hello oszlopának fejlécére kattintva rendezhető, futtassa a hello kísérlet (kattintson **futtatása** hello kísérletvászon alatt). Amikor futása (zöld pipa jelenik meg [metaadatok szerkesztése][edit-metadata]), kattintson hello kimeneti portjára hello [metaadatok szerkesztése] [ edit-metadata] a modul, és válassza ki **Visualize**. Bármely modul kimenete hello megtekintheti a hello azonos módon tooview hello folyamatban hello hello kísérlet az adatok.
> 
> 

## <a name="create-training-and-test-datasets"></a>Képzés és az adatkészletek
Néhány tootrain hello adatmodell, és néhány tootest kell azt.
Így hello a következő lépésben hello kísérlet, a Microsoft hello dataset felosztása két külön adatkészletek: egy a képzési tekinthetők, és egy tesztelési azt.

toodo, hello használjuk [Split Data] [ split] modul.  

1. Hello található [Split Data] [ split] modul, hello vászonra húzva azt, és csatlakoztassa toohello [szerkesztése metaadatok] [ edit-metadata] modul.

2. Alapértelmezés szerint hello megosztási arány 0,5 és hello **Randomized vegyes** paraméter értéke. Ez azt jelenti, hogy hello adatok véletlenszerű fél egy hello-porton keresztül kimeneti [Split Data] [ split] modul, és fele a hello más. Akkor is állítsa be ezeket a paramétereket, valamint hello **véletlenszerű kezdőérték** paraméter, toochange hello elosztva a modell betanítására és tesztelésére adatokat. Ebben a példában azt hagyja azokat-van.
   
   > [!TIP]
   > hello tulajdonság **hello sorok először kimeneti adatkészlet** meghatározza, hogy mennyi hello adatok keresztül hello kimeneti *bal oldali* kimeneti port. Például ha hello arány too0.7, majd hello adatok 70 %-át egy kimenet keresztül hello port – 30 % balra hello jobb porton keresztül.  
   > 
   > 

3. Kattintson duplán a hello [Split Data] [ split] modul, és írjon hello megjegyzést, "képzési/tesztelési adatok felosztása 50 %". 

Használhatunk hello hello kimenetének [Split Data] [ split] modul azonban azt hasonló, de most válasszon toouse hello bal oldali kimeneti adatok betanítási adatok, és jobb hello kimeneti, tesztelési adatai.  

A hello [előző lépésben](machine-learning-walkthrough-2-upload-data.md), alacsony, magas hitelkockázat misclassifying hello költségét értéke ötször magasabb, mint egy alacsony hitelkockázat, nagy misclassifying hello költségét. a tooaccount, azt létrehozni, amely tükrözi a költség függvény új adatkészlet. Hello új adatkészlet minden magas kockázatú példa replikálja a rendszer ötször, amíg minden alacsony kockázat például a rendszer nem replikálja.   

A replikáció tehetünk ennek R-kód használatával:  

1. Keresse meg, és húzza hello [R-parancsfájl végrehajtása] [ execute-r-script] modul hello kísérlet vászonra. 

2. Csatlakozás a bal oldali kimeneti portjára hello hello [Split Data] [ split] modul toohello első bemenet port ("Dataset1") a hello [R-parancsfájl végrehajtása] [ execute-r-script] a modul.

3. Kattintson duplán a hello [R-parancsfájl végrehajtása] [ execute-r-script] modul, és írja be a hello Megjegyzés "Set költség helyesbítése".

4. A hello **tulajdonságok** panelen, törölje hello alapértelmezett szöveg hello **R-parancsfájl** paraméter, és írja be ezt a parancsfájlt:
   
       dataset1 <- maml.mapInputPort(1)
       data.set<-dataset1[dataset1[,21]==1,]
       pos<-dataset1[dataset1[,21]==2,]
       for (i in 1:5) data.set<-rbind(data.set,pos)
       maml.mapOutputPort("data.set")

    ![R-parancsfájl hello R-parancsfájl végrehajtása modul][9]

Igazolnia kell toodo replikációs műveletet minden egyes kimeneti a hello [Split Data] [ split] modul, a modell betanítására és tesztelésére adatok hello rendelkezik hello azonos költségű módosítása. Ez az hello másolásával legegyszerűbb módja toodo hello [R-parancsfájl végrehajtása] [ execute-r-script] modul azt végzett és csatlakoztatásával toohello más kimeneti portot a hello [Split Data] [ split] modul.

1. Kattintson a jobb gombbal hello [R-parancsfájl végrehajtása] [ execute-r-script] modul, és válassza ki **másolási**.

2. Kattintson a jobb gombbal a hello kísérletvászonra, és válassza ki **Beillesztés**.

3. Új modul hello húzzon egy helyen, és csatlakoztassa a jobb oldali kimeneti portjára hello hello [Split Data] [ split] modul toohello először az új port a bemeneti [R-parancsfájl végrehajtása] [ execute-r-script] modul. 

4. Hello vásznon a hello alján kattintson **futtatása**. 

> [!TIP]
> hello R-parancsfájl végrehajtása modul hello példányát azonos parancsfájl hello eredeti modulként hello tartalmazza. Másolja és illessze be egy modult a vásznon a hello, hello másolási eredeti hello hello tulajdonságainak megmaradnak.  
> 
> 

A kísérletben most már a következőhöz hasonló:

![Vegyes modul, és az R parancsfájlok hozzáadása][4]

Az R parancsfájlok használata a kísérleti további információkért lásd: [kiterjesztése az r kísérletbe](machine-learning-extend-your-experiment-with-r.md).

**Következő: [tanítási és hello modellek kiértékelése](machine-learning-walkthrough-4-train-and-evaluate-models.md)**

[0]: ./media/machine-learning-walkthrough-3-create-new-experiment/create-new-experiment.png
[5]: ./media/machine-learning-walkthrough-3-create-new-experiment/rename-experiment.png
[6]: ./media/machine-learning-walkthrough-3-create-new-experiment/experiment-properties.png
[7]: ./media/machine-learning-walkthrough-3-create-new-experiment/add-dataset-to-experiment.png
[8]: ./media/machine-learning-walkthrough-3-create-new-experiment/edit-metadata-with-comment.png
[9]: ./media/machine-learning-walkthrough-3-create-new-experiment/execute-r-script.png
[1]: ./media/machine-learning-walkthrough-3-create-new-experiment/experiment-with-edit-metadata-module.png
[2]: ./media/machine-learning-walkthrough-3-create-new-experiment/select-columns.png
[3]: ./media/machine-learning-walkthrough-3-create-new-experiment/edit-metadata-properties.png
[4]: ./media/machine-learning-walkthrough-3-create-new-experiment/experiment.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
