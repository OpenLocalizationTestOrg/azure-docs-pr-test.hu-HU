---
title: "Az Azure RemoteApp – olyan gyakori forgatókönyveket tartalmaz, a hálózati sávszélesség-használat tesztelése |} Microsoft Docs"
description: "Ismerje meg, mi a helyzet gyakori használati forgatókönyvek, amelyek segítségével kideríthesse, mi is a hálózati sávszélesség funkciókra van szüksége az Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 06417c75-0c63-4ecf-b9d1-66a7af6b7b91
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 8ad172a06e34cd0647079d787097cb2696cf116e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-remoteapp---testing-your-network-bandwidth-usage-with-some-common-scenarios"></a>Az Azure RemoteApp – olyan gyakori forgatókönyveket tartalmaz, a hálózati sávszélesség-használat tesztelése
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. A részletekért olvassa el a [bejelentést](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Azt a bemutatott [becsült Azure RemoteApp a sávszélesség-használat](remoteapp-bandwidth.md), a legjobb módja, hogy megtudja, mi az Azure RemoteApp a hálózat szempontjából néhány használati tesztek futtatásához. Ezeket a teszteket egy meghatározott ideig futtatni, és az egyes forgatókönyvek esetén szükség a sávszélesség mérését. Ha a funkció, a hálózati csomag adatvesztéssel és a hálózati jitter tudni, hogy az adott környezetben létrehozott hálózati mintázatokat is mérni is.

A sávszélesség-használat kiértékelésekor ne feledje, hogy használati változó a vállalaton belül a különböző felhasználók között. Például szöveg olvasók és írók általában használja felhasználók, amelyek a video-nál kisebb sávszélességet. A legjobb eredmények elérése érdekében tanulmányozza a saját felhasználói igényeket, és hozzon létre a következő esetekben egy kombinációja, amely a legjobban jelképezi a vállalatnál lévő felhasználóknál. Ne felejtse el [tekintse át a tényezőket, amelyek hatással lehet a sávszélesség-használat és a felhasználói élmény](remoteapp-bandwidthexperience.md) -, amely segít azonosítani a ideális tesztek.

Először olvassa el a teszteket, válassza ki a vegyes, és futtassa azokat. Az alábbi táblázat segítségével nyomon követheti a teljesítményt.

> [!NOTE]
> Ha a hálózati teszteléssel nem hajtható végre, vagy Önnek nincs ehhez az idő, tekintse meg a [alapvető hálózati sávszélesség becslése/javaslatok](remoteapp-bandwidthguidelines.md). A távolság változhat, azonban, ha Ön *is* saját tesztek futtatásához meg kell.
> 
> 

## <a name="the-usage-tests"></a>A használati tesztek
E vizsgálatok különböző rengeteg idő futtatása, és különböző funkciókat/funkcióit tesztelik, használja a hálózati sávszélességet. Ne felejtse el, válassza a teszt kombinációját legjobban megfelelő sablontípust a egyedi vállalati felhasználók.

### <a name="executivecomplex-powerpoint---run-for-900-1000-seconds"></a>Vezetői/komplex PowerPoint - 900-1000 másodpercig futtatása
A felhasználó megadja a 45-50 valósághű diák a Microsoft Office PowerPoint a teljes képernyős mód között. A diák képet, átmenetek (a animációk) és szín színátmenet, amelyek általában a vállalat hátterek tartalmazhat. A felhasználó egyes diák kell egy legalább 20 másodperc.

Ebben a forgatókönyvben hoz cellaveszteségi forgalmat, ha egy menüpontban húzással állíthatja be a következő menüpontban húzással állíthatja be a bemutató a átmenetek.

### <a name="simple-powerpoint---run-for-620-seconds"></a>Egyszerű PowerPoint - ~ 620 másodpercig futtatása
A felhasználó egy egyszerű PowerPoint-fájlhoz megadja körülbelül 30 diák a Microsoft Office PowerPoint a teljes képernyős módban. A diák több szöveg igényel mint végrehajtó/komplex PowerPoint forgatókönyvben és egyszerűbb hátterek és lemezképek (fekete diagramok). 

### <a name="internet-explorer---run-for-250-seconds"></a>Az Internet Explorer - legfeljebb 250 másodpercig futtatása
A felhasználó a webes böngészik, az Internet Explorer használatával. A felhasználó böngészik, és a szöveg, képek természetes és néhány sematikus diagramok görget. A helyi meghajtó a távoli asztali munkamenetgazda (távoli asztali munkamenetgazda) kiszolgálójának tárolt weblapok egy. MHT fájlt. A felhasználó azt görget Page Up, Page Down felfelé és lefelé kulcsok használata görgetési minden kulcs/típusú változó időközönként:

    - Lefelé - 250 billentyűleütéseket nagyon 500 ms
    - Page Up - 36 billentyűleütéseket minden 1000 ms
    - Lefelé - 75 billentyűleütéseket minden 100 ms
    - Page Down - 20 billentyűleütéseket minden 500 ms
    - -Legfeljebb 120 billentyűleütéseket minden 300 ms

### <a name="pdf-document---simple---run-for-610-seconds"></a>PDF-dokumentumokat - egyszerű - ~ 610 másodpercig futtatása
A felhasználó beolvassa és PDF-dokumentumot keres különböző módokon Adobe Acrobat Reader használatával. A dokumentum táblák, egyszerű diagramokat és több szöveg betűtípust kell állnia. A dokumentum 35-40 lapok hosszú. A felhasználó két különböző ütemben visszamenőleges görget, és továbbítja a négy különböző nagyítás méretben (lapon, a szélességnek, megfelelően legyen 100 %-os, és Ön egy másik). A Nagyítás biztosítja, hogy a szöveg (betűtípus) Renderelés mérete eltér. A Page Up, Page Down felfelé és lefelé kulcsok használata különböző intervallumok minden görgessen lefelé görgetéshez használható van.

### <a name="pdf-document---mixed---run-for-320-seconds"></a>PDF - vegyes - Futtatás ~ 320 másodpercig dokumentálása
A felhasználó beolvassa és PDF-dokumentumot keres különböző módokon Adobe Acrobat Reader használatával. A dokumentum a kiváló minőségű képek (fényképeket is beleértve), táblák, egyszerű diagramokat és több szöveg betűtípusok áll. A felhasználó két különböző ütemben visszamenőleges görget, és továbbítja a négy különböző nagyítás méretben (lapon, a szélességnek, megfelelően legyen 100 %-os, és Ön egy másik). A Nagyítás biztosítja, hogy a szöveg (betűtípus) Renderelés mérete eltér. A Page Up, Page Down felfelé és lefelé kulcsok használata különböző intervallumok minden görgessen lefelé görgetéshez használható van.

### <a name="flash-video-playback---run-for-180-seconds"></a>A videó lejátszása Flash - ~ 180 másodpercig futtatása
A felhasználó megtekinti az Adobe Flash-kódolású videó beágyazott egy weblapon. A weblap a helyi merevlemez-meghajtóról a távoli asztali munkamenetgazda-kiszolgáló tárolja. Egy beágyazott player beépülő modul által a videó lejátszása Internet Explorerből.

Ebben a forgatókönyvben emulálja a felhasználókat tartalmazó multimédiás gazdag tartalom weblapok megtekintése. Adatok eszközeltávolítás VOBR keresztül kell.

### <a name="word-remote-typing---run-for-1800-seconds"></a>Távoli írja be a Word - ~ 1800 másodpercig futtatása
A felhasználó egy dokumentumot meg kell adnia egy RDP-munkameneten keresztül. Billentyűleütéseket küldése ügyféloldali az RDP-kapcsolaton keresztül a Microsoft Word dokumentum futnak a távoli munkamenetben. Gépelési Ez egy karakter minden 250 ms (teljes 7050 karakter). 

Ez az egyik legnépszerűbb Tudásbázis dolgozó. Ebben a forgatókönyvben a figyelt egy olyan felhasználó, írja be a modern munkahelyi processzort teszteli. Ebben a forgatókönyvben rendszer még akkor is, kisebb változásokkal a sávszélesség-használat-és nagybetűket.

## <a name="tracking-the-test-results"></a>A vizsgálati eredmények nyomon követése
A következő táblázattal értékelheti ki a a forgatókönyvek a környezetben. Az alábbi adatokat a folyamat csak illusztrációs - talán jelentősen eltérő azt láthatja. 

Az egyszerűség kedvéért feltételezzük, hogy minden esetben azonos felbontása 1920 x 1080 képpont használatával vizsgálják, és TCP átviteli módokat a hálózaton és késéssel (késleltetés) alatt a 200 ms és a hálózati szolgáltatás körülbelül 1 % 120 ms + jelölését.

Tudnivalók a táblázatban:

* **Átlagos élmény** tartalmazza a hálózati sávszélesség, amikor a felhasználói termelékenység nem jelentősen befolyásolja, de nem zárja ki a video- vagy alkalmi hibáktól. A rendszer a dinamikus logika kihasználásával gyorsan helyreállíthatja. A hálózati sávszélesség becslése kísérletet a felhasználói élmény minőségének biztosítása.
  * **Észrevehető problémák (töréspontot)** tartalmazza a hálózati sávszélesség, amikor a felhasználók észrevette, hogy az a tapasztalat jelentős problémák, így termelékenységük kihatással van a mérhető időszakok. Ezen a ponton az RDP-algoritmusok is tud lépést, és nem garantálható, hogy a felhasználó minőséget miatt nincs elegendő hálózati sávszélességet.
  * **Ajánlott** tartalmazza a hálózati sávszélesség ajánlott a helyes vagy kiváló felhasználói élmény. Ez lépése általában egy nagyobb, mint a megfelelő érték **élmény átlagos** oszlop.
  * **Megjegyzések** megfigyelések és megjegyzéseket tartalmaz.

| Tesztelés | Átlagos élmény | Észrevehető problémák (töréspontot) | A javasolt hálózati sávszélesség | Megjegyzések |
| --- | --- | --- | --- | --- |
| Vezetői/komplex PPT |10 MB/s |1 MB/s |> 10 MB/s, 100 MB/s előnyben részesített |1 MB/s sok animációk elvesznek |
| Egyszerű PPT |5 MB/s |256 KB/s |10 MB/s |256 KB/s a diák észrevehető késéssel betöltése |
| Az Internet Explorer |10 MB/s |1 MB/s |> 10 MB/s, 100 MB/s előnyben részesített |1 MB/s webes videók határozatlan és szaggatott, gyors görgetés problémák |
| Egyszerű PDF |1 MB/s |256 KB/s |5 MB/s |A lap betöltése igénybe tart 256 KB/s |
| Vegyes PDF |1 MB/s |256 KB/s |5 MB/s |256 KB/s a lap egy jelentős időt vesz betöltése |
| A videó lejátszása Flash |10 MB/s |1 MB/s |> 10 MB/s, 100 MB/s előnyben részesített |1 MB/s a videó szemcsés, és néhány keretek eldobott |
| Távoli írja be a Word |256 KB/s |128 KB/s |1 MB/s |256 KB/s a felhasználó billentyűleütéseit között eltelő idő tapasztalhatja |

Értékelje ki a hálózati sávszélesség, felhasználónként, hozzon létre a fenti forgatókönyvekben vegyesen és a szükséges hálózati sávszélesség megfelelő részét. Válassza ki a szükséges az forgatókönyvek legmagasabb száma. Mivel a felhasználók szinte soha nem használja a rendszer kizárólag, fontolja meg néhány egyidejűleg dolgozhat ugyanazon a hálózaton lévő felhasználók számára, foglalási.

## <a name="learn-more"></a>Részletek
* [Azure RemoteApp a sávszélesség-használat becslése](remoteapp-bandwidth.md)
* [Az Azure RemoteApp - hogyan hálózati sávszélességet és a minőségét tapasztalja munkahelyi együtt?](remoteapp-bandwidthexperience.md)
* [Az Azure RemoteApp hálózati sávszélesség - általános irányelveket (Ha nem teszteli a saját)](remoteapp-bandwidthguidelines.md)

