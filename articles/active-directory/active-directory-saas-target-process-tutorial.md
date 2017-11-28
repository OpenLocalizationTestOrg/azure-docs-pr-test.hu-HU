---
title: "Oktatóanyag: Azure Active Directoryval integrált TargetProcess |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és TargetProcess között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7cb91628-e758-480d-a233-7a3caaaff50d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: d15931a5d430252bbd9ae342e1f8fde1a539355b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-targetprocess"></a><span data-ttu-id="4c0eb-103">Oktatóanyag: Azure Active Directoryval integrált TargetProcess</span><span class="sxs-lookup"><span data-stu-id="4c0eb-103">Tutorial: Azure Active Directory integration with TargetProcess</span></span>

<span data-ttu-id="4c0eb-104">Ebben az oktatóanyagban elsajátíthatja TargetProcess integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="4c0eb-104">In this tutorial, you learn how to integrate TargetProcess with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4c0eb-105">TargetProcess integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="4c0eb-105">Integrating TargetProcess with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="4c0eb-106">Megadhatja a TargetProcess hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="4c0eb-106">You can control in Azure AD who has access to TargetProcess</span></span>
- <span data-ttu-id="4c0eb-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett TargetProcess (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="4c0eb-107">You can enable your users to automatically get signed-on to TargetProcess (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="4c0eb-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="4c0eb-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="4c0eb-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4c0eb-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4c0eb-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4c0eb-110">Prerequisites</span></span>

<span data-ttu-id="4c0eb-111">Konfigurálása az Azure AD-integrációs TargetProcess, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="4c0eb-111">To configure Azure AD integration with TargetProcess, you need the following items:</span></span>

- <span data-ttu-id="4c0eb-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="4c0eb-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4c0eb-113">Egy TargetProcess egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="4c0eb-113">A TargetProcess single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4c0eb-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4c0eb-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="4c0eb-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4c0eb-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4c0eb-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4c0eb-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4c0eb-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="4c0eb-118">Scenario description</span></span>
<span data-ttu-id="4c0eb-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4c0eb-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="4c0eb-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4c0eb-121">Adja hozzá a TargetProcess a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="4c0eb-121">Add TargetProcess from the gallery</span></span>
2. <span data-ttu-id="4c0eb-122">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4c0eb-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-targetprocess-from-the-gallery"></a><span data-ttu-id="4c0eb-123">Adja hozzá a TargetProcess a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="4c0eb-123">Add TargetProcess from the gallery</span></span>
<span data-ttu-id="4c0eb-124">Az Azure AD integrálása a TargetProcess konfigurálásához kell hozzáadnia TargetProcess a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-124">To configure the integration of TargetProcess into Azure AD, you need to add TargetProcess from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="4c0eb-125">**A gyűjteményből TargetProcess hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="4c0eb-125">**To add TargetProcess from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="4c0eb-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4c0eb-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="4c0eb-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="4c0eb-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="4c0eb-133">Írja be a keresőmezőbe, **TargetProcess**, jelölje be **TargetProcess** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-133">In the search box, type **TargetProcess**, select **TargetProcess**  from result panel then click **Add** button to add the application.</span></span>

    ![Hozzáadás TargetProcess gyűjteményből](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="4c0eb-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4c0eb-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="4c0eb-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján TargetProcess.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-136">In this section, you configure and test Azure AD single sign-on with TargetProcess based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="4c0eb-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó TargetProcess a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-137">For single sign-on to work, Azure AD needs to know what the counterpart user in TargetProcess is to a user in Azure AD.</span></span> <span data-ttu-id="4c0eb-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a TargetProcess közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-138">In other words, a link relationship between an Azure AD user and the related user in TargetProcess needs to be established.</span></span>

<span data-ttu-id="4c0eb-139">TargetProcess, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-139">In TargetProcess, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="4c0eb-140">Az Azure AD egyszeri bejelentkezést a TargetProcess tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="4c0eb-140">To configure and test Azure AD single sign-on with TargetProcess, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="4c0eb-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="4c0eb-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4c0eb-143">**[TargetProcess tesztfelhasználó létrehozása](#create-a-targetprocess-test-user)**  - való Britta Simon valami TargetProcess, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-143">**[Create a TargetProcess test user](#create-a-targetprocess-test-user)** - to have a counterpart of Britta Simon in TargetProcess that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="4c0eb-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4c0eb-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-145">**[Test Single Sign-On](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="4c0eb-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4c0eb-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="4c0eb-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az TargetProcess alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your TargetProcess application.</span></span>

<span data-ttu-id="4c0eb-148">**Konfigurálása az Azure AD az egyszeri bejelentkezés TargetProcess, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="4c0eb-148">**To configure Azure AD single sign-on with TargetProcess, perform the following steps:**</span></span>

1. <span data-ttu-id="4c0eb-149">Az Azure portálon a a **TargetProcess** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-149">In the Azure portal, on the **TargetProcess** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="4c0eb-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![SAML-alapú bejelentkezés](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_samlbase.png)

3. <span data-ttu-id="4c0eb-153">Az a **TargetProcess tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="4c0eb-153">On the **TargetProcess Domain and URLs** section, perform the following steps:</span></span>

    ![TargetProcess tartomány és az URL-címek szakasz](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_url.png)

    <span data-ttu-id="4c0eb-155">a.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-155">a.</span></span> <span data-ttu-id="4c0eb-156">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.tpondemand.com/`</span><span class="sxs-lookup"><span data-stu-id="4c0eb-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.tpondemand.com/`</span></span>

    <span data-ttu-id="4c0eb-157">b.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-157">b.</span></span> <span data-ttu-id="4c0eb-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.tpondemand.com/`</span><span class="sxs-lookup"><span data-stu-id="4c0eb-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.tpondemand.com/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="4c0eb-159">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-159">These values are not real.</span></span> <span data-ttu-id="4c0eb-160">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="4c0eb-161">Ügyfél [TargetProcess ügyfél-támogatási csoport](mailto:support@targetprocess.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-161">Contact [TargetProcess Client support team](mailto:support@targetprocess.com) to get these values.</span></span> 
 
4. <span data-ttu-id="4c0eb-162">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-162">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![SAML-aláíró tanúsítványa szakasz](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_certificate.png) 

5. <span data-ttu-id="4c0eb-164">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-164">Click **Save** button.</span></span>

    ![Mentés gombja](./media/active-directory-saas-target-process-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="4c0eb-166">A a **TargetProcess konfigurációs** kattintson **konfigurálása TargetProcess** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-166">On the **TargetProcess Configuration** section, click **Configure TargetProcess** to open **Configure sign-on** window.</span></span> <span data-ttu-id="4c0eb-167">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="4c0eb-167">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![TargetProcess konfigurációs szakasz](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_configure.png) 

7. <span data-ttu-id="4c0eb-169">Bejelentkezés a TargetProcess alkalmazást rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-169">Sign-on to your TargetProcess application as an administrator.</span></span>

8. <span data-ttu-id="4c0eb-170">Kattintson a felső menüben **telepítő**.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-170">In the menu on the top, click **Setup**.</span></span>
   
    ![Beállítás](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_05.png)

9. <span data-ttu-id="4c0eb-172">Kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-172">Click **Settings**.</span></span>
   
    ![Beállítások](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_06.png) 

10. <span data-ttu-id="4c0eb-174">Kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-174">Click **Single Sign-on**.</span></span>
   
    ![Kattintson az egyszeri bejelentkezést.](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_07.png) 

11. <span data-ttu-id="4c0eb-176">Az egyszeri bejelentkezés beállításai párbeszédpanel hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4c0eb-176">On the Single Sign-on settings dialog, perform the following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_08.png)
    
    <span data-ttu-id="4c0eb-178">a.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-178">a.</span></span> <span data-ttu-id="4c0eb-179">Kattintson a **egyszeri bejelentkezés engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-179">Click **Enable Single Sign-on**.</span></span>
    
    <span data-ttu-id="4c0eb-180">b.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-180">b.</span></span> <span data-ttu-id="4c0eb-181">A **bejelentkezési URL-cím** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-181">In **Sign-on URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="4c0eb-182">c.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-182">c.</span></span> <span data-ttu-id="4c0eb-183">A letöltött tanúsítvány megnyitása a Jegyzettömbben, másolja a tartalmat, és illessze be azt a **tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-183">Open your downloaded certificate in notepad, copy the content, and then paste it into the **Certificate** textbox.</span></span>
    
    <span data-ttu-id="4c0eb-184">d.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-184">d.</span></span> <span data-ttu-id="4c0eb-185">Kattintson a **JIT-kiépítés engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-185">click **Enable JIT Provisioning**.</span></span>

    <span data-ttu-id="4c0eb-186">e.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-186">e.</span></span> <span data-ttu-id="4c0eb-187">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-187">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="4c0eb-188">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="4c0eb-188">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="4c0eb-189">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-189">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="4c0eb-190">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4c0eb-190">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="4c0eb-191">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="4c0eb-191">Create an Azure AD test user</span></span>
<span data-ttu-id="4c0eb-192">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-192">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="4c0eb-194">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="4c0eb-194">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="4c0eb-195">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-195">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-target-process-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4c0eb-197">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-197">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Azon felhasználók listájának megjelenítéséhez](./media/active-directory-saas-target-process-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4c0eb-199">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-199">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Hozzáadás gomb](./media/active-directory-saas-target-process-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4c0eb-201">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="4c0eb-201">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Felhasználói szakasz](./media/active-directory-saas-target-process-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4c0eb-203">a.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-203">a.</span></span> <span data-ttu-id="4c0eb-204">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-204">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4c0eb-205">b.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-205">b.</span></span> <span data-ttu-id="4c0eb-206">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-206">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="4c0eb-207">c.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-207">c.</span></span> <span data-ttu-id="4c0eb-208">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-208">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="4c0eb-209">d.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-209">d.</span></span> <span data-ttu-id="4c0eb-210">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-210">Click **Create**.</span></span>
 
### <a name="create-a-targetprocess-test-user"></a><span data-ttu-id="4c0eb-211">TargetProcess tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="4c0eb-211">Create a TargetProcess test user</span></span>

<span data-ttu-id="4c0eb-212">Ez a szakasz célja TargetProcess Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-212">The objective of this section is to create a user called Britta Simon in TargetProcess.</span></span>

<span data-ttu-id="4c0eb-213">TargetProcess támogatja közvetlenül az időponthoz kötött kiosztást.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-213">TargetProcess supports just-in-time provisioning.</span></span> <span data-ttu-id="4c0eb-214">Már engedélyezett legyen [az Azure AD konfigurálása egyszeri bejelentkezéshez](#configuring-azure-ad-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="4c0eb-214">You have already enabled it in [Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on).</span></span>

<span data-ttu-id="4c0eb-215">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-215">There is no action item for you in this section.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="4c0eb-216">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="4c0eb-216">Assign the Azure AD test user</span></span>

<span data-ttu-id="4c0eb-217">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés TargetProcess Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-217">In this section, you enable Britta Simon to use Azure single sign-on by granting access to TargetProcess.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="4c0eb-219">**Britta Simon hozzárendelése TargetProcess, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="4c0eb-219">**To assign Britta Simon to TargetProcess, perform the following steps:**</span></span>

1. <span data-ttu-id="4c0eb-220">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-220">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="4c0eb-222">Az alkalmazások listában válassza ki a **TargetProcess**.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-222">In the applications list, select **TargetProcess**.</span></span>

    ![TargetProcess alkalmazáslistában](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_app.png) 

3. <span data-ttu-id="4c0eb-224">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-224">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="4c0eb-226">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-226">Click **Add** button.</span></span> <span data-ttu-id="4c0eb-227">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="4c0eb-229">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-229">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="4c0eb-230">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4c0eb-231">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="4c0eb-232">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="4c0eb-232">Test single sign-on</span></span>

<span data-ttu-id="4c0eb-233">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-233">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="4c0eb-234">Ha a hozzáférési panelen TargetProcess csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az TargetProcess alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="4c0eb-234">When you click the TargetProcess tile in the Access Panel, you should get automatically signed-on to your TargetProcess application.</span></span> <span data-ttu-id="4c0eb-235">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="4c0eb-235">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4c0eb-236">További források</span><span class="sxs-lookup"><span data-stu-id="4c0eb-236">Additional resources</span></span>

* [<span data-ttu-id="4c0eb-237">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="4c0eb-237">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4c0eb-238">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="4c0eb-238">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_203.png

