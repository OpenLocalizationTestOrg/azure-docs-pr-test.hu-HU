---
title: "aaaAzure Active Directory hibrid identitáskezelési elrendezésével kapcsolatos szempontok - többtényezős hitelesítési követelmények meghatározása"
description: "Feltételes hozzáférés-vezérlést Azure Active Directory ellenőrzi hello megadott feltételek hello felhasználói hitelesítés során, és mielőtt engedélyezi a hozzáférést toohello alkalmazás kiválasztása. Ha ezek a feltételek teljesülnek, hello felhasználó hitelesítése és hozzáférési toohello alkalmazás engedélyezve."
documentationcenter: 
services: active-directory
author: femila
manager: billmath
editor: 
ms.assetid: 9c59fda9-47d0-4c7e-b3e7-3575c29beabe
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 49fa7b43772fb3a2d6664747477c60a34cddde2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="determine-multi-factor-authentication-requirements-for-your-hybrid-identity-solution"></a><span data-ttu-id="7508f-104">A hibrid identitáskezelési megoldás a többtényezős hitelesítési követelmények meghatározása</span><span class="sxs-lookup"><span data-stu-id="7508f-104">Determine multi-factor authentication requirements for your hybrid identity solution</span></span>
<span data-ttu-id="7508f-105">A világ mobilitási, a felhasználói adatok és alkalmazások hello felhőben, és egy eszközről ezek az információk védelme vált kiemelkedő.</span><span class="sxs-lookup"><span data-stu-id="7508f-105">In this world of mobility, with users accessing data and applications in hello cloud and from any device, securing this information has become paramount.</span></span>  <span data-ttu-id="7508f-106">Minden nap van egy új főcím kapcsolatos biztonsági problémák.</span><span class="sxs-lookup"><span data-stu-id="7508f-106">Every day there is a new headline about a security breach.</span></span>  <span data-ttu-id="7508f-107">Bár nem garantálja az ilyen problémák elleni van, a többtényezős hitelesítés további réteget biztosít biztonsági toohelp megakadályozzák e problémák.</span><span class="sxs-lookup"><span data-stu-id="7508f-107">Although, there is no guarantee against such breaches, multi-factor authentication, provides an additional layer of security toohelp prevent these breaches.</span></span>
<span data-ttu-id="7508f-108">Indítsa el a multi-factor authentication hello szervezetek szükséges követelmények értékelésekor.</span><span class="sxs-lookup"><span data-stu-id="7508f-108">Start by evaluating hello organizations requirements for multi-factor authentication.</span></span> <span data-ttu-id="7508f-109">Ez azt jelenti, hogy mi az hello szervezet közben toosecure.</span><span class="sxs-lookup"><span data-stu-id="7508f-109">That is, what is hello organization trying toosecure.</span></span>  <span data-ttu-id="7508f-110">Ez a kiértékelés fontos toodefine hello műszaki követelményeiben és hello szervezetek felhasználók a multi-factor authentication.</span><span class="sxs-lookup"><span data-stu-id="7508f-110">This evaluation is important toodefine hello technical requirements for setting up and enabling hello organizations users for multi-factor authentication.</span></span>

> [!NOTE]
> <span data-ttu-id="7508f-111">Ha nem ismeri a többtényezős hitelesítés és a hatása, erősen ajánlott, hogy olvassa el a hello cikk [Mi az Azure multi-factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md) előzetes toocontinue a fejezet elolvasása.</span><span class="sxs-lookup"><span data-stu-id="7508f-111">If you are not familiar with MFA and what it does, it is strongly recommended that you read hello article [What is Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md) prior toocontinue reading this section.</span></span>
> 
> 

<span data-ttu-id="7508f-112">Győződjön meg arról, hogy tooanswer hello következő:</span><span class="sxs-lookup"><span data-stu-id="7508f-112">Make sure tooanswer hello following:</span></span>

* <span data-ttu-id="7508f-113">A vállalat próbál toosecure Microsoft-alkalmazások?</span><span class="sxs-lookup"><span data-stu-id="7508f-113">Is your company trying toosecure Microsoft apps?</span></span> 
* <span data-ttu-id="7508f-114">Hogyan közzétett ezeket az alkalmazásokat?</span><span class="sxs-lookup"><span data-stu-id="7508f-114">How these apps are published?</span></span>
* <span data-ttu-id="7508f-115">A vállalat biztosítja a távelérés tooallow alkalmazottak tooaccess a helyszíni alkalmazások?</span><span class="sxs-lookup"><span data-stu-id="7508f-115">Does your company provide remote access tooallow employees tooaccess on-premises apps?</span></span>

<span data-ttu-id="7508f-116">Ha igen, milyen típusú távelérési? Szükség tooevaluate, amelyben hello elérő felhasználók számára az alkalmazások található.</span><span class="sxs-lookup"><span data-stu-id="7508f-116">If yes, what type of remote access?You also need tooevaluate where hello users who are accessing these applications will be located.</span></span> <span data-ttu-id="7508f-117">Az értékelés egy másik fontos lépés toodefine hello megfelelő a multi-factor authentication stratégia.</span><span class="sxs-lookup"><span data-stu-id="7508f-117">This evaluation is another important step toodefine hello proper multi-factor authentication strategy.</span></span> <span data-ttu-id="7508f-118">Győződjön meg arról, hogy tooanswer hello a következő kérdéseket:</span><span class="sxs-lookup"><span data-stu-id="7508f-118">Make sure tooanswer hello following questions:</span></span>

* <span data-ttu-id="7508f-119">Hol található a hello felhasználók toobe is?</span><span class="sxs-lookup"><span data-stu-id="7508f-119">Where are hello users going toobe located?</span></span>
* <span data-ttu-id="7508f-120">Ezek lehetnek bárhol?</span><span class="sxs-lookup"><span data-stu-id="7508f-120">Can they be located anywhere?</span></span>
* <span data-ttu-id="7508f-121">Nem a vállalat szeretne tooestablish korlátozások toohello felhasználó földrajzi helye szerint?</span><span class="sxs-lookup"><span data-stu-id="7508f-121">Does your company want tooestablish restrictions according toohello user’s location?</span></span>

<span data-ttu-id="7508f-122">Ezek a követelmények elsajátítása után fontos tooalso értékelje ki a multi-factor authentication hello felhasználói követelmények.</span><span class="sxs-lookup"><span data-stu-id="7508f-122">Once you understand these requirements, it is important tooalso evaluate hello user’s requirements for multi-factor authentication.</span></span> <span data-ttu-id="7508f-123">Ez a kiértékelés fontos, mert azt határozza meg a multi-factor authentication terítésével hello követelményei.</span><span class="sxs-lookup"><span data-stu-id="7508f-123">This evaluation is important because it will define hello requirements for rolling out multi-factor authentication.</span></span> <span data-ttu-id="7508f-124">Győződjön meg arról, hogy tooanswer hello a következő kérdéseket:</span><span class="sxs-lookup"><span data-stu-id="7508f-124">Make sure tooanswer hello following questions:</span></span>

* <span data-ttu-id="7508f-125">Jártas a hello felhasználók a többtényezős hitelesítés?</span><span class="sxs-lookup"><span data-stu-id="7508f-125">Are hello users familiar with multi-factor authentication?</span></span>
* <span data-ttu-id="7508f-126">Néhány felhasználását lesz szükség tooprovide további hitelesítési?</span><span class="sxs-lookup"><span data-stu-id="7508f-126">Will some uses be required tooprovide additional authentication?</span></span>  
  * <span data-ttu-id="7508f-127">Ha igen, az összes hello idő, a külső hálózatokat vagy férnek hozzá bizonyos alkalmazásokat, vagy más feltételek mellett?</span><span class="sxs-lookup"><span data-stu-id="7508f-127">If yes, all hello time, when coming from external networks, or accessing specific applications, or under other conditions?</span></span>
* <span data-ttu-id="7508f-128">Hello felhasználóinak kell hogyan képzési toosetup és alkalmazzon a multi-factor authentication?</span><span class="sxs-lookup"><span data-stu-id="7508f-128">Will hello users require training on how toosetup and implement multi-factor authentication?</span></span>
* <span data-ttu-id="7508f-129">Mik azok a hello főbb forgatókönyvek, hogy a vállalat szeretne-e a felhasználóik tooenable többtényezős hitelesítést?</span><span class="sxs-lookup"><span data-stu-id="7508f-129">What are hello key scenarios that your company wants tooenable multi-factor authentication for their users?</span></span>

<span data-ttu-id="7508f-130">Hello előző kérdések megválaszolásával után nem tud toounderstand fogja már megvalósította a multi-factor authentication helyszíni esetén.</span><span class="sxs-lookup"><span data-stu-id="7508f-130">After answering hello previous questions, you will be able toounderstand if there are multi-factor authentication already implemented on-premises.</span></span> <span data-ttu-id="7508f-131">Ez a kiértékelés fontos toodefine hello műszaki követelményeiben és hello szervezetek felhasználók a multi-factor authentication.</span><span class="sxs-lookup"><span data-stu-id="7508f-131">This evaluation is important toodefine hello technical requirements for setting up and enabling hello organizations users for multi-factor authentication.</span></span> <span data-ttu-id="7508f-132">Győződjön meg arról, hogy tooanswer hello a következő kérdéseket:</span><span class="sxs-lookup"><span data-stu-id="7508f-132">Make sure tooanswer hello following questions:</span></span>

* <span data-ttu-id="7508f-133">A vállalatának meg kell tooprotect rendszerjogosultságú fiókok az MFA Használatát?</span><span class="sxs-lookup"><span data-stu-id="7508f-133">Does your company need tooprotect privileged accounts with MFA?</span></span>
* <span data-ttu-id="7508f-134">A vállalatának meg kell tooenable MFA egyes alkalmazás megfelelőségi okokból?</span><span class="sxs-lookup"><span data-stu-id="7508f-134">Does your company need tooenable MFA for certain application for compliance reasons?</span></span>
* <span data-ttu-id="7508f-135">A vállalatának meg kell tooenable MFA ezen alkalmazás-vagy csak a rendszergazdák az összes jogosult felhasználók számára?</span><span class="sxs-lookup"><span data-stu-id="7508f-135">Does your company need tooenable MFA for all eligible users of these application or only administrators?</span></span>
* <span data-ttu-id="7508f-136">Szükség van mindig engedélyezve van az MFA- vagy csak amikor hello felhasználók bejelentkeznek a vállalati hálózaton kívül?</span><span class="sxs-lookup"><span data-stu-id="7508f-136">Do you need have MFA always enabled or only when hello users are logged outside of your corporate network?</span></span>

## <a name="next-steps"></a><span data-ttu-id="7508f-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7508f-137">Next steps</span></span>
[<span data-ttu-id="7508f-138">A hibrid identitás bevezetési stratégia meghatározása</span><span class="sxs-lookup"><span data-stu-id="7508f-138">Define a hybrid identity adoption strategy</span></span>](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)

## <a name="see-also"></a><span data-ttu-id="7508f-139">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="7508f-139">See also</span></span>
[<span data-ttu-id="7508f-140">Kialakítási szempontok áttekintése</span><span class="sxs-lookup"><span data-stu-id="7508f-140">Design considerations overview</span></span>](active-directory-hybrid-identity-design-considerations-overview.md)

