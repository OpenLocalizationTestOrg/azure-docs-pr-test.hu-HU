---
title: "Valószínűleg fertőzött eszközökről indított bejelentkezések"
description: "Ez a jelentés tartalmazza a bejelentkezési kísérlet alatt, amelyek futtathatnak néhány kártevő szoftver (kártevők) eszközökről végrehajtott."
services: active-directory
documentationcenter: 
author: SSalahAhmed
manager: gchander
editor: 
ms.assetid: d0361662-d970-4a66-8eb3-72e9f8824dcf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/04/2016
ms.author: saah;kenhoff
ms.openlocfilehash: 3809e20937d8d9829675e20f893101cb849dcea2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="sign-ins-from-possibly-infected-devices"></a><span data-ttu-id="740f6-103">Valószínűleg fertőzött eszközökről indított bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="740f6-103">Sign ins from possibly infected devices</span></span>
<span data-ttu-id="740f6-104">Ez a jelentés megkísérli azonosítani, hogy van kitéve, és most egy botnet részét képezik a felhasználói eszközökön.</span><span class="sxs-lookup"><span data-stu-id="740f6-104">This report attempts to identify your users' devices that that have become infected and are now part of a botnet.</span></span> <span data-ttu-id="740f6-105">A felhasználói bejelentkezések szemben, amelyek szerepelnek a botnet kiszolgálók kell tudjuk IP-címek IP-címek azt összefüggéseket.</span><span class="sxs-lookup"><span data-stu-id="740f6-105">We correlate IP addresses of users' sign-ins against IP addresses that we know to be in contact with botnet servers.</span></span>

<span data-ttu-id="740f6-106">Javaslat: Ez a jelentés az IP-címek, nem a felhasználói eszközök jelzők.</span><span class="sxs-lookup"><span data-stu-id="740f6-106">Recommendation: This report flags IP addresses, not user devices.</span></span> <span data-ttu-id="740f6-107">Azt javasoljuk, hogy lépjen kapcsolatba a felhasználóval, és győződjön meg a felhasználói eszközök.</span><span class="sxs-lookup"><span data-stu-id="740f6-107">We recommend that you contact the user and scan all the user's devices to be certain.</span></span> <span data-ttu-id="740f6-108">Lehetőség arra is, hogy a felhasználó személyes eszköz fertőzött, vagy az, hogy a felhasználó, aki az azonos IP-cím volt a felhasználó használ, nem rendelkezik-e egy fertőzött eszköz.</span><span class="sxs-lookup"><span data-stu-id="740f6-108">It is also possible that a user's personal device is infected, or that someone other than the user, who was using the same IP address as the user, has an infected device.</span></span>

<span data-ttu-id="740f6-109">Cím kártevőszoftver-fertőzések kapcsolatos további információkért lásd: a [Kártevőkezelési központ](http://go.microsoft.com/fwlink/?linkid=335773).</span><span class="sxs-lookup"><span data-stu-id="740f6-109">For more information about how to address malware infections, see the [Malware Protection Center](http://go.microsoft.com/fwlink/?linkid=335773).</span></span>

![Valószínűleg fertőzött eszközökről indított bejelentkezések](./media/active-directory-reporting-sign-ins-from-possibly-infected-devices/signInsFromPossiblyInfectedDevices.PNG)

