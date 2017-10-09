---
title: "aaa(deprecated) közzététel machine learning web service tooAzure piactér |} Microsoft Docs"
description: "(elavult) Hogyan toopublish az Azure Machine Learning webszolgáltatás toohello Azure piactéren"
services: machine-learning
documentationcenter: 
author: BharathS
manager: jhubbard
editor: cgronlun
ms.assetid: 68e908be-3a99-4cd7-9517-e2b5f2f341b8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: bharaths
ROBOTS: NOINDEX
redirect_url: machine-learning-gallery-experiments
redirect_document_id: True
ms.openlocfilehash: 149abc3df9b79c1b37d233d5e85e803592ff1020
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-publish-azure-machine-learning-web-service-toohello-azure-marketplace"></a><span data-ttu-id="c40a7-103">(elavult) Azure Machine Learning webszolgáltatás toohello Azure piactér közzététele</span><span class="sxs-lookup"><span data-stu-id="c40a7-103">(deprecated) Publish Azure Machine Learning Web Service toohello Azure Marketplace</span></span>

> [!NOTE]
> <span data-ttu-id="c40a7-104">DataMarket és Data Services rendszer hatókörről, és a meglévő előfizetések rendszerből, és elindítása megszakítva 3/31/2017.</span><span class="sxs-lookup"><span data-stu-id="c40a7-104">DataMarket and Data Services are being retired, and existing subscriptions will be retired and cancelled starting 3/31/2017.</span></span> <span data-ttu-id="c40a7-105">Ennek köszönhetően ez a cikk is elavult.</span><span class="sxs-lookup"><span data-stu-id="c40a7-105">As a result, this article is being deprecated.</span></span> 
> 
> <span data-ttu-id="c40a7-106">Másik megoldásként, közzéteheti a Machine Learning kísérleteket toohello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) hello adatok tudományos Közösség hello juttatásra.</span><span class="sxs-lookup"><span data-stu-id="c40a7-106">As an alternative, you can publish your Machine Learning experiments toohello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) for hello benefit of hello data science community.</span></span> <span data-ttu-id="c40a7-107">További információkért lásd: [megosztás és a Cortana Intelligence Gallery hello erőforrások felderítéséhez](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-gallery-how-to-use-contribute-publish).</span><span class="sxs-lookup"><span data-stu-id="c40a7-107">For more information, see [Share and discover resources in hello Cortana Intelligence Gallery](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-gallery-how-to-use-contribute-publish).</span></span>

<span data-ttu-id="c40a7-108">hello Azure piactér hello alábbi képességekkel rendelkezik toopublish Azure Machine Learning webszolgáltatások fizet, vagy a külső felhasználók által megjeleníthető szolgáltatások szabad.</span><span class="sxs-lookup"><span data-stu-id="c40a7-108">hello Azure Marketplace provides hello ability toopublish Azure Machine Learning web services as paid or free services for consumption by external customers.</span></span> <span data-ttu-id="c40a7-109">Ez a cikk áttekintést hivatkozások tooguidelines tooget használatba az adott folyamatot.</span><span class="sxs-lookup"><span data-stu-id="c40a7-109">This article provides an overview of that process with links tooguidelines tooget you started.</span></span> <span data-ttu-id="c40a7-110">Ez a folyamat használatával biztosíthatja a webszolgáltatások más fejlesztők tooconsume érhető el az alkalmazásokban.</span><span class="sxs-lookup"><span data-stu-id="c40a7-110">By using this process, you can make your web services available for other developers tooconsume in their applications.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="overview-of-hello-publishing-process"></a><span data-ttu-id="c40a7-111">Hello közzétételi folyamat áttekintése</span><span class="sxs-lookup"><span data-stu-id="c40a7-111">Overview of hello publishing process</span></span>
<span data-ttu-id="c40a7-112">Az alábbiakban hello hello lépései az Azure Machine Learning web service tooAzure piactér közzététele:</span><span class="sxs-lookup"><span data-stu-id="c40a7-112">hello following are hello steps for publishing an Azure Machine Learning web service tooAzure Marketplace:</span></span>

1. <span data-ttu-id="c40a7-113">Hozzon létre, és tegye közzé a Machine Learning kérés-válasz szolgáltatás (RR-EKET)</span><span class="sxs-lookup"><span data-stu-id="c40a7-113">Create and publish a Machine Learning Request-Response service (RRS)</span></span>
2. <span data-ttu-id="c40a7-114">Hello szolgáltatás tooproduction telepíteni, és szerezze be a hello API-kulcs és OData-végpont adatai.</span><span class="sxs-lookup"><span data-stu-id="c40a7-114">Deploy hello service tooproduction, and obtain hello API Key and OData endpoint information.</span></span>
3. <span data-ttu-id="c40a7-115">Túl a közzétett webes szolgáltatás toopublish hello használata hello URL-[Azure piactér (adatok piaci)](https://publish.windowsazure.com/workspace/)</span><span class="sxs-lookup"><span data-stu-id="c40a7-115">Use hello URL of hello published web service toopublish too[Azure Marketplace (Data Market)](https://publish.windowsazure.com/workspace/)</span></span> 
4. <span data-ttu-id="c40a7-116">Ha küldött, az ajánlatot áttekintette és kell toobe, mielőtt az ügyfelek jóváhagyott elindíthatja az beszerzése.</span><span class="sxs-lookup"><span data-stu-id="c40a7-116">Once submitted, your offer is reviewed and needs toobe approved before your customers can start purchasing it.</span></span> <span data-ttu-id="c40a7-117">hello közzétételi folyamat eltarthat néhány nap.</span><span class="sxs-lookup"><span data-stu-id="c40a7-117">hello publishing process can take a few business days.</span></span> 

## <a name="walk-through"></a><span data-ttu-id="c40a7-118">Útmutató</span><span class="sxs-lookup"><span data-stu-id="c40a7-118">Walk through</span></span>
### <a name="step-1-create-and-publish-a-machine-learning-request-response-service-rrs"></a><span data-ttu-id="c40a7-119">1. lépés: Létrehozása és közzététele a Machine Learning kérés-válasz szolgáltatás (RR-EKET)</span><span class="sxs-lookup"><span data-stu-id="c40a7-119">Step 1: Create and publish a Machine Learning Request-Response service (RRS)</span></span>
 <span data-ttu-id="c40a7-120">Ha ezt nem ez már, kérjük, szánjon egy pillantást, ez [bízná](machine-learning-walkthrough-5-publish-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="c40a7-120">If you have not done this already, please take a look at this [walk through](machine-learning-walkthrough-5-publish-web-service.md).</span></span>

### <a name="step-2-deploy-hello-service-tooproduction-and-obtain-hello-api-key-and-odata-endpoint-information"></a><span data-ttu-id="c40a7-121">2. lépés: Hello szolgáltatás tooproduction telepíteni, és szerezze be a hello API-kulcs és OData-végpont adatai</span><span class="sxs-lookup"><span data-stu-id="c40a7-121">Step 2: Deploy hello service tooproduction, and obtain hello API Key and OData endpoint information</span></span>
1. <span data-ttu-id="c40a7-122">A hello [klasszikus Azure portál](http://manage.windowsazure.com), jelölje be hello **MACHINE LEARNING** hello bal oldali navigációs sávon a lehetőséget, majd válassza ki a munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="c40a7-122">From hello [Azure Classic Portal](http://manage.windowsazure.com), select hello **MACHINE LEARNING** option from hello left navigation bar, and select your workspace.</span></span> 
2. <span data-ttu-id="c40a7-123">Kattintson a hello **WEBSZOLGÁLTATÁSOK** fülre, és azt szeretné, hogy toopublish toohello piactér válassza hello webszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c40a7-123">Click on hello **WEB SERVICES** tab, and select hello web service you would like toopublish toohello marketplace.</span></span>
   
    ![Azure Piactér][workspace]
3. <span data-ttu-id="c40a7-125">Válassza ki kellene például toohave hello piactér felhasznált hello végpontot.</span><span class="sxs-lookup"><span data-stu-id="c40a7-125">Select hello endpoint you would like toohave hello marketplace consume.</span></span> <span data-ttu-id="c40a7-126">Ha nem hozott létre további végpontok, kiválaszthatja a hello **alapértelmezett** végpont.</span><span class="sxs-lookup"><span data-stu-id="c40a7-126">If you have not created any additional endpoints, you can select hello **Default** endpoint.</span></span>
4. <span data-ttu-id="c40a7-127">Való kattintást követően hello végponton, fogja tudni toosee hello **API-kulcs**.</span><span class="sxs-lookup"><span data-stu-id="c40a7-127">Once you have clicked on hello endpoint, you will be able toosee hello **API KEY**.</span></span> <span data-ttu-id="c40a7-128">Szüksége lesz az adott adatok később a 3. lépésben, ezért győződjön egy példányát.</span><span class="sxs-lookup"><span data-stu-id="c40a7-128">You will need this piece of information later on in Step 3, so make a copy of it.</span></span>
   
    ![Azure Piactér][apikey]
5. <span data-ttu-id="c40a7-130">Kattintson a hello **kérelem/válasz** metódus ezen a ponton nem támogatott közzétételi kötegelt végrehajtási szolgáltatás toohello piactéren.</span><span class="sxs-lookup"><span data-stu-id="c40a7-130">Click on hello **REQUEST/RESPONSE** method, at this point we do not support publishing batch execution services toohello marketplace.</span></span> <span data-ttu-id="c40a7-131">Amely a billentyűparanccsal léphet toohello API súgólapján hello kérelem/válasz metódust.</span><span class="sxs-lookup"><span data-stu-id="c40a7-131">That will take you toohello API help page for hello Request/Response method.</span></span>
6. <span data-ttu-id="c40a7-132">Másolás hello **OData-végpont címe**, akkor ezt az információt később a 3. lépés.</span><span class="sxs-lookup"><span data-stu-id="c40a7-132">Copy hello **OData Endpoint Address**, you will need this information later on in Step 3.</span></span>
   
    ![Azure Piactér][odata]

<span data-ttu-id="c40a7-134">hello szolgáltatás telepítése éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="c40a7-134">deploy hello service into production.</span></span>

### <a name="step-3-use-hello-url-of-hello-published-web-service-toopublish-tooazure-marketplace-datamarket"></a><span data-ttu-id="c40a7-135">3. lépés: A közzétett webes szolgáltatás toopublish tooAzure piactér (DataMarket) hello használata hello URL-</span><span class="sxs-lookup"><span data-stu-id="c40a7-135">Step 3: Use hello URL of hello published web service toopublish tooAzure Marketplace (DataMarket)</span></span>
1. <span data-ttu-id="c40a7-136">Keresse meg a túl[Azure piactér (adatok piaci)](http://datamarket.azure.com/home)</span><span class="sxs-lookup"><span data-stu-id="c40a7-136">Navigate too[Azure Marketplace (Data Market)](http://datamarket.azure.com/home)</span></span> 
2. <span data-ttu-id="c40a7-137">Kattintson a hello **közzététel** hivatkozás hello oldal hello tetején.</span><span class="sxs-lookup"><span data-stu-id="c40a7-137">Click on hello **Publish** link at hello top of hello page.</span></span> <span data-ttu-id="c40a7-138">A rendszer ekkor toohello [Microsoft Azure közzétételi Portáljára](https://publish.windowsazure.com)</span><span class="sxs-lookup"><span data-stu-id="c40a7-138">This will take you toohello [Microsoft Azure Publishing Portal](https://publish.windowsazure.com)</span></span>
3. <span data-ttu-id="c40a7-139">Kattintson a hello **közzétevők** szakasz tooregister közzétevőként.</span><span class="sxs-lookup"><span data-stu-id="c40a7-139">Click on hello **publishers** section tooregister as a publisher.</span></span>
4. <span data-ttu-id="c40a7-140">Egy új ajánlat létrehozásakor válassza ki a **adatszolgáltatások**, majd kattintson a **hozzon létre egy új szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="c40a7-140">When creating a new offer, select **Data Services**, then click **Create a New Data Service**.</span></span> 
   
   ![Azure Piactér][image1]
   
   <br />
5. <span data-ttu-id="c40a7-142">A **tervek** információt nyújtanak az ajánlat, beleértve a tarifacsomagot.</span><span class="sxs-lookup"><span data-stu-id="c40a7-142">Under **Plans** provide information on your offering, including a pricing plan.</span></span> <span data-ttu-id="c40a7-143">Döntse el, ha egy ingyenes és fizetős szolgáltatás tartalmazni fogja.</span><span class="sxs-lookup"><span data-stu-id="c40a7-143">Decide if you will offer a free or paid service.</span></span> <span data-ttu-id="c40a7-144">a fizetős, tooget fizetési információk, például a bank és az adót információk adja meg.</span><span class="sxs-lookup"><span data-stu-id="c40a7-144">tooget paid, provide payment information such as your bank and tax information.</span></span>
6. <span data-ttu-id="c40a7-145">A **Marketing** ismertetik az ajánlatot, például a hello címet és a leírást az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="c40a7-145">Under **Marketing** provide information about your offer, such as hello title and description for your offer.</span></span>
7. <span data-ttu-id="c40a7-146">A **árazás** hello árlista beállítása az adott országok terveket, illetve engedélyezheti, hogy a "autoprice" hello rendszer ajánlatát.</span><span class="sxs-lookup"><span data-stu-id="c40a7-146">Under **Pricing** you can set hello price for your plans for specific countries, or let hello system "autoprice" your offer.</span></span>
8. <span data-ttu-id="c40a7-147">A hello **adatszolgáltatás** lapra, majd **webszolgáltatás** , hello **adatforrás**.</span><span class="sxs-lookup"><span data-stu-id="c40a7-147">On hello **Data Service** tab, click **Web Service** as hello **Data Source**.</span></span>
   
    ![Azure Piactér][image2]
9. <span data-ttu-id="c40a7-149">Hello webes szolgáltatás URL-CÍMÉT és API-kulcsát beszerzése a klasszikus Azure portálon hello a fenti 2. lépésben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="c40a7-149">Get hello web service URL and API key from hello Azure Classic Portal, as explained in step 2 above.</span></span>
10. <span data-ttu-id="c40a7-150">Hello piactér adatszolgáltatás beállítása párbeszédpanelen illessze be az OData-végpont címe hello hello **szolgáltatás URL-címe** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="c40a7-150">In hello Marketplace Data Service setup dialog box, paste hello OData endpoint address into hello **Service URL** text box.</span></span>
11. <span data-ttu-id="c40a7-151">A **hitelesítési**, válassza a **fejléc** , hello **hitelesítési séma**.</span><span class="sxs-lookup"><span data-stu-id="c40a7-151">For **Authentication**, choose **Header** as hello **Authentication Scheme**.</span></span>
    
    * <span data-ttu-id="c40a7-152">Adja meg "Engedélyezés" hello **fejlécnév**.</span><span class="sxs-lookup"><span data-stu-id="c40a7-152">Enter "Authorization" for hello **Header Name**.</span></span>
    * <span data-ttu-id="c40a7-153">A hello **Fejlécérték**, írja be a "Tulajdonos" (hello idézőjelek nélkül), kattintson a hello **terület** sáv megnyitásához, és illessze be hello API-kulcs.</span><span class="sxs-lookup"><span data-stu-id="c40a7-153">For hello **Header Value**, enter "Bearer" (without hello quotation marks), click hello **Space** bar, and then paste hello API key.</span></span>
    * <span data-ttu-id="c40a7-154">Jelölje be hello **a szolgáltatás nem OData** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="c40a7-154">Select hello **This Service is OData** check box.</span></span>
    * <span data-ttu-id="c40a7-155">Kattintson a **kapcsolat tesztelése** tootest hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="c40a7-155">Click **Test Connection** tootest hello connection.</span></span>
12. <span data-ttu-id="c40a7-156">A **kategóriák**, győződjön meg arról **Machine Learning** van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="c40a7-156">Under **Categories**, ensure **Machine Learning** is selected.</span></span>
13. <span data-ttu-id="c40a7-157">Ha befejezte az összes beírása hello metaadatok kapcsolatos ajánlatát, kattintson a **közzététel**, majd **tooStaging leküldéses**.</span><span class="sxs-lookup"><span data-stu-id="c40a7-157">When you are done entering all hello metadata about your offer, click on **Publish**, and then **Push tooStaging**.</span></span> <span data-ttu-id="c40a7-158">Ezen a ponton, értesítést kap a fennmaradó probléma merül fel, hogy kell-e toofix.</span><span class="sxs-lookup"><span data-stu-id="c40a7-158">At this point, you will be notified of any remaining issues that you need toofix.</span></span>
14. <span data-ttu-id="c40a7-159">Miután meggyőződött a létrehozása után minden hello fennálló problémákat, kattintson a **toopush tooProduction jóváhagyási kérelmek**.</span><span class="sxs-lookup"><span data-stu-id="c40a7-159">After you have ensured completion of all hello outstanding issues, click on **Request approval toopush tooProduction**.</span></span> <span data-ttu-id="c40a7-160">hello közzétételi folyamat eltarthat néhány nap.</span><span class="sxs-lookup"><span data-stu-id="c40a7-160">hello publishing process can take a few business days.</span></span> 

[image1]:./media/machine-learning-publish-web-service-to-azure-marketplace/image1.png
[image2]:./media/machine-learning-publish-web-service-to-azure-marketplace/image2.png
[workspace]:./media/machine-learning-publish-web-service-to-azure-marketplace/selectworkspace.png
[apikey]:./media/machine-learning-publish-web-service-to-azure-marketplace/apikey.png
[odata]:./media/machine-learning-publish-web-service-to-azure-marketplace/odata.png

