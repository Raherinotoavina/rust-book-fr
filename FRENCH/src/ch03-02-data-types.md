<!--
## Data Types
-->

## Les types de données

<!--
Every value in Rust is of a certain *data type*, which tells Rust what kind of
data is being specified so it knows how to work with that data. We’ll look at
two data type subsets: scalar and compound.
-->

Chaque valeur en Rust est d'un *type* bien déterminé, qui indique à Rust quel
genre de données il manipule pour qu'il sache comment traiter ces données.
Nous allons nous intéresser à deux catégories de types de données : les
scalaires et les composés.

<!--
Keep in mind that Rust is a *statically typed* language, which means that it
must know the types of all variables at compile time. The compiler can usually
infer what type we want to use based on the value and how we use it. In cases
when many types are possible, such as when we converted a `String` to a numeric
type using `parse` in the [“Comparing the Guess to the Secret
Number”][comparing-the-guess-to-the-secret-number]<!-- ignore -- > section in
Chapter 2, we must add a type annotation, like this:
-->

Gardez à l'esprit que Rust est un langage *statiquement typé*, ce qui signifie
qu'il doit connaître les types de toutes les variables au moment de la
compilation. Le compilateur peut souvent déduire quel type utiliser en se basant
sur la valeur et sur la façon dont elle est utilisée. Dans les cas où plusieurs
types sont envisageables, comme lorsque nous avons converti une chaîne de
caractères en un type numérique en utilisant `parse` dans la section
[“Comparer le nombre saisi au nombre
secret”][comparing-the-guess-to-the-secret-number]<!-- ignore -->
du chapitre 2, nous devons ajouter une annotation de type, comme ceci :

<!--
```rust
let guess: u32 = "42".parse().expect("Not a number!");
```
-->

```rust
let supposition: u32 = "42".parse().expect("Ce n'est pas un nombre !");
```

<!--
If we don’t add the type annotation here, Rust will display the following
error, which means the compiler needs more information from us to know which
type we want to use:
-->

Si nous n'ajoutons pas l'annotation de type ici, Rust affichera l'erreur
suivante, signifiant que le compilateur a besoin de plus d'informations pour
déterminer quel type nous souhaitons utiliser :

<!--
```console
{{#include ../listings-sources/ch03-common-programming-concepts/output-only-01-no-type-annotations/output.txt}}
```
-->

```console
{{#include ../listings/ch03-common-programming-concepts/output-only-01-no-type-annotations/output.txt}}
```

<!--
You’ll see different type annotations for other data types.
-->

Vous découvrirez différentes annotations de type au fur et à mesure que nous
aborderons les autres types de données.

<!--
### Scalar Types
-->

### Types scalaires

<!--
A *scalar* type represents a single value. Rust has four primary scalar types:
integers, floating-point numbers, Booleans, and characters. You may recognize
these from other programming languages. Let’s jump into how they work in Rust.
-->

Un type *scalaire* représente une seule valeur. Rust possède quatre types
principaux de scalaires : les entiers, les nombres à virgule flottante, les
booléens et les caractères. Vous les connaissez sûrement d'autres langages de
programmation. Regardons comment ils fonctionnent avec Rust.

<!--
#### Integer Types
-->

#### Types de nombres entiers

<!--
An *integer* is a number without a fractional component. We used one integer
type in Chapter 2, the `u32` type. This type declaration indicates that the
value it’s associated with should be an unsigned integer (signed integer types
start with `i`, instead of `u`) that takes up 32 bits of space. Table 3-1 shows
the built-in integer types in Rust. We can use any of these variants to declare
the type of an integer value.
-->

Un *entier* est un nombre sans partie décimale. Nous avons utilisé un entier
précédemment dans le chapitre 2, le type `u32`. Cette déclaration de type
indique que la valeur à laquelle elle est associée doit être un entier non signé
encodé sur 32 bits dans la mémoire (les entiers pouvant prendre des valeurs
négatives commencent par un `i` (comme *integer* : “entier”), plutôt que par un
`u` comme *unsigned* : “non signé”). Le tableau 3-1 montre les types
d'entiers intégrés au langage. Nous pouvons utiliser chacune de ces variantes
pour déclarer le type d'une valeur entière.

<!--
<span class="caption">Table 3-1: Integer Types in Rust</span>
-->

<span class="caption">Tableau 3-1 : les types d'entiers en Rust</span>

<!--
| Length  | Signed  | Unsigned |
|---------|---------|----------|
| 8-bit   | `i8`    | `u8`     |
| 16-bit  | `i16`   | `u16`    |
| 32-bit  | `i32`   | `u32`    |
| 64-bit  | `i64`   | `u64`    |
| 128-bit | `i128`  | `u128`   |
| arch    | `isize` | `usize`  |
-->

| Taille  | Signé   | Non signé |
|---------|---------|-----------|
| 8 bits  | `i8`    | `u8`      |
| 16 bits | `i16`   | `u16`     |
| 32 bits | `i32`   | `u32`     |
| 64 bits | `i64`   | `u64`     |
| 128 bits| `i128`  | `u128`    |
| archi   | `isize` | `usize`   |

<!--
Each variant can be either signed or unsigned and has an explicit size.
*Signed* and *unsigned* refer to whether it’s possible for the number to be
negative—in other words, whether the number needs to have a sign with it
(signed) or whether it will only ever be positive and can therefore be
represented without a sign (unsigned). It’s like writing numbers on paper: when
the sign matters, a number is shown with a plus sign or a minus sign; however,
when it’s safe to assume the number is positive, it’s shown with no sign.
Signed numbers are stored using [two’s
complement](https://en.wikipedia.org/wiki/Two%27s_complement)<!-- ignore -- >
representation.
-->

Chaque variante peut être signée ou non signée et possède une taille explicite.
*Signé* et *non signé* veut dire respectivement que le nombre peut prendre ou
non des valeurs négatives — en d'autres termes, si l'on peut lui attribuer un
signe (signé) ou s'il sera toujours positif et que l'on peut donc le représenter
sans signe (non signé). C'est comme écrire des nombres sur du papier : quand le
signe est important, le nombre est écrit avec un signe plus ou un signe moins ;
en revanche, quand le nombre est forcément positif, on peut l'écrire sans son
signe. Les nombres signés sont stockés en utilisant le [complément à
deux](https://fr.wikipedia.org/wiki/Compl%C3%A9ment_%C3%A0_deux)<!-- ignore -->.

<!--
Each signed variant can store numbers from -(2<sup>n - 1</sup>) to 2<sup>n -
1</sup> - 1 inclusive, where *n* is the number of bits that variant uses. So an
`i8` can store numbers from -(2<sup>7</sup>) to 2<sup>7</sup> - 1, which equals
-128 to 127. Unsigned variants can store numbers from 0 to 2<sup>n</sup> - 1,
so a `u8` can store numbers from 0 to 2<sup>8</sup> - 1, which equals 0 to 255.
-->

Chaque variante signée peut stocker des nombres allant de −(2<sup>*n* − 1</sup>)
à 2<sup>*n* − 1</sup> − 1 inclus, où *n* est le nombre de bits que cette
variante utilise.
Un `i8` peut donc stocker des nombres allant de −(2<sup>7</sup>) à
2<sup>7</sup> − 1, c'est-à-dire de −128 à 127. Les variantes non signées peuvent
stocker des nombres de 0 à 2<sup>*n*</sup> − 1, donc un `u8` peut stocker
des nombres allant de 0 à 2<sup>8</sup> − 1, c'est-à-dire de 0 à 255.

<!--
Additionally, the `isize` and `usize` types depend on the architecture of the
computer your program is running on, which is denoted in the table as “arch”:
64 bits if you’re on a 64-bit architecture and 32 bits if you’re on a 32-bit
architecture.
-->

De plus, les types `isize` et `usize` dépendent de l'architecture de
l'ordinateur sur lequel votre programme va s'exécuter, d'où la ligne “archi” :
64 bits si vous utilisez une architecture 64 bits, ou 32 bits si vous utilisez
une architecture 32 bits.

<!--
You can write integer literals in any of the forms shown in Table 3-2. Note
that number literals that can be multiple numeric types allow a type suffix,
such as `57u8`, to designate the type. Number literals can also use `_` as a
visual separator to make the number easier to read, such as `1_000`, which will
have the same value as if you had specified `1000`.
-->

Vous pouvez écrire des littéraux d'entiers dans chacune des formes décrites dans
le tableau 3-2. Notez que les littéraux numériques qui peuvent être de plusieurs types
numériques autorisent l'utilisation d'un suffixe de type, tel que `57u8`, afin de
préciser leur type. Les nombres littéraux peuvent aussi utiliser `_` comme
séparateur visuel afin de les rendre plus lisible, comme par exemple `1_000`,
qui a la même valeur que si vous aviez renseigné `1000`.

<!--
<span class="caption">Table 3-2: Integer Literals in Rust</span>
-->

<span class="caption">Tableau 3-2 : les littéraux d'entiers en Rust</span>

<!--
| Number literals  | Example       |
|------------------|---------------|
| Decimal          | `98_222`      |
| Hex              | `0xff`        |
| Octal            | `0o77`        |
| Binary           | `0b1111_0000` |
| Byte (`u8` only) | `b'A'`        |
-->

| Littéral numérique     | Exemple       |
|------------------------|---------------|
| Décimal                | `98_222`      |
| Hexadécimal            | `0xff`        |
| Octal                  | `0o77`        |
| Binaire                | `0b1111_0000` |
| Octet (`u8` seulement) | `b'A'`        |

<!--
So how do you know which type of integer to use? If you’re unsure, Rust’s
defaults are generally good places to start: integer types default to `i32`.
The primary situation in which you’d use `isize` or `usize` is when indexing
some sort of collection.
-->

Comment pouvez-vous déterminer le type d'entier à utiliser ? Si vous n'êtes pas
sûr, les choix par défaut de Rust sont généralement de bons choix : le type
d'entier par défaut est le `i32`. La principale utilisation d'un `isize` ou d'un
`usize` est lorsque l'on indexe une quelconque collection.

<!--
> ##### Integer Overflow
>
> Let’s say you have a variable of type `u8` that can hold values between 0 and
> 255. If you try to change the variable to a value outside of that range, such
> as 256, *integer overflow* will occur, which can result in one of two
> behaviors. When you’re compiling in debug mode, Rust includes checks for
> integer overflow that cause your program to *panic* at runtime if this
> behavior occurs. Rust uses the term panicking when a program exits with an
> error; we’ll discuss panics in more depth in the [“Unrecoverable Errors with
> `panic!`”][unrecoverable-errors-with-panic]<!-- ignore -- > section in Chapter
> 9.
>
> When you’re compiling in release mode with the `--release` flag, Rust does
> *not* include checks for integer overflow that cause panics. Instead, if
> overflow occurs, Rust performs *two’s complement wrapping*. In short, values
> greater than the maximum value the type can hold “wrap around” to the minimum
> of the values the type can hold. In the case of a `u8`, the value 256 becomes
> 0, the value 257 becomes 1, and so on. The program won’t panic, but the
> variable will have a value that probably isn’t what you were expecting it to
> have. Relying on integer overflow’s wrapping behavior is considered an error.
>
> To explicitly handle the possibility of overflow, you can use these families
> of methods provided by the standard library for primitive numeric types:
>
> - Wrap in all modes with the `wrapping_*` methods, such as `wrapping_add`
> - Return the `None` value if there is overflow with the `checked_*` methods
> - Return the value and a boolean indicating whether there was overflow with
>   the `overflowing_*` methods
> - Saturate at the value’s minimum or maximum values with `saturating_*`
>   methods
-->

> ##### Dépassement d'entier
>
> Imaginons que vous avez une variable de type `u8` qui peut stocker des
> valeurs entre 0 et 255. Si vous essayez de changer la variable pour une valeur
> en dehors de cet intervalle, comme 256, vous aurez un dépassement d'entier
> *(integer overflow)*, qui peut se compter de deux manière. Lorsque vous
> compilez en mode débogage, Rust embarque des vérifications pour détecter les
> cas de dépassements d'entiers qui pourraient faire *paniquer* votre programme
> à l'exécution si ce phénomène se produit. Rust utilise le terme *paniquer*
> quand un programme se termine avec une erreur ; nous verrons plus en détail
> les *paniques* dans une section du [chapitre
> 9][unrecoverable-errors-with-panic]<!-- ignore -->.
>
> Lorsque vous compilez en mode publication *(release)* avec le drapeau
> `--release`, Rust ne va *pas* vérifier les potentiels dépassements d'entiers
> qui peuvent faire paniquer le programme. En revanche, en cas de dépassement,
> Rust va effectuer un *rebouclage du complément à deux*. Pour faire simple, les
> valeurs supérieures à la valeur maximale du type seront “rebouclées” depuis la
> valeur minimale que le type peut stocker. Dans le cas d'un `u8`, la valeur 256
> devient 0, la valeur 257 devient 1, et ainsi de suite. Le programme ne va
> paniquer, mais la variable va avoir une valeur qui n'est probablement pas ce
> que vous attendez à avoir. Se fier au comportement du rebouclage lors du
> dépassement d'entier est considéré comme une faute.
>
> Pour gérer explicitement le dépassement, vous pouvez utiliser les familles
> de méthodes suivantes qu'offrent la bibliothèque standard sur les types de
> nombres primitifs :
>
> - Enveloppez les opérations avec les méthodes `wrapping_*`, comme par exemple
>   `wrapping_add`
> - Retourner la valeur `None` s'il y a un dépassement avec des méthodes
>   `checked_*`
> - Retourner la valeur et un booléen qui indique s'il y a eu un dépassement
>   avec des méthodes `overflowing_*`
> - Saturer à la valeur minimale ou maximale avec des méthodes `saturating_*`

<!--
#### Floating-Point Types
-->

#### Types de nombres à virgule flottante

<!--
Rust also has two primitive types for *floating-point numbers*, which are
numbers with decimal points. Rust’s floating-point types are `f32` and `f64`,
which are 32 bits and 64 bits in size, respectively. The default type is `f64`
because on modern CPUs it’s roughly the same speed as `f32` but is capable of
more precision. All floating-point types are signed.
-->

Rust possède également deux types primitifs pour les *nombres à virgule
flottante* (ou *flottants*), qui sont des nombres avec des décimales. Les types
de flottants en Rust sont les `f32` et les `f64`, qui ont respectivement une
taille en mémoire de 32 bits et 64 bits. Le type par défaut est le `f64` car sur
les processeurs récents ce type est quasiment aussi rapide qu'un `f32` mais est
plus précis. Tous les flottants ont un signe.

<!--
Here’s an example that shows floating-point numbers in action:
-->

Voici un exemple montrant l'utilisation de nombres à virgule flottante :

<!--
<span class="filename">Filename: src/main.rs</span>
-->

<span class="filename">Ficher : src/main.rs</span>

<!--
```rust
{{#rustdoc_include ../listings-sources/ch03-common-programming-concepts/no-listing-06-floating-point/src/main.rs}}
```
-->

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-06-floating-point/src/main.rs}}
```

<!--
Floating-point numbers are represented according to the IEEE-754 standard. The
`f32` type is a single-precision float, and `f64` has double precision.
-->

Les nombres à virgule flottante sont représentés selon la norme IEEE-754. Le
type `f32` est un flottant à simple précision, et le `f64` est à double
précision.

<!--
#### Numeric Operations
-->

#### Les opérations numériques

<!--
Rust supports the basic mathematical operations you’d expect for all of the
number types: addition, subtraction, multiplication, division, and remainder.
Integer division rounds down to the nearest integer. The following code shows
how you’d use each numeric operation in a `let` statement:
-->

Rust offre les opérations mathématiques de base dont vous auriez besoin pour
tous les types de nombres : addition, soustraction, multiplication, division et
modulo. Les divisions d'entiers arrondissent le résultat à l'entier le plus
près. Le code suivant montre comment utiliser chacune des opérations numériques
avec une instruction `let` :

<!--
<span class="filename">Filename: src/main.rs</span>
-->

<span class="filename">Fichier : src/main.rs</span>

<!--
```rust
{{#rustdoc_include ../listings-sources/ch03-common-programming-concepts/no-listing-07-numeric-operations/src/main.rs}}
```
-->

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-07-numeric-operations/src/main.rs}}
```

<!--
Each expression in these statements uses a mathematical operator and evaluates
to a single value, which is then bound to a variable. [Appendix B][appendix_b]<!-- ignore -- > contains a
list of all operators that Rust provides.
-->

Chaque expression de ces instructions utilise un opérateur mathématique et
calcule une valeur unique, qui est ensuite attribuée à une variable. [L'annexe B][appendix_b]<!-- ignore -->
présente une liste de tous les opérateurs que Rust fournit.

<!--
#### The Boolean Type
-->

#### Le type booléen

<!--
As in most other programming languages, a Boolean type in Rust has two possible
values: `true` and `false`. Booleans are one byte in size. The Boolean type in
Rust is specified using `bool`. For example:
-->

Comme dans la plupart des langages de programmation, un type booléen a deux
valeurs possibles en Rust : `true` (vrai) et `false` (faux). Les booléens
prennent un octet en mémoire. Le type booléen est désigné en utilisant `bool`.
Par exemple :

<!--
<span class="filename">Filename: src/main.rs</span>
-->

<span class="filename">Fichier : src/main.rs</span>

<!--
```rust
{{#rustdoc_include ../listings-sources/ch03-common-programming-concepts/no-listing-08-boolean/src/main.rs}}
```
-->

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-08-boolean/src/main.rs}}
```

<!--
The main way to use Boolean values is through conditionals, such as an `if`
expression. We’ll cover how `if` expressions work in Rust in the [“Control
Flow”][control-flow]<!-- ignore -- > section.
-->

Les valeurs booléennes sont principalement utilisées par les structures
conditionnelles, comme l'expression `if`. Nous aborderons le fonctionnement
de `if` en Rust dans la section
[“Les structures de contrôle”][control-flow]<!-- ignore -->.

<!--
#### The Character Type
-->

#### Le type caractère

<!--
Rust’s `char` type is the language’s most primitive alphabetic type. Here’s
some examples of declaring `char` values:
-->

Le type `char` (comme *character*) est le type de caractère le plus
rudimentaire. Voici quelques exemples de déclaration de valeurs de type
`char` :

<!--
<span class="filename">Filename: src/main.rs</span>
-->

<span class="filename">Fichier : src/main.rs</span>

<!--
```rust
{{#rustdoc_include ../listings-sources/ch03-common-programming-concepts/no-listing-09-char/src/main.rs}}
```
-->

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-09-char/src/main.rs}}
```

<!--
Note that we specify `char` literals with single quotes, as opposed to string
literals, which use double quotes. Rust’s `char` type is four bytes in size and
represents a Unicode Scalar Value, which means it can represent a lot more than
just ASCII. Accented letters; Chinese, Japanese, and Korean characters; emoji;
and zero-width spaces are all valid `char` values in Rust. Unicode Scalar
Values range from `U+0000` to `U+D7FF` and `U+E000` to `U+10FFFF` inclusive.
However, a “character” isn’t really a concept in Unicode, so your human
intuition for what a “character” is may not match up with what a `char` is in
Rust. We’ll discuss this topic in detail in [“Storing UTF-8 Encoded Text with
Strings”][strings]<!-- ignore -- > in Chapter 8.
-->

Notez que nous renseignons un litéral `char` avec des guillemets simples,
contrairement aux littéraux de chaîne de caractères, qui nécéssite des doubles
guillemets. Le type `char` de Rust prend quatre octets en mémoire et représente
une valeur scalaire Unicode, ce qui veut dire que cela représente plus de
caractères que l'ASCII. Les lettres accentuées ; les caractères chinois,
japonais et coréens ; les emoji ; les espaces de largeur nulle ont tous une
valeur pour `char` avec Rust. Les valeurs scalaires Unicode vont de `U+0000` à
`U+D7FF` et de `U+E000` à `U+10FFFF` inclus. Cependant, le concept de
“caractère” n'est pas clairement défini par Unicode, donc votre notion de
“caractère” peut ne pas correspondre à ce qu'est un `char` en Rust. Nous
aborderons ce sujet plus en détail au [chapitre 8][strings]<!-- ignore -->.

<!--
### Compound Types
-->

### Les types composés

<!--
*Compound types* can group multiple values into one type. Rust has two
primitive compound types: tuples and arrays.
-->

Les *types composés* peuvent regrouper plusieurs valeurs dans un seul type. Rust
a deux types composés de base : les *tuples* et les tableaux *(arrays)*.

<!--
#### The Tuple Type
-->

#### Le type *tuple*

<!--
A tuple is a general way of grouping together a number of values with a variety
of types into one compound type. Tuples have a fixed length: once declared,
they cannot grow or shrink in size.
-->

Un *tuple* est une manière générale de regrouper plusieurs valeurs
de types différents en un seul type composé. Les tuples ont une taille fixée :
à partir du moment où ils ont été déclarés, on ne peut pas y ajouter ou enlever
des valeurs.

<!--
We create a tuple by writing a comma-separated list of values inside
parentheses. Each position in the tuple has a type, and the types of the
different values in the tuple don’t have to be the same. We’ve added optional
type annotations in this example:
-->

Nous créons un *tuple* en écrivant une liste séparée par des virgules entre des
parenthèses. Chaque emplacement dans le tuple a un type, et les types de chacune
des valeurs dans le tuple n'ont pas forcément besoin d'être les mêmes.
Nous avons ajouté des annotations de type dans cet exemple, mais c'est
optionnel :

<!--
<span class="filename">Filename: src/main.rs</span>
-->

<span class="filename">Fichier : src/main.rs</span>

<!--
```rust
{{#rustdoc_include ../listings-sources/ch03-common-programming-concepts/no-listing-10-tuples/src/main.rs}}
```
-->

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-10-tuples/src/main.rs}}
```

<!--
The variable `tup` binds to the entire tuple, because a tuple is considered a
single compound element. To get the individual values out of a tuple, we can
use pattern matching to destructure a tuple value, like this:
-->

La variable `tup` est liée à tout le tuple, car un tuple est considéré
comme étant un unique élément composé. Pour obtenir un élément précis de ce
tuple, nous pouvons utiliser un filtrage par motif *(pattern matching)* pour
déstructurer ce tuple, comme ceci :

<!--
<span class="filename">Filename: src/main.rs</span>
-->

<span class="filename">Fichier : src/main.rs</span>

<!--
```rust
{{#rustdoc_include ../listings-sources/ch03-common-programming-concepts/no-listing-11-destructuring-tuples/src/main.rs}}
```
-->

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-11-destructuring-tuples/src/main.rs}}
```

<!--
This program first creates a tuple and binds it to the variable `tup`. It then
uses a pattern with `let` to take `tup` and turn it into three separate
variables, `x`, `y`, and `z`. This is called *destructuring*, because it breaks
the single tuple into three parts. Finally, the program prints the value of
`y`, which is `6.4`.
-->

Le programme commence par créer un tuple et il l'assigne à la variable `tup`.
Il utilise ensuite un motif avec `let` pour prendre `tup` et le scinder en
trois variables distinctes : `x`, `y`, et `z`.
On appelle cela *déstructurer*, car il divise le tuple en trois parties.
Puis finalement, le programme affiche la valeur de `y`, qui est `6.4`.

<!--
We can also access a tuple element directly by using a period (`.`) followed by
the index of the value we want to access. For example:
-->

Nous pouvons aussi accéder directement à chaque élément du tuple en utilisant
un point (`.`) suivi de l'indice de la valeur que nous souhaitons obtenir. Par
exemple :

<!--
<span class="filename">Filename: src/main.rs</span>
-->

<span class="filename">Fichier : src/main.rs</span>

<!--
```rust
{{#rustdoc_include ../listings-sources/ch03-common-programming-concepts/no-listing-12-tuple-indexing/src/main.rs}}
```
-->

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-12-tuple-indexing/src/main.rs}}
```

<!--
This program creates the tuple `x` and then makes new variables for each
element by using their respective indices. As with most programming languages,
the first index in a tuple is 0.
-->

Ce programme crée le tuple `x` puis crée une nouvelle variable pour
chaque élément en utilisant leur indices respectifs. Comme dans de nombreux
langages de programmation, le premier indice d'un tuple est 0.

<!--
The tuple without any values, `()`, is a special type that has only one value,
also written `()`. The type is called the *unit type* and the value is called
the *unit value*. Expressions implicitly return the unit value if they don’t
return any other value.
-->

Le tuple sans aucune valeur, `()`, est un type spécial qui a une seule et unique
valeur, qui s'écrit aussi `()`. Ce type est aussi appelé le *type unité* et la
valeur est appelée *valeur unité*. Les expressions retournent implicitement la
valeur unité si elles ne retournent aucune autre valeur.

<!--
#### The Array Type
-->

#### Le type tableau

<!--
Another way to have a collection of multiple values is with an *array*. Unlike
a tuple, every element of an array must have the same type. Unlike arrays in
some other languages, arrays in Rust have a fixed length.
-->

Un autre moyen d'avoir une collection de plusieurs valeurs est d'utiliser
un *tableau*. Contrairement aux tuples, chaque élément d'un tableau doit être du
même type. Contrairement aux tableaux de certains autres langages, les tableaux
de Rust ont une taille fixe.

<!--
We write the values in an array as a comma-separated list inside square
brackets:
-->

Nous écrivons les valeurs dans un tableau via une liste entre des crochets,
séparée par des virgules :

<!--
<span class="filename">Filename: src/main.rs</span>
-->

<span class="filename">Fichier : src/main.rs</span>

<!--
```rust
{{#rustdoc_include ../listings-sources/ch03-common-programming-concepts/no-listing-13-arrays/src/main.rs}}
```
-->

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-13-arrays/src/main.rs}}
```

<!--
Arrays are useful when you want your data allocated on the stack rather than
the heap (we will discuss the stack and the heap more in [Chapter
4][stack-and-heap]<!-- ignore -- >) or when you want to ensure you always have a
fixed number of elements. An array isn’t as flexible as the vector type,
though. A vector is a similar collection type provided by the standard library
that *is* allowed to grow or shrink in size. If you’re unsure whether to use an
array or a vector, chances are you should use a vector. [Chapter
8][vectors]<!-- ignore -- > discusses vectors in more detail.
-->

Les tableaux sont utiles quand vous voulez que vos données soient allouées sur
la pile *(stack)* plutôt que sur le tas *(heap)* (nous expliquerons la pile et
le tas au chapitre 4) ou lorsque vous voulez vous assurer que vous avez toujours
un nombre fixe d'éléments. Cependant, un tableau n'est pas aussi flexible qu'un
vecteur *(vector)*. Un vecteur est un type de collection de données similaire
qui est fourni par la bibliothèque standard qui, lui, peut grandir ou rétrécir
en taille. Si vous ne savez pas si vous devez utiliser un tableau ou un
vecteur, il y a de fortes chances que vous devriez utiliser un vecteur. Le
[chapitre 8][vectors]<!-- ignore --> expliquera les vecteurs.

<!--
However, arrays are more useful when you know the number of elements will not
need to change. For example, if you were using the names of the month in a
program, you would probably use an array rather than a vector because you know
it will always contain 12 elements:
-->

Toutefois, les tableaux s'avèrent plus utiles lorsque vous savez que le nombre
d'éléments n'aura pas besoin de changer. Par exemple, si vous utilisez les noms
des mois dans un programme, vous devriez probablement utiliser un tableau
plutôt qu'un vecteur car vous savez qu'il contient toujours 12 éléments :

<!--
```rust
let months = ["January", "February", "March", "April", "May", "June", "July",
              "August", "September", "October", "November", "December"];
```
-->

```rust
let mois = ["Janvier", "Février", "Mars", "Avril", "Mai", "Juin", "Juillet",
            "Août", "Septembre", "Octobre", "Novembre", "Décembre"];
```

<!--
You write an array’s type using square brackets with the type of each element,
a semicolon, and then the number of elements in the array, like so:
-->

Vous pouvez écrire le type d'un tableau en utilisant des crochets et entre ces
crochets y ajouter le type de chaque élément, un point-virgule, et ensuite le
nombre d'éléments dans le tableau, comme ceci :

<!--
```rust
let a: [i32; 5] = [1, 2, 3, 4, 5];
```
-->

```rust
let a: [i32; 5] = [1, 2, 3, 4, 5];
```

<!--
Here, `i32` is the type of each element. After the semicolon, the number `5`
indicates the array contains five elements.
-->

Ici, `i32` est le type de chaque élément. Après le point-virgule, le nombre `5`
indique que le tableau contient cinq éléments.

<!--
You can also initialize an array to contain the same value for each element by
specifying the initial value, followed by a semicolon, and then the length of
the array in square brackets, as shown here:
-->

Vous pouvez initialiser un tableau pour qu'il contienne toujours la même valeur
pour chaque élément, vous pouvez préciser la valeur initiale, suivie par un
point-virgule, et ensuite la taille du tableau, le tout entre crochets, comme
ci-dessous :

<!--
```rust
let a = [3; 5];
```
-->

```rust
let a = [3; 5];
```

<!--
The array named `a` will contain `5` elements that will all be set to the value
`3` initially. This is the same as writing `let a = [3, 3, 3, 3, 3];` but in a
more concise way.
-->

Le tableau `a` va contenir `5` éléments qui auront tous la valeur
initiale `3`. C'est la même chose que d'écrire `let a = [3, 3, 3, 3, 3];` mais
de manière plus concise.

<!--
##### Accessing Array Elements
-->

##### Accéder aux éléments d'un tableau

<!--
An array is a single chunk of memory of a known, fixed size that can be
allocated on the stack. You can access elements of an array using indexing,
like this:
-->

Un tableau est un simple bloc de mémoire de taille connue et fixe, qui peut être
alloué sur la pile. Vous pouvez accéder aux éléments d'un tableau en utilisant
l'indexation, comme ceci :

<!--
<span class="filename">Filename: src/main.rs</span>
-->

<span class="filename">Fichier : src/main.rs</span>

<!--
```rust
{{#rustdoc_include ../listings-sources/ch03-common-programming-concepts/no-listing-14-array-indexing/src/main.rs}}
```
-->

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-14-array-indexing/src/main.rs}}
```

<!--
In this example, the variable named `first` will get the value `1`, because
that is the value at index `[0]` in the array. The variable named `second` will
get the value `2` from index `[1]` in the array.
-->

Dans cet exemple, la variable qui s'appelle `premier` aura la valeur `1`, car
c'est la valeur à l'indice `[0]` dans le tableau. La variable `second`
récupèrera la valeur `2` depuis l'indice `[1]` du tableau.

<!--
##### Invalid Array Element Access
-->

##### Accès incorrect à un élément d'un tableau

<!--
Let’s see what happens if you try to access an element of an array that is past
the end of the array. Say you run this code, similar to the guessing game in
Chapter 2, to get an array index from the user:
-->

Découvrons ce qui se passe quand vous essayez d'accéder à un élément d'un
tableau qui se trouve après la fin du tableau ? Imaginons que vous exécutiez le
code suivant, similaire au jeu du plus ou du moins du chapitre 2, pour demander
un indice de tableau à l'utilisateur :

<!--
<span class="filename">Filename: src/main.rs</span>
-->

<span class="filename">Fichier : src/main.rs</span>

<!--
```rust,ignore,panics
{{#rustdoc_include ../listings-sources/ch03-common-programming-concepts/no-listing-15-invalid-array-access/src/main.rs}}
```
-->

```rust,ignore,panics
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-15-invalid-array-access/src/main.rs}}
```

<!--
This code compiles successfully. If you run this code using `cargo run` and
enter 0, 1, 2, 3, or 4, the program will print out the corresponding value at
that index in the array. If you instead enter a number past the end of the
array, such as 10, you’ll see output like this:
-->

Ce code compile avec succès. Si vous exécutez ce code avec `cargo run` et que
vous entrez 0, 1, 2, 3 ou 4, le programme affichera la valeur correspondante à
cet indice dans le tableau. Si au contraire, vous entrez un indice après la fin
du tableau tel que 10, ceci s'affichera :

<!--
<!-- manual-regeneration
cd listings/ch03-common-programming-concepts/no-listing-15-invalid-array-access
cargo run
10
-- >

```console
thread 'main' panicked at 'index out of bounds: the len is 5 but the index is 10', src/main.rs:19:19
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```
-->

```console
thread 'main' panicked at 'index out of bounds: the len is 5 but the index is 10', src/main.rs:19:19
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

<!--
The program resulted in a *runtime* error at the point of using an invalid
value in the indexing operation. The program exited with an error message and
didn’t execute the final `println!` statement. When you attempt to access an
element using indexing, Rust will check that the index you’ve specified is less
than the array length. If the index is greater than or equal to the length,
Rust will panic. This check has to happen at runtime, especially in this case,
because the compiler can’t possibly know what value a user will enter when they
run the code later.
-->

Le programme a rencontré une erreur *à l'exécution*, au moment d'utiliser une
valeur invalide comme indice. Le programme s'est arrêté avec un message d'erreur
et n'a pas exécuté la dernière instruction `println!`. Quand vous essayez
d'accéder à un élément en utilisant l'indexation, Rust va vérifier que l'indice
que vous avez demandé est plus petit que la taille du tableau. Si l'indice est
supérieur ou égal à la taille du tableau, Rust va *paniquer*. Cette vérification
doit avoir lieu à l'exécution, surtout dans ce cas, parce que le compilateur ne
peut pas deviner la valeur qu'entrera l'utilisateur quand il exécutera le code
plus tard.

<!--
This is an example of Rust’s memory safety principles in action. In many
low-level languages, this kind of check is not done, and when you provide an
incorrect index, invalid memory can be accessed. Rust protects you against this
kind of error by immediately exiting instead of allowing the memory access and
continuing. Chapter 9 discusses more of Rust’s error handling.
-->

C'est un exemple de mise en pratique des principes de sécurité de la mémoire par
Rust. Dans de nombreux langages de bas niveau, ce genre de vérification n'est
pas effectuée, et quand vous utilisez un indice incorrect, de la mémoire
invalide peut être récupérée. Rust vous protège de ce genre d'erreur en quittant
immédiatement l'exécution au lieu de permettre l'accès en mémoire et continuer
son déroulement. Le chapitre 9 expliquera la gestion d'erreurs de Rust.

<!--
[comparing-the-guess-to-the-secret-number]:
ch02-00-guessing-game-tutorial.html#comparing-the-guess-to-the-secret-number
[control-flow]: ch03-05-control-flow.html#control-flow
[strings]: ch08-02-strings.html#storing-utf-8-encoded-text-with-strings
[stack-and-heap]: ch04-01-what-is-ownership.html#the-stack-and-the-heap
[vectors]: ch08-01-vectors.html
[unrecoverable-errors-with-panic]: ch09-01-unrecoverable-errors-with-panic.html
[wrapping]: ../std/num/struct.Wrapping.html
[appendix_b]: appendix-02-operators.md
-->

[comparing-the-guess-to-the-secret-number]:
ch02-00-guessing-game-tutorial.html#comparer-le-nombre-saisi-au-nombre-secret
[control-flow]: ch03-05-control-flow.html#les-structures-de-contrôle
[strings]: ch08-02-strings.html
[stack-and-heap]: ch04-01-what-is-ownership.html
[vectors]: ch08-01-vectors.html
[unrecoverable-errors-with-panic]: ch09-01-unrecoverable-errors-with-panic.html
[wrapping]: https://doc.rust-lang.org/std/num/struct.Wrapping.html
[appendix_b]: appendix-02-operators.md
