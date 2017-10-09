## <a name="public-ip-address"></a>Nyilvános IP-cím
Egy nyilvános IP-cím erőforrás vagy egy fenntartott vagy dinamikus internetes IP-címet tartalmaz. Bár létrehozhat egy nyilvános IP-cím megegyezik egy önálló, tooassociate kell azt tooanother objektum tooactually hello címet használja. A nyilvános IP cím tooa terheléselosztó, Alkalmazásátjáró vagy egy hálózati adapter tooprovide Internet access toothose erőforrásokat lehet társítani.  

| Tulajdonság | Leírás | Példaértékek |
| --- | --- | --- |
| **publicIPAllocationMethod** |Meghatározza, hogy az IP-cím hello *statikus* vagy *dinamikus*. |statikus, dinamikus |
| **idleTimeoutInMinutes** |Meghatározza a hello üresjárati időkorlátot túllépő, 4 perces alapértelmezett értékkel. Ha egy adott munkamenethez nincs további csomagok ezen időn belül érkezik, hello munkamenet megszakítása. |4 és 30 közötti értéket |
| **IP-cím** |IP-cím hozzárendelése tooobject. Ez a tulajdonság csak olvasható. |104.42.233.77 |

### <a name="dns-settings"></a>DNS-beállítások
Nyilvános IP-címek rendelkezik nevű gyermekobjektum **dnsSettings** tartalmazó hello következő tulajdonságai:

| Tulajdonság | Leírás | Példaértékek |
| --- | --- | --- |
| **domainNameLabel** |Nevű a névfeloldáshoz használnak. |a webszolgáltatáshoz, ftp, vm1 |
| **teljesen minősített tartományneve** |Teljesen minősített neve hello nyilvános IP-cím. |www.westus.cloudapp.Azure.com |
| **reverseFqdn** |Teljesen minősített tartománynevét, amely feloldja toohello IP-cím és DNS PTR-rekordot, regisztrálva van. |www.contoso.com. |

A minta nyilvános IP-cím JSON formátumban:

    {
       "name": "PIP01",
       "location": "North US",
       "tags": { "key": "value" },
       "properties": {
          "publicIPAllocationMethod": "Static",
          "idleTimeoutInMinutes": 4,
          "ipAddress": "104.42.233.77",
          "dnsSettings": {
             "domainNameLabel": "mylabel",
             "fqdn": "mylabel.westus.cloudapp.azure.com",
             "reverseFqdn": "contoso.com."
          }
       }
    } 

### <a name="additional-resources"></a>További források
* További információk [nyilvános IP-címek](../articles/virtual-network/virtual-networks-reserved-public-ip.md).
* További tudnivalók [szintű nyilvános IP-címek példány](../articles/virtual-network/virtual-networks-instance-level-public-ip.md).
* Olvasási hello [REST API referenciadokumentációt](https://msdn.microsoft.com/library/azure/mt163638.aspx) nyilvános IP-címek.

