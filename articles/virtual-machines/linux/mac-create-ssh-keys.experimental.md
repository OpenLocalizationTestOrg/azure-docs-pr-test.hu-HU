---
title: "SSH kulcspárok létrehozása Linux rendszerű virtuális gépekhez az Azure-on | Microsoft Docs"
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
ms.openlocfilehash: 19acd4efca7ef043f31b436b96f9129caee9591b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-ssh-public-and-private-key-pair-for-linux-vms"></a><span data-ttu-id="e097c-103">SSH nyilvános és titkos kulcspárok létrehozása Linux rendszerű virtuális gépekhez</span><span class="sxs-lookup"><span data-stu-id="e097c-103">Create an SSH public and private key pair for Linux VMs</span></span>

<span data-ttu-id="e097c-104">Ez a cikk bemutatja, hogyan hozhat létre az SSH-protokoll 2. verziójára épülő nyilvános és titkosított RSA-kulcspárt a Linux rendszerű virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="e097c-104">This article shows you how to generate an SSH protocol version 2 RSA public and private key file pair to use with Linux VMs.</span></span>  <span data-ttu-id="e097c-105">Egy SSH-kulcspárral létrehozhat olyan virtuális gépeket az Azure-ban, amelyek SSH-kulcsokat használnak a hitelesítéshez, aminek köszönhetően nincs szükség jelszavakra a bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="e097c-105">With an SSH key pair, you can create Virtual Machines on Azure that default to using SSH keys for authentication, eliminating the need for passwords to log in.</span></span>  <span data-ttu-id="e097c-106">A jelszavak kitalálhatók, és szüntelen találgatásos támadási kísérleteknek teszik ki a virtuális gépet a jelszó kiderítése céljából.</span><span class="sxs-lookup"><span data-stu-id="e097c-106">Passwords can be guessed, and open your VMs up to relentless brute force attempts to guess your password.</span></span> <span data-ttu-id="e097c-107">Az Azure-sablonokkal vagy az `azure-cli` használatával létrehozott virtuális gépek az üzembe helyezés részeként tartalmazhatják a nyilvános SSH-kulcsot, így nincs szükség az SSH-hoz a jelszavas belépést letiltó, üzembe helyezés utáni konfigurálási lépésre.</span><span class="sxs-lookup"><span data-stu-id="e097c-107">VMs created with Azure Templates or the `azure-cli` can include your SSH public key as part of the deployment, removing a post deployment configuration step of disabling password logins for SSH.</span></span>

## <a name="quick-commands"></a><span data-ttu-id="e097c-108">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="e097c-108">Quick Commands</span></span>

<span data-ttu-id="e097c-109">Futtassa a következő parancsokat egy rendszerhéjból, és a példákat helyettesítse be az Önnek megfelelő értékekkel.</span><span class="sxs-lookup"><span data-stu-id="e097c-109">Run the following commands from a Bash shell, replacing the examples with your own choices.</span></span>

<span data-ttu-id="e097c-110">Az SSH-ra épülő nyilvános kulcsfájl alapértelmezés szerint a következő helyen jön létre: `~/.ssh/id_rsa.pub`.</span><span class="sxs-lookup"><span data-stu-id="e097c-110">Your SSH public key file is by default created at `~/.ssh/id_rsa.pub`.</span></span> <span data-ttu-id="e097c-111">Ha a rendszer a következő parancs használatakor kéri, hozzon létre egy „hozzáférési kódot” a titkos kulcs védelme érdekében.</span><span class="sxs-lookup"><span data-stu-id="e097c-111">When prompted using the following command, you should create a "passphrase" to secure your private key.</span></span> <span data-ttu-id="e097c-112">(A hozzáférési kód a titkos kulcs titkosításához használt jelszó.)</span><span class="sxs-lookup"><span data-stu-id="e097c-112">(The passphrase is a password used to encrypt your private key.)</span></span>

```bash
ssh-keygen -t rsa -b 2048 
```

<span data-ttu-id="e097c-113">Adja az újonnan létrehozott kulcsot a következőhöz: `ssh-agent`.</span><span class="sxs-lookup"><span data-stu-id="e097c-113">Add the newly created key to `ssh-agent`:</span></span>

```bash
ssh-add ~/.ssh/id_rsa
```

> [!IMPORTANT] 
> <span data-ttu-id="e097c-114">A fenti parancsok szinte a Linux operációs rendszer szinte minden disztribúcióján használhatók, de nem feltétlenül működnek a tárolókban, ugyanis ez a környezet jelentősen korlátozott lehet.</span><span class="sxs-lookup"><span data-stu-id="e097c-114">The above commands work on Linux operating systems of almost all distributions, but do not necessarily work in containers, as the environment can be radically constrained.</span></span> 

## <a name="detailed-walkthrough"></a><span data-ttu-id="e097c-115">Részletes bemutató</span><span class="sxs-lookup"><span data-stu-id="e097c-115">Detailed Walkthrough</span></span>

<span data-ttu-id="e097c-116">A nyilvános és titkos SSH-kulcsok használata biztosítja a legegyszerűbb módot a Linux-kiszolgálókra való bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="e097c-116">Using SSH public and private keys is the easiest way to log in to your Linux servers.</span></span> <span data-ttu-id="e097c-117">A [nyíltkulcsú titkosítás](https://en.wikipedia.org/wiki/Public-key_cryptography) a jelszavak használatánál egy sokkal biztonságosabb módszert is lehetővé tesz a Linux vagy BSD virtuális gépekre való bejelentkezéshez az Azure-ban, hiszen a jelszavak sokkal könnyebben törhetők fel találgatásos módszeren alapuló támadással.</span><span class="sxs-lookup"><span data-stu-id="e097c-117">[Public-key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography) provides a much more secure way to log in to your Linux or BSD VM in Azure than passwords, which can be brute-forced far more easily.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e097c-118">A nyilvános kulcs bárkivel megosztható, azonban csak Ön (vagy a helyi biztonsági infrastruktúra) rendelkezik a titkos kulccsal.</span><span class="sxs-lookup"><span data-stu-id="e097c-118">Your public key can be shared with anyone; but only you (or your local security infrastructure) possess your private key.</span></span>  <span data-ttu-id="e097c-119">A titkos SSH-kulcsnak [nagyon biztonságos jelszóval](https://www.xkcd.com/936/) kell rendelkeznie (forrás: [xkcd.com](https://xkcd.com)) a védelme érdekében.</span><span class="sxs-lookup"><span data-stu-id="e097c-119">The SSH private key should have a [very secure password](https://www.xkcd.com/936/) (source:[xkcd.com](https://xkcd.com)) to safeguard it.</span></span>  <span data-ttu-id="e097c-120">Ez a jelszó csak a titkos SSH-kulcs elérésére szolgál, **nem** pedig a felhasználói fiók jelszava.</span><span class="sxs-lookup"><span data-stu-id="e097c-120">This password is just to access the private SSH key and **is not** the user account password.</span></span>  <span data-ttu-id="e097c-121">Amikor jelszót ad hozzá az SSH-kulcshoz, az a 128 bites AES segítségével titkosítja a titkos kulcsot, így a titkos kulcs használhatatlan az azt visszafejtő jelszó nélkül.</span><span class="sxs-lookup"><span data-stu-id="e097c-121">When you add a password to your SSH key, it encrypts the private key using 128-bit AES, so that the private key is useless without the password to decrypt it.</span></span>  <span data-ttu-id="e097c-122">Ha egy támadó ellopná a titkos kulcsot, és azt nem védené jelszó, a támadó a titkos kulccsal bejelentkezhetne azon kiszolgálókra, amelyeken a megfelelő nyilvános kulcs lett telepítve.</span><span class="sxs-lookup"><span data-stu-id="e097c-122">If an attacker stole your private key and that key did not have a password, they would be able to use that private key to log in to any servers that have the corresponding public key.</span></span>  <span data-ttu-id="e097c-123">Ha egy titkos kulcsot jelszó véd, a kulcsot nem használhatja a támadó, így ez további biztonsági réteget nyújt az Azure-beli infrastruktúra számára.</span><span class="sxs-lookup"><span data-stu-id="e097c-123">If a private key is password protected it cannot be used by that attacker, providing an additional layer of security for your infrastructure on Azure.</span></span>

<span data-ttu-id="e097c-124">Ez a cikk az SSH-protokoll 2. verziójára épülő nyilvános és titkosított RSA-kulcsfájl létrehozását ismerteti, amelyek használata a Resource Managerben üzemelő példányok esetén ajánlott.</span><span class="sxs-lookup"><span data-stu-id="e097c-124">This article creates SSH protocol version 2 RSA public and private key files, which are recommended for deployments on the Resource Manager.</span></span>  <span data-ttu-id="e097c-125">Az *ssh-rsa* kulcsokra van szükség a [portálon](https://portal.azure.com) a klasszikus és a Resource Managerben üzemelő példányokhoz is.</span><span class="sxs-lookup"><span data-stu-id="e097c-125">*ssh-rsa* keys are required on the [portal](https://portal.azure.com) for both classic and Resource Manager deployments.</span></span>

## <a name="disable-ssh-passwords-by-using-ssh-keys"></a><span data-ttu-id="e097c-126">SSH-jelszavak letiltása SSH-kulcsokkal</span><span class="sxs-lookup"><span data-stu-id="e097c-126">Disable SSH passwords by using SSH keys</span></span>

<span data-ttu-id="e097c-127">Az Azure legalább 2048 bites ssh-rsa formátumú nyilvános és titkos kulcsokat követel meg.</span><span class="sxs-lookup"><span data-stu-id="e097c-127">Azure requires at least 2048-bit, ssh-rsa format public and private keys.</span></span> <span data-ttu-id="e097c-128">A kulcsok létrehozásához az `ssh-keygen` parancsot használja, amely kérdések sorát teszi fel, majd létrehoz egy titkos kulcsot és egy megfelelő nyilvános kulcsot.</span><span class="sxs-lookup"><span data-stu-id="e097c-128">To create the keys use `ssh-keygen`, which asks a series of questions and then writes a private key and a matching public key.</span></span> <span data-ttu-id="e097c-129">Egy Azure virtuális gép létrehozásakor a rendszer a `~/.ssh/authorized_keys` mappába másolja a nyilvános kulcsot.</span><span class="sxs-lookup"><span data-stu-id="e097c-129">When an Azure VM is created, the public key is copied to `~/.ssh/authorized_keys`.</span></span>  <span data-ttu-id="e097c-130">A rendszer a `~/.ssh/authorized_keys` mappában lévő SSH-kulcsokat használja az ügyfél ellenőrzésére, hogy egyezik-e a megfelelő titkos kulcs egy SSH-bejelentkezési kapcsolat esetén.</span><span class="sxs-lookup"><span data-stu-id="e097c-130">SSH keys in `~/.ssh/authorized_keys` are used to challenge the client to match the corresponding private key on an SSH login connection.</span></span>  <span data-ttu-id="e097c-131">Ha az Azure Linux VM létrehozása során SSH-kulcsokat használt a hitelesítéshez, az Azure úgy konfigurálja az SSHD-kiszolgálót, hogy ne engedélyezze a jelszavas bejelentkezéseket, csak az SSH-kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="e097c-131">When an Azure Linux VM is created using SSH keys for authentication, Azure configures the SSHD server to not allow password logins, only SSH keys.</span></span>  <span data-ttu-id="e097c-132">Így az Azure Linux VM-ek SSH-kulcsokkal való létrehozása által biztonságosan telepítheti a VM-et, és megkímélheti magát az általános, üzembe helyezés utáni konfigurációs lépéstől, amely letiltja a jelszavakat az sshd_config konfigurációs fájlban.</span><span class="sxs-lookup"><span data-stu-id="e097c-132">Therefore, by creating Azure Linux VMs with SSH keys, you can help secure the VM deployment and save yourself the typical post-deployment configuration step of disabling passwords in the sshd_config config file.</span></span>

## <a name="using-ssh-keygen"></a><span data-ttu-id="e097c-133">Az ssh-keygen használata</span><span class="sxs-lookup"><span data-stu-id="e097c-133">Using ssh-keygen</span></span>

<span data-ttu-id="e097c-134">Ez a parancs jelszóval védett (titkosított) SSH-kulcspárt hoz létre 2048 bites RSA használatával, és az egyszerű azonosítás érdekében megjegyzéssel van ellátva.</span><span class="sxs-lookup"><span data-stu-id="e097c-134">This command creates a password secured (encrypted) SSH key pair using 2048-bit RSA and it is commented to easily identify it.</span></span>  

<span data-ttu-id="e097c-135">Az SSH-kulcsokat alapértelmezés szerint a `~/.ssh` könyvtár tárolja.</span><span class="sxs-lookup"><span data-stu-id="e097c-135">SSH keys are by default kept in the `~/.ssh` directory.</span></span>  <span data-ttu-id="e097c-136">Ha Ön nem rendelkezik `~/.ssh` könyvtárral, akkor az `ssh-keygen` parancs létrehozza azt a megfelelő engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="e097c-136">If you do not have a `~/.ssh` directory, the `ssh-keygen` command creates it for you with the correct permissions.</span></span>

```bash
ssh-keygen \
-t rsa \
-b 2048 
```

<span data-ttu-id="e097c-137">*A parancs ismertetése*</span><span class="sxs-lookup"><span data-stu-id="e097c-137">*Command explained*</span></span>

<span data-ttu-id="e097c-138">`ssh-keygen` = a kulcsok létrehozásához használt program,</span><span class="sxs-lookup"><span data-stu-id="e097c-138">`ssh-keygen` = the program used to create the keys</span></span>

<span data-ttu-id="e097c-139">`-t rsa` = a létrehozni kívánt kulcs típusa, amely az RSA formátum [wikipedia] (https://en.wikipedia.org/wiki/RSA_(cryptosystem)</span><span class="sxs-lookup"><span data-stu-id="e097c-139">`-t rsa` = type of key to create which is the RSA format [wikipedia](https://en.wikipedia.org/wiki/RSA_(cryptosystem)</span></span>

<span data-ttu-id="e097c-140">`-b 2048` = a kulcs bitjeinek száma,</span><span class="sxs-lookup"><span data-stu-id="e097c-140">`-b 2048` = bits of the key</span></span>


## <a name="classic-portal-and-x509-certs"></a><span data-ttu-id="e097c-141">A klasszikus portál és az X.509-tanúsítványok</span><span class="sxs-lookup"><span data-stu-id="e097c-141">Classic portal and X.509 certs</span></span>

<span data-ttu-id="e097c-142">Ha a [klasszikus Azure-portált](https://manage.windowsazure.com/) használja, az SSH-kulcsokhoz szükséges lesz az X.509-tanúsítványokra.</span><span class="sxs-lookup"><span data-stu-id="e097c-142">If you are using the Azure [classic portal](https://manage.windowsazure.com/), it requires X.509 certs for the SSH keys.</span></span>  <span data-ttu-id="e097c-143">Más típusú nyilvános SSH-kulcsok nem engedélyezettek, az X.509-tanúsítványokat *kell* használnia.</span><span class="sxs-lookup"><span data-stu-id="e097c-143">No other types of SSH public keys are allowed, they *must* be X.509 certs.</span></span>

<span data-ttu-id="e097c-144">Következőképpen hozhat létre egy X.509-tanúsítványt a meglévő titkos SSH-RSA-kulcsból:</span><span class="sxs-lookup"><span data-stu-id="e097c-144">To create an X.509 cert from your existing SSH-RSA private key:</span></span>

```bash
openssl req -x509 \
-key ~/.ssh/id_rsa \
-nodes \
-days 365 \
-newkey rsa:2048 \
-out ~/.ssh/id_rsa.pem
```

## <a name="classic-deploy-using-asm"></a><span data-ttu-id="e097c-145">Klasszikus üzembe helyezés az `asm` használatával</span><span class="sxs-lookup"><span data-stu-id="e097c-145">Classic deploy using `asm`</span></span>

<span data-ttu-id="e097c-146">Ha a klasszikus üzembe helyezési modellt használja (Azure service management CLI`asm`), használhatja a nyilvános SSH-RSA-kulcsot, vagy egy **pem-tárolóban** lévő RFC4716 formátumú kulcsot.</span><span class="sxs-lookup"><span data-stu-id="e097c-146">If you are using the classic deploy model (Azure service management CLI `asm`), you can use an SSH-RSA public key or an RFC4716 formatted key in a **.pem** container.</span></span>  <span data-ttu-id="e097c-147">A nyilvános SSH-RSA-kulcsot a cikk során korábban hoztuk létre az `ssh-keygen` használatával.</span><span class="sxs-lookup"><span data-stu-id="e097c-147">The SSH-RSA public key is what was created earlier in this article using `ssh-keygen`.</span></span>

<span data-ttu-id="e097c-148">RFC4716 formátumú kulcs meglévő nyilvános SSH-kulcsból történő létrehozása:</span><span class="sxs-lookup"><span data-stu-id="e097c-148">To create a RFC4716 formatted key from an existing SSH public key:</span></span>

```bash
ssh-keygen \
-f ~/.ssh/id_rsa.pub \
-e \
-m RFC4716 > ~/.ssh/id_ssh2.pem
```

## <a name="example-of-ssh-keygen"></a><span data-ttu-id="e097c-149">Ssh-keygen – példa</span><span class="sxs-lookup"><span data-stu-id="e097c-149">Example of ssh-keygen</span></span>

```bash
ssh-keygen -t rsa -b 2048 -C "ahmet@myserver"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ahmet/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/ahmet/.ssh/id_rsa.
Your public key has been saved in /home/ahmet/.ssh/id_rsa.pub.
The key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 ahmet@myserver
The keys randomart image is:
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

<span data-ttu-id="e097c-150">Mentett kulcsfájlok:</span><span class="sxs-lookup"><span data-stu-id="e097c-150">Saved key files:</span></span>

`Enter file in which to save the key (/home/ahmet/.ssh/id_rsa): ~/.ssh/id_rsa`

<span data-ttu-id="e097c-151">A cikkben használt kulcspár neve.</span><span class="sxs-lookup"><span data-stu-id="e097c-151">The key pair name for this article.</span></span>  <span data-ttu-id="e097c-152">Az **id_rsa** nevű kulcspár az alapértelmezett, és mivel egyes eszközök az **id_rsa** nevű titkos kulcsfájlt várhatják, célszerű ezt a nevet adni a kulcspárnak.</span><span class="sxs-lookup"><span data-stu-id="e097c-152">Having a key pair named **id_rsa** is the default and some tools might expect the **id_rsa** private key file name so having one is a good idea.</span></span> <span data-ttu-id="e097c-153">A `~/.ssh/` könyvtár az SSH-kulcspárok és az SSH konfigurációs fájl alapértelmezett helye.</span><span class="sxs-lookup"><span data-stu-id="e097c-153">The directory `~/.ssh/` is the default location for SSH key pairs and the SSH config file.</span></span>  <span data-ttu-id="e097c-154">Ha nincs megadva a teljes elérési útvonal, az `ssh-keygen` az aktuális munkakönyvtárban hozza létre a kulcsokat és nem az alapértelmezett `~/.ssh` könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="e097c-154">If not specified with a full path, `ssh-keygen` will create the keys in the current working directory, not the default `~/.ssh`.</span></span>

<span data-ttu-id="e097c-155">A `~/.ssh` könyvtár listázása.</span><span class="sxs-lookup"><span data-stu-id="e097c-155">A listing of the `~/.ssh` directory.</span></span>

```bash
ls -al ~/.ssh
-rw------- 1 ahmet staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 ahmet staff   410 Aug 25 18:04 rsa.pub
```

<span data-ttu-id="e097c-156">Kulcs jelszava:</span><span class="sxs-lookup"><span data-stu-id="e097c-156">Key Password:</span></span>

`Enter passphrase (empty for no passphrase):`

<span data-ttu-id="e097c-157">`ssh-keygen` a titkos kulcsfájl titkosítására használt jelszóra „hozzáférési kódként” hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="e097c-157">`ssh-keygen` refers to a password used to encrypt the private key as "a passphrase."</span></span>  <span data-ttu-id="e097c-158">*Erősen* ajánlott hozzáférési kódot hozzáadni a kulcspárokhoz.</span><span class="sxs-lookup"><span data-stu-id="e097c-158">It is *strongly* recommended to add a passphrase to your key pairs.</span></span> <span data-ttu-id="e097c-159">A titkos kulcsot védő hozzáférési kód nélkül bárki bejelentkezhet a kulcsfájllal bármely olyan kiszolgálóra, amely rendelkezik a megfelelő nyilvános kulccsal.</span><span class="sxs-lookup"><span data-stu-id="e097c-159">Without a passphrase protecting the private key, anyone with the key file can use it to log in to any server that has the corresponding public key.</span></span> <span data-ttu-id="e097c-160">A jelszó hozzáadása sokkal nagyobb védelmet nyújt arra az esetre, ha valaki megszerezné a titkos kulcsot, mert így lesz ideje módosítani a hitelesítéséhez használt kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="e097c-160">Adding a passphrase offers more protection in case someone is able to gain access to your private key file, giving you time to change the keys used to authenticate you.</span></span>

## <a name="using-ssh-agent-to-store-your-private-key-password"></a><span data-ttu-id="e097c-161">Az ssh-agent használata a titkos kulcs jelszavának tárolásához</span><span class="sxs-lookup"><span data-stu-id="e097c-161">Using ssh-agent to store your private key password</span></span>

<span data-ttu-id="e097c-162">Ha nem szeretné beírni a titkos kulcsfájl jelszavát minden SSH-bejelentkezéskor, az `ssh-agent` segítségével gyorsítótárazhatja a titkos kulcsfájl jelszavát.</span><span class="sxs-lookup"><span data-stu-id="e097c-162">To avoid typing your private key file passphrase with every SSH login, you can use `ssh-agent` to cache your private key file password.</span></span> <span data-ttu-id="e097c-163">Ha Mac gépet használ, az OSX Kulcskarika biztonságos módon menti a titkos kulcsok jelszavát az `ssh-agent` indításakor.</span><span class="sxs-lookup"><span data-stu-id="e097c-163">If you are using a Mac, the OSX Keychain securely stores the private key passwords when you invoke `ssh-agent`.</span></span>

<span data-ttu-id="e097c-164">Ellenőrizze és használja az `ssh-agent` és az `ssh-add` parancsot az SSH-rendszer kulcsfájlokról való informálásához, így nem kell a hozzáférési kódot interaktív módon használnia.</span><span class="sxs-lookup"><span data-stu-id="e097c-164">Verify and use `ssh-agent` and `ssh-add` to inform the SSH system about the key files so that the passphrase will not need to be used interactively.</span></span>

```bash
eval "$(ssh-agent -s)"
```

<span data-ttu-id="e097c-165">Ezután adja hozzá a titkos kulcsot az `ssh-agent` ügynökhöz az `ssh-add` paranccsal.</span><span class="sxs-lookup"><span data-stu-id="e097c-165">Now add the private key to `ssh-agent` using the command `ssh-add`.</span></span>

```bash
ssh-add ~/.ssh/id_rsa
```

<span data-ttu-id="e097c-166">A titkos kulcs jelszava ekkor az `ssh-agent` ügynökben lesz tárolva.</span><span class="sxs-lookup"><span data-stu-id="e097c-166">The private key password is now stored in `ssh-agent`.</span></span>

## <a name="using-ssh-copy-id-to-install-the-new-key"></a><span data-ttu-id="e097c-167">Az új kulcs telepítése az `ssh-copy-id` használatával</span><span class="sxs-lookup"><span data-stu-id="e097c-167">Using `ssh-copy-id` to install the new key</span></span>
<span data-ttu-id="e097c-168">Ha már létrehozott egy virtuális gépet, a következő paranccsal telepítheti az új, SSH-ra épülő nyilvános kulcsot a Linux rendszerű virtuális gépére (a virtuális gép felhasználónevét és a kiszolgáló címét cserélje le a saját értékeire):</span><span class="sxs-lookup"><span data-stu-id="e097c-168">If you have already created a VM you can install the new SSH public key to your Linux VM with the following command, replacing the VM username and the server address with your own values:</span></span>

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub ahmet@myserver
```

## <a name="create-and-configure-an-ssh-config-file"></a><span data-ttu-id="e097c-169">SSH konfigurációs fájl létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e097c-169">Create and configure an SSH config file</span></span>

<span data-ttu-id="e097c-170">Érdemes létrehozni és konfigurálni egy `~/.ssh/config` fájlt a bejelentkezések felgyorsításához és az SSH-ügyfél működésének optimalizálásához.</span><span class="sxs-lookup"><span data-stu-id="e097c-170">It is a best practice to create and configure an `~/.ssh/config` file to speed up log ins and for optimizing your SSH client behavior.</span></span>

<span data-ttu-id="e097c-171">Az alábbi példában a szabványos konfiguráció látható.</span><span class="sxs-lookup"><span data-stu-id="e097c-171">The following example shows a standard configuration.</span></span>

### <a name="create-the-file"></a><span data-ttu-id="e097c-172">A fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="e097c-172">Create the file</span></span>

```bash
touch ~/.ssh/config
```

### <a name="edit-the-file-to-add-the-new-ssh-configuration"></a><span data-ttu-id="e097c-173">A fájl szerkesztése az új SSH-konfiguráció hozzáadásához:</span><span class="sxs-lookup"><span data-stu-id="e097c-173">Edit the file to add the new SSH configuration:</span></span>

```bash
vim ~/.ssh/config
```

### <a name="example-sshconfig-file"></a><span data-ttu-id="e097c-174">`~/.ssh/config` példafájl:</span><span class="sxs-lookup"><span data-stu-id="e097c-174">Example `~/.ssh/config` file:</span></span>

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

<span data-ttu-id="e097c-175">Ez az SSH-konfiguráció szakaszokat ad hozzá mindegyik kiszolgálóhoz, hogy mindegyik saját dedikált kulcspárral rendelkezzen.</span><span class="sxs-lookup"><span data-stu-id="e097c-175">This SSH config gives you sections for each server to enable each to have its own dedicated key pair.</span></span> <span data-ttu-id="e097c-176">Az alapértelmezett beállítások (`Host *`) a konfigurációs fájlban feljebb szereplő adott állomásoknak nem megfelelő állomásokra vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="e097c-176">The default settings (`Host *`) are for any hosts that do not match any of the specific hosts higher up in the config file.</span></span>

### <a name="config-file-explained"></a><span data-ttu-id="e097c-177">A konfigurációs fájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="e097c-177">Config file explained</span></span>

<span data-ttu-id="e097c-178">`Host` = a terminálon hívott állomás neve.</span><span class="sxs-lookup"><span data-stu-id="e097c-178">`Host` = the name of the host being called on the terminal.</span></span>  <span data-ttu-id="e097c-179">`ssh fedora22` a `Host fedora22` címkéjű beállításblokkban található értékek használatára szólítja fel az `SSH`-t. MEGJEGYZÉS: Gazdagép bármilyen olyan címke lehet, amely logikus a használatra vonatkozóan, és egyetlen kiszolgáló tényleges állomásnevét sem képviseli.</span><span class="sxs-lookup"><span data-stu-id="e097c-179">`ssh fedora22` tells `SSH` to use the values in the settings block labeled `Host fedora22`  NOTE: Host can be any label that is logical for your usage and does not represent the actual hostname of any server.</span></span>

<span data-ttu-id="e097c-180">`Hostname 102.160.203.241` = azon kiszolgáló IP-címe vagy DNS-neve, amelyhez hozzáfér.</span><span class="sxs-lookup"><span data-stu-id="e097c-180">`Hostname 102.160.203.241` = the IP address or DNS name for the server being accessed.</span></span>

<span data-ttu-id="e097c-181">`User ahmet` = a használandó távoli felhasználói fiók a kiszolgálóra való bejelentkezéskor.</span><span class="sxs-lookup"><span data-stu-id="e097c-181">`User ahmet` = the remote user account to use when logging in to the server.</span></span>

<span data-ttu-id="e097c-182">`PubKeyAuthentication yes` = közli az SSH-val, hogy SSH-kulcsot kíván használni a bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="e097c-182">`PubKeyAuthentication yes` = tells SSH you want to use an SSH key to log in.</span></span>

<span data-ttu-id="e097c-183">`IdentityFile /home/ahmet/.ssh/id_id_rsa` = a titkos SSH-kulcs és a hitelesítéshez használt megfelelő nyilvános kulcs.</span><span class="sxs-lookup"><span data-stu-id="e097c-183">`IdentityFile /home/ahmet/.ssh/id_id_rsa` = the SSH private key and corresponding public key to use for authentication.</span></span>

## <a name="ssh-into-linux-without-a-password"></a><span data-ttu-id="e097c-184">SSH-ból Linuxba jelszó nélkül</span><span class="sxs-lookup"><span data-stu-id="e097c-184">SSH into Linux without a password</span></span>

<span data-ttu-id="e097c-185">Most, hogy rendelkezik SSH-kulcspárral és konfigurált SSH konfigurációs fájllal, gyorsan és biztonságosan jelentkezhet be a Linux virtuális gépre.</span><span class="sxs-lookup"><span data-stu-id="e097c-185">Now that you have an SSH key pair and a configured SSH config file, you are able to log in to your Linux VM quickly and securely.</span></span> <span data-ttu-id="e097c-186">Amikor először jelentkezik be egy kiszolgálóra SSH-kulccsal, a parancs felszólítja a kulcs jelszavának megadására.</span><span class="sxs-lookup"><span data-stu-id="e097c-186">The first time you log in to a server using an SSH key the command prompts you for the passphrase for that key file.</span></span>

```bash
ssh fedora22
```

### <a name="command-explained"></a><span data-ttu-id="e097c-187">A parancs ismertetése</span><span class="sxs-lookup"><span data-stu-id="e097c-187">Command explained</span></span>

<span data-ttu-id="e097c-188">Az `ssh fedora22` futtatásakor az SSH először megkeresi és betölti a `Host fedora22` blokk beállításait, majd betölti az összes többi beállítást az utolsó `Host *` blokkból.</span><span class="sxs-lookup"><span data-stu-id="e097c-188">When `ssh fedora22` is executed SSH first locates and loads any settings from the `Host fedora22` block, and then loads all the remaining settings from the last block, `Host *`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e097c-189">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e097c-189">Next Steps</span></span>

<span data-ttu-id="e097c-190">Ezután létre kell hoznia az Azure Linux virtuális gépeket az új nyilvános SSH-kulcs használatával.</span><span class="sxs-lookup"><span data-stu-id="e097c-190">Next up is to create Azure Linux VMs using the new SSH public key.</span></span>  <span data-ttu-id="e097c-191">A bejelentkezéshez nyilvános SSH-kulccsal létrehozott Azure virtuális gépek biztonságosabbak, mint az alapértelmezett bejelentkezési módszer jelszavaival létrehozott virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="e097c-191">Azure VMs that are created with an SSH public key as the login are better secured than VMs created with the default login method, passwords.</span></span>  <span data-ttu-id="e097c-192">Az SSH-kulcsokkal létrehozott Azure virtuális gépek alapértelmezés szerint letiltott jelszavakkal vannak konfigurálva, ami megakadályozza a találgatásos támadásokat.</span><span class="sxs-lookup"><span data-stu-id="e097c-192">Azure VMs created using SSH keys are by default configured with passwords disabled, avoiding brute-forced guessing attempts.</span></span> <span data-ttu-id="e097c-193">Ha szüksége van további segítségre az SSH-kulcspár létrehozásához vagy további tanúsítványokra van szüksége, például a klasszikus portál használatához, tekintse meg az [SSH-kulcspárok és -tanúsítványok létrehozásának lépéseit](create-ssh-keys-detailed.md).</span><span class="sxs-lookup"><span data-stu-id="e097c-193">If you need more assistance in creating your SSH key pair or require additional certificates, such as for use with the classic portal, see [Detailed steps to create SSH key pairs and certificates](create-ssh-keys-detailed.md).</span></span>

* [<span data-ttu-id="e097c-194">Biztonságos Linux virtuális gép létrehozása Azure-sablon alapján</span><span class="sxs-lookup"><span data-stu-id="e097c-194">Create a secure Linux VM using an Azure template</span></span>](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="e097c-195">Biztonságos Linux virtuális gép létrehozása az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="e097c-195">Create a secure Linux VM using the Azure portal</span></span>](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="e097c-196">Biztonságos Linux virtuális gép létrehozása az Azure parancssori felülettel</span><span class="sxs-lookup"><span data-stu-id="e097c-196">Create a secure Linux VM using the Azure CLI</span></span>](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
