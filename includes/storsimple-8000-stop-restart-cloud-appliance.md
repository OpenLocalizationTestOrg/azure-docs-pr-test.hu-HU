#### <a name="to-stop-and-start-a-cloud-appliance"></a>Felhőalapú készülék leállítása és indítása

1. Felhőalapú készülék leállításához lépjen a felhőalapú berendezéshez tartozó virtuális gépre.
    ![StorSimple Cloud Appliance virtuális gépe](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart1.png)

2. A parancssávon kattintson a **Leállítás** elemre.

    ![StorSimple Cloud Appliance virtuális gépe](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart2.png)

3. Ha a rendszer megerősítést kér, kattintson az **Igen** gombra.

    ![StorSimple Cloud Appliance virtuális gépe](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart3.png)

4. A leállított virtuális gépeket a rendszer felszabadítja. Amíg a felhőalapú készülék leállítása folyamatban van, az állapota **Felszabadítás**. A felhőalapú készülék leállítás utáni állapota **Leállítva (felszabadítva)**.

    ![StorSimple Cloud Appliance virtuális gépe](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart4.png)

5. Ha a virtuális gép leállt, kattintson a **Start** gombra (amikor a gomb elérhetővé válik) a virtuális gép elindításához. A felhőalapú készülék elindulás utáni állapota **Elindítva**.

    ![StorSimple Cloud Appliance virtuális gépe](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart5.png)

A felhőalapú készülékeket az alábbi parancsmagok segítségével állíthatja le és indíthatja el.

`Stop-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

`Start-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

#### <a name="to-restart-a-cloud-appliance"></a>Felhőalapú készülék újraindítása

Felhőalapú készülék újraindításához lépjen a felhőalapú berendezéshez tartozó virtuális gépre. A parancssávon kattintson az **Újraindítás** elemre. A rendszer kérésére erősítse meg az újraindítást. Ha a felhőalapú készülék használatra kész, az eszköz állapota **Fut**.

![StorSimple Cloud Appliance virtuális gépe](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart6.png)

A felhőalapú készüléket az alábbi parancsmaggal indíthatja újra.

`Restart-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

