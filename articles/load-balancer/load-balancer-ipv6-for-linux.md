---
title: "Linux virtuális gépek DHCPv6 konfigurálása |} Microsoft Docs"
description: "Hogyan kell konfigurálni a DHCPv6 Linux virtuális gépekhez."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
keywords: "IPv6-alapú, azure load balancer, kettős verem, nyilvános IP-cím, natív ipv6, mobil, iot"
ms.assetid: b32719b6-00e8-4cd0-ba7f-e60e8146084b
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2016
ms.author: kumud
ms.openlocfilehash: 5c591e7f1838c86ca74caea9dd3a5e8f874fd8a7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configuring-dhcpv6-for-linux-vms"></a><span data-ttu-id="22127-104">A DHCPv6 konfigurálása Linux rendszerű virtuális gépekhez</span><span class="sxs-lookup"><span data-stu-id="22127-104">Configuring DHCPv6 for Linux VMs</span></span>

<span data-ttu-id="22127-105">Egyes Linux virtuális gép képek az Azure piactéren DHCPv6 alapértelmezés szerint nem rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="22127-105">Some of the Linux virtual machine images in the Azure Marketplace do not have DHCPv6 configured by default.</span></span> <span data-ttu-id="22127-106">IPv6 támogatása érdekében DHCPv6 meg kell adni az Ön által használt Linux operációs rendszert futtató terjesztési belül.</span><span class="sxs-lookup"><span data-stu-id="22127-106">To support IPv6, DHCPv6 must be configured in within the Linux OS distribution that you are using.</span></span> <span data-ttu-id="22127-107">Különböző Linux terjesztések átviteli DHCPv6 konfigurálása, mert használnak a különböző csomagok különböző módokat rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="22127-107">Different Linux distributions have different ways of configuring DHCPv6 because they use different packages.</span></span>

> [!NOTE]
> <span data-ttu-id="22127-108">Az Azure piactéren legutóbbi SUSE Linux és a CoreOS képek előre konfigurált a DHCPv6 törölték.</span><span class="sxs-lookup"><span data-stu-id="22127-108">Recent SUSE Linux and CoreOS images in the Azure Marketplace have been pre-configured with DHCPv6.</span></span> <span data-ttu-id="22127-109">Nincs további módosítások szükségesek, ha használja ezeket a képeket.</span><span class="sxs-lookup"><span data-stu-id="22127-109">No additional changes are required when using those images.</span></span>

<span data-ttu-id="22127-110">Ez a dokumentum ismerteti a DHCPv6 engedélyezése, hogy a Linux virtuális gép IPv6-címet kap.</span><span class="sxs-lookup"><span data-stu-id="22127-110">This document describes how to enable DHCPv6 so that your Linux virtual machine obtains an IPv6 address.</span></span>

> [!WARNING]
> <span data-ttu-id="22127-111">Helytelen a hálózati konfigurációs fájlok szerkesztésével okozhat, hogy a virtuális gép hálózati megszakadna.</span><span class="sxs-lookup"><span data-stu-id="22127-111">Improperly editing network configuration files can cause you to lose network access to your VM.</span></span> <span data-ttu-id="22127-112">Azt javasoljuk, hogy tesztelje a konfigurációs módosítások nem éles rendszerek esetén.</span><span class="sxs-lookup"><span data-stu-id="22127-112">We recommended that you test your configuration changes on non-production systems.</span></span> <span data-ttu-id="22127-113">A jelen cikkben lévő utasítások a Linux-lemezképek, az Azure piactéren legújabb verziói lettek tesztelve.</span><span class="sxs-lookup"><span data-stu-id="22127-113">The instructions in this article have been tested on the latest versions of the Linux images in the Azure Marketplace.</span></span> <span data-ttu-id="22127-114">Részletes útmutatás Linux adott verziójának dokumentációjában.</span><span class="sxs-lookup"><span data-stu-id="22127-114">Consult the documentation for your specific version of Linux for more detailed instructions.</span></span>

## <a name="ubuntu"></a><span data-ttu-id="22127-115">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="22127-115">Ubuntu</span></span>

1. <span data-ttu-id="22127-116">A fájl szerkesztése `/etc/dhcp/dhclient6.conf` és adja hozzá a következő sort:</span><span class="sxs-lookup"><span data-stu-id="22127-116">Edit the file `/etc/dhcp/dhclient6.conf` and add the following line:</span></span>

        timeout 10;

2. <span data-ttu-id="22127-117">Szerkessze a hálózati konfigurációt a eth0 kapcsolat a következő beállításokkal:</span><span class="sxs-lookup"><span data-stu-id="22127-117">Edit the network configuration for the eth0 interface with the following configuration:</span></span>

   * <span data-ttu-id="22127-118">A **Ubuntu 12.04 és 14.04**, a fájl szerkesztése`/etc/network/interfaces.d/eth0.cfg`</span><span class="sxs-lookup"><span data-stu-id="22127-118">On **Ubuntu 12.04 and 14.04**, edit the file `/etc/network/interfaces.d/eth0.cfg`</span></span>
   * <span data-ttu-id="22127-119">A **Ubuntu 16.04**, a fájl szerkesztése`/etc/network/interfaces.d/50-cloud-init.cfg`</span><span class="sxs-lookup"><span data-stu-id="22127-119">On **Ubuntu 16.04**, edit the file `/etc/network/interfaces.d/50-cloud-init.cfg`</span></span>

         iface eth0 inet6 auto
             up sleep 5
             up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. <span data-ttu-id="22127-120">Újítsa meg az IPv6-cím:</span><span class="sxs-lookup"><span data-stu-id="22127-120">Renew IPv6 address:</span></span>

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="debian"></a><span data-ttu-id="22127-121">Debian</span><span class="sxs-lookup"><span data-stu-id="22127-121">Debian</span></span>

1. <span data-ttu-id="22127-122">A fájl szerkesztése `/etc/dhcp/dhclient6.conf` és adja hozzá a következő sort:</span><span class="sxs-lookup"><span data-stu-id="22127-122">Edit the file `/etc/dhcp/dhclient6.conf` and add the following line:</span></span>

        timeout 10;

2. <span data-ttu-id="22127-123">A fájl szerkesztése `/etc/network/interfaces` , és adja hozzá a következő konfigurációt:</span><span class="sxs-lookup"><span data-stu-id="22127-123">Edit the file `/etc/network/interfaces` and add the following configuration:</span></span>

        iface eth0 inet6 auto
            up sleep 5
            up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. <span data-ttu-id="22127-124">Újítsa meg az IPv6-cím:</span><span class="sxs-lookup"><span data-stu-id="22127-124">Renew IPv6 address:</span></span>

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="rhel--centos--oracle-linux"></a><span data-ttu-id="22127-125">RHEL / CentOS / Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="22127-125">RHEL / CentOS / Oracle Linux</span></span>

1. <span data-ttu-id="22127-126">A fájl szerkesztése `/etc/sysconfig/network` és adja hozzá a következő paramétert:</span><span class="sxs-lookup"><span data-stu-id="22127-126">Edit the file `/etc/sysconfig/network` and add the following parameter:</span></span>

        NETWORKING_IPV6=yes

2. <span data-ttu-id="22127-127">A fájl szerkesztése `/etc/sysconfig/network-scripts/ifcfg-eth0` , és adja hozzá az alábbi két paramétert:</span><span class="sxs-lookup"><span data-stu-id="22127-127">Edit the file `/etc/sysconfig/network-scripts/ifcfg-eth0` and add the following two parameters:</span></span>

        IPV6INIT=yes
        DHCPV6C=yes

3. <span data-ttu-id="22127-128">Újítsa meg az IPv6-cím:</span><span class="sxs-lookup"><span data-stu-id="22127-128">Renew IPv6 address:</span></span>

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-11--opensuse-13"></a><span data-ttu-id="22127-129">SLES 11 & openSUSE 13</span><span class="sxs-lookup"><span data-stu-id="22127-129">SLES 11 & openSUSE 13</span></span>

<span data-ttu-id="22127-130">Az Azure-ban legutóbbi SLES és openSUSE képek előre konfigurált a DHCPv6 törölték.</span><span class="sxs-lookup"><span data-stu-id="22127-130">Recent SLES and openSUSE images in Azure have been pre-configured with DHCPv6.</span></span> <span data-ttu-id="22127-131">Nincs további módosítások szükségesek, ha használja ezeket a képeket.</span><span class="sxs-lookup"><span data-stu-id="22127-131">No additional changes are required when using those images.</span></span> <span data-ttu-id="22127-132">Ha egy virtuális Gépet egy korábbi vagy egyéni SUSE lemezképen alapuló, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="22127-132">If you have a VM based on an older or custom SUSE image, use the following steps:</span></span>

1. <span data-ttu-id="22127-133">Telepítse a `dhcp-client` csomagot, ha szükséges:</span><span class="sxs-lookup"><span data-stu-id="22127-133">Install the `dhcp-client` package, if needed:</span></span>

    ```bash
    sudo zypper install dhcp-client
    ```

2. <span data-ttu-id="22127-134">A fájl szerkesztése `/etc/sysconfig/network/ifcfg-eth0` és adja hozzá a következő paramétert:</span><span class="sxs-lookup"><span data-stu-id="22127-134">Edit the file `/etc/sysconfig/network/ifcfg-eth0` and add the following parameter:</span></span>

        DHCLIENT6_MODE='managed'

3. <span data-ttu-id="22127-135">Újítsa meg az IPv6-cím:</span><span class="sxs-lookup"><span data-stu-id="22127-135">Renew the IPv6 address:</span></span>

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-12-and-opensuse-leap"></a><span data-ttu-id="22127-136">SLES 12 és openSUSE termékek</span><span class="sxs-lookup"><span data-stu-id="22127-136">SLES 12 and openSUSE Leap</span></span>

<span data-ttu-id="22127-137">Az Azure-ban legutóbbi SLES és openSUSE képek előre konfigurált a DHCPv6 törölték.</span><span class="sxs-lookup"><span data-stu-id="22127-137">Recent SLES and openSUSE images in Azure have been pre-configured with DHCPv6.</span></span> <span data-ttu-id="22127-138">Nincs további módosítások szükségesek, ha használja ezeket a képeket.</span><span class="sxs-lookup"><span data-stu-id="22127-138">No additional changes are required when using those images.</span></span> <span data-ttu-id="22127-139">Ha egy virtuális Gépet egy korábbi vagy egyéni SUSE lemezképen alapuló, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="22127-139">If you have a VM based on an older or custom SUSE image, use the following steps:</span></span>

1. <span data-ttu-id="22127-140">A fájl szerkesztése `/etc/sysconfig/network/ifcfg-eth0` , és cserélje le ezt a paramétert</span><span class="sxs-lookup"><span data-stu-id="22127-140">Edit the file `/etc/sysconfig/network/ifcfg-eth0` and replace this parameter</span></span>

        #BOOTPROTO='dhcp4'

    <span data-ttu-id="22127-141">a következő értékkel:</span><span class="sxs-lookup"><span data-stu-id="22127-141">with the following value:</span></span>

        BOOTPROTO='dhcp'

2. <span data-ttu-id="22127-142">Adja hozzá a következő paraméter `/etc/sysconfig/network/ifcfg-eth0`:</span><span class="sxs-lookup"><span data-stu-id="22127-142">Add the following parameter to `/etc/sysconfig/network/ifcfg-eth0`:</span></span>

        DHCLIENT6_MODE='managed'

3. <span data-ttu-id="22127-143">Újítsa meg az IPv6-cím:</span><span class="sxs-lookup"><span data-stu-id="22127-143">Renew the IPv6 address:</span></span>

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="coreos"></a><span data-ttu-id="22127-144">CoreOS</span><span class="sxs-lookup"><span data-stu-id="22127-144">CoreOS</span></span>

<span data-ttu-id="22127-145">Az Azure-ban legutóbbi CoreOS képek előre konfigurált a DHCPv6 törölték.</span><span class="sxs-lookup"><span data-stu-id="22127-145">Recent CoreOS images in Azure have been pre-configured with DHCPv6.</span></span> <span data-ttu-id="22127-146">Nincs további módosítások szükségesek, ha használja ezeket a képeket.</span><span class="sxs-lookup"><span data-stu-id="22127-146">No additional changes are required when using those images.</span></span> <span data-ttu-id="22127-147">Ha egy virtuális Gépet egy korábbi vagy egyéni CoreOS lemezképen alapuló, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="22127-147">If you have a VM based on an older or custom CoreOS image, use the following steps:</span></span>

1. <span data-ttu-id="22127-148">A fájl szerkesztése`/etc/systemd/network/10_dhcp.network`</span><span class="sxs-lookup"><span data-stu-id="22127-148">Edit the file `/etc/systemd/network/10_dhcp.network`</span></span>

        [Match]
        eth0

        [Network]
        DHCP=ipv6

2. <span data-ttu-id="22127-149">Újítsa meg az IPv6-cím:</span><span class="sxs-lookup"><span data-stu-id="22127-149">Renew the IPv6 address:</span></span>

    ```bash
    sudo systemctl restart systemd-networkd
    ```
