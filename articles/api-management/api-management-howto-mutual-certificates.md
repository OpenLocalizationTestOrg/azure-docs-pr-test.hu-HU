---
title: "ügyféltanúsítvány-alapú hitelesítés - Azure API Management használata aaaSecure háttér-szolgáltatásaihoz |} Microsoft Docs"
description: "Ismerje meg, hogy a toosecure háttér-szolgáltatásaihoz ügyfél használatával hogyan Tanúsítványalapú hitelesítés az Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 43453331-39b2-4672-80b8-0a87e4fde3c6
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 565bb61044fed1158944202c36e8abe30edf5729
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosecure-back-end-services-using-client-certificate-authentication-in-azure-api-management"></a><span data-ttu-id="ff223-103">Hogyan tanúsítvány toosecure háttér-szolgáltatásaihoz ügyfél-hitelesítés az Azure API Management</span><span class="sxs-lookup"><span data-stu-id="ff223-103">How toosecure back-end services using client certificate authentication in Azure API Management</span></span>
<span data-ttu-id="ff223-104">Az API Management biztosít az ügyféltanúsítványok hello funkció toosecure toohello háttér-szolgáltatás, az API-k.</span><span class="sxs-lookup"><span data-stu-id="ff223-104">API Management provides hello capability toosecure access toohello back-end service of an API using client certificates.</span></span> <span data-ttu-id="ff223-105">Ez az útmutató bemutatja, hogyan toomanage tanúsítványok hello API publisher portálon, és hogyan tooconfigure az API-k toouse egy tanúsítvány tooaccess a háttér-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="ff223-105">This guide shows how toomanage certificates in hello API publisher portal, and how tooconfigure an API toouse a certificate tooaccess its back-end service.</span></span>

<span data-ttu-id="ff223-106">Tanúsítványok hello API Management REST API használatával történő kezelésével kapcsolatos információkért lásd: [Azure API Management REST API tanúsítvány entitás][Azure API Management REST API Certificate entity].</span><span class="sxs-lookup"><span data-stu-id="ff223-106">For information about managing certificates using hello API Management REST API, see [Azure API Management REST API Certificate entity][Azure API Management REST API Certificate entity].</span></span>

## <span data-ttu-id="ff223-107"><a name="prerequisites"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ff223-107"><a name="prerequisites"> </a>Prerequisites</span></span>
<span data-ttu-id="ff223-108">Ez az útmutató bemutatja, hogyan tooconfigure az API Management szolgáltatás példány toouse ügyfél tanúsítvány hitelesítési tooaccess hello háttér-szolgáltatást egy API-t.</span><span class="sxs-lookup"><span data-stu-id="ff223-108">This guide shows you how tooconfigure your API Management service instance toouse client certificate authentication tooaccess hello back-end service for an API.</span></span> <span data-ttu-id="ff223-109">Következő hello ebben a témakörben ismertetett visszaállítási lépésekkel, mielőtt a háttér-szolgáltatás ügyféltanúsítvány-alapú hitelesítés beállítása rendelkeznie kell ([toothis cikk tekintse meg a hitelesítés az Azure Websitesra tooconfigure tanúsítvány] [ tooconfigure certificate authentication in Azure WebSites refer toothis article]), és hozzáférést toohello tanúsítvány és hello hello-tanúsítvány feltöltése hello API Management publisher Portal jelszó.</span><span class="sxs-lookup"><span data-stu-id="ff223-109">Before following hello steps in this topic, you should have your back-end service configured for client certificate authentication ([tooconfigure certificate authentication in Azure WebSites refer toothis article][tooconfigure certificate authentication in Azure WebSites refer toothis article]), and have access toohello certificate and hello password for hello certificate for uploading in hello API Management publisher portal.</span></span>

## <span data-ttu-id="ff223-110"><a name="step1"></a>Egy ügyfél-tanúsítvány feltöltése</span><span class="sxs-lookup"><span data-stu-id="ff223-110"><a name="step1"> </a>Upload a client certificate</span></span>
<span data-ttu-id="ff223-111">tooget elindítani, kattintson a **Publisher portal** a hello Azure portál, az API Management szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="ff223-111">tooget started, click **Publisher portal** in hello Azure Portal for your API Management service.</span></span> <span data-ttu-id="ff223-112">Ezzel megnyitná toohello API Management publisher portálon.</span><span class="sxs-lookup"><span data-stu-id="ff223-112">This takes you toohello API Management publisher portal.</span></span>

![API-közzétevő portál][api-management-management-console]

> <span data-ttu-id="ff223-114">Ha még nem hozott létre az API Management szolgáltatáspéldány, lásd: [hozzon létre egy API-kezelés szolgáltatás példányt] [ Create an API Management service instance] a hello [Ismerkedés az Azure API Management] [ Get started with Azure API Management] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="ff223-114">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="ff223-115">Kattintson a **biztonsági** a hello **API Management** hello balra, majd kattintson a menü **ügyféltanúsítványok**.</span><span class="sxs-lookup"><span data-stu-id="ff223-115">Click **Security** from hello **API Management** menu on hello left, and click **Client certificates**.</span></span>

![Ügyféltanúsítványok][api-management-security-client-certificates]

<span data-ttu-id="ff223-117">egy új tanúsítvány tooupload kattintson **feltöltés tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="ff223-117">tooupload a new certificate, click **Upload certificate**.</span></span>

![Tanúsítvány feltöltése][api-management-upload-certificate]

<span data-ttu-id="ff223-119">Keresse meg a tooyour tanúsítványt, és adja meg hello jelszó hello tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="ff223-119">Browse tooyour certificate, and then enter hello password for hello certificate.</span></span>

> <span data-ttu-id="ff223-120">hello tanúsítványnak kell lennie a **.pfx** formátumban.</span><span class="sxs-lookup"><span data-stu-id="ff223-120">hello certificate must be in **.pfx** format.</span></span> <span data-ttu-id="ff223-121">Önaláírt tanúsítványok használata engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="ff223-121">Self-signed certificates are allowed.</span></span>
> 
> 

![Tanúsítvány feltöltése][api-management-upload-certificate-form]

<span data-ttu-id="ff223-123">Kattintson a **feltöltése** tooupload hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="ff223-123">Click **Upload** tooupload hello certificate.</span></span>

> <span data-ttu-id="ff223-124">hello tanúsítvány jelszava most van hitelesítve.</span><span class="sxs-lookup"><span data-stu-id="ff223-124">hello certificate password is validated at this time.</span></span> <span data-ttu-id="ff223-125">Ha az nem megfelelő egy hibaüzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="ff223-125">If it is incorrect an error message is displayed.</span></span>
> 
> 

![A tanúsítvány feltöltése][api-management-certificate-uploaded]

<span data-ttu-id="ff223-127">Hello tanúsítványt a feltöltést követően megjelenő hello **ügyféltanúsítványok** fülre. Ha több tanúsítvány, jegyezze fel a hello tulajdonos, vagy utolsó négy számjegyét hello ujjlenyomat, amelyek használt tooselect hello tanúsítvány konfigurálása az API toouse tanúsítványokat, amikor hello alábbi szabályozott módon hello [konfigurálása egy API toouse ügyféltanúsítványt az átjáró hitelesítési] [ Configure an API toouse a client certificate for gateway authentication] szakasz.</span><span class="sxs-lookup"><span data-stu-id="ff223-127">Once hello certificate is uploaded, it appears on hello **Client certificates** tab. If you have multiple certificates, make a note of hello subject, or hello last four characters of hello thumbprint, which are used tooselect hello certificate when configuring an API toouse certificates, as covered in hello following [Configure an API toouse a client certificate for gateway authentication][Configure an API toouse a client certificate for gateway authentication] section.</span></span>

> <span data-ttu-id="ff223-128">tanúsítványlánc érvényesítése, például egy önaláírt tanúsítvány használata esetén ki tooturn lépésekkel hello Ez a GYIK ismertetett [elem](api-management-faq.md#can-i-use-a-self-signed-ssl-certificate-for-a-back-end).</span><span class="sxs-lookup"><span data-stu-id="ff223-128">tooturn off certificate chain validation when using, for example, a self-signed certificate, follow hello steps described in this FAQ [item](api-management-faq.md#can-i-use-a-self-signed-ssl-certificate-for-a-back-end).</span></span>
> 
> 

## <span data-ttu-id="ff223-129"><a name="step1a"></a>Egy ügyfél-tanúsítvány törlése</span><span class="sxs-lookup"><span data-stu-id="ff223-129"><a name="step1a"> </a>Delete a client certificate</span></span>
<span data-ttu-id="ff223-130">toodelete egy tanúsítványt, kattintson a **törlése** hello kívánt tanúsítvány mellett.</span><span class="sxs-lookup"><span data-stu-id="ff223-130">toodelete a certificate, click **Delete** beside hello desired certificate.</span></span>

![Tanúsítvány törlése][api-management-certificate-delete]

<span data-ttu-id="ff223-132">Kattintson a **Igen, törölje azt** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="ff223-132">Click **Yes, delete it** tooconfirm.</span></span>

![Törlés megerősítése][api-management-confirm-delete]

<span data-ttu-id="ff223-134">Ha hello tanúsítvány egy API-t használja, majd egy figyelmeztetés képernyő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="ff223-134">If hello certificate is in use by an API, then a warning screen is displayed.</span></span> <span data-ttu-id="ff223-135">toodelete hello tanúsítvány hello távolítsa el a tanúsítvány minden API-, amelyek a konfigurált toouse azt.</span><span class="sxs-lookup"><span data-stu-id="ff223-135">toodelete hello certificate you must first remove hello certificate from any APIs that are configured toouse it.</span></span>

![Törlés megerősítése][api-management-confirm-delete-policy]

## <span data-ttu-id="ff223-137"><a name="step2"></a>Egy API toouse ügyféltanúsítványt az átjáró hitelesítés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ff223-137"><a name="step2"> </a>Configure an API toouse a client certificate for gateway authentication</span></span>
<span data-ttu-id="ff223-138">Kattintson a **API-k** a hello **API Management** hello menüjének balra kattintson a kívánt hello API hello neve, és hello kattintson **biztonsági** fülre.</span><span class="sxs-lookup"><span data-stu-id="ff223-138">Click **APIs** from hello **API Management** menu on hello left, click hello name of hello desired API, and click hello **Security** tab.</span></span>

![API-biztonsági][api-management-api-security]

<span data-ttu-id="ff223-140">Válassza ki **ügyféltanúsítványok** a hello **hitelesítő adatokkal rendelkező** legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="ff223-140">Select **Client certificates** from hello **With credentials** drop-down list.</span></span>

![Ügyféltanúsítványok][api-management-mutual-certificates]

<span data-ttu-id="ff223-142">Válassza ki a kívánt tanúsítványt hello hello **ügyféltanúsítvány** legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="ff223-142">Select hello desired certificate from hello **Client certificate** drop-down list.</span></span> <span data-ttu-id="ff223-143">Ha több tanúsítvány hello tulajdonos tekintse meg, vagy hello utolsó négy számjegyét hello ujjlenyomat hello előző szakasz toodetermine hello megfelelő tanúsítvány leírtaknak megfelelően.</span><span class="sxs-lookup"><span data-stu-id="ff223-143">If there are multiple certificates you can look at hello subject or hello last four characters of hello thumbprint as noted in hello previous section toodetermine hello correct certificate.</span></span>

![Tanúsítvány kiválasztása][api-management-select-certificate]

<span data-ttu-id="ff223-145">Kattintson a **mentése** toosave hello konfigurációs módosítás toohello API.</span><span class="sxs-lookup"><span data-stu-id="ff223-145">Click **Save** toosave hello configuration change toohello API.</span></span>

> <span data-ttu-id="ff223-146">Ez a változás a következő időponttól érvényes azonnal, és használja az adott API toooperations hívások hello tanúsítvány tooauthenticate hello háttér-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="ff223-146">This change is effective immediately, and calls toooperations of that API will use hello certificate tooauthenticate on hello back-end server.</span></span>
> 
> 

![API-módosítások mentése][api-management-save-api]

> <span data-ttu-id="ff223-148">Egy tanúsítványt egy API-t az átjáró hello háttér-szolgáltatás hitelesítésének megadása esetén hello házirend részévé válik, hogy az API-hoz, és megtekinthetők az hello Helyicsoportházirend-szerkesztő.</span><span class="sxs-lookup"><span data-stu-id="ff223-148">When a certificate is specified for gateway authentication for hello back-end service of an API, it becomes part of hello policy for that API, and can be viewed in hello policy editor.</span></span>
> 
> 

![Tanúsítványszabályzat][api-management-certificate-policy]

## <a name="next-steps"></a><span data-ttu-id="ff223-150">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ff223-150">Next steps</span></span>
<span data-ttu-id="ff223-151">A más módon toosecure HTTP alap vagy megosztott titkos hitelesítést, például a háttérszolgáltatás című cikkben olvashat bővebben a következő videó hello.</span><span class="sxs-lookup"><span data-stu-id="ff223-151">For more information on other ways toosecure your backend service, such as HTTP basic or shared secret authentication, see hello following video.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Last-mile-Security/player]
> 
> 

[api-management-management-console]: ./media/api-management-howto-mutual-certificates/api-management-management-console.png
[api-management-security-client-certificates]: ./media/api-management-howto-mutual-certificates/api-management-security-client-certificates.png
[api-management-upload-certificate]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate.png
[api-management-upload-certificate-form]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate-form.png
[api-management-certificate-uploaded]: ./media/api-management-howto-mutual-certificates/api-management-certificate-uploaded.png
[api-management-api-security]: ./media/api-management-howto-mutual-certificates/api-management-api-security.png
[api-management-mutual-certificates]: ./media/api-management-howto-mutual-certificates/api-management-mutual-certificates.png
[api-management-select-certificate]: ./media/api-management-howto-mutual-certificates/api-management-select-certificate.png
[api-management-save-api]: ./media/api-management-howto-mutual-certificates/api-management-save-api.png
[api-management-certificate-policy]: ./media/api-management-howto-mutual-certificates/api-management-certificate-policy.png
[api-management-certificate-delete]: ./media/api-management-howto-mutual-certificates/api-management-certificate-delete.png
[api-management-confirm-delete]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete.png
[api-management-confirm-delete-policy]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete-policy.png



[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How tooadd and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs tooa product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[Azure API Management REST API Certificate entity]: http://msdn.microsoft.com/library/azure/dn783483.aspx
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[tooconfigure certificate authentication in Azure WebSites refer toothis article]: https://azure.microsoft.com/en-us/documentation/articles/app-service-web-configure-tls-mutual-auth/

[Prerequisites]: #prerequisites
[Upload a client certificate]: #step1
[Delete a client certificate]: #step1a
[Configure an API toouse a client certificate for gateway authentication]: #step2
[Test hello configuration by calling an operation in hello Developer Portal]: #step3
[Next steps]: #next-steps



