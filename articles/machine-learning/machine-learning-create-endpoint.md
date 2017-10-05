---
title: "A Machine Learning webszolgáltatás-végpontok létrehozása |} Microsoft Docs"
description: "Az Azure Machine Learning webszolgáltatás-végpontok létrehozása"
services: machine-learning
documentationcenter: 
author: hiteshmadan
manager: padou
editor: cgronlun
ms.assetid: 4657fc1b-5228-4950-a29e-bc709259f728
ms.service: machine-learning
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 10/04/2016
ms.author: himad
ms.openlocfilehash: 9f83ffc9cf7dbe37c1ce9980fd7f5b9133fe78f6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="creating-endpoints"></a><span data-ttu-id="2da5e-103">Végpontok létrehozása</span><span class="sxs-lookup"><span data-stu-id="2da5e-103">Creating Endpoints</span></span>
> [!NOTE]
>  <span data-ttu-id="2da5e-104">Ez a témakör ismerteti a technikákról egy **klasszikus** Machine Learning webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="2da5e-104">This topic describes techniques applicable to a **Classic** Machine Learning Web service.</span></span>
> 
> 

<span data-ttu-id="2da5e-105">Webalapú alkalmazásokhoz, az ügyfelek számára előre eladásra létrehozásakor meg kell adnia a betanított modellek minden ügyfélnek, amely továbbra is a kísérlet, a webszolgáltatás létrehozásához használt van csatolva.</span><span class="sxs-lookup"><span data-stu-id="2da5e-105">When you create Web services that you sell forward to your customers, you need to provide trained models to each customer that are still linked to the experiment from which the Web service was created.</span></span> <span data-ttu-id="2da5e-106">Emellett a kísérletvászonra frissítéseket alkalmazandó szelektív végpont a testreszabások felülírása nélkül.</span><span class="sxs-lookup"><span data-stu-id="2da5e-106">In addition, any updates to the experiment should be applied selectively to an endpoint without overwriting the customizations.</span></span>

<span data-ttu-id="2da5e-107">Ennek megvalósítása érdekében Azure Machine Learning hozhat létre az adott üzembe helyezett webszolgáltatás több végpontot.</span><span class="sxs-lookup"><span data-stu-id="2da5e-107">To accomplish this, Azure Machine Learning allows you to create multiple endpoints for a deployed Web service.</span></span> <span data-ttu-id="2da5e-108">A webszolgáltatás a végpontok egymástól függetlenül tárgyalt, szabályozva, és felügyelt.</span><span class="sxs-lookup"><span data-stu-id="2da5e-108">Each endpoint in the Web service is independently addressed, throttled, and managed.</span></span> <span data-ttu-id="2da5e-109">Minden egyes végpont egy egyedi URL-cím és az ügyfelek számára terjeszthető hitelesítési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="2da5e-109">Each endpoint is a unique URL and authorization key that you can distribute to your customers.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="adding-endpoints-to-a-web-service"></a><span data-ttu-id="2da5e-110">Végpontok felvétele egy webszolgáltatás-bővítmény</span><span class="sxs-lookup"><span data-stu-id="2da5e-110">Adding endpoints to a Web service</span></span>
<span data-ttu-id="2da5e-111">A végpont hozzáadása egy webszolgáltatás-bővítmény három módja van.</span><span class="sxs-lookup"><span data-stu-id="2da5e-111">There are three ways to add an endpoint to a Web service.</span></span>

* <span data-ttu-id="2da5e-112">Automatizáltan</span><span class="sxs-lookup"><span data-stu-id="2da5e-112">Programmatically</span></span>
* <span data-ttu-id="2da5e-113">Az Azure Machine Learning webszolgáltatások portálon keresztül</span><span class="sxs-lookup"><span data-stu-id="2da5e-113">Through the Azure Machine Learning Web Services portal</span></span>
* <span data-ttu-id="2da5e-114">Bár a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="2da5e-114">Though the Azure classic portal</span></span>

<span data-ttu-id="2da5e-115">Miután létrehozta a végpontot, szokásokra is szinkron API-k, kötegelt API-k, segítségével, és az excel-munkalapokat.</span><span class="sxs-lookup"><span data-stu-id="2da5e-115">Once the endpoint is created, you can consume it through synchronous APIs, batch APIs, and excel worksheets.</span></span> <span data-ttu-id="2da5e-116">A felhasználói felületen keresztüli végpontokra hozzáadásán is használhatja a végpont felügyeleti API-k szoftveres a végpontok hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="2da5e-116">In addition to adding endpoints through this UI, you can also use the Endpoint Management APIs to programmatically add endpoints.</span></span>

> [!NOTE]
> <span data-ttu-id="2da5e-117">Ha további végpontok hozzáadta a webes szolgáltatás, az alapértelmezett végpont nem törölhető.</span><span class="sxs-lookup"><span data-stu-id="2da5e-117">If you have added additional endpoints to the Web service, you cannot delete the default endpoint.</span></span>
> 
> 

## <a name="adding-an-endpoint-programmatically"></a><span data-ttu-id="2da5e-118">A végpont programozott módon hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2da5e-118">Adding an endpoint programmatically</span></span>
<span data-ttu-id="2da5e-119">A webszolgáltatás programozott módon adhat hozzá a végpont a [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) példakód.</span><span class="sxs-lookup"><span data-stu-id="2da5e-119">You can add an endpoint to your Web service programmatically using the [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) sample code.</span></span>

## <a name="adding-an-endpoint-using-the-azure-machine-learning-web-services-portal"></a><span data-ttu-id="2da5e-120">Az Azure Machine Learning webszolgáltatások portálon végpont hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2da5e-120">Adding an endpoint using the Azure Machine Learning Web Services portal</span></span>
1. <span data-ttu-id="2da5e-121">A Machine Learning Studio a bal oldali oszlopban kattintson a webszolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="2da5e-121">In Machine Learning Studio, on the left navigation column, click Web Services.</span></span>
2. <span data-ttu-id="2da5e-122">A webes szolgáltatás irányítópultját alján kattintson **végpontok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="2da5e-122">At the bottom of the Web service dashboard, click **Manage endpoints**.</span></span> <span data-ttu-id="2da5e-123">Az Azure Machine Learning webszolgáltatások portal nyitja meg a webszolgáltatás végpontok oldalon.</span><span class="sxs-lookup"><span data-stu-id="2da5e-123">The Azure Machine Learning Web Services portal opens to the endpoints page for the Web service.</span></span>
3. <span data-ttu-id="2da5e-124">Kattintson az **Új** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="2da5e-124">Click **New**.</span></span>
4. <span data-ttu-id="2da5e-125">Írja be nevét és leírását, az új végpont.</span><span class="sxs-lookup"><span data-stu-id="2da5e-125">Type a name and description for the new endpoint.</span></span> <span data-ttu-id="2da5e-126">Végpontneveit 24 karakter vagy kevesebb hosszúságúnak kell lennie, és a magyar ábécét kisbetűket és számokat kell készíteni.</span><span class="sxs-lookup"><span data-stu-id="2da5e-126">Endpoint names must be 24 character or less in length, and must be made up of lower-case alphabets or numbers.</span></span> <span data-ttu-id="2da5e-127">Válassza ki a naplózási szint, és hogy engedélyezve van-e mintaadatokat.</span><span class="sxs-lookup"><span data-stu-id="2da5e-127">Select the logging level and whether sample data is enabled.</span></span> <span data-ttu-id="2da5e-128">További információt a naplózást, [naplózását a Machine Learning webszolgáltatások](machine-learning-web-services-logging.md).</span><span class="sxs-lookup"><span data-stu-id="2da5e-128">For more information on logging, see [Enable logging for Machine Learning Web services](machine-learning-web-services-logging.md).</span></span>

## <a name="adding-an-endpoint-using-the-azure-classic-portal"></a><span data-ttu-id="2da5e-129">A klasszikus Azure portál használatával a végpont hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2da5e-129">Adding an endpoint using the Azure classic portal</span></span>
1. <span data-ttu-id="2da5e-130">Jelentkezzen be a [a klasszikus Azure portálon](http://manage.windowsazure.com), kattintson a **Machine Learning** a bal oldali oszlopban.</span><span class="sxs-lookup"><span data-stu-id="2da5e-130">Sign in to the [Azure classic portal](http://manage.windowsazure.com), click **Machine Learning** in the left column.</span></span> <span data-ttu-id="2da5e-131">Kattintson a munkaterületet, amely tartalmazza a webes szolgáltatás, amelyben érdekli.</span><span class="sxs-lookup"><span data-stu-id="2da5e-131">Click the workspace which contains the Web service in which you are interested.</span></span>
   
    ![Navigáljon a munkaterület](./media/machine-learning-create-endpoint/figure-1.png)
2. <span data-ttu-id="2da5e-133">Kattintson a **webszolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="2da5e-133">Click **Web Services**.</span></span>
   
    ![Navigáljon a webes szolgáltatások](./media/machine-learning-create-endpoint/figure-2.png)
3. <span data-ttu-id="2da5e-135">Kattintson a webszolgáltatás számára elérhető végpontok listájának kíváncsiak vagyunk.</span><span class="sxs-lookup"><span data-stu-id="2da5e-135">Click the Web service you're interested in to see the list of available endpoints.</span></span>
   
    ![Navigáljon a végpont](./media/machine-learning-create-endpoint/figure-3.png)
4. <span data-ttu-id="2da5e-137">Kattintson a lap alján **végpont hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="2da5e-137">At the bottom of the page, click **Add Endpoint**.</span></span> <span data-ttu-id="2da5e-138">Írja be a nevet és leírást, ellenőrizze, hogy nincsenek a webszolgáltatás azonos nevű nincsenek végpontok.</span><span class="sxs-lookup"><span data-stu-id="2da5e-138">Type a name and description, ensure there are no other endpoints with the same name in this Web service.</span></span> <span data-ttu-id="2da5e-139">Amennyiben nincsenek különleges követelmények a késleltetési szintjét hagyja az alapértelmezett értékre.</span><span class="sxs-lookup"><span data-stu-id="2da5e-139">Leave the throttle level with its default value unless you have special requirements.</span></span> <span data-ttu-id="2da5e-140">Sávszélesség-szabályozás kapcsolatos további információkért lásd: [API-végpontok méretezése](machine-learning-scaling-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="2da5e-140">To learn more about throttling, see [Scaling API Endpoints](machine-learning-scaling-webservice.md).</span></span>
   
    ![-Végpont létrehozása](./media/machine-learning-create-endpoint/figure-4.png)

## <a name="next-steps"></a><span data-ttu-id="2da5e-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2da5e-142">Next Steps</span></span>
<span data-ttu-id="2da5e-143">[Az Azure Machine Learning Web service felhasználásához hogyan](machine-learning-consume-web-services.md).</span><span class="sxs-lookup"><span data-stu-id="2da5e-143">[How to consume an Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span></span>

