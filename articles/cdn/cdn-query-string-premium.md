---
title: "lekérdezési karakterláncok - prémium szintű Azure CDN gyorsítótárazási működését aaaControl |} Microsoft Docs"
description: "Az Azure CDN lekérdezési karakterláncok gyorsítótárazásának hogyan fájljai toobe gyorsítótárazza a lekérdezési karakterláncokat tartalmazó vezérlő."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 99db4a85-4f5f-431f-ac3a-69e05518c997
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 5c97cf0230ae13fd378bfce49481f3135a5ef101
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings---premium"></a>Vezérlő Azure CDN a lekérdezési karakterláncok - prémium gyorsítótárazásának
> [!div class="op_single_selector"]
> * [Standard](cdn-query-string.md)
> * [Verizon Azure CDN Premium](cdn-query-string-premium.md)
> 
> 

## <a name="overview"></a>Áttekintés
Lekérdezési karakterláncok gyorsítótárazásának hogyan fájljai toobe gyorsítótárazza a lekérdezési karakterláncokat tartalmazó vezérlő.

> [!IMPORTANT]
> hello Standard és prémium szintű CDN-termékek rendelkeznek hello azonos lekérdezési karakterlánc-gyorsítótár működése, de hello felhasználói felület eltér.  Ez a dokumentum ismerteti a hello felület **verizon Azure CDN Premium**.  A lekérdezési karakterláncok gyorsítótárazása a **Azure CDN Standard Akamai** és **Azure CDN Standard verizon**, lásd: [lekérdezési karakterláncokat tartalmazó kérelmek gyorsítótárazási CDN viselkedésének vezérlése](cdn-query-string.md).
> 
> 

Három módszer áll rendelkezésre:

* **Standard-gyorsítótár**: Ez az alapértelmezett mód hello.  hello CDN élcsomópont továbbítják hello lekérdezési karakterlánc hello kérelmező toohello forrásból hello első kérelem és a gyorsítótár hello eszköz.  Az összes további kérelmet, hogy az eszköz a hello élcsomópont szolgáltatott figyelmen kívül hagyja majd hello lekérdezési karakterlánc eléréséig hello gyorsítótárazott eszköz.
* **no-cache**: Ebben a módban a lekérdezési karakterláncot tartalmazó kérelmek nem gyorsítótárazzák a hello CDN peremhálózati csomópontban.  hello élcsomópont hello eszköz lekérdezi közvetlenül hello forrásból, és átadja toohello kérelmező minden egyes kérelemmel.
* **egyedi gyorsítótár**: Ebben a módban a lekérdezési karakterlánc az egyes kérelmek kezeli a saját gyorsítótárával egyedi eszközként.  Például a hello eredeti adatforrást kérelmet kapott válasz hello *foo.ashx?q=bar* ehhez hello élcsomópont gyorsítótárazott és későbbi gyorsítótárak az, hogy ugyanazt a lekérdezési karakterláncot adott vissza.  A kérelem *foo.ashx?q=somethingelse* volna a saját toolive idővel külön eszközként gyorsítótárazható.

## <a name="changing-query-string-caching-settings-for-premium-cdn-profiles"></a>Lekérdezési karakterláncok gyorsítótárazásának prémium szintű CDN-profil beállításainak módosítása
1. A CDN-profil panelje hello, kattintson a hello **kezelése** gombra.
   
    ![CDN-profil panelje kezelése gomb](./media/cdn-query-string-premium/cdn-manage-btn.png)
   
    Megnyílik a hello CDN felügyeleti portálon.
2. Hello az egérmutatót **HTTP nagy** fülre, majd az egérmutatót hello **gyorsítótár beállításainak** menü.  Kattintson a **lekérdezési karakterlánc-gyorsítótár**.
   
    Lekérdezési karakterlánc gyorsítótárazási beállítások jelennek meg.
   
    ![A CDN a lekérdezési karakterláncban a gyorsítótár](./media/cdn-query-string-premium/cdn-query-string.png)
3. Miután választott, kattintson a hello **frissítés** gombra.

> [!IMPORTANT]
> hello módosításait nem lehet azonnal láthatóak, mivel némi időre van szükség hello regisztrációs toopropagate hello CDN keresztül.  A <b>Verizon Azure CDN</b> típusú profilok propagálása általában 90 percen belül befejeződik, ám egyes esetekben több időt is igénybe vehet.
> 
> 

