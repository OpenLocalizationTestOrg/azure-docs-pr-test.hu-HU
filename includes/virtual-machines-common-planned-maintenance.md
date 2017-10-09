Azure rendszeres időközönként frissítéseket tooimprove hello megbízhatóságát, teljesítményét és hello állomás infrastruktúra biztonságát virtuális gépekhez. Ezek a frissítések között javítását szoftverösszetevőket hello üzemeltetési környezet (például az operációs rendszer, hipervizor, és különböző ügynökök hello gazdagépen rendszerbe állított), a hálózati összetevők frissítése toohardware leszerelése. Ezek a frissítések hello többsége semmilyen hatása toohello üzemeltetett virtuális gépek nélkül kerül sor. Vannak azonban esetekben, amikor frissítések ütközés:

- Hello karbantartási nem igényel újraindítást, ha Azure helyben történő áttelepítés toopause hello virtuális gép használja, amíg a rendszer frissíti a hello állomás.

- Ha karbantartási újraindítást igényel, kapott értesítés hello karbantartási tervezett. Ezekben az esetekben akkor lesz is kell adni egy olyan időkeretet, ahol megkezdése hello karbantartási saját magának, megfelelő egyszerre.

Ez a lap ismerteti, hogyan Microsoft Azure-ban mindkét karbantartási típusú. Nem tervezett események (hosszabb idejű) kapcsolatos további információkért lásd: a virtuális gépek kezelése hello elérhetőségét [Windows] (.. / articles/virtual-machines/windows/manage-availability.md) vagy [Linux](../articles/virtual-machines/linux/manage-availability.md).

A virtuális gépen futó alkalmazások is összefog információ a jövőbeli frissítések hello Azure metaadat-szolgáltatás a [Windows](../articles/virtual-machines/windows/instance-metadata-service.md) vagy [Linux] (.. / articles/virtual-machines/linux/instance-metadata-service.md).

## <a name="in-place-vm-migration"></a>Helyszíni virtuális gép áttelepítés

Ha a frissítéseket egy teljes számítógép újraindítása nem szükséges, egy helyszíni élő áttelepítés szolgál. Hello frissítésekor hello virtuális gép fel van függesztve, körülbelül 30 másodpercig, megőrzi az hello memóriája RAM, amíg üzemeltetési környezet hello hello szükséges frissítések és javítások vonatkozik. virtuális gép hello majd folytatja a működését, és hello óra hello virtuális gép a rendszer automatikusan szinkronizálja.

Virtuális gépek a rendelkezésre állási csoportokban a frissítés tartományai frissített egyszerre csak egy. Frissítési tartományok (UD) virtuális gépeinek szünetel, frissítése és majd folytatása előtt a tervezett karbantartások toohello mozgatása következő UD.

Egyes alkalmazások negatív hatással lehet az ilyen típusú frissítések. Valós idejű Eseményfeldolgozási, például a médiaadatfolyam vagy az átkódolás vagy a magas teljesítmény forgatókönyvek, hálózati végző alkalmazások nem lehet tervezett tootolerate egy 30 másodperces szünet. <!-- sooooo, what should they do? --> 


## <a name="maintenance-requiring-a-reboot"></a>A rendszer újraindítását igénylik karbantartás

Ha a virtuális gépek kell indítani, hogy a tervezett karbantartások toobe, értesítés jelenik meg előre. Tervezett karbantartás két szakasza van: hello önkiszolgáló ablakot, és egy ütemezett karbantartási időszaknál.

Hello **önkiszolgáló ablak** lehetővé teszi, hogy a virtuális gépeken hello karbantartási kezdeményezni. Ebben az időszakban minden virtuális gép toosee azok állapotának lekérdezése, és ellenőrizze az utolsó karbantartási kérése hello eredményét.

Amikor elindítja a karbantartási önkiszolgáló, a virtuális gép áthelyezett tooa csomópontra, amely már frissítve van, majd bekapcsolja azt vissza. Hello virtuális gép újraindul, mert hello mennyiségű ideiglenes lemezes elvész, és dinamikus, virtuális hálózati interfészhez társított IP-címek frissülnek.

Ha önkiszolgáló karbantartási indítja el, és nem sikerül hello folyamat során, hello művelet le van állítva, a hello virtuális gép nem frissül, és hello tervezett karbantartás iterációs is eltávolítja. Lesz, később egy új ütemezéssel rendelkező nyelven kapcsolatba Önnel, és egy új lehetőség toodo önkiszolgáló karbantartási kínált. 

Hello önkiszolgáló időszak elteltével hello **ütemezett karbantartási időszaknál** kezdődik. Időablak során továbbra is kereshet hello karbantartási időszak, de már nem képes toostart hello karbantartási magát.

## <a name="availability-considerations-during-planned-maintenance"></a>Tervezett karbantartás során a rendelkezésre állási lehetőségekért 

Ha toowait amíg hello tervezett karbantartási időszak, van néhány dolgot tooconsider megőrzéséhez a legmagasabb availabilty hello a virtuális gépek. 

### <a name="paired-regions"></a>Párhuzamos régiók

Minden Azure-régió, egy másik régióban belül hello párosított azonos földrajzi hely, együtt, akkor egy regionális pár. Tervezett karbantartás közben Azure csak frissíti a virtuális gépek régió pár egy régió hello. Például hello északi középső Régiójában lévő virtuális gépek frissítésekor Azure nem fogja frissíteni a virtuális gépek déli középső Régiójában: hello ugyanannyi időt vesz igénybe. Azonban más régiókból, mint például a karbantartás alatt lehet Észak-Európa hello azonos időben USA keleti régiója is. Ismertetése régió párok működése segítségével jobban elosztása a virtuális gépek régiók. További információkért lásd: [az Azure-régió párok](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

### <a name="availability-sets-and-scale-sets"></a>Rendelkezésre állási készletek és a méretezési csoportok

A munkaterhelés Azure virtuális gépeken való telepítésekor hello virtuális gépek rendelkezésre állási készlet tooprovide magas rendelkezésre állású tooyour alkalmazáson belül is létrehozhat. Ez biztosítja, hogy szolgáltatáskimaradás vagy karbantartási események, legalább egy virtuális gép érhető el.

Egy rendelkezésre állási csoportot belül az egyes virtuális gépek too20 frissítési tartomány (UDs) vannak elosztva. Tervezett karbantartás közben csak egyetlen frissítési tartományi egy adott időpontban van hatással. Vegye figyelembe, hogy befolyásolja a frissítési tartományok hello sorrendje nem feltétlenül fordulhat elő egymás után. 

Virtuálisgép-méretezési készlet, amely lehetővé teszi az Azure számítási erőforrás toodeploy, és az azonos virtuális gépek egyetlen erőforrásként kezelésére. hello méretezési automatikusan telepíti a frissítési tartományokon, például a virtuális gépek rendelkezésre állási csoportba. Csakúgy, mint a rendelkezésre állási csoportok esetében a méretezési csoportok csak egyetlen frissítési tartományi van hatással az adott időpontban.

A magas rendelkezésre állású virtuális gépek konfigurálásával kapcsolatos további információkért lásd a Windows hello rendelkezésre állásának kezelése a virtuális gépek (.. / articles/virtual-machines/windows/manage-availability.md) vagy [Linux](../articles/virtual-machines/linux/manage-availability.md).
