---
title: "Fájlok tömörítése Azure CDN szolgáltatás használata a jobb teljesítmény érdekében |} Microsoft Docs"
description: "Ismerje meg, hogyan lehet fokozni az fájl átviteli sebesség és az Azure CDN szolgáltatás használata a fájlok tömörítésével lap betöltési teljesítményét növeli."
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
ms.openlocfilehash: 7546650e6096a880f4fb4d0c94dd4ecc00b70160
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="improve-performance-by-compressing-files-in-azure-cdn"></a>A jobb teljesítmény érdekében az Azure CDN fájlok tömörítése
Tömörítés a fájl átviteli sebesség növelése és a lap betöltése teljesítmény növeléséhez a fájlméret, mielőtt a kiszolgáló egyszerű és hatékony módszer. Csökkenti a sávszélességgel kapcsolatos költségek, és gyorsabban élményt nyújt a felhasználók számára.

Két módon lehet engedélyezni a tömörítést:

* Tömörítésének engedélyezéséhez a forrás kiszolgálón, eset a CDN a tömörített fájlok keresztül továbbítja, és tömörített fájlok átadása az azokat kérő ügyfeleknek.
* Közvetlenül a CDN peremhálózati kiszolgálóinak, amelyben eset a CDN tömöríti a fájlok engedélyezni a tömörítést, és a végfelhasználók számára, akkor is, ha a forráskiszolgáló nem tömöríti szolgálnak.

> [!IMPORTANT]
> CDN-konfigurációs módosítások a hálózaton belüli propagálásához egy kis idő múlva.  A <b>Akamai Azure CDN</b> -profilok propagálása általában fejeződik be, egy perc alatt.  A <b>Azure CDN Verizon</b> profilok, általában látni fogja a módosítások alkalmazásához 90 percen belül.  Ha az első alkalommal beállítása tömörítés a CDN-végpont, vegye figyelembe, hogy a tömörítési beállítások propagálása előtt hibaelhárítási POP az 1 – 2 óra várakozás
> 
> 

## <a name="enabling-compression"></a>Tömörítés engedélyezése
> [!NOTE]
> A Standard és prémium szintű CDN rétegek funkcionalitása azonos tömörítést, de a felhasználói felület eltér.  Standard és prémium szintű CDN rétegek közötti különbségekről további információkért lásd: [Azure CDN áttekintése](cdn-overview.md).
> 
> 

### <a name="standard-tier"></a>Standard csomag
> [!NOTE]
> Ez a szakasz vonatkozik **Azure CDN Standard verizon** és **Azure CDN Standard Akamai** profilok.
> 
> 

1. A CDN-profil lapon kattintson a CDN-végpont felügyelni kíván.
   
    ![CDN-profil lap végpontok](./media/cdn-file-compression/cdn-endpoints.png)
   
    A CDN-végpont lap nyílik meg.
2. Kattintson a **konfigurálása** gombra.
   
    ![CDN-profil lapon kezelheti a gomb](./media/cdn-file-compression/cdn-config-btn.png)
   
    A CDN-konfiguráció lapon nyílik meg.
3. Kapcsolja be a **tömörítés**.
   
    ![CDN-tömörítési beállítások](./media/cdn-file-compression/cdn-compress-standard.png)
4. Használja az alapértelmezett típusokat, vagy módosítsa a listát fájltípusok hozzáadásával.
   
   > [!TIP]
   > Lehetséges, közben nem ajánlott tömörítés alkalmazása tömörített formátumban, például ZIP, MP3, MP4, JPG stb.
   > 
   > 
5. A módosítások elvégzése után kattintson a **mentése** gombra.

### <a name="premium-tier"></a>Premium szintű csomag
> [!NOTE]
> Ez a szakasz vonatkozik **verizon Azure CDN Premium** profilok.
> 
> 

1. A CDN-profil lapon kattintson a **kezelése** gombra.
   
    ![CDN-profil lapon kezelheti a gomb](./media/cdn-file-compression/cdn-manage-btn.png)
   
    Megnyitja a CDN-felügyeleti portálon.
2. Vigye a **HTTP nagy** lapra, és vigye a **gyorsítótár beállításainak** menü.  Kattintson a **tömörítés**.

    ![Fájl tömörítése kiválasztása](./media/cdn-file-compression/cdn-compress-select.png)
   
    Tömörítési beállítások jelennek meg.
   
    ![Fájl tömörítése](./media/cdn-file-compression/cdn-compress-files.png)
3. Tömörítésének engedélyezéséhez kattintson a **tömörítés engedélyezve** választógombot.  A MIME-típusok kívánja tömörítése (szóközök nélkül) vesszővel tagolt lista formájában adja meg a **fájltípusok** szövegmező.
   
   > [!TIP]
   > Lehetséges, közben nem ajánlott tömörítés alkalmazása tömörített formátumban, például ZIP, MP3, MP4, JPG stb. 
   > 
   > 
4. A módosítások elvégzése után kattintson a **frissítés** gombra.

## <a name="compression-rules"></a>Tömörítés szabályok
Ezek a táblázatok ismertetik Azure CDN tömörítési viselkedés minden forgatókönyvhöz.

> [!IMPORTANT]
> A **Azure CDN Verizon** (Standard és prémium), csak a megfelelő fájlok tömörítése.  Jogosult tömörítési, egy fájlt kell:
> 
> * Lehet nagyobb, mint 128 bájt.
> * Lehet kisebb, mint 1 MB.
> 
> A **Akamai Azure CDN**, tömörítési fájlok kerülnek.
> 
> Az összes Azure CDN-termékek, a kell lennie a MIME-típust, amelyet [tömörítési konfigurált](#enabling-compression).
> 
> **Verizon Azure CDN** (Standard és prémium) támogatási profilok **gzip** (GNU zip), **deflate**, **bzip2**, vagy **br**(Brotli) kódolást. A kódolási Brotli, a tömörítés csak peremére végezhető el. Az ügyfélböngészőnek Brotli kódolási kérelmet kell küldenie, és a tömörített eszköz kell tömörített a forrás oldalon először. 
>
>**Akamai Azure CDN** profilok támogatása csak **gzip** kódolást.
> 
> **Akamai Azure CDN** végpontok mindig kérjen **gzip** fájlt a forrásból, függetlenül az ügyfélkérés kódolású. 

### <a name="compression-disabled-or-file-is-ineligible-for-compression"></a>A tömörítés le van tiltva, vagy a fájl nem tömörítés
| Az ügyfél kért formátum (keresztül Accept-Encoding fejlécnek) | Gyorsítótárazott fájlformátum | Az ügyfél CDN válasz | Megjegyzések |
| --- | --- | --- | --- |
| Tömörített |Tömörített |Tömörített | |
| Tömörített |Az Uncompressed |Az Uncompressed | |
| Tömörített |Nem gyorsítótárazott |Tömörített és tömörítetlen |Forrás válasz függ |
| Az Uncompressed |Tömörített |Az Uncompressed | |
| Az Uncompressed |Az Uncompressed |Az Uncompressed | |
| Az Uncompressed |Nem gyorsítótárazott |Az Uncompressed | |

### <a name="compression-enabled-and-file-is-eligible-for-compression"></a>A tömörítés engedélyezve van és a fájl nem jogosult az tömörítés
| Az ügyfél kért formátum (keresztül Accept-Encoding fejlécnek) | Gyorsítótárazott fájlformátum | Az ügyfél CDN válasz | Megjegyzések |
| --- | --- | --- | --- |
| Tömörített |Tömörített |Tömörített |CDN transcodes támogatott formátumok közötti |
| Tömörített |Az Uncompressed |Tömörített |CDN tömörítési hajt végre. |
| Tömörített |Nem gyorsítótárazott |Tömörített |CDN tömörítési hajt végre, ha a forrás visszaküldi tömörítetlen.  **Verizon Azure CDN** átadja az első kérésre tömörítetlen fájl és majd tömöríti és gyorsítótárazza a További kérelmeknél fájlt.  A fájlok `Cache-Control: no-cache` fejléc soha nem tömöríthetők. |
| Az Uncompressed |Tömörített |Az Uncompressed |CDN kitömörítés hajt végre. |
| Az Uncompressed |Az Uncompressed |Az Uncompressed | |
| Az Uncompressed |Nem gyorsítótárazott |Az Uncompressed | |

## <a name="media-services-cdn-compression"></a>Media Services-CDN tömörítés
A Media Services CDN streamvégpontok engedélyezve van, tömörítés engedélyezve van-e alapértelmezés szerint a következő tartalomtípus: alkalmazás/vnd.ms-sstr + xml, application/dash+xml,application/vnd.apple.mpegurl, alkalmazás vagy f4m + xml. Ön nem engedélyezését vagy letiltását az említett típusa, az Azure portál használatával tömörítése.  

## <a name="see-also"></a>Lásd még:
* [CDN-fájltömörítés hibaelhárítása](cdn-troubleshoot-compression.md)    

