### 1. **Classe Singleton para Repositórios (Homens e Mulheres)**

Essas classes serão responsáveis apenas por armazenar os dados. Elas não terão lógica de negócio, apenas garantirão que existe uma única instância para cada repositório.

```Python
class RepositorioHomens:
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
            cls._repository = {}
        return cls._instance

    def get_repositorio(self):
        return self._repository


class RepositorioMulheres:
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
            cls._repository = {}
        return cls._instance

    def get_repositorio(self):
        return self._repository
```

### 2. **Factory para Criar os Repositórios**

A Factory Method será responsável por criar os repositórios apropriados (homens ou mulheres) sem incluir nenhuma lógica de manipulação dos dados.

```Python
class RepositorioFactory:
    @staticmethod
    def criar_repositorio(tipo: str):
        if tipo == 'homens':
            return RepositorioHomens().get_repositorio()
        elif tipo == 'mulheres':
            return RepositorioMulheres().get_repositorio()
        else:
            raise ValueError("Tipo de repositório desconhecido")
```

#### Implementação da Fábrica com um Dicionário

A fábrica agora utilizará um dicionário que mapeia os tipos de repositório (por exemplo, "homens" e "mulheres") para suas respectivas classes Singleton.

```Python
class RepositorioFactory:
    _repositorios = {
        'homens': RepositorioHomens,
        'mulheres': RepositorioMulheres
    }

    @staticmethod
    def criar_repositorio(tipo: str):
        repositorio_class = RepositorioFactory._repositorios.get(tipo)
        if repositorio_class:
            return repositorio_class().get_repositorio()
        else:
            raise ValueError(f"Tipo de repositório desconhecido: {tipo}")
```

### 3. **Gerenciador de Dados**

Agora, criamos a classe responsável pela manipulação dos dados nos repositórios, mantendo a lógica de negócio separada do repositório em si.

```Python
class GerenciadorRepositorio:
    def __init__(self, repositorio):
        self.repositorio = repositorio

    def adicionar(self, id_, dados):
        if id_ not in self.repositorio:
            self.repositorio[id_] = dados
        else:
            raise ValueError("Usuário já existe")

    def remover(self, id_):
        if id_ in self.repositorio:
            del self.repositorio[id_]
        else:
            raise ValueError("Usuário não encontrado")

    def buscar(self, id_):
        return self.repositorio.get(id_)

```

### 4. **Integração**

Agora você pode usar a **Factory Method** para obter os repositórios e, em seguida, o gerenciador para manipular os dados dentro de cada repositório, sem quebrar o SRP.

#### Exemplo de uso:

```Python
# Usando a Factory para criar os repositórios
repositorio_homens = RepositorioFactory.criar_repositorio('homens')
repositorio_mulheres = RepositorioFactory.criar_repositorio('mulheres')

# Usando o gerenciador genérico para manipular ambos os repositórios
gerenciador_homens = GerenciadorRepositorio(repositorio_homens)
gerenciador_mulheres = GerenciadorRepositorio(repositorio_mulheres)

# Adicionando dados
gerenciador_homens.adicionar(1, {'nome': 'João', 'idade': 30})
gerenciador_mulheres.adicionar(1, {'nome': 'Maria', 'idade': 28})

# Buscando dados
print(gerenciador_homens.buscar(1))  # {'nome': 'João', 'idade': 30}
print(gerenciador_mulheres.buscar(1))  # {'nome': 'Maria', 'idade': 28}
```
