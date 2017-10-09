## <a name="how-toocreate-a-classic-vnet-using-azure-cli"></a>Hogyan toocreate egy klasszikus virtuális hálózatot az Azure parancssori felület használatával
Hello Azure CLI toomanage használhatja az Azure-erőforrások bármely olyan számítógépről, a Windows, Linux vagy os x rendszerű hello parancssorból. egy VNet hello Azure parancssori felület használatával toocreate kövesse hello lépéseket.

1. Ha még sosem használta az Azure parancssori felület, lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../articles/cli-install-nodejs.md) hello utasítások mentése toohello pont, ahol ki kell választania az Azure-fiókja és -előfizetést.
2. Futtassa a hello **azure-hálózat virtuális hálózat létrehozása** parancs toocreate egy VNet és alhálózat, alább látható módon. hello kimenet után látható hello lista hello paramétereket ismerteti.
   
            azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
   
    Várt kimenet:
   
            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK
   
   * **--vnet**. Hello VNet toobe létrehozott neve. A mi esetünkben *TestVNet*
   * **-e (vagy--címtartomány)**. Virtuális hálózat címterület. A mi esetünkben *192.168.0.0*
   * **-i (vagy - cidr)**. Hálózati maszk CIDR formátumban. A mi esetünkben *16*.
   * **-n (vagy--alhálózatneve**). Hello első alhálózat neve. A mi esetünkben *FrontEnd*.
   * **-p (vagy--ip-alhálózat-start)**. Kezdő IP-cím alhálózati vagy alhálózati címtartományt. A mi esetünkben *192.168.1.0*.
   * **-r (vagy--alhálózat-cidr)**. Hálózati maszk alhálózat CIDR formátumban. A mi esetünkben *24*.
   * **-l (vagy --location)**. Azure-régió, ahol hello VNet létrejön. A mi esetünkben *USA középső RÉGIÓJA*.
3. Futtassa a hello **azure vnet alhálózaton létrehozása** parancs toocreate alhálózat alább látható módon. hello kimenet után látható hello lista hello paramétereket ismerteti.
   
            azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
   
    Itt egy hello parancs fenti hello várt kimenet:
   
            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up hello subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK
   
   * **-t (vagy--vnet-name**. Hello ahol hello alhálózati létrejön a VNet neve. A mi esetünkben *TestVNet*.
   * **-n (vagy --name)**. Hello új alhálózat neve. A mi esetünkben *háttér*.
   * **-a (vagy --address-prefix)**. Az alhálózat CIDR-blokkja. Négy esetünkben *192.168.2.0/24*.
4. Futtassa a hello **azure hálózati vnet show** tooview hello hello új vnet tulajdonságainak parancsot a lent látható módon.
   
            azure network vnet show
   
    Itt egy hello parancs fenti hello várt kimenet:
   
            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up hello virtual network sites
            data:    Name                            : TestVNet
            data:    Location                        : Central US
            data:    State                           : Created
            data:    Address space                   : 192.168.0.0/16
            data:    Subnets:
            data:      Name                          : FrontEnd
            data:      Address prefix                : 192.168.1.0/24
            data:
            data:      Name                          : BackEnd
            data:      Address prefix                : 192.168.2.0/24
            data:
            info:    network vnet show command OK

