---
title: "Korlátozások és ismert problémákat az Azure API Management API importálási |} Microsoft Docs"
description: "Ismert problémák és korlátozások az Azure API Management használata a nyílt API-t, WSDL vagy WADL formátumú importálási részleteit."
services: api-management
documentationcenter: 
author: mattfarm
manager: vlvinogr
editor: 
ms.assetid: 7a5a63f0-3e72-49d3-a28c-1bb23ab495e2
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: apipm
ms.openlocfilehash: ac799d66b5038c207413086b0fa71239ff2a332f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="api-import-restrictions-and-known-issues"></a><span data-ttu-id="fb627-103">API-importálási korlátozások és ismert problémák</span><span class="sxs-lookup"><span data-stu-id="fb627-103">API import restrictions and known issues</span></span>
## <a name="about-this-list"></a><span data-ttu-id="fb627-104">Ez a lista</span><span class="sxs-lookup"><span data-stu-id="fb627-104">About this list</span></span>
<span data-ttu-id="fb627-105">Közben mindent történő esetén győződjön meg arról, hogy importálja az API-t Azure API Management, zökkenőmentes és zökkenőmentes lehetséges, a Microsoft alkalmanként ugyanazok a korlátozások vagy kapcsolatos problémákat jeleznek, amelyek sikeresen importálása előtt kell-e javítani kell.</span><span class="sxs-lookup"><span data-stu-id="fb627-105">While every effort is made to ensure that importing your API into Azure API Management is as seamless and problem-free as possible, we do occasionally impose restrictions or identify issues that will need to be rectified before you can successfully import.</span></span> <span data-ttu-id="fb627-106">Ez a cikk dokumentálja ezeket, az API formátuma által szervezett.</span><span class="sxs-lookup"><span data-stu-id="fb627-106">This article documents these, organised by the import format of the API.</span></span>

## <span data-ttu-id="fb627-107"><a name="open-api"></a>Nyissa meg a Swagger/API</span><span class="sxs-lookup"><span data-stu-id="fb627-107"><a name="open-api"> </a>Open API/Swagger</span></span>
<span data-ttu-id="fb627-108">Általában a nyílt API-t dokumentum importálása hibák fordulnak elő, ha győződjön meg arról ellenőrzése -, vagy a-tervező használata az új Azure-portálon (Tervező - előtér - API Specification szerkesztő megnyitása), vagy az egy 3. fél eszköz például <a href="http://www.swagger.io">Swagger Editor</a>.</span><span class="sxs-lookup"><span data-stu-id="fb627-108">In general, if you are receiving errors importing your Open API document, please ensure you have validated it - either using the designer in the new Azure Portal (Design - Front End - Open API Specification Editor), or with a 3rd party tool such as <a href="http://www.swagger.io">Swagger Editor</a>.</span></span>

* <span data-ttu-id="fb627-109">**Állomásnév** a host name attribútum szükséges.</span><span class="sxs-lookup"><span data-stu-id="fb627-109">**Host Name** we require a host name attribute.</span></span>
* <span data-ttu-id="fb627-110">**Elérési út kiinduló** egy alap elérési útja attribútum szükséges.</span><span class="sxs-lookup"><span data-stu-id="fb627-110">**Base Path** we require a base path attribute.</span></span>
* <span data-ttu-id="fb627-111">**Sémák** kérjük, a rendszer tömb.</span><span class="sxs-lookup"><span data-stu-id="fb627-111">**Schemes** we require a scheme array.</span></span> 

## <span data-ttu-id="fb627-112"><a name="wsdl"></a>WSDL</span><span class="sxs-lookup"><span data-stu-id="fb627-112"><a name="wsdl"> </a>WSDL</span></span>
<span data-ttu-id="fb627-113">SOAP áteresztő API-k létrehozása, vagy a SOAP-REST API háttéralkalmazás szolgálhat WSDL fájljait használja.</span><span class="sxs-lookup"><span data-stu-id="fb627-113">WSDL files are used to generate SOAP Pass-through APIs, or serve as the backend of a SOAP-to-REST API.</span></span>

* <span data-ttu-id="fb627-114">**WSDL** jelenleg nem támogatott API-k ezzel az attribútummal.</span><span class="sxs-lookup"><span data-stu-id="fb627-114">**WSDL:Import** we do not currently support APIs using this attribute.</span></span> <span data-ttu-id="fb627-115">Az ügyfelek kell az importált elem egyesítése egy dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="fb627-115">Customers should merge the imported elements into one document.</span></span>
* <span data-ttu-id="fb627-116">**Több alkotórészek üzenetek** jelenleg nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="fb627-116">**Messages with multiple parts** are currently not supported.</span></span>
* <span data-ttu-id="fb627-117">**WCF wsHttpBinding** SOAP-szolgáltatások a Windows Communication Foundation létre kell használnia a basicHttpBinding - wsHttpBinding nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="fb627-117">**WCF wsHttpBinding** SOAP services created with Windows Communication Foundation should use basicHttpBinding - wsHttpBinding is not supported.</span></span>
* <span data-ttu-id="fb627-118">**Az MTOM** -szolgáltatások MTOM <em>előfordulhat, hogy</em> működik.</span><span class="sxs-lookup"><span data-stu-id="fb627-118">**MTOM** Services using MTOM <em>may</em> work.</span></span> <span data-ttu-id="fb627-119">Jelenleg nem érhető hivatalos támogatást.</span><span class="sxs-lookup"><span data-stu-id="fb627-119">Official support is not offered at this time.</span></span>
* <span data-ttu-id="fb627-120">**A rekurzió** , amelyek meghatározott rekurzív (pl. Lásd tömbje magukat) használata nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="fb627-120">**Recursion** types that are defined recursively (e.g. refer to an array of themselves) are not supported.</span></span>

## <span data-ttu-id="fb627-121"><a name="wadl"></a>WADL</span><span class="sxs-lookup"><span data-stu-id="fb627-121"><a name="wadl"> </a>WADL</span></span>
<span data-ttu-id="fb627-122">Jelenleg nincsenek ismert WADL importálási problémák.</span><span class="sxs-lookup"><span data-stu-id="fb627-122">There are no known WADL import issues currently.</span></span>


[api-management-management-console]: ./media/api-management-howto-add-operations/api-management-management-console.png
[api-management-operations]: ./media/api-management-howto-add-operations/api-management-operations.png
[api-management-add-operation]: ./media/api-management-howto-add-operations/api-management-add-operation.png
[api-management-http-method]: ./media/api-management-howto-add-operations/api-management-http-method.png
[api-management-url-template]: ./media/api-management-howto-add-operations/api-management-url-template.png
[api-management-url-template-rewrite]: ./media/api-management-howto-add-operations/api-management-url-template-rewrite.png
[api-management-description]: ./media/api-management-howto-add-operations/api-management-description.png
[api-management-caching-tab]: ./media/api-management-howto-add-operations/api-management-caching-tab.png
[api-management-request-parameters]: ./media/api-management-howto-add-operations/api-management-request-parameters.png
[api-management-request-body]: ./media/api-management-howto-add-operations/api-management-request-body.png
[api-management-response-code]: ./media/api-management-howto-add-operations/api-management-response-code.png
[api-management-response-body-content-type]: ./media/api-management-howto-add-operations/api-management-response-body-content-type.png
[api-management-response-body]: ./media/api-management-howto-add-operations/api-management-response-body.png


[api-management-contoso-api]: ./media/api-management-howto-add-operations/api-management-contoso-api.png

[api-management-add-new-api]: ./media/api-management-howto-add-operations/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-add-operations/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-add-operations/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-add-operations/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-add-operations/api-management-echo-operations.png

[Add an operation]: #add-operation
[Operation caching]: #operation-caching
[Request parameters]: #request-parameters
[Request body]: #request-body
[Responses]: #responses
[Next steps]: #next-steps

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[How to cache operation results in Azure API Management]: api-management-howto-cache.md
