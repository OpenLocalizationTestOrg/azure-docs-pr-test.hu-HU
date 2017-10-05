---
title: "Az Azure Active Directory hibrid identitáskezelési elrendezésével kapcsolatos szempontok - többtényezős hitelesítési követelmények meghatározása"
description: "Feltételes hozzáférés-vezérlést az Azure Active Directory ellenőrzi a megadott feltételek, ha a felhasználó hitelesítése és az alkalmazáshoz való hozzáférés előtt válasszon. Ha ezek a feltételek teljesülnek, a felhasználó hitelesítése és hozzáférni az alkalmazáshoz engedélyezett."
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
ms.openlocfilehash: 5b3a8ce6e4203dfb3700f324e32687dd910118af
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="determine-multi-factor-authentication-requirements-for-your-hybrid-identity-solution"></a><span data-ttu-id="a3113-104">A hibrid identitáskezelési megoldás a többtényezős hitelesítési követelmények meghatározása</span><span class="sxs-lookup"><span data-stu-id="a3113-104">Determine multi-factor authentication requirements for your hybrid identity solution</span></span>
<span data-ttu-id="a3113-105">A világ mobilitási, a felhasználói adatokat és alkalmazásokat a felhőben, és egy eszközről ezek az információk védelme vált kiemelkedő.</span><span class="sxs-lookup"><span data-stu-id="a3113-105">In this world of mobility, with users accessing data and applications in the cloud and from any device, securing this information has become paramount.</span></span>  <span data-ttu-id="a3113-106">Minden nap van egy új főcím kapcsolatos biztonsági problémák.</span><span class="sxs-lookup"><span data-stu-id="a3113-106">Every day there is a new headline about a security breach.</span></span>  <span data-ttu-id="a3113-107">Bár, nem garantálja az ilyen problémák elleni, a többtényezős hitelesítést, ezek a problémák megelőzése érdekében biztonsági további réteget biztosít.</span><span class="sxs-lookup"><span data-stu-id="a3113-107">Although, there is no guarantee against such breaches, multi-factor authentication, provides an additional layer of security to help prevent these breaches.</span></span>
<span data-ttu-id="a3113-108">Indítsa el a multi-factor authentication a szervezetek szükséges követelmények értékelésekor.</span><span class="sxs-lookup"><span data-stu-id="a3113-108">Start by evaluating the organizations requirements for multi-factor authentication.</span></span> <span data-ttu-id="a3113-109">Ez azt jelenti, hogy mi a szervezet próbál biztonságos.</span><span class="sxs-lookup"><span data-stu-id="a3113-109">That is, what is the organization trying to secure.</span></span>  <span data-ttu-id="a3113-110">Ez a kiértékelés fontos, hogy a és a szervezetek felhasználók a multi-factor authentication lehetővé teszi a műszaki követelményeinek meghatározása.</span><span class="sxs-lookup"><span data-stu-id="a3113-110">This evaluation is important to define the technical requirements for setting up and enabling the organizations users for multi-factor authentication.</span></span>

> [!NOTE]
> <span data-ttu-id="a3113-111">Ha nem ismeri a többtényezős hitelesítés és a hatása, erősen ajánlott, hogy olvassa el a cikk [Mi az Azure multi-factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md) előzetes folytatja a fejezet elolvasása.</span><span class="sxs-lookup"><span data-stu-id="a3113-111">If you are not familiar with MFA and what it does, it is strongly recommended that you read the article [What is Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md) prior to continue reading this section.</span></span>
> 
> 

<span data-ttu-id="a3113-112">Győződjön meg arról, hogy válaszoljon a következő:</span><span class="sxs-lookup"><span data-stu-id="a3113-112">Make sure to answer the following:</span></span>

* <span data-ttu-id="a3113-113">A vállalat próbál Microsoft-alkalmazások védelmét?</span><span class="sxs-lookup"><span data-stu-id="a3113-113">Is your company trying to secure Microsoft apps?</span></span> 
* <span data-ttu-id="a3113-114">Hogyan közzétett ezeket az alkalmazásokat?</span><span class="sxs-lookup"><span data-stu-id="a3113-114">How these apps are published?</span></span>
* <span data-ttu-id="a3113-115">A vállalat biztosítja a távoli hozzáférés úgy, hogy az alkalmazottak a helyszíni alkalmazások elérésére?</span><span class="sxs-lookup"><span data-stu-id="a3113-115">Does your company provide remote access to allow employees to access on-premises apps?</span></span>

<span data-ttu-id="a3113-116">Ha igen, milyen típusú távelérési? Is fel kell mérnie, amelyben a felhasználók ezeket az alkalmazásokat elérő található.</span><span class="sxs-lookup"><span data-stu-id="a3113-116">If yes, what type of remote access?You also need to evaluate where the users who are accessing these applications will be located.</span></span> <span data-ttu-id="a3113-117">Ez a kiértékelés egy másik fontos eleme a megfelelő a multi-factor authentication-stratégia meghatározása.</span><span class="sxs-lookup"><span data-stu-id="a3113-117">This evaluation is another important step to define the proper multi-factor authentication strategy.</span></span> <span data-ttu-id="a3113-118">Győződjön meg arról, hogy válaszoljon a következő kérdésekre:</span><span class="sxs-lookup"><span data-stu-id="a3113-118">Make sure to answer the following questions:</span></span>

* <span data-ttu-id="a3113-119">Ha a felhasználók fog található?</span><span class="sxs-lookup"><span data-stu-id="a3113-119">Where are the users going to be located?</span></span>
* <span data-ttu-id="a3113-120">Ezek lehetnek bárhol?</span><span class="sxs-lookup"><span data-stu-id="a3113-120">Can they be located anywhere?</span></span>
* <span data-ttu-id="a3113-121">Nem a vállalat kíván létesíteni korlátozások a felhasználó földrajzi helye alapján?</span><span class="sxs-lookup"><span data-stu-id="a3113-121">Does your company want to establish restrictions according to the user’s location?</span></span>

<span data-ttu-id="a3113-122">Ezek a követelmények elsajátítása után fontos is a többtényezős hitelesítést a felhasználói követelmények kiértékeléséhez.</span><span class="sxs-lookup"><span data-stu-id="a3113-122">Once you understand these requirements, it is important to also evaluate the user’s requirements for multi-factor authentication.</span></span> <span data-ttu-id="a3113-123">Ez a kiértékelés fontos, mert azt határozza meg a multi-factor Authentication hitelesítés terítésével követelményei.</span><span class="sxs-lookup"><span data-stu-id="a3113-123">This evaluation is important because it will define the requirements for rolling out multi-factor authentication.</span></span> <span data-ttu-id="a3113-124">Győződjön meg arról, hogy válaszoljon a következő kérdésekre:</span><span class="sxs-lookup"><span data-stu-id="a3113-124">Make sure to answer the following questions:</span></span>

* <span data-ttu-id="a3113-125">A felhasználók a multi-factor authentication tisztában van?</span><span class="sxs-lookup"><span data-stu-id="a3113-125">Are the users familiar with multi-factor authentication?</span></span>
* <span data-ttu-id="a3113-126">Néhány felhasználását lesz szükség a további hitelesítés?</span><span class="sxs-lookup"><span data-stu-id="a3113-126">Will some uses be required to provide additional authentication?</span></span>  
  * <span data-ttu-id="a3113-127">Ha igen, minden esetben, ha külső hálózatokat vagy férnek hozzá bizonyos alkalmazásokat, vagy más feltételek mellett érkező?</span><span class="sxs-lookup"><span data-stu-id="a3113-127">If yes, all the time, when coming from external networks, or accessing specific applications, or under other conditions?</span></span>
* <span data-ttu-id="a3113-128">A felhasználóinak kell képzési beállítása és valósítja meg a multi-factor authentication?</span><span class="sxs-lookup"><span data-stu-id="a3113-128">Will the users require training on how to setup and implement multi-factor authentication?</span></span>
* <span data-ttu-id="a3113-129">Mik a legfontosabb forgatókönyvek, amely a vállalat lehetővé szeretné tenni a felhasználók a többtényezős hitelesítést?</span><span class="sxs-lookup"><span data-stu-id="a3113-129">What are the key scenarios that your company wants to enable multi-factor authentication for their users?</span></span>

<span data-ttu-id="a3113-130">Miután a fenti kérdések megválaszolása, fogja érti, ha nincsenek a helyszíni már megvalósította a multi-factor authentication.</span><span class="sxs-lookup"><span data-stu-id="a3113-130">After answering the previous questions, you will be able to understand if there are multi-factor authentication already implemented on-premises.</span></span> <span data-ttu-id="a3113-131">Ez a kiértékelés fontos, hogy a és a szervezetek felhasználók a multi-factor authentication lehetővé teszi a műszaki követelményeinek meghatározása.</span><span class="sxs-lookup"><span data-stu-id="a3113-131">This evaluation is important to define the technical requirements for setting up and enabling the organizations users for multi-factor authentication.</span></span> <span data-ttu-id="a3113-132">Győződjön meg arról, hogy válaszoljon a következő kérdésekre:</span><span class="sxs-lookup"><span data-stu-id="a3113-132">Make sure to answer the following questions:</span></span>

* <span data-ttu-id="a3113-133">Vállalatának meg kell a multi-factor Authentication szolgáltatás a kiemelt jogosultságú fiókok védelméhez?</span><span class="sxs-lookup"><span data-stu-id="a3113-133">Does your company need to protect privileged accounts with MFA?</span></span>
* <span data-ttu-id="a3113-134">Vállalatának meg kell ahhoz, hogy az egyes alkalmazás megfelelőségi okokból MFA?</span><span class="sxs-lookup"><span data-stu-id="a3113-134">Does your company need to enable MFA for certain application for compliance reasons?</span></span>
* <span data-ttu-id="a3113-135">Vállalatának meg kell engedélyezéséről az ezen alkalmazás-vagy csak a rendszergazdák az összes jogosult felhasználók számára?</span><span class="sxs-lookup"><span data-stu-id="a3113-135">Does your company need to enable MFA for all eligible users of these application or only administrators?</span></span>
* <span data-ttu-id="a3113-136">Szükség van mindig engedélyezve van az MFA- vagy csak amikor a felhasználók bejelentkeznek a vállalati hálózaton kívül?</span><span class="sxs-lookup"><span data-stu-id="a3113-136">Do you need have MFA always enabled or only when the users are logged outside of your corporate network?</span></span>

## <a name="next-steps"></a><span data-ttu-id="a3113-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a3113-137">Next steps</span></span>
[<span data-ttu-id="a3113-138">A hibrid identitás bevezetési stratégia meghatározása</span><span class="sxs-lookup"><span data-stu-id="a3113-138">Define a hybrid identity adoption strategy</span></span>](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)

## <a name="see-also"></a><span data-ttu-id="a3113-139">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="a3113-139">See also</span></span>
[<span data-ttu-id="a3113-140">Kialakítási szempontok áttekintése</span><span class="sxs-lookup"><span data-stu-id="a3113-140">Design considerations overview</span></span>](active-directory-hybrid-identity-design-considerations-overview.md)

