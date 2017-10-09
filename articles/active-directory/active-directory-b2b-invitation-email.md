---
title: "hello Azure Active Directory B2B együttműködés meghívó e-mail aaaThe elemeinek |} Microsoft Docs"
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
ms.openlocfilehash: f4908014d71a63442bbdca2182f54c7a79675a82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="hello-elements-of-hello-b2b-collaboration-invitation-email"></a><span data-ttu-id="b916f-103">hello B2B együttműködés meghívó e-mail hello elemei</span><span class="sxs-lookup"><span data-stu-id="b916f-103">hello elements of hello B2B collaboration invitation email</span></span>

<span data-ttu-id="b916f-104">Meghívót e-mailek egy kritikus fontosságú összetevők toobring partnerek a board az Azure AD B2B együttműködés felhasználóként.</span><span class="sxs-lookup"><span data-stu-id="b916f-104">Invitation emails are a critical component toobring partners on board as B2B collaboration users in Azure AD.</span></span> <span data-ttu-id="b916f-105">Használhatja őket tooincrease hello címzett megbízhatósági.</span><span class="sxs-lookup"><span data-stu-id="b916f-105">You can use them tooincrease hello recipient's trust.</span></span> <span data-ttu-id="b916f-106">hozzáadhat érvényességét és közösségi igazoló toohello e-mailek, toomake meg arról, hogy hello címzett érzi hello kiválasztása a Feladatkezelő **Ismerkedés** tooaccept hello meghívó gombra.</span><span class="sxs-lookup"><span data-stu-id="b916f-106">you can add legitimacy and social proof toohello email, toomake sure hello recipient feels comfortable with selecting hello **Get Started** button tooaccept hello invitation.</span></span> <span data-ttu-id="b916f-107">A bizalmi kapcsolat a kulcs azt jelenti, hogy a megosztás súrlódás tooreduce.</span><span class="sxs-lookup"><span data-stu-id="b916f-107">This trust is a key means tooreduce sharing friction.</span></span> <span data-ttu-id="b916f-108">És szeretné toomake hello e-mail megjelenését nagyszerű!</span><span class="sxs-lookup"><span data-stu-id="b916f-108">And you also want toomake hello email look great!</span></span>

![Az Azure AD B2b meghívó e-mail](media/active-directory-b2b-invitation-email/invitation-email.png)

## <a name="explaining-hello-email"></a><span data-ttu-id="b916f-110">Ismertető az üdvözlő e-mail</span><span class="sxs-lookup"><span data-stu-id="b916f-110">Explaining hello email</span></span>
<span data-ttu-id="b916f-111">Vizsgáljuk meg néhány elemet az üdvözlő e-mailek, hogy a legjobb módja toouse képességeit.</span><span class="sxs-lookup"><span data-stu-id="b916f-111">Let's look at a few elements of hello email so you know how best toouse their capabilities.</span></span>

### <a name="subject"></a><span data-ttu-id="b916f-112">Tárgy</span><span class="sxs-lookup"><span data-stu-id="b916f-112">Subject</span></span>
<span data-ttu-id="b916f-113">hello hello e-mail tárgya a következő mintát a következő hello: meghívjuk toohello &lt;tenantname&gt; szervezet</span><span class="sxs-lookup"><span data-stu-id="b916f-113">hello subject of hello email follows hello following pattern: You're invited toohello &lt;tenantname&gt; organization</span></span>

### <a name="from-address"></a><span data-ttu-id="b916f-114">Feladó címe</span><span class="sxs-lookup"><span data-stu-id="b916f-114">From address</span></span>
<span data-ttu-id="b916f-115">A LinkedIn-szerű minta a feladó címe hello használjuk.</span><span class="sxs-lookup"><span data-stu-id="b916f-115">We use a LinkedIn-like pattern for hello From address.</span></span>  <span data-ttu-id="b916f-116">Meg kell törölje ki hello meghívó, és amely vállalati, és elmagyarázza is, hogy az üdvözlő e-mail érkezik egy Microsoft e-mail címet.</span><span class="sxs-lookup"><span data-stu-id="b916f-116">You should be clear who hello inviter is and from which company, and also clarify that hello email is coming from a Microsoft email address.</span></span> <span data-ttu-id="b916f-117">hello formátuma: &lt;megjelenített nevével meghívó&gt; a &lt;tenantname&gt; (Microsoft) keresztül <invites@microsoft.com&gt;</span><span class="sxs-lookup"><span data-stu-id="b916f-117">hello format is: &lt;Display name of inviter&gt; from &lt;tenantname&gt; (via Microsoft) <invites@microsoft.com&gt;</span></span>

### <a name="reply-to"></a><span data-ttu-id="b916f-118">Válasz</span><span class="sxs-lookup"><span data-stu-id="b916f-118">Reply To</span></span>
<span data-ttu-id="b916f-119">hello válasz-tooemail toohello meghívó e-mail érhető el, ha van beállítva, hogy a toohello e-mailt küld egy e-mailek hátsó toohello meghívó a műveletre.</span><span class="sxs-lookup"><span data-stu-id="b916f-119">hello reply-tooemail is set toohello inviter's email when available, so that replying toohello email sends an email back toohello inviter.</span></span>

### <a name="branding"></a><span data-ttu-id="b916f-120">Védjegyek</span><span class="sxs-lookup"><span data-stu-id="b916f-120">Branding</span></span>
<span data-ttu-id="b916f-121">a bérlő az e-mailek használata hello vállalati arculat megjelenítése, amely hello meghívó előfordulhat, hogy állította be a bérlő számára.</span><span class="sxs-lookup"><span data-stu-id="b916f-121">hello invitation emails from your tenant use hello company branding that you may have set up for your tenant.</span></span> <span data-ttu-id="b916f-122">Ha azt szeretné, hogy ez a funkció előnyeit tootake [Itt](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) hello megtudhatja, hogyan vannak tooconfigure azt.</span><span class="sxs-lookup"><span data-stu-id="b916f-122">If you want tootake advantage of this capability, [here](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) are hello details on how tooconfigure it.</span></span> <span data-ttu-id="b916f-123">hello szalagcím emblémájának hello e-mail jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b916f-123">hello banner logo appears in hello email.</span></span> <span data-ttu-id="b916f-124">Hajtsa végre a hello a kép mérete és minőségi utasítások [Itt](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) a legjobb eredmények elérése érdekében.</span><span class="sxs-lookup"><span data-stu-id="b916f-124">Follow hello image size and quality instructions [here](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) for best results.</span></span> <span data-ttu-id="b916f-125">Ezenkívül hello vállalatnév is megjelennek hello hívás tooaction.</span><span class="sxs-lookup"><span data-stu-id="b916f-125">In addition, hello company name also shows up in hello call tooaction.</span></span>

### <a name="call-tooaction"></a><span data-ttu-id="b916f-126">Hívás tooaction</span><span class="sxs-lookup"><span data-stu-id="b916f-126">Call tooaction</span></span>
<span data-ttu-id="b916f-127">hello hívás tooaction két részből áll: Miért hello címzett kapott üdvözlő levelek, és milyen hello címzett alatt kapcsolatba toodo kapcsolatos foglalja össze.</span><span class="sxs-lookup"><span data-stu-id="b916f-127">hello call tooaction consists of two parts: explaining why hello recipient has received hello mail and what hello recipient is being asked toodo about it.</span></span>
- <span data-ttu-id="b916f-128">hello a "Miért" szakasz is kezelhetők, a következő mintát hello használata: a meghívott tooaccess alkalmazások hello már &lt;tenantname&gt; szervezet</span><span class="sxs-lookup"><span data-stu-id="b916f-128">hello "why" section can be addressed using hello following pattern: You've been invited tooaccess applications in hello &lt;tenantname&gt; organization</span></span>

- <span data-ttu-id="b916f-129">És "Mi alatt megkérdezi toodo" szakasz hello hello jelenlétét jelzi hello **Ismerkedés** gombra.</span><span class="sxs-lookup"><span data-stu-id="b916f-129">And hello "what you're being asked toodo" section is indicated by hello presence of hello **Get Started** button.</span></span> <span data-ttu-id="b916f-130">Amikor hello címzett nélkül hello meghívókat hozzá lett adva, erre a gombra kattintva nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b916f-130">When hello recipient has been added without hello need for invitations, this button doesn't show up.</span></span>

### <a name="inviters-information"></a><span data-ttu-id="b916f-131">A meghívó a információk</span><span class="sxs-lookup"><span data-stu-id="b916f-131">Inviter's information</span></span>
<span data-ttu-id="b916f-132">hello meghívó megjelenített név hello e-mail tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b916f-132">hello inviter's display name is included in hello email.</span></span> <span data-ttu-id="b916f-133">És Ezenkívül ha beállította az Azure AD-fiókot a profilkép, e-mailek meghívása hello fogja tartalmazni, valamint, hogy a kép.</span><span class="sxs-lookup"><span data-stu-id="b916f-133">And in addition, if you've set up a profile picture for your Azure AD account, hello inviting email will include that picture as well.</span></span> <span data-ttu-id="b916f-134">Mindkét tervezett tooincrease hello e-mailben abban, hogy a címzett vannak.</span><span class="sxs-lookup"><span data-stu-id="b916f-134">Both are intended tooincrease your recipient's confidence in hello email.</span></span>

<span data-ttu-id="b916f-135">Ha még nem állított profilkép, hello meghívó monogramja hello kép helyett egy ikon jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="b916f-135">If you haven't yet set up your profile picture, an icon with hello inviter's initials in place of hello picture is shown:</span></span>

  ![Hello meghívó monogramja megjelenítése](media/active-directory-b2b-invitation-email/inviters-initials.png)

### <a name="body"></a><span data-ttu-id="b916f-137">Törzs</span><span class="sxs-lookup"><span data-stu-id="b916f-137">Body</span></span>
<span data-ttu-id="b916f-138">hello törzsében üdvözlőüzenetére adott hello meghívó composes vagy hello meghívó API keresztül.</span><span class="sxs-lookup"><span data-stu-id="b916f-138">hello body contains hello message that hello inviter composes or is passed through hello invitation API.</span></span> <span data-ttu-id="b916f-139">Így azt nem dolgozza fel az HTML-címkék biztonsági okokból egy területre.</span><span class="sxs-lookup"><span data-stu-id="b916f-139">It is a text area, so it does not process HTML tags for security reasons.</span></span>

### <a name="footer-section"></a><span data-ttu-id="b916f-140">Élőláb szakasz</span><span class="sxs-lookup"><span data-stu-id="b916f-140">Footer section</span></span>
<span data-ttu-id="b916f-141">hello élőláb hello Microsoft vállalatának arculatát tartalmaz, és lehetővé teszi, hogy a hello címzettje, ha a nem figyelt alias hello e-mail elküldése.</span><span class="sxs-lookup"><span data-stu-id="b916f-141">hello footer contains hello Microsoft company brand and lets hello recipient know if hello email was sent from an unmonitored alias.</span></span> <span data-ttu-id="b916f-142">Bizonyos esetekben:</span><span class="sxs-lookup"><span data-stu-id="b916f-142">Special cases:</span></span>

- <span data-ttu-id="b916f-143">hello meghívó e-mail cím nem rendelkezik hello bérleti meghívása</span><span class="sxs-lookup"><span data-stu-id="b916f-143">hello inviter doesn't have an email address in hello inviting tenancy</span></span>

  ![Meghívó képe nem rendelkezik egy e-mail címet hello bérleti meghívása](media/active-directory-b2b-invitation-email/inviter-no-email.png)


- <span data-ttu-id="b916f-145">hello címzett tooredeem hello meghívó nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="b916f-145">hello recipient doesn't need tooredeem hello invitation</span></span>

  ![Ha a címzett nem szükséges tooredeem meghívó](media/active-directory-b2b-invitation-email/when-recipient-doesnt-redeem.png)


## <a name="next-steps"></a><span data-ttu-id="b916f-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b916f-147">Next steps</span></span>

<span data-ttu-id="b916f-148">Ismerje meg az Azure AD B2B együttműködés további cikkeit:</span><span class="sxs-lookup"><span data-stu-id="b916f-148">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="b916f-149">Mi az az Azure AD B2B együttműködés</span><span class="sxs-lookup"><span data-stu-id="b916f-149">What is Azure AD B2B collaboration</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="b916f-150">Hogyan rendszergazdák Azure Active Directory B2B együttműködés felhasználók hozzá?</span><span class="sxs-lookup"><span data-stu-id="b916f-150">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="b916f-151">Hogyan hozzá az információkkal dolgozó szakemberek B2B együttműködés felhasználók?</span><span class="sxs-lookup"><span data-stu-id="b916f-151">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="b916f-152">B2B együttműködés meghívó érvényesítési</span><span class="sxs-lookup"><span data-stu-id="b916f-152">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="b916f-153">Az Azure AD B2B együttműködés licencelés</span><span class="sxs-lookup"><span data-stu-id="b916f-153">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="b916f-154">Hibaelhárítás az Azure Active Directory B2B együttműködés</span><span class="sxs-lookup"><span data-stu-id="b916f-154">Troubleshooting Azure Active Directory B2B collaboration</span></span>](active-directory-b2b-troubleshooting.md)
* [<span data-ttu-id="b916f-155">Az Azure Active Directory B2B együttműködés gyakori kérdések (GYIK)</span><span class="sxs-lookup"><span data-stu-id="b916f-155">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="b916f-156">Az Azure Active Directory B2B együttműködés API és a Testreszabás</span><span class="sxs-lookup"><span data-stu-id="b916f-156">Azure Active Directory B2B collaboration API and customization</span></span>](active-directory-b2b-api.md)
* [<span data-ttu-id="b916f-157">Többtényezős hitelesítés a B2B-együttműködés felhasználói számára</span><span class="sxs-lookup"><span data-stu-id="b916f-157">Multi-factor authentication for B2B collaboration users</span></span>](active-directory-b2b-mfa-instructions.md)
* [<span data-ttu-id="b916f-158">Adja hozzá a B2B együttműködés felhasználók nélkül</span><span class="sxs-lookup"><span data-stu-id="b916f-158">Add B2B collaboration users without an invitation</span></span>](active-directory-b2b-add-user-without-invite.md)
* [<span data-ttu-id="b916f-159">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="b916f-159">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
