# Modelando Domínios Ricos com OOP, DDD e CQRS

## Entities
- Corrupção de código
	- Permitir edição de propriedades de objeto apenas na instância por método construtor. Ou por métodos de alteração.
	- Manter os atributos **set** das propriedades privados para que os mesmos não possam ser alterados diretamente.
- Podem herdar uma classe abstrata **Entity** que possuí uma propriedade **Id**

## Value Objects
- São objetos de valor que compõem uma **Entidade**.
- Nunca serão uma entidade.
- Primitive Obsession
	- Evitar a utilização de tipos primitivos.
- São classes que podem ser utilizadas por mais de uma **Entidade**. São classes que formam um dado complexo.
	- Exemplo: Em vez de declarar duas propriedades do tipo **string** nome e sobrenome declara-se uma propriedade do tipo **Nome** que nada mais é que uma classe contendo duas **strings** nome e sobrenome (respeitando as regras de corrupção de código).
- Não herdam da classe abstrata **Entity** (não possuem id).

## Commands
- Objetos de entrada de dados.
- Contém apenas as propriedades necessárias para fazer a transferência dos dados.
- Pode-se usar **VOs**.
- As propriedades não precisam de tratativa de **Corrupção de código**.
	- As propriedades não precisam ter o atributo **set** privado.
- Fail Fast Validations.
	- Fazer a validação no command.
	- Se houver algum erro evita a entrada no domínio e com isso pode diminuir a quantidade de requisições no banco.
	- Pode-se criar uma intrface com método(s) de validação no projeto **Shared** e fazer com que todos os commands implementem essa interface.
- Command Result
	- São comandos de retorno.
	- Toda vez que é chamado um **Handler** é retornado um **Command Result**.
	- Não é retornado o mesmo **Command** de entrada porque os dados de retorno geralmente não são os mesmos.

## Handlers
- Fazem a manipulação dos **Commands**.
	- Exemplo de fluxo de um handler (pode não conter todos os passos ou estar na mesma ordem)
		- Faz a validação dos **Commands** (fail fast validation).
		- Faz validações de **Repositório**. Exemplo: Se os dados já existem no banco.
		- Gera os **VOs**.
		- Gera as **Entidades**.
		- Relaciona os **VOs** com as **Entidades**.
		- Faz validações de **Entidades**.
		- Persiste os dados da **Entidade** no **Repositório**.
		- Retorna o **Command Result**.

## Repository
- Interfaces contendo os métodos necessários para acesso a dados, sendo possível fazer a injeção de dependência evitando a necessidade de uma conexão com banco.

## Queries
- Classes com métodos de consulta de dados em **Repositório**.

## Observações
- Pode-se criar classes abstratas no projeto **Shared** que serão herdadas por todas as classes correspondentes no(s) domínio(s).
	- Podem ser todos os tipos de classe por exemplo a classe **Entity** poderia ser criada no **Shared**.
	- O ideal é criar uma classe **ValueObject** no **Shared** e fazer com que todos os **VOs** do(s) domínio(s) herdem dessa classe para que tudo que possuam em comum seja centralizado apenas na classe abstrata.
