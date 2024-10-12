Precisamos criar uma classe que represente os objetos de dados e ajustar a estratégia `GerenciamentoDB` para que ela converta os objetos ao interagir com o banco de dados. A classe de objeto vai abstrair os dados, e o `GerenciamentoDB` vai lidar com a transformação de objetos em dados persistíveis (e vice-versa) no banco de dados.

### Passos

1. **Criação da Classe de Objeto**: Vamos criar uma classe chamada `Pessoa`, que representará os dados (como nome e idade).
2. **Ajustes no GerenciamentoDB**: Modificar a estratégia `GerenciamentoDB` para trabalhar com instâncias de `Pessoa` ao invés de dicionários.

### 1. Classe de Objeto (Pessoa)

Vamos definir a classe `Pessoa` para representar os dados.

```Python
class Pessoa:
    def __init__(self, id_: int, nome: str, idade: int):
        self.id = id_
        self.nome = nome
        self.idade = idade

    def __repr__(self):
        return f"Pessoa(id={self.id}, nome='{self.nome}', idade={self.idade})"
```

Essa classe representa uma pessoa com três atributos: `id`, `nome` e `idade`. O método `__repr__` facilita a visualização do objeto ao exibi-lo.

### 2. Ajustes no `GerenciamentoDB`

Agora, vamos ajustar o `GerenciamentoDB` para salvar e buscar objetos `Pessoa` no banco de dados. O gerenciamento vai converter o objeto em dados quando for adicionar ao banco e reverter os dados em objeto ao buscar.

#### Implementação do `GerenciamentoDB` Ajustada

```Python
import sqlite3

class GerenciamentoDB(GerenciamentoStrategy):
    def __init__(self, db_name: str, tabela: str):
        self.db_name = db_name  # Nome do arquivo do banco de dados
        self.tabela = tabela    # Nome da tabela (homens ou mulheres)

    def adicionar(self, pessoa: Pessoa):
        """Adiciona um novo registro no banco de dados a partir de um objeto Pessoa."""
        with sqlite3.connect(self.db_name) as conn:
            cursor = conn.cursor()
            try:
                cursor.execute(f'''
                    INSERT INTO {self.tabela} (id, nome, idade)
                    VALUES (?, ?, ?)
                ''', (pessoa.id, pessoa.nome, pessoa.idade))
                conn.commit()
            except sqlite3.IntegrityError:
                raise ValueError("Usuário já existe no banco de dados")

    def remover(self, id_):
        """Remove um registro do banco de dados."""
        with sqlite3.connect(self.db_name) as conn:
            cursor = conn.cursor()
            cursor.execute(f'''
                DELETE FROM {self.tabela} WHERE id = ?
            ''', (id_,))
            if cursor.rowcount == 0:
                raise ValueError("Usuário não encontrado no banco de dados")
            conn.commit()

    def buscar(self, id_) -> Pessoa:
        """Busca um registro no banco de dados e retorna um objeto Pessoa."""
        with sqlite3.connect(self.db_name) as conn:
            cursor = conn.cursor()
            cursor.execute(f'''
                SELECT id, nome, idade FROM {self.tabela} WHERE id = ?
            ''', (id_,))
            resultado = cursor.fetchone()
            if resultado:
                # Retorna um objeto Pessoa criado a partir dos dados do banco
                return Pessoa(id_=resultado[0], nome=resultado[1], idade=resultado[2])
            else:
                return None
```

### Explicação das Alterações

1. **`adicionar`**:
    
    - Agora, o método espera um objeto `Pessoa` como argumento, em vez de um dicionário de dados. Ele extrai os atributos (`id`, `nome`, `idade`) do objeto para inserir no banco de dados.
2. **`buscar`**:
    
    - Ao buscar no banco de dados, o método transforma os dados retornados do banco em uma instância da classe `Pessoa` e retorna o objeto. Se não encontrar um registro, retorna `None`.
3. **`remover`**:
    
    - Este método não foi alterado, pois a operação de remoção ainda se baseia apenas no `id`.

### Exemplo de Uso com Objetos

Aqui está um exemplo de como utilizar o novo sistema com objetos `Pessoa`:

```Python
# Criando repositório e gerenciador para homens no banco de dados
repositorio_homens_db = DBRepositorioFactory.criar_repositorio('homens')
gerenciador_homens_db = GerenciadorRepositorio(GerenciamentoDB('banco_de_dados.db', 'homens'))

# Criando um objeto Pessoa
pessoa1 = Pessoa(id_=1, nome='Carlos', idade=35)

# Adicionando o objeto ao banco de dados
gerenciador_homens_db.adicionar(pessoa1)

# Buscando a pessoa no banco de dados
pessoa_encontrada = gerenciador_homens_db.buscar(1)
print(pessoa_encontrada)  # Pessoa(id=1, nome='Carlos', idade=35)

# Removendo a pessoa do banco de dados
gerenciador_homens_db.remover(1)
print(gerenciador_homens_db.buscar(1))  # None
```
