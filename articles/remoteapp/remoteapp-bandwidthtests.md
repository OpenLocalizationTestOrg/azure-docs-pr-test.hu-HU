---
title: "RemoteApp - tesztelés olyan gyakori forgatókönyveket tartalmaz, a hálózati sávszélesség-használat aaaAzure |} Microsoft Docs"
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
ms.openlocfilehash: 22c1dbb61d956d58d01eb4e11363266168e337e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-remoteapp---testing-your-network-bandwidth-usage-with-some-common-scenarios"></a>Az Azure RemoteApp – olyan gyakori forgatókönyveket tartalmaz, a hálózati sávszélesség-használat tesztelése
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Azt a bemutatott [becsült Azure RemoteApp a sávszélesség-használat](remoteapp-bandwidth.md), ki milyen hello hatással az Azure RemoteApp tooyour hálózati toorun legjobb módja toofigure néhány használati teszt hello. Egy készlet idő időtartam és a mérték hello sávszélesség az egyes forgatókönyvek esetén szükség ezen tesztek futtatásához. Ha hello funkció, is tudja mérni a hello hálózati csomag adatvesztéssel és a hálózati jitter toounderstand hello hálózati mintáról olvashat, amelyek az adott környezethez jön létre.

Hello a sávszélesség-használat kiértékelésekor ne feledje, hogy használati változó a vállalaton belül a különböző felhasználók között. Például szöveg olvasók és írók általában használja felhasználók, amelyek a video-nál kisebb sávszélességet. A legjobb eredmények elérése érdekében tanulmányozza a saját felhasználói igényeket, és hozzon létre a következő forgatókönyvek hello, amely a legjobban jelképezi a vállalatnál lévő felhasználóknál hello vegyesen. Ne feledje túl[felülvizsgálati hello tényezők hatással lehetnek a sávszélesség-használat és a felhasználói élmény](remoteapp-bandwidthexperience.md) -, amely segít azonosítani hello ideális teszteket.

Első információk a hello teszteket, válassza ki a vegyes, és futtassa azokat. Hello tábla toohelp követése teljesítmény alatt is használhatja.

> [!NOTE]
> Ha a hálózati teszteléssel nem hajtható végre, vagy nincs hello idő toodo így, tekintse meg a [alapvető hálózati sávszélesség becslése/javaslatok](remoteapp-bandwidthguidelines.md). A távolság változhat, azonban, ha Ön *is* saját tesztek futtatásához meg kell.
> 
> 

## <a name="hello-usage-tests"></a>hello használati tesztek
E vizsgálatok különböző rengeteg idő futtatása, és különböző funkciókat/funkcióit tesztelik, használja a hálózati sávszélességet. Ne felejtse el a tesztet, amely a legjobban illik a egyedi vállalati felhasználók toochoose hello kombinációját.

### <a name="executivecomplex-powerpoint---run-for-900-1000-seconds"></a>Vezetői/komplex PowerPoint - 900-1000 másodpercig futtatása
A felhasználó megadja a 45-50 valósághű diák a Microsoft Office PowerPoint a teljes képernyős mód között. hello diák képet, (az animáció) átmenetek és szín színátmenet, amelyek általában a vállalat hátterek tartalmazhat. hello felhasználói egyes diák kell egy legalább 20 másodperc.

Ebben a forgatókönyvben cellaveszteségi forgalmat hoz létre, amikor egy menüpontban húzással állíthatja be a következő diák toohello hello bemutató átmenetek.

### <a name="simple-powerpoint---run-for-620-seconds"></a>Egyszerű PowerPoint - ~ 620 másodpercig futtatása
A felhasználó egy egyszerű PowerPoint-fájlhoz megadja körülbelül 30 diák a Microsoft Office PowerPoint a teljes képernyős módban. hello diák több szöveg igényel mint hello végrehajtó/komplex PowerPoint forgatókönyvben és egyszerűbb hátterek és lemezképek (fekete diagramok). 

### <a name="internet-explorer---run-for-250-seconds"></a>Az Internet Explorer - legfeljebb 250 másodpercig futtatása
A felhasználó hello webes böngészik, az Internet Explorer használatával. hello felhasználói böngészik, és a szöveg, képek természetes és néhány sematikus diagramok görget. hello hello távoli asztali munkamenetgazda (távoli asztali munkamenetgazda) kiszolgálójának helyi lemezmeghajtóra tárolt weblapok hello egy. MHT fájlt. hello kiváltott Page Up, Page Down felfelé és lefelé kulcsok használata görgetési minden kulcs/típusú változó időközönként:

    - Lefelé - 250 billentyűleütéseket nagyon 500 ms
    - Page Up - 36 billentyűleütéseket minden 1000 ms
    - Lefelé - 75 billentyűleütéseket minden 100 ms
    - Page Down - 20 billentyűleütéseket minden 500 ms
    - -Legfeljebb 120 billentyűleütéseket minden 300 ms

### <a name="pdf-document---simple---run-for-610-seconds"></a>PDF-dokumentumokat - egyszerű - ~ 610 másodpercig futtatása
A felhasználó beolvassa és PDF-dokumentumot keres különböző módokon Adobe Acrobat Reader használatával. hello dokumentum táblák, egyszerű diagramokat és több szöveg betűtípust kell állnia. hello dokumentum 35-40 lapok hosszú. hello felhasználó két különböző ütemben visszamenőleges görget, és továbbítja a négy különböző nagyítás méretben (toopage, illeszkedő toowidth elférjen 100 %-os, és Ön egy másik). nagyítással hello biztosítja, hogy különböző méretű Renderelés hello szöveg (betűtípus). Hello Page Up, Page Down felfelé és lefelé kulcsok használata különböző intervallumok minden görgessen lefelé görgetéshez használható van.

### <a name="pdf-document---mixed---run-for-320-seconds"></a>PDF - vegyes - Futtatás ~ 320 másodpercig dokumentálása
A felhasználó beolvassa és PDF-dokumentumot keres különböző módokon Adobe Acrobat Reader használatával. hello dokumentum kiváló minőségű képek (fényképeket is beleértve), táblák, egyszerű diagramokat és több szöveg betűtípusok áll. hello felhasználó két különböző ütemben visszamenőleges görget, és továbbítja a négy különböző nagyítás méretben (toopage, illeszkedő toowidth elférjen 100 %-os, és Ön egy másik). nagyítással hello biztosítja, hogy különböző méretű Renderelés hello szöveg (betűtípus). Hello Page Up, Page Down felfelé és lefelé kulcsok használata különböző intervallumok minden görgessen lefelé görgetéshez használható van.

### <a name="flash-video-playback---run-for-180-seconds"></a>A videó lejátszása Flash - ~ 180 másodpercig futtatása
A felhasználó megtekinti az Adobe Flash-kódolású videó beágyazott egy weblapon. hello weblap hello helyi merevlemezére hello távoli asztali munkamenetgazda kiszolgáló tárolja. videó hello beépülő modul beágyazott lejátszót játszott Internet Explorerből.

Ebben a forgatókönyvben emulálja a felhasználókat tartalmazó multimédiás gazdag tartalom weblapok megtekintése. Hello adatok eszközeltávolítás VOBR keresztül kell.

### <a name="word-remote-typing---run-for-1800-seconds"></a>Távoli írja be a Word - ~ 1800 másodpercig futtatása
A felhasználó egy dokumentumot meg kell adnia egy RDP-munkameneten keresztül. Billentyűleütéseket küldi hello ügyféloldali keresztül hello RDP munkamenet tooa dokumentumot a Microsoft Word hello távoli munkamenetben futó. Írja be a sebesség hello egy karakter minden 250 ms (teljes 7050 karakter). 

Ez az egyik hello legnépszerűbb Tudásbázis dolgozó. Ebben a forgatókönyvben teszteli, írja be a modern munkahelyi processzor felhasználói hello válaszképességét. Ebben a forgatókönyvben a sávszélesség-használat bizalmas tooeven kisebb változásokkal.

## <a name="tracking-hello-test-results"></a>Hello teszteredmények nyomon követése
A következő tábla tooevaluate hello forgatókönyvek környezetében hello is használhatja. alább hello adatok folyamat csak illusztrációs - talán jelentősen eltérő azt láthatja. 

Az egyszerűség kedvéért feltételezzük, hogy az összes forgatókönyv hello azonos 1920 x 1080 képpont képernyőfelbontás használatával vizsgálják, és TCP átviteli módokat a hálózaton és késéssel (késleltetés) alatt a 200 ms és a hálózati szolgáltatás a hello 120 ms + beleszámított körülbelül 1 %.

Hello tábláról:

* **Átlagos élmény** hello hálózati sávszélesség, amikor a felhasználói termelékenység nem jelentősen befolyásolja, de nem zárja ki a video- vagy alkalmi hibáktól tartalmazza. hello rendszer képes toorecover gyorsan hello dinamikus logika kihasználásával. hello hálózati sávszélesség becslése kísérlet tooguarantee hello minőségének hello felhasználói élményt.
  * **Észrevehető problémák (töréspontot)** hello hálózati sávszélesség, amikor a felhasználók észrevette, hogy az a tapasztalat jelentős problémák, így termelékenységük kihatással van a mérhető időszakok tartalmazza. Ezen a ponton hello RDP algoritmusok is tud lépést, és nem garantálható, hogy hello felhasználói minőséget miatt nincs elegendő hálózati sávszélességet.
  * **Ajánlott** hello hálózati sávszélesség ajánlott a helyes vagy kiváló felhasználói élmény tartalmazza. Ez lépése általában egy nagyobb, mint hello hello megfelelő **élmény átlagos** oszlop.
  * **Megjegyzések** megfigyelések és megjegyzéseket tartalmaz.

| Tesztelés | Átlagos élmény | Észrevehető problémák (töréspontot) | A javasolt hálózati sávszélesség | Megjegyzések |
| --- | --- | --- | --- | --- |
| Vezetői/komplex PPT |10 MB/s |1 MB/s |> 10 MB/s, 100 MB/s előnyben részesített |1 MB/s sok animációk elvesznek |
| Egyszerű PPT |5 MB/s |256 KB/s |10 MB/s |256 KB/s hello diák észrevehető késéssel betöltése |
| Az Internet Explorer |10 MB/s |1 MB/s |> 10 MB/s, 100 MB/s előnyben részesített |1 MB/s webes videók határozatlan és szaggatott, gyors görgetés problémák |
| Egyszerű PDF |1 MB/s |256 KB/s |5 MB/s |Egy kis ideig tart: 256 KB/s tooload hello lap |
| Vegyes PDF |1 MB/s |256 KB/s |5 MB/s |256 KB/s hello foglalja jelentős mennyiségű időt tooload |
| A videó lejátszása Flash |10 MB/s |1 MB/s |> 10 MB/s, 100 MB/s előnyben részesített |1 MB/s videó hello szemcsés, és néhány keretek eldobott |
| Távoli írja be a Word |256 KB/s |128 KB/s |1 MB/s |256 KB/s felhasználói lehet észrevenni hello billentyűleütéseket között |

tooevaluate hello hálózati sávszélesség felhasználónként, a fenti forgatókönyvek hello kombinációját és a szükséges hálózati sávszélesség megfelelő részét hello létrehozása. Válassza ki az forgatókönyvek szükséges hello legmagasabb száma. Mivel a felhasználók a hello rendszer önmagában szinte soha nem használja, fontolja meg néhány tartalék a felhasználók, amelyek működnek a egyidejűleg hello ugyanazon a hálózaton.

## <a name="learn-more"></a>Részletek
* [Azure RemoteApp a sávszélesség-használat becslése](remoteapp-bandwidth.md)
* [Az Azure RemoteApp - hogyan hálózati sávszélességet és a minőségét tapasztalja munkahelyi együtt?](remoteapp-bandwidthexperience.md)
* [Az Azure RemoteApp hálózati sávszélesség - általános irányelveket (Ha nem teszteli a saját)](remoteapp-bandwidthguidelines.md)

