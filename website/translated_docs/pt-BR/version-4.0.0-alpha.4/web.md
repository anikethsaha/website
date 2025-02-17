---
id: version-4.0.0-alpha.4-webui
title: Interface de Usuário da Web
original_id: webui
---

![Uplinks](https://user-images.githubusercontent.com/558752/52916111-fa4ba980-32db-11e9-8a64-f4e06eb920b3.png)

O Verdaccio possui uma interface de usuário da web para exibir apenas os pacotes privados e pode ser personalizável.

```yaml
web:
  enable: true
  title: Verdaccio
  logo: logo.png
  gravatar: true | false
  scope: @scope
  sort_packages: asc | desc
```

Todas as restrições de acesso definidas para [proteger seus pacotes](protect-your-dependencies.md) também se aplicam à interface da web.

### Configuração

| Propriedade   | Tipo       | Obrigatório | Exemplo                        | Suporte  | Descrição                                                                                                                                            |
| ------------- | ---------- | ----------- | ------------------------------ | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| enable        | boolean    | Não         | true/false                     | completo | habilitar a interface web                                                                                                                            |
| title         | string     | Não         | Verdaccio                      | completo | Título da página web                                                                                                                                 |
| gravatar      | boolean    | Não         | true                           | `>v4` | Se esta propriedade estiver habilitada, gravatars serão gerados internamente                                                                         |
| sort_packages | [asc,desc] | Não         | asc                            | `>v4` | Se esta propriedade estiver habilitada, gravatars serão gerados internamente                                                                         |
| logo          | string     | Não         | http://my.logo.domain/logo.png | completo | a URI onde o logotipo está localizado (logotipo do cabeçalho)                                                                                        |
| scope         | string     | Não         | \\@myscope                   | completo | If you're using this registry for a specific module scope, specify that scope to set it in the webui instructions header (note: escape @ with \\@) |

> Recomenda-se que o tamanho do logotipo tenha o seguinte tamanho `40x40` pixels.