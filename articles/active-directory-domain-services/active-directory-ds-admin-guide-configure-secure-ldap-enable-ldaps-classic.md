---
title: "Biztonságos LDAP (LDAPS) konfigurálása az Azure AD tartományi szolgáltatásokban |} Microsoft Docs"
description: "Biztonságos LDAP (LDAPS) konfigurálása az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 9d563c45-9578-410d-96c8-355af64feae8
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: maheshu
ms.openlocfilehash: 3aafe209aad7383cd0610d147b5fdba673023c93
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a><span data-ttu-id="c309f-103">Biztonságos LDAP (LDAPS) használatos az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz tartozó konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c309f-103">Configure secure LDAP (LDAPS) for an Azure AD Domain Services managed domain</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="c309f-104">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="c309f-104">Before you begin</span></span>
<span data-ttu-id="c309f-105">Győződjön meg arról, hogy befejezte [2. feladat – a biztonságos LDAP tanúsítvány exportálása a. PFX-fájl](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md).</span><span class="sxs-lookup"><span data-stu-id="c309f-105">Ensure you've completed [Task 2 - export the secure LDAP certificate to a .PFX file](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md).</span></span>

<span data-ttu-id="c309f-106">Válassza ki, hogy az Azure portál felhasználói élmény előzetes vagy a klasszikus Azure portálon segítségével befejezheti a feladatot.</span><span class="sxs-lookup"><span data-stu-id="c309f-106">Choose whether to use the preview Azure portal experience or the Azure classic portal to complete this task.</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="c309f-107">**Azure-portálon (előzetes verzió)**: [engedélyezése biztonságos LDAP az Azure portál használatával](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span><span class="sxs-lookup"><span data-stu-id="c309f-107">**Azure portal (Preview)**: [Enable secure LDAP using the Azure portal](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span></span>
> * <span data-ttu-id="c309f-108">**A klasszikus Azure portálon**: [engedélyezése biztonságos LDAP a klasszikus Azure portál használatával](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps-classic.md)</span><span class="sxs-lookup"><span data-stu-id="c309f-108">**Azure classic portal**: [Enable secure LDAP using the classic Azure portal](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps-classic.md)</span></span>
>
>


## <a name="task-3---enable-secure-ldap-for-the-managed-domain-using-the-classic-azure-portal"></a><span data-ttu-id="c309f-109">3. feladat – a felügyelt tartomány, a klasszikus Azure portál használatával biztonságos LDAP engedélyezése</span><span class="sxs-lookup"><span data-stu-id="c309f-109">Task 3 - enable secure LDAP for the managed domain using the classic Azure portal</span></span>
<span data-ttu-id="c309f-110">Ahhoz, hogy a biztonságos LDAP, hajtsa végre az alábbi konfigurációs lépéseket:</span><span class="sxs-lookup"><span data-stu-id="c309f-110">To enable secure LDAP, perform the following configuration steps:</span></span>

1. <span data-ttu-id="c309f-111">Keresse meg a  **[a klasszikus Azure portálon](https://manage.windowsazure.com)**.</span><span class="sxs-lookup"><span data-stu-id="c309f-111">Navigate to the **[Azure classic portal](https://manage.windowsazure.com)**.</span></span>
2. <span data-ttu-id="c309f-112">Válassza az **Active Directory** csomópontot a bal oldali panelen.</span><span class="sxs-lookup"><span data-stu-id="c309f-112">Select the **Active Directory** node on the left pane.</span></span>
3. <span data-ttu-id="c309f-113">Válassza ki az Azure AD-címtár (más néven "tenant"), amelyhez engedélyezte az Azure AD tartományi szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="c309f-113">Select the Azure AD directory (also referred to as 'tenant'), for which you have enabled Azure AD Domain Services.</span></span>

    ![Azure AD címtár kiválasztása](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. <span data-ttu-id="c309f-115">Kattintson a **Configure** (Konfigurálás) lapra.</span><span class="sxs-lookup"><span data-stu-id="c309f-115">Click the **Configure** tab.</span></span>

    ![Címtár lapjának konfigurálása](./media/active-directory-domain-services-getting-started/configure-tab.png)
5. <span data-ttu-id="c309f-117">Görgessen le a című **tartományi szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="c309f-117">Scroll down to the section titled **domain services**.</span></span> <span data-ttu-id="c309f-118">Megjelenik egy lehetőség, című **biztonságos LDAP (LDAPS)** az alábbi képernyőfelvételen látható módon:</span><span class="sxs-lookup"><span data-stu-id="c309f-118">You should see an option titled **Secure LDAP (LDAPS)** as shown in the following screenshot:</span></span>

    ![Tartományi szolgáltatások konfigurációja szakasz](./media/active-directory-domain-services-admin-guide/secure-ldap-start.png)
6. <span data-ttu-id="c309f-120">Kattintson a **tanúsítvány konfigurálása...**  gombra kattintva nyissa meg a **tanúsítvány konfigurálása a biztonságos LDAP** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c309f-120">Click the **Configure certificate ...** button to bring up the **Configure Certificate for Secure LDAP** dialog.</span></span>

    ![Biztonságos LDAP-tanúsítvány konfigurálása](./media/active-directory-domain-services-admin-guide/secure-ldap-configure-cert-page.png)
7. <span data-ttu-id="c309f-122">Kattintson a mappa ikon következő **PFX-fájl a tanúsítvány** adhatja meg a PFX-fájlt, amely tartalmazza a felügyelt tartományra biztonságos LDAP eléréséhez használni kívánt tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="c309f-122">Click the folder icon following **PFX FILE WITH CERTIFICATE** to specify the PFX file, which contains the certificate you wish to use for secure LDAP access to the managed domain.</span></span> <span data-ttu-id="c309f-123">Ha a tanúsítvány exportálása PFX-fájl a megadott jelszó is beírhatja.</span><span class="sxs-lookup"><span data-stu-id="c309f-123">Also enter the password you specified when exporting the certificate to the PFX file.</span></span> <span data-ttu-id="c309f-124">Kattintson az alul a Kész gombra.</span><span class="sxs-lookup"><span data-stu-id="c309f-124">Then, click the done button on the bottom.</span></span>

    ![Biztonságos LDAP PFX-fájlt és a jelszó megadása](./media/active-directory-domain-services-admin-guide/secure-ldap-specify-pfx.png)
8. <span data-ttu-id="c309f-126">A **tartományi szolgáltatások** szakasza a **konfigurálása** lapon kell beolvasni szürkén jelenik meg, és a **folyamatban...**  állapot néhány percig.</span><span class="sxs-lookup"><span data-stu-id="c309f-126">The **domain services** section of the **Configure** tab should get grayed out and is in the **Pending...** state for a few minutes.</span></span> <span data-ttu-id="c309f-127">Ebben az időszakban a LDAPS tanúsítvány pontosságát ellenőrizve, és biztonságos LDAP konfigurálva van a felügyelt tartományok.</span><span class="sxs-lookup"><span data-stu-id="c309f-127">During this period, the LDAPS certificate is verified for accuracy and secure LDAP is configured for your managed domain.</span></span>

    ![Biztonságos LDAP - függő állapot](./media/active-directory-domain-services-admin-guide/secure-ldap-pending-state.png)

   > [!NOTE]
   > <span data-ttu-id="c309f-129">Ahhoz, hogy a felügyelt tartományok biztonságos LDAP körülbelül 10 – 15 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="c309f-129">It takes about 10 to 15 minutes to enable secure LDAP for your managed domain.</span></span> <span data-ttu-id="c309f-130">A megadott biztonságos LDAP-tanúsítvány nem felel meg a szükséges feltételeknek, ha biztonságos LDAP nincs engedélyezve a címtárban, és láthatja a hibát.</span><span class="sxs-lookup"><span data-stu-id="c309f-130">If the provided secure LDAP certificate does not match the required criteria, secure LDAP is not enabled for your directory and you see a failure.</span></span> <span data-ttu-id="c309f-131">Ha például a tartomány neve nem megfelelő, a tanúsítvány már lejárt vagy hamarosan lejár.</span><span class="sxs-lookup"><span data-stu-id="c309f-131">For example, the domain name is incorrect, the certificate has already expired or expires soon.</span></span>
   >
   >

9. <span data-ttu-id="c309f-132">Ha a felügyelt tartományok sikeresen engedélyezve a biztonságos LDAP a **folyamatban...**  üzenet kell eltűnik.</span><span class="sxs-lookup"><span data-stu-id="c309f-132">When secure LDAP is successfully enabled for your managed domain, the **Pending...** message should disappear.</span></span> <span data-ttu-id="c309f-133">Meg kell jelennie jelenik meg a tanúsítvány ujjlenyomatát.</span><span class="sxs-lookup"><span data-stu-id="c309f-133">You should see the thumbprint of the certificate displayed.</span></span>

    ![Biztonságos LDAP engedélyezve](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled.png)

<br>

## <a name="task-4---enable-secure-ldap-access-over-the-internet"></a><span data-ttu-id="c309f-135">4. feladat – az interneten keresztül biztonságos LDAP-hozzáférés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="c309f-135">Task 4 - enable secure LDAP access over the internet</span></span>
<span data-ttu-id="c309f-136">**Nem kötelező** – Ha nem tervezi, hogy a felügyelt tartományra LDAPS az interneten keresztül elérni, hagyja ki a konfigurációs feladat.</span><span class="sxs-lookup"><span data-stu-id="c309f-136">**Optional task** - If you do not plan to access the managed domain using LDAPS over the internet, skip this configuration task.</span></span>

<span data-ttu-id="c309f-137">Ez a feladat megkezdése előtt győződjön meg arról, befejezte a leírt lépéseket [3. feladat](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-classic-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="c309f-137">Before you begin this task, ensure you have completed the steps outlined in [Task 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-classic-azure-portal).</span></span>

1. <span data-ttu-id="c309f-138">Megjelenik egy lehetőség, hogy **ENGEDÉLYEZNI biztonságos LDAP hozzáférés keresztül interneten keresztül** a a **tartományi szolgáltatások** szakasza a **konfigurálása** lap.</span><span class="sxs-lookup"><span data-stu-id="c309f-138">You should see an option to **ENABLE SECURE LDAP ACCESS OVER THE INTERNET** in the **domain services** section of the **Configure** page.</span></span> <span data-ttu-id="c309f-139">Ez a beállítás értéke **nem** mivel internet-hozzáféréssel a felügyelt tartományra keresztül biztonságos LDAP alapértelmezés szerint le van tiltva alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="c309f-139">This option is set to **NO** by default since internet access to the managed domain over secure LDAP is disabled by default.</span></span>

    ![Biztonságos LDAP engedélyezve](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled2.png)
2. <span data-ttu-id="c309f-141">Váltás **biztonságos LDAP-hozzáférés engedélyezése az INTERNETEN keresztül** való **Igen**.</span><span class="sxs-lookup"><span data-stu-id="c309f-141">Toggle **ENABLE SECURE LDAP ACCESS OVER THE INTERNET** to **YES**.</span></span> <span data-ttu-id="c309f-142">Kattintson a **mentése** gomb alsó panelje.</span><span class="sxs-lookup"><span data-stu-id="c309f-142">Click the **SAVE** button on the bottom panel.</span></span>
    <span data-ttu-id="c309f-143">![Biztonságos LDAP engedélyezve](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access.png)</span><span class="sxs-lookup"><span data-stu-id="c309f-143">![Secure LDAP enabled](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access.png)</span></span>
3. <span data-ttu-id="c309f-144">A **tartományi szolgáltatások** szakasza a **konfigurálása** lapon kell beolvasni szürkén jelenik meg, és a **folyamatban...**  állapot néhány percig.</span><span class="sxs-lookup"><span data-stu-id="c309f-144">The **domain services** section of the **Configure** tab should get grayed out and is in the **Pending...** state for a few minutes.</span></span> <span data-ttu-id="c309f-145">Némi várakozás után internet-hozzáféréssel a felügyelt tartományra keresztül biztonságos LDAP engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="c309f-145">After some time, internet access to your managed domain over secure LDAP is enabled.</span></span>

    ![Biztonságos LDAP - függő állapot](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access-pending-state.png)

   > [!NOTE]
   > <span data-ttu-id="c309f-147">Internet-hozzáférés engedélyezése a felügyelt tartományok biztonságos LDAP körülbelül 10 percig tart.</span><span class="sxs-lookup"><span data-stu-id="c309f-147">It takes about 10 minutes to enable internet access over secure LDAP for your managed domain.</span></span>
   >
   >
4. <span data-ttu-id="c309f-148">Ha az interneten keresztül a felügyelt tartományra biztonságos LDAP hozzáférés sikeresen engedélyezve van, a **folyamatban...**  üzenet kell eltűnik.</span><span class="sxs-lookup"><span data-stu-id="c309f-148">When secure LDAP access to your managed domain over the internet is successfully enabled, the **Pending...** message should disappear.</span></span> <span data-ttu-id="c309f-149">A külső IP-cím, a mezőben LDAPS keresztül a könyvtár eléréséhez használható kell megjelennie **külső IP-cím a LDAPS ELÉRÉST**.</span><span class="sxs-lookup"><span data-stu-id="c309f-149">You should see the external IP address that can be used to access your directory over LDAPS in the field **EXTERNAL IP ADDRESS FOR LDAPS ACCESS**.</span></span>

    ![Biztonságos LDAP engedélyezve](./media/active-directory-domain-services-admin-guide/secure-ldap-internet-access-enabled.png)

<br>

## <a name="task-5---configure-dns-to-access-the-managed-domain-from-the-internet"></a><span data-ttu-id="c309f-151">5. feladat – az internetről a felügyelt tartomány eléréséhez DNS konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c309f-151">Task 5 - configure DNS to access the managed domain from the internet</span></span>
<span data-ttu-id="c309f-152">**Nem kötelező** – Ha nem tervezi, hogy a felügyelt tartományra LDAPS az interneten keresztül elérni, hagyja ki a konfigurációs feladat.</span><span class="sxs-lookup"><span data-stu-id="c309f-152">**Optional task** - If you do not plan to access the managed domain using LDAPS over the internet, skip this configuration task.</span></span>

<span data-ttu-id="c309f-153">Ez a feladat megkezdése előtt győződjön meg arról, befejezte a leírt lépéseket [4. feladat](#task-4---enable-secure-ldap-access-over-the-internet).</span><span class="sxs-lookup"><span data-stu-id="c309f-153">Before you begin this task, ensure you have completed the steps outlined in [Task 4](#task-4---enable-secure-ldap-access-over-the-internet).</span></span>

<span data-ttu-id="c309f-154">Miután engedélyezte biztonságos LDAP hozzáférést az interneten keresztül a felügyelt tartományok, módosítania DNS, hogy a felügyelt tartomány található ügyfélszámítógépek számára.</span><span class="sxs-lookup"><span data-stu-id="c309f-154">Once you have enabled secure LDAP access over the internet for your managed domain, you need to update DNS so that client computers can find this managed domain.</span></span> <span data-ttu-id="c309f-155">4. feladat végén külső IP-cím jelenik meg a **konfigurálása** lapján **külső IP-cím a LDAPS ELÉRÉST**.</span><span class="sxs-lookup"><span data-stu-id="c309f-155">At the end of task 4, an external IP address is displayed on the **Configure** tab in **EXTERNAL IP ADDRESS FOR LDAPS ACCESS**.</span></span>

<span data-ttu-id="c309f-156">Konfigurálja a külső DNS-szolgáltatónál, hogy a külső IP-címre mutat a felügyelt tartomány (például ldaps.contoso100.com) DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="c309f-156">Configure your external DNS provider so that the DNS name of the managed domain (for example, 'ldaps.contoso100.com') points to this external IP address.</span></span> <span data-ttu-id="c309f-157">A jelen példában kell a következő DNS-bejegyzés létrehozása:</span><span class="sxs-lookup"><span data-stu-id="c309f-157">In our example, we need to create the following DNS entry:</span></span>

    ldaps.contoso100.com  -> 52.165.38.113

<span data-ttu-id="c309f-158">Ennyi az egész – most már készen áll a felügyelt tartományra biztonságos LDAP az interneten keresztül csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="c309f-158">That's it - you are now ready to connect to the managed domain using secure LDAP over the internet.</span></span>

> [!WARNING]
> <span data-ttu-id="c309f-159">Ne feledje, hogy az ügyfél számára megbízhatónak kell lennie a LDAPS tanúsítvány kiállítója tud sikeresen csatlakozni a felügyelt tartományra LDAPS kell.</span><span class="sxs-lookup"><span data-stu-id="c309f-159">Remember that client computers must trust the issuer of the LDAPS certificate to be able to connect successfully to the managed domain using LDAPS.</span></span> <span data-ttu-id="c309f-160">Ha egy vállalati hitelesítésszolgáltatótól vagy egy nyilvánosan megbízható hitelesítésszolgáltató használ, nem kell semmit, mivel az ügyfélszámítógépek számára megbízhatónak ezek tanúsítványkiállítók.</span><span class="sxs-lookup"><span data-stu-id="c309f-160">If you are using an enterprise certification authority or a publicly trusted certification authority, you do not need to do anything since client computers trust these certificate issuers.</span></span> <span data-ttu-id="c309f-161">Ha önaláírt tanúsítványt használ, telepítendő önaláírt tanúsítvány nyilvános részét az ügyfélszámítógépen, a megbízható tanúsítványok tárolójába.</span><span class="sxs-lookup"><span data-stu-id="c309f-161">If you are using a self-signed certificate, you need to install the public part of the self-signed certificate into the trusted certificate store on the client computer.</span></span>
>
>


## <a name="lock-down-ldaps-access-to-your-managed-domain-over-the-internet"></a><span data-ttu-id="c309f-162">Zárolási-le nyilas LDAPS hozzáféréssel a felügyelt tartományok az interneten keresztül</span><span class="sxs-lookup"><span data-stu-id="c309f-162">Lock-down LDAPS access to your managed domain over the internet</span></span>
> [!NOTE]
> <span data-ttu-id="c309f-163">**Nem kötelező** - LDAPS hozzáféréssel a felügyelt tartományra nincs engedélyezve az interneten keresztül, ha a konfigurációs feladat kihagyása.</span><span class="sxs-lookup"><span data-stu-id="c309f-163">**Optional task** - If you have not enabled LDAPS access to the managed domain over the internet, skip this configuration task.</span></span>
>
>

<span data-ttu-id="c309f-164">Ez a feladat megkezdése előtt győződjön meg arról, befejezte a leírt lépéseket [4. feladat](#task-4---enable-secure-ldap-access-over-the-internet).</span><span class="sxs-lookup"><span data-stu-id="c309f-164">Before you begin this task, ensure you have completed the steps outlined in [Task 4](#task-4---enable-secure-ldap-access-over-the-internet).</span></span>

<span data-ttu-id="c309f-165">A felügyelt tartományok LDAPS hozzáférési közzéteszi az interneten keresztül jelenti a biztonsági kockázatot jelentenek.</span><span class="sxs-lookup"><span data-stu-id="c309f-165">Exposing your managed domain for LDAPS access over the internet represents a security threat.</span></span> <span data-ttu-id="c309f-166">A felügyelt tartományra érhető el az interneten, a biztonságos LDAP használt port (Ez azt jelenti, hogy a 636-os port).</span><span class="sxs-lookup"><span data-stu-id="c309f-166">The managed domain is reachable from the internet at the port used for secure LDAP (that is, port 636).</span></span> <span data-ttu-id="c309f-167">Ezért ha szeretné, korlátozza a hozzáférést a felügyelt tartományra ismert IP-címek.</span><span class="sxs-lookup"><span data-stu-id="c309f-167">Therefore, you can choose to restrict access to the managed domain to specific known IP addresses.</span></span> <span data-ttu-id="c309f-168">A nagyobb biztonság érdekében hozzon létre egy hálózati biztonsági csoportot (NSG), és rendelje hozzá azt az alhálózatot, ha engedélyezte az Azure AD tartományi szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="c309f-168">For improved security, create a network security group (NSG) and associate it with the subnet where you have enabled Azure AD Domain Services.</span></span>

<span data-ttu-id="c309f-169">Az alábbi táblázat mutatja be egy minta NSG-t is konfigurálhat, biztonságos LDAP hozzáférést zárolását az interneten keresztül.</span><span class="sxs-lookup"><span data-stu-id="c309f-169">The following table illustrates a sample NSG you can configure, to lock down secure LDAP access over the internet.</span></span> <span data-ttu-id="c309f-170">Az NSG tartalmaz egy TCP-porton keresztül 636 csak egy megadott készletből IP-címek bejövő LDAPS hozzáférést engedélyező szabályokat.</span><span class="sxs-lookup"><span data-stu-id="c309f-170">The NSG contains a set of rules that allow inbound LDAPS access over TCP port 636 only from a specified set of IP addresses.</span></span> <span data-ttu-id="c309f-171">Minden bejövő forgalom az internetről az alapértelmezett "DenyAll" szabály vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="c309f-171">The default 'DenyAll' rule applies to all other inbound traffic from the internet.</span></span> <span data-ttu-id="c309f-172">Az NSG-szabály LDAPS hozzáférést a megadott IP-címeket az interneten keresztül rendelkezik, magasabb prioritású, mint a DenyAll NSG-szabály.</span><span class="sxs-lookup"><span data-stu-id="c309f-172">The NSG rule to allow LDAPS access over the internet from specified IP addresses has a higher priority than the DenyAll NSG rule.</span></span>

![Minta NSG LDAPS hozzáférés biztonságossá az interneten keresztül](./media/active-directory-domain-services-admin-guide/secure-ldap-sample-nsg.png)

<span data-ttu-id="c309f-174">**További információ** - [hálózati biztonsági csoportok](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="c309f-174">**More information** - [Network security groups](../virtual-network/virtual-networks-nsg.md).</span></span>

<br>

## <a name="related-content"></a><span data-ttu-id="c309f-175">Kapcsolódó tartalom</span><span class="sxs-lookup"><span data-stu-id="c309f-175">Related content</span></span>
* [<span data-ttu-id="c309f-176">Azure AD tartományi szolgáltatások – első lépések útmutató</span><span class="sxs-lookup"><span data-stu-id="c309f-176">Azure AD Domain Services - Getting Started guide</span></span>](active-directory-ds-getting-started.md)
* [<span data-ttu-id="c309f-177">Azure AD tartományi szolgáltatások által kezelt tartomány felügyelete</span><span class="sxs-lookup"><span data-stu-id="c309f-177">Administer an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-administer-domain.md)
* [<span data-ttu-id="c309f-178">Csoportházirend a egy Azure AD tartományi szolgáltatások által kezelt tartomány felügyelete</span><span class="sxs-lookup"><span data-stu-id="c309f-178">Administer Group Policy on an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-administer-group-policy.md)
* [<span data-ttu-id="c309f-179">Hálózati biztonsági csoportok</span><span class="sxs-lookup"><span data-stu-id="c309f-179">Network security groups</span></span>](../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="c309f-180">Hálózati biztonsági csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="c309f-180">Create a Network Security Group</span></span>](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)
