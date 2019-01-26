Posts antigos restaurados

É muito legal ser programador e participar de projetos com as mais variadas dimensões e complexidades, mas uma das coisas que eu acho mais legal em ser um programador é o fato de poder utilizar este conhecimento para automatizar tarefas e resolver problemas do cotidiano.

---

Resolvi escrever este post justamente porque eu automatizei uma tarefa que me tomaria algumas horas com um pequeno e simples script [python](https://www.python.org/).

Eu achei legal escrever um pouco sobre isso porque pode servir de exemplo para pessoas que não trabalham com programação diariamente de que elas podem se beneficiar em conhecer um pouco e tentar otimizar tarefas repetitivas.

### Contextualizando

[No início de 2012 eu implementei um projeto pessoal para poder gerar meu blog com páginas estáticas](https://github.com/adlermedrado-archive/dangolino), estilo Jekyl, Pelican, etc., mas muito mais simples pois foi feito para atender somente as minhas necessidades.

Eu implementei também um pequeno importador para converter meu blog [wordpress](https://wordpress.com/) neste novo formato. Tudo funcionou legal, e usei durante um bom tempo, mas passaram-se uns anos e eu precisei trocar de computador e achei um saco atualizar todas as libs necessárias para o projeto funcionar, então eu voltei para o wordpress.

Quando eu voltei ao Wordpress eu deixei alguns desses posts antigos para trás, não os publiquei novamente e eles ficaram num backup para até um dia em que eu tivesse paciência, publica-los novamente. E então passaram-se alguns anos.

Recentemente eu passei a utilizar o [pelican](https://blog.getpelican.com/), que é um gerador de sites estáticos e ontem eu me lembrei dos posts antigos que estavam nos meus backups e decidi que era a oportunidade de importa-los novamente.

O problemas é que isso me daria muito trabalho, e uma das coisas que programador menos gosta é de ficar fazendo trabalho braçal, então, tentei automatizar o processo.

### A solução

Exisitiam em torno de 200 posts, imagina copiar isso um a um manualmente, editar e salvar no formato do Pelican no diretório correto? *No way*.

A primeira coisa que eu fiz foi analisar se quando eu gerei os arquivos estáticos (HTML), eu o fiz deixando-os padronizados, mas nem todos estavam dentro do mesmo padrão. *Shame on me*, lição aprendida, bola pra frente.

Eu salvei todos os arquivos HTML em um diretório chamado *old* e os posts estavam salvos da seguinte maneira:

    |____old 
    | |____2010 
    | | |____04 
    | | | |____arquivos html

Não coloquei toda a árvore aqui para não ficar muito grande, mas basicamente os arquivos estavam agrupados por **ANO** e **MÊS**, então para processa-los eu precisei iterar no diretório *old* e seus subdiretórios:

    for subdir, dirs, files in os.walk(content_root): 
        for file in files: if 'images' not in subdir: 
            with open(file_path, 'r') as content_file: 
                # converte o arquivo

O diretório *old* tem um subdiretório chamado images e eu não processei o que tinha dentro dele justamente por só conter imagens.

O próximo passo foi fazer o parse do HTML para poder trabalhar com o texto, para isso eu usei a biblioteca BeautifoulSoup:

    def extract_data(): 
        content = content_file.read() 
        soup = BeautifulSoup(content, "html.parser") 
        post_title = soup.html.body.h1.text 
        post_content_data = soup.findAll('div', {'class': 'span12'})[1]
        post_date = post_content_data.find('p').text.replace('Post date:', '').strip() 
        if not post_date: 
            raise ValueError("Post date not found") 
        post_content = post_content_data.text.strip() 
        post_content = re.sub(r'Post date: (d+/d+/d+)', '', post_content) 
        post_content = re.sub(r'- (d+:d+:d+)', '', post_content)
        post_slug = file.replace('.html', '') 

    return { 
            'post_title': replace_invalid_chars(post_title),
            'post_date': post_date, 'post_content':         replace_invalid_chars(post_content), 
            'post_slug': replace_invalid_chars(post_slug), 
    }

Esta função basicamente obtinha as informações do post que eu precisava (titulo, data, slug e conteúdo) e fazia alguns tratamentos nestes dados, porque eu não me lembro o motivo, mas muitos caracteres estavam incorretos.

Eu também removi trechos de texto desnecessários, removi espaços em branco das extremidades e coisas simples assim, essa função retorna um dicionário com as informações necessárias para salvar o novo arquivo.

Na sequência eu uma função que criava o novo arquivo markdown, adicionava nele o conteúdo no formato do Pelican e o salva:

    def convert_to_new_post(post_contents): 
        info_message = '**AVISO:** _Este post é muito antigo e seu conteúdo provavelmente está defasado, '  'permanecendo no meu blog apenas por motivos históricos._nn' 
    new_post_file = f"converted/{post_contents['post_slug']}.md"
    new_post = open(new_post_file, 'w') 
    new_post.write(f"Title: {post_contents['post_title']}n")
    new_post.write(f"Date: {post_contents['post_date']}n")
    new_post.write("Author: adlern") new_post.write("Tags: oldn")
    new_post.write(f"Slug: {post_contents['post_slug']}n")
    new_post.write("Status: publishednn") 
    new_post.write(info_message + post_contents['post_content'])
    new_post.close()

### Nem todos os posts foram importados

No final eu descartei alguns posts pois eles eram contextualmente irrelevantes para o site, e apesar de eu não gostar de jogar história fora, aqueles eram tão bobinhos que sinceramente, não farão muita falta.

Mas eles estão salvos aqui, para caso eu mude de idéia algum dia.

### Código completo e considerações finais

Se eu não tivesse criado este script eu teria que fazer esse processo manualmente em duas centenas de arquivos e certamente não seria algo agradável e eu levei algo em torno de 30 minutos para escreve-lo.

O código completo está [salvo em um Gist](https://gist.github.com/adlermedrado/6159cbf6b3fcca2473c175816f7ff94f) a quem interessar possa.

Tags: python, protip
