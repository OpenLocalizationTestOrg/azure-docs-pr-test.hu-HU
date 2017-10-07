---
title: "5. lépés: Hello Machine Learning webszolgáltatás telepítése |} Microsoft Docs"
description: "Hello 5. lépés fejlesztése egy prediktív megoldás forgatókönyv: központi telepítése egy prediktív kísérletben a Machine Learning Studióban webszolgáltatásként."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 3fca74a3-c44b-4583-a218-c14c46ee5338
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: 76391010972ed1450bbda8bfb2352c7b22b51ccc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-step-5-deploy-hello-azure-machine-learning-web-service"></a>Útmutató 5. lépés: Hello Azure Machine Learning webszolgáltatás telepítése
Hello ötödik lépéssel hello forgatókönyv [az Azure Machine Learning a prediktív elemzési megoldás fejlesztése](machine-learning-walkthrough-develop-predictive-solution.md)

1. [Machine Learning-munkaterület létrehozása](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [Meglévő adatok feltöltése](machine-learning-walkthrough-2-upload-data.md)
3. [Új kísérlet létrehozása](machine-learning-walkthrough-3-create-new-experiment.md)
4. [Betanítása és kiértékelése hello modellek](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. **Hello webszolgáltatás telepítése**
6. [Hello webes szolgáltatás](machine-learning-walkthrough-6-access-web-service.md)

- - -
toogive mások egy alkalommal toouse hello a forgatókönyv során korábban kidolgoztunk prediktív modellt, telepíthetünk azt az Azure-on webszolgáltatásként.

Toothis pont be azt korábban már ki a modell betanítása. Azonban telepített hello szolgáltatás már nem lesz toodo képzési - által hello felhasználói bevitel a modell pontozása érintetlen toogenerate új előrejelzéseket. Így fogjuk toodo néhány előkészítő tooconvert ez kísérletezhet a egy ***képzési*** tooa kísérletezhet ***prediktív*** kipróbálásához. 

Ez az egy három lépéses folyamat:  

1. Távolítsa el az egyik hello modell
2. Átalakítás hello *tanítási kísérletet* történő létrehoztunk Önnek egy *prediktív kísérletté*
3. Hello prediktív kísérletté egy webszolgáltatás-bővítmény telepítése

## <a name="remove-one-of-hello-models"></a>Távolítsa el az egyik hello modell

Először létre kell tootrim ehhez a kísérlethez le egy kicsit. A Microsoft jelenleg két különböző modell hello kísérletben, viszont csak szeretnénk toouse egy modell telepítésekor jelenleg ez webszolgáltatásként.  

Tegyük fel, azt döntött, hogy súlyozott hello fa modell jobban teljesített, mint hello SVM modell. Így hello első lépésként toodo eltávolítása hello [két osztályú támogatást vektoros gép] [ two-class-support-vector-machine] modul és a képzési azt használt hello modulok. Érdemes lehet toomake hello kísérlet másolatát először kattintva **Mentés másként** hello hello alján kísérlet vászonra.

Igazolnia kell a modulok a következő toodelete hello:  

* [Két osztályú támogatást vektoros gép][two-class-support-vector-machine]
* [Modell betanításához] [ train-model] és [Score Model] [ score-model] csatlakoztatott tooit volt modulok
* [Adatok normalizálása] [ normalize-data] (mindkettő)
* [Értékelje ki a modell] [ evaluate-model] (mert a rendszer végzett hello modell kiértékelése)

Válassza ki modulokhoz, és nyomja le az ENTER hello Delete billentyűt, vagy kattintson a jobb gombbal hello modult, majd **törlése**. 

![Hello SVM modell eltávolítása][3a]

A modell most hasonlóan kell kinéznie ezt:

![Hello SVM modell eltávolítása][3]

Most már készen áll a toodeploy el ez a modell használatával hello [két osztályú súlyozott döntési fa][two-class-boosted-decision-tree].

## <a name="convert-hello-training-experiment-tooa-predictive-experiment"></a>Hello tanítási kísérletet tooa prediktív kísérletté átalakítása

tooget ez modell kész, telepítési, van erre szükség tooconvert kísérlet tooa prediktív kísérletté betanítása. Ez a három lépést foglal magában:

1. Jelenleg be van tanítva, és akkor cserélje le a képzési modulok hello modell mentése
2. Trim hello kísérlet tooremove modulok képzési csak szükséges
3. Adja meg, ahol hello webszolgáltatás bemenetet fogad, és ahol hello kimeneti generál

Azt megteheti ezt manuálisan, de Szerencsére minden három lépést is elvégezhető kattintva **webes szolgáltatások beállítása** hello kísérletvászonra hello alján (hello választja **prediktív webszolgáltatás** a beállítás).

> [!TIP]
> Ha további részleteket a Mi történik, egy tanítási kísérletet tooa prediktív konvertálásakor kísérletezhet, lásd: [hogyan tooprepare a modell Azure Machine Learning Studio-KözpontiTelepítés](machine-learning-convert-training-experiment-to-scoring-experiment.md).

Amikor rákattint **webes szolgáltatások beállítása**, események történnek:

* hello betanított modell az átalakított tooa egyetlen **Trained Model** modul és hello modul paletta toohello tárolt bal oldalán hello kísérlet vászonra (található e **betanított modellek**)
* A betanításhoz használt modulok eltávolításuk; pontosabban:
  * [Két osztályú súlyozott döntési fája][two-class-boosted-decision-tree]
  * [Tanítási modell][train-model]
  * [Adatok][split]
  * második hello [R-parancsfájl végrehajtása] [ execute-r-script] a Tesztadatok használt modul
* mentett hello betanított modell vissza hello kísérlet kerül
* **Webalkalmazás-szolgáltatás bemeneti** és **webes szolgáltatás kimeneti** modulok hozzáadásakor (ezeket ahol hello felhasználói adatok módba lép, hello modell és azonosítására milyen adatokat ad vissza, hello webszolgáltatás elérésekor)

> [!NOTE]
> Láthatja, hogy hello kísérlet mentett lapon szerepelnek hello kísérletvászonra hello tetején hozzáadott két részből áll. hello eredeti tanítási kísérletet van hello lapon **tanítási kísérletet**, és az újonnan létrehozott prediktív kísérletté hello alatt áll **prediktív kísérletté**. hello prediktív kísérletté hello egyik azt üzembe webszolgáltatásként.

Igazolnia kell a tootake egy további lépést, az ehhez a kísérlethez.
Két hozzáadott [R-parancsfájl végrehajtása] [ execute-r-script] modulok tooprovide súlyozási toohello adatok működik. Csak a képzési és a teszteléshez, hogy változtatnunk körben, amely volt, azt is vegye ki ilyen modulokhoz hello végső modellben.
A Machine Learning Studio eltávolítani egy [R-parancsfájl végrehajtása] [ execute-r-script] modul hello eltávolításakor [vegyes] [ split] modul. Most már eltávolíthatja azt hello más, és csatlakozzon [metaadat-szerkesztő] [ metadata-editor] közvetlenül túl[Score Model][score-model].    

A kísérletben most példához hasonló:  

![A pontozási hello betanított modell][4]  

> [!NOTE]
> Előfordulhat, hogy lehet szeretné megtudni, hogy miért azt maradtak hello UCI német hitelkártya adatok dataset hello prediktív kísérletté. hello szolgáltatás érintetlen tooscore hello felhasználói adatokat, nem hello eredeti adathalmazból, így miért hagyja hello eredeti adathalmazból hello modell?
> 
> IGAZ, hogy hello szolgáltatást nem kell hello eredeti hitelkártya-adatokhoz. Azonban az adatok, amely tartalmaz információkat, például hogy hány oszlopok vannak, és mely oszlopok numerikus azt kell hello séma. A sémaadatok nem szükséges toointerpret hello felhasználói adatok. Ezeket az összetevőket úgy, hogy hello pontozási modul hello adatkészlet sémája hello szolgáltatás futása közben csatlakozik hagyja azt. hello adatok nem használ, csak hello séma.  
> 
> 

Futtassa egyszer utoljára hello kísérlet (kattintson **futtatása**.) Ha azt szeretné, hogy a modell hello tooverify továbbra is működik, kattintson a hello hello kimenete [Score Model] [ score-model] modul, és válassza ki **eredmények megtekintése**. Láthatja, hogy hello eredeti adatok megjelenik-e, amely hello jóváírás kockázati érték ("pontozott címkék") és hello pontozási valószínűségi érték ("pontozza a mennyiségeket valószínűség".) 

## <a name="deploy-hello-web-service"></a>Hello webszolgáltatás telepítése
Hello kísérlet vagy a klasszikus webes szolgáltatás, vagy egy új webes szolgáltatás, amely alapul, az Azure Resource Manager telepítheti.

### <a name="deploy-as-a-classic-web-service"></a>Egy klasszikus webszolgáltatás telepítése
a kísérletben származó klasszikus webszolgáltatás toodeploy kattintson **webes szolgáltatás telepítése** alatt hello vászonra, és válassza ki **webes szolgáltatás telepítése [klasszikus]**. A Machine Learning Studio hello kísérlet webszolgáltatásként telepíti, és az adott webszolgáltatás toohello irányítópult viszi. Ezen az oldalon bármikor visszatérhet toohello kísérlet (**nézet pillanatkép** vagy **legújabb megtekintése**) és egy egyszerű teszt hello webszolgáltatás (lásd: **hello webszolgáltatás tesztelése** alatt). Alkalmazások, amelyek hello webszolgáltatás (további sablonokat, amelyek a jelen útmutató hello következő lépésben) érhető el itt információkat is van.

![Webes szolgáltatás irányítópult][6]

Hello szolgáltatást konfigurálhatja hello kattintva **konfigurációs** fülre. Itt módosíthatja a hello szolgáltatás nevét (azt az adott hello kísérlet név alapértelmezés szerint), és adjon neki egy leírást. Ezenkívül segítségükkel további rövid címkék hello a bemeneti és kimeneti adatokat.  

![Hello webszolgáltatás konfigurálása][5]  

### <a name="deploy-as-a-new-web-service"></a>Egy új webszolgáltatás-bővítmény telepítése

> [!NOTE] 
> egy megfelelő engedélyekkel kell rendelkeznie a hello előfizetés toowhich telepít új webszolgáltatás-bővítmény toodeploy hello webszolgáltatáshoz. További információkért lásd: [egy webszolgáltatás-bővítmény hello Azure Machine Learning webszolgáltatások portálon kezelheti](machine-learning-manage-new-webservice.md). 

egy új webszolgáltatás-bővítmény toodeploy a kísérletben származó:

1. Kattintson a **webes szolgáltatás telepítése** alatt hello vászonra, és válassza ki **[Új] webes szolgáltatás telepítése**. A Machine Learning Studio átviszi toohello Azure Machine Learning webszolgáltatások **telepítése kísérlet** lap.

2. Adjon meg egy nevet, hello webszolgáltatáshoz. 

3. A **ár terv**, kiválaszthatja, hogy egy meglévő tarifacsomagot, vagy jelölje be "az új létrehozása" és az adott hello új tervezze meg egy nevet és egy select hello havi terv lehetőséget. rétegek alapértelmezett toohello tervek az alapértelmezett területi és a webszolgáltatás hello terv telepített toothat régióban.

4. Kattintson a **telepítése**.

Néhány perc elteltével hello **gyors üzembe helyezés** a webszolgáltatáshoz lap nyílik meg.

Hello szolgáltatást konfigurálhatja hello kattintva **konfigurálása** fülre. Itt módosíthatja a hello szolgáltatás title, és adjon neki egy leírást. 

tootest hello webszolgáltatás, kattintson a hello **teszt** lap (lásd: **hello webszolgáltatás tesztelése** alatt). Létrehozásával kapcsolatos információkat hello webszolgáltatás elérhető alkalmazásokat, kattintson a hello **felhasználás** lap (Ez az útmutató következő lépése hello kerül, részletesebben).

> [!TIP]
> Hello webszolgáltatás telepítése után után frissítheti. Például ha toochange a modellt, majd szerkesztheti hello tanítási kísérletet, végeznünk hello modell paramétereket, és kattintson a **webes szolgáltatás telepítése**, kiválasztásával **webes szolgáltatás telepítése [klasszikus]** vagy  **[Új] webszolgáltatás telepítése**. Hello kísérlet újra telepítésekor hello webszolgáltatás, most már a frissített modellel váltja fel.  
> 
> 

## <a name="test-hello-web-service"></a>Teszt hello webszolgáltatás

Amikor hello webszolgáltatás, hello felhasználói adatok hello keresztül kerül **webszolgáltatás bemenetét** ahol toohello megfelelt modul [Score Model] [ score-model] modul és a program pontozza a mennyiségeket. hello prediktív kísérletté létrehoztunk hogy hello módon hello modellt vár adatok hello hello eredeti jóváírás kockázat dataset formátuma azonos.
hello eredményeinek toohello felhasználói webszolgáltatásból hello keresztül hello **webes szolgáltatás kimeneti** modul.

> [!TIP]
> hello prediktív kísérletté konfigurálva, teljes hello tudunk hello módja annak az eredménye hello [Score Model] [ score-model] modul ad vissza. Ide tartoznak minden hello bemeneti adatok plusz hello jóváírás kockázat értékét és hello pontozási valószínűség. Azonban Visszatérés egy másik, ha azt szeretné,-például visszaadhatja csak hello jóváírás kockázat értékét. toodo, az insert egy [Projektoszlopok] [ project-columns] közötti modul [Score Model] [ score-model] és hello **webes szolgáltatás kimeneti** tooeliminate oszlopok nem szeretné, hogy a webes szolgáltatás tooreturn hello. 
> 
> 

Tesztelheti egy klasszikus webes szolgáltatás, vagy **Machine Learning Studio** vagy hello **Azure Machine Learning webszolgáltatások** portálon.
Csak a hello tesztelheti egy új webszolgáltatás-bővítmény **Machine Learning webszolgáltatások** portálon.

> [!TIP]
> Hello Azure Machine Learning webszolgáltatások portálon tesztelésekor akkor is használható tootest hello kérés-válasz szolgáltatás mintaadatok létrehozása hello portálon. A hello **konfigurálása** lapon, válassza ki a "Yes" a **minta adatok engedélyezve?**. Hello hello kérés-válasz lapján megnyitásakor **teszt** lapon hello portal beírja a mintaadatok adatkészletből hello eredeti jóváírás kockázat venni.

### <a name="test-a-classic-web-service"></a>A klasszikus webszolgáltatás tesztelése

A klasszikus webszolgáltatás tesztelheti a Machine Learning Studióban vagy a hello Machine Learning webszolgáltatások portálon. 

#### <a name="test-in-machine-learning-studio"></a>A Machine Learning Studióban tesztelése

1. A hello **IRÁNYÍTÓPULT** hello webszolgáltatás lapján kattintson a hello **teszt** gombra kattint, a **alapértelmezett végpont**. Egy párbeszédpanel jelenik meg, és kéri hello bemeneti adatok hello szolgáltatás. A rendszer hello hello eredeti jóváírás kockázat dataset megjelent oszlopát.  

2. Adja meg az adatok, és kattintson a **OK**. 

#### <a name="test-in-hello-machine-learning-web-services-portal"></a>A Machine Learning webszolgáltatások portál hello tesztelése

1. A hello **IRÁNYÍTÓPULT** hello webszolgáltatás lapján kattintson a hello **teszt preview** hivatkozásra **alapértelmezett végpont**. hello tesztoldalt hello Azure Machine Learning webszolgáltatások portálon hello webszolgáltatási végpontot megnyílik, és kéri a hello bemeneti adatok hello szolgáltatás. A rendszer hello hello eredeti jóváírás kockázat dataset megjelent oszlopát.

2. Kattintson a **kérés-válasz tesztelése**. 

### <a name="test-a-new-web-service"></a>Egy új webszolgáltatás-bővítmény tesztelése

Csak a hello Machine Learning webszolgáltatások portál egy új webszolgáltatás-bővítmény tesztelheti.

1. A hello [Azure Machine Learning webszolgáltatások](https://services.azureml.net/quickstart) portálon kattintson **teszt** hello oldal hello tetején. Hello **teszt** lap megnyitása és adatok hello szolgáltatás szövegszerkesztőben. hello beviteli mezők jelenik meg hello eredeti jóváírás kockázat dataset megjelent toohello oszlop felel meg. 

2. Adja meg az adatok, és kattintson a **teszt kérés-válasz**.

hello hello teszt eredményei jelennek meg hello jobb oldalán található hello hello kimeneti oszlop. 


## <a name="manage-hello-web-service"></a>Hello webszolgáltatás kezelése

### <a name="manage-a-classic-web-service-in-hello-azure-classic-portal"></a>A klasszikus Azure portálon hello klasszikus webszolgáltatás kezelése

Ha a klasszikus webszolgáltatás telepítése után, kezelheti az hello [a klasszikus Azure portálon](https://manage.windowsazure.com).

1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com)
2. Kattintson a Microsoft Azure szolgáltatások panel hello **gépi tanulás**
3. Kattintson a munkaterület
4. Kattintson a hello **webszolgáltatások** lap
5. Kattintson a létrehozott hello webszolgáltatás
6. Kattintson az "alapértelmezett" végpont hello

Itt például hogyan hello webszolgáltatást tesz a figyelheti, és ellenőrizze a teljesítmény beállításokból állnak kezelni tud a hány egyidejű hívás hello szolgáltatás módosításával.

További információ:

* [Végpontok létrehozása](machine-learning-create-endpoint.md)
* [Méretezési webszolgáltatás](machine-learning-scaling-webservice.md)

### <a name="manage-a-classic-or-new-web-service-in-hello-azure-machine-learning-web-services-portal"></a>A klasszikus vagy új webszolgáltatás hello Azure Machine Learning webszolgáltatások portálon kezelése

Ha a webszolgáltatás telepítése után, hogy klasszikus vagy új, kezelheti az hello [Microsoft Azure Machine Learning webszolgáltatások](https://services.azureml.net/quickstart) portálon.

a webszolgáltatás toomonitor hello teljesítményét:

1. Jelentkezzen be toohello [Microsoft Azure Machine Learning webszolgáltatások](https://services.azureml.net/quickstart) portál
2. Kattintson a **webszolgáltatások**
3. Kattintson a webszolgáltatás
4. Kattintson a hello **irányítópult**

- - -
**Következő: [hello webes szolgáltatás](machine-learning-walkthrough-6-access-web-service.md)**

[3]: ./media/machine-learning-walkthrough-5-publish-web-service/publish3.png
[3a]: ./media/machine-learning-walkthrough-5-publish-web-service/publish3a.png
[4]: ./media/machine-learning-walkthrough-5-publish-web-service/publish4.png
[5]: ./media/machine-learning-walkthrough-5-publish-web-service/publish5.png
[6]: ./media/machine-learning-walkthrough-5-publish-web-service/publish6.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[metadata-editor]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[project-columns]: https://msdn.microsoft.com/en-us/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
