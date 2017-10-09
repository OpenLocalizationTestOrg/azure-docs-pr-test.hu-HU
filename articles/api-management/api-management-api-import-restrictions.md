---
title: "aaaRestrictions és ismert problémákat Azure API Management API importálása |} Microsoft Docs"
description: "Ismert problémák és korlátozások az Azure API Management használata hello nyílt API-t, a WSDL vagy a WADL formátumú importálási részleteit."
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
ms.openlocfilehash: 0bed5ace47de6ccbfbecba25ea6b69c5329de089
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="api-import-restrictions-and-known-issues"></a><span data-ttu-id="b75f6-103">API-importálási korlátozások és ismert problémák</span><span class="sxs-lookup"><span data-stu-id="b75f6-103">API import restrictions and known issues</span></span>
## <a name="about-this-list"></a><span data-ttu-id="b75f6-104">Ez a lista</span><span class="sxs-lookup"><span data-stu-id="b75f6-104">About this list</span></span>
<span data-ttu-id="b75f6-105">Miközben mindent történik, amely az API-t importálása az Azure API Management, zökkenőmentes tooensure és zökkenőmentes lehető, a Microsoft alkalmanként ugyanazok a korlátozások vagy toobe javítása sikeresen importálása előtt kell problémákat azonosítása.</span><span class="sxs-lookup"><span data-stu-id="b75f6-105">While every effort is made tooensure that importing your API into Azure API Management is as seamless and problem-free as possible, we do occasionally impose restrictions or identify issues that will need toobe rectified before you can successfully import.</span></span> <span data-ttu-id="b75f6-106">Ez a cikk dokumentálja ezek hello importálási formátuma hello API által szervezett.</span><span class="sxs-lookup"><span data-stu-id="b75f6-106">This article documents these, organised by hello import format of hello API.</span></span>

## <span data-ttu-id="b75f6-107"><a name="open-api"></a>Nyissa meg a Swagger/API</span><span class="sxs-lookup"><span data-stu-id="b75f6-107"><a name="open-api"> </a>Open API/Swagger</span></span>
<span data-ttu-id="b75f6-108">Általában a nyílt API-t dokumentum importálása hibák fordulnak elő, ha ellenőrizze, hogy ellenőrzése – hello-tervező használata a hello az új Azure Portal (Tervező - előtér - szerkesztő megnyitása a API Specification), vagy a és a 3. fél eszköz például <a href="http://www.swagger.io"> Swagger Editort</a>.</span><span class="sxs-lookup"><span data-stu-id="b75f6-108">In general, if you are receiving errors importing your Open API document, please ensure you have validated it - either using hello designer in hello new Azure Portal (Design - Front End - Open API Specification Editor), or with a 3rd party tool such as <a href="http://www.swagger.io">Swagger Editor</a>.</span></span>

* <span data-ttu-id="b75f6-109">**Állomásnév** a host name attribútum szükséges.</span><span class="sxs-lookup"><span data-stu-id="b75f6-109">**Host Name** we require a host name attribute.</span></span>
* <span data-ttu-id="b75f6-110">**Elérési út kiinduló** egy alap elérési útja attribútum szükséges.</span><span class="sxs-lookup"><span data-stu-id="b75f6-110">**Base Path** we require a base path attribute.</span></span>
* <span data-ttu-id="b75f6-111">**Sémák** kérjük, a rendszer tömb.</span><span class="sxs-lookup"><span data-stu-id="b75f6-111">**Schemes** we require a scheme array.</span></span> 

## <span data-ttu-id="b75f6-112"><a name="wsdl"></a>WSDL</span><span class="sxs-lookup"><span data-stu-id="b75f6-112"><a name="wsdl"> </a>WSDL</span></span>
<span data-ttu-id="b75f6-113">WSDL fájlok használt toogenerate SOAP áteresztő API-k, vagy a SOAP-REST API háttéralkalmazás hello szolgál.</span><span class="sxs-lookup"><span data-stu-id="b75f6-113">WSDL files are used toogenerate SOAP Pass-through APIs, or serve as hello backend of a SOAP-to-REST API.</span></span>

* <span data-ttu-id="b75f6-114">**WSDL** jelenleg nem támogatott API-k ezzel az attribútummal.</span><span class="sxs-lookup"><span data-stu-id="b75f6-114">**WSDL:Import** we do not currently support APIs using this attribute.</span></span> <span data-ttu-id="b75f6-115">Az ügyfelek kell importálni hello elem egyesítése egy dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="b75f6-115">Customers should merge hello imported elements into one document.</span></span>
* <span data-ttu-id="b75f6-116">**Több alkotórészek üzenetek** jelenleg nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="b75f6-116">**Messages with multiple parts** are currently not supported.</span></span>
* <span data-ttu-id="b75f6-117">**WCF wsHttpBinding** SOAP-szolgáltatások a Windows Communication Foundation létre kell használnia a basicHttpBinding - wsHttpBinding nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="b75f6-117">**WCF wsHttpBinding** SOAP services created with Windows Communication Foundation should use basicHttpBinding - wsHttpBinding is not supported.</span></span>
* <span data-ttu-id="b75f6-118">**Az MTOM** -szolgáltatások MTOM <em>előfordulhat, hogy</em> működik.</span><span class="sxs-lookup"><span data-stu-id="b75f6-118">**MTOM** Services using MTOM <em>may</em> work.</span></span> <span data-ttu-id="b75f6-119">Jelenleg nem érhető hivatalos támogatást.</span><span class="sxs-lookup"><span data-stu-id="b75f6-119">Official support is not offered at this time.</span></span>
* <span data-ttu-id="b75f6-120">**A rekurzió** , amelyek meghatározott rekurzív (pl. Lásd tooan tömbje magukat) használata nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="b75f6-120">**Recursion** types that are defined recursively (e.g. refer tooan array of themselves) are not supported.</span></span>

## <span data-ttu-id="b75f6-121"><a name="wadl"></a>WADL</span><span class="sxs-lookup"><span data-stu-id="b75f6-121"><a name="wadl"> </a>WADL</span></span>
<span data-ttu-id="b75f6-122">Jelenleg nincsenek ismert WADL importálási problémák.</span><span class="sxs-lookup"><span data-stu-id="b75f6-122">There are no known WADL import issues currently.</span></span>


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

[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How toocreate and publish a product]: api-management-howto-add-products.md
[How toocache operation results in Azure API Management]: api-management-howto-cache.md
