---
title: "egyszeri bejelentkezés az Azure AD-katalógusában alkalmazás összevont aaaHow tooconfigure |} Microsoft Docs"
description: "Hogyan tooconfigure összevont egyszeri bejelentkezéshez egy meglévő Azure AD-katalógusában és használati oktatóanyagok tooget is gyorsan az"
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
ms.openlocfilehash: a93de021fddd253e4fe663c221b822d12625fd54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="fe2de-103">Hogyan tooconfigure összevont egyszeri bejelentkezés az Azure AD-katalógusában alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="fe2de-103">How tooconfigure federated single sign-on for an Azure AD Gallery application</span></span>

<span data-ttu-id="fe2de-104">Összes alkalmazás engedélyezve van a vállalati egyszeri bejelentkezési képesség hello Azure AD-katalógus elérhető egy részletes oktatóanyag rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="fe2de-104">All applications in hello Azure AD gallery enabled with Enterprise single sign-on capability has a step-by-step tutorial available.</span></span> <span data-ttu-id="fe2de-105">Van-e hozzáférési hello [kapcsolatos bemutatók felsorolása toointegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) részletes, lépésenkénti útmutatást.</span><span class="sxs-lookup"><span data-stu-id="fe2de-105">You can access hello [List of tutorials on how toointegrate SaaS apps with Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) for detailed step-by-step guidance.</span></span>

## <a name="overview-of-steps-required"></a><span data-ttu-id="fe2de-106">A szükséges lépések – áttekintés</span><span class="sxs-lookup"><span data-stu-id="fe2de-106">Overview of steps required</span></span>
<span data-ttu-id="fe2de-107">egy alkalmazás hello Azure AD-galériából tooconfigure kell:</span><span class="sxs-lookup"><span data-stu-id="fe2de-107">tooconfigure an application from hello Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="fe2de-108">Alkalmazás hozzáadása hello Azure AD-galériából</span><span class="sxs-lookup"><span data-stu-id="fe2de-108">Add an application from hello Azure AD gallery</span></span>](#add-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="fe2de-109">Hello alkalmazás metaadatainak értékek konfigurálása az Azure AD (bejelentkezési URL-cím, az azonosítója, válasz URL-címe)</span><span class="sxs-lookup"><span data-stu-id="fe2de-109">Configure hello application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="fe2de-110">Válassza ki a felhasználói azonosítót és a felhasználói attribútumok küldött toobe toohello alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fe2de-110">Select User Identifier and add user attributes toobe sent toohello application</span></span>](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [<span data-ttu-id="fe2de-111">Az Azure AD-metaadatok és a tanúsítvány lekérése</span><span class="sxs-lookup"><span data-stu-id="fe2de-111">Retrieve Azure AD metadata and certificate</span></span>](#download-the-azure-ad-metadata-or-certificate)

-   [<span data-ttu-id="fe2de-112">Konfigurálja az Azure AD metaadatok értékeket hello alkalmazásban (bejelentkezési URL-címet, a kibocsátó, a kijelentkezési URL-cím és a tanúsítvány)</span><span class="sxs-lookup"><span data-stu-id="fe2de-112">Configure Azure AD metadata values in hello application (Sign on URL, Issuer, Logout URL and certificate)</span></span>](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="fe2de-113">Felhasználók hozzárendelése toohello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="fe2de-113">Assign users toohello application</span></span>](#assign-users-to-the-application)

## <a name="add-an-application-from-hello-azure-ad-gallery"></a><span data-ttu-id="fe2de-114">Alkalmazás hozzáadása hello Azure AD-galériából</span><span class="sxs-lookup"><span data-stu-id="fe2de-114">Add an application from hello Azure AD gallery</span></span>

<span data-ttu-id="fe2de-115">az alkalmazás a hello Azure AD-katalógusában, tooadd kövesse hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="fe2de-115">tooadd an application from hello Azure AD Gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="fe2de-116">Nyissa meg hello [Azure Portal](https://portal.azure.com) , és jelentkezzen be a egy **globális rendszergazda** vagy **társadminisztrátornak**</span><span class="sxs-lookup"><span data-stu-id="fe2de-116">Open hello [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="fe2de-117">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="fe2de-117">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="fe2de-118">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="fe2de-118">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="fe2de-119">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="fe2de-119">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="fe2de-120">Kattintson a hello **hozzáadása** hello jobb felső sarokban lévő hello a gomb **vállalati alkalmazások** panelen.</span><span class="sxs-lookup"><span data-stu-id="fe2de-120">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="fe2de-121">A hello **adjon meg egy nevet** hello a szövegmező **hello gyűjteményből Hozzáadás** szakaszban, hello alkalmazás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="fe2de-121">In hello **Enter a name** textbox from hello **Add from hello gallery** section, type hello name of hello application.</span></span>

7.  <span data-ttu-id="fe2de-122">Válassza ki a használni kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="fe2de-122">Select hello application you want tooconfigure for single sign-on.</span></span>

8.  <span data-ttu-id="fe2de-123">Hello alkalmazás hozzáadása előtt módosíthatja annak nevét a hello **neve** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="fe2de-123">Before adding hello application, you can change its name from hello **Name** textbox.</span></span>

9.  <span data-ttu-id="fe2de-124">Kattintson a **Hozzáadás** tooadd hello alkalmazás gomb.</span><span class="sxs-lookup"><span data-stu-id="fe2de-124">Click **Add** button, tooadd hello application.</span></span>

<span data-ttu-id="fe2de-125">Rövid időn belül kell tudni toosee hello alkalmazás konfigurációs panelen.</span><span class="sxs-lookup"><span data-stu-id="fe2de-125">After a short period of time, you be able toosee hello application’s configuration blade.</span></span>

## <a name="configure-single-sign-on-for-an-application-from-hello-azure-ad-gallery"></a><span data-ttu-id="fe2de-126">Egyszeri bejelentkezés egy alkalmazás hello Azure AD-galériából konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fe2de-126">Configure single sign-on for an application from hello Azure AD gallery</span></span>

<span data-ttu-id="fe2de-127">tooconfigure egyszeri bejelentkezés egy alkalmazás kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="fe2de-127">tooconfigure single sign-on for an application, follow hello steps below:</span></span>

1.  <span data-ttu-id="fe2de-128">Megnyitás hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **társadminisztrátor**.</span><span class="sxs-lookup"><span data-stu-id="fe2de-128">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin**.</span></span>

2.  <span data-ttu-id="fe2de-129">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="fe2de-129">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="fe2de-130">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="fe2de-130">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="fe2de-131">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="fe2de-131">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="fe2de-132">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="fe2de-132">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="fe2de-133">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="fe2de-133">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="fe2de-134">Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="fe2de-134">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="fe2de-135">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="fe2de-135">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="fe2de-136">Válassza ki **SAML-alapú bejelentkezés** a hello **mód** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="fe2de-136">Select **SAML-based Sign-on** from hello **Mode** dropdown.</span></span>

9.  <span data-ttu-id="fe2de-137">Adja meg a szükséges hello értékeket **tartomány és az URL-címeket.**</span><span class="sxs-lookup"><span data-stu-id="fe2de-137">Enter hello required values in **Domain and URLs.**</span></span> <span data-ttu-id="fe2de-138">Ezeket az értékeket kell beszerezni hello alkalmazás gyártójának segítségét.</span><span class="sxs-lookup"><span data-stu-id="fe2de-138">You should get these values from hello application vendor.</span></span>

   1. <span data-ttu-id="fe2de-139">tooconfigure hello alkalmazásra, amely a Szolgáltató által kezdeményezett egyszeri Bejelentkezést, hello bejelentkezési URL-címen egy szükséges érték.</span><span class="sxs-lookup"><span data-stu-id="fe2de-139">tooconfigure hello application as SP-initiated SSO, hello Sign on URL it’s a required value.</span></span> <span data-ttu-id="fe2de-140">Egyes alkalmazások hello azonosító értéke is szükséges.</span><span class="sxs-lookup"><span data-stu-id="fe2de-140">For some applications, hello Identifier is also a required value.</span></span>

   2. <span data-ttu-id="fe2de-141">tooconfigure hello alkalmazásra, amely a kiállító terjesztési hely által kezdeményezett egyszeri Bejelentkezést, hello válasz URL-cím szükséges érték.</span><span class="sxs-lookup"><span data-stu-id="fe2de-141">tooconfigure hello application as IdP-initiated SSO, hello Reply URL it’s a required value.</span></span> <span data-ttu-id="fe2de-142">Egyes alkalmazások hello azonosító értéke is szükséges.</span><span class="sxs-lookup"><span data-stu-id="fe2de-142">For some applications, hello Identifier is also a required value.</span></span>

10. <span data-ttu-id="fe2de-143">**Választható lehetőség:** kattintson **megjelenítése speciális URL-beállításainak** Ha azt szeretné, hogy toosee hello nem szükséges értékeket.</span><span class="sxs-lookup"><span data-stu-id="fe2de-143">**Optional:** click **Show advanced URL settings** if you want toosee hello non-required values.</span></span>

11. <span data-ttu-id="fe2de-144">A hello **felhasználói attribútumok**, válassza ki a felhasználók a hello egyedi azonosítója hello **felhasználói azonosító** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="fe2de-144">In hello **User attributes**, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span>

12. <span data-ttu-id="fe2de-145">**Nem kötelező:** kattintson **megtekintése és szerkesztése a más felhasználói attribútumok** tooedit hello attribútumok küldött toobe toohello alkalmazás hello SAML-jogkivonat felhasználói bejelentkezéskor.</span><span class="sxs-lookup"><span data-stu-id="fe2de-145">**Optional:** click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

  <span data-ttu-id="fe2de-146">tooadd attribútum:</span><span class="sxs-lookup"><span data-stu-id="fe2de-146">tooadd an attribute:</span></span>
   
   1. <span data-ttu-id="fe2de-147">Kattintson a **Hozzáadás attribútum**.</span><span class="sxs-lookup"><span data-stu-id="fe2de-147">click **Add attribute**.</span></span> <span data-ttu-id="fe2de-148">Adja meg a hello **neve** és hello válassza hello **érték** hello legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="fe2de-148">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   1. <span data-ttu-id="fe2de-149">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="fe2de-149">Click **Save.**</span></span> <span data-ttu-id="fe2de-150">Új attribútum hello hello táblázatban láthatja.</span><span class="sxs-lookup"><span data-stu-id="fe2de-150">You see hello new attribute in hello table.</span></span>

13. <span data-ttu-id="fe2de-151">Kattintson a **konfigurálása &lt;alkalmazásnév&gt;**  tooaccess dokumentáció tooconfigure egyszeri bejelentkezés hello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="fe2de-151">click **Configure &lt;application name&gt;** tooaccess documentation on how tooconfigure single sign-on in hello application.</span></span> <span data-ttu-id="fe2de-152">Is hogy rendelkezik hello metadata URL-címek és a szükséges tanúsítványok toosetup SSO hello alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="fe2de-152">Also, you has hello metadata URLs and certificate required toosetup SSO with hello application.</span></span>

14. <span data-ttu-id="fe2de-153">Kattintson a **mentése** toosave hello konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="fe2de-153">Click **Save** toosave hello configuration.</span></span>

15. <span data-ttu-id="fe2de-154">Felhasználók hozzárendelése toohello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="fe2de-154">Assign users toohello application.</span></span>

## <a name="select-user-identifier-and-add-user-attributes-toobe-sent-toohello-application"></a><span data-ttu-id="fe2de-155">Válassza ki a felhasználói azonosítót és a felhasználói attribútumok küldött toobe toohello alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fe2de-155">Select User Identifier and add user attributes toobe sent toohello application</span></span>

<span data-ttu-id="fe2de-156">tooselect hello felhasználói azonosítót, vagy adja hozzá a felhasználói attribútumok, hajtsa végre az alábbi lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="fe2de-156">tooselect hello User Identifier or add user attributes, follow hello steps below:</span></span>

1.  <span data-ttu-id="fe2de-157">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="fe2de-157">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="fe2de-158">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="fe2de-158">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="fe2de-159">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="fe2de-159">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="fe2de-160">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="fe2de-160">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="fe2de-161">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="fe2de-161">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="fe2de-162">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="fe2de-162">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="fe2de-163">Válassza ki az egyszeri bejelentkezés konfigurált hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="fe2de-163">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="fe2de-164">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="fe2de-164">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="fe2de-165">A hello **felhasználói attribútumok** szakaszban jelölje be a felhasználók a hello egyedi azonosítója hello **felhasználói azonosító** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="fe2de-165">Under hello **User attributes** section, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span> <span data-ttu-id="fe2de-166">hello kiválasztott beállítás toomatch hello várt értéket kell hello alkalmazás tooauthenticate hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="fe2de-166">hello selected option needs toomatch hello expected value in hello application tooauthenticate hello user.</span></span>

  >[!NOTE] 
  ><span data-ttu-id="fe2de-167">Az Azure AD válassza hello formátumban hello NameID attribútum (felhasználói azonosító) kijelölt hello érték alapján, vagy a SAML AuthRequest hello hello alkalmazás által kért formátum hello.</span><span class="sxs-lookup"><span data-stu-id="fe2de-167">Azure AD select hello format for hello NameID attribute (User Identifier) based on hello value selected or hello format requested by hello application in hello SAML AuthRequest.</span></span> <span data-ttu-id="fe2de-168">További információt a Microsoft hello cikk [egyszeri bejelentkezés SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) az hello rész NameIDPolicy.</span><span class="sxs-lookup"><span data-stu-id="fe2de-168">For more information visit hello article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello section NameIDPolicy.</span></span>
  >
  >

9.  <span data-ttu-id="fe2de-169">tooadd felhasználói attribútumok, kattintson **megtekintése és szerkesztése a más felhasználói attribútumok** tooedit hello attribútumok küldött toobe toohello alkalmazás hello SAML-jogkivonat felhasználói bejelentkezéskor.</span><span class="sxs-lookup"><span data-stu-id="fe2de-169">tooadd user attributes, click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="fe2de-170">tooadd attribútum:</span><span class="sxs-lookup"><span data-stu-id="fe2de-170">tooadd an attribute:</span></span>
  
   1. <span data-ttu-id="fe2de-171">Kattintson a **Hozzáadás attribútum**.</span><span class="sxs-lookup"><span data-stu-id="fe2de-171">click **Add attribute**.</span></span> <span data-ttu-id="fe2de-172">Adja meg a hello **neve** és hello válassza hello **érték** hello legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="fe2de-172">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   2. <span data-ttu-id="fe2de-173">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="fe2de-173">Click **Save**.</span></span> <span data-ttu-id="fe2de-174">Új attribútum hello hello táblázatban láthatja.</span><span class="sxs-lookup"><span data-stu-id="fe2de-174">You see hello new attribute in hello table.</span></span>

## <a name="download-hello-azure-ad-metadata-or-certificate"></a><span data-ttu-id="fe2de-175">Az Azure AD-metaadatok hello vagy a tanúsítvány letöltése</span><span class="sxs-lookup"><span data-stu-id="fe2de-175">Download hello Azure AD metadata or certificate</span></span>

<span data-ttu-id="fe2de-176">toodownload hello alkalmazás metaadatainak vagy Azure ad-, tanúsítvány kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="fe2de-176">toodownload hello application metadata or certificate from Azure AD, follow hello steps below:</span></span>

1.  <span data-ttu-id="fe2de-177">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="fe2de-177">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="fe2de-178">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="fe2de-178">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="fe2de-179">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="fe2de-179">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="fe2de-180">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="fe2de-180">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="fe2de-181">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="fe2de-181">click **All Applications** tooview a list of all your applications.</span></span>

  *  <span data-ttu-id="fe2de-182">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="fe2de-182">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications**.</span></span>

6.  <span data-ttu-id="fe2de-183">Válassza ki az egyszeri bejelentkezés konfigurált hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="fe2de-183">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="fe2de-184">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="fe2de-184">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="fe2de-185">Nyissa meg túl**SAML-aláíró tanúsítványa** területen, majd kattintson a **letöltése** oszlop értékét.</span><span class="sxs-lookup"><span data-stu-id="fe2de-185">Go too**SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="fe2de-186">Attól függően, hogy milyen hello alkalmazásnak szüksége van, az egyszeri bejelentkezés konfigurálása lásd: vagy hello beállítás toodownload hello metaadatainak XML-kódja vagy hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="fe2de-186">Depending on what hello application requires configuring single sign-on, you see either hello option toodownload hello Metadata XML or hello Certificate.</span></span>

<span data-ttu-id="fe2de-187">Az Azure AD egy URL-cím tooget hello metaadatok nem biztosít.</span><span class="sxs-lookup"><span data-stu-id="fe2de-187">Azure AD doesn’t provide a URL tooget hello metadata.</span></span> <span data-ttu-id="fe2de-188">hello metaadatok XML-fájlként csak olvasható.</span><span class="sxs-lookup"><span data-stu-id="fe2de-188">hello metadata can only be retrieved as a XML file.</span></span>

## <a name="assign-users-toohello-application"></a><span data-ttu-id="fe2de-189">Felhasználók hozzárendelése toohello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="fe2de-189">Assign users toohello application</span></span>

<span data-ttu-id="fe2de-190">tooassign felhasználók tooan egy vagy több alkalmazás közvetlenül, kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="fe2de-190">tooassign one or more users tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="fe2de-191">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="fe2de-191">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="fe2de-192">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="fe2de-192">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="fe2de-193">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="fe2de-193">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="fe2de-194">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="fe2de-194">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="fe2de-195">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="fe2de-195">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="fe2de-196">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="fe2de-196">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="fe2de-197">Válassza ki a kívánt felhasználó toofrom hello listáját tooassign hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="fe2de-197">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="fe2de-198">Amikor hello alkalmazás betölt, kattintson a **felhasználók és csoportok** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="fe2de-198">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="fe2de-199">Hello kattintson **Hozzáadás** gomb felett hello **felhasználók és csoportok** lista tooopen hello **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="fe2de-199">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="fe2de-200">hello kattintson **felhasználók és csoportok** hello a választó **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="fe2de-200">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="fe2de-201">Hello típusának **teljes név** vagy **e-mail cím** érdekli hello való hozzárendelése hello felhasználó **Keresés név vagy e-mail cím alapján** keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="fe2de-201">Type in hello **full name** or **email address** of hello user you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="fe2de-202">Hello rámutat **felhasználói** a hello lista tooreveal egy **jelölőnégyzet**.</span><span class="sxs-lookup"><span data-stu-id="fe2de-202">Hover over hello **user** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="fe2de-203">Kattintson a hello jelölőnégyzet következő toohello felhasználó profil fénykép vagy embléma tooadd a felhasználó toohello **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="fe2de-203">Click hello checkbox next toohello user’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="fe2de-204">**Választható lehetőség:** Ha túl szeretné**egynél több felhasználó hozzáadása**, egy másik típus **teljes név** vagy **e-mail cím** be hello **Keresés név e-mail cím vagy** keresési mezőbe, majd kattintson a hello jelölőnégyzet tooadd a felhasználó toohello **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="fe2de-204">**Optional:** If you would like too**add more than one user**, type in another **full name** or **email address** into hello **Search by name or email address** search box, and click hello checkbox tooadd this user toohello **Selected** list.</span></span>

13. <span data-ttu-id="fe2de-205">Ha elkészült, válassza a felhasználók, kattintson a hello **válasszon** gomb tooadd őket felhasználók és csoportok toobe toohello listája toohello alkalmazás hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="fe2de-205">When you are finished selecting users, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="fe2de-206">**Nem kötelező:** hello kattintson **Szerepkörválasztás** hello a választó **hozzáadása hozzárendelés** panel tooselect szerepkör tooassign toohello felhasználók kijelölt.</span><span class="sxs-lookup"><span data-stu-id="fe2de-206">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello users you have selected.</span></span>

15. <span data-ttu-id="fe2de-207">Kattintson a hello **hozzárendelése** gomb tooassign hello alkalmazás toohello kijelölt felhasználók.</span><span class="sxs-lookup"><span data-stu-id="fe2de-207">Click hello **Assign** button tooassign hello application toohello selected users.</span></span>

<span data-ttu-id="fe2de-208">Rövid időn belül a kijelölt hello felhasználók kell ezeket az alkalmazásokat használó hello hello megoldás leírása szakaszban ismertetett módszerekkel tudja toolaunch.</span><span class="sxs-lookup"><span data-stu-id="fe2de-208">After a short period of time, hello users you have selected be able toolaunch these applications using hello methods described in hello solution description section.</span></span>

## <a name="customizing-hello-saml-claims-sent-tooan-application"></a><span data-ttu-id="fe2de-209">Hello SAML jogcímek testreszabása küldött tooan alkalmazás</span><span class="sxs-lookup"><span data-stu-id="fe2de-209">Customizing hello SAML claims sent tooan application</span></span>

<span data-ttu-id="fe2de-210">toolearn hogyan toocustomize hello SAML attribútum típusú jogcímek küldött tooyour alkalmazás, lásd: [hozzárendelése az Azure Active Directory-jogcímek](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) további információt.</span><span class="sxs-lookup"><span data-stu-id="fe2de-210">toolearn how toocustomize hello SAML attribute claims sent tooyour application, see [Claims mapping in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fe2de-211">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fe2de-211">Next steps</span></span>
[<span data-ttu-id="fe2de-212">Adja meg az egyszeri bejelentkezés tooyour alkalmazások alkalmazásproxyval</span><span class="sxs-lookup"><span data-stu-id="fe2de-212">Provide single sign-on tooyour apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)



