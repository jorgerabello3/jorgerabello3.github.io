## Objetivo

O intuíto desse artigo é expor algumas perguntas técnicas que eu já fiz em entrevistas técnicas ou perguntas que me foram feitas.

Essa parte tratará exclusivamente sobre git ok ;D ? Outras publicações serão escritas com mais conteúdo específico.

As respostas são apenas a minha visão sobre o assunto, por tanto, de forma alguma são verdade absoluta, e inclusive pode ser que haja espaço para melhorias, correções e até visões diferentes das apresentadas aqui, fique a vontade, use os comentários e enriqueça a discussão.

## Versionamento de Código

### **Qual a diferença entre git e hgithub ?**

Git é um sistema de controle de versão distribuído de código-fonte, ou seja, é uma ferramenta de linha de comando que permite que desenvolvedores gerenciem as diferentes versões de um projeto em seu próprio computador. O Git mantém um histórico completo de todas as alterações realizadas em um projeto e permite que os desenvolvedores revertam para uma versão anterior a qualquer momento.

O GitHub, por outro lado, é uma plataforma de hospedagem de repositórios Git na nuvem. Ele fornece uma interface web que permite aos desenvolvedores gerenciar e colaborar em projetos Git hospedados na plataforma. Além disso, o GitHub oferece recursos adicionais, como rastreamento de problemas, gerenciamento de projetos, integração contínua, revisão de código e muito mais.

Em resumo, o Git é uma ferramenta de controle de versão de código-fonte, enquanto o GitHub é uma plataforma de hospedagem de repositórios Git que permite que os desenvolvedores colaborem em projetos, rastreiem problemas e gerenciem seus projetos de forma mais eficiente. O Git pode ser usado sem o GitHub, mas o GitHub não pode ser usado sem o Git.

### **O que é git flow ?**

O Git Flow é um modelo de fluxo de trabalho para o Git que fornece uma estrutura para gerenciar o processo de desenvolvimento de software. Ele define um conjunto de regras e convenções que os desenvolvedores devem seguir para garantir que as alterações de código sejam gerenciadas de forma consistente e eficiente.

O Git Flow é baseado em duas ramificações principais: a branch "master" e a branch "develop". A branch "master" é usada para manter uma versão estável do código, enquanto a branch "develop" é usada para integrar as alterações de código e desenvolver novos recursos.

Além dessas ramificações principais, o Git Flow define outras ramificações, como "feature", "release" e "hotfix". A ramificação "feature" é usada para desenvolver novos recursos, enquanto a ramificação "release" é usada para preparar o código para o lançamento. A ramificação "hotfix" é usada para corrigir problemas críticos na versão atual do código.

O Git Flow também define um conjunto de convenções para os nomes de ramificações, tags de versão e mensagens de commit, que ajudam a manter um histórico claro e conciso das alterações de código.

O Git Flow é amplamente utilizado na indústria de desenvolvimento de software e é suportado por várias ferramentas de integração contínua e hospedagem de repositórios, como o GitHub e o Bitbucket. Ele é considerado uma abordagem eficiente e organizada para o gerenciamento do processo de desenvolvimento de software usando o Git.

### **O que é git rebase ?**

Rebase é uma operação no Git que permite que um desenvolvedor atualize sua branch local com as alterações mais recentes em outra branch, enquanto mantém um histórico linear de alterações.

O rebase reescreve o histórico de commits da branch atual, aplicando os commits da branch com a qual se deseja integrar. Isso significa que em vez de mesclar as alterações, como ocorre com a operação "merge", o rebase cria uma nova série de commits, que incluem as alterações da branch atual e as alterações da branch com a qual se deseja integrar.

O resultado é um histórico de commits mais limpo e organizado, com um fluxo linear de alterações. No entanto, o rebase pode ser mais complicado do que o merge e deve ser usado com cuidado para evitar a perda de dados ou conflitos.

O rebase é especialmente útil em situações em que vários desenvolvedores estão trabalhando na mesma branch e é necessário manter um histórico de commits claro e organizado. Ele também pode ser usado para atualizar uma branch com as alterações mais recentes em uma branch remota antes de mesclar as alterações localmente.

### **Pra que serve a opção --amend do comando git commit ?**

A opção --amend do comando git commit é usada para alterar o commit mais recente do repositório Git. Quando usada, ela combina as alterações que você está preparando para commit com as alterações do commit anterior.

Isso pode ser útil em várias situações, como quando você percebe que esqueceu de incluir um arquivo ou deseja modificar a mensagem de commit anterior. Em vez de criar um novo commit, você pode usar a opção --amend para atualizar o commit anterior com as alterações que você acabou de fazer.

Para usar a opção --amend, basta adicionar essa opção ao comando git commit, juntamente com as alterações que você deseja incluir no commit. 

Por exemplo:

```bash
git add file.txt
git commit --amend -m "nova mensagem de commit"
```

Esse comando adiciona o arquivo file.txt ao commit anterior e atualiza a mensagem de commit para "nova mensagem de commit". O histórico de commits é atualizado automaticamente para refletir as alterações.

No entanto, é importante lembrar que, se você já compartilhou o commit original com outras pessoas, a alteração do histórico de commits pode causar problemas de sincronização e conflitos no repositório Git compartilhado. Por isso, a opção --amend deve ser usada com cuidado e apenas em commits não compartilhados.

Além disso podemos utilizar o amendo com as opções `--no-verify` e `--no-edit`

A opção `--no-verify` é usada para ignorar os hooks de pré-commit definidos no repositório Git. Os hooks de pré-commit são scripts personalizados que são executados antes de cada commit para verificar o código e aplicar regras de estilo e formatação. Se a opção --no-verify for usada, esses hooks não serão executados e as alterações serão enviadas diretamente para o histórico de commits.

A opção `--no-edit` é usada para aceitar a mensagem de commit padrão gerada pelo Git sem abrir o editor de texto. O Git normalmente abre um editor de texto para permitir que o usuário modifique a mensagem de commit antes de confirmar as alterações. Se a opção --no-edit for usada, a mensagem de commit padrão será usada sem edição.

### **Por que ao fazer um amend commit é necessário utilizar o git push com a opção -f ?**

**TL;DR:** Ao fazer um amend o hash do commit muda e por essa razão o git entende que essa é outra referência diferente da anterior por isso é necessário executar com a opção `-f` ou `--force`.

Para verificar isso faça o seguinte

```bash
# adicione algo 
git add .

# faça um commit
git commit -m "Sua mensagem de commit

# faça um push
git push origin main

# inspecione o hash do commit
# git log --pretty --oneline

# agora edite o arquivo, adicione e faça um -amend
git add .
git commit --amend --no-edit

# inspecione o hash do commit e você verá q ele mudou
# git log --pretty --oneline
```

Quando você usa a opção `--amend` no comando git commit para alterar o commit mais recente do seu repositório Git, você está efetivamente substituindo o commit anterior por um novo commit que contém as alterações e a mensagem de commit atualizadas. Isso significa que o commit anterior não é mais válido e deve ser removido do histórico de commits.

Porém, se você já compartilhou o commit original com outras pessoas (por exemplo, por meio de um push para um repositório remoto), a remoção do commit anterior pode levar a problemas de integridade e sincronização do repositório. Isso ocorre porque outras pessoas podem ter criado novos commits com base no commit original que agora foi removido.

Para resolver esse problema, é necessário usar a opção -f (ou --force) no comando git push ao enviar as alterações para o repositório remoto após usar a opção --amend no commit. A opção -f indica ao Git que as alterações devem ser enviadas forçadamente, mesmo que isso signifique substituir o histórico de commits existente no repositório remoto. Isso garante que o histórico de commits no repositório remoto seja atualizado para refletir as alterações feitas localmente.

No entanto, é importante lembrar que o uso da opção -f no comando git push pode levar a perda de trabalho em outros ramos ou repositórios, por isso deve ser usado com cuidado e somente em situações em que é necessário atualizar o histórico de commits em um repositório remoto. Além disso, é sempre uma boa prática manter uma comunicação clara com a equipe para garantir que todos estejam cientes das mudanças que estão sendo feitas no histórico de commits.