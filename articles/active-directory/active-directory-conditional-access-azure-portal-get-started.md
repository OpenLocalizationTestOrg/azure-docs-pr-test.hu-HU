---
title: "a feltételes hozzáférés az Azure Active Directory használatába aaaGet |} Microsoft Docs"
description: "A helyre vonatkozó feltétellel feltételes hozzáférés tesztelése."
services: active-directory
keywords: "feltételes hozzáférés tooapps, feltételes hozzáférés az Azure AD-vel biztonságos hozzáférés toocompany erőforrásokat, a feltételes hozzáférési házirendek"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/31/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 4521f5a34f5882e026f5e58a7127d8c55cba2f0b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-conditional-access-in-azure-active-directory"></a><span data-ttu-id="7ad62-104">Ismerkedés a feltételes hozzáférés az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="7ad62-104">Get started with conditional access in Azure Active Directory</span></span>

<span data-ttu-id="7ad62-105">Feltételes hozzáférés az Azure Active Directoryban, amely lehetővé teszi, hogy Ön toodefine feltételek alapján, amely engedéllyel rendelkező felhasználók férhetnek hozzá az alkalmazások képesek.</span><span class="sxs-lookup"><span data-stu-id="7ad62-105">Conditional access is a capability of Azure Active Directory that enables you toodefine conditions under which authorized users can access your apps.</span></span> 

<span data-ttu-id="7ad62-106">Ez a témakör nyújt útmutatást a hely feltétel a környezetben a feltételes hozzáférés tesztelése.</span><span class="sxs-lookup"><span data-stu-id="7ad62-106">This topic provides you with instructions for testing a conditional access based on a location condition in your environment.</span></span>  


## <a name="scenario-description"></a><span data-ttu-id="7ad62-107">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="7ad62-107">Scenario description</span></span>

<span data-ttu-id="7ad62-108">Számos szervezetben egy közös vonatkozó követelmény akkor tooonly többtényezős hitelesítés megkövetelése, hogy nem történik a vállalati intranethez hello hozzáférés tooapps.</span><span class="sxs-lookup"><span data-stu-id="7ad62-108">One common requirement in many organizations is tooonly require multi-factor authentication for access tooapps that is not performed from hello corporate intranet.</span></span> <span data-ttu-id="7ad62-109">Az Azure Active Directoryval könnyen megvalósításához a cél helyalapú feltételes hozzáférési házirend konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="7ad62-109">With Azure Active Directory, you can easily accomplish this goal by configuring a location-based conditional access policy.</span></span> <span data-ttu-id="7ad62-110">Ez a témakör részletes útmutatást kapcsolódó házirend beállítása.</span><span class="sxs-lookup"><span data-stu-id="7ad62-110">This topic provides you with detailed instructions for configuring a related policy.</span></span> <span data-ttu-id="7ad62-111">házirend használja hello [megbízható IP-címek](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips) hozzáférés a vállalati hello kísérletek közötti toodistinguish tartozó intranetes és minden egyéb helyek.</span><span class="sxs-lookup"><span data-stu-id="7ad62-111">hello policy leverages [Trusted IPs](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips) toodistinguish between access attempts made from hello corporate's intranet and all other locations.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="7ad62-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7ad62-112">Prerequisites</span></span>

<span data-ttu-id="7ad62-113">hello ebben a témakörben ismertetett forgatókönyv feltételezi, hogy Ön ismeri a leírt hello fogalmak [Azure Active Directory feltételes hozzáférés](active-directory-conditional-access-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="7ad62-113">hello scenario outlined in this topic assumes that you are familiar with hello concepts outlined in [Azure Active Directory conditional access](active-directory-conditional-access-azure-portal.md).</span></span>

<span data-ttu-id="7ad62-114">tootest ebben az esetben szüksége:</span><span class="sxs-lookup"><span data-stu-id="7ad62-114">tootest this scenario, you need to:</span></span>

- <span data-ttu-id="7ad62-115">Tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7ad62-115">Create a test user</span></span> 

- <span data-ttu-id="7ad62-116">Rendelje hozzá az Azure AD Premium licenc toohello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="7ad62-116">Assign an Azure AD Premium license toohello test user</span></span>

- <span data-ttu-id="7ad62-117">A kezelt alkalmazások konfigurálása és hozzárendelése a teszt felhasználó tooit</span><span class="sxs-lookup"><span data-stu-id="7ad62-117">Configure a managed app and assign your test user tooit</span></span>

- <span data-ttu-id="7ad62-118">Megbízható IP-címek konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7ad62-118">Configure trusted IPs</span></span>

<span data-ttu-id="7ad62-119">Ha további információt a megbízható IP-cím van szüksége, tekintse meg [megbízható IP-címek](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).</span><span class="sxs-lookup"><span data-stu-id="7ad62-119">If you need more details about Trusted IPs, see [Trusted IPs](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).</span></span>


## <a name="policy-configuration-steps"></a><span data-ttu-id="7ad62-120">Házirend konfigurációs lépések</span><span class="sxs-lookup"><span data-stu-id="7ad62-120">Policy configuration steps</span></span>

<span data-ttu-id="7ad62-121">**tooconfigure a feltételes hozzáférési házirend tegye:**</span><span class="sxs-lookup"><span data-stu-id="7ad62-121">**tooconfigure your conditional access policy, do:**</span></span>

1. <span data-ttu-id="7ad62-122">Hello hello bal oldali navigációs sávja, az Azure-portálon kattintson **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="7ad62-122">In hello Azure portal, on hello left navbar, click **Azure Active Directory**.</span></span> 

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/01.png)

2. <span data-ttu-id="7ad62-124">A hello **Azure Active Directory** paneljén, hello **kezelése** kattintson **feltételes hozzáférési**.</span><span class="sxs-lookup"><span data-stu-id="7ad62-124">On hello **Azure Active Directory** blade, in hello **Manage** section, click **Conditional access**.</span></span>

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/02.png)
 
3. <span data-ttu-id="7ad62-126">A hello **feltételes hozzáférés** panelen, tooopen hello **új** panelen hello legfelül hello eszköztáron kattintson **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="7ad62-126">On hello **Conditional Access** blade, tooopen hello **New** blade, in hello toolbar on hello top, click **Add**.</span></span>

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/03.png)

4. <span data-ttu-id="7ad62-128">A hello **új** paneljén, hello **neve** szövegmező, írja be a házirend nevét.</span><span class="sxs-lookup"><span data-stu-id="7ad62-128">On hello **New** blade, in hello **Name** textbox, type a name for your policy.</span></span>

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/04.png)

5. <span data-ttu-id="7ad62-130">A hello **hozzárendelés** kattintson **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="7ad62-130">In hello **Assignment** section, click **Users and groups**.</span></span>

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/05.png)

6. <span data-ttu-id="7ad62-132">A hello **felhasználók és csoportok** panelen, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="7ad62-132">On hello **Users and groups** blade, perform hello following steps:</span></span>

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/06.png)

    <span data-ttu-id="7ad62-134">a.</span><span class="sxs-lookup"><span data-stu-id="7ad62-134">a.</span></span> <span data-ttu-id="7ad62-135">Kattintson a **felhasználók és csoportok kiválasztása**.</span><span class="sxs-lookup"><span data-stu-id="7ad62-135">Click **Select users and groups**.</span></span>

    <span data-ttu-id="7ad62-136">b.</span><span class="sxs-lookup"><span data-stu-id="7ad62-136">b.</span></span> <span data-ttu-id="7ad62-137">Kattintson a **Kiválasztás** gombra.</span><span class="sxs-lookup"><span data-stu-id="7ad62-137">Click **Select**.</span></span>

    <span data-ttu-id="7ad62-138">c.</span><span class="sxs-lookup"><span data-stu-id="7ad62-138">c.</span></span> <span data-ttu-id="7ad62-139">A hello **válasszon** panelen, jelölje be a tesztfelhasználó számára, és kattintson **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="7ad62-139">On hello **Select** blade, select your test user, and then click **Select**.</span></span>

    <span data-ttu-id="7ad62-140">d.</span><span class="sxs-lookup"><span data-stu-id="7ad62-140">d.</span></span> <span data-ttu-id="7ad62-141">A hello **felhasználók és csoportok** panelen kattintson a **végzett**.</span><span class="sxs-lookup"><span data-stu-id="7ad62-141">On hello **Users and groups** blade, click **Done**.</span></span>

7. <span data-ttu-id="7ad62-142">A hello **új** panelen, tooopen hello **felhőalapú alkalmazásokba** paneljén, hello **hozzárendelés** területén kattintson **felhőalapú alkalmazásokba**.</span><span class="sxs-lookup"><span data-stu-id="7ad62-142">On hello **New** blade, tooopen hello **Cloud apps** blade, in hello **Assignment** section, click **Cloud apps**.</span></span>

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/07.png)

8. <span data-ttu-id="7ad62-144">A hello **felhőalapú alkalmazásokba** panelen, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="7ad62-144">On hello **Cloud apps** blade, perform hello following steps:</span></span>

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/08.png)

    <span data-ttu-id="7ad62-146">a.</span><span class="sxs-lookup"><span data-stu-id="7ad62-146">a.</span></span> <span data-ttu-id="7ad62-147">Kattintson a **alkalmazásokról**.</span><span class="sxs-lookup"><span data-stu-id="7ad62-147">Click **Select apps**.</span></span>

    <span data-ttu-id="7ad62-148">b.</span><span class="sxs-lookup"><span data-stu-id="7ad62-148">b.</span></span> <span data-ttu-id="7ad62-149">Kattintson a **Kiválasztás** gombra.</span><span class="sxs-lookup"><span data-stu-id="7ad62-149">Click **Select**.</span></span>

    <span data-ttu-id="7ad62-150">c.</span><span class="sxs-lookup"><span data-stu-id="7ad62-150">c.</span></span> <span data-ttu-id="7ad62-151">A hello **kiválasztása** panelen válassza ki a cloud app, és kattintson a **kiválasztása**.</span><span class="sxs-lookup"><span data-stu-id="7ad62-151">On hello **Select** blade, select your cloud app, and then click **Select**.</span></span>

    <span data-ttu-id="7ad62-152">d.</span><span class="sxs-lookup"><span data-stu-id="7ad62-152">d.</span></span> <span data-ttu-id="7ad62-153">A hello **felhőalapú alkalmazásokba** panelen kattintson a **végzett**.</span><span class="sxs-lookup"><span data-stu-id="7ad62-153">On hello **Cloud apps** blade, click **Done**.</span></span>

9. <span data-ttu-id="7ad62-154">A hello **új** panelen, tooopen hello **feltételek** paneljén, hello **hozzárendelés** területén kattintson **feltételek**.</span><span class="sxs-lookup"><span data-stu-id="7ad62-154">On hello **New** blade, tooopen hello **Conditions** blade, in hello **Assignment** section, click **Conditions**.</span></span>

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/09.png)

10. <span data-ttu-id="7ad62-156">A hello **feltételek** panelen, tooopen hello **helyek** panelen kattintson a **helyek**.</span><span class="sxs-lookup"><span data-stu-id="7ad62-156">On hello **Conditions** blade, tooopen hello **Locations** blade, click **Locations**.</span></span>

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/10.png)

11. <span data-ttu-id="7ad62-158">A hello **helyek** panelen, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="7ad62-158">On hello **Locations** blade, perform hello following steps:</span></span>

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/11.png)

    <span data-ttu-id="7ad62-160">a.</span><span class="sxs-lookup"><span data-stu-id="7ad62-160">a.</span></span> <span data-ttu-id="7ad62-161">A **konfigurálása**, kattintson a **Igen**.</span><span class="sxs-lookup"><span data-stu-id="7ad62-161">Under **Configure**, click **Yes**.</span></span>

    <span data-ttu-id="7ad62-162">b.</span><span class="sxs-lookup"><span data-stu-id="7ad62-162">b.</span></span> <span data-ttu-id="7ad62-163">A **Include**, kattintson a **minden hely**.</span><span class="sxs-lookup"><span data-stu-id="7ad62-163">Under **Include**, click **All locations**.</span></span>

    <span data-ttu-id="7ad62-164">c.</span><span class="sxs-lookup"><span data-stu-id="7ad62-164">c.</span></span> <span data-ttu-id="7ad62-165">Kattintson a **kizárása**, és kattintson a **a megbízható IP-címek**.</span><span class="sxs-lookup"><span data-stu-id="7ad62-165">Click **Exclude**, and then click **All trusted IPs**.</span></span>

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/12.png)

    <span data-ttu-id="7ad62-167">d.</span><span class="sxs-lookup"><span data-stu-id="7ad62-167">d.</span></span> <span data-ttu-id="7ad62-168">Kattintson a **Done** (Kész) gombra.</span><span class="sxs-lookup"><span data-stu-id="7ad62-168">Click **Done**.</span></span>

12. <span data-ttu-id="7ad62-169">A hello **feltételek** panelen kattintson a **végzett**.</span><span class="sxs-lookup"><span data-stu-id="7ad62-169">On hello **Conditions** blade, click **Done**.</span></span>

13. <span data-ttu-id="7ad62-170">A hello **új** panelen, tooopen hello **Grant** paneljén, hello **vezérlők** területén kattintson **Grant**.</span><span class="sxs-lookup"><span data-stu-id="7ad62-170">On hello **New** blade, tooopen hello **Grant** blade, in hello **Controls** section, click **Grant**.</span></span>

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/13.png)

14. <span data-ttu-id="7ad62-172">A hello **Grant** panelen, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="7ad62-172">On hello **Grant** blade, perform hello following steps:</span></span>

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/14.png)

    <span data-ttu-id="7ad62-174">a.</span><span class="sxs-lookup"><span data-stu-id="7ad62-174">a.</span></span> <span data-ttu-id="7ad62-175">Válassza ki **többtényezős hitelesítést**.</span><span class="sxs-lookup"><span data-stu-id="7ad62-175">Select **Require multi-factor authentication**.</span></span>

    <span data-ttu-id="7ad62-176">b.</span><span class="sxs-lookup"><span data-stu-id="7ad62-176">b.</span></span> <span data-ttu-id="7ad62-177">Kattintson a **Kiválasztás** gombra.</span><span class="sxs-lookup"><span data-stu-id="7ad62-177">Click **Select**.</span></span>

15. <span data-ttu-id="7ad62-178">A hello **új** panel alatt **házirend engedélyezése**, kattintson a **a**.</span><span class="sxs-lookup"><span data-stu-id="7ad62-178">On hello **New** blade, under **Enable policy**, click **On**.</span></span>

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/15.png)

16. <span data-ttu-id="7ad62-180">A hello **új** panelen kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="7ad62-180">On hello **New** blade, click **Create**.</span></span>


## <a name="testing-hello-policy"></a><span data-ttu-id="7ad62-181">Hello házirend tesztelése</span><span class="sxs-lookup"><span data-stu-id="7ad62-181">Testing hello policy</span></span>

<span data-ttu-id="7ad62-182">tootest a házirendet, hozzá kell férnie az alkalmazást az eszközről, amely:</span><span class="sxs-lookup"><span data-stu-id="7ad62-182">tootest your policy, you should access your app from a device that:</span></span> 

1. <span data-ttu-id="7ad62-183">IP-címet tartalmazza, amely megfelel a konfigurált megbízható IP-címek</span><span class="sxs-lookup"><span data-stu-id="7ad62-183">Has an IP address that is within your configured Trusted IPs</span></span> 

1. <span data-ttu-id="7ad62-184">IP-címet tartalmazza, amely kívül esik a konfigurált megbízható IP-címek</span><span class="sxs-lookup"><span data-stu-id="7ad62-184">Has an IP address that is not within your configured Trusted IPs</span></span>

<span data-ttu-id="7ad62-185">A multi-factor authentication csak kell, amely egy eszközről, amely kívül esik a konfigurált megbízható IP-címek történt kapcsolódási kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="7ad62-185">Multi-factor authentication should only be required during a connection attempt that was made from a device that is not within your configured Trusted IPs.</span></span> 


## <a name="next-steps"></a><span data-ttu-id="7ad62-186">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7ad62-186">Next steps</span></span>

<span data-ttu-id="7ad62-187">Ha szeretne további információ a feltételes hozzáférési toolearn, [Azure Active Directory feltételes hozzáférés](active-directory-conditional-access-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="7ad62-187">If you would like toolearn more about conditional access, see [Azure Active Directory conditional access](active-directory-conditional-access-azure-portal.md).</span></span>

