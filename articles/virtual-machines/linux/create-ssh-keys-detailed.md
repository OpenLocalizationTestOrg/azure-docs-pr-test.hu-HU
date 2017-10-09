---
title: "aaaDetailed lépéseket toocreate SSH kulcs pár Linux virtuális gépek Azure-ban |} Microsoft Docs"
description: "Ismerje meg, további lépéseket toocreate az SSH nyilvános és titkos kulcsból álló kulcspárt Linux virtuális gépek Azure-ban a különböző használatából adódó adott tanúsítványokkal együtt."
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 6/28/2017
ms.author: danlep
ms.openlocfilehash: 9ac52ef4dc87e73b9c07ccc323adc955829e2014
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="detailed-walk-through-toocreate-an-ssh-key-pair-and-additional-certificates-for-a-linux-vm-in-azure"></a><span data-ttu-id="3ca5e-103">Részletes lépésein végighaladva toocreate egy SSH-kulcspárral és további tanúsítványokat a Linux virtuális gépek Azure-ban</span><span class="sxs-lookup"><span data-stu-id="3ca5e-103">Detailed walk through toocreate an SSH key pair and additional certificates for a Linux VM in Azure</span></span>
<span data-ttu-id="3ca5e-104">Egy SSH kulcspár akkor hozhat létre virtuális gépek Azure-alapértelmezett toousing SSH-kulcsok a hitelesítéshez, így nem kell a jelszavak toolog hello.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-104">With an SSH key pair, you can create Virtual Machines on Azure that default toousing SSH keys for authentication, eliminating hello need for passwords toolog in.</span></span> <span data-ttu-id="3ca5e-105">Jelszavak kitalál is lehet, és nyissa meg a virtuális gépek toorelentless találgatásos kísérletek tooguess be a jelszót.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-105">Passwords can be guessed, and open your VMs up toorelentless brute force attempts tooguess your password.</span></span> <span data-ttu-id="3ca5e-106">Hello Azure parancssori felületen vagy a Resource Manager sablonnal létrehozott virtuális gépek lehetnek a nyilvános SSH-kulcs hello üzembe helyezés, a feladás egy vagy több központi telepítési konfigurációs lépés letiltása a jelszavas bejelentkezéseket az SSH eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-106">VMs created with hello Azure CLI or Resource Manager templates can include your SSH public key as part of hello deployment, removing a post deployment configuration step of disabling password logins for SSH.</span></span> <span data-ttu-id="3ca5e-107">A cikkben részletes lépéseket és további példákat a tanúsítványok előállítása, például Linux virtuális gépekhez való használatra.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-107">This article provides detailed steps and additional examples of generating certificates, such as for use with Linux virtual machines.</span></span> <span data-ttu-id="3ca5e-108">Ha azt szeretné, hogy tooquickly létrehozása és egy SSH-kulcspárral használatára, lásd: [hogyan toocreate SSH-nyilvános és titkos kulcsot a Linux virtuális gépek Azure-ban párosítsa](mac-create-ssh-keys.md).</span><span class="sxs-lookup"><span data-stu-id="3ca5e-108">If you want tooquickly create and use an SSH key pair, see [How toocreate an SSH public and private key pair for Linux VMs in Azure](mac-create-ssh-keys.md).</span></span>

## <a name="understanding-ssh-keys"></a><span data-ttu-id="3ca5e-109">Az SSH-kulcsok bemutatása</span><span class="sxs-lookup"><span data-stu-id="3ca5e-109">Understanding SSH keys</span></span>

<span data-ttu-id="3ca5e-110">Az SSH nyilvános és titkos kulcsok használata a hello legegyszerűbb módja toolog tooyour Linux-kiszolgálókon.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-110">Using SSH public and private keys is hello easiest way toolog in tooyour Linux servers.</span></span> <span data-ttu-id="3ca5e-111">[Nyilvános kulcsú](https://en.wikipedia.org/wiki/Public-key_cryptography) egy sokkal biztonságosabb módon toolog a tooyour Linux vagy BSD virtuális gép az Azure-ban biztosítja a jelszavak, amely lehet találgatásos módszeren alapuló sokkal könnyebben.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-111">[Public-key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography) provides a much more secure way toolog in tooyour Linux or BSD VM in Azure than passwords, which can be brute-forced far more easily.</span></span>

<span data-ttu-id="3ca5e-112">A nyilvános kulcs bárkivel megosztható, azonban csak Ön (vagy a helyi biztonsági infrastruktúra) rendelkezik a titkos kulccsal.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-112">Your public key can be shared with anyone; but only you (or your local security infrastructure) possess your private key.</span></span>  <span data-ttu-id="3ca5e-113">hello titkos SSH-kulcsot rendelkeznie kell egy [rendkívül biztonságos jelszó](https://www.xkcd.com/936/) (forrás:[xkcd.com](https://xkcd.com)) toosafeguard azt.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-113">hello SSH private key should have a [very secure password](https://www.xkcd.com/936/) (source:[xkcd.com](https://xkcd.com)) toosafeguard it.</span></span>  <span data-ttu-id="3ca5e-114">Ez a jelszó nem csak tooaccess hello titkos SSH-kulcsfájl és **nem** hello felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-114">This password is just tooaccess hello private SSH key file and **is not** hello user account password.</span></span>  <span data-ttu-id="3ca5e-115">Ha hozzáad egy jelszó tooyour SSH-kulcs, hello 128 bites AES titkosítással, titkos kulcs, az titkosítja, úgy, hogy hello titkos kulcs nélkül hello jelszó toodecrypt használhatatlan azt.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-115">When you add a password tooyour SSH key, it encrypts hello private key using 128-bit AES, so that hello private key is useless without hello password toodecrypt it.</span></span>  <span data-ttu-id="3ca5e-116">Ha egy támadó ellopott a titkos kulcsot, és hogy a kulcs nem volt meg a jelszót, a lesznek képesek toouse, hogy titkos kulcs toolog hello megfelelő nyilvános kulccsal rendelkező tooany-kiszolgálókon.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-116">If an attacker stole your private key and that key did not have a password, they would be able toouse that private key toolog in tooany servers that have hello corresponding public key.</span></span>  <span data-ttu-id="3ca5e-117">Ha egy titkos kulcsot jelszó véd, a kulcsot nem használhatja a támadó, így ez további biztonsági réteget nyújt az Azure-beli infrastruktúra számára.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-117">If a private key is password protected it cannot be used by that attacker, providing an additional layer of security for your infrastructure on Azure.</span></span>

<span data-ttu-id="3ca5e-118">Ez a cikk az SSH protokoll verziója 2 RSA nyilvános és titkos kulcsot tartalmazó fájlt pár (is hivatkozott tooas "ssh-rsa" kulcsok), amely az Azure Resource Manager telepítésekhez javasolt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-118">This article creates an SSH protocol version 2 RSA public and private key file pair (also referred tooas "ssh-rsa" keys), which are recommended for deployments with Azure Resource Manager.</span></span> <span data-ttu-id="3ca5e-119">*ssh-rsa* kulcsok szükségesek hello [portal](https://portal.azure.com) klasszikus és Resource Manager üzembe helyezések esetében.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-119">*ssh-rsa* keys are required on hello [portal](https://portal.azure.com) for both classic and Resource Manager deployments.</span></span>

## <a name="ssh-keys-use-and-benefits"></a><span data-ttu-id="3ca5e-120">Az SSH-kulcsok használata és a használat előnyei</span><span class="sxs-lookup"><span data-stu-id="3ca5e-120">SSH keys use and benefits</span></span>

<span data-ttu-id="3ca5e-121">Az Azure-nak, legalább 2048 bites SSH protokoll 2-es RSA formátumú nyilvános és titkos kulcsok; hello nyilvános kulcsot tartalmazó fájlt rendelkezik hello `.pub` tárolóformátummal.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-121">Azure requires at least 2048-bit, SSH protocol version 2 RSA format public and private keys; hello public key file has hello `.pub` container format.</span></span> <span data-ttu-id="3ca5e-122">a kulcsok használata toocreate hello `ssh-keygen`, amely kérdések sorát, és ezután ír egy titkos kulcsot és egy megfelelő nyilvános kulcsot.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-122">toocreate hello keys use `ssh-keygen`, which asks a series of questions and then writes a private key and a matching public key.</span></span> <span data-ttu-id="3ca5e-123">Egy Azure virtuális gép létrehozásakor Azure másolatok hello nyilvános kulcs toohello `~/.ssh/authorized_keys` hello VM mappájába.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-123">When an Azure VM is created, Azure copies hello public key toohello `~/.ssh/authorized_keys` folder on hello VM.</span></span> <span data-ttu-id="3ca5e-124">Az SSH-kulcsok `~/.ssh/authorized_keys` használt toochallenge hello ügyfél toomatch hello megfelelő titkos kulcsnak SSH bejelentkezés kapcsolaton vannak.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-124">SSH keys in `~/.ssh/authorized_keys` are used toochallenge hello client toomatch hello corresponding private key on an SSH login connection.</span></span>  <span data-ttu-id="3ca5e-125">Egy Azure Linux virtuális gép létrehozásakor SSH-kulcsok használata a hitelesítéshez, Azure konfigurálása hello SSHD server toonot engedélyezése a jelszavas bejelentkezéseket, csak az SSH-kulcsok.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-125">When an Azure Linux VM is created using SSH keys for authentication, Azure configures hello SSHD server toonot allow password logins, only SSH keys.</span></span>  <span data-ttu-id="3ca5e-126">Ezért SSH-kulccsal rendelkező Azure Linux virtuális gépek létrehozásával segíthet biztonságos hello virtuális gép üzembe helyezéséhez és mentett hello Tipikus telepítés utáni konfigurációs lépés letiltása hello jelszavak **sshd_config** fájlt.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-126">Therefore, by creating Azure Linux VMs with SSH keys, you can help secure hello VM deployment and save yourself hello typical post-deployment configuration step of disabling passwords in hello **sshd_config** file.</span></span>

## <a name="using-ssh-keygen"></a><span data-ttu-id="3ca5e-127">Az ssh-keygen használata</span><span class="sxs-lookup"><span data-stu-id="3ca5e-127">Using ssh-keygen</span></span>

<span data-ttu-id="3ca5e-128">Ezzel a paranccsal létrejön egy jelszóval védett (titkosított) SSH-kulcspárral 2048 bites RSA használatával, és azt megjegyzésként tooeasily képes azonosítani.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-128">This command creates a password secured (encrypted) SSH key pair using 2048-bit RSA and it is commented tooeasily identify it.</span></span>  

<span data-ttu-id="3ca5e-129">SSH kulcs van-e őrizni, hello alapértelmezés szerint `~/.ssh` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-129">SSH keys are by default kept in hello `~/.ssh` directory.</span></span>  <span data-ttu-id="3ca5e-130">Ha nem rendelkezik egy `~/.ssh` könyvtár, hello `ssh-keygen` parancs létrehozza azt, hello a megfelelő engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-130">If you do not have a `~/.ssh` directory, hello `ssh-keygen` command creates it for you with hello correct permissions.</span></span>

```bash
ssh-keygen \
    -t rsa \
    -b 2048 \
    -C "azureuser@myserver" \
    -f ~/.ssh/id_rsa \
    -N mypassword
```

<span data-ttu-id="3ca5e-131">*A parancs ismertetése*</span><span class="sxs-lookup"><span data-stu-id="3ca5e-131">*Command explained*</span></span>

<span data-ttu-id="3ca5e-132">`ssh-keygen`hello használt program toocreate hello kulcsok =</span><span class="sxs-lookup"><span data-stu-id="3ca5e-132">`ssh-keygen` = hello program used toocreate hello keys</span></span>

<span data-ttu-id="3ca5e-133">`-t rsa`= a kulcs toocreate, amely hello RSA formátum [wikipedia] típusú[végén parens](`https://en.wikipedia.org/wiki/RSA_(cryptosystem) `) 
 `-b 2048` bits hello kulcs =</span><span class="sxs-lookup"><span data-stu-id="3ca5e-133">`-t rsa` = type of key toocreate which is hello RSA format [wikipedia][parens at end](`https://en.wikipedia.org/wiki/RSA_(cryptosystem) `)
`-b 2048` = bits of hello key</span></span>

<span data-ttu-id="3ca5e-134">`-C "azureuser@myserver"`= hello nyilvános kulcsot tartalmazó fájlt tooeasily hozzáfűzött toohello végét azonosíthassák megjegyzést.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-134">`-C "azureuser@myserver"` = a comment appended toohello end of hello public key file tooeasily identify it.</span></span>  <span data-ttu-id="3ca5e-135">Általában egy e-mailt használnak hello megjegyzésként, de használhat bármit a legjobban az infrastruktúra.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-135">Normally an email is used as hello comment but you can use whatever works best for your infrastructure.</span></span>

## <a name="classic-deploy-using-asm"></a><span data-ttu-id="3ca5e-136">Klasszikus üzembe helyezés az `asm` használatával</span><span class="sxs-lookup"><span data-stu-id="3ca5e-136">Classic deploy using `asm`</span></span>

<span data-ttu-id="3ca5e-137">Hello klasszikus üzembe helyezési modellel használata (`asm` hello CLI módban), egy SSH-RSA nyilvános kulccsal, vagy egy RFC4716 formázott kulcs pem-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-137">If you are using hello classic deployment model (`asm` mode in hello CLI), you can use an SSH-RSA public key or an RFC4716 formatted key in a pem container.</span></span>  <span data-ttu-id="3ca5e-138">hello SSH-RSA nyilvános kulcs, ez a cikk használja a korábban létrehozott elemek `ssh-keygen`.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-138">hello SSH-RSA public key is what was created earlier in this article using `ssh-keygen`.</span></span>

<span data-ttu-id="3ca5e-139">egy RFC4716 toocreate egy meglévő nyilvános SSH-kulccsal a kulcs formátuma:</span><span class="sxs-lookup"><span data-stu-id="3ca5e-139">toocreate a RFC4716 formatted key from an existing SSH public key:</span></span>

```bash
ssh-keygen \
-f ~/.ssh/id_rsa.pub \
-e \
-m RFC4716 > ~/.ssh/id_ssh2.pem
```

## <a name="example-of-ssh-keygen"></a><span data-ttu-id="3ca5e-140">Ssh-keygen – példa</span><span class="sxs-lookup"><span data-stu-id="3ca5e-140">Example of ssh-keygen</span></span>

```bash
ssh-keygen -t rsa -b 2048 -C "azureuser@myserver"
Generating public/private rsa key pair.
Enter file in which toosave hello key (/home/azureuser/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/azureuser/.ssh/id_rsa.
Your public key has been saved in /home/azureuser/.ssh/id_rsa.pub.
hello key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 azureuser@myserver
hello keys randomart image is:
+--[ RSA 2048]----+
|        o o. .   |
|      E. = .o    |
|      ..o...     |
|     . o....     |
|      o S =      |
|     . + O       |
|      + = =      |
|       o +       |
|        .        |
+-----------------+
```

<span data-ttu-id="3ca5e-141">Mentett kulcsfájlok:</span><span class="sxs-lookup"><span data-stu-id="3ca5e-141">Saved key files:</span></span>

`Enter file in which toosave hello key (/home/azureuser/.ssh/id_rsa): ~/.ssh/id_rsa`

<span data-ttu-id="3ca5e-142">Ez a cikk hello kulcspár neve.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-142">hello key pair name for this article.</span></span>  <span data-ttu-id="3ca5e-143">Nevű kulcspár **id_rsa** hello alapértelmezett és az egyes eszközök várt hello **id_rsa** titkos kulcsfájl nevét, egy érdemes.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-143">Having a key pair named **id_rsa** is hello default and some tools might expect hello **id_rsa** private key file name so having one is a good idea.</span></span> <span data-ttu-id="3ca5e-144">hello directory `~/.ssh/` hello SSH-kulcspár és hello SSH konfigurációs fájl alapértelmezett helye.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-144">hello directory `~/.ssh/` is hello default location for SSH key pairs and hello SSH config file.</span></span>  <span data-ttu-id="3ca5e-145">Ha nincs megadva a teljes elérési úttal `ssh-keygen` hoz létre hello kulcsok hello aktuális munkakönyvtárban – nem hello alapértelmezett `~/.ssh`.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-145">If not specified with a full path, `ssh-keygen` creates hello keys in hello current working directory, not hello default `~/.ssh`.</span></span>

<span data-ttu-id="3ca5e-146">Hello listáját `~/.ssh` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-146">A listing of hello `~/.ssh` directory.</span></span>

```bash
ls -al ~/.ssh
-rw------- 1 azureuser staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 azureuser staff   410 Aug 25 18:04 id_rsa.pub
```

<span data-ttu-id="3ca5e-147">Kulcs jelszava:</span><span class="sxs-lookup"><span data-stu-id="3ca5e-147">Key Password:</span></span>

`Enter passphrase (empty for no passphrase):`

<span data-ttu-id="3ca5e-148">`ssh-keygen`néven hivatkozik tooa jelszó hello titkos kulcsot tartalmazó fájlt a "jelszó szükséges."</span><span class="sxs-lookup"><span data-stu-id="3ca5e-148">`ssh-keygen` refers tooa password for hello private key file as "a passphrase."</span></span>  <span data-ttu-id="3ca5e-149">Az *erősen* ajánlott tooadd jelszó tooyour titkos kulccsal.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-149">It is *strongly* recommended tooadd a password tooyour private key.</span></span> <span data-ttu-id="3ca5e-150">Jelszó védelmet nyújtó hello kulcsfájlt, nélkül bárki, aki hello fájl használható toolog tooany Server hello megfelelő nyilvános kulcs.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-150">Without a password protecting hello key file, anyone with hello file can use it toolog in tooany server that has hello corresponding public key.</span></span> <span data-ttu-id="3ca5e-151">Jelszóval (jelszó) további védelmet nyújt, ha valaki képes toogain hozzáférés tooyour titkos kulcs fájlja, adott ideje toochange hello kulcsok használt tooauthenticate meg.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-151">Adding a password (passphrase) offers more protection in case someone is able toogain access tooyour private key file, given you time toochange hello keys used tooauthenticate you.</span></span>

## <a name="using-ssh-agent-toostore-your-private-key-password"></a><span data-ttu-id="3ca5e-152">Ssh-agent toostore a titkos kulcs jelszavának használata</span><span class="sxs-lookup"><span data-stu-id="3ca5e-152">Using ssh-agent toostore your private key password</span></span>

<span data-ttu-id="3ca5e-153">Írja be a titkos kulcsfájl jelszavát minden SSH-bejelentkezéskor tooavoid használható `ssh-agent` toocache a titkos kulcsfájl jelszavát.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-153">tooavoid typing your private key file password with every SSH login, you can use `ssh-agent` toocache your private key file password.</span></span> <span data-ttu-id="3ca5e-154">Ha egy Mac használ, az OSX Kulcskarikájához hello biztonságosan tárolja a hello titkos kulcsok jelszavát indításakor `ssh-agent`.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-154">If you are using a Mac, hello OSX Keychain securely stores hello private key passwords when you invoke `ssh-agent`.</span></span>

<span data-ttu-id="3ca5e-155">Győződjön meg arról, ssh-agent használ, és ssh-tooinform hello SSH rendszer hozzáadása hello kulcs fájlokkal kapcsolatban, hogy hello jelszava nem lesz szükség a toobe interaktív módon használt.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-155">Verify and use ssh-agent and ssh-add tooinform hello SSH system about hello key files so that hello passphrase will not need toobe used interactively.</span></span>

```bash
eval "$(ssh-agent -s)"
```

<span data-ttu-id="3ca5e-156">Adja hozzá a titkos kulcs hello túl`ssh-agent` hello paranccsal `ssh-add`.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-156">Now add hello private key too`ssh-agent` using hello command `ssh-add`.</span></span>

```bash
ssh-add ~/.ssh/id_rsa
```

<span data-ttu-id="3ca5e-157">hello titkos kulcs jelszava most tárolódik `ssh-agent`.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-157">hello private key password is now stored in `ssh-agent`.</span></span>

## <a name="using-ssh-copy-id-toocopy-hello-key-tooan-existing-vm"></a><span data-ttu-id="3ca5e-158">Használatával `ssh-copy-id` toocopy hello kulcs tooan meglévő virtuális gép</span><span class="sxs-lookup"><span data-stu-id="3ca5e-158">Using `ssh-copy-id` toocopy hello key tooan existing VM</span></span>
<span data-ttu-id="3ca5e-159">Ha már létrehozott egy virtuális gép telepítése hello új SSH nyilvános kulcs tooyour Linux virtuális gép együtt:</span><span class="sxs-lookup"><span data-stu-id="3ca5e-159">If you have already created a VM you can install hello new SSH public key tooyour Linux VM with:</span></span>

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub ahmet@myserver
```

## <a name="create-and-configure-an-ssh-config-file"></a><span data-ttu-id="3ca5e-160">SSH konfigurációs fájl létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3ca5e-160">Create and configure an SSH config file</span></span>

<span data-ttu-id="3ca5e-161">Egy ajánlott bevált gyakorlat toocreate, és konfigurálja az `~/.ssh/config` fájl toospeed mentése bejelentkezések és az SSH-ügyfél módot optimalizálási.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-161">It is a recommended best practice toocreate and configure an `~/.ssh/config` file toospeed up log ins and for optimizing your SSH client behavior.</span></span>

<span data-ttu-id="3ca5e-162">a következő példa hello a szabványos konfiguráció látható.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-162">hello following example shows a standard configuration.</span></span>

### <a name="create-hello-file"></a><span data-ttu-id="3ca5e-163">Hello fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="3ca5e-163">Create hello file</span></span>

```bash
touch ~/.ssh/config
```

### <a name="edit-hello-file-tooadd-hello-new-ssh-configuration"></a><span data-ttu-id="3ca5e-164">Hello fájl tooadd hello új SSH-konfiguráció szerkesztése:</span><span class="sxs-lookup"><span data-stu-id="3ca5e-164">Edit hello file tooadd hello new SSH configuration:</span></span>

```bash
vim ~/.ssh/config
```

### <a name="example-sshconfig-file"></a><span data-ttu-id="3ca5e-165">`~/.ssh/config` példafájl:</span><span class="sxs-lookup"><span data-stu-id="3ca5e-165">Example `~/.ssh/config` file:</span></span>

```bash
# Azure Keys
Host fedora22
  Hostname 102.160.203.241
  User ahmet
# ./Azure Keys
# Default Settings
Host *
  PubkeyAuthentication=yes
  IdentitiesOnly=yes
  ServerAliveInterval=60
  ServerAliveCountMax=30
  ControlMaster auto
  ControlPath ~/.ssh/SSHConnections/ssh-%r@%h:%p
  ControlPersist 4h
  IdentityFile ~/.ssh/id_rsa
```

<span data-ttu-id="3ca5e-166">Ez SSH konfigurációs lehetőséget biztosít, részek az egyes kiszolgáló tooenable minden toohave saját dedikált kulcspárral.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-166">This SSH config gives you sections for each server tooenable each toohave its own dedicated key pair.</span></span> <span data-ttu-id="3ca5e-167">alapértelmezett beállítások hello (`Host *`) vannak, amelyek nem felelnek meg bármelyik hello adott gazdagép feljebb hello konfigurációs fájlban állomásokra.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-167">hello default settings (`Host *`) are for any hosts that do not match any of hello specific hosts higher up in hello config file.</span></span>

### <a name="config-file-explained"></a><span data-ttu-id="3ca5e-168">A konfigurációs fájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="3ca5e-168">Config file explained</span></span>

<span data-ttu-id="3ca5e-169">`Host`= hello hívott állomás hello terminál hello neve.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-169">`Host` = hello name of hello host being called on hello terminal.</span></span>  <span data-ttu-id="3ca5e-170">`ssh fedora22`közli `SSH` toouse hello értékek hello beállítások blokkban feliratú `Host fedora22` Megjegyzés: állomás, amely logikus a használatra, és nem felel meg a kiszolgáló tényleges állomásnevét hello bármilyen olyan címke lehet.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-170">`ssh fedora22` tells `SSH` toouse hello values in hello settings block labeled `Host fedora22`  NOTE: Host can be any label that is logical for your usage and does not represent hello actual hostname of any server.</span></span>

<span data-ttu-id="3ca5e-171">`Hostname 102.160.203.241`= hello IP-cím vagy hello server férnek hozzá a DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-171">`Hostname 102.160.203.241` = hello IP address or DNS name for hello server being accessed.</span></span>

<span data-ttu-id="3ca5e-172">`User ahmet`= hello távoli felhasználói fiók toouse toohello server bejelentkezéskor.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-172">`User ahmet` = hello remote user account toouse when logging in toohello server.</span></span>

<span data-ttu-id="3ca5e-173">`PubKeyAuthentication yes`= közli az SSH a kívánt toouse egy SSH-kulcs toolog.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-173">`PubKeyAuthentication yes` = tells SSH you want toouse an SSH key toolog in.</span></span>

<span data-ttu-id="3ca5e-174">`IdentityFile /home/ahmet/.ssh/id_id_rsa`= hello titkos SSH-kulcsot és a megfelelő nyilvános kulcs toouse hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-174">`IdentityFile /home/ahmet/.ssh/id_id_rsa` = hello SSH private key and corresponding public key toouse for authentication.</span></span>

## <a name="ssh-into-linux-without-a-password"></a><span data-ttu-id="3ca5e-175">SSH-ból Linuxba jelszó nélkül</span><span class="sxs-lookup"><span data-stu-id="3ca5e-175">SSH into Linux without a password</span></span>

<span data-ttu-id="3ca5e-176">Most, hogy az SSH-kulcspárral és konfigurált SSH konfigurációs fájl, a Linux virtuális gép tooyour képes toolog biztos gyorsan és biztonságosan.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-176">Now that you have an SSH key pair and a configured SSH config file, you are able toolog in tooyour Linux VM quickly and securely.</span></span> <span data-ttu-id="3ca5e-177">hello első alkalommal jelentkezik be egy SSH-kulcs hello parancssorok tooa-kiszolgáló, az adott kulcsfájl hello jelszavát.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-177">hello first time you log in tooa server using an SSH key hello command prompts you for hello passphrase for that key file.</span></span>

```bash
ssh fedora22
```

### <a name="command-explained"></a><span data-ttu-id="3ca5e-178">A parancs ismertetése</span><span class="sxs-lookup"><span data-stu-id="3ca5e-178">Command explained</span></span>

<span data-ttu-id="3ca5e-179">Ha `ssh fedora22` végrehajtása SSH először megkeresi és betölti a beállításokat a hello `Host fedora22` letiltása, majd betölti az összes hello hello utolsó blokk beállításait fennmaradó `Host *`.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-179">When `ssh fedora22` is executed SSH first locates and loads any settings from hello `Host fedora22` block, and then loads all hello remaining settings from hello last block, `Host *`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3ca5e-180">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3ca5e-180">Next Steps</span></span>

<span data-ttu-id="3ca5e-181">Ezután mentése toocreate Azure Linux virtuális gépeket használ hello új nyilvános SSH-kulcs.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-181">Next up is toocreate Azure Linux VMs using hello new SSH public key.</span></span>  <span data-ttu-id="3ca5e-182">Nyilvános SSH-kulcs hello bejelentkezésként használatával létrehozott Azure virtuális gépek, amelyek jobban biztonságos, mint a virtuális gépek hello alapértelmezett bejelentkezési módszer, jelszavak létre.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-182">Azure VMs that are created with an SSH public key as hello login are better secured than VMs created with hello default login method, passwords.</span></span>  <span data-ttu-id="3ca5e-183">Az SSH-kulcsokkal létrehozott Azure virtuális gépek alapértelmezés szerint letiltott jelszavakkal vannak konfigurálva, ami megakadályozza a találgatásos támadásokat.</span><span class="sxs-lookup"><span data-stu-id="3ca5e-183">Azure VMs created using SSH keys are by default configured with passwords disabled, avoiding brute-forced guessing attempts.</span></span>

* [<span data-ttu-id="3ca5e-184">Biztonságos Linux virtuális gép létrehozása Azure-sablon alapján</span><span class="sxs-lookup"><span data-stu-id="3ca5e-184">Create a secure Linux VM using an Azure template</span></span>](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="3ca5e-185">Biztonságos Linux virtuális gép létrehozása hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="3ca5e-185">Create a secure Linux VM using hello Azure portal</span></span>](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="3ca5e-186">Biztonságos Linux virtuális gép létrehozása Azure CLI hello</span><span class="sxs-lookup"><span data-stu-id="3ca5e-186">Create a secure Linux VM using hello Azure CLI</span></span>](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
