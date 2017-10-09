1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).

2. Kattintson a bal felső hello kezdődően **új > számítási > Windows Server 2016 Datacenter**.

    ![Keresse meg az Azure Virtuálisgép-rendszerképekről toohello hello portálon](./media/virtual-machines-common-portal-create-fqdn/marketplace-new.png)

3. A Windows Server 2016 Datacenter hello hello klasszikus telepítési modell kiválasztása Kattintson a Létrehozás gombra.

    ![Képernyőkép a hello portálon elérhető hello Azure Virtuálisgép-rendszerképekről](./media/virtual-machines-common-portal-create-fqdn/deployment-classic-model.png)

## <a name="1-basics-blade"></a>1. Alapvető beállítások panel

hello alapvető beállítások panel hello virtuális gép felügyeleti adatokat kér.

1. Adjon meg egy **neve** hello virtuális géphez. Hello példában _HeroVM_ hello hello virtuális gép neve. hello nevének 1 – 15 karakter hosszúságúnak kell lennie, és nem tartalmazhat különleges karaktereket.

2. Adjon meg egy **felhasználónév** és egy erős **jelszó** , amelyek használt toocreate helyi fiók a virtuális gép hello. hello helyi fiókot használja a tooand toosign hello VM kezelése. Hello példában _azureuser_ hello felhasználónév.

 hello jelszót kell 8 – 123 karakter hosszú, és három hello négy következő bonyolultsági megfeleljen: egy kisbetű, egy nagybetű, egy számot és egy speciális karaktert. További információk a [felhasználónév- és jelszókövetelményekről](../articles/virtual-machines/windows/faq.md).

3. Hello **előfizetés** nem kötelező megadni. Gyakori beállítás a „használatalapú fizetés”.

4. Válasszon ki egy létező **erőforráscsoport** vagy egy új hello típusnév. Hello példában _HeroVMRG_ hello hello erőforráscsoport neve.

5. Válasszon egy Azure-adatközpontban **hely** hello VM toorun, ahová. Hello példában **USA keleti régiója** hello helye.

6. Amikor elkészült, kattintson a **következő** toocontinue toohello következő panelen.

    ![Képernyőkép a hello-beállítások a hello alapvető beállítások panel az Azure virtuális gép konfigurálása](./media/virtual-machines-common-portal-create-fqdn/basics-blade-classic.png)

## <a name="2-size-blade"></a>2. Méret panel

hello méret panelen azonosítja a virtuális gép hello hello konfigurációs részleteit, és felsorolja a különböző döntések, amely tartalmazza az operációs rendszer, processzorok, a lemez tárolási típus, és a becsült havi használati költségek száma.  

Válassza ki a virtuális gép méretét, és kattintson a **válasszon** toocontinue. Ebben a példában _DS1_\__V2 szabványos_ van hello Virtuálisgép-méretet.

  ![Képernyőfelvétel a hello méret panelen kiválasztható Azure Virtuálisgép-méretek jeleníti meg a hello](./media/virtual-machines-common-portal-create-fqdn/vm-size-classic.png)


## <a name="3-settings-blade"></a>3. Beállítások panel

hello-beállítások panelen a kérelmek tárolási és hálózati beállítások. Elfogadhatja hello alapértelmezett beállításokat. Az Azure szükség szerint létrehozza a megfelelő bejegyzéseket.

Ha megfelelő méretet választott a virtuális gépnek, kipróbálhatja az Azure Premium Storage-ot – ehhez válassza a Prémium (SSD) elemet a Lemez típusa részen.

A módosítások végrehajtása után kattintson az **OK** gombra.

## <a name="4-summary-blade"></a>4. Összefoglalás panel

hello összefoglaló panel hello előző paneleken megadott hello beállításokat sorolja fel. Kattintson a **OK** készen toomake hello kép közben.

 ![Hello virtuális gép megadott beállítások panel összefoglaló jelentést](./media/virtual-machines-common-portal-create-fqdn/summary-blade-classic.png)

Hello virtuális gép létrehozása után hello portál megjeleníti-e a hello új virtuális gépek **összes erőforrás**, és hello irányítópult hello virtuális gép egy csempe megjeleníti. hello megfelelő felhőalapú szolgáltatás és a tárolási fiók is létrehozni és szerepel a listában. Hello virtuális gép és a felhőalapú szolgáltatás automatikusan elindulnak, és azok állapottal jelenik meg, **futtató**.

 ![Virtuálisgép-ügynök és hello végpontok hello virtuális gép konfigurálása](./media/virtual-machines-common-portal-create-fqdn/portal-with-new-vm.png)
