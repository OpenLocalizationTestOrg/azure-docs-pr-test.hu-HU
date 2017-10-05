---
title: "Azure virtuális gépek meghatározott RDP hibaüzenetek |} Microsoft Docs"
description: "Megérteni a vonatkozó hibaüzeneteket, amelyek jelenhet meg tett kísérlet során használja Windows virtuális gépek távoli asztali kapcsolatot az Azure-ban"
keywords: "Távoli asztali hiba, a távoli asztali kapcsolat hiba, nem lehet csatlakozni a virtuális gép, távoli asztal – hibaelhárítás"
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
ms.openlocfilehash: e7c049106726a15e96d4ebe7c7c0388a29c546c8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-specific-rdp-error-messages-to-a-windows-vm-in-azure"></a><span data-ttu-id="29527-104">A Windows Azure-ban az adott RDP hibaüzenetek hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="29527-104">Troubleshooting specific RDP error messages to a Windows VM in Azure</span></span>
<span data-ttu-id="29527-105">Egy adott hibaüzenet jelenhet meg, az Azure-ban a távoli asztali kapcsolatot egy Windows rendszerű virtuális gép (VM) használatakor.</span><span class="sxs-lookup"><span data-stu-id="29527-105">You may receive a specific error message when using Remote Desktop connection to a Windows virtual machine (VM) in Azure.</span></span> <span data-ttu-id="29527-106">Ez a cikk részletesen néhány, a hibát, és azok megoldását hibaelhárítási Gyakori hibaüzenetek.</span><span class="sxs-lookup"><span data-stu-id="29527-106">This article details some of the more common error messages encountered, along with troubleshooting steps to resolve them.</span></span> <span data-ttu-id="29527-107">Ha csatlakozni a virtuális gép RDP Funkciót használnak a do azonban nem egy adott hibaüzenetet kapja, tekintse meg a [hibaelhárítási útmutatója a távoli asztal](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="29527-107">If you are having issues connecting to your VM using RDP but do not encounter a specific error message, see the [troubleshooting guide for Remote Desktop](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="29527-108">További információ a vonatkozó hibaüzeneteket tekintse át a következőket:</span><span class="sxs-lookup"><span data-stu-id="29527-108">For information on specific error messages, see the following:</span></span>

* <span data-ttu-id="29527-109">[A távoli munkamenet meg lett szakítva, mert nincs távoli asztali licenckiszolgálókat kínálnak az licenc](#rdplicense).</span><span class="sxs-lookup"><span data-stu-id="29527-109">[The remote session was disconnected because there are no Remote Desktop License Servers available to provide a license](#rdplicense).</span></span>
* <span data-ttu-id="29527-110">[A távoli asztal nem találja a számítógépen "name"](#rdpname).</span><span class="sxs-lookup"><span data-stu-id="29527-110">[Remote Desktop can't find the computer "name"](#rdpname).</span></span>
* <span data-ttu-id="29527-111">[Hitelesítési hiba történt. Nem lehet kapcsolódni a helyi biztonsági szervezet](#rdpauth).</span><span class="sxs-lookup"><span data-stu-id="29527-111">[An authentication error has occurred. The Local Security Authority cannot be contacted](#rdpauth).</span></span>
* <span data-ttu-id="29527-112">[Windows biztonsági hiba: A hitelesítő adatok nem működött](#wincred).</span><span class="sxs-lookup"><span data-stu-id="29527-112">[Windows Security error: Your credentials did not work](#wincred).</span></span>
* <span data-ttu-id="29527-113">[Ez a számítógép nem tud kapcsolódni a távoli számítógép](#rdpconnect).</span><span class="sxs-lookup"><span data-stu-id="29527-113">[This computer can't connect to the remote computer](#rdpconnect).</span></span>

<a id="rdplicense"></a>

## <a name="the-remote-session-was-disconnected-because-there-are-no-remote-desktop-license-servers-available-to-provide-a-license"></a><span data-ttu-id="29527-114">A távoli munkamenet meg lett szakítva, mert nincs távoli asztali licenckiszolgálókat licenc kínálnak.</span><span class="sxs-lookup"><span data-stu-id="29527-114">The remote session was disconnected because there are no Remote Desktop License Servers available to provide a license.</span></span>
<span data-ttu-id="29527-115">OK: A 120 napos türelmi időszak a távoli asztal kiszolgálói szerepkör lejárt, és telepítenie kell licenceket.</span><span class="sxs-lookup"><span data-stu-id="29527-115">Cause: The 120-day licensing grace period for the Remote Desktop Server role has expired and you need to install licenses.</span></span>

<span data-ttu-id="29527-116">A probléma megoldásához az RDP-fájlt a helyi másolatot mentenek a portálról, és futtassa ezt a parancsot egy PowerShell parancssorba való csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="29527-116">As a workaround, save a local copy of the RDP file from the portal and run this command at a PowerShell command prompt to connect.</span></span> <span data-ttu-id="29527-117">Ezzel a lépéssel letiltja az adott kapcsolathoz csak licencelési:</span><span class="sxs-lookup"><span data-stu-id="29527-117">This step disables licensing for just that connection:</span></span>

        mstsc <File name>.RDP /admin

<span data-ttu-id="29527-118">Ha a virtuális gép több mint két egyidejű távoli asztali kapcsolatok ténylegesen nem szükséges, a Kiszolgálókezelő használatával távolítsa el a távoli asztal-kiszolgálói szerepkört.</span><span class="sxs-lookup"><span data-stu-id="29527-118">If you don't actually need more than two simultaneous Remote Desktop connections to the VM, you can use Server Manager to remove the Remote Desktop Server role.</span></span>

<span data-ttu-id="29527-119">További információkért lásd a következő blogbejegyzésben [Azure virtuális Gépen sikertelen, és "Nincs távoli asztali licenckiszolgálókat érhető el"](https://blogs.msdn.microsoft.com/mast/2014/01/21/rdp-to-azure-vm-fails-with-no-remote-desktop-license-servers-available/).</span><span class="sxs-lookup"><span data-stu-id="29527-119">For more information, see the blog post [Azure VM fails with "No Remote Desktop License Servers available"](https://blogs.msdn.microsoft.com/mast/2014/01/21/rdp-to-azure-vm-fails-with-no-remote-desktop-license-servers-available/).</span></span>

<a id="rdpname"></a>

## <a name="remote-desktop-cant-find-the-computer-name"></a><span data-ttu-id="29527-120">A távoli asztal nem találja a számítógépen "name".</span><span class="sxs-lookup"><span data-stu-id="29527-120">Remote Desktop can't find the computer "name".</span></span>
<span data-ttu-id="29527-121">OK: A távoli asztali ügyfél a számítógépen nem oldható fel az RDP-fájl beállításait a számítógép nevét.</span><span class="sxs-lookup"><span data-stu-id="29527-121">Cause: The Remote Desktop client on your computer can't resolve the name of the computer in the settings of the RDP file.</span></span>

<span data-ttu-id="29527-122">A lehetséges megoldásokról:</span><span class="sxs-lookup"><span data-stu-id="29527-122">Possible solutions:</span></span>

* <span data-ttu-id="29527-123">Ha egy szervezet intraneten, ellenőrizze, hogy a számítógép hozzáfér a proxykiszolgáló-e, és küldhet HTTPS-forgalmat az.</span><span class="sxs-lookup"><span data-stu-id="29527-123">If you're on an organization's intranet, make sure that your computer has access to the proxy server and can send HTTPS traffic to it.</span></span>
* <span data-ttu-id="29527-124">Ha helyileg tárolt RDP-fájlt használ, próbálja meg azt, amelyik a portálon állítja elő.</span><span class="sxs-lookup"><span data-stu-id="29527-124">If you're using a locally stored RDP file, try using the one that's generated by the portal.</span></span> <span data-ttu-id="29527-125">Ez a lépés biztosítja, hogy rendelkezik-e a virtuális gép vagy a felhőszolgáltatás és a virtuális gép végpont portja helytelen DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="29527-125">This step ensures that you have the correct DNS name for the virtual machine, or the cloud service and the endpoint port of the VM.</span></span> <span data-ttu-id="29527-126">Íme egy minta RDP-fájlt a portálon állítja elő:</span><span class="sxs-lookup"><span data-stu-id="29527-126">Here is a sample RDP file generated by the portal:</span></span>
  
        full address:s:tailspin-azdatatier.cloudapp.net:55919
        prompt for credentials:i:1

<span data-ttu-id="29527-127">A cím része az RDP-fájlt tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="29527-127">The address portion of this RDP file has:</span></span>

* <span data-ttu-id="29527-128">A teljesen minősített tartománynév a felhőalapú szolgáltatás, amely tartalmazza a virtuális gép ("Dejójáték-azdatatier.cloudapp.net" Ebben a példában).</span><span class="sxs-lookup"><span data-stu-id="29527-128">The fully qualified domain name of the cloud service that contains the VM ("tailspin-azdatatier.cloudapp.net" in this example).</span></span>
* <span data-ttu-id="29527-129">A külső TCP-portot a végpont a távoli asztal forgalmat (55919).</span><span class="sxs-lookup"><span data-stu-id="29527-129">The external TCP port of the endpoint for Remote Desktop traffic (55919).</span></span>

<a id="rdpauth"></a>

## <a name="an-authentication-error-has-occurred-the-local-security-authority-cannot-be-contacted"></a><span data-ttu-id="29527-130">Hitelesítési hiba történt.</span><span class="sxs-lookup"><span data-stu-id="29527-130">An authentication error has occurred.</span></span> <span data-ttu-id="29527-131">Nem lehet kapcsolódni a helyi biztonsági szervezet.</span><span class="sxs-lookup"><span data-stu-id="29527-131">The Local Security Authority cannot be contacted.</span></span>
<span data-ttu-id="29527-132">OK: A cél virtuális gép nem található a biztonsági jogosultságokat a felhasználó hitelesítő adatait részét.</span><span class="sxs-lookup"><span data-stu-id="29527-132">Cause: The target VM can't locate the security authority in the user name portion of your credentials.</span></span>

<span data-ttu-id="29527-133">A felhasználói név esetén a képernyő *SecurityAuthority*\\*felhasználónév* (Példa: corp\felhasználó1), a *SecurityAuthority* részére vagy a virtuális gép számítógép neve (a helyi biztonsági szervezet) vagy egy Active Directory-tartomány nevét.</span><span class="sxs-lookup"><span data-stu-id="29527-133">When your user name is in the form *SecurityAuthority*\\*UserName* (example: CORP\User1), the *SecurityAuthority* portion is either the VM's computer name (for the local security authority) or an Active Directory domain name.</span></span>

<span data-ttu-id="29527-134">A lehetséges megoldásokról:</span><span class="sxs-lookup"><span data-stu-id="29527-134">Possible solutions:</span></span>

* <span data-ttu-id="29527-135">Ha a fiók nem helyi, a virtuális géphez, győződjön meg arról, hogy a virtuális gép nevét helyesen írta-e.</span><span class="sxs-lookup"><span data-stu-id="29527-135">If the account is local to the VM, make sure that the VM name is spelled correctly.</span></span>
* <span data-ttu-id="29527-136">Ha a fiók az Active Directory-tartomány, a helyesírás a tartomány neve.</span><span class="sxs-lookup"><span data-stu-id="29527-136">If the account is on an Active Directory domain, check the spelling of the domain name.</span></span>
* <span data-ttu-id="29527-137">Ha egy Active Directory tartományi fiók, és a tartomány nevét helyesen írta-e, ellenőrizze, hogy a tartományvezérlő elérhető az adott tartományban.</span><span class="sxs-lookup"><span data-stu-id="29527-137">If it is an Active Directory domain account and the domain name is spelled correctly, verify that a domain controller is available in that domain.</span></span> <span data-ttu-id="29527-138">Ez a jelenség a tartományvezérlőnek, hogy a tartományvezérlő nem érhető el, mivel nincs elindítva az Azure virtuális hálózatokhoz.</span><span class="sxs-lookup"><span data-stu-id="29527-138">It's a common issue in Azure virtual networks that contain domain controllers that a domain controller is unavailable because it hasn't been started.</span></span> <span data-ttu-id="29527-139">A probléma megoldásához használhatja egy helyi rendszergazdai fiók helyett egy olyan tartományi fiók.</span><span class="sxs-lookup"><span data-stu-id="29527-139">As a workaround, you can use a local administrator account instead of a domain account.</span></span>

<a id="wincred"></a>

## <a name="windows-security-error-your-credentials-did-not-work"></a><span data-ttu-id="29527-140">Windows biztonsági hiba: A hitelesítő adatok nem működik.</span><span class="sxs-lookup"><span data-stu-id="29527-140">Windows Security error: Your credentials did not work.</span></span>
<span data-ttu-id="29527-141">OK: A cél virtuális gép nem lehet érvényesíteni a fióknevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="29527-141">Cause: The target VM can't validate your account name and password.</span></span>

<span data-ttu-id="29527-142">Egy Windows-alapú számítógép azt is ellenőrzi a helyi fiókkal vagy egy olyan tartományi fiók hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="29527-142">A Windows-based computer can validate the credentials of either a local account or a domain account.</span></span>

* <span data-ttu-id="29527-143">A helyi fiókok használata a *számítógépnév*\\*felhasználónév* szintaxis (Példa: SQL1\Admin4798).</span><span class="sxs-lookup"><span data-stu-id="29527-143">For local accounts, use the *ComputerName*\\*UserName* syntax (example: SQL1\Admin4798).</span></span>
* <span data-ttu-id="29527-144">A tartományi fiókokat, használja a *tartománynév*\\*felhasználónév* szintaxis (Példa: CONTOSO\peterodman).</span><span class="sxs-lookup"><span data-stu-id="29527-144">For domain accounts, use the *DomainName*\\*UserName* syntax (example: CONTOSO\peterodman).</span></span>

<span data-ttu-id="29527-145">A virtuális Gépet egy olyan tartományvezérlőre, új Active Directory-erdőben található léptette be, ha a helyi rendszergazdai fiókkal jelentkezett be, ugyanezt a jelszót az új erdőben és tartományban egyenértékű fiókja lesz konvertálva.</span><span class="sxs-lookup"><span data-stu-id="29527-145">If you have promoted your VM to a domain controller in a new Active Directory forest, the local administrator account that you signed in with is converted to an equivalent account with the same password in the new forest and domain.</span></span> <span data-ttu-id="29527-146">Majd a helyi fiókot törölték.</span><span class="sxs-lookup"><span data-stu-id="29527-146">The local account is then deleted.</span></span>

<span data-ttu-id="29527-147">Például ha a helyi fiókkal DC1\DCAdmin bejelentkezve, és majd elő a virtuális gép a corp.contoso.com tartományhoz tartozó új erdő tartományvezérlőjeként, az DC1\DCAdmin helyi fiók lekérdezi törlődik, és egy új tartományi fiók (CORP\DCAdmin) hozza létre a jelszót.</span><span class="sxs-lookup"><span data-stu-id="29527-147">For example, if you signed in with the local account DC1\DCAdmin, and then promoted the virtual machine as a domain controller in a new forest for the corp.contoso.com domain, the DC1\DCAdmin local account gets deleted and a new domain account (CORP\DCAdmin) is created with the same password.</span></span>

<span data-ttu-id="29527-148">Győződjön meg arról, hogy a fiók nevét a nevet, amely a virtuális gép ellenőrizheti egy érvényes fiókot, és a jelszó helyességéről.</span><span class="sxs-lookup"><span data-stu-id="29527-148">Make sure that the account name is a name that the virtual machine can verify as a valid account, and that the password is correct.</span></span>

<span data-ttu-id="29527-149">Ha módosítania kell a helyi rendszergazdai fiók jelszavát, lásd: [egy jelszót, vagy a Windows virtuális gépekhez távoli asztal szolgáltatás alaphelyzetbe állításával](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="29527-149">If you need to change the password of the local administrator account, see [How to reset a password or the Remote Desktop service for Windows virtual machines](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<a id="rdpconnect"></a>

## <a name="this-computer-cant-connect-to-the-remote-computer"></a><span data-ttu-id="29527-150">Ez a számítógép nem tud kapcsolódni a távoli számítógépen.</span><span class="sxs-lookup"><span data-stu-id="29527-150">This computer can't connect to the remote computer.</span></span>
<span data-ttu-id="29527-151">OK: A való csatlakozáshoz használt fiók nem jogosult a távoli asztal bejelentkezhet.</span><span class="sxs-lookup"><span data-stu-id="29527-151">Cause: The account that's used to connect does not have Remote Desktop sign-in rights.</span></span>

<span data-ttu-id="29527-152">Minden Windows-számítógép a távoli asztali felhasználók helyi csoport, amely tartalmazza a fiókokat és csoportokat, amelyek jelentkezhet be távolról rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="29527-152">Every Windows computer has a Remote Desktop users local group, which contains the accounts and groups that can sign into it remotely.</span></span> <span data-ttu-id="29527-153">A helyi Rendszergazdák csoport tagjai is hozzáférhetnek, annak ellenére, hogy ezek a fiókok nem szerepelnek a távoli asztali felhasználók helyi csoporthoz.</span><span class="sxs-lookup"><span data-stu-id="29527-153">Members of the local administrators group also have access, even though those accounts are not listed in the Remote Desktop users local group.</span></span> <span data-ttu-id="29527-154">Tartományhoz csatlakozó gépek esetén a helyi Rendszergazdák csoport a tartomány a tartományi rendszergazdák is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="29527-154">For domain-joined machines, the local administrators group also contains the domain administrators for the domain.</span></span>

<span data-ttu-id="29527-155">Győződjön meg arról, hogy a fiók összekötése használata távoli asztal bejelentkezési jogokkal.</span><span class="sxs-lookup"><span data-stu-id="29527-155">Make sure that the account you're using to connect with has Remote Desktop sign-in rights.</span></span> <span data-ttu-id="29527-156">A probléma megoldásához használja a tartomány vagy helyi rendszergazdai fiók távoli asztali kapcsolaton keresztül csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="29527-156">As a workaround, use a domain or local administrator account to connect over Remote Desktop.</span></span> <span data-ttu-id="29527-157">A távoli asztali felhasználók helyi csoporthoz a kívánt fiók hozzáadásához használja a Microsoft Management Console beépülő modult (**Rendszereszközök > helyi felhasználók és csoportok > csoportok > Remote Desktop Users**).</span><span class="sxs-lookup"><span data-stu-id="29527-157">To add the desired account to the Remote Desktop users local group, use the Microsoft Management Console snap-in (**System Tools > Local Users and Groups > Groups > Remote Desktop Users**).</span></span>

## <a name="next-steps"></a><span data-ttu-id="29527-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="29527-158">Next steps</span></span>
<span data-ttu-id="29527-159">Ha ezek a hibák egyike sem történt, és az ismeretlen csatlakozás RDP segítségével adja ki, tekintse meg a [hibaelhárítási útmutatója a távoli asztal](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="29527-159">If none of these errors occurred and you have an unknown issue with connecting using RDP, see the [troubleshooting guide for Remote Desktop](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

* <span data-ttu-id="29527-160">Című témakörben leírt lépéseket a virtuális gép futó alkalmazásokhoz való hozzáférés [egy Azure virtuális gépen futó alkalmazáshoz való hozzáférés hibáinak elhárítása](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="29527-160">For troubleshooting steps in accessing applications running on a VM, see [Troubleshoot access to an application running on an Azure VM](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
* <span data-ttu-id="29527-161">Ha a Secure Shell (SSH) használatával szeretne csatlakozni egy Linux virtuális Gépet az Azure-ban, olvassa el a problémákat [hibaelhárítása SSH kapcsolatok egy Linux virtuális Gépet az Azure-ban](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="29527-161">If you are having issues using Secure Shell (SSH) to connect to a Linux VM in Azure, see [Troubleshoot SSH connections to a Linux VM in Azure](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

