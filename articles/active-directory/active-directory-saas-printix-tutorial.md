---
title: "Oktatóanyag: Azure Active Directoryval integrált Printix |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Printix között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4aea7320-b2d5-49e0-9b63-aeaff0f6fe66
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: 97dbb3fa0531f2f679badb6bb9752f2e42fc9cb3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-printix"></a><span data-ttu-id="5d4fc-103">Oktatóanyag: Azure Active Directoryval integrált Printix</span><span class="sxs-lookup"><span data-stu-id="5d4fc-103">Tutorial: Azure Active Directory integration with Printix</span></span>

<span data-ttu-id="5d4fc-104">Ebben az oktatóanyagban elsajátíthatja Printix integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5d4fc-104">In this tutorial, you learn how to integrate Printix with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5d4fc-105">Printix integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="5d4fc-105">Integrating Printix with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="5d4fc-106">Megadhatja a Printix hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="5d4fc-106">You can control in Azure AD who has access to Printix</span></span>
- <span data-ttu-id="5d4fc-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Printix (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="5d4fc-107">You can enable your users to automatically get signed-on to Printix (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5d4fc-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="5d4fc-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="5d4fc-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5d4fc-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5d4fc-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5d4fc-110">Prerequisites</span></span>

<span data-ttu-id="5d4fc-111">Konfigurálása az Azure AD-integrációs Printix, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="5d4fc-111">To configure Azure AD integration with Printix, you need the following items:</span></span>

- <span data-ttu-id="5d4fc-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="5d4fc-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5d4fc-113">Egy Printix egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="5d4fc-113">A Printix single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5d4fc-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5d4fc-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="5d4fc-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5d4fc-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5d4fc-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5d4fc-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5d4fc-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="5d4fc-118">Scenario description</span></span>
<span data-ttu-id="5d4fc-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5d4fc-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="5d4fc-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5d4fc-121">A gyűjteményből Printix hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5d4fc-121">Adding Printix from the gallery</span></span>
2. <span data-ttu-id="5d4fc-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="5d4fc-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-printix-from-the-gallery"></a><span data-ttu-id="5d4fc-123">A gyűjteményből Printix hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5d4fc-123">Adding Printix from the gallery</span></span>
<span data-ttu-id="5d4fc-124">Az Azure AD integrálása a Printix konfigurálásához kell hozzáadnia Printix a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-124">To configure the integration of Printix into Azure AD, you need to add Printix from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="5d4fc-125">**A gyűjteményből Printix hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="5d4fc-125">**To add Printix from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="5d4fc-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5d4fc-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="5d4fc-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="5d4fc-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="5d4fc-133">Írja be a keresőmezőbe, **Printix**.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-133">In the search box, type **Printix**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-printix-tutorial/tutorial_printix_search.png)

5. <span data-ttu-id="5d4fc-135">Az eredmények panelen válassza ki a **Printix**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-135">In the results panel, select **Printix**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-printix-tutorial/tutorial_printix_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5d4fc-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="5d4fc-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5d4fc-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Printix.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-138">In this section, you configure and test Azure AD single sign-on with Printix based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="5d4fc-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Printix a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Printix is to a user in Azure AD.</span></span> <span data-ttu-id="5d4fc-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Printix közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-140">In other words, a link relationship between an Azure AD user and the related user in Printix needs to be established.</span></span>

<span data-ttu-id="5d4fc-141">Printix, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-141">In Printix, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="5d4fc-142">Az Azure AD egyszeri bejelentkezést a Printix tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="5d4fc-142">To configure and test Azure AD single sign-on with Printix, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="5d4fc-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="5d4fc-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5d4fc-145">**[Printix tesztfelhasználó létrehozása](#creating-a-printix-test-user)**  - való Britta Simon valami Printix, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-145">**[Creating a Printix test user](#creating-a-printix-test-user)** - to have a counterpart of Britta Simon in Printix that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="5d4fc-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5d4fc-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5d4fc-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5d4fc-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5d4fc-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Printix alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Printix application.</span></span>

<span data-ttu-id="5d4fc-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Printix, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="5d4fc-150">**To configure Azure AD single sign-on with Printix, perform the following steps:**</span></span>

1. <span data-ttu-id="5d4fc-151">Az Azure portálon a a **Printix** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-151">In the Azure portal, on the **Printix** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="5d4fc-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-printix-tutorial/tutorial_printix_samlbase.png)

3. <span data-ttu-id="5d4fc-155">Az a **Printix tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="5d4fc-155">On the **Printix Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-printix-tutorial/tutorial_printix_url.png)

    <span data-ttu-id="5d4fc-157">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.printix.net`</span><span class="sxs-lookup"><span data-stu-id="5d4fc-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.printix.net`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5d4fc-158">Az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-158">The value is not real.</span></span> <span data-ttu-id="5d4fc-159">Frissítse az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-159">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="5d4fc-160">Ügyfél [Printix ügyfél-támogatási csoport](mailto:support@printix.net) az értéket be kell olvasni.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-160">Contact [Printix Client support team](mailto:support@printix.net) to get the value.</span></span> 
 
4. <span data-ttu-id="5d4fc-161">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-printix-tutorial/tutorial_printix_certificate.png) 

5. <span data-ttu-id="5d4fc-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-printix-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5d4fc-165">Bejelentkezés a Printix bérlő rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-165">Sign-on to your Printix tenant as an administrator.</span></span>

7. <span data-ttu-id="5d4fc-166">A felső menüben kattintson a jobb felső sarokban, és válasszon "**hitelesítési**".</span><span class="sxs-lookup"><span data-stu-id="5d4fc-166">In the menu on the top, click the icon at the upper right corner and select "**Authentication**".</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-printix-tutorial/tutorial_printix_06.png)

8. <span data-ttu-id="5d4fc-168">Az a **telepítő** lapon jelölje be **engedélyezése Azure vagy Office 365-hitelesítés**</span><span class="sxs-lookup"><span data-stu-id="5d4fc-168">On the **Setup** tab, select **Enable Azure/Office 365 authentication**</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-printix-tutorial/tutorial_printix_07.png)

9. <span data-ttu-id="5d4fc-170">Az a **Azure** lapján adjon meg a TextBox elemhez, az összevonási metaadatok URL-címe "**összevonási metaadat-dokumentum**".</span><span class="sxs-lookup"><span data-stu-id="5d4fc-170">On the **Azure** tab, input federation metadata URL to the textbox of "**Federation metadata document**".</span></span> 

    <span data-ttu-id="5d4fc-171">A metaadatok XML-fájl, amely letöltötte az Azure AD-csatolása [Printix támogatási csoport](mailto:support@printix.net).</span><span class="sxs-lookup"><span data-stu-id="5d4fc-171">Attach the metadata xml file which you downloaded from Azure AD to [Printix support team](mailto:support@printix.net).</span></span> <span data-ttu-id="5d4fc-172">Ezután töltse fel az XML-fájl, és adjon meg egy összevonási metaadatainak URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-172">Then they upload the xml file and provide a federation metadata URL.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-printix-tutorial/tutorial_printix_08.png)
   
10. <span data-ttu-id="5d4fc-174">Kattintson a "**tesztelése**"gombra, majd kattintson a"**OK**" gombra, ha a teszt sikeres volt.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-174">Click the "**Test**" button and click "**OK**" button if the test was successful.</span></span>
   
     <span data-ttu-id="5d4fc-175">Az Azure active directory lap kattintás után megjelenik a **tesztelése** gombra.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-175">Azure active directory page will show after clicking the **test** button.</span></span> <span data-ttu-id="5d4fc-176">"A teszt sikeres volt" Itt azt jelenti, hogy a jelenik azt Azure vizsgálati fiók hitelesítő adatait beírását követően üzenet "beállítások tesztelt OK". Kattintson a **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-176">"The test was successful" here means after entering the credentials of your Azure test account it will pop up a message "Settings tested OK".Then click the **OK** button.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-printix-tutorial/tutorial_printix_09.png)

11. <span data-ttu-id="5d4fc-178">Kattintson a **mentése** gombra kattint, a "**hitelesítési**" lapon.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-178">Click the **Save** button on "**Authentication**" page.</span></span>


> [!TIP]
> <span data-ttu-id="5d4fc-179">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="5d4fc-179">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="5d4fc-180">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-180">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="5d4fc-181">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5d4fc-181">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5d4fc-182">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="5d4fc-182">Creating an Azure AD test user</span></span>
<span data-ttu-id="5d4fc-183">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-183">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="5d4fc-185">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="5d4fc-185">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="5d4fc-186">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-186">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-printix-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5d4fc-188">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-188">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-printix-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5d4fc-190">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-190">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-printix-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5d4fc-192">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="5d4fc-192">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-printix-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5d4fc-194">a.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-194">a.</span></span> <span data-ttu-id="5d4fc-195">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-195">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5d4fc-196">b.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-196">b.</span></span> <span data-ttu-id="5d4fc-197">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-197">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5d4fc-198">c.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-198">c.</span></span> <span data-ttu-id="5d4fc-199">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-199">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="5d4fc-200">d.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-200">d.</span></span> <span data-ttu-id="5d4fc-201">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-201">Click **Create**.</span></span>
 
### <a name="creating-a-printix-test-user"></a><span data-ttu-id="5d4fc-202">Printix tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="5d4fc-202">Creating a Printix test user</span></span>

<span data-ttu-id="5d4fc-203">Ez a szakasz célja Printix Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-203">The objective of this section is to create a user called Britta Simon in Printix.</span></span> <span data-ttu-id="5d4fc-204">Printix támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-204">Printix supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="5d4fc-205">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-205">There is no action item for you in this section.</span></span> <span data-ttu-id="5d4fc-206">Új felhasználó jön létre az Printix elérésére, ha még nem létezik tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-206">A new user is created during an attempt to access Printix if it doesn't exist yet.</span></span> 

> [!NOTE]
> <span data-ttu-id="5d4fc-207">Hozza létre a felhasználó manuálisan kell, ha szeretné-e lépjen kapcsolatba a [Printix támogatási csoport](mailto:support@printix.net).</span><span class="sxs-lookup"><span data-stu-id="5d4fc-207">If you need to create a user manually, you need to contact the [Printix support team](mailto:support@printix.net).</span></span>
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="5d4fc-208">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="5d4fc-208">Assigning the Azure AD test user</span></span>

<span data-ttu-id="5d4fc-209">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Printix Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-209">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Printix.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="5d4fc-211">**Britta Simon hozzárendelése Printix, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="5d4fc-211">**To assign Britta Simon to Printix, perform the following steps:**</span></span>

1. <span data-ttu-id="5d4fc-212">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-212">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="5d4fc-214">Az alkalmazások listában válassza ki a **Printix**.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-214">In the applications list, select **Printix**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-printix-tutorial/tutorial_printix_app.png) 

3. <span data-ttu-id="5d4fc-216">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-216">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="5d4fc-218">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-218">Click **Add** button.</span></span> <span data-ttu-id="5d4fc-219">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-219">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="5d4fc-221">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-221">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="5d4fc-222">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-222">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5d4fc-223">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-223">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5d4fc-224">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="5d4fc-224">Testing single sign-on</span></span>

<span data-ttu-id="5d4fc-225">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-225">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="5d4fc-226">Ha a hozzáférési panelen Printix csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Printix alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="5d4fc-226">When you click the Printix tile in the Access Panel, you should get automatically signed-on to your Printix application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5d4fc-227">További források</span><span class="sxs-lookup"><span data-stu-id="5d4fc-227">Additional resources</span></span>

* [<span data-ttu-id="5d4fc-228">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="5d4fc-228">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5d4fc-229">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="5d4fc-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-printix-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-printix-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-printix-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-printix-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-printix-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-printix-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-printix-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-printix-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-printix-tutorial/tutorial_general_203.png

