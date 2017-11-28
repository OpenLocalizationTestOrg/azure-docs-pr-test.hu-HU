---
title: "Biztonságos LDAP (LDAPS) az Azure AD tartományi szolgáltatásokban aaaConfigure |} Microsoft Docs"
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
ms.openlocfilehash: a0d6e2faf474b1f0cbe157fb4ae2754b1d521ef9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a><span data-ttu-id="fd7c4-103">Biztonságos LDAP (LDAPS) használatos az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz tartozó konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fd7c4-103">Configure secure LDAP (LDAPS) for an Azure AD Domain Services managed domain</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="fd7c4-104">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="fd7c4-104">Before you begin</span></span>
<span data-ttu-id="fd7c4-105">Győződjön meg arról, hogy befejezte [2. feladat – exportálás hello biztonságos LDAP tanúsítvány tooa. PFX-fájl](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md).</span><span class="sxs-lookup"><span data-stu-id="fd7c4-105">Ensure you've completed [Task 2 - export hello secure LDAP certificate tooa .PFX file](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md).</span></span>

<span data-ttu-id="fd7c4-106">Válassza ki e toouse hello Azure portál felhasználói élmény preview vagy hello Azure klasszikus portál toocomplete ezt a feladatot.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-106">Choose whether toouse hello preview Azure portal experience or hello Azure classic portal toocomplete this task.</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="fd7c4-107">**Azure-portálon (előzetes verzió)**: [engedélyezése biztonságos LDAP hello Azure-portál használatával](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span><span class="sxs-lookup"><span data-stu-id="fd7c4-107">**Azure portal (Preview)**: [Enable secure LDAP using hello Azure portal](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span></span>
> * <span data-ttu-id="fd7c4-108">**A klasszikus Azure portálon**: [engedélyezése biztonságos LDAP hello a klasszikus Azure portál használatával](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps-classic.md)</span><span class="sxs-lookup"><span data-stu-id="fd7c4-108">**Azure classic portal**: [Enable secure LDAP using hello classic Azure portal](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps-classic.md)</span></span>
>
>


## <a name="task-3---enable-secure-ldap-for-hello-managed-domain-using-hello-classic-azure-portal"></a><span data-ttu-id="fd7c4-109">3. feladat – hello felügyelt tartomány hello a klasszikus Azure portál használatával biztonságos LDAP engedélyezése</span><span class="sxs-lookup"><span data-stu-id="fd7c4-109">Task 3 - enable secure LDAP for hello managed domain using hello classic Azure portal</span></span>
<span data-ttu-id="fd7c4-110">tooenable biztonságos LDAP, hajtsa végre a következő konfigurációs lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="fd7c4-110">tooenable secure LDAP, perform hello following configuration steps:</span></span>

1. <span data-ttu-id="fd7c4-111">Keresse meg a toohello  **[a klasszikus Azure portálon](https://manage.windowsazure.com)**.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-111">Navigate toohello **[Azure classic portal](https://manage.windowsazure.com)**.</span></span>
2. <span data-ttu-id="fd7c4-112">Jelölje be hello **Active Directory** csomópontot a bal oldali panelen hello.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-112">Select hello **Active Directory** node on hello left pane.</span></span>
3. <span data-ttu-id="fd7c4-113">Válassza ki a hello Azure Active directory (is hivatkozott tooas "tenant"), amelyhez engedélyezte az Azure AD tartományi szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-113">Select hello Azure AD directory (also referred tooas 'tenant'), for which you have enabled Azure AD Domain Services.</span></span>

    ![Azure AD címtár kiválasztása](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. <span data-ttu-id="fd7c4-115">Kattintson a hello **konfigurálása** fülre.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-115">Click hello **Configure** tab.</span></span>

    ![Címtár lapjának konfigurálása](./media/active-directory-domain-services-getting-started/configure-tab.png)
5. <span data-ttu-id="fd7c4-117">Görgessen lefelé toohello című **tartományi szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-117">Scroll down toohello section titled **domain services**.</span></span> <span data-ttu-id="fd7c4-118">Megjelenik egy lehetőség, című **biztonságos LDAP (LDAPS)** látható módon a következő képernyőkép hello:</span><span class="sxs-lookup"><span data-stu-id="fd7c4-118">You should see an option titled **Secure LDAP (LDAPS)** as shown in hello following screenshot:</span></span>

    ![Tartományi szolgáltatások konfigurációja szakasz](./media/active-directory-domain-services-admin-guide/secure-ldap-start.png)
6. <span data-ttu-id="fd7c4-120">Kattintson a hello **tanúsítvány konfigurálása...**  hello fel gomb toobring **tanúsítvány konfigurálása a biztonságos LDAP** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-120">Click hello **Configure certificate ...** button toobring up hello **Configure Certificate for Secure LDAP** dialog.</span></span>

    ![Biztonságos LDAP-tanúsítvány konfigurálása](./media/active-directory-domain-services-admin-guide/secure-ldap-configure-cert-page.png)
7. <span data-ttu-id="fd7c4-122">Kattintson a hello mappa ikon következő **PFX-fájl a tanúsítvány** toospecify hello PFX-fájl, a biztonságos LDAP hozzáférés toohello toouse kívánja hello tanúsítványt tartalmazó felügyelt tartomány.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-122">Click hello folder icon following **PFX FILE WITH CERTIFICATE** toospecify hello PFX file, which contains hello certificate you wish toouse for secure LDAP access toohello managed domain.</span></span> <span data-ttu-id="fd7c4-123">Hello tanúsítvány toohello PFX-fájl exportálása során megadott hello jelszó is beírhatja.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-123">Also enter hello password you specified when exporting hello certificate toohello PFX file.</span></span> <span data-ttu-id="fd7c4-124">Kattintson a Kész gombra a hello alsó hello.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-124">Then, click hello done button on hello bottom.</span></span>

    ![Biztonságos LDAP PFX-fájlt és a jelszó megadása](./media/active-directory-domain-services-admin-guide/secure-ldap-specify-pfx.png)
8. <span data-ttu-id="fd7c4-126">Hello **tartományi szolgáltatások** hello szakasza **konfigurálása** lapon kell beolvasni szürkén jelenik meg, és a hello **folyamatban...**  állapot néhány percig.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-126">hello **domain services** section of hello **Configure** tab should get grayed out and is in hello **Pending...** state for a few minutes.</span></span> <span data-ttu-id="fd7c4-127">Ebben az időszakban hello LDAPS tanúsítvány pontosságát ellenőrizve, és biztonságos LDAP konfigurálva van a felügyelt tartományok.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-127">During this period, hello LDAPS certificate is verified for accuracy and secure LDAP is configured for your managed domain.</span></span>

    ![Biztonságos LDAP - függő állapot](./media/active-directory-domain-services-admin-guide/secure-ldap-pending-state.png)

   > [!NOTE]
   > <span data-ttu-id="fd7c4-129">Tooenable biztonságos LDAP a felügyelt tartományok körülbelül 10 too15 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-129">It takes about 10 too15 minutes tooenable secure LDAP for your managed domain.</span></span> <span data-ttu-id="fd7c4-130">Ha hello biztonságos LDAP-tanúsítvány nem megfelelő hello szükséges feltételeket, biztonságos LDAP nincs engedélyezve a címtárban, és hibát látja.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-130">If hello provided secure LDAP certificate does not match hello required criteria, secure LDAP is not enabled for your directory and you see a failure.</span></span> <span data-ttu-id="fd7c4-131">Ha például hello tartomány neve nem megfelelő, hello tanúsítvány már lejárt vagy hamarosan lejár.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-131">For example, hello domain name is incorrect, hello certificate has already expired or expires soon.</span></span>
   >
   >

9. <span data-ttu-id="fd7c4-132">Ha biztonságos LDAP sikeresen engedélyezve van a felügyelt tartományok, hello **folyamatban...**  üzenet kell eltűnik.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-132">When secure LDAP is successfully enabled for your managed domain, hello **Pending...** message should disappear.</span></span> <span data-ttu-id="fd7c4-133">Meg kell jelennie hello tanúsítvány ujjlenyomata látható hello jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-133">You should see hello thumbprint of hello certificate displayed.</span></span>

    ![Biztonságos LDAP engedélyezve](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled.png)

<br>

## <a name="task-4---enable-secure-ldap-access-over-hello-internet"></a><span data-ttu-id="fd7c4-135">4. feladat – hello biztonságos LDAP-hozzáférés engedélyezése az internet</span><span class="sxs-lookup"><span data-stu-id="fd7c4-135">Task 4 - enable secure LDAP access over hello internet</span></span>
<span data-ttu-id="fd7c4-136">**Nem kötelező** – Ha nem tervezi tooaccess hello felügyelt tartományra LDAPS több mint hello internet, hagyja ki a konfigurációs feladat.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-136">**Optional task** - If you do not plan tooaccess hello managed domain using LDAPS over hello internet, skip this configuration task.</span></span>

<span data-ttu-id="fd7c4-137">Ez a feladat megkezdése előtt győződjön meg arról, végrehajtotta hello leírt [3. feladat](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-classic-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="fd7c4-137">Before you begin this task, ensure you have completed hello steps outlined in [Task 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-classic-azure-portal).</span></span>

1. <span data-ttu-id="fd7c4-138">Megjelenik egy lehetőség túl**ENGEDÉLYEZNI biztonságos LDAP hozzáférés keresztül hello INTERNET** a hello **tartományi szolgáltatások** hello szakasza **konfigurálása** lap.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-138">You should see an option too**ENABLE SECURE LDAP ACCESS OVER hello INTERNET** in hello **domain services** section of hello **Configure** page.</span></span> <span data-ttu-id="fd7c4-139">Ez a beállítás túl**nem** alapértelmezés szerint óta internet access toohello felügyelt tartomány keresztül biztonságos LDAP alapértelmezésben le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-139">This option is set too**NO** by default since internet access toohello managed domain over secure LDAP is disabled by default.</span></span>

    ![Biztonságos LDAP engedélyezve](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled2.png)
2. <span data-ttu-id="fd7c4-141">Váltás **ENGEDÉLYEZNI biztonságos LDAP hozzáférés keresztül hello INTERNET** túl**Igen**.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-141">Toggle **ENABLE SECURE LDAP ACCESS OVER hello INTERNET** too**YES**.</span></span> <span data-ttu-id="fd7c4-142">Kattintson a hello **mentése** hello alsó panelje gombjára.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-142">Click hello **SAVE** button on hello bottom panel.</span></span>
    <span data-ttu-id="fd7c4-143">![Biztonságos LDAP engedélyezve](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access.png)</span><span class="sxs-lookup"><span data-stu-id="fd7c4-143">![Secure LDAP enabled](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access.png)</span></span>
3. <span data-ttu-id="fd7c4-144">Hello **tartományi szolgáltatások** hello szakasza **konfigurálása** lapon kell beolvasni szürkén jelenik meg, és a hello **folyamatban...**  állapot néhány percig.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-144">hello **domain services** section of hello **Configure** tab should get grayed out and is in hello **Pending...** state for a few minutes.</span></span> <span data-ttu-id="fd7c4-145">Némi várakozás után internet access tooyour által kezelt tartomány keresztül biztonságos LDAP engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-145">After some time, internet access tooyour managed domain over secure LDAP is enabled.</span></span>

    ![Biztonságos LDAP - függő állapot](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access-pending-state.png)

   > [!NOTE]
   > <span data-ttu-id="fd7c4-147">Körülbelül 10 percet vesz igénybe tooenable internet-hozzáférés biztonságos LDAP keresztül a felügyelt tartományok.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-147">It takes about 10 minutes tooenable internet access over secure LDAP for your managed domain.</span></span>
   >
   >
4. <span data-ttu-id="fd7c4-148">Biztonságos LDAP hozzáférés tooyour felügyelt tartomány keresztül hello internet sikeresen engedélyezve van, hello **folyamatban...**  üzenet kell eltűnik.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-148">When secure LDAP access tooyour managed domain over hello internet is successfully enabled, hello **Pending...** message should disappear.</span></span> <span data-ttu-id="fd7c4-149">Hello külső IP-címet, amelyet látni lehet használt tooaccess a címtár keresztül hello mezőben LDAPS **külső IP-cím a LDAPS ELÉRÉST**.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-149">You should see hello external IP address that can be used tooaccess your directory over LDAPS in hello field **EXTERNAL IP ADDRESS FOR LDAPS ACCESS**.</span></span>

    ![Biztonságos LDAP engedélyezve](./media/active-directory-domain-services-admin-guide/secure-ldap-internet-access-enabled.png)

<br>

## <a name="task-5---configure-dns-tooaccess-hello-managed-domain-from-hello-internet"></a><span data-ttu-id="fd7c4-151">5. feladat – DNS-tooaccess hello felügyelt tartomány a hello konfigurálása internet</span><span class="sxs-lookup"><span data-stu-id="fd7c4-151">Task 5 - configure DNS tooaccess hello managed domain from hello internet</span></span>
<span data-ttu-id="fd7c4-152">**Nem kötelező** – Ha nem tervezi tooaccess hello felügyelt tartományra LDAPS több mint hello internet, hagyja ki a konfigurációs feladat.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-152">**Optional task** - If you do not plan tooaccess hello managed domain using LDAPS over hello internet, skip this configuration task.</span></span>

<span data-ttu-id="fd7c4-153">Ez a feladat megkezdése előtt győződjön meg arról, végrehajtotta hello leírt [4. feladat](#task-4---enable-secure-ldap-access-over-the-internet).</span><span class="sxs-lookup"><span data-stu-id="fd7c4-153">Before you begin this task, ensure you have completed hello steps outlined in [Task 4](#task-4---enable-secure-ldap-access-over-the-internet).</span></span>

<span data-ttu-id="fd7c4-154">Miután engedélyezte a biztonságos LDAP hozzáférést keresztül hello internet a felügyelt tartományok kell tooupdate DNS, hogy a felügyelt tartomány található ügyfélszámítógépek számára.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-154">Once you have enabled secure LDAP access over hello internet for your managed domain, you need tooupdate DNS so that client computers can find this managed domain.</span></span> <span data-ttu-id="fd7c4-155">4. feladat hello végén, külső IP-cím jelenik meg hello **konfigurálása** lapján **külső IP-cím a LDAPS ELÉRÉST**.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-155">At hello end of task 4, an external IP address is displayed on hello **Configure** tab in **EXTERNAL IP ADDRESS FOR LDAPS ACCESS**.</span></span>

<span data-ttu-id="fd7c4-156">Konfigurálja a külső DNS-szolgáltatónál, úgy, hogy hello hello DNS-nevét (például ldaps.contoso100.com) tartomány pontok toothis külső IP-címet felügyeli.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-156">Configure your external DNS provider so that hello DNS name of hello managed domain (for example, 'ldaps.contoso100.com') points toothis external IP address.</span></span> <span data-ttu-id="fd7c4-157">Ebben a példában igazolnia kell a DNS-bejegyzés a következő toocreate hello:</span><span class="sxs-lookup"><span data-stu-id="fd7c4-157">In our example, we need toocreate hello following DNS entry:</span></span>

    ldaps.contoso100.com  -> 52.165.38.113

<span data-ttu-id="fd7c4-158">Ennyi az egész – most már áll készen tooconnect toohello felügyelt tartomány biztonságos LDAP protokollt használó hello internet.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-158">That's it - you are now ready tooconnect toohello managed domain using secure LDAP over hello internet.</span></span>

> [!WARNING]
> <span data-ttu-id="fd7c4-159">Ne feledje, hogy ügyfélszámítógépek meg kell bíznia hello LDAPS tanúsítvány toobe képes tooconnect hello kiállítójának sikeresen toohello felügyelt tartományba LDAPS használatával.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-159">Remember that client computers must trust hello issuer of hello LDAPS certificate toobe able tooconnect successfully toohello managed domain using LDAPS.</span></span> <span data-ttu-id="fd7c4-160">Ha egy vállalati hitelesítésszolgáltatótól vagy egy nyilvánosan megbízható hitelesítésszolgáltató használ, nem kell toodo semmit ügyfélszámítógépek számára megbízhatónak ezek tanúsítványkiállítók óta.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-160">If you are using an enterprise certification authority or a publicly trusted certification authority, you do not need toodo anything since client computers trust these certificate issuers.</span></span> <span data-ttu-id="fd7c4-161">Ha önaláírt tanúsítványt használ, tooinstall hello nyilvános részét hello önaláírt tanúsítvány kell hello ügyfélszámítógépen hello megbízható tanúsítványok tárolójába.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-161">If you are using a self-signed certificate, you need tooinstall hello public part of hello self-signed certificate into hello trusted certificate store on hello client computer.</span></span>
>
>


## <a name="lock-down-ldaps-access-tooyour-managed-domain-over-hello-internet"></a><span data-ttu-id="fd7c4-162">Zárolási-le nyilas LDAPS tooyour által kezelt tartomány érje el hello internet</span><span class="sxs-lookup"><span data-stu-id="fd7c4-162">Lock-down LDAPS access tooyour managed domain over hello internet</span></span>
> [!NOTE]
> <span data-ttu-id="fd7c4-163">**Nem kötelező** – Ha nem engedélyezte a LDAPS hozzáférés toohello felügyelt tartomány több mint hello internet, hagyja ki a konfigurációs feladat.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-163">**Optional task** - If you have not enabled LDAPS access toohello managed domain over hello internet, skip this configuration task.</span></span>
>
>

<span data-ttu-id="fd7c4-164">Ez a feladat megkezdése előtt győződjön meg arról, végrehajtotta hello leírt [4. feladat](#task-4---enable-secure-ldap-access-over-the-internet).</span><span class="sxs-lookup"><span data-stu-id="fd7c4-164">Before you begin this task, ensure you have completed hello steps outlined in [Task 4](#task-4---enable-secure-ldap-access-over-the-internet).</span></span>

<span data-ttu-id="fd7c4-165">A felügyelt tartományok LDAPS hozzáférési kitettségének keresztül hello internetes biztonsági kockázatot jelentenek jelöli.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-165">Exposing your managed domain for LDAPS access over hello internet represents a security threat.</span></span> <span data-ttu-id="fd7c4-166">hello által kezelt tartomány érhető el a hello interneten, a használt biztonságos LDAP hello port (Ez azt jelenti, hogy a 636-os port).</span><span class="sxs-lookup"><span data-stu-id="fd7c4-166">hello managed domain is reachable from hello internet at hello port used for secure LDAP (that is, port 636).</span></span> <span data-ttu-id="fd7c4-167">Ezért dönthet úgy toorestrict hozzáférés toohello felügyelt tartomány toospecific ismert IP-címeket.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-167">Therefore, you can choose toorestrict access toohello managed domain toospecific known IP addresses.</span></span> <span data-ttu-id="fd7c4-168">A nagyobb biztonság érdekében hozzon létre egy hálózati biztonsági csoportot (NSG), és társítsa azt az hello alhálózatot, ha engedélyezte az Azure AD tartományi szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-168">For improved security, create a network security group (NSG) and associate it with hello subnet where you have enabled Azure AD Domain Services.</span></span>

<span data-ttu-id="fd7c4-169">hello alábbi táblázat mutatja be egy minta NSG is konfigurálhat, biztonságos LDAP hozzáférés keresztül hello toolock internet.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-169">hello following table illustrates a sample NSG you can configure, toolock down secure LDAP access over hello internet.</span></span> <span data-ttu-id="fd7c4-170">hello NSG tartalmaz egy TCP-porton keresztül 636 csak egy megadott készletből IP-címek bejövő LDAPS hozzáférést engedélyező szabályokat.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-170">hello NSG contains a set of rules that allow inbound LDAPS access over TCP port 636 only from a specified set of IP addresses.</span></span> <span data-ttu-id="fd7c4-171">hello alapértelmezett "DenyAll" szabály tooall egyéb bejövő forgalmát hello az interneten.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-171">hello default 'DenyAll' rule applies tooall other inbound traffic from hello internet.</span></span> <span data-ttu-id="fd7c4-172">hello NSG szabály tooallow LDAPS hozzáférés hello keresztül megadott IP-címekről érkező internetes rendelkezik magasabb prioritású, mint hello DenyAll NSG-szabály.</span><span class="sxs-lookup"><span data-stu-id="fd7c4-172">hello NSG rule tooallow LDAPS access over hello internet from specified IP addresses has a higher priority than hello DenyAll NSG rule.</span></span>

![A minta NSG toosecure LDAPS hozzáférés keresztül hello internet](./media/active-directory-domain-services-admin-guide/secure-ldap-sample-nsg.png)

<span data-ttu-id="fd7c4-174">**További információ** - [hálózati biztonsági csoportok](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="fd7c4-174">**More information** - [Network security groups](../virtual-network/virtual-networks-nsg.md).</span></span>

<br>

## <a name="related-content"></a><span data-ttu-id="fd7c4-175">Kapcsolódó tartalom</span><span class="sxs-lookup"><span data-stu-id="fd7c4-175">Related content</span></span>
* [<span data-ttu-id="fd7c4-176">Azure AD tartományi szolgáltatások – első lépések útmutató</span><span class="sxs-lookup"><span data-stu-id="fd7c4-176">Azure AD Domain Services - Getting Started guide</span></span>](active-directory-ds-getting-started.md)
* [<span data-ttu-id="fd7c4-177">Azure AD tartományi szolgáltatások által kezelt tartomány felügyelete</span><span class="sxs-lookup"><span data-stu-id="fd7c4-177">Administer an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-administer-domain.md)
* [<span data-ttu-id="fd7c4-178">Csoportházirend a egy Azure AD tartományi szolgáltatások által kezelt tartomány felügyelete</span><span class="sxs-lookup"><span data-stu-id="fd7c4-178">Administer Group Policy on an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-administer-group-policy.md)
* [<span data-ttu-id="fd7c4-179">Hálózati biztonsági csoportok</span><span class="sxs-lookup"><span data-stu-id="fd7c4-179">Network security groups</span></span>](../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="fd7c4-180">Hálózati biztonsági csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="fd7c4-180">Create a Network Security Group</span></span>](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)
