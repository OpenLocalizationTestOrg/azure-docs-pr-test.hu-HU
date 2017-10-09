Előfizetés, minden egyes üzembe helyezve egy konkrét csomagot kiválasztani, csak egyes rétegek engedélyezett szolgáltatások hello számát korlátozza egy belül több szolgáltatáshoz is létrehozhat. Például létrehozhat too12 szolgáltatások hello alapszintű rétegben, és egy másik 12 szolgáltatásokat, amely hello S1 réteg hello belül azonos előfizetéssel. Rétegek kapcsolatos további információkért lásd: [válasszon egy SKU vagy a réteg az Azure Search](../articles/search/search-sku-tier.md).

Maximális szolgáltatásra vonatkozó korlátozások is generál, kérésre. Kapcsolattartási Azure Ha szüksége több ügyfélszolgálatainak hello belül azonos előfizetéssel.

| Erőforrás | Ingyenes | Alapszintű | S1 | S2 | S3 | S3 HD <sup>1</sup> |
| --- | --- | --- | --- | --- | --- | --- |
| Maximális szolgáltatások |1 |12 |12 |6 |6 |6 |
| SU a maximális skálája <sup>2</sup> |N/A <sup>3</sup> |3 SU <sup>4</sup> |36 SU |36 SU |36 SU |36 SU |

<sup>1</sup> S3 HD nem támogatja a [indexelők](../articles/search/search-indexer-overview.md) most. 

<sup>2</sup> keresési egységek (SU) vannak számlázást egységeket, mert lefoglalt egy *replika* vagy egy *partíció*. A tárolás, az indexelést és a lekérdezési műveletek mindkét erőforrás van szüksége. További információk hogyan keresési egység arra az esetre vonatkoznak, valamint a diagram érvényes kombinációk hello határok alatt maradjunk, lásd: toolearn [lekérdezési és indexelési munkaterhelések szintjeinek erőforrás méretezése](../articles/search/search-capacity-planning.md). 

<sup>3</sup> szabad több előfizetők által használt megosztott erőforrások alapul. Ebben a rétegben, nincsenek az egyes előfizető dedikált erőforrások. Emiatt a maximális skálája jelölésű nem alkalmazható.

<sup>4</sup> basic egy rögzített partíciót tartalmaz. A réteg a további SUs vannak használja több replikák lefoglalásával megnövekedett lekérdezés munkaterhelések.

