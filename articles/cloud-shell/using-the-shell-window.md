---
title: "Az Azure felhőalapú rendszerhéj (előzetes verzió) ablakban |} Microsoft Docs"
description: "Útmutató az Azure felhőalapú rendszerhéj ablakát."
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: juluk
ms.openlocfilehash: a47961dfdaf178a6b793bd68105d9792a9275bb3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="using-the-azure-cloud-shell-window"></a>Az Azure felhőalapú rendszerhéj ablakát használatával

Ez a dokumentum ismerteti, hogyan a felhő rendszerhéj ablakát.

## <a name="concurrent-sessions"></a>Egyidejű munkamenetek
Felhő rendszerhéj lehetővé teszi több egyidejű munkamenetek különböző böngészőlapokon távfelügyeletét minden munkamenet nem létezik-e külön Bash folyamatban.
Ha most kilép egy munkamenet, ügyeljen arra, hogy minden munkamenet ablak lépni, minden folyamat függetlenül fut, de ugyanazon a számítógépen futnak.

## <a name="restart-cloud-shell"></a>Indítsa újra a felhő rendszerhéj
![](media/recycle.png)
> [!WARNING]
> Felhő rendszerhéj újraindítása alaphelyzetbe állítja a gép állapotát, és semmilyen fájl nem őrződik meg a fájl megosztási el fog veszni.

* Kattintson az újraindítás ikonra egy új felhőalapú rendszerhéj környezetben fogadni az eszköztáron.

## <a name="minimize--maximize-cloud-shell-window"></a>Kis méret & felhő rendszerhéj ablakának teljes méretűre állítása
![](media/minmax.png)
* Kattintson a kis méret ikonra a felső sarkában az ablak elrejtéséhez. Kattintson a felhő rendszerhéj ismét az ikonra kattintva felfedése.
* Kattintson a teljes méret ikonra beállítása ablakban maximális magasságát. Ablak visszaállítása eredeti méretének, kattintson a visszaállítás.

## <a name="copy-and-paste"></a>Másolás és beillesztés
* Windows: `Ctrl-insert` másolása és `Shift-insert` beilleszteni. Kattintson a jobb gombbal a legördülő másolja és illessze be is engedélyezheti.
  * A FireFox vagy IE nem támogatja a vágólapra engedélyek megfelelően.
* Mac OS: `Cmd-c` másolása és `Cmd-v` beilleszteni. Kattintson a jobb gombbal a legördülő másolja és illessze be is engedélyezheti.

## <a name="resize-cloud-shell-window"></a>Felhő rendszerhéj ablakát átméretezése
* Kattintással és húzással a felső szegélyéhez, hogy az eszköztár felfelé vagy lefelé méretezze át a felhő rendszerhéj ablakát.

## <a name="scrolling-text-display"></a>A görgethető szöveg megjelenítése
* Görgessen az egérrel és touchpad terminál szöveg áthelyezése.

## <a name="exit-command"></a>Kilépés paranccsal
Futó `exit` megszakítja az aktív munkamenetet. Alapértelmezés szerint ez a viselkedés beavatkozás nélkül 20 perc után következik be.

## <a name="next-steps"></a>Következő lépések
[Felhő rendszerhéj gyors üzembe helyezés](quickstart.md)
