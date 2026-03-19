# Tema Invent - Keycloak

Tema customizado para a tela de login do Keycloak do projeto **Inventario 360**.

## Estrutura de Pastas

```
themes/src/main/resources/theme/invent/
├── common/
│   └── resources/            # Recursos compartilhados entre paginas (vazio no momento)
├── email/                    # Templates de e-mail (vazio no momento, herda do tema pai)
├── login/
│   ├── messages/
│   │   └── messages_en.properties   # Traducoes customizadas (ex: titulo "Bem-Vindo!")
│   ├── resources/
│   │   ├── css/
│   │   │   ├── invent.css           # Estilos customizados do tema Invent (cores, layout, componentes)
│   │   │   └── styles.css           # Estilos base do Keycloak (logo, tooltip, background)
│   │   ├── img/
│   │   │   ├── keycloak-bg-darken.svg   # Imagem de fundo (SVG)
│   │   │   ├── keycloak-bg.png          # Imagem de fundo (PNG)
│   │   │   └── keycloak-logo-text.svg   # Logo do Keycloak em texto (SVG)
│   │   └── js/
│   │       ├── password-policy.js       # Script de politica de senha
│   │       └── userProfile.js           # Script de perfil do usuario
│   ├── template.ftl                     # Template principal FreeMarker da tela de login
│   └── theme.properties                 # Configuracao do tema (parent, imports, CSS)
└── welcome/
    └── resources/            # Recursos da pagina de boas-vindas (vazio no momento)
```

## Configuracao do Tema

O arquivo `login/theme.properties` define:

| Propriedade    | Valor                          | Descricao                                  |
|----------------|--------------------------------|--------------------------------------------|
| `parent`       | `keycloak.v2`                  | Tema pai (herda componentes nao customizados) |
| `import`       | `common/keycloak`              | Importa recursos comuns do Keycloak        |
| `styles`       | `css/styles.css css/invent.css`| CSS carregados na pagina de login          |
| `stylesCommon` | `vendor/patternfly-v5/...`     | CSS do PatternFly v5 (framework UI base)   |
| `darkMode`     | `false`                        | Modo escuro desabilitado                   |

## Como Subir no Docker

### Opcao 1: Volume mount no docker-compose

Adicione o volume apontando a pasta do tema para dentro do container:

```yaml
services:
  keycloak:
    image: quay.io/keycloak/keycloak:latest
    command: start-dev
    environment:
      KEYCLOAK_ADMIN: admin
      KEYCLOAK_ADMIN_PASSWORD: admin
    ports:
      - "8081:8080"
    volumes:
      - ./themes/src/main/resources/theme/invent:/opt/keycloak/themes/invent
```

### Opcao 2: Dockerfile customizado

```dockerfile
FROM quay.io/keycloak/keycloak:latest
COPY themes/src/main/resources/theme/invent /opt/keycloak/themes/invent
```

## Ativando o Tema

Apos subir o container, ative o tema de uma das formas:

### Via Admin Console (manual)

1. Acesse `http://localhost:8081/admin/`
2. Va em **Realm Settings** > **Themes**
3. Em **Login theme**, selecione `invent`
4. Clique em **Save**

### Via script automatico

Execute o script `themes/activate-theme.sh` que ja esta incluido no projeto:

```bash
bash themes/activate-theme.sh
```

O script aguarda o Keycloak iniciar, obtem um token de admin e ativa o tema via API REST.

## Customizacoes do Tema

- **Fonte**: Inter (Google Fonts)
- **Cor primaria**: `#3b82f6` (azul)
- **Layout**: Card centralizado com max-width de 480px
- **Header**: Logo com icone hexagonal + titulo "Inventario 360"
- **Features**: Grid 2x2 com icones (Gestao de SKU, Movimentacoes, Relatorios, Automacao)
- **Footer**: Indicador de conexao segura OAuth + copyright
- **Responsivo**: Layout adapta para mobile (< 520px)
