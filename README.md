[ebook_android_moderno_kotlin_jetpack.md](https://github.com/user-attachments/files/26862014/ebook_android_moderno_kotlin_jetpack.md)
# Estudos-Kotlin# GUIA DEFINITIVO DE DESENVOLVIMENTO ANDROID MODERNO

## Kotlin, Jetpack Compose e Arquitetura Profissional

---

**[CAPA]**

```
┌─────────────────────────────────────────┐
│                                         │
│     GUIA DEFINITIVO DE                  │
│     DESENVOLVIMENTO ANDROID             │
│     MODERNO                             │
│                                         │
│        Kotlin + Jetpack Compose         │
│        + Arquitetura Profissional        │
│                                         │
│     Do Iniciante ao Avançado            │
│                                         │
│           ─────────────                 │
│                                         │
│     [ilustração: smartphone com         │
│      tela de app Android moderna        │
│      mostrando UI bonita]               │
│                                         │
│                                         │
└─────────────────────────────────────────┘
```

---

# SUMÁRIO

1. Fundamentos do Kotlin
2. Jetpack Compose
3. Arquitetura
4. Injeção de Dependência
5. Networking
6. Coroutines e Flow
7. Persistência
8. Navegação
9. Testes
10. Princípios de Software
11. Projeto Completo

---

# MÓDULO 1 — FUNDAMENTOS DO KOTLIN

Kotlin é a linguagem oficial para desenvolvimento Android. Criada pela JetBrains, ela oferece uma sintaxe concisa, moderna e segura que melhora significativamente a produtividade do desenvolvedor.

## 1.1 Sintaxe Básica

Kotlin foi pensada para ser concisa e expressiva. Vamos entender os conceitos fundamentais.

### Hello World em Kotlin

```
fun main() {
    println("Hello, Android!")
}
```

A função `main` é o ponto de entrada de todo programa Kotlin. Em Android, usamos Activity, Application ou Compose como pontos de entrada.

### Variáveis declaradas com `val` e `var`

```
val nome = "Android"      // Imutável (não pode ser重新atribuído)
var idade = 10            // Mutável (pode ser alterado)
nome = "Novo nome"       // ERRO! Não compila
idade = 11               // OK
```

**Regra de ouro**: Sempre use `val` por padrão. Só use `var` quando realmente precisar mudar o valor.

### Inferência de tipos

Kotlin reconhece o tipo automaticamente:

```
val texto = "Hello"       // Tipo: String
val numero = 42          // Tipo: Int
val decimal = 3.14       // Tipo: Double
val ativo = true         // Tipo: Boolean
```

Ainda assim, podemos declarar o tipo explicitamente:

```
val texto: String = "Hello"
val numero: Int = 42
val decimal: Double = 3.14
val ativo: Boolean = true
```

## 1.2 Tipos de Dados

Kotlin possui tipos básicos semelhantes a outras linguagens.

### Tipos Numéricos

```
val inteiro: Int = 100                // Inteiro de 32 bits
val longo: Long = 1_000_000L          // Inteiro de 64 bits
val double: Double = 3.14159          // Ponto flutuante 64 bits
val float: Float = 3.14f              // Ponto flutuante 32 bits
val short: Short = 10                // Inteiro de 16 bits
val byte: Byte = 127                  // Inteiro de 8 bits
```

### Tipos de Texto

```
val textoNormal: String = "Android"
val textoMultilinha = """
    Este é um texto
    de múltiplas
    linhas
""".trimIndent()
```

### Caracteres

```
val letra: Char = 'A'
val emoji: Char = '\u1F600'  // 😀
```

### Booleanos

```
val VERDADEIRO = true
val FALSO = false
```

### Arrays

```
val numeros = arrayOf(1, 2, 3, 4, 5)
val misto = arrayOf("texto", 42, true)

// Arrays tipados
val ints: IntArray = intArrayOf(1, 2, 3)
val strings: Array<String> = arrayOf("a", "b", "c")
```

## 1.3 Null Safety (Tratamento de Nulos)

Esta é uma das features mais importantes do Kotlin — eliminar o temido NullPointerException.

### O problema dos nulos

Em Java, qualquer objeto pode ser nulo e isso causa crashes:

```
// Java
String texto = null;
texto.length();  // NullPointerException! 💥
```

### Tipos Nullable

Em Kotlin, você precisa declarar explicitamente quando algo pode ser nulo usando `?`:

```
var nome: String = "Android"     // NÃO pode ser nulo
var nomeNulo: String? = null    // PODE ser nulo
```

### Operador de chamada segura `?.`

Executa a ação apenas se o objeto não for nulo:

```
var texto: String? = "Hello"
println(texto?.length)  // Imprime 5

texto = null
println(texto?.length)  // Imprime null (não dá crash!)
```

### Operador Elvis `?:`

Fornece um valor padrão se for nulo:

```
var texto: String? = null
println(texto?.length ?: 0)  // Imprime 0
```

### Operador `!!` (Not-null assertion)

Use apenas quando TEM CERTEZA que não é nulo:

```
var texto: String = "Hello"
println(texto!!.length)  // Imprime 5 — funciona

var textoNulo: String? = null
println(textoNulo!!.length)  // 💥 NullPointerException!
```

**Atenção**: Evite `!!` sempre que possível!

## 1.4 Controle de Fluxo

### if / else

```
val nota = 85

if (nota >= 90) {
    println("A")
} else if (nota >= 80) {
    println("B")
} else if (nota >= 70) {
    println("C")
} else {
    println("Reprovado")
}

// Expressão if (retorna valor)
val resultado = if (nota >= 60) "Aprovado" else "Reprovado"
```

### when (switch evoluído)

O `when` substitui o switch de outras linguagens:

```
val dia = 3

when (dia) {
    1 -> println("Domingo")
    2 -> println("Segunda")
    3 -> println("Terça")
    4 -> println("Quarta")
    5 -> println("Quinta")
    6 -> println("Sexta")
    7 -> println("Sábado")
    else -> println("Inválido")
}

// Com múltiplas condições
val x = 5
when (x) {
    1, 2, 3 -> println("Primeiros números")
    in 4..10 -> println("Entre 4 e 10")
    else -> println("Outro")
}

// When como expressão
val resultado = when (dia) {
    1 -> "Domingo"
    2 -> "Segunda"
    else -> "Outro dia"
}
```

## 1.5 Funções

### Funções básicas

```
fun saudar(nome: String) {
    println("Olá, $nome!")
}

fun soma(a: Int, b: Int): Int {
    return a + b
}
```

### Funções de expressão única

Quando a função tem apenas uma expressão, podemos simplificar:

```
fun soma(a: Int, b: Int): Int = a + b

// Com inferência de retorno
fun dobro(a: Int) = a * 2
```

### Parâmetros com valor padrão

```
fun saudar(nome: String, saudacao: String = "Olá") {
    println("$saudacao, $nome!")
}

saudar("Android")           // Olá, Android!
saudar("iOS", "Bem-vindo")  // Bem-vindo, iOS!
```

### Parâmetros nomeados

```
fun criarUsuario(
    nome: String,
    email: String,
    idade: Int
) {
    println("$nome, $email, $idade anos")
}

criarUsuario(nome = " João", idade = 25, email = "joao@email.com")
```

### Funções de ordem superior

Funções que recebem ou retornam outras funções:

```
fun operacao(a: Int, b: Int, funcao: (Int, Int) -> Int): Int {
    return funcao(a, b)
}

val resultado = operacao(10, 5) { a, b -> a + b }
```

## 1.6 Lambdas

Lambdas são funções anônimas:

```
// Sintaxe básica
val soma = { a: Int, b: Int -> a + b }

// Chamando
val resultado = soma(10, 5)  // 15

// Lambda como parâmetro
fun listaDeNumeros(lambda: (Int) -> Int) {
    println(lambda(5))
}

listaDeNumeros { it * 2 }  // 10
```

### It — parâmetros implícito

Quando a lambda tem apenas um parâmetro, usamos `it`:

```
val numeros = listOf(1, 2, 3, 4, 5)

// Dobrar cada número
val dobrados = numeros.map { it * 2 }  // [2, 4, 6, 8, 10]

// Filtrar pares
val pares = numeros.filter { it % 2 == 0 }  // [2, 4]
```

## 1.7 Programação Orientada a Objetos (POO)

### Classes básicas

```
class Pessoa {
    var nome: String = ""
    var idade: Int = 0
    
    fun apresentar() {
        println("Olá, meu nome é $nome e tenho $idade anos")
    }
}

val joao = Pessoa()
joao.nome = "João"
joao.idade = 25
joao.apresentar()
```

### Construtor primário

```
class Pessoa(val nome: String, var idade: Int)

val joao = Pessoa("João", 25)
println(joao.nome)  // João
```

### Construtor secundário

```
class Pessoa {
    var nome: String
    var idade: Int
    
    constructor(nome: String) {
        this.nome = nome
        this.idade = 0
    }
    
    constructor(nome: String, idade: Int) {
        this.nome = nome
        this.idade = idade
    }
}
```

### Herança

```
open class Animal(val nome: String) {
    open fun som() {
        println("Algum som")
    }
}

class Cao(nome: String) : Animal(nome) {
    override fun som() {
        println("Au au!")
    }
}

val dog = Cao("Rex")
dog.som()  // Au au!
```

### Polimorfismo

```
fun fazerSom(animal: Animal) {
    animal.som()
}

val cao = Cao("Rex")
val passaro = Passaro("Piu")

fazerSom(cao)     // Au au!
fazerSom(passaro) // Piu piu!
```

## 1.8 Data Class

Classes especializadas para armazenar dados:

```
data class Usuario(
    val id: Int,
    val nome: String,
    val email: String
)

val usuario = Usuario(1, "João", "joao@email.com")

// Equals gerado automaticamente
val usuario2 = Usuario(1, "João", "joao@email.com")
println(usuario == usuario2)  // true

// toString formatado
println(usuario)
// Usuario(id=1, nome=João, email=joao@email.com)

// copy
val usuarioCopy = usuario.copy(nome = "Maria")
// Usuario(id=1, nome=Maria, email=joao@email.com)

// componentN
val (id, nome, email) = usuario
```

## 1.9 Sealed Classes

Classes que limitam a hierarquia:

```
sealed class Resultado {
    class Sucesso(val dados: String) : Resultado()
    class Erro(val mensagem: String) : Resultado()
    object Carregando : Resultado()
}

fun tratarResultado(resultado: Resultado) {
    when (resultado) {
        is Resultado.Sucesso -> println(resultado.dados)
        is Resultado.Erro -> println(resultado.mensagem)
        is Resultado.Carregando -> println("Carregando...")
    }
}
```

Excelente para tratamento de estados e erros.

## 1.10 Enum Classes

```
enum class DiaSemana {
    DOMINGO, SEGUNDA, TERÇA, QUARTA, QUINTA, SEXTA, SÁBADO
}

enum class StatusHTTP(val codigo: Int) {
    OK(200),
    CREATED(201),
    BAD_REQUEST(400),
    NOT_FOUND(404),
    ERRO_INTERNO(500)
}

fun getDescricao(status: StatusHTTP): String {
    return when (status) {
        StatusHTTP.OK -> "Sucesso"
        StatusHTTP.CREATED -> "Criado"
        StatusHTTP.BAD_REQUEST -> "Requisição inválida"
        StatusHTTP.NOT_FOUND -> "Não encontrado"
        StatusHTTP.ERRO_INTERNO -> "Erro interno"
    }
}
```

## 1.11 Funções de Escopo

Kotlin oferece funções de escopo que facilitam a manipulação de objetos:

### let

Executa código apenas se o objeto não for nulo:

```
var texto: String? = "Hello"
texto?.let {
    println("O texto tem ${it.length} caracteres")
}

texto = null
texto?.let {
    println("Isso não será executado")
}

// ú  til para transformação
val maiusculas = texto?.let { it.uppercase() }
```

### apply

Executa código no contexto do objeto e retorna o mesmo objeto:

```
val pessoa = Pessoa().apply {
    nome = "João"
    idade = 25
}
// Pessoa(nome=João, idade=25)
```

Perfeito para configuração de objetos.

### also

Semelhante ao apply, mas refere-se ao objeto com `it`:

```
val lista = mutableListOf<Int>().also {
    it.add(1)
    it.add(2)
    it.add(3)
}
```

Útil para ações adicionales sem alterar o retorno.

### run

Executa código no contexto de um objeto e retorna o resultado:

```
val resultado = objeto.run {
    // faz algo
    valorFinal
}
```

Útil para execução de blocos de código.

### with

Semelhante a run, mas como função livre:

```
val resultado = with(pessoa) {
    nome + " tem " + idade + " anos"
}
```

## 1.12 Coleções

### List

```
val lista = listOf(1, 2, 3, 4, 5)

// Acessar elementos
println(lista[0])  // 1
println(lista.first())  // 1
println(lista.last())  // 5

// Operações
println(lista.map { it * 2 })  // [2, 4, 6, 8, 10]
println(lista.filter { it > 2 })  // [3, 4, 5]
println(lista.reduce { acc, i -> acc + i })  // 15

// Mutable list
val listaMutavel = mutableListOf(1, 2, 3)
listaMutavel.add(4)
```

### Map (Dicionário)

```
val mapa = mapOf(
    "chave1" to "valor1",
    "chave2" to "valor2"
)

// Acessar
println(mapa["chave1"])  // valor1
println(mapa.getOrDefault("chave3", "padrão"))  // padrão

// Iterar
for ((chave, valor) in mapa) {
    println("$chave -> $valor")
}

// Mutable map
val mapaMutavel = mutableMapOf<String, Any>()
mapaMutavel["nome"] = "João"
```

### Set (Conjunto único)

```
val conjunto = setOf(1, 2, 3, 2, 1)
// {1, 2, 3} — elementos únicos

println(1 in conjunto)  // true
println(conjunto.contains(4))  // false
```

### Sequências (Lazy evaluation)

```
val resultado = lista
    .asSequence()
    .filter { it > 2 }
    .map { it * 10 }
    .first()
```

---

# MÓDULO 2 — JETPACK COMPOSE

Jetpack Compose é o toolkit moderno de UI do Android. Ele simplifica e acelera o desenvolvimento de interfaces com código declarativo.

## 2.1 Conceitos Básicos

### O que é Compose?

Compose é um toolkit de UI declarativo que permite construir natvas Android com menos código.

comparação:

```
// XML tradicional (OLD)
<Button
    android:id="@+id/btn_salvar"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Salvar"
    android:onClick="salvar"
/>

// Compose (MODERNO)
Button(onClick = { salvar() }) {
    Text("Salvar")
}
```

### @Composable

Funções que descrevem a UI:

```
@Composable
fun Saudacao() {
    Text("Olá, Android!")
}
```

Toda função que cria UI deve ter a anotação `@Composable`.

### Preview (Visualização)

```
@Preview
@Composable
fun SaudacaoPreview() {
    Saudacao()
}
```

Permite visualizar o componente no Android Studio.

## 2.2 Estado e Recomposição

### O problema do estado

Em XML tradicional, o estado era gerenciado manualmente. No Compose, mudamos o estado e a UI se atualiza automaticamente.

### remember e mutableStateOf

```
@Composable
fun Contador() {
    var contador by remember { mutableStateOf(0) }
    
    Column {
        Text("Contagem: $contador")
        Button(onClick = { contador++ }) {
            Text("Incrementar")
        }
    }
}
```

Entendendo:
- `mutableStateOf(0)` cria um estado observable
- `remember` mantém o estado durante recomposições
- `by` permite usar sem `.value`

### Estado com classe

```
data class EstadoTela(
    val nome: String = "",
    val email: String = "",
    val isLoading: Boolean = false,
    val erro: String? = null
)

@Composable
fun Formulario() {
    var estado by remember { mutableStateOf(EstadoTela()) }
    
    Column {
        if (estado.isLoading) {
            CircularProgressIndicator()
        } else {
            TextField(
                value = estado.nome,
                onValueChange = { estado = estado.copy(nome = it) },
                label = { Text("Nome") }
            )
        }
    }
}
```

## 2.3 Layouts

### Column (Layout vertical)

```
@Composable
fun MeuLayout() {
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text("Item 1")
        Text("Item 2")
        Text("Item 3")
    }
}
```

### Row (Layout horizontal)

```
@Composable
fun Linha() {
    Row(
        modifier = Modifier.fillMaxWidth(),
        horizontalArrangement = Arrangement.SpaceBetween
    ) {
        Text("Esquerda")
        Text("Centro")
        Text("Direita")
    }
}
```

### Box (Layout livre)

```
@Composable
fun Caixa() {
    Box(
        modifier = Modifier.fillMaxSize(),
        contentAlignment = Alignment.Center
    ) {
        Text("Centralizado")
    }
}
```

### LazyColumn (Lista virtualizada)

```
@Composable
fun ListaNumeros() {
    val itens = (1..100).toList()
    
    LazyColumn(
        modifier = Modifier.fillMaxSize(),
        verticalArrangement = Arrangement.spacedBy(8.dp)
    ) {
        items(itens) { numero ->
            Text(
                text = "Item $numero",
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(horizontal = 16.dp)
            )
        }
    }
}
```

### LazyGrid (Grid virtualizado)

```
@Composable
fun Grade Numeros() {
    val itens = (1..20).toList()
    
    LazyVerticalGrid(
        columns = GridCells.Fixed(3),
        horizontalArrangement = Arrangement.spacedBy(8.dp),
        verticalArrangement = Arrangement.spacedBy(8.dp)
    ) {
        items(itens) { numero ->
            Text(
                text = "$numero",
                modifier = Modifier
                    .size(100.dp)
                    .background(Color.LightGray),
                contentAlignment = Alignment.Center
            )
        }
    }
}
```

## 2.4 Modifier (Modificadores)

O Modifier é клюve para customizar elementos:

### Padding e Margin

```
Text(
    text = "texto",
    modifier = Modifier
        .padding(16.dp)           // externo
        .padding(start = 8.dp)   // específico
)
```

### Background

```
Box(
    modifier = Modifier
        .background(Color.Blue)
        .size(100.dp)
)
```

### Tamanho

```
Box(
    modifier = Modifier
        .width(100.dp)
        .height(50.dp)
        .size(width = 100.dp, height = 50.dp)  // combinação
        .fillMaxSize()                       // ocupa todo o espaço
)
```

### Clickable

```
Text(
    text = "Clique aqui",
    modifier = Modifier.clickable { 
        // ação
    }
)
```

### Border

```
Card(
    modifier = Modifier.border(
        width = 2.dp,
        color = Color.Black,
        shape = RoundedCornerShape(8.dp)
    )
)
```

### Combinação de modifiers

```
Modifier
    .fillMaxWidth()
    .padding(16.dp)
    .background(Color.White)
    .border(1.dp, Color.Gray)
    .clickable { ... }
    .padding(16.dp)
```

## 2.5 Material 3

Material Design 3 é o sistema de design moderno do Google:

### Cores (Color Scheme)

```
val colorScheme = ColorScheme(
    primary = Color(0xFF6750A4),
    onPrimary = Color.White,
    primaryContainer = Color(0xFFEADDFF),
    onPrimaryContainer = Color(0xFF21005D),
    secondary = Color(0xFF625B71),
    onSecondary = Color.White,
    secondaryContainer = Color(0xFFE8DEF8),
    onSecondaryContainer = Color(0xFF1D192B)
)
```

### Tipografia

```
Typography(
    displayLarge = TextStyle(
        fontSize = 57.sp,
        fontWeight = FontWeight.Normal
    ),
    headlineMedium = TextStyle(
        fontSize = 28.sp,
        fontWeight = FontWeight.Normal
    ),
    bodyLarge = TextStyle(
        fontSize = 16.sp,
        fontWeight = FontWeight.Normal
    ),
    labelSmall = TextStyle(
        fontSize = 11.sp,
        fontWeight = FontWeight.Medium
    )
)
```

### Componentes Material 3

#### Button

```
Button(
    onClick = { /* ação */ },
    enabled = true
) {
    Text("Botão primário")
}
```

#### OutlinedButton

```
OutlinedButton(onClick = { /* ação */ }) {
    Text("Botão outline")
}
```

#### TextButton

```
TextButton(onClick = { /* ação */ }) {
    Text("Botão de texto")
}
```

#### FloatingActionButton

```
FloatingActionButton(
    onClick = { /* ação */ },
    containerColor = MaterialTheme.colorScheme.primaryContainer
) {
    Icon(Icons.Default.Add, contentDescription = "Adicionar")
}
```

#### Card

```
Card(
    modifier = Modifier.fillMaxWidth(),
    elevation = CardDefaults.cardElevation(defaultElevation = 4.dp)
) {
    Column(modifier = Modifier.padding(16.dp)) {
        Text("Título")
        Text("Conteúdo")
    }
}
```

#### TextField

```
var texto by remember { mutableStateOf("") }

OutlinedTextField(
    value = texto,
    onValueChange = { texto = it },
    label = { Text("Nome") },
    placeholder = { Text("Digite seu nome") },
    leadingIcon = { Icon(Icons.Default.Person, null) },
    trailingIcon = { Icon(Icons.Default.Close, null) },
    isError = false,
    singleLine = true
)
```

#### Snackbar

```
val snackbarHostState = remember { SnackbarHostState() }

Scaffold(
    snackbarHost = { SnackbarHost(snackbarHostState) }
) { padding ->
    LaunchedEffect(Unit) {
        snackbarHostState.showSnackbar("Mensagem")
    }
}
```

#### BottomNavigation

```
val navController = rememberNavController()

Scaffold(
    bottomBar = {
        NavigationBar {
            NavigationBarItem(
                icon = { Icon(Icons.Default.Home, null) },
                label = { Text("Início") },
                selected = true,
                onClick = { }
            )
            NavigationBarItem(
                icon = { Icon(Icons.Default.Search, null) },
                label = { Text("Buscar") },
                selected = false,
                onClick = { }
            )
        }
    }
) { }
```

## 2.6 Temas e Cores

### Criando um tema

```
@Composable
fun MeusTema(
    darkTheme: Boolean = isSystemInDarkTheme(),
    content: @Composable () -> Unit
) {
    val colorScheme = when {
        darkTheme -> DarkColorScheme
        else -> LightColorScheme
    }
    
    MaterialTheme(
        colorScheme = colorScheme,
        typography = Typography,
        content = content
    )
}
```

### Usando cores do tema

```
@Composable
fun Componente() {
    Text(
        text = "Texto",
        color = MaterialTheme.colorScheme.primary
    )
}
```

### Suporte a Dark Mode

```
@Composable
fun TemaEscuro() {
    val darkTheme = isSystemInDarkTheme()
    val surfaceColor by animateColorAsState(
        targetValue = if (darkTheme) Color.Black else Color.White,
        label = "surface"
    )
}
```

## 2.7 Performance e Recomposição

### O que é recomposição?

Recomposição é quando o Compose re-executa a função @Composable para atualizar a UI quando o estado muda.

### Melhores práticas

#### Use remember para estados que não mudam frequentemente

```
@Composable
fun Tela() {
    var estado by remember { mutableStateOf(initial)
    // ✅ Bom: estado não Recria a cada render
}
```

#### Evite recomposições desnecessárias

```
@Composable
fun TelaIneficiente() {
    val lista = listOf(1, 2, 3)
    
    LazyColumn {
        items(lista) { item ->
            // Problema: nova lista a cada recomposição
        }
    }
}

// Solução: remember
@Composable
fun TelaEficiente() {
    val lista = remember { listOf(1, 2, 3) }
    // ✅ Melhor: mesma lista entre recomposições
}
```

#### Use key para listas

```
LazyColumn {
    items(
        items = lista,
        key = { it.id }  // ✅ Melhora performance
    ) { item ->
        // conteúdo
    }
}
```

#### Evite lambdas no Modifier

```
@Composable
fun Evitar() {
    // ❌ Problema: nova lambda a cada recomposição
    Button(onClick = { executar() }) {
        Text("Clique")
    }
}

// Solução
@Composable
fun Solucao() {
    val onClick = remember { { executar() } }
    // ✅ Lambda estável
    Button(onClick = onClick) {
        Text("Clique")
    }
}
```

### Diagnostics no Android Studio

Use "Recomposition counter" no Android Studio para identificar recomposições frequentes.

---

# MÓDULO 3 — ARQUITETURA

Arquitetura é a organização do código. Uma boa arquitetura facilita manutenção, testes e evolu ção do app.

## 3.1 MVVM (Model-View-ViewModel)

MVVM é o padrão mais usado em Android:

[DIAGRAMA: seta do View para ViewModel (ação do usuário), seta do ViewModel para View (estado atualizado), seta do ViewModel para Model (dados)]

### View

Responsável apenas pela apresentação:

```
@Composable
fun TelaUsuario(
    viewModel: UsuarioViewModel = viewModel()
) {
    val estado by viewModel.estado.collectAsState()
    
    Column {
        when {
            estado.isLoading -> CircularProgressIndicator()
            estado.erro != null -> Text(estado.erro!!)
            else -> {
                Text(estado.usuario?.nome ?: "")
                Text(estado.usuario?.email ?: "")
            }
        }
    }
}
```

### ViewModel

Gerencia o estado e lógica de negócio:

```
@HiltViewModel
class UsuarioViewModel @Inject constructor(
    private val repository: UsuarioRepository
) : ViewModel() {
    
    private val _estado = MutableStateFlow(EstadoUsuario())
    val estado: StateFlow<EstadoUsuario> = _estado
    
    init {
        carregarUsuario()
    }
    
    private fun carregarUsuario() {
        _estado.value = _estado.value.copy(isLoading = true)
        
        viewModelScope.launch {
            try {
                val usuario = repository.buscarUsuario()
                _estado.value = _estado.value.copy(
                    usuario = usuario,
                    isLoading = false
                )
            } catch (e: Exception) {
                _estado.value = _estado.value.copy(
                    erro = e.message,
                    isLoading = false
                )
            }
        }
    }
}
```

### Model/Repository

acessa dados (API, banco, etc):

```
class UsuarioRepository @Inject constructor(
    private val api: UsuarioApi
) {
    suspend fun buscarUsuario(): Usuario {
        return api.buscarUsuario()
    }
}
```

### Estado

```
data class EstadoUsuario(
    val usuario: Usuario? = null,
    val isLoading: Boolean = false,
    val erro: String? = null
)
```

## 3.2 MVI (Model-View-Intent)

MVI é um padrão mais robusto que oferece previsibilidade completa do fluxo de dados.

[DIAGRAMA: UI → Intent/Actions → Reducer → Estado → UI (ciclo completo)]

### Conceitos fundamentais

#### State (Estado)

Estado único e imutável da UI:

```
data class EstadoTela(
    val isLoading: Boolean = false,
    val listaItens: List<Item> = emptyList(),
    val itemSelecionado: Item? = null,
    val erro: String? = null,
    val filtro: TipoFiltro = TipoFiltro.TODOS
)
```

#### Intent/Action (Intenção/Ação)

Ações que o usuário realiza:

```
sealed class Intencao {
    object CarregarLista : Intencao()
    data class SelecionarItem(val item: Item) : Intencao()
    data class Filtrar(val tipo: TipoFiltro) : Intencao()
    object LimparErro : Intencao()
    object AtualizarDados : Intencao()
}
```

#### Reducer (Transformador)

Função pura que transforma o estado baseado na ação:

```
fun reducer(estado: EstadoTela, acao: Acao): EstadoTela {
    return when (acao) {
        is Acao.Carregando -> estado.copy(
            isLoading = true,
            erro = null
        )
        is Acao.Sucesso -> estado.copy(
            isLoading = false,
            listaItens = acao.itens
        )
        is Acao.Erro -> estado.copy(
            isLoading = false,
            erro = acao.mensagem
        )
        is Acao.SelecionarItem -> estado.copy(
            itemSelecionado = acao.item
        )
        is Acao.Filtrar -> estado.copy(
            filtro = acao.tipo,
            listaItens = filtrarLista(estado.listaItens, acao.tipo)
        )
    }
}
```

#### Effects/Side Effects (Efeitos)

Eventos únicos que não fazem parte do estado (navegação, toasts, etc):

```
sealed class Efeito {
    data class NavegarPara(val tela: Tela) : Efeito()
    data class MostrarSnackbar(val mensagem: String) : Efeito()
    object MostrarDialogSair : Efeito()
    data class CopiarTexto(val texto: String) : Efeito()
}
```

### Implementação completa MVI

#### ViewModel MVI

```
@HiltViewModel
class TarefaViewModel @Inject constructor(
    private val repository: TarefaRepository
) : ViewModel() {
    
    private val _estado = MutableStateFlow(EstadoTela())
    val estado: StateFlow<EstadoTela> = _estado
    
    private val _efeitos = MutableSharedFlow<Efeito>()
    val efeitos: SharedFlow<Efeito> = _efeitos
    
    fun processarIntencao(intencao: Intencao) {
        when (intencao) {
            is Intencao.CarregarTarefas -> carregarTarefas()
            is Intencao.AdicionarTarefa -> adicionarTarefa(intencao.descricao)
            is Intencao.ConcluirTarefa -> concluirTarefa(intencao.id)
            is Intencao.ExcluirTarefa -> excluirTarefa(intencao.id)
            is Intencao.SelecionarTarefa -> selecionarTarefa(intencao.tarefa)
            is Intencao.NavegarParaDetalhe -> navegarParaDetalhe(intencao.tarefa)
            is Intencao.LimparErro -> reduzir(Acao.LimparErro)
        }
    }
    
    private fun carregarTarefas() {
        viewModelScope.launch {
            reduzir(Acao.Carregando)
            try {
                val tarefas = repository.buscarTarefas()
                reduzindo(Acao.Sucesso(tarefas))
            } catch (e: Exception) {
                reduzir(Acao.Erro(e.message ?: "Erro desconhecido"))
            }
        }
    }
    
    private fun adicionarTarefa(descricao: String) {
        viewModelScope.launch {
            reduzir(Acao.Carregando)
            try {
                val novaTarefa = Tarefa(id = UUID.randomUUID(), descricao = descricao)
                repository.salvarTarefa(naTaTarefa)
                _efeitos.emit(Efeito.MostrarSnackbar("Tarefa adicionada!"))
                carregarTarefas()
            } catch (e: Exception) {
                reduzir(Acao.Erro(e.message ?: "Erro ao adicionar"))
            }
        }
    }
    
    private fun concluirTarefa(id: String) {
        viewModelScope.launch {
            try {
                repository.concluirTarefa(id)
                _efeitos.emit(Efeito.MostrarSnackbar("Tarefa concluída!"))
                carregarTarefas()
            } catch (e: Exception) {
                reduzir(Acao.Erro(e.message ?: "Erro ao concluir"))
            }
        }
    }
    
    private fun excluirTarefa(id: String) {
        viewModelScope.launch {
            try {
                repository.excluirTarefa(id)
                _efeitos.emit(Efeito.MostrarSnackbar("Tarefa excluída"))
                carregarTarefas()
            } catch (e: Exception) {
                reduzir(Acao.Erro(e.message ?: "Erro ao excluir"))
            }
        }
    }
    
    private fun selecionarTarefa(tarefa: Tarefa) {
        viewModelScope.launch {
            reduzir(Acao.SelecionarTarefa(tarefa))
        }
    }
    
    private fun navegarParaDetalhe(tarefa: Tarefa) {
        viewModelScope.launch {
            _efeitos.emit(Efeito.NavegarPara(TelaDetalhe(tarefa.id)))
        }
    }
    
    private fun reduzir(acao: Acao) {
        val novoEstado = reducer(_estado.value, acao)
        _estado.value = novoEstado
    }
}
```

#### Tela MVI

```
@Composable
fun TelaTarefas(
    viewModel: TarefaViewModel = viewModel()
) {
    val estado by viewModel.estado.collectAsState()
    
    LaunchedEffect(Unit) {
        viewModel.efeitos.collect { efeito ->
            when (efeito) {
                is Efeito.MostrarSnackbar -> {
                    snackbarHostState.showSnackbar(efeito.mensagem)
                }
                is Efeito.NavegarPara -> {
                    navController.navigate(efeito.tela.rota)
                }
            }
        }
    }
    
    Scaffold(
        topBar = {
            TopAppBar(
                title = { Text("Minhas Tarefas") }
            )
        },
        floatingActionButton = {
            FloatingActionButton(onClick = { /* mostrar dialog */ }) {
                Icon(Icons.Default.Add, contentDescription = "Adicionar")
            }
        },
        snackbarHost = { SnackbarHost(snackbarHostState) }
    ) { padding ->
        when {
            estado.isLoading -> {
                Box(
                    modifier = Modifier
                        .fillMaxSize()
                        .padding(padding),
                    contentAlignment = Alignment.Center
                ) {
                    CircularProgressIndicator()
                }
            }
            estado.erro != null -> {
                Column(
                    modifier = Modifier
                        .fillMaxSize()
                        .padding(padding)
                        .padding(16.dp),
                    horizontalAlignment = Alignment.CenterHorizontally,
                    verticalArrangement = Arrangement.Center
                ) {
                    Text(
                        text = estado.erro!!,
                        color = MaterialTheme.colorScheme.error
                    )
                    Spacer(modifier = Modifier.height(16.dp))
                    Button(onClick = { 
                        viewModel.processarIntencao(Intencao.CarregarTarefas)
                    }) {
                        Text("Tentar novamente")
                    }
                }
            }
            else -> {
                LazyColumn(
                    modifier = Modifier
                        .fillMaxSize()
                        .padding(padding)
                        .padding(horizontal = 16.dp),
                    verticalArrangement = Arrangement.spacedBy(8.dp)
                ) {
                    items(estado.listaTarefas) { tarefa ->
                        TarefaItem(
                            tarefa = tarefa,
                            onConcluir = { 
                                viewModel.processarIntencao(
                                    Intencao.ConcluirTarefa(tarefa.id)
                                )
                            },
                            onExcluir = {
                                viewModel.processarIntencao(
                                    Intencao.ExcluirTarefa(tarefa.id)
                                )
                            },
                            onClick = {
                                viewModel.processarIntencao(
                                    Intencao.NavegarParaDetalhe(tarefa)
                                )
                            }
                        )
                    }
                }
            }
        }
    }
}
```

### Fluxo de dados unidirecional (UDF)

[DIAGRAMA detalhado:

1. Usuário interage com UI
2. UI dispara Intent para ViewModel
3. ViewModel processa Intent
4. Reducer transforma Estado
5. Estado atualizado
6. UI observa Estado (via collectAsState)
7. UI recompõe com novos dados
8. Se necessário, Effects são emitidos para ações únicas (navegação, snackbar)
]

Princípios UDF:

- Estado é único e imutável
- Dados fluem em uma direção
- UI reage a mudanças de estado
- Ações disparam mudanças, não atualizam UI diretamente

## 3.3 Diferenças entre MVVM e MVI

| Aspecto | MVVM | MVI |
|--------|-----|-----|
| Estado | Múltiplos LiveData/StateFlow | Estado único e imutável |
| Ação do usuário | Direct binding | Intenções explícitas |
| Previsibilidade | Média | Alta |
| Complexidade | Menor | Maior |
| Testabilidade | Boa | Excelente |
| Bugs de race condition | Mais provável | Menos provável |

## 3.4 State Hoisting

Mover o estado para cima na árvore de composables:

```
@Composable
fun BotaoContador(
    initialCount: Int = 0,
    onCountChanged: (Int) -> Unit = {}
) {
    var count by remember { mutableStateOf(initialCount) }
    
    Button(onClick = { 
        count++
        onCountChanged(count)
    }) {
        Text("Clicks: $count")
    }
}
```

## 3.5 Single Source of Truth

Uma única fonte para o estado:

```
// ✅ Bom: ViewModel é a única fonte
class TelaViewModel : ViewModel() {
    val estado: StateFlow<Estado> = _estado.asStateFlow()
}

// ❌ Ruim: múltiplas fontes
@Composable
fun TelaRuim() {
    var estado by remember { mutableStateOf(Estado()) }
    var lista by remember { mutableStateOf<List>(emptyList()) }
    // Problema: difícil rastrear mudanças
}
```

## 3.6 Clean Architecture

Arquitetura em camadas separadas:

[DIAGRAMA: Presentation (UI + ViewModel) → Domain (Use Cases + Entities) → Data (Repositories + Data Sources)]

### Estrutura de pastas

```
app/
├── src/main/java/com/projeto/
│   ├── data/
│   │   ├── remote/
│   │   │   ├── api/
│   │   │   └── dto/
│   │   ├── repository/
│   │   └── local/
│   ├── domain/
│   │   ├── model/
│   │   ├── repository/
│   │   └── usecase/
│   ├── presentation/
│   │   ├── ui/
│   │   │   ├── components/
│   │   │   ├── screens/
│   │   │   └── theme/
│   │   ├── viewmodel/
│   │   └── navigation/
│   └── di/
└── build.gradle
```

### Camadas

#### Domain ( núcleo )

Contém regras de negócio:

```
// Entity
data class Usuario(
    val id: String,
    val nome: String,
    val email: String
)

// Repository interface
interface UsuarioRepository {
    suspend fun buscarUsuario(id: String): Usuario
    suspend fun salvarUsuario(usuario: Usuario)
    suspend fun excluirUsuario(id: String)
}

// Use Case
class BuscarUsuarioUseCase(
    private val repository: UsuarioRepository
) {
    suspend operator fun invoke(id: String): Usuario {
        return repository.buscarUsuario(id)
    }
}
```

#### Data (Implementação)

Implementa as interfaces do domínio:

```
class UsuarioRepositoryImpl(
    private val api: UsuarioApi,
    private val dao: UsuarioDao
) : UsuarioRepository {
    
    override suspend fun buscarUsuario(id: String): Usuario {
        return try {
            val dto = api.buscarUsuario(id)
            dto.toEntity()
        } catch (e: Exception) {
            dao.buscarUsuario(id)?.toEntity() ?: throw e
        }
    }
}
```

#### Presentation (UI)

ViewModels e Composables:

```
@HiltViewModel
class UsuarioViewModel @Inject constructor(
    private val buscarUsuario: BuscarUsuarioUseCase
) : ViewModel() {
    // implementação
}
```

### Dependencies Rule

[DIAGRAMA: Presentation → Domain ← Data]

Presentation conhece Domain
Domain NÃO conhece ninguém
Data implementa Domain

---

# MÓDULO 4 — INJEÇÃO DE DEPENDÊNCIA

Injeção de Dependência (DI) é uma técnica para fornecer dependências automatizadamente.

## 4.1 Hilt

Hilt é a solução oficial do Google baseada em Dagger.

### Configuração

```
// build.gradle (app)
plugins {
    id 'com.google.dagger.hilt.android'
}

dependencies {
    implementation 'com.google.dagger:hilt-android:2.48'
    kapt 'com.google.dagger:hilt-android-compiler:2.48'
}
```

```
// build.gradle (project)
buildscript {
    classpath 'com.google.dagger:hilt-android-gradle-plugin:2.48'
}
```

### @HiltAndroidApp

Aplicação Hilt:

```
@HiltAndroidApp
class Aplicacao : Application()
```

### @AndroidEntryPoint

Activity ou Fragment com injeção:

```
@AndroidEntryPoint
class MainActivity : ComponentActivity() {
    @Inject
    lateinit var repository: UsuarioRepository
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            // código Compose
        }
    }
}
```

### @Module e @Provides

Módulos de configuração:

```
@Module
@InstallIn(SingletonComponent::class)
object NetworkModule {
    
    @Provides
    @Singleton
    fun providesRetrofit(): Retrofit {
        return Retrofit.Builder()
            .baseUrl("https://api.example.com/")
            .addConverterFactory(GsonConverterFactory.create())
            .build()
    }
    
    @Provides
    @Singleton
    fun providesApi(retrofit: Retrofit): UsuarioApi {
        return retrofit.create(UsuarioApi::class.java)
    }
}
```

### @Inject direto no construtor

```
class UsuarioRepository @Inject constructor(
    private val api: UsuarioApi
) : IUsuarioRepository {
    override suspend fun buscarUsuario(id: String) = api.buscar(id)
}
```

### @ViewModel com Hilt

```
@HiltViewModel
class UsuarioViewModel @Inject constructor(
    private val repository: IUsuarioRepository
) : ViewModel() {
    // ViewModel com dependências injetadas
}
```

### @Module para dependências externas

```
@Module
@InstallIn(SingletonComponent::class)
object AppModule {
    
    @Provides
    @Singleton
    fun provideContext(@ApplicationContext context: Context): Context {
        return context
    }
}
```

## 4.2 Koin

Koin é uma solução mais simples e leve.

### Configuração

```
// build.gradle
dependencies {
    implementation 'io.insert-koin:koin-android:3.5.0'
    implementation 'io.insert-koin:koin-androidx-compose:3.5.0'
}
```

### Inicialização

```
class Aplicacao : Application() {
    override fun onCreate() {
        super.onCreate()
        
        koinApplication {
            modules(moduloApp, moduloNetwork, moduloRepo)
        }
    }
}
```

### DSL do Koin

```
val moduloApp = module {
    single { provideContext(androidContext()) }
}

val moduloNetwork = module {
    single {
        Retrofit.Builder()
            .baseUrl("https://api.com/")
            .addConverterFactory(GsonConverterFactory.create())
            .build()
    }
    
    single { get<Retrofit>().create(UsuarioApi::class.java) }
}

val moduloRepository = module {
    single<UsuarioRepository> { UsuarioRepositoryImpl(get()) }
}

val moduloViewModel = viewModel {
    UsuarioViewModel(get())
}

val KoinModules = listOf(
    moduloApp,
    moduloNetwork,
    moduloRepository,
    moduloViewModel
)
```

### single vs factory

```
// single — mesma instância em todo lugar
single<UsuarioRepository> { UsuarioRepositoryImpl(get()) }

// factory — nova instância a cada solicitação
factory<UsuarioUseCase> { UsuarioUseCase(get()) }
```

### viewModel no Koin

```
@Composable
fun TelaKoin() {
    val viewModel: TarefaViewModel = koinViewModel()
    // implementação
}
```

### Inicialização manual

```
// Sem startKoin
val app = koinApplication {
    modules(listaModulos)
}
```

## 4.3 Comparação Hilt vs Koin

| Aspecto | Hilt | Koin |
|---------|------|------|
| Complexidade | Alta | Baixa |
| Performance | Excelente | Boa |
| Geração de código | Sim (APT) | Não |
| Documentação | Boa | Boa |
| Curva de aprendizado | Alta | Baixa |
| Tamanho APK | +100KB | ~50KB |
| Retrofit/Dagger | Usa | API própria |

## 4.4 Quando usar cada um

Use Koin quando:

- Projeto pequeno/médio
- Necessidade de simplicidade
- Equipe pequena

Use Hilt quando:

- Projeto grande
- Necessidade de performance
- Equipe grande
- Código complexo de teste

---

# MÓDULO 5 — NETWORKING

Networking é a comunicação do app com servidores e APIs externas.

## 5.1 Retrofit

Retrofit é o cliente HTTP mais usado em Android.

### Configuração

```
// build.gradle
dependencies {
    implementation 'com.squareup.retrofit2:retrofit:2.9.0'
    implementation 'com.squareup.retrofit2:converter-gson:2.9.0'
    implementation 'com.squareup.okhttp3:logging-interceptor:4.12.0'
}
```

### Interface API

```
interface UsuarioApi {
    
    @GET("usuarios")
    suspend fun listarUsuarios(): List<UsuarioDto>
    
    @GET("usuarios/{id}")
    suspend fun buscarUsuario(@Path("id") id: String): UsuarioDto
    
    @POST("usuarios")
    suspend fun criarUsuario(@Body usuario: UsuarioDto): UsuarioDto
    
    @PUT("usuarios/{id}")
    suspend fun atualizarUsuario(
        @Path("id") id: String,
        @Body usuario: UsuarioDto
    ): UsuarioDto
    
    @DELETE("usuarios/{id}")
    suspend fun excluirUsuario(@Path("id") id: String)
}
```

### DTOs

```
data class UsuarioDto(
    val id: String?,
    val nome: String,
    val email: String,
    val telefone: String?
)
```

### Configuração do Retrofit

```
object RetrofitClient {
    
    private val logging = HttpLoggingInterceptor().apply {
        level = HttpLoggingInterceptor.Level.BODY
    }
    
    private val authInterceptor = Interceptor { chain ->
        val request = chain.request().newBuilder()
            .addHeader("Authorization", "Bearer $token")
            .addHeader("Content-Type", "application/json")
            .build()
        chain.proceed(request)
    }
    
    private val client = OkHttpClient.Builder()
        .addInterceptor(authInterceptor)
        .addInterceptor(logging)
        .connectTimeout(30, TimeUnit.SECONDS)
        .readTimeout(30, TimeUnit.SECONDS)
        .build()
    
    val retrofit = Retrofit.Builder()
        .baseUrl("https://api.projeto.com/")
        .client(client)
        .addConverterFactory(GsonConverterFactory.create())
        .build()
    
    val api: UsuarioApi = retrofit.create(UsuarioApi::class.java)
}
```

### Interceptors

#### Logging Interceptor

```
val logging = HttpLoggingInterceptor().apply {
    level = HttpLoggingInterceptor.Level.HEADERS
}
```

#### Auth Interceptor

```
val authInterceptor = Interceptor { chain ->
    val prefs = getSharedPreferences("auth", MODE_PRIVATE)
    val token = prefs.getString("token", null)
    
    val request = chain.request().newBuilder().apply {
        token?.let {
            addHeader("Authorization", "Bearer $it")
        }
    }.build()
    
    chain.proceed(request)
}
```

#### Error Handling Interceptor

```
val errorInterceptor = Interceptor { chain ->
    val response = chain.proceed(chain.request())
    
    if (!response.isSuccessful) {
        when (response.code) {
            401 -> throw UnauthorizedException()
            403 -> throw ForbiddenException()
            404 -> throw NotFoundException()
            500 -> throw ServerException()
        }
    }
    
    response
}
```

### Tratamento de Erros

```
sealed class Resultado<out T> {
    data class Sucesso<T>(val dados: T) : Resultado<T>()
    data class Erro(val mensagem: String, val codigo: Int? = null) : Resultado<Nothing>()
}

suspend fun <T> safeApiCall(apiCall: suspend () -> T): Resultado<T> {
    return try {
        Resultado.Sucesso(apiCall())
    } catch (e: HttpException) {
        Resultado.Erro(e.message(), e.code())
    } catch (e: Exception) {
        Resultado.Erro(e.message ?: "Erro desconhecido")
    }
}
```

## 5.2 Ktor Client

Ktor é o cliente HTTP da JetBrains.

### Configuração

```
// build.gradle
plugins {
    id 'org.jetbrains.kotlin.plugin.serialization'
}

dependencies {
    implementation 'io.ktor:ktor-client-core:2.3.7'
    implementation 'io.ktor:ktor-client-android:2.3.7'
    implementation 'io.ktor:ktor-client-content-negotiation:2.3.7'
    implementation 'io.ktor:ktor-serialization-gson:2.3.7'
    implementation 'io.ktor:ktor-client-logging:2.3.7'
}
```

### Configuração básica

```
val httpClient = HttpClient(Android) {
    install(ContentNegotiation) {
        gson()
    }
    
    install(Logging) {
        logger = Logger.DEFAULT
        level = LogLevel.ALL
    }
    
    install(HttpTimeout) {
        requestTimeoutMillis = 30000
        connectTimeoutMillis = 30000
        socketTimeoutMillis = 30000
    }
    
    defaultRequest {
        header("Content-Type", "application/json")
    }
}
```

### Requests com Ktor

```
// GET
suspend fun listarUsuarios(): List<UsuarioDto> {
    return httpClient.get("https://api.com/usuarios").body()
}

// GET com parâmetros
suspend fun buscarUsuario(id: String): UsuarioDto {
    return httpClient.get("https://api.com/usuarios/$id").body()
}

// POST
suspend fun criarUsuario(usuario: UsuarioDto): UsuarioDto {
    return httpClient.post("https://api.com/usuarios") {
        setBody(usuario)
    }.body()
}

// PUT
suspend fun atualizarUsuario(id: String, usuario: UsuarioDto): UsuarioDto {
    return httpClient.put("https://api.com/usuarios/$id") {
        setBody(usuario)
    }.body()
}

// DELETE
suspend fun excluirUsuario(id: String) {
    httpClient.delete("https://api.com/usuarios/$id")
}
```

### Interceptors no Ktor

```
val httpClient = HttpClient(Android) {
    engine {
        addInterceptor { chain ->
            val request = chain.request().newBuilder()
                .addHeader("Authorization", "Bearer $token")
                .build()
            chain.proceed(request)
        }
    }
}
```

## 5.3 Conceitos Importantes

### REST API

Princípios REST:

```
GET    — Ler      — /recurso     → Listar
GET    — Ler      → /recurso/id → Buscar um
POST   — Criar    → /recurso    → Criar
PUT    — Atualizar todo → /recurso/id → Atualizar
PATCH  — Atualizar parte → /recurso/id → Atualizar parcial
DELETE — Deletar → /recurso/id → Remover
```

### JSON Parsing

```
// Gson
val gson = Gson()
val json = gson.toJson(objeto)
val objeto = gson.fromJson(json, Tipo::class.java)

// Moshi
val moshi = Moshi.Builder().build()
val adapter = moshi.adapter(Tipo::class.java)
val json = adapter.toJson(objeto)
val objeto = adapter.fromJson(json)
```

### DTO vs Domain Model

DTO (Data Transfer Object) — formato da API:

```
data class UsuarioDto(
    val id: String?,
    val nome: String,
    val email: String,
    val endereco: EnderecoDto?
)
```

Domain Model — formato interno:

```
data class Usuario(
    val id: String,
    val nome: String,
    val email: Email,
    val endereco: Endereco?
)
```

### Mapper

Transformar entre modelos:

```
fun UsuarioDto.toEntity(): Usuario {
    return Usuario(
        id = id ?: "",
        nome = nome,
        email = Email(email),
        endereco = endereco?.toEntity()
    )
}

fun Usuario.toDto(): UsuarioDto {
    return UsuarioDto(
        id = id,
        nome = nome,
        email = email.valor,
        endereco = endereco?.toDto()
    )
}
```

---

# MÓDULO 6 — COROUTINES E FLOW

Coroutines e Flow são fundamentais para programação assíncrona moderna.

## 6.1 Coroutines

Coroutines permitem escrever código assíncrono de forma sequencial.

### launch — iniciar execução

```
viewModelScope.launch {
    // código executado em-background
    val dados = repository.buscarDados()
    
    // atualizações de UI
    _estado.value = dados
}
```

### async — retornar valor

```
viewModelScope.launch {
    val deferred = async { buscarUsuario(id) }
    val usuario = deferred.await()
}
```

### await com múltiplas requisições

```
viewModelScope.launch {
    val usuario = async { api.buscarUsuario() }
    val posts = async { api.buscarPosts() }
    
    val dados = Dados(
        usuario = usuario.await(),
        posts = posts.await()
    )
}
```

### Dispatchers

Controlam a thread de execução:

```
Dispatchers.Main       // UI thread (Android main thread)
Dispatchers.IO        // Operações de rede/disco
Dispatchers.Default  // CPU intensivo
Dispatchers.Unconfined
```

```
viewModelScope.launch(Dispatchers.IO) {
    // executa em IO
    val dados = api.buscarDados()
    
    withContext(Dispatchers.Main) {
        // volta para UI
        updateUI(dados)
    }
}
```

### suspend functions

Funções que podem ser pausadas:

```
suspend fun buscarDados(): Dados {
    return withContext(Dispatchers.IO) {
        api.buscarDados()
    }
}
```

## 6.2 Flow

Flow é uma sequência assíncrona de dados.

### Criar Flow

```
val fluxo = flow {
    emit(1)
    delay(100)
    emit(2)
    delay(100)
    emit(3)
}
```

### Flow de repository

```
class Repository {
    fun buscarUsuarios(): Flow<List<Usuario>> = flow {
        while (true) {
            emit(api.buscarUsuarios())
            delay(30_000)  // polling a cada 30s
        }
    }.flowOn(Dispatchers.IO)
}
```

### Operadores

#### map — transformar

```
flowOf(1, 2, 3)
    .map { it * 2 }
    .collect { println(it) }
// Output: 2, 4, 6
```

#### filter — filtrar

```
flowOf(1, 2, 3, 4, 5)
    .filter { it > 2 }
    .collect { println(it) }
// Output: 3, 4, 5
```

#### take — limitar

```
flowOf(1, 2, 3, 4, 5)
    .take(3)
    .collect { println(it) }
// Output: 1, 2, 3
```

#### distinctUntilChanged — evitar duplicados

```
flowOf(1, 1, 2, 2, 3)
    .distinctUntilChanged()
    .collect { println(it) }
// Output: 1, 2, 3
```

### collect — coletar valores

```
viewModelScope.launch {
   repository.buscarUsuarios()
        .catch { e -> erro.value = e.message }
        .collect { lista -> usuarios.value = lista }
}
```

## 6.3 StateFlow

StateFlow é um Flow que mantém estado:

```
val _estado = MutableStateFlow(Estado())
val estado: StateFlow<Estado> = _estado

// Atualizar
_estado.value = _estado.value.copy(isLoading = false)

// Collecting
val estado by viewModel.estado.collectAsState()
```

### Exemplo completo

```
@HiltViewModel
class TarefaViewModel @Inject constructor(
    private val repository: TarefaRepository
) : ViewModel() {
    
    private val _estado = MutableStateFlow(EstadoTela())
    val estado: StateFlow<EstadoTela> = _estado
    
    init {
        carregarTarefas()
    }
    
    fun carregarTarefas() {
        viewModelScope.launch {
            _estado.value = _estado.value.copy(isLoading = true)
            
            repository.buscarTarefas()
                .catch { e ->
                    _estado.value = _estado.value.copy(
                        isLoading = false,
                        erro = e.message
                    )
                }
                .collect { tarefas ->
                    _estado.value = _estado.value.copy(
                        isLoading = false,
                        tarefas = tarefas
                    )
                }
        }
    }
}
```

## 6.4 SharedFlow

SharedFlow emite eventos únicos:

```
val _efeitos = MutableSharedFlow<Efeito>()
val efeitos: SharedFlow<Efeito> = _efeitos

// Emitir efeito
viewModelScope.launch {
    _efeitos.emit(Efeito.NavegarPara(tela))
}
```

### collect no Compose

```
LaunchedEffect(Unit) {
    viewModel.efeitos.collect { efeito ->
        when (efeito) {
            is Efeito.Nav -> navController.navigate(efeito.rota)
            is Efeito.Snackbar -> snackbar.show(efeito.msg)
        }
    }
}
```

## 6.5 Channels

Channel é um管道 entre coroutines:

```
val canal = Channel<String>()

// Enviar
viewModelScope.launch {
    canal.send("mensagem")
}

// Receber
viewModelScope.launch {
    val msg = canal.receive()
}
```

## 6.6 Tratamento de Erros

```
viewModelScope.launch {
    flow {
        emit(api.buscar())
    }.catch { erro ->
        emit(EstadoDeErro(erro.message))
    }.collect { estado ->
        _estado.value = estado
    }
}
```

```
flowOf(1, 2, 3)
    .catch { e -> println("Erro: $e") }
    .onEach { println(it) }
    .launchIn(viewModelScope)
```

---

# MÓDULO 7 — PERSISTÊNCIA

Persistência salva dados localmente.

## 7.1 SharedPreferences

Para dados pequenos e simples:

```
val prefs = getSharedPreferences("app_prefs", Context.MODE_PRIVATE)

// Salvar
prefs.edit().apply {
    putString("nome", "João")
    putInt("idade", 25)
    putBoolean("ativo", true)
    apply()
}

// Ler
val nome = prefs.getString("nome", "Padrão")
val idade = prefs.getInt("idade", 0)
val ativo = prefs.getBoolean("ativo", false)

// Remover
prefs.edit().remove("nome").apply()
prefs.edit().clear().apply()
```

### Preferências com DataStore

```
val Context.dataStore: DataStore<Preferences> by preferencesDataStore(name = "settings")
```

## 7.2 DataStore

DataStore é a替代 moderna:

### Configuração

```
// build.gradle
dependencies {
    implementation 'androidx.datastore:datastore-preferences:1.0.0'
}
```

### Implementação

```
class SettingsManager(private val dataStore: DataStore<Preferences>) {
    
    private val CHAVE_NOME = stringPreferencesKey("nome")
    private val CHAVE_TEMA = stringPreferencesKey("tema")
    
    val nome: Flow<String> = dataStore.data.map { it[CHAVE_NOME] ?: "" }
    val tema: Flow<String> = dataStore.data.map { it[CHAVE_TEMA] ?: "claro" }
    
    suspend fun salvarNome(nome: String) {
        dataStore.edit { prefs ->
            prefs[CHAVE_NOME] = nome
        }
    }
    
    suspend fun salvarTema(tema: String) {
        dataStore.edit { prefs ->
            prefs[CHAVE_TEMA] = tema
        }
    }
}
```

### Proto DataStore (para objetos complexos)

Crie um schema .proto:

```
message UserPreferences {
  string username = 1;
  int32 age = 2;
  Theme theme = 3;
}
```

## 7.3 Quando usar cada um

| Recurso | SharedPreferences | DataStore |
|--------|-------------------|----------|
| Dados simples | ✅ Sim | ✅ Sim |
| Dados complexos | ❌ Não | ✅ Sim |
| Async | ❌ Não | ✅ Sim |
| Performance | Boa | Melhor |
| Type safety | ❌ Não | ✅ Sim (Proto) |

---

# MÓDULO 8 — NAVEGAÇÃO

Navegação controla as telas do app.

## 8.1 Navigation Compose

### Configuração

```
// build.gradle
dependencies {
    implementation 'androidx.navigation:navigation-compose:2.7.6'
}
```

### Definição de rotas

```
sealed class Rota(val path: String) {
    object Inicio : Rota("inicio")
    object Detalhes : Rota("detalhes/{id}") {
        fun criar(id: String) = "detalhes/$id"
    }
    object Login : Rota("login")
    object Cadastro : Rota("cadastro")
}
```

### NavHost

```
@Composable
fun AppNavigation() {
    val navController = rememberNavController()
    
    NavHost(
        navController = navController,
        startDestination = Rota.Inicio.path
    ) {
        composable(Rota.Inicio.path) {
            TelaInicio(
                onNavigateToDetalhes = { id ->
                    navController.navigate(Rota.Detalhes.criar(id))
                }
            )
        }
        
        composable(
            route = Rota.Detalhes.path,
            arguments = listOf(
                navArgument("id") { type = NavType.StringType }
            )
        ) { backStackEntry ->
            val id = backStackEntry.arguments?.getString("id") ?: ""
            TelaDetalhes(id = id)
        }
        
        composable(Rota.Login.path) {
            TelaLogin(
                onNavigateToCadastro = {
                    navController.navigate(Rota.Cadastro.path)
                },
                onLoginSuccess = {
                    navController.navigate(Rota.Inicio.path) {
                        popUpTo(Rota.Login.path) { inclusive = true }
                    }
                }
            )
        }
    }
}
```

### Passagem de argumentos

```
composable("detalhes/{id}") { backStackEntry ->
    val id = backStackEntry.arguments?.getString("id") ?: return@composable
    
    TelaDetalhes(id = id)
}

// Passando argumento
navController.navigate("detalhes/123")
```

## 8.2 Deep Links

```
composable(
    "tela/{id}",
    deepLinks = listOf(
        navDeepLink { uriPattern = "meuapp://tela/$id" },
        navDeepLink { uriPattern = "https://meusite.com/tela/$id" }
    )
)
```

---

# MÓDULO 9 — TESTES

Testes garantem qualidade do código.

## 9.1 JUnit

Testes unitários básicos:

```
class Calculadora {
    fun soma(a: Int, b: Int) = a + b
    fun divide(a: Int, b: Int) = a / b
}
```

### Teste básico

```
@Test
fun teste_soma() {
    val calc = Calculadora()
    assertEquals(5, calc.soma(2, 3))
}
```

### Teste com exceções

```
@Test(expected = ArithmeticException::class)
fun teste_divisao_por_zero() {
    val calc = Calculadora()
    calc.divide(10, 0)
}
```

## 9.2 MockK

MockK é framework de mocking para Kotlin:

### Configuração

```
// build.gradle
testImplementation 'io.mockk:mockk:1.13.8'
testImplementation 'io.mockk:mockk-android:1.13.8'
```

### Mock básico

```
val repository = mockk<UsuarioRepository>()

every { repository.buscarUsuario("1") } returns Usuario("1", "João", "joao@email.com")

val usuario = repository.buscarUsuario("1")
verify { repository.buscarUsuario("1") }
```

### Mock com erro

```
every { repository.buscarUsuario(throw IOException()) }
```

### Verify

```
verify { repository.buscarUsuario("1") }
verify(exactly = 1) { repository.buscarUsuario("1") }
verify(none = 1) { repository.excluirUsuario(any()) }
```

### Spy

```
val repository = spyk(UsuarioRepositoryImpl())

every { repository.buscarUsuario("1") } returns Usuario("1", "Teste", "teste@email.com")
```

## 9.3 Mockito

Mockito funciona similar ao MockK:

```
val repository = mock(UsuarioRepository::class.java)

`when`(repository.buscarUsuario("1")).thenReturn(Usuario("1", "João", "joao@email.com"))

verify(repository).buscarUsuario("1")
```

### Diferenças

| Aspecto | MockK | Mockito |
|--------|------|---------|
| Sintaxe Kotlin | native | adaptada |
| Suporte lambdas | ✅ | ❌ |
| Coroutines | ✅ | ❌ |
| Extension functions | ✅ | ❌ |

## 9.4 Turbine

Testes de Flow com Turbine:

### Configuração

```
testImplementation 'app.cash.turbine:turbine:1.0.0'
```

### Teste de Flow

```
@Test
fun teste_flow() = runTest {
    val viewModel = TarefaViewModel(repository)
    
    viewModel.tarefas.test {
        val primeira = awaitItem()
        assertTrue(primeira.isEmpty)
        
        // adicionar tarefa
        viewModel.adicionarTarefa("Tarefa 1")
        
        val segunda = awaitItem()
        assertEquals(1, segunda.size)
        
        awaitComplete()
    }
}
```

## 9.5 Compose UI Test

Testes de UI com Compose:

### Configuração

```
androidTestImplementation 'androidx.compose.ui:ui-test-junit4'
```

### Teste básico

```
@get:Rule
val rule = createComposeRule()

@Test
fun teste_botao() {
    setContent {
        Button(onClick = { }) {
            Text("Clique")
        }
    }
    
    onNodeWithText("Clique").performClick()
}
```

### Verificar estados

```
@Test
fun teste_estado() {
    varclicado = false
    setContent {
        Button(
            onClick = { clicado = true },
            enabled = true
        ) {
            Text("Ativar")
        }
    }
    
    onNodeWithText("Ativar").assertEnabled()
    onNodeWithText("Ativar").performClick()
    assertTrue(clicado)
}
```

### Navegação

```
@Test
fun teste_navegacao() {
    val navController = rememberNavController()
    
    setContent {
        AppNavigation(navController)
    }
    
    onNodeWithText("Tela Inicial").assertExists()
    
    onNodeWithText("Ir para Detalhes").performClick()
    
    onNodeWithText("Detalhes").assertExists()
}
```

---

# MÓDULO 10 — PRINCÍPIOS DE SOFTWARE

Boas práticas de desenvolvimento.

## 10.1 SOLID

Princípios de orientação a objetos:

### S — Single Responsibility

Cada classe faz uma coisa:

```
// ❌ Ruim
class Usuario(
    val nome: String,
    val email: String
) {
    fun salvar() { ... }
    fun enviarEmail() { }
    fun gerarRelatorio() { }
}

// ✅ Bom
class Usuario { ... }
class UsuarioRepository { fun salvar() { } }
class EmailService { fun enviar() { } }
```

### O — Open/Closed

Aberto para extensão, fechado para modificação:

```
// ✅ Bom
abstract class Pagamento {
    abstract fun pagar()
}

class PagamentoCartao : Pagamento() { ... }
class PagamentoBoleto : Pagamento() { ... }
```

### L — Liskov Substitution

Subclasses podem substituir a classe pai:

```
open class Animal { open fun som() }
class Cao : Animal() { override fun som() = "Au" }
class Gato : Animal() { override fun som() = "Miau" }

fun fazerSom(animal: Animal) = animal.som()
```

### I — Interface Segregation

Interfaces menores:

```
// ❌ Ruim
interface Repositorio {
    fun buscar()
    fun salvar()
    fun excluir()
    fun listar()
}

// ✅ Bom
interface Leitura { fun buscar() }
interface Escrita { fun salvar() }
interface Delecao { fun excluir() }
```

### D — Dependency Inversion

Depender de abstrações:

```
// ❌ Ruim
class Servico(val api: ApiRetrofit)

// ✅ Bom
class Servico(val api: IApi)
```

## 10.2 Clean Code

Código limpo e legível:

### Nomes significativos

```
// ❌ Ruim
val d = Date()
val l = mutableListOf()
fun p() { }

// ✅ Bom
val dataAtual = Date()
val itensPendentes = mutableListOf()
fun processar()
```

### Funções pequenas

```
// ❌ Ruim
fun processarTudo() {
    // 100 linhas
}

// ✅ Bom
fun processar() { }
fun validar() { }
fun salvar() { }
```

### Comentários quando necessário

```
// ✅ Bom: explica o "porquê"
async fun buscarDados(): Flow<List<Dado>> {
    // Faz polling a cada 30s para dados em tempo real
    while (true) { ... }
}

// ❌ Ruim: o código já diz o que faz
// Incrementa contador
contador++
```

### DRY (Don't Repeat Yourself)

```
// ❌ Ruim
fun validarEmail(email: String) = email.contains("@")
fun validarEmailAdmin(email: String) = email.contains("@")

// ✅ Bom
fun validarEmail(email: String) = email.contains("@")
```

## 10.3 Organização de Projeto

Estrutura profissional:

```
com/
├── projeto/
│   ├── data/
│   │   ├── remote/
│   │   │   ├── api/
│   │   │   └── dto/
│   │   ├── local/
│   │   └── repository/
│   ├── domain/
│   │   ├── model/
│   │   ├── repository/
│   │   └── usecase/
│   ├── presentation/
│   │   ├── components/
│   │   ├── screens/
│   │   ├── navigation/
│   │   └── theme/
│   └── di/
└── App.kt
```

---

# MÓDULO 11 — PROJETO COMPLETO

Vamos construir um app de tarefas completo com todos os conceitos aprendidos.

## 11.1 Especificações

- **App**: Lista de tarefas (TODO)
- **Arquitetura**: MVI completo
- **DI**: Hilt
- **UI**: Jetpack Compose + Material 3
- **Networking**: Retrofit + Coroutines Flow
- **Persistência**: DataStore
- **Navegação**: Navigation Compose
- **Testes**: Unitários + MockK

## 11.2 Estrutura de Pastas

```
app/src/main/java/com/tarefas/
├── data/
│   ├── local/
│   │   └── TarefaDataStore.kt
│   ├── remote/
│   │   ├── TarefaApi.kt
│   │   └── TarefaDto.kt
│   └── repository/
│       └── TarefaRepositoryImpl.kt
├── domain/
│   ├── model/
│   │   └── Tarefa.kt
│   ├── repository/
│   │   └── TarefaRepository.kt
│   └── usecase/
│       ├── BuscarTarefasUseCase.kt
│       ├── AdicionarTarefaUseCase.kt
│       └── ExcluirTarefaUseCase.kt
├── presentation/
│   ├── navigation/
│   │   └── Navegacao.kt
│   ├── screens/
│   │   ├── home/
│   │   │   ├── HomeContract.kt
│   │   │   ├── HomeScreen.kt
│   │   │   └── HomeViewModel.kt
│   │   └── adicionar/
│   │       ├── AdicionarContract.kt
│   │       ├── AdicionarScreen.kt
│   │       └── AdicionarViewModel.kt
│   ├── components/
│   │   └── TarefaItem.kt
│   └── theme/
│       └── Tema.kt
├── di/
│   └── AppModule.kt
└── App.kt
```

## 11.3 Implementação

### Domain — Model

```
package com.tarefas.domain.model

data class Tarefa(
    val id: String,
    val titulo: String,
    val descricao: String,
    val concluida: Boolean,
    val dataCriacao: Long
)
```

### Domain — Repository Interface

```
package com.tarefas.domain.repository

import com.tarefas.domain.model.Tarefa
import kotlinx.coroutines.flow.Flow

interface TarefaRepository {
    fun buscarTarefas(): Flow<List<Tarefa>>
    suspend fun adicionarTarefa(tarefa: Tarefa)
    suspend fun concluirTarefa(id: String)
    suspend fun excluirTarefa(id: String)
}
```

### Domain — Use Cases

```
package com.tarefas.domain.usecase

import com.tarefas.domain.model.Tarefa
import com.tarefas.domain.repository.TarefaRepository
import kotlinx.coroutines.flow.Flow
import javax.inject.Inject

class BuscarTarefasUseCase @Inject constructor(
    private val repository: TarefaRepository
) {
    operator fun invoke(): Flow<List<Tarefa>> {
        return repository.buscarTarefas()
    }
}

class AdicionarTarefaUseCase @Inject constructor(
    private val repository: TarefaRepository
) {
    suspend operator fun invoke(tarefa: Tarefa) {
        repository.adicionarTarefa(tarefa)
    }
}

class ExcluirTarefaUseCase @Inject constructor(
    private val repository: TarefaRepository
) {
    suspend operator fun invoke(id: String) {
        repository.excluirTarefa(id)
    }
}

class ConcluirTarefaUseCase @Inject constructor(
    private val repository: TarefaRepository
) {
    suspend operator fun invoke(id: String) {
        repository.concluirTarefa(id)
    }
}
```

### Data — DTO

```
package com.tarefas.data.remote.dto

import com.google.gson.annotations.SerializedName

data class TarefaDto(
    @SerializedName("id")
    val id: String?,
    @SerializedName("title")
    val titulo: String,
    @SerializedName("description")
    val descricao: String,
    @SerializedName("completed")
    val concluida: Boolean,
    @SerializedName("created_at")
    val dataCriacao: Long
)
```

### Data — API

```
package com.tarefas.data.remote

import com.tarefas.data.remote.dto.TarefaDto
import retrofit2.http.*

interface TarefaApi {
    @GET("tarefas")
    suspend fun listarTarefas(): List<TarefaDto>
    
    @POST("tarefas")
    suspend fun criarTarefa(@Body tarefa: TarefaDto): TarefaDto
    
    @PUT("tarefas/{id}")
    suspend fun atualizarTarefa(
        @Path("id") id: String,
        @Body tarefa: TarefaDto
    ): TarefaDto
    
    @DELETE("tarefas/{id}")
    suspend fun excluirTarefa(@Path("id") id: String)
}
```

### Data — Repository Implementation

```
package com.tarefas.data.repository

import com.tarefas.data.local.TarefaDataStore
import com.tarefas.data.remote.TarefaApi
import com.tarefas.domain.model.Tarefa
import com.tarefas.domain.repository.TarefaRepository
import kotlinx.coroutines.flow.Flow
import kotlinx.coroutines.flow.map
import javax.inject.Inject

class TarefaRepositoryImpl @Inject constructor(
    private val api: TarefaApi,
    private val dataStore: TarefaDataStore
) : TarefaRepository {
    
    override fun buscarTarefas(): Flow<List<Tarefa>> {
        return dataStore.tarefas
    }
    
    override suspend fun adicionarTarefa(tarefa: Tarefa) {
        dataStore.adicionarTarefa(tarefa)
    }
    
    override suspend fun concluirTarefa(id: String) {
        dataStore.concluirTarefa(id)
    }
    
    override suspend fun excluirTarefa(id: String) {
        dataStore.excluirTarefa(id)
    }
}
```

### Data — DataStore Local

```
package com.tarefas.data.local

import androidx.datastore.core.DataStore
import androidx.datastore.preferences.core.Preferences
import androidx.datastore.preferences.core.edit
import androidx.datastore.preferences.core.stringPreferencesKey
import com.tarefas.domain.model.Tarefa
import kotlinx.coroutines.flow.Flow
import kotlinx.coroutines.flow.map
import javax.inject.Inject
import javax.inject.Singleton

@Singleton
class TarefaDataStore @Inject constructor(
    private val dataStore: DataStore<Preferences>
) {
    companion object {
        val TAREFAS_KEY = stringPreferencesKey("tarefas")
    }
    
    val tarefas: Flow<List<Tarefa>> = dataStore.data.map { prefs ->
        val json = prefs[TAREFAS_KEY] ?: "[]"
        // Parsear JSON para lista de Tarefa
        parseTarefas(json)
    }
    
    suspend fun adicionarTarefa(tarefa: Tarefa) {
        dataStore.edit { prefs ->
            val atual = parseTarefas(prefs[TAREFAS_KEY] ?: "[]")
            val novaLista = atual + tarefa
            prefs[TAREFAS_KEY] = toJson(novaLista)
        }
    }
    
    suspend fun concluirTarefa(id: String) {
        dataStore.edit { prefs ->
            val atual = parseTarefas(prefs[TAREFAS_KEY] ?: "[]")
            val novaLista = atual.map {
                if (it.id == id) it.copy(concluida = true) else it
            }
            prefs[TAREFAS_KEY] = toJson(novaLista)
        }
    }
    
    suspend fun excluirTarefa(id: String) {
        dataStore.edit { prefs ->
            val atual = parseTarefas(prefs[TAREFAS_KEY] ?: "[]")
            val novaLista = atual.filter { it.id != id }
            prefs[TAREFAS_KEY] = toJson(novaLista)
        }
    }
}
```

### DI — Módulos

```
package com.tarefas.di

import android.content.Context
import androidx.datastore.core.DataStore
import androidx.datastore.preferences.core.Preferences
import androidx.datastore.preferences.preferencesDataStore
import com.tarefas.data.local.TarefaDataStore
import com.tarefas.data.remote.TarefaApi
import com.tarefas.data.repository.TarefaRepositoryImpl
import com.tarefas.domain.repository.TarefaRepository
import dagger.Module
import dagger.Provides
import dagger.hilt.InstallIn
import dagger.hilt.android.qualifiers.ApplicationContext
import dagger.hilt.components.SingletonComponent
import okhttp3.OkHttpClient
import okhttp3.logging.HttpLoggingInterceptor
import retrofit2.Retrofit
import retrofit2.converter.gson.GsonConverterFactory
import java.util.concurrent.TimeUnit
import javax.inject.Singleton

private val Context.dataStore: DataStore<Preferences> by preferencesDataStore(name = "tarefas")

@Module
@InstallIn(SingletonComponent::class)
object AppModule {
    
    @Provides
    @Singleton
    fun provideOkHttpClient(): OkHttpClient {
        return OkHttpClient.Builder()
            .addInterceptor(HttpLoggingInterceptor().apply {
                level = HttpLoggingInterceptor.Level.BODY
            })
            .build()
    }
    
    @Provides
    @Singleton
    fun provideTarefaApi(okHttpClient: OkHttpClient): TarefaApi {
        return Retrofit.Builder()
            .baseUrl("https://api.tarefas.com/")
            .client(okHttpClient)
            .addConverterFactory(GsonConverterFactory.create())
            .build()
            .create(TarefaApi::class.java)
    }
    
    @Provides
    @Singleton
    fun provideDataStore(@ApplicationContext context: Context): DataStore<Preferences> {
        return context.dataStore
    }
    
    @Provides
    @Singleton
    fun provideTarefaDataStore(dataStore: DataStore<Preferences>): TarefaDataStore {
        return TarefaDataStore(dataStore)
    }
    
    @Provides
    @Singleton
    fun provideTarefaRepository(
        api: TarefaApi,
        dataStore: TarefaDataStore
    ): TarefaRepository {
        return TarefaRepositoryImpl(api, dataStore)
    }
}
```

### Presentation — MVI Contract (Home)

```
package com.tarefas.presentation.screens.home

import com.tarefas.domain.model.Tarefa

// Estado
data class HomeEstado(
    val isLoading: Boolean = false,
    val tarefas: List<Tarefa> = emptyList(),
    val erro: String? = null
)

// Ações do usuário
sealed class HomeAcao {
    object CarregarTarefas : HomeAcao()
    data class ConcluirTarefa(val id: String) : HomeAcao()
    data class ExcluirTarefa(val id: String) : HomeAcao()
    object LimparErro : HomeAcao()
}

// Efeitos (eventos únicos)
sealed class HomeEfeito {
    data class MostrarSnackbar(val mensagem: String) : HomeEfeito()
    data class NavegarPara(val id: String) : HomeEfeito()
}
```

### Presentation — ViewModel (Home)

```
package com.tarefas.presentation.screens.home

import androidx.lifecycle.ViewModel
import androidx.lifecycle.viewModelScope
import com.tarefas.domain.usecase.BuscarTarefasUseCase
import com.tarefas.domain.usecase.ConcluirTarefaUseCase
import com.tarefas.domain.usecase.ExcluirTarefaUseCase
import dagger.hilt.android.lifecycle.HiltViewModel
import kotlinx.coroutines.flow.MutableSharedFlow
import kotlinx.coroutines.flow.MutableStateFlow
import kotlinx.coroutines.flow.SharedFlow
import kotlinx.coroutines.flow.StateFlow
import kotlinx.coroutines.flow.asSharedFlow
import kotlinx.coroutines.flow.asStateFlow
import kotlinx.coroutines.flow.catch
import kotlinx.coroutines.launch
import javax.inject.Inject

@HiltViewModel
class HomeViewModel @Inject constructor(
    private val buscarTarefasUseCase: BuscarTarefasUseCase,
    private val concluirTarefaUseCase: ConcluirTarefaUseCase,
    private val excluirTarefaUseCase: ExcluirTarefaUseCase
) : ViewModel() {
    
    private val _estado = MutableStateFlow(HomeEstado())
    val estado: StateFlow<HomeEstado> = _estado.asStateFlow()
    
    private val _efeitos = MutableSharedFlow<HomeEfeito>()
    val efeitos: SharedFlow<HomeEfeito> = _efeitos.asSharedFlow()
    
    init {
        processarAcao(HomeAcao.CarregarTarefas)
    }
    
    fun processarAcao(acao: HomeAcao) {
        when (acao) {
            is HomeAcao.CarregarTarefas -> carregarTarefas()
            is HomeAcao.ConcluirTarefa -> concluirTarefa(acao.id)
            is HomeAcao.ExcluirTarefa -> excluirTarefa(acao.id)
            is HomeAcao.LimparErro -> reduzir { it.copy(erro = null) }
        }
    }
    
    private fun carregarTarefas() {
        viewModelScope.launch {
            _estado.value = _estado.value.copy(isLoading = true)
            
            buscarTarefasUseCase()
                .catch { e ->
                    _estado.value = _estado.value.copy(
                        isLoading = false,
                        erro = e.message
                    )
                }
                .collect { tarefas ->
                    _estado.value = _estado.value.copy(
                        isLoading = false,
                        tarefas = tarefas
                    )
                }
        }
    }
    
    private fun concluirTarefa(id: String) {
        viewModelScope.launch {
            try {
                concluirTarefaUseCase(id)
                _efeitos.emit(HomeEfeito.MostrarSnackbar("Tarefa concluída!"))
            } catch (e: Exception) {
                _estado.value = _estado.value.copy(erro = e.message)
            }
        }
    }
    
    private fun excluirTarefa(id: String) {
        viewModelScope.launch {
            try {
                excluirTarefaUseCase(id)
                _efeitos.emit(HomeEfeito.MostrarSnackbar("Tarefa excluída"))
            } catch (e: Exception) {
                _estado.value = _estado.value.copy(erro = e.message)
            }
        }
    }
    
    private fun reduzir(reducer: (HomeEstado) -> HomeEstado) {
        _estado.value = reducer(_estado.value)
    }
}
```

### Presentation — Screen (Home)

```
package com.tarefas.presentation.screens.home

import androidx.compose.foundation.layout.*
import androidx.compose.foundation.lazy.LazyColumn
import androidx.compose.foundation.lazy.items
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.f.Add
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp
import androidx.hilt.navigation.compose.hiltViewModel
import com.tarefas.presentation.components.TarefaItem

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun HomeScreen(
    onNavigateToAdicionar: () -> Unit,
    viewModel: HomeViewModel = hiltViewModel()
) {
    val estado by viewModel.estado.collectAsState()
    var snackbarHostState by remember { SnackbarHostState() }
    
    // Coletar efeitos
    LaunchedEffect(Unit) {
        viewModel.efeitos.collect { efeito ->
            when (efeito) {
                is HomeEfeito.MostrarSnackbar -> {
                    snackbarHostState.showSnackbar(efeito.mensagem)
                }
                is HomeEfeito.NavegarPara -> {
                    // navegar
                }
            }
        }
    }
    
    Scaffold(
        topBar = {
            TopAppBar(
                title = { Text("Minhas Tarefas") },
                colors = TopAppBarDefaults.topAppBarColors(
                    containerColor = MaterialTheme.colorScheme.primaryContainer
                )
            )
        },
        floatingActionButton = {
            FloatingActionButton(
                onClick = onNavigateToAdicionar,
                containerColor = MaterialTheme.colorScheme.primary
            ) {
                Icon(Icons.Default.Add, contentDescription = "Adicionar")
            }
        },
        snackbarHost = { SnackbarHost(snackbarHostState) }
    ) { padding ->
        when {
            estado.isLoading -> {
                Box(
                    modifier = Modifier
                        .fillMaxSize()
                        .padding(padding),
                    contentAlignment = Alignment.Center
                ) {
                    CircularProgressIndicator()
                }
            }
            estado.erro != null -> {
                Column(
                    modifier = Modifier
                        .fillMaxSize()
                        .padding(padding)
                        .padding(16.dp),
                    horizontalAlignment = Alignment.CenterHorizontally,
                    verticalArrangement = Arrangement.Center
                ) {
                    Text(
                        text = estado.erro!!,
                        color = MaterialTheme.colorScheme.error
                    )
                    Spacer(modifier = Modifier.height(16.dp))
                    Button(onClick = { 
                        viewModel.processarAcao(HomeAcao.CarregarTarefas)
                    }) {
                        Text("Tentar novamente")
                    }
                }
            }
            else -> {
                if (estado.tarefas.isEmpty()) {
                    Column(
                        modifier = Modifier
                            .fillMaxSize()
                            .padding(padding),
                        horizontalAlignment = Alignment.CenterHorizontally,
                        verticalArrangement = Arrangement.Center
                    ) {
                        Text(
                            text = "Nenhuma tarefa ainda",
                            style = MaterialTheme.typography.titleMedium
                        )
                        Spacer(modifier = Modifier.height(8.dp))
                        Text(
                            text = "Clique + para adicionar",
                            style = MaterialTheme.typography.bodyMedium,
                            color = MaterialTheme.colorScheme.onSurfaceVariant
                        )
                    }
                } else {
                    LazyColumn(
                        modifier = Modifier
                            .fillMaxSize()
                            .padding(padding),
                        contentPadding = PaddingValues(16.dp),
                        verticalArrangement = Arrangement.spacedBy(12.dp)
                    ) {
                        items(
                            items = estado.tarefas,
                            key = { it.id }
                        ) { tarefa ->
                            TarefaItem(
                                tarefa = tarefa,
                                onConcluir = {
                                    viewModel.processarAcao(HomeAcao.ConcluirTarefa(tarefa.id))
                                },
                                onExcluir = {
                                    viewModel.processarAcao(HomeAcao.ExcluirTarefa(tarefa.id))
                                }
                            )
                        }
                    }
                }
            }
        }
    }
}
```

### Presentation — Componente TarefaItem

```
package com.tarefas.presentation.components

import androidx.compose.animation.animateColorAsState
import androidx.compose.foundation.background
import androidx.compose.foundation.layout.*
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.Check
import androidx.compose.material.icons.filled.Delete
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.text.style.TextDecoration
import androidx.compose.ui.unit.dp
import com.tarefas.domain.model.Tarefa

@Composable
fun TarefaItem(
    tarefa: Tarefa,
    onConcluir: () -> Unit,
    onExcluir: () -> Unit
) {
    val containerColor by animateColorAsState(
        targetValue = if (tarefa.concluida) {
            MaterialTheme.colorScheme.surfaceVariant
        } else {
            MaterialTheme.colorScheme.surface
        },
        label = "card_bg"
    )
    
    Card(
        modifier = Modifier.fillMaxWidth(),
        colors = CardDefaults.cardColors(containerColor = containerColor)
    ) {
        Row(
            modifier = Modifier
                .fillMaxWidth()
                .padding(16.dp),
            horizontalArrangement = Arrangement.SpaceBetween,
            verticalAlignment = Alignment.CenterVertically
        ) {
            Column(modifier = Modifier.weight(1f)) {
                Text(
                    text = tarefa.titulo,
                    style = MaterialTheme.typography.titleMedium,
                    textDecoration = if (tarefa.concluida) 
                        TextDecoration.LineThrough else TextDecoration.None,
                    color = if (tarefa.concluida) 
                        MaterialTheme.colorScheme.onSurfaceVariant 
                    else MaterialTheme.colorScheme.onSurface
                )
                if (tarefa.descricao.isNotEmpty()) {
                    Spacer(modifier = Modifier.height(4.dp))
                    Text(
                        text = tarefa.descricao,
                        style = MaterialTheme.typography.bodyMedium,
                        color = MaterialTheme.colorScheme.onSurfaceVariant
                    )
                }
            }
            
            Row {
                IconButton(
                    onClick = onConcluir,
                    enabled = !tarefa.concluida
                ) {
                    Icon(
                        Icons.Default.Check,
                        contentDescription = "Concluir",
                        tint = if (tarefa.concluida) 
                            MaterialTheme.colorScheme.primary 
                        else MaterialTheme.colorScheme.onSurfaceVariant
                    )
                }
                IconButton(onClick = onExcluir) {
                    Icon(
                        Icons.Default.Delete,
                        contentDescription = "Excluir",
                        tint = MaterialTheme.colorScheme.error
                    )
                }
            }
        }
    }
}
```

### Presentation — Contract (Adicionar)

```
package com.tarefas.presentation.screens.adicionar

data class AdicionarEstado(
    val titulo: String = "",
    val descricao: String = "",
    val isLoading: Boolean = false,
    val erro: String? = null
)

sealed class AdicionarAcao {
    data class AtualizarTitulo(val titulo: String) : AdicionarAcao()
    data class AtualizarDescricao(val descricao: String) : AdicionarAcao()
    object SalvarTarefa : AdicionarAcao()
}

sealed class AdicionarEfeito {
    object TarefaSalva : AdicionarEfeito()
    data class Erro(val mensagem: String) : AdicionarEfeito()
}
```

### Presentation — ViewModel (Adicionar)

```
package com.tarefas.presentation.screens.adicionar

import androidx.lifecycle.ViewModel
import androidx.lifecycle.viewModelScope
import com.tarefas.domain.model.Tarefa
import com.tarefas.domain.usecase.AdicionarTarefaUseCase
import dagger.hilt.android.lifecycle.HiltViewModel
import kotlinx.coroutines.flow.MutableSharedFlow
import kotlinx.coroutines.flow.MutableStateFlow
import kotlinx.coroutines.flow.SharedFlow
import kotlinx.coroutines.flow.StateFlow
import kotlinx.coroutines.flow.asSharedFlow
import kotlinx.coroutines.flow.asStateFlow
import kotlinx.coroutines.launch
import java.util.UUID
import javax.inject.Inject

@HiltViewModel
class AdicionarViewModel @Inject constructor(
    private val adicionarTarefaUseCase: AdicionarTarefaUseCase
) : ViewModel() {
    
    private val _estado = MutableStateFlow(AdicionarEstado())
    val estado: StateFlow<AdicionarEstado> = _estado.asStateFlow()
    
    private val _efeitos = MutableSharedFlow<AdicionarEfeito>()
    val efeitos: SharedFlow<AdicionarEfeito> = _efeitos.asSharedFlow()
    
    fun processarAcao(acao: AdicionarAcao) {
        when (acao) {
            is AdicionarAcao.AtualizarTitulo -> {
                _estado.value = _estado.value.copy(titulo = acao.titulo)
            }
            is AdicionarAcao.AtualizarDescricao -> {
                _estado.value = _estado.value.copy(descricao = acao.descricao)
            }
            is AdicionarAcao.SalvarTarefa -> salvarTarefa()
        }
    }
    
    private fun salvarTarefa() {
        val estado = _estado.value
        
        if (estado.titulo.isBlank()) {
            viewModelScope.launch {
                _efeitos.emit(AdicionarEfeito.Erro("Título é obrigatório"))
            }
            return
        }
        
        viewModelScope.launch {
            _estado.value = _estado.value.copy(isLoading = true)
            
            try {
                val tarefa = Tarefa(
                    id = UUID.randomUUID().toString(),
                    titulo = estado.titulo,
                    descricao = estado.descricao,
                    concluida = false,
                    dataCriacao = System.currentTimeMillis()
                )
                
                adicionarTarefaUseCase(tarefa)
                _estado.value = _estado.value.copy(isLoading = false)
                _efeitos.emit(AdicionarEfeito.TarefaSalva)
                
            } catch (e: Exception) {
                _estado.value = _estado.value.copy(
                    isLoading = false,
                    erro = e.message
                )
                _efeitos.emit(AdicionarEfeito.Erro(e.message ?: "Erro ao salvar"))
            }
        }
    }
}
```

### Presentation — Screen (Adicionar)

```
package com.tarefas.presentation.screens.adicionar

import androidx.compose.foundation.layout.*
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.f.ArrowBack
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp
import androidx.hilt.navigation.compose.hiltViewModel

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun AdicionarScreen(
    onNavigateBack: () -> Unit,
    viewModel: AdicionarViewModel = hiltViewModel()
) {
    val estado by viewModel.estado.collectAsState()
    val snackbarHostState = remember { SnackbarHostState() }
    
    LaunchedEffect(Unit) {
        viewModel.efeitos.collect { efeito ->
            when (efeito) {
                is AdicionarEfeito.TarefaSalva -> onNavigateBack()
                is AdicionarEfeito.Erro -> snackbarHostState.showSnackbar(efeito.mensagem)
            }
        }
    }
    
    Scaffold(
        topBar = {
            TopAppBar(
                title = { Text("Nova Tarefa") },
                navigationIcon = {
                    IconButton(onClick = onNavigateBack) {
                        Icon(Icons.Default.ArrowBack, contentDescription = "Voltar")
                    }
                }
            )
        },
        snackbarHost = { SnackbarHost(snackbarHostState) }
    ) { padding ->
        Column(
            modifier = Modifier
                .fillMaxSize()
                .padding(padding)
                .padding(16.dp)
        ) {
            OutlinedTextField(
                value = estado.titulo,
                onValueChange = { 
                    viewModel.processarAcao(AdicionarAcao.AtualizarTitulo(it))
                },
                label = { Text("Título *") },
                placeholder = { Text("Digite o título") },
                modifier = Modifier.fillMaxWidth(),
                singleLine = true,
                isError = estado.erro != null && estado.titulo.isBlank()
            )
            
            Spacer(modifier = Modifier.height(16.dp))
            
            OutlinedTextField(
                value = estado.descricao,
                onValueChange = {
                    viewModel.processarAcao(AdicionarAcao.AtualizarDescricao(it))
                },
                label = { Text("Descrição") },
                placeholder = { Text("Digite a descrição (opcional)") },
                modifier = Modifier
                    .fillMaxWidth()
                    .height(120.dp),
                maxLines = 4
            )
            
            Spacer(modifier = Modifier.height(24.dp))
            
            Button(
                onClick = { viewModel.processarAcao(AdicionarAcao.SalvarTarefa) },
                modifier = Modifier.fillMaxWidth(),
                enabled = !estado.isLoading
            ) {
                if (estado.isLoading) {
                    CircularProgressIndicator(
                        modifier = Modifier.size(24.dp),
                        color = MaterialTheme.colorScheme.onPrimary
                    )
                } else {
                    Text("Salvar Tarefa")
                }
            }
        }
    }
}
```

### Navigation

```
package com.tarefas.presentation.navigation

import androidx.compose.runtime.Composable
import androidx.navigation.compose.NavHost
import androidx.navigation.compose.composable
import androidx.navigation.compose.rememberNavController
import com.tarefas.presentation.screens.adicionar.AdicionarScreen
import com.tarefas.presentation.screens.home.HomeScreen

sealed class Rota(val path: String) {
    object Home : Rota("home")
    object Adicionar : Rota("adicionar")
}

@Composable
fun Navegacao() {
    val navController = rememberNavController()
    
    NavHost(
        navController = navController,
        startDestination = Rota.Home.path
    ) {
        composable(Rota.Home.path) {
            HomeScreen(
                onNavigateToAdicionar = {
                    navController.navigate(Rota.Adicionar.path)
                }
            )
        }
        
        composable(Rota.Adicionar.path) {
            AdicionarScreen(
                onNavigateBack = { navController.popBackStack() }
            )
        }
    }
}
```

### Theme

```
package com.tarefas.presentation.theme

import android.app.Activity
import androidx.compose.foundation.isSystemInDarkTheme
import androidx.compose.material3.*
import androidx.compose.runtime.Composable
import androidx.compose.runtime.SideEffect
import androidx.compose.ui.graphics.toArgb
import androidx.compose.ui.platform.LocalView
import androidx.core.view.WindowCompat

private val DarkColorScheme = darkColorScheme(
    primary = Purple80,
    secondary = PurpleGrey80,
    tertiary = Pink80
)

private val LightColorScheme = lightColorScheme(
    primary = Purple40,
    secondary = PurpleGrey40,
    tertiary = Pink40
)

@Composable
fun TarefasTheme(
    darkTheme: Boolean = isSystemInDarkTheme(),
    content: @Composable () -> Unit
) {
    val colorScheme = if (darkTheme) DarkColorScheme else LightColorScheme
    
    val view = LocalView.current
    if (!view.isInEditMode) {
        SideEffect {
            val window = (view.context as Activity).window
            window.statusBarColor = colorScheme.primary.toArgb()
            WindowCompat.getInsetsController(window, view).isAppearanceLightStatusBars = !darkTheme
        }
    }
    
    MaterialTheme(
        colorScheme = colorScheme,
        typography = Typography,
        content = content
    )
}
```

### App

```
package com.tarefas

import android.app.Application
import dagger.hilt.android.HiltAndroidApp

@HiltAndroidApp
class App : Application()
```

### MainActivity

```
package com.tarefas

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.Surface
import androidx.compose.ui.Modifier
import com.tarefas.presentation.navigation.Navegacao
import com.tarefas.presentation.theme.TarefasTheme
import dagger.hilt.android.AndroidEntryPoint

@AndroidEntryPoint
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            TarefasTheme {
                Surface(
                    modifier = Modifier.fillMaxSize(),
                    color = MaterialTheme.colorScheme.background
                ) {
                    Navegacao()
                }
            }
        }
    }
}
```

---

## 11.4 Fluxo de Dados Detalhado

[DIAGRAMA do fluxo completo:

1. Usuário clica em FAB (+)
2. HomeScreen dispara HomeAcao.NavegarPara para AdicionarScreen
3. Usuário preenche título e descrição
4. AdicionarScreen dispara AdicionarAcao.AtualizarTitulo
5. reducer atualiza estado com novo título
6. UI recompõe e mostra título
7. Usuário clica em Salvar
8. AdicionarScreen dispara AdicionarAcao.SalvarTarefa
9. AdicionarViewModel.valida campos
10. Se válido, cria Tarefa com UUID
11. Chama AdicionarTarefaUseCase(tarefa)
12. Repository adiciona no DataStore
13. Dispara AdicionarEfeito.TarefaSalva
14. AdicionarScreen recebe efeito e navega para Home
15. HomeScreen mostra lista atualizada

]

---

# EXTRAS

## Performance em Compose

### Evite recomposições desnecessárias

```
@Composable
fun TelaOtimizada() {
    val estado by remember { mutableStateOf(valorInicial) }
    // ✅Bom
}

@Composable
fun TelaRuim() {
    var estado = mutableStateOf(valorInicial)
    // ❌ Problema
}
```

### Use derivedStateOf para cálculos caros

```
@Composable
fun ListaOtimizada(itens: List<Item>) {
    val listaFiltrada = remember(itens) {
        derivedStateOf {
            itens.filter { /* filtro caro */ }
        }
    }
}
```

## Erros Comuns

### 1. Esquecer de usar viewModelScope

```
// ❌ Errado
fun carregar() {
    repository.buscar().collect { }
}

// ✅ Correto
viewModelScope.launch {
    repository.buscar().collect { }
}
```

### 2. Modificar estado diretamente

```
// ❌ Errado
var estado = _estado.value
estado.lista.add(novoItem)

// ✅ Correto
_estado.value = estado.copy(lista = estado.lista + novoItem)
```

### 3. Não tratar erros

```
// ❌ Errado
repository.buscar().collect { }

// ✅ Correto
repository.buscar()
    .catch { _estado.value = erro }
    .collect { }
```

### 4. Não usar StateFlow/SharedFlow para UI

```
// ❌ Errado
valliveData = MutableLiveData()

// ✅ Correto
val estado = MutableStateFlow(Estado())
```

---

## Padrões Usados por Empresas Reais

### Multi-module

- Módulos separados por feature
- build.gradle para cada módulo
- version catalog

### Feature Flags

```
config {
    buildConfigField "boolean", "FEATURE_NOVA_TELA", "true"
}
```

### Analytics

```
class AnalyticsService {
    fun evento(nome: String, parametros: Map<String, Any>)
}
```

### Error Tracking

```
// Sentry, Firebase Crashlytics
```

---

## Boas Práticas

1. **Sempre use Hilt/Koin** para DI
2. **MVI** para telas complexas
3. **Clean Architecture** em projetos grandes
4. **Testes unitários** para Use Cases
5. **Testes de UI** para telas críticas
6. **Code review** obrigatório
7. **Lint** antes de commit
8. **Compose** para toda nova UI
9. **Material 3** como padrão visual

---

# CONCLUSÃO

Este guia cobriu:
- ✅ Kotlin (do básico ao avançado)
- ✅ Jetpack Compose (UI moderna)
- ✅ Arquitetura (MVVM/MVI/Clean)
- ✅ DI (Hilt/Koin)
- ✅ Networking (Retrofit/Ktor)
- ✅ Coroutines/Flow
- ✅ Persistência (DataStore)
- ✅ Navegação
- ✅ Testes (JUnit/MockK/Turbine)
- ✅ SOLID/Clean Code
- ✅ Projeto completo

Você agora tem conhecimento para desenvolver apps Android profissionais, prontos para o mercado de trabalho!

---

**[PÁGINA FINAL]**

```
┌─────────────────────────────────────────┐
│                                         │
│        OBRIGADO POR APRENDER             │
│                                         │
│     Continue praticando e evoluindo!     │
│                                         │
│           ─────────────                 │
│                                         │
│     [ilustração: desenvolvedor          │
│      celebrando com dispositivos        │
│      Android ao fundo]                 │
│                                         │
│                                         │
└─────────────────────────────────────────┘
```
