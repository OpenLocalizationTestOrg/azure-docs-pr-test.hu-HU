---
title: "aaaUpdate hello Azure Linux ügynök a Githubról |} Microsoft Docs"
description: "Megtudhatja, hogyan tooupdate Azure Linux ügynök a Linux virtuális gép Azure toohello legújabb verzió a Githubról"
services: virtual-machines-linux
documentationcenter: 
author: SuperScottz
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: f1f19300-987d-4f29-9393-9aba866f049c
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: mingzhan
ms.openlocfilehash: 4ce7c56efc1e6563e6415f7687573f9fb9e7b4c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooupdate-hello-azure-linux-agent-on-a-vm"></a><span data-ttu-id="adaa5-103">Hogyan tooupdate hello Azure Linux ügynök a virtuális gép</span><span class="sxs-lookup"><span data-stu-id="adaa5-103">How tooupdate hello Azure Linux Agent on a VM</span></span>

<span data-ttu-id="adaa5-104">tooupdate a [Azure Linux ügynök](https://github.com/Azure/WALinuxAgent) a Linux virtuális gép az Azure-ban, már rendelkeznie kell:</span><span class="sxs-lookup"><span data-stu-id="adaa5-104">tooupdate your [Azure Linux Agent](https://github.com/Azure/WALinuxAgent) on a Linux VM in Azure, you must already have:</span></span>

- <span data-ttu-id="adaa5-105">A Linux virtuális gép az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="adaa5-105">A running Linux VM in Azure.</span></span>
- <span data-ttu-id="adaa5-106">Egy kapcsolat toothat Linux virtuális gép SSH használatával.</span><span class="sxs-lookup"><span data-stu-id="adaa5-106">A connection toothat Linux VM using SSH.</span></span>

<span data-ttu-id="adaa5-107">Mindig ellenőrizni kell a csomag hello Linux distro tárházban először.</span><span class="sxs-lookup"><span data-stu-id="adaa5-107">You should always check for a package in hello Linux distro repository first.</span></span> <span data-ttu-id="adaa5-108">Lehetséges hello csomag rendelkezésre nem lehet hello legújabb verziójára, azonban biztosítja automatikus frissítés engedélyezése hello Linux-ügynök mindig lesz hello legújabb frissítésének letöltése.</span><span class="sxs-lookup"><span data-stu-id="adaa5-108">It is possible hello package available may not be hello latest version, however, enabling autoupdate will ensure hello Linux Agent will always get hello latest update.</span></span> <span data-ttu-id="adaa5-109">Kell hello csomag kezelők történő telepítés problémák, támogatási hello distro szállítótól kell keresni.</span><span class="sxs-lookup"><span data-stu-id="adaa5-109">Should you have issues installing from hello package managers, you should seek support from hello distro vendor.</span></span>

## <a name="updating-hello-azure-linux-agent"></a><span data-ttu-id="adaa5-110">Hello Azure Linux ügynök frissítése</span><span class="sxs-lookup"><span data-stu-id="adaa5-110">Updating hello Azure Linux Agent</span></span>

## <a name="ubuntu"></a><span data-ttu-id="adaa5-111">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="adaa5-111">Ubuntu</span></span>

#### <a name="check-your-current-package-version"></a><span data-ttu-id="adaa5-112">Ellenőrizze a csomag aktuális verziója</span><span class="sxs-lookup"><span data-stu-id="adaa5-112">Check your current package version</span></span>

```bash
apt list --installed | grep walinuxagent
```

#### <a name="update-package-cache"></a><span data-ttu-id="adaa5-113">Frissítési csomag gyorsítótár</span><span class="sxs-lookup"><span data-stu-id="adaa5-113">Update package cache</span></span>

```bash
sudo apt-get -qq update
```

#### <a name="install-hello-latest-package-version"></a><span data-ttu-id="adaa5-114">Hello legújabb csomag verziójának telepítése</span><span class="sxs-lookup"><span data-stu-id="adaa5-114">Install hello latest package version</span></span>

```bash
sudo apt-get install walinuxagent
```

#### <a name="ensure-auto-update-is-enabled"></a><span data-ttu-id="adaa5-115">Győződjön meg róla, engedélyezve van az automatikus frissítés</span><span class="sxs-lookup"><span data-stu-id="adaa5-115">Ensure auto update is enabled</span></span>

<span data-ttu-id="adaa5-116">Először ellenőrizze a toosee, ha engedélyezve van:</span><span class="sxs-lookup"><span data-stu-id="adaa5-116">First, check toosee if it is enabled:</span></span>

```bash
cat /etc/waagent.conf
```

<span data-ttu-id="adaa5-117">"AutoUpdate.Enabled" található.</span><span class="sxs-lookup"><span data-stu-id="adaa5-117">Find 'AutoUpdate.Enabled'.</span></span> <span data-ttu-id="adaa5-118">A kimenet jelenik meg, ha engedélyezve van:</span><span class="sxs-lookup"><span data-stu-id="adaa5-118">If you see this output, it is enabled:</span></span>

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

<span data-ttu-id="adaa5-119">Futtatás tooenable:</span><span class="sxs-lookup"><span data-stu-id="adaa5-119">tooenable run:</span></span>

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a><span data-ttu-id="adaa5-120">Hello waagent szolgáltatás újraindítása</span><span class="sxs-lookup"><span data-stu-id="adaa5-120">Restart hello waagent service</span></span>

#### <a name="restart-agent-for-1404"></a><span data-ttu-id="adaa5-121">Indítsa újra az ügynököt a 14.04</span><span class="sxs-lookup"><span data-stu-id="adaa5-121">Restart agent for 14.04</span></span>

```bash
initctl restart walinuxagent
```

#### <a name="restart-agent-for-1604--1704"></a><span data-ttu-id="adaa5-122">Indítsa újra az ügynököt a 16.04 / 17.04</span><span class="sxs-lookup"><span data-stu-id="adaa5-122">Restart agent for 16.04 / 17.04</span></span>

```bash
systemctl restart walinuxagent.service
```

## <a name="debian"></a><span data-ttu-id="adaa5-123">Debian</span><span class="sxs-lookup"><span data-stu-id="adaa5-123">Debian</span></span>

### <a name="debian-7-wheezy"></a><span data-ttu-id="adaa5-124">Debian 7 "Wheezy"</span><span class="sxs-lookup"><span data-stu-id="adaa5-124">Debian 7 “Wheezy”</span></span>

#### <a name="check-your-current-package-version"></a><span data-ttu-id="adaa5-125">Ellenőrizze a csomag aktuális verziója</span><span class="sxs-lookup"><span data-stu-id="adaa5-125">Check your current package version</span></span>

```bash
dpkg -l | grep waagent
```

#### <a name="update-package-cache"></a><span data-ttu-id="adaa5-126">Frissítési csomag gyorsítótár</span><span class="sxs-lookup"><span data-stu-id="adaa5-126">Update package cache</span></span>

```bash
sudo apt-get -qq update
```

#### <a name="install-hello-latest-package-version"></a><span data-ttu-id="adaa5-127">Hello legújabb csomag verziójának telepítése</span><span class="sxs-lookup"><span data-stu-id="adaa5-127">Install hello latest package version</span></span>

```bash
sudo apt-get install waagent
```

#### <a name="enable-agent-auto-update"></a><span data-ttu-id="adaa5-128">Ügynök automatikus frissítésének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="adaa5-128">Enable agent auto update</span></span>
<span data-ttu-id="adaa5-129">Debian ezen verziója nem rendelkezik egy verzió > = 2.0.16, ezért nem áll rendelkezésre az automatikus frissítés.</span><span class="sxs-lookup"><span data-stu-id="adaa5-129">This version of Debian does not have a version >= 2.0.16, therefore AutoUpdate is not available for it.</span></span> <span data-ttu-id="adaa5-130">hello kimenetét a fenti parancs hello megtudhatja, hogy naprakész állapotban-e hello csomag.</span><span class="sxs-lookup"><span data-stu-id="adaa5-130">hello output from hello above command will show you if hello package is up-to-date.</span></span>

### <a name="debian-8-jessie--debian-9-stretch"></a><span data-ttu-id="adaa5-131">Debian 8 "Jessie" / "Stretch" Debian 9</span><span class="sxs-lookup"><span data-stu-id="adaa5-131">Debian 8 “Jessie” / Debian 9 “Stretch”</span></span>

#### <a name="check-your-current-package-version"></a><span data-ttu-id="adaa5-132">Ellenőrizze a csomag aktuális verziója</span><span class="sxs-lookup"><span data-stu-id="adaa5-132">Check your current package version</span></span>

```bash
apt list --installed | grep walinuxagent
```

#### <a name="update-package-cache"></a><span data-ttu-id="adaa5-133">Frissítési csomag gyorsítótár</span><span class="sxs-lookup"><span data-stu-id="adaa5-133">Update package cache</span></span>

```bash
sudo apt-get -qq update
```

#### <a name="install-hello-latest-package-version"></a><span data-ttu-id="adaa5-134">Hello legújabb csomag verziójának telepítése</span><span class="sxs-lookup"><span data-stu-id="adaa5-134">Install hello latest package version</span></span>

```bash
sudo apt-get install waagent
```
#### <a name="ensure-auto-update-is-enabled"></a><span data-ttu-id="adaa5-135">Győződjön meg róla, engedélyezve van az automatikus frissítés</span><span class="sxs-lookup"><span data-stu-id="adaa5-135">Ensure auto update is enabled</span></span> 

<span data-ttu-id="adaa5-136">Először ellenőrizze a toosee, ha engedélyezve van:</span><span class="sxs-lookup"><span data-stu-id="adaa5-136">First, check toosee if it is enabled:</span></span>

```bash
cat /etc/waagent.conf
```

<span data-ttu-id="adaa5-137">"AutoUpdate.Enabled" található.</span><span class="sxs-lookup"><span data-stu-id="adaa5-137">Find 'AutoUpdate.Enabled'.</span></span> <span data-ttu-id="adaa5-138">A kimenet jelenik meg, ha engedélyezve van:</span><span class="sxs-lookup"><span data-stu-id="adaa5-138">If you see this output, it is enabled:</span></span>

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

<span data-ttu-id="adaa5-139">Futtatás tooenable:</span><span class="sxs-lookup"><span data-stu-id="adaa5-139">tooenable run:</span></span>

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a><span data-ttu-id="adaa5-140">Hello waagent szolgáltatás újraindítása</span><span class="sxs-lookup"><span data-stu-id="adaa5-140">Restart hello waagent service</span></span>

```
sudo systemctl restart walinuxagent.service
```

## <a name="redhat--centos"></a><span data-ttu-id="adaa5-141">Redhat vagy CentOS</span><span class="sxs-lookup"><span data-stu-id="adaa5-141">Redhat / CentOS</span></span>

### <a name="rhelcentos-6"></a><span data-ttu-id="adaa5-142">RHEL/CentOS 6</span><span class="sxs-lookup"><span data-stu-id="adaa5-142">RHEL/CentOS 6</span></span>

#### <a name="check-your-current-package-version"></a><span data-ttu-id="adaa5-143">Ellenőrizze a csomag aktuális verziója</span><span class="sxs-lookup"><span data-stu-id="adaa5-143">Check your current package version</span></span>

```bash
sudo yum list WALinuxAgent
```

#### <a name="check-available-updates"></a><span data-ttu-id="adaa5-144">Elérhető frissítések ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="adaa5-144">Check available updates</span></span>

```bash
sudo yum check-update WALinuxAgent
```

#### <a name="install-hello-latest-package-version"></a><span data-ttu-id="adaa5-145">Hello legújabb csomag verziójának telepítése</span><span class="sxs-lookup"><span data-stu-id="adaa5-145">Install hello latest package version</span></span>

```bash
sudo yum install WALinuxAgent
```

#### <a name="ensure-auto-update-is-enabled"></a><span data-ttu-id="adaa5-146">Győződjön meg róla, engedélyezve van az automatikus frissítés</span><span class="sxs-lookup"><span data-stu-id="adaa5-146">Ensure auto update is enabled</span></span> 

<span data-ttu-id="adaa5-147">Először ellenőrizze a toosee, ha engedélyezve van:</span><span class="sxs-lookup"><span data-stu-id="adaa5-147">First, check toosee if it is enabled:</span></span>

```bash
cat /etc/waagent.conf
```

<span data-ttu-id="adaa5-148">"AutoUpdate.Enabled" található.</span><span class="sxs-lookup"><span data-stu-id="adaa5-148">Find 'AutoUpdate.Enabled'.</span></span> <span data-ttu-id="adaa5-149">A kimenet jelenik meg, ha engedélyezve van:</span><span class="sxs-lookup"><span data-stu-id="adaa5-149">If you see this output, it is enabled:</span></span>

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

<span data-ttu-id="adaa5-150">Futtatás tooenable:</span><span class="sxs-lookup"><span data-stu-id="adaa5-150">tooenable run:</span></span>

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a><span data-ttu-id="adaa5-151">Hello waagent szolgáltatás újraindítása</span><span class="sxs-lookup"><span data-stu-id="adaa5-151">Restart hello waagent service</span></span>

```
sudo service waagent restart
```

### <a name="rhelcentos-7"></a><span data-ttu-id="adaa5-152">RHEL/CentOS 7</span><span class="sxs-lookup"><span data-stu-id="adaa5-152">RHEL/CentOS 7</span></span>

#### <a name="check-your-current-package-version"></a><span data-ttu-id="adaa5-153">Ellenőrizze a csomag aktuális verziója</span><span class="sxs-lookup"><span data-stu-id="adaa5-153">Check your current package version</span></span>

```bash
sudo yum list WALinuxAgent
```

#### <a name="check-available-updates"></a><span data-ttu-id="adaa5-154">Elérhető frissítések ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="adaa5-154">Check available updates</span></span>

```bash
sudo yum check-update WALinuxAgent
```

#### <a name="install-hello-latest-package-version"></a><span data-ttu-id="adaa5-155">Hello legújabb csomag verziójának telepítése</span><span class="sxs-lookup"><span data-stu-id="adaa5-155">Install hello latest package version</span></span>

```bash
sudo yum install WALinuxAgent  
```

#### <a name="ensure-auto-update-is-enabled"></a><span data-ttu-id="adaa5-156">Győződjön meg róla, engedélyezve van az automatikus frissítés</span><span class="sxs-lookup"><span data-stu-id="adaa5-156">Ensure auto update is enabled</span></span> 

<span data-ttu-id="adaa5-157">Először ellenőrizze a toosee, ha engedélyezve van:</span><span class="sxs-lookup"><span data-stu-id="adaa5-157">First, check toosee if it is enabled:</span></span>

```bash
cat /etc/waagent.conf
```

<span data-ttu-id="adaa5-158">"AutoUpdate.Enabled" található.</span><span class="sxs-lookup"><span data-stu-id="adaa5-158">Find 'AutoUpdate.Enabled'.</span></span> <span data-ttu-id="adaa5-159">A kimenet jelenik meg, ha engedélyezve van:</span><span class="sxs-lookup"><span data-stu-id="adaa5-159">If you see this output, it is enabled:</span></span>

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

<span data-ttu-id="adaa5-160">Futtatás tooenable:</span><span class="sxs-lookup"><span data-stu-id="adaa5-160">tooenable run:</span></span>

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a><span data-ttu-id="adaa5-161">Hello waagent szolgáltatás újraindítása</span><span class="sxs-lookup"><span data-stu-id="adaa5-161">Restart hello waagent service</span></span>

```bash
sudo systemctl restart waagent.service
```

## <a name="suse-sles"></a><span data-ttu-id="adaa5-162">SUSE SLES</span><span class="sxs-lookup"><span data-stu-id="adaa5-162">SUSE SLES</span></span>

### <a name="suse-sles-11-sp4"></a><span data-ttu-id="adaa5-163">SUSE SLES 11 SP4</span><span class="sxs-lookup"><span data-stu-id="adaa5-163">SUSE SLES 11 SP4</span></span>

#### <a name="check-your-current-package-version"></a><span data-ttu-id="adaa5-164">Ellenőrizze a csomag aktuális verziója</span><span class="sxs-lookup"><span data-stu-id="adaa5-164">Check your current package version</span></span>

```bash
zypper info python-azure-agent
```

#### <a name="check-available-updates"></a><span data-ttu-id="adaa5-165">Elérhető frissítések ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="adaa5-165">Check available updates</span></span>

<span data-ttu-id="adaa5-166">hello feletti kimeneti megtudhatja, hogy hello csomag toodate be van-e.</span><span class="sxs-lookup"><span data-stu-id="adaa5-166">hello above output will show you if hello package is up toodate.</span></span>

#### <a name="install-hello-latest-package-version"></a><span data-ttu-id="adaa5-167">Hello legújabb csomag verziójának telepítése</span><span class="sxs-lookup"><span data-stu-id="adaa5-167">Install hello latest package version</span></span>

```bash
sudo zypper install python-azure-agent
```

#### <a name="ensure-auto-update-is-enabled"></a><span data-ttu-id="adaa5-168">Győződjön meg róla, engedélyezve van az automatikus frissítés</span><span class="sxs-lookup"><span data-stu-id="adaa5-168">Ensure auto update is enabled</span></span> 

<span data-ttu-id="adaa5-169">Először ellenőrizze a toosee, ha engedélyezve van:</span><span class="sxs-lookup"><span data-stu-id="adaa5-169">First, check toosee if it is enabled:</span></span>

```bash
cat /etc/waagent.conf
```

<span data-ttu-id="adaa5-170">"AutoUpdate.Enabled" található.</span><span class="sxs-lookup"><span data-stu-id="adaa5-170">Find 'AutoUpdate.Enabled'.</span></span> <span data-ttu-id="adaa5-171">A kimenet jelenik meg, ha engedélyezve van:</span><span class="sxs-lookup"><span data-stu-id="adaa5-171">If you see this output, it is enabled:</span></span>

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

<span data-ttu-id="adaa5-172">Futtatás tooenable:</span><span class="sxs-lookup"><span data-stu-id="adaa5-172">tooenable run:</span></span>

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a><span data-ttu-id="adaa5-173">Hello waagent szolgáltatás újraindítása</span><span class="sxs-lookup"><span data-stu-id="adaa5-173">Restart hello waagent service</span></span>

```bash
sudo /etc/init.d/waagent restart
```

### <a name="suse-sles-12-sp2"></a><span data-ttu-id="adaa5-174">SUSE SLES 12 SP2</span><span class="sxs-lookup"><span data-stu-id="adaa5-174">SUSE SLES 12 SP2</span></span>

#### <a name="check-your-current-package-version"></a><span data-ttu-id="adaa5-175">Ellenőrizze a csomag aktuális verziója</span><span class="sxs-lookup"><span data-stu-id="adaa5-175">Check your current package version</span></span>

```bash
zypper info python-azure-agent
```

#### <a name="check-available-updates"></a><span data-ttu-id="adaa5-176">Elérhető frissítések ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="adaa5-176">Check available updates</span></span>

<span data-ttu-id="adaa5-177">A fenti hello hello kimenete ez megtudhatja, ha hello csomag-e legfeljebb dátum.</span><span class="sxs-lookup"><span data-stu-id="adaa5-177">In hello output from hello above, this will show you if hello package is upto date.</span></span>

#### <a name="install-hello-latest-package-version"></a><span data-ttu-id="adaa5-178">Hello legújabb csomag verziójának telepítése</span><span class="sxs-lookup"><span data-stu-id="adaa5-178">Install hello latest package version</span></span>

```bash
sudo zypper install python-azure-agent
```

#### <a name="ensure-auto-update-is-enabled"></a><span data-ttu-id="adaa5-179">Győződjön meg róla, engedélyezve van az automatikus frissítés</span><span class="sxs-lookup"><span data-stu-id="adaa5-179">Ensure auto update is enabled</span></span> 

<span data-ttu-id="adaa5-180">Először ellenőrizze a toosee, ha engedélyezve van:</span><span class="sxs-lookup"><span data-stu-id="adaa5-180">First, check toosee if it is enabled:</span></span>

```bash
cat /etc/waagent.conf
```

<span data-ttu-id="adaa5-181">"AutoUpdate.Enabled" található.</span><span class="sxs-lookup"><span data-stu-id="adaa5-181">Find 'AutoUpdate.Enabled'.</span></span> <span data-ttu-id="adaa5-182">A kimenet jelenik meg, ha engedélyezve van:</span><span class="sxs-lookup"><span data-stu-id="adaa5-182">If you see this output, it is enabled:</span></span>

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

<span data-ttu-id="adaa5-183">Futtatás tooenable:</span><span class="sxs-lookup"><span data-stu-id="adaa5-183">tooenable run:</span></span>

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a><span data-ttu-id="adaa5-184">Hello waagent szolgáltatás újraindítása</span><span class="sxs-lookup"><span data-stu-id="adaa5-184">Restart hello waagent service</span></span>

```bash
sudo systemctl restart waagent.service
```

## <a name="oracle-6-and-7"></a><span data-ttu-id="adaa5-185">Oracle 6 és 7</span><span class="sxs-lookup"><span data-stu-id="adaa5-185">Oracle 6 and 7</span></span>

<span data-ttu-id="adaa5-186">Oracle Linux, győződjön meg arról, hogy hello `Addons` tárház engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="adaa5-186">For Oracle Linux, make sure that hello `Addons` repository is enabled.</span></span> <span data-ttu-id="adaa5-187">Válassza a tooedit hello fájl `/etc/yum.repos.d/public-yum-ol6.repo`(Oracle Linux 6) vagy `/etc/yum.repos.d/public-yum-ol7.repo`(Oracle Linux), és módosítsa a hello sor `enabled=0` túl`enabled=1` alatt **[ol6_addons]** vagy **[ol7_addons]** Ebben a fájlban.</span><span class="sxs-lookup"><span data-stu-id="adaa5-187">Choose tooedit hello file `/etc/yum.repos.d/public-yum-ol6.repo`(Oracle Linux 6) or `/etc/yum.repos.d/public-yum-ol7.repo`(Oracle Linux), and change hello line `enabled=0` too`enabled=1` under **[ol6_addons]** or **[ol7_addons]** in this file.</span></span>

<span data-ttu-id="adaa5-188">Ezt követően tooinstall hello legújabb verziójának hello Azure Linux ügynök, típus:</span><span class="sxs-lookup"><span data-stu-id="adaa5-188">Then, tooinstall hello latest version of hello Azure Linux Agent, type:</span></span>

```bash
sudo yum install WALinuxAgent
```

<span data-ttu-id="adaa5-189">Ha nem találja a hello bővítmény tárház egyszerűen hozzá ezeket a sorokat a .repo fájl tooyour Oracle Linux kiadás szerint hello végén:</span><span class="sxs-lookup"><span data-stu-id="adaa5-189">If you don't find hello add-on repository you can simply add these lines at hello end of your .repo file according tooyour Oracle Linux release:</span></span>

<span data-ttu-id="adaa5-190">Oracle Linux 6 virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="adaa5-190">For Oracle Linux 6 virtual machines:</span></span>

```sh
[ol6_addons]
name=Add-Ons for Oracle Linux $releasever ($basearch)
baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL6/addons/x86_64
gpgkey=http://public-yum.oracle.com/RPM-GPG-KEY-oracle-ol6
gpgcheck=1
enabled=1
```

<span data-ttu-id="adaa5-191">Oracle Linux 7 virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="adaa5-191">For Oracle Linux 7 virtual machines:</span></span>

```sh
[ol7_addons]
name=Oracle Linux $releasever Add ons ($basearch)
baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL7/addons/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=0
```

<span data-ttu-id="adaa5-192">Írja be:</span><span class="sxs-lookup"><span data-stu-id="adaa5-192">Then type:</span></span>

```bash
sudo yum update WALinuxAgent
```

<span data-ttu-id="adaa5-193">Ez általában szüksége, de ha valamilyen okból tooinstall kell azt közvetlenül, https://github.com használata hello a következő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="adaa5-193">Typically this is all you need, but if for some reason you need tooinstall it from https://github.com directly, use hello following steps.</span></span>


## <a name="update-hello-linux-agent-when-no-agent-package-exists-for-distribution"></a><span data-ttu-id="adaa5-194">Hello Linux ügynök frissítése, ha nincs ügynök csomag létezik a következő terjesztési</span><span class="sxs-lookup"><span data-stu-id="adaa5-194">Update hello Linux Agent when no agent package exists for distribution</span></span>

<span data-ttu-id="adaa5-195">Írja be a wget (van néhány disztribúciókkal, amely nem az alapértelmezés szerint telepítésre kerülnek, például a CentOS, Oracle Linux és a Redhat 6.4 és 6.5-ös verzió) telepítése `sudo yum install wget` hello parancssorban.</span><span class="sxs-lookup"><span data-stu-id="adaa5-195">Install wget (there are some distros that don't install it by default, such as Redhat, CentOS, and Oracle Linux versions 6.4 and 6.5) by typing `sudo yum install wget` on hello command line.</span></span>

### <a name="1-download-hello-latest-version"></a><span data-ttu-id="adaa5-196">1. Hello legújabb verzió letöltéséhez</span><span class="sxs-lookup"><span data-stu-id="adaa5-196">1. Download hello latest version</span></span>
<span data-ttu-id="adaa5-197">Nyissa meg [hello Azure Linux ügynök a Githubon történő kiadása](https://github.com/Azure/WALinuxAgent/releases) egy weblapot, és megtudhatja, hello legújabb verziószámot.</span><span class="sxs-lookup"><span data-stu-id="adaa5-197">Open [hello release of Azure Linux Agent in GitHub](https://github.com/Azure/WALinuxAgent/releases) in a web page, and find out hello latest version number.</span></span> <span data-ttu-id="adaa5-198">(Beírásával keresheti meg az aktuális verzió `waagent --version`.)</span><span class="sxs-lookup"><span data-stu-id="adaa5-198">(You can locate your current version by typing `waagent --version`.)</span></span>

#### <a name="for-version-22x-or-later-type"></a><span data-ttu-id="adaa5-199">A verzió 2.2.x vagy újabb, írja be:</span><span class="sxs-lookup"><span data-stu-id="adaa5-199">For version 2.2.x or later, type:</span></span>
```bash
wget https://github.com/Azure/WALinuxAgent/archive/v2.2.x.zip
unzip v2.2.x.zip.zip
cd WALinuxAgent-2.2.x
```

<span data-ttu-id="adaa5-200">hello következő verzióját használja 2.2.0 példa:</span><span class="sxs-lookup"><span data-stu-id="adaa5-200">hello following line uses version 2.2.0 as an example:</span></span>

```bash
wget https://github.com/Azure/WALinuxAgent/archive/v2.2.14.zip
unzip v2.2.14.zip  
cd WALinuxAgent-2.2.14
```

### <a name="2-install-hello-azure-linux-agent"></a><span data-ttu-id="adaa5-201">2. Hello Azure Linux ügynök telepítése</span><span class="sxs-lookup"><span data-stu-id="adaa5-201">2. Install hello Azure Linux Agent</span></span>

#### <a name="for-version-22x-use"></a><span data-ttu-id="adaa5-202">A verzió 2.2.x, használja:</span><span class="sxs-lookup"><span data-stu-id="adaa5-202">For version 2.2.x, use:</span></span>
<span data-ttu-id="adaa5-203">Előfordulhat, hogy tooinstall hello csomag `setuptools` először – lásd: [Itt](https://pypi.python.org/pypi/setuptools).</span><span class="sxs-lookup"><span data-stu-id="adaa5-203">You may need tooinstall hello package `setuptools` first--see [here](https://pypi.python.org/pypi/setuptools).</span></span> <span data-ttu-id="adaa5-204">Ezután futtassa:</span><span class="sxs-lookup"><span data-stu-id="adaa5-204">Then run:</span></span>

```bash
sudo python setup.py install
```

#### <a name="ensure-auto-update-is-enabled"></a><span data-ttu-id="adaa5-205">Győződjön meg róla, engedélyezve van az automatikus frissítés</span><span class="sxs-lookup"><span data-stu-id="adaa5-205">Ensure auto update is enabled</span></span>

<span data-ttu-id="adaa5-206">Először ellenőrizze a toosee, ha engedélyezve van:</span><span class="sxs-lookup"><span data-stu-id="adaa5-206">First, check toosee if it is enabled:</span></span>

```bash
cat /etc/waagent.conf
```

<span data-ttu-id="adaa5-207">"AutoUpdate.Enabled" található.</span><span class="sxs-lookup"><span data-stu-id="adaa5-207">Find 'AutoUpdate.Enabled'.</span></span> <span data-ttu-id="adaa5-208">A kimenet jelenik meg, ha engedélyezve van:</span><span class="sxs-lookup"><span data-stu-id="adaa5-208">If you see this output, it is enabled:</span></span>

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

<span data-ttu-id="adaa5-209">Futtatás tooenable:</span><span class="sxs-lookup"><span data-stu-id="adaa5-209">tooenable run:</span></span>

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="3-restart-hello-waagent-service"></a><span data-ttu-id="adaa5-210">3. Hello waagent szolgáltatás újraindítása</span><span class="sxs-lookup"><span data-stu-id="adaa5-210">3. Restart hello waagent service</span></span>
<span data-ttu-id="adaa5-211">A legtöbb Linux disztribúciókkal:</span><span class="sxs-lookup"><span data-stu-id="adaa5-211">For most of Linux distros:</span></span>

```bash
sudo service waagent restart
```

<span data-ttu-id="adaa5-212">Ubuntu használja:</span><span class="sxs-lookup"><span data-stu-id="adaa5-212">For Ubuntu, use:</span></span>

```bash
sudo service walinuxagent restart
```

<span data-ttu-id="adaa5-213">A CoreOS használjon:</span><span class="sxs-lookup"><span data-stu-id="adaa5-213">For CoreOS, use:</span></span>

```bash
sudo systemctl restart waagent
```

### <a name="4-confirm-hello-azure-linux-agent-version"></a><span data-ttu-id="adaa5-214">4. Erősítse meg a hello Azure Linux ügynök verziója</span><span class="sxs-lookup"><span data-stu-id="adaa5-214">4. Confirm hello Azure Linux Agent version</span></span>
    
```bash
waagent -version
```

<span data-ttu-id="adaa5-215">CoreOS, a fenti parancs hello nem működnek.</span><span class="sxs-lookup"><span data-stu-id="adaa5-215">For CoreOS, hello above command may not work.</span></span>

<span data-ttu-id="adaa5-216">Látni fogja, hogy hello Azure Linux-ügynök verziója lett frissítve toohello új verziója.</span><span class="sxs-lookup"><span data-stu-id="adaa5-216">You will see that hello Azure Linux Agent version has been updated toohello new version.</span></span>

<span data-ttu-id="adaa5-217">Hello Azure Linux ügynök kapcsolatos további információkért lásd: [Azure Linux ügynök információs](https://github.com/Azure/WALinuxAgent).</span><span class="sxs-lookup"><span data-stu-id="adaa5-217">For more information regarding hello Azure Linux Agent, see [Azure Linux Agent README](https://github.com/Azure/WALinuxAgent).</span></span>
