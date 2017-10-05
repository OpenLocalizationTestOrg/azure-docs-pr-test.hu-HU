---
title: "Oktatóanyag: Azure Active Directoryval integrált ClickTime |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és ClickTime között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d437b5ab-4d71-4c13-96d0-79018cebbbd4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: jeedes
ms.openlocfilehash: 0e0123a40d52dfd7a2e29c29cb2239e979089ca9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-clicktime"></a><span data-ttu-id="84268-103">Oktatóanyag: Azure Active Directoryval integrált ClickTime</span><span class="sxs-lookup"><span data-stu-id="84268-103">Tutorial: Azure Active Directory integration with ClickTime</span></span>

<span data-ttu-id="84268-104">Ebben az oktatóanyagban elsajátíthatja ClickTime integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="84268-104">In this tutorial, you learn how to integrate ClickTime with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="84268-105">ClickTime integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="84268-105">Integrating ClickTime with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="84268-106">Megadhatja a ClickTime hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="84268-106">You can control in Azure AD who has access to ClickTime</span></span>
- <span data-ttu-id="84268-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett ClickTime (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="84268-107">You can enable your users to automatically get signed-on to ClickTime (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="84268-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="84268-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="84268-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="84268-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="84268-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="84268-110">Prerequisites</span></span>

<span data-ttu-id="84268-111">Konfigurálása az Azure AD-integrációs ClickTime, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="84268-111">To configure Azure AD integration with ClickTime, you need the following items:</span></span>

- <span data-ttu-id="84268-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="84268-112">An Azure AD subscription</span></span>
- <span data-ttu-id="84268-113">Egy ClickTime egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="84268-113">A ClickTime single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="84268-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="84268-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="84268-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="84268-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="84268-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="84268-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="84268-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="84268-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="84268-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="84268-118">Scenario description</span></span>
<span data-ttu-id="84268-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="84268-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="84268-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="84268-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="84268-121">A gyűjteményből ClickTime hozzáadása</span><span class="sxs-lookup"><span data-stu-id="84268-121">Adding ClickTime from the gallery</span></span>
2. <span data-ttu-id="84268-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="84268-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-clicktime-from-the-gallery"></a><span data-ttu-id="84268-123">A gyűjteményből ClickTime hozzáadása</span><span class="sxs-lookup"><span data-stu-id="84268-123">Adding ClickTime from the gallery</span></span>
<span data-ttu-id="84268-124">Az Azure AD integrálása a ClickTime konfigurálásához kell hozzáadnia ClickTime a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="84268-124">To configure the integration of ClickTime into Azure AD, you need to add ClickTime from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="84268-125">**A gyűjteményből ClickTime hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="84268-125">**To add ClickTime from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="84268-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="84268-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="84268-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="84268-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="84268-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="84268-129">Then go to **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="84268-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="84268-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="84268-133">Írja be a keresőmezőbe, **ClickTime**, jelölje be **ClickTime** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="84268-133">In the search box, type **ClickTime**, select **ClickTime** from result panel then click **Add** button to add the application.</span></span>

    ![Az eredménylistában ClickTime](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="84268-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="84268-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="84268-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján ClickTime.</span><span class="sxs-lookup"><span data-stu-id="84268-136">In this section, you configure and test Azure AD single sign-on with ClickTime based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="84268-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó ClickTime a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="84268-137">For single sign-on to work, Azure AD needs to know what the counterpart user in ClickTime is to a user in Azure AD.</span></span> <span data-ttu-id="84268-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a ClickTime közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="84268-138">In other words, a link relationship between an Azure AD user and the related user in ClickTime needs to be established.</span></span>

<span data-ttu-id="84268-139">ClickTime, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="84268-139">In ClickTime, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="84268-140">Az Azure AD egyszeri bejelentkezést a ClickTime tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="84268-140">To configure and test Azure AD single sign-on with ClickTime, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="84268-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="84268-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="84268-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="84268-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="84268-143">**[ClickTime tesztfelhasználó létrehozása](#create-a-clicktime-test-user)**  - való Britta Simon valami ClickTime, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="84268-143">**[Create a ClickTime test user](#create-a-clicktime-test-user)** - to have a counterpart of Britta Simon in ClickTime that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="84268-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="84268-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="84268-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="84268-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="84268-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="84268-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="84268-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az ClickTime alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="84268-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your ClickTime application.</span></span>

<span data-ttu-id="84268-148">**Konfigurálása az Azure AD az egyszeri bejelentkezés ClickTime, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="84268-148">**To configure Azure AD single sign-on with ClickTime, perform the following steps:**</span></span>

1. <span data-ttu-id="84268-149">Az Azure portálon a a **ClickTime** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="84268-149">In the Azure portal, on the **ClickTime** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="84268-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="84268-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_samlbase.png)

3. <span data-ttu-id="84268-153">Az a **ClickTime tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="84268-153">On the **ClickTime Domain and URLs** section, perform the following steps:</span></span>

    ![Az egyszeri bejelentkezés információk ClickTime tartomány és az URL-címek](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_url.png)

    <span data-ttu-id="84268-155">a.</span><span class="sxs-lookup"><span data-stu-id="84268-155">a.</span></span> <span data-ttu-id="84268-156">Az a **azonosító** szövegmező, adja meg az URL-címet:`https://app.clicktime.com/sp/`</span><span class="sxs-lookup"><span data-stu-id="84268-156">In the **Identifier** textbox, type a URL as: `https://app.clicktime.com/sp/`</span></span>
    
    <span data-ttu-id="84268-157">b.</span><span class="sxs-lookup"><span data-stu-id="84268-157">b.</span></span> <span data-ttu-id="84268-158">Az a **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő minták használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="84268-158">In the **Reply URL** textbox, type a URL using the following patterns:</span></span> 

    | |
    |--|
    | `https://app.clicktime.com/Login/` |
    | `https://app.clicktime.com/App/Login/Consume.aspx` |

4. <span data-ttu-id="84268-159">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="84268-159">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_certificate.png) 

5. <span data-ttu-id="84268-161">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="84268-161">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-clicktime-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="84268-163">A a **ClickTime konfigurációs** kattintson **konfigurálása ClickTime** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="84268-163">On the **ClickTime Configuration** section, click **Configure ClickTime** to open **Configure sign-on** window.</span></span> <span data-ttu-id="84268-164">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="84268-164">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![ClickTime konfiguráció](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_configure.png) 

7. <span data-ttu-id="84268-166">Egy másik webes böngészőablakban jelentkezzen be a ClickTime vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="84268-166">In a different web browser window, log into your ClickTime company site as an administrator.</span></span>

8. <span data-ttu-id="84268-167">A felső eszköztáron kattintson **beállítások**, és kattintson a **biztonsági beállítások**.</span><span class="sxs-lookup"><span data-stu-id="84268-167">In the toolbar on the top, click **Preferences**, and then click **Security Settings**.</span></span>

9. <span data-ttu-id="84268-168">Az a **egyszeri bejelentkezési beállítások** konfigurációs szakaszban, hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="84268-168">In the **Single Sign-On Preferences** configuration section, perform the following steps:</span></span>
   
    <span data-ttu-id="84268-169">![Biztonsági beállítások](./media/active-directory-saas-clicktime-tutorial/tic777280.png "biztonsági beállítások")</span><span class="sxs-lookup"><span data-stu-id="84268-169">![Security Settings](./media/active-directory-saas-clicktime-tutorial/tic777280.png "Security Settings")</span></span>
   
    <span data-ttu-id="84268-170">a.</span><span class="sxs-lookup"><span data-stu-id="84268-170">a.</span></span>  <span data-ttu-id="84268-171">Válassza ki **engedélyezése** jelentkezzen be egyszeri bejelentkezés (SSO) használatával **az Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="84268-171">Select **Allow** sign-in using Single Sign-On (SSO) with **Azure AD**.</span></span>
   
    <span data-ttu-id="84268-172">b.</span><span class="sxs-lookup"><span data-stu-id="84268-172">b.</span></span> <span data-ttu-id="84268-173">Az a **identitás szolgáltatói végpont** szövegmezőhöz Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="84268-173">In the **Identity Provider Endpoint** textbox, paste **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="84268-174">c.</span><span class="sxs-lookup"><span data-stu-id="84268-174">c.</span></span>  <span data-ttu-id="84268-175">Nyissa meg a **base-64 kódolású tanúsítvány** az Azure portálról letöltött **Jegyzettömb**, és másolja a tartalmat, majd illessze be azt a **X.509 tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="84268-175">Open the **base-64 encoded certificate** downloaded from Azure portal in **Notepad**, copy the content, and then paste it into the **X.509 Certificate** textbox.</span></span>
   
    <span data-ttu-id="84268-176">d.</span><span class="sxs-lookup"><span data-stu-id="84268-176">d.</span></span>  <span data-ttu-id="84268-177">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="84268-177">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="84268-178">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="84268-178">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="84268-179">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="84268-179">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="84268-180">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="84268-180">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="84268-181">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="84268-181">Create an Azure AD test user</span></span>
<span data-ttu-id="84268-182">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="84268-182">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="84268-184">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="84268-184">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="84268-185">Az Azure portálon a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="84268-185">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-clicktime-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="84268-187">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="84268-187">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>
    
    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-clicktime-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="84268-189">Megnyitásához a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** tetején a **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="84268-189">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>
 
    ![A Hozzáadás gombra.](./media/active-directory-saas-clicktime-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="84268-191">Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="84268-191">In the **User** dialog box, perform the following steps:</span></span>
 
    ![A felhasználó párbeszédpanel](./media/active-directory-saas-clicktime-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="84268-193">a.</span><span class="sxs-lookup"><span data-stu-id="84268-193">a.</span></span> <span data-ttu-id="84268-194">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="84268-194">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="84268-195">b.</span><span class="sxs-lookup"><span data-stu-id="84268-195">b.</span></span> <span data-ttu-id="84268-196">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="84268-196">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="84268-197">c.</span><span class="sxs-lookup"><span data-stu-id="84268-197">c.</span></span> <span data-ttu-id="84268-198">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="84268-198">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="84268-199">d.</span><span class="sxs-lookup"><span data-stu-id="84268-199">d.</span></span> <span data-ttu-id="84268-200">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="84268-200">Click **Create**.</span></span>
 
### <a name="create-a-clicktime-test-user"></a><span data-ttu-id="84268-201">ClickTime tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="84268-201">Create a ClickTime test user</span></span>

<span data-ttu-id="84268-202">Ahhoz, hogy az Azure AD-felhasználók ClickTime bejelentkezni, akkor ki kell építenie ClickTime be.</span><span class="sxs-lookup"><span data-stu-id="84268-202">In order to enable Azure AD users to log into ClickTime, they must be provisioned into ClickTime.</span></span>  
<span data-ttu-id="84268-203">ClickTime, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="84268-203">In the case of ClickTime, provisioning is a manual task.</span></span>

> [!NOTE]
> <span data-ttu-id="84268-204">Bármely más ClickTime felhasználói fiók létrehozása eszközök vagy ClickTime kiépíteni az Azure AD-felhasználói fiókok által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="84268-204">You can use any other ClickTime user account creation tools or APIs provided by ClickTime to provision Azure AD user accounts.</span></span>

<span data-ttu-id="84268-205">**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="84268-205">**To provision a user account, perform the following steps:**</span></span>
1. <span data-ttu-id="84268-206">Jelentkezzen be a **ClickTime** bérlő.</span><span class="sxs-lookup"><span data-stu-id="84268-206">Log in to your **ClickTime** tenant.</span></span>
2. <span data-ttu-id="84268-207">A felső eszköztáron kattintson **vállalati**, és kattintson a **személyek**.</span><span class="sxs-lookup"><span data-stu-id="84268-207">In the toolbar on the top, click **Company**, and then click **People**.</span></span>
   
    <span data-ttu-id="84268-208">![Személyek](./media/active-directory-saas-clicktime-tutorial/tic777282.png "személyek")</span><span class="sxs-lookup"><span data-stu-id="84268-208">![People](./media/active-directory-saas-clicktime-tutorial/tic777282.png "People")</span></span>
3. <span data-ttu-id="84268-209">Kattintson a **személy hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="84268-209">Click **Add Person**.</span></span>
   
    <span data-ttu-id="84268-210">![Adja hozzá a személy](./media/active-directory-saas-clicktime-tutorial/tic777283.png "személy hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="84268-210">![Add Person](./media/active-directory-saas-clicktime-tutorial/tic777283.png "Add Person")</span></span>
4. <span data-ttu-id="84268-211">Új személy csoportjában hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="84268-211">In the New Person section, perform the following steps:</span></span>
   
    <span data-ttu-id="84268-212">![Személyek](./media/active-directory-saas-clicktime-tutorial/tic777284.png "személyek")</span><span class="sxs-lookup"><span data-stu-id="84268-212">![People](./media/active-directory-saas-clicktime-tutorial/tic777284.png "People")</span></span>
   
    <span data-ttu-id="84268-213">a.</span><span class="sxs-lookup"><span data-stu-id="84268-213">a.</span></span>  <span data-ttu-id="84268-214">Az a **teljes név** szövegmező, például a felhasználó teljes név típusa **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="84268-214">In the **full name** textbox, type full name of user like **Britta Simon**.</span></span> 
  
    <span data-ttu-id="84268-215">b.</span><span class="sxs-lookup"><span data-stu-id="84268-215">b.</span></span>  <span data-ttu-id="84268-216">Az a **e-mail cím** szövegmezőben, az e-mailt a felhasználó típusát, például  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="84268-216">In the **email address** textbox, type the email of user like **brittasimon@contoso.com**.</span></span>
       
    > [!NOTE]
    > <span data-ttu-id="84268-217">Ha szeretné, beállíthatja az új személy objektum további tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="84268-217">If you want to, you can set additional properties of the new person object.</span></span>
   
    <span data-ttu-id="84268-218">c.</span><span class="sxs-lookup"><span data-stu-id="84268-218">c.</span></span>  <span data-ttu-id="84268-219">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="84268-219">Click **Save**.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="84268-220">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="84268-220">Assign the Azure AD test user</span></span>

<span data-ttu-id="84268-221">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés ClickTime Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="84268-221">In this section, you enable Britta Simon to use Azure single sign-on by granting access to ClickTime.</span></span>

![A felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="84268-223">**Britta Simon hozzárendelése ClickTime, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="84268-223">**To assign Britta Simon to ClickTime, perform the following steps:**</span></span>

1. <span data-ttu-id="84268-224">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="84268-224">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="84268-226">Az alkalmazások listában válassza ki a **ClickTime**.</span><span class="sxs-lookup"><span data-stu-id="84268-226">In the applications list, select **ClickTime**.</span></span>

    ![Az alkalmazások listáját a ClickTimne hivatkozás](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_app.png) 

3. <span data-ttu-id="84268-228">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="84268-228">In the menu on the left, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202] 

4. <span data-ttu-id="84268-230">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="84268-230">Click **Add** button.</span></span> <span data-ttu-id="84268-231">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="84268-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="84268-233">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="84268-233">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="84268-234">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="84268-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="84268-235">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="84268-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="84268-236">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="84268-236">Test single sign-on</span></span>

<span data-ttu-id="84268-237">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="84268-237">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="84268-238">Ha a hozzáférési panelen ClickTime csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az ClickTime alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="84268-238">When you click the ClickTime tile in the Access Panel, you should get automatically signed-on to your ClickTime application.</span></span>
<span data-ttu-id="84268-239">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="84268-239">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="84268-240">További források</span><span class="sxs-lookup"><span data-stu-id="84268-240">Additional resources</span></span>

* [<span data-ttu-id="84268-241">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="84268-241">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="84268-242">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="84268-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_203.png

