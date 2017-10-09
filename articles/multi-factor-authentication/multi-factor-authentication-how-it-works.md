---
title: "a multi-factor Authentication - aaaAzure annak működéséről"
description: "Azure multi-factor Authentication segítségével biztonságos működés érdekében hozzáférés toodata és alkalmazások mellett egyszerű bejelentkezési folyamatot a felhasználó igény szerint. További biztonságot nyújt, azzal, hogy egy második hitelesítési mód, és kézbesíti az erős hitelesítés keresztül egyszerűen ellenőrzési lehetőségek közül."
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
ms.openlocfilehash: 82f234fb86f145c42e8e56b8bdd2d61720c9ff2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-azure-multi-factor-authentication-works"></a><span data-ttu-id="5e5a8-104">Azure multi-factor Authentication működése</span><span class="sxs-lookup"><span data-stu-id="5e5a8-104">How Azure Multi-Factor Authentication works</span></span>
<span data-ttu-id="5e5a8-105">hello biztonságát kétlépéses ellenőrzést, a réteges megközelítésének rejlik.</span><span class="sxs-lookup"><span data-stu-id="5e5a8-105">hello security of two-step verification lies in its layered approach.</span></span> <span data-ttu-id="5e5a8-106">Jelentős kihívás a támadók számára a több hitelesítési tényezők veszélyeztetése mutatja be.</span><span class="sxs-lookup"><span data-stu-id="5e5a8-106">Compromising multiple authentication factors presents a significant challenge for attackers.</span></span> <span data-ttu-id="5e5a8-107">Még ha egy támadó toolearn hello felhasználó jelszavát, akkor nem használható hello megbízható eszköz a birtokában nélkül is.</span><span class="sxs-lookup"><span data-stu-id="5e5a8-107">Even if an attacker manages toolearn hello user's password, it is useless without also having possession of hello trusted device.</span></span> 

![Proofup](./media/multi-factor-authentication-how-it-works/howitworks.png)

<span data-ttu-id="5e5a8-109">Azure multi-factor Authentication segítségével biztonságos működés érdekében hozzáférés toodata és alkalmazások mellett egyszerű bejelentkezési folyamatot a felhasználó igény szerint.</span><span class="sxs-lookup"><span data-stu-id="5e5a8-109">Azure Multi-Factor Authentication helps safeguard access toodata and applications while meeting user demand for a simple sign-in process.</span></span>  <span data-ttu-id="5e5a8-110">További biztonságot nyújt, azzal, hogy egy második hitelesítési mód, és kézbesíti az erős hitelesítés keresztül egyszerűen ellenőrzési lehetőségek közül.</span><span class="sxs-lookup"><span data-stu-id="5e5a8-110">It provides additional security by requiring a second form of authentication and delivers strong authentication via a range of easy verification options.</span></span>


## <a name="methods-available-for-two-step-verification"></a><span data-ttu-id="5e5a8-111">Módszer áll rendelkezésre a kétlépéses ellenőrzéshez</span><span class="sxs-lookup"><span data-stu-id="5e5a8-111">Methods available for two-step verification</span></span>
<span data-ttu-id="5e5a8-112">Ha egy felhasználó bejelentkezik, egy további ellenőrzési toohello felhasználói küld.</span><span class="sxs-lookup"><span data-stu-id="5e5a8-112">When a user signs in, an additional verification is sent toohello user.</span></span>  <span data-ttu-id="5e5a8-113">Az alábbiakban hello a második ellenőrzési célból használt módszerek listáját.</span><span class="sxs-lookup"><span data-stu-id="5e5a8-113">hello following are a list of methods that can be used for this second verification.</span></span>

| <span data-ttu-id="5e5a8-114">Ellenőrzési módszert</span><span class="sxs-lookup"><span data-stu-id="5e5a8-114">Verification Method</span></span> | <span data-ttu-id="5e5a8-115">Leírás</span><span class="sxs-lookup"><span data-stu-id="5e5a8-115">Description</span></span> |
| --- | --- |
| <span data-ttu-id="5e5a8-116">Telefonhívás</span><span class="sxs-lookup"><span data-stu-id="5e5a8-116">Phone call</span></span> |<span data-ttu-id="5e5a8-117">A hívást kezdeményeznek tooa felhasználó regisztrált telefonjára.</span><span class="sxs-lookup"><span data-stu-id="5e5a8-117">A call is placed tooa user’s registered phone.</span></span> <span data-ttu-id="5e5a8-118">hello felhasználói PIN-kód kerül, ha szükséges, majd megnyomja az hello # gombját.</span><span class="sxs-lookup"><span data-stu-id="5e5a8-118">hello user enters a PIN if necessary then presses hello # key.</span></span> |
| <span data-ttu-id="5e5a8-119">Szöveges üzenet</span><span class="sxs-lookup"><span data-stu-id="5e5a8-119">Text message</span></span> |<span data-ttu-id="5e5a8-120">Egy szöveges üzenetet küld tooa felhasználói mobiltelefon egy hatjegyű kódot.</span><span class="sxs-lookup"><span data-stu-id="5e5a8-120">A text message is sent tooa user’s mobile phone with a six-digit code.</span></span> <span data-ttu-id="5e5a8-121">hello felhasználói ezt a kódot ír hello bejelentkezési oldalára.</span><span class="sxs-lookup"><span data-stu-id="5e5a8-121">hello user enters this code on hello sign-in page.</span></span> |
| <span data-ttu-id="5e5a8-122">Mobilalkalmazás értesítés</span><span class="sxs-lookup"><span data-stu-id="5e5a8-122">Mobile app notification</span></span> |<span data-ttu-id="5e5a8-123">Egy ellenőrző kérést küldött tooa felhasználói okostelefon.</span><span class="sxs-lookup"><span data-stu-id="5e5a8-123">A verification request is sent tooa user’s smart phone.</span></span> <span data-ttu-id="5e5a8-124">hello felhasználói PIN-kód kerül, ha szükséges, majd kiválasztja **ellenőrizze** a hello mobilalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="5e5a8-124">hello user enters a PIN if necessary then selects **Verify** on hello mobile app.</span></span> |
| <span data-ttu-id="5e5a8-125">Mobilalkalmazás ellenőrzőkódot</span><span class="sxs-lookup"><span data-stu-id="5e5a8-125">Mobile app verification code</span></span> |<span data-ttu-id="5e5a8-126">hello mobilalkalmazás, amelyen a felhasználó intelligens telefonon, 30 másodperces megváltoztató ellenőrzőkódot jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="5e5a8-126">hello mobile app, which is running on a user’s smart phone, displays a verification code that changes every 30 seconds.</span></span> <span data-ttu-id="5e5a8-127">hello felhasználói hello legutóbbi kód talál, és beszúrja hello bejelentkezési oldalára.</span><span class="sxs-lookup"><span data-stu-id="5e5a8-127">hello user finds hello most recent code and enters it on hello sign-in page.</span></span> |
| <span data-ttu-id="5e5a8-128">Harmadik féltől származó OATH jogkivonatokkal</span><span class="sxs-lookup"><span data-stu-id="5e5a8-128">Third-party OATH tokens</span></span> | <span data-ttu-id="5e5a8-129">Az Azure multi-factor Authentication kiszolgáló tooaccept beállított külső hitelesítési módszerek lehet.</span><span class="sxs-lookup"><span data-stu-id="5e5a8-129">Azure Multi-Factor Authentication Server can be configured tooaccept third-party verification methods.</span></span> |

<span data-ttu-id="5e5a8-130">Az Azure multi-factor Authentication választható ellenőrzési módszereket biztosít a felhő- és a kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="5e5a8-130">Azure Multi-Factor Authentication provides selectable verification methods for both cloud and server.</span></span> <span data-ttu-id="5e5a8-131">Kiválaszthatja, melyik módszerei érhetőek el a felhasználók számára: telefonhívás, szöveges, alkalmazásban megjelenő értesítésre vagy alkalmazás kódokat.</span><span class="sxs-lookup"><span data-stu-id="5e5a8-131">You can choose which methods are available for your users: phone call, text, app notification, or app codes.</span></span> <span data-ttu-id="5e5a8-132">További információkért lásd: [választható hitelesítési módszerek](multi-factor-authentication-whats-next.md#selectable-verification-methods).</span><span class="sxs-lookup"><span data-stu-id="5e5a8-132">For more information, see [selectable verification methods](multi-factor-authentication-whats-next.md#selectable-verification-methods).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5e5a8-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5e5a8-133">Next steps</span></span>

- <span data-ttu-id="5e5a8-134">További információ a különböző hello [a Azure multi-factor Authentication verziók és használat](multi-factor-authentication-versions-plans.md)</span><span class="sxs-lookup"><span data-stu-id="5e5a8-134">Read about hello different [versions and consumption methods for Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md)</span></span>

- <span data-ttu-id="5e5a8-135">Válasszon e toodeploy Azure MFA [hello felhőalapú vagy helyszíni](multi-factor-authentication-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="5e5a8-135">Choose whether toodeploy Azure MFA [in hello cloud or on-premises](multi-factor-authentication-get-started.md)</span></span>

- <span data-ttu-id="5e5a8-136">Olvasási válaszok az alkalmazásával kapcsolatos [gyakran ismételt kérdések](multi-factor-authentication-faq.md)</span><span class="sxs-lookup"><span data-stu-id="5e5a8-136">Read answers for [Frequently asked questions](multi-factor-authentication-faq.md)</span></span>