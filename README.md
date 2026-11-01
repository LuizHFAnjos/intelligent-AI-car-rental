# AI Car Rental Assistant

Assistente virtual inteligente para atendimento e cotação de aluguel de veículos, desenvolvido com Java, Spring Boot, LangChain4j e Google Gemini.

O projeto demonstra a integração de Inteligência Artificial Generativa em aplicações Java, permitindo que um modelo de linguagem interprete solicitações em linguagem natural e utilize ferramentas implementadas em Java para executar tarefas específicas.

## Sobre o projeto

O AI Car Rental Assistant simula um assistente virtual de uma empresa de aluguel de veículos corporativos.

Seu objetivo é permitir que usuários solicitem informações e cotações de aluguel por meio de conversas em linguagem natural, sem precisar preencher formulários tradicionais ou conhecer as regras de cálculo.

O diferencial do projeto está na utilização de **Tool Calling**, recurso que permite ao modelo de IA identificar quando precisa de uma ferramenta externa e acionar métodos Java para executar operações específicas.

### Funcionalidades

- Interpretar solicitações dos usuários em linguagem natural.
- Responder perguntas sobre o serviço de aluguel de veículos.
- Identificar informações necessárias para realizar uma cotação.
- Solicitar dados adicionais quando as informações fornecidas forem insuficientes.
- Acionar ferramentas Java para calcular valores de aluguel.
- Considerar a categoria do veículo, a quantidade de diárias e a contratação de seguro.
- Retornar respostas contextualizadas de acordo com a solicitação.

### Exemplo de interação

**Usuário:**

> Gostaria de alugar um SUV com seguro por 5 dias. Quanto ficaria?

**Assistente:**

> Vou calcular a cotação considerando um SUV, 5 diárias e a contratação de seguro.

O assistente utiliza a ferramenta Java responsável pelo cálculo e retorna o resultado ao usuário.

Caso alguma informação necessária esteja ausente, o modelo pode solicitar os dados antes de realizar o cálculo.

---

## Como funciona a integração com IA?

O projeto utiliza o modelo Gemini, integrado à aplicação Java por meio do LangChain4j.

O fluxo de funcionamento acontece da seguinte maneira:

1. O usuário envia uma mensagem por meio de uma requisição HTTP.
2. O Controller recebe a solicitação.
3. O serviço de IA encaminha a mensagem ao modelo Gemini, juntamente com as instruções configuradas.
4. O modelo interpreta a solicitação e identifica se precisa utilizar alguma ferramenta.
5. Caso necessário, o LangChain4j permite que o modelo acione um método Java disponibilizado com a anotação `@Tool`.
6. A ferramenta executa o cálculo solicitado e retorna o resultado.
7. O modelo utiliza o retorno da ferramenta para construir uma resposta em linguagem natural.

Quando uma ferramenta não é necessária, o modelo pode responder diretamente ao usuário.

### O que é Tool Calling?

Tool Calling é um recurso que permite que um modelo de IA identifique quando precisa utilizar uma função ou ferramenta externa para realizar determinada tarefa.

Neste projeto, o Gemini pode solicitar a execução de um método Java responsável pelo cálculo da cotação do aluguel.

Dessa forma, a aplicação combina a capacidade de interpretação de linguagem natural dos LLMs com a lógica de negócio implementada em Java.

---

## Arquitetura do projeto

O projeto utiliza uma organização em camadas, separando as responsabilidades de cada componente.

| Pacote | Responsabilidade |
|---|---|
| `config` | Configurações e integração dos componentes com o Gemini. |
| `controller` | Recebimento e gerenciamento das requisições HTTP. |
| `service` | Gerenciamento da interação entre a aplicação e o modelo de IA. |
| `tools` | Disponibilização de ferramentas Java que podem ser acionadas pela IA. |

### Principais componentes

- **AssistantController:** disponibiliza o acesso às funcionalidades do assistente por meio de uma API REST.
- **AssistantAiService:** interface responsável pela interação com o modelo de IA utilizando a integração do LangChain4j.
- **AssistantTools:** disponibiliza métodos Java anotados com `@Tool`, incluindo a funcionalidade de cálculo de cotações.
- **AssistantConfig:** configura os componentes necessários para a integração entre o Gemini e as ferramentas.

---

## Tecnologias utilizadas

- **Java:** linguagem de programação.
- **Spring Boot:** estrutura principal da aplicação.
- **LangChain4j:** integração de modelos de linguagem com aplicações Java e ferramentas externas.
- **Google Gemini:** modelo de Inteligência Artificial Generativa.
- **Maven:** gerenciamento de dependências e build.
- **API REST:** comunicação entre o cliente e a aplicação.

---

## Como executar o projeto

### 1. Pré-requisitos

Antes de começar, é necessário ter instalado:

- Java em uma versão compatível com o projeto.
- Maven ou Maven Wrapper.
- IntelliJ IDEA ou outra IDE de sua preferência.
- Uma chave de API do Google Gemini.

### 2. Clone o repositório

```bash
git clone https://github.com/LuizHFAnjos/ai-car-rental-assistant.git
```

Entre na pasta do projeto:

```bash
cd ai-car-rental-assistant
```

### 3. Configure a chave da API Gemini

Crie uma chave de API no [Google AI Studio](https://aistudio.google.com/apikey).

No arquivo `src/main/resources/application.properties`, configure:

```properties
spring.application.name=java-ai-assistant

gemini.api.key=${GEMINI_API_KEY}
gemini.model=gemini-2.5-flash
```

A aplicação utiliza uma variável de ambiente para acessar a chave, evitando deixar credenciais diretamente no código-fonte.

**Windows (PowerShell):**

```powershell
$env:GEMINI_API_KEY="SUA_CHAVE_API"
```

**Linux/macOS:**

```bash
export GEMINI_API_KEY="SUA_CHAVE_API"
```

Substitua `SUA_CHAVE_API` pela chave gerada no Google AI Studio.

Importante: nunca publique sua chave de API no GitHub. O arquivo `application.properties` deve permanecer protegido pelo `.gitignore`.

### 4. Execute a aplicação

Pelo terminal, utilizando Maven:

```bash
mvn spring-boot:run
```

Caso o projeto tenha Maven Wrapper:

**Windows:**

```powershell
.\mvnw.cmd spring-boot:run
```

**Linux/macOS:**

```bash
./mvnw spring-boot:run
```

Também é possível iniciar a aplicação executando a classe principal do Spring Boot pela IDE.

---

## Como testar a aplicação

Após iniciar a aplicação, você pode testar o assistente utilizando ferramentas que permitem realizar requisições HTTP.

Algumas opções são:

- [Postman](https://www.postman.com/): ferramenta para criar, enviar e analisar requisições HTTP.
- [Insomnia](https://insomnia.rest/): alternativa ao Postman para testar APIs REST.
- [Thunder Client](https://www.thunderclient.com/): extensão do Visual Studio Code para realizar requisições HTTP.

### Testando pelo Postman

1. Abra o Postman e crie uma nova requisição HTTP.
2. Selecione o método HTTP utilizado pelo endpoint do `AssistantController`.
3. Informe a URL local da aplicação, considerando a porta configurada no Spring Boot e a rota disponibilizada pelo Controller.
4. Caso o endpoint receba um corpo JSON, acesse **Body → raw → JSON** e informe os dados esperados.
5. Clique em **Send** para enviar a solicitação.
6. Analise a resposta retornada pelo assistente.

A URL, o método HTTP e o formato do JSON devem corresponder à implementação do `AssistantController`.

### Cenários para testar

Você pode experimentar diferentes situações para observar o comportamento da IA:

**1. Solicitação de cotação**

Informe a categoria do veículo, a quantidade de diárias e se deseja contratar seguro.

**2. Informações incompletas**

Solicite uma cotação sem informar todos os dados necessários e observe se o assistente solicita as informações que faltam.

**3. Dúvidas gerais**

Faça perguntas sobre o serviço de aluguel para verificar se o modelo consegue responder sem acionar uma ferramenta.

**4. Utilização de ferramentas**

Envie uma solicitação que exija o cálculo de uma cotação e observe a integração entre o Gemini e o método Java anotado com `@Tool`.

Esses testes permitem verificar tanto a comunicação com a API REST quanto a capacidade do modelo de interpretar solicitações, identificar informações ausentes e utilizar ferramentas externas para executar tarefas.

---

## Segurança

A chave de API do Gemini é armazenada em uma variável de ambiente, evitando sua exposição no repositório.

Boas práticas adotadas:

- Não versionar credenciais.
- Utilizar variáveis de ambiente para informações sensíveis.
- Manter a configuração local separada do código compartilhado.

---

## Objetivos de aprendizado

Este projeto faz parte dos meus estudos em Inteligência Artificial Generativa e desenvolvimento de aplicações Java.

Durante seu desenvolvimento, são explorados conceitos como:

- Integração de LLMs em aplicações Java.
- Engenharia de prompts.
- Tool Calling e execução de ferramentas externas.
- LangChain4j.
- Desenvolvimento de APIs REST com Spring Boot.
- Integração entre Inteligência Artificial e lógica de negócio.

---

## Autor

**Luiz Henrique**

Estudante de Análise e Desenvolvimento de Sistemas no IFBA, interessado em desenvolvimento de software, Inteligência Artificial Generativa e construção de soluções que conectam tecnologia a problemas reais.

- GitHub: [@LuizHFAnjos](https://github.com/LuizHFAnjos)
- LinkedIn: [luizhfanjos](https://www.linkedin.com/in/luizhfanjos/)