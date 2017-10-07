---
title: "Azure CDN gyorsítótár-viselkedést lekérdezési karakterláncokat tartalmazó aaaControl |} Microsoft Docs"
description: "Az Azure CDN lekérdezési karakterláncok gyorsítótárazásának hogyan fájljai toobe gyorsítótárazza a lekérdezési karakterláncokat tartalmazó vezérlő."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 17410e4f-130e-489c-834e-7ca6d6f9778d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: e7a138b2decec624a29eb703ad9a291d19c44ee8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings"></a>Vezérlő Azure CDN a lekérdezési karakterláncok gyorsítótárazásának
> [!div class="op_single_selector"]
> * [Standard](cdn-query-string.md)
> * [Verizon Azure CDN Premium](cdn-query-string-premium.md)
> 
> 

## <a name="overview"></a>Áttekintés
Lekérdezési karakterláncok gyorsítótárazásának hogyan fájljai toobe gyorsítótárazza a lekérdezési karakterláncokat tartalmazó vezérlő.

> [!IMPORTANT]
> hello Standard és prémium szintű CDN-termékek rendelkeznek hello azonos lekérdezési karakterlánc-gyorsítótár működése, de hello felhasználói felület eltér.  Ez a dokumentum ismerteti a hello felület **Azure CDN Standard Akamai** és **Azure CDN Standard verizon**.  A lekérdezési karakterláncok gyorsítótárazása a **verizon Azure CDN Premium**, lásd: [a lekérdezési karakterláncok - prémium szintű CDN gyorsítótárazási viselkedésének vezérlése kérelmek](cdn-query-string-premium.md).
> 
> 

Három módszer áll rendelkezésre:

* **Lekérdezési karakterláncok figyelmen kívül**: Ez az alapértelmezett mód hello.  hello CDN élcsomópont továbbítják hello lekérdezési karakterlánc hello kérelmező toohello forrásból hello első kérelem és a gyorsítótár hello eszköz.  Az összes további kérelmet, hogy az eszköz a hello élcsomópont szolgáltatott figyelmen kívül hagyja majd hello lekérdezési karakterlánc eléréséig hello gyorsítótárazott eszköz.
* **URL-címet a lekérdezési sztringek gyorsítótárazásának mellőzése**: Ebben a módban a lekérdezési karakterláncot tartalmazó kérelmek nem gyorsítótárazzák a hello CDN peremhálózati csomópontban.  hello élcsomópont hello eszköz lekérdezi közvetlenül hello forrásból, és átadja toohello kérelmező minden egyes kérelemmel.
* **Minden egyedi URL-cím gyorsítótárazása**: Ebben a módban a lekérdezési karakterlánc az egyes kérelmek kezeli a saját gyorsítótárával egyedi eszközként.  Például a hello eredeti adatforrást kérelmet kapott válasz hello *foo.ashx?q=bar* ehhez hello élcsomópont gyorsítótárazott és későbbi gyorsítótárak az, hogy ugyanazt a lekérdezési karakterláncot adott vissza.  A kérelem *foo.ashx?q=somethingelse* volna a saját toolive idővel külön eszközként gyorsítótárazható.

## <a name="changing-query-string-caching-settings-for-standard-cdn-profiles"></a>Lekérdezési karakterláncok gyorsítótárazásának standard szintű CDN-profil beállításainak módosítása
1. A CDN-profil panelje hello kattintson az hello CDN-végpont toomanage kívánja.
   
    ![CDN-profil blade-végpontok](./media/cdn-query-string/cdn-endpoints.png)
   
    CDN-végpont hello panelje megnyílik.
2. Kattintson a hello **konfigurálása** gombra.
   
    ![CDN-profil panelje kezelése gomb](./media/cdn-query-string/cdn-config-btn.png)
   
    hello CDN konfigurációs panel nyílik meg.
3. Válasszon egy beállítást a hello **lekérdezési sztringek gyorsítótárazásának** legördülő menüből.
   
    ![A CDN a lekérdezési karakterláncban a gyorsítótár](./media/cdn-query-string/cdn-query-string.png)
4. Miután választott, kattintson a hello **mentése** gombra.

> [!IMPORTANT]
> hello módosításait nem lehet azonnal láthatóak, mivel némi időre van szükség hello regisztrációs toopropagate hello CDN keresztül.  Az <b>Akamai Azure CDN</b> típusú profilok propagálása általában egy percen belül befejeződik.  A <b>Verizon Azure CDN</b> típusú profilok propagálása általában 90 percen belül befejeződik, ám egyes esetekben több időt is igénybe vehet.
> 
> 

