Nyissa meg a portot, vagy hozzon létre egy végpontot, tooa virtuális gép (VM) az Azure-ban egy alhálózatot vagy a virtuális gép hálózati illesztő hálózati szűrő létrehozásával. Ezek a szűrők, amely szabályozza a bejövő és kimenő forgalmat, hello forgalmat fogadó csatlakoztatott hálózati biztonsági csoport toohello erőforrás helyezhető el.

Ilyenek például a webes forgalom most használja a 80-as porton. Ha már van egy virtuális Gépet, amely konfigurált tooserve webes kérelmek hello szabványos TCP 80-as portjához (ne feledje toostart hello megfelelő szolgáltatásokat, és nyissa meg a bármely OS tűzfalszabályok a virtuális gép hello is), hogy:

1. Hálózati biztonsági csoport létrehozása.
2. Hozzon létre egy bejövő forgalomra vonatkozó szabály átengedi a forgalmat:
   * hello Célporttartomány "80"
   * a Forrásporttartomány hello "*" (amely lehetővé teszi bármely forrásportból)
   * a prioritás értéke kevesebb 65 500 (toobe magasabb a prioritás hello catch – minden alapértelmezett megtagadási bejövő szabály)
3. Hello hálózati biztonsági csoporthoz társítandó hello VM hálózati adapter vagy az alhálózat.

Létrehozhat összetett hálózati konfiguráció toosecure a környezet hálózati biztonsági csoportok vagy a szabályok használatával. A példában csak egy vagy két olyan szabályok, amelyek lehetővé teszik a HTTP-forgalom vagy a távoli felügyelet. További információkért lásd: hello következő ["További információk"](#more-information-on-network-security-groups) szakasz vagy [Mi az a hálózati biztonsági csoport?](../articles/virtual-network/virtual-networks-nsg.md)

