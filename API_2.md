## Projeto II - Vigilant

<details>
  
<summary>
	Mais Detalhes do Projeto II
</summary>

# Vigilant - (Sistema de Gerenciamento de Banco de Dados)

### Parceiro Acadêmico
	
<br/>
	
<img src="https://github.com/Antonio-Zago/bertoti/blob/main/metodologia/imgs/necto.png"/>

##### *Figura 01. Logo Necto System Fonte([Necto](https://necto.com.br))*

A empresa Necto System situada no Parque Tecnológico de São José dos Campos, propôs o seguinte desafio baseado na metodologia ágil Scrum.

### Visão do Projeto

   Este projeto propôs a criação de uma integração para coleta de dados diretamente dos servidores, com o objetivo de construir uma série histórica de informações. A ideia por trás dessa abordagem era desenvolver uma aplicação capaz de realizar a coleta regular de métricas de um ou mais Sistemas Gerenciadores de Banco de Dados (SGBDs) remotos. A ferramenta foi projetada para fornecer dados essenciais que ajudem os usuários a tomar decisões mais informadas sobre a manutenção, balanceamento, escalabilidade e possíveis melhorias em seus bancos de dados e na infraestrutura dos servidores.

Com essa integração, o objetivo é permitir que os usuários monitorem o desempenho de seus sistemas em tempo real, identifiquem tendências ao longo do tempo e adotem uma postura proativa para otimizar e manter a estabilidade das operações. Ao fornecer uma visão completa das métricas do sistema, a aplicação possibilita decisões baseadas em dados sobre ajustes necessários, seja para melhorar a eficiência dos bancos de dados, realizar balanceamento de carga ou até mesmo escalar a infraestrutura para atender à demanda crescente.

Essa iniciativa reflete uma compreensão aprofundada das necessidades de gestão de banco de dados e infraestrutura, evidenciando a capacidade de criar soluções práticas que otimizem a operação dos sistemas e garantam sua confiabilidade e desempenho a longo prazo.

Link do repositório do projeto: [Repositório](https://github.com/apibanco/Vigilant)

### Tecnologias adotadas na solução

<div style="text-align: center;">
  <div style="margin-top: 10px; font-weight: bold;">BackEnd</div>
  <div style="display: inline_block">
    <img src="https://github.com/devicons/devicon/blob/master/icons/java/java-original-wordmark.svg" width="85" height="85" />
    <img src="https://github.com/devicons/devicon/blob/master/icons/spring/spring-original-wordmark.svg" width="85" height="85" />
  </div>
</div>
<div style="text-align: center;">
  <div style="margin-top: 10px; font-weight: bold;">FrontEnd</div>
  <div style="display: inline_block">
    <img src="https://github.com/devicons/devicon/blob/master/icons/angularjs/angularjs-original-wordmark.svg" width="85" height="85" />
    <img src="https://github.com/devicons/devicon/blob/master/icons/css3/css3-original-wordmark.svg" width="85" height="85" />  
    <img src="https://github.com/devicons/devicon/blob/master/icons/bootstrap/bootstrap-original-wordmark.svg" width="85" height="85" />
  </div>
</div>
<div style="text-align: center;">
  <div style="margin-top: 10px; font-weight: bold;">Banco de Dados</div>
  <div style="display: inline_block">
    <img src="https://github.com/devicons/devicon/blob/master/icons/postgresql/postgresql-original-wordmark.svg" width="85" height="85" />
  </div>
</div>

## Informações sobre a Lógica do Sistema:

  Assumi a responsabilidade de implementar a lógica que possibilitou a integração com o banco de dados, permitindo a coleta periódica de parametrizações. Além disso, fui encarregado de desenvolver as consultas (queries) necessárias para extrair os dados e criar procedimentos armazenados (procedures) para garantir a execução eficiente dessas operações.

Essa tarefa exigiu um conhecimento aprofundado das estruturas de banco de dados e suas particularidades, além de habilidades no design de consultas, otimização de desempenho e na criação de procedimentos para automatizar processos complexos. Ao implementar essa lógica, consegui garantir que a integração fosse capaz de extrair as informações necessárias de forma precisa e eficiente, assegurando a coleta confiável das parametrizações.

Minha contribuição na criação de consultas e procedures reforçou minha capacidade de traduzir requisitos de negócios em soluções práticas no ambiente de banco de dados. Além disso, evidenciou meu domínio de SQL e meu compromisso em desenvolver soluções robustas e eficazes, atendendo às necessidades tanto do projeto quanto da equipe.

<details open><summary>Informações código Back-End</summary>
    
   1. Algoritmo para conexão com o Banco de Dados (Postgress).
     
   ```js
   
	public conexao(){
			url = "jdbc:postgresql://localhost:5432/teste";
			usuario = "postgres";
			senha = "toto185100";

			try {
				Class.forName("org.postgresql.Driver");
				con = DriverManager.getConnection(url,usuario,senha);
				System.out.println("Conexão realizada com sucesso!!!");
			} catch (Exception e) {
				e.printStackTrace();
			}
			ExibirTamanhoTabelas(con);
	};

	public static void ExibirTamanhoTabelas(Connection con) {
		String sql = "SELECT 
				esquema, 
				tabela,
				pg_size_pretty(pg_relation_size(esq_tab)) AS tamanho,
				pg_size_pretty(pg_total_relation_size(esq_tab)) AS tamanho_total,
			      FROM 
				(SELECT 
				    tablename AS tabela,
				    schemaname AS esquema,
				    schemaname||'.'||tablename AS esq_tab
				FROM
				    pg_catalog.pg_tables
				WHERE 
				    schemaname NOT IN ('pg_catalog', 'information_schema', 'pg_toast') ) AS x
				ORDER BY 
				    pg_total_relation_size(esq_tab) DESC; ";

		try {
			PreparedStatement pesquisa = con.prepareStatement(sql);
			ResultSet result = pesquisa.executeQuery();
			while(result.next()) {
				System.out.println("NOME: " + result.getString("tabela"));
				System.out.println("TAMANHO: "+result.getString("tamanho"));
				System.out.println("TAMANHO TOTAL: " + result.getString("tamanho_total"));
			}
		} catch(Exception e) {
		    e.printStackTrace();
		}
	}
	
   ```
   
   No primeiro trecho deste código acima, foram definidas as informações necessárias para a conexão com o banco de dados local. A variável "url" contém a URL de conexão com o banco, a porta padrão do PostgreSQL e o nome do banco de dados.

Em seguida, dentro de um bloco try-catch, o código tenta estabelecer a conexão com o banco de dados. A linha Class.forName("org.postgresql.Driver") carrega dinamicamente o driver JDBC necessário para se comunicar com o PostgreSQL. Em seguida, DriverManager.getConnection(url,usuario,senha) estabelece a conexão com o banco de dados usando as informações fornecidas. Se a conexão for estabelecida com sucesso, a mensagem "Conexão realizada com sucesso!!!" é exibida. Caso ocorra algum erro durante a conexão, a exceção é capturada e o rastreamento de pilha do erro é impresso.

Após a conexão ser estabelecida, há uma chamadas de método chamando "ExibirTamanhoTabelas". Esse método exibe o tamanho das tabelas do banco de dados através de um retorno de uma query consultando através da conexão realizada.

</details>   

- Auxiliei também a integração completa das chamadas de todos os métodos do Back-End. Durante esse processo, além de criar alguns métodos, desempenhei um papel fundamental na realização de testes para validar as requisições.

  Essa etapa é de extrema importância, pois envolve garantir que cada funcionalidade do Back-End esteja operando de maneira correta e coesa. Ao criar e implementar esses métodos, pude contribuir para a construção de uma aplicação robusta e funcional. Os testes que conduzi permitiram identificar possíveis problemas e assegurar que as requisições feitas à API estivessem fornecendo os resultados esperados.

  A abordagem sistemática e a atenção aos detalhes nos testes ilustram o compromisso em oferecer um produto final de alta qualidade, além de evidenciar minhas habilidades em depuração e solução de problemas.

  <details open><summary>Detalhes da Interface do Usuário</summary>
  
   1. Trecho do algoritmo responsável por receber o retorno do back-end.
     
   ```js
   
        public class Principal {

		public static void main(String[] args) throws IOException {
			LoginModel loginModel = LoginController.PreencherLogin();
			Menu menu = new Menu(loginModel);
			Properties prop = LoginController.getProp();
			String openMenu = prop.getProperty("openMenu");

			if (openMenu.equals("y")) {
				menu.startmenu();
			} else {
				ImprimeMetricas imprimeMetricas = new ImprimeMetricas(loginModel);
				imprimeMetricas.tamanhobancos();
				imprimeMetricas.tamanhoTabelas();
				imprimeMetricas.selectsChamadas1000x();
				imprimeMetricas.SelectMaisDemoradas();
				imprimeMetricas.selectsMaisDemoradasMedia();
				imprimeMetricas.conflicts();
			}
		}
	}
	
   ```

O código é uma classe Java chamada "Principal".
Na primeiro trecho do código, uma instância da classe "LoginModel" é criada chamada "loginModel", e o método estático "PreencherLogin()" da classe "LoginController" é chamado para preencher os dados do login.
Em seguida, uma instância da classe "Menu" chamada "menu" é criada, passando o objeto "loginModel" como argumento para o construtor da classe "Menu".
A próxima linha cria uma instância da classe "Properties" chamada "prop" e chama o método estático "getProp()" da classe "LoginController" para obter um objeto "Properties".
Em seguida, a propriedade chamada "openMenu" é recuperada do objeto "Properties" e armazenada na variável "openMenu" como uma string.
Em seguida, o código verifica se o valor da variável "openMenu" é igual a "y". Se for, o método "startmenu()" é chamado no objeto "menu". Caso contrário, uma instância da classe "ImprimeMetricas" chamada "imprimeMetricas" é criada, passando o objeto "loginModel" como argumento para o construtor. Em seguida, vários métodos são chamados nessa instância, como "tamanhobancos()", "tamanhoTabelas()", "selectsChamadas1000x()", "SelectMaisDemoradas()", "selectsMaisDemoradasMedia()" e "conflicts()". Esses métodos provavelmente realizam diferentes operações relacionadas a métricas e análises de um sistema.

## Conhecimentos Adquiridos

### Aquisição de Conhecimento Profundo
Durante o desenvolvimento do projeto, aproveitei a oportunidade para adquirir um conhecimento aprofundado sobre sistemas de gerenciamento de banco de dados (SGBDs).

### Manipulação Eficiente de Informações
Desenvolvi habilidades para coletar e manipular informações de forma altamente eficiente, criando séries históricas e gerando métricas relevantes para os usuários.

### Aprimoramento de Consultas SQL
A experiência me permitiu aprimorar minha capacidade de criar consultas SQL, utilizando diversos comandos para extrair informações específicas e impactantes de forma eficaz.

### Exploração de Ferramentas de Gerenciamento
Tive a oportunidade de explorar e me familiarizar profundamente com ferramentas de gerenciamento de banco de dados, como o PostgreSQL, aplicando-as de forma excepcionalmente eficaz.

### Coleta de Métricas Cruciais
Criei consultas e rotinas que possibilitaram a coleta de métricas essenciais, como o dimensionamento de tabelas e bancos de dados, proporcionando insights valiosos aos usuários.

### Aprofundamento nos Princípios Fundamentais
Aprofundei minha compreensão dos princípios fundamentais que regem os SGBDs, destacando a importância de estruturar e organizar dados de maneira adequada para facilitar operações futuras.

### Relevância da Otimização
Reconheci a importância de otimizar consultas e operações de banco de dados para garantir um desempenho mais eficiente, resultando em uma experiência geral mais satisfatória para os usuários.

### Atuação Além da Manipulação de Dados
Minha atuação no projeto não se limitou à coleta e manipulação de dados, mas também envolveu a criação de um ambiente de banco de dados resiliente e otimizado.

### Papel Essencial no Sucesso da Aplicação
Esse trabalho teve um papel fundamental no sucesso da aplicação, contribuindo para a eficácia operacional e o bom desempenho da ferramenta.

### Conhecimento Profundo e Base Sólida
A experiência adquirida proporcionou um conhecimento profundo e uma base sólida, preparando-me para futuros projetos relacionados à gestão de dados e ao uso de SGBDs.
<br>

<details close></summary></summary>

Clique [aqui](https://github.com/apibanco/Vigilant) para mais detalhes do prijeto.
  
</details>

<br>

</details>
