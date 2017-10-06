---
title: "az Azure CDN aaaTroubleshooting fájltömörítés |} Microsoft Docs"
description: "Azure CDN fájltömörítés elhárítása."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: a6624e65-1a77-4486-b473-8d720ce28f8b
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: f00b98beaf6b3b3cd30108ece65a8191edc06ff5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-cdn-file-compression"></a>A CDN-fájlok tömörítési hibáinak elhárítása
Ez a cikk segít elhárítása [CDN fájltömörítés](cdn-improve-performance.md).

Ha ez a cikk bármely pontján további segítségre van szüksége, forduljon az Azure-szakértők hello [MSDN Azure hello és fórumok Stack Overflow hello](https://azure.microsoft.com/support/forums/). Másik lehetőségként is fájl is az Azure támogatási incidens. Nyissa meg toohello [Azure támogatási webhelyén](https://azure.microsoft.com/support/options/) kattintson **támogatás**.

## <a name="symptom"></a>Jelenség
A végponthoz tömörítés engedélyezve van, de tömörített fájlok vannak visszaküldött.

> [!TIP]
> toocheck kell-e a fájlok tömörített vannak visszaadott, egy eszköz, például toouse [Fiddler](http://www.telerik.com/fiddler) vagy a böngésző [fejlesztői eszközök](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/).  Jelölőnégyzet hello HTTP-válaszfejlécek hibát adott vissza a gyorsítótárazott CDN tartalom.  Ha nincs nevű fejléc `Content-Encoding` értékkel rendelkező **gzip**, **bzip2**, vagy **deflate**, a tartalom tömörített.
> 
> ![Tartalom-Encoding fejlécnek](./media/cdn-troubleshoot-compression/cdn-content-header.png)
> 
> 

## <a name="cause"></a>Ok
Van több lehetséges oka, beleértve:

* a kért hello tartalma nem jogosult a tömörítést.
* Tömörítés hello nincs engedélyezve a kért fájl típusa.
* hello HTTP-kérelem nem tartalmazza a kért egy érvényes tömörítési típus fejléc.

## <a name="troubleshooting-steps"></a>Hibaelhárítási lépések
> [!TIP]
> Csakúgy, mint a központi telepítése új végpontok, CDN konfigurációs módosításokat bizonyos idő toopropagate hello a hálózaton keresztül igénybe vehet.  Általában a módosításai érvényesek lesznek 90 percen belül.  Ha hello első alkalommal tömörítés a CDN-végpont kiépítését, érdemes 1 – 2 óra toobe meg arról, hogy hello tömörítési beállítások propagálása toohello POP vár. 
> 
> 

### <a name="verify-hello-request"></a>Hello kérelem ellenőrzése
Először azt kell tennie hello kérelem gyors megerősítést ellenőrzése.  A böngésző használható [fejlesztői eszközök](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) tooview hello küldött kéréseket.

* Ellenőrizze a hello kérelmet küld a tooyour végponti URL-cím, `<endpointname>.azureedge.net`, és nem a forrása.
* Ellenőrizze a hello kérelem tartalmaz egy **elfogadás kódolás** fejlécet, és hello értéke, hogy a fejléc tartalmazza **gzip**, **deflate**, vagy **bzip2** .

> [!NOTE]
> **Akamai Azure CDN** csak támogatási profilok **gzip** kódolást.
> 
> 

![CDN-kérelemfejlécekben](./media/cdn-troubleshoot-compression/cdn-request-headers.png)

### <a name="verify-compression-settings-standard-cdn-profile"></a>Ellenőrizze a tömörítési beállítások (standard szintű CDN-profil)
> [!NOTE]
> Ez a lépés csak akkor alkalmazza, ha a CDN-profilt egy **Azure CDN Standard verizon** vagy **Azure CDN Standard Akamai** profil. 
> 
> 

Keresse meg a hello tooyour végpont [Azure-portálon](https://portal.azure.com) hello kattintson **konfigurálása** gombra.

* Ellenőrizze a tömörítés engedélyezve van.
* Ellenőrizze a tartalom tömörített toobe hello tömörített formátumok listája szerepel hello hello MIME-típust.

![CDN-tömörítési beállítások](./media/cdn-troubleshoot-compression/cdn-compression-settings.png)

### <a name="verify-compression-settings-premium-cdn-profile"></a>Ellenőrizze a tömörítési beállítások (prémium szintű CDN-profil)
> [!NOTE]
> Ez a lépés csak akkor alkalmazza, ha a CDN-profilt egy **verizon Azure CDN Premium** profil.
> 
> 

Keresse meg a hello tooyour végpont [Azure-portálon](https://portal.azure.com) hello kattintson **kezelése** gombra.  Ekkor megnyílik a hello kiegészítő portálon.  Hello az egérmutatót **HTTP nagy** fülre, majd az egérmutatót hello **gyorsítótár beállításainak** menü.  Kattintson a **tömörítés**. 

* Ellenőrizze a tömörítés engedélyezve van.
* Ellenőrizze a hello **fájltípusok** lista tartalmazza-e egy vesszővel tagolt listája (szóközök nélkül) a MIME-típusok.
* Ellenőrizze a tartalom tömörített toobe hello tömörített formátumok listája szerepel hello hello MIME-típust.

![CDN premium tömörítési beállítások](./media/cdn-troubleshoot-compression/cdn-compression-settings-premium.png)

### <a name="verify-hello-content-is-cached"></a>Ellenőrizze a hello tartalom található a gyorsítótárban
> [!NOTE]
> Ez a lépés csak akkor alkalmazza, ha a CDN-profilt egy **Azure CDN Verizon** profil (Standard vagy prémium).
> 
> 

A böngésző fejlesztői eszközök segítségével ellenőrizze a hello fejlécek tooensure hello válaszfájl tárolja a rendszer hello régió, ahol kérik.

* Ellenőrizze a hello **Server** válaszfejlécet.  hello fejléc kell hello formátum **Platform (POP-kiszolgáló azonosítója)**, hello a következő példában látható módon.
* Ellenőrizze a hello **X-gyorsítótár** válaszfejlécet.  olvassa el a hello fejléc **TALÁLATI**.  

![CDN-válaszfejlécek](./media/cdn-troubleshoot-compression/cdn-response-headers.png)

### <a name="verify-hello-file-meets-hello-size-requirements"></a>Ellenőrizze a hello fájl megfelel hello mérete
> [!NOTE]
> Ez a lépés csak akkor alkalmazza, ha a CDN-profilt egy **Azure CDN Verizon** profil (Standard vagy prémium).
> 
> 

toobe tömörítési támogatható, egy fájl hello mérete követelményeknek kell megfelelniük:

* Nagyobb, mint 128 bájt.
* 1 MB-nál kisebb.

### <a name="check-hello-request-at-hello-origin-server-for-a-via-header"></a>Hello származási kiszolgálóján hello kérés ellenőrzéséhez egy **keresztül** fejléc
Hello **keresztül** HTTP-fejléc azt jelzi, hogy kérelem hello toohello webkiszolgálón van átadta-e a proxykiszolgáló.  Microsoft IIS-webkiszolgálókkal alapértelmezés szerint nem a válaszok tömörítésére vonatkozó Ha hello kérelmet tartalmaz egy **keresztül** fejléc.  toooverride ezt a viselkedést hello következőket hajthatja végre:

* **Az IIS 6**: [beállítása HcNoCompressionForProxies = "FALSE" hello IIS-metaadatbázis tulajdonságai](https://msdn.microsoft.com/library/ms525390.aspx)
* **Az IIS 7 és legfeljebb**: [mind **noCompressionForHttp10** és **noCompressionForProxies** tooFalse hello-kiszolgálói konfigurációban](http://www.iis.net/configreference/system.webserver/httpcompression)

