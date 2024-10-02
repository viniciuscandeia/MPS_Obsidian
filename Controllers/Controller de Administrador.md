O `Administrador` tem como responsabilidade:
- Gerenciar usuários (de qualquer natureza)
- Gerenciar lojas
- Gerenciar produtos

### ANTIGO

```Python
class AdministradorController:

	def _criar_entidade(self, novo_usuario: Dict) -> None:
		nome: str = novo_usuario["Nome"]
		email: str = novo_usuario["Email"]
		senha: str = novo_usuario["Senha"]
		username: str = novo_usuario['Username']
		
		objeto_adm = Administrador(nome, username, email, senha)
		
		try:
			adm_repositorio.registrar_administrador(objeto_adm)
		except (
			UsuarioIntegridadeError,
			UsuarioRegistroError,
			UsuarioErroInesperado,
			UsuarioNaoEncontrado,
		) as erro:
			raise Exception(str(erro)) from None

	def _editar_entidade(self, administrador_editado: Dict) -> None:
		try:
			adm_repositorio.editar_administrador(administrador_editado )
		except (
			UsuarioIntegridadeError,
			UsuarioRegistroError,
			UsuarioErroInesperado,
			UsuarioNaoEncontrado,
		) as erro:
			raise Exception(str(erro)) from None

	def listar(self) -> bool:
		repositorio: List[UsuarioBD] = adm_repositorio.pegar_repositorio()
		if repositorio:
			return True
		return False

```



### NOVO

```Python

class AdministradorController:

	def adicionar_usuario(tipo: str, dados: dict):
		usuario = UsuarioFactory.criar_usuario(tipo)
		UsuarioRepository.adicionar(usuario, tipo)

	

```
