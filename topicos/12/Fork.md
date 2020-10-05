Usando o fluxo de trabalho Git Fork-and-Branch
27 de janeiro de 2015 · Arquivado em Educação
Agora que forneci uma introdução ao Git e uma breve visão geral do uso do Git com o GitHub , é hora de desenvolver esse conhecimento dando uma olhada mais de perto em um fluxo de trabalho frequentemente usado ao colaborar com o Git. O fluxo de trabalho “fork and branch” é uma forma comum de colaboração em projetos de código aberto usando Git e GitHub . Neste post, vou percorrer esse fluxo de trabalho (como eu o entendo - estou constantemente aprendendo), com o foco em ajudar aqueles que são novos nesse tipo de coisa.

Se você é novo no Git e / ou GitHub e ainda não leu as postagens anteriores no Git e usando o Git com o GitHub, recomendo fortemente que você as leia primeiro.

Basicamente, o fluxo de trabalho "fork and branch" é semelhante a este:

Bifurque um repositório GitHub.
Clone o repositório bifurcado em seu sistema local.
Adicione um remoto Git para o repositório original.
Crie um branch de recurso no qual colocar suas alterações.
Faça suas alterações no novo branch.
Faça commit das alterações no branch.
Envie o branch para o GitHub.
Abra uma solicitação pull do novo branch para o repositório original.
Limpe depois que sua solicitação pull for mesclada.
Aqui estão alguns detalhes sobre cada uma dessas etapas do fluxo de trabalho.

Bifurcando um Repositório GitHub
A primeira etapa é bifurcar o repositório GitHub com o qual você gostaria de trabalhar. Por exemplo, se você estiver interessado em ajudar a contribuir com conteúdo para o site Open vSwitch , que é hospedado como um repositório GitHub , primeiro você deve bifurcá-lo. Bifurcar é basicamente fazer uma cópia do repositório, mas com um link para o original.

Bifurcar um repositório é realmente simples:

Certifique-se de estar conectado ao GitHub com sua conta.
Encontre o repositório GitHub com o qual você gostaria de trabalhar.
Clique no botão Fork no lado superior direito da página do repositório.
É isso - agora você tem uma cópia do repositório original em sua conta GitHub.

Fazendo um Clone Local
Mesmo que você tenha uma cópia do repositório original em sua conta do GitHub, ainda não tem uma maneira de fazer alterações em sua cópia do repositório. Lembre-se dos dois artigos anteriores do Git / GitHub que primeiro você precisa clonar o repositório para o seu sistema local usando git clone. Caso contrário, você não tem como realmente fazer alterações no repositório bifurcado em sua conta do GitHub.

Antes de executar git clone, no entanto, você precisa determinar a URL do repositório bifurcado. Isso é muito simples: basta navegar até o repositório bifurcado (esta é a cópia do repositório original que reside em sua conta GitHub) e olhar no lado direito da página da web. Você deve ver uma área rotulada “URL clone HTTPS”. Simplesmente copie o URL lá e, em seguida, use-o da git cloneseguinte maneira (este é o URL clone para o repositório do site OVS):

git clone https://github.com/openvswitch/openvswitch.github.io.git
Naturalmente, você substituiria o URL depois git clonepelo URL clone HTTPS apropriado para seu repositório de projeto bifurcado. (Você tem que clonar seu repositório bifurcado, não o original.)

Já falei sobre o que acontece com git cloneantes, então só revisarei rapidamente o que acontece quando você clona o repositório bifurcado. O Git irá copiar o repositório, tanto o conteúdo quanto o histórico de commits, para o seu sistema. O Git também adicionará um remoto Git chamado originque aponta de volta para o repositório bifurcado em sua conta GitHub.

Se você estivesse interessado apenas em fazer um fork do projeto e não contribuir de volta para o projeto original, você poderia parar por aqui. Você poderia continuar o desenvolvimento nesta bifurcação do projeto original usando os fluxos de trabalho e comandos que descrevi nos dois artigos anteriores. No entanto, o objetivo real deste artigo é ajudá-lo a contribuir para um projeto, então você precisa continuar.

Adicionando um Remoto
O Git já adicionou um remoto Git nomeado originpara o clone do repositório Git em seu sistema, e isso permitirá que você envie as alterações de volta para o repositório bifurcado em sua conta GitHub usando git commit(para adicionar commits localmente) e git push. Descrevi esse processo no artigo anterior sobre o uso do Git com o GitHub.

Lembre-se de que a forma genérica de git pushé isso git push <remote> <branch>; isso implica que poderia haver outros controles remotos Git além origin. Esse é definitivamente o caso; como mencionei no artigo anterior, você precisará de um controle remoto Git para qualquer repositório para o qual deseja enviar alterações ou do qual deseja enviar alterações.

Para usar o fluxo de trabalho “fork and branch”, você precisará adicionar um remoto Git apontando de volta para o repositório original (aquele que você criou no GitHub). Você pode chamar esse Git remoto do que quiser; a maioria dos documentos e guias que li recomendam o uso do termo “upstream”. Digamos que você bifurcou o repositório do site OVS e, em seguida, o clonou para o seu sistema. Você gostaria de adicionar um controle remoto Git que aponte para o repositório original, como este:

git remote add upstream https://github.com/openvswitch/openvswitch.github.io.git
Obviamente, você deseja substituir o URL depois git remote add upstreampelo URL clone apropriado para o repositório do projeto original. Para o restante deste artigo, assumirei que você usou upstreamcomo nome para esse controle remoto Git adicional.

Agora, você poderia tentar enviar alterações para o repositório original usando git pushneste ponto, mas provavelmente falharia porque você provavelmente não tem permissão para enviar alterações diretamente para o repositório. Além disso, realmente não seria uma boa ideia. Isso porque outras pessoas podem estar trabalhando no projeto também, e como diabos poderíamos acompanhar as mudanças de todos? É aí que entram os ramos.

Trabalhando em uma filial
Até agora, você bifurcou um repositório de projeto, clonou-o em seu sistema local e adicionou um remoto Git ao seu clone que aponta para o repositório de projeto original no GitHub. Agora você está pronto para começar a fazer alterações em seu repositório Git local. Você não pode simplesmente começar a fazer alterações quer queira quer não; para colaborar efetivamente com outras pessoas no mesmo repo, você deve usar um branch .

Há muito mais ramos do Git do que tenho espaço para discutir aqui (e muito mais que ainda tenho que aprender), mas basta dizer por enquanto que esta é uma maneira de criar uma linha separada de mudanças (um “ramo ”) Que é independente da linha principal (muitas vezes referida como“ mestre ”). Existem muitas razões para usar branches em seu desenvolvimento, mas para os propósitos desta discussão sobre o uso do fluxo de trabalho “fork and branch” para colaborar com outros, o objetivo de um branch é ajudar a facilitar que múltiplos usuários façam alterações em um repositório em o mesmo tempo.

Portanto, supondo que seu objetivo seja emitir uma solicitação pull para alterar suas alterações integradas de volta ao projeto original, você precisará usar um branch. Freqüentemente, você verá isso como um branch de recurso , porque normalmente implementará um novo recurso no projeto.

O fluxo básico se parece com isto (tudo isso está acontecendo em seu repositório Git local):

Crie e verifique um branch de recurso .
Faça alterações nos arquivos.
Faça o commit de suas mudanças para o branch.
Devido à forma como o Git funciona, é incrivelmente rápido e fácil para os desenvolvedores criarem vários branches. Se você é relativamente novo no Git (como presumo que seja se estiver lendo isso), provavelmente vai querer ir com calma e não ficar totalmente louco com branches. Conforme estou aumentando minha familiaridade com o Git, estou aderindo a um único branch de recurso por vez.

Para criar um novo branch e verificar (o que significa dizer ao Git que você fará alterações no branch), use este comando:

git checkout -b <new branch name>
Observe que alguns projetos têm requisitos específicos em torno de nomes de branch para solicitações pull, portanto, esteja ciente de tais diretrizes. Uma vez que o novo branch é criado e retirado, então você pode fazer as mudanças necessárias neste branch para implementar o recurso específico ou alteração que você deseja que o projeto original seja incorporado ao seu repositório. Como regra geral, você deve limitar uma ramificação a uma mudança lógica. A definição de uma mudança lógica irá variar de projeto para projeto e de desenvolvedor para desenvolvedor, mas a ideia básica é que você só deve fazer as mudanças necessárias para implementar um recurso ou aprimoramento específico.

Conforme você faz mudanças nos arquivos no branch, você desejará confirmar essas mudanças, construindo seu changeset com git adde confirmando as mudanças usando git commit. Também esteja ciente de que alguns projetos não querem um monte de commits em uma solicitação pull, então você pode precisar usar git rebasepara “esmagar o histórico de commits ”. Para manter este artigo gerenciável, não vou tentar cobrir isso aqui, mas existem vários guias sobre como isso funciona ( aqui está um guia sobre como usar git rebasepara esmagar commits).

Empurrando mudanças para GitHub
Então, digamos que você tenha feito as alterações necessárias para implementar o recurso ou aprimoramento específico (a “alteração lógica”) e que tenha enviado as alterações para seu repositório local. A próxima etapa é enviar essas alterações de volta ao GitHub.

Se você estivesse trabalhando em um branch chamado new-feature, enviar as alterações feitas nesse branch de volta para o GitHub ficaria assim:

git push origin new-feature
Novamente, lembre-se de que a forma genérica desse comando é git push <remote> <branch>. Nesse caso, você está enviando alterações no new-featurebranch para o originremoto.

Abrindo uma solicitação pull
O GitHub torna essa parte incrivelmente fácil. Depois de enviar um novo branch para o seu repositório, o GitHub solicitará que você crie uma solicitação pull (presumo que você esteja usando o navegador e não os aplicativos nativos do GitHub). Os mantenedores do projeto original podem usar esta solicitação pull para enviar suas alterações para seu repositório e, se aprovarem as alterações, mesclá-las no repositório principal.

Esteja ciente de que, para ajudar a manter as coisas gerenciáveis, alguns projetos de código aberto podem ter diretrizes sobre como as solicitações pull são enviadas. Mencionei anteriormente que pode ser necessário usar um determinado nome para o seu branch de recurso ou talvez as solicitações do projeto para que você crie um problema (outro recurso do GitHub) antes de enviar a solicitação pull (e depois incluir o número do problema na solicitação pull). Basta verificar com os mantenedores do (s) projeto (s) específico (s) para o qual você gostaria de contribuir para ver se esses detalhes existem.

Limpando após uma solicitação de pull mesclada
Se os mantenedores aceitarem suas alterações e mesclá-las no repositório principal, haverá um pouco de limpeza para você fazer. Primeiro, você deve atualizar seu clone local usando git pull upstream master. Isso puxa as alterações do upstreambranch master do repositório original (indicado por ) (indicado por masternaquele comando) para seu repositório clonado local. Um dos commits no histórico de commits será o commit que mesclou seu branch de recursos, então depois de você git pullo branch master do seu repositório local terá as alterações do branch de recursos confirmadas. Isso significa que você pode excluir o branch de recurso (porque as alterações já estão no branch master):

git branch -d <branch name>
Então você pode atualizar o branch master em seu repositório bifurcado:

git push origin master
E envie a exclusão do branch de recurso para o repositório GitHub ( atualização: uma versão anterior deste artigo listada git push -dabaixo):

git push --delete origin <branch name>
E é isso! Você acabou de criar com sucesso um branch de recurso, fez algumas alterações, confirmou essas alterações em seu repositório, enviou-as para o GitHub, abriu uma solicitação de pull, teve suas alterações mescladas pelos mantenedores e depois limpas. Muito legal, hein?

Mantendo seu garfo sincronizado
A propósito, seu repositório bifurcado não permanece sincronizado automaticamente com o repositório original; você precisa cuidar disso sozinho. Afinal, em um projeto de código aberto saudável, vários contribuidores estão bifurcando o repositório, clonando-o, criando branches de recursos, confirmando alterações e enviando solicitações pull.

Para manter sua bifurcação sincronizada com o repositório original, use estes comandos:

git pull upstream master
git push origin master
Isso puxa as alterações do repositório original (aquele apontado pelo upstreamremoto Git) e as envia para seu repositório bifurcado (aquele apontado pelo originremoto). (Esperançosamente, isso faz algum sentido para você agora.)
