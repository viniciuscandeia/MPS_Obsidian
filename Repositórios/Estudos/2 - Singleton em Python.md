O `__new__` é chamado **antes** do `__init__` e é responsável por **criar** a instância do objeto. Isso o torna a escolha mais comum para implementar o Singleton, porque ele permite controlar a **criação** da instância e garantir que apenas uma única instância da classe seja criada.

Como o propósito de um Singleton é garantir que haja apenas uma instância, o `__new__` é perfeito para esse cenário, porque ele intercepta a criação da instância em si.

**Exemplo com `__new__`**:

```Python
class Singleton:
    _instance = None

    def __new__(cls, *args, **kwargs):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance

    def __init__(self, valor):
        self.valor = valor

# Criando duas instâncias
s1 = Singleton(10)
s2 = Singleton(20)

print(s1 is s2)  # True, ambas são a mesma instância
print(s1.valor)  # 20, o valor mais recente sobrescreve o anterior
```

