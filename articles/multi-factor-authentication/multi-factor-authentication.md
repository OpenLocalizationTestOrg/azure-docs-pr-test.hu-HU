---
title: "tudnivalók a kétlépéses ellenőrzést, az Azure MFA aaaLearn |} Microsoft Docs"
description: "Mi az Azure multi-factor Authentication, MFA, további információt hello multi-factor Authentication ügyfél és hello különböző módszereket és verziók miért érdemes használni. "
keywords: "Mi az az mfa bemutatása tooMFA többtényezős hitelesítés áttekintése"
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
ms.openlocfilehash: a91b8d6941d2b6ce72a789a97c57e10e594e7ada
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-multi-factor-authentication"></a><span data-ttu-id="00a03-104">Mi az az Azure Multi-Factor Authentication?</span><span class="sxs-lookup"><span data-stu-id="00a03-104">What is Azure Multi-Factor Authentication?</span></span>
<span data-ttu-id="00a03-105">Kétlépéses ellenőrzés, hogy egynél több ellenőrzési módszert igényel, és a kritikus fontosságú második réteget biztonsági toouser bejelentkezéseket és tranzakciókat ad hitelesítési mód.</span><span class="sxs-lookup"><span data-stu-id="00a03-105">Two-step verification is a method of authentication that requires more than one verification method and adds a critical second layer of security toouser sign-ins and transactions.</span></span> <span data-ttu-id="00a03-106">Azzal, hogy bármely két vagy több ellenőrzési módszer a következő hello működik:</span><span class="sxs-lookup"><span data-stu-id="00a03-106">It works by requiring any two or more of hello following verification methods:</span></span>

* <span data-ttu-id="00a03-107">Valami tudja (általában jelszót)</span><span class="sxs-lookup"><span data-stu-id="00a03-107">Something you know (typically a password)</span></span>
* <span data-ttu-id="00a03-108">Valami (megbízható eszközzel rendelkezik, amely nem egyszerű ismétlődik, például a telefon)</span><span class="sxs-lookup"><span data-stu-id="00a03-108">Something you have (a trusted device that is not easily duplicated, like a phone)</span></span>
* <span data-ttu-id="00a03-109">Valami áll (biometria)</span><span class="sxs-lookup"><span data-stu-id="00a03-109">Something you are (biometrics)</span></span>

<span data-ttu-id="00a03-110"><center>![Felhasználónév és jelszó](./media/multi-factor-authentication/pword.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![tanúsítványok](./media/multi-factor-authentication/phone.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![Phone intelligens](./media/multi-factor-authentication/hware.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![intelligens kártya](./media/multi-factor-authentication/smart.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![virtuális intelligens kártya](./media/multi-factor-authentication/vsmart.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![felhasználónév és jelszó](./media/multi-factor-authentication/cert.png)</center></span><span class="sxs-lookup"><span data-stu-id="00a03-110"><center>![Username and Password](./media/multi-factor-authentication/pword.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Certificates](./media/multi-factor-authentication/phone.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Smart Phone](./media/multi-factor-authentication/hware.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Smart Card](./media/multi-factor-authentication/smart.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Virtual Smart Card](./media/multi-factor-authentication/vsmart.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Username and Password](./media/multi-factor-authentication/cert.png)</center></span></span>

<span data-ttu-id="00a03-111">Az Azure Multi-Factor Authentication (MFA) a Microsoft kétlépéses hitelesítési megoldása.</span><span class="sxs-lookup"><span data-stu-id="00a03-111">Azure Multi-Factor Authentication (MFA) is Microsoft's two-step verification solution.</span></span> <span data-ttu-id="00a03-112">Az Azure MFA segít a biztonságos működés érdekében hozzáférés toodata és alkalmazások mellett egyszerű bejelentkezési folyamatot a felhasználó igény szerint.</span><span class="sxs-lookup"><span data-stu-id="00a03-112">Azure MFA helps safeguard access toodata and applications while meeting user demand for a simple sign-in process.</span></span> <span data-ttu-id="00a03-113">Számos (például telefonos megerősítést, szöveges üzenetet vagy mobilalkalmazást használó) ellenőrzési módszerének köszönhetően erős hitelesítést biztosít.</span><span class="sxs-lookup"><span data-stu-id="00a03-113">It delivers strong authentication via a range of verification methods, including phone call, text message, or mobile app verification.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/WA-MFA-Overview/player]
>
>

## <a name="why-use-azure-multi-factor-authentication"></a><span data-ttu-id="00a03-114">Miért érdemes használni az Azure multi-factor Authentication?</span><span class="sxs-lookup"><span data-stu-id="00a03-114">Why use Azure Multi-Factor Authentication?</span></span>
<span data-ttu-id="00a03-115">Ma, több mint legalább egyszer személyek egyre csatlakoznak.</span><span class="sxs-lookup"><span data-stu-id="00a03-115">Today, more than ever, people are increasingly connected.</span></span> <span data-ttu-id="00a03-116">Az intelligens telefonok, táblagépek, laptopok és számítógépek személyek közül több különböző hogyan azok tooconnect fog, és bármikor maradhat.</span><span class="sxs-lookup"><span data-stu-id="00a03-116">With smart phones, tablets, laptops, and PCs, people have several different options on how they are going tooconnect and stay connected at any time.</span></span> <span data-ttu-id="00a03-117">Személyek férhetnek a fiókok és az alkalmazások bárhonnan, ami azt jelenti, hogy hatékonyabb munkavégzésben, és az ügyfelek kiszolgálásához jobban.</span><span class="sxs-lookup"><span data-stu-id="00a03-117">People can access their accounts and applications from anywhere, which means that they can get more work done and serve their customers better.</span></span>

<span data-ttu-id="00a03-118">Az Azure multi-factor Authentication könnyen toouse, méretezhető, és megbízható megoldás, amely biztosít egy második hitelesítési módszer, így a felhasználók mindig védettek.</span><span class="sxs-lookup"><span data-stu-id="00a03-118">Azure Multi-Factor Authentication is an easy toouse, scalable, and reliable solution that provides a second method of authentication so your users are always protected.</span></span>

| ![Egyszerű tooUse](./media/multi-factor-authentication/simple.png) | ![Méretezhető](./media/multi-factor-authentication/scalable.png) | ![Mindig védve](./media/multi-factor-authentication/protected.png) | ![Megbízható](./media/multi-factor-authentication/reliable.png) |
|:---:|:---:|:---:|:---:|
| <span data-ttu-id="00a03-123">**Egyszerű toouse**</span><span class="sxs-lookup"><span data-stu-id="00a03-123">**Easy toouse**</span></span> |<span data-ttu-id="00a03-124">**Méretezhető**</span><span class="sxs-lookup"><span data-stu-id="00a03-124">**Scalable**</span></span> |<span data-ttu-id="00a03-125">**Mindig védve**</span><span class="sxs-lookup"><span data-stu-id="00a03-125">**Always Protected**</span></span> |<span data-ttu-id="00a03-126">**Megbízható**</span><span class="sxs-lookup"><span data-stu-id="00a03-126">**Reliable**</span></span> |

* <span data-ttu-id="00a03-127">**Egyszerű tooUse** -Azure multi-factor Authentication egy egyszerű tooset be és használja.</span><span class="sxs-lookup"><span data-stu-id="00a03-127">**Easy tooUse** - Azure Multi-Factor Authentication is simple tooset up and use.</span></span> <span data-ttu-id="00a03-128">hello Azure multi-factor Authentication a további védelem lehetővé teszi a felhasználók toomanage a saját eszközeiket.</span><span class="sxs-lookup"><span data-stu-id="00a03-128">hello extra protection that comes with Azure Multi-Factor Authentication allows users toomanage their own devices.</span></span> <span data-ttu-id="00a03-129">Ajánlott az összes sok esetben azt is beállítható néhány egyszerű kattintással.</span><span class="sxs-lookup"><span data-stu-id="00a03-129">Best of all, in many instances it can be set up with just a few simple clicks.</span></span>
* <span data-ttu-id="00a03-130">**Méretezhető** -Azure multi-factor Authentication hello felhő hello fogyaszt, és integrálható a helyszíni AD és az egyéni alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="00a03-130">**Scalable** - Azure Multi-Factor Authentication uses hello power of hello cloud and integrates with your on-premises AD and custom apps.</span></span> <span data-ttu-id="00a03-131">Ez a védelem még akkor is ki van bővítve tooyour nagy mennyiségű, a kritikus fontosságú forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="00a03-131">This protection is even extended tooyour high-volume, mission-critical scenarios.</span></span>
* <span data-ttu-id="00a03-132">**Mindig védett** -Azure multi-factor Authentication hello legmagasabb ipari szabványok használatával erős hitelesítést nyújt.</span><span class="sxs-lookup"><span data-stu-id="00a03-132">**Always Protected** - Azure Multi-Factor Authentication provides strong authentication using hello highest industry standards.</span></span>
* <span data-ttu-id="00a03-133">**Megbízható** -garantáljuk Azure multi-factor Authentication 99,9 %-os rendelkezésre állását.</span><span class="sxs-lookup"><span data-stu-id="00a03-133">**Reliable** - We guarantee 99.9% availability of Azure Multi-Factor Authentication.</span></span> <span data-ttu-id="00a03-134">hello szolgáltatás tekinthető nem érhető el, ha nem tudja a kétlépéses ellenőrzéshez hello tooreceive vagy folyamat ellenőrzési kérelmekről.</span><span class="sxs-lookup"><span data-stu-id="00a03-134">hello service is considered unavailable when it is unable tooreceive or process verification requests for hello two-step verification.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Windows-Azure-Multi-Factor-Authentication/player]


## <a name="next-steps"></a><span data-ttu-id="00a03-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="00a03-135">Next steps</span></span>

- <span data-ttu-id="00a03-136">További tudnivalók [Azure multi-factor Authentication működése](multi-factor-authentication-how-it-works.md)</span><span class="sxs-lookup"><span data-stu-id="00a03-136">Learn about [how Azure Multi-Factor Authentication works](multi-factor-authentication-how-it-works.md)</span></span>

- <span data-ttu-id="00a03-137">További információ a különböző hello [a Azure multi-factor Authentication verziók és használat](multi-factor-authentication-versions-plans.md)</span><span class="sxs-lookup"><span data-stu-id="00a03-137">Read about hello different [versions and consumption methods for Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md)</span></span>
