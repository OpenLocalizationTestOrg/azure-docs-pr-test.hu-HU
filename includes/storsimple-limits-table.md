<!--author=alkohli last changed: 12/15/15-->

| Korlátazonosító | Korlát | Megjegyzések |
| --- | --- | --- |
| Tárfiók hitelesítő adatainak maximális száma |64 | |
| Kötettárolók maximális száma |64 | |
| Kötetek maximális száma |255 | |
| Ütemezésének per sávszélességsablon maximális száma |168 |Óránként, naponta (24 * 7) hello hét ütemezése. |
| A rétegzett kötetek fizikai eszközök maximális mérete |A 8100 és 8600 64 TB |8100 és 8600 különböző fizikai eszközök. |
| A virtuális eszközök, az Azure-ban a rétegzett kötetek maximális mérete |30 TB lehet a 8010-es <br></br> A 8020-as modell 64 TB |8010-es és a 8020-as modell olyan virtuális Azure használó eszközöket Standard és Premium tárolására kulcsattribútumokkal. |
| Fizikai eszközön helyileg rögzített kötet maximális mérete |A 8100 9 TB <br></br> A 8600 24 TB |8100 és 8600 különböző fizikai eszközök. |
| ISCSI-kapcsolatok maximális száma |512 | |
| A kezdeményezők iSCSI-kapcsolatok maximális száma |512 | |
| Hozzáférés-vezérlési rekordokat eszközönként maximális száma |64 | |
| A biztonsági mentési házirend / kötetek maximális száma |24 | |
| A biztonsági mentési házirend tárolása biztonsági másolatok maximális száma |64 | |
| Egy biztonsági mentési házirend ütemezések maximális száma |10 | |
| Bármilyen típusú, amelynek meg kell őrizni kötetenként pillanatképek maximális száma |256 |Ez magában foglalja a helyi pillanatképeket és felhőbeli pillanatképeket. |
| Lehet, hogy bármely eszköz megtalálható a pillanatképek maximális száma |10,000 | |
| A biztonsági másolat, visszaállítási párhuzamosan is, vagy a klónozáshoz kötetek maximális számát |16 |<ul><li>Ha több mint 16 köteteket, azok dolgoz fel egymás után amint elérhetővé válnak a feldolgozási tárolóhelye.</li><li>Új biztonsági másolatot egy klónozott vagy visszaállított rétegzett kötet nem kerülhet sor, amíg hello művelet be nem fejeződik. Egy helyi kötet, azonban biztonsági mentések engedélyezettek után hello kötet online állapotban.</li></ul> |
| Visszaállítás és helyreállítás ideje rétegzett kötetek klónozása |< 2 perc |<ul><li>a restore vagy a Klónozás működését, függetlenül hello kötet mérete 2 percen belül hello kötet legyen elérhető.</li><li>hello kötet kezdetben lehet a teljesítmény lassabb, mint a szokásos módon legtöbb hello adatok és metaadatok továbbra is hello felhőben található. Teljesítmény megnőhet, adatfolyamok hello felhő toohello StorSimple eszközön.</li><li>hello teljes ideje toodownload metaadatok lefoglalt kötet mérete hello függ. Metaadatok automatikusan bevinni hello eszköz hello háttérben hello díj TB-nyi lefoglalt kötet adatait 5 perc. Ez érinthet internetes sávszélesség toohello felhő.</li><li>visszaállítás hello, vagy a Klónozás befejeződött, ha minden hello metaadatok hello eszközön.</li><li>Biztonsági mentési művelet nem hajtható végre, amíg hello visszaállítási, vagy a Klónozás teljesen kész. |
| Állítsa vissza a helyileg rögzített kötetek helyreállítása ideje |< 2 perc |<ul><li>hello kötet hello visszaállítási művelet, függetlenül attól, hello kötet mérete, 2 percen belül legyen elérhető.</li><li>hello kötet kezdetben lehet a teljesítmény lassabb, mint a szokásos módon legtöbb hello adatok és metaadatok továbbra is hello felhőben található. Teljesítmény megnőhet, adatfolyamok hello felhő toohello StorSimple eszközön.</li><li>hello teljes ideje toodownload metaadatok lefoglalt kötet mérete hello függ. Metaadatok automatikusan bevinni hello eszköz hello háttérben hello díj TB-nyi lefoglalt kötet adatait 5 perc. Ez érinthet internetes sávszélesség toohello felhő.</li><li>Ellentétben a rétegzett kötetek, a helyileg rögzített kötetek hello esetben hello kötet adatait is tölt le helyileg hello eszközön. hello visszaállítási művelet befejeződött, amikor minden hello kötetadatokról állapotba toohello eszköz.</li><li>Előfordulhat, hogy hosszú hello visszaállítási műveletek és hello teljes toocomplete hello visszaállítás egy korábbi időpontra kiépített hello helyi kötet, a internetes sávszélességet és a meglévő adatok hello eszközön hello hello méretétől függ. Hello helyileg rögzített kötetet a biztonsági mentési műveletek engedélyezettek, amíg hello visszaállítási művelet van folyamatban. |
| Vékony visszaállítási rendelkezésre állása |Legutóbbi feladatátvétele | |
| Maximális ügyfél olvasási/írási teljesítmény (amikor a hello SSD-réteghez és kiszolgálása között) * |920/720 MB/s a egy egyetlen 10 gbe hálózati adapter |Másolatot too2x az MPIO és két hálózati adapterrel. |
| Maximális ügyfél olvasási/írási teljesítmény (ha szolgáltatott hello HDD-réteget a) * |120/250 MB/s | |
| Maximális ügyfél olvasási/írási teljesítmény (ha szolgáltatott hello felhő rétegtől) * |11/41 MB/s |Olvasás átviteli ügyfelek létrehozása és fenntartása elegendő i/o-várólistamélység függ. |

&#42; I/o-típusonkénti maximális átviteli sebesség és 100 százalék olvasási 100 százalék írási forgatókönyvek mérték. Tényleges átviteli alacsonyabb lehet, és attól függ, i/o kombinálhatók és a hálózati feltételek.

