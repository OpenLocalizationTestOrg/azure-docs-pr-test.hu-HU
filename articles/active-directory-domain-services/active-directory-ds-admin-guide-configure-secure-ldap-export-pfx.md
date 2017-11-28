---
title: "Biztonságos LDAP (LDAPS) az Azure AD tartományi szolgáltatásokban aaaConfigure |} Microsoft Docs"
description: "Biztonságos LDAP (LDAPS) konfigurálása az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: c6da94b6-4328-4230-801a-4b646055d4d7
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: maheshu
ms.openlocfilehash: 356b28f8392b0e203df9c81177ec842d52866c4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a><span data-ttu-id="cbfa2-103">Biztonságos LDAP (LDAPS) használatos az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz tartozó konfigurálása</span><span class="sxs-lookup"><span data-stu-id="cbfa2-103">Configure secure LDAP (LDAPS) for an Azure AD Domain Services managed domain</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="cbfa2-104">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="cbfa2-104">Before you begin</span></span>
<span data-ttu-id="cbfa2-105">Győződjön meg arról, hogy befejezte [1. feladat – tanúsítvány beszerzése biztonságos LDAP](active-directory-ds-admin-guide-configure-secure-ldap.md).</span><span class="sxs-lookup"><span data-stu-id="cbfa2-105">Ensure you've completed [Task 1 - obtain a certificate for secure LDAP](active-directory-ds-admin-guide-configure-secure-ldap.md).</span></span>


## <a name="task-2---export-hello-secure-ldap-certificate-tooa-pfx-file"></a><span data-ttu-id="cbfa2-106">2. feladat – hello biztonságos LDAP tanúsítvány tooa exportálása. PFX-fájlból</span><span class="sxs-lookup"><span data-stu-id="cbfa2-106">Task 2 - export hello secure LDAP certificate tooa .PFX file</span></span>
<span data-ttu-id="cbfa2-107">Ez a feladat megkezdése előtt győződjön meg arról, beszerezte a hello biztonságos LDAP tanúsítványt nyilvános hitelesítésszolgáltatótól származó, vagy önaláírt tanúsítványt hozott létre.</span><span class="sxs-lookup"><span data-stu-id="cbfa2-107">Before you start this task, ensure that you have obtained hello secure LDAP certificate from a public certification authority or have created a self-signed certificate.</span></span>

<span data-ttu-id="cbfa2-108">Hajtsa végre az alábbi lépésekkel hello, tooexport hello LDAPS tooa tanúsítvány. PFX-fájlt.</span><span class="sxs-lookup"><span data-stu-id="cbfa2-108">Perform hello following steps, tooexport hello LDAPS certificate tooa .PFX file.</span></span>

1. <span data-ttu-id="cbfa2-109">Nyomja le az hello **Start** gombra, és írja be **R**. A hello **futtatása** párbeszédpanel, írja be **mmc** kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="cbfa2-109">Press hello **Start** button and type **R**. In hello **Run** dialog, type **mmc** and click **OK**.</span></span>

    ![Hello MMC konzol indítása](./media/active-directory-domain-services-admin-guide/secure-ldap-start-run.png)
2. <span data-ttu-id="cbfa2-111">A hello **felhasználói fiókok felügyelete** kérdés, kattintson a **Igen** toolaunch MMC (Microsoft Management Console) rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="cbfa2-111">On hello **User Account Control** prompt, click **YES** toolaunch MMC (Microsoft Management Console) as administrator.</span></span>
3. <span data-ttu-id="cbfa2-112">A hello **fájl** menüben kattintson a **beépülő modul hozzáadása/eltávolítása...** .</span><span class="sxs-lookup"><span data-stu-id="cbfa2-112">From hello **File** menu, click **Add/Remove Snap-in...**.</span></span>

    ![Adja hozzá a beépülő modul tooMMC konzol](./media/active-directory-domain-services-admin-guide/secure-ldap-add-snapin.png)
4. <span data-ttu-id="cbfa2-114">A hello **hozzáadása vagy eltávolítása a beépülő modulok** párbeszédpanelen jelölje be hello **tanúsítványok** beépülő modul hello kattintson **Hozzáadás >** gombra.</span><span class="sxs-lookup"><span data-stu-id="cbfa2-114">In hello **Add or Remove Snap-ins** dialog, select hello **Certificates** snap-in, and click hello **Add >** button.</span></span>

    ![Adja hozzá a tanúsítványok beépülő modul tooMMC konzol](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin.png)
5. <span data-ttu-id="cbfa2-116">A hello **tanúsítványok beépülő modul** varázslóban válassza **számítógépfiók** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="cbfa2-116">In hello **Certificates snap-in** wizard, select **Computer account** and click **Next**.</span></span>

    ![Tanúsítványok beépülő modul számítógép fiók hozzáadása](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-computer-account.png)
6. <span data-ttu-id="cbfa2-118">A hello **számítógép kijelölése** lapon, hogy melyik **helyi számítógép: (hello számítógépet a konzol fut)** kattintson **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="cbfa2-118">On hello **Select Computer** page, select **Local computer: (hello computer this console is running on)** and click **Finish**.</span></span>

    ![Tanúsítványok beépülő modul – jelölje be a számítógép hozzáadása](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-local-computer.png)
7. <span data-ttu-id="cbfa2-120">A hello **hozzáadása vagy eltávolítása a beépülő modulok** párbeszédpanel, kattintson a **OK** tooadd hello tanúsítványok beépülő modul tooMMC.</span><span class="sxs-lookup"><span data-stu-id="cbfa2-120">In hello **Add or Remove Snap-ins** dialog, click **OK** tooadd hello certificates snap-in tooMMC.</span></span>

    ![Adja hozzá a tanúsítványok beépülő modul tooMMC - kész](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin-done.png)
8. <span data-ttu-id="cbfa2-122">Hello MMC ablak, kattintson a tooexpand **konzolgyökér**.</span><span class="sxs-lookup"><span data-stu-id="cbfa2-122">In hello MMC window, click tooexpand **Console Root**.</span></span> <span data-ttu-id="cbfa2-123">Meg kell jelennie a tanúsítványok beépülő modul hello betöltve.</span><span class="sxs-lookup"><span data-stu-id="cbfa2-123">You should see hello Certificates snap-in loaded.</span></span> <span data-ttu-id="cbfa2-124">Kattintson a **tanúsítványok (helyi számítógép)** tooexpand.</span><span class="sxs-lookup"><span data-stu-id="cbfa2-124">Click **Certificates (Local Computer)** tooexpand.</span></span> <span data-ttu-id="cbfa2-125">Kattintson a tooexpand hello **személyes** csomópontot, majd hello **tanúsítványok** csomópont.</span><span class="sxs-lookup"><span data-stu-id="cbfa2-125">Click tooexpand hello **Personal** node, followed by hello **Certificates** node.</span></span>

    ![Nyissa meg személyes tanúsítványok tárolójában](./media/active-directory-domain-services-admin-guide/secure-ldap-open-personal-store.png)
9. <span data-ttu-id="cbfa2-127">Hello létrehozott önaláírt tanúsítványt kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="cbfa2-127">You should see hello self-signed certificate we created.</span></span> <span data-ttu-id="cbfa2-128">Láthatja, hogy hello tulajdonságainak hello tanúsítvány tooensure hello ujjlenyomat megfelelő találat jelentett hello PowerShell windows hello-tanúsítvány létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="cbfa2-128">You can examine hello properties of hello certificate tooensure hello thumbprint matches that reported on hello PowerShell windows when you created hello certificate.</span></span>
10. <span data-ttu-id="cbfa2-129">Válassza ki az önaláírt tanúsítvány hello és **kattintson a jobb gombbal**.</span><span class="sxs-lookup"><span data-stu-id="cbfa2-129">Select hello self-signed certificate and **right click**.</span></span> <span data-ttu-id="cbfa2-130">Hello helyi menüből válassza ki a **feladataival** válassza **exportálása...** .</span><span class="sxs-lookup"><span data-stu-id="cbfa2-130">From hello right-click menu, select **All Tasks** and select **Export...**.</span></span>

    ![Tanúsítvány exportálása](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert.png)
11. <span data-ttu-id="cbfa2-132">A hello **Tanúsítványexportáló varázsló**, kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="cbfa2-132">In hello **Certificate Export Wizard**, click **Next**.</span></span>

    ![Exportálja a tanúsítványt varázsló](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert-wizard.png)
12. <span data-ttu-id="cbfa2-134">A hello **titkos kulcs exportálása** lapon jelölje be **Igen, hello titkos kulcs exportálását választom**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="cbfa2-134">On hello **Export Private Key** page, select **Yes, export hello private key**, and click **Next**.</span></span>

    ![Tanúsítvány titkos kulcs exportálása](./media/active-directory-domain-services-admin-guide/secure-ldap-export-private-key.png)

    > [!WARNING]
    > <span data-ttu-id="cbfa2-136">Exportálnia kell a titkos kulcs hello hello tanúsítvány együtt.</span><span class="sxs-lookup"><span data-stu-id="cbfa2-136">You MUST export hello private key along with hello certificate.</span></span> <span data-ttu-id="cbfa2-137">Ha megad egy PFX hello hello tanúsítvány titkos kulcsa nem tartalmazó, a felügyelt tartományok biztonságos LDAP engedélyezése sikertelen.</span><span class="sxs-lookup"><span data-stu-id="cbfa2-137">If you provide a PFX that does not contain hello private key for hello certificate, enabling secure LDAP for your managed domain fails.</span></span>
    >
    >
13. <span data-ttu-id="cbfa2-138">A hello **Exportfájlformátum** lapon jelölje be **személyes információcsere - PKCS #12 (. PFX)** hello hello formátumban exportálja a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="cbfa2-138">On hello **Export File Format** page, select **Personal Information Exchange - PKCS #12 (.PFX)** as hello file format for hello exported certificate.</span></span>

    ![A tanúsítvány fájlformátuma exportálása](./media/active-directory-domain-services-admin-guide/secure-ldap-export-to-pfx.png)

    > [!NOTE]
    > <span data-ttu-id="cbfa2-140">Csak hello. PFX-fájl formátuma támogatott.</span><span class="sxs-lookup"><span data-stu-id="cbfa2-140">Only hello .PFX file format is supported.</span></span> <span data-ttu-id="cbfa2-141">Hello tanúsítvány toohello nem exportálja. CER-fájlformátum.</span><span class="sxs-lookup"><span data-stu-id="cbfa2-141">Do not export hello certificate toohello .CER file format.</span></span>
    >
    >
14. <span data-ttu-id="cbfa2-142">A hello **biztonsági** lapra, jelölje be hello **jelszó** lehetőséget és a jelszó tooprotect hello írja be. PFX-fájlt.</span><span class="sxs-lookup"><span data-stu-id="cbfa2-142">On hello **Security** page, select hello **Password** option and type in a password tooprotect hello .PFX file.</span></span> <span data-ttu-id="cbfa2-143">Ne felejtse el ezt a jelszót, mert a következő feladathoz hello lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="cbfa2-143">Remember this password since it will be needed in hello next task.</span></span> <span data-ttu-id="cbfa2-144">Kattintson a **következő** tooproceed.</span><span class="sxs-lookup"><span data-stu-id="cbfa2-144">Click **Next** tooproceed.</span></span>

    ![<span data-ttu-id="cbfa2-145">A Tanúsítványexportálás jelszó</span><span class="sxs-lookup"><span data-stu-id="cbfa2-145">Password for certificate export</span></span> ](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-password.png)

    > [!NOTE]
    > <span data-ttu-id="cbfa2-146">Jegyezze fel ezt a jelszót.</span><span class="sxs-lookup"><span data-stu-id="cbfa2-146">Make a note of this password.</span></span> <span data-ttu-id="cbfa2-147">A felügyelt tartomány biztonságos LDAP engedélyezése során szüksége [3. feladat – hello felügyelt tartomány biztonságos LDAP engedélyezése](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span><span class="sxs-lookup"><span data-stu-id="cbfa2-147">You need it while enabling secure LDAP for this managed domain in [Task 3 - enable secure LDAP for hello managed domain](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span></span>
    >
    >
15. <span data-ttu-id="cbfa2-148">A hello **fájl tooExport** hello nevét és helyét, hol szeretné tooexport hello tanúsítvány adja meg azokat.</span><span class="sxs-lookup"><span data-stu-id="cbfa2-148">On hello **File tooExport** page, specify hello file name and location where you'd like tooexport hello certificate.</span></span>

    ![A Tanúsítványexportálás elérési útja](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-path.png)
16. <span data-ttu-id="cbfa2-150">Hello a következő lapon kattintson **Befejezés** tooexport hello tooa PFX tanúsítványfájl.</span><span class="sxs-lookup"><span data-stu-id="cbfa2-150">On hello following page, click **Finish** tooexport hello certificate tooa PFX file.</span></span> <span data-ttu-id="cbfa2-151">Hello tanúsítvány exportálásakor meg kell jelennie a megerősítő párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="cbfa2-151">You should see confirmation dialog when hello certificate has been exported.</span></span>

    ![Kész tanúsítvány exportálása](./media/active-directory-domain-services-admin-guide/secure-ldap-exported-as-pfx.png)


## <a name="next-step"></a><span data-ttu-id="cbfa2-153">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="cbfa2-153">Next step</span></span>
[<span data-ttu-id="cbfa2-154">3. feladat – hello felügyelt tartomány biztonságos LDAP engedélyezése</span><span class="sxs-lookup"><span data-stu-id="cbfa2-154">Task 3 - enable secure LDAP for hello managed domain</span></span>](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)
