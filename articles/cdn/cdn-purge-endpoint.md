---
title: "az Azure CDN-végpont aaaPurge |} Microsoft Docs"
description: "Ismerje meg, hogyan toopurge minden gyorsítótárazott tartalom az Azure CDN-végponton."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 0b50230b-fe82-4740-90aa-95d4dde8bd4f
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: a09f4a49aa1e2d7655ecae44b5126c11c28fd599
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="purge-an-azure-cdn-endpoint"></a>Az Azure CDN-végpont törlése
## <a name="overview"></a>Áttekintés
Az Azure CDN peremhálózati csomópontok eszközök gyorsítótárazhatják az eléréséig hello eszköz idő-Élettartam (TTL).  Hello eszköz TTL lejárata után Ha egy ügyfél hello eszköz kér hello élcsomópont, hello élcsomópont frissített másolatát hello eszköz tooserve hello ügyfélkérés lekéri és frissítési hello gyorsítótárban tárolja.

hello bevált gyakorlat toomake meg arról, hogy a felhasználók mindig hello legfrissebb verzióját a eszközöket szerezze be az eszközök, az egyes frissítése, és új URL-jeiként közzététele tooversion.  CDN közvetlenül kér le új eszközök hello hello következő ügyfélkéréseket.  Egyes esetekben érdemes toopurge gyorsítótárba helyezték a tartalmat minden peremhálózati csomópontról és annak kikényszerítéséhez, azok összes tooretrieve új frissített eszközök.  Előfordulhat, hogy ennek lehet tooupdates tooyour webalkalmazás, vagy helytelen adatokat tartalmazó tooquickly frissítési eszközök.

> [!TIP]
> Vegye figyelembe, hogy csak kiürítése törli hello gyorsítótárba helyezték a tartalmat a hello CDN peremhálózati kiszolgálóinak.  Bármely alárendelt gyorsítótárak, például proxykiszolgáló, és helyi böngésző gyorsítótárát, előfordulhat, hogy továbbra is gyorsítótárazott másolatának tárolására szolgál hello fájlt.  Fontos tooremember a megadása után a fájl élettartamát idő.  Egy alárendelt ügyfél toorequest hello legújabb verzióját a fájl adjon neki egy egyedi nevet minden alkalommal, amikor végezzen frissítést, vagy ha kihasználja a kényszerítheti [lekérdezési karakterláncok gyorsítótárazása](cdn-query-string.md).  
> 
> 

Ez az oktatóanyag bemutatja, hogyan eszközök kiürítése a végpont összes peremhálózati csomópontjából.

## <a name="walkthrough"></a>Útmutatás
1. A hello [Azure Portal](https://portal.azure.com), keresse meg a toopurge kívánja hello végpont tartalmazó toohello CDN-profilt.
2. A CDN-profil panelje hello kattintson a hello végleges törlése gombra.
   
    ![CDN-profil panelje](./media/cdn-purge-endpoint/cdn-profile-blade.png)
   
    hello kiürítése panel nyílik meg.
   
    ![CDN-kiürítési panelje](./media/cdn-purge-endpoint/cdn-purge-blade.png)
3. A hello üríteni a panelen, hello szolgáltatás címét kívánja toopurge hello URL-cím legördülő listából válassza ki.
   
    ![Űrlap kiürítése](./media/cdn-purge-endpoint/cdn-purge-form.png)
   
   > [!NOTE]
   > Hello kattintva is megkapható toohello kiürítése panel **kiürítése** hello CDN-végpont panelje gombjára.  Ebben az esetben hello **URL-cím** mező ki van töltve a hello szolgáltatást, hogy adott végpont címe lesz.
   > 
   > 
4. Válassza ki, milyen eszközöket kívánja a hello toopurge peremhálózati csomópontok.  Ha tooclear az összes eszköz, kattintson hello **összes** jelölőnégyzetet.  Ellenkező esetben a típus hello elérési útjának minden egyes eszköz kívánja toopurge a hello **elérési** szövegmező. Hello elérési út által támogatott formátumok alatt.
    1. **Egyetlen URL-cím kiürítése**: hello teljes URL-címet, vagy anélkül hello fájlnévkiterjesztés, például megadásával eszköz egyedi kiürítése`/pictures/strasbourg.png`;`/pictures/strasbourg`
    2. **Helyettesítő karakteres kiürítés**: csillag (\*) helyettesítő karakter is használható. Törli az összes mappa, almappák és fájlok a végpont `/*` a hello elérési útját, vagy minden almappák és fájlok egy adott mappában törölhetők követ hello mappa megadásával `/*`, például`/pictures/*`.  Vegye figyelembe, hogy helyettesítő karakteres kiürítés nem támogatott Akamai Azure CDN által jelenleg. 
    3. **Legfelső szintű tartomány kiürítése**: kiürítése hello legfelső szintű hello végpont a "/" hello elérési úton.
   
   > [!TIP]
   > Elérési utak meg kell adni a kiürítési és kell lennie egy relatív URL-cím hello következő illeszkedő [reguláris kifejezés](https://msdn.microsoft.com/library/az24scfc.aspx). **Összes** és **helyettesítő karakteres kiürítés** nem támogatja a **Akamai Azure CDN** jelenleg.
   > > Egyetlen URL-cím kiürítése`@"^\/(?>(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/?)*)$";`  
   > > Lekérdezési karakterlánc`@"^(?:\?[-\@_a-zA-Z0-9\/%:;=!,.\+'&\(\)\u0020]*)?$";`  
   > > Helyettesítő karakteres kiürítés `@"^\/(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/)*\*$";`. 
   > 
   > További **elérési** szövegmezők megjelenik a szöveges tooallow megadása után toobuild több eszközök listáját.  Eszközök hello listából hello három ponttal (…) gombra kattintva törölheti.
   > 
5. Kattintson a hello **kiürítése** gombra.
   
    ![Kiürítése gomb](./media/cdn-purge-endpoint/cdn-purge-button.png)

> [!IMPORTANT]
> Kiürítése kérelmek igénybe vehet a körülbelül 2-3 percet tooprocess **Azure CDN Verizon** (Standard és prémium), és körülbelül 7 perc **Akamai Azure CDN**.  Az Azure CDN rendelkezik maximális száma 50 egyidejű kérelmek kiüríteni egy adott időpontban. 
> 
> 

## <a name="see-also"></a>Lásd még:
* [Eszközök előzetes betöltése Azure CDN-végponton](cdn-preload-endpoint.md)
* [Az Azure CDN REST API-referencia - kiürítése, vagy a végpont előzetes betöltése](https://msdn.microsoft.com/library/mt634451.aspx)

