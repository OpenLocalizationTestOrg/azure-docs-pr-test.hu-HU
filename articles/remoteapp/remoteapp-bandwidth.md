---
title: "Azure RemoteApp a sávszélesség-használat aaaEstimate |} Microsoft Docs"
description: "További tudnivalók hello sávszélességre van szükség az Azure RemoteApp-gyűjtemények és az alkalmazások számára."
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
ms.openlocfilehash: 675ee82f26ddb46a3bb3e0ee95ed397e4064e45f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="estimate-azure-remoteapp-network-bandwidth-usage"></a>Azure RemoteApp a sávszélesség-használat becslése
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Az Azure RemoteApp hello Remote Desktop Protocol (RDP) toocommunicate hello Azure felhőben és a felhasználók a futó alkalmazások közötti használja. Ez a cikk néhány alapvető hasznos útmutatást ad a tooestimate használható hálózati használati és potenciálisan kiértékelheti az Azure RemoteApp felhasználónként sávszélesség-használat.

Sávszélesség-használat felhasználónként becslése rendkívül bonyolult, és több alkalmazás egyidejűleg futó többfeladatos forgatókönyvekben ahol hatással lehet a alkalmazások egymás teljesítmény a hálózati sávszélesség iránti igény igényel. Távoli asztali ügyfél (például a Mac-ügyfél és a HTML5-ügyfél) még akkor is, hello típusú toodifferent sávszélesség eredmények vezethet. Ezek komplikációk előrehalad toohelp, azt fogja felosztása hello használati forgatókönyvek hello közös kategóriák tooreplicate valós forgatókönyv számos. (Ha hello valós forgatókönyvvel, természetesen kategóriák vegyesen és a felhasználó által eltér.)

Azt a további - Ugrás előtt vegye figyelembe, hogy azt feltételezzük, hogy RDP jó tooexcellent élményt nyújt a legtöbb használati forgatókönyvek hálózatokon és mértékű sávszélesség 120 ms alatt több mint 5 MB - RDP tartozó képességét toodynamically alapul hello elérhető hálózaton keresztül beállítása sávszélesség- és hello becsült alkalmazások sávszélesség igényeihez. Ez a cikk "legtöbb használati forgatókönyvek" toolook hello szélén, ahol forgatókönyvek toounwind kezdődik, és a felhasználói élmény kezdődik toodegrade kerül ismertetettek mellett.

Most tekintse meg a következő cikkek hello részleteket, beleértve a tényezők tooconsider, Alapterv javaslatok és mi azt nem vette fel a becslések hello.

* [Hogyan hálózati sávszélességet és a minőségét tapasztalja munkahelyi együtt?](remoteapp-bandwidthexperience.md)
* [A hálózati sávszélesség-használat és olyan gyakori forgatókönyveket tartalmaz tesztelése](remoteapp-bandwidthtests.md)
* [Ha még nem rendelkezik hello idő vagy képességét tootest gyors irányelvek](remoteapp-bandwidthguidelines.md)

## <a name="what-are-we-not-including"></a>Mi a Microsoft nem tartalmazzák az?
Amikor javasolt tesztek és teljes (és admittedly általános) Javaslataink hello, vegye figyelembe, hogy vannak-e számos tényező, amely jelenleg nem érdemes lehet. Például hello felhasználói élmény komplikációk feltöltés és letöltés sávszélesség aszimmetrikus jellege hello által biztosított. a legtöbb Wi-Fi hálózat aszimmetrikus jellege hello továbbá hatással lesz a hello teljesítmény- és hello felhasználói élmény egyensúlyozhatom. Interaktív forgatókönyvek hello előtt, ami növelheti a elveszett video- vagy keretek hello száma, és ezért hatással lehet az adatfolyam-élmény hello hello felhasználói érzete-nél kisebb lehet, hogy hello alárendelt forgalom prioritása. A saját kísérletek toosee Mi az a konkrét használati esetek és hálózati futtathatja.

Bár eszközátirányítás arról lesz szó, azt nem vette szempont hello sávszélesség hello hálózati forgalom csatlakoztatott eszközök, például a tárolási, nyomtatók, képolvasók, Webkamerák és más USB-eszközök által okozott hatását. hello érvénybe ezen eszközök általában ideiglenesen napra hello sávszélesség igényeinek és hello feladat befejezésekor eltűnik. De ha gyakran történik, a sávszélesség igény szerint igen szembetűnő lehet.

Még nem tárgyaljuk egy felhasználó hello belüli más felhasználók kedvezőtlen hatással lehet az ugyanazon a hálózaton. Például egy felhasználó egy 100 MB/s hálózaton a 4 KB-os videó fel lehet, hogy jelentős hatással vannak más toodo próbált ugyanazon a hálózaton lévő felhasználók hello ugyanezt a feladatot. Sajnos azt lekérése egyidejű használatra vonatkozó toogive fokozatosan nehezebben toodetermine hello hatása közös vagy minden felölelő ajánlást kapcsolatos hogyan hello rendszer hajt végre, összesítést. Azt is fel, hogy az alapul szolgáló protokoll technológia hello megkönnyítő hello legjobb hello rendelkezésre álló hálózati sávszélesség használatának, azonban ez a korlátozott.

