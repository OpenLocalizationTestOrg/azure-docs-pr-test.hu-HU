---
title: "Oktatóanyag: Azure Active Directoryval integrált Citrix ShareFile |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a Citrix ShareFile között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: e14fc310-bac4-4f09-99ef-87e5c77288b6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2017
ms.author: jeedes
ms.openlocfilehash: b85680104fe4f33638c559b2a12483a2312a4476
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-citrix-sharefile"></a><span data-ttu-id="d7578-103">Oktatóanyag: Azure Active Directoryval integrált Citrix ShareFile</span><span class="sxs-lookup"><span data-stu-id="d7578-103">Tutorial: Azure Active Directory integration with Citrix ShareFile</span></span>

<span data-ttu-id="d7578-104">Ebben az oktatóanyagban elsajátíthatja a Citrix ShareFile integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d7578-104">In this tutorial, you learn how to integrate Citrix ShareFile with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d7578-105">Citrix ShareFile integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="d7578-105">Integrating Citrix ShareFile with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d7578-106">Az Azure AD, aki hozzáfér a Citrix ShareFile szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="d7578-106">You can control in Azure AD who has access to Citrix ShareFile.</span></span>
- <span data-ttu-id="d7578-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a Citrix ShareFile (egyszeri bejelentkezés).</span><span class="sxs-lookup"><span data-stu-id="d7578-107">You can enable your users to automatically get signed-on to Citrix ShareFile (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="d7578-108">A fiók egyetlen központi helyen – az Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="d7578-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="d7578-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d7578-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d7578-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d7578-110">Prerequisites</span></span>

<span data-ttu-id="d7578-111">Az Azure AD-integráció konfigurálása a Citrix ShareFile, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="d7578-111">To configure Azure AD integration with Citrix ShareFile, you need the following items:</span></span>

- <span data-ttu-id="d7578-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="d7578-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d7578-113">A Citrix ShareFile egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="d7578-113">A Citrix ShareFile single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d7578-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="d7578-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d7578-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="d7578-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d7578-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="d7578-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d7578-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d7578-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d7578-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="d7578-118">Scenario description</span></span>
<span data-ttu-id="d7578-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="d7578-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d7578-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="d7578-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d7578-121">Adja hozzá a Citrix ShareFile a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="d7578-121">Add Citrix ShareFile from the gallery</span></span>
2. <span data-ttu-id="d7578-122">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d7578-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-citrix-sharefile-from-the-gallery"></a><span data-ttu-id="d7578-123">Adja hozzá a Citrix ShareFile a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="d7578-123">Add Citrix ShareFile from the gallery</span></span>
<span data-ttu-id="d7578-124">Az Azure AD integrálása a Citrix ShareFile konfigurálásához kell hozzáadnia a Citrix ShareFile a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="d7578-124">To configure the integration of Citrix ShareFile into Azure AD, you need to add Citrix ShareFile from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d7578-125">**Adja hozzá a Citrix ShareFile a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d7578-125">**To add Citrix ShareFile from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d7578-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d7578-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="d7578-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="d7578-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d7578-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d7578-129">Then go to **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="d7578-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="d7578-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="d7578-133">Írja be a keresőmezőbe, **Citrix ShareFile**, jelölje be **Citrix ShareFile** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d7578-133">In the search box, type **Citrix ShareFile**, select **Citrix ShareFile** from result panel then click **Add** button to add the application.</span></span>

    ![Citrix ShareFile az eredménylistában](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="d7578-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d7578-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="d7578-136">Ebben a szakaszban konfigurálása és tesztelése az Azure AD egyszeri bejelentkezést a Citrix ShareFile "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="d7578-136">In this section, you configure and test Azure AD single sign-on with Citrix ShareFile based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d7578-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a Citrix ShareFile tartozó felhasználót a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="d7578-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Citrix ShareFile is to a user in Azure AD.</span></span> <span data-ttu-id="d7578-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Citrix ShareFile közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="d7578-138">In other words, a link relationship between an Azure AD user and the related user in Citrix ShareFile needs to be established.</span></span>

<span data-ttu-id="d7578-139">A Citrix ShareFile, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="d7578-139">In Citrix ShareFile, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="d7578-140">Az Azure AD egyszeri bejelentkezést a Citrix ShareFile tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="d7578-140">To configure and test Azure AD single sign-on with Citrix ShareFile, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d7578-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="d7578-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d7578-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="d7578-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d7578-143">**[Citrix ShareFile tesztfelhasználó létrehozása](#create-a-citrix-sharefile-test-user)**  - való Britta Simon egy megfelelője a Citrix ShareFile, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="d7578-143">**[Create a Citrix ShareFile test user](#create-a-citrix-sharefile-test-user)** - to have a counterpart of Britta Simon in Citrix ShareFile that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d7578-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d7578-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d7578-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="d7578-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="d7578-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d7578-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="d7578-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a Citrix ShareFile alkalmazásban egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="d7578-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Citrix ShareFile application.</span></span>

<span data-ttu-id="d7578-148">**A Citrix ShareFile konfigurálása az Azure AD egyszeri bejelentkezést, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d7578-148">**To configure Azure AD single sign-on with Citrix ShareFile, perform the following steps:**</span></span>

1. <span data-ttu-id="d7578-149">Az Azure portálon a a **Citrix ShareFile** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d7578-149">In the Azure portal, on the **Citrix ShareFile** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="d7578-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d7578-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_samlbase.png)

3. <span data-ttu-id="d7578-153">Az a **Citrix ShareFile tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="d7578-153">On the **Citrix ShareFile Domain and URLs** section, perform the following steps:</span></span>

    ![Citrix ShareFile tartomány és az URL-címeket az egyszeri bejelentkezés információk](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_url.png)
    
    <span data-ttu-id="d7578-155">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<tenant-name>.sharefile.com/saml/login`</span><span class="sxs-lookup"><span data-stu-id="d7578-155">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenant-name>.sharefile.com/saml/login`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d7578-156">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="d7578-156">This value is not real.</span></span> <span data-ttu-id="d7578-157">Frissítse ezt az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="d7578-157">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="d7578-158">Ügyfél [Citrix ShareFile ügyfél-támogatási csoport](https://www.citrix.co.in/products/sharefile/support.html) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="d7578-158">Contact [Citrix ShareFile Client support team](https://www.citrix.co.in/products/sharefile/support.html) to get this value.</span></span> 

4. <span data-ttu-id="d7578-159">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d7578-159">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_certificate.png) 

5. <span data-ttu-id="d7578-161">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="d7578-161">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-sharefile-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d7578-163">A a **Citrix ShareFile konfigurációs** kattintson **konfigurálása a Citrix ShareFile** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="d7578-163">On the **Citrix ShareFile Configuration** section, click **Configure Citrix ShareFile** to open **Configure sign-on** window.</span></span> <span data-ttu-id="d7578-164">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="d7578-164">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Citrix ShareFile konfiguráció](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_configure.png) 

7. <span data-ttu-id="d7578-166">Egy másik webes böngészőablakban, jelentkezzen be a **Citrix ShareFile** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="d7578-166">In a different web browser window, log into your **Citrix ShareFile** company site as an administrator.</span></span>

8. <span data-ttu-id="d7578-167">A felső eszköztáron kattintson **Admin**.</span><span class="sxs-lookup"><span data-stu-id="d7578-167">In the toolbar on the top, click **Admin**.</span></span>

9. <span data-ttu-id="d7578-168">A bal oldali navigációs ablakból válassza **konfigurálása egyszeri bejelentkezéshez**.</span><span class="sxs-lookup"><span data-stu-id="d7578-168">In the left navigation pane, select **Configure Single Sign-On**.</span></span>
   
    <span data-ttu-id="d7578-169">![Felügyeleti fiók](./media/active-directory-saas-sharefile-tutorial/ic773627.png "felügyeleti fiók")</span><span class="sxs-lookup"><span data-stu-id="d7578-169">![Account Administration](./media/active-directory-saas-sharefile-tutorial/ic773627.png "Account Administration")</span></span>

10. <span data-ttu-id="d7578-170">Az a **egyszeri bejelentkezés / SAML 2.0 konfigurációs** párbeszédpanel lapján **alapbeállítások**, hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="d7578-170">On the **Single Sign-On/ SAML 2.0 Configuration** dialog page under **Basic Settings**, perform the following steps:</span></span>
   
    <span data-ttu-id="d7578-171">![Egyszeri bejelentkezés](./media/active-directory-saas-sharefile-tutorial/ic773628.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="d7578-171">![Single sign-on](./media/active-directory-saas-sharefile-tutorial/ic773628.png "Single sign-on")</span></span>
   
    <span data-ttu-id="d7578-172">a.</span><span class="sxs-lookup"><span data-stu-id="d7578-172">a.</span></span> <span data-ttu-id="d7578-173">Kattintson a **SAML engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="d7578-173">Click **Enable SAML**.</span></span>
    
    <span data-ttu-id="d7578-174">b.</span><span class="sxs-lookup"><span data-stu-id="d7578-174">b.</span></span> <span data-ttu-id="d7578-175">A **a kiállító terjesztési hely kibocsátó / Entitásazonosító** szövegmezőhöz illessze be az értékét **SAML Entitásazonosító** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="d7578-175">In **Your IDP Issuer/ Entity ID** textbox, paste the value of **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="d7578-176">c.</span><span class="sxs-lookup"><span data-stu-id="d7578-176">c.</span></span> <span data-ttu-id="d7578-177">Kattintson a **módosítás** mellett a **X.509 tanúsítvány** mezőben, és ezután az az Azure-portálról letöltött tanúsítvány feltöltése.</span><span class="sxs-lookup"><span data-stu-id="d7578-177">Click **Change** next to the **X.509 Certificate** field and then upload the certificate you downloaded from the Azure portal.</span></span>
    
    <span data-ttu-id="d7578-178">d.</span><span class="sxs-lookup"><span data-stu-id="d7578-178">d.</span></span> <span data-ttu-id="d7578-179">A **bejelentkezési URL-cím** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="d7578-179">In **Login URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="d7578-180">e.</span><span class="sxs-lookup"><span data-stu-id="d7578-180">e.</span></span> <span data-ttu-id="d7578-181">A **kijelentkezési URL-cím** szövegmezőhöz illessze be az értékét **Sign-Out URL-cím** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="d7578-181">In **Logout URL** textbox, paste the value of **Sign-Out URL** which you have copied from Azure portal.</span></span>

11. <span data-ttu-id="d7578-182">Kattintson a **mentése** a Citrix ShareFile felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="d7578-182">Click **Save** on the Citrix ShareFile management portal.</span></span>

> [!TIP]
> <span data-ttu-id="d7578-183">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="d7578-183">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d7578-184">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="d7578-184">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d7578-185">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d7578-185">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="d7578-186">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="d7578-186">Create an Azure AD test user</span></span>

<span data-ttu-id="d7578-187">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="d7578-187">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="d7578-189">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d7578-189">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d7578-190">Az Azure portálon a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="d7578-190">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-sharefile-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="d7578-192">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="d7578-192">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-sharefile-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="d7578-194">Megnyitásához a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** tetején a **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="d7578-194">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![A Hozzáadás gombra.](./media/active-directory-saas-sharefile-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="d7578-196">Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="d7578-196">In the **User** dialog box, perform the following steps:</span></span>

    ![A felhasználó párbeszédpanel](./media/active-directory-saas-sharefile-tutorial/create_aaduser_04.png)

    <span data-ttu-id="d7578-198">a.</span><span class="sxs-lookup"><span data-stu-id="d7578-198">a.</span></span> <span data-ttu-id="d7578-199">Az a **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d7578-199">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d7578-200">b.</span><span class="sxs-lookup"><span data-stu-id="d7578-200">b.</span></span> <span data-ttu-id="d7578-201">Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címe az Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d7578-201">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="d7578-202">c.</span><span class="sxs-lookup"><span data-stu-id="d7578-202">c.</span></span> <span data-ttu-id="d7578-203">Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="d7578-203">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="d7578-204">d.</span><span class="sxs-lookup"><span data-stu-id="d7578-204">d.</span></span> <span data-ttu-id="d7578-205">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d7578-205">Click **Create**.</span></span>
 
### <a name="create-a-citrix-sharefile-test-user"></a><span data-ttu-id="d7578-206">Citrix ShareFile tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d7578-206">Create a Citrix ShareFile test user</span></span>

<span data-ttu-id="d7578-207">Ahhoz, hogy az Azure AD-felhasználók jelentkezzen be a Citrix ShareFile, akkor ki kell építenie a Citrix ShareFile.</span><span class="sxs-lookup"><span data-stu-id="d7578-207">In order to enable Azure AD users to log into Citrix ShareFile, they must be provisioned into Citrix ShareFile.</span></span> <span data-ttu-id="d7578-208">Citrix ShareFile, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="d7578-208">In the case of Citrix ShareFile, provisioning is a manual task.</span></span>

<span data-ttu-id="d7578-209">**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d7578-209">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="d7578-210">Jelentkezzen be a **Citrix ShareFile** bérlő.</span><span class="sxs-lookup"><span data-stu-id="d7578-210">Log in to your **Citrix ShareFile** tenant.</span></span>

2. <span data-ttu-id="d7578-211">Kattintson a **felhasználók kezelése \> otthoni felhasználók kezelése \> + alkalmazott létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="d7578-211">Click **Manage Users \> Manage Users Home \> + Create Employee**.</span></span>
   
   <span data-ttu-id="d7578-212">![Alkalmazott létrehozása](./media/active-directory-saas-sharefile-tutorial/IC781050.png "alkalmazott létrehozása")</span><span class="sxs-lookup"><span data-stu-id="d7578-212">![Create Employee](./media/active-directory-saas-sharefile-tutorial/IC781050.png "Create Employee")</span></span>

3. <span data-ttu-id="d7578-213">Az a **alapvető információkat** csoportjában hajtsa végre a következő lépések:</span><span class="sxs-lookup"><span data-stu-id="d7578-213">On the **Basic Information** section, perform below steps:</span></span>
   
   <span data-ttu-id="d7578-214">![Alapvető információkat](./media/active-directory-saas-sharefile-tutorial/IC799951.png "alapvető információk")</span><span class="sxs-lookup"><span data-stu-id="d7578-214">![Basic Information](./media/active-directory-saas-sharefile-tutorial/IC799951.png "Basic Information")</span></span>
   
   <span data-ttu-id="d7578-215">a.</span><span class="sxs-lookup"><span data-stu-id="d7578-215">a.</span></span> <span data-ttu-id="d7578-216">Az a **E-mail cím** szövegmezőhöz Britta Simon, e-mail címét  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="d7578-216">In the **Email Address** textbox, type the email address of Britta Simon as **brittasimon@contoso.com**.</span></span>
   
   <span data-ttu-id="d7578-217">b.</span><span class="sxs-lookup"><span data-stu-id="d7578-217">b.</span></span> <span data-ttu-id="d7578-218">Az a **Utónév** szövegmezőhöz típus **Utónév** , felhasználó **Britta**.</span><span class="sxs-lookup"><span data-stu-id="d7578-218">In the **First Name** textbox, type **first name** of user as **Britta**.</span></span>
   
   <span data-ttu-id="d7578-219">c.</span><span class="sxs-lookup"><span data-stu-id="d7578-219">c.</span></span> <span data-ttu-id="d7578-220">Az a **Vezetéknév** szövegmezőhöz típus **Vezetéknév** , felhasználó **Simon**.</span><span class="sxs-lookup"><span data-stu-id="d7578-220">In the **Last Name** textbox, type **last name** of user as **Simon**.</span></span>

4. <span data-ttu-id="d7578-221">Kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="d7578-221">Click **Add User**.</span></span>
  
   >[!NOTE]
   ><span data-ttu-id="d7578-222">Az Azure AD fióktulajdonos fog egy e-maileket és hivatkozásra a fiók megerősítéséhez, mielőtt aktívvá válik. Citrix ShareFile felhasználói fiók létrehozása eszközök vagy Citrix ShareFile által nyújtott API-k segítségével kiépíteni az Azure AD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="d7578-222">The Azure AD account holder will receive an email and follow a link to confirm their account before it becomes active.You can use any other Citrix ShareFile user account creation tools or APIs provided by Citrix ShareFile to provision Azure AD user accounts.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="d7578-223">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="d7578-223">Assign the Azure AD test user</span></span>

<span data-ttu-id="d7578-224">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Citrix ShareFile Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="d7578-224">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Citrix ShareFile.</span></span>

![A felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="d7578-226">**Citrix ShareFile Britta Simon rendel, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d7578-226">**To assign Britta Simon to Citrix ShareFile, perform the following steps:**</span></span>

1. <span data-ttu-id="d7578-227">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d7578-227">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="d7578-229">Az alkalmazások listában válassza ki a **Citrix ShareFile**.</span><span class="sxs-lookup"><span data-stu-id="d7578-229">In the applications list, select **Citrix ShareFile**.</span></span>

    ![Az alkalmazások listáját a Citrix ShareFile hivatkozás](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_app.png)  

3. <span data-ttu-id="d7578-231">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="d7578-231">In the menu on the left, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202]

4. <span data-ttu-id="d7578-233">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d7578-233">Click **Add** button.</span></span> <span data-ttu-id="d7578-234">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d7578-234">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="d7578-236">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="d7578-236">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d7578-237">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d7578-237">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d7578-238">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d7578-238">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="d7578-239">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="d7578-239">Test single sign-on</span></span>

<span data-ttu-id="d7578-240">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="d7578-240">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="d7578-241">Ha a hozzáférési panelen a Citrix ShareFile csempére kattint, akkor kell beolvasása automatikusan bejelentkezett a Citrix ShareFile alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="d7578-241">When you click the Citrix ShareFile tile in the Access Panel, you should get automatically signed-on to your Citrix ShareFile application.</span></span>
<span data-ttu-id="d7578-242">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d7578-242">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="d7578-243">További források</span><span class="sxs-lookup"><span data-stu-id="d7578-243">Additional resources</span></span>

* [<span data-ttu-id="d7578-244">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="d7578-244">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d7578-245">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="d7578-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_203.png

