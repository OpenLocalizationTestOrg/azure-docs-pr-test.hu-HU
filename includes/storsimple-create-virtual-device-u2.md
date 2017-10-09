#### <a name="toocreate-a-virtual-device"></a>a virtuális eszköz toocreate
1. A hello Azure-portálon, válassza a toohello **StorSimple Manager** szolgáltatás.
2. Nyissa meg toohello **eszközök** lap. Kattintson a **virtuális eszköz létrehozása** hello hello alján **eszközök** lap.
3. A hello **virtuális eszköz létrehozása párbeszédpanelen**, adja meg a következő adatok hello.
   
    ![StorSimple virtuális eszköz létrehozása](./media/storsimple-create-virtual-device-u2/CreatePremiumsva1.png)
   
   1. **Név** – A virtuális eszköz egyedi neve.
   2. **Modell** -válassza ki a virtuális eszköz hello hello modelljét. Ez a mező csak akkor jelenik meg, ha a 2. frissítést vagy újabb verziót futtat. Egy 8010-es eszközmodell 30 TB Standard szintű, míg egy 8020-as modell 64 TB Premium Storage tárhelyet biztosít. Ha biztonsági mentésekből szeretne elemszintű lekérési forgatókönyveket telepíteni, a 8010-es
   3. toodeploy elem elemszintű lekérése forgatókönyvek biztonsági másolatból. Válassza ki a 8020-as modellt toodeploy nagy teljesítményű, alacsony késleltetésű munkaterhelések vagy katasztrófa utáni helyreállítás egy másodlagos eszközként használni.
   4. **Verzió** -hello virtuális eszköz hello verziójának kiválasztása. Ha egy 8020-as modellt eszközmodell van kiválasztva, majd hello verzió mező nem kínáljuk toohello felhasználói. Ez a beállítás hiányzik. Ha az összes fizikai hello a szolgáltatáshoz regisztrált eszközök az Update 1 (vagy újabb). Ez a mező akkor jelenik meg, csak akkor, ha a frissítés előtti 1 vegyesen és 1. frissítés fizikai eszközöknek regisztrálva hello azonos szolgáltatás. A megadott hello hello virtuális eszköz verziója határozza meg, hogy melyik fizikai eszköz használható feladatátvételhez vagy klónozáshoz, fontos, hogy hozzon létre egy hello virtuális eszköz megfelelő verzióját. A következők szerint válasszon:
      
      * Válassza a 0.3-as frissítést futtató verziót, ha 0.3-as vagy korábbi verziójú fizikai eszközről hajt végre feladatátvételt vagy DR műveletet. 
      * Válassza az 1. frissítést futtató verziót, ha 1. vagy újabb verziójú fizikai eszközről hajt végre feladatátvételt vagy klónozást. 
   5. **Virtuális hálózati** – adjon meg egy virtuális hálózatot, amelyet az toouse a virtuális eszközzel. Prémium szintű Storage (Update 2 vagy újabb) használata esetén ki kell választania egy virtuális hálózatot, amely a prémium szintű Storage-fiók hello támogatott. nem támogatott hello virtuális hálózatok szürkén jelennek meg a hello legördülő listában. Ha nem támogatott virtuális hálózatot választ ki, a rendszer figyelmezteti. 
   6. **A Tárfiók a virtuális eszköz létrehozásához** – válassza ki a toohold hello tárfióklemezkép hello virtuális eszköz kiépítése során. Ezt a tárfiókot kell hello és hello virtuális eszköz és a virtuális hálózat ugyanabban a régióban. Nem lehet megjeleníteni tárolására hello fizikai vagy virtuális eszköz hello. Alapértelmezés szerint erre a célra létrejön egy új tárfiók. Azonban ha tudja, hogy már rendelkezik egy tárfiókot, amely lehetővé teszi a használatot, is kiválaszthatja hello listából. Prémium szintű virtuális eszköz létrehozása, ha hello legördülő lista csak prémium szintű Storage-fiókok jeleníti meg. 
      
      > [!NOTE]
      > hello virtuális eszköz csak dolgozhat hello Azure storage-fiókok. Egyéb felhőszolgáltatók például az Amazon, HP és OpenStack (amelyek hello fizikai eszközön támogatottak) hello StorSimple virtuális eszköz nem támogatottak.
      > 
      > 
   7. Kattintson a hello pipa tooindicate, hogy tudomásul veszi, hogy hello virtuális eszközön tárolt hello adatok egy Microsoft-adatközpontban fog üzemelni. Amikor csak fizikai eszközt használ, a titkosítási kulcs az eszközön található, ezért a Microsoft nem tudja feloldani a titkosítást. 
      
       Amikor egy virtuális eszközt használ, hello titkosítási kulcs és a visszafejtési kulcs hello tárolják a Microsoft Azure-ban. További információkért tekintse meg a [virtuális eszközök használatára vonatkozó biztonsági szempontokat](../articles/storsimple/storsimple-security.md#storsimple-virtual-device-security).
   8. Kattintson a hello négyzet ikon toocreate hello virtuális eszközre. hello eszköz kiépítése körülbelül 30 percet toobe is igénybe vehet.
      
      ![StorSimple-virtuáliseszköz létrehozási fázisa](./media/storsimple-create-virtual-device-u2/StorSimple_VirtualDeviceCreating1M.png)

