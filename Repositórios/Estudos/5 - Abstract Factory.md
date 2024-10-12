O objetivo agora é criar uma **interface** comum para as duas formas de persistência (RAM e Banco de Dados), implementar essas duas variantes, e em seguida, criar uma **Abstract Factory** que será responsável por retornar a fábrica adequada, dependendo do tipo de persistência desejado.

### Passo a Passo:

1. **Interface para Repositórios**: Definimos uma interface que será implementada tanto para persistência em memória RAM quanto para persistência em banco de dados.
2. **Implementações para RAM e Banco de Dados**: Cada implementação terá duas tabelas ou repositórios, um para homens e outro para mulheres.
3. **Abstract Factory**: Uma fábrica abstrata que retornará a fábrica apropriada (RAM ou banco de dados) dependendo da escolha do usuário.

### 1. **Repositórios (Apenas para Armazenamento)**

Os repositórios apenas armazenam dados e não têm métodos de manipulação. A lógica de adicionar, remover e buscar será transferida para gerenciadores.

#### Repositório em Memória para Homens:

```Python 
class RepositorioHomensRAM:
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
            cls._repository = {}
        return cls._instance

    def get_repositorio(self):
        return self._repository
```

#### Repositório em Memória para Mulheres:

```Python 
class RepositorioMulheresRAM:
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
            cls._repository = {}
        return cls._instance

    def get_repositorio(self):
        return self._repository
```

#### Repositório no Banco de Dados para Homens:

```Python 
class RepositorioHomensDB:
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
            cls._repository = {}  # Simulação de tabela no banco de dados
        return cls._instance

    def get_repositorio(self):
        return self._repository
```

#### Repositório no Banco de Dados para Mulheres:

```Python 
class RepositorioMulheresDB:
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
            cls._repository = {}  # Simulação de tabela no banco de dados
        return cls._instance

    def get_repositorio(self):
        return self._repository
```

### 2. **Gerenciador de Dados**

O gerenciador será responsável por manipular os dados no repositório, independentemente de ser em memória ou no banco de dados.

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

### 3. **Criando a Abstract Factory**

Agora, vamos criar uma **Abstract Factory** que retorna a fábrica correta (RAM ou banco de dados) com base no tipo de persistência desejado.

#### Fábrica de Fábricas:

```Python
class AbstractFactory:
    _fabricas = {
        'ram': RAMRepositorioFactory,
        'db': DBRepositorioFactory
    }

    @staticmethod
    def criar_fabrica(tipo: str) -> RepositorioFactory:
        fabrica_class = AbstractFactory._fabricas.get(tipo)
        if fabrica_class:
            return fabrica_class()
        else:
            raise ValueError(f"Tipo de fábrica desconhecido: {tipo}")
```

#### **Interface da Factory Method**

Vamos definir a interface da Abstract Factory:

```Python
from abc import ABC, abstractmethod

class RepositorioFactory(ABC):
    @abstractmethod
    def criar_repositorio(self, tipo: str):
        pass
```
#### Fábrica de Repositórios em RAM:

```Python
class RAMRepositorioFactory(RepositorioFactory):
    _repositorios = {
        'homens': RepositorioHomensRAM,
        'mulheres': RepositorioMulheresRAM
    }

    @staticmethod
    def criar_repositorio(tipo: str):
        repositorio_class = RAMRepositorioFactory._repositorios.get(tipo)
        if repositorio_class:
            return repositorio_class().get_repositorio()
        else:
            raise ValueError(f"Tipo de repositório desconhecido: {tipo}")
```

#### Fábrica de Repositórios em Banco de Dados:

```Python
class DBRepositorioFactory(RepositorioFactory):
    _repositorios = {
        'homens': RepositorioHomensDB,
        'mulheres': RepositorioMulheresDB
    }

    @staticmethod
    def criar_repositorio(tipo: str):
        repositorio_class = DBRepositorioFactory._repositorios.get(tipo)
        if repositorio_class:
            return repositorio_class().get_repositorio()
        else:
            raise ValueError(f"Tipo de repositório desconhecido: {tipo}")
```

### 4. **Uso da Abstract Factory**

Aqui está como usar a Abstract Factory para criar repositórios:

```Python
# Criando a fábrica com base no tipo
tipo_fabrica = 'ram'  # Ou 'db' para banco de dados
factory = AbstractFactory.get_factory(tipo_fabrica)

# Criando repositórios usando a fábrica escolhida
repositorio_homens = factory.criar_repositorio('homens')
repositorio_mulheres = factory.criar_repositorio('mulheres')

# Gerenciando os dados
gerenciador_homens = GerenciadorRepositorio(repositorio_homens)
gerenciador_mulheres = GerenciadorRepositorio(repositorio_mulheres)

# Adicionando dados
gerenciador_homens.adicionar(1, {'nome': 'João', 'idade': 30})
gerenciador_mulheres.adicionar(1, {'nome': 'Maria', 'idade': 28})

# Buscando dados
print(gerenciador_homens.buscar(1))  # {'nome': 'João', 'idade': 30}
print(gerenciador_mulheres.buscar(1))  # {'nome': 'Maria', 'idade': 28}
```
