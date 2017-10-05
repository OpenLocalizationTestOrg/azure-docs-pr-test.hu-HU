---
title: "Oktatóanyag: Azure Active Directoryval integrált Onit |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Onit között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: bc479a28-8fcd-493f-ac53-681975a5149c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: jeedes
ms.openlocfilehash: 47c0055b89dbcf6a30a7f9ac5a33913e7bf463fa
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-onit"></a><span data-ttu-id="42653-103">Oktatóanyag: Azure Active Directoryval integrált Onit</span><span class="sxs-lookup"><span data-stu-id="42653-103">Tutorial: Azure Active Directory integration with Onit</span></span>

<span data-ttu-id="42653-104">Ebben az oktatóanyagban elsajátíthatja Onit integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="42653-104">In this tutorial, you learn how to integrate Onit with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="42653-105">Onit integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="42653-105">Integrating Onit with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="42653-106">Az Azure AD, aki hozzáfér Onit szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="42653-106">You can control in Azure AD who has access to Onit.</span></span>
- <span data-ttu-id="42653-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Onit (egyszeri bejelentkezés) számára a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="42653-107">You can enable your users to automatically get signed-on to Onit (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="42653-108">A fiók egyetlen központi helyen – az Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="42653-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="42653-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="42653-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="42653-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="42653-110">Prerequisites</span></span>

<span data-ttu-id="42653-111">Konfigurálása az Azure AD-integrációs Onit, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="42653-111">To configure Azure AD integration with Onit, you need the following items:</span></span>

- <span data-ttu-id="42653-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="42653-112">An Azure AD subscription</span></span>
- <span data-ttu-id="42653-113">Egy Onit egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="42653-113">An Onit single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="42653-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="42653-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="42653-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="42653-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="42653-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="42653-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="42653-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="42653-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="42653-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="42653-118">Scenario description</span></span>

<span data-ttu-id="42653-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="42653-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="42653-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="42653-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="42653-121">A gyűjteményből Onit hozzáadása</span><span class="sxs-lookup"><span data-stu-id="42653-121">Adding Onit from the gallery</span></span>
2. <span data-ttu-id="42653-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="42653-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-onit-from-the-gallery"></a><span data-ttu-id="42653-123">A gyűjteményből Onit hozzáadása</span><span class="sxs-lookup"><span data-stu-id="42653-123">Adding Onit from the gallery</span></span>
<span data-ttu-id="42653-124">Az Azure AD integrálása a Onit konfigurálásához kell hozzáadnia Onit a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="42653-124">To configure the integration of Onit into Azure AD, you need to add Onit from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="42653-125">**A gyűjteményből Onit hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="42653-125">**To add Onit from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="42653-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="42653-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="42653-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="42653-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="42653-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="42653-129">Then go to **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="42653-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="42653-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="42653-133">Írja be a keresőmezőbe, **Onit**, jelölje be **Onit** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="42653-133">In the search box, type **Onit**, select **Onit** from result panel then click **Add** button to add the application.</span></span>

    ![Az eredménylistában Onit](./media/active-directory-saas-onit-tutorial/tutorial_onit_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="42653-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="42653-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="42653-136">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Onit</span><span class="sxs-lookup"><span data-stu-id="42653-136">In this section, you configure and test Azure AD single sign-on with Onit based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="42653-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Onit a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="42653-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Onit is to a user in Azure AD.</span></span> <span data-ttu-id="42653-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Onit közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="42653-138">In other words, a link relationship between an Azure AD user and the related user in Onit needs to be established.</span></span>

<span data-ttu-id="42653-139">Onit, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="42653-139">In Onit, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="42653-140">Az Azure AD egyszeri bejelentkezést a Onit tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="42653-140">To configure and test Azure AD single sign-on with Onit, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="42653-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="42653-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="42653-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="42653-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="42653-143">**[Hozzon létre egy Onit tesztfelhasználó](#create-an-onit-test-user)**  - való Britta Simon valami Onit, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="42653-143">**[Create an Onit test user](#create-an-onit-test-user)** - to have a counterpart of Britta Simon in Onit that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="42653-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="42653-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="42653-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  ellenőrzése, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="42653-145">**[Test single sign-on](#test-single-sign-on)** to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="42653-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="42653-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="42653-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Onit alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="42653-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Onit application.</span></span>

<span data-ttu-id="42653-148">**Konfigurálása az Azure AD az egyszeri bejelentkezés Onit, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="42653-148">**To configure Azure AD single sign-on with Onit, perform the following steps:**</span></span>

1. <span data-ttu-id="42653-149">Az Azure portálon a a **Onit** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="42653-149">In the Azure portal, on the **Onit** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="42653-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="42653-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-onit-tutorial/tutorial_onit_samlbase.png)

3. <span data-ttu-id="42653-153">Az a **Onit tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="42653-153">On the **Onit Domain and URLs** section, perform the following steps:</span></span>

    ![Az egyszeri bejelentkezés információk Onit tartomány és az URL-címek](./media/active-directory-saas-onit-tutorial/tutorial_onit_url.png)

    <span data-ttu-id="42653-155">a.</span><span class="sxs-lookup"><span data-stu-id="42653-155">a.</span></span> <span data-ttu-id="42653-156">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<sub-domain>.onit.com`</span><span class="sxs-lookup"><span data-stu-id="42653-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<sub-domain>.onit.com`</span></span>

    <span data-ttu-id="42653-157">b.</span><span class="sxs-lookup"><span data-stu-id="42653-157">b.</span></span> <span data-ttu-id="42653-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<sub-domain>.onit.com`</span><span class="sxs-lookup"><span data-stu-id="42653-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<sub-domain>.onit.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="42653-159">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="42653-159">These values are not real.</span></span> <span data-ttu-id="42653-160">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="42653-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="42653-161">Ügyfél [Onit ügyfél-támogatási csoport](https://www.onit.com/support) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="42653-161">Contact [Onit Client support team](https://www.onit.com/support) to get these values.</span></span> 
 
4. <span data-ttu-id="42653-162">Az a **SAML-aláíró tanúsítványa** szakaszban, másolja a **UJJLENYOMAT** tanúsítvány értékét.</span><span class="sxs-lookup"><span data-stu-id="42653-162">On the **SAML Signing Certificate** section, copy the **THUMBPRINT** value of certificate.</span></span>

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-onit-tutorial/tutorial_onit_certificate.png) 

5. <span data-ttu-id="42653-164">Onit alkalmazás vár a SAML helyességi feltételek egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="42653-164">Onit application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="42653-165">Állítsa be a következő jogcímeket ehhez az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="42653-165">Please configure the following claims for this application.</span></span> <span data-ttu-id="42653-166">Ezek az attribútumok értékének kezelheti a **"Atrribute"** az alkalmazás lapján.</span><span class="sxs-lookup"><span data-stu-id="42653-166">You can manage the values of these attributes from the **"Atrribute"** tab of the application.</span></span> <span data-ttu-id="42653-167">Az alábbi képernyőfelvételen látható egy példa a.</span><span class="sxs-lookup"><span data-stu-id="42653-167">The following screenshot shows an example for this.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-onit-tutorial/tutorial_onit_attribute.png) 

6. <span data-ttu-id="42653-169">A a **felhasználói attribútumok** a szakasz a **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum, az ábrán látható módon, és hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="42653-169">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image and perform the following steps:</span></span>
    
    | <span data-ttu-id="42653-170">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="42653-170">Attribute Name</span></span> | <span data-ttu-id="42653-171">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="42653-171">Attribute Value</span></span> |
    | ------------------- | -------------------- |
    | <span data-ttu-id="42653-172">E-mailek</span><span class="sxs-lookup"><span data-stu-id="42653-172">email</span></span> | <span data-ttu-id="42653-173">User.mail</span><span class="sxs-lookup"><span data-stu-id="42653-173">user.mail</span></span> |
    
    <span data-ttu-id="42653-174">a.</span><span class="sxs-lookup"><span data-stu-id="42653-174">a.</span></span> <span data-ttu-id="42653-175">Kattintson a **Hozzáadás attribútum** megnyitásához a **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="42653-175">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-onit-tutorial/tutorial_attribute_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-onit-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="42653-178">b.</span><span class="sxs-lookup"><span data-stu-id="42653-178">b.</span></span> <span data-ttu-id="42653-179">Az a **neve** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="42653-179">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="42653-180">c.</span><span class="sxs-lookup"><span data-stu-id="42653-180">c.</span></span> <span data-ttu-id="42653-181">Az a **érték** kilistázásához írja be a sorhoz látható attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="42653-181">From the **Value** list, type the attribute value shown for that row.</span></span>

    <span data-ttu-id="42653-182">d.</span><span class="sxs-lookup"><span data-stu-id="42653-182">d.</span></span> <span data-ttu-id="42653-183">Hagyja a **Namespace** üres.</span><span class="sxs-lookup"><span data-stu-id="42653-183">Leave the **Namespace** blank.</span></span>
    
    <span data-ttu-id="42653-184">e.</span><span class="sxs-lookup"><span data-stu-id="42653-184">e.</span></span> <span data-ttu-id="42653-185">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="42653-185">Click **Ok**.</span></span>

7. <span data-ttu-id="42653-186">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="42653-186">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-onit-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="42653-188">A a **Onit konfigurációs** kattintson **konfigurálása Onit** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="42653-188">On the **Onit Configuration** section, click **Configure Onit** to open **Configure sign-on** window.</span></span> <span data-ttu-id="42653-189">Másolás a **Sign-Out URL, SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="42653-189">Copy the **Sign-Out URL, SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Onit konfiguráció](./media/active-directory-saas-onit-tutorial/tutorial_onit_configure.png)

9. <span data-ttu-id="42653-191">Egy másik webes böngészőablakban jelentkezzen be a Onit vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="42653-191">In a different web browser window, log into your Onit company site as an administrator.</span></span>

10. <span data-ttu-id="42653-192">Kattintson a felső menüben **felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="42653-192">In the menu on the top, click **Administration**.</span></span>
   
   <span data-ttu-id="42653-193">![Felügyeleti](./media/active-directory-saas-onit-tutorial/IC791174.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="42653-193">![Administration](./media/active-directory-saas-onit-tutorial/IC791174.png "Administration")</span></span>
11. <span data-ttu-id="42653-194">Kattintson a **Szerkesztés Corporation**.</span><span class="sxs-lookup"><span data-stu-id="42653-194">Click **Edit Corporation**.</span></span>
   
   <span data-ttu-id="42653-195">![Szerkesztés Corporation](./media/active-directory-saas-onit-tutorial/IC791175.png "Szerkesztés Corporation")</span><span class="sxs-lookup"><span data-stu-id="42653-195">![Edit Corporation](./media/active-directory-saas-onit-tutorial/IC791175.png "Edit Corporation")</span></span>
   
12. <span data-ttu-id="42653-196">Kattintson a **biztonsági** fülre.</span><span class="sxs-lookup"><span data-stu-id="42653-196">Click the **Security** tab.</span></span>
    
    <span data-ttu-id="42653-197">![Vállalat adatainak szerkesztése](./media/active-directory-saas-onit-tutorial/IC791176.png "vállalat adatainak szerkesztése")</span><span class="sxs-lookup"><span data-stu-id="42653-197">![Edit Company Information](./media/active-directory-saas-onit-tutorial/IC791176.png "Edit Company Information")</span></span>

13. <span data-ttu-id="42653-198">Az a **biztonsági** lapra, hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="42653-198">On the **Security** tab, perform the following steps:</span></span>

    <span data-ttu-id="42653-199">![Egyszeri bejelentkezés](./media/active-directory-saas-onit-tutorial/IC791177.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="42653-199">![Single Sign-On](./media/active-directory-saas-onit-tutorial/IC791177.png "Single Sign-On")</span></span>

    <span data-ttu-id="42653-200">a.</span><span class="sxs-lookup"><span data-stu-id="42653-200">a.</span></span> <span data-ttu-id="42653-201">Mint **hitelesítési stratégia**, jelölje be **egyszeri bejelentkezés és a jelszó**.</span><span class="sxs-lookup"><span data-stu-id="42653-201">As **Authentication Strategy**, select **Single Sign On and Password**.</span></span>
    
    <span data-ttu-id="42653-202">b.</span><span class="sxs-lookup"><span data-stu-id="42653-202">b.</span></span> <span data-ttu-id="42653-203">A **Idp célként megadott URL** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="42653-203">In **Idp Target URL** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="42653-204">c.</span><span class="sxs-lookup"><span data-stu-id="42653-204">c.</span></span> <span data-ttu-id="42653-205">A **Idp kijelentkezési URL-cím** szövegmezőhöz illessze be az értékét **Sign-Out URL-cím**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="42653-205">In **Idp logout URL** textbox, paste the value of  **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="42653-206">d.</span><span class="sxs-lookup"><span data-stu-id="42653-206">d.</span></span> <span data-ttu-id="42653-207">A **Idp tanúsítvány-ujjlenyomat (SHA1)** szövegmező, illessze be a **ujjlenyomat** tanúsítványt, amely az Azure-portálon másolta értékét.</span><span class="sxs-lookup"><span data-stu-id="42653-207">In **Idp Cert Fingerprint (SHA1)** textbox, paste the  **Thumbprint** value of certificate, which you have copied from Azure portal.</span></span>

> [!TIP]
> <span data-ttu-id="42653-208">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="42653-208">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="42653-209">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="42653-209">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="42653-210">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="42653-210">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="42653-211">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="42653-211">Create an Azure AD test user</span></span>

<span data-ttu-id="42653-212">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="42653-212">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="42653-214">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="42653-214">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="42653-215">Az Azure portálon a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="42653-215">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-onit-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="42653-217">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="42653-217">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-onit-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="42653-219">Megnyitásához a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** tetején a **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="42653-219">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![A Hozzáadás gombra.](./media/active-directory-saas-onit-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="42653-221">Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="42653-221">In the **User** dialog box, perform the following steps:</span></span>

    ![A felhasználó párbeszédpanel](./media/active-directory-saas-onit-tutorial/create_aaduser_04.png)

    <span data-ttu-id="42653-223">a.</span><span class="sxs-lookup"><span data-stu-id="42653-223">a.</span></span> <span data-ttu-id="42653-224">Az a **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="42653-224">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="42653-225">b.</span><span class="sxs-lookup"><span data-stu-id="42653-225">b.</span></span> <span data-ttu-id="42653-226">Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címe az Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="42653-226">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="42653-227">c.</span><span class="sxs-lookup"><span data-stu-id="42653-227">c.</span></span> <span data-ttu-id="42653-228">Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="42653-228">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="42653-229">d.</span><span class="sxs-lookup"><span data-stu-id="42653-229">d.</span></span> <span data-ttu-id="42653-230">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="42653-230">Click **Create**.</span></span>
 
### <a name="create-an-onit-test-user"></a><span data-ttu-id="42653-231">Hozzon létre egy Onit tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="42653-231">Create an Onit test user</span></span>

<span data-ttu-id="42653-232">Ahhoz, hogy az Azure AD-felhasználók Onit bejelentkezni, akkor ki kell építenie Onit be.</span><span class="sxs-lookup"><span data-stu-id="42653-232">In order to enable Azure AD users to log into Onit, they must be provisioned into Onit.</span></span>  

<span data-ttu-id="42653-233">Onit, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="42653-233">In the case of Onit, provisioning is a manual task.</span></span>

<span data-ttu-id="42653-234">**Adja meg a felhasználók átadása, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="42653-234">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="42653-235">Jelentkezzen be a **Onit** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="42653-235">Sign on to your **Onit** company site as an administrator.</span></span>
2. <span data-ttu-id="42653-236">Kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="42653-236">Click **Add User**.</span></span>
   
   <span data-ttu-id="42653-237">![Felügyeleti](./media/active-directory-saas-onit-tutorial/IC791180.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="42653-237">![Administration](./media/active-directory-saas-onit-tutorial/IC791180.png "Administration")</span></span>
3. <span data-ttu-id="42653-238">Az a **felhasználó hozzáadása** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="42653-238">On the **Add User** dialog page, perform the following steps:</span></span>
   
   <span data-ttu-id="42653-239">![Felhasználó hozzáadása](./media/active-directory-saas-onit-tutorial/IC791181.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="42653-239">![Add User](./media/active-directory-saas-onit-tutorial/IC791181.png "Add User")</span></span>
   
  1. <span data-ttu-id="42653-240">Típus a **neve** és a **E-mail cím** egy érvényes Azure AD-fiókot szeretné azokat a kapcsolódó szövegmezők kiépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="42653-240">Type the **Name** and the **Email Address** of a valid Azure AD account you want to provision into the related textboxes.</span></span>
  2. <span data-ttu-id="42653-241">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="42653-241">Click **Create**.</span></span>    
   
 > [!NOTE]
 > <span data-ttu-id="42653-242">Az Azure Active Directory fióktulajdonos kap egy e-mailt, és a következő hivatkozás a fiók megerősítéséhez, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="42653-242">The Azure Active Directory account holder receives an email and follows a link to confirm their account before it becomes active.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="42653-243">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="42653-243">Assign the Azure AD test user</span></span>

<span data-ttu-id="42653-244">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Onit Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="42653-244">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Onit.</span></span>

![A felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="42653-246">**Britta Simon hozzárendelése Onit, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="42653-246">**To assign Britta Simon to Onit, perform the following steps:**</span></span>

1. <span data-ttu-id="42653-247">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="42653-247">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="42653-249">Az alkalmazások listában válassza ki a **Onit**.</span><span class="sxs-lookup"><span data-stu-id="42653-249">In the applications list, select **Onit**.</span></span>

    ![Az alkalmazások listáját a Onit hivatkozás](./media/active-directory-saas-onit-tutorial/tutorial_onit_app.png)  

3. <span data-ttu-id="42653-251">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="42653-251">In the menu on the left, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202]

4. <span data-ttu-id="42653-253">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="42653-253">Click **Add** button.</span></span> <span data-ttu-id="42653-254">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="42653-254">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="42653-256">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="42653-256">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="42653-257">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="42653-257">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="42653-258">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="42653-258">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="42653-259">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="42653-259">Test single sign-on</span></span>

<span data-ttu-id="42653-260">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="42653-260">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="42653-261">Ha a hozzáférési panelen Onit csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Onit alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="42653-261">When you click the Onit tile in the Access Panel, you should get automatically signed-on to your Onit application.</span></span>
<span data-ttu-id="42653-262">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="42653-262">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="42653-263">További források</span><span class="sxs-lookup"><span data-stu-id="42653-263">Additional resources</span></span>

* [<span data-ttu-id="42653-264">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="42653-264">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="42653-265">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="42653-265">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-onit-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-onit-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-onit-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-onit-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-onit-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-onit-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-onit-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-onit-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-onit-tutorial/tutorial_general_203.png

