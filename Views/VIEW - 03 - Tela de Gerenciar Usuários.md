Consultar [[Fluxo.canvas|Fluxo]] para entender o que terá em cada tela.

Por sua simplicidade, não é necessário usar **Template Method**.

```Python
class Adm:
	def tela_gerenciar_usuarios(self):
		self.mensagem_inicial()
		self.adicionar_usuario()
		self.administrar_usuario()
		self.listar_usuarios()
		self.voltar()
		self.pegar_opcao()

	def mensagem_inicial(self):
		print('Tela de Gerenciar Usuáiros')

	def adicionar_usuario(self):
		print('1 - Adicionar Usuário')

	def administrar_usuario(self):
		print('2 - Administrar Usuário')

	def listar_usuarios(self):
		print('3 - Listar Usuários')

	def voltar(self):
		print('4 - Voltar')

	def pegar_opcao(self):
		return input('>> ')
```

```Python
class Gerente:
	def tela_gerenciar_usuarios(self):
		self.mensagem_inicial()
		self.adicionar_usuario()
		self.administrar_usuario()
		self.listar_usuarios()
		self.voltar()
		self.pegar_opcao()

	def mensagem_inicial(self):
		print('Tela de Gerenciar Usuários')

	def adicionar_usuario(self):
		print('1 - Adicionar Usuário')

	def administrar_usuario(self):
		pass

	def listar_usuarios(self):
		pass

	def voltar(self):
		print('2 - Voltar')

	def pegar_opcao(self):
		return input('>> ')
```