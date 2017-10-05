---
title: "Oktatóanyag: Azure Active Directoryval integrált Druva |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Druva között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ab92b600-1fea-4905-b1c7-ef8e4d8c495c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: b23e73c47b9a00893e036b67826e4b7ead819a1d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-druva"></a><span data-ttu-id="f21ed-103">Oktatóanyag: Azure Active Directoryval integrált Druva</span><span class="sxs-lookup"><span data-stu-id="f21ed-103">Tutorial: Azure Active Directory integration with Druva</span></span>

<span data-ttu-id="f21ed-104">Ebben az oktatóanyagban elsajátíthatja Druva integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f21ed-104">In this tutorial, you learn how to integrate Druva with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f21ed-105">Druva integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="f21ed-105">Integrating Druva with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="f21ed-106">Az Azure AD, aki hozzáfér Druva szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="f21ed-106">You can control in Azure AD who has access to Druva.</span></span>
- <span data-ttu-id="f21ed-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Druva (egyszeri bejelentkezés) számára a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="f21ed-107">You can enable your users to automatically get signed-on to Druva (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="f21ed-108">A fiók egyetlen központi helyen – az Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="f21ed-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="f21ed-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f21ed-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f21ed-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f21ed-110">Prerequisites</span></span>

<span data-ttu-id="f21ed-111">Konfigurálása az Azure AD-integrációs Druva, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="f21ed-111">To configure Azure AD integration with Druva, you need the following items:</span></span>

- <span data-ttu-id="f21ed-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="f21ed-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f21ed-113">Egy Druva egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="f21ed-113">A Druva single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f21ed-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="f21ed-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f21ed-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="f21ed-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f21ed-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="f21ed-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f21ed-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f21ed-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f21ed-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="f21ed-118">Scenario description</span></span>
<span data-ttu-id="f21ed-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="f21ed-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f21ed-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="f21ed-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f21ed-121">A gyűjteményből Druva hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f21ed-121">Adding Druva from the gallery</span></span>
2. <span data-ttu-id="f21ed-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="f21ed-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-druva-from-the-gallery"></a><span data-ttu-id="f21ed-123">A gyűjteményből Druva hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f21ed-123">Adding Druva from the gallery</span></span>
<span data-ttu-id="f21ed-124">Az Azure AD integrálása a Druva konfigurálásához kell hozzáadnia Druva a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="f21ed-124">To configure the integration of Druva into Azure AD, you need to add Druva from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="f21ed-125">**A gyűjteményből Druva hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f21ed-125">**To add Druva from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="f21ed-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f21ed-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="f21ed-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="f21ed-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="f21ed-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f21ed-129">Then go to **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="f21ed-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="f21ed-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="f21ed-133">Írja be a keresőmezőbe, **Druva**, jelölje be **Druva** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f21ed-133">In the search box, type **Druva**, select **Druva** from result panel then click **Add** button to add the application.</span></span>

    ![Az eredménylistában Druva](./media/active-directory-saas-druva-tutorial/tutorial_druva_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="f21ed-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f21ed-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="f21ed-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Druva.</span><span class="sxs-lookup"><span data-stu-id="f21ed-136">In this section, you configure and test Azure AD single sign-on with Druva based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="f21ed-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Druva a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="f21ed-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Druva is to a user in Azure AD.</span></span> <span data-ttu-id="f21ed-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Druva közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="f21ed-138">In other words, a link relationship between an Azure AD user and the related user in Druva needs to be established.</span></span>

<span data-ttu-id="f21ed-139">Druva, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="f21ed-139">In Druva, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="f21ed-140">Az Azure AD egyszeri bejelentkezést a Druva tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="f21ed-140">To configure and test Azure AD single sign-on with Druva, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="f21ed-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="f21ed-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="f21ed-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="f21ed-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f21ed-143">**[Druva tesztfelhasználó létrehozása](#create-a-druva-test-user)**  - való Britta Simon valami Druva, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="f21ed-143">**[Create a Druva test user](#create-a-druva-test-user)** - to have a counterpart of Britta Simon in Druva that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="f21ed-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="f21ed-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f21ed-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="f21ed-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="f21ed-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f21ed-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="f21ed-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Druva alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="f21ed-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Druva application.</span></span>

<span data-ttu-id="f21ed-148">**Konfigurálása az Azure AD az egyszeri bejelentkezés Druva, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f21ed-148">**To configure Azure AD single sign-on with Druva, perform the following steps:**</span></span>

1. <span data-ttu-id="f21ed-149">Az Azure portálon a a **Druva** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="f21ed-149">In the Azure portal, on the **Druva** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="f21ed-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="f21ed-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-druva-tutorial/tutorial_druva_samlbase.png)

3. <span data-ttu-id="f21ed-153">Az a **Druva tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="f21ed-153">On the **Druva Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-druva-tutorial/tutorial_druva_url.png)

    <span data-ttu-id="f21ed-155">Az a **bejelentkezési URL-cím** szövegmező, írja be az URL-cím:`https://cloud.druva.com/home`</span><span class="sxs-lookup"><span data-stu-id="f21ed-155">In the **Sign-on URL** textbox, type the URL: `https://cloud.druva.com/home`</span></span>

4. <span data-ttu-id="f21ed-156">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="f21ed-156">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-druva-tutorial/tutorial_druva_certificate.png) 

5. <span data-ttu-id="f21ed-158">A Druva alkalmazás vár a SAML helyességi feltételek egy meghatározott formátumban, amelyhez egyéni attribútum leképezései hozzáadása a **SAML-jogkivonat attribútumok** konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="f21ed-158">Your Druva application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your **SAML Token Attributes** configuration.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-druva-tutorial/tutorial_druva_attribute.png)

6. <span data-ttu-id="f21ed-160">A a **felhasználói attribútumok** a szakasz a **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum, az előző ábrán látható módon, és hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="f21ed-160">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the preceding image and perform the following steps:</span></span>

    | <span data-ttu-id="f21ed-161">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="f21ed-161">Attribute Name</span></span>      | <span data-ttu-id="f21ed-162">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="f21ed-162">Attribute Value</span></span>      |
    | ------------------- | -------------------- |
    | <span data-ttu-id="f21ed-163">insync\_auth\_token</span><span class="sxs-lookup"><span data-stu-id="f21ed-163">insync\_auth\_token</span></span> |<span data-ttu-id="f21ed-164">Adja meg a token generált érték</span><span class="sxs-lookup"><span data-stu-id="f21ed-164">Enter the token generated value</span></span> |
    
    <span data-ttu-id="f21ed-165">a.</span><span class="sxs-lookup"><span data-stu-id="f21ed-165">a.</span></span> <span data-ttu-id="f21ed-166">Kattintson a **Hozzáadás attribútum** megnyitásához a **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f21ed-166">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-druva-tutorial/tutorial_attribute_04.png)
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-druva-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="f21ed-169">b.</span><span class="sxs-lookup"><span data-stu-id="f21ed-169">b.</span></span> <span data-ttu-id="f21ed-170">Az a **neve** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="f21ed-170">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="f21ed-171">c.</span><span class="sxs-lookup"><span data-stu-id="f21ed-171">c.</span></span> <span data-ttu-id="f21ed-172">Az a **érték** kilistázásához írja be a sorhoz látható attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="f21ed-172">From the **Value** list, type the attribute value shown for that row.</span></span> <span data-ttu-id="f21ed-173">A token generált érték esetén, tekintse meg az oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="f21ed-173">The token generated value is explained later in tutorial.</span></span>
    
    <span data-ttu-id="f21ed-174">d.</span><span class="sxs-lookup"><span data-stu-id="f21ed-174">d.</span></span> <span data-ttu-id="f21ed-175">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="f21ed-175">Click **Ok**.</span></span>    

7. <span data-ttu-id="f21ed-176">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="f21ed-176">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-druva-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="f21ed-178">A a **Druva konfigurációs** kattintson **konfigurálása Druva** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="f21ed-178">On the **Druva Configuration** section, click **Configure Druva** to open **Configure sign-on** window.</span></span> <span data-ttu-id="f21ed-179">Másolás a **Sign-Out és SAML-alapú egyszeri bejelentkezés szolgáltatás URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="f21ed-179">Copy the **Sign-Out URL and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-druva-tutorial/tutorial_druva_configure.png) 

9. <span data-ttu-id="f21ed-181">Egy másik webes böngészőablakban jelentkezzen be a Druva vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="f21ed-181">In a different web browser window, log in to your Druva company site as an administrator.</span></span>

10. <span data-ttu-id="f21ed-182">Ugrás a **kezelése \> beállítások**.</span><span class="sxs-lookup"><span data-stu-id="f21ed-182">Go to **Manage \> Settings**.</span></span>

    <span data-ttu-id="f21ed-183">![Beállítások](./media/active-directory-saas-druva-tutorial/ic795091.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="f21ed-183">![Settings](./media/active-directory-saas-druva-tutorial/ic795091.png "Settings")</span></span>

11. <span data-ttu-id="f21ed-184">Az egyszeri bejelentkezés beállításai párbeszédpanel hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="f21ed-184">On the Single Sign-On Settings dialog, perform the following steps:</span></span>

    <span data-ttu-id="f21ed-185">![Az egyszeri bejelentkezés beállítások](./media/active-directory-saas-druva-tutorial/ic795092.png "az egyszeri bejelentkezés beállításai")</span><span class="sxs-lookup"><span data-stu-id="f21ed-185">![Single Sign-On Settings](./media/active-directory-saas-druva-tutorial/ic795092.png "Single Sign-On Settings")</span></span>
    
    <span data-ttu-id="f21ed-186">a.</span><span class="sxs-lookup"><span data-stu-id="f21ed-186">a.</span></span> <span data-ttu-id="f21ed-187">Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe** értéket, amely az Azure-portálról másolta a **azonosító szolgáltató bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="f21ed-187">Paste **SAML Single Sign-On Service URL** value, which you have copied from the Azure portal into the **ID Provider Login URL** textbox.</span></span>
    
    <span data-ttu-id="f21ed-188">b.</span><span class="sxs-lookup"><span data-stu-id="f21ed-188">b.</span></span> <span data-ttu-id="f21ed-189">Beillesztés **Sign-Out URL-cím** értéket, amely az Azure-portálról másolta a **azonosító szolgáltató kijelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="f21ed-189">Paste **Sign-Out URL** value, which you have copied from the Azure portal into the **ID Provider Logout URL** textbox.</span></span>
    
     <span data-ttu-id="f21ed-190">c.</span><span class="sxs-lookup"><span data-stu-id="f21ed-190">c.</span></span> <span data-ttu-id="f21ed-191">Nyissa meg a base-64 kódolású tanúsítvány a Jegyzettömbben, a tartalmának másolása a vágólapra és illessze be azt a **azonosító szolgáltató tanúsítvány** szövegmező</span><span class="sxs-lookup"><span data-stu-id="f21ed-191">Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste it to the **ID Provider Certificate** textbox</span></span>
     
     <span data-ttu-id="f21ed-192">d.</span><span class="sxs-lookup"><span data-stu-id="f21ed-192">d.</span></span> <span data-ttu-id="f21ed-193">Lehetőségre a **beállítások** kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="f21ed-193">To open the **Settings** page, click **Save**.</span></span>

12. <span data-ttu-id="f21ed-194">Az a **beállítások** kattintson **SSO jogkivonat készítése**.</span><span class="sxs-lookup"><span data-stu-id="f21ed-194">On the **Settings** page, click **Generate SSO Token**.</span></span>

    <span data-ttu-id="f21ed-195">![Beállítások](./media/active-directory-saas-druva-tutorial/ic795093.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="f21ed-195">![Settings](./media/active-directory-saas-druva-tutorial/ic795093.png "Settings")</span></span>

13. <span data-ttu-id="f21ed-196">Az a **egyszeri bejelentkezés hitelesítési jogkivonat** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="f21ed-196">On the **Single Sign-on Authentication Token** dialog, perform the following steps:</span></span>

    <span data-ttu-id="f21ed-197">![Egyszeri bejelentkezési Token](./media/active-directory-saas-druva-tutorial/ic795094.png "SSO jogkivonat")</span><span class="sxs-lookup"><span data-stu-id="f21ed-197">![SSO Token](./media/active-directory-saas-druva-tutorial/ic795094.png "SSO Token")</span></span>
    
    <span data-ttu-id="f21ed-198">a.</span><span class="sxs-lookup"><span data-stu-id="f21ed-198">a.</span></span> <span data-ttu-id="f21ed-199">Kattintson a **másolási**, illessze be az értéket másolta a **érték** textbox a **attribútum hozzáadása** szakasz.</span><span class="sxs-lookup"><span data-stu-id="f21ed-199">Click **Copy**, Paste copied value in the **Value** textbox in the **Add Attribute** section.</span></span>
    
    <span data-ttu-id="f21ed-200">b.</span><span class="sxs-lookup"><span data-stu-id="f21ed-200">b.</span></span> <span data-ttu-id="f21ed-201">Kattintson a **Bezárás** gombra.</span><span class="sxs-lookup"><span data-stu-id="f21ed-201">Click **Close**.</span></span>

> [!TIP]
> <span data-ttu-id="f21ed-202">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="f21ed-202">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="f21ed-203">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="f21ed-203">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="f21ed-204">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f21ed-204">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="f21ed-205">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="f21ed-205">Create an Azure AD test user</span></span>

<span data-ttu-id="f21ed-206">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="f21ed-206">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="f21ed-208">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f21ed-208">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="f21ed-209">Az Azure portálon a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="f21ed-209">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-druva-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="f21ed-211">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="f21ed-211">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-druva-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="f21ed-213">Megnyitásához a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** tetején a **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="f21ed-213">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![A Hozzáadás gombra.](./media/active-directory-saas-druva-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="f21ed-215">Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="f21ed-215">In the **User** dialog box, perform the following steps:</span></span>

    ![A felhasználó párbeszédpanel](./media/active-directory-saas-druva-tutorial/create_aaduser_04.png)

    <span data-ttu-id="f21ed-217">a.</span><span class="sxs-lookup"><span data-stu-id="f21ed-217">a.</span></span> <span data-ttu-id="f21ed-218">Az a **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f21ed-218">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f21ed-219">b.</span><span class="sxs-lookup"><span data-stu-id="f21ed-219">b.</span></span> <span data-ttu-id="f21ed-220">Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címe az Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f21ed-220">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="f21ed-221">c.</span><span class="sxs-lookup"><span data-stu-id="f21ed-221">c.</span></span> <span data-ttu-id="f21ed-222">Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="f21ed-222">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="f21ed-223">d.</span><span class="sxs-lookup"><span data-stu-id="f21ed-223">d.</span></span> <span data-ttu-id="f21ed-224">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="f21ed-224">Click **Create**.</span></span>
 
### <a name="create-a-druva-test-user"></a><span data-ttu-id="f21ed-225">Druva tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="f21ed-225">Create a Druva test user</span></span>

<span data-ttu-id="f21ed-226">Ahhoz, hogy az Azure AD-felhasználók Druva bejelentkezni, akkor ki kell építenie a Druva.</span><span class="sxs-lookup"><span data-stu-id="f21ed-226">In order to enable Azure AD users to log in to Druva, they must be provisioned into Druva.</span></span> <span data-ttu-id="f21ed-227">Druva, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="f21ed-227">In the case of Druva, provisioning is a manual task.</span></span>

<span data-ttu-id="f21ed-228">**Adja meg a felhasználók átadása, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f21ed-228">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="f21ed-229">Jelentkezzen be a **Druva** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="f21ed-229">Log in to your **Druva** company site as administrator.</span></span>

2. <span data-ttu-id="f21ed-230">Ugrás a **kezelése \> felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="f21ed-230">Go to **Manage \> Users**.</span></span>
   
   <span data-ttu-id="f21ed-231">![Felhasználók kezelése](./media/active-directory-saas-druva-tutorial/ic795097.png "felhasználók kezelése")</span><span class="sxs-lookup"><span data-stu-id="f21ed-231">![Manage Users](./media/active-directory-saas-druva-tutorial/ic795097.png "Manage Users")</span></span>

3. <span data-ttu-id="f21ed-232">Kattintson a **új**.</span><span class="sxs-lookup"><span data-stu-id="f21ed-232">Click **Create New**.</span></span>
   
   <span data-ttu-id="f21ed-233">![Felhasználók kezelése](./media/active-directory-saas-druva-tutorial/ic795098.png "felhasználók kezelése")</span><span class="sxs-lookup"><span data-stu-id="f21ed-233">![Manage Users](./media/active-directory-saas-druva-tutorial/ic795098.png "Manage Users")</span></span>

4. <span data-ttu-id="f21ed-234">Új felhasználó létrehozása párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="f21ed-234">On the Create New User dialog, perform the following steps:</span></span>
   
   <span data-ttu-id="f21ed-235">![Hozzon létre Új_felhasználó](./media/active-directory-saas-druva-tutorial/ic795099.png "Új_felhasználó létrehozása")</span><span class="sxs-lookup"><span data-stu-id="f21ed-235">![Create NewUser](./media/active-directory-saas-druva-tutorial/ic795099.png "Create NewUser")</span></span>
   
   <span data-ttu-id="f21ed-236">a.</span><span class="sxs-lookup"><span data-stu-id="f21ed-236">a.</span></span> <span data-ttu-id="f21ed-237">Az a **E-mail cím** szövegmező, adja meg az e-mail címét, például a felhasználó  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="f21ed-237">In the **Email address** textbox, enter the email of user like **brittasimon@contoso.com**.</span></span>
   
   <span data-ttu-id="f21ed-238">b.</span><span class="sxs-lookup"><span data-stu-id="f21ed-238">b.</span></span> <span data-ttu-id="f21ed-239">Az a **neve** szövegmező, írja be például a felhasználó nevét **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f21ed-239">In the **Name** textbox, enter the name of user like **BrittaSimon**.</span></span>
   
   <span data-ttu-id="f21ed-240">c.</span><span class="sxs-lookup"><span data-stu-id="f21ed-240">c.</span></span> <span data-ttu-id="f21ed-241">Kattintson a **létrehozza a felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="f21ed-241">Click **Create User**.</span></span>

>[!NOTE]
><span data-ttu-id="f21ed-242">Bármely más Druva felhasználói fiók létrehozása eszközök vagy Druva kiépíteni az Azure AD-felhasználói fiókok által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="f21ed-242">You can use any other Druva user account creation tools or APIs provided by Druva to provision Azure AD user accounts.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="f21ed-243">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="f21ed-243">Assign the Azure AD test user</span></span>

<span data-ttu-id="f21ed-244">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Druva Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="f21ed-244">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Druva.</span></span>

![A felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="f21ed-246">**Britta Simon hozzárendelése Druva, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f21ed-246">**To assign Britta Simon to Druva, perform the following steps:**</span></span>

1. <span data-ttu-id="f21ed-247">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f21ed-247">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="f21ed-249">Az alkalmazások listában válassza ki a **Druva**.</span><span class="sxs-lookup"><span data-stu-id="f21ed-249">In the applications list, select **Druva**.</span></span>

    ![Az alkalmazások listáját a Druva hivatkozás](./media/active-directory-saas-druva-tutorial/tutorial_druva_app.png)  

3. <span data-ttu-id="f21ed-251">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="f21ed-251">In the menu on the left, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202]

4. <span data-ttu-id="f21ed-253">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="f21ed-253">Click **Add** button.</span></span> <span data-ttu-id="f21ed-254">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f21ed-254">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="f21ed-256">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="f21ed-256">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="f21ed-257">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f21ed-257">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f21ed-258">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f21ed-258">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="f21ed-259">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="f21ed-259">Test single sign-on</span></span>

<span data-ttu-id="f21ed-260">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="f21ed-260">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="f21ed-261">Ha a hozzáférési panelen Druva csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Druva alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="f21ed-261">When you click the Druva tile in the Access Panel, you should get automatically signed-on to your Druva application.</span></span>
<span data-ttu-id="f21ed-262">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f21ed-262">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="f21ed-263">További források</span><span class="sxs-lookup"><span data-stu-id="f21ed-263">Additional resources</span></span>

* [<span data-ttu-id="f21ed-264">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="f21ed-264">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f21ed-265">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="f21ed-265">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-druva-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-druva-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-druva-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-druva-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-druva-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-druva-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-druva-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-druva-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-druva-tutorial/tutorial_general_203.png

