hello telepítési parancsfájl hello virtuális környezet létrehozása kihagyja az Azure-on, ha azt észleli, hogy a kompatibilis virtuális környezet már létezik.  Ez jelentősen felgyorsíthatja a telepítést.  A pip kihagyja a már telepített csomagok telepítését.

Bizonyos esetekben érdemes lehet tooforce delete virtuális környezet.  Érdemes toodo ez Ha úgy dönt, a virtuális környezet tooinclude a tárház részeként.  Akkor is érdemes lehet toodo ez Ha tooget kell egyes csomagokat, vagy a módosítások toorequirements.txt tesztelése.

Van néhány beállítások toomanage hello meglévő Azure virtuális környezetben:

### <a name="option-1-use-ftp"></a>1. lehetőség: FTP használata
FTP-ügyféllel csatlakoztassa a kiszolgálót toohello, és be fog tudni toodelete hello env mappát.  Vegye figyelembe, hogy egyes FTP-ügyfelek (például a webböngészők) lehet, hogy csak olvasható, és nem teszik lehetővé toodelete mappákat, ezért érdemes toomake meg arról, hogy toouse az FTP-ügyfél ezt a funkciót a.  hello FTP-állomás nevének és a felhasználó jelennek meg a webalkalmazás panelen a hello [Azure Portal](https://portal.azure.com).

### <a name="option-2-toggle-runtime"></a>2. lehetőség: Futtatókörnyezet-váltás
Itt található, amely kihasználja hello tényt, hogy hello telepítési parancsfájl törli hello env mappát, ha az nem egyezik meg a Python kívánt verziójával hello helyett.  Ezzel hatékonyan hello meglévő Environment-környezet törlése, és hozzon létre egy újat.

1. Kapcsoló tooa eltérő verziójú Python (runtime.txt vagy hello **Alkalmazásbeállítások** panel az Azure portál hello)
2. továbbítson módosításokat a gittel (hagyja figyelmen kívül az esetleges pip-telepítési hibákat)
3. A Python kapcsoló hátsó tooinitial verzióját
4. továbbítson újabb módosításokat a gittel

### <a name="option-3-customize-deployment-script"></a>3. lehetőség: A telepítési parancsfájl testreszabása
Ha testre szabta hello telepítési parancsfájlt, módosíthatja a hello kód a Deploy.cmd fájl tooforce azt toodelete hello env mappát.

