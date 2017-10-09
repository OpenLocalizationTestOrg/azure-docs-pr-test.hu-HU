Előfordulhat, hogy egyes csomagok nem települnek a pip használatával, ha Azure-on futtatja őket.  Hello csomag nem érhető el a Python-Csomagindexet hello egyszerűen lehet.  Annak oka az lehet, hogy egy fordító szükséges (a fordító nem áll rendelkezésre hello gépen futó hello webalkalmazást az Azure App Service szolgáltatásban).

Ebben a szakaszban megnézzük, módon toodeal probléma megoldásához.

### <a name="request-wheels"></a>Kerekek kérése
Hello csomag telepítéséhez fordító szükséges, ha meg kell hello csomag tulajdonos toorequest hello csomag elérhetővé kerekeket való kapcsolódás.

Hello nemrégiben elérhetővé vált a [Microsoft Visual C++ Compiler for Python 2.7][Microsoft Visual C++ Compiler for Python 2.7], mostantól egyszerűbb toobuild rendelkező csomagok építését natív Python 2.7.

### <a name="build-wheels-requires-windows"></a>Kerekek építése (Windows rendszert igényel)
Megjegyzés: Ez a beállítás használata esetén győződjön meg arról, hogy toocompile hello csomag Python-környezetben, amely megfelel a hello webalkalmazást az Azure App Service-ben használt a platformmal/architektúrával/verzióval hello (Windows/32-bit/2.7 vagy 3.4).

Ha hello csomag telepítése nem, mert fordító igényel, hello fordító telepítése a helyi számítógépen, és építsen egy kereket hello csomaghoz, amelyet majd belefoglalhat a tárházba.

Mac/Linux-felhasználók: Ha a hozzáférési tooa Windows számítógép nem rendelkezik, tekintse meg [hozzon létre egy virtuális gép futó Windows] [ Create a Virtual Machine Running Windows] arról, hogyan toocreate egy Azure virtuális gép.  Toobuild hello kerekek használják, toohello tárház adja hozzá, és ha szeretné vetni hello VM. 

Python 2.7 esetén telepítheti [Microsoft Visual C++ Compiler for Python 2.7][Microsoft Visual C++ Compiler for Python 2.7].

Python 3.4 esetén telepítheti [Microsoft Visual C++ 2010 Express][Microsoft Visual C++ 2010 Express].

kerekek toobuild, szüksége lesz a kerékcsomagra hello:

    env\scripts\pip install wheel

Fogjuk `pip wheel` toocompile egy függőséget:

    env\scripts\pip wheel azure==0.8.4

Ez létrehoz egy .whl fájlt hello \wheelhouse mappában.  Hello \wheelhouse mappát és kerék fájlok tooyour tárház hozzáadása.

Szerkessze a requirements.txt tooadd hello `--find-links` hello felső lehetőséget. Ez alapján a pip toolook pontos egyezést előtt folyamatos toohello python-csomagindexet hello helyi mappában.

    --find-links wheelhouse
    azure==0.8.4

Ha azt szeretné, hogy minden index hello \wheelhouse mappát és nem használható hello python csomagban lévő összes függősége tooinclude, pip tooignore hello csomagindexet kényszerítheti hozzáadásával `--no-index` toohello a Requirements.txt fájl tetejéhez.

    --no-index

### <a name="customize-installation"></a>A telepítés testreszabása
Testre szabhatja a hello telepítési parancsfájl tooinstall hello virtuális környezetben egy alternatív telepítővel, például a easy csomag\_telepítése.  A deploy.cmd fájlban megtekinthet egy megjegyzésként szereplő példát.  Győződjön meg arról, hogy az ilyen jellegű csomagok, tooprevent pip ne telepítse őket a requirements.txt fájlban nem láthatók.

Adja hozzá az toohello telepítési parancsfájlba:

    env\scripts\easy_install somepackage

Is könnyen képes toouse\_tooinstall telepíthessenek exe telepítő (zip-kompatibilis, ezért az easy közül néhány\_install támogatja őket).  Vegyen fel hello telepítő tooyour tárházat, és hívja meg az easy\_telepítése úgy, hogy az elérési út toohello hello végrehajtható.

Adja hozzá az toohello telepítési parancsfájlba:

    env\scripts\easy_install "%DEPLOYMENT_SOURCE%\installers\somepackage.exe"

### <a name="include-hello-virtual-environment-in-hello-repository-requires-windows"></a>Hello virtuális környezetbe felvenni hello tárházba (Windowst igényel)
Megjegyzés: Ez a beállítás használata esetén győződjön meg arról, hogy toouse hello webalkalmazást az Azure App Service-ben használt a platformmal/architektúrával/verzióval hello megfelelő virtuális környezetet (Windows/32-bit/2.7 vagy 3.4).

Ha hello virtuális környezet hello tárházban, megakadályozhatja a hello üzembe helyezési parancsfájl virtuáliskörnyezet-felügyeletet folytasson Azure létrehoz egy üres fájlt:

    .skipPythonDeployment

Azt javasoljuk, hogy törölje hello létező virtuális környezetet az hello app, tooprevent vissza fájlok akkorról, amikor hello virtuális környezet felügyelete automatikus volt.

[Create a Virtual Machine Running Windows]: http://azure.microsoft.com/documentation/articles/virtual-machines-windows-hero-tutorial/
[Microsoft Visual C++ Compiler for Python 2.7]: http://aka.ms/vcpython27
[Microsoft Visual C++ 2010 Express]: http://go.microsoft.com/?linkid=9709949
