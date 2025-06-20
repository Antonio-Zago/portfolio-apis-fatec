## Projeto V - DataViz 

# DataViz - Sistema de análise de dados de recrutamento e seleção. Tem como objetivo oferecer insights valiosos

### Parceiro Acadêmico
	
<br/>

<img src="https://d15k2d11r6t6rl.cloudfront.net/public/users/Integrators/7ba73aaa-3da9-4cf1-abf2-ccc85dea5875/uid_4917177/logotipo-pro4tech_Prancheta%201.png" width="200"/>

##### *Figura 01. Logo Pro4Tech Fonte([Pro4Tech](https://pro4tech.com.br/))*

### Visão do Projeto

Neste projeto, foi proposta a implementação de um sistema para otimizar a maneira como os dados de recrutamento são coletados, visualizados e analisados visando centralizar e visualizar dados dispersos, permitir uma tomada de decisão estratégica, gerar relatórios personalizados e automatizar processos manuais, além de possibilitar a integração de dados de diferentes fontes.

O foco principal do projeto é fornecer insights valiosos a partir de métricas de eficiência no recrutamento (ex. tempo médio de contratação, quantidade de contratações por processo seletivo).
Identificação de padrões e tendências para otimizar o processo de seleção.
Personalização de relatórios conforme as necessidades específicas dos gestores.

### Tecnologias adotadas na solução

<div style="text-align: center;">
  <div style="margin-top: 10px; font-weight: bold;">BackEnd</div>
  <div style="display: inline_block">
    <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/java/java-original-wordmark.svg" width="85" height="85" />
    <img src="https://github.com/devicons/devicon/blob/master/icons/spring/spring-original-wordmark.svg" width="85" height="85" />
    <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/apachespark/apachespark-original.svg" width="85" height="85" />
  </div>
</div>
<div style="text-align: center;">
  <div style="margin-top: 10px; font-weight: bold;">FrontEnd</div>
  <div style="display: inline_block">
    <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/vuejs/vuejs-original.svg" width="85" height="85" />
  </div>
</div>
<div style="text-align: center;">
  <div style="margin-top: 10px; font-weight: bold;">Banco de Dados</div>
  <div style="display: inline_block">
    <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/mysql/mysql-original.svg" width="85" height="85" />
  </div>
</div>

## Iniciativas Implementadas


<details ><summary>Informações sobre a modelagem estrela realizada no projeto</summary>
     
  ![image](https://github.com/user-attachments/assets/86838ff8-31cb-44c8-a80c-72fb44585aaa)
   
* Tive uma participação ativa na modelagem do esquema estrela do banco de dados, foi necessário identificar as principais necessidades do cliente referentes aos pontos em que deveriamos analisar, a partir disso definimos as principais questões a serem respodidas a partir da modelagem, são elas: </br>
	* Tempo médio em dias de contratações realizadas em um determinado período  
	* Quantidade de contratações por processo seletivo em determinado período  
	* Quantidade de contratações por participante de RH por período 
	* Quantidade de vagas por participante de RH  
	* Análise da pontuação de candidatos por critério de avaliação  
	* Quantidade de pontos por critério de avaliação para cada candidato por vaga 

</details>  

    
<details ><summary>Informações sobre a implementação de importação dos dados do cliente para o banco de dados</summary>
     
   ```java
		public void Salvar(String arquivo){

		Dataset<Row> dadosPlanilha = spark
				.read()
				.format("csv")
				.option("header", true)
				.option("delimiter", ";")
				.load(arquivo);


		var dadosPlanilhaTratados = dadosPlanilha
		.withColumn("idTempo", functions.row_number().over(Window.orderBy("idProcessoSeletivo")))
        .withColumn("mes", functions.month( functions.to_date(functions.col("datacontratacao"), "dd/MM/yyyy")))
		.withColumn("ano", functions.year( functions.to_date(functions.col("datacontratacao"), "dd/MM/yyyy")))
        .withColumn("semestre", functions.when(
        		functions.month(functions.to_date(functions.col("datacontratacao"), "dd/MM/yyyy")).between(1, 6), 1)
        		.when(functions.month(functions.to_date(functions.col("datacontratacao"), "dd/MM/yyyy")).between(7, 12), 2))
        .withColumn("trimestre", functions.when(
        		functions.month(functions.to_date(functions.col("datacontratacao"), "dd/MM/yyyy")).between(1, 4), 1)
        		.when(functions.month(functions.to_date(functions.col("datacontratacao"), "dd/MM/yyyy")).between(5, 8), 2)
        		.when(functions.month(functions.to_date(functions.col("datacontratacao"), "dd/MM/yyyy")).between(9, 12), 3))
		.withColumn("idProcessoSeletivo", functions.col("idProcessoSeletivo").cast("long"))
        .withColumn("nome", functions.col("nome"))
		.withColumn("status", functions.col("status"))
        .withColumn("descricao", functions.col("descricao"))
		.withColumn("criadoPor",functions.col("criadoPor"))
		.withColumn("tempoMedio", functions.col("idProcessoSeletivo").cast("long"))
		.withColumn("date_diff", functions.datediff(functions.to_date(functions.col("datacontratacao"), "dd/MM/yyyy"), functions.to_date(functions.col("datainiciovaga"), "dd/MM/yyyy")))
		.withColumn("idParticipanteRH", functions.col("idParticipanteRH").cast("long"))
		.withColumn("cargo", functions.col("cargo"))
		.withColumn("idVaga", functions.col("idVaga").cast("long"))
		.withColumn("titulovaga", functions.col("titulovaga"))
		.withColumn("numeroposicoes", functions.col("numeroposicoes").cast("integer"))
		.withColumn("requisitosvagas", functions.col("requisitosvagas"))
		.withColumn("estado", functions.col("estado"))
		.withColumn("dataCriacao", functions.to_date(functions.col("dataCriacao"), "dd/MM/yyyy"))
		.withColumn("inicioProcessoSeletivo", functions.to_date(functions.col("inicioProcessoSeletivo"), "dd/MM/yyyy"))
		.withColumn("fimProcessoSeletivo", functions.to_date(functions.col("fimProcessoSeletivo"), "dd/MM/yyyy"))
		.withColumn("idCandidato", functions.col("idCandidato").cast("long"))
		.withColumn("nomeCandidato", functions.col("nomeCandidato"))
		.withColumn("idCriterio", functions.col("idCriterio").cast("long"))
		.withColumn("nomeCriterio", functions.col("nomeCriterio"))
		.withColumn("pontuacao", functions.col("pontuacao").cast("long"));



		var temposDs = serviceTempo.SalvarDatas(dadosPlanilhaTratados);

		service.SalvarProcessosSeletivos(temposDs);

		serviceParticipantesRH.SalvarParticipantesRH(temposDs);

		serviceVagas.SalvarVagas(temposDs);
		
		serviceCriterios.SalvarCriterios(temposDs);
		
		serviceCandidatos.SalvarCandidatos(temposDs);

		serviceContratacoes.SalvarContratacoes(temposDs);
		
		serviceAvaliacoes.SalvarAvaliacoes(temposDs);

   ```
   
* Salvar(): Nesse método são carregados os dados da planilha através de uma instância do Spark e feito o mapeamento das colunas da planilha com os campos das tabelas no banco de dados utilizando os métodos “withColumn”  do framework. Um ponto importante a ressaltar é relativo a salvar os registros das tabelas de fatos do modelo, para isso foi necessário agrupar os registros das tabelas de dimensão e realizar um tratamento para obter as medidas previstas no modelo estrela.

</details>   

 <details ><summary>Detalhes das rotinas para retorno das métricas</summary>
       
   ```java

@Repository
public interface FatoContratacoesRepository extends JpaRepository<FatoContratacoes, Integer>{

	 	@Query(nativeQuery = true, value =  "SELECT a.processo_seletivo, sum(a.tempo_medio) / count(a.processo_seletivo) tempo_medio, b.nome FROM fato_contratacoes a INNER JOIN dim_processo_seletivo b ON b.id_processo_seletivo = a.processo_seletivo WHERE (b.inicio_processo_seletivo < :fim || :fim is null) and (b.fim_processo_seletivo > :inicio or b.fim_processo_seletivo is null)  GROUP BY a.processo_seletivo")
	    List<ProcessoSeletivoTempoMedioDto> RetornarTempoMedioProcessoSeletivo(@Param("inicio") LocalDateTime inicio, @Param("fim") Optional<LocalDateTime> fim);
	
	
	    @Query(nativeQuery = true, value =  "SELECT a.processo_seletivo, b.nome, sum(a.quantidade) quantidade FROM fato_contratacoes a INNER JOIN dim_processo_seletivo b ON b.id_processo_seletivo = a.processo_seletivo WHERE (b.inicio_processo_seletivo < :fim || :fim is null) and (b.fim_processo_seletivo > :inicio or b.fim_processo_seletivo is null) GROUP BY a.processo_seletivo")
	    List<ProcessoSeletivoQuantidadeDto> RetornarQuantidadeProcessoSeletivo(@Param("inicio") LocalDateTime inicio, @Param("fim") Optional<LocalDateTime> fim);

@Query(nativeQuery = true, value = "SELECT v.titulo_vaga, AVG(f.tempo_medio) " +
       "FROM fato_contratacoes f " +
       "JOIN dim_vaga v ON f.vaga = v.id_vaga " +
       "JOIN dim_tempo t ON f.tempo = t.id_tempo " +
       "WHERE (t.ano > :anoInicial OR (t.ano = :anoInicial AND t.mes >= :mesInicial)) " +
       "AND (t.ano < :anoFinal OR (t.ano = :anoFinal AND t.mes <= :mesFinal)) " +
       "GROUP BY v.titulo_vaga")
    List<Object[]> TempoMedioContratacoesPorVaga(
        @Param("mesInicial") int mesInicial,
        @Param("anoInicial") int anoInicial,
        @Param("mesFinal") int mesFinal,
        @Param("anoFinal") int anoFinal);

}
   ```

Nesse trecho da classe de repositório o método "RetornarTempoMedioProcessoSeletivo" é realizado um select para retornar o tempo médio de contratação por processo seletivo, utilizo uma função de soma do tempo médio e faço o agrupamento por processo seletivo e utilizo um filtro de inicio e fim. </br>
O método "RetornarQuantidadeProcessoSeletivo" retorna o total de contratações realizadas por processo seletivo, e também faço um filtro por data inicio e fim.</br>
O método "TempoMedioContratacoesPorVaga" retorna o tempo médio de contratações para cada vaga aberta no processo seletivo, nesse método é passado os parametros de mês e ano inicial e final

</details> 

 <details ><summary>Detalhes do componente para renderizar o gráfico de tempo médio de contratações</summary>
       
   ```Vue

<template>
    <div>
        <h1 class="titulo">Tempo Médio de Contratação por Processo Seletivo</h1>
        <column-chart :data="chartData" :colors="['#3903fc', '#e88700', '#3903fc']"></column-chart>
        <div class="export-buttons">
            <button @click="exportExcel">XLSX</button>
            <button @click="exportPdf">PDF</button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'TempoMedioDeContratacaoPorProcessoSeletivo',
    data() {
        return {
            chartData: []
        };
    },
    async mounted() {
        await this.fetchData();
    },
    methods: {
        async fetchData() {
            try {
                const response = await fetch(`${import.meta.env.VITE_BASE_API_URL}/fatoContratacoes`);
                if (!response.ok) {
                    throw new Error('Network response was not ok');
                }
                const data = await response.json();
                this.transformData(data);
            } catch (error) {
                console.error('Erro ao buscar dados:', error);
            }
        },
        transformData(data) {
            this.chartData = data.reduce((acc, curr) => {
                acc[curr.nome] = curr.tempo_medio;
                return acc;
            }, {});
            console.log(this.chartData);
        },
        async exportExcel() {
            try {
                const response = await fetch(`${import.meta.env.VITE_BASE_API_URL}/excel/tempo`, {
                    method: 'GET',
                });
                const blob = await response.blob();
                const url = URL.createObjectURL(blob);
                const link = document.createElement('a');
                link.href = url;
                link.download = 'tempo.xlsx';
                link.click();
                URL.revokeObjectURL(url);
            } catch (error) {
                console.error('Erro ao exportar Excel:', error);
            }
        },
        async exportPdf() {
            try {
                const response = await fetch(`${import.meta.env.VITE_BASE_API_URL}/pdf/tempo`, {
                    method: 'GET',
                });
                const blob = await response.blob();
                const url = URL.createObjectURL(blob);
                const link = document.createElement('a');
                link.href = url;
                link.download = 'tempo.pdf';
                link.click();
                URL.revokeObjectURL(url);
            } catch (error) {
                console.error('Erro ao exportar PDF:', error);
            }
        }
    }
};
</script>

   ```

Nesse trecho do componente de gráfico do tempo médio de contratação por processo seletivo é realizado uma requisição para o endpoint e renderizado em um gráfico de colunas utilizando a tag <column-chart>, passando os dados retornados da API

</details> 

 <details ><summary>Detalhes do componente para renderizar a lista de quantidade de contratações por processo seletivo</summary>
       
   ```Vue
<template>
  <div>
    <h1 class="titulo">Quantidade de Contratações por Processo Seletivo</h1>

    <div class="filtros">
      <div class="filtro-item">
        <label for="mesInicial">Mês Inicial:</label>
        <select id="mesInicial" v-model="mesInicial" @change="fetchData">
          <option v-for="mes in meses" :key="mes.numero" :value="mes.numero">
            {{ mes.numero }}
          </option>
        </select>
      </div>

      <div class="filtro-item">
        <label for="anoInicial">Ano Inicial:</label>
        <select id="anoInicial" v-model="anoInicial" @change="fetchData">
          <option v-for="ano in anos" :key="ano.numero" :value="ano.numero">
            {{ ano.numero }}
          </option>
        </select>
      </div>

      <div class="filtro-item">
        <label for="mesFinal">Mês Final:</label>
        <select id="mesFinal" v-model="mesFinal" @change="fetchData">
          <option v-for="mes in meses" :key="mes.numero" :value="mes.numero">
            {{ mes.numero }}
          </option>
        </select>
      </div>

      <div class="filtro-item">
        <label for="anoFinal">Ano Final:</label>
        <select id="anoFinal" v-model="anoFinal" @change="fetchData">
          <option v-for="ano in anos" :key="ano.numero" :value="ano.numero">
            {{ ano.numero }}
          </option>
        </select>
      </div>
    </div>

    <bar-chart :data="chartData" :colors="['#019cbb', '#3a6aaa', '#019cbb']"></bar-chart>

    <div class="export-buttons">
      <button @click="exportExcel">XLSX</button>
      <button @click="exportPdf">PDF</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'QuantidadeDeProcessoSeletivo',
  data() {
    return {
      chartData: [],
      mesInicial: 1,
      anoInicial: 2024,
      mesFinal: 12,
      anoFinal: 2024,
      meses: [
        { numero: 1 },
        { numero: 2 },
        { numero: 3 },
        { numero: 4 },
        { numero: 5 },
        { numero: 6 },
        { numero: 7 },
        { numero: 8 },
        { numero: 9 },
        { numero: 10 },
        { numero: 11 },
        { numero: 12 }
      ],
      anos: [
        { numero: 2023 },
        { numero: 2024 },
        { numero: 2025 }
      ]
    };
  },
  async mounted() {
    await this.fetchData();
  },
  methods: {
    async fetchData() {
      try {
        const dataInicial = `${this.anoInicial}-${this.mesInicial.toString().padStart(2, '0')}-01T00:00:00`;
        const dataFinal = `${this.anoFinal}-${this.mesFinal.toString().padStart(2, '0')}-01T00:00:00`;
        const url = `${import.meta.env.VITE_BASE_API_URL}/fatoContratacoes/quantidade?inicio=${dataInicial}&fim=${dataFinal}`;
        
        const response = await fetch(url);
        if (!response.ok) {
          throw new Error('Network response was not ok');
        }
        const data = await response.json();
        console.log(data);
        this.transformData(data);
      } catch (error) {
        console.error('Erro ao buscar dados:', error);
      }
    },
    transformData(data) {
      this.chartData = data.map(item => [item.nome, item.quantidade]);
    },
    async exportExcel() {
      try {
        const response = await fetch(`${import.meta.env.VITE_BASE_API_URL}/excel/processosSeletivos`, {
          method: 'GET',
        });
        const blob = await response.blob();
        const url = URL.createObjectURL(blob);
        const link = document.createElement('a');
        link.href = url;
        link.download = 'ProcessosSeletivos.xlsx';
        link.click();
        URL.revokeObjectURL(url);
      } catch (error) {
        console.error('Erro ao exportar Excel:', error);
      }
    },
    async exportPdf() {
      try {
        const response = await fetch(`${import.meta.env.VITE_BASE_API_URL}/pdf/processosSeletivos`, {
          method: 'GET',
        });
        const blob = await response.blob();
        const url = URL.createObjectURL(blob);
        const link = document.createElement('a');
        link.href = url;
        link.download = 'ProcessosSeletivos.pdf';
        link.click();
        URL.revokeObjectURL(url);
      } catch (error) {
        console.error('Erro ao exportar PDF:', error);
      }
    }
  }
};
</script>

   ```

Nesse trecho do componente que lista a quatidade de contratações por processo seletivo é realizado uma requisição para o endpoint e renderizado em uma lista o retorno da API

</details> 

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
      <td>★★★★★</td>
	<td>Consigo ensinar outros devs</td>
    </tr>
    <tr>
      <td>Spark</td>
      <td>★★★☆☆</td>
	<td>Sei fazer com ajuda</td>
    </tr>	
    <tr>
      <td>Vue</td>
      <td>★★★★☆</td>
	<td>Entendi</td>
    </tr>
    <tr>
      <td>Mysql</td>
      <td>★★★★★</td>
	<td>Consigo ensinar outros devs</td>
    </tr>
     <tr>
      <td>Conceitos de DevOps</td>
      <td>★★★★☆</td>
	<td>Entendi</td>
    </tr>
     <tr>
      <td>Sql e Spark para ETL</td>
      <td>★★★★☆</td>
	<td>Entendi</td>
    </tr>
  </table>
  
  <h3 align="center">Soft Skills</h3>
  <table align="center">
    <tr>
      <th width="270px">Habilidade</th>
      <th width="280px">Descrição</th>
    </tr>
    <tr>
      <td>Resolução de problemas</td>
      <td>Durante o desenvolvimento, principalmente do ETL com a utilização do Spark foram surgindo algumas dificuldades que tive que superar</td>
    </tr>
    <tr>
  <td>Colaboração em equipe</td>
  <td>Apoiei os demais integrantes da equipe em diversas situações</td>
</tr>
<tr>
  <td>Autonomia e Proatividade</td>
  <td>Assumi a responsabilidade de explorar novas tecnologias, estudar documentações e propor melhorias ao projeto, demonstrando iniciativa e comprometimento com a evolução constante da solução desenvolvida.</td>
</tr>
<tr>
  <td>Organização e gestão do tempo</td>
  <td>Entrega de diversas etapas do projeto (modelagem, backend, frontend, visualizações) em prazos que exigem planejamento e priorização de tarefas.</td>
</tr>
<tr>
  <td>Aprendizado continuo</td>
  <td>Aprendi e apliquei tecnologias novas como Apache Spark e Vue.js, que exigem curva de aprendizado específica.</td>
</tr>
  </table>


 <h2 align="center"> Meus Projetos :books:</h2>
 
   <p align="justify" style="font-family:roboto;"><li><a href="https://github.com/Antonio-Zago/portfolio-apis-fatec/blob/main/API_1.md">1º Semestre: Ibet - Assistente Virtual</a></li></p>
   <p align="justify" style="font-family:roboto;"><li><a href="https://github.com/Antonio-Zago/portfolio-apis-fatec/blob/main/API_2.md">2º Semestre: Vigilant - Sistema de Gerenciamento de Banco de Dados</a></li></p>
   <p align="justify" style="font-family:roboto;"><li><a href="https://github.com/Antonio-Zago/portfolio-apis-fatec/blob/main/API_3.md">3° Semestre: PromoAll - Ecommerce com um motor de regras para promoções aplicadas no momento da compra.</a></li></p>
   <p align="justify" style="font-family:roboto;"><li><a href="https://github.com/Antonio-Zago/portfolio-apis-fatec/blob/main/API_4.md">4° Semestre: BoatHelp - Aplicação Web para sincronização dos dados administrativos, financeiros e operacionais.</a></li></p>
   <p align="justify" style="font-family:roboto;"><li><a href="https://github.com/Antonio-Zago/portfolio-apis-fatec/blob/main/API_5.md">5º Semestre: DataViz - Sistema de análise de dados de recrutamento e seleção. Tem como objetivo oferecer insights valiosos</a></li></p>
   <p align="justify" style="font-family:roboto;"><li><a href="https://github.com/Antonio-Zago/portfolio-apis-fatec/blob/main/API_6.md">6º Semestre: Data Forest - Plataforma web, que permite o cadastro de uma área geográfica</a></li></p>
