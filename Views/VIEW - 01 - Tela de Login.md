
Consultar [[Views/Fluxo.canvas|Fluxo]] para entender o que terá em cada tela.

Por sua simplicidade, não é necessário usar **Template Method**.
### Classe TelaLoginView

```Python
class TelaLoginView:
	def tela_login(self) -> dict:
		self.mensagem_inicial()
		return {'username': self.pegar_username(), 'senha': self.pegar_senha()}

	def mensagem_inicial(self):
		print('Tela de Login')

	def pegar_username(self) -> str:
		return input('Username: ')

	def pegar_senha(self) -> str:
		return input('Senha   : ')
```
