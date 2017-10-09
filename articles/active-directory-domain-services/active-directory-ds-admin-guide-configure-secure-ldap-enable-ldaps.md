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
ms.date: 08/14/2017
ms.author: maheshu
ms.openlocfilehash: 8781285cd02d690788056b985b017efd7e4d6b3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a><span data-ttu-id="0f9be-103">Biztonságos LDAP (LDAPS) használatos az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz tartozó konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0f9be-103">Configure secure LDAP (LDAPS) for an Azure AD Domain Services managed domain</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="0f9be-104">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="0f9be-104">Before you begin</span></span>
<span data-ttu-id="0f9be-105">Győződjön meg arról, hogy befejezte [2. feladat – exportálás hello biztonságos LDAP tanúsítvány tooa. PFX-fájl](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md).</span><span class="sxs-lookup"><span data-stu-id="0f9be-105">Ensure you've completed [Task 2 - export hello secure LDAP certificate tooa .PFX file](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md).</span></span>

<span data-ttu-id="0f9be-106">Válassza ki e toouse hello Azure portál felhasználói élmény preview vagy hello Azure klasszikus portál toocomplete ezt a feladatot.</span><span class="sxs-lookup"><span data-stu-id="0f9be-106">Choose whether toouse hello preview Azure portal experience or hello Azure classic portal toocomplete this task.</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="0f9be-107">**Azure-portálon (előzetes verzió)**: [engedélyezése biztonságos LDAP hello Azure-portál használatával](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span><span class="sxs-lookup"><span data-stu-id="0f9be-107">**Azure portal (Preview)**: [Enable secure LDAP using hello Azure portal](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span></span>
> * <span data-ttu-id="0f9be-108">**A klasszikus Azure portálon**: [engedélyezése biztonságos LDAP hello a klasszikus Azure portál használatával](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps-classic.md)</span><span class="sxs-lookup"><span data-stu-id="0f9be-108">**Azure classic portal**: [Enable secure LDAP using hello classic Azure portal](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps-classic.md)</span></span>
>
>


## <a name="task-3---enable-secure-ldap-for-hello-managed-domain-using-hello-azure-portal-preview"></a><span data-ttu-id="0f9be-109">3. feladat – hello felügyelt tartomány hello Azure portal (előzetes verzió) segítségével biztonságos LDAP engedélyezése</span><span class="sxs-lookup"><span data-stu-id="0f9be-109">Task 3 - enable secure LDAP for hello managed domain using hello Azure portal (Preview)</span></span>
<span data-ttu-id="0f9be-110">tooenable biztonságos LDAP, hajtsa végre a következő konfigurációs lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="0f9be-110">tooenable secure LDAP, perform hello following configuration steps:</span></span>

1. <span data-ttu-id="0f9be-111">Keresse meg a toohello  **[Azure-portálon](https://portal.azure.com)**.</span><span class="sxs-lookup"><span data-stu-id="0f9be-111">Navigate toohello **[Azure portal](https://portal.azure.com)**.</span></span>

2. <span data-ttu-id="0f9be-112">Keresési hello tartományi szolgáltatások **keresési erőforrások** keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="0f9be-112">Search for 'domain services' in hello **Search resources** search box.</span></span> <span data-ttu-id="0f9be-113">Válassza ki **Azure AD tartományi szolgáltatások** hello keresési eredmény alapján.</span><span class="sxs-lookup"><span data-stu-id="0f9be-113">Select **Azure AD Domain Services** from hello search result.</span></span> <span data-ttu-id="0f9be-114">Hello **Azure AD tartományi szolgáltatások** panel a felügyelt tartományok sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="0f9be-114">hello **Azure AD Domain Services** blade lists your managed domain.</span></span>

    ![Található felügyelt tartomány telepítése folyamatban](./media/getting-started/domain-services-provisioning-state-find-resource.png)

2. <span data-ttu-id="0f9be-116">Kattintson hello hello felügyelt tartomány (például "contoso100.com") toosee hello tartománnyal kapcsolatos további részletekért.</span><span class="sxs-lookup"><span data-stu-id="0f9be-116">Click hello name of hello managed domain (for example, 'contoso100.com') toosee more details about hello domain.</span></span>

    ![Tartományi szolgáltatások - üzembe helyezési állapota](./media/getting-started/domain-services-provisioning-state.png)

3. <span data-ttu-id="0f9be-118">Kattintson a **biztonságos LDAP** hello navigációs ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="0f9be-118">Click **Secure LDAP** on hello navigation pane.</span></span>

    ![Tartományi szolgáltatások - biztonságos LDAP panel](./media/active-directory-domain-services-admin-guide/secure-ldap-blade.png)

4. <span data-ttu-id="0f9be-120">Biztonságos LDAP hozzáférés tooyour által kezelt tartomány alapértelmezés szerint le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="0f9be-120">By default, secure LDAP access tooyour managed domain is disabled.</span></span> <span data-ttu-id="0f9be-121">Váltás **biztonságos LDAP** túl**engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="0f9be-121">Toggle **Secure LDAP** too**Enable**.</span></span>

    ![Biztonságos LDAP engedélyezése](./media/active-directory-domain-services-admin-guide/secure-ldap-blade-configure.png)
5. <span data-ttu-id="0f9be-123">Alapértelmezés szerint biztonságos LDAP hozzáférés tooyour felügyelt tartomány keresztül hello internet le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="0f9be-123">By default, secure LDAP access tooyour managed domain over hello internet is disabled.</span></span> <span data-ttu-id="0f9be-124">Váltás **biztonságos LDAP hozzáférés engedélyezése keresztül hello internet** túl**engedélyezése**, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="0f9be-124">Toggle **Allow secure LDAP access over hello internet** too**Enable**, if desired.</span></span> 

6. <span data-ttu-id="0f9be-125">Kattintson a hello mappa ikon következő **. Biztonságos LDAP tanúsítvány PFX-fájl**.</span><span class="sxs-lookup"><span data-stu-id="0f9be-125">Click hello folder icon following **.PFX file with secure LDAP certificate**.</span></span> <span data-ttu-id="0f9be-126">Adja meg a hello elérési toohello PFX-fájl hello tanúsítvánnyal biztonságos LDAP hozzáférés toohello kezelt tartományban.</span><span class="sxs-lookup"><span data-stu-id="0f9be-126">Specify hello path toohello PFX file with hello certificate for secure LDAP access toohello managed domain.</span></span>

7. <span data-ttu-id="0f9be-127">Adja meg a hello **jelszó toodecrypt. PFX-fájl**.</span><span class="sxs-lookup"><span data-stu-id="0f9be-127">Specify hello **Password toodecrypt .PFX file**.</span></span> <span data-ttu-id="0f9be-128">Adja meg a hello hello tanúsítvány toohello PFX-fájljának exportálásakor használja ugyanazt a jelszót.</span><span class="sxs-lookup"><span data-stu-id="0f9be-128">Provide hello same password you used when exporting hello certificate toohello PFX file.</span></span>

8. <span data-ttu-id="0f9be-129">Amikor elkészült, kattintson a hello **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="0f9be-129">When you are done, click hello **Save** button.</span></span>

9. <span data-ttu-id="0f9be-130">Megjelenik egy értesítés, amely arról tájékoztatja, biztonságos LDAP hello felügyelt tartomány van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="0f9be-130">You see a notification that informs you secure LDAP is being configured for hello managed domain.</span></span> <span data-ttu-id="0f9be-131">Amíg ez a művelet be nem fejeződik, hello tartomány számára más beállításokat nem módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="0f9be-131">Until this operation is complete, you cannot modify other settings for hello domain.</span></span>

    ![Biztonságos LDAP hello felügyelt tartomány beállítása](./media/active-directory-domain-services-admin-guide/secure-ldap-blade-configuring.png)

> [!NOTE]
> <span data-ttu-id="0f9be-133">Tooenable biztonságos LDAP a felügyelt tartományok körülbelül 10 too15 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="0f9be-133">It takes about 10 too15 minutes tooenable secure LDAP for your managed domain.</span></span> <span data-ttu-id="0f9be-134">Ha hello biztonságos LDAP-tanúsítvány nem megfelelő hello szükséges feltételeket, biztonságos LDAP nincs engedélyezve a címtárban, és hibát látja.</span><span class="sxs-lookup"><span data-stu-id="0f9be-134">If hello provided secure LDAP certificate does not match hello required criteria, secure LDAP is not enabled for your directory and you see a failure.</span></span> <span data-ttu-id="0f9be-135">Ha például hello tartomány neve nem megfelelő, hello tanúsítvány már lejárt vagy hamarosan lejár.</span><span class="sxs-lookup"><span data-stu-id="0f9be-135">For example, hello domain name is incorrect, hello certificate has already expired or expires soon.</span></span> <span data-ttu-id="0f9be-136">Ebben az esetben próbálkozzon újra egy érvényes tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="0f9be-136">In this case, retry with a valid certificate.</span></span>
>
>

<br>

## <a name="task-4---configure-dns-tooaccess-hello-managed-domain-from-hello-internet"></a><span data-ttu-id="0f9be-137">4. feladat – DNS-tooaccess hello felügyelt tartomány a hello konfigurálása internet</span><span class="sxs-lookup"><span data-stu-id="0f9be-137">Task 4 - configure DNS tooaccess hello managed domain from hello internet</span></span>
> [!NOTE]
> <span data-ttu-id="0f9be-138">**Nem kötelező** – Ha nem tervezi tooaccess hello felügyelt tartományra LDAPS több mint hello internet, hagyja ki a konfigurációs feladat.</span><span class="sxs-lookup"><span data-stu-id="0f9be-138">**Optional task** - If you do not plan tooaccess hello managed domain using LDAPS over hello internet, skip this configuration task.</span></span>
>
>

<span data-ttu-id="0f9be-139">Ez a feladat megkezdése előtt győződjön meg arról, végrehajtotta hello leírt [3. feladat](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview).</span><span class="sxs-lookup"><span data-stu-id="0f9be-139">Before you begin this task, ensure you have completed hello steps outlined in [Task 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview).</span></span>

<span data-ttu-id="0f9be-140">Miután engedélyezte a biztonságos LDAP hozzáférést keresztül hello internet a felügyelt tartományok kell tooupdate DNS, hogy a felügyelt tartomány található ügyfélszámítógépek számára.</span><span class="sxs-lookup"><span data-stu-id="0f9be-140">Once you have enabled secure LDAP access over hello internet for your managed domain, you need tooupdate DNS so that client computers can find this managed domain.</span></span> <span data-ttu-id="0f9be-141">3. feladat hello végén, külső IP-cím jelenik meg hello **tulajdonságok** panel az **külső IP-cím a LDAPS ELÉRÉST**.</span><span class="sxs-lookup"><span data-stu-id="0f9be-141">At hello end of task 3, an external IP address is displayed on hello **Properties** blade in **EXTERNAL IP ADDRESS FOR LDAPS ACCESS**.</span></span>

<span data-ttu-id="0f9be-142">Konfigurálja a külső DNS-szolgáltatónál, úgy, hogy hello hello DNS-nevét (például ldaps.contoso100.com) tartomány pontok toothis külső IP-címet felügyeli.</span><span class="sxs-lookup"><span data-stu-id="0f9be-142">Configure your external DNS provider so that hello DNS name of hello managed domain (for example, 'ldaps.contoso100.com') points toothis external IP address.</span></span> <span data-ttu-id="0f9be-143">Ebben a példában igazolnia kell a DNS-bejegyzés a következő toocreate hello:</span><span class="sxs-lookup"><span data-stu-id="0f9be-143">In our example, we need toocreate hello following DNS entry:</span></span>

    ldaps.contoso100.com  -> 52.165.38.113

<span data-ttu-id="0f9be-144">Ennyi az egész – most már áll készen tooconnect toohello felügyelt tartomány biztonságos LDAP protokollt használó hello internet.</span><span class="sxs-lookup"><span data-stu-id="0f9be-144">That's it - you are now ready tooconnect toohello managed domain using secure LDAP over hello internet.</span></span>

> [!WARNING]
> <span data-ttu-id="0f9be-145">Ne feledje, hogy ügyfélszámítógépek meg kell bíznia hello LDAPS tanúsítvány toobe képes tooconnect hello kiállítójának sikeresen toohello felügyelt tartományba LDAPS használatával.</span><span class="sxs-lookup"><span data-stu-id="0f9be-145">Remember that client computers must trust hello issuer of hello LDAPS certificate toobe able tooconnect successfully toohello managed domain using LDAPS.</span></span> <span data-ttu-id="0f9be-146">Ha egy nyilvánosan megbízható hitelesítésszolgáltató használ, nem kell toodo semmit óta az ügyfélszámítógépek számára megbízhatónak ezek tanúsítványkiállítók.</span><span class="sxs-lookup"><span data-stu-id="0f9be-146">If you are using a publicly trusted certification authority, you do not need toodo anything since client computers trust these certificate issuers.</span></span> <span data-ttu-id="0f9be-147">Ha önaláírt tanúsítványt használ, telepítse a hello önaláírt tanúsítvány nyilvános részét hello hello ügyfélszámítógépen hello megbízható tanúsítványok tárolójába.</span><span class="sxs-lookup"><span data-stu-id="0f9be-147">If you are using a self-signed certificate, install hello public part of hello self-signed certificate into hello trusted certificate store on hello client computer.</span></span>
>
>


## <a name="task-5---lock-down-ldaps-access-tooyour-managed-domain-over-hello-internet"></a><span data-ttu-id="0f9be-148">5. feladat – lock-le nyilas LDAPS hozzáférési tooyour felügyelt tartomány keresztül hello internet</span><span class="sxs-lookup"><span data-stu-id="0f9be-148">Task 5 - lock-down LDAPS access tooyour managed domain over hello internet</span></span>
> [!NOTE]
> <span data-ttu-id="0f9be-149">**Nem kötelező** – Ha nem engedélyezte a LDAPS hozzáférés toohello felügyelt tartomány több mint hello internet, hagyja ki a konfigurációs feladat.</span><span class="sxs-lookup"><span data-stu-id="0f9be-149">**Optional task** - If you have not enabled LDAPS access toohello managed domain over hello internet, skip this configuration task.</span></span>
>
>

<span data-ttu-id="0f9be-150">Ez a feladat megkezdése előtt győződjön meg arról, végrehajtotta hello leírt [3. feladat](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview).</span><span class="sxs-lookup"><span data-stu-id="0f9be-150">Before you begin this task, ensure you have completed hello steps outlined in [Task 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview).</span></span>

<span data-ttu-id="0f9be-151">A felügyelt tartományok LDAPS hozzáférési kitettségének keresztül hello internetes biztonsági kockázatot jelentenek jelöli.</span><span class="sxs-lookup"><span data-stu-id="0f9be-151">Exposing your managed domain for LDAPS access over hello internet represents a security threat.</span></span> <span data-ttu-id="0f9be-152">hello által kezelt tartomány érhető el a hello interneten, a használt biztonságos LDAP hello port (Ez azt jelenti, hogy a 636-os port).</span><span class="sxs-lookup"><span data-stu-id="0f9be-152">hello managed domain is reachable from hello internet at hello port used for secure LDAP (that is, port 636).</span></span> <span data-ttu-id="0f9be-153">Ezért dönthet úgy toorestrict hozzáférés toohello felügyelt tartomány toospecific ismert IP-címeket.</span><span class="sxs-lookup"><span data-stu-id="0f9be-153">Therefore, you can choose toorestrict access toohello managed domain toospecific known IP addresses.</span></span> <span data-ttu-id="0f9be-154">A nagyobb biztonság érdekében hozzon létre egy hálózati biztonsági csoportot (NSG), és társítsa azt az hello alhálózatot, ha engedélyezte az Azure AD tartományi szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="0f9be-154">For improved security, create a network security group (NSG) and associate it with hello subnet where you have enabled Azure AD Domain Services.</span></span>

<span data-ttu-id="0f9be-155">hello alábbi táblázat mutatja be egy minta NSG is konfigurálhat, biztonságos LDAP hozzáférés keresztül hello toolock internet.</span><span class="sxs-lookup"><span data-stu-id="0f9be-155">hello following table illustrates a sample NSG you can configure, toolock down secure LDAP access over hello internet.</span></span> <span data-ttu-id="0f9be-156">hello NSG tartalmaz egy TCP-porton keresztül 636 csak egy megadott készletből IP-címek bejövő LDAPS hozzáférést engedélyező szabályokat.</span><span class="sxs-lookup"><span data-stu-id="0f9be-156">hello NSG contains a set of rules that allow inbound LDAPS access over TCP port 636 only from a specified set of IP addresses.</span></span> <span data-ttu-id="0f9be-157">hello alapértelmezett "DenyAll" szabály tooall egyéb bejövő forgalmát hello az interneten.</span><span class="sxs-lookup"><span data-stu-id="0f9be-157">hello default 'DenyAll' rule applies tooall other inbound traffic from hello internet.</span></span> <span data-ttu-id="0f9be-158">hello NSG szabály tooallow LDAPS hozzáférés hello keresztül megadott IP-címekről érkező internetes rendelkezik magasabb prioritású, mint hello DenyAll NSG-szabály.</span><span class="sxs-lookup"><span data-stu-id="0f9be-158">hello NSG rule tooallow LDAPS access over hello internet from specified IP addresses has a higher priority than hello DenyAll NSG rule.</span></span>

![A minta NSG toosecure LDAPS hozzáférés keresztül hello internet](./media/active-directory-domain-services-admin-guide/secure-ldap-sample-nsg.png)

<span data-ttu-id="0f9be-160">**További információ** - [hálózati biztonsági csoportok](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="0f9be-160">**More information** - [Network security groups](../virtual-network/virtual-networks-nsg.md).</span></span>

<br>

## <a name="related-content"></a><span data-ttu-id="0f9be-161">Kapcsolódó tartalom</span><span class="sxs-lookup"><span data-stu-id="0f9be-161">Related content</span></span>
* [<span data-ttu-id="0f9be-162">Azure AD tartományi szolgáltatások – első lépések útmutató</span><span class="sxs-lookup"><span data-stu-id="0f9be-162">Azure AD Domain Services - Getting Started guide</span></span>](active-directory-ds-getting-started.md)
* [<span data-ttu-id="0f9be-163">Azure AD tartományi szolgáltatások által kezelt tartomány felügyelete</span><span class="sxs-lookup"><span data-stu-id="0f9be-163">Administer an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-administer-domain.md)
* [<span data-ttu-id="0f9be-164">Csoportházirend a egy Azure AD tartományi szolgáltatások által kezelt tartomány felügyelete</span><span class="sxs-lookup"><span data-stu-id="0f9be-164">Administer Group Policy on an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-administer-group-policy.md)
* [<span data-ttu-id="0f9be-165">Hálózati biztonsági csoportok</span><span class="sxs-lookup"><span data-stu-id="0f9be-165">Network security groups</span></span>](../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="0f9be-166">Hálózati biztonsági csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="0f9be-166">Create a Network Security Group</span></span>](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)
