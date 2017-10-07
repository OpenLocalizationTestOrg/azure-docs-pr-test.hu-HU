---
title: "Hogyan toomanage AzureML web services API Management használata aaaLearn |} Microsoft Docs"
description: "Egy útmutató, megjelenítő, hogy miként toomanage AzureML webes szolgáltatásokat, az API Management használata."
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
ms.openlocfilehash: 6e764fbfd71be6cc908a1c8d3d8889969fc651a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="learn-how-toomanage-azureml-web-services-using-api-management"></a><span data-ttu-id="385d7-104">Ismerje meg, hogy miként toomanage AzureML webes szolgáltatásokat, az API Management használata</span><span class="sxs-lookup"><span data-stu-id="385d7-104">Learn how toomanage AzureML web services using API Management</span></span>
## <a name="overview"></a><span data-ttu-id="385d7-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="385d7-105">Overview</span></span>
<span data-ttu-id="385d7-106">Ez az útmutató bemutatja, hogyan tooquickly az API Management toomanage az AzureML webszolgáltatások első lépéseiben.</span><span class="sxs-lookup"><span data-stu-id="385d7-106">This guide shows you how tooquickly get started using API Management toomanage your AzureML web services.</span></span>

## <a name="what-is-azure-api-management"></a><span data-ttu-id="385d7-107">Mi az Azure API Management?</span><span class="sxs-lookup"><span data-stu-id="385d7-107">What is Azure API Management?</span></span>
<span data-ttu-id="385d7-108">Az Azure API-kezelés nem az Azure-szolgáltatások, amely lehetővé teszi a REST API-végpontokat kezelheti a felhasználói hozzáférés-szabályozás és figyelési irányítópult meghatározásával.</span><span class="sxs-lookup"><span data-stu-id="385d7-108">Azure API Management is an Azure service that lets you manage your REST API endpoints by defining user access, usage throttling, and dashboard monitoring.</span></span> <span data-ttu-id="385d7-109">Kattintson a [Itt](https://azure.microsoft.com/services/api-management/) Azure API Management leírását.</span><span class="sxs-lookup"><span data-stu-id="385d7-109">Click [here](https://azure.microsoft.com/services/api-management/) for details on Azure API Management.</span></span> <span data-ttu-id="385d7-110">Kattintson a [Itt](../api-management/api-management-get-started.md) az útmutató, hogyan tooget el az Azure API Management szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="385d7-110">Click [here](../api-management/api-management-get-started.md) for a guide on how tooget started with Azure API Management.</span></span> <span data-ttu-id="385d7-111">Ez más útmutató, ez az útmutató alapján, minden olyan további témaköreit, beleértve az értesítési konfigurációk, a árképzési szint, a válasz kezelése, a felhasználói hitelesítést, termékek, a fejlesztői előfizetésekhez és a használati dashboarding létrehozása.</span><span class="sxs-lookup"><span data-stu-id="385d7-111">This other guide, which this guide is based on, covers more topics, including notification configurations, tier pricing, response handling, user authentication, creating products, developer subscriptions, and usage dashboarding.</span></span>

## <a name="what-is-azureml"></a><span data-ttu-id="385d7-112">Mi az az AzureML?</span><span class="sxs-lookup"><span data-stu-id="385d7-112">What is AzureML?</span></span>
<span data-ttu-id="385d7-113">Az AzureML az Azure-szolgáltatások a machine Learning szolgáltatáshoz, amely lehetővé teszi tooeasily build, telepítheti, és speciális elemzési megoldások megosztása.</span><span class="sxs-lookup"><span data-stu-id="385d7-113">AzureML is an Azure service for machine learning that enables you tooeasily build, deploy, and share advanced analytics solutions.</span></span> <span data-ttu-id="385d7-114">Kattintson a [Itt](https://azure.microsoft.com/services/machine-learning/) AzureML leírását.</span><span class="sxs-lookup"><span data-stu-id="385d7-114">Click [here](https://azure.microsoft.com/services/machine-learning/) for details on AzureML.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="385d7-115">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="385d7-115">Prerequisites</span></span>
<span data-ttu-id="385d7-116">toocomplete ebben az útmutatóban szüksége:</span><span class="sxs-lookup"><span data-stu-id="385d7-116">toocomplete this guide, you need:</span></span>

* <span data-ttu-id="385d7-117">Egy Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="385d7-117">An Azure account.</span></span> <span data-ttu-id="385d7-118">Ha nem rendelkezik Azure-fiókot, kattintson a [Itt](https://azure.microsoft.com/pricing/free-trial/) kapcsolatos részletes tudnivalókért toocreate egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="385d7-118">If you don’t have an Azure account, click [here](https://azure.microsoft.com/pricing/free-trial/) for details on how toocreate a free trial account.</span></span>
* <span data-ttu-id="385d7-119">Az AzureML-fiók.</span><span class="sxs-lookup"><span data-stu-id="385d7-119">An AzureML account.</span></span> <span data-ttu-id="385d7-120">Ha nincs AzureML fiókja, kattintson a [Itt](https://studio.azureml.net/) kapcsolatos részletes tudnivalókért toocreate egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="385d7-120">If you don’t have an AzureML account, click [here](https://studio.azureml.net/) for details on how toocreate a free trial account.</span></span>
* <span data-ttu-id="385d7-121">hello munkaterületen, a szolgáltatás és a api_key egy webes szolgáltatásként telepített AzureML kísérleti fázisú funkciókat.</span><span class="sxs-lookup"><span data-stu-id="385d7-121">hello workspace, service, and api_key for an AzureML experiment deployed as a web service.</span></span> <span data-ttu-id="385d7-122">Kattintson a [Itt](machine-learning-create-experiment.md) talál részletes információt hogyan toocreate az AzureML kipróbálásához.</span><span class="sxs-lookup"><span data-stu-id="385d7-122">Click [here](machine-learning-create-experiment.md) for details on how toocreate an AzureML experiment.</span></span> <span data-ttu-id="385d7-123">Kattintson a [Itt](machine-learning-publish-a-machine-learning-web-service.md) talál részletes információt hogyan toodeploy az AzureML webszolgáltatásként kipróbálásához.</span><span class="sxs-lookup"><span data-stu-id="385d7-123">Click [here](machine-learning-publish-a-machine-learning-web-service.md) for details on how toodeploy an AzureML experiment as a web service.</span></span> <span data-ttu-id="385d7-124">Alternatív megoldásként a függelék rendelkezik hogyan toocreate és tesztelése egy egyszerű AzureML kísérletezhet, és telepítse azt egy webszolgáltatás vonatkozó utasításokat.</span><span class="sxs-lookup"><span data-stu-id="385d7-124">Alternately, Appendix A has instructions for how toocreate and test a simple AzureML experiment and deploy it as a web service.</span></span>

## <a name="create-an-api-management-instance"></a><span data-ttu-id="385d7-125">API Management-példány létrehozása</span><span class="sxs-lookup"><span data-stu-id="385d7-125">Create an API Management instance</span></span>
<span data-ttu-id="385d7-126">Az alábbiakban hello API Management toomanage használatának lépései a AzureML webes szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="385d7-126">Below are hello steps for using API Management toomanage your AzureML web service.</span></span> <span data-ttu-id="385d7-127">Először létre kell hoznia egy szolgáltatáspéldány.</span><span class="sxs-lookup"><span data-stu-id="385d7-127">First create a service instance.</span></span> <span data-ttu-id="385d7-128">Jelentkezzen be toohello [klasszikus portál](https://manage.windowsazure.com/) kattintson **új** > **alkalmazásszolgáltatások** > **API Management**  >  **Létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="385d7-128">Log in toohello [Classic Portal](https://manage.windowsazure.com/) and click **New** > **App Services** > **API Management** > **Create**.</span></span>

![példány létrehozása](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-instance.png)

<span data-ttu-id="385d7-130">Adjon meg egy egyedi **URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="385d7-130">Specify a unique **URL**.</span></span> <span data-ttu-id="385d7-131">Ez az útmutató használ **demoazureml** – valami más kell toochoose.</span><span class="sxs-lookup"><span data-stu-id="385d7-131">This guide uses **demoazureml** – you will need toochoose something different.</span></span> <span data-ttu-id="385d7-132">Válassza ki a kívánt hello **előfizetés** és **régió** a szolgáltatáspéldány.</span><span class="sxs-lookup"><span data-stu-id="385d7-132">Choose hello desired **Subscription** and **Region** for your service instance.</span></span> <span data-ttu-id="385d7-133">Miután kiválasztotta a kívánt beállításokat, kattintson a hello Tovább gombra.</span><span class="sxs-lookup"><span data-stu-id="385d7-133">After making your selections, click hello next button.</span></span>

![Hozzon létre-szolgáltatás-1](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-1.png)

<span data-ttu-id="385d7-135">Adjon meg egy értéket hello **szervezetnév**.</span><span class="sxs-lookup"><span data-stu-id="385d7-135">Specify a value for hello **Organization Name**.</span></span> <span data-ttu-id="385d7-136">Ez az útmutató használ **demoazureml** – valami más kell toochoose.</span><span class="sxs-lookup"><span data-stu-id="385d7-136">This guide uses **demoazureml** – you will need toochoose something different.</span></span> <span data-ttu-id="385d7-137">Adja meg az e-mail cím hello **rendszergazdai e-mail** mező.</span><span class="sxs-lookup"><span data-stu-id="385d7-137">Enter your email address in hello **administrator e-mail** field.</span></span> <span data-ttu-id="385d7-138">Ez az e-mail cím az API Management rendszer hello értesítésekhez használatos.</span><span class="sxs-lookup"><span data-stu-id="385d7-138">This email address is used for notifications from hello API Management system.</span></span>

![Hozzon létre-szolgáltatás-2](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-2.png)

<span data-ttu-id="385d7-140">Kattintson a szolgáltatáspéldány a hello jelölőnégyzetet toocreate.</span><span class="sxs-lookup"><span data-stu-id="385d7-140">Click hello check box toocreate your service instance.</span></span> <span data-ttu-id="385d7-141">*Egy új szolgáltatás toobe létrehozott toothirty percig foglal*.</span><span class="sxs-lookup"><span data-stu-id="385d7-141">*It takes up toothirty minutes for a new service toobe created*.</span></span>

## <a name="create-hello-api"></a><span data-ttu-id="385d7-142">Hello API létrehozása</span><span class="sxs-lookup"><span data-stu-id="385d7-142">Create hello API</span></span>
<span data-ttu-id="385d7-143">Hello szolgáltatáspéldány létrehozása után a következő lépésben hello toocreate hello API.</span><span class="sxs-lookup"><span data-stu-id="385d7-143">Once hello service instance is created, hello next step is toocreate hello API.</span></span> <span data-ttu-id="385d7-144">Az API egy ügyfélalkalmazásokból meghívható műveletkészletből áll.</span><span class="sxs-lookup"><span data-stu-id="385d7-144">An API consists of a set of operations that can be invoked from a client application.</span></span> <span data-ttu-id="385d7-145">API-műveleteket a proxy tooexisting webes szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="385d7-145">API operations are proxied tooexisting web services.</span></span> <span data-ttu-id="385d7-146">Ez az útmutató a meglévő AzureML RR-EKET és BES webszolgáltatások proxy toohello API-k hoz létre.</span><span class="sxs-lookup"><span data-stu-id="385d7-146">This guide creates APIs that proxy toohello existing AzureML RRS and BES web services.</span></span>

<span data-ttu-id="385d7-147">API-k létrehozása és konfigurált portálról hello API publisher, amely hello klasszikus Azure portálon keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="385d7-147">APIs are created and configured from hello API publisher portal, which is accessed through hello Azure Classic Portal.</span></span> <span data-ttu-id="385d7-148">tooreach hello publisher portál, válassza ki a szolgáltatáspéldányt.</span><span class="sxs-lookup"><span data-stu-id="385d7-148">tooreach hello publisher portal, select your service instance.</span></span>

![szolgáltatáspéldány kiválasztása](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-service-instance.png)

<span data-ttu-id="385d7-150">Kattintson a **kezelése** a klasszikus Azure portál hello az API Management szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="385d7-150">Click **Manage** in hello Azure Classic Portal for your API Management service.</span></span>

![szolgáltatás kezelése](./media/machine-learning-manage-web-service-endpoints-using-api-management/manage-service.png)

<span data-ttu-id="385d7-152">Kattintson a **API-k** a hello **API Management** hello maradt, és válassza a menü **hozzáadása API**.</span><span class="sxs-lookup"><span data-stu-id="385d7-152">Click **APIs** from hello **API Management** menu on hello left, and then click **Add API**.</span></span>

![API-felügyeleti-menü](./media/machine-learning-manage-web-service-endpoints-using-api-management/api-management-menu.png)

<span data-ttu-id="385d7-154">Típus **AzureML bemutató API** , hello **webes API-név**.</span><span class="sxs-lookup"><span data-stu-id="385d7-154">Type **AzureML Demo API** as hello **Web API name**.</span></span> <span data-ttu-id="385d7-155">Típus **https://ussouthcentral.services.azureml.net** , hello **webszolgáltatás URL-címe**.</span><span class="sxs-lookup"><span data-stu-id="385d7-155">Type **https://ussouthcentral.services.azureml.net** as hello **Web service URL**.</span></span> <span data-ttu-id="385d7-156">Típus **azureml-bemutató** , hello **webes API URL-címe utótag**.</span><span class="sxs-lookup"><span data-stu-id="385d7-156">Type **azureml-demo** as hello **Web API URL suffix**.</span></span> <span data-ttu-id="385d7-157">Ellenőrizze **HTTPS** , hello **webes API URL-címe** séma.</span><span class="sxs-lookup"><span data-stu-id="385d7-157">Check **HTTPS** as hello **Web API URL** scheme.</span></span> <span data-ttu-id="385d7-158">Válassza ki **alapszintű** , **termékek**.</span><span class="sxs-lookup"><span data-stu-id="385d7-158">Select **Starter** as **Products**.</span></span> <span data-ttu-id="385d7-159">Ha befejezte, kattintson a **mentése** toocreate hello API.</span><span class="sxs-lookup"><span data-stu-id="385d7-159">When finished, click **Save** toocreate hello API.</span></span>

![Adja hozzá-új-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-new-api.png)

## <a name="add-hello-operations"></a><span data-ttu-id="385d7-161">Hello műveletek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="385d7-161">Add hello operations</span></span>
<span data-ttu-id="385d7-162">Kattintson a **hozzáadási művelet** tooadd műveletek toothis API.</span><span class="sxs-lookup"><span data-stu-id="385d7-162">Click **Add operation** tooadd operations toothis API.</span></span>

![Adja hozzá művelet](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-operation.png)

<span data-ttu-id="385d7-164">Hello **új művelet** ablak jelenik meg, és hello **aláírás** alapértelmezett kiválasztása lapon.</span><span class="sxs-lookup"><span data-stu-id="385d7-164">hello **New operation** window will be displayed and hello **Signature** tab will be selected by default.</span></span>

## <a name="add-rrs-operation"></a><span data-ttu-id="385d7-165">RR-EKET művelet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="385d7-165">Add RRS Operation</span></span>
<span data-ttu-id="385d7-166">Először létre kell hoznia a hello szolgáltatást az AzureML RR-EKET művelete.</span><span class="sxs-lookup"><span data-stu-id="385d7-166">First create an operation for hello AzureML RRS service.</span></span> <span data-ttu-id="385d7-167">Válassza ki **POST** , hello **HTTP-műveletet**.</span><span class="sxs-lookup"><span data-stu-id="385d7-167">Select **POST** as hello **HTTP verb**.</span></span> <span data-ttu-id="385d7-168">Típus **/workspaces/ {munkaterület} /services/ {szolgáltatás} / execute? api-version = {apiversion} & részletei = {részletek}** hello, **URL-cím sablon**.</span><span class="sxs-lookup"><span data-stu-id="385d7-168">Type **/workspaces/{workspace}/services/{service}/execute?api-version={apiversion}&details={details}** as hello **URL template**.</span></span> <span data-ttu-id="385d7-169">Típus **RRS hajtható végre** , hello **megjelenített név**.</span><span class="sxs-lookup"><span data-stu-id="385d7-169">Type **RRS Execute** as hello **Display name**.</span></span>

![Adja hozzá-rrs-művelet-aláírása](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-signature.png)

<span data-ttu-id="385d7-171">Kattintson a **válaszok** > **hozzáadása** hello bal, és válassza a **200 OK**.</span><span class="sxs-lookup"><span data-stu-id="385d7-171">Click **Responses** > **ADD** on hello left and select **200 OK**.</span></span> <span data-ttu-id="385d7-172">Kattintson a **mentése** toosave ezt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="385d7-172">Click **Save** toosave this operation.</span></span>

![Adja hozzá-rrs-művelet-válasz](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-response.png)

## <a name="add-bes-operations"></a><span data-ttu-id="385d7-174">Adja hozzá a BES műveletek</span><span class="sxs-lookup"><span data-stu-id="385d7-174">Add BES Operations</span></span>
<span data-ttu-id="385d7-175">Képernyőfelvételek nincsenek hello BES műveletek, mivel ezek nagyon hasonló toothose hello RR-EKET művelet hozzáadásához.</span><span class="sxs-lookup"><span data-stu-id="385d7-175">Screenshots are not included for hello BES operations as they are very similar toothose for adding hello RRS operation.</span></span>

### <a name="submit-but-not-start-a-batch-execution-job"></a><span data-ttu-id="385d7-176">A kötegelt végrehajtási feladatok elküldése (de indul el)</span><span class="sxs-lookup"><span data-stu-id="385d7-176">Submit (but not start) a Batch Execution job</span></span>
<span data-ttu-id="385d7-177">Kattintson a **hozzáadási művelet** tooadd hello AzureML BES művelet toohello API.</span><span class="sxs-lookup"><span data-stu-id="385d7-177">Click **add operation** tooadd hello AzureML BES operation toohello API.</span></span> <span data-ttu-id="385d7-178">Válassza ki **POST** a hello **HTTP-műveletet**.</span><span class="sxs-lookup"><span data-stu-id="385d7-178">Select **POST** for hello **HTTP verb**.</span></span> <span data-ttu-id="385d7-179">Típus **/workspaces/ {munkaterület} /services/ {szolgáltatás} / feladatok? api-version = {apiversion}** a hello **URL-cím sablon**.</span><span class="sxs-lookup"><span data-stu-id="385d7-179">Type **/workspaces/{workspace}/services/{service}/jobs?api-version={apiversion}** for hello **URL template**.</span></span> <span data-ttu-id="385d7-180">Típus **BES nyújt** a hello **megjelenített név**.</span><span class="sxs-lookup"><span data-stu-id="385d7-180">Type **BES Submit** for hello **Display name**.</span></span> <span data-ttu-id="385d7-181">Kattintson a **válaszok** > **hozzáadása** hello bal, és válassza a **200 OK**.</span><span class="sxs-lookup"><span data-stu-id="385d7-181">Click **Responses** > **ADD** on hello left and select **200 OK**.</span></span> <span data-ttu-id="385d7-182">Kattintson a **mentése** toosave ezt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="385d7-182">Click **Save** toosave this operation.</span></span>

### <a name="start-a-batch-execution-job"></a><span data-ttu-id="385d7-183">Kötegelt végrehajtási feladat indítása</span><span class="sxs-lookup"><span data-stu-id="385d7-183">Start a Batch Execution job</span></span>
<span data-ttu-id="385d7-184">Kattintson a **hozzáadási művelet** tooadd hello AzureML BES művelet toohello API.</span><span class="sxs-lookup"><span data-stu-id="385d7-184">Click **add operation** tooadd hello AzureML BES operation toohello API.</span></span> <span data-ttu-id="385d7-185">Válassza ki **POST** a hello **HTTP-műveletet**.</span><span class="sxs-lookup"><span data-stu-id="385d7-185">Select **POST** for hello **HTTP verb**.</span></span> <span data-ttu-id="385d7-186">Típus **/workspaces/ {munkaterület} {szolgáltatás} /services/ /jobs/ {jobid} / start? api-version = {apiversion}** a hello **URL-cím sablon**.</span><span class="sxs-lookup"><span data-stu-id="385d7-186">Type **/workspaces/{workspace}/services/{service}/jobs/{jobid}/start?api-version={apiversion}** for hello **URL template**.</span></span> <span data-ttu-id="385d7-187">Típus **BES Start** a hello **megjelenített név**.</span><span class="sxs-lookup"><span data-stu-id="385d7-187">Type **BES Start** for hello **Display name**.</span></span> <span data-ttu-id="385d7-188">Kattintson a **válaszok** > **hozzáadása** hello bal, és válassza a **200 OK**.</span><span class="sxs-lookup"><span data-stu-id="385d7-188">Click **Responses** > **ADD** on hello left and select **200 OK**.</span></span> <span data-ttu-id="385d7-189">Kattintson a **mentése** toosave ezt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="385d7-189">Click **Save** toosave this operation.</span></span>

### <a name="get-hello-status-or-result-of-a-batch-execution-job"></a><span data-ttu-id="385d7-190">Szerezze be a hello állapot vagy a kötegelt végrehajtási feladat eredménye</span><span class="sxs-lookup"><span data-stu-id="385d7-190">Get hello status or result of a Batch Execution job</span></span>
<span data-ttu-id="385d7-191">Kattintson a **hozzáadási művelet** tooadd hello AzureML BES művelet toohello API.</span><span class="sxs-lookup"><span data-stu-id="385d7-191">Click **add operation** tooadd hello AzureML BES operation toohello API.</span></span> <span data-ttu-id="385d7-192">Válassza ki **beolvasása** a hello **HTTP-műveletet**.</span><span class="sxs-lookup"><span data-stu-id="385d7-192">Select **GET** for hello **HTTP verb**.</span></span> <span data-ttu-id="385d7-193">Típus **/workspaces/ {munkaterület} {szolgáltatás} /services/ /jobs/ {jobid}? api-version = {apiversion}** a hello **URL-cím sablon**.</span><span class="sxs-lookup"><span data-stu-id="385d7-193">Type **/workspaces/{workspace}/services/{service}/jobs/{jobid}?api-version={apiversion}** for hello **URL template**.</span></span> <span data-ttu-id="385d7-194">Típus **BES állapot** a hello **megjelenített név**.</span><span class="sxs-lookup"><span data-stu-id="385d7-194">Type **BES Status** for hello **Display name**.</span></span> <span data-ttu-id="385d7-195">Kattintson a **válaszok** > **hozzáadása** hello bal, és válassza a **200 OK**.</span><span class="sxs-lookup"><span data-stu-id="385d7-195">Click **Responses** > **ADD** on hello left and select **200 OK**.</span></span> <span data-ttu-id="385d7-196">Kattintson a **mentése** toosave ezt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="385d7-196">Click **Save** toosave this operation.</span></span>

### <a name="delete-a-batch-execution-job"></a><span data-ttu-id="385d7-197">A kötegelt végrehajtási feladatok törlése</span><span class="sxs-lookup"><span data-stu-id="385d7-197">Delete a Batch Execution job</span></span>
<span data-ttu-id="385d7-198">Kattintson a **hozzáadási művelet** tooadd hello AzureML BES művelet toohello API.</span><span class="sxs-lookup"><span data-stu-id="385d7-198">Click **add operation** tooadd hello AzureML BES operation toohello API.</span></span> <span data-ttu-id="385d7-199">Válassza ki **törlése** a hello **HTTP-műveletet**.</span><span class="sxs-lookup"><span data-stu-id="385d7-199">Select **DELETE** for hello **HTTP verb**.</span></span> <span data-ttu-id="385d7-200">Típus **/workspaces/ {munkaterület} {szolgáltatás} /services/ /jobs/ {jobid}? api-version = {apiversion}** a hello **URL-cím sablon**.</span><span class="sxs-lookup"><span data-stu-id="385d7-200">Type **/workspaces/{workspace}/services/{service}/jobs/{jobid}?api-version={apiversion}** for hello **URL template**.</span></span> <span data-ttu-id="385d7-201">Típus **BES törlése** a hello **megjelenített név**.</span><span class="sxs-lookup"><span data-stu-id="385d7-201">Type **BES Delete** for hello **Display name**.</span></span> <span data-ttu-id="385d7-202">Kattintson a **válaszok** > **hozzáadása** hello bal, és válassza a **200 OK**.</span><span class="sxs-lookup"><span data-stu-id="385d7-202">Click **Responses** > **ADD** on hello left and select **200 OK**.</span></span> <span data-ttu-id="385d7-203">Kattintson a **mentése** toosave ezt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="385d7-203">Click **Save** toosave this operation.</span></span>

## <a name="call-an-operation-from-hello-developer-portal"></a><span data-ttu-id="385d7-204">Egy művelet hívja hello fejlesztői portálján</span><span class="sxs-lookup"><span data-stu-id="385d7-204">Call an operation from hello Developer Portal</span></span>
<span data-ttu-id="385d7-205">Műveletek hívható közvetlenül a hello fejlesztői portálján, amely egy kényelmes módszert arra tooview biztosít, és tesztelje az API-k hello működésére.</span><span class="sxs-lookup"><span data-stu-id="385d7-205">Operations can be called directly from hello Developer portal, which provides a convenient way tooview and test hello operations of an API.</span></span> <span data-ttu-id="385d7-206">Ez az útmutató lépésben fel fogja hívni hello **RRS hajtható végre** toohello hozzáadott metódus **AzureML bemutató API**.</span><span class="sxs-lookup"><span data-stu-id="385d7-206">In this guide step you will call hello **RRS Execute** method that was added toohello **AzureML Demo API**.</span></span> <span data-ttu-id="385d7-207">Kattintson a **fejlesztői portálján** hello hello menüből a hello klasszikus portál jobb felső.</span><span class="sxs-lookup"><span data-stu-id="385d7-207">Click **Developer portal** from hello menu at hello top right of hello Classic Portal.</span></span>

![fejlesztői-portál](./media/machine-learning-manage-web-service-endpoints-using-api-management/developer-portal.png)

<span data-ttu-id="385d7-209">Kattintson a **API-k** hello felső menüben, majd kattintson a **AzureML bemutató API** toosee hello műveletek érhető el.</span><span class="sxs-lookup"><span data-stu-id="385d7-209">Click **APIs** from hello top menu, and then click **AzureML Demo API** toosee hello operations available.</span></span>

![demoazureml-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/demoazureml-api.png)

<span data-ttu-id="385d7-211">Válassza ki **RRS hajtható végre** hello a művelethez.</span><span class="sxs-lookup"><span data-stu-id="385d7-211">Select **RRS Execute** for hello operation.</span></span> <span data-ttu-id="385d7-212">Kattintson a **kipróbálás**.</span><span class="sxs-lookup"><span data-stu-id="385d7-212">Click **Try It**.</span></span>

![Próbálja meg-it](./media/machine-learning-manage-web-service-endpoints-using-api-management/try-it.png)

<span data-ttu-id="385d7-214">A kérelemben szereplő paraméterek, írja be a **munkaterület**, **szolgáltatás**, **2.0** a hello **apiversion**, és **true**a hello **részletek**.</span><span class="sxs-lookup"><span data-stu-id="385d7-214">For Request parameters, type your **workspace**,  **service**, **2.0** for hello **apiversion**, and  **true** for hello **details**.</span></span> <span data-ttu-id="385d7-215">Megtalálhatja a **munkaterület** és **szolgáltatás** hello AzureML webes szolgáltatás irányítópultján (lásd: **hello webszolgáltatás tesztelése** függelék).</span><span class="sxs-lookup"><span data-stu-id="385d7-215">You can find your **workspace** and **service** in hello AzureML web service dashboard (see **Test hello web service** in Appendix A).</span></span>

<span data-ttu-id="385d7-216">Kérelemfejléc, kattintson **Hozzáadás fejléc** és írja be **Content-Type** és **az application/json**, majd kattintson a **Hozzáadás fejléc** és írja be **engedélyezési** és **tulajdonosi <YOUR AZUREML SERVICE API-KEY>** .</span><span class="sxs-lookup"><span data-stu-id="385d7-216">For Request headers, click **Add header** and type **Content-Type** and **application/json**, then click **Add header** and type **Authorization** and **Bearer <YOUR AZUREML SERVICE API-KEY>**.</span></span> <span data-ttu-id="385d7-217">Megtalálhatja a **api-kulcs** hello AzureML webes szolgáltatás irányítópultján (lásd: **hello webszolgáltatás tesztelése** függelék).</span><span class="sxs-lookup"><span data-stu-id="385d7-217">You can find your **api key** in hello AzureML web service dashboard (see **Test hello web service** in Appendix A).</span></span>

<span data-ttu-id="385d7-218">Típus **{"Bemenetek": {"input1": {"ColumnNames": "Értékek" ["Col2"]: [["Ez az a jó naponta"]]}}, "GlobalParameters": {}}** hello kérelemtörzset a.</span><span class="sxs-lookup"><span data-stu-id="385d7-218">Type **{"Inputs": {"input1": {"ColumnNames": ["Col2"], "Values": [["This is a good day"]]}}, "GlobalParameters": {}}** for hello request body.</span></span>

![az azureml-bemutató-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/azureml-demo-api.png)

<span data-ttu-id="385d7-220">Kattintson a **küldése**.</span><span class="sxs-lookup"><span data-stu-id="385d7-220">Click **Send**.</span></span>

![Küldés](./media/machine-learning-manage-web-service-endpoints-using-api-management/send.png)

<span data-ttu-id="385d7-222">Után egy művelet indítása, hello fejlesztői portál megjeleníti-e a hello **a kért URL-cím** hello háttér-szolgáltatásból hello **válaszállapot**, hello **válaszfejlécek**, és bármely **válasz tartalom**.</span><span class="sxs-lookup"><span data-stu-id="385d7-222">After an operation is invoked, hello developer portal displays hello **Requested URL** from hello back-end service, hello **Response status**, hello **Response headers**, and any **Response content**.</span></span>

![válasz-állapot](./media/machine-learning-manage-web-service-endpoints-using-api-management/response-status.png)

## <a name="appendix-a---creating-and-testing-a-simple-azureml-web-service"></a><span data-ttu-id="385d7-224">A függelék – létrehozása és tesztelése egy egyszerű AzureML webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="385d7-224">Appendix A - Creating and testing a simple AzureML web service</span></span>
### <a name="creating-hello-experiment"></a><span data-ttu-id="385d7-225">Hello kísérlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="385d7-225">Creating hello experiment</span></span>
<span data-ttu-id="385d7-226">Az alábbiakban egy egyszerű AzureML kísérlet létrehozását és telepítését webszolgáltatásként hello lépéseit.</span><span class="sxs-lookup"><span data-stu-id="385d7-226">Below are hello steps for creating a simple AzureML experiment and deploying it as a web service.</span></span> <span data-ttu-id="385d7-227">webes szolgáltatás vesz hello bemenetként a tetszőleges szöveget, és adja vissza egész számok-ként funkciókat.</span><span class="sxs-lookup"><span data-stu-id="385d7-227">hello web service takes as input a column of arbitrary text and returns a set of features represented as integers.</span></span> <span data-ttu-id="385d7-228">Példa:</span><span class="sxs-lookup"><span data-stu-id="385d7-228">For example:</span></span>

| <span data-ttu-id="385d7-229">Szöveg</span><span class="sxs-lookup"><span data-stu-id="385d7-229">Text</span></span> | <span data-ttu-id="385d7-230">Kivonatolt szöveg</span><span class="sxs-lookup"><span data-stu-id="385d7-230">Hashed Text</span></span> |
| --- | --- |
| <span data-ttu-id="385d7-231">Ez az jó naponta</span><span class="sxs-lookup"><span data-stu-id="385d7-231">This is a good day</span></span> |<span data-ttu-id="385d7-232">1 1 2 2 0 2 0 1</span><span class="sxs-lookup"><span data-stu-id="385d7-232">1 1 2 2 0 2 0 1</span></span> |

<span data-ttu-id="385d7-233">Először egy böngészőt, nyissa meg azt: [https://studio.azureml.net/](https://studio.azureml.net/) , és írja be a hitelesítő adatok toolog a.</span><span class="sxs-lookup"><span data-stu-id="385d7-233">First, using a browser of your choice, navigate to: [https://studio.azureml.net/](https://studio.azureml.net/) and enter your credentials toolog in.</span></span> <span data-ttu-id="385d7-234">Ezután hozzon létre egy új üres kísérlet.</span><span class="sxs-lookup"><span data-stu-id="385d7-234">Next, create a new blank experiment.</span></span>

![Keresés – kísérlet-sablonok](./media/machine-learning-manage-web-service-endpoints-using-api-management/search-experiment-templates.png)

<span data-ttu-id="385d7-236">Nevezze át túl**SimpleFeatureHashingExperiment**.</span><span class="sxs-lookup"><span data-stu-id="385d7-236">Rename it too**SimpleFeatureHashingExperiment**.</span></span> <span data-ttu-id="385d7-237">Bontsa ki a **mentett adatkészletek** , és húzza **könyv értékelést az Amazon** alakzatot a kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="385d7-237">Expand **Saved Datasets** and drag **Book Reviews from Amazon** onto your experiment.</span></span>

![egyszerű-szolgáltatás-kivonatolás-kísérlet](./media/machine-learning-manage-web-service-endpoints-using-api-management/simple-feature-hashing-experiment.png)

<span data-ttu-id="385d7-239">Bontsa ki a **Data Transformation** és **adatkezelési** , és húzza **Select Columns in Dataset** alakzatot a kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="385d7-239">Expand **Data Transformation** and **Manipulation** and drag **Select Columns in Dataset** onto your experiment.</span></span> <span data-ttu-id="385d7-240">Csatlakozás **könyv értékelést az Amazon** túl**Select Columns in Dataset**.</span><span class="sxs-lookup"><span data-stu-id="385d7-240">Connect **Book Reviews from Amazon** too**Select Columns in Dataset**.</span></span>

![select-columns](./media/machine-learning-manage-web-service-endpoints-using-api-management/project-columns.png)

<span data-ttu-id="385d7-242">Kattintson a **Select Columns in Dataset** majd **Oszlopválasztás** válassza **Col2**.</span><span class="sxs-lookup"><span data-stu-id="385d7-242">Click **Select Columns in Dataset** and then click **Launch column selector** and select **Col2**.</span></span> <span data-ttu-id="385d7-243">Kattintson a pipa tooapply hello ezeket a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="385d7-243">Click hello checkmark tooapply these changes.</span></span>

![select-columns](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-columns.png)

<span data-ttu-id="385d7-245">Bontsa ki a **Szövegelemzések** , és húzza **Szolgáltatáskivonatolás** alakzatot hello kísérlet.</span><span class="sxs-lookup"><span data-stu-id="385d7-245">Expand **Text Analytics** and drag **Feature Hashing** onto hello experiment.</span></span> <span data-ttu-id="385d7-246">Csatlakozás **Select Columns in Dataset** túl**Szolgáltatáskivonatolás**.</span><span class="sxs-lookup"><span data-stu-id="385d7-246">Connect **Select Columns in Dataset** too**Feature Hashing**.</span></span>

![Connect--projektoszlopok](./media/machine-learning-manage-web-service-endpoints-using-api-management/connect-project-columns.png)

<span data-ttu-id="385d7-248">Típus **3** a hello **bitsize kivonatoláshoz**.</span><span class="sxs-lookup"><span data-stu-id="385d7-248">Type **3** for hello **Hashing bitsize**.</span></span> <span data-ttu-id="385d7-249">Ezzel létrehoz 8 (23) oszlopot.</span><span class="sxs-lookup"><span data-stu-id="385d7-249">This will create 8 (23) columns.</span></span>

![kivonatoló bitsize](./media/machine-learning-manage-web-service-endpoints-using-api-management/hashing-bitsize.png)

<span data-ttu-id="385d7-251">Ezen a ponton érdemes lehet tooclick **futtatása** tootest hello kísérlet.</span><span class="sxs-lookup"><span data-stu-id="385d7-251">At this point, you may want tooclick **Run** tootest hello experiment.</span></span>

![Futtatás](./media/machine-learning-manage-web-service-endpoints-using-api-management/run.png)

### <a name="create-a-web-service"></a><span data-ttu-id="385d7-253">Webszolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="385d7-253">Create a web service</span></span>
<span data-ttu-id="385d7-254">Most hozzon létre egy webszolgáltatás-bővítmény.</span><span class="sxs-lookup"><span data-stu-id="385d7-254">Now create a web service.</span></span> <span data-ttu-id="385d7-255">Bontsa ki a **webszolgáltatás** , és húzza **bemeneti** alakzatot a kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="385d7-255">Expand **Web Service** and drag **Input** onto your experiment.</span></span> <span data-ttu-id="385d7-256">Csatlakozás **bemeneti** túl**Szolgáltatáskivonatolás**.</span><span class="sxs-lookup"><span data-stu-id="385d7-256">Connect **Input** too**Feature Hashing**.</span></span> <span data-ttu-id="385d7-257">Húzással is **kimeneti** alakzatot a kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="385d7-257">Also drag **output** onto your experiment.</span></span> <span data-ttu-id="385d7-258">Csatlakozás **kimeneti** túl**Szolgáltatáskivonatolás**.</span><span class="sxs-lookup"><span data-stu-id="385d7-258">Connect **Output** too**Feature Hashing**.</span></span>

![kimeneti-az-szolgáltatás-kivonatolás](./media/machine-learning-manage-web-service-endpoints-using-api-management/output-to-feature-hashing.png)

<span data-ttu-id="385d7-260">Kattintson a **webszolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="385d7-260">Click **Publish web service**.</span></span>

![közzététel-webszolgáltatás](./media/machine-learning-manage-web-service-endpoints-using-api-management/publish-web-service.png)

<span data-ttu-id="385d7-262">Kattintson a **Igen** toopublish hello kísérlet.</span><span class="sxs-lookup"><span data-stu-id="385d7-262">Click **Yes** toopublish hello experiment.</span></span>

![Igen – közzététele](./media/machine-learning-manage-web-service-endpoints-using-api-management/yes-to-publish.png)

### <a name="test-hello-web-service"></a><span data-ttu-id="385d7-264">Teszt hello webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="385d7-264">Test hello web service</span></span>
<span data-ttu-id="385d7-265">Az AzureML webszolgáltatás RSS (kérelem/válasz szolgáltatás) és a BES (kötegelt végrehajtási szolgáltatás) végpontok áll.</span><span class="sxs-lookup"><span data-stu-id="385d7-265">An AzureML web service consists of RSS (request/response service) and BES (batch execution service) endpoints.</span></span> <span data-ttu-id="385d7-266">RSS szinkron végrehajtásra van.</span><span class="sxs-lookup"><span data-stu-id="385d7-266">RSS is for synchronous execution.</span></span> <span data-ttu-id="385d7-267">BES aszinkron feladat végrehajtása szolgál.</span><span class="sxs-lookup"><span data-stu-id="385d7-267">BES is for asynchronous job execution.</span></span> <span data-ttu-id="385d7-268">a webes szolgáltatás hello minta Python forrás alábbi tootest, esetleg toodownload és az Azure SDK telepítése hello Python (lásd: [hogyan tooinstall Python](../python-how-to-install.md)).</span><span class="sxs-lookup"><span data-stu-id="385d7-268">tootest your web service with hello sample Python source below, you may need toodownload and install hello Azure SDK for Python (see: [How tooinstall Python](../python-how-to-install.md)).</span></span>

<span data-ttu-id="385d7-269">Konfigurálnia kell hello **munkaterület**, **szolgáltatás**, és **api_key** hello minta forrás alábbi kísérletbe.</span><span class="sxs-lookup"><span data-stu-id="385d7-269">You will also need hello **workspace**, **service**, and **api_key** of your experiment for hello sample source below.</span></span> <span data-ttu-id="385d7-270">Található hello munkaterületet, és a szolgáltatás lehet **kérelem/válasz** vagy **kötegelt végrehajtási** hello webes szolgáltatás irányítópultján a kísérleti fázisú funkciókat.</span><span class="sxs-lookup"><span data-stu-id="385d7-270">You can find hello workspace and service by clicking either **Request/Response** or **Batch Execution** for your experiment in hello web service dashboard.</span></span>

![Keresés – munkaterület-és-szolgáltatás](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-workspace-and-service.png)

<span data-ttu-id="385d7-272">Hello található **api_key** hello webes szolgáltatás irányítópultján kísérletbe kattintva.</span><span class="sxs-lookup"><span data-stu-id="385d7-272">You can find hello **api_key** by clicking your experiment in hello web service dashboard.</span></span>

![Keresés – api-kulcs](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-api-key.png)

#### <a name="test-rrs-endpoint"></a><span data-ttu-id="385d7-274">Teszt RR-EKET végpont</span><span class="sxs-lookup"><span data-stu-id="385d7-274">Test RRS endpoint</span></span>
##### <a name="test-button"></a><span data-ttu-id="385d7-275">Teszt gomb</span><span class="sxs-lookup"><span data-stu-id="385d7-275">Test button</span></span>
<span data-ttu-id="385d7-276">Egy egyszerű módot tootest hello RRS-végpont esetében tooclick **teszt** hello webes szolgáltatás irányítópultján.</span><span class="sxs-lookup"><span data-stu-id="385d7-276">An easy way tootest hello RRS endpoint is tooclick **Test** on hello web service dashboard.</span></span>

![Teszt](./media/machine-learning-manage-web-service-endpoints-using-api-management/test.png)

<span data-ttu-id="385d7-278">Típus **Ez az jó naponta** a **col2**.</span><span class="sxs-lookup"><span data-stu-id="385d7-278">Type **This is a good day** for **col2**.</span></span> <span data-ttu-id="385d7-279">Kattintson a pipa hello.</span><span class="sxs-lookup"><span data-stu-id="385d7-279">Click hello checkmark.</span></span>

![Adja meg adatok](./media/machine-learning-manage-web-service-endpoints-using-api-management/enter-data.png)

<span data-ttu-id="385d7-281">Látni fogja hasonlót</span><span class="sxs-lookup"><span data-stu-id="385d7-281">You will see something like</span></span>

![minta-kimenet](./media/machine-learning-manage-web-service-endpoints-using-api-management/sample-output.png)

##### <a name="sample-code"></a><span data-ttu-id="385d7-283">Mintakód</span><span class="sxs-lookup"><span data-stu-id="385d7-283">Sample Code</span></span>
<span data-ttu-id="385d7-284">Egy másik módja tootest a RR-EKET az Ügyfélkód származik.</span><span class="sxs-lookup"><span data-stu-id="385d7-284">Another way tootest your RRS is from your client code.</span></span> <span data-ttu-id="385d7-285">Ha **kérelem/válasz** hello irányítópult és görgetési toohello alsó, jelennek meg példakód C#, Python, és R. Látni fogja emellett hello RR-EKET kérelem, beleértve a hello kérelem URI, fejlécek és fő hello szintaxisát.</span><span class="sxs-lookup"><span data-stu-id="385d7-285">If you click **Request/response** on hello dashboard and scroll toohello bottom, you will see sample code for C#, Python, and R. You will also see hello syntax of hello RRS request, including hello request URI, headers, and body.</span></span>

<span data-ttu-id="385d7-286">Ez az útmutató bemutatja egy működő Python példa.</span><span class="sxs-lookup"><span data-stu-id="385d7-286">This guide shows a working Python example.</span></span> <span data-ttu-id="385d7-287">Szüksége lesz a toomodify azt hello **munkaterület**, **szolgáltatás**, és **api_key** a kísérlet.</span><span class="sxs-lookup"><span data-stu-id="385d7-287">You will need toomodify it with hello **workspace**, **service**, and **api_key** of your experiment.</span></span>

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
        print("hello request failed with status code: " + str(error.code))
        print(error.info())
        print(json.loads(error.read()))

#### <a name="test-bes-endpoint"></a><span data-ttu-id="385d7-288">Teszt BES végpont</span><span class="sxs-lookup"><span data-stu-id="385d7-288">Test BES endpoint</span></span>
<span data-ttu-id="385d7-289">Kattintson a **kötegelt végrehajtás** hello irányítópult és görgetési toohello alján.</span><span class="sxs-lookup"><span data-stu-id="385d7-289">Click **Batch execution** on hello dashboard and scroll toohello bottom.</span></span> <span data-ttu-id="385d7-290">Mintakód látni fogja a C#, Python, és R. Látni fogja emellett hello szintaxisát hello BES kérelmek toosubmit egy feladatot, elindíthat egy feladatot, hello állapot vagy a feladat eredményeinek lekérése és törli a feladatot.</span><span class="sxs-lookup"><span data-stu-id="385d7-290">You will see sample code for C#, Python, and R. You will also see hello syntax of hello BES requests toosubmit a job, start a job, get hello status or results of a job, and delete a job.</span></span>

<span data-ttu-id="385d7-291">Ez az útmutató bemutatja egy működő Python példa.</span><span class="sxs-lookup"><span data-stu-id="385d7-291">This guide shows a working Python example.</span></span> <span data-ttu-id="385d7-292">Toomodify van szüksége a hello **munkaterület**, **szolgáltatás**, és **api_key** a kísérlet.</span><span class="sxs-lookup"><span data-stu-id="385d7-292">You need toomodify it with hello **workspace**, **service**, and **api_key** of your experiment.</span></span> <span data-ttu-id="385d7-293">Továbbá szüksége toomodify hello **tárfióknév**, **tárfiók kulcsa**, és **storage-tároló nevének**.</span><span class="sxs-lookup"><span data-stu-id="385d7-293">Additionally, you need toomodify hello **storage account name**, **storage account key**, and **storage container name**.</span></span> <span data-ttu-id="385d7-294">Végül, szüksége lesz toomodify hello helyét hello **bemeneti fájl** és hello hello helyének **kimeneti fájl**.</span><span class="sxs-lookup"><span data-stu-id="385d7-294">Lastly, you will need toomodify hello location of hello **input file** and hello location of hello **output file**.</span></span>

    import urllib2
    import json
    import time
    from azure.storage import *
    workspace = "<REPLACE WITH YOUR WORKSPACE ID>"
    service = "<REPLACE WITH YOUR SERVICE ID>"
    api_key = "<REPLACE WITH hello API KEY FOR YOUR WEB SERVICE>"
    storage_account_name = "<REPLACE WITH YOUR AZURE STORAGE ACCOUNT NAME>"
    storage_account_key = "<REPLACE WITH YOUR AZURE STORAGE KEY>"
    storage_container_name = "<REPLACE WITH YOUR AZURE STORAGE CONTAINER NAME>"
    input_file = "<REPLACE WITH hello LOCATION OF YOUR INPUT FILE>" # Example: C:\\mydata.csv
    output_file = "<REPLACE WITH hello LOCATION OF YOUR OUTPUT FILE>" # Example: C:\\myresults.csv
    input_blob_name = "mydatablob.csv"
    output_blob_name = "myresultsblob.csv"
    def printHttpError(httpError):
    print("hello request failed with status code: " + str(httpError.code))
    print(httpError.info())
    print(json.loads(httpError.read()))
    return
    def saveBlobToFile(blobUrl, resultsLabel):
    print("Reading hello result from " + blobUrl)
    try:
        response = urllib2.urlopen(blobUrl)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    with open(output_file, "w+") as f:
        f.write(response.read())
    print(resultsLabel + " have been written toohello file " + output_file)
    return
    def processResults(result):
    first = True
    results = result["Results"]
    for outputName in results:
        result_blob_location = results[outputName]
        sas_token = result_blob_location["SasBlobToken"]
        base_url = result_blob_location["BaseLocation"]
        relative_url = result_blob_location["RelativeLocation"]
        print("hello results for " + outputName + " are available at hello following Azure Storage location:")
        print("BaseLocation: " + base_url)
        print("RelativeLocation: " + relative_url)
        print("SasBlobToken: " + sas_token)
        if (first):
            first = False
            url3 = base_url + relative_url + sas_token
            saveBlobToFile(url3, "hello results for " + outputName)
    return

    def invokeBatchExecutionService():
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace +"/services/" + service +"/jobs"
    blob_service = BlobService(account_name=storage_account_name, account_key=storage_account_key)
    print("Uploading hello input tooblob storage...")
    data_to_upload = open(input_file, "r").read()
    blob_service.put_blob(storage_container_name, input_blob_name, data_to_upload, x_ms_blob_type="BlockBlob")
    print "Uploaded hello input tooblob storage"
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
    print("Submitting hello job...")
    # submit hello job
    req = urllib2.Request(url + "?api-version=2.0", body, headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    result = response.read()
    job_id = result[1:-1] # remove hello enclosing double-quotes
    print("Job ID: " + job_id)
    # start hello job
    print("Starting hello job...")
    req = urllib2.Request(url + "/" + job_id + "/start?api-version=2.0", "", headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    url2 = url + "/" + job_id + "?api-version=2.0"

    while True:
        print("Checking hello job status...")
        # If you are using Python 3+, replace urllib2 with urllib.request in hello follwing code
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
