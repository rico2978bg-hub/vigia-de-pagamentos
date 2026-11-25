# Vigia de Pagamentos

Painel que vigia as transações de cartão de uma orquestradora de pagamentos, encontra o que está fazendo a taxa de aprovação cair e recomenda o que fazer.

**[Abrir o painel](https://rico2978bg-hub.github.io/vigia-de-pagamentos/)**

> Os dados são **100% fictícios**, gerados para um treino de hackathon. Empresas, lojistas e adquirentes são inventados.

## O problema

Em outubro, a aprovação de cartão caiu de **82% para 62%**. Olhando só a média, parecia um problema só. Quando a gente fatia os dados, aparecem **quatro causas de naturezas diferentes**, e cada uma pede um remédio diferente:

| alerta | natureza | o que o painel detecta | ação recomendada |
|---|---|---|---|
| Queda por adquirente + bandeira | infraestrutura | uma combinação (ex.: Adq-B + Mastercard) caiu de ~81% para ~35% em uma semana, enquanto outras adquirentes seguem normais | desviar o tráfego para outra adquirente e abrir chamado |
| Ataque de teste de cartão | fraude | aparelho com 10 ou mais compras em 1 hora, de valor baixo e repetido, de madrugada | bloquear os aparelhos e limitar compras por aparelho |
| Retentativa errada | estratégia | recusa por saldo insuficiente (código 51) retentada em menos de 1h: aprova 5%, contra 41% esperando 24h | retentativa inteligente por código de recusa |
| Loja com chargeback acima de 1% | operação | loja com chargebacks por "produto não recebido" | falar com a loja; endurecer o antifraude não resolve |

## Como funciona

- Um único arquivo, `index.html`, sem servidor e sem dependências. Os alertas são **calculados no navegador** a partir das transações embutidas na página, sem nenhum número fixo no código.
- O gráfico mostra a aprovação por dia, com e sem o ataque.
- Cada alerta traz: o que está acontecendo, o número que prova, o impacto estimado e a ação recomendada.
- **Explicar com IA:** dentro do Claude, um botão transforma cada alerta numa explicação para um diretor. Fora do Claude, por exemplo no GitHub Pages, esse botão fica oculto, e o resto funciona normalmente.

`transacoes.csv` traz os dados brutos: cerca de 10,5 mil transações de 01/09 a 31/10/2026.

## Limitações

- Os impactos em R$ são estimativas pelo ticket médio.
- Não há filtro de datas nem carregamento de um CSV novo pela interface.
- PIX e boleto não entram nos alertas.
