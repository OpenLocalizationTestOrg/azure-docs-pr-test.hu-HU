---
title: "Oktatóanyag: Azure Active Directoryval integrált Procore SSO |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Procore SSO között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9818edd3-48c0-411d-b05a-3ec805eafb2e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: jeedes
ms.openlocfilehash: 042a41eaa6bb21670b4c12068f1b017222f64b99
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-procore-sso"></a><span data-ttu-id="dad05-103">Oktatóanyag: Azure Active Directoryval integrált Procore egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="dad05-103">Tutorial: Azure Active Directory integration with Procore SSO</span></span>

<span data-ttu-id="dad05-104">Ebben az oktatóanyagban elsajátíthatja Procore SSO integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="dad05-104">In this tutorial, you learn how to integrate Procore SSO with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="dad05-105">Procore SSO integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="dad05-105">Integrating Procore SSO with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="dad05-106">Megadhatja a Procore SSO hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="dad05-106">You can control in Azure AD who has access to Procore SSO</span></span>
- <span data-ttu-id="dad05-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a Procore SSO (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="dad05-107">You can enable your users to automatically get signed-on to Procore SSO (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="dad05-108">Kezelheti a fiókokat, egy központi helyen – az Azure felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="dad05-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="dad05-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="dad05-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

<!--## Overview

To enable single sign-on with Procore SSO, it must be configured to use Azure Active Directory as an identity provider. This guide provides information and tips on how to perform this configuration in Procore SSO.

>[!Note]: 
>This embedded guide is brand new in the new Azure portal, and we’d love to hear your thoughts. Use the Feedback ? button at the top of the portal to provide feedback. The older guide for using the [Azure classic portal](https://manage.windowsazure.com) to configure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a><span data-ttu-id="dad05-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="dad05-110">Prerequisites</span></span>

<span data-ttu-id="dad05-111">Az Azure AD-integrációs konfigurálásához Procore egyszeri bejelentkezési modellel, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="dad05-111">To configure Azure AD integration with Procore SSO, you need the following items:</span></span>

- <span data-ttu-id="dad05-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="dad05-112">An Azure AD subscription</span></span>
- <span data-ttu-id="dad05-113">Egy Procore SSO egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="dad05-113">A Procore SSO single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="dad05-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="dad05-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="dad05-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="dad05-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="dad05-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="dad05-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="dad05-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="dad05-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="dad05-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="dad05-118">Scenario description</span></span>
<span data-ttu-id="dad05-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="dad05-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="dad05-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="dad05-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="dad05-121">A gyűjteményből Procore SSO hozzáadása</span><span class="sxs-lookup"><span data-stu-id="dad05-121">Adding Procore SSO from the gallery</span></span>
2. <span data-ttu-id="dad05-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="dad05-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-procore-sso-from-the-gallery"></a><span data-ttu-id="dad05-123">A gyűjteményből Procore SSO hozzáadása</span><span class="sxs-lookup"><span data-stu-id="dad05-123">Adding Procore SSO from the gallery</span></span>
<span data-ttu-id="dad05-124">Az Azure AD integrálása a Procore SSO konfigurálásához kell hozzáadnia Procore SSO a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="dad05-124">To configure the integration of Procore SSO into Azure AD, you need to add Procore SSO from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="dad05-125">**A gyűjteményből Procore SSO hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="dad05-125">**To add Procore SSO from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="dad05-126">Az a  **[Azure felügyeleti portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="dad05-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="dad05-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="dad05-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="dad05-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="dad05-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="dad05-131">Kattintson a **Hozzáadás** gombra a párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="dad05-131">Click **Add** button on the top of the dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="dad05-133">Írja be a keresőmezőbe, **Procore SSO**.</span><span class="sxs-lookup"><span data-stu-id="dad05-133">In the search box, type **Procore SSO**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_search.png)

5. <span data-ttu-id="dad05-135">Az eredmények panelen válassza ki a **Procore SSO**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="dad05-135">In the results panel, select **Procore SSO**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="dad05-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="dad05-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="dad05-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés Procore SSO "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="dad05-138">In this section, you configure and test Azure AD single sign-on with Procore SSO based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="dad05-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Procore SSO a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="dad05-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Procore SSO is to a user in Azure AD.</span></span> <span data-ttu-id="dad05-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Procore SSO közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="dad05-140">In other words, a link relationship between an Azure AD user and the related user in Procore SSO needs to be established.</span></span>

<span data-ttu-id="dad05-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** Procore SSO a.</span><span class="sxs-lookup"><span data-stu-id="dad05-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Procore SSO.</span></span>

<span data-ttu-id="dad05-142">Az Azure AD az egyszeri bejelentkezés Procore egyszeri bejelentkezési modellel tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="dad05-142">To configure and test Azure AD single sign-on with Procore SSO, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="dad05-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="dad05-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="dad05-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="dad05-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="dad05-145">**[Procore SSO tesztfelhasználó létrehozása](#creating-a-procore-sso-test-user)**  - való egy megfelelője a Britta Simon Procore SSO, amely csatolva van rá, hogy az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="dad05-145">**[Creating a Procore SSO test user](#creating-a-procore-sso-test-user)** - to have a counterpart of Britta Simon in Procore SSO that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="dad05-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="dad05-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="dad05-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="dad05-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="dad05-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="dad05-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="dad05-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure felügyeleti portálon, és konfigurálása egyszeri bejelentkezéshez az Procore SSO-alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="dad05-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your Procore SSO application.</span></span>

<span data-ttu-id="dad05-150">**Konfigurálja az Azure AD az egyszeri bejelentkezés Procore egyszeri bejelentkezési modellel, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="dad05-150">**To configure Azure AD single sign-on with Procore SSO, perform the following steps:**</span></span>

1. <span data-ttu-id="dad05-151">Az Azure felügyeleti portálján a a **Procore SSO** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="dad05-151">In the Azure Management portal, on the **Procore SSO** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="dad05-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** a engedélyezése az egyszeri bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="dad05-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_samlbase.png)

3. <span data-ttu-id="dad05-155">Az a **Procore SSO-tartomány és az URL-címek** szakaszban, a felhasználó nem rendelkezik, az alkalmazás már előre integrálva van az Azure-ral bármely lépések végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="dad05-155">On the **Procore SSO Domain and URLs** section, the user does not have to perform any steps as the app is already pre-integrated with Azure.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_url.png)

4. <span data-ttu-id="dad05-157">Az a **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse az XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="dad05-157">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_certificate.png) 

5. <span data-ttu-id="dad05-159">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="dad05-159">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="dad05-161">A a **Procore SSO konfigurációs** kattintson **Procore SSO konfigurálása** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="dad05-161">On the **Procore SSO Configuration** section, click **Configure Procore SSO** to open **Configure sign-on** window.</span></span> <span data-ttu-id="dad05-162">Másolás a **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="dad05-162">Copy the **SAML Entity ID and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_configure.png) 

7. <span data-ttu-id="dad05-164">Egyszeri bejelentkezés konfigurálása **Procore SSO** ügyféloldali, jelentkezzen be rendszergazdaként a procore vállalati webhely.</span><span class="sxs-lookup"><span data-stu-id="dad05-164">To configure single sign-on on **Procore SSO** side, login to your procore company site as an administrator.</span></span>

8. <span data-ttu-id="dad05-165">Az eszközkészlet legördülő le, kattintson a **Admin** az SSO beállításai oldal megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="dad05-165">From the toolbox drop down, click on **Admin** to open the SSO settings page.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/procore_tool_admin.png)

9. <span data-ttu-id="dad05-167">Illessze be az értékeket a mezőkbe, lásd az alábbi-</span><span class="sxs-lookup"><span data-stu-id="dad05-167">Paste the values in the boxes as described below-</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/procore_setting_admin.png)    

    <span data-ttu-id="dad05-169">a.</span><span class="sxs-lookup"><span data-stu-id="dad05-169">a.</span></span> <span data-ttu-id="dad05-170">Az a **egyszeri bejelentkezési kibocsátó URL-cím** mezőbe illessze be az SAML-entitás azonosítója az Azure-portálon átmásolva.</span><span class="sxs-lookup"><span data-stu-id="dad05-170">In the **Single Sign On Issuer URL** box, paste the SAML Entity ID copied from the Azure portal.</span></span>

    <span data-ttu-id="dad05-171">b.</span><span class="sxs-lookup"><span data-stu-id="dad05-171">b.</span></span> <span data-ttu-id="dad05-172">Az a **SAML bejelentkezési cél URL-cím** mezőbe illessze be az Azure-portálon átmásolva a SAML-alapú egyszeri bejelentkezési URL-címe.</span><span class="sxs-lookup"><span data-stu-id="dad05-172">In the **SAML Sign On Target URL** box, paste the SAML Single Sign-On Service URL copied from the Azure portal.</span></span>

    <span data-ttu-id="dad05-173">c.</span><span class="sxs-lookup"><span data-stu-id="dad05-173">c.</span></span> <span data-ttu-id="dad05-174">Most nyissa meg a **metaadatainak XML-kódja** újabb Azure-portálról letöltött, és másolja a tanúsítvány nevű címkében **x.509**.</span><span class="sxs-lookup"><span data-stu-id="dad05-174">Now open the **Metadata XML** downloaded above from the Azure portal and copy the certficate in the tag named **X509Certificate**.</span></span> <span data-ttu-id="dad05-175">Illessze be a másolt érték a **egyszeri bejelentkezés x509 tanúsítvány** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="dad05-175">Paste the copied value into the **Single Sign On x509 Certificate** box.</span></span>

10. <span data-ttu-id="dad05-176">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="dad05-176">Click on **Save Changes**.</span></span>

11. <span data-ttu-id="dad05-177">Után ezeket a beállításokat, akkor el kell küldenie a **tartománynév** (pl. **contoso.com**) keresztül, amely jelentkezik be a Procore a [Procore támogatási csoport](https://support.procore.com/) és azok aktiválódik összevont egyszeri Bejelentkezéses ehhez a tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="dad05-177">After these settings, you needs to send the **domain name** (e.g **contoso.com**) through which you are logging into Procore to the [Procore Support team](https://support.procore.com/) and they will activate federated SSO for that domain.</span></span>

<!--### Next steps

To ensure users can sign-in to Procore SSO after it has been configured to use Azure Active Directory, review the following tasks and topics:

- User accounts must be pre-provisioned into Procore SSO prior to sign-in. To set this up, see Provisioning.
 
- Users must be assigned access to Procore SSO in Azure AD to sign-in. To assign users, see Users.
 
- To configure access polices for Procore SSO users, see Access Policies.
 
- For additional information on deploying single sign-on to users, see [this article](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="dad05-178">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="dad05-178">Creating an Azure AD test user</span></span>
<span data-ttu-id="dad05-179">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure felügyeleti portálján Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="dad05-179">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="dad05-181">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="dad05-181">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="dad05-182">Az a **Azure Management portal**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="dad05-182">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-procoresso-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="dad05-184">Ugrás a **felhasználók és csoportok** kattintson **minden felhasználó** azon felhasználók listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="dad05-184">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-procoresso-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="dad05-186">Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="dad05-186">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-procoresso-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="dad05-188">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="dad05-188">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-procoresso-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="dad05-190">a.</span><span class="sxs-lookup"><span data-stu-id="dad05-190">a.</span></span> <span data-ttu-id="dad05-191">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="dad05-191">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="dad05-192">b.</span><span class="sxs-lookup"><span data-stu-id="dad05-192">b.</span></span> <span data-ttu-id="dad05-193">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="dad05-193">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="dad05-194">c.</span><span class="sxs-lookup"><span data-stu-id="dad05-194">c.</span></span> <span data-ttu-id="dad05-195">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="dad05-195">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="dad05-196">d.</span><span class="sxs-lookup"><span data-stu-id="dad05-196">d.</span></span> <span data-ttu-id="dad05-197">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="dad05-197">Click **Create**.</span></span>
 
### <a name="creating-a-procore-sso-test-user"></a><span data-ttu-id="dad05-198">Procore SSO tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="dad05-198">Creating a Procore SSO test user</span></span>

<span data-ttu-id="dad05-199">Kérjük, kövesse a következő lépések végrehajtásával Procore tesztfelhasználó létrehozása az oldalon.</span><span class="sxs-lookup"><span data-stu-id="dad05-199">Please follow the below steps to create a Procore test user on their side.</span></span>

1. <span data-ttu-id="dad05-200">Rendszergazdaként a procore vállalati webhelyre való bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="dad05-200">Login to your procore company site as an administrator.</span></span>  

2. <span data-ttu-id="dad05-201">Az eszközkészlet legördülő le, kattintson a **Directory** a vállalati címtár lap megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="dad05-201">From the toolbox drop down, click on **Directory** to open the company directory page.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/Procore_sso_directory.png)

3. <span data-ttu-id="dad05-203">Kattintson a **hozzáadása egy személy** a képernyőt, és írja be a beállítást hajtsa végre a következő beállítások -</span><span class="sxs-lookup"><span data-stu-id="dad05-203">Click on **Add a Person** option to open the form and enter perform following options -</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/Procore_user_add.png)

    <span data-ttu-id="dad05-205">a.</span><span class="sxs-lookup"><span data-stu-id="dad05-205">a.</span></span> <span data-ttu-id="dad05-206">Az a **Utónév** szövegmezőhöz típus felhasználó utónevét például **Britta**.</span><span class="sxs-lookup"><span data-stu-id="dad05-206">In the **First Name** textbox, type user's first name like **Britta**.</span></span>

    <span data-ttu-id="dad05-207">b.</span><span class="sxs-lookup"><span data-stu-id="dad05-207">b.</span></span> <span data-ttu-id="dad05-208">Az a **Vezetéknév** szövegmező, például típus felhasználó vezetékneve **Simon**.</span><span class="sxs-lookup"><span data-stu-id="dad05-208">In the **Last name** textbox, type user's last name like **Simon**.</span></span>

    <span data-ttu-id="dad05-209">c.</span><span class="sxs-lookup"><span data-stu-id="dad05-209">c.</span></span> <span data-ttu-id="dad05-210">Az a **E-mail cím** szövegmezőhöz típusa a felhasználó e-mail címét, például  **BrittaSimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="dad05-210">In the **Email Address** textbox, type user's email address like **BrittaSimon@contoso.com**.</span></span>

    <span data-ttu-id="dad05-211">d.</span><span class="sxs-lookup"><span data-stu-id="dad05-211">d.</span></span> <span data-ttu-id="dad05-212">Válassza ki **engedély sablon** , **engedély sablon alkalmazása később**.</span><span class="sxs-lookup"><span data-stu-id="dad05-212">Select **Permission Template** as **Apply Permission Template Later**.</span></span>

    <span data-ttu-id="dad05-213">e.</span><span class="sxs-lookup"><span data-stu-id="dad05-213">e.</span></span> <span data-ttu-id="dad05-214">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="dad05-214">Click **Create**.</span></span>

4. <span data-ttu-id="dad05-215">Ellenőrizze, és frissítse az újonnan hozzáadott ismerős részleteit.</span><span class="sxs-lookup"><span data-stu-id="dad05-215">Check and update the details for the newly added contact.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/Procore_user_check.png)

5. <span data-ttu-id="dad05-217">Kattintson a **mentése és küldése Invitiation** (ha szükséges egy összehíváshoz mail keresztül) vagy **mentése** (Mentés közvetlenül) a felhasználói regisztráció befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="dad05-217">Click on **Save and Send Invitiation** (if an invite through mail is required) or **Save** (Save directly) to complete the user registration.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/Procore_user_save.png)    

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="dad05-219">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="dad05-219">Assigning the Azure AD test user</span></span>

<span data-ttu-id="dad05-220">Ebben a szakaszban engedélyezze Britta Simon által biztosított saját hozzáférés Procore SSO Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="dad05-220">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Procore SSO.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="dad05-222">**Britta Simon hozzárendelése Procore SSO, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="dad05-222">**To assign Britta Simon to Procore SSO, perform the following steps:**</span></span>

1. <span data-ttu-id="dad05-223">Az Azure felügyeleti portálra, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="dad05-223">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="dad05-225">Az alkalmazások listában válassza ki a **Procore SSO**.</span><span class="sxs-lookup"><span data-stu-id="dad05-225">In the applications list, select **Procore SSO**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_app.png) 

3. <span data-ttu-id="dad05-227">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="dad05-227">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="dad05-229">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="dad05-229">Click **Add** button.</span></span> <span data-ttu-id="dad05-230">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="dad05-230">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="dad05-232">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="dad05-232">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="dad05-233">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="dad05-233">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="dad05-234">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="dad05-234">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="dad05-235">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="dad05-235">Testing single sign-on</span></span>

<span data-ttu-id="dad05-236">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="dad05-236">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="dad05-237">Ha azt szeretné, az egyszeri bejelentkezés beállításainak ellenőrzéséhez nyissa meg a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="dad05-237">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="dad05-238">A hozzáférési Panel kapcsolatos további tudnivalókért lásd: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="dad05-238">For more details about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> <span data-ttu-id="dad05-239">Ha a hozzáférési panelen Procore SSO csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Procore SSO alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="dad05-239">When you click the Procore SSO tile in the Access Panel, you should get automatically signed-on to your Procore SSO application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dad05-240">További források</span><span class="sxs-lookup"><span data-stu-id="dad05-240">Additional resources</span></span>

* [<span data-ttu-id="dad05-241">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="dad05-241">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="dad05-242">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="dad05-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_203.png

