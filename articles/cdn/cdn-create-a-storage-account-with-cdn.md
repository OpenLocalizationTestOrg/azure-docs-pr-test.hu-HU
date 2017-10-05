---
title: "Azure-tárfiók integrálása az Azure CDN |} Microsoft Docs"
description: "Ismerje meg, hogyan használható az Azure tartalom Delivery Network (CDN) tartalmak nagy sávszélességű kézbesítéséhez az Azure Storage blobs gyorsítótárazása révén."
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
ms.openlocfilehash: 511076935d06ed0908341044e37069e74530be49
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="integrate-an-azure-storage-account-with-azure-cdn"></a><span data-ttu-id="bd13e-103">Azure-tárfiók integrálása az Azure CDN szolgáltatás használata</span><span class="sxs-lookup"><span data-stu-id="bd13e-103">Integrate an Azure storage account with Azure CDN</span></span>
<span data-ttu-id="bd13e-104">CDN az az Azure storage engedélyezhető a gyorsítótár teljes tartalmát.</span><span class="sxs-lookup"><span data-stu-id="bd13e-104">CDN can be enabled to cache content from your Azure storage.</span></span> <span data-ttu-id="bd13e-105">A fejlesztők a tartalmak nagy sávszélességű kézbesítéséhez a blobok és számítási példányokért fizikai csomópontokon az Egyesült Államok, Európa, Ázsia, Ausztrália és Dél-Amerika a statikus tartalom gyorsítótárazása révén globális megoldást kínál.</span><span class="sxs-lookup"><span data-stu-id="bd13e-105">It offers developers a global solution for delivering high-bandwidth content by caching blobs and static content of compute instances at physical nodes in the United States, Europe, Asia, Australia and South America.</span></span>

## <a name="step-1-create-a-storage-account"></a><span data-ttu-id="bd13e-106">1. lépés: Tárfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="bd13e-106">Step 1: Create a storage account</span></span>
<span data-ttu-id="bd13e-107">A következő eljárással hozzon létre egy új tárfiókot, Azure-előfizetéssel.</span><span class="sxs-lookup"><span data-stu-id="bd13e-107">Use the following procedure to create a new storage account for a Azure subscription.</span></span> <span data-ttu-id="bd13e-108">A storage-fiók az Azure storage-szolgáltatásokhoz való hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="bd13e-108">A storage account gives access to Azure storage services.</span></span> <span data-ttu-id="bd13e-109">A tárfiók eléréséhez szükséges az Azure storage szolgáltatás összetevői a névtér a legmagasabb szintű jelöli: Blob-szolgáltatások, Queue szolgáltatások és Table szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="bd13e-109">The storage account represents the highest level of the namespace for accessing each of the Azure storage service components: Blob services, Queue services, and Table services.</span></span> <span data-ttu-id="bd13e-110">További információkért tekintse meg a [Microsoft Azure Storage bemutatása](../storage/common/storage-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="bd13e-110">For more information, refer to the [Introduction to Microsoft Azure Storage](../storage/common/storage-introduction.md).</span></span>

<span data-ttu-id="bd13e-111">Hozzon létre egy tárfiókot, vagy a szolgáltatás rendszergazdájának vagy társadminisztrátorának a társított előfizetés kell lennie.</span><span class="sxs-lookup"><span data-stu-id="bd13e-111">To create a storage account, you must be either the service administrator or a co-administrator for the associated subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="bd13e-112">Többféleképpen hozzon létre egy tárfiókot, beleértve az Azure portál és a Powershell segítségével.</span><span class="sxs-lookup"><span data-stu-id="bd13e-112">There are several methods you can use to create a storage account, including the Azure Portal and Powershell.</span></span>  <span data-ttu-id="bd13e-113">Ebben az oktatóanyagban használni fogjuk az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="bd13e-113">For this tutorial, we'll be using the Azure Portal.</span></span>  
> 
> 

<span data-ttu-id="bd13e-114">**A storage-fiók egy Azure-előfizetés létrehozása**</span><span class="sxs-lookup"><span data-stu-id="bd13e-114">**To create a storage account for an Azure subscription**</span></span>

1. <span data-ttu-id="bd13e-115">Jelentkezzen be az [Azure portálra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="bd13e-115">Sign in to the [Azure Portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="bd13e-116">Válassza ki a bal felső sarokban, **új**.</span><span class="sxs-lookup"><span data-stu-id="bd13e-116">In the upper left corner, select **New**.</span></span> <span data-ttu-id="bd13e-117">Az a **új** párbeszédpanelen válassza **adatok + tárolás**, majd kattintson a **tárfiók**.</span><span class="sxs-lookup"><span data-stu-id="bd13e-117">In the **New** Dialog, select **Data  + Storage**, then click **Storage account**.</span></span>
    
    <span data-ttu-id="bd13e-118">A **storage-fiók létrehozása** panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="bd13e-118">The **Create storage account** blade appears.</span></span>   

    ![Storage-fiók létrehozása][create-new-storage-account]  

3. <span data-ttu-id="bd13e-120">Az a **neve** mezőbe írja be egy altartomány nevét.</span><span class="sxs-lookup"><span data-stu-id="bd13e-120">In the **Name** field, type a subdomain name.</span></span> <span data-ttu-id="bd13e-121">Ez a bejegyzés 3-24 kisbetűket és számokat tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="bd13e-121">This entry can contain 3-24 lowercase letters and numbers.</span></span>
   
    <span data-ttu-id="bd13e-122">Ez az érték lesz az állomásnév, az előfizetés Blob, sor vagy tábla erőforrásainak címzéséhez használt URI-Azonosítóra belül.</span><span class="sxs-lookup"><span data-stu-id="bd13e-122">This value becomes the host name within the URI that is used to address Blob, Queue, or Table resources for the subscription.</span></span> <span data-ttu-id="bd13e-123">Egy tároló-erőforrás a Blob szolgáltatás megoldására használhatja egy URI-t a következő formátumban, ahol  *&lt;StorageAccountLabel&gt;*  hivatkozik a beírt érték **adjon meg egy URL-cím**:</span><span class="sxs-lookup"><span data-stu-id="bd13e-123">To address a container resource in the Blob service, you would use a URI in the following format, where *&lt;StorageAccountLabel&gt;* refers to the value you typed in **Enter a URL**:</span></span>
   
    <span data-ttu-id="bd13e-124">http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mycontainer&gt;*</span><span class="sxs-lookup"><span data-stu-id="bd13e-124">http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mycontainer&gt;*</span></span>
   
    <span data-ttu-id="bd13e-125">**Fontos:** az URL-cím címke űrlapok a tárfiók URI altartomány, és az Azure-ban az összes üzemeltetett szolgáltatások egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="bd13e-125">**Important:** The URL label forms the subdomain of the storage  account URI and must be unique among all hosted services in  Azure.</span></span>
   
    <span data-ttu-id="bd13e-126">Ezt az értéket is használja a tárfiók a portálon, vagy neveként ezt a fiókot programozott módon való hozzáférés során.</span><span class="sxs-lookup"><span data-stu-id="bd13e-126">This value is also used as the name of this storage account in the portal, or when accessing this account programmatically.</span></span>
4. <span data-ttu-id="bd13e-127">Hagyja meg az alapértelmezett értéket **telepítési modell**, **fiók kind**, **teljesítmény**, és **replikációs**.</span><span class="sxs-lookup"><span data-stu-id="bd13e-127">Leave the defaults for **Deployment model**, **Account kind**, **Performance**, and **Replication**.</span></span> 
5. <span data-ttu-id="bd13e-128">Válassza ki a **előfizetés** , amely a tárolási fiók használandó.</span><span class="sxs-lookup"><span data-stu-id="bd13e-128">Select the **Subscription** that the storage account will be used with.</span></span>
6. <span data-ttu-id="bd13e-129">Válasszon ki vagy hozzon létre egy **erőforráscsoportot**.</span><span class="sxs-lookup"><span data-stu-id="bd13e-129">Select or create a **Resource Group**.</span></span>  <span data-ttu-id="bd13e-130">További információ az erőforráscsoportokkal kapcsolatban: [Az Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md#resource-groups).</span><span class="sxs-lookup"><span data-stu-id="bd13e-130">For more information on Resource Groups, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md#resource-groups).</span></span>
7. <span data-ttu-id="bd13e-131">Válasszon egy helyet a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="bd13e-131">Select a location for your storage account.</span></span>
8. <span data-ttu-id="bd13e-132">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="bd13e-132">Click **Create**.</span></span> <span data-ttu-id="bd13e-133">A storage-fiók létrehozása eltarthat néhány percet.</span><span class="sxs-lookup"><span data-stu-id="bd13e-133">The process of creating the storage account might take several minutes to complete.</span></span>

## <a name="step-2-enable-cdn-for-the-storage-account"></a><span data-ttu-id="bd13e-134">2. lépés: A tárfiók CDN engedélyezése</span><span class="sxs-lookup"><span data-stu-id="bd13e-134">Step 2: Enable CDN for the storage account</span></span>

<span data-ttu-id="bd13e-135">Az a legújabb integrációs most engedélyezheti a CDN a tárfiók anélkül, hogy a tároló portálbővítményt.</span><span class="sxs-lookup"><span data-stu-id="bd13e-135">With the newest integration, you can now enable CDN for your storage account without leaving your storage portal extension.</span></span> 

1. <span data-ttu-id="bd13e-136">Válassza ki a tárfiókot, keressen a "CDN" vagy a bal oldali navigációs menü görgessen lefelé, majd kattintson az "Azure CDN".</span><span class="sxs-lookup"><span data-stu-id="bd13e-136">Select the storage account, search "CDN" or scroll down from the left navigation menu, then click "Azure CDN".</span></span>
    
    <span data-ttu-id="bd13e-137">A **Azure CDN** panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="bd13e-137">The **Azure CDN** blade appears.</span></span>

    ![CDN engedélyezése navigációs][cdn-enable-navigation]
    
2. <span data-ttu-id="bd13e-139">Írja be a szükséges adatokat az új végpont létrehozásához</span><span class="sxs-lookup"><span data-stu-id="bd13e-139">Create a new endpoint by entering the required information</span></span>
    - <span data-ttu-id="bd13e-140">**CDN-profil**: hozzon létre egy új, vagy egy meglévő profil.</span><span class="sxs-lookup"><span data-stu-id="bd13e-140">**CDN Profile**: You can create a new or use an existing profile.</span></span>
    - <span data-ttu-id="bd13e-141">**IP-címek**: csak akkor kell új CDN-profil létrehozásakor válassza ki a tarifacsomagot.</span><span class="sxs-lookup"><span data-stu-id="bd13e-141">**Pricing tier**: You only need to select a pricing tier if you create a new CDN profile.</span></span>
    - <span data-ttu-id="bd13e-142">**CDN-végpont nevének**: Adja meg a választott / egy végpont nevét.</span><span class="sxs-lookup"><span data-stu-id="bd13e-142">**CDN endpoint name**: Enter an endpoint name per your choice.</span></span>

    > [!TIP]
    > <span data-ttu-id="bd13e-143">A létrehozott CDN-végpont használja az állomásnevet a tárfiók származási alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="bd13e-143">The created CDN endpoint uses the hostname of your storage account as origin by default.</span></span>

    <span data-ttu-id="bd13e-144">! [cdn új végpont létrehozásához] [a cdn-új-végpont-létrehozása]</span><span class="sxs-lookup"><span data-stu-id="bd13e-144">![cdn new endpoint creation][cdn-new-endpoint-creation]</span></span>

3. <span data-ttu-id="bd13e-145">A létrehozás után az új végpont a fenti végpont listában jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="bd13e-145">After creation, the new endpoint will show up in the endpoint list above.</span></span>

    ![tárolási új CDN-végpont][cdn-storage-new-endpoint]

> [!NOTE]
> <span data-ttu-id="bd13e-147">Azure CDN-bővítmény engedélyezése a CDN is választhatja. [Oktatóanyag](#Tutorial-cdn-create-profile).</span><span class="sxs-lookup"><span data-stu-id="bd13e-147">You can also go to Azure CDN extension to enable CDN.[Tutorial](#Tutorial-cdn-create-profile).</span></span>
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]  

## <a name="step-3-enable-additional-cdn-features"></a><span data-ttu-id="bd13e-148">3. lépés: További CDN-funkciók engedélyezése</span><span class="sxs-lookup"><span data-stu-id="bd13e-148">Step 3: Enable additional CDN features</span></span>

<span data-ttu-id="bd13e-149">A storage-fiók "Azure CDN" panelen kattintson a CDN-végpont a listából a CDN konfigurációs panel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="bd13e-149">From storage account "Azure CDN" blade, click the CDN endpoint from the list to open CDN configuration blade.</span></span> <span data-ttu-id="bd13e-150">További CDN szolgáltatásai engedélyezheti a kézbesítésre, például a tömörítés, lekérdezési karakterláncot, földrajzi szűrést.</span><span class="sxs-lookup"><span data-stu-id="bd13e-150">You can enable additional CDN features for your delivery, such as compression, query string, geo filtering.</span></span> <span data-ttu-id="bd13e-151">Adja hozzá az egyéni tartomány leképezése a CDN-végpont is, és az egyéni tartomány HTTPS engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="bd13e-151">You can also add custom domain mapping to your CDN endpoint and enable custom domain HTTPS.</span></span>
    
![CDN cdn tárolással][cdn-storage-cdn-configuration]

## <a name="step-4-access-cdn-content"></a><span data-ttu-id="bd13e-153">4. lépés: Hozzáférési CDN-tartalom</span><span class="sxs-lookup"><span data-stu-id="bd13e-153">Step 4: Access CDN content</span></span>
<span data-ttu-id="bd13e-154">A CDN a gyorsítótárazott tartalom eléréséhez a CDN a portálon megadott URL-CÍMÉT használja.</span><span class="sxs-lookup"><span data-stu-id="bd13e-154">To access cached content on the CDN, use the CDN URL provided in the portal.</span></span> <span data-ttu-id="bd13e-155">A gyorsítótárazott blob címet a következőhöz hasonló lesz:</span><span class="sxs-lookup"><span data-stu-id="bd13e-155">The address for a cached blob will be similar to the following:</span></span>

<span data-ttu-id="bd13e-156">http://<*EndpointName*\>.azureedge.net/ <*myPublicContainer*\>/<*Blobnév*\></span><span class="sxs-lookup"><span data-stu-id="bd13e-156">http://<*EndpointName*\>.azureedge.net/<*myPublicContainer*\>/<*BlobName*\></span></span>

> [!NOTE]
> <span data-ttu-id="bd13e-157">Ha engedélyezi a CDN hozzáférést egy tárfiókba, az összes nyilvánosan elérhető objektumok jogosultak a CDN peremhálózati gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="bd13e-157">Once you enable CDN access to a storage account, all publicly available objects are eligible for CDN edge caching.</span></span> <span data-ttu-id="bd13e-158">Ha módosítja a CDN jelenleg gyorsítótárazott objektumhoz, az új tartalom nem lesz elérhető a CDN mindaddig, amíg a CDN tartalmát frissíti, a gyorsítótárazott tartalom idő a működés közbeni időszak lejártával.</span><span class="sxs-lookup"><span data-stu-id="bd13e-158">If you modify an object that is currently cached in the CDN, the new content will not be available via the CDN until the CDN refreshes its content when the cached content time-to-live period expires.</span></span>
> 
> 

## <a name="step-5-remove-content-from-the-cdn"></a><span data-ttu-id="bd13e-159">5. lépés: A tartalom eltávolítása a CDN-t</span><span class="sxs-lookup"><span data-stu-id="bd13e-159">Step 5: Remove content from the CDN</span></span>
<span data-ttu-id="bd13e-160">Ha már nem szeretne gyorsítótárazása az objektum az az Azure Content Delivery Network (CDN), akkor is igénybe vehet az alábbi lépések egyikét:</span><span class="sxs-lookup"><span data-stu-id="bd13e-160">If you no longer wish to cache an object in the Azure Content Delivery Network (CDN), you can take one of the following steps:</span></span>

* <span data-ttu-id="bd13e-161">Biztosíthatja, hogy a tároló privát nyilvános helyett.</span><span class="sxs-lookup"><span data-stu-id="bd13e-161">You can make the container private instead of public.</span></span> <span data-ttu-id="bd13e-162">További információk: [Manage anonymous read access to containers and blobs](../storage/blobs/storage-manage-access-to-resources.md) (Tárolók és blobok névtelen olvasási hozzáférésének kezelése).</span><span class="sxs-lookup"><span data-stu-id="bd13e-162">See [Manage anonymous read access to containers and blobs](../storage/blobs/storage-manage-access-to-resources.md) for more information.</span></span>
* <span data-ttu-id="bd13e-163">Tiltsa le, vagy törölni a CDN-végpontot a felügyeleti portál használatával.</span><span class="sxs-lookup"><span data-stu-id="bd13e-163">You can disable or delete the CDN endpoint using the Management Portal.</span></span>
* <span data-ttu-id="bd13e-164">Az üzemeltetett szolgáltatás nem válaszol a kérelmekre a objektum módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="bd13e-164">You can modify your hosted service to no longer respond to requests for the object.</span></span>

<span data-ttu-id="bd13e-165">Az objektum már gyorsítótárazza a CDN gyorsítótárában marad, amíg az objektum idő a működés közbeni időszakának lejártáig támogatja, vagy a végpont véglegesen törlődnek.</span><span class="sxs-lookup"><span data-stu-id="bd13e-165">An object already cached in the CDN will remain cached until the time-to-live period for the object expires or until the endpoint is purged.</span></span> <span data-ttu-id="bd13e-166">Az idő a működés közbeni időszak lejártával a CDN ellenőrzi, hogy a CDN-végpont továbbra is érvényes, és az objektum névtelenül továbbra is elérhetők maradnak.</span><span class="sxs-lookup"><span data-stu-id="bd13e-166">When the time-to-live period expires, the CDN will check to see whether the CDN endpoint is still valid and the object still anonymously accessible.</span></span> <span data-ttu-id="bd13e-167">Ha nem, majd az objektum rendszer már nem gyorsítótárazható.</span><span class="sxs-lookup"><span data-stu-id="bd13e-167">If it is not, then the object will no longer be cached.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bd13e-168">További források</span><span class="sxs-lookup"><span data-stu-id="bd13e-168">Additional resources</span></span>
* [<span data-ttu-id="bd13e-169">CDN-tartalom leképezése egyéni tartományra</span><span class="sxs-lookup"><span data-stu-id="bd13e-169">How to Map CDN Content to a Custom Domain</span></span>](cdn-map-content-to-custom-domain.md)
* [<span data-ttu-id="bd13e-170">Az egyéni tartomány HTTPS engedélyezése</span><span class="sxs-lookup"><span data-stu-id="bd13e-170">Enable HTTPS for your custom domain</span></span>](cdn-custom-ssl.md)

[create-new-storage-account]: ./media/cdn-create-a-storage-account-with-cdn/CDN_CreateNewStorageAcct.png
[cdn-enable-navigation]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-new-endpoint-creation.png
[cdn-storage-new-endpoint]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-new-endpoint-list.png
[cdn-storage-cdn-configuration]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-endpoint-configuration.png 
