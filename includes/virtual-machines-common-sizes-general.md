
<!-- A-series, Av2-series, D-series, Dv2-series, DS-series*, DSv2-series* -->

- A-sorozatú hello és Av2 sorozatú virtuális gépek is telepíthető a különböző hardvertípusok, és a processzorok. hello mérete folyamatban van, hello hardver, egységes processzorteljesítmény toooffer, függetlenül a telepítették hello hardver-példányt futtató hello alapján. toodetermine hello fizikai hardver ekkora telepítve lekérdezés hello virtuális hardver a virtuális gép hello belül.

- D sorozatú virtuális gépek rendszer nagyobb számítási teljesítményt és a ideiglenes lemezek igény tervezett toorun alkalmazás. D sorozatú virtuális gépek hello mennyiségű ideiglenes lemezes gyorsabb processzorok, memória-vCPU magasabb arány és egy SSD-meghajtót (SSD) adja meg. További információkért lásd: hello közlemény a hello Azure blog [D sorozatú új virtuálisgép-méretek](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/).

- Dv2-sorozat, utólagos toohello eredeti D sorozat nagyobb teljesítményű CPU szolgáltatásokat. hello Dv2-sorozat CPU pedig körülbelül 35 % gyorsabb, mint az hello D sorozatú Processzor. Alapul hello legújabb generációját 2,4 GHz-es Intel Xeon® E5-2673 v3 (Haswell) processzor, és a hello Intel Turbo program technológia 2.0, lépjen be too3.1 GHz-es. hello Dv2-sorozat rendelkezik hello memóriát és lemezterületet ugyanazokat a konfigurációkat, a D sorozat hello.

- hello alapvető a rétegek méretének összege elsősorban a fejlesztés munkaterhelések és más alkalmazásokat, amelyek nem igényelnek terheléselosztás, automatikus skálázás vagy memóriaigényes virtuális gépek. Az éles környezetekhez jobban megfelelő virtuálisgép-méretekkel kapcsolatos információért tekintse meg a (Virtuális gépek méretei)[virtual-machines-size-specs.md], a virtuális gépek díjszabásával kapcsolatban pedig [A virtuális gépek díjszabása](https://azure.microsoft.com/pricing/details/virtual-machines/)című szakaszt.

## <a name="dsv3-series"></a>Dsv3-sorozat

ACU: 160–190

Hello alapuló Dsv3-sorozat méretek 2.3 GHz-es Intel XEON® E5-2673 v4 (Broadwell) processzor és is Intel Turbo program technológia 2.0-s és 3.5-ös GHz elérése és prémium szintű Storage. hello Dsv3-sorozat méretek vCPU, a memória és a legtöbb termelési számítási feladatokhoz ideiglenes tárolási kínálnak.


| Méret             | vCPU | Memória: GiB | Ideiglenes tárterület (SSD) GiB | Adatlemezek max. száma | Maximális gyorsítótárazott és ideiglenes tárolóteljesítmény: IOPS/MBps (gyorsítótár mérete GiB-ban) | Max. gyorsítótárazás nélküli lemezteljesítmény: IOPS/MBps | Hálózati adapterek max. száma / várt hálózati teljesítmény (Mbps) |
|------------------|--------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------------------------|
| Standard_D2s_v3  | 2      | 8           | 16             | 4              | 4,000 / 32 (50)                                                       | 3,200 / 48                                | 2/közepes                                   |
| Standard_D4s_v3  | 4      | 16          | 32             | 8              | 8,000 / 64 (100)                                                      | 6,400 / 96                                | 2/közepes                                   |
| Standard_D8s_v3  | 8      | 32          | 64             | 16             | 16,000 / 128 (200)                                                    | 12,800 / 192                              | 4/magas                                       |
| Standard_D16s_v3 | 16     | 64          | 128            | 32             | 32,000 / 256 (400)                                                    | 25,600 / 384                              | 8/magas                                       |


## <a name="dv3-series"></a>Dv3-sorozat

ACU: 160–190

Hello alapuló Dv3-sorozat méretek 2.3 GHz-es Intel XEON® E5-2673 v4 (Broadwell) processzor és Intel Turbo program technológia 2.0-s és 3.5-ös GHz érhető el. hello Dv3-sorozat méretek vCPU, a memória és a legtöbb termelési számítási feladatokhoz ideiglenes tárolási kínálnak.

Az adatlemezes tárolást a virtuális gépektől függetlenül számlázzuk. toouse prémium tárolólemezek hello Dsv3 méretű használata. hello tarifa- és számlázási mérőszámok Dsv3 méretű hello ugyanaz, mint a Dv3-sorozat történik. 


| Méret            | vCPU | Memória: GiB | Ideiglenes tárterület (SSD) GiB | Adatlemezek max. száma | Ideiglenes tárterület maximális teljesítménye: IOPS / Olvasási MBps / Írási MBps | Hálózati adapterek max. száma/hálózati sávszélesség |
|-----------------|-----------|-------------|----------------|----------------|----------------------------------------------------------|------------------------------|
| Standard_D2_v3  | 2         | 8           | 50             | 4              | 3000/46/23                                               | 2/közepes                 |
| Standard_D4_v3  | 4         | 16          | 100            | 8              | 6000/93/46                                               | 2/közepes                 |
| Standard_D8_v3  | 8         | 32          | 200            | 16             | 12000/187/93                                             | 4/magas                     |
| Standard_D16_v3 | 16        | 64          | 400            | 32             | 24000/375/187                                            | 8/magas                     |


## <a name="dsv2-series"></a>DSv2-sorozat

ACU: 210–250

| Méret | vCPU | Memória: GiB | Ideiglenes tárterület (SSD) GiB | Adatlemezek max. száma | Maximális gyorsítótárazott és ideiglenes tárolóteljesítmény: IOPS/MBps (gyorsítótár mérete GiB-ban) | Max. gyorsítótárazás nélküli lemezteljesítmény: IOPS/MBps | Hálózati adapterek max. száma / várt hálózati teljesítmény (Mbps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS1_v2 |1 |3.5 |7 |2 |4,000 / 32 (43) |3,200 / 48 |2 / 750 |
| Standard_DS2_v2 |2 |7 |14 |4 |8,000 / 64 (86) |6,400 / 96 |2 / 1500 |
| Standard_DS3_v2 |4 |14 |28 |8 |16,000 / 128 (172) |12,800 / 192 |4 / 3000 |
| Standard_DS4_v2 |8 |28 |56 |16 |32,000 / 256 (344) |25,600 / 384 |8 / 6000 |
| Standard_DS5_v2 |16 |56 |112 |32 |64,000 / 512 (688) |51,200 / 768 |8 / 6000 - 12000 &#8224;|



## <a name="dv2-series"></a>Dv2-sorozat

ACU: 210–250

| Méret              | vCPU | Memória: GiB | Ideiglenes tárterület (SSD) GiB | Ideiglenes tárterület maximális teljesítménye: IOPS / Olvasási MBps / Írási MBps | Adatlemezek max. száma / teljesítménye: IOPS | Hálózati adapterek max. száma / várt hálózati teljesítmény (Mbps) |
|-------------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D1_v2    | 1         | 3.5         | 50             | 3000 / 46 / 23                                           | 2 / 2x500                         | 2 / 750                 |
| Standard_D2_v2    | 2         | 7           | 100            | 6000 / 93 / 46                                           | 4 / 4x500                         | 2 / 1500                     |
| Standard_D3_v2    | 4         | 14          | 200            | 12000 / 187 / 93                                         | 8 / 8x500                         | 4 / 3000                     |
| Standard_D4_v2    | 8         | 28          | 400            | 24000 / 375 / 187                                        | 16 / 16x500                       | 8 / 6000                     |
| Standard_D5_v2    | 16        | 56          | 800            | 48000 / 750 / 375                                        | 32 / 32x500                       | 8 / 6000 - 12000 &#8224;          |


<br>

## <a name="ds-series"></a>DS-sorozat

ACU: 160

| Méret | vCPU | Memória: GiB | Ideiglenes tárterület (SSD) GiB | Adatlemezek max. száma | Maximális gyorsítótárazott és ideiglenes tárolóteljesítmény: IOPS/MBps (gyorsítótár mérete GiB-ban) | Max. gyorsítótárazás nélküli lemezteljesítmény: IOPS/MBps | Hálózati adapterek max. száma / várt hálózati teljesítmény (Mbps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS1 |1 |3.5 |7 |2 |4,000 / 32 (43) |3,200 / 32 |2 / 500 |
| Standard_DS2 |2 |7 |14 |4 |8,000 / 64 (86) |6,400 / 64 |2 / 1000 |
| Standard_DS3 |4 |14 |28 |8 |16,000 / 128 (172) |12,800 / 128 |4 / 2000 |
| Standard_DS4 |8 |28 |56 |16 |32,000 / 256 (344) |25,600 / 256 |8 / 4000 |

<br>

## <a name="d-series"></a>D-sorozat 

ACU: 160

| Méret         | vCPU | Memória: GiB | Ideiglenes tárterület (SSD) GiB | Ideiglenes tárterület maximális teljesítménye: IOPS / Olvasási MBps / Írási MBps | Adatlemezek max. száma / teljesítménye: IOPS | Hálózati adapterek max. száma / várt hálózati teljesítmény (Mbps) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D1  | 1         | 3.5         | 50             | 3000 / 46 / 23                                           | 2 / 2x500                         | 2 / 500                 |
| Standard_D2  | 2         | 7           | 100            | 6000 / 93 / 46                                           | 4 / 4x500                         | 2 / 1000                     |
| Standard_D3  | 4         | 14          | 200            | 12000 / 187 / 93                                         | 8 / 8x500                         | 4 / 2000                     |
| Standard_D4  | 8         | 28          | 400            | 24000 / 375 / 187                                        | 16 / 16x500                       | 8 / 4000                     |

<br>


## <a name="av2-series"></a>Av2-sorozat

ACU: 100

| Méret            | vCPU | Memória: GiB | Ideiglenes tárterület (SSD) GiB | Ideiglenes tárterület maximális teljesítménye: IOPS / Olvasási MBps / Írási MBps | Adatlemezek max. száma / teljesítménye: IOPS | Hálózati adapterek max. száma / várt hálózati teljesítmény (Mbps) | 
|-----------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_A1_v2  | 1         | 2           | 10             | 1000 / 20 / 10                                           | 2 / 2x500               | 2 / 250                 |
| Standard_A2_v2  | 2         | 4           | 20             | 2000 / 40 / 20                                           | 4 / 4x500               | 2 / 500                 |
| Standard_A4_v2  | 4         | 8           | 40             | 4000 / 80 / 40                                           | 8 / 8x500               | 4 / 1000                     |
| Standard_A8_v2  | 8         | 16          | 80             | 8000 / 160 / 80                                          | 16 / 16x500             | 8 / 2000                     |
| Standard_A2m_v2 | 2         | 16          | 20             | 2000 / 40 / 20                                           | 4 / 4x500               | 2 / 500                 |
| Standard_A4m_v2 | 4         | 32          | 40             | 4000 / 80 / 40                                           | 8 / 8x500               | 4 / 1000                     |
| Standard_A8m_v2 | 8         | 64          | 80             | 8000 / 160 / 80                                          | 16 / 16x500             | 8 / 2000                     |

<br>

## <a name="a-series"></a>A-sorozat

ACU: 50–100

| Méret | vCPU | Memória: GiB | Ideiglenes tárterület (HDD) GiB | Adatlemezek max. száma | Adatlemezek max. teljesítménye: IOPS | Hálózati adapterek max. száma / várt hálózati teljesítmény (Mbps)  |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A0* |1 |0.768 |20 |1 |1x500 |2 / 100 |
| Standard_A1 |1 |1.75 |70 |2 |2x500 |2 / 500  |
| Standard_A2 |2 |3.5 |135 |4 |4x500 |2 / 500 |
| Standard_A3 |4 |7 |285 |8 |8x500 |2 / 1000 |
| Standard_A4 |8 |14 |605 |16 |16x500 |4 / 2000 |
| Standard_A5 |2 |14 |135 |4 |4x500 |2 / 500 |
| Standard_A6 |4 |28 |285 |8 |8x500 |2 / 1000 |
| Standard_A7 |8 |56 |605 |16 |16x500 |4 / 2000 |
<br>

* hello A0 méretű túlzott előfizetett hello fizikai hardveren. Csak a meghatározott méretet, a többi felhasználói telepítés hatással lehet a futó számítási feladatok teljesítményére hello. hello relatív teljesítménye tulajdonos tooan hozzávetőleges variancia 15 százalékos várt hello alapjául az alábbiakban leírt.

### <a name="standard-a0---a4-using-cli-and-powershell"></a>Standard A0–A4, CLI és PowerShell használatával
Hello klasszikus üzembe helyezési modellel néhány virtuális gép mérete nevet némileg eltérnek a parancssori felület és a PowerShell:

* A Standard_A0 neve ExtraSmall 
* A Standard_A1 neve Small
* A Standard_A2 neve Medium
* A Standard_A3 neve Large
* A Standard_A4 neve ExtraLarge

## <a name="basic-a"></a>Alapszintű A

|Méret – Méret\név | vCPU |Memory (Memória)|Hálózati adapterek (max)|Ideiglenes lemez max. mérete |Legfeljebb adatlemez, egyenként 1023 GB)|Legfeljebb IOPS (300 lemezenként)|
|---|---|---|---|---|---|---|
|A0\Basic_A0|1|768 MB|2| 20 GB|1|1x300|
|A1\Basic_A1|1|1,75 GB|2| 40 GB |2|2x300|
|A2\Basic_A2|2|3,5 GB|2| 60 GB|4|4x300|
|A3\Basic_A3|4|7 GB|2| 120 GB |8|8x300|
|A4\Basic_A4|8|14 GB|2| 240 GB |16|16x300|
