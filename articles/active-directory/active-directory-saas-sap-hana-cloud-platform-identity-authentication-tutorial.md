---
title: "Oktatóanyag: Azure Active Directory-integráció SAP HANA felhő Platform identitás hitelesítéssel |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és az SAP HANA felhőalapú Platform identitás hitelesítés között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1c1320d1-7ba4-4b5f-926f-4996b44d9b5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 7799bf03cc6705f805a48f329a265a3d84bed55f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform-identity-authentication"></a><span data-ttu-id="f10cf-103">Oktatóanyag: Azure Active Directory-integráció SAP HANA felhő Platform identitás hitelesítéssel</span><span class="sxs-lookup"><span data-stu-id="f10cf-103">Tutorial: Azure Active Directory integration with SAP HANA Cloud Platform Identity Authentication</span></span>

<span data-ttu-id="f10cf-104">Ebben az oktatóanyagban elsajátíthatja SAP HANA felhőalapú Platform identitás hitelesítés integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f10cf-104">In this tutorial, you learn how to integrate SAP HANA Cloud Platform Identity Authentication with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="f10cf-105">SAP HANA felhőalapú Platform identitás hitelesítés hozzáférési SAP alkalmazásokhoz az Azure AD használatával a fő IdP IdP proxy-ként használatos.</span><span class="sxs-lookup"><span data-stu-id="f10cf-105">SAP HANA Cloud Platform Identity Authentication is used as a proxy IdP to access SAP applications using Azure AD as the main IdP.</span></span>

<span data-ttu-id="f10cf-106">SAP HANA felhőalapú Platform identitás hitelesítés integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="f10cf-106">Integrating SAP HANA Cloud Platform Identity Authentication with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="f10cf-107">Megadhatja a SAP alkalmazáshoz hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="f10cf-107">You can control in Azure AD who has access to SAP application</span></span>
- <span data-ttu-id="f10cf-108">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett az alkalmazások egyszeri bejelentkezés (SSO) az Azure AD-fiókok az SAP</span><span class="sxs-lookup"><span data-stu-id="f10cf-108">You can enable your users to automatically get signed-on to SAP applications single sign-on (SSO) with their Azure AD accounts</span></span>
- <span data-ttu-id="f10cf-109">Kezelheti a fiókokat, egy központi helyen - a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="f10cf-109">You can manage your accounts in one central location - the Azure classic portal</span></span>

<span data-ttu-id="f10cf-110">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f10cf-110">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="f10cf-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f10cf-111">Prerequisites</span></span>

<span data-ttu-id="f10cf-112">SAP HANA felhőalapú Platform identitás hitelesítés az Azure AD-integrációs konfigurál, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="f10cf-112">To configure Azure AD integration with SAP HANA Cloud Platform Identity Authentication, you need the following items:</span></span>

- <span data-ttu-id="f10cf-113">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="f10cf-113">An Azure AD subscription</span></span>
- <span data-ttu-id="f10cf-114">A **SAP HANA felhőalapú Platform identitás hitelesítés** SSO előfizetés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="f10cf-114">A **SAP HANA Cloud Platform Identity Authentication** SSO enabled subscription</span></span>


>[!NOTE] 
><span data-ttu-id="f10cf-115">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="f10cf-115">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>
>

<span data-ttu-id="f10cf-116">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="f10cf-116">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f10cf-117">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="f10cf-117">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="f10cf-118">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, beszerezheti a [egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f10cf-118">If you don't have an Azure AD trial environment, you can get a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f10cf-119">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="f10cf-119">Scenario description</span></span>
<span data-ttu-id="f10cf-120">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="f10cf-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span>

<span data-ttu-id="f10cf-121">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="f10cf-121">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f10cf-122">SAP HANA felhőalapú Platform identitás hitelesítés hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="f10cf-122">Adding SAP HANA Cloud Platform Identity Authentication from the gallery</span></span>
2. <span data-ttu-id="f10cf-123">Azure AD egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f10cf-123">Configuring and testing Azure AD SSO</span></span>

<span data-ttu-id="f10cf-124">Technikai részleteket ról, mielőtt elengedhetetlen megérteni a fogalmakat, nézze meg fog.</span><span class="sxs-lookup"><span data-stu-id="f10cf-124">Before diving into the technical details, it is vital to understand the concepts you're going to look at.</span></span> <span data-ttu-id="f10cf-125">Az SAP HANA felhőalapú Platform identitás hitelesítés és az Azure Active Directory összevonási lehetővé teszi több alkalmazáshoz vagy szolgáltatáshoz (mint egy IdP) AAD védi az SAP-alkalmazásokkal és szolgáltatásokkal SAP HANA felhő Platform identitás által védett egyszeri bejelentkezés megvalósítása Hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="f10cf-125">The SAP HANA Cloud Platform Identity Authentication and Azure Active Directory federation enables you to implement SSO across applications or services protected by AAD (as an IdP) with SAP applications and services protected by SAP HANA Cloud Platform Identity Authentication.</span></span>

<span data-ttu-id="f10cf-126">Jelenleg SAP HANA felhőalapú Platform identitás hitelesítés úgy működik, mint egy Proxy identitásszolgáltató SAP-alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="f10cf-126">Currently, SAP HANA Cloud Platform Identity Authentication acts as a Proxy Identity Provider to SAP-applications.</span></span> <span data-ttu-id="f10cf-127">Az Azure Active Directory pedig úgy működik, mint a kezdő identitásszolgáltató az ehhez a telepítéshez.</span><span class="sxs-lookup"><span data-stu-id="f10cf-127">Azure Active Directory in turn acts as the leading Identity Provider in this setup.</span></span> 

<span data-ttu-id="f10cf-128">A következő ábra szemlélteti ezt:</span><span class="sxs-lookup"><span data-stu-id="f10cf-128">The following diagram illustrates this:</span></span>    

![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/architecture-01.png)

<span data-ttu-id="f10cf-130">Ez a beállítás a megbízható alkalmazások az Azure Active Directoryban az SAP HANA felhőalapú Platform identitás hitelesítés bérlő lesz beállítva.</span><span class="sxs-lookup"><span data-stu-id="f10cf-130">With this setup, your SAP HANA Cloud Platform Identity Authentication tenant will be configured as a trusted application in Azure Active Directory.</span></span> 

<span data-ttu-id="f10cf-131">Az SAP HANA felhőalapú Platform identitás hitelesítés felügyeleti konzolon SAP alkalmazások és szolgáltatások, így a védeni kívánt ezt követően konfigurálni!</span><span class="sxs-lookup"><span data-stu-id="f10cf-131">All SAP applications and services you want to protect through this way are subsequently configured in the SAP HANA Cloud Platform Identity Authentication management console!</span></span>

<span data-ttu-id="f10cf-132">Ez azt jelenti, hogy SAP alkalmazásokhoz és szolgáltatásokhoz való hozzáférés engedélyezési kell kerül sor az SAP HANA felhőalapú Platform identitás hitelesítés (figyelésekor engedélyezési konfigurálása az Azure Active Directoryban) telepítéshez.</span><span class="sxs-lookup"><span data-stu-id="f10cf-132">This means that authorization for granting access to SAP applications and services needs to take place in SAP HANA Cloud Platform Identity Authentication for such a setup (as opposed to configuring authorization in Azure Active Directory).</span></span>

<span data-ttu-id="f10cf-133">Az Azure Active Directory piactéren keresztül alkalmazásként SAP HANA felhőalapú Platform identitás hitelesítés konfigurálásával irányuló beállítása szükséges az egyes jogcímek nem kell egy érvényes előállításához szükséges SAML helyességi feltételek és átalakítások / SAP-alkalmazásokból hitelesítésére szolgáló jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="f10cf-133">By configuring SAP HANA Cloud Platform Identity Authentication as an application through the Azure Active Directory Marketplace, you don't need to take care of configuring needed individual claims / SAML assertions and transformations needed to produce a valid authentication token for SAP applications.</span></span>

>[!NOTE] 
><span data-ttu-id="f10cf-134">Webes egyszeri bejelentkezés jelenleg csak a két fél által tesztelték.</span><span class="sxs-lookup"><span data-stu-id="f10cf-134">Currently Web SSO has been tested by both parties, only.</span></span> <span data-ttu-id="f10cf-135">Alkalmazás-API vagy API-API kommunikációhoz szükséges adatfolyamok kell működnie, de nem lettek tesztelve, még.</span><span class="sxs-lookup"><span data-stu-id="f10cf-135">Flows needed for App-to-API or API-to-API communication should work but have not been tested, yet.</span></span> <span data-ttu-id="f10cf-136">Azok a soron következő tevékenységek részeként kell vizsgálni.</span><span class="sxs-lookup"><span data-stu-id="f10cf-136">They will be tested as part of subsequent activities.</span></span>
>

## <a name="add-sap-hana-cloud-platform-identity-authentication-from-the-gallery"></a><span data-ttu-id="f10cf-137">SAP HANA felhőalapú Platform identitás hitelesítés hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="f10cf-137">Add SAP HANA Cloud Platform Identity Authentication from the gallery</span></span>
<span data-ttu-id="f10cf-138">Az Azure AD integrálása a SAP HANA felhőalapú Platform identitás hitelesítés konfigurálásához szüksége SAP HANA felhőalapú Platform identitás hitelesítés hozzáadása a kezelt SaaS-alkalmazások listáját a gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="f10cf-138">To configure the integration of SAP HANA Cloud Platform Identity Authentication into Azure AD, you need to add SAP HANA Cloud Platform Identity Authentication from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="f10cf-139">**SAP HANA felhőalapú Platform identitás hitelesítés hozzáadása a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f10cf-139">**To add SAP HANA Cloud Platform Identity Authentication from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="f10cf-140">Az a [ **Azure Management portal**](https://portal.azure.com), kattintson a bal oldali navigációs panelen, **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f10cf-140">In the [**Azure Management portal**](https://portal.azure.com), on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f10cf-142">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="f10cf-142">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="f10cf-143">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f10cf-143">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="f10cf-145">Kattintson a **Hozzáadás** gombra a párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="f10cf-145">Click **Add** button on the top of the dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="f10cf-147">Írja be a keresőmezőbe, **SAP HANA felhőalapú Platform identitás hitelesítés**.</span><span class="sxs-lookup"><span data-stu-id="f10cf-147">In the search box, type **SAP HANA Cloud Platform Identity Authentication**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_01.png)

5. <span data-ttu-id="f10cf-149">Az eredmények panelen válassza ki a **SAP HANA felhőalapú Platform identitás hitelesítés**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f10cf-149">In the results panel, select **SAP HANA Cloud Platform Identity Authentication**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_02.png)


##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="f10cf-151">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f10cf-151">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="f10cf-152">Ebben a szakaszban, tesztelése és konfigurálása Azure AD SSO SAP HANA felhőalapú Platform identitás hitelesítés "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="f10cf-152">In this section, you configure and test Azure AD SSO with SAP HANA Cloud Platform Identity Authentication based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="f10cf-153">Az egyszeri bejelentkezés működjön az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó SAP HANA felhőalapú Platform identitás hitelesítés a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="f10cf-153">For SSO to work, Azure AD needs to know what the counterpart user in SAP HANA Cloud Platform Identity Authentication is to a user in Azure AD.</span></span> <span data-ttu-id="f10cf-154">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a SAP HANA felhőalapú Platform identitás hitelesítés közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="f10cf-154">In other words, a link relationship between an Azure AD user and the related user in SAP HANA Cloud Platform Identity Authentication needs to be established.</span></span>

<span data-ttu-id="f10cf-155">A hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** az SAP HANA felhőalapú Platform identitás hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="f10cf-155">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in SAP HANA Cloud Platform Identity Authentication.</span></span>

<span data-ttu-id="f10cf-156">Az Azure AD SSO SAP HANA felhőalapú Platform identitás hitelesítés tesztelése és konfigurálása, végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="f10cf-156">To configure and test Azure AD SSO with SAP HANA Cloud Platform Identity Authentication, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="f10cf-157">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="f10cf-157">**[Configuring Azure AD single sign-on](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="f10cf-158">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="f10cf-158">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f10cf-159">**[Egy SAP HANA felhőalapú Platform identitás hitelesítés tesztfelhasználó létrehozása](#creating-a-sap-hana-cloud-platform-identity-authentication-test-user)**  - való Britta Simon megfelelője a egy SAP HANA felhőalapú Platform identitás hitelesítés, amely csatolva van rá, hogy az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="f10cf-159">**[Creating a SAP HANA Cloud Platform Identity Authentication test user](#creating-a-sap-hana-cloud-platform-identity-authentication-test-user)** - to have a counterpart of Britta Simon in SAP HANA Cloud Platform Identity Authentication that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="f10cf-160">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="f10cf-160">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f10cf-161">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="f10cf-161">**[Testing single sign-on](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-sso"></a><span data-ttu-id="f10cf-162">Az Azure AD-egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f10cf-162">Configuring Azure AD SSO</span></span>

<span data-ttu-id="f10cf-163">Ebben a szakaszban engedélyezze az Azure AD egyszeri Bejelentkezést az Azure felügyeleti portálon, és az SAP HANA felhőalapú Platform identitás hitelesítés alkalmazásban egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="f10cf-163">In this section, you enable Azure AD SSO in the Azure Management portal and configure single sign-on in your SAP HANA Cloud Platform Identity Authentication application.</span></span>

<span data-ttu-id="f10cf-164">SAP HANA felhő Platform identitás hitelesítési kérelem vár a SAML helyességi feltételek egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="f10cf-164">SAP HANA Cloud Platform Identity Authentication application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="f10cf-165">Ezek az attribútumok értékének kezelheti a "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="f10cf-165">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="f10cf-166">Az alábbi képernyőfelvételen látható egy példa a.</span><span class="sxs-lookup"><span data-stu-id="f10cf-166">The following screenshot shows an example for this.</span></span>

![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_03.png)

<span data-ttu-id="f10cf-168">**SAP HANA felhőalapú Platform identitás hitelesítés az Azure AD SSO konfigurálásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f10cf-168">**To configure Azure AD SSO with SAP HANA Cloud Platform Identity Authentication, perform the following steps:**</span></span>

1. <span data-ttu-id="f10cf-169">Az Azure felügyeleti portálján a a **SAP HANA felhőalapú Platform identitás hitelesítés** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="f10cf-169">In the Azure Management portal, on the **SAP HANA Cloud Platform Identity Authentication** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="f10cf-171">A a **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** a engedélyezése az egyszeri bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="f10cf-171">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása][5]

3. <span data-ttu-id="f10cf-173">Az a **felhasználói attribútumok** a szakasz a **egyszeri bejelentkezés** párbeszédpanel, ha az SAP-alkalmazást például a "Keresztnév" attribútumot vár.</span><span class="sxs-lookup"><span data-stu-id="f10cf-173">In the **User Attributes** section on the **Single sign-on** dialog, if your SAP application expects an attribute for example "firstName".</span></span> <span data-ttu-id="f10cf-174">A SAML-jogkivonat attribútumok párbeszédpanelen adja hozzá a "Keresztnév" attribútumot.</span><span class="sxs-lookup"><span data-stu-id="f10cf-174">On the SAML token attributes dialog, add the "firstName" attribute.</span></span>
 1. <span data-ttu-id="f10cf-175">Kattintson a **Hozzáadás attribútum** megnyitásához a **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f10cf-175">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása][6]

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_05.png)
 2. <span data-ttu-id="f10cf-178">Az a **attribútumnév** szövegmező, írja be az attribútum neve "Keresztnév".</span><span class="sxs-lookup"><span data-stu-id="f10cf-178">In the **Attribute Name** textbox, type the attribute name "firstName".</span></span>
 3. <span data-ttu-id="f10cf-179">Az a **attribútumérték** listára, válassza ki a "user.givenname" attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="f10cf-179">From the **Attribute Value** list, select the attribute value "user.givenname".</span></span>
 4. <span data-ttu-id="f10cf-180">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="f10cf-180">Click **Ok**.</span></span>

4. <span data-ttu-id="f10cf-181">Az a **SAP HANA felhő Platform identitás hitelesítési tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="f10cf-181">On the **SAP HANA Cloud Platform Identity Authentication Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_06.png)
 1. <span data-ttu-id="f10cf-183">Az a **URL-cím bejelentkezési** szövegmező, írja be a bejelentkezési URL-címen az SAP-alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="f10cf-183">In the **Sign On URL** textbox, type the sign on URL for the SAP application.</span></span>
 2. <span data-ttu-id="f10cf-184">Az a **azonosító** szövegmező, írja be az értéket a következő mintát:`<entity-id>.accounts.ondemand.com`</span><span class="sxs-lookup"><span data-stu-id="f10cf-184">In the **Identifier** textbox, type the value following pattern: `<entity-id>.accounts.ondemand.com`</span></span> 
    * <span data-ttu-id="f10cf-185">Ha ez az érték nem tudja, kövesse az SAP HANA felhőalapú Platform identitás hitelesítés dokumentáció [bérlői SAML 2.0 konfigurációs](https://help.hana.ondemand.com/cloud_identity/frameset.htm?e81a19b0067f4646982d7200a8dab3ca.html).</span><span class="sxs-lookup"><span data-stu-id="f10cf-185">If you don't know this value, please follow the SAP HANA Cloud Platform Identity Authentication documentation on [Tenant SAML 2.0 Configuration](https://help.hana.ondemand.com/cloud_identity/frameset.htm?e81a19b0067f4646982d7200a8dab3ca.html).</span></span>

5. <span data-ttu-id="f10cf-186">A a **SAP HANA felhő Platform hitelesítési Identitáskonfigurációs** kattintson **SAP HANA felhő Platform identitás hitelesítés beállítása** megnyitásához **bejelentkezéskonfigurálása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f10cf-186">On the **SAP HANA Cloud Platform Identity Authentication Configuration** section, click **Configure SAP HANA Cloud Platform Identity Authentication** to open **Configure sign-on** dialog.</span></span> <span data-ttu-id="f10cf-187">Kattintson a **SAML XML metaadatok** , és mentse a fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="f10cf-187">Then, click on **SAML XML Metadata** and save the file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_07.png) 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_08.png)

6. <span data-ttu-id="f10cf-190">Az alkalmazáshoz konfigurált SSO, keresse fel SAP HANA felhő Platform identitás hitelesítési felügyeleti konzolon.</span><span class="sxs-lookup"><span data-stu-id="f10cf-190">To get SSO configured for your application, go to SAP HANA Cloud Platform Identity Authentication Administration Console.</span></span> <span data-ttu-id="f10cf-191">Az URL-címnek a következő mintát:`https://<tenant-id>.accounts.ondemand.com/admin`</span><span class="sxs-lookup"><span data-stu-id="f10cf-191">The URL has the following pattern: `https://<tenant-id>.accounts.ondemand.com/admin`</span></span>
 * <span data-ttu-id="f10cf-192">Majd kövesse a dokumentáció SAP HANA felhő Platform identitás hitelesítés [konfigurálása a Microsoft Azure AD vállalati identitás-szolgáltatóként SAP HANA felhő Platform identitás hitelesítési](https://help.hana.ondemand.com/cloud_identity/frameset.htm?626b17331b4d4014b8790d3aea70b240.html).</span><span class="sxs-lookup"><span data-stu-id="f10cf-192">Then, follow the documentation on SAP HANA Cloud Platform Identity Authentication to [Configure Microsoft Azure AD as Corporate Identity Provider at SAP HANA Cloud Platform Identity Authentication](https://help.hana.ondemand.com/cloud_identity/frameset.htm?626b17331b4d4014b8790d3aea70b240.html).</span></span> 

7. <span data-ttu-id="f10cf-193">Az Azure felügyeleti portálon kattintson **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="f10cf-193">In the Azure Management portal, click **Save** button.</span></span>
8. <span data-ttu-id="f10cf-194">Csak akkor, ha azt szeretné, adja hozzá, és engedélyezze az egyszeri Bejelentkezést egy másik SAP-alkalmazással, továbbra is az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="f10cf-194">Continue the following steps only if you want to add and enable SSO for another SAP application.</span></span> <span data-ttu-id="f10cf-195">Ismételje meg a "Hozzáadás SAP HANA felhő Platform identitás-hitelesítés a gyűjteményből" szakaszban hozzáadása az SAP HANA felhőalapú Platform identitás hitelesítés egy másik példánya.</span><span class="sxs-lookup"><span data-stu-id="f10cf-195">Repeat steps under the section “Adding SAP HANA Cloud Platform Identity Authentication from the gallery” to add another instance of SAP HANA Cloud Platform Identity Authentication.</span></span>
9. <span data-ttu-id="f10cf-196">Az Azure felügyeleti portálján a a **SAP HANA felhőalapú Platform identitás hitelesítés** alkalmazás integráció lapján, kattintson a **társított bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="f10cf-196">In the Azure Management portal, on the **SAP HANA Cloud Platform Identity Authentication** application integration page, click **Linked Sign-on**.</span></span>

    ![Csatolt bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/linked_sign_on.png)
10. <span data-ttu-id="f10cf-198">Ezt követően mentse a konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="f10cf-198">Then, save the configuration.</span></span>

>[!NOTE] 
><span data-ttu-id="f10cf-199">Az új alkalmazás előző SAP alkalmazást SSO konfigurációját fogja használni.</span><span class="sxs-lookup"><span data-stu-id="f10cf-199">The new application will leverage the SSO configuration for the previous SAP application.</span></span> <span data-ttu-id="f10cf-200">Ellenőrizze, hogy az azonos vállalati identitás-szolgáltatóktól használhatja az SAP HANA felhő Platform identitás hitelesítési felügyeleti konzolon.</span><span class="sxs-lookup"><span data-stu-id="f10cf-200">Please make sure you use the same Corporate Identity Providers in the SAP HANA Cloud Platform Identity Authentication Administration Console.</span></span>
>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="f10cf-201">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="f10cf-201">Create an Azure AD test user</span></span>
<span data-ttu-id="f10cf-202">Ez a szakasz célja a tesztfelhasználó létrehozása az új portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="f10cf-202">The objective of this section is to create a test user in the new portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="f10cf-204">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f10cf-204">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="f10cf-205">Az a **Azure Management portal**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f10cf-205">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f10cf-207">Ugrás a **felhasználók és csoportok** kattintson **minden felhasználó** azon felhasználók listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="f10cf-207">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f10cf-209">Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f10cf-209">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f10cf-211">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="f10cf-211">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_04.png) 
  1. <span data-ttu-id="f10cf-213">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f10cf-213">In the **Name** textbox, type **BrittaSimon**.</span></span>
  2. <span data-ttu-id="f10cf-214">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f10cf-214">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>
  3. <span data-ttu-id="f10cf-215">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="f10cf-215">Select **Show Password** and write down the value of the **Password**.</span></span>
  4. <span data-ttu-id="f10cf-216">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="f10cf-216">Click **Create**.</span></span> 

### <a name="create-a-sap-hana-cloud-platform-identity-authentication-test-user"></a><span data-ttu-id="f10cf-217">SAP HANA felhőalapú Platform identitás hitelesítés tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="f10cf-217">Create a SAP HANA Cloud Platform Identity Authentication test user</span></span>

<span data-ttu-id="f10cf-218">Egy felhasználó létrehozása SAP HANA felhőalapú Platform identitás hitelesítés nincs szükségünk.</span><span class="sxs-lookup"><span data-stu-id="f10cf-218">You don't need to create an user on SAP HANA Cloud Platform Identity Authentication.</span></span> <span data-ttu-id="f10cf-219">Az Azure AD felhasználókhoz tartozó tárolóban lévő felhasználók számára az egyszeri bejelentkezési funkciók használhatja.</span><span class="sxs-lookup"><span data-stu-id="f10cf-219">Users who are in the Azure AD user store can use the SSO functionality.</span></span>

<span data-ttu-id="f10cf-220">SAP HANA felhőalapú Platform identitás hitelesítés az identitás-összevonási beállítás támogatja.</span><span class="sxs-lookup"><span data-stu-id="f10cf-220">SAP HANA Cloud Platform Identity Authentication supports the Identity Federation option.</span></span> <span data-ttu-id="f10cf-221">Ez a beállítás lehetővé teszi, hogy az alkalmazás Ha a vállalati identitásszolgáltató által hitelesített felhasználók létezik-e a tároló az SAP HANA felhő Platform identitás felhasználóhitelesítés.</span><span class="sxs-lookup"><span data-stu-id="f10cf-221">This option allows the application to check if the users authenticated by the corporate identity provider exist in the user store of SAP HANA Cloud Platform Identity Authentication.</span></span> 

<span data-ttu-id="f10cf-222">Az alapértelmezett beállítás, az identitás-összevonási beállítás le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="f10cf-222">In the default setting, the Identity Federation option is disabled.</span></span> <span data-ttu-id="f10cf-223">Identitás-összevonási engedélyezve van, ha csak a felhasználók az SAP HANA felhőalapú Platform identitás hitelesítés importált képesek hozzáférni az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="f10cf-223">If Identity Federation is enabled, only the users that are imported in SAP HANA Cloud Platform Identity Authentication are able to access the application.</span></span> 

<span data-ttu-id="f10cf-224">Engedélyezheti vagy tilthatja le az identitás-összevonási SAP HANA felhő Platform identitás hitelesítéssel kapcsolatos további információkért tekintse meg az identitás-összevonási engedélyezése SAP HANA felhő Platform identitás hitelesítéssel a [identitás-összevonás konfigurálása a tároló az SAP HANA felhő Platform identitás felhasználóhitelesítéssel. ](https://help.hana.ondemand.com/cloud_identity/frameset.htm?c029bbbaefbf4350af15115396ba14e2.html).</span><span class="sxs-lookup"><span data-stu-id="f10cf-224">For more information about how to enable or disable Identity Federation with SAP HANA Cloud Platform Identity Authentication, see Enable Identity Federation with SAP HANA Cloud Platform Identity Authentication in [Configure Identity Federation with the User Store of SAP HANA Cloud Platform Identity Authentication.](https://help.hana.ondemand.com/cloud_identity/frameset.htm?c029bbbaefbf4350af15115396ba14e2.html).</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="f10cf-225">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="f10cf-225">Assign the Azure AD test user</span></span>

<span data-ttu-id="f10cf-226">Ebben a szakaszban engedélyezze Britta Simon Azure egyszeri bejelentkezés nyújtó az SAP HANA felhő Platform identitás hitelesítés használatára.</span><span class="sxs-lookup"><span data-stu-id="f10cf-226">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to SAP HANA Cloud Platform Identity Authentication.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="f10cf-228">**Az SAP HANA felhőalapú Platform identitás hitelesítés Britta Simon rendeléséhez hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f10cf-228">**To assign Britta Simon to SAP HANA Cloud Platform Identity Authentication, perform the following steps:**</span></span>

1. <span data-ttu-id="f10cf-229">Az Azure felügyeleti portálra, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f10cf-229">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="f10cf-231">Az alkalmazások listában válassza ki a **SAP HANA felhőalapú Platform identitás hitelesítés**.</span><span class="sxs-lookup"><span data-stu-id="f10cf-231">In the applications list, select **SAP HANA Cloud Platform Identity Authentication**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_09.png)

3. <span data-ttu-id="f10cf-233">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="f10cf-233">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="f10cf-235">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="f10cf-235">Click **Add** button.</span></span> <span data-ttu-id="f10cf-236">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f10cf-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="f10cf-238">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="f10cf-238">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="f10cf-239">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f10cf-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f10cf-240">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f10cf-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    

### <a name="test-single-sign-on"></a><span data-ttu-id="f10cf-241">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="f10cf-241">Test single sign-on</span></span>

<span data-ttu-id="f10cf-242">Ebben a szakaszban a Azure AD SSO konfigurációját, a hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="f10cf-242">In this section, you test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="f10cf-243">Ha a hozzáférési Panel egy SAP HANA felhőalapú Platform identitás hitelesítés mozaik gombra kattint, akkor kell beolvasása automatikusan bejelentkezett az SAP HANA felhőalapú Platform identitás hitelesítés alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="f10cf-243">When you click the SAP HANA Cloud Platform Identity Authentication tile in the Access Panel, you should get automatically signed-on to your SAP HANA Cloud Platform Identity Authentication application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="f10cf-244">További források</span><span class="sxs-lookup"><span data-stu-id="f10cf-244">Additional resources</span></span>

* [<span data-ttu-id="f10cf-245">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="f10cf-245">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f10cf-246">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="f10cf-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_06.png

[100]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_203.png