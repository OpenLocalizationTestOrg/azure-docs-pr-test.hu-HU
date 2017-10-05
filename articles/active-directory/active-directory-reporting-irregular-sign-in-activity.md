---
title: "Rendszertelen bejelentkezési tevékenység"
description: "Egy jelentés, amely bejelentkezések azonosított, rendellenes a gépi tanulási algoritmus."
services: active-directory
documentationcenter: 
author: SSalahAhmed
manager: gchander
editor: 
ms.assetid: a5a270a9-a0eb-4a99-b04c-ccde3829ffe4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/04/2016
ms.author: saah;kenhoff
ms.openlocfilehash: 565321b6f3ad5988f383a701cb9d8bd5c9937795
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="irregular-sign-in-activity"></a><span data-ttu-id="692db-103">Rendszertelen bejelentkezési tevékenység</span><span class="sxs-lookup"><span data-stu-id="692db-103">Irregular sign-in activity</span></span>
<span data-ttu-id="692db-104">Szabálytalan bejelentkezések megegyeznek a gépi tanulási algoritmusok, alapján egy "lehetetlen odautazás" állapot kombinálva, a rendellenes bejelentkezési a hely és eszköz által azonosított.</span><span class="sxs-lookup"><span data-stu-id="692db-104">Irregular sign-ins are those that have been identified by our machine learning algorithms, on the basis of an "impossible travel" condition combined with an anomalous sign in location and device.</span></span> <span data-ttu-id="692db-105">Ez azt jelezheti, hogy egy támadó sikeresen bejelentkezett az ezzel a fiókkal.</span><span class="sxs-lookup"><span data-stu-id="692db-105">This may indicate that a hacker has successfully signed in using this account.</span></span>
<span data-ttu-id="692db-106">Kapni fog egy értesítő a globális rendszergazdáknak Ha 10 vagy több rendellenes bejelentkezési esemény vagy kevesebb mint 30 napnyi belül előforduló azt.</span><span class="sxs-lookup"><span data-stu-id="692db-106">We will send an email notification to the global admins if we encounter 10 or more anomalous sign-in events within a span of 30 days or less.</span></span> <span data-ttu-id="692db-107">Ellenőrizze, hogy tartalmazzák aad-alerts-noreply@mail.windowsazure.com a megbízható feladók listáján.</span><span class="sxs-lookup"><span data-stu-id="692db-107">Please be sure to include aad-alerts-noreply@mail.windowsazure.com in your safe senders list.</span></span>

