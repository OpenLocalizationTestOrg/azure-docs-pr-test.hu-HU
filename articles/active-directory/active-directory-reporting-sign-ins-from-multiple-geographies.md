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
# <a name="sign-ins-from-multiple-geographies"></a><span data-ttu-id="26c9f-103">Bejelentkezések különböző földrajzi régiókból</span><span class="sxs-lookup"><span data-stu-id="26c9f-103">Sign-ins from multiple geographies</span></span>
<span data-ttu-id="26c9f-104">Ez a jelentés tartalmaz, ahol különböző régiókban oly módon, hogy megjelent két bejelentkezést és a bejelentkezések között eltelt idő a felhasználó is tudott volna elutazni közötti azokban a régiókban lehetetlenné teszi a felhasználó sikeres bejelentkezések.</span><span class="sxs-lookup"><span data-stu-id="26c9f-104">This report includes successful sign-ins from a user where two sign-ins appeared to originate from different regions and the time between the sign-ins makes it impossible for the user to have traveled between those regions.</span></span> <span data-ttu-id="26c9f-105">Lehetséges okok a következők:</span><span class="sxs-lookup"><span data-stu-id="26c9f-105">Possible causes include:</span></span>

* <span data-ttu-id="26c9f-106">Felhasználó oszt meg a jelszavát másokkal</span><span class="sxs-lookup"><span data-stu-id="26c9f-106">User is sharing their password with other users</span></span>
* <span data-ttu-id="26c9f-107">Felhasználó a távoli asztal segítségével elindítani a webböngészőt a bejelentkezéshez</span><span class="sxs-lookup"><span data-stu-id="26c9f-107">User is using a remote desktop to launch a web browser for sign-in</span></span>
* <span data-ttu-id="26c9f-108">Egy támadó szándékkal valaki bejelentkezett felhasználói fiók egy másik országból</span><span class="sxs-lookup"><span data-stu-id="26c9f-108">A hacker has signed in to the account of a user from a different country</span></span>
* <span data-ttu-id="26c9f-109">Felhasználó által használt, a VPN vagy a proxy</span><span class="sxs-lookup"><span data-stu-id="26c9f-109">User is using a VPN or proxy</span></span>
* <span data-ttu-id="26c9f-110">Van bejelentkezett felhasználó több eszközről egy időben, például az asztali és a mobiltelefon, és az IP-címét a mobiltelefon szokatlan.</span><span class="sxs-lookup"><span data-stu-id="26c9f-110">User is signed in from multiple devices at the same time, such as a desktop and a mobile phone, and the IP address of the mobile phone is unusual.</span></span>

<span data-ttu-id="26c9f-111">Ez a jelentés eredményeinek megtudhatja, a sikeres bejelentkezési események, együtt a bejelentkezéseket, a régiókban, ahol a bejelentkezések oly módon, hogy megjelent és a becsült utazás időpontja között eltelt idő azokban a régiókban között.</span><span class="sxs-lookup"><span data-stu-id="26c9f-111">Results from this report will show you the successful sign-in events, together with the time between the sign-ins, the regions where the sign-ins appeared to originate from, and the estimated travel time between those regions.</span></span> <span data-ttu-id="26c9f-112">Megjelenített utazás idő csak becsült érték, és eltér a tényleges utazás alkalommal a helyek közötti lehet.</span><span class="sxs-lookup"><span data-stu-id="26c9f-112">The travel time shown is only an estimate and may be different from the actual travel time between the locations.</span></span>

![Több földrajzi területről indított bejelentkezések](./media/active-directory-reporting-sign-ins-from-multiple-geographies/signInsFromMultipleGeographies.PNG)

