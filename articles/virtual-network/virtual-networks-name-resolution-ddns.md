---
title: "Dinamikus DNS tooregister állomásnevek aaaUsing"
description: "Ez a lap részleteket nyújt hogyan tooset be dinamikus DNS tooregister állomásnevek a saját DNS-kiszolgálókat."
services: dns
documentationcenter: na
author: GarethBradshawMSFT
manager: timlt
editor: 
ms.assetid: c315961a-fa33-45cf-82b9-4551e70d32dd
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2017
ms.author: garbrad
ms.openlocfilehash: 8d4b44265714e6976f26bfb3446e8101aa70996a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-dynamic-dns-tooregister-hostnames-in-your-own-dns-server"></a><span data-ttu-id="846ec-103">Dinamikus DNS tooregister állomásnevek a saját DNS-kiszolgáló használatával</span><span class="sxs-lookup"><span data-stu-id="846ec-103">Using Dynamic DNS tooregister hostnames in your own DNS server</span></span>
<span data-ttu-id="846ec-104">[Azure biztosít névfeloldás](virtual-networks-name-resolution-for-vms-and-role-instances.md) a virtuális gépek (VM) és a szerepkörpéldányok.</span><span class="sxs-lookup"><span data-stu-id="846ec-104">[Azure provides name resolution](virtual-networks-name-resolution-for-vms-and-role-instances.md) for virtual machines (VMs) and role instances.</span></span> <span data-ttu-id="846ec-105">Amikor a névfeloldás kell túlmutató Azure által biztosított naplókon, megadhatja a saját DNS-kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="846ec-105">However, when your name resolution needs go beyond those provided by Azure, you can provide your own DNS servers.</span></span> <span data-ttu-id="846ec-106">Ez biztosítja az energiagazdálkodási tootailor a DNS-megoldás toosuit hello saját igényeinek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="846ec-106">This gives you hello power tootailor your DNS solution toosuit your own specific needs.</span></span> <span data-ttu-id="846ec-107">Például szükség lehet tooaccess a helyszíni erőforrások az Active Directory tartományvezérlőjéhez keresztül.</span><span class="sxs-lookup"><span data-stu-id="846ec-107">For example, you may need tooaccess on-premises resources via your Active Directory domain controller.</span></span>

<span data-ttu-id="846ec-108">Az egyéni DNS-kiszolgálók működtetnek továbbíthatja Azure virtuális gépeken, ha az állomásnév lekérdezi hello azonos virtuális hálózaton tooAzure tooresolve hostnames.</span><span class="sxs-lookup"><span data-stu-id="846ec-108">When your custom DNS servers are hosted as Azure VMs you can forward hostname queries for hello same vnet tooAzure tooresolve hostnames.</span></span> <span data-ttu-id="846ec-109">Ha nem szeretné toouse Ez az útvonal, a virtuális gép állomásnevek regisztrálhatja a dinamikus DNS használatával DNS-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="846ec-109">If you do not wish toouse this route, you can register your VM hostnames in your DNS server using Dynamic DNS.</span></span>  <span data-ttu-id="846ec-110">Azure nincs hello lehetőséget (pl. a hitelesítő adatok) toodirectly rekordokat létrehozni a DNS-kiszolgálók, így alternatív megoldások gyakran van szükség.</span><span class="sxs-lookup"><span data-stu-id="846ec-110">Azure doesn't have hello ability (e.g. credentials) toodirectly create records in your DNS servers, so alternative arrangements are often needed.</span></span> <span data-ttu-id="846ec-111">Az alábbiakban néhány gyakori helyzetek lehetőségeket.</span><span class="sxs-lookup"><span data-stu-id="846ec-111">Here are some common scenarios with alternatives.</span></span>

## <a name="windows-clients"></a><span data-ttu-id="846ec-112">Windows-ügyfelek</span><span class="sxs-lookup"><span data-stu-id="846ec-112">Windows clients</span></span>
<span data-ttu-id="846ec-113">A nem tartományhoz csatlakoztatott Windows-ügyfelek nem biztonságos dinamikus DNS (DDNS) frissítések történt kísérlet, ha indulnak, vagy ha megváltoztatja az IP-címet.</span><span class="sxs-lookup"><span data-stu-id="846ec-113">Non-domain-joined Windows clients attempt unsecured Dynamic DNS (DDNS) updates when they boot or when their IP address changes.</span></span> <span data-ttu-id="846ec-114">hello DNS-neve: a hello állomásnév plus hello elsődleges DNS-utótagot.</span><span class="sxs-lookup"><span data-stu-id="846ec-114">hello DNS name is hello hostname plus hello primary DNS suffix.</span></span> <span data-ttu-id="846ec-115">Azure hello elsődleges DNS-utótag üresen hagyja, de ez a virtuális gép, hello hello segítségével beállíthatja [felhasználói felület](https://technet.microsoft.com/library/cc794784.aspx) vagy [automatizálás segítségével](https://social.technet.microsoft.com/forums/windowsserver/3720415a-6a9a-4bca-aa2a-6df58a1a47d7/change-primary-dns-suffix).</span><span class="sxs-lookup"><span data-stu-id="846ec-115">Azure leaves hello primary DNS suffix blank, but you can set this in hello VM, via hello [UI](https://technet.microsoft.com/library/cc794784.aspx) or [by using automation](https://social.technet.microsoft.com/forums/windowsserver/3720415a-6a9a-4bca-aa2a-6df58a1a47d7/change-primary-dns-suffix).</span></span>

<span data-ttu-id="846ec-116">A tartományhoz csatlakoztatott Windows-ügyfelek regisztrálja az IP-címek hello tartományvezérlő biztonságos dinamikus DNS használatával.</span><span class="sxs-lookup"><span data-stu-id="846ec-116">Domain-joined Windows clients register their IP addresses with hello domain controller by using secure Dynamic DNS.</span></span> <span data-ttu-id="846ec-117">hello tartományhoz való csatlakozást folyamat hello ügyfél hello elsődleges DNS-utótag be és hoz létre, és hello megbízhatósági kapcsolatot tart fenn.</span><span class="sxs-lookup"><span data-stu-id="846ec-117">hello domain-join process sets hello primary DNS suffix on hello client and creates and maintains hello trust relationship.</span></span>

## <a name="linux-clients"></a><span data-ttu-id="846ec-118">Linux-ügyfelek</span><span class="sxs-lookup"><span data-stu-id="846ec-118">Linux clients</span></span>
<span data-ttu-id="846ec-119">Linux-ügyfelek általában nincs regisztrálása maguk hello DNS-kiszolgáló indításakor, akkor azt feltételezik, hogy hello DHCP-kiszolgáló minderre.</span><span class="sxs-lookup"><span data-stu-id="846ec-119">Linux clients generally don't register themselves with hello DNS server on startup, they assume hello DHCP server does it.</span></span> <span data-ttu-id="846ec-120">Azure DHCP-kiszolgálók nem rendelkeznek hello lehetősége vagy a hitelesítő adatok tooregister rekordokat a DNS-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="846ec-120">Azure's DHCP servers do not have hello ability or credentials tooregister records in your DNS server.</span></span>  <span data-ttu-id="846ec-121">Használhatja a nevű eszköz *nsupdate*, amely hello Bind csomagban található, toosend dinamikus DNS-frissítések.</span><span class="sxs-lookup"><span data-stu-id="846ec-121">You can use a tool called *nsupdate*, which is included in hello Bind package, toosend Dynamic DNS updates.</span></span> <span data-ttu-id="846ec-122">Mivel a dinamikus DNS protokoll hello szabványosított, használhatja *nsupdate* még ha nem a Bind hello DNS-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="846ec-122">Because hello Dynamic DNS protocol is standardized, you can use *nsupdate* even when you're not using Bind on hello DNS server.</span></span>

<span data-ttu-id="846ec-123">DHCP-ügyfél toocreate hello által biztosított hello hurkok használja, és hello gazdagépnév-bejegyzés a hello DNS-kiszolgáló karbantartása.</span><span class="sxs-lookup"><span data-stu-id="846ec-123">You can use hello hooks that are provided by hello DHCP client toocreate and maintain hello hostname entry in hello DNS server.</span></span> <span data-ttu-id="846ec-124">Hello DHCP ciklusban hello ügyfél hello parancsfájlok végrehajtása */etc/dhcp/dhclient-exit-hooks.d/*.</span><span class="sxs-lookup"><span data-stu-id="846ec-124">During hello DHCP cycle, hello client executes hello scripts in */etc/dhcp/dhclient-exit-hooks.d/*.</span></span> <span data-ttu-id="846ec-125">Ez lehet tooregister hello új IP-címet használt *nsupdate*.</span><span class="sxs-lookup"><span data-stu-id="846ec-125">This can be used tooregister hello new IP address by using *nsupdate*.</span></span> <span data-ttu-id="846ec-126">Példa:</span><span class="sxs-lookup"><span data-stu-id="846ec-126">For example:</span></span>

        #!/bin/sh
        requireddomain=mydomain.local

        # only execute on hello primary nic
        if [ "$interface" != "eth0" ]
        then
            return
        fi

        # when we have a new IP, perform nsupdate
        if [ "$reason" = BOUND ] || [ "$reason" = RENEW ] ||
           [ "$reason" = REBIND ] || [ "$reason" = REBOOT ]
        then
            host=`hostname`
            nsupdatecmds=/var/tmp/nsupdatecmds
              echo "update delete $host.$requireddomain a" > $nsupdatecmds
              echo "update add $host.$requireddomain 3600 a $new_ip_address" >> $nsupdatecmds
              echo "send" >> $nsupdatecmds

              nsupdate $nsupdatecmds
        fi

        
        

<span data-ttu-id="846ec-127">Is használhatja a hello *nsupdate* parancs tooperform biztonságos dinamikus DNS-frissítések.</span><span class="sxs-lookup"><span data-stu-id="846ec-127">You can also use hello *nsupdate* command tooperform secure Dynamic DNS updates.</span></span> <span data-ttu-id="846ec-128">Például egy kötés DNS-kiszolgálót használ, egy nyilvános-titkos kulcsból álló kulcspárt esetén [generált](http://linux.yyz.us/nsupdate/).</span><span class="sxs-lookup"><span data-stu-id="846ec-128">For example, when you're using a Bind DNS server, a public-private key pair is [generated](http://linux.yyz.us/nsupdate/).</span></span>  <span data-ttu-id="846ec-129">hello a DNS-kiszolgáló [konfigurált](http://linux.yyz.us/dns/ddns-server.html) hello úgy, hogy ellenőrizni tudja a hello aláírás hello kérésre hello kulcs nyilvános része.</span><span class="sxs-lookup"><span data-stu-id="846ec-129">hello DNS server is [configured](http://linux.yyz.us/dns/ddns-server.html) with hello public part of hello key so that it can verify hello signature on hello request.</span></span> <span data-ttu-id="846ec-130">Hello kell használnia *-k* beállítás tooprovide hello-kulcspár túl*nsupdate* ahhoz, hogy hello dinamikus DNS-frissítési kérelem toobe aláírva.</span><span class="sxs-lookup"><span data-stu-id="846ec-130">You must use hello *-k* option tooprovide hello key-pair too*nsupdate* in order for hello Dynamic DNS update request toobe signed.</span></span>

<span data-ttu-id="846ec-131">A Windows DNS-kiszolgáló használata, ha használható a Kerberos-hitelesítés hello *-g* paraméterének *nsupdate* (nem érhető el a hello Windows-verzión *nsupdate* ).</span><span class="sxs-lookup"><span data-stu-id="846ec-131">When you're using a Windows DNS server, you can use Kerberos authentication with hello *-g* parameter in *nsupdate* (not available in hello Windows version of *nsupdate*).</span></span> <span data-ttu-id="846ec-132">toodo ehhez, *kinit* tooload hello hitelesítő adatait (pl. egy [keytab fájl](http://www.itadmintools.com/2011/07/creating-kerberos-keytab-files.html)).</span><span class="sxs-lookup"><span data-stu-id="846ec-132">toodo this, use *kinit* tooload hello credentials (e.g. from a [keytab file](http://www.itadmintools.com/2011/07/creating-kerberos-keytab-files.html)).</span></span> <span data-ttu-id="846ec-133">Majd *nsupdate -g* felveszi hello gyorsítótár hello hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="846ec-133">Then *nsupdate -g* will pick up hello credentials from hello cache.</span></span>

<span data-ttu-id="846ec-134">Ha szükséges, a DNS keresési utótag tooyour virtuális gépek is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="846ec-134">If needed, you can add a DNS search suffix tooyour VMs.</span></span> <span data-ttu-id="846ec-135">hello DNS-utótag megadott hello */etc/resolv.conf* fájlt.</span><span class="sxs-lookup"><span data-stu-id="846ec-135">hello DNS suffix is specified in hello */etc/resolv.conf* file.</span></span> <span data-ttu-id="846ec-136">A legtöbb Linux disztribúciókkal automatikusan kezeli a hello tartalmát a fájl, ezért általában nem szerkeszthető.</span><span class="sxs-lookup"><span data-stu-id="846ec-136">Most Linux distros automatically manage hello content of this file, so usually you can't edit it.</span></span> <span data-ttu-id="846ec-137">Azonban a beállítás felülbírálható hello utótag hello DHCP-ügyfél használatával *felülír* parancsot.</span><span class="sxs-lookup"><span data-stu-id="846ec-137">However, you can override hello suffix by using hello DHCP client's *supersede* command.</span></span> <span data-ttu-id="846ec-138">toodo Ez a */etc/dhcp/dhclient.conf*, adja hozzá:</span><span class="sxs-lookup"><span data-stu-id="846ec-138">toodo this, in */etc/dhcp/dhclient.conf*, add:</span></span>

        supersede domain-name <required-dns-suffix>;

