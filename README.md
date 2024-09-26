<h1>Instalação e Configuração Dashy Board em Raspberry Pi.</h1>

<p>Tudo bom pessoal, sou apaixonado por tecnología e informatica. Por isso sempre estou estudando e pesquisando o que posso usar no meu dia a dia. Seja como usuário comum ou como devops.</p>

<p>Como eu sou um usuário curioso, sempre estou achando coisas interessante para me ajudar a monitorar os aplicativos a minha rede em casa.</p>

<br>
<br>

<p>Este repositório documenta a Configuração do Dashy. O objetivo é fornecer um guia passo a passo para configurar Dashy funcional e acessível a partir de outros dispositivos na mesma rede.</p>

<p align="center">
  <img src="https://github.com/alexsandrofabbro/Dashy---Raspberry-Pi/blob/main/Dashy.gif" alt="Dashy Logo" width="800x600">
</p>

<p align="center">
  <strong>Dashy</strong> é um dashboard altamente personalizável, que permite organizar e gerenciar links, serviços e aplicações a partir de uma interface simples e intuitiva.
</p>
<br>
<br>

<h2>Índice</h2>
<ul>
    <li><a href="#pré-requisitos">Pré-requisitos</a></li>
    <li><a href="#instalacao-com=docker">Instalação com Docker</a></li>
    <li><a href="#instalacao-docker-compose">Instalação com Docker Compose</a></li>
    <li><a href="#configuração-do-flannel">Configuração do Flannel</a></li>
    <li><a href="#configuração-do-metallb">Configuração do MetalLB</a></li>
    <li><a href="#deploy-de-aplicações">Deploy de Aplicações</a></li>
    <li><a href="#monitoramento-e-acesso-ao-cluster">Monitoramento e Acesso ao Cluster</a></li>
    <li><a href="#recursos-adicionais">Recursos Adicionais</a></li>
    <li><a href="#contribuições">Contribuições</a></li>
    <li><a href="#licença">Licença</a></li>
</ul>

<h2 id="pré-requisitos">Pré-requisitos</h2>
<ul>
    <li>1 Raspberry Pi 3 ou superior com pelo menos 2GB de RAM.</li>
    <li>Cartões microSD com pelo menos 16GB.</li>
    <li>Sistema operacional: Raspberry Pi OS Lite ou Ubuntu Server.</li>
    <li>Docker e Docker-Compose configurado.</li>
    <li>Rede interna com DHCP configurado.</li>
</ul>

<h2 id="instalacao-com=docker">Instalação com Docker</h2>
<ol>
    Para iniciar o Dashy com o Docker, execute o seguinte comando no terminal:<br>
    <br>
    <pre><code><br>
        docker run -d \
        -p 4000:80 \
        --name dashy \
        -v /local/path/to/config.yml:/app/public/conf.yml \
        lissy93/dashy:latest
    </pre></code><br>
    <p>Esse comando faz o seguinte:</p>
    <ul>
      <li><strong>-d</strong>: Executa o container em segundo plano (detached).</li>
      <li><strong>-p 4000:80</strong>: Mapeia a porta 4000 do host para a porta 80 do container, tornando o Dashy acessível em <code>http://localhost:4000</code>.</li>
      <li><strong>-v /local/path/to/config.yml:/app/public/conf.yml</strong>: Monta o arquivo de configuração local <code>config.yml</code> no container.</li>
      <li><strong>lissy93/dashy:latest</strong>: A imagem oficial do Dashy.</li>
    </ul>
    <br>
</ol>
<br>

<h2 id="instalacao-docker-compose">Instalação com Docker Compose</h2>
<ol>
    <p>Para gerenciar o Dashy usando Docker Compose, crie um arquivo chamado <code>docker-compose.yml</code> com o seguinte conteúdo:</p>
    <pre><code>
        version: '3'
        services:
          dashy:
            image: lissy93/dashy:latest
            container_name: dashy
            ports:
              - "4000:80"
            volumes:
              - ./config.yml:/app/public/conf.yml
            restart: unless-stopped
    </code></pre>
    <p>Salve o arquivo e execute o seguinte comando para iniciar o Dashy:</p>
    <pre><code>docker-compose up -d</code></pre>    
    <p>Esse comando faz o seguinte:</p>
    <ul>
      <li><strong>up -d</strong>: Inicia os serviços definidos no arquivo <code>docker-compose.yml</code> em segundo plano (detached).</li>
      <li>O Dashy estará disponível no seu navegador na URL http://<strong>IP-do-Raspberry</strong>:8080.</li>
    </ul>
</ol>
<h2 id="configuração-do-flannel">Configuração do Flannel</h2>
<p>O Flannel é configurado automaticamente pelo k3s. Verifique se a rede está funcional com:</p>
<pre><code>kubectl get pods -A
</code></pre>

<h2 id="configuração-do-metallb">Configuração do MetalLB</h2>
<ol>
    <li>Instale o MetalLB:</li>
</ol>
<pre><code>sudo kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.13.9/config/manifests/metallb-native.yaml
</code></pre>
<ol start="2">
    <li>Configure o MetalLB com um intervalo de IPs da sua rede local:</li>
</ol>
<pre><code>apiVersion: metallb.io/v1beta1
kind: IPAddressPool
metadata:
  namespace: metallb-system
  name: ip-pool
spc:
  address:
    - 192.168.1.100-192.168.1.110
</code></pre>
<ol start="3">
    <li>Aplique a configuração:</li>
</ol>
<pre><code>kubectl apply -f poll-metallb.yaml
</code></pre>
<ol start="4">
    <li>Configurando Advanced L2 Configuration:</li>
</ol>
<pre><code>apiVersion: metallb.io/v1beta1
kind: L2Advertisement
metadata:
  name: homelab-l2
  namespace: metallb-system
spec:
  ipAddressPools:
    - ip-pool
</code></pre>
<ol start="5">
    <li>Aplique a configuração Advanced L2 Configuration:</li>
</ol>
<pre><code>sudo kubectl apply -f L2Advertisement.yaml
</code></pre>

<h2 id="deploy-de-aplicações">Configurando Nginx para teste</h2>
<p>Exemplo de deploy de uma aplicação simples:</p>
<pre><code>sudo kubectl create deploy nginx –image nginx:latest</code></pre>
<p>Expondo o Nginx para acessar pelo browser:</p>
<pre><code>sudo kubectl expose deploy nginx –port 80 --type LoadBalancer</code></pre>

<p>Agora podemos acessar pelo endereço http://192.168.0.101:80 no seu browser</p>



<h2 id="monitoramento-e-acesso-ao-cluster">Monitoramento e Acesso ao Cluster</h2>
<ul>
    <li>Acesse suas aplicações pelo IP atribuído pelo MetalLB.</li>
    <li>Use <code>kubectl</code> para monitorar o estado do cluster e dos pods.</li>
</ul>

<h2 id="recursos-adicionais">Recursos Adicionais</h2>
<ul>
    <li><a href="https://k3s.io/">Documentação oficial do k3s</a></li>
    <li><a href="https://github.com/flannel-io/flannel">Flannel</a></li>
    <li><a href="https://metallb.universe.tf/">MetalLB</a></li>
</ul>

<h2 id="contribuições">Contribuições</h2>
<p>Contribuições são bem-vindas! Sinta-se à vontade para abrir issues e pull requests.</p>

<h2 id="licença">Licença</h2>
<p>Este projeto está licenciado sob a licença MIT. Veja o arquivo <a href="LICENSE">LICENSE</a> para mais detalhes.</p>
