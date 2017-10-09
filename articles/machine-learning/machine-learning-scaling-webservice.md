---
title: "aaaHow tooincrease egyidejűségi beállítása pedig az Azure Machine Learning webszolgáltatás |} Microsoft Docs"
description: "Ismerje meg, hogyan tooincrease egyidejűségi, az Azure Machine Learning webszolgáltatás további végpontok hozzáadása."
services: machine-learning
documentationcenter: 
author: neerajkh
manager: srikants
editor: cgronlun
keywords: "az Azure gépi tanulási, web services operationalization, a méretezés, a végponthoz, egyidejűségi beállítása"
ms.assetid: c2c51d7f-fd2d-4f03-bc51-bf47e6969296
ms.service: machine-learning
ms.devlang: NA
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/23/2017
ms.author: neerajkh
ms.openlocfilehash: e2ad16ec766820a64f36c31232f6a33a79196af4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scaling-an-azure-machine-learning-web-service-by-adding-additional-endpoints"></a><span data-ttu-id="977b5-104">Az Azure Machine Learning webszolgáltatás méretezhetővé további végpontok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="977b5-104">Scaling an Azure Machine Learning web service by adding additional endpoints</span></span>
> [!NOTE]
> <span data-ttu-id="977b5-105">Ez a témakör ismerteti a technikák alkalmazható tooa **klasszikus** Machine Learning webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="977b5-105">This topic describes techniques applicable tooa **Classic** Machine Learning Web service.</span></span> 
> 
> 

<span data-ttu-id="977b5-106">Alapértelmezés szerint minden egyes közzétett webszolgáltatás konfigurált toosupport 20 egyidejű kérelmek és lehet magas, mint 200 egyidejű kérelemre.</span><span class="sxs-lookup"><span data-stu-id="977b5-106">By default, each published Web service is configured toosupport 20 concurrent requests and can be as high as 200 concurrent requests.</span></span> <span data-ttu-id="977b5-107">Hello a klasszikus Azure portálon tartalmaz egy módja tooset ezt az értéket, amíg Azure Machine Learning automatikusan optimalizálja hello beállítás tooprovide hello legjobb teljesítmény érdekében a webszolgáltatáshoz és hello portál értéket a rendszer figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="977b5-107">While hello Azure classic portal provides a way tooset this value, Azure Machine Learning automatically optimizes hello setting tooprovide hello best performance for your web service and hello portal value is ignored.</span></span> 

<span data-ttu-id="977b5-108">Ha azt tervezi, toocall hello API-t a nagyobb terheléshez, mint 200 egyidejű hívások maximális értéket fogja támogatni, hello kell hozzon létre több végpontot azonos webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="977b5-108">If you plan toocall hello API with a higher load than a Max Concurrent Calls value of 200 will support, you should create multiple endpoints on hello same Web service.</span></span> <span data-ttu-id="977b5-109">Majd véletlenszerűen terjesztheti a terhelés összes őket.</span><span class="sxs-lookup"><span data-stu-id="977b5-109">You can then randomly distribute your load across all of them.</span></span>

<span data-ttu-id="977b5-110">a gyakori feladatok hello skálázás egy webszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="977b5-110">hello scaling of a Web service is a common task.</span></span> <span data-ttu-id="977b5-111">Valamilyen okból tooscale toosupport több mint 200 egyidejű kérelemre, magasabb rendelkezésre állását több végpontot keresztül, vagy adjon meg külön végpontok hello webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="977b5-111">Some reasons tooscale are toosupport more than 200 concurrent requests, increase availability through multiple endpoints, or provide separate endpoints for hello web service.</span></span> <span data-ttu-id="977b5-112">Hello méretezési hello további végpontokat hozzáadásával növelhető keresztül azonos webszolgáltatás [a klasszikus Azure portálon](https://manage.windowsazure.com/) vagy hello [Azure Machine Learning webszolgáltatás](https://services.azureml.net/) portálon.</span><span class="sxs-lookup"><span data-stu-id="977b5-112">You can increase hello scale by adding additional endpoints for hello same Web service through [Azure classic portal](https://manage.windowsazure.com/) or hello [Azure Machine Learning Web Service](https://services.azureml.net/) portal.</span></span>

<span data-ttu-id="977b5-113">Új végpontok hozzáadásával kapcsolatos további információkért lásd: [végpontok létrehozása](machine-learning-create-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="977b5-113">For more information on adding new endpoints, see [Creating Endpoints](machine-learning-create-endpoint.md).</span></span>

<span data-ttu-id="977b5-114">Ne feledje, hogy a nagy feldolgozási száma használatával hátrányos, ha nem hívás hello API ennek megfelelően nagy sebességet lehet.</span><span class="sxs-lookup"><span data-stu-id="977b5-114">Keep in mind that using a high concurrency count can be detrimental if you're not calling hello API with a correspondingly high rate.</span></span> <span data-ttu-id="977b5-115">Szórványos időtúllépések és/vagy a teljesítményt a hello késés Ha viszonylag kis terhelésű elhelyezése egy API-t nagy terhelés konfigurált jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="977b5-115">You might see sporadic timeouts and/or spikes in hello latency if you put a relatively low load on an API configured for high load.</span></span>

<span data-ttu-id="977b5-116">hello szinkron API-k általában hol van szükség egy alacsony késési igényű helyzetekben használják.</span><span class="sxs-lookup"><span data-stu-id="977b5-116">hello synchronous APIs are typically used in situations where a low latency is desired.</span></span> <span data-ttu-id="977b5-117">Itt késés lévő hello idő hello API toocomplete egy kérelmek, és nem derül ki minden hálózati késésekből származik.</span><span class="sxs-lookup"><span data-stu-id="977b5-117">Latency here implies hello time it takes for hello API toocomplete one request, and doesn't account for any network delays.</span></span> <span data-ttu-id="977b5-118">Tegyük fel, hogy rendelkezik-e az API-k és egy 50-ms késleltetés.</span><span class="sxs-lookup"><span data-stu-id="977b5-118">Let's say you have an API with a 50-ms latency.</span></span> <span data-ttu-id="977b5-119">toofully felhasználhatják az hello rendelkezésre álló kapacitásból nagy adatátviteli szintű és az egyidejű hívások maximális számának = 20, ez az API 20 * 1000 kell toocall / 50 = 400 másodpercenként alkalommal fordult elő.</span><span class="sxs-lookup"><span data-stu-id="977b5-119">toofully consume hello available capacity with throttle level High and Max Concurrent Calls = 20, you need toocall this API 20 * 1000 / 50 = 400 times per second.</span></span> <span data-ttu-id="977b5-120">Ez további kiterjesztése, egyidejű hívások maximális 200 lehetővé teszi, hogy Ön toocall hello API 4000 hányszor másodpercenként, feltéve, hogy egy 50-ms késleltetés.</span><span class="sxs-lookup"><span data-stu-id="977b5-120">Extending this further, a Max Concurrent Calls of 200 allows you toocall hello API 4000 times per second, assuming a 50-ms latency.</span></span>

<!--Image references-->
[1]: ./media/machine-learning-scaling-webservice/machlearn-1.png
[2]: ./media/machine-learning-scaling-webservice/machlearn-2.png
