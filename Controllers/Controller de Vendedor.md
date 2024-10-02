### ANTIGO

```Python
class VendedorController:

	def _criar_entidade(self, novo_usuario: Dict) -> None:
		nome: str = novo_usuario["Nome"]
		username:str = novo_usuario['Username']
		email: str = novo_usuario["Email"]
		senha: str = novo_usuario["Senha"]
		_id_loja: str = novo_usuario["Id_loja"]
		
		objeto_vendedor = Vendedor(nome, username, email, senha, _id_loja)
		
		try:
			vendedor_repositorio.registrar_vendedor(objeto_vendedor)
		except (
			UsuarioIntegridadeError,
			UsuarioRegistroError,
			UsuarioErroInesperado,
			UsuarioNaoEncontrado,
		) as erro:
			raise Exception(str(erro)) from None

	def listar(self) -> bool:
		repositorio: List[Vendedor] = vendedor_repositorio.pegar_repositorio()
		if repositorio:
			return True
		return False
```