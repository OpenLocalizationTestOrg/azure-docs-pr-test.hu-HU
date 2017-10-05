---
title: "További tudnivalók a kétlépéses ellenőrzést, az Azure MFA |} Microsoft Docs"
description: "Mi az Azure multi-factor Authentication, MFA, további információt a multi-factor Authentication ügyfél és a különböző módszereket és verziók miért érdemes használni. "
keywords: "az MFA, multi-factor Authentication áttekintése, mi az az mfa bemutatása"
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: c40d7a34-1274-4496-96b0-784850c06e9b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/03/2017
ms.author: kgremban
ms.openlocfilehash: 7334ab5b278c3339fdbc2e363fd5c609604d3e14
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="what-is-azure-multi-factor-authentication"></a><span data-ttu-id="c11cd-104">Mi az az Azure Multi-Factor Authentication?</span><span class="sxs-lookup"><span data-stu-id="c11cd-104">What is Azure Multi-Factor Authentication?</span></span>
<span data-ttu-id="c11cd-105">Kétlépéses ellenőrzés, hogy egynél több ellenőrzési módszert igényel, és a kritikus fontosságú második biztonsági réteget ad hozzá felhasználói bejelentkezéseket és tranzakciókat hitelesítési mód.</span><span class="sxs-lookup"><span data-stu-id="c11cd-105">Two-step verification is a method of authentication that requires more than one verification method and adds a critical second layer of security to user sign-ins and transactions.</span></span> <span data-ttu-id="c11cd-106">Működését tekintve a igénylő bármely két vagy több, az alábbi hitelesítési módszerek:</span><span class="sxs-lookup"><span data-stu-id="c11cd-106">It works by requiring any two or more of the following verification methods:</span></span>

* <span data-ttu-id="c11cd-107">Valami tudja (általában jelszót)</span><span class="sxs-lookup"><span data-stu-id="c11cd-107">Something you know (typically a password)</span></span>
* <span data-ttu-id="c11cd-108">Valami (megbízható eszközzel rendelkezik, amely nem egyszerű ismétlődik, például a telefon)</span><span class="sxs-lookup"><span data-stu-id="c11cd-108">Something you have (a trusted device that is not easily duplicated, like a phone)</span></span>
* <span data-ttu-id="c11cd-109">Valami áll (biometria)</span><span class="sxs-lookup"><span data-stu-id="c11cd-109">Something you are (biometrics)</span></span>

<span data-ttu-id="c11cd-110"><center>![Felhasználónév és jelszó](./media/multi-factor-authentication/pword.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![tanúsítványok](./media/multi-factor-authentication/phone.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![Phone intelligens](./media/multi-factor-authentication/hware.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![intelligens kártya](./media/multi-factor-authentication/smart.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![virtuális intelligens kártya](./media/multi-factor-authentication/vsmart.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![felhasználónév és jelszó](./media/multi-factor-authentication/cert.png)</center></span><span class="sxs-lookup"><span data-stu-id="c11cd-110"><center>![Username and Password](./media/multi-factor-authentication/pword.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Certificates](./media/multi-factor-authentication/phone.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Smart Phone](./media/multi-factor-authentication/hware.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Smart Card](./media/multi-factor-authentication/smart.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Virtual Smart Card](./media/multi-factor-authentication/vsmart.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Username and Password](./media/multi-factor-authentication/cert.png)</center></span></span>

<span data-ttu-id="c11cd-111">Az Azure Multi-Factor Authentication (MFA) a Microsoft kétlépéses hitelesítési megoldása.</span><span class="sxs-lookup"><span data-stu-id="c11cd-111">Azure Multi-Factor Authentication (MFA) is Microsoft's two-step verification solution.</span></span> <span data-ttu-id="c11cd-112">Az Azure MFA segíti az adatok és alkalmazások védelmét az illetéktelen hozzáférésekkel szemben, miközben a felhasználói igényeknek megfelelő, egyszerű bejelentkezési folyamat használatát teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="c11cd-112">Azure MFA helps safeguard access to data and applications while meeting user demand for a simple sign-in process.</span></span> <span data-ttu-id="c11cd-113">Számos (például telefonos megerősítést, szöveges üzenetet vagy mobilalkalmazást használó) ellenőrzési módszerének köszönhetően erős hitelesítést biztosít.</span><span class="sxs-lookup"><span data-stu-id="c11cd-113">It delivers strong authentication via a range of verification methods, including phone call, text message, or mobile app verification.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/WA-MFA-Overview/player]
>
>

## <a name="why-use-azure-multi-factor-authentication"></a><span data-ttu-id="c11cd-114">Miért érdemes használni az Azure multi-factor Authentication?</span><span class="sxs-lookup"><span data-stu-id="c11cd-114">Why use Azure Multi-Factor Authentication?</span></span>
<span data-ttu-id="c11cd-115">Ma, több mint legalább egyszer személyek egyre csatlakoznak.</span><span class="sxs-lookup"><span data-stu-id="c11cd-115">Today, more than ever, people are increasingly connected.</span></span> <span data-ttu-id="c11cd-116">Az intelligens telefonok, táblagépek, laptopok és számítógépek személyek közül számos különböző hogyan a rendszer hamarosan csatlakozni, és bármikor maradhat.</span><span class="sxs-lookup"><span data-stu-id="c11cd-116">With smart phones, tablets, laptops, and PCs, people have several different options on how they are going to connect and stay connected at any time.</span></span> <span data-ttu-id="c11cd-117">Személyek férhetnek a fiókok és az alkalmazások bárhonnan, ami azt jelenti, hogy hatékonyabb munkavégzésben, és az ügyfelek kiszolgálásához jobban.</span><span class="sxs-lookup"><span data-stu-id="c11cd-117">People can access their accounts and applications from anywhere, which means that they can get more work done and serve their customers better.</span></span>

<span data-ttu-id="c11cd-118">Az Azure multi-factor Authentication egy könnyen használható, méretezhető és megbízható megoldás, amely egy második hitelesítési módszer, így a felhasználók mindig védett.</span><span class="sxs-lookup"><span data-stu-id="c11cd-118">Azure Multi-Factor Authentication is an easy to use, scalable, and reliable solution that provides a second method of authentication so your users are always protected.</span></span>

| ![Egyszerű használat](./media/multi-factor-authentication/simple.png) | ![Méretezhető](./media/multi-factor-authentication/scalable.png) | ![Mindig védve](./media/multi-factor-authentication/protected.png) | ![Megbízható](./media/multi-factor-authentication/reliable.png) |
|:---:|:---:|:---:|:---:|
| <span data-ttu-id="c11cd-123">**Könnyen használható.**</span><span class="sxs-lookup"><span data-stu-id="c11cd-123">**Easy to use**</span></span> |<span data-ttu-id="c11cd-124">**Méretezhető**</span><span class="sxs-lookup"><span data-stu-id="c11cd-124">**Scalable**</span></span> |<span data-ttu-id="c11cd-125">**Mindig védve**</span><span class="sxs-lookup"><span data-stu-id="c11cd-125">**Always Protected**</span></span> |<span data-ttu-id="c11cd-126">**Megbízható**</span><span class="sxs-lookup"><span data-stu-id="c11cd-126">**Reliable**</span></span> |

* <span data-ttu-id="c11cd-127">**Könnyen használható** -Azure multi-factor Authentication egy egyszerű beállítása és használata.</span><span class="sxs-lookup"><span data-stu-id="c11cd-127">**Easy to Use** - Azure Multi-Factor Authentication is simple to set up and use.</span></span> <span data-ttu-id="c11cd-128">A további védelem a Azure multi-factor Authentication lehetővé teszi, hogy a felhasználók a saját eszközök kezelésére.</span><span class="sxs-lookup"><span data-stu-id="c11cd-128">The extra protection that comes with Azure Multi-Factor Authentication allows users to manage their own devices.</span></span> <span data-ttu-id="c11cd-129">Ajánlott az összes sok esetben azt is beállítható néhány egyszerű kattintással.</span><span class="sxs-lookup"><span data-stu-id="c11cd-129">Best of all, in many instances it can be set up with just a few simple clicks.</span></span>
* <span data-ttu-id="c11cd-130">**Méretezhető** -Azure multi-factor Authentication használja ki a felhőt, és integrálható a helyszíni AD és az egyéni alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="c11cd-130">**Scalable** - Azure Multi-Factor Authentication uses the power of the cloud and integrates with your on-premises AD and custom apps.</span></span> <span data-ttu-id="c11cd-131">Ez a védelem még akkor is ki van bővítve a nagy mennyiségű, a kritikus fontosságú forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="c11cd-131">This protection is even extended to your high-volume, mission-critical scenarios.</span></span>
* <span data-ttu-id="c11cd-132">**Mindig védett** -Azure multi-factor Authentication használata a legmagasabb iparági szabványoknak megfelelő erős hitelesítést nyújt.</span><span class="sxs-lookup"><span data-stu-id="c11cd-132">**Always Protected** - Azure Multi-Factor Authentication provides strong authentication using the highest industry standards.</span></span>
* <span data-ttu-id="c11cd-133">**Megbízható** -garantáljuk Azure multi-factor Authentication 99,9 %-os rendelkezésre állását.</span><span class="sxs-lookup"><span data-stu-id="c11cd-133">**Reliable** - We guarantee 99.9% availability of Azure Multi-Factor Authentication.</span></span> <span data-ttu-id="c11cd-134">A szolgáltatás nem érhető el tekintendő, ha nem jelenik meg, vagy a kétlépéses ellenőrzéshez hitelesítési kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="c11cd-134">The service is considered unavailable when it is unable to receive or process verification requests for the two-step verification.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Windows-Azure-Multi-Factor-Authentication/player]


## <a name="next-steps"></a><span data-ttu-id="c11cd-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c11cd-135">Next steps</span></span>

- <span data-ttu-id="c11cd-136">További tudnivalók [Azure multi-factor Authentication működése](multi-factor-authentication-how-it-works.md)</span><span class="sxs-lookup"><span data-stu-id="c11cd-136">Learn about [how Azure Multi-Factor Authentication works](multi-factor-authentication-how-it-works.md)</span></span>

- <span data-ttu-id="c11cd-137">Olvassa el a különböző [a Azure multi-factor Authentication verziók és használat](multi-factor-authentication-versions-plans.md)</span><span class="sxs-lookup"><span data-stu-id="c11cd-137">Read about the different [versions and consumption methods for Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md)</span></span>
