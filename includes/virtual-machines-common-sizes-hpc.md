<!-- A-series - compute-intensive instances, H-series -->

hello A8-A11 és H-sorozat értékek is *számítási igényű példányok*. hello hardver, ezek mérete futtató terveztek és számítási igényű optimalizálva, és a hálózati igényű alkalmazásokat, beleértve a nagy teljesítményű számítástechnikai (HPC) alkalmazások, modellezési és szimulációja fürtre. A8-A11 adatsorozat hello Intel Xeon E5-2670 @ 2.6-os GHz-es pedig hello H-sorozat Intel Xeon E5-2667 v3 @ 3,2 GHz-es használja. 

Az Azure H sorozatú virtuális gépek hello következő generációs nagy teljesítményű számítástechnikai a virtuális gépek számítási igények felső határát, például molekuláris modellezési és számítási folyadékból dynamics célzó. Ezek 8 – 16 vCPU virtuális gépek hello Intel Haswell E5-2667 V3 processzor technológia DDR4 memória kiemeli épül, és SSD-alapú ideiglenes tárolók. 

Ezenkívül toohello jelentős processzorteljesítményre, hello H-sorozat különböző lehetőséget kínál kis késleltetésű RDMA hálózati FDR InfiniBand és több memória konfigurációk toosupport intenzív számítási memóriakövetelményei használatával.



## <a name="h-series"></a>H-sorozat

ACU: 290–300

| Méret | vCPU | Memória: GiB | Ideiglenes tárterület (SSD) GiB | Adatlemezek max. száma | Lemezek max. teljesítménye: IOPS | Hálózati adapterek maximális száma |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_H8 |8 |56 |1000 |16 |16x500 |2  |
| Standard_H16 |16 |112 |2000 |32 |32x500 |4 |
| Standard_H8m |8 |112 |1000 |16 |16x500 |2  |
| Standard_H16m |16 |224 |2000 |32 |32x500 |4  |
| Standard_H16r* |16 |112 |2000 |32 |32x500 |4  |
| Standard_H16mr* |16 |224 |2000 |32 |32x500 |4 |

*MPI-alkalmazások esetében a dedikált RDMA-háttérhálózatot az FDR InfiniBand hálózat biztosítja, amely rendkívül alacsony késést és magas sávszélességet kínál.

<br>



## <a name="a-series---compute-intensive-instances"></a>A-sorozat – nagy számítási igényű példányok

ACU: 225

| Méret | vCPU | Memória: GiB | Ideiglenes tárterület (HDD) GiB | Adatlemezek max. száma | Adatlemezek max. teljesítménye: IOPS | Hálózati adapterek maximális száma|
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A8* |8 |56 |382 |16 |16x500 |2 |
| Standard_A9* |16 |112 |382 |16 |16x500 |4 |
| Standard_A10 |8 |56 |382 |16 |16x500 |2  |
| Standard_A11 |16 |112 |382 |16 |16x500 |4 |

*MPI-alkalmazások esetében a dedikált RDMA-háttérhálózatot az FDR InfiniBand hálózat biztosítja, amely rendkívül alacsony késést és magas sávszélességet kínál.

<br>



