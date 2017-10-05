---
title: "Oktatóanyag: Azure Active Directoryval integrált kép továbbítási |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a lemezkép továbbítási között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 65bb5990-07ef-4244-9f41-cd28fc2cb5a2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 0bfbbaee7a74df6508584b7c8846fd07c2dc15c9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-image-relay"></a><span data-ttu-id="1e7e1-103">Oktatóanyag: Azure Active Directoryval integrált kép továbbító</span><span class="sxs-lookup"><span data-stu-id="1e7e1-103">Tutorial: Azure Active Directory integration with Image Relay</span></span>

<span data-ttu-id="1e7e1-104">Ebben az oktatóanyagban elsajátíthatja kép továbbítási integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="1e7e1-104">In this tutorial, you learn how to integrate Image Relay with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="1e7e1-105">Kép továbbítási integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="1e7e1-105">Integrating Image Relay with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="1e7e1-106">Megadhatja a kép továbbítási hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="1e7e1-106">You can control in Azure AD who has access to Image Relay</span></span>
- <span data-ttu-id="1e7e1-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett kép irányítja (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="1e7e1-107">You can enable your users to automatically get signed-on to Image Relay (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="1e7e1-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="1e7e1-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="1e7e1-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1e7e1-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1e7e1-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1e7e1-110">Prerequisites</span></span>

<span data-ttu-id="1e7e1-111">Az Azure AD rendszerrel történő integráció konfigurálása kép továbbító, a következő elemeket kell:</span><span class="sxs-lookup"><span data-stu-id="1e7e1-111">To configure Azure AD integration with Image Relay, you need the following items:</span></span>

- <span data-ttu-id="1e7e1-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="1e7e1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="1e7e1-113">Egy kép továbbítási egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="1e7e1-113">An Image Relay single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1e7e1-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="1e7e1-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="1e7e1-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="1e7e1-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="1e7e1-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1e7e1-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="1e7e1-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="1e7e1-118">Scenario description</span></span>
<span data-ttu-id="1e7e1-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="1e7e1-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="1e7e1-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="1e7e1-121">Kép továbbítási hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="1e7e1-121">Adding Image Relay from the gallery</span></span>
2. <span data-ttu-id="1e7e1-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="1e7e1-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-image-relay-from-the-gallery"></a><span data-ttu-id="1e7e1-123">Kép továbbítási hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="1e7e1-123">Adding Image Relay from the gallery</span></span>
<span data-ttu-id="1e7e1-124">Az Azure AD integrálása a kép továbbítási konfigurálásához kell hozzáadnia kép továbbító a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-124">To configure the integration of Image Relay into Azure AD, you need to add Image Relay from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="1e7e1-125">**Adja hozzá a kép továbbító a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1e7e1-125">**To add Image Relay from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="1e7e1-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="1e7e1-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="1e7e1-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="1e7e1-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="1e7e1-133">Írja be a keresőmezőbe, **kép továbbítási**.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-133">In the search box, type **Image Relay**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_search.png)

5. <span data-ttu-id="1e7e1-135">Az eredmények panelen válassza ki a **kép továbbítási**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-135">In the results panel, select **Image Relay**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="1e7e1-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="1e7e1-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="1e7e1-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján kép továbbító.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-138">In this section, you configure and test Azure AD single sign-on with Image Relay based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="1e7e1-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a kép továbbítási megfelelőjére felhasználót a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Image Relay is to a user in Azure AD.</span></span> <span data-ttu-id="1e7e1-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a kép továbbítási közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-140">In other words, a link relationship between an Azure AD user and the related user in Image Relay needs to be established.</span></span>

<span data-ttu-id="1e7e1-141">Kép továbbítási, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-141">In Image Relay, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="1e7e1-142">Az Azure AD egyszeri bejelentkezést a kép továbbítási tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="1e7e1-142">To configure and test Azure AD single sign-on with Image Relay, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="1e7e1-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="1e7e1-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1e7e1-145">**[Egy kép továbbítási tesztfelhasználó létrehozása](#creating-an-image-relay-test-user)**  - való egy megfelelője a Britta Simon kép továbbító, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-145">**[Creating an Image Relay test user](#creating-an-image-relay-test-user)** - to have a counterpart of Britta Simon in Image Relay that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="1e7e1-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="1e7e1-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="1e7e1-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1e7e1-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="1e7e1-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a lemezkép Relay alkalmazásban egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Image Relay application.</span></span>

<span data-ttu-id="1e7e1-150">**Az Azure AD az egyszeri bejelentkezés konfigurálása kép továbbítási, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1e7e1-150">**To configure Azure AD single sign-on with Image Relay, perform the following steps:**</span></span>

1. <span data-ttu-id="1e7e1-151">Az Azure portálon a a **kép továbbítási** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-151">In the Azure portal, on the **Image Relay** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="1e7e1-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_samlbase.png)

3. <span data-ttu-id="1e7e1-155">Az a **kép továbbítási tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="1e7e1-155">On the **Image Relay Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_url.png)

    <span data-ttu-id="1e7e1-157">a.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-157">a.</span></span> <span data-ttu-id="1e7e1-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.imagerelay.com/`</span><span class="sxs-lookup"><span data-stu-id="1e7e1-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.imagerelay.com/`</span></span>

    <span data-ttu-id="1e7e1-159">b.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-159">b.</span></span> <span data-ttu-id="1e7e1-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.imagerelay.com/sso/metadata`</span><span class="sxs-lookup"><span data-stu-id="1e7e1-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.imagerelay.com/sso/metadata`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="1e7e1-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-161">These values are not real.</span></span> <span data-ttu-id="1e7e1-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="1e7e1-163">Ügyfél [kép továbbítási ügyfél-támogatási csoport](http://support.imagerelay.com/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-163">Contact [Image Relay Client support team](http://support.imagerelay.com/) to get these values.</span></span> 
 


4. <span data-ttu-id="1e7e1-164">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_certificate.png) 

5. <span data-ttu-id="1e7e1-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="1e7e1-168">A a **kép továbbítási konfigurációs** területén kattintson **konfigurálása kép továbbítási** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-168">On the **Image Relay Configuration** section, click **Configure Image Relay** to open **Configure sign-on** window.</span></span> <span data-ttu-id="1e7e1-169">Másolás a **Sign-Out szolgáltatás URL-CÍMÉT és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="1e7e1-169">Copy the **Sign-Out Service URL and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_configure.png) 

7. <span data-ttu-id="1e7e1-171">Egy másik böngészőablakban jelentkezzen be a kép továbbítási vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-171">In another browser window, sign in to your Image Relay company site as an administrator.</span></span>

8. <span data-ttu-id="1e7e1-172">A felső eszköztáron kattintson a **felhasználók és engedélyek** munkaterhelés.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-172">In the toolbar on the top, click the **Users & Permissions** workload.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_06.png) 

9. <span data-ttu-id="1e7e1-174">Kattintson a **hozzon létre új engedély**.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-174">Click **Create New Permission**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_08.png)

10. <span data-ttu-id="1e7e1-176">Az a **egyetlen bejelentkezést a beállítások** munkaterhelés, jelölje be a **csak bejelentkezés egyszeri bejelentkezés útján lehet ennek a csoportnak a** jelölőnégyzetet, majd kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-176">In the **Single Sign On Settings** workload, select the **This Group can only sign-in via Single Sign On** check box, and then click **Save**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_09.png) 

11. <span data-ttu-id="1e7e1-178">Ugrás a **Fiókbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-178">Go to **Account Settings**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_10.png) 

12. <span data-ttu-id="1e7e1-180">Lépjen a **egyetlen bejelentkezést a beállítások** munkaterhelés.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-180">Go to the **Single Sign On Settings** workload.</span></span>
    
     ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_11.png)

13. <span data-ttu-id="1e7e1-182">Az a **SAML beállítások** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1e7e1-182">On the **SAML Settings** dialog, perform the following steps:</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_12.png)
    
    <span data-ttu-id="1e7e1-184">a.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-184">a.</span></span> <span data-ttu-id="1e7e1-185">A **bejelentkezési URL-cím** szövegmezőhöz illessze be az értékét **egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-185">In **Login URL** textbox, paste the value of **Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="1e7e1-186">b.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-186">b.</span></span> <span data-ttu-id="1e7e1-187">A **kijelentkezési URL-cím** szövegmezőhöz illessze be az értékét **egyetlen Sign-Out URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-187">In **Logout URL**  textbox, paste the value of **Single Sign-Out Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="1e7e1-188">c.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-188">c.</span></span> <span data-ttu-id="1e7e1-189">Mint **név Azonosítóformátumának**, jelölje be **urn: oasis: nevek: tc: SAML:1.1:nameid-formátum: emailAddress**.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-189">As **Name Id Format**, select **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.</span></span>

    <span data-ttu-id="1e7e1-190">d.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-190">d.</span></span> <span data-ttu-id="1e7e1-191">Mint **kötelező beállítások a szolgáltató (kép továbbító) a érkező kéréseket**, jelölje be **FELADÁS egy vagy több kötelező**.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-191">As **Binding Options for Requests from the Service Provider (Image Relay)**, select **POST Binding**.</span></span>

    <span data-ttu-id="1e7e1-192">e.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-192">e.</span></span> <span data-ttu-id="1e7e1-193">A **x.509 tanúsítvány**, kattintson a **frissítés tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-193">Under **x.509 Certificate**, click **Update Certificate**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_17.png)

    <span data-ttu-id="1e7e1-195">f.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-195">f.</span></span> <span data-ttu-id="1e7e1-196">A letöltött tanúsítvány megnyitása a Jegyzettömbben, másolja a tartalmat, és illessze be az x.509 tanúsítvány szövegmező.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-196">Open the downloaded certificate in notepad, copy the content, and then paste it into the x.509 Certificate textbox.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_18.png)

    <span data-ttu-id="1e7e1-198">g.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-198">g.</span></span> <span data-ttu-id="1e7e1-199">A **fordítója Felhasználólétesítés** szakaszban jelölje be a **fordítója Felhasználólétesítés engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-199">In **Just-In-Time User Provisioning** section, select the **Enable Just-In-Time User Provisioning**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_19.png)

    <span data-ttu-id="1e7e1-201">h.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-201">h.</span></span> <span data-ttu-id="1e7e1-202">Válassza ki az engedélycsoport (például **SSO alapvető**) amely megengedett csak keresztül egyszeri bejelentkezés bejelentkezni.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-202">Select the permission group (for example, **SSO Basic**) which is allowed to sign in only through single sign-on.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_20.png)

    <span data-ttu-id="1e7e1-204">i.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-204">i.</span></span> <span data-ttu-id="1e7e1-205">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-205">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="1e7e1-206">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="1e7e1-206">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="1e7e1-207">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-207">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="1e7e1-208">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="1e7e1-208">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="1e7e1-209">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="1e7e1-209">Creating an Azure AD test user</span></span>
<span data-ttu-id="1e7e1-210">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-210">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="1e7e1-212">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1e7e1-212">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="1e7e1-213">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-213">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="1e7e1-215">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-215">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="1e7e1-217">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-217">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="1e7e1-219">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="1e7e1-219">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="1e7e1-221">a.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-221">a.</span></span> <span data-ttu-id="1e7e1-222">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-222">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="1e7e1-223">b.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-223">b.</span></span> <span data-ttu-id="1e7e1-224">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-224">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="1e7e1-225">c.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-225">c.</span></span> <span data-ttu-id="1e7e1-226">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-226">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="1e7e1-227">d.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-227">d.</span></span> <span data-ttu-id="1e7e1-228">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-228">Click **Create**.</span></span>
 
### <a name="creating-an-image-relay-test-user"></a><span data-ttu-id="1e7e1-229">Egy kép továbbítási tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="1e7e1-229">Creating an Image Relay test user</span></span>

<span data-ttu-id="1e7e1-230">Ez a szakasz célja Britta Simon meghívta lemezképét továbbítási felhasználó létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-230">The objective of this section is to create a user called Britta Simon in Image Relay.</span></span>

<span data-ttu-id="1e7e1-231">**A felhasználó Britta Simon meghívta lemezképét továbbítási létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1e7e1-231">**To create a user called Britta Simon in Image Relay, perform the following steps:**</span></span>

1. <span data-ttu-id="1e7e1-232">Bejelentkezés a kép továbbítási vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-232">Sign-on to your Image Relay company site as an administrator.</span></span>

2. <span data-ttu-id="1e7e1-233">Nyissa meg a **felhasználók és engedélyek** válassza **egyszeri Bejelentkezéses felhasználó létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-233">Go to **Users & Permissions**     and select **Create SSO User**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_21.png) 

3. <span data-ttu-id="1e7e1-235">Adja meg a **E-mail**, **Utónév**, **Vezetéknév**, és **vállalati** ellátásához, majd válassza ki az engedélycsoport (például egyszeri bejelentkezés alapvető) amely a csoport, amely csak keresztül egyszeri bejelentkezés bejelentkezhessen a kívánt felhasználó.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-235">Enter the **Email**, **First Name**, **Last Name**, and **Company** of the user you want to provision and select the permission group (for example, SSO Basic) which is the group that can sign in only through single sign-on.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_22.png) 

4. <span data-ttu-id="1e7e1-237">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-237">Click **Create**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="1e7e1-238">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="1e7e1-238">Assigning the Azure AD test user</span></span>

<span data-ttu-id="1e7e1-239">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés kép továbbítási Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-239">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Image Relay.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="1e7e1-241">**Kép továbbítási Britta Simon rendel, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1e7e1-241">**To assign Britta Simon to Image Relay, perform the following steps:**</span></span>

1. <span data-ttu-id="1e7e1-242">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-242">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="1e7e1-244">Az alkalmazások listában válassza ki a **kép továbbítási**.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-244">In the applications list, select **Image Relay**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_app.png) 

3. <span data-ttu-id="1e7e1-246">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-246">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="1e7e1-248">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-248">Click **Add** button.</span></span> <span data-ttu-id="1e7e1-249">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-249">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="1e7e1-251">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-251">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="1e7e1-252">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-252">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="1e7e1-253">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-253">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="1e7e1-254">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="1e7e1-254">Testing single sign-on</span></span>

<span data-ttu-id="1e7e1-255">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-255">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>    

<span data-ttu-id="1e7e1-256">A hozzáférési panelen kép továbbítási csempére kattintva, meg kell beolvasása automatikusan bejelentkezett a kép továbbítási alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="1e7e1-256">When you click the Image Relay tile in the Access Panel, you should get automatically signed-on to your Image Relay application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="1e7e1-257">További források</span><span class="sxs-lookup"><span data-stu-id="1e7e1-257">Additional resources</span></span>

* [<span data-ttu-id="1e7e1-258">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="1e7e1-258">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1e7e1-259">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="1e7e1-259">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_04.png


[100]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_203.png

