<!--author=SharS last changed: 9/17/15-->

### <a name="upgrade-sharepoint-2010-toosharepoint-2013-and-then-install-hello-storsomple-adapter-for-sharepoint"></a>Frissítse a SharePoint 2010 tooSharePoint 2013, és telepítse a SharePoint hello StorSomple Adapter
> [!IMPORTANT]
> Minden olyan fájlok, amelyek korábban került tooexternal tárolási RBS nem lesz elérhető, amíg hello frissítés nem fejeződött és hello RBS a szolgáltatás ismét engedélyezve. toolimit felhasználói befolyásolja, a frissítéshez vagy újratelepítés tervezett karbantartási időszak alatt történjen.
> 
> 

#### <a name="tooupgrade-sharepoint-2010-toosharepoint-2013-and-then-install-hello-adapter"></a>a SharePoint 2010 tooupgrade tooSharePoint 2013, majd a telepítés hello adapter
1. SharePoint 2010 hello farm Megjegyzés hello BLOB tárolására hello elérési útját externalized, Blobok és hello tartalom-adatbázisok, amelyhez RBS engedélyezve van. 
2. Telepítse és konfigurálja a hello új SharePoint 2013-farmhoz. 
3. Adatbázisok, alkalmazások és webhelycsoportok áthelyezése hello SharePoint 2010 farm toohello új SharePoint 2013-farmhoz. Útmutatásért lépjen túl[hello frissítési folyamat tooSharePoint 2013 áttekintése](https://technet.microsoft.com/library/cc262483.aspx).
4. A hello új farm SharePoint hello StorSimple-Adapter telepítése. Nyissa meg túl[telepítés hello StorSimple Adapter a SharePoint](#install-the-storsimple-adapter-for-sharepoint) eljárásait.
5. Az 1. lépésben feljegyzett hello információk alapján, ugyanaz a tartalom-adatbázisok készletét, és adja meg a azonos BLOB használt elérési út tárolása hello SharePoint 2010 telepítési hello hello RBS engedélyezése. Nyissa meg túl[konfigurálása RBS](#configure-rbs) eljárásait. Ez a lépés befejezése után korábban externalized fájlok hello új farm elérhetőnek kell lennie. 

### <a name="upgrade-hello-storsimple-adapter-for-sharepoint"></a>A SharePoint hello StorSimple Adapter frissítése
> [!IMPORTANT]
> A frissítési toooccur ütemezze úgy a tervezett karbantartási időszak alatt az a következő okok miatt hello:
> 
> * Korábban externalized tartalom nem lesz elérhető amíg hello adapter újratelepítése után.
> * Bármely alkalmazásba feltöltött tartalmat toohello hely után hello hello StorSimple Adapter korábbi verziójának eltávolítása a SharePoint, de hello új verzió telepítése előtt hello tartalom-adatbázist fogja tárolni. Szüksége lesz toomove tartalom toohello StorSimple eszköz hello új adapter telepítését követően. Használhatja a hello Microsoft` RBS Migrate()` SharePoint toomigrate hello tartalmának része PowerShell-parancsmagot. További információkért lásd: [tartalmat át a virtuális gépbe vagy onnan RBS](https://technet.microsoft.com/library/ff628255.aspx). 
> 
> 

#### <a name="tooupgrade-hello-storsimple-adapter-for-sharepoint"></a>a SharePoint StorSimple Adapter tooupgrade hello
1. StorSimple Adapter hello korábbi verziójának eltávolítása a SharePoint.
   
   > [!NOTE]
   > Ez automatikusan letiltja a RBS, a tartalom-adatbázisok hello. Azonban a meglévő Blobok hello StorSimple eszközön marad. Mivel RBS le van tiltva, és hello Blobok még nincsenek hátsó toohello áttelepített tartalom-adatbázisokra, ezeket a Blobok irányuló sikertelen lesz. 
   > 
   > 
2. Telepítés új StorSimple Adapter hello SharePoint. hello új adapter automatikusan észleli a hello tartalom-adatbázisok korábban engedélyezve vagy tiltva RBS és hello előző beállításokat fogja használni.

