---
title: "Az API Management használata AzureML webszolgáltatások kezelése |} Microsoft Docs"
description: "Egy útmutató bemutatja, hogyan kezelheti az AzureML-webszolgáltatások API Management használata."
keywords: "gépi tanulási, az api management"
services: machine-learning
documentationcenter: 
author: roalexan
manager: jhubbard
editor: 
ms.assetid: 05150ae1-5b6a-4d25-ac67-fb2f24a68e8d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2017
ms.author: roalexan
ms.openlocfilehash: 65eff3f4971f79886a840bb19bf76aaab48878de
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="learn-how-to-manage-azureml-web-services-using-api-management"></a><span data-ttu-id="31a41-104">AzureML webszolgáltatások kezelése az API Management használatával</span><span class="sxs-lookup"><span data-stu-id="31a41-104">Learn how to manage AzureML web services using API Management</span></span>
## <a name="overview"></a><span data-ttu-id="31a41-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="31a41-105">Overview</span></span>
<span data-ttu-id="31a41-106">Ez az útmutató bemutatja, hogyan gyorsan megkezdheti az API Management az AzureML webszolgáltatások kezeli.</span><span class="sxs-lookup"><span data-stu-id="31a41-106">This guide shows you how to quickly get started using API Management to manage your AzureML web services.</span></span>

## <a name="what-is-azure-api-management"></a><span data-ttu-id="31a41-107">Mi az Azure API Management?</span><span class="sxs-lookup"><span data-stu-id="31a41-107">What is Azure API Management?</span></span>
<span data-ttu-id="31a41-108">Az Azure API-kezelés nem az Azure-szolgáltatások, amely lehetővé teszi a REST API-végpontokat kezelheti a felhasználói hozzáférés-szabályozás és figyelési irányítópult meghatározásával.</span><span class="sxs-lookup"><span data-stu-id="31a41-108">Azure API Management is an Azure service that lets you manage your REST API endpoints by defining user access, usage throttling, and dashboard monitoring.</span></span> <span data-ttu-id="31a41-109">Kattintson a [Itt](https://azure.microsoft.com/services/api-management/) Azure API Management leírását.</span><span class="sxs-lookup"><span data-stu-id="31a41-109">Click [here](https://azure.microsoft.com/services/api-management/) for details on Azure API Management.</span></span> <span data-ttu-id="31a41-110">Kattintson a [Itt](../api-management/api-management-get-started.md) az Azure API Management az első lépések útmutató.</span><span class="sxs-lookup"><span data-stu-id="31a41-110">Click [here](../api-management/api-management-get-started.md) for a guide on how to get started with Azure API Management.</span></span> <span data-ttu-id="31a41-111">Ez más útmutató, ez az útmutató alapján, minden olyan további témaköreit, beleértve az értesítési konfigurációk, a árképzési szint, a válasz kezelése, a felhasználói hitelesítést, termékek, a fejlesztői előfizetésekhez és a használati dashboarding létrehozása.</span><span class="sxs-lookup"><span data-stu-id="31a41-111">This other guide, which this guide is based on, covers more topics, including notification configurations, tier pricing, response handling, user authentication, creating products, developer subscriptions, and usage dashboarding.</span></span>

## <a name="what-is-azureml"></a><span data-ttu-id="31a41-112">Mi az az AzureML?</span><span class="sxs-lookup"><span data-stu-id="31a41-112">What is AzureML?</span></span>
<span data-ttu-id="31a41-113">Az AzureML az Azure-szolgáltatások a gépi tanulási, amelynek segítségével könnyedén létre, telepíthetnek és speciális elemzési megoldások megosztása.</span><span class="sxs-lookup"><span data-stu-id="31a41-113">AzureML is an Azure service for machine learning that enables you to easily build, deploy, and share advanced analytics solutions.</span></span> <span data-ttu-id="31a41-114">Kattintson a [Itt](https://azure.microsoft.com/services/machine-learning/) AzureML leírását.</span><span class="sxs-lookup"><span data-stu-id="31a41-114">Click [here](https://azure.microsoft.com/services/machine-learning/) for details on AzureML.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="31a41-115">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="31a41-115">Prerequisites</span></span>
<span data-ttu-id="31a41-116">Ez az útmutató, szüksége:</span><span class="sxs-lookup"><span data-stu-id="31a41-116">To complete this guide, you need:</span></span>

* <span data-ttu-id="31a41-117">Egy Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="31a41-117">An Azure account.</span></span> <span data-ttu-id="31a41-118">Ha nem rendelkezik Azure-fiókot, kattintson a [Itt](https://azure.microsoft.com/pricing/free-trial/) talál részletes egy ingyenes próbafiók létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="31a41-118">If you don’t have an Azure account, click [here](https://azure.microsoft.com/pricing/free-trial/) for details on how to create a free trial account.</span></span>
* <span data-ttu-id="31a41-119">Az AzureML-fiók.</span><span class="sxs-lookup"><span data-stu-id="31a41-119">An AzureML account.</span></span> <span data-ttu-id="31a41-120">Ha nincs AzureML fiókja, kattintson a [Itt](https://studio.azureml.net/) talál részletes egy ingyenes próbafiók létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="31a41-120">If you don’t have an AzureML account, click [here](https://studio.azureml.net/) for details on how to create a free trial account.</span></span>
* <span data-ttu-id="31a41-121">A munkaterületen, a szolgáltatás és a api_key egy webes szolgáltatásként telepített AzureML kísérleti fázisú funkciókat.</span><span class="sxs-lookup"><span data-stu-id="31a41-121">The workspace, service, and api_key for an AzureML experiment deployed as a web service.</span></span> <span data-ttu-id="31a41-122">Kattintson a [Itt](machine-learning-create-experiment.md) talál részletes AzureML kísérlet létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="31a41-122">Click [here](machine-learning-create-experiment.md) for details on how to create an AzureML experiment.</span></span> <span data-ttu-id="31a41-123">Kattintson a [Itt](machine-learning-publish-a-machine-learning-web-service.md) vonatkozó részletes információt az AzureML telepítendő kísérletezhet webszolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="31a41-123">Click [here](machine-learning-publish-a-machine-learning-web-service.md) for details on how to deploy an AzureML experiment as a web service.</span></span> <span data-ttu-id="31a41-124">Alternatív megoldásként a melléklet rendelkezik létrehozása és tesztelése egy egyszerű AzureML kísérletet, és egy webszolgáltatás üzembe vonatkozó utasításokat.</span><span class="sxs-lookup"><span data-stu-id="31a41-124">Alternately, Appendix A has instructions for how to create and test a simple AzureML experiment and deploy it as a web service.</span></span>

## <a name="create-an-api-management-instance"></a><span data-ttu-id="31a41-125">API Management-példány létrehozása</span><span class="sxs-lookup"><span data-stu-id="31a41-125">Create an API Management instance</span></span>
<span data-ttu-id="31a41-126">Az alábbiakban megtalálja a lépéseket az API Management használata az AzureML webszolgáltatásba kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="31a41-126">Below are the steps for using API Management to manage your AzureML web service.</span></span> <span data-ttu-id="31a41-127">Először létre kell hoznia egy szolgáltatáspéldány.</span><span class="sxs-lookup"><span data-stu-id="31a41-127">First create a service instance.</span></span> <span data-ttu-id="31a41-128">Jelentkezzen be a [klasszikus portál](https://manage.windowsazure.com/) kattintson **új** > **alkalmazásszolgáltatások** > **API Management** > **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="31a41-128">Log in to the [Classic Portal](https://manage.windowsazure.com/) and click **New** > **App Services** > **API Management** > **Create**.</span></span>

![példány létrehozása](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-instance.png)

<span data-ttu-id="31a41-130">Adjon meg egy egyedi **URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="31a41-130">Specify a unique **URL**.</span></span> <span data-ttu-id="31a41-131">Ez az útmutató használ **demoazureml** – válasszon egy másik kell.</span><span class="sxs-lookup"><span data-stu-id="31a41-131">This guide uses **demoazureml** – you will need to choose something different.</span></span> <span data-ttu-id="31a41-132">Válassza ki a kívánt **Előfizetést** és **Régiót** a szolgáltatáspéldányához.</span><span class="sxs-lookup"><span data-stu-id="31a41-132">Choose the desired **Subscription** and **Region** for your service instance.</span></span> <span data-ttu-id="31a41-133">Miután kiválasztotta a kívánt beállításokat, kattintson a Tovább gombra.</span><span class="sxs-lookup"><span data-stu-id="31a41-133">After making your selections, click the next button.</span></span>

![Hozzon létre-szolgáltatás-1](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-1.png)

<span data-ttu-id="31a41-135">Adjon meg egy értéket a **szervezetnév**.</span><span class="sxs-lookup"><span data-stu-id="31a41-135">Specify a value for the **Organization Name**.</span></span> <span data-ttu-id="31a41-136">Ez az útmutató használ **demoazureml** – válasszon egy másik kell.</span><span class="sxs-lookup"><span data-stu-id="31a41-136">This guide uses **demoazureml** – you will need to choose something different.</span></span> <span data-ttu-id="31a41-137">Az e-mail címét adja meg a **rendszergazdai e-mail** mező.</span><span class="sxs-lookup"><span data-stu-id="31a41-137">Enter your email address in the **administrator e-mail** field.</span></span> <span data-ttu-id="31a41-138">Erre az e-mail-címre fognak érkezni az API Management rendszer értesítései.</span><span class="sxs-lookup"><span data-stu-id="31a41-138">This email address is used for notifications from the API Management system.</span></span>

![Hozzon létre-szolgáltatás-2](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-2.png)

<span data-ttu-id="31a41-140">A szolgáltatáspéldány létrehozásához jelölje be a jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="31a41-140">Click the check box to create your service instance.</span></span> <span data-ttu-id="31a41-141">*A létrehozandó új szolgáltatás akár 30 percet vesz igénybe*.</span><span class="sxs-lookup"><span data-stu-id="31a41-141">*It takes up to thirty minutes for a new service to be created*.</span></span>

## <a name="create-the-api"></a><span data-ttu-id="31a41-142">Az API-t létrehozni</span><span class="sxs-lookup"><span data-stu-id="31a41-142">Create the API</span></span>
<span data-ttu-id="31a41-143">A szolgáltatáspéldány létrehozása után a következő lépésre az API-t létrehozni.</span><span class="sxs-lookup"><span data-stu-id="31a41-143">Once the service instance is created, the next step is to create the API.</span></span> <span data-ttu-id="31a41-144">Az API egy ügyfélalkalmazásokból meghívható műveletkészletből áll.</span><span class="sxs-lookup"><span data-stu-id="31a41-144">An API consists of a set of operations that can be invoked from a client application.</span></span> <span data-ttu-id="31a41-145">Az API-műveleteket létező webszolgáltatásokhoz használják proxyként.</span><span class="sxs-lookup"><span data-stu-id="31a41-145">API operations are proxied to existing web services.</span></span> <span data-ttu-id="31a41-146">Ez az útmutató a meglévő AzureML RR-EKET és BES webszolgáltatásokhoz proxybeállítások API-kat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="31a41-146">This guide creates APIs that proxy to the existing AzureML RRS and BES web services.</span></span>

<span data-ttu-id="31a41-147">API-kat létrehozni és konfigurálni a portálról API publisher, amely a klasszikus Azure portálon keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="31a41-147">APIs are created and configured from the API publisher portal, which is accessed through the Azure Classic Portal.</span></span> <span data-ttu-id="31a41-148">A közzétevő portál eléréséhez a szolgáltatáspéldány kiválasztása</span><span class="sxs-lookup"><span data-stu-id="31a41-148">To reach the publisher portal, select your service instance.</span></span>

![szolgáltatáspéldány kiválasztása](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-service-instance.png)

<span data-ttu-id="31a41-150">Kattintson a **kezelése** a klasszikus Azure-portálon az API Management szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="31a41-150">Click **Manage** in the Azure Classic Portal for your API Management service.</span></span>

![szolgáltatás kezelése](./media/machine-learning-manage-web-service-endpoints-using-api-management/manage-service.png)

<span data-ttu-id="31a41-152">Kattintson a **API-k** a a **API Management** a bal oldali menüben, majd **hozzáadása API**.</span><span class="sxs-lookup"><span data-stu-id="31a41-152">Click **APIs** from the **API Management** menu on the left, and then click **Add API**.</span></span>

![API-felügyeleti-menü](./media/machine-learning-manage-web-service-endpoints-using-api-management/api-management-menu.png)

<span data-ttu-id="31a41-154">Típus **AzureML bemutató API** , a **webes API-név**.</span><span class="sxs-lookup"><span data-stu-id="31a41-154">Type **AzureML Demo API** as the **Web API name**.</span></span> <span data-ttu-id="31a41-155">Típus **https://ussouthcentral.services.azureml.net** , a **webszolgáltatás URL-címe**.</span><span class="sxs-lookup"><span data-stu-id="31a41-155">Type **https://ussouthcentral.services.azureml.net** as the **Web service URL**.</span></span> <span data-ttu-id="31a41-156">Típus **azureml-bemutató** , a **webes API URL-címe utótag**.</span><span class="sxs-lookup"><span data-stu-id="31a41-156">Type **azureml-demo** as the **Web API URL suffix**.</span></span> <span data-ttu-id="31a41-157">Ellenőrizze **HTTPS** , a **webes API URL-címe** séma.</span><span class="sxs-lookup"><span data-stu-id="31a41-157">Check **HTTPS** as the **Web API URL** scheme.</span></span> <span data-ttu-id="31a41-158">Válassza ki **alapszintű** , **termékek**.</span><span class="sxs-lookup"><span data-stu-id="31a41-158">Select **Starter** as **Products**.</span></span> <span data-ttu-id="31a41-159">Ha befejezte, kattintson a **mentése** az API-t létrehozni.</span><span class="sxs-lookup"><span data-stu-id="31a41-159">When finished, click **Save** to create the API.</span></span>

![Adja hozzá-új-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-new-api.png)

## <a name="add-the-operations"></a><span data-ttu-id="31a41-161">A műveletek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="31a41-161">Add the operations</span></span>
<span data-ttu-id="31a41-162">Kattintson a **hozzáadási művelet** műveletek hozzáadása az API.</span><span class="sxs-lookup"><span data-stu-id="31a41-162">Click **Add operation** to add operations to this API.</span></span>

![Adja hozzá művelet](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-operation.png)

<span data-ttu-id="31a41-164">A **új művelet** ablak megjelenik, és a **aláírás** alapértelmezett kiválasztása lapon.</span><span class="sxs-lookup"><span data-stu-id="31a41-164">The **New operation** window will be displayed and the **Signature** tab will be selected by default.</span></span>

## <a name="add-rrs-operation"></a><span data-ttu-id="31a41-165">RR-EKET művelet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="31a41-165">Add RRS Operation</span></span>
<span data-ttu-id="31a41-166">Először létre kell hoznia egy műveletet az AzureML RR-EKET szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="31a41-166">First create an operation for the AzureML RRS service.</span></span> <span data-ttu-id="31a41-167">Válassza ki **POST** , a **HTTP-műveletet**.</span><span class="sxs-lookup"><span data-stu-id="31a41-167">Select **POST** as the **HTTP verb**.</span></span> <span data-ttu-id="31a41-168">Típus **/workspaces/ {munkaterület} /services/ {szolgáltatás} / execute? api-version = {apiversion} & részletei = {részletek}** , a **URL-cím sablon**.</span><span class="sxs-lookup"><span data-stu-id="31a41-168">Type **/workspaces/{workspace}/services/{service}/execute?api-version={apiversion}&details={details}** as the **URL template**.</span></span> <span data-ttu-id="31a41-169">Típus **RRS hajtható végre** , a **megjelenített név**.</span><span class="sxs-lookup"><span data-stu-id="31a41-169">Type **RRS Execute** as the **Display name**.</span></span>

![Adja hozzá-rrs-művelet-aláírása](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-signature.png)

<span data-ttu-id="31a41-171">Kattintson a **válaszok** > **hozzáadása** a bal oldalon válassza ki **200 OK**.</span><span class="sxs-lookup"><span data-stu-id="31a41-171">Click **Responses** > **ADD** on the left and select **200 OK**.</span></span> <span data-ttu-id="31a41-172">Kattintson a **mentése** menti ezt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="31a41-172">Click **Save** to save this operation.</span></span>

![Adja hozzá-rrs-művelet-válasz](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-response.png)

## <a name="add-bes-operations"></a><span data-ttu-id="31a41-174">Adja hozzá a BES műveletek</span><span class="sxs-lookup"><span data-stu-id="31a41-174">Add BES Operations</span></span>
<span data-ttu-id="31a41-175">Képernyőfelvételek nem szerepelnek a BES műveletek szerint nagyon hasonló a hozzáadása a RR-EKET műveletet.</span><span class="sxs-lookup"><span data-stu-id="31a41-175">Screenshots are not included for the BES operations as they are very similar to those for adding the RRS operation.</span></span>

### <a name="submit-but-not-start-a-batch-execution-job"></a><span data-ttu-id="31a41-176">A kötegelt végrehajtási feladatok elküldése (de indul el)</span><span class="sxs-lookup"><span data-stu-id="31a41-176">Submit (but not start) a Batch Execution job</span></span>
<span data-ttu-id="31a41-177">Kattintson a **hozzáadási művelet** az AzureML BES művelet hozzáadása az API-t.</span><span class="sxs-lookup"><span data-stu-id="31a41-177">Click **add operation** to add the AzureML BES operation to the API.</span></span> <span data-ttu-id="31a41-178">Válassza ki **POST** a a **HTTP-műveletet**.</span><span class="sxs-lookup"><span data-stu-id="31a41-178">Select **POST** for the **HTTP verb**.</span></span> <span data-ttu-id="31a41-179">Típus **/workspaces/ {munkaterület} /services/ {szolgáltatás} / feladatok? api-version = {apiversion}** a a **URL-cím sablon**.</span><span class="sxs-lookup"><span data-stu-id="31a41-179">Type **/workspaces/{workspace}/services/{service}/jobs?api-version={apiversion}** for the **URL template**.</span></span> <span data-ttu-id="31a41-180">Típus **BES nyújt** a a **megjelenített név**.</span><span class="sxs-lookup"><span data-stu-id="31a41-180">Type **BES Submit** for the **Display name**.</span></span> <span data-ttu-id="31a41-181">Kattintson a **válaszok** > **hozzáadása** a bal oldalon válassza ki **200 OK**.</span><span class="sxs-lookup"><span data-stu-id="31a41-181">Click **Responses** > **ADD** on the left and select **200 OK**.</span></span> <span data-ttu-id="31a41-182">Kattintson a **mentése** menti ezt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="31a41-182">Click **Save** to save this operation.</span></span>

### <a name="start-a-batch-execution-job"></a><span data-ttu-id="31a41-183">Kötegelt végrehajtási feladat indítása</span><span class="sxs-lookup"><span data-stu-id="31a41-183">Start a Batch Execution job</span></span>
<span data-ttu-id="31a41-184">Kattintson a **hozzáadási művelet** az AzureML BES művelet hozzáadása az API-t.</span><span class="sxs-lookup"><span data-stu-id="31a41-184">Click **add operation** to add the AzureML BES operation to the API.</span></span> <span data-ttu-id="31a41-185">Válassza ki **POST** a a **HTTP-műveletet**.</span><span class="sxs-lookup"><span data-stu-id="31a41-185">Select **POST** for the **HTTP verb**.</span></span> <span data-ttu-id="31a41-186">Típus **/workspaces/ {munkaterület} {szolgáltatás} /services/ /jobs/ {jobid} / start? api-version = {apiversion}** a a **URL-cím sablon**.</span><span class="sxs-lookup"><span data-stu-id="31a41-186">Type **/workspaces/{workspace}/services/{service}/jobs/{jobid}/start?api-version={apiversion}** for the **URL template**.</span></span> <span data-ttu-id="31a41-187">Típus **BES Start** a a **megjelenített név**.</span><span class="sxs-lookup"><span data-stu-id="31a41-187">Type **BES Start** for the **Display name**.</span></span> <span data-ttu-id="31a41-188">Kattintson a **válaszok** > **hozzáadása** a bal oldalon válassza ki **200 OK**.</span><span class="sxs-lookup"><span data-stu-id="31a41-188">Click **Responses** > **ADD** on the left and select **200 OK**.</span></span> <span data-ttu-id="31a41-189">Kattintson a **mentése** menti ezt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="31a41-189">Click **Save** to save this operation.</span></span>

### <a name="get-the-status-or-result-of-a-batch-execution-job"></a><span data-ttu-id="31a41-190">Az állapot vagy a kötegelt végrehajtási feladat eredménye</span><span class="sxs-lookup"><span data-stu-id="31a41-190">Get the status or result of a Batch Execution job</span></span>
<span data-ttu-id="31a41-191">Kattintson a **hozzáadási művelet** az AzureML BES művelet hozzáadása az API-t.</span><span class="sxs-lookup"><span data-stu-id="31a41-191">Click **add operation** to add the AzureML BES operation to the API.</span></span> <span data-ttu-id="31a41-192">Válassza ki **beolvasása** a a **HTTP-műveletet**.</span><span class="sxs-lookup"><span data-stu-id="31a41-192">Select **GET** for the **HTTP verb**.</span></span> <span data-ttu-id="31a41-193">Típus **/workspaces/ {munkaterület} {szolgáltatás} /services/ /jobs/ {jobid}? api-version = {apiversion}** a a **URL-cím sablon**.</span><span class="sxs-lookup"><span data-stu-id="31a41-193">Type **/workspaces/{workspace}/services/{service}/jobs/{jobid}?api-version={apiversion}** for the **URL template**.</span></span> <span data-ttu-id="31a41-194">Típus **BES állapot** a a **megjelenített név**.</span><span class="sxs-lookup"><span data-stu-id="31a41-194">Type **BES Status** for the **Display name**.</span></span> <span data-ttu-id="31a41-195">Kattintson a **válaszok** > **hozzáadása** a bal oldalon válassza ki **200 OK**.</span><span class="sxs-lookup"><span data-stu-id="31a41-195">Click **Responses** > **ADD** on the left and select **200 OK**.</span></span> <span data-ttu-id="31a41-196">Kattintson a **mentése** menti ezt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="31a41-196">Click **Save** to save this operation.</span></span>

### <a name="delete-a-batch-execution-job"></a><span data-ttu-id="31a41-197">A kötegelt végrehajtási feladatok törlése</span><span class="sxs-lookup"><span data-stu-id="31a41-197">Delete a Batch Execution job</span></span>
<span data-ttu-id="31a41-198">Kattintson a **hozzáadási művelet** az AzureML BES művelet hozzáadása az API-t.</span><span class="sxs-lookup"><span data-stu-id="31a41-198">Click **add operation** to add the AzureML BES operation to the API.</span></span> <span data-ttu-id="31a41-199">Válassza ki **törlése** a a **HTTP-műveletet**.</span><span class="sxs-lookup"><span data-stu-id="31a41-199">Select **DELETE** for the **HTTP verb**.</span></span> <span data-ttu-id="31a41-200">Típus **/workspaces/ {munkaterület} {szolgáltatás} /services/ /jobs/ {jobid}? api-version = {apiversion}** a a **URL-cím sablon**.</span><span class="sxs-lookup"><span data-stu-id="31a41-200">Type **/workspaces/{workspace}/services/{service}/jobs/{jobid}?api-version={apiversion}** for the **URL template**.</span></span> <span data-ttu-id="31a41-201">Típus **BES törlése** a a **megjelenített név**.</span><span class="sxs-lookup"><span data-stu-id="31a41-201">Type **BES Delete** for the **Display name**.</span></span> <span data-ttu-id="31a41-202">Kattintson a **válaszok** > **hozzáadása** a bal oldalon válassza ki **200 OK**.</span><span class="sxs-lookup"><span data-stu-id="31a41-202">Click **Responses** > **ADD** on the left and select **200 OK**.</span></span> <span data-ttu-id="31a41-203">Kattintson a **mentése** menti ezt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="31a41-203">Click **Save** to save this operation.</span></span>

## <a name="call-an-operation-from-the-developer-portal"></a><span data-ttu-id="31a41-204">Művelet meghívása a Fejlesztői portálról</span><span class="sxs-lookup"><span data-stu-id="31a41-204">Call an operation from the Developer Portal</span></span>
<span data-ttu-id="31a41-205">Műveletek hívható közvetlenül a megtekintése, és az API-k működésének teszteléséhez kényelmes megoldást kínál Developer portálon.</span><span class="sxs-lookup"><span data-stu-id="31a41-205">Operations can be called directly from the Developer portal, which provides a convenient way to view and test the operations of an API.</span></span> <span data-ttu-id="31a41-206">Ez az útmutató lépésben fogja hívni a **RRS hajtható végre** módszerrel, amely hozzá lett adva a **AzureML bemutató API**.</span><span class="sxs-lookup"><span data-stu-id="31a41-206">In this guide step you will call the **RRS Execute** method that was added to the **AzureML Demo API**.</span></span> <span data-ttu-id="31a41-207">Kattintson a **fejlesztői portálján** a menü felső sarkában a klasszikus portálon.</span><span class="sxs-lookup"><span data-stu-id="31a41-207">Click **Developer portal** from the menu at the top right of the Classic Portal.</span></span>

![fejlesztői-portál](./media/machine-learning-manage-web-service-endpoints-using-api-management/developer-portal.png)

<span data-ttu-id="31a41-209">Kattintson a **API-k** a felső menüben, majd kattintson a **AzureML bemutató API** a rendelkezésre álló műveletek megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="31a41-209">Click **APIs** from the top menu, and then click **AzureML Demo API** to see the operations available.</span></span>

![demoazureml-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/demoazureml-api.png)

<span data-ttu-id="31a41-211">Válassza ki **RRS hajtható végre** a művelethez.</span><span class="sxs-lookup"><span data-stu-id="31a41-211">Select **RRS Execute** for the operation.</span></span> <span data-ttu-id="31a41-212">Kattintson a **kipróbálás**.</span><span class="sxs-lookup"><span data-stu-id="31a41-212">Click **Try It**.</span></span>

![Próbálja meg-it](./media/machine-learning-manage-web-service-endpoints-using-api-management/try-it.png)

<span data-ttu-id="31a41-214">A kérelemben szereplő paraméterek, írja be a **munkaterület**, **szolgáltatás**, **2.0** a a **apiversion**, és **igaz** a a **részletek**.</span><span class="sxs-lookup"><span data-stu-id="31a41-214">For Request parameters, type your **workspace**,  **service**, **2.0** for the **apiversion**, and  **true** for the **details**.</span></span> <span data-ttu-id="31a41-215">Megtalálhatja a **munkaterület** és **szolgáltatás** az AzureML webes szolgáltatás irányítópultján (lásd: **tesztelése a webszolgáltatás** függelék).</span><span class="sxs-lookup"><span data-stu-id="31a41-215">You can find your **workspace** and **service** in the AzureML web service dashboard (see **Test the web service** in Appendix A).</span></span>

<span data-ttu-id="31a41-216">Kérelemfejléc, kattintson **Hozzáadás fejléc** és írja be **Content-Type** és **az application/json**, majd kattintson a **Hozzáadás fejléc** és írja be **engedélyezési** és **tulajdonosi <YOUR AZUREML SERVICE API-KEY>** .</span><span class="sxs-lookup"><span data-stu-id="31a41-216">For Request headers, click **Add header** and type **Content-Type** and **application/json**, then click **Add header** and type **Authorization** and **Bearer <YOUR AZUREML SERVICE API-KEY>**.</span></span> <span data-ttu-id="31a41-217">Megtalálhatja a **api-kulcs** az AzureML webes szolgáltatás irányítópultján (lásd: **tesztelése a webszolgáltatás** függelék).</span><span class="sxs-lookup"><span data-stu-id="31a41-217">You can find your **api key** in the AzureML web service dashboard (see **Test the web service** in Appendix A).</span></span>

<span data-ttu-id="31a41-218">Típus **{"Bemenetek": {"input1": {"ColumnNames": "Értékek" ["Col2"]: [["Ez az a jó naponta"]]}}, "GlobalParameters": {}}** a kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="31a41-218">Type **{"Inputs": {"input1": {"ColumnNames": ["Col2"], "Values": [["This is a good day"]]}}, "GlobalParameters": {}}** for the request body.</span></span>

![az azureml-bemutató-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/azureml-demo-api.png)

<span data-ttu-id="31a41-220">Kattintson a **küldése**.</span><span class="sxs-lookup"><span data-stu-id="31a41-220">Click **Send**.</span></span>

![Küldés](./media/machine-learning-manage-web-service-endpoints-using-api-management/send.png)

<span data-ttu-id="31a41-222">Után egy művelet indítása, a fejlesztői portál megjeleníti-e a **a kért URL-cím** a háttér-szolgáltatás a **válaszállapot**, a **válaszfejlécek**, és az egyik **válasz tartalom**.</span><span class="sxs-lookup"><span data-stu-id="31a41-222">After an operation is invoked, the developer portal displays the **Requested URL** from the back-end service, the **Response status**, the **Response headers**, and any **Response content**.</span></span>

![válasz-állapot](./media/machine-learning-manage-web-service-endpoints-using-api-management/response-status.png)

## <a name="appendix-a---creating-and-testing-a-simple-azureml-web-service"></a><span data-ttu-id="31a41-224">A függelék – létrehozása és tesztelése egy egyszerű AzureML webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="31a41-224">Appendix A - Creating and testing a simple AzureML web service</span></span>
### <a name="creating-the-experiment"></a><span data-ttu-id="31a41-225">A kísérlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="31a41-225">Creating the experiment</span></span>
<span data-ttu-id="31a41-226">Az alábbiakban az AzureML egyszerű kísérlet létrehozását és telepítését webszolgáltatásként lépéseit.</span><span class="sxs-lookup"><span data-stu-id="31a41-226">Below are the steps for creating a simple AzureML experiment and deploying it as a web service.</span></span> <span data-ttu-id="31a41-227">A webes szolgáltatás vesz igénybe, adja meg a tetszőleges szöveget, és adja vissza egész számok-ként funkciókat.</span><span class="sxs-lookup"><span data-stu-id="31a41-227">The web service takes as input a column of arbitrary text and returns a set of features represented as integers.</span></span> <span data-ttu-id="31a41-228">Példa:</span><span class="sxs-lookup"><span data-stu-id="31a41-228">For example:</span></span>

| <span data-ttu-id="31a41-229">Szöveg</span><span class="sxs-lookup"><span data-stu-id="31a41-229">Text</span></span> | <span data-ttu-id="31a41-230">Kivonatolt szöveg</span><span class="sxs-lookup"><span data-stu-id="31a41-230">Hashed Text</span></span> |
| --- | --- |
| <span data-ttu-id="31a41-231">Ez az jó naponta</span><span class="sxs-lookup"><span data-stu-id="31a41-231">This is a good day</span></span> |<span data-ttu-id="31a41-232">1 1 2 2 0 2 0 1</span><span class="sxs-lookup"><span data-stu-id="31a41-232">1 1 2 2 0 2 0 1</span></span> |

<span data-ttu-id="31a41-233">Először egy böngészőt, nyissa meg azt: [https://studio.azureml.net/](https://studio.azureml.net/) bejelentkezési hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="31a41-233">First, using a browser of your choice, navigate to: [https://studio.azureml.net/](https://studio.azureml.net/) and enter your credentials to log in.</span></span> <span data-ttu-id="31a41-234">Ezután hozzon létre egy új üres kísérlet.</span><span class="sxs-lookup"><span data-stu-id="31a41-234">Next, create a new blank experiment.</span></span>

![Keresés – kísérlet-sablonok](./media/machine-learning-manage-web-service-endpoints-using-api-management/search-experiment-templates.png)

<span data-ttu-id="31a41-236">Nevezze át **SimpleFeatureHashingExperiment**.</span><span class="sxs-lookup"><span data-stu-id="31a41-236">Rename it to **SimpleFeatureHashingExperiment**.</span></span> <span data-ttu-id="31a41-237">Bontsa ki a **mentett adatkészletek** , és húzza **könyv értékelést az Amazon** alakzatot a kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="31a41-237">Expand **Saved Datasets** and drag **Book Reviews from Amazon** onto your experiment.</span></span>

![egyszerű-szolgáltatás-kivonatolás-kísérlet](./media/machine-learning-manage-web-service-endpoints-using-api-management/simple-feature-hashing-experiment.png)

<span data-ttu-id="31a41-239">Bontsa ki a **Data Transformation** és **adatkezelési** , és húzza **Select Columns in Dataset** alakzatot a kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="31a41-239">Expand **Data Transformation** and **Manipulation** and drag **Select Columns in Dataset** onto your experiment.</span></span> <span data-ttu-id="31a41-240">Csatlakozás **értékelést le az Amazon** való **oszlopok kiválasztása az adathalmaz**.</span><span class="sxs-lookup"><span data-stu-id="31a41-240">Connect **Book Reviews from Amazon** to **Select Columns in Dataset**.</span></span>

![select-columns](./media/machine-learning-manage-web-service-endpoints-using-api-management/project-columns.png)

<span data-ttu-id="31a41-242">Kattintson a **Select Columns in Dataset** majd **Oszlopválasztás** válassza **Col2**.</span><span class="sxs-lookup"><span data-stu-id="31a41-242">Click **Select Columns in Dataset** and then click **Launch column selector** and select **Col2**.</span></span> <span data-ttu-id="31a41-243">Kattintson a pipa ikonra, hogy alkalmazza a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="31a41-243">Click the checkmark to apply these changes.</span></span>

![select-columns](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-columns.png)

<span data-ttu-id="31a41-245">Bontsa ki a **Szövegelemzések** , és húzza **Szolgáltatáskivonatolás** alakzatot a kísérletet.</span><span class="sxs-lookup"><span data-stu-id="31a41-245">Expand **Text Analytics** and drag **Feature Hashing** onto the experiment.</span></span> <span data-ttu-id="31a41-246">Csatlakozás **oszlopok kiválasztása az adathalmaz** való **Szolgáltatáskivonatolás**.</span><span class="sxs-lookup"><span data-stu-id="31a41-246">Connect **Select Columns in Dataset** to **Feature Hashing**.</span></span>

![Connect--projektoszlopok](./media/machine-learning-manage-web-service-endpoints-using-api-management/connect-project-columns.png)

<span data-ttu-id="31a41-248">Típus **3** a a **bitsize kivonatoláshoz**.</span><span class="sxs-lookup"><span data-stu-id="31a41-248">Type **3** for the **Hashing bitsize**.</span></span> <span data-ttu-id="31a41-249">Ezzel létrehoz 8 (23) oszlopot.</span><span class="sxs-lookup"><span data-stu-id="31a41-249">This will create 8 (23) columns.</span></span>

![kivonatoló bitsize](./media/machine-learning-manage-web-service-endpoints-using-api-management/hashing-bitsize.png)

<span data-ttu-id="31a41-251">Ezen a ponton érdemes lehet kattintson **futtatása** tesztelése a kísérletet.</span><span class="sxs-lookup"><span data-stu-id="31a41-251">At this point, you may want to click **Run** to test the experiment.</span></span>

![Futtatás](./media/machine-learning-manage-web-service-endpoints-using-api-management/run.png)

### <a name="create-a-web-service"></a><span data-ttu-id="31a41-253">Webszolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="31a41-253">Create a web service</span></span>
<span data-ttu-id="31a41-254">Most hozzon létre egy webszolgáltatás-bővítmény.</span><span class="sxs-lookup"><span data-stu-id="31a41-254">Now create a web service.</span></span> <span data-ttu-id="31a41-255">Bontsa ki a **webszolgáltatás** , és húzza **bemeneti** alakzatot a kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="31a41-255">Expand **Web Service** and drag **Input** onto your experiment.</span></span> <span data-ttu-id="31a41-256">Csatlakozás **bemeneti** való **Szolgáltatáskivonatolás**.</span><span class="sxs-lookup"><span data-stu-id="31a41-256">Connect **Input** to **Feature Hashing**.</span></span> <span data-ttu-id="31a41-257">Húzással is **kimeneti** alakzatot a kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="31a41-257">Also drag **output** onto your experiment.</span></span> <span data-ttu-id="31a41-258">Csatlakozás **kimeneti** való **Szolgáltatáskivonatolás**.</span><span class="sxs-lookup"><span data-stu-id="31a41-258">Connect **Output** to **Feature Hashing**.</span></span>

![kimeneti-az-szolgáltatás-kivonatolás](./media/machine-learning-manage-web-service-endpoints-using-api-management/output-to-feature-hashing.png)

<span data-ttu-id="31a41-260">Kattintson a **webszolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="31a41-260">Click **Publish web service**.</span></span>

![közzététel-webszolgáltatás](./media/machine-learning-manage-web-service-endpoints-using-api-management/publish-web-service.png)

<span data-ttu-id="31a41-262">Kattintson a **Igen** közzététele a kísérletet.</span><span class="sxs-lookup"><span data-stu-id="31a41-262">Click **Yes** to publish the experiment.</span></span>

![Igen – közzététele](./media/machine-learning-manage-web-service-endpoints-using-api-management/yes-to-publish.png)

### <a name="test-the-web-service"></a><span data-ttu-id="31a41-264">A webszolgáltatás tesztelése</span><span class="sxs-lookup"><span data-stu-id="31a41-264">Test the web service</span></span>
<span data-ttu-id="31a41-265">Az AzureML webszolgáltatás RSS (kérelem/válasz szolgáltatás) és a BES (kötegelt végrehajtási szolgáltatás) végpontok áll.</span><span class="sxs-lookup"><span data-stu-id="31a41-265">An AzureML web service consists of RSS (request/response service) and BES (batch execution service) endpoints.</span></span> <span data-ttu-id="31a41-266">RSS szinkron végrehajtásra van.</span><span class="sxs-lookup"><span data-stu-id="31a41-266">RSS is for synchronous execution.</span></span> <span data-ttu-id="31a41-267">BES aszinkron feladat végrehajtása szolgál.</span><span class="sxs-lookup"><span data-stu-id="31a41-267">BES is for asynchronous job execution.</span></span> <span data-ttu-id="31a41-268">A webes szolgáltatás, az alábbi minta Python-forrással rendelkező tesztelése, esetleg töltse le és telepítse az Azure SDK Python (lásd: [Python telepítése](../python-how-to-install.md)).</span><span class="sxs-lookup"><span data-stu-id="31a41-268">To test your web service with the sample Python source below, you may need to download and install the Azure SDK for Python (see: [How to install Python](../python-how-to-install.md)).</span></span>

<span data-ttu-id="31a41-269">Konfigurálnia kell a **munkaterület**, **szolgáltatás**, és **api_key** a kísérlet az alábbi minta adatforrásra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="31a41-269">You will also need the **workspace**, **service**, and **api_key** of your experiment for the sample source below.</span></span> <span data-ttu-id="31a41-270">Megtalálhatja az munkaterület és a szolgáltatás lehet **kérelem/válasz** vagy **kötegelt végrehajtási** webes szolgáltatás irányítópultján a kísérleti fázisú funkciókat.</span><span class="sxs-lookup"><span data-stu-id="31a41-270">You can find the workspace and service by clicking either **Request/Response** or **Batch Execution** for your experiment in the web service dashboard.</span></span>

![Keresés – munkaterület-és-szolgáltatás](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-workspace-and-service.png)

<span data-ttu-id="31a41-272">Megtalálhatja az **api_key** webes szolgáltatás irányítópultján kísérletbe kattintva.</span><span class="sxs-lookup"><span data-stu-id="31a41-272">You can find the **api_key** by clicking your experiment in the web service dashboard.</span></span>

![Keresés – api-kulcs](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-api-key.png)

#### <a name="test-rrs-endpoint"></a><span data-ttu-id="31a41-274">Teszt RR-EKET végpont</span><span class="sxs-lookup"><span data-stu-id="31a41-274">Test RRS endpoint</span></span>
##### <a name="test-button"></a><span data-ttu-id="31a41-275">Teszt gomb</span><span class="sxs-lookup"><span data-stu-id="31a41-275">Test button</span></span>
<span data-ttu-id="31a41-276">Az RRS végpont teszteléséhez egyszerűen: kattintson **tesztelése** a webes szolgáltatás irányítópultján.</span><span class="sxs-lookup"><span data-stu-id="31a41-276">An easy way to test the RRS endpoint is to click **Test** on the web service dashboard.</span></span>

![Teszt](./media/machine-learning-manage-web-service-endpoints-using-api-management/test.png)

<span data-ttu-id="31a41-278">Típus **Ez az jó naponta** a **col2**.</span><span class="sxs-lookup"><span data-stu-id="31a41-278">Type **This is a good day** for **col2**.</span></span> <span data-ttu-id="31a41-279">Kattintson a pipa jelre.</span><span class="sxs-lookup"><span data-stu-id="31a41-279">Click the checkmark.</span></span>

![Adja meg adatok](./media/machine-learning-manage-web-service-endpoints-using-api-management/enter-data.png)

<span data-ttu-id="31a41-281">Látni fogja hasonlót</span><span class="sxs-lookup"><span data-stu-id="31a41-281">You will see something like</span></span>

![minta-kimenet](./media/machine-learning-manage-web-service-endpoints-using-api-management/sample-output.png)

##### <a name="sample-code"></a><span data-ttu-id="31a41-283">Mintakód</span><span class="sxs-lookup"><span data-stu-id="31a41-283">Sample Code</span></span>
<span data-ttu-id="31a41-284">Egy másik módszer a RR-EKET az Ügyfélkód származik.</span><span class="sxs-lookup"><span data-stu-id="31a41-284">Another way to test your RRS is from your client code.</span></span> <span data-ttu-id="31a41-285">Ha **kérelem/válasz** az irányítópultot és a felülről lefelé görgetési jelennek meg példakód C#, Python, és R. Ezenkívül megjelenik az RRS kérelem a kérelem URI-azonosítója, beleértve a szintaxist fejlécek és törzse.</span><span class="sxs-lookup"><span data-stu-id="31a41-285">If you click **Request/response** on the dashboard and scroll to the bottom, you will see sample code for C#, Python, and R. You will also see the syntax of the RRS request, including the request URI, headers, and body.</span></span>

<span data-ttu-id="31a41-286">Ez az útmutató bemutatja egy működő Python példa.</span><span class="sxs-lookup"><span data-stu-id="31a41-286">This guide shows a working Python example.</span></span> <span data-ttu-id="31a41-287">Módosítani kell a **munkaterület**, **szolgáltatás**, és **api_key** a kísérlet.</span><span class="sxs-lookup"><span data-stu-id="31a41-287">You will need to modify it with the **workspace**, **service**, and **api_key** of your experiment.</span></span>

    import urllib2
    import json
    workspace = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE WORKSPACE ID>"
    service = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE SERVICE ID>"
    api_key = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE API KEY>"
    data = {
    "Inputs": {
        "input1": {
            "ColumnNames": ["Col2"],
            "Values": [ [ "This is a good day" ] ]
        },
    },
    "GlobalParameters": { }
    }
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace + "/services/" + service + "/execute?api-version=2.0&details=true"
    headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}
    body = str.encode(json.dumps(data))
    try:
        req = urllib2.Request(url, body, headers)
        response = urllib2.urlopen(req)
        result = response.read()
        print "result:" + result
            except urllib2.HTTPError, error:
        print("The request failed with status code: " + str(error.code))
        print(error.info())
        print(json.loads(error.read()))

#### <a name="test-bes-endpoint"></a><span data-ttu-id="31a41-288">Teszt BES végpont</span><span class="sxs-lookup"><span data-stu-id="31a41-288">Test BES endpoint</span></span>
<span data-ttu-id="31a41-289">Kattintson a **kötegelt végrehajtás** az irányítópult és alján görgessen.</span><span class="sxs-lookup"><span data-stu-id="31a41-289">Click **Batch execution** on the dashboard and scroll to the bottom.</span></span> <span data-ttu-id="31a41-290">Mintakód látni fogja a C#, Python, és R. A feladat elküldése, elindíthat egy feladatot, az állapot vagy a feladat eredményeinek lekérése és törli a feladatot BES kérelmeket szintaxist is megtekintheti.</span><span class="sxs-lookup"><span data-stu-id="31a41-290">You will see sample code for C#, Python, and R. You will also see the syntax of the BES requests to submit a job, start a job, get the status or results of a job, and delete a job.</span></span>

<span data-ttu-id="31a41-291">Ez az útmutató bemutatja egy működő Python példa.</span><span class="sxs-lookup"><span data-stu-id="31a41-291">This guide shows a working Python example.</span></span> <span data-ttu-id="31a41-292">Módosítani kell a **munkaterület**, **szolgáltatás**, és **api_key** a kísérlet.</span><span class="sxs-lookup"><span data-stu-id="31a41-292">You need to modify it with the **workspace**, **service**, and **api_key** of your experiment.</span></span> <span data-ttu-id="31a41-293">Továbbá, hogy módosítania kell a **tárfióknév**, **tárfiók kulcsa**, és **storage-tároló nevének**.</span><span class="sxs-lookup"><span data-stu-id="31a41-293">Additionally, you need to modify the **storage account name**, **storage account key**, and **storage container name**.</span></span> <span data-ttu-id="31a41-294">Végül, szüksége lesz a helyét, módosítsa a **bemeneti fájl** és helye a **kimeneti fájl**.</span><span class="sxs-lookup"><span data-stu-id="31a41-294">Lastly, you will need to modify the location of the **input file** and the location of the **output file**.</span></span>

    import urllib2
    import json
    import time
    from azure.storage import *
    workspace = "<REPLACE WITH YOUR WORKSPACE ID>"
    service = "<REPLACE WITH YOUR SERVICE ID>"
    api_key = "<REPLACE WITH THE API KEY FOR YOUR WEB SERVICE>"
    storage_account_name = "<REPLACE WITH YOUR AZURE STORAGE ACCOUNT NAME>"
    storage_account_key = "<REPLACE WITH YOUR AZURE STORAGE KEY>"
    storage_container_name = "<REPLACE WITH YOUR AZURE STORAGE CONTAINER NAME>"
    input_file = "<REPLACE WITH THE LOCATION OF YOUR INPUT FILE>" # Example: C:\\mydata.csv
    output_file = "<REPLACE WITH THE LOCATION OF YOUR OUTPUT FILE>" # Example: C:\\myresults.csv
    input_blob_name = "mydatablob.csv"
    output_blob_name = "myresultsblob.csv"
    def printHttpError(httpError):
    print("The request failed with status code: " + str(httpError.code))
    print(httpError.info())
    print(json.loads(httpError.read()))
    return
    def saveBlobToFile(blobUrl, resultsLabel):
    print("Reading the result from " + blobUrl)
    try:
        response = urllib2.urlopen(blobUrl)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    with open(output_file, "w+") as f:
        f.write(response.read())
    print(resultsLabel + " have been written to the file " + output_file)
    return
    def processResults(result):
    first = True
    results = result["Results"]
    for outputName in results:
        result_blob_location = results[outputName]
        sas_token = result_blob_location["SasBlobToken"]
        base_url = result_blob_location["BaseLocation"]
        relative_url = result_blob_location["RelativeLocation"]
        print("The results for " + outputName + " are available at the following Azure Storage location:")
        print("BaseLocation: " + base_url)
        print("RelativeLocation: " + relative_url)
        print("SasBlobToken: " + sas_token)
        if (first):
            first = False
            url3 = base_url + relative_url + sas_token
            saveBlobToFile(url3, "The results for " + outputName)
    return

    def invokeBatchExecutionService():
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace +"/services/" + service +"/jobs"
    blob_service = BlobService(account_name=storage_account_name, account_key=storage_account_key)
    print("Uploading the input to blob storage...")
    data_to_upload = open(input_file, "r").read()
    blob_service.put_blob(storage_container_name, input_blob_name, data_to_upload, x_ms_blob_type="BlockBlob")
    print "Uploaded the input to blob storage"
    input_blob_path = "/" + storage_container_name + "/" + input_blob_name
    connection_string = "DefaultEndpointsProtocol=https;AccountName=" + storage_account_name + ";AccountKey=" + storage_account_key
    payload =  {
        "Input": {
            "ConnectionString": connection_string,
            "RelativeLocation": input_blob_path
        },
        "Outputs": {
            "output1": { "ConnectionString": connection_string, "RelativeLocation": "/" + storage_container_name + "/" + output_blob_name },
        },
        "GlobalParameters": {
        }
    }
        body = str.encode(json.dumps(payload))
    headers = { "Content-Type":"application/json", "Authorization":("Bearer " + api_key)}
    print("Submitting the job...")
    # submit the job
    req = urllib2.Request(url + "?api-version=2.0", body, headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    result = response.read()
    job_id = result[1:-1] # remove the enclosing double-quotes
    print("Job ID: " + job_id)
    # start the job
    print("Starting the job...")
    req = urllib2.Request(url + "/" + job_id + "/start?api-version=2.0", "", headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    url2 = url + "/" + job_id + "?api-version=2.0"

    while True:
        print("Checking the job status...")
        # If you are using Python 3+, replace urllib2 with urllib.request in the follwing code
        req = urllib2.Request(url2, headers = { "Authorization":("Bearer " + api_key) })
        try:
            response = urllib2.urlopen(req)
        except urllib2.HTTPError, error:
            printHttpError(error)
            return
        result = json.loads(response.read())
        status = result["StatusCode"]
        print "status:" + status
        if (status == 0 or status == "NotStarted"):
            print("Job " + job_id + " not yet started...")
        elif (status == 1 or status == "Running"):
            print("Job " + job_id + " running...")
        elif (status == 2 or status == "Failed"):
            print("Job " + job_id + " failed!")
            print("Error details: " + result["Details"])
            break
        elif (status == 3 or status == "Cancelled"):
            print("Job " + job_id + " cancelled!")
            break
        elif (status == 4 or status == "Finished"):
            print("Job " + job_id + " finished!")
            processResults(result)
            break
        time.sleep(1) # wait one second
    return
    invokeBatchExecutionService()
