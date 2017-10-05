---
title: "Több földrajzi területről indított bejelentkezések"
description: "Különböző régiókban, és a modulok nem teszi lehetővé a felhasználó elutazott volna ezeket régiók közötti idő oly módon, hogy egy jelentést, amely jelzi a felhasználó, akinek két bejelentkezési jelent meg."
services: active-directory
documentationcenter: 
author: SSalahAhmed
manager: gchander
editor: 
ms.assetid: 79259c8a-2388-4747-b41e-c07434ea9a02
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/04/2016
ms.author: saah;kenhoff
ms.openlocfilehash: 1de57f64692ade442f9ef8d1e3b587ffee35d7cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="sign-ins-from-multiple-geographies"></a>Bejelentkezések különböző földrajzi régiókból
Ez a jelentés tartalmaz, ahol különböző régiókban oly módon, hogy megjelent két bejelentkezést és a bejelentkezések között eltelt idő a felhasználó is tudott volna elutazni közötti azokban a régiókban lehetetlenné teszi a felhasználó sikeres bejelentkezések. Lehetséges okok a következők:

* Felhasználó oszt meg a jelszavát másokkal
* Felhasználó a távoli asztal segítségével elindítani a webböngészőt a bejelentkezéshez
* Egy támadó szándékkal valaki bejelentkezett felhasználói fiók egy másik országból
* Felhasználó által használt, a VPN vagy a proxy
* Van bejelentkezett felhasználó több eszközről egy időben, például az asztali és a mobiltelefon, és az IP-címét a mobiltelefon szokatlan.

Ez a jelentés eredményeinek megtudhatja, a sikeres bejelentkezési események, együtt a bejelentkezéseket, a régiókban, ahol a bejelentkezések oly módon, hogy megjelent és a becsült utazás időpontja között eltelt idő azokban a régiókban között. Megjelenített utazás idő csak becsült érték, és eltér a tényleges utazás alkalommal a helyek közötti lehet.

![Több földrajzi területről indított bejelentkezések](./media/active-directory-reporting-sign-ins-from-multiple-geographies/signInsFromMultipleGeographies.PNG)

