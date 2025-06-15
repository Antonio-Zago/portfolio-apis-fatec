## Projeto IV - BoatHelp

<details>
  
<summary>
	Mais Detalhes do Projeto IV
</summary>

# BoatHelp - Sistema de abertura de chamados de suporte com níveis diferentes de acesso

### Parceiro Acadêmico
	
<br/>

![image](https://static.wixstatic.com/media/28f919_850cdd0bc47d4fbd8aa3eeb79db23bf3~mv2.png/v1/fill/w_144,h_50,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/Subiter_NovoLogoCol.png)

##### *Figura 01. Logo Subiter Fonte([Subiter](https://www.subiter.com))*

### Visão do Projeto

Neste projeto, foi proposta a implementação de um sistema integrado para otimizar a gestão, abrangendo o cadastro de usuários, equipamentos e horários. A diversidade de perfis de usuários, como administradores, suporte e clientes, é fundamental para garantir a segurança e a eficiência nas operações.

O foco principal do projeto está no gerenciamento de chamados, que é crucial para melhorar o atendimento. A ausência de um sistema integrado tem causado atrasos e dificuldades na resolução das demandas, afetando negativamente a satisfação dos clientes. A proposta inclui o acompanhamento completo de cada chamado, com o objetivo de não apenas resolver as questões rapidamente, mas também realizar uma análise detalhada de cada interação, gerando insights valiosos para a melhoria contínua dos processos.

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
    <img src="https://github.com/devicons/devicon/blob/master/icons/sqlite/sqlite-original-wordmark.svg" width="85" height="85" />
  </div>
</div>

## Iniciativas Implementadas

 - Tive uma participação ativa na criação do Front end da aplicação com a criação de diversas telas e a integração das mesmas com o back end
    
<details open><summary>Informações sobre a implementação do front end dos chamados dos clientes</summary>
     
   ```html
   
	    <script>

		import Chamado_Cliente from "../services/chamado_cliente";
		import Vue from 'vue'
		import { BootstrapVue } from 'bootstrap-vue'
		import 'bootstrap/dist/css/bootstrap.css'
		import 'bootstrap-vue/dist/bootstrap-vue.css'
		
		Vue.use(BootstrapVue)
		
		export default {
		  name: "ChamadoClienteView",
		  data() {
		    return {
		      chamado_clientes: [],
		      chamado_cliente: {
		        criticidadeChamado: "",
		        dataChamado: "",
		        assuntoChamado:"",
		        descricaoChamado: "",
		        situacaoChamado: "F",
		        solucaoChamado: "",
		      },
		      solucao: ""
		    };
		  },
		  mounted() {
		    this.listar();
		  },
		  methods: {
		    listar() {
		      let token = JSON.parse(localStorage.getItem("authUser")).access_token;
		      Chamado_Cliente.listar(token).then((resposta) => {
		        const resp = resposta.data;
		        const result = resp.filter(resp => resp.usuarioChamado.name === "Victor");
		        this.chamado_clientes = result;
		      });
		    },
		    deletar(id) {
		      Chamado_Cliente.deletar(id).then(() => {
		        this.listar();
		        alert("Deletado com Sucesso");
		      });
		    },
		    finalizar(chamado_cliente) {
		      let token = JSON.parse(localStorage.getItem("authUser")).access_token;
		      this.chamado_cliente.criticidadeChamado = chamado_cliente.criticidadeChamado;
		      this.chamado_cliente.dataChamado = chamado_cliente.dataChamado;
		      this.chamado_cliente.assuntoChamado = chamado_cliente.assuntoChamado;
		      this.chamado_cliente.descricaoChamado = chamado_cliente.descricaoChamado;
		      this.chamado_cliente.solucaoChamado = chamado_cliente.solucaoChamado;
		      Chamado_Cliente.atualizar(this.chamado_cliente, chamado_cliente.id, token).then(()=>{
		          alert('Atualizado com sucesso!');
		          this.limparFormularios();
		          this.listar();
		        })
		    },
		    popularModal(solucao) {
		      this.solucao = solucao;
		    },
		    salvar() {
		      console.log(this.chamado_cliente)
		      Chamado_Cliente.atualizar(this.chamado_cliente).then(() => {
		        alert('Atualizado com sucesso!');
		        this.limparFormularios();
		        this.listar();
		      })
		    },
		    limparFormularios() {
		      this.chamado_cliente.usuarioChamado = "";
		      this.chamado_cliente.criticidadeChamado = "";
		      this.chamado_cliente.descricaoChamado = "";
		      this.chamado_cliente.situacaoChamado = "";
		    }
		  },
		};
		</script>

   ```
   
* listar(): Esse método é responsável por listar os chamados dos clientes. Ele começa recuperando o token de autenticação do usuário, que está armazenado no localStorage (provavelmente após um login). Esse token é passado para o método listar do serviço Chamado_Cliente. Quando a resposta é recebida, o método filtra os chamados para mostrar apenas aqueles onde o nome do usuário associado ao chamado é "Victor". O resultado é armazenado na propriedade chamado_clientes, que provavelmente é usada para exibir esses dados na interface do usuário.

* deletar(id): O método deletar recebe o id de um chamado que deve ser excluído. Ele chama o método deletar do serviço Chamado_Cliente passando o id do chamado a ser deletado. Após a exclusão ser realizada com sucesso, o método listar() é chamado novamente para atualizar a lista de chamados e um alerta é exibido para o usuário informando que a operação foi bem-sucedida.

* finalizar(chamado_cliente): O método finalizar é utilizado para atualizar um chamado já existente. Ele começa pegando o token de autenticação do usuário, similar ao que é feito no método listar(). Em seguida, os dados do chamado que precisam ser atualizados são atribuídos à propriedade chamado_cliente. O método atualizar do serviço Chamado_Cliente é chamado, passando o chamado_cliente atualizado e o id do chamado, além do token. Após a atualização ser concluída com sucesso, um alerta é exibido e a função listar() é chamada para refletir as mudanças na interface. Além disso, a função limparFormularios() é chamada, provavelmente para limpar os campos de entrada do formulário após a operação.

* popularModal(solucao): Este método recebe uma solução como argumento e a atribui à propriedade solucao. O nome do método sugere que ele popula um modal (provavelmente uma janela de diálogo na interface do usuário) com a solução de um chamado, permitindo que o usuário visualize ou edite essa informação.

* salvar(): O método salvar parece estar relacionado à atualização de um chamado com novas informações. Ele primeiro registra no console o objeto chamado_cliente atual, que provavelmente contém as mudanças feitas pelo usuário. Em seguida, o método atualizar do serviço Chamado_Cliente é chamado para atualizar o chamado no banco de dados. Após a atualização ser concluída com sucesso, um alerta é exibido e o método listar() é chamado para atualizar a lista de chamados na interface. Como no método finalizar, a função limparFormularios() é chamada para limpar os campos do formulário após a operação.

</details>   


 <details open><summary>Detalhes da tela de cadastro de novos chamados</summary>
       
   ```html
   
        <script>

		import Chamado from "../services/chamado";
		
		export default {
		  name: "CadastroUsuarioView",
		
		  data() {
		    return {
		      tiposChamado: [],
		      mostrar: false,
		      nome: "Victor",
		      data: "",
		      endereco: "",
		      chamado: {
		        usuarioChamado: {
		          id: 1
		        },
		        tipoChamado: {
		          id: 1
		        },
		        assuntoChamado: "",
		        descricaoChamado: "",
		        criticidadeChamado: "",
		        situacaoChamado: "A",
		        solucaoChamado: "Ainda não possui solução"
		      }
		    };
		  },
		
		  mounted() {
		    this.listarTiposChamado();
		  },
		
		  methods: {
		    salvar(){
		      let token = JSON.parse(localStorage.getItem("authUser")).access_token;
		
		      Chamado.salvar(this.chamado, token).then(() => {
		        alert('Salvo com sucesso');
		        this.limparFormularios();
		      });
		    },
		
		    mostrarAgendamento(){
		      if(this.chamado.tipoChamado.id === 1 || this.chamado.tipoChamado.id === 2){
		        this.mostrar = true
		      }else{
		        this.mostrar = false
		      }
		    },
		
		    listarTiposChamado(){
		      Chamado.listarTipoServico().then((resp) => {
		        this.tiposChamado = resp.data;
		      })
		    },
		
		    limparFormularios() {
		      this.chamado.descricaoChamado = "";
		      this.chamado.assuntoChamado = "";
		      this.data = "";
		      this.endereco = "";
		      this.chamado.criticidadeChamado = "";
		      this.chamado.tipoChamado = "";
		    }
		    
		  }
		};
		</script>
         
   ```
Esse código é um componente Vue.js chamado CadastroUsuarioView, responsável por gerenciar o cadastro de chamados de usuários. O componente tem um conjunto de variáveis no objeto data(), que são utilizadas para armazenar as informações inseridas no formulário de cadastro, como o nome do usuário, a data, o endereço e os dados do chamado, incluindo o tipo, assunto, descrição, criticidade, situação e solução.

Ao ser montado, o componente chama o método listarTiposChamado(), que faz uma requisição ao serviço Chamado para recuperar os tipos de chamados disponíveis. Esses tipos são então armazenados na variável tiposChamado, provavelmente usada para preencher um campo de seleção na interface. O componente também oferece uma lógica para controlar a exibição de um formulário de agendamento. Se o tipo de chamado selecionado for um dos tipos específicos (id 1 ou 2), o campo de agendamento será mostrado; caso contrário, ele é escondido.

Quando o usuário clica no botão de salvar, o método salvar() é chamado. Esse método recupera o token de autenticação do usuário do localStorage, e com ele, chama o serviço Chamado.salvar() para salvar os dados do chamado. Após o chamado ser salvo com sucesso, o componente exibe um alerta de confirmação e limpa os campos do formulário através do método limparFormularios().

Além disso, o componente mantém a função de limpar o formulário, que reseta todos os campos de entrada de dados para seus valores iniciais ou vazios, o que é útil após a criação de um chamado ou quando o usuário decide reiniciar o processo de cadastro.

No geral, esse código cria uma interface de cadastro de chamados interativa, que lida com a coleta de dados do usuário, interage com a API backend para salvar os chamados e exibe os resultados de forma condicional, com base nos tipos de chamados selecionados.

</details> 

## Conhecimentos Adquiridos

#### Aprendizado do VueJs:
    Adquiri habilidades no uso do VueJs, explorando suas funcionalidades e sintaxe.
#### Consulta à Documentação Oficial:
    Compreendi a importância de consultar a documentação oficial do VueJs para obter informações detalhadas e precisas sobre a tecnologia.
#### Estudo Aprofundado:
    Reconheci a necessidade de dedicar tempo a um estudo aprofundado para construir uma base sólida e confiável de conhecimento em VueJs.
#### Noções Básicas vs. Complexidades:
    Percebi que, embora os tutoriais sejam úteis para noções básicas, o estudo da documentação permitiu a compreensão das complexidades da tecnologia.
#### Exploração de Recursos Avançados:
    Aprofundei meu conhecimento explorando recursos avançados do VueJs, além do que é geralmente abordado em tutoriais introdutórios.
#### Constante Busca por Novos Aprendizados:
    Reforcei a importância de estar constantemente em busca de novos aprendizados para acompanhar as evoluções tecnológicas.
#### Atualização sobre Tendências do Mercado:
    Compreendi a necessidade de manter-me atualizado sobre as últimas tecnologias e tendências do mercado para permanecer relevante no cenário profissional.
#### Desenvolvimento de Projeto Sofisticado:
    Utilizando o conhecimento adquirido, desenvolvi um projeto mais sofisticado e eficaz, incorporando práticas avançadas do VueJs.
#### Aquisição de Habilidades Valiosas:
    Ao explorar a documentação e desenvolver um projeto mais complexo, adquiri habilidades valiosas que contribuíram significativamente para minha trajetória profissional.

<br>

<details close></summary></summary>

Clique [aqui](https://github.com/Doc-Docker/APISubiter) para mais detalhes do projeto.

</details>

<br>

</details>
