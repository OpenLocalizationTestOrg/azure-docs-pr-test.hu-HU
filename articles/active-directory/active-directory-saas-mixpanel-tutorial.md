---
title: "Oktatóanyag: Azure Active Directoryval integrált Mixpanel |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Mixpanel között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a2df26ef-d441-44ac-a9f3-b37bf9709bcb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 3dd11b3477de1329c1c8e45a6dbf212b1635fd95
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mixpanel"></a><span data-ttu-id="5cc89-103">Oktatóanyag: Azure Active Directoryval integrált Mixpanel</span><span class="sxs-lookup"><span data-stu-id="5cc89-103">Tutorial: Azure Active Directory integration with Mixpanel</span></span>

<span data-ttu-id="5cc89-104">Ebben az oktatóanyagban elsajátíthatja Mixpanel integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5cc89-104">In this tutorial, you learn how to integrate Mixpanel with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5cc89-105">Mixpanel integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="5cc89-105">Integrating Mixpanel with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="5cc89-106">Megadhatja a Mixpanel hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="5cc89-106">You can control in Azure AD who has access to Mixpanel</span></span>
- <span data-ttu-id="5cc89-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Mixpanel (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="5cc89-107">You can enable your users to automatically get signed-on to Mixpanel (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5cc89-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="5cc89-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="5cc89-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5cc89-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5cc89-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5cc89-110">Prerequisites</span></span>

<span data-ttu-id="5cc89-111">Konfigurálása az Azure AD-integrációs Mixpanel, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="5cc89-111">To configure Azure AD integration with Mixpanel, you need the following items:</span></span>

- <span data-ttu-id="5cc89-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="5cc89-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5cc89-113">Egy Mixpanel egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="5cc89-113">A Mixpanel single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5cc89-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="5cc89-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5cc89-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="5cc89-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5cc89-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="5cc89-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5cc89-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5cc89-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5cc89-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="5cc89-118">Scenario description</span></span>
<span data-ttu-id="5cc89-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="5cc89-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5cc89-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="5cc89-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5cc89-121">A gyűjteményből Mixpanel hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5cc89-121">Adding Mixpanel from the gallery</span></span>
2. <span data-ttu-id="5cc89-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="5cc89-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mixpanel-from-the-gallery"></a><span data-ttu-id="5cc89-123">A gyűjteményből Mixpanel hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5cc89-123">Adding Mixpanel from the gallery</span></span>
<span data-ttu-id="5cc89-124">Az Azure AD integrálása a Mixpanel konfigurálásához kell hozzáadnia Mixpanel a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="5cc89-124">To configure the integration of Mixpanel into Azure AD, you need to add Mixpanel from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="5cc89-125">**A gyűjteményből Mixpanel hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="5cc89-125">**To add Mixpanel from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="5cc89-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="5cc89-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5cc89-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="5cc89-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="5cc89-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="5cc89-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="5cc89-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="5cc89-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="5cc89-133">Írja be a keresőmezőbe, **Mixpanel**.</span><span class="sxs-lookup"><span data-stu-id="5cc89-133">In the search box, type **Mixpanel**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_search.png)

5. <span data-ttu-id="5cc89-135">Az eredmények panelen válassza ki a **Mixpanel**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5cc89-135">In the results panel, select **Mixpanel**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5cc89-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="5cc89-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5cc89-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Mixpanel.</span><span class="sxs-lookup"><span data-stu-id="5cc89-138">In this section, you configure and test Azure AD single sign-on with Mixpanel based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="5cc89-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Mixpanel a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="5cc89-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Mixpanel is to a user in Azure AD.</span></span> <span data-ttu-id="5cc89-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Mixpanel közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="5cc89-140">In other words, a link relationship between an Azure AD user and the related user in Mixpanel needs to be established.</span></span>

<span data-ttu-id="5cc89-141">Mixpanel, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="5cc89-141">In Mixpanel, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="5cc89-142">Az Azure AD egyszeri bejelentkezést a Mixpanel tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="5cc89-142">To configure and test Azure AD single sign-on with Mixpanel, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="5cc89-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="5cc89-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="5cc89-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="5cc89-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5cc89-145">**[Mixpanel tesztfelhasználó létrehozása](#creating-a-mixpanel-test-user)**  - való Britta Simon valami Mixpanel, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="5cc89-145">**[Creating a Mixpanel test user](#creating-a-mixpanel-test-user)** - to have a counterpart of Britta Simon in Mixpanel that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="5cc89-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="5cc89-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5cc89-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="5cc89-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5cc89-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5cc89-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5cc89-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Mixpanel alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="5cc89-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Mixpanel application.</span></span>

<span data-ttu-id="5cc89-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Mixpanel, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="5cc89-150">**To configure Azure AD single sign-on with Mixpanel, perform the following steps:**</span></span>

1. <span data-ttu-id="5cc89-151">Az Azure portálon a a **Mixpanel** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="5cc89-151">In the Azure portal, on the **Mixpanel** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="5cc89-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="5cc89-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_samlbase.png)

3. <span data-ttu-id="5cc89-155">Az a **Mixpanel tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="5cc89-155">On the **Mixpanel Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_url.png)

     <span data-ttu-id="5cc89-157">Az a **bejelentkezési URL-cím** szövegmező, adja meg az URL-címet:`https://mixpanel.com/login/`</span><span class="sxs-lookup"><span data-stu-id="5cc89-157">In the **Sign-on URL** textbox, type a URL as: `https://mixpanel.com/login/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5cc89-158">Regisztrálja a [https://mixpanel.com/register/](https://mixpanel.com/register/) állíthat be a bejelentkezési hitelesítő adatokat, és lépjen kapcsolatba a [Mixpanel támogatási csoport](mailto:support@mixpanel.com) ahhoz, hogy a bérlő SSO beállításait.</span><span class="sxs-lookup"><span data-stu-id="5cc89-158">Please register at [https://mixpanel.com/register/](https://mixpanel.com/register/) to set up your login credentials and  contact the [Mixpanel support team](mailto:support@mixpanel.com) to enable SSO settings for your tenant.</span></span> <span data-ttu-id="5cc89-159">Is letölthető az URL-cím bejelentkezési érték szükség esetén a Mixpanel támogatási csoportjához.</span><span class="sxs-lookup"><span data-stu-id="5cc89-159">You can also get your Sign On URL value if necessary from your Mixpanel support team.</span></span> 
 
4. <span data-ttu-id="5cc89-160">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="5cc89-160">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_certificate.png) 

5. <span data-ttu-id="5cc89-162">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="5cc89-162">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mixpanel-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5cc89-164">A a **Mixpanel konfigurációs** kattintson **konfigurálása Mixpanel** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="5cc89-164">On the **Mixpanel Configuration** section, click **Configure Mixpanel** to open **Configure sign-on** window.</span></span> <span data-ttu-id="5cc89-165">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="5cc89-165">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_configure.png) 

7. <span data-ttu-id="5cc89-167">Egy másik böngészőablakban bejelentkezés Mixpanel Alkalmazásmódosítások rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="5cc89-167">In a different browser window, sign-on to your Mixpanel application as an administrator.</span></span>

8. <span data-ttu-id="5cc89-168">A lap alján, kattintson a kevés **áttételi** ikonra a bal oldali sarokban.</span><span class="sxs-lookup"><span data-stu-id="5cc89-168">On bottom of the page, click the little **gear** icon in the left corner.</span></span> 
   
    ![Mixpanel egyszeri bejelentkezés](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_06.png) 

9. <span data-ttu-id="5cc89-170">Kattintson a **biztonságos hozzáférését** fülre, majd **beállításainak módosítása**.</span><span class="sxs-lookup"><span data-stu-id="5cc89-170">Click the **Access security** tab, and then click **Change settings**.</span></span>
   
    ![Mixpanel beállítások](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_08.png) 

10. <span data-ttu-id="5cc89-172">A a **módosítania kell a tanúsítványt** párbeszédpanel lap, kattintson a **fájl kiválasztása** a letöltött tanúsítvány feltöltése, majd **tovább**.</span><span class="sxs-lookup"><span data-stu-id="5cc89-172">On the **Change your certificate** dialog page, click **Choose file** to upload your downloaded certificate, and then click **NEXT**.</span></span>
   
    ![Mixpanel beállítások](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_09.png) 

11.  <span data-ttu-id="5cc89-174">A hitelesítési URL-cím beviteli mezőben szereplő a **módosítsa a hitelesítési URL-címet** párbeszédpanel lapon, és beillesztheti értékének **SAML-alapú egyszeri bejelentkezési URL-címe** másolta az Azure-portálról, amely majd **Következő**.</span><span class="sxs-lookup"><span data-stu-id="5cc89-174">In the authentication URL textbox on the **Change your authentication  URL** dialog page, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal, and then click **NEXT**.</span></span>
   
   ![Mixpanel beállítások](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_10.png) 

12. <span data-ttu-id="5cc89-176">Kattintson a **Done** (Kész) gombra.</span><span class="sxs-lookup"><span data-stu-id="5cc89-176">Click **Done**.</span></span>

> [!TIP]
> <span data-ttu-id="5cc89-177">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="5cc89-177">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="5cc89-178">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="5cc89-178">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="5cc89-179">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5cc89-179">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5cc89-180">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="5cc89-180">Creating an Azure AD test user</span></span>
<span data-ttu-id="5cc89-181">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="5cc89-181">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="5cc89-183">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="5cc89-183">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="5cc89-184">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="5cc89-184">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5cc89-186">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="5cc89-186">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5cc89-188">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="5cc89-188">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5cc89-190">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="5cc89-190">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5cc89-192">a.</span><span class="sxs-lookup"><span data-stu-id="5cc89-192">a.</span></span> <span data-ttu-id="5cc89-193">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5cc89-193">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5cc89-194">b.</span><span class="sxs-lookup"><span data-stu-id="5cc89-194">b.</span></span> <span data-ttu-id="5cc89-195">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5cc89-195">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5cc89-196">c.</span><span class="sxs-lookup"><span data-stu-id="5cc89-196">c.</span></span> <span data-ttu-id="5cc89-197">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="5cc89-197">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="5cc89-198">d.</span><span class="sxs-lookup"><span data-stu-id="5cc89-198">d.</span></span> <span data-ttu-id="5cc89-199">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="5cc89-199">Click **Create**.</span></span>
 
### <a name="creating-a-mixpanel-test-user"></a><span data-ttu-id="5cc89-200">Mixpanel tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="5cc89-200">Creating a Mixpanel test user</span></span>

<span data-ttu-id="5cc89-201">Ez a szakasz célja Mixpanel Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="5cc89-201">The objective of this section is to create a user called Britta Simon in Mixpanel.</span></span> 

1. <span data-ttu-id="5cc89-202">Jelentkezzen be rendszergazdaként a Mixpanel vállalati webhely.</span><span class="sxs-lookup"><span data-stu-id="5cc89-202">Sign on to your Mixpanel company site as an administrator.</span></span>

2. <span data-ttu-id="5cc89-203">Kattintson a lap alján a gombra kevés fogaskerék nyissa meg a bal oldali sarokban a **beállítások** ablak.</span><span class="sxs-lookup"><span data-stu-id="5cc89-203">On the bottom of the page, click the little gear button on the left corner to open the **Settings** window.</span></span>

3. <span data-ttu-id="5cc89-204">Kattintson a **Team** fülre.</span><span class="sxs-lookup"><span data-stu-id="5cc89-204">Click the **Team** tab.</span></span>

4. <span data-ttu-id="5cc89-205">Az a **csapattag** szövegmező, írja be a Britta meg e-mail címét az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="5cc89-205">In the **team member** textbox, type Britta's email address in the Azure.</span></span>
   
    ![Mixpanel beállítások](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_11.png) 

5. <span data-ttu-id="5cc89-207">Kattintson a **meghívása**.</span><span class="sxs-lookup"><span data-stu-id="5cc89-207">Click **Invite**.</span></span> 

> [!Note]
> <span data-ttu-id="5cc89-208">A felhasználó kap egy e-mailt a profil beállításához.</span><span class="sxs-lookup"><span data-stu-id="5cc89-208">The user will get an email to set up the profile.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="5cc89-209">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="5cc89-209">Assigning the Azure AD test user</span></span>

<span data-ttu-id="5cc89-210">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Mixpanel Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="5cc89-210">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Mixpanel.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="5cc89-212">**Britta Simon hozzárendelése Mixpanel, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="5cc89-212">**To assign Britta Simon to Mixpanel, perform the following steps:**</span></span>

1. <span data-ttu-id="5cc89-213">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="5cc89-213">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="5cc89-215">Az alkalmazások listában válassza ki a **Mixpanel**.</span><span class="sxs-lookup"><span data-stu-id="5cc89-215">In the applications list, select **Mixpanel**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_app.png) 

3. <span data-ttu-id="5cc89-217">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="5cc89-217">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="5cc89-219">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="5cc89-219">Click **Add** button.</span></span> <span data-ttu-id="5cc89-220">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5cc89-220">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="5cc89-222">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="5cc89-222">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="5cc89-223">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5cc89-223">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5cc89-224">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5cc89-224">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5cc89-225">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="5cc89-225">Testing single sign-on</span></span>

<span data-ttu-id="5cc89-226">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="5cc89-226">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="5cc89-227">Ha a hozzáférési panelen Mixpanel csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Mixpanel alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="5cc89-227">When you click the Mixpanel tile in the Access Panel, you should get automatically signed-on to your Mixpanel application.</span></span>
<span data-ttu-id="5cc89-228">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="5cc89-228">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5cc89-229">További források</span><span class="sxs-lookup"><span data-stu-id="5cc89-229">Additional resources</span></span>

* [<span data-ttu-id="5cc89-230">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="5cc89-230">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5cc89-231">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="5cc89-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_203.png

