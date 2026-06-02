Passo a Passo do Vídeo:

1️⃣ Instalar o Docker no seu computador:

Baixe o aplicativo Docker Desktop de acordo com o seu sistema operacional (Windows, MacOS ou Linux): https://docker.com

Obs: No Linux existe um passo extra. Após baixar o Docker Desktop, você precisará abrir um terminal (CMD) e executar o comando:

sudo apt install docker-compose

2️⃣ Baixe o arquivo docker-compose.yml do Github:

Faça download do arquivo docker-compose.yml no Github: https://guilhermelaz.com.br/n8n-waha-...

3️⃣ Crie uma pasta com o arquivo dentro, e execute o comando no terminal:

Nos seus downloads, crie uma nova pasta com nome "N8N WAHA Local" ou parecido, e coloque o arquivo docker-compose.yml dentro dela.

Depois abra essa pasta com o arquivo dentro, abra um terminal nela clicando com o botão direito e "abrir no terminal" e execute o comando:

docker-compose up -d

ou para macbook: docker compose up -d

Aguarde o docker baixar e subir os containers dos aplicativos.

4️⃣ Conectar seu Whatsapp no WAHA:

Após baixar tudo, abra o aplicativo Docker Desktop e você verá uma nova stack de containers com os aplicativos N8N e WAHA, e os bancos de dados Redis e Postgres. Para acessá-los basta clicar na porta do container através da tabela de exibição (formato xxxx:xxxx).
Acesse o WAHA através do navegador clicando na porta 3000:3000 pelo aplicativo Docker Desktop, depois clique em "Dashboard" logo em cima.
No dashboard, inicie a sessão default já existente e leia o QR Code que aparece clicando em "Login".

5️⃣ Configurar o N8N:

Após entrar no WAHA com seu Whatsapp, volte ao aplicativo do Docker Desktop e acesse o N8N, clicando na porta 5678:5678 e faça as configurações da sua conta.

Após criar a conta, informe um e-mail para receber uma chave de ativação gratuita do N8N. Isso irá te ajudar liberando algumas funções extras.

Copie a chave que chega em seu e-mail, e no painel do N8N, vá nos três pontinhos do canto inferior esquerdo, clique em settings, e depois "Enter Activation Key" na tela que aparece, e informe a chave que recebeu.

Após ativar o N8N, vá para o painel "Community Nodes" (do lado esquerdo da tela, ainda em settings), clique em "Install a community node" e digite:

n8n-nodes-waha

Clique em "Install" e aguarde.

Pronto! A configuração do N8N está feita e agora podemos montar o Workflow.

6️⃣ Montar o Workflow:

Crie um novo workflow, adicione um Trigger de "Webhook" e o configure da seguinte forma:

HTTP Method: POST
Path: webhook

Depois copie a URL de teste que aparece logo acima, vá no Dashboard do Waha, configurações da sua sessão, "+ Webhook", cole a URL, em events deixe apenas o "message" e clique em "Update".

Após isso volte ao N8N, clique para esperar um evento de teste e envie uma mensagem para o seu número conectado no WAHA através de OUTRO NÚMERO (um segundo número, de algum familiar, amigo, etc. Não funciona com o próprio número da sessão).

Após receber os dados, adicione um node "Set" em frente ao Webhook e salve crie os seguintes campos:

session = session
event = event
chatId = from
pushName = pushName (O outro que está lá em baixo, dentro de _data)
payload_id = id
message = body
fromMe = fromMe

Teste e depois adicione um novo node, dessa vez um "Switch" em frente ao SET. Neste switch apenas faça a comparação, se o "event" for igual a "message" e siga o fluxo normal.

Adicione o node do AI Agent e faça as configurações de prompt, system prompt, chat model e memory.

Pegue a chave de API gratuita do Google Gemini aqui: https://aistudio.google.com/app/apikey

A configuração do node do Redis Chat Memory é a seguinte:

Password: default
Host: host.docker.internal

Depois adicione um Node do WAHA para visualizar a mensagem, passando as configurações de Sessão, Chat ID e Payload ID.

E por fim, adicione um Node do WAHA para responder a mensagem, passando as configurações de Sessão, Chat ID e TEXT, onde TEXT é o Output gerado pelo node do Agente de IA.


BIBLIOTECA N8N: https://n8n.io/workflows/?q=rag 

