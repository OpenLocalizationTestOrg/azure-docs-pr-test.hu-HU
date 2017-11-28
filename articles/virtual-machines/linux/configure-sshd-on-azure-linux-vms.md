---
title: "aaaConfigure SSHD Azure Linux virtuális gépeken |} Microsoft Docs"
description: "Ajánlott biztonsági eljárások és toolockdown SSH tooAzure Linux virtuális gépek SSHD konfigurálja."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/21/2016
ms.author: v-livech
ms.openlocfilehash: c2361be7199a24b129c06acfc899dd32f6e1d6fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-sshd-on-azure-linux-vms"></a><span data-ttu-id="640f8-103">Az Azure Linux virtuális gépeken futó SSHD konfigurálása</span><span class="sxs-lookup"><span data-stu-id="640f8-103">Configure SSHD on Azure Linux VMs</span></span>

<span data-ttu-id="640f8-104">Ez a cikk bemutatja, hogyan toolockdown hello jelszavak helyett a SSH-kulcsok használatával SSH-kiszolgáló Linux, tooprovide bevált gyakorlatok biztonsági, valamint a toospeed hello SSH bejelentkezési folyamatot.</span><span class="sxs-lookup"><span data-stu-id="640f8-104">This article shows how toolockdown hello SSH Server on Linux, tooprovide best practices security and also toospeed up hello SSH login process by using SSH keys instead of passwords.</span></span>  <span data-ttu-id="640f8-105">toofurther zárolási fogjuk toodisable hello gyökér szintű felhasználó nem tud toologin SSHD hello felhasználók, amelyek számára engedélyezett egy jóváhagyott listájából keresztül toologin letiltása az SSH protokoll verzió: 1, a minimális kulcs bit beállítása és automatikus-kijelentkezik a tétlen felhasználók konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="640f8-105">toofurther lockdown SSHD we are going toodisable hello root user from being able toologin, limit hello users that are allowed toologin via an approved group list, disabling SSH protocol version 1, set a minimum key bit, and configure auto-logout of idle users.</span></span>  <span data-ttu-id="640f8-106">Ez a cikk hello követelményei: az Azure-fiók ([ingyenes próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/)) és [SSH nyilvános és titkos kulcs fájlok](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="640f8-106">hello requirements for this article are: an Azure account ([get a free trial](https://azure.microsoft.com/pricing/free-trial/)) and [SSH public and private key files](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="quick-commands"></a><span data-ttu-id="640f8-107">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="640f8-107">Quick Commands</span></span>

<span data-ttu-id="640f8-108">Konfigurálása `/etc/ssh/sshd_config` a hello a következő beállításokat:</span><span class="sxs-lookup"><span data-stu-id="640f8-108">Configure `/etc/ssh/sshd_config` with hello following settings:</span></span>

### <a name="disable-password-logins"></a><span data-ttu-id="640f8-109">Letiltsa a jelszavas bejelentkezéseket</span><span class="sxs-lookup"><span data-stu-id="640f8-109">Disable password logins</span></span>

```bash
PasswordAuthentication no
```

### <a name="disable-login-by-hello-root-user"></a><span data-ttu-id="640f8-110">Tiltsa le a bejelentkezést hello gyökér szintű felhasználó által</span><span class="sxs-lookup"><span data-stu-id="640f8-110">Disable login by hello root user</span></span>

```bash
PermitRootLogin no
```

### <a name="allowed-groups-list"></a><span data-ttu-id="640f8-111">Engedélyezett csoportok listáját</span><span class="sxs-lookup"><span data-stu-id="640f8-111">Allowed groups list</span></span>

```bash
AllowGroups wheel
```

### <a name="allowed-users-list"></a><span data-ttu-id="640f8-112">Engedélyezett felhasználók listája</span><span class="sxs-lookup"><span data-stu-id="640f8-112">Allowed users list</span></span>

```bash
AllowUsers ahmet ralph
```

### <a name="disable-ssh-protocol-version-1"></a><span data-ttu-id="640f8-113">Tiltsa le az SSH protokoll 1-es verziójával</span><span class="sxs-lookup"><span data-stu-id="640f8-113">Disable SSH protocol version 1</span></span>

```bash
Protocol 2
```

### <a name="minimum-key-bits"></a><span data-ttu-id="640f8-114">Minimális kulcs bits</span><span class="sxs-lookup"><span data-stu-id="640f8-114">Minimum key bits</span></span>

```bash
ServerKeyBits 2048
```

### <a name="disconnect-idle-users"></a><span data-ttu-id="640f8-115">A tétlen felhasználók leválasztása</span><span class="sxs-lookup"><span data-stu-id="640f8-115">Disconnect idle users</span></span>

```bash
ClientAliveInterval 300
ClientAliveCountMax 0
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="640f8-116">Részletes bemutató</span><span class="sxs-lookup"><span data-stu-id="640f8-116">Detailed Walkthrough</span></span>

<span data-ttu-id="640f8-117">SSHD hello SSH kiszolgálót, amelyen a hello Linux virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="640f8-117">SSHD is hello SSH Server that runs on hello Linux VM.</span></span>  <span data-ttu-id="640f8-118">Az SSH egy ügyfél, amely egy a MacBook, Linux munkaállomás a rendszerhéj vagy a Bash futó Windows.</span><span class="sxs-lookup"><span data-stu-id="640f8-118">SSH is a client that runs from a shell on your MacBook, Linux workstation, or from a Bash on Windows.</span></span>  <span data-ttu-id="640f8-119">SSH egyben hello protokoll toosecure használja, és a munkaállomás és a Linux virtuális gép is így SSH (virtuális magánhálózat) VPN hello között hello kommunikáció titkosítására.</span><span class="sxs-lookup"><span data-stu-id="640f8-119">SSH is also hello protocol used toosecure and encrypt hello communication between your workstation and hello Linux VM making SSH also a VPN (Virtual Private Network).</span></span>

<span data-ttu-id="640f8-120">Ez a cikk a nagyon fontos tookeep egy bejelentkezési tooyour Linux virtuális gép nyissa meg a teljes útmutató hello esetén.</span><span class="sxs-lookup"><span data-stu-id="640f8-120">For this article, it is very important tookeep one login tooyour Linux VM open for hello entire walk-through.</span></span>  <span data-ttu-id="640f8-121">Miután az SSH-kapcsolat létrejött, marad munkamenetekkel mindaddig, amíg hello ablak nincs lezárva.</span><span class="sxs-lookup"><span data-stu-id="640f8-121">Once an SSH connection is established, it remains as an open session as long as hello window is not closed.</span></span>  <span data-ttu-id="640f8-122">Egy terminál jelentkezett be, hogy lehetővé teszi a módosításokat végzett toobe toohello SSHD szolgáltatás nélkül használhatatlanná tévő változást, ha éppen zárolva.</span><span class="sxs-lookup"><span data-stu-id="640f8-122">Having one terminal logged in, allows for changes toobe made toohello SSHD service without being locked out if a breaking change is made.</span></span>  <span data-ttu-id="640f8-123">Ön ártatlan tévedéssel a Linux virtuális gép hibás SSHD konfigurálása, ha Azure kínál hello képességét tooreset egy hibás SSHD-konfiguráció hello [Azure VM hozzáférési bővítmény](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="640f8-123">If you do get locked out of your Linux VM with a broken SSHD configuration, Azure offers hello ability tooreset a broken SSHD configuration with hello [Azure VM Access Extension](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="640f8-124">Ezért azt megnyitása két terminálok és a Linux virtuális gép SSH toohello mindkettő.</span><span class="sxs-lookup"><span data-stu-id="640f8-124">For this reason we open two terminals and SSH toohello Linux VM from both of them.</span></span>  <span data-ttu-id="640f8-125">A Microsoft hello első terminál toomake hello módosítások tooSSHDs konfigurációs fájlt használja, és indítsa újra hello SSHD szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="640f8-125">We use hello first terminal toomake hello changes tooSSHDs configuration file and restart hello SSHD service.</span></span>  <span data-ttu-id="640f8-126">Második terminál tootest hello azokat hello szolgáltatás újraindítása után módosítja használjuk.</span><span class="sxs-lookup"><span data-stu-id="640f8-126">We use hello second terminal tootest those changes once hello service is restarted.</span></span>  <span data-ttu-id="640f8-127">Mivel azt választotta, hogy letiltja SSH jelszavak és hagyatkoznia szigorúan SSH-kulcsok, ha az SSH-kulcsok nem helyesek, és bezárja hello kapcsolat toohello VM, hello VM lesz véglegesen zárolva van, és nem lesz képes toologin tooit toobe törölni, majd újból-beli.</span><span class="sxs-lookup"><span data-stu-id="640f8-127">Because we are disabling SSH passwords and relying strictly on SSH keys, if your SSH keys are not correct and you close hello connection toohello VM, hello VM will be permanently locked and no one will be able toologin tooit requiring it toobe deleted and recreated.</span></span>

## <a name="disable-password-logins"></a><span data-ttu-id="640f8-128">Letiltsa a jelszavas bejelentkezéseket</span><span class="sxs-lookup"><span data-stu-id="640f8-128">Disable password logins</span></span>

<span data-ttu-id="640f8-129">hello leggyorsabb módon toosecure Linux virtuális gép meg-e toodisable jelszavas bejelentkezéseket.</span><span class="sxs-lookup"><span data-stu-id="640f8-129">hello quickest way toosecure you Linux VM is toodisable password logins.</span></span>  <span data-ttu-id="640f8-130">Ha engedélyezve vannak a jelszavas bejelentkezéseket, bejárási hello webes azonnal megkezdődik toobrute kísérlet botok becslés hello jelszó kényszerítése a Linux virtuális gép SSH.</span><span class="sxs-lookup"><span data-stu-id="640f8-130">When password logins are enabled, bots crawling hello web will immediately start attempting toobrute force guess hello password for your Linux VM using SSH.</span></span>  <span data-ttu-id="640f8-131">Teljes letiltása a jelszavas bejelentkezéseket, lehetővé teszi, hogy hello SSH server tooignore összes jelszó bejelentkezési kísérletek.</span><span class="sxs-lookup"><span data-stu-id="640f8-131">Disabling password logins completely, enables hello SSH server tooignore all password login attempts.</span></span>

```bash
PasswordAuthentication no
```

## <a name="disable-login-by-hello-root-user"></a><span data-ttu-id="640f8-132">Tiltsa le a bejelentkezést hello gyökér szintű felhasználó által</span><span class="sxs-lookup"><span data-stu-id="640f8-132">Disable login by hello root user</span></span>

<span data-ttu-id="640f8-133">Következő Linux ajánlott eljárások hello `root` felhasználónak kell soha nem kell bejelentkeznie SSH vagy használatával `sudo su`.</span><span class="sxs-lookup"><span data-stu-id="640f8-133">Following Linux best practices, hello `root` user should never be logged into over SSH or using `sudo su`.</span></span>  <span data-ttu-id="640f8-134">Gyökér szintű engedélyek igénylő összes parancs mindig fusson a hello keresztül `sudo` parancsot, amely a jövőbeli naplózás összes műveletet naplózza.</span><span class="sxs-lookup"><span data-stu-id="640f8-134">All commands needing root level permissions should always be run through hello `sudo` command, which logs all actions for future auditing.</span></span>  <span data-ttu-id="640f8-135">Letiltása hello `root` SSH-kapcsolaton keresztül van bejelentkezve a felhasználó egy biztonsági ajánlott eljárások lépés, amely csak a jogosult felhasználók végezhetnek tooSSH biztosítja.</span><span class="sxs-lookup"><span data-stu-id="640f8-135">Disabling hello `root` user from logging in via SSH is a security best practices step that ensures only authorized users are allowed tooSSH.</span></span>

```bash
PermitRootLogin no
```

## <a name="allowed-groups-list"></a><span data-ttu-id="640f8-136">Engedélyezett csoportok listáját</span><span class="sxs-lookup"><span data-stu-id="640f8-136">Allowed groups list</span></span>

<span data-ttu-id="640f8-137">SSH megakadályozhatják a felhasználók és csoportok számára engedélyezett, vagy le van tiltva az ssh-naplózás egy módszert biztosít.</span><span class="sxs-lookup"><span data-stu-id="640f8-137">SSH offers a method of restricting users and group that are allowed or disallowed from logging in over SSH.</span></span>  <span data-ttu-id="640f8-138">Ez a funkció listák tooapprove használ, vagy a bejelentkezés megtagadása – meghatározott felhasználókhoz és csoportokhoz.</span><span class="sxs-lookup"><span data-stu-id="640f8-138">This feature uses lists tooapprove or deny specific users and groups from logging in.</span></span>  <span data-ttu-id="640f8-139">Hello kerék csoport toohello beállítás `AllowGroups` lista korlátozza a jóváhagyott bejelentkezések keresztül SSH toojust felhasználói fiókok hello kerék csoportba.</span><span class="sxs-lookup"><span data-stu-id="640f8-139">Setting hello wheel group toohello `AllowGroups` list restricts approved logins over SSH toojust user accounts that are in hello wheel group.</span></span>

```bash
AllowGroups wheel
```

## <a name="allowed-users-list"></a><span data-ttu-id="640f8-140">Engedélyezett felhasználók listája</span><span class="sxs-lookup"><span data-stu-id="640f8-140">Allowed users list</span></span>

<span data-ttu-id="640f8-141">Tooaccomplish hello azonos pontosabb úgy feladat, amely korlátozza, hogy SSH bejelentkezéshez toojust felhasználók `AllowGroups` van.</span><span class="sxs-lookup"><span data-stu-id="640f8-141">Restricting SSH logins toojust users is a more specific way tooaccomplish hello same task that `AllowGroups` is.</span></span>  

```bash
AllowUsers ahmet ralph
```

## <a name="disable-ssh-protocol-version-1"></a><span data-ttu-id="640f8-142">Tiltsa le az SSH protokoll 1-es verziójával</span><span class="sxs-lookup"><span data-stu-id="640f8-142">Disable SSH protocol version 1</span></span>

<span data-ttu-id="640f8-143">SSH protokoll verziója 1 nem biztonságos, és le kell tiltani.</span><span class="sxs-lookup"><span data-stu-id="640f8-143">SSH protocol version 1 is insecure and should be disabled.</span></span>  <span data-ttu-id="640f8-144">SSH protokoll 2-es verziójú hello aktuális verzióra, amely egy biztonságos módon tooSSH tooyour server kínál.</span><span class="sxs-lookup"><span data-stu-id="640f8-144">SSH protocol version 2 is hello current version that offers a secure way tooSSH tooyour server.</span></span>  <span data-ttu-id="640f8-145">1-es VERZIÓJÁVAL letiltása megtagadja a bármely SSH-ügyfél, amely tooestablish próbált kapcsolatot az 1-es verziójával hello SSH-kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="640f8-145">Disabling SSH version 1 denies any SSH clients that are attempting tooestablish a connection with hello SSH server using SSH version 1.</span></span>  <span data-ttu-id="640f8-146">Csak SSH-2-es kapcsolatok toonegotiate hello SSH-kiszolgálót a kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="640f8-146">Only SSH version 2 connections are allowed toonegotiate a connection with hello SSH server.</span></span>

```bash
Protocol 2
```

## <a name="minimum-key-bits"></a><span data-ttu-id="640f8-147">Minimális kulcs bits</span><span class="sxs-lookup"><span data-stu-id="640f8-147">Minimum key bits</span></span>

<span data-ttu-id="640f8-148">Ajánlott biztonsági eljárásokat követve jelszó SSH bejelentkezés le vannak tiltva, és csak az SSH-kulcsok használata engedélyezett toobe használt tooauthenticate hello SSH-kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="640f8-148">Following security best practices, password SSH logins are disabled and only SSH keys are allowed toobe used tooauthenticate with hello SSH server.</span></span>  <span data-ttu-id="640f8-149">E SSH-kulcsok különböző hosszúságú kulcsok mérése bitben használatával hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="640f8-149">These SSH keys can be created using different length keys, measured in bits.</span></span>  <span data-ttu-id="640f8-150">Ajánlott eljárásokat, hogy a 2048 bit hosszúságú kulcsokban hello minimális elfogadható kulcs erősségét állapotok.</span><span class="sxs-lookup"><span data-stu-id="640f8-150">Best practices states that keys of 2048 bits in length are hello minimum acceptable key strength.</span></span>  <span data-ttu-id="640f8-151">Kisebb, mint 2048 bites kulccsal elméletileg kell megszakadt.</span><span class="sxs-lookup"><span data-stu-id="640f8-151">Keys of less than 2048 bits could theoretically be broken.</span></span>  <span data-ttu-id="640f8-152">A beállítás hello `ServerKeyBits` túl`2048` lehetővé teszi, hogy a kapcsolatokat, 2048 bites vagy annál nagyobb kulcsokkal, és lehetővé a kapcsolódást kisebb, mint 2048 bit.</span><span class="sxs-lookup"><span data-stu-id="640f8-152">Setting hello `ServerKeyBits` too`2048` allows any connections using keys of 2048 bits or greater and deny connections of less than 2048 bits.</span></span>

```bash
ServerKeyBits 2048
```

## <a name="disconnect-idle-users"></a><span data-ttu-id="640f8-153">A tétlen felhasználók leválasztása</span><span class="sxs-lookup"><span data-stu-id="640f8-153">Disconnect idle users</span></span>

<span data-ttu-id="640f8-154">SSH rendelkezik hello képességét toodisconnect rendelkező felhasználók, amelyek több mint a másodpercben megadott ideje maradtak tétlen kapcsolatok megnyitása.</span><span class="sxs-lookup"><span data-stu-id="640f8-154">SSH has hello ability toodisconnect users that have open connections that have remained idle for more than a set period of seconds.</span></span>  <span data-ttu-id="640f8-155">A megnyitott munkamenetek tooonly hello Linux virtuális gép aktív korlátok hello elérhetővé tegyék ezen felhasználók tartása.</span><span class="sxs-lookup"><span data-stu-id="640f8-155">Keeping open sessions tooonly those users who are active limits hello exposure of hello Linux VM.</span></span>

```bash
ClientAliveInterval 300
ClientAliveCountMax 0
```

## <a name="restart-sshd"></a><span data-ttu-id="640f8-156">Indítsa újra a SSHD</span><span class="sxs-lookup"><span data-stu-id="640f8-156">Restart SSHD</span></span>

<span data-ttu-id="640f8-157">tooenable hello beállításai a `/etc/ssh/sshd_config` indítsa újra a hello SSHD folyamattal, amely újraindítja a hello SSH-kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="640f8-157">tooenable hello settings in `/etc/ssh/sshd_config` restart hello SSHD process which restarts hello SSH server.</span></span>  <span data-ttu-id="640f8-158">hello terminálablakot toorestart hello SSH-kiszolgálót használ hello SSH munkamenetekkel elvesztése nélkül nyitva marad.</span><span class="sxs-lookup"><span data-stu-id="640f8-158">hello terminal window you use toorestart hello SSH server remain open without losing hello open SSH session.</span></span>  <span data-ttu-id="640f8-159">tootest hello új SSH kiszolgálóbeállítások használjon egy második terminálablakot vagy -lapon.  Egy külön terminál tootest hello SSH-kapcsolat használata lehetővé teszi az toogo vissza és ellenőrizze további módosításokat toohello `/etc/ssh/sshd_config` hello első terminálban zárolta SSHD használhatatlanná tévő változást nélkül.</span><span class="sxs-lookup"><span data-stu-id="640f8-159">tootest hello new SSH server settings use a second terminal window or tab.  Using a separate terminal tootest hello SSH connection allows you toogo back and make additional changes toohello `/etc/ssh/sshd_config` in hello first terminal, without being locked out by a breaking SSHD change.</span></span>  

### <a name="on-redhat-centos-and-fedora"></a><span data-ttu-id="640f8-160">Redhat, Centos és Fedora</span><span class="sxs-lookup"><span data-stu-id="640f8-160">On Redhat, Centos and Fedora</span></span>

```bash
service sshd restart
```

### <a name="on-debian--ubuntu"></a><span data-ttu-id="640f8-161">A Debian és Ubuntu</span><span class="sxs-lookup"><span data-stu-id="640f8-161">On Debian & Ubuntu</span></span>

```bash
service ssh restart
```

## <a name="reset-sshd-using-azure-reset-access"></a><span data-ttu-id="640f8-162">Azure alaphelyzetbe állítása-hozzáférés SSHD alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="640f8-162">Reset SSHD using Azure reset-access</span></span>

<span data-ttu-id="640f8-163">Ha a legfrissebb toohello SSHD konfigurációjának módosítása a zárolt hello Azure Virtuálisgép-access-bővítmény tooreset hello SSHD hátsó toohello eredeti konfiguráció is használhatja.</span><span class="sxs-lookup"><span data-stu-id="640f8-163">If you are locked out from a breaking change toohello SSHD configuration you can use hello Azure VM access-extension tooreset hello SSHD configuration back toohello original configuration.</span></span>

<span data-ttu-id="640f8-164">Cserélje le a neveit saját.</span><span class="sxs-lookup"><span data-stu-id="640f8-164">Replace any example names with your own.</span></span>

```azurecli
azure vm reset-access \
--resource-group myResourceGroup \
--name myVM \
--reset-ssh
```

## <a name="install-fail2ban"></a><span data-ttu-id="640f8-165">Fail2ban telepítése</span><span class="sxs-lookup"><span data-stu-id="640f8-165">Install Fail2ban</span></span>

<span data-ttu-id="640f8-166">Erősen ajánlott tooinstall és a telepítési hello az nyílt forráskódú alkalmazás Fail2ban, mely blokkok ismétlődő kísérletek toologin tooyour Linux virtuális gép használata a találgatásos SSH-n keresztül.</span><span class="sxs-lookup"><span data-stu-id="640f8-166">It is strongly recommended tooinstall and setup hello open source app Fail2ban, which blocks repeated attempts toologin tooyour Linux VM over SSH using brute force.</span></span>  <span data-ttu-id="640f8-167">Ismétlődő Fail2ban naplók kísérletek toologin sikertelen SSH-n keresztül, és ezután hoz létre a tűzfalszabályok tooblock hello IP-címet, amely hello kísérletek származó vannak.</span><span class="sxs-lookup"><span data-stu-id="640f8-167">Fail2ban logs repeated failed attempts toologin over SSH and then creates firewall rules tooblock hello IP address that hello attempts are originating from.</span></span>

* [<span data-ttu-id="640f8-168">Fail2ban kezdőlap</span><span class="sxs-lookup"><span data-stu-id="640f8-168">Fail2ban homepage</span></span>](http://www.fail2ban.org/wiki/index.php/Main_Page)

## <a name="next-steps"></a><span data-ttu-id="640f8-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="640f8-169">Next Steps</span></span>

<span data-ttu-id="640f8-170">Most, hogy beállította, és a Linux virtuális gépén hello SSH-kiszolgálót zárolva vannak további biztonsági ajánlott eljárások követésével.</span><span class="sxs-lookup"><span data-stu-id="640f8-170">Now that you have configured and locked down hello SSH server on your Linux VM there are additional security best practices you can follow.</span></span>  

* [<span data-ttu-id="640f8-171">Kezelheti a felhasználókat, az SSH és az ellenőrzése vagy javítása lemezek Azure Linux virtuális gépek használatával hello VMAccess bővítmény</span><span class="sxs-lookup"><span data-stu-id="640f8-171">Manage users, SSH, and check or repair disks on Azure Linux VMs using hello VMAccess Extension</span></span>](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

* [<span data-ttu-id="640f8-172">A Linux virtuális gépet az Azure parancssori felület hello lemezzel titkosítása</span><span class="sxs-lookup"><span data-stu-id="640f8-172">Encrypt disks on a Linux VM using hello Azure CLI</span></span>](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

* [<span data-ttu-id="640f8-173">Hozzáférés és biztonság az Azure Resource Manager sablonokban</span><span class="sxs-lookup"><span data-stu-id="640f8-173">Access and security in Azure Resource Manager templates</span></span>](dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
