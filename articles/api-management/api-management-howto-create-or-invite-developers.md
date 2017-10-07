---
title: "aaaHow az Azure API Management felhasználói fiókok kezelése |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate vagy a meghívott felhasználó felhasználók az Azure API Management"
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 078abfa5-1e4f-4c9d-b9c7-a172bd19c1a2
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 3966f4454e29621d7c615beefee352ec91b48b2e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-user-accounts-in-azure-api-management"></a>Hogyan toomanage felhasználói fiókot, az Azure API Management
Az API Management fejlesztők hello teszi az API Management használata API-k hello felhasználók. Ez az útmutató azt mutatja be toohow toocreate és a meghívás fejlesztők toouse hello API-k és termékek, hogy elérhető toothem az API Management-példánnyal. A felhasználói fiókok kezelése programozott módon információkért lásd: hello [felhasználói entitás](https://msdn.microsoft.com/library/azure/dn776330.aspx) hello dokumentáció [API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) hivatkozás.

## <a name="create-developer"></a>Hozzon létre egy új fejlesztő
Kattintson egy új fejlesztői toocreate **Publisher portal** hello Azure portál, az API-kezelés szolgáltatás a. Ezzel megnyitná toohello API Management publisher portálon. Ha még nem hozott létre az API Management szolgáltatáspéldány, lásd: [hozzon létre egy API-kezelés szolgáltatás példányt] [ Create an API Management service instance] a hello [Ismerkedés az Azure API Management] [ Get started with Azure API Management] oktatóanyag.

![Közzétevő portál][api-management-management-console]

Kattintson a **felhasználók** a hello **API Management** hello maradt, és válassza a menü **felhasználó hozzáadása**.

![Fejlesztői létrehozása][api-management-create-developer]

Adja meg a hello **E-mail**, **jelszó**, és **neve** hello új fejlesztői, és kattintson a **mentése**.

![Fejlesztői létrehozása][api-management-add-new-user]

Alapértelmezés szerint az újonnan létrehozott fejlesztői fiókok vannak **aktív**, és a társított hello **fejlesztők** csoport.

![Új fejlesztői][api-management-new-developer]

A fejlesztői fiókok egy **aktív** állapot lehet használt tooaccess összes hello API-k, amelyeknél az előfizetések. tooassociate az újonnan létrehozott hello fejlesztői további csoportokkal, lásd: [hogyan tooassociate csoportok fejlesztőkkel][How tooassociate groups with developers].

## <a name="invite-developer"></a>Egy fejlesztő meghívása
tooinvite egy fejlesztő kattintson **felhasználók** a hello **API Management** hello maradt, és válassza a menü **hívhat meg felhasználói**.

![Fejlesztői meghívása][api-management-invite-developer]

Adja meg a hello fejlesztői hello név és e-mail címét, majd kattintson a **meghívása**.

![Fejlesztői meghívása][api-management-invite-developer-window]

Egy megerősítő üzenet jelenik meg, de újonnan meghívott hello fejlesztői nem jelenik meg amíg hello lista után hello meghívó elfogadja őket. 

![Jóváhagyás meghívása][api-management-invite-developer-confirmation]

Amikor egy fejlesztő felkérik, egy e-mailt küld toohello fejlesztői. Ez az e-mail sablon használatával jön létre, és testre szabható. További információkért lásd: [konfigurálás e-mail sablonok][Configure email templates].

Hello felkérés elfogadása után hello fiók aktívvá válik.

## <a name="block-developer"></a> Inaktiválja vagy újraaktiválásához fejlesztői fiók létrehozása
Alapértelmezés szerint az újonnan létrehozott vagy a meghívott fejlesztői fiókok vannak **aktív**. fejlesztői fiók létrehozása toodeactivate kattintson **blokk**. letiltott fejlesztői fiók létrehozása tooreactivate kattintson **aktiválás**. Letiltott fejlesztői fiók létrehozása nem hello fejlesztői portálján érheti el és bármely API-k hívása. toodelete egy felhasználói fiókot, kattintson a **törlése**.

![Blokk fejlesztői][api-management-new-developer]

## <a name="reset-a-user-password"></a>Felhasználói jelszó alaphelyzetbe állítása
tooreset hello jelszavát egy felhasználói fiókot, kattintson a hello hello fiók nevét.

![Új jelszó létrehozása][api-management-view-developer]

Kattintson a **jelszó-átállítási** toosend egy hivatkozás toohello felhasználói tooreset a jelszavát.

![Új jelszó létrehozása][api-management-reset-password]

tooprogrammatically együttműködnek a felhasználói fiókok, lásd: hello [felhasználói entitás](https://msdn.microsoft.com/library/azure/dn776330.aspx) hello dokumentáció [API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) hivatkozás. egy felhasználói fiók jelszavát tooa érték tooreset, hello használható [egy felhasználó frissítéséhez](https://msdn.microsoft.com/library/azure/dn776330.aspx#UpdateUser) művelet, és adja meg a hello kívánt jelszót.

## <a name="pending-verification"></a>Függőben lévő ellenőrzési
![Függőben lévő ellenőrzési][api-management-pending-verification]

## <a name="next-steps"></a>Következő lépések
A fejlesztői fiók létrehozása után szerepkörökhöz társítandó, és tooproducts és API-k fizessen elő. További információkért lásd: [hogyan toocreate és -felhasználási csoportok][How toocreate and use groups].

[api-management-management-console]: ./media/api-management-howto-create-or-invite-developers/api-management-management-console.png
[api-management-add-new-user]: ./media/api-management-howto-create-or-invite-developers/api-management-add-new-user.png
[api-management-create-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png
[api-management-invite-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer.png
[api-management-new-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-new-developer.png
[api-management-invite-developer-window]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-window.png
[api-management-invite-developer-confirmation]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-confirmation.png
[api-management-pending-verification]: ./media/api-management-howto-create-or-invite-developers/api-management-pending-verification.png
[api-management-view-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-view-developer.png
[api-management-reset-password]: ./media/api-management-howto-create-or-invite-developers/api-management-reset-password.png


[Create a new developer]: #create-developer
[Invite a developer]: #invite-developer
[Deactivate or reactivate a developer account]: #block-developer
[Next steps]: #next-steps
[How toocreate and use groups]: api-management-howto-create-groups.md
[How tooassociate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Configure email templates]: api-management-howto-configure-notifications.md#email-templates
