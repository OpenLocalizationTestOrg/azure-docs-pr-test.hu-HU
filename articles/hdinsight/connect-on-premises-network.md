---
title: "aaaConnect HDInsight tooyour a helyszíni hálózat - Azure HDInsight |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy HDInsight fürt egy Azure virtuális hálózatban, és csatlakoztassa tooyour a helyi hálózaton. Ismerje meg, hogyan tooconfigure névfeloldás HDInsight és a helyszíni hálózat között egyéni DNS-kiszolgáló használatával."
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/21/2017
ms.author: larryfr
ms.openlocfilehash: 8a3adf0e3df7726d8e6566d723700506baaf89a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-hdinsight-tooyour-on-premise-network"></a>HDInsight tooyour helyi hálózat

Ismerje meg, hogyan tooconnect HDInsight tooyour a helyszíni hálózati Azure virtuális hálózatok és a VPN-átjáró használatával. Ez a dokumentum a tervezési tudnivalókat tartalmaz:

* Egy Azure virtuális hálózatot, amely a tooyour a HDInsight eszközzel a helyszíni hálózat.

* Konfigurálja a DNS-név feloldása hello virtuális hálózat és a helyszíni hálózat között.

* Hálózati biztonsági csoportok toorestrict internet access tooHDInsight konfigurálása.

* A portok hello virtuális hálózaton a HDInsight által biztosított.

## <a name="create-hello-virtual-network-configuration"></a>Hello virtuális hálózati konfiguráció létrehozása

> [!IMPORTANT]
> Ha a részletes útmutatót a csatlakozó HDInsight tooyour a helyszíni hálózat az Azure virtuális hálózat használatával című hello [csatlakozás HDInsight tooyour helyi hálózati](connect-on-premises-network.md) dokumentum.

Használjon hello következő dokumentumok toolearn hogyan toocreate egy Azure virtuális hálózathoz csatlakoztatott tooyour a helyszíni hálózat:
    
* [Hello Azure-portál használatával](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)

* [Az Azure PowerShell használata](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

* [Az Azure parancssori felület használata](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)

## <a name="configure-name-resolution"></a>Névfeloldás konfigurálása

tooallow HDInsight és hello csatlakoztatott hálózati toocommunicate nevű erőforrások, végezze el a következő műveletek hello:

* Hozzon létre egy egyéni DNS-kiszolgáló hello Azure-beli virtuális hálózathoz.

* Hello virtuális hálózati toouse hello egyéni DNS-kiszolgáló konfigurálása hello alapértelmezett Azure rekurzív feloldó helyett.

* Konfigurálja a továbbítási hello egyéni DNS-kiszolgáló és a helyi DNS-kiszolgáló között.

Ez a konfiguráció lehetővé teszi, hogy a következő viselkedés hello:

* A teljes tartománynevek DNS-utótag hello rendelkező kérelmek __hello virtuális hálózat__ toohello egyéni DNS-kiszolgáló a rendszer továbbítja. hello egyéni DNS-kiszolgáló ezután továbbítja a kérelmek toohello Azure rekurzív feloldó, amely hello IP-címet adja vissza.

* Minden más kérelemhez toohello a helyi DNS-kiszolgáló továbbítja. A nyilvános internet erőforrások, például a microsoft.com még akkor is, kérelmeket a rendszer továbbítja toohello a helyi DNS-kiszolgáló a névfeloldáshoz.

A következő diagram hello a zöld sorai hello virtuális hálózat DNS-utótagja hello végződő erőforrásokra vonatkozó kéréseket. Kék sorai hello a helyi hálózaton vagy az erőforrásokra vonatkozó kéréseket hello nyilvános internethez.

![A dokumentumban használt hello konfigurációs feloldása DNS-kérelmek ábrája](./media/connect-on-premises-network/on-premises-to-cloud-dns.png)

### <a name="create-a-custom-dns-server"></a>Hozzon létre egy egyéni DNS-kiszolgáló

> [!IMPORTANT]
> Hozzon létre és hello DNS-kiszolgáló HDInsight hello virtuális hálózaton történő telepítése előtt konfigurálnia kell.

Linux virtuális gép által használt hello toocreate [kötési](https://www.isc.org/downloads/bind/) DNS szoftverek használata hello a következő lépéseket:

> [!NOTE]
> hello következő lépések használják hello [Azure-portálon](https://portal.azure.com) toocreate egy Azure virtuális gépen. Más módokon toocreate egy virtuális gépet, lásd: hello [hozzon létre Virtuálisgép - Azure CLI](../virtual-machines/linux/quick-create-cli.md) és [hozzon létre Virtuálisgép - Azure PowerShell](../virtual-machines/linux/quick-create-portal.md) dokumentumok.

1. A hello [Azure-portálon](https://portal.azure.com), jelölje be  __+__ , __számítási__, és __Ubuntu Server 16.04 LTS__.

    ![Egy Ubuntu virtuális gép létrehozása](./media/connect-on-premises-network/create-ubuntu-vm.png)

2. A hello __alapjai__ területen adja meg a következő információ hello:

    * __Név__: egy rövid nevet, amely azonosítja a virtuális gépet. Például __DNSProxy__.
    * __Felhasználónév__: hello hello SSH-fiókjának nevét.
    * __Nyilvános SSH-kulcs__ vagy __jelszó__: hello hello SSH-fiókhoz tartozó hitelesítési módszer. Azt javasoljuk, nyilvános kulcsok, mert azok biztonságosabb. További információkért lásd: hello [Linux virtuális gépek létrehozása és használata SSH-kulcsok](../virtual-machines/linux/mac-create-ssh-keys.md) dokumentum.
    * __Erőforráscsoport__: válasszon __meglévő__, majd válassza ki a korábban létrehozott virtuális hálózat hello tartalmazó hello erőforráscsoportot.
    * __Hely__: Válasszon hello hello virtuális hálózat ugyanazon a helyen.

    ![Alapszintű virtuálisgép-konfiguráció](./media/connect-on-premises-network/vm-basics.png)

    Hagyja más bejegyzések: hello alapértelmezett értékeket, és válassza ki __OK__.

3. A hello __méret kiválasztása__ szakaszban, jelölje be hello Virtuálisgép-méretet. Ebben az oktatóanyagban válassza ki a legkisebb hello és legalacsonyabb költséget lehetőséget. toocontinue, használjon hello __válasszon__ gombra.

4. A hello __beállítások__ területen adja meg a következő információ hello:

    * __Virtuális hálózati__: válassza ki a korábban létrehozott virtuális hálózati hello.

    * __Alhálózati__: hello alapértelmezett alhálózati hello virtuális hálózat kiválasztása. Tegye __nem__ válasszon hello hello VPN-átjáró által használt.

    * __Diagnosztikai tárfiók__: Válasszon egy meglévő tárfiókot használ, vagy hozzon létre egy újat.

    ![Virtuális hálózati beállítások](./media/connect-on-premises-network/virtual-network-settings.png)

    Hagyja hello más bejegyzések hello alapértelmezett értéket, majd válasszon __OK__ toocontinue.

5. A hello __beszerzési__ szakaszban, jelölje be hello __beszerzési__ gomb toocreate hello virtuális gépet.

6. Hello virtuális gép létrehozása után annak __áttekintése__ szakasz jelenik meg. Hello hello bal oldali listában jelölje ki __tulajdonságok__. Mentse a hello __nyilvános IP-cím__ és __magánhálózati IP-cím__ értékeket. A következő szakaszban hello lesz.

    ![Nyilvános és magánhálózati IP-címek](./media/connect-on-premises-network/vm-ip-addresses.png)

### <a name="install-and-configure-bind-dns-software"></a>Telepítse és konfigurálja a Bind (DNS szoftver)

1. Használja az SSH tooconnect toohello __nyilvános IP-cím__ hello virtuális gép. a következő példa hello tooa virtuális gépen: 40.68.254.142 csatlakozik:

    ```bash
    ssh sshuser@40.68.254.142
    ```

    Cserélje le `sshuser` hello SSH-felhasználói fiókot a megadott hello fürt létrehozásakor.

    > [!NOTE]
    > Számos módon tooobtain hello `ssh` segédprogram. A Linux, Unix és macOS akkor biztosított hello operációs rendszer részeként. Ha Windows használja, fontolja meg hello alábbi beállítások egyikét:
    >
    > * [Azure-felhőbe rendszerhéj](../cloud-shell/quickstart.md)
    > * [A Windows 10 Ubuntu bash](https://msdn.microsoft.com/commandline/wsl/about)
    > * [Git (https://git-scm.com/)](https://git-scm.com/)
    > * [OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)

2. tooinstall Bind, a következő parancsok hello SSH-munkamenetből hello használata:

    ```bash
    sudo apt-get update -y
    sudo apt-get install bind9 -y
    ```

3. tooconfigure Bind tooforward neve feloldási kérések tooyour helyszíni DNS-kiszolgáló, használja a szöveg hello hello tartalmát, a következő hello `/etc/bind/named.conf.options` fájlt:

        acl goodclients {
            10.0.0.0/16; # Replace with hello IP address range of hello virtual network
            10.1.0.0/16; # Replace with hello IP address range of hello on-premises network
            localhost;
            localnets;
        };

        options {
                directory "/var/cache/bind";

                recursion yes;

                allow-query { goodclients; };

                forwarders {
                192.168.0.1; # Replace with hello IP address of hello on-premises DNS server
                };

                dnssec-validation auto;

                auth-nxdomain no;    # conform tooRFC1035
                listen-on { any; };
        };

    > [!IMPORTANT]
    > Cserélje le a hello hello értékek `goodclients` hello IP-címtartománnyal rendelkező szakasza hello virtuális hálózat és a helyszíni hálózat. Ez a rész hello címek, amelyek a DNS-kiszolgáló elfogadja az érkező kérelmeket.
    >
    > Cserélje le a hello `192.168.0.1` hello bejegyzést `forwarders` hello IP-cím szakasz a helyi DNS-kiszolgáló. A bejegyzés útvonalak DNS kérelmek tooyour a helyi DNS-kiszolgálót.

    tooedit ezt a fájlt, a következő parancs használata hello:

    ```bash
    sudo nano /etc/bind/named.conf.options
    ```

    toosave hello fájl használata __Ctrl + X__, __Y__, majd __Enter__.

4. Hello SSH-munkamenetet használja a következő parancs hello:

    ```bash
    hostname -f
    ```

    Ez a parancs visszaadja a szöveg a következő érték hasonló toohello:

        dnsproxy.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net

    Hello `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` szövege hello __DNS-utótag__ ehhez a virtuális hálózathoz. Mentse ezt az értéket, a rendszer később.

5. tooconfigure Bind tooresolve DNS-nevek erőforrások hello virtuális hálózatról, használja a szöveg hello hello tartalmát, a következő hello `/etc/bind/named.conf.local` fájlt:

        // Replace hello following with hello DNS suffix for your virtual network
        zone "icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net" {
            type forward;
            forwarders {168.63.129.16;}; # hello Azure recursive resolver
        };

    > [!IMPORTANT]
    > Le kell cserélnie hello `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` a korábban kapott hello DNS-utótagot.

    tooedit ezt a fájlt, a következő parancs használata hello:

    ```bash
    sudo nano /etc/bind/named.conf.local
    ```

    toosave hello fájl használata __Ctrl + X__, __Y__, majd __Enter__.

6. toostart Bind, a következő parancs hello használata:

    ```bash
    sudo service bind9 restart
    ```

7. tooverify, amely a kötés képes névfeloldásra hello a helyszíni hálózat, a következő parancsok használata hello erőforrások:

    ```bash
    sudo apt install dnsutils
    nslookup dns.mynetwork.net 10.0.0.4
    ```

    > [!IMPORTANT]
    > Cserélje le `dns.mynetwork.net` a hello teljesen minősített tartománynevét (FQDN) a helyszíni hálózati erőforrások.
    >
    > Cserélje le `10.0.0.4` a hello __belső IP-cím__ a egyéni DNS-kiszolgáló hello virtuális hálózatban.

    hello válasz jelenik meg a következő szöveg hasonló toohello:

        Server:         10.0.0.4
        Address:        10.0.0.4#53

        Non-authoritative answer:
        Name:   dns.mynetwork.net
        Address: 192.168.0.4

### <a name="configure-hello-virtual-network-toouse-hello-custom-dns-server"></a>Hello virtuális hálózati toouse hello egyéni DNS-kiszolgáló konfigurálása

tooconfigure hello virtuális hálózati toouse hello egyéni DNS-kiszolgáló helyett hello Azure rekurzív feloldó lépések hello használata:

1. A hello [Azure-portálon](https://portal.azure.com), hello virtuális hálózatot, majd válassza ki és __DNS-kiszolgálók__.

2. Válassza ki __egyéni__, és írja be a hello __belső IP-cím__ hello egyéni DNS-kiszolgáló. Végül válassza ki __mentése__.

    ![Hello hálózati hello egyéni DNS-kiszolgáló beállítása](./media/connect-on-premises-network/configure-custom-dns.png)

### <a name="configure-hello-on-premises-dns-server"></a>Hello a helyi DNS-kiszolgáló konfigurálása

Hello előző szakaszban hello egyéni DNS server tooforward kérelmek toohello a helyi DNS-kiszolgáló konfigurálva. Ezután konfigurálnia kell hello helyszíni DNS server tooforward kérelmek toohello egyéni DNS-kiszolgáló.

Adott útmutatást a tooconfigure a DNS-kiszolgáló, a DNS-kiszolgáló szoftver hello dokumentációban. Hello bemutatjuk, hogyan keresse meg tooconfigure egy __feltételes továbbítók__.

Feltételes továbbítás csak egy kapcsolatspecifikus DNS-utótag kérelmek továbbítja. Ebben az esetben konfigurálnia kell egy továbbítót hello DNS-utótag hello virtuális hálózat. Ez utótag kérelmek továbbítani toohello hello egyéni DNS-kiszolgáló IP-címét. 

hello következő szöveg látható egy példa egy feltételes továbbítók konfigurációja hello **kötési** DNS szoftver:

    zone "icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net" {
        type forward;
        forwarders {10.0.0.4;}; # hello custom DNS server's internal IP address
    };

A DNS-sel kapcsolatos **Windows Server 2016**, lásd: hello [Add-DnsServerConditionalForwarderZone](https://technet.microsoft.com/itpro/powershell/windows/dnsserver/add-dnsserverconditionalforwarderzone) dokumentáció...

Miután konfigurálta a hello a helyi DNS-kiszolgáló, használhatja `nslookup` hello virtuális hálózat nevét, hogy hogyan oldható hello a helyszíni hálózati tooverify a. a következő példa hello 

```bash
nslookup dnsproxy.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net 196.168.0.4
```

Ebben a példában, használja a helyi DNS-kiszolgálójának 196.168.0.4 hello tooresolve hello hello egyéni DNS-kiszolgáló nevét. Cserélje le hello IP-cím hello hello a helyi DNS-kiszolgáló. Cserélje le a hello `dnsproxy` hello teljesen minősített tartománynevét hello egyéni DNS-kiszolgáló címet.

## <a name="optional-control-network-traffic"></a>Választható lehetőség: Vezérlő hálózati forgalom

Hálózati biztonsági csoportokkal (NSG) vagy a felhasználó által definiált útvonalak (UDR) toocontrol hálózati forgalmat is használhatja. Az NSG-k lehetővé teszik toofilter bejövő és kimenő forgalmat, és adatforgalom engedélyezéséhez vagy letiltásához hello. Udr-EK hogyan a forgalom között hello virtuális hálózatán lévő erőforrásokat toocontrol lehetővé teszik, hello internet és a helyszíni hálózat hello.

> [!WARNING]
> HDInsight hello Azure felhőben az adott IP-címekről érkező bejövő hozzáférést, és korlátlan kimenő hozzáférést igényel. Az NSG-k vagy udr-EK toocontrol forgalom használatakor hello a következő lépéseket kell végrehajtania:
>
> 1. Hello IP-címek található hello helyre, amely tartalmazza a virtuális hálózat. Hely szerint szükséges IP-címek listáját lásd: [szükséges IP-címek](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip).
>
> 2. Hello IP-címekről érkező bejövő forgalom engedélyezése.
>
>    * __NSG__: engedélyezése __bejövő__ porton forgalom __443-as__ a hello __Internet__.
>    * __UDR__: Set hello __következő ugrásaként__ hello útvonal too__Internet__ típusú.

Például egy Azure PowerShell vagy a hello Azure CLI toocreate NSG-k segítségével, tekintse meg a hello [kiterjesztése HDInsight az Azure virtuális hálózatokkal](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-nsg) dokumentum.

## <a name="create-hello-hdinsight-cluster"></a>Hello HDInsight-fürt létrehozása

> [!WARNING]
> A virtuális hálózati hello HDInsight telepítése előtt konfigurálnia kell hello egyéni DNS-kiszolgáló.

Hello használata hello szükséges lépések [hello Azure-portál használatával HDInsight-fürtök létrehozása](./hdinsight-hadoop-create-linux-clusters-portal.md) dokumentum toocreate HDInsight-fürtöt.

> [!WARNING]
> * Fürt létrehozása során ki kell választania, amely tartalmazza a virtuális hálózat hello helyét.
>
> * A hello __speciális beállítások__ rész-konfiguráció, ki kell választania hello virtuális hálózat és a korábban létrehozott alhálózati.

## <a name="connecting-toohdinsight"></a>Csatlakozás tooHDInsight

A legtöbb dokumentáció a HDInsight feltételezi, hogy rendelkezik-e hozzáférési toohello fürt keresztül hello internet. Például, hogy csatlakozhasson https://CLUSTERNAME.azurehdinsight.net toohello fürthöz. A cím hello nyilvános átjárón, amely esetén nem érhető el, használja az NSG-k vagy udr-EK toorestrict a hozzáférést a hello internet használja.

toodirectly tooHDInsight hello virtuális hálózaton keresztül csatlakozni, használja a következő lépéseket hello:

1. toodiscover hello belső teljes tartománynevek hello HDInsight-csomópont, a hello a következő módszerek valamelyikével:

    ```powershell
    $resourceGroupName = "hello resource group that contains hello virtual network used with HDInsight"

    $clusterNICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName | where-object {$_.Name -like "*node*"}

    $nodes = @()
    foreach($nic in $clusterNICs) {
        $node = new-object System.Object
        $node | add-member -MemberType NoteProperty -name "Type" -value $nic.Name.Split('-')[1]
        $node | add-member -MemberType NoteProperty -name "InternalIP" -value $nic.IpConfigurations.PrivateIpAddress
        $node | add-member -MemberType NoteProperty -name "InternalFQDN" -value $nic.DnsSettings.InternalFqdn
        $nodes += $node
    }
    $nodes | sort-object Type
    ```

    ```azurecli
    az network nic list --resource-group <resourcegroupname> --output table --query "[?contains(name,'node')].{NICname:name,InternalIP:ipConfigurations[0].privateIpAddress,InternalFQDN:dnsSettings.internalFqdn}"
    ```

2. toodetermine hello port, amelyet a szolgáltatás érhető el, lásd: hello [a HDInsight Hadoop-szolgáltatás által használt portok](./hdinsight-hadoop-port-settings-for-services.md) dokumentum.

    > [!IMPORTANT]
    > Egyes hello átjárócsomópontokkal üzemeltetett szolgáltatások aktívak csak az egyik csomópont egyszerre. Ha megpróbál egy központi csomóponton szolgáltatások elérésére, és sikertelen lesz, váltás más átjárócsomópont toohello.
    >
    > Például Ambari csak egy központi csomóponton aktív egyszerre lehet. Ha egy központi csomóponton Ambari eléréséhez adja vissza, 404 hibaüzenetet, majd a futtató hello más átjárócsomópont.

## <a name="next-steps"></a>Következő lépések

* A HDInsight eszközzel virtuális hálózatban további információkért lásd: [kiterjesztése HDInsight Azure virtuális hálózatok használatával](./hdinsight-extend-hadoop-virtual-network.md).

* Egy Azure virtuális hálózatot további információkért lásd: hello [Azure Virtual Network áttekintése](../virtual-network/virtual-networks-overview.md).

* Hálózati biztonsági csoportokkal kapcsolatos további információkért lásd: [hálózati biztonsági csoportok](../virtual-network/virtual-networks-nsg.md).

* A felhasználó által definiált útvonalak további információkért lásd: [felhasználó által definiált útvonalak és IP-továbbítás](../virtual-network/virtual-networks-udr-overview.md).
