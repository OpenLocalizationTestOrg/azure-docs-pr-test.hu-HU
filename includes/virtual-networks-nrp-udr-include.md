## <a name="route-tables"></a>Az útvonaltáblák
Útválasztási táblázat erőforrások hogyan a forgalom az Azure-infrastruktúra belül használt útvonalak toodefine tartalmazza. Felhasználó által definiált útvonalak (UDR) toosend használhatja egy adott alhálózaton tooa virtuális készüléknek, például egy tűzfal vagy behatolás észlelési rendszert (Azonosítók) származó összes forgalmat. Egy útvonal tábla toosubnets lehet társítani. 

Az útvonaltáblák hello következő tulajdonságai tartalmaznak.

| Tulajdonság | Leírás | Példaértékek |
| --- | --- | --- |
| **útvonalak** |Felhasználó által megadott útvonalak hello útvonaltábla |Lásd: [felhasználó által megadott útvonalak](#User-defined-routes) |
| **alhálózatok** |Alhálózatok hello útvonaltábla gyűjteménye túl vonatkozik.|Lásd: [alhálózatok](#Subnets) |

### <a name="user-defined-routes"></a>Felhasználó által definiált útvonalak
Létrehozhat udr-EK toospecify, ahol forgalmat kell küldeni, a cél címe alapján. Az eltolásokat tekintheti útvonal hello alapértelmezett átjáró definíciófrissítések a hálózati csomagok hello célcím alapján.

Udr-EK hello következő tulajdonságai tartalmaznak. 

| Tulajdonság | Leírás | Példaértékek |
| --- | --- | --- |
| **addressPrefix** |Cím előtagján vagy a teljes IP-cím hello cél |192.168.1.0/24, 192.168.1.101 |
| **nexthoptype elem** |Eszköz hello forgalomtípushoz túl küld|VirtualAppliance, VPN-átjárót, az Internet |
| **nexthopipaddress eleme** |Hello következő ugrás IP-címe |192.168.1.4 |

A minta útvonaltábla JSON formátumban:

    {
        "name": "UDR-BackEnd",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-BackEnd",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/routeTables",
        "location": "westus",
        "properties": {
            "provisioningState": "Succeeded",
            "routes": [
                {
                    "name": "RouteToFrontEnd",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-BackEnd/routes/RouteToFrontEnd",
                    "etag": "W/\"v\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "192.168.1.0/24",
                        "nextHopType": "VirtualAppliance",
                        "nextHopIpAddress": "192.168.0.4"
                    }
                }
            ],
            "subnets": [
                {
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd"
                }
            ]
        }
    }

### <a name="additional-resources"></a>További források
* További információk [udr-EK](../articles/virtual-network/virtual-networks-udr-overview.md).
* Olvasási hello [REST API referenciadokumentációt](https://msdn.microsoft.com/library/azure/mt502549.aspx) az útválasztási táblázatokat.
* Olvasási hello [REST API referenciadokumentációt](https://msdn.microsoft.com/library/azure/mt502539.aspx) felhasználó által megadott útvonalak (udr-EK).

