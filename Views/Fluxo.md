Eu vou usar UML para representar as telas. Os atributos de cada classe serão as informações mostradas.

Apenas será definido os atributos da tela atual.

## Nível 0 - Tela de Login

```plantuml

@startuml
!theme carbon-gray

class Login {
  + nome_usuario
  + senha
}

Login --> TelaInicialAdministrador

Login --> TelaInicialGerente

Login --> TelaInicialVendedor

@enduml

```

#### Observações

- Usar email no login?
- Nesse modelo, será necessário adicionar por fora o primeiro `Administrador` no banco de dados.

## Nível 1 - Tela Inicial de Administrador

```plantuml

@startuml
!theme carbon-gray

class TelaInicialAdministrador {
  + Gerenciar Usuarios
  + Logout
}

TelaInicialAdministrador --> TelaGerenciarUsuarios
TelaInicialAdministrador --> Login

@enduml

```

#### Observações

- O `Administrador` poderá ter acesso as lojas?

## Nível 1 - Tela Inicial de Gerente

```plantuml

@startuml
!theme carbon-gray

class TelaInicialGerente {
  + Gerenciar Usuários
  + Gerenciar Produtos
  + Realizar Venda
  + Logout
}

TelaInicialGerente --> TelaGerenciarUsuarios
TelaInicialGerente --> TelaGerenciarProdutos
TelaInicialGerente --> TelaRealizarVenda
TelaInicialGerente --> Login

@enduml

```

## Nível 1 - Tela Inicial de Vendedor

```plantuml

@startuml
!theme carbon-gray

class TelaInicialVendedor {
  + Realizar Venda
  + Logout
}

TelaInicialVendedor --> TelaRealizarVenda
TelaInicialVendedor --> Login

@enduml

```

## Nível 2 - Tela de Gerenciar Usuários

### Para Administradores

```plantuml

@startuml
!theme carbon-gray

class TelaGerenciarUsuarios {
  + Adicionar Usuário
  + Editar Usuário
  + Excluir Usuário
  + Listar Usuários
  + Voltar
}

TelaGerenciarUsuarios --> TelaAdicionarUsuario
TelaGerenciarUsuarios --> TelaEditarUsuario
TelaGerenciarUsuarios --> TelaExcluirUsuario
TelaGerenciarUsuarios --> TelaListarUsuarios
TelaGerenciarUsuarios --> TelaInicialAdministrador

@enduml

```

### Para Gerentes

```plantuml

@startuml
!theme carbon-gray

class TelaGerenciarUsuarios {
  + Adicionar Usuário
  + Editar Usuário
  + Excluir Usuário
  + Listar Usuários
  + Voltar
}

TelaGerenciarUsuarios --> TelaAdicionarUsuario
TelaGerenciarUsuarios --> TelaEditarUsuario
TelaGerenciarUsuarios --> TelaExcluirUsuario
TelaGerenciarUsuarios --> TelaListarUsuarios
TelaGerenciarUsuarios --> TelaInicialGerente

@enduml

```
## Nível 2 - Tela de Gerenciar Produtos

### Para Gerentes

```plantuml

@startuml
!theme carbon-gray

class TelaGerenciarProdutos {
  + Adicionar Produto
  + Editar Produto
  + Excluir Produto
  + Listar Produtos
  + Voltar
}

TelaGerenciarProdutos --> TelaAdicionarProduto
TelaGerenciarProdutos --> TelaEditarProduto
TelaGerenciarProdutos --> TelaExcluirProduto
TelaGerenciarProdutos --> TelaListarProdutos
TelaGerenciarProdutos --> TelaInicialGerente

@enduml

```

## Nível 2 - Tela de Realizar Venda

**Definir fluxo**!

1. Tem uma opção que leva para uma tela listando os produtos? Nessa tela, digitaria o ID do produto e sua quantidade.
2. Digitaria o ID do produto e sua quantidade.

## Nível 3 - Tela de Adicionar Usuário

### Para Administradores

Os `Administradores` podem adicionar qualquer tipo de usuário. 

- Para adicionar `Gerente`, é necessário criar uma loja.
- Para adicionar `Vendedor`, é necessário associar a uma loja existente.

```plantuml

@startuml
!theme carbon-gray

class TelaAdicionarUsuario {
  + Adicionar Administrador
  + Adicionar Gerente
  + Adicionar Vendedor
  + Voltar
}

TelaAdicionarUsuario --> TelaAdicionarAdministrador
TelaAdicionarUsuario --> TelaAdicionarGerente
TelaAdicionarUsuario --> TelaAdicionarVendedor
TelaAdicionarUsuario --> TelaGerenciarUsuarios

@enduml

```

### Para Gerentes

Os `Gerentes` podem adicionar `Gerente` e `Vendedor`.

Na hora da criação, é preciso associar os novos usuários a loja.

```plantuml

@startuml
!theme carbon-gray

class TelaAdicionarUsuario {
  + Adicionar Gerente
  + Adicionar Vendedor
  + Voltar
}

TelaAdicionarUsuario --> TelaAdicionarGerente
TelaAdicionarUsuario --> TelaAdicionarVendedor
TelaAdicionarUsuario --> TelaGerenciarUsuarios

@enduml

```
