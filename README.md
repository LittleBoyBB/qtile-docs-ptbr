# Sumário
 - Instalando Qtile
 	- Guia por distros
 		- Arch
 		- Fedora
 		- Funtoo
 		- Ubuntu
 	- Instalando do código fonte
 		- xcffib
 		- cairocffi
 		- pangocairo
 		- asyncio/trollius
 		- dbus/gobject
 		- Qtile
 - Configuração
 	- Ordem de busca de configuração
 	- Configuração padrão
 		- Key Bindings
 		- Mouse Bindings
 	- Variaveis de configuração
 		- Objetos Lazy
 		- Grupos
 		- Teclas
 		- Layouts
 		- Mouse
 		- Telas
 		- Hooks
 - Teste suas configurações
 - Iniciando Qtile
 - API de comandos
 - Scripting
 - qshell
 - iqshell
 - Contribuindo
 - Hacking em Qtile


# Instalando Qtile
## Guia por distros
Abaixo estão os métodos de instalação preferidos para distribuições específicas. Se você estiver executando algo mais, consulte Instalando do código fonte.
 - Arch
 - Fedora
 - Funtoo
 - Ubuntu

## Instalando do código fonte
Primeiro, você precisa instalar todas as dependências do Qtile (embora algumas sejam opcionais, dependendo da versão do Python, conforme abaixo).

**Observe que as versões 3.3 e mais recentes do Python 3 são atualmente suportadas e testadas.**

### xcffib
Qtile usa xcffib como um binding XCB, que tem suas próprias instruções para instalar do código fonte. No entanto, se você quiser pular a construção dele, você pode instalar suas dependências, você precisará de `libxcb` e `libffi` com os cabeçalhos associados (`libxcb-render0-dev` e `libffi-dev` no Ubuntu), e instalá-lo via PyPI:
```
pip install xcffib
```

### cairocfii
Qtile usa cairocffi com suporte XCB via xcffib. Você precisará do `libcairo2`, a biblioteca subjacente usada pelo binding. Você deve ter certeza antes de instalar o cairocffi que o xcffib foi instalado, caso contrário as ligações cairo-xcb necessárias não serão construídas. Depois de ter as dependências instaladas, você pode usar a versão mais recente no PyPI:
```
pip install cairocffi
```

### pangocairo
Você também precisará do `libpangocairo`, que no Ubuntu pode ser instalado via sudo `apt-get install libpangocairo-1.0-0`. Qtile usa isso para fornecer renderização de texto (e se liga diretamente a ele via cffi com uma pequena in-thee binding).

### asyncio/trollius
Qtile usa o módulo asyncio como introduzido na PEP 3156 para o seu loop de eventos. Com base na sua versão do Python, existem diferentes maneiras de instalá-lo:

 - Python >= 3.4: O módulo asyncio vem como parte da biblioteca padrão, então não há nada mais para instalar.
 - Python 3.3: Tem toda a infra-estrutura necessária para implementar a PEP 3156, mas o módulo asyncio deve ser instalado a partir do projeto Tulip. Para isso, ligue para:

 `pip install asyncio`

 Alternativamente, você pode instalar o trollius (veja o próximo ponto). Note, no entanto, que o trollius está obsoleto, e é recomendável usar tulip, pois o trollius provavelmente será descartado se (e quando) o suporte ao Python 2 for descontinuado.
 - Python 2 (e 3.3 sem asyncio): Você precisará instalar trollius, que backports as funcionalidades do módulo asyncio para trabalhar sem a infra-estrutura introduzida em PEP 3156. Você pode instalar isso de PyPI:
 `pip install trollius`

### dbus/object
Até que alguém chegue e escreva uma biblioteca baseada em async, o qtile dependerá de python-dbus para interagir com dbus. Isso significa que se você quiser usar coisas como o daemon de notificação ou widgets mpris, você precisará instalar `python-gobject` e `python-dbus`. O Qtile funcionará bem sem estes, embora emita um aviso que algumas coisas não estão funcionando.

### Qtile
Com as dependências no lugar, agora você pode instalar qtile:
```
git clone git://github.com/qtile/qtile.git
cd qtile
sudo python setup.py install
```
Versões estáveis do Qtile podem ser instaladas a partir do PyPI:
```
pip install qtile
```
Enquanto as bibliotecas necessárias estiverem no lugar, isso pode ser feito em qualquer ponto, no entanto, é recomendável que você instale primeiro xcffib para garantir que os bindings do cairo-xcb sejam construídos (veja acima).