---
title: "Oktatóanyag: Azure Active Directory-integráció a T & E Express |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a T & E Express között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: B42374E5-2559-4309-8EF2-820BEE7EBB0C
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: jeedes
ms.openlocfilehash: 869e5284c71904fcc817ceee0f39d94fab1bc6f3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-te-express"></a><span data-ttu-id="ea4f3-103">Oktatóanyag: Azure Active Directory-integráció a T & E Express</span><span class="sxs-lookup"><span data-stu-id="ea4f3-103">Tutorial: Azure Active Directory integration with T&E Express</span></span>

<span data-ttu-id="ea4f3-104">Ebben az oktatóanyagban elsajátíthatja a & E Express integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ea4f3-104">In this tutorial, you learn how to integrate T&E Express with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ea4f3-105">T & E Express integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="ea4f3-105">Integrating T&E Express with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="ea4f3-106">Szabályozhatja, aki hozzáfér a & E Express Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="ea4f3-106">You can control in Azure AD who has access to T&E Express</span></span>
- <span data-ttu-id="ea4f3-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett történő T & E Express (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="ea4f3-107">You can enable your users to automatically get signed-on to T&E Express (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ea4f3-108">Kezelheti a fiókokat, egy központi helyen – az Azure felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="ea4f3-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="ea4f3-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ea4f3-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ea4f3-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ea4f3-110">Prerequisites</span></span>

<span data-ttu-id="ea4f3-111">Az Azure AD-integráció konfigurálása a T & E Express, az alábbi elemek szükségesek:</span><span class="sxs-lookup"><span data-stu-id="ea4f3-111">To configure Azure AD integration with T&E Express, you need the following items:</span></span>

- <span data-ttu-id="ea4f3-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="ea4f3-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ea4f3-113">A T & E Express egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="ea4f3-113">A T&E Express single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ea4f3-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ea4f3-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="ea4f3-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ea4f3-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="ea4f3-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ea4f3-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ea4f3-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="ea4f3-118">Scenario description</span></span>
<span data-ttu-id="ea4f3-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ea4f3-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="ea4f3-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ea4f3-121">T & E Express hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="ea4f3-121">Adding T&E Express from the gallery</span></span>
2. <span data-ttu-id="ea4f3-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="ea4f3-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-te-express-from-the-gallery"></a><span data-ttu-id="ea4f3-123">T & E Express hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="ea4f3-123">Adding T&E Express from the gallery</span></span>
<span data-ttu-id="ea4f3-124">Az Azure AD integrálása a T & E Express konfigurálásához kell hozzáadnia a & E Express a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-124">To configure the integration of T&E Express into Azure AD, you need to add T&E Express from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="ea4f3-125">**T & E Express hozzáadása a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ea4f3-125">**To add T&E Express from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="ea4f3-126">Az a  **[Azure felügyeleti portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ea4f3-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="ea4f3-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="ea4f3-131">Kattintson a **Hozzáadás** gombra a párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-131">Click **Add** button on the top of the dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="ea4f3-133">Írja be a keresőmezőbe, **T & E Express**.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-133">In the search box, type **T&E Express**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_search.png)

5. <span data-ttu-id="ea4f3-135">Az eredmények panelen válassza ki a **T & E Express**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-135">In the results panel, select **T&E Express**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ea4f3-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="ea4f3-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ea4f3-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a T & E Express "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-138">In this section, you configure and test Azure AD single sign-on with T&E Express based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="ea4f3-139">Az egyszeri bejelentkezés működéséhez az Azure AD számára a T & E Express a partner felhasználó egy felhasználó számára az Azure AD kell.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-139">For single sign-on to work, Azure AD needs to know what the counterpart user in T&E Express is to a user in Azure AD.</span></span> <span data-ttu-id="ea4f3-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a T & E Express közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-140">In other words, a link relationship between an Azure AD user and the related user in T&E Express needs to be established.</span></span>

<span data-ttu-id="ea4f3-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a T & E Express.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in T&E Express.</span></span>

<span data-ttu-id="ea4f3-142">Az Azure AD egyszeri bejelentkezést a T & E Express tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="ea4f3-142">To configure and test Azure AD single sign-on with T&E Express, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="ea4f3-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="ea4f3-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ea4f3-145">**[T & E Express tesztfelhasználó létrehozása](#creating-a-te-express-test-user)**  - kell rendelkeznie a megfelelője a Britta Simon a T & E Express, amely csatolva van rá, hogy az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-145">**[Creating a T&E Express test user](#creating-a-te-express-test-user)** - to have a counterpart of Britta Simon in T&E Express that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="ea4f3-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ea4f3-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ea4f3-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ea4f3-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ea4f3-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure felügyeleti portálon, és a T & E Express alkalmazást az egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your T&E Express application.</span></span>

<span data-ttu-id="ea4f3-150">**T & E Express konfigurálása az Azure AD egyszeri bejelentkezést, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ea4f3-150">**To configure Azure AD single sign-on with T&E Express, perform the following steps:**</span></span>

1. <span data-ttu-id="ea4f3-151">Az Azure felügyeleti portálján a a **T & E Express** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-151">In the Azure Management portal, on the **T&E Express** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="ea4f3-153">A a **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** a engedélyezése az egyszeri bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_samlbase.png)

3. <span data-ttu-id="ea4f3-155">Az a **& E Express tartományi és URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="ea4f3-155">On the **T&E Express Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_url.png)

    <span data-ttu-id="ea4f3-157">a.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-157">a.</span></span> <span data-ttu-id="ea4f3-158">Az a **azonosító** szövegmező, írja be az értéket, mint:`https://<domain>.tyeexpress.com`</span><span class="sxs-lookup"><span data-stu-id="ea4f3-158">In the **Identifier** textbox, type the value as: `https://<domain>.tyeexpress.com`</span></span>

    <span data-ttu-id="ea4f3-159">b.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-159">b.</span></span> <span data-ttu-id="ea4f3-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<domain>.tyeexpress.com/authorize/samlConsume.aspx`</span><span class="sxs-lookup"><span data-stu-id="ea4f3-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<domain>.tyeexpress.com/authorize/samlConsume.aspx`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ea4f3-161">Ne feledje, hogy ezek nincsenek a valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-161">Please note that these are not the real values.</span></span> <span data-ttu-id="ea4f3-162">Akkor kell frissíteni ezeket az értékeket a tényleges azonosítóját és válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-162">You have to update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="ea4f3-163">Itt javasoljuk, hogy az azonosító a karakterlánc egyedi értéket használja.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-163">Here we suggest you to use the unique value of string in the Identifier.</span></span> <span data-ttu-id="ea4f3-164">Ügyfél [T & E Express támogatási csoport](http://www.tyeexpress.com/contacto.aspx) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-164">Contact [T&E Express support team](http://www.tyeexpress.com/contacto.aspx) to get these values.</span></span>

5. <span data-ttu-id="ea4f3-165">Az a **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse az XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-165">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_certificate.png) 

6. <span data-ttu-id="ea4f3-167">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-167">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="ea4f3-169">Egyszeri bejelentkezés konfigurálása **T & E expressz** ügyféloldali, jelentkezzen be a T & ztés használata az express SAML-alapú egyszeri bejelentkezési anélkül, hogy a rendszergazda hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-169">To configure single sign-on on **T&E Express** side, login to the T&E express application without SAML single sign on using admin credentials.</span></span>

9. <span data-ttu-id="ea4f3-170">Az a **Admin** lapra, kattintson a **SAML tartomány** a SAML-beállítások lap megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-170">Under the **Admin** Tab, Click on **SAML domain** to Open the SAML settings page.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tyeexpress-tutorial/tye-SAML.png)

10. <span data-ttu-id="ea4f3-172">Válassza ki a **Activar(Activate)** parancsát **nem** való **SI(Yes)**.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-172">Select the **Activar(Activate)** option from **No** to **SI(Yes)**.</span></span> <span data-ttu-id="ea4f3-173">Az a **Identity Provider metaadatok** szövegmezőhöz illessze be a metaadatok XML, amelyekre donwloaded Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-173">In the **Identity Provider Metadata** textbox, paste the metadata XML which you have donwloaded from Azure portal.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tyeexpress-tutorial/tyeAdmin.png)

11. <span data-ttu-id="ea4f3-175">Kattintson a **Guardar(Save)** gombra a beállítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-175">Click on the **Guardar(Save)** button to save the settings.</span></span> 


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ea4f3-176">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="ea4f3-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="ea4f3-177">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure felügyeleti portálján Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-177">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="ea4f3-179">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ea4f3-179">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="ea4f3-180">Az a **Azure Management portal**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-180">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tyeexpress-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ea4f3-182">Ugrás a **felhasználók és csoportok** kattintson **minden felhasználó** azon felhasználók listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-182">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tyeexpress-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ea4f3-184">Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-184">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tyeexpress-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ea4f3-186">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="ea4f3-186">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tyeexpress-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ea4f3-188">a.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-188">a.</span></span> <span data-ttu-id="ea4f3-189">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-189">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ea4f3-190">b.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-190">b.</span></span> <span data-ttu-id="ea4f3-191">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-191">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ea4f3-192">c.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-192">c.</span></span> <span data-ttu-id="ea4f3-193">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-193">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="ea4f3-194">d.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-194">d.</span></span> <span data-ttu-id="ea4f3-195">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-195">Click **Create**.</span></span>
 
### <a name="creating-a-te-express-test-user"></a><span data-ttu-id="ea4f3-196">T & E Express tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="ea4f3-196">Creating a T&E Express test user</span></span>

<span data-ttu-id="ea4f3-197">Ahhoz, hogy az Azure AD-felhasználók jelentkezzen be a & E Express, akkor ki kell építenie történő T & E Express.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-197">In order to enable Azure AD users to log into T&E Express, they must be provisioned into T&E Express.</span></span>  
<span data-ttu-id="ea4f3-198">T & E Express, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-198">In case of T&E Express, provisioning is a manual task.</span></span>

<span data-ttu-id="ea4f3-199">**A felhasználói fiókok létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ea4f3-199">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="ea4f3-200">Jelentkezzen be rendszergazdaként a T & E Express vállalati webhely.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-200">Log in to your T&E Express company site as an administrator.</span></span>

2. <span data-ttu-id="ea4f3-201">Admin címke kattintson a felhasználók számára a felhasználók fő lap megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-201">Under Admin tag, click on Users to open the Users master page.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-tyeexpress-tutorial/tye-adminusers.png)

3. <span data-ttu-id="ea4f3-203">A kezdőlapon, kattintson a  **+**  a felhasználókat vehessen fel.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-203">On the home page, click on **+** to add the users.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-tyeexpress-tutorial/tye-usershome.png)

4. <span data-ttu-id="ea4f3-205">Itt adhatja meg a kötelező részleteit a formában, és kattintson a Mentés gombra kattintva mentse a részletek kéri.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-205">Enter all the mandatory details as asked in the form and click the save button to save the details.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-tyeexpress-tutorial/tye-usersadd.png)

    ![Alkalmazott hozzáadása](./media/active-directory-saas-tyeexpress-tutorial/tye-userssave.png)


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="ea4f3-208">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="ea4f3-208">Assigning the Azure AD test user</span></span>

<span data-ttu-id="ea4f3-209">Ebben a szakaszban Britta Simon saját hozzáférést biztosít a T & E Express által használandó Azure egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-209">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to T&E Express.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="ea4f3-211">**T & E Express Britta Simon rendeléséhez hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ea4f3-211">**To assign Britta Simon to T&E Express, perform the following steps:**</span></span>

1. <span data-ttu-id="ea4f3-212">Az Azure felügyeleti portálra, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-212">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="ea4f3-214">Az alkalmazások listában válassza ki a **T & E Express**.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-214">In the applications list, select **T&E Express**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_app.png) 

3. <span data-ttu-id="ea4f3-216">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-216">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="ea4f3-218">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-218">Click **Add** button.</span></span> <span data-ttu-id="ea4f3-219">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-219">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="ea4f3-221">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-221">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="ea4f3-222">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-222">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ea4f3-223">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-223">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ea4f3-224">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="ea4f3-224">Testing single sign-on</span></span>

<span data-ttu-id="ea4f3-225">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-225">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="ea4f3-226">Ha a hozzáférési panelen a T & E Express csempére kattint, akkor kell beolvasása automatikusan bejelentkezett a T & E Express alkalmazásra.</span><span class="sxs-lookup"><span data-stu-id="ea4f3-226">When you click the T&E Express tile in the Access Panel, you should get automatically signed-on to your T&E Express application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ea4f3-227">További források</span><span class="sxs-lookup"><span data-stu-id="ea4f3-227">Additional resources</span></span>

* [<span data-ttu-id="ea4f3-228">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="ea4f3-228">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ea4f3-229">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="ea4f3-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_203.png

