## <a name="prepare-your-intel-nuc"></a><span data-ttu-id="913cf-101">Az Intel NUC előkészítése</span><span class="sxs-lookup"><span data-stu-id="913cf-101">Prepare your Intel NUC</span></span>

<span data-ttu-id="913cf-102">toocomplete hello hardverbeállításokat, kell:</span><span class="sxs-lookup"><span data-stu-id="913cf-102">toocomplete hello hardware setup, you need to:</span></span>

- <span data-ttu-id="913cf-103">Csatlakoztassa az Intel NUC toohello tápegység hello Kit része.</span><span class="sxs-lookup"><span data-stu-id="913cf-103">Connect your Intel NUC toohello power supply included in hello kit.</span></span>
- <span data-ttu-id="913cf-104">Az Intel NUC tooyour hálózati Ethernet kábellel csatlakoztassa.</span><span class="sxs-lookup"><span data-stu-id="913cf-104">Connect your Intel NUC tooyour network using an Ethernet cable.</span></span>

<span data-ttu-id="913cf-105">Ezzel befejezte az Intel NUC átjáróeszköz hello hardver beállítása.</span><span class="sxs-lookup"><span data-stu-id="913cf-105">You have now completed hello hardware setup of your Intel NUC gateway device.</span></span>

### <a name="sign-in-and-access-hello-terminal"></a><span data-ttu-id="913cf-106">Bejelentkezhetnek és elérhetik a hello Terminálszolgáltatások</span><span class="sxs-lookup"><span data-stu-id="913cf-106">Sign in and access hello terminal</span></span>

<span data-ttu-id="913cf-107">Két beállítások tooaccess terminál környezettel rendelkezik az Intel NUC meg:</span><span class="sxs-lookup"><span data-stu-id="913cf-107">You have two options tooaccess a terminal environment on your Intel NUC:</span></span>

- <span data-ttu-id="913cf-108">Ha a billentyűzet és csatlakoztatott tooyour Intel NUC figyelése, hello rendszerhéj közvetlenül végezheti el.</span><span class="sxs-lookup"><span data-stu-id="913cf-108">If you have a keyboard and monitor connected tooyour Intel NUC, you can access hello shell directly.</span></span> <span data-ttu-id="913cf-109">hello alapértelmezett hitelesítő adatok felhasználónév **legfelső szintű** és a jelszó **legfelső szintű**.</span><span class="sxs-lookup"><span data-stu-id="913cf-109">hello default credentials are username **root** and password **root**.</span></span>

- <span data-ttu-id="913cf-110">Az SSH használata a asztali gépen Intel NUC hozzáférés hello rendszerhéjat.</span><span class="sxs-lookup"><span data-stu-id="913cf-110">Access hello shell on your Intel NUC using SSH from your desktop machine.</span></span>

#### <a name="sign-in-with-ssh"></a><span data-ttu-id="913cf-111">SSH bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="913cf-111">Sign in with SSH</span></span>

<span data-ttu-id="913cf-112">SSH be toosign, kell az Intel NUC hello IP-címét.</span><span class="sxs-lookup"><span data-stu-id="913cf-112">toosign in with SSH, you need hello IP address of your Intel NUC.</span></span> <span data-ttu-id="913cf-113">Ha a billentyűzet és csatlakoztatott tooyour Intel NUC figyelni, használja a hello `ifconfig` toofind hello IP-cím parancsot.</span><span class="sxs-lookup"><span data-stu-id="913cf-113">If you have a keyboard and monitor connected tooyour Intel NUC, use hello `ifconfig` command toofind hello IP address.</span></span> <span data-ttu-id="913cf-114">Másik lehetőségként csatlakozás tooyour útválasztó toolist hello címét a hálózaton található eszközöket.</span><span class="sxs-lookup"><span data-stu-id="913cf-114">Alternatively, connect tooyour router toolist hello addresses of devices on your network.</span></span>

<span data-ttu-id="913cf-115">Jelentkezzen be a felhasználónevet **legfelső szintű** és a jelszó **legfelső szintű**.</span><span class="sxs-lookup"><span data-stu-id="913cf-115">Sign in with username **root** and password **root**.</span></span>

#### <a name="optional-share-a-folder-on-your-intel-nuc"></a><span data-ttu-id="913cf-116">Választható lehetőség: Az Intel NUC a mappa megosztása</span><span class="sxs-lookup"><span data-stu-id="913cf-116">Optional: Share a folder on your Intel NUC</span></span>

<span data-ttu-id="913cf-117">Ha szükséges érdemes lehet tooshare egy mappát az Intel NUC a az asztali környezetet.</span><span class="sxs-lookup"><span data-stu-id="913cf-117">Optionally, you may want tooshare a folder on your Intel NUC with your desktop environment.</span></span> <span data-ttu-id="913cf-118">A mappa megosztása lehetővé teszi, hogy Ön toouse az előnyben részesített asztali szövegszerkesztőben (például [Visual Studio Code](https://code.visualstudio.com/) vagy [Sublime Text](http://www.sublimetext.com/)) használata helyett az Intel NUC tooedit fájlok `nano` vagy `vi`.</span><span class="sxs-lookup"><span data-stu-id="913cf-118">Sharing a folder enables you toouse your preferred desktop text editor (such as [Visual Studio Code](https://code.visualstudio.com/) or [Sublime Text](http://www.sublimetext.com/)) tooedit files on your Intel NUC instead of using `nano` or `vi`.</span></span>

<span data-ttu-id="913cf-119">tooshare egy mappát a Windows hello Intel NUC Samba kiszolgáló konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="913cf-119">tooshare a folder with Windows, configure a Samba server on hello Intel NUC.</span></span> <span data-ttu-id="913cf-120">Másik megoldásként használhatja hello SFTP server hello Intel NUC egy SFTP-ügyféllel a asztali számítógépre a.</span><span class="sxs-lookup"><span data-stu-id="913cf-120">Alternatively, use hello SFTP server on hello Intel NUC with an SFTP client on your desktop machine.</span></span>
