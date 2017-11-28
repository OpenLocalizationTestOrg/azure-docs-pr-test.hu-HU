---
title: "tooa gyűjtemény alkalmazás konfigurált összevont bejelentkezés aaaProblems egyszeri bejelentkezés |} Microsoft Docs"
description: "Útmutató a hello bizonyos hibákat, amikor bejelentkezik egy alkalmazás már konfigurálta az SAML-alapú összevont egyszeri bejelentkezés az Azure ad-vel"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: ba20a4904860cf063967029cad33fb80f16e4956
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="problems-signing-in-tooa-gallery-application-configured-for-federated-single-sign-on"></a><span data-ttu-id="e2c0b-103">Összevont egyszeri bejelentkezés beállítása tooa gyűjtemény alkalmazásban problémák</span><span class="sxs-lookup"><span data-stu-id="e2c0b-103">Problems signing in tooa gallery application configured for federated single sign-on</span></span>

<span data-ttu-id="e2c0b-104">tootroubleshoot a probléma tooverify hello alkalmazás konfigurációja az alábbiak szerint Azure AD van szüksége:</span><span class="sxs-lookup"><span data-stu-id="e2c0b-104">tootroubleshoot your problem, you need tooverify hello application configuration in Azure AD as follow:</span></span>

-   <span data-ttu-id="e2c0b-105">Az Azure AD-gyűjtemény alkalmazást hello összes hello konfigurációs lépéseket követte.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-105">You have followed all hello configuration steps for hello Azure AD gallery application.</span></span>

-   <span data-ttu-id="e2c0b-106">hello azonosító és az aad-ben megadott válasz URL-cím egyezik azok hello alkalmazás a várt értékek</span><span class="sxs-lookup"><span data-stu-id="e2c0b-106">hello Identifier and Reply URL configured in AAD match they expected values in hello application</span></span>

-   <span data-ttu-id="e2c0b-107">Hozzárendelt felhasználók toohello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="e2c0b-107">You have assigned users toohello application</span></span>

## <a name="application-not-found-in-directory"></a><span data-ttu-id="e2c0b-108">Az alkalmazás nem található a könyvtárban</span><span class="sxs-lookup"><span data-stu-id="e2c0b-108">Application not found in directory</span></span>

<span data-ttu-id="e2c0b-109">*Hiba AADSTS70001: "Https://contoso.com" azonosítójú alkalmazás nem található hello directory*.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-109">*Error AADSTS70001: Application with Identifier ‘https://contoso.com’ was not found in hello directory*.</span></span>

<span data-ttu-id="e2c0b-110">**Lehetséges ok**</span><span class="sxs-lookup"><span data-stu-id="e2c0b-110">**Possible cause**</span></span>

<span data-ttu-id="e2c0b-111">hello kibocsátó attribútum küldi hello alkalmazás tooAzure a hello SAML-kérelmet AD nem felel meg az Azure AD hello alkalmazásban konfigurált hello azonosító értéket.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-111">hello Issuer attribute sends from hello application tooAzure AD in hello SAML request doesn’t match hello Identifier value configured in hello application Azure AD.</span></span>

<span data-ttu-id="e2c0b-112">**Megoldás**</span><span class="sxs-lookup"><span data-stu-id="e2c0b-112">**Resolution**</span></span>

<span data-ttu-id="e2c0b-113">Ellenőrizze, hogy azt az egyező hello Azure AD-ben konfigurált azonosító értéket hello SAML-kérelmet található hello kibocsátó attribútum:</span><span class="sxs-lookup"><span data-stu-id="e2c0b-113">Ensure that hello Issuer attribute in hello SAML request it’s matching hello Identifier value configured in Azure AD:</span></span>

1.  <span data-ttu-id="e2c0b-114">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="e2c0b-114">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="e2c0b-115">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-115">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="e2c0b-116">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-116">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="e2c0b-117">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-117">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="e2c0b-118">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-118">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="e2c0b-119">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="e2c0b-119">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="e2c0b-120">Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="e2c0b-120">Select hello application you want tooconfigure single sign-on</span></span>

7.  <span data-ttu-id="e2c0b-121">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-121">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="e2c0b-122">Nyissa meg túl**tartomány és az URL-címek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-122">Go too**Domain and URLs** section.</span></span> <span data-ttu-id="e2c0b-123">Ellenőrizze a szövegmező van megfelelő hello hello azonosító értéket hello hiba jelenik meg a következő hello hello értéket.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-123">Verify that hello value in hello Identifier textbox is matching hello value for hello identifier value displayed in hello error.</span></span>

<span data-ttu-id="e2c0b-124">Után hello azonosító értéket frissítette az Azure ad-ben, és megfelelő hello értéke hello alkalmazás a hello SAML-kérelmet küld, meg kell tudni toosign toohello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-124">After you have updated hello Identifier value in Azure AD and it’s matching hello value sends by hello application in hello SAML request, you should be able toosign in toohello application.</span></span>

## <a name="hello-reply-address-does-not-match-hello-reply-addresses-configured-for-hello-application"></a><span data-ttu-id="e2c0b-125">hello a válaszcím nem egyezik meg a hello válasz címek hello alkalmazáshoz konfigurált.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-125">hello reply address does not match hello reply addresses configured for hello application.</span></span>

<span data-ttu-id="e2c0b-126">*AADSTS50011. hiba: a hello válaszcímet "https://contoso.com" nem felel meg a hello válasz címek hello alkalmazáshoz konfigurált*</span><span class="sxs-lookup"><span data-stu-id="e2c0b-126">*Error AADSTS50011: hello reply address ‘https://contoso.com’ does not match hello reply addresses configured for hello application*</span></span>

<span data-ttu-id="e2c0b-127">**Lehetséges ok**</span><span class="sxs-lookup"><span data-stu-id="e2c0b-127">**Possible cause**</span></span>

<span data-ttu-id="e2c0b-128">hello hello SAML kérelem AssertionConsumerServiceURL értéke nem egyezik a hello válasz URL-CÍMEN értékkel vagy mintával konfigurálva az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-128">hello AssertionConsumerServiceURL value in hello SAML request doesn't match hello Reply URL value or pattern configured in Azure AD.</span></span> <span data-ttu-id="e2c0b-129">hello hello SAML-kérelmet AssertionConsumerServiceURL értéket az hello hiba látható hello URL-cím.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-129">hello AssertionConsumerServiceURL value in hello SAML request is hello URL you see in hello error.</span></span>

<span data-ttu-id="e2c0b-130">**Megoldás**</span><span class="sxs-lookup"><span data-stu-id="e2c0b-130">**Resolution**</span></span>

<span data-ttu-id="e2c0b-131">Győződjön meg arról, hogy hello AssertionConsumerServiceURL értéket hello SAML-kérelmet az egyező hello válasz URL-címével konfigurálva az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-131">Ensure that hello AssertionConsumerServiceURL value in hello SAML request it's matching hello Reply URL value configured in Azure AD.</span></span>

1.  <span data-ttu-id="e2c0b-132">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="e2c0b-132">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="e2c0b-133">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-133">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="e2c0b-134">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-134">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="e2c0b-135">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-135">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="e2c0b-136">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-136">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="e2c0b-137">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="e2c0b-137">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="e2c0b-138">Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="e2c0b-138">Select hello application you want tooconfigure single sign-on</span></span>

7.  <span data-ttu-id="e2c0b-139">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-139">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="e2c0b-140">Nyissa meg túl**tartomány és az URL-címek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-140">Go too**Domain and URLs** section.</span></span> <span data-ttu-id="e2c0b-141">Győződjön meg arról, vagy frissítse hello válasz URL-CÍMEN szövegmező toomatch hello AssertionConsumerServiceURL érték az SAML-kérelmet hello hello értéket.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-141">Verify or update hello value in hello Reply URL textbox toomatch hello AssertionConsumerServiceURL value in hello SAML request.</span></span>  
    * <span data-ttu-id="e2c0b-142">Ha nem lát hello válasz URL-cím beviteli mező, válassza ki a hello **megjelenítése speciális URL-beállításainak** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-142">If you don't see hello Reply URL textbox, select hello **Show advanced URL settings** checkbox.</span></span>

<span data-ttu-id="e2c0b-143">Után az Azure ad-ben frissítette hello válasz URL-címével, és ennek megfelelő hello érték küld hello alkalmazás hello a SAML-kérelmet, toohello alkalmazás képes toosign kell lennie.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-143">After you have updated hello Reply URL value in Azure AD and it’s matching hello value sends by hello application in hello SAML request, you should be able toosign in toohello application.</span></span>

## <a name="user-not-assigned-a-role"></a><span data-ttu-id="e2c0b-144">Nem hozzárendelt felhasználó</span><span class="sxs-lookup"><span data-stu-id="e2c0b-144">User not assigned a role</span></span>

<span data-ttu-id="e2c0b-145">*Hiba AADSTS50105: hello bejelentkezett felhasználó "brian@contoso.com" nem szerepét tooa hello alkalmazás*.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-145">*Error AADSTS50105: hello signed in user 'brian@contoso.com' is not assigned tooa role for hello application*.</span></span>

<span data-ttu-id="e2c0b-146">**Lehetséges ok**</span><span class="sxs-lookup"><span data-stu-id="e2c0b-146">**Possible cause**</span></span>

<span data-ttu-id="e2c0b-147">hello felhasználó nem rendelkezik hozzáféréssel toohello alkalmazás az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-147">hello user has not been granted access toohello application in Azure AD.</span></span>

<span data-ttu-id="e2c0b-148">**Megoldás**</span><span class="sxs-lookup"><span data-stu-id="e2c0b-148">**Resolution**</span></span>

<span data-ttu-id="e2c0b-149">tooassign felhasználók tooan egy vagy több alkalmazás közvetlenül, kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="e2c0b-149">tooassign one or more users tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="e2c0b-150">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="e2c0b-150">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="e2c0b-151">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-151">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="e2c0b-152">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-152">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="e2c0b-153">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-153">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="e2c0b-154">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-154">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="e2c0b-155">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="e2c0b-155">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="e2c0b-156">Válassza ki a kívánt felhasználó toofrom hello listáját tooassign hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-156">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="e2c0b-157">Amikor hello alkalmazás betölt, kattintson a **felhasználók és csoportok** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-157">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="e2c0b-158">Hello kattintson **Hozzáadás** gomb felett hello **felhasználók és csoportok** lista tooopen hello **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-158">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="e2c0b-159">hello kattintson **felhasználók és csoportok** hello a választó **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-159">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="e2c0b-160">Hello típusának **teljes név** vagy **e-mail cím** érdekli hello való hozzárendelése hello felhasználó **Keresés név vagy e-mail cím alapján** keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-160">Type in hello **full name** or **email address** of hello user you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="e2c0b-161">Hello rámutat **felhasználói** a hello lista tooreveal egy **jelölőnégyzet**.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-161">Hover over hello **user** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="e2c0b-162">Kattintson a hello jelölőnégyzet következő toohello felhasználó profil fénykép vagy embléma tooadd a felhasználó toohello **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-162">Click hello checkbox next toohello user’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="e2c0b-163">**Választható lehetőség:** Ha túl szeretné**egynél több felhasználó hozzáadása**, egy másik típus **teljes név** vagy **e-mail cím** be hello **Keresés név e-mail cím vagy** keresési mezőbe, majd kattintson a hello jelölőnégyzet tooadd a felhasználó toohello **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-163">**Optional:** If you would like too**add more than one user**, type in another **full name** or **email address** into hello **Search by name or email address** search box, and click hello checkbox tooadd this user toohello **Selected** list.</span></span>

13. <span data-ttu-id="e2c0b-164">Ha elkészült, válassza a felhasználók, kattintson a hello **válasszon** gomb tooadd őket felhasználók és csoportok toobe toohello listája toohello alkalmazás hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-164">When you are finished selecting users, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="e2c0b-165">**Nem kötelező:** hello kattintson **Szerepkörválasztás** hello a választó **hozzáadása hozzárendelés** panel tooselect szerepkör tooassign toohello felhasználók kijelölt.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-165">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello users you have selected.</span></span>

15. <span data-ttu-id="e2c0b-166">Kattintson a hello **hozzárendelése** gomb tooassign hello alkalmazás toohello kijelölt felhasználók.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-166">Click hello **Assign** button tooassign hello application toohello selected users.</span></span>

<span data-ttu-id="e2c0b-167">Rövid időn belül a kijelölt hello felhasználók kell ezeket az alkalmazásokat használó hello hello megoldás leírása szakaszban ismertetett módszerekkel tudja toolaunch.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-167">After a short period of time, hello users you have selected be able toolaunch these applications using hello methods described in hello solution description section.</span></span>

## <a name="not-a-valid-saml-request"></a><span data-ttu-id="e2c0b-168">Nem egy érvényes SAML-kérés</span><span class="sxs-lookup"><span data-stu-id="e2c0b-168">Not a valid SAML Request</span></span>

<span data-ttu-id="e2c0b-169">*Hiba AADSTS75005: hello kérelme, mert nem egy Saml2 protokoll érvényes üzenetet.*</span><span class="sxs-lookup"><span data-stu-id="e2c0b-169">*Error AADSTS75005: hello request is not a valid Saml2 protocol message.*</span></span>

<span data-ttu-id="e2c0b-170">**Lehetséges ok**</span><span class="sxs-lookup"><span data-stu-id="e2c0b-170">**Possible cause**</span></span>

<span data-ttu-id="e2c0b-171">Az Azure AD nem támogatja az SAML-kérelmek az egyszeri bejelentkezés hello alkalmazás által küldött hello.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-171">Azure AD doesn’t support hello SAML Request sent by hello application for Single Sign-on.</span></span> <span data-ttu-id="e2c0b-172">Néhány gyakori kérdések a következők:</span><span class="sxs-lookup"><span data-stu-id="e2c0b-172">Some common issues are:</span></span>

-   <span data-ttu-id="e2c0b-173">Hiányzik a kötelező mezőket hello SAML-kérelmet a</span><span class="sxs-lookup"><span data-stu-id="e2c0b-173">Missing required fields in hello SAML request</span></span>

-   <span data-ttu-id="e2c0b-174">SAML kódolt kérelmek módja</span><span class="sxs-lookup"><span data-stu-id="e2c0b-174">SAML request encoded method</span></span>

<span data-ttu-id="e2c0b-175">**Megoldás**</span><span class="sxs-lookup"><span data-stu-id="e2c0b-175">**Resolution**</span></span>

1.  <span data-ttu-id="e2c0b-176">Rögzítse a SAML-kérelmet.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-176">Capture SAML request.</span></span> <span data-ttu-id="e2c0b-177">hello az oktatóanyagot követve [hogyan toodebug SAML-alapú egyszeri bejelentkezés tooapplications az Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging) toolearn hogyan toocapture hello SAML kérelem.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-177">follow hello tutorial [How toodebug SAML-based single sign-on tooapplications in Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging) toolearn how toocapture hello SAML request.</span></span>

2.  <span data-ttu-id="e2c0b-178">Forduljon a hello alkalmazás gyártójának és megosztásnévhez:</span><span class="sxs-lookup"><span data-stu-id="e2c0b-178">Contact hello application vendor and share:</span></span>

   -   <span data-ttu-id="e2c0b-179">SAML-kérelmet</span><span class="sxs-lookup"><span data-stu-id="e2c0b-179">SAML request</span></span>

   -   [<span data-ttu-id="e2c0b-180">Azure AD-egyszeri bejelentkezés SAML protokoll követelmények</span><span class="sxs-lookup"><span data-stu-id="e2c0b-180">Azure AD Single Sign-on SAML protocol requirements</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference)

<span data-ttu-id="e2c0b-181">Ellenőrizni kell az egyszeri bejelentkezés hello Azure AD SAML-alapú megvalósítás támogatják.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-181">They should validate they support hello Azure AD SAML implementation for Single Sign-on.</span></span>

## <a name="no-resource-in-requiredresourceaccess-list"></a><span data-ttu-id="e2c0b-182">Nincs erőforrás requiredResourceAccess listában</span><span class="sxs-lookup"><span data-stu-id="e2c0b-182">No resource in requiredResourceAccess list</span></span>

<span data-ttu-id="e2c0b-183">*Hiba AADSTS65005:hello ügyfélalkalmazás kért hozzáférés tooresource "00000002-0000-0000-c000-000000000000'. A kérelem sikertelen volt, mert hello ügyfél ehhez az erőforráshoz nem megadott requiredResourceAccess listáján található*.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-183">*Error AADSTS65005:hello client application has requested access tooresource '00000002-0000-0000-c000-000000000000'. This request has failed because hello client has not specified this resource in its requiredResourceAccess list*.</span></span>

<span data-ttu-id="e2c0b-184">**Lehetséges ok**</span><span class="sxs-lookup"><span data-stu-id="e2c0b-184">**Possible cause**</span></span>

<span data-ttu-id="e2c0b-185">hello alkalmazásobjektum sérült.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-185">hello application object is corrupted.</span></span>

<span data-ttu-id="e2c0b-186">**Megoldás: 1. lehetőséget**</span><span class="sxs-lookup"><span data-stu-id="e2c0b-186">**Resolution: option 1**</span></span>

<span data-ttu-id="e2c0b-187">toosolve hello problémát, vegye fel a hello egyedi azonosító értéket hello Azure AD-konfigurációban.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-187">toosolve hello problem, add hello unique Identifier value in hello Azure AD configuration.</span></span> <span data-ttu-id="e2c0b-188">tooadd azonosító értéket, kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="e2c0b-188">tooadd a Identifier value, follow hello steps below:</span></span>

1.  <span data-ttu-id="e2c0b-189">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="e2c0b-189">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="e2c0b-190">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-190">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="e2c0b-191">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-191">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="e2c0b-192">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-192">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="e2c0b-193">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-193">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="e2c0b-194">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="e2c0b-194">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="e2c0b-195">Válassza ki az egyszeri bejelentkezés konfigurált hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-195">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="e2c0b-196">Miután hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menü</span><span class="sxs-lookup"><span data-stu-id="e2c0b-196">Once hello application loads, click on hello **Single sign-on** from hello application’s left hand navigation menu</span></span>

8.  <span data-ttu-id="e2c0b-197">A hello **tartomány és az URL-cím** szakaszban, tanulmányozza az hello **megjelenítése speciális URL-beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-197">Under hello **Domain and URL** section, check on hello **Show advanced URL settings**.</span></span>

9.  <span data-ttu-id="e2c0b-198">a hello **azonosító** szövegmezőbe írja be a hello alkalmazás egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-198">in hello **Identifier** textbox type a unique identifier for hello application.</span></span>

10. <span data-ttu-id="e2c0b-199">**Mentés** hello konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-199">**Save** hello configuration.</span></span>


<span data-ttu-id="e2c0b-200">**Névfeloldási beállítást 2**</span><span class="sxs-lookup"><span data-stu-id="e2c0b-200">**Resolution option 2**</span></span>

<span data-ttu-id="e2c0b-201">Ha a fenti 1 beállítás nem működik az Ön, távolítsa el hello alkalmazás hello könyvtárból.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-201">If option 1 above did not work for you, try removing hello application from hello directory.</span></span> <span data-ttu-id="e2c0b-202">Adja hozzá, és konfigurálja újra a hello alkalmazás, hajtsa végre az alábbi lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="e2c0b-202">Then, add and reconfigure hello application, follow hello steps below:</span></span>

1.  <span data-ttu-id="e2c0b-203">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="e2c0b-203">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="e2c0b-204">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-204">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="e2c0b-205">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-205">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="e2c0b-206">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-206">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="e2c0b-207">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-207">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="e2c0b-208">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="e2c0b-208">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="e2c0b-209">Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="e2c0b-209">Select hello application you want tooconfigure single sign-on</span></span>

7.  <span data-ttu-id="e2c0b-210">Kattintson a **törlése** hello felső – bal oldali hello alkalmazás **áttekintése** panelen.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-210">Click **Delete** at hello top-left of hello application **Overview** blade.</span></span>

8.  <span data-ttu-id="e2c0b-211">Frissítse az Azure AD, és adja hozzá a hello alkalmazás hello Azure AD-gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-211">Refresh Azure AD and Add hello application from hello Azure AD gallery.</span></span> <span data-ttu-id="e2c0b-212">Ezt követően konfigurálja a hello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="e2c0b-212">Then, Configure hello application</span></span>

<span data-ttu-id="e2c0b-213"><span id="_Hlk477190176" class="anchor"></span>Hello alkalmazás újrakonfigurálása, után toohello alkalmazás képes toosign kell lennie.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-213"><span id="_Hlk477190176" class="anchor"></span>After reconfiguring hello application, you should be able toosign in toohello application.</span></span>

## <a name="certificate-or-key-not-configured"></a><span data-ttu-id="e2c0b-214">Tanúsítvány és kulcs nincs konfigurálva</span><span class="sxs-lookup"><span data-stu-id="e2c0b-214">Certificate or key not configured</span></span>

<span data-ttu-id="e2c0b-215">*Hiba AADSTS50003: Nincs aláírási kulcs konfigurálva.*</span><span class="sxs-lookup"><span data-stu-id="e2c0b-215">*Error AADSTS50003: No signing key configured.*</span></span>

<span data-ttu-id="e2c0b-216">**Lehetséges ok**</span><span class="sxs-lookup"><span data-stu-id="e2c0b-216">**Possible cause**</span></span>

<span data-ttu-id="e2c0b-217">hello objektum sérült, és az Azure AD nem ismeri fel a hello alkalmazáshoz konfigurált hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-217">hello application object is corrupted and Azure AD doesn’t recognize hello certificate configured for hello application.</span></span>

<span data-ttu-id="e2c0b-218">**Megoldás**</span><span class="sxs-lookup"><span data-stu-id="e2c0b-218">**Resolution**</span></span>

<span data-ttu-id="e2c0b-219">toodelete, és hozzon létre egy új tanúsítványt, kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="e2c0b-219">toodelete and create a new certificate, follow hello steps below:</span></span>

1.  <span data-ttu-id="e2c0b-220">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="e2c0b-220">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="e2c0b-221">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-221">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="e2c0b-222">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-222">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="e2c0b-223">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-223">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="e2c0b-224">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-224">click **All Applications** tooview a list of all your applications.</span></span>

 * <span data-ttu-id="e2c0b-225">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="e2c0b-225">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="e2c0b-226">Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="e2c0b-226">Select hello application you want tooconfigure single sign-on</span></span>

7.  <span data-ttu-id="e2c0b-227">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-227">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="e2c0b-228">Kattintson a **hozzon létre új tanúsítvány** alatt hello **aláíró tanúsítvány SAML** szakasz.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-228">click **Create new certificate** under hello **SAML signing Certificate** section.</span></span>

9.  <span data-ttu-id="e2c0b-229">Válassza ki a lejárati dátum.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-229">Select Expiration date.</span></span> <span data-ttu-id="e2c0b-230">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="e2c0b-230">Then, click **Save.**</span></span>

10. <span data-ttu-id="e2c0b-231">Ellenőrizze **új tanúsítvány aktiválásához** toooverride hello aktív tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-231">Check **Make new certificate active** toooverride hello active certificate.</span></span> <span data-ttu-id="e2c0b-232">Kattintson a **mentése** hello panel hello tetején, és fogadja el a tooactivate hello helyettesítő tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-232">Then, click **Save** at hello top of hello blade and accept tooactivate hello rollover certificate.</span></span>

11. <span data-ttu-id="e2c0b-233">A hello **SAML-aláíró tanúsítványa** területén kattintson **eltávolítása** tooremove hello **nem használt** tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-233">Under hello **SAML Signing Certificate** section, click **remove** tooremove hello **Unused** certificate.</span></span>

## <a name="problem-when-customizing-hello-saml-claims-sent-tooan-application"></a><span data-ttu-id="e2c0b-234">Probléma hello SAML jogcímek testreszabása küldött tooan alkalmazás</span><span class="sxs-lookup"><span data-stu-id="e2c0b-234">Problem when customizing hello SAML claims sent tooan application</span></span>

<span data-ttu-id="e2c0b-235">toolearn hogyan toocustomize hello SAML attribútum típusú jogcímek küldött tooyour alkalmazás, lásd: [hozzárendelése az Azure Active Directory-jogcímek](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) további információt.</span><span class="sxs-lookup"><span data-stu-id="e2c0b-235">toolearn how toocustomize hello SAML attribute claims sent tooyour application, see [Claims mapping in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e2c0b-236">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e2c0b-236">Next steps</span></span>
[<span data-ttu-id="e2c0b-237">Hogyan toodebug SAML-alapú egyszeri bejelentkezés tooapplications Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="e2c0b-237">How toodebug SAML-based single sign-on tooapplications in Azure AD</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging)
