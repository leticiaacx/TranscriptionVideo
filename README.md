# TranscriptionVideo
Desenvolver uma aplicação que permita a submissão de vídeos para que os mesmos sejam transcritos. Embora pareça simples, é importante lembrar que:

- Os vídeos podem ter diferentes formatos
- O processo de transcrição não é muito rápido, então iremos utilizar uma ferramenta que fará o processo de transcrição de forma assíncrona. Isto é, os vídeos serão submetidos e sua transcrição só ficara disponível minutos depois
- A aplicação precisa ter controle de autenticação e autorização
- Visto que transcrição não é barato, precisaremos controlar “cotas” de uso para cada usuário

# Especificações

A aplicação contará com apenas 4 casos de uso.

## Caso de uso: Login

- Recomendamos utilizar um serviço de autenticação, como Firebase Authentication

<aside>
🔥

Se for utilizar Firebase Authentication, recomendo:

- Criar um novo projeto no [Console Firebase](https://console.firebase.google.com/) (É gratuito)
- Seguir os passos da documentação para autenticação “Password-Based” para Firebase Web [aqui](https://firebase.google.com/docs/auth/web/password-auth)
- Destaco um ponto importante:
    - Para realizar “login”, o frontend o SDK do firebase, se comunicando diretamente com ele (sem passar pelo backend)
    - Já o backend deverá utilizar a lib “firebase-admin”, para poder fazer operações como “validar um token” ou “criar um usuário”
</aside>

## Caso de uso: Cadastro

- Deve conter um formulário simples, com validações relevantes para o cadastro do usuário

## Caso de uso: Visualizar solicitações de transcrição

- Visualizar as minhas solicitações enviadas para transcrição
    - Sugiro exibir campos relevantes, como o status da transcrição
    - Também é interessante permitir o download do arquivo de transcrição, quando estiver finalizada

## Caso de uso: Solicitar nova transcrição

- Submeter arquivos de mídia. Sugiro escolher um formato único por simplicidade (ex: MP4)
- É importante que o procedimento seja **assíncrono**. Isto é, o usuário submete o arquivo e já recebe uma resposta positiva, indicando que o mesmo será processado. Apenas após alguns segundos/minutos, a transcrição estará disponível
- Deverá utilizar a API Open AI Transcriptions: https://platform.openai.com/docs/guides/speech-to-text/quickstart
- Deverá criar um sistema de “cota” de uso para o usuário. Deve limitar as submissões (em número, MBs, minutos transcritos, etc) de cada usuário, em um único dia.
Seja criativo💡!

### **Passo-a-passo para transcrição (é apenas um exemplo):**

- Receber arquivo via requisição HTTP
- Armazenar o arquivo em pasta local temporária
- Criar um registro deste arquivo no banco de dados, com status indicando “processamento”
- Responder requisição HTTP com status de sucesso
- Converter o arquivo para formato áudio (MP3)
- Utilizar API Open AI para gerar a transcrição (Máx 25MB)
- Deletar arquivo da pasta temporária
- Atualizar registro do banco de dados, indicando que arquivo foi transcrito
