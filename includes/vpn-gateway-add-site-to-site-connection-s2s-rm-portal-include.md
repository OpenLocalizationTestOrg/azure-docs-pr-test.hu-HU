1. Keresse meg a virtuális hálózati átjáró tooand nyitott hello panelen. Nincsenek több módon toonavigate. A jelen példában azt nyílik toohello átjáró "VNet1GW" címen túl**TestVNet1 -> Áttekintés -> csatlakoztatott eszközök -> VNet1GW**.
2. VNet1GW hello paneljén kattintson **kapcsolatok**. Hello hello kapcsolatok panel felső részén kattintson **+ Hozzáadás** tooopen hello **kapcsolat hozzáadása a** panelen.

    ![Helyek közötti kapcsolat létrehozása](./media/vpn-gateway-add-site-to-site-connection-s2s-rm-portal-include/connection1.png)

3. A hello **kapcsolat hozzáadása a** panelen adja meg a hello értékek toocreate a kapcsolatot.

  - **Név:** Nevezze el a kapcsolatot. A példában a **VNet1toSite2** nevet használjuk.
  - **Kapcsolat típusa:** Válassza a **Helyek közötti (IPSec)** típust.
  - **Virtuális hálózati átjáró:** hello értéke fix, mivel az átjáró csatlakozik.
  - **Helyi hálózati átjáró:** kattintson **helyi hálózati átjáró kiválasztása** és select hello helyi hálózati átjáró, amelyet az toouse. A példában a **Site2**-t használjuk.
  - **Megosztott kulcs:** itt hello értéket meg kell egyeznie a hello érték, amely a helyi helyszíni VPN-eszköz alkalmaz. Hello példában használtuk "abc123", de lehet (és kell) használhat összetettebb. fontos dolog, hogy az itt megadott hello érték hello azonos értéket, a VPN-eszköz beállításakor megadott hello kell lennie.
  - fennmaradó értékek hello **előfizetés**, **erőforráscsoport**, és **hely** rögzítettek.

4. Kattintson a **OK** toocreate a kapcsolatot. Látni fogja, *kapcsolat létrehozása* flash hello képernyőn.
5. Hello kapcsolat megtekintheti a hello **kapcsolatok** hello virtuális hálózati átjáró panelen. hello állapot kerül a *ismeretlen* túl*csatlakozás*, majd túl*sikeres*.
