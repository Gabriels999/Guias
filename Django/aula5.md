# Parte 5

### Variáveis de ambiente:

- É interessante passar a senha e login do através de variáveis de ambiente, isso porquê com esses dados hardcoded no código fica muito fácil visualizá-los.
- Faremos isso usando o `import os` e com o `os.getenv("teste")` ele retorna as variáveis de ambiente que passarmos como parâmetros da função.
- Em seu arquivo _settings.py_ inclua o código:

```
SECRET_KEY = os.getenv('SECRET_KEY', 'adicione-um-segredo-aqui')
```

- **Explicação**

  - O primeiro parâmetro de getenv é o nome da variável global e a segunda é o valor que será recebido, caso a variável citada esteja vazia.
  - Para não fazer isso é possível também exportar o valor da variável `export SECRET_KEY=valor_da_variavel` que é um comando que cria uma variável de ambiente naquele terminal em questão.
  - Outra forma de gerar é o comando:

  ```
  python -c 'from django.core.management.utils import get_random_secret_key; \
            print(get_random_secret_key())'

  ```

  - Quando estamos falando de rodar um projeto localmente, é uma boa prática criar um arquivo chamado **.env** e dentro dele inserir todas as variáveis de ambiente a serem acessados pelo Django. Para que o Django consiga ler este arquivo é necessário baixar uma biblioteca chamada _dotenv_ que pode ser instalada com o comando: `pip install python-dotenv`

  - Para finalizar a configuração, insira no _settings.py_:

  ```
  from dotenv import load_dotenv

  load_dotenv()
  ```

  - Em seguida verifique em um novo terminal usando `./manage.py shell` para abrir um terminal e buscar o valor estabelecido lá no arquivo _.env_. Caso ele retorne os valores, a biblioteca está funcionando, caso contrário refaça os passos.
  - Não se esqueça de incluir o _.env_ no seu _.gitignore_ caso ele já não esteja lá.
  - Caso estejamos trabalhando com docker a ideia é um pouco diferente

### Aplicações Plugáveis:

- É possível utilizar apps criadas por outras pessoas dentro de nosso projeto Django. Isso faz com que se economize tempo tanto evitando a construção de algo já pronto quanto sem ter que dar o mesmo grau de manutenção como se tem que fazer em uma app criada por nós desde o começo.
- O Django já vem com algumas instaladas por padrão em nosso projeto. Elas (e as outras que serão acrescidas) podem ser visualizadas no arquivo _settings.py_ na parte de `INSTALLED_APPS=[]`

### O uso do banco de dados pelo Django:

- usando o `./manage.py showmigrations` mostra todas as tabelas existentes em todas as apps instaladas naquele projeto.
- `./manage.py migrate` é o comando usado para criar todas as tabelas que são referentes às apps já instaladas no seu projeto.
  - Caso queira conferir a criação destas tabelas é possível rodar o `./manage.py showmigrations` novamente e as tabelas aparecerão com o [X]
- Crie um usuário usando o comando `./manage.py createsuperuser` e preencha o login e a senha.
- Tente criar um usuário pela interface do admin. Este usuário criado por padrão não é um superuser e portanto não consegue logar no admin. E somente os superusers conseguem mudar o status de outros usuários.
- **O que são migrations ?**
  - São "versionamentos" do banco de dados. Cada alteração feita na estrutura do banco de dados será inserida em uma migration e isso vai criar um código SQL para fazer a transformação dessa estrutura.

### Mostrar o usuário logado:

- Em seu html adicione dentro do block você pode acessar as informações do usuário através da palavra _user_
- Ex.:

  - ```
    {{ user }}
    {{ user.first_name }}
    {{ cursos }}
    ```

- Adicionr a app Django Debug Toolbar no seu projeto através do `pip install django-debug-toolbar` e finalize a configuração acompanhando os passos da <a href='https://django-debug-toolbar.readthedocs.io/en/latest/'>documentação</a>.
- Para subir o projeto para produção sem incluir o que queremos de desenvolvimento (como por exemplo o debug-toolbar) é necessário em seu arquivo _settings.py_ insira o código:
- ```
  DEBUG = os.getenv("DEBUG", "False") == True
  ```
- E também acrescentar isso no seu arquivo _urls.py_ para diferenciar se a variável de ambiente DEBUG está True ou não. Isso é feito com esse código:

```
if settings.DEBUG == True:
	urlpatterns = [ path(‘__debug__/’, include(‘debug-toolbar.urls’)) ]
```

- Para conectar o banco postgresql com o projeto Django é necessário instalar a biblioteca _psycopg2_ para que e o próprio Django já deve reconhecer a biblioteca e rodar o projeto sem mostrar erros.
- Para conferir isso basta apenas ver esta conexão através do seu cliente de preferência (Dbeaver, extensões do Vscode, etc).
