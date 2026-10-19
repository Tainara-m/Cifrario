# Cifrário

Aplicação web para encriptar mensagens por **substituição de caracteres**. Você escreve o texto, escolhe o nível e a forma de encriptação, e baixa um **PDF** com a mensagem encriptada e a **chave** (uma tabela que troca cada símbolo pelo caractere original).

Funciona inteira no navegador, em um único arquivo HTML. Nada é enviado para servidor nenhum.

> **Sobre a autoria:** este projeto foi desenvolvido pelo **Claude** (IA da Anthropic) a partir da orientação e dos requisitos da **Tainara**, que definiu a ideia, os níveis, as formas de encriptação e o formato de saída em PDF, e revisou o resultado ao longo do desenvolvimento.

## Como usar

1. Abra o `cifrario.html` no navegador (é preciso internet na primeira abertura, veja [Dependências](#dependências)).
2. Na aba **Encriptar**:
   1. Escreva a mensagem (até 3000 caracteres).
   2. Escolha o **nível**: Fácil, Moderado ou Difícil.
   3. Escolha a **forma**: Emojis, Símbolos, Caracteres ou Binário.
   4. Clique em **Encriptar** e depois em **Baixar PDF**.
3. Na aba **Decriptar**, cole a mensagem encriptada e o código da chave para ler o texto original.

Também dá para decifrar sem o aplicativo: o PDF traz a tabela completa no formato `símbolo = caractere`. Basta procurar cada símbolo da mensagem na tabela e escrever o caractere correspondente.

## Níveis

| Nível | Como a tabela é montada | O que muda para quem tenta quebrar |
|---|---|---|
| **Fácil** | Ordem fixa, só deslocada. Cada caractere tem um símbolo. | O padrão aparece rápido. Serve para brincadeiras e jogos. |
| **Moderado** | Tabela embaralhada. Cada caractere tem um símbolo, e espaços também viram símbolos. | Segura curiosos, mas um texto longo ainda pode ser quebrado por contagem de letras. |
| **Difícil** | Tabela embaralhada em que cada caractere tem 2 ou 3 símbolos diferentes, sorteados a cada uso. | A contagem de letras deixa de funcionar. |

## Formas de encriptação

| Forma | Símbolos usados |
|---|---|
| **Emojis** | 420 emojis (animais, comidas, objetos, natureza) |
| **Símbolos** | 336 símbolos (setas, operadores matemáticos, formas geométricas) |
| **Caracteres** | 319 letras latinas acentuadas, gregas e cirílicas |
| **Binário** | Códigos de 8 dígitos (níveis fácil e moderado) ou de 10 dígitos (nível difícil) |

## O que pode ser encriptado

Letras (com acentos do português), números, espaços, quebras de linha e a pontuação comum, num total de 151 caracteres. Emojis e outros caracteres dentro do texto original **não são aceitos**, e o aplicativo informa quais são. Aspas curvas, travessões e reticências são convertidos para as versões simples antes de encriptar.

## A chave

A chave tem duas formas, e as duas vão no PDF:

- **Código da chave**, como `E2-6EE6B45E 5CA4816B 804000B9 0339F154`. A letra indica a forma (E, S, C ou B), o número indica o nível, e o restante é a semente que gera a tabela. É o que a aba Decriptar usa.
- **Tabela de substituição**, com todos os símbolos e os caracteres que eles representam, para decifrar à mão.

Sem a chave, a mensagem não deve ser entregue junto: por isso o PDF separa a mensagem (página 1) da chave (páginas seguintes).

## Limitações

- Isto é uma **cifra de substituição**, não criptografia moderna. Mesmo no nível difícil, uma análise estatística em texto longo pode quebrá-la. **Não use para proteger informações importantes**, como senhas, dados pessoais ou financeiros.
- A tabela é gerada por um embaralhamento a partir de uma semente. As sementes dos níveis moderado e difícil têm 128 bits, mas o embaralhador usado serve para este fim didático, e não para segurança.
- Nada fica salvo: se você perder a chave, não há como recuperar a mensagem pelo aplicativo.

## Dependências

- [jsPDF](https://github.com/parallax/jsPDF) 2.5.1, carregado por CDN (cdnjs), para gerar o PDF.
- Fontes do Google Fonts (Bricolage Grotesque, Instrument Sans e JetBrains Mono), com fontes do sistema como alternativa.
- O restante é HTML, CSS e JavaScript puros, sem build e sem framework.

Para uso totalmente offline, baixe o `jspdf.umd.min.js` e troque a tag `<script src="https://cdnjs...">` do HTML por uma referência local.

## Estrutura

```
cifrario.html   # aplicação completa (interface, cifras e geração do PDF)
README.md
```

## Publicar no GitHub Pages

1. Renomeie `cifrario.html` para `index.html`.
2. Em **Settings → Pages**, escolha a branch principal e a pasta raiz.
3. O site fica disponível em `https://SEU-USUARIO.github.io/NOME-DO-REPOSITORIO/`.

## Como foi testado

Durante o desenvolvimento, foram feitos testes automatizados de ida e volta (encriptar e decriptar devolve o texto original) em todas as combinações de forma e nível, com textos que incluem acentos, pontuação, quebras de linha e 3000 caracteres. A interface também foi testada com navegador automatizado em largura de celular. A conferência visual do PDF dos modos com emoji e símbolos deve ser feita por quem for usar, porque depende das fontes de cada computador.

## Créditos

- **Ideia, requisitos e orientação:** Tainara
- **Desenvolvimento (código, interface e testes):** Claude, da Anthropic, a partir dessa orientação
