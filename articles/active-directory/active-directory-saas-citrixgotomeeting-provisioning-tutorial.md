---
title: "Oktatóanyag: Azure Active Directoryval integrált Citrix GoToMeeting |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Citrix GoToMeeting között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0f59fedb-2cf8-48d2-a5fb-222ed943ff78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 3c6eed5309dfa384c292b0cf63f8aa58988add81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-citrix-gotomeeting-for-automatic-user-provisioning"></a><span data-ttu-id="b85c7-103">Oktatóanyag: Citrix GoToMeeting konfigurálása az automatikus felhasználó létesítése</span><span class="sxs-lookup"><span data-stu-id="b85c7-103">Tutorial: Configuring Citrix GoToMeeting for Automatic User Provisioning</span></span>

<span data-ttu-id="b85c7-104">hello Ez az oktatóanyag célja tooshow meg hello a Citrix GoToMeeting és az Azure AD tooautomatically kiépítése és deaktiválás rendelkezés lévő felhasználói fiókok Azure AD tooCitrix GoToMeeting tooperform szükséges lépéseket.</span><span class="sxs-lookup"><span data-stu-id="b85c7-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in Citrix GoToMeeting and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooCitrix GoToMeeting.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b85c7-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b85c7-105">Prerequisites</span></span>

<span data-ttu-id="b85c7-106">Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="b85c7-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="b85c7-107">Az Azure Active directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="b85c7-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="b85c7-108">A Citrix GoToMeeting egyszeri bejelentkezés engedélyezve van az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="b85c7-108">A Citrix GoToMeeting single-sign on enabled subscription.</span></span>
*   <span data-ttu-id="b85c7-109">Egy felhasználói fiókot a Citrix GoToMeeting Team rendszergazdai engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="b85c7-109">A user account in Citrix GoToMeeting with Team Admin permissions.</span></span>

## <a name="assigning-users-toocitrix-gotomeeting"></a><span data-ttu-id="b85c7-110">Felhasználók tooCitrix GoToMeeting hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="b85c7-110">Assigning users tooCitrix GoToMeeting</span></span>

<span data-ttu-id="b85c7-111">Az Azure Active Directory mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű elvét használja.</span><span class="sxs-lookup"><span data-stu-id="b85c7-111">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="b85c7-112">Automatikus felhasználói fiók kiépítése hello kontextusában csak hello felhasználók és csoportok "hozzárendelt" tooan alkalmazás Azure Active Directory szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="b85c7-112">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD is synchronized.</span></span>

<span data-ttu-id="b85c7-113">Mielőtt hello szolgáltatás kiépítését engedélyezése és konfigurálása, kell toodecide milyen felhasználói és/vagy az Azure AD-csoportok hello felhasználókkal, akik tooyour Citrix GoToMeeting app kell meghatároznia.</span><span class="sxs-lookup"><span data-stu-id="b85c7-113">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour Citrix GoToMeeting app.</span></span> <span data-ttu-id="b85c7-114">Ha úgy döntött, hozzárendelheti a felhasználók tooyour Citrix GoToMeeting app itt hello utasításokat követve:</span><span class="sxs-lookup"><span data-stu-id="b85c7-114">Once decided, you can assign these users tooyour Citrix GoToMeeting app by following hello instructions here:</span></span>

[<span data-ttu-id="b85c7-115">Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások</span><span class="sxs-lookup"><span data-stu-id="b85c7-115">Assign a user or group tooan enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-toocitrix-gotomeeting"></a><span data-ttu-id="b85c7-116">Fontos tippek a felhasználók tooCitrix GoToMeeting hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="b85c7-116">Important tips for assigning users tooCitrix GoToMeeting</span></span>

*   <span data-ttu-id="b85c7-117">Javasoljuk, hogy egyetlen Azure AD-felhasználó tooCitrix GoToMeeting tootest hello konfigurálása kiosztás van hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="b85c7-117">It is recommended that a single Azure AD user is assigned tooCitrix GoToMeeting tootest hello provisioning configuration.</span></span> <span data-ttu-id="b85c7-118">További felhasználók és/vagy csoportok később is rendelhető.</span><span class="sxs-lookup"><span data-stu-id="b85c7-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="b85c7-119">Amikor egy felhasználó tooCitrix GoToMeeting rendel, ki kell választania egy érvényes felhasználói szerepkörnek.</span><span class="sxs-lookup"><span data-stu-id="b85c7-119">When assigning a user tooCitrix GoToMeeting, you must select a valid user role.</span></span> <span data-ttu-id="b85c7-120">hello "alapértelmezett" szerepkör nem működik történő üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="b85c7-120">hello "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="b85c7-121">Az automatikus felhasználó-kiépítés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="b85c7-121">Enable Automated User Provisioning</span></span>

<span data-ttu-id="b85c7-122">Ez a szakasz végigvezeti a csatlakozás az Azure AD tooCitrix GoToMeeting felhasználói fiók kiépítése API és a kiépítés szolgáltatás toocreate hello konfigurálása, frissítése, és tiltsa le a hozzárendelt felhasználói fiókok a Citrix GoToMeeting alapján a felhasználók és csoportok az Azure AD-hozzárendelés.</span><span class="sxs-lookup"><span data-stu-id="b85c7-122">This section guides you through connecting your Azure AD tooCitrix GoToMeeting's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in Citrix GoToMeeting based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="b85c7-123">Dönthet úgy is tooenabled SAML-alapú egyszeri bejelentkezést a Citrix GoToMeeting hello megjelenő utasításokat követve [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b85c7-123">You may also choose tooenabled SAML-based Single Sign-On for Citrix GoToMeeting, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="b85c7-124">Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.</span><span class="sxs-lookup"><span data-stu-id="b85c7-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="tooconfigure-automatic-user-account-provisioning"></a><span data-ttu-id="b85c7-125">tooconfigure automatikus felhasználói fiók kiépítése:</span><span class="sxs-lookup"><span data-stu-id="b85c7-125">tooconfigure automatic user account provisioning:</span></span>

1. <span data-ttu-id="b85c7-126">A hello [Azure-portálon](https://portal.azure.com), keresse meg a toohello **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="b85c7-126">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="b85c7-127">Ha már konfigurált Citrix GoToMeeting egyszeri bejelentkezést, keresse meg a Citrix GoToMeeting hello keresési mező példányát.</span><span class="sxs-lookup"><span data-stu-id="b85c7-127">If you have already configured Citrix GoToMeeting for single sign-on, search for your instance of Citrix GoToMeeting using hello search field.</span></span> <span data-ttu-id="b85c7-128">Máskülönben válassza **Hozzáadás** keresse meg a **Citrix GoToMeeting** hello alkalmazás gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="b85c7-128">Otherwise, select **Add** and search for **Citrix GoToMeeting** in hello application gallery.</span></span> <span data-ttu-id="b85c7-129">Válassza ki a Citrix GoToMeeting hello keresési eredmények közül, és vegye fel tooyour alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="b85c7-129">Select Citrix GoToMeeting from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="b85c7-130">Jelölje ki a Citrix GoToMeeting példányát, majd válassza ki a hello **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="b85c7-130">Select your instance of Citrix GoToMeeting, then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="b85c7-131">Set hello **kiépítési** mód túl**automatikus**.</span><span class="sxs-lookup"><span data-stu-id="b85c7-131">Set hello **Provisioning** Mode too**Automatic**.</span></span> 

    ![Kiépítés](./media/active-directory-saas-citrixgotomeeting-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="b85c7-133">A rendszergazdai hitelesítő adataival szakasz hello hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="b85c7-133">Under hello Admin Credentials section, perform hello following steps:</span></span>
   
    <span data-ttu-id="b85c7-134">a.</span><span class="sxs-lookup"><span data-stu-id="b85c7-134">a.</span></span> <span data-ttu-id="b85c7-135">A hello **Citrix GoToMeeting rendszergazda felhasználóneve** szövegmező, írja be a hello felhasználónevet egy rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="b85c7-135">In hello **Citrix GoToMeeting Admin User Name** textbox, type hello user name of an administrator.</span></span>

    <span data-ttu-id="b85c7-136">b.</span><span class="sxs-lookup"><span data-stu-id="b85c7-136">b.</span></span> <span data-ttu-id="b85c7-137">A hello **Citrix GoToMeeting rendszergazdai jelszó** szövegmezőhöz hello rendszergazdai jelszót.</span><span class="sxs-lookup"><span data-stu-id="b85c7-137">In hello **Citrix GoToMeeting Admin Password** textbox, hello administrator's password.</span></span>

6. <span data-ttu-id="b85c7-138">Hello Azure-portálon, kattintson **kapcsolat tesztelése** tooensure az Azure AD kapcsolódhatnak tooyour Citrix GoToMeeting alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b85c7-138">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour Citrix GoToMeeting app.</span></span> <span data-ttu-id="b85c7-139">Ha hello létesített kapcsolat megszakad, győződjön meg arról, a Citrix GoToMeeting fiók Team rendszergazdai jogosultságokkal rendelkezik, majd próbálja hello **"Rendszergazdai hitelesítő adatok"** léptessen ismét.</span><span class="sxs-lookup"><span data-stu-id="b85c7-139">If hello connection fails, ensure your Citrix GoToMeeting account has Team Admin permissions and try hello **"Admin Credentials"** step again.</span></span>

7. <span data-ttu-id="b85c7-140">Adja meg a hello e-mail címet vagy egy csoport létesítési hiba értesítések a hello kapjanak **értesítő e-mailt** mezőben, majd jelölje be hello jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="b85c7-140">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox.</span></span>

8. <span data-ttu-id="b85c7-141">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="b85c7-141">Click **Save.**</span></span>

9. <span data-ttu-id="b85c7-142">A hello hozzárendelések szakaszt, válassza a **szinkronizálása Azure Active Directory-felhasználók tooCitrix GoToMeeting.**</span><span class="sxs-lookup"><span data-stu-id="b85c7-142">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooCitrix GoToMeeting.**</span></span>

10. <span data-ttu-id="b85c7-143">A hello **attribútum-leképezésekhez** szakaszban, tekintse át az Azure AD tooCitrix GoToMeeting szinkronizált hello felhasználói attribútumok.</span><span class="sxs-lookup"><span data-stu-id="b85c7-143">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooCitrix GoToMeeting.</span></span> <span data-ttu-id="b85c7-144">kiválasztott attribútumok hello **egyező** tulajdonságok használt toomatch hello tartozó felhasználói fiókok Citrix GoToMeeting a frissítési műveletekben.</span><span class="sxs-lookup"><span data-stu-id="b85c7-144">hello attributes selected as **Matching** properties are used toomatch hello user accounts in Citrix GoToMeeting for update operations.</span></span> <span data-ttu-id="b85c7-145">Válassza ki a hello Mentés gombra toocommit módosításokat.</span><span class="sxs-lookup"><span data-stu-id="b85c7-145">Select hello Save button toocommit any changes.</span></span>

11. <span data-ttu-id="b85c7-146">tooenable hello Azure AD létesítési szolgáltatás a Citrix GoToMeeting, módosítás hello **kiépítési állapot** túl**a** hello beállítások szakaszában a</span><span class="sxs-lookup"><span data-stu-id="b85c7-146">tooenable hello Azure AD provisioning service for Citrix GoToMeeting, change hello **Provisioning Status** too**On** in hello Settings section</span></span>

12. <span data-ttu-id="b85c7-147">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="b85c7-147">Click **Save.**</span></span>

<span data-ttu-id="b85c7-148">Hello azokat a felhasználókat a kezdeti szinkronizálását kezdődik, és/vagy csoportok hozzárendelve tooCitrix GoToMeeting hello felhasználók és csoportok szakaszban.</span><span class="sxs-lookup"><span data-stu-id="b85c7-148">It starts hello initial synchronization of any users and/or groups assigned tooCitrix GoToMeeting in hello Users and Groups section.</span></span> <span data-ttu-id="b85c7-149">hello kezdeti szinkronizálás hosszabb, mint bekövetkező körülbelül 20 percenként, mindaddig, amíg hello szolgáltatás fut. ezt követő szinkronizálások tooperform vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="b85c7-149">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="b85c7-150">Használhatja a hello **szinkronizálás részleteivel** toomonitor folyamatban szakaszt, és hivatkozások tooprovisioning Tevékenységjelentések, minden hello szolgáltatást a Citrix GoToMeeting app a kiépítés végrehajtott műveletekről, amelyeket követve.</span><span class="sxs-lookup"><span data-stu-id="b85c7-150">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service on your Citrix GoToMeeting app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b85c7-151">További források</span><span class="sxs-lookup"><span data-stu-id="b85c7-151">Additional resources</span></span>

* [<span data-ttu-id="b85c7-152">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="b85c7-152">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b85c7-153">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="b85c7-153">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="b85c7-154">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b85c7-154">Configure Single Sign-on</span></span>](active-directory-saas-citrix-gotomeeting-tutorial.md)


