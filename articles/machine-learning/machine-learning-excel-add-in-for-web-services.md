---
title: "Excel-bővítmény a Machine Learning webszolgáltatások |} Microsoft Docs"
description: "Hogyan használható az Azure Machine Learning webszolgáltatások közvetlenül az Excel programban programozás nélkül."
services: machine-learning
documentationcenter: 
author: tedway
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: 9618079d-502f-4974-a3e2-8f924042a23f
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/14/2017
ms.author: tedway;garye
ms.openlocfilehash: 0d60dd87bbdd4d3eafac0f8876cc9e41412a53ea
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="excel-add-in-for-azure-machine-learning-web-services"></a><span data-ttu-id="8e77a-103">Excel-bővítmény az Azure Machine Learning webszolgáltatásokhoz</span><span class="sxs-lookup"><span data-stu-id="8e77a-103">Excel Add-in for Azure Machine Learning web services</span></span>
<span data-ttu-id="8e77a-104">Excel megkönnyíti a webszolgáltatások közvetlenül a szükséges kód írása nélkül.</span><span class="sxs-lookup"><span data-stu-id="8e77a-104">Excel makes it easy to call web services directly without the need to write any code.</span></span>

## <a name="steps-to-use-an-existing-web-service-in-the-workbook"></a><span data-ttu-id="8e77a-105">A munkafüzet egy meglévő webes szolgáltatás használatának lépéseit</span><span class="sxs-lookup"><span data-stu-id="8e77a-105">Steps to Use an Existing web service in the Workbook</span></span>

1. <span data-ttu-id="8e77a-106">Nyissa meg a [minta Excel-fájl](http://aka.ms/amlexcel-sample-2), amely Excel-bővítmény és a Titanic utasok adatait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="8e77a-106">Open the [sample Excel file](http://aka.ms/amlexcel-sample-2), which contains the Excel add-in and data about passengers on the Titanic.</span></span>
2. <span data-ttu-id="8e77a-107">Válassza ki a webszolgáltatás rákattintva-"Titanic túlélő Előrejelzőjének (Excel-bővítmény minta) [Pontszám]" Ebben a példában.</span><span class="sxs-lookup"><span data-stu-id="8e77a-107">Choose the web service by clicking it - "Titanic Survivor Predictor (Excel Add-in Sample) [Score]" in this example.</span></span>
   
    ![Válassza ki a webszolgáltatás][01]
3. <span data-ttu-id="8e77a-109">Ehhez szükséges, hogy a **Predict** szakasz.</span><span class="sxs-lookup"><span data-stu-id="8e77a-109">This takes you to the **Predict** section.</span></span>  <span data-ttu-id="8e77a-110">Ez a munkafüzet már tartalmaz adatot, de üres a munkafüzet az Excel programban a cella kijelöléséhez és kattintson a **mintaadatokkal**.</span><span class="sxs-lookup"><span data-stu-id="8e77a-110">This workbook already contains sample data, but for a blank workbook you can select a cell in Excel and click **Use sample data**.</span></span>
4. <span data-ttu-id="8e77a-111">Válassza ki az adatokat a fejlécek és a bemeneti adatok tartomány ikonra.</span><span class="sxs-lookup"><span data-stu-id="8e77a-111">Select the data with headers and click the input data range icon.</span></span>  <span data-ttu-id="8e77a-112">Győződjön meg arról, hogy a "adataimat fejlécekkel rendelkezik" jelölőnégyzet be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="8e77a-112">Make sure the "My data has headers" box is checked.</span></span>
5. <span data-ttu-id="8e77a-113">A **kimeneti**, adja meg a cella számát, ahol azt szeretné, hogy a kimenet, például "H1" Itt.</span><span class="sxs-lookup"><span data-stu-id="8e77a-113">Under **Output**, enter the cell number where you want the output to be, for example "H1" here.</span></span>
6. <span data-ttu-id="8e77a-114">Kattintson a **előrejelzése**.</span><span class="sxs-lookup"><span data-stu-id="8e77a-114">Click **Predict**.</span></span>
   
    ![A szakasz előrejelzése][02]

<span data-ttu-id="8e77a-116">A webes szolgáltatás, vagy használja a meglévő webszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="8e77a-116">Deploy a web service or use an existing Web service.</span></span> <span data-ttu-id="8e77a-117">Egy webszolgáltatás-bővítmény telepítésével kapcsolatos további információkért lásd: [útmutató 5. lépés: központi telepítése az Azure Machine Learning Web service](machine-learning-walkthrough-5-publish-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="8e77a-117">For more information on deploying a web service, see [Walkthrough Step 5: Deploy the Azure Machine Learning Web service](machine-learning-walkthrough-5-publish-web-service.md).</span></span>

<span data-ttu-id="8e77a-118">A webszolgáltatáshoz API-kulcs beszerzése.</span><span class="sxs-lookup"><span data-stu-id="8e77a-118">Get the API key for your web service.</span></span> <span data-ttu-id="8e77a-119">Ahol elvégezhető ez a művelet függ a klasszikus Machine Learning webszolgáltatás egy új Machine Learning-webszolgáltatás közzététele e.</span><span class="sxs-lookup"><span data-stu-id="8e77a-119">Where you perform this action depends on whether you published a Classic Machine Learning web service of a New Machine Learning web service.</span></span>

<span data-ttu-id="8e77a-120">**A klasszikus webszolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="8e77a-120">**Use a Classic web service**</span></span> 

1. <span data-ttu-id="8e77a-121">A Machine Learning Studióban, kattintson a **WEBSZOLGÁLTATÁSOK** szakaszt a bal oldali ablaktáblán, és válassza ki a webszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="8e77a-121">In Machine Learning Studio, click the **WEB SERVICES** section in the left pane, and then select the web service.</span></span>
   
    ![Studio válasszon egy webszolgáltatás-bővítmény][04]
2. <span data-ttu-id="8e77a-123">Másolja az API-kulcsot a webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="8e77a-123">Copy the API key for the web service.</span></span>
   
    ![Studio API-kulcs][05]
3. <span data-ttu-id="8e77a-125">Az a **IRÁNYÍTÓPULT** a webszolgáltatás lapra, majd a **kérelem/válasz** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="8e77a-125">On the **DASHBOARD** tab for the web service, click the **REQUEST/RESPONSE** link.</span></span>
4. <span data-ttu-id="8e77a-126">Keresse meg a **kérelem URI-azonosítója** szakasz.</span><span class="sxs-lookup"><span data-stu-id="8e77a-126">Look for the **Request URI** section.</span></span>  <span data-ttu-id="8e77a-127">Másolja ki és mentse az URL-címet.</span><span class="sxs-lookup"><span data-stu-id="8e77a-127">Copy and save the URL.</span></span>

> [!NOTE]
> <span data-ttu-id="8e77a-128">Már lehetséges a jelentkezzen be a [Azure Machine Learning webszolgáltatások](https://services.azureml.net) portálra, ahol az adott klasszikus Machine Learning webszolgáltatás API-kulcs beszerzése.</span><span class="sxs-lookup"><span data-stu-id="8e77a-128">It is now possible to sign into the [Azure Machine Learning Web Services](https://services.azureml.net) portal to obtain the API key for a Classic Machine Learning web service.</span></span>
> 
> 

<span data-ttu-id="8e77a-129">**Új webszolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="8e77a-129">**Use a New web service**</span></span>

1. <span data-ttu-id="8e77a-130">Az a [Azure Machine Learning webszolgáltatások](https://services.azureml.net) portálon kattintson **webszolgáltatások**, majd válassza a webes szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="8e77a-130">In the [Azure Machine Learning Web Services](https://services.azureml.net) portal, click **Web Services**, then select your web service.</span></span> 
2. <span data-ttu-id="8e77a-131">Kattintson a **felhasználásához**.</span><span class="sxs-lookup"><span data-stu-id="8e77a-131">Click **Consume**.</span></span>
3. <span data-ttu-id="8e77a-132">Keresse meg a **alapvető fogyasztási adatai** szakasz.</span><span class="sxs-lookup"><span data-stu-id="8e77a-132">Look for the **Basic consumption info** section.</span></span> <span data-ttu-id="8e77a-133">Másolja ki és mentse a **elsődleges kulcs** és a **kérés-válasz** URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="8e77a-133">Copy and save the **Primary Key** and the **Request-Response** URL.</span></span>

## <a name="steps-to-add-a-new-web-service"></a><span data-ttu-id="8e77a-134">Lépések végrehajtásával adja hozzá egy új webszolgáltatás-bővítmény</span><span class="sxs-lookup"><span data-stu-id="8e77a-134">Steps to Add a New web service</span></span>

1. <span data-ttu-id="8e77a-135">A webes szolgáltatás, vagy használja a meglévő webszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="8e77a-135">Deploy a web service or use an existing Web service.</span></span> <span data-ttu-id="8e77a-136">Egy webszolgáltatás-bővítmény telepítésével kapcsolatos további információkért lásd: [útmutató 5. lépés: központi telepítése az Azure Machine Learning Web service](machine-learning-walkthrough-5-publish-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="8e77a-136">For more information on deploying a web service, see [Walkthrough Step 5: Deploy the Azure Machine Learning Web service](machine-learning-walkthrough-5-publish-web-service.md).</span></span>
2. <span data-ttu-id="8e77a-137">Kattintson a **felhasználásához**.</span><span class="sxs-lookup"><span data-stu-id="8e77a-137">Click **Consume**.</span></span>
3. <span data-ttu-id="8e77a-138">Keresse meg a **alapvető fogyasztási adatai** szakasz.</span><span class="sxs-lookup"><span data-stu-id="8e77a-138">Look for the **Basic consumption info** section.</span></span> <span data-ttu-id="8e77a-139">Másolja ki és mentse a **elsődleges kulcs** és a **kérés-válasz** URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="8e77a-139">Copy and save the **Primary Key** and the **Request-Response** URL.</span></span>
4. <span data-ttu-id="8e77a-140">Az Excel programban, lépjen a **webszolgáltatások** szakasz (Ha a **Predict** területen kattintson a Vissza gombra kattintva a webes szolgáltatások listáját).</span><span class="sxs-lookup"><span data-stu-id="8e77a-140">In Excel, go to the **Web Services** section (if you are in the **Predict** section, click the back arrow to go to the list of web services).</span></span>
   
    ![Ugrás a webes szolgáltatás kiválasztása][03]
5. <span data-ttu-id="8e77a-142">Kattintson a **webszolgáltatás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="8e77a-142">Click **Add Web Service**.</span></span>
6. <span data-ttu-id="8e77a-143">Az URL-cím illessze be az Excel-bővítmény szövegmező feliratú **URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="8e77a-143">Paste the URL into the Excel add-in text box labeled **URL**.</span></span>
7. <span data-ttu-id="8e77a-144">Az API/elsődleges kulcs illessze be a mezőbe **API-kulcs**.</span><span class="sxs-lookup"><span data-stu-id="8e77a-144">Paste the API/Primary key into the text box labeled **API key**.</span></span>
8. <span data-ttu-id="8e77a-145">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="8e77a-145">Click **Add**.</span></span>
   
    ![URL-CÍMÉT és API-kulcsát egy klasszikus webszolgáltatáshoz.][06]
9. <span data-ttu-id="8e77a-147">A webszolgáltatás hajtsa végre az előző szakasz utasításait, "Lépéseket egy meglévő webes szolgáltatás használatára."</span><span class="sxs-lookup"><span data-stu-id="8e77a-147">To use the web service, follow the preceding directions, "Steps to Use an Existing web Service."</span></span>

## <a name="sharing-your-workbook"></a><span data-ttu-id="8e77a-148">A munkafüzet megosztása</span><span class="sxs-lookup"><span data-stu-id="8e77a-148">Sharing Your Workbook</span></span>
<span data-ttu-id="8e77a-149">Mentse a munkafüzetet, ha a hozzáadott webszolgáltatások API/elsődleges kulcsa is menti.</span><span class="sxs-lookup"><span data-stu-id="8e77a-149">If you save your workbook, then the API/Primary key for the web services you have added is also saved.</span></span> <span data-ttu-id="8e77a-150">Ez azt jelenti, hogy csak ossza meg a munkafüzetet megbízható személlyel.</span><span class="sxs-lookup"><span data-stu-id="8e77a-150">That means you should only share the workbook with individuals you trust.</span></span>

<span data-ttu-id="8e77a-151">Kérje meg a következő szakaszban megjegyzés vagy a kérdéseket a [fórum](http://go.microsoft.com/fwlink/?LinkID=403669&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="8e77a-151">Ask any questions in the following comment section or on our [forum](http://go.microsoft.com/fwlink/?LinkID=403669&clcid=0x409).</span></span>

[01]: ./media/machine-learning-excel-add-in-for-web-services/image1.png
[02]: ./media/machine-learning-excel-add-in-for-web-services/image2.png
[03]: ./media/machine-learning-excel-add-in-for-web-services/image3.png
[04]: ./media/machine-learning-excel-add-in-for-web-services/image4.png
[05]: ./media/machine-learning-excel-add-in-for-web-services/image5.png
[06]: ./media/machine-learning-excel-add-in-for-web-services/image6.png
