1. Jelentkezzen be Azure előfizetés hello tooyour [az bejelentkezési](/cli/azure/#login) parancsot, és kövesse hello képernyőn megjelenő utasításokat. További információt a bejelentkezésről az [Azure CLI 2.0-s verzió használatának első lépéseit](/cli/azure/get-started-with-azure-cli) ismertető cikkben talál.

  ```azurecli
  az login
  ```
2. Ha több Azure-előfizetéssel rendelkezik, hello fiókhoz hello előfizetések listája

  ```azurecli
  az account list --all
  ```
3. Adja meg, hogy szeretné-e toouse hello előfizetés.

  ```azurecli
  az account set --subscription <replace_with_your_subscription_id>
  ```