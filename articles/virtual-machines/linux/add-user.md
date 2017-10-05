---
title: "Felhasználó hozzáadása a Linux virtuális gépek Azure-on |} Microsoft Docs"
description: "Felhasználó hozzáadása az Azure-on Linux virtuális gép."
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
ms.openlocfilehash: a95157f57c0cbd1f2a9ed68a0fe83140d7c9ec40
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="add-a-user-to-an-azure-vm"></a><span data-ttu-id="27da4-103">Felhasználó hozzáadása az Azure virtuális gép</span><span class="sxs-lookup"><span data-stu-id="27da4-103">Add a user to an Azure VM</span></span>
<span data-ttu-id="27da4-104">Az első feladatok számára bármely új Linux virtuális gép egyik hozzon létre egy új felhasználót.</span><span class="sxs-lookup"><span data-stu-id="27da4-104">One of the first tasks on any new Linux VM is to create a new user.</span></span>  <span data-ttu-id="27da4-105">Ez a cikk azt végezze el a sudo felhasználói fiók létrehozásakor a jelszó beállítása SSH nyilvános kulcsok hozzáadása, és végül használja `visudo` sudo jelszó nélkül engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="27da4-105">In this article, we walk through creating a sudo user account, setting the password, adding SSH Public Keys, and finally use `visudo` to allow sudo without a password.</span></span>

<span data-ttu-id="27da4-106">Előfeltételek: [az Azure-fiók](https://azure.microsoft.com/pricing/free-trial/), [SSH nyilvános és titkos kulcsok](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), egy Azure-erőforráscsoportot, és az Azure parancssori felület telepítve, és át lett váltva Azure Resource Manager módban a `azure config mode arm`.</span><span class="sxs-lookup"><span data-stu-id="27da4-106">Prerequisites are: [an Azure account](https://azure.microsoft.com/pricing/free-trial/), [SSH public and private keys](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), an Azure resource group, and the Azure CLI installed and switched to Azure Resource Manager mode using `azure config mode arm`.</span></span>

## <a name="quick-commands"></a><span data-ttu-id="27da4-107">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="27da4-107">Quick Commands</span></span>
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

# Copy the SSH Public Key to the new user
ssh-copy-id -i ~/.ssh/id_rsa exampleuser@exampleserver

# Change sudoers to allow no password
# Execute visudo as root to edit the /etc/sudoers file
visudo

# On RedHat family distros uncomment this line:
## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL

# to this
## Same thing without a password
%wheel        ALL=(ALL)       NOPASSWD: ALL

# On Debian family distros change this line:
# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# to this
%sudo   ALL=(ALL) NOPASSWD:ALL

# Verify everything
# Verify the SSH keys & User account
bill@slackware$ ssh -i ~/.ssh/id_rsa exampleuser@exampleserver

# Verify sudo access
sudo top
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="27da4-108">Részletes bemutató</span><span class="sxs-lookup"><span data-stu-id="27da4-108">Detailed Walkthrough</span></span>
### <a name="introduction"></a><span data-ttu-id="27da4-109">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="27da4-109">Introduction</span></span>
<span data-ttu-id="27da4-110">A első és a leggyakoribb feladat, az új kiszolgáló egyik adhat hozzá felhasználói fiókot.</span><span class="sxs-lookup"><span data-stu-id="27da4-110">One of the first and most common task with a new server is to add a user account.</span></span>  <span data-ttu-id="27da4-111">Legfelső szintű bejelentkezések le kell tiltani, és a legfelső szintű fiókon nem használható a Linux-kiszolgálóval, csak a sudo.</span><span class="sxs-lookup"><span data-stu-id="27da4-111">Root logins should be disabled and the root account itself should not be used with your Linux server, only sudo.</span></span>  <span data-ttu-id="27da4-112">Jogosultságot ad a felhasználó legfelső szintű kiterjesztés jogosultságokkal sudo használatával azt a megfelelő módon felügyelheti, és használja a Linux.</span><span class="sxs-lookup"><span data-stu-id="27da4-112">Giving a user root escalation privileges using sudo it the proper way to administer and use Linux.</span></span>

<span data-ttu-id="27da4-113">A paranccsal `useradd` azt ad hozzá felhasználói fiókokat a Linux virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="27da4-113">Using the command `useradd` we are adding user accounts to the Linux VM.</span></span>  <span data-ttu-id="27da4-114">Futó `useradd` módosítja `/etc/passwd`, `/etc/shadow`, `/etc/group`, és `/etc/gshadow`.</span><span class="sxs-lookup"><span data-stu-id="27da4-114">Running `useradd` modifies `/etc/passwd`, `/etc/shadow`, `/etc/group`, and `/etc/gshadow`.</span></span>  <span data-ttu-id="27da4-115">Azt ad hozzá egy parancssori jelzőjét, hogy a `useradd` parancs futtatásával is adja hozzá az új felhasználó a megfelelő sudo csoport Linux rendszeren.</span><span class="sxs-lookup"><span data-stu-id="27da4-115">We are adding a command-line flag to the `useradd` command to also add the new user to the proper sudo group on Linux.</span></span>  <span data-ttu-id="27da4-116">Akkor is Te `useradd` bejegyzést hoz létre a `/etc/passwd` az nem engedi meg az új felhasználói fiókhoz jelszót.</span><span class="sxs-lookup"><span data-stu-id="27da4-116">Even thou `useradd` creates an entry into `/etc/passwd` it does not give the new user account a password.</span></span>  <span data-ttu-id="27da4-117">Az új felhasználó használja az egyszerű egy kezdeti jelszó létrehozzuk `passwd` parancsot.</span><span class="sxs-lookup"><span data-stu-id="27da4-117">We are creating an initial password for the new user using the simple `passwd` command.</span></span>  <span data-ttu-id="27da4-118">Az utolsó lépése, hogy engedélyezi, hogy a felhasználó sudo jogosultságokkal rendelkező parancsok végrehajtása ne kelljen minden parancsot írja be a jelszót a sudo szabályok módosítása.</span><span class="sxs-lookup"><span data-stu-id="27da4-118">The last step is to modify the sudo rules to allow that user to execute commands with sudo privileges without having to enter a password for every command.</span></span>  <span data-ttu-id="27da4-119">A titkos kulcsot, a felhasználói fiók jelenleg vélhetően adatokkal van bejelentkezve ezeken a biztonságos és sudo hozzáférést jelszó nélkül is.</span><span class="sxs-lookup"><span data-stu-id="27da4-119">Logging in using the Private key we are assuming that user account is safe from bad actors and are going to allow sudo access without a password.</span></span>  

### <a name="adding-a-single-sudo-user-to-an-azure-vm"></a><span data-ttu-id="27da4-120">Egyetlen sudo felhasználók hozzáadása az Azure virtuális gép</span><span class="sxs-lookup"><span data-stu-id="27da4-120">Adding a single sudo user to an Azure VM</span></span>
<span data-ttu-id="27da4-121">Jelentkezzen be az SSH-kulcsokat használó Azure virtuális Géphez.</span><span class="sxs-lookup"><span data-stu-id="27da4-121">Log in to the Azure VM using SSH keys.</span></span>  <span data-ttu-id="27da4-122">Ha még nem telepítő SSH nyilvános hozzáférés a kulcshoz, végezze el az ebben a cikkben először [nyilvános kulcs Authentication használata az Azure-ral](http://link.to/article).</span><span class="sxs-lookup"><span data-stu-id="27da4-122">If you have not setup SSH public key access, complete this article first [Using Public Key Authentication with Azure](http://link.to/article).</span></span>  

<span data-ttu-id="27da4-123">A `useradd` parancs az alábbi feladatokat hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="27da4-123">The `useradd` command completes the following tasks:</span></span>

* <span data-ttu-id="27da4-124">új felhasználói fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="27da4-124">create a new user account</span></span>
* <span data-ttu-id="27da4-125">Hozzon létre egy új felhasználói csoportot ugyanazzal a névvel</span><span class="sxs-lookup"><span data-stu-id="27da4-125">create a new user group with the same name</span></span>
* <span data-ttu-id="27da4-126">az üres bejegyzés hozzáadása`/etc/passwd`</span><span class="sxs-lookup"><span data-stu-id="27da4-126">add a blank entry to `/etc/passwd`</span></span>
* <span data-ttu-id="27da4-127">az üres bejegyzés hozzáadása`/etc/gpasswd`</span><span class="sxs-lookup"><span data-stu-id="27da4-127">add a blank entry to `/etc/gpasswd`</span></span>

<span data-ttu-id="27da4-128">A `-G` parancssori jelző hozzáadja az új felhasználói fiók a megfelelő Linux csoporthoz jogosultságot ad az új felhasználói fiók legfelső szintű kiterjesztés jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="27da4-128">The `-G` command-line flag adds the new user account to the proper Linux group giving the new user account root escalation privileges.</span></span>

#### <a name="add-the-user"></a><span data-ttu-id="27da4-129">A felhasználó hozzáadása</span><span class="sxs-lookup"><span data-stu-id="27da4-129">Add the user</span></span>
```bash
# On RedHat family distros
sudo useradd -G wheel exampleUser

# On Debian family distros
sudo useradd -G sudo exampleUser
```

#### <a name="set-a-password"></a><span data-ttu-id="27da4-130">Jelszó beállítása</span><span class="sxs-lookup"><span data-stu-id="27da4-130">Set a password</span></span>
<span data-ttu-id="27da4-131">A `useradd` parancs létrehozza a felhasználó, és mindkét bejegyzés hozzáadása `/etc/passwd` és `/etc/gpasswd` , de nem állítja be a jelszót.</span><span class="sxs-lookup"><span data-stu-id="27da4-131">The `useradd` command creates the user and adds an entry to both `/etc/passwd` and `/etc/gpasswd` but does not actually set the password.</span></span>  <span data-ttu-id="27da4-132">A jelszó hozzá lett adva a bejegyzés a `passwd` parancsot.</span><span class="sxs-lookup"><span data-stu-id="27da4-132">The password is added to the entry using the `passwd` command.</span></span>

```bash
sudo passwd exampleUser
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
```

<span data-ttu-id="27da4-133">Most már tudunk sudo jogosultsággal a kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="27da4-133">We now have a user with sudo privileges on the server.</span></span>

### <a name="add-your-ssh-public-key-to-the-new-user-account"></a><span data-ttu-id="27da4-134">Az SSH nyilvános kulcs hozzáadása az új felhasználói fiók</span><span class="sxs-lookup"><span data-stu-id="27da4-134">Add your SSH Public Key to the new user account</span></span>
<span data-ttu-id="27da4-135">A számítógépről, használja a `ssh-copy-id` parancsot az új jelszót.</span><span class="sxs-lookup"><span data-stu-id="27da4-135">From your machine, use the `ssh-copy-id` command with the new password.</span></span>

```bash
ssh-copy-id -i ~/.ssh/id_rsa exampleuser@exampleserver
```

### <a name="using-visudo-to-allow-sudo-usage-without-a-password"></a><span data-ttu-id="27da4-136">Visudo segítségével jelszó nélküli sudo használatának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="27da4-136">Using visudo to allow sudo usage without a password</span></span>
<span data-ttu-id="27da4-137">Használatával `visudo` szerkesztése a `/etc/sudoers` fájlt ad hozzá néhány rétegének védelmet biztosít a fontos fájl helytelen módosítása.</span><span class="sxs-lookup"><span data-stu-id="27da4-137">Using `visudo` to edit the `/etc/sudoers` file adds a few layers of protection against incorrectly modifying this important file.</span></span>  <span data-ttu-id="27da4-138">Végrehajtása után `visudo`, a `/etc/sudoers` fájl zárolva van, annak érdekében, hogy más felhasználó nem módosíthatja azt aktívan szerkesztése közben.</span><span class="sxs-lookup"><span data-stu-id="27da4-138">Upon executing `visudo`, the `/etc/sudoers` file is locked to ensure no other user can make changes while it is actively being edited.</span></span>  <span data-ttu-id="27da4-139">A `/etc/sudoers` fájl is be van jelölve a hibák által `visudo` amikor megpróbál menti vagy kilép, hibás sudoers fájl nem menthető.</span><span class="sxs-lookup"><span data-stu-id="27da4-139">The `/etc/sudoers` file is also checked for mistakes by `visudo` when you attempt to save or exit so you cannot save a broken sudoers file.</span></span>

<span data-ttu-id="27da4-140">Már van felhasználók a megfelelő alapértelmezett csoport a sudo hozzáféréshez.</span><span class="sxs-lookup"><span data-stu-id="27da4-140">We already have users in the correct default group for sudo access.</span></span>  <span data-ttu-id="27da4-141">Most fogjuk ezeket a csoportokat a jelszó nélküli sudo használandó engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="27da4-141">Now we are going to enable those groups to use sudo with no password.</span></span>

```bash
# Execute visudo as root to edit the /etc/sudoers file
visudo

# On RedHat family distros uncomment this line:
## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL

# to this
## Same thing without a password
%wheel        ALL=(ALL)       NOPASSWD: ALL

# On Debian family distros change this line:
# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# to this
%sudo   ALL=(ALL) NOPASSWD:ALL
```

### <a name="verify-the-user-ssh-keys-and-sudo"></a><span data-ttu-id="27da4-142">Ellenőrizze a felhasználó ssh kulcsok és a sudo</span><span class="sxs-lookup"><span data-stu-id="27da4-142">Verify the user, ssh keys, and sudo</span></span>
```bash
# Verify the SSH keys & User account
ssh -i ~/.ssh/id_rsa exampleuser@exampleserver

# Verify sudo access
sudo top
```
