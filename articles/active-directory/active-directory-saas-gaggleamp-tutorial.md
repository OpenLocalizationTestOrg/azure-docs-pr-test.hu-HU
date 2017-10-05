---
title: "Oktatóanyag: Azure Active Directoryval integrált GaggleAMP |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és GaggleAMP között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9cc1a4b7-964b-406b-9e0c-05cb1a7c9856
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: c591cf99aecc4153e378c42a530b80deeca63158
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-gaggleamp"></a><span data-ttu-id="10085-103">Oktatóanyag: Azure Active Directoryval integrált GaggleAMP</span><span class="sxs-lookup"><span data-stu-id="10085-103">Tutorial: Azure Active Directory integration with GaggleAMP</span></span>

<span data-ttu-id="10085-104">Ebben az oktatóanyagban elsajátíthatja GaggleAMP integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="10085-104">In this tutorial, you learn how to integrate GaggleAMP with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="10085-105">GaggleAMP integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="10085-105">Integrating GaggleAMP with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="10085-106">Megadhatja a GaggleAMP hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="10085-106">You can control in Azure AD who has access to GaggleAMP</span></span>
- <span data-ttu-id="10085-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett GaggleAMP (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="10085-107">You can enable your users to automatically get signed-on to GaggleAMP (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="10085-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="10085-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="10085-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="10085-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="10085-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="10085-110">Prerequisites</span></span>

<span data-ttu-id="10085-111">Konfigurálása az Azure AD-integrációs GaggleAMP, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="10085-111">To configure Azure AD integration with GaggleAMP, you need the following items:</span></span>

- <span data-ttu-id="10085-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="10085-112">An Azure AD subscription</span></span>
- <span data-ttu-id="10085-113">Egy GaggleAMP egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="10085-113">A GaggleAMP single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="10085-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="10085-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="10085-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="10085-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="10085-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="10085-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="10085-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="10085-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="10085-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="10085-118">Scenario description</span></span>
<span data-ttu-id="10085-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="10085-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="10085-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="10085-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="10085-121">A gyűjteményből GaggleAMP hozzáadása</span><span class="sxs-lookup"><span data-stu-id="10085-121">Adding GaggleAMP from the gallery</span></span>
2. <span data-ttu-id="10085-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="10085-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-gaggleamp-from-the-gallery"></a><span data-ttu-id="10085-123">A gyűjteményből GaggleAMP hozzáadása</span><span class="sxs-lookup"><span data-stu-id="10085-123">Adding GaggleAMP from the gallery</span></span>
<span data-ttu-id="10085-124">Az Azure AD integrálása a GaggleAMP konfigurálásához kell hozzáadnia GaggleAMP a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="10085-124">To configure the integration of GaggleAMP into Azure AD, you need to add GaggleAMP from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="10085-125">**A gyűjteményből GaggleAMP hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="10085-125">**To add GaggleAMP from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="10085-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="10085-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="10085-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="10085-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="10085-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="10085-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="10085-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="10085-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="10085-133">Írja be a keresőmezőbe, **GaggleAMP**.</span><span class="sxs-lookup"><span data-stu-id="10085-133">In the search box, type **GaggleAMP**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_search.png)

5. <span data-ttu-id="10085-135">Az eredmények panelen válassza ki a **GaggleAMP**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="10085-135">In the results panel, select **GaggleAMP**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="10085-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="10085-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="10085-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján GaggleAMP.</span><span class="sxs-lookup"><span data-stu-id="10085-138">In this section, you configure and test Azure AD single sign-on with GaggleAMP based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="10085-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó GaggleAMP a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="10085-139">For single sign-on to work, Azure AD needs to know what the counterpart user in GaggleAMP is to a user in Azure AD.</span></span> <span data-ttu-id="10085-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a GaggleAMP közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="10085-140">In other words, a link relationship between an Azure AD user and the related user in GaggleAMP needs to be established.</span></span>

<span data-ttu-id="10085-141">GaggleAMP, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="10085-141">In GaggleAMP, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="10085-142">Az Azure AD egyszeri bejelentkezést a GaggleAMP tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="10085-142">To configure and test Azure AD single sign-on with GaggleAMP, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="10085-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="10085-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="10085-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="10085-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="10085-145">**[GaggleAMP tesztfelhasználó létrehozása](#creating-a-gaggleamp-test-user)**  - való Britta Simon valami GaggleAMP, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="10085-145">**[Creating a GaggleAMP test user](#creating-a-gaggleamp-test-user)** - to have a counterpart of Britta Simon in GaggleAMP that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="10085-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="10085-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="10085-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="10085-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="10085-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="10085-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="10085-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az GaggleAMP alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="10085-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your GaggleAMP application.</span></span>

<span data-ttu-id="10085-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés GaggleAMP, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="10085-150">**To configure Azure AD single sign-on with GaggleAMP, perform the following steps:**</span></span>

1. <span data-ttu-id="10085-151">Az Azure portálon a a **GaggleAMP** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="10085-151">In the Azure portal, on the **GaggleAMP** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="10085-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="10085-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_samlbase.png)

3. <span data-ttu-id="10085-155">Az a **GaggleAMP tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="10085-155">On the **GaggleAMP Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_url.png)

     <span data-ttu-id="10085-157">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.gaggleamp.com`</span><span class="sxs-lookup"><span data-stu-id="10085-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.gaggleamp.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="10085-158">Az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="10085-158">The value is not real.</span></span> <span data-ttu-id="10085-159">Frissítse az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="10085-159">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="10085-160">Ügyfél [GaggleAMP ügyfél-támogatási csoport](mailto:sales@gaggleamp.com) az értéket be kell olvasni.</span><span class="sxs-lookup"><span data-stu-id="10085-160">Contact [GaggleAMP Client support team](mailto:sales@gaggleamp.com) to get the value.</span></span> 
 
4. <span data-ttu-id="10085-161">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="10085-161">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_certificate.png) 

5. <span data-ttu-id="10085-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="10085-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="10085-165">A a **GaggleAMP konfigurációs** kattintson **konfigurálása GaggleAMP** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="10085-165">On the **GaggleAMP Configuration** section, click **Configure GaggleAMP** to open **Configure sign-on** window.</span></span> <span data-ttu-id="10085-166">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="10085-166">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_configure.png) 

7. <span data-ttu-id="10085-168">Egy másik böngészőben példányát, keresse meg a SAML SSO lap: a Gaggle támogatási csoport létrehozott (például: *https://accounts.gaggleamp.com/saml_configurations/oXH8sQcP79dOzgFPqrMTyw/edit*).</span><span class="sxs-lookup"><span data-stu-id="10085-168">In another browser instance, navigate to the SAML SSO page created for you by the Gaggle support team (for example: *https://accounts.gaggleamp.com/saml_configurations/oXH8sQcP79dOzgFPqrMTyw/edit*).</span></span>

8. <span data-ttu-id="10085-169">Az a **SAML SSO** lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="10085-169">On your **SAML SSO** page, perform the following steps:</span></span>  
   
    ![GaggleAMP egyszeri bejelentkezés](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_06.png) 
 
    <span data-ttu-id="10085-171">a.</span><span class="sxs-lookup"><span data-stu-id="10085-171">a.</span></span> <span data-ttu-id="10085-172">Az a **Identity Provider kibocsátó** szövegmezőhöz illessze be az értékét **kiállítójának URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="10085-172">In the **Identity Provider Issuer** textbox, paste the value of **Issuer URL** which you have copied from Azure portal.</span></span> 
 
    <span data-ttu-id="10085-173">b.</span><span class="sxs-lookup"><span data-stu-id="10085-173">b.</span></span> <span data-ttu-id="10085-174">Az a **Identity Provider egyetlen bejelentkezési URL-címet** szövegmezőhöz illessze be az értékét **egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="10085-174">In the **Identity Provider Single Sign-On URL** textbox, paste the  value of **Single Sign-On Service URL** which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="10085-175">c.</span><span class="sxs-lookup"><span data-stu-id="10085-175">c.</span></span> <span data-ttu-id="10085-176">Kattintson a **mentése**</span><span class="sxs-lookup"><span data-stu-id="10085-176">Click **Save**</span></span>      

    <span data-ttu-id="10085-177">d.</span><span class="sxs-lookup"><span data-stu-id="10085-177">d.</span></span> <span data-ttu-id="10085-178">Küldjön a **tanúsítvány (Base64)** tanúsítványt a a [GaggleAMP támogatási csoport](mailto:sales@gaggleamp.com).</span><span class="sxs-lookup"><span data-stu-id="10085-178">Send the **Certificate (Base64)** certificate to your [GaggleAMP support team](mailto:sales@gaggleamp.com).</span></span>

> [!TIP]
> <span data-ttu-id="10085-179">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="10085-179">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="10085-180">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="10085-180">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="10085-181">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="10085-181">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="10085-182">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="10085-182">Creating an Azure AD test user</span></span>
<span data-ttu-id="10085-183">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="10085-183">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="10085-185">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="10085-185">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="10085-186">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="10085-186">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="10085-188">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="10085-188">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="10085-190">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="10085-190">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="10085-192">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="10085-192">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="10085-194">a.</span><span class="sxs-lookup"><span data-stu-id="10085-194">a.</span></span> <span data-ttu-id="10085-195">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="10085-195">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="10085-196">b.</span><span class="sxs-lookup"><span data-stu-id="10085-196">b.</span></span> <span data-ttu-id="10085-197">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="10085-197">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="10085-198">c.</span><span class="sxs-lookup"><span data-stu-id="10085-198">c.</span></span> <span data-ttu-id="10085-199">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="10085-199">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="10085-200">d.</span><span class="sxs-lookup"><span data-stu-id="10085-200">d.</span></span> <span data-ttu-id="10085-201">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="10085-201">Click **Create**.</span></span>
 
### <a name="creating-a-gaggleamp-test-user"></a><span data-ttu-id="10085-202">GaggleAMP tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="10085-202">Creating a GaggleAMP test user</span></span>

<span data-ttu-id="10085-203">Ez a szakasz célja GaggleAMP Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="10085-203">The objective of this section is to create a user called Britta Simon in GaggleAMP.</span></span> <span data-ttu-id="10085-204">GaggleAMP támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="10085-204">GaggleAMP supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="10085-205">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="10085-205">There is no action item for you in this section.</span></span> <span data-ttu-id="10085-206">Új felhasználó jön létre az GaggleAMP elérésére, ha még nem létezik tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="10085-206">A new user is created during an attempt to access GaggleAMP if it doesn't exist yet.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="10085-207">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="10085-207">Assigning the Azure AD test user</span></span>

<span data-ttu-id="10085-208">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés GaggleAMP Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="10085-208">In this section, you enable Britta Simon to use Azure single sign-on by granting access to GaggleAMP.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="10085-210">**Britta Simon hozzárendelése GaggleAMP, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="10085-210">**To assign Britta Simon to GaggleAMP, perform the following steps:**</span></span>

1. <span data-ttu-id="10085-211">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="10085-211">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="10085-213">Az alkalmazások listában válassza ki a **GaggleAMP**.</span><span class="sxs-lookup"><span data-stu-id="10085-213">In the applications list, select **GaggleAMP**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_app.png) 

3. <span data-ttu-id="10085-215">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="10085-215">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="10085-217">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="10085-217">Click **Add** button.</span></span> <span data-ttu-id="10085-218">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="10085-218">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="10085-220">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="10085-220">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="10085-221">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="10085-221">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="10085-222">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="10085-222">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="10085-223">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="10085-223">Testing single sign-on</span></span>

<span data-ttu-id="10085-224">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="10085-224">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="10085-225">Ha a hozzáférési panelen GaggleAMP csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az GaggleAMP alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="10085-225">When you click the GaggleAMP tile in the Access Panel, you should get automatically signed-on to your GaggleAMP application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="10085-226">További források</span><span class="sxs-lookup"><span data-stu-id="10085-226">Additional resources</span></span>

* [<span data-ttu-id="10085-227">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="10085-227">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="10085-228">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="10085-228">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_203.png

