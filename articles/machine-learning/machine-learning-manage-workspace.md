---
title: "a Machine Learning-munkaterület aaaManage |} Microsoft Docs"
description: "Hozzáférés tooAzure Machine Learning munkaterületek, kezelése és szolgáltatások telepítésére és kezelésére ML API webkiszolgáló"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: daf3d413-7a77-4beb-9a7a-6b4bdf717719
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: garye
ms.openlocfilehash: 7bd378d82aa46f4340ca974c7dc5a612c089cdca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-an-azure-machine-learning-workspace"></a><span data-ttu-id="9e77f-103">Azure Machine Learning-munkaterület kezelése</span><span class="sxs-lookup"><span data-stu-id="9e77f-103">Manage an Azure Machine Learning workspace</span></span>

> [!NOTE]
> <span data-ttu-id="9e77f-104">Webszolgáltatások hello Machine Learning webszolgáltatások portálon kezeléséről további információért lásd: [egy webszolgáltatás-bővítmény hello Azure Machine Learning webszolgáltatások portálon kezelheti](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="9e77f-104">For information on managing Web services in hello Machine Learning Web Services portal, see [Manage a Web service using hello Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span>
> 
> 

<span data-ttu-id="9e77f-105">Gépi tanulás munkaterületeket, vagy hello Azure-portálon kezelheti, vagy a klasszikus Azure portálon hello.</span><span class="sxs-lookup"><span data-stu-id="9e77f-105">You can manage Machine Learning workspaces in either hello Azure portal or hello Azure classic portal.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="use-hello-azure-portal"></a><span data-ttu-id="9e77f-106">Hello Azure portál használata</span><span class="sxs-lookup"><span data-stu-id="9e77f-106">Use hello Azure portal</span></span>

<span data-ttu-id="9e77f-107">toomanage munkaterületeinek hello Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="9e77f-107">toomanage a workspace in hello Azure portal:</span></span>

1. <span data-ttu-id="9e77f-108">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/) Azure-előfizetéshez rendszergazdai fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="9e77f-108">Sign in toohello [Azure portal](https://portal.azure.com/) using an Azure subscription administrator account.</span></span>
2. <span data-ttu-id="9e77f-109">Hello oldal hello tetején hello a keresési mezőbe, írja be a "számítógép tanulási munkaterületek", és válassza ki **Machine Learning munkaterületek**.</span><span class="sxs-lookup"><span data-stu-id="9e77f-109">In hello search box at hello top of hello page, enter "machine learning workspaces" and then select **Machine Learning Workspaces**.</span></span>
3. <span data-ttu-id="9e77f-110">Kattintson a kívánt toomanage hello munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="9e77f-110">Click hello workspace you want toomanage.</span></span>

<span data-ttu-id="9e77f-111">Ezenkívül toohello szabványos erőforrás-kezelési információkat és a rendelkezésre álló lehetőségeket, a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="9e77f-111">In addition toohello standard resource management information and options available, you can:</span></span>

- <span data-ttu-id="9e77f-112">Nézet **tulajdonságok** - ezen a lapon látható hello munkaterületet, és az erőforrás-információkat, és módosíthatja hello előfizetés és az erőforráscsoport, a munkaterület kapcsolódnak.</span><span class="sxs-lookup"><span data-stu-id="9e77f-112">View **Properties** - This page displays hello workspace and resource information, and you can change hello subscription and resource group that this workspace is connected with.</span></span>
- <span data-ttu-id="9e77f-113">**Tárolási kulcsok újraszinkronizálásra** -hello munkaterület kulcsok toohello tárfiók tart fenn.</span><span class="sxs-lookup"><span data-stu-id="9e77f-113">**Resync Storage Keys** - hello workspace maintains keys toohello storage account.</span></span> <span data-ttu-id="9e77f-114">Ha hello storage-fiók vált kulcsok, ezt követően kattinthat **kulcsok újraszinkronizálásra** toosynchronize hello kulcsok hello munkaterület.</span><span class="sxs-lookup"><span data-stu-id="9e77f-114">If hello storage account changes keys, then you can click **Resync keys** toosynchronize hello keys with hello workspace.</span></span>

<span data-ttu-id="9e77f-115">a munkaterülethez tartozó toomanage hello webszolgáltatások hello Machine Learning webszolgáltatások portál használata.</span><span class="sxs-lookup"><span data-stu-id="9e77f-115">toomanage hello web services associated with this workspace, use hello Machine Learning Web Services portal.</span></span> <span data-ttu-id="9e77f-116">Lásd: [egy webszolgáltatás-bővítmény hello Azure Machine Learning webszolgáltatások portálon kezelheti](machine-learning-manage-new-webservice.md) kapcsolatos részletes információkért.</span><span class="sxs-lookup"><span data-stu-id="9e77f-116">See [Manage a Web service using hello Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md) for complete information.</span></span>

> [!NOTE]
> <span data-ttu-id="9e77f-117">toodeploy vagy új webszolgáltatások, akkor hozzá kell rendelni egy munkatárs kezelése vagy a rendszergazda szerepkörhöz a hello előfizetés toowhich hello webszolgáltatás telepítve van.</span><span class="sxs-lookup"><span data-stu-id="9e77f-117">toodeploy or manage New web services you must be assigned a contributor or administrator role on hello subscription toowhich hello web service is deployed.</span></span> <span data-ttu-id="9e77f-118">Ha meghívni egy másik felhasználó tooa gépi tanulási munkaterület, hozzá kell rendelnie őket tooa közreműködő vagy a rendszergazda szerepkör hello előfizetés ahhoz, azok központilag telepíthetne vagy kezelhetne webszolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="9e77f-118">If you invite another user tooa machine learning workspace, you must assign them tooa contributor or administrator role on hello subscription before they can deploy or manage web services.</span></span> 
> 
><span data-ttu-id="9e77f-119">A hozzáférési engedélyek beállítása további információkért lásd: [access-hozzárendelések megtekintése a felhasználók és csoportok az Azure portál – nyilvános előzetes hello](../active-directory/role-based-access-control-manage-assignments.md).</span><span class="sxs-lookup"><span data-stu-id="9e77f-119">For more information on setting access permissions, see [View access assignments for users and groups in hello Azure portal - Public preview](../active-directory/role-based-access-control-manage-assignments.md).</span></span>

## <a name="use-hello-azure-classic-portal"></a><span data-ttu-id="9e77f-120">A klasszikus Azure portálon hello használata</span><span class="sxs-lookup"><span data-stu-id="9e77f-120">Use hello Azure classic portal</span></span>

<span data-ttu-id="9e77f-121">Hello klasszikus Azure portál használatával, a Machine Learning-munkaterületekkel képes kezelni:</span><span class="sxs-lookup"><span data-stu-id="9e77f-121">Using hello Azure classic portal, you can manage your Machine Learning workspaces to:</span></span>

* <span data-ttu-id="9e77f-122">Hello munkaterület használatának figyelése</span><span class="sxs-lookup"><span data-stu-id="9e77f-122">Monitor how hello workspace is being used</span></span>
* <span data-ttu-id="9e77f-123">Hello munkaterület tooallow konfigurálása vagy megtagadja a hozzáférést</span><span class="sxs-lookup"><span data-stu-id="9e77f-123">Configure hello workspace tooallow or deny access</span></span>
* <span data-ttu-id="9e77f-124">Hello munkaterületen létrehozott webszolgáltatások kezelése</span><span class="sxs-lookup"><span data-stu-id="9e77f-124">Manage Web services created in hello workspace</span></span>
* <span data-ttu-id="9e77f-125">Hello munkaterület törlése</span><span class="sxs-lookup"><span data-stu-id="9e77f-125">Delete hello workspace</span></span>

<span data-ttu-id="9e77f-126">Emellett a hello irányítópult fület az munkaterület használatának áttekintése és a munkaterület adatok gyors áttekintő biztosítja.</span><span class="sxs-lookup"><span data-stu-id="9e77f-126">In addition, hello dashboard tab provides an overview of your workspace usage and a quick glance of your workspace information.</span></span>  

> [!TIP]
> <span data-ttu-id="9e77f-127">Az Azure Machine Learning Studio, a hello **WEBSZOLGÁLTATÁSOK** lapon adhat hozzá, frissítés, vagy egy Machine Learning webszolgáltatás-bővítmény törlése.</span><span class="sxs-lookup"><span data-stu-id="9e77f-127">In Azure Machine Learning Studio, on hello **WEB SERVICES** tab, you can add, update, or delete a Machine Learning Web service.</span></span>
> 
> 

<span data-ttu-id="9e77f-128">a klasszikus Azure portálon hello munkaterületeinek toomanage:</span><span class="sxs-lookup"><span data-stu-id="9e77f-128">toomanage a workspace in hello Azure classic portal:</span></span>

1. <span data-ttu-id="9e77f-129">Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com/) használatával a Microsoft Azure fiók – hello Azure-előfizetéssel társított hello fiókot használja.</span><span class="sxs-lookup"><span data-stu-id="9e77f-129">Sign in toohello [Azure classic portal](https://manage.windowsazure.com/) using your Microsoft Azure account - use hello account that's associated with hello Azure subscription.</span></span>
2. <span data-ttu-id="9e77f-130">Kattintson a Microsoft Azure szolgáltatások panel hello **MACHINE LEARNING**.</span><span class="sxs-lookup"><span data-stu-id="9e77f-130">In hello Microsoft Azure services panel, click **MACHINE LEARNING**.</span></span>
3. <span data-ttu-id="9e77f-131">Kattintson a kívánt toomanage hello munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="9e77f-131">Click hello workspace you want toomanage.</span></span>

<span data-ttu-id="9e77f-132">hello munkaterület lap rendelkezik a három lappal:</span><span class="sxs-lookup"><span data-stu-id="9e77f-132">hello workspace page has three tabs:</span></span>

* <span data-ttu-id="9e77f-133">**IRÁNYÍTÓPULT** -lehetővé teszi az tooview munkaterület használati és információ</span><span class="sxs-lookup"><span data-stu-id="9e77f-133">**DASHBOARD** - Allows you tooview workspace usage and information</span></span>
* <span data-ttu-id="9e77f-134">**KONFIGURÁLÁS** -lehetővé teszi a toomanage hozzáférés toohello munkaterület</span><span class="sxs-lookup"><span data-stu-id="9e77f-134">**CONFIGURE** - Allows you toomanage access toohello workspace</span></span>
* <span data-ttu-id="9e77f-135">**WEBSZOLGÁLTATÁSOK** -lehetővé teszi a munkaterületről közzétett toomanage webszolgáltatások</span><span class="sxs-lookup"><span data-stu-id="9e77f-135">**WEB SERVICES** - Allows you toomanage Web services that have been published from this workspace</span></span>

### <a name="toomonitor-how-hello-workspace-is-being-used"></a><span data-ttu-id="9e77f-136">hello munkaterület használatának toomonitor</span><span class="sxs-lookup"><span data-stu-id="9e77f-136">toomonitor how hello workspace is being used</span></span>
<span data-ttu-id="9e77f-137">Kattintson a hello **IRÁNYÍTÓPULT** fülre.</span><span class="sxs-lookup"><span data-stu-id="9e77f-137">Click hello **DASHBOARD** tab.</span></span>

<span data-ttu-id="9e77f-138">Hello irányítópultról összesített használatát a munkaterület tekintheti meg és munkaterület adatok gyors áttekintő beolvasása.</span><span class="sxs-lookup"><span data-stu-id="9e77f-138">From hello dashboard, you can view overall usage of your workspace and get a quick glance of workspace information.</span></span>

* <span data-ttu-id="9e77f-139">Hello **számítási** hello számítási erőforrásokat hello munkaterületen használja a diagram ábrázolja.</span><span class="sxs-lookup"><span data-stu-id="9e77f-139">hello **COMPUTE** chart shows hello compute resources being used by hello workspace.</span></span> <span data-ttu-id="9e77f-140">Hello nézet toodisplay relatív vagy abszolút értékek módosíthatja, és módosíthatja a hello időkeretre hello diagram.</span><span class="sxs-lookup"><span data-stu-id="9e77f-140">You can change hello view toodisplay relative or absolute values, and you can change hello timeframe displayed in hello chart.</span></span>
* <span data-ttu-id="9e77f-141">**Használati áttekintése** hello munkaterületen használja az Azure storage jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="9e77f-141">**Usage overview** displays Azure storage being used by hello workspace.</span></span>
* <span data-ttu-id="9e77f-142">**Gyors áttekintő** munkaterület információkat és hasznos hivatkozásokat összegzését tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="9e77f-142">**Quick glance** provides a summary of workspace information and useful links.</span></span>

> [!NOTE]
> <span data-ttu-id="9e77f-143">Hello **bejelentkezési tooML Studio** a hivatkozás megnyitja a Machine Learning Studio jelenleg bejelentkezett Microsoft Account hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="9e77f-143">hello **Sign-in tooML Studio** link opens Machine Learning Studio using hello Microsoft Account you are currently signed into.</span></span> <span data-ttu-id="9e77f-144">hello Azure klasszikus portál toocreate munkaterület toohello toosign használt Microsoft Account automatikusan nincs engedélye tooopen munkaterületet.</span><span class="sxs-lookup"><span data-stu-id="9e77f-144">hello Microsoft Account you used toosign in toohello Azure classic portal toocreate a workspace does not automatically have permission tooopen that workspace.</span></span> <span data-ttu-id="9e77f-145">a munkaterület tooopen be kell jelentkeznie a Microsoft hello munkaterület hello tulajdonosaként megadott Account toohello, vagy tooreceive hello tulajdonos toojoin hello munkaterületről meghívót van szüksége.</span><span class="sxs-lookup"><span data-stu-id="9e77f-145">tooopen a workspace, you must be signed in toohello Microsoft Account that was defined as hello owner of hello workspace, or you need tooreceive an invitation from hello owner toojoin hello workspace.</span></span>
> 
> 

### <a name="toogrant-or-suspend-access-for-users"></a><span data-ttu-id="9e77f-146">toogrant vagy felhasználók hozzáférésének felfüggesztése</span><span class="sxs-lookup"><span data-stu-id="9e77f-146">toogrant or suspend access for users</span></span>
<span data-ttu-id="9e77f-147">Kattintson a hello **KONFIGURÁLÁSA** fülre.</span><span class="sxs-lookup"><span data-stu-id="9e77f-147">Click hello **CONFIGURE** tab.</span></span>

<span data-ttu-id="9e77f-148">Hello konfigurációs lapján a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="9e77f-148">From hello configuration tab you can:</span></span>

* <span data-ttu-id="9e77f-149">Hozzáférés toohello Machine Learning-munkaterület felfüggesztése MEGTAGADÁS kattintva.</span><span class="sxs-lookup"><span data-stu-id="9e77f-149">Suspend access toohello Machine Learning workspace by clicking DENY.</span></span> <span data-ttu-id="9e77f-150">Felhasználók többé nem tud tooopen hello munkaterület a Machine Learning Studióban.</span><span class="sxs-lookup"><span data-stu-id="9e77f-150">Users will no longer be able tooopen hello workspace in Machine Learning Studio.</span></span> <span data-ttu-id="9e77f-151">toorestore, kattintson az engedélyezés férhet hozzá.</span><span class="sxs-lookup"><span data-stu-id="9e77f-151">toorestore access, click ALLOW.</span></span>

<span data-ttu-id="9e77f-152">további fiókok toomanage rendelkező hozzáférés toohello munkaterület a Machine Learning Studióban, kattintson a **bejelentkezési tooML Studio** a hello **IRÁNYÍTÓPULT** lapon (lásd: hello portáladatbázis előző Megjegyzés vonatkozó  **TooML Studio Sign-in**).</span><span class="sxs-lookup"><span data-stu-id="9e77f-152">toomanage additional accounts who have access toohello workspace in Machine Learning Studio, click **Sign-in tooML Studio** in hello **DASHBOARD** tab (see hello preceeding note regarding **Sign-in tooML Studio**).</span></span> <span data-ttu-id="9e77f-153">Hello munkaterületen megnyílik a Machine Learning Studióban.</span><span class="sxs-lookup"><span data-stu-id="9e77f-153">This opens hello workspace in Machine Learning Studio.</span></span> <span data-ttu-id="9e77f-154">Itt kattintson a hello **beállítások** fülre, majd **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="9e77f-154">From here, click hello **SETTINGS** tab and then **USERS**.</span></span> <span data-ttu-id="9e77f-155">Kattinthat **több felhasználók MEGHÍVÁSA** toogive felhasználók hozzáférését toohello munkaterületet, vagy válasszon ki egy felhasználót, majd kattintson **eltávolítása**.</span><span class="sxs-lookup"><span data-stu-id="9e77f-155">You can click **INVITE MORE USERS** toogive users access toohello workspace, or select a user and click **REMOVE**.</span></span>

### <a name="toomanage-web-services-in-this-workspace"></a><span data-ttu-id="9e77f-156">a munkaterület toomanage webszolgáltatások</span><span class="sxs-lookup"><span data-stu-id="9e77f-156">toomanage web services in this workspace</span></span>
<span data-ttu-id="9e77f-157">Kattintson a hello **WEBSZOLGÁLTATÁSOK** fülre.</span><span class="sxs-lookup"><span data-stu-id="9e77f-157">Click hello **WEB SERVICES** tab.</span></span>

<span data-ttu-id="9e77f-158">Ez megjeleníti a munkaterületről közzétett webes szolgáltatások listáját.</span><span class="sxs-lookup"><span data-stu-id="9e77f-158">This displays a list of web services published from this workspace.</span></span>
<span data-ttu-id="9e77f-159">egy webszolgáltatás-bővítmény toomanage hello lista tooopen hello webszolgáltatás oldalát hello nevére kattint.</span><span class="sxs-lookup"><span data-stu-id="9e77f-159">toomanage a web service, click hello name in hello list tooopen hello Web service page.</span></span>

<span data-ttu-id="9e77f-160">Egy webszolgáltatás-bővítmény definiált egy vagy több végpontot is rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="9e77f-160">A Web service may have one or more endpoints defined.</span></span>

* <span data-ttu-id="9e77f-161">További végpontok hozzáadása toohello "Alapértelmezett" végpont is meghatározhatja.</span><span class="sxs-lookup"><span data-stu-id="9e77f-161">You can define more endpoints in addition toohello "Default" endpoint.</span></span> <span data-ttu-id="9e77f-162">tooadd hello végpont, kattintson a **végpontok kezelése** hello irányítópult tooopen hello Azure Machine Learning webszolgáltatások portal hello alján.</span><span class="sxs-lookup"><span data-stu-id="9e77f-162">tooadd hello endpoint, click **Manage Endpoints** at hello bottom of hello dashboard tooopen hello Azure Machine Learning Web Services portal.</span></span>
* <span data-ttu-id="9e77f-163">toodelete (hello "Alapértelmezett" végpont nem törölhetők) végpont, jelölje be a hello végpont sor hello elején hello jelölőnégyzetet, majd kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="9e77f-163">toodelete an endpoint (you cannot delete hello "Default" endpoint), click hello check box at hello beginning of hello endpoint row, and click **DELETE**.</span></span> <span data-ttu-id="9e77f-164">Ezzel eltávolítja hello végpont hello webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="9e77f-164">This removes hello endpoint from hello Web service.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="9e77f-165">Ha egy alkalmazás hello végpont törlése a hello webszolgáltatási végpontot, hello alkalmazás kap egy hiba hello legközelebb megkísérli tooaccess hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="9e77f-165">If an application is using hello web service endpoint when hello endpoint is deleted, hello application will receive an error hello next time it tries tooaccess hello service.</span></span>
  > 
  > 

<span data-ttu-id="9e77f-166">Kattintson egy webes szolgáltatás végpont tooopen hello nevére azt.</span><span class="sxs-lookup"><span data-stu-id="9e77f-166">Click hello name of a Web service endpoint tooopen it.</span></span> 

<span data-ttu-id="9e77f-167">A webes szolgáltatás teljes használatának hello irányítópult megtekintheti egy meghatározott időtartamra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="9e77f-167">From hello dashboard, you can view overall usage of your Web service over a period of time.</span></span> <span data-ttu-id="9e77f-168">Hello időszak tooview hello hello használati diagramok jobb felső sarkában található hello időszak legördülő menüből kiválaszthatja.</span><span class="sxs-lookup"><span data-stu-id="9e77f-168">You can select hello period tooview from hello Period dropdown menu in hello upper right of hello usage charts.</span></span> <span data-ttu-id="9e77f-169">hello irányítópult a következő információ hello jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="9e77f-169">hello dashboard shows hello following information:</span></span>

* <span data-ttu-id="9e77f-170">**Kérelmek keresztül idő** kérések száma hello lépés grafikonja kijelölt időszakban hello jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="9e77f-170">**Requests Over Time** displays a step graph of hello number of requests over hello selected time period.</span></span> <span data-ttu-id="9e77f-171">Segíthet megállapítani, hogy a használati igényeiben jelentkező problémát.</span><span class="sxs-lookup"><span data-stu-id="9e77f-171">It can help identify if you are experiencing spikes in usage.</span></span>
* <span data-ttu-id="9e77f-172">**Kérés-válasz kérelmek** hello teljes hello szolgáltatás által fogadott hello kijelölt időszakra vonatkozóan, és azok számát nem sikerült a kérés-válasz hívások száma.</span><span class="sxs-lookup"><span data-stu-id="9e77f-172">**Request-Response Requests** displays hello total number of Request-Response calls hello service has received over hello selected time period and how many of them failed.</span></span>
* <span data-ttu-id="9e77f-173">**Átlagos kérés-válasz számítási ideje** jeleníti meg, az átlagos hello idő szükséges tooexecute hello fogadott kérések.</span><span class="sxs-lookup"><span data-stu-id="9e77f-173">**Average Request-Response Compute Time** displays an average of hello time needed tooexecute hello received requests.</span></span>
* <span data-ttu-id="9e77f-174">**Kötegelt kérelmek** hello az összes száma kötegelt kérelmekben hello szolgáltatás kapott kijelölt időszakban hello keresztül, és azok számát nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="9e77f-174">**Batch Requests** displays hello total number of Batch Requests hello service has received over hello selected time period and how many of them failed.</span></span>
* <span data-ttu-id="9e77f-175">**Átlagos késés feladat** jeleníti meg, az átlagos hello idő szükséges tooexecute hello fogadott kérések.</span><span class="sxs-lookup"><span data-stu-id="9e77f-175">**Average Job Latency** displays an average of hello time needed tooexecute hello received requests.</span></span>
* <span data-ttu-id="9e77f-176">**Hibák** hello bekövetkezett hibák összesített számát jeleníti meg a hívások toohello webes szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="9e77f-176">**Errors** displays hello aggregate number of errors that have occurred on calls toohello web service.</span></span>
* <span data-ttu-id="9e77f-177">**Szolgáltatási költségei** hello hello szolgáltatáshoz tartozó számlázási csomag díjai hello jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="9e77f-177">**Services Costs** displays hello charges for hello billing plan associated with hello service.</span></span>

<span data-ttu-id="9e77f-178">Hello konfigurációs lapján, a következő tulajdonságai hello frissítheti:</span><span class="sxs-lookup"><span data-stu-id="9e77f-178">From hello Configure page, you can update hello following properties:</span></span>

* <span data-ttu-id="9e77f-179">**Leírás** lehetővé teszi a tooenter hello webszolgáltatás leírását.</span><span class="sxs-lookup"><span data-stu-id="9e77f-179">**Description** allows you tooenter a description for hello Web service.</span></span> <span data-ttu-id="9e77f-180">Leírás megadása kötelező.</span><span class="sxs-lookup"><span data-stu-id="9e77f-180">Description is a required field.</span></span>
* <span data-ttu-id="9e77f-181">**Naplózás** lehetővé teszi a hello végponton hibanaplózást tooenable vagy letiltása.</span><span class="sxs-lookup"><span data-stu-id="9e77f-181">**Logging** allows you tooenable or disable error logging on hello endpoint.</span></span> <span data-ttu-id="9e77f-182">A naplózást további információkért lásd: Enable [Machine Learning webszolgáltatások naplózása](machine-learning-web-services-logging.md).</span><span class="sxs-lookup"><span data-stu-id="9e77f-182">For more information on Logging, see Enable [logging for Machine Learning web services](machine-learning-web-services-logging.md).</span></span>
* <span data-ttu-id="9e77f-183">**Engedélyezze a mintaadatok** tooprovide mintaadatok használható tootest hello kérés-válasz szolgáltatás lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="9e77f-183">**Enable Sample data** allows you tooprovide sample data that you can use tootest hello Request-Response service.</span></span> <span data-ttu-id="9e77f-184">Ha a Machine Learning Studióban létrehozott hello webszolgáltatás, hello mintaadatok forrása hello adatok a használt tootrain a modell.</span><span class="sxs-lookup"><span data-stu-id="9e77f-184">If you created hello web service in Machine Learning Studio, hello sample data is taken from hello data your used tootrain your model.</span></span> <span data-ttu-id="9e77f-185">Ha programozottan létrehozott hello szolgáltatást, hello adatok hello JSON csomag részeként megadott hello példaadatokat származik.</span><span class="sxs-lookup"><span data-stu-id="9e77f-185">If you created hello service programmatically, hello data is taken from hello example data you provided as part of hello JSON package.</span></span>

