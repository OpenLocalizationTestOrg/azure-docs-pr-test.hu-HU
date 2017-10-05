---
title: "Az Azure Active Directoryban feltételes hozzáférés – első lépések |} Microsoft Docs"
description: "A helyre vonatkozó feltétellel feltételes hozzáférés tesztelése."
services: active-directory
keywords: "alkalmazások, a feltételes hozzáférés az Azure ad-vel, a biztonságos hozzáférés a vállalati erőforrásokhoz, a feltételes hozzáférési házirendekkel a feltételes hozzáférés"
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
ms.openlocfilehash: cd53e8be32d1e98aaf9f72177895871dba69df86
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="get-started-with-conditional-access-in-azure-active-directory"></a><span data-ttu-id="1ee34-104">Ismerkedés a feltételes hozzáférés az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="1ee34-104">Get started with conditional access in Azure Active Directory</span></span>

<span data-ttu-id="1ee34-105">Feltételes hozzáférés az Azure Active Directoryban, amely lehetővé teszi a feltételeket, amely alatt engedéllyel rendelkező felhasználók férhetnek hozzá az alkalmazások képesek.</span><span class="sxs-lookup"><span data-stu-id="1ee34-105">Conditional access is a capability of Azure Active Directory that enables you to define conditions under which authorized users can access your apps.</span></span> 

<span data-ttu-id="1ee34-106">Ez a témakör nyújt útmutatást a hely feltétel a környezetben a feltételes hozzáférés tesztelése.</span><span class="sxs-lookup"><span data-stu-id="1ee34-106">This topic provides you with instructions for testing a conditional access based on a location condition in your environment.</span></span>  


## <a name="scenario-description"></a><span data-ttu-id="1ee34-107">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="1ee34-107">Scenario description</span></span>

<span data-ttu-id="1ee34-108">Számos szervezetben egy közös követelmény-hoz csak többtényezős hitelesítés megkövetelése az alkalmazásokhoz való hozzáférés, amely nem történik a vállalati intranethez.</span><span class="sxs-lookup"><span data-stu-id="1ee34-108">One common requirement in many organizations is to only require multi-factor authentication for access to apps that is not performed from the corporate intranet.</span></span> <span data-ttu-id="1ee34-109">Az Azure Active Directoryval könnyen megvalósításához a cél helyalapú feltételes hozzáférési házirend konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="1ee34-109">With Azure Active Directory, you can easily accomplish this goal by configuring a location-based conditional access policy.</span></span> <span data-ttu-id="1ee34-110">Ez a témakör részletes útmutatást kapcsolódó házirend beállítása.</span><span class="sxs-lookup"><span data-stu-id="1ee34-110">This topic provides you with detailed instructions for configuring a related policy.</span></span> <span data-ttu-id="1ee34-111">A házirend használja [megbízható IP-címek](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips) kérés érkezett a vállalati hozzáférés kísérletek közötti különbséget tenni a következőre intranetes és minden egyéb helyek.</span><span class="sxs-lookup"><span data-stu-id="1ee34-111">The policy leverages [Trusted IPs](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips) to distinguish between access attempts made from the corporate's intranet and all other locations.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="1ee34-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1ee34-112">Prerequisites</span></span>

<span data-ttu-id="1ee34-113">A témakörben ismertetett forgatókönyv feltételezi, hogy Ön ismeri a ismertetett fogalmakat [Azure Active Directory feltételes hozzáférés](active-directory-conditional-access-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1ee34-113">The scenario outlined in this topic assumes that you are familiar with the concepts outlined in [Azure Active Directory conditional access](active-directory-conditional-access-azure-portal.md).</span></span>

<span data-ttu-id="1ee34-114">Ez a forgatókönyv teszteléséhez kell:</span><span class="sxs-lookup"><span data-stu-id="1ee34-114">To test this scenario, you need to:</span></span>

- <span data-ttu-id="1ee34-115">Tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="1ee34-115">Create a test user</span></span> 

- <span data-ttu-id="1ee34-116">Az Azure AD Premium licenc hozzárendelése a tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="1ee34-116">Assign an Azure AD Premium license to the test user</span></span>

- <span data-ttu-id="1ee34-117">Konfigurálhatja egy felügyelt alkalmazást, és rendelje hozzá a tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="1ee34-117">Configure a managed app and assign your test user to it</span></span>

- <span data-ttu-id="1ee34-118">Megbízható IP-címek konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1ee34-118">Configure trusted IPs</span></span>

<span data-ttu-id="1ee34-119">Ha további információt a megbízható IP-cím van szüksége, tekintse meg [megbízható IP-címek](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).</span><span class="sxs-lookup"><span data-stu-id="1ee34-119">If you need more details about Trusted IPs, see [Trusted IPs](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).</span></span>


## <a name="policy-configuration-steps"></a><span data-ttu-id="1ee34-120">Házirend konfigurációs lépések</span><span class="sxs-lookup"><span data-stu-id="1ee34-120">Policy configuration steps</span></span>

<span data-ttu-id="1ee34-121">**A feltételes hozzáférési házirend konfigurálásához hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="1ee34-121">**To configure your conditional access policy, do:**</span></span>

1. <span data-ttu-id="1ee34-122">Kattintson a bal oldali navigációs sávja, az Azure-portálon **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="1ee34-122">In the Azure portal, on the left navbar, click **Azure Active Directory**.</span></span> 

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/01.png)

2. <span data-ttu-id="1ee34-124">A a **Azure Active Directory** panelen, a a **kezelése** kattintson **feltételes hozzáférés**.</span><span class="sxs-lookup"><span data-stu-id="1ee34-124">On the **Azure Active Directory** blade, in the **Manage** section, click **Conditional access**.</span></span>

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/02.png)
 
3. <span data-ttu-id="1ee34-126">Az a **feltételes hozzáférési** panelt, nyissa meg a **új** panelen, a felső eszköztáron kattintson **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="1ee34-126">On the **Conditional Access** blade, to open the **New** blade, in the toolbar on the top, click **Add**.</span></span>

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/03.png)

4. <span data-ttu-id="1ee34-128">Az a **új** panelen, a a **neve** szövegmező, írja be a házirend nevét.</span><span class="sxs-lookup"><span data-stu-id="1ee34-128">On the **New** blade, in the **Name** textbox, type a name for your policy.</span></span>

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/04.png)

5. <span data-ttu-id="1ee34-130">Az a **hozzárendelés** kattintson **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="1ee34-130">In the **Assignment** section, click **Users and groups**.</span></span>

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/05.png)

6. <span data-ttu-id="1ee34-132">Az a **felhasználók és csoportok** panelen végezze el a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1ee34-132">On the **Users and groups** blade, perform the following steps:</span></span>

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/06.png)

    <span data-ttu-id="1ee34-134">a.</span><span class="sxs-lookup"><span data-stu-id="1ee34-134">a.</span></span> <span data-ttu-id="1ee34-135">Kattintson a **felhasználók és csoportok kiválasztása**.</span><span class="sxs-lookup"><span data-stu-id="1ee34-135">Click **Select users and groups**.</span></span>

    <span data-ttu-id="1ee34-136">b.</span><span class="sxs-lookup"><span data-stu-id="1ee34-136">b.</span></span> <span data-ttu-id="1ee34-137">Kattintson a **Kiválasztás** gombra.</span><span class="sxs-lookup"><span data-stu-id="1ee34-137">Click **Select**.</span></span>

    <span data-ttu-id="1ee34-138">c.</span><span class="sxs-lookup"><span data-stu-id="1ee34-138">c.</span></span> <span data-ttu-id="1ee34-139">Az a **válasszon** panelen, jelölje be a tesztfelhasználó számára, és kattintson **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="1ee34-139">On the **Select** blade, select your test user, and then click **Select**.</span></span>

    <span data-ttu-id="1ee34-140">d.</span><span class="sxs-lookup"><span data-stu-id="1ee34-140">d.</span></span> <span data-ttu-id="1ee34-141">Az a **felhasználók és csoportok** panelen kattintson a **végzett**.</span><span class="sxs-lookup"><span data-stu-id="1ee34-141">On the **Users and groups** blade, click **Done**.</span></span>

7. <span data-ttu-id="1ee34-142">Az a **új** panelt, nyissa meg a **felhőalapú alkalmazásokba** panelen, a a **hozzárendelés** területen kattintson **felhőalapú alkalmazásokba**.</span><span class="sxs-lookup"><span data-stu-id="1ee34-142">On the **New** blade, to open the **Cloud apps** blade, in the **Assignment** section, click **Cloud apps**.</span></span>

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/07.png)

8. <span data-ttu-id="1ee34-144">Az a **felhőalapú alkalmazásokba** panelen végezze el a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1ee34-144">On the **Cloud apps** blade, perform the following steps:</span></span>

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/08.png)

    <span data-ttu-id="1ee34-146">a.</span><span class="sxs-lookup"><span data-stu-id="1ee34-146">a.</span></span> <span data-ttu-id="1ee34-147">Kattintson a **alkalmazásokról**.</span><span class="sxs-lookup"><span data-stu-id="1ee34-147">Click **Select apps**.</span></span>

    <span data-ttu-id="1ee34-148">b.</span><span class="sxs-lookup"><span data-stu-id="1ee34-148">b.</span></span> <span data-ttu-id="1ee34-149">Kattintson a **Kiválasztás** gombra.</span><span class="sxs-lookup"><span data-stu-id="1ee34-149">Click **Select**.</span></span>

    <span data-ttu-id="1ee34-150">c.</span><span class="sxs-lookup"><span data-stu-id="1ee34-150">c.</span></span> <span data-ttu-id="1ee34-151">A a **válasszon** panelen válassza ki a cloud app, és kattintson a **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="1ee34-151">On the **Select** blade, select your cloud app, and then click **Select**.</span></span>

    <span data-ttu-id="1ee34-152">d.</span><span class="sxs-lookup"><span data-stu-id="1ee34-152">d.</span></span> <span data-ttu-id="1ee34-153">Az a **felhőalapú alkalmazásokba** panelen kattintson a **végzett**.</span><span class="sxs-lookup"><span data-stu-id="1ee34-153">On the **Cloud apps** blade, click **Done**.</span></span>

9. <span data-ttu-id="1ee34-154">Az a **új** panelt, nyissa meg a **feltételek** panelen, a a **hozzárendelés** területen kattintson **feltételek**.</span><span class="sxs-lookup"><span data-stu-id="1ee34-154">On the **New** blade, to open the **Conditions** blade, in the **Assignment** section, click **Conditions**.</span></span>

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/09.png)

10. <span data-ttu-id="1ee34-156">A a **feltételek** panelt, nyissa meg a **helyek** panelen kattintson a **helyek**.</span><span class="sxs-lookup"><span data-stu-id="1ee34-156">On the **Conditions** blade, to open the **Locations** blade, click **Locations**.</span></span>

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/10.png)

11. <span data-ttu-id="1ee34-158">Az a **helyek** panelen végezze el a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1ee34-158">On the **Locations** blade, perform the following steps:</span></span>

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/11.png)

    <span data-ttu-id="1ee34-160">a.</span><span class="sxs-lookup"><span data-stu-id="1ee34-160">a.</span></span> <span data-ttu-id="1ee34-161">A **konfigurálása**, kattintson a **Igen**.</span><span class="sxs-lookup"><span data-stu-id="1ee34-161">Under **Configure**, click **Yes**.</span></span>

    <span data-ttu-id="1ee34-162">b.</span><span class="sxs-lookup"><span data-stu-id="1ee34-162">b.</span></span> <span data-ttu-id="1ee34-163">A **Include**, kattintson a **minden hely**.</span><span class="sxs-lookup"><span data-stu-id="1ee34-163">Under **Include**, click **All locations**.</span></span>

    <span data-ttu-id="1ee34-164">c.</span><span class="sxs-lookup"><span data-stu-id="1ee34-164">c.</span></span> <span data-ttu-id="1ee34-165">Kattintson a **kizárása**, és kattintson a **a megbízható IP-címek**.</span><span class="sxs-lookup"><span data-stu-id="1ee34-165">Click **Exclude**, and then click **All trusted IPs**.</span></span>

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/12.png)

    <span data-ttu-id="1ee34-167">d.</span><span class="sxs-lookup"><span data-stu-id="1ee34-167">d.</span></span> <span data-ttu-id="1ee34-168">Kattintson a **Done** (Kész) gombra.</span><span class="sxs-lookup"><span data-stu-id="1ee34-168">Click **Done**.</span></span>

12. <span data-ttu-id="1ee34-169">Az a **feltételek** panelen kattintson a **végzett**.</span><span class="sxs-lookup"><span data-stu-id="1ee34-169">On the **Conditions** blade, click **Done**.</span></span>

13. <span data-ttu-id="1ee34-170">Az a **új** panelt, nyissa meg a **Grant** panelen, a a **vezérlők** területen kattintson **Grant**.</span><span class="sxs-lookup"><span data-stu-id="1ee34-170">On the **New** blade, to open the **Grant** blade, in the **Controls** section, click **Grant**.</span></span>

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/13.png)

14. <span data-ttu-id="1ee34-172">Az a **Grant** panelen végezze el a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1ee34-172">On the **Grant** blade, perform the following steps:</span></span>

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/14.png)

    <span data-ttu-id="1ee34-174">a.</span><span class="sxs-lookup"><span data-stu-id="1ee34-174">a.</span></span> <span data-ttu-id="1ee34-175">Válassza ki **többtényezős hitelesítést**.</span><span class="sxs-lookup"><span data-stu-id="1ee34-175">Select **Require multi-factor authentication**.</span></span>

    <span data-ttu-id="1ee34-176">b.</span><span class="sxs-lookup"><span data-stu-id="1ee34-176">b.</span></span> <span data-ttu-id="1ee34-177">Kattintson a **Kiválasztás** gombra.</span><span class="sxs-lookup"><span data-stu-id="1ee34-177">Click **Select**.</span></span>

15. <span data-ttu-id="1ee34-178">Az a **új** panel alatt **házirend engedélyezése**, kattintson a **a**.</span><span class="sxs-lookup"><span data-stu-id="1ee34-178">On the **New** blade, under **Enable policy**, click **On**.</span></span>

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/15.png)

16. <span data-ttu-id="1ee34-180">Az a **új** panelen kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="1ee34-180">On the **New** blade, click **Create**.</span></span>


## <a name="testing-the-policy"></a><span data-ttu-id="1ee34-181">A házirendek tesztelésével</span><span class="sxs-lookup"><span data-stu-id="1ee34-181">Testing the policy</span></span>

<span data-ttu-id="1ee34-182">A házirend teszteléséhez az eszközről kell hozzáférést az alkalmazás, amely:</span><span class="sxs-lookup"><span data-stu-id="1ee34-182">To test your policy, you should access your app from a device that:</span></span> 

1. <span data-ttu-id="1ee34-183">IP-címet tartalmazza, amely megfelel a konfigurált megbízható IP-címek</span><span class="sxs-lookup"><span data-stu-id="1ee34-183">Has an IP address that is within your configured Trusted IPs</span></span> 

1. <span data-ttu-id="1ee34-184">IP-címet tartalmazza, amely kívül esik a konfigurált megbízható IP-címek</span><span class="sxs-lookup"><span data-stu-id="1ee34-184">Has an IP address that is not within your configured Trusted IPs</span></span>

<span data-ttu-id="1ee34-185">A multi-factor authentication csak kell, amely egy eszközről, amely kívül esik a konfigurált megbízható IP-címek történt kapcsolódási kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="1ee34-185">Multi-factor authentication should only be required during a connection attempt that was made from a device that is not within your configured Trusted IPs.</span></span> 


## <a name="next-steps"></a><span data-ttu-id="1ee34-186">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1ee34-186">Next steps</span></span>

<span data-ttu-id="1ee34-187">Ha azt szeretné, további információ a feltételes hozzáférési, lásd: [Azure Active Directory feltételes hozzáférés](active-directory-conditional-access-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1ee34-187">If you would like to learn more about conditional access, see [Azure Active Directory conditional access](active-directory-conditional-access-azure-portal.md).</span></span>

