# Interpretação do Q-score para comparação entre interfaces proteína–proteína

## Visão geral

O **Q-score** é uma métrica utilizada para comparar a similaridade entre duas interfaces proteína–proteína.  
Diferentemente de medidas baseadas apenas na superposição global das estruturas, o Q-score busca avaliar se duas interfaces preservam um **padrão semelhante de contatos entre resíduos**.

Em termos intuitivos, o Q-score responde à seguinte pergunta:

> **As duas interfaces aproximam resíduos equivalentes de maneira semelhante?**

Valores de Q-score variam entre **0 e 1**:

- **Q ≈ 0**: interfaces sem similaridade relevante no padrão de contatos.
- **Q ≈ 1**: interfaces com padrão de contatos altamente semelhante.

---

## O que o Q-score mede?

O Q-score compara pares de resíduos em contato nas duas interfaces e leva em conta **as distâncias entre esses resíduos**.

Assim, a métrica não considera apenas se um contato existe ou não, mas também **quão parecida é a geometria desses contatos**.

### Interpretação intuitiva

Imagine duas interfaces:

- se os mesmos resíduos, ou resíduos correspondentes, interagem entre si;
- e se essas interações ocorrem com distâncias semelhantes,

então o Q-score será alto.

Se, por outro lado:

- os contatos forem diferentes,
- ou os mesmos contatos ocorrerem com geometrias muito distintas,

o Q-score será menor.

---

## O que significa "ponderar pelas distâncias"?

Dizer que o Q-score é "ponderado pelas distâncias" significa que nem todos os contatos contribuem da mesma forma para o resultado final.

Por exemplo:

- um contato que aparece nas duas interfaces com distâncias muito parecidas contribui **fortemente** para aumentar o Q-score;
- um contato presente nas duas interfaces, mas com distâncias bem diferentes, contribui **menos**;
- um contato presente em apenas uma das interfaces não contribui para a similaridade.

Em outras palavras, o Q-score leva em conta **tanto a presença dos contatos quanto a semelhança geométrica entre eles**.

---

## Diferença entre Q-score e RMSD

Embora ambos sejam usados para comparar estruturas, **Q-score e RMSD medem coisas diferentes**.

| Métrica | O que mede | Interpretação |
|---|---|---|
| **Q-score** | Conservação do padrão de contatos e da geometria da interface | Indica se duas interfaces interagem de forma semelhante |
| **RMSD** | Desvio médio entre coordenadas atômicas após superposição | Indica o quanto duas estruturas diferem espacialmente |

### Ponto importante

Um **RMSD baixo** não garante que duas interfaces tenham o mesmo padrão de interação.

Da mesma forma, um **Q-score alto** não exige que toda a estrutura tenha RMSD muito baixo.

Isso ocorre porque:

- o **RMSD** depende da superposição espacial de átomos ou resíduos;
- o **Q-score** depende da conservação da **rede de contatos interfaciais**.

### Exemplo conceitual

Duas proteínas podem apresentar pequenas mudanças de orientação relativa ou diferenças fora da interface, aumentando o RMSD global, mas ainda preservar quase o mesmo padrão de contatos na interface.  
Nesse caso, o **Q-score pode permanecer alto mesmo com RMSD moderado**.

---

## Como interpretar os limiares do Q-score?

### Limiar mínimo de similaridade no estudo original

No trabalho que introduziu o uso do Q-score para comparar interfaces, os autores analisaram homodímeros com baixa identidade de sequência e modelaram a distribuição esperada de valores de Q-score entre interfaces arbitrárias.

Eles observaram que:

- quando **Q > 0,08**, a probabilidade de esse valor surgir ao acaso se tornava muito baixa;
- por isso, foi adotado **Q = 0,1** como limiar mínimo para considerar duas interfaces como potencialmente semelhantes.

Portanto:

- **Q ≥ 0,1** sugere que a semelhança observada já é maior do que a esperada ao acaso;
- esse limiar, porém, é **permissivo** e indica apenas uma similaridade mínima.

### Critério mais restritivo usado no ProtCAD

No ProtCAD, foi utilizado um critério muito mais rigoroso:

- interfaces com **Q ≥ 0,75** foram consideradas estruturalmente equivalentes para fins de agrupamento.

Esse limiar não significa apenas "alguma semelhança", mas sim uma **alta conservação do padrão geométrico de interação**.

Em termos práticos, isso indica que as interfaces:

- preservam boa parte dos contatos correspondentes;
- mantêm uma geometria de interação semelhante;
- provavelmente representam o mesmo modo de associação.

---

## Como visualizar isso intuitivamente?

Uma maneira simples de pensar é imaginar que a interface é uma **rede de contatos**.

- Se duas interfaces compartilham apenas alguns contatos vagamente parecidos, o Q-score será baixo.
- Se compartilham muitos contatos com distâncias semelhantes, o Q-score será alto.

### Esquema conceitual

```mermaid
flowchart LR
    A[Interface 1] --> B[Mesmos resíduos em contato?]
    B --> C[Distâncias semelhantes?]
    C --> D[Q-score alto]

    A --> E[Poucos contatos em comum]
    E --> F[Geometria diferente]
    F --> G[Q-score baixo]
