---
title: "Oktatóanyag: Azure Active Directoryval integrált Cisco Spark |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a Cisco Spark között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c47894b1-f5df-4755-845d-f12f4c602dc4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: a0a221622afe1c801a331e2319f3a7ace3111dad
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-spark"></a><span data-ttu-id="8341c-103">Oktatóanyag: Azure Active Directoryval integrált Cisco Spark</span><span class="sxs-lookup"><span data-stu-id="8341c-103">Tutorial: Azure Active Directory integration with Cisco Spark</span></span>

<span data-ttu-id="8341c-104">Ebben az oktatóanyagban elsajátíthatja Cisco Spark integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8341c-104">In this tutorial, you learn how to integrate Cisco Spark with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8341c-105">Cisco Spark integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="8341c-105">Integrating Cisco Spark with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="8341c-106">Megadhatja a Cisco Spark hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="8341c-106">You can control in Azure AD who has access to Cisco Spark</span></span>
- <span data-ttu-id="8341c-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a Cisco Spark (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="8341c-107">You can enable your users to automatically get signed-on to Cisco Spark (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8341c-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="8341c-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="8341c-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8341c-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8341c-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8341c-110">Prerequisites</span></span>

<span data-ttu-id="8341c-111">Cisco Spark az Azure AD-integrációs konfigurálásához a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="8341c-111">To configure Azure AD integration with Cisco Spark, you need the following items:</span></span>

- <span data-ttu-id="8341c-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="8341c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8341c-113">A Cisco Spark egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="8341c-113">A Cisco Spark single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8341c-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="8341c-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8341c-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="8341c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8341c-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="8341c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8341c-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8341c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8341c-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="8341c-118">Scenario description</span></span>
<span data-ttu-id="8341c-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="8341c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8341c-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="8341c-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8341c-121">Cisco Spark hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="8341c-121">Adding Cisco Spark from the gallery</span></span>
2. <span data-ttu-id="8341c-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="8341c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cisco-spark-from-the-gallery"></a><span data-ttu-id="8341c-123">Cisco Spark hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="8341c-123">Adding Cisco Spark from the gallery</span></span>
<span data-ttu-id="8341c-124">Az Azure AD integrálása a Cisco Spark konfigurálásához kell hozzáadnia Cisco Spark a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="8341c-124">To configure the integration of Cisco Spark into Azure AD, you need to add Cisco Spark from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="8341c-125">**Cisco Spark hozzáadása a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="8341c-125">**To add Cisco Spark from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="8341c-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8341c-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8341c-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="8341c-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="8341c-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8341c-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="8341c-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="8341c-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="8341c-133">Írja be a keresőmezőbe, **Cisco Spark**.</span><span class="sxs-lookup"><span data-stu-id="8341c-133">In the search box, type **Cisco Spark**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_search.png)

5. <span data-ttu-id="8341c-135">Az eredmények panelen válassza ki a **Cisco Spark**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="8341c-135">In the results panel, select **Cisco Spark**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8341c-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="8341c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8341c-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Cisco Spark "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="8341c-138">In this section, you configure and test Azure AD single sign-on with Cisco Spark based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="8341c-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Cisco Spark a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="8341c-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Cisco Spark is to a user in Azure AD.</span></span> <span data-ttu-id="8341c-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a Cisco Spark közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="8341c-140">In other words, a link relationship between an Azure AD user and the related user in Cisco Spark needs to be established.</span></span>

<span data-ttu-id="8341c-141">A Cisco Spark, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="8341c-141">In Cisco Spark, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="8341c-142">Az Azure AD egyszeri bejelentkezést a Cisco Spark tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="8341c-142">To configure and test Azure AD single sign-on with Cisco Spark, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="8341c-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="8341c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="8341c-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="8341c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8341c-145">**[Cisco Spark tesztfelhasználó létrehozása](#creating-a-cisco-spark-test-user)**  - való Britta Simon egy megfelelője a Cisco Spark, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="8341c-145">**[Creating a Cisco Spark test user](#creating-a-cisco-spark-test-user)** - to have a counterpart of Britta Simon in Cisco Spark that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="8341c-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="8341c-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8341c-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="8341c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8341c-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8341c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8341c-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a Cisco Spark alkalmazásban egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="8341c-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Cisco Spark application.</span></span>

<span data-ttu-id="8341c-150">**Cisco Spark az Azure AD az egyszeri bejelentkezés konfigurálása, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="8341c-150">**To configure Azure AD single sign-on with Cisco Spark, perform the following steps:**</span></span>

1. <span data-ttu-id="8341c-151">Az Azure portálon a a **Cisco Spark** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="8341c-151">In the Azure portal, on the **Cisco Spark** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="8341c-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="8341c-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_samlbase.png)

3. <span data-ttu-id="8341c-155">Az a **Cisco Spark tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="8341c-155">On the **Cisco Spark Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_url.png)

    <span data-ttu-id="8341c-157">a.</span><span class="sxs-lookup"><span data-stu-id="8341c-157">a.</span></span> <span data-ttu-id="8341c-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg az URL-címet:`https://web.ciscospark.com/#/signin`</span><span class="sxs-lookup"><span data-stu-id="8341c-158">In the **Sign-on URL** textbox, type a URL as: `https://web.ciscospark.com/#/signin`</span></span>

    <span data-ttu-id="8341c-159">b.</span><span class="sxs-lookup"><span data-stu-id="8341c-159">b.</span></span> <span data-ttu-id="8341c-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://idbroker.webex.com/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="8341c-160">In the **Identifier** textbox, type a URL using the following pattern: `https://idbroker.webex.com/<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="8341c-161">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="8341c-161">This value is not real.</span></span> <span data-ttu-id="8341c-162">Frissítse ezt az értéket a tényleges azonosítója.</span><span class="sxs-lookup"><span data-stu-id="8341c-162">Update this value with the actual Identifier.</span></span> <span data-ttu-id="8341c-163">Ügyfél [Cisco Spark ügyfél-támogatási csoport](https://support.ciscospark.com/) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="8341c-163">Contact [Cisco Spark Client support team](https://support.ciscospark.com/) to get this value.</span></span> 
 
4. <span data-ttu-id="8341c-164">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="8341c-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_certificate.png) 

5. <span data-ttu-id="8341c-166">Cisco Spark alkalmazás vár a SAML helyességi feltételek megadott attribútumokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="8341c-166">Cisco Spark application expects the SAML assertions to contain specific attributes.</span></span> <span data-ttu-id="8341c-167">Konfigurálja a következő attribútumok ehhez az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="8341c-167">Configure the following attributes  for this application.</span></span> <span data-ttu-id="8341c-168">Ezek az attribútumok értékének kezelheti a **felhasználói attribútumok** szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="8341c-168">You can manage the values of these attributes from the **User Attributes** section on application integration page.</span></span> <span data-ttu-id="8341c-169">Az alábbi képernyőfelvételen látható egy példa a.</span><span class="sxs-lookup"><span data-stu-id="8341c-169">The following screenshot shows an example for this.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_07.png) 

6. <span data-ttu-id="8341c-171">A a **felhasználói attribútumok** a szakasz a **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum, a fenti ábrán látható módon, és hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="8341c-171">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image above and perform the following steps:</span></span>
    
    | <span data-ttu-id="8341c-172">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="8341c-172">Attribute Name</span></span>  | <span data-ttu-id="8341c-173">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="8341c-173">Attribute Value</span></span> |
    | --------------- | -------------------- |    
    |   <span data-ttu-id="8341c-174">egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="8341c-174">uid</span></span>    | <span data-ttu-id="8341c-175">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="8341c-175">user.userprincipalname</span></span> |   

    <span data-ttu-id="8341c-176">a.</span><span class="sxs-lookup"><span data-stu-id="8341c-176">a.</span></span> <span data-ttu-id="8341c-177">Kattintson a **Hozzáadás attribútum** megnyitásához a **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8341c-177">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_attribute_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="8341c-180">b.</span><span class="sxs-lookup"><span data-stu-id="8341c-180">b.</span></span> <span data-ttu-id="8341c-181">Az a **neve** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="8341c-181">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="8341c-182">c.</span><span class="sxs-lookup"><span data-stu-id="8341c-182">c.</span></span> <span data-ttu-id="8341c-183">Az a **érték** kilistázásához írja be a sorhoz látható attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="8341c-183">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="8341c-184">d.</span><span class="sxs-lookup"><span data-stu-id="8341c-184">d.</span></span> <span data-ttu-id="8341c-185">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="8341c-185">Click **Ok**.</span></span>

7. <span data-ttu-id="8341c-186">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="8341c-186">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="8341c-188">Jelentkezzen be [Cisco Cloud Collaboration felügyeleti](https://admin.ciscospark.com/) a teljes körű rendszergazdai hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="8341c-188">Sign in to [Cisco Cloud Collaboration Management](https://admin.ciscospark.com/) with your full administrator credentials.</span></span>

9. <span data-ttu-id="8341c-189">Válassza ki **beállítások** alatt pedig a **hitelesítési** területén kattintson **módosítás**.</span><span class="sxs-lookup"><span data-stu-id="8341c-189">Select **Settings** and under the **Authentication** section, click **Modify**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_10.png)
    
10. <span data-ttu-id="8341c-191">Válassza ki **a 3. fél identitásszolgáltató integrálását. (Speciális)**  , és navigáljon a következő képernyő.</span><span class="sxs-lookup"><span data-stu-id="8341c-191">Select **Integrate a 3rd-party identity provider. (Advanced)** and go to the next screen.</span></span>

11. <span data-ttu-id="8341c-192">Az a **Idp metaadatok importálása** lapon, vagy húzza és dobja el az Azure AD-metaadatfájl a lapra, vagy a fájl böngésző kapcsoló használatával keresse meg és töltse fel az Azure AD-metaadatait tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="8341c-192">On the **Import Idp Metadata** page, either drag and drop the Azure AD metadata file onto the page or use the file browser option to locate and upload the Azure AD metadata file.</span></span> <span data-ttu-id="8341c-193">Ezt követően válassza **szükséges metaadatokat (biztonságosabb) a hitelesítésszolgáltató által aláírt tanúsítvány** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="8341c-193">Then, select **Require certificate signed by a certificate authority in Metadata (more secure)** and click **Next**.</span></span> 
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_11.png)

12. <span data-ttu-id="8341c-195">Válassza ki **SSO-kapcsolat tesztelése**, és egy új böngészőlapon nyílik meg, amikor hitelesítéséhez az Azure ad-vel történő bejelentkezéssel.</span><span class="sxs-lookup"><span data-stu-id="8341c-195">Select **Test SSO Connection**, and when a new browser tab opens, authenticate with Azure AD by signing in.</span></span>

13. <span data-ttu-id="8341c-196">Lépjen vissza a **Cisco Cloud Collaboration felügyeleti** böngészőlapon.</span><span class="sxs-lookup"><span data-stu-id="8341c-196">Return to the **Cisco Cloud Collaboration Management** browser tab.</span></span> <span data-ttu-id="8341c-197">Ha a teszt sikeres volt, válassza ki a **Ez a teszt sikeres volt. Engedélyezi az egyszeri bejelentkezés beállítást** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="8341c-197">If the test was successful, select **This test was successful. Enable Single Sign-On option** and click **Next**.</span></span>

> [!TIP]
> <span data-ttu-id="8341c-198">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="8341c-198">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="8341c-199">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="8341c-199">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="8341c-200">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8341c-200">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8341c-201">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8341c-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="8341c-202">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="8341c-202">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="8341c-204">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="8341c-204">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="8341c-205">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8341c-205">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8341c-207">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="8341c-207">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8341c-209">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="8341c-209">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8341c-211">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="8341c-211">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8341c-213">a.</span><span class="sxs-lookup"><span data-stu-id="8341c-213">a.</span></span> <span data-ttu-id="8341c-214">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8341c-214">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8341c-215">b.</span><span class="sxs-lookup"><span data-stu-id="8341c-215">b.</span></span> <span data-ttu-id="8341c-216">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8341c-216">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8341c-217">c.</span><span class="sxs-lookup"><span data-stu-id="8341c-217">c.</span></span> <span data-ttu-id="8341c-218">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="8341c-218">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="8341c-219">d.</span><span class="sxs-lookup"><span data-stu-id="8341c-219">d.</span></span> <span data-ttu-id="8341c-220">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="8341c-220">Click **Create**.</span></span>
 
### <a name="creating-a-cisco-spark-test-user"></a><span data-ttu-id="8341c-221">Cisco Spark tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8341c-221">Creating a Cisco Spark test user</span></span>

<span data-ttu-id="8341c-222">Ebben a szakaszban egy Cisco Spark Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="8341c-222">In this section, you create a user called Britta Simon in Cisco Spark.</span></span> <span data-ttu-id="8341c-223">Ebben a szakaszban egy Cisco Spark Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="8341c-223">In this section, you create a user called Britta Simon in Cisco Spark.</span></span>

1. <span data-ttu-id="8341c-224">Lépjen a [Cisco Cloud Collaboration felügyeleti](https://admin.ciscospark.com/) a teljes körű rendszergazdai hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="8341c-224">Go to the [Cisco Cloud Collaboration Management](https://admin.ciscospark.com/) with your full administrator credentials.</span></span>

2. <span data-ttu-id="8341c-225">Kattintson a **felhasználók** , majd **felhasználók kezelése**.</span><span class="sxs-lookup"><span data-stu-id="8341c-225">Click **Users** and then **Manage Users**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_12.png) 

3. <span data-ttu-id="8341c-227">Az a **kezelése felhasználó** ablakban válassza ki **manuális hozzáadása vagy módosítása a felhasználók** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="8341c-227">In the **Manage User** window, select **Manually add or modify users** and click **Next**.</span></span>

4. <span data-ttu-id="8341c-228">Válassza ki **nevek és E-mail cím**.</span><span class="sxs-lookup"><span data-stu-id="8341c-228">Select **Names and Email address**.</span></span> <span data-ttu-id="8341c-229">Ezután adja meg a szövegmező az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="8341c-229">Then, fill out the textbox as follows:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_13.png) 
    
    <span data-ttu-id="8341c-231">a.</span><span class="sxs-lookup"><span data-stu-id="8341c-231">a.</span></span> <span data-ttu-id="8341c-232">Az a **Utónév** szövegmezőhöz típus **Britta**.</span><span class="sxs-lookup"><span data-stu-id="8341c-232">In the **First Name** textbox, type **Britta**.</span></span> 
    
    <span data-ttu-id="8341c-233">b.</span><span class="sxs-lookup"><span data-stu-id="8341c-233">b.</span></span> <span data-ttu-id="8341c-234">Az a **Vezetéknév** szövegmezőhöz típus **Simon**.</span><span class="sxs-lookup"><span data-stu-id="8341c-234">In the **Last Name** textbox, type **Simon**.</span></span>
    
    <span data-ttu-id="8341c-235">c.</span><span class="sxs-lookup"><span data-stu-id="8341c-235">c.</span></span> <span data-ttu-id="8341c-236">Az a **E-mail cím** szövegmezőhöz típus  **britta.simon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="8341c-236">In the **Email address** textbox, type **britta.simon@contoso.com**.</span></span>

5. <span data-ttu-id="8341c-237">Kattintson a plusz jelre Britta Simon hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="8341c-237">Click the plus sign to add Britta Simon.</span></span> <span data-ttu-id="8341c-238">Ezután kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="8341c-238">Then, click **Next**.</span></span>

6. <span data-ttu-id="8341c-239">Az a **szolgáltatások hozzáadása a felhasználók** ablak, kattintson a **mentése** , majd **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="8341c-239">In the **Add Services for Users** window, click **Save** and then **Finish**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="8341c-240">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="8341c-240">Assigning the Azure AD test user</span></span>

<span data-ttu-id="8341c-241">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Cisco Spark az Azure egyszeri bejelentkezés használatára.</span><span class="sxs-lookup"><span data-stu-id="8341c-241">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Cisco Spark.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="8341c-243">**Cisco Spark Britta Simon rendel, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="8341c-243">**To assign Britta Simon to Cisco Spark, perform the following steps:**</span></span>

1. <span data-ttu-id="8341c-244">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8341c-244">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="8341c-246">Az alkalmazások listában válassza ki a **Cisco Spark**.</span><span class="sxs-lookup"><span data-stu-id="8341c-246">In the applications list, select **Cisco Spark**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_app.png) 

3. <span data-ttu-id="8341c-248">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="8341c-248">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="8341c-250">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="8341c-250">Click **Add** button.</span></span> <span data-ttu-id="8341c-251">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8341c-251">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="8341c-253">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="8341c-253">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="8341c-254">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8341c-254">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8341c-255">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8341c-255">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8341c-256">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="8341c-256">Testing single sign-on</span></span>

<span data-ttu-id="8341c-257">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="8341c-257">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="8341c-258">Ha a hozzáférési panelen a Cisco Spark csempére kattint, akkor kell beolvasása automatikusan bejelentkezett a Cisco Spark alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="8341c-258">When you click the Cisco Spark tile in the Access Panel, you should get automatically signed-on to your Cisco Spark application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8341c-259">További források</span><span class="sxs-lookup"><span data-stu-id="8341c-259">Additional resources</span></span>

* [<span data-ttu-id="8341c-260">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="8341c-260">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8341c-261">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="8341c-261">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_04.png
[10]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_060.png
[100]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_203.png

