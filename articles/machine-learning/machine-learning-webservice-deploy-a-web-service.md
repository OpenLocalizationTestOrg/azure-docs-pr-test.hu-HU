---
title: "egy új, az Azure Machine Learning webszolgáltatás-bővítmény aaaDeploying |} Microsoft Docs"
description: "hello munkafolyamat üzembe az ARM-alapú webszolgáltatás"
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.assetid: a358b04f-0d08-4d50-820e-eeac971854cf
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2017
ms.author: v-donglo
ROBOTS: NOINDEX
redirect_url: machine-learning-publish-a-machine-learning-web-service
redirect_document_id: True
ms.openlocfilehash: 2cbfda44b97a6b992fbdfdfb0c761e6c9e169035
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-new-web-service"></a><span data-ttu-id="7fd75-103">Új webszolgáltatás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="7fd75-103">Deploy a new web service</span></span>
<span data-ttu-id="7fd75-104">A Microsoft Azure Machine learning most biztosít alapuló webszolgáltatások [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) lehetővé teszi a új számlázási csomag beállítások és a webes szolgáltatás toomultiple régiók telepítése.</span><span class="sxs-lookup"><span data-stu-id="7fd75-104">Microsoft Azure Machine learning now provides web services that are based on [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) allowing for new billing plan options and deploying your web service toomultiple regions.</span></span>

<span data-ttu-id="7fd75-105">hello általános munkafolyamat toodeploy egy webszolgáltatás-bővítmény a Microsoft Azure Machine Learning webszolgáltatások használatával van:</span><span class="sxs-lookup"><span data-stu-id="7fd75-105">hello general workflow toodeploy a web service using Microsoft Azure Machine Learning Web Services is:</span></span>

* <span data-ttu-id="7fd75-106">A prediktív kísérlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="7fd75-106">Create a predictive experiment</span></span>
* <span data-ttu-id="7fd75-107">Telepítés</span><span class="sxs-lookup"><span data-stu-id="7fd75-107">deploy it</span></span>
* <span data-ttu-id="7fd75-108">Konfigurálja a neve</span><span class="sxs-lookup"><span data-stu-id="7fd75-108">configure its name</span></span>
* <span data-ttu-id="7fd75-109">számlázási csomag</span><span class="sxs-lookup"><span data-stu-id="7fd75-109">billing plan</span></span>
* <span data-ttu-id="7fd75-110">Tesztelheti</span><span class="sxs-lookup"><span data-stu-id="7fd75-110">test it</span></span>
* <span data-ttu-id="7fd75-111">ezért használja fel.</span><span class="sxs-lookup"><span data-stu-id="7fd75-111">consume it.</span></span>

<span data-ttu-id="7fd75-112">a következő ábra hello hello munkafolyamat mutatja be.</span><span class="sxs-lookup"><span data-stu-id="7fd75-112">hello following graphic illustrates hello workflow.</span></span>

![Webes szolgáltatás telepítési munkafolyamat][1]

## <a name="deploy-web-service-from-studio"></a><span data-ttu-id="7fd75-114">A Studio webszolgáltatás telepítése</span><span class="sxs-lookup"><span data-stu-id="7fd75-114">Deploy web service from Studio</span></span>
<span data-ttu-id="7fd75-115">kísérlet egy új webszolgáltatás toodeploy.</span><span class="sxs-lookup"><span data-stu-id="7fd75-115">toodeploy an experiment as a new web service.</span></span> <span data-ttu-id="7fd75-116">Jelentkezzen be a Machine Learning Studio hello, és hozzon létre egy új prediktív webszolgáltatás-bővítmény.</span><span class="sxs-lookup"><span data-stu-id="7fd75-116">Sign into hello Machine Learning Studio and create a new predictive web service.</span></span> 

<span data-ttu-id="7fd75-117">**Megjegyzés:**: Ha már telepítette, egy kísérlet egy klasszikus webszolgáltatás nem telepítheti azt egy új webszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="7fd75-117">**Note**: If you have already deployed an experiment as a classic web service you cannot deploy it as a new web service.</span></span>

<span data-ttu-id="7fd75-118">Kattintson a **futtatása** hello hello alján kísérletezhet a vászonra, és kattintson a **webes szolgáltatás telepítése** és **[Új] webes szolgáltatás telepítése**.</span><span class="sxs-lookup"><span data-stu-id="7fd75-118">Click **Run** at hello bottom of hello experiment canvas and then click **Deploy Web Service** and **Deploy Web Service [New]**.</span></span> <span data-ttu-id="7fd75-119">hello Machine Learning webszolgáltatás Manager hello telepítési lap nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="7fd75-119">hello deployment page of hello Machine Learning Web Service manager will open.</span></span>

## <a name="machine-learning-web-service-manager-deploy-experiment-page"></a><span data-ttu-id="7fd75-120">Machine Learning Web Service Manager telepítése kísérlet lap</span><span class="sxs-lookup"><span data-stu-id="7fd75-120">Machine Learning Web Service Manager Deploy Experiment Page</span></span>
<span data-ttu-id="7fd75-121">Hello telepítése kísérlet oldalon adjon meg egy nevet, hello webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="7fd75-121">On hello Deploy Experiment page, enter a name for hello web service.</span></span>
<span data-ttu-id="7fd75-122">Válasszon egy tarifacsomagot.</span><span class="sxs-lookup"><span data-stu-id="7fd75-122">Select a pricing plan.</span></span> <span data-ttu-id="7fd75-123">Rendelkezik egy meglévő tarifacsomagot is kiválaszthatja, ellenkező esetben kell létrehoznia egy új ár terv hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="7fd75-123">If you have an existing pricing plan you can select it, otherwise you must create a new price plan for hello service.</span></span> 

1. <span data-ttu-id="7fd75-124">A hello **ár terv** legördülő listán, jelöljön ki egy meglévő terv vagy hello **jelölje be új csomagot** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="7fd75-124">In hello **Price Plan** drop down, select an existing plan or select hello **Select new plan** option.</span></span>
2. <span data-ttu-id="7fd75-125">A **neve**, adjon meg egy nevet, amely azonosítja a számlán hello terv.</span><span class="sxs-lookup"><span data-stu-id="7fd75-125">In **Plan Name**, type a name that will identify hello plan on your bill.</span></span>
3. <span data-ttu-id="7fd75-126">Válasszon ki egy hello **havi megtervezése rétegek**.</span><span class="sxs-lookup"><span data-stu-id="7fd75-126">Select one of hello **Monthly Plan Tiers**.</span></span> <span data-ttu-id="7fd75-127">Ne feledje, hogy hello terv rétegek alapértelmezett toohello tervek az alapértelmezett régiójában a webszolgáltatás üzembe helyezett toothat régióban.</span><span class="sxs-lookup"><span data-stu-id="7fd75-127">Note that hello plan tiers default toohello plans for your default region and your web service is deployed toothat region.</span></span>

<span data-ttu-id="7fd75-128">Kattintson a **telepítés** és a webszolgáltatás hello gyors üzembe helyezés lap nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="7fd75-128">Click **Deploy** and hello Quickstart page for your web service opens.</span></span>

## <a name="quickstart-page"></a><span data-ttu-id="7fd75-129">Gyors üzembe helyezés lap</span><span class="sxs-lookup"><span data-stu-id="7fd75-129">Quickstart page</span></span>
<span data-ttu-id="7fd75-130">hello webes szolgáltatás gyors üzembe helyezés lap lehetővé teszi az access és útmutatást hello leggyakoribb feladatokat kell végrehajtania egy új webszolgáltatás-bővítmény létrehozása után a.</span><span class="sxs-lookup"><span data-stu-id="7fd75-130">hello web service Quickstart page gives you access and guidance on hello most common tasks you will perform after creating a new web service.</span></span> <span data-ttu-id="7fd75-131">Itt egyszerűen elérhetők mindkét hello **teszt** lap és **felhasználás** lap.</span><span class="sxs-lookup"><span data-stu-id="7fd75-131">From here you can easily access both hello **Test** page and **Consume** page.</span></span>

## <a name="testing-your-web-service"></a><span data-ttu-id="7fd75-132">A webszolgáltatás tesztelése</span><span class="sxs-lookup"><span data-stu-id="7fd75-132">Testing your web service</span></span>
<span data-ttu-id="7fd75-133">Hello gyors üzembe helyezés lapon kattintson a gyakori feladatok teszt webes szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="7fd75-133">From hello Quickstart page, click Test web service under common tasks.</span></span>   

<span data-ttu-id="7fd75-134">tootest hello webszolgáltatás, egy kérés-válasz szolgáltatás (RR-EKET):</span><span class="sxs-lookup"><span data-stu-id="7fd75-134">tootest hello web service as a Request-Response Service (RRS):</span></span>

* <span data-ttu-id="7fd75-135">Kattintson a **teszt** hello menüsávjában.</span><span class="sxs-lookup"><span data-stu-id="7fd75-135">Click **Test** on hello menu bar.</span></span>
* <span data-ttu-id="7fd75-136">Kattintson a **kérés-válasz**.</span><span class="sxs-lookup"><span data-stu-id="7fd75-136">Click **Request-Response**.</span></span>
* <span data-ttu-id="7fd75-137">Adja meg a megfelelő értékeket hello bemeneti oszlopok kísérletbe.</span><span class="sxs-lookup"><span data-stu-id="7fd75-137">Enter appropriate values for hello input columns of your experiment.</span></span>
* <span data-ttu-id="7fd75-138">Kattintson a vizsgálat **kérés-válasz**.</span><span class="sxs-lookup"><span data-stu-id="7fd75-138">Click Test **Request-Response**.</span></span>

<span data-ttu-id="7fd75-139">Hello jobb oldalán található hello megjeleníteni kívánt eredményeket.</span><span class="sxs-lookup"><span data-stu-id="7fd75-139">You results will display on hello right hand side of hello page.</span></span>

<span data-ttu-id="7fd75-140">a kötegelt végrehajtási szolgáltatás (BES) webszolgáltatás tootest, fog használni egy CSV-fájlt:</span><span class="sxs-lookup"><span data-stu-id="7fd75-140">tootest a Batch Execution Service (BES) web service, you will use a CSV file:</span></span>

* <span data-ttu-id="7fd75-141">Kattintson a **teszt** hello menüsávjában.</span><span class="sxs-lookup"><span data-stu-id="7fd75-141">Click **Test** on hello menu bar.</span></span>
* <span data-ttu-id="7fd75-142">Kattintson a **kötegelt**.</span><span class="sxs-lookup"><span data-stu-id="7fd75-142">Click **Batch**.</span></span>
* <span data-ttu-id="7fd75-143">A bemeneti alatt kattintson a Tallózás gombra, és keresse meg a tooyour mintaadatfájlokat.</span><span class="sxs-lookup"><span data-stu-id="7fd75-143">Under your input, click Browse and navigate tooyour sample data file.</span></span>
* <span data-ttu-id="7fd75-144">Kattintson a **teszt**.</span><span class="sxs-lookup"><span data-stu-id="7fd75-144">Click **Test**.</span></span>

<span data-ttu-id="7fd75-145">a teszt hello állapota akkor jelenik meg a **tesztelése kötegelt feladatok**.</span><span class="sxs-lookup"><span data-stu-id="7fd75-145">hello status of your test is displayed under **Test Batch Jobs**.</span></span>

## <a name="consuming-your-web-service"></a><span data-ttu-id="7fd75-146">A webszolgáltatás felhasználása</span><span class="sxs-lookup"><span data-stu-id="7fd75-146">Consuming your Web Service</span></span>
<span data-ttu-id="7fd75-147">Amikor telepít egy webszolgáltatás, az Azure Machine Learning kísérleteket a REST API számos különféle eszközök és rendszerek által felhasználható adja meg.</span><span class="sxs-lookup"><span data-stu-id="7fd75-147">When deployed as a web service, Azure Machine Learning experiments provide a REST API that can be consumed by a wide range of devices and platforms.</span></span> <span data-ttu-id="7fd75-148">Ennek az az oka hello egyszerű REST API fogad el, és válaszol, JSON formátumú üzenetek.</span><span class="sxs-lookup"><span data-stu-id="7fd75-148">This is because hello simple REST API accepts and responds with JSON formatted messages.</span></span> <span data-ttu-id="7fd75-149">hello Azure Machine Learning portal biztosít, amely használható kód toocall hello webszolgáltatás R, C# és Python.</span><span class="sxs-lookup"><span data-stu-id="7fd75-149">hello Azure Machine Learning portal provides code that can be used toocall hello web service in R, C#, and Python.</span></span>

<span data-ttu-id="7fd75-150">Hello felhasználása lapon találja meg:</span><span class="sxs-lookup"><span data-stu-id="7fd75-150">On hello Consuming page you can find:</span></span>

* <span data-ttu-id="7fd75-151">hello API-kulcs és URI-azonosítói webszolgáltatás alkalmazásokban felhasználásához tartozó.</span><span class="sxs-lookup"><span data-stu-id="7fd75-151">hello API key and URI's for consuming web service in apps.</span></span>
* <span data-ttu-id="7fd75-152">Az Excel és a webes alkalmazás sablonok tookick a fogyasztás folyamat elindításához.</span><span class="sxs-lookup"><span data-stu-id="7fd75-152">Excel and web app templates tookick start your consumption process.</span></span>
* <span data-ttu-id="7fd75-153">A C#, python, és R tooget mintakód elindult.</span><span class="sxs-lookup"><span data-stu-id="7fd75-153">Sample code in C#, python, and R tooget you started.</span></span>

<span data-ttu-id="7fd75-154">A webszolgáltatások felhasználása további információkért lásd: [hogyan tooconsume az Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span><span class="sxs-lookup"><span data-stu-id="7fd75-154">For more information on consuming web services, see [How tooconsume an Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7fd75-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7fd75-155">Next Steps</span></span>
<span data-ttu-id="7fd75-156">Webszolgáltatások felhasználása további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="7fd75-156">For more information on consuming web services, see:</span></span>

[<span data-ttu-id="7fd75-157">Hogyan tooconsume az Azure Machine Learning Web service</span><span class="sxs-lookup"><span data-stu-id="7fd75-157">How tooconsume an Azure Machine Learning Web service</span></span>](machine-learning-consume-web-services.md)

[<span data-ttu-id="7fd75-158">Az Azure Machine Learning webszolgáltatások: Telepítés és használat</span><span class="sxs-lookup"><span data-stu-id="7fd75-158">Azure Machine Learning Web Services: Deployment and consumption</span></span>](machine-learning-deploy-consume-web-service-guide.md)

<!--Image references-->
[1]: ./media/machine-learning-webservice-deploy-a-web-service/armdeploymentworkflow.png


<!--links-->
