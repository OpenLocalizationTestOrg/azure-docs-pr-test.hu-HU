## <a name="nic"></a>A HÁLÓZATI ADAPTER
Kártya (NIC) erőforrás hálózati illesztő hálózati kapcsolat tooan meglévő alhálózat virtuális hálózat az erőforrás biztosít. Bár létrehozhat egy hálózati adapter nem önálló objektumként, tooassociate kell azt tooanother objektum tooactually biztosít kapcsolatot. A hálózati adapter által használt tooconnect tooa alhálózatot, egy nyilvános IP-címet vagy egy terheléselosztó lehet.  

| Tulajdonság | Leírás | Példaértékek |
| --- | --- | --- |
| **virtualMachine** |Virtuális gép hello hálózati adapter társítva van. |/Subscriptions/{GUID}/../microsoft.COMPUTE/virtualMachines/vm1 |
| **macAddress** |Hello hálózati adapter MAC-címe |4 és 30 közötti értéket |
| **hálózati biztonsági csoporthoz tartozik** |Társított NSG-t toohello hálózati adapter |/Subscriptions/{GUID}/../microsoft.Network/networkSecurityGroups/myNSG1 |
| **dnsSettings** |Hello hálózati adapter DNS-beállításait |Lásd: [PIP](#Public-IP-address) |

A hálózati kártyát, vagy a hálózati adapter, jelöli meg a hálózati adaptert, amely társított tooa virtuális gép (VM). A virtuális gép egy vagy több hálózati adapterrel rendelkezhet.

![A hálózati adapter által a egyetlen virtuális gép](./media/resource-groups-networking/Figure3.png)

### <a name="ip-configurations"></a>IP-konfigurációk
Hálózati adapterei nevű gyermekobjektum **IP-konfigurációk** tartalmazó hello következő tulajdonságai:

| Tulajdonság | Leírás | Példaértékek |
| --- | --- | --- |
| **alhálózat** |Alhálózati hello NIC való onnected. |/Subscriptions/{GUID}/../microsoft.Network/virtualNetworks/myvnet1/Subnets/mysub1 |
| **privateipaddress tulajdonságot** |Hello NIC hello alhálózat IP-címe |10.0.0.8 |
| **címkiosztási** |IP-címkiosztási módszerét |Dinamikus vagy statikus |
| **enableIPForwarding** |E hello hálózati adapter is használható Útválasztás |IGAZ vagy HAMIS eredményt ad |
| **elsődleges** |-E a hálózati adapter hello hello elsődleges hálózati hello méretű VM |IGAZ vagy HAMIS eredményt ad |
| **Nyilvános** |A PIP társított hálózati adapter hello |Lásd: [DNS-beállítások](#DNS-settings) |
| **konfigurációja terheléselosztói Háttércímkészletet** |Biztonsági másolatot a záró cím készletek hello hálózati adapter társítva | |
| **loadBalancerInboundNatRules** |A bejövő terhelés terheléselosztó NAT szabályok hello hálózati adapter társítva | |

A minta nyilvános IP-cím JSON formátumban:

    {
        "name": "lb-nic1-be",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/networkInterfaces",
        "location": "eastus",
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "ipConfigurations": [
                {
                    "name": "NIC-config",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/NIC-config",
                    "etag": "W/\"0027f1a2-3ac8-49de-b5d5-fd46550500b1\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "privateIPAddress": "10.0.0.4",
                        "privateIPAllocationMethod": "Dynamic",
                        "subnet": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet"
                        },
                        "loadBalancerBackendAddressPools": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool"
                            }
                        ],
                        "loadBalancerInboundNatRules": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1"
                            }
                        ]
                    }
                }
            ],
            "dnsSettings": { ... },
            "macAddress": "00-0D-3A-10-F1-29",
            "enableIPForwarding": false,
            "primary": true,
            "virtualMachine": {
                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Compute/virtualMachines/web1"
            }
        }
    }

### <a name="additional-resources"></a>További források
* Olvasási hello [REST API referenciadokumentációt](https://msdn.microsoft.com/library/azure/mt163579.aspx) a hálózati adapterek.

