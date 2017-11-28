---
title: "aaaCreating webszolgáltatás-végpontok a Machine Learning |} Microsoft Docs"
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
ms.openlocfilehash: 10a2bc586c6fe35e28d8bf0293854c578827c453
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="creating-endpoints"></a><span data-ttu-id="4d926-103">Végpontok létrehozása</span><span class="sxs-lookup"><span data-stu-id="4d926-103">Creating Endpoints</span></span>
> [!NOTE]
>  <span data-ttu-id="4d926-104">Ez a témakör ismerteti a technikák alkalmazható tooa **klasszikus** Machine Learning webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="4d926-104">This topic describes techniques applicable tooa **Classic** Machine Learning Web service.</span></span>
> 
> 

<span data-ttu-id="4d926-105">Webalapú alkalmazásokhoz, előre tooyour ügyfelek értékesít létrehozásakor kell tooprovide betanított modellek tooeach ügyfelek, amelyek továbbra is csatolt toohello kísérlet mely hello webes szolgáltatás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="4d926-105">When you create Web services that you sell forward tooyour customers, you need tooprovide trained models tooeach customer that are still linked toohello experiment from which hello Web service was created.</span></span> <span data-ttu-id="4d926-106">Ezenkívül toohello kísérlet kell frissítéseket alkalmazza a szelektív tooan végpont hello testreszabások felülírása nélkül.</span><span class="sxs-lookup"><span data-stu-id="4d926-106">In addition, any updates toohello experiment should be applied selectively tooan endpoint without overwriting hello customizations.</span></span>

<span data-ttu-id="4d926-107">tooaccomplish az Azure Machine Learning toocreate lehetővé teszi az adott üzembe helyezett webszolgáltatás több végpontot.</span><span class="sxs-lookup"><span data-stu-id="4d926-107">tooaccomplish this, Azure Machine Learning allows you toocreate multiple endpoints for a deployed Web service.</span></span> <span data-ttu-id="4d926-108">A webszolgáltatás hello végpontok egymástól függetlenül címzett, szabályozva, és felügyelt.</span><span class="sxs-lookup"><span data-stu-id="4d926-108">Each endpoint in hello Web service is independently addressed, throttled, and managed.</span></span> <span data-ttu-id="4d926-109">Minden egyes végpont tooyour ügyfelek terjeszthető egyedi URL-cím és a hitelesítési kulcs esetében.</span><span class="sxs-lookup"><span data-stu-id="4d926-109">Each endpoint is a unique URL and authorization key that you can distribute tooyour customers.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="adding-endpoints-tooa-web-service"></a><span data-ttu-id="4d926-110">Végpontok tooa webszolgáltatás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4d926-110">Adding endpoints tooa Web service</span></span>
<span data-ttu-id="4d926-111">Nincsenek három módon tooadd egy végpont tooa webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="4d926-111">There are three ways tooadd an endpoint tooa Web service.</span></span>

* <span data-ttu-id="4d926-112">Automatizáltan</span><span class="sxs-lookup"><span data-stu-id="4d926-112">Programmatically</span></span>
* <span data-ttu-id="4d926-113">Hello Azure Machine Learning webszolgáltatások portálon keresztül</span><span class="sxs-lookup"><span data-stu-id="4d926-113">Through hello Azure Machine Learning Web Services portal</span></span>
* <span data-ttu-id="4d926-114">Ha a klasszikus Azure portálon hello</span><span class="sxs-lookup"><span data-stu-id="4d926-114">Though hello Azure classic portal</span></span>

<span data-ttu-id="4d926-115">Hello végpont létrehozását követően szokásokra is szinkron API-k, kötegelt API-k, segítségével, és az excel-munkalapokat.</span><span class="sxs-lookup"><span data-stu-id="4d926-115">Once hello endpoint is created, you can consume it through synchronous APIs, batch APIs, and excel worksheets.</span></span> <span data-ttu-id="4d926-116">Továbbá a felhasználói felületen keresztüli végpontokra tooadding, használhatja hello végpont felügyeleti API-k tooprogrammatically végpont-hozzáadáshoz.</span><span class="sxs-lookup"><span data-stu-id="4d926-116">In addition tooadding endpoints through this UI, you can also use hello Endpoint Management APIs tooprogrammatically add endpoints.</span></span>

> [!NOTE]
> <span data-ttu-id="4d926-117">Ha további végpontok toohello webszolgáltatás hozzáadott, hello alapértelmezett végpont nem törölhető.</span><span class="sxs-lookup"><span data-stu-id="4d926-117">If you have added additional endpoints toohello Web service, you cannot delete hello default endpoint.</span></span>
> 
> 

## <a name="adding-an-endpoint-programmatically"></a><span data-ttu-id="4d926-118">A végpont programozott módon hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4d926-118">Adding an endpoint programmatically</span></span>
<span data-ttu-id="4d926-119">Hozzáadhat egy végpont tooyour webszolgáltatás programozott módon hello [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) példakód.</span><span class="sxs-lookup"><span data-stu-id="4d926-119">You can add an endpoint tooyour Web service programmatically using hello [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) sample code.</span></span>

## <a name="adding-an-endpoint-using-hello-azure-machine-learning-web-services-portal"></a><span data-ttu-id="4d926-120">Hello Azure Machine Learning webszolgáltatások portálon végpont hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4d926-120">Adding an endpoint using hello Azure Machine Learning Web Services portal</span></span>
1. <span data-ttu-id="4d926-121">A Machine Learning Studio hello bal oldali oszlopban kattintson a webszolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="4d926-121">In Machine Learning Studio, on hello left navigation column, click Web Services.</span></span>
2. <span data-ttu-id="4d926-122">Hello Web service irányítópult hello alul kattintson **végpontok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="4d926-122">At hello bottom of hello Web service dashboard, click **Manage endpoints**.</span></span> <span data-ttu-id="4d926-123">hello Azure Machine Learning webszolgáltatások portál megnyitja a webszolgáltatás hello toohello végpontok oldalon.</span><span class="sxs-lookup"><span data-stu-id="4d926-123">hello Azure Machine Learning Web Services portal opens toohello endpoints page for hello Web service.</span></span>
3. <span data-ttu-id="4d926-124">Kattintson az **Új** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="4d926-124">Click **New**.</span></span>
4. <span data-ttu-id="4d926-125">Írja be nevét és leírását hello új végpont.</span><span class="sxs-lookup"><span data-stu-id="4d926-125">Type a name and description for hello new endpoint.</span></span> <span data-ttu-id="4d926-126">Végpontneveit 24 karakter vagy kevesebb hosszúságúnak kell lennie, és a magyar ábécét kisbetűket és számokat kell készíteni.</span><span class="sxs-lookup"><span data-stu-id="4d926-126">Endpoint names must be 24 character or less in length, and must be made up of lower-case alphabets or numbers.</span></span> <span data-ttu-id="4d926-127">Válassza ki a hello naplózási szint, és hogy engedélyezve van-e mintaadatokat.</span><span class="sxs-lookup"><span data-stu-id="4d926-127">Select hello logging level and whether sample data is enabled.</span></span> <span data-ttu-id="4d926-128">További információt a naplózást, [naplózását a Machine Learning webszolgáltatások](machine-learning-web-services-logging.md).</span><span class="sxs-lookup"><span data-stu-id="4d926-128">For more information on logging, see [Enable logging for Machine Learning Web services](machine-learning-web-services-logging.md).</span></span>

## <a name="adding-an-endpoint-using-hello-azure-classic-portal"></a><span data-ttu-id="4d926-129">A klasszikus Azure portálon hello segítségével a végpont hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4d926-129">Adding an endpoint using hello Azure classic portal</span></span>
1. <span data-ttu-id="4d926-130">Jelentkezzen be toohello [a klasszikus Azure portálon](http://manage.windowsazure.com), kattintson a **Machine Learning** hello bal oldali oszlopban.</span><span class="sxs-lookup"><span data-stu-id="4d926-130">Sign in toohello [Azure classic portal](http://manage.windowsazure.com), click **Machine Learning** in hello left column.</span></span> <span data-ttu-id="4d926-131">Kattintson a hello webes szolgáltatás, amelyben érdekli tartalmazó hello munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="4d926-131">Click hello workspace which contains hello Web service in which you are interested.</span></span>
   
    ![Keresse meg a tooworkspace](./media/machine-learning-create-endpoint/figure-1.png)
2. <span data-ttu-id="4d926-133">Kattintson a **webszolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="4d926-133">Click **Web Services**.</span></span>
   
    ![Keresse meg a tooWeb szolgáltatások](./media/machine-learning-create-endpoint/figure-2.png)
3. <span data-ttu-id="4d926-135">Kattintson az elérhető végpontok listáját toosee hello kíváncsiak vagyunk hello webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="4d926-135">Click hello Web service you're interested in toosee hello list of available endpoints.</span></span>
   
    ![Keresse meg a tooendpoint](./media/machine-learning-create-endpoint/figure-3.png)
4. <span data-ttu-id="4d926-137">Hello a hello lap alján, kattintson **végpont hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="4d926-137">At hello bottom of hello page, click **Add Endpoint**.</span></span> <span data-ttu-id="4d926-138">Írja be a nevet és leírást, ellenőrizze, hogy nincsenek az azonos név ennek a webszolgáltatásnak a hello nincsenek végpontok.</span><span class="sxs-lookup"><span data-stu-id="4d926-138">Type a name and description, ensure there are no other endpoints with hello same name in this Web service.</span></span> <span data-ttu-id="4d926-139">Amennyiben nincsenek különleges követelmények hello késleltetési szintjét hagyja az alapértelmezett értékre.</span><span class="sxs-lookup"><span data-stu-id="4d926-139">Leave hello throttle level with its default value unless you have special requirements.</span></span> <span data-ttu-id="4d926-140">toolearn szabályozással kapcsolatos további információkért lásd: [API-végpontok méretezése](machine-learning-scaling-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="4d926-140">toolearn more about throttling, see [Scaling API Endpoints](machine-learning-scaling-webservice.md).</span></span>
   
    ![-Végpont létrehozása](./media/machine-learning-create-endpoint/figure-4.png)

## <a name="next-steps"></a><span data-ttu-id="4d926-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4d926-142">Next Steps</span></span>
<span data-ttu-id="4d926-143">[Hogyan tooconsume az Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span><span class="sxs-lookup"><span data-stu-id="4d926-143">[How tooconsume an Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span></span>

