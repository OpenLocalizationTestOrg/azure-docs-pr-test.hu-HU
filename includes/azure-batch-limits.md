| **Erőforrás** | **Alapértelmezett korlát** | **Felső korlát** |
| --- | --- | --- |
| Az előfizető számára régiónként rendelkezésre álló Batch-fiókok | 3 |50 |
| Magok száma (a Batch szolgáltatás mód) a Batch-fiók dedikált<sup>1</sup> | 20 | N/A<sup>2</sup> |
| Alacsony prioritású magok száma (a Batch szolgáltatás mód) a Batch-fiókhoz<sup>3</sup> | 50 | N/A<sup>4</sup> |
| Aktív feladatok és a feladatok ütemezésének<sup>5</sup> / Batch-fiókhoz. | 20 | 5000<sup>6</sup> |
| Készletek száma Batch-fiókonként | 20 | 2500 |

<sup>1</sup> tárolókészlet foglalási módban a túl csak a fiókok vannak látható dedikált core kvóták**a Batch szolgáltatás**. A fiókok hello módban a túl**felhasználói előfizetési**, core kvóták hello VM magok kvóta regionális szinten, vagy a virtuális gép termékcsalád az előfizetésében alapulnak.

<sup>2</sup> Batch-fiók dedikált magok számát hello növelhető, de nincs megadva hello maximális számát. Kérje az Azure támogatási toodiscuss beállítások növeléséhez.

<sup>3</sup> tárolókészlet foglalási módban a túl alacsony prioritású core kvóták látható vannak, csak a fiókok**a Batch szolgáltatás**. Alacsony prioritású magok válnak elérhetővé a fiókok tárolókészlet foglalási módban a túl**felhasználói előfizetési**.

<sup>4</sup> Batch-fiók alacsony prioritású magok számát hello növelhető, de nincs megadva hello maximális számát. Kérje az Azure támogatási toodiscuss beállítások növeléséhez.

<sup>5</sup> befejezett feladatok és a feladatok ütemezésének nem korlátozottak.

<sup>6</sup> forduljon Azure támogatási szolgálatához, ha azt szeretné, hogy toorequest meghaladja ezt a határt növelését.
