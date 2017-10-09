## <a name="set-up-hello-development-environment"></a><span data-ttu-id="e471d-101">Hello fejlesztési környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="e471d-101">Set up hello development environment</span></span>

<span data-ttu-id="e471d-102">Ez a szakasz bemutatja, hogyan állítja be a fejlesztési környezetet, beleértve a hozzáadása egy tartományvezérlőre, a kapcsolódó szolgáltatások-kapcsolat hozzáadása az ASP.NET MVC alkalmazás létrehozása és megadását hello névtér irányelvek szükséges.</span><span class="sxs-lookup"><span data-stu-id="e471d-102">This section walks you setting up your development environment, including creating an ASP.NET MVC app, adding a Connected Services connection, adding a controller, and specifying hello required namespace directives.</span></span>

### <a name="create-an-aspnet-mvc-app-project"></a><span data-ttu-id="e471d-103">ASP.NET MVC alkalmazás projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="e471d-103">Create an ASP.NET MVC app project</span></span>

1. <span data-ttu-id="e471d-104">Nyissa meg a Visual Studiót.</span><span class="sxs-lookup"><span data-stu-id="e471d-104">Open Visual Studio.</span></span>

1. <span data-ttu-id="e471d-105">Válassza ki **fájl -> Új -> projekt** hello főmenüből</span><span class="sxs-lookup"><span data-stu-id="e471d-105">Select **File->New->Project** from hello main menu</span></span>

1. <span data-ttu-id="e471d-106">A hello **új projekt** párbeszédpanelen adja meg a következő ábra a kijelölt hello hello beállításokat:</span><span class="sxs-lookup"><span data-stu-id="e471d-106">On hello **New Project** dialog, specify hello options as highlighted in hello following figure:</span></span>

    ![ASP.NET-projekt létrehozása](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-1.png)

1. <span data-ttu-id="e471d-108">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="e471d-108">Select **OK**.</span></span>

1. <span data-ttu-id="e471d-109">A hello **új ASP.NET projekt** párbeszédpanelen adja meg a következő ábra a kijelölt hello hello beállításokat:</span><span class="sxs-lookup"><span data-stu-id="e471d-109">On hello **New ASP.NET Project** dialog, specify hello options as highlighted in hello following figure:</span></span>

    ![Adja meg az MVC](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-2.png)

1. <span data-ttu-id="e471d-111">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="e471d-111">Select **OK**.</span></span>

### <a name="use-connected-services-tooconnect-tooan-azure-storage-account"></a><span data-ttu-id="e471d-112">Kapcsolódó szolgáltatások tooconnect tooan Azure storage-fiók használata</span><span class="sxs-lookup"><span data-stu-id="e471d-112">Use Connected Services tooconnect tooan Azure storage account</span></span>

1. <span data-ttu-id="e471d-113">A hello **Megoldáskezelőben**, kattintson a jobb gombbal a projekt hello és hello helyi menüből válassza ki a **Hozzáadás -> kapcsolódó szolgáltatási**.</span><span class="sxs-lookup"><span data-stu-id="e471d-113">In hello **Solution Explorer**, right-click hello project, and from hello context menu, select **Add->Connected Service**.</span></span>

1. <span data-ttu-id="e471d-114">A hello **kapcsolódó szolgáltatás hozzáadása** párbeszédablakban válassza **Azure Storage**, majd válassza ki **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="e471d-114">On hello **Add Connected Service** dialog, select **Azure Storage**, and then select **Configure**.</span></span>

    ![Csatlakoztatott Service párbeszédpanelen](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-3.png)

1. <span data-ttu-id="e471d-116">A hello **Azure Storage** párbeszédpanelen válassza ki a kívánt Azure storage-fiók, amellyel toowork szeretne, majd válassza hello **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="e471d-116">On hello **Azure Storage** dialog, select hello desired Azure storage account with which you want toowork, and select **Add**.</span></span>
