A DNS-zónák használt toohost hello DNS-rekordok az adott tartományban. az Azure DNS, a tartomány toostart toocreate DNS-zóna van szüksége a tartománynevet. Ezután a tartománya összes DNS-rekordja ebben a DNS-zónában jön létre.

Hello "contoso.com" tartomány például számos DNS-rekordokat, például "mail.contoso.com" (levelezési kiszolgálóhoz) és "www.contoso.com" (webhelyhez) is tartalmazhat.

DNS-zóna Azure DNS-ben való létrehozásakor:

* hello zóna neve hello hello erőforráscsoporton belül egyedinek kell lennie, és hello zóna nem kell már létezik. Ellenkező esetben hello művelet sikertelen lesz.
* hello egyes zónanevek felhasználhatók egy másik erőforráscsoportban vagy egy másik Azure-előfizetésben.
* Ha több zóna rendelkezik hello ugyanazzal a névvel minden példány hozzá van rendelve különböző névkiszolgáló-címet. Címek csak egy készletét hello regisztrációs konfigurálható.

> [!NOTE]
> Nincs tooown a tartomány neve toocreate ezt a nevet a DNS-zóna Azure DNS-ben. Azonban szüksége tooown hello tartomány tooconfigure hello Azure DNS névkiszolgálóit, hello megfelelő névkiszolgálóit hello regisztrációs hello a tartománynevet.
> 
> További információkért lásd: [delegálása a tartományi tooAzure DNS](../articles/dns/dns-domain-delegation.md).
