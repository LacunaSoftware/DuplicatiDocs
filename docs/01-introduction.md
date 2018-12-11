# Introdução

## Sobre este manual

Este manual pode ser usado como um guia para que melhor entenda as capacidades do Módulo Agente do Backup e-notariado, mas também pode ser usado como um guia de referência.

Ao longo dele, a aplicação que é o Módulo Agente será referenciada apenas pela palavra "Aplicação" ou "Agente".

Algumas partes estão marcadas com ícones. Informações muito importantes são marcadas com um pequeno triângulo contendo um ponto de exclamação, Exemplo:

*****
> ![](icon_important.png) Isto é uma informação importante

*****

Informações extras sobre um assunto são marcadas com um pequeno círculo contendo a letra 'I'. Exemplo:

*****
> ![](icon_info.png) Esta é uma informação extra.

*****

Este manual a princípio cobre instalações feitas em sistemas Windows, uma vez que a instalação em outros sistemas operacionais ainda não foi implementada.

## Visão Geral

Esta aplicação é um cliente de backups que armazena de forma segura backups criptografados, incrementais e comprimidos em armazenamento remoto na nuvem. 

Esta aplicação **não** é:

* **Um programa de sincronização de arquivos.**  
A aplicação é uma solução de backups baseados em blocos. Os arquivos são separados em pequenos pedaços de dados (blocos), que são então criptografados e comprimidos antes de serem enviados para a nuvem. Na nuvem, a aplicação não envia os arquivos originais mas sim arquivos que contêm os blocos dos arquivos originais e outros dados necessários que permitem que a aplicação restaure os arquivos aos seus estados originais, no processo de restauração. Este sistema de backups baseados em blocos permite funcionalidades como versionamento de arquivos e deduplicação. 
* **Um software para fazer uma imagem do disco rígido.** 
Esta aplicação pode fazer backups de arquivos e pastas selecionados. Um software para imagens de discos rígidos podem criar um arquivo de imagem de um volume ou disco rígido completo, incluindo o setor de boot. Se quiser restaurar um volume ou disco rígido completo, incluindo o setor de boot e o sistema operacional, precisa de outra aplicação. 
* **Um software de backup para arquivos armazenados na nuvem.**  
Esta aplicação não pode fazer backup de arquivos armazenados remotamente. Ela precisa ser instalada na máquina onde os arquivos de origem para o backup estão localizados. Opcionalmente, arquivos e pastas localizados na rede local podem ser selecionados para o backup usando caminhos UNC.

## Funcionalidades

* **Criptografia segura** 
A aplicação usa a forte criptografia AES-256 para proteger seus backups. Foi planejado seguindo o princípio TNO: Trust No One (Não Confie Ninguém). Por exemplo, todos os dados são criptografados localmente antes de serem transferidos para o sistema de armazenamento remoto. A senha/chave usada no seu backup nunca sai do seu computador. 
* **Backups incrementais**  
A aplicação realiza inicialmente um backup completo. Uma vez que isto é feito, ela atualiza o backup inicial ao só adicionar dados modificados. Isto é, se apenas pequenas partes de um arquivo grande mudaram, apenas essas pequenas partes são adicionadas ao backup. Isto economiza tempo e espaço e o tamanho do backup cresce normalmente devagar.
* **Compactação**  
Todos os dados do backup são compactados antes de serem criptografados e enviados. Para obter uma melhor performance, ela também detecta arquivos que já estão compactados e os adiciona como já estão aos arquivos Zip. Por exemplo, arquivos de mídia como mp3, jpeg ou mkv já estão bastante comprimidos.
* **Verificação online de backups**
Esta aplicação baixa regularmente conjuntos aleatórios de arquivos do backup, restaura seu conteúdo e confere a integridade dos arquivos. Deste modo é possível detectar problemas com seu armazenamento remoto antes de precisar dos dados e encontrar problemas.  
* **Deduplicação**
A aplicação analisa o conteúdo dos arquivos e armazena blocos de dados. Por isso, ela irá detectar arquivos duplicados e conteúdos similares, agindo de modo que os armazene apenas uma vez no backup. Conforme a aplicação analisa o conteúdo dos arquivos ela lida muito bem com arquivos ou pastas que foram movidos ou renomeados. Como o conteúdo não muda, o próximo backup será diminuto.  
* **Proteção contra falhas**
Esta aplicação foi desenvolvida para lidar com vários tipos de problemas: Problemas de rede, backups interrompidos, sistemas de armazenamento corruptos ou indisponíveis. Mesmo se a execução de um backup for interrompida, ela pode ser retomada depois. A aplicação irá então realizar o backup de tudo que faltou na última execução, e mesmo se arquivos remotos forem corrompidos, ela irá tentar repará-los se os dados locais ainda estiverem presentes ou então restaurar o máximo que for possível.  
* **Interface Web**
A aplicação possui uma interface web. Ela pode ser usada para configurar e executar backups na sua máquina local. Também permite que configure e execute backups remotamente em máquinas servidores como NAS (Network Attached Storage). Apenas instale a aplicação no seu NAS e configure a execução pela interface web.  
* **Interface de linha de comando**
Não esquecemos dos administradores de sistemas! A aplicação oferece todas suas funcionalidades pelo programa Duplicati.CommandLine.exe. Isto permite que você execute backups por meio de scripts ou por um terminal.  
* **Metadados**
A aplicação também armazena metadados de arquivos no backup. Quando arquivos do backup são restaurados, os horários de última modificação e criação também serão restaurados assim como as permissões de acesso do sistema. Para evitar arquivos inacessíveis, quando o sitema do usuário mudou por exemplo, a restauração das permissões é opcional.  
* **Agendador**
O agendador integrado executa seus backups automaticamente nos tempos e intervalos definidos. Um backup por dia, nos finais de semana, por hora ou até 15:00 toda terceira segunda, todos são possíveis. Mesmo que um dia seja perdido, caso a máquina esteja desligada por exemplo, a aplicação irá executar o que faltou assim que for possível.  
* **Atualizações automáticas**
A aplicação possui um atualizador integrado que permite baixar e instalar a última versão disponível para você. Deste modo você pode facilmente mantê-la atualizada.  
* **Realize o backup de arquivos abertos**
Quando um arquivo está em uso por um processo ou aplicação, ele não pode normalmente ser lido por outro processo, tornando impossível fazer o backup desse arquivo. Em sistemas Windows, a aplicação pode usar VSS (Volume Shadow Copy Services) para criar um snapshot consistente de um volume, que é então usado pela aplicação para realizar um backup confiável destes arquivos abertos.

## Licença

A aplicação está licenciada sob a LGPL e disponível para Windows. Mais informações sobre o modelo de licenciamento LGPL podem ser encontradas em [Contrato de Licenciamento](appendix-f-license-agreement).

## Requisitos de Sistema

A aplicação deve ser instalada em um dispositivo com um sistema operacional suportado. Atualmente, estes são suportados:

* Windows Vista e superiores (ambas versões 32 e 64 bit)
* Windows Server 2008 e superiores (ambas versões 32 e 64 bit)

Dispositivos Windows devem ter .NET Framework 3.5 ou acima instalados.

A aplicação pode realizar backups de arquivos que estão abertos por outros processos, no Windows isto é realizado por meio do VSS. Para criar um snapshot VSS, é necessário instalar componentes C++ Run-time que podem ser encontrados [clicando aqui](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads).


A aplicação também necessita permissões de administrador na máquina para ser instalada.

Ela também precisa de aproximadamente 40 MB de espaço livre em disco para sua instalação. No entando, espaço adicional é necessário para a execucção:

* Ela cria um pequeno banco de dados que contém configurações do programa e todas as configurações de backups.
* Para cada configuração de backup, um banco de dados local é criado, permitindo que a aplicação recupere informações sobre arquivos na localização remota sem realmente carregar ou baixar arquivos. Este banco de dados torna a aplicação muito mais rápida uma vez que consultas são feitas localmente sem necessitar conexões remotas. O tamanho deste banco de dados local varia, dependendo no número de arquivos de origem selecionados para o backup, o tamanho total de dados e o tamanho escolhido para os blocos. Na maioria dos casos, um banco de dados local consume entre 10MB e alguns GBs de armazenamento loca.
* A aplicação cria arquivos temporários enquanto realiza backups ou restaurações. O tamanho de armazenamento necessário depende do tamanho de carregamento de volumes (DBLOCK) escolhido. O tamanho padrão é 50 MB, mas este valor pode ser modificado em cada configuração de backup. Um pequeno número (de 1 a 5) de arquivos DBLOCK temporários são armazenados localmente antes de serem carregados na nuvem. Depois de um carregamento bem-sucedido, estes arquivos temporários serão apagados automaticamente.

## Explicando o processo de backup

Softwares tradicionais de backup fazem backups completos em intervalos regulares (uma vez por semana por exemplo). Todos os outros backups são incrementais. Estes backups incrementais enviam todos os arquivos adicionados e modificados para o local de destino. A desvantagem é que se uma pasta precisar ser recuperada do backup mais recente, o último backup completo precisa ser recuperado antes, seguido por todos os backups incrementais que foram feitos depois do último backup completo. Este procedimento é complexo e tende a erros.

Fazer um backup completo todo dia resulta em backups confiáveis, mas consume muito tempo e muitos recursos. Todos os dados de origem precisam ser enviados e armazenados no local de destino toda vez que o backup é executado.

Esta aplicação combina o melhor dos dois mundos. Quando um backup é executado, apenas as partes alteradas de arquivos são enviadas para o destino. Deste ponto de vista, ela se comporta como se estivesse fazendo um backup incremental. Quando um ou mais arquivos (ou todos os arquivos e pastas) precisam ser recuperados do backup mais recente, este backup (e todos os outos) lembram um backup completo: todos os dados podem ser restaurados com uma única operação, sem precisar refazer uma série de backups incrementais.

Ela usa um novo formato de armazenamento para backups, baseado em blocos. Isto é, ela não armazena os arquivos, os divide em vários pequenos blocos. Uma simples explicação:

> Imagine que seus arquivos locais consistem em vários tijolos pequenos de diferentes formas e cores. A aplicação pega estes arquivos, os divide em tijolos únicos e os guarda em pequenos sacos. Sempre que um saco estiver cheio, é armazenado em uma grande caixa (que é seu armazenamento em nuvem). Quando algo muda, a aplicação coloca os novos blocos em um novo saco e os coloca na caixa. Quando um arquivo local precisa ser restaurado, a aplicação sabe quais tijolos ela precisa e em quais caixas eles estão. Então, ela pega os sacos que precisa, retira os tijolos e reconstroi seu arquivo. Se o arquivo ainda estiver no seu computador (em uma versão que você não quer mais), ela pode simplesmente trocar os tijolos incorretos, atualizando então o arquivo existente.
>
> De tempos em tempos, a aplicação vai notar que existem sacos possuindo tijolos que não sai mais necessários. Ela pega esses sacos, organiza os tijolos, joga fora os que não são mais necessários e então coloca os outros em novos sacos e então de volta na caixa. A aplicação também irá detectar se existe um grande número de sacos que possuem um número muito pequeno de tijolos. Ela pega todos esses sacos, retira os tijolos, os coloca em novos sacos (mais cheios) e então de volta na caixa.
>
> E repetindo: Não existe a necessidade de carregar backups completos regularmente. Isto faz com que a aplicação seja a escolha perfeita para backups incrementais de grandes bibliotecas de mídia.
