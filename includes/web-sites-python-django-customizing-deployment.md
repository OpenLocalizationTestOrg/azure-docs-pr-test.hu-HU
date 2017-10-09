<span data-ttu-id="09d17-101">**Ha az alábbi két feltétel igaz**, az Azure megállapítja, hogy az alkalmazása a Pythont használja:</span><span class="sxs-lookup"><span data-stu-id="09d17-101">Azure will determine that your application uses Python **if both of these conditions are true**:</span></span>

* <span data-ttu-id="09d17-102">a Requirements.txt fájl hello gyökérmappa</span><span class="sxs-lookup"><span data-stu-id="09d17-102">requirements.txt file in hello root folder</span></span>
* <span data-ttu-id="09d17-103">bármely .py fájl hello gyökérmappában vagy egy runtime.txt, amely a pythont adja meg</span><span class="sxs-lookup"><span data-stu-id="09d17-103">any .py file in hello root folder OR a runtime.txt that specifies python</span></span>

<span data-ttu-id="09d17-104">Amikor hello esetben egy Python adott központi telepítési parancsfájl, amely hello szabványos szinkronizálását, fájlok, valamint egyéb Python-műveleteket, mint a használja:</span><span class="sxs-lookup"><span data-stu-id="09d17-104">When that's hello case, it will use a Python specific deployment script, which performs hello standard synchronization of files, as well as additional Python operations such as:</span></span>

* <span data-ttu-id="09d17-105">A virtuális környezet automatikus felügyeletét</span><span class="sxs-lookup"><span data-stu-id="09d17-105">Automatic management of virtual environment</span></span>
* <span data-ttu-id="09d17-106">A requirements.txt fájlban felsorolt csomagok pippel történő telepítését</span><span class="sxs-lookup"><span data-stu-id="09d17-106">Installation of packages listed in requirements.txt using pip</span></span>
* <span data-ttu-id="09d17-107">A hello megfelelő Web.config fájl létrehozását hello alapján kiválasztott Python-verzió.</span><span class="sxs-lookup"><span data-stu-id="09d17-107">Creation of hello appropriate web.config based on hello selected Python version.</span></span>
* <span data-ttu-id="09d17-108">A statikus fájlok összegyűjtését a Django-alkalmazások számára</span><span class="sxs-lookup"><span data-stu-id="09d17-108">Collect static files for Django applications</span></span>

<span data-ttu-id="09d17-109">Bizonyos elemeinek hello alapértelmezett telepítési lépést anélkül, hogy toocustomize hello parancsfájl szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="09d17-109">You can control certain aspects of hello default deployment steps without having toocustomize hello script.</span></span>

<span data-ttu-id="09d17-110">Ha tooskip Python adott központi telepítési lépéseket, akkor a üres fájl létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="09d17-110">If you want tooskip all Python specific deployment steps, you can create this empty file:</span></span>

    \.skipPythonDeployment

<span data-ttu-id="09d17-111">Ha tooskip statikus fájlok összegyűjtését a Django-alkalmazáshoz használni szeretne:</span><span class="sxs-lookup"><span data-stu-id="09d17-111">If you want tooskip collection of static files for your Django application:</span></span>

    \.skipDjango 

<span data-ttu-id="09d17-112">Pontosabban a központi telepítés hello következő fájlok létrehozásával felülírhatja hello alapértelmezett telepítési parancsfájl:</span><span class="sxs-lookup"><span data-stu-id="09d17-112">For more control over deployment, you can override hello default deployment script by creating hello following files:</span></span>

    \.deployment
    \deploy.cmd

<span data-ttu-id="09d17-113">Használhatja a hello [Azure parancssori felület] [ Azure command-line interface] toocreate hello fájlokat.</span><span class="sxs-lookup"><span data-stu-id="09d17-113">You can use hello [Azure command-line interface][Azure command-line interface] toocreate hello files.</span></span>  <span data-ttu-id="09d17-114">Futtassa a következő parancsot a projektmappából:</span><span class="sxs-lookup"><span data-stu-id="09d17-114">Use this command from your project folder:</span></span>

    azure site deploymentscript --python

<span data-ttu-id="09d17-115">Ha ezek a fájlok nem léteznek, az Azure létrehoz, majd futtat egy ideiglenes üzembe helyezési parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="09d17-115">When these files don't exist, Azure creates a temporary deployment script and runs it.</span></span>  <span data-ttu-id="09d17-116">Azonos toohello egy hoz létre a fenti hello paranccsal is.</span><span class="sxs-lookup"><span data-stu-id="09d17-116">It is identical toohello one you create with hello command above.</span></span>

[Azure command-line interface]: http://azure.microsoft.com/downloads/
