<!--author=SharS last changed: 02/04/2016-->

#### <a name="toocreate-a-volume"></a>a kötet toocreate
1. Hello eszközön **gyors üzembe helyezés** kattintson **kötet hozzáadása**. Ekkor elindul a hello kötet hozzáadása varázslóban.
2. Hello hozzá egy kötet varázslóban a **alapbeállítások**, a következő hello:
   
   1. Adjon **nevet** a kötetnek.
   2. Adja meg a hello **kiosztott kapacitást** a kötet GB-ban vagy TB. hello kötet kapacitása egy fizikai eszköz 1 GB és 64 TB-os között kell lennie.
   3. Hello a legördülő listán válassza ki a hello **használat típusa** a köteten. 
   4. Ha ezt a kötetet archív adatokhoz használ, jelölje be a hello **kötet használata ritkábban használt archív adatokhoz** jelölőnégyzetet. Minden más használat esetén egyszerűen válassza a **Rétegzett kötet** lehetőséget. (A rétegzett kötetek neve korábban elsődleges kötet volt.)
      
        ![Kötet hozzáadása](./media/storsimple-create-volume/ScreenshotUpdate1VolumeFlow.png)
      
      1. Hello nyíl ikonra ![nyíl ikon](./media/storsimple-create-volume/HCS_ArrowIcon-include.png) toogo toohello következő oldalra.
3. A hello **további beállításokat** párbeszédpanelen adja hozzá egy új hozzáférés-vezérlési rekordot (ACR):
   
   1. Adja meg az ACR **nevét**.
   2. A **iSCSI-kezdeményező neve**, adja meg a hello iSCSI minősített nevét (IQN) Windows-gazdagépen. Ha nem rendelkezik hello IQN, nyissa meg túl[Get hello egy Windows Server-állomás IQN-Nevének](#get-the-iqn-of-a-windows-server-host).
   3. Ajánlott engedélyezni a alapértelmezett biztonsági mentés hello kiválasztásával **engedélyezése ehhez a kötethez alapértelmezett biztonsági mentés** jelölőnégyzetet. hello alapértelmezett biztonsági mentés létrehoz egy házirendet, amely mindennap 22:30-kor (az eszköz ideje), és létrehoz egy felhő-pillanatfelvételt a kötetről.
      
      > [!NOTE]
      > Hello biztonsági mentés itt engedélyezése után nem lehet visszaállítani. Ezt a beállítást kell tooedit hello kötet toomodify.
      > 
      > 
      
        ![Kötet hozzáadása](./media/storsimple-create-volume/AddVolume2-include.png)
4. Kattintson a pipa ikonra hello ![pipa ikon](./media/storsimple-create-volume/HCS_CheckIcon-include.png). A kötet jön létre a megadott hello beállításait.

![Videó elérhető](./media/storsimple-create-volume/Video_icon.png)**Videó elérhető**

videó toowatch hogyan toocreate a StorSimple-kötet kattintson [Itt](https://azure.microsoft.com/documentation/videos/create-a-storsimple-volume/).

