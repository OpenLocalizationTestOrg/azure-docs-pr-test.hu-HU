---
title: "Engedélyezze a Microsoft vállalati Windows Hello a szervezet |} Microsoft Docs"
description: "A Microsoft Passport engedélyezése a szervezetében üzembe helyezési utasításokat tartalmaz."
services: active-directory
documentationcenter: 
keywords: "a Microsoft Passport, Microsoft Windows Hello-üzleti központi telepítés konfigurálása"
author: MarkusVi
manager: femila
tags: azure-classic-portal
ms.assetid: 7dbbe3c6-1cd7-429c-a9b2-115fcbc02416
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.openlocfilehash: 58943e1e29755c983e55c675dd4fe7b75ac47b34
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="enable-microsoft-windows-hello-for-business-in-your-organization"></a><span data-ttu-id="3d3bf-104">Engedélyezze a Microsoft vállalati Windows Hello a szervezetben</span><span class="sxs-lookup"><span data-stu-id="3d3bf-104">Enable Microsoft Windows Hello for Business in your organization</span></span>
<span data-ttu-id="3d3bf-105">Után [Windows 10-tartományhoz csatlakoztatott eszközök csatlakoztatása az Azure Active Directoryval](active-directory-azureadjoin-devices-group-policy.md), engedélyezze a Microsoft vállalati Windows Hello a szervezet érdekében tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="3d3bf-105">After [connecting Windows 10 domain-joined devices with Azure Active Directory](active-directory-azureadjoin-devices-group-policy.md), do the following to enable Microsoft Windows Hello for Business in your organization:</span></span>

1. <span data-ttu-id="3d3bf-106">A System Center Configuration Manager központi telepítése</span><span class="sxs-lookup"><span data-stu-id="3d3bf-106">Deploy System Center Configuration Manager</span></span>  
2. <span data-ttu-id="3d3bf-107">Házirend-beállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3d3bf-107">Configure policy settings</span></span>
3. <span data-ttu-id="3d3bf-108">A tanúsítványprofilok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3d3bf-108">Configure the certificate profile</span></span>  

## <a name="deploy-system-center-configuration-manager"></a><span data-ttu-id="3d3bf-109">A System Center Configuration Manager központi telepítése</span><span class="sxs-lookup"><span data-stu-id="3d3bf-109">Deploy System Center Configuration Manager</span></span>
<span data-ttu-id="3d3bf-110">A Windows Hello üzleti kulcsok alapján felhasználói tanúsítványok telepítése, a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="3d3bf-110">To deploy user certificates based on Windows Hello for Business keys, you need the following:</span></span>

* <span data-ttu-id="3d3bf-111">**A System Center Configuration Manager aktuális ágának** -1606 vagy jobb verzióját telepíteni kell.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-111">**System Center Configuration Manager Current Branch** - You need to install version 1606 or better.</span></span> <span data-ttu-id="3d3bf-112">További információkért lásd: a [dokumentáció a System Center Configuration Manager](https://technet.microsoft.com/library/mt346023.aspx) és [System Center Configuration Manager Team Blog](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx).</span><span class="sxs-lookup"><span data-stu-id="3d3bf-112">For more information, see the [Documentation for System Center Configuration Manager](https://technet.microsoft.com/library/mt346023.aspx) and [System Center Configuration Manager Team Blog](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx).</span></span>
* <span data-ttu-id="3d3bf-113">**Nyilvános kulcsokra épülő infrastruktúra (PKI)** - engedélyezése a Microsoft vállalati Windows Hello segítségével a felhasználói tanúsítványok, rendelkeznie kell egy nyilvános kulcsokra épülő infrastruktúra helyen.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-113">**Public key infrastructure (PKI)** - To enable Microsoft Windows Hello for Business by using user certificates, you must have a PKI in place.</span></span> <span data-ttu-id="3d3bf-114">Ha még nincs fiókja, vagy nem kívánja használni a felhasználói tanúsítványok, telepíthet egy új tartományvezérlő, amely a Windows Server 2016-os build 10551 (vagy nagyobb frekvenciával) telepítve van.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-114">If you don’t have one, or you don’t want to use it for user certificates, you can deploy a new domain controller that has Windows Server 2016 build 10551 (or better) installed.</span></span> <span data-ttu-id="3d3bf-115">Kövesse a lépéseket a [replika tartományvezérlő telepítése meglévő tartományban](https://technet.microsoft.com/library/jj574134.aspx) vagy [egy új Active Directory-erdő telepítéséhez, ha hoz létre egy új környezetben](https://technet.microsoft.com/library/jj574166).</span><span class="sxs-lookup"><span data-stu-id="3d3bf-115">Follow the steps to [install a replica domain controller in an existing domain](https://technet.microsoft.com/library/jj574134.aspx) or to [install a new Active Directory forest, if you're creating a new environment](https://technet.microsoft.com/library/jj574166).</span></span> <span data-ttu-id="3d3bf-116">(Az ISO letölthetők a [Signiant Media Exchange](https://datatransfer.microsoft.com/signiant_media_exchange/spring/main?sdkAccessible=true).)</span><span class="sxs-lookup"><span data-stu-id="3d3bf-116">(The ISOs are available for download on [Signiant Media Exchange](https://datatransfer.microsoft.com/signiant_media_exchange/spring/main?sdkAccessible=true).)</span></span>

## <a name="configure-policy-settings"></a><span data-ttu-id="3d3bf-117">Házirend-beállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3d3bf-117">Configure policy settings</span></span>
<span data-ttu-id="3d3bf-118">Az a Microsoft Windows Hello-üzleti házirend-beállítások megadásához két lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="3d3bf-118">To configure the Microsoft Windows Hello for Business policy settings, you have two options:</span></span>

* <span data-ttu-id="3d3bf-119">Az Active Directory csoportházirend</span><span class="sxs-lookup"><span data-stu-id="3d3bf-119">Group Policy in Active Directory</span></span> 
* <span data-ttu-id="3d3bf-120">A System Center Configuration Managerben</span><span class="sxs-lookup"><span data-stu-id="3d3bf-120">The System Center Configuration Manager</span></span> 

<span data-ttu-id="3d3bf-121">Az Active Directoryban a csoportházirendekkel a konfigurálásának javasolt módja a Microsoft Windows Hello az üzleti házirend-beállításokat.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-121">Using Group Policy in Active Directory is the recommended method to configure Microsoft Windows Hello for Business policy settings.</span></span> 

<span data-ttu-id="3d3bf-122">A System Center Configuration Manager az előnyben részesített módszere is használata esetén azt szolgáltatást tanúsítványok központi telepítésére.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-122">Using System Center Configuration Manager is the preferred method when you also use it to deploy certificates.</span></span> <span data-ttu-id="3d3bf-123">Ebben a forgatókönyvben:</span><span class="sxs-lookup"><span data-stu-id="3d3bf-123">This scenario:</span></span>

* <span data-ttu-id="3d3bf-124">Biztosítja a kompatibilitást a újabb történő telepítések esetén</span><span class="sxs-lookup"><span data-stu-id="3d3bf-124">Ensures compatibility with the newer deployment scenarios</span></span>
* <span data-ttu-id="3d3bf-125">Az ügyféloldali Windows 10-es verzió 1607 vagy nagyobb frekvenciával igényel.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-125">Requires on the client side Windows 10 Version 1607 or better.</span></span>

### <a name="configure-microsoft-windows-hello-for-business-via-group-policy-in-active-directory"></a><span data-ttu-id="3d3bf-126">Konfigurálja a Microsoft vállalati Windows Hello az Active Directory csoportházirend segítségével</span><span class="sxs-lookup"><span data-stu-id="3d3bf-126">Configure Microsoft Windows Hello for Business via group policy in Active Directory</span></span>
<span data-ttu-id="3d3bf-127">**Lépéseket**:</span><span class="sxs-lookup"><span data-stu-id="3d3bf-127">**Steps**:</span></span>

1. <span data-ttu-id="3d3bf-128">Nyissa meg a Kiszolgálókezelőt, és navigáljon a **eszközök** > **csoportházirend-kezelő**.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-128">Open Server Manager, and navigate to **Tools** > **Group Policy Management**.</span></span>
2. <span data-ttu-id="3d3bf-129">A Csoportházirend kezelése eszközt navigáljon a kívánja engedélyezése az Azure AD-csatlakozás tartományhoz tartozó tartományi csomópontot.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-129">From Group Policy Management, navigate to the domain node that corresponds to the domain in which you want to enable Azure AD Join.</span></span>
3. <span data-ttu-id="3d3bf-130">Kattintson a jobb gombbal **csoportházirend-objektumok**, és válassza ki **új**.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-130">Right-click **Group Policy Objects**, and select **New**.</span></span> <span data-ttu-id="3d3bf-131">Nevezze el a csoportházirend-objektum, például engedélyezése vállalati Windows Hello.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-131">Give your Group Policy Object a name, for example, Enable Windows Hello for Business.</span></span> <span data-ttu-id="3d3bf-132">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-132">Click **OK**.</span></span>
4. <span data-ttu-id="3d3bf-133">Kattintson a jobb gombbal az új csoportházirend-objektumot, majd válassza ki **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-133">Right-click your new Group Policy Object, and then select **Edit**.</span></span>
5. <span data-ttu-id="3d3bf-134">Navigáljon a **számítógép konfigurációja** > **házirendek** > **felügyeleti sablonok** > **Windows Összetevők** > **a vállalati Windows Hello**.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-134">Navigate to **Computer Configuration** > **Policies** > **Administrative Templates** > **Windows Components** > **Windows Hello for Business**.</span></span>
6. <span data-ttu-id="3d3bf-135">Kattintson a jobb gombbal **engedélyezése vállalati Windows Hello**, majd válassza ki **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-135">Right-click **Enable Windows Hello for Business**, and then select **Edit**.</span></span>
7. <span data-ttu-id="3d3bf-136">Válassza ki a **engedélyezve** választógomb, és kattintson a **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-136">Select the **Enabled** option button, and then click **Apply**.</span></span> <span data-ttu-id="3d3bf-137">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-137">Click **OK**.</span></span>
8. <span data-ttu-id="3d3bf-138">A csoportházirend-objektum most hozzákapcsolhatja egy tetszés szerinti helyre.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-138">You can now link the Group Policy Object to a location of your choice.</span></span> <span data-ttu-id="3d3bf-139">Ahhoz, hogy ez a házirend az összes a tartományhoz csatlakoztatott Windows 10-eszközök a szervezetben, kapcsolja a csoportházirendet a tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-139">To enable this policy for all of the domain-joined Windows 10 devices in your organization, link the Group Policy to the domain.</span></span> <span data-ttu-id="3d3bf-140">Példa:</span><span class="sxs-lookup"><span data-stu-id="3d3bf-140">For example:</span></span>
   * <span data-ttu-id="3d3bf-141">Egy adott szervezeti egység (OU) az Active Directoryban, ha Windows 10-es tartományhoz csatlakoztatott számítógépek lesznek elhelyezve</span><span class="sxs-lookup"><span data-stu-id="3d3bf-141">A specific organizational unit (OU) in Active Directory where Windows 10 domain-joined computers will be located</span></span>
   * <span data-ttu-id="3d3bf-142">Egy adott biztonsági csoport, amely tartalmazza a Windows 10-tartományhoz csatlakoztatott számítógépeket, amelyek automatikusan regisztrálva lesz az Azure ad szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="3d3bf-142">A specific security group that contains Windows 10 domain-joined computers that will be automatically registered with Azure AD</span></span>

### <a name="configure-windows-hello-for-business-using-system-center-configuration-manager"></a><span data-ttu-id="3d3bf-143">A System Center Configuration Manager vállalati Windows Hello konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3d3bf-143">Configure Windows Hello for Business using System Center Configuration Manager</span></span>
<span data-ttu-id="3d3bf-144">**Lépéseket**:</span><span class="sxs-lookup"><span data-stu-id="3d3bf-144">**Steps**:</span></span>

1. <span data-ttu-id="3d3bf-145">Nyissa meg a **System Center Configuration Manager**, és navigáljon arra **eszközök és megfelelőség > megfelelőségi beállítások > Vállalati erőforrások elérése > üzleti profilok a Windows Hello-**.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-145">Open the **System Center Configuration Manager**, and then navigate to **Assets & Compliance > Compliance Settings > Company Resource Access > Windows Hello for Business Profiles**.</span></span>
   
    ![A vállalati Windows Hello konfigurálása](./media/active-directory-azureadjoin-passport-deployment/01.png)
2. <span data-ttu-id="3d3bf-147">A felső eszköztáron kattintson **létrehozása a Windows Hello-profil üzleti**.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-147">In the toolbar on the top, click **Create Windows Hello for business Profile**.</span></span>
   
    ![A vállalati Windows Hello konfigurálása](./media/active-directory-azureadjoin-passport-deployment/02.png)
3. <span data-ttu-id="3d3bf-149">Az a **általános** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="3d3bf-149">On the **General** dialog, perform the following steps:</span></span>
   
    ![A vállalati Windows Hello konfigurálása](./media/active-directory-azureadjoin-passport-deployment/03.png)
   
    <span data-ttu-id="3d3bf-151">a.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-151">a.</span></span> <span data-ttu-id="3d3bf-152">Az a **neve** szövegmező, írja be például a profil nevét **WHfB profil**.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-152">In the **Name** textbox, type a name for your profile, for example, **My WHfB Profile**.</span></span>
   
    <span data-ttu-id="3d3bf-153">b.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-153">b.</span></span> <span data-ttu-id="3d3bf-154">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-154">Click **Next**.</span></span>
4. <span data-ttu-id="3d3bf-155">Az a **által támogatott platformok** párbeszédpanelen jelölje ki a platformokat, amelyek megkapják a Windows Hello-profil üzleti, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-155">On the **Supported Platforms** dialog, select the platforms that will be provisioned with this Windows Hello for business profile, and then click **Next**.</span></span>
   
    ![A vállalati Windows Hello konfigurálása](./media/active-directory-azureadjoin-passport-deployment/04.png)
5. <span data-ttu-id="3d3bf-157">Az a **beállítások** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="3d3bf-157">On the **Settings** dialog, perform the following steps:</span></span>
   
    ![A vállalati Windows Hello konfigurálása](./media/active-directory-azureadjoin-passport-deployment/05.png)
   
    <span data-ttu-id="3d3bf-159">a.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-159">a.</span></span> <span data-ttu-id="3d3bf-160">Mint **konfigurálása vállalati Windows Hello**, jelölje be **engedélyezve**.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-160">As **Configure Windows Hello for Business**, select **Enabled**.</span></span>
   
    <span data-ttu-id="3d3bf-161">b.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-161">b.</span></span> <span data-ttu-id="3d3bf-162">Mint **egy platformmegbízhatósági modul (TPM) használata**, jelölje be **szükséges**.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-162">As **Use a Trusted Platform Module (TPM)**, select **Required**.</span></span> 
   
    <span data-ttu-id="3d3bf-163">c.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-163">c.</span></span> <span data-ttu-id="3d3bf-164">Mint **hitelesítési módszer**, jelölje be **tanúsítványalapú**.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-164">As **Authentication method**, select **Certificate-based**.</span></span>
   
    <span data-ttu-id="3d3bf-165">d.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-165">d.</span></span> <span data-ttu-id="3d3bf-166">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-166">Click **Next**.</span></span>
6. <span data-ttu-id="3d3bf-167">Az a **összegzés** párbeszédpanel, kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-167">On the **Summary** dialog, click **Next**.</span></span>
7. <span data-ttu-id="3d3bf-168">Az a **befejezési** párbeszédpanel, kattintson a **Bezárás**.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-168">On the **Completion** dialog, click **Close**.</span></span>
8. <span data-ttu-id="3d3bf-169">A felső eszköztáron kattintson **telepítés**.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-169">In the toolbar on the top, click **Deploy**.</span></span>
   
    ![A vállalati Windows Hello konfigurálása](./media/active-directory-azureadjoin-passport-deployment/06.png)

## <a name="configure-the-certificate-profile"></a><span data-ttu-id="3d3bf-171">A tanúsítványprofilok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3d3bf-171">Configure the certificate profile</span></span>
<span data-ttu-id="3d3bf-172">Ha a helyszíni hitelesítéshez tanúsítvány alapú hitelesítést használ, meg kell konfigurálnia és tanúsítványprofil központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-172">If you are using certificate-based authentication for on-premises authentication, you need to configure and deploy a certificate profile.</span></span> <span data-ttu-id="3d3bf-173">Ez a feladat meg kell adnia egy NDES-kiszolgáló és a Tanúsítványregisztrációs pont helyrendszer-szerepkör a System Center Configuration Manager számára.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-173">This task requires you to set up an NDES server and Certificate Registration Point site role in the System Center Configuration Manager.</span></span> <span data-ttu-id="3d3bf-174">További részletekért lásd: a [előfeltételei a Configuration Manager Tanúsítványprofiljaihoz](https://technet.microsoft.com/library/dn261205.aspx).</span><span class="sxs-lookup"><span data-stu-id="3d3bf-174">For more details, see the [Prerequisites for Certificate Profiles in Configuration Manager](https://technet.microsoft.com/library/dn261205.aspx).</span></span>

1. <span data-ttu-id="3d3bf-175">Nyissa meg a **System Center Configuration Manager**, és navigáljon arra **eszközök és megfelelőség > megfelelőségi beállítások > Vállalati erőforrások elérése > Tanúsítványprofilok**.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-175">Open the **System Center Configuration Manager**, and then navigate to **Assets & Compliance > Compliance Settings > Company Resource Access > Certificate Profiles**.</span></span>
2. <span data-ttu-id="3d3bf-176">Válasszon olyan sablont, amely rendelkezik az intelligens kártyás bejelentkezési kibővített kulcshasználat (EKU).</span><span class="sxs-lookup"><span data-stu-id="3d3bf-176">Select a template that has Smart Card sign-in extended key usage (EKU).</span></span>

<span data-ttu-id="3d3bf-177">Az a **SCEP-igénylés** lap a tanúsítványprofilok, meg kell adnia **telepítse a Passport for Work szolgáltatásba, másként meghibásodás történik** , a **kulcstároló-szolgáltató**.</span><span class="sxs-lookup"><span data-stu-id="3d3bf-177">On the **SCEP Enrollment** page of the certificate profile, you need to choose **Install to Passport for Work otherwise fail** as the **Key Storage Provider**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3d3bf-178">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3d3bf-178">Next steps</span></span>
* [<span data-ttu-id="3d3bf-179">Vállalati használatú Windows 10: Az eszközök munkahelyi célú használata</span><span class="sxs-lookup"><span data-stu-id="3d3bf-179">Windows 10 for the enterprise: Ways to use devices for work</span></span>](active-directory-azureadjoin-windows10-devices-overview.md)
* [<span data-ttu-id="3d3bf-180">A felhőalapú képességek kiterjesztése a Windows 10-eszközökre az Azure Active Directory Joinon keresztül</span><span class="sxs-lookup"><span data-stu-id="3d3bf-180">Extending cloud capabilities to Windows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-user-upgrade.md)
* [<span data-ttu-id="3d3bf-181">Microsoft Passporton keresztül jelszó nélkül identitások hitelesítése</span><span class="sxs-lookup"><span data-stu-id="3d3bf-181">Authenticating identities without passwords through Microsoft Passport</span></span>](active-directory-azureadjoin-passport.md)
* [<span data-ttu-id="3d3bf-182">További információk az Azure AD Join használati forgatókönyveiről</span><span class="sxs-lookup"><span data-stu-id="3d3bf-182">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="3d3bf-183">Tartományhoz csatlakoztatott eszközök csatlakoztatása az Azure AD-hez Windows 10-es környezetben</span><span class="sxs-lookup"><span data-stu-id="3d3bf-183">Connect domain-joined devices to Azure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="3d3bf-184">Az Azure AD Join beállítása</span><span class="sxs-lookup"><span data-stu-id="3d3bf-184">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)

