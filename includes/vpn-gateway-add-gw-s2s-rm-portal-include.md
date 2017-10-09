1. Kattintson a bal oldali portál oldalán található hello hello,  **+**  , és írja be a "Virtuális hálózati átjáró" keresésben. Az **Eredmények** között keresse meg a **Virtuális hálózati átjáró** elemet, és kattintson rá.
2. Hello hello "Virtuális hálózati átjáró" panel alsó részén, kattintson a **létrehozása**. Ekkor megnyílik a hello **virtuális hálózati átjáró létrehozása** panelen.

    ![Virtuális hálózati átjáró létrehozása panel mezői](./media/vpn-gateway-add-gw-s2s-rm-portal-include/vnet_gw.png "Új átjáró")

3. A hello **virtuális hálózati átjáró létrehozása** panelen adja meg a virtuális hálózati átjáró hello értékeket.

  - **Név**: adjon nevet az átjárónak. A rendszer nem hello ugyanaz, mint egy átjáró alhálózatának elnevezése. Azt a hello átjáró objektum létrehozásakor hello nevét.
  - **Átjáró típusa**: válassza ki a **VPN** elemet. VPN-átjárók használata hello virtuális hálózati átjáró típusát **VPN**. 
  - **VPN-típus**: válassza ki a konfigurációjához megadott hello VPN-típust. A legtöbb konfigurációhoz útvonalalapú VPN-típus szükséges.
  - **SKU**: Select hello gateway SKU hello legördülő menüből. hello a legördülő listában hello termékváltozatok hello választja, a VPN-típus függ. Az átjáró-termékváltozatokkal kapcsolatos további információkért lásd: [Gateway SKUs](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku) (Átjáró-termékváltozatok).
  - **Hely**: tooscroll toosee hely szükség lehet. Hello beállítása **hely** mezőben toopoint toohello helyet, ahol a virtuális hálózat található. Hello helye nem mutat toohello régió, ahol a virtuális hálózaton található, ha hello virtuális hálózati nem jelenik meg a következő lépésben hello "Válasszon egy virtuális hálózatot" legördülő.
  - **Virtuális hálózati**: válassza ki a virtuális hálózati toowhich hello szeretné tooadd ezt az átjárót. Kattintson a **virtuális hálózati** tooopen hello "Válasszon egy virtuális hálózatot" panelen. Válassza ki a virtuális hálózat hello. Ha nem látja a virtuális hálózat, ellenőrizze, hogy hello a Location mező mutat toohello régió, ahol a virtuális hálózat is található.
  - **Nyilvános IP-cím**: hello "Nyilvános IP-cím létrehozása" panel hoz létre egy nyilvános IP-cím objektum. hello nyilvános IP-cím hello VPN-átjáró létrehozásakor dinamikusan hozzárendelt. A VPN Gateway jelenleg csak a *Dinamikus* nyilvános IP-cím lefoglalását támogatja. Ez azonban nem jelenti azt, hogy hello IP-cím hozzá van rendelve tooyour VPN-átjáró után módosítja. hello egyetlen alkalom hello nyilvános IP-cím módosításainak mikor van hello átjáró törlődik, és újból létrehozza. Nem módosul átméretezés, alaphelyzetbe állítás, illetve a VPN Gateway belső karbantartása/frissítése során.

    - Első lépésként kattintson **nyilvános IP-cím** tooopen panel "Nyilvános IP-cím kiválasztása" hello, majd kattintson az **+ új** tooopen hello "Nyilvános IP-cím létrehozása" panelen.
    - Ezt követően adjon meg egy **neve** a nyilvános IP-címhez, majd kattintson a **OK** : hello alsó részén a panel toosave a módosításokat.

      ![Nyilvános IP-cím létrehozása](./media/vpn-gateway-add-gw-s2s-rm-portal-include/pip.png "PIP létrehozása")

4. Ellenőrizze a hello beállításait. Kiválaszthatja **PIN-kód toodashboard** hello panelt, ha azt szeretné, hogy az átjáró tooappear hello irányítópult hello alján. 
5. Kattintson a **létrehozása** toobegin hello VPN-átjáró létrehozásához. hello beállítások lesznek érvényesítve, és látni fogja, hello "A virtuális hálózati átjáró" hello irányítópult csempét. Átjáró létrehozása akár is igénybe vehet too45 perc. Szükség lehet toorefresh a portál toosee befejeződött hello állapotát.

Hello átjáró létrehozása után megtekintheti a hello IP-cím lett rendelve tooit hello virtuális hálózati hello portálon megtekintésével. hello átjáró csatlakoztatott eszközként jelenik meg. Kattinthat hello csatlakoztatott eszközre (a virtuális hálózati átjáró) tooview további információt.
