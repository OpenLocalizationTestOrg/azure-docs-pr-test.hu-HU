---
title: "Linux virtuális gépek DHCPv6 aaaConfiguring |} Microsoft Docs"
description: "Hogyan tooconfigure DHCPv6 Linux virtuális gépekhez."
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
ms.openlocfilehash: abd5a98c3496b189946f59bab1d9c20dcd0aa2c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-dhcpv6-for-linux-vms"></a><span data-ttu-id="78594-104">A DHCPv6 konfigurálása Linux rendszerű virtuális gépekhez</span><span class="sxs-lookup"><span data-stu-id="78594-104">Configuring DHCPv6 for Linux VMs</span></span>

<span data-ttu-id="78594-105">Hello Azure piactér hello Linux virtuálisgép-lemezképeket nem rendelkeznek DHCPv6 alapértelmezés szerint konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="78594-105">Some of hello Linux virtual machine images in hello Azure Marketplace do not have DHCPv6 configured by default.</span></span> <span data-ttu-id="78594-106">toosupport, IPv6 és DHCPv6 hello Linux operációs rendszer telepítési Ön által használt belül meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="78594-106">toosupport IPv6, DHCPv6 must be configured in within hello Linux OS distribution that you are using.</span></span> <span data-ttu-id="78594-107">Különböző Linux terjesztések átviteli DHCPv6 konfigurálása, mert használnak a különböző csomagok különböző módokat rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="78594-107">Different Linux distributions have different ways of configuring DHCPv6 because they use different packages.</span></span>

> [!NOTE]
> <span data-ttu-id="78594-108">Hello Azure piactér legutóbbi SUSE Linux és a CoreOS képek előre konfigurált a DHCPv6 törölték.</span><span class="sxs-lookup"><span data-stu-id="78594-108">Recent SUSE Linux and CoreOS images in hello Azure Marketplace have been pre-configured with DHCPv6.</span></span> <span data-ttu-id="78594-109">Nincs további módosítások szükségesek, ha használja ezeket a képeket.</span><span class="sxs-lookup"><span data-stu-id="78594-109">No additional changes are required when using those images.</span></span>

<span data-ttu-id="78594-110">Ez a dokumentum ismerteti, hogyan tooenable DHCPv6, hogy a Linux virtuális gép IPv6-címet kap.</span><span class="sxs-lookup"><span data-stu-id="78594-110">This document describes how tooenable DHCPv6 so that your Linux virtual machine obtains an IPv6 address.</span></span>

> [!WARNING]
> <span data-ttu-id="78594-111">Helytelen a hálózati konfigurációs fájlok szerkesztésével, akkor toolose hálózati hozzáférési tooyour VM okozhat.</span><span class="sxs-lookup"><span data-stu-id="78594-111">Improperly editing network configuration files can cause you toolose network access tooyour VM.</span></span> <span data-ttu-id="78594-112">Azt javasoljuk, hogy tesztelje a konfigurációs módosítások nem éles rendszerek esetén.</span><span class="sxs-lookup"><span data-stu-id="78594-112">We recommended that you test your configuration changes on non-production systems.</span></span> <span data-ttu-id="78594-113">Ez a cikk utasításait hello hello hello Linux képek hello Azure piactér legújabb verziói lettek tesztelve.</span><span class="sxs-lookup"><span data-stu-id="78594-113">hello instructions in this article have been tested on hello latest versions of hello Linux images in hello Azure Marketplace.</span></span> <span data-ttu-id="78594-114">Hello dokumentációjában talál részletes ismertetését Linux az adott verziójához.</span><span class="sxs-lookup"><span data-stu-id="78594-114">Consult hello documentation for your specific version of Linux for more detailed instructions.</span></span>

## <a name="ubuntu"></a><span data-ttu-id="78594-115">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="78594-115">Ubuntu</span></span>

1. <span data-ttu-id="78594-116">Hello fájl szerkesztésével `/etc/dhcp/dhclient6.conf` , és adja hozzá a következő sor hello:</span><span class="sxs-lookup"><span data-stu-id="78594-116">Edit hello file `/etc/dhcp/dhclient6.conf` and add hello following line:</span></span>

        timeout 10;

2. <span data-ttu-id="78594-117">Hello hálózati konfiguráció hello eth0 csatoló Szerkesztés a konfiguráció a következő hello:</span><span class="sxs-lookup"><span data-stu-id="78594-117">Edit hello network configuration for hello eth0 interface with hello following configuration:</span></span>

   * <span data-ttu-id="78594-118">A **Ubuntu 12.04 és 14.04**, hello fájl szerkesztése`/etc/network/interfaces.d/eth0.cfg`</span><span class="sxs-lookup"><span data-stu-id="78594-118">On **Ubuntu 12.04 and 14.04**, edit hello file `/etc/network/interfaces.d/eth0.cfg`</span></span>
   * <span data-ttu-id="78594-119">A **Ubuntu 16.04**, hello fájl szerkesztése`/etc/network/interfaces.d/50-cloud-init.cfg`</span><span class="sxs-lookup"><span data-stu-id="78594-119">On **Ubuntu 16.04**, edit hello file `/etc/network/interfaces.d/50-cloud-init.cfg`</span></span>

         iface eth0 inet6 auto
             up sleep 5
             up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. <span data-ttu-id="78594-120">Újítsa meg az IPv6-cím:</span><span class="sxs-lookup"><span data-stu-id="78594-120">Renew IPv6 address:</span></span>

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="debian"></a><span data-ttu-id="78594-121">Debian</span><span class="sxs-lookup"><span data-stu-id="78594-121">Debian</span></span>

1. <span data-ttu-id="78594-122">Hello fájl szerkesztésével `/etc/dhcp/dhclient6.conf` , és adja hozzá a következő sor hello:</span><span class="sxs-lookup"><span data-stu-id="78594-122">Edit hello file `/etc/dhcp/dhclient6.conf` and add hello following line:</span></span>

        timeout 10;

2. <span data-ttu-id="78594-123">Hello fájl szerkesztésével `/etc/network/interfaces` , és adja hozzá a következő konfigurációs hello:</span><span class="sxs-lookup"><span data-stu-id="78594-123">Edit hello file `/etc/network/interfaces` and add hello following configuration:</span></span>

        iface eth0 inet6 auto
            up sleep 5
            up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. <span data-ttu-id="78594-124">Újítsa meg az IPv6-cím:</span><span class="sxs-lookup"><span data-stu-id="78594-124">Renew IPv6 address:</span></span>

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="rhel--centos--oracle-linux"></a><span data-ttu-id="78594-125">RHEL / CentOS / Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="78594-125">RHEL / CentOS / Oracle Linux</span></span>

1. <span data-ttu-id="78594-126">Hello fájl szerkesztésével `/etc/sysconfig/network` , és adja hozzá a következő paraméter hello:</span><span class="sxs-lookup"><span data-stu-id="78594-126">Edit hello file `/etc/sysconfig/network` and add hello following parameter:</span></span>

        NETWORKING_IPV6=yes

2. <span data-ttu-id="78594-127">Hello fájl szerkesztésével `/etc/sysconfig/network-scripts/ifcfg-eth0` , és adja hozzá a következő két paraméter hello:</span><span class="sxs-lookup"><span data-stu-id="78594-127">Edit hello file `/etc/sysconfig/network-scripts/ifcfg-eth0` and add hello following two parameters:</span></span>

        IPV6INIT=yes
        DHCPV6C=yes

3. <span data-ttu-id="78594-128">Újítsa meg az IPv6-cím:</span><span class="sxs-lookup"><span data-stu-id="78594-128">Renew IPv6 address:</span></span>

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-11--opensuse-13"></a><span data-ttu-id="78594-129">SLES 11 & openSUSE 13</span><span class="sxs-lookup"><span data-stu-id="78594-129">SLES 11 & openSUSE 13</span></span>

<span data-ttu-id="78594-130">Az Azure-ban legutóbbi SLES és openSUSE képek előre konfigurált a DHCPv6 törölték.</span><span class="sxs-lookup"><span data-stu-id="78594-130">Recent SLES and openSUSE images in Azure have been pre-configured with DHCPv6.</span></span> <span data-ttu-id="78594-131">Nincs további módosítások szükségesek, ha használja ezeket a képeket.</span><span class="sxs-lookup"><span data-stu-id="78594-131">No additional changes are required when using those images.</span></span> <span data-ttu-id="78594-132">Ha egy virtuális Gépet egy korábbi vagy egyéni SUSE lemezképen alapuló, használja a hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="78594-132">If you have a VM based on an older or custom SUSE image, use hello following steps:</span></span>

1. <span data-ttu-id="78594-133">Telepítse a hello `dhcp-client` csomagot, ha szükséges:</span><span class="sxs-lookup"><span data-stu-id="78594-133">Install hello `dhcp-client` package, if needed:</span></span>

    ```bash
    sudo zypper install dhcp-client
    ```

2. <span data-ttu-id="78594-134">Hello fájl szerkesztésével `/etc/sysconfig/network/ifcfg-eth0` , és adja hozzá a következő paraméter hello:</span><span class="sxs-lookup"><span data-stu-id="78594-134">Edit hello file `/etc/sysconfig/network/ifcfg-eth0` and add hello following parameter:</span></span>

        DHCLIENT6_MODE='managed'

3. <span data-ttu-id="78594-135">Újítsa meg a hello IPv6-cím:</span><span class="sxs-lookup"><span data-stu-id="78594-135">Renew hello IPv6 address:</span></span>

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-12-and-opensuse-leap"></a><span data-ttu-id="78594-136">SLES 12 és openSUSE termékek</span><span class="sxs-lookup"><span data-stu-id="78594-136">SLES 12 and openSUSE Leap</span></span>

<span data-ttu-id="78594-137">Az Azure-ban legutóbbi SLES és openSUSE képek előre konfigurált a DHCPv6 törölték.</span><span class="sxs-lookup"><span data-stu-id="78594-137">Recent SLES and openSUSE images in Azure have been pre-configured with DHCPv6.</span></span> <span data-ttu-id="78594-138">Nincs további módosítások szükségesek, ha használja ezeket a képeket.</span><span class="sxs-lookup"><span data-stu-id="78594-138">No additional changes are required when using those images.</span></span> <span data-ttu-id="78594-139">Ha egy virtuális Gépet egy korábbi vagy egyéni SUSE lemezképen alapuló, használja a hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="78594-139">If you have a VM based on an older or custom SUSE image, use hello following steps:</span></span>

1. <span data-ttu-id="78594-140">Hello fájl szerkesztésével `/etc/sysconfig/network/ifcfg-eth0` , és cserélje le ezt a paramétert</span><span class="sxs-lookup"><span data-stu-id="78594-140">Edit hello file `/etc/sysconfig/network/ifcfg-eth0` and replace this parameter</span></span>

        #BOOTPROTO='dhcp4'

    <span data-ttu-id="78594-141">az érték a következő hello:</span><span class="sxs-lookup"><span data-stu-id="78594-141">with hello following value:</span></span>

        BOOTPROTO='dhcp'

2. <span data-ttu-id="78594-142">Adja hozzá a következő paraméternek túl hello`/etc/sysconfig/network/ifcfg-eth0`:</span><span class="sxs-lookup"><span data-stu-id="78594-142">Add hello following parameter too`/etc/sysconfig/network/ifcfg-eth0`:</span></span>

        DHCLIENT6_MODE='managed'

3. <span data-ttu-id="78594-143">Újítsa meg a hello IPv6-cím:</span><span class="sxs-lookup"><span data-stu-id="78594-143">Renew hello IPv6 address:</span></span>

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="coreos"></a><span data-ttu-id="78594-144">CoreOS</span><span class="sxs-lookup"><span data-stu-id="78594-144">CoreOS</span></span>

<span data-ttu-id="78594-145">Az Azure-ban legutóbbi CoreOS képek előre konfigurált a DHCPv6 törölték.</span><span class="sxs-lookup"><span data-stu-id="78594-145">Recent CoreOS images in Azure have been pre-configured with DHCPv6.</span></span> <span data-ttu-id="78594-146">Nincs további módosítások szükségesek, ha használja ezeket a képeket.</span><span class="sxs-lookup"><span data-stu-id="78594-146">No additional changes are required when using those images.</span></span> <span data-ttu-id="78594-147">Ha egy virtuális Gépet egy korábbi vagy egyéni CoreOS lemezképen alapuló, használja a hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="78594-147">If you have a VM based on an older or custom CoreOS image, use hello following steps:</span></span>

1. <span data-ttu-id="78594-148">Hello fájl szerkesztése`/etc/systemd/network/10_dhcp.network`</span><span class="sxs-lookup"><span data-stu-id="78594-148">Edit hello file `/etc/systemd/network/10_dhcp.network`</span></span>

        [Match]
        eth0

        [Network]
        DHCP=ipv6

2. <span data-ttu-id="78594-149">Újítsa meg a hello IPv6-cím:</span><span class="sxs-lookup"><span data-stu-id="78594-149">Renew hello IPv6 address:</span></span>

    ```bash
    sudo systemctl restart systemd-networkd
    ```
