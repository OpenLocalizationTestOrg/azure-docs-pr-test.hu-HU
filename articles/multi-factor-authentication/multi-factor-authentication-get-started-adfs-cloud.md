---
title: "A felhőerőforrások védelme az Azure MFA és AD FS szolgáltatással | Microsoft Docs"
description: "Ez az Azure Multi-Factor Authentication-oldal leírja, hogyan kezdheti el az Azure MFA és az AD FS használatát a felhőben."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 0927fc67-8090-4fdd-913a-b3cfed3fbe77
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/29/2017
ms.author: kgremban
ms.openlocfilehash: 6cf4ec4f777ea1f2b852945ab82da2547946f378
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="securing-cloud-resources-with-azure-multi-factor-authentication-and-ad-fs"></a><span data-ttu-id="3a3e2-103">A felhőerőforrások védelme Azure Multi-Factor Authentication hitelesítéssel és AD FS-sel</span><span class="sxs-lookup"><span data-stu-id="3a3e2-103">Securing cloud resources with Azure Multi-Factor Authentication and AD FS</span></span>
<span data-ttu-id="3a3e2-104">Ha a szervezete Azure Active Directory-összevonást használ, és az Azure AD által elért erőforrásokkal rendelkezik, az Azure Multi-Factor Authentication segítségével vagy az Active Directory összevonási szolgáltatásokkal (AD FS) védheti meg ezeket az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-104">If your organization is federated with Azure Active Directory, use Azure Multi-Factor Authentication or Active Directory Federation Services (AD FS) to secure resources that are accessed by Azure AD.</span></span> <span data-ttu-id="3a3e2-105">Az alábbi eljárásokkal védheti meg az Azure Active Directory-erőforrásokat az Azure Multi-Factor Authentication segítségével vagy az Active Directory összevonási szolgáltatásokkal.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-105">Use the following procedures to secure Azure Active Directory resources with either Azure Multi-Factor Authentication or Active Directory Federation Services.</span></span>

## <a name="secure-azure-ad-resources-using-ad-fs"></a><span data-ttu-id="3a3e2-106">Az Azure AD-erőforrások AD FS-sel való védelme</span><span class="sxs-lookup"><span data-stu-id="3a3e2-106">Secure Azure AD resources using AD FS</span></span>
<span data-ttu-id="3a3e2-107">A felhőszolgáltatás biztosításához állítson be egy jogcímszabályt, hogy az Active Directory összevonási szolgáltatások a multipleauthn jogcímet adja ki, amikor egy felhasználó sikeresen végez kétlépéses ellenőrzést.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-107">To secure your cloud resource, set up a claims rule so that Active Directory Federation Services emits the multipleauthn claim when a user performs two-step verification successfully.</span></span> <span data-ttu-id="3a3e2-108">Ez a jogcím átkerül az Azure AD-re.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-108">This claim is passed on to Azure AD.</span></span> <span data-ttu-id="3a3e2-109">Az alábbi eljárás bemutatja ennek lépéseit:</span><span class="sxs-lookup"><span data-stu-id="3a3e2-109">Follow this procedure to walk through the steps:</span></span>


1. <span data-ttu-id="3a3e2-110">Nyissa meg az AD FS felügyeleti konzolt.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-110">Open AD FS Management.</span></span>
2. <span data-ttu-id="3a3e2-111">A bal oldalon válassza a **Függő entitás megbízhatóságai** elemet.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-111">On the left, select **Relying Party Trusts**.</span></span>
3. <span data-ttu-id="3a3e2-112">Kattintson a jobb gombbal a **Microsoft Office 365 Identity Platform** elemre, és válassza a **Jogcímszabályok szerkesztése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-112">Right-click on **Microsoft Office 365 Identity Platform** and select **Edit Claim Rules**.</span></span>

   ![Felhő](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)

4. <span data-ttu-id="3a3e2-114">A Kiadás átalakítási szabályai részben kattintson a **Szabály hozzáadása** elemre.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-114">On Issuance Transform Rules, click **Add Rule**.</span></span>

   ![Felhő](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)

5. <span data-ttu-id="3a3e2-116">Az Átalakítási jogcímszabály hozzáadása varázslóban válassza a **Bejövő jogcím továbbítása vagy szűrése** elemet a legördülő menüből, majd kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-116">On the Add Transform Claim Rule Wizard, select **Pass Through or Filter an Incoming Claim** from the drop-down and click **Next**.</span></span>

   ![Felhő](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)

6. <span data-ttu-id="3a3e2-118">Adjon nevet a szabálynak.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-118">Give your rule a name.</span></span> 
7. <span data-ttu-id="3a3e2-119">Válassza a **Hitelesítési módszerek hivatkozásai** lehetőséget a Bejövő jogcím típusaként.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-119">Select **Authentication Methods References** as the Incoming claim type.</span></span>
8. <span data-ttu-id="3a3e2-120">Válassza **Az összes jogcímérték továbbítása** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-120">Select **Pass through all claim values**.</span></span>
    <span data-ttu-id="3a3e2-121">![Átalakítási jogcímszabály hozzáadása varázsló](./media/multi-factor-authentication-get-started-adfs-cloud/configurewizard.png)</span><span class="sxs-lookup"><span data-stu-id="3a3e2-121">![Add Transform Claim Rule Wizard](./media/multi-factor-authentication-get-started-adfs-cloud/configurewizard.png)</span></span>
9. <span data-ttu-id="3a3e2-122">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-122">Click **Finish**.</span></span> <span data-ttu-id="3a3e2-123">Zárja be az AD FS felügyeleti konzolt.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-123">Close the AD FS Management console.</span></span>

## <a name="trusted-ips-for-federated-users"></a><span data-ttu-id="3a3e2-124">Megbízható IP-címek összevont felhasználók számára</span><span class="sxs-lookup"><span data-stu-id="3a3e2-124">Trusted IPs for federated users</span></span>
<span data-ttu-id="3a3e2-125">Az adminisztrátorok a megbízható IP-címek segítségével megkerülhetik a kétlépéses ellenőrzést olyan IP-címek vagy összevont felhasználók esetében, akiknek a kérései a saját intranetes hálózatukról származnak.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-125">Trusted IPs allow administrators to by-pass two-step verification for specific IP addresses, or for federated users that have requests originating from within their own intranet.</span></span> <span data-ttu-id="3a3e2-126">A következő szakaszok leírják az Azure Multi-Factor Authentication megbízható IP-címei és az összevont felhasználók konfigurálását, valamint a kétlépéses ellenőrzés megkerülését, amikor egy kérés összevont felhasználó intranetes hálózatáról származik.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-126">The following sections describe how to configure Azure Multi-Factor Authentication Trusted IPs with federated users and by-pass two-step verification when a request originates from within a federated users intranet.</span></span> <span data-ttu-id="3a3e2-127">Ehhez úgy kell konfigurálni az AD FS-t, hogy áteresztést vagy a bejövő jogcímsablonok szűrését használja a vállalati hálózaton belüli jogcímtípushoz.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-127">This is achieved by configuring AD FS to use a pass-through or filter an incoming claim template with the Inside Corporate Network claim type.</span></span>

<span data-ttu-id="3a3e2-128">Ez a példa az Office 365-öt használja a függőentitás-megbízhatóságokhoz.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-128">This example uses Office 365 for our Relying Party Trusts.</span></span>

### <a name="configure-the-ad-fs-claims-rules"></a><span data-ttu-id="3a3e2-129">Az AD FS-jogcímszabályok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3a3e2-129">Configure the AD FS claims rules</span></span>
<span data-ttu-id="3a3e2-130">Az első lépés az AD FS-jogcímek konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-130">The first thing we need to do is to configure the AD FS claims.</span></span> <span data-ttu-id="3a3e2-131">Két jogcímszabályt hozzon létre: egyet a vállalati hálózaton belüli jogcímtípushoz és egy másikat ahhoz, hogy a felhasználók bejelentkezve maradjanak.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-131">Create two claims rules, one for the Inside Corporate Network claim type and an additional one for keeping our users signed in.</span></span>

1. <span data-ttu-id="3a3e2-132">Nyissa meg az AD FS felügyeleti konzolt.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-132">Open AD FS Management.</span></span>
2. <span data-ttu-id="3a3e2-133">A bal oldalon válassza a **Függő entitás megbízhatóságai** elemet.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-133">On the left, select **Relying Party Trusts**.</span></span>
3. <span data-ttu-id="3a3e2-134">Középen kattintson a jobb gombbal a **Microsoft Office 365 Identity Platform** elemre, és válassza a **Jogcímszabályok szerkesztése…** lehetőséget.
   ![Felhő](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)</span><span class="sxs-lookup"><span data-stu-id="3a3e2-134">Right-click on **Microsoft Office 365 Identity Platform** and select **Edit Claim Rules…**
![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)</span></span>
4. <span data-ttu-id="3a3e2-135">A Kiadás átalakítási szabályai részben kattintson a **Szabály hozzáadása** lehetőségre.
   ![Felhő](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)</span><span class="sxs-lookup"><span data-stu-id="3a3e2-135">On Issuance Transform Rules, click **Add Rule.**
![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)</span></span>
5. <span data-ttu-id="3a3e2-136">Az Átalakítási jogcímszabály hozzáadása varázslóban válassza a **Bejövő jogcím továbbítása vagy szűrése** elemet a legördülő menüből, majd kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-136">On the Add Transform Claim Rule Wizard, select **Pass Through or Filter an Incoming Claim** from the drop-down and click **Next**.</span></span>
   <span data-ttu-id="3a3e2-137">![Felhő](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)</span><span class="sxs-lookup"><span data-stu-id="3a3e2-137">![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)</span></span>
6. <span data-ttu-id="3a3e2-138">A Jogcímszabály neve melletti mezőben adjon nevet a szabálynak.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-138">In the box next to Claim rule name, give your rule a name.</span></span> <span data-ttu-id="3a3e2-139">Például: InsideCorpNet.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-139">For example: InsideCorpNet.</span></span>
7. <span data-ttu-id="3a3e2-140">A Bejövő jogcím típusa melletti legördülő listából válassza a **Vállalati hálózaton belül** elemet.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-140">From the drop-down, next to Incoming claim type, select **Inside Corporate Network**.</span></span>
   <span data-ttu-id="3a3e2-141">![Felhő](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip4.png)</span><span class="sxs-lookup"><span data-stu-id="3a3e2-141">![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip4.png)</span></span>
8. <span data-ttu-id="3a3e2-142">Kattintson a **Finish** (Befejezés) gombra.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-142">Click **Finish**.</span></span>
9. <span data-ttu-id="3a3e2-143">A Kiadás átalakítási szabályai részben kattintson a **Szabály hozzáadása** elemre.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-143">On Issuance Transform Rules, click **Add Rule**.</span></span>
10. <span data-ttu-id="3a3e2-144">Az Átalakítási jogcímszabály hozzáadása varázslóban válassza a **Jogcímek küldése egyéni szabállyal** elemet a legördülő menüből, és kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-144">On the Add Transform Claim Rule Wizard, select **Send Claims Using a Custom Rule** from the drop-down and click **Next**.</span></span>
11. <span data-ttu-id="3a3e2-145">A Jogcímszabály neve: alatti mezőbe írja be a következőt: *Keep Users Signed In* (A felhasználók maradjanak bejelentkezve).</span><span class="sxs-lookup"><span data-stu-id="3a3e2-145">In the box under Claim rule name: enter *Keep Users Signed In*.</span></span>
12. <span data-ttu-id="3a3e2-146">Az Egyéni szabály mezőbe írja be a következőt:</span><span class="sxs-lookup"><span data-stu-id="3a3e2-146">In the Custom rule box, enter:</span></span>

        c:[Type == "http://schemas.microsoft.com/2014/03/psso"]
            => issue(claim = c);
    ![Felhő](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip5.png)
13. <span data-ttu-id="3a3e2-148">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-148">Click **Finish**.</span></span>
14. <span data-ttu-id="3a3e2-149">Kattintson az **Alkalmaz** gombra.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-149">Click **Apply**.</span></span>
15. <span data-ttu-id="3a3e2-150">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-150">Click **Ok**.</span></span>
16. <span data-ttu-id="3a3e2-151">Zárja be az AD FS felügyeleti konzolt.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-151">Close AD FS Management.</span></span>

### <a name="configure-azure-multi-factor-authentication-trusted-ips-with-federated-users"></a><span data-ttu-id="3a3e2-152">Azure Multi-Factor Authentication megbízható IP-címeinek konfigurálása összevont felhasználókkal</span><span class="sxs-lookup"><span data-stu-id="3a3e2-152">Configure Azure Multi-Factor Authentication Trusted IPs with Federated Users</span></span>
<span data-ttu-id="3a3e2-153">Most, hogy megvannak a jogcímek, konfigurálhatjuk a megbízható IP-címeket.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-153">Now that the claims are in place, we can configure trusted IPs.</span></span>

1. <span data-ttu-id="3a3e2-154">Jelentkezzen be a [klasszikus Azure portálra](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="3a3e2-154">Sign in to the [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="3a3e2-155">A bal oldalon kattintson az **Active Directory** elemre.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-155">On the left, click **Active Directory**.</span></span>
3. <span data-ttu-id="3a3e2-156">A Címtár területen válassza ki a címtárat, ahová a megbízható IP-címeket be szeretné állítani.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-156">Under Directory, select the directory where you want to set up trusted IPs.</span></span>
4. <span data-ttu-id="3a3e2-157">A kiválasztott címtár esetében kattintson a **Konfigurálás** elemre.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-157">On the Directory you have selected, click **Configure**.</span></span>
5. <span data-ttu-id="3a3e2-158">A Multi-Factor Authentication szakaszban kattintson a **Szolgáltatásbeállítások kezelése** elemre.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-158">In the multi-factor authentication section, click **Manage service settings**.</span></span>
6. <span data-ttu-id="3a3e2-159">A Szolgáltatásbeállítások oldalon, a Megbízható IP-címek területen jelölje be a **Többtényezős hitelesítés kihagyása az összevont felhasználók intranetről indított kérelmei esetén** elemet.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-159">On the Service Settings page, under trusted IPs, select **Skip multi-factor-authentication for requests from federated users on my intranet**.</span></span>  

   ![Felhő](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip6.png)
   
7. <span data-ttu-id="3a3e2-161">Kattintson a **mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-161">Click **save**.</span></span>
8. <span data-ttu-id="3a3e2-162">A frissítések alkalmazása után kattintson a **bezárás** gombra.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-162">Once the updates have been applied, click **close**.</span></span>

<span data-ttu-id="3a3e2-163">Készen is van.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-163">That’s it!</span></span> <span data-ttu-id="3a3e2-164">Ekkor az összevont Office 365-felhasználóknak csak az MFA-t kell használniuk, amikor egy jogcím a vállalati intraneten kívülről származik.</span><span class="sxs-lookup"><span data-stu-id="3a3e2-164">At this point, federated Office 365 users should only have to use MFA when a claim originates from outside the corporate intranet.</span></span>
