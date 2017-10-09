a következő tábla listák hello beállítása PolicyBased és RouteBased VPN-átjáró követelményei hello. Ez a táblázat tooboth hello Resource Manager és klasszikus üzembe helyezési modellre vonatkozik. Hello Klasszikus modell PolicyBased VPN-átjárók vannak hello ugyanaz, mint a statikus átjárók, és útválasztó-alapú átjárók vannak hello ugyanaz, mint a dinamikus átjárók.

|  | **PolicyBased alapszintű VPN Gateway** | **RouteBased alapszintű VPN Gateway** | **RouteBased Standard VPN Gateway** | **RouteBased nagy teljesítményű VPN Gateway** |
| --- | --- | --- | --- | --- |
| **Pont-pont kapcsolat (S2S)** |PolicyBased VPN-konfiguráció |RouteBased VPN-konfiguráció |RouteBased VPN-konfiguráció |RouteBased VPN-konfiguráció |
| **Pont–hely típusú kapcsolat (P2S**) |Nem támogatott |Támogatott (párhuzamosan használható az S2S mellett) |Támogatott (párhuzamosan használható az S2S mellett) |Támogatott (párhuzamosan használható az S2S mellett) |
| **Hitelesítési módszer** |Előre megosztott kulcs |Előre megosztott kulcs S2S-kapcsolatokhoz, tanúsítvány P2S-kapcsolatokhoz |Előre megosztott kulcs S2S-kapcsolatokhoz, tanúsítvány P2S-kapcsolatokhoz |Előre megosztott kulcs S2S-kapcsolatokhoz, tanúsítvány P2S-kapcsolatokhoz |
| **S2S-kapcsolatok maximális száma** |1 |10 |10 |30 |
| **P2S-kapcsolatok maximális száma** |Nem támogatott |128 |128 |128 |
| **Aktív útválasztás-támogatás (BGP)** |Nem támogatott |Nem támogatott |Támogatott |Támogatott |

