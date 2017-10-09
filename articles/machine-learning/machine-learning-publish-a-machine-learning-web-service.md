---
title: "a Machine Learning webszolgáltatásba aaaDeploy |} Microsoft Docs"
description: "Hogyan tooconvert egy tooa prediktív kísérletté kísérletezhet, a telepítés előkészítéséhez, majd az Azure Machine Learning webszolgáltatás üzembe."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 73a3e9c6-00d0-41d4-8cf1-2ec87713867e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: garye
ms.openlocfilehash: 9cb7af637632b2c3688c11483f29cf24df8fd065
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-azure-machine-learning-web-service"></a>Azure Machine Learning webszolgáltatás üzembe helyezése
Az Azure Machine Learning lehetővé teszi a toobuild, tesztelése és telepítése a prediktív elemzési megoldásokat.

A pont-az-áttekintése látható ez történik, a három lépést:

* **[Hozzon létre egy tanítási kísérletet]**  -Azure Machine Learning Studio olyan együttműködési Látványelem-fejlesztési környezet tootrain használni, és tesztelje a prediktív elemzési modell betanítási adatok, hozzá kell adni.
* **[Alakítsa át a tooa prediktív kísérletté]**  – Ha a modell még betanítva meglévő adatokkal, és készen áll a toouse azt tooscore új adatokat, előkészítése, és az előrejelzés kísérletbe egyszerűsítésére.
* **[A webszolgáltatás üzembe]**  -, a prediktív kísérletté telepítése egy [új] vagy [klasszikus] Azure webszolgáltatás. Felhasználók tooyour adatmodell küldhet és fogadhat a modell előrejelzéseket.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="create-a-training-experiment"></a>Hozzon létre egy tanítási kísérletet
egy prediktív elemzési modell tootrain, használhat Azure Machine Learning Studióban toocreate egy tanítási kísérletet, amelynél közé tartoznak a különböző modulok tooload betanítási adatok, készítse elő az hello adatokat szükség szerint, gépi tanulási algoritmusok vonatkoznak, és a hello eredmények értékelése . A kísérlet többször és különböző gépi tanulási a algoritmusok toocompare próbálja és hello eredmények értékelése.

hello folyamat létrehozásának és kezelésének képzési kísérletek alaposabban máshol van. További információkért lásd: ezek a cikkek:

* [Egy egyszerű kísérlet létrehozása az Azure Machine Learning Studióban](machine-learning-create-experiment.md)
* [Azure Machine Learning a prediktív megoldás kifejlesztése](machine-learning-walkthrough-develop-predictive-solution.md)
* [A betanítási adatok importálása az Azure Machine Learning Studióban](machine-learning-data-science-import-data.md)
* [Az Azure Machine Learning Studióban kísérletismétlések kezelése](machine-learning-manage-experiment-iterations.md)

## <a name="convert-hello-training-experiment-tooa-predictive-experiment"></a>Hello tanítási kísérletet tooa prediktív kísérletté átalakítása
Ha a modell már be tanítva, most készen áll a képzés kísérletezhet egy prediktív kísérletté tooscore új adatokká tooconvert.

Prediktív tooa átalakításával kísérletezhet, a betanított modell készen toobe pontozási webszolgáltatásként telepített kap. Felhasználók hello webszolgáltatás küldhet, bemeneti adatokat tooyour modell és a modell küldi vissza hello előrejelzés eredmények. Konvertálásakor prediktív tooa kísérletezhet, vegye figyelembe a mások által használt modell toobe várt módját.

tooconvert a tanítási kísérletet tooa prediktív kísérletezhet, kattintson a **futtatása** hello kísérletvászonra hello alsó részén, kattintson a **webes szolgáltatások beállítása**, majd jelölje be **prediktív webszolgáltatás** .

![Átalakítás tooscoring kísérlet](./media/machine-learning-publish-a-machine-learning-web-service/figure-1.png)

További információt a tooperform az átalakításhoz lásd [hogyan tooprepare a modell Azure Machine Learning Studio-KözpontiTelepítés](machine-learning-convert-training-experiment-to-scoring-experiment.md).

hello következő lépések bemutatják egy prediktív kísérletté egy új webszolgáltatás üzembe helyezése. Hello kísérlet klasszikus webszolgáltatásként is telepíthet.

## <a name="deploy-it-as-a-web-service"></a>Üzembe egy webszolgáltatás-bővítmény

Hello prediktív kísérletté telepíthet egy új webszolgáltatás-bővítmény vagy klasszikus webszolgáltatásként.

### <a name="deploy-hello-predictive-experiment-as-a-new-web-service"></a>Hello prediktív kísérletté egy új webszolgáltatás-bővítmény telepítése
Most, hogy hello prediktív kísérletté elő van készítve, telepíthet egy új Azure webszolgáltatás. Hello webszolgáltatás segítségével, is el lehet küldeni tooyour adatmodell, és hello modell visszatér az előrejelzés.

toodeploy a prediktív kísérletezhet, kattintson a **futtatása** hello hello alján kísérlet vászonra. Amikor hello kísérlet futása befejeződött, kattintson a **webes szolgáltatás telepítése** válassza ki **webes szolgáltatás telepítése [új]**.  Megnyílik a hello Machine Learning webszolgáltatás portál hello telepítési lapján.

> [!NOTE] 
> toodeploy egy új webszolgáltatás-bővítmény, megfelelő engedélyekkel kell rendelkeznie a hello előfizetés toowhich meg hello webes szolgáltatás telepítéséhez. További információ: [egy webszolgáltatás-bővítmény hello Azure Machine Learning webszolgáltatások portálon kezelheti](machine-learning-manage-new-webservice.md). 

#### <a name="machine-learning-web-service-portal-deploy-experiment-page"></a>Machine Learning webszolgáltatásba portal telepítés kísérlet lap
Hello telepítése kísérlet oldalon adjon meg egy nevet, hello webszolgáltatáshoz.
Válasszon egy tarifacsomagot. Rendelkezik egy meglévő tarifacsomagot is kiválaszthatja, ellenkező esetben kell létrehoznia egy új ár terv hello szolgáltatást.

1. A hello **ár terv** legördülő listán, jelöljön ki egy meglévő terv vagy hello **jelölje be új csomagot** lehetőséget.
2. A **neve**, adjon meg egy nevet, amely azonosítja a számlán hello terv.
3. Válasszon ki egy hello **havi megtervezése rétegek**. rétegek alapértelmezett toohello tervek az alapértelmezett területi és a webszolgáltatás hello terv telepített toothat régióban.

Kattintson a **telepítés** és hello **gyors üzembe helyezés** a webszolgáltatáshoz lap nyílik meg.

hello webes szolgáltatás gyors üzembe helyezés lap lehetővé teszi az hozzáférési és útmutatást a hello leggyakoribb feladatokat kell végrehajtania egy webszolgáltatás-bővítmény létrehozása után. Itt egyszerűen hozzáférhet hello tesztoldalt és a felhasználás lap.

<!-- ![Deploy hello web service](./media/machine-learning-publish-a-machine-learning-web-service/figure-2.png)-->

#### <a name="test-your-new-web-service"></a>Az új webes szolgáltatás tesztelése
tootest az új webszolgáltatás kattintson **webszolgáltatás tesztelése** a közös műveletek területen. Hello tesztelése oldalon tesztelheti, hogy a webszolgáltatás, egy kérés-válasz szolgáltatás (RR-EKET) vagy egy kötegelt végrehajtási szolgáltatás (BES).

hello RR-EKET tesztoldalt hello bemenetek, kimenetek és globális paramétereket, hello kísérlet megadott jeleníti meg. tootest hello webszolgáltatás, manuálisan értékeket adhat meg megfelelő hello bemeneti adatokat vagy adjon meg egy vesszővel tagolt adatfájlból (CSV) formátumú fájl hello teszt értékeket tartalmazó.

tootest RRS használatával üzemmódról hello lista nézet, adja meg megfelelő értékeket hello bemeneti adatokat, és kattintson a **teszt kérés-válasz**. Az előrejelzés eredmények hello kimeneti oszlop toohello balra jeleníti meg.

![Hello webszolgáltatás telepítése](./media/machine-learning-publish-a-machine-learning-web-service/figure-5-test-request-response.png)

tootest a BES esetében kattintson **kötegelt**. Hello kötegelt tesztelése oldalon a bemeneti alatt kattintson a Tallózás gombra, és válassza ki a megfelelő mintaértékek tartalmazó CSV-fájl. Ha egy CSV-fájl nem rendelkezik, és a használata a Machine Learning Studio prediktív kísérletté létrehozott, töltse le a prediktív kísérletté hello adatkészlet, és használja azt.

toodownload hello adatkészlet, nyissa meg a Machine Learning Studio. Nyissa meg a prediktív kísérletté, és kattintson a jobb gombbal a kísérleti fázisú funkciókat hello bemenet. Hello helyi menüből válassza ki a **dataset** majd **letöltése**.

![Hello webszolgáltatás telepítése](./media/machine-learning-publish-a-machine-learning-web-service/figure-7-mls-download.png)

Kattintson a **teszt**. a kötegelt végrehajtási feladat hello állapotának megjelenítése toohello jogosultság **teszt kötegelt feladatok**.

![Hello webszolgáltatás telepítése](./media/machine-learning-publish-a-machine-learning-web-service/figure-6-test-batch-execution.png)

<!--![Test hello web service](./media/machine-learning-publish-a-machine-learning-web-service/figure-3.png)-->

A hello **konfigurációs** lapon hello leírást módosíthatja, cím, hello tárfiók kulcsának frissítése, és mintaadatokat engedélyezése a webszolgáltatáshoz.

![Hello webszolgáltatás konfigurálása](./media/machine-learning-publish-a-machine-learning-web-service/figure-8-arm-configure.png)

Miután hello webszolgáltatás telepítése után, teheti:

* **Hozzáférés** azt hello web service API-n keresztül.
* **Kezelése** azt a Azure Machine Learning web services portálra, vagy a klasszikus Azure portálon hello.
* **Frissítés** , ha a modell megváltozik.

#### <a name="access-your-new-web-service"></a>Az új webes szolgáltatás
A webszolgáltatás a Machine Learning Studio telepít, miután toohello adatszolgáltatás küldhet és válaszokat programozott módon kapni.

Hello **felhasználás** lap minden, a webszolgáltatás kell tooaccess hello információkat biztosít. Például hello API-kulcs engedélyezett tooallow toohello szolgáltatás biztosítja.

A Machine Learning webszolgáltatásba elérésével kapcsolatos további információkért lásd: [hogyan tooconsume az Azure Machine Learning Web service](machine-learning-consume-web-services.md).

#### <a name="manage-your-new-web-service"></a>Az új webes szolgáltatás kezelése
A új szolgáltatások Machine Learning webszolgáltatások webportál kezelheti. A hello [portál fő lapján](https://services.azureml-test.net/), kattintson a **webszolgáltatások**. Szolgáltatások hello weblapról törölheti vagy másolja át a szolgáltatást. egy adott szolgáltatás toomonitor kattintson hello szolgáltatást, és kattintson a **irányítópult**. hello webes szolgáltatással kapcsolatos toomonitor kötegelt kattintson **kötegelt kérelmek napló**.

### <a name="deploy-hello-predictive-experiment-as-a-classic-web-service"></a>Klasszikus webszolgáltatásként hello prediktív kísérletté telepítése

Most, hogy hello prediktív kísérletté megfelelően elő van készítve, telepítheti azt a klasszikus Azure webszolgáltatásként. Hello webszolgáltatás segítségével, is el lehet küldeni tooyour adatmodell, és hello modell visszatér az előrejelzés.

toodeploy a prediktív kísérletezhet, kattintson a **futtatása** hello hello alján kísérletezhet a vászonra, és kattintson a **webes szolgáltatás telepítése**. hello webszolgáltatás be van állítva, és hello web service irányítópultot kerülnek.

![Hello webszolgáltatás telepítése](./media/machine-learning-publish-a-machine-learning-web-service/figure-2.png)

#### <a name="test-your-classic-web-service"></a>A klasszikus webes szolgáltatás tesztelése

Hello webszolgáltatás hello Machine Learning webszolgáltatások portál vagy a Machine Learning Studio tesztelheti.

tootest hello kérelem-válasz webes szolgáltatás, kattintson a hello **teszt** hello webes szolgáltatás irányítópultját gombjára. Egy párbeszédpanel jelenik meg, tooask meg hello bemeneti adatok hello szolgáltatás. Ezek a kísérletben pontozási hello által várt hello oszlopok. Adja meg az adatok, és kattintson a **OK**. hello webes szolgáltatás által létrehozott hello eredmények hello irányítópult hello alján jelennek meg.

Kattinthat a hello **teszt** tekintse meg a szolgáltatás hello Azure Machine Learning webszolgáltatások portálon hivatkozás tootest hello új webes szolgáltatás szakasz korábban ismertetett módon.

tootest hello kötegelt végrehajtási szolgáltatás, kattintson a **teszt** preview hivatkozásra. Hello kötegelt tesztelése oldalon a bemeneti alatt kattintson a Tallózás gombra, és válassza ki a megfelelő mintaértékek tartalmazó CSV-fájl. Ha egy CSV-fájl nem rendelkezik, és a használata a Machine Learning Studio prediktív kísérletté létrehozott, töltse le a prediktív kísérletté hello adatkészlet, és használja azt.

![Teszt hello webszolgáltatás](./media/machine-learning-publish-a-machine-learning-web-service/figure-3.png)

A hello **konfigurációs** lapon módosíthatja a hello szolgáltatás hello megjelenítendő nevét, és adjon neki egy leírást. hello név és leírás megjelenik-e hello [a klasszikus Azure portálon](http://manage.windowsazure.com/) a webszolgáltatások kezelhetik.

Megadhatja egy leírást a bemeneti adatok, a kimeneti adatok és a webes szolgáltatás paraméterei egy karakterláncot adjon meg minden oszlop alapján a **bemeneti SÉMÁT**, **kimeneti SÉMÁVAL**, és **WEBSZOLGÁLTATÁS A paraméter**. A leírások hello minta kód dokumentációjában hello webszolgáltatáshoz használt.

Naplózási toodiagnose esetleges hibákat, ha hozzáfér a webes szolgáltatás is lát használatával engedélyezheti. További információkért lásd: [naplózását a Machine Learning webszolgáltatások](machine-learning-web-services-logging.md).

![Hello webszolgáltatás konfigurálása](./media/machine-learning-publish-a-machine-learning-web-service/figure-4.png)

Beállíthatja úgy is hello webszolgáltatás végpontjainak hello a hello Azure Machine Learning webszolgáltatások portál hasonló toohello eljárás korábban hello új webes szolgáltatás szakaszban látható. hello-beállítások, adhat hozzá vagy módosítható hello szolgáltatás leírása, a naplózásának engedélyezése és a mintaadatok engedélyezése teszteléshez.

#### <a name="access-your-classic-web-service"></a>A klasszikus webes szolgáltatáshoz
A webszolgáltatás a Machine Learning Studio telepít, miután toohello adatszolgáltatás küldhet és válaszokat programozott módon kapni.

hello irányítópult tooaccess a webszolgáltatás szükséges összes hello információkat biztosít. Például hello API-kulcs része tooallow engedélyezett toohello szolgáltatás, és API súgóoldalak biztosított toohelp programozás a kezdéshez.

A Machine Learning webszolgáltatásba elérésével kapcsolatos további információkért lásd: [hogyan tooconsume az Azure Machine Learning Web service](machine-learning-consume-web-services.md).

#### <a name="manage-your-classic-web-service"></a>A klasszikus webszolgáltatás kezelése
Nincsenek a különböző műveleteket végezhet toomonitor egy webszolgáltatás-bővítmény. A frissítést, és törölje azt. További végpontok tooa klasszikus webszolgáltatás hozzáadása toohello alapértelmezett végpont annak regisztrálásakor létrehozott is hozzáadhat.

További információkért lásd: [kezelése az Azure Machine Learning-munkaterület](machine-learning-manage-workspace.md) és [egy webszolgáltatás-bővítmény hello Azure Machine Learning webszolgáltatások portálon kezelheti](machine-learning-manage-new-webservice.md).

<!-- When this article gets published, fix hello link and uncomment
For more information on how toomanage Azure Machine Learning web service endpoints using hello REST API, see **Azure machine learning web service endpoints**.
-->

## <a name="update-hello-web-service"></a>Hello webes szolgáltatás frissítése
Tooyour webszolgáltatás hello modell módosítása a további betanítási adatok, például a módosításokat, és telepítse újra, felülírja az eredeti webszolgáltatás hello.

tooupdate hello webszolgáltatás, nyissa meg a hello eredeti prediktív kísérletté toodeploy hello webszolgáltatás használta, és egy szerkeszthető másolat kattintva **SAVE AS**. A módosításokat, és kattintson a **webes szolgáltatás telepítése**.

Telepítése előtt a kísérlet, mert a kell adnia, ha azt szeretné, toooverwrite (klasszikus webszolgáltatás) vagy a frissítés (új webes szolgáltatás) hello létező szolgáltatás. Kattintson a **Igen** vagy **frissítés** hello meglévő webes szolgáltatás leáll, és telepíti a hello új prediktív kísérletté üzembe van helyezve a helyére.

> [!NOTE]
> Ha hello eredeti webszolgáltatás konfigurációs változtatásokat hajtott végre, például megad egy új megjelenítendő név vagy leírás, akkor tooenter ezeket az értékeket újra.
> 
> 

A webszolgáltatás frissítésének egyik lehetséges módja tooretrain hello modell programozott módon. További információ: [Retrain Machine Learning models programmatically](machine-learning-retrain-models-programmatically.md) (Machine Learning-modellek szoftveres átképezése).

<!-- internal links -->
[Hozzon létre egy tanítási kísérletet]: #create-a-training-experiment
[Alakítsa át a tooa prediktív kísérletté]: #convert-the-training-experiment-to-a-predictive-experiment
[A webszolgáltatás üzembe]: #deploy-it-as-a-web-service
[új]: #deploy-the-predictive-experiment-as-a-new-Web-service
[klasszikus]: #deploy-the-predictive-experiment-as-a-new-Web-service
[Access]: #access-the-Web-service
[Manage]: #manage-the-Web-service-in-the-azure-management-portal
[Update]: #update-the-Web-service
