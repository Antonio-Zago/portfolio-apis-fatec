## Projeto VI - Data Forest

# Data Forest - Plataforma web, que permite o cadastro de uma área geográfica, e a partir dela, fazer um cruzamento com nossa base de dados e nossos modelos de inteligência artificial

### Parceiro Acadêmico
	
<br/>

<img src="https://www.kersys.com.br/wp-content/uploads/2022/01/kersysplus.png" width="200"/>

##### *Figura 01. Logo Kersys Fonte([Kersys](https://www.kersys.com.br))*

### Visão do Projeto

O projeto propõe o desenvolvimento de uma plataforma web voltada à promoção do reflorestamento, atuando como facilitadora em todo o processo — desde a visualização geográfica da área em um mapa, até o planejamento estratégico e o acompanhamento contínuo da recuperação ambiental. </br> </br>
A plataforma permitirá o cadastro de áreas específicas, que serão analisadas com base em uma combinação entre dados geográficos, históricos e climáticos, juntamente com modelos de inteligência artificial desenvolvidos pela equipe. A partir desse cruzamento, serão gerados insights personalizados para cada região, oferecendo estratégias eficazes de reflorestamento e um monitoramento em tempo real que contribui para a minimização de riscos e o aumento da taxa de sucesso das iniciativas.  </br> </br>
Além disso, a solução trará informações detalhadas sobre a saúde florestal da área, identificação de riscos ambientais, a recomendação de espécies vegetais mais adequadas conforme a estação do ano e a meta de longevidade, além da geração de relatórios que possibilitam um acompanhamento histórico de todo o processo. A plataforma será direcionada a produtores rurais, engenheiros ambientais e parceiros estratégicos da empresa, buscando aliar inovação tecnológica à sustentabilidade ambiental.

Link do repositório do projeto: [Repositório](https://github.com/bytelabss/ByteLabs-API6Sem)

### Tecnologias adotadas na solução

<div style="text-align: center;">
  <div style="margin-top: 10px; font-weight: bold;"><b>BackEnd</b> </div>
  <div style="display: inline_block">
    <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/flask/flask-original-wordmark.svg" width="85" height="85" />
    <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/python/python-original.svg" width="85" height="85" />
    <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/pandas/pandas-original-wordmark.svg" width="85" height="85" />
    <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/scikitlearn/scikitlearn-original.svg" width="85" height="85" />	  
  </div>
</div>
<div style="text-align: center;">
  <div style="margin-top: 10px; font-weight: bold;"><b>FrontEnd</b> </div>
  <div style="display: inline_block">
    <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/react/react-original-wordmark.svg" width="85" height="85" />
  </div>
</div>
<div style="text-align: center;">
  <div style="margin-top: 10px; font-weight: bold;"><b>Banco de Dados </b> </div>
  <div style="display: inline_block">
    <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/mongodb/mongodb-original-wordmark.svg" width="85" height="85" />
    <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/postgresql/postgresql-original-wordmark.svg" width="85" height="85" />
  </div>
</div>

## Iniciativas Implementadas


<details><summary>Mapa interativo listando as áreas cadastradas </summary>
     
   ```html
		<MapContainer
            center={pontoInicial}
            zoom={13}
            scrollWheelZoom={true}
            className="map-container"
        >
            <TileLayer
                attribution='&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
                url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
            />

            {areas.map((area) => {
                const coordinates = area.geom.coordinates[0].map(
                    (coord) => new LatLng(coord[0], coord[1])
                );

                return (
                    <Polygon
                        key={area.id}
                        positions={coordinates}
                        pathOptions={blackOptions}
                        eventHandlers={{
                            click: () => handlePolygonClick(area),
                        }}
                    >
                        <Popup
                            closeOnClick={true}
                            autoClose={true}
                            open={selectedAreaId === area.id}
                        >
                            <div>
                                <h3 align="center">{area.name}</h3>
                                <p>
                                    <strong>Descrição:</strong> {area.description}
                                </p>
                                <p>
                                    <strong>Área:</strong> {area.area_in_m2} m²
                                </p>
                                <hr style={{ margin: "10px 0", border: "1px solid #ccc" }} />
                                {classificationResult && (
                                    <div>
                                        <h4 align="center">Classificação</h4>
                                        <p>
                                            <strong>Cluster:</strong> {classificationResult.cluster}
                                        </p>
                                        <p>
                                            <strong>Espécie:</strong>{" "}
                                            {classificationResult.species
                                                ?.toLowerCase()
                                                .replace(/\b\w/g, (char) => char.toUpperCase())}

                                        </p>
                                    </div>
                                )}
                                <hr style={{ margin: "10px 0", border: "1px solid #ccc" }} />
                                {strategyResult && (
                                    <div>
                                        <h4 align="center">Estratégia</h4>
                                        <p>
                                            <strong>Estratégia:</strong>{" "}
                                                {strategyResult.estrategia_prevista
                                                ?.toLowerCase()
                                                .replace(/_/g, " ")
                                                .replace(/\b\w/g, (char) => char.toUpperCase())}

                                        </p>
                                        <p>
                                            <strong>Justificativa:</strong>{" "}
                                            {strategyResult.justificativa}
                                        </p>
                                    </div>
                                )}
                            </div>
                        </Popup>
                    </Polygon>
                );
            })}
        </MapContainer>

   ```
   
* Componente que permite renderizar na tela as áreas cadastradas em um mapa interativo através das coordenadas cadastradas, a biblioteca utilizada para criação do mapa e dos poligono foi o leaftlet

</details>   

    
<details ><summary>Cadastro de novas áreas de reflorestamento para moniitoramento</summary>
     
   ```python
		class ReforestedAreaService:
    def __init__(self):
        self.repository = ReforestedAreaRepository()

    def create_area(self, user_id, name, description, area_in_m2, geom):
        area = ReforestedArea(user_id, name, description, area_in_m2, geom)
        return self.repository.insert(area)

    def get_area_by_id(self, area_id):
        area = self.repository.get_by_id(area_id)
        if not area:
            raise ReforestedAreaNotFoundError()
        return area

    def list_areas(self, offset=0, limit=10):
        return self.repository.list_areas(offset, limit)

    def update_area(self, area: ReforestedArea, **validated_data):
        for attr, value in validated_data.items():
            setattr(area, attr, value)
        area.updated_at = datetime.datetime.utcnow()
        return self.repository.update(area)

    def delete_area(self, area_id):
        area = self.get_area_by_id(area_id)
        return self.repository.delete(area.id)

   ```
   
* Classe service do CRUD de áreas reflorestadas. Para cadastro da área é necessário que na requisição seja passado as informações de user_id, nome, descrição, o tamanho da área e metros quadrados e os pontos de coordenada que delimitam a área

</details>   

 <details ><summary>Listagem dos usuários cadastrados</summary>
       
   ```javascript


const UserList: React.FC = () => {
  const [users, setUsers] = useState<User[]>([]);
  const [loading, setLoading] = useState<boolean>(true);
  const [showForm, setShowForm] = useState<boolean>(false);

  const fetchUsers = async () => {
    try {
      const token = localStorage.getItem("token");
      const res = await fetch("http://localhost:5000/users", {
        headers: {
          "Content-Type": "application/json",
          Authorization: `Bearer ${token}`,
        },
      });

      const data = await res.json();
      setUsers(Array.isArray(data) ? data : data.users || []);
    } catch (err) {
      console.error("Erro ao buscar usuários:", err);
    } finally {
      setLoading(false);
    }
  };

  const deleteUser = async (userId: string) => {
    const token = localStorage.getItem("token");

    const confirmed = window.confirm("Tem certeza que deseja excluir este usuário?");
    if (!confirmed) return;

    try {
      const res = await fetch(`http://localhost:5000/users/${userId}`, {
        method: "DELETE",
        headers: {
          Authorization: `Bearer ${token}`,
        },
      });

      if (res.ok) {
        setUsers(users.filter((u) => u.id !== userId));
      } else {
        console.error("Erro ao excluir usuário:", await res.text());
      }
    } catch (err) {
      console.error("Erro de rede:", err);
    }
  };

  useEffect(() => {
    fetchUsers();
  }, []);

  const handleUserCreated = () => {
    setShowForm(false);
    fetchUsers(); // atualiza a lista
  };

  const loggedUserId = localStorage.getItem("id");

  console.log(loggedUserId)

  return (
    <div className="max-w-4xl mx-auto mt-10 p-6 bg-white rounded-lg shadow-md">
      <div className="flex justify-between items-center mb-6">
        <h2 className="text-2xl font-semibold">Lista de Usuários</h2>
        <button
          onClick={() => setShowForm(!showForm)}
          className="bg-blue-500 text-white px-4 py-2 rounded-md hover:bg-blue-600 transition"
        >
          {showForm ? "Fechar formulário" : "Cadastrar novo usuário"}
        </button>
      </div>

      {showForm && (
        <div className="mb-6">
          <UserCreateForm
            onSubmit={(data) => {
              console.log("Usuário criado:", data);
              handleUserCreated();
            }}
          />
        </div>
      )}

      {loading ? (
        <p className="text-center">Carregando usuários...</p>
      ) : (
        <table className="w-full table-auto border border-gray-200">
          <thead className="bg-gray-100">
            <tr>
              <th className="p-3 text-left border-b">Nome</th>
              <th className="p-3 text-left border-b">Email</th>
              <th className="p-3 text-left border-b">Papel</th>
              <th className="text-left p-2">Ações</th>
            </tr>
          </thead>
          <tbody>
            {users.map((user) => (

                
              <tr key={user.id} className="hover:bg-gray-50">
                <td className="p-3 border-b">{user.full_name}</td>
                <td className="p-3 border-b">{user.email}</td>
                <td className="p-3 border-b capitalize">{user.role}</td>
                <td className="p-2">
                    {user.id !== loggedUserId && (
                        <button
                        onClick={() => deleteUser(user.id)}
                        className="text-red-600 hover:underline"
                        >
                        Excluir
                        </button>
                    )}
                    </td>
              </tr>
            ))}
          </tbody>
        </table>
      )}
      {loggedUserId && (
  <p className="mt-4 text-sm">
    <button
      onClick={async () => {
        const confirmed = window.confirm("Tem certeza que deseja excluir seus dados pessoais? Essa ação é irreversível.");
        if (!confirmed) return;

        try {
          const token = localStorage.getItem("token");

          const res = await fetch(`http://localhost:5000/users/${loggedUserId}`, {
            method: "DELETE",
            headers: {
              Authorization: `Bearer ${token}`,
            },
          });

          if (res.ok) {
            localStorage.removeItem("token");
            localStorage.removeItem("id");
            window.location.href = "/SignIn";
          } else {
            console.error("Erro ao excluir usuário logado:", await res.text());
            alert("Erro ao excluir seus dados. Tente novamente.");
          }
        } catch (err) {
          console.error("Erro de rede:", err);
          alert("Erro de rede. Tente novamente.");
        }
      }}
      className="text-red-600 hover:underline bg-transparent border-none p-0 cursor-pointer"
    >
      Excluir meus dados pessoais
    </button>
  </p>
)}

    </div>
  );
};

   ```

Nesse trecho é feito uma busca pelos usuários cadastrados no sistema e listado em uma tabela, além disso foi implementado um botão com a função de apagar todos os dados pessoais do usuário logado, para atender a LGPD

</details> 

 <details ><summary>Implementação da criptografia dos dados pessoais dos usuários</summary>
       
   ```python

 def create_user(self, full_name: str, email: str, role: UserRole, password: str) -> User:
        session2 = SecondarySession()
        service2 = UsersKeysService(session2)

        if not full_name or not email or not password:
            raise InvalidUserDataError

        if self.repository.get_by_email(email):
            raise EmailAlreadyInUseError
        
        encryption_key = Fernet.generate_key()
        fernet = Fernet(encryption_key)
    
        encrypted_email = fernet.encrypt(email.encode("utf-8")).decode()
        encrypted_full_name = fernet.encrypt(full_name.encode("utf-8")).decode()

        user = User(full_name=encrypted_full_name, email=encrypted_email, role=role)
        user.set_password(password)

        user = self.repository.insert(user)

        encryption_data = {
            "user_id": user.id,
            "encryption_key": encryption_key.decode() 
        }

        service2.create_user(
            user_id=encryption_data["user_id"],
            encryption_key=encryption_data["encryption_key"]
        )

        user_data = {
            'id' : str(user.id),
            'full_name': full_name,
            'email': email,
        }

        users = redis_client.get(f"users")

        if users:
            users = json.loads(users)  
        else:
            users = [] 

        users.append(user_data)

        redis_client.setex(f"users", 3600, json.dumps(users))

        return user

    def get_user_by_id(self, id: UUID) -> User:
        user = self.repository.get_by_id(id)
        
        usersRedi = redis_client.get(f"users")

        if usersRedi:
            users = json.loads(usersRedi)  
        else:
            users = [] 

        for user_json in users:
            if str(user.id) == str(user_json["id"]):
                user.email = user_json["email"]
                user.full_name = user_json["full_name"]
                break

        if not user:
            raise UserNotFoundError
        return user

   ```

Nesse trecho da classe de service dos usuários, foi implementada a criptografia dos dados pessoais dos usuários, armazenando a chave de criptografia em um banco de dados secundário para permitir a exclusão dos dados pessoais também dos backups do banco, apenas excluindo a chave de criptografia. </br>
Além disso, foi implementado o serviço do REDIS, para armazenamento em cache dos dados pessoais dos usuários, para facilitar a visualização dos dados no sistema
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
      <td>Flask</td>
      <td>★★★★☆</td>
	<td>Entendi</td>
    </tr>
    <tr>
      <td>Postgress</td>
      <td>★★★★☆</td>
	<td>Entendi</td>
    </tr>	
    <tr>
      <td>React</td>
      <td>★★★☆☆</td>
	<td>Sei fazer com ajuda</td>
    </tr>
    <tr>
      <td>Scikit-Learn</td>
      <td>★★★★☆</td>
	<td>Entendi</td>
    </tr>
   <tr>
      <td>MongoDb</td>
      <td>★★★★☆</td>
	<td>Entendi</td>
    </tr>
    <tr>
      <td>REDIS</td>
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
      <td>Pensamento Analítico</td>
      <td>Ao lidar com previsões baseadas em inteligência artificial, aprimorei minha capacidade analítica</td>
    </tr>
    <tr>
      <td>Resolução de Desafios Técnicos</td>
      <td>Durante a implementação do mapa interativo e da plotagem de formas geométricas, enfrentei desafios técnicos complexos que exigiram criatividade, paciência e persistência para encontrar soluções eficazes.</td>
    </tr>
    <tr>
      <td>Colaboração e Comunicação Técnica</td>
      <td>Trabalhar em equipe nesse projeto me ajudou a melhorar minha comunicação técnica, tanto na hora de compartilhar decisões quanto de documentar o raciocínio por trás de certas escolhas, facilitando a colaboração com colegas de diferentes áreas</td>
    </tr>
    <tr>
      <td>Adaptação Contínua</td>
      <td>A necessidade de trabalhar com diferentes bibliotecas, linguagens e frameworks reforçou minha adaptabilidade frente a novas ferramentas e contextos, habilidade essencial em ambientes tecnológicos dinâmicos.</td>
    </tr>
  </table>


 <h2 align="center"> Meus Projetos :books:</h2>
 
   <p align="justify" style="font-family:roboto;"><li><a href="https://github.com/Antonio-Zago/portfolio-apis-fatec/blob/main/API_1.md">1º Semestre: Ibet - Assistente Virtual</a></li></p>
   <p align="justify" style="font-family:roboto;"><li><a href="https://github.com/Antonio-Zago/portfolio-apis-fatec/blob/main/API_2.md">2º Semestre: Vigilant - Sistema de Gerenciamento de Banco de Dados</a></li></p>
   <p align="justify" style="font-family:roboto;"><li><a href="https://github.com/Antonio-Zago/portfolio-apis-fatec/blob/main/API_3.md">3° Semestre: PromoAll - Ecommerce com um motor de regras para promoções aplicadas no momento da compra.</a></li></p>
   <p align="justify" style="font-family:roboto;"><li><a href="https://github.com/Antonio-Zago/portfolio-apis-fatec/blob/main/API_4.md">4° Semestre: BoatHelp - Aplicação Web para sincronização dos dados administrativos, financeiros e operacionais.</a></li></p>
   <p align="justify" style="font-family:roboto;"><li><a href="https://github.com/Antonio-Zago/portfolio-apis-fatec/blob/main/API_5.md">5º Semestre: DataViz - Sistema de análise de dados de recrutamento e seleção. Tem como objetivo oferecer insights valiosos</a></li></p>
   <p align="justify" style="font-family:roboto;"><li><a href="https://github.com/Antonio-Zago/portfolio-apis-fatec/blob/main/API_6.md">6º Semestre: Data Forest - Plataforma web, que permite o cadastro de uma área geográfica</a></li></p>
