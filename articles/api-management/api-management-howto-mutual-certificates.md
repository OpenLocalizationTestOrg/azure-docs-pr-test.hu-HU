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
# <a name="how-toosecure-back-end-services-using-client-certificate-authentication-in-azure-api-management"></a>Hogyan tanúsítvány toosecure háttér-szolgáltatásaihoz ügyfél-hitelesítés az Azure API Management
Az API Management biztosít az ügyféltanúsítványok hello funkció toosecure toohello háttér-szolgáltatás, az API-k. Ez az útmutató bemutatja, hogyan toomanage tanúsítványok hello API publisher portálon, és hogyan tooconfigure az API-k toouse egy tanúsítvány tooaccess a háttér-szolgáltatás.

Tanúsítványok hello API Management REST API használatával történő kezelésével kapcsolatos információkért lásd: [Azure API Management REST API tanúsítvány entitás][Azure API Management REST API Certificate entity].

## <a name="prerequisites"></a>Előfeltételek
Ez az útmutató bemutatja, hogyan tooconfigure az API Management szolgáltatás példány toouse ügyfél tanúsítvány hitelesítési tooaccess hello háttér-szolgáltatást egy API-t. Következő hello ebben a témakörben ismertetett visszaállítási lépésekkel, mielőtt a háttér-szolgáltatás ügyféltanúsítvány-alapú hitelesítés beállítása rendelkeznie kell ([toothis cikk tekintse meg a hitelesítés az Azure Websitesra tooconfigure tanúsítvány] [ tooconfigure certificate authentication in Azure WebSites refer toothis article]), és hozzáférést toohello tanúsítvány és hello hello-tanúsítvány feltöltése hello API Management publisher Portal jelszó.

## <a name="step1"></a>Egy ügyfél-tanúsítvány feltöltése
tooget elindítani, kattintson a **Publisher portal** a hello Azure portál, az API Management szolgáltatás. Ezzel megnyitná toohello API Management publisher portálon.

![API-közzétevő portál][api-management-management-console]

> Ha még nem hozott létre az API Management szolgáltatáspéldány, lásd: [hozzon létre egy API-kezelés szolgáltatás példányt] [ Create an API Management service instance] a hello [Ismerkedés az Azure API Management] [ Get started with Azure API Management] oktatóanyag.
> 
> 

Kattintson a **biztonsági** a hello **API Management** hello balra, majd kattintson a menü **ügyféltanúsítványok**.

![Ügyféltanúsítványok][api-management-security-client-certificates]

egy új tanúsítvány tooupload kattintson **feltöltés tanúsítvány**.

![Tanúsítvány feltöltése][api-management-upload-certificate]

Keresse meg a tooyour tanúsítványt, és adja meg hello jelszó hello tanúsítvány.

> hello tanúsítványnak kell lennie a **.pfx** formátumban. Önaláírt tanúsítványok használata engedélyezett.
> 
> 

![Tanúsítvány feltöltése][api-management-upload-certificate-form]

Kattintson a **feltöltése** tooupload hello tanúsítványt.

> hello tanúsítvány jelszava most van hitelesítve. Ha az nem megfelelő egy hibaüzenet jelenik meg.
> 
> 

![A tanúsítvány feltöltése][api-management-certificate-uploaded]

Hello tanúsítványt a feltöltést követően megjelenő hello **ügyféltanúsítványok** fülre. Ha több tanúsítvány, jegyezze fel a hello tulajdonos, vagy utolsó négy számjegyét hello ujjlenyomat, amelyek használt tooselect hello tanúsítvány konfigurálása az API toouse tanúsítványokat, amikor hello alábbi szabályozott módon hello [konfigurálása egy API toouse ügyféltanúsítványt az átjáró hitelesítési] [ Configure an API toouse a client certificate for gateway authentication] szakasz.

> tanúsítványlánc érvényesítése, például egy önaláírt tanúsítvány használata esetén ki tooturn lépésekkel hello Ez a GYIK ismertetett [elem](api-management-faq.md#can-i-use-a-self-signed-ssl-certificate-for-a-back-end).
> 
> 

## <a name="step1a"></a>Egy ügyfél-tanúsítvány törlése
toodelete egy tanúsítványt, kattintson a **törlése** hello kívánt tanúsítvány mellett.

![Tanúsítvány törlése][api-management-certificate-delete]

Kattintson a **Igen, törölje azt** tooconfirm.

![Törlés megerősítése][api-management-confirm-delete]

Ha hello tanúsítvány egy API-t használja, majd egy figyelmeztetés képernyő jelenik meg. toodelete hello tanúsítvány hello távolítsa el a tanúsítvány minden API-, amelyek a konfigurált toouse azt.

![Törlés megerősítése][api-management-confirm-delete-policy]

## <a name="step2"></a>Egy API toouse ügyféltanúsítványt az átjáró hitelesítés konfigurálása
Kattintson a **API-k** a hello **API Management** hello menüjének balra kattintson a kívánt hello API hello neve, és hello kattintson **biztonsági** fülre.

![API-biztonsági][api-management-api-security]

Válassza ki **ügyféltanúsítványok** a hello **hitelesítő adatokkal rendelkező** legördülő listából.

![Ügyféltanúsítványok][api-management-mutual-certificates]

Válassza ki a kívánt tanúsítványt hello hello **ügyféltanúsítvány** legördülő listából. Ha több tanúsítvány hello tulajdonos tekintse meg, vagy hello utolsó négy számjegyét hello ujjlenyomat hello előző szakasz toodetermine hello megfelelő tanúsítvány leírtaknak megfelelően.

![Tanúsítvány kiválasztása][api-management-select-certificate]

Kattintson a **mentése** toosave hello konfigurációs módosítás toohello API.

> Ez a változás a következő időponttól érvényes azonnal, és használja az adott API toooperations hívások hello tanúsítvány tooauthenticate hello háttér-kiszolgálón.
> 
> 

![API-módosítások mentése][api-management-save-api]

> Egy tanúsítványt egy API-t az átjáró hello háttér-szolgáltatás hitelesítésének megadása esetén hello házirend részévé válik, hogy az API-hoz, és megtekinthetők az hello Helyicsoportházirend-szerkesztő.
> 
> 

![Tanúsítványszabályzat][api-management-certificate-policy]

## <a name="next-steps"></a>Következő lépések
A más módon toosecure HTTP alap vagy megosztott titkos hitelesítést, például a háttérszolgáltatás című cikkben olvashat bővebben a következő videó hello.

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



