<span data-ttu-id="8bf1f-101">tooenable profil szerkesztése az alkalmazásra, szüksége lesz toocreate egy Profilszerkesztési házirend.</span><span class="sxs-lookup"><span data-stu-id="8bf1f-101">tooenable profile editing on your application, you will need toocreate a profile editing policy.</span></span> <span data-ttu-id="8bf1f-102">Ezzel a házirend-profil szerkesztése és hello tartalmát hello alkalmazás sikeres befejezését követően kap jogkivonatok során végighaladnia fogyasztók hello szolgáltatásokat írja le.</span><span class="sxs-lookup"><span data-stu-id="8bf1f-102">This policy describes hello experiences that consumers will go through during profile editing and hello contents of tokens that hello application will receive on successful completion.</span></span>

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

<span data-ttu-id="8bf1f-103">Hello házirendek beállításainak szakaszában, válassza ki a **Profilszerkesztési házirendek** kattintson **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="8bf1f-103">In hello policies section of settings, select **Profile editing policies** and click **+ Add**.</span></span>

![Válassza ki a Profilszerkesztési házirendek és hello Hozzáadás gombra.](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-policy.png)

<span data-ttu-id="8bf1f-105">Adja meg a házirend **neve** a az alkalmazás tooreference.</span><span class="sxs-lookup"><span data-stu-id="8bf1f-105">Enter a policy **Name** for your application tooreference.</span></span> <span data-ttu-id="8bf1f-106">Adja meg például a következőt: `SiPe`.</span><span class="sxs-lookup"><span data-stu-id="8bf1f-106">For example, enter `SiPe`.</span></span>

<span data-ttu-id="8bf1f-107">Válassza az **Identitásszolgáltatók** lehetőséget, és jelölje be a **Bejelentkezés helyi fiókba** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="8bf1f-107">Select **Identity providers** and check **Local Account Signin**.</span></span> <span data-ttu-id="8bf1f-108">Azt is megteheti, hogy közösségi identitásszolgáltatókat választ ki, ha ezek már be vannak állítva.</span><span class="sxs-lookup"><span data-stu-id="8bf1f-108">Optionally, you can also select social identity providers, if already configured.</span></span> <span data-ttu-id="8bf1f-109">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="8bf1f-109">Click **OK**.</span></span>

![Válassza ki a helyi fiókkal bejelentkezik identitás-szolgáltatóként, és hello OK gombra.](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-identity-providers.png)

<span data-ttu-id="8bf1f-111">Válassza a **Profilattribútumok** elemet.</span><span class="sxs-lookup"><span data-stu-id="8bf1f-111">Select **Profile attributes**.</span></span> <span data-ttu-id="8bf1f-112">Válassza ki az attribútumok hello fogyasztói megtekintéséhez és szerkesztéséhez a profilban.</span><span class="sxs-lookup"><span data-stu-id="8bf1f-112">Choose attributes hello consumer can view and edit in their profile.</span></span> <span data-ttu-id="8bf1f-113">Például jelölje be az **Ország/régió**, a **Megjelenítendő név** és az **Irányítószám** attribútumokat.</span><span class="sxs-lookup"><span data-stu-id="8bf1f-113">For example, check **Country/Region**, **Display Name**, and **Postal Code**.</span></span> <span data-ttu-id="8bf1f-114">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="8bf1f-114">Click **OK**.</span></span>

![Egyes attribútumok kiválasztása és hello OK gombra.](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-attributes.png)

<span data-ttu-id="8bf1f-116">Válassza az **Alkalmazásjogcímek** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="8bf1f-116">Select **Application claims**.</span></span> <span data-ttu-id="8bf1f-117">Válassza ki a kívánt visszaadott hello engedélyezési jogkivonatokba jogcímek küldött vissza tooyour alkalmazás sikeres Profilszerkesztési élmény után.</span><span class="sxs-lookup"><span data-stu-id="8bf1f-117">Choose claims you want returned in hello authorization tokens sent back tooyour application after a successful profile editing experience.</span></span> <span data-ttu-id="8bf1f-118">Válassza például a **Megjelenítendő név** és az **Irányítószám** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="8bf1f-118">For example, select **Display Name**, **Postal Code**.</span></span>

![Válasszon ki néhány alkalmazásjogcímet, majd kattintson az OK gombra.](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-application-claims.png)

<span data-ttu-id="8bf1f-120">Kattintson a **létrehozása** tooadd hello házirend.</span><span class="sxs-lookup"><span data-stu-id="8bf1f-120">Click **Create** tooadd hello policy.</span></span> <span data-ttu-id="8bf1f-121">hello házirend van megadva, **B2C_1_SiPe**.</span><span class="sxs-lookup"><span data-stu-id="8bf1f-121">hello policy is listed as **B2C_1_SiPe**.</span></span> <span data-ttu-id="8bf1f-122">Hello **B2C_1_** előtag hozzáfűzött toohello nevét.</span><span class="sxs-lookup"><span data-stu-id="8bf1f-122">hello **B2C_1_** prefix is appended toohello name.</span></span>

<span data-ttu-id="8bf1f-123">Nyissa meg a hello házirend kiválasztásával **B2C_1_SiPe**.</span><span class="sxs-lookup"><span data-stu-id="8bf1f-123">Open hello policy by selecting **B2C_1_SiPe**.</span></span> <span data-ttu-id="8bf1f-124">Ellenőrizze a megadott hello hello beállításait, majd kattintson az **futtatása most**.</span><span class="sxs-lookup"><span data-stu-id="8bf1f-124">Verify hello settings specified in hello table then click **Run now**.</span></span>

![Szabályzat kiválasztása és futtatása](media/active-directory-b2c-create-profile-editing-policy/run-b2c-editing-policy.png)

| <span data-ttu-id="8bf1f-126">Beállítás</span><span class="sxs-lookup"><span data-stu-id="8bf1f-126">Setting</span></span>      | <span data-ttu-id="8bf1f-127">Érték</span><span class="sxs-lookup"><span data-stu-id="8bf1f-127">Value</span></span>  |
| ------------ | ------ |
| <span data-ttu-id="8bf1f-128">**Alkalmazások**</span><span class="sxs-lookup"><span data-stu-id="8bf1f-128">**Applications**</span></span> | <span data-ttu-id="8bf1f-129">Contoso B2C-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="8bf1f-129">Contoso B2C app</span></span> |
| <span data-ttu-id="8bf1f-130">**Válasz URL-cím kiválasztása**</span><span class="sxs-lookup"><span data-stu-id="8bf1f-130">**Select reply url**</span></span> | `https://localhost:44316/` |

<span data-ttu-id="8bf1f-131">Egy új böngészőlapon nyílik meg, és ellenőrizheti, hogy hello Profilszerkesztési végfelhasználói élmény konfigurált módon.</span><span class="sxs-lookup"><span data-stu-id="8bf1f-131">A new browser tab opens, and you can verify hello profile editing consumer experience as configured.</span></span>

> [!NOTE]
> <span data-ttu-id="8bf1f-132">Foglalja el tooa perc, a házirend létrehozásához, és frissíti a tootake hatása.</span><span class="sxs-lookup"><span data-stu-id="8bf1f-132">It takes up tooa minute for policy creation and updates tootake effect.</span></span>
>