---
title: "Oktatóanyag: Azure Active Directoryval integrált etouches |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és etouches között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 76cccaa8-859c-4c16-9d1d-8a6496fc7520
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 3cd9e9d6aae924369065ca492b1f6380c0ddc5fe
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-etouches"></a><span data-ttu-id="251e4-103">Oktatóanyag: Azure Active Directoryval integrált etouches</span><span class="sxs-lookup"><span data-stu-id="251e4-103">Tutorial: Azure Active Directory integration with etouches</span></span>

<span data-ttu-id="251e4-104">Ebben az oktatóanyagban elsajátíthatja etouches integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="251e4-104">In this tutorial, you learn how to integrate etouches with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="251e4-105">Etouches integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="251e4-105">Integrating etouches with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="251e4-106">Megadhatja a etouches hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="251e4-106">You can control in Azure AD who has access to etouches</span></span>
- <span data-ttu-id="251e4-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a etouches (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="251e4-107">You can enable your users to automatically get signed-on to etouches (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="251e4-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="251e4-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="251e4-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="251e4-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="251e4-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="251e4-110">Prerequisites</span></span>

<span data-ttu-id="251e4-111">Konfigurálása az Azure AD-integrációs etouches, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="251e4-111">To configure Azure AD integration with etouches, you need the following items:</span></span>

- <span data-ttu-id="251e4-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="251e4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="251e4-113">Egy etouches egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="251e4-113">An etouches single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="251e4-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="251e4-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="251e4-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="251e4-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="251e4-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="251e4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="251e4-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="251e4-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="251e4-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="251e4-118">Scenario description</span></span>
<span data-ttu-id="251e4-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="251e4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="251e4-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="251e4-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="251e4-121">A gyűjteményből etouches hozzáadása</span><span class="sxs-lookup"><span data-stu-id="251e4-121">Adding etouches from the gallery</span></span>
2. <span data-ttu-id="251e4-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="251e4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-etouches-from-the-gallery"></a><span data-ttu-id="251e4-123">A gyűjteményből etouches hozzáadása</span><span class="sxs-lookup"><span data-stu-id="251e4-123">Adding etouches from the gallery</span></span>
<span data-ttu-id="251e4-124">Az Azure AD integrálása a etouches konfigurálásához kell hozzáadnia etouches a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="251e4-124">To configure the integration of etouches into Azure AD, you need to add etouches from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="251e4-125">**A gyűjteményből etouches hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="251e4-125">**To add etouches from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="251e4-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="251e4-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="251e4-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="251e4-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="251e4-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="251e4-129">Then go to **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="251e4-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="251e4-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="251e4-133">Írja be a keresőmezőbe, **etouches**, jelölje be **etouches** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="251e4-133">In the search box, type **etouches**, select **etouches** from result panel then click **Add** button to add the application.</span></span>

    ![az eredménylistában etouches](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="251e4-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="251e4-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="251e4-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján etouches.</span><span class="sxs-lookup"><span data-stu-id="251e4-136">In this section, you configure and test Azure AD single sign-on with etouches based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="251e4-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó etouches a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="251e4-137">For single sign-on to work, Azure AD needs to know what the counterpart user in etouches is to a user in Azure AD.</span></span> <span data-ttu-id="251e4-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a etouches közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="251e4-138">In other words, a link relationship between an Azure AD user and the related user in etouches needs to be established.</span></span>

<span data-ttu-id="251e4-139">Etouches, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="251e4-139">In etouches, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="251e4-140">Az Azure AD egyszeri bejelentkezést a etouches tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="251e4-140">To configure and test Azure AD single sign-on with etouches, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="251e4-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="251e4-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="251e4-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="251e4-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="251e4-143">**[Hozzon létre egy etouches tesztfelhasználó](#create-an-etouches-test-user)**  - Britta Simon egy partner, a felhasználó az Azure AD ábrázolását kapcsolódó etouches rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="251e4-143">**[Create an etouches test user](#create-an-etouches-test-user)** - to have a counterpart of Britta Simon in etouches that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="251e4-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="251e4-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="251e4-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="251e4-145">**[Test Single Sign-On](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="251e4-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="251e4-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="251e4-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az etouches alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="251e4-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your etouches application.</span></span>

<span data-ttu-id="251e4-148">**Konfigurálása az Azure AD az egyszeri bejelentkezés etouches, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="251e4-148">**To configure Azure AD single sign-on with etouches, perform the following steps:**</span></span>

1. <span data-ttu-id="251e4-149">Az Azure portálon a a **etouches** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="251e4-149">In the Azure portal, on the **etouches** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="251e4-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="251e4-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_samlbase.png)

3. <span data-ttu-id="251e4-153">Az a **etouches tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="251e4-153">On the **etouches Domain and URLs** section, perform the following steps:</span></span>

    ![az egyszeri bejelentkezési adatokat etouches tartomány és az URL-címek](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_url.png)

    <span data-ttu-id="251e4-155">a.</span><span class="sxs-lookup"><span data-stu-id="251e4-155">a.</span></span> <span data-ttu-id="251e4-156">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://www.eiseverywhere.com/saml/accounts/?sso&accountid=<ACCOUNTID>`</span><span class="sxs-lookup"><span data-stu-id="251e4-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://www.eiseverywhere.com/saml/accounts/?sso&accountid=<ACCOUNTID>`</span></span>

    <span data-ttu-id="251e4-157">b.</span><span class="sxs-lookup"><span data-stu-id="251e4-157">b.</span></span> <span data-ttu-id="251e4-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://www.eiseverywhere.com/<instance name>`</span><span class="sxs-lookup"><span data-stu-id="251e4-158">In the **Identifier** textbox, type a URL using the following pattern: `https://www.eiseverywhere.com/<instance name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="251e4-159">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="251e4-159">These values are not real.</span></span> <span data-ttu-id="251e4-160">A tényleges bejelentkezési URL-cím és az azonosítóját, amelynek az ismertetése, az oktatóanyag későbbi részében frissítse az értéket.</span><span class="sxs-lookup"><span data-stu-id="251e4-160">You update the value with the actual Sign on URL and Identifier, which is explained later in the tutorial.</span></span>
    > 

4. <span data-ttu-id="251e4-161">etouches alkalmazás vár a SAML helyességi feltételek egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="251e4-161">etouches application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="251e4-162">A következő jogcímek alkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="251e4-162">Configure the following claims for this application.</span></span> <span data-ttu-id="251e4-163">Ezek az attribútumok értékének kezelheti a **felhasználói attribútum** az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="251e4-163">You can manage the values of these attributes from the **User Attribute** of the application.</span></span> <span data-ttu-id="251e4-164">Az alábbi képernyőfelvételen látható egy példa a.</span><span class="sxs-lookup"><span data-stu-id="251e4-164">The following screenshot shows an example for this.</span></span> 

    ![Felhasználói attribútum](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_attribute.png) 

5. <span data-ttu-id="251e4-166">A a **felhasználói attribútumok** a szakasz a **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum, az ábrán látható módon, és hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="251e4-166">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image and perform the following steps:</span></span>
    
    | <span data-ttu-id="251e4-167">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="251e4-167">Attribute Name</span></span> | <span data-ttu-id="251e4-168">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="251e4-168">Attribute Value</span></span> |
    | ------------------- | -------------------- |
    | <span data-ttu-id="251e4-169">E-mail cím</span><span class="sxs-lookup"><span data-stu-id="251e4-169">Email</span></span> | <span data-ttu-id="251e4-170">User.mail</span><span class="sxs-lookup"><span data-stu-id="251e4-170">user.mail</span></span> |    
    
    <span data-ttu-id="251e4-171">a.</span><span class="sxs-lookup"><span data-stu-id="251e4-171">a.</span></span> <span data-ttu-id="251e4-172">Kattintson a **Hozzáadás attribútum** megnyitásához a **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="251e4-172">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Attribútum hozzáadása](./media/active-directory-saas-etouches-tutorial/tutorial_attribute_04.png)

    ![Attribútum párbeszédpanel hozzáadása](./media/active-directory-saas-etouches-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="251e4-175">b.</span><span class="sxs-lookup"><span data-stu-id="251e4-175">b.</span></span> <span data-ttu-id="251e4-176">Az a **neve** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="251e4-176">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="251e4-177">c.</span><span class="sxs-lookup"><span data-stu-id="251e4-177">c.</span></span> <span data-ttu-id="251e4-178">Az a **érték** kilistázásához írja be a sorhoz látható attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="251e4-178">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="251e4-179">d.</span><span class="sxs-lookup"><span data-stu-id="251e4-179">d.</span></span> <span data-ttu-id="251e4-180">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="251e4-180">Click **Ok**.</span></span> 

6. <span data-ttu-id="251e4-181">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="251e4-181">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_certificate.png) 

7. <span data-ttu-id="251e4-183">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="251e4-183">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-etouches-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="251e4-185">Ahhoz, hogy az egyszeri bejelentkezés az alkalmazáshoz konfigurált, a következő lépésekkel az etouches alkalmazásban:</span><span class="sxs-lookup"><span data-stu-id="251e4-185">To get SSO configured for your application, perform the following steps in the etouches application:</span></span> 

    ![etouches konfiguráció](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_06.png) 

    <span data-ttu-id="251e4-187">a.</span><span class="sxs-lookup"><span data-stu-id="251e4-187">a.</span></span> <span data-ttu-id="251e4-188">Jelentkezzen be **etouches** alkalmazást a rendszergazdai jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="251e4-188">Login to **etouches** application using the Admin rights.</span></span>
   
    <span data-ttu-id="251e4-189">b.</span><span class="sxs-lookup"><span data-stu-id="251e4-189">b.</span></span> <span data-ttu-id="251e4-190">Lépjen a **SAML** konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="251e4-190">Go to the **SAML** Configuration.</span></span>

    <span data-ttu-id="251e4-191">c.</span><span class="sxs-lookup"><span data-stu-id="251e4-191">c.</span></span> <span data-ttu-id="251e4-192">Az a **általános beállítások** szakaszt, nyissa meg a letöltött tanúsítvány Azure-portálon a Jegyzettömbben, másolja a tartalmat, és illessze be a kiállító terjesztési hely metaadatok szövegmezőjét.</span><span class="sxs-lookup"><span data-stu-id="251e4-192">In the **General Settings** section, open your downloaded certificate from Azure portal in notepad, copy the content, and then paste it into the IDP metadata textbox.</span></span> 

    <span data-ttu-id="251e4-193">d.</span><span class="sxs-lookup"><span data-stu-id="251e4-193">d.</span></span> <span data-ttu-id="251e4-194">Kattintson a **mentése & kapcsolat** gombra.</span><span class="sxs-lookup"><span data-stu-id="251e4-194">Click on the **Save & Stay** button.</span></span>
  
    <span data-ttu-id="251e4-195">e.</span><span class="sxs-lookup"><span data-stu-id="251e4-195">e.</span></span> <span data-ttu-id="251e4-196">Kattintson a **frissítéshez tartozó metaadatokat** a SAML metaadatszakasz gombjára.</span><span class="sxs-lookup"><span data-stu-id="251e4-196">Click on the **Update Metadata** button in the SAML Metadata section.</span></span> 

    <span data-ttu-id="251e4-197">f.</span><span class="sxs-lookup"><span data-stu-id="251e4-197">f.</span></span> <span data-ttu-id="251e4-198">Ez megnyitja a lapot, és hajtsa végre az egyszeri Bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="251e4-198">This opens the page and perform SSO.</span></span> <span data-ttu-id="251e4-199">Miután működik-e az SSO majd állíthat be a felhasználónevet.</span><span class="sxs-lookup"><span data-stu-id="251e4-199">Once the SSO is working then you can set up the username.</span></span>

    <span data-ttu-id="251e4-200">g.</span><span class="sxs-lookup"><span data-stu-id="251e4-200">g.</span></span> <span data-ttu-id="251e4-201">A felhasználónév mezőben válassza ki a **emailaddress** az alábbi ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="251e4-201">In the Username field, select the **emailaddress** as shown in the image below.</span></span> 

    <span data-ttu-id="251e4-202">h.</span><span class="sxs-lookup"><span data-stu-id="251e4-202">h.</span></span> <span data-ttu-id="251e4-203">Másolás a **SP Entitásazonosító** értékét, és illessze be azt a **azonosító** szövegmező, amely **etouches tartomány és az URL-címek** Azure-portál szakaszban.</span><span class="sxs-lookup"><span data-stu-id="251e4-203">Copy the **SP entity ID** value and paste it into the **Identifier**  textbox, which is in **etouches Domain and URLs** section on Azure portal.</span></span>

    <span data-ttu-id="251e4-204">i.</span><span class="sxs-lookup"><span data-stu-id="251e4-204">i.</span></span> <span data-ttu-id="251e4-205">Másolás a **egyszeri bejelentkezési URL-cím / ACS** értékét, és illessze be azt a **bejelentkezési URL-cím** szövegmező, amely **etouches tartomány és az URL-címek** Azure-portál szakaszban.</span><span class="sxs-lookup"><span data-stu-id="251e4-205">Copy the **SSO URL / ACS** value and paste it into the **Sign on URL** textbox, which is in **etouches Domain and URLs** section on Azure portal.</span></span>
   
> [!TIP]
> <span data-ttu-id="251e4-206">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="251e4-206">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="251e4-207">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="251e4-207">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="251e4-208">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="251e4-208">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="251e4-209">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="251e4-209">Create an Azure AD test user</span></span>
<span data-ttu-id="251e4-210">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="251e4-210">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="251e4-212">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="251e4-212">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="251e4-213">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="251e4-213">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-etouches-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="251e4-215">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="251e4-215">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-etouches-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="251e4-217">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="251e4-217">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![A Hozzáadás gombra.](./media/active-directory-saas-etouches-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="251e4-219">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="251e4-219">On the **User** dialog page, perform the following steps:</span></span>
 
    ![A felhasználó párbeszédpanel](./media/active-directory-saas-etouches-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="251e4-221">a.</span><span class="sxs-lookup"><span data-stu-id="251e4-221">a.</span></span> <span data-ttu-id="251e4-222">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="251e4-222">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="251e4-223">b.</span><span class="sxs-lookup"><span data-stu-id="251e4-223">b.</span></span> <span data-ttu-id="251e4-224">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="251e4-224">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="251e4-225">c.</span><span class="sxs-lookup"><span data-stu-id="251e4-225">c.</span></span> <span data-ttu-id="251e4-226">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="251e4-226">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="251e4-227">d.</span><span class="sxs-lookup"><span data-stu-id="251e4-227">d.</span></span> <span data-ttu-id="251e4-228">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="251e4-228">Click **Create**.</span></span>
 
### <a name="create-an-etouches-test-user"></a><span data-ttu-id="251e4-229">Hozzon létre egy etouches tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="251e4-229">Create an etouches test user</span></span>

<span data-ttu-id="251e4-230">Ebben a szakaszban egy etouches Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="251e4-230">In this section, you create a user called Britta Simon in etouches.</span></span> <span data-ttu-id="251e4-231">Együttműködve [etouches ügyfél-támogatási csoport](https://www.etouches.com/event-software/support/customer-support/) a felhasználók hozzáadása a etouches platform.</span><span class="sxs-lookup"><span data-stu-id="251e4-231">Work with [etouches Client support team](https://www.etouches.com/event-software/support/customer-support/) to add the users in the etouches platform.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="251e4-232">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="251e4-232">Assign the Azure AD test user</span></span>

<span data-ttu-id="251e4-233">Ebben a szakaszban engedélyezze Britta Simon Azure egyszeri bejelentkezéshez használandó etouches való hozzáférés biztosítása.</span><span class="sxs-lookup"><span data-stu-id="251e4-233">In this section, you enable Britta Simon to use Azure single sign-on by granting access to etouches.</span></span>

![A felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="251e4-235">**Britta Simon hozzárendelése etouches, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="251e4-235">**To assign Britta Simon to etouches, perform the following steps:**</span></span>

1. <span data-ttu-id="251e4-236">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="251e4-236">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="251e4-238">Az alkalmazások listában válassza ki a **etouches**.</span><span class="sxs-lookup"><span data-stu-id="251e4-238">In the applications list, select **etouches**.</span></span>

    ![Az alkalmazások listáját a etouches hivatkozás](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_app.png) 

3. <span data-ttu-id="251e4-240">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="251e4-240">In the menu on the left, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202] 

4. <span data-ttu-id="251e4-242">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="251e4-242">Click **Add** button.</span></span> <span data-ttu-id="251e4-243">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="251e4-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="251e4-245">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="251e4-245">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="251e4-246">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="251e4-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="251e4-247">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="251e4-247">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="251e4-248">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="251e4-248">Test single sign-on</span></span>


<span data-ttu-id="251e4-249">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="251e4-249">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="251e4-250">Ha a hozzáférési panelen etouches csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az etouches alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="251e4-250">When you click the etouches tile in the Access Panel, you should get automatically signed-on to your etouches application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="251e4-251">További források</span><span class="sxs-lookup"><span data-stu-id="251e4-251">Additional resources</span></span>

* [<span data-ttu-id="251e4-252">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="251e4-252">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="251e4-253">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="251e4-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_203.png

