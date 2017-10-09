## <span data-ttu-id="abec3-101"><a name="os-config"></a>Adja hozzá az IP-címek tooa virtuális gép operációs rendszere</span><span class="sxs-lookup"><span data-stu-id="abec3-101"><a name="os-config"></a>Add IP addresses tooa VM operating system</span></span>

<span data-ttu-id="abec3-102">Csatlakoztassa és bejelentkezési tooa VM létrehozott több privát IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="abec3-102">Connect and login tooa VM you created with multiple private IP addresses.</span></span> <span data-ttu-id="abec3-103">Minden hello magánhálózati IP-címek (többek között a következőket hello elsődleges), hogy hozzáadta a virtuális gép toohello manuálisan kell hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="abec3-103">You must manually add all hello private IP addresses (including hello primary) that you added toohello VM.</span></span> <span data-ttu-id="abec3-104">Hajtsa végre a következő lépéseket a virtuális gép operációs rendszer hello:</span><span class="sxs-lookup"><span data-stu-id="abec3-104">Complete hello following steps for your VM operating system:</span></span>

### <a name="windows"></a><span data-ttu-id="abec3-105">Windows</span><span class="sxs-lookup"><span data-stu-id="abec3-105">Windows</span></span>

1. <span data-ttu-id="abec3-106">A parancssorba írja be az *ipconfig /all* parancsot.</span><span class="sxs-lookup"><span data-stu-id="abec3-106">From a command prompt, type *ipconfig /all*.</span></span>  <span data-ttu-id="abec3-107">Csak akkor jelenik meg hello *elsődleges* privát IP-cím (DHCP).</span><span class="sxs-lookup"><span data-stu-id="abec3-107">You only see hello *Primary* private IP address (through DHCP).</span></span>
2. <span data-ttu-id="abec3-108">Típus *ncpa.cpl* a hello parancssor tooopen hello **hálózati kapcsolat** ablak.</span><span class="sxs-lookup"><span data-stu-id="abec3-108">Type *ncpa.cpl* in hello command prompt tooopen hello **Network connections** window.</span></span>
3. <span data-ttu-id="abec3-109">Nyissa meg a megfelelő adapter hello hello tulajdonságainak: **helyi kapcsolat**.</span><span class="sxs-lookup"><span data-stu-id="abec3-109">Open hello properties for hello appropriate adapter: **Local Area Connection**.</span></span>
4. <span data-ttu-id="abec3-110">Kattintson duplán A TCP/IP protokoll 4-es verziója (IPv4) elemre.</span><span class="sxs-lookup"><span data-stu-id="abec3-110">Double-click Internet Protocol version 4 (IPv4).</span></span>
5. <span data-ttu-id="abec3-111">Válassza ki **következő IP-cím használata hello** , és írja be a következő értékek hello:</span><span class="sxs-lookup"><span data-stu-id="abec3-111">Select **Use hello following IP address** and enter hello following values:</span></span>

    * <span data-ttu-id="abec3-112">**IP-cím**: Adja meg a hello *elsődleges* magánhálózati IP-cím</span><span class="sxs-lookup"><span data-stu-id="abec3-112">**IP address**: Enter hello *Primary* private IP address</span></span>
    * <span data-ttu-id="abec3-113">**Alhálózati maszk**: Állítsa be az alhálózatának megfelelően.</span><span class="sxs-lookup"><span data-stu-id="abec3-113">**Subnet mask**: Set based on your subnet.</span></span> <span data-ttu-id="abec3-114">Például akkor, ha hello alhálózat egy/24 alhálózati majd hello alhálózati maszk pedig a 255.255.255.0.</span><span class="sxs-lookup"><span data-stu-id="abec3-114">For example, if hello subnet is a /24 subnet then hello subnet mask is 255.255.255.0.</span></span>
    * <span data-ttu-id="abec3-115">**Alapértelmezett átjáró**: hello hello alhálózat első IP-címet.</span><span class="sxs-lookup"><span data-stu-id="abec3-115">**Default gateway**: hello first IP address in hello subnet.</span></span> <span data-ttu-id="abec3-116">Az alhálózat 10.0.0.0/24, majd hello átjáró IP-cím akkor 10.0.0.1.</span><span class="sxs-lookup"><span data-stu-id="abec3-116">If your subnet is 10.0.0.0/24, then hello gateway IP address is 10.0.0.1.</span></span>
    * <span data-ttu-id="abec3-117">Kattintson a **a következő DNS-kiszolgálócímek használata hello** , és írja be a következő értékek hello:</span><span class="sxs-lookup"><span data-stu-id="abec3-117">Click **Use hello following DNS server addresses** and enter hello following values:</span></span>
        * <span data-ttu-id="abec3-118">**Elsődleges DNS-kiszolgáló**: Ha nem a saját DNS-kiszolgálóját használja, adja meg a következőt: 168.63.129.16.</span><span class="sxs-lookup"><span data-stu-id="abec3-118">**Preferred DNS server**: If you are not using your own DNS server, enter 168.63.129.16.</span></span>  <span data-ttu-id="abec3-119">Ha a saját DNS-kiszolgálót használ, adja meg hello IP-címet a kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="abec3-119">If you are using your own DNS server, enter hello IP address for your server.</span></span>
    * <span data-ttu-id="abec3-120">Kattintson a hello **speciális** gombra, majd adja hozzá a további IP-címeket.</span><span class="sxs-lookup"><span data-stu-id="abec3-120">Click hello **Advanced** button and add additional IP addresses.</span></span> <span data-ttu-id="abec3-121">Adja hozzá az egyes hello másodlagos magánhálózati IP-címek 8. lépés toohello NIC az ugyanazon az alhálózaton hello elsődleges IP-címhez megadott hello szerepel.</span><span class="sxs-lookup"><span data-stu-id="abec3-121">Add each of hello secondary private IP addresses listed in step 8 toohello NIC with hello same subnet specified for hello primary IP address.</span></span>
        >[!WARNING] 
        ><span data-ttu-id="abec3-122">Nem lépések hello fenti megfelelően, ha megszakadhat a kapcsolat tooyour virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="abec3-122">If you do not follow hello steps above correctly, you may lose connectivity tooyour VM.</span></span> <span data-ttu-id="abec3-123">Ellenőrizze, hogy 5. lépés megadott hello információt pontos a folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="abec3-123">Ensure hello information entered for step 5 is accurate before proceeding.</span></span>

    * <span data-ttu-id="abec3-124">Kattintson a **OK** tooclose kimenő hello TCP/IP-beállításokat, majd **OK** újra tooclose hello adapterre vonatkozó beállításai.</span><span class="sxs-lookup"><span data-stu-id="abec3-124">Click **OK** tooclose out hello TCP/IP settings and then **OK** again tooclose hello adapter settings.</span></span> <span data-ttu-id="abec3-125">A rendszer újból létesíti az RDP-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="abec3-125">Your RDP connection is re-established.</span></span>

6. <span data-ttu-id="abec3-126">A parancssorba írja be az *ipconfig /all* parancsot.</span><span class="sxs-lookup"><span data-stu-id="abec3-126">From a command prompt, type *ipconfig /all*.</span></span> <span data-ttu-id="abec3-127">Megjelenik az összes hozzáadott IP-cím, és a DHCP ki van kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="abec3-127">All IP addresses you added are shown and DHCP is turned off.</span></span>


### <a name="validation-windows"></a><span data-ttu-id="abec3-128">Ellenőrzés (Windows)</span><span class="sxs-lookup"><span data-stu-id="abec3-128">Validation (Windows)</span></span>

<span data-ttu-id="abec3-129">tooensure képes tooconnect toohello interneten keresztül hello nyilvános IP-cím tartozó, megfelelő használatával hozzáadását követően a másodlagos IP-konfigurációja az lépések felett, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="abec3-129">tooensure you are able tooconnect toohello internet from your secondary IP configuration via hello public IP associated it, once you have added it correctly using steps above, use hello following command:</span></span>

```bash
ping -S 10.0.0.5 hotmail.com
```
>[!NOTE]
><span data-ttu-id="abec3-130">Másodlagos IP-konfigurációk esetén csak a ping paranccsal toohello Internet egy nyilvános IP-cím hozzárendelve hello konfiguráció-e.</span><span class="sxs-lookup"><span data-stu-id="abec3-130">For secondary IP configurations, you can only ping toohello Internet if hello configuration has a public IP address associated with it.</span></span> <span data-ttu-id="abec3-131">Az elsődleges IP-konfiguráció egy nyilvános IP-cím nincs szükség tooping toohello Internet.</span><span class="sxs-lookup"><span data-stu-id="abec3-131">For primary IP configurations, a public IP address is not required tooping toohello Internet.</span></span>

### <a name="linux-ubuntu"></a><span data-ttu-id="abec3-132">Linux (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="abec3-132">Linux (Ubuntu)</span></span>

1. <span data-ttu-id="abec3-133">Nyisson meg egy terminálablakot.</span><span class="sxs-lookup"><span data-stu-id="abec3-133">Open a terminal window.</span></span>
2. <span data-ttu-id="abec3-134">Ellenőrizze, hogy hello gyökér szintű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="abec3-134">Make sure you are hello root user.</span></span> <span data-ttu-id="abec3-135">Ha nem, adja meg a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="abec3-135">If you are not, enter hello following command:</span></span>

    ```bash
    sudo -i
    ```

3. <span data-ttu-id="abec3-136">Frissítés hello konfigurációs fájl hello hálózati adapter (feltéve, hogy a "eth0").</span><span class="sxs-lookup"><span data-stu-id="abec3-136">Update hello configuration file of hello network interface (assuming ‘eth0’).</span></span>

    * <span data-ttu-id="abec3-137">Hello DHCP meglévő sorelemet megtartása.</span><span class="sxs-lookup"><span data-stu-id="abec3-137">Keep hello existing line item for dhcp.</span></span> <span data-ttu-id="abec3-138">hello elsődleges IP-cím marad konfigurált, mint korábban.</span><span class="sxs-lookup"><span data-stu-id="abec3-138">hello primary IP address remains configured as it was previously.</span></span>
    * <span data-ttu-id="abec3-139">Vegyen fel egy további statikus IP-címének konfigurációja tartalmazó hello a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="abec3-139">Add a configuration for an additional static IP address with hello following commands:</span></span>

        ```bash
        cd /etc/network/interfaces.d/
        ls
        ```

    <span data-ttu-id="abec3-140">Egy .cfg fájlnak kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="abec3-140">You should see a .cfg file.</span></span>
4. <span data-ttu-id="abec3-141">Nyissa meg hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="abec3-141">Open hello file.</span></span> <span data-ttu-id="abec3-142">A következő sort: hello hello fájl végét hello kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="abec3-142">You should see hello following lines at hello end of hello file:</span></span>

    ```bash
    auto eth0
    iface eth0 inet dhcp
    ```

5. <span data-ttu-id="abec3-143">Adja hozzá az alábbi után, amely a fájl létezik hello sorok hello:</span><span class="sxs-lookup"><span data-stu-id="abec3-143">Add hello following lines after hello lines that exist in this file:</span></span>

    ```bash
    iface eth0 inet static
    address <your private IP address here>
    netmask <your subnet mask>
    ```

6. <span data-ttu-id="abec3-144">Hello fájl mentése hello a következő parancs segítségével:</span><span class="sxs-lookup"><span data-stu-id="abec3-144">Save hello file by using hello following command:</span></span>

    ```bash
    :wq
    ```

7. <span data-ttu-id="abec3-145">Alaphelyzetbe állítása hello hálózati illesztő – hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="abec3-145">Reset hello network interface with hello following command:</span></span>

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="abec3-146">Azonos sor, ha távoli kapcsolatot hello ifdown és ifup is futott.</span><span class="sxs-lookup"><span data-stu-id="abec3-146">Run both ifdown and ifup in hello same line if using a remote connection.</span></span>
    >

8. <span data-ttu-id="abec3-147">Ellenőrizze, hogy hello IP-cím hozzá toohello hálózati illesztő rendelkező hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="abec3-147">Verify hello IP address is added toohello network interface with hello following command:</span></span>

    ```bash
    ip addr list eth0
    ```

    <span data-ttu-id="abec3-148">Hello IP-cím hozzáadni a hello lista kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="abec3-148">You should see hello IP address you added as part of hello list.</span></span>

### <a name="linux-redhat-centos-and-others"></a><span data-ttu-id="abec3-149">Linux (Redhat, CentOS és egyebek)</span><span class="sxs-lookup"><span data-stu-id="abec3-149">Linux (Redhat, CentOS, and others)</span></span>

1. <span data-ttu-id="abec3-150">Nyisson meg egy terminálablakot.</span><span class="sxs-lookup"><span data-stu-id="abec3-150">Open a terminal window.</span></span>
2. <span data-ttu-id="abec3-151">Ellenőrizze, hogy hello gyökér szintű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="abec3-151">Make sure you are hello root user.</span></span> <span data-ttu-id="abec3-152">Ha nem, adja meg a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="abec3-152">If you are not, enter hello following command:</span></span>

    ```bash
    sudo -i
    ```

3. <span data-ttu-id="abec3-153">Adja meg a jelszavát, és kövesse a megjelenő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="abec3-153">Enter your password and follow instructions as prompted.</span></span> <span data-ttu-id="abec3-154">Miután hello gyökér szintű felhasználó, keresse meg a toohello hálózati parancsfájlmappa a hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="abec3-154">Once you are hello root user, navigate toohello network scripts folder with hello following command:</span></span>

    ```bash
    cd /etc/sysconfig/network-scripts
    ```

4. <span data-ttu-id="abec3-155">Lista hello kapcsolódó ifcfg fájlokat a hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="abec3-155">List hello related ifcfg files using hello following command:</span></span>

    ```bash
    ls ifcfg-*
    ```

    <span data-ttu-id="abec3-156">Megtekintheti az *ifcfg-eth0* hello fájlok egyikét.</span><span class="sxs-lookup"><span data-stu-id="abec3-156">You should see *ifcfg-eth0* as one of hello files.</span></span>

5. <span data-ttu-id="abec3-157">tooadd IP-cím, egy konfigurációs fájl létrehozása az alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="abec3-157">tooadd an IP address, create a configuration file for it as shown below.</span></span> <span data-ttu-id="abec3-158">Vegye figyelembe, hogy minden IP-konfigurációhoz létre kell hozni egy fájlt.</span><span class="sxs-lookup"><span data-stu-id="abec3-158">Note that one file must be created for each IP configuration.</span></span>

    ```bash
    touch ifcfg-eth0:0
    ```

6. <span data-ttu-id="abec3-159">Nyissa meg hello *ifcfg-eth0:0* hello parancs a következő fájlt:</span><span class="sxs-lookup"><span data-stu-id="abec3-159">Open hello *ifcfg-eth0:0* file with hello following command:</span></span>

    ```bash
    vi ifcfg-eth0:0
    ```

7. <span data-ttu-id="abec3-160">Adja hozzá a tartalom toohello fájl *eth0:0* ebben az esetben a parancs a következő hello.</span><span class="sxs-lookup"><span data-stu-id="abec3-160">Add content toohello file, *eth0:0* in this case, with hello following command.</span></span> <span data-ttu-id="abec3-161">Lehet, hogy tooupdate információkat a IP-címe alapján.</span><span class="sxs-lookup"><span data-stu-id="abec3-161">Be sure tooupdate information based on your IP address.</span></span>

    ```bash
    DEVICE=eth0:0
    BOOTPROTO=static
    ONBOOT=yes
    IPADDR=192.168.101.101
    NETMASK=255.255.255.0
    ```

8. <span data-ttu-id="abec3-162">A következő parancs hello hello fájl mentése:</span><span class="sxs-lookup"><span data-stu-id="abec3-162">Save hello file with hello following command:</span></span>

    ```bash
    :wq
    ```

9. <span data-ttu-id="abec3-163">Indítsa újra a hello hálózati szolgáltatások, és gondoskodjon arról, hogy hello változtatások sikeres hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="abec3-163">Restart hello network services and make sure hello changes are successful by running hello following commands:</span></span>

    ```bash
    /etc/init.d/network restart
    ifconfig
    ```

    <span data-ttu-id="abec3-164">Megtekintheti az hello IP-cím hozzá, *eth0:0*, a visszaadott hello listában.</span><span class="sxs-lookup"><span data-stu-id="abec3-164">You should see hello IP address you added, *eth0:0*, in hello list returned.</span></span>

### <a name="validation-linux"></a><span data-ttu-id="abec3-165">Ellenőrzés (Linux)</span><span class="sxs-lookup"><span data-stu-id="abec3-165">Validation (Linux)</span></span>

<span data-ttu-id="abec3-166">Biztosan tudni tooconnect toohello tooensure interneten keresztül hello nyilvános IP-cím a másodlagos IP-konfigurációja az tartozó azt, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="abec3-166">tooensure you are able tooconnect toohello internet from your secondary IP configuration via hello public IP associated it, use hello following command:</span></span>

```bash
ping -I 10.0.0.5 hotmail.com
```
>[!NOTE]
><span data-ttu-id="abec3-167">Másodlagos IP-konfigurációk esetén csak a ping paranccsal toohello Internet egy nyilvános IP-cím hozzárendelve hello konfiguráció-e.</span><span class="sxs-lookup"><span data-stu-id="abec3-167">For secondary IP configurations, you can only ping toohello Internet if hello configuration has a public IP address associated with it.</span></span> <span data-ttu-id="abec3-168">Az elsődleges IP-konfiguráció egy nyilvános IP-cím nincs szükség tooping toohello Internet.</span><span class="sxs-lookup"><span data-stu-id="abec3-168">For primary IP configurations, a public IP address is not required tooping toohello Internet.</span></span>

<span data-ttu-id="abec3-169">Linux virtuális gépek, a másodlagos hálózati adapter kimenő kapcsolat toovalidate közben esetleg tooadd megfelelő útvonalak.</span><span class="sxs-lookup"><span data-stu-id="abec3-169">For Linux VMs, when trying toovalidate outbound connectivity from a secondary NIC, you may need tooadd appropriate routes.</span></span> <span data-ttu-id="abec3-170">Nincsenek számos módon toodo ez.</span><span class="sxs-lookup"><span data-stu-id="abec3-170">There are many ways toodo this.</span></span> <span data-ttu-id="abec3-171">Tekintse át a Linux-disztribúciójára vonatkozó megfelelő dokumentációt.</span><span class="sxs-lookup"><span data-stu-id="abec3-171">Please see appropriate documentation for your Linux distribution.</span></span> <span data-ttu-id="abec3-172">hello következő Ez egy metódus tooaccomplish van:</span><span class="sxs-lookup"><span data-stu-id="abec3-172">hello following is one method tooaccomplish this:</span></span>

```bash
echo 150 custom >> /etc/iproute2/rt_tables 

ip rule add from 10.0.0.5 lookup custom
ip route add default via 10.0.0.1 dev eth2 table custom

```
- <span data-ttu-id="abec3-173">Lehet, hogy tooreplace:</span><span class="sxs-lookup"><span data-stu-id="abec3-173">Be sure tooreplace:</span></span>
    - <span data-ttu-id="abec3-174">**10.0.0.5** hello privát IP-címet, amely rendelkezik egy nyilvános IP-cím társított tooit cím</span><span class="sxs-lookup"><span data-stu-id="abec3-174">**10.0.0.5** with hello private IP address that has a public IP address associated tooit</span></span>
    - <span data-ttu-id="abec3-175">**10.0.0.1** tooyour alapértelmezett átjáró</span><span class="sxs-lookup"><span data-stu-id="abec3-175">**10.0.0.1** tooyour default gateway</span></span>
    - <span data-ttu-id="abec3-176">**eth2** toohello nevét a másodlagos hálózati adapter</span><span class="sxs-lookup"><span data-stu-id="abec3-176">**eth2** toohello name of your secondary NIC</span></span>
