# 🍽️ Cardápio Digital
https://yamanarimatt.github.io/Cardapio-Digital/
Sistema de cardápio digital responsivo, com integração ao WhatsApp e acompanhamento de pedido em tempo real. Arquivo único HTML, sem dependências pagas, sem servidor back-end necessário.

---

## Funcionalidades

- Menu de categorias com navegação por âncora
- Cards de pratos com controle de quantidade inline
- Tags visuais: Destaque, Vegano, Picante, Novo
- Carrinho lateral com campo de observações por item
- Seleção de tipo de atendimento: retirada, entrega ou mesa
- Envio de pedido formatado via WhatsApp
- Timeline de acompanhamento do pedido com 4 etapas
- 100% responsivo — celular e computador

---

## Início rápido

1. Baixe o arquivo `cardapio.html`
2. Abra em qualquer navegador — funciona offline
3. Para colocar online, suba o arquivo em qualquer hospedagem estática (ver seção [Hospedagem](#hospedagem))

Não é necessário instalar nada, configurar banco de dados ou contratar servidor.

---

## Personalização

Todo o conteúdo editável está concentrado em dois blocos no início do arquivo. Você não precisa mexer em mais nada.

### 1. Identidade e cores — bloco CSS `:root`

```css
:root {
  /* 🏪 Nome e slogan */
  --nome-restaurante: "Bistrô Noturno";
  --slogan: "Sabores que contam histórias";

  /* 📱 WhatsApp — apenas números, com DDI e DDD */
  --whatsapp: "5567999999999";

  /* 🎨 Cores */
  --cor-primaria: #1a1a1a;      /* header, botões escuros */
  --cor-secundaria: #f5f0e8;    /* textos sobre fundo escuro */
  --cor-acento: #c8a96e;        /* dourado — destaques */
  --cor-acento2: #8b4513;       /* marrom — preços, links */
  --cor-texto: #1a1a1a;         /* texto geral */
  --cor-fundo: #faf8f4;         /* fundo da página */
  --cor-card: #ffffff;          /* fundo dos cards */
  --cor-borda: #e8e2d9;         /* bordas */

  /* 🖋 Fontes (Google Fonts) */
  --fonte-titulo: 'Fraunces', serif;
  --fonte-corpo: 'DM Sans', sans-serif;
}
```

Para trocar as fontes, substitua os nomes acima por qualquer família do [Google Fonts](https://fonts.google.com) e atualize também o link `<link href="https://fonts.googleapis.com/css2?family=...">` no `<head>`.

### 2. Objeto de configuração — bloco JavaScript `CONFIG`

```js
const CONFIG = {
  nome: "Bistrô Noturno",
  slogan: "Sabores que contam histórias",
  subtitulo: "Peça agora e receba em breve",
  whatsapp: "5567999999999", // DDI + DDD + número, sem espaços
};
```

### 3. Cardápio — array `CARDAPIO`

Cada categoria tem um nome, descrição opcional e uma lista de itens.

```js
const CARDAPIO = [
  {
    categoria: "Entradas",
    descricao: "Para começar bem",  // opcional
    itens: [
      {
        id: 1,                      // número único, não repita
        nome: "Bruschetta ao Tomate",
        desc: "Pão artesanal grelhado com tomate fresco e azeite trufado",
        preco: 28,                  // valor em reais (número, sem R$)
        emoji: "🍞",               // aparece no card e no carrinho
        tags: ["vegano"],           // veja tags disponíveis abaixo
      },
    ]
  },
];
```

**Tags disponíveis**

| Tag | Exibição |
|---|---|
| `"destaque"` | ⭐ Destaque (amarelo) |
| `"vegano"` | 🌿 Vegano (verde) |
| `"picante"` | 🌶️ Picante (vermelho) |
| `"novo"` | ✨ Novo (azul) |

Para não usar nenhuma tag, deixe `tags: []`.

### 4. Usar foto real no lugar do emoji

Adicione o campo `img` ao item com a URL da imagem:

```js
{ id: 5, nome: "Filé ao Molho Madeira", img: "https://seusite.com/foto-file.jpg", ... }
```

A imagem é exibida com altura de 160px e corte automático (`object-fit: cover`). Recomenda-se imagens de no mínimo 600×400px.

### 5. Velocidade da timeline de status

Por padrão, cada etapa do acompanhamento avança a cada 12 segundos. Para ajustar, localize no JavaScript:

```js
}, 12000); // valor em milissegundos
```

Troque `12000` pelo intervalo desejado (ex: `30000` para 30 segundos).

---

## Estrutura do pedido enviado ao WhatsApp

Quando o cliente clica em "Enviar pedido", o WhatsApp abre com uma mensagem no formato:

```
Olá! Gostaria de fazer um pedido 🍽️

Pedido #4821
Cliente: Maria Silva
Tipo: Entrega
Endereço: Rua das Flores, 42, Centro

Itens:
• 2x Risoto de Cogumelos — R$ 144,00
  _(sem parmesão)_
• 1x Petit Gâteau — R$ 34,00

Total: R$ 178,00

_Pedido enviado pelo cardápio digital_
```

O número do pedido é gerado aleatoriamente a cada envio (entre 1000 e 9999).

---

## Tipos de atendimento

O campo "Tipo de atendimento" no carrinho oferece três opções:

- **Retirada no local** — sem campo extra
- **Entrega** — exibe campo de endereço (obrigatório antes de enviar)
- **Mesa (consumo local)** — sem campo extra

Para adicionar ou remover opções, edite o `<select id="tipoAtend">` no HTML.

---

## Hospedagem

O arquivo é estático — qualquer serviço de hospedagem gratuita funciona.

**Opções recomendadas:**

| Serviço | Como usar |
|---|---|
| [GitHub Pages](https://pages.github.com) | Suba o arquivo em um repositório público e ative Pages nas configurações |
| [Netlify](https://netlify.com) | Arraste o arquivo para o painel de deploy |
| [Vercel](https://vercel.com) | Faça upload via CLI ou interface web |
| [Tiiny.host](https://tiiny.host) | Upload direto, sem conta necessária |

Para apontar um domínio próprio (ex: `cardapio.seurestaurante.com.br`), consulte a documentação do serviço escolhido — todos suportam domínios customizados gratuitamente.

---

## Limitações

- O sistema não tem painel administrativo — alterações no cardápio exigem editar o arquivo HTML diretamente.
- Não há integração com sistema de pagamento; o pagamento é combinado via WhatsApp.
- A timeline de status é simulada localmente no navegador do cliente — não há sincronização com o restaurante.
- Sem histórico de pedidos salvo; ao recarregar a página o carrinho é esvaziado.

---

## Ideias de evolução

- Painel admin em outra página HTML para editar o cardápio sem tocar no código
- Integração com Google Sheets para registro dos pedidos
- Campo de cupom de desconto
- Galeria de fotos por item
- Horário de funcionamento com aviso automático de "fechado"

---

## Licença

Uso livre. Modifique, distribua e adapte à vontade.
