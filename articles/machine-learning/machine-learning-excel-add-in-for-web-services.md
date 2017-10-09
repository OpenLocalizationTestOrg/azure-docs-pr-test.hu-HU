---
title: "aaaExcel bővítmény a Machine Learning webszolgáltatások |} Microsoft Docs"
description: "Azure Machine Learning Web toouse hogyan közvetlenül az Excel services-programozás nélkül."
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
ms.openlocfilehash: c52f40d33c9907f284e4750afe47181dc3365fe5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="excel-add-in-for-azure-machine-learning-web-services"></a><span data-ttu-id="f920f-103">Excel-bővítmény az Azure Machine Learning webszolgáltatásokhoz</span><span class="sxs-lookup"><span data-stu-id="f920f-103">Excel Add-in for Azure Machine Learning web services</span></span>
<span data-ttu-id="f920f-104">Excel segítségével könnyen toocall webszolgáltatások közvetlenül hello kell toowrite kódok nélkül.</span><span class="sxs-lookup"><span data-stu-id="f920f-104">Excel makes it easy toocall web services directly without hello need toowrite any code.</span></span>

## <a name="steps-toouse-an-existing-web-service-in-hello-workbook"></a><span data-ttu-id="f920f-105">Egy meglévő webszolgáltatás hello munkafüzetben szereplő lépéseket tooUse</span><span class="sxs-lookup"><span data-stu-id="f920f-105">Steps tooUse an Existing web service in hello Workbook</span></span>

1. <span data-ttu-id="f920f-106">Nyissa meg hello [minta Excel-fájl](http://aka.ms/amlexcel-sample-2), tartalmazó hello Excel-bővítmény, és az utasok adatait hello Titanic.</span><span class="sxs-lookup"><span data-stu-id="f920f-106">Open hello [sample Excel file](http://aka.ms/amlexcel-sample-2), which contains hello Excel add-in and data about passengers on hello Titanic.</span></span>
2. <span data-ttu-id="f920f-107">Kattintással válassza ki a hello webszolgáltatás-"Titanic túlélő Előrejelzőjének (Excel-bővítmény minta) [Pontszám]" Ebben a példában.</span><span class="sxs-lookup"><span data-stu-id="f920f-107">Choose hello web service by clicking it - "Titanic Survivor Predictor (Excel Add-in Sample) [Score]" in this example.</span></span>
   
    ![Válassza ki a webszolgáltatás][01]
3. <span data-ttu-id="f920f-109">Ezzel megnyitná toohello **Predict** szakasz.</span><span class="sxs-lookup"><span data-stu-id="f920f-109">This takes you toohello **Predict** section.</span></span>  <span data-ttu-id="f920f-110">Ez a munkafüzet már tartalmaz adatot, de üres a munkafüzet az Excel programban a cella kijelöléséhez és kattintson a **mintaadatokkal**.</span><span class="sxs-lookup"><span data-stu-id="f920f-110">This workbook already contains sample data, but for a blank workbook you can select a cell in Excel and click **Use sample data**.</span></span>
4. <span data-ttu-id="f920f-111">Válassza ki a fejlécek hello adatok és hello bemeneti adatok tartomány ikonra.</span><span class="sxs-lookup"><span data-stu-id="f920f-111">Select hello data with headers and click hello input data range icon.</span></span>  <span data-ttu-id="f920f-112">Győződjön meg arról, hogy hello "adataimat fejlécekkel rendelkezik" jelölőnégyzet be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="f920f-112">Make sure hello "My data has headers" box is checked.</span></span>
5. <span data-ttu-id="f920f-113">A **kimeneti**, adja meg hello cella kívánt kimeneti toobe, például "H1" hello itt.</span><span class="sxs-lookup"><span data-stu-id="f920f-113">Under **Output**, enter hello cell number where you want hello output toobe, for example "H1" here.</span></span>
6. <span data-ttu-id="f920f-114">Kattintson a **előrejelzése**.</span><span class="sxs-lookup"><span data-stu-id="f920f-114">Click **Predict**.</span></span>
   
    ![A szakasz előrejelzése][02]

<span data-ttu-id="f920f-116">A webes szolgáltatás, vagy használja a meglévő webszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="f920f-116">Deploy a web service or use an existing Web service.</span></span> <span data-ttu-id="f920f-117">Egy webszolgáltatás-bővítmény telepítésével kapcsolatos további információkért lásd: [útmutató 5. lépés: hello Azure Machine Learning webszolgáltatás telepítése](machine-learning-walkthrough-5-publish-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="f920f-117">For more information on deploying a web service, see [Walkthrough Step 5: Deploy hello Azure Machine Learning Web service](machine-learning-walkthrough-5-publish-web-service.md).</span></span>

<span data-ttu-id="f920f-118">A webszolgáltatáshoz hello API-kulcs beszerzése.</span><span class="sxs-lookup"><span data-stu-id="f920f-118">Get hello API key for your web service.</span></span> <span data-ttu-id="f920f-119">Ahol elvégezhető ez a művelet függ a klasszikus Machine Learning webszolgáltatás egy új Machine Learning-webszolgáltatás közzététele e.</span><span class="sxs-lookup"><span data-stu-id="f920f-119">Where you perform this action depends on whether you published a Classic Machine Learning web service of a New Machine Learning web service.</span></span>

<span data-ttu-id="f920f-120">**A klasszikus webszolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="f920f-120">**Use a Classic web service**</span></span> 

1. <span data-ttu-id="f920f-121">A Machine Learning Studióban, kattintson a hello **WEBSZOLGÁLTATÁSOK** szakasz hello bal oldali ablaktáblán, majd válassza ki a hello webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="f920f-121">In Machine Learning Studio, click hello **WEB SERVICES** section in hello left pane, and then select hello web service.</span></span>
   
    ![Studio válasszon egy webszolgáltatás-bővítmény][04]
2. <span data-ttu-id="f920f-123">Másolja az API-kulcs hello hello webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="f920f-123">Copy hello API key for hello web service.</span></span>
   
    ![Studio API-kulcs][05]
3. <span data-ttu-id="f920f-125">A hello **IRÁNYÍTÓPULT** hello webszolgáltatáshoz lapra, majd hello **kérelem/válasz** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="f920f-125">On hello **DASHBOARD** tab for hello web service, click hello **REQUEST/RESPONSE** link.</span></span>
4. <span data-ttu-id="f920f-126">Keresse meg hello **kérelem URI-azonosítója** szakasz.</span><span class="sxs-lookup"><span data-stu-id="f920f-126">Look for hello **Request URI** section.</span></span>  <span data-ttu-id="f920f-127">Másolja ki és mentse a hello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="f920f-127">Copy and save hello URL.</span></span>

> [!NOTE]
> <span data-ttu-id="f920f-128">Már lehetséges toosign történő hello [Azure Machine Learning webszolgáltatások](https://services.azureml.net) portál tooobtain hello API-kulcs egy klasszikus Machine Learning webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="f920f-128">It is now possible toosign into hello [Azure Machine Learning Web Services](https://services.azureml.net) portal tooobtain hello API key for a Classic Machine Learning web service.</span></span>
> 
> 

<span data-ttu-id="f920f-129">**Új webszolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="f920f-129">**Use a New web service**</span></span>

1. <span data-ttu-id="f920f-130">A hello [Azure Machine Learning webszolgáltatások](https://services.azureml.net) portálon kattintson **webszolgáltatások**, majd válassza a webes szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="f920f-130">In hello [Azure Machine Learning Web Services](https://services.azureml.net) portal, click **Web Services**, then select your web service.</span></span> 
2. <span data-ttu-id="f920f-131">Kattintson a **felhasználásához**.</span><span class="sxs-lookup"><span data-stu-id="f920f-131">Click **Consume**.</span></span>
3. <span data-ttu-id="f920f-132">Keresse meg hello **alapvető fogyasztási adatai** szakasz.</span><span class="sxs-lookup"><span data-stu-id="f920f-132">Look for hello **Basic consumption info** section.</span></span> <span data-ttu-id="f920f-133">Másolja ki és mentse a hello **elsődleges kulcs** és hello **kérés-válasz** URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="f920f-133">Copy and save hello **Primary Key** and hello **Request-Response** URL.</span></span>

## <a name="steps-tooadd-a-new-web-service"></a><span data-ttu-id="f920f-134">Lépéseket tooAdd egy új webszolgáltatás-bővítmény</span><span class="sxs-lookup"><span data-stu-id="f920f-134">Steps tooAdd a New web service</span></span>

1. <span data-ttu-id="f920f-135">A webes szolgáltatás, vagy használja a meglévő webszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="f920f-135">Deploy a web service or use an existing Web service.</span></span> <span data-ttu-id="f920f-136">Egy webszolgáltatás-bővítmény telepítésével kapcsolatos további információkért lásd: [útmutató 5. lépés: hello Azure Machine Learning webszolgáltatás telepítése](machine-learning-walkthrough-5-publish-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="f920f-136">For more information on deploying a web service, see [Walkthrough Step 5: Deploy hello Azure Machine Learning Web service](machine-learning-walkthrough-5-publish-web-service.md).</span></span>
2. <span data-ttu-id="f920f-137">Kattintson a **felhasználásához**.</span><span class="sxs-lookup"><span data-stu-id="f920f-137">Click **Consume**.</span></span>
3. <span data-ttu-id="f920f-138">Keresse meg hello **alapvető fogyasztási adatai** szakasz.</span><span class="sxs-lookup"><span data-stu-id="f920f-138">Look for hello **Basic consumption info** section.</span></span> <span data-ttu-id="f920f-139">Másolja ki és mentse a hello **elsődleges kulcs** és hello **kérés-válasz** URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="f920f-139">Copy and save hello **Primary Key** and hello **Request-Response** URL.</span></span>
4. <span data-ttu-id="f920f-140">Az Excel programban, lépjen a toohello **webszolgáltatások** szakasz (Ha Ön a hello **Predict** hello vissza toogo toohello webes szolgáltatások listájában kattintson).</span><span class="sxs-lookup"><span data-stu-id="f920f-140">In Excel, go toohello **Web Services** section (if you are in hello **Predict** section, click hello back arrow toogo toohello list of web services).</span></span>
   
    ![Nyissa meg tooWeb szolgáltatás kiválasztása][03]
5. <span data-ttu-id="f920f-142">Kattintson a **webszolgáltatás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="f920f-142">Click **Add Web Service**.</span></span>
6. <span data-ttu-id="f920f-143">Hello URL-cím illessze be az Excel-bővítmény szövegmező feliratú hello **URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="f920f-143">Paste hello URL into hello Excel add-in text box labeled **URL**.</span></span>
7. <span data-ttu-id="f920f-144">Beillesztés hello API/elsődleges kulcs feliratú hello szövegmezőbe **API-kulcs**.</span><span class="sxs-lookup"><span data-stu-id="f920f-144">Paste hello API/Primary key into hello text box labeled **API key**.</span></span>
8. <span data-ttu-id="f920f-145">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="f920f-145">Click **Add**.</span></span>
   
    ![URL-CÍMÉT és API-kulcsát egy klasszikus webszolgáltatáshoz.][06]
9. <span data-ttu-id="f920f-147">toouse hello webszolgáltatás, kövesse az utasításokat megelőző hello, "Lépések tooUse egy meglévő webes szolgáltatás."</span><span class="sxs-lookup"><span data-stu-id="f920f-147">toouse hello web service, follow hello preceding directions, "Steps tooUse an Existing web Service."</span></span>

## <a name="sharing-your-workbook"></a><span data-ttu-id="f920f-148">A munkafüzet megosztása</span><span class="sxs-lookup"><span data-stu-id="f920f-148">Sharing Your Workbook</span></span>
<span data-ttu-id="f920f-149">Ha a munkafüzet mentéséhez hello API/elsődleges kulcs hello web Services hozzáadott is menti.</span><span class="sxs-lookup"><span data-stu-id="f920f-149">If you save your workbook, then hello API/Primary key for hello web services you have added is also saved.</span></span> <span data-ttu-id="f920f-150">Ez azt jelenti, hogy csak ossza meg hello munkafüzet megbízható személlyel.</span><span class="sxs-lookup"><span data-stu-id="f920f-150">That means you should only share hello workbook with individuals you trust.</span></span>

<span data-ttu-id="f920f-151">Bármely kérdései vannak a következő megjegyzés szakasz vagy a hello a [fórum](http://go.microsoft.com/fwlink/?LinkID=403669&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="f920f-151">Ask any questions in hello following comment section or on our [forum](http://go.microsoft.com/fwlink/?LinkID=403669&clcid=0x409).</span></span>

[01]: ./media/machine-learning-excel-add-in-for-web-services/image1.png
[02]: ./media/machine-learning-excel-add-in-for-web-services/image2.png
[03]: ./media/machine-learning-excel-add-in-for-web-services/image3.png
[04]: ./media/machine-learning-excel-add-in-for-web-services/image4.png
[05]: ./media/machine-learning-excel-add-in-for-web-services/image5.png
[06]: ./media/machine-learning-excel-add-in-for-web-services/image6.png
