---
title: "az Azure MFA és az AD FS aaaSecure felhőalapú erőforrások |} Microsoft Docs"
description: "Ez a hello Azure többtényezős hitelesítés lap, amely leírja, hogyan tooget lépések az Azure MFA és az AD FS hello felhőben."
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
ms.openlocfilehash: 8d38d6a4af63ddcaf0fefded0b73d82d5178aa36
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="securing-cloud-resources-with-azure-multi-factor-authentication-and-ad-fs"></a><span data-ttu-id="aebab-103">A felhőerőforrások védelme Azure Multi-Factor Authentication hitelesítéssel és AD FS-sel</span><span class="sxs-lookup"><span data-stu-id="aebab-103">Securing cloud resources with Azure Multi-Factor Authentication and AD FS</span></span>
<span data-ttu-id="aebab-104">Ha a szervezet össze van vonva az Azure Active Directory, használja az Azure multi-factor Authentication vagy Active Directory összevonási szolgáltatások (AD FS) toosecure erőforrások az Azure AD által elért.</span><span class="sxs-lookup"><span data-stu-id="aebab-104">If your organization is federated with Azure Active Directory, use Azure Multi-Factor Authentication or Active Directory Federation Services (AD FS) toosecure resources that are accessed by Azure AD.</span></span> <span data-ttu-id="aebab-105">A következő eljárások toosecure Azure Active Directory-erőforrások Azure multi-factor Authentication vagy Active Directory összevonási szolgáltatások hello használata.</span><span class="sxs-lookup"><span data-stu-id="aebab-105">Use hello following procedures toosecure Azure Active Directory resources with either Azure Multi-Factor Authentication or Active Directory Federation Services.</span></span>

## <a name="secure-azure-ad-resources-using-ad-fs"></a><span data-ttu-id="aebab-106">Az Azure AD-erőforrások AD FS-sel való védelme</span><span class="sxs-lookup"><span data-stu-id="aebab-106">Secure Azure AD resources using AD FS</span></span>
<span data-ttu-id="aebab-107">toosecure a felhő erőforrás olyan jogcímeket szabályt beállítani, hogy az Active Directory összevonási szolgáltatások hello multipleauthn jogcím bocsát ki, amikor a felhasználó sikeresen elvégzi a kétlépéses ellenőrzést.</span><span class="sxs-lookup"><span data-stu-id="aebab-107">toosecure your cloud resource, set up a claims rule so that Active Directory Federation Services emits hello multipleauthn claim when a user performs two-step verification successfully.</span></span> <span data-ttu-id="aebab-108">A jogcím átadódik tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="aebab-108">This claim is passed on tooAzure AD.</span></span> <span data-ttu-id="aebab-109">Ez az eljárás toowalk hello lépéseit követve:</span><span class="sxs-lookup"><span data-stu-id="aebab-109">Follow this procedure toowalk through hello steps:</span></span>


1. <span data-ttu-id="aebab-110">Nyissa meg az AD FS felügyeleti konzolt.</span><span class="sxs-lookup"><span data-stu-id="aebab-110">Open AD FS Management.</span></span>
2. <span data-ttu-id="aebab-111">Hello bal oldalon válassza ki a **függő entitás Megbízhatóságai**.</span><span class="sxs-lookup"><span data-stu-id="aebab-111">On hello left, select **Relying Party Trusts**.</span></span>
3. <span data-ttu-id="aebab-112">Kattintson a jobb gombbal a **Microsoft Office 365 Identity Platform** elemre, és válassza a **Jogcímszabályok szerkesztése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="aebab-112">Right-click on **Microsoft Office 365 Identity Platform** and select **Edit Claim Rules**.</span></span>

   ![Felhő](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)

4. <span data-ttu-id="aebab-114">A Kiadás átalakítási szabályai részben kattintson a **Szabály hozzáadása** elemre.</span><span class="sxs-lookup"><span data-stu-id="aebab-114">On Issuance Transform Rules, click **Add Rule**.</span></span>

   ![Felhő](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)

5. <span data-ttu-id="aebab-116">Az átalakítási Jogcímszabály szabály varázsló hello, válassza a **továbbítása vagy szűrése egy bejövő jogcím** hello legördülő listán, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="aebab-116">On hello Add Transform Claim Rule Wizard, select **Pass Through or Filter an Incoming Claim** from hello drop-down and click **Next**.</span></span>

   ![Felhő](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)

6. <span data-ttu-id="aebab-118">Adjon nevet a szabálynak.</span><span class="sxs-lookup"><span data-stu-id="aebab-118">Give your rule a name.</span></span> 
7. <span data-ttu-id="aebab-119">Válassza ki **hitelesítési módszerek hivatkozásai** , hello bejövő jogcím típusa.</span><span class="sxs-lookup"><span data-stu-id="aebab-119">Select **Authentication Methods References** as hello Incoming claim type.</span></span>
8. <span data-ttu-id="aebab-120">Válassza **Az összes jogcímérték továbbítása** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="aebab-120">Select **Pass through all claim values**.</span></span>
    <span data-ttu-id="aebab-121">![Átalakítási jogcímszabály hozzáadása varázsló](./media/multi-factor-authentication-get-started-adfs-cloud/configurewizard.png)</span><span class="sxs-lookup"><span data-stu-id="aebab-121">![Add Transform Claim Rule Wizard](./media/multi-factor-authentication-get-started-adfs-cloud/configurewizard.png)</span></span>
9. <span data-ttu-id="aebab-122">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="aebab-122">Click **Finish**.</span></span> <span data-ttu-id="aebab-123">Zárja be a hello AD FS felügyeleti konzolon.</span><span class="sxs-lookup"><span data-stu-id="aebab-123">Close hello AD FS Management console.</span></span>

## <a name="trusted-ips-for-federated-users"></a><span data-ttu-id="aebab-124">Megbízható IP-címek összevont felhasználók számára</span><span class="sxs-lookup"><span data-stu-id="aebab-124">Trusted IPs for federated users</span></span>
<span data-ttu-id="aebab-125">Megbízható IP-címek lehetővé teszik a rendszergazdák tooby-fázis kétlépéses ellenőrzés bizonyos IP-címeket vagy összevont felhasználók, amelyek a saját intraneten belüli származó kérelmek.</span><span class="sxs-lookup"><span data-stu-id="aebab-125">Trusted IPs allow administrators tooby-pass two-step verification for specific IP addresses, or for federated users that have requests originating from within their own intranet.</span></span> <span data-ttu-id="aebab-126">hello alábbi szakaszok azt ismertetik, hogyan tooconfigure Azure multi-factor Authentication megbízható IP-címeket az összevont felhasználók és kihagyva kétlépéses ellenőrzést, ha a kérelem származási egy összevont felhasználók intraneten belül.</span><span class="sxs-lookup"><span data-stu-id="aebab-126">hello following sections describe how tooconfigure Azure Multi-Factor Authentication Trusted IPs with federated users and by-pass two-step verification when a request originates from within a federated users intranet.</span></span> <span data-ttu-id="aebab-127">Ez úgy konfigurálja az AD FS toouse csatlakoztatott vagy szűrő egy bejövő jogcím-sablont a vállalati hálózaton belül jogcímtípus hello érhető el.</span><span class="sxs-lookup"><span data-stu-id="aebab-127">This is achieved by configuring AD FS toouse a pass-through or filter an incoming claim template with hello Inside Corporate Network claim type.</span></span>

<span data-ttu-id="aebab-128">Ez a példa az Office 365-öt használja a függőentitás-megbízhatóságokhoz.</span><span class="sxs-lookup"><span data-stu-id="aebab-128">This example uses Office 365 for our Relying Party Trusts.</span></span>

### <a name="configure-hello-ad-fs-claims-rules"></a><span data-ttu-id="aebab-129">Az AD FS hello jogcímek szabályok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="aebab-129">Configure hello AD FS claims rules</span></span>
<span data-ttu-id="aebab-130">hello első lépésként szükségünk van toodo tooconfigure hello AD FS-jogcímek.</span><span class="sxs-lookup"><span data-stu-id="aebab-130">hello first thing we need toodo is tooconfigure hello AD FS claims.</span></span> <span data-ttu-id="aebab-131">Két jogcímek szabályt hoz létre, egy a vállalati hálózaton belül hello jogcím típusokat, hogy a bejelentkezett felhasználók a további egyikét.</span><span class="sxs-lookup"><span data-stu-id="aebab-131">Create two claims rules, one for hello Inside Corporate Network claim type and an additional one for keeping our users signed in.</span></span>

1. <span data-ttu-id="aebab-132">Nyissa meg az AD FS felügyeleti konzolt.</span><span class="sxs-lookup"><span data-stu-id="aebab-132">Open AD FS Management.</span></span>
2. <span data-ttu-id="aebab-133">Hello bal oldalon válassza ki a **függő entitás Megbízhatóságai**.</span><span class="sxs-lookup"><span data-stu-id="aebab-133">On hello left, select **Relying Party Trusts**.</span></span>
3. <span data-ttu-id="aebab-134">Középen kattintson a jobb gombbal a **Microsoft Office 365 Identity Platform** elemre, és válassza a **Jogcímszabályok szerkesztése…** lehetőséget.
   ![Felhő](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)</span><span class="sxs-lookup"><span data-stu-id="aebab-134">Right-click on **Microsoft Office 365 Identity Platform** and select **Edit Claim Rules…**
![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)</span></span>
4. <span data-ttu-id="aebab-135">A Kiadás átalakítási szabályai részben kattintson a **Szabály hozzáadása** lehetőségre.
   ![Felhő](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)</span><span class="sxs-lookup"><span data-stu-id="aebab-135">On Issuance Transform Rules, click **Add Rule.**
![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)</span></span>
5. <span data-ttu-id="aebab-136">Az átalakítási Jogcímszabály szabály varázsló hello, válassza a **továbbítása vagy szűrése egy bejövő jogcím** hello legördülő listán, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="aebab-136">On hello Add Transform Claim Rule Wizard, select **Pass Through or Filter an Incoming Claim** from hello drop-down and click **Next**.</span></span>
   <span data-ttu-id="aebab-137">![Felhő](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)</span><span class="sxs-lookup"><span data-stu-id="aebab-137">![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)</span></span>
6. <span data-ttu-id="aebab-138">Hello mezőben következő tooClaim szabály neve nevezze el a szabályt.</span><span class="sxs-lookup"><span data-stu-id="aebab-138">In hello box next tooClaim rule name, give your rule a name.</span></span> <span data-ttu-id="aebab-139">Például: InsideCorpNet.</span><span class="sxs-lookup"><span data-stu-id="aebab-139">For example: InsideCorpNet.</span></span>
7. <span data-ttu-id="aebab-140">Hello legördülő listán, a következő tooIncoming jogcím-típust, jelölje be **vállalati hálózaton belül**.</span><span class="sxs-lookup"><span data-stu-id="aebab-140">From hello drop-down, next tooIncoming claim type, select **Inside Corporate Network**.</span></span>
   <span data-ttu-id="aebab-141">![Felhő](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip4.png)</span><span class="sxs-lookup"><span data-stu-id="aebab-141">![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip4.png)</span></span>
8. <span data-ttu-id="aebab-142">Kattintson a **Finish** (Befejezés) gombra.</span><span class="sxs-lookup"><span data-stu-id="aebab-142">Click **Finish**.</span></span>
9. <span data-ttu-id="aebab-143">A Kiadás átalakítási szabályai részben kattintson a **Szabály hozzáadása** elemre.</span><span class="sxs-lookup"><span data-stu-id="aebab-143">On Issuance Transform Rules, click **Add Rule**.</span></span>
10. <span data-ttu-id="aebab-144">Az átalakítási Jogcímszabály szabály varázsló hello, válassza a **jogcímek küldése egyéni szabály segítségével** hello legördülő listán, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="aebab-144">On hello Add Transform Claim Rule Wizard, select **Send Claims Using a Custom Rule** from hello drop-down and click **Next**.</span></span>
11. <span data-ttu-id="aebab-145">Jogcímszabály neve alatt hello mezőben: Adja meg *tartsa felhasználók aláírt a*.</span><span class="sxs-lookup"><span data-stu-id="aebab-145">In hello box under Claim rule name: enter *Keep Users Signed In*.</span></span>
12. <span data-ttu-id="aebab-146">Hello egyéni szabály mezőben adja meg:</span><span class="sxs-lookup"><span data-stu-id="aebab-146">In hello Custom rule box, enter:</span></span>

        c:[Type == "http://schemas.microsoft.com/2014/03/psso"]
            => issue(claim = c);
    ![Felhő](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip5.png)
13. <span data-ttu-id="aebab-148">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="aebab-148">Click **Finish**.</span></span>
14. <span data-ttu-id="aebab-149">Kattintson az **Alkalmaz** gombra.</span><span class="sxs-lookup"><span data-stu-id="aebab-149">Click **Apply**.</span></span>
15. <span data-ttu-id="aebab-150">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="aebab-150">Click **Ok**.</span></span>
16. <span data-ttu-id="aebab-151">Zárja be az AD FS felügyeleti konzolt.</span><span class="sxs-lookup"><span data-stu-id="aebab-151">Close AD FS Management.</span></span>

### <a name="configure-azure-multi-factor-authentication-trusted-ips-with-federated-users"></a><span data-ttu-id="aebab-152">Azure Multi-Factor Authentication megbízható IP-címeinek konfigurálása összevont felhasználókkal</span><span class="sxs-lookup"><span data-stu-id="aebab-152">Configure Azure Multi-Factor Authentication Trusted IPs with Federated Users</span></span>
<span data-ttu-id="aebab-153">Most, hogy hello jogcímek helyen, konfigurálhatjuk megbízható IP-címek.</span><span class="sxs-lookup"><span data-stu-id="aebab-153">Now that hello claims are in place, we can configure trusted IPs.</span></span>

1. <span data-ttu-id="aebab-154">Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="aebab-154">Sign in toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="aebab-155">Hello bal oldalon kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="aebab-155">On hello left, click **Active Directory**.</span></span>
3. <span data-ttu-id="aebab-156">Könyvtárban jelölje ki a kívánt tooset megbízható IP-címek fel hello címtárát.</span><span class="sxs-lookup"><span data-stu-id="aebab-156">Under Directory, select hello directory where you want tooset up trusted IPs.</span></span>
4. <span data-ttu-id="aebab-157">A kijelölt könyvtár hello, kattintson **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="aebab-157">On hello Directory you have selected, click **Configure**.</span></span>
5. <span data-ttu-id="aebab-158">Hello többtényezős hitelesítés területen kattintson **szolgáltatás beállításainak kezelése**.</span><span class="sxs-lookup"><span data-stu-id="aebab-158">In hello multi-factor authentication section, click **Manage service settings**.</span></span>
6. <span data-ttu-id="aebab-159">Hello szolgáltatás beállításai lapon a megbízható IP-címek, jelölje be **több-factor – hitelesítési kérelmek kihagyása az összevont felhasználók intranetről**.</span><span class="sxs-lookup"><span data-stu-id="aebab-159">On hello Service Settings page, under trusted IPs, select **Skip multi-factor-authentication for requests from federated users on my intranet**.</span></span>  

   ![Felhő](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip6.png)
   
7. <span data-ttu-id="aebab-161">Kattintson a **mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="aebab-161">Click **save**.</span></span>
8. <span data-ttu-id="aebab-162">Amikor hello frissítések telepítve lettek, kattintson a **bezárása**.</span><span class="sxs-lookup"><span data-stu-id="aebab-162">Once hello updates have been applied, click **close**.</span></span>

<span data-ttu-id="aebab-163">Készen is van.</span><span class="sxs-lookup"><span data-stu-id="aebab-163">That’s it!</span></span> <span data-ttu-id="aebab-164">Ezen a ponton összevont Office 365 felhasználóknak csak rendelkezniük kell toouse többtényezős Hitelesítést, amikor a jogcím külső hello vállalati intranethez származik.</span><span class="sxs-lookup"><span data-stu-id="aebab-164">At this point, federated Office 365 users should only have toouse MFA when a claim originates from outside hello corporate intranet.</span></span>
