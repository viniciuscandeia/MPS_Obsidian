### ANTIGO

```Python
class GerenteController:
	def _criar_entidade(self, novo_usuario: Dict) -> None:
		nome: str = novo_usuario["Nome"]
		email: str = novo_usuario["Email"]
		senha: str = novo_usuario["Senha"]
		username: str = novo_usuario["Username"]
		_id_loja: str = novo_usuario["Id_loja"]
		
		objeto_gerente = Gerente(nome, username, email, senha, _id_loja)
		
		try:
			gerente_repositorio.registrar_gerente(objeto_gerente)
		except (
			UsuarioIntegridadeError,
			UsuarioRegistroError,
			UsuarioErroInesperado,
			UsuarioNaoEncontrado,
		) as erro:
			raise Exception(str(erro)) from None

	def listar(self) -> bool:
		repositorio: List[UsuarioBD] = gerente_repositorio.pegar_repositorio()
		if repositorio:
			return True
		return False

```


