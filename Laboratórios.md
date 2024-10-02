
## LABORATÓRIO 1 - PROJETO 

0 - Criar o repositório do seu projeto

1 - Criem um diagrama de casos de uso para o sistema de gerenciamento de usuários do seu projeto. Considere pelo menos dois tipos de atores.

2 - Elaborem um diagrama de classe de análise (Fronteira, Entidade e Controle)

4- Criem a divisão de pacotes/pastas para o seu sistema

5 - Implementem a ADIÇÃO DE USUÁRIOS. 

6 - Armazenem os usuários numa coleção e implemente a persistencia numa coleção utilizando memória RAM

7- Implementem a funcionalidade de LISTAR TODOS OS USUÁRIOS

## Laboratório 2 Projeto: Tratamento de erros

1 - Atualizar o diagrama de classes do laboratório iniciado na aula anterior, incluindo as features abaixo (validação dos campos utilizando exceções e uso de 2 mecanismos de persistencia)

2 - Para adição de usuário, implemente as validações de campos utilizando tratamento de erros seguindo as regras abaixo:

Login:

- máximo 12 caracteres
- não pode ser vazio
- não pode ter números 

 Senha:

- Seguir as regras das politicas do AWS IAM:
- https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/id_credentials_passwords_account-policy.html

3 - Permitir o armazenamento de usuários numa coleção também utilizando arquivo binário ou BD (pode ser chaveado com o armazenamento em RAM no início da execução). Implementar tratamento de exceções (por exemplo, IOException ou SQLException).

## LABORATÓRIO PADRÕES 1  
  

1. Implementar o cadastro (CRUD) de uma nova entidade do seu projeto (preferencialmemte tenha relacionamento com Usuario ou com outra Entidadee) e depois crie uma fachada única (Singleton Facade) para os Gerentes (Controller).

## LABORATÓRIO PADRÕES 2 (PROJETO 2)

1. Implemente utilizando Factory Method ou Abstract Factory a comunicação entre as camadas business e infra
2. [v] De acordo com o padrão Adapter, identifique e implemente um cenário de uso no seu projeto
3. [v] Na camada business implemente, utilizando Template Method, a geração de mais um tipo de relatórios (por exemplo, HTML e PDF). Os relatórios devem gerar estatísticas de acesso dos usuários no sistema.
4. Atualizar diagrama de classe com identificação dos padrões de projeto utilizados até o momento no documento de requisitos/projeto