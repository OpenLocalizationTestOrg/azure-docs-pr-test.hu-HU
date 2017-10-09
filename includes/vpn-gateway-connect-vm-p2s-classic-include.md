Virtuális gép, amely telepített tooyour virtuális hálózatot hozzon létre egy távoli asztali kapcsolat tooyour VM tooa is elérheti. hello legjobb módja tooinitially győződjön meg arról, hogy a virtuális gép által tooconnect tooyour csatlakoztathatja a privát IP-cím, ahelyett, hogy a számítógép nevét. Ily módon teszteli toosee Ha is elérheti, nem e névfeloldás megfelelően van konfigurálva. 

1. Keresse meg a hello magánhálózati IP-címet a virtuális gép számára. Hello magánhálózati IP-címet a virtuális gépek található megtekintésével vagy hello hello hello Azure-portálon a virtuális gép tulajdonságainak vagy a PowerShell használatával.
2. Ellenőrizze, hogy csatlakoztatott tooyour hello pont-pont VPN használatával VNet kapcsolat. 
3. Távoli asztali kapcsolat megnyitásához írja be a "RDP" vagy "Távoli asztali kapcsolat" hello keresőmezőbe hello tálcán, majd válassza ki a távoli asztali kapcsolat. Nyissa meg a távoli asztali kapcsolat hello "mstsc" parancsot a PowerShell használatával is. 
3. A távoli asztali kapcsolat esetén adja meg a hello VM hello magánhálózati IP-címe. Kattintson a "Beállítások megjelenítése" tooadjust további beállításokat, majd csatlakozzon.

### <a name="tootroubleshoot-an-rdp-connection-tooa-vm"></a>az RDP-kapcsolat tooa VM tootroubleshoot

Ha problémája van tooa virtuális gép a VPN-kapcsolaton keresztül csatlakozó, dolgot néhány ellenőrizheti. További hibaelhárítási információkért lásd: [hibaelhárítása távoli asztali kapcsolatok tooa VM](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md).

- Ellenőrizze, hogy a VPN-kapcsolat sikeresen létrejött-e.
- Győződjön meg arról, hogy csatlakozik-e virtuális gép hello toohello magánhálózati IP-címet.
- Használja az "ipconfig" toocheck hello hozzárendelt, amelyből kapcsolódik hello számítógépen toohello Ethernet-adapter IPv4-címmel. Ha hello IP-cím hello Vnetben, amely csatlakozik hello címtartományán belül, vagy a VPNClientAddressPool hello címtartományán belül, egy átfedő címtér hivatkozott tooas. A címtartomány átfedésben van így, hello hálózati forgalom nem elérni az Azure, a helyi hálózaton hello marad.
- Csatlakozhat a virtuális gép toohello hello privát IP-cím, de nem hello a számítógép nevét, ellenőrizze, hogy DNS megfelelően van konfigurálva. A virtuális gépek névfeloldásának működésével kapcsolatos további információkért lásd [a virtuális gépek névfeloldásával](../articles/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) foglakozó cikket.
- Győződjön meg arról, hogy hello VPN-konfigurációs ügyfélcsomag készítésének után hello DNS-kiszolgáló IP-címek hello VNet lett megadva. Ha hello DNS-kiszolgáló IP-címének frissítése létrehozása, és egy új VPN-konfigurációs ügyfélcsomag telepítéséhez.
