---
title: "csoportok használata az Azure API Management aaaManage fejlesztői fiókok |} Microsoft Docs"
description: "Ismerje meg, hogyan toomanage fejlesztői fiókok csoportok az Azure API Management"
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 33660b45-442f-44be-9a4a-1899d7f699b0
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: c46e010e41d9705ae161dcd60d734a76d19c9e93
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-use-groups-toomanage-developer-accounts-in-azure-api-management"></a>Hogyan toocreate és -felhasználási csoportok toomanage fejlesztői fiókok az Azure API Management
Az API Management csoportok olyan termékek toodevelopers használt toomanage hello láthatóságát. Termékek első készült látható toogroups, és ezeket a csoportokat a fejlesztők megtekintheti és toohello termékek hello csoportokhoz társított előfizetés majd. 

Az API Management követően nem módosítható rendszer csoportok hello rendelkezik.

* **Rendszergazdák** – A csoportot Azure-előfizető rendszergazdák alkotják. Rendszergazdák kezelése az API Management szolgáltatáspéldány, API-k, műveletek és a fejlesztők által használt termékek létrehozása hello.
* **Fejlesztők** – A fejlesztői portál hitelesített felhasználói tartoznak ebbe a csoportba. A fejlesztők olyan alkalmazásokat, az API-k használatával készíthetnek hello ügyfelek. A fejlesztők kapnak hozzáférést toohello fejlesztői portálján, és alkalmazások összeállítását, amelyek hívja az API-k hello működésére.
* **A vendégek** -hitelesítés nélküli developer portálon a felhasználóknak, például a lehetséges ügyfelek látogató hello fejlesztői portálján az API Management példány alá esik az ebbe a csoportba. Ezek is hozzáférést bizonyos csak olvasható, például a hello képességét tooview API-k, de nem hívható meg őket.

Toothese rendszer csoportok hozzáadása, a rendszergazdák egyéni csoportot hozhat létre vagy [kihasználja a társított Azure Active Directory-bérlők külső csoportok][leverage external groups in associated Azure Active Directory tenants]. Egyéni és külső csoportok rendszer csoportok jogosultságot ad a fejlesztők számára látható együtt is használható, és tooAPI terméket. Például létrehozhat egy egyéni csoport tagja-e egy adott fejlesztők fiókpartner szervezetében dolgozó és a hozzáférés engedélyezése csak megfelelő API-k tartalmazó az API-k toohello. Egy felhasználó egyszerre több csoport tagja is lehet.

Ez az útmutató bemutatja, hogyan API Management példány rendszergazdák új csoportok hozzáadása és rendelheti őket hozzá a termékek és a fejlesztők számára.

> [!NOTE]
> Ezenkívül toocreating és hello publisher portálon csoportok kezelése, akkor hozhat létre és kezelhet a csoportok hello API Management REST API használatával [csoport](https://msdn.microsoft.com/library/azure/dn776329.aspx) entitás.
> 
> 

## <a name="create-group"></a>Hozzon létre egy csoportot
egy új csoport toocreate kattintson **Publisher portal** hello Azure portál, az API-kezelés szolgáltatás a. Ezzel megnyitná toohello API Management publisher portálon.

![Közzétevő portál][api-management-management-console]

> Ha még nem hozott létre az API Management szolgáltatáspéldány, lásd: [hozzon létre egy API-kezelés szolgáltatás példányt] [ Create an API Management service instance] a hello [Ismerkedés az Azure API Management] [ Get started with Azure API Management] oktatóanyag.
> 
> 

Kattintson a **csoportok** a hello **API Management** hello maradt, és válassza a menü **csoport hozzáadása**.

![Új csoport hozzáadása][api-management-add-group]

Adjon meg egy egyedi nevet hello csoport és az opcionális leírást, és kattintson a **mentése**.

![Új csoport hozzáadása][api-management-add-group-window]

hello új csoport megjelenik a(z) hello csoportok esetén külön-külön tooedit hello **neve** vagy **leírás** hello csoport, kattintson a hello csoport hello listában hello nevét. toodelete hello csoportjában kattintson a **törlése**.

![Csoport hozzáadva][api-management-new-group]

Most, hogy hello csoport jön létre, termékek és a fejlesztők kapcsolódó lehet.

## <a name="associate-group-product"></a>Társítsa a termék egy csoportot
a csoportban található, a termék tooassociate kattintson **termékek** a hello **API Management** hello menüjének balra, és kattintson a kívánt termék hello hello nevét.

![Láthatóság megadása][api-management-add-group-to-product]

Jelölje be hello **látható** tooadd lapra, és távolítsa el a csoportokból és tooview hello aktuális hello termékhez. tooadd vagy csoportok, ellenőrizze, vagy törölje a hello négyzet jelölését, hello szükségeskonfiguráció-csoportokat, és kattintson a **mentése**.

![Láthatóság megadása][api-management-add-group-to-product-visibility]

> [!NOTE]
> Active Directory-csoportok tooadd, lásd: [hogyan tooauthorize fejlesztői fiókok Azure Active Directory, az Azure API Management](api-management-howto-aad.md).
> 
> hello tooconfigure csoportjait **látható** termék lapra, majd **csoportok kezelése**.
> 
> 

Ha a termék egy csoporthoz tartozik, fejlesztők csoport megtekintheti és toohello termék előfizetés.

## <a name="associate-group-developer"></a>Csoportok társítani a fejlesztők számára
tooassociate csoportok fejlesztőkkel, kattintson a **felhasználók** a hello **API Management** hello maradt, és hello fejlesztők melletti jelölőnégyzet hello menüjének kívánja tooassociate egy csoportba.

![Fejlesztői toogroup hozzáadása][api-management-add-group-to-developer]

Ha hello szükséges, hogy a fejlesztők a rendszer ellenőrzi, kattintson a kívánt csoport hello hello **tooGroup hozzáadása** legördülő listán. Csoportok fejlesztők eltávolítható hello segítségével **távolítsa el a csoportból** legördülő listán. 

![Fejlesztők][api-management-add-group-to-developer-saved]

Hello fejlesztői és hello csoport közötti hello társítás hozzáadása után megtekintheti az hello **felhasználók** fülre.

## <a name="next-steps"></a>Következő lépések
* Egy fejlesztő tooa csoport hozzáadása után tekinthet meg és a csoport toohello termékek előfizetés. További információkért lásd: [hogyan létrehozása, és a termék közzététele az Azure API Management][How create and publish a product in Azure API Management],
* Ezenkívül toocreating és hello publisher portálon csoportok kezelése, akkor hozhat létre és kezelhet a csoportok hello API Management REST API használatával [csoport](https://msdn.microsoft.com/library/azure/dn776329.aspx) entitás.

[api-management-management-console]: ./media/api-management-howto-create-groups/api-management-management-console.png
[api-management-add-group]: ./media/api-management-howto-create-groups/api-management-add-group.png
[api-management-add-group-window]: ./media/api-management-howto-create-groups/api-management-add-group-window.png
[api-management-new-group]: ./media/api-management-howto-create-groups/api-management-new-group.png
[api-management-add-group-to-product]: ./media/api-management-howto-create-groups/api-management-add-group-to-product.png
[api-management-add-group-to-product-visibility]: ./media/api-management-howto-create-groups/api-management-add-group-to-product-visibility.png
[api-management-add-group-to-developer]: ./media/api-management-howto-create-groups/api-management-add-group-to-developer.png
[api-management-add-group-to-developer-saved]: ./media/api-management-howto-create-groups/api-management-add-group-to-developer-saved.png

[api-management-]: ./media/api-management-howto-create-groups/api-management-.png

[Create a group]: #create-group
[Associate a group with a product]: #associate-group-product
[Associate groups with developers]: #associate-group-developer
[Next steps]: #next-steps

[How create and publish a product in Azure API Management]: api-management-howto-add-products.md

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[leverage external groups in associated Azure Active Directory tenants]: api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group
