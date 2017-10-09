| Erőforrás | Alapértelmezett korlát |
| --- | --- |
| Előfizetésenként storage-fiókok száma |200<sup>1</sup> |
| A maximális tárolókapacitás fiók |500 TB<sup>2</sup> |
| Blob tárolók, blobok, fájlmegosztások, táblák, üzenetsorok, entitásokat vagy tárfiók üzenetek maximális száma |Korlátlan |
| Egy egyetlen blob tároló, a tábla vagy a várólista maximális mérete |Ugyanaz, mint a maximális tárolókapacitás fiók |
| Legfeljebb blokkokat a blokkblob vagy hozzáfűző blob |50,000 |
| A blokkblob egy blokkja maximális mérete |100 MB |
| A blokkblob maximális mérete |50 000 x 100 MB (KB. 4.75 TB) |
| A blokk egy hozzáfűző BLOB maximális mérete |4 MB |
| A hozzáfűző blob maximális mérete |50 000 x 4 MB (KB. 195 GB) |
| Az oldalakra vonatkozó blob maximális mérete |8 TB |
| Egy tábla entitás maximális mérete |1 MB |
| Egy tábla entitásban levő tulajdonságok maximális száma |252 |
| A várólistában lévő üzenetek maximális mérete |64 KB |
| A fájlmegosztás maximális mérete |5 TB |
| A fájlmegosztás maximális fájlméret |1 TB |
| Egy megosztott fájlok maximális száma |Csak határértéke hello 5 TB-os kapacitásáig hello fájlmegosztás |
| Az egy maximális iops-érték |1000 |
| Egy megosztott fájlok maximális száma |Csak határértéke hello 5 TB-os kapacitásáig hello fájlmegosztás |
| Tárolt hozzáférési házirendek tároló, a fájlmegosztást, a tábla vagy a várólista maximális száma |5 |
| A tárfiók / maximális lekérdezési gyakorisága |Blobok: 20 000 kérelmek / másodperc<sup>2</sup> a blobok bármilyen érvényes méretű (tárfiókonként csak által hello fiók be-és kilépési korlátok) <br />Fájlok: 1000 iops-értéket (8 KB-nál) fájlmegosztáshoz <br />Várólisták: 20 000 üzenetek / másodperc (feltételezve 1 KB méretű üzeneteket)<br />Táblázatok: 20 000 tranzakciók száma másodpercenként (feltételezve 1 KB entitás mérete) |
| Egy BLOB tároló átviteli |Másolatot too60 MB másodpercenként vagy mentése too500 kérelmek másodpercenként |
| Cél átviteli egyetlen sor (1 KB üzenetek) |Too2000 üzenetek / másodperc |
| Cél átviteli egyetlen tábla partíció (1 KB entitások) |Másolatot too2000 entitások száma másodpercenként |
| Cél átviteli egy fájlmegosztás |Too60 MB / s |
| Maximális érkező<sup>3</sup> tárolási fiókonként (US régió) |Ha a Georedundáns/ZRS 10 GB/s<sup>4</sup> engedélyezve van, az LRS 20 GB/s<sup>2</sup> |
| Maximális kilépő<sup>3</sup> tárolási fiókonként (US régió) |Ha a ZRS RA-GRS vagy GRS 20 GB/s<sup>4</sup> engedélyezve van, az LRS 30 GB/s<sup>2</sup> |
| Maximális érkező<sup>3</sup> tárolási fiókonként (Amerikai Egyesült régiókban) |Ha a Georedundáns/ZRS 5 GB/s<sup>4</sup> engedélyezve van, az LRS 10 GB/s<sup>2</sup> |
| Maximális kilépő<sup>3</sup> tárolási fiókonként (Amerikai Egyesült régiókban) |Ha a ZRS RA-GRS vagy GRS 10 GB/s<sup>4</sup> engedélyezve van, az LRS 15 GB/s<sup>2</sup> |

<sup>1</sup>Ez magában foglalja a Standard és prémium szintű storage-fiókok. Ha több mint 200 tárfiókra van szüksége, nyújtson be egy kérést az [Azure ügyfélszolgálatán](https://azure.microsoft.com/support/faq/) keresztül. hello Azure Storage csapat lesz az üzleti esetek és jóváhagyhatja too250 storage-fiókok létrehozása. 

<sup>2</sup> tooget a standard szintű tárfiókot múltbeli toogrow hello hirdetett korlátok, a kapacitás, a be-és kilépési és a kérelmek aránya, végezze el a kérelmet [Azure támogatási](https://azure.microsoft.com/support/faq/). hello Azure Storage csapat hello kérelmet a rendszer tekintse át, és előfordulhat, hogy hagyja jóvá a magasabb korlátok eseti alapon.

<sup>3</sup>*érkező* tooall (kérelmek) küldött adatok mennyisége tooa tárfiók hivatkozik. *Kimenő forgalom* tárfiókból kapott tooall adatokat (válasz) hivatkozik.  

<sup>4</sup>azure Storage replikációs lehetőségek a következők:
* **RA-GRS**: írásvédett georedundáns tárolás. RA-GRS engedélyezve van, ha kilépő hello másodlagos hely célpontjai azonos toothose hello elsődleges helyen.
* **Georedundáns**: georedundáns tárolást. 
* **A ZRS**: zónaredundáns tárolás. Csak a blokkblobokhoz érhető el. 
* **LRS**: helyileg redundáns tárolás. 


