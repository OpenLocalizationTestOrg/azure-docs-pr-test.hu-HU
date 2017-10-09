Üzembe helyezési hitelesítő adatok létrehozása a hello [beállítva az az webalkalmazás üzembe helyező felhasználó](/cli/azure/webapp/deployment/user#set) parancsot.

A központi telepítés felhasználóra szükség az FTP és a helyi Git telepítési tooa webalkalmazás. hello felhasználónév és jelszó fiókszinten. _Ezek nem azonosak az Azure-előfizetés hitelesítő adataival._

Hello a következő parancsban cserélje le  *\<felhasználónév->* és  *\<jelszó >* új felhasználónevet és jelszót. hello felhasználó nevének egyedinek kell lennie. hello jelszó legalább 8 karakter hosszúságúnak kell lennie, és két a következő három elemek hello: betűket, számokat, szimbólumokat. 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

Ha egy ` 'Conflict'. Details: 409` hiba, a módosítás hello felhasználónév. ` 'Bad Request'. Details: 400` hibaüzenet esetén használjon erősebb jelszót.

Ezt az üzembe helyező felhasználót csak egyszer kell létrehoznia, és minden Azure-környezetben használhatja.

> [!NOTE]
> Rekord hello felhasználónevet és jelszót. Őket toodeploy hello webalkalmazás később szüksége.
>
>