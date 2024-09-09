# atv_automatic_translation

## Exercícios
1. Tente valores diferentes do argumento num_examples na funçãoload_data_nmt. Como isso afeta os tamanhos do vocabulário do idioma de origem e do idioma de destino?
   
**- num_examples pequeno:** Se num_examples for pequeno, a função vai processar poucas linhas do dataset, o que significa que o número de palavras únicas (vocabulário) será limitado. Quanto menos exemplos, menor o vocabulário tanto na língua de origem quanto na de destino.

**- num_examples grande** ou não especificado: À medida que aumenta o valor de num_examples, ou não define um valor (processando o dataset inteiro), mais frases serão incluídas no processamento. Isso aumentará o vocabulário, pois novas palavras serão encontradas em mais exemplos. Dessa forma, o vocabulário de origem e destino será maior.

### Exemplo
```
# Função tokenize_nmt já definida
def tokenize_nmt(text, num_examples=None):
    """Tokenize the English-French dataset."""
    source, target = [], []
    for i, line in enumerate(text.split('\n')):
        if num_examples and i > num_examples:
            break
        parts = line.split('\t')
        if len(parts) == 2:
            source.append(parts[0].split(' '))
            target.append(parts[1].split(' '))
    return source, target

# Função para contar vocabulário
def count_vocab(tokenized_data):
    vocab = set()
    for sentence in tokenized_data:
        vocab.update(sentence)  # Adiciona cada palavra ao conjunto (garantindo unicidade)
    return len(vocab)  # Retorna o número de palavras únicas

# Carregar o dataset de exemplo
text = """
hello how are you?\tbonjour comment ça va ?
i am fine, thank you\tje vais bien, merci
what is your name?\tcomment tu t'appelles ?
my name is John\tje m'appelle John
see you tomorrow\tà demain
good morning\tbonjour
have a nice day\tbonne journée
good night\tbonne nuit
"""

# Testando com diferentes valores de num_examples
for examples in [2, 4, 6, None]:
    source, target = tokenize_nmt(text, num_examples=examples)
    source_vocab_size = count_vocab(source)
    target_vocab_size = count_vocab(target)
    
    print(f"Num examples: {examples}")
    print(f"Source vocab size: {source_vocab_size}")
    print(f"Target vocab size: {target_vocab_size}\n")

RESULTADO
Num examples: 2
Source vocab size: 9
Target vocab size: 9

Num examples: 4
Source vocab size: 16
Target vocab size: 13

Num examples: 6
Source vocab size: 20
Target vocab size: 15

Num examples: None
Source vocab size: 25
Target vocab size: 18

```
2. O texto em alguns idiomas, como chinês e japonês, não tem indicadores de limite de palavras (por exemplo, espaço). A tokenização em nível de palavra ainda é uma boa ideia para esses casos? Por que ou por que não?
   
    A tokenização em nível de palavra não é a melhor abordagem para chinês e japonês, pois esses idiomas não possuem delimitadores de palavras explícitos, como espaços entre as palavras. Um único bloco de texto pode conter várias palavras, mas sem delimitação explícita entre elas, o que torna difícil identificar os limites das palavras de forma direta. Sem espaços, pode haver ambiguidade ao tentar separar palavras.
   
- FONTE NO TEXTO:
```
   Diferente da tokenização em nível de personagem in:numref:sec_language_model, para tradução automática
 preferimos tokenização em nível de palavra aqui (modelos de última geração podem usar técnicas de tokenização
mais avançadas). A seguinte função tokenize_nmt tokeniza os primeiros pares de sequência de texto num_examples,
Onde cada token é uma palavra ou um sinal de pontuação. Esta função retorna duas listas de listas de tokens:
source etarget. Especificamente, source [i] é uma lista de tokens do  sequência de texto no idioma de origem
(inglês aqui) e target [i] é a do idioma de destino (francês aqui).
```

