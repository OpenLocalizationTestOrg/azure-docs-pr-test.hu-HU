---
title: "Oktatóanyag: Azure Active Directoryval integrált Zoho |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Zoho között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 9874e1f3-ade5-42e7-a700-e08b3731236a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: jeedes
ms.openlocfilehash: f0688cb75584ada805b944d2ef5409d66ab37339
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zoho"></a><span data-ttu-id="15d5d-103">Oktatóanyag: Azure Active Directoryval integrált Zoho</span><span class="sxs-lookup"><span data-stu-id="15d5d-103">Tutorial: Azure Active Directory integration with Zoho</span></span>

<span data-ttu-id="15d5d-104">Ebben az oktatóanyagban elsajátíthatja Zoho integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="15d5d-104">In this tutorial, you learn how to integrate Zoho with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="15d5d-105">Zoho integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="15d5d-105">Integrating Zoho with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="15d5d-106">Az Azure AD, aki hozzáfér Zoho szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="15d5d-106">You can control in Azure AD who has access to Zoho.</span></span>
- <span data-ttu-id="15d5d-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Zoho (egyszeri bejelentkezés) számára a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="15d5d-107">You can enable your users to automatically get signed-on to Zoho (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="15d5d-108">A fiók egyetlen központi helyen – az Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="15d5d-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="15d5d-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="15d5d-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="15d5d-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="15d5d-110">Prerequisites</span></span>

<span data-ttu-id="15d5d-111">Konfigurálása az Azure AD-integrációs Zoho, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="15d5d-111">To configure Azure AD integration with Zoho, you need the following items:</span></span>

- <span data-ttu-id="15d5d-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="15d5d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="15d5d-113">Egy Zoho egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="15d5d-113">A Zoho single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="15d5d-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="15d5d-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="15d5d-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="15d5d-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="15d5d-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="15d5d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="15d5d-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="15d5d-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="15d5d-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="15d5d-118">Scenario description</span></span>
<span data-ttu-id="15d5d-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="15d5d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="15d5d-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="15d5d-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="15d5d-121">A gyűjteményből Zoho hozzáadása</span><span class="sxs-lookup"><span data-stu-id="15d5d-121">Adding Zoho from the gallery</span></span>
2. <span data-ttu-id="15d5d-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="15d5d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-zoho-from-the-gallery"></a><span data-ttu-id="15d5d-123">A gyűjteményből Zoho hozzáadása</span><span class="sxs-lookup"><span data-stu-id="15d5d-123">Adding Zoho from the gallery</span></span>
<span data-ttu-id="15d5d-124">Az Azure AD integrálása a Zoho konfigurálásához kell hozzáadnia Zoho a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="15d5d-124">To configure the integration of Zoho into Azure AD, you need to add Zoho from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="15d5d-125">**A gyűjteményből Zoho hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="15d5d-125">**To add Zoho from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="15d5d-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="15d5d-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="15d5d-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="15d5d-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="15d5d-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="15d5d-129">Then go to **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="15d5d-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="15d5d-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="15d5d-133">Írja be a keresőmezőbe, **Zoho**, jelölje be **Zoho** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="15d5d-133">In the search box, type **Zoho**, select **Zoho** from result panel then click **Add** button to add the application.</span></span>

    ![Az eredménylistában Zoho](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="15d5d-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="15d5d-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="15d5d-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Zoho.</span><span class="sxs-lookup"><span data-stu-id="15d5d-136">In this section, you configure and test Azure AD single sign-on with Zoho based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="15d5d-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Zoho a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="15d5d-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Zoho is to a user in Azure AD.</span></span> <span data-ttu-id="15d5d-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Zoho közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="15d5d-138">In other words, a link relationship between an Azure AD user and the related user in Zoho needs to be established.</span></span>

<span data-ttu-id="15d5d-139">Zoho, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="15d5d-139">In Zoho, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="15d5d-140">Az Azure AD egyszeri bejelentkezést a Zoho tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="15d5d-140">To configure and test Azure AD single sign-on with Zoho, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="15d5d-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="15d5d-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="15d5d-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="15d5d-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="15d5d-143">**[Zoho tesztfelhasználó létrehozása](#create-a-zoho-test-user)**  - való Britta Simon valami Zoho, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="15d5d-143">**[Create a Zoho test user](#create-a-zoho-test-user)** - to have a counterpart of Britta Simon in Zoho that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="15d5d-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="15d5d-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="15d5d-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="15d5d-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="15d5d-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="15d5d-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="15d5d-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Zoho alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="15d5d-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Zoho application.</span></span>

<span data-ttu-id="15d5d-148">**Konfigurálása az Azure AD az egyszeri bejelentkezés Zoho, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="15d5d-148">**To configure Azure AD single sign-on with Zoho, perform the following steps:**</span></span>

1. <span data-ttu-id="15d5d-149">Az Azure portálon a a **Zoho** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="15d5d-149">In the Azure portal, on the **Zoho** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="15d5d-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="15d5d-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_samlbase.png)

3. <span data-ttu-id="15d5d-153">Az a **Zoho tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="15d5d-153">On the **Zoho Domain and URLs** section, perform the following steps:</span></span>

    ![Az egyszeri bejelentkezés információk Zoho tartomány és az URL-címek](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_url.png)

    <span data-ttu-id="15d5d-155">a.</span><span class="sxs-lookup"><span data-stu-id="15d5d-155">a.</span></span> <span data-ttu-id="15d5d-156">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company name>.zohomail.com`</span><span class="sxs-lookup"><span data-stu-id="15d5d-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company name>.zohomail.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="15d5d-157">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="15d5d-157">This value is not real.</span></span> <span data-ttu-id="15d5d-158">Frissítse ezt az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="15d5d-158">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="15d5d-159">Ügyfél [Zoho ügyfél-támogatási csoport](https://www.zoho.com/mail/contact.html) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="15d5d-159">Contact [Zoho Client support team](https://www.zoho.com/mail/contact.html) to get this value.</span></span> 
 
4. <span data-ttu-id="15d5d-160">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="15d5d-160">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_certificate.png) 

5. <span data-ttu-id="15d5d-162">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="15d5d-162">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="15d5d-164">A a **Zoho konfigurációs** kattintson **konfigurálása Zoho** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="15d5d-164">On the **Zoho Configuration** section, click **Configure Zoho** to open **Configure sign-on** window.</span></span> <span data-ttu-id="15d5d-165">Másolás a **Sign-Out URL-cím, jelszó URL-cím módosítása és SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="15d5d-165">Copy the **Sign-Out URL, Change Password URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Zoho konfiguráció](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_configure.png) 

7. <span data-ttu-id="15d5d-167">Egy másik webes böngészőablakban jelentkezzen be a Zoho Mail vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="15d5d-167">In a different web browser window, log into your Zoho Mail company site as an administrator.</span></span>

8. <span data-ttu-id="15d5d-168">Lépjen a **Vezérlőpult**.</span><span class="sxs-lookup"><span data-stu-id="15d5d-168">Go to the **Control panel**.</span></span>
   
    <span data-ttu-id="15d5d-169">![Vezérlőpult](./media/active-directory-saas-zoho-mail-tutorial/ic789607.png "vezérlőpultot")</span><span class="sxs-lookup"><span data-stu-id="15d5d-169">![Control Panel](./media/active-directory-saas-zoho-mail-tutorial/ic789607.png "Control Panel")</span></span>

9. <span data-ttu-id="15d5d-170">Kattintson a **SAML-alapú hitelesítés** fülre.</span><span class="sxs-lookup"><span data-stu-id="15d5d-170">Click the **SAML Authentication** tab.</span></span>
   
    <span data-ttu-id="15d5d-171">![SAML-alapú hitelesítés](./media/active-directory-saas-zoho-mail-tutorial/ic789608.png "SAML-alapú hitelesítés")</span><span class="sxs-lookup"><span data-stu-id="15d5d-171">![SAML Authentication](./media/active-directory-saas-zoho-mail-tutorial/ic789608.png "SAML Authentication")</span></span>

10. <span data-ttu-id="15d5d-172">Az a **SAML-alapú hitelesítés részletei** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="15d5d-172">In the **SAML Authentication Details** section, perform the following steps:</span></span>
   
    <span data-ttu-id="15d5d-173">![SAML-alapú hitelesítés részletei](./media/active-directory-saas-zoho-mail-tutorial/ic789609.png "SAML-alapú hitelesítés részletei")</span><span class="sxs-lookup"><span data-stu-id="15d5d-173">![SAML Authentication Details](./media/active-directory-saas-zoho-mail-tutorial/ic789609.png "SAML Authentication Details")</span></span>
   
    <span data-ttu-id="15d5d-174">a.</span><span class="sxs-lookup"><span data-stu-id="15d5d-174">a.</span></span> <span data-ttu-id="15d5d-175">Az a **bejelentkezési URL-címe** szövegmezőhöz Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="15d5d-175">In the **Login URL** textbox, paste **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="15d5d-176">b.</span><span class="sxs-lookup"><span data-stu-id="15d5d-176">b.</span></span> <span data-ttu-id="15d5d-177">Az a **kijelentkezési URL-cím** szövegmező, Beillesztés **Sign-Out URL-cím** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="15d5d-177">In the **Logout URL** textbox, paste **Sign-Out URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="15d5d-178">c.</span><span class="sxs-lookup"><span data-stu-id="15d5d-178">c.</span></span> <span data-ttu-id="15d5d-179">Az a **jelszó URL-cím módosítása** szövegmezőhöz Beillesztés **jelszó URL-cím módosítása** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="15d5d-179">In the **Change Password URL** textbox, paste **Change Password URL** which you have copied from Azure portal.</span></span>
       
    <span data-ttu-id="15d5d-180">d.</span><span class="sxs-lookup"><span data-stu-id="15d5d-180">d.</span></span> <span data-ttu-id="15d5d-181">Nyissa meg a base-64 kódolású tanúsítvány a Jegyzettömbben az Azure portálról letöltött, annak tartalmának másolása a vágólapra és illessze be azt a **PublicKey** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="15d5d-181">Open your base-64 encoded certificate downloaded from Azure portal in notepad, copy the content of it into your clipboard, and then paste it to the **PublicKey** textbox.</span></span>
   
    <span data-ttu-id="15d5d-182">e.</span><span class="sxs-lookup"><span data-stu-id="15d5d-182">e.</span></span> <span data-ttu-id="15d5d-183">Mint **algoritmus**, jelölje be **RSA**.</span><span class="sxs-lookup"><span data-stu-id="15d5d-183">As **Algorithm**, select **RSA**.</span></span>
   
    <span data-ttu-id="15d5d-184">f.</span><span class="sxs-lookup"><span data-stu-id="15d5d-184">f.</span></span> <span data-ttu-id="15d5d-185">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="15d5d-185">Click **OK**.</span></span>

> [!TIP]
> <span data-ttu-id="15d5d-186">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="15d5d-186">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="15d5d-187">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="15d5d-187">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="15d5d-188">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="15d5d-188">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="15d5d-189">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="15d5d-189">Create an Azure AD test user</span></span>

<span data-ttu-id="15d5d-190">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="15d5d-190">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="15d5d-192">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="15d5d-192">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="15d5d-193">Az Azure portálon a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="15d5d-193">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="15d5d-195">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="15d5d-195">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="15d5d-197">Megnyitásához a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** tetején a **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="15d5d-197">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![A Hozzáadás gombra.](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="15d5d-199">Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="15d5d-199">In the **User** dialog box, perform the following steps:</span></span>

    ![A felhasználó párbeszédpanel](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_04.png)

    <span data-ttu-id="15d5d-201">a.</span><span class="sxs-lookup"><span data-stu-id="15d5d-201">a.</span></span> <span data-ttu-id="15d5d-202">Az a **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="15d5d-202">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="15d5d-203">b.</span><span class="sxs-lookup"><span data-stu-id="15d5d-203">b.</span></span> <span data-ttu-id="15d5d-204">Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címe az Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="15d5d-204">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="15d5d-205">c.</span><span class="sxs-lookup"><span data-stu-id="15d5d-205">c.</span></span> <span data-ttu-id="15d5d-206">Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="15d5d-206">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="15d5d-207">d.</span><span class="sxs-lookup"><span data-stu-id="15d5d-207">d.</span></span> <span data-ttu-id="15d5d-208">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="15d5d-208">Click **Create**.</span></span>
 
### <a name="create-a-zoho-test-user"></a><span data-ttu-id="15d5d-209">Zoho tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="15d5d-209">Create a Zoho test user</span></span>

<span data-ttu-id="15d5d-210">Ahhoz, hogy az Azure AD-felhasználók Zoho Mail bejelentkezni, akkor ki kell építenie Zoho Mail be.</span><span class="sxs-lookup"><span data-stu-id="15d5d-210">In order to enable Azure AD users to log into Zoho Mail, they must be provisioned into Zoho Mail.</span></span> <span data-ttu-id="15d5d-211">Zoho levelek, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="15d5d-211">In the case of Zoho Mail, provisioning is a manual task.</span></span>

> [!NOTE]
> <span data-ttu-id="15d5d-212">Bármely más Zoho Mail felhasználói fiók létrehozása eszközök, vagy rendelkezés AAD felhasználói fiókokhoz Zoho Mail által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="15d5d-212">You can use any other Zoho Mail user account creation tools or APIs provided by Zoho Mail to provision AAD user accounts.</span></span>

### <a name="to-provision-a-user-account-perform-the-following-steps"></a><span data-ttu-id="15d5d-213">Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="15d5d-213">To provision a user account, perform the following steps:</span></span>

1. <span data-ttu-id="15d5d-214">Jelentkezzen be a **Zoho Mail** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="15d5d-214">Log in to your **Zoho Mail** company site as an administrator.</span></span>

2. <span data-ttu-id="15d5d-215">Ugrás a **vezérlőpultot \> Mail & Docs**.</span><span class="sxs-lookup"><span data-stu-id="15d5d-215">Go to **Control Panel \> Mail & Docs**.</span></span>

3. <span data-ttu-id="15d5d-216">Ugrás a **felhasználó adatait \> felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="15d5d-216">Go to **User Details \> Add User**.</span></span>
   
    <span data-ttu-id="15d5d-217">![Felhasználó hozzáadása](./media/active-directory-saas-zoho-mail-tutorial/ic789611.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="15d5d-217">![Add User](./media/active-directory-saas-zoho-mail-tutorial/ic789611.png "Add User")</span></span>

4. <span data-ttu-id="15d5d-218">Az a **felhasználók hozzáadása az** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="15d5d-218">On the **Add users** dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="15d5d-219">![Felhasználó hozzáadása](./media/active-directory-saas-zoho-mail-tutorial/ic789612.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="15d5d-219">![Add User](./media/active-directory-saas-zoho-mail-tutorial/ic789612.png "Add User")</span></span>
   
    <span data-ttu-id="15d5d-220">a.</span><span class="sxs-lookup"><span data-stu-id="15d5d-220">a.</span></span> <span data-ttu-id="15d5d-221">Az a **Utónév** szövegmező, a felhasználó utónevét típusát, például **Britta**.</span><span class="sxs-lookup"><span data-stu-id="15d5d-221">In the **First Name** textbox, type the first name of user like **Britta**.</span></span>

    <span data-ttu-id="15d5d-222">b.</span><span class="sxs-lookup"><span data-stu-id="15d5d-222">b.</span></span> <span data-ttu-id="15d5d-223">Az a **Vezetéknév** szövegmező, a felhasználó vezetékneve típusát, például **Simon**.</span><span class="sxs-lookup"><span data-stu-id="15d5d-223">In the **Last Name** textbox, type the last name of user like **Simon**.</span></span>

    <span data-ttu-id="15d5d-224">c.</span><span class="sxs-lookup"><span data-stu-id="15d5d-224">c.</span></span> <span data-ttu-id="15d5d-225">Az a **E-mail azonosító** szövegmező, a felhasználó az e-mailek azonosító típusát, például  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="15d5d-225">In the **Email ID** textbox, type the email id of user like **brittasimon@contoso.com**.</span></span>

    <span data-ttu-id="15d5d-226">d.</span><span class="sxs-lookup"><span data-stu-id="15d5d-226">d.</span></span> <span data-ttu-id="15d5d-227">Az a **jelszó** szövegmező, adja meg a felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="15d5d-227">In the **Password** textbox, enter password of user.</span></span>
   
    <span data-ttu-id="15d5d-228">e.</span><span class="sxs-lookup"><span data-stu-id="15d5d-228">e.</span></span> <span data-ttu-id="15d5d-229">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="15d5d-229">Click **OK**.</span></span>  
      
    > [!NOTE]
    > <span data-ttu-id="15d5d-230">Az Azure Active Directory fióktulajdonos kap egy e-mailt, egy hivatkozás, mielőtt aktívvá válik, győződjön meg arról, hogy a fiók.</span><span class="sxs-lookup"><span data-stu-id="15d5d-230">The Azure Active Directory account holder will receive an email with a link to confirm the account before it becomes active.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="15d5d-231">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="15d5d-231">Assign the Azure AD test user</span></span>

<span data-ttu-id="15d5d-232">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Zoho Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="15d5d-232">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Zoho.</span></span>

![A felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="15d5d-234">**Britta Simon hozzárendelése Zoho, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="15d5d-234">**To assign Britta Simon to Zoho, perform the following steps:**</span></span>

1. <span data-ttu-id="15d5d-235">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="15d5d-235">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="15d5d-237">Az alkalmazások listában válassza ki a **Zoho**.</span><span class="sxs-lookup"><span data-stu-id="15d5d-237">In the applications list, select **Zoho**.</span></span>

    ![Az alkalmazások listáját a Zoho hivatkozás](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_app.png)  

3. <span data-ttu-id="15d5d-239">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="15d5d-239">In the menu on the left, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202]

4. <span data-ttu-id="15d5d-241">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="15d5d-241">Click **Add** button.</span></span> <span data-ttu-id="15d5d-242">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="15d5d-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="15d5d-244">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="15d5d-244">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="15d5d-245">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="15d5d-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="15d5d-246">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="15d5d-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="15d5d-247">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="15d5d-247">Test single sign-on</span></span>

<span data-ttu-id="15d5d-248">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="15d5d-248">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="15d5d-249">Ha a hozzáférési panelen Zoho csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Zoho alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="15d5d-249">When you click the Zoho tile in the Access Panel, you should get automatically signed-on to your Zoho application.</span></span>
<span data-ttu-id="15d5d-250">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="15d5d-250">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="15d5d-251">További források</span><span class="sxs-lookup"><span data-stu-id="15d5d-251">Additional resources</span></span>

* [<span data-ttu-id="15d5d-252">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="15d5d-252">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="15d5d-253">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="15d5d-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_203.png

