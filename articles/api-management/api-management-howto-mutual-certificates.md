---
title: "Háttér-szolgáltatásaihoz ügyfél használatával biztonságos tanúsítványhitelesítés - Azure API Management |} Microsoft Docs"
description: "Megtudhatja, mennyire biztonságos a háttér-szolgáltatásaihoz ügyféltanúsítvány-alapú hitelesítés használata az Azure API Management."
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
ms.openlocfilehash: 2ebe71c96fd9076a48f689041634dbd23d3d8414
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-secure-back-end-services-using-client-certificate-authentication-in-azure-api-management"></a><span data-ttu-id="35e4d-103">Az Azure API Management tanúsítványhitelesítés biztonságossá tétele a háttér-szolgáltatásaihoz ügyfél használatával</span><span class="sxs-lookup"><span data-stu-id="35e4d-103">How to secure back-end services using client certificate authentication in Azure API Management</span></span>
<span data-ttu-id="35e4d-104">API-kezelés lehetővé teszi a biztonságos hozzáférés a háttér-szolgáltatásra, az API-k az ügyféltanúsítványok.</span><span class="sxs-lookup"><span data-stu-id="35e4d-104">API Management provides the capability to secure access to the back-end service of an API using client certificates.</span></span> <span data-ttu-id="35e4d-105">Ez az útmutató bemutatja az API-közzétevő portálon tanúsítványok kezelése és konfigurálása egy API-t egy tanúsítványt a háttér-szolgáltatás elérésére használhat.</span><span class="sxs-lookup"><span data-stu-id="35e4d-105">This guide shows how to manage certificates in the API publisher portal, and how to configure an API to use a certificate to access its back-end service.</span></span>

<span data-ttu-id="35e4d-106">Tanúsítványok, az API Management REST API használatával történő kezelésével kapcsolatos információkért lásd: [Azure API Management REST API tanúsítvány entitás][Azure API Management REST API Certificate entity].</span><span class="sxs-lookup"><span data-stu-id="35e4d-106">For information about managing certificates using the API Management REST API, see [Azure API Management REST API Certificate entity][Azure API Management REST API Certificate entity].</span></span>

## <span data-ttu-id="35e4d-107"><a name="prerequisites"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="35e4d-107"><a name="prerequisites"> </a>Prerequisites</span></span>
<span data-ttu-id="35e4d-108">Ez az útmutató bemutatja, hogyan konfigurálhatja az API Management szolgáltatáspéldány ügyféltanúsítvány-alapú hitelesítés használatát a háttér-szolgáltatás számára az API-k eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="35e4d-108">This guide shows you how to configure your API Management service instance to use client certificate authentication to access the back-end service for an API.</span></span> <span data-ttu-id="35e4d-109">Ez a témakör a lépések végrehajtása előtt rendelkeznie kell a háttér-szolgáltatás ügyféltanúsítvány-alapú hitelesítés beállítása ([konfigurálása az Azure Websitesra tanúsítványhitelesítés tekintse meg a cikk][to configure certificate authentication in Azure WebSites refer to this article]), és a tanúsítvány feltöltése az API Management publisher portálon a tanúsítvány és a jelszó hozzáféréssel rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="35e4d-109">Before following the steps in this topic, you should have your back-end service configured for client certificate authentication ([to configure certificate authentication in Azure WebSites refer to this article][to configure certificate authentication in Azure WebSites refer to this article]), and have access to the certificate and the password for the certificate for uploading in the API Management publisher portal.</span></span>

## <span data-ttu-id="35e4d-110"><a name="step1"></a>Egy ügyfél-tanúsítvány feltöltése</span><span class="sxs-lookup"><span data-stu-id="35e4d-110"><a name="step1"> </a>Upload a client certificate</span></span>
<span data-ttu-id="35e4d-111">Első lépésként kattintson a **Közzétevő portál** elemre az API Management szolgáltatás Azure Portalján.</span><span class="sxs-lookup"><span data-stu-id="35e4d-111">To get started, click **Publisher portal** in the Azure Portal for your API Management service.</span></span> <span data-ttu-id="35e4d-112">Ezzel továbblép az API Management közzétevő portáljára.</span><span class="sxs-lookup"><span data-stu-id="35e4d-112">This takes you to the API Management publisher portal.</span></span>

![API-közzétevő portál][api-management-management-console]

> <span data-ttu-id="35e4d-114">Ha még nem hozott létre API Management szolgáltatáspéldányt, tekintse meg az [Ismerkedés az Azure API Management szolgáltatással][Get started with Azure API Management] oktatóanyag [API Management szolgáltatáspéldány létrehozása][Create an API Management service instance] című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="35e4d-114">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="35e4d-115">Kattintson a **biztonsági** a a **API Management** menüt, és kattintson **ügyféltanúsítványok**.</span><span class="sxs-lookup"><span data-stu-id="35e4d-115">Click **Security** from the **API Management** menu on the left, and click **Client certificates**.</span></span>

![Ügyféltanúsítványok][api-management-security-client-certificates]

<span data-ttu-id="35e4d-117">Töltsön fel új tanúsítványt, kattintson a **feltöltés tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="35e4d-117">To upload a new certificate, click **Upload certificate**.</span></span>

![Tanúsítvány feltöltése][api-management-upload-certificate]

<span data-ttu-id="35e4d-119">Keresse meg a tanúsítványt, és írja be a jelszót a tanúsítványhoz.</span><span class="sxs-lookup"><span data-stu-id="35e4d-119">Browse to your certificate, and then enter the password for the certificate.</span></span>

> <span data-ttu-id="35e4d-120">A tanúsítványnak kell lennie a **.pfx** formátumban.</span><span class="sxs-lookup"><span data-stu-id="35e4d-120">The certificate must be in **.pfx** format.</span></span> <span data-ttu-id="35e4d-121">Önaláírt tanúsítványok használata engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="35e4d-121">Self-signed certificates are allowed.</span></span>
> 
> 

![Tanúsítvány feltöltése][api-management-upload-certificate-form]

<span data-ttu-id="35e4d-123">Kattintson a **feltöltése** töltse fel a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="35e4d-123">Click **Upload** to upload the certificate.</span></span>

> <span data-ttu-id="35e4d-124">A tanúsítvány jelszava most van hitelesítve.</span><span class="sxs-lookup"><span data-stu-id="35e4d-124">The certificate password is validated at this time.</span></span> <span data-ttu-id="35e4d-125">Ha az nem megfelelő egy hibaüzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="35e4d-125">If it is incorrect an error message is displayed.</span></span>
> 
> 

![A tanúsítvány feltöltése][api-management-certificate-uploaded]

<span data-ttu-id="35e4d-127">A tanúsítvány a feltöltést követően megjelenik az **ügyféltanúsítványok** fülre.</span><span class="sxs-lookup"><span data-stu-id="35e4d-127">Once the certificate is uploaded, it appears on the **Client certificates** tab.</span></span> <span data-ttu-id="35e4d-128">Ha több tanúsítvány, akkor a tulajdonos jegyezze fel, vagy az ujjlenyomat az utolsó négy számjegyét, válassza ki a tanúsítványt egy API-t a tanúsítványok, konfigurálásakor, az alábbi szabályozott módon használt [átjáró hitelesítéshez használandó ügyféltanúsítványt API konfigurálása] [ Configure an API to use a client certificate for gateway authentication] szakasz.</span><span class="sxs-lookup"><span data-stu-id="35e4d-128">If you have multiple certificates, make a note of the subject, or the last four characters of the thumbprint, which are used to select the certificate when configuring an API to use certificates, as covered in the following [Configure an API to use a client certificate for gateway authentication][Configure an API to use a client certificate for gateway authentication] section.</span></span>

> <span data-ttu-id="35e4d-129">Kapcsolja ki a tanúsítványlánc érvényesítése használata esetén például egy önaláírt tanúsítványt, kövesse a lépéseket, ez a GYIK ismertetett [elem](api-management-faq.md#can-i-use-a-self-signed-ssl-certificate-for-a-back-end).</span><span class="sxs-lookup"><span data-stu-id="35e4d-129">To turn off certificate chain validation when using, for example, a self-signed certificate, follow the steps described in this FAQ [item](api-management-faq.md#can-i-use-a-self-signed-ssl-certificate-for-a-back-end).</span></span>
> 
> 

## <span data-ttu-id="35e4d-130"><a name="step1a"></a>Egy ügyfél-tanúsítvány törlése</span><span class="sxs-lookup"><span data-stu-id="35e4d-130"><a name="step1a"> </a>Delete a client certificate</span></span>
<span data-ttu-id="35e4d-131">Törli a tanúsítványt, kattintson a **törlése** mellett a kívánt tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="35e4d-131">To delete a certificate, click **Delete** beside the desired certificate.</span></span>

![Tanúsítvány törlése][api-management-certificate-delete]

<span data-ttu-id="35e4d-133">Kattintson a **Igen, törölje azt** megerősítéséhez.</span><span class="sxs-lookup"><span data-stu-id="35e4d-133">Click **Yes, delete it** to confirm.</span></span>

![Törlés megerősítése][api-management-confirm-delete]

<span data-ttu-id="35e4d-135">Ha a tanúsítvány egy API-t használja, majd egy figyelmeztetés képernyő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="35e4d-135">If the certificate is in use by an API, then a warning screen is displayed.</span></span> <span data-ttu-id="35e4d-136">A tanúsítvány törlése el kell távolítani a tanúsítványt bármely olyan API-, a használatára konfigurált.</span><span class="sxs-lookup"><span data-stu-id="35e4d-136">To delete the certificate you must first remove the certificate from any APIs that are configured to use it.</span></span>

![Törlés megerősítése][api-management-confirm-delete-policy]

## <span data-ttu-id="35e4d-138"><a name="step2"></a>Átjáró hitelesítéshez használandó ügyféltanúsítványt API konfigurálása</span><span class="sxs-lookup"><span data-stu-id="35e4d-138"><a name="step2"> </a>Configure an API to use a client certificate for gateway authentication</span></span>
<span data-ttu-id="35e4d-139">Kattintson a **API-k** a a **API Management** a bal oldali menüben kattintson a kívánt API nevére, majd kattintson a **biztonsági** fülre.</span><span class="sxs-lookup"><span data-stu-id="35e4d-139">Click **APIs** from the **API Management** menu on the left, click the name of the desired API, and click the **Security** tab.</span></span>

![API-biztonsági][api-management-api-security]

<span data-ttu-id="35e4d-141">Válassza ki **ügyféltanúsítványok** a a **hitelesítő adatokkal rendelkező** legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="35e4d-141">Select **Client certificates** from the **With credentials** drop-down list.</span></span>

![Ügyféltanúsítványok][api-management-mutual-certificates]

<span data-ttu-id="35e4d-143">A kívánt tanúsítvány kiválasztása a **ügyféltanúsítvány** legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="35e4d-143">Select the desired certificate from the **Client certificate** drop-down list.</span></span> <span data-ttu-id="35e4d-144">Ha több tanúsítvány vessen egy pillantást a megfelelő tanúsítványt a tulajdonos vagy az ujjlenyomat az előző szakaszban leírtaknak megfelelően utolsó négy számjegyét.</span><span class="sxs-lookup"><span data-stu-id="35e4d-144">If there are multiple certificates you can look at the subject or the last four characters of the thumbprint as noted in the previous section to determine the correct certificate.</span></span>

![Tanúsítvány kiválasztása][api-management-select-certificate]

<span data-ttu-id="35e4d-146">Kattintson a **mentése** menteni a konfiguráció módosítását az API-hoz.</span><span class="sxs-lookup"><span data-stu-id="35e4d-146">Click **Save** to save the configuration change to the API.</span></span>

> <span data-ttu-id="35e4d-147">Ez a változás a következő időponttól érvényes azonnal, és az adott API műveletek hívások a tanúsítványt használni kívánt telefonszámot a háttér-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="35e4d-147">This change is effective immediately, and calls to operations of that API will use the certificate to authenticate on the back-end server.</span></span>
> 
> 

![API-módosítások mentése][api-management-save-api]

> <span data-ttu-id="35e4d-149">Egy tanúsítványt egy API-t az átjáró a háttér-szolgáltatás hitelesítéséhez megadása esetén a szabályzat részévé válik, hogy az API-hoz, és tekintheti meg a Helyicsoportházirend-szerkesztő.</span><span class="sxs-lookup"><span data-stu-id="35e4d-149">When a certificate is specified for gateway authentication for the back-end service of an API, it becomes part of the policy for that API, and can be viewed in the policy editor.</span></span>
> 
> 

![Tanúsítványszabályzat][api-management-certificate-policy]

## <a name="next-steps"></a><span data-ttu-id="35e4d-151">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="35e4d-151">Next steps</span></span>
<span data-ttu-id="35e4d-152">Biztonságossá tehető a háttérszolgáltatás, például HTTP alap vagy megosztott titkos hitelesítés, a további információk: a következő videó.</span><span class="sxs-lookup"><span data-stu-id="35e4d-152">For more information on other ways to secure your backend service, such as HTTP basic or shared secret authentication, see the following video.</span></span>

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



[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[Azure API Management REST API Certificate entity]: http://msdn.microsoft.com/library/azure/dn783483.aspx
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[to configure certificate authentication in Azure WebSites refer to this article]: https://azure.microsoft.com/en-us/documentation/articles/app-service-web-configure-tls-mutual-auth/

[Prerequisites]: #prerequisites
[Upload a client certificate]: #step1
[Delete a client certificate]: #step1a
[Configure an API to use a client certificate for gateway authentication]: #step2
[Test the configuration by calling an operation in the Developer Portal]: #step3
[Next steps]: #next-steps



