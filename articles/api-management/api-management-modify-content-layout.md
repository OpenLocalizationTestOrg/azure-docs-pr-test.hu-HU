---
title: "aaaModify lap tartalmát hello developer portálon az Azure API Management |} Microsoft Docs"
description: "Ismerje meg, hogyan tooedit lap tartalmát hello developer portálon az Azure API Management."
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
ms.openlocfilehash: fd5a854e900d9512518643e593b1b59a0952621f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="modify-hello-content-and-layout-of-pages-on-hello-developer-portal-in-azure-api-management"></a>Hello tartalom és hello developer portálon az Azure API Management lapjainak elrendezését módosítása
Azure API Management három alapvető módját toocustomize hello fejlesztői portálján szerepelnek:

* [Hello szerkesztéshez statikus lapok és elrendezés elemei] [ modify-content-layout] (Ez az útmutató alapján)
* [Frissítés hello stílusok hello fejlesztői portálján keresztül használt elemei][customize-styles]
* [Módosíthatja a hello hello portál által létrehozott lapok használt] [ portal-templates] (pl. API docs, termékek, felhasználói hitelesítés, stb.)

## <a name="page-structure"></a>A fejlesztői portál oldalainak szerkezete

hello fejlesztői portálján a Tartalomkezelés rendszeren alapul. minden oldalon hello elrendezésének épül widgeteket néven kis elemei készlete:

![A fejlesztői portál oldalszerkezete][api-management-customization-widget-structure]

Az összes widget szerkeszthető. 
* hello core tartalma adott tooeach egyes lapok hello "Tartalom" widget találhatók. A weblapot azt jelenti, hogy ezt a widgetet hello tartalmának szerkesztéséhez.
* Minden elrendezés elemei a hello további widgetek tartalmazza. Módosítások toothese widgeteket tooall lapok érvényesek. A hivatkozott tooas "elrendezés widgeteket" fogja.

Napi lapon szerkesztése egy általában csak módosítja hello tartalom widgetben érhetik el lesz különböző minden egyes lapok tartalmát.

## <a name="modify-layout-widget"></a>Elrendezés widgetet hello tartalmának módosítása

Tartalom hello developer portálon hello publisher portálon elérhető Azure Portal hello módosítása. tooreach, kattintson a **Publisher portal** hello szolgáltatás eszköztáron az API Management-példány.

![Közzétevő portál][api-management-management-console]

Kattintson az adott widget tooedit hello tartalmát **Widgeteket** hello a **fejlesztői portálján** hello bal oldali menüben. Az ebben a példában lehetővé teszi, hogy hello fejléc widget hello tartalmának módosítása. Jelölje be hello **fejléc** widget hello listából.

![Widgetek fejléc][api-management-widgets-header]

hello fejléc hello tartalmát belül hello szerkeszthető **törzs** mező. Hello szöveg tetszés szerint módosítsa, majd kattintson a **mentése** hello lap hello alján.

Most el tudja toosee hello új fejléc minden oldalon hello developer portálon.

> tooopen hello fejlesztői portálján a hello publisher portál, kattintson a **fejlesztői portálján** hello felső sávon.
> 
> 

## <a name="edit-page-contents"></a>Lap hello tartalom szerkesztése

toosee hello lista az összes meglévő tartalom lap, kattintson a **tartalom** a hello **fejlesztői portálján** hello publisher portál menüjében.

![Tartalom kezelése][api-management-customization-manage-content]

Kattintson a hello **üdvözlő** lapon tooedit hello kezdőlapján hello developer portálon jelenik meg. Módosítások hello kívánja, tekintse meg őket, ha szükséges, és kattintson **közzététele most** toomake őket látható tooeveryone.

> hello kezdőlap elrendezésű egy speciális, amely lehetővé teszi, hogy a fejléc toodisplay hello tetején. A fejléc olyan nem szerkeszthető a hello **tartalom** szakasz. tooedit a fejléc, kattintson a **Widgeteket** a hello **fejlesztői portálján** menüjében válassza **kezdőlap** a hello **aktuális réteg** legördülő listáról, és nyissa meg hello **szalagcím** elem alatt hello **szakasz kiemelt**. Ez a widget hello tartalma csakúgy, mint bármely más lapján módosítható.
> 
> 

## <a name="next-steps"></a>Következő lépések
* [Frissítés hello stílusok hello fejlesztői portálján keresztül használt elemei][customize-styles]
* [Módosíthatja a hello hello portál által létrehozott lapok használt] [ portal-templates] (pl. API docs, termékek, felhasználói hitelesítés, stb.)

[Structure of developer portal pages]: #page-structure
[Modifying hello contents of a layout widget]: #modify-layout-widget
[Edit hello contents of a page]: #edit-page-contents
[Next steps]: #next-steps

[modify-content-layout]: api-management-modify-content-layout.md
[customize-styles]: api-management-customize-styles.md
[portal-templates]: api-management-developer-portal-templates.md

[api-management-customization-widget-structure]: ./media/api-management-modify-content-layout/portal-widget-structure.png
[api-management-management-console]: ./media/api-management-modify-content-layout/api-management-management-console.png
[api-management-widgets-header]: ./media/api-management-modify-content-layout/api-management-widgets-header.png
[api-management-customization-manage-content]: ./media/api-management-modify-content-layout/api-management-customization-manage-content.png
