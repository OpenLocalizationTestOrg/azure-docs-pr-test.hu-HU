---
title: "Oktatóanyag: Azure Active Directoryval integrált DigiCert |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a DigiCert között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 646f3129-aa67-4875-9073-1d0b6a3173d9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jeedes
ms.openlocfilehash: 2ceb3c0833edcd4ecd875c5e8006961ed7216c66
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-digicert"></a><span data-ttu-id="8c75c-103">Oktatóanyag: Azure Active Directoryval integrált DigiCert</span><span class="sxs-lookup"><span data-stu-id="8c75c-103">Tutorial: Azure Active Directory integration with DigiCert</span></span>

<span data-ttu-id="8c75c-104">Ebben az oktatóanyagban elsajátíthatja DigiCert integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8c75c-104">In this tutorial, you learn how to integrate DigiCert with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8c75c-105">DigiCert integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="8c75c-105">Integrating DigiCert with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="8c75c-106">Megadhatja a DigiCert hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="8c75c-106">You can control in Azure AD who has access to DigiCert</span></span>
- <span data-ttu-id="8c75c-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a DigiCert (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="8c75c-107">You can enable your users to automatically get signed-on to DigiCert (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8c75c-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="8c75c-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="8c75c-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8c75c-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8c75c-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8c75c-110">Prerequisites</span></span>

<span data-ttu-id="8c75c-111">Az Azure AD-integráció konfigurálása a DigiCert, a következő elemeket kell:</span><span class="sxs-lookup"><span data-stu-id="8c75c-111">To configure Azure AD integration with DigiCert, you need the following items:</span></span>

- <span data-ttu-id="8c75c-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="8c75c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8c75c-113">A DigiCert egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="8c75c-113">A DigiCert single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8c75c-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="8c75c-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8c75c-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="8c75c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8c75c-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="8c75c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8c75c-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8c75c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8c75c-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="8c75c-118">Scenario description</span></span>
<span data-ttu-id="8c75c-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="8c75c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8c75c-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="8c75c-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8c75c-121">A gyűjteményből DigiCert hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8c75c-121">Adding DigiCert from the gallery</span></span>
2. <span data-ttu-id="8c75c-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="8c75c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-digicert-from-the-gallery"></a><span data-ttu-id="8c75c-123">A gyűjteményből DigiCert hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8c75c-123">Adding DigiCert from the gallery</span></span>
<span data-ttu-id="8c75c-124">Az Azure AD integrálása a DigiCert konfigurálásához kell hozzáadnia DigiCert a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="8c75c-124">To configure the integration of DigiCert into Azure AD, you need to add DigiCert from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="8c75c-125">**Adja hozzá a DigiCert a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="8c75c-125">**To add DigiCert from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="8c75c-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8c75c-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8c75c-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="8c75c-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="8c75c-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8c75c-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="8c75c-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="8c75c-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="8c75c-133">Írja be a keresőmezőbe, **DigiCert**.</span><span class="sxs-lookup"><span data-stu-id="8c75c-133">In the search box, type **DigiCert**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-digicert-tutorial/tutorial_digicert_search.png)

5. <span data-ttu-id="8c75c-135">Az eredmények panelen válassza ki a **DigiCert**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="8c75c-135">In the results panel, select **DigiCert**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-digicert-tutorial/tutorial_digicert_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8c75c-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="8c75c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8c75c-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a DigiCert "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="8c75c-138">In this section, you configure and test Azure AD single sign-on with DigiCert based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="8c75c-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a DigiCert tartozó felhasználót a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="8c75c-139">For single sign-on to work, Azure AD needs to know what the counterpart user in DigiCert is to a user in Azure AD.</span></span> <span data-ttu-id="8c75c-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a DigiCert közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="8c75c-140">In other words, a link relationship between an Azure AD user and the related user in DigiCert needs to be established.</span></span>

<span data-ttu-id="8c75c-141">A DigiCert, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="8c75c-141">In DigiCert, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="8c75c-142">Az Azure AD egyszeri bejelentkezést a DigiCert tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="8c75c-142">To configure and test Azure AD single sign-on with DigiCert, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="8c75c-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="8c75c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="8c75c-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="8c75c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8c75c-145">**[A DigiCert tesztfelhasználó létrehozása](#creating-a-digicert-test-user)**  - való Britta Simon valami DigiCert, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="8c75c-145">**[Creating a DigiCert test user](#creating-a-digicert-test-user)** - to have a counterpart of Britta Simon in DigiCert that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="8c75c-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="8c75c-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8c75c-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="8c75c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8c75c-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8c75c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8c75c-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a DigiCert alkalmazásban egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="8c75c-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your DigiCert application.</span></span>

<span data-ttu-id="8c75c-150">**A DigiCert konfigurálása az Azure AD egyszeri bejelentkezést, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="8c75c-150">**To configure Azure AD single sign-on with DigiCert, perform the following steps:**</span></span>

1. <span data-ttu-id="8c75c-151">Az Azure portálon a a **DigiCert** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="8c75c-151">In the Azure portal, on the **DigiCert** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="8c75c-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="8c75c-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-digicert-tutorial/tutorial_digicert_samlbase.png)

3. <span data-ttu-id="8c75c-155">Az a **DigiCert tartomány és az URL-címek** szakaszban, a felhasználó nem rendelkezik, az alkalmazás már előre integrálva van az Azure-ral bármely lépések végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="8c75c-155">On the **DigiCert Domain and URLs** section, the user does not have to perform any steps as the app is already pre-integrated with Azure.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-digicert-tutorial/tutorial_digicert_url.png)

4. <span data-ttu-id="8c75c-157">DigiCert alkalmazás vár a SAML helyességi feltételek egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="8c75c-157">DigiCert application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="8c75c-158">A következő jogcímek alkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="8c75c-158">Configure the following claims for this application.</span></span> <span data-ttu-id="8c75c-159">Ezek az attribútumok értékének kezelheti a "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="8c75c-159">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="8c75c-160">Az alábbi képernyőfelvételen látható egy példa ehhez a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="8c75c-160">The following screenshot shows an example for this configuration.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-digicert-tutorial/tutorial_digicert_attributes.png)
    
5. <span data-ttu-id="8c75c-162">A a **felhasználói attribútumok** a szakasz a **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum, az ábrán látható módon, és hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="8c75c-162">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image and perform the following steps:</span></span>
    
    | <span data-ttu-id="8c75c-163">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="8c75c-163">Attribute Name</span></span> | <span data-ttu-id="8c75c-164">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="8c75c-164">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="8c75c-165">Vállalati</span><span class="sxs-lookup"><span data-stu-id="8c75c-165">company</span></span> | <span data-ttu-id="8c75c-166">< companycode ></span><span class="sxs-lookup"><span data-stu-id="8c75c-166">< companycode ></span></span> |
    | <span data-ttu-id="8c75c-167">digicertrole</span><span class="sxs-lookup"><span data-stu-id="8c75c-167">digicertrole</span></span> | <span data-ttu-id="8c75c-168">CanAccessCertCentral</span><span class="sxs-lookup"><span data-stu-id="8c75c-168">CanAccessCertCentral</span></span> |

    > [!Note]
    > <span data-ttu-id="8c75c-169">Értékének **vállalati** attribútum nincs valós.</span><span class="sxs-lookup"><span data-stu-id="8c75c-169">The value of **company** attribute is not real.</span></span> <span data-ttu-id="8c75c-170">Ez az érték frissítés tényleges vállalati kóddal.</span><span class="sxs-lookup"><span data-stu-id="8c75c-170">Update this value with actual company code.</span></span> <span data-ttu-id="8c75c-171">Az érték beolvasásához **vállalati** névjegy attribútuma [DigiCert támogatási csoport](mailto:support@digicert.com).</span><span class="sxs-lookup"><span data-stu-id="8c75c-171">To get the value of **company** attribute contact [DigiCert support team](mailto:support@digicert.com).</span></span>

    <span data-ttu-id="8c75c-172">a.</span><span class="sxs-lookup"><span data-stu-id="8c75c-172">a.</span></span> <span data-ttu-id="8c75c-173">Kattintson a **Hozzáadás attribútum** megnyitásához a **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8c75c-173">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-digicert-tutorial/tutorial_attribute_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-digicert-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="8c75c-176">b.</span><span class="sxs-lookup"><span data-stu-id="8c75c-176">b.</span></span> <span data-ttu-id="8c75c-177">Az a **neve** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="8c75c-177">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="8c75c-178">c.</span><span class="sxs-lookup"><span data-stu-id="8c75c-178">c.</span></span> <span data-ttu-id="8c75c-179">Az a **érték** kilistázásához írja be a sorhoz látható attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="8c75c-179">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="8c75c-180">d.</span><span class="sxs-lookup"><span data-stu-id="8c75c-180">d.</span></span> <span data-ttu-id="8c75c-181">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="8c75c-181">Click **Ok**.</span></span> 

6. <span data-ttu-id="8c75c-182">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="8c75c-182">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-digicert-tutorial/tutorial_digicert_certificate.png) 

7. <span data-ttu-id="8c75c-184">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="8c75c-184">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-digicert-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="8c75c-186">Egyszeri bejelentkezés konfigurálása **DigiCert** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [DigiCert támogatási csoport](mailto:support@digicert.com).</span><span class="sxs-lookup"><span data-stu-id="8c75c-186">To configure single sign-on on **DigiCert** side, you need to send the downloaded **Metadata XML** to [DigiCert support team](mailto:support@digicert.com).</span></span> <span data-ttu-id="8c75c-187">Akkor állítsa be ezt a beállítást, hogy a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="8c75c-187">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="8c75c-188">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="8c75c-188">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="8c75c-189">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="8c75c-189">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="8c75c-190">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8c75c-190">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8c75c-191">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8c75c-191">Creating an Azure AD test user</span></span>
<span data-ttu-id="8c75c-192">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="8c75c-192">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="8c75c-194">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="8c75c-194">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="8c75c-195">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8c75c-195">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-digicert-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8c75c-197">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="8c75c-197">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-digicert-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8c75c-199">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="8c75c-199">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-digicert-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8c75c-201">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="8c75c-201">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-digicert-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8c75c-203">a.</span><span class="sxs-lookup"><span data-stu-id="8c75c-203">a.</span></span> <span data-ttu-id="8c75c-204">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8c75c-204">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8c75c-205">b.</span><span class="sxs-lookup"><span data-stu-id="8c75c-205">b.</span></span> <span data-ttu-id="8c75c-206">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8c75c-206">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8c75c-207">c.</span><span class="sxs-lookup"><span data-stu-id="8c75c-207">c.</span></span> <span data-ttu-id="8c75c-208">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="8c75c-208">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="8c75c-209">d.</span><span class="sxs-lookup"><span data-stu-id="8c75c-209">d.</span></span> <span data-ttu-id="8c75c-210">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="8c75c-210">Click **Create**.</span></span>
 
### <a name="creating-a-digicert-test-user"></a><span data-ttu-id="8c75c-211">A DigiCert tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8c75c-211">Creating a DigiCert test user</span></span>

<span data-ttu-id="8c75c-212">Ebben a szakaszban a DigiCert Britta Simon nevű felhasználó hoz létre.</span><span class="sxs-lookup"><span data-stu-id="8c75c-212">In this section, you create a user called Britta Simon in DigiCert.</span></span> <span data-ttu-id="8c75c-213">Adjon együttműködve [DigiCert támogatási csoport](mailto:support@digicert.com) felhasználót is hozzáadhat a DigiCert.</span><span class="sxs-lookup"><span data-stu-id="8c75c-213">Please work with [DigiCert support team](mailto:support@digicert.com) to add the users in DigiCert.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="8c75c-214">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="8c75c-214">Assigning the Azure AD test user</span></span>

<span data-ttu-id="8c75c-215">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés DigiCert Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="8c75c-215">In this section, you enable Britta Simon to use Azure single sign-on by granting access to DigiCert.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="8c75c-217">**Britta Simon hozzárendelése DigiCert, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="8c75c-217">**To assign Britta Simon to DigiCert, perform the following steps:**</span></span>

1. <span data-ttu-id="8c75c-218">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8c75c-218">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="8c75c-220">Az alkalmazások listában válassza ki a **DigiCert**.</span><span class="sxs-lookup"><span data-stu-id="8c75c-220">In the applications list, select **DigiCert**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-digicert-tutorial/tutorial_digicert_app.png) 

3. <span data-ttu-id="8c75c-222">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="8c75c-222">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="8c75c-224">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="8c75c-224">Click **Add** button.</span></span> <span data-ttu-id="8c75c-225">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8c75c-225">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="8c75c-227">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="8c75c-227">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="8c75c-228">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8c75c-228">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8c75c-229">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8c75c-229">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8c75c-230">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="8c75c-230">Testing single sign-on</span></span>

<span data-ttu-id="8c75c-231">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="8c75c-231">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="8c75c-232">Ha a hozzáférési panelen DigiCert csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az DeigiCert alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="8c75c-232">When you click the DigiCert tile in the Access Panel, you should get automatically signed-on to your DeigiCert application.</span></span>
<span data-ttu-id="8c75c-233">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="8c75c-233">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="8c75c-234">További források</span><span class="sxs-lookup"><span data-stu-id="8c75c-234">Additional resources</span></span>

* [<span data-ttu-id="8c75c-235">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="8c75c-235">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8c75c-236">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="8c75c-236">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_203.png

