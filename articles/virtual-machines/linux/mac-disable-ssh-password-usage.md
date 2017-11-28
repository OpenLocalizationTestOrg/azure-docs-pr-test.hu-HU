---
title: "a Linux virtuális gép SSHD konfigurálásával aaaDisable SSH jelszavuk |} Microsoft Docs"
description: "A Linux virtuális Gépet az Azure-on biztonságos jelszavas bejelentkezéseket az SSH letiltásával."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: 
ms.assetid: 46137640-a7d2-40e5-a1e9-9effef7eb190
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/26/2016
ms.author: v-livech
ms.openlocfilehash: fb67b2f5b8b3bf2ba214858940b04f2ea9013fb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="disable-ssh-passwords-on-your-linux-vm-by-configuring-sshd"></a><span data-ttu-id="deeae-103">Tiltsa le a Linux virtuális gép SSH jelszavuk SSHD konfigurálásával</span><span class="sxs-lookup"><span data-stu-id="deeae-103">Disable SSH passwords on your Linux VM by configuring SSHD</span></span>
<span data-ttu-id="deeae-104">Ez a cikk foglalkozik, hogyan toolock hello bejelentkezési biztonsági a Linux virtuális gép le.</span><span class="sxs-lookup"><span data-stu-id="deeae-104">This article focuses on how toolock down hello login security of your Linux VM.</span></span>  <span data-ttu-id="deeae-105">Amint hello 22-es SSH-port már meg van nyitva toohello globális botok indítsa el a toologin próbált jelszavak találgatás által.</span><span class="sxs-lookup"><span data-stu-id="deeae-105">As soon as hello SSH port 22 is opened toohello world bots start trying toologin by guessing passwords.</span></span>  <span data-ttu-id="deeae-106">Milyen műveleteket végezzük el ebben a cikkben az SSH-n keresztül letiltsa a jelszavas bejelentkezéseket.</span><span class="sxs-lookup"><span data-stu-id="deeae-106">What we will do in this article is disable password logins over SSH.</span></span>  <span data-ttu-id="deeae-107">Hello képességét teljesen eltávolításával toouse jelszavak azt hello Linux virtuális gép védelme az ilyen típusú találgatásos támadás kényszerítése.</span><span class="sxs-lookup"><span data-stu-id="deeae-107">By completely removing hello ability toouse passwords we protect hello Linux VM from this type of brute force attack.</span></span>  <span data-ttu-id="deeae-108">hello bővült rendkívüli azt konfigurálja Linux SSHD tooonly lehetővé SSH nyilvános és titkos kulcsok, keresztül bejelentkezések messze hello legbiztonságosabb módja toologin tooLinux.</span><span class="sxs-lookup"><span data-stu-id="deeae-108">hello added bonus is we will configure Linux SSHD tooonly allow logins via SSH public & private keys, by far hello most secure way toologin tooLinux.</span></span>  <span data-ttu-id="deeae-109">hello lehetséges kombinációinak azt tooguess hello titkos kulcs jelentős, és ezért megnehezíti a még bothering tootry toobrute kényszerített SSH-kulcsok botok igényelnének.</span><span class="sxs-lookup"><span data-stu-id="deeae-109">hello possible combinations of it would require tooguess hello private key is immense and therefore discourages bots from even bothering tootry toobrute force SSH keys.</span></span>

## <a name="goals"></a><span data-ttu-id="deeae-110">Célok</span><span class="sxs-lookup"><span data-stu-id="deeae-110">Goals</span></span>
* <span data-ttu-id="deeae-111">SSHD toodisallow konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="deeae-111">Configure SSHD toodisallow:</span></span>
  * <span data-ttu-id="deeae-112">Jelszavas bejelentkezéseket</span><span class="sxs-lookup"><span data-stu-id="deeae-112">Password logins</span></span>
  * <span data-ttu-id="deeae-113">Legfelső szintű felhasználói bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="deeae-113">Root user login</span></span>
  * <span data-ttu-id="deeae-114">Kérdés-válasz hitelesítés</span><span class="sxs-lookup"><span data-stu-id="deeae-114">Challenge-response authentication</span></span>
* <span data-ttu-id="deeae-115">SSHD tooallow konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="deeae-115">Configure SSHD tooallow:</span></span>
  * <span data-ttu-id="deeae-116">csak SSH kulcs bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="deeae-116">only SSH key logins</span></span>
* <span data-ttu-id="deeae-117">Indítsa újra a SSHD, miközben továbbra is jelentkezik</span><span class="sxs-lookup"><span data-stu-id="deeae-117">Restart SSHD while still logged in</span></span>
* <span data-ttu-id="deeae-118">Hello új SSHD tesztkonfiguráció</span><span class="sxs-lookup"><span data-stu-id="deeae-118">Test hello new SSHD configuration</span></span>

## <a name="introduction"></a><span data-ttu-id="deeae-119">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="deeae-119">Introduction</span></span>
[<span data-ttu-id="deeae-120">SSH definiálva</span><span class="sxs-lookup"><span data-stu-id="deeae-120">SSH defined</span></span>](https://en.wikipedia.org/wiki/Secure_Shell)

<span data-ttu-id="deeae-121">SSHD hello SSH kiszolgálót, amelyen a hello Linux virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="deeae-121">SSHD is hello SSH Server that runs on hello Linux VM.</span></span>  <span data-ttu-id="deeae-122">Az SSH egy egy rendszerhéjból a munkaállomáson MacBook vagy Linux rendszerű futtató ügyfelet.</span><span class="sxs-lookup"><span data-stu-id="deeae-122">SSH is a client that runs from a shell on your MacBook or Linux workstation.</span></span>  <span data-ttu-id="deeae-123">SSH egyben hello protokoll toosecure használja, és a munkaállomás és a Linux virtuális gép hello között hello kommunikáció titkosítására.</span><span class="sxs-lookup"><span data-stu-id="deeae-123">SSH is also hello protocol used toosecure and encrypt hello communication between your workstation and hello Linux VM.</span></span>

<span data-ttu-id="deeae-124">Ez a cikk a nagyon fontos tookeep Linux virtuális gép teljes hello nyitva bízná egy bejelentkezési tooyour esetén.</span><span class="sxs-lookup"><span data-stu-id="deeae-124">For this article it is very important tookeep one login tooyour Linux VM open for hello entire walk through.</span></span>  <span data-ttu-id="deeae-125">Ezen okból program megnyitja két terminálok és a Linux virtuális gép SSH toohello két őket.</span><span class="sxs-lookup"><span data-stu-id="deeae-125">For this reason we will open two terminals and SSH toohello Linux VM from both of them.</span></span>  <span data-ttu-id="deeae-126">A Microsoft hello első terminál toomake hello módosítások tooSSHDs konfigurációs fájlt használja, és hello SSHD szolgáltatás újraindításához.</span><span class="sxs-lookup"><span data-stu-id="deeae-126">We will use hello first terminal toomake hello changes tooSSHDs configuration file and restart hello SSHD service.</span></span>  <span data-ttu-id="deeae-127">Második terminál tootest hello azokat hello szolgáltatás újraindítása után módosítja használjuk.</span><span class="sxs-lookup"><span data-stu-id="deeae-127">We will use hello second terminal tootest those changes once hello service is restarted.</span></span>  <span data-ttu-id="deeae-128">Mivel azt választotta, hogy letiltja SSH jelszavak és hagyatkoznia szigorúan SSH-kulcsok, ha az SSH-kulcsok nem helyesek, és bezárja hello kapcsolat toohello VM, hello VM lesz véglegesen zárolva van, és nem lesz képes toologin tooit toobe törölni, majd újból-beli.</span><span class="sxs-lookup"><span data-stu-id="deeae-128">Because we are disabling SSH passwords and relying strictly on SSH keys, if your SSH keys are not correct and you close hello connection toohello VM, hello VM will be permanently locked and no one will be able toologin tooit requiring it toobe deleted and recreated.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="deeae-129">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="deeae-129">Prerequisites</span></span>
* [<span data-ttu-id="deeae-130">SSH-kulcsok létrehozása Linux és Mac rendszerben Linux virtuális gépek Azure-ban</span><span class="sxs-lookup"><span data-stu-id="deeae-130">Create SSH keys on Linux and Mac for Linux VMs in Azure</span></span>](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* <span data-ttu-id="deeae-131">Azure-fiók</span><span class="sxs-lookup"><span data-stu-id="deeae-131">Azure account</span></span>
  * [<span data-ttu-id="deeae-132">ingyenes próba-előfizetés</span><span class="sxs-lookup"><span data-stu-id="deeae-132">free trial signup</span></span>](https://azure.microsoft.com/pricing/free-trial/)
  * [<span data-ttu-id="deeae-133">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="deeae-133">Azure portal</span></span>](http://portal.azure.com)
* <span data-ttu-id="deeae-134">Azure-on futó Linux virtuális gép</span><span class="sxs-lookup"><span data-stu-id="deeae-134">Linux VM running on azure</span></span>
* <span data-ttu-id="deeae-135">SSH nyilvános és titkos kulcsból álló kulcspárt a`~/.ssh/`</span><span class="sxs-lookup"><span data-stu-id="deeae-135">SSH public & private key pair in `~/.ssh/`</span></span>
* <span data-ttu-id="deeae-136">A nyilvános SSH-kulcs `~/.ssh/authorized_keys` a hello Linux virtuális gép</span><span class="sxs-lookup"><span data-stu-id="deeae-136">SSH public key in `~/.ssh/authorized_keys` on hello Linux VM</span></span>
* <span data-ttu-id="deeae-137">A virtuális gép hello Sudo jogok</span><span class="sxs-lookup"><span data-stu-id="deeae-137">Sudo rights on hello VM</span></span>
* <span data-ttu-id="deeae-138">Nyissa meg a 22-es portot</span><span class="sxs-lookup"><span data-stu-id="deeae-138">Port 22 open</span></span>

## <a name="quick-commands"></a><span data-ttu-id="deeae-139">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="deeae-139">Quick Commands</span></span>
<span data-ttu-id="deeae-140">*Fűszerezett Linux rendszergazdák számára, akik csak hello TLDR verzió Kezdje itt.  Összes többi felhasználója számára, hogy hello próbál részletes magyarázata és lépésein végighaladva átugorja ezt a szakaszt.*</span><span class="sxs-lookup"><span data-stu-id="deeae-140">*Seasoned Linux Admins who just want hello TLDR version start here.  For everyone else that wants hello detailed explanation and walk through skip this section.*</span></span>

```bash
sudo vim /etc/ssh/sshd_config
```

<span data-ttu-id="deeae-141">Hello konfigurációs fájl szerkesztése az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="deeae-141">Edit hello config file as follows:</span></span>

```sh
# Change PasswordAuthentication toothis:
PasswordAuthentication no

# Change PubkeyAuthentication toothis:
PubkeyAuthentication yes

# Change PermitRootLogin toothis:
PermitRootLogin no

# Change ChallengeResponseAuthentication toothis:
ChallengeResponseAuthentication no
```

<span data-ttu-id="deeae-142">Indítsa újra a hello SSHD szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="deeae-142">Restart hello SSHD service.</span></span> <span data-ttu-id="deeae-143">A Debian-alapú disztribúciókkal:</span><span class="sxs-lookup"><span data-stu-id="deeae-143">On Debian-based distros:</span></span>

```bash
sudo service ssh restart
```

<span data-ttu-id="deeae-144">A Red Hat-alapú disztribúciókkal:</span><span class="sxs-lookup"><span data-stu-id="deeae-144">On Red Hat-based distros:</span></span>

```bash
sudo service sshd restart
```

## <a name="detailed-walk-through"></a><span data-ttu-id="deeae-145">Részletes lépésein végighaladva</span><span class="sxs-lookup"><span data-stu-id="deeae-145">Detailed Walk Through</span></span>
<span data-ttu-id="deeae-146">Bejelentkezési toohello Linux virtuális gép terminál 1 (T1).</span><span class="sxs-lookup"><span data-stu-id="deeae-146">Login toohello Linux VM on terminal 1 (T1).</span></span>  <span data-ttu-id="deeae-147">Bejelentkezési toohello Linux virtuális gép terminál 2 (T2).</span><span class="sxs-lookup"><span data-stu-id="deeae-147">Login toohello Linux VM on terminal 2 (T2).</span></span>

<span data-ttu-id="deeae-148">A T2 fogjuk tooedit hello SSHD konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="deeae-148">On T2 we are going tooedit hello SSHD configuration file.</span></span>  

```bash
sudo vim /etc/ssh/sshd_config
```

<span data-ttu-id="deeae-149">Itt rendszer csupán hello beállítások toodisable jelszavak szerkesztése és SSH-kulcs bejelentkezések engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="deeae-149">From here we will edit just hello settings toodisable passwords and enable SSH key logins.</span></span>  <span data-ttu-id="deeae-150">Nincsenek a kutatás és toomake Linux & SSH biztonságban kell módosítani kell a fájl számos olyan beállítást.</span><span class="sxs-lookup"><span data-stu-id="deeae-150">There are many settings in this file that you should research and change toomake Linux & SSH as secure as you need.</span></span>

#### <a name="disable-password-logins"></a><span data-ttu-id="deeae-151">Letiltsa a jelszavas bejelentkezéseket</span><span class="sxs-lookup"><span data-stu-id="deeae-151">Disable Password logins</span></span>

```sh
# Change PasswordAuthentication toothis:
PasswordAuthentication no
```

#### <a name="enable-public-key-authentication"></a><span data-ttu-id="deeae-152">Nyilvános kulcsos hitelesítés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="deeae-152">Enable Public Key Authentication</span></span>

```sh
# Change PubkeyAuthentication toothis:
PubkeyAuthentication yes
```

#### <a name="disable-root-login"></a><span data-ttu-id="deeae-153">Tiltsa le a legfelső szintű bejelentkezést</span><span class="sxs-lookup"><span data-stu-id="deeae-153">Disable Root Login</span></span>

```sh
# Change PermitRootLogin toothis:
PermitRootLogin no
```

#### <a name="disable-challenge-response-authentication"></a><span data-ttu-id="deeae-154">Kérdés-válasz hitelesítés letiltása</span><span class="sxs-lookup"><span data-stu-id="deeae-154">Disable Challenge-response Authentication</span></span>
```sh
# Change ChallengeResponseAuthentication toothis:
ChallengeResponseAuthentication no
```

### <a name="restart-sshd"></a><span data-ttu-id="deeae-155">Indítsa újra a SSHD</span><span class="sxs-lookup"><span data-stu-id="deeae-155">Restart SSHD</span></span>
<span data-ttu-id="deeae-156">Hello T1 rendszerhéjból győződjön meg arról, hogy továbbra is jelentkezett be.</span><span class="sxs-lookup"><span data-stu-id="deeae-156">From hello T1 shell verify that you are still logged in.</span></span>  <span data-ttu-id="deeae-157">Ez elengedhetetlen, így azt nem ártatlan tévedéssel a virtuális Gépet, ha az SSH-kulcsok helytelenek, mivel a jelszavak most le vannak tiltva.</span><span class="sxs-lookup"><span data-stu-id="deeae-157">This is critical so you do not get locked out of your VM if your SSH keys are not correct since passwords are now disabled.</span></span>  <span data-ttu-id="deeae-158">Ha minden olyan beállítás helytelen a Linux virtuális gépén, használhatja a T1 toofix sshd_config, továbbra is vannak, naplózva lesznek, és SSH fog életben hello kapcsolat során hello SSHD szolgáltatás újraindítása.</span><span class="sxs-lookup"><span data-stu-id="deeae-158">If any setting are incorrect on your Linux VM you can use T1 toofix sshd_config as you will still be logged in and SSH will keep hello connection alive during hello SSHD service restart.</span></span>

<span data-ttu-id="deeae-159">A T2 futtatása:</span><span class="sxs-lookup"><span data-stu-id="deeae-159">From T2 run:</span></span>

##### <a name="on-hello-debian-family"></a><span data-ttu-id="deeae-160">A hello Debian termékcsalád</span><span class="sxs-lookup"><span data-stu-id="deeae-160">On hello Debian Family</span></span>
```bash
sudo service ssh restart
```

##### <a name="on-hello-redhat-family"></a><span data-ttu-id="deeae-161">A hello RedHat termékcsalád</span><span class="sxs-lookup"><span data-stu-id="deeae-161">On hello RedHat Family</span></span>
```bash
sudo service sshd restart
```

<span data-ttu-id="deeae-162">A virtuális gép védelme akkor találgatásos jelszó-bejelentkezési kísérletek jelszavak most le vannak tiltva.</span><span class="sxs-lookup"><span data-stu-id="deeae-162">Passwords are now disabled on your VM protecting it from brute force password login attempts.</span></span>  <span data-ttu-id="deeae-163">Csak SSH kulccsal rendelkező engedélyezett fogja tudni toologin gyorsabb és sokkal biztonságosabb.</span><span class="sxs-lookup"><span data-stu-id="deeae-163">With only SSH Keys allowed you will be able toologin faster and much more secure.</span></span>

