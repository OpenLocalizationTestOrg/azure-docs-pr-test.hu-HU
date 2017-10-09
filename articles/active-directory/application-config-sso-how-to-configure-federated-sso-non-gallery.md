---
title: "egyszeri bejelentkezés egy nem galéria alkalmazáshoz összevont aaaHow tooconfigure |} Microsoft Docs"
description: "Hogyan tooconfigure összevont egyszeri bejelentkezéshez, amelyet az Azure AD-val toointegrate egyéni nem galéria alkalmazáshoz"
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
ms.openlocfilehash: f4a37cb500c075d0ce917ad8f13411f2ce208c56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-federated-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="cd5a0-103">Hogyan tooconfigure összevont egyszeri bejelentkezés egy nem galéria alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="cd5a0-103">How tooconfigure federated single sign-on for a non-gallery application</span></span>

<span data-ttu-id="cd5a0-104">toohave az Azure AD premium szüksége tooconfigure nem galéria-alkalmazás, és hello alkalmazás támogatja az SAML 2.0-s.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-104">tooconfigure a non-gallery application, you need toohave Azure AD premium and hello application supports SAML 2.0.</span></span> <span data-ttu-id="cd5a0-105">Az Azure AD-verziókkal kapcsolatos további információkért látogasson el a [az Azure AD árképzési](https://azure.microsoft.com/pricing/details/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="cd5a0-105">For more information about Azure AD versions, visit [Azure AD pricing](https://azure.microsoft.com/pricing/details/active-directory/).</span></span>

## <a name="overview-of-steps-required"></a><span data-ttu-id="cd5a0-106">A szükséges lépések – áttekintés</span><span class="sxs-lookup"><span data-stu-id="cd5a0-106">Overview of steps required</span></span>
<span data-ttu-id="cd5a0-107">Az alábbiakban a hello lépéseket szükséges tooconfigure összevont egyszeri bejelentkezés nem galéria magas szintű áttekintését van (például egyéni) alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-107">Below is a high level overview of hello steps required tooconfigure federated single sign-on for a non-gallery (e.g., custom) application.</span></span>

-   [<span data-ttu-id="cd5a0-108">Hello alkalmazás metaadatainak értékek konfigurálása az Azure AD (bejelentkezési URL-cím, az azonosítója, válasz URL-címe)</span><span class="sxs-lookup"><span data-stu-id="cd5a0-108">Configure hello application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>](#_Configuring_single_sign-on)

-   [<span data-ttu-id="cd5a0-109">Válassza ki a felhasználói azonosítót és a felhasználói attribútumok küldött toobe toohello alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="cd5a0-109">Select User Identifier and add user attributes toobe sent toohello application</span></span>](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [<span data-ttu-id="cd5a0-110">Az Azure AD-metaadatok és a tanúsítvány lekérése</span><span class="sxs-lookup"><span data-stu-id="cd5a0-110">Retrieve Azure AD metadata and certificate</span></span>](#download-the-azure-ad-metadata-or-certificate)

-   [<span data-ttu-id="cd5a0-111">Konfigurálja az Azure AD metaadatok értékeket hello alkalmazásban (bejelentkezési URL-címet, a kibocsátó, a kijelentkezési URL-cím és a tanúsítvány)</span><span class="sxs-lookup"><span data-stu-id="cd5a0-111">Configure Azure AD metadata values in hello application (Sign on URL, Issuer, Logout URL and certificate)</span></span>](#_Configuring_single_sign-on)

-   [<span data-ttu-id="cd5a0-112">Felhasználók hozzárendelése toohello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="cd5a0-112">Assign users toohello application</span></span>](#_Assign_users_to_the_application)

## <a name="configuring-single-sign-on-toonon-gallery-applications"></a><span data-ttu-id="cd5a0-113">Egyszeri bejelentkezés toonon-gyűjtemény alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="cd5a0-113">Configuring single sign-on toonon-gallery applications</span></span>

<span data-ttu-id="cd5a0-114">tooconfigure egyszeri bejelentkezés egy alkalmazás, amely nincs az Azure AD-dokumentumtárban hello, kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="cd5a0-114">tooconfigure single sign-on for an application that is not in hello Azure AD gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="cd5a0-115">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="cd5a0-115">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="cd5a0-116">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-116">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="cd5a0-117">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-117">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="cd5a0-118">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-118">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="cd5a0-119">Kattintson a hello **hozzáadása** hello jobb felső sarokban lévő hello a gomb **vállalati alkalmazások** panelen.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-119">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="cd5a0-120">Kattintson a **nem-gyűjtemény alkalmazás** a hello **saját alkalmazás felvétele** szakasz</span><span class="sxs-lookup"><span data-stu-id="cd5a0-120">click **Non-gallery application** in hello **Add your own app** section</span></span>

7.  <span data-ttu-id="cd5a0-121">Adja meg hello hello alkalmazás hello **neve** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-121">Enter hello name of hello application in hello **Name** textbox.</span></span>

8.  <span data-ttu-id="cd5a0-122">Kattintson a **Hozzáadás** tooadd hello alkalmazás gomb.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-122">Click **Add** button, tooadd hello application.</span></span>

9.  <span data-ttu-id="cd5a0-123">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-123">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

10. <span data-ttu-id="cd5a0-124">Válassza ki **SAML-alapú bejelentkezés** a hello **mód** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-124">Select **SAML-based Sign-on** in hello **Mode** dropdown.</span></span>

11. <span data-ttu-id="cd5a0-125">Adja meg a szükséges hello értékeket **tartomány és az URL-címeket.**</span><span class="sxs-lookup"><span data-stu-id="cd5a0-125">Enter hello required values in **Domain and URLs.**</span></span> <span data-ttu-id="cd5a0-126">Ezeket az értékeket kell beszerezni hello alkalmazás gyártójának segítségét.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-126">You should get these values from hello application vendor.</span></span>

   1. <span data-ttu-id="cd5a0-127">tooconfigure hello alkalmazásra, amely a kiállító terjesztési hely által kezdeményezett egyszeri Bejelentkezést, adja meg a hello válasz URL-CÍMEN és hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-127">tooconfigure hello application as IdP-initiated SSO, enter hello Reply URL and hello Identifier.</span></span>

   2. <span data-ttu-id="cd5a0-128">**Választható lehetőség:** tooconfigure hello alkalmazásra, amely a Szolgáltató által kezdeményezett egyszeri Bejelentkezést, hello bejelentkezési URL-címen egy szükséges érték.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-128">**Optional:** tooconfigure hello application as SP-initiated SSO, hello Sign on URL it’s a required value.</span></span>

12. <span data-ttu-id="cd5a0-129">A hello **felhasználói attribútumok**, válassza ki a felhasználók a hello egyedi azonosítója hello **felhasználói azonosító** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-129">In hello **User attributes**, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span>

13. <span data-ttu-id="cd5a0-130">**Nem kötelező:** kattintson **megtekintése és szerkesztése a más felhasználói attribútumok** tooedit hello attribútumok küldött toobe toohello alkalmazás hello SAML-jogkivonat felhasználói bejelentkezéskor.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-130">**Optional:** click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="cd5a0-131">tooadd attribútum:</span><span class="sxs-lookup"><span data-stu-id="cd5a0-131">tooadd an attribute:</span></span>

   1. <span data-ttu-id="cd5a0-132">Kattintson a **Hozzáadás attribútum**.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-132">click **Add attribute**.</span></span> <span data-ttu-id="cd5a0-133">Adja meg a hello **neve** és hello válassza hello **érték** hello legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-133">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   2. <span data-ttu-id="cd5a0-134">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="cd5a0-134">Click **Save.**</span></span> <span data-ttu-id="cd5a0-135">Új attribútum hello hello táblázatban láthatja.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-135">You see hello new attribute in hello table.</span></span>

14. <span data-ttu-id="cd5a0-136">Kattintson a **konfigurálása &lt;alkalmazásnév&gt;**  tooaccess dokumentáció tooconfigure egyszeri bejelentkezés hello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-136">click **Configure &lt;application name&gt;** tooaccess documentation on how tooconfigure single sign-on in hello application.</span></span> <span data-ttu-id="cd5a0-137">Is, akkor az Azure AD URL-címek és rendelkezik hello alkalmazásához szükséges tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-137">Also, you has Azure AD URLs and certificate required for hello application.</span></span>

15. [<span data-ttu-id="cd5a0-138">Felhasználók hozzárendelése toohello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-138">Assign users toohello application.</span></span>](#assign-users-to-the-application)

## <a name="select-user-identifier-and-add-user-attributes-toobe-sent-toohello-application"></a><span data-ttu-id="cd5a0-139">Válassza ki a felhasználói azonosítót és a felhasználói attribútumok küldött toobe toohello alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="cd5a0-139">Select User Identifier and add user attributes toobe sent toohello application</span></span>

<span data-ttu-id="cd5a0-140">tooselect hello felhasználói azonosítót, vagy adja hozzá a felhasználói attribútumok, hajtsa végre az alábbi lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="cd5a0-140">tooselect hello User Identifier or add user attributes, follow hello steps below:</span></span>

1.  <span data-ttu-id="cd5a0-141">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="cd5a0-141">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="cd5a0-142">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-142">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="cd5a0-143">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-143">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="cd5a0-144">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-144">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="cd5a0-145">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-145">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="cd5a0-146">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="cd5a0-146">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="cd5a0-147">Válassza ki az egyszeri bejelentkezés konfigurált hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-147">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="cd5a0-148">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-148">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="cd5a0-149">A hello **felhasználói attribútumok** szakaszban jelölje be a felhasználók a hello egyedi azonosítója hello **felhasználói azonosító** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-149">Under hello **User attributes** section, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span> <span data-ttu-id="cd5a0-150">hello kiválasztott beállítás toomatch hello várt értéket kell hello alkalmazás tooauthenticate hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-150">hello selected option needs toomatch hello expected value in hello application tooauthenticate hello user.</span></span>

 ><span data-ttu-id="cd5a0-151">[! Vegye figyelembe} az Azure AD hello formátum hello NameID attribútum (felhasználói azonosító) kijelölt hello érték alapján, vagy a SAML AuthRequest hello hello alkalmazás által kért formátum hello.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-151">[!NOTE} Azure AD select hello format for hello NameID attribute (User Identifier) based on hello value selected or hello format requested by hello application in hello SAML AuthRequest.</span></span> <span data-ttu-id="cd5a0-152">További információt a Microsoft hello cikk [egyszeri bejelentkezés SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) az hello rész NameIDPolicy.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-152">For more information visit hello article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello section NameIDPolicy.</span></span>
 >
 >

9.  <span data-ttu-id="cd5a0-153">tooadd felhasználói attribútumok, kattintson **megtekintése és szerkesztése a más felhasználói attribútumok** tooedit hello attribútumok küldött toobe toohello alkalmazás hello SAML-jogkivonat felhasználói bejelentkezéskor.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-153">tooadd user attributes, click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="cd5a0-154">tooadd attribútum:</span><span class="sxs-lookup"><span data-stu-id="cd5a0-154">tooadd an attribute:</span></span>

   1. <span data-ttu-id="cd5a0-155">Kattintson a **Hozzáadás attribútum**.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-155">click **Add attribute**.</span></span> <span data-ttu-id="cd5a0-156">Adja meg a hello **neve** és hello válassza hello **érték** hello legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-156">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   2. <span data-ttu-id="cd5a0-157">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="cd5a0-157">Click **Save.**</span></span> <span data-ttu-id="cd5a0-158">Új attribútum hello hello táblázatban láthatja.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-158">You see hello new attribute in hello table.</span></span>

## <a name="download-hello-azure-ad-metadata-or-certificate"></a><span data-ttu-id="cd5a0-159">Az Azure AD-metaadatok hello vagy a tanúsítvány letöltése</span><span class="sxs-lookup"><span data-stu-id="cd5a0-159">Download hello Azure AD metadata or certificate</span></span>

<span data-ttu-id="cd5a0-160">toodownload hello alkalmazás metaadatainak vagy Azure ad-, tanúsítvány kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="cd5a0-160">toodownload hello application metadata or certificate from Azure AD, follow hello steps below:</span></span>

1.  <span data-ttu-id="cd5a0-161">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="cd5a0-161">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="cd5a0-162">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-162">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="cd5a0-163">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-163">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="cd5a0-164">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-164">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="cd5a0-165">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-165">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="cd5a0-166">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="cd5a0-166">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="cd5a0-167">Válassza ki az egyszeri bejelentkezés konfigurált hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-167">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="cd5a0-168">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-168">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="cd5a0-169">Nyissa meg túl**SAML-aláíró tanúsítványa** területen, majd kattintson a **letöltése** oszlop értékét.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-169">Go too**SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="cd5a0-170">Attól függően, hogy milyen hello alkalmazásnak szüksége van, az egyszeri bejelentkezés konfigurálása lásd: vagy hello beállítás toodownload hello metaadatainak XML-kódja vagy hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-170">Depending on what hello application requires configuring single sign-on, you see either hello option toodownload hello Metadata XML or hello Certificate.</span></span>

<span data-ttu-id="cd5a0-171">Az Azure AD egy URL-cím tooget hello metaadatok nem biztosít.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-171">Azure AD doesn’t provide a URL tooget hello metadata.</span></span> <span data-ttu-id="cd5a0-172">hello metaadatok XML-fájlként csak olvasható.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-172">hello metadata can only be retrieved as a XML file.</span></span>

## <a name="assign-users-toohello-application"></a><span data-ttu-id="cd5a0-173">Felhasználók hozzárendelése toohello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="cd5a0-173">Assign users toohello application</span></span>

<span data-ttu-id="cd5a0-174">tooassign felhasználók tooan egy vagy több alkalmazás közvetlenül, kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="cd5a0-174">tooassign one or more users tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="cd5a0-175">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="cd5a0-175">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="cd5a0-176">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-176">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="cd5a0-177">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-177">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="cd5a0-178">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-178">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="cd5a0-179">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-179">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="cd5a0-180">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="cd5a0-180">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="cd5a0-181">Válassza ki a kívánt felhasználó toofrom hello listáját tooassign hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-181">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="cd5a0-182">Amikor hello alkalmazás betölt, kattintson a **felhasználók és csoportok** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-182">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="cd5a0-183">Hello kattintson **Hozzáadás** gomb felett hello **felhasználók és csoportok** lista tooopen hello **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-183">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="cd5a0-184">hello kattintson **felhasználók és csoportok** hello a választó **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-184">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="cd5a0-185">Hello típusának **teljes név** vagy **e-mail cím** érdekli hello való hozzárendelése hello felhasználó **Keresés név vagy e-mail cím alapján** keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-185">Type in hello **full name** or **email address** of hello user you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="cd5a0-186">Hello rámutat **felhasználói** a hello lista tooreveal egy **jelölőnégyzet**.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-186">Hover over hello **user** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="cd5a0-187">Kattintson a hello jelölőnégyzet következő toohello felhasználó profil fénykép vagy embléma tooadd a felhasználó toohello **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-187">Click hello checkbox next toohello user’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="cd5a0-188">**Választható lehetőség:** Ha túl szeretné**egynél több felhasználó hozzáadása**, egy másik típus **teljes név** vagy **e-mail cím** be hello **Keresés név e-mail cím vagy** keresési mezőbe, majd kattintson a hello jelölőnégyzet tooadd a felhasználó toohello **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-188">**Optional:** If you would like too**add more than one user**, type in another **full name** or **email address** into hello **Search by name or email address** search box, and click hello checkbox tooadd this user toohello **Selected** list.</span></span>

13. <span data-ttu-id="cd5a0-189">Ha elkészült, válassza a felhasználók, kattintson a hello **válasszon** gomb tooadd őket felhasználók és csoportok toobe toohello listája toohello alkalmazás hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-189">When you are finished selecting users, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="cd5a0-190">**Nem kötelező:** hello kattintson **Szerepkörválasztás** hello a választó **hozzáadása hozzárendelés** panel tooselect szerepkör tooassign toohello felhasználók kijelölt.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-190">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello users you have selected.</span></span>

15. <span data-ttu-id="cd5a0-191">Kattintson a hello **hozzárendelése** gomb tooassign hello alkalmazás toohello kijelölt felhasználók.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-191">Click hello **Assign** button tooassign hello application toohello selected users.</span></span>

<span data-ttu-id="cd5a0-192">Rövid időn belül a kijelölt hello felhasználók kell ezeket az alkalmazásokat használó hello hello megoldás leírása szakaszban ismertetett módszerekkel tudja toolaunch.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-192">After a short period of time, hello users you have selected be able toolaunch these applications using hello methods described in hello solution description section.</span></span>

## <a name="customizing-hello-saml-claims-sent-tooan-application"></a><span data-ttu-id="cd5a0-193">Hello SAML jogcímek testreszabása küldött tooan alkalmazás</span><span class="sxs-lookup"><span data-stu-id="cd5a0-193">Customizing hello SAML claims sent tooan application</span></span>

<span data-ttu-id="cd5a0-194">toolearn hogyan toocustomize hello SAML attribútum típusú jogcímek küldött tooyour alkalmazás, lásd: [hozzárendelése az Azure Active Directory-jogcímek](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) további információt.</span><span class="sxs-lookup"><span data-stu-id="cd5a0-194">toolearn how toocustomize hello SAML attribute claims sent tooyour application, see [Claims mapping in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cd5a0-195">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cd5a0-195">Next steps</span></span>
[<span data-ttu-id="cd5a0-196">Adja meg az egyszeri bejelentkezés tooyour alkalmazások alkalmazásproxyval</span><span class="sxs-lookup"><span data-stu-id="cd5a0-196">Provide single sign-on tooyour apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)
