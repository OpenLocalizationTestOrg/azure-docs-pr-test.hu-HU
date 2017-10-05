---
title: "Oktatóanyag: Azure Active Directory-integráció a ScaleX vállalati |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és ScaleX vállalati között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c2379a8d-a659-45f1-87db-9ba156d83183
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2017
ms.author: jeedes
ms.openlocfilehash: 0ebed0c2605862426384c0e219e52c9d626b6246
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-scalex-enterprise"></a><span data-ttu-id="16542-103">Oktatóanyag: Azure Active Directory-integráció a ScaleX vállalati</span><span class="sxs-lookup"><span data-stu-id="16542-103">Tutorial: Azure Active Directory integration with ScaleX Enterprise</span></span>

<span data-ttu-id="16542-104">Ebben az oktatóanyagban elsajátíthatja ScaleX vállalati integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="16542-104">In this tutorial, you learn how to integrate ScaleX Enterprise with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="16542-105">ScaleX vállalati integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="16542-105">Integrating ScaleX Enterprise with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="16542-106">Megadhatja a ScaleX vállalati hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="16542-106">You can control in Azure AD who has access to ScaleX Enterprise</span></span>
- <span data-ttu-id="16542-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett ScaleX vállalati (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="16542-107">You can enable your users to automatically get signed-on to ScaleX Enterprise (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="16542-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="16542-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="16542-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg.</span><span class="sxs-lookup"><span data-stu-id="16542-109">If you want to know more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="16542-110">Alkalmazás-hozzáférés és egyszeri bejelentkezés az [Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="16542-110">What is application access and single sign-on with [Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="16542-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="16542-111">Prerequisites</span></span>

<span data-ttu-id="16542-112">Az Azure AD-integráció konfigurálása a ScaleX vállalati, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="16542-112">To configure Azure AD integration with ScaleX Enterprise, you need the following items:</span></span>

- <span data-ttu-id="16542-113">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="16542-113">An Azure AD subscription</span></span>
- <span data-ttu-id="16542-114">Egy ScaleX vállalati egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="16542-114">A ScaleX Enterprise single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="16542-115">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="16542-115">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="16542-116">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="16542-116">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="16542-117">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="16542-117">Do not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="16542-118">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="16542-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="16542-119">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="16542-119">Scenario description</span></span>
<span data-ttu-id="16542-120">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="16542-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="16542-121">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="16542-121">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="16542-122">A gyűjteményből ScaleX vállalati hozzáadása</span><span class="sxs-lookup"><span data-stu-id="16542-122">Adding ScaleX Enterprise from the gallery</span></span>
2. <span data-ttu-id="16542-123">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="16542-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-scalex-enterprise-from-the-gallery"></a><span data-ttu-id="16542-124">A gyűjteményből ScaleX vállalati hozzáadása</span><span class="sxs-lookup"><span data-stu-id="16542-124">Adding ScaleX Enterprise from the gallery</span></span>
<span data-ttu-id="16542-125">ScaleX vállalat az Azure AD-integráció konfigurálásához szüksége ScaleX vállalati hozzáadása a kezelt SaaS-alkalmazások listáját a gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="16542-125">To configure the integration of ScaleX Enterprise in to Azure AD, you need to add ScaleX Enterprise from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="16542-126">**A gyűjteményből ScaleX vállalati hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="16542-126">**To add ScaleX Enterprise from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="16542-127">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="16542-127">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="16542-129">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="16542-129">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="16542-130">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="16542-130">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="16542-132">Kattintson a **Hozzáadás** gombra a párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="16542-132">Click **Add** button on the top of the dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="16542-134">Írja be a keresőmezőbe, **ScaleX vállalati**.</span><span class="sxs-lookup"><span data-stu-id="16542-134">In the search box, type **ScaleX Enterprise**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_search.png)

5. <span data-ttu-id="16542-136">Az eredmények panelen válassza ki a **ScaleX vállalati**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="16542-136">In the results panel, select **ScaleX Enterprise**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="16542-138">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="16542-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="16542-139">Ebben a szakaszban konfigurálása és tesztelése az Azure AD egyszeri bejelentkezést a ScaleX vállalati "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="16542-139">In this section, you configure and test Azure AD single sign-on with ScaleX Enterprise based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="16542-140">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó ScaleX vállalati a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="16542-140">For single sign-on to work, Azure AD needs to know what the counterpart user in ScaleX Enterprise is to a user in Azure AD.</span></span> <span data-ttu-id="16542-141">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó ScaleX vállalat közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="16542-141">In other words, a link relationship between an Azure AD user and the related user in ScaleX Enterprise needs to be established.</span></span>

<span data-ttu-id="16542-142">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** ScaleX vállalat.</span><span class="sxs-lookup"><span data-stu-id="16542-142">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in ScaleX Enterprise.</span></span>

<span data-ttu-id="16542-143">Az Azure AD egyszeri bejelentkezést a vállalati ScaleX tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="16542-143">To configure and test Azure AD single sign-on with ScaleX Enterprise, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="16542-144">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="16542-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="16542-145">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="16542-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="16542-146">**[ScaleX vállalati tesztfelhasználó létrehozása](#creating-a-scalex-enterprise-test-user)**  - kell rendelkeznie a megfelelője a Britta Simon ScaleX vállalat, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="16542-146">**[Creating a ScaleX Enterprise test user](#creating-a-scalex-enterprise-test-user)** - to have a counterpart of Britta Simon in ScaleX Enterprise that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="16542-147">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="16542-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="16542-148">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="16542-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="16542-149">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="16542-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="16542-150">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az ScaleX vállalati alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="16542-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your ScaleX Enterprise application.</span></span>

<span data-ttu-id="16542-151">**Az Azure AD egyszeri bejelentkezést a ScaleX vállalati megadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="16542-151">**To configure Azure AD single sign-on with ScaleX Enterprise, perform the following steps:**</span></span>

1. <span data-ttu-id="16542-152">Az Azure portálon a a **ScaleX vállalati** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="16542-152">In the Azure portal, on the **ScaleX Enterprise** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="16542-154">A a **egyszeri bejelentkezés** párbeszédpanel, mint **mód** válasszon **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="16542-154">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_samlbase.png)

3. <span data-ttu-id="16542-156">Az a **ScaleX vállalati tartomány és az URL-címek** területen tegye a következőket, ha szeretne beállítani az alkalmazás **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="16542-156">On the **ScaleX Enterprise Domain and URLs** section, perform the following steps if you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_url1.png)

    <span data-ttu-id="16542-158">a.</span><span class="sxs-lookup"><span data-stu-id="16542-158">a.</span></span> <span data-ttu-id="16542-159">Az a **azonosító** szövegmező, írja be az értéket a következő minta használatával:`https://platform.rescale.com/saml2/<company id>/`</span><span class="sxs-lookup"><span data-stu-id="16542-159">In the **Identifier** textbox, type the value using the following pattern: `https://platform.rescale.com/saml2/<company id>/`</span></span>

    <span data-ttu-id="16542-160">b.</span><span class="sxs-lookup"><span data-stu-id="16542-160">b.</span></span> <span data-ttu-id="16542-161">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://platform.rescale.com/saml2/<company id>/acs/`</span><span class="sxs-lookup"><span data-stu-id="16542-161">In the **Reply URL** textbox, type a URL using the following pattern: `https://platform.rescale.com/saml2/<company id>/acs/`</span></span>

4. <span data-ttu-id="16542-162">Ellenőrizze **megjelenítése speciális URL-beállításainak**, ha szeretne beállítani az alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="16542-162">Check **Show advanced URL settings**, if you wish to configure the application in **SP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_url2.png)

    <span data-ttu-id="16542-164">Az a **bejelentkezési URL-cím** szövegmező, írja be az értéket a következő minta használatával:`https://platform.rescale.com/saml2/<company id>/sso/`</span><span class="sxs-lookup"><span data-stu-id="16542-164">In the **Sign-on URL** textbox, type the value using the following pattern: `https://platform.rescale.com/saml2/<company id>/sso/`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="16542-165">Ezek a megadandó nem valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="16542-165">These are not the real values.</span></span> <span data-ttu-id="16542-166">Ezek az értékek frissíti a tényleges azonosítója, válasz és bejelentkezési URL-címe.</span><span class="sxs-lookup"><span data-stu-id="16542-166">Update these values with the actual Identifier, Reply URL or Sign-On URL.</span></span> <span data-ttu-id="16542-167">Ügyfél [ScaleX vállalati ügyfél-támogatási csoport](http://info.rescale.com/contact_sales) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="16542-167">Contact [ScaleX Enterprise Client support team](http://info.rescale.com/contact_sales) to get these values.</span></span> 

5. <span data-ttu-id="16542-168">A ScaleX alkalmazás vár a SAML helyességi feltételek egy meghatározott formátumban, amelyhez szükség van egyéni attribútum leképezéseket a SAML-jogkivonat attribútumok konfiguráció módosítására.</span><span class="sxs-lookup"><span data-stu-id="16542-168">Your ScaleX application expects the SAML assertions in a specific format, which requires you to modify custom attribute mappings to your SAML token attributes configuration.</span></span> <span data-ttu-id="16542-169">Kattintson a **nézet és egyéb felhasználói attribútumok szerkesztése** jelölőnégyzetet, hogy nyissa meg az egyéni attribútumok beállításait.</span><span class="sxs-lookup"><span data-stu-id="16542-169">Click **View and edit all other user attributes** checkbox to open the custom attributes settings.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/scalex_attributes.png)
    
    <span data-ttu-id="16542-171">a.</span><span class="sxs-lookup"><span data-stu-id="16542-171">a.</span></span> <span data-ttu-id="16542-172">Kattintson a jobb gombbal a attribútum **neve** , és válassza a törlés.</span><span class="sxs-lookup"><span data-stu-id="16542-172">Right click the attribute **name** and click delete.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/delete_attribute_name.png)

    <span data-ttu-id="16542-174">b.</span><span class="sxs-lookup"><span data-stu-id="16542-174">b.</span></span> <span data-ttu-id="16542-175">Kattintson a **emailaddress** attribútum a attribútum szerkesztése ablak megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="16542-175">Click **emailaddress** attribute to open the Edit Attribute window.</span></span> <span data-ttu-id="16542-176">Módosítsa az értéket **user.mail** való **user.userprincipalname** kattintson az OK gombra.</span><span class="sxs-lookup"><span data-stu-id="16542-176">Change its value from **user.mail** to **user.userprincipalname** and click Ok.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/edit_email_attribute.png)   
    
5. <span data-ttu-id="16542-178">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="16542-178">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the Certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_certificate.png) 

6. <span data-ttu-id="16542-180">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="16542-180">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="16542-182">A a **ScaleX vállalati konfiguráció** kattintson **konfigurálása ScaleX vállalati** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="16542-182">On the **ScaleX Enterprise Configuration** section, click **Configure ScaleX Enterprise** to open **Configure sign-on** window.</span></span> <span data-ttu-id="16542-183">Másolás a **SAML Entitásazonosító** és **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="16542-183">Copy the **SAML Entity ID** and **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_configure.png) 

8. <span data-ttu-id="16542-185">Egyszeri bejelentkezés konfigurálása **ScaleX vállalati** ügyféloldali, jelentkezzen be rendszergazdaként a vállalati ScaleX vállalati webhely.</span><span class="sxs-lookup"><span data-stu-id="16542-185">To configure single sign-on on **ScaleX Enterprise** side, login to the ScaleX Enterprise company website as an administrator.</span></span>

9. <span data-ttu-id="16542-186">Kattintson a felső jobbra, és válassza a menü **Contoso felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="16542-186">Click the menu in the upper right and select **Contoso Administration**.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="16542-187">Contoso csak egy példa.</span><span class="sxs-lookup"><span data-stu-id="16542-187">Contoso is just an example.</span></span> <span data-ttu-id="16542-188">A tényleges vállalat neve legyen.</span><span class="sxs-lookup"><span data-stu-id="16542-188">This should be your actual Company Name.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/Test_Admin.png) 

10. <span data-ttu-id="16542-190">Válassza ki **integrációja** a felső menüben, és válassza ki a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="16542-190">Select **Integrations** from the top menu and select **Single Sign-On**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/admin_sso.png) 

11. <span data-ttu-id="16542-192">Töltse ki az űrlapot az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="16542-192">Complete the form as follows:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/scalex_admin_save.png) 
    
    <span data-ttu-id="16542-194">a.</span><span class="sxs-lookup"><span data-stu-id="16542-194">a.</span></span> <span data-ttu-id="16542-195">Válassza ki **"Bármely felhasználó, aki hitelesítheti létrehozása egyszeri bejelentkezési modellel."**</span><span class="sxs-lookup"><span data-stu-id="16542-195">Select **“Create any user who can authenticate with SSO.”**</span></span>

    <span data-ttu-id="16542-196">b.</span><span class="sxs-lookup"><span data-stu-id="16542-196">b.</span></span> <span data-ttu-id="16542-197">**Szolgáltató saml**: illessze be az érték ***urn: oasis: nevek: tc: SAML:2.0:nameid-formátum: állandó***</span><span class="sxs-lookup"><span data-stu-id="16542-197">**Service Provider saml**: Paste the value ***urn:oasis:names:tc:SAML:2.0:nameid-format:persistent***</span></span>

    <span data-ttu-id="16542-198">c.</span><span class="sxs-lookup"><span data-stu-id="16542-198">c.</span></span> <span data-ttu-id="16542-199">**Az ACS-válasz identitásszolgáltató e-mail mező neve**: illessze be az érték`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`</span><span class="sxs-lookup"><span data-stu-id="16542-199">**Name of Identity Provider email field in ACS response**: Paste the value `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`</span></span>

    <span data-ttu-id="16542-200">d.</span><span class="sxs-lookup"><span data-stu-id="16542-200">d.</span></span> <span data-ttu-id="16542-201">**Identity Provider EntityDescriptor Entitásazonosító:** illessze be a **SAML Entitásazonosító** az Azure-portálon átmásolja értéket.</span><span class="sxs-lookup"><span data-stu-id="16542-201">**Identity Provider EntityDescriptor Entity ID:** Paste the **SAML Entity ID** value copied from the Azure portal.</span></span>

    <span data-ttu-id="16542-202">e.</span><span class="sxs-lookup"><span data-stu-id="16542-202">e.</span></span> <span data-ttu-id="16542-203">**Identity Provider SingleSignOnService URL-címe:** illessze be a **SAML-alapú egyszeri bejelentkezési URL-címe** Azure-portálról.</span><span class="sxs-lookup"><span data-stu-id="16542-203">**Identity Provider SingleSignOnService URL:** Paste the **SAML Single Sign-On Service URL** from the Azure portal.</span></span>

    <span data-ttu-id="16542-204">f.</span><span class="sxs-lookup"><span data-stu-id="16542-204">f.</span></span> <span data-ttu-id="16542-205">**Szolgáltató nyilvános X509 identitástanúsítvány:** nyissa meg a X509 tanúsítvány az Azure-portálról letöltve a Jegyzettömbben beillesztése ebben a mezőben.</span><span class="sxs-lookup"><span data-stu-id="16542-205">**Identity Provider public X509 certificate:** Open the X509 certificate downloaded from the Azure in notepad and paste the contents in this box.</span></span> <span data-ttu-id="16542-206">Ellenőrizze, hogy nincsenek a nincs sortörések a tanúsítványok tartalmának közepén.</span><span class="sxs-lookup"><span data-stu-id="16542-206">Ensure there are no line breaks in the middle of the certificate contents.</span></span>
    
    <span data-ttu-id="16542-207">g.</span><span class="sxs-lookup"><span data-stu-id="16542-207">g.</span></span> <span data-ttu-id="16542-208">Ellenőrizze az alábbi jelölőnégyzeteket: **engedélyezve, a NameID titkosítására és a bejelentkezési AuthnRequests.**</span><span class="sxs-lookup"><span data-stu-id="16542-208">Check the following checkboxes: **Enabled, Encrypt NameID and Sign AuthnRequests.**</span></span>

    <span data-ttu-id="16542-209">h.</span><span class="sxs-lookup"><span data-stu-id="16542-209">h.</span></span> <span data-ttu-id="16542-210">Kattintson a **egyszeri bejelentkezési beállítások** menti a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="16542-210">Click **Update SSO Settings** to save the settings.</span></span>

> [!TIP]
> <span data-ttu-id="16542-211">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="16542-211">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="16542-212">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="16542-212">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="16542-213">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="16542-213">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="16542-214">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="16542-214">Creating an Azure AD test user</span></span>
<span data-ttu-id="16542-215">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="16542-215">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="16542-217">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="16542-217">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="16542-218">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="16542-218">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="16542-220">Ugrás a **felhasználók és csoportok** kattintson **minden felhasználó** azon felhasználók listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="16542-220">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="16542-222">Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="16542-222">At the top of the dialog, click **Add** to open the **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="16542-224">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="16542-224">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="16542-226">a.</span><span class="sxs-lookup"><span data-stu-id="16542-226">a.</span></span> <span data-ttu-id="16542-227">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="16542-227">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="16542-228">b.</span><span class="sxs-lookup"><span data-stu-id="16542-228">b.</span></span> <span data-ttu-id="16542-229">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="16542-229">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="16542-230">c.</span><span class="sxs-lookup"><span data-stu-id="16542-230">c.</span></span> <span data-ttu-id="16542-231">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="16542-231">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="16542-232">d.</span><span class="sxs-lookup"><span data-stu-id="16542-232">d.</span></span> <span data-ttu-id="16542-233">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="16542-233">Click **Create**.</span></span>
 
### <a name="creating-a-scalex-enterprise-test-user"></a><span data-ttu-id="16542-234">ScaleX vállalati tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="16542-234">Creating a ScaleX Enterprise test user</span></span>

<span data-ttu-id="16542-235">Ahhoz, hogy az Azure AD-felhasználók ScaleX vállalati bejelentkezni, akkor ki kell építenie a ScaleX vállalati.</span><span class="sxs-lookup"><span data-stu-id="16542-235">To enable Azure AD users to log in to ScaleX Enterprise, they must be provisioned in to ScaleX Enterprise.</span></span> <span data-ttu-id="16542-236">ScaleX vállalati automatikus feladatról, és nincs manuális lépések szükségesek.</span><span class="sxs-lookup"><span data-stu-id="16542-236">In the case of ScaleX Enterprise, provisioning is an automatic task and no manual steps are required.</span></span> <span data-ttu-id="16542-237">Bárki, aki képes sikeresen hitelesíteni egyszeri bejelentkezési hitelesítő adatokkal rendelkező automatikusan megkapják a ScaleX oldalon.</span><span class="sxs-lookup"><span data-stu-id="16542-237">Any user who can successfully authenticate with SSO credentials will be automatically provisioned on the ScaleX side.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="16542-238">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="16542-238">Assigning the Azure AD test user</span></span>

<span data-ttu-id="16542-239">Ebben a szakaszban engedélyezze Britta Simon szerint ScaleX vállalati való hozzáférés engedélyezése az Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="16542-239">In this section, you enable Britta Simon to use Azure single sign-on by granting user access to ScaleX Enterprise.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="16542-241">**Britta Simon hozzárendelése ScaleX vállalati, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="16542-241">**To assign Britta Simon to ScaleX Enterprise, perform the following steps:**</span></span>

1. <span data-ttu-id="16542-242">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="16542-242">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="16542-244">Az alkalmazások listában válassza ki a **ScaleX vállalati**.</span><span class="sxs-lookup"><span data-stu-id="16542-244">In the applications list, select **ScaleX Enterprise**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_app.png) 

3. <span data-ttu-id="16542-246">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="16542-246">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="16542-248">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="16542-248">Click **Add** button.</span></span> <span data-ttu-id="16542-249">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="16542-249">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="16542-251">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="16542-251">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="16542-252">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="16542-252">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="16542-253">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="16542-253">Click **Assign** button on **Add Assignment** dialog.</span></span>

### <a name="testing-single-sign-on"></a><span data-ttu-id="16542-254">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="16542-254">Testing single sign-on</span></span>

<span data-ttu-id="16542-255">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="16542-255">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="16542-256">Kattintson a hozzáférési panelen ScaleX vállalati csempére, akkor fogja lekérni automatikusan bejelentkezett az ScaleX vállalati alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="16542-256">Click the ScaleX Enterprise tile in the Access Panel, you will get automatically signed-on to your ScaleX Enterprise application.</span></span> <span data-ttu-id="16542-257">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="16542-257">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="16542-258">További források</span><span class="sxs-lookup"><span data-stu-id="16542-258">Additional resources</span></span>

* [<span data-ttu-id="16542-259">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="16542-259">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="16542-260">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="16542-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_203.png

