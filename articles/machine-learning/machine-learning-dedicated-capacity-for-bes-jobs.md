---
title: "A Machine Learning kötegelt végrehajtási szolgáltatás feladatok kapacitás dedikált |} Microsoft Docs"
description: "A gépi tanulási feladatok Azure Batch-szolgáltatások áttekintése."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.service: machine-learning
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: 3879eb3d0c6fa9d74fff01b07f5c07d3991dfbbd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-batch-service-for-machine-learning-jobs"></a><span data-ttu-id="6d831-103">A gépi tanulási feladatok az Azure Batch szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="6d831-103">Azure Batch service for Machine Learning jobs</span></span>

<span data-ttu-id="6d831-104">Gépi tanulás Batch-készlet feldolgozása ügyfél által felügyelt méretezési biztosít az Azure Machine Learning kötegelt végrehajtási szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="6d831-104">Machine Learning Batch Pool processing provides customer-managed scale for the Azure Machine Learning Batch Execution Service.</span></span> <span data-ttu-id="6d831-105">Klasszikus kötegelt feldolgozása gépi tanulás kerül sor, több-bérlős környezetben, amely korlátozza az egyidejűleg futó feladatainak számát akkor is elküldhetik, és a feladatok várólistára első-first out alapon.</span><span class="sxs-lookup"><span data-stu-id="6d831-105">Classic batch processing for machine learning takes place in a multi-tenant environment, which limits the number of concurrent jobs you can submit, and jobs are queued on a first-in-first-out basis.</span></span> <span data-ttu-id="6d831-106">Ez a bizonytalanság azt jelenti, hogy Ön nem pontosan előre jelezni mikor fog futni a feladat.</span><span class="sxs-lookup"><span data-stu-id="6d831-106">This uncertainty means that you can't accurately predict when your job will run.</span></span>

<span data-ttu-id="6d831-107">Készlet kötegfeldolgozási hozhat létre készleteket, amelyre elküldheti a kötegelt feladatok.</span><span class="sxs-lookup"><span data-stu-id="6d831-107">Batch Pool processing allows you to create pools on which you can submit batch jobs.</span></span> <span data-ttu-id="6d831-108">Szabályozhatja a készlet méretét, és mely készletbe a feladat elküldése megtörtént.</span><span class="sxs-lookup"><span data-stu-id="6d831-108">You control the size of the pool, and to which pool the job is submitted.</span></span> <span data-ttu-id="6d831-109">A BES feladat fut, a saját feldolgozási terület előre jelezhető feldolgozás és képes létrehozni a feldolgozási terhelés elküldött megfelelő erőforráskészletek biztosítása.</span><span class="sxs-lookup"><span data-stu-id="6d831-109">Your BES job runs in its own processing space providing predictable processing performance and the ability to create resource pools that correspond to the processing load that you submit.</span></span>

## <a name="how-to-use-batch-pool-processing"></a><span data-ttu-id="6d831-110">Batch-készlet feldolgozási használata</span><span class="sxs-lookup"><span data-stu-id="6d831-110">How to use Batch Pool processing</span></span>

<span data-ttu-id="6d831-111">A Batch-készlet feldolgozási konfigurációs már nem érhető el az Azure portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="6d831-111">Configuration of Batch Pool Processing is not currently available through the Azure portal.</span></span> <span data-ttu-id="6d831-112">Batch-készlet feldolgozási használatához az alábbiak szükségesek:</span><span class="sxs-lookup"><span data-stu-id="6d831-112">To use Batch Pool processing, you must:</span></span>

-   <span data-ttu-id="6d831-113">Készlet Batch-fiók létrehozása, és szerezze be a készlet szolgáltatás URL-címe és engedélykulcs CSS hívása</span><span class="sxs-lookup"><span data-stu-id="6d831-113">Call CSS to create a Batch Pool Account and obtain a Pool Service URL and an authorization key</span></span>
-   <span data-ttu-id="6d831-114">Hozzon létre egy új erőforrás-kezelő alapú webszolgáltatás és a számlázási csomag</span><span class="sxs-lookup"><span data-stu-id="6d831-114">Create a New Resource Manager based web service and billing plan</span></span>

<span data-ttu-id="6d831-115">A fiók létrehozásához, hívja a Microsoft ügyfélszolgálata és a támogatási szolgálathoz (CSS), és adja meg az előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="6d831-115">To create your account, call Microsoft Customer Service and Support (CSS) and provide your Subscription ID.</span></span> <span data-ttu-id="6d831-116">CSS Önnel együttműködve helyzetnek megfelelő kapacitásának meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="6d831-116">CSS will work with you to determine the appropriate capacity for your scenario.</span></span> <span data-ttu-id="6d831-117">CSS ezután konfigurálja a fiókot hozhat létre készleteket maximális számát és a virtuális gépek (VM) elhelyezheti mindegyik készlet maximális számát.</span><span class="sxs-lookup"><span data-stu-id="6d831-117">CSS then configures your account with the maximum number of pools you can create and the maximum number of virtual machines (VMs) that you can place in each pool.</span></span> <span data-ttu-id="6d831-118">A fiók van konfigurálva, ha rendelkezésre állnak a készlet szolgáltatás URL-címe és engedélykulcs.</span><span class="sxs-lookup"><span data-stu-id="6d831-118">Once your account is configured, you are provided your Pool Service URL and an authorization key.</span></span>

<span data-ttu-id="6d831-119">A fiók létrehozását követően a készlet szolgáltatás URL-címe és a hitelesítési kulcs használatával készlet felügyeleti műveleteket a Batch-készlet.</span><span class="sxs-lookup"><span data-stu-id="6d831-119">After your account is created, you use the Pool Service URL and authorization key to perform pool management operations on your Batch Pool.</span></span>

![Kötegelt készlet szolgáltatás architektúrája.](media/machine-learning-dedicated-capacity-for-bes-jobs/pool-architecture.png)

<span data-ttu-id="6d831-121">A készlet szolgáltatás URL-címe, amely az Ön számára biztosított CSS-készlet létrehozása művelet hívása készletek létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="6d831-121">You create pools by calling the Create Pool operation on the pool service URL that CSS provided to you.</span></span> <span data-ttu-id="6d831-122">Készletet hoz létre, amikor adja meg, hány virtuális gépek és a swagger.json egy új Resource Manager URL-Machine Learning webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="6d831-122">When you create a pool, specify the number of VMs and the URL of the swagger.json of a New Resource Manager based Machine Learning web service.</span></span> <span data-ttu-id="6d831-123">Ennek a webszolgáltatásnak a számlázási társítás létrehozásához valósul meg.</span><span class="sxs-lookup"><span data-stu-id="6d831-123">This web service is provided to establish the billing association.</span></span> <span data-ttu-id="6d831-124">A Batch-készlet szolgáltatás a swagger.json használ a tárolókészlet társítsa egy számlázási csomagot.</span><span class="sxs-lookup"><span data-stu-id="6d831-124">The Batch Pool service uses the swagger.json to associate the pool with a billing plan.</span></span> <span data-ttu-id="6d831-125">Futtathatja a BES webszolgáltatás, mindkét alapú új erőforrás-kezelő és a klasszikus, a készlet.</span><span class="sxs-lookup"><span data-stu-id="6d831-125">You can run any BES web service, both New Resource Manager based and classic, you choose on the pool.</span></span>

<span data-ttu-id="6d831-126">Bármely új Resource Manager-alapú webszolgáltatás, de vegye figyelembe, hogy a feladatokhoz tartozó számlázási terhére-e a szolgáltatáshoz tartozó számlázási csomagot.</span><span class="sxs-lookup"><span data-stu-id="6d831-126">You can use any New Resource Manager based web service, but be aware that the billing for the jobs are charged against the billing plan associated with that service.</span></span> <span data-ttu-id="6d831-127">Érdemes lehet egy webszolgáltatás és az új számlázási csomag kifejezetten a futó feladatok Batch-készlet létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="6d831-127">You may want to create a web service and new billing plan specifically for running Batch Pool jobs.</span></span>

<span data-ttu-id="6d831-128">Webszolgáltatások létrehozásával kapcsolatos további információkért lásd: [központi telepítése az Azure Machine Learning webszolgáltatás](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="6d831-128">For more information on creating web services, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>

<span data-ttu-id="6d831-129">Miután létrehozta a készletben, a a kötegelt kérelem URL-cím segítségével a webszolgáltatás BES feladat elküldése.</span><span class="sxs-lookup"><span data-stu-id="6d831-129">Once you have created a pool, you submit the BES job using the Batch Requests URL for the web service.</span></span> <span data-ttu-id="6d831-130">Dönthet úgy, hogy küldje el a készlethez, vagy a klasszikus kötegfeldolgozási.</span><span class="sxs-lookup"><span data-stu-id="6d831-130">You can choose to submit it to a pool or to classic batch processing.</span></span> <span data-ttu-id="6d831-131">A Batch-készlet feldolgozása a feladat elküldése, a következő paraméter a feladat elküldése kérelemtörzset hozzáadhat:</span><span class="sxs-lookup"><span data-stu-id="6d831-131">To submit a job to Batch Pool processing, you add the following parameter to the job submission request body:</span></span>

<span data-ttu-id="6d831-132">"AzureBatchPoolId": "&lt;azonosító tárolókészlet&gt;"</span><span class="sxs-lookup"><span data-stu-id="6d831-132">"AzureBatchPoolId":"&lt;pool ID&gt;"</span></span>

<span data-ttu-id="6d831-133">Ha nem adja hozzá a paramétert, a feladat a klasszikus kötegelt folyamat-környezetében futtatható.</span><span class="sxs-lookup"><span data-stu-id="6d831-133">If you do not add the parameter, the job is run in the classic batch process environment.</span></span> <span data-ttu-id="6d831-134">A készlet elegendő erőforrás áll rendelkezésre, ha a a feladat futtatása azonnal elindul.</span><span class="sxs-lookup"><span data-stu-id="6d831-134">If the pool has available resources, the job starts running immediately.</span></span> <span data-ttu-id="6d831-135">A készlet nem rendelkezik szabad erőforrást, ha a feladat várólistára van állítva, amíg egy erőforrás áll rendelkezésre.</span><span class="sxs-lookup"><span data-stu-id="6d831-135">If the pool does not have free resources, your job is queued until a resource is available.</span></span>

<span data-ttu-id="6d831-136">Ha talál meg, hogy rendszeresen eléri a készletek kapacitását, és nagyobb kapacitást van szüksége, CSS hívja, és növelje a kvóták képviselője dolgozni.</span><span class="sxs-lookup"><span data-stu-id="6d831-136">If you find that you regularly reach the capacity of your pools, and you need increased capacity, you can call CSS and work with a representative to increase your quotas.</span></span>

<span data-ttu-id="6d831-137">Példa egy kérelem:</span><span class="sxs-lookup"><span data-stu-id="6d831-137">Example Request:</span></span>

<span data-ttu-id="6d831-138">https://ussouthcentral.Services.azureml.NET/Subscriptions/80c77c7674ba4c8c82294c3b2957990c/Services/9fe659022c9747e3b9b7b923c3830623/Jobs?API-Version=2.0</span><span class="sxs-lookup"><span data-stu-id="6d831-138">https://ussouthcentral.services.azureml.net/subscriptions/80c77c7674ba4c8c82294c3b2957990c/services/9fe659022c9747e3b9b7b923c3830623/jobs?api-version=2.0</span></span>

```json
{

    "Input":{
    
        "ConnectionString":"DefaultEndpointsProtocol=https;BlobEndpoint=https://sampleaccount.blob.core.windows.net/;TableEndpoint
        =https://sampleaccount.table.core.windows.net/;QueueEndpoint=https://sampleaccount.queue.core.windows.net/;FileEndpoint=https://zhguim
        l.file.core.windows.net/;AccountName=sampleaccount;AccountKey=&lt;Key&gt;;",
        
        "BaseLocation":null,
        
        "RelativeLocation":"testint/AdultCensusIncomeBinaryClassificationDataset.csv",
        
        "SasBlobToken":null
    
    },
    
    "GlobalParameters":{ },
    
    "Outputs":{
    
        "output1":{
        
            "ConnectionString":"DefaultEndpointsProtocol=https;BlobEndpoint=https://sampleaccount.blob.core.windows.net/;TableEndpo
            int=https://sampleaccount.table.core.windows.net/;QueueEndpoint=https://sampleaccount.queue.core.windows.net/;FileEndpoint=https://sampleaccount.file.core.windows.net/;AccountName=sampleaccount;AccountKey=&lt;Key&gt;",
            "BaseLocation":null,
            "RelativeLocation":"testintoutput/testint\_results.csv",
            
            "SasBlobToken":null
        
        }
    
    },
    
    "AzureBatchPoolId":"8dfc151b0d3e446497b845f3b29ef53b"

}
```

## <a name="considerations-when-using-batch-pool-processing"></a><span data-ttu-id="6d831-139">Batch-készlet feldolgozási használatának szempontjai</span><span class="sxs-lookup"><span data-stu-id="6d831-139">Considerations when using Batch Pool processing</span></span>

<span data-ttu-id="6d831-140">Batch-készlet feldolgozásakor – a számlázható szolgáltatás pedig, amelyhez Ön társítandó Resource Manageren alapul számlázási csomagot.</span><span class="sxs-lookup"><span data-stu-id="6d831-140">Batch Pool Processing is an always-on billable service and that requires you to associate it with a Resource Manager based billing plan.</span></span> <span data-ttu-id="6d831-141">A készlet fut; számítási órák számát csak számlázása függetlenül a feladatok futnak a készlethez tartozó idő során.</span><span class="sxs-lookup"><span data-stu-id="6d831-141">You are only billed for the number of compute hours the pool is running; regardless of the number of jobs run during that time pool.</span></span> <span data-ttu-id="6d831-142">Készletet hoz létre, ha kell fizetni az egyes virtuális gépek a készlet számítási órában a készlet nem törlik, még akkor is, ha nincsenek kötegelt feladatok futnak a készletben.</span><span class="sxs-lookup"><span data-stu-id="6d831-142">If you create a pool, you are billed for the compute hours of each virtual machine in the pool until the pool is deleted, even if no batch jobs are running in the pool.</span></span> <span data-ttu-id="6d831-143">A virtuális gépek számlázási akkor kezdődik, amikor a kiépítés után, és leáll, amikor törölve lettek.</span><span class="sxs-lookup"><span data-stu-id="6d831-143">Billing for the virtual machines starts when they have finished provisioning and stops when they have been deleted.</span></span> <span data-ttu-id="6d831-144">Használhatja a sémák megtalálható-e a [Machine Learning díjszabás lap](https://azure.microsoft.com/pricing/details/machine-learning/).</span><span class="sxs-lookup"><span data-stu-id="6d831-144">You can use any of the plans found on the [Machine Learning Pricing page](https://azure.microsoft.com/pricing/details/machine-learning/).</span></span>

<span data-ttu-id="6d831-145">Számlázási. példa:</span><span class="sxs-lookup"><span data-stu-id="6d831-145">Billing example:</span></span>

<span data-ttu-id="6d831-146">Ha a Batch-készlet létrehozása 2 virtuális gépekkel és 24 óra múlva törli azt a számlázási csomag terhelt számítási 48 óra; függetlenül attól, hogy hány feladat futtatná adott időszakban.</span><span class="sxs-lookup"><span data-stu-id="6d831-146">If you create a Batch Pool with 2 virtual machines and delete it after 24 hours your billing plan is debited 48 compute hours; regardless of how many jobs were run during that period.</span></span>

<span data-ttu-id="6d831-147">Ha a Batch-készlet létrehozása 4 virtuális gépekkel és 12 óra elteltével törölje, a számlázási csomag is számítási terhelést 48 óra.</span><span class="sxs-lookup"><span data-stu-id="6d831-147">If you create a Batch Pool with 4 virtual machines and delete it after 12 hours, your billing plan is also debited 48 compute hours.</span></span>

<span data-ttu-id="6d831-148">Azt javasoljuk, hogy a feladatok teljes meghatározásához a feladatállapot lekérdezésére.</span><span class="sxs-lookup"><span data-stu-id="6d831-148">We recommend that you poll the job status to determine when jobs complete.</span></span> <span data-ttu-id="6d831-149">Ha a feladatok futása befejeződött, hívja meg a készlet átméretezése műveletet a virtuális gépek számának beállítása a tárolókészlet nulla.</span><span class="sxs-lookup"><span data-stu-id="6d831-149">When all your jobs have finished running, call the Resize Pool operation to set the number of virtual machines in the pool to zero.</span></span> <span data-ttu-id="6d831-150">Ha rövid készlet erőforrások és kell létrehoznia egy új készletet, például egy másik számlázási csomag elleni számlázási törölheti a készlet helyette ha összes feladat futása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="6d831-150">If you are short on pool resources and you need to create a new pool, for example to bill against a different billing plan, you can delete the pool instead when all your jobs have finished running.</span></span>


| <span data-ttu-id="6d831-151">**Használja a kötegelt mikor feldolgozása**</span><span class="sxs-lookup"><span data-stu-id="6d831-151">**Use Batch Pool Processing when**</span></span>    | <span data-ttu-id="6d831-152">**Használja a klasszikus kötegfeldolgozási végezhető, ha**</span><span class="sxs-lookup"><span data-stu-id="6d831-152">**Use classic batch processing when**</span></span>  |
|---|---|
|<span data-ttu-id="6d831-153">Nagyszámú feladatok futtatásához szükséges</span><span class="sxs-lookup"><span data-stu-id="6d831-153">You need to run a large number of jobs</span></span><br><span data-ttu-id="6d831-154">Vagy</span><span class="sxs-lookup"><span data-stu-id="6d831-154">Or</span></span><br/><span data-ttu-id="6d831-155">A feladatok azonnal elindul tudnia kell</span><span class="sxs-lookup"><span data-stu-id="6d831-155">You need to know that your jobs will run immediately</span></span><br/><span data-ttu-id="6d831-156">Vagy</span><span class="sxs-lookup"><span data-stu-id="6d831-156">Or</span></span><br/><span data-ttu-id="6d831-157">Garantált átviteli van szüksége.</span><span class="sxs-lookup"><span data-stu-id="6d831-157">You need guaranteed throughput.</span></span> <span data-ttu-id="6d831-158">Például szeretné egy adott időszakon feladatok száma futnak, és szeretné, hogy a számítási erőforrásokat, az igényeknek horizontális.</span><span class="sxs-lookup"><span data-stu-id="6d831-158">For example, you need to run a number of jobs in a given time frame, and want to scale out your compute resources to meet your needs.</span></span>    | <span data-ttu-id="6d831-159">Néhány feladatot futtat</span><span class="sxs-lookup"><span data-stu-id="6d831-159">You are running just a few jobs</span></span><br/><span data-ttu-id="6d831-160">És</span><span class="sxs-lookup"><span data-stu-id="6d831-160">And</span></span><br/> <span data-ttu-id="6d831-161">Nem szükséges azonnal futtatni a feladatot</span><span class="sxs-lookup"><span data-stu-id="6d831-161">You don’t need the jobs to run immediately</span></span> |
