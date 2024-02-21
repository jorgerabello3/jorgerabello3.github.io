# API de datas

No Java 8 surgiu o pacote `java.time` que nos trouxe uma nova API de datas. Essa API foi inspirada parcialmente no Joda Time que é uma API que já existe há algum tempo e é bem melhor para trabalhar com datas. O Joda-Time é uma biblioteca open source bastante conhecida.

## Datas de Modo Mais Fluente

Vamos imaginar que você precise criar uma data com um mês a partir da data atual, no Java 7:

```java
Calendar mesQueVem = Calendar.getInstance();
mesQueVem.add(Calendar.MONTH, 1);
```

Com a nova API de datas o código fica bem mais moderno utilizando sua interface fluente

```java
LocalDate mesQueVem = LocalDate.now().plusMonths(1);
```

Além de `plusMonths()` ainda podemos adicionar dias `plusDays()`, anos `plusYears()` e por aí vai.

De forma semelhante podemos decrementar os valores

```java
LocalDate mesPassado = LocalDate.now().minusMonths(1);
```

A classe `LocalDate` representa uma data sem time zone, algo como `25-01-1988`, se as informações de horário forem importantes usamos a calsse `LocalDateTime`

```java
LocalDateTime mesQueVem = LocalDateTime.now().plusMonths(1);
LocalDateTime mesPassado = LocalDateTime.now().minusMonths(1);
```

Com LocalDateTime a saída ficará mais ou menos assim:

```bash
25-01-1988T15:01.345
```

Outra forma de criar uma data com horário específico seria utilizar o método `atTime()` da classe LocalDate

```java
LocalDateTime dateTime = LocalDate.now().atTime(12, 0);
```

Note que criamos a partir de `LocalDate`, porém `atTime()` retorna um `LocalDateTime`.

Assim como foi feito com o método `atTime()` podemo combinar os diferentes modelos

```java
LocalTime agora = LocalTime.now();
LocalDate hoje = LocalDate.now();
LocalDateTime dataEHora = hoje.atTime(agora);
```

Se precisarmos de um horário baseado em um TimeZone, podemos contar com o método `atZone()`

```java
LocalTime agora = LocalTime.now();
LocalDate hoje = LocalDate.now();
LocalDateTime dataEHora = hoje.atTime(agora);

ZonedDateTime dataComHoraETimeZone = dataEHora.atZone(ZoneId.of("America/Sao_Paulo"));
```

Podemos converter esses objetos para outras medidas de tempo utilizando os métodos to, por exemplo `toLocalDateTime()`.

```java
LocalDateTime dataEHoraSemTimeZone = dataComHoraETimeZone.toLocalDateTime();
```

As classes dessa nova API ainda contam com métodos estáticos of, que são factories methods para construção de novas instâncias

```java
LocalDate date = LocalDate.of(1988, 01, 25);
LocalDateTime dataTime = LocalDateTime.of(1988, 01, 25, 10, 30);
```

Podemos converter Strings em datas com o método `parse()` porém esse método exige que a String esteja em um formato correto `YYYY-MM-DD`

```java
LocalDate date = LocalDate.parse("1988-01-25");
System.out.println(date);
```

O modelo do `java.time` é imutável, cada operação devolve um novo valor nunca alterando o valor interno dos horários, datas e intervalos utilizados na operação. De modo semelhante aos setters, os modelos imutáveis possuem métodos with para alterar uma data. Por exemplo:

```java
LocalDate date = LocalDate.now().withYear(1988);
System.out.println(date.getYear());
```

Aqui é criada uma data com o ano atual, por conta do `now()` o ano é 2023 nesse caso, depois com o o `withYear()` o ano é alterado para 1988 !

Outros comportamentos essenciais e interessantes é poder saber se alguma medida de tempo acontece antes, depois ou ao mesmo tempo que outra, para essa finalidade temos os métodos is

```java
LocalDate hoje = LocalDate.now();
LocalDate amanha = LocalDate.now().plusDays(1);

System.out.println(hoje.isBefore(amanha));
System.out.println(hoje.isAfter(amanha));
System.out.println(hoje.isEqual(amanha));
```

Nesse exemplo apenas `isBefore()` vai retornar true.

Para comparar datas iguais mas em time zones diferentes também temos um método chamado `isEqual()` já que o `equals()`` não funcionaria.

```java
ZonedDateTime tokyo = ZonedDateTime.of(1988, 5, 13, 10, 30, 0, 0, ZoneId.of("Asia/Tokyo"));

ZonedDateTime saoPaulo = ZonedDateTime.of(1988, 5, 13, 10, 30, 0, 0, ZoneId.of("America/Sao_Paulo"));

System.out.println(tokyo.isEqual(saoPaulo));
```

Para que esse resultado seja true precisaríamos acertar a diferença de 12 horas entre as duas time zones

```java
ZonedDateTime tokyo = ZonedDateTime .of(1988, 5, 13, 10, 30, 0, 0, ZoneId.of("Asia/Tokyo"));

ZonedDateTime saoPaulo = ZonedDateTime.of(1988, 5, 13, 10, 30, 0, 0, ZoneId.of("America/Sao_Paulo"));
```

Uma curiosidade, experimente mudar o mês para 1 (janeiro) e você notará que o `isEqual()` falha, se você debugar vai notar o por que:

Um objeto `ZonedDateTime` tem alguns atributos, a saber:

- dateTime: o date time propriamente dito algo como 1988-05-13T10:30+09:00[Asia/Tokyo]

- offset: O deslocamento em horas a partir de UTC/Greenwich, algo como +09:00

- zone: a zona propriamente dita, algo como Asia/Tokyo

Se você prestar atenção ao objeto gerado para São Paulo, vai notar que no mês de maio o offset é -03:00 mas em janeiro é -02:00, isso ocorre por que, nesse momento momento em que o horário de verão está em vigor no Brasil, o offset pode ser diferente. Durante o horário de verão, o Brasil pode mudar para o fuso horário de "-02:00" para aproveitar mais a luz do dia.

Sendo assim para que esse mesmo código funciona é preciso compensar essa 1 hora

```java
ZonedDateTime tokyo = ZonedDateTime.of(1988, 1, 13, 10, 30, 0, 0, ZoneId.of("Asia/Tokyo"));

ZonedDateTime saoPaulo = ZonedDateTime.of(1988, 1, 13, 10, 30, 0, 0, ZoneId.of("America/Sao_Paulo"));

// compensando o horário de verão
tokyo = tokyo.plusHours(11);

System.out.println(tokyo.isEqual(saoPaulo));
```

A API possui ainda outros modelos que facilitam bastante o nosso trabalho como as classes `MonthDay`, `Year` e `YearMonth`, por exemplo:

```java
System.out.println("hoje é dia: " + MonthDay.now().getDayOfMonth());
```

Podemos por exemplo obter a o mês de uma data

```java
LocalDate hoje = LocalDate.now();
YearMonth yearMonth = YearMonth.from(hoje);
System.out.println(yearMonth.getMonth() + " " + yearMonth.getYear());
```

## Enums vs Constantes

Calendar utilizava constantes para representar unidades temporais, a nova API faz isso por meio de enums, como exemplo podemos citar a enum `Month`, onde cada valor tem um valor inteiro e representa o mes, seguindo o intervalo de 1 (Janeiro), 2 (Fevereiro) até 12 (Dezembro). Você não precisa mas trabalhar com essas enums deixa seu código muito mais legível.

```java
System.out.println(LocalDate.of(1988, 01, 25));
System.out.println(LocalDate.of(1988, Month.JANUARY, 25));
```

Outra vantagem de se utilizar as enums é a de poder contar com seus métodos auxiliares, por exemplo o `firstMonthOfQuarter()` para consultar o mês correspondente ao primeiro mês deste trimestre.

```java
System.out.println(Month.NOVEMBER.firstMonthOfQuarter());
System.out.println(Month.JANUARY.plus(2));
System.out.println(Month.JANUARY.minus(1));
```

Note que ao imprimir o nome de um mês vamos sempre ver o mês em ingles, por exemplo `JANUARY`, `JULY` e por aí vai. Para obter o mes em outra configuração podemos contar com o Locales

```java
Locale pt = new Locale("pt");
System.out.println(Month.JANUARY.getDisplayName(TextStyle.FULL, pt));
```

O argumento `TextStyle` é uma enum que informa o estilo de formatação:

Os valores possíveis são:

```java
Locale pt = new Locale("pt");

System.out.println(Month.JANUARY.getDisplayName(TextStyle.FULL, pt)); // Janeiro

System.out.println(Month.JANUARY.getDisplayName(TextStyle.FULL_STANDALONE, pt)); // 1

System.out.println(Month.JANUARY.getDisplayName(TextStyle.NARROW, pt)); // J

System.out.println(Month.JANUARY.getDisplayName(TextStyle.NARROW_STANDALONE, pt)); // 1

System.out.println(Month.JANUARY.getDisplayName(TextStyle.SHORT, pt)); // jan

System.out.println(Month.JANUARY.getDisplayName(TextStyle.SHORT_STANDALONE, pt)); // 1
```

Outro método interessante introduzido na api `java.time` foi o `dayOfWeek()` com ele podemos representar facilmente um dia da semana.

## Formatando datas

Para formatar datas podemos fazer algo como

```java
LocalDateTime agora = LocalDateTime.now();

String dataFormatada = agora.format(DateTimeFormatter.ISO_LOCAL_TIME);

System.out.println(dataFormatada);
```

A saída seria algo como `03:12:31.428` ou seja usando o pattern hh:mm:ss.ms

O `DateTimeFormatter` possui diversas outras opções, mas vamos dizer que tenhamos um formato próprio ou queremos criar algo que não exista ainda, uma das formas seria utilizar o método `ofPattern()`, que recebe uma String como parâmetro

```java
LocalDateTime agora = LocalDateTime.now();

String dataFormatada = agora.format(DateTimeFormatter.ofPattern("dd/MM/yyyy"));

System.out.println(dataFormatada);
```

Esse método ainda possui uma sobrecarga que além do pattern pode receber um `Locale`.

## Lidando com datas inválidas

Qual a saída desse código ?

```java
Calendar calendar = Calendar.getInstance();
calendar.set(2014, Calendar.FEBRUARY, 30);
SimpleDateFormat simpleDateFormat = new SimpleDateFormat("dd/MM/yyyy");
System.out.println(simpleDateFormat.format(calendar.getTime()));
```

Se você respondeu 30 de Fevereiro de 2014 você errou feio, errou rude ! Porém ao executar o código, veja que não há se quer um alerta sobre o o problema eminente, o Calendar simplesmente ajusta a data para 02/03/2014 e vida que segue ! Nem é preciso dizer que isso poderia causar diversos tipos de prejuízos né ?

Agora tente fazer o mesmo com a nova API de datas

```java
LocalDate.of(2014, Month.FEBRUARY, 30);
```

E você receberá uma belíssima exception

```bash
Exception in thread "main" java.time.DateTimeException: Invalid date 'FEBRUARY 30'
	at java.time.LocalDate.create(LocalDate.java:431)
	at java.time.LocalDate.of(LocalDate.java:249)
	at br.com.jorgerabellodev.lambadas.datas.Main.main(Main.java:9)
```

O mesmo acontece com horários

```java
LocalDateTime hour = LocalDate.now().atTime(25, 12);
```

Esse código produzirá um `java.time.DateTimeException`

```bash
Exception in thread "main" java.time.DateTimeException: Invalid value for HourOfDay (valid values 0 - 23): 25
	at java.time.temporal.ValueRange.checkValidValue(ValueRange.java:311)
	at java.time.temporal.ChronoField.checkValidValue(ChronoField.java:703)
	at java.time.LocalTime.of(LocalTime.java:296)
	at java.time.LocalDate.atTime(LocalDate.java:1724)
	at br.com.jorgerabellodev.lambadas.datas.Main.main(Main.java:10)
```

## Duração e período

Só quem já precisou trabalhar com diferença de alguma medida de tempo no Java até a versão 7 sabe o inferno que era fazer isso, apenas para compartilhar um pouco do sofrimento aqui vai um código que poderia ser utilizado pra tal finalidade caso você tenha tido a sorte de não ter precisado fazer isso até hoje.

```java
 Calendar now = Calendar.getInstance();

Calendar anotherDate = Calendar.getInstance();
anotherDate.set(1988, Calendar.JANUARY, 25);

long difference = now.getTimeInMillis() - anotherDate.getTimeInMillis();

long milliSecondsInADay = 1000 * 60 * 60 * 24;

long dias = difference / milliSecondsInADay;

System.out.println(dias);
```

O problema é resolvido porém trabalhar com diferença entre datas utilizando milisegundos pode nem sempre ser uma boa ideia !

Com a nova API de datas, podemos fazer a mesma coisa de forma mais segura e e simples

```java
LocalDate now = LocalDate.now();

LocalDate anotherDate = LocalDate.of(1988, Month.JANUARY, 25);

long dias = ChronoUnit.DAYS.between(anotherDate, now);

System.out.println(dias);
```

A enum ChronoUnit está presente no pacote `java.time.temporal` e possui uma representação para cada medida de tempo e data, além de vários métodos auxiliares que facilitam extrair informações úteis de datas

```java
LocalDate now = LocalDate.now();

LocalDate anotherDate = LocalDate.of(1988, Month.JANUARY, 25);

long dias = ChronoUnit.DAYS.between(anotherDate, now);

long meses = ChronoUnit.MONTHS.between(anotherDate, now);

long anos = ChronoUnit.YEARS.between(anotherDate, now);

System.out.printf("%s dias, %s meses e %s anos", dias, meses, anos);
```

Agora se precisarmos obter os dias, meses e anos entre duas datas, podemos utilizar `Period`, essa classe da API também possui o método `between()` que recebe duas instâncias de `LocalDate`

```java
LocalDate now = LocalDate.now();
LocalDate anotherDate = LocalDate.of(1988, Month.JANUARY, 25);

Period period = Period.between(anotherDate, now);

System.out.printf("%s dias, %s meses e %s anos",
            period.getDays(), period.getMonths(), period.getYears());
```

Pode ser que precisemos criar um período entre horas, minutos e segundos, nesse caso `Period` não servirá, para essa finalidade vamos utilizar `Duration`

```java
LocalDateTime now = LocalDateTime.now();

LocalDateTime aFewHours = LocalDateTime.now().plusHours(4);

Duration difference = Duration.between(now, aFewHours);

if (difference.isNegative()) {
    difference = difference.negated();
}

System.out.printf("%s horas, %s minutos, %s segundos",
            difference.toHours(), difference.toMinutes(), difference.getSeconds());
```

Com esse último artigo, encerro a demonstração das features do Java 8 e espero que você que está lendo, tenha compreendido um pouco melhor essas funcionalidades, além disso espero que você escreve código de forma mais fluída e com menos sofrimento !