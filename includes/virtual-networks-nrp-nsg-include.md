## <a name="network-security-group"></a>Hálózati biztonsági csoport
Az NSG-erőforrás lehetővé teszi a munkaterhelések biztonsági határ hello létrehozását, implementálásával engedélyezése, és megtagadási szabályoknak. Ezek a szabályok lehetnek tooa virtuális gép, egy hálózati adapter vagy alhálózat alkalmazza.

| Tulajdonság | Leírás | Példaértékek |
| --- | --- | --- |
| **alhálózatok** |NSG vonatkozik, az alhálózati azonosítók hello listája. |/Subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/TestRG/Providers/Microsoft.Network/virtualNetworks/TestVNet/Subnets/FrontEnd |
| **securityRules** |Hello NSG alkotó biztonsági szabályok listája |Lásd: [biztonsági szabály](#Security-rule) alatt |
| **defaultSecurityRules** |Minden NSG található alapértelmezett biztonsági szabályok listája |Lásd: [alapértelmezett biztonsági szabályok](#Default-security-rules) alatt |

* **A biztonsági szabály** -egy NSG rendelkezhet több biztonsági szabály lett meghatározva. Minden egyes szabály engedélyezheti vagy tagadhatja különböző típusú forgalom.

### <a name="security-rule"></a>A biztonsági szabály
A szabály egy NSG-t az alábbi hello tulajdonságokat tartalmazó gyermek erőforrása.

| Tulajdonság | Leírás | Példaértékek |
| --- | --- | --- |
| **Leírás** |Hello szabály leírása |Bejövő adatforgalom engedélyezésére X alhálózatban lévő virtuális gépen |
| **protokoll** |Protokoll toomatch hello szabály |TCP, UDP vagy * |
| **sourcePortRange** |Source port tartomány toomatch hello szabály |80, 100-200, * |
| **destinationPortRange** |Cél port tartomány toomatch hello szabály |80, 100-200, * |
| **sourceAddressPrefix** |Forrás cím előtag toomatch hello szabály |10.10.10.1, 10.10.10.0/24, VirtualNetwork |
| **destinationAddressPrefix** |Rendeltetési cím előtag toomatch hello szabály |10.10.10.1, 10.10.10.0/24, VirtualNetwork |
| **iránya** |Forgalom toomatch hello szabály irányának |bejövő vagy kimenő |
| **prioritás** |Hello szabály prioritását. Szabályok prioritás szerinti sorrendben ellenőrzi, a szabály vonatkozik, ha nincsenek további szabályok kiértékelésének egyeztetéséhez. |10, 100, 65000 |
| **hozzáférés** |Ha hello szabálynak hozzáférés tooapply típusa |engedélyezés vagy megtagadás |

NSG a JSON formátum. minta:

    {
        "name": "NSG-BackEnd",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/networkSecurityGroups",
        "location": "westus",
        "tags": {
            "displayName": "NSG - Front End"
        },
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "securityRules": [
                {
                    "name": "rdp-rule",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd/securityRules/rdp-rule",
                    "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "description": "Allow RDP",
                        "protocol": "Tcp",
                        "sourcePortRange": "*",
                        "destinationPortRange": "3389",
                        "sourceAddressPrefix": "Internet",
                        "destinationAddressPrefix": "*",
                        "access": "Allow",
                        "priority": 100,
                        "direction": "Inbound"
                    }
                }
            ],
            "defaultSecurityRules": [
                { [...],
            "subnets": [
                {
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd"
                }
            ]
        }
    }

### <a name="default-security-rules"></a>Alapértelmezett biztonsági szabályok

Alapértelmezett biztonsági szabály rendelkezik hello ugyanazok a tulajdonságok a biztonsági szabályok érhető el. Hálózati kapcsolat tooprovide alkalmazott NSG-k toothem rendelkező erőforrások között vannak ilyenek. Ellenőrizze, hogy tudja, amely [alapértelmezett biztonsági szabályok](../articles/virtual-network/virtual-networks-nsg.md#default-rules) létezik.

### <a name="additional-resources"></a>További források
* További információk [NSG-k](../articles/virtual-network/virtual-networks-nsg.md).
* Olvasási hello [REST API referenciadokumentációt](https://msdn.microsoft.com/library/azure/mt163615.aspx) az NSG-ket.
* Olvasási hello [REST API referenciadokumentációt](https://msdn.microsoft.com/library/azure/mt163580.aspx) a biztonsági szabályok.
