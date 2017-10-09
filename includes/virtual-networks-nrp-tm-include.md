## <a name="traffic-manager-profile"></a>Traffic Manager-profil
A TRAFFIC manager és a gyermek végpont útválasztási tooendpoints DNS az Azure-ban, és az Azure-on kívüli engedélyezése Ilyen forgalomeloszlás, útválasztási házirend metódusok szabályozzák. A TRAFFIC manager is lehetővé teszi a végpont állapotfigyelő toobe figyeli, és megfelelő módon használják fel forgalmat a végpontok hello állapotának alapján. 

| Tulajdonság | Leírás |
| --- | --- |
| **trafficRoutingMethod** |a lehetséges értékek: *teljesítmény*, *Weighted*, és *prioritása* |
| **dnsConfig** |Hello-profil teljes Tartományneve |
| **Protocol (Protokoll)** |figyelés a protokollt, a lehetséges értékek: *HTTP* és *HTTPS* |
| **Port** |figyelési port |
| **Elérési út** |figyelési elérési út |
| **Végpontok** |végpont erőforrások tárolója |

### <a name="endpoint"></a>Végpont
A végpont a Traffic Manager-profil gyermek erőforrása. Azt jelenti, hogy egy szolgáltatás, vagy webes végpont toowhich felhasználói adatforgalom elosztása konfigurált hello házirend alapján virtualizál hello Traffic Manager-profil erőforrás. 

| Tulajdonság | Leírás |
| --- | --- |
| **Típus** |hello típus hello végpont, a lehetséges értékek: *Azure végpont*, *külső végpont*, és *beágyazott végpont* |
| **targetresourceid azonosítója** |egy szolgáltatás vagy webes végpont nyilvános IP-címe. Ez lehet egy Azure-bA vagy külső végpont. |
| **Súlyozás** |végpont súly használt forgalom kezeléséhez. |
| **Priority (Prioritás)** |hello a végponthoz, a feladatátvételi művelet használt toodefine prioritása |

A Traffic Manager minta Json formátumban: 

        {
            "apiVersion": "[variables('tmApiVersion')]",
            "type": "Microsoft.Network/trafficManagerProfiles",
            "name": "VMEndpointExample",
            "location": "global",
            "dependsOn": [
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '0')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '1')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '2')]",
            ],
            "properties": {
                "profileStatus": "Enabled",
                "trafficRoutingMethod": "Weighted",
                "dnsConfig": {
                    "relativeName": "[parameters('dnsname')]",
                    "ttl": 30
                },
                "monitorConfig": {
                    "protocol": "http",
                    "port": 80,
                    "path": "/"
                },
                "endpoints": [
                    {
                        "name": "endpoint0",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 0))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint1",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 1))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint2",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 2))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    }
                ]
            }
        }


## <a name="additional-resources"></a>További források
Olvasási [REST API-dokumentáció a Traffic Manager](https://msdn.microsoft.com/library/azure/mt163664.aspx) további információt.

