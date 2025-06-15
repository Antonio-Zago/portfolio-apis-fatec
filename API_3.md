## Projeto III - PromoAll

# PromoAll - Motor de promoções em um Ecommerce

 ## Parceiro Acadêmico
MidAll</br>

![image](https://static.wixstatic.com/media/456d95_d8bfdcb4942b46c69950e9616742df4e~mv2.png/v1/fill/w_312,h_248,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/Logo%20MidAll.png)
##### *Figura 01. Fonte(www.midall.com.br)*

A MidAll é uma empresa de serviços e consultoria em TI, com sede no Parque Tecnológico em São José dos Campos. Desenvolve soluções de data driven, transformação digital, agilidade e eficiência e governança e segurança.


### Visão do Projeto

Criação de um motor de promoções em um Ecommerce, onde a criação, edição e exclusão de promoções possam ser feitas de forma ágil e intuitiva. Com requisitos funcionais:
* Interface de cadastro de produtos e promoções;
* Edição de produtos;
* Carrinho de compras;
* Criação de promoções;
* Categoria de promoções;
* Listagem de produtos e promoções.


Dessa forma, foi desenvolvido o PromoAll

<img src ="https://github.com/Doc-Docker/APIMidAll/blob/main/Images/logo2promoall.png" width="200" height="200"/>

##### *Figura 02. Fonte(www.github.com/Doc-Docker/APIMidAll)*

### Tecnologias utilizadas:

<div style="display: inline_block"><br> 
 <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/java/java-original-wordmark.svg" width="100" height="100" />
 <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/microsoftsqlserver/microsoftsqlserver-plain-wordmark.svg"  width="100" height="100" />
 <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/spring/spring-original-wordmark.svg" width="100" height="100" />
 <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/angularjs/angularjs-original-wordmark.svg" width="100" height="100"  />
 <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/css3/css3-original-wordmark.svg" width="100" height="100" />
 <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/bootstrap/bootstrap-original-wordmark.svg" width="100" height="100" />
</div>

</br>

Para o front-end foi utilizado o Angular que é uma plataforma baseada em Typescript, para criação das telas de interação com o cliente, e para realizar as requisições para a API que foi desenvolvida. O Java com o framework Spring foi utilizado para criação da API de backend, com a criação das rotas HTTP, conexão com o banco de dados, tratamento de erros e aplicação das regras de negócio. Como banco de dados, foi utilizado o H2 que é um sistema de gerenciamento de banco de dados relacional em memória
         
         
### Contribuições pessoais:

Desenvolvimento no front-end, utilizando o Angular, dentro desse desenvolvimento tive vários desafios na criação de telas, como:
* Criação da tela home; </br>
   Desenvolvi a tela inicial da aplicação, utilizando o bootstrap para estilização da página, criação do componente home em TypeScript 
* Criação da tela de listagem de produtos</br>
   Desenvolvimento da tela com uma tabela para exibição dos dados adquiridos do banco de dados, criação da classe de services para fazer a conexão com o banco de dados e executar a requisição de getAll de produtos, utilizando api httpClient do angular
  
   <details>
     <summary>Código lista de produtos html</summary>
       
        
        <table class="table table-condensed table-hover">
                <thead>
                    <tr>
                        <th>ID</th>
                        <th>Name</th>
                        <th>Price</th>
                        <th>Description</th>
                        <th></th>
                        <th></th>
                        <th></th>
                    </tr>
                </thead>
                <tbody>
                    <tr *ngFor="let product of products">
                        <td>{{ product.id }}</td>
                        <td>{{ product.name }}</td>
                        <td>{{ product.price }}</td>
                        <td>{{ product.description }}</td>

                        <td>
                            <select class="form-control" type="number" [(ngModel)]="product.quantidade">
                                <option [value]="n" *ngFor="let n of lista" >
                                  <p>{{n}}</p>
                                </option>
                            </select>
                        </td>

                        <td>

                            <button class="btn btn-primary" routerLink="/products-form/{{ product.id }}">
                                <i class="fa fa-edit"></i>
                            </button>

                           

                        </td>

                        <td>
                            <button  class="btn btn-success" (click)="addProduct(product)" >
                                <i class="fa fa-plus"></i>
                            </button>
                        </td>

                        <td>
                            <button  class="btn btn-danger" (click)="preDelete(product)"
                                    data-toggle="modal" data-target="#deleteModal" >
                                <i class="fa fa-trash"></i>
                            </button>
                        </td>
                
                    </tr>
                </tbody>
            </table> 
    </details>
    
    * Tabela feita em html listando através de um ngFor do Angular os produtos que estão na lista de produtos que foram inseridas pela requisição get em produtos
     </br>
    <details >
     <summary>Código da função que faz a requisição na classe service</summary>
          
          
          
          ngOnInit(): void {
             this.service
               .getProducts()
               .subscribe( res => this.products = res )
           }
     
    
            
     </details>
     
     * Chamada do método de services para requisição get em produtos, retornando todos os produtos cadastrados no banco e inserindo em uma lista products


* Criação das telas de cadastro de produtos; </br>
   Desenvolvimento do formulário de cadastro de produtos, criação da classe de services para envio da requisição POST com objeto do tipo produto.
   
   <details>
      <summary>Código da função na classe service para criação e atualização de produtos </summary>
          
          * Método para envio de uma requisição POST para cadastrar produtos
          
          onSubmit(){

           if(this.id){
             this.service.update(this.id, this.product)
             .subscribe( res => {
               this.success = true;
               this.errors = null;
             }
             )
           }
           else{

             this.service
               .insert(this.product)
               .subscribe( res =>{
                 this.success = true;
                 this.errors = null;

               }, errorRes =>{
                 this.success = false;
                 this.errors = errorRes.error.errors

               }

               )
           }


         }
   </details>
   
   * Função que chama a requisição POST de produto, para inserção através do método na classe services
   
* Criação da tela de carrinho de compras;</br>
   Desenvolvimento de uma tela para mostrar todos os produtos selecionados pelo usuário no sistema
   
   <details>
      <summary>Código TS para exibição no carrinho de compras </summary>
          
          
          
          ngOnInit(): void {
            this.products = [];
            this.finalPrice = 0;
            this.product;
            this.discount;
            this.categoria=0;
            this.noDiscount = 0;
            this.lista = [];
            this.teste = [];
            Cart.products.forEach(element => {
              this.product = element;
              this.id  = element.id;
              this.quantidade = element.quantidade;
              this.categoria =  element.id;
              this.products.push(element);

                this.total = this.noDiscount += (element.price  * element.quantidade);

                this.service.getDiscount(this.id, this.quantidade, this.total, this.categoria).subscribe(
                    response =>
                    { const product : Product = new Product();
                      this.discount = response;
                      this.product.discount = this.discount
                      this.finalPrice = this.finalPrice += (element.price * element.quantidade)-(this.discount)
                      console.log("teste", this.categoria)
                    errorResponse => console.log(errorResponse)
                })
            });

          }

   </details>
   
    * Carrinho de compras em html para listagem dos produtos
   
   
* Criação da função no carrinho de compras para aplicação das promoções; </br>
    Desenvolvimento em TypeScript da função para aplicaçação dos descontos e desenvolvimento para visualização em html
    <details>
      <summary>Código html para exibição do valor do carrinho aplicado os descontos  </summary>
            
            
            
            <tr>
                    <th>Total Price:</th>
                    <th>{{ finalPrice }}</th>
                    <th></th>
                    <th></th>
                   </tr>
                   <tr>
                     <th>Price Without Discount:</th>
                     <th>{{ noDiscount }}</th>
                     <th></th>
                     <th></th>
             </tr>
   </details>
   
   * Aplicando descontos no carrinho de compras e armazenando em uma variavel finalPrice 
   
* Criação do botão para adicionar produto no carrinho </br>
    Desenvolvimento do botão para adicionar o produto selecionado pelo cliente no carrinho de compras
    <details>
      <summary>Código html do botão  </summary>
           
           
           
           <button  class="btn btn-success" (click)="addProduct(product)" >
              <i class="fa fa-plus"></i>
           </button>
   </details>
   
   <details>
      <summary>Código TS para adição no carrinho  </summary>
      
           addProduct(product : Product){

             if(product.quantidade != null){ 
               Cart.products.push(product);
             }

             this.ngOnInit();
           }
   </details>
   
   * Adicionando produto na lista que representa o carrinho de compras
   


</br>
Além disso, tive o desafio de criar as requisições http de acordo com que foi desenvolvido no back end, consiliando o objeto json que seria enviado através das requisições POST e PUT, e adaptando no layout os objetos recebidos através da requisição GET, além disso, enviar as informações corretas para a requisição DELETE.

## Conhecimentos Adquiridos


 <h3 align="center"> Hard Skills </h3>
  <table align="center">
    <tr>
      <th width="270px">Tecnologia/Metodologia</th>
      <th width="85px">Nota</th>
      <th width="200px">Classificação</th>
    </tr>
    <tr>
      <td>Java</td>
      <td>★★★★☆</td>
	<td>Entendi</td>
    </tr>
    <tr>
      <td>Angular</td>
      <td>★★★☆☆</td>
	<td>Sei fazer com ajuda</td>
    </tr>	
    <tr>
      <td>Sql Server</td>
      <td>★★★★☆</td>
	<td>Entendi</td>
    </tr>
  </table>
  
  <h3 align="center">Soft Skills</h3>
  <table align="center">
    <tr>
  <td>Resolução de Problemas</td>
  <td>Durante a construção do carrinho de compras e aplicação das promoções, enfrentei diversos obstáculos técnicos, como regras de negócio complexas e requisições HTTP específicas. Isso exigiu criatividade e persistência na busca por soluções eficazes.</td>
</tr>
<tr>
  <td>Trabalho em Equipe</td>
  <td>Apesar do desenvolvimento individual de algumas partes, foi necessário alinhar com colegas e professores sobre regras de negócio e funcionamento da aplicação, garantindo que todas as funcionalidades se integrassem de forma coesa.</td>
</tr>
<tr>
  <td>Aprendizado Contínuo</td>
  <td>Enfrentei desafios com tecnologias novas, como Angular e Spring, e precisei buscar constantemente materiais, tutoriais e documentação para evoluir rapidamente e entregar com qualidade.</td>
</tr>
<tr>
  <td>Comunicação Técnica</td>
  <td>Durante o desenvolvimento, documentei funcionalidades, escrevi descrições para o README e alinhei com a equipe técnica sobre os fluxos de dados. Isso ajudou na compreensão e colaboração com outros envolvidos.</td>
</tr>
<tr>
  <td>Autonomia</td>
  <td>Assumi tarefas importantes do front-end por conta própria, tomando decisões de design e implementação, o que fortaleceu minha autoconfiança e capacidade de execução independente.</td>
</tr>
  </table>
   

  Os conhecimentos adquiridos em aula foram essencias para desenvolvimento desse projeto, aplicamos os conhecimentos aprendidos para seguir os padrões de arquitetura, torná-lo componentizável e seguindo modos de construção comuns aos utilizados no mercado e comunidade. Criação do banco de dados utilizado na aplicação, seguindo o padrão de chaves primaria e estrangeiras nas tabelas, criação do modelo e entidades do banco. Criação do padrão de pastas tanto no frontend como no backend. Aprendizados dos frameworks utilizados, vue e spring.
  

 <h2 align="center"> Meus Projetos :books:</h2>
 
   <p align="justify" style="font-family:roboto;"><li><a href="https://github.com/Antonio-Zago/portfolio-apis-fatec/blob/main/API_1.md">1º Semestre: Ibet - Assistente Virtual</a></li></p>
   <p align="justify" style="font-family:roboto;"><li><a href="https://github.com/Antonio-Zago/portfolio-apis-fatec/blob/main/API_2.md">2º Semestre: Vigilant - Sistema de Gerenciamento de Banco de Dados</a></li></p>
   <p align="justify" style="font-family:roboto;"><li><a href="https://github.com/Antonio-Zago/portfolio-apis-fatec/blob/main/API_3.md">3° Semestre: PromoAll - Ecommerce com um motor de regras para promoções aplicadas no momento da compra.</a></li></p>
   <p align="justify" style="font-family:roboto;"><li><a href="https://github.com/Antonio-Zago/portfolio-apis-fatec/blob/main/API_4.md">4° Semestre: BoatHelp - Aplicação Web para sincronização dos dados administrativos, financeiros e operacionais.</a></li></p>
   <p align="justify" style="font-family:roboto;"><li><a href="https://github.com/Antonio-Zago/portfolio-apis-fatec/blob/main/API_5.md">5º Semestre: DataViz - Sistema de análise de dados de recrutamento e seleção. Tem como objetivo oferecer insights valiosos</a></li></p>
   <p align="justify" style="font-family:roboto;"><li><a href="https://github.com/Antonio-Zago/portfolio-apis-fatec/blob/main/API_6.md">6º Semestre: Data Forest - Plataforma web, que permite o cadastro de uma área geográfica</a></li></p>
