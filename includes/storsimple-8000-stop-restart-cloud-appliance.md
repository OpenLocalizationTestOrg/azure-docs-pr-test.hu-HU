#### <a name="toostop-and-start-a-cloud-appliance"></a>toostop és a felhő készülék start

1. egy felhőalapú készülék toostop nyissa meg a felhő alapplatformjaként VM toohello.
    ![StorSimple Cloud Appliance virtuális gépe](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart1.png)

2. Hello parancssávon kattintson **leállítása**.

    ![StorSimple Cloud Appliance virtuális gépe](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart2.png)

3. Ha a rendszer megerősítést kér, kattintson az **Igen** gombra.

    ![StorSimple Cloud Appliance virtuális gépe](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart3.png)

4. A leállított virtuális gépeket a rendszer felszabadítja. Hello felhő készülék leáll, miközben állapotú-e **Deallocating**. Hello felhő készülék leállítása után állapotú-e **leállítva (felszabadítva)**.

    ![StorSimple Cloud Appliance virtuális gépe](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart4.png)

5. Amikor egy virtuális gép le van állítva, kattintson a **Start** (gomb használhatóvá váljon) toostart hello virtuális gép. Miután hello felhő készülék elindult, állapotú-e **elindítva**.

    ![StorSimple Cloud Appliance virtuális gépe](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart5.png)

A következő parancsmagok toostop hello használja, és indítsa el a felhő alapplatformjaként.

`Stop-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

`Start-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

#### <a name="toorestart-a-cloud-appliance"></a>a felhő készülék toorestart

egy felhőalapú készülék toorestart nyissa meg a felhő alapplatformjaként VM toohello. Hello parancssávon kattintson **indítsa újra a**. Amikor a rendszer kéri, erősítse meg a hello újraindítás. Hello felhő készüléknek meg toouse készen áll, amikor állapotú-e **futtató**.

![StorSimple Cloud Appliance virtuális gépe](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart6.png)

A következő parancsmag toorestart egy felhőalapú készülék hello használata.

`Restart-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

