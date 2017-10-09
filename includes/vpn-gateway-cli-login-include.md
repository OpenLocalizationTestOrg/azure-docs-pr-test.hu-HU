<span data-ttu-id="20495-101">Jelentkezzen be Azure előfizetés hello tooyour [az bejelentkezési](/cli/azure/#login) parancsot, és kövesse hello képernyőn megjelenő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="20495-101">Log in tooyour Azure subscription with hello [az login](/cli/azure/#login) command and follow hello on-screen directions.</span></span> <span data-ttu-id="20495-102">További információt a bejelentkezésről az [Azure CLI 2.0-s verzió használatának első lépéseit](/cli/azure/get-started-with-azure-cli) ismertető cikkben talál.</span><span class="sxs-lookup"><span data-stu-id="20495-102">For more information about logging in, see [Get Started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli).</span></span>

```azurecli
az login
```

<span data-ttu-id="20495-103">Ha több Azure-előfizetéssel rendelkezik, hello fiókhoz hello előfizetések listája</span><span class="sxs-lookup"><span data-stu-id="20495-103">If you have more than one Azure subscription, list hello subscriptions for hello account.</span></span>

```azurecli
az account list --all
```

<span data-ttu-id="20495-104">Adja meg, hogy szeretné-e toouse hello előfizetés.</span><span class="sxs-lookup"><span data-stu-id="20495-104">Specify hello subscription that you want toouse.</span></span>

```azurecli
az account set --subscription <replace_with_your_subscription_id>
```