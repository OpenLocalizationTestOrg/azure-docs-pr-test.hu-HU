<!-- F-series, Fs-series* -->

F-sorozat alapján hello 2,4 GHz-es Intel Xeon® E5-2673 v3 (Haswell) processzort, amely magas, mint 3.1-es GHz-es hello Intel Turbo program technológia 2.0 az óra sebesség érhető el. Ez az azonos hello CPU-teljesítmény, a virtuális gépek Dv2 sorozatát hello.  Alacsonyabb áron óránként lista F-sorozat hello értéke hello legjobb ár-teljesítmény hello Azure portfóliót hello Azure számítási egység (ACU) vCPU száma alapján. 

Az F-sorozat virtuális gépei remek választásnak bizonyulnak az olyan számítási feladatokhoz, amelyekhez gyorsabb processzor szükséges, azonban kisebb a vCPU-nkénti memória- vagy ideiglenes tárterületigényük.  A munkaterhelések elemzés, játék-kiszolgálók, webkiszolgálókat és kötegfeldolgozási például ki a hello F-sorozat értékének hello előnyeit.

hello Fs-sorozat hello F-sorozat, továbbá tooPremium Storage előnyei összes hello biztosít.

## <a name="fs-series"></a>Fs-sorozat*

ACU: 210–250

| Méret | vCPU | Memória: GiB | Ideiglenes tárterület (SSD) GiB | Adatlemezek max. száma | Maximális gyorsítótárazott és ideiglenes tárolóteljesítmény: IOPS/MBps (gyorsítótár mérete GiB-ban) | Max. gyorsítótárazás nélküli lemezteljesítmény: IOPS/MBps | Hálózati adapterek max. száma / várt hálózati teljesítmény (Mbps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_F1s |1 |2 |4 |2 |4,000 / 32 (12) |3,200 / 48 |2 / 750 |
| Standard_F2s |2 |4 |8 |4 |8,000 / 64 (24) |6,400 / 96 |2 / 1500 |
| Standard_F4s |4 |8 |16 |8 |16,000 / 128 (48) |12,800 / 192 |4 / 3000 |
| Standard_F8s |8 |16 |32 |16 |32,000 / 256 (96) |25,600 / 384 |8 / 6000 |
| Standard_F16s |16 |32 |64 |32 |64,000 / 512 (192) |51,200 / 768 |8 / 6000-12000 &#8224; |

MBps = 10^6 bájt/másodperc és GiB = 1024^3 bájt.

* hello maximális lemez átviteli (IOPS vagy MB/s) Fs több virtuális gép lehetséges korlátozhatja hello száma, mérete és a hello csíkozást csatolt lemez(ek).  Részletekért lásd a [Premium Storage: Nagy teljesítményű tárolási szolgáltatás Azure-alapú virtuális gépek számítási feladataihoz](../articles/storage/common/storage-premium-storage.md) című cikket.


<br>

## <a name="f-series"></a>F-sorozat

ACU: 210–250

| Méret         | vCPU | Memória: GiB | Ideiglenes tárterület (SSD) GiB | Ideiglenes tárterület maximális teljesítménye: IOPS / Olvasási MBps / Írási MBps | Adatlemezek max. száma / teljesítménye: IOPS | Hálózati adapterek max. száma / várt hálózati teljesítmény (Mbps) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_F1  | 1         | 2           | 16             | 3000 / 46 / 23                                           | 2 / 2x500                         | 2 / 750                 |
| Standard_F2  | 2         | 4           | 32             | 6000 / 93 / 46                                           | 4 / 4x500                         | 2 / 1500                     |
| Standard_F4  | 4         | 8           | 64             | 12000 / 187 / 93                                         | 8 / 8x500                         | 4 / 3000                     |
| Standard_F8  | 8         | 16          | 128            | 24000 / 375 / 187                                        | 16 / 16x500                       | 8 / 6000                     |
| Standard_F16 | 16        | 32          | 256            | 48000 / 750 / 375                                        | 32 / 32x500                       | 8 / 6000 - 12000 &#8224;           |


<br>


