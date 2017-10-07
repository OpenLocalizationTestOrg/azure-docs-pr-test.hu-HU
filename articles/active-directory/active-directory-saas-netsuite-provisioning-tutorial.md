---
title: "Oktatóanyag: Azure Active Directoryval integrált Netsuite |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Netsuite között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8a6d3994-ee33-4a6f-b0a2-9d0389467f16
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 5bb2989c1296b9f2abc9e8c84855731adc484aab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-netsuite-for-automatic-user-provisioning"></a><span data-ttu-id="9a726-103">Oktatóanyag: Netsuite konfigurálása az automatikus felhasználó létesítése</span><span class="sxs-lookup"><span data-stu-id="9a726-103">Tutorial: Configuring Netsuite for Automatic User Provisioning</span></span>

<span data-ttu-id="9a726-104">hello Ez az oktatóanyag célja tooshow meg hello tooperform Netsuite és az Azure AD tooautomatically kiépítése és deaktiválás rendelkezés lévő felhasználói fiókok Azure AD tooNetsuite a szükséges lépéseket.</span><span class="sxs-lookup"><span data-stu-id="9a726-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in Netsuite and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooNetsuite.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9a726-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9a726-105">Prerequisites</span></span>

<span data-ttu-id="9a726-106">Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="9a726-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="9a726-107">Az Azure Active directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="9a726-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="9a726-108">Egy Netsuite egyszeri bejelentkezés engedélyezve van az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="9a726-108">A Netsuite single-sign on enabled subscription.</span></span>
*   <span data-ttu-id="9a726-109">Egy felhasználói fiókot az Netsuite Team rendszergazdai engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="9a726-109">A user account in Netsuite with Team Admin permissions.</span></span>

## <a name="assigning-users-toonetsuite"></a><span data-ttu-id="9a726-110">Felhasználók tooNetsuite hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="9a726-110">Assigning users tooNetsuite</span></span>

<span data-ttu-id="9a726-111">Az Azure Active Directory mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű elvét használja.</span><span class="sxs-lookup"><span data-stu-id="9a726-111">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="9a726-112">Automatikus felhasználói fiók kiépítése hello kontextusában, csak hello felhasználók és csoportok "hozzárendelt" tooan alkalmazás az Azure AD szinkronizálva van.</span><span class="sxs-lookup"><span data-stu-id="9a726-112">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD are synchronized.</span></span>

<span data-ttu-id="9a726-113">Mielőtt hello szolgáltatás kiépítését engedélyezése és konfigurálása, kell toodecide milyen felhasználói és/vagy csoportok tooyour Netsuite alkalmazást kell használni az Azure AD jelentik hello felhasználók.</span><span class="sxs-lookup"><span data-stu-id="9a726-113">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour Netsuite app.</span></span> <span data-ttu-id="9a726-114">Ha úgy döntött, hozzárendelheti a felhasználók tooyour Netsuite app itt hello utasításokat követve:</span><span class="sxs-lookup"><span data-stu-id="9a726-114">Once decided, you can assign these users tooyour Netsuite app by following hello instructions here:</span></span>

[<span data-ttu-id="9a726-115">Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások</span><span class="sxs-lookup"><span data-stu-id="9a726-115">Assign a user or group tooan enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-toonetsuite"></a><span data-ttu-id="9a726-116">Fontos tippek a felhasználók tooNetsuite hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="9a726-116">Important tips for assigning users tooNetsuite</span></span>

*   <span data-ttu-id="9a726-117">Javasoljuk, hogy egyetlen Azure AD-felhasználó tooNetsuite tootest hello konfigurálása kiosztás van hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="9a726-117">It is recommended that a single Azure AD user is assigned tooNetsuite tootest hello provisioning configuration.</span></span> <span data-ttu-id="9a726-118">További felhasználók és/vagy csoportok később is rendelhető.</span><span class="sxs-lookup"><span data-stu-id="9a726-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="9a726-119">Amikor egy felhasználó tooNetsuite rendel, ki kell választania egy érvényes felhasználói szerepkörnek.</span><span class="sxs-lookup"><span data-stu-id="9a726-119">When assigning a user tooNetsuite, you must select a valid user role.</span></span> <span data-ttu-id="9a726-120">hello "alapértelmezett" szerepkör nem működik történő üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="9a726-120">hello "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-user-provisioning"></a><span data-ttu-id="9a726-121">Felhasználó-kiépítés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="9a726-121">Enable User Provisioning</span></span>

<span data-ttu-id="9a726-122">Ez a szakasz végigvezeti a csatlakozás az Azure AD tooNetsuite felhasználói fiók kiépítése API és kiépítése szolgáltatáshoz toocreate hello konfigurálása, frissítése, és tiltsa le a hozzárendelt felhasználói fiókok a Netsuite alapján a felhasználók és csoportok hozzárendelése az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="9a726-122">This section guides you through connecting your Azure AD tooNetsuite's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in Netsuite based on user and group assignment in Azure AD.</span></span>

> [!TIP] 
> <span data-ttu-id="9a726-123">Dönthet úgy is tooenabled SAML-alapú egyszeri bejelentkezést a Netsuite hello megjelenő utasításokat követve [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="9a726-123">You may also choose tooenabled SAML-based Single Sign-On for Netsuite, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="9a726-124">Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.</span><span class="sxs-lookup"><span data-stu-id="9a726-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="tooconfigure-user-account-provisioning"></a><span data-ttu-id="9a726-125">tooconfigure felhasználói fiók kiépítése:</span><span class="sxs-lookup"><span data-stu-id="9a726-125">tooconfigure user account provisioning:</span></span>

<span data-ttu-id="9a726-126">hello ebben a szakaszban célja toooutline hogyan tooNetsuite tooenable a felhasználók átadása az Active Directory felhasználói fiókok.</span><span class="sxs-lookup"><span data-stu-id="9a726-126">hello objective of this section is toooutline how tooenable user provisioning of Active Directory user accounts tooNetsuite.</span></span>

1. <span data-ttu-id="9a726-127">A hello [Azure-portálon](https://portal.azure.com), keresse meg a toohello **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="9a726-127">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="9a726-128">Ha már konfigurált Netsuite egyszeri bejelentkezést, keresse meg a hello keresési mező Netsuite példányát.</span><span class="sxs-lookup"><span data-stu-id="9a726-128">If you have already configured Netsuite for single sign-on, search for your instance of Netsuite using hello search field.</span></span> <span data-ttu-id="9a726-129">Máskülönben válassza **Hozzáadás** keresse meg a **Netsuite** hello alkalmazás gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="9a726-129">Otherwise, select **Add** and search for **Netsuite** in hello application gallery.</span></span> <span data-ttu-id="9a726-130">Válassza ki a Netsuite hello keresési eredmények közül, és vegye fel tooyour alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="9a726-130">Select Netsuite from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="9a726-131">Jelölje ki a Netsuite példányát, majd jelölje ki a hello **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="9a726-131">Select your instance of Netsuite, then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="9a726-132">Set hello **kiépítési üzemmódban** túl**automatikus**.</span><span class="sxs-lookup"><span data-stu-id="9a726-132">Set hello **Provisioning Mode** too**Automatic**.</span></span> 

    ![Kiépítés](./media/active-directory-saas-netsuite-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="9a726-134">A hello **rendszergazdai hitelesítő adataival** területen adja meg a következő konfigurációs beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="9a726-134">Under hello **Admin Credentials** section, provide hello following configuration settings:</span></span>
   
    <span data-ttu-id="9a726-135">a.</span><span class="sxs-lookup"><span data-stu-id="9a726-135">a.</span></span> <span data-ttu-id="9a726-136">A hello **rendszergazda felhasználóneve** szövegmezőhöz típus egy Netsuite fióknév rendelkezik hello **rendszergazda** Netsuite.com rendelt profillal.</span><span class="sxs-lookup"><span data-stu-id="9a726-136">In hello **Admin User Name** textbox, type a Netsuite account name that has hello **System Administrator** profile in Netsuite.com assigned.</span></span>
   
    <span data-ttu-id="9a726-137">b.</span><span class="sxs-lookup"><span data-stu-id="9a726-137">b.</span></span> <span data-ttu-id="9a726-138">A hello **rendszergazdai jelszó** szövegmezőhöz típus hello fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="9a726-138">In hello **Admin Password** textbox, type hello password for this account.</span></span>
      
6. <span data-ttu-id="9a726-139">Hello Azure-portálon, kattintson **kapcsolat tesztelése** tooensure az Azure AD tooyour Netsuite alkalmazás képes kapcsolódni.</span><span class="sxs-lookup"><span data-stu-id="9a726-139">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour Netsuite app.</span></span>

7. <span data-ttu-id="9a726-140">A hello **értesítő e-mailt** mezőbe írja be a hello e-mail címet vagy egy csoportot ki kell létesítési hiba értesítéseket, és jelölje be hello jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="9a726-140">In hello **Notification Email** field, enter hello email address of a person or group who should receive provisioning error notifications, and check hello checkbox.</span></span>

8. <span data-ttu-id="9a726-141">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="9a726-141">Click **Save.**</span></span>

9. <span data-ttu-id="9a726-142">A hello hozzárendelések szakaszt, válassza a **szinkronizálása Azure Active Directory-felhasználók tooNetsuite.**</span><span class="sxs-lookup"><span data-stu-id="9a726-142">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooNetsuite.**</span></span>

10. <span data-ttu-id="9a726-143">A hello **attribútum-leképezésekhez** szakaszban, tekintse át az Azure AD tooNetsuite szinkronizált hello felhasználói attribútumok.</span><span class="sxs-lookup"><span data-stu-id="9a726-143">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooNetsuite.</span></span> <span data-ttu-id="9a726-144">Vegye figyelembe, hogy a kiválasztott attribútumok hello **egyező** tulajdonságok használt toomatch hello tartozó felhasználói fiókok Netsuite a frissítési műveletekben.</span><span class="sxs-lookup"><span data-stu-id="9a726-144">Note that hello attributes selected as **Matching** properties are used toomatch hello user accounts in Netsuite for update operations.</span></span> <span data-ttu-id="9a726-145">Válassza ki a hello Mentés gombra toocommit módosításokat.</span><span class="sxs-lookup"><span data-stu-id="9a726-145">Select hello Save button toocommit any changes.</span></span>

11. <span data-ttu-id="9a726-146">tooenable hello Azure AD Netsuite, módosítás hello a létesítési szolgáltatás **kiépítési állapot** túl**a** hello beállítások szakaszában a</span><span class="sxs-lookup"><span data-stu-id="9a726-146">tooenable hello Azure AD provisioning service for Netsuite, change hello **Provisioning Status** too**On** in hello Settings section</span></span>

12. <span data-ttu-id="9a726-147">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="9a726-147">Click **Save.**</span></span>

<span data-ttu-id="9a726-148">Hello kezdeti szinkronizálás bármely felhasználói és/vagy csoportok tooNetsuite a hello felhasználók és csoportok szakasz kezdődik.</span><span class="sxs-lookup"><span data-stu-id="9a726-148">It starts hello initial synchronization of any users and/or groups assigned tooNetsuite in hello Users and Groups section.</span></span> <span data-ttu-id="9a726-149">Vegye figyelembe, hogy a hello kezdeti szinkronizálás hosszabb, mint bekövetkező körülbelül 20 percenként, mindaddig, amíg hello szolgáltatás fut. ezt követő szinkronizálások tooperform vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="9a726-149">Note that hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="9a726-150">Használhatja a hello **szinkronizálás részleteivel** toomonitor folyamatban szakaszt, és kövesse az Netsuite alkalmazásnak szolgáltatás kiépítését hello által végzett összes műveletet írják le hivatkozások tooprovisioning tevékenység jelentéseket.</span><span class="sxs-lookup"><span data-stu-id="9a726-150">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service on your Netsuite app.</span></span>

<span data-ttu-id="9a726-151">Mostantól létrehozhat egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="9a726-151">You can now create a test account.</span></span> <span data-ttu-id="9a726-152">Várjon, amíg fel hello fiókot töltött tooverify szinkronizált tooNetsuite too20 perc.</span><span class="sxs-lookup"><span data-stu-id="9a726-152">Wait for up too20 minutes tooverify that hello account has been synchronized tooNetsuite.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9a726-153">További források</span><span class="sxs-lookup"><span data-stu-id="9a726-153">Additional resources</span></span>

* [<span data-ttu-id="9a726-154">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="9a726-154">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9a726-155">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="9a726-155">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="9a726-156">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9a726-156">Configure Single Sign-on</span></span>](active-directory-saas-netsuite-tutorial.md)