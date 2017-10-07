---
title: "aaaConfigure értesítések és az e-mail sablonok az Azure API Management |} Microsoft Docs"
description: "Megtudhatja, hogyan tooconfigure értesítések és az e-mail sablonok az Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: ee25f26d-4752-433b-af9c-3817db38aed5
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: dc23289c25a1641992b73cb955099b3f207b6968
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-notifications-and-email-templates-in-azure-api-management"></a>Hogyan tooconfigure értesítések és az e-mail sablonok az Azure API Management
API-kezelési biztosítja hello tooconfigure értesítéseket meghatározott események és tooconfigure hello e-mail sablonok, amelyek használt toocommunicate hello rendszergazdák és fejlesztők számára az API Management-példány. Ez a témakör bemutatja, hogyan tooconfigure értesítések hello használható eseményt, és ezeket az eseményeket használt hello e-mail sablonok konfigurálásának áttekintése.

## <a name="publisher-notifications"></a>Publisher értesítések konfigurálása
tooconfigure értesítéseket, kattintson a **Publisher portal** a hello Azure portál, az API Management szolgáltatás. Ezzel megnyitná toohello API Management publisher portálon.

![Közzétevő portál][api-management-management-console]

> [!NOTE] 
> Ha még nem hozott létre az API Management szolgáltatáspéldány, lásd: [hozzon létre egy API-kezelés szolgáltatás példányt] [ Create an API Management service instance] a hello [Ismerkedés az Azure API Management] [ Get started with Azure API Management] oktatóanyag.

Kattintson a **értesítések** a hello **API Management** hello menüjének balra tooview hello elérhető értesítések.

![A Publisher értesítések][api-management-publisher-notifications]

hello alábbi listán szereplő események konfigurálható az értesítésekhez.

* **Előfizetési kérelmek (jóváhagyásra van szükség)** – hello megadott e-mailek címzettjeinek és a felhasználók kapnak értesítő e-mailek kapcsolatos előfizetés kéréseinek API jóváhagyásra van szükség.
* **Új előfizetések** – hello megadott e-mailek címzettjeinek és a felhasználók új API termék előfizetések kapcsolatos e-mail értesítéseket kap.
* **Gyűjteményelem alkalmazáskérelmeinek** – hello megadott e-mailek címzettjeinek és a felhasználók e-mail értesítéseket kap, amikor új alkalmazások elküldött toohello alkalmazáskatalógusában.
* **Titkos másolat** – hello megadott e-mailek címzettjeinek és a felhasználók kapnak e-mailek Titkos másolatot minden küldött e-mailek toodevelopers.
* **Új probléma vagy megjegyzés** – hello megadott e-mailek címzettjeinek és a felhasználók kapnak értesítő e-mailek, amikor új probléma vagy megjegyzés küldése hello fejlesztői portálján.
* **Zárja be a fiók üzenet** – hello megadott e-mailek címzettjeinek és a felhasználók e-mail értesítéseket kap, ha a fiók le van zárva.
* **Megközelíti előfizetés kvótakorlátot** - e-mailek címzettjeinek következő hello és a felhasználók e-mail értesítéseket kap, ha az előfizetés használatának lekérdezi Bezárás toousage kvóta.

Az egyes eseményekhez megadhatja az e-mailek címzettjeinek hello e-mail cím beviteli mezőbe, vagy választhat a felhasználók listájából.

toospecify hello e-mail címek toobe értesítést kap, mezőben adja meg őket hello e-mail címet. Ha az e-mail címeket, külön azokat vesszővel válassza el őket.

![Értesítés címzettjeinek][api-management-email-addresses]

toospecify hello felhasználók toobe értesítést kap, kattintson a **címzettet adjon hozzá**, hello hello felhasználók toobe értesítés melletti jelölőnégyzetet, majd kattintson **OK**.

> [!NOTE] 
> Csak a rendszergazdák hello listában jelennek meg.


Miután hello értesítés címzettjeinek, kattintson a **mentése** tooapply hello értesítés címzettjeinek frissítése.

> [!NOTE] 
> Ha elnavigál hello **Publisher értesítések** lapon hello publisher portal riasztást küld a hiba nem mentett módosítások vannak.


## <a name="email-templates"></a>E-mail sablonok konfigurálása
Az API Management biztosít e-mail sablonok hello hello működés során felügyelete és hello szolgáltatás használatával küldött e-mail üzenetek. a következő e-mail sablonok hello vannak megadva.

* Application gallery küldésének jóváhagyott
* Fejlesztői farewell levél
* Értesítési megközelítő fejlesztői kvótakorlátot
* Felhasználó meghívása
* Új megjegyzés hozzá tooan probléma
* Kapott új probléma
* Új előfizetés aktiválása
* Előfizetés megújítása megerősítése
* Előfizetés visszautasítja
* Előfizetés kérelem érkezett

Ezek a sablonok módosítható igény szerint.

tooview és hello e-mail sablonok az API Management-példány beállításához kattintson a gombra **értesítések** a hello **API Management** hello balra, és jelölje be hello menüjének **E-mail sablonok**  fülre.

![E-mail-sablonok][api-management-email-templates]

tooview vagy egy adott sablon módosításához válassza ki azt a hello **sablonok** legördülő listából.

![E-mail sablonok listája][api-management-email-templates-list]

Minden e-mail sablon egyszerű szöveges tárgy és törzs HTML formátumban definícióját rendelkezik. Minden elem testre szabható, igény szerint.

![E-mail sablon szerkesztő][api-management-email-template]

Hello **paraméterek** lista felsorolja azokat a paraméterek, amikor hello tárgya és törzse beszúrt, kijelölt kicserélt hello érték lesz üdvözlő e-mail küldésekor. a paramétert, tooinsert kurzorral hello ahol hello paraméter toogo kívánja, majd kattintson a hello toohello balra nyíl hello paraméternév.

Kattintson a **előzetes** vagy **egy teszt küldése** toosee hogyan hello e-mail fog meg, vagy egy teszt e-mailek küldése.

> [!NOTE] 
> hello paraméterek nem cserélhető le tényleges értékek előnézet vagy egy teszt küldése.

toosave hello módosítások toohello e-mail sablont, kattintson a **mentése**, vagy toocancel hello kattintson **Mégse**.
 

[api-management-management-console]: ./media/api-management-howto-configure-notifications/api-management-management-console.png
[api-management-publisher-notifications]: ./media/api-management-howto-configure-notifications/api-management-publisher-notifications.png
[api-management-email-addresses]: ./media/api-management-howto-configure-notifications/api-management-email-addresses.png


[api-management-email-templates]: ./media/api-management-howto-configure-notifications/api-management-email-templates.png
[api-management-email-templates-list]: ./media/api-management-howto-configure-notifications/api-management-email-templates-list.png
[api-management-email-template]: ./media/api-management-howto-configure-notifications/api-management-email-template.png


[Configure publisher notifications]: #publisher-notifications
[Configure email templates]: #email-templates

[How toocreate and use groups]: api-management-howto-create-groups.md
[How tooassociate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
