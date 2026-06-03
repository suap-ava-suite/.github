<div align="center">

<img src="https://raw.githubusercontent.com/suap-ava-suite/cdn-suap_ava_suite/main/favicons/favicon-96x96.png" alt="SUAP/AVA Suite" width="96" />

# SUAP/AVA Suite

**A ponte entre o seu Sistema de Gestão Acadêmica e o Moodle — pronta para produção.**

[
[
[
[
[

</div>

***

## O que é a SUAP/AVA Suite?

A **SUAP/AVA Suite** é um ecossistema open-source de integração entre **Sistemas de Gestão Acadêmica (SGA)** — como o SUAP, SIGAA e qAcadêmico — e o **Moodle LMS**. Com ela, instituições de ensino eliminam o trabalho manual de sincronização de turmas, alunos e professores: tudo flui automaticamente do SGA para o AVA, com notas e frequências sincronizadas de volta.

> 🎯 **Projetada para o IFRN e pronta para qualquer instituição** que use o Moodle como AVA.

***

## Por que usar?

| Problema comum | Como a Suite resolve |
|---|---|
| Cadastro manual de turmas e usuários no Moodle | Sincronização automática via API a partir do SGA |
| Login separado para o AVA | Autenticação OAuth2 com as credenciais institucionais |
| Alunos perdem cursos espalhados em vários Moodles | Painel unificado com todos os cursos em um só lugar |
| Dependências Python inconsistentes entre projetos | Meta-pacote centralizado com versões travadas |
| Assets CSS/JS duplicados em cada projeto | CDN compartilhado para toda a Suite |

***

## Arquitetura em um olhar

```
┌─────────────────────────────────────────────────────────┐
│                  Instituição de Ensino                  │
│                                                         │
│  ┌──────────────┐        ┌────────────────────────────┐ │
│  │     SGA      │◄──────►│   Integrador AVA (Django)  │ │
│  │ SUAP / SIGAA │        │   + Painel AVA (Django)    │ │
│  └──────────────┘        └───────────┬────────────────┘ │
│                                      │ REST API          │
│                          ┌───────────▼────────────────┐ │
│                          │        Moodle LMS          │ │
│                          │  local_suap · tool_sga     │ │
│                          │  auth_suap · tool_painelava│ │
│                          └────────────────────────────┘ │
└─────────────────────────────────────────────────────────┘
```

> 💡 Uma **GitHub Page** com o detalhamento completo da arquitetura e do fluxo de dados estará disponível em breve.

***

## Componentes

### 🐍 Aplicações Django

| Projeto | Descrição |
|---|---|
| [`integrador_ava`](https://github.com/suap-ava-suite/djangoapp-integrador_ava) | **Middleware** central que recebe dados do SGA e os sincroniza no Moodle. Suporta SUAP nativamente e qualquer SGA via padrão genérico. |
| [`painel_ava`](https://github.com/suap-ava-suite/djangoapp-painel_ava) | **Dashboard** unificado: cada usuário visualiza todos os seus cursos e diários de todos os Moodles integrados em um único portal. |

### 🔌 Plugins Moodle

| Projeto | Tipo | Descrição |
|---|---|---|
| [`local_suap`](https://github.com/suap-ava-suite/moodle-local_suap) | Local Plugin | Núcleo da integração. Recebe os dados do Integrador AVA, sincroniza categorias, cursos, usuários, grupos, matrículas, coortes, notas e faltas. |
| [`tool_sga`](https://github.com/suap-ava-suite/moodle-tool_sga) | Admin Tool | API REST para receber sincronizações vindas diretamente do SGA via `POST` autenticado por token. |
| [`auth_suap`](https://github.com/suap-ava-suite/moodle-auth_suap) | Auth Plugin | Login via **OAuth2 do SUAP**. Cria e atualiza perfis automaticamente com foto, e-mail institucional, campus e tipo de usuário. |
| [`tool_painelava`](https://github.com/suap-ava-suite/moodle-tool_painelava) | Admin Tool | API externa para o Painel AVA: retorna todos os cursos de um usuário organizados por tipo (Diário, FIC, Coordenação etc.). |

### 📦 Pacotes Python

| Projeto | Descrição |
|---|---|
| [`avaintegration_metapackage`](https://github.com/suap-ava-suite/pypkg-avaintegration_metapackage) | Meta-pacote Python 3.14 / Django 6.0 com todas as dependências do ecossistema AVA consolidadas e versionadas. |

### 🗂️ Infraestrutura compartilhada

| Projeto | Descrição |
|---|---|
| [`cdn-suap_ava_suite`](https://github.com/suap-ava-suite/cdn-suap_ava_suite) | CDN central de assets estáticos (CSS, JS, temas, favicons) compartilhados entre todos os projetos da Suite. |

***

## Começando

Cada repositório possui seu próprio guia de instalação. Para um deploy completo da Suite, consulte a documentação de cada componente na seguinte ordem recomendada:

1. **Plugins Moodle** → `moodle-local_suap`, `moodle-auth_suap`, `moodle-tool_sga`, `moodle-tool_painelava`
2. **Aplicações Django** → `djangoapp-integrador_ava`, `djangoapp-painel_ava`
3. **Meta-pacote Python** → `pypkg-avaintegration_metapackage`

***

## Contribuindo

Contribuições são bem-vindas! Abra uma *issue* ou *pull request* no repositório correspondente ao componente que deseja melhorar. Siga as diretrizes de contribuição de cada projeto.

***

## Licença

Distribuído sob a licença **MIT**. Veja o arquivo `LICENSE` em cada repositório para mais detalhes.

***

<div align="center">
  Desenvolvido com ❤️ para a comunidade de educação pública brasileira.
</div>
