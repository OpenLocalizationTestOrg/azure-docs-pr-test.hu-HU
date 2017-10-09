## <a name="os-config"></a>Adja hozzá az IP-címek tooa virtuális gép operációs rendszere

Csatlakoztassa és bejelentkezési tooa VM létrehozott több privát IP-címmel. Minden hello magánhálózati IP-címek (többek között a következőket hello elsődleges), hogy hozzáadta a virtuális gép toohello manuálisan kell hozzáadni. Hajtsa végre a következő lépéseket a virtuális gép operációs rendszer hello:

### <a name="windows"></a>Windows

1. A parancssorba írja be az *ipconfig /all* parancsot.  Csak akkor jelenik meg hello *elsődleges* privát IP-cím (DHCP).
2. Típus *ncpa.cpl* a hello parancssor tooopen hello **hálózati kapcsolat** ablak.
3. Nyissa meg a megfelelő adapter hello hello tulajdonságainak: **helyi kapcsolat**.
4. Kattintson duplán A TCP/IP protokoll 4-es verziója (IPv4) elemre.
5. Válassza ki **következő IP-cím használata hello** , és írja be a következő értékek hello:

    * **IP-cím**: Adja meg a hello *elsődleges* magánhálózati IP-cím
    * **Alhálózati maszk**: Állítsa be az alhálózatának megfelelően. Például akkor, ha hello alhálózat egy/24 alhálózati majd hello alhálózati maszk pedig a 255.255.255.0.
    * **Alapértelmezett átjáró**: hello hello alhálózat első IP-címet. Az alhálózat 10.0.0.0/24, majd hello átjáró IP-cím akkor 10.0.0.1.
    * Kattintson a **a következő DNS-kiszolgálócímek használata hello** , és írja be a következő értékek hello:
        * **Elsődleges DNS-kiszolgáló**: Ha nem a saját DNS-kiszolgálóját használja, adja meg a következőt: 168.63.129.16.  Ha a saját DNS-kiszolgálót használ, adja meg hello IP-címet a kiszolgáló.
    * Kattintson a hello **speciális** gombra, majd adja hozzá a további IP-címeket. Adja hozzá az egyes hello másodlagos magánhálózati IP-címek 8. lépés toohello NIC az ugyanazon az alhálózaton hello elsődleges IP-címhez megadott hello szerepel.
        >[!WARNING] 
        >Nem lépések hello fenti megfelelően, ha megszakadhat a kapcsolat tooyour virtuális gép. Ellenőrizze, hogy 5. lépés megadott hello információt pontos a folytatás előtt.

    * Kattintson a **OK** tooclose kimenő hello TCP/IP-beállításokat, majd **OK** újra tooclose hello adapterre vonatkozó beállításai. A rendszer újból létesíti az RDP-kapcsolatot.

6. A parancssorba írja be az *ipconfig /all* parancsot. Megjelenik az összes hozzáadott IP-cím, és a DHCP ki van kapcsolva.


### <a name="validation-windows"></a>Ellenőrzés (Windows)

tooensure képes tooconnect toohello interneten keresztül hello nyilvános IP-cím tartozó, megfelelő használatával hozzáadását követően a másodlagos IP-konfigurációja az lépések felett, a következő parancs hello használata:

```bash
ping -S 10.0.0.5 hotmail.com
```
>[!NOTE]
>Másodlagos IP-konfigurációk esetén csak a ping paranccsal toohello Internet egy nyilvános IP-cím hozzárendelve hello konfiguráció-e. Az elsődleges IP-konfiguráció egy nyilvános IP-cím nincs szükség tooping toohello Internet.

### <a name="linux-ubuntu"></a>Linux (Ubuntu)

1. Nyisson meg egy terminálablakot.
2. Ellenőrizze, hogy hello gyökér szintű felhasználó. Ha nem, adja meg a következő parancs hello:

    ```bash
    sudo -i
    ```

3. Frissítés hello konfigurációs fájl hello hálózati adapter (feltéve, hogy a "eth0").

    * Hello DHCP meglévő sorelemet megtartása. hello elsődleges IP-cím marad konfigurált, mint korábban.
    * Vegyen fel egy további statikus IP-címének konfigurációja tartalmazó hello a következő parancsokat:

        ```bash
        cd /etc/network/interfaces.d/
        ls
        ```

    Egy .cfg fájlnak kell megjelennie.
4. Nyissa meg hello fájlt. A következő sort: hello hello fájl végét hello kell megjelennie:

    ```bash
    auto eth0
    iface eth0 inet dhcp
    ```

5. Adja hozzá az alábbi után, amely a fájl létezik hello sorok hello:

    ```bash
    iface eth0 inet static
    address <your private IP address here>
    netmask <your subnet mask>
    ```

6. Hello fájl mentése hello a következő parancs segítségével:

    ```bash
    :wq
    ```

7. Alaphelyzetbe állítása hello hálózati illesztő – hello a következő parancsot:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

    > [!IMPORTANT]
    > Azonos sor, ha távoli kapcsolatot hello ifdown és ifup is futott.
    >

8. Ellenőrizze, hogy hello IP-cím hozzá toohello hálózati illesztő rendelkező hello a következő parancsot:

    ```bash
    ip addr list eth0
    ```

    Hello IP-cím hozzáadni a hello lista kell megjelennie.

### <a name="linux-redhat-centos-and-others"></a>Linux (Redhat, CentOS és egyebek)

1. Nyisson meg egy terminálablakot.
2. Ellenőrizze, hogy hello gyökér szintű felhasználó. Ha nem, adja meg a következő parancs hello:

    ```bash
    sudo -i
    ```

3. Adja meg a jelszavát, és kövesse a megjelenő utasításokat. Miután hello gyökér szintű felhasználó, keresse meg a toohello hálózati parancsfájlmappa a hello a következő parancsot:

    ```bash
    cd /etc/sysconfig/network-scripts
    ```

4. Lista hello kapcsolódó ifcfg fájlokat a hello a következő parancsot:

    ```bash
    ls ifcfg-*
    ```

    Megtekintheti az *ifcfg-eth0* hello fájlok egyikét.

5. tooadd IP-cím, egy konfigurációs fájl létrehozása az alább látható módon. Vegye figyelembe, hogy minden IP-konfigurációhoz létre kell hozni egy fájlt.

    ```bash
    touch ifcfg-eth0:0
    ```

6. Nyissa meg hello *ifcfg-eth0:0* hello parancs a következő fájlt:

    ```bash
    vi ifcfg-eth0:0
    ```

7. Adja hozzá a tartalom toohello fájl *eth0:0* ebben az esetben a parancs a következő hello. Lehet, hogy tooupdate információkat a IP-címe alapján.

    ```bash
    DEVICE=eth0:0
    BOOTPROTO=static
    ONBOOT=yes
    IPADDR=192.168.101.101
    NETMASK=255.255.255.0
    ```

8. A következő parancs hello hello fájl mentése:

    ```bash
    :wq
    ```

9. Indítsa újra a hello hálózati szolgáltatások, és gondoskodjon arról, hogy hello változtatások sikeres hello a következő parancsok futtatásával:

    ```bash
    /etc/init.d/network restart
    ifconfig
    ```

    Megtekintheti az hello IP-cím hozzá, *eth0:0*, a visszaadott hello listában.

### <a name="validation-linux"></a>Ellenőrzés (Linux)

Biztosan tudni tooconnect toohello tooensure interneten keresztül hello nyilvános IP-cím a másodlagos IP-konfigurációja az tartozó azt, a következő parancs hello használata:

```bash
ping -I 10.0.0.5 hotmail.com
```
>[!NOTE]
>Másodlagos IP-konfigurációk esetén csak a ping paranccsal toohello Internet egy nyilvános IP-cím hozzárendelve hello konfiguráció-e. Az elsődleges IP-konfiguráció egy nyilvános IP-cím nincs szükség tooping toohello Internet.

Linux virtuális gépek, a másodlagos hálózati adapter kimenő kapcsolat toovalidate közben esetleg tooadd megfelelő útvonalak. Nincsenek számos módon toodo ez. Tekintse át a Linux-disztribúciójára vonatkozó megfelelő dokumentációt. hello következő Ez egy metódus tooaccomplish van:

```bash
echo 150 custom >> /etc/iproute2/rt_tables 

ip rule add from 10.0.0.5 lookup custom
ip route add default via 10.0.0.1 dev eth2 table custom

```
- Lehet, hogy tooreplace:
    - **10.0.0.5** hello privát IP-címet, amely rendelkezik egy nyilvános IP-cím társított tooit cím
    - **10.0.0.1** tooyour alapértelmezett átjáró
    - **eth2** toohello nevét a másodlagos hálózati adapter
