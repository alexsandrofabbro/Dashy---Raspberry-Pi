Português


# Dashy---Raspberry-Pi
Instalação e Configuração Dashy Board em Raspberry Pi

Tudo bom pessoal, sou apaixonado por tecnología e informatica. Por isso sempre estou estudando e pesquisando o que posso usar no meu dia a dia. Seja como usuário comum ou como devops.

Como eu sou um usuário curioso, sempre estou achando coisas interessante para me ajudar a monitorar os aplicativos a minha rede em casa.

Eu preciso de um aplicativo que me fácilite o acesso a algumas aplicações expecificas que utilizo e esse Dashy Board vai me ajudar com isso. Assim espero :) :) :)
<br>

Eu lhe apresento o Dashy Board que é um aplicativo open source, altamente personalizável, fácil de usar e com privacidade. Ele possiu vários temas integrados, também podemos realizar alteraçãos com a ajuda do CSS.<br>
<br>
<br>
******* Observação *******
<br>
<br>
Eu tive um problema com o Ubuntu server 24.04 LTS <br>
<br>
Ele esta dando essa mensagem:<br>

File "/home/admindev/.local/lib/python3.10/site-packages/urllib3/connectionpool.py", line 496, in _make_request
    conn.request(
TypeError: HTTPConnection.request() got an unexpected keyword argument 'chunked'<br>
<br>

Eu consegui resolver esse problema com os comandos abaixo:<br>

sudo apt update; sudo apt upgrade;
sudo apt install docker.io docker-compose-v2
sudo usermod -aG docker ${USER}

<br>
<br>
                                    


Nesse repositorio estou disponiblizando docker-compose e algumas dicas para quem deseja saber mais sobre Dashy.<br>
<br>
<br>
<br>

Vou deixar alguns links importantes sobre o Dashy que encontrei durante as minhas pesquisas:<br>
Site Dashy<br>
https://dashy.to/

Docker Dashy<br>
https://hub.docker.com/r/lissy93/dashy<br>
https://codeberg.org/alicia/dashy/src/branch/snyk-fix-d436d6927115f144ca04f98d9b6acaee/docker-compose.yml

Sites sobre Dashy<br>
https://wiki.opensourceisawesome.com/books/self-hosted-dashboards/page/dashy-powerful-informative-configurable-self-hosting-dashboard

Dashy on Raspberry Pi<br>
https://www.addictedtotech.net/install-dashy-dashboard-using-portainer-and-docker-on-a-raspberry-pi-4-episode-30/

Videos no Youtube<br>
https://www.youtube.com/watch?v=QsQUzutGarA&t=115s<br>
https://www.youtube.com/watch?v=zwPJm1Al3a8&t=108s<br>
<br>


Inglês
<br>
<br>
# Dashy---Raspberry-Pi 
<br>
Installation and Configuration Dashy Board on Raspberry Pi
<br>
<br>
All good guys, I'm passionate about technology and computers. That's why I'm always studying and researching what I can use in my daily life. Whether as a regular user or as a devops user.

As I'm a curious user, I'm always finding interesting things to help me monitor my network and computers at home.

I need an application that makes it easier for me to access some specific applications that I use and this Dashy Board will help me.

I present to you Dashy Board, which is an open source, highly customizable, easy to use and privacy-friendly application. It has several integrated themes, we can also make changes with the help of CSS.<br>
<br>

In this repository I am providing docker-compose and some tips for those who want to know more about Dashy.<br>
<br>
<br>
<br>

I'll leave some important links about Dashy that I found during my research: <br>

Site Dashy <br>
https://dashy.to/

Docker Dashy <br>
https://hub.docker.com/r/lissy93/dashy <br>
https://codeberg.org/alicia/dashy/src/branch/snyk-fix-d436d6927115f144ca04f98d9b6acaee/docker-compose.yml

Website about Dashy <br>
https://wiki.opensourceisawesome.com/books/self-hosted-dashboards/page/dashy-powerful-informative-configurable-self-hosting-dashboard

Dashy on Raspberry Pi <br>
https://www.addictedtotech.net/install-dashy-dashboard-using-portainer-and-docker-on-a-raspberry-pi-4-episode-30/

Youtube channels <br>
https://www.youtube.com/watch?v=QsQUzutGarA&t=115s<br>
https://www.youtube.com/watch?v=zwPJm1Al3a8&t=108s<br>
<br>

<p>***************************************************************************************************************************************************************************************************</p>


<h1>Instalação e Configuração do Dashy.</h1>

<p>Este repositório documenta a Configuração do Dashy. O objetivo é fornecer um guia passo a passo para configurar Dashy funcional e acessível a partir de outros dispositivos na mesma rede.</p>

<p align="center">
  <img src="https://github.com/Lissy93/dashy/blob/master/public/web-icons/dashy-logo.png" alt="Dashy Logo" width="150">
</p>

<p align="center">
  <strong>Dashy</strong> é um dashboard altamente personalizável, que permite organizar e gerenciar links, serviços e aplicações a partir de uma interface simples e intuitiva.
</p>

<h2>Índice</h2>
<ul>
    <li><a href="#pré-requisitos">Pré-requisitos</a></li>
    <li><a href="#arquitetura-do-cluster">Arquitetura do Cluster</a></li>
    <li><a href="#configuração-do-ambiente">Configuração do Ambiente</a></li>
    <li><a href="#instalação-do-k3s">Instalação do k3s</a></li>
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
    <li>3 Raspberry Pi 4 com pelo menos 2GB de RAM.</li>
    <li>Cartões microSD com pelo menos 16GB.</li>
    <li>Sistema operacional: Raspberry Pi OS Lite ou Ubuntu Server.</li>
    <li>Rede interna com DHCP configurado.</li>
</ul>

<h2 id="arquitetura-do-cluster">Arquitetura do Cluster</h2>
<ul>
    <li><strong>Master Node:</strong> 1 Raspberry Pi (com k3s server).</li>
    <li><strong>Worker Nodes:</strong> 2 Raspberry Pi (com k3s agent).</li>
    <li><strong>Rede:</strong> Flannel como CNI (Container Network Interface).</li>
    <li><strong>LoadBalancer:</strong> MetalLB configurado em modo Layer 2.</li>
</ul>

<h2 id="configuração-do-ambiente">Configuração do Ambiente</h2>
<ol>
    Após instalação do Ubuntu Server no cartão de memória, vocês podem estar configurando o IP fixo do Raspberry Pi no
    arquivo "network-config" como exemplo abaixo:<br>
    <br>
    <pre><code>network:<br>
      version: 2<br>
    
      ethernets:<br>
        eth0:<br>
           addresses: [192.168.0.201/24]<br>
           gateway4: 192.168.0.1<br>
           dhcp4: false<br>
           optional: true<br>
           nameservers:<br>
               addresses: [192.168.0.1, 0.0.0.0]</pre></code><br>
    <br>
    Para conectar a esse Raspberry Pi pela rede vou utilizar a conexão por <b>ssh</b>.<br>
    <br>
    <b>OBS:</b> Lembrando de conferir se a <b>porta 22</b> e a conexão por <b>autenticação</b> esteja liberada.
    Vocês podem está verificando isso em <b>/etc/ssh</b> no arquivo <b>ssh_config</b>
    <br>
    <br>
    <br>
    Após essa configuração, podem inserir o cartão de memória no Raspberry Pi e conectar pelo terminal utilizando o comando <b>ssh</b>.<br>
    <br>
    <br>
    <h2>Preparação dos Raspberry Pis.</h2>
    <b>Obs:</b> Esses passos têm que ser realizados em todos os Raspberry Pi que irão fazer parte do seu Cluster.<br>
    <br>
    Iremos realizar os comandos pelo terminal e com o usuário <b>Root.</b><br> 
    Digite o seguinte comando para ir como <b>Root</b> e depois a senha do root.<br>
    <br>
    <pre><code>sudo su</pre></code>
    <br>
    Configurando os hostname dos clusters com o seguinte comando:<br>
    <br>
    <pre><code>hostname cluster01</pre></code>
    <br>
    Para salvar o nome no arquivo host teremos que executar o comando abaixo:<br>
    <br>
    <pre><code><b>echo "cluster01" > /etc/hostname</b></pre></code><br>
    <br>
    Se você quiser confirmar se foi alterado o nome, execute esse comando:<br>
    <br>
    <pre><code><b>cat /etc/hostname</b></pre></code><br>
    <br>
    Adicionar o IP do Raspberry Pi no arquivo hosts, para isso vamos usar o seguinte comando com o nano como o exemplo abaixo:<br>
    <br>
    <pre><code><b>nano /etc/hosts</b></pre></code><br>
    <pre><code>127.0.0.1         localhost<br>
    <b>192.168.0.201  cluster01</b><br>  
    ::1 localhost<br>
    127.0.1.1         pop-os.localdomain pop-os</pre></code><br>
    <b>Obs.</b> Se for configurar o Kubernetes em desktops, temos que desabilitar o Swap.<br>
    <br>
    <br>
    Como no Raspberry Pi não vem ativado. não iremos executar esse comando:<br>
    <br>
    <pre><code><b>swapoff -a</b></pre></code><br>
    <br>
    Habilitando a memória que por padrão ela vem desabilitada:<br>
    <br>
    <pre><code><b>nano /boot/firmware/cmdline.txt</b></pre></code><br>
    <br>
    No final da linha você irá adicionar o seguinte:<br>
    <br>
    <pre><code><b>cgroup_enable=cpuset, cgroup_enable=memory, cgroup_memory=1, swapaccount=1</b></pre></code><br>
    <br>
    Também temos que instalar o Java e o Docker<br>
    <br>
    Vamos criar um arquivo .json<br>
    Para isso, vamos utilizar o seguinte comando:<br>
    <br>
    <pre><code><b>nano /etc/docker/daemon.json</b></pre></code><br>
    <br>
    Ele é um arquivo novo e vamos digitar os seguintes comandos:<br>
    <br>
    <pre><code>
    {<br>
       "exec-opts": ["native.cgroupdriver=systemd"],<br>
       "log-driver": "json-file",<br>
       "log-opts": {<br>
             "max-size": "100m"<br>
         },<br>
       "storage-driver": "overlay2"<br>
    }<br>
    </pre></code>
    <br>
    Não podemos esquecer de habilitar uma opção no arquivo <b>sysctl.conf</b>.<br>
    <br>
    Para isso vamos executar o seguinte comando:<br>
    <pre><code><b>nano /etc/sysctl.conf</b></pre></code><br>
    <br>
    tem que localizar a linha que está escrito <b>#net.ipv4.ip_forward=1</b> e remover o <b>#</b> 
    <br>
</ol>
<br>

<h2 id="instalação-do-k3s">Instalação do k3s</h2>
<ol>
    <li>Instale o k3s no nó master com o Flannel:</li>
</ol>
<pre><code>curl -sfL https://get.k3s.io | sh -s - --flannel-iface=eth0
</code></pre>
<ol start="2">
    <li>Obtenha o token do master para conectar os nós workers:</li>
</ol>
<pre><code>cat /var/lib/rancher/k3s/server/node-token
</code></pre>
<ol start="3">
    <li>Conecte os workers ao cluster:</li>
</ol>
<pre><code>curl -sfL https://get.k3s.io | K3S_URL=https://&lt;master-node-ip&gt;:6443 K3S_TOKEN=&lt;token&gt; sh -
</code></pre>

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
