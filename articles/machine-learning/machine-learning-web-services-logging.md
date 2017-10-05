---
title: "A Machine Learning webszolgáltatások naplózása |} Microsoft Docs"
description: "Útmutató a Machine Learning webszolgáltatások naplózásának engedélyezéséről. Naplózási nyújt további információkat az API-k hibaelhárítás elősegítése érdekében."
services: machine-learning
documentationcenter: 
author: raymondlaghaeian
manager: jhubbard
editor: cgronlun
ms.assetid: c54d41e1-0300-46ef-bbfc-d6f7dca85086
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/15/2017
ms.author: raymondl;garye
ms.openlocfilehash: 7d0b2db01427430d6b0a317cdfefc265dd4b06e2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="enable-logging-for-machine-learning-web-services"></a><span data-ttu-id="ce1a9-104">A naplózás engedélyezése a Machine Learning webszolgáltatásokhoz</span><span class="sxs-lookup"><span data-stu-id="ce1a9-104">Enable logging for Machine Learning web services</span></span>
<span data-ttu-id="ce1a9-105">Ez a dokumentum tájékoztatást nyújt a Machine Learning webszolgáltatások a naplózási szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="ce1a9-105">This document provides information on the logging capability of Machine Learning web services.</span></span> <span data-ttu-id="ce1a9-106">Naplózási nyújt további információkat, csak olyan hiba száma és egy üzenetet, amely segíthet a hívások a Machine Learning API-k elhárításában.</span><span class="sxs-lookup"><span data-stu-id="ce1a9-106">Logging provides additional information, beyond just an error number and a message, that can help you troubleshoot your calls to the Machine Learning APIs.</span></span>  

## <a name="how-to-enable-logging-for-a-web-service"></a><span data-ttu-id="ce1a9-107">Az adott webszolgáltatás naplózásának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="ce1a9-107">How to enable logging for a Web service</span></span>

<span data-ttu-id="ce1a9-108">A naplózás engedélyezése a [Azure Machine Learning webszolgáltatások](https://services.azureml.net) portálon.</span><span class="sxs-lookup"><span data-stu-id="ce1a9-108">You enable logging from the [Azure Machine Learning Web Services](https://services.azureml.net) portal.</span></span> 

1. <span data-ttu-id="ce1a9-109">Jelentkezzen be az Azure Machine Learning webszolgáltatások portálon, a [https://services.azureml.net](https://services.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="ce1a9-109">Sign in to the Azure Machine Learning Web Services portal at [https://services.azureml.net](https://services.azureml.net).</span></span> <span data-ttu-id="ce1a9-110">A klasszikus webszolgáltatáshoz is kaphat a portálra kattintva **új webes felhasználói élményt** a Machine Learning webszolgáltatások oldalon a Machine Learning Studióban.</span><span class="sxs-lookup"><span data-stu-id="ce1a9-110">For a Classic web service, you can also get to the portal by clicking **New Web Services Experience** on the Machine Learning Web Services page in Machine Learning Studio.</span></span>

   ![Új webes felhasználói élményt hivatkozás](media/machine-learning-web-services-logging/new-web-services-experience-link.png)

2. <span data-ttu-id="ce1a9-112">A felső menüsávban kattintson **webszolgáltatások** egy új a webes szolgáltatás, vagy kattintson a **klasszikus webszolgáltatások** klasszikus webszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="ce1a9-112">On the top menu bar, click **Web Services** for a New web service, or click **Classic Web Services** for a Classic web service.</span></span>

   ![Válassza ki az új vagy a klasszikus webszolgáltatások](media/machine-learning-web-services-logging/select-web-service.png)

3. <span data-ttu-id="ce1a9-114">Új webszolgáltatás kattintson a webszolgáltatás nevét.</span><span class="sxs-lookup"><span data-stu-id="ce1a9-114">For a New web service, click the web service name.</span></span> <span data-ttu-id="ce1a9-115">A klasszikus webszolgáltatáshoz kattintson a webszolgáltatás nevét, és a következő lapon kattintson a megfelelő végpont.</span><span class="sxs-lookup"><span data-stu-id="ce1a9-115">For a Classic web service, click the web service name and then on the next page click the appropriate endpoint.</span></span>

4. <span data-ttu-id="ce1a9-116">A felső menüsávban kattintson **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="ce1a9-116">On the top menu bar, click **Configure**.</span></span>

5. <span data-ttu-id="ce1a9-117">Állítsa be a **naplózás engedélyezése** lehetőséggel *hiba* (csak a hibák bejelentkezni) vagy *összes* (a teljes körű naplózás).</span><span class="sxs-lookup"><span data-stu-id="ce1a9-117">Set the **Enable Logging** option to *Error* (to log only errors) or *All* (for full logging).</span></span>

   ![Válassza ki a naplózási szint](media/machine-learning-web-services-logging/enable-logging.png)

6. <span data-ttu-id="ce1a9-119">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="ce1a9-119">Click **Save**.</span></span>

7. <span data-ttu-id="ce1a9-120">Klasszikus webszolgáltatások, hozza létre a **ml-diagnosztika** tároló.</span><span class="sxs-lookup"><span data-stu-id="ce1a9-120">For Classic web services, create the **ml-diagnostics** container.</span></span>

   <span data-ttu-id="ce1a9-121">Minden web service naplóit őrizze meg a következő nevű blobtárolóban **ml-diagnosztika** a webszolgáltatás társított tárfiókban.</span><span class="sxs-lookup"><span data-stu-id="ce1a9-121">All web service logs are kept in a blob container named **ml-diagnostics** in the storage account associated with the web service.</span></span> <span data-ttu-id="ce1a9-122">Az új webszolgáltatások ebben a tárolóban van jön létre, a webszolgáltatás elérésére.</span><span class="sxs-lookup"><span data-stu-id="ce1a9-122">For New web services, this container is created the first time you access the web service.</span></span> <span data-ttu-id="ce1a9-123">Klasszikus webszolgáltatások kell létrehozni a tárolót, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="ce1a9-123">For Classic web services, you need to create the container if it doesn't already exist.</span></span> 

   1. <span data-ttu-id="ce1a9-124">Az a [Azure-portálon](https://portal.azure.com), keresse fel a webes szolgáltatáshoz tartozó tárfiók.</span><span class="sxs-lookup"><span data-stu-id="ce1a9-124">In the [Azure portal](https://portal.azure.com), go to the storage account associated with the web service.</span></span>

   2. <span data-ttu-id="ce1a9-125">A **Blob szolgáltatás**, kattintson a **tárolók**.</span><span class="sxs-lookup"><span data-stu-id="ce1a9-125">Under **Blob Service**, click **Containers**.</span></span>

   3. <span data-ttu-id="ce1a9-126">Ha a tároló **ml-diagnosztika** nem létezik, kattintson a **+ tároló**, adjon a tároló a neve "ml-diagnosztika", és válassza ki a **hozzáférési típus** mint "Blob".</span><span class="sxs-lookup"><span data-stu-id="ce1a9-126">If the container **ml-diagnostics** doesn't exist, click **+Container**, give the container the name "ml-diagnostics", and select the **Access type** as "Blob".</span></span> <span data-ttu-id="ce1a9-127">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="ce1a9-127">Click **OK**.</span></span>

      ![Válassza ki a naplózási szint](media/machine-learning-web-services-logging/create-ml-diagnostics-container.png)

> [!TIP]
>
> <span data-ttu-id="ce1a9-129">Az adott klasszikus webszolgáltatás a Web Services irányítópultján a Machine Learning Studióban is rendelkezik egy kapcsoló naplózás engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="ce1a9-129">For a Classic web service, the Web Services Dashboard in Machine Learning Studio also has a switch to enable logging.</span></span> <span data-ttu-id="ce1a9-130">Azonban naplózási most már felügyelt, a Web Services portálon keresztül, mert szeretné a portálon keresztül naplózását, ebben a cikkben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="ce1a9-130">However, because logging is now managed through the Web Services portal, you need to enable logging through the portal as described in this article.</span></span> <span data-ttu-id="ce1a9-131">Ha már engedélyezte Studio van bejelentkezve, a Web Services portálon, tiltsa le a naplózást, és engedélyezze újra.</span><span class="sxs-lookup"><span data-stu-id="ce1a9-131">If you already enabled logging in Studio, then in the Web Services Portal, disable logging and enable it again.</span></span>


## <a name="the-effects-of-enabling-logging"></a><span data-ttu-id="ce1a9-132">A naplózás engedélyezése hatásait</span><span class="sxs-lookup"><span data-stu-id="ce1a9-132">The effects of enabling logging</span></span>
<span data-ttu-id="ce1a9-133">Ha naplózása engedélyezve van, a diagnosztikai és a webszolgáltatás végpontja hibákat naplózza a **ml-diagnosztika** munkaterület a felhasználó kapcsolódik az Azure-Tárfiók a blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="ce1a9-133">When logging is enabled, the diagnostics and errors from the web service endpoint are logged in the **ml-diagnostics** blob container in the Azure Storage Account linked with the user’s workspace.</span></span> <span data-ttu-id="ce1a9-134">Ez a tároló összes a webszolgáltatás végpontjainak tároló fiókhoz társított összes munkaterületek tartozó összes diagnosztikai adatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ce1a9-134">This container holds all the diagnostics information for all the web service endpoints for all the workspaces associated with this storage account.</span></span>

<span data-ttu-id="ce1a9-135">A naplók bármely több rendelkezésre álló eszközök segítségével megismerheti az Azure-Tárfiók használatával tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="ce1a9-135">The logs can be viewed using any of the several tools available to explore an Azure Storage Account.</span></span> <span data-ttu-id="ce1a9-136">A legegyszerűbb lehet, hogy a lépjen a tárfiókhoz, az Azure portálon kattintson a **tárolók**, majd kattintson a tároló **ml-diagnosztika**.</span><span class="sxs-lookup"><span data-stu-id="ce1a9-136">The easiest may be to navigate to the storage account in the Azure portal, click **Containers**, and then click the container **ml-diagnostics**.</span></span>  

## <a name="log-blob-detail-information"></a><span data-ttu-id="ce1a9-137">Naplóadatok blob részletei</span><span class="sxs-lookup"><span data-stu-id="ce1a9-137">Log blob detail information</span></span>
<span data-ttu-id="ce1a9-138">Minden egyes blob a tárolóban pontosan a következő műveletek egyikét tartozó diagnosztikai adatokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="ce1a9-138">Each blob in the container holds the diagnostics information for exactly one of the following actions:</span></span>

* <span data-ttu-id="ce1a9-139">A Batch-végrehajtási metódus egy végrehajtása</span><span class="sxs-lookup"><span data-stu-id="ce1a9-139">An execution of the Batch-Execution method</span></span>  
* <span data-ttu-id="ce1a9-140">A kérés-válasz metódus egy végrehajtása</span><span class="sxs-lookup"><span data-stu-id="ce1a9-140">An execution of the Request-Response method</span></span>  
* <span data-ttu-id="ce1a9-141">A kérés-válasz tároló inicializálása</span><span class="sxs-lookup"><span data-stu-id="ce1a9-141">Initialization of a Request-Response container</span></span>

<span data-ttu-id="ce1a9-142">Minden egyes blob neve a következő előtag van:</span><span class="sxs-lookup"><span data-stu-id="ce1a9-142">The name of each blob has a prefix of the following form:</span></span> 


`{Workspace Id}-{Web service Id}-{Endpoint Id}/{Log type}`


<span data-ttu-id="ce1a9-143">Ha _típusú naplózása_ a következő értékek egyike:</span><span class="sxs-lookup"><span data-stu-id="ce1a9-143">Where _Log type_ is one of the following values:</span></span>  

* <span data-ttu-id="ce1a9-144">Kötegelt</span><span class="sxs-lookup"><span data-stu-id="ce1a9-144">batch</span></span>  
* <span data-ttu-id="ce1a9-145">pontszám/kérelem</span><span class="sxs-lookup"><span data-stu-id="ce1a9-145">score/requests</span></span>  
* <span data-ttu-id="ce1a9-146">pontszám/init</span><span class="sxs-lookup"><span data-stu-id="ce1a9-146">score/init</span></span>  

