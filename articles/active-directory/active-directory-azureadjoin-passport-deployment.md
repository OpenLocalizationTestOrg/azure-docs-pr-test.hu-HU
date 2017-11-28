---
title: "aaaEnable Microsoft vállalati Windows Hello a szervezet |} Microsoft Docs"
description: "Központi telepítési utasításokat tooenable Microsoft Passport a szervezetében."
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
ms.openlocfilehash: 6041f5916f606752bc55844b1b2d0a423b913cd3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-microsoft-windows-hello-for-business-in-your-organization"></a><span data-ttu-id="f1d70-104">Engedélyezze a Microsoft vállalati Windows Hello a szervezetben</span><span class="sxs-lookup"><span data-stu-id="f1d70-104">Enable Microsoft Windows Hello for Business in your organization</span></span>
<span data-ttu-id="f1d70-105">Miután [Windows 10-tartományhoz csatlakoztatott eszközök csatlakoztatása az Azure Active Directoryval](active-directory-azureadjoin-devices-group-policy.md), Microsoft Windows Hello for Business tooenable a szervezet következő hello:</span><span class="sxs-lookup"><span data-stu-id="f1d70-105">After [connecting Windows 10 domain-joined devices with Azure Active Directory](active-directory-azureadjoin-devices-group-policy.md), do hello following tooenable Microsoft Windows Hello for Business in your organization:</span></span>

1. <span data-ttu-id="f1d70-106">A System Center Configuration Manager központi telepítése</span><span class="sxs-lookup"><span data-stu-id="f1d70-106">Deploy System Center Configuration Manager</span></span>  
2. <span data-ttu-id="f1d70-107">Házirend-beállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f1d70-107">Configure policy settings</span></span>
3. <span data-ttu-id="f1d70-108">Hello tanúsítványprofil konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f1d70-108">Configure hello certificate profile</span></span>  

## <a name="deploy-system-center-configuration-manager"></a><span data-ttu-id="f1d70-109">A System Center Configuration Manager központi telepítése</span><span class="sxs-lookup"><span data-stu-id="f1d70-109">Deploy System Center Configuration Manager</span></span>
<span data-ttu-id="f1d70-110">a Windows Hello üzleti kulcsok alapján toodeploy felhasználói tanúsítvány következőkre lesz szüksége hello:</span><span class="sxs-lookup"><span data-stu-id="f1d70-110">toodeploy user certificates based on Windows Hello for Business keys, you need hello following:</span></span>

* <span data-ttu-id="f1d70-111">**A System Center Configuration Manager aktuális ágának** -1606 vagy jobb tooinstall verziója szükséges.</span><span class="sxs-lookup"><span data-stu-id="f1d70-111">**System Center Configuration Manager Current Branch** - You need tooinstall version 1606 or better.</span></span> <span data-ttu-id="f1d70-112">További információkért lásd: hello [dokumentáció a System Center Configuration Manager](https://technet.microsoft.com/library/mt346023.aspx) és [System Center Configuration Manager Team Blog](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx).</span><span class="sxs-lookup"><span data-stu-id="f1d70-112">For more information, see hello [Documentation for System Center Configuration Manager](https://technet.microsoft.com/library/mt346023.aspx) and [System Center Configuration Manager Team Blog](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx).</span></span>
* <span data-ttu-id="f1d70-113">**Nyilvános kulcsokra épülő infrastruktúra (PKI)** -tooenable Microsoft Windows Hello üzleti felhasználói tanúsítványok használatával, rendelkeznie kell egy nyilvános kulcsokra épülő infrastruktúra helyen.</span><span class="sxs-lookup"><span data-stu-id="f1d70-113">**Public key infrastructure (PKI)** - tooenable Microsoft Windows Hello for Business by using user certificates, you must have a PKI in place.</span></span> <span data-ttu-id="f1d70-114">Ha még nincs fiókja, vagy nem szeretné, hogy toouse azt a felhasználói tanúsítványok telepítése egy új tartományvezérlő, amely a Windows Server 2016-os build 10551 (vagy nagyobb frekvenciával) telepítve van.</span><span class="sxs-lookup"><span data-stu-id="f1d70-114">If you don’t have one, or you don’t want toouse it for user certificates, you can deploy a new domain controller that has Windows Server 2016 build 10551 (or better) installed.</span></span> <span data-ttu-id="f1d70-115">Hello lépésekkel túl[replika tartományvezérlő telepítése meglévő tartományban](https://technet.microsoft.com/library/jj574134.aspx) vagy túl[egy új Active Directory-erdő telepítéséhez, ha hoz létre egy új környezetben](https://technet.microsoft.com/library/jj574166).</span><span class="sxs-lookup"><span data-stu-id="f1d70-115">Follow hello steps too[install a replica domain controller in an existing domain](https://technet.microsoft.com/library/jj574134.aspx) or too[install a new Active Directory forest, if you're creating a new environment](https://technet.microsoft.com/library/jj574166).</span></span> <span data-ttu-id="f1d70-116">(hello ISO letölthetők a [Signiant Media Exchange](https://datatransfer.microsoft.com/signiant_media_exchange/spring/main?sdkAccessible=true).)</span><span class="sxs-lookup"><span data-stu-id="f1d70-116">(hello ISOs are available for download on [Signiant Media Exchange](https://datatransfer.microsoft.com/signiant_media_exchange/spring/main?sdkAccessible=true).)</span></span>

## <a name="configure-policy-settings"></a><span data-ttu-id="f1d70-117">Házirend-beállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f1d70-117">Configure policy settings</span></span>
<span data-ttu-id="f1d70-118">Microsoft Windows Hello tooconfigure hello házirend-beállítások, két lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="f1d70-118">tooconfigure hello Microsoft Windows Hello for Business policy settings, you have two options:</span></span>

* <span data-ttu-id="f1d70-119">Az Active Directory csoportházirend</span><span class="sxs-lookup"><span data-stu-id="f1d70-119">Group Policy in Active Directory</span></span> 
* <span data-ttu-id="f1d70-120">hello System Center Configuration Managerben</span><span class="sxs-lookup"><span data-stu-id="f1d70-120">hello System Center Configuration Manager</span></span> 

<span data-ttu-id="f1d70-121">Az Active Directoryban a csoportházirendekkel üzleti házirend-beállítások a Microsoft Windows Hello metódus tooconfigure ajánlott hello.</span><span class="sxs-lookup"><span data-stu-id="f1d70-121">Using Group Policy in Active Directory is hello recommended method tooconfigure Microsoft Windows Hello for Business policy settings.</span></span> 

<span data-ttu-id="f1d70-122">A System Center Configuration Manager hello előnyben részesített módszert is használata esetén azt toodeploy tanúsítványokat.</span><span class="sxs-lookup"><span data-stu-id="f1d70-122">Using System Center Configuration Manager is hello preferred method when you also use it toodeploy certificates.</span></span> <span data-ttu-id="f1d70-123">Ebben a forgatókönyvben:</span><span class="sxs-lookup"><span data-stu-id="f1d70-123">This scenario:</span></span>

* <span data-ttu-id="f1d70-124">Biztosítja a kompatibilitást hello újabb történő telepítések esetén</span><span class="sxs-lookup"><span data-stu-id="f1d70-124">Ensures compatibility with hello newer deployment scenarios</span></span>
* <span data-ttu-id="f1d70-125">Hello ügyféloldali Windows 10-es verzió 1607 vagy nagyobb frekvenciával igényel.</span><span class="sxs-lookup"><span data-stu-id="f1d70-125">Requires on hello client side Windows 10 Version 1607 or better.</span></span>

### <a name="configure-microsoft-windows-hello-for-business-via-group-policy-in-active-directory"></a><span data-ttu-id="f1d70-126">Konfigurálja a Microsoft vállalati Windows Hello az Active Directory csoportházirend segítségével</span><span class="sxs-lookup"><span data-stu-id="f1d70-126">Configure Microsoft Windows Hello for Business via group policy in Active Directory</span></span>
<span data-ttu-id="f1d70-127">**Lépéseket**:</span><span class="sxs-lookup"><span data-stu-id="f1d70-127">**Steps**:</span></span>

1. <span data-ttu-id="f1d70-128">Nyissa meg a Kiszolgálókezelőt, és keresse meg a túl**eszközök** > **csoportházirend-kezelő**.</span><span class="sxs-lookup"><span data-stu-id="f1d70-128">Open Server Manager, and navigate too**Tools** > **Group Policy Management**.</span></span>
2. <span data-ttu-id="f1d70-129">A Csoportházirend kezelése keresse meg a használni kívánt Azure AD Join tooenable toohello tartományhoz tartozó tartományi csomópontot toohello.</span><span class="sxs-lookup"><span data-stu-id="f1d70-129">From Group Policy Management, navigate toohello domain node that corresponds toohello domain in which you want tooenable Azure AD Join.</span></span>
3. <span data-ttu-id="f1d70-130">Kattintson a jobb gombbal **csoportházirend-objektumok**, és válassza ki **új**.</span><span class="sxs-lookup"><span data-stu-id="f1d70-130">Right-click **Group Policy Objects**, and select **New**.</span></span> <span data-ttu-id="f1d70-131">Nevezze el a csoportházirend-objektum, például engedélyezése vállalati Windows Hello.</span><span class="sxs-lookup"><span data-stu-id="f1d70-131">Give your Group Policy Object a name, for example, Enable Windows Hello for Business.</span></span> <span data-ttu-id="f1d70-132">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="f1d70-132">Click **OK**.</span></span>
4. <span data-ttu-id="f1d70-133">Kattintson a jobb gombbal az új csoportházirend-objektumot, majd válassza ki **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="f1d70-133">Right-click your new Group Policy Object, and then select **Edit**.</span></span>
5. <span data-ttu-id="f1d70-134">Keresse meg a túl**számítógép konfigurációja** > **házirendek** > **felügyeleti sablonok** > **Windows Összetevők** > **a vállalati Windows Hello**.</span><span class="sxs-lookup"><span data-stu-id="f1d70-134">Navigate too**Computer Configuration** > **Policies** > **Administrative Templates** > **Windows Components** > **Windows Hello for Business**.</span></span>
6. <span data-ttu-id="f1d70-135">Kattintson a jobb gombbal **engedélyezése vállalati Windows Hello**, majd válassza ki **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="f1d70-135">Right-click **Enable Windows Hello for Business**, and then select **Edit**.</span></span>
7. <span data-ttu-id="f1d70-136">Jelölje be hello **engedélyezve** választógomb, és kattintson a **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="f1d70-136">Select hello **Enabled** option button, and then click **Apply**.</span></span> <span data-ttu-id="f1d70-137">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="f1d70-137">Click **OK**.</span></span>
8. <span data-ttu-id="f1d70-138">Most hozzákapcsolhatja hello csoportházirend-objektum tooa tetszőleges helyre.</span><span class="sxs-lookup"><span data-stu-id="f1d70-138">You can now link hello Group Policy Object tooa location of your choice.</span></span> <span data-ttu-id="f1d70-139">tooenable ezt a házirendet az összes hello tartományhoz csatlakoztatott Windows 10-eszközök a szervezetben, hivatkozás hello csoportházirend toohello tartomány.</span><span class="sxs-lookup"><span data-stu-id="f1d70-139">tooenable this policy for all of hello domain-joined Windows 10 devices in your organization, link hello Group Policy toohello domain.</span></span> <span data-ttu-id="f1d70-140">Példa:</span><span class="sxs-lookup"><span data-stu-id="f1d70-140">For example:</span></span>
   * <span data-ttu-id="f1d70-141">Egy adott szervezeti egység (OU) az Active Directoryban, ha Windows 10-es tartományhoz csatlakoztatott számítógépek lesznek elhelyezve</span><span class="sxs-lookup"><span data-stu-id="f1d70-141">A specific organizational unit (OU) in Active Directory where Windows 10 domain-joined computers will be located</span></span>
   * <span data-ttu-id="f1d70-142">Egy adott biztonsági csoport, amely tartalmazza a Windows 10-tartományhoz csatlakoztatott számítógépeket, amelyek automatikusan regisztrálva lesz az Azure ad szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="f1d70-142">A specific security group that contains Windows 10 domain-joined computers that will be automatically registered with Azure AD</span></span>

### <a name="configure-windows-hello-for-business-using-system-center-configuration-manager"></a><span data-ttu-id="f1d70-143">A System Center Configuration Manager vállalati Windows Hello konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f1d70-143">Configure Windows Hello for Business using System Center Configuration Manager</span></span>
<span data-ttu-id="f1d70-144">**Lépéseket**:</span><span class="sxs-lookup"><span data-stu-id="f1d70-144">**Steps**:</span></span>

1. <span data-ttu-id="f1d70-145">Megnyitás hello **System Center Configuration Manager**, és keresse meg a túl**eszközök és megfelelőség > megfelelőségi beállítások > Vállalati erőforrások elérése > üzleti profilok a Windows Hello**.</span><span class="sxs-lookup"><span data-stu-id="f1d70-145">Open hello **System Center Configuration Manager**, and then navigate too**Assets & Compliance > Compliance Settings > Company Resource Access > Windows Hello for Business Profiles**.</span></span>
   
    ![A vállalati Windows Hello konfigurálása](./media/active-directory-azureadjoin-passport-deployment/01.png)
2. <span data-ttu-id="f1d70-147">Hello hello felső eszköztárán kattintson **vállalati profil létrehozása a Windows Hello**.</span><span class="sxs-lookup"><span data-stu-id="f1d70-147">In hello toolbar on hello top, click **Create Windows Hello for business Profile**.</span></span>
   
    ![A vállalati Windows Hello konfigurálása](./media/active-directory-azureadjoin-passport-deployment/02.png)
3. <span data-ttu-id="f1d70-149">A hello **általános** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="f1d70-149">On hello **General** dialog, perform hello following steps:</span></span>
   
    ![A vállalati Windows Hello konfigurálása](./media/active-directory-azureadjoin-passport-deployment/03.png)
   
    <span data-ttu-id="f1d70-151">a.</span><span class="sxs-lookup"><span data-stu-id="f1d70-151">a.</span></span> <span data-ttu-id="f1d70-152">A hello **neve** szövegmező, írja be például a profil nevét **WHfB profil**.</span><span class="sxs-lookup"><span data-stu-id="f1d70-152">In hello **Name** textbox, type a name for your profile, for example, **My WHfB Profile**.</span></span>
   
    <span data-ttu-id="f1d70-153">b.</span><span class="sxs-lookup"><span data-stu-id="f1d70-153">b.</span></span> <span data-ttu-id="f1d70-154">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="f1d70-154">Click **Next**.</span></span>
4. <span data-ttu-id="f1d70-155">A hello **által támogatott platformok** párbeszédpanelen jelölje be hello platformokat, amelyek megkapják a Windows Hello üzleti profil, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="f1d70-155">On hello **Supported Platforms** dialog, select hello platforms that will be provisioned with this Windows Hello for business profile, and then click **Next**.</span></span>
   
    ![A vállalati Windows Hello konfigurálása](./media/active-directory-azureadjoin-passport-deployment/04.png)
5. <span data-ttu-id="f1d70-157">A hello **beállítások** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="f1d70-157">On hello **Settings** dialog, perform hello following steps:</span></span>
   
    ![A vállalati Windows Hello konfigurálása](./media/active-directory-azureadjoin-passport-deployment/05.png)
   
    <span data-ttu-id="f1d70-159">a.</span><span class="sxs-lookup"><span data-stu-id="f1d70-159">a.</span></span> <span data-ttu-id="f1d70-160">Mint **konfigurálása vállalati Windows Hello**, jelölje be **engedélyezve**.</span><span class="sxs-lookup"><span data-stu-id="f1d70-160">As **Configure Windows Hello for Business**, select **Enabled**.</span></span>
   
    <span data-ttu-id="f1d70-161">b.</span><span class="sxs-lookup"><span data-stu-id="f1d70-161">b.</span></span> <span data-ttu-id="f1d70-162">Mint **egy platformmegbízhatósági modul (TPM) használata**, jelölje be **szükséges**.</span><span class="sxs-lookup"><span data-stu-id="f1d70-162">As **Use a Trusted Platform Module (TPM)**, select **Required**.</span></span> 
   
    <span data-ttu-id="f1d70-163">c.</span><span class="sxs-lookup"><span data-stu-id="f1d70-163">c.</span></span> <span data-ttu-id="f1d70-164">Mint **hitelesítési módszer**, jelölje be **tanúsítványalapú**.</span><span class="sxs-lookup"><span data-stu-id="f1d70-164">As **Authentication method**, select **Certificate-based**.</span></span>
   
    <span data-ttu-id="f1d70-165">d.</span><span class="sxs-lookup"><span data-stu-id="f1d70-165">d.</span></span> <span data-ttu-id="f1d70-166">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="f1d70-166">Click **Next**.</span></span>
6. <span data-ttu-id="f1d70-167">A hello **összegzés** párbeszédpanel, kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="f1d70-167">On hello **Summary** dialog, click **Next**.</span></span>
7. <span data-ttu-id="f1d70-168">A hello **befejezési** párbeszédpanel, kattintson a **Bezárás**.</span><span class="sxs-lookup"><span data-stu-id="f1d70-168">On hello **Completion** dialog, click **Close**.</span></span>
8. <span data-ttu-id="f1d70-169">Hello hello felső eszköztárán kattintson **telepítés**.</span><span class="sxs-lookup"><span data-stu-id="f1d70-169">In hello toolbar on hello top, click **Deploy**.</span></span>
   
    ![A vállalati Windows Hello konfigurálása](./media/active-directory-azureadjoin-passport-deployment/06.png)

## <a name="configure-hello-certificate-profile"></a><span data-ttu-id="f1d70-171">Hello tanúsítványprofil konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f1d70-171">Configure hello certificate profile</span></span>
<span data-ttu-id="f1d70-172">Ha a tanúsítvány alapú hitelesítést használ a helyszíni hitelesítéshez, tooconfigure kell, és tanúsítványprofil központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="f1d70-172">If you are using certificate-based authentication for on-premises authentication, you need tooconfigure and deploy a certificate profile.</span></span> <span data-ttu-id="f1d70-173">Ez a feladat egy NDES-kiszolgáló és a Tanúsítványregisztrációs pont helyrendszer-szerepkör a System Center Configuration Manager hello tooset igényel.</span><span class="sxs-lookup"><span data-stu-id="f1d70-173">This task requires you tooset up an NDES server and Certificate Registration Point site role in hello System Center Configuration Manager.</span></span> <span data-ttu-id="f1d70-174">További részletekért lásd: hello [előfeltételei a Configuration Manager Tanúsítványprofiljaihoz](https://technet.microsoft.com/library/dn261205.aspx).</span><span class="sxs-lookup"><span data-stu-id="f1d70-174">For more details, see hello [Prerequisites for Certificate Profiles in Configuration Manager](https://technet.microsoft.com/library/dn261205.aspx).</span></span>

1. <span data-ttu-id="f1d70-175">Megnyitás hello **System Center Configuration Manager**, majd keresse meg a túl**eszközök és megfelelőség > megfelelőségi beállítások > Vállalati erőforrások elérése > Tanúsítványprofilok**.</span><span class="sxs-lookup"><span data-stu-id="f1d70-175">Open hello **System Center Configuration Manager**, and then navigate too**Assets & Compliance > Compliance Settings > Company Resource Access > Certificate Profiles**.</span></span>
2. <span data-ttu-id="f1d70-176">Válasszon olyan sablont, amely rendelkezik az intelligens kártyás bejelentkezési kibővített kulcshasználat (EKU).</span><span class="sxs-lookup"><span data-stu-id="f1d70-176">Select a template that has Smart Card sign-in extended key usage (EKU).</span></span>

<span data-ttu-id="f1d70-177">A hello **SCEP-igénylés** lap hello tanúsítványprofilt, akkor kell toochoose **tooPassport for Work szolgáltatásba, másként meghibásodás történik telepítése** hello, **kulcstároló-szolgáltató**.</span><span class="sxs-lookup"><span data-stu-id="f1d70-177">On hello **SCEP Enrollment** page of hello certificate profile, you need toochoose **Install tooPassport for Work otherwise fail** as hello **Key Storage Provider**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f1d70-178">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f1d70-178">Next steps</span></span>
* [<span data-ttu-id="f1d70-179">Windows 10 Enterprise hello: módon toouse eszközök munkára</span><span class="sxs-lookup"><span data-stu-id="f1d70-179">Windows 10 for hello enterprise: Ways toouse devices for work</span></span>](active-directory-azureadjoin-windows10-devices-overview.md)
* [<span data-ttu-id="f1d70-180">Felhőalapú képességek tooWindows 10 eszközöknek az Azure Active Directory csatlakozási kiterjesztése</span><span class="sxs-lookup"><span data-stu-id="f1d70-180">Extending cloud capabilities tooWindows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-user-upgrade.md)
* [<span data-ttu-id="f1d70-181">Microsoft Passporton keresztül jelszó nélkül identitások hitelesítése</span><span class="sxs-lookup"><span data-stu-id="f1d70-181">Authenticating identities without passwords through Microsoft Passport</span></span>](active-directory-azureadjoin-passport.md)
* [<span data-ttu-id="f1d70-182">További információk az Azure AD Join használati forgatókönyveiről</span><span class="sxs-lookup"><span data-stu-id="f1d70-182">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="f1d70-183">Csatlakozás a tartományhoz csatlakozó eszközök tooAzure AD-hez Windows 10-es környezetben</span><span class="sxs-lookup"><span data-stu-id="f1d70-183">Connect domain-joined devices tooAzure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="f1d70-184">Az Azure AD Join beállítása</span><span class="sxs-lookup"><span data-stu-id="f1d70-184">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)

