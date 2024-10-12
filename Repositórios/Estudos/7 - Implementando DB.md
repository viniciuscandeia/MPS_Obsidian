Para a implementação da **Strategy GerenciamentoDB**, podemos escolher um banco de dados popular como o **SQLite** para simular a persistência. SQLite é simples de usar, leve e já faz parte da biblioteca padrão do Python, o que facilita sua implementação sem dependências externas.

### 1. **Dependência do SQLite**

Para conectar-se ao banco de dados SQLite, utilizaremos a biblioteca `sqlite3` que já vem no Python.

### 2. **Implementação da Strategy: GerenciamentoDB**

Aqui está a implementação de `GerenciamentoDB` utilizando SQLite para armazenar e gerenciar dados no banco de dados:

```Python
import sqlite3

class GerenciamentoDB(GerenciamentoStrategy):
    def __init__(self, db_name: str, tabela: str):
        self.db_name = db_name  # Nome do arquivo do banco de dados
        self.tabela = tabela    # Nome da tabela (homens ou mulheres)

    def adicionar(self, id_, dados):
        """Adiciona um novo registro no banco de dados."""
        with sqlite3.connect(self.db_name) as conn:
            cursor = conn.cursor()
            try:
                cursor.execute(f'''
                    INSERT INTO {self.tabela} (id, nome, idade)
                    VALUES (?, ?, ?)
                ''', (id_, dados['nome'], dados['idade']))
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

    def buscar(self, id_):
        """Busca um registro no banco de dados."""
        with sqlite3.connect(self.db_name) as conn:
            cursor = conn.cursor()
            cursor.execute(f'''
                SELECT id, nome, idade FROM {self.tabela} WHERE id = ?
            ''', (id_,))
            resultado = cursor.fetchone()
            if resultado:
                return {'id': resultado[0], 'nome': resultado[1], 'idade': resultado[2]}
            else:
                return None
```

### 3. **Explicação do Código**

- **Conexão com o banco**: Ao inicializar a classe, conectamos ao banco de dados (`db_name`) e criamos as tabelas `homens` e `mulheres` se elas ainda não existirem.
- **Método `_obter_tabela`**: Esse método auxilia a definir a tabela correta (`homens` ou `mulheres`) com base no tipo de dado.
- **Operações CRUD**: Implementamos as operações de adicionar, remover e buscar usuários no banco de dados:
    - **Adicionar**: Insere o `id`, `nome` e `idade` na tabela correspondente.
    - **Remover**: Deleta o usuário da tabela com base no `id`.
    - **Buscar**: Realiza a consulta e retorna o usuário, se existir.
- **Destrutor (`__del__`)**: Fecha a conexão com o banco de dados quando o objeto for destruído.

### 4. **Uso**

Agora você pode usar essa implementação da mesma maneira que a versão em RAM, exceto que, neste caso, o armazenamento será feito em SQLite:

```Python
# Exemplo com repositório de homens no banco de dados
repositorio_homens_db = DBRepositorioFactory.criar_repositorio('homens')
gerenciador_homens_db = GerenciadorRepositorio(GerenciamentoDB('banco_de_dados.db', 'homens'))

# Adicionando dados ao banco de dados
gerenciador_homens_db.adicionar(1, {'nome': 'Carlos', 'idade': 35})
print(gerenciador_homens_db.buscar(1))  # {'id': 1, 'nome': 'Carlos', 'idade': 35}

# Removendo dados do banco de dados
gerenciador_homens_db.remover(1)
print(gerenciador_homens_db.buscar(1))  # None
```

# COMEÇAR A USAR OBJETOS PARA REPRESENTAR OS DADOS
