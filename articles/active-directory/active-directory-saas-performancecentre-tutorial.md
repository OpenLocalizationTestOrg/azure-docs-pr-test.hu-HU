---
title: "Oktatóanyag: Azure Active Directoryval integrált PerformanceCentre |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és PerformanceCentre között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 65288c32-f7e6-4eb3-a6dc-523c3d748d1c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: e86adaf4bd9b4752f2aece8207a8a423ec5590a6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-performancecentre"></a><span data-ttu-id="8ff31-103">Oktatóanyag: Azure Active Directoryval integrált PerformanceCentre</span><span class="sxs-lookup"><span data-stu-id="8ff31-103">Tutorial: Azure Active Directory integration with PerformanceCentre</span></span>

<span data-ttu-id="8ff31-104">Ebben az oktatóanyagban elsajátíthatja PerformanceCentre integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8ff31-104">In this tutorial, you learn how to integrate PerformanceCentre with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8ff31-105">PerformanceCentre integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="8ff31-105">Integrating PerformanceCentre with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="8ff31-106">Megadhatja a PerformanceCentre hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="8ff31-106">You can control in Azure AD who has access to PerformanceCentre</span></span>
- <span data-ttu-id="8ff31-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett PerformanceCentre (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="8ff31-107">You can enable your users to automatically get signed-on to PerformanceCentre (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8ff31-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="8ff31-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="8ff31-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8ff31-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8ff31-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8ff31-110">Prerequisites</span></span>

<span data-ttu-id="8ff31-111">Konfigurálása az Azure AD-integrációs PerformanceCentre, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="8ff31-111">To configure Azure AD integration with PerformanceCentre, you need the following items:</span></span>

- <span data-ttu-id="8ff31-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="8ff31-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8ff31-113">Egy PerformanceCentre egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="8ff31-113">A PerformanceCentre single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8ff31-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="8ff31-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8ff31-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="8ff31-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8ff31-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="8ff31-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8ff31-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8ff31-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8ff31-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="8ff31-118">Scenario description</span></span>
<span data-ttu-id="8ff31-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="8ff31-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8ff31-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="8ff31-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8ff31-121">A gyűjteményből PerformanceCentre hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8ff31-121">Adding PerformanceCentre from the gallery</span></span>
2. <span data-ttu-id="8ff31-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="8ff31-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-performancecentre-from-the-gallery"></a><span data-ttu-id="8ff31-123">A gyűjteményből PerformanceCentre hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8ff31-123">Adding PerformanceCentre from the gallery</span></span>
<span data-ttu-id="8ff31-124">Az Azure AD integrálása a PerformanceCentre konfigurálásához kell hozzáadnia PerformanceCentre a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="8ff31-124">To configure the integration of PerformanceCentre into Azure AD, you need to add PerformanceCentre from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="8ff31-125">**A gyűjteményből PerformanceCentre hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="8ff31-125">**To add PerformanceCentre from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="8ff31-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8ff31-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8ff31-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="8ff31-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="8ff31-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8ff31-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="8ff31-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="8ff31-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="8ff31-133">Írja be a keresőmezőbe, **PerformanceCentre**.</span><span class="sxs-lookup"><span data-stu-id="8ff31-133">In the search box, type **PerformanceCentre**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_search.png)

5. <span data-ttu-id="8ff31-135">Az eredmények panelen válassza ki a **PerformanceCentre**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="8ff31-135">In the results panel, select **PerformanceCentre**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8ff31-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="8ff31-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8ff31-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján PerformanceCentre.</span><span class="sxs-lookup"><span data-stu-id="8ff31-138">In this section, you configure and test Azure AD single sign-on with PerformanceCentre based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="8ff31-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó PerformanceCentre a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="8ff31-139">For single sign-on to work, Azure AD needs to know what the counterpart user in PerformanceCentre is to a user in Azure AD.</span></span> <span data-ttu-id="8ff31-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a PerformanceCentre közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="8ff31-140">In other words, a link relationship between an Azure AD user and the related user in PerformanceCentre needs to be established.</span></span>

<span data-ttu-id="8ff31-141">PerformanceCentre, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="8ff31-141">In PerformanceCentre, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="8ff31-142">Az Azure AD egyszeri bejelentkezést a PerformanceCentre tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="8ff31-142">To configure and test Azure AD single sign-on with PerformanceCentre, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="8ff31-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="8ff31-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="8ff31-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="8ff31-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8ff31-145">**[PerformanceCentre tesztfelhasználó létrehozása](#creating-a-performancecentre-test-user)**  - való Britta Simon valami PerformanceCentre, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="8ff31-145">**[Creating a PerformanceCentre test user](#creating-a-performancecentre-test-user)** - to have a counterpart of Britta Simon in PerformanceCentre that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="8ff31-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="8ff31-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8ff31-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="8ff31-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8ff31-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8ff31-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8ff31-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az PerformanceCentre alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="8ff31-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your PerformanceCentre application.</span></span>

<span data-ttu-id="8ff31-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés PerformanceCentre, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="8ff31-150">**To configure Azure AD single sign-on with PerformanceCentre, perform the following steps:**</span></span>

1. <span data-ttu-id="8ff31-151">Az Azure portálon a a **PerformanceCentre** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="8ff31-151">In the Azure portal, on the **PerformanceCentre** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="8ff31-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="8ff31-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_samlbase.png)

3. <span data-ttu-id="8ff31-155">Az a **PerformanceCentre tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="8ff31-155">On the **PerformanceCentre Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_url.png)

    <span data-ttu-id="8ff31-157">a.</span><span class="sxs-lookup"><span data-stu-id="8ff31-157">a.</span></span> <span data-ttu-id="8ff31-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`http://companyname.performancecentre.com/saml/SSO`</span><span class="sxs-lookup"><span data-stu-id="8ff31-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `http://companyname.performancecentre.com/saml/SSO`</span></span>

    <span data-ttu-id="8ff31-159">b.</span><span class="sxs-lookup"><span data-stu-id="8ff31-159">b.</span></span> <span data-ttu-id="8ff31-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`http://companyname.performancecentre.com`</span><span class="sxs-lookup"><span data-stu-id="8ff31-160">In the **Identifier** textbox, type a URL using the following pattern: `http://companyname.performancecentre.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="8ff31-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="8ff31-161">These values are not real.</span></span> <span data-ttu-id="8ff31-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="8ff31-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="8ff31-163">Ügyfél [PerformanceCentre ügyfél-támogatási csoport](https://www.performancecentre.com/contact-us/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="8ff31-163">Contact [PerformanceCentre Client support team](https://www.performancecentre.com/contact-us/) to get these values.</span></span> 

4. <span data-ttu-id="8ff31-164">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="8ff31-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_certificate.png) 

5. <span data-ttu-id="8ff31-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="8ff31-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-performancecentre-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="8ff31-168">A a **PerformanceCentre konfigurációs** kattintson **konfigurálása PerformanceCentre** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="8ff31-168">On the **PerformanceCentre Configuration** section, click **Configure PerformanceCentre** to open **Configure sign-on** window.</span></span> <span data-ttu-id="8ff31-169">Másolás a **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="8ff31-169">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_configure.png) 

7. <span data-ttu-id="8ff31-171">Bejelentkezés a **PerformanceCentre** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="8ff31-171">Sign-on to your **PerformanceCentre** company site as administrator.</span></span>

8. <span data-ttu-id="8ff31-172">A lap bal oldalán kattintson **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="8ff31-172">In the tab on the left side, click **Configure**.</span></span>
   
    ![Az Azure AD-egyszeri bejelentkezés][10]

9. <span data-ttu-id="8ff31-174">A lap bal oldalán kattintson **vegyes**, és kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="8ff31-174">In the tab on the left side, click **Miscellaneous**, and then click **Single Sign On**.</span></span>
   
    ![Az Azure AD-egyszeri bejelentkezés][11]

10. <span data-ttu-id="8ff31-176">Mint **protokoll**, jelölje be **SAML**.</span><span class="sxs-lookup"><span data-stu-id="8ff31-176">As **Protocol**, select **SAML**.</span></span>
   
    ![Az Azure AD-egyszeri bejelentkezés][12]

11. <span data-ttu-id="8ff31-178">Nyissa meg a letöltött metaadat-fájlt a Jegyzettömbben, másolja a tartalmat, illessze be azt a **Identity Provider metaadatok** szövegmező, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="8ff31-178">Open your downloaded metadata file in notepad, copy the content, paste it into the **Identity Provider Metadata** textbox, and then click **Save**.</span></span>
   
    ![Az Azure AD-egyszeri bejelentkezés][13]

12. <span data-ttu-id="8ff31-180">Ellenőrizze, hogy az értékek a **entitás alap URL-cím** és **entitás azonosító URL-cím** helyes-e.</span><span class="sxs-lookup"><span data-stu-id="8ff31-180">Verify that the values for the **Entity Base URL** and **Entity ID URL** are correct.</span></span>
    
     ![Az Azure AD-egyszeri bejelentkezés][14]

> [!TIP]
> <span data-ttu-id="8ff31-182">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="8ff31-182">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="8ff31-183">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="8ff31-183">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="8ff31-184">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8ff31-184">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8ff31-185">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8ff31-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="8ff31-186">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="8ff31-186">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="8ff31-188">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="8ff31-188">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="8ff31-189">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8ff31-189">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8ff31-191">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="8ff31-191">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8ff31-193">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="8ff31-193">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8ff31-195">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="8ff31-195">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8ff31-197">a.</span><span class="sxs-lookup"><span data-stu-id="8ff31-197">a.</span></span> <span data-ttu-id="8ff31-198">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8ff31-198">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8ff31-199">b.</span><span class="sxs-lookup"><span data-stu-id="8ff31-199">b.</span></span> <span data-ttu-id="8ff31-200">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8ff31-200">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8ff31-201">c.</span><span class="sxs-lookup"><span data-stu-id="8ff31-201">c.</span></span> <span data-ttu-id="8ff31-202">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="8ff31-202">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="8ff31-203">d.</span><span class="sxs-lookup"><span data-stu-id="8ff31-203">d.</span></span> <span data-ttu-id="8ff31-204">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="8ff31-204">Click **Create**.</span></span>
 
### <a name="creating-a-performancecentre-test-user"></a><span data-ttu-id="8ff31-205">PerformanceCentre tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8ff31-205">Creating a PerformanceCentre test user</span></span>

<span data-ttu-id="8ff31-206">Ez a szakasz célja PerformanceCentre Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="8ff31-206">The objective of this section is to create a user called Britta Simon in PerformanceCentre.</span></span>

<span data-ttu-id="8ff31-207">**A felhasználó Britta Simon meghívta PerformanceCentre létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="8ff31-207">**To create a user called Britta Simon in PerformanceCentre, perform the following steps:**</span></span>

1. <span data-ttu-id="8ff31-208">Jelentkezzen be rendszergazdaként a PerformanceCentre vállalati webhely.</span><span class="sxs-lookup"><span data-stu-id="8ff31-208">Sign on to your PerformanceCentre company site as administrator.</span></span>

2. <span data-ttu-id="8ff31-209">A bal oldali menüben kattintson a **Interrelate**, és kattintson a **létrehozása résztvevő**.</span><span class="sxs-lookup"><span data-stu-id="8ff31-209">In the menu on the left, click **Interrelate**, and then click **Create Participant**.</span></span>
   
    ![Felhasználó létrehozása][400]

3. <span data-ttu-id="8ff31-211">Az a **Interrelate - résztvevő létrehozása** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="8ff31-211">On the **Interrelate - Create Participant** dialog, perform the following steps:</span></span>
   
    ![Felhasználó létrehozása][401]
    
    <span data-ttu-id="8ff31-213">a.</span><span class="sxs-lookup"><span data-stu-id="8ff31-213">a.</span></span> <span data-ttu-id="8ff31-214">Írja be a szükséges attribútumokat a Britta Simon kapcsolódó szövegmezőből.</span><span class="sxs-lookup"><span data-stu-id="8ff31-214">Type the required attributes for Britta Simon into related textboxes.</span></span>
    
    >[!IMPORTANT]
    ><span data-ttu-id="8ff31-215">Britta tartozó felhasználónév attribútum PerformanceCentre kell lennie az Azure AD ugyanaz, mint a felhasználó nevét.</span><span class="sxs-lookup"><span data-stu-id="8ff31-215">Britta's User Name attribute in PerformanceCentre must be the same as the User Name in Azure AD.</span></span>
    
    <span data-ttu-id="8ff31-216">b.</span><span class="sxs-lookup"><span data-stu-id="8ff31-216">b.</span></span> <span data-ttu-id="8ff31-217">Válassza ki **ügyfél rendszergazda** , **válassza ki a szerepkör**.</span><span class="sxs-lookup"><span data-stu-id="8ff31-217">Select **Client Administrator** as **Choose Role**.</span></span>
    
    <span data-ttu-id="8ff31-218">c.</span><span class="sxs-lookup"><span data-stu-id="8ff31-218">c.</span></span> <span data-ttu-id="8ff31-219">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="8ff31-219">Click **Save**.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="8ff31-220">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="8ff31-220">Assigning the Azure AD test user</span></span>

<span data-ttu-id="8ff31-221">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés PerformanceCentre Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="8ff31-221">In this section, you enable Britta Simon to use Azure single sign-on by granting access to PerformanceCentre.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="8ff31-223">**Britta Simon hozzárendelése PerformanceCentre, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="8ff31-223">**To assign Britta Simon to PerformanceCentre, perform the following steps:**</span></span>

1. <span data-ttu-id="8ff31-224">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8ff31-224">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="8ff31-226">Az alkalmazások listában válassza ki a **PerformanceCentre**.</span><span class="sxs-lookup"><span data-stu-id="8ff31-226">In the applications list, select **PerformanceCentre**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_app.png) 

3. <span data-ttu-id="8ff31-228">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="8ff31-228">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="8ff31-230">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="8ff31-230">Click **Add** button.</span></span> <span data-ttu-id="8ff31-231">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8ff31-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="8ff31-233">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="8ff31-233">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="8ff31-234">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8ff31-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8ff31-235">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8ff31-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8ff31-236">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="8ff31-236">Testing single sign-on</span></span>

<span data-ttu-id="8ff31-237">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="8ff31-237">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>  

<span data-ttu-id="8ff31-238">Ha a hozzáférési panelen PerformanceCentre csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az PerformanceCentre alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="8ff31-238">When you click the PerformanceCentre tile in the Access Panel, you should get automatically signed-on to your PerformanceCentre application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8ff31-239">További források</span><span class="sxs-lookup"><span data-stu-id="8ff31-239">Additional resources</span></span>

* [<span data-ttu-id="8ff31-240">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="8ff31-240">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8ff31-241">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="8ff31-241">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_04.png
[10]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_06.png
[11]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_07.png
[12]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_08.png
[13]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_09.png
[14]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_10.png

[100]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_203.png
[400]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_11.png
[401]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_12.png

