
### 1. `__init__(self, ...)`

Este é o **método inicializador** de uma classe. Ele é chamado após a criação da instância do objeto e é utilizado para inicializar os atributos da classe com valores específicos. Em resumo, ele configura o objeto após ser criado.

- Não retorna o objeto em si (retorna `None`).
- Responsável por personalizar a criação da instância, ajustando seus atributos.

Exemplo

```Python
class Pessoa:
    def __init__(self, nome, idade):
        self.nome = nome
        self.idade = idade

# Criando um objeto da classe Pessoa
p = Pessoa('João', 30)
print(p.nome)  # João
```

### 2. `__new__(cls, ...)`

O método `__new__` é responsável pela **criação** de uma nova instância de classe. Ele é chamado **antes** do `__init__`. A função principal do `__new__` é alocar a memória para o novo objeto e, normalmente, ele retorna a instância recém-criada. Este método é útil quando você quer controlar o processo de criação do objeto (por exemplo, em padrões de projeto como Singleton).

- É chamado antes de `__init__` e retorna uma nova instância da classe.
- Você geralmente só precisa sobrescrever `__new__` em casos especiais, como ao usar classes imutáveis (por exemplo, `int`, `str` ou `tuple`).

Exemplo:
\
```Python
class MinhaClasse:
    def __new__(cls):
        print("Criando instância")
        return super().__new__(cls)

    def __init__(self):
        print("Inicializando instância")

# Criando o objeto
obj = MinhaClasse()
# Saída:
# Criando instância
# Inicializando instância
```

### 3. `__call__(self, ...)`

O método `__call__` permite que a instância de uma classe se comporte como uma **função**. Quando você define `__call__` em uma classe, você pode "chamar" as instâncias dessa classe como se fossem funções. Esse método é útil quando você deseja que a instância do objeto execute alguma lógica quando chamada diretamente.

- Transforma o objeto em algo "chamável" (como uma função).

Exemplo:

```Python
class Somador:
    def __init__(self, valor):
        self.valor = valor

    def __call__(self, x):
        return self.valor + x

# Criando o objeto
soma = Somador(10)

# Chamando o objeto como uma função
print(soma(5))  # 15
```
