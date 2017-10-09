<span data-ttu-id="4c895-101">Előfordulhat, hogy egyes csomagok nem települnek a pip használatával, ha Azure-on futtatja őket.</span><span class="sxs-lookup"><span data-stu-id="4c895-101">Some packages may not install using pip when run on Azure.</span></span>  <span data-ttu-id="4c895-102">Hello csomag nem érhető el a Python-Csomagindexet hello egyszerűen lehet.</span><span class="sxs-lookup"><span data-stu-id="4c895-102">It may simply be that hello package is not available on hello Python Package Index.</span></span>  <span data-ttu-id="4c895-103">Annak oka az lehet, hogy egy fordító szükséges (a fordító nem áll rendelkezésre hello gépen futó hello webalkalmazást az Azure App Service szolgáltatásban).</span><span class="sxs-lookup"><span data-stu-id="4c895-103">It could be that a compiler is required (a compiler is not available on hello machine running hello web app in Azure App Service).</span></span>

<span data-ttu-id="4c895-104">Ebben a szakaszban megnézzük, módon toodeal probléma megoldásához.</span><span class="sxs-lookup"><span data-stu-id="4c895-104">In this section, we'll look at ways toodeal with this issue.</span></span>

### <a name="request-wheels"></a><span data-ttu-id="4c895-105">Kerekek kérése</span><span class="sxs-lookup"><span data-stu-id="4c895-105">Request wheels</span></span>
<span data-ttu-id="4c895-106">Hello csomag telepítéséhez fordító szükséges, ha meg kell hello csomag tulajdonos toorequest hello csomag elérhetővé kerekeket való kapcsolódás.</span><span class="sxs-lookup"><span data-stu-id="4c895-106">If hello package installation requires a compiler, you should try contacting hello package owner toorequest that wheels be made available for hello package.</span></span>

<span data-ttu-id="4c895-107">Hello nemrégiben elérhetővé vált a [Microsoft Visual C++ Compiler for Python 2.7][Microsoft Visual C++ Compiler for Python 2.7], mostantól egyszerűbb toobuild rendelkező csomagok építését natív Python 2.7.</span><span class="sxs-lookup"><span data-stu-id="4c895-107">With hello recent availability of [Microsoft Visual C++ Compiler for Python 2.7][Microsoft Visual C++ Compiler for Python 2.7], it is now easier toobuild packages that have native code for Python 2.7.</span></span>

### <a name="build-wheels-requires-windows"></a><span data-ttu-id="4c895-108">Kerekek építése (Windows rendszert igényel)</span><span class="sxs-lookup"><span data-stu-id="4c895-108">Build wheels (requires Windows)</span></span>
<span data-ttu-id="4c895-109">Megjegyzés: Ez a beállítás használata esetén győződjön meg arról, hogy toocompile hello csomag Python-környezetben, amely megfelel a hello webalkalmazást az Azure App Service-ben használt a platformmal/architektúrával/verzióval hello (Windows/32-bit/2.7 vagy 3.4).</span><span class="sxs-lookup"><span data-stu-id="4c895-109">Note: When using this option, make sure toocompile hello package using a Python environment that matches hello platform/architecture/version that is used on hello web app in Azure App Service (Windows/32-bit/2.7 or 3.4).</span></span>

<span data-ttu-id="4c895-110">Ha hello csomag telepítése nem, mert fordító igényel, hello fordító telepítése a helyi számítógépen, és építsen egy kereket hello csomaghoz, amelyet majd belefoglalhat a tárházba.</span><span class="sxs-lookup"><span data-stu-id="4c895-110">If hello package doesn't install because it requires a compiler, you can install hello compiler on your local machine and build a wheel for hello package, which you will then include in your repository.</span></span>

<span data-ttu-id="4c895-111">Mac/Linux-felhasználók: Ha a hozzáférési tooa Windows számítógép nem rendelkezik, tekintse meg [hozzon létre egy virtuális gép futó Windows] [ Create a Virtual Machine Running Windows] arról, hogyan toocreate egy Azure virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="4c895-111">Mac/Linux Users: If you don't have access tooa Windows machine, see [Create a Virtual Machine Running Windows][Create a Virtual Machine Running Windows] for how toocreate a VM on Azure.</span></span>  <span data-ttu-id="4c895-112">Toobuild hello kerekek használják, toohello tárház adja hozzá, és ha szeretné vetni hello VM.</span><span class="sxs-lookup"><span data-stu-id="4c895-112">You can use it toobuild hello wheels, add them toohello repository, and discard hello VM if you like.</span></span> 

<span data-ttu-id="4c895-113">Python 2.7 esetén telepítheti [Microsoft Visual C++ Compiler for Python 2.7][Microsoft Visual C++ Compiler for Python 2.7].</span><span class="sxs-lookup"><span data-stu-id="4c895-113">For Python 2.7, you can install [Microsoft Visual C++ Compiler for Python 2.7][Microsoft Visual C++ Compiler for Python 2.7].</span></span>

<span data-ttu-id="4c895-114">Python 3.4 esetén telepítheti [Microsoft Visual C++ 2010 Express][Microsoft Visual C++ 2010 Express].</span><span class="sxs-lookup"><span data-stu-id="4c895-114">For Python 3.4, you can install [Microsoft Visual C++ 2010 Express][Microsoft Visual C++ 2010 Express].</span></span>

<span data-ttu-id="4c895-115">kerekek toobuild, szüksége lesz a kerékcsomagra hello:</span><span class="sxs-lookup"><span data-stu-id="4c895-115">toobuild wheels, you'll need hello wheel package:</span></span>

    env\scripts\pip install wheel

<span data-ttu-id="4c895-116">Fogjuk `pip wheel` toocompile egy függőséget:</span><span class="sxs-lookup"><span data-stu-id="4c895-116">You'll use `pip wheel` toocompile a dependency:</span></span>

    env\scripts\pip wheel azure==0.8.4

<span data-ttu-id="4c895-117">Ez létrehoz egy .whl fájlt hello \wheelhouse mappában.</span><span class="sxs-lookup"><span data-stu-id="4c895-117">This creates a .whl file in hello \wheelhouse folder.</span></span>  <span data-ttu-id="4c895-118">Hello \wheelhouse mappát és kerék fájlok tooyour tárház hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="4c895-118">Add hello \wheelhouse folder and wheel files tooyour repository.</span></span>

<span data-ttu-id="4c895-119">Szerkessze a requirements.txt tooadd hello `--find-links` hello felső lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="4c895-119">Edit your requirements.txt tooadd hello `--find-links` option at hello top.</span></span> <span data-ttu-id="4c895-120">Ez alapján a pip toolook pontos egyezést előtt folyamatos toohello python-csomagindexet hello helyi mappában.</span><span class="sxs-lookup"><span data-stu-id="4c895-120">This tells pip toolook for an exact match in hello local folder before going toohello python package index.</span></span>

    --find-links wheelhouse
    azure==0.8.4

<span data-ttu-id="4c895-121">Ha azt szeretné, hogy minden index hello \wheelhouse mappát és nem használható hello python csomagban lévő összes függősége tooinclude, pip tooignore hello csomagindexet kényszerítheti hozzáadásával `--no-index` toohello a Requirements.txt fájl tetejéhez.</span><span class="sxs-lookup"><span data-stu-id="4c895-121">If you want tooinclude all your dependencies in hello \wheelhouse folder and not use hello python package index at all, you can force pip tooignore hello package index by adding `--no-index` toohello top of your requirements.txt.</span></span>

    --no-index

### <a name="customize-installation"></a><span data-ttu-id="4c895-122">A telepítés testreszabása</span><span class="sxs-lookup"><span data-stu-id="4c895-122">Customize installation</span></span>
<span data-ttu-id="4c895-123">Testre szabhatja a hello telepítési parancsfájl tooinstall hello virtuális környezetben egy alternatív telepítővel, például a easy csomag\_telepítése.</span><span class="sxs-lookup"><span data-stu-id="4c895-123">You can customize hello deployment script tooinstall a package in hello virtual environment using an alternate installer, such as easy\_install.</span></span>  <span data-ttu-id="4c895-124">A deploy.cmd fájlban megtekinthet egy megjegyzésként szereplő példát.  Győződjön meg arról, hogy az ilyen jellegű csomagok, tooprevent pip ne telepítse őket a requirements.txt fájlban nem láthatók.</span><span class="sxs-lookup"><span data-stu-id="4c895-124">See deploy.cmd for an example that is commented out.  Make sure that such packages aren't listed in requirements.txt, tooprevent pip from installing them.</span></span>

<span data-ttu-id="4c895-125">Adja hozzá az toohello telepítési parancsfájlba:</span><span class="sxs-lookup"><span data-stu-id="4c895-125">Add this toohello deployment script:</span></span>

    env\scripts\easy_install somepackage

<span data-ttu-id="4c895-126">Is könnyen képes toouse\_tooinstall telepíthessenek exe telepítő (zip-kompatibilis, ezért az easy közül néhány\_install támogatja őket).</span><span class="sxs-lookup"><span data-stu-id="4c895-126">You may also be able toouse easy\_install tooinstall from an exe installer (some are zip compatible, so easy\_install supports them).</span></span>  <span data-ttu-id="4c895-127">Vegyen fel hello telepítő tooyour tárházat, és hívja meg az easy\_telepítése úgy, hogy az elérési út toohello hello végrehajtható.</span><span class="sxs-lookup"><span data-stu-id="4c895-127">Add hello installer tooyour repository, and invoke easy\_install by passing hello path toohello executable.</span></span>

<span data-ttu-id="4c895-128">Adja hozzá az toohello telepítési parancsfájlba:</span><span class="sxs-lookup"><span data-stu-id="4c895-128">Add this toohello deployment script:</span></span>

    env\scripts\easy_install "%DEPLOYMENT_SOURCE%\installers\somepackage.exe"

### <a name="include-hello-virtual-environment-in-hello-repository-requires-windows"></a><span data-ttu-id="4c895-129">Hello virtuális környezetbe felvenni hello tárházba (Windowst igényel)</span><span class="sxs-lookup"><span data-stu-id="4c895-129">Include hello virtual environment in hello repository (requires Windows)</span></span>
<span data-ttu-id="4c895-130">Megjegyzés: Ez a beállítás használata esetén győződjön meg arról, hogy toouse hello webalkalmazást az Azure App Service-ben használt a platformmal/architektúrával/verzióval hello megfelelő virtuális környezetet (Windows/32-bit/2.7 vagy 3.4).</span><span class="sxs-lookup"><span data-stu-id="4c895-130">Note: When using this option, make sure toouse a virtual environment that matches hello platform/architecture/version that is used on hello web app in Azure App Service (Windows/32-bit/2.7 or 3.4).</span></span>

<span data-ttu-id="4c895-131">Ha hello virtuális környezet hello tárházban, megakadályozhatja a hello üzembe helyezési parancsfájl virtuáliskörnyezet-felügyeletet folytasson Azure létrehoz egy üres fájlt:</span><span class="sxs-lookup"><span data-stu-id="4c895-131">If you include hello virtual environment in hello repository, you can prevent hello deployment script from doing virtual environment management on Azure by creating an empty file:</span></span>

    .skipPythonDeployment

<span data-ttu-id="4c895-132">Azt javasoljuk, hogy törölje hello létező virtuális környezetet az hello app, tooprevent vissza fájlok akkorról, amikor hello virtuális környezet felügyelete automatikus volt.</span><span class="sxs-lookup"><span data-stu-id="4c895-132">We recommend that you delete hello existing virtual environment on hello app, tooprevent leftover files from when hello virtual environment was managed automatically.</span></span>

[Create a Virtual Machine Running Windows]: http://azure.microsoft.com/documentation/articles/virtual-machines-windows-hero-tutorial/
[Microsoft Visual C++ Compiler for Python 2.7]: http://aka.ms/vcpython27
[Microsoft Visual C++ 2010 Express]: http://go.microsoft.com/?linkid=9709949
