---
title: "aaaCreate SSH kulcs pár Linux virtuális gépek Azure-on |} Microsoft Docs"
description: "Nyilvános és titkos SSH-kulcspárok biztonságos létrehozása Azure-beli Linux rendszerű virtuális gépekhez."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: 
ms.assetid: 34ae9482-da3e-4b2d-9d0d-9d672aa42498
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/08/2017
ms.author: rasquill
ms.openlocfilehash: c4c7cec77c9b48295f2a28c8179b30a4dc38a555
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-ssh-public-and-private-key-pair-for-linux-vms"></a><span data-ttu-id="3db6f-103">SSH nyilvános és titkos kulcspárok létrehozása Linux rendszerű virtuális gépekhez</span><span class="sxs-lookup"><span data-stu-id="3db6f-103">Create an SSH public and private key pair for Linux VMs</span></span>

<span data-ttu-id="3db6f-104">Ez a cikk bemutatja, hogyan toogenerate az SSH protokoll 2-es RSA nyilvános és titkos kulcs fájl pár toouse Linux virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="3db6f-104">This article shows you how toogenerate an SSH protocol version 2 RSA public and private key file pair toouse with Linux VMs.</span></span>  <span data-ttu-id="3db6f-105">Egy SSH kulcspár akkor hozhat létre virtuális gépek Azure-alapértelmezett toousing SSH-kulcsok a hitelesítéshez, így nem kell a jelszavak toolog hello.</span><span class="sxs-lookup"><span data-stu-id="3db6f-105">With an SSH key pair, you can create Virtual Machines on Azure that default toousing SSH keys for authentication, eliminating hello need for passwords toolog in.</span></span>  <span data-ttu-id="3db6f-106">Jelszavak kitalál is lehet, és nyissa meg a virtuális gépek toorelentless találgatásos kísérletek tooguess be a jelszót.</span><span class="sxs-lookup"><span data-stu-id="3db6f-106">Passwords can be guessed, and open your VMs up toorelentless brute force attempts tooguess your password.</span></span> <span data-ttu-id="3db6f-107">Virtuális gépek Azure-sablonok vagy hello `azure-cli` hello üzembe helyezés, a feladás egy vagy több központi telepítési konfigurációs lépés letiltása a jelszavas bejelentkezéseket az SSH eltávolítása a nyilvános SSH-kulcs tartalmazhatnak.</span><span class="sxs-lookup"><span data-stu-id="3db6f-107">VMs created with Azure Templates or hello `azure-cli` can include your SSH public key as part of hello deployment, removing a post deployment configuration step of disabling password logins for SSH.</span></span>

## <a name="quick-commands"></a><span data-ttu-id="3db6f-108">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="3db6f-108">Quick Commands</span></span>

<span data-ttu-id="3db6f-109">Futtassa a parancsokat követően a rendszerhéjakba, cseréje hello példák segítségével a saját választhat hello.</span><span class="sxs-lookup"><span data-stu-id="3db6f-109">Run hello following commands from a Bash shell, replacing hello examples with your own choices.</span></span>

<span data-ttu-id="3db6f-110">Az SSH-ra épülő nyilvános kulcsfájl alapértelmezés szerint a következő helyen jön létre: `~/.ssh/id_rsa.pub`.</span><span class="sxs-lookup"><span data-stu-id="3db6f-110">Your SSH public key file is by default created at `~/.ssh/id_rsa.pub`.</span></span> <span data-ttu-id="3db6f-111">Amikor a rendszer kéri, a következő parancs hello segítségével, a titkos kulcsot a "jelszó" toosecure kell létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="3db6f-111">When prompted using hello following command, you should create a "passphrase" toosecure your private key.</span></span> <span data-ttu-id="3db6f-112">(hello jelszót a, a jelszót használt tooencrypt a titkos kulcsot.)</span><span class="sxs-lookup"><span data-stu-id="3db6f-112">(hello passphrase is a password used tooencrypt your private key.)</span></span>

```bash
ssh-keygen -t rsa -b 2048 
```

<span data-ttu-id="3db6f-113">Adja hozzá az újonnan létrehozott hello kulcs túl`ssh-agent`:</span><span class="sxs-lookup"><span data-stu-id="3db6f-113">Add hello newly created key too`ssh-agent`:</span></span>

```bash
ssh-add ~/.ssh/id_rsa
```

> [!IMPORTANT] 
> <span data-ttu-id="3db6f-114">hello fenti parancsok szinte minden terjesztéseket Linux operációs rendszeren, de nem feltétlenül működik-tárolókban, hello környezet így nagyon korlátozott módon.</span><span class="sxs-lookup"><span data-stu-id="3db6f-114">hello above commands work on Linux operating systems of almost all distributions, but do not necessarily work in containers, as hello environment can be radically constrained.</span></span> 

## <a name="detailed-walkthrough"></a><span data-ttu-id="3db6f-115">Részletes bemutató</span><span class="sxs-lookup"><span data-stu-id="3db6f-115">Detailed Walkthrough</span></span>

<span data-ttu-id="3db6f-116">Az SSH nyilvános és titkos kulcsok használata a hello legegyszerűbb módja toolog tooyour Linux-kiszolgálókon.</span><span class="sxs-lookup"><span data-stu-id="3db6f-116">Using SSH public and private keys is hello easiest way toolog in tooyour Linux servers.</span></span> <span data-ttu-id="3db6f-117">[Nyilvános kulcsú](https://en.wikipedia.org/wiki/Public-key_cryptography) egy sokkal biztonságosabb módon toolog a tooyour Linux vagy BSD virtuális gép az Azure-ban biztosítja a jelszavak, amely lehet találgatásos módszeren alapuló sokkal könnyebben.</span><span class="sxs-lookup"><span data-stu-id="3db6f-117">[Public-key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography) provides a much more secure way toolog in tooyour Linux or BSD VM in Azure than passwords, which can be brute-forced far more easily.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3db6f-118">A nyilvános kulcs bárkivel megosztható, azonban csak Ön (vagy a helyi biztonsági infrastruktúra) rendelkezik a titkos kulccsal.</span><span class="sxs-lookup"><span data-stu-id="3db6f-118">Your public key can be shared with anyone; but only you (or your local security infrastructure) possess your private key.</span></span>  <span data-ttu-id="3db6f-119">hello titkos SSH-kulcsot rendelkeznie kell egy [rendkívül biztonságos jelszó](https://www.xkcd.com/936/) (forrás:[xkcd.com](https://xkcd.com)) toosafeguard azt.</span><span class="sxs-lookup"><span data-stu-id="3db6f-119">hello SSH private key should have a [very secure password](https://www.xkcd.com/936/) (source:[xkcd.com](https://xkcd.com)) toosafeguard it.</span></span>  <span data-ttu-id="3db6f-120">Ez a jelszó nem csak tooaccess hello titkos SSH-kulcs és **nem** hello felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="3db6f-120">This password is just tooaccess hello private SSH key and **is not** hello user account password.</span></span>  <span data-ttu-id="3db6f-121">Ha hozzáad egy jelszó tooyour SSH-kulcs, hello 128 bites AES titkosítással, titkos kulcs, az titkosítja, úgy, hogy hello titkos kulcs nélkül hello jelszó toodecrypt használhatatlan azt.</span><span class="sxs-lookup"><span data-stu-id="3db6f-121">When you add a password tooyour SSH key, it encrypts hello private key using 128-bit AES, so that hello private key is useless without hello password toodecrypt it.</span></span>  <span data-ttu-id="3db6f-122">Ha egy támadó ellopott a titkos kulcsot, és hogy a kulcs nem volt meg a jelszót, a lesznek képesek toouse, hogy titkos kulcs toolog hello megfelelő nyilvános kulccsal rendelkező tooany-kiszolgálókon.</span><span class="sxs-lookup"><span data-stu-id="3db6f-122">If an attacker stole your private key and that key did not have a password, they would be able toouse that private key toolog in tooany servers that have hello corresponding public key.</span></span>  <span data-ttu-id="3db6f-123">Ha egy titkos kulcsot jelszó véd, a kulcsot nem használhatja a támadó, így ez további biztonsági réteget nyújt az Azure-beli infrastruktúra számára.</span><span class="sxs-lookup"><span data-stu-id="3db6f-123">If a private key is password protected it cannot be used by that attacker, providing an additional layer of security for your infrastructure on Azure.</span></span>

<span data-ttu-id="3db6f-124">Ez a cikk hoz létre SSH protokoll verziója 2 RSA nyilvános és titkos kulcs, amelyek központi erőforrás-kezelő hello használata ajánlott.</span><span class="sxs-lookup"><span data-stu-id="3db6f-124">This article creates SSH protocol version 2 RSA public and private key files, which are recommended for deployments on hello Resource Manager.</span></span>  <span data-ttu-id="3db6f-125">*ssh-rsa* kulcsok szükségesek hello [portal](https://portal.azure.com) klasszikus és Resource Manager üzembe helyezések esetében.</span><span class="sxs-lookup"><span data-stu-id="3db6f-125">*ssh-rsa* keys are required on hello [portal](https://portal.azure.com) for both classic and Resource Manager deployments.</span></span>

## <a name="disable-ssh-passwords-by-using-ssh-keys"></a><span data-ttu-id="3db6f-126">SSH-jelszavak letiltása SSH-kulcsokkal</span><span class="sxs-lookup"><span data-stu-id="3db6f-126">Disable SSH passwords by using SSH keys</span></span>

<span data-ttu-id="3db6f-127">Az Azure legalább 2048 bites ssh-rsa formátumú nyilvános és titkos kulcsokat követel meg.</span><span class="sxs-lookup"><span data-stu-id="3db6f-127">Azure requires at least 2048-bit, ssh-rsa format public and private keys.</span></span> <span data-ttu-id="3db6f-128">a kulcsok használata toocreate hello `ssh-keygen`, amely kérdések sorát, és ezután ír egy titkos kulcsot és egy megfelelő nyilvános kulcsot.</span><span class="sxs-lookup"><span data-stu-id="3db6f-128">toocreate hello keys use `ssh-keygen`, which asks a series of questions and then writes a private key and a matching public key.</span></span> <span data-ttu-id="3db6f-129">Egy Azure virtuális gép létrehozásakor a hello nyilvános kulcs túl másolódik`~/.ssh/authorized_keys`.</span><span class="sxs-lookup"><span data-stu-id="3db6f-129">When an Azure VM is created, hello public key is copied too`~/.ssh/authorized_keys`.</span></span>  <span data-ttu-id="3db6f-130">Az SSH-kulcsok `~/.ssh/authorized_keys` használt toochallenge hello ügyfél toomatch hello megfelelő titkos kulcsnak SSH bejelentkezés kapcsolaton vannak.</span><span class="sxs-lookup"><span data-stu-id="3db6f-130">SSH keys in `~/.ssh/authorized_keys` are used toochallenge hello client toomatch hello corresponding private key on an SSH login connection.</span></span>  <span data-ttu-id="3db6f-131">Egy Azure Linux virtuális gép létrehozásakor SSH-kulcsok használata a hitelesítéshez, Azure konfigurálása hello SSHD server toonot engedélyezése a jelszavas bejelentkezéseket, csak az SSH-kulcsok.</span><span class="sxs-lookup"><span data-stu-id="3db6f-131">When an Azure Linux VM is created using SSH keys for authentication, Azure configures hello SSHD server toonot allow password logins, only SSH keys.</span></span>  <span data-ttu-id="3db6f-132">Ezért az SSH-kulcsok Azure Linux virtuális gépek létrehozásával is segítségével biztonságos hello virtuális gép üzembe helyezéséhez és takarítson meg hello Tipikus telepítés utáni konfigurációs lépés letiltása jelszavak hello sshd_config konfigurációs fájlban.</span><span class="sxs-lookup"><span data-stu-id="3db6f-132">Therefore, by creating Azure Linux VMs with SSH keys, you can help secure hello VM deployment and save yourself hello typical post-deployment configuration step of disabling passwords in hello sshd_config config file.</span></span>

## <a name="using-ssh-keygen"></a><span data-ttu-id="3db6f-133">Az ssh-keygen használata</span><span class="sxs-lookup"><span data-stu-id="3db6f-133">Using ssh-keygen</span></span>

<span data-ttu-id="3db6f-134">Ezzel a paranccsal létrejön egy jelszóval védett (titkosított) SSH-kulcspárral 2048 bites RSA használatával, és azt megjegyzésként tooeasily képes azonosítani.</span><span class="sxs-lookup"><span data-stu-id="3db6f-134">This command creates a password secured (encrypted) SSH key pair using 2048-bit RSA and it is commented tooeasily identify it.</span></span>  

<span data-ttu-id="3db6f-135">SSH kulcs van-e őrizni, hello alapértelmezés szerint `~/.ssh` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="3db6f-135">SSH keys are by default kept in hello `~/.ssh` directory.</span></span>  <span data-ttu-id="3db6f-136">Ha nem rendelkezik egy `~/.ssh` könyvtár, hello `ssh-keygen` parancs létrehozza azt, hello a megfelelő engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="3db6f-136">If you do not have a `~/.ssh` directory, hello `ssh-keygen` command creates it for you with hello correct permissions.</span></span>

```bash
ssh-keygen \
-t rsa \
-b 2048 
```

<span data-ttu-id="3db6f-137">*A parancs ismertetése*</span><span class="sxs-lookup"><span data-stu-id="3db6f-137">*Command explained*</span></span>

<span data-ttu-id="3db6f-138">`ssh-keygen`hello használt program toocreate hello kulcsok =</span><span class="sxs-lookup"><span data-stu-id="3db6f-138">`ssh-keygen` = hello program used toocreate hello keys</span></span>

<span data-ttu-id="3db6f-139">`-t rsa`kulcs toocreate, amely hello RSA formátum típusú = [wikipedia](https://en.wikipedia.org/wiki/RSA_(cryptosystem)</span><span class="sxs-lookup"><span data-stu-id="3db6f-139">`-t rsa` = type of key toocreate which is hello RSA format [wikipedia](https://en.wikipedia.org/wiki/RSA_(cryptosystem)</span></span>

<span data-ttu-id="3db6f-140">`-b 2048`bits hello kulcs =</span><span class="sxs-lookup"><span data-stu-id="3db6f-140">`-b 2048` = bits of hello key</span></span>


## <a name="classic-portal-and-x509-certs"></a><span data-ttu-id="3db6f-141">A klasszikus portál és az X.509-tanúsítványok</span><span class="sxs-lookup"><span data-stu-id="3db6f-141">Classic portal and X.509 certs</span></span>

<span data-ttu-id="3db6f-142">Ha használ hello Azure [klasszikus portál](https://manage.windowsazure.com/), X.509 Tanúsítványos igényel a hello SSH-kulcsok.</span><span class="sxs-lookup"><span data-stu-id="3db6f-142">If you are using hello Azure [classic portal](https://manage.windowsazure.com/), it requires X.509 certs for hello SSH keys.</span></span>  <span data-ttu-id="3db6f-143">Más típusú nyilvános SSH-kulcsok nem engedélyezettek, az X.509-tanúsítványokat *kell* használnia.</span><span class="sxs-lookup"><span data-stu-id="3db6f-143">No other types of SSH public keys are allowed, they *must* be X.509 certs.</span></span>

<span data-ttu-id="3db6f-144">egy X.509 tanúsítvány számára a meglévő titkos SSH-RSA-kulcsot a toocreate:</span><span class="sxs-lookup"><span data-stu-id="3db6f-144">toocreate an X.509 cert from your existing SSH-RSA private key:</span></span>

```bash
openssl req -x509 \
-key ~/.ssh/id_rsa \
-nodes \
-days 365 \
-newkey rsa:2048 \
-out ~/.ssh/id_rsa.pem
```

## <a name="classic-deploy-using-asm"></a><span data-ttu-id="3db6f-145">Klasszikus üzembe helyezés az `asm` használatával</span><span class="sxs-lookup"><span data-stu-id="3db6f-145">Classic deploy using `asm`</span></span>

<span data-ttu-id="3db6f-146">Hello klasszikus használata modell rendszerbe állítása (Azure szolgáltatásfelügyelet CLI `asm`), egy SSH-RSA nyilvános kulccsal, vagy egy RFC4716 formázott kulcsban egy **.pem** tároló.</span><span class="sxs-lookup"><span data-stu-id="3db6f-146">If you are using hello classic deploy model (Azure service management CLI `asm`), you can use an SSH-RSA public key or an RFC4716 formatted key in a **.pem** container.</span></span>  <span data-ttu-id="3db6f-147">hello SSH-RSA nyilvános kulcs, ez a cikk használja a korábban létrehozott elemek `ssh-keygen`.</span><span class="sxs-lookup"><span data-stu-id="3db6f-147">hello SSH-RSA public key is what was created earlier in this article using `ssh-keygen`.</span></span>

<span data-ttu-id="3db6f-148">egy RFC4716 toocreate egy meglévő nyilvános SSH-kulccsal a kulcs formátuma:</span><span class="sxs-lookup"><span data-stu-id="3db6f-148">toocreate a RFC4716 formatted key from an existing SSH public key:</span></span>

```bash
ssh-keygen \
-f ~/.ssh/id_rsa.pub \
-e \
-m RFC4716 > ~/.ssh/id_ssh2.pem
```

## <a name="example-of-ssh-keygen"></a><span data-ttu-id="3db6f-149">Ssh-keygen – példa</span><span class="sxs-lookup"><span data-stu-id="3db6f-149">Example of ssh-keygen</span></span>

```bash
ssh-keygen -t rsa -b 2048 -C "ahmet@myserver"
Generating public/private rsa key pair.
Enter file in which toosave hello key (/home/ahmet/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/ahmet/.ssh/id_rsa.
Your public key has been saved in /home/ahmet/.ssh/id_rsa.pub.
hello key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 ahmet@myserver
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

<span data-ttu-id="3db6f-150">Mentett kulcsfájlok:</span><span class="sxs-lookup"><span data-stu-id="3db6f-150">Saved key files:</span></span>

`Enter file in which toosave hello key (/home/ahmet/.ssh/id_rsa): ~/.ssh/id_rsa`

<span data-ttu-id="3db6f-151">Ez a cikk hello kulcspár neve.</span><span class="sxs-lookup"><span data-stu-id="3db6f-151">hello key pair name for this article.</span></span>  <span data-ttu-id="3db6f-152">Nevű kulcspár **id_rsa** hello alapértelmezett és az egyes eszközök várt hello **id_rsa** titkos kulcsfájl nevét, egy érdemes.</span><span class="sxs-lookup"><span data-stu-id="3db6f-152">Having a key pair named **id_rsa** is hello default and some tools might expect hello **id_rsa** private key file name so having one is a good idea.</span></span> <span data-ttu-id="3db6f-153">hello directory `~/.ssh/` hello SSH-kulcspár és hello SSH konfigurációs fájl alapértelmezett helye.</span><span class="sxs-lookup"><span data-stu-id="3db6f-153">hello directory `~/.ssh/` is hello default location for SSH key pairs and hello SSH config file.</span></span>  <span data-ttu-id="3db6f-154">Ha nincs megadva a teljes elérési úttal `ssh-keygen` lesz az aktuális munkakönyvtárban hello hello kulcsok létrehozása, nem az alapértelmezett hello `~/.ssh`.</span><span class="sxs-lookup"><span data-stu-id="3db6f-154">If not specified with a full path, `ssh-keygen` will create hello keys in hello current working directory, not hello default `~/.ssh`.</span></span>

<span data-ttu-id="3db6f-155">Hello listáját `~/.ssh` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="3db6f-155">A listing of hello `~/.ssh` directory.</span></span>

```bash
ls -al ~/.ssh
-rw------- 1 ahmet staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 ahmet staff   410 Aug 25 18:04 rsa.pub
```

<span data-ttu-id="3db6f-156">Kulcs jelszava:</span><span class="sxs-lookup"><span data-stu-id="3db6f-156">Key Password:</span></span>

`Enter passphrase (empty for no passphrase):`

<span data-ttu-id="3db6f-157">`ssh-keygen`néven hivatkozik tooa használt jelszó tooencrypt hello titkos kulcs "egy hozzáférési kódot."</span><span class="sxs-lookup"><span data-stu-id="3db6f-157">`ssh-keygen` refers tooa password used tooencrypt hello private key as "a passphrase."</span></span>  <span data-ttu-id="3db6f-158">Az *erősen* tooadd jelszót tooyour kulcspárok ajánlott.</span><span class="sxs-lookup"><span data-stu-id="3db6f-158">It is *strongly* recommended tooadd a passphrase tooyour key pairs.</span></span> <span data-ttu-id="3db6f-159">A hozzáférési kód védelmet nyújtó hello titkos kulcs nélküli bárki, aki hello kulcsfájl használható toolog hello megfelelő nyilvános kulccsal rendelkező tooany kiszolgálónak.</span><span class="sxs-lookup"><span data-stu-id="3db6f-159">Without a passphrase protecting hello private key, anyone with hello key file can use it toolog in tooany server that has hello corresponding public key.</span></span> <span data-ttu-id="3db6f-160">Hozzáadása egy hozzáférési kódot további védelmet nyújt, ha valaki képes toogain hozzáférés tooyour titkos kulcs fájlja, felkínálva idő toochange hello kulcsok használt tooauthenticate meg.</span><span class="sxs-lookup"><span data-stu-id="3db6f-160">Adding a passphrase offers more protection in case someone is able toogain access tooyour private key file, giving you time toochange hello keys used tooauthenticate you.</span></span>

## <a name="using-ssh-agent-toostore-your-private-key-password"></a><span data-ttu-id="3db6f-161">Ssh-agent toostore a titkos kulcs jelszavának használata</span><span class="sxs-lookup"><span data-stu-id="3db6f-161">Using ssh-agent toostore your private key password</span></span>

<span data-ttu-id="3db6f-162">Írja be a titkos kulcs tooavoid fájl jelszót minden SSH-bejelentkezéskor, használhatja a `ssh-agent` toocache a titkos kulcsfájl jelszavát.</span><span class="sxs-lookup"><span data-stu-id="3db6f-162">tooavoid typing your private key file passphrase with every SSH login, you can use `ssh-agent` toocache your private key file password.</span></span> <span data-ttu-id="3db6f-163">Ha egy Mac használ, az OSX Kulcskarikájához hello biztonságosan tárolja a hello titkos kulcsok jelszavát indításakor `ssh-agent`.</span><span class="sxs-lookup"><span data-stu-id="3db6f-163">If you are using a Mac, hello OSX Keychain securely stores hello private key passwords when you invoke `ssh-agent`.</span></span>

<span data-ttu-id="3db6f-164">Győződjön meg arról, és használjon `ssh-agent` és `ssh-add` tooinform hello SSH rendszer hello kulcs fájlokkal kapcsolatban, hogy hello jelszava nem lesz szükség a toobe interaktív módon használt.</span><span class="sxs-lookup"><span data-stu-id="3db6f-164">Verify and use `ssh-agent` and `ssh-add` tooinform hello SSH system about hello key files so that hello passphrase will not need toobe used interactively.</span></span>

```bash
eval "$(ssh-agent -s)"
```

<span data-ttu-id="3db6f-165">Adja hozzá a titkos kulcs hello túl`ssh-agent` hello paranccsal `ssh-add`.</span><span class="sxs-lookup"><span data-stu-id="3db6f-165">Now add hello private key too`ssh-agent` using hello command `ssh-add`.</span></span>

```bash
ssh-add ~/.ssh/id_rsa
```

<span data-ttu-id="3db6f-166">hello titkos kulcs jelszava most tárolódik `ssh-agent`.</span><span class="sxs-lookup"><span data-stu-id="3db6f-166">hello private key password is now stored in `ssh-agent`.</span></span>

## <a name="using-ssh-copy-id-tooinstall-hello-new-key"></a><span data-ttu-id="3db6f-167">Használatával `ssh-copy-id` tooinstall hello új kulcs</span><span class="sxs-lookup"><span data-stu-id="3db6f-167">Using `ssh-copy-id` tooinstall hello new key</span></span>
<span data-ttu-id="3db6f-168">Ha már létrehozott egy virtuális gép telepítése hello új SSH nyilvános kulcs tooyour Linux virtuális gép a hello a következő parancsot, hello VM felhasználónév és a hello kiszolgálócímet cseréje a saját értékeit:</span><span class="sxs-lookup"><span data-stu-id="3db6f-168">If you have already created a VM you can install hello new SSH public key tooyour Linux VM with hello following command, replacing hello VM username and hello server address with your own values:</span></span>

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub ahmet@myserver
```

## <a name="create-and-configure-an-ssh-config-file"></a><span data-ttu-id="3db6f-169">SSH konfigurációs fájl létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3db6f-169">Create and configure an SSH config file</span></span>

<span data-ttu-id="3db6f-170">A bevált gyakorlat toocreate, és konfigurálja az `~/.ssh/config` fájl toospeed mentése bejelentkezések és az SSH-ügyfél módot optimalizálási.</span><span class="sxs-lookup"><span data-stu-id="3db6f-170">It is a best practice toocreate and configure an `~/.ssh/config` file toospeed up log ins and for optimizing your SSH client behavior.</span></span>

<span data-ttu-id="3db6f-171">a következő példa hello a szabványos konfiguráció látható.</span><span class="sxs-lookup"><span data-stu-id="3db6f-171">hello following example shows a standard configuration.</span></span>

### <a name="create-hello-file"></a><span data-ttu-id="3db6f-172">Hello fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="3db6f-172">Create hello file</span></span>

```bash
touch ~/.ssh/config
```

### <a name="edit-hello-file-tooadd-hello-new-ssh-configuration"></a><span data-ttu-id="3db6f-173">Hello fájl tooadd hello új SSH-konfiguráció szerkesztése:</span><span class="sxs-lookup"><span data-stu-id="3db6f-173">Edit hello file tooadd hello new SSH configuration:</span></span>

```bash
vim ~/.ssh/config
```

### <a name="example-sshconfig-file"></a><span data-ttu-id="3db6f-174">`~/.ssh/config` példafájl:</span><span class="sxs-lookup"><span data-stu-id="3db6f-174">Example `~/.ssh/config` file:</span></span>

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

<span data-ttu-id="3db6f-175">Ez SSH konfigurációs lehetőséget biztosít, részek az egyes kiszolgáló tooenable minden toohave saját dedikált kulcspárral.</span><span class="sxs-lookup"><span data-stu-id="3db6f-175">This SSH config gives you sections for each server tooenable each toohave its own dedicated key pair.</span></span> <span data-ttu-id="3db6f-176">alapértelmezett beállítások hello (`Host *`) vannak, amelyek nem felelnek meg bármelyik hello adott gazdagép feljebb hello konfigurációs fájlban állomásokra.</span><span class="sxs-lookup"><span data-stu-id="3db6f-176">hello default settings (`Host *`) are for any hosts that do not match any of hello specific hosts higher up in hello config file.</span></span>

### <a name="config-file-explained"></a><span data-ttu-id="3db6f-177">A konfigurációs fájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="3db6f-177">Config file explained</span></span>

<span data-ttu-id="3db6f-178">`Host`= hello hívott állomás hello terminál hello neve.</span><span class="sxs-lookup"><span data-stu-id="3db6f-178">`Host` = hello name of hello host being called on hello terminal.</span></span>  <span data-ttu-id="3db6f-179">`ssh fedora22`közli `SSH` toouse hello értékek hello beállítások blokkban feliratú `Host fedora22` Megjegyzés: állomás, amely logikus a használatra, és nem felel meg a kiszolgáló tényleges állomásnevét hello bármilyen olyan címke lehet.</span><span class="sxs-lookup"><span data-stu-id="3db6f-179">`ssh fedora22` tells `SSH` toouse hello values in hello settings block labeled `Host fedora22`  NOTE: Host can be any label that is logical for your usage and does not represent hello actual hostname of any server.</span></span>

<span data-ttu-id="3db6f-180">`Hostname 102.160.203.241`= hello IP-cím vagy hello server férnek hozzá a DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="3db6f-180">`Hostname 102.160.203.241` = hello IP address or DNS name for hello server being accessed.</span></span>

<span data-ttu-id="3db6f-181">`User ahmet`= hello távoli felhasználói fiók toouse toohello server bejelentkezéskor.</span><span class="sxs-lookup"><span data-stu-id="3db6f-181">`User ahmet` = hello remote user account toouse when logging in toohello server.</span></span>

<span data-ttu-id="3db6f-182">`PubKeyAuthentication yes`= közli az SSH a kívánt toouse egy SSH-kulcs toolog.</span><span class="sxs-lookup"><span data-stu-id="3db6f-182">`PubKeyAuthentication yes` = tells SSH you want toouse an SSH key toolog in.</span></span>

<span data-ttu-id="3db6f-183">`IdentityFile /home/ahmet/.ssh/id_id_rsa`= hello titkos SSH-kulcsot és a megfelelő nyilvános kulcs toouse hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="3db6f-183">`IdentityFile /home/ahmet/.ssh/id_id_rsa` = hello SSH private key and corresponding public key toouse for authentication.</span></span>

## <a name="ssh-into-linux-without-a-password"></a><span data-ttu-id="3db6f-184">SSH-ból Linuxba jelszó nélkül</span><span class="sxs-lookup"><span data-stu-id="3db6f-184">SSH into Linux without a password</span></span>

<span data-ttu-id="3db6f-185">Most, hogy az SSH-kulcspárral és konfigurált SSH konfigurációs fájl, a Linux virtuális gép tooyour képes toolog biztos gyorsan és biztonságosan.</span><span class="sxs-lookup"><span data-stu-id="3db6f-185">Now that you have an SSH key pair and a configured SSH config file, you are able toolog in tooyour Linux VM quickly and securely.</span></span> <span data-ttu-id="3db6f-186">hello első alkalommal jelentkezik be egy SSH-kulcs hello parancssorok tooa-kiszolgáló, az adott kulcsfájl hello jelszavát.</span><span class="sxs-lookup"><span data-stu-id="3db6f-186">hello first time you log in tooa server using an SSH key hello command prompts you for hello passphrase for that key file.</span></span>

```bash
ssh fedora22
```

### <a name="command-explained"></a><span data-ttu-id="3db6f-187">A parancs ismertetése</span><span class="sxs-lookup"><span data-stu-id="3db6f-187">Command explained</span></span>

<span data-ttu-id="3db6f-188">Ha `ssh fedora22` végrehajtása SSH először megkeresi és betölti a beállításokat a hello `Host fedora22` letiltása, majd betölti az összes hello hello utolsó blokk beállításait fennmaradó `Host *`.</span><span class="sxs-lookup"><span data-stu-id="3db6f-188">When `ssh fedora22` is executed SSH first locates and loads any settings from hello `Host fedora22` block, and then loads all hello remaining settings from hello last block, `Host *`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3db6f-189">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3db6f-189">Next Steps</span></span>

<span data-ttu-id="3db6f-190">Ezután mentése toocreate Azure Linux virtuális gépeket használ hello új nyilvános SSH-kulcs.</span><span class="sxs-lookup"><span data-stu-id="3db6f-190">Next up is toocreate Azure Linux VMs using hello new SSH public key.</span></span>  <span data-ttu-id="3db6f-191">Nyilvános SSH-kulcs hello bejelentkezésként használatával létrehozott Azure virtuális gépek, amelyek jobban biztonságos, mint a virtuális gépek hello alapértelmezett bejelentkezési módszer, jelszavak létre.</span><span class="sxs-lookup"><span data-stu-id="3db6f-191">Azure VMs that are created with an SSH public key as hello login are better secured than VMs created with hello default login method, passwords.</span></span>  <span data-ttu-id="3db6f-192">Az SSH-kulcsokkal létrehozott Azure virtuális gépek alapértelmezés szerint letiltott jelszavakkal vannak konfigurálva, ami megakadályozza a találgatásos támadásokat.</span><span class="sxs-lookup"><span data-stu-id="3db6f-192">Azure VMs created using SSH keys are by default configured with passwords disabled, avoiding brute-forced guessing attempts.</span></span> <span data-ttu-id="3db6f-193">Ha az SSH-kulcspárral létrehozásához további segítségre van szüksége, vagy további tanúsítványt igényel, mint például használja a klasszikus portálon hello, lásd: [részletes lépéseket toocreate SSH-kulcspár és tanúsítványok](create-ssh-keys-detailed.md).</span><span class="sxs-lookup"><span data-stu-id="3db6f-193">If you need more assistance in creating your SSH key pair or require additional certificates, such as for use with hello classic portal, see [Detailed steps toocreate SSH key pairs and certificates](create-ssh-keys-detailed.md).</span></span>

* [<span data-ttu-id="3db6f-194">Biztonságos Linux virtuális gép létrehozása Azure-sablon alapján</span><span class="sxs-lookup"><span data-stu-id="3db6f-194">Create a secure Linux VM using an Azure template</span></span>](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="3db6f-195">Biztonságos Linux virtuális gép létrehozása hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="3db6f-195">Create a secure Linux VM using hello Azure portal</span></span>](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="3db6f-196">Biztonságos Linux virtuális gép létrehozása Azure CLI hello</span><span class="sxs-lookup"><span data-stu-id="3db6f-196">Create a secure Linux VM using hello Azure CLI</span></span>](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
