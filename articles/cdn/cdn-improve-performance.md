---
title: "Azure CDN szolgáltatás használata a fájlok tömörítésével aaaImprove teljesítmény |} Microsoft Docs"
description: "Ismerje meg, hogyan tooimprove fájl átvitele az Azure CDN szolgáltatás használata a fájlok tömörítésével sebesség és növeli a lap betöltése teljesítményét."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: af1cddff-78d8-476b-a9d0-8c2164e4de5d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 9a1698b6c29233c2e2e6fb17cdd8e919ae8bfa1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="improve-performance-by-compressing-files-in-azure-cdn"></a>A jobb teljesítmény érdekében az Azure CDN fájlok tömörítése
Tömörítés egyszerű és hatékony módszer tooimprove fájl átviteli sebesség és növekedése lap betöltési teljesítményét előtte méretének csökkentésével küldi hello kiszolgáló. Csökkenti a sávszélességgel kapcsolatos költségek, és gyorsabban élményt nyújt a felhasználók számára.

Két módon tooenable tömörítése:

* Engedélyezheti tömörítés a forrás kiszolgálón, ebben az esetben hello hello CDN átmegy tömörített fájlok és tömörített fájlok tooclients azokat kérő biztosításához.
* Engedélyezheti a tömörítést közvetlenül a CDN peremhálózati kiszolgálóinak, eset hello CDN tömöríti fájlok hello és azokat tooend felhasználók kiszolgáló akkor is, ha azok nem tömöríti az hello eredeti kiszolgálóra.

> [!IMPORTANT]
> CDN konfigurációs módosítások lépnek bizonyos idő toopropagate hello a hálózaton keresztül.  A <b>Akamai Azure CDN</b> -profilok propagálása általában fejeződik be, egy perc alatt.  A <b>Azure CDN Verizon</b> profilok, általában látni fogja a módosítások alkalmazásához 90 percen belül.  Ha hello első alkalommal tömörítés a CDN-végpont kiépítését, érdemes lehet 1 – 2 óra toobe meg arról, hogy hello tömörítési beállítások propagálása előtt hibaelhárítási toohello POP vár
> 
> 

## <a name="enabling-compression"></a>Tömörítés engedélyezése
> [!NOTE]
> hello Standard és prémium szintű CDN rétegek biztosítanak hello tömörítési ugyanezeket a funkciókat, de hello felhasználói felület eltér.  Standard és prémium szintű CDN rétegek hello különbségei kapcsolatos további információkért lásd: [Azure CDN áttekintése](cdn-overview.md).
> 
> 

### <a name="standard-tier"></a>Standard csomag
> [!NOTE]
> Ez a szakasz vonatkozik túl**Azure CDN Standard verizon** és **Azure CDN Standard Akamai** profilok.
> 
> 

1. Hello CDN profil lapon kattintson hello CDN-végpont toomanage kívánja.
   
    ![CDN-profil lap végpontok](./media/cdn-file-compression/cdn-endpoints.png)
   
    hello CDN-végpont lap nyílik meg.
2. Kattintson a hello **konfigurálása** gombra.
   
    ![CDN-profil lapon kezelheti a gomb](./media/cdn-file-compression/cdn-config-btn.png)
   
    hello CDN konfigurálása lapon nyílik meg.
3. Kapcsolja be a **tömörítés**.
   
    ![CDN-tömörítési beállítások](./media/cdn-file-compression/cdn-compress-standard.png)
4. Hello alapértelmezett típusát használhatja, vagy módosítsa a hello lista fájltípusok hozzáadásával.
   
   > [!TIP]
   > While lehetséges, nem ajánlott tooapply tömörítési toocompressed formátumban, például ZIP, MP3, MP4, JPG stb.
   > 
   > 
5. A módosítások elvégzése után kattintson a hello **mentése** gombra.

### <a name="premium-tier"></a>Premium szintű csomag
> [!NOTE]
> Ez a szakasz vonatkozik túl**verizon Azure CDN Premium** profilok.
> 
> 

1. Hello CDN-profil lapján, kattintson a hello **kezelése** gombra.
   
    ![CDN-profil lapon kezelheti a gomb](./media/cdn-file-compression/cdn-manage-btn.png)
   
    Megnyílik a hello CDN felügyeleti portálon.
2. Hello az egérmutatót **HTTP nagy** fülre, majd az egérmutatót hello **gyorsítótár beállításainak** menü.  Kattintson a **tömörítés**.

    ![Fájl tömörítése kiválasztása](./media/cdn-file-compression/cdn-compress-select.png)
   
    Tömörítési beállítások jelennek meg.
   
    ![Fájl tömörítése](./media/cdn-file-compression/cdn-compress-files.png)
3. Tömörítés engedélyezése hello kattintva **tömörítés engedélyezve** választógombot.  Adja meg a hello MIME-típusok vesszővel tagolt lista formájában (szóközök nélkül) a hello toocompress kívánja **fájltípusok** szövegmező.
   
   > [!TIP]
   > While lehetséges, nem ajánlott tooapply tömörítési toocompressed formátumban, például ZIP, MP3, MP4, JPG stb. 
   > 
   > 
4. A módosítások elvégzése után kattintson a hello **frissítés** gombra.

## <a name="compression-rules"></a>Tömörítés szabályok
Ezek a táblázatok ismertetik Azure CDN tömörítési viselkedés minden forgatókönyvhöz.

> [!IMPORTANT]
> A **Azure CDN Verizon** (Standard és prémium), csak a megfelelő fájlok tömörítése.  jogosult a tömörítési toobe, egy fájlt kell:
> 
> * Lehet nagyobb, mint 128 bájt.
> * Lehet kisebb, mint 1 MB.
> 
> A **Akamai Azure CDN**, tömörítési fájlok kerülnek.
> 
> Az összes Azure CDN-termékek, a kell lennie a MIME-típust, amelyet [tömörítési konfigurált](#enabling-compression).
> 
> **Verizon Azure CDN** (Standard és prémium) támogatási profilok **gzip** (GNU zip), **deflate**, **bzip2**, vagy **br**(Brotli) kódolást. A kódolási Brotli, hello tömörítés csak hello szélén végezhető el. hello ügyfélböngészőnek Brotli kódolási hello kérelmet kell küldenie és hello tömörített eszköz kell tömörített hello forrás oldalon először. 
>
>**Akamai Azure CDN** profilok támogatása csak **gzip** kódolást.
> 
> **Akamai Azure CDN** végpontok mindig kérjen **gzip** kódolású hello a forrásból, függetlenül attól, hello ügyfélkérés fájl. 

### <a name="compression-disabled-or-file-is-ineligible-for-compression"></a>A tömörítés le van tiltva, vagy a fájl nem tömörítés
| Az ügyfél kért formátum (keresztül Accept-Encoding fejlécnek) | Gyorsítótárazott fájlformátum | CDN-válasz toohello ügyfél | Megjegyzések |
| --- | --- | --- | --- |
| Tömörített |Tömörített |Tömörített | |
| Tömörített |Az Uncompressed |Az Uncompressed | |
| Tömörített |Nem gyorsítótárazott |Tömörített és tömörítetlen |Forrás válasz függ |
| Az Uncompressed |Tömörített |Az Uncompressed | |
| Az Uncompressed |Az Uncompressed |Az Uncompressed | |
| Az Uncompressed |Nem gyorsítótárazott |Az Uncompressed | |

### <a name="compression-enabled-and-file-is-eligible-for-compression"></a>A tömörítés engedélyezve van és a fájl nem jogosult az tömörítés
| Az ügyfél kért formátum (keresztül Accept-Encoding fejlécnek) | Gyorsítótárazott fájlformátum | CDN-válasz toohello ügyfél | Megjegyzések |
| --- | --- | --- | --- |
| Tömörített |Tömörített |Tömörített |CDN transcodes támogatott formátumok közötti |
| Tömörített |Az Uncompressed |Tömörített |CDN tömörítési hajt végre. |
| Tömörített |Nem gyorsítótárazott |Tömörített |CDN tömörítési hajt végre, ha a forrás visszaküldi tömörítetlen.  **Verizon Azure CDN** fázisok hello tömörítetlen fájl hello első kérésre, majd tömöríti és gyorsítótárak hello további kérelmeknél fájlt.  A fájlok `Cache-Control: no-cache` fejléc soha nem tömöríthetők. |
| Az Uncompressed |Tömörített |Az Uncompressed |CDN kitömörítés hajt végre. |
| Az Uncompressed |Az Uncompressed |Az Uncompressed | |
| Az Uncompressed |Nem gyorsítótárazott |Az Uncompressed | |

## <a name="media-services-cdn-compression"></a>Media Services-CDN tömörítés
A Media Services CDN streamvégpontok engedélyezve van, tömörítés engedélyezve van-e alapértelmezés szerint a következő tartalomtípus hello: alkalmazás/vnd.ms-sstr + xml, application/dash+xml,application/vnd.apple.mpegurl, alkalmazás vagy f4m + xml. Engedélyezi/letiltja hello tömörítése nem említett hello Azure portál segítségével.  

## <a name="see-also"></a>Lásd még:
* [CDN-fájltömörítés hibaelhárítása](cdn-troubleshoot-compression.md)    

