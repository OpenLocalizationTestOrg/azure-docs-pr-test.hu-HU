---
title: "a Machine Learning kötegelt végrehajtási szolgáltatás feladatok kapacitás aaaDedicated |} Microsoft Docs"
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
ms.openlocfilehash: bba7970bb31c50e5b0b9d5f4ff4f0d2dacf942e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-batch-service-for-machine-learning-jobs"></a><span data-ttu-id="5d59b-103">A gépi tanulási feladatok az Azure Batch szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="5d59b-103">Azure Batch service for Machine Learning jobs</span></span>

<span data-ttu-id="5d59b-104">Gépi tanulás Batch-készlet feldolgozása ügyfél által felügyelt méretezési biztosít hello Azure Machine Learning kötegelt végrehajtási szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="5d59b-104">Machine Learning Batch Pool processing provides customer-managed scale for hello Azure Machine Learning Batch Execution Service.</span></span> <span data-ttu-id="5d59b-105">A machine Learning szolgáltatáshoz klasszikus kötegfeldolgozási egy több-bérlős környezet, mely korlátok hello száma egyidejűleg futó feladatainak is elküldhetik, és feladatok várólistára első-first out alapon történik.</span><span class="sxs-lookup"><span data-stu-id="5d59b-105">Classic batch processing for machine learning takes place in a multi-tenant environment, which limits hello number of concurrent jobs you can submit, and jobs are queued on a first-in-first-out basis.</span></span> <span data-ttu-id="5d59b-106">Ez a bizonytalanság azt jelenti, hogy Ön nem pontosan előre jelezni mikor fog futni a feladat.</span><span class="sxs-lookup"><span data-stu-id="5d59b-106">This uncertainty means that you can't accurately predict when your job will run.</span></span>

<span data-ttu-id="5d59b-107">Kötegfeldolgozási készlet lehetővé teszi toocreate készletek, amelyre elküldheti a kötegelt feladatok.</span><span class="sxs-lookup"><span data-stu-id="5d59b-107">Batch Pool processing allows you toocreate pools on which you can submit batch jobs.</span></span> <span data-ttu-id="5d59b-108">Hello készlet mérete hello szabályozhatja, és toowhich készlet hello feladat küldése.</span><span class="sxs-lookup"><span data-stu-id="5d59b-108">You control hello size of hello pool, and toowhich pool hello job is submitted.</span></span> <span data-ttu-id="5d59b-109">A BES feladat fut, a saját területet biztosít feldolgozása előre jelezhető feldolgozás és hello képességét toocreate-erőforráskészleteket, amelyekben felel meg az elküldött toohello feldolgozási terhelés.</span><span class="sxs-lookup"><span data-stu-id="5d59b-109">Your BES job runs in its own processing space providing predictable processing performance and hello ability toocreate resource pools that correspond toohello processing load that you submit.</span></span>

## <a name="how-toouse-batch-pool-processing"></a><span data-ttu-id="5d59b-110">Hogyan toouse Batch-készlet feldolgozása</span><span class="sxs-lookup"><span data-stu-id="5d59b-110">How toouse Batch Pool processing</span></span>

<span data-ttu-id="5d59b-111">A Batch-készlet feldolgozási konfigurációs már nem érhető el hello Azure-portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="5d59b-111">Configuration of Batch Pool Processing is not currently available through hello Azure portal.</span></span> <span data-ttu-id="5d59b-112">Batch-készlet toouse feldolgozása, meg kell:</span><span class="sxs-lookup"><span data-stu-id="5d59b-112">toouse Batch Pool processing, you must:</span></span>

-   <span data-ttu-id="5d59b-113">Meghívja a CSS toocreate készlet Batch-fiók, és szerezze be a készlet szolgáltatás URL-címe és engedélykulcs</span><span class="sxs-lookup"><span data-stu-id="5d59b-113">Call CSS toocreate a Batch Pool Account and obtain a Pool Service URL and an authorization key</span></span>
-   <span data-ttu-id="5d59b-114">Hozzon létre egy új erőforrás-kezelő alapú webszolgáltatás és a számlázási csomag</span><span class="sxs-lookup"><span data-stu-id="5d59b-114">Create a New Resource Manager based web service and billing plan</span></span>

<span data-ttu-id="5d59b-115">toocreate a fiókjához, hívja a Microsoft ügyfélszolgálata és a támogatási szolgálathoz (CSS), és adja meg, az előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="5d59b-115">toocreate your account, call Microsoft Customer Service and Support (CSS) and provide your Subscription ID.</span></span> <span data-ttu-id="5d59b-116">CSS kapacitással rendelkező átjáróeszközt, toodetermine hello megfelelő helyzetnek fog működni.</span><span class="sxs-lookup"><span data-stu-id="5d59b-116">CSS will work with you toodetermine hello appropriate capacity for your scenario.</span></span> <span data-ttu-id="5d59b-117">CSS ezután konfigurálja a fiók hello készleteket hozhat létre és virtuális gépek (VM) elhelyezheti mindegyik készlet maximális száma hello maximális számát.</span><span class="sxs-lookup"><span data-stu-id="5d59b-117">CSS then configures your account with hello maximum number of pools you can create and hello maximum number of virtual machines (VMs) that you can place in each pool.</span></span> <span data-ttu-id="5d59b-118">A fiók van konfigurálva, ha rendelkezésre állnak a készlet szolgáltatás URL-címe és engedélykulcs.</span><span class="sxs-lookup"><span data-stu-id="5d59b-118">Once your account is configured, you are provided your Pool Service URL and an authorization key.</span></span>

<span data-ttu-id="5d59b-119">Miután a fiókja létrejött, az hello készlet szolgáltatás URL-címe és a hitelesítési kulcs tooperform készlet a felügyeleti műveleteihez a Batch-készlet.</span><span class="sxs-lookup"><span data-stu-id="5d59b-119">After your account is created, you use hello Pool Service URL and authorization key tooperform pool management operations on your Batch Pool.</span></span>

![Kötegelt készlet szolgáltatás architektúrája.](media/machine-learning-dedicated-capacity-for-bes-jobs/pool-architecture.png)

<span data-ttu-id="5d59b-121">Hello készlet létrehozása művelet hello készlet szolgáltatás URL-CÍMÉT, hogy a megadott CSS tooyou meghívásával készletek hoz létre.</span><span class="sxs-lookup"><span data-stu-id="5d59b-121">You create pools by calling hello Create Pool operation on hello pool service URL that CSS provided tooyou.</span></span> <span data-ttu-id="5d59b-122">Készletet hoz létre, amikor adja meg, virtuális gépek és hello URL-CÍMÉT egy új Resource Manager hello swagger.json hello száma Machine Learning webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="5d59b-122">When you create a pool, specify hello number of VMs and hello URL of hello swagger.json of a New Resource Manager based Machine Learning web service.</span></span> <span data-ttu-id="5d59b-123">Ez a webszolgáltatás tooestablish hello számlázási társítás valósul meg.</span><span class="sxs-lookup"><span data-stu-id="5d59b-123">This web service is provided tooestablish hello billing association.</span></span> <span data-ttu-id="5d59b-124">hello Batch-készlet szolgáltatás hello swagger.json tooassociate hello készletet használ egy számlázási csomagot.</span><span class="sxs-lookup"><span data-stu-id="5d59b-124">hello Batch Pool service uses hello swagger.json tooassociate hello pool with a billing plan.</span></span> <span data-ttu-id="5d59b-125">Futtathatja a BES webszolgáltatás, mindkét alapú új erőforrás-kezelő és a klasszikus, hello készletében.</span><span class="sxs-lookup"><span data-stu-id="5d59b-125">You can run any BES web service, both New Resource Manager based and classic, you choose on hello pool.</span></span>

<span data-ttu-id="5d59b-126">Bármely új Resource Manager-alapú webszolgáltatás, de vegye figyelembe, hogy hello számlázási hello feladatok elleni hello szolgáltatáshoz tartozó számlázási csomag van-e szó.</span><span class="sxs-lookup"><span data-stu-id="5d59b-126">You can use any New Resource Manager based web service, but be aware that hello billing for hello jobs are charged against hello billing plan associated with that service.</span></span> <span data-ttu-id="5d59b-127">Érdemes lehet egy webszolgáltatás és az új számlázási megtervezése toocreate kifejezetten a futó feladatok Batch-készlet.</span><span class="sxs-lookup"><span data-stu-id="5d59b-127">You may want toocreate a web service and new billing plan specifically for running Batch Pool jobs.</span></span>

<span data-ttu-id="5d59b-128">Webszolgáltatások létrehozásával kapcsolatos további információkért lásd: [központi telepítése az Azure Machine Learning webszolgáltatás](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="5d59b-128">For more information on creating web services, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>

<span data-ttu-id="5d59b-129">Miután létrehozta a készletben, elküldését hello BES hello webszolgáltatás feladat használ hello kötegelt kérelmek URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="5d59b-129">Once you have created a pool, you submit hello BES job using hello Batch Requests URL for hello web service.</span></span> <span data-ttu-id="5d59b-130">Kiválaszthatja a toosubmit azt tooa készletet vagy tooclassic kötegfeldolgozási.</span><span class="sxs-lookup"><span data-stu-id="5d59b-130">You can choose toosubmit it tooa pool or tooclassic batch processing.</span></span> <span data-ttu-id="5d59b-131">egy feladat tooBatch készlet feldolgozása toosubmit, hozzáadása a következő paraméter toohello feladat elküldése kérelemtörzset hello:</span><span class="sxs-lookup"><span data-stu-id="5d59b-131">toosubmit a job tooBatch Pool processing, you add hello following parameter toohello job submission request body:</span></span>

<span data-ttu-id="5d59b-132">"AzureBatchPoolId": "&lt;azonosító tárolókészlet&gt;"</span><span class="sxs-lookup"><span data-stu-id="5d59b-132">"AzureBatchPoolId":"&lt;pool ID&gt;"</span></span>

<span data-ttu-id="5d59b-133">Ha nem adja hozzá hello paramétert, hello feladat hello klasszikus kötegelt folyamat környezetében futtatható.</span><span class="sxs-lookup"><span data-stu-id="5d59b-133">If you do not add hello parameter, hello job is run in hello classic batch process environment.</span></span> <span data-ttu-id="5d59b-134">Hello készlet elegendő erőforrás áll rendelkezésre, ha a hello feladat azonnal futásának indításakor.</span><span class="sxs-lookup"><span data-stu-id="5d59b-134">If hello pool has available resources, hello job starts running immediately.</span></span> <span data-ttu-id="5d59b-135">Hello készlet nem rendelkezik szabad erőforrást, ha a feladat várólistára van állítva, amíg egy erőforrás áll rendelkezésre.</span><span class="sxs-lookup"><span data-stu-id="5d59b-135">If hello pool does not have free resources, your job is queued until a resource is available.</span></span>

<span data-ttu-id="5d59b-136">Ha talál meg, hogy rendszeresen eléri a készletek hello kapacitását, és nagyobb kapacitást van szüksége, CSS hívja, és a kvóták egy reprezentatív tooincrease dolgozni.</span><span class="sxs-lookup"><span data-stu-id="5d59b-136">If you find that you regularly reach hello capacity of your pools, and you need increased capacity, you can call CSS and work with a representative tooincrease your quotas.</span></span>

<span data-ttu-id="5d59b-137">Példa egy kérelem:</span><span class="sxs-lookup"><span data-stu-id="5d59b-137">Example Request:</span></span>

<span data-ttu-id="5d59b-138">https://ussouthcentral.Services.azureml.NET/Subscriptions/80c77c7674ba4c8c82294c3b2957990c/Services/9fe659022c9747e3b9b7b923c3830623/Jobs?API-Version=2.0</span><span class="sxs-lookup"><span data-stu-id="5d59b-138">https://ussouthcentral.services.azureml.net/subscriptions/80c77c7674ba4c8c82294c3b2957990c/services/9fe659022c9747e3b9b7b923c3830623/jobs?api-version=2.0</span></span>

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

## <a name="considerations-when-using-batch-pool-processing"></a><span data-ttu-id="5d59b-139">Batch-készlet feldolgozási használatának szempontjai</span><span class="sxs-lookup"><span data-stu-id="5d59b-139">Considerations when using Batch Pool processing</span></span>

<span data-ttu-id="5d59b-140">Batch-készlet feldolgozásakor – a számlázható szolgáltatás pedig, amely szükséges az erőforrás-kezelő szerint számlázási csomag tooassociate.</span><span class="sxs-lookup"><span data-stu-id="5d59b-140">Batch Pool Processing is an always-on billable service and that requires you tooassociate it with a Resource Manager based billing plan.</span></span> <span data-ttu-id="5d59b-141">Csak számlázása hello számú hello készlet fut; számítási óra függetlenül attól, feladatok futtatása során idő készlethez hello száma.</span><span class="sxs-lookup"><span data-stu-id="5d59b-141">You are only billed for hello number of compute hours hello pool is running; regardless of hello number of jobs run during that time pool.</span></span> <span data-ttu-id="5d59b-142">Készletet hoz létre, ha díját is felszámítjuk hello számítási órán át az egyes virtuális gépek hello készletben hello készlet nem törlik, még akkor is, ha nincsenek kötegelt feladatok futnak hello készletben.</span><span class="sxs-lookup"><span data-stu-id="5d59b-142">If you create a pool, you are billed for hello compute hours of each virtual machine in hello pool until hello pool is deleted, even if no batch jobs are running in hello pool.</span></span> <span data-ttu-id="5d59b-143">Számlázási hello virtuális gépek akkor kezdődik, amikor a kiépítés után, és leáll, amikor törölve lettek.</span><span class="sxs-lookup"><span data-stu-id="5d59b-143">Billing for hello virtual machines starts when they have finished provisioning and stops when they have been deleted.</span></span> <span data-ttu-id="5d59b-144">Bármelyik hello csomagok hello használható [Machine Learning díjszabás lap](https://azure.microsoft.com/pricing/details/machine-learning/).</span><span class="sxs-lookup"><span data-stu-id="5d59b-144">You can use any of hello plans found on hello [Machine Learning Pricing page](https://azure.microsoft.com/pricing/details/machine-learning/).</span></span>

<span data-ttu-id="5d59b-145">Számlázási. példa:</span><span class="sxs-lookup"><span data-stu-id="5d59b-145">Billing example:</span></span>

<span data-ttu-id="5d59b-146">Ha a Batch-készlet létrehozása 2 virtuális gépekkel és 24 óra múlva törli azt a számlázási csomag terhelt számítási 48 óra; függetlenül attól, hogy hány feladat futtatná adott időszakban.</span><span class="sxs-lookup"><span data-stu-id="5d59b-146">If you create a Batch Pool with 2 virtual machines and delete it after 24 hours your billing plan is debited 48 compute hours; regardless of how many jobs were run during that period.</span></span>

<span data-ttu-id="5d59b-147">Ha a Batch-készlet létrehozása 4 virtuális gépekkel és 12 óra elteltével törölje, a számlázási csomag is számítási terhelést 48 óra.</span><span class="sxs-lookup"><span data-stu-id="5d59b-147">If you create a Batch Pool with 4 virtual machines and delete it after 12 hours, your billing plan is also debited 48 compute hours.</span></span>

<span data-ttu-id="5d59b-148">Azt javasoljuk, hogy hello feladat állapota toodetermine lekérdezésére feladat befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="5d59b-148">We recommend that you poll hello job status toodetermine when jobs complete.</span></span> <span data-ttu-id="5d59b-149">Ha a feladatok futása befejeződött, hello készlet toozero hívás hello átméretezési készlet művelet tooset hello virtuális gépek száma.</span><span class="sxs-lookup"><span data-stu-id="5d59b-149">When all your jobs have finished running, call hello Resize Pool operation tooset hello number of virtual machines in hello pool toozero.</span></span> <span data-ttu-id="5d59b-150">Ha rövid készlet erőforrások és toocreate egy új készletet, például egy másik számlázási csomag elleni toobill kell törölheti hello készlet helyette ha összes feladat futása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="5d59b-150">If you are short on pool resources and you need toocreate a new pool, for example toobill against a different billing plan, you can delete hello pool instead when all your jobs have finished running.</span></span>


| <span data-ttu-id="5d59b-151">**Használja a kötegelt mikor feldolgozása**</span><span class="sxs-lookup"><span data-stu-id="5d59b-151">**Use Batch Pool Processing when**</span></span>    | <span data-ttu-id="5d59b-152">**Használja a klasszikus kötegfeldolgozási végezhető, ha**</span><span class="sxs-lookup"><span data-stu-id="5d59b-152">**Use classic batch processing when**</span></span>  |
|---|---|
|<span data-ttu-id="5d59b-153">Feladatok nagy számú toorun van szüksége</span><span class="sxs-lookup"><span data-stu-id="5d59b-153">You need toorun a large number of jobs</span></span><br><span data-ttu-id="5d59b-154">Vagy</span><span class="sxs-lookup"><span data-stu-id="5d59b-154">Or</span></span><br/><span data-ttu-id="5d59b-155">A feladatok által futtatott azonnal tooknow van szüksége</span><span class="sxs-lookup"><span data-stu-id="5d59b-155">You need tooknow that your jobs will run immediately</span></span><br/><span data-ttu-id="5d59b-156">Vagy</span><span class="sxs-lookup"><span data-stu-id="5d59b-156">Or</span></span><br/><span data-ttu-id="5d59b-157">Garantált átviteli van szüksége.</span><span class="sxs-lookup"><span data-stu-id="5d59b-157">You need guaranteed throughput.</span></span> <span data-ttu-id="5d59b-158">Például meg kell toorun egy adott időszakon feladatok száma, és szeretné, hogy tooscale a számítási erőforrások toomeet ki az igényeinek.</span><span class="sxs-lookup"><span data-stu-id="5d59b-158">For example, you need toorun a number of jobs in a given time frame, and want tooscale out your compute resources toomeet your needs.</span></span>    | <span data-ttu-id="5d59b-159">Néhány feladatot futtat</span><span class="sxs-lookup"><span data-stu-id="5d59b-159">You are running just a few jobs</span></span><br/><span data-ttu-id="5d59b-160">És</span><span class="sxs-lookup"><span data-stu-id="5d59b-160">And</span></span><br/> <span data-ttu-id="5d59b-161">A felesleges hello feladatok toorun azonnal</span><span class="sxs-lookup"><span data-stu-id="5d59b-161">You don’t need hello jobs toorun immediately</span></span> |
