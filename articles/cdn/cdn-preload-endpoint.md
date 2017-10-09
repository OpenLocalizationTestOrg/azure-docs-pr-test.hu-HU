---
title: "Azure CDN-végpont aaaPre terhelési eszközök |} Microsoft Docs"
description: "Ismerje meg, hogyan toopre terhelésű gyorsítótárazott tartalom Azure CDN-végpont."
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 5ea3eba5-1335-413e-9af3-3918ce608a83
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 08ac4b834f1ac8ce59d22e65fa8adea11bafcf17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="pre-load-assets-on-an-azure-cdn-endpoint"></a>Eszközök előzetes betöltése Azure CDN-végponton
[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Alapértelmezés szerint eszközök először gyorsítótárba kerüljenek-e, erre felkérést kapnak. Ez azt jelenti, hogy minden egyes régió hello első kérelmet hosszabb időt vehet igénybe, mivel hello peremhálózati kiszolgálóinak nem kell hello tartalom gyorsítótárazva, és tooforward hello kérelem toohello eredeti kiszolgálóra lesz szüksége. Előre a tartalom betöltése elkerülhető az első találati késés.

Továbbá tooproviding jobb felhasználói élmény, a gyorsítótárazott eszközök előzetes betöltése hálózati forgalmat a hello eredeti kiszolgálóra is csökkentheti.

> [!NOTE]
> Előzetes betöltését az eszközök akkor hasznos, ha nagy események vagy tartalom, amely elérhetővé válik egy időben elérhető tooa sok felhasználó, például egy új movie kiadás vagy szoftverfrissítés.
> 
> 

Ez az oktatóanyag bemutatja, hogyan előzetes betöltését a gyorsítótárazott tartalom minden Azure CDN peremhálózati csomópontján.

## <a name="walkthrough"></a>Útmutatás
1. A hello [Azure Portal](https://portal.azure.com), keresse meg a terhelési toopre kívánja hello végpont tartalmazó toohello CDN-profil.  hello-profil panelje megnyílik.
2. Kattintson a hello végpont hello listában.  hello végpont panel nyílik meg.
3. Hello CDN-végpont panelen kattintson a hello Betöltés gombra.
   
    ![CDN-végpont panelje](./media/cdn-preload-endpoint/cdn-endpoint-blade.png)
   
    hello terhelés panel nyílik meg.
   
    ![CDN-betöltési panelje](./media/cdn-preload-endpoint/cdn-load-blade.png)
4. Hello elérési útját adja meg minden egyes eszköz tooload kívánja (pl. `/pictures/kitten.png`) a hello **elérési** szövegmező.
   
   > [!TIP]
   > További **elérési** szövegmezők megjelenik a szöveges tooallow megadása után toobuild több eszközök listáját.  Eszközök hello listából hello három ponttal (…) gombra kattintva törölheti.
   > 
   > Elérési út lehet egy relatív URL-címet, amely megfelel a következő hello [reguláris kifejezés](https://msdn.microsoft.com/library/az24scfc.aspx):  
   > >Egy egyetlen fájl elérési útját betöltése `@"^(?:\/[a-zA-Z0-9-_.%=\u0020]+)+$"`;  
   > >A lekérdezési karakterlánc egyetlen fájl betöltése`@"^(?:\?[-_a-zA-Z0-9\/%:;=!,.\+'&\u0020]*)?$";`  
   > 
   > Minden eszköz a saját elérési utat kell rendelkeznie.  Nincs előre betöltése eszközök helyettesítő funkció sem.
   > 
   > 
   
    ![Betöltési gomb](./media/cdn-preload-endpoint/cdn-load-paths.png)
5. Kattintson a hello **terhelés** gombra.
   
    ![Betöltési gomb](./media/cdn-preload-endpoint/cdn-load-button.png)

> [!NOTE]
> CDN-profil percenként 10 vonatkozó korlátozása van. 50 elérési utat kérelmenként engedélyezettek. Mindegyik elérési útból útvonal-hossza legfeljebb 1024 karakterből állhat.
> 
> 

## <a name="see-also"></a>Lásd még:
* [Az Azure CDN-végpont törlése](cdn-purge-endpoint.md)
* [Az Azure CDN REST API-referencia - kiürítése, vagy a végpont előzetes betöltése](https://msdn.microsoft.com/library/mt634451.aspx)

