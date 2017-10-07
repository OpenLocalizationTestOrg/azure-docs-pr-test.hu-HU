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
# <a name="walkthrough-step-5-deploy-hello-azure-machine-learning-web-service"></a><span data-ttu-id="c5856-103">Útmutató 5. lépés: Hello Azure Machine Learning webszolgáltatás telepítése</span><span class="sxs-lookup"><span data-stu-id="c5856-103">Walkthrough Step 5: Deploy hello Azure Machine Learning web service</span></span>
<span data-ttu-id="c5856-104">Hello ötödik lépéssel hello forgatókönyv [az Azure Machine Learning a prediktív elemzési megoldás fejlesztése](machine-learning-walkthrough-develop-predictive-solution.md)</span><span class="sxs-lookup"><span data-stu-id="c5856-104">This is hello fifth step of hello walkthrough, [Develop a predictive analytics solution in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span></span>

1. [<span data-ttu-id="c5856-105">Machine Learning-munkaterület létrehozása</span><span class="sxs-lookup"><span data-stu-id="c5856-105">Create a Machine Learning workspace</span></span>](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [<span data-ttu-id="c5856-106">Meglévő adatok feltöltése</span><span class="sxs-lookup"><span data-stu-id="c5856-106">Upload existing data</span></span>](machine-learning-walkthrough-2-upload-data.md)
3. [<span data-ttu-id="c5856-107">Új kísérlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="c5856-107">Create a new experiment</span></span>](machine-learning-walkthrough-3-create-new-experiment.md)
4. [<span data-ttu-id="c5856-108">Betanítása és kiértékelése hello modellek</span><span class="sxs-lookup"><span data-stu-id="c5856-108">Train and evaluate hello models</span></span>](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. <span data-ttu-id="c5856-109">**Hello webszolgáltatás telepítése**</span><span class="sxs-lookup"><span data-stu-id="c5856-109">**Deploy hello web service**</span></span>
6. [<span data-ttu-id="c5856-110">Hello webes szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="c5856-110">Access hello web service</span></span>](machine-learning-walkthrough-6-access-web-service.md)

- - -
<span data-ttu-id="c5856-111">toogive mások egy alkalommal toouse hello a forgatókönyv során korábban kidolgoztunk prediktív modellt, telepíthetünk azt az Azure-on webszolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="c5856-111">toogive others a chance toouse hello predictive model we've developed in this walkthrough, we can deploy it as a web service on Azure.</span></span>

<span data-ttu-id="c5856-112">Toothis pont be azt korábban már ki a modell betanítása.</span><span class="sxs-lookup"><span data-stu-id="c5856-112">Up toothis point we've been experimenting with training our model.</span></span> <span data-ttu-id="c5856-113">Azonban telepített hello szolgáltatás már nem lesz toodo képzési - által hello felhasználói bevitel a modell pontozása érintetlen toogenerate új előrejelzéseket.</span><span class="sxs-lookup"><span data-stu-id="c5856-113">But hello deployed service is no longer going toodo training - it's going toogenerate new predictions by scoring hello user's input based on our model.</span></span> <span data-ttu-id="c5856-114">Így fogjuk toodo néhány előkészítő tooconvert ez kísérletezhet a egy ***képzési*** tooa kísérletezhet ***prediktív*** kipróbálásához.</span><span class="sxs-lookup"><span data-stu-id="c5856-114">So we're going toodo some preparation tooconvert this experiment from a ***training*** experiment tooa ***predictive*** experiment.</span></span> 

<span data-ttu-id="c5856-115">Ez az egy három lépéses folyamat:</span><span class="sxs-lookup"><span data-stu-id="c5856-115">This is a three-step process:</span></span>  

1. <span data-ttu-id="c5856-116">Távolítsa el az egyik hello modell</span><span class="sxs-lookup"><span data-stu-id="c5856-116">Remove one of hello models</span></span>
2. <span data-ttu-id="c5856-117">Átalakítás hello *tanítási kísérletet* történő létrehoztunk Önnek egy *prediktív kísérletté*</span><span class="sxs-lookup"><span data-stu-id="c5856-117">Convert hello *training experiment* we've created into a *predictive experiment*</span></span>
3. <span data-ttu-id="c5856-118">Hello prediktív kísérletté egy webszolgáltatás-bővítmény telepítése</span><span class="sxs-lookup"><span data-stu-id="c5856-118">Deploy hello predictive experiment as a web service</span></span>

## <a name="remove-one-of-hello-models"></a><span data-ttu-id="c5856-119">Távolítsa el az egyik hello modell</span><span class="sxs-lookup"><span data-stu-id="c5856-119">Remove one of hello models</span></span>

<span data-ttu-id="c5856-120">Először létre kell tootrim ehhez a kísérlethez le egy kicsit.</span><span class="sxs-lookup"><span data-stu-id="c5856-120">First, we need tootrim this experiment down a little.</span></span> <span data-ttu-id="c5856-121">A Microsoft jelenleg két különböző modell hello kísérletben, viszont csak szeretnénk toouse egy modell telepítésekor jelenleg ez webszolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="c5856-121">We currently have two different models in hello experiment, but we only want toouse one model when we deploy this as a web service.</span></span>  

<span data-ttu-id="c5856-122">Tegyük fel, azt döntött, hogy súlyozott hello fa modell jobban teljesített, mint hello SVM modell.</span><span class="sxs-lookup"><span data-stu-id="c5856-122">Let's say we've decided that hello boosted tree model performed better than hello SVM model.</span></span> <span data-ttu-id="c5856-123">Így hello első lépésként toodo eltávolítása hello [két osztályú támogatást vektoros gép] [ two-class-support-vector-machine] modul és a képzési azt használt hello modulok.</span><span class="sxs-lookup"><span data-stu-id="c5856-123">So hello first thing toodo is remove hello [Two-Class Support Vector Machine][two-class-support-vector-machine] module and hello modules that were used for training it.</span></span> <span data-ttu-id="c5856-124">Érdemes lehet toomake hello kísérlet másolatát először kattintva **Mentés másként** hello hello alján kísérlet vászonra.</span><span class="sxs-lookup"><span data-stu-id="c5856-124">You may want toomake a copy of hello experiment first by clicking **Save As** at hello bottom of hello experiment canvas.</span></span>

<span data-ttu-id="c5856-125">Igazolnia kell a modulok a következő toodelete hello:</span><span class="sxs-lookup"><span data-stu-id="c5856-125">We need toodelete hello following modules:</span></span>  

* <span data-ttu-id="c5856-126">[Két osztályú támogatást vektoros gép][two-class-support-vector-machine]</span><span class="sxs-lookup"><span data-stu-id="c5856-126">[Two-Class Support Vector Machine][two-class-support-vector-machine]</span></span>
* <span data-ttu-id="c5856-127">[Modell betanításához] [ train-model] és [Score Model] [ score-model] csatlakoztatott tooit volt modulok</span><span class="sxs-lookup"><span data-stu-id="c5856-127">[Train Model][train-model] and [Score Model][score-model] modules that were connected tooit</span></span>
* <span data-ttu-id="c5856-128">[Adatok normalizálása] [ normalize-data] (mindkettő)</span><span class="sxs-lookup"><span data-stu-id="c5856-128">[Normalize Data][normalize-data] (both of them)</span></span>
* <span data-ttu-id="c5856-129">[Értékelje ki a modell] [ evaluate-model] (mert a rendszer végzett hello modell kiértékelése)</span><span class="sxs-lookup"><span data-stu-id="c5856-129">[Evaluate Model][evaluate-model] (because we're finished evaluating hello models)</span></span>

<span data-ttu-id="c5856-130">Válassza ki modulokhoz, és nyomja le az ENTER hello Delete billentyűt, vagy kattintson a jobb gombbal hello modult, majd **törlése**.</span><span class="sxs-lookup"><span data-stu-id="c5856-130">Select each module and press hello Delete key, or right-click hello module and select **Delete**.</span></span> 

![Hello SVM modell eltávolítása][3a]

<span data-ttu-id="c5856-132">A modell most hasonlóan kell kinéznie ezt:</span><span class="sxs-lookup"><span data-stu-id="c5856-132">Our model should now look something like this:</span></span>

![Hello SVM modell eltávolítása][3]

<span data-ttu-id="c5856-134">Most már készen áll a toodeploy el ez a modell használatával hello [két osztályú súlyozott döntési fa][two-class-boosted-decision-tree].</span><span class="sxs-lookup"><span data-stu-id="c5856-134">Now we're ready toodeploy this model using hello [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree].</span></span>

## <a name="convert-hello-training-experiment-tooa-predictive-experiment"></a><span data-ttu-id="c5856-135">Hello tanítási kísérletet tooa prediktív kísérletté átalakítása</span><span class="sxs-lookup"><span data-stu-id="c5856-135">Convert hello training experiment tooa predictive experiment</span></span>

<span data-ttu-id="c5856-136">tooget ez modell kész, telepítési, van erre szükség tooconvert kísérlet tooa prediktív kísérletté betanítása.</span><span class="sxs-lookup"><span data-stu-id="c5856-136">tooget this model ready for deployment, we need tooconvert this training experiment tooa predictive experiment.</span></span> <span data-ttu-id="c5856-137">Ez a három lépést foglal magában:</span><span class="sxs-lookup"><span data-stu-id="c5856-137">This involves three steps:</span></span>

1. <span data-ttu-id="c5856-138">Jelenleg be van tanítva, és akkor cserélje le a képzési modulok hello modell mentése</span><span class="sxs-lookup"><span data-stu-id="c5856-138">Save hello model we've trained and then replace our training modules</span></span>
2. <span data-ttu-id="c5856-139">Trim hello kísérlet tooremove modulok képzési csak szükséges</span><span class="sxs-lookup"><span data-stu-id="c5856-139">Trim hello experiment tooremove modules that were only needed for training</span></span>
3. <span data-ttu-id="c5856-140">Adja meg, ahol hello webszolgáltatás bemenetet fogad, és ahol hello kimeneti generál</span><span class="sxs-lookup"><span data-stu-id="c5856-140">Define where hello web service will accept input and where it generates hello output</span></span>

<span data-ttu-id="c5856-141">Azt megteheti ezt manuálisan, de Szerencsére minden három lépést is elvégezhető kattintva **webes szolgáltatások beállítása** hello kísérletvászonra hello alján (hello választja **prediktív webszolgáltatás** a beállítás).</span><span class="sxs-lookup"><span data-stu-id="c5856-141">We could do this manually, but fortunately all three steps can be accomplished by clicking **Set Up Web Service** at hello bottom of hello experiment canvas (and selecting hello **Predictive Web Service** option).</span></span>

> [!TIP]
> <span data-ttu-id="c5856-142">Ha további részleteket a Mi történik, egy tanítási kísérletet tooa prediktív konvertálásakor kísérletezhet, lásd: [hogyan tooprepare a modell Azure Machine Learning Studio-KözpontiTelepítés](machine-learning-convert-training-experiment-to-scoring-experiment.md).</span><span class="sxs-lookup"><span data-stu-id="c5856-142">If you want more details on what happens when you convert a training experiment tooa predictive experiment, see [How tooprepare your model for deployment in Azure Machine Learning Studio](machine-learning-convert-training-experiment-to-scoring-experiment.md).</span></span>

<span data-ttu-id="c5856-143">Amikor rákattint **webes szolgáltatások beállítása**, események történnek:</span><span class="sxs-lookup"><span data-stu-id="c5856-143">When you click **Set Up Web Service**, several things happen:</span></span>

* <span data-ttu-id="c5856-144">hello betanított modell az átalakított tooa egyetlen **Trained Model** modul és hello modul paletta toohello tárolt bal oldalán hello kísérlet vászonra (található e **betanított modellek**)</span><span class="sxs-lookup"><span data-stu-id="c5856-144">hello trained model is converted tooa single **Trained Model** module and stored in hello module palette toohello left of hello experiment canvas (you can find it under **Trained Models**)</span></span>
* <span data-ttu-id="c5856-145">A betanításhoz használt modulok eltávolításuk; pontosabban:</span><span class="sxs-lookup"><span data-stu-id="c5856-145">Modules that were used for training are removed; specifically:</span></span>
  * <span data-ttu-id="c5856-146">[Két osztályú súlyozott döntési fája][two-class-boosted-decision-tree]</span><span class="sxs-lookup"><span data-stu-id="c5856-146">[Two-Class Boosted Decision Tree][two-class-boosted-decision-tree]</span></span>
  * <span data-ttu-id="c5856-147">[Tanítási modell][train-model]</span><span class="sxs-lookup"><span data-stu-id="c5856-147">[Train Model][train-model]</span></span>
  * <span data-ttu-id="c5856-148">[Adatok][split]</span><span class="sxs-lookup"><span data-stu-id="c5856-148">[Split Data][split]</span></span>
  * <span data-ttu-id="c5856-149">második hello [R-parancsfájl végrehajtása] [ execute-r-script] a Tesztadatok használt modul</span><span class="sxs-lookup"><span data-stu-id="c5856-149">hello second [Execute R Script][execute-r-script] module that was used for test data</span></span>
* <span data-ttu-id="c5856-150">mentett hello betanított modell vissza hello kísérlet kerül</span><span class="sxs-lookup"><span data-stu-id="c5856-150">hello saved trained model is added back into hello experiment</span></span>
* <span data-ttu-id="c5856-151">**Webalkalmazás-szolgáltatás bemeneti** és **webes szolgáltatás kimeneti** modulok hozzáadásakor (ezeket ahol hello felhasználói adatok módba lép, hello modell és azonosítására milyen adatokat ad vissza, hello webszolgáltatás elérésekor)</span><span class="sxs-lookup"><span data-stu-id="c5856-151">**Web service input** and **Web service output** modules are added (these identify where hello user's data will enter hello model, and what data is returned, when hello web service is accessed)</span></span>

> [!NOTE]
> <span data-ttu-id="c5856-152">Láthatja, hogy hello kísérlet mentett lapon szerepelnek hello kísérletvászonra hello tetején hozzáadott két részből áll.</span><span class="sxs-lookup"><span data-stu-id="c5856-152">You can see that hello experiment is saved in two parts under tabs that have been added at hello top of hello experiment canvas.</span></span> <span data-ttu-id="c5856-153">hello eredeti tanítási kísérletet van hello lapon **tanítási kísérletet**, és az újonnan létrehozott prediktív kísérletté hello alatt áll **prediktív kísérletté**.</span><span class="sxs-lookup"><span data-stu-id="c5856-153">hello original training experiment is under hello tab **Training experiment**, and hello newly created predictive experiment is under **Predictive experiment**.</span></span> <span data-ttu-id="c5856-154">hello prediktív kísérletté hello egyik azt üzembe webszolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="c5856-154">hello predictive experiment is hello one we'll deploy as a web service.</span></span>

<span data-ttu-id="c5856-155">Igazolnia kell a tootake egy további lépést, az ehhez a kísérlethez.</span><span class="sxs-lookup"><span data-stu-id="c5856-155">We need tootake one additional step with this particular experiment.</span></span>
<span data-ttu-id="c5856-156">Két hozzáadott [R-parancsfájl végrehajtása] [ execute-r-script] modulok tooprovide súlyozási toohello adatok működik.</span><span class="sxs-lookup"><span data-stu-id="c5856-156">We added two [Execute R Script][execute-r-script] modules tooprovide a weighting function toohello data.</span></span> <span data-ttu-id="c5856-157">Csak a képzési és a teszteléshez, hogy változtatnunk körben, amely volt, azt is vegye ki ilyen modulokhoz hello végső modellben.</span><span class="sxs-lookup"><span data-stu-id="c5856-157">That was just a trick we needed for training and testing, so we can take out those modules in hello final model.</span></span>
<span data-ttu-id="c5856-158">A Machine Learning Studio eltávolítani egy [R-parancsfájl végrehajtása] [ execute-r-script] modul hello eltávolításakor [vegyes] [ split] modul.</span><span class="sxs-lookup"><span data-stu-id="c5856-158">Machine Learning Studio removed one [Execute R Script][execute-r-script] module when it removed hello [Split][split] module.</span></span> <span data-ttu-id="c5856-159">Most már eltávolíthatja azt hello más, és csatlakozzon [metaadat-szerkesztő] [ metadata-editor] közvetlenül túl[Score Model][score-model].</span><span class="sxs-lookup"><span data-stu-id="c5856-159">Now we can remove hello other and connect [Metadata Editor][metadata-editor] directly too[Score Model][score-model].</span></span>    

<span data-ttu-id="c5856-160">A kísérletben most példához hasonló:</span><span class="sxs-lookup"><span data-stu-id="c5856-160">Our experiment should now look like this:</span></span>  

![A pontozási hello betanított modell][4]  

> [!NOTE]
> <span data-ttu-id="c5856-162">Előfordulhat, hogy lehet szeretné megtudni, hogy miért azt maradtak hello UCI német hitelkártya adatok dataset hello prediktív kísérletté.</span><span class="sxs-lookup"><span data-stu-id="c5856-162">You may be wondering why we left hello UCI German Credit Card Data dataset in hello predictive experiment.</span></span> <span data-ttu-id="c5856-163">hello szolgáltatás érintetlen tooscore hello felhasználói adatokat, nem hello eredeti adathalmazból, így miért hagyja hello eredeti adathalmazból hello modell?</span><span class="sxs-lookup"><span data-stu-id="c5856-163">hello service is going tooscore hello user's data, not hello original dataset, so why leave hello original dataset in hello model?</span></span>
> 
> <span data-ttu-id="c5856-164">IGAZ, hogy hello szolgáltatást nem kell hello eredeti hitelkártya-adatokhoz.</span><span class="sxs-lookup"><span data-stu-id="c5856-164">It's true that hello service doesn't need hello original credit card data.</span></span> <span data-ttu-id="c5856-165">Azonban az adatok, amely tartalmaz információkat, például hogy hány oszlopok vannak, és mely oszlopok numerikus azt kell hello séma.</span><span class="sxs-lookup"><span data-stu-id="c5856-165">But it does need hello schema for that data, which includes information such as how many columns there are and which columns are numeric.</span></span> <span data-ttu-id="c5856-166">A sémaadatok nem szükséges toointerpret hello felhasználói adatok.</span><span class="sxs-lookup"><span data-stu-id="c5856-166">This schema information is necessary toointerpret hello user's data.</span></span> <span data-ttu-id="c5856-167">Ezeket az összetevőket úgy, hogy hello pontozási modul hello adatkészlet sémája hello szolgáltatás futása közben csatlakozik hagyja azt.</span><span class="sxs-lookup"><span data-stu-id="c5856-167">We leave these components connected so that hello scoring module has hello dataset schema when hello service is running.</span></span> <span data-ttu-id="c5856-168">hello adatok nem használ, csak hello séma.</span><span class="sxs-lookup"><span data-stu-id="c5856-168">hello data isn't used, just hello schema.</span></span>  
> 
> 

<span data-ttu-id="c5856-169">Futtassa egyszer utoljára hello kísérlet (kattintson **futtatása**.) Ha azt szeretné, hogy a modell hello tooverify továbbra is működik, kattintson a hello hello kimenete [Score Model] [ score-model] modul, és válassza ki **eredmények megtekintése**.</span><span class="sxs-lookup"><span data-stu-id="c5856-169">Run hello experiment one last time (click **Run**.) If you want tooverify that hello model is still working, click hello output of hello [Score Model][score-model] module and select **View Results**.</span></span> <span data-ttu-id="c5856-170">Láthatja, hogy hello eredeti adatok megjelenik-e, amely hello jóváírás kockázati érték ("pontozott címkék") és hello pontozási valószínűségi érték ("pontozza a mennyiségeket valószínűség".)</span><span class="sxs-lookup"><span data-stu-id="c5856-170">You can see that hello original data is displayed, along with hello credit risk value ("Scored Labels") and hello scoring probability value ("Scored Probabilities".)</span></span> 

## <a name="deploy-hello-web-service"></a><span data-ttu-id="c5856-171">Hello webszolgáltatás telepítése</span><span class="sxs-lookup"><span data-stu-id="c5856-171">Deploy hello web service</span></span>
<span data-ttu-id="c5856-172">Hello kísérlet vagy a klasszikus webes szolgáltatás, vagy egy új webes szolgáltatás, amely alapul, az Azure Resource Manager telepítheti.</span><span class="sxs-lookup"><span data-stu-id="c5856-172">You can deploy hello experiment as either a Classic web service, or as a New web service that's based on Azure Resource Manager.</span></span>

### <a name="deploy-as-a-classic-web-service"></a><span data-ttu-id="c5856-173">Egy klasszikus webszolgáltatás telepítése</span><span class="sxs-lookup"><span data-stu-id="c5856-173">Deploy as a Classic web service</span></span>
<span data-ttu-id="c5856-174">a kísérletben származó klasszikus webszolgáltatás toodeploy kattintson **webes szolgáltatás telepítése** alatt hello vászonra, és válassza ki **webes szolgáltatás telepítése [klasszikus]**.</span><span class="sxs-lookup"><span data-stu-id="c5856-174">toodeploy a Classic web service derived from our experiment, click **Deploy Web Service** below hello canvas and select **Deploy Web Service [Classic]**.</span></span> <span data-ttu-id="c5856-175">A Machine Learning Studio hello kísérlet webszolgáltatásként telepíti, és az adott webszolgáltatás toohello irányítópult viszi.</span><span class="sxs-lookup"><span data-stu-id="c5856-175">Machine Learning Studio deploys hello experiment as a web service and takes you toohello dashboard for that web service.</span></span> <span data-ttu-id="c5856-176">Ezen az oldalon bármikor visszatérhet toohello kísérlet (**nézet pillanatkép** vagy **legújabb megtekintése**) és egy egyszerű teszt hello webszolgáltatás (lásd: **hello webszolgáltatás tesztelése** alatt).</span><span class="sxs-lookup"><span data-stu-id="c5856-176">From this page you can return toohello experiment (**View snapshot** or **View latest**) and run a simple test of hello web service (see **Test hello web service** below).</span></span> <span data-ttu-id="c5856-177">Alkalmazások, amelyek hello webszolgáltatás (további sablonokat, amelyek a jelen útmutató hello következő lépésben) érhető el itt információkat is van.</span><span class="sxs-lookup"><span data-stu-id="c5856-177">There is also information here for creating applications that can access hello web service (more on that in hello next step of this walkthrough).</span></span>

![Webes szolgáltatás irányítópult][6]

<span data-ttu-id="c5856-179">Hello szolgáltatást konfigurálhatja hello kattintva **konfigurációs** fülre. Itt módosíthatja a hello szolgáltatás nevét (azt az adott hello kísérlet név alapértelmezés szerint), és adjon neki egy leírást.</span><span class="sxs-lookup"><span data-stu-id="c5856-179">You can configure hello service by clicking hello **CONFIGURATION** tab. Here you can modify hello service name (it's given hello experiment name by default) and give it a description.</span></span> <span data-ttu-id="c5856-180">Ezenkívül segítségükkel további rövid címkék hello a bemeneti és kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="c5856-180">You can also give more friendly labels for hello input and output data.</span></span>  

![Hello webszolgáltatás konfigurálása][5]  

### <a name="deploy-as-a-new-web-service"></a><span data-ttu-id="c5856-182">Egy új webszolgáltatás-bővítmény telepítése</span><span class="sxs-lookup"><span data-stu-id="c5856-182">Deploy as a New web service</span></span>

> [!NOTE] 
> <span data-ttu-id="c5856-183">egy megfelelő engedélyekkel kell rendelkeznie a hello előfizetés toowhich telepít új webszolgáltatás-bővítmény toodeploy hello webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="c5856-183">toodeploy a New web service you must have sufficient permissions in hello subscription toowhich you are deploying hello web service.</span></span> <span data-ttu-id="c5856-184">További információkért lásd: [egy webszolgáltatás-bővítmény hello Azure Machine Learning webszolgáltatások portálon kezelheti](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="c5856-184">For more information, see [Manage a web service using hello Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

<span data-ttu-id="c5856-185">egy új webszolgáltatás-bővítmény toodeploy a kísérletben származó:</span><span class="sxs-lookup"><span data-stu-id="c5856-185">toodeploy a New web service derived from our experiment:</span></span>

1. <span data-ttu-id="c5856-186">Kattintson a **webes szolgáltatás telepítése** alatt hello vászonra, és válassza ki **[Új] webes szolgáltatás telepítése**.</span><span class="sxs-lookup"><span data-stu-id="c5856-186">Click **Deploy Web Service** below hello canvas and select **Deploy Web Service [New]**.</span></span> <span data-ttu-id="c5856-187">A Machine Learning Studio átviszi toohello Azure Machine Learning webszolgáltatások **telepítése kísérlet** lap.</span><span class="sxs-lookup"><span data-stu-id="c5856-187">Machine Learning Studio transfers you toohello Azure Machine Learning web services **Deploy Experiment** page.</span></span>

2. <span data-ttu-id="c5856-188">Adjon meg egy nevet, hello webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="c5856-188">Enter a name for hello web service.</span></span> 

3. <span data-ttu-id="c5856-189">A **ár terv**, kiválaszthatja, hogy egy meglévő tarifacsomagot, vagy jelölje be "az új létrehozása" és az adott hello új tervezze meg egy nevet és egy select hello havi terv lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="c5856-189">For **Price Plan**, you can select an existing pricing plan, or select "Create new" and give hello new plan a name and select hello monthly plan option.</span></span> <span data-ttu-id="c5856-190">rétegek alapértelmezett toohello tervek az alapértelmezett területi és a webszolgáltatás hello terv telepített toothat régióban.</span><span class="sxs-lookup"><span data-stu-id="c5856-190">hello plan tiers default toohello plans for your default region and your web service is deployed toothat region.</span></span>

4. <span data-ttu-id="c5856-191">Kattintson a **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="c5856-191">Click **Deploy**.</span></span>

<span data-ttu-id="c5856-192">Néhány perc elteltével hello **gyors üzembe helyezés** a webszolgáltatáshoz lap nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="c5856-192">After a few minutes, hello **Quickstart** page for your web service opens.</span></span>

<span data-ttu-id="c5856-193">Hello szolgáltatást konfigurálhatja hello kattintva **konfigurálása** fülre. Itt módosíthatja a hello szolgáltatás title, és adjon neki egy leírást.</span><span class="sxs-lookup"><span data-stu-id="c5856-193">You can configure hello service by clicking hello **Configure** tab. Here you can modify hello service title and give it a description.</span></span> 

<span data-ttu-id="c5856-194">tootest hello webszolgáltatás, kattintson a hello **teszt** lap (lásd: **hello webszolgáltatás tesztelése** alatt).</span><span class="sxs-lookup"><span data-stu-id="c5856-194">tootest hello web service, click hello **Test** tab (see **Test hello web service** below).</span></span> <span data-ttu-id="c5856-195">Létrehozásával kapcsolatos információkat hello webszolgáltatás elérhető alkalmazásokat, kattintson a hello **felhasználás** lap (Ez az útmutató következő lépése hello kerül, részletesebben).</span><span class="sxs-lookup"><span data-stu-id="c5856-195">For information on creating applications that can access hello web service, click hello **Consume** tab (hello next step in this walkthrough will go into more detail).</span></span>

> [!TIP]
> <span data-ttu-id="c5856-196">Hello webszolgáltatás telepítése után után frissítheti.</span><span class="sxs-lookup"><span data-stu-id="c5856-196">You can update hello web service after you've deployed it.</span></span> <span data-ttu-id="c5856-197">Például ha toochange a modellt, majd szerkesztheti hello tanítási kísérletet, végeznünk hello modell paramétereket, és kattintson a **webes szolgáltatás telepítése**, kiválasztásával **webes szolgáltatás telepítése [klasszikus]** vagy  **[Új] webszolgáltatás telepítése**.</span><span class="sxs-lookup"><span data-stu-id="c5856-197">For example, if you want toochange your model, then you can edit hello training experiment, tweak hello model parameters, and click **Deploy Web Service**, selecting **Deploy Web Service [Classic]** or **Deploy Web Service [New]**.</span></span> <span data-ttu-id="c5856-198">Hello kísérlet újra telepítésekor hello webszolgáltatás, most már a frissített modellel váltja fel.</span><span class="sxs-lookup"><span data-stu-id="c5856-198">When you deploy hello experiment again, it replaces hello web service, now using your updated model.</span></span>  
> 
> 

## <a name="test-hello-web-service"></a><span data-ttu-id="c5856-199">Teszt hello webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="c5856-199">Test hello web service</span></span>

<span data-ttu-id="c5856-200">Amikor hello webszolgáltatás, hello felhasználói adatok hello keresztül kerül **webszolgáltatás bemenetét** ahol toohello megfelelt modul [Score Model] [ score-model] modul és a program pontozza a mennyiségeket.</span><span class="sxs-lookup"><span data-stu-id="c5856-200">When hello web service is accessed, hello user's data enters through hello **Web service input** module where it's passed toohello [Score Model][score-model] module and scored.</span></span> <span data-ttu-id="c5856-201">hello prediktív kísérletté létrehoztunk hogy hello módon hello modellt vár adatok hello hello eredeti jóváírás kockázat dataset formátuma azonos.</span><span class="sxs-lookup"><span data-stu-id="c5856-201">hello way we've set up hello predictive experiment, hello model expects data in hello same format as hello original credit risk dataset.</span></span>
<span data-ttu-id="c5856-202">hello eredményeinek toohello felhasználói webszolgáltatásból hello keresztül hello **webes szolgáltatás kimeneti** modul.</span><span class="sxs-lookup"><span data-stu-id="c5856-202">hello results are returned toohello user from hello web service through hello **Web service output** module.</span></span>

> [!TIP]
> <span data-ttu-id="c5856-203">hello prediktív kísérletté konfigurálva, teljes hello tudunk hello módja annak az eredménye hello [Score Model] [ score-model] modul ad vissza.</span><span class="sxs-lookup"><span data-stu-id="c5856-203">hello way we have hello predictive experiment configured, hello entire results from hello [Score Model][score-model] module are returned.</span></span> <span data-ttu-id="c5856-204">Ide tartoznak minden hello bemeneti adatok plusz hello jóváírás kockázat értékét és hello pontozási valószínűség.</span><span class="sxs-lookup"><span data-stu-id="c5856-204">This includes all hello input data plus hello credit risk value and hello scoring probability.</span></span> <span data-ttu-id="c5856-205">Azonban Visszatérés egy másik, ha azt szeretné,-például visszaadhatja csak hello jóváírás kockázat értékét.</span><span class="sxs-lookup"><span data-stu-id="c5856-205">But you can return something different if you want - for example, you could return just hello credit risk value.</span></span> <span data-ttu-id="c5856-206">toodo, az insert egy [Projektoszlopok] [ project-columns] közötti modul [Score Model] [ score-model] és hello **webes szolgáltatás kimeneti** tooeliminate oszlopok nem szeretné, hogy a webes szolgáltatás tooreturn hello.</span><span class="sxs-lookup"><span data-stu-id="c5856-206">toodo this, insert a [Project Columns][project-columns] module between [Score Model][score-model] and hello **Web service output** tooeliminate columns you don't want hello web service tooreturn.</span></span> 
> 
> 

<span data-ttu-id="c5856-207">Tesztelheti egy klasszikus webes szolgáltatás, vagy **Machine Learning Studio** vagy hello **Azure Machine Learning webszolgáltatások** portálon.</span><span class="sxs-lookup"><span data-stu-id="c5856-207">You can test a Classic web service either in **Machine Learning Studio** or in hello **Azure Machine Learning Web Services** portal.</span></span>
<span data-ttu-id="c5856-208">Csak a hello tesztelheti egy új webszolgáltatás-bővítmény **Machine Learning webszolgáltatások** portálon.</span><span class="sxs-lookup"><span data-stu-id="c5856-208">You can test a New web service only in hello **Machine Learning Web Services** portal.</span></span>

> [!TIP]
> <span data-ttu-id="c5856-209">Hello Azure Machine Learning webszolgáltatások portálon tesztelésekor akkor is használható tootest hello kérés-válasz szolgáltatás mintaadatok létrehozása hello portálon.</span><span class="sxs-lookup"><span data-stu-id="c5856-209">When testing in hello Azure Machine Learning Web Services portal, you can have hello portal create sample data that you can use tootest hello Request-Response service.</span></span> <span data-ttu-id="c5856-210">A hello **konfigurálása** lapon, válassza ki a "Yes" a **minta adatok engedélyezve?**.</span><span class="sxs-lookup"><span data-stu-id="c5856-210">On hello **Configure** page, select "Yes" for **Sample Data Enabled?**.</span></span> <span data-ttu-id="c5856-211">Hello hello kérés-válasz lapján megnyitásakor **teszt** lapon hello portal beírja a mintaadatok adatkészletből hello eredeti jóváírás kockázat venni.</span><span class="sxs-lookup"><span data-stu-id="c5856-211">When you open hello Request-Response tab on hello **Test** page, hello portal fills in sample data taken from hello original credit risk dataset.</span></span>

### <a name="test-a-classic-web-service"></a><span data-ttu-id="c5856-212">A klasszikus webszolgáltatás tesztelése</span><span class="sxs-lookup"><span data-stu-id="c5856-212">Test a Classic web service</span></span>

<span data-ttu-id="c5856-213">A klasszikus webszolgáltatás tesztelheti a Machine Learning Studióban vagy a hello Machine Learning webszolgáltatások portálon.</span><span class="sxs-lookup"><span data-stu-id="c5856-213">You can test a Classic web service in Machine Learning Studio or in hello Machine Learning Web Services portal.</span></span> 

#### <a name="test-in-machine-learning-studio"></a><span data-ttu-id="c5856-214">A Machine Learning Studióban tesztelése</span><span class="sxs-lookup"><span data-stu-id="c5856-214">Test in Machine Learning Studio</span></span>

1. <span data-ttu-id="c5856-215">A hello **IRÁNYÍTÓPULT** hello webszolgáltatás lapján kattintson a hello **teszt** gombra kattint, a **alapértelmezett végpont**.</span><span class="sxs-lookup"><span data-stu-id="c5856-215">On hello **DASHBOARD** page for hello web service, click hello **Test** button under **Default Endpoint**.</span></span> <span data-ttu-id="c5856-216">Egy párbeszédpanel jelenik meg, és kéri hello bemeneti adatok hello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c5856-216">A dialog pops up and asks you for hello input data for hello service.</span></span> <span data-ttu-id="c5856-217">A rendszer hello hello eredeti jóváírás kockázat dataset megjelent oszlopát.</span><span class="sxs-lookup"><span data-stu-id="c5856-217">These are hello same columns that appeared in hello original credit risk dataset.</span></span>  

2. <span data-ttu-id="c5856-218">Adja meg az adatok, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="c5856-218">Enter a set of data and then click **OK**.</span></span> 

#### <a name="test-in-hello-machine-learning-web-services-portal"></a><span data-ttu-id="c5856-219">A Machine Learning webszolgáltatások portál hello tesztelése</span><span class="sxs-lookup"><span data-stu-id="c5856-219">Test in hello Machine Learning Web Services portal</span></span>

1. <span data-ttu-id="c5856-220">A hello **IRÁNYÍTÓPULT** hello webszolgáltatás lapján kattintson a hello **teszt preview** hivatkozásra **alapértelmezett végpont**.</span><span class="sxs-lookup"><span data-stu-id="c5856-220">On hello **DASHBOARD** page for hello web service, click hello **Test preview** link under **Default Endpoint**.</span></span> <span data-ttu-id="c5856-221">hello tesztoldalt hello Azure Machine Learning webszolgáltatások portálon hello webszolgáltatási végpontot megnyílik, és kéri a hello bemeneti adatok hello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c5856-221">hello test page in hello Azure Machine Learning Web Services portal for hello web service endpoint opens and asks you for hello input data for hello service.</span></span> <span data-ttu-id="c5856-222">A rendszer hello hello eredeti jóváírás kockázat dataset megjelent oszlopát.</span><span class="sxs-lookup"><span data-stu-id="c5856-222">These are hello same columns that appeared in hello original credit risk dataset.</span></span>

2. <span data-ttu-id="c5856-223">Kattintson a **kérés-válasz tesztelése**.</span><span class="sxs-lookup"><span data-stu-id="c5856-223">Click **Test Request-Response**.</span></span> 

### <a name="test-a-new-web-service"></a><span data-ttu-id="c5856-224">Egy új webszolgáltatás-bővítmény tesztelése</span><span class="sxs-lookup"><span data-stu-id="c5856-224">Test a New web service</span></span>

<span data-ttu-id="c5856-225">Csak a hello Machine Learning webszolgáltatások portál egy új webszolgáltatás-bővítmény tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="c5856-225">You can test a New web service only in hello Machine Learning Web Services portal.</span></span>

1. <span data-ttu-id="c5856-226">A hello [Azure Machine Learning webszolgáltatások](https://services.azureml.net/quickstart) portálon kattintson **teszt** hello oldal hello tetején.</span><span class="sxs-lookup"><span data-stu-id="c5856-226">In hello [Azure Machine Learning Web Services](https://services.azureml.net/quickstart) portal, click **Test** at hello top of hello page.</span></span> <span data-ttu-id="c5856-227">Hello **teszt** lap megnyitása és adatok hello szolgáltatás szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="c5856-227">hello **Test** page opens and you can input data for hello service.</span></span> <span data-ttu-id="c5856-228">hello beviteli mezők jelenik meg hello eredeti jóváírás kockázat dataset megjelent toohello oszlop felel meg.</span><span class="sxs-lookup"><span data-stu-id="c5856-228">hello input fields displayed correspond toohello columns that appeared in hello original credit risk dataset.</span></span> 

2. <span data-ttu-id="c5856-229">Adja meg az adatok, és kattintson a **teszt kérés-válasz**.</span><span class="sxs-lookup"><span data-stu-id="c5856-229">Enter a set of data and then click **Test Request-Response**.</span></span>

<span data-ttu-id="c5856-230">hello hello teszt eredményei jelennek meg hello jobb oldalán található hello hello kimeneti oszlop.</span><span class="sxs-lookup"><span data-stu-id="c5856-230">hello results of hello test are displayed on hello right-hand side of hello page in hello output column.</span></span> 


## <a name="manage-hello-web-service"></a><span data-ttu-id="c5856-231">Hello webszolgáltatás kezelése</span><span class="sxs-lookup"><span data-stu-id="c5856-231">Manage hello web service</span></span>

### <a name="manage-a-classic-web-service-in-hello-azure-classic-portal"></a><span data-ttu-id="c5856-232">A klasszikus Azure portálon hello klasszikus webszolgáltatás kezelése</span><span class="sxs-lookup"><span data-stu-id="c5856-232">Manage a Classic web service in hello Azure classic portal</span></span>

<span data-ttu-id="c5856-233">Ha a klasszikus webszolgáltatás telepítése után, kezelheti az hello [a klasszikus Azure portálon](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="c5856-233">Once you've deployed your Classic web service, you can manage it from hello [Azure classic portal](https://manage.windowsazure.com).</span></span>

1. <span data-ttu-id="c5856-234">Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com)</span><span class="sxs-lookup"><span data-stu-id="c5856-234">Sign in toohello [Azure classic portal](https://manage.windowsazure.com)</span></span>
2. <span data-ttu-id="c5856-235">Kattintson a Microsoft Azure szolgáltatások panel hello **gépi tanulás**</span><span class="sxs-lookup"><span data-stu-id="c5856-235">In hello Microsoft Azure services panel, click **MACHINE LEARNING**</span></span>
3. <span data-ttu-id="c5856-236">Kattintson a munkaterület</span><span class="sxs-lookup"><span data-stu-id="c5856-236">Click your workspace</span></span>
4. <span data-ttu-id="c5856-237">Kattintson a hello **webszolgáltatások** lap</span><span class="sxs-lookup"><span data-stu-id="c5856-237">Click hello **Web services** tab</span></span>
5. <span data-ttu-id="c5856-238">Kattintson a létrehozott hello webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="c5856-238">Click hello web service we created</span></span>
6. <span data-ttu-id="c5856-239">Kattintson az "alapértelmezett" végpont hello</span><span class="sxs-lookup"><span data-stu-id="c5856-239">Click hello "default" endpoint</span></span>

<span data-ttu-id="c5856-240">Itt például hogyan hello webszolgáltatást tesz a figyelheti, és ellenőrizze a teljesítmény beállításokból állnak kezelni tud a hány egyidejű hívás hello szolgáltatás módosításával.</span><span class="sxs-lookup"><span data-stu-id="c5856-240">From here, you can do things like monitor how hello web service is doing and make performance tweaks by changing how many concurrent calls hello service can handle.</span></span>

<span data-ttu-id="c5856-241">További információ:</span><span class="sxs-lookup"><span data-stu-id="c5856-241">For more details, see:</span></span>

* [<span data-ttu-id="c5856-242">Végpontok létrehozása</span><span class="sxs-lookup"><span data-stu-id="c5856-242">Creating Endpoints</span></span>](machine-learning-create-endpoint.md)
* [<span data-ttu-id="c5856-243">Méretezési webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="c5856-243">Scaling web service</span></span>](machine-learning-scaling-webservice.md)

### <a name="manage-a-classic-or-new-web-service-in-hello-azure-machine-learning-web-services-portal"></a><span data-ttu-id="c5856-244">A klasszikus vagy új webszolgáltatás hello Azure Machine Learning webszolgáltatások portálon kezelése</span><span class="sxs-lookup"><span data-stu-id="c5856-244">Manage a Classic or New web service in hello Azure Machine Learning Web Services portal</span></span>

<span data-ttu-id="c5856-245">Ha a webszolgáltatás telepítése után, hogy klasszikus vagy új, kezelheti az hello [Microsoft Azure Machine Learning webszolgáltatások](https://services.azureml.net/quickstart) portálon.</span><span class="sxs-lookup"><span data-stu-id="c5856-245">Once you've deployed your web service, whether Classic or New, you can manage it from hello [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/quickstart) portal.</span></span>

<span data-ttu-id="c5856-246">a webszolgáltatás toomonitor hello teljesítményét:</span><span class="sxs-lookup"><span data-stu-id="c5856-246">toomonitor hello performance of your web service:</span></span>

1. <span data-ttu-id="c5856-247">Jelentkezzen be toohello [Microsoft Azure Machine Learning webszolgáltatások](https://services.azureml.net/quickstart) portál</span><span class="sxs-lookup"><span data-stu-id="c5856-247">Sign in toohello [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/quickstart) portal</span></span>
2. <span data-ttu-id="c5856-248">Kattintson a **webszolgáltatások**</span><span class="sxs-lookup"><span data-stu-id="c5856-248">Click **Web services**</span></span>
3. <span data-ttu-id="c5856-249">Kattintson a webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="c5856-249">Click your web service</span></span>
4. <span data-ttu-id="c5856-250">Kattintson a hello **irányítópult**</span><span class="sxs-lookup"><span data-stu-id="c5856-250">Click hello **Dashboard**</span></span>

- - -
<span data-ttu-id="c5856-251">**Következő: [hello webes szolgáltatás](machine-learning-walkthrough-6-access-web-service.md)**</span><span class="sxs-lookup"><span data-stu-id="c5856-251">**Next: [Access hello web service](machine-learning-walkthrough-6-access-web-service.md)**</span></span>

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
