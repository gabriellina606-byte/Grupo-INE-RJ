# Integração com a Escola Avançada (API V2)

Fonte: coleção Postman "Escola Avançada API V2" (documenter.getpostman.com/view/20632445/2s93z3f5Bt).

## Como a API funciona

- Endereço base: `https://<URL-DA-ESCOLA>/api/v2/?<recurso>/<ação>`. A URL é fornecida pelo suporte (WhatsApp (92) 99414-2045).
- Autenticação: campo `token` enviado no corpo (`multipart/form-data`) de cada requisição. Exceção: o `DELETE` de remover curso manda os campos como cabeçalhos.
- O token é gerado pela escola em **Configurações → Gerar token** (fim da página).
- Respostas em JSON: `{ "erro": "", "resultado": ... }`. Sucesso quando `erro` vem vazio.
- **O token dá acesso aos dados dos alunos.** Ele nunca pode ir para o navegador: as chamadas são feitas só no servidor (função serverless), com o token guardado em variável de ambiente.

## Endpoints que vamos usar

| Para quê | Endpoint | Campos principais | Retorno |
|---|---|---|---|
| Catálogo do site | `POST ?cursos/listar` | `categoria` (opcional) | `nome`, `aulas`, `carga_horaria`, `obs` (descrição), `categoria_loja`, `preco`, `parcelas`, `capa_image`, `status` |
| Ementa de cada curso | `POST ?cursos/aulas` | `curso` (id) | lista de aulas (`"01 - Introdução"`, ...) |
| Ver se o aluno já existe | `POST ?usuarios/listar` | `cpf` ou `email` ou `id` | dados do aluno, `login` |
| Cadastrar aluno | `POST ?usuarios/novo` | `nome`, `email`, `fone`, `cpf`, `status=ativo`, `datafinal`, `certificado=s`, `vendedor`... | `login` (id do aluno) e `senha` |
| Liberar cursos | `POST ?usuarios/vinculocurso` | `aluno` + `idcurso` ou `idcombo` ou `categoria`. **Só `aluno` = todos os cursos.** | `"Todos os cursos vinculados!"` |
| Enviar login e senha | `POST ?usuarios/envioemail` | `aluno` | `"E-mail enviado!"` |
| Reembolso/cancelamento | `DELETE ?usuarios/remover_curso_combo` | `aluno`, `idcurso`, `idcombo` (cabeçalhos) | `"Cursos removidos com sucesso!"` |
| Progresso do aluno | `POST ?usuarios/cursosvinculados` | `id_aluno` | curso, situação, % concluída, última aula |

Outros endpoints disponíveis (não necessários agora): editar aluno, financeiro (parcelas, recebimentos/inadimplentes), turmas, horários, notas presenciais, contrato em PDF, mensagens, aniversariantes, melhores alunos, novo funcionário.

**Não existe na API:** webhook, pagamento e verificação de certificado. O pagamento fica com um gateway externo, e a verificação de certificado continua apontando para a página atual da plataforma.

## Fluxo de venda do combo

```
Site (vitrine) ──▶ Checkout (gateway) ──pagamento aprovado──▶ Função no servidor
                                                               1. usuarios/listar (cpf/e-mail)
                                                               2. se não existe: usuarios/novo
                                                               3. usuarios/vinculocurso (só "aluno" = todos os cursos)
                                                               4. usuarios/envioemail
                                                               5. (opcional) WhatsApp com o acesso
Reembolso ──▶ Função no servidor ──▶ usuarios/remover_curso_combo (ou editar status=inativo)
```

O catálogo (`cursos/listar` + `cursos/aulas`) é lido no servidor e guardado em cache, por exemplo 1 vez por dia. Assim o site mostra nomes, descrições, carga horária, capas e lista de aulas reais, sem expor o token.

## Dois caminhos para o pagamento

| | A. Checkout pronto (Digital Manager Guru) | B. Checkout próprio + API |
|---|---|---|
| Como | Integração nativa da plataforma (Configurações → Integrações). O Guru matricula e remove o aluno sozinho. | Gateway (Asaas, Mercado Pago ou Pagar.me) + função serverless que chama a API. |
| Código | Quase nenhum | Uma função pequena (Vercel/Netlify) |
| Custo | Mensalidade do Guru + taxas | Só as taxas do gateway (hospedagem gratuita no início) |
| Controle do checkout | Layout do Guru | Total, com a marca INE-RJ |

Recomendação: **B**. A API é simples e cobre o fluxo inteiro. A plataforma já trabalha com Asaas (aparece em "BOLETOS BAIXADOS DO ASAAS" no financeiro), o que facilita usar a mesma conta. O **A** fica como plano rápido, se for preciso vender antes de a função estar pronta.

## Perguntas para o suporte da Escola Avançada

1. Qual é a URL da API do Grupo INE-RJ?
2. `cursos/listar` não retorna o **id** do curso. Como obter os ids de curso e de combo (`idcurso`, `idcombo`)?
3. O "Combo todos os cursos" tem um `idcombo`, ou basta vincular só com `aluno`?
4. Que valor usar em `datafinal` (fim do acesso) para o combo?
5. `vendedor` é obrigatório? Qual id usar para as vendas do site, para que contem nas matrículas da escola?
6. Existe ambiente de testes ou aluno de teste que possamos apagar?
7. Há limite de requisições?
8. O e-mail de `envioemail` sai com a marca da escola? Dá para personalizar?
