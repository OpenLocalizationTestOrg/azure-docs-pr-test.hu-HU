Az API-alkalmazás hello Azure CLI tooget hello távoli telepítés URL-CÍMÉT használja. Hello a következő parancsban cserélje le  *\<alkalmazás_neve >* a webalkalmazás-névvel.

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

A helyi Git telepítési toobe képes toopush toohello távoli konfigurálása.

```bash
git remote add azure <URI from previous step>
```

Leküldéses toohello Azure távoli toodeploy az alkalmazást. A korábban létrehozott hello központi felhasználói létrehozásakor hello jelszó megadását kéri. Győződjön meg arról, hogy hello jelszó megadását létrehozott hello gyors üzembe helyezés során korábban küldje el, és nem hello jelszó toolog toohello Azure-portált használja.

```bash
git push azure master
```
