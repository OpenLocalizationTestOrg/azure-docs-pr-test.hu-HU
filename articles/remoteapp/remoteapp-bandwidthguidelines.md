---
title: "aaaAzure RemoteApp hálózati sávszélesség - általános irányelveket |} Microsoft Docs"
description: "Ismerje meg az Azure RemoteApp-gyűjtemények és az alkalmazások néhány alapvető hálózati sávszélesség útmutatást."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 529bf318-ae37-4c14-a11c-43fa703d68a3
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: d3407eea71e2e8ac524787ee680cfd870c800864
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-remoteapp-network-bandwidth---general-guidelines-if-you-cant-test-your-own"></a>Az Azure RemoteApp hálózati sávszélesség - általános irányelveket (Ha nem teszteli a saját)
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Ha nem rendelkezik hello idő vagy funkció toorun hello [hálózati sávszélesség tesztek](remoteapp-bandwidthtests.md) az Azure RemoteApp, az alábbiakban néhány viszonylag általános irányelveket, amelyik segíthet a hálózati sávszélesség felhasználónként becslése.

Ha ezek a forgatókönyvek vegyesen, nem ajánlott semmilyen nagyobb, mint (egyenlő) 10 MB/s, hello minimális sávszélesség internetkapcsolattal rendelkező modern alkalmazások, a távoli környezetekben. (Bár a bemutatott, ez nem garantálja a megfelelőbb mint átlagos felhasználói élményt.)

## <a name="complex-powerpoint-simple-powerpoint"></a>Összetett PowerPoint, egyszerű PowerPoint
Az Azure RemoteApp legjobb 100 MB-os hálózati végzi. A hello a 10 MB/s hálózati profil, ha 120 ms fent jitter több mint 5 %, hello felhasználói élményt átlagos jelenik meg. 1 MB/s hello különböző van szembeszökő egyenetlenség – például előforduló, hello előfordulhat, hogy látni animált átmenetek minden keretek ki van hagyva, mert.

## <a name="internet-explorer-mixed-pdf-pdf-text"></a>Internet Explorer, a PDF-, PDF-, szöveges vegyes
10 MB/s hálózati profil Bezárás tooLAN a legtöbb területét. 1 MB/s egy OK a felhasználói élményt nyújtják, bár előfordulhat, egyes jitter egy felhasználó görgetésekor miközben képek hello képernyőn.

## <a name="flash-video-youtube"></a>Flash videó (YouTube)
100 MB/s LAN hello legjobb élményt, biztosít, elfogadható (azaz a rendszer hello képkockasebessége tartani, de növeli szolgáltatás) pedig 10 MB/s. 1 MB/s-t a jitter nagyon magas és észrevehető.

## <a name="word-typing-word-remote-input"></a>A Word gépelési (Word távoli bemeneti)
Ez egy webfarmos alacsony sávszélességű használat. 256 KB/s nyújtunk olyan jó a LAN élményt.

## <a name="learn-more"></a>Részletek
* [Azure RemoteApp a sávszélesség-használat becslése](remoteapp-bandwidth.md)
* [Az Azure RemoteApp - hogyan hálózati sávszélességet és a minőségét tapasztalja munkahelyi együtt?](remoteapp-bandwidthexperience.md)
* [Az Azure RemoteApp - tseting olyan gyakori forgatókönyveket tartalmaz, a hálózati sávszélesség-használat](remoteapp-bandwidthtests.md)

