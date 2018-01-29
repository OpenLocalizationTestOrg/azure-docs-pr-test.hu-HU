- A virtuális hálózatnak a Batch-fiókkal megegyező Azure**-régióban** és **-előfizetésben** kell lennie.

- A virtuális hálózati konfigurációval létrehozott készletek esetében csak az Azure Resource Manager-alapú virtuális hálózatok támogatottak. A felhőszolgáltatás-konfigurációval létrehozott készletek esetében csak a klasszikus virtuális hálózatok támogatottak. 
  
- Egy klasszikus virtuális hálózat használatához a `MicrosoftAzureBatch` szolgáltatásnévnek rendelkeznie kell a `Classic Virtual Machine Contributor` szerepköralapú hozzáférés-vezérlési (RBAC) szerepkörrel az adott virtuális hálózaton. Azure Resource Manager-alapú virtuális hálózat használatához nincs szükség további engedély beállítására.

- A készlethez meghatározott alhálózatnak elegendő hozzá nem rendelt IP-címmel kell rendelkeznie ahhoz, hogy helyet tudjon adni a készlethez kijelölt számú virtuális gépnek. Ez a szám a készlet `targetDedicatedNodes` és `targetLowPriorityNodes` tulajdonságának összege. Ha az alhálózaton nincs elegendő hozzá nem rendelt IP-cím, akkor a készlet részlegesen lefoglalja a számítási csomópontokat, és átméretezési hiba következik be. 

- A virtuális hálózatnak engedélyeznie kell a Batch szolgáltatástól kiinduló kommunikációt, hogy képes legyen feladatok ütemezésére a számítási csomópontokon. Ehhez ellenőrizheti, hogy a virtuális hálózattal vannak-e társítva hálózati biztonsági csoportok (NSG-k). Ha a megadott alhálózaton a számítási csomópontok felé irányuló kommunikációt egy NSG letiltja, akkor a Batch szolgáltatás **nem használhatóra** állítja a számítási csomópontok állapotát. 

- Ha a megadott virtuális hálózathoz hálózati biztonsági csoportok (NSG-k) és/vagy tűzfal van társítva, a következő táblázatokban látható módon konfigurálja a bejövő és kimenő portokat:


  |    Célport(ok)    |    Forrás IP-címe      |   Forrásport    |    Hozzáad a Batch NSG-ket?    |    Szükséges a virtuális gép használatához?    |    Felhasználói művelet   |
  |---------------------------|---------------------------|----------------------------|----------------------------|-------------------------------------|-----------------------|
  |   <ul><li>Virtuálisgép-konfigurációval létrehozott készletek esetén: 29876, 29877</li><li>Felhőszolgáltatás-konfigurációval létrehozott készletek esetén: 10100, 20100, 30100</li></ul>        |    Csak a Batch szolgáltatási szerepkör IP-címei | * vagy 443 |    Igen. A Batch az NSG-ket a virtuális gépekhez kapcsolt hálózati adapterek (NIC) szintjén adja hozzá. Ezek az NSG-k csak a Batch szolgáltatási szerepkör IP-címeiről érkező forgalmat engedélyezik. Még ha meg is nyitja ezeket a portokat a teljes web felé, a forgalom blokkolva lesz a hálózati adapteren. |    Igen  |  Nem kell megadnia NSG-t, mert a Batch szolgáltatás csak a Batch IP-címeit engedélyezi. <br /><br /> Ha azonban NSG-t ad meg, győződjön meg arról, hogy ezek a portok nyitva vannak a bejövő forgalom számára. <br /><br /> A Batch akkor is hozzáadja az NSG-ket a virtuális gépekhez csatlakoztatott hálózati adapter szintjén, ha az NSG-ben a forrás IP-címeként * van megadva. |
  |    3389 (Windows), 22 (Linux)               |    Felhasználói, hibakeresési célra használt gépek a virtuális gép távolról való eléréséhez.    |   *  | Nem                                    |    Nem                    |    Adjon hozzá NSG-ket, ha engedélyezni szeretné a távoli elérést (RDP vagy SSH) a virtuális géphez.   |                                


  |    Kimenő port(ok)    |    Cél    |    Hozzáad a Batch NSG-ket?    |    Szükséges a virtuális gép használatához?    |    Felhasználói művelet    |
  |------------------------|-------------------|----------------------------|-------------------------------------|------------------------|
  |    443    |    Azure Storage    |    Nem    |    Igen    |    Ha hozzáadott NSG-t, győződjön meg arról, hogy nyitva van a port a kimenő forgalom számára.    |

   Arról is győződjön meg, hogy az Azure Storage-végpont feloldható bármely, a VNetet kiszolgáló egyéni DNS-kiszolgáló által. Az `<account>.table.core.windows.net`, `<account>.queue.core.windows.net` és `<account>.blob.core.windows.net` űrlap URL-címeinek feloldhatónak kell lenniük. 

   Ha hozzáad egy Resource Manager-alapú NSG-t, hogy használatát [címkék szolgáltatás](../articles/virtual-network/security-overview.md#service-tags) jelölje be a tároló IP-címeket az adott régió kimenő kapcsolatok. Vegye figyelembe, hogy a tároló IP-címek és a Batch-fiók és a virtuális hálózat ugyanabban a régióban kell lennie. Szolgáltatás címkék jelenleg előzetes verzióban érhetők a kijelölt Azure-régióban.