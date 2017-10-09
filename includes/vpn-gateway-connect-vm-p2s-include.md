Virtuális gép, amely telepített tooyour virtuális hálózatot hozzon létre egy távoli asztali kapcsolat tooyour VM tooa is elérheti. hello legjobb módja tooinitially győződjön meg arról, hogy a virtuális gép által tooconnect tooyour csatlakoztathatja a privát IP-cím, ahelyett, hogy a számítógép nevét. Ily módon teszteli toosee Ha is elérheti, nem e névfeloldás megfelelően van konfigurálva.

1. Keresse meg a hello magánhálózati IP-címet. Hello magánhálózati IP-címet a virtuális gépek található megtekintésével vagy hello hello hello Azure-portálon a virtuális gép tulajdonságainak vagy a PowerShell használatával.

  - Azure portál – keresse meg a virtuális gép hello Azure-portálon. Virtuális gép hello hello tulajdonságainak megtekintése. hello magánhálózati IP-cím szerepel.

  - PowerShell - használata hello példa tooview virtuális gépek és a erőforráscsoportokból magánhálózati IP-címek listáját. Nincs szükség a toomodify ebben a példában annak használata előtt.

    ```powershell
    $VMs = Get-AzureRmVM
    $Nics = Get-AzureRmNetworkInterface | Where VirtualMachine -ne $null

    foreach($Nic in $Nics)
    {
      $VM = $VMs | Where-Object -Property Id -eq $Nic.VirtualMachine.Id
      $Prv = $Nic.IpConfigurations | Select-Object -ExpandProperty PrivateIpAddress
      $Alloc = $Nic.IpConfigurations | Select-Object -ExpandProperty PrivateIpAllocationMethod
      Write-Output "$($VM.Name): $Prv,$Alloc"
    }
    ```

2. Ellenőrizze, hogy csatlakoztatott tooyour hello pont-pont VPN használatával VNet kapcsolat.
3. Nyissa meg **távoli asztali kapcsolat** hello tálcán hello keresőmezőbe írja be a "RDP" vagy "Távoli asztali kapcsolat", majd válassza ki a távoli asztali kapcsolat. Nyissa meg a távoli asztali kapcsolat hello "mstsc" parancsot a PowerShell használatával is. 
4. A távoli asztali kapcsolat esetén adja meg a hello VM hello magánhálózati IP-címe. Kattintson a "Beállítások megjelenítése" tooadjust további beállításokat, majd csatlakozzon.

### <a name="tootroubleshoot-an-rdp-connection-tooa-vm"></a>az RDP-kapcsolat tooa VM tootroubleshoot

Ha problémája van tooa virtuális gép a VPN-kapcsolaton keresztül csatlakozó, ellenőrizze a hello következőket:

- Ellenőrizze, hogy a VPN-kapcsolat sikeresen létrejött-e.
- Győződjön meg arról, hogy csatlakozik-e virtuális gép hello toohello magánhálózati IP-címet.
- Használja az "ipconfig" toocheck hello hozzárendelt, amelyből kapcsolódik hello számítógépen toohello Ethernet-adapter IPv4-címmel. Ha hello IP-cím hello Vnetben, amely csatlakozik hello címtartományán belül, vagy a VPNClientAddressPool hello címtartományán belül, egy átfedő címtér hivatkozott tooas. A címtartomány átfedésben van így, hello hálózati forgalom nem elérni az Azure, a helyi hálózaton hello marad.
- Csatlakozhat a virtuális gép toohello hello privát IP-cím, de nem hello a számítógép nevét, ellenőrizze, hogy DNS megfelelően van konfigurálva. A virtuális gépek névfeloldásának működésével kapcsolatos további információkért lásd [a virtuális gépek névfeloldásával](../articles/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) foglakozó cikket.
- Győződjön meg arról, hogy hello VPN-konfigurációs ügyfélcsomag készítésének után hello DNS-kiszolgáló IP-címek hello VNet lett megadva. Ha hello DNS-kiszolgáló IP-címének frissítése létrehozása, és egy új VPN-konfigurációs ügyfélcsomag telepítéséhez.
- Az RDP-kapcsolatok kapcsolatos további információkért lásd: [hibaelhárítása távoli asztali kapcsolatok tooa VM](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md).
