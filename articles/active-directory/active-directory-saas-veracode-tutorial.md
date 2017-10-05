---
title: "Oktatóanyag: Azure Active Directoryval integrált Veracode |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Veracode között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4fe78050-cb6d-4db9-96ec-58cc0779167f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: d49349c5ae08e67d91e30967f3644623211823ce
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-veracode"></a><span data-ttu-id="f72a3-103">Oktatóanyag: Azure Active Directoryval integrált Veracode</span><span class="sxs-lookup"><span data-stu-id="f72a3-103">Tutorial: Azure Active Directory integration with Veracode</span></span>

<span data-ttu-id="f72a3-104">Ebben az oktatóanyagban elsajátíthatja Veracode integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f72a3-104">In this tutorial, you learn how to integrate Veracode with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f72a3-105">Veracode integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="f72a3-105">Integrating Veracode with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="f72a3-106">Az Azure AD, aki hozzáfér Veracode szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="f72a3-106">You can control in Azure AD who has access to Veracode.</span></span>
- <span data-ttu-id="f72a3-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Veracode (egyszeri bejelentkezés) számára a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="f72a3-107">You can enable your users to automatically get signed-on to Veracode (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="f72a3-108">A fiók egyetlen központi helyen – az Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="f72a3-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="f72a3-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f72a3-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f72a3-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f72a3-110">Prerequisites</span></span>

<span data-ttu-id="f72a3-111">Konfigurálása az Azure AD-integrációs Veracode, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="f72a3-111">To configure Azure AD integration with Veracode, you need the following items:</span></span>

- <span data-ttu-id="f72a3-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="f72a3-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f72a3-113">Egy Veracode egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="f72a3-113">A Veracode single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f72a3-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="f72a3-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f72a3-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="f72a3-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f72a3-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="f72a3-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f72a3-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f72a3-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f72a3-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="f72a3-118">Scenario description</span></span>
<span data-ttu-id="f72a3-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="f72a3-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f72a3-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="f72a3-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f72a3-121">Adja hozzá a Veracode a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="f72a3-121">Add Veracode from the gallery</span></span>
2. <span data-ttu-id="f72a3-122">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f72a3-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-veracode-from-the-gallery"></a><span data-ttu-id="f72a3-123">Adja hozzá a Veracode a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="f72a3-123">Add Veracode from the gallery</span></span>
<span data-ttu-id="f72a3-124">Az Azure AD integrálása a Veracode konfigurálásához kell hozzáadnia Veracode a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="f72a3-124">To configure the integration of Veracode into Azure AD, you need to add Veracode from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="f72a3-125">**A gyűjteményből Veracode hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f72a3-125">**To add Veracode from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="f72a3-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f72a3-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="f72a3-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="f72a3-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="f72a3-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f72a3-129">Then go to **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="f72a3-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="f72a3-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="f72a3-133">Írja be a keresőmezőbe, **Veracode**, jelölje be **Veracode** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f72a3-133">In the search box, type **Veracode**, select  **Veracode** from result panel then click **Add** button to add the application.</span></span>

    ![Az eredménylistában Veracode](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="f72a3-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f72a3-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="f72a3-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Veracode.</span><span class="sxs-lookup"><span data-stu-id="f72a3-136">In this section, you configure and test Azure AD single sign-on with Veracode based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="f72a3-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Veracode a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="f72a3-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Veracode is to a user in Azure AD.</span></span> <span data-ttu-id="f72a3-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Veracode közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="f72a3-138">In other words, a link relationship between an Azure AD user and the related user in Veracode needs to be established.</span></span>

<span data-ttu-id="f72a3-139">Veracode, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="f72a3-139">In Veracode, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="f72a3-140">Az Azure AD egyszeri bejelentkezést a Veracode tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="f72a3-140">To configure and test Azure AD single sign-on with Veracode, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="f72a3-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="f72a3-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="f72a3-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="f72a3-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f72a3-143">**[Veracode tesztfelhasználó létrehozása](#create-a-veracode-test-user)**  - való Britta Simon valami Veracode, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="f72a3-143">**[Create a Veracode test user](#create-a-veracode-test-user)** - to have a counterpart of Britta Simon in Veracode that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="f72a3-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="f72a3-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f72a3-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="f72a3-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="f72a3-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f72a3-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="f72a3-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Veracode alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="f72a3-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Veracode application.</span></span>

<span data-ttu-id="f72a3-148">**Konfigurálása az Azure AD az egyszeri bejelentkezés Veracode, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f72a3-148">**To configure Azure AD single sign-on with Veracode, perform the following steps:**</span></span>

1. <span data-ttu-id="f72a3-149">Az Azure portálon a a **Veracode** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="f72a3-149">In the Azure portal, on the **Veracode** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="f72a3-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="f72a3-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_samlbase.png)

3. <span data-ttu-id="f72a3-153">Az a **Veracode tartomány és az URL-címek** szakaszban, a felhasználó nem rendelkezik, az alkalmazás már előre integrálva van az Azure-ral bármely lépések végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="f72a3-153">On the **Veracode Domain and URLs** section, the user does not have to perform any steps as the app is already pre-integrated with Azure.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_url.png)

4. <span data-ttu-id="f72a3-155">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="f72a3-155">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_certificate.png) 

5. <span data-ttu-id="f72a3-157">Ez a szakasz célja felvázoló engedélyezése a felhasználók hitelesítéséhez Veracode fiókkal az Azure AD összevonási alapján a SAML protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="f72a3-157">The objective of this section is to outline how to enable users to authenticate to Veracode with their account in Azure AD using federation based on the SAML protocol.</span></span>

    <span data-ttu-id="f72a3-158">A Veracode alkalmazás vár a SAML helyességi feltételek egy meghatározott formátumban, amelyhez egyéni attribútum leképezései hozzáadása a **saml-jogkivonat attribútumok** konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="f72a3-158">Your Veracode application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your **saml token attributes** configuration.</span></span> <span data-ttu-id="f72a3-159">Az alábbi képernyőfelvételen látható egy példa a.</span><span class="sxs-lookup"><span data-stu-id="f72a3-159">The following screenshot shows an example for this.</span></span>
    
    <span data-ttu-id="f72a3-160">![Attribútumok](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_attr.png "attribútumok")</span><span class="sxs-lookup"><span data-stu-id="f72a3-160">![Attributes](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_attr.png "Attributes")</span></span>

6. <span data-ttu-id="f72a3-161">A kötelező attribútum-leképezésekhez hozzáadásához hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="f72a3-161">To add the required attribute mappings, perform the following steps:</span></span>

    | <span data-ttu-id="f72a3-162">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="f72a3-162">Attribute Name</span></span> | <span data-ttu-id="f72a3-163">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="f72a3-163">Attribute Value</span></span> |
    |--- |--- |
    | <span data-ttu-id="f72a3-164">Utónév</span><span class="sxs-lookup"><span data-stu-id="f72a3-164">firstname</span></span> |<span data-ttu-id="f72a3-165">User.givenName</span><span class="sxs-lookup"><span data-stu-id="f72a3-165">User.givenname</span></span> |
    | <span data-ttu-id="f72a3-166">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="f72a3-166">lastname</span></span> |<span data-ttu-id="f72a3-167">User.surname</span><span class="sxs-lookup"><span data-stu-id="f72a3-167">User.surname</span></span> |
    | <span data-ttu-id="f72a3-168">E-mailek</span><span class="sxs-lookup"><span data-stu-id="f72a3-168">email</span></span> |<span data-ttu-id="f72a3-169">User.mail</span><span class="sxs-lookup"><span data-stu-id="f72a3-169">User.mail</span></span> |
    
    <span data-ttu-id="f72a3-170">a.</span><span class="sxs-lookup"><span data-stu-id="f72a3-170">a.</span></span> <span data-ttu-id="f72a3-171">Kattintson a fenti adatokat minden egyes sorhoz kapcsolódóan **hozzáadása a felhasználói attribútum**.</span><span class="sxs-lookup"><span data-stu-id="f72a3-171">For each data row in the table above, click **add user attribute**.</span></span>
    
    <span data-ttu-id="f72a3-172">![Attribútumok](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addattr.png "attribútumok")</span><span class="sxs-lookup"><span data-stu-id="f72a3-172">![Attributes](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addattr.png "Attributes")</span></span>
    
    <span data-ttu-id="f72a3-173">![Attribútumok](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addattr1.png "attribútumok")</span><span class="sxs-lookup"><span data-stu-id="f72a3-173">![Attributes](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addattr1.png "Attributes")</span></span>
    
    <span data-ttu-id="f72a3-174">b.</span><span class="sxs-lookup"><span data-stu-id="f72a3-174">b.</span></span> <span data-ttu-id="f72a3-175">Az a **attribútumnév** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="f72a3-175">In the **Attribute Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="f72a3-176">c.</span><span class="sxs-lookup"><span data-stu-id="f72a3-176">c.</span></span> <span data-ttu-id="f72a3-177">Az a **attribútumérték** szövegmező, válassza ki az adott sorhoz feltüntetett attribútumot értéket.</span><span class="sxs-lookup"><span data-stu-id="f72a3-177">In the **Attribute Value** textbox, select the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="f72a3-178">d.</span><span class="sxs-lookup"><span data-stu-id="f72a3-178">d.</span></span> <span data-ttu-id="f72a3-179">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="f72a3-179">Click **Ok**.</span></span>

7. <span data-ttu-id="f72a3-180">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="f72a3-180">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-veracode-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="f72a3-182">A a **Veracode konfigurációs** kattintson **konfigurálása Veracode** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="f72a3-182">On the **Veracode Configuration** section, click **Configure Veracode** to open **Configure sign-on** window.</span></span> <span data-ttu-id="f72a3-183">Másolás a **SAML Entitásazonosító** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="f72a3-183">Copy the **SAML Entity ID** from the **Quick Reference section.**</span></span>

    ![Veracode konfiguráció](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_configure.png) 

9. <span data-ttu-id="f72a3-185">Egy másik webes böngészőablakban jelentkezzen be a Veracode vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="f72a3-185">In a different web browser window, log into your Veracode company site as an administrator.</span></span>

10. <span data-ttu-id="f72a3-186">Kattintson a felső menüben **beállítások**, és kattintson a **Admin**.</span><span class="sxs-lookup"><span data-stu-id="f72a3-186">In the menu on the top, click **Settings**, and then click **Admin**.</span></span>
   
    <span data-ttu-id="f72a3-187">![Felügyeleti](./media/active-directory-saas-veracode-tutorial/ic802911.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="f72a3-187">![Administration](./media/active-directory-saas-veracode-tutorial/ic802911.png "Administration")</span></span>

11. <span data-ttu-id="f72a3-188">Kattintson a **SAML** fülre.</span><span class="sxs-lookup"><span data-stu-id="f72a3-188">Click the **SAML** tab.</span></span>

12. <span data-ttu-id="f72a3-189">Az a **szervezeti SAML beállítások** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="f72a3-189">In the **Organization SAML Settings** section, perform the following steps:</span></span>
   
    <span data-ttu-id="f72a3-190">![Felügyeleti](./media/active-directory-saas-veracode-tutorial/ic802912.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="f72a3-190">![Administration](./media/active-directory-saas-veracode-tutorial/ic802912.png "Administration")</span></span>
   
    <span data-ttu-id="f72a3-191">a.</span><span class="sxs-lookup"><span data-stu-id="f72a3-191">a.</span></span>  <span data-ttu-id="f72a3-192">A **kibocsátó** szövegmezőhöz illessze be az értékét **SAML Entitásazonosító** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="f72a3-192">In  **Issuer** textbox, paste the value of  **SAML Entity ID** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="f72a3-193">b.</span><span class="sxs-lookup"><span data-stu-id="f72a3-193">b.</span></span> <span data-ttu-id="f72a3-194">Kattintson az Azure-portálról letöltött tanúsítvány feltöltésének **Choose File**.</span><span class="sxs-lookup"><span data-stu-id="f72a3-194">To upload your downloaded certificate from Azure portal, click **Choose File**.</span></span>
   
    <span data-ttu-id="f72a3-195">c.</span><span class="sxs-lookup"><span data-stu-id="f72a3-195">c.</span></span> <span data-ttu-id="f72a3-196">Válassza ki **engedélyezze az önkiszolgáló regisztrációt**.</span><span class="sxs-lookup"><span data-stu-id="f72a3-196">Select **Enable Self Registration**.</span></span>

13. <span data-ttu-id="f72a3-197">Az a **önkiszolgáló regisztrációs beállítások** szakaszt, hajtsa végre az alábbi lépéseket, és kattintson a **mentése**:</span><span class="sxs-lookup"><span data-stu-id="f72a3-197">In the **Self Registration Settings** section, perform the following steps, and then click **Save**:</span></span>
   
    <span data-ttu-id="f72a3-198">![Felügyeleti](./media/active-directory-saas-veracode-tutorial/ic802913.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="f72a3-198">![Administration](./media/active-directory-saas-veracode-tutorial/ic802913.png "Administration")</span></span>
   
    <span data-ttu-id="f72a3-199">a.</span><span class="sxs-lookup"><span data-stu-id="f72a3-199">a.</span></span> <span data-ttu-id="f72a3-200">Mint **új felhasználó aktiválása**, jelölje be **nem aktiválási szükséges**.</span><span class="sxs-lookup"><span data-stu-id="f72a3-200">As **New User Activation**, select **No Activation Required**.</span></span>
   
    <span data-ttu-id="f72a3-201">b.</span><span class="sxs-lookup"><span data-stu-id="f72a3-201">b.</span></span> <span data-ttu-id="f72a3-202">Mint **felhasználói adatfrissítések**, jelölje be **preferencia Veracode felhasználói adatok**.</span><span class="sxs-lookup"><span data-stu-id="f72a3-202">As **User Data Updates**, select **Preference Veracode User Data**.</span></span>
   
    <span data-ttu-id="f72a3-203">c.</span><span class="sxs-lookup"><span data-stu-id="f72a3-203">c.</span></span> <span data-ttu-id="f72a3-204">A **SAML attribútum adatai**, jelölje be az alábbiakat:</span><span class="sxs-lookup"><span data-stu-id="f72a3-204">For **SAML Attribute Details**, select the following:</span></span>
      * <span data-ttu-id="f72a3-205">**Felhasználói szerepkörök**</span><span class="sxs-lookup"><span data-stu-id="f72a3-205">**User Roles**</span></span>
      * <span data-ttu-id="f72a3-206">**Csoportházirendet felügyelő rendszergazda**</span><span class="sxs-lookup"><span data-stu-id="f72a3-206">**Policy Administrator**</span></span>
      * <span data-ttu-id="f72a3-207">**Felülvizsgáló**</span><span class="sxs-lookup"><span data-stu-id="f72a3-207">**Reviewer**</span></span>
      * <span data-ttu-id="f72a3-208">**Biztonsági vezető**</span><span class="sxs-lookup"><span data-stu-id="f72a3-208">**Security Lead**</span></span>
      * <span data-ttu-id="f72a3-209">**Vezetői**</span><span class="sxs-lookup"><span data-stu-id="f72a3-209">**Executive**</span></span>
      * <span data-ttu-id="f72a3-210">**Küldő**</span><span class="sxs-lookup"><span data-stu-id="f72a3-210">**Submitter**</span></span>
      * <span data-ttu-id="f72a3-211">**Létrehozó**</span><span class="sxs-lookup"><span data-stu-id="f72a3-211">**Creator**</span></span>
      * <span data-ttu-id="f72a3-212">**Az ellenőrzési típusok**</span><span class="sxs-lookup"><span data-stu-id="f72a3-212">**All Scan Types**</span></span>
      * <span data-ttu-id="f72a3-213">**A csoporttagságot**</span><span class="sxs-lookup"><span data-stu-id="f72a3-213">**Team Memberships**</span></span>
      * <span data-ttu-id="f72a3-214">**Alapértelmezett csoport**</span><span class="sxs-lookup"><span data-stu-id="f72a3-214">**Default Team**</span></span>

> [!TIP]
> <span data-ttu-id="f72a3-215">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="f72a3-215">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="f72a3-216">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="f72a3-216">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="f72a3-217">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f72a3-217">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="f72a3-218">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="f72a3-218">Create an Azure AD test user</span></span>

<span data-ttu-id="f72a3-219">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="f72a3-219">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="f72a3-221">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f72a3-221">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="f72a3-222">Az Azure portálon a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="f72a3-222">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-veracode-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="f72a3-224">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="f72a3-224">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-veracode-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="f72a3-226">Megnyitásához a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** tetején a **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="f72a3-226">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![A Hozzáadás gombra.](./media/active-directory-saas-veracode-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="f72a3-228">Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="f72a3-228">In the **User** dialog box, perform the following steps:</span></span>

    ![A felhasználó párbeszédpanel](./media/active-directory-saas-veracode-tutorial/create_aaduser_04.png)

    <span data-ttu-id="f72a3-230">a.</span><span class="sxs-lookup"><span data-stu-id="f72a3-230">a.</span></span> <span data-ttu-id="f72a3-231">Az a **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f72a3-231">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f72a3-232">b.</span><span class="sxs-lookup"><span data-stu-id="f72a3-232">b.</span></span> <span data-ttu-id="f72a3-233">Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címe az Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f72a3-233">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="f72a3-234">c.</span><span class="sxs-lookup"><span data-stu-id="f72a3-234">c.</span></span> <span data-ttu-id="f72a3-235">Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="f72a3-235">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="f72a3-236">d.</span><span class="sxs-lookup"><span data-stu-id="f72a3-236">d.</span></span> <span data-ttu-id="f72a3-237">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="f72a3-237">Click **Create**.</span></span>
 
### <a name="create-a-veracode-test-user"></a><span data-ttu-id="f72a3-238">Veracode tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="f72a3-238">Create a Veracode test user</span></span>
<span data-ttu-id="f72a3-239">Ahhoz, hogy az Azure AD-felhasználók Veracode bejelentkezni, akkor ki kell építenie Veracode be.</span><span class="sxs-lookup"><span data-stu-id="f72a3-239">In order to enable Azure AD users to log into Veracode, they must be provisioned into Veracode.</span></span> <span data-ttu-id="f72a3-240">Veracode, ha egy automatizált feladat.</span><span class="sxs-lookup"><span data-stu-id="f72a3-240">In the case of Veracode, provisioning is an automated task.</span></span> <span data-ttu-id="f72a3-241">Nincs művelet elem meg.</span><span class="sxs-lookup"><span data-stu-id="f72a3-241">There is no action item for you.</span></span> <span data-ttu-id="f72a3-242">Felhasználók automatikusan létrejönnek szükség esetén az első egy bejelentkezési kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="f72a3-242">Users are automatically created if necessary during the first single sign-on attempt.</span></span>

> [!NOTE]
> <span data-ttu-id="f72a3-243">Bármely más Veracode felhasználói fiók létrehozása eszközök vagy Veracode kiépíteni az Azure AD-felhasználói fiókok által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="f72a3-243">You can use any other Veracode user account creation tools or APIs provided by Veracode to provision Azure AD user accounts.</span></span>
> 

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="f72a3-244">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="f72a3-244">Assign the Azure AD test user</span></span>

<span data-ttu-id="f72a3-245">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Veracode Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="f72a3-245">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Veracode.</span></span>

![A felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="f72a3-247">**Britta Simon hozzárendelése Veracode, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f72a3-247">**To assign Britta Simon to Veracode, perform the following steps:**</span></span>

1. <span data-ttu-id="f72a3-248">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f72a3-248">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="f72a3-250">Az alkalmazások listában válassza ki a **Veracode**.</span><span class="sxs-lookup"><span data-stu-id="f72a3-250">In the applications list, select **Veracode**.</span></span>

    ![Az alkalmazások listáját a Veracode hivatkozás](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_app.png)  

3. <span data-ttu-id="f72a3-252">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="f72a3-252">In the menu on the left, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202]

4. <span data-ttu-id="f72a3-254">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="f72a3-254">Click **Add** button.</span></span> <span data-ttu-id="f72a3-255">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f72a3-255">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="f72a3-257">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="f72a3-257">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="f72a3-258">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f72a3-258">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f72a3-259">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f72a3-259">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="f72a3-260">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="f72a3-260">Test single sign-on</span></span>

<span data-ttu-id="f72a3-261">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="f72a3-261">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="f72a3-262">Ha a hozzáférési panelen Veracode csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Veracode alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="f72a3-262">When you click the Veracode tile in the Access Panel, you should get automatically signed-on to your Veracode application.</span></span>
<span data-ttu-id="f72a3-263">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f72a3-263">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="f72a3-264">További források</span><span class="sxs-lookup"><span data-stu-id="f72a3-264">Additional resources</span></span>

* [<span data-ttu-id="f72a3-265">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="f72a3-265">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f72a3-266">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="f72a3-266">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_203.png

