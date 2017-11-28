---
title: "hozzárendelt aaaAn alkalmazás nem jelenik meg a hozzáférési panel hello |} Microsoft Docs"
description: "Ezért az alkalmazás nem jelenik meg a hozzáférési Panel hello hibaelhárítása"
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
ms.reviwer: japere
ms.openlocfilehash: 089883f406267df4552c7fc991883f335ad49fd5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="an-assigned-application-is-not-appearing-on-hello-access-panel"></a><span data-ttu-id="67ceb-103">Kijelölt alkalmazást nem szereplő hello hozzáférési panel</span><span class="sxs-lookup"><span data-stu-id="67ceb-103">An assigned application is not appearing on hello access panel</span></span>

<span data-ttu-id="67ceb-104">hello hozzáférési Panel egy webes portál, amely lehetővé teszi a felhasználó munkahelyi vagy iskolai fiókkal az Azure Active Directory (Azure AD) tooview és kezdő felhőalapú alkalmazások, hogy hello Azure AD-rendszergazda rendelkezik hozzáféréssel őket.</span><span class="sxs-lookup"><span data-stu-id="67ceb-104">hello Access Panel is a web-based portal which enables a user with a work or school account in Azure Active Directory (Azure AD) tooview and start cloud-based applications that hello Azure AD administrator has granted them access to.</span></span> <span data-ttu-id="67ceb-105">Ezeket az alkalmazásokat úgy vannak konfigurálva, hello Azure AD portálon hello felhasználó nevében.</span><span class="sxs-lookup"><span data-stu-id="67ceb-105">These applications are configured on behalf of hello user in hello Azure AD portal.</span></span> <span data-ttu-id="67ceb-106">hello alkalmazás megfelelően kell konfigurálni, és a hozzárendelt toohello felhasználó vagy csoport hello felhasználó tagja a hozzáférési Panel hello toosee hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="67ceb-106">hello application must be configured properly and assigned toohello user or a group hello user is a member of toosee hello application in hello Access Panel.</span></span>

<span data-ttu-id="67ceb-107">a következő kategóriák hello esik azért jelent meg a felhasználó alkalmazások hello típusa:</span><span class="sxs-lookup"><span data-stu-id="67ceb-107">hello type of apps a user may be seeing fall in hello following categories:</span></span>

-   <span data-ttu-id="67ceb-108">Office 365-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="67ceb-108">Office 365 Applications</span></span>

-   <span data-ttu-id="67ceb-109">Összevonási-alapú egyszeri bejelentkezési modellel konfigurált a Microsoft és külső alkalmazások</span><span class="sxs-lookup"><span data-stu-id="67ceb-109">Microsoft and third-party applications configured with federation-based SSO</span></span>

-   <span data-ttu-id="67ceb-110">Jelszó-alapú egyszeri bejelentkezés alkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="67ceb-110">Password-based SSO applications</span></span>

-   <span data-ttu-id="67ceb-111">A már meglévő SSO megoldásaival alkalmazások</span><span class="sxs-lookup"><span data-stu-id="67ceb-111">Applications with existing SSO solutions</span></span>

## <a name="general-issues-toocheck-first"></a><span data-ttu-id="67ceb-112">Általános problémák első toocheck</span><span class="sxs-lookup"><span data-stu-id="67ceb-112">General issues toocheck first</span></span>

-   <span data-ttu-id="67ceb-113">Ha egy alkalmazás előbbiekben felvett tooa felhasználói, próbálja toosign bejövő és kimenő adatforgalma újra hello felhasználói hozzáférési panelre után néhány perc toosee Ha hello adja hozzá.</span><span class="sxs-lookup"><span data-stu-id="67ceb-113">If an application was just added tooa user, try toosign in and out again into hello user’s Access Panel after a few minutes toosee if hello application is added.</span></span>

-   <span data-ttu-id="67ceb-114">A licenc csak el lett távolítva a felhasználó vagy csoport hello felhasználó esetén tagja ennek attól függően, hogy hello méretét és összetettségét hello csoportot a végzett módosítások toobe hosszú ideig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="67ceb-114">If a license was just removed from a user or group hello user is a member of this may take a long time, depending on hello size and complexity of hello group for changes toobe made.</span></span> <span data-ttu-id="67ceb-115">Lehetővé teszi további alkalommal jelentkezik be a hozzáférési Panel hello előtt.</span><span class="sxs-lookup"><span data-stu-id="67ceb-115">Allow for extra time before signing into hello Access Panel.</span></span>

## <a name="problems-related-tooapplication-configuration"></a><span data-ttu-id="67ceb-116">Kapcsolódó tooapplication konfigurációs problémák</span><span class="sxs-lookup"><span data-stu-id="67ceb-116">Problems related tooapplication configuration</span></span>

<span data-ttu-id="67ceb-117">Egy alkalmazás nem lehetséges, hogy megjelenjen a a felhasználó hozzáférési Panel, mert nincs megfelelően konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="67ceb-117">An application may not be appearing in a user’s Access Panel because it is not configured properly.</span></span> <span data-ttu-id="67ceb-118">Az alábbiakban néhány módszert tooapplication kapcsolódó konfigurációs problémák hibaelhárítását hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="67ceb-118">Below are some ways you can troubleshoot issues related tooapplication configuration:</span></span>

-   [<span data-ttu-id="67ceb-119">Hogyan tooconfigure összevont egyszeri bejelentkezés az Azure AD gyűjtemény alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="67ceb-119">How tooconfigure federated single sign-on for an Azure AD gallery application</span></span>](#how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application)

-   [<span data-ttu-id="67ceb-120">Hogyan tooconfigure összevont egyszeri bejelentkezés egy nem galéria alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="67ceb-120">How tooconfigure federated single sign-on for a non-gallery application</span></span>](#how-to-configure-federated-single-sign-on-for-a-non-gallery-application)

-   [<span data-ttu-id="67ceb-121">Hogyan tooconfigure jelszó egyszeri bejelentkezést egy Azure AD-gyűjtemény alkalmazást alkalmazás</span><span class="sxs-lookup"><span data-stu-id="67ceb-121">How tooconfigure a password single sign-on application for an Azure AD gallery application</span></span>](#how-to-configure-password-single-sign-on-for-a-non-gallery-application)

-   [<span data-ttu-id="67ceb-122">Hogyan tooconfigure jelszó egyszeri bejelentkezés alkalmazás nem galéria alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="67ceb-122">How tooconfigure a password single sign-on application for a non-gallery application</span></span>](#how-to-configure-password-single-sign-on-for-a-non-gallery-application)

### <a name="how-tooconfigure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="67ceb-123">Hogyan tooconfigure összevont egyszeri bejelentkezés az Azure AD gyűjtemény alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="67ceb-123">How tooconfigure federated single sign-on for an Azure AD gallery application</span></span>

<span data-ttu-id="67ceb-124">Összes alkalmazás hello Azure AD-dokumentumtárban vállalati egyszeri bejelentkezésre alkalmas engedélyezve van elérhető részletes oktatóanyaga.</span><span class="sxs-lookup"><span data-stu-id="67ceb-124">All applications in hello Azure AD gallery enabled with Enterprise Single Sign-On capability has a step-by-step tutorial available.</span></span> <span data-ttu-id="67ceb-125">Van-e hozzáférési hello [kapcsolatos bemutatók felsorolása toointegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) kapcsolatos részletek részletes útmutatás.</span><span class="sxs-lookup"><span data-stu-id="67ceb-125">You can access hello [List of tutorials on how toointegrate SaaS apps with Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) for a detail step-by-step guidance.</span></span>

<span data-ttu-id="67ceb-126">egy alkalmazás hello Azure AD-galériából tooconfigure kell:</span><span class="sxs-lookup"><span data-stu-id="67ceb-126">tooconfigure an application from hello Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="67ceb-127">Alkalmazás hozzáadása hello Azure AD-galériából</span><span class="sxs-lookup"><span data-stu-id="67ceb-127">Add an application from hello Azure AD gallery</span></span>](#add-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="67ceb-128">Hello alkalmazás metaadatainak értékek konfigurálása az Azure AD (bejelentkezési URL-cím, az azonosítója, válasz URL-címe)</span><span class="sxs-lookup"><span data-stu-id="67ceb-128">Configure hello application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="67ceb-129">Válassza ki a felhasználói azonosítót és a felhasználói attribútumok küldött toobe toohello alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="67ceb-129">Select User Identifier and add user attributes toobe sent toohello application</span></span>](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [<span data-ttu-id="67ceb-130">Az Azure AD-metaadatok és a tanúsítvány lekérése</span><span class="sxs-lookup"><span data-stu-id="67ceb-130">Retrieve Azure AD metadata and certificate</span></span>](#download-the-azure-ad-metadata-or-certificate)

-   [<span data-ttu-id="67ceb-131">Konfigurálja az Azure AD metaadatok értékeket hello alkalmazásban (bejelentkezési URL-címet, a kibocsátó, a kijelentkezési URL-cím és a tanúsítvány)</span><span class="sxs-lookup"><span data-stu-id="67ceb-131">Configure Azure AD metadata values in hello application (Sign on URL, Issuer, Logout URL and certificate)</span></span>](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

#### <a name="add-an-application-from-hello-azure-ad-gallery"></a><span data-ttu-id="67ceb-132">Alkalmazás hozzáadása hello Azure AD-galériából</span><span class="sxs-lookup"><span data-stu-id="67ceb-132">Add an application from hello Azure AD gallery</span></span>

<span data-ttu-id="67ceb-133">az alkalmazás a hello Azure AD-katalógusában, tooadd kövesse hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="67ceb-133">tooadd an application from hello Azure AD Gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="67ceb-134">Nyissa meg hello [Azure Portal](https://portal.azure.com) , és jelentkezzen be a egy **globális rendszergazda** vagy **társadminisztrátornak**</span><span class="sxs-lookup"><span data-stu-id="67ceb-134">Open hello [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="67ceb-135">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="67ceb-135">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="67ceb-136">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="67ceb-136">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="67ceb-137">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="67ceb-137">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="67ceb-138">Kattintson a hello **hozzáadása** hello jobb felső sarokban lévő hello a gomb **vállalati alkalmazások** panelen.</span><span class="sxs-lookup"><span data-stu-id="67ceb-138">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="67ceb-139">A hello **adjon meg egy nevet** hello a szövegmező **hello gyűjteményből Hozzáadás** szakaszban, hello alkalmazás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="67ceb-139">In hello **Enter a name** textbox from hello **Add from hello gallery** section, type hello name of hello application.</span></span>

7.  <span data-ttu-id="67ceb-140">Válassza ki a használni kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="67ceb-140">Select hello application you want tooconfigure for single sign-on.</span></span>

8.  <span data-ttu-id="67ceb-141">Hello alkalmazás hozzáadása előtt módosíthatja annak nevét a hello **neve** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="67ceb-141">Before adding hello application, you can change its name from hello **Name** textbox.</span></span>

9.  <span data-ttu-id="67ceb-142">Kattintson a **Hozzáadás** tooadd hello alkalmazás gomb.</span><span class="sxs-lookup"><span data-stu-id="67ceb-142">Click **Add** button, tooadd hello application.</span></span>

<span data-ttu-id="67ceb-143">Egy rövid időszak után kell képes toosee hello alkalmazás konfigurációs panelen.</span><span class="sxs-lookup"><span data-stu-id="67ceb-143">After a short period, you be able toosee hello application’s configuration blade.</span></span>

#### <a name="configure-single-sign-on-for-an-application-from-hello-azure-ad-gallery"></a><span data-ttu-id="67ceb-144">Egyszeri bejelentkezés egy alkalmazás hello Azure AD-galériából konfigurálása</span><span class="sxs-lookup"><span data-stu-id="67ceb-144">Configure single sign-on for an application from hello Azure AD gallery</span></span>

<span data-ttu-id="67ceb-145">tooconfigure egyszeri bejelentkezés egy alkalmazás kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="67ceb-145">tooconfigure single sign-on for an application, follow hello steps below:</span></span>

1.  <span data-ttu-id="67ceb-146">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="67ceb-146">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="67ceb-147">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="67ceb-147">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="67ceb-148">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="67ceb-148">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="67ceb-149">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="67ceb-149">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="67ceb-150">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="67ceb-150">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="67ceb-151">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="67ceb-151">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="67ceb-152">Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="67ceb-152">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="67ceb-153">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="67ceb-153">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="67ceb-154">Válassza ki **SAML-alapú bejelentkezés** a hello **mód** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="67ceb-154">Select **SAML-based Sign-on** from hello **Mode** dropdown.</span></span>

9.  <span data-ttu-id="67ceb-155">Adja meg a szükséges hello értékeket **tartomány és az URL-címeket.**</span><span class="sxs-lookup"><span data-stu-id="67ceb-155">Enter hello required values in **Domain and URLs.**</span></span> <span data-ttu-id="67ceb-156">Ezeket az értékeket kell beszerezni hello alkalmazás gyártójának segítségét.</span><span class="sxs-lookup"><span data-stu-id="67ceb-156">You should get these values from hello application vendor.</span></span>

   1. <span data-ttu-id="67ceb-157">tooconfigure hello alkalmazásra, amely a Szolgáltató által kezdeményezett egyszeri Bejelentkezést, hello bejelentkezési URL-címen egy szükséges érték.</span><span class="sxs-lookup"><span data-stu-id="67ceb-157">tooconfigure hello application as SP-initiated SSO, hello Sign on URL it’s a required value.</span></span> <span data-ttu-id="67ceb-158">Egyes alkalmazások hello azonosító értéke is szükséges.</span><span class="sxs-lookup"><span data-stu-id="67ceb-158">For some applications, hello Identifier is also a required value.</span></span>

   2. <span data-ttu-id="67ceb-159">tooconfigure hello alkalmazásra, amely a kiállító terjesztési hely által kezdeményezett egyszeri Bejelentkezést, hello válasz URL-cím szükséges érték.</span><span class="sxs-lookup"><span data-stu-id="67ceb-159">tooconfigure hello application as IdP-initiated SSO, hello Reply URL it’s a required value.</span></span> <span data-ttu-id="67ceb-160">Egyes alkalmazások hello azonosító értéke is szükséges.</span><span class="sxs-lookup"><span data-stu-id="67ceb-160">For some applications, hello Identifier is also a required value.</span></span>

10. <span data-ttu-id="67ceb-161">**Választható lehetőség:** kattintson **megjelenítése speciális URL-beállításainak** Ha azt szeretné, hogy toosee hello nem szükséges értékeket.</span><span class="sxs-lookup"><span data-stu-id="67ceb-161">**Optional:** click **Show advanced URL settings** if you want toosee hello non-required values.</span></span>

11. <span data-ttu-id="67ceb-162">A hello **felhasználói attribútumok**, válassza ki a felhasználók a hello egyedi azonosítója hello **felhasználói azonosító** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="67ceb-162">In hello **User attributes**, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span>

12. <span data-ttu-id="67ceb-163">**Nem kötelező:** kattintson **megtekintése és szerkesztése a más felhasználói attribútumok** tooedit hello attribútumok küldött toobe toohello alkalmazás hello SAML-jogkivonat felhasználói bejelentkezéskor.</span><span class="sxs-lookup"><span data-stu-id="67ceb-163">**Optional:** click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="67ceb-164">tooadd attribútum:</span><span class="sxs-lookup"><span data-stu-id="67ceb-164">tooadd an attribute:</span></span>

   1. <span data-ttu-id="67ceb-165">Kattintson a **Hozzáadás attribútum**.</span><span class="sxs-lookup"><span data-stu-id="67ceb-165">click **Add attribute**.</span></span> <span data-ttu-id="67ceb-166">Adja meg a hello **neve** és hello válassza hello **érték** hello legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="67ceb-166">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   2. <span data-ttu-id="67ceb-167">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="67ceb-167">click **Save.**</span></span> <span data-ttu-id="67ceb-168">Új attribútum hello hello táblázatban láthatja.</span><span class="sxs-lookup"><span data-stu-id="67ceb-168">You see hello new attribute in hello table.</span></span>

13. <span data-ttu-id="67ceb-169">Kattintson a **konfigurálása &lt;alkalmazásnév&gt;**  tooaccess dokumentáció tooconfigure egyszeri bejelentkezés hello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="67ceb-169">click **Configure &lt;application name&gt;** tooaccess documentation on how tooconfigure single sign-on in hello application.</span></span> <span data-ttu-id="67ceb-170">Is hogy rendelkezik hello metadata URL-címek és a szükséges tanúsítványok toosetup SSO hello alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="67ceb-170">Also, you has hello metadata URLs and certificate required toosetup SSO with hello application.</span></span>

14. <span data-ttu-id="67ceb-171">Kattintson a **mentése** toosave hello konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="67ceb-171">click **Save** toosave hello configuration.</span></span>

15. <span data-ttu-id="67ceb-172">Felhasználók hozzárendelése toohello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="67ceb-172">Assign users toohello application.</span></span>

#### <a name="select-user-identifier-and-add-user-attributes-toobe-sent-toohello-application"></a><span data-ttu-id="67ceb-173">Válassza ki a felhasználói azonosítót és a felhasználói attribútumok küldött toobe toohello alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="67ceb-173">Select User Identifier and add user attributes toobe sent toohello application</span></span>

<span data-ttu-id="67ceb-174">tooselect hello felhasználói azonosítót, vagy adja hozzá a felhasználói attribútumok, hajtsa végre az alábbi lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="67ceb-174">tooselect hello User Identifier or add user attributes, follow hello steps below:</span></span>

1.  <span data-ttu-id="67ceb-175">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="67ceb-175">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="67ceb-176">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="67ceb-176">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="67ceb-177">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="67ceb-177">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="67ceb-178">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="67ceb-178">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="67ceb-179">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="67ceb-179">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="67ceb-180">Ha azt szeretné, hogy itt tooshow hello alkalmazás nem látja, használja a hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="67ceb-180">If you do not see hello application you want tooshow up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="67ceb-181">Válassza ki az egyszeri bejelentkezés konfigurált hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="67ceb-181">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="67ceb-182">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="67ceb-182">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="67ceb-183">A hello **felhasználói attribútumok** szakaszban jelölje be a felhasználók a hello egyedi azonosítója hello **felhasználói azonosító** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="67ceb-183">Under hello **User attributes** section, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span> <span data-ttu-id="67ceb-184">hello kiválasztott beállítás toomatch hello várt értéket kell hello alkalmazás tooauthenticate hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="67ceb-184">hello selected option needs toomatch hello expected value in hello application tooauthenticate hello user.</span></span>

   >[!NOTE] 
   ><span data-ttu-id="67ceb-185">Az Azure AD válassza hello formátumban hello NameID attribútum (felhasználói azonosító) kijelölt hello érték alapján, vagy a SAML AuthRequest hello hello alkalmazás által kért formátum hello.</span><span class="sxs-lookup"><span data-stu-id="67ceb-185">Azure AD select hello format for hello NameID attribute (User Identifier) based on hello value selected or hello format requested by hello application in hello SAML AuthRequest.</span></span> <span data-ttu-id="67ceb-186">További információt a Microsoft hello cikk [egyszeri bejelentkezés SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) az hello rész NameIDPolicy.</span><span class="sxs-lookup"><span data-stu-id="67ceb-186">For more information visit hello article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello section NameIDPolicy.</span></span>
   >
   >

9.  <span data-ttu-id="67ceb-187">tooadd felhasználói attribútumok, kattintson **megtekintése és szerkesztése a más felhasználói attribútumok** tooedit hello attribútumok küldött toobe toohello alkalmazás hello SAML-jogkivonat felhasználói bejelentkezéskor.</span><span class="sxs-lookup"><span data-stu-id="67ceb-187">tooadd user attributes, click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="67ceb-188">tooadd attribútum:</span><span class="sxs-lookup"><span data-stu-id="67ceb-188">tooadd an attribute:</span></span>

   1. <span data-ttu-id="67ceb-189">Kattintson a **Hozzáadás attribútum**.</span><span class="sxs-lookup"><span data-stu-id="67ceb-189">click **Add attribute**.</span></span> <span data-ttu-id="67ceb-190">Adja meg a hello **neve** és hello válassza hello **érték** hello legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="67ceb-190">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   2. <span data-ttu-id="67ceb-191">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="67ceb-191">click **Save.**</span></span> <span data-ttu-id="67ceb-192">Új attribútum hello hello tábla jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="67ceb-192">You will see hello new attribute in hello table.</span></span>

#### <a name="download-hello-azure-ad-metadata-or-certificate"></a><span data-ttu-id="67ceb-193">Az Azure AD-metaadatok hello vagy a tanúsítvány letöltése</span><span class="sxs-lookup"><span data-stu-id="67ceb-193">Download hello Azure AD metadata or certificate</span></span>

<span data-ttu-id="67ceb-194">toodownload hello alkalmazás metaadatainak vagy Azure ad-, tanúsítvány kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="67ceb-194">toodownload hello application metadata or certificate from Azure AD, follow hello steps below:</span></span>

1.  <span data-ttu-id="67ceb-195">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="67ceb-195">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="67ceb-196">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="67ceb-196">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="67ceb-197">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="67ceb-197">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="67ceb-198">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="67ceb-198">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="67ceb-199">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="67ceb-199">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="67ceb-200">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="67ceb-200">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="67ceb-201">Válassza ki az egyszeri bejelentkezés konfigurált hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="67ceb-201">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="67ceb-202">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="67ceb-202">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="67ceb-203">Nyissa meg túl**SAML-aláíró tanúsítványa** területen, majd kattintson a **letöltése** oszlop értékét.</span><span class="sxs-lookup"><span data-stu-id="67ceb-203">Go too**SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="67ceb-204">Attól függően, hogy milyen hello alkalmazásnak szüksége van, az egyszeri bejelentkezés konfigurálása lásd: vagy hello beállítás toodownload hello metaadatainak XML-kódja vagy hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="67ceb-204">Depending on what hello application requires configuring single sign-on, you see either hello option toodownload hello Metadata XML or hello Certificate.</span></span>

    <span data-ttu-id="67ceb-205">Az Azure AD egy URL-cím tooget hello metaadatok nem biztosít.</span><span class="sxs-lookup"><span data-stu-id="67ceb-205">Azure AD doesn’t provide a URL tooget hello metadata.</span></span> <span data-ttu-id="67ceb-206">hello metaadatok XML-fájlként csak olvasható.</span><span class="sxs-lookup"><span data-stu-id="67ceb-206">hello metadata can only be retrieved as a XML file.</span></span>

### <a name="how-tooconfigure-federated-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="67ceb-207">Hogyan tooconfigure összevont egyszeri bejelentkezés egy nem galéria alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="67ceb-207">How tooconfigure federated single sign-on for a non-gallery application</span></span>

<span data-ttu-id="67ceb-208">toohave az Azure AD premium szüksége tooconfigure nem galéria-alkalmazás, és hello alkalmazás támogatja az SAML 2.0-s.</span><span class="sxs-lookup"><span data-stu-id="67ceb-208">tooconfigure a non-gallery application, you need toohave Azure AD premium and hello application supports SAML 2.0.</span></span> <span data-ttu-id="67ceb-209">Az Azure AD-verziókkal kapcsolatos további információkért látogasson el a [az Azure AD árképzési](https://azure.microsoft.com/pricing/details/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="67ceb-209">For more information about Azure AD versions, visit [Azure AD pricing](https://azure.microsoft.com/pricing/details/active-directory/).</span></span>

-   [<span data-ttu-id="67ceb-210">Hello alkalmazás metaadatainak értékek konfigurálása az Azure AD (bejelentkezési URL-cím, az azonosítója, válasz URL-címe)</span><span class="sxs-lookup"><span data-stu-id="67ceb-210">Configure hello application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>](#configuring-single-sign-on)

-   [<span data-ttu-id="67ceb-211">Válassza ki a felhasználói azonosítót és a felhasználói attribútumok küldött toobe toohello alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="67ceb-211">Select User Identifier and add user attributes toobe sent toohello application</span></span>](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [<span data-ttu-id="67ceb-212">Az Azure AD-metaadatok és a tanúsítvány lekérése</span><span class="sxs-lookup"><span data-stu-id="67ceb-212">Retrieve Azure AD metadata and certificate</span></span>](#download-the-azure-ad-metadata-or-certificate)

-   [<span data-ttu-id="67ceb-213">Konfigurálja az Azure AD metaadatok értékeket hello alkalmazásban (bejelentkezési URL-címet, a kibocsátó, a kijelentkezési URL-cím és a tanúsítvány)</span><span class="sxs-lookup"><span data-stu-id="67ceb-213">Configure Azure AD metadata values in hello application (Sign on URL, Issuer, Logout URL and certificate)</span></span>](#configuring-single-sign-on)

#### <a name="configure-hello-applications-metadata-values-in-azure-ad-sign-on-url-identifier-reply-url"></a><span data-ttu-id="67ceb-214">Hello alkalmazás metaadatainak értékek konfigurálása az Azure AD (bejelentkezési URL-cím, az azonosítója, válasz URL-címe)</span><span class="sxs-lookup"><span data-stu-id="67ceb-214">Configure hello application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>

<span data-ttu-id="67ceb-215">tooconfigure egyszeri bejelentkezés egy alkalmazás, amely nincs az Azure AD-dokumentumtárban hello, kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="67ceb-215">tooconfigure single sign-on for an application that is not in hello Azure AD gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="67ceb-216">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="67ceb-216">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="67ceb-217">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="67ceb-217">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="67ceb-218">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="67ceb-218">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="67ceb-219">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="67ceb-219">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="67ceb-220">Kattintson a hello **hozzáadása** hello jobb felső sarokban lévő hello a gomb **vállalati alkalmazások** panelen.</span><span class="sxs-lookup"><span data-stu-id="67ceb-220">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="67ceb-221">Kattintson a **nem-gyűjtemény alkalmazás** a hello **saját alkalmazás felvétele** szakasz.</span><span class="sxs-lookup"><span data-stu-id="67ceb-221">click **Non-gallery application** in hello **Add your own app** section.</span></span>

7.  <span data-ttu-id="67ceb-222">Adja meg hello hello alkalmazás hello **neve** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="67ceb-222">Enter hello name of hello application in hello **Name** textbox.</span></span>

8.  <span data-ttu-id="67ceb-223">Kattintson a **Hozzáadás** tooadd hello alkalmazás gomb.</span><span class="sxs-lookup"><span data-stu-id="67ceb-223">Click **Add** button, tooadd hello application.</span></span>

9.  <span data-ttu-id="67ceb-224">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="67ceb-224">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

10. <span data-ttu-id="67ceb-225">Válassza ki **SAML-alapú bejelentkezés** a hello **mód** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="67ceb-225">Select **SAML-based Sign-on** in hello **Mode** dropdown.</span></span>

11. <span data-ttu-id="67ceb-226">Adja meg a szükséges hello értékeket **tartomány és az URL-címeket.**</span><span class="sxs-lookup"><span data-stu-id="67ceb-226">Enter hello required values in **Domain and URLs.**</span></span> <span data-ttu-id="67ceb-227">Ezeket az értékeket kell beszerezni hello alkalmazás gyártójának segítségét.</span><span class="sxs-lookup"><span data-stu-id="67ceb-227">You should get these values from hello application vendor.</span></span>

   1. <span data-ttu-id="67ceb-228">tooconfigure hello alkalmazásra, amely a kiállító terjesztési hely által kezdeményezett egyszeri Bejelentkezést, adja meg a hello válasz URL-CÍMEN és hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="67ceb-228">tooconfigure hello application as IdP-initiated SSO, enter hello Reply URL and hello Identifier.</span></span>

   2.  <span data-ttu-id="67ceb-229">**Választható lehetőség:** tooconfigure hello alkalmazásra, amely a Szolgáltató által kezdeményezett egyszeri Bejelentkezést, hello bejelentkezési URL-címen egy szükséges érték.</span><span class="sxs-lookup"><span data-stu-id="67ceb-229">**Optional:** tooconfigure hello application as SP-initiated SSO, hello Sign on URL it’s a required value.</span></span>

12. <span data-ttu-id="67ceb-230">A hello **felhasználói attribútumok**, válassza ki a felhasználók a hello egyedi azonosítója hello **felhasználói azonosító** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="67ceb-230">In hello **User attributes**, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span>

13. <span data-ttu-id="67ceb-231">**Nem kötelező:** kattintson **megtekintése és szerkesztése a más felhasználói attribútumok** tooedit hello attribútumok küldött toobe toohello alkalmazás hello SAML-jogkivonat felhasználói bejelentkezéskor.</span><span class="sxs-lookup"><span data-stu-id="67ceb-231">**Optional:** click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="67ceb-232">tooadd attribútum:</span><span class="sxs-lookup"><span data-stu-id="67ceb-232">tooadd an attribute:</span></span>

   1. <span data-ttu-id="67ceb-233">Kattintson a **Hozzáadás attribútum**.</span><span class="sxs-lookup"><span data-stu-id="67ceb-233">click **Add attribute**.</span></span> <span data-ttu-id="67ceb-234">Adja meg a hello **neve** és hello válassza hello **érték** hello legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="67ceb-234">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   2. <span data-ttu-id="67ceb-235">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="67ceb-235">Click **Save.**</span></span> <span data-ttu-id="67ceb-236">Új attribútum hello hello táblázatban láthatja.</span><span class="sxs-lookup"><span data-stu-id="67ceb-236">You see hello new attribute in hello table.</span></span>

14. <span data-ttu-id="67ceb-237">Kattintson a **konfigurálása &lt;alkalmazásnév&gt;**  tooaccess dokumentáció tooconfigure egyszeri bejelentkezés hello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="67ceb-237">click **Configure &lt;application name&gt;** tooaccess documentation on how tooconfigure single sign-on in hello application.</span></span> <span data-ttu-id="67ceb-238">Is, akkor az Azure AD URL-címek és rendelkezik hello alkalmazásához szükséges tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="67ceb-238">Also, you has Azure AD URLs and certificate required for hello application.</span></span>

#### <a name="select-user-identifier-and-add-user-attributes-toobe-sent-toohello-application"></a><span data-ttu-id="67ceb-239">Válassza ki a felhasználói azonosítót és a felhasználói attribútumok küldött toobe toohello alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="67ceb-239">Select User Identifier and add user attributes toobe sent toohello application</span></span>

<span data-ttu-id="67ceb-240">tooselect hello felhasználói azonosítót, vagy adja hozzá a felhasználói attribútumok, hajtsa végre az alábbi lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="67ceb-240">tooselect hello User Identifier or add user attributes, follow hello steps below:</span></span>

1.  <span data-ttu-id="67ceb-241">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="67ceb-241">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="67ceb-242">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="67ceb-242">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="67ceb-243">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="67ceb-243">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="67ceb-244">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="67ceb-244">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="67ceb-245">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="67ceb-245">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="67ceb-246">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="67ceb-246">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="67ceb-247">Válassza ki az egyszeri bejelentkezés konfigurált hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="67ceb-247">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="67ceb-248">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="67ceb-248">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="67ceb-249">A hello **felhasználói attribútumok** szakaszban jelölje be a felhasználók a hello egyedi azonosítója hello **felhasználói azonosító** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="67ceb-249">Under hello **User attributes** section, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span> <span data-ttu-id="67ceb-250">hello kiválasztott beállítás toomatch hello várt értéket kell hello alkalmazás tooauthenticate hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="67ceb-250">hello selected option needs toomatch hello expected value in hello application tooauthenticate hello user.</span></span>

   >[!NOTE] 
   ><span data-ttu-id="67ceb-251">Az Azure AD válassza hello formátumban hello NameID attribútum (felhasználói azonosító) kijelölt hello érték alapján, vagy a SAML AuthRequest hello hello alkalmazás által kért formátum hello.</span><span class="sxs-lookup"><span data-stu-id="67ceb-251">Azure AD select hello format for hello NameID attribute (User Identifier) based on hello value selected or hello format requested by hello application in hello SAML AuthRequest.</span></span> <span data-ttu-id="67ceb-252">További információt a Microsoft hello cikk [egyszeri bejelentkezés SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) az hello rész NameIDPolicy.</span><span class="sxs-lookup"><span data-stu-id="67ceb-252">For more information visit hello article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello section NameIDPolicy.</span></span>
   >
   >

9.  <span data-ttu-id="67ceb-253">tooadd felhasználói attribútumok, kattintson **megtekintése és szerkesztése a más felhasználói attribútumok** tooedit hello attribútumok küldött toobe toohello alkalmazás hello SAML-jogkivonat felhasználói bejelentkezéskor.</span><span class="sxs-lookup"><span data-stu-id="67ceb-253">tooadd user attributes, click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="67ceb-254">tooadd attribútum:</span><span class="sxs-lookup"><span data-stu-id="67ceb-254">tooadd an attribute:</span></span>

   1. <span data-ttu-id="67ceb-255">Kattintson a **Hozzáadás attribútum**.</span><span class="sxs-lookup"><span data-stu-id="67ceb-255">click **Add attribute**.</span></span> <span data-ttu-id="67ceb-256">Adja meg a hello **neve** és hello válassza hello **érték** hello legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="67ceb-256">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   2. <span data-ttu-id="67ceb-257">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="67ceb-257">Click **Save.**</span></span> <span data-ttu-id="67ceb-258">Új attribútum hello hello táblázatban láthatja.</span><span class="sxs-lookup"><span data-stu-id="67ceb-258">You see hello new attribute in hello table.</span></span>

#### <a name="download-hello-azure-ad-metadata-or-certificate"></a><span data-ttu-id="67ceb-259">Az Azure AD-metaadatok hello vagy a tanúsítvány letöltése</span><span class="sxs-lookup"><span data-stu-id="67ceb-259">Download hello Azure AD metadata or certificate</span></span>

<span data-ttu-id="67ceb-260">toodownload hello alkalmazás metaadatainak vagy Azure ad-, tanúsítvány kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="67ceb-260">toodownload hello application metadata or certificate from Azure AD, follow hello steps below:</span></span>

1.  <span data-ttu-id="67ceb-261">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="67ceb-261">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="67ceb-262">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="67ceb-262">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="67ceb-263">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="67ceb-263">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="67ceb-264">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="67ceb-264">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="67ceb-265">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="67ceb-265">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="67ceb-266">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="67ceb-266">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="67ceb-267">Válassza ki az egyszeri bejelentkezés konfigurált hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="67ceb-267">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="67ceb-268">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="67ceb-268">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="67ceb-269">Nyissa meg túl**SAML-aláíró tanúsítványa** területen, majd kattintson a **letöltése** oszlop értékét.</span><span class="sxs-lookup"><span data-stu-id="67ceb-269">Go too**SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="67ceb-270">Attól függően, hogy milyen hello alkalmazásnak szüksége van, az egyszeri bejelentkezés konfigurálása lásd: vagy hello beállítás toodownload hello metaadatainak XML-kódja vagy hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="67ceb-270">Depending on what hello application requires configuring single sign-on, you see either hello option toodownload hello Metadata XML or hello Certificate.</span></span>

<span data-ttu-id="67ceb-271">Az Azure AD egy URL-cím tooget hello metaadatok nem biztosít.</span><span class="sxs-lookup"><span data-stu-id="67ceb-271">Azure AD doesn’t provide a URL tooget hello metadata.</span></span> <span data-ttu-id="67ceb-272">hello metaadatok XML-fájlként csak olvasható.</span><span class="sxs-lookup"><span data-stu-id="67ceb-272">hello metadata can only be retrieved as a XML file.</span></span>

### <a name="how-tooconfigure-password-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="67ceb-273">Hogyan tooconfigure jelszó egyszeri bejelentkezés az Azure AD gyűjtemény alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="67ceb-273">How tooconfigure password single sign-on for an Azure AD gallery application</span></span>

<span data-ttu-id="67ceb-274">egy alkalmazás hello Azure AD-galériából tooconfigure kell:</span><span class="sxs-lookup"><span data-stu-id="67ceb-274">tooconfigure an application from hello Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="67ceb-275">Alkalmazás hozzáadása hello Azure AD-galériából</span><span class="sxs-lookup"><span data-stu-id="67ceb-275">Add an application from hello Azure AD gallery</span></span>](#add-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="67ceb-276">Állítsa be hello alkalmazását jelszó egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="67ceb-276">Configure hello application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

#### <a name="add-an-application-from-hello-azure-ad-gallery"></a><span data-ttu-id="67ceb-277">Alkalmazás hozzáadása hello Azure AD-galériából</span><span class="sxs-lookup"><span data-stu-id="67ceb-277">Add an application from hello Azure AD gallery</span></span>

<span data-ttu-id="67ceb-278">az alkalmazás a hello Azure AD-katalógusában, tooadd kövesse hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="67ceb-278">tooadd an application from hello Azure AD Gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="67ceb-279">Nyissa meg hello [Azure Portal](https://portal.azure.com) , és jelentkezzen be a egy **globális rendszergazda** vagy **társadminisztrátornak**</span><span class="sxs-lookup"><span data-stu-id="67ceb-279">Open hello [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="67ceb-280">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="67ceb-280">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="67ceb-281">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="67ceb-281">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="67ceb-282">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="67ceb-282">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="67ceb-283">Kattintson a hello **hozzáadása** hello jobb felső sarokban lévő hello a gomb **vállalati alkalmazások** panelen.</span><span class="sxs-lookup"><span data-stu-id="67ceb-283">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="67ceb-284">A hello **adjon meg egy nevet** hello a szövegmező **hello gyűjteményből Hozzáadás** szakaszban, hello alkalmazás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="67ceb-284">In hello **Enter a name** textbox from hello **Add from hello gallery** section, type hello name of hello application.</span></span>

7.  <span data-ttu-id="67ceb-285">Válassza ki a használni kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="67ceb-285">Select hello application you want tooconfigure for single sign-on.</span></span>

8.  <span data-ttu-id="67ceb-286">Hello alkalmazás hozzáadása előtt módosíthatja annak nevét a hello **neve** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="67ceb-286">Before adding hello application, you can change its name from hello **Name** textbox.</span></span>

9.  <span data-ttu-id="67ceb-287">Kattintson a **Hozzáadás** tooadd hello alkalmazás gomb.</span><span class="sxs-lookup"><span data-stu-id="67ceb-287">Click **Add** button, tooadd hello application.</span></span>

<span data-ttu-id="67ceb-288">Egy rövid időszak után kell képes toosee hello alkalmazás konfigurációs panelen.</span><span class="sxs-lookup"><span data-stu-id="67ceb-288">After a short period, you be able toosee hello application’s configuration blade.</span></span>

#### <a name="configure-hello-application-for-password-single-sign-on"></a><span data-ttu-id="67ceb-289">Állítsa be hello alkalmazását jelszó egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="67ceb-289">Configure hello application for password single sign-on</span></span>

<span data-ttu-id="67ceb-290">tooconfigure egyszeri bejelentkezés egy alkalmazás kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="67ceb-290">tooconfigure single sign-on for an application, follow hello steps below:</span></span>

1.  <span data-ttu-id="67ceb-291">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="67ceb-291">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="67ceb-292">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="67ceb-292">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="67ceb-293">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="67ceb-293">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="67ceb-294">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="67ceb-294">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="67ceb-295">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="67ceb-295">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="67ceb-296">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="67ceb-296">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="67ceb-297">Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="67ceb-297">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="67ceb-298">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="67ceb-298">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="67ceb-299">Jelölje be hello mód **jelszóalapú bejelentkezés.**</span><span class="sxs-lookup"><span data-stu-id="67ceb-299">Select hello mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="67ceb-300">[Felhasználók hozzárendelése toohello alkalmazás](#how-to-assign-a-user-to-an-application-directly).</span><span class="sxs-lookup"><span data-stu-id="67ceb-300">[Assign users toohello application](#how-to-assign-a-user-to-an-application-directly).</span></span>

10. <span data-ttu-id="67ceb-301">Emellett is megadhatja hello felhasználó nevében hitelesítő adatok hello felhasználók hello sorát kiválasztásával, és kattintson a **frissítéséhez szükséges hitelesítő adatokat** és hello felhasználónév és jelszó megadásával hello felhasználók nevében.</span><span class="sxs-lookup"><span data-stu-id="67ceb-301">Additionally, you can also provide credentials on behalf of hello user by selecting hello rows of hello users and clicking on **Update Credentials** and entering hello username and password on behalf of hello users.</span></span> <span data-ttu-id="67ceb-302">Ellenkező esetben a felhasználók fognak felszólító tooenter hello hitelesítő adatok magukat az indítás után.</span><span class="sxs-lookup"><span data-stu-id="67ceb-302">Otherwise, users be prompted tooenter hello credentials themselves upon launch.</span></span>

### <a name="how-tooconfigure-password-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="67ceb-303">Hogyan tooconfigure jelszó egyszeri bejelentkezés egy nem galéria alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="67ceb-303">How tooconfigure password single sign-on for a non-gallery application</span></span>

<span data-ttu-id="67ceb-304">egy alkalmazás hello Azure AD-galériából tooconfigure kell:</span><span class="sxs-lookup"><span data-stu-id="67ceb-304">tooconfigure an application from hello Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="67ceb-305">Nem galéria alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="67ceb-305">Add a non-gallery application</span></span>](#add-a-non-gallery-application)

-   [<span data-ttu-id="67ceb-306">Állítsa be hello alkalmazását jelszó egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="67ceb-306">Configure hello application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

#### <a name="add-a-non-gallery-application"></a><span data-ttu-id="67ceb-307">Nem galéria alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="67ceb-307">Add a non-gallery application</span></span>

<span data-ttu-id="67ceb-308">az alkalmazás a hello Azure AD-katalógusában, tooadd kövesse hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="67ceb-308">tooadd an application from hello Azure AD Gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="67ceb-309">Megnyitás hello [Azure Portal](https://portal.azure.com) , és jelentkezzen be egy **globális rendszergazda** vagy **társadminisztrátor**.</span><span class="sxs-lookup"><span data-stu-id="67ceb-309">Open hello [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**.</span></span>

2.  <span data-ttu-id="67ceb-310">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="67ceb-310">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="67ceb-311">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="67ceb-311">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="67ceb-312">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="67ceb-312">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="67ceb-313">Kattintson a hello **hozzáadása** hello jobb felső sarokban lévő hello a gomb **vállalati alkalmazások** panelen.</span><span class="sxs-lookup"><span data-stu-id="67ceb-313">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="67ceb-314">Kattintson a **nem-gyűjtemény alkalmazás.**</span><span class="sxs-lookup"><span data-stu-id="67ceb-314">click **Non-gallery application.**</span></span>

7.  <span data-ttu-id="67ceb-315">Adja meg az alkalmazás nevére hello hello **neve** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="67ceb-315">Enter hello name of your application in hello **Name** textbox.</span></span> <span data-ttu-id="67ceb-316">Válassza ki **hozzáadása.**</span><span class="sxs-lookup"><span data-stu-id="67ceb-316">Select **Add.**</span></span>

<span data-ttu-id="67ceb-317">Egy rövid időszak után kell képes toosee hello alkalmazás konfigurációs panelen.</span><span class="sxs-lookup"><span data-stu-id="67ceb-317">After a short period, you be able toosee hello application’s configuration blade.</span></span>

#### <a name="configure-hello-application-for-password-single-sign-on"></a><span data-ttu-id="67ceb-318">Állítsa be hello alkalmazását jelszó egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="67ceb-318">Configure hello application for password single sign-on</span></span>

<span data-ttu-id="67ceb-319">tooconfigure egyszeri bejelentkezés egy alkalmazás kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="67ceb-319">tooconfigure single sign-on for an application, follow hello steps below:</span></span>

1.  <span data-ttu-id="67ceb-320">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="67ceb-320">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="67ceb-321">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="67ceb-321">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="67ceb-322">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="67ceb-322">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="67ceb-323">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="67ceb-323">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="67ceb-324">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="67ceb-324">click **All Applications** tooview a list of all your applications.</span></span>

    1.  <span data-ttu-id="67ceb-325">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="67ceb-325">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="67ceb-326">Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="67ceb-326">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="67ceb-327">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="67ceb-327">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="67ceb-328">Jelölje be hello mód **jelszóalapú bejelentkezés.**</span><span class="sxs-lookup"><span data-stu-id="67ceb-328">Select hello mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="67ceb-329">Adja meg a hello **bejelentkezési URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="67ceb-329">Enter hello **Sign-on URL**.</span></span> <span data-ttu-id="67ceb-330">Ez a hello URL-cím, ahol felhasználók adja meg a felhasználónevet és jelszót toosign a számára.</span><span class="sxs-lookup"><span data-stu-id="67ceb-330">This is hello URL where users enter their username and password toosign in to.</span></span> <span data-ttu-id="67ceb-331">Győződjön meg arról, mezők hello bejelentkezés láthatók hello URL-címen.</span><span class="sxs-lookup"><span data-stu-id="67ceb-331">Ensure hello sign in fields are visible at hello URL.</span></span>

10. <span data-ttu-id="67ceb-332">[Felhasználók hozzárendelése toohello alkalmazás](#how-to-assign-a-user-to-an-application-directly).</span><span class="sxs-lookup"><span data-stu-id="67ceb-332">[Assign users toohello application](#how-to-assign-a-user-to-an-application-directly).</span></span>

11. <span data-ttu-id="67ceb-333">Emellett is megadhatja hello felhasználó nevében hitelesítő adatok hello felhasználók hello sorát kiválasztásával, és kattintson a **frissítéséhez szükséges hitelesítő adatokat** és hello felhasználónév és jelszó megadásával hello felhasználók nevében.</span><span class="sxs-lookup"><span data-stu-id="67ceb-333">Additionally, you can also provide credentials on behalf of hello user by selecting hello rows of hello users and clicking on **Update Credentials** and entering hello username and password on behalf of hello users.</span></span> <span data-ttu-id="67ceb-334">Ellenkező esetben a felhasználók fognak felszólító tooenter hello hitelesítő adatok magukat az indítás után.</span><span class="sxs-lookup"><span data-stu-id="67ceb-334">Otherwise, users be prompted tooenter hello credentials themselves upon launch.</span></span>

## <a name="problems-related-tooassigning-applications-toousers"></a><span data-ttu-id="67ceb-335">Problémák kapcsolódó tooassigning alkalmazások toousers</span><span class="sxs-lookup"><span data-stu-id="67ceb-335">Problems related tooassigning applications toousers</span></span>

<span data-ttu-id="67ceb-336">A felhasználó nem lehet annak, hogy egy alkalmazás a hozzáférési panelen, mert ezek nincsenek hozzárendelve toohello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="67ceb-336">A user may not be seeing an application on their Access Panel because they are not assigned toohello application.</span></span> <span data-ttu-id="67ceb-337">Az alábbiakban néhány módon toocheck:</span><span class="sxs-lookup"><span data-stu-id="67ceb-337">Below are some ways toocheck:</span></span>

-   [<span data-ttu-id="67ceb-338">Ellenőrizze, hogy ha a felhasználó hozzá van rendelve toohello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="67ceb-338">Check if a user is assigned toohello application</span></span>](#check-if-a-user-is-assigned-to-the-application)

-   [<span data-ttu-id="67ceb-339">Hogyan tooassign közvetlenül egy felhasználói tooan alkalmazást</span><span class="sxs-lookup"><span data-stu-id="67ceb-339">How tooassign a user tooan application directly</span></span>](#how-to-assign-a-user-to-an-application-directly)

-   [<span data-ttu-id="67ceb-340">Ellenőrizze, hogy a felhasználó hozzá van rendelve a tooa licenc kapcsolódó toohello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="67ceb-340">Check if a user is assigned tooa license related toohello application</span></span>](#check-if-a-user-is-under-a-license-related-to-the-application)

-   [<span data-ttu-id="67ceb-341">Hogyan tooassign licenc tooa felhasználó</span><span class="sxs-lookup"><span data-stu-id="67ceb-341">How tooassign a license tooa user</span></span>](#how-to-assign-a-user-a-license)

### <a name="check-if-a-user-is-assigned-toohello-application"></a><span data-ttu-id="67ceb-342">Ellenőrizze, hogy ha a felhasználó hozzá van rendelve toohello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="67ceb-342">Check if a user is assigned toohello application</span></span>

<span data-ttu-id="67ceb-343">toocheck a felhasználó hozzá van rendelve a toohello alkalmazást, kövesse az hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="67ceb-343">toocheck if a user is assigned toohello application, follow hello steps below:</span></span>

1.  <span data-ttu-id="67ceb-344">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="67ceb-344">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="67ceb-345">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="67ceb-345">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="67ceb-346">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="67ceb-346">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="67ceb-347">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="67ceb-347">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="67ceb-348">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="67ceb-348">click **All Applications** tooview a list of all your applications.</span></span>

6.  <span data-ttu-id="67ceb-349">**Keresési** a szóban forgó hello alkalmazás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="67ceb-349">**Search** for hello name of hello application in question.</span></span>

7.  <span data-ttu-id="67ceb-350">Kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="67ceb-350">click **Users and groups**.</span></span>

8.  <span data-ttu-id="67ceb-351">Ellenőrizze a toosee, ha a felhasználó a toohello alkalmazás hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="67ceb-351">Check toosee if your user is assigned toohello application.</span></span>

   * <span data-ttu-id="67ceb-352">Ha nem hello kövesse "hogyan tooassign közvetlenül a felhasználói tooan alkalmazás" toodo így.</span><span class="sxs-lookup"><span data-stu-id="67ceb-352">If not follow hello steps in “How tooassign a user tooan application directly” toodo so.</span></span>

### <a name="how-tooassign-a-user-tooan-application-directly"></a><span data-ttu-id="67ceb-353">Hogyan tooassign közvetlenül egy felhasználói tooan alkalmazást</span><span class="sxs-lookup"><span data-stu-id="67ceb-353">How tooassign a user tooan application directly</span></span>

<span data-ttu-id="67ceb-354">tooassign felhasználók tooan egy vagy több alkalmazás közvetlenül, kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="67ceb-354">tooassign one or more users tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="67ceb-355">Megnyitás hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda**.</span><span class="sxs-lookup"><span data-stu-id="67ceb-355">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator**.</span></span>

2.  <span data-ttu-id="67ceb-356">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="67ceb-356">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="67ceb-357">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="67ceb-357">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="67ceb-358">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="67ceb-358">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="67ceb-359">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="67ceb-359">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="67ceb-360">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="67ceb-360">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="67ceb-361">Válassza ki a kívánt felhasználó toofrom hello listáját tooassign hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="67ceb-361">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="67ceb-362">Amikor hello alkalmazás betölt, kattintson a **felhasználók és csoportok** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="67ceb-362">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="67ceb-363">Hello kattintson **Hozzáadás** gomb felett hello **felhasználók és csoportok** lista tooopen hello **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="67ceb-363">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="67ceb-364">hello kattintson **felhasználók és csoportok** hello a választó **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="67ceb-364">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="67ceb-365">Hello típusának **teljes név** vagy **e-mail cím** érdekli hello való hozzárendelése hello felhasználó **Keresés név vagy e-mail cím alapján** keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="67ceb-365">Type in hello **full name** or **email address** of hello user you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="67ceb-366">Hello rámutat **felhasználói** a hello lista tooreveal egy **jelölőnégyzet**.</span><span class="sxs-lookup"><span data-stu-id="67ceb-366">Hover over hello **user** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="67ceb-367">Kattintson a hello jelölőnégyzet következő toohello felhasználó profil fénykép vagy embléma tooadd a felhasználó toohello **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="67ceb-367">Click hello checkbox next toohello user’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="67ceb-368">**Választható lehetőség:** Ha túl szeretné**egynél több felhasználó hozzáadása**, egy másik típus **teljes név** vagy **e-mail cím** be hello **Keresés név e-mail cím vagy** keresési mezőbe, majd kattintson a hello jelölőnégyzet tooadd a felhasználó toohello **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="67ceb-368">**Optional:** If you would like too**add more than one user**, type in another **full name** or **email address** into hello **Search by name or email address** search box, and click hello checkbox tooadd this user toohello **Selected** list.</span></span>

13. <span data-ttu-id="67ceb-369">Ha elkészült, válassza a felhasználók, kattintson a hello **válasszon** gomb tooadd őket felhasználók és csoportok toobe toohello listája toohello alkalmazás hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="67ceb-369">When you are finished selecting users, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="67ceb-370">**Nem kötelező:** hello kattintson **Szerepkörválasztás** hello a választó **hozzáadása hozzárendelés** panel tooselect szerepkör tooassign toohello felhasználók kijelölt.</span><span class="sxs-lookup"><span data-stu-id="67ceb-370">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello users you have selected.</span></span>

15. <span data-ttu-id="67ceb-371">Kattintson a hello **hozzárendelése** gomb tooassign hello alkalmazás toohello kijelölt felhasználók.</span><span class="sxs-lookup"><span data-stu-id="67ceb-371">Click hello **Assign** button tooassign hello application toohello selected users.</span></span>

<span data-ttu-id="67ceb-372">Miután rövid időn belül, kijelölt hello felhasználók kell tudni toolaunch ezeket az alkalmazásokat hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="67ceb-372">After a short period, hello users you have selected be able toolaunch these applications in hello Access Panel.</span></span>

### <a name="check-if-a-user-is-under-a-license-related-toohello-application"></a><span data-ttu-id="67ceb-373">Annak ellenőrzése, hogy egy felhasználó egy licenc kapcsolódó toohello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="67ceb-373">Check if a user is under a license related toohello application</span></span>

<span data-ttu-id="67ceb-374">toocheck a felhasználó hozzárendelt licencek, hajtsa végre hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="67ceb-374">toocheck a user’s assigned licenses, follow hello steps below:</span></span>

1.  <span data-ttu-id="67ceb-375">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="67ceb-375">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="67ceb-376">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="67ceb-376">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="67ceb-377">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="67ceb-377">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="67ceb-378">Kattintson a **felhasználók és csoportok** hello navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="67ceb-378">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="67ceb-379">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="67ceb-379">click **All users**.</span></span>

6.  <span data-ttu-id="67ceb-380">**Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.</span><span class="sxs-lookup"><span data-stu-id="67ceb-380">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="67ceb-381">Kattintson a **licencek** toosee licencek hello felhasználóhoz van rendelve.</span><span class="sxs-lookup"><span data-stu-id="67ceb-381">click **Licenses** toosee which licenses hello user currently has assigned.</span></span>

  * <span data-ttu-id="67ceb-382">Hello felhasználó van hozzárendelve, tooan Office-licencet az első fél Office alkalmazások tooappear ezzel engedélyezi a felhasználó hozzáférési Panel hello.</span><span class="sxs-lookup"><span data-stu-id="67ceb-382">If hello user is assigned tooan Office license this enable First Party Office applications tooappear on hello user’s Access Panel.</span></span>

### <a name="how-tooassign-a-user-a-license"></a><span data-ttu-id="67ceb-383">Hogyan tooassign egy felhasználói licenc</span><span class="sxs-lookup"><span data-stu-id="67ceb-383">How tooassign a user a license</span></span> 

<span data-ttu-id="67ceb-384">a licenc tooa felhasználó tooassign kövesse hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="67ceb-384">tooassign a license tooa user, follow hello steps below:</span></span>

1.  <span data-ttu-id="67ceb-385">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="67ceb-385">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="67ceb-386">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="67ceb-386">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="67ceb-387">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="67ceb-387">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="67ceb-388">Kattintson a **felhasználók és csoportok** hello navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="67ceb-388">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="67ceb-389">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="67ceb-389">click **All users**.</span></span>

6.  <span data-ttu-id="67ceb-390">**Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.</span><span class="sxs-lookup"><span data-stu-id="67ceb-390">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="67ceb-391">Kattintson a **licencek** toosee licencek hello felhasználóhoz van rendelve.</span><span class="sxs-lookup"><span data-stu-id="67ceb-391">click **Licenses** toosee which licenses hello user currently has assigned.</span></span>

8.  <span data-ttu-id="67ceb-392">Kattintson a hello **hozzárendelése** gombra.</span><span class="sxs-lookup"><span data-stu-id="67ceb-392">click hello **Assign** button.</span></span>

9.  <span data-ttu-id="67ceb-393">Válassza ki **egy vagy több termék** választható termékek hello listája.</span><span class="sxs-lookup"><span data-stu-id="67ceb-393">Select **one or more products** from hello list of available products.</span></span>

10. <span data-ttu-id="67ceb-394">**Nem kötelező** hello kattintson **hozzárendelés beállításai** elem toogranularly rendelje hozzá a termékek.</span><span class="sxs-lookup"><span data-stu-id="67ceb-394">**Optional** click hello **assignment options** item toogranularly assign products.</span></span> <span data-ttu-id="67ceb-395">Kattintson a **Ok** amikor ez befejeződik.</span><span class="sxs-lookup"><span data-stu-id="67ceb-395">Click **Ok** when this is completed.</span></span>

11. <span data-ttu-id="67ceb-396">Kattintson a hello **hozzárendelése** tooassign ezen licencek toothis felhasználói gombra.</span><span class="sxs-lookup"><span data-stu-id="67ceb-396">Click hello **Assign** button tooassign these licenses toothis user.</span></span>

## <a name="problems-related-tooassigning-applications-toogroups"></a><span data-ttu-id="67ceb-397">Problémák kapcsolódó tooassigning alkalmazások toogroups</span><span class="sxs-lookup"><span data-stu-id="67ceb-397">Problems related tooassigning applications toogroups</span></span>

<span data-ttu-id="67ceb-398">Egy felhasználó lehet annak, hogy egy alkalmazás a hozzáférési Panel a mert hello alkalmazás rendelt csoportba.</span><span class="sxs-lookup"><span data-stu-id="67ceb-398">A user may be seeing an application on their Access Panel because they are part of a group that has been assigned hello application.</span></span> <span data-ttu-id="67ceb-399">Az alábbiakban néhány módon toocheck:</span><span class="sxs-lookup"><span data-stu-id="67ceb-399">Below are some ways toocheck:</span></span>

-   [<span data-ttu-id="67ceb-400">A felhasználói csoporttagság ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="67ceb-400">Check a user’s group memberships</span></span>](#check-a-users-group-memberships)

-   [<span data-ttu-id="67ceb-401">Hogyan tooassign egy alkalmazás tooa csoportosítani közvetlenül</span><span class="sxs-lookup"><span data-stu-id="67ceb-401">How tooassign an application tooa group directly</span></span>](#how-to-assign-an-application-to-a-group-directly)

-   [<span data-ttu-id="67ceb-402">Ellenőrizze, hogy a felhasználó csoportba tooa licenc hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="67ceb-402">Check if a user is part of group assigned tooa license</span></span>](#check-if-a-user-is-part-of-group-assigned-to-a-license)

-   [<span data-ttu-id="67ceb-403">Hogyan tooassign tooa licenccsoportok</span><span class="sxs-lookup"><span data-stu-id="67ceb-403">How tooassign a license tooa group</span></span>](#how-to-assign-a-license-to-a-group)

### <a name="check-a-users-group-memberships"></a><span data-ttu-id="67ceb-404">A felhasználói csoporttagság ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="67ceb-404">Check a user’s group memberships</span></span>

<span data-ttu-id="67ceb-405">toocheck egy csoport tagságát, hajtsa végre hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="67ceb-405">toocheck a group’s membership, follow hello steps below:</span></span>

1.  <span data-ttu-id="67ceb-406">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="67ceb-406">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="67ceb-407">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="67ceb-407">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="67ceb-408">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="67ceb-408">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="67ceb-409">Kattintson a **felhasználók és csoportok** hello navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="67ceb-409">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="67ceb-410">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="67ceb-410">click **All users**.</span></span>

6.  <span data-ttu-id="67ceb-411">**Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.</span><span class="sxs-lookup"><span data-stu-id="67ceb-411">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="67ceb-412">Kattintson a **csoportok**.</span><span class="sxs-lookup"><span data-stu-id="67ceb-412">click **Groups**.</span></span>

8.  <span data-ttu-id="67ceb-413">Toosee ellenőrizze, hogy a felhasználók egy csoportjának toohello alkalmazás részét.</span><span class="sxs-lookup"><span data-stu-id="67ceb-413">Check toosee if your user is part of a Group assigned toohello application.</span></span>

  * <span data-ttu-id="67ceb-414">Ha azt szeretné, hogy tooremove hello felhasználói hello csoportból **hello sorban kattintson** hello csoport, és válassza a törlés.</span><span class="sxs-lookup"><span data-stu-id="67ceb-414">If you want tooremove hello user from hello group, **click hello row** of hello group and select delete.</span></span>

### <a name="how-tooassign-an-application-tooa-group-directly"></a><span data-ttu-id="67ceb-415">Hogyan tooassign egy alkalmazás tooa csoportosítani közvetlenül</span><span class="sxs-lookup"><span data-stu-id="67ceb-415">How tooassign an application tooa group directly</span></span>

<span data-ttu-id="67ceb-416">egy vagy több tooassign csoportok közvetlenül, tooan alkalmazás kövesse hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="67ceb-416">tooassign one or more groups tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="67ceb-417">Megnyitás hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda**.</span><span class="sxs-lookup"><span data-stu-id="67ceb-417">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator**.</span></span>

2.  <span data-ttu-id="67ceb-418">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="67ceb-418">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="67ceb-419">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="67ceb-419">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="67ceb-420">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="67ceb-420">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="67ceb-421">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="67ceb-421">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="67ceb-422">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="67ceb-422">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="67ceb-423">Válassza ki a kívánt felhasználó toofrom hello listáját tooassign hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="67ceb-423">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="67ceb-424">Amikor hello alkalmazás betölt, kattintson a **felhasználók és csoportok** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="67ceb-424">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="67ceb-425">Hello kattintson **Hozzáadás** gomb felett hello **felhasználók és csoportok** lista tooopen hello **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="67ceb-425">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="67ceb-426">hello kattintson **felhasználók és csoportok** hello a választó **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="67ceb-426">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="67ceb-427">Hello típusa **teljes csoportnév** érdekli hello való hozzárendelése hello csoport **Keresés név vagy e-mail cím alapján** keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="67ceb-427">Type in hello **full group name** of hello group you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="67ceb-428">Hello rámutat **csoport** a hello lista tooreveal egy **jelölőnégyzet**.</span><span class="sxs-lookup"><span data-stu-id="67ceb-428">Hover over hello **group** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="67ceb-429">Kattintson a profil fénykép vagy embléma tooadd hello jelölőnégyzet következő toohello csoport a felhasználó toohello **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="67ceb-429">Click hello checkbox next toohello group’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="67ceb-430">**Nem kötelező:** Ha túl szeretné**egynél több csoport hozzáadása**, egy másik típus **teljes csoport neve** hello be **Keresés név vagy e-mail cím alapján** keresőmezőbe hello jelölőnégyzet tooadd kattintson a csoport toohello **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="67ceb-430">**Optional:** If you would like too**add more than one group**, type in another **full group name** into hello **Search by name or email address** search box, and click hello checkbox tooadd this group toohello **Selected** list.</span></span>

13. <span data-ttu-id="67ceb-431">Ha befejezte a csoportok kiválasztásával, kattintson a hello **válasszon** gomb tooadd őket felhasználók és csoportok toobe toohello listája toohello alkalmazás hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="67ceb-431">When you are finished selecting groups, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="67ceb-432">**Választható lehetőség:** hello kattintson **Szerepkörválasztás** hello a választó **hozzáadása hozzárendelés** panel tooselect egy szerepkör tooassign toohello csoportokat kijelölt.</span><span class="sxs-lookup"><span data-stu-id="67ceb-432">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello groups you have selected.</span></span>

15. <span data-ttu-id="67ceb-433">Kattintson a hello **hozzárendelése** gomb tooassign hello alkalmazás toohello kijelölt csoportok.</span><span class="sxs-lookup"><span data-stu-id="67ceb-433">Click hello **Assign** button tooassign hello application toohello selected groups.</span></span>

<span data-ttu-id="67ceb-434">Miután rövid időn belül, kijelölt hello felhasználók kell tudni toolaunch ezeket az alkalmazásokat hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="67ceb-434">After a short period, hello users you have selected be able toolaunch these applications in hello Access Panel.</span></span>

### <a name="check-if-a-user-is-part-of-group-assigned-tooa-license"></a><span data-ttu-id="67ceb-435">Ellenőrizze, hogy a felhasználó csoportba tooa licenc hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="67ceb-435">Check if a user is part of group assigned tooa license</span></span>

1.  <span data-ttu-id="67ceb-436">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="67ceb-436">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="67ceb-437">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="67ceb-437">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="67ceb-438">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="67ceb-438">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="67ceb-439">Kattintson a **felhasználók és csoportok** hello navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="67ceb-439">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="67ceb-440">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="67ceb-440">click **All users**.</span></span>

6.  <span data-ttu-id="67ceb-441">**Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.</span><span class="sxs-lookup"><span data-stu-id="67ceb-441">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="67ceb-442">Kattintson a **csoportok**.</span><span class="sxs-lookup"><span data-stu-id="67ceb-442">click **Groups**.</span></span>

8.  <span data-ttu-id="67ceb-443">Kattintson egy meghatározott csoport hello sorát.</span><span class="sxs-lookup"><span data-stu-id="67ceb-443">click hello row of a specific group.</span></span>

9.  <span data-ttu-id="67ceb-444">Kattintson a **licencek** melyik licencek hello csoporthoz rendelt tooit toosee.</span><span class="sxs-lookup"><span data-stu-id="67ceb-444">click **Licenses** toosee which licenses hello group has assigned tooit.</span></span>

   * <span data-ttu-id="67ceb-445">Ha hello csoport hozzárendelt tooan Office-licencet ezzel előfordulhat, hogy engedélyezi bizonyos első fél Office alkalmazások tooappear hello felhasználói hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="67ceb-445">If hello group is assigned tooan Office license this may enable certain First Party Office applications tooappear on hello user’s Access Panel.</span></span>

### <a name="how-tooassign-a-license-tooa-group"></a><span data-ttu-id="67ceb-446">Hogyan tooassign tooa licenccsoportok</span><span class="sxs-lookup"><span data-stu-id="67ceb-446">How tooassign a license tooa group</span></span>

<span data-ttu-id="67ceb-447">tooassign tooa licenccsoport, kövesse a hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="67ceb-447">tooassign a license tooa group, follow hello steps below:</span></span>

1.  <span data-ttu-id="67ceb-448">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="67ceb-448">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="67ceb-449">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="67ceb-449">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="67ceb-450">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="67ceb-450">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="67ceb-451">Kattintson a **felhasználók és csoportok** hello navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="67ceb-451">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="67ceb-452">Kattintson a **összes csoport**.</span><span class="sxs-lookup"><span data-stu-id="67ceb-452">click **All groups**.</span></span>

6.  <span data-ttu-id="67ceb-453">**Keresési** hello csoport érdekli, és **hello sorban kattintson** tooselect.</span><span class="sxs-lookup"><span data-stu-id="67ceb-453">**Search** for hello group you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="67ceb-454">Kattintson a **licencek** toosee mely licencek hello csoport jelenleg hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="67ceb-454">click **Licenses** toosee which licenses hello group currently has assigned.</span></span>

8.  <span data-ttu-id="67ceb-455">Kattintson a hello **hozzárendelése** gombra.</span><span class="sxs-lookup"><span data-stu-id="67ceb-455">click hello **Assign** button.</span></span>

9.  <span data-ttu-id="67ceb-456">Válassza ki **egy vagy több termék** választható termékek hello listája.</span><span class="sxs-lookup"><span data-stu-id="67ceb-456">Select **one or more products** from hello list of available products.</span></span>

10. <span data-ttu-id="67ceb-457">**Nem kötelező** hello kattintson **hozzárendelés beállításai** elem toogranularly rendelje hozzá a termékek.</span><span class="sxs-lookup"><span data-stu-id="67ceb-457">**Optional** click hello **assignment options** item toogranularly assign products.</span></span> <span data-ttu-id="67ceb-458">Kattintson a **Ok** amikor ez befejeződik.</span><span class="sxs-lookup"><span data-stu-id="67ceb-458">Click **Ok** when this is completed.</span></span>

11. <span data-ttu-id="67ceb-459">Kattintson a hello **hozzárendelése** tooassign licencek toothis csoportbiztonsági gombra.</span><span class="sxs-lookup"><span data-stu-id="67ceb-459">Click hello **Assign** button tooassign these licenses toothis group.</span></span> <span data-ttu-id="67ceb-460">Ez eltarthat egy ideig, attól függően, hogy hello méretét és összetettségét hello csoport.</span><span class="sxs-lookup"><span data-stu-id="67ceb-460">This may take a long time, depending on hello size and complexity of hello group.</span></span>

>[!NOTE]
><span data-ttu-id="67ceb-461">toodo Ez gyorsabb, érdemes lehet ideiglenesen hozzárendelés közvetlenül a licenc toohello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="67ceb-461">toodo this faster, consider temporarily assigning a license toohello user directly.</span></span> 
>
>

## <a name="next-steps"></a><span data-ttu-id="67ceb-462">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="67ceb-462">Next steps</span></span>
[<span data-ttu-id="67ceb-463">Adja hozzá az új felhasználók tooAzure Active Directory</span><span class="sxs-lookup"><span data-stu-id="67ceb-463">Add new users tooAzure Active Directory</span></span>](active-directory-users-create-azure-portal.md)

