Configurando 2 contas de Git no mesmo computador
Aprenda como configurar uma conta pessoal e uma conta da empresa no Git, incluindo chaves SSH e emails.

Configurar o Git da minha máquina para não usar somente o meu email pessoal, mas também o da empresa.

Lembrando que os processos descritos abaixo funcionam no Linux, quando nós criamos um commit, ao ver no log, ele fica numa estrutura similar a essa:

commit 82a24142b6df8049787760c82d29728f8fb0eee5 (HEAD -> master, origin/master, origin/HEAD)
Author: Pablo Francisco Moura <pablo.f.moura17@gmail.com>
Date:   Sun Sep 5 23:31:04 2022 -0300

Tem uma parte separada especificamente para o Author. Se você não configurar, o Git vai pegar o que tiver de informação no computador (.gitconfig) e na maioria das vezes ele não vai colocar direito nem o nome e muito menos o email.

Para configurar uma conta única e global é super simples, basta digitar:

git config user.name --global "Pablo Francisco Moura"
git config user.email --global "pablo.f.moura17@gmail.com"
Mas e se precisar mudar para o email da empresa no projeto X? A forma não automatizada é dentro do projeto definir manualmente:

git config user.name "Pablo Francisco Moura"
git config user.email "pablo.moura@empresax.com.br"

Mas e se tiverem vários projetos da empresa, você precisa definir manualmente. Mas podemos automatizar as coisas.

Configurando diferentes emails:
Quando o Git vai assinar um commit ele primeiro procura as informações dentro da pasta .git dentro do projeto e caso não tenha nada definido localmente, ele procura pela configuração global que fica num arquivo chamado .gitconfig na pasta raiz do usuário.

E é aqui que iremos configurar o Git para ler o que nós queremos que ele leia.

~/Development
   /github # meus projetos pessoais
   /empresax # projetos da empresax

Se estiver na pasta github use meu email pessoal
Se estiver na pasta empresax use o email da empresa
E aí para definir isso, eu criaremos 2 arquivos na raiz do diretŕio (~$):

.gitconfig-github
.gitconfig-empresax
E os conteúdos ficam:

# .gitconfig-github

[user]
  email = pablo.f.moura17@gmail.com
E no outro:

# .gitconfig-zapt

[user]
  email = pablo.moura@empresax.com.br

Lembrando que aqui estou mostrando só o email, mas outras configurações como aliases específicos para cada conta e qualquer outra coisa, você consegue separar nesses arquivos.

Mas só fazendo isso, ainda não vai funcionar, nós precisamos informar ao Git quando é parar ler um ou outro, para isso nós editaremos o .gitconfig original para ficar assim:

[user]
  name = Pabblo Francisco Moura

[includeIf "gitdir:~/Development/github/"]
  path = .gitconfig-github
[includeIf "gitdir:~/Development/empresax/"]
  path = .gitconfig-empresax

Note que eu tenho meu nome global (user.name) quando criar. Utilizo um includeIf exatamente para quando cair numa opção ou outra, ele adicionar meus dados específicos como email (user.email) que definimos nos diretórios ~/Development/github ou ~/Development/empresax. Se eu criar um commit na EmpresaX, já ficaria com o commit assim:

commit e07164f972a15bbf7c681970a5cf547db966867d (origin/fix/update-map-use-lat-long, fix/update-map-use-lat-long)
Author: Pablo Francisco Moura <pablo.moura@empresax.com.br>
Date:   Thu Aug 26 12:16:44 2022 -0300

Como podem ver, o email já fica certinho o da empresa e não o meu pessoal.

Configurando duas chaves SSH:
Agora vamos a segunda parte que é chaves ssh.

Você precisa criar chaves SSH separadas, é só rodar:

ssh-keygen -t rsa -C "email@pessoal.com" -f "id_rsa_pessoal"
ssh-keygen -t rsa -C "email@trabalho.com" -f "id_rsa_trabalho"
Ao fazer isso, ele vai criar duas chaves separadas na pasta ~/.ssh, depois basta adicionar na sua máquina usando:

ssh-add ~/.ssh/id_rsa_pessoal
ssh-add ~/.ssh/id_rsa_trabalho
Depois disso precisamos configurar o ssh para também entender quando usar uma chave ou outra. Para isso vamos criar um arquivo config dentro da pasta .ssh:

cd ~/.ssh
touch config
code config # você pode usar vi, vim, nano, seu editor favorito.
Lá dentro do arquivo é só editar:

# Conta pessoal como default
Host github.com
   HostName github.com
   User git
   IdentityFile ~/.ssh/id_rsa_pessoal
   
# Conta do trabalho
Host github.com-trabalho  
   HostName github.com
   User git
   IdentityFile ~/.ssh/id_rsa_trabalho
Ali está como github, mas para o gitlab é só mudar no Host e Hostname, você inclusive pode ter diferentes configs de diferentes repositórios remotos no mesmo arquivo.

E aí, sempre que for clonar um repositório, se ele for de trabalho, basta editar na url para ficar de acordo com a estrutura acima:

git clone git@github.com-trabalho:seu_user/repo_name.git

Pronto, agora sua máquina está configurada para trabalhar com duas contas do Git e assim você não fica misturando usuários e chaves entre coisas pessoais e empresa!