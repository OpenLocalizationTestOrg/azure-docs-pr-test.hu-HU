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
ms.date: 08/14/2017
ms.author: maheshu
ms.openlocfilehash: 3b19f078b0d6dc3e02d951014056406fd1b099a8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a><span data-ttu-id="9aade-103">Biztonságos LDAP (LDAPS) használatos az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz tartozó konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9aade-103">Configure secure LDAP (LDAPS) for an Azure AD Domain Services managed domain</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="9aade-104">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="9aade-104">Before you begin</span></span>
<span data-ttu-id="9aade-105">Győződjön meg arról, hogy befejezte [2. feladat – a biztonságos LDAP tanúsítvány exportálása a. PFX-fájl](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md).</span><span class="sxs-lookup"><span data-stu-id="9aade-105">Ensure you've completed [Task 2 - export the secure LDAP certificate to a .PFX file](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md).</span></span>

<span data-ttu-id="9aade-106">Válassza ki, hogy az Azure portál felhasználói élmény előzetes vagy a klasszikus Azure portálon segítségével befejezheti a feladatot.</span><span class="sxs-lookup"><span data-stu-id="9aade-106">Choose whether to use the preview Azure portal experience or the Azure classic portal to complete this task.</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="9aade-107">**Azure-portálon (előzetes verzió)**: [engedélyezése biztonságos LDAP az Azure portál használatával](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span><span class="sxs-lookup"><span data-stu-id="9aade-107">**Azure portal (Preview)**: [Enable secure LDAP using the Azure portal](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span></span>
> * <span data-ttu-id="9aade-108">**A klasszikus Azure portálon**: [engedélyezése biztonságos LDAP a klasszikus Azure portál használatával](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps-classic.md)</span><span class="sxs-lookup"><span data-stu-id="9aade-108">**Azure classic portal**: [Enable secure LDAP using the classic Azure portal](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps-classic.md)</span></span>
>
>


## <a name="task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview"></a><span data-ttu-id="9aade-109">3. feladat – az Azure portál (előzetes verzió) használatával a felügyelt tartomány számára biztonságos LDAP engedélyezése</span><span class="sxs-lookup"><span data-stu-id="9aade-109">Task 3 - enable secure LDAP for the managed domain using the Azure portal (Preview)</span></span>
<span data-ttu-id="9aade-110">Ahhoz, hogy a biztonságos LDAP, hajtsa végre az alábbi konfigurációs lépéseket:</span><span class="sxs-lookup"><span data-stu-id="9aade-110">To enable secure LDAP, perform the following configuration steps:</span></span>

1. <span data-ttu-id="9aade-111">Keresse meg a  **[Azure-portálon](https://portal.azure.com)**.</span><span class="sxs-lookup"><span data-stu-id="9aade-111">Navigate to the **[Azure portal](https://portal.azure.com)**.</span></span>

2. <span data-ttu-id="9aade-112">Keressen a "tartományi szolgáltatások" kifejezésre a **keresési erőforrások** keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="9aade-112">Search for 'domain services' in the **Search resources** search box.</span></span> <span data-ttu-id="9aade-113">Válassza ki **Azure AD tartományi szolgáltatások** a keresési eredmény alapján.</span><span class="sxs-lookup"><span data-stu-id="9aade-113">Select **Azure AD Domain Services** from the search result.</span></span> <span data-ttu-id="9aade-114">A **Azure AD tartományi szolgáltatások** panel a felügyelt tartományok sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="9aade-114">The **Azure AD Domain Services** blade lists your managed domain.</span></span>

    ![Található felügyelt tartomány telepítése folyamatban](./media/getting-started/domain-services-provisioning-state-find-resource.png)

2. <span data-ttu-id="9aade-116">Kattintson a nevére, a felügyelt tartomány (például "contoso100.com") a tartománnyal kapcsolatos további részletek megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="9aade-116">Click the name of the managed domain (for example, 'contoso100.com') to see more details about the domain.</span></span>

    ![Tartományi szolgáltatások - üzembe helyezési állapota](./media/getting-started/domain-services-provisioning-state.png)

3. <span data-ttu-id="9aade-118">Kattintson a **biztonságos LDAP** a navigációs ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="9aade-118">Click **Secure LDAP** on the navigation pane.</span></span>

    ![Tartományi szolgáltatások - biztonságos LDAP panel](./media/active-directory-domain-services-admin-guide/secure-ldap-blade.png)

4. <span data-ttu-id="9aade-120">Alapértelmezés szerint le van tiltva a felügyelt tartományra biztonságos LDAP-hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="9aade-120">By default, secure LDAP access to your managed domain is disabled.</span></span> <span data-ttu-id="9aade-121">Váltás **biztonságos LDAP** való **engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="9aade-121">Toggle **Secure LDAP** to **Enable**.</span></span>

    ![Biztonságos LDAP engedélyezése](./media/active-directory-domain-services-admin-guide/secure-ldap-blade-configure.png)
5. <span data-ttu-id="9aade-123">Alapértelmezés szerint le van tiltva az interneten keresztül a felügyelt tartományra biztonságos LDAP-hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="9aade-123">By default, secure LDAP access to your managed domain over the internet is disabled.</span></span> <span data-ttu-id="9aade-124">Váltás **biztonságos LDAP-hozzáférés engedélyezése az interneten keresztül** való **engedélyezése**, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="9aade-124">Toggle **Allow secure LDAP access over the internet** to **Enable**, if desired.</span></span> 

6. <span data-ttu-id="9aade-125">Kattintson a mappa ikon következő **. Biztonságos LDAP tanúsítvány PFX-fájl**.</span><span class="sxs-lookup"><span data-stu-id="9aade-125">Click the folder icon following **.PFX file with secure LDAP certificate**.</span></span> <span data-ttu-id="9aade-126">Adja meg a PFX-fájl elérési útját a felügyelt tartományra biztonságos LDAP hozzáférést tanúsítványával.</span><span class="sxs-lookup"><span data-stu-id="9aade-126">Specify the path to the PFX file with the certificate for secure LDAP access to the managed domain.</span></span>

7. <span data-ttu-id="9aade-127">Adja meg a **visszafejtéséhez szükséges jelszót. PFX-fájl**.</span><span class="sxs-lookup"><span data-stu-id="9aade-127">Specify the **Password to decrypt .PFX file**.</span></span> <span data-ttu-id="9aade-128">Adja meg a tanúsítványt a PFX-fájlba exportálásakor használja ugyanazt a jelszót.</span><span class="sxs-lookup"><span data-stu-id="9aade-128">Provide the same password you used when exporting the certificate to the PFX file.</span></span>

8. <span data-ttu-id="9aade-129">Amikor elkészült, kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="9aade-129">When you are done, click the **Save** button.</span></span>

9. <span data-ttu-id="9aade-130">Megjelenik egy értesítés, amely arról értesíti, biztonságos LDAP tartozik a felügyelt tartomány számára.</span><span class="sxs-lookup"><span data-stu-id="9aade-130">You see a notification that informs you secure LDAP is being configured for the managed domain.</span></span> <span data-ttu-id="9aade-131">Amíg ez a művelet be nem fejeződik, a tartomány számára más beállításokat nem módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="9aade-131">Until this operation is complete, you cannot modify other settings for the domain.</span></span>

    ![A felügyelt tartomány számára biztonságos LDAP beállítása](./media/active-directory-domain-services-admin-guide/secure-ldap-blade-configuring.png)

> [!NOTE]
> <span data-ttu-id="9aade-133">Ahhoz, hogy a felügyelt tartományok biztonságos LDAP körülbelül 10 – 15 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="9aade-133">It takes about 10 to 15 minutes to enable secure LDAP for your managed domain.</span></span> <span data-ttu-id="9aade-134">A megadott biztonságos LDAP-tanúsítvány nem felel meg a szükséges feltételeknek, ha biztonságos LDAP nincs engedélyezve a címtárban, és láthatja a hibát.</span><span class="sxs-lookup"><span data-stu-id="9aade-134">If the provided secure LDAP certificate does not match the required criteria, secure LDAP is not enabled for your directory and you see a failure.</span></span> <span data-ttu-id="9aade-135">Ha például a tartomány neve nem megfelelő, a tanúsítvány már lejárt vagy hamarosan lejár.</span><span class="sxs-lookup"><span data-stu-id="9aade-135">For example, the domain name is incorrect, the certificate has already expired or expires soon.</span></span> <span data-ttu-id="9aade-136">Ebben az esetben próbálkozzon újra egy érvényes tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="9aade-136">In this case, retry with a valid certificate.</span></span>
>
>

<br>

## <a name="task-4---configure-dns-to-access-the-managed-domain-from-the-internet"></a><span data-ttu-id="9aade-137">4. feladat – az internetről a felügyelt tartomány eléréséhez DNS konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9aade-137">Task 4 - configure DNS to access the managed domain from the internet</span></span>
> [!NOTE]
> <span data-ttu-id="9aade-138">**Nem kötelező** – Ha nem tervezi, hogy a felügyelt tartományra LDAPS az interneten keresztül elérni, hagyja ki a konfigurációs feladat.</span><span class="sxs-lookup"><span data-stu-id="9aade-138">**Optional task** - If you do not plan to access the managed domain using LDAPS over the internet, skip this configuration task.</span></span>
>
>

<span data-ttu-id="9aade-139">Ez a feladat megkezdése előtt győződjön meg arról, befejezte a leírt lépéseket [3. feladat](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview).</span><span class="sxs-lookup"><span data-stu-id="9aade-139">Before you begin this task, ensure you have completed the steps outlined in [Task 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview).</span></span>

<span data-ttu-id="9aade-140">Miután engedélyezte biztonságos LDAP hozzáférést az interneten keresztül a felügyelt tartományok, módosítania DNS, hogy a felügyelt tartomány található ügyfélszámítógépek számára.</span><span class="sxs-lookup"><span data-stu-id="9aade-140">Once you have enabled secure LDAP access over the internet for your managed domain, you need to update DNS so that client computers can find this managed domain.</span></span> <span data-ttu-id="9aade-141">3. feladat végén külső IP-cím jelenik meg a **tulajdonságok** panel az **külső IP-cím a LDAPS ELÉRÉST**.</span><span class="sxs-lookup"><span data-stu-id="9aade-141">At the end of task 3, an external IP address is displayed on the **Properties** blade in **EXTERNAL IP ADDRESS FOR LDAPS ACCESS**.</span></span>

<span data-ttu-id="9aade-142">Konfigurálja a külső DNS-szolgáltatónál, hogy a külső IP-címre mutat a felügyelt tartomány (például ldaps.contoso100.com) DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="9aade-142">Configure your external DNS provider so that the DNS name of the managed domain (for example, 'ldaps.contoso100.com') points to this external IP address.</span></span> <span data-ttu-id="9aade-143">A jelen példában kell a következő DNS-bejegyzés létrehozása:</span><span class="sxs-lookup"><span data-stu-id="9aade-143">In our example, we need to create the following DNS entry:</span></span>

    ldaps.contoso100.com  -> 52.165.38.113

<span data-ttu-id="9aade-144">Ennyi az egész – most már készen áll a felügyelt tartományra biztonságos LDAP az interneten keresztül csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="9aade-144">That's it - you are now ready to connect to the managed domain using secure LDAP over the internet.</span></span>

> [!WARNING]
> <span data-ttu-id="9aade-145">Ne feledje, hogy az ügyfél számára megbízhatónak kell lennie a LDAPS tanúsítvány kiállítója tud sikeresen csatlakozni a felügyelt tartományra LDAPS kell.</span><span class="sxs-lookup"><span data-stu-id="9aade-145">Remember that client computers must trust the issuer of the LDAPS certificate to be able to connect successfully to the managed domain using LDAPS.</span></span> <span data-ttu-id="9aade-146">Ha egy nyilvánosan megbízható hitelesítésszolgáltató használ, nem kell semmit, mivel az ügyfélszámítógépek számára megbízhatónak ezek tanúsítványkiállítók.</span><span class="sxs-lookup"><span data-stu-id="9aade-146">If you are using a publicly trusted certification authority, you do not need to do anything since client computers trust these certificate issuers.</span></span> <span data-ttu-id="9aade-147">Ha önaláírt tanúsítványt használ, telepítse az önaláírt tanúsítvány nyilvános részét az ügyfélszámítógépen, a megbízható tanúsítványok tárolójába.</span><span class="sxs-lookup"><span data-stu-id="9aade-147">If you are using a self-signed certificate, install the public part of the self-signed certificate into the trusted certificate store on the client computer.</span></span>
>
>


## <a name="task-5---lock-down-ldaps-access-to-your-managed-domain-over-the-internet"></a><span data-ttu-id="9aade-148">5. feladat – az interneten keresztül a felügyelt tartományok LDAPS elérésére zár ki</span><span class="sxs-lookup"><span data-stu-id="9aade-148">Task 5 - lock-down LDAPS access to your managed domain over the internet</span></span>
> [!NOTE]
> <span data-ttu-id="9aade-149">**Nem kötelező** - LDAPS hozzáféréssel a felügyelt tartományra nincs engedélyezve az interneten keresztül, ha a konfigurációs feladat kihagyása.</span><span class="sxs-lookup"><span data-stu-id="9aade-149">**Optional task** - If you have not enabled LDAPS access to the managed domain over the internet, skip this configuration task.</span></span>
>
>

<span data-ttu-id="9aade-150">Ez a feladat megkezdése előtt győződjön meg arról, befejezte a leírt lépéseket [3. feladat](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview).</span><span class="sxs-lookup"><span data-stu-id="9aade-150">Before you begin this task, ensure you have completed the steps outlined in [Task 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview).</span></span>

<span data-ttu-id="9aade-151">A felügyelt tartományok LDAPS hozzáférési közzéteszi az interneten keresztül jelenti a biztonsági kockázatot jelentenek.</span><span class="sxs-lookup"><span data-stu-id="9aade-151">Exposing your managed domain for LDAPS access over the internet represents a security threat.</span></span> <span data-ttu-id="9aade-152">A felügyelt tartományra érhető el az interneten, a biztonságos LDAP használt port (Ez azt jelenti, hogy a 636-os port).</span><span class="sxs-lookup"><span data-stu-id="9aade-152">The managed domain is reachable from the internet at the port used for secure LDAP (that is, port 636).</span></span> <span data-ttu-id="9aade-153">Ezért ha szeretné, korlátozza a hozzáférést a felügyelt tartományra ismert IP-címek.</span><span class="sxs-lookup"><span data-stu-id="9aade-153">Therefore, you can choose to restrict access to the managed domain to specific known IP addresses.</span></span> <span data-ttu-id="9aade-154">A nagyobb biztonság érdekében hozzon létre egy hálózati biztonsági csoportot (NSG), és rendelje hozzá azt az alhálózatot, ha engedélyezte az Azure AD tartományi szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="9aade-154">For improved security, create a network security group (NSG) and associate it with the subnet where you have enabled Azure AD Domain Services.</span></span>

<span data-ttu-id="9aade-155">Az alábbi táblázat mutatja be egy minta NSG-t is konfigurálhat, biztonságos LDAP hozzáférést zárolását az interneten keresztül.</span><span class="sxs-lookup"><span data-stu-id="9aade-155">The following table illustrates a sample NSG you can configure, to lock down secure LDAP access over the internet.</span></span> <span data-ttu-id="9aade-156">Az NSG tartalmaz egy TCP-porton keresztül 636 csak egy megadott készletből IP-címek bejövő LDAPS hozzáférést engedélyező szabályokat.</span><span class="sxs-lookup"><span data-stu-id="9aade-156">The NSG contains a set of rules that allow inbound LDAPS access over TCP port 636 only from a specified set of IP addresses.</span></span> <span data-ttu-id="9aade-157">Minden bejövő forgalom az internetről az alapértelmezett "DenyAll" szabály vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="9aade-157">The default 'DenyAll' rule applies to all other inbound traffic from the internet.</span></span> <span data-ttu-id="9aade-158">Az NSG-szabály LDAPS hozzáférést a megadott IP-címeket az interneten keresztül rendelkezik, magasabb prioritású, mint a DenyAll NSG-szabály.</span><span class="sxs-lookup"><span data-stu-id="9aade-158">The NSG rule to allow LDAPS access over the internet from specified IP addresses has a higher priority than the DenyAll NSG rule.</span></span>

![Minta NSG LDAPS hozzáférés biztonságossá az interneten keresztül](./media/active-directory-domain-services-admin-guide/secure-ldap-sample-nsg.png)

<span data-ttu-id="9aade-160">**További információ** - [hálózati biztonsági csoportok](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="9aade-160">**More information** - [Network security groups](../virtual-network/virtual-networks-nsg.md).</span></span>

<br>

## <a name="related-content"></a><span data-ttu-id="9aade-161">Kapcsolódó tartalom</span><span class="sxs-lookup"><span data-stu-id="9aade-161">Related content</span></span>
* [<span data-ttu-id="9aade-162">Azure AD tartományi szolgáltatások – első lépések útmutató</span><span class="sxs-lookup"><span data-stu-id="9aade-162">Azure AD Domain Services - Getting Started guide</span></span>](active-directory-ds-getting-started.md)
* [<span data-ttu-id="9aade-163">Azure AD tartományi szolgáltatások által kezelt tartomány felügyelete</span><span class="sxs-lookup"><span data-stu-id="9aade-163">Administer an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-administer-domain.md)
* [<span data-ttu-id="9aade-164">Csoportházirend a egy Azure AD tartományi szolgáltatások által kezelt tartomány felügyelete</span><span class="sxs-lookup"><span data-stu-id="9aade-164">Administer Group Policy on an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-administer-group-policy.md)
* [<span data-ttu-id="9aade-165">Hálózati biztonsági csoportok</span><span class="sxs-lookup"><span data-stu-id="9aade-165">Network security groups</span></span>](../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="9aade-166">Hálózati biztonsági csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="9aade-166">Create a Network Security Group</span></span>](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)
