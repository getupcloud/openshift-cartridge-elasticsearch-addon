OpenShift Elasticsearch Cartridge
=================================
Este cartucho disponibiliza um cluster Elasticsearch para sua aplicação.

Para criar sua aplicação Elasticsearch, primeiro você precisa registrar-se na Getup.
Acesse http://getupcloud.com/#/sign-up e faça seu cadastro.
Você recebe gratuitamente 750hs para testar a plataforma.

Para adicionar Elasticsearch a sua aplicação, execute no terminal:

    rhc cartridge-add http://reflector-getupcloud.getup.io/github/getupcloud/openshift-elasticsearch-addon-cartridge -a <app>

Adicionando nodes ao cluster
============================
Antes de adicionar nodes ao cluster, você precisa exportar a variável OPENSHIFT_ELSTICSEARCH_MASTER com os dados de conexão do primeiro gear.

Descubra a URL SSH do primeiro gear Elasticsearch e entre nele:

    $ rhc app-show <app> --gears
    $ ssh <ELASTICSEARCH-GEAR-SSH-URL>

Descubra e anote o valor das seguintes variáveis:

    > env | grep OPENSHIFT_GEAR_DNS
    > env | grep OPENSHIFT_ELASTICSEARCH_NODE_PROXY_PORT
    > exit

Exporte os dados de conexao para todo o app:

    $ rhc env-set -a <app> OPENSHIFT_ELASTICSEARCH_MASTER=$GEAR_DNS:$NODE_PROXY_PORT
                                                          ^^^^^^^^^ ^^^^^^^^^^^^^^^^

Agora estamos prontos para adicionar novos nodes:

    $ rhc cartridge-scale getup-elasticsearch 2 -a <app>

Plugins
=======
Para instalar plugins você precisa entrar no gear de ES e executar o comando `elasticsearch/bin/plugin` como de costume:

    $ rhc app-show <app> --gears
    $ ssh <ELASTICSEARCH-GEAR-SSH-URL>
    > elasticsearch/bin/plugin --install mobz/elasticsearch-head

Configuração
============
As configurações são construidas durante o start, a partir do arquivo `elasticsearch/config/elasticsearch.yml.erb`.

Licença
=======
Este projeto é licenciado sob [Apache License 2.0](http://www.apache.org/licenses/LICENSE-2.0.html).
