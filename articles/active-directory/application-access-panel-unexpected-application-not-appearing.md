---
title: "Egy hozzárendelt alkalmazás nem jelenik meg a hozzáférési panel |} Microsoft Docs"
description: "Ezért az alkalmazás nem jelenik meg a hozzáférési Panel hibaelhárítása"
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
ms.openlocfilehash: 9ea5744d77b90929598ea5feb80c7bbdff3772fc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="an-assigned-application-is-not-appearing-on-the-access-panel"></a><span data-ttu-id="63cf6-103">Egy hozzárendelt alkalmazás nem jelenik meg a hozzáférési panel</span><span class="sxs-lookup"><span data-stu-id="63cf6-103">An assigned application is not appearing on the access panel</span></span>

<span data-ttu-id="63cf6-104">A hozzáférési Panel egy webes portál, amely lehetővé teszi a felhasználó munkahelyi vagy iskolai fiókkal az Azure Active Directory (Azure AD) a megtekintése, és indítsa el a felhőalapú alkalmazások, hogy az Azure AD-rendszergazda engedélyezte őket hozzáférés annak.</span><span class="sxs-lookup"><span data-stu-id="63cf6-104">The Access Panel is a web-based portal which enables a user with a work or school account in Azure Active Directory (Azure AD) to view and start cloud-based applications that the Azure AD administrator has granted them access to.</span></span> <span data-ttu-id="63cf6-105">Ezeket az alkalmazásokat úgy vannak konfigurálva, az Azure AD portálon a felhasználó nevében.</span><span class="sxs-lookup"><span data-stu-id="63cf6-105">These applications are configured on behalf of the user in the Azure AD portal.</span></span> <span data-ttu-id="63cf6-106">Az alkalmazás megfelelően konfigurálva legyen, és a felhasználó vagy egy csoport, a felhasználó tagja a hozzáférési panelen az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="63cf6-106">The application must be configured properly and assigned to the user or a group the user is a member of to see the application in the Access Panel.</span></span>

<span data-ttu-id="63cf6-107">Alkalmazások azért jelent meg a felhasználó típusa a következő kategóriákba tartoznak:</span><span class="sxs-lookup"><span data-stu-id="63cf6-107">The type of apps a user may be seeing fall in the following categories:</span></span>

-   <span data-ttu-id="63cf6-108">Office 365-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="63cf6-108">Office 365 Applications</span></span>

-   <span data-ttu-id="63cf6-109">Összevonási-alapú egyszeri bejelentkezési modellel konfigurált a Microsoft és külső alkalmazások</span><span class="sxs-lookup"><span data-stu-id="63cf6-109">Microsoft and third-party applications configured with federation-based SSO</span></span>

-   <span data-ttu-id="63cf6-110">Jelszó-alapú egyszeri bejelentkezés alkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="63cf6-110">Password-based SSO applications</span></span>

-   <span data-ttu-id="63cf6-111">A már meglévő SSO megoldásaival alkalmazások</span><span class="sxs-lookup"><span data-stu-id="63cf6-111">Applications with existing SSO solutions</span></span>

## <a name="general-issues-to-check-first"></a><span data-ttu-id="63cf6-112">Először ellenőrizze a általános problémák</span><span class="sxs-lookup"><span data-stu-id="63cf6-112">General issues to check first</span></span>

-   <span data-ttu-id="63cf6-113">Ha egy alkalmazás egy felhasználó előbbiekben felvett, próbáljon újból bejelentkezni bejövő és kimenő adatforgalma újra a felhasználó hozzáférési panelre néhány perc múlva szerepel-e az alkalmazás van.</span><span class="sxs-lookup"><span data-stu-id="63cf6-113">If an application was just added to a user, try to sign in and out again into the user’s Access Panel after a few minutes to see if the application is added.</span></span>

-   <span data-ttu-id="63cf6-114">Ha a licenc csak felhasználó vagy csoport, a felhasználó nem tagja ennek eltarthat egy ideig, attól függően, hogy méretét és összetettségét, el kell végezni a változásokat a csoport el lett távolítva.</span><span class="sxs-lookup"><span data-stu-id="63cf6-114">If a license was just removed from a user or group the user is a member of this may take a long time, depending on the size and complexity of the group for changes to be made.</span></span> <span data-ttu-id="63cf6-115">Lehetővé teszi további idő előtt jelentkezik be a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="63cf6-115">Allow for extra time before signing into the Access Panel.</span></span>

## <a name="problems-related-to-application-configuration"></a><span data-ttu-id="63cf6-116">Alkalmazás-konfigurációval kapcsolatos problémák</span><span class="sxs-lookup"><span data-stu-id="63cf6-116">Problems related to application configuration</span></span>

<span data-ttu-id="63cf6-117">Egy alkalmazás nem lehetséges, hogy megjelenjen a a felhasználó hozzáférési Panel, mert nincs megfelelően konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="63cf6-117">An application may not be appearing in a user’s Access Panel because it is not configured properly.</span></span> <span data-ttu-id="63cf6-118">Az alábbiakban néhány módszert alkalmazás konfigurációjával kapcsolatos problémák hibaelhárítását hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="63cf6-118">Below are some ways you can troubleshoot issues related to application configuration:</span></span>

-   [<span data-ttu-id="63cf6-119">Összevont egyszeri bejelentkezés az Azure AD gyűjtemény alkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="63cf6-119">How to configure federated single sign-on for an Azure AD gallery application</span></span>](#how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application)

-   [<span data-ttu-id="63cf6-120">Összevont egyszeri bejelentkezés nem galéria alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="63cf6-120">How to configure federated single sign-on for a non-gallery application</span></span>](#how-to-configure-federated-single-sign-on-for-a-non-gallery-application)

-   [<span data-ttu-id="63cf6-121">Egy jelszó egyszeri bejelentkezési alkalmazás gyűjtemény az Azure AD alkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="63cf6-121">How to configure a password single sign-on application for an Azure AD gallery application</span></span>](#how-to-configure-password-single-sign-on-for-a-non-gallery-application)

-   [<span data-ttu-id="63cf6-122">A jelszó egyszeri bejelentkezési alkalmazás nem galéria alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="63cf6-122">How to configure a password single sign-on application for a non-gallery application</span></span>](#how-to-configure-password-single-sign-on-for-a-non-gallery-application)

### <a name="how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="63cf6-123">Összevont egyszeri bejelentkezés az Azure AD gyűjtemény alkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="63cf6-123">How to configure federated single sign-on for an Azure AD gallery application</span></span>

<span data-ttu-id="63cf6-124">Minden alkalmazást a vállalati egyszeri bejelentkezésre alkalmas engedélyezve van az Azure AD-katalógus elérhető egy részletes oktatóanyag rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="63cf6-124">All applications in the Azure AD gallery enabled with Enterprise Single Sign-On capability has a step-by-step tutorial available.</span></span> <span data-ttu-id="63cf6-125">Érheti el a [SaaS-alkalmazásokhoz az Azure Active Directoryval integrációjával kapcsolatos bemutatók felsorolása](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) részletes lépésenkénti útmutatót.</span><span class="sxs-lookup"><span data-stu-id="63cf6-125">You can access the [List of tutorials on how to integrate SaaS apps with Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) for a detail step-by-step guidance.</span></span>

<span data-ttu-id="63cf6-126">Egy alkalmazás kell az Azure AD-galériából konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="63cf6-126">To configure an application from the Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="63cf6-127">Alkalmazás hozzáadása az Azure AD-galériából</span><span class="sxs-lookup"><span data-stu-id="63cf6-127">Add an application from the Azure AD gallery</span></span>](#add-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="63cf6-128">Az alkalmazás metaadatai értékeinek konfigurálása az Azure AD (bejelentkezési URL-cím, az azonosítója, válasz URL-címe)</span><span class="sxs-lookup"><span data-stu-id="63cf6-128">Configure the application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="63cf6-129">Válassza ki a felhasználói azonosítóját, és elküldi az alkalmazás felhasználói attribútumok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="63cf6-129">Select User Identifier and add user attributes to be sent to the application</span></span>](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [<span data-ttu-id="63cf6-130">Az Azure AD-metaadatok és a tanúsítvány lekérése</span><span class="sxs-lookup"><span data-stu-id="63cf6-130">Retrieve Azure AD metadata and certificate</span></span>](#download-the-azure-ad-metadata-or-certificate)

-   [<span data-ttu-id="63cf6-131">Konfigurálja az Azure AD metaadatok értékeket az alkalmazásban (bejelentkezési URL-címet, a kibocsátó, a kijelentkezési URL-cím és a tanúsítvány)</span><span class="sxs-lookup"><span data-stu-id="63cf6-131">Configure Azure AD metadata values in the application (Sign on URL, Issuer, Logout URL and certificate)</span></span>](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

#### <a name="add-an-application-from-the-azure-ad-gallery"></a><span data-ttu-id="63cf6-132">Alkalmazás hozzáadása az Azure AD-galériából</span><span class="sxs-lookup"><span data-stu-id="63cf6-132">Add an application from the Azure AD gallery</span></span>

<span data-ttu-id="63cf6-133">Hozzáadhat egy alkalmazást az Azure AD-gyűjteményből, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="63cf6-133">To add an application from the Azure AD Gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="63cf6-134">Nyissa meg a [Azure Portal](https://portal.azure.com) , és jelentkezzen be egy **globális rendszergazda** vagy **társadminisztrátornak**</span><span class="sxs-lookup"><span data-stu-id="63cf6-134">Open the [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="63cf6-135">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="63cf6-135">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="63cf6-136">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="63cf6-136">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="63cf6-137">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="63cf6-137">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="63cf6-138">Kattintson a **Hozzáadás** gombra a jobb felső sarkában a **vállalati alkalmazások** panelen.</span><span class="sxs-lookup"><span data-stu-id="63cf6-138">click the **Add** button at the top-right corner on the **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="63cf6-139">Az a **adjon meg egy nevet** a szövegmező a **hozzáadása a gyűjteményből** területen írja be az alkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="63cf6-139">In the **Enter a name** textbox from the **Add from the gallery** section, type the name of the application.</span></span>

7.  <span data-ttu-id="63cf6-140">Válassza ki az alkalmazást, az egyszeri bejelentkezés konfigurálni szeretné.</span><span class="sxs-lookup"><span data-stu-id="63cf6-140">Select the application you want to configure for single sign-on.</span></span>

8.  <span data-ttu-id="63cf6-141">Ad hozzá az alkalmazást, mielőtt a nevét módosíthatja a **neve** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="63cf6-141">Before adding the application, you can change its name from the **Name** textbox.</span></span>

9.  <span data-ttu-id="63cf6-142">Kattintson a **Hozzáadás** gombra, vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="63cf6-142">Click **Add** button, to add the application.</span></span>

<span data-ttu-id="63cf6-143">Egy rövid időszak után el az alkalmazás konfigurációs panelen láthatók.</span><span class="sxs-lookup"><span data-stu-id="63cf6-143">After a short period, you be able to see the application’s configuration blade.</span></span>

#### <a name="configure-single-sign-on-for-an-application-from-the-azure-ad-gallery"></a><span data-ttu-id="63cf6-144">Egyszeri bejelentkezés egy alkalmazás az Azure AD-galériából konfigurálása</span><span class="sxs-lookup"><span data-stu-id="63cf6-144">Configure single sign-on for an application from the Azure AD gallery</span></span>

<span data-ttu-id="63cf6-145">Egyszeri bejelentkezés egy alkalmazás konfigurálásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="63cf6-145">To configure single sign-on for an application, follow the steps below:</span></span>

1.  <span data-ttu-id="63cf6-146">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="63cf6-146">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="63cf6-147">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="63cf6-147">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="63cf6-148">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="63cf6-148">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="63cf6-149">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="63cf6-149">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="63cf6-150">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="63cf6-150">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="63cf6-151">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="63cf6-151">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="63cf6-152">Válassza ki az egyszeri bejelentkezés konfigurálni kívánt alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="63cf6-152">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="63cf6-153">Ha az alkalmazás betölt, kattintson a **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="63cf6-153">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="63cf6-154">Válassza ki **SAML-alapú bejelentkezés** a a **mód** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="63cf6-154">Select **SAML-based Sign-on** from the **Mode** dropdown.</span></span>

9.  <span data-ttu-id="63cf6-155">Adja meg a szükséges értékeket a **tartomány és az URL-címeket.**</span><span class="sxs-lookup"><span data-stu-id="63cf6-155">Enter the required values in **Domain and URLs.**</span></span> <span data-ttu-id="63cf6-156">Ezeket az értékeket az alkalmazás gyártójának szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="63cf6-156">You should get these values from the application vendor.</span></span>

   1. <span data-ttu-id="63cf6-157">Az alkalmazás beállítása a Szolgáltató által kezdeményezett egyszeri Bejelentkezést, a bejelentkezési URL-címen nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="63cf6-157">To configure the application as SP-initiated SSO, the Sign on URL it’s a required value.</span></span> <span data-ttu-id="63cf6-158">Egyes alkalmazások azonosító is kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="63cf6-158">For some applications, the Identifier is also a required value.</span></span>

   2. <span data-ttu-id="63cf6-159">Az alkalmazás beállítása a kiállító terjesztési hely által kezdeményezett egyszeri Bejelentkezést, a válasz URL-CÍMEN nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="63cf6-159">To configure the application as IdP-initiated SSO, the Reply URL it’s a required value.</span></span> <span data-ttu-id="63cf6-160">Egyes alkalmazások azonosító is kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="63cf6-160">For some applications, the Identifier is also a required value.</span></span>

10. <span data-ttu-id="63cf6-161">**Választható lehetőség:** kattintson **megjelenítése speciális URL-beállításainak** Ha meg szeretné tekinteni a nem szükséges értékeket.</span><span class="sxs-lookup"><span data-stu-id="63cf6-161">**Optional:** click **Show advanced URL settings** if you want to see the non-required values.</span></span>

11. <span data-ttu-id="63cf6-162">Az a **felhasználói attribútumok**, válassza ki a felhasználók egyedi azonosítója a **felhasználói azonosító** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="63cf6-162">In the **User attributes**, select the unique identifier for your users in the **User Identifier** dropdown.</span></span>

12. <span data-ttu-id="63cf6-163">**Választható lehetőség:** kattintson **nézet és egyéb felhasználói attribútumok szerkesztése** kell küldeni a alkalmazását a SAML-jogkivonat felhasználói bejelentkezés attribútumok szerkesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="63cf6-163">**Optional:** click **View and edit all other user attributes** to edit the attributes to be sent to the application in the SAML token when user sign in.</span></span>

   <span data-ttu-id="63cf6-164">Attribútum hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="63cf6-164">To add an attribute:</span></span>

   1. <span data-ttu-id="63cf6-165">Kattintson a **Hozzáadás attribútum**.</span><span class="sxs-lookup"><span data-stu-id="63cf6-165">click **Add attribute**.</span></span> <span data-ttu-id="63cf6-166">Adja meg a **neve** majd válassza a **érték** a legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="63cf6-166">Enter the **Name** and the select the **Value** from the dropdown.</span></span>

   2. <span data-ttu-id="63cf6-167">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="63cf6-167">click **Save.**</span></span> <span data-ttu-id="63cf6-168">Az új attribútumot a táblázatban látható.</span><span class="sxs-lookup"><span data-stu-id="63cf6-168">You see the new attribute in the table.</span></span>

13. <span data-ttu-id="63cf6-169">Kattintson a **konfigurálása &lt;alkalmazásnév&gt;**  hozzáférés dokumentációjának egyszeri bejelentkezést az alkalmazás konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="63cf6-169">click **Configure &lt;application name&gt;** to access documentation on how to configure single sign-on in the application.</span></span> <span data-ttu-id="63cf6-170">Is hogy rendelkezik, a metadata URL-címek és az egyszeri bejelentkezés beállítása az alkalmazáshoz szükséges tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="63cf6-170">Also, you has the metadata URLs and certificate required to setup SSO with the application.</span></span>

14. <span data-ttu-id="63cf6-171">Kattintson a **mentése** a konfiguráció mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="63cf6-171">click **Save** to save the configuration.</span></span>

15. <span data-ttu-id="63cf6-172">Felhasználók hozzárendelése az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="63cf6-172">Assign users to the application.</span></span>

#### <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a><span data-ttu-id="63cf6-173">Válassza ki a felhasználói azonosítóját, és elküldi az alkalmazás felhasználói attribútumok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="63cf6-173">Select User Identifier and add user attributes to be sent to the application</span></span>

<span data-ttu-id="63cf6-174">Válassza ki a felhasználói azonosító, vagy adja hozzá a felhasználói attribútumok, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="63cf6-174">To select the User Identifier or add user attributes, follow the steps below:</span></span>

1.  <span data-ttu-id="63cf6-175">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="63cf6-175">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="63cf6-176">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="63cf6-176">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="63cf6-177">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="63cf6-177">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="63cf6-178">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="63cf6-178">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="63cf6-179">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="63cf6-179">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="63cf6-180">Ha nem látja az alkalmazás szeretné itt jelenik meg, akkor használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="63cf6-180">If you do not see the application you want to show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="63cf6-181">Válassza ki az alkalmazást, az egyszeri bejelentkezés konfigurálta.</span><span class="sxs-lookup"><span data-stu-id="63cf6-181">Select the application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="63cf6-182">Ha az alkalmazás betölt, kattintson a **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="63cf6-182">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="63cf6-183">Az a **felhasználói attribútumok** területen válassza ki a felhasználók egyedi azonosítója a **felhasználói azonosító** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="63cf6-183">Under the **User attributes** section, select the unique identifier for your users in the **User Identifier** dropdown.</span></span> <span data-ttu-id="63cf6-184">A kiválasztott beállítás a várt érték a felhasználó hitelesítésére az alkalmazásban egyezniük kell.</span><span class="sxs-lookup"><span data-stu-id="63cf6-184">The selected option needs to match the expected value in the application to authenticate the user.</span></span>

   >[!NOTE] 
   ><span data-ttu-id="63cf6-185">Az Azure AD a NameID attribútum (felhasználói azonosító) formátumát a megadott érték alapján kijelölése, vagy a SAML AuthRequest az alkalmazás által kért formátuma.</span><span class="sxs-lookup"><span data-stu-id="63cf6-185">Azure AD select the format for the NameID attribute (User Identifier) based on the value selected or the format requested by the application in the SAML AuthRequest.</span></span> <span data-ttu-id="63cf6-186">További információ látogasson el a [egyszeri bejelentkezés SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) NameIDPolicy szakaszban.</span><span class="sxs-lookup"><span data-stu-id="63cf6-186">For more information visit the article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under the section NameIDPolicy.</span></span>
   >
   >

9.  <span data-ttu-id="63cf6-187">Felhasználói attribútumok hozzáadásához kattintson **nézet és egyéb felhasználói attribútumok szerkesztése** kell küldeni a alkalmazását a SAML-jogkivonat felhasználói bejelentkezés attribútumok szerkesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="63cf6-187">To add user attributes, click **View and edit all other user attributes** to edit the attributes to be sent to the application in the SAML token when user sign in.</span></span>

   <span data-ttu-id="63cf6-188">Attribútum hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="63cf6-188">To add an attribute:</span></span>

   1. <span data-ttu-id="63cf6-189">Kattintson a **Hozzáadás attribútum**.</span><span class="sxs-lookup"><span data-stu-id="63cf6-189">click **Add attribute**.</span></span> <span data-ttu-id="63cf6-190">Adja meg a **neve** majd válassza a **érték** a legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="63cf6-190">Enter the **Name** and the select the **Value** from the dropdown.</span></span>

   2. <span data-ttu-id="63cf6-191">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="63cf6-191">click **Save.**</span></span> <span data-ttu-id="63cf6-192">Látni fogja az új attribútumot a táblában.</span><span class="sxs-lookup"><span data-stu-id="63cf6-192">You will see the new attribute in the table.</span></span>

#### <a name="download-the-azure-ad-metadata-or-certificate"></a><span data-ttu-id="63cf6-193">Az Azure AD-metaadatok vagy a tanúsítvány letöltése</span><span class="sxs-lookup"><span data-stu-id="63cf6-193">Download the Azure AD metadata or certificate</span></span>

<span data-ttu-id="63cf6-194">Az alkalmazás metaadatainak vagy tanúsítvány letöltéséhez az Azure AD, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="63cf6-194">To download the application metadata or certificate from Azure AD, follow the steps below:</span></span>

1.  <span data-ttu-id="63cf6-195">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="63cf6-195">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="63cf6-196">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="63cf6-196">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="63cf6-197">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="63cf6-197">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="63cf6-198">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="63cf6-198">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="63cf6-199">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="63cf6-199">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="63cf6-200">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="63cf6-200">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="63cf6-201">Válassza ki az alkalmazást, az egyszeri bejelentkezés konfigurálta.</span><span class="sxs-lookup"><span data-stu-id="63cf6-201">Select the application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="63cf6-202">Ha az alkalmazás betölt, kattintson a **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="63cf6-202">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="63cf6-203">Ugrás a **SAML-aláíró tanúsítványa** területen, majd kattintson **letöltése** oszlop értékét.</span><span class="sxs-lookup"><span data-stu-id="63cf6-203">Go to **SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="63cf6-204">Attól függően, hogy milyen az alkalmazáshoz az szükséges, az egyszeri bejelentkezés konfigurálása lásd: a metaadatok XML-kód letöltése beállítás, vagy a tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="63cf6-204">Depending on what the application requires configuring single sign-on, you see either the option to download the Metadata XML or the Certificate.</span></span>

    <span data-ttu-id="63cf6-205">Az Azure AD nem biztosít a metaadatok beolvasása URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="63cf6-205">Azure AD doesn’t provide a URL to get the metadata.</span></span> <span data-ttu-id="63cf6-206">A metaadatok XML-fájlként csak olvasható.</span><span class="sxs-lookup"><span data-stu-id="63cf6-206">The metadata can only be retrieved as a XML file.</span></span>

### <a name="how-to-configure-federated-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="63cf6-207">Összevont egyszeri bejelentkezés nem galéria alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="63cf6-207">How to configure federated single sign-on for a non-gallery application</span></span>

<span data-ttu-id="63cf6-208">Beállíthat egy nem galéria-alkalmazást, akkor kell rendelkeznie az Azure AD premium és az alkalmazás támogatja az SAML 2.0-s.</span><span class="sxs-lookup"><span data-stu-id="63cf6-208">To configure a non-gallery application, you need to have Azure AD premium and the application supports SAML 2.0.</span></span> <span data-ttu-id="63cf6-209">Az Azure AD-verziókkal kapcsolatos további információkért látogasson el a [az Azure AD árképzési](https://azure.microsoft.com/pricing/details/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="63cf6-209">For more information about Azure AD versions, visit [Azure AD pricing](https://azure.microsoft.com/pricing/details/active-directory/).</span></span>

-   [<span data-ttu-id="63cf6-210">Az alkalmazás metaadatai értékeinek konfigurálása az Azure AD (bejelentkezési URL-cím, az azonosítója, válasz URL-címe)</span><span class="sxs-lookup"><span data-stu-id="63cf6-210">Configure the application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>](#configuring-single-sign-on)

-   [<span data-ttu-id="63cf6-211">Válassza ki a felhasználói azonosítóját, és elküldi az alkalmazás felhasználói attribútumok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="63cf6-211">Select User Identifier and add user attributes to be sent to the application</span></span>](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [<span data-ttu-id="63cf6-212">Az Azure AD-metaadatok és a tanúsítvány lekérése</span><span class="sxs-lookup"><span data-stu-id="63cf6-212">Retrieve Azure AD metadata and certificate</span></span>](#download-the-azure-ad-metadata-or-certificate)

-   [<span data-ttu-id="63cf6-213">Konfigurálja az Azure AD metaadatok értékeket az alkalmazásban (bejelentkezési URL-címet, a kibocsátó, a kijelentkezési URL-cím és a tanúsítvány)</span><span class="sxs-lookup"><span data-stu-id="63cf6-213">Configure Azure AD metadata values in the application (Sign on URL, Issuer, Logout URL and certificate)</span></span>](#configuring-single-sign-on)

#### <a name="configure-the-applications-metadata-values-in-azure-ad-sign-on-url-identifier-reply-url"></a><span data-ttu-id="63cf6-214">Az alkalmazás metaadatai értékeinek konfigurálása az Azure AD (bejelentkezési URL-cím, az azonosítója, válasz URL-címe)</span><span class="sxs-lookup"><span data-stu-id="63cf6-214">Configure the application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>

<span data-ttu-id="63cf6-215">Egyszeri bejelentkezés egy alkalmazás, amely nincs az Azure AD-katalógus konfigurálásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="63cf6-215">To configure single sign-on for an application that is not in the Azure AD gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="63cf6-216">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="63cf6-216">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="63cf6-217">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="63cf6-217">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="63cf6-218">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="63cf6-218">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="63cf6-219">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="63cf6-219">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="63cf6-220">Kattintson a **Hozzáadás** gombra a jobb felső sarkában a **vállalati alkalmazások** panelen.</span><span class="sxs-lookup"><span data-stu-id="63cf6-220">click the **Add** button at the top-right corner on the **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="63cf6-221">Kattintson a **nem-gyűjtemény alkalmazás** a a **saját alkalmazás felvétele** szakasz.</span><span class="sxs-lookup"><span data-stu-id="63cf6-221">click **Non-gallery application** in the **Add your own app** section.</span></span>

7.  <span data-ttu-id="63cf6-222">Adja meg az alkalmazás nevét a **neve** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="63cf6-222">Enter the name of the application in the **Name** textbox.</span></span>

8.  <span data-ttu-id="63cf6-223">Kattintson a **Hozzáadás** gombra, vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="63cf6-223">Click **Add** button, to add the application.</span></span>

9.  <span data-ttu-id="63cf6-224">Ha az alkalmazás betölt, kattintson a **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="63cf6-224">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

10. <span data-ttu-id="63cf6-225">Válassza ki **SAML-alapú bejelentkezés** a a **mód** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="63cf6-225">Select **SAML-based Sign-on** in the **Mode** dropdown.</span></span>

11. <span data-ttu-id="63cf6-226">Adja meg a szükséges értékeket a **tartomány és az URL-címeket.**</span><span class="sxs-lookup"><span data-stu-id="63cf6-226">Enter the required values in **Domain and URLs.**</span></span> <span data-ttu-id="63cf6-227">Ezeket az értékeket az alkalmazás gyártójának szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="63cf6-227">You should get these values from the application vendor.</span></span>

   1. <span data-ttu-id="63cf6-228">Az alkalmazás beállítása a kiállító terjesztési hely által kezdeményezett egyszeri Bejelentkezést, írja be a válasz URL-CÍMEN és azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="63cf6-228">To configure the application as IdP-initiated SSO, enter the Reply URL and the Identifier.</span></span>

   2.  <span data-ttu-id="63cf6-229">**Választható lehetőség:** SP által kezdeményezett egyszeri Bejelentkezést, a bejelentkezési URL-címen, az alkalmazás konfigurálásához szükséges érték.</span><span class="sxs-lookup"><span data-stu-id="63cf6-229">**Optional:** To configure the application as SP-initiated SSO, the Sign on URL it’s a required value.</span></span>

12. <span data-ttu-id="63cf6-230">Az a **felhasználói attribútumok**, válassza ki a felhasználók egyedi azonosítója a **felhasználói azonosító** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="63cf6-230">In the **User attributes**, select the unique identifier for your users in the **User Identifier** dropdown.</span></span>

13. <span data-ttu-id="63cf6-231">**Választható lehetőség:** kattintson **nézet és egyéb felhasználói attribútumok szerkesztése** kell küldeni a alkalmazását a SAML-jogkivonat felhasználói bejelentkezés attribútumok szerkesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="63cf6-231">**Optional:** click **View and edit all other user attributes** to edit the attributes to be sent to the application in the SAML token when user sign in.</span></span>

   <span data-ttu-id="63cf6-232">Attribútum hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="63cf6-232">To add an attribute:</span></span>

   1. <span data-ttu-id="63cf6-233">Kattintson a **Hozzáadás attribútum**.</span><span class="sxs-lookup"><span data-stu-id="63cf6-233">click **Add attribute**.</span></span> <span data-ttu-id="63cf6-234">Adja meg a **neve** majd válassza a **érték** a legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="63cf6-234">Enter the **Name** and the select the **Value** from the dropdown.</span></span>

   2. <span data-ttu-id="63cf6-235">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="63cf6-235">Click **Save.**</span></span> <span data-ttu-id="63cf6-236">Az új attribútumot a táblázatban látható.</span><span class="sxs-lookup"><span data-stu-id="63cf6-236">You see the new attribute in the table.</span></span>

14. <span data-ttu-id="63cf6-237">Kattintson a **konfigurálása &lt;alkalmazásnév&gt;**  hozzáférés dokumentációjának egyszeri bejelentkezést az alkalmazás konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="63cf6-237">click **Configure &lt;application name&gt;** to access documentation on how to configure single sign-on in the application.</span></span> <span data-ttu-id="63cf6-238">Is hogy rendelkezik, az Azure AD URL-címek és az alkalmazáshoz szükséges tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="63cf6-238">Also, you has Azure AD URLs and certificate required for the application.</span></span>

#### <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a><span data-ttu-id="63cf6-239">Válassza ki a felhasználói azonosítóját, és elküldi az alkalmazás felhasználói attribútumok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="63cf6-239">Select User Identifier and add user attributes to be sent to the application</span></span>

<span data-ttu-id="63cf6-240">Válassza ki a felhasználói azonosító, vagy adja hozzá a felhasználói attribútumok, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="63cf6-240">To select the User Identifier or add user attributes, follow the steps below:</span></span>

1.  <span data-ttu-id="63cf6-241">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="63cf6-241">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="63cf6-242">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="63cf6-242">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="63cf6-243">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="63cf6-243">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="63cf6-244">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="63cf6-244">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="63cf6-245">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="63cf6-245">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="63cf6-246">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="63cf6-246">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="63cf6-247">Válassza ki az alkalmazást, az egyszeri bejelentkezés konfigurálta.</span><span class="sxs-lookup"><span data-stu-id="63cf6-247">Select the application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="63cf6-248">Ha az alkalmazás betölt, kattintson a **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="63cf6-248">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="63cf6-249">Az a **felhasználói attribútumok** területen válassza ki a felhasználók egyedi azonosítója a **felhasználói azonosító** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="63cf6-249">Under the **User attributes** section, select the unique identifier for your users in the **User Identifier** dropdown.</span></span> <span data-ttu-id="63cf6-250">A kiválasztott beállítás a várt érték a felhasználó hitelesítésére az alkalmazásban egyezniük kell.</span><span class="sxs-lookup"><span data-stu-id="63cf6-250">The selected option needs to match the expected value in the application to authenticate the user.</span></span>

   >[!NOTE] 
   ><span data-ttu-id="63cf6-251">Az Azure AD a NameID attribútum (felhasználói azonosító) formátumát a megadott érték alapján kijelölése, vagy a SAML AuthRequest az alkalmazás által kért formátuma.</span><span class="sxs-lookup"><span data-stu-id="63cf6-251">Azure AD select the format for the NameID attribute (User Identifier) based on the value selected or the format requested by the application in the SAML AuthRequest.</span></span> <span data-ttu-id="63cf6-252">További információ látogasson el a [egyszeri bejelentkezés SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) NameIDPolicy szakaszban.</span><span class="sxs-lookup"><span data-stu-id="63cf6-252">For more information visit the article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under the section NameIDPolicy.</span></span>
   >
   >

9.  <span data-ttu-id="63cf6-253">Felhasználói attribútumok hozzáadásához kattintson **nézet és egyéb felhasználói attribútumok szerkesztése** kell küldeni a alkalmazását a SAML-jogkivonat felhasználói bejelentkezés attribútumok szerkesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="63cf6-253">To add user attributes, click **View and edit all other user attributes** to edit the attributes to be sent to the application in the SAML token when user sign in.</span></span>

   <span data-ttu-id="63cf6-254">Attribútum hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="63cf6-254">To add an attribute:</span></span>

   1. <span data-ttu-id="63cf6-255">Kattintson a **Hozzáadás attribútum**.</span><span class="sxs-lookup"><span data-stu-id="63cf6-255">click **Add attribute**.</span></span> <span data-ttu-id="63cf6-256">Adja meg a **neve** majd válassza a **érték** a legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="63cf6-256">Enter the **Name** and the select the **Value** from the dropdown.</span></span>

   2. <span data-ttu-id="63cf6-257">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="63cf6-257">Click **Save.**</span></span> <span data-ttu-id="63cf6-258">Az új attribútumot a táblázatban látható.</span><span class="sxs-lookup"><span data-stu-id="63cf6-258">You see the new attribute in the table.</span></span>

#### <a name="download-the-azure-ad-metadata-or-certificate"></a><span data-ttu-id="63cf6-259">Az Azure AD-metaadatok vagy a tanúsítvány letöltése</span><span class="sxs-lookup"><span data-stu-id="63cf6-259">Download the Azure AD metadata or certificate</span></span>

<span data-ttu-id="63cf6-260">Az alkalmazás metaadatainak vagy tanúsítvány letöltéséhez az Azure AD, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="63cf6-260">To download the application metadata or certificate from Azure AD, follow the steps below:</span></span>

1.  <span data-ttu-id="63cf6-261">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="63cf6-261">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="63cf6-262">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="63cf6-262">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="63cf6-263">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="63cf6-263">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="63cf6-264">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="63cf6-264">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="63cf6-265">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="63cf6-265">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="63cf6-266">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="63cf6-266">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="63cf6-267">Válassza ki az alkalmazást, az egyszeri bejelentkezés konfigurálta.</span><span class="sxs-lookup"><span data-stu-id="63cf6-267">Select the application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="63cf6-268">Ha az alkalmazás betölt, kattintson a **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="63cf6-268">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="63cf6-269">Ugrás a **SAML-aláíró tanúsítványa** területen, majd kattintson **letöltése** oszlop értékét.</span><span class="sxs-lookup"><span data-stu-id="63cf6-269">Go to **SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="63cf6-270">Attól függően, hogy milyen az alkalmazáshoz az szükséges, az egyszeri bejelentkezés konfigurálása lásd: a metaadatok XML-kód letöltése beállítás, vagy a tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="63cf6-270">Depending on what the application requires configuring single sign-on, you see either the option to download the Metadata XML or the Certificate.</span></span>

<span data-ttu-id="63cf6-271">Az Azure AD nem biztosít a metaadatok beolvasása URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="63cf6-271">Azure AD doesn’t provide a URL to get the metadata.</span></span> <span data-ttu-id="63cf6-272">A metaadatok XML-fájlként csak olvasható.</span><span class="sxs-lookup"><span data-stu-id="63cf6-272">The metadata can only be retrieved as a XML file.</span></span>

### <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="63cf6-273">Jelszó egyszeri bejelentkezés az Azure AD gyűjtemény alkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="63cf6-273">How to configure password single sign-on for an Azure AD gallery application</span></span>

<span data-ttu-id="63cf6-274">Egy alkalmazás kell az Azure AD-galériából konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="63cf6-274">To configure an application from the Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="63cf6-275">Alkalmazás hozzáadása az Azure AD-galériából</span><span class="sxs-lookup"><span data-stu-id="63cf6-275">Add an application from the Azure AD gallery</span></span>](#add-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="63cf6-276">Állítsa be az alkalmazását jelszó egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="63cf6-276">Configure the application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

#### <a name="add-an-application-from-the-azure-ad-gallery"></a><span data-ttu-id="63cf6-277">Alkalmazás hozzáadása az Azure AD-galériából</span><span class="sxs-lookup"><span data-stu-id="63cf6-277">Add an application from the Azure AD gallery</span></span>

<span data-ttu-id="63cf6-278">Hozzáadhat egy alkalmazást az Azure AD-gyűjteményből, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="63cf6-278">To add an application from the Azure AD Gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="63cf6-279">Nyissa meg a [Azure Portal](https://portal.azure.com) , és jelentkezzen be egy **globális rendszergazda** vagy **társadminisztrátornak**</span><span class="sxs-lookup"><span data-stu-id="63cf6-279">Open the [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="63cf6-280">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="63cf6-280">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="63cf6-281">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="63cf6-281">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="63cf6-282">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="63cf6-282">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="63cf6-283">Kattintson a **Hozzáadás** gombra a jobb felső sarkában a **vállalati alkalmazások** panelen.</span><span class="sxs-lookup"><span data-stu-id="63cf6-283">click the **Add** button at the top-right corner on the **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="63cf6-284">Az a **adjon meg egy nevet** a szövegmező a **hozzáadása a gyűjteményből** területen írja be az alkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="63cf6-284">In the **Enter a name** textbox from the **Add from the gallery** section, type the name of the application.</span></span>

7.  <span data-ttu-id="63cf6-285">Válassza ki az alkalmazást, az egyszeri bejelentkezés konfigurálni szeretné.</span><span class="sxs-lookup"><span data-stu-id="63cf6-285">Select the application you want to configure for single sign-on.</span></span>

8.  <span data-ttu-id="63cf6-286">Ad hozzá az alkalmazást, mielőtt a nevét módosíthatja a **neve** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="63cf6-286">Before adding the application, you can change its name from the **Name** textbox.</span></span>

9.  <span data-ttu-id="63cf6-287">Kattintson a **Hozzáadás** gombra, vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="63cf6-287">Click **Add** button, to add the application.</span></span>

<span data-ttu-id="63cf6-288">Egy rövid időszak után el az alkalmazás konfigurációs panelen láthatók.</span><span class="sxs-lookup"><span data-stu-id="63cf6-288">After a short period, you be able to see the application’s configuration blade.</span></span>

#### <a name="configure-the-application-for-password-single-sign-on"></a><span data-ttu-id="63cf6-289">Állítsa be az alkalmazását jelszó egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="63cf6-289">Configure the application for password single sign-on</span></span>

<span data-ttu-id="63cf6-290">Egyszeri bejelentkezés egy alkalmazás konfigurálásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="63cf6-290">To configure single sign-on for an application, follow the steps below:</span></span>

1.  <span data-ttu-id="63cf6-291">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="63cf6-291">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="63cf6-292">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="63cf6-292">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="63cf6-293">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="63cf6-293">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="63cf6-294">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="63cf6-294">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="63cf6-295">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="63cf6-295">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="63cf6-296">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="63cf6-296">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="63cf6-297">Válassza ki az egyszeri bejelentkezés konfigurálni kívánt alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="63cf6-297">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="63cf6-298">Ha az alkalmazás betölt, kattintson a **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="63cf6-298">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="63cf6-299">Válassza ki a módot **jelszóalapú bejelentkezés.**</span><span class="sxs-lookup"><span data-stu-id="63cf6-299">Select the mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="63cf6-300">[Felhasználók hozzárendelése az alkalmazás](#how-to-assign-a-user-to-an-application-directly).</span><span class="sxs-lookup"><span data-stu-id="63cf6-300">[Assign users to the application](#how-to-assign-a-user-to-an-application-directly).</span></span>

10. <span data-ttu-id="63cf6-301">Emellett is megadhatja a felhasználó nevében hitelesítő adatok a felhasználók a sorok kijelöléséhez és parancsával **frissítéséhez szükséges hitelesítő adatokat** , majd gépelje be a felhasználónevet és jelszót a felhasználók nevében.</span><span class="sxs-lookup"><span data-stu-id="63cf6-301">Additionally, you can also provide credentials on behalf of the user by selecting the rows of the users and clicking on **Update Credentials** and entering the username and password on behalf of the users.</span></span> <span data-ttu-id="63cf6-302">Ellenkező esetben megkérdezi a felhasználókat a hitelesítő adatok magukat az indítás után.</span><span class="sxs-lookup"><span data-stu-id="63cf6-302">Otherwise, users be prompted to enter the credentials themselves upon launch.</span></span>

### <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="63cf6-303">Jelszó egyszeri bejelentkezés nem galéria alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="63cf6-303">How to configure password single sign-on for a non-gallery application</span></span>

<span data-ttu-id="63cf6-304">Egy alkalmazás kell az Azure AD-galériából konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="63cf6-304">To configure an application from the Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="63cf6-305">Nem galéria alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="63cf6-305">Add a non-gallery application</span></span>](#add-a-non-gallery-application)

-   [<span data-ttu-id="63cf6-306">Állítsa be az alkalmazását jelszó egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="63cf6-306">Configure the application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

#### <a name="add-a-non-gallery-application"></a><span data-ttu-id="63cf6-307">Nem galéria alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="63cf6-307">Add a non-gallery application</span></span>

<span data-ttu-id="63cf6-308">Hozzáadhat egy alkalmazást az Azure AD-gyűjteményből, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="63cf6-308">To add an application from the Azure AD Gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="63cf6-309">Nyissa meg a [Azure Portal](https://portal.azure.com) , és jelentkezzen be egy **globális rendszergazda** vagy **társadminisztrátor**.</span><span class="sxs-lookup"><span data-stu-id="63cf6-309">Open the [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**.</span></span>

2.  <span data-ttu-id="63cf6-310">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="63cf6-310">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="63cf6-311">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="63cf6-311">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="63cf6-312">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="63cf6-312">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="63cf6-313">Kattintson a **Hozzáadás** gombra a jobb felső sarkában a **vállalati alkalmazások** panelen.</span><span class="sxs-lookup"><span data-stu-id="63cf6-313">click the **Add** button at the top-right corner on the **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="63cf6-314">Kattintson a **nem-gyűjtemény alkalmazás.**</span><span class="sxs-lookup"><span data-stu-id="63cf6-314">click **Non-gallery application.**</span></span>

7.  <span data-ttu-id="63cf6-315">Adja meg az alkalmazás nevét a **neve** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="63cf6-315">Enter the name of your application in the **Name** textbox.</span></span> <span data-ttu-id="63cf6-316">Válassza ki **hozzáadása.**</span><span class="sxs-lookup"><span data-stu-id="63cf6-316">Select **Add.**</span></span>

<span data-ttu-id="63cf6-317">Egy rövid időszak után el az alkalmazás konfigurációs panelen láthatók.</span><span class="sxs-lookup"><span data-stu-id="63cf6-317">After a short period, you be able to see the application’s configuration blade.</span></span>

#### <a name="configure-the-application-for-password-single-sign-on"></a><span data-ttu-id="63cf6-318">Állítsa be az alkalmazását jelszó egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="63cf6-318">Configure the application for password single sign-on</span></span>

<span data-ttu-id="63cf6-319">Egyszeri bejelentkezés egy alkalmazás konfigurálásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="63cf6-319">To configure single sign-on for an application, follow the steps below:</span></span>

1.  <span data-ttu-id="63cf6-320">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="63cf6-320">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="63cf6-321">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="63cf6-321">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="63cf6-322">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="63cf6-322">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="63cf6-323">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="63cf6-323">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="63cf6-324">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="63cf6-324">click **All Applications** to view a list of all your applications.</span></span>

    1.  <span data-ttu-id="63cf6-325">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="63cf6-325">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="63cf6-326">Válassza ki az egyszeri bejelentkezés konfigurálni kívánt alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="63cf6-326">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="63cf6-327">Ha az alkalmazás betölt, kattintson a **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="63cf6-327">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="63cf6-328">Válassza ki a módot **jelszóalapú bejelentkezés.**</span><span class="sxs-lookup"><span data-stu-id="63cf6-328">Select the mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="63cf6-329">Adja meg a **bejelentkezési URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="63cf6-329">Enter the **Sign-on URL**.</span></span> <span data-ttu-id="63cf6-330">Ez az URL-CÍMÉT ahol adja meg a felhasználók a felhasználónevével és jelszavával bejelentkezni.</span><span class="sxs-lookup"><span data-stu-id="63cf6-330">This is the URL where users enter their username and password to sign in to.</span></span> <span data-ttu-id="63cf6-331">Győződjön meg arról, a mezők bejelentkezési URL-címen láthatók.</span><span class="sxs-lookup"><span data-stu-id="63cf6-331">Ensure the sign in fields are visible at the URL.</span></span>

10. <span data-ttu-id="63cf6-332">[Felhasználók hozzárendelése az alkalmazás](#how-to-assign-a-user-to-an-application-directly).</span><span class="sxs-lookup"><span data-stu-id="63cf6-332">[Assign users to the application](#how-to-assign-a-user-to-an-application-directly).</span></span>

11. <span data-ttu-id="63cf6-333">Emellett is megadhatja a felhasználó nevében hitelesítő adatok a felhasználók a sorok kijelöléséhez és parancsával **frissítéséhez szükséges hitelesítő adatokat** , majd gépelje be a felhasználónevet és jelszót a felhasználók nevében.</span><span class="sxs-lookup"><span data-stu-id="63cf6-333">Additionally, you can also provide credentials on behalf of the user by selecting the rows of the users and clicking on **Update Credentials** and entering the username and password on behalf of the users.</span></span> <span data-ttu-id="63cf6-334">Ellenkező esetben megkérdezi a felhasználókat a hitelesítő adatok magukat az indítás után.</span><span class="sxs-lookup"><span data-stu-id="63cf6-334">Otherwise, users be prompted to enter the credentials themselves upon launch.</span></span>

## <a name="problems-related-to-assigning-applications-to-users"></a><span data-ttu-id="63cf6-335">Felhasználók hozzárendelése kapcsolatos problémák</span><span class="sxs-lookup"><span data-stu-id="63cf6-335">Problems related to assigning applications to users</span></span>

<span data-ttu-id="63cf6-336">A felhasználó nem lehet annak, hogy egy alkalmazás a hozzáférési panelen, mert az alkalmazás nem vannak rendelve.</span><span class="sxs-lookup"><span data-stu-id="63cf6-336">A user may not be seeing an application on their Access Panel because they are not assigned to the application.</span></span> <span data-ttu-id="63cf6-337">Az alábbiakban néhány ellenőrizze módjai:</span><span class="sxs-lookup"><span data-stu-id="63cf6-337">Below are some ways to check:</span></span>

-   [<span data-ttu-id="63cf6-338">Ellenőrizze, hogy ha a felhasználó hozzá van rendelve az alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="63cf6-338">Check if a user is assigned to the application</span></span>](#check-if-a-user-is-assigned-to-the-application)

-   [<span data-ttu-id="63cf6-339">A felhasználó közvetlenül egy alkalmazás hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="63cf6-339">How to assign a user to an application directly</span></span>](#how-to-assign-a-user-to-an-application-directly)

-   [<span data-ttu-id="63cf6-340">Ellenőrizze, hogy ha a felhasználó hozzá van rendelve az alkalmazáshoz kapcsolódó licenc</span><span class="sxs-lookup"><span data-stu-id="63cf6-340">Check if a user is assigned to a license related to the application</span></span>](#check-if-a-user-is-under-a-license-related-to-the-application)

-   [<span data-ttu-id="63cf6-341">Egy felhasználói licenc hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="63cf6-341">How to assign a license to a user</span></span>](#how-to-assign-a-user-a-license)

### <a name="check-if-a-user-is-assigned-to-the-application"></a><span data-ttu-id="63cf6-342">Ellenőrizze, hogy ha a felhasználó hozzá van rendelve az alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="63cf6-342">Check if a user is assigned to the application</span></span>

<span data-ttu-id="63cf6-343">Ellenőrizze, hogy ha a felhasználó hozzá van rendelve az alkalmazást, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="63cf6-343">To check if a user is assigned to the application, follow the steps below:</span></span>

1.  <span data-ttu-id="63cf6-344">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="63cf6-344">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="63cf6-345">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="63cf6-345">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="63cf6-346">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="63cf6-346">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="63cf6-347">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="63cf6-347">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="63cf6-348">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="63cf6-348">click **All Applications** to view a list of all your applications.</span></span>

6.  <span data-ttu-id="63cf6-349">**Keresési** a szóban forgó alkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="63cf6-349">**Search** for the name of the application in question.</span></span>

7.  <span data-ttu-id="63cf6-350">Kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="63cf6-350">click **Users and groups**.</span></span>

8.  <span data-ttu-id="63cf6-351">Ellenőrizze, hogy ha a felhasználó hozzá van rendelve az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="63cf6-351">Check to see if your user is assigned to the application.</span></span>

   * <span data-ttu-id="63cf6-352">Ha nem, kövesse a "How to felhasználó hozzárendelése egy alkalmazás közvetlenül" ehhez.</span><span class="sxs-lookup"><span data-stu-id="63cf6-352">If not follow the steps in “How to assign a user to an application directly” to do so.</span></span>

### <a name="how-to-assign-a-user-to-an-application-directly"></a><span data-ttu-id="63cf6-353">A felhasználó közvetlenül egy alkalmazás hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="63cf6-353">How to assign a user to an application directly</span></span>

<span data-ttu-id="63cf6-354">Hozzárendelése egy vagy több felhasználó alkalmazás közvetlenül, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="63cf6-354">To assign one or more users to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="63cf6-355">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda**.</span><span class="sxs-lookup"><span data-stu-id="63cf6-355">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator**.</span></span>

2.  <span data-ttu-id="63cf6-356">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="63cf6-356">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="63cf6-357">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="63cf6-357">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="63cf6-358">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="63cf6-358">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="63cf6-359">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="63cf6-359">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="63cf6-360">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="63cf6-360">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="63cf6-361">Válassza ki szeretné osztani a felhasználót, hogy a listában az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="63cf6-361">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="63cf6-362">Ha az alkalmazás betölt, kattintson **felhasználók és csoportok** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="63cf6-362">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="63cf6-363">Kattintson a **Hozzáadás** gombra kattint, a a **felhasználók és csoportok** nyissa meg a listában a **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="63cf6-363">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="63cf6-364">Kattintson a **felhasználók és csoportok** a választó a **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="63cf6-364">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="63cf6-365">Írja be a **teljes név** vagy **e-mail cím** érdekli hozzárendelése a felhasználó a **Keresés név vagy e-mail cím alapján** keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="63cf6-365">Type in the **full name** or **email address** of the user you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="63cf6-366">Vigye a **felhasználói** a listában, hogy láthatóvá váljon a **jelölőnégyzet**.</span><span class="sxs-lookup"><span data-stu-id="63cf6-366">Hover over the **user** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="63cf6-367">A felhasználói profil fénykép vagy adja hozzá a felhasználót emblémát jelölőnégyzetét, kattintson a **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="63cf6-367">Click the checkbox next to the user’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="63cf6-368">**Választható lehetőség:** Ha azt szeretné, hogy **egynél több felhasználó hozzáadása**, egy másik típus **teljes név** vagy **e-mail cím** be a **Keresés név vagy e-mail cím alapján** mező, és a jelölőnégyzet bejelölésével adja hozzá a felhasználót, hogy a **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="63cf6-368">**Optional:** If you would like to **add more than one user**, type in another **full name** or **email address** into the **Search by name or email address** search box, and click the checkbox to add this user to the **Selected** list.</span></span>

13. <span data-ttu-id="63cf6-369">Ha elkészült, válassza a felhasználók, kattintson a **válasszon** gombra kattintva vegye fel a listára a felhasználók és csoportok hozzá kell rendelni az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="63cf6-369">When you are finished selecting users, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="63cf6-370">**Választható lehetőség:** kattintson a **Szerepkörválasztás** a választó a **hozzáadása hozzárendelés** hozzárendelése a kiválasztott felhasználói szerepkör kiválasztása panel.</span><span class="sxs-lookup"><span data-stu-id="63cf6-370">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the users you have selected.</span></span>

15. <span data-ttu-id="63cf6-371">Kattintson a **hozzárendelése** gombra kattintva a kijelölt felhasználók az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="63cf6-371">Click the **Assign** button to assign the application to the selected users.</span></span>

<span data-ttu-id="63cf6-372">Rövid ideig a kijelölt felhasználók tudják elindítani ezeket az alkalmazásokat a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="63cf6-372">After a short period, the users you have selected be able to launch these applications in the Access Panel.</span></span>

### <a name="check-if-a-user-is-under-a-license-related-to-the-application"></a><span data-ttu-id="63cf6-373">Annak ellenőrzése, hogy a felhasználó a licenc, az alkalmazással kapcsolatos</span><span class="sxs-lookup"><span data-stu-id="63cf6-373">Check if a user is under a license related to the application</span></span>

<span data-ttu-id="63cf6-374">A felhasználó licenc-hozzárendeléseket ellenőrzéséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="63cf6-374">To check a user’s assigned licenses, follow the steps below:</span></span>

1.  <span data-ttu-id="63cf6-375">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="63cf6-375">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="63cf6-376">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="63cf6-376">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="63cf6-377">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="63cf6-377">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="63cf6-378">Kattintson a **felhasználók és csoportok** a navigációs menü.</span><span class="sxs-lookup"><span data-stu-id="63cf6-378">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="63cf6-379">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="63cf6-379">click **All users**.</span></span>

6.  <span data-ttu-id="63cf6-380">**Keresési** érdekli, felhasználó és **sorára kattintson** kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="63cf6-380">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="63cf6-381">Kattintson a **licencek** megtekintéséhez, amely licencek, a felhasználó jelenleg hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="63cf6-381">click **Licenses** to see which licenses the user currently has assigned.</span></span>

  * <span data-ttu-id="63cf6-382">Ha a felhasználó hozzá van rendelve egy Office licenc a engedélyezése első fél Office-alkalmazásokat jelennek meg a felhasználó hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="63cf6-382">If the user is assigned to an Office license this enable First Party Office applications to appear on the user’s Access Panel.</span></span>

### <a name="how-to-assign-a-user-a-license"></a><span data-ttu-id="63cf6-383">Egy felhasználói licenc hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="63cf6-383">How to assign a user a license</span></span> 

<span data-ttu-id="63cf6-384">A licenc hozzárendelése egy felhasználóhoz, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="63cf6-384">To assign a license to a user, follow the steps below:</span></span>

1.  <span data-ttu-id="63cf6-385">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="63cf6-385">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="63cf6-386">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="63cf6-386">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="63cf6-387">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="63cf6-387">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="63cf6-388">Kattintson a **felhasználók és csoportok** a navigációs menü.</span><span class="sxs-lookup"><span data-stu-id="63cf6-388">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="63cf6-389">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="63cf6-389">click **All users**.</span></span>

6.  <span data-ttu-id="63cf6-390">**Keresési** érdekli, felhasználó és **sorára kattintson** kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="63cf6-390">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="63cf6-391">Kattintson a **licencek** megtekintéséhez, amely licencek, a felhasználó jelenleg hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="63cf6-391">click **Licenses** to see which licenses the user currently has assigned.</span></span>

8.  <span data-ttu-id="63cf6-392">Kattintson a **hozzárendelése** gombra.</span><span class="sxs-lookup"><span data-stu-id="63cf6-392">click the **Assign** button.</span></span>

9.  <span data-ttu-id="63cf6-393">Válassza ki **egy vagy több termék** választható termékek közül.</span><span class="sxs-lookup"><span data-stu-id="63cf6-393">Select **one or more products** from the list of available products.</span></span>

10. <span data-ttu-id="63cf6-394">**Nem kötelező** kattintson a **hozzárendelés beállításai** elem termékek granularly hozzárendelni.</span><span class="sxs-lookup"><span data-stu-id="63cf6-394">**Optional** click the **assignment options** item to granularly assign products.</span></span> <span data-ttu-id="63cf6-395">Kattintson a **Ok** amikor ez befejeződik.</span><span class="sxs-lookup"><span data-stu-id="63cf6-395">Click **Ok** when this is completed.</span></span>

11. <span data-ttu-id="63cf6-396">Kattintson a **hozzárendelése** gomb a licencek hozzárendelése a felhasználóhoz.</span><span class="sxs-lookup"><span data-stu-id="63cf6-396">Click the **Assign** button to assign these licenses to this user.</span></span>

## <a name="problems-related-to-assigning-applications-to-groups"></a><span data-ttu-id="63cf6-397">Csoportok alkalmazásoké kapcsolatos problémák</span><span class="sxs-lookup"><span data-stu-id="63cf6-397">Problems related to assigning applications to groups</span></span>

<span data-ttu-id="63cf6-398">Egy felhasználó lehet annak, hogy egy alkalmazás a hozzáférési Panel a mert, amelyet az alkalmazáshoz rendelt csoportba.</span><span class="sxs-lookup"><span data-stu-id="63cf6-398">A user may be seeing an application on their Access Panel because they are part of a group that has been assigned the application.</span></span> <span data-ttu-id="63cf6-399">Az alábbiakban néhány ellenőrizze módjai:</span><span class="sxs-lookup"><span data-stu-id="63cf6-399">Below are some ways to check:</span></span>

-   [<span data-ttu-id="63cf6-400">A felhasználói csoporttagság ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="63cf6-400">Check a user’s group memberships</span></span>](#check-a-users-group-memberships)

-   [<span data-ttu-id="63cf6-401">Egy alkalmazás egy csoporthoz közvetlenül hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="63cf6-401">How to assign an application to a group directly</span></span>](#how-to-assign-an-application-to-a-group-directly)

-   [<span data-ttu-id="63cf6-402">Annak ellenőrzése, hogy a felhasználó csoporthoz hozzárendelt licenc</span><span class="sxs-lookup"><span data-stu-id="63cf6-402">Check if a user is part of group assigned to a license</span></span>](#check-if-a-user-is-part-of-group-assigned-to-a-license)

-   [<span data-ttu-id="63cf6-403">A licenc hozzárendelése egy csoportot</span><span class="sxs-lookup"><span data-stu-id="63cf6-403">How to assign a license to a group</span></span>](#how-to-assign-a-license-to-a-group)

### <a name="check-a-users-group-memberships"></a><span data-ttu-id="63cf6-404">A felhasználói csoporttagság ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="63cf6-404">Check a user’s group memberships</span></span>

<span data-ttu-id="63cf6-405">Ellenőrizze a csoport tagságát, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="63cf6-405">To check a group’s membership, follow the steps below:</span></span>

1.  <span data-ttu-id="63cf6-406">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="63cf6-406">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="63cf6-407">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="63cf6-407">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="63cf6-408">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="63cf6-408">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="63cf6-409">Kattintson a **felhasználók és csoportok** a navigációs menü.</span><span class="sxs-lookup"><span data-stu-id="63cf6-409">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="63cf6-410">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="63cf6-410">click **All users**.</span></span>

6.  <span data-ttu-id="63cf6-411">**Keresési** érdekli, felhasználó és **sorára kattintson** kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="63cf6-411">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="63cf6-412">Kattintson a **csoportok**.</span><span class="sxs-lookup"><span data-stu-id="63cf6-412">click **Groups**.</span></span>

8.  <span data-ttu-id="63cf6-413">Ellenőrizze, hogy a felhasználók egy csoportjának az alkalmazás része.</span><span class="sxs-lookup"><span data-stu-id="63cf6-413">Check to see if your user is part of a Group assigned to the application.</span></span>

  * <span data-ttu-id="63cf6-414">Ha a felhasználó a csoportból eltávolítani kívánt **sorára kattintson** a csoport- és select tulajdonosával.</span><span class="sxs-lookup"><span data-stu-id="63cf6-414">If you want to remove the user from the group, **click the row** of the group and select delete.</span></span>

### <a name="how-to-assign-an-application-to-a-group-directly"></a><span data-ttu-id="63cf6-415">Egy alkalmazás egy csoporthoz közvetlenül hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="63cf6-415">How to assign an application to a group directly</span></span>

<span data-ttu-id="63cf6-416">Közvetlenül egy alkalmazás hozzárendelése egy vagy több csoportot, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="63cf6-416">To assign one or more groups to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="63cf6-417">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda**.</span><span class="sxs-lookup"><span data-stu-id="63cf6-417">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator**.</span></span>

2.  <span data-ttu-id="63cf6-418">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="63cf6-418">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="63cf6-419">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="63cf6-419">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="63cf6-420">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="63cf6-420">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="63cf6-421">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="63cf6-421">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="63cf6-422">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="63cf6-422">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="63cf6-423">Válassza ki szeretné osztani a felhasználót, hogy a listában az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="63cf6-423">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="63cf6-424">Ha az alkalmazás betölt, kattintson **felhasználók és csoportok** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="63cf6-424">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="63cf6-425">Kattintson a **Hozzáadás** gombra kattint, a a **felhasználók és csoportok** nyissa meg a listában a **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="63cf6-425">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="63cf6-426">Kattintson a **felhasználók és csoportok** a választó a **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="63cf6-426">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="63cf6-427">Írja be a **teljes csoportnév** érdekli való hozzárendelése a csoport a **Keresés név vagy e-mail cím alapján** keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="63cf6-427">Type in the **full group name** of the group you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="63cf6-428">Vigye a **csoport** a listában, hogy láthatóvá váljon a **jelölőnégyzet**.</span><span class="sxs-lookup"><span data-stu-id="63cf6-428">Hover over the **group** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="63cf6-429">A csoport profilképet vagy adja hozzá a felhasználót emblémát jelölőnégyzetét, kattintson a **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="63cf6-429">Click the checkbox next to the group’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="63cf6-430">**Nem kötelező:** Ha azt szeretné, hogy **egynél több csoport hozzáadása**, egy másik típus **teljes csoportnév** be a **Keresés név vagy e-mail cím alapján** mező, és kattintson a jelölőnégyzetbe, a csoport hozzáadása a **kijelölt** lista.</span><span class="sxs-lookup"><span data-stu-id="63cf6-430">**Optional:** If you would like to **add more than one group**, type in another **full group name** into the **Search by name or email address** search box, and click the checkbox to add this group to the **Selected** list.</span></span>

13. <span data-ttu-id="63cf6-431">Ha befejezte a csoportok kiválasztásával, kattintson a **válasszon** gombra kattintva vegye fel a listára a felhasználók és csoportok hozzá kell rendelni az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="63cf6-431">When you are finished selecting groups, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="63cf6-432">**Választható lehetőség:** kattintson a **Szerepkörválasztás** a választó a **hozzáadása hozzárendelés** panelt, és válassza ki a szerepkör hozzárendelése a kijelölt csoportok.</span><span class="sxs-lookup"><span data-stu-id="63cf6-432">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the groups you have selected.</span></span>

15. <span data-ttu-id="63cf6-433">Kattintson a **hozzárendelése** gombra az alkalmazás a kiválasztott csoportok számára.</span><span class="sxs-lookup"><span data-stu-id="63cf6-433">Click the **Assign** button to assign the application to the selected groups.</span></span>

<span data-ttu-id="63cf6-434">Rövid ideig a kijelölt felhasználók tudják elindítani ezeket az alkalmazásokat a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="63cf6-434">After a short period, the users you have selected be able to launch these applications in the Access Panel.</span></span>

### <a name="check-if-a-user-is-part-of-group-assigned-to-a-license"></a><span data-ttu-id="63cf6-435">Annak ellenőrzése, hogy a felhasználó csoporthoz hozzárendelt licenc</span><span class="sxs-lookup"><span data-stu-id="63cf6-435">Check if a user is part of group assigned to a license</span></span>

1.  <span data-ttu-id="63cf6-436">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="63cf6-436">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="63cf6-437">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="63cf6-437">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="63cf6-438">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="63cf6-438">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="63cf6-439">Kattintson a **felhasználók és csoportok** a navigációs menü.</span><span class="sxs-lookup"><span data-stu-id="63cf6-439">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="63cf6-440">Kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="63cf6-440">click **All users**.</span></span>

6.  <span data-ttu-id="63cf6-441">**Keresési** érdekli, felhasználó és **sorára kattintson** kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="63cf6-441">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="63cf6-442">Kattintson a **csoportok**.</span><span class="sxs-lookup"><span data-stu-id="63cf6-442">click **Groups**.</span></span>

8.  <span data-ttu-id="63cf6-443">egy adott csoport sorára kattintson.</span><span class="sxs-lookup"><span data-stu-id="63cf6-443">click the row of a specific group.</span></span>

9.  <span data-ttu-id="63cf6-444">Kattintson a **licencek** megtekintéséhez, amely licencek, a csoport van rendelve.</span><span class="sxs-lookup"><span data-stu-id="63cf6-444">click **Licenses** to see which licenses the group has assigned to it.</span></span>

   * <span data-ttu-id="63cf6-445">Ha a csoport egy Office-licencet, ezzel lehetővé teheti a bizonyos első fél Office alkalmazások jelennek meg a felhasználó hozzáférési Panel van hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="63cf6-445">If the group is assigned to an Office license this may enable certain First Party Office applications to appear on the user’s Access Panel.</span></span>

### <a name="how-to-assign-a-license-to-a-group"></a><span data-ttu-id="63cf6-446">A licenc hozzárendelése egy csoportot</span><span class="sxs-lookup"><span data-stu-id="63cf6-446">How to assign a license to a group</span></span>

<span data-ttu-id="63cf6-447">A licenc hozzárendelése egy csoporthoz, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="63cf6-447">To assign a license to a group, follow the steps below:</span></span>

1.  <span data-ttu-id="63cf6-448">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="63cf6-448">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="63cf6-449">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="63cf6-449">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="63cf6-450">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="63cf6-450">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="63cf6-451">Kattintson a **felhasználók és csoportok** a navigációs menü.</span><span class="sxs-lookup"><span data-stu-id="63cf6-451">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="63cf6-452">Kattintson a **összes csoport**.</span><span class="sxs-lookup"><span data-stu-id="63cf6-452">click **All groups**.</span></span>

6.  <span data-ttu-id="63cf6-453">**Keresési** érdekli csoport és **sorára kattintson** kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="63cf6-453">**Search** for the group you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="63cf6-454">Kattintson a **licencek** megtekintéséhez, amely licencek, a csoport jelenleg hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="63cf6-454">click **Licenses** to see which licenses the group currently has assigned.</span></span>

8.  <span data-ttu-id="63cf6-455">Kattintson a **hozzárendelése** gombra.</span><span class="sxs-lookup"><span data-stu-id="63cf6-455">click the **Assign** button.</span></span>

9.  <span data-ttu-id="63cf6-456">Válassza ki **egy vagy több termék** választható termékek közül.</span><span class="sxs-lookup"><span data-stu-id="63cf6-456">Select **one or more products** from the list of available products.</span></span>

10. <span data-ttu-id="63cf6-457">**Nem kötelező** kattintson a **hozzárendelés beállításai** elem termékek granularly hozzárendelni.</span><span class="sxs-lookup"><span data-stu-id="63cf6-457">**Optional** click the **assignment options** item to granularly assign products.</span></span> <span data-ttu-id="63cf6-458">Kattintson a **Ok** amikor ez befejeződik.</span><span class="sxs-lookup"><span data-stu-id="63cf6-458">Click **Ok** when this is completed.</span></span>

11. <span data-ttu-id="63cf6-459">Kattintson a **hozzárendelése** gomb a licencek hozzárendelése ehhez a csoporthoz.</span><span class="sxs-lookup"><span data-stu-id="63cf6-459">Click the **Assign** button to assign these licenses to this group.</span></span> <span data-ttu-id="63cf6-460">Ez eltarthat egy ideig, attól függően, hogy méretét és összetettségét, a csoport.</span><span class="sxs-lookup"><span data-stu-id="63cf6-460">This may take a long time, depending on the size and complexity of the group.</span></span>

>[!NOTE]
><span data-ttu-id="63cf6-461">Ehhez gyorsabb, érdemes lehet ideiglenesen rendel egy licencet a felhasználó közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="63cf6-461">To do this faster, consider temporarily assigning a license to the user directly.</span></span> 
>
>

## <a name="next-steps"></a><span data-ttu-id="63cf6-462">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="63cf6-462">Next steps</span></span>
[<span data-ttu-id="63cf6-463">Új felhasználók hozzáadása az Azure Active Directoryhoz</span><span class="sxs-lookup"><span data-stu-id="63cf6-463">Add new users to Azure Active Directory</span></span>](active-directory-users-create-azure-portal.md)

