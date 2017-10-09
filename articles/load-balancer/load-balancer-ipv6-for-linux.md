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
# <a name="configuring-dhcpv6-for-linux-vms"></a>A DHCPv6 konfigurálása Linux rendszerű virtuális gépekhez

Hello Azure piactér hello Linux virtuálisgép-lemezképeket nem rendelkeznek DHCPv6 alapértelmezés szerint konfigurálva. toosupport, IPv6 és DHCPv6 hello Linux operációs rendszer telepítési Ön által használt belül meg kell adni. Különböző Linux terjesztések átviteli DHCPv6 konfigurálása, mert használnak a különböző csomagok különböző módokat rendelkezik.

> [!NOTE]
> Hello Azure piactér legutóbbi SUSE Linux és a CoreOS képek előre konfigurált a DHCPv6 törölték. Nincs további módosítások szükségesek, ha használja ezeket a képeket.

Ez a dokumentum ismerteti, hogyan tooenable DHCPv6, hogy a Linux virtuális gép IPv6-címet kap.

> [!WARNING]
> Helytelen a hálózati konfigurációs fájlok szerkesztésével, akkor toolose hálózati hozzáférési tooyour VM okozhat. Azt javasoljuk, hogy tesztelje a konfigurációs módosítások nem éles rendszerek esetén. Ez a cikk utasításait hello hello hello Linux képek hello Azure piactér legújabb verziói lettek tesztelve. Hello dokumentációjában talál részletes ismertetését Linux az adott verziójához.

## <a name="ubuntu"></a>Ubuntu

1. Hello fájl szerkesztésével `/etc/dhcp/dhclient6.conf` , és adja hozzá a következő sor hello:

        timeout 10;

2. Hello hálózati konfiguráció hello eth0 csatoló Szerkesztés a konfiguráció a következő hello:

   * A **Ubuntu 12.04 és 14.04**, hello fájl szerkesztése`/etc/network/interfaces.d/eth0.cfg`
   * A **Ubuntu 16.04**, hello fájl szerkesztése`/etc/network/interfaces.d/50-cloud-init.cfg`

         iface eth0 inet6 auto
             up sleep 5
             up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. Újítsa meg az IPv6-cím:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="debian"></a>Debian

1. Hello fájl szerkesztésével `/etc/dhcp/dhclient6.conf` , és adja hozzá a következő sor hello:

        timeout 10;

2. Hello fájl szerkesztésével `/etc/network/interfaces` , és adja hozzá a következő konfigurációs hello:

        iface eth0 inet6 auto
            up sleep 5
            up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. Újítsa meg az IPv6-cím:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="rhel--centos--oracle-linux"></a>RHEL / CentOS / Oracle Linux

1. Hello fájl szerkesztésével `/etc/sysconfig/network` , és adja hozzá a következő paraméter hello:

        NETWORKING_IPV6=yes

2. Hello fájl szerkesztésével `/etc/sysconfig/network-scripts/ifcfg-eth0` , és adja hozzá a következő két paraméter hello:

        IPV6INIT=yes
        DHCPV6C=yes

3. Újítsa meg az IPv6-cím:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-11--opensuse-13"></a>SLES 11 & openSUSE 13

Az Azure-ban legutóbbi SLES és openSUSE képek előre konfigurált a DHCPv6 törölték. Nincs további módosítások szükségesek, ha használja ezeket a képeket. Ha egy virtuális Gépet egy korábbi vagy egyéni SUSE lemezképen alapuló, használja a hello a következő lépéseket:

1. Telepítse a hello `dhcp-client` csomagot, ha szükséges:

    ```bash
    sudo zypper install dhcp-client
    ```

2. Hello fájl szerkesztésével `/etc/sysconfig/network/ifcfg-eth0` , és adja hozzá a következő paraméter hello:

        DHCLIENT6_MODE='managed'

3. Újítsa meg a hello IPv6-cím:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-12-and-opensuse-leap"></a>SLES 12 és openSUSE termékek

Az Azure-ban legutóbbi SLES és openSUSE képek előre konfigurált a DHCPv6 törölték. Nincs további módosítások szükségesek, ha használja ezeket a képeket. Ha egy virtuális Gépet egy korábbi vagy egyéni SUSE lemezképen alapuló, használja a hello a következő lépéseket:

1. Hello fájl szerkesztésével `/etc/sysconfig/network/ifcfg-eth0` , és cserélje le ezt a paramétert

        #BOOTPROTO='dhcp4'

    az érték a következő hello:

        BOOTPROTO='dhcp'

2. Adja hozzá a következő paraméternek túl hello`/etc/sysconfig/network/ifcfg-eth0`:

        DHCLIENT6_MODE='managed'

3. Újítsa meg a hello IPv6-cím:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="coreos"></a>CoreOS

Az Azure-ban legutóbbi CoreOS képek előre konfigurált a DHCPv6 törölték. Nincs további módosítások szükségesek, ha használja ezeket a képeket. Ha egy virtuális Gépet egy korábbi vagy egyéni CoreOS lemezképen alapuló, használja a hello a következő lépéseket:

1. Hello fájl szerkesztése`/etc/systemd/network/10_dhcp.network`

        [Match]
        eth0

        [Network]
        DHCP=ipv6

2. Újítsa meg a hello IPv6-cím:

    ```bash
    sudo systemctl restart systemd-networkd
    ```
