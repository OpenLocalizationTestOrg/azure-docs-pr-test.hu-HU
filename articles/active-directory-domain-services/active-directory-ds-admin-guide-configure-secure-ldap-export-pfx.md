---
title: "Biztonságos LDAP (LDAPS) konfigurálása az Azure AD tartományi szolgáltatásokban |} Microsoft Docs"
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
ms.openlocfilehash: 5d46f376d46b8bbf3f93de57a7d4e31abdbcdb2f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a><span data-ttu-id="10e1a-103">Biztonságos LDAP (LDAPS) használatos az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz tartozó konfigurálása</span><span class="sxs-lookup"><span data-stu-id="10e1a-103">Configure secure LDAP (LDAPS) for an Azure AD Domain Services managed domain</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="10e1a-104">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="10e1a-104">Before you begin</span></span>
<span data-ttu-id="10e1a-105">Győződjön meg arról, hogy befejezte [1. feladat – tanúsítvány beszerzése biztonságos LDAP](active-directory-ds-admin-guide-configure-secure-ldap.md).</span><span class="sxs-lookup"><span data-stu-id="10e1a-105">Ensure you've completed [Task 1 - obtain a certificate for secure LDAP](active-directory-ds-admin-guide-configure-secure-ldap.md).</span></span>


## <a name="task-2---export-the-secure-ldap-certificate-to-a-pfx-file"></a><span data-ttu-id="10e1a-106">2. feladat – a biztonságos LDAP tanúsítvány exportálása a. PFX-fájlból</span><span class="sxs-lookup"><span data-stu-id="10e1a-106">Task 2 - export the secure LDAP certificate to a .PFX file</span></span>
<span data-ttu-id="10e1a-107">Ez a feladat megkezdése előtt győződjön meg arról, szerzett be a biztonságos LDAP tanúsítványt nyilvános hitelesítésszolgáltatótól származó, vagy önaláírt tanúsítványt hozott létre.</span><span class="sxs-lookup"><span data-stu-id="10e1a-107">Before you start this task, ensure that you have obtained the secure LDAP certificate from a public certification authority or have created a self-signed certificate.</span></span>

<span data-ttu-id="10e1a-108">A következő lépésekkel, a LDAPS tanúsítvány exportálása a. PFX-fájlt.</span><span class="sxs-lookup"><span data-stu-id="10e1a-108">Perform the following steps, to export the LDAPS certificate to a .PFX file.</span></span>

1. <span data-ttu-id="10e1a-109">Nyomja meg a **Start** gombra, és írja be **R**. Az a **futtatása** párbeszédpanel, írja be **mmc** kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="10e1a-109">Press the **Start** button and type **R**. In the **Run** dialog, type **mmc** and click **OK**.</span></span>

    ![Nyissa meg az MMC konzolt](./media/active-directory-domain-services-admin-guide/secure-ldap-start-run.png)
2. <span data-ttu-id="10e1a-111">Az a **felhasználói fiókok felügyelete** kérdés, kattintson a **Igen** rendszergazdaként (Microsoft Management Console) MMC indítása.</span><span class="sxs-lookup"><span data-stu-id="10e1a-111">On the **User Account Control** prompt, click **YES** to launch MMC (Microsoft Management Console) as administrator.</span></span>
3. <span data-ttu-id="10e1a-112">Az a **fájl** menüben kattintson a **beépülő modul hozzáadása/eltávolítása...** .</span><span class="sxs-lookup"><span data-stu-id="10e1a-112">From the **File** menu, click **Add/Remove Snap-in...**.</span></span>

    ![Az MMC-konzolt a beépülő modul hozzáadása](./media/active-directory-domain-services-admin-guide/secure-ldap-add-snapin.png)
4. <span data-ttu-id="10e1a-114">Az a **hozzáadása vagy eltávolítása a beépülő modulok** párbeszédpanelen válassza a **tanúsítványok** beépülő modult, majd kattintson a **Hozzáadás >** gombra.</span><span class="sxs-lookup"><span data-stu-id="10e1a-114">In the **Add or Remove Snap-ins** dialog, select the **Certificates** snap-in, and click the **Add >** button.</span></span>

    ![Az MMC konzol Tanúsítványok beépülő modul hozzáadása](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin.png)
5. <span data-ttu-id="10e1a-116">Az a **tanúsítványok beépülő modul** varázslóban válassza **számítógépfiók** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="10e1a-116">In the **Certificates snap-in** wizard, select **Computer account** and click **Next**.</span></span>

    ![Tanúsítványok beépülő modul számítógép fiók hozzáadása](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-computer-account.png)
6. <span data-ttu-id="10e1a-118">Az a **számítógép kijelölése** lapon, hogy melyik **helyi számítógép: (a számítógép a konzol fut)** kattintson **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="10e1a-118">On the **Select Computer** page, select **Local computer: (the computer this console is running on)** and click **Finish**.</span></span>

    ![Tanúsítványok beépülő modul – jelölje be a számítógép hozzáadása](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-local-computer.png)
7. <span data-ttu-id="10e1a-120">Az a **hozzáadása vagy eltávolítása a beépülő modulok** párbeszédpanel, kattintson a **OK** hozzáadása a tanúsítványok beépülő MMC-hez.</span><span class="sxs-lookup"><span data-stu-id="10e1a-120">In the **Add or Remove Snap-ins** dialog, click **OK** to add the certificates snap-in to MMC.</span></span>

    ![Tanúsítványok beépülő MMC - végzett hozzáadása](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin-done.png)
8. <span data-ttu-id="10e1a-122">Az MMC ablak Ide kattintva kibonthatja a **konzolgyökér**.</span><span class="sxs-lookup"><span data-stu-id="10e1a-122">In the MMC window, click to expand **Console Root**.</span></span> <span data-ttu-id="10e1a-123">Meg kell jelennie a tanúsítványok beépülő modul betöltése.</span><span class="sxs-lookup"><span data-stu-id="10e1a-123">You should see the Certificates snap-in loaded.</span></span> <span data-ttu-id="10e1a-124">Kattintson a **tanúsítványok (helyi számítógép)** kibontásához.</span><span class="sxs-lookup"><span data-stu-id="10e1a-124">Click **Certificates (Local Computer)** to expand.</span></span> <span data-ttu-id="10e1a-125">Ide kattintva bontsa ki a **személyes** csomópontot, majd a **tanúsítványok** csomópont.</span><span class="sxs-lookup"><span data-stu-id="10e1a-125">Click to expand the **Personal** node, followed by the **Certificates** node.</span></span>

    ![Nyissa meg személyes tanúsítványok tárolójában](./media/active-directory-domain-services-admin-guide/secure-ldap-open-personal-store.png)
9. <span data-ttu-id="10e1a-127">Meg kell jelenniük a létrehozott önaláírt tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="10e1a-127">You should see the self-signed certificate we created.</span></span> <span data-ttu-id="10e1a-128">A tanúsítvány meg arról, hogy az ujjlenyomat, amely a PowerShell windows jelentett létrehozása után a tanúsítvány tulajdonságainak ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="10e1a-128">You can examine the properties of the certificate to ensure the thumbprint matches that reported on the PowerShell windows when you created the certificate.</span></span>
10. <span data-ttu-id="10e1a-129">Válassza ki az önaláírt tanúsítványt, és **kattintson a jobb gombbal**.</span><span class="sxs-lookup"><span data-stu-id="10e1a-129">Select the self-signed certificate and **right click**.</span></span> <span data-ttu-id="10e1a-130">A helyi menüben válassza ki a **feladataival** válassza **exportálása...** .</span><span class="sxs-lookup"><span data-stu-id="10e1a-130">From the right-click menu, select **All Tasks** and select **Export...**.</span></span>

    ![Tanúsítvány exportálása](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert.png)
11. <span data-ttu-id="10e1a-132">Az a **Tanúsítványexportáló varázsló**, kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="10e1a-132">In the **Certificate Export Wizard**, click **Next**.</span></span>

    ![Exportálja a tanúsítványt varázsló](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert-wizard.png)
12. <span data-ttu-id="10e1a-134">Az a **titkos kulcs exportálása** lapon jelölje be **Igen, a titkos kulcs exportálását választom**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="10e1a-134">On the **Export Private Key** page, select **Yes, export the private key**, and click **Next**.</span></span>

    ![Tanúsítvány titkos kulcs exportálása](./media/active-directory-domain-services-admin-guide/secure-ldap-export-private-key.png)

    > [!WARNING]
    > <span data-ttu-id="10e1a-136">Exportálnia kell a titkos kulcsot, valamint a tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="10e1a-136">You MUST export the private key along with the certificate.</span></span> <span data-ttu-id="10e1a-137">Ha megad egy PFX a tanúsítványhoz tartozó titkos kulcs nem tartalmazó, a felügyelt tartományok biztonságos LDAP engedélyezése sikertelen.</span><span class="sxs-lookup"><span data-stu-id="10e1a-137">If you provide a PFX that does not contain the private key for the certificate, enabling secure LDAP for your managed domain fails.</span></span>
    >
    >
13. <span data-ttu-id="10e1a-138">Az a **Exportfájlformátum** lapon jelölje be **személyes információcsere - PKCS #12 (. PFX)** , a fájlformátum az exportált tanúsítványhoz.</span><span class="sxs-lookup"><span data-stu-id="10e1a-138">On the **Export File Format** page, select **Personal Information Exchange - PKCS #12 (.PFX)** as the file format for the exported certificate.</span></span>

    ![A tanúsítvány fájlformátuma exportálása](./media/active-directory-domain-services-admin-guide/secure-ldap-export-to-pfx.png)

    > [!NOTE]
    > <span data-ttu-id="10e1a-140">Csak a. PFX-fájl formátuma támogatott.</span><span class="sxs-lookup"><span data-stu-id="10e1a-140">Only the .PFX file format is supported.</span></span> <span data-ttu-id="10e1a-141">Nem exportálja a tanúsítványt a. CER-fájlformátum.</span><span class="sxs-lookup"><span data-stu-id="10e1a-141">Do not export the certificate to the .CER file format.</span></span>
    >
    >
14. <span data-ttu-id="10e1a-142">Az a **biztonsági** lapon jelölje be a **jelszó** lehetőséget és a jelszót írja be védelméhez a. PFX-fájlt.</span><span class="sxs-lookup"><span data-stu-id="10e1a-142">On the **Security** page, select the **Password** option and type in a password to protect the .PFX file.</span></span> <span data-ttu-id="10e1a-143">Ne felejtse el ezt a jelszót, mert a következő feladat lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="10e1a-143">Remember this password since it will be needed in the next task.</span></span> <span data-ttu-id="10e1a-144">Kattintson a **tovább** a folytatáshoz.</span><span class="sxs-lookup"><span data-stu-id="10e1a-144">Click **Next** to proceed.</span></span>

    ![<span data-ttu-id="10e1a-145">A Tanúsítványexportálás jelszó</span><span class="sxs-lookup"><span data-stu-id="10e1a-145">Password for certificate export</span></span> ](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-password.png)

    > [!NOTE]
    > <span data-ttu-id="10e1a-146">Jegyezze fel ezt a jelszót.</span><span class="sxs-lookup"><span data-stu-id="10e1a-146">Make a note of this password.</span></span> <span data-ttu-id="10e1a-147">A felügyelt tartomány biztonságos LDAP engedélyezése során szüksége [3. feladat – a felügyelt tartomány számára biztonságos LDAP engedélyezése](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span><span class="sxs-lookup"><span data-stu-id="10e1a-147">You need it while enabling secure LDAP for this managed domain in [Task 3 - enable secure LDAP for the managed domain](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span></span>
    >
    >
15. <span data-ttu-id="10e1a-148">Az a **exportálandó fájl** csoportjában adja meg a fájl nevét és helyét, hol szeretné, exportálja a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="10e1a-148">On the **File to Export** page, specify the file name and location where you'd like to export the certificate.</span></span>

    ![A Tanúsítványexportálás elérési útja](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-path.png)
16. <span data-ttu-id="10e1a-150">Kattintson a következő lap **Befejezés** exportálja a tanúsítványt egy PFX-fájl.</span><span class="sxs-lookup"><span data-stu-id="10e1a-150">On the following page, click **Finish** to export the certificate to a PFX file.</span></span> <span data-ttu-id="10e1a-151">Amikor exportálta a tanúsítványt meg kell jelennie a megerősítő párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="10e1a-151">You should see confirmation dialog when the certificate has been exported.</span></span>

    ![Kész tanúsítvány exportálása](./media/active-directory-domain-services-admin-guide/secure-ldap-exported-as-pfx.png)


## <a name="next-step"></a><span data-ttu-id="10e1a-153">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="10e1a-153">Next step</span></span>
[<span data-ttu-id="10e1a-154">3. feladat – a felügyelt tartomány számára biztonságos LDAP engedélyezése</span><span class="sxs-lookup"><span data-stu-id="10e1a-154">Task 3 - enable secure LDAP for the managed domain</span></span>](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)
