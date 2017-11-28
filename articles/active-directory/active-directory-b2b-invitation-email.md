---
title: "Az elemek az Azure Active Directory B2B együttműködés meghívó e-mail |} Microsoft Docs"
description: "Az Azure Active Directory B2B együttműködés meghívó e-mail sablon"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/23/2017
ms.author: sasubram
ms.openlocfilehash: 458a2cab13b7e83f120e0926a95d454070181dfb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="the-elements-of-the-b2b-collaboration-invitation-email"></a><span data-ttu-id="62d83-103">A B2B együttműködés meghívó e-mail elemei</span><span class="sxs-lookup"><span data-stu-id="62d83-103">The elements of the B2B collaboration invitation email</span></span>

<span data-ttu-id="62d83-104">Meghívót e-mailek a board partnerek átveendő az Azure AD B2B együttműködés felhasználóként kritikus összetevője.</span><span class="sxs-lookup"><span data-stu-id="62d83-104">Invitation emails are a critical component to bring partners on board as B2B collaboration users in Azure AD.</span></span> <span data-ttu-id="62d83-105">A címzett megbízhatósági növeléséhez használhatja őket.</span><span class="sxs-lookup"><span data-stu-id="62d83-105">You can use them to increase the recipient's trust.</span></span> <span data-ttu-id="62d83-106">hozzáadhat érvényességét, és az e-mailt, győződjön meg arról, hogy a címzett közösségi igazolás érzi, válassza a Feladatkezelő a **Ismerkedés** gombra kattintva fogadja el a meghívást.</span><span class="sxs-lookup"><span data-stu-id="62d83-106">you can add legitimacy and social proof to the email, to make sure the recipient feels comfortable with selecting the **Get Started** button to accept the invitation.</span></span> <span data-ttu-id="62d83-107">Ebben a megbízhatósági kapcsolatban a kulcs azt jelenti, hogy megosztási súrlódás csökkentése érdekében.</span><span class="sxs-lookup"><span data-stu-id="62d83-107">This trust is a key means to reduce sharing friction.</span></span> <span data-ttu-id="62d83-108">És is szeretne az e-mailt remekül!</span><span class="sxs-lookup"><span data-stu-id="62d83-108">And you also want to make the email look great!</span></span>

![Az Azure AD B2b meghívó e-mail](media/active-directory-b2b-invitation-email/invitation-email.png)

## <a name="explaining-the-email"></a><span data-ttu-id="62d83-110">Ismertető az e-mailben</span><span class="sxs-lookup"><span data-stu-id="62d83-110">Explaining the email</span></span>
<span data-ttu-id="62d83-111">Vizsgáljuk meg az e-mailt néhány elemeinek így megtudhatja, hogyan lehet a legjobban azok képességeinek használatához.</span><span class="sxs-lookup"><span data-stu-id="62d83-111">Let's look at a few elements of the email so you know how best to use their capabilities.</span></span>

### <a name="subject"></a><span data-ttu-id="62d83-112">Tárgy</span><span class="sxs-lookup"><span data-stu-id="62d83-112">Subject</span></span>
<span data-ttu-id="62d83-113">Az e-mail tárgyát a következő mintát követi: meghívjuk az &lt;tenantname&gt; szervezet</span><span class="sxs-lookup"><span data-stu-id="62d83-113">The subject of the email follows the following pattern: You're invited to the &lt;tenantname&gt; organization</span></span>

### <a name="from-address"></a><span data-ttu-id="62d83-114">Feladó címe</span><span class="sxs-lookup"><span data-stu-id="62d83-114">From address</span></span>
<span data-ttu-id="62d83-115">A LinkedIn-szerű minta a feladó címe az használjuk.</span><span class="sxs-lookup"><span data-stu-id="62d83-115">We use a LinkedIn-like pattern for the From address.</span></span>  <span data-ttu-id="62d83-116">Legyen törölje a jelet a meghívó, aki és amely a vállalati és is elmagyarázza, hogy az e-mailt származik-e a Microsoft e-mail cím.</span><span class="sxs-lookup"><span data-stu-id="62d83-116">You should be clear who the inviter is and from which company, and also clarify that the email is coming from a Microsoft email address.</span></span> <span data-ttu-id="62d83-117">A formátum: &lt;megjelenített nevével meghívó&gt; a &lt;tenantname&gt; (Microsoft) keresztül <invites@microsoft.com&gt;</span><span class="sxs-lookup"><span data-stu-id="62d83-117">The format is: &lt;Display name of inviter&gt; from &lt;tenantname&gt; (via Microsoft) <invites@microsoft.com&gt;</span></span>

### <a name="reply-to"></a><span data-ttu-id="62d83-118">Válasz</span><span class="sxs-lookup"><span data-stu-id="62d83-118">Reply To</span></span>
<span data-ttu-id="62d83-119">A válaszcím e-mailben a meghívó e-mail érhető el, ha van beállítva, hogy a megválaszolása az e-mailt küld egy e-mailt vissza a meghívó.</span><span class="sxs-lookup"><span data-stu-id="62d83-119">The reply-to email is set to the inviter's email when available, so that replying to the email sends an email back to the inviter.</span></span>

### <a name="branding"></a><span data-ttu-id="62d83-120">Védjegyek</span><span class="sxs-lookup"><span data-stu-id="62d83-120">Branding</span></span>
<span data-ttu-id="62d83-121">A meghívó e-maileket a bérlő használatát a vállalati arculat megjelenítése, amikor előfordulhat, hogy állította be a bérlő számára.</span><span class="sxs-lookup"><span data-stu-id="62d83-121">The invitation emails from your tenant use the company branding that you may have set up for your tenant.</span></span> <span data-ttu-id="62d83-122">Ha azt szeretné, hogy ez a funkció előnyeit [Itt](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) a konfigurálásának részletes adatai.</span><span class="sxs-lookup"><span data-stu-id="62d83-122">If you want to take advantage of this capability, [here](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) are the details on how to configure it.</span></span> <span data-ttu-id="62d83-123">A szalagcím emblémájának jelenik meg az e-mailben.</span><span class="sxs-lookup"><span data-stu-id="62d83-123">The banner logo appears in the email.</span></span> <span data-ttu-id="62d83-124">Hajtsa végre a lemezkép mérete és minőségi utasítások [Itt](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) a legjobb eredmények elérése érdekében.</span><span class="sxs-lookup"><span data-stu-id="62d83-124">Follow the image size and quality instructions [here](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) for best results.</span></span> <span data-ttu-id="62d83-125">Emellett a vállalat nevét is megjelennek a művelet hívása.</span><span class="sxs-lookup"><span data-stu-id="62d83-125">In addition, the company name also shows up in the call to action.</span></span>

### <a name="call-to-action"></a><span data-ttu-id="62d83-126">A művelet hívása</span><span class="sxs-lookup"><span data-stu-id="62d83-126">Call to action</span></span>
<span data-ttu-id="62d83-127">A művelet hívása két részből áll: a címzett miért kapott a levelek, és mi a címzett alatt kapcsolatba arról foglalja össze.</span><span class="sxs-lookup"><span data-stu-id="62d83-127">The call to action consists of two parts: explaining why the recipient has received the mail and what the recipient is being asked to do about it.</span></span>
- <span data-ttu-id="62d83-128">A "Miért" szakaszt a következő minta használatával lehet megoldani:, hogy meghívott hozzáférés az alkalmazásokhoz a &lt;tenantname&gt; szervezet</span><span class="sxs-lookup"><span data-stu-id="62d83-128">The "why" section can be addressed using the following pattern: You've been invited to access applications in the &lt;tenantname&gt; organization</span></span>

- <span data-ttu-id="62d83-129">És a "Mi alatt megkérdezi ehhez" szakasz jelenlétét jelzi a **Ismerkedés** gombra.</span><span class="sxs-lookup"><span data-stu-id="62d83-129">And the "what you're being asked to do" section is indicated by the presence of the **Get Started** button.</span></span> <span data-ttu-id="62d83-130">Ha a címzett meghívókat szükségessége nélkül hozzá lett adva, erre a gombra kattintva nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="62d83-130">When the recipient has been added without the need for invitations, this button doesn't show up.</span></span>

### <a name="inviters-information"></a><span data-ttu-id="62d83-131">A meghívó a információk</span><span class="sxs-lookup"><span data-stu-id="62d83-131">Inviter's information</span></span>
<span data-ttu-id="62d83-132">A meghívó megjelenített név szerepel az e-mailt.</span><span class="sxs-lookup"><span data-stu-id="62d83-132">The inviter's display name is included in the email.</span></span> <span data-ttu-id="62d83-133">És emellett, ha beállította az Azure AD-fiókot a profilkép, hívja fel az e-mailt fogja tartalmazni, valamint, hogy a kép.</span><span class="sxs-lookup"><span data-stu-id="62d83-133">And in addition, if you've set up a profile picture for your Azure AD account, the inviting email will include that picture as well.</span></span> <span data-ttu-id="62d83-134">Mindkét célja, hogy növelje a címzett abban, hogy az e-mailben.</span><span class="sxs-lookup"><span data-stu-id="62d83-134">Both are intended to increase your recipient's confidence in the email.</span></span>

<span data-ttu-id="62d83-135">Ha még nem állított profilkép, a meghívó monogramja a kép helyett egy ikon jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="62d83-135">If you haven't yet set up your profile picture, an icon with the inviter's initials in place of the picture is shown:</span></span>

  ![a meghívó monogramja megjelenítése](media/active-directory-b2b-invitation-email/inviters-initials.png)

### <a name="body"></a><span data-ttu-id="62d83-137">Törzs</span><span class="sxs-lookup"><span data-stu-id="62d83-137">Body</span></span>
<span data-ttu-id="62d83-138">Az üzenet, amely a meghívó composes, vagy a meghívó API keresztül törzsében.</span><span class="sxs-lookup"><span data-stu-id="62d83-138">The body contains the message that the inviter composes or is passed through the invitation API.</span></span> <span data-ttu-id="62d83-139">Így azt nem dolgozza fel az HTML-címkék biztonsági okokból egy területre.</span><span class="sxs-lookup"><span data-stu-id="62d83-139">It is a text area, so it does not process HTML tags for security reasons.</span></span>

### <a name="footer-section"></a><span data-ttu-id="62d83-140">Élőláb szakasz</span><span class="sxs-lookup"><span data-stu-id="62d83-140">Footer section</span></span>
<span data-ttu-id="62d83-141">A lábléc tartalmazza a Microsoft vállalatának arculatát, és lehetővé teszi, hogy a címzett tudja, ha az e-mailben küldött egy nem figyelt aliast.</span><span class="sxs-lookup"><span data-stu-id="62d83-141">The footer contains the Microsoft company brand and lets the recipient know if the email was sent from an unmonitored alias.</span></span> <span data-ttu-id="62d83-142">Bizonyos esetekben:</span><span class="sxs-lookup"><span data-stu-id="62d83-142">Special cases:</span></span>

- <span data-ttu-id="62d83-143">A meghívó nem rendelkezik e-mail címmel hívja fel a bérlet</span><span class="sxs-lookup"><span data-stu-id="62d83-143">The inviter doesn't have an email address in the inviting tenancy</span></span>

  ![Meghívó képe nem rendelkezik e-mail címmel hívja fel a bérlet](media/active-directory-b2b-invitation-email/inviter-no-email.png)


- <span data-ttu-id="62d83-145">A címzett nem szükséges a meghívó beváltása</span><span class="sxs-lookup"><span data-stu-id="62d83-145">The recipient doesn't need to redeem the invitation</span></span>

  ![Ha a címzett meghívó beváltani nem szükséges](media/active-directory-b2b-invitation-email/when-recipient-doesnt-redeem.png)


## <a name="next-steps"></a><span data-ttu-id="62d83-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="62d83-147">Next steps</span></span>

<span data-ttu-id="62d83-148">Ismerje meg az Azure AD B2B együttműködés további cikkeit:</span><span class="sxs-lookup"><span data-stu-id="62d83-148">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="62d83-149">Mi az az Azure AD B2B együttműködés</span><span class="sxs-lookup"><span data-stu-id="62d83-149">What is Azure AD B2B collaboration</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="62d83-150">Hogyan rendszergazdák Azure Active Directory B2B együttműködés felhasználók hozzá?</span><span class="sxs-lookup"><span data-stu-id="62d83-150">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="62d83-151">Hogyan hozzá az információkkal dolgozó szakemberek B2B együttműködés felhasználók?</span><span class="sxs-lookup"><span data-stu-id="62d83-151">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="62d83-152">B2B együttműködés meghívó érvényesítési</span><span class="sxs-lookup"><span data-stu-id="62d83-152">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="62d83-153">Az Azure AD B2B együttműködés licencelés</span><span class="sxs-lookup"><span data-stu-id="62d83-153">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="62d83-154">Hibaelhárítás az Azure Active Directory B2B együttműködés</span><span class="sxs-lookup"><span data-stu-id="62d83-154">Troubleshooting Azure Active Directory B2B collaboration</span></span>](active-directory-b2b-troubleshooting.md)
* [<span data-ttu-id="62d83-155">Az Azure Active Directory B2B együttműködés gyakori kérdések (GYIK)</span><span class="sxs-lookup"><span data-stu-id="62d83-155">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="62d83-156">Az Azure Active Directory B2B együttműködés API és a Testreszabás</span><span class="sxs-lookup"><span data-stu-id="62d83-156">Azure Active Directory B2B collaboration API and customization</span></span>](active-directory-b2b-api.md)
* [<span data-ttu-id="62d83-157">Többtényezős hitelesítés a B2B-együttműködés felhasználói számára</span><span class="sxs-lookup"><span data-stu-id="62d83-157">Multi-factor authentication for B2B collaboration users</span></span>](active-directory-b2b-mfa-instructions.md)
* [<span data-ttu-id="62d83-158">Adja hozzá a B2B együttműködés felhasználók nélkül</span><span class="sxs-lookup"><span data-stu-id="62d83-158">Add B2B collaboration users without an invitation</span></span>](active-directory-b2b-add-user-without-invite.md)
* [<span data-ttu-id="62d83-159">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="62d83-159">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
