---
title: "Azure virtuális gépek aaaSpecific RDP hibaüzenetekben |} Microsoft Docs"
description: "Megérteni a vonatkozó hibaüzeneteket, amelyek jelenhet meg tett kísérlet során használja a távoli asztali kapcsolat tooa Windows rendszerű virtuális gép az Azure-ban"
keywords: "Távoli asztali hiba, a távoli asztali kapcsolat hiba, nem lehet kapcsolódni a tooVM, távoli asztal – hibaelhárítás"
services: virtual-machines-windows
documentationcenter: 
author: genlin
manager: timlt
editor: 
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: 5feb1d64-ee6f-4907-949a-a7cffcbc6153
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: support-article
ms.date: 05/26/2017
ms.author: genli
ms.openlocfilehash: 8e1551be23e696bd60adbd76c3e1ea86d9dd11aa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-specific-rdp-error-messages-tooa-windows-vm-in-azure"></a><span data-ttu-id="331ad-104">Hibaelhárítás az adott RDP hiba üzenetek tooa Windows Azure-ban</span><span class="sxs-lookup"><span data-stu-id="331ad-104">Troubleshooting specific RDP error messages tooa Windows VM in Azure</span></span>
<span data-ttu-id="331ad-105">Egy adott hibaüzenet jelenhet meg, az Azure-ban a távoli asztali kapcsolat tooa Windows rendszerű virtuális gép (VM) használatakor.</span><span class="sxs-lookup"><span data-stu-id="331ad-105">You may receive a specific error message when using Remote Desktop connection tooa Windows virtual machine (VM) in Azure.</span></span> <span data-ttu-id="331ad-106">Ez a cikk részletek néhány gyakori hibaüzenetek észlelt, és hibaelhárítási lépéseket tooresolve hello őket.</span><span class="sxs-lookup"><span data-stu-id="331ad-106">This article details some of hello more common error messages encountered, along with troubleshooting steps tooresolve them.</span></span> <span data-ttu-id="331ad-107">Ha a Kapcsolódás a virtuális gép tooyour problémák RDP Funkciót használnak a do azonban nem egy adott hibaüzenetet kapja, lásd: hello [hibaelhárítási útmutatója a távoli asztal](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="331ad-107">If you are having issues connecting tooyour VM using RDP but do not encounter a specific error message, see hello [troubleshooting guide for Remote Desktop](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="331ad-108">Információ a vonatkozó hibaüzeneteket: hello következő:</span><span class="sxs-lookup"><span data-stu-id="331ad-108">For information on specific error messages, see hello following:</span></span>

* <span data-ttu-id="331ad-109">[hello távoli munkamenet meg lett szakítva, mert nincs elérhető távoli asztali licenckiszolgálókat tooprovide licenc](#rdplicense).</span><span class="sxs-lookup"><span data-stu-id="331ad-109">[hello remote session was disconnected because there are no Remote Desktop License Servers available tooprovide a license](#rdplicense).</span></span>
* <span data-ttu-id="331ad-110">[A távoli asztal nem találja a hello "számítógépnév"](#rdpname).</span><span class="sxs-lookup"><span data-stu-id="331ad-110">[Remote Desktop can't find hello computer "name"](#rdpname).</span></span>
* <span data-ttu-id="331ad-111">[Hitelesítési hiba történt. hello helyi biztonsági szervezet nem érhető el](#rdpauth).</span><span class="sxs-lookup"><span data-stu-id="331ad-111">[An authentication error has occurred. hello Local Security Authority cannot be contacted](#rdpauth).</span></span>
* <span data-ttu-id="331ad-112">[Windows biztonsági hiba: A hitelesítő adatok nem működött](#wincred).</span><span class="sxs-lookup"><span data-stu-id="331ad-112">[Windows Security error: Your credentials did not work](#wincred).</span></span>
* <span data-ttu-id="331ad-113">[Ezen a számítógépen nem lehet kapcsolódni a távoli számítógép toohello](#rdpconnect).</span><span class="sxs-lookup"><span data-stu-id="331ad-113">[This computer can't connect toohello remote computer](#rdpconnect).</span></span>

<a id="rdplicense"></a>

## <a name="hello-remote-session-was-disconnected-because-there-are-no-remote-desktop-license-servers-available-tooprovide-a-license"></a><span data-ttu-id="331ad-114">hello távoli munkamenet meg lett szakítva, mert nincs távoli asztali licenckiszolgálókat elérhető tooprovide egy licencet.</span><span class="sxs-lookup"><span data-stu-id="331ad-114">hello remote session was disconnected because there are no Remote Desktop License Servers available tooprovide a license.</span></span>
<span data-ttu-id="331ad-115">OK: lejárt hello 120 napos türelmi időszak hello távoli asztal-kiszolgálói szerepkör, és tooinstall licencekre van szüksége.</span><span class="sxs-lookup"><span data-stu-id="331ad-115">Cause: hello 120-day licensing grace period for hello Remote Desktop Server role has expired and you need tooinstall licenses.</span></span>

<span data-ttu-id="331ad-116">A probléma megoldásához hello RDP-fájlt a helyi másolatot mentenek hello portálról, és futtassa a parancsot egy PowerShell-parancssorba tooconnect.</span><span class="sxs-lookup"><span data-stu-id="331ad-116">As a workaround, save a local copy of hello RDP file from hello portal and run this command at a PowerShell command prompt tooconnect.</span></span> <span data-ttu-id="331ad-117">Ezzel a lépéssel letiltja az adott kapcsolathoz csak licencelési:</span><span class="sxs-lookup"><span data-stu-id="331ad-117">This step disables licensing for just that connection:</span></span>

        mstsc <File name>.RDP /admin

<span data-ttu-id="331ad-118">Ha több mint két egyidejű távoli asztali kapcsolatok toohello VM ténylegesen nem szükséges, a Kiszolgálókezelő tooremove hello távoli asztali kiszolgáló szerepkör is használhatja.</span><span class="sxs-lookup"><span data-stu-id="331ad-118">If you don't actually need more than two simultaneous Remote Desktop connections toohello VM, you can use Server Manager tooremove hello Remote Desktop Server role.</span></span>

<span data-ttu-id="331ad-119">További információkért lásd: hello blogbejegyzésben [Azure virtuális Gépen sikertelen, és "Nincs távoli asztali licenckiszolgálókat érhető el"](https://blogs.msdn.microsoft.com/mast/2014/01/21/rdp-to-azure-vm-fails-with-no-remote-desktop-license-servers-available/).</span><span class="sxs-lookup"><span data-stu-id="331ad-119">For more information, see hello blog post [Azure VM fails with "No Remote Desktop License Servers available"](https://blogs.msdn.microsoft.com/mast/2014/01/21/rdp-to-azure-vm-fails-with-no-remote-desktop-license-servers-available/).</span></span>

<a id="rdpname"></a>

## <a name="remote-desktop-cant-find-hello-computer-name"></a><span data-ttu-id="331ad-120">A távoli asztal hello számítógép "name" nem található.</span><span class="sxs-lookup"><span data-stu-id="331ad-120">Remote Desktop can't find hello computer "name".</span></span>
<span data-ttu-id="331ad-121">OK: hello távoli asztali ügyfél a számítógép neve nem oldható fel hello hello számítógép hello beállítások hello RDP-fájlt.</span><span class="sxs-lookup"><span data-stu-id="331ad-121">Cause: hello Remote Desktop client on your computer can't resolve hello name of hello computer in hello settings of hello RDP file.</span></span>

<span data-ttu-id="331ad-122">A lehetséges megoldásokról:</span><span class="sxs-lookup"><span data-stu-id="331ad-122">Possible solutions:</span></span>

* <span data-ttu-id="331ad-123">Ha egy szervezet intraneten, ellenőrizze, hogy a számítógép hozzáférés toohello proxykiszolgálóval rendelkezik-e, és HTTPS-forgalom tooit küldhet.</span><span class="sxs-lookup"><span data-stu-id="331ad-123">If you're on an organization's intranet, make sure that your computer has access toohello proxy server and can send HTTPS traffic tooit.</span></span>
* <span data-ttu-id="331ad-124">Ha helyileg tárolt RDP-fájl használata esetén próbálja hello egy hello portál által létrehozott használatával.</span><span class="sxs-lookup"><span data-stu-id="331ad-124">If you're using a locally stored RDP file, try using hello one that's generated by hello portal.</span></span> <span data-ttu-id="331ad-125">Ez a lépés biztosítja, hogy rendelkezik-e helyes DNS-névvel hello hello virtuális gépet, vagy hello felhőalapú szolgáltatás és virtuális gép hello hello végpont portja.</span><span class="sxs-lookup"><span data-stu-id="331ad-125">This step ensures that you have hello correct DNS name for hello virtual machine, or hello cloud service and hello endpoint port of hello VM.</span></span> <span data-ttu-id="331ad-126">Íme egy minta hello portál által előállított RDP-fájlt:</span><span class="sxs-lookup"><span data-stu-id="331ad-126">Here is a sample RDP file generated by hello portal:</span></span>
  
        full address:s:tailspin-azdatatier.cloudapp.net:55919
        prompt for credentials:i:1

<span data-ttu-id="331ad-127">hello cím része az RDP-fájlt tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="331ad-127">hello address portion of this RDP file has:</span></span>

* <span data-ttu-id="331ad-128">hello teljesen minősített tartománynevét, amely tartalmazza a virtuális gép ("Dejójáték-azdatatier.cloudapp.net" Ebben a példában) hello hello felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="331ad-128">hello fully qualified domain name of hello cloud service that contains hello VM ("tailspin-azdatatier.cloudapp.net" in this example).</span></span>
* <span data-ttu-id="331ad-129">hello külső TCP-port hello végpont a távoli asztal forgalmat (55919).</span><span class="sxs-lookup"><span data-stu-id="331ad-129">hello external TCP port of hello endpoint for Remote Desktop traffic (55919).</span></span>

<a id="rdpauth"></a>

## <a name="an-authentication-error-has-occurred-hello-local-security-authority-cannot-be-contacted"></a><span data-ttu-id="331ad-130">Hitelesítési hiba történt.</span><span class="sxs-lookup"><span data-stu-id="331ad-130">An authentication error has occurred.</span></span> <span data-ttu-id="331ad-131">nem lehet kapcsolódni a helyi biztonsági szervezet hello.</span><span class="sxs-lookup"><span data-stu-id="331ad-131">hello Local Security Authority cannot be contacted.</span></span>
<span data-ttu-id="331ad-132">OK: hello cél virtuális gép nem található hello biztonsági hatóság hello felhasználói részét a hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="331ad-132">Cause: hello target VM can't locate hello security authority in hello user name portion of your credentials.</span></span>

<span data-ttu-id="331ad-133">A felhasználónév esetén hello formában *SecurityAuthority*\\*felhasználónév* (Példa: corp\felhasználó1), hello *SecurityAuthority* részére vagy hello VM számítógép neve (a helyi biztonsági szervezet hello) vagy egy Active Directory-tartomány nevét.</span><span class="sxs-lookup"><span data-stu-id="331ad-133">When your user name is in hello form *SecurityAuthority*\\*UserName* (example: CORP\User1), hello *SecurityAuthority* portion is either hello VM's computer name (for hello local security authority) or an Active Directory domain name.</span></span>

<span data-ttu-id="331ad-134">A lehetséges megoldásokról:</span><span class="sxs-lookup"><span data-stu-id="331ad-134">Possible solutions:</span></span>

* <span data-ttu-id="331ad-135">Ha hello fiók helyi toohello VM, győződjön meg arról, hogy hello virtuális gép nevét helyesen írta-e.</span><span class="sxs-lookup"><span data-stu-id="331ad-135">If hello account is local toohello VM, make sure that hello VM name is spelled correctly.</span></span>
* <span data-ttu-id="331ad-136">Ha hello fiók Active Directory-tartomány, helyesírás hello hello tartománynév.</span><span class="sxs-lookup"><span data-stu-id="331ad-136">If hello account is on an Active Directory domain, check hello spelling of hello domain name.</span></span>
* <span data-ttu-id="331ad-137">Ha egy Active Directory tartományi fiók, és hello tartomány nevét helyesen írta-e, ellenőrizze, hogy a tartományvezérlő elérhető az adott tartományban.</span><span class="sxs-lookup"><span data-stu-id="331ad-137">If it is an Active Directory domain account and hello domain name is spelled correctly, verify that a domain controller is available in that domain.</span></span> <span data-ttu-id="331ad-138">Ez a jelenség a tartományvezérlőnek, hogy a tartományvezérlő nem érhető el, mivel nincs elindítva az Azure virtuális hálózatokhoz.</span><span class="sxs-lookup"><span data-stu-id="331ad-138">It's a common issue in Azure virtual networks that contain domain controllers that a domain controller is unavailable because it hasn't been started.</span></span> <span data-ttu-id="331ad-139">A probléma megoldásához használhatja egy helyi rendszergazdai fiók helyett egy olyan tartományi fiók.</span><span class="sxs-lookup"><span data-stu-id="331ad-139">As a workaround, you can use a local administrator account instead of a domain account.</span></span>

<a id="wincred"></a>

## <a name="windows-security-error-your-credentials-did-not-work"></a><span data-ttu-id="331ad-140">Windows biztonsági hiba: A hitelesítő adatok nem működik.</span><span class="sxs-lookup"><span data-stu-id="331ad-140">Windows Security error: Your credentials did not work.</span></span>
<span data-ttu-id="331ad-141">OK: a hello cél virtuális gép nem lehet érvényesíteni a fióknevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="331ad-141">Cause: hello target VM can't validate your account name and password.</span></span>

<span data-ttu-id="331ad-142">A Windows-alapú számítógép ellenőrzéséhez hello vagy helyi fiók, vagy egy olyan tartományi fiók hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="331ad-142">A Windows-based computer can validate hello credentials of either a local account or a domain account.</span></span>

* <span data-ttu-id="331ad-143">A helyi fiókok használata a hello *számítógépnév*\\*felhasználónév* szintaxis (Példa: SQL1\Admin4798).</span><span class="sxs-lookup"><span data-stu-id="331ad-143">For local accounts, use hello *ComputerName*\\*UserName* syntax (example: SQL1\Admin4798).</span></span>
* <span data-ttu-id="331ad-144">A tartományi fiókokat, használja a hello *tartománynév*\\*felhasználónév* szintaxis (Példa: CONTOSO\peterodman).</span><span class="sxs-lookup"><span data-stu-id="331ad-144">For domain accounts, use hello *DomainName*\\*UserName* syntax (example: CONTOSO\peterodman).</span></span>

<span data-ttu-id="331ad-145">Hello jelentkezett be helyi rendszergazdai fiók alakítja át a virtuális gép tooa tartományvezérlő egy új Active Directory-erdő léptette be, ha ugyanaz a hello fiók tooan egyenértékű jelszó hello új erdőben és tartományban.</span><span class="sxs-lookup"><span data-stu-id="331ad-145">If you have promoted your VM tooa domain controller in a new Active Directory forest, hello local administrator account that you signed in with is converted tooan equivalent account with hello same password in hello new forest and domain.</span></span> <span data-ttu-id="331ad-146">hello helyi fiókot majd törölték.</span><span class="sxs-lookup"><span data-stu-id="331ad-146">hello local account is then deleted.</span></span>

<span data-ttu-id="331ad-147">Például ha hello helyi fiókkal DC1\DCAdmin bejelentkezve, és majd előléptetett hello virtuális gép hello corp.contoso.com tartományhoz tartozó új erdő tartományvezérlőjeként, hello helyi fiók törlése DC1\DCAdmin és egy új (CORP\DCAdmin tartományi fiókot. ) jön létre a hello azonos jelszót.</span><span class="sxs-lookup"><span data-stu-id="331ad-147">For example, if you signed in with hello local account DC1\DCAdmin, and then promoted hello virtual machine as a domain controller in a new forest for hello corp.contoso.com domain, hello DC1\DCAdmin local account gets deleted and a new domain account (CORP\DCAdmin) is created with hello same password.</span></span>

<span data-ttu-id="331ad-148">Győződjön meg arról, hogy hello fióknév hello virtuális gép ellenőrizheti egy érvényes fiókot, és adott hello jelszava helyes nevét.</span><span class="sxs-lookup"><span data-stu-id="331ad-148">Make sure that hello account name is a name that hello virtual machine can verify as a valid account, and that hello password is correct.</span></span>

<span data-ttu-id="331ad-149">Ha szüksége toochange hello hello helyi rendszergazdai fiók jelszavát, tekintse meg [hogyan tooreset jelszó vagy a távoli asztal hello szolgáltatás fut a Windows virtuális gépek](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="331ad-149">If you need toochange hello password of hello local administrator account, see [How tooreset a password or hello Remote Desktop service for Windows virtual machines](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<a id="rdpconnect"></a>

## <a name="this-computer-cant-connect-toohello-remote-computer"></a><span data-ttu-id="331ad-150">Ezen a számítógépen nem lehet kapcsolódni a távoli számítógép toohello.</span><span class="sxs-lookup"><span data-stu-id="331ad-150">This computer can't connect toohello remote computer.</span></span>
<span data-ttu-id="331ad-151">OK: hello tooconnect által használt fiók nem jogosult a távoli asztal bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="331ad-151">Cause: hello account that's used tooconnect does not have Remote Desktop sign-in rights.</span></span>

<span data-ttu-id="331ad-152">Minden Windows-számítógép a távoli asztali felhasználók helyi csoport, amely tartalmazza a hello fiókokat és csoportokat, amelyek jelentkezhet be távolról rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="331ad-152">Every Windows computer has a Remote Desktop users local group, which contains hello accounts and groups that can sign into it remotely.</span></span> <span data-ttu-id="331ad-153">Hello helyi Rendszergazdák csoport tagjai is hozzáférhetnek, annak ellenére, hogy ezek a fiókok nem szereplő hello távoli asztali felhasználók helyi csoportot.</span><span class="sxs-lookup"><span data-stu-id="331ad-153">Members of hello local administrators group also have access, even though those accounts are not listed in hello Remote Desktop users local group.</span></span> <span data-ttu-id="331ad-154">Tartományhoz csatlakozó gépek hello helyi Rendszergazdák csoport hello tartomány hello tartományi rendszergazdák is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="331ad-154">For domain-joined machines, hello local administrators group also contains hello domain administrators for hello domain.</span></span>

<span data-ttu-id="331ad-155">Győződjön meg arról, hogy a tooconnect használata hello fiókja rendelkezik-e a távoli asztal bejelentkezési jogokkal.</span><span class="sxs-lookup"><span data-stu-id="331ad-155">Make sure that hello account you're using tooconnect with has Remote Desktop sign-in rights.</span></span> <span data-ttu-id="331ad-156">A probléma megoldásához használja a tartomány vagy helyi rendszergazdai fiók tooconnect távoli asztali kapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="331ad-156">As a workaround, use a domain or local administrator account tooconnect over Remote Desktop.</span></span> <span data-ttu-id="331ad-157">tooadd hello kívánt fiókot toohello távoli asztali felhasználók helyi csoport, hello Microsoft Management Console beépülő modullal (**Rendszereszközök > helyi felhasználók és csoportok > csoportok > Remote Desktop Users**).</span><span class="sxs-lookup"><span data-stu-id="331ad-157">tooadd hello desired account toohello Remote Desktop users local group, use hello Microsoft Management Console snap-in (**System Tools > Local Users and Groups > Groups > Remote Desktop Users**).</span></span>

## <a name="next-steps"></a><span data-ttu-id="331ad-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="331ad-158">Next steps</span></span>
<span data-ttu-id="331ad-159">Ha ezek a hibák egyike sem történt, és csatlakozás RDP Funkciót használnak ismeretlen hibája, lásd: hello [hibaelhárítási útmutatója a távoli asztal](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="331ad-159">If none of these errors occurred and you have an unknown issue with connecting using RDP, see hello [troubleshooting guide for Remote Desktop](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

* <span data-ttu-id="331ad-160">Című témakörben leírt lépéseket a virtuális gép futó alkalmazásokhoz való hozzáférés [kapcsolatos problémák elhárítása access tooan alkalmazást egy Azure virtuális Gépen futó](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="331ad-160">For troubleshooting steps in accessing applications running on a VM, see [Troubleshoot access tooan application running on an Azure VM](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
* <span data-ttu-id="331ad-161">Ha a Secure Shell (SSH) tooconnect tooa Linux virtuális gép használata az Azure-ban problémát tapasztal, tekintse meg [hibaelhárítása SSH kapcsolatok tooa Linux virtuális gép az Azure-ban](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="331ad-161">If you are having issues using Secure Shell (SSH) tooconnect tooa Linux VM in Azure, see [Troubleshoot SSH connections tooa Linux VM in Azure](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

