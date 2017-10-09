## <a name="defining-a-backup-policy"></a>Biztonsági mentési házirend meghatározása
A biztonsági mentési házirend egy mátrixot határoz meg, amikor hello pillanatképet készíteni az adatokról, és mennyi ideig e pillanatképet megőrzi. Virtuális gépek biztonsági mentéséről szóló házirend meghatározása esetén *naponta egyszer* indíthat el biztonsági mentési feladatot. Új házirend létrehozása esetén alkalmazott toohello tárolóban. hello biztonsági mentési házirendek felülete így néz ki:

![Biztonsági mentési házirend](./media/backup-create-policy-for-vms/backup-policy.png)

egy házirend toocreate:

1. Adjon meg egy nevet hello **házirendnév**.
2. Az adatokról napi vagy heti rendszerességgel készíthet pillanatképet. Használjon hello **biztonsági mentési gyakorisága** legördülő menü toochoose e pillanatképet készíteni az adatokról napi vagy heti rendszerességgel.
   
   * Ha úgy dönt, hogy a napi időköz, használja a kijelölt hello vezérlő tooselect hello időpontot hello hello pillanatkép. toochange hello órában, kijelölését hello óra, és hello válasszon egy másikat.
     
     ![Napi biztonsági mentési házirend](./media/backup-create-policy-for-vms/backup-policy-daily.png) <br/>
   * Ha a heti rendszerességet választja, használja a hello kiemelt vezérlőkkel tooselect hello nap hello hét, és a nap tootake hello pillanatkép hello időpontja. Hello nap menüben válasszon ki egy vagy több napot. Hello óra menüben válasszon ki egy óra. toochange hello órában, kijelölését hello kiválasztott órát, és hello válasszon egy másikat.
     
     ![Heti biztonsági mentési házirend](./media/backup-create-policy-for-vms/backup-policy-weekly.png)
3. Alapértelmezés szerint az összes **Megőrzési időtartam** beállítás ki van választva. Törölje a megőrzési időtartam azon korlátozásainak nem szeretné, hogy toouse. Ezt követően adja a hello interval(s) toouse.
   
    Havi és éves megőrzési időtartamok lehetővé teszik a heti vagy napi növekményes alapján toospecify hello pillanatképeket.
   
   > [!NOTE]
   > A virtuális gépek védelméhez a rendszer naponta egyszer készít biztonsági mentést. hello idő, amikor hello biztonsági mentés van hello azonos minden megőrzési időtartam.
   > 
   > 
4. Beállítása után hello házirend beállításait minden hello panel hello tetején kattintson **mentése**.
   
    Új házirend hello azonnal alkalmazott toohello tárolóban.

