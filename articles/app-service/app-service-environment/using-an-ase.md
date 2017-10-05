---
title: "Azure App Service-környezet használata"
description: "Hogyan létrehozását, közzétételét és alkalmazásokat az Azure App Service-környezet skálázása"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: a22450c4-9b8b-41d4-9568-c4646f4cf66b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: 279951d40b7780120d0b94e183f06e00ccece016
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="use-an-app-service-environment"></a><span data-ttu-id="5249d-103">App Service-környezet használata</span><span class="sxs-lookup"><span data-stu-id="5249d-103">Use an App Service environment</span></span> #

## <a name="overview"></a><span data-ttu-id="5249d-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="5249d-104">Overview</span></span> ##

<span data-ttu-id="5249d-105">Az Azure App Service Environment-környezet az Azure App Service egy alhálózatba Azure virtuális hálózatban egy ügyfél központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="5249d-105">Azure App Service Environment is a deployment of Azure App Service into a subnet in a customer’s Azure virtual network.</span></span> <span data-ttu-id="5249d-106">Áll:</span><span class="sxs-lookup"><span data-stu-id="5249d-106">It consists of:</span></span>

- <span data-ttu-id="5249d-107">**Előtérkiszolgáló-végpontok**: az előtér-webkiszolgálóinak, ahol a HTTP/HTTPS egy App Service-környezetben (ASE) leáll.</span><span class="sxs-lookup"><span data-stu-id="5249d-107">**Front ends**: The front ends are where HTTP/HTTPS terminates in an App Service environment (ASE).</span></span>
- <span data-ttu-id="5249d-108">**Feldolgozók**: A munkavállalók az erőforrásokat, amelyek az alkalmazások tárolására.</span><span class="sxs-lookup"><span data-stu-id="5249d-108">**Workers**: The workers are the resources that host your apps.</span></span>
- <span data-ttu-id="5249d-109">**Adatbázis**: az adatbázis tartalmazza, amely meghatározza a környezetben.</span><span class="sxs-lookup"><span data-stu-id="5249d-109">**Database**: The database holds information that defines the environment.</span></span>
- <span data-ttu-id="5249d-110">**Tárolási**: A tárolót használja a rendszer az ügyfél-közzétett alkalmazások futtatásához.</span><span class="sxs-lookup"><span data-stu-id="5249d-110">**Storage**: The storage is used to host the customer-published apps.</span></span>

> [!NOTE]
> <span data-ttu-id="5249d-111">App Service Environment-környezet két verziója van: ASEv1 és ASEv2.</span><span class="sxs-lookup"><span data-stu-id="5249d-111">There are two versions of App Service Environment: ASEv1 and ASEv2.</span></span> <span data-ttu-id="5249d-112">ASEv1 az erőforrások kell kezelni, mielőtt használhatná azokat.</span><span class="sxs-lookup"><span data-stu-id="5249d-112">In ASEv1, you must manage the resources before you can use them.</span></span> <span data-ttu-id="5249d-113">Megtudhatja, hogyan konfigurálhatja és kezelheti a ASEv1, lásd: [konfigurálása egy App Service-környezet v1][ConfigureASEv1].</span><span class="sxs-lookup"><span data-stu-id="5249d-113">To learn how to configure and manage ASEv1, see [Configure an App Service environment v1][ConfigureASEv1].</span></span> <span data-ttu-id="5249d-114">Ez a cikk többi ASEv2 összpontosít.</span><span class="sxs-lookup"><span data-stu-id="5249d-114">The rest of this article focuses on ASEv2.</span></span>
>
>

<span data-ttu-id="5249d-115">Telepíthet egy ASE (ASEv1 és ASEv2) egy külső vagy belső virtuális IP-címre az alkalmazás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="5249d-115">You can deploy an ASE (ASEv1 and ASEv2) with an external or internal VIP for app access.</span></span> <span data-ttu-id="5249d-116">A központi telepítés egy külső virtuális IP-címre gyakran nevezik egy külső ASE.</span><span class="sxs-lookup"><span data-stu-id="5249d-116">The deployment with an external VIP is commonly called an External ASE.</span></span> <span data-ttu-id="5249d-117">A belső verzió a ILB ASE nevezik, mert egy belső terheléselosztón (ILB) használ.</span><span class="sxs-lookup"><span data-stu-id="5249d-117">The internal version is called the ILB ASE because it uses an internal load balancer (ILB).</span></span> <span data-ttu-id="5249d-118">A ILB ASE kapcsolatos további információkért lásd: [létrehozása és használata egy ILB ASE][MakeILBASE].</span><span class="sxs-lookup"><span data-stu-id="5249d-118">To learn more about the ILB ASE, see [Create and use an ILB ASE][MakeILBASE].</span></span>

## <a name="create-a-web-app-in-an-ase"></a><span data-ttu-id="5249d-119">Webalkalmazás létrehozása a Service-környezetben</span><span class="sxs-lookup"><span data-stu-id="5249d-119">Create a web app in an ASE</span></span> ##

<span data-ttu-id="5249d-120">Egy webalkalmazás létrehozása a Service-környezetben, azt ugyanazzal az eljárással, általában létrehozása, de néhány kisebb különbség az.</span><span class="sxs-lookup"><span data-stu-id="5249d-120">To create a web app in an ASE, you use the same process as when you create it normally, but with a few small differences.</span></span> <span data-ttu-id="5249d-121">Mikor hozzon létre egy új App Service-csomagot:</span><span class="sxs-lookup"><span data-stu-id="5249d-121">When you create a new App Service plan:</span></span>

- <span data-ttu-id="5249d-122">Helyett válassza a földrajzi helyet, ahol az alkalmazás telepítéséhez, egy ASE az helyeként válassza.</span><span class="sxs-lookup"><span data-stu-id="5249d-122">Instead of choosing a geographic location in which to deploy your app, you choose an ASE as your location.</span></span>
- <span data-ttu-id="5249d-123">-Környezetben létrehozott összes App Service-csomagok IP-címek egy elszigetelt kell lennie.</span><span class="sxs-lookup"><span data-stu-id="5249d-123">All App Service plans created in an ASE must be in an Isolated pricing tier.</span></span>

<span data-ttu-id="5249d-124">Ha még nem rendelkezik egy ASE, utasításait követve létrehozhat [App Service-környezet létrehozása][MakeExternalASE].</span><span class="sxs-lookup"><span data-stu-id="5249d-124">If you don't have an ASE, you can create one by following the instructions in [Create an App Service environment][MakeExternalASE].</span></span>

<span data-ttu-id="5249d-125">A webalkalmazás létrehozása a Service-környezetben:</span><span class="sxs-lookup"><span data-stu-id="5249d-125">To create a web app in an ASE:</span></span>

1. <span data-ttu-id="5249d-126">Válassza ki **új** > **Web + mobil** > **webalkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="5249d-126">Select **New** > **Web + Mobile** > **Web App**.</span></span>

2. <span data-ttu-id="5249d-127">Adja meg a webalkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="5249d-127">Enter a name for the web app.</span></span> <span data-ttu-id="5249d-128">Ha már ki az App Service-csomag Service-környezetben, a tartománynév, az alkalmazás a ASE tartomány nevét jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="5249d-128">If you already selected an App Service plan in an ASE, the domain name for the app reflects the domain name of the ASE.</span></span>

    ![Webes alkalmazás nevének kiválasztása][1]

3. <span data-ttu-id="5249d-130">Válasszon egy előfizetést.</span><span class="sxs-lookup"><span data-stu-id="5249d-130">Select a subscription.</span></span>

4. <span data-ttu-id="5249d-131">Adjon meg egy új erőforráscsoport nevét, vagy válasszon **meglévő** , és válassza ki a legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="5249d-131">Enter a name for a new resource group, or select **Use existing** and select one from the drop-down list.</span></span>

5. <span data-ttu-id="5249d-132">Válassza ki a ASE meglévő App Service-csomagot, vagy hozzon létre egy újat az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="5249d-132">Select an existing App Service plan in your ASE, or create a new one by following these steps:</span></span>

    <span data-ttu-id="5249d-133">a.</span><span class="sxs-lookup"><span data-stu-id="5249d-133">a.</span></span> <span data-ttu-id="5249d-134">Válassza ki **új**.</span><span class="sxs-lookup"><span data-stu-id="5249d-134">Select **Create New**.</span></span>

    <span data-ttu-id="5249d-135">b.</span><span class="sxs-lookup"><span data-stu-id="5249d-135">b.</span></span> <span data-ttu-id="5249d-136">Adja meg az App Service-csomag nevét.</span><span class="sxs-lookup"><span data-stu-id="5249d-136">Enter the name for your App Service plan.</span></span>

    <span data-ttu-id="5249d-137">c.</span><span class="sxs-lookup"><span data-stu-id="5249d-137">c.</span></span> <span data-ttu-id="5249d-138">Válassza ki a ASE a a **hely** legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="5249d-138">Select your ASE in the **Location** drop-down list.</span></span>

    <span data-ttu-id="5249d-139">d.</span><span class="sxs-lookup"><span data-stu-id="5249d-139">d.</span></span> <span data-ttu-id="5249d-140">Válasszon egy **elszigetelt** tarifacsomagra vált.</span><span class="sxs-lookup"><span data-stu-id="5249d-140">Select an **Isolated** pricing tier.</span></span> <span data-ttu-id="5249d-141">Válassza ki **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="5249d-141">Select **Select**.</span></span>

    <span data-ttu-id="5249d-142">e.</span><span class="sxs-lookup"><span data-stu-id="5249d-142">e.</span></span> <span data-ttu-id="5249d-143">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="5249d-143">Select **OK**.</span></span>
    
    ![Elkülönített tarifacsomagok][2]

6. <span data-ttu-id="5249d-145">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="5249d-145">Select **Create**.</span></span>

## <a name="how-scale-works"></a><span data-ttu-id="5249d-146">Hogyan méretezése működik</span><span class="sxs-lookup"><span data-stu-id="5249d-146">How scale works</span></span> ##

<span data-ttu-id="5249d-147">Az App Service-csomag minden App Service-alkalmazást futtat.</span><span class="sxs-lookup"><span data-stu-id="5249d-147">Every App Service app runs in an App Service plan.</span></span> <span data-ttu-id="5249d-148">A tároló modell környezetek App Service-csomagok tárolására, de App Service-csomagok tárolására az alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="5249d-148">The container model is environments hold App Service plans, and App Service plans hold apps.</span></span> <span data-ttu-id="5249d-149">Akkor az alkalmazások, az App Service-csomag vertikális, és így az alkalmazások méretezése a csomagot.</span><span class="sxs-lookup"><span data-stu-id="5249d-149">When you scale an app, you scale the App Service plan and thus scale all the apps in the same plan.</span></span>

<span data-ttu-id="5249d-150">A ASEv2 akkor az App Service-csomag, a szükséges infrastruktúra automatikusan kerül.</span><span class="sxs-lookup"><span data-stu-id="5249d-150">In ASEv2, when you scale an App Service plan, the needed infrastructure is automatically added.</span></span> <span data-ttu-id="5249d-151">Nincs a skálázási műveletek késleltetés során az infrastruktúra kerül.</span><span class="sxs-lookup"><span data-stu-id="5249d-151">There is a time delay to scale operations while the infrastructure is added.</span></span> <span data-ttu-id="5249d-152">A ASEv1 a szükséges infrastruktúra hozzá kell adnia hoz létre, vagy az App Service-csomag kiterjesztése.</span><span class="sxs-lookup"><span data-stu-id="5249d-152">In ASEv1, the needed infrastructure must be added before you can create or scale out your App Service plan.</span></span> 

<span data-ttu-id="5249d-153">A több-bérlős App Service, a méretezés oka általában azonnali erőforrások készlete áll rendelkezésre annak támogatásához.</span><span class="sxs-lookup"><span data-stu-id="5249d-153">In the multitenant App Service, scaling is usually immediate because a pool of resources is readily available to support it.</span></span> <span data-ttu-id="5249d-154">-Környezetben nincs ilyen puffer, és szükség esetén erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="5249d-154">In an ASE, there is no such buffer, and resources are allocated upon need.</span></span>

<span data-ttu-id="5249d-155">Service-környezetben akár 100 példányok méretezheti.</span><span class="sxs-lookup"><span data-stu-id="5249d-155">In an ASE, you can scale up to 100 instances.</span></span> <span data-ttu-id="5249d-156">100 logikailemez egy egyetlen App Service-csomag összes lehetnek, vagy több App Service-csomagokról pontjain.</span><span class="sxs-lookup"><span data-stu-id="5249d-156">Those 100 instances can be all in one single App Service plan or distributed across multiple App Service plans.</span></span>

## <a name="ip-addresses"></a><span data-ttu-id="5249d-157">IP-címek</span><span class="sxs-lookup"><span data-stu-id="5249d-157">IP addresses</span></span> ##

<span data-ttu-id="5249d-158">Egy alkalmazás egy dedikált IP-címet lefoglalni az App Service képes a.</span><span class="sxs-lookup"><span data-stu-id="5249d-158">App Service has the ability to allocate a dedicated IP address to an app.</span></span> <span data-ttu-id="5249d-159">Ez a funkció érhető el az IP-alapú SSL konfigurálása után a [egy meglévő egyéni SSL-tanúsítvány kötését az Azure-webalkalmazásokban][ConfigureSSL].</span><span class="sxs-lookup"><span data-stu-id="5249d-159">This capability is available after you configure an IP-based SSL, as described in [Bind an existing custom SSL certificate to Azure web apps][ConfigureSSL].</span></span> <span data-ttu-id="5249d-160">-Környezetben van azonban a figyelmet a jelentősebb kivételt.</span><span class="sxs-lookup"><span data-stu-id="5249d-160">However, in an ASE, there is a notable exception.</span></span> <span data-ttu-id="5249d-161">Az IP-alapú SSL-Példánynak környezetben használandó további IP-cím nem vehető fel.</span><span class="sxs-lookup"><span data-stu-id="5249d-161">You can't add additional IP addresses to be used for an IP-based SSL in an ILB ASE.</span></span>

<span data-ttu-id="5249d-162">ASEv1 meg kell kiosztani az IP-címek erőforrások előtt is használhatja.</span><span class="sxs-lookup"><span data-stu-id="5249d-162">In ASEv1, you need to allocate the IP addresses as resources before you can use them.</span></span> <span data-ttu-id="5249d-163">ASEv2 használhatja őket az alkalmazásból, mint hogy az a több-bérlős App Service.</span><span class="sxs-lookup"><span data-stu-id="5249d-163">In ASEv2, you use them from your app just as you do in the multitenant App Service.</span></span> <span data-ttu-id="5249d-164">Nincs mindig egy tartalék cím ASEv2 legfeljebb 30 IP-címet.</span><span class="sxs-lookup"><span data-stu-id="5249d-164">There is always one spare address in ASEv2 up to 30 IP addresses.</span></span> <span data-ttu-id="5249d-165">Minden alkalommal, amikor valamelyik használja, egy másik jelenik meg, hogy egy címet mindig könnyen hozzáférhető legyen.</span><span class="sxs-lookup"><span data-stu-id="5249d-165">Each time you use one, another is added so that an address is always readily available for use.</span></span> <span data-ttu-id="5249d-166">Gyors egymásutánban címek késleltetési idő legyen szükséges, egy másik IP-címet, amely megakadályozza a hozzáadása IP lefoglalni időpontot.</span><span class="sxs-lookup"><span data-stu-id="5249d-166">A time delay is required to allocate another IP address, which prevents adding IP addresses in quick succession.</span></span>

## <a name="front-end-scaling"></a><span data-ttu-id="5249d-167">Előtér-skálázás</span><span class="sxs-lookup"><span data-stu-id="5249d-167">Front-end scaling</span></span> ##

<span data-ttu-id="5249d-168">A ASEv2 amikor az App Service-csomagokról a horizontális munkavállalók automatikusan támogatja azokat.</span><span class="sxs-lookup"><span data-stu-id="5249d-168">In ASEv2, when you scale out your App Service plans, workers are automatically added to support them.</span></span> <span data-ttu-id="5249d-169">Minden ASE két előtér-webkiszolgálóinak hozza létre.</span><span class="sxs-lookup"><span data-stu-id="5249d-169">Every ASE is created with two front ends.</span></span> <span data-ttu-id="5249d-170">Továbbá az első karakterrel végződik automatikusan minden 15 példányok egy előtér sebességgel kibővítési az App Service-csomagokról.</span><span class="sxs-lookup"><span data-stu-id="5249d-170">In addition, the front ends automatically scale out at a rate of one front end for every 15 instances in your App Service plans.</span></span> <span data-ttu-id="5249d-171">Például ha 15, akkor rendelkezik három előtér-webkiszolgálóinak.</span><span class="sxs-lookup"><span data-stu-id="5249d-171">For example, if you have 15 instances, then you have three front ends.</span></span> <span data-ttu-id="5249d-172">Ha 30 példányokhoz méretezni, majd, hogy négy első befejeződik, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="5249d-172">If you scale to 30 instances, then you have four front ends, and so on.</span></span>

<span data-ttu-id="5249d-173">Ez a szám előtér-webkiszolgálóinak bőven elegendő a legtöbb forgatókönyvek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="5249d-173">This number of front ends should be more than enough for most scenarios.</span></span> <span data-ttu-id="5249d-174">Azonban ki lehet terjeszteni gyorsabban.</span><span class="sxs-lookup"><span data-stu-id="5249d-174">However, you can scale out at a faster rate.</span></span> <span data-ttu-id="5249d-175">Módosíthatja a arány minden öt példányok mindig egy első befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="5249d-175">You can change the ratio to as low as one front end for every five instances.</span></span> <span data-ttu-id="5249d-176">Nincs a arány módosítására járnak.</span><span class="sxs-lookup"><span data-stu-id="5249d-176">There is a charge for changing the ratio.</span></span> <span data-ttu-id="5249d-177">További információkért lásd: [Azure App Service szolgáltatás díjszabása][Pricing].</span><span class="sxs-lookup"><span data-stu-id="5249d-177">For more information, see [Azure App Service pricing][Pricing].</span></span>

<span data-ttu-id="5249d-178">Előtér-erőforrások a HTTP/HTTPS-végpont a ASE a rendszer.</span><span class="sxs-lookup"><span data-stu-id="5249d-178">Front-end resources are the HTTP/HTTPS endpoint for the ASE.</span></span> <span data-ttu-id="5249d-179">Az alapértelmezett előtér-konfigurációval előtér / memóriahasználatát érték következetesen körülbelül 60 %.</span><span class="sxs-lookup"><span data-stu-id="5249d-179">With the default front-end configuration, memory usage per front end is consistently around 60 percent.</span></span> <span data-ttu-id="5249d-180">Ügyfél munkaterheléseinek előtér nem futtatható.</span><span class="sxs-lookup"><span data-stu-id="5249d-180">Customer workloads don't run on a front end.</span></span> <span data-ttu-id="5249d-181">Méretezési illetően előtér kulcsfontosságú tényező a CPU, amelyek célja a elsősorban HTTPS-forgalmat.</span><span class="sxs-lookup"><span data-stu-id="5249d-181">The key factor for a front end with respect to scale is the CPU, which is driven primarily by HTTPS traffic.</span></span>

## <a name="app-access"></a><span data-ttu-id="5249d-182">Alkalmazás-hozzáférés</span><span class="sxs-lookup"><span data-stu-id="5249d-182">App access</span></span> ##

<span data-ttu-id="5249d-183">Service-környezetben külső az alkalmazások létrehozásakor használt tartomány eltér a több-bérlős App Service.</span><span class="sxs-lookup"><span data-stu-id="5249d-183">In an External ASE, the domain that's used when you create apps is different from the multitenant App Service.</span></span> <span data-ttu-id="5249d-184">Ez magában foglalja a ASE nevét.</span><span class="sxs-lookup"><span data-stu-id="5249d-184">It includes the name of the ASE.</span></span> <span data-ttu-id="5249d-185">Egy külső ASE létrehozásával kapcsolatos további információkért lásd: [App Service-környezet létrehozása][MakeExternalASE].</span><span class="sxs-lookup"><span data-stu-id="5249d-185">For more information on how to create an External ASE, see [Create an App Service environment][MakeExternalASE].</span></span> <span data-ttu-id="5249d-186">A tartománynév-külső környezetben a következőképpen néz *.&lt; asename környezet&gt;. p.azurewebsites.net*.</span><span class="sxs-lookup"><span data-stu-id="5249d-186">The domain name in an External ASE looks like *.&lt;asename&gt;.p.azurewebsites.net*.</span></span> <span data-ttu-id="5249d-187">Például, ha a ASE nevű _külső-ase_ és nevű alkalmazás működteti _contoso_ abban a ASE, lépjen a következő URL-címek:</span><span class="sxs-lookup"><span data-stu-id="5249d-187">For example, if your ASE is named _external-ase_ and you host an app called _contoso_ in that ASE, you reach it at the following URLs:</span></span>

- <span data-ttu-id="5249d-188">contoso.external-ase.p.azurewebsites.net</span><span class="sxs-lookup"><span data-stu-id="5249d-188">contoso.external-ase.p.azurewebsites.net</span></span>
- <span data-ttu-id="5249d-189">contoso.SCM.external-ase.p.azurewebsites.net</span><span class="sxs-lookup"><span data-stu-id="5249d-189">contoso.scm.external-ase.p.azurewebsites.net</span></span>

<span data-ttu-id="5249d-190">Az URL-cím contoso.scm.external ase.p.azurewebsites.net szolgál a Kudu konzol eléréséhez, vagy az alkalmazás-közzététel webes telepítése.</span><span class="sxs-lookup"><span data-stu-id="5249d-190">The URL contoso.scm.external-ase.p.azurewebsites.net is used to access the Kudu console or for publishing your app by using web deploy.</span></span> <span data-ttu-id="5249d-191">A Kudu konzolon információkért lásd: [Kudu konzol az Azure App Service-][Kudu].</span><span class="sxs-lookup"><span data-stu-id="5249d-191">For information on the Kudu console, see [Kudu console for Azure App Service][Kudu].</span></span> <span data-ttu-id="5249d-192">A Kudu konzol lehetővé teszi egy webes felhasználói felület hibakeresés, fájlok feltöltése, szerkesztheti, és még sok más.</span><span class="sxs-lookup"><span data-stu-id="5249d-192">The Kudu console gives you a web UI for debugging, uploading files, editing files, and much more.</span></span>

<span data-ttu-id="5249d-193">-Példánynak környezetben határozhatja meg a tartomány a központi telepítéskor.</span><span class="sxs-lookup"><span data-stu-id="5249d-193">In an ILB ASE, you determine the domain at deployment time.</span></span> <span data-ttu-id="5249d-194">Egy ILB ASE létrehozásával kapcsolatos további információkért lásd: [létrehozása és használata egy ILB ASE][MakeILBASE].</span><span class="sxs-lookup"><span data-stu-id="5249d-194">For more information on how to create an ILB ASE, see [Create and use an ILB ASE][MakeILBASE].</span></span> <span data-ttu-id="5249d-195">Ha a tartomány nevét adja meg _ilb-ase.info_, az alkalmazásokat, hogy ASE használja, hogy a tartomány létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="5249d-195">If you specify the domain name _ilb-ase.info_, the apps in that ASE use that domain during app creation.</span></span> <span data-ttu-id="5249d-196">Az alkalmazás nevű _contoso_, az URL-címei:</span><span class="sxs-lookup"><span data-stu-id="5249d-196">For the app named _contoso_, the URLs are:</span></span>

- <span data-ttu-id="5249d-197">contoso.ilb-ase.info</span><span class="sxs-lookup"><span data-stu-id="5249d-197">contoso.ilb-ase.info</span></span>
- <span data-ttu-id="5249d-198">contoso.SCM.ilb-ase.info</span><span class="sxs-lookup"><span data-stu-id="5249d-198">contoso.scm.ilb-ase.info</span></span>

## <a name="publishing"></a><span data-ttu-id="5249d-199">Közzététel</span><span class="sxs-lookup"><span data-stu-id="5249d-199">Publishing</span></span> ##

<span data-ttu-id="5249d-200">Csakúgy, mint a több bérlős App Service-környezetben is közzéteheti a:</span><span class="sxs-lookup"><span data-stu-id="5249d-200">As with the multitenant App Service, in an ASE you can publish with:</span></span>

- <span data-ttu-id="5249d-201">A webes telepítése.</span><span class="sxs-lookup"><span data-stu-id="5249d-201">Web deployment.</span></span>
- <span data-ttu-id="5249d-202">AZ FTP.</span><span class="sxs-lookup"><span data-stu-id="5249d-202">FTP.</span></span>
- <span data-ttu-id="5249d-203">Folyamatos integrációt.</span><span class="sxs-lookup"><span data-stu-id="5249d-203">Continuous integration.</span></span>
- <span data-ttu-id="5249d-204">Adatértékmezők áthúzása a Kudu konzolon.</span><span class="sxs-lookup"><span data-stu-id="5249d-204">Drag and drop in the Kudu console.</span></span>
- <span data-ttu-id="5249d-205">Egy IDE, mint például a Visual Studio, az eclipse-ben vagy az IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="5249d-205">An IDE, such as Visual Studio, Eclipse, or IntelliJ IDEA.</span></span>

<span data-ttu-id="5249d-206">Egy külső mértékéig e közzétételi beállítások viselkedése azonos.</span><span class="sxs-lookup"><span data-stu-id="5249d-206">With an External ASE, these publishing options all behave the same.</span></span> <span data-ttu-id="5249d-207">További információkért lásd: [telepítése az Azure App Service][AppDeploy].</span><span class="sxs-lookup"><span data-stu-id="5249d-207">For more information, see [Deployment in Azure App Service][AppDeploy].</span></span> 

<span data-ttu-id="5249d-208">A közzététel a fő különbség van egy ILB ASE tekintetében.</span><span class="sxs-lookup"><span data-stu-id="5249d-208">The major difference with publishing is with respect to an ILB ASE.</span></span> <span data-ttu-id="5249d-209">Egy ILB mértékéig a közzétételi végpontok elérhetők minden csak a Példánynak.</span><span class="sxs-lookup"><span data-stu-id="5249d-209">With an ILB ASE, the publishing endpoints are all available only through the ILB.</span></span> <span data-ttu-id="5249d-210">A Példánynak virtuális hálózatban ASE alhálózaton magánhálózati IP-címe van.</span><span class="sxs-lookup"><span data-stu-id="5249d-210">The ILB is on a private IP in the ASE subnet in the virtual network.</span></span> <span data-ttu-id="5249d-211">Ha nincs a Példánynak a hálózati hozzáférést, olyan alkalmazások, hogy ASE nem tehető közzé.</span><span class="sxs-lookup"><span data-stu-id="5249d-211">If you don’t have network access to the ILB, you can't publish any apps on that ASE.</span></span> <span data-ttu-id="5249d-212">Leírtaknak megfelelően [létrehozása és használata egy ILB ASE][MakeILBASE], konfigurálnia kell DNS az alkalmazások a rendszerben.</span><span class="sxs-lookup"><span data-stu-id="5249d-212">As noted in [Create and use an ILB ASE][MakeILBASE], you need to configure DNS for the apps in the system.</span></span> <span data-ttu-id="5249d-213">Az SCM végpontot, amely tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="5249d-213">That includes the SCM endpoint.</span></span> <span data-ttu-id="5249d-214">Ha ezek még nincs megfelelően beállítva, nem tehető közzé.</span><span class="sxs-lookup"><span data-stu-id="5249d-214">If they're not defined properly, you can't publish.</span></span> <span data-ttu-id="5249d-215">A IDEs is kell rendelkeznie a Példánynak a hálózati hozzáférést közvetlenül való közzétételhez.</span><span class="sxs-lookup"><span data-stu-id="5249d-215">Your IDEs also need to have network access to the ILB in order to publish directly to it.</span></span>

<span data-ttu-id="5249d-216">Internet alapú CI rendszereknek, például a Githubon és a Visual Studio Team Services, egy ILB mértékéig nem működnek, mert a közzétételi végpont nem érhető el az Internet.</span><span class="sxs-lookup"><span data-stu-id="5249d-216">Internet-based CI systems, such as GitHub and Visual Studio Team Services, don't work with an ILB ASE because the publishing endpoint is not Internet accessible.</span></span> <span data-ttu-id="5249d-217">Ehelyett kell használnia, amely lekéréses modellt használ, például a Dropbox CI rendszer.</span><span class="sxs-lookup"><span data-stu-id="5249d-217">Instead, you need to use a CI system that uses a pull model, such as Dropbox.</span></span>

<span data-ttu-id="5249d-218">A közzétételi-Példánynak környezetben alkalmazások végpontjainak használja a tartományban, amelyhez a Példánynak ASE hozták létre.</span><span class="sxs-lookup"><span data-stu-id="5249d-218">The publishing endpoints for apps in an ILB ASE use the domain that the ILB ASE was created with.</span></span> <span data-ttu-id="5249d-219">Az alkalmazás közzétételi profil és az alkalmazás portálpaneljéhez láthatja (a **áttekintése** > **Essentials** és is **tulajdonságok**).</span><span class="sxs-lookup"><span data-stu-id="5249d-219">You can see it in the app's publishing profile and in the app's portal blade (in **Overview** > **Essentials** and also in **Properties**).</span></span> 

## <a name="pricing"></a><span data-ttu-id="5249d-220">Díjszabás</span><span class="sxs-lookup"><span data-stu-id="5249d-220">Pricing</span></span> ##

<span data-ttu-id="5249d-221">Az árképzés SKU nevű **elszigetelt** csak ASEv2 való használatra készült.</span><span class="sxs-lookup"><span data-stu-id="5249d-221">The pricing SKU called **Isolated** was created for use only with ASEv2.</span></span> <span data-ttu-id="5249d-222">A Termékváltozat árképzési elszigetelt ASEv2 tárolt összes App Service-csomagokról találhatók.</span><span class="sxs-lookup"><span data-stu-id="5249d-222">All App Service plans that are hosted in ASEv2 are in the Isolated pricing SKU.</span></span> <span data-ttu-id="5249d-223">App Service-csomag elkülönített díjszabás régiónként eltérőek lehetnek.</span><span class="sxs-lookup"><span data-stu-id="5249d-223">Isolated App Service plan rates can vary per region.</span></span> 

<span data-ttu-id="5249d-224">Az App Service-csomagokról ára mellett nincs ASE magát egy simán arány.</span><span class="sxs-lookup"><span data-stu-id="5249d-224">In addition to the price for your App Service plans, there is a flat rate for ASE itself.</span></span> <span data-ttu-id="5249d-225">Simán sebessége a ASE mérete nem változik, és egy alapértelmezett további 1 aránya skálázás ASE infrastruktúra fizet előtér minden 15 App Service-csomag példányok.</span><span class="sxs-lookup"><span data-stu-id="5249d-225">The flat rate doesn't change with the size of your ASE and pays for the ASE infrastructure at a default scaling rate of 1 additional front-end for every 15 App Service plan instances.</span></span>  

<span data-ttu-id="5249d-226">Ha az alapértelmezett méretezési minden 15 App Service-csomag példányok 1 előtér mérték nem elég gyors, mely első vége hozzáadott vagy a bejárati-végpontok méretének aránya módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="5249d-226">If the default scale rate of 1 front end for every 15 App Service plan instances is not fast enough, you can adjust the ratio at which front-ends are added or the size of the front-ends.</span></span>  <span data-ttu-id="5249d-227">Ha módosítja a arány vagy méretét, kell fizetnie az előtér-mag, amely alapértelmezés szerint nem lesz hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="5249d-227">When you adjust the ratio or size, you pay for the front-end cores that would not be added by default.</span></span>  

<span data-ttu-id="5249d-228">Például ha a 10-re méretarány, előtér megjelenik minden 10 példányok az App Service-csomagokról.</span><span class="sxs-lookup"><span data-stu-id="5249d-228">For example, if you adjust the scale ratio to 10, a front end is added for every 10 instances in your App Service plans.</span></span> <span data-ttu-id="5249d-229">Az egyszerű díj hozzá van rendelve egy előtér minden 15-példányok méretezési mértéke.</span><span class="sxs-lookup"><span data-stu-id="5249d-229">The flat fee covers a scale rate of one front end for every 15 instances.</span></span> <span data-ttu-id="5249d-230">10 méretarány, a díj a 10 App Service-csomag példányok hozzáadott harmadik előtér fizetnie.</span><span class="sxs-lookup"><span data-stu-id="5249d-230">With a scale ratio of 10, you pay a fee for the third front end that's added for the 10 App Service plan instances.</span></span> <span data-ttu-id="5249d-231">Nem kell azt a után kell fizetnie, 15 példányok elérésekor, mert automatikusan hozzáadják.</span><span class="sxs-lookup"><span data-stu-id="5249d-231">You don't need to pay for it when you reach 15 instances because it was added automatically.</span></span>

<span data-ttu-id="5249d-232">Ha 2 mag az első-vége a méretet, de nem módosíthatja a arány majd kell fizetnie a felesleges magok.</span><span class="sxs-lookup"><span data-stu-id="5249d-232">If you adjusted the size of the front-ends to 2 cores but do not adjust the ratio then you pay for the extra cores.</span></span>  <span data-ttu-id="5249d-233">Egy ASE 2 első részek, így még akkor kell fizetnie 2 extra mag 2 mag első célok mérete nagyobb, ha automatikus méretezési küszöbérték alatt hozza létre.</span><span class="sxs-lookup"><span data-stu-id="5249d-233">An ASE is created with 2 front-ends, so even below the automatic scaling threshold you would pay for 2 extra cores if you increased the size to 2 core front-ends.</span></span>

<span data-ttu-id="5249d-234">További információkért lásd: [Azure App Service szolgáltatás díjszabása][Pricing].</span><span class="sxs-lookup"><span data-stu-id="5249d-234">For more information, see [Azure App Service pricing][Pricing].</span></span>

## <a name="delete-an-ase"></a><span data-ttu-id="5249d-235">Egy ASE törlése</span><span class="sxs-lookup"><span data-stu-id="5249d-235">Delete an ASE</span></span> ##

<span data-ttu-id="5249d-236">Egy ASE törlése:</span><span class="sxs-lookup"><span data-stu-id="5249d-236">To delete an ASE:</span></span> 

1. <span data-ttu-id="5249d-237">Használjon **törlése** tetején a **App Service Environment-környezet** panelen.</span><span class="sxs-lookup"><span data-stu-id="5249d-237">Use **Delete** at the top of the **App Service Environment** blade.</span></span> 

2. <span data-ttu-id="5249d-238">Adja meg annak megerősítéséhez, hogy törli-e a ASE nevét.</span><span class="sxs-lookup"><span data-stu-id="5249d-238">Enter the name of your ASE to confirm that you want to delete it.</span></span> <span data-ttu-id="5249d-239">Ha töröl egy ASE, összes benne lévő tartalom is törli.</span><span class="sxs-lookup"><span data-stu-id="5249d-239">When you delete an ASE, you delete all of the content within it as well.</span></span> 

    ![ASE törlése][3]

<!--Image references-->
[1]: ./media/using_an_app_service_environment/usingase-appcreate.png
[2]: ./media/using_an_app_service_environment/usingase-pricingtiers.png
[3]: ./media/using_an_app_service_environment/usingase-delete.png


<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[ASEReadme]: ./readme.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/virtual-networks-nsg.md
[ConfigureASEv1]: ../../app-service-web/app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: ../../app-service-web/app-service-app-service-environment-intro.md
[webapps]: ../../app-service-web/app-service-web-overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[APIapps]: ../../app-service-api/app-service-api-apps-why-best-platform.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../../app-service-web/web-sites-purchase-ssl-web-site.md
[Kudu]: http://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[AppDeploy]: ../../app-service-web/web-sites-deploy.md
[ASEWAF]: ../../app-service-web/app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
