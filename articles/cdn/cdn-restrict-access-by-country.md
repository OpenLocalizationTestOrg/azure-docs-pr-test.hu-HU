---
title: "Azure CDN-tartalom ország aaaRestrict |} Microsoft Docs"
description: "Ismerje meg, hogyan toorestrict hozzáférés tooyour Azure CDN tartalom használatával hello szolgáltatás Geo-szűrés."
services: cdn
documentationcenter: 
author: lichard
manager: akucer
editor: 
ms.assetid: 12c17cc5-28ee-4b0b-ba22-2266be2e786a
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: rli
ms.openlocfilehash: ffdd994612b6c9cfbf1a6e29d260709b4afa86e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="restrict-azure-cdn-content-by-country"></a>Ország Azure CDN-tartalom korlátozása

## <a name="overview"></a>Áttekintés
Ha egy felhasználó alapértelmezés szerint a tartalmat igényel, függetlenül attól, hol hello felhasználói kérelmet ezt a kiszolgált hello tartalom. Bizonyos esetekben érdemes lehet toorestrict tooyour tartalom elérése ország. Ez a témakör azt ismerteti, hogyan toouse hello **földrajzi-szűrés** rendelés tooconfigure hello szolgáltatás tooallow blokkolja a hozzáférést ország szolgáltatása.

> [!IMPORTANT]
> hello Verizon és Akamai kínálnak hello azonos földrajzi-szűrés funkciót, de egy kis értékkülönbségeket te országhívó számokat támogatják-e. Tekintse meg a 3. lépés a hivatkozás toohello különbségek miatt.


Tooconfiguring megfontolást információ az ilyen típusú korlátozás: hello [szempontok](cdn-restrict-access-by-country.md#considerations) szakasz hello hello témakör végén.  

![Ország szerinti szűrés](./media/cdn-filtering/cdn-country-filtering-akamai.png)

## <a name="step-1-define-hello-directory-path"></a>1. lépés: Hello könyvtár elérési útjának megadása
Válassza ki a végpont hello portálon, és hello hello bal oldali navigációs toofind földrajzi-szűrés lapján található a szolgáltatás.

Ország szűrő konfigurálásakor meg kell adnia a hello relatív elérési út toohello hely toowhich felhasználók engedélyez vagy megtagadta a hozzáférést. Az összes fájl földrajzi-szűrés alkalmazhatja "/" vagy a kijelölt mappák elérési útjaiban "/ képek /" megadásával. Is alkalmazhat földrajzi-szűrés tooa egyfájlos hello fájl megadásával, és a záró perjelet kihagyva hello "/ képek/város.png".

Példa könyvtár elérési útja szűrő:

    /                                 
    /Photos/
    /Photos/Strasbourg/
      /Photos/Strasbourg/city.png

## <a name="step-2-define-hello-action-block-or-allow"></a>2. lépés: Hello művelet meghatározása: letiltása vagy engedélyezése
**Blokkolás:** hello felhasználók meghatározott országok elutasításra kerülne hozzáférés tooassets lekéri a rekurzív elérési út. Ha nincs más országban szűrési beállítások vannak konfigurálva, hogy a helyen, majd a többi felhasználó hozzáférése engedélyezett lesz.

**Engedélyezése:** hello felhasználók csak meghatározott országok engedélyezett hozzáférési tooassets lekéri a rekurzív elérési út.

## <a name="step-3-define-hello-countries"></a>3. lépés: Hello országok meghatározása
Válassza ki a hello országok tooblock szeretné, vagy lehetővé teszik a hello elérési útja. 

Például az blokkolás /Photos/Strasbourgban/hello szabály beleértve fájlok szűrheti:

    http://<endpoint>.azureedge.net/Photos/Strasbourg/1000.jpg
    http://<endpoint>.azureedge.net/Photos/Strasbourg/Cathedral/1000.jpg


### <a name="country-codes"></a>Országkód
Hello **földrajzi-szűrés** szolgáltatás használja, amelyből kérelmet engedélyezett vagy letiltott ország kódok toodefine hello országok biztonságos könyvtár. Hello országhívó számokat a rendszer megtalálta [Azure CDN országhívószámok](https://msdn.microsoft.com/library/mt761717.aspx). 

## <a id="considerations"></a>Szempontok
* Verizon too90 percig vagy módosításokat tooyour ország szűrési konfigurációs tootake hatás Akamai, néhány perc igénybe vehet.
* Ez a funkció nem támogatja a helyettesítő karakterek (például "*").
* hello földrajzi-szűrés konfiguráció hello relatív elérési út társított lesz rekurzív módon alkalmazza toothat elérési útja.
* Csak egy szabályt alkalmazott toohello lehetnek azonos relatív elérési út (nem hozható létre több országban szűrő adott pont toohello azonos relatív elérési útja. Azonban a mappa több országban szűrő állhat. Ennek az az ország szűrők toohello rekurzív jellege miatt. Más szóval a korábban konfigurált mappa almappája más országban szűrő rendelhetők.

