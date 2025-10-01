# Projeto-GoodAccess-

Equipe envolvida:

Gabriel De biasi Couto

João Pedro da Silva Costa

Lucas Mendes Moraes

Pedro Noronha dos Santos

Rodrigo Campos Cordeiro

GoodWe Assistant - Análise de Energia Solar com Streamlit
Este é o README do projeto "GoodWe Assistant", uma aplicação web desenvolvida em Python com a biblioteca Streamlit. A aplicação permite visualizar e analisar dados de geração de energia de um inversor solar da GoodWe, integrando simulação de automação residencial, um assistente com Inteligência Artificial e visualização em Realidade Aumentada.

📜 Visão Geral do Código (app.py)
O arquivo app.py contém toda a lógica da aplicação. Ele é estruturado em várias seções lógicas para facilitar a manutenção e o entendimento.

1. Importações e Configurações Iniciais
No início do script, importamos todas as bibliotecas necessárias:

streamlit: Para a criação da interface web.

pandas: Para a manipulação e análise dos dados.

plotly.express: Para a criação dos gráficos interativos.

json, os, pathlib, datetime: Bibliotecas padrão do Python para manipulação de arquivos, datas e variáveis de ambiente.

streamlit_js_eval: Para executar JavaScript no lado do cliente e detectar se o usuário está em um dispositivo móvel.

Módulos locais como ai_openai e goodwe_client (que contêm a lógica para se comunicar com a IA e a API da GoodWe, respectivamente).

2. Funções Utilitárias
Esta seção contém funções de apoio que são usadas em várias partes do código:

carregar_mock(path): Carrega os dados de um arquivo JSON local (mock_today.json). Esta função é usada no "Modo Mock" e garante que a aplicação possa ser demonstrada sem uma conexão real com a API.

kwh(x) e kw(x): Funções de formatação para exibir os números de energia e potência em um formato legível (ex: "10,50 kWh").

resumo_dia(df): Recebe o DataFrame com os dados do dia e calcula métricas chave, como a energia total gerada, o pico de potência, o horário desse pico e o estado de carga inicial e final da bateria.

3. Integração com a API SEMS (GoodWe)
Esta é a parte central para a coleta de dados reais:

parse_column_timeseries(resp_json, column_name): A API da GoodWe retorna dados em um formato JSON complexo. Esta função é especializada em extrair as séries temporais (valor vs. tempo) desse JSON e convertê-las para um DataFrame do Pandas.

fetch_realtime_df(...): Orquestra a busca de dados. Ela primeiro realiza o login (crosslogin), depois itera sobre as colunas de dados desejadas (como "Pac", "Eday"), chama a API para cada uma (get_inverter_data_by_column) e, no final, une todos os DataFrames resultantes em um único DataFrame consolidado.

4. Interface do Usuário (UI)
Toda a aparência e interatividade da aplicação são definidas aqui, usando as funções do Streamlit:

st.set_page_config(): Define o título da página, o ícone e o layout.

st.markdown("<style>...", unsafe_allow_html=True): Injeta CSS customizado para estilizar a aplicação com as cores da marca (vermelho e branco) e melhorar a aparência dos botões e títulos.

st.sidebar: Cria a barra lateral de configuração, onde o usuário pode:

Escolher entre o modo "Mock" e "Real".

Inserir o número de série do inversor e a data.

Fornecer as credenciais da API SEMS (se estiver no modo "Real").

Dashboard Principal: O corpo principal da página exibe:

st.metric(): Mostra os KPIs calculados pela função resumo_dia.

st.plotly_chart(): Renderiza os gráficos de linha interativos de potência e carga da bateria.

st.expander() e st.dataframe(): Permitem ao usuário visualizar a tabela completa de dados.

5. Simulação de Dispositivos da Casa
Esta seção demonstra a lógica de automação residencial:

st.session_state.devices: Utiliza o estado da sessão do Streamlit para armazenar a lista de dispositivos, seu consumo e seu estado (ligado/desligado). Isso garante que os dados não se percam a cada interação do usuário.

st.form(): Cria um formulário para adicionar novos dispositivos de forma organizada.

st.button(): Os botões de "Ligar/Desligar" e "Remover" são criados dinamicamente para cada dispositivo, com uma chave única (key=f"toggle_{dev_id}") para que o Streamlit saiba qual dispositivo está sendo alterado.

6. Assistente com IA
A integração com a IA é feita de forma simples:

st.text_area(): Fornece uma caixa de texto para o usuário digitar sua pergunta.

st.button("Enviar"): Ao ser clicado, o botão pega o texto do usuário e os dados resumidos do dia (resumo_dia) e os envia para a função explicar_dia(), que se comunica com a API da OpenAI.

st.info(): Exibe a resposta da IA de forma destacada.

7. Visualização em Realidade Aumentada (AR)
Esta é uma funcionalidade avançada para dispositivos móveis:

streamlit_js_eval(): Executa um pequeno script JavaScript para verificar o navigator.userAgent do navegador e determinar se o acesso está sendo feito por um dispositivo móvel.

st.radio(): Permite ao usuário escolher entre a câmera frontal e traseira.

html(gerar_ar_html(...)): Gera e renderiza um bloco de HTML e JavaScript. O JavaScript usa a API navigator.mediaDevices.getUserMedia para acessar a câmera do dispositivo e exibe o feed de vídeo. Sobre este vídeo, uma camada de HTML (<div>) mostra o consumo total de energia em tempo real, criando o efeito de Realidade Aumentada.

Como Executar o Projeto
Pré-requisitos:

Python 3.8 ou superior instalado.

pip (gerenciador de pacotes do Python).

Clone o Repositório:

git clone <URL-DO-SEU-REPOSITORIO>
cd <NOME-DO-SEU-REPOSITORIO>

Instale as Dependências:
Crie um arquivo requirements.txt com o seguinte conteúdo:

streamlit
pandas
plotly
streamlit-js-eval
openai # Adicione se estiver usando a API da OpenAI

E instale as bibliotecas:

pip install -r requirements.txt

Execute a Aplicação:
No terminal, execute o seguinte comando:

streamlit run app.py

Acesse no Navegador:
Abra o seu navegador e acesse o endereço http://localhost:8501. A aplicação estará funcionando no modo "Mock" por padrão. Para usar o modo "Real", preencha suas credenciais na barra lateral.
