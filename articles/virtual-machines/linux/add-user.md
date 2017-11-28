---
title: "egy felhasználó tooa Azure Linux virtuális gép aaaAdd |} Microsoft Docs"
description: "Adjon hozzá egy felhasználói tooa Linux virtuális gép az Azure-on."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f8aa633b-8b75-45d7-b61d-11ac112cedd5
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/18/2016
ms.author: v-livech
ms.openlocfilehash: eed050154adf0cbed2c168e7aa675bd3ded85fcd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-user-tooan-azure-vm"></a><span data-ttu-id="fcc92-103">Egy felhasználó tooan Azure virtuális gép hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fcc92-103">Add a user tooan Azure VM</span></span>
<span data-ttu-id="fcc92-104">Első feladatai hello bármely új Linux virtuális gép egyik toocreate egy új felhasználót.</span><span class="sxs-lookup"><span data-stu-id="fcc92-104">One of hello first tasks on any new Linux VM is toocreate a new user.</span></span>  <span data-ttu-id="fcc92-105">Ez a cikk azt végezze el a sudo felhasználói fiók, jelszó beállítása hello, létrehozása SSH nyilvános kulcsok hozzáadása, és végül használja `visudo` tooallow sudo jelszó nélkül.</span><span class="sxs-lookup"><span data-stu-id="fcc92-105">In this article, we walk through creating a sudo user account, setting hello password, adding SSH Public Keys, and finally use `visudo` tooallow sudo without a password.</span></span>

<span data-ttu-id="fcc92-106">Előfeltételek: [az Azure-fiók](https://azure.microsoft.com/pricing/free-trial/), [SSH nyilvános és titkos kulcsok](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), egy Azure-erőforráscsoportot és hello Azure parancssori felület telepítve, és tooAzure erőforrás-kezelő módban a kapcsolt `azure config mode arm`.</span><span class="sxs-lookup"><span data-stu-id="fcc92-106">Prerequisites are: [an Azure account](https://azure.microsoft.com/pricing/free-trial/), [SSH public and private keys](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), an Azure resource group, and hello Azure CLI installed and switched tooAzure Resource Manager mode using `azure config mode arm`.</span></span>

## <a name="quick-commands"></a><span data-ttu-id="fcc92-107">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="fcc92-107">Quick Commands</span></span>
```bash
# Add a new user on RedHat family distros
sudo useradd -G wheel exampleUser

# Add a new user on Debian family distros
sudo useradd -G sudo exampleUser

# Set a password
sudo passwd exampleUser
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully

# Copy hello SSH Public Key toohello new user
ssh-copy-id -i ~/.ssh/id_rsa exampleuser@exampleserver

# Change sudoers tooallow no password
# Execute visudo as root tooedit hello /etc/sudoers file
visudo

# On RedHat family distros uncomment this line:
## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL

# toothis
## Same thing without a password
%wheel        ALL=(ALL)       NOPASSWD: ALL

# On Debian family distros change this line:
# Allow members of group sudo tooexecute any command
%sudo   ALL=(ALL:ALL) ALL

# toothis
%sudo   ALL=(ALL) NOPASSWD:ALL

# Verify everything
# Verify hello SSH keys & User account
bill@slackware$ ssh -i ~/.ssh/id_rsa exampleuser@exampleserver

# Verify sudo access
sudo top
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="fcc92-108">Részletes bemutató</span><span class="sxs-lookup"><span data-stu-id="fcc92-108">Detailed Walkthrough</span></span>
### <a name="introduction"></a><span data-ttu-id="fcc92-109">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="fcc92-109">Introduction</span></span>
<span data-ttu-id="fcc92-110">Az új kiszolgáló első és a leggyakoribb feladat hello egyik tooadd egy felhasználói fiókot.</span><span class="sxs-lookup"><span data-stu-id="fcc92-110">One of hello first and most common task with a new server is tooadd a user account.</span></span>  <span data-ttu-id="fcc92-111">Legfelső szintű bejelentkezések le kell tiltani, és hello root fiókjának magát a Linux-kiszolgálóval, csak a sudo nem használható.</span><span class="sxs-lookup"><span data-stu-id="fcc92-111">Root logins should be disabled and hello root account itself should not be used with your Linux server, only sudo.</span></span>  <span data-ttu-id="fcc92-112">Jogosultságot ad a felhasználói legfelső szintű kiterjesztés sudo hello megfelelő módon tooadminister, és használja a Linux használatával jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="fcc92-112">Giving a user root escalation privileges using sudo it hello proper way tooadminister and use Linux.</span></span>

<span data-ttu-id="fcc92-113">Hello paranccsal `useradd` azt ad hozzá felhasználói fiókokat toohello Linux virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="fcc92-113">Using hello command `useradd` we are adding user accounts toohello Linux VM.</span></span>  <span data-ttu-id="fcc92-114">Futó `useradd` módosítja `/etc/passwd`, `/etc/shadow`, `/etc/group`, és `/etc/gshadow`.</span><span class="sxs-lookup"><span data-stu-id="fcc92-114">Running `useradd` modifies `/etc/passwd`, `/etc/shadow`, `/etc/group`, and `/etc/gshadow`.</span></span>  <span data-ttu-id="fcc92-115">Azt ad hozzá egy parancssori jelző toohello `useradd` tooalso hello új felhasználói toohello megfelelő sudo csoport hozzáadása Linux parancs.</span><span class="sxs-lookup"><span data-stu-id="fcc92-115">We are adding a command-line flag toohello `useradd` command tooalso add hello new user toohello proper sudo group on Linux.</span></span>  <span data-ttu-id="fcc92-116">Akkor is Te `useradd` bejegyzést hoz létre a `/etc/passwd` az nem engedi hello új felhasználói fiókhoz jelszót.</span><span class="sxs-lookup"><span data-stu-id="fcc92-116">Even thou `useradd` creates an entry into `/etc/passwd` it does not give hello new user account a password.</span></span>  <span data-ttu-id="fcc92-117">Egy kezdeti hello hello egyszerű használatával új felhasználói jelszavát létrehozzuk `passwd` parancsot.</span><span class="sxs-lookup"><span data-stu-id="fcc92-117">We are creating an initial password for hello new user using hello simple `passwd` command.</span></span>  <span data-ttu-id="fcc92-118">hello utolsó lépése toomodify hello sudo szabályok tooallow sudo jogosultságokkal rendelkező adott felhasználói tooexecute parancsok tooenter minden parancs jelszó nélkül.</span><span class="sxs-lookup"><span data-stu-id="fcc92-118">hello last step is toomodify hello sudo rules tooallow that user tooexecute commands with sudo privileges without having tooenter a password for every command.</span></span>  <span data-ttu-id="fcc92-119">Hello titkos kulcsot, a felhasználói fiók jelenleg vélhetően adatokkal van bejelentkezve ezeken a biztonságos és tooallow sudo hozzáférés jelszó nélkül is.</span><span class="sxs-lookup"><span data-stu-id="fcc92-119">Logging in using hello Private key we are assuming that user account is safe from bad actors and are going tooallow sudo access without a password.</span></span>  

### <a name="adding-a-single-sudo-user-tooan-azure-vm"></a><span data-ttu-id="fcc92-120">A sudo egyetlen felhasználó tooan Azure virtuális gép hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fcc92-120">Adding a single sudo user tooan Azure VM</span></span>
<span data-ttu-id="fcc92-121">Jelentkezzen be toohello SSH-kulcsokat használó Azure virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="fcc92-121">Log in toohello Azure VM using SSH keys.</span></span>  <span data-ttu-id="fcc92-122">Ha még nem telepítő SSH nyilvános hozzáférés a kulcshoz, végezze el az ebben a cikkben először [nyilvános kulcs Authentication használata az Azure-ral](http://link.to/article).</span><span class="sxs-lookup"><span data-stu-id="fcc92-122">If you have not setup SSH public key access, complete this article first [Using Public Key Authentication with Azure](http://link.to/article).</span></span>  

<span data-ttu-id="fcc92-123">Hello `useradd` parancs végrehajtása után a következő feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="fcc92-123">hello `useradd` command completes hello following tasks:</span></span>

* <span data-ttu-id="fcc92-124">új felhasználói fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="fcc92-124">create a new user account</span></span>
* <span data-ttu-id="fcc92-125">Hozzon létre egy új felhasználói csoportot hello ugyanazzal a névvel</span><span class="sxs-lookup"><span data-stu-id="fcc92-125">create a new user group with hello same name</span></span>
* <span data-ttu-id="fcc92-126">üres bejegyzés túl hozzáadása`/etc/passwd`</span><span class="sxs-lookup"><span data-stu-id="fcc92-126">add a blank entry too`/etc/passwd`</span></span>
* <span data-ttu-id="fcc92-127">üres bejegyzés túl hozzáadása`/etc/gpasswd`</span><span class="sxs-lookup"><span data-stu-id="fcc92-127">add a blank entry too`/etc/gpasswd`</span></span>

<span data-ttu-id="fcc92-128">Hello `-G` parancssori jelző hello új felhasználói fiók toohello megfelelő Linux csoport hozzáadása jogosultságot ad az új felhasználói fiók hello legfelső szintű kiterjesztés jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="fcc92-128">hello `-G` command-line flag adds hello new user account toohello proper Linux group giving hello new user account root escalation privileges.</span></span>

#### <a name="add-hello-user"></a><span data-ttu-id="fcc92-129">Hello felhasználó hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fcc92-129">Add hello user</span></span>
```bash
# On RedHat family distros
sudo useradd -G wheel exampleUser

# On Debian family distros
sudo useradd -G sudo exampleUser
```

#### <a name="set-a-password"></a><span data-ttu-id="fcc92-130">Jelszó beállítása</span><span class="sxs-lookup"><span data-stu-id="fcc92-130">Set a password</span></span>
<span data-ttu-id="fcc92-131">Hello `useradd` parancs hello felhasználó hoz létre, és hozzáad egy bejegyzés tooboth `/etc/passwd` és `/etc/gpasswd` , de valójában a hello jelszó nincs beállítva.</span><span class="sxs-lookup"><span data-stu-id="fcc92-131">hello `useradd` command creates hello user and adds an entry tooboth `/etc/passwd` and `/etc/gpasswd` but does not actually set hello password.</span></span>  <span data-ttu-id="fcc92-132">hello jelszó kerül hello segítségével toohello bejegyzés `passwd` parancsot.</span><span class="sxs-lookup"><span data-stu-id="fcc92-132">hello password is added toohello entry using hello `passwd` command.</span></span>

```bash
sudo passwd exampleUser
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
```

<span data-ttu-id="fcc92-133">Most már tudunk sudo jogosultsággal hello kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="fcc92-133">We now have a user with sudo privileges on hello server.</span></span>

### <a name="add-your-ssh-public-key-toohello-new-user-account"></a><span data-ttu-id="fcc92-134">Az SSH nyilvános kulcs toohello új felhasználói fiók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fcc92-134">Add your SSH Public Key toohello new user account</span></span>
<span data-ttu-id="fcc92-135">A számítógépről, használja a hello `ssh-copy-id` hello új jelszó parancsot.</span><span class="sxs-lookup"><span data-stu-id="fcc92-135">From your machine, use hello `ssh-copy-id` command with hello new password.</span></span>

```bash
ssh-copy-id -i ~/.ssh/id_rsa exampleuser@exampleserver
```

### <a name="using-visudo-tooallow-sudo-usage-without-a-password"></a><span data-ttu-id="fcc92-136">Használatával visudo tooallow sudo használati jelszó nélkül</span><span class="sxs-lookup"><span data-stu-id="fcc92-136">Using visudo tooallow sudo usage without a password</span></span>
<span data-ttu-id="fcc92-137">Használatával `visudo` tooedit hello `/etc/sudoers` fájlt ad hozzá néhány rétegének védelmet biztosít a fontos fájl helytelen módosítása.</span><span class="sxs-lookup"><span data-stu-id="fcc92-137">Using `visudo` tooedit hello `/etc/sudoers` file adds a few layers of protection against incorrectly modifying this important file.</span></span>  <span data-ttu-id="fcc92-138">Végrehajtása után `visudo`, hello `/etc/sudoers` fájl zárolt tooensure más felhasználó nem módosíthatja azt aktívan szerkesztése közben.</span><span class="sxs-lookup"><span data-stu-id="fcc92-138">Upon executing `visudo`, hello `/etc/sudoers` file is locked tooensure no other user can make changes while it is actively being edited.</span></span>  <span data-ttu-id="fcc92-139">Hello `/etc/sudoers` fájl is be van jelölve a hibák által `visudo` toosave kísérletet, vagy amikor a lépni a hibás sudoers fájl nem menthető.</span><span class="sxs-lookup"><span data-stu-id="fcc92-139">hello `/etc/sudoers` file is also checked for mistakes by `visudo` when you attempt toosave or exit so you cannot save a broken sudoers file.</span></span>

<span data-ttu-id="fcc92-140">Már van felhasználó hello megfelelő alapértelmezett csoport sudo hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="fcc92-140">We already have users in hello correct default group for sudo access.</span></span>  <span data-ttu-id="fcc92-141">Most fogjuk tooenable ezen csoportok toouse sudo jelszó nélküli.</span><span class="sxs-lookup"><span data-stu-id="fcc92-141">Now we are going tooenable those groups toouse sudo with no password.</span></span>

```bash
# Execute visudo as root tooedit hello /etc/sudoers file
visudo

# On RedHat family distros uncomment this line:
## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL

# toothis
## Same thing without a password
%wheel        ALL=(ALL)       NOPASSWD: ALL

# On Debian family distros change this line:
# Allow members of group sudo tooexecute any command
%sudo   ALL=(ALL:ALL) ALL

# toothis
%sudo   ALL=(ALL) NOPASSWD:ALL
```

### <a name="verify-hello-user-ssh-keys-and-sudo"></a><span data-ttu-id="fcc92-142">Győződjön meg arról hello felhasználói ssh kulcsok és a sudo</span><span class="sxs-lookup"><span data-stu-id="fcc92-142">Verify hello user, ssh keys, and sudo</span></span>
```bash
# Verify hello SSH keys & User account
ssh -i ~/.ssh/id_rsa exampleuser@exampleserver

# Verify sudo access
sudo top
```
