### 1. **Crie o Singleton para o Gerenciamento do Repositório**

A classe Singleton será responsável apenas por garantir que exista apenas uma instância do repositório na memória, sem se envolver diretamente na lógica de manipulação dos dados.

```Python
class RepositorioManager:
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
            cls._repository = {}  # Repositório inicializado aqui
        return cls._instance

    def get_repository(self):
        return self._repository
```

Aqui, o `RepositorioManager` cuida da criação e manutenção de uma única instância do repositório (pode ser um dicionário, uma lista, ou outro tipo de estrutura em memória).

### 2. **Crie Classes Separadas para Manipular o Repositório**

A lógica de gerenciamento, como adicionar, remover, atualizar ou consultar dados, deve ser encapsulada em **classes separadas**, que recebem o repositório do Singleton e operam sobre ele. Isso mantém o código modular e de fácil manutenção.

#### Exemplo de Manipulador de Usuários:

```Python
class GerenciadorUsuarios:
    def __init__(self, repositorio):
        self.repositorio = repositorio

    def adicionar_usuario(self, user_id, user_data):
        if user_id not in self.repositorio:
            self.repositorio[user_id] = user_data
        else:
            raise ValueError("Usuário já existe")

    def remover_usuario(self, user_id):
        if user_id in self.repositorio:
            del self.repositorio[user_id]
        else:
            raise ValueError("Usuário não encontrado")

    def buscar_usuario(self, user_id):
        return self.repositorio.get(user_id, None)
```

Aqui o `GerenciadorUsuarios` cuida apenas da lógica relacionada aos usuários. Ele recebe o repositório centralizado no Singleton e faz as operações necessárias.

### 3. **Integração**

Você usaria o Singleton `RepositorioManager` para obter o repositório centralizado e, em seguida, criaria as classes específicas para manipular esse repositório.

#### Exemplo de uso:

```Python
# Criando o singleton
singleton_repo_manager = RepositorioManager()

# Obtendo o repositório centralizado
repositorio = singleton_repo_manager.get_repository()

# Criando o gerenciador de usuários
gerenciador_usuarios = GerenciadorUsuarios(repositorio)

# Adicionando usuários
gerenciador_usuarios.adicionar_usuario(1, {'nome': 'João', 'idade': 30})
print(repositorio)  # {1: {'nome': 'João', 'idade': 30}}

# Buscando um usuário
print(gerenciador_usuarios.buscar_usuario(1))  # {'nome': 'João', 'idade': 30}
```
