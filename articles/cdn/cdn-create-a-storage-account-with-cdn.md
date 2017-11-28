---
title: "egy Azure storage-fiók Azure CDN aaaIntegrate |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Azure tartalom Delivery Network (CDN) toodeliver nagy sávszélességű tartalom gyorsítótárazása révén blobok Azure Storage-ból."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: cbc2ff98-916d-4339-8959-622823c5b772
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: e44716969d6a784265cc4b1da34f0d021a17b38d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-an-azure-storage-account-with-azure-cdn"></a><span data-ttu-id="1a5a1-103">Azure-tárfiók integrálása az Azure CDN szolgáltatás használata</span><span class="sxs-lookup"><span data-stu-id="1a5a1-103">Integrate an Azure storage account with Azure CDN</span></span>
<span data-ttu-id="1a5a1-104">CDN lehet engedélyezve az Azure tárterületet lévő tartalmak toocache.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-104">CDN can be enabled toocache content from your Azure storage.</span></span> <span data-ttu-id="1a5a1-105">A fejlesztők a tartalmak nagy sávszélességű kézbesítéséhez a blobok és számítási példányokért fizikai csomópontokon hello az Amerikai Egyesült Államok, Európa, Ázsia, Ausztrália és Dél-Amerika a statikus tartalom gyorsítótárazása révén globális megoldást kínál.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-105">It offers developers a global solution for delivering high-bandwidth content by caching blobs and static content of compute instances at physical nodes in hello United States, Europe, Asia, Australia and South America.</span></span>

## <a name="step-1-create-a-storage-account"></a><span data-ttu-id="1a5a1-106">1. lépés: Tárfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="1a5a1-106">Step 1: Create a storage account</span></span>
<span data-ttu-id="1a5a1-107">A következő eljárás toocreate egy új tárfiókot, Azure-előfizetéshez tartozó hello használata.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-107">Use hello following procedure toocreate a new storage account for a Azure subscription.</span></span> <span data-ttu-id="1a5a1-108">A storage-fiók az Azure storage-szolgáltatásokhoz való hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-108">A storage account gives access to Azure storage services.</span></span> <span data-ttu-id="1a5a1-109">hello tárfiók hello legmagasabb szintű hello névtér eléréséhez hello az Azure storage szolgáltatás összetevői jelöli: Blob-szolgáltatások, Queue szolgáltatások és Table szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-109">hello storage account represents hello highest level of hello namespace for accessing each of hello Azure storage service components: Blob services, Queue services, and Table services.</span></span> <span data-ttu-id="1a5a1-110">További információkért tekintse meg a toohello [Azure Storage bemutatása tooMicrosoft](../storage/common/storage-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="1a5a1-110">For more information, refer toohello [Introduction tooMicrosoft Azure Storage](../storage/common/storage-introduction.md).</span></span>

<span data-ttu-id="1a5a1-111">a tárfiók toocreate, kell lennie, vagy hello szolgáltatás rendszergazdájának vagy társadminisztrátorának a kapcsolódó hello előfizetés.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-111">toocreate a storage account, you must be either hello service administrator or a co-administrator for hello associated subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="1a5a1-112">Többféleképpen is használhat egy tárfiókot, beleértve a hello Azure portál és a Powershell toocreate.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-112">There are several methods you can use toocreate a storage account, including hello Azure Portal and Powershell.</span></span>  <span data-ttu-id="1a5a1-113">Ebben az oktatóanyagban használni fogjuk hello Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-113">For this tutorial, we'll be using hello Azure Portal.</span></span>  
> 
> 

<span data-ttu-id="1a5a1-114">**toocreate egy tárfiókot, Azure-előfizetések**</span><span class="sxs-lookup"><span data-stu-id="1a5a1-114">**toocreate a storage account for an Azure subscription**</span></span>

1. <span data-ttu-id="1a5a1-115">Jelentkezzen be toohello [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1a5a1-115">Sign in toohello [Azure Portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="1a5a1-116">Hello bal felső sarokban, válassza ki **új**.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-116">In hello upper left corner, select **New**.</span></span> <span data-ttu-id="1a5a1-117">A hello **új** párbeszédpanelen válassza **adatok + tárolás**, majd kattintson a **tárfiók**.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-117">In hello **New** Dialog, select **Data  + Storage**, then click **Storage account**.</span></span>
    
    <span data-ttu-id="1a5a1-118">Hello **storage-fiók létrehozása** panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-118">hello **Create storage account** blade appears.</span></span>   

    ![Storage-fiók létrehozása][create-new-storage-account]  

3. <span data-ttu-id="1a5a1-120">A hello **neve** mezőbe írja be egy altartomány nevét.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-120">In hello **Name** field, type a subdomain name.</span></span> <span data-ttu-id="1a5a1-121">Ez a bejegyzés 3-24 kisbetűket és számokat tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-121">This entry can contain 3-24 lowercase letters and numbers.</span></span>
   
    <span data-ttu-id="1a5a1-122">Ez az érték lesz hello állomásnév belül hello hello előfizetés Blob, sor vagy tábla erőforrásainak címzéséhez használt URI-Azonosítóra.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-122">This value becomes hello host name within hello URI that is used to address Blob, Queue, or Table resources for hello subscription.</span></span> <span data-ttu-id="1a5a1-123">A Blob szolgáltatás hello tároló erőforrás megoldására használt URI hello a következő formátumban, ahol  *&lt;StorageAccountLabel&gt;*  beírt toohello érték hivatkozik **URL-címetadjonmeg**:</span><span class="sxs-lookup"><span data-stu-id="1a5a1-123">To address a container resource in hello Blob service, you would use a URI in hello following format, where *&lt;StorageAccountLabel&gt;* refers toohello value you typed in **Enter a URL**:</span></span>
   
    <span data-ttu-id="1a5a1-124">http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mycontainer&gt;*</span><span class="sxs-lookup"><span data-stu-id="1a5a1-124">http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mycontainer&gt;*</span></span>
   
    <span data-ttu-id="1a5a1-125">**Fontos:** hello URL-cím címke űrlapok hello altartomány hello storage-fiókjának URI és az Azure-ban az összes üzemeltetett szolgáltatások egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-125">**Important:** hello URL label forms hello subdomain of hello storage  account URI and must be unique among all hosted services in  Azure.</span></span>
   
    <span data-ttu-id="1a5a1-126">Ez az érték is használható hello neveként a tárfiók hello portálon, vagy ennek a fióknak programozott módon való hozzáférés során.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-126">This value is also used as hello name of this storage account in hello portal, or when accessing this account programmatically.</span></span>
4. <span data-ttu-id="1a5a1-127">Hagyja meg az alapértelmezett hello beállításait **telepítési modell**, **fiók kind**, **teljesítmény**, és **replikációs**.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-127">Leave hello defaults for **Deployment model**, **Account kind**, **Performance**, and **Replication**.</span></span> 
5. <span data-ttu-id="1a5a1-128">Jelölje be hello **előfizetés** , hogy hello tárfiók lesz használható.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-128">Select hello **Subscription** that hello storage account will be used with.</span></span>
6. <span data-ttu-id="1a5a1-129">Válasszon ki vagy hozzon létre egy **erőforráscsoportot**.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-129">Select or create a **Resource Group**.</span></span>  <span data-ttu-id="1a5a1-130">További információ az erőforráscsoportokkal kapcsolatban: [Az Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md#resource-groups).</span><span class="sxs-lookup"><span data-stu-id="1a5a1-130">For more information on Resource Groups, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md#resource-groups).</span></span>
7. <span data-ttu-id="1a5a1-131">Válasszon egy helyet a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-131">Select a location for your storage account.</span></span>
8. <span data-ttu-id="1a5a1-132">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-132">Click **Create**.</span></span> <span data-ttu-id="1a5a1-133">hello a hello storage-fiók létrehozása eltarthat néhány percig toocomplete.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-133">hello process of creating hello storage account might take several minutes toocomplete.</span></span>

## <a name="step-2-enable-cdn-for-hello-storage-account"></a><span data-ttu-id="1a5a1-134">2. lépés: A hello tárfiók CDN engedélyezése</span><span class="sxs-lookup"><span data-stu-id="1a5a1-134">Step 2: Enable CDN for hello storage account</span></span>

<span data-ttu-id="1a5a1-135">A legújabb integrációs hello most már engedélyezheti CDN a tárfiók a tárolási portálbővítményt maradjanak.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-135">With hello newest integration, you can now enable CDN for your storage account without leaving your storage portal extension.</span></span> 

1. <span data-ttu-id="1a5a1-136">Válassza ki hello tárfiókot, "CDN" vagy görgessen lefelé kereshet hello bal oldali navigációs menü, majd kattintson az "Azure CDN".</span><span class="sxs-lookup"><span data-stu-id="1a5a1-136">Select hello storage account, search "CDN" or scroll down from hello left navigation menu, then click "Azure CDN".</span></span>
    
    <span data-ttu-id="1a5a1-137">Hello **Azure CDN** panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-137">hello **Azure CDN** blade appears.</span></span>

    ![CDN engedélyezése navigációs][cdn-enable-navigation]
    
2. <span data-ttu-id="1a5a1-139">Új végpont létrehozásához szükséges hello beírásával</span><span class="sxs-lookup"><span data-stu-id="1a5a1-139">Create a new endpoint by entering hello required information</span></span>
    - <span data-ttu-id="1a5a1-140">**CDN-profil**: hozzon létre egy új, vagy egy meglévő profil.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-140">**CDN Profile**: You can create a new or use an existing profile.</span></span>
    - <span data-ttu-id="1a5a1-141">**IP-címek**: csak akkor kell tooselect egy tarifacsomagra új CDN-profil létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-141">**Pricing tier**: You only need tooselect a pricing tier if you create a new CDN profile.</span></span>
    - <span data-ttu-id="1a5a1-142">**CDN-végpont nevének**: Adja meg a választott / egy végpont nevét.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-142">**CDN endpoint name**: Enter an endpoint name per your choice.</span></span>

    > [!TIP]
    > <span data-ttu-id="1a5a1-143">CDN-végpont létrehozása hello származási hello állomásnév a tárfiók alapértelmezés szerint használja.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-143">hello created CDN endpoint uses hello hostname of your storage account as origin by default.</span></span>

    <span data-ttu-id="1a5a1-144">! [cdn új végpont létrehozásához] [a cdn-új-végpont-létrehozása]</span><span class="sxs-lookup"><span data-stu-id="1a5a1-144">![cdn new endpoint creation][cdn-new-endpoint-creation]</span></span>

3. <span data-ttu-id="1a5a1-145">A létrehozás után hello új végpont a fenti hello végpont listában jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-145">After creation, hello new endpoint will show up in hello endpoint list above.</span></span>

    ![tárolási új CDN-végpont][cdn-storage-new-endpoint]

> [!NOTE]
> <span data-ttu-id="1a5a1-147">TooAzure CDN bővítmény tooenable CDN is választhatja. [Oktatóanyag](#Tutorial-cdn-create-profile).</span><span class="sxs-lookup"><span data-stu-id="1a5a1-147">You can also go tooAzure CDN extension tooenable CDN.[Tutorial](#Tutorial-cdn-create-profile).</span></span>
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]  

## <a name="step-3-enable-additional-cdn-features"></a><span data-ttu-id="1a5a1-148">3. lépés: További CDN-funkciók engedélyezése</span><span class="sxs-lookup"><span data-stu-id="1a5a1-148">Step 3: Enable additional CDN features</span></span>

<span data-ttu-id="1a5a1-149">A storage-fiók "Azure CDN" panelen kattintson az hello CDN-végpont hello lista tooopen CDN konfigurációs paneljén.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-149">From storage account "Azure CDN" blade, click hello CDN endpoint from hello list tooopen CDN configuration blade.</span></span> <span data-ttu-id="1a5a1-150">További CDN szolgáltatásai engedélyezheti a kézbesítésre, például a tömörítés, lekérdezési karakterláncot, földrajzi szűrést.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-150">You can enable additional CDN features for your delivery, such as compression, query string, geo filtering.</span></span> <span data-ttu-id="1a5a1-151">Adja hozzá az egyéni tartomány leképezése tooyour CDN-végpont is, és az egyéni tartomány HTTPS engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-151">You can also add custom domain mapping tooyour CDN endpoint and enable custom domain HTTPS.</span></span>
    
![CDN cdn tárolással][cdn-storage-cdn-configuration]

## <a name="step-4-access-cdn-content"></a><span data-ttu-id="1a5a1-153">4. lépés: Hozzáférési CDN-tartalom</span><span class="sxs-lookup"><span data-stu-id="1a5a1-153">Step 4: Access CDN content</span></span>
<span data-ttu-id="1a5a1-154">megadott tooaccess gyorsítótárba helyezték a tartalmat a hello CDN, a CDN URL-cím használata hello hello portálon.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-154">tooaccess cached content on hello CDN, use hello CDN URL provided in hello portal.</span></span> <span data-ttu-id="1a5a1-155">gyorsítótárazott blob hello címet hasonló toohello következő lesz:</span><span class="sxs-lookup"><span data-stu-id="1a5a1-155">hello address for a cached blob will be similar toohello following:</span></span>

<span data-ttu-id="1a5a1-156">http://<*EndpointName*\>.azureedge.net/ <*myPublicContainer*\>/<*Blobnév*\></span><span class="sxs-lookup"><span data-stu-id="1a5a1-156">http://<*EndpointName*\>.azureedge.net/<*myPublicContainer*\>/<*BlobName*\></span></span>

> [!NOTE]
> <span data-ttu-id="1a5a1-157">Ha engedélyezi a CDN hozzáférés tooa storage-fiók, az összes nyilvánosan elérhető objektumok jogosultak a CDN peremhálózati gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-157">Once you enable CDN access tooa storage account, all publicly available objects are eligible for CDN edge caching.</span></span> <span data-ttu-id="1a5a1-158">Ha módosít egy objektumot, amely jelenleg tárolja a hello CDN, hello új tartalmak nem lesznek elérhetők keresztül hello CDN amíg nem hello CDN tartalmát frissíti, hello gyorsítótárazott tartalom idő a működés közbeni időszak lejártával.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-158">If you modify an object that is currently cached in hello CDN, hello new content will not be available via hello CDN until hello CDN refreshes its content when hello cached content time-to-live period expires.</span></span>
> 
> 

## <a name="step-5-remove-content-from-hello-cdn"></a><span data-ttu-id="1a5a1-159">5. lépés: Tartalom eltávolítása a CDN hello</span><span class="sxs-lookup"><span data-stu-id="1a5a1-159">Step 5: Remove content from hello CDN</span></span>
<span data-ttu-id="1a5a1-160">Ha már nem kívánja toocache egy objektumot a hello Azure Content Delivery Network (CDN), akkor hello lépések valamelyikét hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="1a5a1-160">If you no longer wish toocache an object in hello Azure Content Delivery Network (CDN), you can take one of hello following steps:</span></span>

* <span data-ttu-id="1a5a1-161">Hogy hello tároló privát nyilvános helyett.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-161">You can make hello container private instead of public.</span></span> <span data-ttu-id="1a5a1-162">Lásd: [kezelheti a névtelen olvasási hozzáférés toocontainers és blobok](../storage/blobs/storage-manage-access-to-resources.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-162">See [Manage anonymous read access toocontainers and blobs](../storage/blobs/storage-manage-access-to-resources.md) for more information.</span></span>
* <span data-ttu-id="1a5a1-163">Tiltsa le, vagy törölni hello CDN-végpontot hello felügyeleti portál használatával.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-163">You can disable or delete hello CDN endpoint using hello Management Portal.</span></span>
* <span data-ttu-id="1a5a1-164">Az üzemeltetett szolgáltatás toono hosszabb válaszoljon toorequests hello objektum módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-164">You can modify your hosted service toono longer respond toorequests for hello object.</span></span>

<span data-ttu-id="1a5a1-165">Az objektum már gyorsítótárazza a hello CDN gyorsítótárában marad, amíg hello objektum hello idő a működés közbeni időszak lejár, vagy hello végpont véglegesen törlődnek.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-165">An object already cached in hello CDN will remain cached until hello time-to-live period for hello object expires or until hello endpoint is purged.</span></span> <span data-ttu-id="1a5a1-166">Hello idő a működés közbeni időszak lejár, hello CDN ellenőrzi toosee e hello CDN-végpont továbbra is érvényes, és hello objektum névtelenül továbbra is elérhetők maradnak.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-166">When hello time-to-live period expires, hello CDN will check toosee whether hello CDN endpoint is still valid and hello object still anonymously accessible.</span></span> <span data-ttu-id="1a5a1-167">Ha nem, majd hello objektum rendszer már nem gyorsítótárazható.</span><span class="sxs-lookup"><span data-stu-id="1a5a1-167">If it is not, then hello object will no longer be cached.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1a5a1-168">További források</span><span class="sxs-lookup"><span data-stu-id="1a5a1-168">Additional resources</span></span>
* [<span data-ttu-id="1a5a1-169">Hogyan tooMap CDN tartalom tooa egyéni tartományhoz</span><span class="sxs-lookup"><span data-stu-id="1a5a1-169">How tooMap CDN Content tooa Custom Domain</span></span>](cdn-map-content-to-custom-domain.md)
* [<span data-ttu-id="1a5a1-170">Az egyéni tartomány HTTPS engedélyezése</span><span class="sxs-lookup"><span data-stu-id="1a5a1-170">Enable HTTPS for your custom domain</span></span>](cdn-custom-ssl.md)

[create-new-storage-account]: ./media/cdn-create-a-storage-account-with-cdn/CDN_CreateNewStorageAcct.png
[cdn-enable-navigation]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-new-endpoint-creation.png
[cdn-storage-new-endpoint]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-new-endpoint-list.png
[cdn-storage-cdn-configuration]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-endpoint-configuration.png 
