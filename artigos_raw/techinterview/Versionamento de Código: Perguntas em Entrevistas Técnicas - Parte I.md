## Objetivo

O intuíto desse artigo é expor algumas perguntas técnicas que eu já fiz em entrevistas técnicas ou perguntas que me foram feitas.

Essa parte tratará exclusivamente sobre git ok ;D ? Outras publicações serão escritas com mais conteúdo específico.

As respostas são apenas a minha visão sobre o assunto, por tanto, de forma alguma são verdade absoluta, e inclusive pode ser que haja espaço para melhorias, correções e até visões diferentes das apresentadas aqui, fique a vontade, use os comentários e enriqueça a discussão.

## Versionamento de Código

### **Para que serve o comando git add ?**
O comando git add é usado para adicionar arquivos ao índice (ou "área de preparação") do Git, que é uma espécie de área intermediária entre o diretório de trabalho e o repositório Git. Quando você faz alterações nos arquivos do seu projeto, essas alterações ficam registradas no diretório de trabalho. No entanto, o Git não registra automaticamente todas essas alterações no histórico de commits. É necessário adicionar explicitamente essas alterações ao índice por meio do comando git add.

Ao adicionar arquivos ao índice com o comando git add, você está informando ao Git que esses arquivos devem ser incluídos no próximo commit que você fizer. O comando git add pode ser usado de diferentes maneiras, dependendo do que você deseja fazer:

```bash
git add <nome_do_arquivo> adiciona um arquivo específico ao índice.
```

```bash
git add . adiciona todos os arquivos modificados ou adicionados ao índice.
```

```bash
git add -u adiciona todos os arquivos modificados ou removidos ao índice, mas não adiciona novos arquivos.
```

Depois de adicionar os arquivos ao índice, você pode confirmar as alterações com o comando git commit. É importante lembrar que o índice é diferente do repositório Git. Adicionar um arquivo ao índice não significa que ele já está registrado no histórico de commits. O arquivo só será registrado no histórico de commits após um commit ser feito com as alterações adicionadas ao índice.

### **Para que serve o comando git commit ?**

**TL;DR:** O comando git commit é uma das principais ferramentas do Git, que permite que você crie um histórico completo e detalhado de todas as alterações feitas em um repositório.

O comando git commit é usado para registrar as alterações feitas em um repositório Git. Quando você faz modificações nos arquivos do seu projeto e está satisfeito com as mudanças, você precisa executar o comando git commit para criar uma nova revisão (ou commit) no histórico do repositório. Esse comando cria uma nova versão do código com uma mensagem de confirmação que descreve as mudanças que você fez.

A sintaxe básica do comando é git commit -m "mensagem de commit". A opção -m é usada para adicionar uma mensagem de commit curta e descritiva, que deve resumir o que foi modificado no código. Por exemplo: "Adiciona novo recurso X" ou "Corrige bug Y".

É possível incluir várias alterações em um único commit. Para isso, é necessário adicionar todos os arquivos que você deseja incluir no commit ao índice (usando o comando git add) antes de executar o comando git commit.

Ao criar um commit, o Git salva uma "imagem" do estado atual do seu repositório, incluindo todas as alterações que foram adicionadas ao índice. Cada commit é identificado por um hash único, que é usado para referenciar a revisão em outras partes do Git (como ramificações, tags e histórico de commits). Além disso, o commit também armazena o nome do autor, a data em que foi criado e uma mensagem descritiva.

### **Para que serve o comando git push ?**

O comando git push é usado para enviar as alterações que você fez no seu repositório local para um repositório remoto (como o GitHub).

Depois de fazer alterações no seu repositório local, você pode usar o comando git push para atualizar o repositório remoto com as alterações. O push envia os commits que você fez no seu branch local para o branch correspondente no repositório remoto.

A sintaxe básica do comando git push é git push <remote> <branch>, onde <remote> é o nome do repositório remoto (geralmente origin) e <branch> é o nome do branch no repositório remoto que você deseja atualizar com suas alterações.

Antes de executar o comando git push, você precisa garantir que o seu repositório local esteja atualizado com as últimas alterações do repositório remoto. Para fazer isso, você pode executar o comando git pull antes do git push.

Caso haja conflitos entre as alterações feitas localmente e as alterações no repositório remoto, o git push irá falhar e você precisará resolver os conflitos antes de tentar enviar as alterações novamente.

O git push também pode ser usado para criar um novo branch remoto. Para fazer isso, você pode usar a opção -u para definir o upstream branch. Por exemplo: git push -u origin my-new-branch. Isso criará um novo branch my-new-branch no repositório remoto e fará com que o seu branch local rastreie esse novo branch.

Em resumo, o comando git push é uma das principais ferramentas do Git para enviar as alterações que você fez no seu repositório local para um repositório remoto. Ele permite que você compartilhe suas alterações com outras pessoas que trabalham no mesmo projeto e mantenha o histórico completo e atualizado do repositório.

### **Para que serve o comando git pull ?**

O comando git pull é usado para buscar as alterações mais recentes de um repositório remoto (como o GitHub) e mesclá-las com as alterações do seu repositório local. Isso é útil para garantir que você esteja trabalhando com a versão mais atual do código, especialmente se outras pessoas também estão contribuindo para o mesmo projeto.

O git pull combina dois outros comandos do Git: git fetch e git merge. Quando você executa o git pull, o Git primeiro faz um fetch para buscar as atualizações mais recentes do repositório remoto, mas sem aplicar as alterações no seu repositório local. Em seguida, ele faz um merge para combinar as alterações do repositório remoto com as alterações do seu repositório local.

A sintaxe básica do comando git pull é git pull <remote> <branch>, onde <remote> é o nome do repositório remoto (geralmente origin) e <branch> é o nome do branch no repositório remoto que você deseja mesclar com o seu branch local.

Quando você executa o git pull, pode ocorrer um conflito de mesclagem se houver alterações conflitantes no seu repositório local e no repositório remoto. O Git tentará mesclar automaticamente as alterações, mas se houver conflitos que não possam ser resolvidos automaticamente, você precisará resolvê-los manualmente.

Em resumo, o comando git pull é usado para buscar as alterações mais recentes de um repositório remoto e mesclá-las com as alterações do seu repositório local. É uma maneira conveniente de manter o seu código atualizado e sincronizado com as alterações de outras pessoas que também estão trabalhando no mesmo projeto.

### **Qual a diferença entre git reset, git reset --hard e git revert ?**

Os comandos git reset, git reset --hard e git revert são usados para desfazer alterações em um repositório Git, mas cada um tem um comportamento diferente.

git reset: O comando git reset é usado para desfazer as alterações em um branch local, removendo as alterações que foram adicionadas ao índice (staging area) e ao histórico de commits. É importante notar que, ao usar o comando git reset, as alterações são permanentemente excluídas do histórico de commits, portanto, é uma operação perigosa e que deve ser usada com cuidado. A sintaxe básica é git reset <commit> para definir o ponto de reset.

git reset --hard: O comando git reset --hard é semelhante ao git reset, mas também descarta as alterações no diretório de trabalho (working directory). Isso significa que todas as alterações que não foram adicionadas ao índice (staging area) serão permanentemente perdidas e substituídas pelas alterações do commit especificado. A sintaxe básica é git reset --hard <commit>.

git revert: O comando git revert é usado para desfazer um commit anterior, criando um novo commit que desfaz as alterações introduzidas no commit original. Isso é útil quando você deseja desfazer um commit, mas deseja manter um registro das alterações que foram desfeitas. A sintaxe básica é git revert <commit>.

Em resumo, o git reset e o git reset --hard são usados para desfazer alterações em um repositório Git, mas ambos removem permanentemente as alterações. O git revert, por outro lado, cria um novo commit que desfaz as alterações de um commit anterior, mantendo um registro das alterações desfeitas. A escolha do comando a ser usado depende do objetivo que se pretende alcançar.

### **Para que serve o comando git stash ?**

O comando git stash é usado para armazenar temporariamente as alterações em um branch Git que ainda não foram commitadas, permitindo que você as remova temporariamente do diretório de trabalho (working directory) para trabalhar em outra tarefa ou branch sem fazer commit das alterações incompletas.

Ao executar o comando git stash, o Git salva o estado atual do diretório de trabalho (working directory) e do índice (staging area) em um local temporário e limpa o diretório de trabalho para o estado do último commit. Isso permite que você trabalhe em outra tarefa ou branch sem as alterações incompletas.

Você pode usar o comando git stash apply para restaurar as alterações armazenadas anteriormente, ou git stash pop para aplicar as alterações armazenadas e removê-las da pilha de stash.

O git stash é especialmente útil em situações em que você deseja trabalhar em uma nova tarefa, mas não deseja fazer commit das alterações incompletas em um branch em andamento. Também pode ser usado em situações em que você precisa alternar entre diferentes branches rapidamente sem precisar fazer commit das alterações incompletas em um branch específico.

### **Para que serve o comando git log ?**

O comando git log é usado para exibir um registro detalhado de todos os commits (confirmações) que foram feitos em um repositório Git. Ele mostra informações sobre os commits em ordem cronológica reversa, ou seja, do commit mais recente para o mais antigo.

Ao executar o comando git log, você verá informações sobre cada commit, como seu hash (identificador único), autor, data, hora, mensagem de commit e as alterações feitas. Além disso, é possível usar algumas opções para personalizar a saída do comando, como filtrar por autor, data ou mensagem de commit, exibir apenas uma determinada quantidade de commits ou visualizar um gráfico das ramificações.

O git log é útil para monitorar o histórico de alterações em um repositório Git, revisar as alterações feitas por outros colaboradores do projeto, entender como uma determinada funcionalidade foi desenvolvida ao longo do tempo e identificar onde um problema foi introduzido em caso de bugs ou erros.

### **O que é cherry pick ?**

O comando git cherry-pick é usado para aplicar um commit específico de uma branch para outra. Ele permite que você selecione um único commit de uma branch e o "copie" para outra branch, aplicando as alterações desse commit.

O cherry-pick é útil em situações em que você deseja aplicar as alterações de um commit específico de uma branch para outra sem mesclar as branches inteiras. Por exemplo, se você corrigiu um bug crítico em uma branch de correção de bugs e deseja aplicar essa correção em uma branch de produção sem trazer todas as outras alterações da branch de correção de bugs.

Para executar o cherry-pick, primeiro você precisa obter o hash do commit que deseja aplicar e, em seguida, usar o comando git cherry-pick <hash_do_commit> na branch para a qual deseja aplicar as alterações. É importante lembrar que o cherry-pick pode causar conflitos de mesclagem se houver alterações semelhantes em ambas as branches, portanto, é importante revisar as alterações após a aplicação.