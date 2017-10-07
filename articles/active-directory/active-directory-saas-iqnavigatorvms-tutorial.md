---
title: "Oktatóanyag: Azure Active Directory-integráció IQNavigator virtuális gépek |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és IQNavigator virtuális gépek között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a8a09b25-dfa5-4c31-aea2-53bf1853b365
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: 5a5a7dd3f427acfba2f0ae10552a7179db730118
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-iqnavigator-vms"></a><span data-ttu-id="13ed2-103">Oktatóanyag: Azure Active Directory-integráció IQNavigator virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="13ed2-103">Tutorial: Azure Active Directory integration with IQNavigator VMS</span></span>

<span data-ttu-id="13ed2-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate IQNavigator virtuális Gépeket az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="13ed2-104">In this tutorial, you learn how toointegrate IQNavigator VMS with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="13ed2-105">IQNavigator virtuális gépek integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="13ed2-105">Integrating IQNavigator VMS with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="13ed2-106">Megadhatja a virtuális gépek hozzáférési tooIQNavigator rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="13ed2-106">You can control in Azure AD who has access tooIQNavigator VMS</span></span>
- <span data-ttu-id="13ed2-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooIQNavigator (egyszeri bejelentkezés) virtuális Gépeket az Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="13ed2-107">You can enable your users tooautomatically get signed-on tooIQNavigator VMS (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="13ed2-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="13ed2-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="13ed2-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="13ed2-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="13ed2-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="13ed2-110">Prerequisites</span></span>

<span data-ttu-id="13ed2-111">tooconfigure IQNavigator virtuális Gépeket az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="13ed2-111">tooconfigure Azure AD integration with IQNavigator VMS, you need hello following items:</span></span>

- <span data-ttu-id="13ed2-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="13ed2-112">An Azure AD subscription</span></span>
- <span data-ttu-id="13ed2-113">Egy IQNavigator virtuális gépek egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="13ed2-113">A IQNavigator VMS single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="13ed2-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="13ed2-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="13ed2-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="13ed2-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="13ed2-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="13ed2-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="13ed2-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="13ed2-117">If you don't have an Azure AD trial environment, you can get a one-month trial here [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="13ed2-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="13ed2-118">Scenario description</span></span>
<span data-ttu-id="13ed2-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="13ed2-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="13ed2-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="13ed2-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="13ed2-121">Hello gyűjteményből IQNavigator virtuális gépek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="13ed2-121">Adding IQNavigator VMS from hello gallery</span></span>
2. <span data-ttu-id="13ed2-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="13ed2-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-iqnavigator-vms-from-hello-gallery"></a><span data-ttu-id="13ed2-123">Hello gyűjteményből IQNavigator virtuális gépek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="13ed2-123">Adding IQNavigator VMS from hello gallery</span></span>
<span data-ttu-id="13ed2-124">tooconfigure hello integrációs IQNavigator virtuális gépek az Azure AD-be, meg kell tooadd IQNavigator virtuális gépek hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="13ed2-124">tooconfigure hello integration of IQNavigator VMS into Azure AD, you need tooadd IQNavigator VMS from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="13ed2-125">**tooadd IQNavigator virtuális gépek hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="13ed2-125">**tooadd IQNavigator VMS from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="13ed2-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="13ed2-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="13ed2-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="13ed2-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="13ed2-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="13ed2-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="13ed2-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="13ed2-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="13ed2-133">Hello keresési mezőbe, írja be a **IQNavigator virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="13ed2-133">In hello search box, type **IQNavigator VMS**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_search.png)

5. <span data-ttu-id="13ed2-135">A hello eredmények panelen válassza ki a **IQNavigator virtuális gépek**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="13ed2-135">In hello results panel, select **IQNavigator VMS**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="13ed2-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="13ed2-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="13ed2-138">Ebben a szakaszban konfigurálhatja, és IQNavigator virtuális gépek az Azure AD az egyszeri bejelentkezés tesztelése "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="13ed2-138">In this section, you configure and test Azure AD single sign-on with IQNavigator VMS based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="13ed2-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói IQNavigator virtuális gépeken tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="13ed2-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in IQNavigator VMS is tooa user in Azure AD.</span></span> <span data-ttu-id="13ed2-140">Ez azt jelenti hello kapcsolódó felhasználói IQNavigator virtuális gépeken és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="13ed2-140">In other words, a link relationship between an Azure AD user and hello related user in IQNavigator VMS needs toobe established.</span></span>

<span data-ttu-id="13ed2-141">Az IQNavigator virtuális Gépeken, rendelje a hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="13ed2-141">In IQNavigator VMS, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="13ed2-142">tooconfigure és IQNavigator virtuális gépek az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="13ed2-142">tooconfigure and test Azure AD single sign-on with IQNavigator VMS, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="13ed2-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="13ed2-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="13ed2-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="13ed2-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="13ed2-145">**[IQNavigator virtuális gépek tesztfelhasználó létrehozása](#creating-a-iqnavigator-vms-test-user)**  -toohave egy megfelelője a Britta Simon IQNavigator virtuális gépeken, amelyek felhasználói csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="13ed2-145">**[Creating a IQNavigator VMS test user](#creating-a-iqnavigator-vms-test-user)** - toohave a counterpart of Britta Simon in IQNavigator VMS that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="13ed2-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="13ed2-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="13ed2-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="13ed2-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="13ed2-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="13ed2-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="13ed2-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az IQNavigator virtuális gépek alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="13ed2-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your IQNavigator VMS application.</span></span>

<span data-ttu-id="13ed2-150">**az Azure AD tooconfigure egyszeri bejelentkezés IQNavigator virtuális gépek, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="13ed2-150">**tooconfigure Azure AD single sign-on with IQNavigator VMS, perform hello following steps:**</span></span>

1. <span data-ttu-id="13ed2-151">Az Azure portál, a hello hello **IQNavigator virtuális gépek** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="13ed2-151">In hello Azure portal, on hello **IQNavigator VMS** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="13ed2-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="13ed2-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_samlbase.png)

3. <span data-ttu-id="13ed2-155">A hello **IQNavigator virtuális gépek tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="13ed2-155">On hello **IQNavigator VMS Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_url.png)

    <span data-ttu-id="13ed2-157">a.</span><span class="sxs-lookup"><span data-stu-id="13ed2-157">a.</span></span> <span data-ttu-id="13ed2-158">A hello **azonosító** szövegmezőhöz típus hello URL-címe:`iqn.com`</span><span class="sxs-lookup"><span data-stu-id="13ed2-158">In hello **Identifier** textbox, type hello URL:`iqn.com`</span></span>

    <span data-ttu-id="13ed2-159">b.</span><span class="sxs-lookup"><span data-stu-id="13ed2-159">b.</span></span> <span data-ttu-id="13ed2-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.iqnavigator.com/security/login?client_name=https://sts.window.net/<instance name>`</span><span class="sxs-lookup"><span data-stu-id="13ed2-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<subdomain>.iqnavigator.com/security/login?client_name=https://sts.window.net/<instance name>`</span></span>

4. <span data-ttu-id="13ed2-161">Ellenőrizze **megjelenítése speciális URL-beállításainak**, hajtsa végre a következő lépés hello:</span><span class="sxs-lookup"><span data-stu-id="13ed2-161">Check **Show advanced URL settings**, perform hello following step:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_url1.png)

    <span data-ttu-id="13ed2-163">A hello **állapot továbbítása** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.iqnavigator.com`</span><span class="sxs-lookup"><span data-stu-id="13ed2-163">In hello **Relay state** textbox, type a URL using hello following pattern:`https://<subdomain>.iqnavigator.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="13ed2-164">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="13ed2-164">These values are not real.</span></span> <span data-ttu-id="13ed2-165">Frissítheti ezeket az értékeket hello tényleges válasz URL-CÍMEN és továbbító állapottal.</span><span class="sxs-lookup"><span data-stu-id="13ed2-165">Update these values with hello actual Reply URL and Relay state.</span></span> <span data-ttu-id="13ed2-166">Ügyfél [IQNavigator virtuális gépek ügyfél-támogatási csoport](https://www.beeline.com/iqn-product-support/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="13ed2-166">Contact [IQNavigator VMS Client support team](https://www.beeline.com/iqn-product-support/) tooget these values.</span></span> 

5. <span data-ttu-id="13ed2-167">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="13ed2-167">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="13ed2-169">toogenerate hello **metaadatok** URL-címe, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="13ed2-169">toogenerate hello **Metadata** url, perform hello following steps:</span></span>

    <span data-ttu-id="13ed2-170">a.</span><span class="sxs-lookup"><span data-stu-id="13ed2-170">a.</span></span> <span data-ttu-id="13ed2-171">Kattintson a **App regisztrációk**.</span><span class="sxs-lookup"><span data-stu-id="13ed2-171">Click **App registrations**.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_appregistrations.png)
   
    <span data-ttu-id="13ed2-173">b.</span><span class="sxs-lookup"><span data-stu-id="13ed2-173">b.</span></span> <span data-ttu-id="13ed2-174">Kattintson a **végpontok** tooopen **végpontok** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="13ed2-174">Click **Endpoints** tooopen **Endpoints** dialog box.</span></span>  
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_endpointicon.png)

    <span data-ttu-id="13ed2-176">c.</span><span class="sxs-lookup"><span data-stu-id="13ed2-176">c.</span></span> <span data-ttu-id="13ed2-177">Kattintson a hello Másolás gombra toocopy **ÖSSZEVONÁSI METAADAT-dokumentum** URL-cím és illessze be a Jegyzettömbbe.</span><span class="sxs-lookup"><span data-stu-id="13ed2-177">Click hello copy button toocopy **FEDERATION METADATA DOCUMENT** url and paste it into notepad.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_endpoint.png)
     
    <span data-ttu-id="13ed2-179">d.</span><span class="sxs-lookup"><span data-stu-id="13ed2-179">d.</span></span> <span data-ttu-id="13ed2-180">Most lépjen tulajdonságlapján toohello **IQNavigator virtuális gépek** és másolási hello **alkalmazásazonosító** használatával **másolási** gombra, majd illessze be a Jegyzettömbbe.</span><span class="sxs-lookup"><span data-stu-id="13ed2-180">Now go toohello property page of **IQNavigator VMS** and copy hello **Application Id** using **Copy** button and paste it into notepad.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_appid.png)

    <span data-ttu-id="13ed2-182">e.</span><span class="sxs-lookup"><span data-stu-id="13ed2-182">e.</span></span> <span data-ttu-id="13ed2-183">Hello készítése **metaadatainak URL-CÍMÉT** hello mintát a következő használatával:`<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span><span class="sxs-lookup"><span data-stu-id="13ed2-183">Generate hello **Metadata URL** using hello following pattern: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span></span>

7. <span data-ttu-id="13ed2-184">IQNavigator alkalmazás hello névazonosítója jogcím hello egyedi felhasználói azonosító értéket várt.</span><span class="sxs-lookup"><span data-stu-id="13ed2-184">IQNavigator application expect hello unique user identifier value in hello Name Identifier claim.</span></span> <span data-ttu-id="13ed2-185">Ügyfél leképezheti hello névazonosítója jogcímet hello a megfelelő értéket.</span><span class="sxs-lookup"><span data-stu-id="13ed2-185">Customer can map hello correct value for hello Name Identifier claim.</span></span> <span data-ttu-id="13ed2-186">Ebben az esetben azt leképezett hello felhasználó. UserPrincipalName hello bemutató célra.</span><span class="sxs-lookup"><span data-stu-id="13ed2-186">In this case we have mapped hello user.UserPrincipalName for hello demo purpose.</span></span> <span data-ttu-id="13ed2-187">De tooyour szervezeti beállítások szerint kell leképez hello megfelelő érték azt.</span><span class="sxs-lookup"><span data-stu-id="13ed2-187">But according tooyour organization settings you should map hello correct value for it.</span></span>   

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_attribute.png)

8. <span data-ttu-id="13ed2-189">A hello **IQNavigator virtuális gépek konfigurációs** kattintson **konfigurálása IQNavigator virtuális gépek** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="13ed2-189">On hello **IQNavigator VMS Configuration** section, click **Configure IQNavigator VMS** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="13ed2-190">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="13ed2-190">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_configure.png) 

9. <span data-ttu-id="13ed2-192">tooconfigure egyszeri bejelentkezést a **IQNavigator virtuális gépek** oldalsó toosend hello kell **metaadatainak URL-CÍMÉT**, **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** túl [IQNavigator virtuális gépek támogatási csoport](https://www.beeline.com/iqn-product-support/).</span><span class="sxs-lookup"><span data-stu-id="13ed2-192">tooconfigure single sign-on on **IQNavigator VMS** side, you need toosend hello **Metadata URL**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[IQNavigator VMS support team](https://www.beeline.com/iqn-product-support/).</span></span> <span data-ttu-id="13ed2-193">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="13ed2-193">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="13ed2-194">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="13ed2-194">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="13ed2-195">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="13ed2-195">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="13ed2-196">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="13ed2-196">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="13ed2-197">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="13ed2-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="13ed2-198">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="13ed2-198">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="13ed2-200">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="13ed2-200">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="13ed2-201">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="13ed2-201">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-iqnavigatorvms-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="13ed2-203">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="13ed2-203">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-iqnavigatorvms-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="13ed2-205">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="13ed2-205">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-iqnavigatorvms-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="13ed2-207">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="13ed2-207">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-iqnavigatorvms-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="13ed2-209">a.</span><span class="sxs-lookup"><span data-stu-id="13ed2-209">a.</span></span> <span data-ttu-id="13ed2-210">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="13ed2-210">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="13ed2-211">b.</span><span class="sxs-lookup"><span data-stu-id="13ed2-211">b.</span></span> <span data-ttu-id="13ed2-212">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="13ed2-212">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="13ed2-213">c.</span><span class="sxs-lookup"><span data-stu-id="13ed2-213">c.</span></span> <span data-ttu-id="13ed2-214">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="13ed2-214">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="13ed2-215">d.</span><span class="sxs-lookup"><span data-stu-id="13ed2-215">d.</span></span> <span data-ttu-id="13ed2-216">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="13ed2-216">Click **Create**.</span></span>
 
### <a name="creating-a-iqnavigator-vms-test-user"></a><span data-ttu-id="13ed2-217">IQNavigator virtuális gépek tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="13ed2-217">Creating a IQNavigator VMS test user</span></span>

<span data-ttu-id="13ed2-218">hello ebben a szakaszban célja toocreate IQNavigator virtuális gépek Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="13ed2-218">hello objective of this section is toocreate a user called Britta Simon in IQNavigator VMS.</span></span> <span data-ttu-id="13ed2-219">Együttműködve [IQNavigator virtuális gépek támogatási csoport](https://www.beeline.com/iqn-product-support/) tooadd hello felhasználók hello IQNavigator virtuális gépek fiók.</span><span class="sxs-lookup"><span data-stu-id="13ed2-219">Work with  [IQNavigator VMS support team](https://www.beeline.com/iqn-product-support/) tooadd hello users in hello IQNavigator VMS account.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="13ed2-220">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="13ed2-220">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="13ed2-221">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooIQNavigator virtuális gépek megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="13ed2-221">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooIQNavigator VMS.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="13ed2-223">**tooassign Britta Simon tooIQNavigator virtuális Gépeket, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="13ed2-223">**tooassign Britta Simon tooIQNavigator VMS, perform hello following steps:**</span></span>

1. <span data-ttu-id="13ed2-224">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="13ed2-224">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="13ed2-226">Hello alkalmazások listában válassza ki a **IQNavigator virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="13ed2-226">In hello applications list, select **IQNavigator VMS**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_app.png) 

3. <span data-ttu-id="13ed2-228">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="13ed2-228">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="13ed2-230">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="13ed2-230">Click **Add** button.</span></span> <span data-ttu-id="13ed2-231">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="13ed2-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="13ed2-233">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="13ed2-233">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="13ed2-234">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="13ed2-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="13ed2-235">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="13ed2-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="13ed2-236">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="13ed2-236">Testing single sign-on</span></span>

<span data-ttu-id="13ed2-237">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="13ed2-237">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="13ed2-238">Hello IQNavigator virtuális gépek csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour IQNavigator virtuális gépek alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="13ed2-238">When you click hello IQNavigator VMS tile in hello Access Panel, you should get automatically signed-on tooyour IQNavigator VMS application.</span></span>
<span data-ttu-id="13ed2-239">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="13ed2-239">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="13ed2-240">További források</span><span class="sxs-lookup"><span data-stu-id="13ed2-240">Additional resources</span></span>

* [<span data-ttu-id="13ed2-241">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="13ed2-241">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="13ed2-242">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="13ed2-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_203.png

