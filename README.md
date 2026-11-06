# Triagem Agêntica

Um instrumento de triagem para a entrada de demandas de IA generativa. Responde duas
perguntas, nesta ordem: **que nível de autonomia o problema realmente exige** e, se houver
agente mesmo, **como construir**. Onde a solução roda decorre da segunda.

A página é um arquivo único, sem servidor e sem cadastro. O que você responde fica no seu
navegador e não sai dele.

👉 **[Abrir a ferramenta](https://hpesanto.github.io/triagem-agentica/)**

---

## Por que existe

A maioria das demandas que chega rotulada como agêntica não precisa de um agente. Precisa de
uma chamada ao modelo, ou de um fluxo determinístico com o modelo em um dos passos.

A diferença não é estética. Cada degrau de autonomia multiplica custo e cauda de latência, e
troca teste determinístico por avaliação estatística — que é custo de sustentação permanente,
não esforço de implantação.

## A escada

| Nível | O que é | Quem decide o caminho |
|---|---|---|
| **L0** Determinístico | Regra, consulta, API, fluxo fixo. Sem modelo | O código |
| **L1** Modelo como função | Uma chamada. Sem ferramenta, sem estado, sem laço | O código |
| **L2** Fluxo com modelo | Passos conhecidos em desenho. **RAG puro é aqui** | O código |
| **L3** Agente | O modelo escolhe qual ferramenta, em que ordem, quando parar | O modelo |
| **L4** Multiagente | Governança federada: cada agente com dono que veta | Os modelos, entre si |

Três enganos de nomenclatura que o instrumento tenta desfazer:

- **RAG puro é L2, não L3.** Buscar e gerar são dois passos conhecidos.
- **Roteador sozinho não é agência.** É um classificador L1 mais um `switch`.
- **L4 é nível de governança**, não de topologia nem de protocolo. Vários agentes não fazem
  L4; o que faz é existir mais de um dono que precise poder vetar mudança no próprio agente.

## Como funciona

**Gate A — nível.** Nove perguntas, cada uma estabelecendo um piso. O nível é o maior piso
acionado. Separadamente, cinco incompatibilidades duras que viram débito arquitetural quando
presentes em L3 ou L4.

**Gate B — via de construção.** Só se aplica de L3 em diante. Nove eliminatórias que decidem
sozinhas — seis forçam a via aberta, três forçam a gerenciada. Sem eliminatória, oito
discriminadores pontuados, com zona cinza resolvendo a favor da via gerenciada, porque a
promoção depois é um caminho conhecido e o inverso é desperdício.

**Saída.** Um registro de decisão copiável, com os débitos abertos e o que fecha cada um.

## O gate é orientativo

Ele não barra demanda. Produz o veredito e, quando a equipe decide diferente, registra o que
foi trocado e por quê. A hipótese por trás disso: gate com poder de veto tende a virar
burocracia contornada, enquanto gate que torna a troca visível tende a sobreviver.

## Como adaptar

Os limiares são um ponto de partida defensável, não uma verdade. O saldo de corte de ±3 nos
discriminadores, o teto de latência e as faixas de custo relativo devem ser recalibrados com
os números da sua operação.

O que vale preservar é a estrutura: pisos que estabelecem o nível mínimo, eliminatórias que
decidem sozinhas, discriminadores que só pesam quando nenhuma eliminatória acionou, e um
registro que torna visível quando alguém decide diferente.

Para adaptar, edite `index.html`. As listas de perguntas, incompatibilidades, eliminatórias e
discriminadores estão no topo do `<script>`, em arrays nomeados (`QS`, `INC`, `E2`, `E1L`,
`DISC`), e os limiares estão em `levelOf()` e `viaOf()`.

## Licença

MIT — veja `LICENSE`. Use, adapte e redistribua à vontade.
