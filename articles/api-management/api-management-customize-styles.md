---
title: "hello developer portálon az Azure API Management aaaCustomize stílusok |} Microsoft Docs"
description: "Ismerje meg, hogyan toomodify hello stílusok használt bármilyen oldalt az Azure API Management hello developer portálon."
services: api-management
documentationcenter: 
author: antonba
manager: vlvinogr
editor: 
ms.assetid: 186128fe-41c0-4efb-9efe-2478ad4d103f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/09/2017
ms.author: antonba
ms.openlocfilehash: aaaa86527992ba43e64eab5fd35c7f57b573c812
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="customize-hello-styling-of-hello-developer-portal-in-azure-api-management"></a>Hello stílusbeállításokat az Azure API Management hello fejlesztői portál testreszabása
Azure API Management három alapvető módját toocustomize hello fejlesztői portálján szerepelnek:

* [Statikus és elrendezés elemei hello tartalom szerkesztése][modify-content-layout]
* [Hello fejlesztői portálján keresztül elemei használt hello stílusok frissítse] [ customize-styles] (Ez az útmutató alapján)
* [Módosíthatja a hello hello portál által létrehozott lapok használt] [ portal-templates] (pl. API docs, termékek, felhasználói hitelesítés, stb.)

## <a name="change-headers-styling"></a>Hello stílusbeállításokat a lap elemeinek módosítása

hello színek, betűtípusok, méretét, céljának megfelelőként és más stílus kapcsolatos elemek hello portál bármely lapján vannak definiálva stílus szabályok. 

Hello hello stílus szabályok szerkesztése végezheti **fejlesztői portálján** közben rendszergazdaként naplózva. Nincs tooget először nyissa meg a hello Azure portálon, és kattintson **Publisher portal** hello szolgáltatás eszköztáron az API Management-példány.

![Közzétevő portál][api-management-management-console]

Kattintson a **fejlesztői portálján** a hello jobb felső. 

![Fejlesztői portálon hivatkozás hello publisher portálon][api-management-pp-dp-link]

tooopen hello testreszabási eszköztár az egérmutatót hello testreszabási ikon (vagy válassza ki azt), és majd kattintson a "stílusait" hello eszköztáron.

![Testreszabási eszköztár gombja][api-management-customization-toolbar-button]

Nincs szerkesztési stílusbeállításokat szabályok két fő irányban - nézze át a hello listán bárhol használt összes hello stílus szabályok alapértelmezés szerint megjelenik, és szükség szerint módosítsa a stílus, vagy dönthet úgy, **egy elem kijelölése hello oldalon** , majd Kattintson bárhová a hello lap toosee csak hello stílusok adott elemhez.

![Testreszabás eszköztár][api-management-customization-toolbar]

Kattintson a hello **egy elem kijelölése hello oldalon** ehhez a példához lehetőséget.  Elemek most kiemeli az rámutatáskor őket a hello egér toosignify kattintott szerkesztése kezdenie milyen elem stílusok. Áthelyezési hello egér hello szöveg hello fejléc (általában kell hello vállalatnév itt), majd azt. Elnevezett és kategorizált stílusbeállításokat szabályok hello stílusbeállításokat szerkesztő belül jelenik meg. Minden egyes szabály hello kijelölt elem stílusbeállításokat tulajdonságait képviselik. Például a fent kijelölt hello fejlécszöveg hello hello szöveg mérete a @font-size-h1 hello betűtípus lehetőségeket hello neve alatt @headings-font-family.

> Ha ismeri [bootstrap][bootstrap], ezek a szabályok valóban [kevesebb változók] [ LESS variables] hello bootstrap téma hello által használt belül fejlesztői portálján.
> 
> 

Módosítsuk hello hello címsor szövegének színét. Válassza ki a hello bejegyzést hello  **@headings-color**  mező, és írja be **#000000**. Ez a hello hexadecimális kód fekete hello szín. Mivel ezt láthatja, hogy egy négyzetes színe kijelző hello szövegmező hello végén megjelenik-e. Ha a kijelző gombra kattint, az elemleírás lehetővé teszi, hogy toochoose színt.

![Színválasztó][api-management-customization-toolbar-color-picker]

Módosítások vannak előzetesen valós időben, akkor tegye őket, de látható csak tooadministrators. toomake a módosítások láthatóvá tooeveryone, kattintson a hello **közzététel** hello stílusbeállításokat szerkesztőben gombra, majd erősítse meg a hello módosításokat.

![Közzététel menü][api-management-customization-toolbar-publish-form]

> toochange hello stílus tooany szabályok más elem hello oldalon, kövesse az azonos hello eljárást, ha Ön hello fejléc esetében tette. Kattintson a **egy elem kijelölése hello oldalon** hello stílusbeállításokat szerkesztő válassza hello elem érdekli, és kezdő hello értékek hello képernyőn megjelenő hello stílus szabályok módosítása.
> 
> 


## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan toocustomize hello tartalma fejlesztői portálján lapok használatával [fejlesztői portál sablonok](api-management-developer-portal-templates.md).

[Change hello styling of hello headers]: #change-headers-styling
[Next steps]: #next-steps

[Azure Classic Portal]: https://manage.windowsazure.com/

[api-management-management-console]: ./media/api-management-customize-styles/api-management-management-console.png
[api-management-pp-dp-link]: ./media/api-management-customize-styles/api-management-pp-dp-link.png
[api-management-customization-toolbar-button]: ./media/api-management-customize-styles/api-management-customization-toolbar-button.png
[api-management-customization-toolbar]: ./media/api-management-customize-styles/api-management-customization-toolbar.png
[api-management-customization-toolbar-color-picker]: ./media/api-management-customize-styles/api-management-customization-toolbar-color-picker.png
[api-management-customization-toolbar-publish-form]: ./media/api-management-customize-styles/api-management-customization-toolbar-publish-form.png

[modify-content-layout]: api-management-modify-content-layout.md
[customize-styles]: api-management-customize-styles.md
[portal-templates]: api-management-developer-portal-templates.md

[bootstrap]: http://getbootstrap.com/
[LESS variables]: http://getbootstrap.com/css/
