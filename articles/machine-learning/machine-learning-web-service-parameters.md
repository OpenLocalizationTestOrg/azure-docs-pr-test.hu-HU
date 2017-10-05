---
title: "Használja az Azure Machine Learning webszolgáltatás-paramétereket |} Microsoft Docs"
description: "Hogyan használható az Azure Machine Learning webszolgáltatás-paramétereket a webszolgáltatás elérésekor a modell működésének módosítása céljából."
services: machine-learning
documentationcenter: 
author: raymondlaghaeian
manager: jhubbard
editor: cgronlun
ms.assetid: c49187db-b976-4731-89d6-11a0bf653db1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/12/2017
ms.author: raymondl;garye
ms.openlocfilehash: 482726c1dae5385964e08b720e529817d5907537
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-machine-learning-web-service-parameters"></a><span data-ttu-id="43914-103">Az Azure Machine Learning webszolgáltatás paramétereinek használata</span><span class="sxs-lookup"><span data-stu-id="43914-103">Use Azure Machine Learning Web Service Parameters</span></span>
<span data-ttu-id="43914-104">Az Azure Machine Learning webszolgáltatás közzététele, amely konfigurálható paraméterekkel lehetővé tevő modulokat tartalmaz a kísérlet hozta létre.</span><span class="sxs-lookup"><span data-stu-id="43914-104">An Azure Machine Learning web service is created by publishing an experiment that contains modules with configurable parameters.</span></span> <span data-ttu-id="43914-105">Bizonyos esetekben érdemes lehet a modul megváltozzon, miközben fut a webes szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="43914-105">In some cases, you may want to change the module behavior while the web service is running.</span></span> <span data-ttu-id="43914-106">*Webszolgáltatási paramétereket* engedélyezi, hogy a feladat végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="43914-106">*Web Service Parameters* allow you to do this task.</span></span> 

<span data-ttu-id="43914-107">Ilyenek például beállítását a [és adatokat importálhat] [ reader] modul, hogy a felhasználó a közzétett webes szolgáltatás adhat meg egy másik adatforráshoz, a webszolgáltatás elérésekor.</span><span class="sxs-lookup"><span data-stu-id="43914-107">A common example is setting up the [Import Data][reader] module so that the user of the published web service can specify a different data source when the web service is accessed.</span></span> <span data-ttu-id="43914-108">Vagy konfigurálása a [adatok exportálása] [ writer] modul, hogy egy másik cél adható meg.</span><span class="sxs-lookup"><span data-stu-id="43914-108">Or configuring the [Export Data][writer] module so that a different destination can be specified.</span></span> <span data-ttu-id="43914-109">Más például bitjei számának módosítása a [Szolgáltatáskivonatolás] [ feature-hashing] modul vagy száma szükséges szolgáltatásokat a [szűrő-alapú szolgáltatás kiválasztása] [ filter-based-feature-selection] modul.</span><span class="sxs-lookup"><span data-stu-id="43914-109">Some other examples include changing the number of bits for the [Feature Hashing][feature-hashing] module or the number of desired features for the [Filter-Based Feature Selection][filter-based-feature-selection] module.</span></span> 

<span data-ttu-id="43914-110">Állítsa be a webszolgáltatás-paramétereket, és rendelje hozzá őket egy vagy több modulja paraméter a kísérletben, és megadhatja, hogy azok kötelező vagy választható.</span><span class="sxs-lookup"><span data-stu-id="43914-110">You can set Web Service Parameters and associate them with one or more module parameters in your experiment, and you can specify whether they are required or optional.</span></span> <span data-ttu-id="43914-111">A felhasználó a webszolgáltatás lehet majd adja meg ezeket a paramétereket, amikor azok a webszolgáltatás hívására.</span><span class="sxs-lookup"><span data-stu-id="43914-111">The user of the web service can then provide values for these parameters when they call the web service.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="how-to-set-and-use-web-service-parameters"></a><span data-ttu-id="43914-112">Állítsa be, és webszolgáltatás-paramétereket használata</span><span class="sxs-lookup"><span data-stu-id="43914-112">How to set and use Web Service Parameters</span></span>
<span data-ttu-id="43914-113">A webszolgáltatási paraméter a paraméter egy modul melletti ikonra kattint, majd válassza a "Web service paraméter beállítása" határozza meg.</span><span class="sxs-lookup"><span data-stu-id="43914-113">You define a Web Service Parameter by clicking the icon next to the parameter for a module and selecting "Set as web service parameter".</span></span> <span data-ttu-id="43914-114">Ez létrehoz egy új webszolgáltatási paraméter, és kapcsolódik az adott modul paraméter.</span><span class="sxs-lookup"><span data-stu-id="43914-114">This creates a new Web Service Parameter and connects it to that module parameter.</span></span> <span data-ttu-id="43914-115">Majd amikor a webes szolgáltatás, a felhasználó érték adható meg a webszolgáltatási paraméter, illetve az alkalmazásának a modul paraméterhez.</span><span class="sxs-lookup"><span data-stu-id="43914-115">Then, when the web service is accessed, the user can specify a value for the Web Service Parameter and it is applied to the module parameter.</span></span>

<span data-ttu-id="43914-116">A webszolgáltatási paraméter határozza meg, miután a rendelkezésére áll a kísérletben más modul paraméter.</span><span class="sxs-lookup"><span data-stu-id="43914-116">Once you define a Web Service Parameter, it's available to any other module parameter in the experiment.</span></span> <span data-ttu-id="43914-117">Egy webszolgáltatási egy modul egy paraméterének társított paraméter megadása esetén használható, hogy ugyanazon webszolgáltatási paraméter bármely más modul mindaddig, amíg a paraméter a ugyanolyan típusú értéket vár.</span><span class="sxs-lookup"><span data-stu-id="43914-117">If you define a Web Service Parameter associated with a parameter for one module, you can use that same Web Service Parameter for any other module, as long as the parameter expects the same type of value.</span></span> <span data-ttu-id="43914-118">Például ha a webszolgáltatási paraméter egy numerikus értéket, majd csak használat modul paraméter, amely egy numerikus értéket várt.</span><span class="sxs-lookup"><span data-stu-id="43914-118">For example, if the Web Service Parameter is a numeric value, then it can only be used for module parameters that expect a numeric value.</span></span> <span data-ttu-id="43914-119">A felhasználó a webes szolgáltatás paraméter beállít egy értéket, akkor lépnek érvénybe az összes társított modul paraméterei.</span><span class="sxs-lookup"><span data-stu-id="43914-119">When the user sets a value for the Web Service Parameter, it will be applied to all associated module parameters.</span></span>

<span data-ttu-id="43914-120">Eldöntheti, hogy a webes paraméter alapértelmezett értéket adjon meg.</span><span class="sxs-lookup"><span data-stu-id="43914-120">You can decide whether to provide a default value for the Web Service Parameter.</span></span> <span data-ttu-id="43914-121">Ha így tesz, a paraméter megadása esetén a felhasználó a webszolgáltatás nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="43914-121">If you do, then the parameter is optional for the user of the web service.</span></span> <span data-ttu-id="43914-122">Ha nem ad meg alapértelmezett értéket, majd a felhasználó szükséges adjon meg egy értéket, ha hozzáfér a webes szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="43914-122">If you don't provide a default value, then the user is required to enter a value when the web service is accessed.</span></span>

<span data-ttu-id="43914-123">A webszolgáltatás API dokumentációja tartalmaz adatokat a szolgáltatás felhasználó programozott módon megadása a webszolgáltatási paraméter, a webszolgáltatás elérésekor.</span><span class="sxs-lookup"><span data-stu-id="43914-123">The API documentation for the web service includes information for the web service user on how to specify the Web Service Parameter programmatically when accessing the web service.</span></span>

> [!NOTE]
> <span data-ttu-id="43914-124">Az API dokumentációjának klasszikus webszolgáltatáshoz keresztül valósul meg a **API súgólap** hivatkozásra a webszolgáltatás **IRÁNYÍTÓPULT** a Machine Learning Studióban.</span><span class="sxs-lookup"><span data-stu-id="43914-124">The API documentation for a classic web service is provided through the **API help page** link in the web service **DASHBOARD** in Machine Learning Studio.</span></span> <span data-ttu-id="43914-125">Egy új webszolgáltatás-bővítmény API dokumentációjában keresztül valósul meg a [Azure Machine Learning webszolgáltatások](https://services.azureml.net/Quickstart) portált a **felhasználás** és **Swagger API** a Web pages a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="43914-125">The API documentation for a new web service is provided through the [Azure Machine Learning Web Services](https://services.azureml.net/Quickstart) portal on the **Consume** and **Swagger API** pages for your web service.</span></span>
> 
> 

## <a name="example"></a><span data-ttu-id="43914-126">Példa</span><span class="sxs-lookup"><span data-stu-id="43914-126">Example</span></span>
<span data-ttu-id="43914-127">Tegyük fel, tételezzük fel, hogy az a kísérlet van egy [adatok exportálása] [ writer] modul, amely adatokat küld az Azure blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="43914-127">As an example, let's assume we have an experiment with an [Export Data][writer] module that sends information to Azure blob storage.</span></span> <span data-ttu-id="43914-128">A webszolgáltatási nevű, "Blob elérési út" paraméter fogunk meghatározni, amely lehetővé teszi, hogy a szolgáltatás felhasználó elérési útjának módosítása a blob-tároló, a szolgáltatás elérésekor.</span><span class="sxs-lookup"><span data-stu-id="43914-128">We'll define a Web Service Parameter named "Blob path" that allows the web service user to change the path to the blob storage when the service is accessed.</span></span>

1. <span data-ttu-id="43914-129">A Machine Learning Studióban, kattintson a [adatok exportálása] [ writer] modul rá kattintva jelölje ki.</span><span class="sxs-lookup"><span data-stu-id="43914-129">In Machine Learning Studio, click the [Export Data][writer] module to select it.</span></span> <span data-ttu-id="43914-130">A Tulajdonságok panelen jobb oldalán a kísérletvászonra tulajdonságai jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="43914-130">Its properties are shown in the Properties pane to the right of the experiment canvas.</span></span>
2. <span data-ttu-id="43914-131">Adja meg a tárolási típusát:</span><span class="sxs-lookup"><span data-stu-id="43914-131">Specify the storage type:</span></span>
   
   * <span data-ttu-id="43914-132">A **adja meg az adatok cél**, jelölje be az "Azure Blob Storage".</span><span class="sxs-lookup"><span data-stu-id="43914-132">Under **Please specify data destination**, select "Azure Blob Storage".</span></span>
   * <span data-ttu-id="43914-133">A **adja meg a hitelesítési típus**, jelölje be az "Account".</span><span class="sxs-lookup"><span data-stu-id="43914-133">Under **Please specify authentication type**, select "Account".</span></span>
   * <span data-ttu-id="43914-134">Írja be az Azure blob storage fiók adatait.</span><span class="sxs-lookup"><span data-stu-id="43914-134">Enter the account information for the Azure blob storage.</span></span> 
     <p /><span data-ttu-id="43914-135">
3.Kattintson a jobb oldalán ikonra a **elérési útját a blob-tároló paraméter-tól kezdődően**.</span><span class="sxs-lookup"><span data-stu-id="43914-135">
3. Click the icon to the right of the **Path to blob beginning with container parameter**.</span></span> <span data-ttu-id="43914-136">Néz ki:</span><span class="sxs-lookup"><span data-stu-id="43914-136">It looks like this:</span></span>
   
   ![Webes szolgáltatás paraméter ikon][icon]
   
   <span data-ttu-id="43914-138">Válassza ki a "Web service paraméter beállítása".</span><span class="sxs-lookup"><span data-stu-id="43914-138">Select "Set as web service parameter".</span></span>
   
   <span data-ttu-id="43914-139">Bejegyzés hozzáadása az **webszolgáltatás-paramétereket** "Blob-tároló kezdetű elérési" nevű Tulajdonságok ablaktábla alján.</span><span class="sxs-lookup"><span data-stu-id="43914-139">An entry is added under **Web Service Parameters** at the bottom of the Properties pane with the name "Path to blob beginning with container".</span></span> <span data-ttu-id="43914-140">Ez az a webszolgáltatási paraméter, amelyik most már a társított [adatok exportálása] [ writer] modul paraméter.</span><span class="sxs-lookup"><span data-stu-id="43914-140">This is the Web Service Parameter that is now associated with this [Export Data][writer] module parameter.</span></span>
4. <span data-ttu-id="43914-141">Nevezze át a webes paraméter, kattintson a nevére, írja be a "Blob path", és nyomja le a **Enter** kulcs.</span><span class="sxs-lookup"><span data-stu-id="43914-141">To rename the Web Service Parameter, click the name, enter "Blob path", and press the **Enter** key.</span></span> 
5. <span data-ttu-id="43914-142">A webszolgáltatási paraméter alapértelmezett értéke megadásához kattintson az ikonra a nevétől jobbra, válassza a "Megadása az alapértelmezett érték", adjon meg egy értéket (például "container1/output1.csv"), és nyomja le az ENTER a **Enter** kulcs.</span><span class="sxs-lookup"><span data-stu-id="43914-142">To provide a default value for the Web Service Parameter, click the icon to the right of the name, select "Provide default value", enter a value (for example, "container1/output1.csv"), and press the **Enter** key.</span></span>
   
   ![Webszolgáltatási paraméter][parameter]
6. <span data-ttu-id="43914-144">Kattintson a **Run** (Futtatás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="43914-144">Click **Run**.</span></span> 
7. <span data-ttu-id="43914-145">Kattintson a **webes szolgáltatás telepítése** válassza **webes szolgáltatás telepítése [klasszikus]** vagy **[Új] webes szolgáltatás telepítése** a webszolgáltatás telepítése.</span><span class="sxs-lookup"><span data-stu-id="43914-145">Click **Deploy Web Service** and select **Deploy Web Service [Classic]** or **Deploy Web Service [New]** to deploy the web service.</span></span>

> [!NOTE] 
> <span data-ttu-id="43914-146">Egy új webszolgáltatás-bővítmény telepítése, megfelelő engedélyekkel kell rendelkeznie, amelyhez az előfizetést, a webszolgáltatás telepítése.</span><span class="sxs-lookup"><span data-stu-id="43914-146">To deploy a New web service you must have sufficient permissions in the subscription to which you deploying the web service.</span></span> <span data-ttu-id="43914-147">További információ: [kezelése az Azure Machine Learning webszolgáltatások portál használatával egy webszolgáltatás-bővítmény](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="43914-147">For more information see, [Manage a Web service using the Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

<span data-ttu-id="43914-148">A felhasználó a webszolgáltatás most adhatja meg az új cél a [adatok exportálása] [ writer] modul a webszolgáltatáshoz való hozzáféréskor.</span><span class="sxs-lookup"><span data-stu-id="43914-148">The user of the web service can now specify a new destination for the [Export Data][writer] module when accessing the web service.</span></span>

## <a name="more-information"></a><span data-ttu-id="43914-149">További információ</span><span class="sxs-lookup"><span data-stu-id="43914-149">More information</span></span>
<span data-ttu-id="43914-150">Részletes példa, tekintse meg a [webszolgáltatás-paramétereket](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx) bejegyzést a [Machine Learning Blog](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx).</span><span class="sxs-lookup"><span data-stu-id="43914-150">For a more detailed example, see the [Web Service Parameters](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx) entry in the [Machine Learning Blog](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx).</span></span>

<span data-ttu-id="43914-151">A Machine Learning webszolgáltatás elérése további információkért lásd: [hogyan kell használni az Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span><span class="sxs-lookup"><span data-stu-id="43914-151">For more information on accessing a Machine Learning web service, see [How to consume an Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span></span>

<!-- Images -->
[icon]: ./media/machine-learning-web-service-parameters/icon.png
[parameter]: ./media/machine-learning-web-service-parameters/parameter.png


<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[reader]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[writer]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/

