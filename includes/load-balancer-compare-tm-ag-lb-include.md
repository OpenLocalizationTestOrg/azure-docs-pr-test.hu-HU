## <a name="load-balancer-differences"></a>Terheléselosztási különbségek

Nincsenek más beállítások toodistribute hálózati forgalmat a Microsoft Azure használatával. Ezek a lehetőségek különböző működési elveken alapulnak, különböző funkciókészlettel rendelkeznek és különböző forgatókönyveket támogatnak. Elkülönítve vagy egymással kombinálva egyaránt használhatók.

* **Az Azure Load Balancer** hello (réteg 4 hello OSI hálózati referencia-verem) szállítási rétegben működik. Hálózati szintű terjesztési forgalom hello alkalmazás példányai között biztosít az Azure adatközpontba.
* **Alkalmazásátjáró** hello (réteg 7 hello OSI hálózati referencia-verem) alkalmazás rétegben működik. Úgy működik, mint egy fordított proxy szolgáltatás hello ügyfélkapcsolat leáll, és a továbbítási kérelmek tooback-a befejezési végpontok.
* **A TRAFFIC Manager** hello DNS szint működik.  DNS válaszok toodirect végfelhasználói forgalom elosztott tooglobally végpontokat használ. Az ügyfelek ezután csatlakoznak toothose végpontok közvetlenül.

a következő táblázat hello minden egyes szolgáltatás által kínált hello szolgáltatásokat foglalja össze:

| Szolgáltatás | Azure Load Balancer | Application Gateway | Traffic Manager |
| --- | --- | --- | --- |
| Technológia |Forgalmi szint (4. szint) |Alkalmazásszint (7. szint) |DNS-szint |
| Támogatott alkalmazásprotokollok |Bármelyik |HTTP, HTTPS és WebSockets |Bármelyik (a végpontmonitoringhoz szükség van egy HTTP-végpontra) |
| Végpontok |Azure-os virtuális gépek és Cloud Services-szerepkörpéldányok |Bármely belső Azure IP-cím, nyilvános internetes IP-cím, Azure-beli virtuális gép vagy Azure-felhőszolgáltatás |Azure-os virtuális gépek, Cloud Services-példányok, Azure Web Apps és külső végpontok |
| Vnet-támogatás |Internetes és belső (virtuális hálózati) alkalmazásokhoz egyaránt felhasználható |Internetes és belső (virtuális hálózati) alkalmazásokhoz egyaránt felhasználható |Csak az internetes alkalmazásokat támogatja |
| Végpontmonitoring |Mintavételen keresztül támogatott |Mintavételen keresztül támogatott |HTTP/HTTPS GET-en keresztül támogatott |

Az Azure Load Balancer és Alkalmazásátjáró útvonal hálózati forgalom tooendpoints, de rendelkezik a különböző használati forgatókönyvek toowhich forgalom toohandle. hello következő táblázat segítségével hello két terheléselosztók hello különbség ismertetése:

| Típus | Azure Load Balancer | Application Gateway |
| --- | --- | --- |
| Protokollok |UDP/TCP |HTTP, HTTPS és WebSockets |
| IP-címfenntartás |Támogatott |Nem támogatott |
| Terheléselosztási mód |5 rekordos (forrás IP-címe, forrásport, cél IP-címe, célport, protokolltípus) |Ciklikus időszeletelés<br>Útválasztás URL-cím alapján |
| Terheléselosztási mód (forrás IP-cím/fix kiszolgálású munkamenetek) |2 rekordos (forrás IP-cím és cél IP-cím), 3 rekordos (forrás IP-cím, cél IP-cím és port). Méretezhető felfelé vagy lefelé hello virtuális gépek száma alapján |Cookie-alapú affinitás<br>Útválasztás URL-cím alapján |
| Állapotminták |Alapértelmezett: 15 másodperces mintavételi időköz. A rotációból kivéve: 2 folyamatos hiba után. Támogatja a felhasználó által definiált mintavételeket |Üresjárati mintavételi időköz – 30 másodperc. 5 egymásutáni élő forgalmi hiba vagy egyetlen üresjárati módú mintavételi hiba után kivéve. Támogatja a felhasználó által definiált mintavételeket |
| SSL-kiürítés |Nem támogatott |Támogatott |
| URL-cím-alapú útválasztás | Nem támogatott | Támogatott|
| SSL-házirend | Nem támogatott | Támogatott|
