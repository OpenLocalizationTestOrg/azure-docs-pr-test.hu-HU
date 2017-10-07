---
title: "Oktatóanyag: Azure Active Directoryval integrált Jive |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Jive között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6fbfdbe7-d66c-4305-9fea-76d6a6a92830
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: b1c0d0bc2d79427c055f577fe5f9d30d10f1bbdd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-jive-for-user-provisioning"></a><span data-ttu-id="bafcb-103">Oktatóanyag: A felhasználók átadása Jive konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bafcb-103">Tutorial: Configuring Jive for User Provisioning</span></span>

<span data-ttu-id="bafcb-104">hello Ez az oktatóanyag célja tooshow meg hello tooperform Jive és az Azure AD tooautomatically kiépítése és deaktiválás rendelkezés lévő felhasználói fiókok Azure AD tooJive a szükséges lépéseket.</span><span class="sxs-lookup"><span data-stu-id="bafcb-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in Jive and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooJive.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bafcb-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="bafcb-105">Prerequisites</span></span>

<span data-ttu-id="bafcb-106">Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="bafcb-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="bafcb-107">Az Azure Active directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="bafcb-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="bafcb-108">Egy Jive egyszeri bejelentkezés engedélyezve van az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="bafcb-108">A Jive single-sign on enabled subscription.</span></span>
*   <span data-ttu-id="bafcb-109">Egy felhasználói fiókot az Jive Team rendszergazdai engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="bafcb-109">A user account in Jive with Team Admin permissions.</span></span>

## <a name="assigning-users-toojive"></a><span data-ttu-id="bafcb-110">Felhasználók tooJive hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="bafcb-110">Assigning users tooJive</span></span>

<span data-ttu-id="bafcb-111">Az Azure Active Directory mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű elvét használja.</span><span class="sxs-lookup"><span data-stu-id="bafcb-111">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="bafcb-112">Automatikus felhasználói fiók kiépítése hello kontextusában csak hello felhasználók és csoportok "hozzárendelt" tooan alkalmazás Azure Active Directory szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="bafcb-112">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD is synchronized.</span></span>

<span data-ttu-id="bafcb-113">Mielőtt hello szolgáltatás kiépítését engedélyezése és konfigurálása, kell toodecide milyen felhasználói és/vagy az Azure AD-csoportok hello felhasználókkal, akik tooyour Jive app kell meghatároznia.</span><span class="sxs-lookup"><span data-stu-id="bafcb-113">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour Jive app.</span></span> <span data-ttu-id="bafcb-114">Ha úgy döntött, hozzárendelheti a felhasználók tooyour Jive app itt hello utasításokat követve:</span><span class="sxs-lookup"><span data-stu-id="bafcb-114">Once decided, you can assign these users tooyour Jive app by following hello instructions here:</span></span>

[<span data-ttu-id="bafcb-115">Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások</span><span class="sxs-lookup"><span data-stu-id="bafcb-115">Assign a user or group tooan enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-toojive"></a><span data-ttu-id="bafcb-116">Fontos tippek a felhasználók tooJive hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="bafcb-116">Important tips for assigning users tooJive</span></span>

*   <span data-ttu-id="bafcb-117">Javasoljuk, hogy egyetlen Azure AD-felhasználó tooJive tootest hello konfigurálása kiosztás rendelni.</span><span class="sxs-lookup"><span data-stu-id="bafcb-117">It is recommended that a single Azure AD user be assigned tooJive tootest hello provisioning configuration.</span></span> <span data-ttu-id="bafcb-118">További felhasználók és/vagy csoportok később is rendelhető.</span><span class="sxs-lookup"><span data-stu-id="bafcb-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="bafcb-119">Amikor egy felhasználó tooJive rendel, ki kell választania egy érvényes felhasználói szerepkörnek.</span><span class="sxs-lookup"><span data-stu-id="bafcb-119">When assigning a user tooJive, you must select a valid user role.</span></span> <span data-ttu-id="bafcb-120">hello "alapértelmezett" szerepkör nem működik történő üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="bafcb-120">hello "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-user-provisioning"></a><span data-ttu-id="bafcb-121">Felhasználó-kiépítés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="bafcb-121">Enable User Provisioning</span></span>

<span data-ttu-id="bafcb-122">Ez a szakasz végigvezeti a csatlakozás az Azure AD tooJive felhasználói fiók kiépítése API és kiépítése szolgáltatáshoz toocreate hello konfigurálása, frissítése, és tiltsa le a hozzárendelt felhasználói fiókok a Jive alapján a felhasználók és csoportok hozzárendelése az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="bafcb-122">This section guides you through connecting your Azure AD tooJive's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in Jive based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="bafcb-123">Dönthet úgy is SAML-alapú egyszeri bejelentkezés az Jive hello megjelenő utasításokat követve tooenabled [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="bafcb-123">You may also choose tooenabled SAML-based Single Sign-On for Jive, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="bafcb-124">Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.</span><span class="sxs-lookup"><span data-stu-id="bafcb-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="tooconfigure-user-account-provisioning"></a><span data-ttu-id="bafcb-125">tooconfigure felhasználói fiók kiépítése:</span><span class="sxs-lookup"><span data-stu-id="bafcb-125">tooconfigure user account provisioning:</span></span>

<span data-ttu-id="bafcb-126">hello ebben a szakaszban célja toooutline hogyan tooJive tooenable a felhasználók átadása az Active Directory felhasználói fiókok.</span><span class="sxs-lookup"><span data-stu-id="bafcb-126">hello objective of this section is toooutline how tooenable user provisioning of Active Directory user accounts tooJive.</span></span>
<span data-ttu-id="bafcb-127">Ez az eljárás részeként szükség tooprovide szüksége toorequest Jive.com a felhasználó biztonsági jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="bafcb-127">As part of this procedure, you are required tooprovide a user security token you need toorequest from Jive.com.</span></span>

1. <span data-ttu-id="bafcb-128">A hello [Azure-portálon](https://portal.azure.com), keresse meg a toohello **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="bafcb-128">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="bafcb-129">Ha már konfigurált Jive egyszeri bejelentkezést, keresse meg a hello keresési mező Jive példányát.</span><span class="sxs-lookup"><span data-stu-id="bafcb-129">If you have already configured Jive for single sign-on, search for your instance of Jive using hello search field.</span></span> <span data-ttu-id="bafcb-130">Máskülönben válassza **Hozzáadás** keresse meg a **Jive** hello alkalmazás gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="bafcb-130">Otherwise, select **Add** and search for **Jive** in hello application gallery.</span></span> <span data-ttu-id="bafcb-131">Válassza ki a Jive hello keresési eredmények közül, és vegye fel tooyour alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="bafcb-131">Select Jive from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="bafcb-132">Jelölje ki a Jive példányát, majd jelölje ki a hello **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="bafcb-132">Select your instance of Jive, then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="bafcb-133">Set hello **kiépítési üzemmódban** túl**automatikus**.</span><span class="sxs-lookup"><span data-stu-id="bafcb-133">Set hello **Provisioning Mode** too**Automatic**.</span></span> 

    ![Kiépítés](./media/active-directory-saas-jive-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="bafcb-135">A hello **rendszergazdai hitelesítő adataival** területen adja meg a következő konfigurációs beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="bafcb-135">Under hello **Admin Credentials** section, provide hello following configuration settings:</span></span>
   
    <span data-ttu-id="bafcb-136">a.</span><span class="sxs-lookup"><span data-stu-id="bafcb-136">a.</span></span> <span data-ttu-id="bafcb-137">A hello **Jive rendszergazda felhasználóneve** szövegmezőhöz típus egy Jive fióknév rendelkezik hello **rendszergazda** Jive.com rendelt profillal.</span><span class="sxs-lookup"><span data-stu-id="bafcb-137">In hello **Jive Admin User Name** textbox, type a Jive account name that has hello **System Administrator** profile in Jive.com assigned.</span></span>
   
    <span data-ttu-id="bafcb-138">b.</span><span class="sxs-lookup"><span data-stu-id="bafcb-138">b.</span></span> <span data-ttu-id="bafcb-139">A hello **Jive rendszergazdai jelszó** szövegmezőhöz típus hello fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="bafcb-139">In hello **Jive Admin Password** textbox, type hello password for this account.</span></span>
   
    <span data-ttu-id="bafcb-140">c.</span><span class="sxs-lookup"><span data-stu-id="bafcb-140">c.</span></span> <span data-ttu-id="bafcb-141">A hello **Jive bérlői URL-cím** szövegmezőhöz típus hello Jive bérlői URL-cím.</span><span class="sxs-lookup"><span data-stu-id="bafcb-141">In hello **Jive Tenant URL** textbox, type hello Jive tenant URL.</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="bafcb-142">hello Jive bérlői URL-cím a szervezet toolog tooJive a által használt URL-cím.</span><span class="sxs-lookup"><span data-stu-id="bafcb-142">hello Jive tenant URL is URL that is used by your organization toolog in tooJive.</span></span>  
      > <span data-ttu-id="bafcb-143">Általában a hello URL-címnek a hello a következő formátumban: **www.\< szervezet\>. jive.com**.</span><span class="sxs-lookup"><span data-stu-id="bafcb-143">Typically, hello URL has hello following format: **www.\<organization\>.jive.com**.</span></span>          

6. <span data-ttu-id="bafcb-144">Hello Azure-portálon, kattintson **kapcsolat tesztelése** tooensure az Azure AD tooyour Jive alkalmazás képes kapcsolódni.</span><span class="sxs-lookup"><span data-stu-id="bafcb-144">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour Jive app.</span></span>

7. <span data-ttu-id="bafcb-145">Adja meg a hello e-mail címet vagy egy csoport létesítési hiba értesítések a hello kapjanak **értesítő e-mailt** mezőben, majd jelölje be az alábbi hello jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="bafcb-145">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox below.</span></span>

8. <span data-ttu-id="bafcb-146">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="bafcb-146">Click **Save.**</span></span>

9. <span data-ttu-id="bafcb-147">A hello hozzárendelések szakaszt, válassza a **szinkronizálása Azure Active Directory-felhasználók tooJive.**</span><span class="sxs-lookup"><span data-stu-id="bafcb-147">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooJive.**</span></span>

10. <span data-ttu-id="bafcb-148">A hello **attribútum-leképezésekhez** szakaszban, tekintse át az Azure AD tooJive szinkronizált hello felhasználói attribútumok.</span><span class="sxs-lookup"><span data-stu-id="bafcb-148">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooJive.</span></span> <span data-ttu-id="bafcb-149">kiválasztott attribútumok hello **egyező** tulajdonságok használt toomatch hello tartozó felhasználói fiókok Jive a frissítési műveletekben.</span><span class="sxs-lookup"><span data-stu-id="bafcb-149">hello attributes selected as **Matching** properties are used toomatch hello user accounts in Jive for update operations.</span></span> <span data-ttu-id="bafcb-150">Válassza ki a hello Mentés gombra toocommit módosításokat.</span><span class="sxs-lookup"><span data-stu-id="bafcb-150">Select hello Save button toocommit any changes.</span></span>

11. <span data-ttu-id="bafcb-151">tooenable hello Azure AD Jive, módosítás hello a létesítési szolgáltatás **kiépítési állapot** túl**a** hello beállítások szakaszában a</span><span class="sxs-lookup"><span data-stu-id="bafcb-151">tooenable hello Azure AD provisioning service for Jive, change hello **Provisioning Status** too**On** in hello Settings section</span></span>

12. <span data-ttu-id="bafcb-152">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="bafcb-152">Click **Save.**</span></span>

<span data-ttu-id="bafcb-153">Hello kezdeti szinkronizálás bármely felhasználói és/vagy csoportok tooJive a hello felhasználók és csoportok szakasz kezdődik.</span><span class="sxs-lookup"><span data-stu-id="bafcb-153">It starts hello initial synchronization of any users and/or groups assigned tooJive in hello Users and Groups section.</span></span> <span data-ttu-id="bafcb-154">hello kezdeti szinkronizálás hosszabb, mint bekövetkező körülbelül 20 percenként, mindaddig, amíg hello szolgáltatás fut. ezt követő szinkronizálások tooperform vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="bafcb-154">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="bafcb-155">Használhatja a hello **szinkronizálás részleteivel** toomonitor folyamatban szakaszt, és kövesse az Jive alkalmazásnak szolgáltatás kiépítését hello által végzett összes műveletet írják le hivatkozások tooprovisioning tevékenység jelentéseket.</span><span class="sxs-lookup"><span data-stu-id="bafcb-155">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service on your Jive app.</span></span>

<span data-ttu-id="bafcb-156">Mostantól létrehozhat egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="bafcb-156">You can now create a test account.</span></span> <span data-ttu-id="bafcb-157">Várjon, amíg fel hello fiókot töltött tooverify szinkronizált tooJive too20 perc.</span><span class="sxs-lookup"><span data-stu-id="bafcb-157">Wait for up too20 minutes tooverify that hello account has been synchronized tooJive.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bafcb-158">További források</span><span class="sxs-lookup"><span data-stu-id="bafcb-158">Additional resources</span></span>

* [<span data-ttu-id="bafcb-159">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="bafcb-159">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="bafcb-160">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="bafcb-160">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="bafcb-161">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bafcb-161">Configure Single Sign-on</span></span>](active-directory-saas-jive-tutorial.md)