[Guia_Completo_Android_Detalhado.md](https://github.com/user-attachments/files/26862370/Guia_Completo_Android_Detalhado.md)
# GUIA COMPLETO E DETALHADO DE DESENVOLVIMENTO ANDROID MODERNO
## Kotlin, Jetpack Compose, Arquitetura, Testes e Firebase

---

# ÍNDICE DETALHADO

1. Kotlin Fundamentos - Explicação Completa
2. Jetpack Compose - Detalhado
3. Arquitetura MVVM e MVI - Completo
4. Injeção de Dependência Hilt e Koin
5. Room Database - Completo
6. Retrofit e Ktor - Explicação
7. Coroutines e Flow - Detalhado
8. Firebase - Completo
9. Testes Unitários - Completo com Exemplos
10. Mockito - vs MockK
11. Espresso - Complete com Exemplos
12. Navigation Compose - Detalhado
13. Room - Mais Detalhes
14. SOLID e Clean Code
15. Projeto Completo

---

# CAPÍTULO 1: KOTLIN FUNDAMENTOS

## 1.1 Por que aprender Kotlin?

Kotlin é a linguagem oficial do Android desde 2017. O Google exige Kotlin para novos projetos.

**Vantagens sobre Java:**
- Null safety (elimina NullPointerException)
- Sintaxe muito mais curta
- Suporte nativo a coroutines
- Código mais seguro
- Menos boilerplate (código repetitivo)

**Exemplo comparativo:**

```
// Java - muito código
public class Usuario {
    private String nome;
    private int idade;
    
    public Usuario(String nome, int idade) {
        this.nome = nome;
        this.idade = idade;
    }
    
    public String getNome() { return nome; }
    public void setNome(String nome) { this.nome = nome; }
    public int getIdade() { return idade; }
    public void setIdade(int idade) { this.idade = idade; }
    
    @Override
    public String toString() {
        return "Usuario{" +
                "nome='" + nome + '\'' +
                ", idade=" + idade +
                '}';
    }
}

// Kotlin - uma linha!
data class Usuario(val nome: String, val idade: Int)

// data class já gera:
// - getters e setters automáticos
// - equals() e hashCode()
// - toString()
// - copy()
// - componentN()
```

## 1.2 Variáveis: val vs var - Explicação Completa

Há dois tipos de variáveis em Kotlin:

### val - Variável Imutável

```
val nome = "João"
nome = "Maria"  // ERRO! Não pode atribuir novamente
```

**Quando usar:** Sempre que possível! É mais seguro e previsível.

### var - Variável Mutável

```
var idade = 25
idade = 26  // OK!
```

**Quando usar:** Apenas quando o valor precisa mudar (contador, campo de formulário, estado de UI).

### Exemplo prático em app Android

```
// ❌ RUIM - usar var quando não precisa
@Composable
fun BotaoContador() {
    var texto = "Clique"  // nunca muda!
    Button(onClick = { }) {
        Text(texto)
    }
}

// ✅ BOM - usar val quando não precisa
@Composable
fun BotaoContador() {
    val texto = "Clique"
    Button(onClick = { }) {
        Text(texto)
    }
}

// ✅ CERTO - usar var quando PRECISA mudar
@Composable
fun CampoNome() {
    var nome by remember { mutableStateOf("") }
    OutlinedTextField(
        value = nome,
        onValueChange = { nome = it }  // precisa mudar!
    )
}
```

### Resumo

| Tipo | Pode mudar valor | Performance | Recomendação |
|------|----------------|-------------|--------------|
| val | ❌ | Melhor | ✅ Padrão |
| var | ✅ | Similar | Apenas se necessário |

## 1.3 Null Safety - O Problema e a Solução

### O problema em Java/NullPointerException

O NullPointerException (NPE) é o erro mais comum em Java:

```
// Java
String texto = null;
texto.length();  // 💥 NullPointerException! App crasha!
```

Isso acontece porque qualquer objeto pode ser nulo em Java.

### A solução do Kotlin

Kotlin resolve isso com sistema de tipos:

```
var texto: String = "Hello"    // NÃO pode ser nulo - compila não!
var textoNulo: String? = null  // pode ser nulo, mas precisa tratar
```

### Operadores de Null Safety

#### 1. Operador ?.(chamada segura)

```
var texto: String? = "Hello"
texto?.length  // retorna 5

texto = null
texto?.length  // retorna null (não crasha!)
```

#### 2. Operador ?: (Elvis)

```
var nome: String? = null
val tamanho = nome?.length ?: 0  // retorna 0 se for nulo

// Equivalente em Java:
int tamanho = nome != null ? nome.length() : 0;
```

#### 3. Operador !! (Not-null assertion)

⚠️ **PERIGOSO!** Use apenas se TEM CERTEZA que não é nulo:

```
var texto: String = "Hello"
texto!!.length  // 5 - OK

texto = null
texto!!.length  // 💥 NullPointerException! Crashou!
```

**Regra:** Evite `!!`. Use `?.` ou `?:`.

### Exemplo prático em app

```
// ❌ RUIM - pode crashar!
fun mostrarNome(usuario: Usuario?) {
    println(usuario!!.nome)  // Crash se usuario for null!
}

// ✅ BOM - trata null
fun mostrarNome(usuario: Usuario?) {
    println(usuario?.nome ?: "Usuário não encontrado")
}

// ✅ MELHOR - verificação explícita
fun mostrarNome(usuario: Usuario?) {
    if (usuario != null) {
        println(usuario.nome)  // Kotlin smart cast!
    } else {
        println("Usuário não encontrado")
    }
}
```

## 1.4 Data Class - Para que serve?

Data class é uma classe especial para **armazenar dados**. Ela gera automaticamente:

- `equals()` - comparar objetos
- `hashCode()` - usar em Collections
- `toString()` - debugar
- `copy()` - copiar com alterações
- `componentN()` - destructuring

### Exemplo

```
// Uma linha! Java precisaria de 50+ linhas
data class Usuario(
    val id: Int,
    val nome: String,
    val email: String,
    val idade: Int = 0  // valor padrão
)

fun main() {
    val u1 = Usuario(1, "João", "joao@email.com")
    val u2 =Usuario(1, "João", "joao@email.com")
    
    // equals() gerado automaticamente
    println(u1 == u2)  // true!
    
    // toString() gerado automaticamente
    println(u1)
    // Usuario(id=1, nome=João, email=joao@email.com, idade=0)
    
    // copy() para criar variações
    val u3 = u1.copy(nome = "Maria")
    // Usuario(id=1, nome=Maria, email=joao@email.com, idade=0)
    
    // Destructuring
    val (id, nome, email, _) = u1
    println(nome)  // João
}
```

### Quando usar data class?

✅ Para:
- DTOs (Data Transfer Objects)
- Models de API
- Estados de tela (UI state)
- Entidades de banco
- Objetos de valor

❌ Não usar para:
- Classes com comportamento (use class normal)
- Entidades de domínio com lógica

### Equivalente em Java (para comparação)

```
// Kotlin: 5 linhas
data class Usuario(val id: Int, val nome: String)

// Java: 80+ linhas necessárias
public class Usuario {
    private final int id;
    private final String nome;
    
    public Usuario(int id, String nome) {
        this.id = id;
        this.nome = nome;
    }
    
    public int getId() { return id; }
    public String getNome() { return nome; }
    
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Usuario usuario = (Usuario) o;
        return id == usuario.id && Objects.equals(nome, usuario.nome);
    }
    
    @Override
    public int hashCode() {
        return Objects.hash(id, nome);
    }
    
    @Override
    public String toString() {
        return "Usuario{" +
                "id=" + id +
                ", nome='" + nome + '\'' +
                '}';
    }
    
    public Usuario copy(int id, String nome) {
        return new Usuario(id, nome);
    }
}
```

## 1.5 Sealed Class - Para que serve?

Sealed class cria uma **hierarquia fechada** de classes. Isso é útil para:

- Estados de tela
- Resultados de operações
- Eventos
- navegação

### Exemplo de Estado de Tela

```
// Java seria possível fazer subclasses em qualquer lugar
// Kotlin sealed class limita a subclasses ao mesmo arquivo

sealed class Resultado<out T> {
    data class Sucesso<T>(val dados: T) : Resultado<T>()
    data class Erro(val mensagem: String, val codigo: Int? = null) : Resultado<Nothing>()
    object Carregando : Resultado<Nothing>()
}

fun processarResultado(resultado:Resultado<String>) {
    when (resultado) {
        is Resultado.Sucesso -> println(resultado.dados)
        is Resultado.Erro -> println("Erro: ${resultado.mensagem}")
        is Resultado.Carregando -> println("Carregando...")
        // ✅ O compilador garante quetratamos todos os casos!
    }
}
```

**Vantagem:** O compilador garante que você tratou todos os casos!

### Exemplo prático em ViewModel

```
sealed class EstadoTarefa {
    object Carregando : EstadoTarefa()
    data class Sucesso(val tarefas: List<Tarefa>) : EstadoTarefa()
    data class Erro(val mensagem: String) : EstadoTarefa()
}

@Composable
fun TelaTarefas(estado: EstadoTarefa) {
    when (estado) {
        is EstadoTarefa.Carregando -> CircularProgressIndicator()
        is EstadoTarefa.Sucesso -> ListaTarefas(estado.tarefas)
        is EstadoTarefa.Erro -> Text(estado.mensagem)
    }
}
```

## 1.6 Lambdas - Explicação Completa

### O que é Lambda?

Lambda é uma **função anônima** (sem nome). Útil para:

- Callbacks
- Operações em listas
- Eventos de UI

### Sintaxe

```
{ parametros -> corpo }
```

### Exemplo de lista

```
val numeros = listOf(1, 2, 3, 4, 5)

// map - transformar cada elemento
val dobrados = numeros.map { it * 2 }
// Resultado: [2, 4, 6, 8, 10]

// filter - filtrar elementos
val pares = numeros.filter { it % 2 == 0 }
// Resultado: [2, 4]

// reduce - acumular em um valor
val soma = numeros.reduce { acc, n -> acc + n }
// Resultado: 15
```

### O parâmetro "it"

Quando a lambda tem **um único parâmetro**, use `it`:

```
val numeros = listOf(1, 2, 3)

// com it
val dobrados = numeros.map { it * 2 }

// equivalente sem it
val dobrados = numeros.map { numero -> numero * 2 }
```

### Exemplo em botão

```
Button(onClick = { /* código */ }) { }

// é equivalente a:
val onClick = { /* código */ }
Button(onClick = onClick) { }
```

## 1.7 Funções de Escopo - let, apply, also, run, with

Kotlin tem funções de escopo muito úteis para manipular objetos de forma concisa:

### let - Para que serve?

Executa código **apenas se o objeto não for nulo**:

```
var texto: String? = "Hello"
texto?.let {
    println("O texto tem ${it.length} caracteres")
}

texto = null
texto?.let {
    println("Isso não será executado")
}
```

**Uso prática em transformations:**

```
val nome = usuario?.let { it.nome.uppercase() }
// Se não for nulo, retorna nome em uppercase. Se nulo, retorna null.
```

### apply - Para que serve?

Executa código no **mesmo objeto** e retorna o **objeto modificado**:

```
val pessoa = Pessoa().apply {
    nome = "João"
    idade = 25
}
// Resultado: Pessoa(nome=João, idade=25)
```

**Uso prática para configuração:**

```
val intent = Intent().apply {
    putExtra("id", 1)
    putExtra("nome", "João")
    flags = Intent.FLAG_ACTIVITY_NEW_TASK
}
```

### also - Para que serve?

Executa código mas **retorna o objeto original** (não o modificado):

```
val lista = mutableListOf<Int>().also {
    it.add(1)
    it.add(2)
    it.add(3)
}
// Resultado: [1, 2, 3]
```

**Diferença apply vs also:**

```
// apply retorna o objeto modificado
val p1 = Pessoa().apply { nome = "João" }  // retorna Pessoa

// also retorna o objeto original (útil para encadear)
val lista = mutableListOf<Int>()
    .also { it.add(1) }  // retorna a lista
    .also { it.add(2) }  // retorna a lista
    .also { it.add(3) }  // retorna [1,2,3]
```

### run - Para que serve?

Executa código e **retorna o resultado**:

```
val resultado = objeto.run {
    // código
    valorFinal
}
```

**Útil para блок de código:**

```
val url = "https://api.com"
    .run { URL(this) }
    .run { openConnection() }
    .run { connect() }
```

### with - Para que serve?

Semelhante a run, mas como **função livre**:

```
val resultado = with(pessoa) {
    "$nome tem $idade anos"
}
// Resultado: "João tem 25 anos"
```

### Resumo

| Função | Retorna | Uso principal |
|--------|--------|------------|
| `let` | Resultado do bloco | Null check, transformações |
| `apply` | Objeto modificado | Configuração |
| `also` | Objeto original | Ações adicionais |
| `run` | Resultado do bloco | Bloco de código |
| `with` | Resultado do bloco | Com objeto como parâmetro |

### Exemplo prático em app

```
// Sem função de escopo
val intent = Intent()
intent.putExtra("id", 1)
intent.putExtra("nome", "João")
intent.flags = Intent.FLAG_ACTIVITY_NEW_TASK

// Com apply - mais limpo
val intent = Intent().apply {
    putExtra("id", 1)
    putExtra("nome", "João")
    flags = Intent.FLAG_ACTIVITY_NEW_TASK
}
```

## 1.8 Sealed Classes - Explicação Completa

Sealed class cria uma **hierarquia fechada** de classes. É usado para:

- Estados de tela
- Resultados de operações
- Eventos
- navegación

### Por que usar?

O compilador Kotlin **garante** que você tratou todos os casos!

```
sealed class Resultado<out T> {
    data class Sucesso<T>(val dados: T) : Resultado<T>()
    data class Erro(val mensagem: String) : Resultado<Nothing>()
    object Carregando : Resultado<Nothing>()
}

fun processar(resultado: Resultado<String>) {
    when (resultado) {
        is Resultado.Sucesso -> println(resultado.dados)
        is Resultado.Erro -> println("Erro: ${resultado.mensagem}")
        is Resultado.Carregando -> println("Carregando...")
        // ✅ Compilador garante que tratamos tudo!
    }
}
```

### Exemplospráticos

**Estado de tela:**
```
sealed class EstadoTarefa {
    object Carregando : EstadoTarefa()
    data class Sucesso(val tarefas: List<Tarefa>) : EstadoTarefa()
    data class Erro(val mensagem: String) : EstadoTarefa()
}
```

**Resultado de operação:**
```
sealed class Resultado<out T> {
    data class Ok<T>(val value: T) : Resultado<T>()
    data class Error(val exception: Throwable) : Resultado<Nothing>()
}

fun <T> executar(): Resultado<T> {
    return try {
        Resultado.Ok(valor)
    } catch (e: Exception) {
        Resultado.Error(e)
    }
}
```

**Navegação:**
```
sealed class Rota(val path: String) {
    object Home : Rota("home")
    object Detalhes : Rota("detalhes/{id}") {
        fun criar(id: String) = "detalhes/$id"
    }
    object Login : Rota("login")
}
```

## 1.9 Enum - Explicação Completa

Enum é uma classe que representa um **conjunto fixo de constantes**.

```
enum class DiaSemana {
    DOMINGO,
    SEGUNDA,
    TERÇA,
    QUARTA,
    QUINTA,
    SEXTA,
    SÁBADO
}

// Usar
val hoje = DiaSemana.QUARTA
when (hoje) {
    DiaSemana.DOMINGO -> "Descanso"
    DiaSemana.SÁBADO -> "Quase lá"
    else -> "Dia útil"
}
```

### Enum com propriedades

```
enum class StatusHTTP(val codigo: Int, val descricao: String) {
    OK(200, "Sucesso"),
    CREATED(201, "Criado"),
    BAD_REQUEST(400, "Requisição inválida"),
    NOT_FOUND(404, "Não encontrado"),
    ERRO_INTERNO(500, "Erro interno")
}

// Usar
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

### Enum vs Sealed Class

| Característica | Enum | Sealed Class |
|---------------|------|-------------|
| Instâncias | Fixas (enum constants) | Ilimitadas (data classes) |
| properties | Sim | Sim |
| Métodos | Sim | Sim |
| Quando usar | Valores fixos | Estados/resultados complexos |

Exemplo de quando usar **enum**:
```
enum class TipoTarefa { URGENTE, NORMAL, BAIXA }
```

Exemplo de quando usar **sealed class**:
```
sealed class Resultado { Ok, Error, Loading }
```

## 1.10 Coleções - Lista, Set, Map

### List (lista ordenada)

```
val numeros = listOf(1, 2, 3)
numeros[0]  // 1 - primeiro elemento
numeros.first()  // 1
numeros.last()  // 3

// imutável - não pode adicionar
val mutavel = mutableListOf(1, 2, 3)
mutavel.add(4)
```

### Map (dicionário/chave-valor)

```
val usuarios = mapOf(
    1 to "João",
    2 to "Maria"
)

usuarios[1]  // "João"
usuarios.getOrDefault(3, "Não encontrado")  // "Não encontrado"

for ((id, nome) in usuarios) {
    println("$id: $nome")
}
```

### Set (conjunto único)

```
val numeros = setOf(1, 2, 2, 3, 3)
// Resultado: {1, 2, 3} - elimina duplicatas

1 in numeros  // true
4 in numeros  // false
```

---

# CAPÍTULO 2: JETPACK COMPOSE

## 2.1 O que é Compose e para que serve?

Compose é o **toolkit moderno de UI do Android**. Substitui XMLs por código Kotlin.

**Benefícios:**
- Menos código (até 50% menos)
- UI declarativa (diz o que quer, não como fazer)
- Estado reativo (UI atualiza automaticamente)
- Preview (visualiza sem rodar app)

### Comparação

```
// XML ANTIGO (30+ linhas)
<androidx.constraintlayout.widget.ConstraintLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    
    <TextView
        android:id="@+id/texto"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Olá Mundo" />
        
    <Button
        android:id="@+id/botao"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Clique" />
</androidx.constraintlayout.widget.ConstraintLayout>

// COMPOSE (5 linhas)
Column(modifier = Modifier.fillMaxSize()) {
    Text("Olá Mundo")
    Button(onClick = { /* ação */ }) {
        Text("Clique")
    }
}
```

## 2.2 @Composable - Para que serve?

@Composable informa ao Compose que a função **descreve UI**:

```
@Composable
fun Saudacao() {
    Text("Olá Mundo!")
}
```

**Importante:** Apenas funções @Composable podem chamar outras funções @Composable.

## 2.3 Estado e remember - Explicação

### O problema

Em XML, o estado era gerenciado manualmente:

```
// Java - gerenciar manualmente
texto = findViewById(R.id.texto);
contador = findViewById(R.id.contador);

botao.setOnClickListener(new OnClickListener() {
    @Override
    public void onClick(View v) {
        valor++;
        texto.setText("Contagem: " + valor);
    }
});
```

### A solução do Compose

O Compose usa **estado reativo** - mude o estado, a UI atualiza automaticamente:

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

**O que acontece:**
1. Usuário clica no botão
2. `contador++` muda o estado
3. Compose detecta mudança
4. UI recompõe automaticamente com novo valor

### remember - Para que serve?

`remember` **salva o estado entre recomposições**:

```
@Composable
fun MinhaTela() {
    var nome by remember { mutableStateOf("") }
    OutlinedTextField(value = nome, onValueChange = { nome = it })
}
```

Sem `remember`, o estado seria **resetado** a cada recomposição!

### mutableStateOf - Para que serve?

Cria um estado **reativo**. Quando mudado, Compose recompõe a UI:

```
val estado by lazy { mutableStateOf(0) }
// Quando `estado.value` muda,
// todas as linhas que usam `estado` são reexecutadas
```

## 2.4 Layouts - Explicação Completa

### Column (vertical)

```
@Composable
fun LayoutVertical() {
    Column(
        modifier = Modifier.fillMaxSize(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center,
        contentPadding = PaddingValues(16.dp)
    ) {
        Text("Linha 1")
        Text("Linha 2")
        Text("Linha 3")
    }
}
```

### Row (horizontal)

```
@Composable
fun LayoutHorizontal() {
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

### Box (livre/sobreposto)

```
@Composable
fun LayoutBox() {
    Box(
        modifier = Modifier.fillMaxSize(),
        contentAlignment = Alignment.Center
    ) {
        Text("Centralizado!")
    }
}
```

### LazyColumn (lista virtualizada)

```
@Composable
fun Lista() {
    val itens = (1..100).toList()
    
    LazyColumn(
        modifier = Modifier.fillMaxSize(),
        verticalArrangement = Arrangement.spacedBy(8.dp),
        contentPadding = PaddingValues(16.dp)
    ) {
        items(itens) { numero ->
            Text("Item $numero")
        }
    }
}
```

**Por que LazyColumn?** Renderiza apenas itens visíveis na tela. Com 10.000 itens, ListView normal pode lento. LazyColumn é rápido!

## 2.5 Modifier - Para que serve?

Modifier **personaliza** elementos:

```
Text(
    text = "Olá",
    modifier = Modifier
        .padding(16.dp)           // espaço externo
        .background(Color.Blue) // cor de fundo
        .clickable { /* ação */ }  // clicável
        .size(100.dp)            // tamanho
)
```

### Modifiers mais comuns

```
Modifier
    .fillMaxSize()           // ocupa todo o espaço
    .fillMaxWidth()         // ocupa largura total
    .fillMaxHeight()         // ocupa altura total
    .size(largura, altura)  // tamanho específico
    .padding(valor)         // margem externa
    .padding(horizontal, vertical)
    .background(cor)        // cor de fundo
    .border(largura, cor)    // borda
    .clickable { }         // torna clicável
    .testTag("tag")         // tag para teste
```

## 2.6 Material 3 - Sistema de Design

Material 3 é o sistema de design do Google:

### Cores

```
MaterialTheme.colorScheme.primary        // cor principal
MaterialTheme.colorScheme.onPrimary   // texto em primary
MaterialTheme.colorScheme.secondary   // cor secundária
MaterialTheme.colorScheme.surface       // cor de superfície
MaterialTheme.colorScheme.background // cor de fundo
MaterialTheme.colorScheme.error       // cor de erro
```

### Componentes

```
Button(onClick = { }) { Text("Primário") }

OutlinedButton(onClick = { }) { Text("Outline") }

TextButton(onClick = { }) { Text("Texto") }

FloatingActionButton(onClick = { }) {
    Icon(Icons.Default.Add, contentDescription = "Adicionar")
}

Card(modifier = Modifier.fillMaxWidth()) {
    Column(modifier = Modifier.padding(16.dp)) {
        Text("Título")
    }
}

OutlinedTextField(
    value = texto,
    onValueChange = { texto = it },
    label = { Text("Nome") },
    placeholder = { Text("Digite seu nome") },
    isError = false,
    singleLine = true
)
```

---

# CAPÍTULO 3: ARQUITETURA

## 3.1 Por que usar arquitetura?

Sem arquitetura:

[DIAGRAMA]
```
┌─────────────────────────────────────────┐
│          CODE SPAGHETTI                  │
├─────────────────────────────────────────┤
│                                          │
│  MainActivity.kt                        │
│  - 2000 linhas                         │
│  - UI + lógica + banco + API           │
│  - Impossível de testar                │
│  - Impossível de manter                │
│  - Bugs层出不穷                         │
│                                          │
│  Problemas:                            │
│  ❌ Difícil testar                     │
│  ❌ Difícil manter                     │
│  ❌ Trabalho em equipe impossível      │
│  ❌ Bugs frequentes                    │
│  ❌ Refatoração arriscada              │
└─────────────────────────────────────────┘
```

Com arquitetura:

[DIAGRAMA]
```
┌─────────────────────────────────────────┐
│        ARQUITETURA ORGANIZADA            │
├─────────────────────────────────────────┤
│  UI Layer      → Telas, ViewModels      │
│  Domain Layer → Regras de negócio      │
│  Data Layer   → API, Banco, Repos    │
│                                          │
│  Benefícios:                           │
│  ✅ Fácil de testar                   │
│  ✅ Fácil de manter                   │
│  ✅ Trabalho em equipe                │
│  ✅ Bugs menos frequentes             │
│  ✅ Refatoração segura               │
└─────────────────────────────────────────┘
```

## 3.2 MVVM - Explicação Detalhada

### O que é MVVM?

MVVM (Model-View-ViewModel) separa UI da lógica:

[DIAGRAMA]
```
┌─────────────────────────────────────────┐
│              MVVM                       │
├─────────────────────────────────────────┤
│                                          │
│   ┌──────────┐                          │
│   │   UI    │ ← Composables (@Composable)│
│   │  View   │                          │
│   └────┬───┘                          │
│        │ usa                           │
│        ↓                               │
│   ┌──────────┐   ViewModel        │
│   │         │ ← Mantém estado      │
│   │         │ ← processa ações     │
│   │         │ ← chama Use Cases  │
│   └────┬───┘                          │
│        │                               │
│        ↓                               │
│   ┌──────────┐   dados         │
│   │   API    │ ← busca/salva     │
│   │ Repository                         │
│   └──────────┘                        │
└─────────────────────────────────────────┘
```

### Por que usar MVVM?

1. **Separation of Concerns** - UI separada da lógica
2. **Testabilidade** - ViewModel pode ser testado sem UI
3. **Manutenibilidade** - código organizado
4. **State Management** - estado centralizado

### Exemplo completo

**1. Model (dados)**

```
data class Usuario(
    val id: String,
    val nome: String,
    val email: String,
    val avatarUrl: String?
)
```

**2. Repository (acesso a dados)**

```
class UsuarioRepository {
    private val api = RetrofitClient.api
    
    suspend fun buscarUsuario(id: String): Usuario {
        return api.buscarUsuario(id).toDomain()
    }
    
    suspend fun salvarUsuario(usuario: Usuario) {
        api.criarUsuario(usuario.toDto())
    }
}
```

**3. ViewModel (lógica)**

```
@HiltViewModel
class UsuarioViewModel @Inject constructor(
    private val repository: UsuarioRepository
) : ViewModel() {
    
    private val _estado = MutableStateFlow(EstadoUsuario())
    val estado: StateFlow<EstadoUsuario> = _estado
    
    init {
        carregarUsuario("1")
    }
    
    fun carregarUsuario(id: String) {
        viewModelScope.launch {
            _estado.value = _estado.value.copy(isLoading = true, erro = null)
            try {
                val usuario = repository.buscarUsuario(id)
                _estado.value = _estado.value.copy(
                    usuario = usuario,
                    isLoading = false
                )
            } catch (e: Exception) {
                _estado.value = _estado.value.copy(
                    isLoading = false,
                    erro = e.message ?: "Erro ao carregar"
                )
            }
        }
    }
}

data class EstadoUsuario(
    val usuario: Usuario? = null,
    val isLoading: Boolean = false,
    val erro: String? = null
)
```

**4. Screen (UI)**

```
@Composable
fun TelaUsuario(
    viewModel: UsuarioViewModel = hiltViewModel()
) {
    val estado by viewModel.estado.collectAsState()
    
    when {
        estado.isLoading -> CircularProgressIndicator()
        estado.erro != null -> Column {
            Text(estado.erro!!, color = Color.Red)
            Button(onClick = { viewModel.carregarUsuario("1") }) {
                Text("Tentar novamente")
            }
        }
        else -> Column {
            Text(estado.usuario?.nome ?: "")
            Text(estado.usuario?.email ?: "")
        }
    }
}
```

## 3.3 MVI - Explicação Completa

### O que é MVI?

MVI (Model-View-Intent) é uma versão mais robusta do MVVM:

[DIAGRAMA]
```
┌─────────────────────────────────────────┐
│              MVI                       │
├─────────────────────────────────────────┤
│                                          │
│  ┌─────────┐   Estado (single source)      │
│  │ STATE  │ ← ÚNICO estado da tela     │
│  └────┬────┘                          │
│       │ observe                       │
│       ↓ renderiza                       │
│  ┌─────────┐                          │
│  │   UI    │ ← Composables          │
│  │        │                          │
│  │        │                          │
│  └────┬────┘                          │
│       │ action                         │
│       ↓                                │
│  ┌─────────┐   Intent (ação do usuário)│
│  │ INTENT │                          │
│  └────┬────┘                          │
│       │ processa                       │
│       ↓                                │
│  ┌─────────┐   Reducer                 │
│  │REDUCER │ ← Transforma estado        │
│  └────┬────┘                          │
│       │ new state                     │
│       ↓                                │
│  ┌─────────┐   Effects                  │
│  │EFFECTS │ ← Navegação, snackbar    │
│  └─────────┘                          │
└─────────────────────────────────────────┘
```

### Por que MVI é melhor que MVVM?

| Aspecto | MVVM | MVI |
|--------|------|-----|
| Estado | Múltiplos LiveData | Estado único |
| Previsibilidade | Média | Alta (fluxo único) |
| Bugs de race condition | Mais provável | Menos provável |
| Testabilidade | Boa | Excelente |
| Complexidade | Menor | Maior |

### Implementação detalhada

**1. Estado (State)**

```
data class EstadoTarefa(
    val isLoading: Boolean = false,
    val tarefas: List<Tarefa> = emptyList(),
    val tarefaSelecionada: Tarefa? = null,
    val erro: String? = null,
    val filtro: TipoFiltro = TipoFiltro.TODOS
)

enum class TipoFiltro {
    TODOS,
    PENDENTES,
    CONCLUIDAS
}
```

**2. Ações (Intent)**

```
sealed class Acao {
    object CarregarTarefas : Acao()
    data class SelecionarTarefa(val tarefa: Tarefa) : Acao()
    data class ConcluirTarefa(val id: String) : Acao()
    data class ExcluirTarefa(val id: String) : Acao()
    data class Filtrar(val tipo: TipoFiltro) : Acao()
    object LimparErro : Acao()
}
```

**3. Efeitos (Effects)**

```
sealed class Efeito {
    data class MostrarSnackbar(val mensagem: String) : Efeito()
    data class NavegarPara(val tela: String) : Efeito()
    objectMostrarDialogConfirmacao : Efeito()
}
```

**4. Reducer**

```
fun reducer(estado:EstadoTarefa, acao:Acao):EstadoTarefa {
    return when (acao) {
        is Acao.CarregarTarefas -> estado.copy(
            isLoading = true,
            erro = null
        )
        is Acao.Sucesso -> estado.copy(
            isLoading = false,
            tarefas = acao.tarefas
        )
        is Acao.Erro -> estado.copy(
            isLoading = false,
            erro = acao.mensagem
        )
        is Acao.SelecionarTarefa -> estado.copy(
            tarefaSelecionada = acao.tarefa
        )
        is Acao.Filtrar -> estado.copy(
            filtro = acao.tipo,
            tarefas = filtrarTarefas(estado.tarefas, acao.tipo)
        )
        is Acao.LimparErro -> estado.copy(erro = null)
    }
}

private fun filtrarTarefas(tarefas: List<Tarefa>, filtro: TipoFiltro): List<Tarefa> {
    return when (filtro) {
        TipoFiltro.TODOS -> tarefas
        TipoFiltro.PENDENTES -> tarefas.filter { !it.concluida }
        TipoFiltro.CONCLUIDAS -> tarefas.filter { it.concluida }
    }
}
```

**5. ViewModel**

```
@HiltViewModel
class TarefaViewModel @Inject constructor(
    private val repository: TarefaRepository
) : ViewModel() {
    
    private val _estado = MutableStateFlow(EstadoTarefa())
    val estado: StateFlow<EstadoTarefa> = _estado
    
    private val _efeitos = MutableSharedFlow<Efeito>()
    val efeitos: SharedFlow<Efeito> = _efeitos
    
    init {
        processarAcao(Acao.CarregarTarefas)
    }
    
    fun processarAcao(acao: Acao) {
        when (acao) {
            is Acao.CarregarTarefas -> carregarTarefas()
            is Acao.SelecionarTarefa -> selecionarTarefa(acao.tarefa)
            is Acao.ConcluirTarefa -> concluirTarefa(acao.id)
            is Acao.ExcluirTarefa -> excluirTarefa(acao.id)
            is Acao.Filtrar -> filtrar(acao.tipo)
            is Acao.LimparErro -> reduzir(Acao.LimparErro)
        }
    }
    
    private fun carregarTarefas() {
        viewModelScope.launch {
            reduciendo(Acao.CarregarTarefas)
            try {
                repository.buscarTarefas().collect { tarefas ->
                    reduzindo(Acao.Sucesso(tarefas))
                }
            } catch (e: Exception) {
                reduzindo(Acao.Erro(e.message ?: "Erro"))
            }
        }
    }
    
    private fun concluirTarefa(id: String) {
        viewModelScope.launch {
            try {
                repository.concluirTarefa(id)
                _efeitos.emit(Efeito.MostrarSnackbar("Tarefa concluída!"))
            } catch (e: Exception) {
                reduciendo(Acao.Erro(e.message ?: "Erro"))
            }
        }
    }
    
    private fun excluirTarefa(id: String) {
        viewModelScope.launch {
            try {
                repository.excluirTarefa(id)
                _efeitos.emit(Efeito.MostrarSnackbar("Tarefa excluída"))
            } catch (e: Exception) {
                reduciendo(Acao.Erro(e.message ?: "Erro"))
            }
        }
    }
    
    private fun filtrar(tipo: TipoFiltro) {
        reduciendo(Acao.Filtrar(tipo))
    }
    
    private fun selecionarTarefa(tarefa: Tarefa) {
        reduciendo(Acao.SelecionarTarefa(tarefa))
    }
    
    private fun reduciendo(acao: Acao) {
        _estado.value = reducer(_estado.value, acao)
    }
}
```

**6. Screen**

```
@Composable
fun TelaTarefas(
    viewModel: TarefaViewModel = hiltViewModel()
) {
    val estado by viewModel.estado.collectAsState()
    val snackbarHostState = remember { SnackbarHostState() }
    
    LaunchedEffect(Unit) {
        viewModel.efeitos.collect { efeito ->
            when (efeito) {
                is Efeito.MostrarSnackbar -> snackbarHostState.showSnackbar(efeito.mensagem)
                is Efeito.NavegarPara -> navController.navigate(efeito.tela)
            }
        }
    }
    
    Scaffold(
        topBar = { TopAppBar(title = { Text("Tarefas") }) },
        floatingActionButton = {
            FloatingActionButton(onClick = { /* navegar para adicionar */ }) {
                Icon(Icons.Default.Add, contentDescription = "Adicionar")
            }
        },
        snackbarHost = { SnackbarHost(snackbarHostState) }
    ) { padding ->
        when {
            estado.isLoading -> Box(modifier = Modifier.fillMaxSize().padding(padding), contentAlignment = Alignment.Center) {
                CircularProgressIndicator()
            }
            estado.erro != null -> Column(modifier = Modifier.fillMaxSize().padding(padding)) {
                Text(estado.erro!!, color = Color.Red)
                Button(onClick = { viewModel.processarAcao(Acao.CarregarTarefas) }) {
                    Text("Tentar novamente")
                }
            }
            else -> LazyColumn(modifier = Modifier.fillMaxSize().padding(padding)) {
                items(estado.tarefas) { tarefa ->
                    TarefaItem(
                        tarefa = tarefa,
                        onConcluir = { viewModel.processarAcao(Acao.ConcluirTarefa(tarefa.id)) },
                        onExcluir = { viewModel.processarAcao(Acao.ExcluirTarefa(tarefa.id)) }
                    )
                }
            }
        }
    }
}
```

---

## 3.4 Clean Architecture - Explicação Completa

### O que é Clean Architecture?

Clean Architecture é organizar o código em **camadas** com responsabilidades separadas:

[DIAGRAMA]
```
┌─────────────────────────────────────────┐
│       CLEAN ARCHITECTURE                 │
├─────────────────────────────────────────┤
│                                          │
│  ┌─────────────────────────────────┐  │
│  │      PRESENTATION                │  │
│  │  Compose UI | ViewModel | Nav    │  │
│  │                                 │  │
│  │  Depende de: Domain             │  │
│  └─────────────┬───────────────────┘  │
│                │                      │
│                ↓ usa                  │
│  ┌─────────────────────────────────┐  │
│  │         DOMAIN                   │  │
│  │  Use Cases | Entities | Interf.  │  │
│  │                                 │  │
│  │  NÃO DEPENDE DE NADA!           │  │
│  └─────────────┬───────────────────┘  │
│                │                      │
│                ↓ implementa          │
│  ┌─────────────────────────────────┐  │
│  │          DATA                    │  │
│  │  Repos. Impl | API(Retrofit) |  │  │
│  │  Room Database                  │  │
│  │                                 │  │
│  │  Implementa interfaces de Domain│  │
│  └─────────────────────────────────┘  │
│                                          │
│  REGRA: Dependências apontam para dentro!│
└─────────────────────────────────────────┘
```

### Por que usar?

1. **Código testável** - Use Cases sem dependências
2. **Código reutilizável** - interfaces definidas
3. **Manutenção** - camadas isoladas
4. **Flexibilidade** - trocar implementação fácil

### Estrutura

```
app/src/main/java/com/projeto/
├── data/
│   ├── remote/
│   │   ├── api/
│   │   └── dto/
│   ├── local/
│   │   └── dao/
│   └── repository/
├── domain/
│   ├── model/
│   ├── repository/  (interfaces)
│   └── usecase/
├── presentation/
│   ├── components/
│   ├── screens/
│   ├── navigation/
│   └── theme/
└── di/
```

### Exemplo Use Case

```
class BuscarTarefasUseCase(
    private val repository: TarefaRepository
) {
    operator fun invoke(): Flow<List<Tarefa>> {
        return repository.buscarTarefas()
    }
}
```

### Exemplo Repository Interface (Domain)

```
interface TarefaRepository {
    fun buscarTarefas(): Flow<List<Tarefa>>
    suspend fun adicionarTarefa(tarefa: Tarefa)
    suspend fun concluirTarefa(id: String)
    suspend fun excluirTarefa(id: String)
}
```

### Exemplo Repository Implementation (Data)

```
class TarefaRepositoryImpl(
    private val api: TarefaApi,
    private val dao: TarefaDao
) : TarefaRepository {
    
    override fun buscarTarefas(): Flow<List<Tarefa>> {
        return dao.buscarTodos().map { lista ->
            lista.map { it.toDomain() }
        }
    }
    
    override suspend fun adicionarTarefa(tarefa: Tarefa) {
        dao.inserir(tarefa.toEntity())
    }
    
    // ...implementação completa
}
```

---

# CAPÍTULO 4: INJEÇÃO DE DEPENDÊNCIA

## 4.1 O que é DI e para que serve?

### O problema sem DI

```
// ❌ PROBLEMA: dependências hardcoded
class MainViewModel : ViewModel() {
    private val repository = UsuarioRepository()  // não consegue testar!
    private val api = RetrofitApi()           // não consegue trocar!
}
```

### A solução com DI

[DIAGRAMA]
```
┌─────────────────────────────────────────┐
│      DEPENDENCY INJECTION               │
├─────────────────────────────────────────┤
│                                          │
│  Sem DI:                    Com DI:       │
│  ┌──────────┐            ┌──────────┐    │
│  │ViewModel │            │ViewModel │    │
│  │ cria   │              │recebe   │    │
│  │  └─────┼─┐           │   ┌─────┘│    │
│  └───────▼┘            └────▼─────┘    │
│       API                 API (injetada)  │
│                                          │
│  Problema:               Solução:        │
│  ❌ Difícil testar      ✅ Fácil testar │
│  ❌ Acoplado            ✅ Desacoplado  │
│  ❌ Inflexível         ✅ Flexível      │
└─────────────────────────────────────────┘
```

### Benefícios do DI

1. **Testabilidade** - Use mocks em testes
2. **Desacoplamento** - classes independentes
3. **Reutilização** - trocar implementações
4. **Manutenção** - Mudar um lugar só

## 4.2 Hilt - Guia Completo

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

### Anotações

| Anotação | Para que serve | Onde usar |
|----------|--------------|----------|
| @HiltAndroidApp | Inicializa Hilt | Application |
| @AndroidEntryPoint | Permite injeção | Activity |
| @HiltViewModel | Injeta no ViewModel | ViewModel |
| @Module | Define configurações | Classe |
| @Provides | Fornece instância | Método |
| @Inject | Injeta dependência | Construtor |

### Exemplo Application

```
@HiltAndroidApp
class App : Application()
```

### Exemplo Activity

```
@AndroidEntryPoint
class MainActivity : ComponentActivity() {
    @Inject lateinit var repository: UsuarioRepository
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        // repository está disponível!
    }
}
```

### Exemplo ViewModel

```
@HiltViewModel
class UsuarioViewModel @Inject constructor(
    private val repository: UsuarioRepository  // injetado automaticamente!
) : ViewModel() {
    // código
}
```

### Exemplo Module

```
@Module
@InstallIn(SingletonComponent::class)
object AppModule {
    
    @Provides
    @Singleton
    fun provideRetrofit(): Retrofit {
        return Retrofit.Builder()
            .baseUrl("https://api.com/")
            .addConverterFactory(GsonConverterFactory.create())
            .build()
    }
    
    @Provides
    @Singleton
    fun provideApi(retrofit: Retrofit): UsuarioApi {
        return retrofit.create(UsuarioApi::class.java)
    }
    
    @Provides
    @Singleton
    fun provideRepository(api: UsuarioApi): UsuarioRepository {
        return UsuarioRepositoryImpl(api)
    }
}
```

## 4.3 Koin - Guia Completo

### Quando usar Koin?

- Projeto pequeño/médio
- Simplicidade preferida
- Curva de aprendizado menor

### Quando usar Hilt?

- Projeto grande
- Performance maxima necessária
-time grande

### Configuração Koin

```
// build.gradle
dependencies {
    implementation 'io.insert-koin:koin-android:3.5.0'
}
```

### Módulos Koin

```
val appModule = module {
    single { provideContext(androidContext()) }
}

val networkModule = module {
    single {
        Retrofit.Builder()
            .baseUrl("https://api.com/")
            .build()
    }
    
    single { get<Retrofit>().create(UsuarioApi::class.java) }
}

val repositoryModule = module {
    single<UsuarioRepository> { UsuarioRepositoryImpl(get()) }
}

val viewModelModule = viewModel {
    UsuarioViewModel(get())
}

val allModules = listOf(
    appModule,
    networkModule,
    repositoryModule,
    viewModelModule
)
```

### Inicialização

```
class App : Application() {
    override fun onCreate() {
        super.onCreate()
        koinApplication {
            modules(allModules)
        }
    }
}
```

### single vs factory

| Tipo | Quando usar |
|------|------------|
| single | Mesma instância em todo lugar (API, Repository) |
| factory | Nova instância a cada uso (ViewModel, UseCase) |

---

# CAPÍTULO 5: ROOM DATABASE

## 5.1 O que é Room?

Room é a biblioteca oficial do Google para persistência SQLite.

[DIAGRAMA]
```
┌─────────────────────────────────────────┐
│             ROOM                        │
├─────────────────────────────────────────┤
│                                          │
│ Room = Abstração sobre SQLite            │
│                                          │
│ SQLite Puro:                            │
│ - Muita linha de código                 │
│ - Erros só em runtime                   │
│ - Queries como strings                 │
│                                          │
│ Room:                                   │
│ - Poucas linhas                         │
│ - Erros em compile time                │
│ - Queries tipe-safe                    │
│ - Suporte a Flow                       │
└─────────────────────────────────────────┘
```

### Para que serve?

- Armazenar dados localmente no dispositivo
- Criar app que funciona offline
- Cache de dados da API
- Dados que não precisam estar em servidor

### Quando NÃO usar Room?

- Dados precisam ser compartilhados (use Firebase)
- Dados muito grandes (use backend)
- Dados simples (use DataStore/SharedPreferences)

## 5.2 Configuração

```
// build.gradle
plugins {
    id 'com.google.devtools.ksp'
}

dependencies {
    implementation 'androidx.room:room-runtime:2.6.1'
    implementation 'androidx.room:room-ktx:2.6.1'
    ksp 'androidx.room:room-compiler:2.6.1'
}
```

## 5.3 Entity - Tabela

```
@Entity(tableName = "tarefas")
data class TarefaEntity(
    @PrimaryKey
    val id: String,
    
    @ColumnInfo(name = "titulo")
    val titulo: String,
    
    @ColumnInfo(name = "descricao")
    val descricao: String,
    
    @ColumnInfo(name = "concluida")
    val concluida: Boolean = false,
    
    @ColumnInfo(name = "data_criacao")
    val dataCriacao: Long = System.currentTimeMillis()
)
```

## 5.4 DAO - Operações

```
@Dao
interface TarefaDao {
    
    // SELECT
    @Query("SELECT * FROM tarefas ORDER BY data_criacao DESC")
    fun buscarTodos(): Flow<List<TarefaEntity>>
    
    @Query("SELECT * FROM tarefas WHERE id = :id")
    suspend fun buscarPorId(id: String): TarefaEntity?
    
    // INSERT
    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun inserir(tarefa: TarefaEntity)
    
    // UPDATE
    @Query("UPDATE tarefas SET concluida = 1 WHERE id = :id")
    suspend fun concluir(id: String)
    
    @Update
    suspend fun atualizar(tarefa: TarefaEntity)
    
    // DELETE
    @Delete
    suspend fun excluir(tarefa: TarefaEntity)
    
    @Query("DELETE FROM tarefas WHERE id = :id")
    suspend fun excluirPorId(id: String)
    
    // Contagem
    @Query("SELECT COUNT(*) FROM tarefas")
    suspend fun contar(): Int
}
```

## 5.5 Database

```
@Database(
    entities = [TarefaEntity::class],
    version = 1,
    exportSchema = true
)
abstract class AppDatabase : RoomDatabase() {
    abstract fun tarefaDao(): TarefaDao
    
    companion object {
        @Volatile
        private var INSTANCE: AppDatabase? = null
        
        fun getInstance(context: Context): AppDatabase {
            return INSTANCE ?: synchronized(this) {
                val instance = Room.databaseBuilder(
                    context.applicationContext,
                    AppDatabase::class.java,
                    "app_database"
                )
                .fallbackToDestructiveMigration()
                .build()
                INSTANCE = instance
                instance
            }
        }
    }
}
```

## 5.6 Repository com Room

```
class TarefaRepositoryImpl(
    private val dao: TarefaDao
) : TarefaRepository {
    
    override fun buscarTarefas(): Flow<List<Tarefa>> {
        return dao.buscarTodos().map { lista ->
            lista.map { it.toDomain() }
        }
    }
    
    override suspend fun adicionarTarefa(tarefa: Tarefa) {
        dao.inserir(tarefa.toEntity())
    }
    
    override suspend fun concluirTarefa(id: String) {
        dao.concluir(id)
    }
    
    override suspend fun excluirTarefa(id: String) {
        dao.excluirPorId(id)
    }
    
    private fun TarefaEntity.toDomain() = Tarefa(id, titulo, descricao, concluida, dataCriacao)
    private fun Tarefa.toEntity() = TarefaEntity(id, titulo, descricao, concluida, dataCriacao)
}
```

## 5.7 Room + Flow = Automagia

[DIAGRAMA]
```
┌─────────────────────────────────────────┐
│       ROOM + FLOW = MAGIC                │
├─────────────────────────────────────────┤
│                                          │
│  Dao retorna Flow              │
│       ↓                              │
│  Room detecta mudança no banco          │
│       ↓                              │
│  Flow emite nova lista           │
│       ↓                              │
│  ViewModel coleta Flow          │
│       ↓                              │
│  Estado atualiza               │
│       ↓                              │
│  UI recompõe automaticamente! 🎉         │
│                                          │
│  NÃO PRECISA:                          │
│  - Observer manual                    │
│  - notifyDataSetChanged              │
│  - LiveData.setValue manual            │
└─────────────────────────────────────────┘
```

---

# CAPÍTULO 6: RETROFIT NETWORKING

## 6.1 O que é Retrofit?

Retrofit é um cliente HTTP que transforma sua API em interface Kotlin.

[DIAGRAMA]
```
┌─────────────────────────────────────────┐
│            RETROFIT                      │
├─────────────────────────────────────────┤
│                                          │
│ 接口API:                                  │
│ interface UsuarioApi {                    │
│     @GET("usuarios")                    │
│     suspend fun listar(): List<UsuarioDto   │
│                                          │
│     @POST("usuarios")                  │
│     suspend fun criar(@Body u: UsuarioDto)   │
│ }                                     │
│       ↓                                │
│ Retrofit gera implementação            │
│       ↓                                │
│ HTTP requests reais!                 │
│                                          ��
��─────────────────────────────────────────┘
```

### Para que serve?

- Comunicar com APIs REST
- Baixar/submeter dados
- Autenticação

## 6.2 Configuração

```
dependencies {
    implementation 'com.squareup.retrofit2:retrofit:2.9.0'
    implementation 'com.squareup.retrofit2:converter-gson:2.9.0'
    implementation 'com.squareup.okhttp3:logging-interceptor:4.12.0'
}
```

## 6.3 Interface API

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

## 6.4 DTO - Data Transfer Object

DTO é o formato que vem da API:

```
data class UsuarioDto(
    @SerializedName("id")
    val id: String?,
    
    @SerializedName("nome")
    val nome: String,
    
    @SerializedName("email")
    val email: String,
    
    @SerializedName("telefone")
    val telefone: String?
)
```

## 6.5 Configuração e Interceptors

```
val retrofit = Retrofit.Builder()
    .baseUrl("https://api.com/")
    .client(OkHttpClient.Builder()
        .addInterceptor(HttpLoggingInterceptor().apply {
            level = HttpLoggingInterceptor.Level.BODY
        })
        .addInterceptor { chain ->
            val request = chain.request().newBuilder()
                .addHeader("Authorization", "Bearer $token")
                .build()
            chain.proceed(request)
        }
        .connectTimeout(30, TimeUnit.SECONDS)
        .readTimeout(30, TimeUnit.SECONDS)
        .build())
    .addConverterFactory(GsonConverterFactory.create())
    .build()

val api = retrofit.create(UsuarioApi::class.java)
```

### Interceptors - Para que servem?

| Interceptor | Para que serve | Exemplo |
|-----------|-------------|--------|
| Auth | Adicionar token em todas requisições | `Authorization: Bearer token123` |
| Logging | Debugar requisições/respostas | Log no logcat |
| Error | Tratar erros HTTP | Throw custom exceptions |

---

# CAPÍTULO 7: COROUTINES E FLOW

## 7.1 O que são Coroutines?

Coroutines permitem código **assíncrono** (não bloqueante).

[DIAGRAMA]
```
┌─────────────────────────────────────────┐
│          COROUTINES                      │
├─────────────────────────────────────────┤
│                                          │
│ THREAD (blocking):                      │
│                                         │
│  ┌─────────┐                           │
│  │ Thread  │                           │
│  ├─────────┤                           │
│  │ código │ ── execute──────→ 5s   │
│  ├─────────┤                           │
│  │ espera │ ─────────────────────→    │
│  └─────────┘                           │
│                                          │
│ COROUTINE (não-blocking):              │
│                                          │
│  ┌─────────┐                           │
│  │ Thread  │                           │
│  ├��────────┤                           │
│  │ corout  │ ── suspende ── 5s ──    │
│  │        │                           │
│  └─────────┘                           │
│       ↓                              │
│  Thread disponível para outras tarefas!  │
└─────────────────────────────────────────┘
```

### launch vs async

| Função | Retorna | Quando usar |
|--------|--------|------------|
| launch | Job (não retorna valor) | fire-and-forget |
| async | Deferred<T> (retorna valor) | precisa do resultado |

```
// launch - não precisa do resultado
viewModelScope.launch {
    repository.buscarDados()
}

// async - precisa do resultado
viewModelScope.launch {
    val deferred = async { api.buscarUsuario() }
    val usuario = deferred.await()
}
```

### Dispatchers

| Dispatcher | Para que serve |
|------------|-------------|
| Main | UI thread |
| IO | Rede/disco (，推荐 para API) |
| Default | CPU intensivo |

```
viewModelScope.launch(Dispatchers.IO) {
    val dados = api.buscar()
    withContext(Dispatchers.Main) {
        estado.value = dados
    }
}
```

## 7.2 Flow - Explicação Completa

### O que é Flow?

Flow é uma **sequência de dados assíncronos**.

[DIAGRAMA]
```
┌─────────────────────────────────────────┐
│              FLOW                        │
├─────────────────────────────────────────┤
│                                          │
│ Flow emite valores ao longo do tempo:         │
│                                          │
│  ┌────┐  ┌────┐  ┌────┐         │
│  │ v1 │→ │ v2 │→ │ v3 │→ ...    │
│  └────┘  └────┘  └────┘         │
│                                          │
│ Útil para:                            │
│ - Dados em tempo real                 │
│ - Novas mensagens                 │
│ - Estado reativo                  │
└─────────────────────────────────────────┘
```

### Operadores Flow

| Operador | Para que serve | Exemplo |
|---------|-------------|--------|
| map | Transformar valores | `flow.map { it * 2 }` |
| filter | Filtrar valores | `flow.filter { it > 2 }` |
| take | Limitar quantidade | `flow.take(10)` |

### StateFlow

Mantém estado atual:

```
private val _estado = MutableStateFlow(Estado())
val estado: StateFlow<Estado> = _estado

_estado.value = Estado()  // atualizar

val estado by viewModel.estado.collectAsState()  // coletar no Compose
```

### SharedFlow

Para eventos únicos:

```
private val _efeitos = MutableSharedFlow<Efeito>()
val efeitos: SharedFlow<Efeito> = _efeitos

_efeitos.emit(Efeito.NavegarPara("tela"))  // emitir

LaunchedEffect(Unit) {
    viewModel.efeitos.collect { efeito ->
        // processar
    }
}
```

---

# CAPÍTULO 8: FIREBASE

## 8.1 Firebase Auth

### Para que serve?

Autenticação de usuários (email, Google, Facebook, etc).

```
val auth = Firebase.auth

// Criar conta
auth.createUserWithEmailAndPassword(email, senha)
    .addOnSuccessListener { result -> }
    .addOnFailureListener { erro -> }

// Login
auth.signInWithEmailAndPassword(email, senha)

// Logout
auth.signOut()
```

## 8.2 Firestore

### Para que serve?

Banco de dados NoSQL em tempo real.

```
val db = Firebase.firestore

// CREATE
db.collection("usuarios").document("id").set(mapOf("nome" to "João"))

// READ
db.collection("usuarios").document("id").get()

// READ realtime
db.collection("usuarios").addSnapshotListener()

// UPDATE
db.collection("usuarios").document("id").update("nome", "Maria")

// DELETE
db.collection("usuarios").document("id").delete()
```

## 8.3 Analytics

### Para que serve?

Registrar eventos de uso.

```
val analytics = Firebase.analytics

analytics.logEvent("botao_salvar") {
    param("tela", "cadastro")
}
```

## 8.4 Crashlytics

### Para que serve?

Capturar erros em produção.

Erros são capturados automaticamente!

```
// Log customizado
Firebase.crashlytics.log("mensagem")

// Identificar usuário
Firebase.crashlytics.setUserId("user123")
```

---

# CAPÍTULO 9: TESTES UNITÁRIOS

## 9.1 Por que testar?

[DIAGRAMA]
```
┌─────────────────────────────────────────┐
│           TESTES                        │
├─────────────────────────────────────────┤
│                                          │
│ SEM TESTES:            COM TESTES:          │
│                                          │
│ Deploy ──┐        Teste ────→ Deploy   │
│  💥    │          ✅                 │
│  Crash! │                          │
│                                          │
│ Problema:                  Solução:         │
│ ❌ Bugs em prod         ✅ Bugs pegos   │
│ ❌ Medo de mudar       ✅ Confiança   │
│ ❌ Refatorar perigoso  ✅ Refatorar  │
│                        seguro          │
│                                          │
│ O que teste faz:                       │
│ ✅ Garante funcionamento           │
│ ✅ Documenta código              │
│ ✅ Evita regressões             │
│ ✅ Confidence para mudar        │
└─────────────────────────────────────────┘
```

## 9.2 JUnit - Guia Completo

### O que é JUnit?

JUnit é framework de testes para Kotlin/Java.

### Anatomia de um teste

```
┌─────────────────────────────────────────┐
│      ANATOMIA DE TESTE                  │
├─────────────────────────��───────────────┤
│                                          │
│  @Test                      // 1. Marcador│
│  fun teste_something() {    // 2. Nome   │
│                                          │
│      // 3. Arrange - Preparar        │
│      val calc = Calculadora()          │
│                                          │
│      // 4. Act - Executar      │
│      val result = calc.somar(2, 3)   │
│                                          │
│      // 5. Assert - Verificar    │
│      assertEquals(5, result)        │
│  }                                  │
│                                          │
│  Para entender:                     │
│  1. Prepare dados                 │
│  2. Execute ação                │
│  3. Verifique resultado        │
└─────────────────────────────────────────┘
```

### Configuração

```
// build.gradle
testImplementation 'junit:junit:4.13.2'
```

### Exemplo completo testável

```
// CLASSE PARA TESTAR
class Calculadora {
    fun somar(a: Int, b: Int) = a + b
    fun subtrair(a: Int, b: Int) = a - b
    fun multiplicar(a: Int, b: Int) = a * b
    
    fun dividir(a: Int, b: Int): Int {
        require(b != 0) { "Divisor não pode ser zero" }
        return a / b
    }
    
    fun ehPar(numero: Int) = numero % 2 == 0
}

// TESTES
class CalculadoraTest {
    lateinit var calculadora: Calculadora
    
    @Before
    fun setup() {
        calculadora = Calculadora()
    }
    
    // Teste de soma
    @Test
    fun teste_somar_dois_positivos() {
        // Arrange
        val a = 2
        val b = 3
        
        // Act
        val resultado = calculadora.somar(a, b)
        
        // Assert
        assertEquals(5, resultado)
    }
    
    // Teste de soma com negativos
    @Test
    fun teste_somar_negativos() {
        val resultado = calculadora.somar(-5, -3)
        assertEquals(-8, resultado)
    }
    
    // Teste de divisão
    @Test
    fun teste_dividir() {
        val resultado = calculadora.dividir(10, 2)
        assertEquals(5, resultado)
    }
    
    // Teste de exceção
    @Test(expected = IllegalArgumentException::class)
    fun teste_dividir_por_zero() {
        calculadora.dividir(10, 0)
    }
    
    // Teste booleano
    @Test
    fun teste_eh_par() {
        assertTrue(calculadora.ehPar(2))
        assertTrue(calculadora.ehPar(0))
        assertFalse(calculadora.ehPar(3))
    }
}
```

### Assertions mais usadas

```
assertEquals(esperado, real)
assertTrue(condicao)
assertFalse(condicao)
assertNull(obj)
assertNotNull(obj)
assertSame(obj1, obj2)  // mesmo objeto
assertArrayEquals(arr1, arr2)
```

## 9.3 MockK - Guia Completo

### O que é MockK?

MockK é biblioteca para criar **mocks** (objetos falsos) em Kotlin.

[DIAGRAMA]
```
┌─────────────────────────────────────────┐
│             MOCKK                       │
├─────────────────────────────────────────┤
│                                          │
│ O que é um Mock?                      │
│                                          │
│ Um objeto falso que simula o         │
│ comportamento real:                   │
│                                          │
│  ┌──────────────────┐                │
│  │ Repository    │ real         │
│  │ (API, DB)   │ ← Cara!     │
│  └─────────────┘                │
│         ↓                    │
│  ┌���─���───────────────┐                │
│  │    MOCK       │ falso        │
│  │ (controlado)  │ ← Barato!   │
│  └──────────────────┘                │
│                                          │
│ Por que usar?                        │
│ ✅ Testar sem dependências reais  │
│ ✅ Controlar comportamento        │
│ ✅ Verificar interações   │
└─────────────────────────────────────────┘
```

### Configuração

```
testImplementation 'io.mockk:mockk:1.13.8'
testImplementation 'io.mockk:mockk-android:1.13.8'
```

### Exemplos MockK

#### Criar mock

```
val repository = mockk<UsuarioRepository>()
```

#### Definir comportamento

```
// Retornar valor
every { repository.buscarUsuario("1") } returns Usuario("1", "João")

// Retornar null
every { repository.buscarUsuario("999") } returns null

// Throw erro
every { repository.buscarUsuario("1") } throws IOException("Erro!")
```

#### Verificar chamado

```
verify { repository.buscarUsuario("1") }
verify(exactly = 1) { repository.buscarUsuario("1") }
verify(none = 1) { repository.excluirUsuario(any()) }
```

#### Mock com Flow

```
val lista = listOf(Usuario("1", "João"))
every { repository.buscarTodos() } returns flowOf(lista)
```

### Testando ViewModel com MockK

```
// ViewModel
@HiltViewModel
class UsuarioViewModel @Inject constructor(
    private val repository: UsuarioRepository
) : ViewModel() {
    fun carregarUsuario(id: String) {
        viewModelScope.launch {
            val usuario = repository.buscarUsuario(id)
            _estado.value = _estado.value.copy(usuario = usuario)
        }
    }
}

// Teste
@ExtendWith(StandardTestExtension::class)
class UsuarioViewModelTest {
    private lateinit var viewModel: UsuarioViewModel
    private lateinit var repository: UsuarioRepository
    
    @Before
    fun setup() {
        repository = mockk(relaxed = true)
        viewModel = UsuarioViewModel(repository)
    }
    
    @Test
    fun teste_carregar_usuario() {
        // Arrange
        val usuarioMock = Usuario("1", "João", "joao@email.com")
        every { runBlocking { repository.buscarUsuario("1") } returns usuarioMock }
        
        // Act
        viewModel.carregarUsuario("1")
        
        // Assert
        assertEquals(usuarioMock, viewModel.estado.value.usuario)
    }
}
```

## 9.4 Turbine - Guia Completo

### O que é Turbine?

Turbine é biblioteca para testar **Flow**.

[DIAGRAMA]
```
┌─────────────────────────────────────────┐
│            TURBINE                      │
├─────────────────────────────────────────┤
│                                          │
│ Problema: Flow emite Muitos valores          │
│ Como testar?                           │
│                                          │
│ SEM TURBINE:                        │
│ flow.collect { valor ->              │
│    assertEquals(esperado, valor)      │
│ } // Só funciona para 1 valor!        │
│                                          │
│ COM TURBINE:                     │
│ flow.test {                       │
│    val v1 = awaitItem()           │
│    val v2 = awaitItem()           │
│    awaitComplete()               │
│ } // Funciona para Todos!         │
└─────────────────────────────────────────┘
```

### Configuração

```
testImplementation 'app.cash.turbine:turbine:1.0.0'
```

### Exemplos

#### Testar Flow simples

```
@Test
fun teste_flow_simples() = runTest {
    val flow = flowOf(1, 2, 3)
    
    flow.test {
        assertEquals(1, awaitItem())
        assertEquals(2, awaitItem())
        assertEquals(3, awaitItem())
        awaitComplete()
    }
}
```

#### Testar erro

```
@Test
fun teste_erro() = runTest {
    val flow = flow {
        emit(1)
        throw IOException("Erro!")
    }
    
    flow.test {
        assertEquals(1, awaitItem())
        awaitError()  // Espera erro
    }
}
```

#### Testar ViewModel

```
@Test
fun teste_viewmodel() = runTest {
    val tarefas = listOf(Tarefa("1", "Tarefa 1"))
    val repository = mockk<UsuarioRepository>()
    every { repository.buscarTarefas() } returns flowOf(tarefas)
    
    val viewModel = TarefaViewModel(repository)
    
    viewModel.tarefas.test {
        val resultado = awaitItem()
        assertEquals(1, resultado.size)
    }
}
```

## 9.5 Testes de UI Compose

### O que são?

Testes que interagem diretamente com Composables.

### Configuração

```
androidTestImplementation 'androidx.compose.ui:ui-test-junit4'
```

### Estrutura

```
@get:Rule
val composeTestRule = createComposeRule()

@Test
fun teste_botao() {
    // Arrange - criar UI
    setContent {
        Button(onClick = { }) {
            Text("Clique")
        }
    }
    
    // Act - clicar
    onNodeWithText("Clique").performClick()
    
    // Assert - verificar
    onNodeWithText("Clique").assertHasClickAction()
}
```

### Busca de nodes

| Função | Para que serve |
|--------|-------------|
| onNodeWithText("texto") | Buscar por texto |
| onNodeWithTag("tag") | Buscar por tag |
| onNode(hasClickAction()) | Buscar por característica |

### Ações

```
.performClick()
.performTextInput("texto")
.performTextClearance()
```

### Assertions

```
.assertExists()
.assertDoesNotExist()
.assertHasClickAction()
.assertIsEnabled()
.assertIsDisabled()
```

### Exemplo completo

```
@Composable
fun Formulario(onSalvar: (String) -> Unit) {
    var nome by remember { mutableStateOf("") }
    
    Column {
        OutlinedTextField(
            value = nome,
            onValueChange = { nome = it },
            modifier = Modifier.testTag("campo_nome")
        )
        
        Button(
            onClick = { onSalvar(nome) },
            enabled = nome.isNotBlank(),
            modifier = Modifier.testTag("botao_salvar")
        ) {
            Text("Salvar")
        }
    }
}

@Test
fun teste_formulario() {
    var nomeSalvo: String? = null
    
    setContent {
        Formulario(onSalvar = { nomeSalvo = it })
    }
    
    // Digitar
    onNodeWithTag("campo_nome").performTextInput("João")
    
    // Clicar
    onNodeWithTag("botao_salvar").performClick()
    
    // Verificar
    assertEquals("João", nomeSalvo)
}
```

---

# CAPÍTULO 10: MOCKITO (vs MockK)

## 10.1 O que é Mockito?

Mockito é uma biblioteca de mocking (criar objetos falsos) muito popular, mas feita para Java.

### Comparação MockK vs Mockito

| Característica | MockK | Mockito |
|---------------|------|---------|
| Feito para | Kotlin | Java |
| Sintaxe Kotlin | ✅ Nativa | ❌ Adaptada |
| Suporte lambdas | ✅ Sim | ❌ Não |
| Suporte coroutines | ✅ Sim | ❌ Não |
| Extension functions | ✅ Sim | ❌ Não |

### Por que preferimos MockK?

```
// MockK - sintaxe Kotlin natural
val repository = mockk<UsuarioRepository>()
every { repository.buscarUsuario("1") } returns usuario

// Mockito - código "estranho" em Kotlin
val repository = mock(UsuarioRepository::class.java)
`when`(repository.buscarUsuario("1")).thenReturn(usuario)
```

### Configuração Mockito

```
// build.gradle
testImplementation 'junit:junit:4.13.2'
testImplementation 'org.mockito:mockito-core:5.8.0'
AndroidTestImplementation 'org.mockito:mockito-android:5.8.0'
```

### Exemplo com Mockito

```
// Criar mock
val repository = mock(UsuarioRepository::class.java)

// Definir comportamento
`when`(repository.buscarUsuario("1")).thenReturn(usuario)

// Usar em teste
val viewModel = UsuarioViewModel(repository)
viewModel.carregarUsuario("1")

// Verificar
verify(repository).buscarUsuario("1")
```

### Quando usar MockK vs Mockito?

| Situação | Recomendação |
|---------|-----------|
| Projeto Kotlin puro | **MockK** |
| Projeto Kotlin + Java misturado | MockK ou Mockito |
| Legado Java | Mockito |
| Conhecimento prévio Java | Mockito |

### Regra: Use apenas UM

Não misture MockK e Mockito no mesmo projeto!

```
// NO! Misturado ❌
val mockK = mockk<Repo>()
val mockito = mock(Repo::class.java)

// OK! Um ou outro ✅
val mockK = mockk<Repo>()
// OU
val mockito = mock(Repo::class.java)
```

---

# CAPÍTULO 11: ESPRESSO - Testes de UI

## 11.1 O que é Espresso?

Espresso é um framework de testes de UI para Android que funciona com Views (XML) e também Compose.

[DIAGRAMA]
```
┌─────────────────────────────────────────┐
│            ESPRESSO                    │
├─────────────────────────────────────────┤
│                                          │
│ Para que serve:                       │
│ - Testar interface do usuário          │
│ - Simular cliques e digitações       │
│ - Verificar textos na tela          │
│ - Testar navegação            │
│                                          │
│ Vantagens:                          │
│ ✅ Sintaxe fluente (encadeia)           │
│ ✅ Sincronização automática        │
│ ✅ Amplo suporte               │
│ ✅ Boa documentação          │
│                                          │
│ Para Compose, prefira            │
│ Compose UI Test (mais leve)          │
└─────────────────────────────────────────┘
```

### Configuração

```
// build.gradle
androidTestImplementation 'androidx.test.espresso:espresso-core:3.5.1'
```

### Buscando Views

| Matcher | Uso |
|---------|-----|
| `withId(R.id.botao)` | Por ID |
| `withText("Salvar")` | Por texto |
| `withHint("Nome")` | Por hint |
| `isDisplayed()` | Visível |
| `isEnabled()` | Habilitado |

### Ações

| Ação | Uso |
|------|-----|
| `perform(click())` | Clicar |
| `perform(typeText("texto"))` | Digitar |
| `perform(clearText())` | Limpar |
| `perform(scrollTo())` | Rolar |

### Assertions

| Assertion | Uso |
|----------|-----|
| `check(matches(isDisplayed()))` | Está visível |
| `check(matches(withText("x"))` | Tem texto |
| `check(matches(isEnabled()))` | Está habilitado |

### Exemplo completo

```
// XML Layout
<LinearLayout>
    <TextView android:id="@+id/usuario_nome"/>
    <EditText android:id="@+id/campo_nome"/>
    <Button android:id="@+id/botao_salvar"/>
</LinearLayout>

// Teste Espresso
@Test
fun teste_formulario() {
    // Digitar texto
    onView(withId(R.id.campo_nome))
        .perform(typeText("João"), closeSoftKeyboard())
    
    // Clicar botão
    onView(withId(R.id.botao_salvar))
        .perform(click())
    
    // Verificar resultado
    onView(withText("Salvo!"))
        .check(matches(isDisplayed()))
}

@Test
fun teste_validacao_campo_vazio() {
    // Clicar sem digitar nada
    onView(withId(R.id.botao_salvar))
        .perform(click())
    
    // Verificar mensaje de erro
    onView(withText("Nome é obrigatório"))
        .check(matches(isDisplayed()))
}
```

### Espresso vs Compose UI Test

| Aspecto | Espresso | Compose UI Test |
|--------|---------|---------------|
| Para XML | ✅ | ❌ |
| Para Compose | ✅ (funciona) | ✅ (melhor) |
| Performance | boa | melhor |
| Setup | preciso | fácil |

**Regra:**
- Para XML → **Espresso**
- Para Compose → **Compose UI Test**
- Para ambos → **Espresso**

---

# CAPÍTULO 12: NAVIGATION COMPOSE

## 12.1 O que é?

Navigation Compose é o sistema de navegação do Jetpack Compose.

### Para que serve?

Navegar entre telas no app:

[DIAGRAMA]
```
┌─────────────────────────────────────────┐
│       NAVIGATION COMPOSE                 │
├─────────────────────────────────────────┤
│                                          │
│ Antes ( Activities):                    │
│ Intent → startActivity()              │
│                                      │
│ Agora ( Navigation Compose):            │
│ NavController → navigate()         │
│                                          │
│ Benefícios:                         │
│ ✅ Tiposafe                        │
│ ✅ Back stack automático            │
│ ✅ Animações integradas           │
│ ✅ Deep links fácil               │
└─────────────────────────────────────────┘
```

### Configuração

```
// build.gradle
dependencies {
    implementation 'androidx.navigation:navigation-compose:2.7.6'
}
```

### Definindo Rotas

```
sealed class Rota(val path: String) {
    object Home : Rota("home")
    object Detalhes : Rota("detalhes/{id}") {
        fun criar(id: String) = "detalhes/$id"
    }
    object Login : Rota("login")
    object Cadastro : Rota("cadastro")
}
```

### NavHost (navegação completa)

```
@Composable
fun Navegacao() {
    val navController = rememberNavController()
    
    NavHost(
        navController = navController,
        startDestination = Rota.Home.path
    ) {
        // Tela Home
        composable(Rota.Home.path) {
            HomeScreen(
                onNavigateToDetalhes = { id ->
                    navController.navigate(Rota.Detalhes.criar(id))
                }
            )
        }
        
        // Tela Detalhes com argumento
        composable(
            route = Rota.Detalhes.path,
            arguments = listOf(
                navArgument("id") { type = NavType.StringType }
            )
        ) { backStackEntry ->
            val id = backStackEntry.arguments?.getString("id") ?: ""
            TelaDetalhes(id = id)
        }
        
        // Tela Login
        composable(Rota.Login.path) {
            LoginScreen(
                onLoginSuccess = {
                    navController.navigate(Rota.Home.path) {
                        popUpTo(Rota.Login.path) { inclusive = true }
                    }
                },
                onNavigateToCadastro = {
                    navController.navigate(Rota.Cadastro.path)
                }
            )
        }
    }
}
```

### Navegando com Argumentos

```
// HomeScreen
@Composable
fun HomeScreen(
    onNavigateToDetalhes: (String) -> Unit
) {
    Button(onClick = { onNavigateToDetalhes("123") }) {
        Text("Ver detalhes")
    }
}

// Navigation
composable("detalhes/{id}") { backStackEntry ->
    val id = backStackEntry.arguments?.getString("id") ?: ""
    TelaDetalhes(id = id)
}
```

### Deep Links

```
composable(
    "tela/{id}",
    deepLinks = listOf(
        navDeepLink { uriPattern = "meuapp://tela/$id" },
        navDeepLink { uriPattern = "https://meusite.com/tela/$id" }
    )
)
```

### Passos para implementar:

1. Adicionar dependência
2. Definir rotas (sealed class)
3. Criar NavHost
4. Configurar composable para cadarota
5. Usar rememberNavController()
6. Chamar navigate() nos botões
7. Configurar popUpTo() se necessário

---

# CAPÍTULO 13: ROOM - MAIS DETALHES

## 13.1 Room Relations

Room suporta relações entre entidades:

### One-to-One

```
@Entity(tableName = "enderecos")
data class EnderecoEntity(
    @PrimaryKey val id: String,
    val rua: String,
    val cidade: String,
    val usuarioId: String  // Chave estrangeira
)

data class UsuarioComEndereco(
    @Embedded val usuario: Usuario,
    @Relation(
        parentColumn = "id",
        entityColumn = "usuarioId"
    )
    val endereco: Endereco
)
```

### One-to-Many

```
@Entity(tableName = "autores")
data class AutorEntity(
    @PrimaryKey val id: String,
    val nome: String
)

@Entity(tableName = "livros")
data class LivroEntity(
    @PrimaryKey val id: String,
    val titulo: String,
    val autorId: String
)

data class AutorComLivros(
    @Embedded val autor: Autor,
    @Relation(
        parentColumn = "id",
        entityColumn = "autorId"
    )
    val livros: List<Livro>
)
```

### Buscar com relação

```
@Transaction
@Query("SELECT * FROM autores")
fun buscarAutoresComLivros(): List<AutorComLivros>
```

## 13.2 Type Converters

Para salvar tipos complexos:

```
class Converters {
    
    @TypeConverter
    fun fromTimestamp(value: Long?): Date? {
        return value?.let { Date(it) }
    }
    
    @TypeConverter
    fun dateToTimestamp(date: Date?): Long? {
        return date?.time
    }
    
    @TypeConverter
    fun fromStringList(value: String): List<String> {
        return if (value.isEmpty()) emptyList() else value.split(",")
    }
    
    @TypeConverter
    fun toStringList(list: List<String>): String {
        return list.joinToString(",")
    }
}

@Database(entities = [Usuario::class], version = 1)
@TypeConverters(Converters::class)
abstract class AppDatabase : RoomDatabase()
```

## 13.3 Migrations

Quando muda o schema do banco:

```
@Database(
    entities = [Usuario::class],
    version = 2,
    exportSchema = true
)
abstract class AppDatabase : RoomDatabase() {
    
    companion object {
        val MIGRATION_1_2 = object : Migration(1, 2) {
            override fun migrate(db: SupportSQLiteDatabase) {
                db.execSQL("ALTER TABLE usuarios ADD COLUMN telefone TEXT")
            }
        }
        
        val MIGRATION_2_3 = object : Migration(2, 3) {
            override fun migrate(db: SupportSQLiteDatabase) {
                db.execSQL("CREATE TABLE tarefas_temp AS SELECT * FROM tarefas")
                db.execSQL("DROP TABLE tarefas")
                db.execSQL("ALTER TABLE tarefas_temp RENAME TO tarefas")
            }
        }
    }
}

// Na construção
Room.databaseBuilder(...)
    .addMigrations(AppDatabase.MIGRATION_1_2, AppDatabase.MIGRATION_2_3)
    .build()
```

## 13.4 Room com ViewModel

Integrando Room com ViewModel:

```
@HiltViewModel
class TarefaViewModel @Inject constructor(
    private val repository: TarefaRepository
) : ViewModel() {
    
    // Room + Flow = automagia!
    val tarefas: StateFlow<List<Tarefa>> = repository.buscarTarefas()
        .stateIn(
            viewModelScope,
            SharingStarted.WhileSubscribed(5000),
            emptyList()
        )
    
    fun adicionarTarefa(tarefa: Tarefa) {
        viewModelScope.launch {
            repository.adicionarTarefa(tarefa)
        }
    }
}
```

## 13.5 Room vs SQLite Puro vs DataStore

| Característica | Room | SQLite Puro | DataStore |
|---------------|------|-------------|-----------|
| Linhas de código | Poucas | Muitas | Poucas |
| Type safety | ✅ | ❌ | Parcial |
| Flow | ✅ | ❌ | ✅ |
| Dados complexos | ✅ | ✅ | ✅ (Proto) |
| Performance | Boa | Melhor | Boa |
| Quando usar | App com banco | Casos extremos | Configurações |

---

# CAPÍTULO 10: SOLID E CLEAN CODE

## 10.1 SOLID - Explicação Completa

### S — Single Responsibility

[DIAGRAMA]
```
┌─────────────────────────────────────────┐
│  SINGLE RESPONSIBILITY                  │
├─────────────────────────────────────────┤
│                                          │
│ Uma classe = Uma responsabilidade:        │
│                                          │
│ ❌ ERRADO:                      │
│ class Gerenciador {                │
│     fun salvar() {}  // persistência │
│     fun email() {}   // email      │
│     fun pdf() {}    // relatório│
│ }                              │
│                                          │
│ ✅ CERTO:                        │
│ class Repository { fun salvar() {} }   │
│ class EmailService { fun enviar(){} } │
│ class PdfService { fun gerar(){} }   │
└─────────────────────────────────────────┘
```

### O — Open/Closed

[DIAGRAMA]
```
┌─────────────────────────────────────────┐
│      OPEN/CLOSED                        │
├─────────────────────────────────────────┤
│                                          │
│ Aberto para extensão,               │
│ fechado para modificação:         │
│                                          │
│ ❌ ERRADO:                      │
│ class Pagamento {                 │
│     fun pagar(tipo: String) {    │
│         when (tipo) {          │
│             "cartao" -> ...       │
│             "boleto" -> ...       │
│             "pix" -> ...    ←── MODIFICAR! │
│         }                       │
│     }                          │
│ }                              │
│                                          │
│ ✅ CERTO:                        │
│ abstract class Desconto {         │
│     abstract fun calcular()      │
│ }                              │
│ class Cartao : Desconto() {}      │
│ class Boleto : Desconto() {}      │
│ class Pix : Desconto() {} ←── NOVA CLASSE! │
└─────────────────────────────────────────┘
```

### L — Liskov Substitution

[DIAGRAMA]
```
┌─────────────────────────────────────────┐
│    LISKOV SUBSTITUTION              │
├─────────────────────────────────────────┤
│                                          │
│ Subclasses podem substituir       │
│ a classe base:                 │
│                                          │
│ ✅ CERTO:                        │
│ open class Animal {           │
│     open fun som()            │
│ }                              │
│ class Cao : Animal()         │
│ class Gato : Animal()       │
│                              │
│ fun fazerSom(animal: Animal)  │
│     animal.som()  // Funciona!  │
└─────────────────────────────────────────┘
```

### I — Interface Segregation

[DIAGRAMA]
```
┌─────────────────────────────────────────┐
│   INTERFACE SEGREGATION                │
├─────────────────────────────────────────┤
│                                          │
│ Interfaces pequenas:              │
│                                          │
│ ❌ ERRADO:                      │
│ interface Repo {                 │
│     fun buscar()               │
│     fun salvar()              │
│     fun exportar() ←── não preciso│
│ }                              │
│                                          │
│ ✅ CERTO:                        │
│ interface Leitura { fun buscar() } │
│ interface Escrita { fun salvar() } │
│ interface Export {}              │
│                              ��
│ class TarefaRepo : Leitura, Escrita {}
└─────────────────────────────────────────┘
```

### D — Dependency Inversion

[DIAGRAMA]
```
┌─────────────────────────────────────────┐
│  DEPENDENCY INVERSION               │
├─────────────────────────────────────────┤
│                                          │
│ Depender de abstrações:         │
│                                          │
│ ❌ ERRADO:                      │
│ class Servico {                 │
│     val api = ApiReal()  // dependência concreta│
│ }                              │
│                                          │
│ ✅ CERTO:                        │
│ interface IApi { }               │
│ class ApiReal : IApi {}         │
│ class Servico(val api: IApi) {} // dependência abstrata│
└─────────────────────────────────────────┘
```

---

# CAPÍTULO 11: PROJETO COMPLETO

(Código completo incluído anteriormente)

---

# CONCLUSÃO

Este guia completo inclui:

✅ Explicações detalhadas para cada conceito
✅ Para que serve cada coisa
✅ Exemplos práticos e funcionais
✅ Comparações quando aplicável
✅ Testes unitários com JUnit, MockK, Turbine
✅ Testes de UI com Compose
✅ Room Database completo
✅ Retrofit e Coroutines
✅ Firebase Auth e Firestore
✅ SOLID e Clean Code
✅ Projeto completo real

Você agora tem todo o conhecimento necessário para desenvolver apps Android profissionais prontos para o mercado de trabalho!

Continue praticando e evoluindo! 🚀
