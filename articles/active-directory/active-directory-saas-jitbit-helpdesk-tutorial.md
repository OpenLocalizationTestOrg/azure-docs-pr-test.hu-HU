---
title: "Oktatóanyag: Azure Active Directoryval integrált Jitbit segélyszolgálat |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Jitbit segélyszolgálat között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 15ce27d4-0621-4103-8a34-e72c98d72ec3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 6523ee3179dafd79528093b856b0ec10fafb4f7b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-jitbit-helpdesk"></a><span data-ttu-id="da4aa-103">Oktatóanyag: Azure Active Directoryval integrált Jitbit segélyszolgálat</span><span class="sxs-lookup"><span data-stu-id="da4aa-103">Tutorial: Azure Active Directory integration with Jitbit Helpdesk</span></span>

<span data-ttu-id="da4aa-104">Ebben az oktatóanyagban elsajátíthatja Jitbit segélyszolgálat integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="da4aa-104">In this tutorial, you learn how to integrate Jitbit Helpdesk with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="da4aa-105">Jitbit segélyszolgálat integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="da4aa-105">Integrating Jitbit Helpdesk with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="da4aa-106">Megadhatja a Jitbit segélyszolgálati hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="da4aa-106">You can control in Azure AD who has access to Jitbit Helpdesk</span></span>
- <span data-ttu-id="da4aa-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett az Jitbit ügyfélszolgálathoz (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="da4aa-107">You can enable your users to automatically get signed-on to Jitbit Helpdesk (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="da4aa-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="da4aa-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="da4aa-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="da4aa-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="da4aa-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="da4aa-110">Prerequisites</span></span>

<span data-ttu-id="da4aa-111">Konfigurálása az Azure AD-integrációs Jitbit segélyszolgálat, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="da4aa-111">To configure Azure AD integration with Jitbit Helpdesk, you need the following items:</span></span>

- <span data-ttu-id="da4aa-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="da4aa-112">An Azure AD subscription</span></span>
- <span data-ttu-id="da4aa-113">Egy Jitbit segélyszolgálat egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="da4aa-113">A Jitbit Helpdesk single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="da4aa-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="da4aa-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="da4aa-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="da4aa-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="da4aa-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="da4aa-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="da4aa-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="da4aa-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="da4aa-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="da4aa-118">Scenario description</span></span>
<span data-ttu-id="da4aa-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="da4aa-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="da4aa-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="da4aa-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="da4aa-121">A gyűjteményből Jitbit segélyszolgálat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="da4aa-121">Adding Jitbit Helpdesk from the gallery</span></span>
2. <span data-ttu-id="da4aa-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="da4aa-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-jitbit-helpdesk-from-the-gallery"></a><span data-ttu-id="da4aa-123">A gyűjteményből Jitbit segélyszolgálat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="da4aa-123">Adding Jitbit Helpdesk from the gallery</span></span>
<span data-ttu-id="da4aa-124">Az Azure AD integrálása a Jitbit segélyszolgálat konfigurálásához kell hozzáadnia Jitbit segélyszolgálat a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="da4aa-124">To configure the integration of Jitbit Helpdesk into Azure AD, you need to add Jitbit Helpdesk from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="da4aa-125">**A gyűjteményből Jitbit segélyszolgálat hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="da4aa-125">**To add Jitbit Helpdesk from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="da4aa-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="da4aa-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="da4aa-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="da4aa-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="da4aa-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="da4aa-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="da4aa-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="da4aa-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="da4aa-133">Írja be a keresőmezőbe, **Jitbit segélyszolgálat**.</span><span class="sxs-lookup"><span data-stu-id="da4aa-133">In the search box, type **Jitbit Helpdesk**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_search.png)

5. <span data-ttu-id="da4aa-135">Az eredmények panelen válassza ki a **Jitbit segélyszolgálat**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="da4aa-135">In the results panel, select **Jitbit Helpdesk**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="da4aa-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="da4aa-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="da4aa-138">Ebben a szakaszban konfigurálhatja, és az Azure AD az egyszeri bejelentkezés Jitbit segélyszolgálat-teszthez "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="da4aa-138">In this section, you configure and test Azure AD single sign-on with Jitbit Helpdesk based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="da4aa-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Jitbit segélyszolgálat a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="da4aa-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Jitbit Helpdesk is to a user in Azure AD.</span></span> <span data-ttu-id="da4aa-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a Jitbit segélyszolgálat közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="da4aa-140">In other words, a link relationship between an Azure AD user and the related user in Jitbit Helpdesk needs to be established.</span></span>

<span data-ttu-id="da4aa-141">Jitbit segélyszolgálat, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="da4aa-141">In Jitbit Helpdesk, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="da4aa-142">Az Azure AD egyszeri bejelentkezést a Jitbit segélyszolgálat tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="da4aa-142">To configure and test Azure AD single sign-on with Jitbit Helpdesk, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="da4aa-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="da4aa-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="da4aa-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="da4aa-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="da4aa-145">**[Jitbit segélyszolgálat tesztfelhasználó létrehozása](#creating-a-jitbit-helpdesk-test-user)**  - való egy megfelelője a Britta Simon Jitbit segélyszolgálat, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="da4aa-145">**[Creating a Jitbit Helpdesk test user](#creating-a-jitbit-helpdesk-test-user)** - to have a counterpart of Britta Simon in Jitbit Helpdesk that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="da4aa-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="da4aa-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="da4aa-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="da4aa-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="da4aa-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="da4aa-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="da4aa-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Jitbit támogató alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="da4aa-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Jitbit Helpdesk application.</span></span>

<span data-ttu-id="da4aa-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Jitbit segélyszolgálat, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="da4aa-150">**To configure Azure AD single sign-on with Jitbit Helpdesk, perform the following steps:**</span></span>

1. <span data-ttu-id="da4aa-151">Az Azure portálon a a **Jitbit segélyszolgálat** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="da4aa-151">In the Azure portal, on the **Jitbit Helpdesk** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="da4aa-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="da4aa-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_samlbase.png)

3. <span data-ttu-id="da4aa-155">Az a **Jitbit segélyszolgálat tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="da4aa-155">On the **Jitbit Helpdesk Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_url.png)

    <span data-ttu-id="da4aa-157">a.</span><span class="sxs-lookup"><span data-stu-id="da4aa-157">a.</span></span> <span data-ttu-id="da4aa-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:</span><span class="sxs-lookup"><span data-stu-id="da4aa-158">In the **Sign-on URL** textbox, type a URL using the following pattern:</span></span> 
    | |     
    | ----------------------------------------|
    | `https://<hostname>/helpdesk/User/Login`|
    | `https://<tenant-name>.Jitbit.com`|
    | |
    
    > [!NOTE] 
    > <span data-ttu-id="da4aa-159">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="da4aa-159">This value is not real.</span></span> <span data-ttu-id="da4aa-160">Frissítse ezt az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="da4aa-160">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="da4aa-161">Ügyfél [Jitbit segélyszolgálat ügyfél-támogatási csoport](https://www.jitbit.com/support/) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="da4aa-161">Contact [Jitbit Helpdesk Client support team](https://www.jitbit.com/support/) to get this value.</span></span> 
    
    <span data-ttu-id="da4aa-162">b.</span><span class="sxs-lookup"><span data-stu-id="da4aa-162">b.</span></span>  <span data-ttu-id="da4aa-163">Az a **azonosító** szövegmező, a következő URL-címet írja be:`https://www.jitbit.com/web-helpdesk/`</span><span class="sxs-lookup"><span data-stu-id="da4aa-163">In the **Identifier** textbox, type a URL as following: `https://www.jitbit.com/web-helpdesk/`</span></span>

    
 


4. <span data-ttu-id="da4aa-164">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="da4aa-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_certificate.png) 

5. <span data-ttu-id="da4aa-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="da4aa-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="da4aa-168">A a **Jitbit segélyszolgálat konfigurációs** kattintson **konfigurálása Jitbit segélyszolgálat** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="da4aa-168">On the **Jitbit Helpdesk Configuration** section, click **Configure Jitbit Helpdesk** to open **Configure sign-on** window.</span></span> <span data-ttu-id="da4aa-169">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="da4aa-169">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_configure.png) 

7. <span data-ttu-id="da4aa-171">Egy másik webes böngészőablakban jelentkezzen be a Jitbit segélyszolgálat vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="da4aa-171">In a different web browser window, log into your Jitbit Helpdesk company site as an administrator.</span></span>

8. <span data-ttu-id="da4aa-172">A felső eszköztáron kattintson **felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="da4aa-172">In the toolbar on the top, click **Administration**.</span></span>
   
    <span data-ttu-id="da4aa-173">![Felügyeleti](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777681.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="da4aa-173">![Administration](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777681.png "Administration")</span></span>

9. <span data-ttu-id="da4aa-174">Kattintson a **általános beállítások**.</span><span class="sxs-lookup"><span data-stu-id="da4aa-174">Click **General settings**.</span></span>
   
    <span data-ttu-id="da4aa-175">![Felhasználók, a vállalatok és az engedélyek](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777680.png "felhasználók, a vállalatok és engedélyek")</span><span class="sxs-lookup"><span data-stu-id="da4aa-175">![Users, companies, and permissions](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777680.png "Users, companies, and permissions")</span></span>

10. <span data-ttu-id="da4aa-176">Az a **hitelesítési beállítások** konfigurációs szakaszban, hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="da4aa-176">In the **Authentication settings** configuration section, perform the following steps:</span></span>
   
    <span data-ttu-id="da4aa-177">![Hitelesítési beállítások](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777683.png "hitelesítési beállítások")</span><span class="sxs-lookup"><span data-stu-id="da4aa-177">![Authentication settings](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777683.png "Authentication settings")</span></span>
    
    <span data-ttu-id="da4aa-178">a.</span><span class="sxs-lookup"><span data-stu-id="da4aa-178">a.</span></span> <span data-ttu-id="da4aa-179">Válassza ki **engedélyezése SAML 2.0-s egyszeri bejelentkezés**, jelentkezzen be az egyszeri bejelentkezés (SSO), hogy **OneLogin**.</span><span class="sxs-lookup"><span data-stu-id="da4aa-179">Select **Enable SAML 2.0 single sign on**, to sign in using Single Sign-On (SSO), with **OneLogin**.</span></span>

    <span data-ttu-id="da4aa-180">b.</span><span class="sxs-lookup"><span data-stu-id="da4aa-180">b.</span></span> <span data-ttu-id="da4aa-181">Az a **végponti URL-cím** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="da4aa-181">In the **EndPoint URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="da4aa-182">c.</span><span class="sxs-lookup"><span data-stu-id="da4aa-182">c.</span></span> <span data-ttu-id="da4aa-183">Nyissa meg a **base-64** kódolású Jegyzettömb-tanúsítványt, a tartalmának másolása a vágólapra és illessze be azt a **X.509 tanúsítvány** szövegmező</span><span class="sxs-lookup"><span data-stu-id="da4aa-183">Open your **base-64** encoded certificate in notepad, copy the content of it into your clipboard, and then paste it to the **X.509 Certificate** textbox</span></span>

    <span data-ttu-id="da4aa-184">d.</span><span class="sxs-lookup"><span data-stu-id="da4aa-184">d.</span></span> <span data-ttu-id="da4aa-185">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="da4aa-185">Click **Save changes**.</span></span>

> [!TIP]
> <span data-ttu-id="da4aa-186">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="da4aa-186">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="da4aa-187">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="da4aa-187">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="da4aa-188">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="da4aa-188">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="da4aa-189">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="da4aa-189">Creating an Azure AD test user</span></span>
<span data-ttu-id="da4aa-190">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="da4aa-190">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="da4aa-192">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="da4aa-192">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="da4aa-193">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="da4aa-193">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="da4aa-195">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="da4aa-195">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="da4aa-197">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="da4aa-197">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="da4aa-199">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="da4aa-199">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="da4aa-201">a.</span><span class="sxs-lookup"><span data-stu-id="da4aa-201">a.</span></span> <span data-ttu-id="da4aa-202">Az a **neve** szövegmezőhöz típus neve megegyezik **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="da4aa-202">In the **Name** textbox, type name as **BrittaSimon**.</span></span>

    <span data-ttu-id="da4aa-203">b.</span><span class="sxs-lookup"><span data-stu-id="da4aa-203">b.</span></span> <span data-ttu-id="da4aa-204">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="da4aa-204">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="da4aa-205">c.</span><span class="sxs-lookup"><span data-stu-id="da4aa-205">c.</span></span> <span data-ttu-id="da4aa-206">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="da4aa-206">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="da4aa-207">d.</span><span class="sxs-lookup"><span data-stu-id="da4aa-207">d.</span></span> <span data-ttu-id="da4aa-208">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="da4aa-208">Click **Create**.</span></span>
 
### <a name="creating-a-jitbit-helpdesk-test-user"></a><span data-ttu-id="da4aa-209">Jitbit segélyszolgálat tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="da4aa-209">Creating a Jitbit Helpdesk test user</span></span>

<span data-ttu-id="da4aa-210">Ahhoz, hogy az Azure AD-felhasználók Jitbit segélyszolgálat bejelentkezni, akkor ki kell építenie Jitbit segélyszolgálat be.</span><span class="sxs-lookup"><span data-stu-id="da4aa-210">In order to enable Azure AD users to log into Jitbit Helpdesk, they must be provisioned into Jitbit Helpdesk.</span></span>  <span data-ttu-id="da4aa-211">Jitbit segélyszolgálat, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="da4aa-211">In the case of Jitbit Helpdesk, provisioning is a manual task.</span></span>

<span data-ttu-id="da4aa-212">**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="da4aa-212">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="da4aa-213">Jelentkezzen be a **Jitbit segélyszolgálat** bérlő.</span><span class="sxs-lookup"><span data-stu-id="da4aa-213">Log in to your **Jitbit Helpdesk** tenant.</span></span>

2. <span data-ttu-id="da4aa-214">Kattintson a felső menüben **felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="da4aa-214">In the menu on the top, click **Administration**.</span></span>
   
    <span data-ttu-id="da4aa-215">![Felügyeleti](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777681.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="da4aa-215">![Administration](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777681.png "Administration")</span></span>

3. <span data-ttu-id="da4aa-216">Kattintson a **felhasználók, a vállalatok és az engedélyek**.</span><span class="sxs-lookup"><span data-stu-id="da4aa-216">Click **Users, companies and permissions**.</span></span>
   
    <span data-ttu-id="da4aa-217">![Felhasználók, a vállalatok és az engedélyek](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777682.png "felhasználók, a vállalatok és engedélyek")</span><span class="sxs-lookup"><span data-stu-id="da4aa-217">![Users, companies, and permissions](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777682.png "Users, companies, and permissions")</span></span>

4. <span data-ttu-id="da4aa-218">Kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="da4aa-218">Click **Add user**.</span></span>
   
    <span data-ttu-id="da4aa-219">![Felhasználó hozzáadása](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777685.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="da4aa-219">![Add user](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777685.png "Add user")</span></span>
   
5. <span data-ttu-id="da4aa-220">Létrehozás területen írja be az adatokat az Azure AD-fiókot kíván létrehozni az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="da4aa-220">In the Create section, type the data of the Azure AD account you want to provision as follows:</span></span>

    <span data-ttu-id="da4aa-221">![Hozzon létre](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777686.png "létrehozása")</span><span class="sxs-lookup"><span data-stu-id="da4aa-221">![Create](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777686.png "Create")</span></span>
   
   <span data-ttu-id="da4aa-222">a.</span><span class="sxs-lookup"><span data-stu-id="da4aa-222">a.</span></span> <span data-ttu-id="da4aa-223">Az a **felhasználónév** szövegmezőhöz típus **BrittaSimon**, a felhasználónév, mint az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="da4aa-223">In the **Username** textbox, type **BrittaSimon**, the user name as in the Azure portal.</span></span>

   <span data-ttu-id="da4aa-224">b.</span><span class="sxs-lookup"><span data-stu-id="da4aa-224">b.</span></span> <span data-ttu-id="da4aa-225">Az a **E-mail** szövegmezőhöz, például a felhasználó e-mailek  **BrittaSimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="da4aa-225">In the **Email** textbox, type email of the user like **BrittaSimon@contoso.com**.</span></span>

   <span data-ttu-id="da4aa-226">c.</span><span class="sxs-lookup"><span data-stu-id="da4aa-226">c.</span></span> <span data-ttu-id="da4aa-227">Az a **Utónév** szövegmező, például a felhasználó első típusnév **Britta**.</span><span class="sxs-lookup"><span data-stu-id="da4aa-227">In the **First Name** textbox, type first name of the user like **Britta**.</span></span>

   <span data-ttu-id="da4aa-228">d.</span><span class="sxs-lookup"><span data-stu-id="da4aa-228">d.</span></span> <span data-ttu-id="da4aa-229">Az a **Vezetéknév** szövegmező, például a felhasználó vezetékneve típus **Simon**.</span><span class="sxs-lookup"><span data-stu-id="da4aa-229">In the **Last Name** textbox, type last name of the user like **Simon**.</span></span>
   
   <span data-ttu-id="da4aa-230">e.</span><span class="sxs-lookup"><span data-stu-id="da4aa-230">e.</span></span> <span data-ttu-id="da4aa-231">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="da4aa-231">Click **Create**.</span></span>

>[!NOTE]
><span data-ttu-id="da4aa-232">Jitbit segélyszolgálat felhasználói fiók létrehozása eszközök vagy Jitbit segélyszolgálat által nyújtott API-k segítségével kiépíteni az Azure AD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="da4aa-232">You can use any other Jitbit Helpdesk user account creation tools or APIs provided by Jitbit Helpdesk to provision Azure AD user accounts.</span></span>
> 
        

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="da4aa-233">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="da4aa-233">Assigning the Azure AD test user</span></span>

<span data-ttu-id="da4aa-234">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Jitbit segélyszolgálat Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="da4aa-234">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Jitbit Helpdesk.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="da4aa-236">**Britta Simon hozzárendelése Jitbit segélyszolgálat, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="da4aa-236">**To assign Britta Simon to Jitbit Helpdesk, perform the following steps:**</span></span>

1. <span data-ttu-id="da4aa-237">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="da4aa-237">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="da4aa-239">Az alkalmazások listában válassza ki a **Jitbit segélyszolgálat**.</span><span class="sxs-lookup"><span data-stu-id="da4aa-239">In the applications list, select **Jitbit Helpdesk**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_app.png) 

3. <span data-ttu-id="da4aa-241">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="da4aa-241">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="da4aa-243">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="da4aa-243">Click **Add** button.</span></span> <span data-ttu-id="da4aa-244">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="da4aa-244">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="da4aa-246">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="da4aa-246">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="da4aa-247">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="da4aa-247">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="da4aa-248">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="da4aa-248">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="da4aa-249">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="da4aa-249">Testing single sign-on</span></span>

<span data-ttu-id="da4aa-250">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="da4aa-250">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="da4aa-251">Ha a hozzáférési panelen Jitbit segélyszolgálat csempére kattint, Jitbit segélyszolgálat alkalmazás bejelentkezési oldalán szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="da4aa-251">When you click the Jitbit Helpdesk tile in the Access Panel, you should get login page of Jitbit Helpdesk application.</span></span>
<span data-ttu-id="da4aa-252">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="da4aa-252">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="da4aa-253">További források</span><span class="sxs-lookup"><span data-stu-id="da4aa-253">Additional resources</span></span>

* [<span data-ttu-id="da4aa-254">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="da4aa-254">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="da4aa-255">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="da4aa-255">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_203.png

