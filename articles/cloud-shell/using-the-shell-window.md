---
title: "aaaUsing hello Azure Cloud rendszerhéj (előzetes verzió) ablakban |} Microsoft Docs"
description: "A forgatókönyv hello Azure Cloud rendszerhéj ablakát."
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
ms.openlocfilehash: 571db3c8e177799a9e05f38a7cf8d2a4d5f8c8de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-azure-cloud-shell-window"></a>Hello Azure Cloud rendszerhéj ablakát használatával

Ez a dokumentum azt ismerteti, hogyan toouse hello felhő rendszerhéj ablakát.

## <a name="concurrent-sessions"></a>Egyidejű munkamenetek
Minden munkamenet tooexist Bash különálló folyamatként tételével a felhő rendszerhéj lehetővé teszi több egyidejű munkamenetek különböző böngészőlapokon.
Ha most kilép a munkamenet meg arról, hogy minden munkamenet ablakból tooexit minden folyamat függetlenül fut, bár a számítógépen futnak, hello azonos gépen.

## <a name="restart-cloud-shell"></a>Indítsa újra a felhő rendszerhéj
![](media/recycle.png)
> [!WARNING]
> Felhő rendszerhéj újraindítása alaphelyzetbe állítja a gép állapotát, és semmilyen fájl nem őrződik meg a fájl megosztási el fog veszni.

* Kattintson a hello újraindítás ikonjára hello eszköztár tooreceive egy új felhőalapú rendszerhéj-környezetben.

## <a name="minimize--maximize-cloud-shell-window"></a>Kis méret & felhő rendszerhéj ablakának teljes méretűre állítása
![](media/minmax.png)
* Hello kattintson hello ikonra minimalizálása érdekében jobb oldalán hello ablak toohide felső azt. Kattintson a felhő rendszerhéj ikon hello újra toounhide.
* Kattintson a hello ikon tooset ablak toomax magassága maximalizálása érdekében. toorestore ablak tooprevious mérete, kattintson a Visszaállítás gombra.

## <a name="copy-and-paste"></a>Másolás és beillesztés
* Windows: `Ctrl-insert` toocopy és `Shift-insert` toopaste. Kattintson a jobb gombbal a legördülő másolja és illessze be is engedélyezheti.
  * A FireFox vagy IE nem támogatja a vágólapra engedélyek megfelelően.
* Mac OS: `Cmd-c` toocopy és `Cmd-v` toopaste. Kattintson a jobb gombbal a legördülő másolja és illessze be is engedélyezheti.

## <a name="resize-cloud-shell-window"></a>Felhő rendszerhéj ablakát átméretezése
* Kattintással és húzással hello eszköztár felső széle hello felfelé vagy lefelé tooresize hello felhő rendszerhéj ablakát.

## <a name="scrolling-text-display"></a>A görgethető szöveg megjelenítése
* Görgessen az egeret vagy touchpad toomove terminál szóra.

## <a name="exit-command"></a>Kilépés paranccsal
Futó `exit` hello aktív munkamenet leállítása. Alapértelmezés szerint ez a viselkedés beavatkozás nélkül 20 perc után következik be.

## <a name="next-steps"></a>Következő lépések
[Felhő rendszerhéj gyors üzembe helyezés](quickstart.md)
