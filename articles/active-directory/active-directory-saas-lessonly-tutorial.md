---
title: "Oktatóanyag: Azure Active Directoryval integrált Lesson.ly |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Lesson.ly között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8c9dc6e6-5d85-4553-8a35-c7137064b928
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: fc1e1b2de0a138dbe88d794f802b002321948ab8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lessonly"></a><span data-ttu-id="49c78-103">Oktatóanyag: Azure Active Directoryval integrált Lesson.ly</span><span class="sxs-lookup"><span data-stu-id="49c78-103">Tutorial: Azure Active Directory integration with Lesson.ly</span></span>

<span data-ttu-id="49c78-104">Ebben az oktatóanyagban elsajátíthatja Lesson.ly integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="49c78-104">In this tutorial, you learn how to integrate Lesson.ly with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="49c78-105">Lesson.ly integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="49c78-105">Integrating Lesson.ly with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="49c78-106">Megadhatja a Lesson.ly hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="49c78-106">You can control in Azure AD who has access to Lesson.ly</span></span>
- <span data-ttu-id="49c78-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Lesson.ly (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="49c78-107">You can enable your users to automatically get signed-on to Lesson.ly (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="49c78-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="49c78-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="49c78-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="49c78-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="49c78-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="49c78-110">Prerequisites</span></span>

<span data-ttu-id="49c78-111">Konfigurálása az Azure AD-integrációs Lesson.ly, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="49c78-111">To configure Azure AD integration with Lesson.ly, you need the following items:</span></span>

- <span data-ttu-id="49c78-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="49c78-112">An Azure AD subscription</span></span>
- <span data-ttu-id="49c78-113">Egy Lesson.ly egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="49c78-113">A Lesson.ly single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="49c78-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="49c78-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="49c78-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="49c78-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="49c78-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="49c78-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="49c78-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="49c78-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="49c78-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="49c78-118">Scenario description</span></span>
<span data-ttu-id="49c78-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="49c78-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="49c78-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="49c78-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="49c78-121">A gyűjteményből Lesson.ly hozzáadása</span><span class="sxs-lookup"><span data-stu-id="49c78-121">Adding Lesson.ly from the gallery</span></span>
2. <span data-ttu-id="49c78-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="49c78-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lessonly-from-the-gallery"></a><span data-ttu-id="49c78-123">A gyűjteményből Lesson.ly hozzáadása</span><span class="sxs-lookup"><span data-stu-id="49c78-123">Adding Lesson.ly from the gallery</span></span>
<span data-ttu-id="49c78-124">Az Azure AD integrálása a Lesson.ly konfigurálásához kell hozzáadnia Lesson.ly a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="49c78-124">To configure the integration of Lesson.ly into Azure AD, you need to add Lesson.ly from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="49c78-125">**A gyűjteményből Lesson.ly hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="49c78-125">**To add Lesson.ly from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="49c78-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="49c78-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="49c78-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="49c78-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="49c78-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="49c78-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="49c78-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="49c78-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="49c78-133">Írja be a keresőmezőbe, **Lesson.ly**.</span><span class="sxs-lookup"><span data-stu-id="49c78-133">In the search box, type **Lesson.ly**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_search.png)

5. <span data-ttu-id="49c78-135">Az eredmények panelen válassza ki a **Lesson.ly**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="49c78-135">In the results panel, select **Lesson.ly**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="49c78-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="49c78-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="49c78-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Lesson.ly.</span><span class="sxs-lookup"><span data-stu-id="49c78-138">In this section, you configure and test Azure AD single sign-on with Lesson.ly based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="49c78-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Lesson.ly a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="49c78-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Lesson.ly is to a user in Azure AD.</span></span> <span data-ttu-id="49c78-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Lesson.ly közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="49c78-140">In other words, a link relationship between an Azure AD user and the related user in Lesson.ly needs to be established.</span></span>

<span data-ttu-id="49c78-141">Lesson.ly, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="49c78-141">In Lesson.ly, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="49c78-142">Az Azure AD egyszeri bejelentkezést a Lesson.ly tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="49c78-142">To configure and test Azure AD single sign-on with Lesson.ly, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="49c78-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="49c78-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="49c78-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="49c78-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="49c78-145">**[Lesson.ly tesztfelhasználó létrehozása](#creating-a-lessonly-test-user)**  - való Britta Simon valami Lesson.ly, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="49c78-145">**[Creating a Lesson.ly test user](#creating-a-lessonly-test-user)** - to have a counterpart of Britta Simon in Lesson.ly that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="49c78-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="49c78-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="49c78-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="49c78-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="49c78-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="49c78-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="49c78-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Lesson.ly alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="49c78-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Lesson.ly application.</span></span>

<span data-ttu-id="49c78-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Lesson.ly, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="49c78-150">**To configure Azure AD single sign-on with Lesson.ly, perform the following steps:**</span></span>

1. <span data-ttu-id="49c78-151">Az Azure portálon a a **Lesson.ly** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="49c78-151">In the Azure portal, on the **Lesson.ly** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="49c78-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="49c78-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_samlbase.png)

3. <span data-ttu-id="49c78-155">Az a **Lesson.ly tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="49c78-155">On the **Lesson.ly Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_url.png)

    <span data-ttu-id="49c78-157">a.</span><span class="sxs-lookup"><span data-stu-id="49c78-157">a.</span></span> <span data-ttu-id="49c78-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:</span><span class="sxs-lookup"><span data-stu-id="49c78-158">In the **Sign-on URL** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.lesson.ly/signin`|
    | `https://<companyname>.lessonly.com/signin`|

    >[!NOTE]
    ><span data-ttu-id="49c78-159">Ha egy általános hivatkozó neve, amely **#companyname** egy tényleges név le kell cserélni.</span><span class="sxs-lookup"><span data-stu-id="49c78-159">When referencing a generic name that **companyname** needs to be replaced by an actual name.</span></span>
    
    <span data-ttu-id="49c78-160">b.</span><span class="sxs-lookup"><span data-stu-id="49c78-160">b.</span></span> <span data-ttu-id="49c78-161">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:</span><span class="sxs-lookup"><span data-stu-id="49c78-161">In the **Identifier** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.lesson.ly/auth/saml/metadata`|
    | `https://<companyname>.lessonly.com/auth/saml/metadata`|

    > [!NOTE] 
    > <span data-ttu-id="49c78-162">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="49c78-162">These values are not real.</span></span> <span data-ttu-id="49c78-163">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="49c78-163">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="49c78-164">Ügyfél [Lesson.ly ügyfél-támogatási csoport](mailto:dev@lessonly.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="49c78-164">Contact [Lesson.ly Client support team](mailto:dev@lessonly.com) to get these values.</span></span> 

4. <span data-ttu-id="49c78-165">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="49c78-165">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_certificate.png)

5. <span data-ttu-id="49c78-167">A Lesson.ly alkalmazás vár a SAML helyességi feltételek egy meghatározott formátumban, amelyhez egyéni attribútum leképezései hozzáadása a **SAML-jogkivonat attribútumok** konfigurációs. Az alábbi képernyőfelvételen látható egy példa a.</span><span class="sxs-lookup"><span data-stu-id="49c78-167">The Lesson.ly application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your **SAML Token Attributes** configuration.The following screenshot shows an example for this.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_06.png)
           
6. <span data-ttu-id="49c78-169">A a **felhasználói attribútumok** a szakasz a **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum, az előző ábrán látható módon, és hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="49c78-169">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the preceding image and perform the following steps:</span></span>

    | <span data-ttu-id="49c78-170">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="49c78-170">Attribute Name</span></span>   | <span data-ttu-id="49c78-171">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="49c78-171">Attribute Value</span></span> |
    | ---------------  | ----------------|
    | <span data-ttu-id="49c78-172">urn: oid:2.5.4.42</span><span class="sxs-lookup"><span data-stu-id="49c78-172">urn:oid:2.5.4.42</span></span> |<span data-ttu-id="49c78-173">User.givenName</span><span class="sxs-lookup"><span data-stu-id="49c78-173">user.givenname</span></span> |
    | <span data-ttu-id="49c78-174">urn: oid:2.5.4.4</span><span class="sxs-lookup"><span data-stu-id="49c78-174">urn:oid:2.5.4.4</span></span>  |<span data-ttu-id="49c78-175">User.surname</span><span class="sxs-lookup"><span data-stu-id="49c78-175">user.surname</span></span> |
    | <span data-ttu-id="49c78-176">urn: oid:0.9.2342.19200300.1001.3</span><span class="sxs-lookup"><span data-stu-id="49c78-176">urn:oid:0.9.2342.19200300.1001.3</span></span> |<span data-ttu-id="49c78-177">User.mail</span><span class="sxs-lookup"><span data-stu-id="49c78-177">user.mail</span></span> |

    <span data-ttu-id="49c78-178">a.</span><span class="sxs-lookup"><span data-stu-id="49c78-178">a.</span></span> <span data-ttu-id="49c78-179">Kattintson a **Hozzáadás attribútum** megnyitásához a **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="49c78-179">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lessonly-tutorial/tutorial_attribute_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lessonly-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="49c78-182">b.</span><span class="sxs-lookup"><span data-stu-id="49c78-182">b.</span></span> <span data-ttu-id="49c78-183">Az a **neve** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="49c78-183">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="49c78-184">c.</span><span class="sxs-lookup"><span data-stu-id="49c78-184">c.</span></span> <span data-ttu-id="49c78-185">Az a **érték** kilistázásához írja be a sorhoz látható attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="49c78-185">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="49c78-186">d.</span><span class="sxs-lookup"><span data-stu-id="49c78-186">d.</span></span> <span data-ttu-id="49c78-187">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="49c78-187">Click **Ok**.</span></span>     

7. <span data-ttu-id="49c78-188">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="49c78-188">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lessonly-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="49c78-190">A a **Lesson.ly konfigurációs** kattintson **konfigurálása Lesson.ly** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="49c78-190">On the **Lesson.ly Configuration** section, click **Configure Lesson.ly** to open **Configure sign-on** window.</span></span> <span data-ttu-id="49c78-191">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="49c78-191">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_configure.png)

9. <span data-ttu-id="49c78-193">Egyszeri bejelentkezés konfigurálása **Lesson.ly** oldalon kell küldeniük a letöltött **Certificate(Base64)** és **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** való [Lesson.ly támogatási csoport](mailto:dev@lessonly.com).</span><span class="sxs-lookup"><span data-stu-id="49c78-193">To configure single sign-on on **Lesson.ly** side, you need to send the downloaded **Certificate(Base64)** and **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [Lesson.ly support team](mailto:dev@lessonly.com).</span></span>

> [!TIP]
> <span data-ttu-id="49c78-194">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="49c78-194">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="49c78-195">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="49c78-195">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="49c78-196">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="49c78-196">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="49c78-197">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="49c78-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="49c78-198">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="49c78-198">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="49c78-200">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="49c78-200">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="49c78-201">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="49c78-201">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lessonly-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="49c78-203">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="49c78-203">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lessonly-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="49c78-205">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="49c78-205">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lessonly-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="49c78-207">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="49c78-207">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lessonly-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="49c78-209">a.</span><span class="sxs-lookup"><span data-stu-id="49c78-209">a.</span></span> <span data-ttu-id="49c78-210">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="49c78-210">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="49c78-211">b.</span><span class="sxs-lookup"><span data-stu-id="49c78-211">b.</span></span> <span data-ttu-id="49c78-212">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="49c78-212">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="49c78-213">c.</span><span class="sxs-lookup"><span data-stu-id="49c78-213">c.</span></span> <span data-ttu-id="49c78-214">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="49c78-214">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="49c78-215">d.</span><span class="sxs-lookup"><span data-stu-id="49c78-215">d.</span></span> <span data-ttu-id="49c78-216">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="49c78-216">Click **Create**.</span></span>
 
### <a name="creating-a-lessonly-test-user"></a><span data-ttu-id="49c78-217">Lesson.ly tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="49c78-217">Creating a Lesson.ly test user</span></span>

<span data-ttu-id="49c78-218">Ez a szakasz célja Lesson.ly Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="49c78-218">The objective of this section is to create a user called Britta Simon in Lesson.ly.</span></span> <span data-ttu-id="49c78-219">Lesson.LY támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="49c78-219">Lesson.ly supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="49c78-220">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="49c78-220">There is no action item for you in this section.</span></span> <span data-ttu-id="49c78-221">Új felhasználó jön létre az Lesson.ly elérésére, ha még nem létezik tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="49c78-221">A new user will be created during an attempt to access Lesson.ly if it doesn't exist yet.</span></span>

> [!NOTE]
> <span data-ttu-id="49c78-222">Ha hozzon létre manuálisan egy felhasználó van szüksége, forduljon a kell a [Lesson.ly támogatási csoport](mailto:dev@lessonly.com).</span><span class="sxs-lookup"><span data-stu-id="49c78-222">If you need to create an user manually, you need to contact the [Lesson.ly support team](mailto:dev@lessonly.com).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="49c78-223">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="49c78-223">Assigning the Azure AD test user</span></span>

<span data-ttu-id="49c78-224">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Lesson.ly Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="49c78-224">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Lesson.ly.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="49c78-226">**Britta Simon hozzárendelése Lesson.ly, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="49c78-226">**To assign Britta Simon to Lesson.ly, perform the following steps:**</span></span>

1. <span data-ttu-id="49c78-227">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="49c78-227">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="49c78-229">Az alkalmazások listában válassza ki a **Lesson.ly**.</span><span class="sxs-lookup"><span data-stu-id="49c78-229">In the applications list, select **Lesson.ly**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_app.png) 

3. <span data-ttu-id="49c78-231">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="49c78-231">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="49c78-233">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="49c78-233">Click **Add** button.</span></span> <span data-ttu-id="49c78-234">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="49c78-234">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="49c78-236">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="49c78-236">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="49c78-237">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="49c78-237">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="49c78-238">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="49c78-238">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="49c78-239">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="49c78-239">Testing single sign-on</span></span>

<span data-ttu-id="49c78-240">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="49c78-240">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="49c78-241">Ha a hozzáférési panelen Lesson.ly csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Lesson.ly alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="49c78-241">When you click the Lesson.ly tile in the Access Panel, you should get automatically signed-on to your Lesson.ly application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="49c78-242">További források</span><span class="sxs-lookup"><span data-stu-id="49c78-242">Additional resources</span></span>

* [<span data-ttu-id="49c78-243">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="49c78-243">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="49c78-244">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="49c78-244">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_203.png

