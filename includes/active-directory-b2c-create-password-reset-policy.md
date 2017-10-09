<span data-ttu-id="44d85-101">tooenable minden részletre kiterjedő jelszó-változtatási az alkalmazásra, szüksége lesz egy jelszó-visszaállítási házirend toocreate.</span><span class="sxs-lookup"><span data-stu-id="44d85-101">tooenable fine-grained password reset on your application, you will need toocreate a password reset policy.</span></span> <span data-ttu-id="44d85-102">Vegye figyelembe, hogy hello bérlői kiterjedő jelszó-átállítási beállítás van megadva [Itt](../articles/active-directory-b2c/active-directory-b2c-reference-sspr.md).</span><span class="sxs-lookup"><span data-stu-id="44d85-102">Note that hello tenant-wide password reset option specified [here](../articles/active-directory-b2c/active-directory-b2c-reference-sspr.md).</span></span> <span data-ttu-id="44d85-103">Ezzel a házirend-jelszó alaphelyzetbe állítása során hello fogyasztók halad, és alkalmazás hello jogkivonatok hello tartalmát kap hello szolgáltatásokat sikeres befejezését követően ismerteti.</span><span class="sxs-lookup"><span data-stu-id="44d85-103">This policy describes hello experiences that hello consumers will go through during password reset and hello contents of tokens that hello application will receive on successful completion.</span></span>

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

<span data-ttu-id="44d85-104">Hello házirendek beállításainak szakaszában, válassza ki a **jelszó-átállítási házirendek** kattintson **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="44d85-104">In hello policies section of settings, select **Password reset policies** and click **+ Add**.</span></span>

![Jelölje ki a regisztráció vagy bejelentkezés házirendek és hello Hozzáadás gombra.](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-policy.png)

<span data-ttu-id="44d85-106">Adja meg a házirend **neve** a az alkalmazás tooreference.</span><span class="sxs-lookup"><span data-stu-id="44d85-106">Enter a policy **Name** for your application tooreference.</span></span> <span data-ttu-id="44d85-107">Adja meg például a következőt: `SSPR`.</span><span class="sxs-lookup"><span data-stu-id="44d85-107">For example, enter `SSPR`.</span></span>

<span data-ttu-id="44d85-108">Válassza az **Identitásszolgáltatók** lehetőséget, és jelölje be a **Jelszó visszaállítása e-mail-címmel** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="44d85-108">Select **Identity providers** and check **Reset password using email address**.</span></span> <span data-ttu-id="44d85-109">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="44d85-109">Click **OK**.</span></span>

![Válassza ki a jelszó-átállítási identitás-szolgáltatóként e-mail címet használja, és hello OK gombra.](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-identity-providers.png)

<span data-ttu-id="44d85-111">Válassza az **Alkalmazásjogcímek** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="44d85-111">Select **Application claims**.</span></span> <span data-ttu-id="44d85-112">Válassza ki a kívánt visszaadott hello engedélyezési jogkivonatokba jogcímek hátsó tooyour alkalmazás küld, miután a sikeres jelszó-létrehozási élmény.</span><span class="sxs-lookup"><span data-stu-id="44d85-112">Choose claims you want returned in hello authorization tokens sent back tooyour application after a successful password reset experience.</span></span> <span data-ttu-id="44d85-113">Válassza például a **Felhasználó objektumazonosítója** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="44d85-113">For example, select **User's Object ID**.</span></span>

![Válasszon ki néhány alkalmazásjogcímet, majd kattintson az OK gombra.](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-application-claims.png)

<span data-ttu-id="44d85-115">Kattintson a **létrehozása** tooadd hello házirend.</span><span class="sxs-lookup"><span data-stu-id="44d85-115">Click **Create** tooadd hello policy.</span></span> <span data-ttu-id="44d85-116">hello házirend van megadva, **B2C_1_SSPR**.</span><span class="sxs-lookup"><span data-stu-id="44d85-116">hello policy is listed as **B2C_1_SSPR**.</span></span> <span data-ttu-id="44d85-117">Hello **B2C_1_** előtag hozzáfűzött toohello nevét.</span><span class="sxs-lookup"><span data-stu-id="44d85-117">hello **B2C_1_** prefix is appended toohello name.</span></span>

<span data-ttu-id="44d85-118">Nyissa meg a hello házirend kiválasztásával **B2C_1_SSPR**.</span><span class="sxs-lookup"><span data-stu-id="44d85-118">Open hello policy by selecting **B2C_1_SSPR**.</span></span> <span data-ttu-id="44d85-119">Ellenőrizze a megadott hello hello beállításait, majd kattintson az **futtatása most**.</span><span class="sxs-lookup"><span data-stu-id="44d85-119">Verify hello settings specified in hello table then click **Run now**.</span></span>

![Szabályzat kiválasztása és futtatása](media/active-directory-b2c-create-password-reset-policy/run-b2c-password-reset-policy.png)

| <span data-ttu-id="44d85-121">Beállítás</span><span class="sxs-lookup"><span data-stu-id="44d85-121">Setting</span></span>      | <span data-ttu-id="44d85-122">Érték</span><span class="sxs-lookup"><span data-stu-id="44d85-122">Value</span></span>  |
| ------------ | ------ |
| <span data-ttu-id="44d85-123">**Alkalmazások**</span><span class="sxs-lookup"><span data-stu-id="44d85-123">**Applications**</span></span> | <span data-ttu-id="44d85-124">Contoso B2C-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="44d85-124">Contoso B2C app</span></span> |
| <span data-ttu-id="44d85-125">**Válasz URL-cím kiválasztása**</span><span class="sxs-lookup"><span data-stu-id="44d85-125">**Select reply url**</span></span> | `https://localhost:44316/` |

<span data-ttu-id="44d85-126">Egy új böngészőlapon nyílik meg, és ellenőrizheti, hogy hello jelszó alaphelyzetbe állítása az alkalmazás felhasználói élményt.</span><span class="sxs-lookup"><span data-stu-id="44d85-126">A new browser tab opens, and you can verify hello password reset consumer experience in your application.</span></span>

> [!NOTE]
> <span data-ttu-id="44d85-127">Foglalja el tooa perc, a házirend létrehozásához, és frissíti a tootake hatása.</span><span class="sxs-lookup"><span data-stu-id="44d85-127">It takes up tooa minute for policy creation and updates tootake effect.</span></span>
>
