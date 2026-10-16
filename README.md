# 🚌 Recarga Buss

Aplicativo mobile para **recarga e acompanhamento de passagens de ônibus**, com suporte a **meia-passagem para estudantes**. Desenvolvido como Trabalho de Conclusão de Curso (TCC) no **CEEP**.

![React Native](https://img.shields.io/badge/React%20Native-0.81-61DAFB?logo=react&logoColor=white)
![Expo](https://img.shields.io/badge/Expo-54-000020?logo=expo&logoColor=white)
![Firebase](https://img.shields.io/badge/Firebase-12-FFCA28?logo=firebase&logoColor=black)
![Plataforma](https://img.shields.io/badge/Plataforma-Android%20%7C%20iOS-0A2A66)

---

## 📋 Sobre o projeto

O **Recarga Buss** permite que o usuário crie uma conta, cadastre seu cartão, compre passagens por **PIX** ou **cartão** e acompanhe em tempo real quantas passagens comprou, usou e ainda tem disponíveis. Estudantes podem enviar a declaração de matrícula para ativar o valor de meia-passagem, e o app avisa quando a declaração está perto de vencer.

## 🎯 Motivação

Na cidade de cascavel **não existe um aplicativo para recarregar o cartão Vale Sim**. Quem depende do transporte coletivo, principalmente estudantes, precisa se deslocar até um ponto de recarga, o que gera filas, perda de tempo e dificuldade de acesso.

O Recarga Buss nasceu para resolver um problema real da comunidade: levar a recarga para o celular, de forma simples, com pagamento digital, acompanhamento do saldo de passagens e controle do benefício de meia-passagem.

> **Nota:** esta versão é uma prova de conceito. A integração direta com o sistema oficial do cartão Vale Sim (a operadora que realiza a recarga efetiva) é a evolução natural do projeto.

## ✨ Funcionalidades

- **Cadastro e login** com e-mail e senha (Firebase Authentication), com máscara de CPF e tratamento de erros.
- **Sessão persistente**: o app mantém o usuário logado e abre direto no menu.
- **Recarga de passagens**
  - Escolha da quantidade de passagens e cálculo automático do total.
  - Pagamento por **PIX** (com código copia e cola) ou por **cartão cadastrado**.
  - Valor da **meia-passagem** aplicado automaticamente para estudantes.
  - Atualização atômica do saldo no banco de dados.
  - **Notificação local** de confirmação da recarga.
- **Passagens**: painel em tempo real com total comprado, usadas e restantes.
- **Extrato**: histórico de recargas ordenado da mais recente para a mais antiga, atualizado em tempo real.
- **Declaração de matrícula**
  - Seleção de arquivo (PDF ou imagem) e informe da data de vencimento.
  - Indicação de prazo (dias restantes, vence hoje ou vencida).
  - Ativação/remoção do benefício de estudante.
  - **Notificação agendada** 5 dias antes do vencimento.
- **Cartão**: cadastro e exclusão de cartão, com prévia visual, detecção de bandeira/banco e máscaras de número e validade. O CVC não é armazenado.

## 🛠️ Tecnologias

| Categoria | Tecnologia |
|---|---|
| Framework | [React Native](https://reactnative.dev/) 0.81 + [Expo](https://expo.dev/) SDK 54 |
| Linguagem | JavaScript (React 19) |
| Navegação | React Navigation (Stack) |
| Backend | [Firebase](https://firebase.google.com/) Authentication e Cloud Firestore |
| Armazenamento local | AsyncStorage |
| Notificações | expo-notifications |
| Arquivos | expo-document-picker |
| Build | EAS Build |

## 📁 Estrutura do projeto

```
Recarga-Buss/
├── assets/                  # Ícones e splash screen
├── src/
│   ├── components/          # Componentes reutilizáveis
│   │   ├── ExtratoCard.js
│   │   ├── InfoCard.js
│   │   ├── Inpunt.js
│   │   └── Menucard.js
│   ├── pages/               # Telas do aplicativo
│   │   ├── Cadastro.js
│   │   ├── Login.js
│   │   ├── Menu.js
│   │   ├── passagens.js
│   │   ├── Recarga.js
│   │   ├── Extrato.js
│   │   ├── Declaracao.js
│   │   └── AdicionarCartao.js
│   ├── styles/colors.js     # Paleta de cores
│   ├── App.js               # Navegação e controle de autenticação
│   ├── firebaseConfig.js    # Configuração do Firebase
│   └── dados_globais.js
├── app.json                 # Configuração do Expo
├── eas.json                 # Perfis de build (EAS)
├── index.js                 # Ponto de entrada
└── package.json
```

## 🗄️ Modelo de dados (Firestore)

```
usuarios/{uid}
 ├── nome, cpf, email, criadoEm
 ├── saldoPassagens             # passagens disponíveis
 ├── passagensTotalHistorico    # total já comprado
 ├── isEstudante, tipoPassagem  # meia-passagem
 ├── cartaoCadastrado, cartaoNumero, cartaoNome, cartaoValidade
 └── extrato/{id}               # subcoleção com o histórico de recargas
      └── tipo, quantidade, valorTotal, pagamento, data, tipoPassagemAplicada

declaracoes/minha_matricula
 └── dataVencimento, arquivo {name, mimeType, size}, atualizadoEm
```

## 🚀 Como executar

### Pré-requisitos

- [Node.js](https://nodejs.org/) 18 ou superior
- npm
- Aplicativo **Expo Go** no celular, ou um emulador Android/iOS

### Passo a passo

```bash
# 1. Clone o repositório
git clone https://github.com/Enthony-Oliveira/Recarga-Buss.git
cd Recarga-Buss

# 2. Instale as dependências
npm install

# 3. Inicie o projeto
npx expo start
```

Depois, escaneie o QR Code com o Expo Go (Android) ou a câmera (iOS), ou use os atalhos:

```bash
npm run android   # abre no emulador/dispositivo Android
npm run ios       # abre no simulador iOS (somente macOS)
npm run web       # abre no navegador
```

> ⚠️ As notificações e o seletor de arquivos funcionam melhor em dispositivo físico ou em um *development build*.

### Configurando seu próprio Firebase

1. Crie um projeto no [Console do Firebase](https://console.firebase.google.com/).
2. Ative **Authentication** (método *E-mail/senha*) e o **Cloud Firestore**.
3. Registre um app Web e copie as credenciais para `src/firebaseConfig.js`:

```js
const firebaseConfig = {
  apiKey: "SUA_API_KEY",
  authDomain: "SEU_PROJETO.firebaseapp.com",
  projectId: "SEU_PROJETO",
  storageBucket: "SEU_PROJETO.firebasestorage.app",
  messagingSenderId: "SEU_SENDER_ID",
  appId: "SEU_APP_ID",
};
```

4. Configure as regras de segurança do Firestore para que cada usuário acesse apenas os próprios dados.

## 📦 Gerando o APK

O projeto usa o [EAS Build](https://docs.expo.dev/build/introduction/). O perfil `preview` gera um APK para instalação direta:

```bash
npm install -g eas-cli
eas login
eas build --platform android --profile preview
```

## 🔭 Melhorias futuras

- Integração com o sistema oficial do cartão Vale Sim, para que a recarga seja creditada de fato no cartão físico.
- Integração real com gateway de pagamento (PIX e cartão).
- Upload do comprovante de matrícula para o Firebase Storage.
- Validação do CPF e do cartão.
- Desconto de passagens ao utilizar o ônibus (integração com validador).
- Recuperação de senha por e-mail.

## 👨‍💻 Autor

**Enthony Oliveira**
Projeto de TCC — CEEP

[![GitHub](https://img.shields.io/badge/GitHub-Enthony--Oliveira-181717?logo=github)](https://github.com/Enthony-Oliveira)

## 📄 Licença

Projeto desenvolvido para fins acadêmicos.
