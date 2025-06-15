## Projeto I - Assistente Ibet

<details>
  
<summary>
         Mais Detalhes do Projeto I
</summary>

# Ibet - Assistente Virtual

<br>
     
<img src ="https://bkpsitecpsnew.blob.core.windows.net/uploadsitecps/sites/1/2012/05/Logo-Fatec-1200x800-1.jpg" width="200" height="200"/>

##### *Figura 01. Logo Fatec - Profº Jassen Vidal*

   A FATEC (Faculdade de Tecnologia do Estado de São Paulo) é uma instituição pública estadual vinculada à Secretaria de Desenvolvimento Econômico do Estado de São Paulo. Criada com o objetivo de oferecer formação superior tecnológica de qualidade, a FATEC busca atender às demandas do mercado de trabalho e do desenvolvimento regional, oferecendo cursos voltados para a área de tecnologia, inovação e desenvolvimento.

![image](https://github.com/criskurim/CodeYCode/blob/main/Imagens/logo-removebg-preview.png)

##### *Figura 02. Logo do Projeto Ibet Assistente*

### Visão do Projeto

   A assistente virtual Ibet foi criada para oferecer aos usuários uma forma prática e eficiente de acessar informações sobre esportes. Ela disponibiliza uma série de recursos pensados para melhorar a experiência do usuário. Entre as funcionalidades oferecidas pela Ibet estão:

   <strong>Configuração de Alarmes para Jogos:</strong> A assistente permite que os usuários definam alarmes para serem alertados sobre jogos específicos, garantindo que eles fiquem atualizados em tempo real sobre as partidas de seu interesse.

   <strong>Placar em Tempo Real:</strong> A Ibet apresenta placares atualizados ao vivo, permitindo que os usuários acompanhem os resultados das partidas enquanto acontecem, sem qualquer atraso.
   
   <strong>Acesso a Jogos Anteriores:</strong> Além de informações sobre eventos atuais, a Ibet também disponibiliza dados de jogos passados, oferecendo aos usuários a oportunidade de reviver momentos marcantes do esporte.

   <strong>Vídeos e Conteúdo Relacionado:</strong> A ferramenta oferece uma variedade de vídeos e outros conteúdos sobre esportes, como entrevistas, resumos de jogos e análises, para que os usuários possam se aprofundar ainda mais nos acontecimentos esportivos.
   
   <strong>Interação por Comando de Voz:</strong> Um dos maiores diferenciais da Ibet é a possibilidade de interação por meio de comandos de voz. Isso permite que os usuários obtenham todas as informações desejadas sem a necessidade de digitar ou clicar, tornando a experiência mais prática e intuitiva.

Link do repositório do projeto: [Repositório](https://github.com/AndrewAugusto/Ibet_Assistente)

### Tecnologias adotadas no Projeto

<div style="text-align: center;">
  <div style="margin-top: 10px; font-weight: bold;">BackEnd</div>
  <div style="display: inline_block">
    <img src="https://github.com/devicons/devicon/blob/master/icons/python/python-original-wordmark.svg" width="85" height="85" />
  </div>
</div>
<div style="text-align: center;">
  <div style="margin-top: 10px; font-weight: bold;">Banco de Dados</div>
  <div style="display: inline_block">
    <img src="https://github.com/devicons/devicon/blob/master/icons/sqlite/sqlite-original.svg" width="85" height="85" />
  </div>
</div>

## Iniciativas Implementadas:
  Participei ativamente do processo de criação da tela inicial da aplicação, contribuindo para o desenvolvimento de uma interface intuitiva e funcional.
  Durante o processo, trabalhei na definição da organização dos elementos da tela, como botões, menus e informações chave, buscando sempre otimizar o fluxo de navegação e facilitar o acesso às funcionalidades principais. Também estive envolvido na escolha das cores, fontes e outros detalhes visuais, de forma a garantir uma identidade visual coesa e que estivesse alinhada com o propósito da aplicação.
  
<details open><summary>Informações sobre o desenvolvimento da tela de login</summary>
 
   1. Código da tela de login
     
   ```python
   
   class sm(ScreenManager):
    def MudarPagina2(self):
        self.current = 'p2'
        self.nome = self.get_screen('pinicial').ids.nome.text
        self.email = self.get_screen('pinicial').ids.email.text
        self.get_screen('p2').ids.nome2.text = f'Olá {self.nome}, seu email: {self.email}'
        
 
    def MudarPaginaInicial(self):
        self.current = 'pinicial'
class PaginaInicial(Screen):
    pass
class Pagina2(Screen):
    pass
class Tarefa(BoxLayout):
    def __init__(self,text='',**kwargs):
        super().__init__(**kwargs)
        self.ids.nome2.text = text
class teste(App):
    def build(self):
        return sm()
if __name__ == "__main__":
    teste().run()  
	    
   ```
     
   A classe principal, chamada sm, herda de ScreenManager, e é responsável pela troca entre as telas. Dentro dessa classe, há dois métodos principais. O método MudarPagina2 é usado para mudar para a segunda tela, p2. Além de mudar para essa tela, ele também coleta os dados inseridos na Tela Inicial (nome e email), que são então exibidos na Página 2. A linha de código que faz isso é responsável por atualizar um campo de texto específico, exibindo a mensagem "Olá [nome], seu email: [email]". O outro método, MudarPaginaInicial, muda a tela de volta para a Tela Inicial.

<img src="https://raw.githubusercontent.com/AndrewAugusto/Ibet_Assistente/7b919f2042e732cd0c0da1f7be25ed1b62b513f2/Tela%20inicial.gif" width="500" height="300" />

##### *Figura 03. Gif da tela inicial*

</details> 

 <details open><summary>Informações sobre o desenvolvimento do código para exibir agenda de jogos</summary>
  
   1. Trecho do código responsável por chamar o método para exibir a agenda e permitir ao usuário excluir ou inserir novas informações
     
   ```python
   
           
    elif encontrar_comando('agenda', frase):
        bet_pergunta = ("Aqui está sua agenda completa: ")
        print(bet_pergunta)
        sintese_voz(bet_pergunta)
        agenda_c_bd.exibir_agenda()
     while True:
            bet_pergunta = ("O que deseja realizar com a agenda: ")
            print(bet_pergunta)
            sintese_voz(bet_pergunta)
            termo_busca = ouvir_microfone()
            if(encontrar_comando('inserir', termo_busca)):
                bet_pergunta = ("Digite a data do jogo:  ")
                print(bet_pergunta)
                sintese_voz(bet_pergunta)
                data = str(input('Data (dd/mm/aaaa): '))
                    
                bet_pergunta = ("Digite o horario do jogo? ")
                print(bet_pergunta)
                sintese_voz(bet_pergunta)
                horario = str(input('Horario (hh:mm): '))
                bet_pergunta = ("Fale o nome do primeiro time que irá jogar:  ")
                print(bet_pergunta)
                sintese_voz(bet_pergunta)
                time1 = ouvir_microfone()
                bet_pergunta = ("Fale o nome do segundo time que irá jogar:  ")
                print(bet_pergunta)
                sintese_voz(bet_pergunta)
                time2 = ouvir_microfone()
                print(f'{time1} x {time2}')
                print(f'{data} --- {horario}')
                while True:
                    bet_pergunta = ("Deseja inserir esse jogo na agenda? ")
                    print(bet_pergunta)
                    sintese_voz(bet_pergunta)
                    resposta = ouvir_microfone()
                    if(encontrar_comando('sim', resposta)):
                        agenda_c_bd.inserir_na_agenda(time1, time2, data, horario)
                        break
                    elif(encontrar_comando('não', resposta)):
                        break
                    else:
                        continue
                
                break
            elif(encontrar_comando('deletar', termo_busca)):
                bet_pergunta = ("Digite o código do jogo para deletar: ")
                print(bet_pergunta)
                sintese_voz(bet_pergunta)
                cod_deletar = str(input('Código: '))
                agenda_c_bd.deletar_na_agenda(cod_deletar)
                
                break
            else:
                continue
    
   ```

2. Trecho do código responsável por exibir, inserir e deletar da agenda de jogos do usuário
     
   ```python
   

         def exibir_agenda():
             banco = sqlite3.connect('teste_agenda.db')
             cursor = banco.cursor()
             cursor.execute("SELECT time, data from agenda1")
             for i in cursor:
                 print (i)
             banco.close()
         def inserir_na_agenda(time, data):
             try:
                 banco = sqlite3.connect('teste_agenda.db')
                 cursor = banco.cursor()
                 cursor.execute("INSERT INTO agenda1 VALUES('"+time+"', '"+data+"')")
                 banco.commit()
                 banco.close()
                 print("Agenda atualizada")
             except sqlite3.Error as erro:
                 print(f'Erro ao inserir: {erro}')
             print()
         def deletar_na_agenda(time):
             try:
                 banco = sqlite3.connect('teste_agenda.db')
                 cursor = banco.cursor()
                 cursor.execute("DELETE FROM agenda1 WHERE time = '"+time+"'")
                 banco.commit()
                 banco.close()
                 print("Dados apagados com sucesso!")
             except sqlite3.Error as erro:
                 print(f'Erro ao excluir: {erro}')
             print()
         def atualizar_na_agenda(time, data):
             banco = sqlite3.connect('teste_agenda.db')
             cursor = banco.cursor()
             cursor.execute("UPDATE agenda1 SET data = '"+data+"' WHERE time = '"+time+"'")
             banco.commit()
             banco.close()
             print("Dados atualizados com sucesso")
             print()
    
   ```
	
   - Esse trecho de código é responsável por conectar no banco de dados e realizar a consulta, inserção e exclusão de dados da agenda do usuário
</details> 

## Aprendizados Efetivos

<h4><strong>Início com Python:</strong></h4>
<pre>
   Iniciei minha jornada com a linguagem de programação Python, abrindo portas para novas possibilidades.
</pre>

<h4><strong>Compreensão Profunda do Scrum:</strong></h4>
<pre>
   Adquiri uma compreensão profunda da metodologia ágil Scrum, aplicando seus princípios de forma prática.
</pre>

<h4><strong>Adoção do Paradigma Imperativo:</strong></h4>
<pre>
   Optei por adotar o paradigma de programação imperativo para construir meu projeto, utilizando uma abordagem estruturada.
</pre>

<h4><strong>Base Sólida em Lógica de Programação:</strong></h4>
<pre>
   Desenvolvi uma base sólida em lógica de programação, capacitando-me para resolver desafios computacionais de maneira eficaz.
</pre>

<h4><strong>Introdução e Uso de Estruturas de Dados:</strong></h4>
<pre>
   Introduzi e utilizei com sucesso as primeiras estruturas de dados em meu projeto, explorando as capacidades da linguagem Python.
</pre>

<h4><strong>Evolução das Habilidades de Comunicação:</strong></h4>
<pre>
   Minhas habilidades de comunicação estão em constante evolução, contribuindo para uma melhor interação com colegas.
</pre>

<h4><strong>Desenvolvimento Backend com Python:</strong></h4>
<pre>
   Desenvolvimento do backend com Python, criando aplicações robustas.
</pre>

<h4><strong>Criação de APIs:</strong></h4>
<pre>
   Criação de APIs para fornecer serviços e funcionalidades.
</pre>

<h4><strong>Versionamento de Código com Git:</strong></h4>
<pre>
   Domínio do versionamento de código com o uso do Git.
</pre>

<h4><strong>Projetar Arquitetura de Sistemas:</strong></h4>
<pre>
   Capacidade de projetar a arquitetura de sistemas alinhada aos requisitos funcionais e não funcionais.
</pre>

<h4><strong>Desenvolvimento Integrado com Bancos de Dados Relacionais:</strong></h4>
<pre>
   Experiência no desenvolvimento integrado com bancos de dados relacionais.
</pre>

<br>

<details close></summary></summary>

Clique [aqui](https://github.com/AndrewAugusto/Ibet_Assistente) para mais detalhes do Projeto.

</details>

<br>

</details>
