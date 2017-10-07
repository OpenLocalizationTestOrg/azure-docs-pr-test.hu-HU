---
title: "aaaHow toocreate és a termék közzététele az Azure API Management"
description: "Megtudhatja, hogyan toocreate és termékek közzététele az Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 31de55cb-9384-490b-a2f2-6dfcf83da764
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: f0a37f08b4e29ca68be9caec4c7604e3b4b6aaa6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-publish-a-product-in-azure-api-management"></a>Hogyan toocreate és a termék közzététele az Azure API Management
Az Azure API Management a termék egy vagy több API-k, valamint a használati kvóta és hello használati feltételeket tartalmaz. Miután közzétette a termék, a fejlesztők toohello termék előfizetés és toouse hello termék API-k megkezdéséhez. hello a témakör egy útmutató toocreating termék, az API-k hozzáadása, és a közzététel a fejlesztők számára.

## <a name="create-product"></a>Termék létrehozása
Műveletek bekerülnek és tooan API a hello publisher portálon. tooaccess hello publisher portálon kattintson **Publisher portal** a hello Azure portál, az API Management szolgáltatás.

![Közzétevő portál][api-management-management-console]

> Ha még nem hozott létre az API Management szolgáltatáspéldány, lásd: [hozzon létre egy API-kezelés szolgáltatás példányt] [ Create an API Management service instance] a hello [Ismerkedés az Azure API Management] [ Get started with Azure API Management] oktatóanyag.
> 
> 

Kattintson a **termékek** hello menüben található hello bal oldali toodisplay hello **termékek** lapon, majd kattintson **termék hozzáadása**.

![Termékek][api-management-products]

![Új termék][api-management-add-new-product]

Adjon meg egy leíró nevet a hello termék hello **neve** mező és hello hello termék leírása **leírás** mező.

Az API Management termékek lehet **nyitott** vagy **védett**. Védett termékek kell is használhatók, miközben a Megnyitás előfizetett toobefore termékek előfizetés nélkül is használható. Ellenőrizze **előfizetés szükséges** toocreate egy védett terméket, amely a előfizetést igényel. Ez az hello alapértelmezett beállítása.

Ellenőrizze **előfizetés jóváhagyás szükséges** Ha szeretné, hogy egy rendszergazda tooreview, és fogadja el vagy utasítsa el az előfizetés kísérletek toothis termék. Ha hello jelölőnégyzet nincs bejelölve, a előfizetés kísérletek lesz automatikusan jóváhagyta. Az előfizetések további információkért lásd: [nézet előfizetők tooa termék][View subscribers tooa product].

tooallow fejlesztői fiókok toosubscribe többször toohello terméket, ellenőrizze a hello **több előfizetés engedélyezése** jelölőnégyzetet. Ha a négyzet nincs bejelölve, minden developer-fiók csak egy alkalommal toohello termék kérhet le.

![Korlátlan több előfizetéssel][api-management-unlimited-multiple-subscriptions]

több egyidejű előfizetéssel toolimit hello száma ellenőrizze hello **egyidejű előfizetések számának korlátozása** jelölje be a jelölőnégyzetet, és írja be a hello előfizetési korlátját. A következő példa hello egyidejű előfizetések fejlesztői fiókonként korlátozott toofour.

![Négy több előfizetéssel][api-management-four-multiple-subscriptions]

Ha minden új termék beállításainak konfigurálása után kattintson **mentése** toocreate hello új terméket.

![Termékek][api-management-products-page]

> Alapértelmezés szerint új termékek közzé nem tett, és látható csak toohello **rendszergazdák** csoport.
> 
> 

tooconfigure termék, kattintson a hello termék nevére a hello **termékek** fülre.

## <a name="add-apis"></a>API-k hozzáadása tooa termék
Hello **termékek** lapon négy hivatkozása a konfigurációhoz: **összegzés**, **beállítások**, **látható**, és  **Előfizetők**. Hello **összegzés** lapon látható, ahol akkor is vegye fel az API-k és közzétételét és a termék.

![Összefoglalás][api-management-new-product-summary]

A termék közzététele előtt kell tooadd egy vagy több API-k. toodo, kattintson **hozzáadása API tooproduct**.

![API-k hozzáadása][api-management-add-apis-to-product]

Jelölje be hello szükséges API-t, és kattintson **mentése**.

## <a name="add-description"></a>Hozzáadás leíró tooa termék
Hello **beállítások** a lapon adhatók meg tooprovide részletes információkat hello termék például mindössze egyetlen célra szorítkoznak, hello hozzáférést biztosít a API-k és más hasznos információkat. hello tartalom hello API hívása lesz, és egyszerű szöveg- vagy HTML-kódot is beírhatók hello a fejlesztők irányul.

![A termék beállításai][api-management-product-settings]

Ellenőrizze **előfizetés szükséges** toocreate egy előfizetés toobe használt, vagy törölje a jelet igénylő védett termék hello jelölőnégyzet toocreate egy megnyitott terméket, amely a előfizetés nélkül meghívható.

Válassza ki **előfizetés jóváhagyás szükséges** Ha azt szeretné, toomanually összes termék előfizetési kérelmek jóváhagyása. Alapértelmezés szerint az összes termék előfizetések automatikusan kapnak.

tooallow fejlesztői fiókok toosubscribe többször toohello terméket, ellenőrizze a hello **több előfizetés engedélyezése** jelölje be a jelölőnégyzetet, és opcionálisan adja meg a határértéket. Ha a négyzet nincs bejelölve, minden developer-fiók csak egy alkalommal toohello termék kérhet le.

Nem kötelező kitölteni hello **használati feltételek** hello termék, amely előfizetők el kell fogadnia ahhoz toouse hello termékben a hello használati feltételeit leíró mező.

## <a name="publish-product"></a>Termék közzététele
A termék API-k hello hívása előtt hello termék közzé kell tenni. A hello **összegzés** hello termék lapra, majd **közzététel**, és kattintson a **Igen, azt közzé** tooconfirm. Kattintson egy olyan, korábban kiadott termék magánhálózat toomake **Közzététel megszüntetése**.

![Termék közzététele][api-management-publish-product]

## <a name="make-visible"></a>Ellenőrizze a termék látható toodevelopers
Hello **látható** lapon meghatározhatja, melyik szerepkörök képes toosee hello termék hello developer portálon, és toohello termék előfizetés toochoose.

![A termék látható][api-management-product-visiblity]

tooenable vagy tiltsa le látható-e a termék hello fejlesztők számára az egy csoportba, ellenőrizze, vagy hello csoport hello jelölőnégyzeteket, törölje a jelet, és kattintson a **mentése**.

> További információkért lásd: [hogyan toocreate és -felhasználási csoportok toomanage fejlesztői fiókok az Azure API Management][How toocreate and use groups toomanage developer accounts in Azure API Management].
> 
> 

## <a name="view-subscribers"></a>Előfizetők tooa termék megtekintése
Hello **előfizetők** lap felsorolja az előfizetett toohello termék hello fejlesztőknek. hello részleteit és beállításait minden fejlesztői tekintheti meg a fejlesztői hello nevére kattintva. Ebben a példában nincs fejlesztők még előfizetett toohello termék.

![Fejlesztők][api-management-developer-list]

## <a name="next-steps"></a>Következő lépések
Egyszer hello szükséges API-k és hello termék közzétett, a fejlesztők is toohello termék előfizetés és toocall hello API-k megkezdéséhez. Lásd: mutatja be, ezeket a cikkeket, valamint a speciális termék konfigurációs [hogyan létrehozása, és speciális termékbeállítások konfigurálása az Azure API Management][How create and configure advanced product settings in Azure API Management].

Működő termékekkel kapcsolatos további információkért tekintse meg a következő videó hello.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Using-Products/player]
> 
> 

[Create a product]: #create-product
[Add APIs tooa product]: #add-apis
[Add descriptive information tooa product]: #add-description
[Publish a product]: #publish-product
[Make a product visible toodevelopers]: #make-visible
[View subscribers tooa product]: #view-subscribers
[Next steps]: #next-steps

[api-management-management-console]: ./media/api-management-howto-add-products/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-add-products/api-management-add-product.png
[api-management-add-new-product]: ./media/api-management-howto-add-products/api-management-add-new-product.png
[api-management-unlimited-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-unlimited-multiple-subscriptions.png
[api-management-four-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-four-multiple-subscriptions.png
[api-management-products-page]: ./media/api-management-howto-add-products/api-management-products-page.png
[api-management-new-product-summary]: ./media/api-management-howto-add-products/api-management-new-product-summary.png
[api-management-add-apis-to-product]: ./media/api-management-howto-add-products/api-management-add-apis-to-product.png
[api-management-product-settings]: ./media/api-management-howto-add-products/api-management-product-settings.png
[api-management-publish-product]: ./media/api-management-howto-add-products/api-management-publish-product.png
[api-management-product-visiblity]: ./media/api-management-howto-add-products/api-management-product-visibility.png
[api-management-developer-list]: ./media/api-management-howto-add-products/api-management-developer-list.png



[api-management-products]: ./media/api-management-howto-add-products/api-management-products.png
[api-management-]: ./media/api-management-howto-add-products/
[api-management-]: ./media/api-management-howto-add-products/


[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How toocreate and publish a product]: api-management-howto-add-products.md
[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps
[How toocreate and use groups toomanage developer accounts in Azure API Management]: api-management-howto-create-groups.md
[How create and configure advanced product settings in Azure API Management]: api-management-howto-product-with-rules.md 
