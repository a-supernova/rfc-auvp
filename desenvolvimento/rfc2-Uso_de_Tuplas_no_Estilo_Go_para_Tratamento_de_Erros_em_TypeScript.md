### AUVP - A Única Verdade Possível
### Request for comments: 2
### Categoria: Desenvolvimento de Software

### Autores: `Wendell Guimarães`
 

# RFC2 - Uso de Tuplas no Estilo Go para Tratamento de Erros em TypeScript

### Status: `Proposto`

### Criado em: `01/07/2025`

---

## Sumário

Propõe-se a adoção de um padrão inspirado em Go para o tratamento de erros em TypeScript utilizando tuplas `[resultado, erro]`. O objetivo é oferecer uma alternativa explícita, segura e previsível ao uso de `try/catch`, reduzindo o acoplamento e facilitando a leitura de fluxos assíncronos e síncronos.

---

## Motivação

O TypeScript permite o uso de exceções (`throw`) e blocos `try/catch`, mas essa abordagem tem desvantagens:

- Oculta o fluxo de controle e pode dificultar a leitura.
- Pode capturar erros inesperados, comprometendo a previsibilidade.
- Reduz a clareza do contrato de funções (o tipo de retorno não explicita que pode haver falha).

Exemplo do problema:

```ts

    function multiplicar(a: number, b: number): number {
        try {
            if (typeof a !== 'number' || typeof b !== 'number') {
                throw new Error('Parâmetros inválidos');
            }
            return a * b;
        } catch (error) {
            return 0; // Retorna 0 em caso de erro, mas não informa o erro ao chamador.
            // ou
            throw error; // Lança o erro, mas o chamador precisa envolver a chamada em
        }
    }

    function tabuada(numero: number): number[] { // Por mais que em typescript seja possível definir o tipo de entrada e saída, não é garantido que o dado enviado seja válido.
        const resultado: number[] = [];
        for (let i = 1; i <= 10; i++) {
            try {
                const produto = multiplicar(numero, i);
                resultado.push(produto);
            } catch (error) {
                console.error('Erro ao calcular a tabuada:', error);
                return []; // Retorna um array vazio em caso de erro, mas não informa o erro ao chamador.
                // ou
                throw new Error('Erro ao calcular a tabuada'); // Lança um erro, mas o chamador precisa lidar com isso.
            }
        }
        return resultado;
    }

  
    console.log(tabuada(2)); // Saída: [2, 4, 6, 8, 10, 12, 14, 16, 18, 20]
    console.log(tabuada('a')); // Saída: [] ou erro, nesse caso será necessário envolver a chamada em um try/catch para lidar com o erro.



```


A linguagem Go resolve isso retornando tuplas `(result, err)` e delegando ao chamador a responsabilidade de verificar o erro. Esta RFC propõe trazer esse conceito para o TypeScript.

Exemplo em go:

```go
func parseJSON(json string) (map[string]interface{}, error) {
    var data map[string]interface{}
    err := json.Unmarshal([]byte(json), &data)
    if err != nil {
        return nil, err
    }
    return data, nil
}
```

---

## Exemplo Proposto

### Refatoração do exemplo anterior

```ts
type Result<T> = [T, null] | [null, Error];

function multiplicar(a: number, b: number): Result<number> {
    if (typeof a !== 'number' || typeof b !== 'number') {
        return [null, new Error('Parâmetros inválidos')];
    }
    return [a * b, null];
}
function tabuada(numero: number): Result<number[]> {
    const resultado: number[] = [];
    for (let i = 1; i <= 10; i++) {
        const [produto, erro] = multiplicar(numero, i);
        if (erro) {
            return [null, erro]; // Retorna o erro de forma explícita
        }
        resultado.push(produto);
    }
    return [resultado, null]; // Retorna o resultado sem erro
}

console.log(tabuada(2)); // Saída: [ [ 2, 4, 6, 8, 10, 12, 14, 16, 18, 20 ], null ]
console.log(tabuada('a')); // Saída: [ null, Error: Parâmetros inválidos ]

```

## Benefícios

- Explícito: O contrato da função deixa claro que pode retornar erro.

- Seguro: Evita exceções não tratadas.

- Padronizado: Estilo semelhante ao Go, conhecido por sua clareza no controle de fluxo.

- Composição funcional: Facilita a composição de funções que retornam Result.

## Casos de Uso

- Parsing (JSON.parse, Date.parse)

- Acesso a arquivos / banco de dados

- Requisições HTTP (fetch, axios, etc.)

- Operações assíncronas com await

- Validações de entrada

## Alternativas Consideradas

- Try/Catch padrão: É mais idiomático porem menos explícito, pode resultar nas limitas já descritas (try/catch hell).

-  Uso de Result<T, E> Estilo Rust, com objetos ao invés de tuplas: Embora seja uma alternativa válida, pode ser mais verboso e menos intuitivo para desenvolvedores acostumados com o estilo Go.


## Considerações Finais

Este padrão não substitui completamente o uso de exceções, mas pode ser adotado como convenção em módulos onde a previsibilidade e o controle explícito de erros são importantes. A proposta visa melhorar a robustez e a legibilidade do código TypeScript em aplicações de todos os tamanhos.


