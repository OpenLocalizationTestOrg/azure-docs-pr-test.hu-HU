## <a name="virtual-network"></a>Virtual Network
Virtuális hálózatok (VNET) és alhálózatok erőforrások segítségével határozza meg az Azure-ban futó munkaterhelések biztonsági korlátot. Egy VNet jellemzőek, címterekhez, meghatározott CIDR-blokkok gyűjteménye. 

> [!NOTE]
> A hálózati rendszergazdák jártas CIDR-formátumban. Ha nem ismeri a CIDR, [olvashat azokról bővebben](http://whatismyipaddress.com/cidr).
> 
> 

![Virtuális hálózat, több alhálózattal](./media/resource-groups-networking/Figure4.png)

Vnetek hello következő tulajdonságai tartalmaznak.

| Tulajdonság | Leírás | Példaértékek |
| --- | --- | --- |
| **Címtartományt** |A CIDR jelölésrendszer VNet hello alkotó címelőtagokat gyűjteménye |192.168.0.0/16 |
| **alhálózatok** |Hello virtuális hálózatot alkotó alhálózatok gyűjteménye |Lásd: [alhálózatok](#Subnets) alatt. |
| **IP-cím** |IP-cím hozzárendelése tooobject. Ez a tulajdonság csak olvasható. |104.42.233.77 |

### <a name="subnets"></a>Alhálózatok
Egy alhálózat egy Vnetet gyermek erőforrása, és segítségével meghatározhatja a címterek belül használja az IP-cím előtagokat CIDR-blokkja részeit. Hálózati adapter toosubnets, és a csatlakoztatott tooVMs biztosítható a kapcsolat a különböző munkaterhelések lehet hozzáadni.

Alhálózatok hello következő tulajdonságai tartalmaznak. 

| Tulajdonság | Leírás | Példaértékek |
| --- | --- | --- |
| **addressPrefix** |Egyetlen címelőtag fel hello alhálózat CIDR-jelöléssel |192.168.1.0/24 |
| **hálózati biztonsági csoporthoz tartozik** |Alkalmazott NSG toohello alhálózati |Lásd: [NSG-k](#Network-Security-Group) |
| **Migrálták** |Az útvonaltábla alkalmazott toohello alhálózati |Lásd: [UDR](#Route-table) |
| **IP-konfigurációk** |Hálózati adapter csatlakoztatott toohello alhálózati által használt IP-konfigurációs objektumok gyűjteménye |Lásd: [UDR](#Route-table) |

A minta VNet JSON formátumban:

    {
        "name": "TestVNet",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/virtualNetworks",
        "location": "westus",
        "tags": {
            "displayName": "VNet"
        },
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "addressSpace": {
                "addressPrefixes": [
                    "192.168.0.0/16"
                ]
            },
            "subnets": [
                {
                    "name": "FrontEnd",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "192.168.1.0/24",
                        "networkSecurityGroup": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd"
                        },
                        "routeTable": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-FrontEnd"
                        },
                        "ipConfigurations": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConfigurations/ipconfig1"
                            },
                            ...]
                    }
                },
                ...]
        }
    }

### <a name="additional-resources"></a>További források
* További információk [VNet](../articles/virtual-network/virtual-networks-overview.md).
* Olvasási hello [REST API referenciadokumentációt](https://msdn.microsoft.com/library/azure/mt163650.aspx) tartozó Vnetek esetében.
* Olvasási hello [REST API referenciadokumentációt](https://msdn.microsoft.com/library/azure/mt163618.aspx) alhálózatok esetében.

