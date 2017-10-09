#### <a name="toostop-and-start-a-virtual-device"></a>toostop és a virtuális eszköz indítása
toostop egy virtuális eszközt, kattintson a nevére, és kattintson a **leállítási**. Hello virtuális eszköz leállítás fázisában van, amíg állapotú-e **leállítása**. Hello virtuális eszköz leállítása után állapotú-e **leállítva**.

A következő parancsmagok toostop hello használja, és indítsa el a virtuális eszköz.

`Stop-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

`Start-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

#### <a name="toorestart-a-virtual-device"></a>a virtuális eszköz toorestart
Ha egy virtuális eszközön fut, és azt szeretné, hogy toorestart, kattintson a nevére, és kattintson **indítsa újra a**. Hello virtuális eszköz újraindítása, közben állapotú-e **újraindítása**. Virtuális eszköz hello meg toouse készen áll, amikor állapotú-e **futtató**.

A következő parancsmag toorestart a virtuális eszköz hello használata.

`Restart-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

