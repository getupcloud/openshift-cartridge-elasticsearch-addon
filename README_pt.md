OpenShift Elasticsearch Cartridge
=================================
Este cartucho disponibiliza um cluster Elasticsearch embarcado para sua aplicação.

Para criar sua aplicação Elasticsearch, primeiro você precisa registrar-se na Getup.
Acesse http://getupcloud.com/#/sign-up e faça seu cadastro.
Você recebe gratuitamente 750hs para testar a plataforma.

Para adicionar Elasticsearch a sua aplicação, execute no terminal:

    rhc cartridge-add --app <app> http://reflector-getupcloud.getup.io/github/getupcloud/openshift-cartridge-elasticsearch-addon

Para criar uma app escalável inclua a opção `--scaling` no comando acima.

Adicionando nodes ao cluster
============================
Antes de adicionar nodes ao cluster, você precisa exportar a variável OPENSHIFT_ELASTICSEARCH_MASTER com os dados de conexão do primeiro gear.

Descubra a URL SSH do primeiro gear Elasticsearch e entre nele:

    $ rhc app-show <app> --gears
    $ ssh <ELASTICSEARCH-GEAR-SSH-URL>

Descubra e anote o valor das seguintes variáveis:

    > env | grep OPENSHIFT_GEAR_DNS
    > env | grep OPENSHIFT_ELASTICSEARCH_NODE_PROXY_PORT
    > exit

* Se nada aparecer no segundo comando talvez sua app não seja escalável. Por favor veja a primeira seção.

Exporte os dados de conexao para todo o app:

    $ rhc env-set --app <app> OPENSHIFT_ELASTICSEARCH_MASTER=$GEAR_DNS:$NODE_PROXY_PORT
                                                             ^^^^^^^^^ ^^^^^^^^^^^^^^^^

Agora estamos prontos para adicionar novos nodes:

    $ rhc cartridge-scale --app <app> getup-elasticsearch-addon 2

Plugins
=======
Para instalar plugins você precisa entrar no gear de ES e executar o comando `elasticsearch-addon/bin/plugin` como de costume:

    $ rhc app-show <app> --gears
    $ ssh <ELASTICSEARCH-GEAR-SSH-URL> elasticsearch-addon/bin/plugin --install mobz/elasticsearch-head

Configuração
============
As configurações são construidas durante o start, a partir do arquivo `elasticsearch/config/elasticsearch.yml.erb`.

Créditos
========
Baseado no trabalho inicial de https://github.com/ncdc/openshift-elasticsearch-cartridge.

Licença
=======
Este projeto é licenciado sob [Apache License 2.0](http://www.apache.org/licenses/LICENSE-2.0.html).
