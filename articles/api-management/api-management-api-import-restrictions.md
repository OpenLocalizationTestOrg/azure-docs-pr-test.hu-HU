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
# <a name="api-import-restrictions-and-known-issues"></a>API-importálási korlátozások és ismert problémák
## <a name="about-this-list"></a>Ez a lista
Miközben mindent történik, amely az API-t importálása az Azure API Management, zökkenőmentes tooensure és zökkenőmentes lehető, a Microsoft alkalmanként ugyanazok a korlátozások vagy toobe javítása sikeresen importálása előtt kell problémákat azonosítása. Ez a cikk dokumentálja ezek hello importálási formátuma hello API által szervezett.

## <a name="open-api"></a>Nyissa meg a Swagger/API
Általában a nyílt API-t dokumentum importálása hibák fordulnak elő, ha ellenőrizze, hogy ellenőrzése – hello-tervező használata a hello az új Azure Portal (Tervező - előtér - szerkesztő megnyitása a API Specification), vagy a és a 3. fél eszköz például <a href="http://www.swagger.io"> Swagger Editort</a>.

* **Állomásnév** a host name attribútum szükséges.
* **Elérési út kiinduló** egy alap elérési útja attribútum szükséges.
* **Sémák** kérjük, a rendszer tömb. 

## <a name="wsdl"></a>WSDL
WSDL fájlok használt toogenerate SOAP áteresztő API-k, vagy a SOAP-REST API háttéralkalmazás hello szolgál.

* **WSDL** jelenleg nem támogatott API-k ezzel az attribútummal. Az ügyfelek kell importálni hello elem egyesítése egy dokumentumot.
* **Több alkotórészek üzenetek** jelenleg nem támogatottak.
* **WCF wsHttpBinding** SOAP-szolgáltatások a Windows Communication Foundation létre kell használnia a basicHttpBinding - wsHttpBinding nem támogatott.
* **Az MTOM** -szolgáltatások MTOM <em>előfordulhat, hogy</em> működik. Jelenleg nem érhető hivatalos támogatást.
* **A rekurzió** , amelyek meghatározott rekurzív (pl. Lásd tooan tömbje magukat) használata nem támogatott.

## <a name="wadl"></a>WADL
Jelenleg nincsenek ismert WADL importálási problémák.


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
