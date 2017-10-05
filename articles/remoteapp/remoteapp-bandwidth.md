---
title: "Azure RemoteApp a sávszélesség-használat becsléséhez |} Microsoft Docs"
description: "További tudnivalók az Azure RemoteApp-gyűjtemények és az alkalmazások számára a hálózati sávszélesség-követelményekkel."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 3127f4c7-f532-46c3-ba9b-649f647abec1
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 16b4ba974742d004ea02e3f83e522b9c43f2ef40
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="estimate-azure-remoteapp-network-bandwidth-usage"></a>Azure RemoteApp a sávszélesség-használat becslése
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. A részletekért olvassa el a [bejelentést](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Az Azure RemoteApp a távoli asztal protokoll (RDP) használ az Azure felhőben és a felhasználók futó alkalmazások közötti kommunikációhoz. A cikkben néhány alapvető útmutatók használatával, hogy hálózati használat becsléséhez és potenciálisan kiértékelheti az Azure RemoteApp felhasználónként sávszélesség-használat.

Sávszélesség-használat felhasználónként becslése rendkívül bonyolult, és több alkalmazás egyidejűleg futó többfeladatos forgatókönyvekben ahol hatással lehet a alkalmazások egymás teljesítmény a hálózati sávszélesség iránti igény igényel. Még a távoli asztali ügyfél (például a Mac-ügyfél és a HTML5-ügyfél) típusú különböző sávszélesség eredményekhez vezethet. Kezelheti a komplikációk keresztül, azt fogja felosztása a használati forgatókönyvek a számos gyakori kategóriája valós forgatókönyv replikálásához. (Ha a valós forgatókönyvvel, természetesen kategóriák vegyesen és a felhasználó által eltér.)

Azt a további - Ugrás előtt vegye figyelembe, hogy azt feltételezzük, hogy RDP biztosít kiváló felületet jó legtöbb használati forgatókönyvek a hálózatok és mértékű sávszélesség 120 ms alatt több mint 5 MB - RDP tartozó képes dinamikusan úgy, hogy a rendelkezésre álló hálózati sávszélesség alapul ezért az alkalmazás becsült sávszélesség. Ez a cikk kerül ruházzák "legtöbb használati forgatókönyvek" a peremhálózaton, ahol forgatókönyvek veszik át a lecseréléshez és felhasználói élmény kezd kezdődik meg.

Alapkonfiguráció javaslatokat, és mi azt nem vette fel a becslések most tekintse meg a részletes tényezőket kell figyelembe venni, beleértve a következő cikkeket.

* [Hogyan hálózati sávszélességet és a minőségét tapasztalja munkahelyi együtt?](remoteapp-bandwidthexperience.md)
* [A hálózati sávszélesség-használat és olyan gyakori forgatókönyveket tartalmaz tesztelése](remoteapp-bandwidthtests.md)
* [Ha még nem rendelkezik, az idő vagy képességét tesztelése gyors irányelvek](remoteapp-bandwidthguidelines.md)

## <a name="what-are-we-not-including"></a>Mi a Microsoft nem tartalmazzák az?
Amikor a javasolt tesztek és az általános (és admittedly általános) javaslatokat, vegye figyelembe, hogy vannak-e számos tényező, amely jelenleg nem érdemes lehet. Például a felhasználói élmény komplikációk feltöltés és aszimmetrikus jellege által biztosított letölti a sávszélesség. A legtöbb Wi-Fi hálózat aszimmetrikus jellege továbbá hatással van a teljesítmény és a felhasználói élmény egyensúlyozhatom. Interaktív forgatókönyvek alacsonyabb, mint a felsőbb rétegbeli, amely az elveszett video- vagy keretek számának növeléséhez, és ezért hatással lehet az adatfolyam-továbbítási élmény felhasználói érzete előfordulhat, hogy az alsóbb rétegbeli forgalom prioritása. A konkrét használati esetek és hálózati meg a saját kísérletek is futtathatja.

Bár eszközátirányítás arról lesz szó, azt nem vette figyelembe a csatlakoztatott eszközök, például a tárolási, nyomtatók, képolvasók, Webkamerák és más USB-eszközök által okozott hálózati forgalom a sávszélesség hatását. A hatását, hogy az eszközök általában ideiglenesen napra sávszélesség igényeinek és eltűnik, ha a tevékenység befejeződött. De ha gyakran történik, a sávszélesség igény szerint igen szembetűnő lehet.

Még nem tárgyaljuk egy felhasználó kedvezőtlen hatással lehet az ugyanazon a hálózaton belüli más felhasználók. Például egy felhasználó egy 100 MB/s hálózaton a 4 KB-os videó fel lehet, hogy jelentős hatással vannak ugyanaz a feladat végrehajtását megkísérlő ugyanazon a hálózaton lévő többi felhasználóval. Azt sajnos lekérdezi fokozatosan nehezebben egyidejű használata közös vagy minden felölelő ajánlást kapcsolatban a rendszer teljesítményét összesítő, hogy a hatásának megállapításához. Azt is fel, hogy az alapul szolgáló protokoll technológiát biztosítják a lehető legjobb felhasználását, a rendelkezésre álló hálózati sávszélességet, azonban ez a korlátozott.

