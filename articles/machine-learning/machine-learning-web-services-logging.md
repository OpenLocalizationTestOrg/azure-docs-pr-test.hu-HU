---
title: "a Machine Learning webszolgáltatások aaaLogging |} Microsoft Docs"
description: "Megtudhatja, hogyan tooenable naplózási Machine Learning webszolgáltatások. Naplózási toohelp hibaelhárítása hello API-k további információt tartalmaz."
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
ms.openlocfilehash: ed23933d52d2151af658af2307d7df8743071f65
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-logging-for-machine-learning-web-services"></a><span data-ttu-id="16f4f-104">A naplózás engedélyezése a Machine Learning webszolgáltatásokhoz</span><span class="sxs-lookup"><span data-stu-id="16f4f-104">Enable logging for Machine Learning web services</span></span>
<span data-ttu-id="16f4f-105">Ez a dokumentum információkat biztosít a naplózási képesség Machine Learning webszolgáltatások hello.</span><span class="sxs-lookup"><span data-stu-id="16f4f-105">This document provides information on hello logging capability of Machine Learning web services.</span></span> <span data-ttu-id="16f4f-106">Naplózási nyújt további információkat, csak olyan hiba száma és egy üzenet, a hívások toohello Machine Learning API-k hibaelhárításához nyújt segítséget.</span><span class="sxs-lookup"><span data-stu-id="16f4f-106">Logging provides additional information, beyond just an error number and a message, that can help you troubleshoot your calls toohello Machine Learning APIs.</span></span>  

## <a name="how-tooenable-logging-for-a-web-service"></a><span data-ttu-id="16f4f-107">Hogyan tooenable naplózási az adott webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="16f4f-107">How tooenable logging for a Web service</span></span>

<span data-ttu-id="16f4f-108">Engedélyezi a naplózást a hello [Azure Machine Learning webszolgáltatások](https://services.azureml.net) portálon.</span><span class="sxs-lookup"><span data-stu-id="16f4f-108">You enable logging from hello [Azure Machine Learning Web Services](https://services.azureml.net) portal.</span></span> 

1. <span data-ttu-id="16f4f-109">Jelentkezzen be Azure Machine Learning webszolgáltatások portálon toohello, [https://services.azureml.net](https://services.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="16f4f-109">Sign in toohello Azure Machine Learning Web Services portal at [https://services.azureml.net](https://services.azureml.net).</span></span> <span data-ttu-id="16f4f-110">A klasszikus webszolgáltatáshoz is megkapható toohello portal kattintva **új webes felhasználói élményt** hello Machine Learning webszolgáltatások oldalon a Machine Learning Studióban.</span><span class="sxs-lookup"><span data-stu-id="16f4f-110">For a Classic web service, you can also get toohello portal by clicking **New Web Services Experience** on hello Machine Learning Web Services page in Machine Learning Studio.</span></span>

   ![Új webes felhasználói élményt hivatkozás](media/machine-learning-web-services-logging/new-web-services-experience-link.png)

2. <span data-ttu-id="16f4f-112">Hello felső menüsávban kattintson **webszolgáltatások** egy új a webes szolgáltatás, vagy kattintson a **klasszikus webszolgáltatások** klasszikus webszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="16f4f-112">On hello top menu bar, click **Web Services** for a New web service, or click **Classic Web Services** for a Classic web service.</span></span>

   ![Válassza ki az új vagy a klasszikus webszolgáltatások](media/machine-learning-web-services-logging/select-web-service.png)

3. <span data-ttu-id="16f4f-114">Új webszolgáltatás kattintson a hello webszolgáltatás nevét.</span><span class="sxs-lookup"><span data-stu-id="16f4f-114">For a New web service, click hello web service name.</span></span> <span data-ttu-id="16f4f-115">Az adott klasszikus webszolgáltatás kattintson hello webszolgáltatás neve majd a következő oldalon hello hello megfelelő végpont.</span><span class="sxs-lookup"><span data-stu-id="16f4f-115">For a Classic web service, click hello web service name and then on hello next page click hello appropriate endpoint.</span></span>

4. <span data-ttu-id="16f4f-116">Hello felső menüsávban kattintson **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="16f4f-116">On hello top menu bar, click **Configure**.</span></span>

5. <span data-ttu-id="16f4f-117">Set hello **naplózás engedélyezése** beállítás túl*hiba* (toolog csak a hibák) vagy *összes* (a teljes körű naplózás).</span><span class="sxs-lookup"><span data-stu-id="16f4f-117">Set hello **Enable Logging** option too*Error* (toolog only errors) or *All* (for full logging).</span></span>

   ![Válassza ki a naplózási szint](media/machine-learning-web-services-logging/enable-logging.png)

6. <span data-ttu-id="16f4f-119">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="16f4f-119">Click **Save**.</span></span>

7. <span data-ttu-id="16f4f-120">Klasszikus webszolgáltatások, hozza létre a hello **ml-diagnosztika** tároló.</span><span class="sxs-lookup"><span data-stu-id="16f4f-120">For Classic web services, create hello **ml-diagnostics** container.</span></span>

   <span data-ttu-id="16f4f-121">Minden web service naplóit őrizze meg a következő nevű blobtárolóban **ml-diagnosztika** hello webszolgáltatás társított hello tárfiókban.</span><span class="sxs-lookup"><span data-stu-id="16f4f-121">All web service logs are kept in a blob container named **ml-diagnostics** in hello storage account associated with hello web service.</span></span> <span data-ttu-id="16f4f-122">Az új webszolgáltatások, a tároló jön létre hello hello webszolgáltatás első megnyitásakor.</span><span class="sxs-lookup"><span data-stu-id="16f4f-122">For New web services, this container is created hello first time you access hello web service.</span></span> <span data-ttu-id="16f4f-123">A klasszikus webszolgáltatások kell toocreate hello tároló Ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="16f4f-123">For Classic web services, you need toocreate hello container if it doesn't already exist.</span></span> 

   1. <span data-ttu-id="16f4f-124">A hello [Azure-portálon](https://portal.azure.com)lépjen hello webszolgáltatás társított toohello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="16f4f-124">In hello [Azure portal](https://portal.azure.com), go toohello storage account associated with hello web service.</span></span>

   2. <span data-ttu-id="16f4f-125">A **Blob szolgáltatás**, kattintson a **tárolók**.</span><span class="sxs-lookup"><span data-stu-id="16f4f-125">Under **Blob Service**, click **Containers**.</span></span>

   3. <span data-ttu-id="16f4f-126">Ha hello tároló **ml-diagnosztika** nem létezik, kattintson a **+ tároló**, hello tároló hello neve "ml-diagnosztika" ad, és válassza ki a hello **hozzáférési típus** mint "Blob".</span><span class="sxs-lookup"><span data-stu-id="16f4f-126">If hello container **ml-diagnostics** doesn't exist, click **+Container**, give hello container hello name "ml-diagnostics", and select hello **Access type** as "Blob".</span></span> <span data-ttu-id="16f4f-127">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="16f4f-127">Click **OK**.</span></span>

      ![Válassza ki a naplózási szint](media/machine-learning-web-services-logging/create-ml-diagnostics-container.png)

> [!TIP]
>
> <span data-ttu-id="16f4f-129">Az adott klasszikus webszolgáltatás hello Web Services irányítópult a Machine Learning Studióban egy kapcsoló tooenable naplózás is rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="16f4f-129">For a Classic web service, hello Web Services Dashboard in Machine Learning Studio also has a switch tooenable logging.</span></span> <span data-ttu-id="16f4f-130">Azonban mivel naplózási most már felügyelt hello Web Services portálon keresztül, meg kell tooenable naplózás hello portálon ebben a cikkben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="16f4f-130">However, because logging is now managed through hello Web Services portal, you need tooenable logging through hello portal as described in this article.</span></span> <span data-ttu-id="16f4f-131">Ha már engedélyezte Studio van bejelentkezve, hello Web Services portálra, tiltsa le a naplózást, és engedélyezze újra.</span><span class="sxs-lookup"><span data-stu-id="16f4f-131">If you already enabled logging in Studio, then in hello Web Services Portal, disable logging and enable it again.</span></span>


## <a name="hello-effects-of-enabling-logging"></a><span data-ttu-id="16f4f-132">hello hatásait naplózás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="16f4f-132">hello effects of enabling logging</span></span>
<span data-ttu-id="16f4f-133">Ha naplózása engedélyezve van, hello diagnosztika és hello webszolgáltatási végpontot hibákat naplózza hello **ml-diagnosztika** hello Azure-Tárfiók a blob-tároló hello felhasználói munkaterület kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="16f4f-133">When logging is enabled, hello diagnostics and errors from hello web service endpoint are logged in hello **ml-diagnostics** blob container in hello Azure Storage Account linked with hello user’s workspace.</span></span> <span data-ttu-id="16f4f-134">Ez a tároló összes hello webszolgáltatás végpontjainak tároló fiókhoz társított összes hello munkaterületek tartozó összes hello diagnosztikai adatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="16f4f-134">This container holds all hello diagnostics information for all hello web service endpoints for all hello workspaces associated with this storage account.</span></span>

<span data-ttu-id="16f4f-135">hello naplók bármely hello több eszközök elérhető tooexplore egy Azure Storage-fiók használatával tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="16f4f-135">hello logs can be viewed using any of hello several tools available tooexplore an Azure Storage Account.</span></span> <span data-ttu-id="16f4f-136">hello legegyszerűbb előfordulhat, hogy toonavigate toohello tárfiók hello Azure-portálon kell, kattintson a **tárolók**, majd kattintson a hello tároló **ml-diagnosztika**.</span><span class="sxs-lookup"><span data-stu-id="16f4f-136">hello easiest may be toonavigate toohello storage account in hello Azure portal, click **Containers**, and then click hello container **ml-diagnostics**.</span></span>  

## <a name="log-blob-detail-information"></a><span data-ttu-id="16f4f-137">Naplóadatok blob részletei</span><span class="sxs-lookup"><span data-stu-id="16f4f-137">Log blob detail information</span></span>
<span data-ttu-id="16f4f-138">Minden egyes blob hello tárolóban hello diagnosztikai adatokat a hello a következő műveletek egyikét tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="16f4f-138">Each blob in hello container holds hello diagnostics information for exactly one of hello following actions:</span></span>

* <span data-ttu-id="16f4f-139">Egy hello Batch-végrehajtási mód végrehajtását</span><span class="sxs-lookup"><span data-stu-id="16f4f-139">An execution of hello Batch-Execution method</span></span>  
* <span data-ttu-id="16f4f-140">Egy kérés-válasz hello metódus végrehajtása</span><span class="sxs-lookup"><span data-stu-id="16f4f-140">An execution of hello Request-Response method</span></span>  
* <span data-ttu-id="16f4f-141">A kérés-válasz tároló inicializálása</span><span class="sxs-lookup"><span data-stu-id="16f4f-141">Initialization of a Request-Response container</span></span>

<span data-ttu-id="16f4f-142">minden egyes blob hello nevét a következő űrlap hello előtaggal van:</span><span class="sxs-lookup"><span data-stu-id="16f4f-142">hello name of each blob has a prefix of hello following form:</span></span> 


`{Workspace Id}-{Web service Id}-{Endpoint Id}/{Log type}`


<span data-ttu-id="16f4f-143">Ha _típusú naplózása_ hello a következő értékek egyike:</span><span class="sxs-lookup"><span data-stu-id="16f4f-143">Where _Log type_ is one of hello following values:</span></span>  

* <span data-ttu-id="16f4f-144">Kötegelt</span><span class="sxs-lookup"><span data-stu-id="16f4f-144">batch</span></span>  
* <span data-ttu-id="16f4f-145">pontszám/kérelem</span><span class="sxs-lookup"><span data-stu-id="16f4f-145">score/requests</span></span>  
* <span data-ttu-id="16f4f-146">pontszám/init</span><span class="sxs-lookup"><span data-stu-id="16f4f-146">score/init</span></span>  

