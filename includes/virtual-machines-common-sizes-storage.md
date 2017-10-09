
hello Ls-sorozat, amely a kis késleltetésű ideiglenes tárolási rendszerre van szüksége, például Cassandra, MongoDB, Cloudera, beleértve a NoSQL-adatbázisok és Redis van optimalizálva. hello Ls-sorozat kínál fel too32 Vcpu, hello segítségével [Intel® Xeon® processzor E5 v3 termékcsalád](http://www.intel.com/content/www/us/en/processors/xeon/xeon-e5-solutions.html). hello Ls-sorozat lekérdezi hello azonos CPU-teljesítmény, a G/GS sorozatnak hello és 8 GiB vCPU / memóriával rendelkezik.  

## <a name="ls-series"></a>Ls-sorozat

ACU: 180–240
 
| Méret          | vCPU | Memória: GiB | Ideiglenes tárterület (SSD) GiB | Adatlemezek max. száma | Maximális gyorsítótárazott és ideiglenes tárolóteljesítmény: IOPS/MBps (gyorsítótár mérete GiB-ban) | Max. gyorsítótárazás nélküli lemezteljesítmény: IOPS/MBps | Hálózati adapterek max. száma / várt hálózati teljesítmény (Mbps) | 
|---------------|-----------|-------------|--------------------------|----------------|-------------------------------------------------------------|-------------------------------------------|------------------------------| 
| Standard_L4s  | 4    | 32   | 678   | 8              | NA / NA (0)          | 5,000 / 125                               | 2 / 4000       | 
| Standard_L8s  | 8    | 64   | 1,388 | 16             | NA / NA (0)          | 10,000 / 250                              | 4 / 8000  | 
| Standard_L16s | 16   | 128  | 2,807 | 32             | NA / NA (0)          | 20,000 / 500                              | 8 / 6000 - 16000 &#8224; | 
| Standard_L32s* | 32 | 256  | 5,630 | 64             | NA / NA (0)          | 40,000 / 1,000                            | 8 / 20000 | 
 

lehet, hogy hello maximális átviteli sebesség Ls sorozatú virtuális gépek lehetséges attól függ, hogy hello száma, mérete és bármely csíkozást csatlakoztatott lemezekkel. Részletekért lásd a [Premium Storage: Nagy teljesítményű tárolási szolgáltatás Azure-alapú virtuális gépek számítási feladataihoz](../articles/storage/common/storage-premium-storage.md) című cikket. 

* Elkülönített toohardware dedikált tooa egyetlen felhasználói példány.

