#### <a name="toocreate-a-cloud-appliance"></a>a felhő készülék toocreate

1. A hello Azure-portálon, válassza a toohello **StorSimple Device Manager** szolgáltatás.
2. Nyissa meg toohello **eszközök** panelen. Hello parancssávon hello szolgáltatás összefoglaló paneljén kattintson **létrehozás felhő készülék**.
    ![StorSimple felhőalapú berendezés létrehozása](./media/storsimple-8000-create-cloud-appliance-u2/sca-create1.png)
3. A hello **létrehozás felhő készülék** panelen adja meg a következő adatok hello.
   
    ![StorSimple felhőalapú berendezés létrehozása](./media/storsimple-8000-create-cloud-appliance-u2/sca-create2m.png)
   
   1. **Név** – A felhőalapú berendezés egyedi neve.
   2. **Modell** -hello felhő készülék hello modell kiválasztása. A 8010-es eszköz 30 TB Standard szintű, a 8020-as pedig 64 TB Prémium szintű tárhelyet biztosít. Adja meg a biztonsági mentések 8010 toodeploy elem elemszintű lekérése lehetőségeket. Válassza ki a 8020, toodeploy nagy teljesítményű, alacsony késleltetésű munkaterhelések, vagy vész-helyreállítási másodlagos eszközként használni.
   3. **Verzió** -hello felhő készülék hello verziójának kiválasztása. hello verziója megfelel-e, amely használt toocreate hello felhő készülék hello virtuális lemezképet toohello verziója. A megadott hello felhő hello verziója készülék meghatározza, hogy mely fizikai eszköz feladatátvételt, vagy a klónozáshoz, fontos, hogy hozzon létre egy hello felhő készülék megfelelő verzióját.
   4. **Virtuális hálózati** – adjon meg egy virtuális hálózatot, amelyet meg szeretne toouse a felhő készüléket. Prémium szintű Storage használata esetén ki kell választania egy virtuális hálózatot, amely a prémium szintű Storage-fiók hello támogatott. virtuális hálózatok nem támogatott hello hello legördülő lista szürkén jelennek meg. Ha nem támogatott virtuális hálózatot választ ki, a rendszer figyelmezteti.
   5. **Alhálózati** -kijelölt hello virtuális hálózaton, hello legördülő lista megjeleníti kapcsolódó hello alhálózatok. Rendeljen hozzá egy alhálózatot tooyour felhő készülék.
   6. **A tárfiók** – válassza ki a toohold hello tárfióklemezkép hello felhő készülék kiépítése során. Ezt a tárfiókot kell hello és hello felhő készülék és a virtuális hálózat ugyanabban a régióban. Nem lehet megjeleníteni tárolására fizikai hello vagy hello felhő készülék. Alapértelmezés szerint erre a célra létrejön egy új tárfiók. Azonban ha tudja, hogy már rendelkezik egy tárfiókot, amely lehetővé teszi a használatot, is kiválaszthatja hello listából. A prémium szintű felhőalapú készülék létrehozása, ha hello legördülő listából, az csak prémium szintű Storage-fiókok jeleníti meg.
      
      > [!NOTE]
      > hello felhő készülék csak dolgozhat hello Azure storage-fiókok.
    
   7. Válassza ki, hogy tudomásul veszi, hogy hello felhő készüléken tárolt hello adatokat egy Microsoft-adatközpontban található hello jelölőnégyzet tooindicate.
       * Amikor csak fizikai eszközt használ, a titkosítási kulcs az eszközön található, ezért a Microsoft nem tudja feloldani a titkosítást.

       * A felhő készülék használatakor hello titkosítási kulcs és a visszafejtési kulcs hello tárolják a Microsoft Azure-ban. További információkért tekintse meg a [felhőalapú berendezések használatára vonatkozó biztonsági szempontokat](../articles/storsimple/storsimple-security.md#storsimple-virtual-device-security).
   8. Kattintson a **létrehozása** tooprovision hello felhő készüléket. hello eszköz kiépítése körülbelül 30 percet toobe is igénybe vehet. Értesítést kap hello felhő készülék sikeresen létrehozva. Nyissa meg tooDevices panelt, és eszközök listája hello toodisplay hello felhő készülék frissíti. hello hello készülék állapota **tooset kész mentése**.
      
      ![StorSimple felhő készülék készen tooset mentése](./media/storsimple-8000-create-cloud-appliance-u2/sca-create3.png)

