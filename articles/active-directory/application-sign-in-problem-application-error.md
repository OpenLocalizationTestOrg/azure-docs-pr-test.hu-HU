---
title: "a bejelentkezés után az alkalmazás oldalon aaaError |} Microsoft Docs"
description: "Hogyan tooresolve problémák az Azure ad-val során a bejelentkezéshez hello alkalmazás maga bocsát ki a hiba"
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
ms.openlocfilehash: 317b6f8e6417520ead80ae4e26c591ba6b134683
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="error-on-an-applications-page-after-signing-in"></a><span data-ttu-id="3727c-103">Hiba történt a bejelentkezés után az alkalmazás az oldalon</span><span class="sxs-lookup"><span data-stu-id="3727c-103">Error on an application's page after signing in</span></span>

<span data-ttu-id="3727c-104">Ebben a forgatókönyvben az Azure AD hello felhasználó írt alá, de hello alkalmazás nem teszi lehetővé hello felhasználói toosuccessfully Befejezés hello bejelentkezési folyamata hibát jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="3727c-104">In this scenario, Azure AD has signed hello user in, but hello application is displaying an error not allowing hello user toosuccessfully finish hello sign in flow.</span></span> <span data-ttu-id="3727c-105">Ebben a forgatókönyvben hello alkalmazás nem fogad el hello válasz probléma az Azure ad.</span><span class="sxs-lookup"><span data-stu-id="3727c-105">In this scenario, hello application is not accepting hello response issue by Azure AD.</span></span>

<span data-ttu-id="3727c-106">Ennek oka néhány miért hello alkalmazás nem fogadta el az Azure AD hello válaszát.</span><span class="sxs-lookup"><span data-stu-id="3727c-106">There are some possible reasons why hello application didn’t accept hello response from Azure AD.</span></span> <span data-ttu-id="3727c-107">Ha hello hiba hello alkalmazásban nem elég egyértelmű tooknow Mi hiányzik hello válaszként, majd:</span><span class="sxs-lookup"><span data-stu-id="3727c-107">If hello error in hello application is not clear enough tooknow what is missing in hello response, then:</span></span>

-   <span data-ttu-id="3727c-108">Az Azure AD hello gyűjteménye hello alkalmazás esetén ellenőrizze a hello cikkben minden hello lépésekkel [hogyan toodebug SAML-alapú egyszeri bejelentkezés tooapplications az Azure Active Directoryban](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging).</span><span class="sxs-lookup"><span data-stu-id="3727c-108">If hello application is hello Azure AD Gallery, verify you have followed all hello steps in hello article [How toodebug SAML-based single sign-on tooapplications in Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging).</span></span>

-   <span data-ttu-id="3727c-109">Egy eszköz, például használja [Fiddler](http://www.telerik.com/fiddler) toocapture SAML kérte, SAML-válasz és SAML-jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="3727c-109">Use a tool like [Fiddler](http://www.telerik.com/fiddler) toocapture SAML request, SAML response and SAML token.</span></span>

-   <span data-ttu-id="3727c-110">Közös hello SAML-válasz hello alkalmazás szállító tooknow Mi hiányzik.</span><span class="sxs-lookup"><span data-stu-id="3727c-110">Share hello SAML response with hello application vendor tooknow what is missing.</span></span>

## <a name="missing-attributes-in-hello-saml-response"></a><span data-ttu-id="3727c-111">Hiányzó attribútumok hello SAML-válasz</span><span class="sxs-lookup"><span data-stu-id="3727c-111">Missing attributes in hello SAML response</span></span>

<span data-ttu-id="3727c-112">tooadd hello Azure AD konfigurációs toobe hello Azure AD válaszként küldött attribútum hello lépéseket kövesse:</span><span class="sxs-lookup"><span data-stu-id="3727c-112">tooadd an attribute in hello Azure AD configuration toobe sent in hello Azure AD response, follow hello steps below:</span></span>

1.  <span data-ttu-id="3727c-113">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="3727c-113">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="3727c-114">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="3727c-114">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="3727c-115">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="3727c-115">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="3727c-116">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="3727c-116">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="3727c-117">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="3727c-117">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="3727c-118">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="3727c-118">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="3727c-119">Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3727c-119">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="3727c-120">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="3727c-120">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="3727c-121">Kattintson a **megtekintés és Szerkesztés minden más felhasználói attribútumok alapján** hello **felhasználói attribútumok** szakasz tooedit hello attribútumok küldött toobe toohello alkalmazás hello SAML-jogkivonat felhasználói bejelentkezéskor.</span><span class="sxs-lookup"><span data-stu-id="3727c-121">click **View and edit all other user attributes under** hello **User Attributes** section tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="3727c-122">tooadd attribútum:</span><span class="sxs-lookup"><span data-stu-id="3727c-122">tooadd an attribute:</span></span>

   * <span data-ttu-id="3727c-123">Kattintson a **Hozzáadás attribútum**.</span><span class="sxs-lookup"><span data-stu-id="3727c-123">click **Add attribute**.</span></span> <span data-ttu-id="3727c-124">Adja meg a hello **neve** és hello válassza hello **érték** hello legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="3727c-124">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   * <span data-ttu-id="3727c-125">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="3727c-125">Click **Save.**</span></span> <span data-ttu-id="3727c-126">Új attribútum hello hello táblázatban láthatja.</span><span class="sxs-lookup"><span data-stu-id="3727c-126">You see hello new attribute in hello table.</span></span>

9.  <span data-ttu-id="3727c-127">Hello konfigurációjának mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="3727c-127">Save hello configuration.</span></span>

<span data-ttu-id="3727c-128">Hello felhasználó jelentkezik be toohello alkalmazást, amikor legközelebb az Azure AD küldenek hello új attribútum hello SAML-válasz.</span><span class="sxs-lookup"><span data-stu-id="3727c-128">Next time hello user signs in toohello application, Azure AD send hello new attribute in hello SAML response.</span></span>

## <a name="hello-application-expects-a-different-user-identifier-value-or-format"></a><span data-ttu-id="3727c-129">hello alkalmazás egy másik felhasználói azonosító értéke vagy a format vár</span><span class="sxs-lookup"><span data-stu-id="3727c-129">hello application expects a different User Identifier value or format</span></span>

<span data-ttu-id="3727c-130">hello bejelentkezési toohello alkalmazás sikertelen, mert hello SAML-válasz attribútumok például a szerepkörök hiányzik, vagy hello alkalmazás által várt paraméterekkel hello entityid beállítást attribútum formátumát.</span><span class="sxs-lookup"><span data-stu-id="3727c-130">hello sign-in toohello application is failing because hello SAML response is missing attributes such as roles or because hello application is expecting a different format for hello EntityID attribute.</span></span>

## <a name="add-an-attribute-in-hello-azure-ad-application-configuration"></a><span data-ttu-id="3727c-131">Attribútum hozzáadása a hello Azure AD alkalmazás beállításai:</span><span class="sxs-lookup"><span data-stu-id="3727c-131">Add an attribute in hello Azure AD application configuration:</span></span>

<span data-ttu-id="3727c-132">toochange hello felhasználói azonosító értéke, kövesse a hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="3727c-132">toochange hello User Identifier value, follow hello steps below:</span></span>

1.  <span data-ttu-id="3727c-133">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="3727c-133">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="3727c-134">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="3727c-134">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="3727c-135">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="3727c-135">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="3727c-136">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="3727c-136">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="3727c-137">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="3727c-137">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="3727c-138">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="3727c-138">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="3727c-139">Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3727c-139">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="3727c-140">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="3727c-140">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="3727c-141">A hello **felhasználói attribútumok**, válassza ki a felhasználók a hello egyedi azonosítója hello **felhasználói azonosító** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="3727c-141">Under hello **User attributes**, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span>

## <a name="change-entityid-user-identifier-format"></a><span data-ttu-id="3727c-142">Entityid (felhasználói azonosító) beállítást formátumának módosítása</span><span class="sxs-lookup"><span data-stu-id="3727c-142">Change EntityID (User Identifier) format</span></span>

<span data-ttu-id="3727c-143">Ha hello alkalmazás hello entityid beállítást attribútum egy másik formátumát vár.</span><span class="sxs-lookup"><span data-stu-id="3727c-143">If hello application expects another format for hello EntityID attribute.</span></span> <span data-ttu-id="3727c-144">Majd nem fogja tudni tooselect hello entityid (felhasználói azonosító) beállítást formátuma, hogy az Azure AD küld toohello alkalmazás hello válaszul felhasználói hitelesítés után.</span><span class="sxs-lookup"><span data-stu-id="3727c-144">Then, you won’t be able tooselect hello EntityID (User Identifier) format that Azure AD sends toohello application in hello response after user authentication.</span></span>

<span data-ttu-id="3727c-145">Az Azure AD válassza hello formátumban hello NameID attribútum (felhasználói azonosító) kijelölt hello érték alapján, vagy a SAML AuthRequest hello hello alkalmazás által kért formátum hello.</span><span class="sxs-lookup"><span data-stu-id="3727c-145">Azure AD select hello format for hello NameID attribute (User Identifier) based on hello value selected or hello format requested by hello application in hello SAML AuthRequest.</span></span> <span data-ttu-id="3727c-146">További információt a Microsoft hello cikk [egyszeri bejelentkezés SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) az hello rész NameIDPolicy.</span><span class="sxs-lookup"><span data-stu-id="3727c-146">For more information visit hello article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello section NameIDPolicy.</span></span>

## <a name="hello-application-expects-a-different-signature-method-for-hello-saml-response"></a><span data-ttu-id="3727c-147">hello alkalmazás vár egy eltérő aláírással módszer, amellyel hello SAML-válasz</span><span class="sxs-lookup"><span data-stu-id="3727c-147">hello application expects a different signature method for hello SAML response</span></span>

<span data-ttu-id="3727c-148">Azure Active Directory által digitálisan aláírt hello SAML-jogkivonat részeinek toochange.</span><span class="sxs-lookup"><span data-stu-id="3727c-148">toochange what parts of hello SAML token are digitally signed by Azure Active Directory.</span></span> <span data-ttu-id="3727c-149">Kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="3727c-149">Follow hello steps below:</span></span>

1.  <span data-ttu-id="3727c-150">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="3727c-150">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="3727c-151">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="3727c-151">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="3727c-152">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="3727c-152">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="3727c-153">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="3727c-153">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="3727c-154">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="3727c-154">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="3727c-155">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="3727c-155">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="3727c-156">Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3727c-156">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="3727c-157">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="3727c-157">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="3727c-158">Kattintson a **megjelenítése speciális tanúsítvány-aláírási beállítások** alatt hello **SAML-aláíró tanúsítványa** szakasz.</span><span class="sxs-lookup"><span data-stu-id="3727c-158">click **Show advanced certificate signing settings** under hello **SAML Signing Certificate** section.</span></span>

9.  <span data-ttu-id="3727c-159">Jelölje be hello megfelelő **aláíró beállítás** hello alkalmazás által várt:</span><span class="sxs-lookup"><span data-stu-id="3727c-159">Select hello appropriate **Signing Option** expected by hello application:</span></span>

  * <span data-ttu-id="3727c-160">Bejelentkezési SAML-válasz</span><span class="sxs-lookup"><span data-stu-id="3727c-160">Sign SAML response</span></span>

  * <span data-ttu-id="3727c-161">Bejelentkezési SAML-válasz és helyességi feltétel</span><span class="sxs-lookup"><span data-stu-id="3727c-161">Sign SAML response and assertion</span></span>

  * <span data-ttu-id="3727c-162">Bejelentkezési SAML-előfeltétel</span><span class="sxs-lookup"><span data-stu-id="3727c-162">Sign SAML assertion</span></span>

<span data-ttu-id="3727c-163">Hello felhasználó jelentkezik be toohello alkalmazást, amikor legközelebb az Azure AD bejelentkezési hello hello SAML-válasz kijelölt részét.</span><span class="sxs-lookup"><span data-stu-id="3727c-163">Next time hello user signs in toohello application, Azure AD sign hello part of hello SAML response selected.</span></span>

## <a name="hello-application-expects-hello-signing-algorithm-toobe-sha-1"></a><span data-ttu-id="3727c-164">hello alkalmazást aláíró algoritmus toobe SHA-1 hello vár</span><span class="sxs-lookup"><span data-stu-id="3727c-164">hello application expects hello signing algorithm toobe SHA-1</span></span>

<span data-ttu-id="3727c-165">Alapértelmezés szerint az Azure AD aláírja a hello SAML-jogkivonat hello használata a legtöbb biztonsági algoritmus.</span><span class="sxs-lookup"><span data-stu-id="3727c-165">By default, Azure AD signs hello SAML token using hello most security algorithm.</span></span> <span data-ttu-id="3727c-166">Hello bejelentkezési algoritmus tooSHA-1 módosítása nem ajánlott, kivéve, ha hello alkalmazáshoz szükséges.</span><span class="sxs-lookup"><span data-stu-id="3727c-166">Changing hello sign algorithm tooSHA-1 is not recommended unless required by hello application.</span></span>

<span data-ttu-id="3727c-167">toochange aláírási algoritmus hello hajtsa végre az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="3727c-167">toochange hello signing algorithm, follow hello steps below:</span></span>

1.  <span data-ttu-id="3727c-168">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="3727c-168">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="3727c-169">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="3727c-169">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="3727c-170">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="3727c-170">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="3727c-171">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="3727c-171">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="3727c-172">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="3727c-172">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="3727c-173">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="3727c-173">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="3727c-174">Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3727c-174">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="3727c-175">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="3727c-175">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="3727c-176">Kattintson a **megjelenítése speciális tanúsítvány-aláírási beállítások** alatt hello **SAML-aláíró tanúsítványa** szakasz.</span><span class="sxs-lookup"><span data-stu-id="3727c-176">click **Show advanced certificate signing settings** under hello **SAML Signing Certificate** section.</span></span>

9.  <span data-ttu-id="3727c-177">Válassza ki az SHA-1, a hello **aláírási algoritmus**.</span><span class="sxs-lookup"><span data-stu-id="3727c-177">Select SHA-1, in hello **Signing Algorithm**.</span></span>

<span data-ttu-id="3727c-178">Amikor legközelebb hello toohello alkalmazás felhasználó bejelentkezik, az Azure AD bejelentkezési hello SHA-1 algoritmussal SAML-jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="3727c-178">Next time hello user signs in toohello application, Azure AD sign hello SAML token using SHA-1 algorithm.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3727c-179">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3727c-179">Next steps</span></span>
[<span data-ttu-id="3727c-180">Hogyan toodebug SAML-alapú egyszeri bejelentkezés tooapplications az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="3727c-180">How toodebug SAML-based single sign-on tooapplications in Azure Active Directory</span></span>](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging)
