# Grupo INE-RJ — caminho para a nova loja de cursos

## 1. Diagnóstico da loja atual

Página analisada: `estudandoead.com/grupoine/loja_virtual/vercombo.php?curso=COMBO, TODOS OS CURSOS`

O que se sabe da página atual:

- Roda numa plataforma white-label compartilhada (`estudandoead.com/<escola>/loja_virtual/`), a mesma usada por outras escolas (Focursos, EAD Ximango, Educ Tech, ETOP...). Ou seja: a marca INE-RJ fica dentro do domínio de outra empresa.
- Páginas PHP com o nome do produto na URL (`?curso=COMBO,%20TODOS%20OS%20CURSOS`): URL feia, ruim para SEO e para compartilhar.
- Combo com "mais de 70 cursos", 1000 horas, categoria "Combos", certificado e link "Verificar certificado".
- Exemplos de cursos listados: Access 2016, After Effects, Inteligência Artificial, Manutenção de Celular.
- O preço não aparece nos resultados de busca (provavelmente só aparece no checkout).

Problemas típicos dessa plataforma que a nova versão resolve:

| Problema | Impacto | Solução no protótipo |
|---|---|---|
| Domínio e marca de terceiros | Pouca confiança, sem identidade | Domínio próprio (ex.: `cursos.grupoine-rj.com.br`) |
| Preço escondido até o checkout | Abandono | Preço e parcelamento na primeira dobra |
| Lista longa sem busca/filtro | Aluno não acha o curso | Catálogo com busca e filtro por área |
| Sem página por curso | Sem SEO, sem anúncio por curso | Página de detalhe com ementa e carga horária |
| Checkout genérico | Conversão baixa | Checkout em 1 tela, Pix em destaque |
| Reputação (Reclame Aqui) | Objeção de compra | FAQ claro, garantia de 7 dias, verificação pública de certificado |

## 2. Telas do protótipo (`index.html`)

1. **Início / Combo** – oferta, números (70+ cursos, 1000 h), áreas, como funciona, FAQ.
2. **Catálogo** – busca e filtro por área.
3. **Curso** – ementa, carga horária, compra do combo ou do curso avulso.
4. **Matrícula** – dados, Pix / cartão 12× / boleto, resumo.
5. **Área do aluno** – progresso e certificados.
6. **Verificar certificado** – validação pública por código.

Para abrir: dê duplo clique em `index.html` ou publique no GitHub Pages (Settings → Pages → branch `main`, pasta `/`).

## 3. Caminho recomendado

### Fase 0 — Apresentação (agora)
- Mostrar este protótipo ao interessado.
- Coletar: lista real dos cursos com carga horária e ementa, preços, CNPJ, telefone/WhatsApp, logo e cores oficiais, depoimentos reais com autorização.

### Fase 1 — Loja (2 a 4 semanas)
- **Stack sugerida:** Next.js + Tailwind no front, hospedagem na Vercel; banco Supabase (Postgres + login).
- **Pagamento:** Asaas, Mercado Pago ou Pagar.me (Pix, cartão parcelado, boleto, webhooks de confirmação).
- **Conteúdo:** catálogo vindo do banco (um registro por curso: nome, área, horas, ementa, preço avulso).
- **SEO:** URL por curso (`/cursos/excel-basico-ao-avancado`), meta tags, sitemap, Google Analytics + Pixel da Meta.
- **WhatsApp:** botão flutuante e envio do acesso após o pagamento (Z-API ou API oficial do WhatsApp).

### Fase 2 — Ensino
Duas opções:
- **Rápida:** manter a plataforma de aulas atual e só trocar a loja (o pagamento aprovado cria o aluno lá, se houver API ou exportação).
- **Completa:** plataforma própria (vídeos no Vimeo/Bunny Stream ou Panda Video, progresso por aula, prova final, certificado em PDF com código e QR Code).

### Fase 3 — Crescimento
- Assinatura mensal (recorrência) além do combo anual.
- Cupons e afiliados.
- Recuperação de carrinho por e-mail/WhatsApp.
- Painel administrativo com vendas, alunos ativos e conclusões.

## 4. Pontos de atenção
- Certificados de cursos livres não são diplomas MEC: comunicar isso com clareza evita reclamações.
- Cumprir LGPD (aviso de privacidade, consentimento para WhatsApp) e o direito de arrependimento de 7 dias (CDC, art. 49).
- Todos os preços, alunos e certificados do protótipo são fictícios.
