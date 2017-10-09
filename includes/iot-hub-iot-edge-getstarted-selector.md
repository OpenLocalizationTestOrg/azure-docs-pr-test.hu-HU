> [!div class="op_single_selector"]
> * [<span data-ttu-id="c5642-101">Linux</span><span class="sxs-lookup"><span data-stu-id="c5642-101">Linux</span></span>](../articles/iot-hub/iot-hub-linux-iot-edge-get-started.md)
> * [<span data-ttu-id="c5642-102">Windows</span><span class="sxs-lookup"><span data-stu-id="c5642-102">Windows</span></span>](../articles/iot-hub/iot-hub-windows-iot-edge-get-started.md)
> 
> 

<span data-ttu-id="c5642-103">Ez a cikk ismerteti a hello részletes bemutatóért [Hello World mintakód] [ lnk-helloworld-sample] tooillustrate hello alapvető összetevői hello [Azure IoT peremhálózati] [ lnk-iot-edge] architektúra.</span><span class="sxs-lookup"><span data-stu-id="c5642-103">This article provides a detailed walkthrough of hello [Hello World sample code][lnk-helloworld-sample] tooillustrate hello fundamental components of hello [Azure IoT Edge][lnk-iot-edge] architecture.</span></span> <span data-ttu-id="c5642-104">hello minta hello Azure IoT peremhálózati toobuild egy egyszerű átjárót használja, amelyre bejelentkezik a "hello world" üzenet tooa fájl, ötpercenként.</span><span class="sxs-lookup"><span data-stu-id="c5642-104">hello sample uses hello Azure IoT Edge toobuild a simple gateway that logs a "hello world" message tooa file every five seconds.</span></span>

<span data-ttu-id="c5642-105">A bemutató tartalma:</span><span class="sxs-lookup"><span data-stu-id="c5642-105">This walkthrough covers:</span></span>

* <span data-ttu-id="c5642-106">**Hello World – mintaarchitektúra**: ismerteti, hogyan [Azure IoT peremhálózati architekturális fogalmak] [ lnk-edge-concepts] toohello Hello World PéldaAlkalmazás, és hogyan hello összetevők működnek együtt a vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="c5642-106">**Hello World sample architecture**: Describes how [Azure IoT Edge architectural concepts][lnk-edge-concepts] apply toohello Hello World sample and how hello components fit together.</span></span>
* <span data-ttu-id="c5642-107">**Hogyan toobuild hello minta**: hello lépéseket szükséges toobuild hello minta.</span><span class="sxs-lookup"><span data-stu-id="c5642-107">**How toobuild hello sample**: hello steps required toobuild hello sample.</span></span>
* <span data-ttu-id="c5642-108">**Hogyan toorun hello minta**: hello lépéseket szükséges toorun hello minta.</span><span class="sxs-lookup"><span data-stu-id="c5642-108">**How toorun hello sample**: hello steps required toorun hello sample.</span></span> 
* <span data-ttu-id="c5642-109">**Tipikus kimeneti**: hello például kimeneti tooexpect hello minta futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="c5642-109">**Typical output**: An example of hello output tooexpect when you run hello sample.</span></span>
* <span data-ttu-id="c5642-110">**Kód kódtöredékek**: hogyan hello Hello World PéldaAlkalmazás megvalósító kódot kódtöredékek tooshow gyűjteménye kulcs IoT peremhálózati átjáró összetevőket.</span><span class="sxs-lookup"><span data-stu-id="c5642-110">**Code snippets**: A collection of code snippets tooshow how hello Hello World sample implements key IoT Edge gateway components.</span></span>


## <a name="hello-world-sample-architecture"></a><span data-ttu-id="c5642-111">Hello World mintaarchitektúra</span><span class="sxs-lookup"><span data-stu-id="c5642-111">Hello World sample architecture</span></span>
<span data-ttu-id="c5642-112">hello Hello World PéldaAlkalmazás hello előző szakaszban leírt hello fogalmak mutatja be.</span><span class="sxs-lookup"><span data-stu-id="c5642-112">hello Hello World sample illustrates hello concepts described in hello previous section.</span></span> <span data-ttu-id="c5642-113">hello Hello World PéldaAlkalmazás valósít meg olyan IoT peremhálózati átjáróval, amely rendelkezik egy folyamat két IoT peremhálózati modulok áll:</span><span class="sxs-lookup"><span data-stu-id="c5642-113">hello Hello World sample implements a IoT Edge gateway that has a pipeline made up of two IoT Edge modules:</span></span>

* <span data-ttu-id="c5642-114">Hello *hello world* modul létrehoz egy üzenetet, ötpercenként, és átadja toohello naplózó modul.</span><span class="sxs-lookup"><span data-stu-id="c5642-114">hello *hello world* module creates a message every five seconds and passes it toohello logger module.</span></span>
* <span data-ttu-id="c5642-115">Hello *naplózó* modul írási műveletek köszönőüzenetei tooa fájl kap.</span><span class="sxs-lookup"><span data-stu-id="c5642-115">hello *logger* module writes hello messages it receives tooa file.</span></span>

![Példa az Azure IoT Edge segítségével összeállított Hello World- architektúrára][4]

<span data-ttu-id="c5642-117">Hello előző szakaszban leírtak a modul nem felel meg a Hello World hello üzenetek közvetlenül toohello naplózó modul öt másodpercenként.</span><span class="sxs-lookup"><span data-stu-id="c5642-117">As described in hello previous section, hello Hello World module does not pass messages directly toohello logger module every five seconds.</span></span> <span data-ttu-id="c5642-118">Ehelyett azt tesz közzé egy üzenet toohello broker öt másodpercenként.</span><span class="sxs-lookup"><span data-stu-id="c5642-118">Instead, it publishes a message toohello broker every five seconds.</span></span>

<span data-ttu-id="c5642-119">hello naplózó modul hello broker hello üzenetet kap, és kezeli rá hello üzenet tooa fájl hello tartalmának írása.</span><span class="sxs-lookup"><span data-stu-id="c5642-119">hello logger module receives hello message from hello broker and acts upon it, writing hello contents of hello message tooa file.</span></span>

<span data-ttu-id="c5642-120">hello naplózó modul csak akkor hello broker származó üzenetek, új üzenetek toohello broker soha nem teszi közzé.</span><span class="sxs-lookup"><span data-stu-id="c5642-120">hello logger module only consumes messages from hello broker, it never publishes new messages toohello broker.</span></span>

![Hogyan hello broker üzenetirányítást végez az Azure IoT Edge modulok között][5]

<span data-ttu-id="c5642-122">hello a fenti ábrán architektúráját mutatja be hello hello Hello World PéldaAlkalmazás és hello relatív elérési toohello forrásfájlok hello különböző részeit hello mintát megvalósító [tárház][lnk-iot-edge].</span><span class="sxs-lookup"><span data-stu-id="c5642-122">hello figure above shows hello architecture of hello Hello World sample and hello relative paths toohello source files that implement different portions of hello sample in hello [repository][lnk-iot-edge].</span></span> <span data-ttu-id="c5642-123">Hello kód saját elképzelései, vagy használjon hello kódrészletek, alá útmutatóként.</span><span class="sxs-lookup"><span data-stu-id="c5642-123">Explore hello code on your own, or use hello code snippets below as a guide.</span></span>

<!-- Images -->
[4]: media/iot-hub-iot-edge-getstarted-selector/high_level_architecture.png
[5]: media/iot-hub-iot-edge-getstarted-selector/detailed_architecture.png

<!-- Links -->
[lnk-helloworld-sample]: https://github.com/Azure/iot-edge/tree/master/samples/hello_world
[lnk-iot-edge]: https://github.com/Azure/iot-edge
[lnk-edge-concepts]: ../articles/iot-hub/iot-hub-iot-edge-overview.md