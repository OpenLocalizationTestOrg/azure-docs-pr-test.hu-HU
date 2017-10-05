---
title: "Oktatóanyag: Azure Active Directoryval integrált AnswerHub |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és AnswerHub között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 818b91d7-01df-4b36-9706-f167c710a73c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 3a1c9cc5d7a2ebe28e9fb7e0e6ed8e3d393873ae
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-answerhub"></a><span data-ttu-id="3d9f1-103">Oktatóanyag: Azure Active Directoryval integrált AnswerHub</span><span class="sxs-lookup"><span data-stu-id="3d9f1-103">Tutorial: Azure Active Directory integration with AnswerHub</span></span>

<span data-ttu-id="3d9f1-104">Ebben az oktatóanyagban elsajátíthatja AnswerHub integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3d9f1-104">In this tutorial, you learn how to integrate AnswerHub with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3d9f1-105">AnswerHub integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="3d9f1-105">Integrating AnswerHub with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3d9f1-106">Megadhatja a AnswerHub hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="3d9f1-106">You can control in Azure AD who has access to AnswerHub</span></span>
- <span data-ttu-id="3d9f1-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett AnswerHub (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="3d9f1-107">You can enable your users to automatically get signed-on to AnswerHub (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3d9f1-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="3d9f1-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="3d9f1-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3d9f1-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3d9f1-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3d9f1-110">Prerequisites</span></span>

<span data-ttu-id="3d9f1-111">Konfigurálása az Azure AD-integrációs AnswerHub, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="3d9f1-111">To configure Azure AD integration with AnswerHub, you need the following items:</span></span>

- <span data-ttu-id="3d9f1-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="3d9f1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3d9f1-113">Egy AnswerHub egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="3d9f1-113">An AnswerHub single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3d9f1-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3d9f1-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="3d9f1-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3d9f1-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3d9f1-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3d9f1-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3d9f1-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="3d9f1-118">Scenario description</span></span>
<span data-ttu-id="3d9f1-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3d9f1-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="3d9f1-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3d9f1-121">A gyűjteményből AnswerHub hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3d9f1-121">Adding AnswerHub from the gallery</span></span>
2. <span data-ttu-id="3d9f1-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="3d9f1-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-answerhub-from-the-gallery"></a><span data-ttu-id="3d9f1-123">A gyűjteményből AnswerHub hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3d9f1-123">Adding AnswerHub from the gallery</span></span>
<span data-ttu-id="3d9f1-124">Az Azure AD integrálása a AnswerHub konfigurálásához kell hozzáadnia AnswerHub a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-124">To configure the integration of AnswerHub into Azure AD, you need to add AnswerHub from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3d9f1-125">**A gyűjteményből AnswerHub hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3d9f1-125">**To add AnswerHub from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3d9f1-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3d9f1-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="3d9f1-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="3d9f1-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="3d9f1-133">Írja be a keresőmezőbe, **AnswerHub**.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-133">In the search box, type **AnswerHub**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_search.png)

5. <span data-ttu-id="3d9f1-135">Az eredmények panelen válassza ki a **AnswerHub**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-135">In the results panel, select **AnswerHub**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3d9f1-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="3d9f1-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3d9f1-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján AnswerHub.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-138">In this section, you configure and test Azure AD single sign-on with AnswerHub based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="3d9f1-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó AnswerHub a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-139">For single sign-on to work, Azure AD needs to know what the counterpart user in AnswerHub is to a user in Azure AD.</span></span> <span data-ttu-id="3d9f1-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a AnswerHub közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-140">In other words, a link relationship between an Azure AD user and the related user in AnswerHub needs to be established.</span></span>

<span data-ttu-id="3d9f1-141">AnswerHub, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-141">In AnswerHub, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="3d9f1-142">Az Azure AD egyszeri bejelentkezést a AnswerHub tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="3d9f1-142">To configure and test Azure AD single sign-on with AnswerHub, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3d9f1-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3d9f1-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3d9f1-145">**[Egy AnswerHub tesztfelhasználó létrehozása](#creating-an-answerhub-test-user)**  - való Britta Simon valami AnswerHub, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-145">**[Creating an AnswerHub test user](#creating-an-answerhub-test-user)** - to have a counterpart of Britta Simon in AnswerHub that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="3d9f1-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3d9f1-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3d9f1-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3d9f1-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3d9f1-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az AnswerHub alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your AnswerHub application.</span></span>

<span data-ttu-id="3d9f1-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés AnswerHub, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3d9f1-150">**To configure Azure AD single sign-on with AnswerHub, perform the following steps:**</span></span>

1. <span data-ttu-id="3d9f1-151">Az Azure portálon a a **AnswerHub** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-151">In the Azure portal, on the **AnswerHub** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="3d9f1-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_samlbase.png)

3. <span data-ttu-id="3d9f1-155">Az a **AnswerHub tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="3d9f1-155">On the **AnswerHub Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_url.png)

    <span data-ttu-id="3d9f1-157">a.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-157">a.</span></span> <span data-ttu-id="3d9f1-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company>.answerhub.com`</span><span class="sxs-lookup"><span data-stu-id="3d9f1-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company>.answerhub.com`</span></span>

    <span data-ttu-id="3d9f1-159">b.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-159">b.</span></span> <span data-ttu-id="3d9f1-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company>.answerhub.com`</span><span class="sxs-lookup"><span data-stu-id="3d9f1-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<company>.answerhub.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3d9f1-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-161">These values are not real.</span></span> <span data-ttu-id="3d9f1-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="3d9f1-163">Ügyfél [AnswerHub ügyfél-támogatási csoport](mailto:success@answerhub.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-163">Contact [AnswerHub Client support team](mailto:success@answerhub.com) to get these values.</span></span> 
 
4. <span data-ttu-id="3d9f1-164">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_certificate.png) 

5. <span data-ttu-id="3d9f1-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-answerhub-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3d9f1-168">A a **AnswerHub konfigurációs** kattintson **konfigurálása AnswerHub** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-168">On the **AnswerHub Configuration** section, click **Configure AnswerHub** to open **Configure sign-on** window.</span></span> <span data-ttu-id="3d9f1-169">Másolás a **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="3d9f1-169">Copy the **Sign-Out URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_configure.png) 

7. <span data-ttu-id="3d9f1-171">Egy másik webes böngészőablakban jelentkezzen be a AnswerHub vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-171">In a different web browser window, log into your AnswerHub company site as an administrator.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="3d9f1-172">Ha AnswerHub segítségre van szüksége, forduljon a [AnswerHub tartozó támogatási csoport](mailto:success@answerhub.com.).</span><span class="sxs-lookup"><span data-stu-id="3d9f1-172">If you need help configuring AnswerHub, contact [AnswerHub's support team](mailto:success@answerhub.com.).</span></span>
   
8. <span data-ttu-id="3d9f1-173">Ugrás a **felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-173">Go to **Administration**.</span></span>

9. <span data-ttu-id="3d9f1-174">Kattintson a **felhasználók és csoportok** fülre.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-174">Click the **User and Group** tab.</span></span>

10. <span data-ttu-id="3d9f1-175">A navigációs ablakban a bal oldalon található a **közösségi beállítások** területen kattintson **SAML-alapú telepítő**.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-175">In the navigation pane on the left side, in the **Social Settings** section, click **SAML Setup**.</span></span>

11. <span data-ttu-id="3d9f1-176">Kattintson a **IDP Config** fülre.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-176">Click **IDP Config** tab.</span></span>

12. <span data-ttu-id="3d9f1-177">Az a **IDP Config** lapra, hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="3d9f1-177">On the **IDP Config** tab, perform the following steps:</span></span>

     <span data-ttu-id="3d9f1-178">![SAML-alapú telepítő](./media/active-directory-saas-answerhub-tutorial/ic785172.png "SAML-alapú telepítő")</span><span class="sxs-lookup"><span data-stu-id="3d9f1-178">![SAML Setup](./media/active-directory-saas-answerhub-tutorial/ic785172.png "SAML Setup")</span></span>  
  
     <span data-ttu-id="3d9f1-179">a.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-179">a.</span></span> <span data-ttu-id="3d9f1-180">A **IDP bejelentkezési URL-cím** szövegmezőhöz Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-180">In **IDP Login URL** textbox, paste **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
  
     <span data-ttu-id="3d9f1-181">b.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-181">b.</span></span> <span data-ttu-id="3d9f1-182">A **IDP kijelentkezési URL-cím** szövegmezőhöz Beillesztés **Sign-Out URL-cím** érték, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-182">In **IDP Logout URL** textbox, paste **Sign-Out URL** value which you have copied from Azure portal.</span></span>
     
     <span data-ttu-id="3d9f1-183">c.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-183">c.</span></span> <span data-ttu-id="3d9f1-184">A **IDP azonosító formátuma** szövegmező, adja meg a felhasználói azonosító érték ugyanaz, mint a kijelölt Azure-portálon a **felhasználói attribútumok** szakasz.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-184">In **IDP Name Identifier Format** textbox, enter the user Identifier value same as selected in Azure portal in **User Attributes** section.</span></span>
  
     <span data-ttu-id="3d9f1-185">d.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-185">d.</span></span> <span data-ttu-id="3d9f1-186">Kattintson a **a kulcsoknak és tanúsítványoknak**.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-186">Click **Keys and Certificates**.</span></span>

13. <span data-ttu-id="3d9f1-187">A kulcsoknak és tanúsítványoknak lapon hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="3d9f1-187">On the Keys and Certificates tab, perform the following steps:</span></span>
    
     <span data-ttu-id="3d9f1-188">![A kulcsoknak és tanúsítványoknak](./media/active-directory-saas-answerhub-tutorial/ic785173.png "a kulcsoknak és tanúsítványoknak")</span><span class="sxs-lookup"><span data-stu-id="3d9f1-188">![Keys and Certificates](./media/active-directory-saas-answerhub-tutorial/ic785173.png "Keys and Certificates")</span></span>  
 
     <span data-ttu-id="3d9f1-189">a.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-189">a.</span></span> <span data-ttu-id="3d9f1-190">Nyissa meg a base-64 kódolású tanúsítványt, amely a Jegyzettömbben az Azure portálról letöltött a tartalmának másolása a vágólapra és illessze be azt a **kiállító terjesztési hely nyilvános kulcsát (x 509-formátumú)** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-190">Open your base-64 encoded certificate which you have downloaded from Azure portal in notepad, copy the content of it into your clipboard, and then paste it to the **IDP Public Key (x509 Format)** textbox.</span></span>
  
     <span data-ttu-id="3d9f1-191">b.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-191">b.</span></span> <span data-ttu-id="3d9f1-192">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-192">Click **Save**.</span></span>

14. <span data-ttu-id="3d9f1-193">Az a **IDP Config** lapra, majd **mentése**.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-193">On the **IDP Config** tab, click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="3d9f1-194">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="3d9f1-194">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="3d9f1-195">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-195">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="3d9f1-196">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3d9f1-196">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3d9f1-197">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="3d9f1-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="3d9f1-198">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-198">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="3d9f1-200">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3d9f1-200">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3d9f1-201">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-201">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-answerhub-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3d9f1-203">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-203">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-answerhub-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3d9f1-205">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-205">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-answerhub-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3d9f1-207">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="3d9f1-207">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-answerhub-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3d9f1-209">a.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-209">a.</span></span> <span data-ttu-id="3d9f1-210">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-210">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3d9f1-211">b.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-211">b.</span></span> <span data-ttu-id="3d9f1-212">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-212">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3d9f1-213">c.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-213">c.</span></span> <span data-ttu-id="3d9f1-214">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-214">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="3d9f1-215">d.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-215">d.</span></span> <span data-ttu-id="3d9f1-216">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-216">Click **Create**.</span></span>
 
### <a name="creating-an-answerhub-test-user"></a><span data-ttu-id="3d9f1-217">Egy AnswerHub tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="3d9f1-217">Creating an AnswerHub test user</span></span>

<span data-ttu-id="3d9f1-218">Ahhoz, hogy az Azure AD-felhasználók AnswerHub bejelentkezni, akkor ki kell építenie a AnswerHub.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-218">To enable Azure AD users to log in to AnswerHub, they must be provisioned into AnswerHub.</span></span>  
<span data-ttu-id="3d9f1-219">AnswerHub, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-219">In the case of AnswerHub, provisioning is a manual task.</span></span>

<span data-ttu-id="3d9f1-220">**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3d9f1-220">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="3d9f1-221">Jelentkezzen be a **AnswerHub** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-221">Log in to your **AnswerHub** company site as administrator.</span></span>

2. <span data-ttu-id="3d9f1-222">Ugrás a **felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-222">Go to **Administration**.</span></span>

3. <span data-ttu-id="3d9f1-223">Kattintson a **felhasználók és csoportok** fülre.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-223">Click the **Users & Groups** tab.</span></span>

4. <span data-ttu-id="3d9f1-224">A navigációs ablakban a bal oldalon található a **felhasználók kezelése** területen kattintson **létrehozása vagy importálása felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-224">In the navigation pane on the left side, in the **Manage Users** section, click **Create or import users**.</span></span>
   
   <span data-ttu-id="3d9f1-225">![Felhasználók és csoportok](./media/active-directory-saas-answerhub-tutorial/ic785175.png "felhasználók és csoportok")</span><span class="sxs-lookup"><span data-stu-id="3d9f1-225">![Users & Groups](./media/active-directory-saas-answerhub-tutorial/ic785175.png "Users & Groups")</span></span>

5. <span data-ttu-id="3d9f1-226">Típus a **E-mail cím**, **felhasználónév** és **jelszó** egy érvényes Azure Active Directory-fiók a kapcsolódó szövegmezők kiépíteni, és kattintson a kívánt **mentése**.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-226">Type the **Email address**, **Username** and **Password** of a valid Azure Active Directory account you want to provision into the related textboxes, and then click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="3d9f1-227">Bármely más AnswerHub felhasználói fiók létrehozása eszközök vagy rendelkezés AAD felhasználói fiókokhoz AnswerHub által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-227">You can use any other AnswerHub user account creation tools or APIs provided by AnswerHub to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="3d9f1-228">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="3d9f1-228">Assigning the Azure AD test user</span></span>

<span data-ttu-id="3d9f1-229">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés AnswerHub Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-229">In this section, you enable Britta Simon to use Azure single sign-on by granting access to AnswerHub.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="3d9f1-231">**Britta Simon hozzárendelése AnswerHub, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3d9f1-231">**To assign Britta Simon to AnswerHub, perform the following steps:**</span></span>

1. <span data-ttu-id="3d9f1-232">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-232">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="3d9f1-234">Az alkalmazások listában válassza ki a **AnswerHub**.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-234">In the applications list, select **AnswerHub**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_app.png) 

3. <span data-ttu-id="3d9f1-236">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-236">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="3d9f1-238">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-238">Click **Add** button.</span></span> <span data-ttu-id="3d9f1-239">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-239">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="3d9f1-241">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-241">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="3d9f1-242">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-242">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3d9f1-243">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-243">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3d9f1-244">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="3d9f1-244">Testing single sign-on</span></span>

<span data-ttu-id="3d9f1-245">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-245">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="3d9f1-246">Ha a hozzáférési panelen AnswerHub csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az AnswerHub alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="3d9f1-246">When you click the AnswerHub tile in the Access Panel, you should get automatically signed-on to your AnswerHub application.</span></span>
<span data-ttu-id="3d9f1-247">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3d9f1-247">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3d9f1-248">További források</span><span class="sxs-lookup"><span data-stu-id="3d9f1-248">Additional resources</span></span>

* [<span data-ttu-id="3d9f1-249">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="3d9f1-249">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3d9f1-250">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="3d9f1-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_203.png

