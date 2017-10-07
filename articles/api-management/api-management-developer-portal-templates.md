---
title: "aaaCustomize hello API Management fejlesztői portálján sablonokkal-Azure |} Microsoft Docs"
description: "Ismerje meg, hogyan toocustomize hello Azure API Management fejlesztői portálján sablonok használatával."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: a195675b-f7d0-4fc9-90bf-860e6f17ccf7
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: b00d5f1534e9466f30ff3920e7aae048feb8b8c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocustomize-hello-azure-api-management-developer-portal-using-templates"></a>Hogyan toocustomize hello Azure API Management fejlesztői portálján sablonokkal

Azure API Management három alapvető módját toocustomize hello fejlesztői portálján szerepelnek:

* [Statikus és elrendezés elemei hello tartalom szerkesztése][modify-content-layout]
* [Frissítés hello stílusok hello fejlesztői portálján keresztül használt elemei][customize-styles]
* [Módosíthatja a hello hello portál által létrehozott lapok használt] [ portal-templates] (Ez az útmutató alapján)

Sablonok használt toocustomize hello tartalmat a rendszer fejlesztői portál lapjai (pl. API docs, termékek, felhasználói hitelesítés, stb.) a rendszer. Használatával [DotLiquid](http://dotliquidmarkup.org/) szintaxis és a megadott készlete a honosított karakterlánc-erőforrások, ikonok és Lapvezérlők, ahogyan szeretné rendelkezik rugalmas lehetőségeket biztosítanak tooconfigure hello hello lapok tartalmát.

## <a name="developer-portal-templates-overview"></a>Fejlesztői portál sablonok – áttekintés
Sablonok szerkesztéséhez végezheti el hello **fejlesztői portálján** közben rendszergazdaként bejelentkezve alatt. Nincs tooget először nyissa meg a hello Azure portálon, és kattintson **Publisher portal** hello szolgáltatás eszköztáron az API Management-példány.

![Közzétevő portál][api-management-management-console]

Kattintson a **fejlesztői portálján** a hello jobb felső. 

![Fejlesztői portál menüjében][api-management-developer-portal-menu]

tooaccess hello fejlesztői portál sablonok, kattintson a hello ikon hello bal oldali toodisplay hello testreszabási menü testreszabása, és kattintson a **sablonok**.

![Fejlesztői portál sablonok][api-management-customize-menu]

hello sablonok listájának kiterjedő hello különböző oldalain hello fejlesztői portálján a sablonok számos kategóriája jeleníti meg. Minden sablon nem egyezik, de hello lépéseket tooedit őket és hello változtatásokat azonos hello. tooedit sablont, kattintson a hello sablon hello nevét.

![Fejlesztői portál sablonok][api-management-templates-menu]

Kattintson egy sablon viszi toohello developer portálon, amely testre szabható, hogy a sablon alapján. Az ebben a példában hello **termékek listáját** sablon jelenik meg. Hello **termékek listáját** sablon vezérlők hello hello piros téglalap által jelzett üdvözlő képernyőt területének. 

![Termékek sablon][api-management-developer-portal-templates-overview]

Egyes sablonok, például a hello **felhasználói profil** sablonok testreszabása hello különböző részei ugyanazon az oldalon. 

![Felhasználói profil sablonok][api-management-user-profile-templates]

minden developer portálon sablon hello szerkesztő két részből áll hello aljához hello oldal jelenik meg. hello bal oldali ablaktáblán hello sablon szerkesztése hello, valamint hello jobb oldalán hello adatmodell hello sablon jeleníti meg. 

hello sablonszerkesztési panelen, amely a hello megjelenését és viselkedését hello megfelelő oldal hello developer portálon hello markup tartalmazza. hello markup hello sablonban használ hello [DotLiquid](http://dotliquidmarkup.org/) szintaxist. Egy népszerű szerkesztője a DotLiquid [tervezőknek DotLiquid](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers). A végzett módosítások toohello sablon szerkesztése során jelennek meg a valós idejű hello böngészővel, de azok nem látható tooyour ügyfelek amíg [mentése](#to-save-a-template) és [közzététele](#to-publish-a-template) hello sablon.

![Sablon markup][api-management-template]

Hello **sablon adatok** ablaktáblán egy útmutató toohello adatokat biztosít, amelyek egy megadott sablont használható hello entitások modellre. Ez az útmutató biztosít hello hello fejlesztői portálján a jelenleg megjelenített élő adatok megjelenítésével jelzi ezt. Hello sablon ablaktáblák bővítheti hello téglalap hello jobb felső sarkában hello kattintva **sablon adatok** ablaktáblán.

![Sablon adatmodell][api-management-template-data]

Az előző példában hello nincsenek hello developer portálról hello adatokból hello megjelenik eredményező két termék **sablon adatok** ablaktáblán, ahogy az alábbi példa hello.

```json
{
    "Paging": {
        "Page": 1,
        "PageSize": 10,
        "TotalItemCount": 2,
        "ShowAll": false,
        "PageCount": 1
    },
    "Filtering": {
        "Pattern": null,
        "Placeholder": "Search products"
    },
    "Products": [
        {
            "Id": "56ec64c380ed850042060001",
            "Title": "Starter",
            "Description": "Subscribers will be able toorun 5 calls/minute up tooa maximum of 100 calls/week.",
            "Terms": "",
            "ProductState": 1,
            "AllowMultipleSubscriptions": false,
            "MultipleSubscriptionsCount": 1
        },
        {
            "Id": "56ec64c380ed850042060002",
            "Title": "Unlimited",
            "Description": "Subscribers have completely unlimited access toohello API. Administrator approval is required.",
            "Terms": null,
            "ProductState": 1,
            "AllowMultipleSubscriptions": false,
            "MultipleSubscriptionsCount": 1
        }
    ]
}
```

a hello hello markup **termékek listáját** sablon folyamatok tooprovide szükséges hello kimeneti adatok hello hello termékek toodisplay információk összegyűjtése és a hivatkozás tooeach egyedileg importált iteráció által. Megjegyzés: hello `<search-control>` és `<page-control>` hello kódkiterjesztési elemek. Ezek szabályozza, hogy a Keresés és a lapozás hello lap hello hello megjelenítését. `ProductsStrings|PageTitleProducts`honosított karakterlánc hivatkozás hello tartalmazó `h2` hello lap a fejléc szövege. Erőforrásait, Lapvezérlők és fejlesztői portál sablonok használható ikonok listáját lásd: [API Management fejlesztői portál sablonok referenciája](api-management-developer-portal-templates-reference.md).

```html
<search-control></search-control>
<div class="row">
    <div class="col-md-9">
        <h2>{% localized "ProductsStrings|PageTitleProducts" %}</h2>
    </div>
</div>
<div class="row">
    <div class="col-md-12">
    {% if products.size > 0 %}
    <ul class="list-unstyled">
    {% for product in products %}
        <li>
            <h3><a href="/products/{{product.id}}">{{product.title}}</a></h3>
            {{product.description}}
        </li>    
    {% endfor %}
    </ul>
    <paging-control></paging-control>
    {% else %}
    {% localized "CommonResources|NoItemsToDisplay" %}
    {% endif %}
    </div>
</div>
```

## <a name="toosave-a-template"></a>a sablon toosave
toosave sablont, kattintson a Mentés hello sablon szerkesztőben.

![Sablon mentése][api-management-save-template]

Mindaddig, amíg azok közzé lettek téve, amelyek a módosítások mentése nem élő hello developer portálon.

## <a name="toopublish-a-template"></a>a sablon toopublish
Mentett sablonok külön-külön vagy együtt tehetők közzé. toopublish egy egyéni sablont, kattintson a közzététel hello sablon szerkesztőben.

![Sablon közzététele][api-management-publish-template]

Kattintson a **Igen** tooconfirm és hello sablon élő hello developer portálon.

![Erősítse meg közzététele][api-management-publish-template-confirm]

toopublish jelenleg közzé nem tett verzióival, kattintson a **közzététel** hello sablonok listában. Közzé nem tett sablonok hello sablon nevét a következő csillag jelölik. Ebben a példában hello **termékek listáját** és **termék** sablonok frissítése folyamatban van.

![Sablonok közzététele][api-management-publish-templates]

Kattintson a **testreszabások közzététele** tooconfirm.

![Erősítse meg közzététele][api-management-publish-customizations]

Újonnan közzétett sablonokat is azonnal hatékonyan hello fejlesztői portálján.

## <a name="toorevert-a-template-toohello-previous-version"></a>a sablon toohello korábbi verziójáról toorevert
toorevert sablon toohello előző közzétett verziót, kattintson a hello sablon szerkesztőben állítsa vissza.

![Sablon visszaállítása][api-management-revert-template]

Kattintson a **Igen** tooconfirm.

![Erősítse meg][api-management-revert-template-confirm]

hello korábban sablon közzétett változata hello developer portálon live, miután hello állítsa vissza a művelet befejeződött.

## <a name="toorestore-a-template-toohello-default-version"></a>a sablon toohello alapértelmezett verzió toorestore
Visszaállítás sablonok tootheir alapértelmezett verziója két lépésből áll. Első hello sablonok vissza kell állítani, és majd vissza hello verziók közzé kell tenni.

ugyanazt a sablont toohello alapértelmezett változata toorestore kattintson a visszaállítás hello sablon szerkesztőben.

![Sablon visszaállítása][api-management-reset-template]

Kattintson a **Igen** tooconfirm.

![Erősítse meg][api-management-reset-template-confirm]

toorestore összes sablonok tootheir alapértelmezett verzió, kattintson a **visszaállítása az alapértelmezett sablonok** hello sablon listán.

![Állítsa vissza a sablonok][api-management-restore-templates]

hello visszaállított sablonok majd közzé kell tenni külön-külön és egyszerre hello utasításait követve [toopublish sablon](#to-publish-a-template).

## <a name="next-steps"></a>Következő lépések
A fejlesztői portál sablonok, erőforrásait, ikonok és Lapvezérlők hivatkozás információkért lásd: [API Management fejlesztői portál sablonok referenciája](api-management-developer-portal-templates-reference.md).

[modify-content-layout]: api-management-modify-content-layout.md
[customize-styles]: api-management-customize-styles.md
[portal-templates]: api-management-developer-portal-templates.md

[api-management-customize-menu]: ./media/api-management-developer-portal-templates/api-management-customize-menu.png
[api-management-templates-menu]: ./media/api-management-developer-portal-templates/api-management-templates-menu.png
[api-management-developer-portal-templates-overview]: ./media/api-management-developer-portal-templates/api-management-developer-portal-templates-overview.png
[api-management-template]: ./media/api-management-developer-portal-templates/api-management-template.png
[api-management-template-data]: ./media/api-management-developer-portal-templates/api-management-template-data.png
[api-management-developer-portal-menu]: ./media/api-management-developer-portal-templates/api-management-developer-portal-menu.png
[api-management-management-console]: ./media/api-management-developer-portal-templates/api-management-management-console.png
[api-management-browse]: ./media/api-management-developer-portal-templates/api-management-browse.png
[api-management-user-profile-templates]: ./media/api-management-developer-portal-templates/api-management-user-profile-templates.png
[api-management-save-template]: ./media/api-management-developer-portal-templates/api-management-save-template.png
[api-management-publish-template]: ./media/api-management-developer-portal-templates/api-management-publish-template.png
[api-management-publish-template-confirm]: ./media/api-management-developer-portal-templates/api-management-publish-template-confirm.png
[api-management-publish-templates]: ./media/api-management-developer-portal-templates/api-management-publish-templates.png
[api-management-publish-customizations]: ./media/api-management-developer-portal-templates/api-management-publish-customizations.png
[api-management-revert-template]: ./media/api-management-developer-portal-templates/api-management-revert-template.png
[api-management-revert-template-confirm]: ./media/api-management-developer-portal-templates/api-management-revert-template-confirm.png
[api-management-reset-template]: ./media/api-management-developer-portal-templates/api-management-reset-template.png
[api-management-reset-template-confirm]: ./media/api-management-developer-portal-templates/api-management-reset-template-confirm.png
[api-management-restore-templates]: ./media/api-management-developer-portal-templates/api-management-restore-templates.png







