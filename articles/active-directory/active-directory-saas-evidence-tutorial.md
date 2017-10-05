---
title: "Oktatóanyag: Azure Active Directoryval integrált Evidence.com |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Evidence.com között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: f9a7cb7c-ff67-40dc-872c-1fa35f9dd03b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: a9c474cfc648fc8a306d736c89a4813d82c133ea
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-evidencecom"></a><span data-ttu-id="fe02c-103">Oktatóanyag: Azure Active Directoryval integrált Evidence.com</span><span class="sxs-lookup"><span data-stu-id="fe02c-103">Tutorial: Azure Active Directory integration with Evidence.com</span></span>

<span data-ttu-id="fe02c-104">Ebben az oktatóanyagban elsajátíthatja Evidence.com integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="fe02c-104">In this tutorial, you learn how to integrate Evidence.com with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="fe02c-105">Evidence.com integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="fe02c-105">Integrating Evidence.com with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="fe02c-106">Az Azure AD, aki hozzáfér Evidence.com szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="fe02c-106">You can control in Azure AD who has access to Evidence.com.</span></span>
- <span data-ttu-id="fe02c-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Evidence.com (egyszeri bejelentkezés) számára a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="fe02c-107">You can enable your users to automatically get signed-on to Evidence.com (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="fe02c-108">A fiók egyetlen központi helyen – az Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="fe02c-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="fe02c-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="fe02c-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fe02c-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fe02c-110">Prerequisites</span></span>

<span data-ttu-id="fe02c-111">Konfigurálása az Azure AD-integrációs Evidence.com, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="fe02c-111">To configure Azure AD integration with Evidence.com, you need the following items:</span></span>

- <span data-ttu-id="fe02c-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="fe02c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="fe02c-113">Egy Evidence.com egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="fe02c-113">A Evidence.com single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="fe02c-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="fe02c-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="fe02c-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="fe02c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="fe02c-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="fe02c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="fe02c-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fe02c-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="fe02c-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="fe02c-118">Scenario description</span></span>
<span data-ttu-id="fe02c-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="fe02c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="fe02c-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="fe02c-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="fe02c-121">A gyűjteményből Evidence.com hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fe02c-121">Adding Evidence.com from the gallery</span></span>
2. <span data-ttu-id="fe02c-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="fe02c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-evidencecom-from-the-gallery"></a><span data-ttu-id="fe02c-123">A gyűjteményből Evidence.com hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fe02c-123">Adding Evidence.com from the gallery</span></span>
<span data-ttu-id="fe02c-124">Az Azure AD integrálása a Evidence.com konfigurálásához kell hozzáadnia Evidence.com a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="fe02c-124">To configure the integration of Evidence.com into Azure AD, you need to add Evidence.com from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="fe02c-125">**A gyűjteményből Evidence.com hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="fe02c-125">**To add Evidence.com from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="fe02c-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="fe02c-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="fe02c-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="fe02c-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="fe02c-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="fe02c-129">Then go to **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="fe02c-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="fe02c-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="fe02c-133">Írja be a keresőmezőbe, **Evidence.com**, jelölje be **Evidence.com** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="fe02c-133">In the search box, type **Evidence.com**, select **Evidence.com** from result panel then click **Add** button to add the application.</span></span>

    ![Az eredménylistában Evidence.com](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="fe02c-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fe02c-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="fe02c-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Evidence.com.</span><span class="sxs-lookup"><span data-stu-id="fe02c-136">In this section, you configure and test Azure AD single sign-on with Evidence.com based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="fe02c-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Evidence.com a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="fe02c-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Evidence.com is to a user in Azure AD.</span></span> <span data-ttu-id="fe02c-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Evidence.com közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="fe02c-138">In other words, a link relationship between an Azure AD user and the related user in Evidence.com needs to be established.</span></span>

<span data-ttu-id="fe02c-139">Evidence.com, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="fe02c-139">In Evidence.com, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="fe02c-140">Az Azure AD egyszeri bejelentkezést a Evidence.com tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="fe02c-140">To configure and test Azure AD single sign-on with Evidence.com, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="fe02c-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="fe02c-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="fe02c-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="fe02c-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="fe02c-143">**[Evidence.com tesztfelhasználó létrehozása](#create-a-evidencecom-test-user)**  - való Britta Simon valami Evidence.com, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="fe02c-143">**[Create a Evidence.com test user](#create-a-evidencecom-test-user)** - to have a counterpart of Britta Simon in Evidence.com that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="fe02c-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="fe02c-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="fe02c-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="fe02c-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="fe02c-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fe02c-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="fe02c-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Evidence.com alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="fe02c-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Evidence.com application.</span></span>

<span data-ttu-id="fe02c-148">**Konfigurálása az Azure AD az egyszeri bejelentkezés Evidence.com, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="fe02c-148">**To configure Azure AD single sign-on with Evidence.com, perform the following steps:**</span></span>

1. <span data-ttu-id="fe02c-149">Az Azure portálon a a **Evidence.com** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="fe02c-149">In the Azure portal, on the **Evidence.com** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="fe02c-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="fe02c-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_samlbase.png)

3. <span data-ttu-id="fe02c-153">Az a **Evidence.com tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="fe02c-153">On the **Evidence.com Domain and URLs** section, perform the following steps:</span></span>

    ![Az egyszeri bejelentkezés információk Evidence.com tartomány és az URL-címek](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_url.png)

    <span data-ttu-id="fe02c-155">a.</span><span class="sxs-lookup"><span data-stu-id="fe02c-155">a.</span></span> <span data-ttu-id="fe02c-156">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<yourtenant>.evidence.com`</span><span class="sxs-lookup"><span data-stu-id="fe02c-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<yourtenant>.evidence.com`</span></span>

    <span data-ttu-id="fe02c-157">b.</span><span class="sxs-lookup"><span data-stu-id="fe02c-157">b.</span></span> <span data-ttu-id="fe02c-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<yourtenant>.evidence.com`</span><span class="sxs-lookup"><span data-stu-id="fe02c-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<yourtenant>.evidence.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="fe02c-159">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="fe02c-159">These values are not real.</span></span> <span data-ttu-id="fe02c-160">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="fe02c-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="fe02c-161">Ügyfél [Evidence.com ügyfél-támogatási csoport](https://communities.taser.com/support/SupportContactUs?typ=LE) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="fe02c-161">Contact [Evidence.com Client support team](https://communities.taser.com/support/SupportContactUs?typ=LE) to get these values.</span></span> 

4. <span data-ttu-id="fe02c-162">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="fe02c-162">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_certificate.png) 

5. <span data-ttu-id="fe02c-164">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="fe02c-164">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-evidence-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="fe02c-166">A a **Evidence.com konfigurációs** kattintson **konfigurálása Evidence.com** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="fe02c-166">On the **Evidence.com Configuration** section, click **Configure Evidence.com** to open **Configure sign-on** window.</span></span> <span data-ttu-id="fe02c-167">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="fe02c-167">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Evidence.com konfiguráció](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_configure.png) 

7. <span data-ttu-id="fe02c-169">Egy külön webkiszolgáló böngészőablakban, jelentkezzen be a Evidence.com bérlői rendszergazdaként, és keresse meg **Admin** lap</span><span class="sxs-lookup"><span data-stu-id="fe02c-169">In a separate web browser window, login to your Evidence.com tenant as an administrator and navigate to **Admin** Tab</span></span>

8. <span data-ttu-id="fe02c-170">Kattintson a **ügynökség az egyszeri bejelentkezés**</span><span class="sxs-lookup"><span data-stu-id="fe02c-170">Click on **Agency Single Sign On**</span></span>

9. <span data-ttu-id="fe02c-171">Válassza ki **SAML alapján egyszeri bejelentkezéshez**</span><span class="sxs-lookup"><span data-stu-id="fe02c-171">Select **SAML Based Single Sign On**</span></span>

10. <span data-ttu-id="fe02c-172">Másolás a **SAML Entitásazonosító**, **SAML-alapú egyszeri bejelentkezési URL-címe** és **Sign-Out URL-cím** Evidence.com megfelelő mezőiben és az Azure-portálon a megjelenő értékeket.</span><span class="sxs-lookup"><span data-stu-id="fe02c-172">Copy the **SAML Entity ID**, **SAML Single Sign-On Service URL** and **Sign-Out URL** values shown in the Azure portal and to the corresponding fields in Evidence.com.</span></span>

11. <span data-ttu-id="fe02c-173">Nyissa meg a letöltött Certificate(Base64) fájlt a Jegyzettömbben, annak tartalmának másolása a vágólapra és illessze be azt a **biztonsági tanúsítvány** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="fe02c-173">Open your downloaded Certificate(Base64) file in notepad, copy the content of it into your clipboard, and then paste it to the **Security Certificate** box.</span></span> 

12. <span data-ttu-id="fe02c-174">A konfiguráció mentése Evidence.com.</span><span class="sxs-lookup"><span data-stu-id="fe02c-174">Save the configuration in Evidence.com.</span></span>

> [!TIP]
> <span data-ttu-id="fe02c-175">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="fe02c-175">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="fe02c-176">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="fe02c-176">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="fe02c-177">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="fe02c-177">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="fe02c-178">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="fe02c-178">Create an Azure AD test user</span></span>

<span data-ttu-id="fe02c-179">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="fe02c-179">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="fe02c-181">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="fe02c-181">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="fe02c-182">Az Azure portálon a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="fe02c-182">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-evidence-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="fe02c-184">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="fe02c-184">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-evidence-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="fe02c-186">Megnyitásához a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** tetején a **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="fe02c-186">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![A Hozzáadás gombra.](./media/active-directory-saas-evidence-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="fe02c-188">Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="fe02c-188">In the **User** dialog box, perform the following steps:</span></span>

    ![A felhasználó párbeszédpanel](./media/active-directory-saas-evidence-tutorial/create_aaduser_04.png)

    <span data-ttu-id="fe02c-190">a.</span><span class="sxs-lookup"><span data-stu-id="fe02c-190">a.</span></span> <span data-ttu-id="fe02c-191">Az a **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="fe02c-191">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="fe02c-192">b.</span><span class="sxs-lookup"><span data-stu-id="fe02c-192">b.</span></span> <span data-ttu-id="fe02c-193">Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címe az Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="fe02c-193">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="fe02c-194">c.</span><span class="sxs-lookup"><span data-stu-id="fe02c-194">c.</span></span> <span data-ttu-id="fe02c-195">Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="fe02c-195">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="fe02c-196">d.</span><span class="sxs-lookup"><span data-stu-id="fe02c-196">d.</span></span> <span data-ttu-id="fe02c-197">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="fe02c-197">Click **Create**.</span></span>
 
### <a name="create-a-evidencecom-test-user"></a><span data-ttu-id="fe02c-198">Evidence.com tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="fe02c-198">Create a Evidence.com test user</span></span>

<span data-ttu-id="fe02c-199">Az Azure Active Directory-felhasználók tudjanak bejelentkezni azok kell kiosztani a Evidence.com alkalmazáson belüli hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="fe02c-199">For Azure AD users to be able to sign in, they must be provisioned for access inside the Evidence.com application.</span></span> <span data-ttu-id="fe02c-200">Ez a szakasz ismerteti, hogyan adhat hozzá az Azure AD felhasználói fiókokat Evidence.com belül</span><span class="sxs-lookup"><span data-stu-id="fe02c-200">This section describes how to create Azure AD user accounts inside Evidence.com</span></span>

<span data-ttu-id="fe02c-201">**Egy felhasználói fiókot az Evidence.com telepítéséhez:**</span><span class="sxs-lookup"><span data-stu-id="fe02c-201">**To provision a user account in Evidence.com:**</span></span>

1. <span data-ttu-id="fe02c-202">A webböngésző ablakának jelentkezzen be a Evidence.com vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="fe02c-202">In a web browser window, log into your Evidence.com company site as an administrator.</span></span>

2. <span data-ttu-id="fe02c-203">Navigáljon a **Admin** fülre.</span><span class="sxs-lookup"><span data-stu-id="fe02c-203">Navigate to **Admin** tab.</span></span>

3. <span data-ttu-id="fe02c-204">Kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="fe02c-204">Click on **Add User**.</span></span>

4. <span data-ttu-id="fe02c-205">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="fe02c-205">Click the **Add** button.</span></span>

5. <span data-ttu-id="fe02c-206">A **E-mail cím** hozzáadott felhasználó egyeznie kell a felhasználónévvel a felhasználók számára hozzáférést kíván az Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="fe02c-206">The **Email Address** of the added user must match the username of the users in Azure AD who you wish to give access.</span></span> <span data-ttu-id="fe02c-207">Ha a felhasználónév és e-mail cím nem ugyanazt az értéket a szervezetében, használhatja a **Evidence.com > attribútumok > egyszeri bejelentkezés** szakasza az Azure-portálon módosíthatja a nameidenitifer küldött Evidence.com, ha az e-mail cím lehet.</span><span class="sxs-lookup"><span data-stu-id="fe02c-207">If the username and email address are not the same value in your organization, you can use the **Evidence.com > Attributes > Single Sign-On** section of the Azure portal to change the nameidenitifer sent to Evidence.com to be the email address.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="fe02c-208">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="fe02c-208">Assign the Azure AD test user</span></span>

<span data-ttu-id="fe02c-209">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Evidence.com Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="fe02c-209">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Evidence.com.</span></span>

![A felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="fe02c-211">**Britta Simon hozzárendelése Evidence.com, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="fe02c-211">**To assign Britta Simon to Evidence.com, perform the following steps:**</span></span>

1. <span data-ttu-id="fe02c-212">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="fe02c-212">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="fe02c-214">Az alkalmazások listában válassza ki a **Evidence.com**.</span><span class="sxs-lookup"><span data-stu-id="fe02c-214">In the applications list, select **Evidence.com**.</span></span>

    ![Az alkalmazások listáját a Evidence.com hivatkozás](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_app.png)  

3. <span data-ttu-id="fe02c-216">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="fe02c-216">In the menu on the left, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202]

4. <span data-ttu-id="fe02c-218">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="fe02c-218">Click **Add** button.</span></span> <span data-ttu-id="fe02c-219">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fe02c-219">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="fe02c-221">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="fe02c-221">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="fe02c-222">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fe02c-222">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="fe02c-223">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fe02c-223">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="fe02c-224">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="fe02c-224">Test single sign-on</span></span>

<span data-ttu-id="fe02c-225">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="fe02c-225">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="fe02c-226">Ha a hozzáférési panelen Evidence.com csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Evidence.com alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="fe02c-226">When you click the Evidence.com tile in the Access Panel, you should get automatically signed-on to your Evidence.com application.</span></span>
<span data-ttu-id="fe02c-227">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="fe02c-227">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="fe02c-228">További források</span><span class="sxs-lookup"><span data-stu-id="fe02c-228">Additional resources</span></span>

* [<span data-ttu-id="fe02c-229">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="fe02c-229">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="fe02c-230">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="fe02c-230">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_203.png

