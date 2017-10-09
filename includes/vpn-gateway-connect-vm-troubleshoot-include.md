Ha problémája van tooa virtuális gép a VPN-kapcsolaton keresztül csatlakozó, ellenőrizze a hello következőket:

- Ellenőrizze, hogy a VPN-kapcsolat sikeresen létrejött-e.
- Győződjön meg arról, hogy csatlakozik-e virtuális gép hello toohello magánhálózati IP-címet.
- Csatlakozhat a virtuális gép toohello hello privát IP-cím, de nem hello a számítógép nevét, ellenőrizze, hogy DNS megfelelően van konfigurálva. A virtuális gépek névfeloldásának működésével kapcsolatos további információkért lásd [a virtuális gépek névfeloldásával](../articles/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) foglakozó cikket.

Pont-pont keresztül kapcsolódik, ellenőrizze a következő további elemek hello:

- Használja az "ipconfig" toocheck hello hozzárendelt, amelyből kapcsolódik hello számítógépen toohello Ethernet-adapter IPv4-címmel. Ha hello IP-cím hello Vnetben, amely csatlakozik hello címtartományán belül, vagy a VPNClientAddressPool hello címtartományán belül, egy átfedő címtér hivatkozott tooas. A címtartomány átfedésben van így, hello hálózati forgalom nem elérni az Azure, a helyi hálózaton hello marad.
- Győződjön meg arról, hogy hello VPN-konfigurációs ügyfélcsomag készítésének után hello DNS-kiszolgáló IP-címek hello VNet lett megadva. Ha hello DNS-kiszolgáló IP-címének frissítése létrehozása, és egy új VPN-konfigurációs ügyfélcsomag telepítéséhez.

Egy RDP-kapcsolat hibaelhárítással kapcsolatos további információkért lásd: [hibaelhárítása távoli asztali kapcsolatok tooa VM](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md).
