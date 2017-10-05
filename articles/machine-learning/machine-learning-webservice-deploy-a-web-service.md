---
title: "Egy új, az Azure Machine Learning webszolgáltatás üzembe helyezése |} Microsoft Docs"
description: "A munkafolyamat üzembe az ARM-alapú webszolgáltatás"
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
redirect_document_id: TRUE
ms.openlocfilehash: 1415709f9da2bb2cce859af9feb0ec15c1fa5801
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-a-new-web-service"></a><span data-ttu-id="04dbe-103">Új webszolgáltatás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="04dbe-103">Deploy a new web service</span></span>
<span data-ttu-id="04dbe-104">A Microsoft Azure Machine learning most biztosít alapuló webszolgáltatások [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) lehetővé teszi a új számlázási csomag beállítások és a webszolgáltatás telepítése több területre.</span><span class="sxs-lookup"><span data-stu-id="04dbe-104">Microsoft Azure Machine learning now provides web services that are based on [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) allowing for new billing plan options and deploying your web service to multiple regions.</span></span>

<span data-ttu-id="04dbe-105">Az általános munkafolyamat használatával a Microsoft Azure Machine Learning webszolgáltatások webes szolgáltatás telepítése a következő:</span><span class="sxs-lookup"><span data-stu-id="04dbe-105">The general workflow to deploy a web service using Microsoft Azure Machine Learning Web Services is:</span></span>

* <span data-ttu-id="04dbe-106">A prediktív kísérlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="04dbe-106">Create a predictive experiment</span></span>
* <span data-ttu-id="04dbe-107">Telepítés</span><span class="sxs-lookup"><span data-stu-id="04dbe-107">deploy it</span></span>
* <span data-ttu-id="04dbe-108">Konfigurálja a neve</span><span class="sxs-lookup"><span data-stu-id="04dbe-108">configure its name</span></span>
* <span data-ttu-id="04dbe-109">számlázási csomag</span><span class="sxs-lookup"><span data-stu-id="04dbe-109">billing plan</span></span>
* <span data-ttu-id="04dbe-110">Tesztelheti</span><span class="sxs-lookup"><span data-stu-id="04dbe-110">test it</span></span>
* <span data-ttu-id="04dbe-111">ezért használja fel.</span><span class="sxs-lookup"><span data-stu-id="04dbe-111">consume it.</span></span>

<span data-ttu-id="04dbe-112">Az alábbi ábra mutatja be a munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="04dbe-112">The following graphic illustrates the workflow.</span></span>

![Webes szolgáltatás telepítési munkafolyamat][1]

## <a name="deploy-web-service-from-studio"></a><span data-ttu-id="04dbe-114">A Studio webszolgáltatás telepítése</span><span class="sxs-lookup"><span data-stu-id="04dbe-114">Deploy web service from Studio</span></span>
<span data-ttu-id="04dbe-115">A kísérlet telepítendő új webszolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="04dbe-115">To deploy an experiment as a new web service.</span></span> <span data-ttu-id="04dbe-116">Jelentkezzen be a Machine Learning Studio eszközben, és hozzon létre egy új prediktív webszolgáltatás-bővítmény.</span><span class="sxs-lookup"><span data-stu-id="04dbe-116">Sign into the Machine Learning Studio and create a new predictive web service.</span></span> 

<span data-ttu-id="04dbe-117">**Megjegyzés:**: Ha már telepítette, egy kísérlet egy klasszikus webszolgáltatás nem telepítheti azt egy új webszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="04dbe-117">**Note**: If you have already deployed an experiment as a classic web service you cannot deploy it as a new web service.</span></span>

<span data-ttu-id="04dbe-118">Kattintson a **futtatása** a lap alján a kísérlet vászonra, és kattintson a **webes szolgáltatás telepítése** és **[Új] webes szolgáltatás telepítése**.</span><span class="sxs-lookup"><span data-stu-id="04dbe-118">Click **Run** at the bottom of the experiment canvas and then click **Deploy Web Service** and **Deploy Web Service [New]**.</span></span> <span data-ttu-id="04dbe-119">A Machine Learning webszolgáltatás-kezelő a központi telepítés lap nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="04dbe-119">The deployment page of the Machine Learning Web Service manager will open.</span></span>

## <a name="machine-learning-web-service-manager-deploy-experiment-page"></a><span data-ttu-id="04dbe-120">Machine Learning Web Service Manager telepítése kísérlet lap</span><span class="sxs-lookup"><span data-stu-id="04dbe-120">Machine Learning Web Service Manager Deploy Experiment Page</span></span>
<span data-ttu-id="04dbe-121">A kísérletben telepítése lapon adja meg a webszolgáltatás nevét.</span><span class="sxs-lookup"><span data-stu-id="04dbe-121">On the Deploy Experiment page, enter a name for the web service.</span></span>
<span data-ttu-id="04dbe-122">Válasszon egy tarifacsomagot.</span><span class="sxs-lookup"><span data-stu-id="04dbe-122">Select a pricing plan.</span></span> <span data-ttu-id="04dbe-123">Ha egy meglévő tarifacsomagot is kiválaszthatja, ellenkező esetben kell létrehoznia egy új ár tervet a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="04dbe-123">If you have an existing pricing plan you can select it, otherwise you must create a new price plan for the service.</span></span> 

1. <span data-ttu-id="04dbe-124">Az a **ár terv** legördülő listán, jelöljön ki egy meglévő terv vagy a **jelölje be új csomagot** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="04dbe-124">In the **Price Plan** drop down, select an existing plan or select the **Select new plan** option.</span></span>
2. <span data-ttu-id="04dbe-125">A **neve**, adjon meg egy nevet, amely azonosítja a számlázási csomagot.</span><span class="sxs-lookup"><span data-stu-id="04dbe-125">In **Plan Name**, type a name that will identify the plan on your bill.</span></span>
3. <span data-ttu-id="04dbe-126">Válasszon egyet a a **havi megtervezése rétegek**.</span><span class="sxs-lookup"><span data-stu-id="04dbe-126">Select one of the **Monthly Plan Tiers**.</span></span> <span data-ttu-id="04dbe-127">Vegye figyelembe, hogy a terv rétegek alapértelmezett régiójától és a webes szolgáltatás alapértelmezés szerint a rendszer adott régióban.</span><span class="sxs-lookup"><span data-stu-id="04dbe-127">Note that the plan tiers default to the plans for your default region and your web service is deployed to that region.</span></span>

<span data-ttu-id="04dbe-128">Kattintson a **telepítés** és megnyílik a gyors üzembe helyezés lap a webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="04dbe-128">Click **Deploy** and the Quickstart page for your web service opens.</span></span>

## <a name="quickstart-page"></a><span data-ttu-id="04dbe-129">Gyors üzembe helyezés lap</span><span class="sxs-lookup"><span data-stu-id="04dbe-129">Quickstart page</span></span>
<span data-ttu-id="04dbe-130">A webes szolgáltatás gyors üzembe helyezés lap lehetővé teszi az access és útmutatást a leggyakoribb feladatokat kell végrehajtania egy új webszolgáltatás-bővítmény létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="04dbe-130">The web service Quickstart page gives you access and guidance on the most common tasks you will perform after creating a new web service.</span></span> <span data-ttu-id="04dbe-131">Itt egyszerűen elérhetők mind a **teszt** lap és **felhasználás** lap.</span><span class="sxs-lookup"><span data-stu-id="04dbe-131">From here you can easily access both the **Test** page and **Consume** page.</span></span>

## <a name="testing-your-web-service"></a><span data-ttu-id="04dbe-132">A webszolgáltatás tesztelése</span><span class="sxs-lookup"><span data-stu-id="04dbe-132">Testing your web service</span></span>
<span data-ttu-id="04dbe-133">A gyors üzembe helyezés lapon kattintson a gyakori feladatok teszt webszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="04dbe-133">From the Quickstart page, click Test web service under common tasks.</span></span>   

<span data-ttu-id="04dbe-134">A webes szolgáltatás tesztelése, egy kérés-válasz szolgáltatás (RR-EKET):</span><span class="sxs-lookup"><span data-stu-id="04dbe-134">To test the web service as a Request-Response Service (RRS):</span></span>

* <span data-ttu-id="04dbe-135">Kattintson a **teszt** menüsávjában.</span><span class="sxs-lookup"><span data-stu-id="04dbe-135">Click **Test** on the menu bar.</span></span>
* <span data-ttu-id="04dbe-136">Kattintson a **kérés-válasz**.</span><span class="sxs-lookup"><span data-stu-id="04dbe-136">Click **Request-Response**.</span></span>
* <span data-ttu-id="04dbe-137">Adja meg a bemeneti oszlopok kísérletbe megfelelő értékeket.</span><span class="sxs-lookup"><span data-stu-id="04dbe-137">Enter appropriate values for the input columns of your experiment.</span></span>
* <span data-ttu-id="04dbe-138">Kattintson a vizsgálat **kérés-válasz**.</span><span class="sxs-lookup"><span data-stu-id="04dbe-138">Click Test **Request-Response**.</span></span>

<span data-ttu-id="04dbe-139">A lap jobb oldalán a megjeleníteni kívánt eredményeket.</span><span class="sxs-lookup"><span data-stu-id="04dbe-139">You results will display on the right hand side of the page.</span></span>

<span data-ttu-id="04dbe-140">A kötegelt végrehajtási szolgáltatás (BES) webszolgáltatás teszteléséhez egy CSV-fájlt fogja használni:</span><span class="sxs-lookup"><span data-stu-id="04dbe-140">To test a Batch Execution Service (BES) web service, you will use a CSV file:</span></span>

* <span data-ttu-id="04dbe-141">Kattintson a **teszt** menüsávjában.</span><span class="sxs-lookup"><span data-stu-id="04dbe-141">Click **Test** on the menu bar.</span></span>
* <span data-ttu-id="04dbe-142">Kattintson a **kötegelt**.</span><span class="sxs-lookup"><span data-stu-id="04dbe-142">Click **Batch**.</span></span>
* <span data-ttu-id="04dbe-143">A bemeneti kattintson a Tallózás gombra, és navigáljon a minta adatfájl.</span><span class="sxs-lookup"><span data-stu-id="04dbe-143">Under your input, click Browse and navigate to your sample data file.</span></span>
* <span data-ttu-id="04dbe-144">Kattintson a **teszt**.</span><span class="sxs-lookup"><span data-stu-id="04dbe-144">Click **Test**.</span></span>

<span data-ttu-id="04dbe-145">A vizsgálat állapotának jelenik meg **tesztelése kötegelt feladatok**.</span><span class="sxs-lookup"><span data-stu-id="04dbe-145">The status of your test is displayed under **Test Batch Jobs**.</span></span>

## <a name="consuming-your-web-service"></a><span data-ttu-id="04dbe-146">A webszolgáltatás felhasználása</span><span class="sxs-lookup"><span data-stu-id="04dbe-146">Consuming your Web Service</span></span>
<span data-ttu-id="04dbe-147">Amikor telepít egy webszolgáltatás, az Azure Machine Learning kísérleteket a REST API számos különféle eszközök és rendszerek által felhasználható adja meg.</span><span class="sxs-lookup"><span data-stu-id="04dbe-147">When deployed as a web service, Azure Machine Learning experiments provide a REST API that can be consumed by a wide range of devices and platforms.</span></span> <span data-ttu-id="04dbe-148">Ennek az az oka az egyszerű REST API fogad el, és megválaszolja az JSON formátumú üzenetek.</span><span class="sxs-lookup"><span data-stu-id="04dbe-148">This is because the simple REST API accepts and responds with JSON formatted messages.</span></span> <span data-ttu-id="04dbe-149">Az Azure Machine Learning portal biztosít kódot, amely segítségével az R, C# és Python a webszolgáltatás hívására.</span><span class="sxs-lookup"><span data-stu-id="04dbe-149">The Azure Machine Learning portal provides code that can be used to call the web service in R, C#, and Python.</span></span>

<span data-ttu-id="04dbe-150">A felhasználása oldalon található:</span><span class="sxs-lookup"><span data-stu-id="04dbe-150">On the Consuming page you can find:</span></span>

* <span data-ttu-id="04dbe-151">Az API-kulcs és az URI-azonosítói webszolgáltatás alkalmazásokban felhasználásához tartozó.</span><span class="sxs-lookup"><span data-stu-id="04dbe-151">The API key and URI's for consuming web service in apps.</span></span>
* <span data-ttu-id="04dbe-152">Az Excel és a webes alkalmazás-sablonok indítsa a fogyasztás folyamat elindításához.</span><span class="sxs-lookup"><span data-stu-id="04dbe-152">Excel and web app templates to kick start your consumption process.</span></span>
* <span data-ttu-id="04dbe-153">Példakód C#, python, és R az első lépésekhez.</span><span class="sxs-lookup"><span data-stu-id="04dbe-153">Sample code in C#, python, and R to get you started.</span></span>

<span data-ttu-id="04dbe-154">A webszolgáltatások felhasználása további információkért lásd: [hogyan kell használni az Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span><span class="sxs-lookup"><span data-stu-id="04dbe-154">For more information on consuming web services, see [How to consume an Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="04dbe-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="04dbe-155">Next Steps</span></span>
<span data-ttu-id="04dbe-156">Webszolgáltatások felhasználása további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="04dbe-156">For more information on consuming web services, see:</span></span>

[<span data-ttu-id="04dbe-157">Hogyan kell használni az Azure Machine Learning Web service</span><span class="sxs-lookup"><span data-stu-id="04dbe-157">How to consume an Azure Machine Learning Web service</span></span>](machine-learning-consume-web-services.md)

[<span data-ttu-id="04dbe-158">Az Azure Machine Learning webszolgáltatások: Telepítés és használat</span><span class="sxs-lookup"><span data-stu-id="04dbe-158">Azure Machine Learning Web Services: Deployment and consumption</span></span>](machine-learning-deploy-consume-web-service-guide.md)

<!--Image references-->
[1]: ./media/machine-learning-webservice-deploy-a-web-service/armdeploymentworkflow.png


<!--links-->
