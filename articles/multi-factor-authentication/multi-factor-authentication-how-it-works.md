---
title: Az Azure multi-factor Authentication - elv
description: "Az Azure Multi-Factor Authentication szolgáltatás révén biztonságosabb a hozzáférés az adatokhoz és az alkalmazásokhoz, és a felhasználók is egyszerűbben jelentkezhetnek be. További biztonságot nyújt, azzal, hogy egy második hitelesítési mód, és kézbesíti az erős hitelesítés keresztül egyszerűen ellenőrzési lehetőségek közül."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d14db902-9afe-4fca-b3a5-4bd54b3d8ec5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: 6fee02885cc76b3a4fdad11e8702f623d6fe6597
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-azure-multi-factor-authentication-works"></a><span data-ttu-id="07b3d-104">Azure multi-factor Authentication működése</span><span class="sxs-lookup"><span data-stu-id="07b3d-104">How Azure Multi-Factor Authentication works</span></span>
<span data-ttu-id="07b3d-105">A kétlépéses ellenőrzést biztonságát a réteges megközelítésének rejlik.</span><span class="sxs-lookup"><span data-stu-id="07b3d-105">The security of two-step verification lies in its layered approach.</span></span> <span data-ttu-id="07b3d-106">Jelentős kihívás a támadók számára a több hitelesítési tényezők veszélyeztetése mutatja be.</span><span class="sxs-lookup"><span data-stu-id="07b3d-106">Compromising multiple authentication factors presents a significant challenge for attackers.</span></span> <span data-ttu-id="07b3d-107">Akkor is, ha egy támadó kezeli, és ismerje meg a jelszót, akkor nem használható a megbízható eszköz a birtokában nélkül is.</span><span class="sxs-lookup"><span data-stu-id="07b3d-107">Even if an attacker manages to learn the user's password, it is useless without also having possession of the trusted device.</span></span> 

![Proofup](./media/multi-factor-authentication-how-it-works/howitworks.png)

<span data-ttu-id="07b3d-109">Az Azure Multi-Factor Authentication szolgáltatás révén biztonságosabb a hozzáférés az adatokhoz és az alkalmazásokhoz, és a felhasználók is egyszerűbben jelentkezhetnek be.</span><span class="sxs-lookup"><span data-stu-id="07b3d-109">Azure Multi-Factor Authentication helps safeguard access to data and applications while meeting user demand for a simple sign-in process.</span></span>  <span data-ttu-id="07b3d-110">További biztonságot nyújt, azzal, hogy egy második hitelesítési mód, és kézbesíti az erős hitelesítés keresztül egyszerűen ellenőrzési lehetőségek közül.</span><span class="sxs-lookup"><span data-stu-id="07b3d-110">It provides additional security by requiring a second form of authentication and delivers strong authentication via a range of easy verification options.</span></span>


## <a name="methods-available-for-two-step-verification"></a><span data-ttu-id="07b3d-111">Módszer áll rendelkezésre a kétlépéses ellenőrzéshez</span><span class="sxs-lookup"><span data-stu-id="07b3d-111">Methods available for two-step verification</span></span>
<span data-ttu-id="07b3d-112">Amikor egy felhasználó bejelentkezik, egy további ellenőrzési küld a felhasználónak.</span><span class="sxs-lookup"><span data-stu-id="07b3d-112">When a user signs in, an additional verification is sent to the user.</span></span>  <span data-ttu-id="07b3d-113">Az alábbiakban a második ellenőrzési célból használt módszerek listáját.</span><span class="sxs-lookup"><span data-stu-id="07b3d-113">The following are a list of methods that can be used for this second verification.</span></span>

| <span data-ttu-id="07b3d-114">Ellenőrzési módszert</span><span class="sxs-lookup"><span data-stu-id="07b3d-114">Verification Method</span></span> | <span data-ttu-id="07b3d-115">Leírás</span><span class="sxs-lookup"><span data-stu-id="07b3d-115">Description</span></span> |
| --- | --- |
| <span data-ttu-id="07b3d-116">Telefonhívás</span><span class="sxs-lookup"><span data-stu-id="07b3d-116">Phone call</span></span> |<span data-ttu-id="07b3d-117">A meghívásra kerül a felhasználó regisztrált telefonjára.</span><span class="sxs-lookup"><span data-stu-id="07b3d-117">A call is placed to a user’s registered phone.</span></span> <span data-ttu-id="07b3d-118">A felhasználó megadja a PIN-kódot, ha szükséges, majd a a # billentyűt nyomja.</span><span class="sxs-lookup"><span data-stu-id="07b3d-118">The user enters a PIN if necessary then presses the # key.</span></span> |
| <span data-ttu-id="07b3d-119">Szöveges üzenet</span><span class="sxs-lookup"><span data-stu-id="07b3d-119">Text message</span></span> |<span data-ttu-id="07b3d-120">Egy szöveges üzenetet küld a felhasználó mobiltelefonra egy hatjegyű kódot.</span><span class="sxs-lookup"><span data-stu-id="07b3d-120">A text message is sent to a user’s mobile phone with a six-digit code.</span></span> <span data-ttu-id="07b3d-121">A felhasználó megadja ezt a kódot a bejelentkezési oldalon.</span><span class="sxs-lookup"><span data-stu-id="07b3d-121">The user enters this code on the sign-in page.</span></span> |
| <span data-ttu-id="07b3d-122">Mobilalkalmazás értesítés</span><span class="sxs-lookup"><span data-stu-id="07b3d-122">Mobile app notification</span></span> |<span data-ttu-id="07b3d-123">A felhasználó okostelefon egy ellenőrzési kérés érkezik.</span><span class="sxs-lookup"><span data-stu-id="07b3d-123">A verification request is sent to a user’s smart phone.</span></span> <span data-ttu-id="07b3d-124">A felhasználó a PIN-kód kerül, ha szükséges, majd kiválasztja **ellenőrizze** meg a mobilalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="07b3d-124">The user enters a PIN if necessary then selects **Verify** on the mobile app.</span></span> |
| <span data-ttu-id="07b3d-125">Mobilalkalmazás ellenőrzőkódot</span><span class="sxs-lookup"><span data-stu-id="07b3d-125">Mobile app verification code</span></span> |<span data-ttu-id="07b3d-126">A mobilalkalmazás, amelyen a felhasználó intelligens telefonon, 30 másodperces megváltoztató ellenőrzőkódot jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="07b3d-126">The mobile app, which is running on a user’s smart phone, displays a verification code that changes every 30 seconds.</span></span> <span data-ttu-id="07b3d-127">A felhasználó megkeresi a legújabb kódot, és megadja azt a bejelentkezési oldalon.</span><span class="sxs-lookup"><span data-stu-id="07b3d-127">The user finds the most recent code and enters it on the sign-in page.</span></span> |
| <span data-ttu-id="07b3d-128">Harmadik féltől származó OATH jogkivonatokkal</span><span class="sxs-lookup"><span data-stu-id="07b3d-128">Third-party OATH tokens</span></span> | <span data-ttu-id="07b3d-129">Az Azure multi-factor Authentication kiszolgáló beállítható úgy, hogy fogadja el a harmadik fél hitelesítési módszerek.</span><span class="sxs-lookup"><span data-stu-id="07b3d-129">Azure Multi-Factor Authentication Server can be configured to accept third-party verification methods.</span></span> |

<span data-ttu-id="07b3d-130">Az Azure multi-factor Authentication választható ellenőrzési módszereket biztosít a felhő- és a kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="07b3d-130">Azure Multi-Factor Authentication provides selectable verification methods for both cloud and server.</span></span> <span data-ttu-id="07b3d-131">Kiválaszthatja, melyik módszerei érhetőek el a felhasználók számára: telefonhívás, szöveges, alkalmazásban megjelenő értesítésre vagy alkalmazás kódokat.</span><span class="sxs-lookup"><span data-stu-id="07b3d-131">You can choose which methods are available for your users: phone call, text, app notification, or app codes.</span></span> <span data-ttu-id="07b3d-132">További információkért lásd: [választható hitelesítési módszerek](multi-factor-authentication-whats-next.md#selectable-verification-methods).</span><span class="sxs-lookup"><span data-stu-id="07b3d-132">For more information, see [selectable verification methods](multi-factor-authentication-whats-next.md#selectable-verification-methods).</span></span>

## <a name="next-steps"></a><span data-ttu-id="07b3d-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="07b3d-133">Next steps</span></span>

- <span data-ttu-id="07b3d-134">Olvassa el a különböző [a Azure multi-factor Authentication verziók és használat](multi-factor-authentication-versions-plans.md)</span><span class="sxs-lookup"><span data-stu-id="07b3d-134">Read about the different [versions and consumption methods for Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md)</span></span>

- <span data-ttu-id="07b3d-135">Válassza ki, hogy az Azure MFA telepítendő [a felhőben, vagy a helyszíni](multi-factor-authentication-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="07b3d-135">Choose whether to deploy Azure MFA [in the cloud or on-premises](multi-factor-authentication-get-started.md)</span></span>

- <span data-ttu-id="07b3d-136">Olvasási válaszok az alkalmazásával kapcsolatos [gyakran ismételt kérdések](multi-factor-authentication-faq.md)</span><span class="sxs-lookup"><span data-stu-id="07b3d-136">Read answers for [Frequently asked questions](multi-factor-authentication-faq.md)</span></span>