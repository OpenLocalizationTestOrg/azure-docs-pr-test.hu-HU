---
title: "Oktatóanyag: Azure Active Directoryval integrált vállalati Dropbox |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a vállalati Dropbox között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0f3a42e4-6897-4234-af84-b47c148ec3e1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 0fb01eab4f7c6c4516eac64a4343e46ea221f98d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-dropbox-for-business-for-automatic-user-provisioning"></a><span data-ttu-id="83484-103">Oktatóanyag: Üzleti Dropbox konfigurálása automatikus felhasználói történő üzembe helyezéséhez</span><span class="sxs-lookup"><span data-stu-id="83484-103">Tutorial: Configuring Dropbox for Business for Automatic User Provisioning</span></span>

<span data-ttu-id="83484-104">hello Ez az oktatóanyag célja meg van szüksége a Dropbox tooperform üzleti és az Azure AD tooautomatically kiépítése és deaktiválás rendelkezés lévő felhasználói fiókok Azure AD tooDropbox vállalati lépéseket hello tooshow.</span><span class="sxs-lookup"><span data-stu-id="83484-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in Dropbox for Business and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooDropbox for Business.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="83484-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="83484-105">Prerequisites</span></span>

<span data-ttu-id="83484-106">Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="83484-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="83484-107">Az Azure Active directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="83484-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="83484-108">A Dropbox az üzleti egyszeri bejelentkezés engedélyezve van az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="83484-108">A Dropbox for Business single-sign on enabled subscription.</span></span>
*   <span data-ttu-id="83484-109">Egy felhasználói fiókot az üzleti Dropbox Team rendszergazdai engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="83484-109">A user account in Dropbox for Business with Team Admin permissions.</span></span>

## <a name="assigning-users-toodropbox-for-business"></a><span data-ttu-id="83484-110">A vállalati felhasználók tooDropbox hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="83484-110">Assigning users tooDropbox for Business</span></span>

<span data-ttu-id="83484-111">Az Azure Active Directory mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű elvét használja.</span><span class="sxs-lookup"><span data-stu-id="83484-111">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="83484-112">Automatikus felhasználói fiók kiépítése hello kontextusában csak hello felhasználók és csoportok "hozzárendelt" tooan alkalmazás Azure Active Directory szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="83484-112">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD is synchronized.</span></span>

<span data-ttu-id="83484-113">Mielőtt hello szolgáltatás kiépítését engedélyezése és konfigurálása, kell toodecide milyen felhasználói és/vagy az Azure AD-csoportok hello felhasználókkal, akik tooyour Dropbox alkalmazás kell meghatároznia.</span><span class="sxs-lookup"><span data-stu-id="83484-113">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour Dropbox for Business app.</span></span> <span data-ttu-id="83484-114">Ha úgy döntött, itt hello utasításokat követve rendelhet a felhasználók tooyour Dropbox alkalmazás:</span><span class="sxs-lookup"><span data-stu-id="83484-114">Once decided, you can assign these users tooyour Dropbox for Business app by following hello instructions here:</span></span>

[<span data-ttu-id="83484-115">Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások</span><span class="sxs-lookup"><span data-stu-id="83484-115">Assign a user or group tooan enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-toodropbox-for-business"></a><span data-ttu-id="83484-116">Fontos tippek az üzleti felhasználók tooDropbox hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="83484-116">Important tips for assigning users tooDropbox for Business</span></span>

*   <span data-ttu-id="83484-117">Javasoljuk, hogy egyetlen Azure AD-felhasználó hozzá van rendelve az üzleti tootest hello konfigurálása kiosztás tooDropbox.</span><span class="sxs-lookup"><span data-stu-id="83484-117">It is recommended that a single Azure AD user is assigned tooDropbox for Business tootest hello provisioning configuration.</span></span> <span data-ttu-id="83484-118">További felhasználók és/vagy csoportok később is rendelhető.</span><span class="sxs-lookup"><span data-stu-id="83484-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="83484-119">Amikor egy felhasználó tooDropbox rendel a vállalati, ki kell választania egy érvényes felhasználói szerepkörnek.</span><span class="sxs-lookup"><span data-stu-id="83484-119">When assigning a user tooDropbox for Business, you must select a valid user role.</span></span> <span data-ttu-id="83484-120">hello "alapértelmezett" szerepkör nem működik a kiépítés...</span><span class="sxs-lookup"><span data-stu-id="83484-120">hello "Default Access" role does not work for provisioning..</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="83484-121">Az automatikus felhasználó-kiépítés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="83484-121">Enable Automated User Provisioning</span></span>

<span data-ttu-id="83484-122">Ez a szakasz végigvezeti a csatlakozás az Azure AD tooDropbox vállalat felhasználói fiók kiépítése API és a kiépítés szolgáltatás toocreate hello konfigurálása, frissítése, és tiltsa le a hozzárendelt felhasználói fiókok a Dropbox vállalati felhasználók és csoportok alapján az Azure AD-hozzárendelés.</span><span class="sxs-lookup"><span data-stu-id="83484-122">This section guides you through connecting your Azure AD tooDropbox for Business's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in Dropbox for Business based on user and group assignment in Azure AD.</span></span>

>[!Tip]
><span data-ttu-id="83484-123">Dönthet úgy is tooenabled SAML-alapú egyszeri bejelentkezést a vállalati hello megjelenő utasításokat követve Dropbox [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="83484-123">You may also choose tooenabled SAML-based Single Sign-On for Dropbox for Business, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="83484-124">Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.</span><span class="sxs-lookup"><span data-stu-id="83484-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="tooconfigure-automatic-user-account-provisioning"></a><span data-ttu-id="83484-125">tooconfigure automatikus felhasználói fiók kiépítése:</span><span class="sxs-lookup"><span data-stu-id="83484-125">tooconfigure automatic user account provisioning:</span></span>

1. <span data-ttu-id="83484-126">A hello [Azure-portálon](https://portal.azure.com), keresse meg a toohello **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="83484-126">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="83484-127">Ha már konfigurálta a vállalati Dropbox az egyszeri bejelentkezést, keresse meg az üzleti hello keresési mező Dropbox-példány.</span><span class="sxs-lookup"><span data-stu-id="83484-127">If you have already configured Dropbox for Business for single sign-on, search for your instance of Dropbox for Business using hello search field.</span></span> <span data-ttu-id="83484-128">Máskülönben válassza **Hozzáadás** keresse meg a **Dropbox vállalati** hello alkalmazás gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="83484-128">Otherwise, select **Add** and search for **Dropbox for Business** in hello application gallery.</span></span> <span data-ttu-id="83484-129">Válassza ki a vállalati Dropbox hello a keresési eredmények, és vegye fel tooyour alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="83484-129">Select Dropbox for Business from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="83484-130">Jelölje ki a Dropbox vállalati példányát, majd jelölje ki a hello **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="83484-130">Select your instance of Dropbox for Business, then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="83484-131">Set hello **kiépítési üzemmódban** túl**automatikus**.</span><span class="sxs-lookup"><span data-stu-id="83484-131">Set hello **Provisioning Mode** too**Automatic**.</span></span> 

    ![Kiépítés](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="83484-133">A hello **rendszergazdai hitelesítő adataival** kattintson **engedélyezés**.</span><span class="sxs-lookup"><span data-stu-id="83484-133">Under hello **Admin Credentials** section, click **Authorize**.</span></span> <span data-ttu-id="83484-134">A Dropbox bejelentkezési párbeszédpanel üzleti egy új böngészőablakban nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="83484-134">It opens a Dropbox for Business login dialog in a new browser window.</span></span>

6. <span data-ttu-id="83484-135">A hello **bejelentkezési tooDropbox toolink az Azure ad-val** párbeszédpanelen tooyour Dropbox üzleti bérlő bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="83484-135">On hello **Sign-in tooDropbox toolink with Azure AD** dialog, sign in tooyour Dropbox for Business tenant.</span></span>

     <span data-ttu-id="83484-136">![A felhasználók átadása](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769518.png "a felhasználók átadása")</span><span class="sxs-lookup"><span data-stu-id="83484-136">![User provisioning](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769518.png "User provisioning")</span></span>

7. <span data-ttu-id="83484-137">Győződjön meg arról, hogy szeretné-e toogive Azure Active Directory engedély toomake módosítások tooyour Dropbox üzleti bérlő számára.</span><span class="sxs-lookup"><span data-stu-id="83484-137">Confirm that you would like toogive Azure Active Directory permission toomake changes tooyour Dropbox for Business tenant.</span></span> <span data-ttu-id="83484-138">Kattintson a **engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="83484-138">Click **Allow**.</span></span>
    
      <span data-ttu-id="83484-139">![A felhasználók átadása](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769519.png "a felhasználók átadása")</span><span class="sxs-lookup"><span data-stu-id="83484-139">![User provisioning](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769519.png "User provisioning")</span></span>

8. <span data-ttu-id="83484-140">Hello Azure-portálon, kattintson **kapcsolat tesztelése** tooensure az Azure AD alkalmazás Dropbox tooyour kapcsolódhatnak.</span><span class="sxs-lookup"><span data-stu-id="83484-140">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour Dropbox for Business app.</span></span> <span data-ttu-id="83484-141">Hello létesített kapcsolat megszakad, ha a vállalati fiók Team rendszergazdai jogosultságokkal rendelkezik, győződjön meg arról, hogy a dropbox-ba, és próbálkozzon hello **"Engedélyezés"** léptessen ismét.</span><span class="sxs-lookup"><span data-stu-id="83484-141">If hello connection fails, ensure your Dropbox for Business account has Team Admin permissions and try hello **"Authorize"** step again.</span></span>

9. <span data-ttu-id="83484-142">Adja meg a hello e-mail címet vagy egy csoport létesítési hiba értesítések a hello kapjanak **értesítő e-mailt** mezőben, majd jelölje be hello jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="83484-142">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox.</span></span>

10. <span data-ttu-id="83484-143">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="83484-143">Click **Save.**</span></span>

11. <span data-ttu-id="83484-144">A hello hozzárendelések szakaszt, válassza a **szinkronizálása Azure Active Directory-felhasználók tooDropbox vállalati.**</span><span class="sxs-lookup"><span data-stu-id="83484-144">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooDropbox for Business.**</span></span>

12. <span data-ttu-id="83484-145">A hello **attribútum-leképezésekhez** szakaszban, tekintse át a vállalati szinkronizált az Azure AD tooDropbox hello felhasználói attribútumok.</span><span class="sxs-lookup"><span data-stu-id="83484-145">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooDropbox for Business.</span></span> <span data-ttu-id="83484-146">kiválasztott attribútumok hello **egyező** tulajdonságainak használt toomatch hello felhasználói fiókok a Dropbox vállalati a frissítési műveletek.</span><span class="sxs-lookup"><span data-stu-id="83484-146">hello attributes selected as **Matching** properties are used toomatch hello user accounts in Dropbox for Business for update operations.</span></span> <span data-ttu-id="83484-147">Válassza ki a hello Mentés gombra toocommit módosításokat.</span><span class="sxs-lookup"><span data-stu-id="83484-147">Select hello Save button toocommit any changes.</span></span>

13. <span data-ttu-id="83484-148">tooenable hello Azure AD létesítési szolgáltatás a Dropbox vállalati, a módosítás hello **kiépítési állapot** túl**a** hello beállítások szakaszában a</span><span class="sxs-lookup"><span data-stu-id="83484-148">tooenable hello Azure AD provisioning service for Dropbox for Business, change hello **Provisioning Status** too**On** in hello Settings section</span></span>

14. <span data-ttu-id="83484-149">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="83484-149">Click **Save.**</span></span>

<span data-ttu-id="83484-150">Hello kezdeti szinkronizálás bármely felhasználói és/vagy csoportok hello felhasználók a vállalati tooDropbox és a csoportok szakasz kezdődik.</span><span class="sxs-lookup"><span data-stu-id="83484-150">It starts hello initial synchronization of any users and/or groups assigned tooDropbox for Business in hello Users and Groups section.</span></span> <span data-ttu-id="83484-151">hello kezdeti szinkronizálás hosszabb, mint bekövetkező körülbelül 20 percenként, mindaddig, amíg hello szolgáltatás fut. ezt követő szinkronizálások tooperform vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="83484-151">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="83484-152">Használhatja a hello **szinkronizálás részleteivel** toomonitor folyamatban szakaszt, és hajtsa végre a hivatkozások tooprovisioning Tevékenységjelentések, amely alkalmazás a Dropbox a szolgáltatás kiépítését hello által végzett összes műveletet írják le.</span><span class="sxs-lookup"><span data-stu-id="83484-152">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service on your Dropbox for Business app.</span></span>

<span data-ttu-id="83484-153">Mostantól létrehozhat egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="83484-153">You can now create a test account.</span></span> <span data-ttu-id="83484-154">Várjon, amíg fel hello fiókot töltött tooverify tooDropbox szinkronizálása a vállalati too20 perc.</span><span class="sxs-lookup"><span data-stu-id="83484-154">Wait for up too20 minutes tooverify that hello account has been synchronized tooDropbox for Business.</span></span>

<span data-ttu-id="83484-155">A sikeresen befejezett felhasználók átadásához ciklus kapcsolódó állapotát jelzi.</span><span class="sxs-lookup"><span data-stu-id="83484-155">A successfully completed user provisioning cycle is indicated by a related status.</span></span>

<span data-ttu-id="83484-156">![Felhasználók hozzárendelése](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/IC769523.png "felhasználók hozzárendelése")</span><span class="sxs-lookup"><span data-stu-id="83484-156">![Assign users](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/IC769523.png "Assign users")</span></span>


## <a name="additional-resources"></a><span data-ttu-id="83484-157">További források</span><span class="sxs-lookup"><span data-stu-id="83484-157">Additional resources</span></span>

* [<span data-ttu-id="83484-158">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="83484-158">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="83484-159">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="83484-159">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="83484-160">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="83484-160">Configure Single Sign-on</span></span>](active-directory-saas-dropboxforbusiness-tutorial.md)