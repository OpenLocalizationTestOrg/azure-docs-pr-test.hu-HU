---
title: "tooa nem galéria alkalmazás konfigurált összevont bejelentkezés aaaProblems egyszeri bejelentkezés |} Microsoft Docs"
description: "Útmutató a bejelentkezéshez a SAML-alapú összevont egyszeri bejelentkezés az Azure ad szolgáltatással konfigurált tooan alkalmazás veszélyeket hello konkrét problémák"
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
ms.openlocfilehash: 1243456695c097f404a66fc89893efa2afdaaf22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="problems-signing-in-tooa-non-gallery-application-configured-for-federated-single-sign-on"></a><span data-ttu-id="bca8c-103">Problémák tooa nem galéria alkalmazás beállítása az összevont egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="bca8c-103">Problems signing in tooa non-gallery application configured for federated single sign-on</span></span>

<span data-ttu-id="bca8c-104">tootroubleshoot a probléma tooverify hello alkalmazás konfigurációja az alábbiak szerint Azure AD van szüksége:</span><span class="sxs-lookup"><span data-stu-id="bca8c-104">tootroubleshoot your problem, you need tooverify hello application configuration in Azure AD as follow:</span></span>

-   <span data-ttu-id="bca8c-105">Az Azure AD-gyűjtemény alkalmazást hello összes hello konfigurációs lépéseket követte.</span><span class="sxs-lookup"><span data-stu-id="bca8c-105">You have followed all hello configuration steps for hello Azure AD gallery application.</span></span>

-   <span data-ttu-id="bca8c-106">hello azonosító és az aad-ben megadott válasz URL-cím egyezik azok hello alkalmazás a várt értékek</span><span class="sxs-lookup"><span data-stu-id="bca8c-106">hello Identifier and Reply URL configured in AAD match they expected values in hello application</span></span>

-   <span data-ttu-id="bca8c-107">Hozzárendelt felhasználók toohello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="bca8c-107">You have assigned users toohello application</span></span>

## <a name="application-not-found-in-directory"></a><span data-ttu-id="bca8c-108">Az alkalmazás nem található a könyvtárban</span><span class="sxs-lookup"><span data-stu-id="bca8c-108">Application not found in directory</span></span>

<span data-ttu-id="bca8c-109">*Hiba AADSTS70001: "Https://contoso.com" azonosítójú alkalmazás nem található hello directory*.</span><span class="sxs-lookup"><span data-stu-id="bca8c-109">*Error AADSTS70001: Application with Identifier ‘https://contoso.com’ was not found in hello directory*.</span></span>

<span data-ttu-id="bca8c-110">**Lehetséges ok**</span><span class="sxs-lookup"><span data-stu-id="bca8c-110">**Possible cause**</span></span>

<span data-ttu-id="bca8c-111">hello kibocsátó attribútum küldi hello alkalmazás tooAzure a hello SAML-kérelmet AD nem felel meg az Azure AD hello alkalmazásban konfigurált hello azonosító értéket.</span><span class="sxs-lookup"><span data-stu-id="bca8c-111">hello Issuer attribute sends from hello application tooAzure AD in hello SAML request doesn’t match hello Identifier value configured in hello application Azure AD.</span></span>

<span data-ttu-id="bca8c-112">**Megoldás**</span><span class="sxs-lookup"><span data-stu-id="bca8c-112">**Resolution**</span></span>

<span data-ttu-id="bca8c-113">Ellenőrizze, hogy azt az egyező hello Azure AD-ben konfigurált azonosító értéket hello SAML-kérelmet található hello kibocsátó attribútum:</span><span class="sxs-lookup"><span data-stu-id="bca8c-113">Ensure that hello Issuer attribute in hello SAML request it’s matching hello Identifier value configured in Azure AD:</span></span>

1.  <span data-ttu-id="bca8c-114">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="bca8c-114">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="bca8c-115">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="bca8c-115">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="bca8c-116">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="bca8c-116">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="bca8c-117">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="bca8c-117">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="bca8c-118">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="bca8c-118">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="bca8c-119">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="bca8c-119">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="bca8c-120">Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bca8c-120">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="bca8c-121">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="bca8c-121">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="bca8c-122"><span id="_Hlk477190042" class="anchor"></span>Nyissa meg túl**tartomány és az URL-címek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="bca8c-122"><span id="_Hlk477190042" class="anchor"></span>Go too**Domain and URLs** section.</span></span> <span data-ttu-id="bca8c-123">Ellenőrizze a szövegmező van megfelelő hello hello azonosító értéket hello hiba jelenik meg a következő hello hello értéket.</span><span class="sxs-lookup"><span data-stu-id="bca8c-123">Verify that hello value in hello Identifier textbox is matching hello value for hello identifier value displayed in hello error.</span></span>

<span data-ttu-id="bca8c-124">Után hello azonosító értéket frissítette az Azure ad-ben, és megfelelő hello értéke hello alkalmazás a hello SAML-kérelmet küld, meg kell tudni toosign toohello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="bca8c-124">After you have updated hello Identifier value in Azure AD and it’s matching hello value sends by hello application in hello SAML request, you should be able toosign in toohello application.</span></span>

## <a name="hello-reply-address-does-not-match-hello-reply-addresses-configured-for-hello-application"></a><span data-ttu-id="bca8c-125">hello a válaszcím nem egyezik meg a hello válasz címek hello alkalmazáshoz konfigurált.</span><span class="sxs-lookup"><span data-stu-id="bca8c-125">hello reply address does not match hello reply addresses configured for hello application.</span></span> 

<span data-ttu-id="bca8c-126">*AADSTS50011. hiba: a hello válaszcímet "https://contoso.com" nem felel meg a hello válasz címek hello alkalmazáshoz konfigurált*</span><span class="sxs-lookup"><span data-stu-id="bca8c-126">*Error AADSTS50011: hello reply address ‘https://contoso.com’ does not match hello reply addresses configured for hello application*</span></span> 

<span data-ttu-id="bca8c-127">**Lehetséges ok**</span><span class="sxs-lookup"><span data-stu-id="bca8c-127">**Possible cause**</span></span> 

<span data-ttu-id="bca8c-128">hello hello SAML kérelem AssertionConsumerServiceURL értéke nem egyezik a hello válasz URL-CÍMEN értékkel vagy mintával konfigurálva az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="bca8c-128">hello AssertionConsumerServiceURL value in hello SAML request doesn't match hello Reply URL value or pattern configured in Azure AD.</span></span> <span data-ttu-id="bca8c-129">hello hello SAML-kérelmet AssertionConsumerServiceURL értéket az hello hiba látható hello URL-cím.</span><span class="sxs-lookup"><span data-stu-id="bca8c-129">hello AssertionConsumerServiceURL value in hello SAML request is hello URL you see in hello error.</span></span> 

<span data-ttu-id="bca8c-130">**Megoldás**</span><span class="sxs-lookup"><span data-stu-id="bca8c-130">**Resolution**</span></span> 

<span data-ttu-id="bca8c-131">Győződjön meg arról, hogy hello AssertionConsumerServiceURL értéket hello SAML-kérelmet az egyező hello válasz URL-címével konfigurálva az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bca8c-131">Ensure that hello AssertionConsumerServiceURL value in hello SAML request it's matching hello Reply URL value configured in Azure AD.</span></span> 
 
1.  <span data-ttu-id="bca8c-132">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="bca8c-132">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span> 

2.  <span data-ttu-id="bca8c-133">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="bca8c-133">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span> 

3.  <span data-ttu-id="bca8c-134">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="bca8c-134">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span> 

4.  <span data-ttu-id="bca8c-135">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="bca8c-135">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span> 

5.  <span data-ttu-id="bca8c-136">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="bca8c-136">click **All Applications** tooview a list of all your applications.</span></span> 

  * <span data-ttu-id="bca8c-137">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="bca8c-137">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and       set hello **Show** option too**All Applications.**</span></span>
  
6.  <span data-ttu-id="bca8c-138">Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="bca8c-138">Select hello application you want tooconfigure single sign-on</span></span>

7.  <span data-ttu-id="bca8c-139">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="bca8c-139">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="bca8c-140">Nyissa meg túl**tartomány és az URL-címek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="bca8c-140">Go too**Domain and URLs** section.</span></span> <span data-ttu-id="bca8c-141">Győződjön meg arról, vagy frissítse hello válasz URL-CÍMEN szövegmező toomatch hello AssertionConsumerServiceURL érték az SAML-kérelmet hello hello értéket.</span><span class="sxs-lookup"><span data-stu-id="bca8c-141">Verify or update hello value in hello Reply URL textbox toomatch hello AssertionConsumerServiceURL value in hello SAML request.</span></span>

  * <span data-ttu-id="bca8c-142">Ha nem lát hello válasz URL-cím beviteli mező, válassza ki a hello **megjelenítése speciális URL-beállításainak** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="bca8c-142">If you don't see hello Reply URL textbox, select hello **Show advanced URL settings** checkbox.</span></span> 

<span data-ttu-id="bca8c-143">Után az Azure ad-ben frissítette hello válasz URL-címével, és ennek megfelelő hello érték küld hello alkalmazás hello a SAML-kérelmet, toohello alkalmazás képes toosign kell lennie.</span><span class="sxs-lookup"><span data-stu-id="bca8c-143">After you have updated hello Reply URL value in Azure AD and it’s matching hello value sends by hello application in hello SAML request, you should be able toosign in toohello application.</span></span>

## <a name="user-not-assigned-a-role"></a><span data-ttu-id="bca8c-144">Nem hozzárendelt felhasználó</span><span class="sxs-lookup"><span data-stu-id="bca8c-144">User not assigned a role</span></span>

<span data-ttu-id="bca8c-145">*Hiba AADSTS50105: hello bejelentkezett felhasználó "brian@contoso.com" tooa szerepkör hello alkalmazáshoz nincs hozzárendelve*</span><span class="sxs-lookup"><span data-stu-id="bca8c-145">*Error AADSTS50105: hello signed in user 'brian@contoso.com' is not assigned tooa role for hello application*</span></span>

<span data-ttu-id="bca8c-146">**Lehetséges ok**</span><span class="sxs-lookup"><span data-stu-id="bca8c-146">**Possible cause**</span></span>

<span data-ttu-id="bca8c-147">hello felhasználó nem rendelkezik hozzáféréssel toohello alkalmazás az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="bca8c-147">hello user has not been granted access toohello application in Azure AD.</span></span>

<span data-ttu-id="bca8c-148">**Megoldás**</span><span class="sxs-lookup"><span data-stu-id="bca8c-148">**Resolution**</span></span>

<span data-ttu-id="bca8c-149">tooassign felhasználók tooan egy vagy több alkalmazás közvetlenül, kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="bca8c-149">tooassign one or more users tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="bca8c-150">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="bca8c-150">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="bca8c-151">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="bca8c-151">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="bca8c-152">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="bca8c-152">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="bca8c-153">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="bca8c-153">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="bca8c-154">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="bca8c-154">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="bca8c-155">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="bca8c-155">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="bca8c-156">Válassza ki a kívánt felhasználó toofrom hello listáját tooassign hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bca8c-156">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="bca8c-157">Amikor hello alkalmazás betölt, kattintson a **felhasználók és csoportok** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="bca8c-157">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="bca8c-158">Hello kattintson **Hozzáadás** gomb felett hello **felhasználók és csoportok** lista tooopen hello **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="bca8c-158">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="bca8c-159">hello kattintson **felhasználók és csoportok** hello a választó **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="bca8c-159">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="bca8c-160">Hello típusának **teljes név** vagy **e-mail cím** érdekli hello való hozzárendelése hello felhasználó **Keresés név vagy e-mail cím alapján** keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="bca8c-160">Type in hello **full name** or **email address** of hello user you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="bca8c-161">Hello rámutat **felhasználói** a hello lista tooreveal egy **jelölőnégyzet**.</span><span class="sxs-lookup"><span data-stu-id="bca8c-161">Hover over hello **user** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="bca8c-162">Kattintson a hello jelölőnégyzet következő toohello felhasználó profil fénykép vagy embléma tooadd a felhasználó toohello **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="bca8c-162">Click hello checkbox next toohello user’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="bca8c-163">**Választható lehetőség:** Ha túl szeretné**egynél több felhasználó hozzáadása**, egy másik típus **teljes név** vagy **e-mail cím** be hello **Keresés név e-mail cím vagy** keresési mezőbe, majd kattintson a hello jelölőnégyzet tooadd a felhasználó toohello **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="bca8c-163">**Optional:** If you would like too**add more than one user**, type in another **full name** or **email address** into hello **Search by name or email address** search box, and click hello checkbox tooadd this user toohello **Selected** list.</span></span>

13. <span data-ttu-id="bca8c-164">Ha elkészült, válassza a felhasználók, kattintson a hello **válasszon** gomb tooadd őket felhasználók és csoportok toobe toohello listája toohello alkalmazás hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="bca8c-164">When you are finished selecting users, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="bca8c-165">**Nem kötelező:** hello kattintson **Szerepkörválasztás** hello a választó **hozzáadása hozzárendelés** panel tooselect szerepkör tooassign toohello felhasználók kijelölt.</span><span class="sxs-lookup"><span data-stu-id="bca8c-165">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello users you have selected.</span></span>

15. <span data-ttu-id="bca8c-166">Kattintson a hello **hozzárendelése** gomb tooassign hello alkalmazás toohello kijelölt felhasználók.</span><span class="sxs-lookup"><span data-stu-id="bca8c-166">Click hello **Assign** button tooassign hello application toohello selected users.</span></span>

<span data-ttu-id="bca8c-167">Rövid időn belül a kijelölt hello felhasználók kell ezeket az alkalmazásokat használó hello hello megoldás leírása szakaszban ismertetett módszerekkel tudja toolaunch.</span><span class="sxs-lookup"><span data-stu-id="bca8c-167">After a short period of time, hello users you have selected be able toolaunch these applications using hello methods described in hello solution description section.</span></span>

## <a name="not-a-valid-saml-request"></a><span data-ttu-id="bca8c-168">Nem egy érvényes SAML-kérés</span><span class="sxs-lookup"><span data-stu-id="bca8c-168">Not a valid SAML Request</span></span>

<span data-ttu-id="bca8c-169">*Hiba AADSTS75005: hello kérelme, mert nem egy Saml2 protokoll érvényes üzenetet.*</span><span class="sxs-lookup"><span data-stu-id="bca8c-169">*Error AADSTS75005: hello request is not a valid Saml2 protocol message.*</span></span>

<span data-ttu-id="bca8c-170">**Lehetséges ok**</span><span class="sxs-lookup"><span data-stu-id="bca8c-170">**Possible cause**</span></span>

<span data-ttu-id="bca8c-171">Az Azure AD nem támogatja az SAML-kérelmek az egyszeri bejelentkezés hello alkalmazás által küldött hello.</span><span class="sxs-lookup"><span data-stu-id="bca8c-171">Azure AD doesn’t support hello SAML Request sent by hello application for Single Sign-on.</span></span> <span data-ttu-id="bca8c-172">Néhány gyakori kérdések a következők:</span><span class="sxs-lookup"><span data-stu-id="bca8c-172">Some common issues are:</span></span>

-   <span data-ttu-id="bca8c-173">Hiányzik a kötelező mezőket hello SAML-kérelmet a</span><span class="sxs-lookup"><span data-stu-id="bca8c-173">Missing required fields in hello SAML request</span></span>

-   <span data-ttu-id="bca8c-174">SAML kódolt kérelmek módja</span><span class="sxs-lookup"><span data-stu-id="bca8c-174">SAML request encoded method</span></span>

<span data-ttu-id="bca8c-175">**Megoldás**</span><span class="sxs-lookup"><span data-stu-id="bca8c-175">**Resolution**</span></span>

1.  <span data-ttu-id="bca8c-176">Rögzítse a SAML-kérelmet.</span><span class="sxs-lookup"><span data-stu-id="bca8c-176">Capture SAML request.</span></span> <span data-ttu-id="bca8c-177">az oktatóanyag hello [hogyan toodebug SAML-alapú egyszeri bejelentkezés tooapplications az Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging) toolearn hogyan toocapture hello SAML kérelem.</span><span class="sxs-lookup"><span data-stu-id="bca8c-177">follow hello tutorial on [how toodebug SAML-based single sign-on tooapplications in Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging) toolearn how toocapture hello SAML request.</span></span>

2.  <span data-ttu-id="bca8c-178">Forduljon a hello alkalmazás gyártójának és megosztásnévhez:</span><span class="sxs-lookup"><span data-stu-id="bca8c-178">Contact hello application vendor and share:</span></span>

    -   <span data-ttu-id="bca8c-179">SAML-kérelmet</span><span class="sxs-lookup"><span data-stu-id="bca8c-179">SAML request</span></span>

    -   [<span data-ttu-id="bca8c-180">Azure AD-egyszeri bejelentkezés SAML protokoll követelmények</span><span class="sxs-lookup"><span data-stu-id="bca8c-180">Azure AD Single Sign-on SAML protocol requirements</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference)

<span data-ttu-id="bca8c-181">Ellenőrizni kell az egyszeri bejelentkezés hello Azure AD SAML-alapú megvalósítás támogatják.</span><span class="sxs-lookup"><span data-stu-id="bca8c-181">They should validate they support hello Azure AD SAML implementation for Single Sign-on.</span></span>

## <a name="no-resource-in-requiredresourceaccess-list"></a><span data-ttu-id="bca8c-182">Nincs erőforrás requiredResourceAccess listában</span><span class="sxs-lookup"><span data-stu-id="bca8c-182">No resource in requiredResourceAccess list</span></span>

<span data-ttu-id="bca8c-183">*Hiba AADSTS65005: hello ügyfélalkalmazás kért hozzáférés tooresource "00000002-0000-0000-c000-000000000000'. A kérelem sikertelen volt, mert hello ügyfél ehhez az erőforráshoz nem megadott requiredResourceAccess listáján található*.</span><span class="sxs-lookup"><span data-stu-id="bca8c-183">*Error AADSTS65005: hello client application has requested access tooresource '00000002-0000-0000-c000-000000000000'. This request has failed because hello client has not specified this resource in its requiredResourceAccess list*.</span></span>

<span data-ttu-id="bca8c-184">**Lehetséges ok**</span><span class="sxs-lookup"><span data-stu-id="bca8c-184">**Possible cause**</span></span>

<span data-ttu-id="bca8c-185">hello alkalmazásobjektum sérült.</span><span class="sxs-lookup"><span data-stu-id="bca8c-185">hello application object is corrupted.</span></span>

<span data-ttu-id="bca8c-186">**Megoldás**</span><span class="sxs-lookup"><span data-stu-id="bca8c-186">**Resolution**</span></span>

<span data-ttu-id="bca8c-187">toosolve hello probléma, távolítsa el hello alkalmazás hello könyvtárból.</span><span class="sxs-lookup"><span data-stu-id="bca8c-187">toosolve hello problem, remove hello application from hello directory.</span></span> <span data-ttu-id="bca8c-188">Adja hozzá, és konfigurálja újra a hello alkalmazás, hajtsa végre az alábbi lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="bca8c-188">Then, add and reconfigure hello application, follow hello steps below:</span></span>

1.  <span data-ttu-id="bca8c-189">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="bca8c-189">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="bca8c-190">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="bca8c-190">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="bca8c-191">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="bca8c-191">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="bca8c-192">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="bca8c-192">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="bca8c-193">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="bca8c-193">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="bca8c-194">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="bca8c-194">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="bca8c-195">Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bca8c-195">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="bca8c-196">Kattintson a **törlése** hello felső – bal oldali hello alkalmazás **áttekintése** panelen.</span><span class="sxs-lookup"><span data-stu-id="bca8c-196">Click **Delete** at hello top-left of hello application **Overview** blade.</span></span>

8.  <span data-ttu-id="bca8c-197">Frissítse az Azure AD, és adja hozzá a hello alkalmazás hello Azure AD-gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="bca8c-197">Refresh Azure AD and Add hello application from hello Azure AD gallery.</span></span> <span data-ttu-id="bca8c-198">Ezt követően konfigurálja újra a hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="bca8c-198">Then, Configure hello application again.</span></span>

<span data-ttu-id="bca8c-199">Hello alkalmazás újrakonfigurálása, után toohello alkalmazás képes toosign kell lennie.</span><span class="sxs-lookup"><span data-stu-id="bca8c-199">After reconfiguring hello application, you should be able toosign in toohello application.</span></span>

## <a name="certificate-or-key-not-configured"></a><span data-ttu-id="bca8c-200">Tanúsítvány és kulcs nincs konfigurálva</span><span class="sxs-lookup"><span data-stu-id="bca8c-200">Certificate or key not configured</span></span>

<span data-ttu-id="bca8c-201">Hiba AADSTS50003: Nincs aláírási kulcs konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="bca8c-201">Error AADSTS50003: No signing key configured.</span></span>

<span data-ttu-id="bca8c-202">**Lehetséges ok**</span><span class="sxs-lookup"><span data-stu-id="bca8c-202">**Possible cause**</span></span>

<span data-ttu-id="bca8c-203">hello objektum sérült, és az Azure AD nem ismeri fel a hello alkalmazáshoz konfigurált hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="bca8c-203">hello application object is corrupted and Azure AD doesn’t recognize hello certificate configured for hello application.</span></span>

<span data-ttu-id="bca8c-204">**Megoldás**</span><span class="sxs-lookup"><span data-stu-id="bca8c-204">**Resolution**</span></span>

<span data-ttu-id="bca8c-205">toodelete, és hozzon létre egy új tanúsítványt, kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="bca8c-205">toodelete and create a new certificate, follow hello steps below:</span></span>

1.  <span data-ttu-id="bca8c-206">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="bca8c-206">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="bca8c-207">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="bca8c-207">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="bca8c-208">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="bca8c-208">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="bca8c-209">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="bca8c-209">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="bca8c-210">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="bca8c-210">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="bca8c-211">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="bca8c-211">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="bca8c-212">Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bca8c-212">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="bca8c-213">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="bca8c-213">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="bca8c-214">Kattintson a **hozzon létre új tanúsítvány** alatt hello **aláíró tanúsítvány SAML** szakasz.</span><span class="sxs-lookup"><span data-stu-id="bca8c-214">click **Create new certificate** under hello **SAML signing Certificate** section.</span></span>

9.  <span data-ttu-id="bca8c-215">Válassza ki a lejárati dátum.</span><span class="sxs-lookup"><span data-stu-id="bca8c-215">Select Expiration date.</span></span> <span data-ttu-id="bca8c-216">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="bca8c-216">Then, click **Save.**</span></span>

10. <span data-ttu-id="bca8c-217">Ellenőrizze **új tanúsítvány aktiválásához** toooverride hello aktív tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="bca8c-217">Check **Make new certificate active** toooverride hello active certificate.</span></span> <span data-ttu-id="bca8c-218">Kattintson a **mentése** hello panel hello tetején, és fogadja el a tooactivate hello helyettesítő tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="bca8c-218">Then, click **Save** at hello top of hello blade and accept tooactivate hello rollover certificate.</span></span>

11. <span data-ttu-id="bca8c-219">A hello **SAML-aláíró tanúsítványa** területén kattintson **eltávolítása** tooremove hello **nem használt** tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="bca8c-219">Under hello **SAML Signing Certificate** section, click **remove** tooremove hello **Unused** certificate.</span></span>

## <a name="problem-when-customizing-hello-saml-claims-sent-tooan-application"></a><span data-ttu-id="bca8c-220">Probléma hello SAML jogcímek testreszabása küldött tooan alkalmazás</span><span class="sxs-lookup"><span data-stu-id="bca8c-220">Problem when customizing hello SAML claims sent tooan application</span></span>

<span data-ttu-id="bca8c-221">toolearn hogyan toocustomize hello SAML attribútum típusú jogcímek küldött tooyour alkalmazás, lásd: [hozzárendelése az Azure Active Directory-jogcímek](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) további információt.</span><span class="sxs-lookup"><span data-stu-id="bca8c-221">toolearn how toocustomize hello SAML attribute claims sent tooyour application, see [Claims mapping in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bca8c-222">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bca8c-222">Next steps</span></span>
[<span data-ttu-id="bca8c-223">Azure AD-egyszeri bejelentkezés SAML protokoll követelmények</span><span class="sxs-lookup"><span data-stu-id="bca8c-223">Azure AD Single Sign-on SAML protocol requirements</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference)
