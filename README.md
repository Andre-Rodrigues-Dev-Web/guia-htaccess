# guia-htaccess
Guia definitivo para usar htaccess

## Flags e Símbolos do Mod Rewrite
Os flags modificam as regras do rewrite (RewriteRule). Confira abaixo quatro exemplos que ajudarão você a compreender os códigos que usaremos:

[F]: Significa Forbidden, fazem o servidor retornar um erro 403 Forbidden;
[L]: Significa Last, significam que se aquela condição for válida as condições abaixo não serão processadas;
[R]: Significa Redirect, é responsável por redirecionar o site para a URL especificada;
[NC]: Significa no case-sensitive, ou seja, meusite.com é igual a MEUSITE.COM.
Além dos flags, temos os símbolos:

%{HTTP_HOST} significa a url digitada;
^ significa início da string;
^(.*)$ significa quaisquer caracteres. Este símbolo irá substituir o valor por outro, na hora que for recebida a requisição. Exemplo: meusite.com/$1 poderá ser meusite.com/qualquer-pagina/.
Redirecionamento via htaccess
Agora que você já sabe o que é o arquivo do Apache, seus flags e símbolos, está na hora de começar. Vamos ver abaixo os exemplos de redirecionamento via htaccess:

Redirecionamento de http para https
O comando abaixo irá redirecionar todo o acesso do endereço http://meusite.com/ para https://meusite.com

# Redireciona de http para https
<IfModule mod_rewrite.c>
RewriteEngine on
RewriteCond %{HTTP:X-Forwarded-Proto} !https
RewriteRule ^(.*)$ https://%{HTTP_HOST}/$1 [L,R=301]
</IfModule>
Obs.: É necessário ter um certificado SSL válido e instalado no domínio para que o redirecionamento funcione corretamente. Recomendo desativar plugins de redirecionamento de SSL no WordPress. Pode causar loops e tirar o site do ar.

Redirecionamento de URL com ou sem o WWW
Abaixo, você terá dois códigos, use apenas 1, para redirecionar o seu domínio e garantir que todos possam acessar, usando o www. ou sem.

Redirecionamento sem WWW para com WWW
O comando a seguir irá redirecionar o seu endereço meusite.com para www.meusite.com

# Redireciona domínio sem o www para endereço com o www
<IfModule mod_rewrite.c>
RewriteEngine on
RewriteCond %{HTTP_HOST} ^seusite.com [NC]
RewriteRule ^(.*)$ http://www.meusite.com/$1 [L,R=301]
</IfModule>
Redirecionamento com WWW para sem WWW
O comando abaixo irá redirecionar o seu endereço www.meusite.com para meusite.com

# Redireciona domínio com o www para endereço sem o www
<IfModule mod_rewrite.c>
RewriteEngine on
RewriteCond %{HTTP_HOST} ^www.meusite.com [NC]
RewriteRule ^(.*)$ http://meusite.com/$1 [L,R=301]
</IfModule>
Redirecionamento de Domínio para Subdomínio
O código abaixo redireciona o seu domínio para um subdomínio. Exemplo: meusite.com para subdominio.meusite.com.

# Redireciona Domínio para um Subdomínio
<IfModule mod_rewrite.c>
RewriteEngine on
RewriteCond %{HTTP_HOST} ^meusite.com [NC]
RewriteRule ^(.*)$ http://subdominio.meusite.com/$1 [L,R=301]
</IfModule>
O código abaixo redireciona o seu Subdomínio para um Domínio. Exemplo: blog.meusite.com para meusite.com.

Redirecionamento de Subdomínio para Domínio

# Redireciona Subdomínio para um Domínio
<IfModule mod_rewrite.c>
RewriteEngine on
RewriteCond %{HTTP_HOST} ^subdominio.meusite.com [NC]
RewriteRule ^(.*)$ http://meusite.com/$1 [L,R=301]
</IfModule>
Redirecionar domínio para outro
O comando abaixo faz o redirecionamento de um domínio para outro. Este é o código que você deve usar quando quer passar a autoridade do domínio antigo para o novo.

# Redireciona domínio antigo para novo domínio
<IfModule mod_rewrite.c>
RewriteEngine on
RewriteCond %{HTTP_HOST} ^meuantigosite.com [NC]
RewriteRule ^(.*)$ http://meunovosite.com/$1 [L,R=301]
</IfModule>
Observações sobre os redirecionamentos
Você teve acesso a diversos códigos para fazer o redirecionamento via htaccess, mas antes de sair por ai copiando e colando os códigos, peço que siga essas dicas e observações:

Substitua o nome meusite.com pelo endereço real do seu domínio e depois insira o código no .htaccess;
Respeite o uso de maiúsculas e minúsculas no código;
Sempre que fazer um redirecionamento, limpe completamente o cache do seu navegador para fazer testes. Em alguns casos o redirecionamento está correto, mas não funciona devido ao histórico do browser.
Use o código 301 quando você não pretende mudar o redirecionamento e o 302 quando você planeja desfazer o redirecionamento em breve.
Verifique os códigos que você já possui no .htaccess antes de inserir novos. Pode ser que o código já exista.
Não faça redirecionamentos conflitantes. Exemplo: Redirecionar um Domínio para Subdomínio e depois querer devolver a visita par ao meu endereço. O navegador irá acusar loop de redirecionamento e o site sairá do ar.
